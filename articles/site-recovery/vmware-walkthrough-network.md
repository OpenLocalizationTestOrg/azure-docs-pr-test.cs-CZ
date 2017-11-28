---
title: "aaaPlan sítě pro replikaci tooAzure VMware | Microsoft Docs"
description: "Tento článek popisuje plánování požadované při replikaci virtuálních počítačů VMware tooAzure sítě"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5578a0b4-0352-41c3-9cce-56beb17a4d97
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 2b4f385c768cc7f5e98abae0afb8258b00f3724f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-plan-networking-for-vmware-tooazure-replication"></a><span data-ttu-id="491c7-103">Krok 4: Plánování sítě pro replikaci tooAzure VMware</span><span class="sxs-lookup"><span data-stu-id="491c7-103">Step 4: Plan networking for VMware tooAzure replication</span></span>

<span data-ttu-id="491c7-104">Tento článek shrnuje sítě při replikovat místní virtuální počítače VMware tooAzure pomocí hello plánování do úvahy [Azure Site Recovery](site-recovery-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="491c7-104">This article summarizes network planning considerations when replicating on-premises VMware VMs tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="491c7-105">Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="491c7-105">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="connect-tooreplica-vms"></a><span data-ttu-id="491c7-106">Připojit virtuální počítače tooreplica</span><span class="sxs-lookup"><span data-stu-id="491c7-106">Connect tooreplica VMs</span></span>

<span data-ttu-id="491c7-107">Při plánování replikace a převzetí služeb při selhání, je mezi klíčové otázky hello jak tooconnect toohello virtuálního počítače Azure po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="491c7-107">When planning your replication and failover strategy, one of hello key questions is how tooconnect toohello Azure VM after failover.</span></span> <span data-ttu-id="491c7-108">Při navrhování strategie sítě pro virtuální počítače Azure repliky existuje několik možností:</span><span class="sxs-lookup"><span data-stu-id="491c7-108">There are a couple of choices when designing your network strategy for replica Azure VMs:</span></span>

- <span data-ttu-id="491c7-109">**Použít jinou IP adresu**: můžete vybrat toouse jiný rozsah IP adres pro síť virtuálních počítačů Azure replikovat hello.</span><span class="sxs-lookup"><span data-stu-id="491c7-109">**Use different IP address**: You can select toouse a different IP address range for hello replicated Azure VM network.</span></span> <span data-ttu-id="491c7-110">V tento scénář hello počítač získá nové adresy IP po převzetí služeb při selhání a je nutná aktualizace DNS.</span><span class="sxs-lookup"><span data-stu-id="491c7-110">In this scenario hello VM gets a new IP address after failover, and a DNS update is required.</span></span>
- <span data-ttu-id="491c7-111">**Zachovat stejnou IP adresu**: můžete chtít toouse hello stejného rozsahu IP adres, který ve vaší primární místní lokalitě pro hello síť Azure po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="491c7-111">**Retain same IP address**: You might want toouse hello same IP address range as that in your primary on-premises site, for hello Azure network after failover.</span></span> <span data-ttu-id="491c7-112">Zachování hello stejné IP adresy zjednodušuje obnovení hello snížením sítě o problémech souvisejících s po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="491c7-112">Keeping hello same IP addresses simplifies hello recovery by reducing network related issues after failover.</span></span> <span data-ttu-id="491c7-113">Ale když replikujete tooAzure, budete potřebovat tooupdate tras se nové umístění hello hello IP adres po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="491c7-113">However, when you're replicating tooAzure, you will need tooupdate routes with hello new location of hello IP addresses after failover.</span></span> 


## <a name="retain-ip-addresses"></a><span data-ttu-id="491c7-114">Zachovat IP adresy</span><span class="sxs-lookup"><span data-stu-id="491c7-114">Retain IP addresses</span></span>

<span data-ttu-id="491c7-115">Site Recovery poskytuje hello schopností tooretain pevné IP adresy při přebírání služeb při selhání tooAzure s převzetím služeb podsítě.</span><span class="sxs-lookup"><span data-stu-id="491c7-115">Site Recovery provides hello capability tooretain fixed IP addresses when failing over tooAzure, with a subnet failover.</span></span>

<span data-ttu-id="491c7-116">S převzetí služeb při selhání podsíť určité podsíti se nachází v lokalitě 1 nebo 2 lokality, ale nikdy v obou lokalitách současně.</span><span class="sxs-lookup"><span data-stu-id="491c7-116">With subnet failover, a specific subnet is present at Site 1 or Site 2, but never at both sites simultaneously.</span></span> <span data-ttu-id="491c7-117">V pořadí toomaintain hello adresní prostor IP adres v hello události převzetí služeb při selhání můžete programově uspořádat pro hello směrovač infrastruktury toomove hello podsítě z jedné lokality tooanother.</span><span class="sxs-lookup"><span data-stu-id="491c7-117">In order toomaintain hello IP address space in hello event of a failover, you programmatically arrange for hello router infrastructure toomove hello subnets from one site tooanother.</span></span> <span data-ttu-id="491c7-118">Během převzetí služeb při selhání související hello přesunutí podsítě s hello chráněných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="491c7-118">During failover, hello subnets move with hello associated protected VMs.</span></span> <span data-ttu-id="491c7-119">Hello Hlavní nevýhodou je, že v případě hello selhání, máte toomove hello celou podsíť.</span><span class="sxs-lookup"><span data-stu-id="491c7-119">hello main drawback is that in hello event of a failure, you have toomove hello whole subnet.</span></span>


### <a name="failover-example"></a><span data-ttu-id="491c7-120">Příklad převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="491c7-120">Failover example</span></span>

<span data-ttu-id="491c7-121">Podívejme se na příklad tooAzure převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="491c7-121">Let's look at an example for failover tooAzure.</span></span>

- <span data-ttu-id="491c7-122">Ficticious společnosti, společnosti Woodgrove Bank má místní infrastruktury hostování jejich obchodních aplikací.</span><span class="sxs-lookup"><span data-stu-id="491c7-122">A ficticious company, Woodgrove Bank, has an on-premises infrastructure hosting their business apps.</span></span> <span data-ttu-id="491c7-123">Své mobilní aplikace jsou hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="491c7-123">Their mobile applications are hosted on Azure.</span></span>
- <span data-ttu-id="491c7-124">Připojení mezi virtuálními počítači Woodgrove Bank v Azure a místními servery jsou poskytovány připojení site-to-site (VPN) mezi hello místní hraniční síti a hello virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="491c7-124">Connectivity between Woodgrove Bank VMs in Azure and on-premises servers is provided by a site-to-site (VPN) connection between hello on-premises edge network and hello Azure virtual network.</span></span>
- <span data-ttu-id="491c7-125">To VPN znamená, že hello společnosti virtuální sítě v Azure se zobrazí jako rozšíření své místní sítě.</span><span class="sxs-lookup"><span data-stu-id="491c7-125">This VPN means that hello company's virtual network in Azure appears as an extension of their on-premises network.</span></span>
- <span data-ttu-id="491c7-126">Woodgrove chce toouse Site Recovery tooreplicate místní úlohy tooAzure.</span><span class="sxs-lookup"><span data-stu-id="491c7-126">Woodgrove wants toouse Site Recovery tooreplicate on-premises workloads tooAzure.</span></span>
 - <span data-ttu-id="491c7-127">Woodgrove má toodeal s aplikacemi a konfigurace, které závisí na pevně IP adresy a proto po převzetí služeb při selhání tooAzure potřebovat tooretain IP adresy pro své aplikace.</span><span class="sxs-lookup"><span data-stu-id="491c7-127">Woodgrove has toodeal with applications and configurations which depend on hard-coded IP addresses, and thus need tooretain IP addresses for their applications after failover tooAzure.</span></span>
 - <span data-ttu-id="491c7-128">Woodgrove má přiřazené IP adres z rozsahu 172.16.1.0/24 172.16.2.0/24 tooits prostředky běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="491c7-128">Woodgrove has assigned IP addresses from range 172.16.1.0/24, 172.16.2.0/24 tooits resources running in Azure.</span></span>


<span data-ttu-id="491c7-129">Pro Woodgrove toobe možné tooreplicate, jeho tooAzure virtuálních počítačů při zachování hello IP adresy zde je, jaké hello společnost potřebuje toodo:</span><span class="sxs-lookup"><span data-stu-id="491c7-129">For Woodgrove toobe able tooreplicate its VMS tooAzure while retaining hello IP addresses, here's what hello company needs toodo:</span></span>

1. <span data-ttu-id="491c7-130">Vytvoření virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="491c7-130">Create an Azure virtual network.</span></span> <span data-ttu-id="491c7-131">Je nutné rozšíření hello místní sítě, tak, aby aplikace můžete převzetí služeb při selhání bez problémů.</span><span class="sxs-lookup"><span data-stu-id="491c7-131">It should be an extension of hello on-premises network, so that applications can fail over seamlessly.</span></span>
2. <span data-ttu-id="491c7-132">Azure umožňuje vám tooadd site-to-site VPN připojení, kromě připojení toopoint-to-site toohello virtuálním sítím vytvořeným v Azure.</span><span class="sxs-lookup"><span data-stu-id="491c7-132">Azure allows you tooadd site-to-site VPN connectivity, in addition toopoint-to-site connectivity toohello virtual networks created in Azure.</span></span>
3. <span data-ttu-id="491c7-133">Při nastavování připojení site-to-site hello, v hello Azure síť, pouze v případě, že se liší od rozsah adres IP místní hello hello rozsah IP adres je možné směrovat provoz toohello místní umístění (místní síť).</span><span class="sxs-lookup"><span data-stu-id="491c7-133">When setting up hello site-to-site connection, in hello Azure network, you can route traffic toohello on-premises location (local-network) only if hello IP address range is different from hello on-premises IP address range.</span></span>
    - <span data-ttu-id="491c7-134">Je to proto, že Azure nepodporuje roztažené podsítě.</span><span class="sxs-lookup"><span data-stu-id="491c7-134">This is because Azure doesn’t support stretched subnets.</span></span> <span data-ttu-id="491c7-135">Takže pokud máte podsíť 192.168.1.0/24 místně, nemůžete přidat 192.168.1.0/24 místní sítě v hello síť Azure.</span><span class="sxs-lookup"><span data-stu-id="491c7-135">So if you have subnet 192.168.1.0/24 on-premises, you can’t add a local-network 192.168.1.0/24 in hello Azure network.</span></span>
    - <span data-ttu-id="491c7-136">Toto je očekávané, protože Azure není známo, že nejsou žádné aktivní virtuální počítače v podsíti hello a zotavení po havárii. jenom se vytváří této podsíti hello.</span><span class="sxs-lookup"><span data-stu-id="491c7-136">This is expected because Azure doesn’t know that there are no active VMs in hello subnet, and that hello subnet is being created for disaster recovery only.</span></span>
    - <span data-ttu-id="491c7-137">nesmí být v konfliktu toobe možné toocorrectly směrovat síťový provoz z podsítě hello síť Azure v síti hello a místní sítí hello.</span><span class="sxs-lookup"><span data-stu-id="491c7-137">toobe able toocorrectly route network traffic out of an Azure network hello subnets in hello network and hello local-network mustn't conflict.</span></span>

![Před převzetí služeb při selhání podsíť](./media/site-recovery-network-design/network-design7.png)

### <a name="before-failover"></a><span data-ttu-id="491c7-139">Před převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="491c7-139">Before failover</span></span>

1. <span data-ttu-id="491c7-140">Vytvoření další sítě (například síť obnovení).</span><span class="sxs-lookup"><span data-stu-id="491c7-140">Create an additional network (for example Recovery Network).</span></span> <span data-ttu-id="491c7-141">Toto je hello sítě ve který převzal virtuální počítače byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="491c7-141">This is hello network in which failed over VMs are created.</span></span>
2. <span data-ttu-id="491c7-142">tooensure, který hello IP adresu pro virtuální počítač se uchovávají po převzetí služeb ve vlastnostech virtuálního počítače hello > **konfigurace**, zadejte hello stejnou IP adresu tohoto hello má místní virtuální počítač a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="491c7-142">tooensure that hello IP address for a VM is retained after a failover, in hello VM properties > **Configure**, specify hello same IP address that hello VM has on-premises, and click **Save**.</span></span>
3. <span data-ttu-id="491c7-143">Když hello virtuálních počítačů při selhání, přiřadí Azure Site Recovery hello zadat IP adresu tooit.</span><span class="sxs-lookup"><span data-stu-id="491c7-143">When hello VM is failed over, Azure Site Recovery will assign hello provided IP address tooit.</span></span>

    ![Vlastnosti sítě](./media/site-recovery-network-design/network-design8.png)

4. <span data-ttu-id="491c7-145">Po převzetí služeb při selhání je aktivační událost se aktivuje a hello virtuální počítače jsou vytvořené v Azure s IP adresou hello vyžaduje, můžete připojit pomocí sítě toohello [připojení virtuální sítě tooVnet](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span><span class="sxs-lookup"><span data-stu-id="491c7-145">After failover is trigger is triggered, and hello VMs are created in Azure with hello required IP address, you can connect toohello network using a [Vnet tooVnet connection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span></span> <span data-ttu-id="491c7-146">Tato akce může provádět skriptování.</span><span class="sxs-lookup"><span data-stu-id="491c7-146">This action can be scripted.</span></span>
5. <span data-ttu-id="491c7-147">Trasy potřebovat odpovídajícím způsobem upravit, toobe tooreflect této 192.168.1.0/24 teď přesunul tooAzure.</span><span class="sxs-lookup"><span data-stu-id="491c7-147">Routes need toobe appropriately modified, tooreflect that 192.168.1.0/24 has now moved tooAzure.</span></span>

    ![Po převzetí služeb při selhání podsíť](./media/site-recovery-network-design/network-design9.png)

### <a name="after-failover"></a><span data-ttu-id="491c7-149">Po převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="491c7-149">After failover</span></span>

<span data-ttu-id="491c7-150">Pokud nemáte síť Azure, jak je popsáno výše, můžete vytvořit připojení site-to-site VPN mezi primární lokalitou a Azure, po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="491c7-150">If you don't have an Azure network as illustrated above, you can create a site-to-site VPN connection between your primary site and Azure, after failover.</span></span>

## <a name="change-ip-addresses"></a><span data-ttu-id="491c7-151">Změna IP adresy</span><span class="sxs-lookup"><span data-stu-id="491c7-151">Change IP addresses</span></span>

<span data-ttu-id="491c7-152">To [příspěvku na blogu](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) vysvětluje, jak tooset až hello Azure síťová infrastruktura, pokud nepotřebujete tooretain IP adres po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="491c7-152">This [blog post](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) explains how tooset up hello Azure networking infrastructure when you don't need tooretain IP addresses after failover.</span></span> <span data-ttu-id="491c7-153">Popis aplikace spustí, vypadá na jak tooset až sítě místně a v Azure a s informacemi o spuštění převzetí služeb při selhání se ukončí.</span><span class="sxs-lookup"><span data-stu-id="491c7-153">It starts with an application description, looks at how tooset up networking on-premises and in Azure, and concludes with information about running failovers.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="491c7-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="491c7-154">Next steps</span></span>

<span data-ttu-id="491c7-155">Přejděte příliš[krok 5: Příprava Azure](vmware-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="491c7-155">Go too[Step 5: Prepare Azure](vmware-walkthrough-prepare-azure.md)</span></span>
