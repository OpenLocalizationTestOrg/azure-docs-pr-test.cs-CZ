---
title: "Další směrování aaaFind s Azure sítě sledovacích procesů další segment - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak zjistíte, jaké hello dalšího směrování typ je a pomocí ip adresa dalšího směrování pomocí rozhraní příkazového řádku Azure."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 0700c274-3e0d-4dca-acfa-3ceac8990613
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 54124c051021413695d70ba93c370605abc6ebbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-azure-cli-10"></a><span data-ttu-id="1ba5b-103">Zjistěte, jaký typ hello dalšího směrování je díky funkci další segment hello v sledovací proces sítě Azure pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="1ba5b-103">Find out what hello next hop type is using hello Next Hop capability in Azure Network Watcher using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="1ba5b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1ba5b-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="1ba5b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1ba5b-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="1ba5b-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="1ba5b-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="1ba5b-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="1ba5b-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="1ba5b-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="1ba5b-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="1ba5b-109">Další směrování je funkce sledovací proces sítě, která poskytuje možnost hello získat hello typ dalšího směrování a IP adresu podle zadaný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-109">Next hop is a feature of Network Watcher that provides hello ability get hello next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="1ba5b-110">Tato funkce je užitečná při určování, zda je brána, internet nebo cílové tooits tooget virtuální sítě prochází přes odchozího provozu z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks tooget tooits destination.</span></span>

<span data-ttu-id="1ba5b-111">Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-111">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="1ba5b-112">Než začnete</span><span class="sxs-lookup"><span data-stu-id="1ba5b-112">Before you begin</span></span>

<span data-ttu-id="1ba5b-113">V tomto scénáři použijete hello rozhraní příkazového řádku Azure toofind hello typ dalšího směrování a IP adresu.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-113">In this scenario, you will use hello Azure CLI toofind hello next hop type and IP address.</span></span>

<span data-ttu-id="1ba5b-114">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="1ba5b-115">scénář Hello také předpokládá, že skupina prostředků se platný virtuální počítač existuje toobe použít.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-115">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="1ba5b-116">Scénář</span><span class="sxs-lookup"><span data-stu-id="1ba5b-116">Scenario</span></span>

<span data-ttu-id="1ba5b-117">scénář Hello popsaná v tomto článku používá další směrování, funkce sledovací proces sítě, který vyhledá hello typ dalšího směrování a IP adresu pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-117">hello scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out hello next hop type and IP address for a resource.</span></span> <span data-ttu-id="1ba5b-118">toolearn Další informace o další směrování, navštivte [další směrování přehled](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1ba5b-118">toolearn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>


## <a name="get-next-hop"></a><span data-ttu-id="1ba5b-119">Získat další směrování</span><span class="sxs-lookup"><span data-stu-id="1ba5b-119">Get Next Hop</span></span>

<span data-ttu-id="1ba5b-120">tooget hello další segment říkáme hello `azure netowrk watcher next-hop` rutiny.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-120">tooget hello next hop we call hello `azure netowrk watcher next-hop` cmdlet.</span></span> <span data-ttu-id="1ba5b-121">Jsme předat skupinu prostředků hello rutiny hello sledovací proces sítě, hello NetworkWatcher, virtuálního počítače Id, zdrojové IP adresy a cílové IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-121">We pass hello cmdlet hello Network Watcher resource group, hello NetworkWatcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="1ba5b-122">V tomto příkladu je hello cílové IP adresy tooa virtuálních počítačů v jiné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-122">In this example, hello destination IP address is tooa VM in another virtual network.</span></span> <span data-ttu-id="1ba5b-123">Mezi hello dvě virtuální sítě je bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-123">There is a virtual network gateway between hello two virtual networks.</span></span> 

```azurecli
azure network watcher next-hop -g resourceGroupName -n networkWatcherName -t targetResourceId -a <source-ip> -d <destination-ip>
```

> [!NOTE]
<span data-ttu-id="1ba5b-124">Pokud hello virtuální počítač má několik síťových adaptérů a předávání IP je povolena na žádném z hello síťové adaptéry, pak hello parametr síťový adaptér (-i síťový adaptér id) musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-124">If hello VM has multiple NICs and IP forwarding is enabled on any of hello NICs, then hello NIC parameter (-i nic-id) must be specified.</span></span> <span data-ttu-id="1ba5b-125">V opačném případě je volitelný.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-125">Otherwise it is optional.</span></span>

## <a name="review-results"></a><span data-ttu-id="1ba5b-126">Zkontrolujte výsledky</span><span class="sxs-lookup"><span data-stu-id="1ba5b-126">Review results</span></span>

<span data-ttu-id="1ba5b-127">Po dokončení, jsou uvedené výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-127">When complete, hello results are provided.</span></span> <span data-ttu-id="1ba5b-128">IP adresa dalšího směrování Hello je i hello typ prostředku, který je vrácen.</span><span class="sxs-lookup"><span data-stu-id="1ba5b-128">hello next hop IP address is returned as well as hello type of resource it is.</span></span>

```
data:    Next Hop Ip Address             : 10.0.1.2
info:    network watcher next-hop command OK
```

<span data-ttu-id="1ba5b-129">Hello následující seznam obsahuje hello aktuálně k dispozici NextHopType hodnoty:</span><span class="sxs-lookup"><span data-stu-id="1ba5b-129">hello following list shows hello currently available NextHopType values:</span></span>

<span data-ttu-id="1ba5b-130">**Typ dalšího směrování**</span><span class="sxs-lookup"><span data-stu-id="1ba5b-130">**Next Hop Type**</span></span>

* <span data-ttu-id="1ba5b-131">Internet</span><span class="sxs-lookup"><span data-stu-id="1ba5b-131">Internet</span></span>
* <span data-ttu-id="1ba5b-132">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="1ba5b-132">VirtualAppliance</span></span>
* <span data-ttu-id="1ba5b-133">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="1ba5b-133">VirtualNetworkGateway</span></span>
* <span data-ttu-id="1ba5b-134">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="1ba5b-134">VnetLocal</span></span>
* <span data-ttu-id="1ba5b-135">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="1ba5b-135">HyperNetGateway</span></span>
* <span data-ttu-id="1ba5b-136">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="1ba5b-136">VnetPeering</span></span>
* <span data-ttu-id="1ba5b-137">Žádný</span><span class="sxs-lookup"><span data-stu-id="1ba5b-137">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ba5b-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1ba5b-138">Next steps</span></span>

<span data-ttu-id="1ba5b-139">Zjistěte, jak tooreview nastavení skupiny zabezpečení sítě prostřednictvím kódu programu přechodem [NSG auditování s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="1ba5b-139">Learn how tooreview your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>
