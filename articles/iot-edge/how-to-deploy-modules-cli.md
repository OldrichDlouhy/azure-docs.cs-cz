---
title: Nasazení modulů z příkazového řádku Azure CLI – Azure IoT Edge
description: Pomocí rozhraní příkazového řádku Azure s rozšířením Azure IoT nahrajte modul IoT Edge z IoT Hub do zařízení IoT Edge, jak je nakonfigurované v manifestu nasazení.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/16/2019
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: fbd0d65624852737c424128e9125b8370b870d4d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "82133948"
---
# <a name="deploy-azure-iot-edge-modules-with-azure-cli"></a>Nasazení modulů Azure IoT Edge pomocí Azure CLI

Jakmile vytvoříte IoT Edge moduly s obchodní logikou, chcete je nasadit do svých zařízení, aby fungovaly na hraničních zařízeních. Pokud máte více modulů, které spolupracují při shromažďování a zpracování dat, můžete je nasadit najednou a deklarovat pravidla směrování, která je spojují.

[Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) je open source nástroj příkazového řádku pro různé platformy, který slouží ke správě prostředků Azure, jako je IoT Edge. Umožňuje spravovat prostředky Azure IoT Hub, instance služby Device Provisioning a propojená centra. Nové rozšíření IoT rozšiřuje rozhraní příkazového řádku Azure pomocí funkcí, jako je Správa zařízení a kompletní funkce IoT Edge.

Tento článek ukazuje, jak vytvořit manifest nasazení JSON a pak ho použít k nahrání nasazení do zařízení IoT Edge. Informace o vytvoření nasazení, které cílí na více zařízení na základě jejich sdílených značek, najdete v tématu [nasazení a sledování IoT Edgech modulů ve velkém měřítku](how-to-deploy-cli-at-scale.md) .

## <a name="prerequisites"></a>Požadavky

* [IoT Hub](../iot-hub/iot-hub-create-using-cli.md) ve vašem předplatném Azure.
* [IoT Edge zařízení](how-to-register-device.md#register-with-the-azure-cli) s nainstalovaným modulem runtime IoT Edge.
* Rozhraní příkazového [řádku Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) ve vašem prostředí. Minimální verze rozhraní příkazového řádku Azure CLI musí být 2.0.70 nebo vyšší. Ke kontrole použijte příkaz `az --version`. Tato verze podporuje příkazy rozšíření az a zavádí příkazové rozhraní Knack.
* [Rozšíření IoT pro Azure CLI](https://github.com/Azure/azure-iot-cli-extension)

## <a name="configure-a-deployment-manifest"></a>Konfigurace manifestu nasazení

Manifest nasazení je dokument JSON, který popisuje, které moduly se mají nasadit, způsob, jakým jsou toky dat mezi moduly a požadované vlastnosti v modulu vlákna. Další informace o tom, jak manifesty nasazení fungují a jak je vytvořit, najdete v tématu [Vysvětlení způsobu použití, konfigurace a](module-composition.md)opětovného použití modulů IoT Edge.

Pokud chcete nasadit moduly pomocí Azure CLI, uložte manifest nasazení lokálně jako soubor. JSON. Cestu k souboru použijete v další části, když spustíte příkaz pro použití konfigurace na zařízení.

Tady je základní manifest nasazení s jedním modulem jako příklad:

```json
{
  "content": {
    "modulesContent": {
      "$edgeAgent": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "runtime": {
            "type": "docker",
            "settings": {
              "minDockerVersion": "v1.25",
              "loggingOptions": "",
              "registryCredentials": {}
            }
          },
          "systemModules": {
            "edgeAgent": {
              "type": "docker",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                "createOptions": "{}"
              }
            },
            "edgeHub": {
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
              }
            }
          },
          "modules": {
            "SimulatedTemperatureSensor": {
              "version": "1.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                "createOptions": "{}"
              }
            }
          }
        }
      },
      "$edgeHub": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "routes": {
            "upstream": "FROM /messages/* INTO $upstream"
          },
          "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
          }
        }
      },
      "SimulatedTemperatureSensor": {
        "properties.desired": {
          "SendData": true,
          "SendInterval": 5
        }
      }
    }
  }
}
```

## <a name="deploy-to-your-device"></a>Nasazení zařízení

Moduly se nasazují do zařízení pomocí manifestu nasazení, který jste nakonfigurovali s informacemi o modulu.

Změňte adresáře do složky, ve které je uložen váš manifest nasazení. Pokud jste použili jednu z šablon VS Code IoT Edge, použijte `deployment.json` soubor ve složce **config** adresáře řešení, a ne na `deployment.template.json` soubor.

Pomocí následujícího příkazu použijte konfiguraci pro IoT Edge zařízení:

   ```azurecli
   az iot edge set-modules --device-id [device id] --hub-name [hub name] --content [file path]
   ```

Parametr ID zařízení rozlišuje velká a malá písmena. Parametr obsahu odkazuje na soubor manifestu nasazení, který jste uložili.

   ![AZ IoT Edge Set-module Output](./media/how-to-deploy-cli/set-modules.png)

## <a name="view-modules-on-your-device"></a>Zobrazit moduly na zařízení

Až nasadíte moduly do svého zařízení, můžete je zobrazit pomocí následujícího příkazu:

Zobrazení modulů v zařízení IoT Edge:

   ```azurecli
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

Parametr ID zařízení rozlišuje velká a malá písmena.

   ![AZ IoT Hub Module-identity list Output](./media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Další kroky

Naučte se [nasazovat a monitorovat IoT Edge moduly ve velkém měřítku](how-to-deploy-at-scale.md) .
