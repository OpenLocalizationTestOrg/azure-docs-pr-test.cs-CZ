---
title: "Vytvoření instance sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje kroky k vytvoření instance sledovací proces sítě pomocí portálu a REST API služby Azure"
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
ms.openlocfilehash: 2aeaffdd5ab552e18677cbd1a24a748dd14bf172
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-network-watcher-instance"></a><span data-ttu-id="ee15e-103">Vytvoření instance sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="ee15e-103">Create an Azure Network Watcher instance</span></span>

<span data-ttu-id="ee15e-104">Sledovací proces sítě je místní služba, která umožňuje sledovat a diagnostikovat podmínky na úrovni scénář sítě, do a z Azure.</span><span class="sxs-lookup"><span data-stu-id="ee15e-104">Network Watcher is a regional service that enables you to monitor and diagnose conditions at a network scenario level in, to, and from Azure.</span></span> <span data-ttu-id="ee15e-105">Scénář úrovně monitorování umožňuje diagnostikovat problémy na úrovni zobrazení koncová sítě.</span><span class="sxs-lookup"><span data-stu-id="ee15e-105">Scenario level monitoring enables you to diagnose problems at an end to end network level view.</span></span> <span data-ttu-id="ee15e-106">Diagnostika sítě a k dispozici sledovací proces sítě vizualizace nástroje vám pomůžou pochopit, diagnostikovat a získáte přehled o k síti v Azure.</span><span class="sxs-lookup"><span data-stu-id="ee15e-106">Network diagnostic and visualization tools available with Network Watcher help you understand, diagnose, and gain insights to your network in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="ee15e-107">Sledovací proces sítě aktuálně podporuje pouze rozhraní příkazového řádku 1.0, pokyny pro vytvoření nové instance sledovací proces sítě se poskytuje pro rozhraní příkazového řádku 1.0.</span><span class="sxs-lookup"><span data-stu-id="ee15e-107">As Network Watcher currently only supports CLI 1.0, the instructions to create a new Network Watcher instance is provided for CLI 1.0.</span></span>

## <a name="create-a-network-watcher-in-the-portal"></a><span data-ttu-id="ee15e-108">Vytvoření sledovací proces sítě v portálu.</span><span class="sxs-lookup"><span data-stu-id="ee15e-108">Create a Network Watcher in the portal</span></span>

<span data-ttu-id="ee15e-109">Přejděte na **další služby** > **sítě** > **sítě sledovacích procesů**.</span><span class="sxs-lookup"><span data-stu-id="ee15e-109">Navigate to **More Services** > **Networking** > **Network Watcher**.</span></span> <span data-ttu-id="ee15e-110">Můžete vybrat všechny odběry, které chcete povolit sledovací proces sítě pro.</span><span class="sxs-lookup"><span data-stu-id="ee15e-110">You can select all the subscriptions you want to enable Network Watcher for.</span></span> <span data-ttu-id="ee15e-111">Tato akce vytvoří sledovací proces sítě v každé oblasti, která je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ee15e-111">This action creates a Network Watcher in every region that is available.</span></span>

![Vytvoření sledovací proces sítě][1]

<span data-ttu-id="ee15e-113">Když povolíte sledovací proces sítě pomocí portálu, název instance sledovací proces sítě automaticky nastaví NetworkWatcher_region_name kde region_name odpovídá oblasti Azure, podporou instance.</span><span class="sxs-lookup"><span data-stu-id="ee15e-113">When you enable Network Watcher using the Portal, the name of the Network Watcher instance will automatically be set to NetworkWatcher_region_name where region_name corresponds to the Azure Region where the instance was enabled.</span></span>  <span data-ttu-id="ee15e-114">Například sledovací proces sítě povolené v oblasti západní centrální USA bude mít název NetworkWatcher_westcentralus</span><span class="sxs-lookup"><span data-stu-id="ee15e-114">For example, a Network Watcher enabled in West Central US region will be named NetworkWatcher_westcentralus</span></span>

<span data-ttu-id="ee15e-115">Kromě toho instance sledovací proces sítě se automaticky přidá do skupinu prostředků s názvem NetworkWatcherRG.</span><span class="sxs-lookup"><span data-stu-id="ee15e-115">Additionally, the Network Watcher instance will automatically be added into a Resource Group called NetworkWatcherRG.</span></span>  <span data-ttu-id="ee15e-116">Tato skupina prostředků bude vytvořen, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="ee15e-116">This Resource Group will be created if it does not already exist.</span></span>

<span data-ttu-id="ee15e-117">Pokud chcete přizpůsobit název instance sledovací proces sítě a skupina prostředků je umístěn do, můžete použít prostředí Powershell, REST API nebo ARMClient metod popsaných níže.</span><span class="sxs-lookup"><span data-stu-id="ee15e-117">If you wish to customize the name of a Network Watcher instance and the Resource Group it's placed into, you can use Powershell, the REST API, or ARMClient methods described below.</span></span>  <span data-ttu-id="ee15e-118">V každé možnosti musí existovat skupině prostředků než umístíte sledovací proces sítě do ní.</span><span class="sxs-lookup"><span data-stu-id="ee15e-118">In each option, the Resource Group must exist before you place the Network Watcher into it.</span></span>  

## <a name="create-a-network-watcher-with-powershell"></a><span data-ttu-id="ee15e-119">Vytvoření sledovací proces sítě pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ee15e-119">Create a Network Watcher with PowerShell</span></span>

<span data-ttu-id="ee15e-120">Chcete-li vytvořit instanci sledovací proces sítě, spusťte v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="ee15e-120">To create an instance of Network Watcher, run the following example:</span></span>

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-rest-api"></a><span data-ttu-id="ee15e-121">Vytvoření sledovací proces sítě pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="ee15e-121">Create a Network Watcher with the REST API</span></span>

<span data-ttu-id="ee15e-122">ARMclient se používá k volání rozhraní REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ee15e-122">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="ee15e-123">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="ee15e-123">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

### <a name="log-in-with-armclient"></a><span data-ttu-id="ee15e-124">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="ee15e-124">Log in with ARMClient</span></span>

```powerShell
armclient login
```

### <a name="create-the-network-watcher"></a><span data-ttu-id="ee15e-125">Vytvoření sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="ee15e-125">Create the network watcher</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="ee15e-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ee15e-126">Next steps</span></span>

<span data-ttu-id="ee15e-127">Teď, když máte instanci sledovací proces sítě, informace o funkcích, které jsou k dispozici:</span><span class="sxs-lookup"><span data-stu-id="ee15e-127">Now that you have an instance of Network Watcher, learn about the features available:</span></span>

* [<span data-ttu-id="ee15e-128">Topologie</span><span class="sxs-lookup"><span data-stu-id="ee15e-128">Topology</span></span>](network-watcher-topology-overview.md)
* [<span data-ttu-id="ee15e-129">Zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="ee15e-129">Packet capture</span></span>](network-watcher-packet-capture-overview.md)
* [<span data-ttu-id="ee15e-130">Ověření IP toku</span><span class="sxs-lookup"><span data-stu-id="ee15e-130">IP flow verify</span></span>](network-watcher-ip-flow-verify-overview.md)
* [<span data-ttu-id="ee15e-131">Další směrování</span><span class="sxs-lookup"><span data-stu-id="ee15e-131">Next hop</span></span>](network-watcher-next-hop-overview.md)
* [<span data-ttu-id="ee15e-132">Zobrazení skupin zabezpečení</span><span class="sxs-lookup"><span data-stu-id="ee15e-132">Security group view</span></span>](network-watcher-security-group-view-overview.md)
* [<span data-ttu-id="ee15e-133">Protokolování toku NSG</span><span class="sxs-lookup"><span data-stu-id="ee15e-133">NSG flow logging</span></span>](network-watcher-nsg-flow-logging-overview.md)
* [<span data-ttu-id="ee15e-134">Řešení potíží virtuální síťová brána</span><span class="sxs-lookup"><span data-stu-id="ee15e-134">Virtual Network Gateway troubleshooting</span></span>](network-watcher-troubleshoot-overview.md)

<span data-ttu-id="ee15e-135">Po vytvoření instance sledovací proces sítě zachycení balíčku lze nastavit pomocí následujících článek: [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="ee15e-135">Once a Network Watcher instance has been created, package capture can be configured by following the article: [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

[1]: ./media/network-watcher-create/figure1.png











