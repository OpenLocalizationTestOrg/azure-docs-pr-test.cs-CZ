---
title: "Vytvoření služby Azure IoT Hub pomocí šablony (PowerShell) | Microsoft Docs"
description: "Postup vytvoření služby IoT Hub pomocí prostředí PowerShell pomocí šablony Azure Resource Manager."
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
ms.openlocfilehash: f83fac6cffc9e58582417324a4348ca3b6220f0c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a><span data-ttu-id="a0859-103">Vytvoření služby IoT hub pomocí šablony Azure Resource Manageru (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="a0859-103">Create an IoT hub using Azure Resource Manager template (PowerShell)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="a0859-104">Azure Resource Manager můžete použít k vytváření a správě Azure IoT hubs prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="a0859-104">You can use Azure Resource Manager to create and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="a0859-105">V tomto kurzu se dozvíte, jak k vytvoření služby IoT hub pomocí prostředí PowerShell pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a0859-105">This tutorial shows you how to use an Azure Resource Manager template to create an IoT hub with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="a0859-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a0859-106">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="a0859-107">Tento článek se zabývá pomocí modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a0859-107">This article covers using the Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="a0859-108">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="a0859-108">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="a0859-109">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="a0859-109">An active Azure account.</span></span> <br/><span data-ttu-id="a0859-110">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="a0859-110">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="a0859-111">[Azure PowerShell 1.0] [ lnk-powershell-install] nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a0859-111">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

> [!TIP]
> <span data-ttu-id="a0859-112">Článek [použití Azure Powershellu s Azure Resource Manager] [ lnk-powershell-arm] poskytuje další informace o tom, jak pomocí prostředí PowerShell a Azure Resource Manager šablony vytváření prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="a0859-112">The article [Using Azure PowerShell with Azure Resource Manager][lnk-powershell-arm] provides more information about how to use PowerShell and Azure Resource Manager templates to create Azure resources.</span></span>

## <a name="connect-to-your-azure-subscription"></a><span data-ttu-id="a0859-113">Připojení k předplatnému služby Azure</span><span class="sxs-lookup"><span data-stu-id="a0859-113">Connect to your Azure subscription</span></span>

<span data-ttu-id="a0859-114">V příkazovém řádku prostředí PowerShell zadejte následující příkaz k přihlášení k předplatnému Azure:</span><span class="sxs-lookup"><span data-stu-id="a0859-114">In a PowerShell command prompt, enter the following command to sign in to your Azure subscription:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="a0859-115">Pokud máte víc předplatných Azure, přihlášení do Azure uděluje přístup do všech předplatná Azure přidružená přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="a0859-115">If you have multiple Azure subscriptions, signing in to Azure grants you access to all the Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="a0859-116">Pomocí následujícího příkazu zobrazíte seznam předplatných Azure, které je k dispozici pro použití:</span><span class="sxs-lookup"><span data-stu-id="a0859-116">Use the following command to list the Azure subscriptions available for you to use:</span></span>

```powershell
Get-AzureRMSubscription
```

<span data-ttu-id="a0859-117">Pomocí následujícího příkazu vyberte předplatné, které chcete použít ke spuštění příkazů pro vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a0859-117">Use the following command to select subscription that you want to use to run the commands to create your IoT hub.</span></span> <span data-ttu-id="a0859-118">Z výstupu předchozí příkaz můžete použít buď název odběru nebo ID:</span><span class="sxs-lookup"><span data-stu-id="a0859-118">You can use either the subscription name or ID from the output of the previous command:</span></span>

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

<span data-ttu-id="a0859-119">Chcete-li zjistit, kde můžete nasadit služby IoT hub a aktuálně podporované verze rozhraní API můžete použít následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="a0859-119">You can use the following commands to discover where you can deploy an IoT hub and the currently supported API versions:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

<span data-ttu-id="a0859-120">Vytvořte skupinu prostředků tak, aby obsahovala služby IoT hub pomocí následujícího příkazu v jednom z podporovaných umístění pro IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="a0859-120">Create a resource group to contain your IoT hub using the following command in one of the supported locations for IoT Hub.</span></span> <span data-ttu-id="a0859-121">Tento příklad vytvoří skupinu prostředků s názvem **MyIoTRG1**:</span><span class="sxs-lookup"><span data-stu-id="a0859-121">This example creates a resource group called **MyIoTRG1**:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-a-template-to-create-an-iot-hub"></a><span data-ttu-id="a0859-122">Odeslat šablonu pro vytvoření služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="a0859-122">Submit a template to create an IoT hub</span></span>

<span data-ttu-id="a0859-123">Použijte šablonu JSON pro vytvoření služby IoT hub ve vaší skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0859-123">Use a JSON template to create an IoT hub in your resource group.</span></span> <span data-ttu-id="a0859-124">Šablonu Azure Resource Manager můžete také provést změny do stávající služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a0859-124">You can also use an Azure Resource Manager template to make changes to an existing IoT hub.</span></span>

1. <span data-ttu-id="a0859-125">Pomocí textového editoru vytvořit šablonu Azure Resource Manager názvem **template.json** s následující definice prostředků k vytvoření nového centra IoT standardní.</span><span class="sxs-lookup"><span data-stu-id="a0859-125">Use a text editor to create an Azure Resource Manager template called **template.json** with the following resource definition to create a new standard IoT hub.</span></span> <span data-ttu-id="a0859-126">Přidá IoT Hub v tomto příkladu **východní USA** oblast, vytvoří dvě skupiny uživatelů (**cg1** a **cg2**) na koncový bod kompatibilní s centrem událostí a používá  **2016-02-03** verze rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a0859-126">This example adds the IoT Hub in the **East US** region, creates two consumer groups (**cg1** and **cg2**) on the Event Hub-compatible endpoint, and uses the **2016-02-03** API version.</span></span> <span data-ttu-id="a0859-127">Tato šablona také očekává, že budete předávat název centra IoT jako parametr názvem **hubName**.</span><span class="sxs-lookup"><span data-stu-id="a0859-127">This template also expects you to pass in the IoT hub name as a parameter called **hubName**.</span></span> <span data-ttu-id="a0859-128">Aktuální seznam umístění, které podporují služby IoT Hub naleznete v části [stavu Azure][lnk-status].</span><span class="sxs-lookup"><span data-stu-id="a0859-128">For the current list of locations that support IoT Hub see [Azure Status][lnk-status].</span></span>

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

2. <span data-ttu-id="a0859-129">Uložte soubor šablony Azure Resource Manager na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="a0859-129">Save the Azure Resource Manager template file on your local machine.</span></span> <span data-ttu-id="a0859-130">Tento příklad předpokládá můžete uložit ve složce s názvem **c:\templates**.</span><span class="sxs-lookup"><span data-stu-id="a0859-130">This example assumes you save it in a folder called **c:\templates**.</span></span>

3. <span data-ttu-id="a0859-131">Spusťte následující příkaz k nasazení nového centra IoT předávání název služby IoT hub jako parametr.</span><span class="sxs-lookup"><span data-stu-id="a0859-131">Run the following command to deploy your new IoT hub, passing the name of your IoT hub as a parameter.</span></span> <span data-ttu-id="a0859-132">V tomto příkladu je název služby IoT hub `abcmyiothub`.</span><span class="sxs-lookup"><span data-stu-id="a0859-132">In this example, the name of the IoT hub is `abcmyiothub`.</span></span> <span data-ttu-id="a0859-133">Musí být globálně jedinečný název služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="a0859-133">The name of your IoT hub must be globally unique:</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. <span data-ttu-id="a0859-134">Výstup zobrazuje klíče pro službu IoT hub, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a0859-134">The output displays the keys for the IoT hub you created.</span></span>

5. <span data-ttu-id="a0859-135">Chcete-li ověřit, vaše aplikace přidat nového centra IoT, navštivte [portál Azure] [ lnk-azure-portal] a zobrazení seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0859-135">To verify your application added the new IoT hub, visit the [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="a0859-136">Můžete taky použít **Get-AzureRmResource** rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a0859-136">Alternatively, use the **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="a0859-137">Tato ukázková aplikace přidá S1 Standard IoT Hub pro kterou se účtují.</span><span class="sxs-lookup"><span data-stu-id="a0859-137">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="a0859-138">Odstraněním služby IoT hub prostřednictvím [portál Azure] [ lnk-azure-portal] nebo pomocí **AzureRmResource odebrat** rutiny prostředí PowerShell po dokončení.</span><span class="sxs-lookup"><span data-stu-id="a0859-138">You can delete the IoT hub through the [Azure portal][lnk-azure-portal] or by using the **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0859-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a0859-139">Next steps</span></span>

<span data-ttu-id="a0859-140">Nyní jste nasadili služby IoT hub pomocí šablony Azure Resource Manager pomocí prostředí PowerShell, můžete chtít Další:</span><span class="sxs-lookup"><span data-stu-id="a0859-140">Now you have deployed an IoT hub using an Azure Resource Manager template with PowerShell, you may want to explore further:</span></span>

* <span data-ttu-id="a0859-141">Přečtěte si informace o možnostech [zprostředkovatele prostředků služby IoT Hub REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="a0859-141">Read about the capabilities of the [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="a0859-142">Čtení [přehled Azure Resource Manageru] [ lnk-azure-rm-overview] Další informace o funkcích nástroje Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a0859-142">Read [Azure Resource Manager overview][lnk-azure-rm-overview] to learn more about the capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="a0859-143">Další informace o vývoji pro Centrum IoT, naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="a0859-143">To learn more about developing for IoT Hub, see the following articles:</span></span>

* <span data-ttu-id="a0859-144">[Úvod do jazyka C SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="a0859-144">[Introduction to C SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="a0859-145">[Sady SDK služby Azure IoT][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="a0859-145">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="a0859-146">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="a0859-146">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="a0859-147">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="a0859-147">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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
