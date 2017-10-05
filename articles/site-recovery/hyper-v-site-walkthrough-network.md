---
title: "Plán sítí pro technologii Hyper-V (bez System Center VMM) do Azure s replikací pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje plánování sítě požadované při replikaci virtuálních počítačů technologie Hyper-V (bez VMM) do Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5ca12254-975d-48e8-a84d-422825dac9b2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 100b9d8a55c2c163e7a04680f0f7d7963315ee73
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-4-plan-networking-for-hyper-v-to-azure-replication"></a><span data-ttu-id="fc8e9-103">Krok 4: Plánování sítí pro technologii Hyper-V na Azure replikace</span><span class="sxs-lookup"><span data-stu-id="fc8e9-103">Step 4: Plan networking for Hyper-V to Azure replication</span></span>

<span data-ttu-id="fc8e9-104">Tento článek shrnuje sítě aspekty plánování, když replikovat místní virtuální počítače Hyper-V (bez System Center VMM) do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-104">This article summarizes network planning considerations when replicating on-premises Hyper-V VMs (without System Center VMM) to Azure using the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="fc8e9-105">Případné připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="fc8e9-105">Post any comments at the bottom of this article, or ask questions in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="connecting-to-replica-vms-after-failover"></a><span data-ttu-id="fc8e9-106">Připojení k virtuální počítače repliky po převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="fc8e9-106">Connecting to replica VMs after failover</span></span>

<span data-ttu-id="fc8e9-107">Při plánování replikace a převzetí služeb při selhání, mezi klíčové otázky je způsob připojení k virtuálnímu počítači Azure po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-107">When planning your replication and failover strategy, one of the key questions is how to connect to the Azure VM after failover.</span></span> <span data-ttu-id="fc8e9-108">Při navrhování strategie sítě pro virtuální počítače Azure repliky existuje několik možností:</span><span class="sxs-lookup"><span data-stu-id="fc8e9-108">There are a couple of choices when designing your network strategy for replica Azure VMs:</span></span>

- <span data-ttu-id="fc8e9-109">**Jinou IP adresu**: můžete vybrat použít jiný rozsah IP adres pro replikované síť virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-109">**Different IP address**: You can select to use a different IP address range for the replicated Azure VM network.</span></span> <span data-ttu-id="fc8e9-110">V tomto scénáři virtuální počítač získá nové adresy IP po převzetí služeb při selhání, a je nutná aktualizace DNS.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-110">In this scenario the VM gets a new IP address after failover, and a DNS update is required.</span></span> [<span data-ttu-id="fc8e9-111">Další informace</span><span class="sxs-lookup"><span data-stu-id="fc8e9-111">Learn more</span></span>](site-recovery-test-failover-vmm-to-vmm.md#prepare-the-infrastructure-for-test-failover)
- <span data-ttu-id="fc8e9-112">**Stejnou IP adresu**: můžete chtít použít stejného rozsahu IP adres, který ve vaší síti primární místní síť Azure po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-112">**Same IP address**: You might want to use the same IP address range as that in your primary on-premises network, for the Azure network after failover.</span></span>  <span data-ttu-id="fc8e9-113">Zachovat stejné IP adresy zjednodušuje obnovení snížením problémy související se sítí po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-113">Keeping the same IP addresses simplifies the recovery by reducing network related issues after failover.</span></span> <span data-ttu-id="fc8e9-114">Ale pokud replikujete do Azure, musíte po převzetí služeb při selhání aktualizace tras se nové umístění na IP adresy.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-114">However, when you're replicating to Azure, you will need to update routes with the new location of the IP addresses after failover.</span></span>


## <a name="retain-ip-addresses"></a><span data-ttu-id="fc8e9-115">Zachovat IP adresy</span><span class="sxs-lookup"><span data-stu-id="fc8e9-115">Retain IP addresses</span></span>

<span data-ttu-id="fc8e9-116">Site Recovery poskytuje schopnost zachovat pevné IP adresy při přebírání služeb při selhání do Azure, s převzetím služeb podsítě.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-116">Site Recovery provides the capability to retain fixed IP addresses when failing over to Azure, with a subnet failover.</span></span>

<span data-ttu-id="fc8e9-117">S převzetí služeb při selhání podsíť určité podsíti se nachází v lokalitě 1 nebo 2 lokality, ale nikdy v obou lokalitách současně.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-117">With subnet failover, a specific subnet is present at Site 1 or Site 2, but never at both sites simultaneously.</span></span> <span data-ttu-id="fc8e9-118">Aby byla zachována adresní prostor IP adres v případě selhání, můžete prostřednictvím kódu programu uspořádejte infrastruktury směrovač přesunout podsítě z jedné lokality do jiného.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-118">In order to maintain the IP address space in the event of a failover, you programmatically arrange for the router infrastructure to move the subnets from one site to another.</span></span> <span data-ttu-id="fc8e9-119">Během převzetí služeb při selhání přesuňte podsítí s přidružené chráněných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-119">During failover, the subnets move with the associated protected VMs.</span></span> <span data-ttu-id="fc8e9-120">Hlavní nevýhodou je, že v případě selhání, je nutné přesunout celý podsítě.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-120">The main drawback is that in the event of a failure, you have to move the whole subnet.</span></span>


### <a name="failover-example"></a><span data-ttu-id="fc8e9-121">Příklad převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="fc8e9-121">Failover example</span></span>

<span data-ttu-id="fc8e9-122">Podívejme se na příklad pro převzetí služeb při selhání do Azure.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-122">Let's look at an example for failover to Azure.</span></span>

- <span data-ttu-id="fc8e9-123">Ficticious společnosti, společnosti Woodgrove Bank má místní infrastruktury hostování jejich obchodních aplikací.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-123">A ficticious company, Woodgrove Bank, has an on-premises infrastructure hosting their business apps.</span></span> <span data-ttu-id="fc8e9-124">Své mobilní aplikace jsou hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-124">Their mobile applications are hosted on Azure.</span></span>
- <span data-ttu-id="fc8e9-125">Připojení mezi virtuálními počítači Woodgrove Bank v Azure a místními servery jsou poskytovány připojení site-to-site (VPN) mezi místní hraniční sítě a virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-125">Connectivity between Woodgrove Bank VMs in Azure and on-premises servers is provided by a site-to-site (VPN) connection between the on-premises edge network and the Azure virtual network.</span></span>
- <span data-ttu-id="fc8e9-126">Tuto síť VPN znamená, že virtuální síť společnosti v Azure zobrazí jako rozšíření své místní sítě.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-126">This VPN means that the company's virtual network in Azure appears as an extension of their on-premises network.</span></span>
- <span data-ttu-id="fc8e9-127">Woodgrove chce pomocí Site Recovery můžete replikovat místní úlohy do Azure.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-127">Woodgrove wants to use Site Recovery to replicate on-premises workloads to Azure.</span></span>
 - <span data-ttu-id="fc8e9-128">Woodgrove má jak nakládat s aplikací a konfigurace, které závisí na pevně IP adresy a proto potřeba zachovat IP adresy pro své aplikace po převzetí služeb při selhání do Azure.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-128">Woodgrove has to deal with applications and configurations which depend on hard-coded IP addresses, and thus need to retain IP addresses for their applications after failover to Azure.</span></span>
 - <span data-ttu-id="fc8e9-129">Woodgrove jeho přiřazení IP adres z rozsahu 172.16.1.0/24, 172.16.2.0/24 na jeho prostředky, které běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-129">Woodgrove has assigned IP addresses from range 172.16.1.0/24, 172.16.2.0/24 to its resources running in Azure.</span></span>


<span data-ttu-id="fc8e9-130">Woodgrove, abyste mohli replikovat jejích virtuálních počítačů do Azure a zachovat stávající IP adresy, zde je, co společnosti musí udělat:</span><span class="sxs-lookup"><span data-stu-id="fc8e9-130">For Woodgrove to be able to replicate its VMs to Azure while retaining the IP addresses, here's what the company needs to do:</span></span>

1. <span data-ttu-id="fc8e9-131">Vytvoření virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-131">Create an Azure virtual network.</span></span> <span data-ttu-id="fc8e9-132">Rozšíření místní sítě, třeba tak, že aplikace bezproblémově přebírány.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-132">It should be an extension of the on-premises network, so that applications can fail over seamlessly.</span></span>
2. <span data-ttu-id="fc8e9-133">Azure umožňuje přidat připojení VPN typu site-to-site, kromě připojení point-to-site k virtuálním sítím vytvořeným v Azure.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-133">Azure allows you to add site-to-site VPN connectivity, in addition to point-to-site connectivity to the virtual networks created in Azure.</span></span>
3. <span data-ttu-id="fc8e9-134">Při nastavování připojení site-to-site, v síť Azure, můžete směrovat provoz do místního umístění (místní síť) pouze v případě, že rozsah IP adres se liší od místní rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-134">When setting up the site-to-site connection, in the Azure network, you can route traffic to the on-premises location (local-network) only if the IP address range is different from the on-premises IP address range.</span></span>
    - <span data-ttu-id="fc8e9-135">Je to proto, že Azure nepodporuje roztažené podsítě.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-135">This is because Azure doesn’t support stretched subnets.</span></span> <span data-ttu-id="fc8e9-136">Takže pokud máte podsíť 192.168.1.0/24 místně, nemůžete přidat 192.168.1.0/24 místní sítě v síti Azure.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-136">So if you have subnet 192.168.1.0/24 on-premises, you can’t add a local-network 192.168.1.0/24 in the Azure network.</span></span>
    - <span data-ttu-id="fc8e9-137">Toto je očekávané, protože Azure není známo, že neexistují žádné aktivní virtuální počítače v podsíti, a že podsíť se vytváří jenom zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-137">This is expected because Azure doesn’t know that there are no active VMs in the subnet, and that the subnet is being created for disaster recovery only.</span></span>
    - <span data-ttu-id="fc8e9-138">Abyste mohli správně směrovat síťový provoz mimo síť Azure podsítě v síti a v místní síti nesmí být v konfliktu.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-138">To be able to correctly route network traffic out of an Azure network the subnets in the network and the local-network mustn't conflict.</span></span>

![Před převzetí služeb při selhání podsíť](./media/hyper-v-site-walkthrough-network/network-design7.png)

### <a name="before-failover"></a><span data-ttu-id="fc8e9-140">Před převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="fc8e9-140">Before failover</span></span>

1. <span data-ttu-id="fc8e9-141">Vytvoření další sítě (například síť obnovení).</span><span class="sxs-lookup"><span data-stu-id="fc8e9-141">Create an additional network (for example Recovery Network).</span></span> <span data-ttu-id="fc8e9-142">Jedná se o síť ve který převzal virtuální počítače byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-142">This is the network in which failed over VMs are created.</span></span>
2. <span data-ttu-id="fc8e9-143">K zajištění, že IP adresu pro virtuální počítač se uchovávají po převzetí služeb ve vlastnostech virtuálního počítače > **konfigurace**, zadejte stejnou IP adresu, aby měl virtuální počítač na místě a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-143">To ensure that the IP address for a VM is retained after a failover, in the VM properties > **Configure**, specify the same IP address that the VM has on-premises, and click **Save**.</span></span>
3. <span data-ttu-id="fc8e9-144">Když je virtuální počítač převzal, přiřadí Azure Site Recovery k němu zadaná IP adresa.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-144">When the VM is failed over, Azure Site Recovery will assign the provided IP address to it.</span></span>

    ![Vlastnosti sítě](./media/hyper-v-site-walkthrough-network/network-design8.png)

4. <span data-ttu-id="fc8e9-146">Po převzetí služeb při selhání je aktivační událost se aktivuje a virtuální počítače jsou vytvořeny v Azure s požadované IP adresu, můžete připojit k síti pomocí [virtuální síť k virtuální síti vonnection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span><span class="sxs-lookup"><span data-stu-id="fc8e9-146">After failover is trigger is triggered, and the VMs are created in Azure with the required IP address, you can connect to the network using a [Vnet to Vnet vonnection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span></span> <span data-ttu-id="fc8e9-147">Tato akce může provádět skriptování.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-147">This action can be scripted.</span></span>
5. <span data-ttu-id="fc8e9-148">Trasy potřebovat k odpovídajícím způsobem upravit, aby odpovídaly že této 192.168.1.0/24 je nyní přesunuta do Azure.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-148">Routes need to be appropriately modified, to reflect that 192.168.1.0/24 has now moved to Azure.</span></span>

    ![Po převzetí služeb při selhání podsíť](./media/hyper-v-site-walkthrough-network/network-design9.png)

### <a name="after-failover"></a><span data-ttu-id="fc8e9-150">Po převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="fc8e9-150">After failover</span></span>

<span data-ttu-id="fc8e9-151">Pokud nemáte síť Azure, jak je popsáno výše, můžete vytvořit připojení site-to-site VPN mezi primární lokalitou a Azure, po failvoer.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-151">If you don't have an Azure network as illustrated above, you can create a site-to-site VPN connection between your primary site and Azure, after failvoer.</span></span>

## <a name="change-ip-addresses"></a><span data-ttu-id="fc8e9-152">Změna IP adresy</span><span class="sxs-lookup"><span data-stu-id="fc8e9-152">Change IP addresses</span></span>

<span data-ttu-id="fc8e9-153">To [příspěvku na blogu](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) vysvětluje, jak nastavit Azure síťová infrastruktura, pokud nepotřebujete zachovat IP adres po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-153">This [blog post](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) explains how to set up the Azure networking infrastructure when you don't need to retain IP addresses after failover.</span></span> <span data-ttu-id="fc8e9-154">Popis aplikace spustí, vypadá v tom, jak nastavit místní sítí a Azure a s informacemi o spuštění převzetí služeb při selhání se ukončí.</span><span class="sxs-lookup"><span data-stu-id="fc8e9-154">It starts with an application description, looks at how to set up networking on-premises and in Azure, and concludes with information about running failovers.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="fc8e9-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fc8e9-155">Next steps</span></span>

<span data-ttu-id="fc8e9-156">Přejděte na [krok 5: Příprava Azure](hyper-v-site-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="fc8e9-156">Go to [Step 5: Prepare Azure](hyper-v-site-walkthrough-prepare-azure.md)</span></span>
