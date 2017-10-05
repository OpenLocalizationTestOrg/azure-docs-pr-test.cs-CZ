---
title: "Ověřte provoz s IP Adresou sledovací proces sítě Azure tok ověření – prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje, jak zkontrolovat, pokud provoz nebo z virtuálního počítače povolený nebo zakázaný pomocí prostředí PowerShell"
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
ms.openlocfilehash: bf0c01a9af0e28647d11ad89a9d164716d5c8312
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="f6f18-103">Zkontrolujte, jestli je povolené nebo zakázané do nebo z virtuálního počítače s tokem IP přenosy ověřte součást sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="f6f18-103">Check if traffic is allowed or denied to or from a VM with IP flow verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="f6f18-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f6f18-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="f6f18-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f6f18-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="f6f18-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f6f18-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="f6f18-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f6f18-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="f6f18-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="f6f18-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="f6f18-109">Tok IP ověřte, že je funkce sledovací proces sítě, která vám umožní ověřit, pokud provoz je povolený do nebo z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f6f18-109">IP flow verify is a feature of Network Watcher that allows you to verify if traffic is allowed to or from a virtual machine.</span></span> <span data-ttu-id="f6f18-110">Tento scénář je vhodný pro zjištění aktuálního stavu o tom, jestli virtuální počítač může kontaktovat na externí prostředek nebo back-end.</span><span class="sxs-lookup"><span data-stu-id="f6f18-110">This scenario is useful to get a current state of whether a virtual machine can talk to an external resource or backend.</span></span> <span data-ttu-id="f6f18-111">Ověřte toku IP umožňuje ověřit, pokud vaše skupina zabezpečení sítě (NSG) pravidla jsou správně nakonfigurovány a vyřešit toky, kteří jsou Blokovaní pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="f6f18-111">IP flow verify can be used to verify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="f6f18-112">Dalším důvodem pro použití IP tok ověření, je potřeba zajistit provoz, který chcete blokovat, je správně blokován nastavením NSG.</span><span class="sxs-lookup"><span data-stu-id="f6f18-112">Another reason for using IP flow verify is to ensure traffic that you want blocked is being blocked properly by the NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="f6f18-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="f6f18-113">Before you begin</span></span>

<span data-ttu-id="f6f18-114">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě nebo máte existující instanci sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="f6f18-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="f6f18-115">Tento scénář také předpokládá, že skupina prostředků se platný virtuální počítač existuje má být použit.</span><span class="sxs-lookup"><span data-stu-id="f6f18-115">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="f6f18-116">Scénář</span><span class="sxs-lookup"><span data-stu-id="f6f18-116">Scenario</span></span>

<span data-ttu-id="f6f18-117">Tento scénář používá IP tok ověření k ověření, pokud virtuální počítač může kontaktovat známé Bing IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f6f18-117">This scenario uses IP flow verify to verify if a virtual machine can talk to a known Bing IP address.</span></span> <span data-ttu-id="f6f18-118">Pokud je odepřená provoz, vrátí pravidlo zabezpečení, které je odepřen, aby provoz.</span><span class="sxs-lookup"><span data-stu-id="f6f18-118">If the traffic is denied, it returns the security rule that is denying that traffic.</span></span> <span data-ttu-id="f6f18-119">Další informace o toku IP ověřte najdete [IP tok ověření – přehled](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="f6f18-119">To learn more about IP flow verify, visit [IP flow verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="f6f18-120">Načtení sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="f6f18-120">Retrieve Network Watcher</span></span>

<span data-ttu-id="f6f18-121">Prvním krokem je pro získání instance sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="f6f18-121">The first step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="f6f18-122">`$networkWatcher` Se předá proměnná IP tok ověření rutiny.</span><span class="sxs-lookup"><span data-stu-id="f6f18-122">The `$networkWatcher` variable is passed to the IP flow verify cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a><span data-ttu-id="f6f18-123">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="f6f18-123">Get a VM</span></span>

<span data-ttu-id="f6f18-124">Tok IP ověřte testy přenosy do nebo z IP adresy na virtuální počítač do nebo z vzdálený cíl.</span><span class="sxs-lookup"><span data-stu-id="f6f18-124">IP flow verify tests traffic to or from an IP address on a virtual machine to or from a remote destination.</span></span> <span data-ttu-id="f6f18-125">Id virtuálního počítače je vyžadováno pro rutinu.</span><span class="sxs-lookup"><span data-stu-id="f6f18-125">An Id of a virtual machine is required for the cmdlet.</span></span> <span data-ttu-id="f6f18-126">Pokud už znáte ID virtuálního počítače používat, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="f6f18-126">If you already know the ID of the virtual machine to use, you can skip this step.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="get-the-nics"></a><span data-ttu-id="f6f18-127">Získat síťové karty</span><span class="sxs-lookup"><span data-stu-id="f6f18-127">Get the NICS</span></span>

<span data-ttu-id="f6f18-128">IP adresa síťového adaptéru na virtuálním počítači je potřeba v tomto příkladu, nemůžeme načíst síťové adaptéry na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="f6f18-128">The IP address of a NIC on the virtual machine is needed, in this example we retrieve the NICs on a virtual machine.</span></span> <span data-ttu-id="f6f18-129">Pokud už znáte IP adresu, která chcete testovat na virtuálním počítači, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="f6f18-129">If you already know the IP address that you want to test on the virtual machine, you can skip this step.</span></span>

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="run-ip-flow-verify"></a><span data-ttu-id="f6f18-130">Ověření spuštění toku IP</span><span class="sxs-lookup"><span data-stu-id="f6f18-130">Run IP flow verify</span></span>

<span data-ttu-id="f6f18-131">Teď, když máme informace potřebné ke spuštění rutiny jsme spustit `Test-AzureRmNetworkWatcherIPFlow` rutiny k otestování provozu.</span><span class="sxs-lookup"><span data-stu-id="f6f18-131">Now that we have the information needed to run the cmdlet, we run the `Test-AzureRmNetworkWatcherIPFlow` cmdlet to test the traffic.</span></span> <span data-ttu-id="f6f18-132">V tomto příkladu používáme první IP adresu na první síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="f6f18-132">In this example, we are using the first IP address on the first NIC.</span></span>

```powershell
Test-AzureRmNetworkWatcherIPFlow -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id `
-Direction Outbound -Protocol TCP `
-LocalIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress -LocalPort 6895 -RemoteIPAddress 204.79.197.200 -RemotePort 80
```

> [!NOTE]
> <span data-ttu-id="f6f18-133">Tok IP ověření vyžaduje, aby prostředků virtuálního počítače je přidělená ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="f6f18-133">IP flow verify requires that the VM resource is allocated to run.</span></span>

## <a name="review-results"></a><span data-ttu-id="f6f18-134">Zkontrolujte výsledky</span><span class="sxs-lookup"><span data-stu-id="f6f18-134">Review Results</span></span>

<span data-ttu-id="f6f18-135">Po spuštění `Test-AzureRmNetworkWatcherIPFlow` budou vráceny výsledky, v následujícím příkladu je výsledky vrácené z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="f6f18-135">After running `Test-AzureRmNetworkWatcherIPFlow` the results are returned, the following example is the results returned from the preceding step.</span></span>

```
Access RuleName                                  
------ --------                                  
Allow  defaultSecurityRules/AllowInternetOutBound
```

## <a name="next-steps"></a><span data-ttu-id="f6f18-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f6f18-136">Next steps</span></span>

<span data-ttu-id="f6f18-137">Pokud je blokován provoz a neměl by být, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) sledovat pravidla zabezpečení sítě skupiny a zabezpečení, které jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="f6f18-137">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that are defined.</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













