---
title: "aaaCreate Azure IoT Hub pomocí rutiny prostředí PowerShell | Microsoft Docs"
description: "Jak toouse toocreate rutiny prostředí PowerShell služby IoT hub."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 005cd8d48eb39d2e8b1bfb9ef84330d4aae4658f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-new-azurermiothub-cmdlet"></a>Vytvoření služby IoT hub pomocí rutiny New-AzureRmIotHub hello

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Úvod

Můžete použít toocreate rutin prostředí Azure PowerShell a spravovat Azure IoT hubs. Tento kurz ukazuje, jak toocreate služby IoT hub pomocí prostředí PowerShell.

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Azure Resource Manager hello.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Aktivní účet Azure. <br/>Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* [Rutiny Azure PowerShell][lnk-powershell-install].

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

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Je nutné toodeploy skupiny prostředků služby IoT hub. Můžete použít existující skupinu prostředků nebo vytvořte novou.

Můžete použít následující příkaz toodiscover hello umístění, kde můžete nasadit služby IoT hub hello:

```powershell
((Get-AzureRmResourceProvider `
  -ProviderNamespace Microsoft.Devices).ResourceTypes `
  | Where-Object ResourceTypeName -eq IoTHubs).Locations
```

toocreate skupinu prostředků pro službu IoT hub v jednom z umístění hello podporované pro IoT Hub, hello použijte následující příkaz. Tento příklad vytvoří skupinu prostředků s názvem **MyIoTRG1** v hello **východní USA** oblasti:

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a>Vytvoření centra IoT

toocreate služby IoT hub ve skupině prostředků hello, kterou jste vytvořili v předchozím kroku hello hello použijte následující příkaz. Tento příklad vytvoří **S1** rozbočovače názvem **MyTestIoTHub** v hello **východní USA** oblasti:

```powershell
New-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

Hello název centra IoT hello musí být jedinečný.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


Můžete vytvořit seznam všech centra IoT hello ve vašem předplatném pomocí hello následující příkaz:

```powershell
Get-AzureRmIotHub
```

předchozí příklad Hello přidá S1 Standard IoT Hub pro kterou se účtují. Odstraněním hello IoT hub pomocí hello následující příkaz:

```powershell
Remove-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

Alternativně můžete odebrat skupinu prostředků a všechny hello prostředků obsahuje pomocí hello následující příkaz:

```powershell
Remove-AzureRmResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a>Další kroky

Nyní jste nasadili služby IoT hub pomocí rutiny prostředí PowerShell, můžete další tooexplore:

* Zjišťování dalších [rutiny prostředí PowerShell pro práci se službou IoT hub][lnk-iothub-cmdlets].
* Přečtěte si informace o možnostech hello hello [zprostředkovatele prostředků služby IoT Hub REST API][lnk-rest-api].

toolearn Další informace o vývoji pro IoT Hub, najdete v části hello následující články:

* [Úvod tooC SDK][lnk-c-sdk]
* [Sady SDK služby Azure IoT][lnk-sdks]

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Simulaci zařízení s hranou IoT][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-iothub-cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.iothub/
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
