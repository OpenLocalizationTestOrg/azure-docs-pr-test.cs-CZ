---
title: "Ověřte aaaverify provoz s tokem IP sledovací proces sítě Azure – prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje, jak toocheck Pokud tooor provoz z virtuálního počítače povolený nebo zakázaný pomocí prostředí PowerShell"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e1dad757-8c5d-467f-812e-7cc751143207
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 924da1de1bd554e15816886f8e51d7f170f0e7ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="801f2-103">Zkontrolujte, jestli je povolené nebo zakázané tooor z virtuálního počítače s tokem IP přenosy ověřte součást sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="801f2-103">Check if traffic is allowed or denied tooor from a VM with IP flow verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="801f2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="801f2-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="801f2-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="801f2-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="801f2-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="801f2-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="801f2-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="801f2-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="801f2-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="801f2-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="801f2-109">Tok IP ověřte, že je funkce sledovací proces sítě, která umožňuje tooverify Pokud provoz je povolený tooor z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="801f2-109">IP flow verify is a feature of Network Watcher that allows you tooverify if traffic is allowed tooor from a virtual machine.</span></span> <span data-ttu-id="801f2-110">Tento scénář je užitečné tooget aktuálním stavu o tom, jestli virtuální počítač může kontaktovat tooan externí prostředek nebo back-end.</span><span class="sxs-lookup"><span data-stu-id="801f2-110">This scenario is useful tooget a current state of whether a virtual machine can talk tooan external resource or backend.</span></span> <span data-ttu-id="801f2-111">Tok IP ověření může být použité tooverify, pokud vaše skupina zabezpečení sítě (NSG) pravidla jsou správně nakonfigurovány a řešení potíží s toky, kteří jsou Blokovaní pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="801f2-111">IP flow verify can be used tooverify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="801f2-112">Dalším důvodem pro použití IP tok ověření je tooensure přenosy, které chcete blokovat je správně blokován nastavením hello NSG.</span><span class="sxs-lookup"><span data-stu-id="801f2-112">Another reason for using IP flow verify is tooensure traffic that you want blocked is being blocked properly by hello NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="801f2-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="801f2-113">Before you begin</span></span>

<span data-ttu-id="801f2-114">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě nebo existující instanci sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="801f2-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="801f2-115">scénář Hello také předpokládá, že skupina prostředků se platný virtuální počítač existuje toobe použít.</span><span class="sxs-lookup"><span data-stu-id="801f2-115">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="801f2-116">Scénář</span><span class="sxs-lookup"><span data-stu-id="801f2-116">Scenario</span></span>

<span data-ttu-id="801f2-117">Tento scénář používá IP tok ověření tooverify, pokud virtuální počítač může kontaktovat tooa známé Bing IP adresu.</span><span class="sxs-lookup"><span data-stu-id="801f2-117">This scenario uses IP flow verify tooverify if a virtual machine can talk tooa known Bing IP address.</span></span> <span data-ttu-id="801f2-118">Pokud je odepřená hello provoz, vrátí hello pravidlo zabezpečení, které je odepřen, aby provoz.</span><span class="sxs-lookup"><span data-stu-id="801f2-118">If hello traffic is denied, it returns hello security rule that is denying that traffic.</span></span> <span data-ttu-id="801f2-119">Další informace o toku IP ověřit, navštivte toolearn [IP tok ověření – přehled](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="801f2-119">toolearn more about IP flow verify, visit [IP flow verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="801f2-120">Načtení sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="801f2-120">Retrieve Network Watcher</span></span>

<span data-ttu-id="801f2-121">prvním krokem Hello je tooretrieve hello sledovací proces sítě instance.</span><span class="sxs-lookup"><span data-stu-id="801f2-121">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="801f2-122">Hello `$networkWatcher` proměnné se předá toohello IP tok ověření rutiny.</span><span class="sxs-lookup"><span data-stu-id="801f2-122">hello `$networkWatcher` variable is passed toohello IP flow verify cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a><span data-ttu-id="801f2-123">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="801f2-123">Get a VM</span></span>

<span data-ttu-id="801f2-124">Tok IP ověřte testy tooor provoz z IP adresy na virtuální počítač tooor z vzdálený cíl.</span><span class="sxs-lookup"><span data-stu-id="801f2-124">IP flow verify tests traffic tooor from an IP address on a virtual machine tooor from a remote destination.</span></span> <span data-ttu-id="801f2-125">Id virtuálního počítače je vyžadováno pro rutinu hello.</span><span class="sxs-lookup"><span data-stu-id="801f2-125">An Id of a virtual machine is required for hello cmdlet.</span></span> <span data-ttu-id="801f2-126">Pokud už znáte ID hello toouse hello virtuálního počítače, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="801f2-126">If you already know hello ID of hello virtual machine toouse, you can skip this step.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="get-hello-nics"></a><span data-ttu-id="801f2-127">Získat hello síťové karty</span><span class="sxs-lookup"><span data-stu-id="801f2-127">Get hello NICS</span></span>

<span data-ttu-id="801f2-128">v tomto příkladu, nemůžeme načíst hello síťové adaptéry na virtuálním počítači, je potřeba Hello IP adresa síťového adaptéru na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="801f2-128">hello IP address of a NIC on hello virtual machine is needed, in this example we retrieve hello NICs on a virtual machine.</span></span> <span data-ttu-id="801f2-129">Pokud už znáte hello IP adresu, kterou chcete tootest hello virtuálního počítače, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="801f2-129">If you already know hello IP address that you want tootest on hello virtual machine, you can skip this step.</span></span>

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="run-ip-flow-verify"></a><span data-ttu-id="801f2-130">Ověření spuštění toku IP</span><span class="sxs-lookup"><span data-stu-id="801f2-130">Run IP flow verify</span></span>

<span data-ttu-id="801f2-131">Teď, když máme hello informace potřebné toorun hello rutiny, spustíme hello `Test-AzureRmNetworkWatcherIPFlow` rutiny tootest hello provoz.</span><span class="sxs-lookup"><span data-stu-id="801f2-131">Now that we have hello information needed toorun hello cmdlet, we run hello `Test-AzureRmNetworkWatcherIPFlow` cmdlet tootest hello traffic.</span></span> <span data-ttu-id="801f2-132">V tomto příkladu používáme hello první IP adresou na síťovém adaptéru hello první.</span><span class="sxs-lookup"><span data-stu-id="801f2-132">In this example, we are using hello first IP address on hello first NIC.</span></span>

```powershell
Test-AzureRmNetworkWatcherIPFlow -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id `
-Direction Outbound -Protocol TCP `
-LocalIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress -LocalPort 6895 -RemoteIPAddress 204.79.197.200 -RemotePort 80
```

> [!NOTE]
> <span data-ttu-id="801f2-133">Tok IP ověření vyžaduje, aby hello prostředků virtuálního počítače je přidělená toorun.</span><span class="sxs-lookup"><span data-stu-id="801f2-133">IP flow verify requires that hello VM resource is allocated toorun.</span></span>

## <a name="review-results"></a><span data-ttu-id="801f2-134">Zkontrolujte výsledky</span><span class="sxs-lookup"><span data-stu-id="801f2-134">Review Results</span></span>

<span data-ttu-id="801f2-135">Po spuštění `Test-AzureRmNetworkWatcherIPFlow` hello výsledky se vrátí, hello následující příklad je hello výsledky vrácené z hello předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="801f2-135">After running `Test-AzureRmNetworkWatcherIPFlow` hello results are returned, hello following example is hello results returned from hello preceding step.</span></span>

```
Access RuleName                                  
------ --------                                  
Allow  defaultSecurityRules/AllowInternetOutBound
```

## <a name="next-steps"></a><span data-ttu-id="801f2-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="801f2-136">Next steps</span></span>

<span data-ttu-id="801f2-137">Pokud je blokován provoz a neměl by být, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="801f2-137">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are defined.</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













