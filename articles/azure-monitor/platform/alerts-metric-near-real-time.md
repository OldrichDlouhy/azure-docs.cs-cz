---
title: Podporované prostředky pro výstrahy metriky v Azure Monitor
description: Referenční informace o metrikách a protokolech podpory pro výstrahy metrik v Azure Monitor
author: harelbr
ms.author: harelbr
services: monitoring
ms.topic: conceptual
ms.date: 7/26/2020
ms.subservice: alerts
ms.openlocfilehash: bbfbc5e92fc49e45d1d16621992dbf4483f01bb5
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87327153"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Podporované prostředky pro výstrahy metriky v Azure Monitor

Azure Monitor teď podporuje [nový typ výstrahy metriky](./alerts-overview.md) , který má významné výhody oproti starším [klasickým výstrahám metrik](./alerts-classic.overview.md). K dispozici jsou metriky pro [velký seznam služeb Azure](./metrics-supported.md). Novější výstrahy podporují (rostoucí) podmnožinu typů prostředků. Tento článek obsahuje seznam této podmnožiny.

Můžete také použít novější výstrahy metriky pro oblíbená data protokolu uložená v Log Analytics pracovním prostoru extrahované jako metriky. Další informace najdete v [upozorněních metriky pro protokoly](./alerts-metric-logs.md).

## <a name="portal-powershell-cli-rest-support"></a>Portál, PowerShell, CLI, podpora REST
V současné době můžete vytvořit novější výstrahy metrik pouze v [šablonách](./alerts-metric-create-templates.md)Azure Portal, [REST API](/rest/api/monitor/metricalerts/)nebo správce prostředků. Brzy se připravuje podpora pro konfiguraci novějších výstrah pomocí PowerShellu a Azure CLI verze 2,0 a novější.

## <a name="metrics-and-dimensions-supported"></a>Podporované metriky a dimenze
Novější výstrahy metrik podporují upozorňování na metriky, které používají dimenze. Dimenze můžete použít k filtrování metriky na správnou úroveň. Všechny podporované metriky spolu s použitelnými rozměry lze prozkoumat a rozlišit z [Azure monitor-Průzkumník metrik](./metrics-charts.md).

Tady je úplný seznam zdrojů metrik Azure monitorů podporovaných novějšími výstrahami:

|Typ prostředku  |Podporované dimenze |Výstrahy s více prostředky| Dostupné metriky|
|---------|---------|-----|----------|
|Microsoft.ApiManagement/service | Ano | Ne | [API Management](./metrics-supported.md#microsoftapimanagementservice) |
|Microsoft. AppConfiguration/configurationStores |Ano | Ne | [App Configuration](./metrics-supported.md#microsoftappconfigurationconfigurationstores) |
|Microsoft. AppPlatform/pružina | Ano | Ne | [Azure Spring Cloud](./metrics-supported.md#microsoftappplatformspring) |
|Microsoft. Automation/automationAccounts | Ano| Ne | [Účty služby Automation](./metrics-supported.md#microsoftautomationautomationaccounts) |
|Microsoft.Batch/batchAccounts | Ano | Ne | [Účty Batch](./metrics-supported.md#microsoftbatchbatchaccounts) |
|Microsoft. cache/Redis | Ano | Ne | [Azure Cache for Redis](./metrics-supported.md#microsoftcacheredis) |
|Microsoft. ClassicStorage/storageAccounts | Ano | Ne | [Účty úložiště (klasické)](./metrics-supported.md#microsoftclassicstoragestorageaccounts) |
|Microsoft. ClassicStorage/storageAccounts/blobServices | Ano | Ne | |
|Microsoft. ClassicStorage/storageAccounts/služby | Ano | Ne | |
|Microsoft. ClassicStorage/storageAccounts/queueServices | Ano | Ne | |
|Microsoft. ClassicStorage/storageAccounts/tableServices | Ano | Ne | |
|Microsoft. Cognitiveservices Account/účty | Ano | Ne | [Cognitive Services](./metrics-supported.md#microsoftcognitiveservicesaccounts) |
|Microsoft.Compute/virtualMachines | Ano | Ano | [Virtual Machines](./metrics-supported.md#microsoftcomputevirtualmachines) |
|Microsoft.Compute/virtualMachineScaleSets | Ano | Ne |[Virtual Machine Scale Sets](./metrics-supported.md#microsoftcomputevirtualmachinescalesets) |
|Microsoft. ContainerInstance/containerGroups | Ano| Ne | [Skupiny kontejnerů](./metrics-supported.md#microsoftcontainerinstancecontainergroups) |
|Microsoft. ContainerRegistry/Registry | Ne | Ne | [Registry kontejnerů](./metrics-supported.md#microsoftcontainerregistryregistries) |
|Microsoft. ContainerService/managedClusters | Ano | Ne | [Spravované clustery](./metrics-supported.md#microsoftcontainerservicemanagedclusters) |
|Microsoft. DataBoxEdge/dataBoxEdgeDevices | Ano | Ano | [Data Box](./metrics-supported.md#microsoftdataboxedgedataboxedgedevices) |
|Microsoft. DataFactory/DataFactory| Ano| Ne | [Datové továrny v1](./metrics-supported.md#microsoftdatafactorydatafactories) |
|Microsoft. DataFactory/továrny |Ano | Ne | [Datové továrny v2](./metrics-supported.md#microsoftdatafactoryfactories) |
|Microsoft. datashare/Accounts | Ano | Ne | |
|Microsoft. DBforMariaDB/servery | Ne | Ne | [DB pro MariaDB](./metrics-supported.md#microsoftdbformariadbservers) |
|Microsoft. DBforMySQL/servery | Ne | Ne |[DB pro MySQL](./metrics-supported.md#microsoftdbformysqlservers)|
|Microsoft. DBforPostgreSQL/servery | Ne | Ne | [DB pro PostgreSQL](./metrics-supported.md#microsoftdbforpostgresqlservers)|
|Microsoft. DBforPostgreSQL/serversv2 | Ne | Ne | [DB pro PostgreSQL v2](./metrics-supported.md#microsoftdbforpostgresqlserversv2)|
|Microsoft. DBforPostgreSQL/singleservers | Ne | Ne | [DB pro PostgreSQL (jednotlivé servery)](./metrics-supported.md#microsoftdbforpostgresqlsingleservers)|
|Microsoft. Devices/IotHubs | Ano | Ne |[IoT Hub](./metrics-supported.md#microsoftdevicesiothubs) |
|Microsoft. Devices/provisioningServices| Ano | Ne | [Služby Device Provisioning](./metrics-supported.md#microsoftdevicesprovisioningservices) |
|Microsoft.DocumentDB/databaseAccounts | Ano | Ne | [Databáze Cosmos](./metrics-supported.md#microsoftdocumentdbdatabaseaccounts) |
|Microsoft. EventGrid/domény | Ano | Ne | [Domény Event Gridu](./metrics-supported.md#microsofteventgriddomains) |
|Microsoft. EventGrid/systemTopics | Ano | Ne | [Témata týkající se Event Grid systému](./metrics-supported.md#microsofteventgridsystemtopics) |
|Microsoft. EventGrid/témata |Ano | Ne | [Témata Event Gridu](./metrics-supported.md#microsofteventgridtopics) |
|Microsoft. EventHub/clustery |Ano| Ne | [Clustery Event Hubs](./metrics-supported.md#microsofteventhubclusters) |
|Microsoft. EventHub/obory názvů |Ano| Ne | [Event Hubs](./metrics-supported.md#microsofteventhubnamespaces) |
|Microsoft. HDInsight/clustery | Ano | Ne | [Clustery HDInsight](./metrics-supported.md#microsofthdinsightclusters) |
|Microsoft. Insights/Components | Ano | Ne | [Application Insights](./metrics-supported.md#microsoftinsightscomponents) |
|Trezory a trezory Microsoft. | Ne |Ne |[Trezory](./metrics-supported.md#microsoftkeyvaultvaults)|
|Microsoft. Kusto/clustery | Ano |Ne |[Clustery Průzkumník dat](./metrics-supported.md#microsoftkustoclusters)|
|Microsoft. Logic/Workflows | Ne | Ne |[Logic Apps](./metrics-supported.md#microsoftlogicworkflows) |
|Microsoft. MachineLearningServices/pracovní prostory | Ano | Ne | [Machine Learning](./metrics-supported.md#microsoftmachinelearningservicesworkspaces) |
|Microsoft. Maps/Accounts | Ano | Ne | [Účty mapy](./metrics-supported.md#microsoftmapsaccounts) |
|Microsoft. Media/MediaServices | Ne | Ne | [Media Services](./metrics-supported.md#microsoftmediamediaservices) |
|Microsoft. Media/MediaServices/starají | Ano | Ne | [Koncové body streamování Media Services](./metrics-supported.md#microsoftmediamediaservicesstreamingendpoints) |
|Microsoft. NetApp/netAppAccounts/capacityPools | Ano | Ne | [Fondy kapacit Azure NetApp](./metrics-supported.md#microsoftnetappnetappaccountscapacitypools) |
|Microsoft. NetApp/netAppAccounts/capacityPools/svazky | Ano | Ne | [Svazky NetApp Azure](./metrics-supported.md#microsoftnetappnetappaccountscapacitypoolsvolumes) |
|Microsoft. Network/applicationGateways | Ano | Ne | [Aplikační brány](./metrics-supported.md#microsoftnetworkapplicationgateways) |
|Microsoft. Network/azurefirewalls | Ano | Ne | [brány firewall,](./metrics-supported.md#microsoftnetworkazurefirewalls) |
|Microsoft. Network/dnsZones | Ne | Ne | [Zóny DNS](./metrics-supported.md#microsoftnetworkdnszones) |
|Microsoft. Network/expressRouteCircuits | – | Ne |[Okruhy ExpressRoute](./metrics-supported.md#microsoftnetworkexpressroutecircuits) |
|Microsoft. Network/loadBalancers (pouze pro standardní SKU)| Ano| Ne | [Nástroje pro vyrovnávání zatížení](./metrics-supported.md#microsoftnetworkloadbalancers) |
|Microsoft. Network/natGateways| Ne | Ne | |
|Microsoft. Network/privateEndpoints| Ne | Ne | |
|Microsoft. Network/privateLinkServices| Ne | Ne |
|Microsoft. Network/publicipaddresses | Ne | Ne |[Veřejné IP adresy](./metrics-supported.md#microsoftnetworkpublicipaddresses)|
|Microsoft. Network/trafficManagerProfiles | Ano | Ne | [Profily Traffic Manageru](./metrics-supported.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft. OperationalInsights/pracovní prostory| Ano | Ne | [Pracovní prostory služby Log Analytics](./metrics-supported.md#microsoftoperationalinsightsworkspaces)|
|Microsoft. peering/peering | Ano | Ne | [Partnerské vztahy](./metrics-supported.md#microsoftpeeringpeerings) |
|Microsoft. peering/peeringServices | Ano | Ne | [Služby Peering Service](./metrics-supported.md#microsoftpeeringpeeringservices) |
|Microsoft. PowerBIDedicated/– kapacity | Ne | Ne | [Capacities](./metrics-supported.md#microsoftpowerbidedicatedcapacities) |
|Microsoft. Relay/obory názvů | Ano | Ne | [Předávání](./metrics-supported.md#microsoftrelaynamespaces) |
|Microsoft. Search/searchServices | Ne | Ne | [Hledat služby](./metrics-supported.md#microsoftsearchsearchservices) |
|Microsoft. ServiceBus/obory názvů | Ano | Ne | [Service Bus](./metrics-supported.md#microsoftservicebusnamespaces) |
|Microsoft. SQL/managedInstances | Ne | Ano | [Spravované instance SQL](./metrics-supported.md#microsoftsqlmanagedinstances) |
|Microsoft. SQL/servery/databáze | Ne | Ano | [Databáze SQL](./metrics-supported.md#microsoftsqlserversdatabases) |
|Microsoft. SQL/servery/elasticPools | Ne | Ano | [Elastické fondy SQL](./metrics-supported.md#microsoftsqlserverselasticpools) |
|Microsoft. Storage/storageAccounts |Ano | Ne | [Účty úložiště](./metrics-supported.md#microsoftstoragestorageaccounts)|
|Microsoft. Storage/storageAccounts/Services | Ano| Ne | Služby [BLOB Services](./metrics-supported.md#microsoftstoragestorageaccountsblobservices), [souborové](./metrics-supported.md#microsoftstoragestorageaccountsfileservices)služby, [služby front](./metrics-supported.md#microsoftstoragestorageaccountsqueueservices) a [tabulkové služby](./metrics-supported.md#microsoftstoragestorageaccountstableservices)|
|Microsoft. StorageCache/caches | Ano | Ne | |
|Microsoft. StorageSync/storageSyncServices | Ano | Ne | [Služby synchronizace úložiště](./metrics-supported.md#microsoftstoragesyncstoragesyncservices) |
|Microsoft. StreamAnalytics/streamingjobs | Ano | Ne | [Stream Analytics](./metrics-supported.md#microsoftstreamanalyticsstreamingjobs) |
|Microsoft. VMWareCloudSimple/virtualMachines | Ano | Ne | [Virtuální počítače CloudSimple](./metrics-supported.md#microsoftvmwarecloudsimplevirtualmachines) |
|Microsoft. Web/hostingEnvironments/multiRolePools | Ano | Ne | [App Service Environment fondů s více rolemi](./metrics-supported.md#microsoftwebhostingenvironmentsmultirolepools)|
|Microsoft. Web/hostingEnvironments/workerPools | Ano | Ne | [App Service Environment fondy pracovních procesů](./metrics-supported.md#microsoftwebhostingenvironmentsworkerpools)|
|Microsoft. Web/serverových farem | Ano | Ne | [Plány App Service](./metrics-supported.md#microsoftwebserverfarms)|
|Microsoft. Web/weby | Ano | Ne | [App Services](./metrics-supported.md#microsoftwebsites-excluding-functions) a [funkce](./metrics-supported.md#microsoftwebsites-functions)|
|Microsoft. Web/weby/sloty | Ano | Ne | [Sloty App Service](./metrics-supported.md#microsoftwebsitesslots)|

## <a name="payload-schema"></a>Schéma datové části

> [!NOTE]
> Můžete také použít [běžné schéma výstrah](https://aka.ms/commonAlertSchemaDocs), které poskytuje pro integraci Webhooku jedinou rozšiřitelnou a jednotnou datovou část napříč všemi službami výstrahy v Azure monitor. [Přečtěte si o běžných definicích schémat výstrah.](https://aka.ms/commonAlertSchemaDefinitions)


Operace POST obsahuje následující datovou část a schéma JSON pro všechny blízké novější výstrahy metriky, když se používá vhodně konfigurovaná [Skupina akcí](./action-groups.md) :

```json
{
  "schemaId": "AzureMonitorMetricAlert",
  "data": {
    "version": "2.0",
    "status": "Activated",
    "context": {
      "timestamp": "2018-02-28T10:44:10.1714014Z",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
      "name": "StorageCheck",
      "description": "",
      "conditionType": "SingleResourceMultipleMetricCriteria",
      "severity":"3",
      "condition": {
        "windowSize": "PT5M",
        "allOf": [
          {
            "metricName": "Transactions",
            "metricNamespace":"microsoft.storage/storageAccounts",
            "dimensions": [
              {
                "name": "AccountResourceId",
                "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
              },
              {
                "name": "GeoType",
                "value": "Primary"
              }
            ],
            "operator": "GreaterThan",
            "threshold": "0",
            "timeAggregation": "PT5M",
            "metricValue": 1
          }
        ]
      },
      "subscriptionId": "00000000-0000-0000-0000-000000000000",
      "resourceGroupName": "Contoso",
      "resourceName": "diag500",
      "resourceType": "Microsoft.Storage/storageAccounts",
      "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
      "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
    },
    "properties": {
      "key1": "value1",
      "key2": "value2"
    }
  }
}
```

## <a name="next-steps"></a>Další kroky

* Přečtěte si další informace o novém [prostředí výstrah](./alerts-overview.md).
* Přečtěte si informace o [upozorněních protokolu v Azure](./alerts-unified-log.md).
* Přečtěte si o [výstrahách v Azure](./alerts-overview.md).

