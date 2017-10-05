---
title: "Azure ukázkový skript prostředí PowerShell - port otevřete aplikace nástroji pro vyrovnávání zatížení | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - otevřete port nástroji pro vyrovnávání zatížení Azure pro aplikace Service Fabric."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 2958bdef0889076249918608c04c66678fa80b97
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="open-an-application-port-in-the-azure-load-balancer"></a><span data-ttu-id="32a8f-103">Otevřete port aplikace nástroji pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="32a8f-103">Open an application port in the Azure load balancer</span></span>

<span data-ttu-id="32a8f-104">Aplikace běžící v Azure Service Fabric se nachází za nástrojem pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="32a8f-104">A Service Fabric application running in Azure sits behind the Azure load balancer.</span></span> <span data-ttu-id="32a8f-105">Tento ukázkový skript otevře port v k nástroji pro vyrovnávání zatížení Azure tak, aby aplikace Service Fabric může komunikovat s externími klienty.</span><span class="sxs-lookup"><span data-stu-id="32a8f-105">This sample script opens a port in an Azure load balancer so that a Service Fabric application can communicate with external clients.</span></span> <span data-ttu-id="32a8f-106">Podle potřeby upravte požadované parametry.</span><span class="sxs-lookup"><span data-stu-id="32a8f-106">Customize the parameters as needed.</span></span> 

<span data-ttu-id="32a8f-107">V případě potřeby nainstalujte modul Service Fabric prostředí PowerShell s [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="32a8f-107">If needed, install the Service Fabric PowerShell module with the [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="32a8f-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="32a8f-108">Sample script</span></span>

<span data-ttu-id="32a8f-109">[!code-powershell[hlavní](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "otevřít port v nástroje pro vyrovnávání zatížení")]</span><span class="sxs-lookup"><span data-stu-id="32a8f-109">[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in the load balancer")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="32a8f-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="32a8f-110">Script explanation</span></span>

<span data-ttu-id="32a8f-111">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="32a8f-111">This script uses the following commands.</span></span> <span data-ttu-id="32a8f-112">Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.</span><span class="sxs-lookup"><span data-stu-id="32a8f-112">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="32a8f-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="32a8f-113">Command</span></span> | <span data-ttu-id="32a8f-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="32a8f-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="32a8f-115">Get-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="32a8f-115">Get-AzureRmResource</span></span>](/powershell/module/azurerm.resources/get-azurermresource) | <span data-ttu-id="32a8f-116">Získá prostředek služby Azure.</span><span class="sxs-lookup"><span data-stu-id="32a8f-116">Gets an Azure resource.</span></span>  |
| [<span data-ttu-id="32a8f-117">Get-AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="32a8f-117">Get-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/get-azurermloadbalancer) | <span data-ttu-id="32a8f-118">Získá nástroje pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="32a8f-118">Gets the Azure load balancer.</span></span> |
| [<span data-ttu-id="32a8f-119">Přidat AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="32a8f-119">Add-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | <span data-ttu-id="32a8f-120">Přidá test konfigurace pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="32a8f-120">Adds a probe configuration to a load balancer.</span></span>|
| [<span data-ttu-id="32a8f-121">Get-AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="32a8f-121">Get-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | <span data-ttu-id="32a8f-122">Získá test konfigurace pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="32a8f-122">Gets a probe configuration for a load balancer.</span></span> |
| [<span data-ttu-id="32a8f-123">Přidat AzureRmLoadBalancerRuleConfig</span><span class="sxs-lookup"><span data-stu-id="32a8f-123">Add-AzureRmLoadBalancerRuleConfig</span></span>](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | <span data-ttu-id="32a8f-124">Přidá pravidlo konfigurace pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="32a8f-124">Adds a rule configuration to a load balancer.</span></span> |
| [<span data-ttu-id="32a8f-125">Set-AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="32a8f-125">Set-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/set-azurermloadbalancer) | <span data-ttu-id="32a8f-126">Nastaví cíl stavu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="32a8f-126">Sets the goal state for a load balancer.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="32a8f-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="32a8f-127">Next steps</span></span>

<span data-ttu-id="32a8f-128">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="32a8f-128">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="32a8f-129">Další ukázky pro Azure Service Fabric Powershell najdete v [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="32a8f-129">Additional Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
