---
title: "Pomocí prostředí PowerShell ke správě prostředků Azure Service Bus | Microsoft Docs"
description: "Modul prostředí PowerShell slouží k vytvoření a Správa prostředků služby Service Bus"
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
ms.openlocfilehash: 1205f9fcabf5788c970fbce257aa5ad04f32cddc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-manage-service-bus-resources"></a><span data-ttu-id="9e032-103">Pomocí prostředí PowerShell pro správu prostředků služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="9e032-103">Use PowerShell to manage Service Bus resources</span></span>

<span data-ttu-id="9e032-104">Microsoft Azure PowerShell je skriptovací prostředí, které můžete řídit a automatizovat nasazení a správu služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="9e032-104">Microsoft Azure PowerShell is a scripting environment that you can use to control and automate the deployment and management of Azure services.</span></span> <span data-ttu-id="9e032-105">Tento článek popisuje postup použití [modulu Powershellu pro Service Bus Resource Manager](/powershell/module/azurerm.servicebus) zřizují a spravují entit služby Service Bus (obory názvů, fronty, témata a odběry) pomocí místní konzoly Azure PowerShell nebo skriptu.</span><span class="sxs-lookup"><span data-stu-id="9e032-105">This article describes how to use the [Service Bus Resource Manager PowerShell module](/powershell/module/azurerm.servicebus) to provision and manage Service Bus entities (namespaces, queues, topics, and subscriptions) using a local Azure PowerShell console or script.</span></span>

<span data-ttu-id="9e032-106">Můžete také spravovat entit služby Service Bus pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9e032-106">You can also manage Service Bus entities using Azure Resource Manager templates.</span></span> <span data-ttu-id="9e032-107">Další informace najdete v článku [vytvořit Service Bus prostředků pomocí šablony Azure Resource Manager](service-bus-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9e032-107">For more information, see the article [Create Service Bus resources using Azure Resource Manager templates](service-bus-resource-manager-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e032-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9e032-108">Prerequisites</span></span>

<span data-ttu-id="9e032-109">Než začnete, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="9e032-109">Before you begin, you'll need the following:</span></span>

* <span data-ttu-id="9e032-110">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="9e032-110">An Azure subscription.</span></span> <span data-ttu-id="9e032-111">Další informace o získání předplatného najdete v tématu [možností nákupu][purchase options], [nabízí člen][member offers], nebo [bezplatný účet][free account].</span><span class="sxs-lookup"><span data-stu-id="9e032-111">For more information about obtaining a subscription, see [purchase options][purchase options], [member offers][member offers], or [free account][free account].</span></span>
* <span data-ttu-id="9e032-112">Počítač s prostředím Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9e032-112">A computer with Azure PowerShell.</span></span> <span data-ttu-id="9e032-113">Pokyny najdete v tématu [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="9e032-113">For instructions, see [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps).</span></span>
* <span data-ttu-id="9e032-114">Obecné znalosti skriptů prostředí PowerShell, balíčků NuGet a rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9e032-114">A general understanding of PowerShell scripts, NuGet packages, and the .NET Framework.</span></span>

## <a name="get-started"></a><span data-ttu-id="9e032-115">Začínáme</span><span class="sxs-lookup"><span data-stu-id="9e032-115">Get started</span></span>

<span data-ttu-id="9e032-116">Prvním krokem je pomocí prostředí PowerShell pro přihlášení k účtu Azure a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="9e032-116">The first step is to use PowerShell to log in to your Azure account and Azure subscription.</span></span> <span data-ttu-id="9e032-117">Postupujte podle pokynů v [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/get-started-azureps) se přihlaste k účtu Azure a načtení a přístup k prostředkům ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="9e032-117">Follow the instructions in [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps) to log in to your Azure account, and retrieve and access the resources in your Azure subscription.</span></span>

## <a name="provision-a-service-bus-namespace"></a><span data-ttu-id="9e032-118">Zřízení oboru názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="9e032-118">Provision a Service Bus namespace</span></span>

<span data-ttu-id="9e032-119">Při práci s obory názvů Service Bus, můžete použít [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace), [New-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace), [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace), a [Set-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespace) rutiny.</span><span class="sxs-lookup"><span data-stu-id="9e032-119">When working with Service Bus namespaces, you can use the [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace), [New-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace), [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace), and [Set-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespace) cmdlets.</span></span>

<span data-ttu-id="9e032-120">Tento příklad vytvoří pár lokální proměnné ve skriptu; `$Namespace` a `$Location`.</span><span class="sxs-lookup"><span data-stu-id="9e032-120">This example creates a few local variables in the script; `$Namespace` and `$Location`.</span></span>

* <span data-ttu-id="9e032-121">`$Namespace`je název oboru názvů Service Bus, se kterým chcete pracovat.</span><span class="sxs-lookup"><span data-stu-id="9e032-121">`$Namespace` is the name of the Service Bus namespace with which we want to work.</span></span>
* <span data-ttu-id="9e032-122">`$Location`Určuje datové centrum, ve kterém bude jsme zřízení oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="9e032-122">`$Location` identifies the data center in which will we provision the namespace.</span></span>
* <span data-ttu-id="9e032-123">`$CurrentNamespace`ukládá odkaz na obor názvů, který jsme načíst (nebo ji vytvořte).</span><span class="sxs-lookup"><span data-stu-id="9e032-123">`$CurrentNamespace` stores the reference namespace that we retrieve (or create).</span></span>

<span data-ttu-id="9e032-124">Ve skriptu skutečné `$Namespace` a `$Location` lze předat jako parametry.</span><span class="sxs-lookup"><span data-stu-id="9e032-124">In an actual script, `$Namespace` and `$Location` can be passed as parameters.</span></span>

<span data-ttu-id="9e032-125">Tato část skript provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="9e032-125">This part of the script does the following:</span></span>

1. <span data-ttu-id="9e032-126">Pokusí se načíst oboru názvů Service Bus se zadaným názvem.</span><span class="sxs-lookup"><span data-stu-id="9e032-126">Attempts to retrieve a Service Bus namespace with the specified name.</span></span>
2. <span data-ttu-id="9e032-127">Pokud je nalezen obor názvů, sestavy, co byl nalezen.</span><span class="sxs-lookup"><span data-stu-id="9e032-127">If the namespace is found, it reports what was found.</span></span>
3. <span data-ttu-id="9e032-128">Pokud není nalezen obor názvů, vytvoří obor názvů a pak načte nově vytvořený obor názvů.</span><span class="sxs-lookup"><span data-stu-id="9e032-128">If the namespace is not found, it creates the namespace and then retrieves the newly created namespace.</span></span>
   
    ``` powershell
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
   
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Host "The namespace $Namespace already exists in the $Location region:"
        # Report what was found
        Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "The $Namespace namespace does not exist."
        Write-Host "Creating the $Namespace namespace in the $Location region..."
        New-AzureRmServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
        Write-Host "The $Namespace namespace in Resource Group $ResGrpName in the $Location region has been successfully created."
                
    }
    ```

### <a name="create-a-namespace-authorization-rule"></a><span data-ttu-id="9e032-129">Vytvoření oboru názvů autorizační pravidla</span><span class="sxs-lookup"><span data-stu-id="9e032-129">Create a namespace authorization rule</span></span>

<span data-ttu-id="9e032-130">Následující příklad ukazuje, jak spravovat pravidla autorizace oboru názvů pomocí [New-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespaceauthorizationrule), [Get-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespaceauthorizationrule), [Set-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespaceauthorizationrule), a [rutiny Remove-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespaceauthorizationrule).</span><span class="sxs-lookup"><span data-stu-id="9e032-130">The following example shows how to manage namespace authorization rules using the [New-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespaceauthorizationrule), [Get-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespaceauthorizationrule), [Set-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespaceauthorizationrule), and [Remove-AzureRmServiceBusNamespaceAuthorizationRule cmdlets](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespaceauthorizationrule).</span></span>

```powershell
# Query to see if rule exists
$CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

# Check if the rule already exists or needs to be created
if ($CurrentRule)
{
    Write-Host "The $AuthRule rule already exists for the namespace $Namespace."
}
else
{
    Write-Host "The $AuthRule rule does not exist."
    Write-Host "Creating the $AuthRule rule for the $Namespace namespace..."
    New-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule -Rights @("Listen","Send")
    $CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
    Write-Host "The $AuthRule rule for the $Namespace namespace has been successfully created."

    Write-Host "Setting rights on the namespace"
    $authRuleObj = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

    Write-Host "Remove Send rights"
    $authRuleObj.Rights.Remove("Send")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Add Send and Manage rights to the namespace"
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

## <a name="create-a-queue"></a><span data-ttu-id="9e032-131">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="9e032-131">Create a queue</span></span>

<span data-ttu-id="9e032-132">Pokud chcete vytvořit fronta nebo téma, proveďte kontrolu oboru názvů pomocí skriptu v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="9e032-132">To create a queue or topic, perform a namespace check using the script in the previous section.</span></span> <span data-ttu-id="9e032-133">Pak vytvořte fronty:</span><span class="sxs-lookup"><span data-stu-id="9e032-133">Then, create the queue:</span></span>

```powershell
# Check if queue already exists
$CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName

if($CurrentQ)
{
    Write-Host "The queue $QueueName already exists in the $Location region:"
}
else
{
    Write-Host "The $QueueName queue does not exist."
    Write-Host "Creating the $QueueName queue in the $Location region..."
    New-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -EnablePartitioning $True
    $CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName
    Write-Host "The $QueueName queue in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

### <a name="modify-queue-properties"></a><span data-ttu-id="9e032-134">Úprava vlastností fronty.</span><span class="sxs-lookup"><span data-stu-id="9e032-134">Modify queue properties</span></span>

<span data-ttu-id="9e032-135">Po provedení skript v předchozí části, můžete použít [Set-AzureRmServiceBusQueue](/powershell/module/azurerm.servicebus/set-azurermservicebusqueue) rutiny aktualizovat vlastnosti fronty, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="9e032-135">After executing the script in the preceding section, you can use the [Set-AzureRmServiceBusQueue](/powershell/module/azurerm.servicebus/set-azurermservicebusqueue) cmdlet to update the properties of a queue, as in the following example:</span></span>

```powershell
$CurrentQ.DeadLetteringOnMessageExpiration = $True
$CurrentQ.MaxDeliveryCount = 7
$CurrentQ.MaxSizeInMegabytes = 2048
$CurrentQ.EnableExpress = $True

Set-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -QueueObj $CurrentQ
```

## <a name="provisioning-other-service-bus-entities"></a><span data-ttu-id="9e032-136">Zřizování dalšími subjekty, Service Bus</span><span class="sxs-lookup"><span data-stu-id="9e032-136">Provisioning other Service Bus entities</span></span>

<span data-ttu-id="9e032-137">Můžete použít [modulu Service Bus PowerShell](/powershell/module/azurerm.servicebus) ke zřízení jinými entitami, jako jsou témata a odběry.</span><span class="sxs-lookup"><span data-stu-id="9e032-137">You can use the [Service Bus PowerShell module](/powershell/module/azurerm.servicebus) to provision other entities, such as topics and subscriptions.</span></span> <span data-ttu-id="9e032-138">Tyto rutiny jsou syntakticky podobná rutiny vytvoření fronty ukázáno v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="9e032-138">These cmdlets are syntactically similar to the queue creation cmdlets demonstrated in the previous section.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9e032-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9e032-139">Next steps</span></span>

- <span data-ttu-id="9e032-140">Najdete kompletní dokumentaci modulu Powershellu pro Service Bus Resource Manager [zde](/powershell/module/azurerm.servicebus).</span><span class="sxs-lookup"><span data-stu-id="9e032-140">See the complete Service Bus Resource Manager PowerShell module documentation [here](/powershell/module/azurerm.servicebus).</span></span> <span data-ttu-id="9e032-141">Tato stránka obsahuje seznam všech dostupných rutin.</span><span class="sxs-lookup"><span data-stu-id="9e032-141">This page lists all available cmdlets.</span></span>
- <span data-ttu-id="9e032-142">Informace o používání šablon Azure Resource Manageru, najdete v článku [vytvořit Service Bus prostředků pomocí šablony Azure Resource Manager](service-bus-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9e032-142">For information about using Azure Resource Manager templates, see the article [Create Service Bus resources using Azure Resource Manager templates](service-bus-resource-manager-overview.md).</span></span>
- <span data-ttu-id="9e032-143">Informace o [knihovny Service Bus .NET správy](service-bus-management-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="9e032-143">Information about [Service Bus .NET management libraries](service-bus-management-libraries.md).</span></span>

<span data-ttu-id="9e032-144">Existují některé alternativní způsoby správy entit služby Service Bus, jak je popsáno v těchto příspěvcích na blogu:</span><span class="sxs-lookup"><span data-stu-id="9e032-144">There are some alternate ways to manage Service Bus entities, as described in these blog posts:</span></span>

* [<span data-ttu-id="9e032-145">Postup vytvoření fronty, témata a odběry pomocí skriptu prostředí PowerShell služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="9e032-145">How to create Service Bus queues, topics and subscriptions using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [<span data-ttu-id="9e032-146">Postup vytvoření Namespace Service Bus a centra událostí pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9e032-146">How to create a Service Bus Namespace and an Event Hub using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)
* [<span data-ttu-id="9e032-147">Skripty prostředí PowerShell služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="9e032-147">Service Bus PowerShell Scripts</span></span>](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
