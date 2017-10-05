---
title: "Vytvoření služby Azure IoT Hub pomocí rutiny prostředí PowerShell | Microsoft Docs"
description: "Jak používat rutiny prostředí PowerShell k vytvoření služby IoT hub."
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
ms.openlocfilehash: 02227adeb8a9a7463506efa44ddc2977f8aae65a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-iot-hub-using-the-new-azurermiothub-cmdlet"></a><span data-ttu-id="5c9b0-103">Vytvoření služby IoT hub pomocí rutiny New-AzureRmIotHub</span><span class="sxs-lookup"><span data-stu-id="5c9b0-103">Create an IoT hub using the New-AzureRmIotHub cmdlet</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a><span data-ttu-id="5c9b0-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="5c9b0-104">Introduction</span></span>

<span data-ttu-id="5c9b0-105">Rutiny prostředí Azure PowerShell můžete použít k vytváření a správě Azure IoT hubs.</span><span class="sxs-lookup"><span data-stu-id="5c9b0-105">You can use Azure PowerShell cmdlets to create and manage Azure IoT hubs.</span></span> <span data-ttu-id="5c9b0-106">V tomto kurzu se dozvíte, jak vytvoření služby IoT hub pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5c9b0-106">This tutorial shows you how to create an IoT hub with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="5c9b0-107">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5c9b0-107">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="5c9b0-108">Tento článek se zabývá pomocí modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5c9b0-108">This article covers using the Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="5c9b0-109">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="5c9b0-109">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="5c9b0-110">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="5c9b0-110">An active Azure account.</span></span> <br/><span data-ttu-id="5c9b0-111">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="5c9b0-111">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="5c9b0-112">[Rutiny Azure PowerShell][lnk-powershell-install].</span><span class="sxs-lookup"><span data-stu-id="5c9b0-112">[Azure PowerShell cmdlets][lnk-powershell-install].</span></span>

## <a name="connect-to-your-azure-subscription"></a><span data-ttu-id="5c9b0-113">Připojení k předplatnému služby Azure</span><span class="sxs-lookup"><span data-stu-id="5c9b0-113">Connect to your Azure subscription</span></span>
<span data-ttu-id="5c9b0-114">V příkazovém řádku prostředí PowerShell zadejte následující příkaz k přihlášení k předplatnému Azure:</span><span class="sxs-lookup"><span data-stu-id="5c9b0-114">In a PowerShell command prompt, enter the following command to sign in to your Azure subscription:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="5c9b0-115">Pokud máte víc předplatných Azure, přihlášení do Azure uděluje přístup do všech předplatná Azure přidružená přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="5c9b0-115">If you have multiple Azure subscriptions, signing in to Azure grants you access to all the Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="5c9b0-116">Pomocí následujícího příkazu zobrazíte seznam předplatných Azure, které je k dispozici pro použití:</span><span class="sxs-lookup"><span data-stu-id="5c9b0-116">Use the following command to list the Azure subscriptions available for you to use:</span></span>

```powershell
Get-AzureRMSubscription
```

<span data-ttu-id="5c9b0-117">Pomocí následujícího příkazu vyberte předplatné, které chcete použít ke spuštění příkazů pro vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5c9b0-117">Use the following command to select subscription that you want to use to run the commands to create your IoT hub.</span></span> <span data-ttu-id="5c9b0-118">Z výstupu předchozí příkaz můžete použít buď název odběru nebo ID:</span><span class="sxs-lookup"><span data-stu-id="5c9b0-118">You can use either the subscription name or ID from the output of the previous command:</span></span>

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

## <a name="create-resource-group"></a><span data-ttu-id="5c9b0-119">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5c9b0-119">Create resource group</span></span>

<span data-ttu-id="5c9b0-120">Potřebujete skupinu prostředků pro nasazení služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5c9b0-120">You need a resource group to deploy an IoT hub.</span></span> <span data-ttu-id="5c9b0-121">Můžete použít existující skupinu prostředků nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="5c9b0-121">You can use an existing resource group or create a new one.</span></span>

<span data-ttu-id="5c9b0-122">Tento příkaz můžete vyhledat umístění, kde můžete nasadit služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="5c9b0-122">You can use the following command to discover the locations where you can deploy an IoT hub:</span></span>

```powershell
((Get-AzureRmResourceProvider `
  -ProviderNamespace Microsoft.Devices).ResourceTypes `
  | Where-Object ResourceTypeName -eq IoTHubs).Locations
```

<span data-ttu-id="5c9b0-123">Pokud chcete vytvořit skupinu prostředků pro službu IoT hub v jednom z podporovaných umístění pro IoT Hub, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="5c9b0-123">To create a resource group for your IoT hub in one of the supported locations for IoT Hub, use the following command.</span></span> <span data-ttu-id="5c9b0-124">Tento příklad vytvoří skupinu prostředků s názvem **MyIoTRG1** v **východní USA** oblasti:</span><span class="sxs-lookup"><span data-stu-id="5c9b0-124">This example creates a resource group called **MyIoTRG1** in the **East US** region:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a><span data-ttu-id="5c9b0-125">Vytvoření centra IoT</span><span class="sxs-lookup"><span data-stu-id="5c9b0-125">Create an IoT hub</span></span>

<span data-ttu-id="5c9b0-126">Chcete-li vytvoření služby IoT hub ve skupině prostředků, kterou jste vytvořili v předchozím kroku, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="5c9b0-126">To create an IoT hub in the resource group you created in the previous step, use the following command.</span></span> <span data-ttu-id="5c9b0-127">Tento příklad vytvoří **S1** rozbočovače názvem **MyTestIoTHub** v **východní USA** oblasti:</span><span class="sxs-lookup"><span data-stu-id="5c9b0-127">This example creates an **S1** hub called **MyTestIoTHub** in the **East US** region:</span></span>

```powershell
New-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

<span data-ttu-id="5c9b0-128">Název služby IoT hub musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="5c9b0-128">The name of the IoT hub must be unique.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


<span data-ttu-id="5c9b0-129">Všechny služby IoT hubs je možné uvést v rámci vašeho předplatného, pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="5c9b0-129">You can list all the IoT hubs in your subscription using the following command:</span></span>

```powershell
Get-AzureRmIotHub
```

<span data-ttu-id="5c9b0-130">Předchozí příklad přidá S1 Standard IoT Hub pro kterou se účtují.</span><span class="sxs-lookup"><span data-stu-id="5c9b0-130">The previous example adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="5c9b0-131">Odstraněním služby IoT hub pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="5c9b0-131">You can delete the IoT hub using the following command:</span></span>

```powershell
Remove-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

<span data-ttu-id="5c9b0-132">Alternativně můžete odebrat skupinu prostředků a všechny prostředky, které obsahuje pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="5c9b0-132">Alternatively, you can remove a resource group and all the resources it contains using the following command:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a><span data-ttu-id="5c9b0-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5c9b0-133">Next steps</span></span>

<span data-ttu-id="5c9b0-134">Nyní jste nasadili služby IoT hub pomocí rutiny prostředí PowerShell, můžete chtít Další:</span><span class="sxs-lookup"><span data-stu-id="5c9b0-134">Now you have deployed an IoT hub using a PowerShell cmdlet, you may want to explore further:</span></span>

* <span data-ttu-id="5c9b0-135">Zjišťování dalších [rutiny prostředí PowerShell pro práci se službou IoT hub][lnk-iothub-cmdlets].</span><span class="sxs-lookup"><span data-stu-id="5c9b0-135">Discover other [PowerShell cmdlets for working with your IoT hub][lnk-iothub-cmdlets].</span></span>
* <span data-ttu-id="5c9b0-136">Přečtěte si informace o možnostech [zprostředkovatele prostředků služby IoT Hub REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="5c9b0-136">Read about the capabilities of the [IoT Hub resource provider REST API][lnk-rest-api].</span></span>

<span data-ttu-id="5c9b0-137">Další informace o vývoji pro Centrum IoT, naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="5c9b0-137">To learn more about developing for IoT Hub, see the following articles:</span></span>

* <span data-ttu-id="5c9b0-138">[Úvod do jazyka C SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="5c9b0-138">[Introduction to C SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="5c9b0-139">[Sady SDK služby Azure IoT][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="5c9b0-139">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="5c9b0-140">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="5c9b0-140">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="5c9b0-141">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="5c9b0-141">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-iothub-cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.iothub/
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
