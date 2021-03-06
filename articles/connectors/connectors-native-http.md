---
title: Volání koncových bodů služby pomocí protokolu HTTP nebo HTTPS
description: Odesílat odchozí požadavky HTTP nebo HTTPS do koncových bodů služby z Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: conceptual
ms.date: 06/09/2020
tags: connectors
ms.openlocfilehash: 8c7a0ddb80ba28548fc1821cc2063e500af0fa66
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87286627"
---
# <a name="call-service-endpoints-over-http-or-https-from-azure-logic-apps"></a>Volání koncových bodů služby přes HTTP nebo HTTPS z Azure Logic Apps

Pomocí [Azure Logic Apps](../logic-apps/logic-apps-overview.md) a integrované triggeru nebo akce http můžete vytvářet automatizované úlohy a pracovní postupy, které odesílají požadavky do koncových bodů služby přes protokol HTTP nebo HTTPS. Například můžete monitorovat koncový bod služby pro svůj web tak, že zkontrolujete jeho koncový bod podle konkrétního plánu. Když v tomto koncovém bodu dojde k zadané události, jako je například váš web vypíná, událost aktivuje pracovní postup vaší aplikace logiky a spustí akce v tomto pracovním postupu. Pokud místo toho chcete přijmout příchozí volání HTTPS a reagovat na ně, použijte integrovaný [Trigger žádosti nebo akci reakce](../connectors/connectors-native-reqres.md).

* Pokud chcete zjistit nebo zadat *dotaz* na koncový bod podle opakovaného plánu, [přidejte Trigger http](#http-trigger) jako první krok pracovního postupu. Pokaždé, když Trigger zkontroluje koncový bod, Trigger zavolá nebo odešle *požadavek* na koncový bod. Odpověď koncového bodu Určuje, jestli je Workflow vaší aplikace logiky spuštěn. Aktivační událost předá do akcí ve vaší aplikaci logiky veškerý obsah z odpovědi koncového bodu.

* Pokud chcete zavolat koncový bod odkudkoli jinde ve vašem pracovním postupu, [přidejte akci HTTP](#http-action). Odpověď koncového bodu Určuje, jak se spustí zbývající akce pracovního postupu.

Tento článek ukazuje, jak přidat Trigger nebo akci HTTP do pracovního postupu aplikace logiky.

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure. Pokud nemáte předplatné Azure, [zaregistrujte si bezplatný účet Azure](https://azure.microsoft.com/free/).

* Adresa URL cílového koncového bodu, který chcete volat

* Základní znalosti o [tom, jak vytvářet aplikace logiky](../logic-apps/quickstart-create-first-logic-app-workflow.md) Pokud s Logic Apps začínáte, přečtěte si téma [co je Azure Logic Apps](../logic-apps/logic-apps-overview.md)?

* Aplikace logiky, ze které chcete zavolat cílový koncový bod. Pokud chcete začít s triggerem HTTP, [vytvořte prázdnou aplikaci logiky](../logic-apps/quickstart-create-first-logic-app-workflow.md). Pokud chcete použít akci HTTP, spusťte aplikaci logiky s libovolným triggerem, který chcete. V tomto příkladu se jako první krok používá Trigger HTTP.

<a name="http-trigger"></a>

## <a name="add-an-http-trigger"></a>Přidání triggeru HTTP

Tato integrovaná aktivační událost provede volání HTTP na zadanou adresu URL pro koncový bod a vrátí odpověď.

1. Přihlaste se na [Azure Portal](https://portal.azure.com). Otevřete prázdnou aplikaci logiky v návrháři aplikace logiky.

1. V poli hledání návrháře vyberte možnost **předdefinované**. Do vyhledávacího pole zadejte `http` jako filtr. V seznamu **triggery** vyberte Trigger **http** .

   ![Vyberte Aktivační událost HTTP.](./media/connectors-native-http/select-http-trigger.png)

   V tomto příkladu se aktivační událost přejmenuje na Trigger HTTP, aby měl krok výstižnější název. Dále tento příklad přidá akci HTTP a oba názvy musí být jedinečné.

1. Zadejte hodnoty pro [parametry triggeru protokolu HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md#http-trigger) , které chcete zahrnout do volání cílového koncového bodu. Nastavte opakování pro to, jak často chcete, aby aktivační událost kontrolovala cílový koncový bod.

   ![Zadejte parametry triggeru protokolu HTTP.](./media/connectors-native-http/http-trigger-parameters.png)

   Pokud vyberete jiný typ ověřování než **žádné**, nastavení ověřování se liší v závislosti na vašem výběru. Další informace o typech ověřování dostupných pro protokol HTTP najdete v těchto tématech:

   * [Přidání ověřování do odchozích volání](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
   * [Ověření přístupu k prostředkům pomocí spravovaných identit](../logic-apps/create-managed-service-identity.md)

1. Chcete-li přidat další dostupné parametry, otevřete seznam **Přidat nový parametr** a vyberte požadované parametry.

1. Pokračujte v vytváření pracovního postupu aplikace logiky s akcemi, které se spustí, když se Trigger aktivuje.

1. Až budete hotovi, nezapomeňte uložit aplikaci logiky. Na panelu nástrojů návrháře vyberte **Uložit**.

<a name="http-action"></a>

## <a name="add-an-http-action"></a>Přidat akci HTTP

Tato Vestavěná akce provede volání HTTP na zadanou adresu URL pro koncový bod a vrátí odpověď.

1. Přihlaste se na [Azure Portal](https://portal.azure.com). Otevřete aplikaci logiky v návrháři aplikace logiky.

   V tomto příkladu se jako první krok používá Trigger HTTP.

1. V kroku, kam chcete přidat akci HTTP, vyberte **Nový krok**.

   Chcete-li přidat akci mezi kroky, přesuňte ukazatel myši na šipku mezi jednotlivými kroky. Vyberte symbol plus ( **+** ), který se zobrazí, a pak vyberte **přidat akci**.

1. V části **zvolit akci**vyberte **předdefinovaná**. Do vyhledávacího pole zadejte `http` jako filtr. V seznamu **Akce** vyberte akci **http** .

   ![Vybrat akci HTTP](./media/connectors-native-http/select-http-action.png)

   Tento příklad přejmenuje akci na "HTTP Action", takže krok má výstižnější název.

1. Zadejte hodnoty [parametrů akce protokolu HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action) , které chcete zahrnout do cílového koncového bodu.

   ![Zadejte parametry akce HTTP.](./media/connectors-native-http/http-action-parameters.png)

   Pokud vyberete jiný typ ověřování než **žádné**, nastavení ověřování se liší v závislosti na vašem výběru. Další informace o typech ověřování dostupných pro protokol HTTP najdete v těchto tématech:

   * [Přidání ověřování do odchozích volání](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
   * [Ověření přístupu k prostředkům pomocí spravovaných identit](../logic-apps/create-managed-service-identity.md)

1. Chcete-li přidat další dostupné parametry, otevřete seznam **Přidat nový parametr** a vyberte požadované parametry.

1. Až budete hotovi, nezapomeňte uložit aplikaci logiky. Na panelu nástrojů návrháře vyberte **Uložit**.

<a name="tls-support"></a>

## <a name="transport-layer-security-tls"></a>Protokol TLS (Transport Layer Security)

Odchozí volání na základě schopnosti cílového koncového bodu podporují protokol TLS (Transport Layer Security), který byl dříve SSL (Secure Sockets Layer) (SSL), verze 1,0, 1,1 a 1,2. Logic Apps vyjednávat s koncovým bodem pomocí nejvyšší možné podporované verze.

Například pokud koncový bod podporuje 1,2, konektor HTTP nejprve používá 1,2. V opačném případě konektor používá další nejvyšší podporovanou verzi.

<a name="self-signed"></a>

## <a name="self-signed-certificates"></a>Certifikáty podepsané svým držitelem

* Pro Logic Apps v globálním prostředí Azure s více klienty nepovoluje konektor HTTP konektor protokolu TLS/SSL podepsaný svým držitelem. Pokud vaše aplikace logiky provede volání HTTP na server a uvede certifikát podepsaný svým držitelem (TLS/SSL), volání HTTP se nezdařila s `TrustFailure` chybou.

* Pro Logic Apps v [prostředí ISE (Integration Service Environment)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)umožňuje konektor http certifikáty podepsané svým držitelem pro ověřování TLS/SSL. Je však třeba nejprve [Povolit podporu certifikátu podepsaného svým držitelem](../logic-apps/create-integration-service-environment-rest-api.md#request-body) pro stávající ISE nebo nový ISE pomocí REST API Logic Apps a nainstalovat veřejný certifikát do `TrustedRoot` umístění.

## <a name="content-with-multipartform-data-type"></a>Obsah s datovým typem multipart/form-

Chcete-li zpracovat obsah, který je `multipart/form-data` typu v požadavcích http, můžete přidat objekt JSON, který `$content-type` obsahuje `$multipart` atributy a, do těla požadavku HTTP pomocí tohoto formátu.

```json
"body": {
   "$content-type": "multipart/form-data",
   "$multipart": [
      {
         "body": "<output-from-trigger-or-previous-action>",
         "headers": {
            "Content-Disposition": "form-data; name=file; filename=<file-name>"
         }
      }
   ]
}
```

Předpokládejme například, že máte aplikaci logiky, která odesílá požadavek HTTP POST pro excelový soubor na web pomocí rozhraní API tohoto webu, které podporuje daný `multipart/form-data` typ. Tady je postup, jak by tato akce mohla vypadat:

![Data formuláře v částech](./media/connectors-native-http/http-action-multipart.png)

Tady je stejný příklad, který ukazuje definici JSON akce HTTP v základní definici pracovního postupu:

```json
"HTTP_action": {
   "inputs": {
      "body": {
         "$content-type": "multipart/form-data",
         "$multipart": [
            {
               "body": "@trigger()",
               "headers": {
                  "Content-Disposition": "form-data; name=file; filename=myExcelFile.xlsx"
               }
            }
         ]
      },
      "method": "POST",
      "uri": "https://finance.contoso.com"
   },
   "runAfter": {},
   "type": "Http"
}
```

<a name="asynchronous-pattern"></a>

## <a name="asynchronous-request-response-behavior"></a>Asynchronní chování požadavků a odpovědí

Ve výchozím nastavení se všechny akce založené na protokolu HTTP v Azure Logic Apps řídí standardním [vzorem asynchronní operace](/azure/architecture/patterns/async-request-reply). Tento vzorec určuje, že po volání akce HTTP nebo odeslání požadavku koncovému bodu, službě, systému nebo rozhraní API příjemce okamžitě vrátí odpověď ["202 přijatý"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.3) . Tento kód potvrdí, že příjemce požadavek přijal, ale nedokončil zpracování. Odpověď může obsahovat `location` záhlaví, které určuje adresu URL a ID aktualizace, které volající může použít k cyklickému dotazování nebo kontrolu stavu asynchronního požadavku, dokud příjemce neukončí zpracování a vrátí odpověď na úspěch ["200 OK"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.1) nebo jinou odpověď mimo 202. Volající ale nemusí čekat na dokončení zpracování žádosti a může pokračovat v běhu další akce. Další informace najdete v tématu [asynchronní integrace mikroslužeb vynutila autonomii](/azure/architecture/microservices/design/interservice-communication#synchronous-versus-asynchronous-messaging)mikroslužeb.

* V návrháři aplikace logiky, akce HTTP, ale ne Trigger, má nastavení **asynchronního vzoru** , které je ve výchozím nastavení povolené. Toto nastavení určuje, že volající nečeká na dokončení zpracování, a může přejít k další akci, ale pokračuje v kontrole stavu, dokud se zpracování nezastaví. Pokud je toto nastavení zakázáno, určuje, že volající čeká na dokončení zpracování, než přejde k další akci.

  Toto nastavení můžete najít pomocí těchto kroků:

  1. V záhlaví akce HTTP vyberte tlačítko se třemi tečkami (**...**), které otevře nastavení akce.

  1. Najde nastavení **asynchronního vzoru** .

     ![Nastavení asynchronního vzoru](./media/connectors-native-http/asynchronous-pattern-setting.png)

* Definice základní JavaScript Object Notation akce HTTP implicitně sleduje vzorek asynchronní operace.

<a name="disable-asynchronous-operations"></a>

## <a name="disable-asynchronous-operations"></a>Zakázat asynchronní operace

V některých scénářích může být vhodné použít asynchronní chování akce HTTP například v případě, že chcete:

* [Vyhněte se časovým limitům protokolu HTTP pro dlouhotrvající úlohy](#avoid-http-timeouts)
* [Zakázat kontrolu hlaviček umístění](#disable-location-header-check)

<a name="turn-off-asynchronous-pattern-setting"></a>

### <a name="turn-off-asynchronous-pattern-setting"></a>Vypnout nastavení **asynchronního vzoru**

1. V návrháři aplikace logiky v záhlaví akce HTTP vyberte tlačítko se třemi tečkami (**...**), které otevře nastavení akce.

1. Vyhledejte nastavení **asynchronního vzoru** , v případě povolení vypněte nastavení **vypnuto** a vyberte možnost **Hotovo**.

   ![Zakázat nastavení "asynchronního" vzoru](./media/connectors-native-http/disable-asynchronous-pattern-setting.png)

<a name="add-disable-async-pattern-option"></a>

### <a name="disable-asynchronous-pattern-in-actions-json-definition"></a>Zakázat asynchronní vzor v definici JSON akce

V podkladové definici JSON akce HTTP [přidejte `"DisableAsyncPattern"` možnost operace](../logic-apps/logic-apps-workflow-actions-triggers.md#operation-options) do definice akce tak, aby akce prochází vzorem synchronní operace. Další informace najdete v tématu [spuštění akcí ve vzoru synchronní operace](../logic-apps/logic-apps-workflow-actions-triggers.md#disable-asynchronous-pattern).

<a name="avoid-http-timeouts"></a>

## <a name="avoid-http-timeouts-for-long-running-tasks"></a>Vyhněte se časovým limitům protokolu HTTP pro dlouhotrvající úlohy

Požadavky HTTP mají [časový](../logic-apps/logic-apps-limits-and-config.md#http-limits)limit. Máte-li dlouhou spuštěnou akci protokolu HTTP, která vyprší od tohoto limitu, máte tyto možnosti:

* [Zakažte model asynchronní operace akce http](#disable-asynchronous-operations) , aby se akce nepřetržitě nedotazoval ani nezkontrolovala stav žádosti. Místo toho akce počká, až příjemce odpoví na stav a výsledky po dokončení zpracování žádosti.

* Nahraďte akci HTTP [akcí Webhooku http](../connectors/connectors-native-webhook.md), která čeká, až příjemce odpoví na stav a výsledky po dokončení zpracování žádosti.

<a name="disable-location-header-check"></a>

## <a name="disable-checking-location-headers"></a>Zakázat kontrolu hlaviček umístění

Některé koncové body, služby, systémy nebo rozhraní API vrátí odpověď "202 přijato", která nemá `location` hlavičku. Chcete-li zabránit tomu, aby akce HTTP průběžně kontrolovala stav žádosti `location` , když hlavička neexistuje, můžete mít tyto možnosti:

* [Zakažte model asynchronní operace akce http](#disable-asynchronous-operations) , aby se akce nepřetržitě nedotazoval ani nezkontrolovala stav žádosti. Místo toho akce počká, až příjemce odpoví na stav a výsledky po dokončení zpracování žádosti.

* Nahraďte akci HTTP [akcí Webhooku http](../connectors/connectors-native-webhook.md), která čeká, až příjemce odpoví na stav a výsledky po dokončení zpracování žádosti.

## <a name="known-issues"></a>Známé problémy

<a name="omitted-headers"></a>

### <a name="omitted-http-headers"></a>Vynechané hlavičky HTTP

Pokud aktivační událost nebo akce HTTP obsahují tyto hlavičky, Logic Apps odebere tato záhlaví z vygenerované zprávy požadavku bez zobrazení upozornění nebo chyby:

* `Accept-*`
* `Allow`
* `Content-*`s těmito výjimkami: `Content-Disposition` , `Content-Encoding` a`Content-Type`
* `Cookie`
* `Expires`
* `Host`
* `Last-Modified`
* `Origin`
* `Set-Cookie`
* `Transfer-Encoding`

I když Logic Apps nebrání v ukládání aplikací logiky, které používají Trigger HTTP nebo akci s těmito záhlavími, Logic Apps tyto hlavičky ignoruje.

## <a name="connector-reference"></a>Referenční informace ke konektorům

Další informace o parametrech Trigger a Action najdete v těchto částech:

* [Parametry triggeru HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md#http-trigger)
* [Parametry akce HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action)

### <a name="output-details"></a>Podrobnosti výstupu

Zde jsou další informace o výstupech z triggeru nebo akce HTTP, které vrací tyto informace:

| Vlastnost | Typ | Popis |
|----------|------|-------------|
| `headers` | Objekt JSON | Hlavičky z požadavku |
| `body` | Objekt JSON | Objekt s obsahem textu z požadavku |
| `status code` | Celé číslo | Stavový kód z požadavku |
|||

| Stavový kód | Popis |
|-------------|-------------|
| 200 | OK |
| 202 | Přijato |
| 400 | Chybný požadavek |
| 401 | Neautorizováno |
| 403 | Forbidden |
| 404 | Nenalezeno |
| 500 | Vnitřní chyba serveru. Došlo k neznámé chybě. |
|||

## <a name="next-steps"></a>Další kroky

* Další informace o dalších [konektorech Logic Apps](../connectors/apis-list.md)

