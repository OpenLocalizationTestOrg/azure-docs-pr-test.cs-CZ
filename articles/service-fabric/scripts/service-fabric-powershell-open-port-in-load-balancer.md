---
title: "aaaAzure ukázka skriptu prostředí PowerShell - port otevřete aplikace nástroji pro vyrovnávání zatížení | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - otevřít port v hello Vyrovnávání zatížení Azure pro aplikace Service Fabric."
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
ms.openlocfilehash: 6acb28942851dce63f89f7de362b7cf1dc7b1fee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="open-an-application-port-in-hello-azure-load-balancer"></a><span data-ttu-id="f8e57-103">Otevřete port aplikace nástroji pro vyrovnávání zatížení Azure hello</span><span class="sxs-lookup"><span data-stu-id="f8e57-103">Open an application port in hello Azure load balancer</span></span>

<span data-ttu-id="f8e57-104">Aplikace běžící v Azure Service Fabric se nachází za nástrojem pro vyrovnávání zatížení Azure hello.</span><span class="sxs-lookup"><span data-stu-id="f8e57-104">A Service Fabric application running in Azure sits behind hello Azure load balancer.</span></span> <span data-ttu-id="f8e57-105">Tento ukázkový skript otevře port v k nástroji pro vyrovnávání zatížení Azure tak, aby aplikace Service Fabric může komunikovat s externími klienty.</span><span class="sxs-lookup"><span data-stu-id="f8e57-105">This sample script opens a port in an Azure load balancer so that a Service Fabric application can communicate with external clients.</span></span> <span data-ttu-id="f8e57-106">Parametry hello přizpůsobte podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="f8e57-106">Customize hello parameters as needed.</span></span> 

<span data-ttu-id="f8e57-107">V případě potřeby nainstalujte modul Service Fabric prostředí PowerShell hello s hello [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f8e57-107">If needed, install hello Service Fabric PowerShell module with hello [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="f8e57-108">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="f8e57-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in hello load balancer")]

## <a name="script-explanation"></a><span data-ttu-id="f8e57-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="f8e57-109">Script explanation</span></span>

<span data-ttu-id="f8e57-110">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="f8e57-110">This script uses hello following commands.</span></span> <span data-ttu-id="f8e57-111">Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.</span><span class="sxs-lookup"><span data-stu-id="f8e57-111">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="f8e57-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f8e57-112">Command</span></span> | <span data-ttu-id="f8e57-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f8e57-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f8e57-114">Get-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="f8e57-114">Get-AzureRmResource</span></span>](/powershell/module/azurerm.resources/get-azurermresource) | <span data-ttu-id="f8e57-115">Získá prostředek služby Azure.</span><span class="sxs-lookup"><span data-stu-id="f8e57-115">Gets an Azure resource.</span></span>  |
| [<span data-ttu-id="f8e57-116">Get-AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="f8e57-116">Get-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/get-azurermloadbalancer) | <span data-ttu-id="f8e57-117">Získá pro vyrovnávání zatížení Azure hello.</span><span class="sxs-lookup"><span data-stu-id="f8e57-117">Gets hello Azure load balancer.</span></span> |
| [<span data-ttu-id="f8e57-118">Přidat AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="f8e57-118">Add-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | <span data-ttu-id="f8e57-119">Přidá Vyrovnávání zatížení tooa Konfigurace testu.</span><span class="sxs-lookup"><span data-stu-id="f8e57-119">Adds a probe configuration tooa load balancer.</span></span>|
| [<span data-ttu-id="f8e57-120">Get-AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="f8e57-120">Get-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | <span data-ttu-id="f8e57-121">Získá test konfigurace pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="f8e57-121">Gets a probe configuration for a load balancer.</span></span> |
| [<span data-ttu-id="f8e57-122">Přidat AzureRmLoadBalancerRuleConfig</span><span class="sxs-lookup"><span data-stu-id="f8e57-122">Add-AzureRmLoadBalancerRuleConfig</span></span>](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | <span data-ttu-id="f8e57-123">Přidá pravidlo Vyrovnávání zatížení tooa konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f8e57-123">Adds a rule configuration tooa load balancer.</span></span> |
| [<span data-ttu-id="f8e57-124">Set-AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="f8e57-124">Set-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/set-azurermloadbalancer) | <span data-ttu-id="f8e57-125">Nastaví hello cíle stavu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="f8e57-125">Sets hello goal state for a load balancer.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f8e57-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f8e57-126">Next steps</span></span>

<span data-ttu-id="f8e57-127">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f8e57-127">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="f8e57-128">Další ukázky pro Azure Service Fabric Powershell naleznete v hello [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f8e57-128">Additional Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
