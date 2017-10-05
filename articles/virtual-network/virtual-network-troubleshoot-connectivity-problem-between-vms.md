---
title: "Řešení potíží s připojením mezi virtuálními počítači Azure | Microsoft Docs"
description: "Naučte se řešení problémů s připojením mezi virtuálními počítači Azure."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/25/2017
ms.author: genli
ms.openlocfilehash: fd0f25c07cb3f385d3e8480ce1e33dec1ae0a214
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a><span data-ttu-id="4d521-103">Řešení potíží s připojením mezi virtuálními počítači Azure</span><span class="sxs-lookup"><span data-stu-id="4d521-103">Troubleshooting connectivity problems between Azure VMs</span></span>

<span data-ttu-id="4d521-104">Může dojít k potížím s připojením mezi virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="4d521-104">You might experience connectivity problems between Azure VMs.</span></span> <span data-ttu-id="4d521-105">Tento článek obsahuje postup pro odstraňování potíží při řešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="4d521-105">This article provides troubleshooting steps to help you resolve this problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a><span data-ttu-id="4d521-106">Příznaky</span><span class="sxs-lookup"><span data-stu-id="4d521-106">Symptom</span></span>

<span data-ttu-id="4d521-107">Virtuální počítač Azure se nemůže připojit k jiným virtuálním Počítačem Azure.</span><span class="sxs-lookup"><span data-stu-id="4d521-107">An Azure VM cannot connect to another Azure VM.</span></span>

## <a name="troubleshooting-guidance"></a><span data-ttu-id="4d521-108">Pokyny při řešení potíží</span><span class="sxs-lookup"><span data-stu-id="4d521-108">Troubleshooting guidance</span></span> 

1. [<span data-ttu-id="4d521-109">Zkontrolujte, pokud síťový adaptér je špatně nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="4d521-109">Check if NIC is misconfigured</span></span>](#step-1-check-if-nic-is-misconfigured)
2. [<span data-ttu-id="4d521-110">Zkontrolujte, pokud je síťový provoz blokována NSG nebo UDR</span><span class="sxs-lookup"><span data-stu-id="4d521-110">Check if network traffic is blocked by NSG or UDR</span></span>](#step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr)
3. [<span data-ttu-id="4d521-111">Zkontrolujte, pokud je síťový provoz blokována bránou firewall virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4d521-111">Check if network traffic is blocked by VM firewall</span></span>](#step-3-check-if-network-traffic-is-blocked-by-vm-firewall)
4. [<span data-ttu-id="4d521-112">Zkontrolujte, zda virtuální počítač aplikace nebo služba naslouchá na portu</span><span class="sxs-lookup"><span data-stu-id="4d521-112">Check whether VM app or service is listening on the port</span></span>](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port)
5. [<span data-ttu-id="4d521-113">Zkontrolujte, zda je problém způsoben překládat pomocí SNAT</span><span class="sxs-lookup"><span data-stu-id="4d521-113">Check whether the problem is caused by SNAT</span></span>](#step-5-check-whether-the-problem-is-caused-by-snat)
6. [<span data-ttu-id="4d521-114">Zkontrolujte, zda je přenos dat blokován nastavením seznamy ACL pro klasické virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4d521-114">Check whether traffic is blocked by ACLs for the classic VM</span></span>](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm)
7. [<span data-ttu-id="4d521-115">Zkontrolujte, zda je pro klasické virtuální počítač vytvořen koncový bod</span><span class="sxs-lookup"><span data-stu-id="4d521-115">Check whether the endpoint is created for the classic VM</span></span>](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm)
8. [<span data-ttu-id="4d521-116">Nelze se připojit k síťové sdílené složky virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4d521-116">Unable to connect to a VM network share</span></span>](#step-8-unable-to-connect-to-a-vm-network-share)
9. [<span data-ttu-id="4d521-117">Připojení mezi virtuálními</span><span class="sxs-lookup"><span data-stu-id="4d521-117">Inter-Vnet connectivity</span></span>](#step-9-inter-vnet-connectivity)

## <a name="troubleshooting-steps"></a><span data-ttu-id="4d521-118">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="4d521-118">Troubleshooting steps</span></span>

<span data-ttu-id="4d521-119">Postupujte podle těchto kroků k vyřešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="4d521-119">Follow these steps to troubleshoot the problem.</span></span> <span data-ttu-id="4d521-120">Zkontrolujte, zda byl problém vyřešen po dokončení každého kroku.</span><span class="sxs-lookup"><span data-stu-id="4d521-120">Check whether the problem is resolved after each step.</span></span> 

### <a name="step-1-check-if-nic-is-misconfigured"></a><span data-ttu-id="4d521-121">Krok 1: Kontrola, pokud síťový adaptér je špatně nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="4d521-121">Step 1: Check if NIC is misconfigured</span></span>

<span data-ttu-id="4d521-122">Postupujte podle [jak resetovat síťové rozhraní pro virtuální počítač s Windows Azure](../virtual-machines/windows/reset-network-interface.md).</span><span class="sxs-lookup"><span data-stu-id="4d521-122">Follow [How to reset network interface for Azure Windows VM](../virtual-machines/windows/reset-network-interface.md).</span></span> 

<span data-ttu-id="4d521-123">Pokud k problému dochází po úpravě síťové rozhraní (NIC), postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="4d521-123">If the problem occurs after you modify the network interface (NIC), follow these steps:</span></span>

<span data-ttu-id="4d521-124">**Mulit seskupování virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="4d521-124">**Mulit-NIC VMs**</span></span>

1. <span data-ttu-id="4d521-125">Přidejte síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="4d521-125">Add a NIC.</span></span>
2. <span data-ttu-id="4d521-126">Opravte problémy v chybný síťový adaptér nebo odebrat chybný síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="4d521-126">Fix the problems in the bad NIC or remove bad NIC.</span></span>  <span data-ttu-id="4d521-127">Potom readd síťovou kartu.</span><span class="sxs-lookup"><span data-stu-id="4d521-127">Then readd the NIC.</span></span>

<span data-ttu-id="4d521-128">Další informace najdete v tématu [síťová rozhraní pro přidání nebo odebrání z virtuálních počítačů](virtual-network-network-interface-vm.md).</span><span class="sxs-lookup"><span data-stu-id="4d521-128">For more information, see [Add network interfaces to or remove from virtual machines](virtual-network-network-interface-vm.md).</span></span>

<span data-ttu-id="4d521-129">**Jedním seskupování virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="4d521-129">**Single-NIC VM**</span></span> 

- [<span data-ttu-id="4d521-130">Znovu nasaďte Windows virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4d521-130">Redeploy Windows VM</span></span>](../virtual-machines/windows/redeploy-to-new-node.md)
- [<span data-ttu-id="4d521-131">Opětovné nasazení virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="4d521-131">Redeploy Linux VM</span></span>](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr"></a><span data-ttu-id="4d521-132">Krok 2: Kontrola, pokud je síťový provoz blokována NSG nebo UDR</span><span class="sxs-lookup"><span data-stu-id="4d521-132">Step 2: Check if network traffic is blocked by NSG or UDR</span></span>

<span data-ttu-id="4d521-133">Využívat [ověřte toku IP sledovací proces sítě](../network-watcher/network-watcher-ip-flow-verify-overview.md) a [protokolování toku NSG](../network-watcher/network-watcher-nsg-flow-logging-overview.md) a zjistit, zda existuje skupina zabezpečení sítě nebo trasy definované uživatelem, který je v konfliktu s přenosy.</span><span class="sxs-lookup"><span data-stu-id="4d521-133">Utilize [Network Watcher IP Flow Verify](../network-watcher/network-watcher-ip-flow-verify-overview.md) and [NSG Flow Logging](../network-watcher/network-watcher-nsg-flow-logging-overview.md) to identify if there is a Network Security Group or User-Defined Route that is interfering with traffic flow.</span></span>

### <a name="step-3-check-if-network-traffic-is-blocked-by-vm-firewall"></a><span data-ttu-id="4d521-134">Krok 3: Kontrola, pokud síťový provoz je blokována bránou firewall virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4d521-134">Step 3: Check if network traffic is blocked by VM firewall</span></span>

<span data-ttu-id="4d521-135">Zakažte bránu firewall a poté otestujte výsledek.</span><span class="sxs-lookup"><span data-stu-id="4d521-135">Disable the firewall, and then test the result.</span></span> <span data-ttu-id="4d521-136">Pokud se problém nevyřeší, ověřte nastavení v bráně firewall a znovu povolte.</span><span class="sxs-lookup"><span data-stu-id="4d521-136">If the problem is resolved, validate the settings in the firewall and re-enable.</span></span>

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-the-port"></a><span data-ttu-id="4d521-137">Krok 4: Kontrola, zda virtuální počítač aplikace nebo služba naslouchá na portu</span><span class="sxs-lookup"><span data-stu-id="4d521-137">Step 4: Check whether VM app or service is listening on the port</span></span>

<span data-ttu-id="4d521-138">Zkontrolujte, zda virtuální počítač aplikace nebo služba naslouchá na portu můžete použít jednu z následujících metod</span><span class="sxs-lookup"><span data-stu-id="4d521-138">You can use one of the following methods to check whether VM Application or Service is listening on the port</span></span>

- <span data-ttu-id="4d521-139">Spusťte následující příkazy ke kontrole, jestli server naslouchá na tomto portu.</span><span class="sxs-lookup"><span data-stu-id="4d521-139">Run the following commands to check whether the server is listening on that port.</span></span>

<span data-ttu-id="4d521-140">**Virtuální počítač s Windows**</span><span class="sxs-lookup"><span data-stu-id="4d521-140">**Windows VM**</span></span>

    netstat –ano

<span data-ttu-id="4d521-141">**Virtuální počítač s Linuxem**</span><span class="sxs-lookup"><span data-stu-id="4d521-141">**Linux VM**</span></span>

    netstat -l

- <span data-ttu-id="4d521-142">Spustit **Telnet** příkazu ve virtuálním počítači na sebe sama k otestování portu.</span><span class="sxs-lookup"><span data-stu-id="4d521-142">Run the **Telnet** command on the VM to itself to test the port.</span></span> <span data-ttu-id="4d521-143">Pokud se test nezdaří, aplikace nebo služba není nakonfigurován pro naslouchání na portu.</span><span class="sxs-lookup"><span data-stu-id="4d521-143">If the test fails, application or service is not configured to listen on the port.</span></span>

### <a name="step-5-check-whether-the-problem-is-caused-by-snat"></a><span data-ttu-id="4d521-144">Krok 5: Zkontrolujte, zda je problém způsoben překládat pomocí SNAT</span><span class="sxs-lookup"><span data-stu-id="4d521-144">Step 5: Check whether the problem is caused by SNAT</span></span>

<span data-ttu-id="4d521-145">V některých případech je virtuální počítač umístěn za řešení vyrovnávání zatížení, který má závislost na prostředky mimo Azure.</span><span class="sxs-lookup"><span data-stu-id="4d521-145">In some scenarios, the VM is placed behind a Load balance solution that has a dependency on resources outside of Azure.</span></span> <span data-ttu-id="4d521-146">V těchto scénářích platí, pokud dojde k problémům nepřerušované připojení problém může být způsobeno [překládat pomocí SNAT port vyčerpání](../load-balancer/load-balancer-outbound-connections.md).</span><span class="sxs-lookup"><span data-stu-id="4d521-146">In these scenarios, if you experience intermittent connection problems, the problem may be caused by [SNAT port exhaustion](../load-balancer/load-balancer-outbound-connections.md).</span></span> <span data-ttu-id="4d521-147">K vyřešení problému, vytvořte pro každý virtuální počítač, který je za zařízením na Vyrovnávání zatížení a zabezpečení pomocí NSG nebo seznamu ACL VIP (nebo splnění pro classic).</span><span class="sxs-lookup"><span data-stu-id="4d521-147">To resolve the issue, create a VIP (or ILPIP for classic) for each VM that is behind the Load balancer and secure with NSG or ACL.</span></span> 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm"></a><span data-ttu-id="4d521-148">Krok 6: Zkontrolujte, zda je přenos dat blokován nastavením seznamy ACL pro klasické virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4d521-148">Step 6: Check whether traffic is blocked by ACLs for the classic VM</span></span>

<span data-ttu-id="4d521-149">Seznam ACL umožňuje selektivně povolit nebo zakázat přenosů pro koncový bod virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4d521-149">An ACL provides the ability to selectively permit or deny traffic for a virtual machine endpoint.</span></span> <span data-ttu-id="4d521-150">Další informace najdete v tématu [spravovat seznam ACL u koncového bodu](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).</span><span class="sxs-lookup"><span data-stu-id="4d521-150">For more information, see [Manage the ACL on an endpoint](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).</span></span>

### <a name="step-7-check-whether-the-endpoint-is-created-for-the-classic-vm"></a><span data-ttu-id="4d521-151">Krok 7: Zkontrolujte, zda je pro klasické virtuální počítač vytvořen koncový bod</span><span class="sxs-lookup"><span data-stu-id="4d521-151">Step 7: Check whether the endpoint is created for the classic VM</span></span>

<span data-ttu-id="4d521-152">Všechny virtuální počítače, které vytvoříte v Azure pomocí modelu nasazení classic může automaticky komunikovat přes kanál privátní síť s dalšími virtuálními počítači ve stejné cloudové služby nebo virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4d521-152">All VMs that you create in Azure using the classic deployment model can automatically communicate over a private network channel with other virtual machines in the same cloud service or virtual network.</span></span> <span data-ttu-id="4d521-153">Počítače v jiných virtuálních sítích však vyžadují koncové body směrovat příchozí síťový provoz do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4d521-153">However, computers on other virtual networks require endpoints to direct the inbound network traffic to a virtual machine.</span></span> <span data-ttu-id="4d521-154">Další informace najdete v tématu [jak nastavit koncové body](../virtual-machines/windows/classic/setup-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="4d521-154">For more information, see [How to set up endpoints](../virtual-machines/windows/classic/setup-endpoints.md).</span></span>

### <a name="step-8-unable-to-connect-to-a-vm-network-share"></a><span data-ttu-id="4d521-155">Krok 8: Nelze se připojit k síťové sdílené složky virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4d521-155">Step 8: Unable to connect to a VM network share</span></span>

<span data-ttu-id="4d521-156">Pokud není možné se připojit k síťové sdílené složky virtuálních počítačů, problém může být způsobeno není k dispozici síťové adaptéry ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="4d521-156">If you are unable to connect to a VM network share, the problem can be caused by unavailable NICs in the VM.</span></span> <span data-ttu-id="4d521-157">Pokud chcete odstranit není k dispozici síťové karty, najdete v části [jak odstranit není k dispozici síťové karty](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)</span><span class="sxs-lookup"><span data-stu-id="4d521-157">To delete the unavailable NICs, see [How to delete the unavailable NICs](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)</span></span>

### <a name="step-9-inter-vnet-connectivity"></a><span data-ttu-id="4d521-158">Krok 9: Připojení mezi virtuálními</span><span class="sxs-lookup"><span data-stu-id="4d521-158">Step 9: Inter-Vnet connectivity</span></span>

<span data-ttu-id="4d521-159">Využívat [ověřte toku IP sledovací proces sítě](../network-watcher/network-watcher-ip-flow-verify-overview.md) a [protokolování toku NSG](../network-watcher/network-watcher-nsg-flow-logging-overview.md) a zjistit, zda existuje skupina zabezpečení sítě nebo trasy definované uživatelem, který je v konfliktu s přenosy.</span><span class="sxs-lookup"><span data-stu-id="4d521-159">Utilize [Network Watcher IP Flow Verify](../network-watcher/network-watcher-ip-flow-verify-overview.md) and [NSG Flow Logging](../network-watcher/network-watcher-nsg-flow-logging-overview.md) to identify if there is a Network Security Group or User-Defined Route that is interfering with traffic flow.</span></span> <span data-ttu-id="4d521-160">Můžete taky ověřit konfiguraci Inter-Vnet [zde](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).</span><span class="sxs-lookup"><span data-stu-id="4d521-160">You can also validate your Inter-Vnet configuration [here](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).</span></span>

### <a name="need-help-contact-support"></a><span data-ttu-id="4d521-161">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="4d521-161">Need help?</span></span> <span data-ttu-id="4d521-162">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="4d521-162">Contact support.</span></span>
<span data-ttu-id="4d521-163">Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.</span><span class="sxs-lookup"><span data-stu-id="4d521-163">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>