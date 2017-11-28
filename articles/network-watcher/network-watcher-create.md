---
title: "aaaCreate instance sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje kroky toocreate hello instanci sledovací proces sítě pomocí portálu pro hello a REST API služby Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 90d4f90c9709a80e4b27863e79e5b6e16de145c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-network-watcher-instance"></a><span data-ttu-id="00822-103">Vytvoření instance sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="00822-103">Create an Azure Network Watcher instance</span></span>

<span data-ttu-id="00822-104">Sledovací proces sítě je místní služba, která umožňuje vám toomonitor a diagnostikovat podmínky na úrovni scénář sítě, do a z Azure.</span><span class="sxs-lookup"><span data-stu-id="00822-104">Network Watcher is a regional service that enables you toomonitor and diagnose conditions at a network scenario level in, to, and from Azure.</span></span> <span data-ttu-id="00822-105">Scénář úrovně monitorování umožňuje toodiagnose problémy v síti tooend koncové pro úroveň zobrazení.</span><span class="sxs-lookup"><span data-stu-id="00822-105">Scenario level monitoring enables you toodiagnose problems at an end tooend network level view.</span></span> <span data-ttu-id="00822-106">Diagnostika sítě a k dispozici sledovací proces sítě vizualizace nástroje vám pomůžou pochopit, diagnostikovat a získat statistiky tooyour sítě v Azure.</span><span class="sxs-lookup"><span data-stu-id="00822-106">Network diagnostic and visualization tools available with Network Watcher help you understand, diagnose, and gain insights tooyour network in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="00822-107">Sledovací proces sítě aktuálně podporuje pouze rozhraní příkazového řádku 1.0, hello pokyny toocreate novou instanci sledovací proces sítě se poskytuje pro rozhraní příkazového řádku 1.0.</span><span class="sxs-lookup"><span data-stu-id="00822-107">As Network Watcher currently only supports CLI 1.0, hello instructions toocreate a new Network Watcher instance is provided for CLI 1.0.</span></span>

## <a name="create-a-network-watcher-in-hello-portal"></a><span data-ttu-id="00822-108">Vytvoření sledovací proces sítě hello portálu</span><span class="sxs-lookup"><span data-stu-id="00822-108">Create a Network Watcher in hello portal</span></span>

<span data-ttu-id="00822-109">Přejděte příliš**více služeb** > **sítě** > **sledovací proces sítě**.</span><span class="sxs-lookup"><span data-stu-id="00822-109">Navigate too**More Services** > **Networking** > **Network Watcher**.</span></span> <span data-ttu-id="00822-110">Můžete vybrat všechny odběry hello chcete tooenable sledovací proces sítě pro.</span><span class="sxs-lookup"><span data-stu-id="00822-110">You can select all hello subscriptions you want tooenable Network Watcher for.</span></span> <span data-ttu-id="00822-111">Tato akce vytvoří sledovací proces sítě v každé oblasti, která je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="00822-111">This action creates a Network Watcher in every region that is available.</span></span>

![Vytvoření sledovací proces sítě][1]

<span data-ttu-id="00822-113">Když povolíte sledovací proces sítě pomocí hello portál, název hello instance hello sledovací proces sítě se automaticky nastaví tooNetworkWatcher_region_name kde region_name odpovídá toohello oblast Azure, podporou hello instance.</span><span class="sxs-lookup"><span data-stu-id="00822-113">When you enable Network Watcher using hello Portal, hello name of hello Network Watcher instance will automatically be set tooNetworkWatcher_region_name where region_name corresponds toohello Azure Region where hello instance was enabled.</span></span>  <span data-ttu-id="00822-114">Například sledovací proces sítě povolené v oblasti západní centrální USA bude mít název NetworkWatcher_westcentralus</span><span class="sxs-lookup"><span data-stu-id="00822-114">For example, a Network Watcher enabled in West Central US region will be named NetworkWatcher_westcentralus</span></span>

<span data-ttu-id="00822-115">Kromě toho instance hello sledovací proces sítě se automaticky přidá do skupinu prostředků s názvem NetworkWatcherRG.</span><span class="sxs-lookup"><span data-stu-id="00822-115">Additionally, hello Network Watcher instance will automatically be added into a Resource Group called NetworkWatcherRG.</span></span>  <span data-ttu-id="00822-116">Tato skupina prostředků bude vytvořen, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="00822-116">This Resource Group will be created if it does not already exist.</span></span>

<span data-ttu-id="00822-117">Pokud chcete toocustomize hello název instance sledovací proces sítě a hello skupina prostředků je umístěn do, můžete použít prostředí Powershell, hello REST API nebo ARMClient metod popsaných níže.</span><span class="sxs-lookup"><span data-stu-id="00822-117">If you wish toocustomize hello name of a Network Watcher instance and hello Resource Group it's placed into, you can use Powershell, hello REST API, or ARMClient methods described below.</span></span>  <span data-ttu-id="00822-118">V každé možnosti musí existovat hello skupiny prostředků než umístíte hello sledovací proces sítě do ní.</span><span class="sxs-lookup"><span data-stu-id="00822-118">In each option, hello Resource Group must exist before you place hello Network Watcher into it.</span></span>  

## <a name="create-a-network-watcher-with-powershell"></a><span data-ttu-id="00822-119">Vytvoření sledovací proces sítě pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="00822-119">Create a Network Watcher with PowerShell</span></span>

<span data-ttu-id="00822-120">toocreate instanci sledovací proces sítě, spusťte hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="00822-120">toocreate an instance of Network Watcher, run hello following example:</span></span>

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-hello-rest-api"></a><span data-ttu-id="00822-121">Vytvoření sledovací proces sítě s hello REST API</span><span class="sxs-lookup"><span data-stu-id="00822-121">Create a Network Watcher with hello REST API</span></span>

<span data-ttu-id="00822-122">ARMclient je použité toocall hello REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="00822-122">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="00822-123">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="00822-123">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

### <a name="log-in-with-armclient"></a><span data-ttu-id="00822-124">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="00822-124">Log in with ARMClient</span></span>

```powerShell
armclient login
```

### <a name="create-hello-network-watcher"></a><span data-ttu-id="00822-125">Vytvoření sledovací proces sítě hello</span><span class="sxs-lookup"><span data-stu-id="00822-125">Create hello network watcher</span></span>

```powershell
$subscriptionId = '<subscription id>'
$networkWatcherName = '<name of network watcher>'
$resourceGroupName = '<resource group name>'
$apiversion = "2016-09-01"
$requestBody = @"
{
'location': 'West Central US'
}
"@

armclient put "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}?api-version=${api-version}" $requestBody
```

## <a name="next-steps"></a><span data-ttu-id="00822-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="00822-126">Next steps</span></span>

<span data-ttu-id="00822-127">Teď, když máte instanci sledovací proces sítě, další informace o hello funkce, které jsou k dispozici:</span><span class="sxs-lookup"><span data-stu-id="00822-127">Now that you have an instance of Network Watcher, learn about hello features available:</span></span>

* [<span data-ttu-id="00822-128">Topologie</span><span class="sxs-lookup"><span data-stu-id="00822-128">Topology</span></span>](network-watcher-topology-overview.md)
* [<span data-ttu-id="00822-129">Zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="00822-129">Packet capture</span></span>](network-watcher-packet-capture-overview.md)
* [<span data-ttu-id="00822-130">Ověření IP toku</span><span class="sxs-lookup"><span data-stu-id="00822-130">IP flow verify</span></span>](network-watcher-ip-flow-verify-overview.md)
* [<span data-ttu-id="00822-131">Další směrování</span><span class="sxs-lookup"><span data-stu-id="00822-131">Next hop</span></span>](network-watcher-next-hop-overview.md)
* [<span data-ttu-id="00822-132">Zobrazení skupin zabezpečení</span><span class="sxs-lookup"><span data-stu-id="00822-132">Security group view</span></span>](network-watcher-security-group-view-overview.md)
* [<span data-ttu-id="00822-133">Protokolování toku NSG</span><span class="sxs-lookup"><span data-stu-id="00822-133">NSG flow logging</span></span>](network-watcher-nsg-flow-logging-overview.md)
* [<span data-ttu-id="00822-134">Řešení potíží virtuální síťová brána</span><span class="sxs-lookup"><span data-stu-id="00822-134">Virtual Network Gateway troubleshooting</span></span>](network-watcher-troubleshoot-overview.md)

<span data-ttu-id="00822-135">Po vytvoření instance sledovací proces sítě zachycení balíčku můžete nakonfigurovat taky v následujícím článku hello: [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="00822-135">Once a Network Watcher instance has been created, package capture can be configured by following hello article: [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

[1]: ./media/network-watcher-create/figure1.png











