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
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a><span data-ttu-id="be036-103">Vytvoření služby IoT hub pomocí šablony Azure Resource Manageru (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="be036-103">Create an IoT hub using Azure Resource Manager template (PowerShell)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="be036-104">Můžete použít Azure Resource Manager toocreate a spravovat Azure IoT hubs prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="be036-104">You can use Azure Resource Manager toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="be036-105">Tento kurz ukazuje, jak toouse toocreate šablony Azure Resource Manager služby IoT hub pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="be036-105">This tutorial shows you how toouse an Azure Resource Manager template toocreate an IoT hub with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="be036-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="be036-106">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="be036-107">Tento článek se zabývá pomocí modelu nasazení Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="be036-107">This article covers using hello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="be036-108">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="be036-108">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="be036-109">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="be036-109">An active Azure account.</span></span> <br/><span data-ttu-id="be036-110">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="be036-110">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="be036-111">[Azure PowerShell 1.0] [ lnk-powershell-install] nebo novější.</span><span class="sxs-lookup"><span data-stu-id="be036-111">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

> [!TIP]
> <span data-ttu-id="be036-112">článek Hello [použití Azure Powershellu s Azure Resource Manager] [ lnk-powershell-arm] poskytuje další informace o toouse prostředí PowerShell a Azure Resource Manager toocreate šablony Azure prostředky.</span><span class="sxs-lookup"><span data-stu-id="be036-112">hello article [Using Azure PowerShell with Azure Resource Manager][lnk-powershell-arm] provides more information about how toouse PowerShell and Azure Resource Manager templates toocreate Azure resources.</span></span>

## <a name="connect-tooyour-azure-subscription"></a><span data-ttu-id="be036-113">Připojit tooyour předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="be036-113">Connect tooyour Azure subscription</span></span>

<span data-ttu-id="be036-114">V příkazovém řádku prostředí PowerShell zadejte následující příkaz toosign v tooyour předplatného Azure hello:</span><span class="sxs-lookup"><span data-stu-id="be036-114">In a PowerShell command prompt, enter hello following command toosign in tooyour Azure subscription:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="be036-115">Pokud máte víc předplatných Azure, přihlášení tooAzure uděluje přístup tooall hello předplatná Azure přidružená přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="be036-115">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="be036-116">Použijte následující příkaz toolist hello předplatná Azure k dispozici pro vás toouse hello:</span><span class="sxs-lookup"><span data-stu-id="be036-116">Use hello following command toolist hello Azure subscriptions available for you toouse:</span></span>

```powershell
Get-AzureRMSubscription
```

<span data-ttu-id="be036-117">Použijte následující příkaz tooselect předplatné, že budete chtít toouse toorun hello příkazy toocreate služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="be036-117">Use hello following command tooselect subscription that you want toouse toorun hello commands toocreate your IoT hub.</span></span> <span data-ttu-id="be036-118">Název odběru hello nebo ID můžete použít z hello výstup hello předchozí příkaz:</span><span class="sxs-lookup"><span data-stu-id="be036-118">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

<span data-ttu-id="be036-119">Můžete použít následující příkazy toodiscover, kde můžete nasadit služby IoT hub a hello aktuálně podporované verze API hello:</span><span class="sxs-lookup"><span data-stu-id="be036-119">You can use hello following commands toodiscover where you can deploy an IoT hub and hello currently supported API versions:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

<span data-ttu-id="be036-120">Vytvoření služby IoT hub pomocí hello následující příkaz v jednom z umístění hello podporované pro IoT Hub toocontain skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="be036-120">Create a resource group toocontain your IoT hub using hello following command in one of hello supported locations for IoT Hub.</span></span> <span data-ttu-id="be036-121">Tento příklad vytvoří skupinu prostředků s názvem **MyIoTRG1**:</span><span class="sxs-lookup"><span data-stu-id="be036-121">This example creates a resource group called **MyIoTRG1**:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-a-template-toocreate-an-iot-hub"></a><span data-ttu-id="be036-122">Odeslání toocreate šablony služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="be036-122">Submit a template toocreate an IoT hub</span></span>

<span data-ttu-id="be036-123">Použijte toocreate JSON šablony služby IoT hub ve vaší skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="be036-123">Use a JSON template toocreate an IoT hub in your resource group.</span></span> <span data-ttu-id="be036-124">Můžete použít také Azure Resource Manager šablony toomake změny tooan stávající služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="be036-124">You can also use an Azure Resource Manager template toomake changes tooan existing IoT hub.</span></span>

1. <span data-ttu-id="be036-125">Použít text editoru toocreate názvem šablonu Azure Resource Manager **template.json** s hello následující toocreate definice prostředků novou standardní službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="be036-125">Use a text editor toocreate an Azure Resource Manager template called **template.json** with hello following resource definition toocreate a new standard IoT hub.</span></span> <span data-ttu-id="be036-126">Tento příklad přidá hello IoT Hub v hello **východní USA** oblast, vytvoří dvě skupiny uživatelů (**cg1** a **cg2**) na koncový bod hello kompatibilní s centrem událostí a hello používá **2016-02-03** verze rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="be036-126">This example adds hello IoT Hub in hello **East US** region, creates two consumer groups (**cg1** and **cg2**) on hello Event Hub-compatible endpoint, and uses hello **2016-02-03** API version.</span></span> <span data-ttu-id="be036-127">Tato šablona také očekává, že budete toopass v název centra IoT hello jako parametr s názvem **hubName**.</span><span class="sxs-lookup"><span data-stu-id="be036-127">This template also expects you toopass in hello IoT hub name as a parameter called **hubName**.</span></span> <span data-ttu-id="be036-128">Hello aktuální seznam umístění, které podporují služby IoT Hub naleznete v části [stavu Azure][lnk-status].</span><span class="sxs-lookup"><span data-stu-id="be036-128">For hello current list of locations that support IoT Hub see [Azure Status][lnk-status].</span></span>

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

2. <span data-ttu-id="be036-129">Uložte soubor šablony Azure Resource Manager hello na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="be036-129">Save hello Azure Resource Manager template file on your local machine.</span></span> <span data-ttu-id="be036-130">Tento příklad předpokládá můžete uložit ve složce s názvem **c:\templates**.</span><span class="sxs-lookup"><span data-stu-id="be036-130">This example assumes you save it in a folder called **c:\templates**.</span></span>

3. <span data-ttu-id="be036-131">Spusťte následující příkaz toodeploy hello nového centra IoT předávání hello název služby IoT hub jako parametr.</span><span class="sxs-lookup"><span data-stu-id="be036-131">Run hello following command toodeploy your new IoT hub, passing hello name of your IoT hub as a parameter.</span></span> <span data-ttu-id="be036-132">V tomto příkladu je název hello hello IoT hub `abcmyiothub`.</span><span class="sxs-lookup"><span data-stu-id="be036-132">In this example, hello name of hello IoT hub is `abcmyiothub`.</span></span> <span data-ttu-id="be036-133">musí být globálně jedinečný název Hello služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="be036-133">hello name of your IoT hub must be globally unique:</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. <span data-ttu-id="be036-134">výstup Hello zobrazí hello klíče pro hello IoT hub, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="be036-134">hello output displays hello keys for hello IoT hub you created.</span></span>

5. <span data-ttu-id="be036-135">tooverify přidat aplikace hello novou službu IoT hub, navštivte hello [portál Azure] [ lnk-azure-portal] a zobrazení seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="be036-135">tooverify your application added hello new IoT hub, visit hello [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="be036-136">Můžete taky použít hello **Get-AzureRmResource** rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="be036-136">Alternatively, use hello **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="be036-137">Tato ukázková aplikace přidá S1 Standard IoT Hub pro kterou se účtují.</span><span class="sxs-lookup"><span data-stu-id="be036-137">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="be036-138">Odstraněním hello IoT hub prostřednictvím hello [portál Azure] [ lnk-azure-portal] nebo pomocí hello **odebrat AzureRmResource** rutiny prostředí PowerShell po dokončení.</span><span class="sxs-lookup"><span data-stu-id="be036-138">You can delete hello IoT hub through hello [Azure portal][lnk-azure-portal] or by using hello **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="be036-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="be036-139">Next steps</span></span>

<span data-ttu-id="be036-140">Nyní jste nasadili služby IoT hub pomocí šablony Azure Resource Manager pomocí prostředí PowerShell, můžete další tooexplore:</span><span class="sxs-lookup"><span data-stu-id="be036-140">Now you have deployed an IoT hub using an Azure Resource Manager template with PowerShell, you may want tooexplore further:</span></span>

* <span data-ttu-id="be036-141">Přečtěte si informace o možnostech hello hello [zprostředkovatele prostředků služby IoT Hub REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="be036-141">Read about hello capabilities of hello [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="be036-142">Čtení [přehled Azure Resource Manageru] [ lnk-azure-rm-overview] toolearn více informací o hello možnosti nástroje Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="be036-142">Read [Azure Resource Manager overview][lnk-azure-rm-overview] toolearn more about hello capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="be036-143">toolearn Další informace o vývoji pro IoT Hub, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="be036-143">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="be036-144">[Úvod tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="be036-144">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="be036-145">[Sady SDK služby Azure IoT][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="be036-145">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="be036-146">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="be036-146">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="be036-147">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="be036-147">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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
