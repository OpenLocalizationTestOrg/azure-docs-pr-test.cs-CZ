---
title: "Najít další segment s Azure sítě sledovacích procesů další segment – prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje, jak můžete najít, co je typ dalšího směrování a ip adresu pomocí dalšího přechodu pomocí prostředí PowerShell."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 6a656c55-17bd-40f1-905d-90659087639c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 00161e7c6fb4becdb7d8eab266fa27128e50f8ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-powershell"></a><span data-ttu-id="74bae-103">Zjistěte, co typ dalšího směrování je pomocí funkce další směrování v sledovací proces sítě Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="74bae-103">Find out what the next hop type is using the Next Hop capability in Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="74bae-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="74bae-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="74bae-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="74bae-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="74bae-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="74bae-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="74bae-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="74bae-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="74bae-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="74bae-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="74bae-109">Další směrování je funkce sledovací proces sítě, která poskytuje možnost get typ dalšího směrování a IP adresy, které jsou založené na zadaný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="74bae-109">Next hop is a feature of Network Watcher that provides the ability get the next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="74bae-110">Tato funkce je užitečná při určování, zda prochází odchozího provozu z virtuálního počítače brány, internet nebo virtuální sítě získat do cíle.</span><span class="sxs-lookup"><span data-stu-id="74bae-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks to get to its destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="74bae-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="74bae-111">Before you begin</span></span>

<span data-ttu-id="74bae-112">V tomto scénáři budete používat portál Azure se najít typ dalšího směrování a IP adresu.</span><span class="sxs-lookup"><span data-stu-id="74bae-112">In this scenario, you will use the Azure portal to find the next hop type and IP address.</span></span>

<span data-ttu-id="74bae-113">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="74bae-113">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span> <span data-ttu-id="74bae-114">Tento scénář také předpokládá, že skupina prostředků se platný virtuální počítač existuje má být použit.</span><span class="sxs-lookup"><span data-stu-id="74bae-114">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="74bae-115">Scénář</span><span class="sxs-lookup"><span data-stu-id="74bae-115">Scenario</span></span>

<span data-ttu-id="74bae-116">Scénář popsaná v tomto článku používá další směrování, funkce sledovací proces sítě, který vyhledá typ dalšího směrování a IP adresu pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="74bae-116">The scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out the next hop type and IP address for a resource.</span></span> <span data-ttu-id="74bae-117">Další informace o další směrování, navštivte [další směrování přehled](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="74bae-117">To learn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="74bae-118">Načtení sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="74bae-118">Retrieve Network Watcher</span></span>

<span data-ttu-id="74bae-119">Prvním krokem je pro získání instance sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="74bae-119">The first step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="74bae-120">`$networkWatcher` Dalšího směrování je předán proměnná ověřte rutiny.</span><span class="sxs-lookup"><span data-stu-id="74bae-120">The `$networkWatcher` variable is passed to the next hop verify cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-virtual-machine"></a><span data-ttu-id="74bae-121">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="74bae-121">Get a virtual machine</span></span>

<span data-ttu-id="74bae-122">Další směrování vrátí dalšího směrování a IP adresu dalšího přechodu z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="74bae-122">Next hop returns the next hop and the IP address of the next hop from a virtual machine.</span></span> <span data-ttu-id="74bae-123">Id virtuálního počítače je vyžadováno pro rutinu.</span><span class="sxs-lookup"><span data-stu-id="74bae-123">An Id of a virtual machine is required for the cmdlet.</span></span> <span data-ttu-id="74bae-124">Pokud už znáte ID virtuálního počítače používat, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="74bae-124">If you already know the ID of the virtual machine to use, you can skip this step.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

> [!NOTE]
> <span data-ttu-id="74bae-125">Další směrování vyžaduje, aby prostředků virtuálního počítače je přidělená ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="74bae-125">Next hop requires that the VM resource is allocated to run.</span></span>

## <a name="get-the-network-interfaces"></a><span data-ttu-id="74bae-126">Získat rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="74bae-126">Get the network interfaces</span></span>

<span data-ttu-id="74bae-127">IP adresa síťového adaptéru na virtuálním počítači je potřeba v tomto příkladu, nemůžeme načíst síťové adaptéry na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="74bae-127">The IP address of a NIC on the virtual machine is needed, in this example we retrieve the NICs on a virtual machine.</span></span> <span data-ttu-id="74bae-128">Pokud už znáte IP adresu, která chcete testovat na virtuálním počítači, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="74bae-128">If you already know the IP address that you want to test on the virtual machine, you can skip this step.</span></span>

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="get-next-hop"></a><span data-ttu-id="74bae-129">Získat další směrování</span><span class="sxs-lookup"><span data-stu-id="74bae-129">Get Next Hop</span></span>

<span data-ttu-id="74bae-130">Teď říkáme `Get-AzureRmNetworkWatcherNextHop` rutiny.</span><span class="sxs-lookup"><span data-stu-id="74bae-130">Now we call the `Get-AzureRmNetworkWatcherNextHop` cmdlet.</span></span> <span data-ttu-id="74bae-131">Jsme předat rutinu sledovací proces sítě, virtuální počítač Id, zdrojové IP adresy a cílové IP adresy.</span><span class="sxs-lookup"><span data-stu-id="74bae-131">We pass the cmdlet the Network Watcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="74bae-132">V tomto příkladu je cílovou IP adresu pro virtuální počítač v jiné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="74bae-132">In this example, the destination IP address is to a VM in another virtual network.</span></span> <span data-ttu-id="74bae-133">Mezi dvě virtuální sítě je bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="74bae-133">There is a virtual network gateway between the two virtual networks.</span></span>

```powershell
Get-AzureRmNetworkWatcherNextHop -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id -SourceIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress  -DestinationIPAddress 10.0.2.4 
```

## <a name="review-results"></a><span data-ttu-id="74bae-134">Zkontrolujte výsledky</span><span class="sxs-lookup"><span data-stu-id="74bae-134">Review results</span></span>

<span data-ttu-id="74bae-135">Po dokončení, jsou uvedené výsledky.</span><span class="sxs-lookup"><span data-stu-id="74bae-135">When complete, the results are provided.</span></span> <span data-ttu-id="74bae-136">IP adresa dalšího směrování je vrácen a také typ prostředku, který je.</span><span class="sxs-lookup"><span data-stu-id="74bae-136">The next hop IP address is returned as well as the type of resource it is.</span></span> <span data-ttu-id="74bae-137">V tomto scénáři je veřejnou IP adresu brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="74bae-137">In this scenario, it is the public IP address of the virtual network gateway.</span></span>

```
NextHopIpAddress NextHopType           RouteTableId 
---------------- -----------           ------------ 
13.78.238.92     VirtualNetworkGateway Gateway Route
```

<span data-ttu-id="74bae-138">V následujícím seznamu jsou aktuálně dostupné hodnoty NextHopType:</span><span class="sxs-lookup"><span data-stu-id="74bae-138">The following list shows the currently available NextHopType values:</span></span>

<span data-ttu-id="74bae-139">**Typ dalšího směrování**</span><span class="sxs-lookup"><span data-stu-id="74bae-139">**Next Hop Type**</span></span>

* <span data-ttu-id="74bae-140">Internet</span><span class="sxs-lookup"><span data-stu-id="74bae-140">Internet</span></span>
* <span data-ttu-id="74bae-141">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="74bae-141">VirtualAppliance</span></span>
* <span data-ttu-id="74bae-142">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="74bae-142">VirtualNetworkGateway</span></span>
* <span data-ttu-id="74bae-143">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="74bae-143">VnetLocal</span></span>
* <span data-ttu-id="74bae-144">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="74bae-144">HyperNetGateway</span></span>
* <span data-ttu-id="74bae-145">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="74bae-145">VnetPeering</span></span>
* <span data-ttu-id="74bae-146">Žádný</span><span class="sxs-lookup"><span data-stu-id="74bae-146">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="74bae-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74bae-147">Next steps</span></span>

<span data-ttu-id="74bae-148">Zjistěte, jak zkontrolovat nastavení skupiny zabezpečení sítě prostřednictvím kódu programu, navštivte stránky [NSG auditování s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="74bae-148">Learn how to review your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>

















