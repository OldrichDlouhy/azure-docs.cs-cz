---
title: Kurz – streamování protokolů do centra událostí Azure | Microsoft Docs
description: Přečtěte si, jak nastavit Azure Diagnostics pro odesílání Azure Active Directory protokolů do centra událostí.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: eba44252672248b983d7f6e0c843f638e5f73447
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "74007660"
---
# <a name="tutorial-stream-azure-active-directory-logs-to-an-azure-event-hub"></a>Kurz: streamování Azure Active Directorych protokolů do centra událostí Azure

V tomto kurzu se naučíte nastavit Azure Monitor nastavení diagnostiky pro streamování protokolů Azure Active Directory (Azure AD) do centra událostí Azure. Tento mechanismus můžete použít k integraci svých protokolů s nástroji pro správu akcí a informací o zabezpečení (SIEM) třetích stran, jako je Splunk nebo QRadar.

## <a name="prerequisites"></a>Požadavky 

Pokud chcete používat tuto funkci, potřebujete tyto položky:

* Předplatné Azure. Pokud předplatné Azure nemáte, můžete si [zaregistrovat bezplatnou zkušební verzi](https://azure.microsoft.com/free/).
* Tenanta Azure AD.
* Uživatele, který je *globálním správcem* nebo *správcem zabezpečení* pro tohoto tenanta Azure AD.
* Obor názvů služby Event Hubs a centrum událostí ve vašem předplatném Azure. Informace o tom, jak [vytvořit centrum událostí](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

## <a name="stream-logs-to-an-event-hub"></a>Streamování protokolů do centra událostí

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com). 

2. Vyberte **Azure Active Directory** > **monitorování** > **protokolů auditu**. 

3. Vyberte **Exportovat nastavení**.  
    
4. V podokně **Nastavení diagnostiky** proveďte jednu z následujících akcí:
    * Pokud chcete změnit stávající nastavení, vyberte **Upravit nastavení**.
    * Pokud chcete přidat nové nastavení, vyberte **Přidat nastavení diagnostiky**.  
      Můžete mít až tři nastavení.

      ![Export nastavení](./media/quickstart-azure-monitor-stream-logs-to-event-hub/ExportSettings.png)

5. Zaškrtněte políčko **Streamovat do centra událostí** a pak vyberte **Centrum událostí / Konfigurovat**.

6. Vyberte předplatné Azure a obor názvů služby Event Hubs, kam chcete protokoly směrovat.  
    Předplatné i obor názvů služby Event Hubs musí být přidružené k tenantovi Azure AD, ze kterého se protokoly streamují. Můžete také určit, do kterého centra událostí v rámci oboru názvů služby Event Hubs se mají protokoly odesílat. Pokud nezadáte žádné centrum událostí, v oboru názvů se vytvoří centrum událostí s výchozím názvem **insights-logs-audit**.

7. Výběrem **OK** ukončete konfiguraci centra událostí.

8. Proveďte jednu nebo obě z následujících akcí:
    * Pokud chcete do účtu úložiště odesílat protokoly auditu, zaškrtněte políčko **Protokoly auditu**. 
    * Pokud chcete do účtu úložiště odesílat protokoly přihlášení, zaškrtněte políčko **Protokoly přihlášení**.

9. Vyberte **Uložit** a nastavení se uloží.

    ![Nastavení diagnostiky](./media/quickstart-azure-monitor-stream-logs-to-event-hub/DiagnosticSettings.png)

10. Přibližně po 15 minutách zkontrolujte, jestli se v centru událostí zobrazí události. Provedete to tak, že z portálu přejdete do centra událostí a zkontrolujete, jestli je počet **příchozích zpráv** vyšší než nula. 

    ![Protokoly auditu](./media/quickstart-azure-monitor-stream-logs-to-event-hub/InsightsLogsAudit.png)

## <a name="access-data-from-your-event-hub"></a>Přístup k datům z centra událostí

Jakmile se data zobrazí v centru událostí, můžete k nim přistupovat a číst je dvěma způsoby:

* **Konfigurace podporovaného nástroje SIEM**. Většina nástrojů vyžaduje ke čtení dat z centra událostí připojovací řetězec centra událostí a určitá oprávnění k vašemu předplatnému Azure. Mezi nástroje třetích stran, které se integrují se službou Azure Monitor, patří mimo jiné:
    
    * **ArcSight**: Další informace o integraci protokolů služby Azure AD s Splunk najdete v tématu [integrování protokolů Azure Active Directory s ArcSight pomocí Azure monitor](howto-integrate-activity-logs-with-arcsight.md).
    
    * **Splunk:** Další informace o integraci protokolů Azure AD do nástroje Splunk najdete v tématu [Integrace protokolů Azure AD do nástroje Splunk pomocí služby Azure Monitor](tutorial-integrate-activity-logs-with-splunk.md).
    
    * **IBM QRadar:** DSM a Azure Event Hub Protocol můžete stáhnout z webu [podpory IBM](https://www.ibm.com/support). Další informace o integraci s Azure najdete na webu [IBM QRadar Security Intelligence Platform 7.3.0](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0).
    
    * **Sumo Logic:** Informace o nastavení nástroje Sumo Logic na využívání dat z centra událostí najdete v článku [Install the Azure AD app and view the dashboards](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards) (Instalace aplikace Azure AD a zobrazení řídicích panelů). 

* **Nastavení vlastních nástrojů**. Pokud diagnostika služby Azure Monitor ještě nepodporuje váš stávající nástroj SIEM, můžete pomocí rozhraní API služby Event Hubs nastavit vlastní nástroje. Další informace najdete v tématu [Začínáme s přijímáním zpráv z centra událostí](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph).


## <a name="next-steps"></a>Další kroky

* [Integrace protokolů Azure Active Directory s využitím ArcSight pomocí Azure Monitor](howto-integrate-activity-logs-with-arcsight.md)
* [Integrace protokolů Azure AD s nástrojem Splunk pomocí služby Azure Monitor](tutorial-integrate-activity-logs-with-splunk.md)
* [Integrace protokolů Azure AD s nástrojem SumoLogic pomocí služby Azure Monitor](howto-integrate-activity-logs-with-sumologic.md)
* [Interpretace schématu protokolů auditu v Azure Monitor](reference-azure-monitor-audit-log-schema.md)
* [Interpretace schématu protokolů přihlášení ve službě Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md)
