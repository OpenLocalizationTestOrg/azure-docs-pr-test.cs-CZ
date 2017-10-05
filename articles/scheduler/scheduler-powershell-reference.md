---
title: Reference rutin Powershellu pro Scheduler
description: Reference rutin Powershellu pro Scheduler
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 9a26c457-d7a1-4e4a-bc79-f26592155218
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 141919ab4506b3de4c4a69670dcf54c60ee6409c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scheduler-powershell-cmdlets-reference"></a><span data-ttu-id="72bfc-103">Reference rutin Powershellu pro Scheduler</span><span class="sxs-lookup"><span data-stu-id="72bfc-103">Scheduler PowerShell Cmdlets Reference</span></span>
<span data-ttu-id="72bfc-104">Následující tabulka popisuje a obsahuje odkazy na stránce odkaz každé hlavní rutiny ve službě Azure Scheduler.</span><span class="sxs-lookup"><span data-stu-id="72bfc-104">The following table describes and links to the reference page of each of the major cmdlets in Azure Scheduler.</span></span>

<span data-ttu-id="72bfc-105">Chcete-li nainstalovat Azure PowerShell a přidružit ho ke svému předplatnému Azure, prohlédněte si téma [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="72bfc-105">To install Azure PowerShell and associate it with your Azure subscription, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> 

<span data-ttu-id="72bfc-106">Další informace o [rutiny Azure Resource Manager](/powershell/azure/overview), najdete v části [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="72bfc-106">For more information about [Azure Resource Manager cmdlets](/powershell/azure/overview), see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

| <span data-ttu-id="72bfc-107">Rutina</span><span class="sxs-lookup"><span data-stu-id="72bfc-107">Cmdlet</span></span> | <span data-ttu-id="72bfc-108">Popis rutiny</span><span class="sxs-lookup"><span data-stu-id="72bfc-108">Cmdlet Description</span></span> |
| --- | --- |
| [<span data-ttu-id="72bfc-109">Zakázat AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="72bfc-109">Disable-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |<span data-ttu-id="72bfc-110">Zakáže kolekce úloh.</span><span class="sxs-lookup"><span data-stu-id="72bfc-110">Disables a job collection.</span></span> |
| [<span data-ttu-id="72bfc-111">Povolit AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="72bfc-111">Enable-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |<span data-ttu-id="72bfc-112">Umožňuje kolekce úloh.</span><span class="sxs-lookup"><span data-stu-id="72bfc-112">Enables a job collection.</span></span> |
| [<span data-ttu-id="72bfc-113">Get-AzureRmSchedulerJob</span><span class="sxs-lookup"><span data-stu-id="72bfc-113">Get-AzureRmSchedulerJob</span></span>](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |<span data-ttu-id="72bfc-114">Získá plánovače úloh.</span><span class="sxs-lookup"><span data-stu-id="72bfc-114">Gets Scheduler jobs.</span></span> |
| [<span data-ttu-id="72bfc-115">Get-AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="72bfc-115">Get-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |<span data-ttu-id="72bfc-116">Získá kolekce úloh.</span><span class="sxs-lookup"><span data-stu-id="72bfc-116">Gets job collections.</span></span> |
| [<span data-ttu-id="72bfc-117">Get-AzureRmSchedulerJobHistory</span><span class="sxs-lookup"><span data-stu-id="72bfc-117">Get-AzureRmSchedulerJobHistory</span></span>](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |<span data-ttu-id="72bfc-118">Získá historie úlohy.</span><span class="sxs-lookup"><span data-stu-id="72bfc-118">Gets job history.</span></span> |
| [<span data-ttu-id="72bfc-119">Nové AzureRmSchedulerHttpJob</span><span class="sxs-lookup"><span data-stu-id="72bfc-119">New-AzureRmSchedulerHttpJob</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |<span data-ttu-id="72bfc-120">Vytvoří úlohu HTTP.</span><span class="sxs-lookup"><span data-stu-id="72bfc-120">Creates an HTTP job.</span></span> |
| [<span data-ttu-id="72bfc-121">Nové AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="72bfc-121">New-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |<span data-ttu-id="72bfc-122">Vytvoří kolekci úloh.</span><span class="sxs-lookup"><span data-stu-id="72bfc-122">Creates a job collection.</span></span> |
| [<span data-ttu-id="72bfc-123">Nové AzureRmSchedulerServiceBusQueueJob</span><span class="sxs-lookup"><span data-stu-id="72bfc-123">New-AzureRmSchedulerServiceBusQueueJob</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) |<span data-ttu-id="72bfc-124">Vytvoří úlohu fronty service bus.</span><span class="sxs-lookup"><span data-stu-id="72bfc-124">Creates a service bus queue job.</span></span> |
| [<span data-ttu-id="72bfc-125">Nové AzureRmSchedulerServiceBusTopicJob</span><span class="sxs-lookup"><span data-stu-id="72bfc-125">New-AzureRmSchedulerServiceBusTopicJob</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |<span data-ttu-id="72bfc-126">Vytvoří úlohu tématu service bus.</span><span class="sxs-lookup"><span data-stu-id="72bfc-126">Creates a service bus topic job.</span></span> |
| [<span data-ttu-id="72bfc-127">Nové AzureRmSchedulerStorageQueueJob</span><span class="sxs-lookup"><span data-stu-id="72bfc-127">New-AzureRmSchedulerStorageQueueJob</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |<span data-ttu-id="72bfc-128">Vytvoří úlohu fronty úložiště.</span><span class="sxs-lookup"><span data-stu-id="72bfc-128">Creates a storage queue job.</span></span> |
| [<span data-ttu-id="72bfc-129">Odebrat AzureRmSchedulerJob</span><span class="sxs-lookup"><span data-stu-id="72bfc-129">Remove-AzureRmSchedulerJob</span></span>](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |<span data-ttu-id="72bfc-130">Odebere plánovače úloh.</span><span class="sxs-lookup"><span data-stu-id="72bfc-130">Removes a Scheduler job.</span></span> |
| [<span data-ttu-id="72bfc-131">Odebrat AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="72bfc-131">Remove-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |<span data-ttu-id="72bfc-132">Odebere kolekce úloh.</span><span class="sxs-lookup"><span data-stu-id="72bfc-132">Removes a job collection.</span></span> |
| [<span data-ttu-id="72bfc-133">Set-AzureRmSchedulerHttpJob</span><span class="sxs-lookup"><span data-stu-id="72bfc-133">Set-AzureRmSchedulerHttpJob</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |<span data-ttu-id="72bfc-134">Upravuje HTTP plánovače úloh.</span><span class="sxs-lookup"><span data-stu-id="72bfc-134">Modifies a Scheduler HTTP job.</span></span> |
| [<span data-ttu-id="72bfc-135">Set-AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="72bfc-135">Set-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |<span data-ttu-id="72bfc-136">Upravuje kolekce úloh.</span><span class="sxs-lookup"><span data-stu-id="72bfc-136">Modifies a job collection.</span></span> |
| [<span data-ttu-id="72bfc-137">Set-AzureRmSchedulerServiceBusQueueJob</span><span class="sxs-lookup"><span data-stu-id="72bfc-137">Set-AzureRmSchedulerServiceBusQueueJob</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |<span data-ttu-id="72bfc-138">Upravuje úlohy fronty sběrnice služby.</span><span class="sxs-lookup"><span data-stu-id="72bfc-138">Modifies a service bus queue job.</span></span> |
| [<span data-ttu-id="72bfc-139">Set-AzureRmSchedulerServiceBusTopicJob</span><span class="sxs-lookup"><span data-stu-id="72bfc-139">Set-AzureRmSchedulerServiceBusTopicJob</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |<span data-ttu-id="72bfc-140">Upravuje úlohu tématu service bus.</span><span class="sxs-lookup"><span data-stu-id="72bfc-140">Modifies a service bus topic job.</span></span> |
| [<span data-ttu-id="72bfc-141">Set-AzureRmSchedulerStorageQueueJob</span><span class="sxs-lookup"><span data-stu-id="72bfc-141">Set-AzureRmSchedulerStorageQueueJob</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |<span data-ttu-id="72bfc-142">Upravuje úlohy fronty úložiště.</span><span class="sxs-lookup"><span data-stu-id="72bfc-142">Modifies a storage queue job.</span></span> |

<span data-ttu-id="72bfc-143">Podrobnější informace můžete spustit některý z následujících rutin:</span><span class="sxs-lookup"><span data-stu-id="72bfc-143">For more detailed information, you can run any of the following cmdlets:</span></span> 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a><span data-ttu-id="72bfc-144">Viz také</span><span class="sxs-lookup"><span data-stu-id="72bfc-144">See Also</span></span>
 [<span data-ttu-id="72bfc-145">Co je Scheduler?</span><span class="sxs-lookup"><span data-stu-id="72bfc-145">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="72bfc-146">Koncepty, terminologie a hierarchie entit Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="72bfc-146">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="72bfc-147">Úvod do používání Scheduleru na portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="72bfc-147">Get started using Scheduler in the Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="72bfc-148">Plány a fakturace v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="72bfc-148">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="72bfc-149">REST API Azure Scheduleru – referenční informace</span><span class="sxs-lookup"><span data-stu-id="72bfc-149">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="72bfc-150">Vysoká dostupnost a spolehlivost Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="72bfc-150">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="72bfc-151">Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="72bfc-151">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="72bfc-152">Odchozí ověření Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="72bfc-152">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

