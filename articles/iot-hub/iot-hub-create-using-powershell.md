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
# <a name="create-an-iot-hub-using-hello-new-azurermiothub-cmdlet"></a><span data-ttu-id="6b805-103">Vytvoření služby IoT hub pomocí rutiny New-AzureRmIotHub hello</span><span class="sxs-lookup"><span data-stu-id="6b805-103">Create an IoT hub using hello New-AzureRmIotHub cmdlet</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a><span data-ttu-id="6b805-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="6b805-104">Introduction</span></span>

<span data-ttu-id="6b805-105">Můžete použít toocreate rutin prostředí Azure PowerShell a spravovat Azure IoT hubs.</span><span class="sxs-lookup"><span data-stu-id="6b805-105">You can use Azure PowerShell cmdlets toocreate and manage Azure IoT hubs.</span></span> <span data-ttu-id="6b805-106">Tento kurz ukazuje, jak toocreate služby IoT hub pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6b805-106">This tutorial shows you how toocreate an IoT hub with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="6b805-107">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="6b805-107">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="6b805-108">Tento článek se zabývá pomocí modelu nasazení Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="6b805-108">This article covers using hello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="6b805-109">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="6b805-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="6b805-110">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="6b805-110">An active Azure account.</span></span> <br/><span data-ttu-id="6b805-111">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="6b805-111">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="6b805-112">[Rutiny Azure PowerShell][lnk-powershell-install].</span><span class="sxs-lookup"><span data-stu-id="6b805-112">[Azure PowerShell cmdlets][lnk-powershell-install].</span></span>

## <a name="connect-tooyour-azure-subscription"></a><span data-ttu-id="6b805-113">Připojit tooyour předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="6b805-113">Connect tooyour Azure subscription</span></span>
<span data-ttu-id="6b805-114">V příkazovém řádku prostředí PowerShell zadejte následující příkaz toosign v tooyour předplatného Azure hello:</span><span class="sxs-lookup"><span data-stu-id="6b805-114">In a PowerShell command prompt, enter hello following command toosign in tooyour Azure subscription:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="6b805-115">Pokud máte víc předplatných Azure, přihlášení tooAzure uděluje přístup tooall hello předplatná Azure přidružená přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="6b805-115">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="6b805-116">Použijte následující příkaz toolist hello předplatná Azure k dispozici pro vás toouse hello:</span><span class="sxs-lookup"><span data-stu-id="6b805-116">Use hello following command toolist hello Azure subscriptions available for you toouse:</span></span>

```powershell
Get-AzureRMSubscription
```

<span data-ttu-id="6b805-117">Použijte následující příkaz tooselect předplatné, že budete chtít toouse toorun hello příkazy toocreate služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="6b805-117">Use hello following command tooselect subscription that you want toouse toorun hello commands toocreate your IoT hub.</span></span> <span data-ttu-id="6b805-118">Název odběru hello nebo ID můžete použít z hello výstup hello předchozí příkaz:</span><span class="sxs-lookup"><span data-stu-id="6b805-118">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

## <a name="create-resource-group"></a><span data-ttu-id="6b805-119">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="6b805-119">Create resource group</span></span>

<span data-ttu-id="6b805-120">Je nutné toodeploy skupiny prostředků služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6b805-120">You need a resource group toodeploy an IoT hub.</span></span> <span data-ttu-id="6b805-121">Můžete použít existující skupinu prostředků nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="6b805-121">You can use an existing resource group or create a new one.</span></span>

<span data-ttu-id="6b805-122">Můžete použít následující příkaz toodiscover hello umístění, kde můžete nasadit služby IoT hub hello:</span><span class="sxs-lookup"><span data-stu-id="6b805-122">You can use hello following command toodiscover hello locations where you can deploy an IoT hub:</span></span>

```powershell
((Get-AzureRmResourceProvider `
  -ProviderNamespace Microsoft.Devices).ResourceTypes `
  | Where-Object ResourceTypeName -eq IoTHubs).Locations
```

<span data-ttu-id="6b805-123">toocreate skupinu prostředků pro službu IoT hub v jednom z umístění hello podporované pro IoT Hub, hello použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="6b805-123">toocreate a resource group for your IoT hub in one of hello supported locations for IoT Hub, use hello following command.</span></span> <span data-ttu-id="6b805-124">Tento příklad vytvoří skupinu prostředků s názvem **MyIoTRG1** v hello **východní USA** oblasti:</span><span class="sxs-lookup"><span data-stu-id="6b805-124">This example creates a resource group called **MyIoTRG1** in hello **East US** region:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a><span data-ttu-id="6b805-125">Vytvoření centra IoT</span><span class="sxs-lookup"><span data-stu-id="6b805-125">Create an IoT hub</span></span>

<span data-ttu-id="6b805-126">toocreate služby IoT hub ve skupině prostředků hello, kterou jste vytvořili v předchozím kroku hello hello použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="6b805-126">toocreate an IoT hub in hello resource group you created in hello previous step, use hello following command.</span></span> <span data-ttu-id="6b805-127">Tento příklad vytvoří **S1** rozbočovače názvem **MyTestIoTHub** v hello **východní USA** oblasti:</span><span class="sxs-lookup"><span data-stu-id="6b805-127">This example creates an **S1** hub called **MyTestIoTHub** in hello **East US** region:</span></span>

```powershell
New-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

<span data-ttu-id="6b805-128">Hello název centra IoT hello musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="6b805-128">hello name of hello IoT hub must be unique.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


<span data-ttu-id="6b805-129">Můžete vytvořit seznam všech centra IoT hello ve vašem předplatném pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6b805-129">You can list all hello IoT hubs in your subscription using hello following command:</span></span>

```powershell
Get-AzureRmIotHub
```

<span data-ttu-id="6b805-130">předchozí příklad Hello přidá S1 Standard IoT Hub pro kterou se účtují.</span><span class="sxs-lookup"><span data-stu-id="6b805-130">hello previous example adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="6b805-131">Odstraněním hello IoT hub pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6b805-131">You can delete hello IoT hub using hello following command:</span></span>

```powershell
Remove-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

<span data-ttu-id="6b805-132">Alternativně můžete odebrat skupinu prostředků a všechny hello prostředků obsahuje pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6b805-132">Alternatively, you can remove a resource group and all hello resources it contains using hello following command:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a><span data-ttu-id="6b805-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6b805-133">Next steps</span></span>

<span data-ttu-id="6b805-134">Nyní jste nasadili služby IoT hub pomocí rutiny prostředí PowerShell, můžete další tooexplore:</span><span class="sxs-lookup"><span data-stu-id="6b805-134">Now you have deployed an IoT hub using a PowerShell cmdlet, you may want tooexplore further:</span></span>

* <span data-ttu-id="6b805-135">Zjišťování dalších [rutiny prostředí PowerShell pro práci se službou IoT hub][lnk-iothub-cmdlets].</span><span class="sxs-lookup"><span data-stu-id="6b805-135">Discover other [PowerShell cmdlets for working with your IoT hub][lnk-iothub-cmdlets].</span></span>
* <span data-ttu-id="6b805-136">Přečtěte si informace o možnostech hello hello [zprostředkovatele prostředků služby IoT Hub REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="6b805-136">Read about hello capabilities of hello [IoT Hub resource provider REST API][lnk-rest-api].</span></span>

<span data-ttu-id="6b805-137">toolearn Další informace o vývoji pro IoT Hub, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="6b805-137">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="6b805-138">[Úvod tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="6b805-138">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="6b805-139">[Sady SDK služby Azure IoT][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="6b805-139">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="6b805-140">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="6b805-140">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="6b805-141">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="6b805-141">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-iothub-cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.iothub/
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
