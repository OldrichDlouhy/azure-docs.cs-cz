---
title: Jak zabezpečit webovou aplikaci pomocí interaktivního jednotného přihlašování
titleSuffix: Azure Maps
description: Jak nakonfigurovat webovou aplikaci, která podporuje jednotné přihlašování Azure AD s Azure Maps Web SDK pomocí protokolu OpenID Connect.
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/12/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: devx-track-javascript
ms.openlocfilehash: b86a8f726c039e3fa909cdc6f3f7b33b7c9c01ff
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87279742"
---
# <a name="secure-a-web-application-with-user-sign-in"></a>Zabezpečení webové aplikace pomocí přihlášení uživatele

Následující příručka se vztahuje na aplikaci, která je hostována na webových serverech, spravuje více obchodních scénářů a nasazuje na webové servery. Aplikace má požadavky na poskytování chráněných prostředků pouze uživatelům Azure AD. Cílem scénáře je povolit webové aplikaci ověřování ve službě Azure AD a volat Azure Maps rozhraní REST API jménem uživatele.

[!INCLUDE [authentication details](./includes/view-authentication-details.md)]

## <a name="create-an-application-registration-in-azure-ad"></a>Vytvoření registrace aplikace v Azure AD

Aby se uživatelé mohli přihlásit, musíte ve službě Azure AD vytvořit webovou aplikaci. Tato webová aplikace pak bude delegovat přístup uživatelů k Azure Maps rozhraní REST API.

1. V Azure Portal v seznamu služeb Azure vyberte **Azure Active Directory**  >  **Registrace aplikací**  >  **Nová registrace**.  

    > [!div class="mx-imgBorder"]
    > ![Registrace aplikace](./media/how-to-manage-authentication/app-registration.png)

2. Zadejte **název**, vyberte **typ účtu podpory**, zadejte identifikátor URI pro přesměrování, který bude představovat adresu URL, kterou služba Azure AD vydá token, a je adresa URL, kde je ovládací prvek mapy hostovaný. Další podrobnosti najdete v tématu scénář Azure AD [: webová aplikace, která](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-overview)přihlašuje uživatele. Dokončete uvedené kroky ze scénáře služby Azure AD.  

3. Po dokončení registrace aplikace potvrďte, že přihlašování aplikace funguje pro uživatele. Po přihlášení můžete aplikaci udělit delegovaný přístup k Azure Maps rozhraní REST API.
    
4.  Pokud chcete přiřadit Azure Maps oprávnění k delegovanému rozhraní API, Projděte si aplikaci. Pak vyberte **oprávnění API**  >  **Přidat oprávnění**. V části **rozhraní API moje organizace používá**, vyhledejte a vyberte **Azure Maps**.

    > [!div class="mx-imgBorder"]
    > ![Přidat oprávnění rozhraní API pro aplikace](./media/how-to-manage-authentication/app-permissions.png)

5. Zaškrtněte políčko vedle pole přístup k **Azure Maps**a pak vyberte **Přidat oprávnění**.

    > [!div class="mx-imgBorder"]
    > ![Výběr oprávnění rozhraní API pro aplikace](./media/how-to-manage-authentication/select-app-permissions.png)

6. Povolení, aby webová aplikace volala Azure Maps rozhraní REST API tím, že konfiguruje registraci aplikace pomocí tajného klíče aplikace, podrobný postup najdete v tématu [Webová aplikace, která volá webová rozhraní API: registrace aplikace](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-app-registration). Pro ověření ve službě Azure AD jménem uživatele je vyžadován tajný kód. Registrační certifikát aplikace nebo tajný klíč by měl být uložený v zabezpečeném úložišti, aby se webová aplikace mohla načíst pro ověření ve službě Azure AD. 
   
   * Pokud už aplikace má nakonfigurovanou registraci aplikace Azure AD a tajný klíč, může se tento krok přeskočit.

> [!Tip]
> Pokud je aplikace hostovaná v prostředí Azure, doporučujeme používat [spravované identity pro prostředky Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) a instanci Azure Key Vault pro přístup k tajným klíčům [získáním přístupového tokenu](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-token) pro přístup k Azure Key Vault tajným klíčům nebo certifikátům. Pokud se chcete připojit k Azure Key Vault načtení tajných kódů, Projděte si [kurz připojení prostřednictvím spravované identity](https://docs.microsoft.com/azure/key-vault/general/tutorial-net-create-vault-azure-web-app).
   
7. Implementujte koncový bod zabezpečeného tokenu pro Azure Maps Web SDK pro přístup k tokenu. 
   
   * Vzorový řadič tokenu najdete v tématu [Azure Maps ukázek Azure AD](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples/blob/master/src/OpenIdConnect/AzureMapsOpenIdConnectv1/AzureMapsOpenIdConnect/Controllers/TokenController.cs). 
   * V případě implementace bez AspNetCore nebo jiné najdete další informace v dokumentaci k [získání tokenu pro aplikaci](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token) z Azure AD.
   * Koncový bod zabezpečeného tokenu zodpovídá za vrácení přístupového tokenu pro ověřeného a autorizovaného uživatele pro volání rozhraní REST API Azure Maps.

8. Nakonfigurujte řízení přístupu na základě role Azure pro uživatele nebo skupiny. Viz [udělení přístupu na základě rolí pro uživatele](#grant-role-based-access-for-users-to-azure-maps).

9. Nakonfigurujte stránku webové aplikace pomocí Azure Maps Web SDK pro přístup ke koncovému bodu zabezpečeného tokenu. 

```javascript
var map = new atlas.Map("map", {
        center: [-122.33, 47.64],
        zoom: 12,
        language: "en-US",
        authOptions: {
            authType: "anonymous",
            clientId: "<insert>",  // azure map account client id
            getToken: function (resolve, reject, map) {
                var xhttp = new XMLHttpRequest();
                xhttp.open("GET", "/api/token", true); // the url path maps to the token endpoint.
                xhttp.onreadystatechange = function () {
                    if (this.readyState === 4 && this.status === 200) {
                        resolve(this.responseText);
                    } else if (this.status !== 200) {
                        reject(this.responseText);
                    }
                };

                xhttp.send();
            }
        }
    });
    map.events.add("tokenacquired", function () {
        console.log("token acquired");
    });
    map.events.add("error", function (err) {
        console.log(JSON.stringify(err.error));
    });
```

[!INCLUDE [grant role access to users](./includes/grant-rbac-users.md)]

## <a name="next-steps"></a>Další kroky

Další porozumění scénáři webové aplikace:
> [!div class="nextstepaction"]
> [Scénář: webová aplikace, která se přihlásí uživatelům](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-overview)

Najděte metriky využití API pro váš účet Azure Maps:
> [!div class="nextstepaction"]
> [Zobrazení metrik využití](how-to-view-api-usage.md)

Prozkoumejte ukázky, které ukazují, jak integrovat Azure AD s Azure Maps:
> [!div class="nextstepaction"]
> [Ukázky Azure Maps webové aplikace Azure AD](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples/tree/master/src/OpenIdConnect)
