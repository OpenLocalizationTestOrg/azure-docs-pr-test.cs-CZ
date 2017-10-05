---
title: "Mapování sítě mezi dvěma oblastmi Azure ve službě Azure Site Recovery | Microsoft Docs"
description: "Azure Site Recovery koordinuje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Další informace o převzetí služeb při selhání do Azure nebo do sekundárního datacentra."
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
ms.openlocfilehash: 9d6a806ec533259797080fbfee2c38f918ebd8a2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="network-mapping-between-two-azure-regions"></a><span data-ttu-id="83ca5-104">Mapování sítě mezi dvěma oblastmi Azure</span><span class="sxs-lookup"><span data-stu-id="83ca5-104">Network mapping between two Azure regions</span></span>


<span data-ttu-id="83ca5-105">Tento článek popisuje, jak k mapování virtuální sítě Azure dvou oblastí Azure mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="83ca5-105">This article describes how to map Azure virtual networks of two Azure regions with each other.</span></span> <span data-ttu-id="83ca5-106">Mapování sítě zajišťuje, když replikovaného virtuálního počítače je vytvořen v cílové oblasti Azure, je vytvořený ve virtuální síti, který je namapovaný na virtuální sítě zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="83ca5-106">Network mapping ensures that when replicated virtual machine is created in the target Azure region, it is created on the virtual network that is mapped to virtual network of the source virtual machine.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="83ca5-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="83ca5-107">Prerequisites</span></span>
<span data-ttu-id="83ca5-108">Předtím, než je mapovat sítě, ujistěte se, jste vytvořili [virtuálních sítí Azure](../virtual-network/virtual-networks-overview.md) v obou zdroje a cíle oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="83ca5-108">Before you map networks make sure, you have created [Azure virtual networks](../virtual-network/virtual-networks-overview.md) in both source and target Azure regions.</span></span>

## <a name="map-networks"></a><span data-ttu-id="83ca5-109">Mapování sítě</span><span class="sxs-lookup"><span data-stu-id="83ca5-109">Map networks</span></span>

<span data-ttu-id="83ca5-110">Chcete-li jinou virtuální sítí v jiné oblasti mapovat virtuální síť Azure v jedné oblasti Azure, přejděte infrastruktura Site Recovery -> mapování sítě (pro virtuální počítače Azure) a vytvořit mapování sítě.</span><span class="sxs-lookup"><span data-stu-id="83ca5-110">To map an Azure virtual network in one Azure region to another virtual network in another region, go to Site Recovery Infrastructure -> Network Mapping (For Azure Virtual Machines) and create a network mapping.</span></span>

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


<span data-ttu-id="83ca5-112">Virtuální počítač v následujícím příkladu je spuštěná ve východní Asie oblasti a je právě replikován pro jihovýchodní Asie.</span><span class="sxs-lookup"><span data-stu-id="83ca5-112">In the example below my virtual machine is running in East Asia region and is being replicated to Southeast Asia.</span></span>

<span data-ttu-id="83ca5-113">Vyberte zdrojové a cílové sítě a potom klikněte na tlačítko OK vytvoření mapování sítě z východní Asie k jihovýchodní Asie.</span><span class="sxs-lookup"><span data-stu-id="83ca5-113">Select the source and target network and then click OK to create a network mapping from East Asia to Southeast Asia.</span></span>

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


<span data-ttu-id="83ca5-115">To samé vytvoření mapování sítě z jihovýchodní Asie k východní Asie.</span><span class="sxs-lookup"><span data-stu-id="83ca5-115">Do the same thing to create a network mapping from Southeast Asia to East Asia.</span></span>  
![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="mapping-network-when-enabling-replication"></a><span data-ttu-id="83ca5-117">Mapování sítě při povolení replikace</span><span class="sxs-lookup"><span data-stu-id="83ca5-117">Mapping network when enabling replication</span></span>

<span data-ttu-id="83ca5-118">Pokud mapování sítě neuděláte, když se replikace virtuálního počítače pro první čas formuláře jedné oblasti Azure do jiného, můžete cílové sítě v rámci stejného procesu.</span><span class="sxs-lookup"><span data-stu-id="83ca5-118">If network mapping is not done when you are replicating a virtual machine for the first time form one Azure region to another, then you can choose target network as part of the same process.</span></span> <span data-ttu-id="83ca5-119">Site Recovery vytvoří mapování sítě z oblasti zdrojové do cílové oblasti a cílová oblast zdroj oblasti založené na tomto výběru.</span><span class="sxs-lookup"><span data-stu-id="83ca5-119">Site Recovery creates network mappings from source region to target region and from target region to source region based on this selection.</span></span>   

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

<span data-ttu-id="83ca5-121">Ve výchozím nastavení, Site Recovery vytvoří síť v cílové oblasti, kde je stejný jako ke zdrojové síti a přidáním '-automatické obnovení systému ' jako příponu na název zdrojové síti.</span><span class="sxs-lookup"><span data-stu-id="83ca5-121">By default, Site Recovery creates a network in the target region that is identical to the source network and by adding '-asr' as a suffix to the name of the source network.</span></span> <span data-ttu-id="83ca5-122">Kliknutím na tlačítko Přizpůsobit můžete již vytvořené síťové.</span><span class="sxs-lookup"><span data-stu-id="83ca5-122">You can choose an already created network by clicking Customize.</span></span>

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)


<span data-ttu-id="83ca5-124">Pokud již probíhá mapování sítě, nelze změnit cíl virtuální síť při povolení replikace.</span><span class="sxs-lookup"><span data-stu-id="83ca5-124">If the network mapping is already done, you can't change the target virtual network while enabling replication.</span></span> <span data-ttu-id="83ca5-125">Chcete-li ji změnit, upravte existující mapování sítě.</span><span class="sxs-lookup"><span data-stu-id="83ca5-125">To change it, modify existing network mapping.</span></span>  

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> <span data-ttu-id="83ca5-128">Pokud změníte mapování sítě z oblasti-1 na 2 oblasti, nezapomeňte že upravit mapování sítě z oblasti 2 oblasti-1 také.</span><span class="sxs-lookup"><span data-stu-id="83ca5-128">If you modify a network mapping from region-1 to region-2, make sure you modify the network mapping from region-2 to region-1 as well.</span></span>
>
>


## <a name="subnet-selection"></a><span data-ttu-id="83ca5-129">Výběr podsítě</span><span class="sxs-lookup"><span data-stu-id="83ca5-129">Subnet selection</span></span>
<span data-ttu-id="83ca5-130">Podsíť cílového virtuálního počítače je vybrána na základě názvu podsíti zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="83ca5-130">Subnet of the target virtual machine is selected based on the name of the subnet of the source virtual machine.</span></span> <span data-ttu-id="83ca5-131">Pokud je k dispozici v cílové síti podsíť se stejným názvem jako zdrojový virtuální počítač, pak, je zvolen pro cílový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="83ca5-131">If there is a subnet of the same name as that of the source virtual machine available in the target network, then that is chosen for the target virtual machine.</span></span> <span data-ttu-id="83ca5-132">Pokud neexistuje žádná podsíť s tímto názvem v cílové síti, pak abecedně první podsíť je zvolen jako cílové podsíti.</span><span class="sxs-lookup"><span data-stu-id="83ca5-132">If there is no subnet with the same name in the target network, then alphabetically first subnet is chosen as the target subnet.</span></span> <span data-ttu-id="83ca5-133">Tato podsíť můžete upravit tak, že přejdete na výpočty a síť nastavení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="83ca5-133">You can modify this subnet by going to Compute and Network settings of the virtual machine.</span></span>

![Upravit podsíť](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a><span data-ttu-id="83ca5-135">IP adresa</span><span class="sxs-lookup"><span data-stu-id="83ca5-135">IP address</span></span>

<span data-ttu-id="83ca5-136">IP adresa pro každé síťové rozhraní cílového virtuálního počítače je zvolen následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="83ca5-136">IP address for each of the network interface of the target virtual machine is chosen as follows:</span></span>

### <a name="dhcp"></a><span data-ttu-id="83ca5-137">DHCP</span><span class="sxs-lookup"><span data-stu-id="83ca5-137">DHCP</span></span>
<span data-ttu-id="83ca5-138">Síťové rozhraní zdrojový virtuální počítač používá protokol DHCP, pak síťové rozhraní cílového virtuálního počítače je také nastavena jako DHCP.</span><span class="sxs-lookup"><span data-stu-id="83ca5-138">If the network interface of the source virtual machine is using DHCP, then network interface of the target virtual machine is also set as DHCP.</span></span>

### <a name="static-ip"></a><span data-ttu-id="83ca5-139">Statická IP adresa</span><span class="sxs-lookup"><span data-stu-id="83ca5-139">Static IP</span></span>
<span data-ttu-id="83ca5-140">Pokud síťové rozhraní zdrojový virtuální počítač používá statickou IP adresu, pak síťové rozhraní cílový virtuální počítač také nastaven na používá statickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="83ca5-140">If the network interface of the source virtual machine is using Static IP, then network interface of the target virtual machine is also set to use Static IP.</span></span> <span data-ttu-id="83ca5-141">Statická IP adresa je zvolen následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="83ca5-141">Static IP is chosen as follows:</span></span>

#### <a name="same-address-space"></a><span data-ttu-id="83ca5-142">Stejné adresní prostor</span><span class="sxs-lookup"><span data-stu-id="83ca5-142">Same address space</span></span>

<span data-ttu-id="83ca5-143">Podsíť zdrojové a cílové podsíti mají stejné adresní prostor, cílová IP adresa je nastavit stejné jako IP adresa síťového rozhraní zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="83ca5-143">If the source subnet and the target subnet have the same address space, then target IP is set same as the IP of  the network interface of the source virtual machine.</span></span> <span data-ttu-id="83ca5-144">Pokud není k dispozici stejnou IP Adresou, některé dostupnou IP adresu nastavena jako cílová IP adresa.</span><span class="sxs-lookup"><span data-stu-id="83ca5-144">If same IP is not available, then some other available IP is set as the target IP.</span></span>

#### <a name="different-address-space"></a><span data-ttu-id="83ca5-145">Jiným adresním prostorem</span><span class="sxs-lookup"><span data-stu-id="83ca5-145">Different address space</span></span>

<span data-ttu-id="83ca5-146">Pokud podsíť zdrojové a cílové podsíti jiným adresním prostorem, cílová IP adresa je nastaven jako dostupnou IP adresu v cílové podsíti.</span><span class="sxs-lookup"><span data-stu-id="83ca5-146">If the source subnet and the target subnet have different address space, then target IP is set as any available IP in the target subnet.</span></span>

<span data-ttu-id="83ca5-147">Cílová IP adresa na každé rozhraní sítě můžete upravit tak, že přejdete do nastavení výpočty a síť virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="83ca5-147">You can modify the target IP on each network interface by going to Compute and Network settings of the virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83ca5-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="83ca5-148">Next steps</span></span>

- <span data-ttu-id="83ca5-149">Další informace o [sítě pokyny pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="83ca5-149">Learn about [networking guidance for replicating Azure VMs](site-recovery-azure-to-azure-networking-guidance.md).</span></span>
