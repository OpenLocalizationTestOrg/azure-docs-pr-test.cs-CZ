---
title: "potíže s připojením k aaaTroubleshooting mezi virtuálními počítači Azure | Microsoft Docs"
description: "Zjistěte, jak tooTroubleshoot hello potíže s připojením k mezi virtuálními počítači Azure."
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
ms.openlocfilehash: ee0819178dcbee2bf529a495758e6c33f49e085e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a><span data-ttu-id="d7553-103">Řešení potíží s připojením mezi virtuálními počítači Azure</span><span class="sxs-lookup"><span data-stu-id="d7553-103">Troubleshooting connectivity problems between Azure VMs</span></span>

<span data-ttu-id="d7553-104">Může dojít k potížím s připojením mezi virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="d7553-104">You might experience connectivity problems between Azure VMs.</span></span> <span data-ttu-id="d7553-105">Tento článek obsahuje řešení problémů s kroky toohelp vyřešíte tento problém.</span><span class="sxs-lookup"><span data-stu-id="d7553-105">This article provides troubleshooting steps toohelp you resolve this problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a><span data-ttu-id="d7553-106">Příznaky</span><span class="sxs-lookup"><span data-stu-id="d7553-106">Symptom</span></span>

<span data-ttu-id="d7553-107">Virtuální počítač Azure se nemůže připojit tooanother virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="d7553-107">An Azure VM cannot connect tooanother Azure VM.</span></span>

## <a name="troubleshooting-guidance"></a><span data-ttu-id="d7553-108">Pokyny při řešení potíží</span><span class="sxs-lookup"><span data-stu-id="d7553-108">Troubleshooting guidance</span></span> 

1. [<span data-ttu-id="d7553-109">Zkontrolujte, pokud síťový adaptér je špatně nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="d7553-109">Check if NIC is misconfigured</span></span>](#step-1-check-if-nic-is-misconfigured)
2. [<span data-ttu-id="d7553-110">Zkontrolujte, pokud je síťový provoz blokována NSG nebo UDR</span><span class="sxs-lookup"><span data-stu-id="d7553-110">Check if network traffic is blocked by NSG or UDR</span></span>](#step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr)
3. [<span data-ttu-id="d7553-111">Zkontrolujte, pokud je síťový provoz blokována bránou firewall virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d7553-111">Check if network traffic is blocked by VM firewall</span></span>](#step-3-check-if-network-traffic-is-blocked-by-vm-firewall)
4. [<span data-ttu-id="d7553-112">Zkontrolujte, zda virtuální počítač aplikace nebo služba naslouchá na portu hello</span><span class="sxs-lookup"><span data-stu-id="d7553-112">Check whether VM app or service is listening on hello port</span></span>](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port)
5. [<span data-ttu-id="d7553-113">Zkontrolujte, zda text hello problém je způsoben překládat pomocí SNAT</span><span class="sxs-lookup"><span data-stu-id="d7553-113">Check whether hello problem is caused by SNAT</span></span>](#step-5-check-whether-the-problem-is-caused-by-snat)
6. [<span data-ttu-id="d7553-114">Zkontrolujte zda je přenos dat blokován nastavením seznamy ACL pro hello classic virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d7553-114">Check whether traffic is blocked by ACLs for hello classic VM</span></span>](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm)
7. [<span data-ttu-id="d7553-115">Zkontrolujte zda hello koncový bod vytvořen pro hello classic virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d7553-115">Check whether hello endpoint is created for hello classic VM</span></span>](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm)
8. [<span data-ttu-id="d7553-116">Nelze tooconnect tooa virtuální počítač síťové sdílené složky</span><span class="sxs-lookup"><span data-stu-id="d7553-116">Unable tooconnect tooa VM network share</span></span>](#step-8-unable-to-connect-to-a-vm-network-share)
9. [<span data-ttu-id="d7553-117">Připojení mezi virtuálními</span><span class="sxs-lookup"><span data-stu-id="d7553-117">Inter-Vnet connectivity</span></span>](#step-9-inter-vnet-connectivity)

## <a name="troubleshooting-steps"></a><span data-ttu-id="d7553-118">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="d7553-118">Troubleshooting steps</span></span>

<span data-ttu-id="d7553-119">Postupujte podle těchto kroků tootroubleshoot hello problém.</span><span class="sxs-lookup"><span data-stu-id="d7553-119">Follow these steps tootroubleshoot hello problem.</span></span> <span data-ttu-id="d7553-120">Zkontrolujte, zda text hello problém vyřešen po dokončení každého kroku.</span><span class="sxs-lookup"><span data-stu-id="d7553-120">Check whether hello problem is resolved after each step.</span></span> 

### <a name="step-1-check-if-nic-is-misconfigured"></a><span data-ttu-id="d7553-121">Krok 1: Kontrola, pokud síťový adaptér je špatně nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="d7553-121">Step 1: Check if NIC is misconfigured</span></span>

<span data-ttu-id="d7553-122">Postupujte podle [jak tooreset síťové rozhraní pro virtuální počítač s Windows Azure](../virtual-machines/windows/reset-network-interface.md).</span><span class="sxs-lookup"><span data-stu-id="d7553-122">Follow [How tooreset network interface for Azure Windows VM](../virtual-machines/windows/reset-network-interface.md).</span></span> 

<span data-ttu-id="d7553-123">Pokud vznikne problém hello po úpravě hello síťové rozhraní (NIC), postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="d7553-123">If hello problem occurs after you modify hello network interface (NIC), follow these steps:</span></span>

<span data-ttu-id="d7553-124">**Mulit seskupování virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="d7553-124">**Mulit-NIC VMs**</span></span>

1. <span data-ttu-id="d7553-125">Přidejte síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="d7553-125">Add a NIC.</span></span>
2. <span data-ttu-id="d7553-126">Opravte problémy hello v hello chybný síťový adaptér nebo odeberte chybné síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="d7553-126">Fix hello problems in hello bad NIC or remove bad NIC.</span></span>  <span data-ttu-id="d7553-127">Potom readd hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="d7553-127">Then readd hello NIC.</span></span>

<span data-ttu-id="d7553-128">Další informace najdete v tématu [tooor rozhraní sítě přidat odebrat z virtuálních počítačů](virtual-network-network-interface-vm.md).</span><span class="sxs-lookup"><span data-stu-id="d7553-128">For more information, see [Add network interfaces tooor remove from virtual machines](virtual-network-network-interface-vm.md).</span></span>

<span data-ttu-id="d7553-129">**Jedním seskupování virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="d7553-129">**Single-NIC VM**</span></span> 

- [<span data-ttu-id="d7553-130">Znovu nasaďte Windows virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d7553-130">Redeploy Windows VM</span></span>](../virtual-machines/windows/redeploy-to-new-node.md)
- [<span data-ttu-id="d7553-131">Opětovné nasazení virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="d7553-131">Redeploy Linux VM</span></span>](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr"></a><span data-ttu-id="d7553-132">Krok 2: Kontrola, pokud je síťový provoz blokována NSG nebo UDR</span><span class="sxs-lookup"><span data-stu-id="d7553-132">Step 2: Check if network traffic is blocked by NSG or UDR</span></span>

<span data-ttu-id="d7553-133">Využívat [ověřte toku IP sledovací proces sítě](../network-watcher/network-watcher-ip-flow-verify-overview.md) a [protokolování toku NSG](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify, pokud je skupina zabezpečení sítě nebo trasy definované uživatelem, který je v konfliktu s přenosy.</span><span class="sxs-lookup"><span data-stu-id="d7553-133">Utilize [Network Watcher IP Flow Verify](../network-watcher/network-watcher-ip-flow-verify-overview.md) and [NSG Flow Logging](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify if there is a Network Security Group or User-Defined Route that is interfering with traffic flow.</span></span>

### <a name="step-3-check-if-network-traffic-is-blocked-by-vm-firewall"></a><span data-ttu-id="d7553-134">Krok 3: Kontrola, pokud síťový provoz je blokována bránou firewall virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d7553-134">Step 3: Check if network traffic is blocked by VM firewall</span></span>

<span data-ttu-id="d7553-135">Zakažte hello brány firewall a poté otestujte hello výsledek.</span><span class="sxs-lookup"><span data-stu-id="d7553-135">Disable hello firewall, and then test hello result.</span></span> <span data-ttu-id="d7553-136">Pokud je hello problém vyřešen, ověřte nastavení hello v bráně firewall hello a znovu povolte.</span><span class="sxs-lookup"><span data-stu-id="d7553-136">If hello problem is resolved, validate hello settings in hello firewall and re-enable.</span></span>

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-hello-port"></a><span data-ttu-id="d7553-137">Krok 4: Kontrola, zda virtuální počítač aplikace nebo služba naslouchá na portu hello</span><span class="sxs-lookup"><span data-stu-id="d7553-137">Step 4: Check whether VM app or service is listening on hello port</span></span>

<span data-ttu-id="d7553-138">Můžete použít jednu z následujících metod toocheck, zda virtuální počítač aplikace nebo služba naslouchá na portu hello hello</span><span class="sxs-lookup"><span data-stu-id="d7553-138">You can use one of hello following methods toocheck whether VM Application or Service is listening on hello port</span></span>

- <span data-ttu-id="d7553-139">Spusťte následující příkazy toocheck, zda text hello server naslouchá na tomto portu hello.</span><span class="sxs-lookup"><span data-stu-id="d7553-139">Run hello following commands toocheck whether hello server is listening on that port.</span></span>

<span data-ttu-id="d7553-140">**Virtuální počítač s Windows**</span><span class="sxs-lookup"><span data-stu-id="d7553-140">**Windows VM**</span></span>

    netstat –ano

<span data-ttu-id="d7553-141">**Virtuální počítač s Linuxem**</span><span class="sxs-lookup"><span data-stu-id="d7553-141">**Linux VM**</span></span>

    netstat -l

- <span data-ttu-id="d7553-142">Spustit hello **Telnet** na hello port hello tootest tooitself virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d7553-142">Run hello **Telnet** command on hello VM tooitself tootest hello port.</span></span> <span data-ttu-id="d7553-143">Pokud hello test se nezdaří, aplikace nebo služba není nakonfigurované toolisten na portu hello.</span><span class="sxs-lookup"><span data-stu-id="d7553-143">If hello test fails, application or service is not configured toolisten on hello port.</span></span>

### <a name="step-5-check-whether-hello-problem-is-caused-by-snat"></a><span data-ttu-id="d7553-144">Krok 5: Zkontrolujte, zda text hello problém je způsoben překládat pomocí SNAT</span><span class="sxs-lookup"><span data-stu-id="d7553-144">Step 5: Check whether hello problem is caused by SNAT</span></span>

<span data-ttu-id="d7553-145">V některých scénářích hello virtuální počítač je umístěn za řešení vyrovnávání zatížení, který má závislost na prostředky mimo Azure.</span><span class="sxs-lookup"><span data-stu-id="d7553-145">In some scenarios, hello VM is placed behind a Load balance solution that has a dependency on resources outside of Azure.</span></span> <span data-ttu-id="d7553-146">V těchto scénářích platí, pokud dojde k problémům nepřerušované připojení hello problém může být způsobeno [překládat pomocí SNAT port vyčerpání](../load-balancer/load-balancer-outbound-connections.md).</span><span class="sxs-lookup"><span data-stu-id="d7553-146">In these scenarios, if you experience intermittent connection problems, hello problem may be caused by [SNAT port exhaustion](../load-balancer/load-balancer-outbound-connections.md).</span></span> <span data-ttu-id="d7553-147">tooresolve hello problém, vytvoření virtuální adresy IP (nebo splnění pro klasické) pro každý virtuální počítač, který je za zařízením na Vyrovnávání zatížení hello a zabezpečení pomocí NSG nebo seznamu ACL.</span><span class="sxs-lookup"><span data-stu-id="d7553-147">tooresolve hello issue, create a VIP (or ILPIP for classic) for each VM that is behind hello Load balancer and secure with NSG or ACL.</span></span> 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-hello-classic-vm"></a><span data-ttu-id="d7553-148">Krok 6: Zkontrolujte zda je přenos dat blokován nastavením seznamy ACL pro hello classic virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d7553-148">Step 6: Check whether traffic is blocked by ACLs for hello classic VM</span></span>

<span data-ttu-id="d7553-149">Seznam ACL poskytuje hello možnost tooselectively povolení nebo zakážou provoz pro koncový bod virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d7553-149">An ACL provides hello ability tooselectively permit or deny traffic for a virtual machine endpoint.</span></span> <span data-ttu-id="d7553-150">Další informace najdete v tématu [spravovat hello seznamu ACL v koncovém bodě](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).</span><span class="sxs-lookup"><span data-stu-id="d7553-150">For more information, see [Manage hello ACL on an endpoint](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).</span></span>

### <a name="step-7-check-whether-hello-endpoint-is-created-for-hello-classic-vm"></a><span data-ttu-id="d7553-151">Krok 7: Zkontrolujte zda hello koncový bod vytvořen pro hello classic virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d7553-151">Step 7: Check whether hello endpoint is created for hello classic VM</span></span>

<span data-ttu-id="d7553-152">Všechny virtuální počítače, které vytvoříte v Azure pomocí modelu nasazení classic hello můžete automaticky komunikovat přes kanál privátní síť s dalšími virtuálními počítači v hello že stejné cloudové služby nebo virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d7553-152">All VMs that you create in Azure using hello classic deployment model can automatically communicate over a private network channel with other virtual machines in hello same cloud service or virtual network.</span></span> <span data-ttu-id="d7553-153">Počítače v jiných virtuálních sítích však vyžadují koncové body toodirect hello příchozích síťových přenosů tooa virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d7553-153">However, computers on other virtual networks require endpoints toodirect hello inbound network traffic tooa virtual machine.</span></span> <span data-ttu-id="d7553-154">Další informace najdete v tématu [jak tooset až koncové body](../virtual-machines/windows/classic/setup-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="d7553-154">For more information, see [How tooset up endpoints](../virtual-machines/windows/classic/setup-endpoints.md).</span></span>

### <a name="step-8-unable-tooconnect-tooa-vm-network-share"></a><span data-ttu-id="d7553-155">Krok 8: Nelze tooconnect tooa virtuální počítač síťové sdílené složky</span><span class="sxs-lookup"><span data-stu-id="d7553-155">Step 8: Unable tooconnect tooa VM network share</span></span>

<span data-ttu-id="d7553-156">Pokud jste nelze tooconnect tooa virtuální počítač síťové sdílené složky, hello problém může být způsobeno není k dispozici síťové adaptéry v hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d7553-156">If you are unable tooconnect tooa VM network share, hello problem can be caused by unavailable NICs in hello VM.</span></span> <span data-ttu-id="d7553-157">toodelete hello síťové adaptéry k dispozici, najdete v části [jak toodelete hello není k dispozici síťové karty](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)</span><span class="sxs-lookup"><span data-stu-id="d7553-157">toodelete hello unavailable NICs, see [How toodelete hello unavailable NICs](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)</span></span>

### <a name="step-9-inter-vnet-connectivity"></a><span data-ttu-id="d7553-158">Krok 9: Připojení mezi virtuálními</span><span class="sxs-lookup"><span data-stu-id="d7553-158">Step 9: Inter-Vnet connectivity</span></span>

<span data-ttu-id="d7553-159">Využívat [ověřte toku IP sledovací proces sítě](../network-watcher/network-watcher-ip-flow-verify-overview.md) a [protokolování toku NSG](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify, pokud je skupina zabezpečení sítě nebo trasy definované uživatelem, který je v konfliktu s přenosy.</span><span class="sxs-lookup"><span data-stu-id="d7553-159">Utilize [Network Watcher IP Flow Verify](../network-watcher/network-watcher-ip-flow-verify-overview.md) and [NSG Flow Logging](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify if there is a Network Security Group or User-Defined Route that is interfering with traffic flow.</span></span> <span data-ttu-id="d7553-160">Můžete taky ověřit konfiguraci Inter-Vnet [zde](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).</span><span class="sxs-lookup"><span data-stu-id="d7553-160">You can also validate your Inter-Vnet configuration [here](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).</span></span>

### <a name="need-help-contact-support"></a><span data-ttu-id="d7553-161">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="d7553-161">Need help?</span></span> <span data-ttu-id="d7553-162">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="d7553-162">Contact support.</span></span>
<span data-ttu-id="d7553-163">Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém.</span><span class="sxs-lookup"><span data-stu-id="d7553-163">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>
