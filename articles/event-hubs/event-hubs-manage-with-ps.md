---
title: "Pomocí prostředí PowerShell ke správě prostředků Azure Event Hubs | Microsoft Docs"
description: "Modul prostředí PowerShell slouží k vytváření a správě služby Event Hubs"
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
ms.openlocfilehash: 2b49c01153b1104612e6ebf9c88566fc40d1f635
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-powershell-to-manage-event-hubs-resources"></a><span data-ttu-id="e8476-103">Pomocí prostředí PowerShell ke správě prostředků služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="e8476-103">Use PowerShell to manage Event Hubs resources</span></span>

<span data-ttu-id="e8476-104">Microsoft Azure PowerShell je skriptovací prostředí, které můžete řídit a automatizovat nasazení a správu služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="e8476-104">Microsoft Azure PowerShell is a scripting environment that you can use to control and automate the deployment and management of Azure services.</span></span> <span data-ttu-id="e8476-105">Tento článek popisuje způsob použití [modulu PowerShell Resource Manager centra událostí](/powershell/module/azurerm.eventhub) a zřizovat a spravovat služby Event Hubs entity (obory názvů, jednotlivé služba event hubs a skupiny uživatelů) pomocí místní konzoly Azure PowerShell nebo skriptu.</span><span class="sxs-lookup"><span data-stu-id="e8476-105">This article describes how to use the [Event Hubs Resource Manager PowerShell module](/powershell/module/azurerm.eventhub) to provision and manage Event Hubs entities (namespaces, individual event hubs, and consumer groups) using a local Azure PowerShell console or script.</span></span>

<span data-ttu-id="e8476-106">Můžete také spravovat služby Event Hubs prostředků pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="e8476-106">You can also manage Event Hubs resources using Azure Resource Manager templates.</span></span> <span data-ttu-id="e8476-107">Další informace najdete v článku [vytvoření oboru názvů Event Hubs s skupina rozbočovače a příjemce událostí pomocí šablony Azure Resource Manager](event-hubs-resource-manager-namespace-event-hub.md).</span><span class="sxs-lookup"><span data-stu-id="e8476-107">For more information, see the article [Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8476-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e8476-108">Prerequisites</span></span>

<span data-ttu-id="e8476-109">Než začnete, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="e8476-109">Before you begin, you'll need the following:</span></span>

* <span data-ttu-id="e8476-110">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e8476-110">An Azure subscription.</span></span> <span data-ttu-id="e8476-111">Další informace o získání předplatného najdete v tématu [možností nákupu][purchase options], [nabízí člen][member offers], nebo [bezplatný účet][free account].</span><span class="sxs-lookup"><span data-stu-id="e8476-111">For more information about obtaining a subscription, see [purchase options][purchase options], [member offers][member offers], or [free account][free account].</span></span>
* <span data-ttu-id="e8476-112">Počítač s prostředím Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e8476-112">A computer with Azure PowerShell.</span></span> <span data-ttu-id="e8476-113">Pokyny najdete v tématu [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="e8476-113">For instructions, see [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps).</span></span>
* <span data-ttu-id="e8476-114">Obecné znalosti skriptů prostředí PowerShell, balíčků NuGet a rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e8476-114">A general understanding of PowerShell scripts, NuGet packages, and the .NET Framework.</span></span>

## <a name="get-started"></a><span data-ttu-id="e8476-115">Začínáme</span><span class="sxs-lookup"><span data-stu-id="e8476-115">Get started</span></span>

<span data-ttu-id="e8476-116">Prvním krokem je pomocí prostředí PowerShell pro přihlášení k účtu Azure a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e8476-116">The first step is to use PowerShell to log in to your Azure account and Azure subscription.</span></span> <span data-ttu-id="e8476-117">Postupujte podle pokynů v [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/get-started-azureps) k přihlášení k účtu Azure, pak načíst a přístup k prostředkům ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="e8476-117">Follow the instructions in [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps) to log in to your Azure account, then retrieve and access the resources in your Azure subscription.</span></span>

## <a name="provision-an-event-hubs-namespace"></a><span data-ttu-id="e8476-118">Zřídit na obor názvů služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="e8476-118">Provision an Event Hubs namespace</span></span>

<span data-ttu-id="e8476-119">Při práci se službou Event Hubs obory názvů, můžete použít [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [New-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [odebrat AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace), a [Set-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) rutiny.</span><span class="sxs-lookup"><span data-stu-id="e8476-119">When working with Event Hubs namespaces, you can use the [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [New-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [Remove-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace), and [Set-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) cmdlets.</span></span>

<span data-ttu-id="e8476-120">Tento příklad vytvoří pár lokální proměnné ve skriptu; `$Namespace` a `$Location`.</span><span class="sxs-lookup"><span data-stu-id="e8476-120">This example creates a few local variables in the script; `$Namespace` and `$Location`.</span></span>

* <span data-ttu-id="e8476-121">`$Namespace`je název oboru názvů služby Event Hubs, se kterým chcete pracovat.</span><span class="sxs-lookup"><span data-stu-id="e8476-121">`$Namespace` is the name of the Event Hubs namespace with which we want to work.</span></span>
* <span data-ttu-id="e8476-122">`$Location`Určuje datové centrum, ve kterém bude jsme zřízení oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="e8476-122">`$Location` identifies the data center in which will we provision the namespace.</span></span>
* <span data-ttu-id="e8476-123">`$CurrentNamespace`ukládá odkaz na obor názvů, který jsme načíst (nebo ji vytvořte).</span><span class="sxs-lookup"><span data-stu-id="e8476-123">`$CurrentNamespace` stores the reference namespace that we retrieve (or create).</span></span>

<span data-ttu-id="e8476-124">Ve skriptu skutečné `$Namespace` a `$Location` lze předat jako parametry.</span><span class="sxs-lookup"><span data-stu-id="e8476-124">In an actual script, `$Namespace` and `$Location` can be passed as parameters.</span></span>

<span data-ttu-id="e8476-125">Tato část skript provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="e8476-125">This part of the script does the following:</span></span>

1. <span data-ttu-id="e8476-126">Pokusí se načíst na obor názvů služby Event Hubs se zadaným názvem.</span><span class="sxs-lookup"><span data-stu-id="e8476-126">Attempts to retrieve an Event Hubs namespace with the specified name.</span></span>
2. <span data-ttu-id="e8476-127">Pokud je nalezen obor názvů, sestavy, co byl nalezen.</span><span class="sxs-lookup"><span data-stu-id="e8476-127">If the namespace is found, it reports what was found.</span></span>
3. <span data-ttu-id="e8476-128">Pokud není nalezen obor názvů, vytvoří obor názvů a pak načte nově vytvořený obor názvů.</span><span class="sxs-lookup"><span data-stu-id="e8476-128">If the namespace is not found, it creates the namespace and then retrieves the newly created namespace.</span></span>

    ```powershell
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
   
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Host "The namespace $Namespace already exists in the $Location region:"
        # Report what was found
        Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "The $Namespace namespace does not exist."
        Write-Host "Creating the $Namespace namespace in the $Location region..."
        New-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
        Write-Host "The $Namespace namespace in Resource Group $ResGrpName in the $Location region has been successfully created."
    }
    ```

## <a name="create-an-event-hub"></a><span data-ttu-id="e8476-129">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="e8476-129">Create an event hub</span></span>

<span data-ttu-id="e8476-130">Vytvoření centra událostí, proveďte kontrolu oboru názvů pomocí skriptu v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="e8476-130">To create an event hub, perform a namespace check using the script in the previous section.</span></span> <span data-ttu-id="e8476-131">Potom použít [New-AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) rutiny pro vytvoření centra událostí:</span><span class="sxs-lookup"><span data-stu-id="e8476-131">Then, use the [New-AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) cmdlet to create the event hub:</span></span>

```powershell
# Check if event hub already exists
$CurrentEH = Get-AzureRMEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName

if($CurrentEH)
{
    Write-Host "The event hub $EventHubName already exists in the $Location region:"
    # Report what was found
    Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "The $EventHubName event hub does not exist."
    Write-Host "Creating the $EventHubName event hub in the $Location region..."
    New-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -Location $Location -MessageRetentionInDays 3
    $CurrentEH = Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "The $EventHubName event hub in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

### <a name="create-a-consumer-group"></a><span data-ttu-id="e8476-132">Vytvořte skupinu uživatelů</span><span class="sxs-lookup"><span data-stu-id="e8476-132">Create a consumer group</span></span>

<span data-ttu-id="e8476-133">Vytvořte skupinu uživatelů v rámci centra událostí, provádět obor názvů a události rozbočovače kontrol pomocí skriptů v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="e8476-133">To create a consumer group within an event hub, perform the namespace and event hub checks using the scripts in the previous section.</span></span> <span data-ttu-id="e8476-134">Potom použít [New-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) vytvořte skupiny uživatelů v rámci centra událostí.</span><span class="sxs-lookup"><span data-stu-id="e8476-134">Then, use the [New-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) cmdlet to create the consumer group within the event hub.</span></span> <span data-ttu-id="e8476-135">Například:</span><span class="sxs-lookup"><span data-stu-id="e8476-135">For example:</span></span>

```powershell
# Check if consumer group already exists
$CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -ErrorAction Ignore

if($CurrentCG)
{
    Write-Host "The consumer group $ConsumerGroupName in event hub $EventHubName already exists in the $Location region:"
    # Report what was found
    Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "The $ConsumerGroupName consumer group does not exist."
    Write-Host "Creating the $ConsumerGroupName consumer group in the $Location region..."
    New-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
    $CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "The $ConsumerGroupName consumer group in event hub $EventHubName in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

#### <a name="set-user-metadata"></a><span data-ttu-id="e8476-136">Metadata uživatele sady</span><span class="sxs-lookup"><span data-stu-id="e8476-136">Set user metadata</span></span>

<span data-ttu-id="e8476-137">Po provedení skripty v předchozí části, můžete použít [Set-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) rutiny aktualizovat vlastnosti skupiny příjemců, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="e8476-137">After executing the scripts in the preceding sections, you can use the [Set-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) cmdlet to update the properties of a consumer group, as in the following example:</span></span>

```powershell
# Set some user metadata on the CG
Write-Host "Setting the UserMetadata field to 'Testing'"
Set-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -UserMetadata "Testing"
# Show result
Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
```

## <a name="remove-event-hub"></a><span data-ttu-id="e8476-138">Odebrat centra událostí</span><span class="sxs-lookup"><span data-stu-id="e8476-138">Remove event hub</span></span>

<span data-ttu-id="e8476-139">Chcete-li odebrat centra událostí, který jste vytvořili, můžete použít `Remove-*` rutin, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="e8476-139">To remove the event hubs you created, you can use the `Remove-*` cmdlets, as in the following example:</span></span>

```powershell
# Clean up
Remove-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
Remove-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
Remove-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
```

## <a name="next-steps"></a><span data-ttu-id="e8476-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e8476-140">Next steps</span></span>

- <span data-ttu-id="e8476-141">Naleznete v dokumentaci k dokončení Powershellu Resource Manager centra událostí modulu [zde](/powershell/module/azurerm.eventhub).</span><span class="sxs-lookup"><span data-stu-id="e8476-141">See the complete Event Hubs Resource Manager PowerShell module documentation [here](/powershell/module/azurerm.eventhub).</span></span> <span data-ttu-id="e8476-142">Tato stránka obsahuje seznam všech dostupných rutin.</span><span class="sxs-lookup"><span data-stu-id="e8476-142">This page lists all available cmdlets.</span></span>
- <span data-ttu-id="e8476-143">Informace o používání šablon Azure Resource Manageru, najdete v článku [vytvoření oboru názvů Event Hubs s skupina rozbočovače a příjemce událostí pomocí šablony Azure Resource Manager](event-hubs-resource-manager-namespace-event-hub.md).</span><span class="sxs-lookup"><span data-stu-id="e8476-143">For information about using Azure Resource Manager templates, see the article [Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub.md).</span></span>
- <span data-ttu-id="e8476-144">Informace o [knihovny .NET centra událostí správy](event-hubs-management-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="e8476-144">Information about [Event Hubs .NET management libraries](event-hubs-management-libraries.md).</span></span>

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
