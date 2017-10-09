---
title: "aaaCreate Azure IoT Hub pomocí šablony (PowerShell) | Microsoft Docs"
description: "Jak toouse toocreate šablony Azure Resource Manager služby IoT Hub pomocí prostředí PowerShell."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 7eade855-c289-4ffb-b5ef-02be8c5f670f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: e98ff5e898200cd727b9326fb3df393e43b021e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a>Vytvoření služby IoT hub pomocí šablony Azure Resource Manageru (PowerShell)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Můžete použít Azure Resource Manager toocreate a spravovat Azure IoT hubs prostřednictvím kódu programu. Tento kurz ukazuje, jak toouse toocreate šablony Azure Resource Manager služby IoT hub pomocí prostředí PowerShell.

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Azure Resource Manager hello.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Aktivní účet Azure. <br/>Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* [Azure PowerShell 1.0] [ lnk-powershell-install] nebo novější.

> [!TIP]
> článek Hello [použití Azure Powershellu s Azure Resource Manager] [ lnk-powershell-arm] poskytuje další informace o toouse prostředí PowerShell a Azure Resource Manager toocreate šablony Azure prostředky.

## <a name="connect-tooyour-azure-subscription"></a>Připojit tooyour předplatného Azure

V příkazovém řádku prostředí PowerShell zadejte následující příkaz toosign v tooyour předplatného Azure hello:

```powershell
Login-AzureRmAccount
```

Pokud máte víc předplatných Azure, přihlášení tooAzure uděluje přístup tooall hello předplatná Azure přidružená přihlašovacích údajů. Použijte následující příkaz toolist hello předplatná Azure k dispozici pro vás toouse hello:

```powershell
Get-AzureRMSubscription
```

Použijte následující příkaz tooselect předplatné, že budete chtít toouse toorun hello příkazy toocreate služby IoT hub hello. Název odběru hello nebo ID můžete použít z hello výstup hello předchozí příkaz:

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

Můžete použít následující příkazy toodiscover, kde můžete nasadit služby IoT hub a hello aktuálně podporované verze API hello:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Vytvoření služby IoT hub pomocí hello následující příkaz v jednom z umístění hello podporované pro IoT Hub toocontain skupiny prostředků. Tento příklad vytvoří skupinu prostředků s názvem **MyIoTRG1**:

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-a-template-toocreate-an-iot-hub"></a>Odeslání toocreate šablony služby IoT hub

Použijte toocreate JSON šablony služby IoT hub ve vaší skupině prostředků. Můžete použít také Azure Resource Manager šablony toomake změny tooan stávající služby IoT hub.

1. Použít text editoru toocreate názvem šablonu Azure Resource Manager **template.json** s hello následující toocreate definice prostředků novou standardní službu IoT hub. Tento příklad přidá hello IoT Hub v hello **východní USA** oblast, vytvoří dvě skupiny uživatelů (**cg1** a **cg2**) na koncový bod hello kompatibilní s centrem událostí a hello používá **2016-02-03** verze rozhraní API. Tato šablona také očekává, že budete toopass v název centra IoT hello jako parametr s názvem **hubName**. Hello aktuální seznam umístění, které podporují služby IoT Hub naleznete v části [stavu Azure][lnk-status].

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Uložte soubor šablony Azure Resource Manager hello na místním počítači. Tento příklad předpokládá můžete uložit ve složce s názvem **c:\templates**.

3. Spusťte následující příkaz toodeploy hello nového centra IoT předávání hello název služby IoT hub jako parametr. V tomto příkladu je název hello hello IoT hub `abcmyiothub`. musí být globálně jedinečný název Hello služby IoT hub:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. výstup Hello zobrazí hello klíče pro hello IoT hub, kterou jste vytvořili.

5. tooverify přidat aplikace hello novou službu IoT hub, navštivte hello [portál Azure] [ lnk-azure-portal] a zobrazení seznamu prostředků. Můžete taky použít hello **Get-AzureRmResource** rutiny prostředí PowerShell.

> [!NOTE]
> Tato ukázková aplikace přidá S1 Standard IoT Hub pro kterou se účtují. Odstraněním hello IoT hub prostřednictvím hello [portál Azure] [ lnk-azure-portal] nebo pomocí hello **odebrat AzureRmResource** rutiny prostředí PowerShell po dokončení.

## <a name="next-steps"></a>Další kroky

Nyní jste nasadili služby IoT hub pomocí šablony Azure Resource Manager pomocí prostředí PowerShell, můžete další tooexplore:

* Přečtěte si informace o možnostech hello hello [zprostředkovatele prostředků služby IoT Hub REST API][lnk-rest-api].
* Čtení [přehled Azure Resource Manageru] [ lnk-azure-rm-overview] toolearn více informací o hello možnosti nástroje Azure Resource Manager.

toolearn Další informace o vývoji pro IoT Hub, najdete v části hello následující články:

* [Úvod tooC SDK][lnk-c-sdk]
* [Sady SDK služby Azure IoT][lnk-sdks]

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Simulaci zařízení s Azure IoT Edge][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../azure-resource-manager/powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
