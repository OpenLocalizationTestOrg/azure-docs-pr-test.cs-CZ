---
title: "Další směrování aaaFind s Azure sítě sledovacích procesů další segment – prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje, jak zjistíte, jaké hello dalšího směrování typ je a pomocí ip adresa dalšího směrování pomocí prostředí PowerShell."
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
ms.openlocfilehash: fdb0b4a02d95fc45c103fe952fc1afa095414c18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-powershell"></a><span data-ttu-id="afc26-103">Zjistěte, jaký typ hello dalšího směrování je díky funkci další segment hello v sledovací proces sítě Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="afc26-103">Find out what hello next hop type is using hello Next Hop capability in Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="afc26-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="afc26-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="afc26-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="afc26-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="afc26-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="afc26-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="afc26-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="afc26-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="afc26-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="afc26-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="afc26-109">Další směrování je funkce sledovací proces sítě, která poskytuje možnost hello získat hello typ dalšího směrování a IP adresu podle zadaný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="afc26-109">Next hop is a feature of Network Watcher that provides hello ability get hello next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="afc26-110">Tato funkce je užitečná při určování, zda je brána, internet nebo cílové tooits tooget virtuální sítě prochází přes odchozího provozu z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="afc26-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks tooget tooits destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="afc26-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="afc26-111">Before you begin</span></span>

<span data-ttu-id="afc26-112">V tomto scénáři použijete hello Azure portálu toofind hello typ dalšího směrování a IP adresu.</span><span class="sxs-lookup"><span data-stu-id="afc26-112">In this scenario, you will use hello Azure portal toofind hello next hop type and IP address.</span></span>

<span data-ttu-id="afc26-113">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="afc26-113">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="afc26-114">scénář Hello také předpokládá, že skupina prostředků se platný virtuální počítač existuje toobe použít.</span><span class="sxs-lookup"><span data-stu-id="afc26-114">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="afc26-115">Scénář</span><span class="sxs-lookup"><span data-stu-id="afc26-115">Scenario</span></span>

<span data-ttu-id="afc26-116">scénář Hello popsaná v tomto článku používá další směrování, funkce sledovací proces sítě, který vyhledá hello typ dalšího směrování a IP adresu pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="afc26-116">hello scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out hello next hop type and IP address for a resource.</span></span> <span data-ttu-id="afc26-117">toolearn Další informace o další směrování, navštivte [další směrování přehled](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="afc26-117">toolearn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="afc26-118">Načtení sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="afc26-118">Retrieve Network Watcher</span></span>

<span data-ttu-id="afc26-119">prvním krokem Hello je tooretrieve hello sledovací proces sítě instance.</span><span class="sxs-lookup"><span data-stu-id="afc26-119">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="afc26-120">Hello `$networkWatcher` proměnné je předán další segment toohello ověřte rutiny.</span><span class="sxs-lookup"><span data-stu-id="afc26-120">hello `$networkWatcher` variable is passed toohello next hop verify cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-virtual-machine"></a><span data-ttu-id="afc26-121">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="afc26-121">Get a virtual machine</span></span>

<span data-ttu-id="afc26-122">Další směrování vrátí další segment hello a hello IP adresa dalšího směrování hello z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="afc26-122">Next hop returns hello next hop and hello IP address of hello next hop from a virtual machine.</span></span> <span data-ttu-id="afc26-123">Id virtuálního počítače je vyžadováno pro rutinu hello.</span><span class="sxs-lookup"><span data-stu-id="afc26-123">An Id of a virtual machine is required for hello cmdlet.</span></span> <span data-ttu-id="afc26-124">Pokud už znáte ID hello toouse hello virtuálního počítače, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="afc26-124">If you already know hello ID of hello virtual machine toouse, you can skip this step.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

> [!NOTE]
> <span data-ttu-id="afc26-125">Další směrování vyžaduje, aby hello prostředků virtuálního počítače je přidělená toorun.</span><span class="sxs-lookup"><span data-stu-id="afc26-125">Next hop requires that hello VM resource is allocated toorun.</span></span>

## <a name="get-hello-network-interfaces"></a><span data-ttu-id="afc26-126">Získat hello síťová rozhraní</span><span class="sxs-lookup"><span data-stu-id="afc26-126">Get hello network interfaces</span></span>

<span data-ttu-id="afc26-127">v tomto příkladu, nemůžeme načíst hello síťové adaptéry na virtuálním počítači, je potřeba Hello IP adresa síťového adaptéru na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="afc26-127">hello IP address of a NIC on hello virtual machine is needed, in this example we retrieve hello NICs on a virtual machine.</span></span> <span data-ttu-id="afc26-128">Pokud už znáte hello IP adresu, kterou chcete tootest hello virtuálního počítače, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="afc26-128">If you already know hello IP address that you want tootest on hello virtual machine, you can skip this step.</span></span>

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="get-next-hop"></a><span data-ttu-id="afc26-129">Získat další směrování</span><span class="sxs-lookup"><span data-stu-id="afc26-129">Get Next Hop</span></span>

<span data-ttu-id="afc26-130">Teď říkáme hello `Get-AzureRmNetworkWatcherNextHop` rutiny.</span><span class="sxs-lookup"><span data-stu-id="afc26-130">Now we call hello `Get-AzureRmNetworkWatcherNextHop` cmdlet.</span></span> <span data-ttu-id="afc26-131">Jsme předat hello rutiny hello sledovací proces sítě, virtuální počítač Id, zdrojovou IP adresu a cílové IP adresy.</span><span class="sxs-lookup"><span data-stu-id="afc26-131">We pass hello cmdlet hello Network Watcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="afc26-132">V tomto příkladu je hello cílové IP adresy tooa virtuálních počítačů v jiné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="afc26-132">In this example, hello destination IP address is tooa VM in another virtual network.</span></span> <span data-ttu-id="afc26-133">Mezi hello dvě virtuální sítě je bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="afc26-133">There is a virtual network gateway between hello two virtual networks.</span></span>

```powershell
Get-AzureRmNetworkWatcherNextHop -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id -SourceIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress  -DestinationIPAddress 10.0.2.4 
```

## <a name="review-results"></a><span data-ttu-id="afc26-134">Zkontrolujte výsledky</span><span class="sxs-lookup"><span data-stu-id="afc26-134">Review results</span></span>

<span data-ttu-id="afc26-135">Po dokončení, jsou uvedené výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="afc26-135">When complete, hello results are provided.</span></span> <span data-ttu-id="afc26-136">IP adresa dalšího směrování Hello je i hello typ prostředku, který je vrácen.</span><span class="sxs-lookup"><span data-stu-id="afc26-136">hello next hop IP address is returned as well as hello type of resource it is.</span></span> <span data-ttu-id="afc26-137">V tomto scénáři je hello veřejnou IP adresu brány virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="afc26-137">In this scenario, it is hello public IP address of hello virtual network gateway.</span></span>

```
NextHopIpAddress NextHopType           RouteTableId 
---------------- -----------           ------------ 
13.78.238.92     VirtualNetworkGateway Gateway Route
```

<span data-ttu-id="afc26-138">Hello následující seznam obsahuje hello aktuálně k dispozici NextHopType hodnoty:</span><span class="sxs-lookup"><span data-stu-id="afc26-138">hello following list shows hello currently available NextHopType values:</span></span>

<span data-ttu-id="afc26-139">**Typ dalšího směrování**</span><span class="sxs-lookup"><span data-stu-id="afc26-139">**Next Hop Type**</span></span>

* <span data-ttu-id="afc26-140">Internet</span><span class="sxs-lookup"><span data-stu-id="afc26-140">Internet</span></span>
* <span data-ttu-id="afc26-141">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="afc26-141">VirtualAppliance</span></span>
* <span data-ttu-id="afc26-142">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="afc26-142">VirtualNetworkGateway</span></span>
* <span data-ttu-id="afc26-143">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="afc26-143">VnetLocal</span></span>
* <span data-ttu-id="afc26-144">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="afc26-144">HyperNetGateway</span></span>
* <span data-ttu-id="afc26-145">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="afc26-145">VnetPeering</span></span>
* <span data-ttu-id="afc26-146">Žádný</span><span class="sxs-lookup"><span data-stu-id="afc26-146">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="afc26-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="afc26-147">Next steps</span></span>

<span data-ttu-id="afc26-148">Zjistěte, jak tooreview nastavení skupiny zabezpečení sítě prostřednictvím kódu programu přechodem [NSG auditování s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="afc26-148">Learn how tooreview your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>

















