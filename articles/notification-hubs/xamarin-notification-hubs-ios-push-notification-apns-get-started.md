---
title: Posílání nabízených oznámení do Xamarin pomocí Azure Notification Hubs | Microsoft Docs
description: V tomto kurzu zjistíte, jak pomocí služby Azure Notification Hubs odesílat nabízená oznámení do aplikace Xamarin.iOS.
services: notification-hubs
keywords: nabízená oznámení ios,nabízení zpráv,nabízená oznámení,nabízená zpráva
documentationcenter: xamarin
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 07/07/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 05/23/2019
ms.openlocfilehash: 3a10b17b65c518b483a713701986765b8c979c79
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86529960"
---
# <a name="tutorial-send-push-notifications-to-xamarinios-apps-using-azure-notification-hubs"></a>Kurz: odesílání nabízených oznámení do aplikací pro Xamarin. iOS pomocí Azure Notification Hubs

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Přehled

V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace systému iOS. Vytvoříte prázdnou aplikaci Xamarin. iOS, která přijímá nabízená oznámení pomocí [služby APNs (Apple Push Notification Service)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html).

Jakmile budete hotovi, budete moct používat vaše centrum oznámení k všesměrovému vysílání nabízených oznámení pro všechna zařízení používající vaši aplikaci. Dokončený kód je k dispozici v ukázce [aplikace NotificationHubs][GitHub].

V tomto kurzu vytvoříte nebo aktualizujete kód tak, aby prováděl následující úlohy:

> [!div class="checklist"]
> * Vygenerujete soubor s žádostí o podepsání certifikátu
> * Registrace aplikace pro nabízená oznámení
> * Vytvoření zřizovacího profilu pro aplikaci
> * Nakonfigurujete v centru oznámení nabízená oznámení pro iOS
> * Odešlete nabízená oznámení

## <a name="prerequisites"></a>Předpoklady

* **Předplatné Azure**. Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.
* Poslední verze jazyka [Xcode][Install Xcode]
* Zařízení kompatibilní s iOS 10 (nebo novější verzí)
* Členství v [programu pro vývojáře Apple](https://developer.apple.com/programs/).
* [Visual Studio pro Mac]
  
  > [!NOTE]
  > Z důvodu požadavků na konfiguraci pro nabízená oznámení iOS musíte nasadit a otestovat vzorovou aplikaci na fyzickém zařízení iOS (iPhone nebo iPad) namísto simulátoru.

Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy služby Notification Hubs pro aplikace Xamarin.iOS.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="connect-your-app-to-the-notification-hub"></a>Připojte aplikaci k centru oznámení

### <a name="create-a-new-project"></a>Vytvoření nového projektu

1. V sadě Visual Studio vytvořte nový projekt pro iOS, vyberte šablonu **Aplikace s jedním zobrazením** a klikněte na **Další**.

     ![Visual Studio – Výběr typu aplikace][31]

2. Zadejte název aplikace a identifikátor organizace, klikněte na **Další**a pak na **vytvořit** .

3. V zobrazení Řešení dvakrát klikněte na soubor *Info.plist* a v části **Identita** se ujistěte, že identifikátor sady odpovídá identifikátoru použitému při vytváření profilu zřizování. V části **Podepisování** zkontrolujte, že v části **Tým** je vybraný váš vývojářský účet, možnost Automatically manage signing (Automaticky se starat o podepisování) je vybraná a váš podpisový certifikát a profil zřizování jsou automaticky vybrané.

    ![Visual Studio – Konfigurace aplikace pro iOS][32]

4. V zobrazení řešení poklikejte na `Entitlements.plist` a ujistěte se, že je zaškrtnuté políčko **Povolit nabízená oznámení** .

    ![Visual Studio – konfigurace oprávnění iOS][33]

5. Přidejte balíček zasílání zpráv Azure. V zobrazení řešení klikněte pravým tlačítkem myši na projekt a vyberte **Přidat**  >  **Přidat balíčky NuGet**. Vyhledejte balíček **Xamarin.Azure.NotificationHubs.iOS** a přidejte ho do svého projektu.

6. Přidejte do své třídy nový soubor, pojmenujte jej `Constants.cs` a přidejte následující proměnné a nahraďte zástupné symboly řetězcového literálu pomocí `hubname` a `DefaultListenSharedAccessSignature` dříve uvedeného.

    ```csharp
    // Azure app-specific connection string and hub path
    public const string ListenConnectionString = "<Azure DefaultListenSharedAccess Connection String>";
    public const string NotificationHubName = "<Azure Notification Hub Name>";
    ```

7. Do `AppDelegate.cs` přidejte následující příkaz using:

    ```csharp
    using WindowsAzure.Messaging;
    using UserNotifications
    ```

8. Deklarovat instanci `SBNotificationHub` :

    ```csharp
    private SBNotificationHub Hub { get; set; }
    ```

9. V nástroji `AppDelegate.cs` aktualizujte `FinishedLaunching()` tak, aby odpovídaly následujícímu kódu:

    ```csharp
    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        if (UIDevice.CurrentDevice.CheckSystemVersion(10, 0))
        {
            UNUserNotificationCenter.Current.RequestAuthorization(UNAuthorizationOptions.Alert | UNAuthorizationOptions.Badge | UNAuthorizationOptions.Sound,
                                                                    (granted, error) => InvokeOnMainThread(UIApplication.SharedApplication.RegisterForRemoteNotifications));
        }
        else if (UIDevice.CurrentDevice.CheckSystemVersion(8, 0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes(
                    UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                    new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();
        }
        else
        {
            UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes(notificationTypes);
        }

        return true;
    }
    ```

10. V `AppDelegate.cs` , přepište `RegisteredForRemoteNotifications()` metodu:

    ```csharp
    public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
    {
        Hub = new SBNotificationHub(Constants.ListenConnectionString, Constants.NotificationHubName);

        Hub.UnregisterAll (deviceToken, (error) => {
            if (error != null)
            {
                System.Diagnostics.Debug.WriteLine("Error calling Unregister: {0}", error.ToString());
                return;
            }

            NSSet tags = null; // create tags if you want
            Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                if (errorCallback != null)
                    System.Diagnostics.Debug.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
            });
        });
    }
    ```

11. V `AppDelegate.cs` , přepište `ReceivedRemoteNotification()` metodu:

    ```csharp
    public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
    {
        ProcessNotification(userInfo, false);
    }
    ```

12. V nástroji `AppDelegate.cs` vytvořte `ProcessNotification()` metodu:

    ```csharp
    void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
    {
        // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
        if (null != options && options.ContainsKey(new NSString("aps")))
        {
            //Get the aps dictionary
            NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;

            //Extract the alert text
            // NOTE: If you're using the simple alert by just specifying
            // "  aps:{alert:"alert msg here"}  ", this will work fine.
            // But if you're using a complex alert with Localization keys, etc.,
            // your "alert" object from the aps dictionary will be another NSDictionary.
            // Basically the JSON gets dumped right into a NSDictionary,
            // so keep that in mind.
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //If this came from the ReceivedRemoteNotification while the app was running,
            // we of course need to manually process things like the sound, badge, and alert.
            if (!fromFinishedLaunching)
            {
                //Manually show an alert
                if (!string.IsNullOrEmpty(alert))
                {
                    var myAlert = UIAlertController.Create("Notification", alert, UIAlertControllerStyle.Alert);
                    myAlert.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
                    UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(myAlert, true, null);
                }
            }
        }
    }
    ```

    > [!NOTE]
    > Můžete si vybrat, že se má potlačit `FailedToRegisterForRemoteNotifications()` zpracování situací, jako je například žádné síťové připojení. To je obzvláště důležité, když by uživatel mohl spustit aplikaci v režimu offline (například letadlo) a vy chcete zpracovávat scénáře zpráv oznámení specifické pro vaši aplikaci.

13. Spusťte aplikaci v zařízení.

## <a name="send-test-push-notifications"></a>Odešlete nabízená oznámení

Příjem oznámení ve vaší aplikaci můžete otestovat pomocí možnosti *Testovací odeslání* na webu [Azure Portal]. Do zařízení se odešle testovací nabízené oznámení.

![Portál Azure – testovací odeslání][30]

Nabízená oznámení se většinou posílají ve službě back-end, jako je služba Mobile Apps, nebo v technologii ASP.NET pomocí kompatibilní knihovny. Pokud pro váš back-end není dostupná žádná knihovna, můžete k zasílání zpráv oznámení použít také přímo rozhraní REST API.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste rozeslali oznámení do všech zařízení s iOS zaregistrovaných v back-endu. Pokud se chcete naučit zasílat nabízená oznámení určitým zařízením s iOSem, pokračujte následujícím kurzem:

> [!div class="nextstepaction"]
>[Nabízená oznámení odesílaná konkrétním zařízením](notification-hubs-ios-xplat-segmented-apns-push-notification.md)

<!-- Images. -->
[6]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png
[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png
[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png
[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png
[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png
[32]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-app-settings.png
[33]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-entitlements-settings.png

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: https://go.microsoft.com/fwlink/p/?LinkId=272456
[Visual Studio pro Mac]: https://visualstudio.microsoft.com/vs/mac/
[Local and Push Notification Programming Guide]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#//apple_ref/doc/uid/TP40008194-CH6-SW1
[Apple Push Notification Service]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html
[Apple Push Notification Service fwlink]: https://go.microsoft.com/fwlink/p/?LinkId=272584
[GitHub]: https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs
[Azure Portal]: https://portal.azure.com
