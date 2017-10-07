---
title: "prostředky Azure Service Bus toomanage prostředí PowerShell aaaUse | Microsoft Docs"
description: "Použít toocreate modulu prostředí PowerShell a správu prostředků služby Service Bus"
services: service-bus-messaging
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/06/2017
ms.author: sethm
ms.openlocfilehash: 737044def913c5798e7e05fc4f1aeece76c8f4dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomanage-service-bus-resources"></a><span data-ttu-id="0d927-103">Použití prostředků služby Service Bus toomanage prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="0d927-103">Use PowerShell toomanage Service Bus resources</span></span>

<span data-ttu-id="0d927-104">Microsoft Azure PowerShell je skriptovací prostředí, můžete použít toocontrol a automatizovat hello nasazení a správu služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="0d927-104">Microsoft Azure PowerShell is a scripting environment that you can use toocontrol and automate hello deployment and management of Azure services.</span></span> <span data-ttu-id="0d927-105">Tento článek popisuje, jak toouse hello [modulu Powershellu pro Service Bus Resource Manager](/powershell/module/azurerm.servicebus) tooprovision a spravovat entit služby Service Bus (obory názvů, fronty, témata a odběry) pomocí místní konzoly Azure PowerShell nebo skript.</span><span class="sxs-lookup"><span data-stu-id="0d927-105">This article describes how toouse hello [Service Bus Resource Manager PowerShell module](/powershell/module/azurerm.servicebus) tooprovision and manage Service Bus entities (namespaces, queues, topics, and subscriptions) using a local Azure PowerShell console or script.</span></span>

<span data-ttu-id="0d927-106">Můžete také spravovat entit služby Service Bus pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0d927-106">You can also manage Service Bus entities using Azure Resource Manager templates.</span></span> <span data-ttu-id="0d927-107">Další informace najdete v článku hello [vytvořit Service Bus prostředků pomocí šablony Azure Resource Manager](service-bus-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0d927-107">For more information, see hello article [Create Service Bus resources using Azure Resource Manager templates](service-bus-resource-manager-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d927-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0d927-108">Prerequisites</span></span>

<span data-ttu-id="0d927-109">Než začnete, budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="0d927-109">Before you begin, you'll need hello following:</span></span>

* <span data-ttu-id="0d927-110">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="0d927-110">An Azure subscription.</span></span> <span data-ttu-id="0d927-111">Další informace o získání předplatného najdete v tématu [možností nákupu][purchase options], [nabízí člen][member offers], nebo [bezplatný účet][free account].</span><span class="sxs-lookup"><span data-stu-id="0d927-111">For more information about obtaining a subscription, see [purchase options][purchase options], [member offers][member offers], or [free account][free account].</span></span>
* <span data-ttu-id="0d927-112">Počítač s prostředím Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0d927-112">A computer with Azure PowerShell.</span></span> <span data-ttu-id="0d927-113">Pokyny najdete v tématu [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="0d927-113">For instructions, see [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps).</span></span>
* <span data-ttu-id="0d927-114">Obecné znalosti skriptů prostředí PowerShell, balíčků NuGet a hello rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0d927-114">A general understanding of PowerShell scripts, NuGet packages, and hello .NET Framework.</span></span>

## <a name="get-started"></a><span data-ttu-id="0d927-115">Začínáme</span><span class="sxs-lookup"><span data-stu-id="0d927-115">Get started</span></span>

<span data-ttu-id="0d927-116">prvním krokem Hello je toouse toolog prostředí PowerShell v tooyour účet Azure a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="0d927-116">hello first step is toouse PowerShell toolog in tooyour Azure account and Azure subscription.</span></span> <span data-ttu-id="0d927-117">Postupujte podle pokynů hello v [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/get-started-azureps) toolog tooyour účet Azure a načtení a přístup hello prostředky ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="0d927-117">Follow hello instructions in [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps) toolog in tooyour Azure account, and retrieve and access hello resources in your Azure subscription.</span></span>

## <a name="provision-a-service-bus-namespace"></a><span data-ttu-id="0d927-118">Zřízení oboru názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="0d927-118">Provision a Service Bus namespace</span></span>

<span data-ttu-id="0d927-119">Při práci s obory názvů Service Bus, můžete použít hello [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace), [New-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace), [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace), a [Set-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespace) rutiny.</span><span class="sxs-lookup"><span data-stu-id="0d927-119">When working with Service Bus namespaces, you can use hello [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace), [New-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace), [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace), and [Set-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespace) cmdlets.</span></span>

<span data-ttu-id="0d927-120">Tento příklad vytvoří pár lokální proměnné ve skriptu hello; `$Namespace` a `$Location`.</span><span class="sxs-lookup"><span data-stu-id="0d927-120">This example creates a few local variables in hello script; `$Namespace` and `$Location`.</span></span>

* <span data-ttu-id="0d927-121">`$Namespace`je název oboru názvů Service Bus hello se kterým má být toowork hello.</span><span class="sxs-lookup"><span data-stu-id="0d927-121">`$Namespace` is hello name of hello Service Bus namespace with which we want toowork.</span></span>
* <span data-ttu-id="0d927-122">`$Location`identifikuje hello datového centra, ve kterém jsme zřizovat hello oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="0d927-122">`$Location` identifies hello data center in which will we provision hello namespace.</span></span>
* <span data-ttu-id="0d927-123">`$CurrentNamespace`úložiště hello odkaz na obor názvů, který jsme načíst (nebo ji vytvořte).</span><span class="sxs-lookup"><span data-stu-id="0d927-123">`$CurrentNamespace` stores hello reference namespace that we retrieve (or create).</span></span>

<span data-ttu-id="0d927-124">Ve skriptu skutečné `$Namespace` a `$Location` lze předat jako parametry.</span><span class="sxs-lookup"><span data-stu-id="0d927-124">In an actual script, `$Namespace` and `$Location` can be passed as parameters.</span></span>

<span data-ttu-id="0d927-125">Tato část skriptu hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="0d927-125">This part of hello script does hello following:</span></span>

1. <span data-ttu-id="0d927-126">Pokusy o tooretrieve v oboru názvů Service Bus s hello zadán název.</span><span class="sxs-lookup"><span data-stu-id="0d927-126">Attempts tooretrieve a Service Bus namespace with hello specified name.</span></span>
2. <span data-ttu-id="0d927-127">Pokud je nalezen hello obor názvů, sestavy, co byl nalezen.</span><span class="sxs-lookup"><span data-stu-id="0d927-127">If hello namespace is found, it reports what was found.</span></span>
3. <span data-ttu-id="0d927-128">Pokud není nalezen hello obor názvů, vytvoří obor názvů hello a pak načte hello nově vytvořený obor názvů.</span><span class="sxs-lookup"><span data-stu-id="0d927-128">If hello namespace is not found, it creates hello namespace and then retrieves hello newly created namespace.</span></span>
   
    ``` powershell
    # Query toosee if hello namespace currently exists
    $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
   
    # Check if hello namespace already exists or needs toobe created
    if ($CurrentNamespace)
    {
        Write-Host "hello namespace $Namespace already exists in hello $Location region:"
        # Report what was found
        Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "hello $Namespace namespace does not exist."
        Write-Host "Creating hello $Namespace namespace in hello $Location region..."
        New-AzureRmServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
        Write-Host "hello $Namespace namespace in Resource Group $ResGrpName in hello $Location region has been successfully created."
                
    }
    ```

### <a name="create-a-namespace-authorization-rule"></a><span data-ttu-id="0d927-129">Vytvoření oboru názvů autorizační pravidla</span><span class="sxs-lookup"><span data-stu-id="0d927-129">Create a namespace authorization rule</span></span>

<span data-ttu-id="0d927-130">Hello následující příklad ukazuje, jak toomanage obor názvů autorizační pravidla pomocí hello [New-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespaceauthorizationrule), [Get-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespaceauthorizationrule), [Set-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespaceauthorizationrule), a [rutiny Remove-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespaceauthorizationrule).</span><span class="sxs-lookup"><span data-stu-id="0d927-130">hello following example shows how toomanage namespace authorization rules using hello [New-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespaceauthorizationrule), [Get-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespaceauthorizationrule), [Set-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespaceauthorizationrule), and [Remove-AzureRmServiceBusNamespaceAuthorizationRule cmdlets](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespaceauthorizationrule).</span></span>

```powershell
# Query toosee if rule exists
$CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

# Check if hello rule already exists or needs toobe created
if ($CurrentRule)
{
    Write-Host "hello $AuthRule rule already exists for hello namespace $Namespace."
}
else
{
    Write-Host "hello $AuthRule rule does not exist."
    Write-Host "Creating hello $AuthRule rule for hello $Namespace namespace..."
    New-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule -Rights @("Listen","Send")
    $CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
    Write-Host "hello $AuthRule rule for hello $Namespace namespace has been successfully created."

    Write-Host "Setting rights on hello namespace"
    $authRuleObj = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

    Write-Host "Remove Send rights"
    $authRuleObj.Rights.Remove("Send")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Add Send and Manage rights toohello namespace"
    $authRuleObj.Rights.Add("Send")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj
    $authRuleObj.Rights.Add("Manage")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Show value of primary key"
    $CurrentKey = Get-AzureRmServiceBusNamespaceKey -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
        
    Write-Host "Remove this authorization rule"
    Remove-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
}
```

## <a name="create-a-queue"></a><span data-ttu-id="0d927-131">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="0d927-131">Create a queue</span></span>

<span data-ttu-id="0d927-132">toocreate fronta nebo téma, proveďte kontrolu oboru názvů pomocí skriptu hello v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="0d927-132">toocreate a queue or topic, perform a namespace check using hello script in hello previous section.</span></span> <span data-ttu-id="0d927-133">Pak vytvořte hello fronty:</span><span class="sxs-lookup"><span data-stu-id="0d927-133">Then, create hello queue:</span></span>

```powershell
# Check if queue already exists
$CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName

if($CurrentQ)
{
    Write-Host "hello queue $QueueName already exists in hello $Location region:"
}
else
{
    Write-Host "hello $QueueName queue does not exist."
    Write-Host "Creating hello $QueueName queue in hello $Location region..."
    New-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -EnablePartitioning $True
    $CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName
    Write-Host "hello $QueueName queue in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

### <a name="modify-queue-properties"></a><span data-ttu-id="0d927-134">Úprava vlastností fronty.</span><span class="sxs-lookup"><span data-stu-id="0d927-134">Modify queue properties</span></span>

<span data-ttu-id="0d927-135">Po provedení hello skript v předcházející části hello, můžete použít hello [Set-AzureRmServiceBusQueue](/powershell/module/azurerm.servicebus/set-azurermservicebusqueue) rutiny tooupdate hello vlastností fronty, stejně jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="0d927-135">After executing hello script in hello preceding section, you can use hello [Set-AzureRmServiceBusQueue](/powershell/module/azurerm.servicebus/set-azurermservicebusqueue) cmdlet tooupdate hello properties of a queue, as in hello following example:</span></span>

```powershell
$CurrentQ.DeadLetteringOnMessageExpiration = $True
$CurrentQ.MaxDeliveryCount = 7
$CurrentQ.MaxSizeInMegabytes = 2048
$CurrentQ.EnableExpress = $True

Set-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -QueueObj $CurrentQ
```

## <a name="provisioning-other-service-bus-entities"></a><span data-ttu-id="0d927-136">Zřizování dalšími subjekty, Service Bus</span><span class="sxs-lookup"><span data-stu-id="0d927-136">Provisioning other Service Bus entities</span></span>

<span data-ttu-id="0d927-137">Můžete použít hello [modulu Service Bus PowerShell](/powershell/module/azurerm.servicebus) tooprovision jinými entitami, jako jsou témata a odběry.</span><span class="sxs-lookup"><span data-stu-id="0d927-137">You can use hello [Service Bus PowerShell module](/powershell/module/azurerm.servicebus) tooprovision other entities, such as topics and subscriptions.</span></span> <span data-ttu-id="0d927-138">Tyto rutiny jsou syntakticky podobné toohello fronty vytvoření rutiny ukázáno v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="0d927-138">These cmdlets are syntactically similar toohello queue creation cmdlets demonstrated in hello previous section.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d927-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d927-139">Next steps</span></span>

- <span data-ttu-id="0d927-140">V tématu hello kompletní dokumentaci modulu Powershellu pro Service Bus Resource Manager [zde](/powershell/module/azurerm.servicebus).</span><span class="sxs-lookup"><span data-stu-id="0d927-140">See hello complete Service Bus Resource Manager PowerShell module documentation [here](/powershell/module/azurerm.servicebus).</span></span> <span data-ttu-id="0d927-141">Tato stránka obsahuje seznam všech dostupných rutin.</span><span class="sxs-lookup"><span data-stu-id="0d927-141">This page lists all available cmdlets.</span></span>
- <span data-ttu-id="0d927-142">Informace o používání šablon Azure Resource Manageru, najdete v článku hello [vytvořit Service Bus prostředků pomocí šablony Azure Resource Manager](service-bus-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0d927-142">For information about using Azure Resource Manager templates, see hello article [Create Service Bus resources using Azure Resource Manager templates](service-bus-resource-manager-overview.md).</span></span>
- <span data-ttu-id="0d927-143">Informace o [knihovny Service Bus .NET správy](service-bus-management-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="0d927-143">Information about [Service Bus .NET management libraries](service-bus-management-libraries.md).</span></span>

<span data-ttu-id="0d927-144">Existují některé alternativní způsoby toomanage entit služby Service Bus, jak je popsáno v těchto příspěvcích na blogu:</span><span class="sxs-lookup"><span data-stu-id="0d927-144">There are some alternate ways toomanage Service Bus entities, as described in these blog posts:</span></span>

* [<span data-ttu-id="0d927-145">Jak toocreate Service Bus fronty, témata a odběry pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="0d927-145">How toocreate Service Bus queues, topics and subscriptions using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [<span data-ttu-id="0d927-146">Jak toocreate a Service Bus Namespace a Centrum událostí pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="0d927-146">How toocreate a Service Bus Namespace and an Event Hub using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)
* [<span data-ttu-id="0d927-147">Skripty prostředí PowerShell služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="0d927-147">Service Bus PowerShell Scripts</span></span>](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
