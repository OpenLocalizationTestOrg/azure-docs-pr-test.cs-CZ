---
title: "prostředky Azure Event Hubs toomanage prostředí PowerShell aaaUse | Microsoft Docs"
description: "Použít toocreate modulu prostředí PowerShell a spravovat služby Event Hubs"
services: event-hubs
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: d79cb307c2b4a031d059ce6ca67117ffc0b4600b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomanage-event-hubs-resources"></a><span data-ttu-id="b66fa-103">Pomocí prostředí PowerShell toomanage Event Hubs prostředky</span><span class="sxs-lookup"><span data-stu-id="b66fa-103">Use PowerShell toomanage Event Hubs resources</span></span>

<span data-ttu-id="b66fa-104">Microsoft Azure PowerShell je skriptovací prostředí, můžete použít toocontrol a automatizovat hello nasazení a správu služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="b66fa-104">Microsoft Azure PowerShell is a scripting environment that you can use toocontrol and automate hello deployment and management of Azure services.</span></span> <span data-ttu-id="b66fa-105">Tento článek popisuje, jak toouse hello [modulu PowerShell Resource Manager centra událostí](/powershell/module/azurerm.eventhub) tooprovision a spravovat služby Event Hubs entity (obory názvů, jednotlivé služba event hubs a skupiny uživatelů) pomocí místní konzoly Azure PowerShell nebo skript.</span><span class="sxs-lookup"><span data-stu-id="b66fa-105">This article describes how toouse hello [Event Hubs Resource Manager PowerShell module](/powershell/module/azurerm.eventhub) tooprovision and manage Event Hubs entities (namespaces, individual event hubs, and consumer groups) using a local Azure PowerShell console or script.</span></span>

<span data-ttu-id="b66fa-106">Můžete také spravovat služby Event Hubs prostředků pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b66fa-106">You can also manage Event Hubs resources using Azure Resource Manager templates.</span></span> <span data-ttu-id="b66fa-107">Další informace najdete v článku hello [vytvoření oboru názvů Event Hubs s skupina rozbočovače a příjemce událostí pomocí šablony Azure Resource Manager](event-hubs-resource-manager-namespace-event-hub.md).</span><span class="sxs-lookup"><span data-stu-id="b66fa-107">For more information, see hello article [Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b66fa-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b66fa-108">Prerequisites</span></span>

<span data-ttu-id="b66fa-109">Než začnete, budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="b66fa-109">Before you begin, you'll need hello following:</span></span>

* <span data-ttu-id="b66fa-110">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="b66fa-110">An Azure subscription.</span></span> <span data-ttu-id="b66fa-111">Další informace o získání předplatného najdete v tématu [možností nákupu][purchase options], [nabízí člen][member offers], nebo [bezplatný účet][free account].</span><span class="sxs-lookup"><span data-stu-id="b66fa-111">For more information about obtaining a subscription, see [purchase options][purchase options], [member offers][member offers], or [free account][free account].</span></span>
* <span data-ttu-id="b66fa-112">Počítač s prostředím Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b66fa-112">A computer with Azure PowerShell.</span></span> <span data-ttu-id="b66fa-113">Pokyny najdete v tématu [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="b66fa-113">For instructions, see [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps).</span></span>
* <span data-ttu-id="b66fa-114">Obecné znalosti skriptů prostředí PowerShell, balíčků NuGet a hello rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b66fa-114">A general understanding of PowerShell scripts, NuGet packages, and hello .NET Framework.</span></span>

## <a name="get-started"></a><span data-ttu-id="b66fa-115">Začínáme</span><span class="sxs-lookup"><span data-stu-id="b66fa-115">Get started</span></span>

<span data-ttu-id="b66fa-116">prvním krokem Hello je toouse toolog prostředí PowerShell v tooyour účet Azure a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="b66fa-116">hello first step is toouse PowerShell toolog in tooyour Azure account and Azure subscription.</span></span> <span data-ttu-id="b66fa-117">Postupujte podle pokynů hello v [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/get-started-azureps) toolog v tooyour účet Azure, pak načíst a přístup k prostředkům hello ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="b66fa-117">Follow hello instructions in [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps) toolog in tooyour Azure account, then retrieve and access hello resources in your Azure subscription.</span></span>

## <a name="provision-an-event-hubs-namespace"></a><span data-ttu-id="b66fa-118">Zřídit na obor názvů služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="b66fa-118">Provision an Event Hubs namespace</span></span>

<span data-ttu-id="b66fa-119">Při práci se službou Event Hubs obory názvů, můžete použít hello [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [New-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [odebrat AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace) , a [Set-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) rutiny.</span><span class="sxs-lookup"><span data-stu-id="b66fa-119">When working with Event Hubs namespaces, you can use hello [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [New-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [Remove-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace), and [Set-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) cmdlets.</span></span>

<span data-ttu-id="b66fa-120">Tento příklad vytvoří pár lokální proměnné ve skriptu hello; `$Namespace` a `$Location`.</span><span class="sxs-lookup"><span data-stu-id="b66fa-120">This example creates a few local variables in hello script; `$Namespace` and `$Location`.</span></span>

* <span data-ttu-id="b66fa-121">`$Namespace`je název hello hello Event Hubs oboru názvů, ke kterému má být toowork.</span><span class="sxs-lookup"><span data-stu-id="b66fa-121">`$Namespace` is hello name of hello Event Hubs namespace with which we want toowork.</span></span>
* <span data-ttu-id="b66fa-122">`$Location`identifikuje hello datového centra, ve kterém jsme zřizovat hello oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="b66fa-122">`$Location` identifies hello data center in which will we provision hello namespace.</span></span>
* <span data-ttu-id="b66fa-123">`$CurrentNamespace`úložiště hello odkaz na obor názvů, který jsme načíst (nebo ji vytvořte).</span><span class="sxs-lookup"><span data-stu-id="b66fa-123">`$CurrentNamespace` stores hello reference namespace that we retrieve (or create).</span></span>

<span data-ttu-id="b66fa-124">Ve skriptu skutečné `$Namespace` a `$Location` lze předat jako parametry.</span><span class="sxs-lookup"><span data-stu-id="b66fa-124">In an actual script, `$Namespace` and `$Location` can be passed as parameters.</span></span>

<span data-ttu-id="b66fa-125">Tato část skriptu hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="b66fa-125">This part of hello script does hello following:</span></span>

1. <span data-ttu-id="b66fa-126">Pokusy o tooretrieve na obor názvů služby Event Hubs s hello zadán název.</span><span class="sxs-lookup"><span data-stu-id="b66fa-126">Attempts tooretrieve an Event Hubs namespace with hello specified name.</span></span>
2. <span data-ttu-id="b66fa-127">Pokud je nalezen hello obor názvů, sestavy, co byl nalezen.</span><span class="sxs-lookup"><span data-stu-id="b66fa-127">If hello namespace is found, it reports what was found.</span></span>
3. <span data-ttu-id="b66fa-128">Pokud není nalezen hello obor názvů, vytvoří obor názvů hello a pak načte hello nově vytvořený obor názvů.</span><span class="sxs-lookup"><span data-stu-id="b66fa-128">If hello namespace is not found, it creates hello namespace and then retrieves hello newly created namespace.</span></span>

    ```powershell
    # Query toosee if hello namespace currently exists
    $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
   
    # Check if hello namespace already exists or needs toobe created
    if ($CurrentNamespace)
    {
        Write-Host "hello namespace $Namespace already exists in hello $Location region:"
        # Report what was found
        Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "hello $Namespace namespace does not exist."
        Write-Host "Creating hello $Namespace namespace in hello $Location region..."
        New-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
        Write-Host "hello $Namespace namespace in Resource Group $ResGrpName in hello $Location region has been successfully created."
    }
    ```

## <a name="create-an-event-hub"></a><span data-ttu-id="b66fa-129">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="b66fa-129">Create an event hub</span></span>

<span data-ttu-id="b66fa-130">toocreate centra událostí, proveďte kontrolu oboru názvů pomocí skriptu hello v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="b66fa-130">toocreate an event hub, perform a namespace check using hello script in hello previous section.</span></span> <span data-ttu-id="b66fa-131">Poté použijte hello [New-AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) centra událostí hello toocreate rutiny:</span><span class="sxs-lookup"><span data-stu-id="b66fa-131">Then, use hello [New-AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) cmdlet toocreate hello event hub:</span></span>

```powershell
# Check if event hub already exists
$CurrentEH = Get-AzureRMEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName

if($CurrentEH)
{
    Write-Host "hello event hub $EventHubName already exists in hello $Location region:"
    # Report what was found
    Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "hello $EventHubName event hub does not exist."
    Write-Host "Creating hello $EventHubName event hub in hello $Location region..."
    New-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -Location $Location -MessageRetentionInDays 3
    $CurrentEH = Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "hello $EventHubName event hub in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

### <a name="create-a-consumer-group"></a><span data-ttu-id="b66fa-132">Vytvořte skupinu uživatelů</span><span class="sxs-lookup"><span data-stu-id="b66fa-132">Create a consumer group</span></span>

<span data-ttu-id="b66fa-133">toocreate skupiny příjemců v Centru událostí provést hello oboru názvů událostí rozbočovače kontroly a pomocí skriptů hello v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="b66fa-133">toocreate a consumer group within an event hub, perform hello namespace and event hub checks using hello scripts in hello previous section.</span></span> <span data-ttu-id="b66fa-134">Poté použijte hello [New-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) rutiny toocreate hello skupiny uživatelů v rámci centra událostí hello.</span><span class="sxs-lookup"><span data-stu-id="b66fa-134">Then, use hello [New-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) cmdlet toocreate hello consumer group within hello event hub.</span></span> <span data-ttu-id="b66fa-135">Například:</span><span class="sxs-lookup"><span data-stu-id="b66fa-135">For example:</span></span>

```powershell
# Check if consumer group already exists
$CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -ErrorAction Ignore

if($CurrentCG)
{
    Write-Host "hello consumer group $ConsumerGroupName in event hub $EventHubName already exists in hello $Location region:"
    # Report what was found
    Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "hello $ConsumerGroupName consumer group does not exist."
    Write-Host "Creating hello $ConsumerGroupName consumer group in hello $Location region..."
    New-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
    $CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "hello $ConsumerGroupName consumer group in event hub $EventHubName in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

#### <a name="set-user-metadata"></a><span data-ttu-id="b66fa-136">Metadata uživatele sady</span><span class="sxs-lookup"><span data-stu-id="b66fa-136">Set user metadata</span></span>

<span data-ttu-id="b66fa-137">Po provedení hello skripty v předchozích částech hello, můžete použít hello [Set-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) rutiny tooupdate hello vlastnosti skupiny příjemců, stejně jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="b66fa-137">After executing hello scripts in hello preceding sections, you can use hello [Set-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) cmdlet tooupdate hello properties of a consumer group, as in hello following example:</span></span>

```powershell
# Set some user metadata on hello CG
Write-Host "Setting hello UserMetadata field too'Testing'"
Set-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -UserMetadata "Testing"
# Show result
Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
```

## <a name="remove-event-hub"></a><span data-ttu-id="b66fa-138">Odebrat centra událostí</span><span class="sxs-lookup"><span data-stu-id="b66fa-138">Remove event hub</span></span>

<span data-ttu-id="b66fa-139">Služba event hubs hello tooremove jste vytvořili, můžete použít hello `Remove-*` rutiny jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="b66fa-139">tooremove hello event hubs you created, you can use hello `Remove-*` cmdlets, as in hello following example:</span></span>

```powershell
# Clean up
Remove-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
Remove-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
Remove-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
```

## <a name="next-steps"></a><span data-ttu-id="b66fa-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b66fa-140">Next steps</span></span>

- <span data-ttu-id="b66fa-141">Najdete v dokumentaci hello dokončení Powershellu Resource Manager centra událostí modulu [zde](/powershell/module/azurerm.eventhub).</span><span class="sxs-lookup"><span data-stu-id="b66fa-141">See hello complete Event Hubs Resource Manager PowerShell module documentation [here](/powershell/module/azurerm.eventhub).</span></span> <span data-ttu-id="b66fa-142">Tato stránka obsahuje seznam všech dostupných rutin.</span><span class="sxs-lookup"><span data-stu-id="b66fa-142">This page lists all available cmdlets.</span></span>
- <span data-ttu-id="b66fa-143">Informace o používání šablon Azure Resource Manageru, najdete v článku hello [vytvoření oboru názvů Event Hubs s skupina rozbočovače a příjemce událostí pomocí šablony Azure Resource Manager](event-hubs-resource-manager-namespace-event-hub.md).</span><span class="sxs-lookup"><span data-stu-id="b66fa-143">For information about using Azure Resource Manager templates, see hello article [Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub.md).</span></span>
- <span data-ttu-id="b66fa-144">Informace o [knihovny .NET centra událostí správy](event-hubs-management-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="b66fa-144">Information about [Event Hubs .NET management libraries](event-hubs-management-libraries.md).</span></span>

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
