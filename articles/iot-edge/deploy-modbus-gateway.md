---
title: "Nasazení zařízení Modbus se službou Azure IoT Edge | Dokumentace Microsoftu"
description: "Jak umožnit zařízením s protokolem Modbus TCP komunikaci s Azure IoT Hub vytvořením zařízení s bránou IoT Edge"
services: iot-Edge
documentationcenter: 
author: kgremban
manager: timlt
editor: chrisgmsft
ms.assetid: 
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2017
ms.author: kgremban
ms.custom: 
ms.openlocfilehash: e239bde48c3da0d899e3c78bdd39f520c4128b95
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway---preview"></a>Připojení zařízení Modbus TCP přes zařízení s bránou IoT Edge – preview

Pokud chcete připojit zařízení IoT využívající protokol Modbus TCP ke službě Azure IoT Hub, můžete jako bránu použít zařízení IoT Edge. Zařízení brány čte data ze zařízení Modbus a předává je do cloudu s použitím podporovaného protokolu. 

![Připojení zařízení Modbus ke službě IoT Hub přes bránu Edge](./media/deploy-modbus-gateway/diagram.png)

Tento článek popisuje vytvoření vlastní bitové kopie kontejneru pro modul Modbus (nebo můžete použít připravený vzor) a jeho nasazení do zařízení IoT Edge, které bude sloužit jako brána. 

V tomto článku se předpokládá, že používáte protokol Modbus TCP. Další informace o tom, jak nakonfigurovat modul pro podporu Modbus RTU najdete v projektu [Modulu Azure IoT Edge Modbus](https://github.com/Azure/iot-edge-modbus) na Githubu. 

## <a name="prerequisites"></a>Požadavky
* Zařízení Azure IoT Edge. Postup vytvoření tohoto zařízení najdete v článku [Nasazení Azure IoT Edge na simulovaném zařízení ve Windows](tutorial-simulate-device-windows.md) nebo [na Linuxu](tutorial-simulate-device-linux.md). 
* Primární připojovací řetězec klíče pro zařízení IoT Edge.
* Fyzické nebo simulované zařízení Modbus, které podporuje protokol Modbus TCP.

## <a name="prepare-a-modbus-container"></a>Příprava kontejneru Modbus

Chcete-li otestovat funkci brány Modbus, Microsoft nabízí jednoduchý modul, který můžete použít. Chcete-li použít vzorový modul, přejděte k části [Spuštění řešení](#run-the-solution) a zadejte následující jako identifikátor URI bitové kopie: 

```URL
microsoft/azureiotedge-modbus-tcp:1.0-preview
```

Pokud chcete vytvořit vlastní modul a přizpůsobit ho pro vaše prostředí, použijte open source [modul Azure IoT Edge Modbus](https://github.com/Azure/iot-edge-modbus) na Githubu. Podle pokynů uvedených v projektu na GitHubu vytvořte vlastní bitovou kopii kontejneru. Když vytvoříte vlastní bitovou kopii kontejneru, vyhledejte pokyny k jejímu publikování do registru a nasazení vlastního modulu do zařízení v článku [Vývoj a nasazení modulu IoT Edge v jazyce C#](tutorial-csharp-module.md). 


## <a name="run-the-solution"></a>Spuštění řešení
1. Na webu [Azure Portal](https://portal.azure.com/) otevřete vaše centrum IoT Hub.
2. Přejděte na **IoT Edge (preview)** a vyberte zařízení IoT Edge.
3. Vyberte **Set modules** (Nastavit moduly).
4. Přidejte modul Modbus:
   1. Vyberte **Add IoT Edge module** (Přidat modul IoT Edge).
   2. Do pole **Name** (Název) zadejte „modbus“.
   3. Do pole **Image** (Bitová kopie) zadejte identifikátor URI vzorového kontejneru: `microsoft/azureiotedge-modbus-tcp:1.0-preview`.
   4. Zaškrtnutím políčka **Enable** (Povolit) aktualizujte požadované vlastnosti dvojčete modulu.
   5. Do textového pole zkopírujte následující JSON. Změňte hodnotu **SlaveConnection** na adresu IPv4 vašeho zařízení Modbus.

      ```JSON
      {  
        "properties.desired":{  
          "PublishInterval":"2000",
          "SlaveConfigs":{  
            "Slave01":{  
              "SlaveConnection":"<IPV4 address>",
              "HwId":"PowerMeter-0a:01:01:01:01:01",
              "Operations":{  
                "Op01":{  
                  "PollingInterval": "1000",
                  "UnitId":"1",
                  "StartAddress":"400001",
                  "Count":"2",
                  "DisplayName":"Voltage"
                }
              }
            }
          }
        }
      }
      ```

   6. Vyberte **Uložit**.
5. Zpět v kroku **přidání modulů** vyberte **Další**.
7. V kroku **určení tras** zkopírujte do textového pole následující JSON. Tato trasa odešle všechny zprávy shromážděné modulem Modbus do centra IoT Hub. V této trase znamená „modbusOutput“ koncový bod, který modul Modbus pomocí používá pro výstup dat a „upstream“ je speciální cílové umístění, které bráně Edge říká, aby odesílala zprávy do centra IoT Hub. 
   ```JSON
   {
    "routes": {
      "modbusToIoTHub":"FROM /messages/modules/modbus/outputs/modbusOutput INTO $upstream"
    }
   }
   ```

8. Vyberte **Next** (Další). 
9. V kroku **kontroly šablony** vyberte **Submit** (Odeslat). 
10. Vraťte se na obrazovku podrobností o zařízení a zvolte **Refresh** (Obnovit). Měli byste vidět nový modul **modbus** spuštěný společně s runtime IoT Edge.

## <a name="view-data"></a>Zobrazení dat
Takto si můžete prohlédnout data přicházející z modulu modbus:
```cmd/sh
docker logs -f modbus
```

Můžete také zobrazit telemetrická data odeslaná ze zařízení pomocí [nástroje IoT Hub explorer](https://github.com/azure/iothub-explorer). 

## <a name="next-steps"></a>Další kroky

- Další informace o tom, jak zařízení IoT Edge může fungovat jako brána najdete v článku [Vytvoření zařízení IoT Edge které slouží jako transparentní brána](how-to-create-transparent-gateway.md).
- Další informace o fungování modulů IoT Edge najdete v tématu [Vysvětlení modulů Azure IoT Edge](iot-edge-modules.md).