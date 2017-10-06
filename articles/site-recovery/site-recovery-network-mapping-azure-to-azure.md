---
title: "aaaNetwork mapování mezi dvěma oblastí Azure ve službě Azure Site Recovery | Microsoft Docs"
description: "Azure Site Recovery koordinuje hello replikace, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Další informace o převzetí služeb při selhání tooAzure nebo do sekundárního datacentra."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: pratshar
ms.openlocfilehash: 4f80c44e3f94eaf446bc01a7041d91fe34aa78d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="network-mapping-between-two-azure-regions"></a><span data-ttu-id="6ca22-104">Mapování sítě mezi dvěma oblastmi Azure</span><span class="sxs-lookup"><span data-stu-id="6ca22-104">Network mapping between two Azure regions</span></span>


<span data-ttu-id="6ca22-105">Tento článek popisuje, jak toomap Azure virtuální sítě dvou oblastí Azure mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="6ca22-105">This article describes how toomap Azure virtual networks of two Azure regions with each other.</span></span> <span data-ttu-id="6ca22-106">Mapování sítě zajišťuje, když replikovaného virtuálního počítače je vytvořen v cílové hello oblast Azure, je vytvořený na hello virtuální síť, která je sítě namapované toovirtual hello zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6ca22-106">Network mapping ensures that when replicated virtual machine is created in hello target Azure region, it is created on hello virtual network that is mapped toovirtual network of hello source virtual machine.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="6ca22-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6ca22-107">Prerequisites</span></span>
<span data-ttu-id="6ca22-108">Předtím, než je mapovat sítě, ujistěte se, jste vytvořili [virtuálních sítí Azure](../virtual-network/virtual-networks-overview.md) v obou zdroje a cíle oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="6ca22-108">Before you map networks make sure, you have created [Azure virtual networks](../virtual-network/virtual-networks-overview.md) in both source and target Azure regions.</span></span>

## <a name="map-networks"></a><span data-ttu-id="6ca22-109">Mapování sítě</span><span class="sxs-lookup"><span data-stu-id="6ca22-109">Map networks</span></span>

<span data-ttu-id="6ca22-110">toomap virtuální síť Azure v jedné oblasti Azure tooanother virtuální sítě v jiné oblasti, přejděte tooSite infrastruktura Recovery -> mapování sítě (pro virtuální počítače Azure) a vytvořit mapování sítě.</span><span class="sxs-lookup"><span data-stu-id="6ca22-110">toomap an Azure virtual network in one Azure region tooanother virtual network in another region, go tooSite Recovery Infrastructure -> Network Mapping (For Azure Virtual Machines) and create a network mapping.</span></span>

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


<span data-ttu-id="6ca22-112">Následující příklad virtuální počítač běží ve východní Asie oblasti a je v hello replikují tooSoutheast Asie.</span><span class="sxs-lookup"><span data-stu-id="6ca22-112">In hello example below my virtual machine is running in East Asia region and is being replicated tooSoutheast Asia.</span></span>

<span data-ttu-id="6ca22-113">Vyberte hello zdrojová a Cílová síť a pak klikněte na tlačítko OK toocreate mapování sítě z východní Asie tooSoutheast Asie.</span><span class="sxs-lookup"><span data-stu-id="6ca22-113">Select hello source and target network and then click OK toocreate a network mapping from East Asia tooSoutheast Asia.</span></span>

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


<span data-ttu-id="6ca22-115">Hello stejnou věc toocreate mapování sítě z jihovýchodní Asie tooEast Asie.</span><span class="sxs-lookup"><span data-stu-id="6ca22-115">Do hello same thing toocreate a network mapping from Southeast Asia tooEast Asia.</span></span>  
![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="mapping-network-when-enabling-replication"></a><span data-ttu-id="6ca22-117">Mapování sítě při povolení replikace</span><span class="sxs-lookup"><span data-stu-id="6ca22-117">Mapping network when enabling replication</span></span>

<span data-ttu-id="6ca22-118">Pokud mapování sítě není provést, pokud jsou replikace virtuálního počítače pro hello první čas formuláře jedné oblasti Azure tooanother, pak můžete vybrat cílovou síť jako součást hello stejný postup.</span><span class="sxs-lookup"><span data-stu-id="6ca22-118">If network mapping is not done when you are replicating a virtual machine for hello first time form one Azure region tooanother, then you can choose target network as part of hello same process.</span></span> <span data-ttu-id="6ca22-119">Site Recovery vytvoří mapování sítě z oblasti tootarget oblast zdroje a cílová oblast toosource oblast založené na tomto výběru.</span><span class="sxs-lookup"><span data-stu-id="6ca22-119">Site Recovery creates network mappings from source region tootarget region and from target region toosource region based on this selection.</span></span>   

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

<span data-ttu-id="6ca22-121">Ve výchozím nastavení, Site Recovery vytvoří síť v hello cílová oblast, která je stejná toohello zdrojové síti a přidáním '-automatické obnovení systému, jako název toohello příponu hello zdroje sítě.</span><span class="sxs-lookup"><span data-stu-id="6ca22-121">By default, Site Recovery creates a network in hello target region that is identical toohello source network and by adding '-asr' as a suffix toohello name of hello source network.</span></span> <span data-ttu-id="6ca22-122">Kliknutím na tlačítko Přizpůsobit můžete již vytvořené síťové.</span><span class="sxs-lookup"><span data-stu-id="6ca22-122">You can choose an already created network by clicking Customize.</span></span>

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)


<span data-ttu-id="6ca22-124">Pokud již probíhá hello mapování sítě, nelze změnit hello cílová virtuální síť při povolení replikace.</span><span class="sxs-lookup"><span data-stu-id="6ca22-124">If hello network mapping is already done, you can't change hello target virtual network while enabling replication.</span></span> <span data-ttu-id="6ca22-125">toochange, upravte existující mapování sítě.</span><span class="sxs-lookup"><span data-stu-id="6ca22-125">toochange it, modify existing network mapping.</span></span>  

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> <span data-ttu-id="6ca22-128">Pokud změníte mapování sítě z oblasti-1 tooregion-2, nezapomeňte že upravit mapování sítě hello z oblasti 2 tooregion-1 také.</span><span class="sxs-lookup"><span data-stu-id="6ca22-128">If you modify a network mapping from region-1 tooregion-2, make sure you modify hello network mapping from region-2 tooregion-1 as well.</span></span>
>
>


## <a name="subnet-selection"></a><span data-ttu-id="6ca22-129">Výběr podsítě</span><span class="sxs-lookup"><span data-stu-id="6ca22-129">Subnet selection</span></span>
<span data-ttu-id="6ca22-130">Podsíť hello cílového virtuálního počítače je vybrána na základě názvu hello hello podsítě hello zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6ca22-130">Subnet of hello target virtual machine is selected based on hello name of hello subnet of hello source virtual machine.</span></span> <span data-ttu-id="6ca22-131">Pokud podsíť hello je stejný jako u hello zdrojového virtuálního počítače k dispozici v cílové síti hello, název a který je vybrán pro hello cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6ca22-131">If there is a subnet of hello same name as that of hello source virtual machine available in hello target network, then that is chosen for hello target virtual machine.</span></span> <span data-ttu-id="6ca22-132">Pokud neexistuje žádná podsíť s hello stejný název v hello cílové síti, pak abecedně první podsíť je vybrána jako hello cílové podsíti.</span><span class="sxs-lookup"><span data-stu-id="6ca22-132">If there is no subnet with hello same name in hello target network, then alphabetically first subnet is chosen as hello target subnet.</span></span> <span data-ttu-id="6ca22-133">Tato podsíť můžete upravit tak, že přejdete tooCompute a nastavení sítě hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6ca22-133">You can modify this subnet by going tooCompute and Network settings of hello virtual machine.</span></span>

![Upravit podsíť](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a><span data-ttu-id="6ca22-135">IP adresa</span><span class="sxs-lookup"><span data-stu-id="6ca22-135">IP address</span></span>

<span data-ttu-id="6ca22-136">IP adresa pro každý hello síťového rozhraní hello cílového virtuálního počítače je zvolen následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6ca22-136">IP address for each of hello network interface of hello target virtual machine is chosen as follows:</span></span>

### <a name="dhcp"></a><span data-ttu-id="6ca22-137">DHCP</span><span class="sxs-lookup"><span data-stu-id="6ca22-137">DHCP</span></span>
<span data-ttu-id="6ca22-138">Síťové rozhraní hello hello zdrojového virtuálního počítače používá protokol DHCP, pak síťové rozhraní hello cílový virtuální počítač je také nastavena jako DHCP.</span><span class="sxs-lookup"><span data-stu-id="6ca22-138">If hello network interface of hello source virtual machine is using DHCP, then network interface of hello target virtual machine is also set as DHCP.</span></span>

### <a name="static-ip"></a><span data-ttu-id="6ca22-139">Statická IP adresa</span><span class="sxs-lookup"><span data-stu-id="6ca22-139">Static IP</span></span>
<span data-ttu-id="6ca22-140">Pokud síťové rozhraní hello hello zdrojový virtuální počítač používá statickou IP adresu, pak síťové rozhraní hello cílový virtuální počítač je také nastavit toouse statickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="6ca22-140">If hello network interface of hello source virtual machine is using Static IP, then network interface of hello target virtual machine is also set toouse Static IP.</span></span> <span data-ttu-id="6ca22-141">Statická IP adresa je zvolen následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6ca22-141">Static IP is chosen as follows:</span></span>

#### <a name="same-address-space"></a><span data-ttu-id="6ca22-142">Stejné adresní prostor</span><span class="sxs-lookup"><span data-stu-id="6ca22-142">Same address space</span></span>

<span data-ttu-id="6ca22-143">Pokud hello hello zdroj podsíť a podsíť hello cíl stejný adresní prostor, cílová IP adresa je nastavena stejné jako IP hello hello síťového rozhraní hello zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6ca22-143">If hello source subnet and hello target subnet have hello same address space, then target IP is set same as hello IP of  hello network interface of hello source virtual machine.</span></span> <span data-ttu-id="6ca22-144">Pokud není k dispozici stejnou IP Adresou, některé dostupnou IP adresu nastavena jako hello cílová IP adresa.</span><span class="sxs-lookup"><span data-stu-id="6ca22-144">If same IP is not available, then some other available IP is set as hello target IP.</span></span>

#### <a name="different-address-space"></a><span data-ttu-id="6ca22-145">Jiným adresním prostorem</span><span class="sxs-lookup"><span data-stu-id="6ca22-145">Different address space</span></span>

<span data-ttu-id="6ca22-146">Pokud hello zdroj podsíť a podsíť cíl hello jiným adresním prostorem, cílová IP adresa je nastaven jako dostupnou IP adresu v hello cílové podsíti.</span><span class="sxs-lookup"><span data-stu-id="6ca22-146">If hello source subnet and hello target subnet have different address space, then target IP is set as any available IP in hello target subnet.</span></span>

<span data-ttu-id="6ca22-147">Hello cílová IP adresa na každé rozhraní sítě můžete upravit tak, že přejdete tooCompute a nastavení sítě hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6ca22-147">You can modify hello target IP on each network interface by going tooCompute and Network settings of hello virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ca22-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6ca22-148">Next steps</span></span>

- <span data-ttu-id="6ca22-149">Další informace o [sítě pokyny pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="6ca22-149">Learn about [networking guidance for replicating Azure VMs](site-recovery-azure-to-azure-networking-guidance.md).</span></span>
