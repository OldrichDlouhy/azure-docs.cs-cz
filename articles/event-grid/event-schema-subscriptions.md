---
title: Předplatné Azure jako zdroj Event Grid
description: Popisuje vlastnosti, které jsou k dispozici pro události předplatného s Azure Event Grid
ms.topic: reference
ms.date: 07/07/2020
ms.openlocfilehash: 72b1a73bf418b417cd29f88063781e7b45979998
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86105893"
---
# <a name="azure-subscription-as-an-event-grid-source"></a>Předplatné Azure jako zdroj Event Grid

Tento článek popisuje vlastnosti a schéma pro události předplatného Azure.Úvod do schémat událostí najdete v tématu [Azure Event Grid schéma událostí](event-schema.md).

Předplatná Azure a skupiny prostředků emitují stejné typy událostí. Typy událostí se vztahují ke změnám prostředků nebo akcím. Hlavním rozdílem je, že skupiny prostředků generují události pro prostředky v rámci skupiny prostředků a předplatné Azure generuje události pro prostředky v rámci předplatného.

Události prostředků se vytvářejí pro operace PUT, PATCH, POST a DELETE, které se odesílají do `management.azure.com` . Operace GET nevytváří události. Operace odeslané do roviny dat (například `myaccount.blob.core.windows.net` ) nevytvářejí události. Události akce poskytují data události pro operace, jako je například výpis klíčů pro určitý prostředek.

Když se přihlásíte k odběru událostí pro předplatné Azure, koncový bod obdrží všechny události pro toto předplatné. Události mohou zahrnovat událost, kterou chcete zobrazit, jako je například aktualizace virtuálního počítače, ale také události, které nemusí být pro vás důležité, například zápis nové položky v historii nasazení. Můžete přijmout všechny události v koncovém bodu a napsat kód, který zpracovává události, které chcete zpracovat. Případně můžete nastavit filtr při vytváření odběru události.

Chcete-li události programově zpracovávat, můžete události řadit podle `operationName` hodnoty. Například váš koncový bod události může zpracovávat pouze události pro operace, které jsou rovny `Microsoft.Compute/virtualMachines/write` nebo `Microsoft.Storage/storageAccounts/write` .

Předmět události je ID prostředku prostředku, který je cílem operace. Chcete-li filtrovat události pro určitý prostředek, zadejte toto ID prostředku při vytváření odběru události. Chcete-li filtrovat podle typu prostředku, použijte hodnotu v následujícím formátu:`/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.Compute/virtualMachines`


## <a name="event-grid-event-schema"></a>Schéma událostí služby Event Grid

### <a name="available-event-types"></a>Dostupné typy událostí

Předplatné Azure generuje události správy z Azure Resource Manager, například když se vytvoří virtuální počítač nebo se odstraní účet úložiště.

| Typ události | Description |
| ---------- | ----------- |
| Microsoft. Resources. ResourceActionCancel | Je aktivována při zrušení akce u prostředku. |
| Microsoft. Resources. ResourceActionFailure | Vyvolá se v případě, že akce u prostředku není úspěšná. |
| Microsoft. Resources. ResourceActionSuccess | Vyvolá se, když je akce prostředku úspěšná. |
| Microsoft. Resources. ResourceDeleteCancel | Vyvolá se při zrušení operace delete. Tato událost nastane, když se zruší nasazení šablony. |
| Microsoft. Resources. ResourceDeleteFailure | Vyvolá se v případě, že operace odstranění není úspěšná. |
| Microsoft. Resources. ResourceDeleteSuccess | Vyvolá se, když je operace delete úspěšná. |
| Microsoft. Resources. ResourceWriteCancel | Vyvolá se při zrušení operace CREATE nebo Update. |
| Microsoft. Resources. ResourceWriteFailure | Vyvolá se v případě, že operace vytvoření nebo aktualizace není úspěšná. |
| Microsoft. Resources. ResourceWriteSuccess | Vyvolá se, když je operace vytvořit nebo aktualizovat úspěšná. |

### <a name="example-event"></a>Příklad události

Následující příklad ukazuje schéma pro událost **ResourceWriteSuccess** . Stejné schéma se používá pro události **ResourceWriteFailure** a **ResourceWriteCancel** s různými hodnotami pro `eventType` .

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceWriteSuccess",
  "eventTime": "2018-07-19T18:38:04.6117357Z",
  "id": "4db48cba-50a2-455a-93b4-de41a3b5b7f6",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/write",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "{expiration}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/write",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}"
}]
```

Následující příklad ukazuje schéma pro událost **ResourceDeleteSuccess** . Stejné schéma se používá pro události **ResourceDeleteFailure** a **ResourceDeleteCancel** s různými hodnotami pro `eventType` .

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2018-07-19T19:24:12.763881Z",
  "id": "19a69642-1aad-4a96-a5ab-8d05494513ce",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/delete",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "262800",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "DELETE",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2018-02-01"
    },
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}"
}]
```

Následující příklad ukazuje schéma pro událost **ResourceActionSuccess** . Stejné schéma se používá pro události **ResourceActionFailure** a **ResourceActionCancel** s různými hodnotami pro `eventType` .

```json
[{   
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
  "eventType": "Microsoft.Resources.ResourceActionSuccess",
  "eventTime": "2018-10-08T22:46:22.6022559Z",
  "id": "{ID}",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
      "action": "Microsoft.EventHub/namespaces/AuthorizationRules/listKeys/action",
      "evidence": {
        "role": "Contributor",
        "roleAssignmentScope": "/subscriptions/{subscription-id}",
        "roleAssignmentId": "{ID}",
        "roleDefinitionId": "{ID}",
        "principalId": "{ID}",
        "principalType": "ServicePrincipal"
      }     
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "aio": "{token}",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/identity/claims/identityprovider": "{URL}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",       "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "POST",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey/listKeys?api-version=2017-04-01"
    },
    "resourceProvider": "Microsoft.EventHub",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
    "operationName": "Microsoft.EventHub/namespaces/AuthorizationRules/listKeys/action",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}" 
}]
```

### <a name="event-properties"></a>Vlastnosti události

Událost má následující data nejvyšší úrovně:

| Vlastnost | Typ | Description |
| -------- | ---- | ----------- |
| téma | řetězec | Úplná cesta prostředku ke zdroji událostí. Do tohoto pole nejde zapisovat. Tuto hodnotu poskytuje Event Grid. |
| závislosti | řetězec | Cesta k předmětu události, kterou definuje vydavatel. |
| Typ | řetězec | Jeden z registrovaných typů události pro tento zdroj události. |
| eventTime | řetězec | Čas, kdy se událost generuje na základě času UTC poskytovatele. |
| id | řetězec | Jedinečný identifikátor události |
| data | odkazy objektů | Data události odběru |
| dataVersion | řetězec | Verze schématu datového objektu. Verzi schématu definuje vydavatel. |
| metadataVersion | řetězec | Verze schématu metadat události. Schéma vlastností nejvyšší úrovně definuje Event Grid. Tuto hodnotu poskytuje Event Grid. |

Datový objekt má následující vlastnosti:

| Vlastnost | Typ | Description |
| -------- | ---- | ----------- |
| autorizace | odkazy objektů | Požadovaná autorizace pro operaci. |
| podpory | odkazy objektů | Vlastnosti deklarací identity. Další informace najdete v tématu [specifikace JWT](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html). |
| correlationId | řetězec | ID operace pro řešení potíží. |
| httpRequest | odkazy objektů | Podrobnosti operace Tento objekt je zahrnutý pouze při aktualizaci stávajícího prostředku nebo při odstraňování prostředku. |
| resourceProvider | řetězec | Poskytovatel prostředků pro operaci. |
| resourceUri | řetězec | Identifikátor URI prostředku v operaci. |
| operationName | řetězec | Operace, kterou jste provedli. |
| status | řetězec | Stav operace. |
| subscriptionId | řetězec | ID předplatného prostředku |
| tenantId | řetězec | ID tenanta prostředku |

## <a name="tutorials-and-how-tos"></a>Kurzy a postupy
|Nadpis |Popis  |
|---------|---------|
| [Kurz: Azure Automation s využitím Event Grid a Microsoft Teams](ensure-tags-exists-on-new-virtual-machines.md) |Vytvořte virtuální počítač, který odešle událost. Událost aktivuje Runbook služby Automation, který zaznamená virtuální počítač, a aktivuje zprávu odeslanou kanálu Microsoft Teams. |
| [Postupy: přihlášení k odběru událostí prostřednictvím portálu](subscribe-through-portal.md) | Použijte portál k přihlášení k odběru událostí pro předplatné Azure. |
| [Azure CLI: přihlášení k odběru událostí pro předplatné Azure](./scripts/event-grid-cli-azure-subscription.md) |Ukázkový skript, který vytvoří předplatné služby Event Grid k předplatnému Azure a odesílá události do Webhooku. |
| [PowerShell: přihlášení k odběru událostí pro předplatné Azure](./scripts/event-grid-powershell-azure-subscription.md)| Ukázkový skript, který vytvoří předplatné služby Event Grid k předplatnému Azure a odesílá události do Webhooku. |

## <a name="next-steps"></a>Další kroky

* Úvod do Azure Event Grid najdete v tématu [co je Event Grid?](overview.md).
* Další informace o vytváření předplatného Azure Event Grid najdete v tématu [schéma předplatného Event Grid](subscription-creation-schema.md).
