---
title: "aaaCreate virtuální počítač (klasický) s více síťovými kartami pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate a konfigurace virtuálních počítačů s více síťovými kartami pomocí prostředí PowerShell."
services: virtual-network, virtual-machines
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: a1a3952c-2dcc-4977-bd7a-52d623c1fb07
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 8ef35bd4cfd7e6a527080f1cfc541275ca86f5e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics"></a><span data-ttu-id="3a7cd-103">Vytvoření virtuálního počítače (klasické) s více síťovými kartami</span><span class="sxs-lookup"><span data-stu-id="3a7cd-103">Create a VM (Classic) with multiple NICs</span></span>
<span data-ttu-id="3a7cd-104">Můžete vytvořit virtuální počítače (VM) v Azure a připojit více síťových rozhraní (NIC) tooeach vaše virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-104">You can create virtual machines (VMs) in Azure and attach multiple network interfaces (NICs) tooeach of your VMs.</span></span> <span data-ttu-id="3a7cd-105">Několik síťových adaptérů jsou nutné pro mnoho síťových virtuálních zařízení, například doručení aplikace a řešení optimalizace sítě WAN.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-105">Multiple NICs are a requirement for many network virtual appliances, such as application delivery and WAN optimization solutions.</span></span> <span data-ttu-id="3a7cd-106">Několik síťových adaptérů také poskytují izolaci provozu mezi síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-106">Multiple NICs also provide isolation of traffic between NICs.</span></span>

![Síťový adaptér více virtuálních počítačů](./media/virtual-networks-multiple-nics/IC757773.png)

<span data-ttu-id="3a7cd-108">Zobrazí obrázek Hello virtuálního počítače se síťovými adaptéry, tři, každý z nich připojený tooa jiné podsíti.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-108">hello figure shows a VM with three NICs, each connected tooa different subnet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3a7cd-109">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="3a7cd-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="3a7cd-110">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-110">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="3a7cd-111">Společnost Microsoft doporučuje, aby většina nových nasazení používala Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-111">Microsoft recommends that most new deployments use Resource Manager.</span></span>

* <span data-ttu-id="3a7cd-112">Internetový VIP (nasazení classic) je podporován pouze na hello "Výchozí" síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-112">Internet-facing VIP (classic deployments) is only supported on hello "default" NIC.</span></span> <span data-ttu-id="3a7cd-113">Existuje jenom jedna IP adresa VIP toohello z hello výchozí síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-113">There is only one VIP toohello IP of hello default NIC.</span></span>
* <span data-ttu-id="3a7cd-114">V tomto okamžiku nejsou podporovány adresy Instance úroveň veřejné IP (LPIP) (nasazení classic) pro více virtuálních počítačů síťovou kartu.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-114">At this time, Instance Level Public IP (LPIP) addresses (classic deployments) are not supported for multi NIC VMs.</span></span>
* <span data-ttu-id="3a7cd-115">Hello pořadí síťových karet hello z uvnitř hello virtuálního počítače budou náhodné a také můžete změnit napříč aktualizace infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-115">hello order of hello NICs from inside hello VM will be random, and could also change across Azure infrastructure updates.</span></span> <span data-ttu-id="3a7cd-116">Ale hello IP adresy a hello odpovídající ethernetová adresa MAC adresy zůstane stejný hello.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-116">However, hello IP addresses, and hello corresponding ethernet MAC addresses will remain hello same.</span></span> <span data-ttu-id="3a7cd-117">Předpokládejme například, **Eth1** 10.1.0.100 IP adresu a adresu MAC 00-0D-3A-B0-39-0D; po aktualizaci infrastruktury Azure a restartování, se může změnit příliš**Eth2**, ale hello IP a MAC párování bude zůstat stejné hello.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-117">For example, assume **Eth1** has IP address 10.1.0.100 and MAC address 00-0D-3A-B0-39-0D; after an Azure infrastructure update and reboot, it could be changed too**Eth2**, but hello IP and MAC pairing will remain hello same.</span></span> <span data-ttu-id="3a7cd-118">Když je restartování spouštěná zákazníka, hello seskupování pořadí zůstane stejný hello.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-118">When a restart is customer-initiated, hello NIC order will remain hello same.</span></span>
* <span data-ttu-id="3a7cd-119">Hello adresa pro každý síťový adaptér na každý virtuální počítač se musí nacházet v podsíti, několik síťových adaptérů v rámci jednoho virtuálního počítače lze každou přiřadit hello adresy, které jsou ve stejné podsíti.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-119">hello address for each NIC on each VM must be located in a subnet, multiple NICs on a single VM can each be assigned addresses that are in hello same subnet.</span></span>
* <span data-ttu-id="3a7cd-120">Hello velikost virtuálního počítače určuje hello počet síťových ADAPTÉRŮ, který můžete vytvořit pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-120">hello VM size determines hello number of NICS that you can create for a VM.</span></span> <span data-ttu-id="3a7cd-121">Referenční dokumentace hello [systému Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) virtuálních počítačů velikostí články toodetermine kolik síťových karet podporuje každý velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-121">Reference hello [Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) VM sizes articles toodetermine how many NICS each VM size supports.</span></span> 

## <a name="network-security-groups-nsgs"></a><span data-ttu-id="3a7cd-122">Skupiny zabezpečení sítě (Nsg)</span><span class="sxs-lookup"><span data-stu-id="3a7cd-122">Network Security Groups (NSGs)</span></span>
<span data-ttu-id="3a7cd-123">V nasazení Resource Manager může být všechny síťové adaptéry na virtuálním počítači související s skupina zabezpečení sítě (NSG), včetně všech síťových adaptérů na virtuálním počítači, který má několik síťových adaptérů povoleno.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-123">In a Resource Manager deployment, any NIC on a VM may be associated with a Network Security Group (NSG), including any NICs on a VM that has multiple NICs enabled.</span></span> <span data-ttu-id="3a7cd-124">Pokud síťový adaptér je přiřazena adresa v podsíti, kde je skupina NSG přidružená hello podsíť, pak hello pravidla v podsíti hello NSG, budou platit toothat síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-124">If a NIC is assigned an address within a subnet where hello subnet is associated with an NSG, then hello rules in hello subnet’s NSG also apply toothat NIC.</span></span> <span data-ttu-id="3a7cd-125">V přidání podsítě tooassociating pomocí skupin Nsg lze přiřadit síťový adaptér s skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-125">In addition tooassociating subnets with NSGs, you can also associate a NIC with an NSG.</span></span>

<span data-ttu-id="3a7cd-126">Pokud podsíť je přidružen skupinu NSG a síťovou kartu v této podsíti jednotlivě souvisí s skupinu NSG, pravidla NSG hello spojené se použijí v **toku pořadí** podle toohello směr provozu hello předávány do nebo z Hello síťovou kartu:</span><span class="sxs-lookup"><span data-stu-id="3a7cd-126">If a subnet is associated with an NSG, and a NIC within that subnet is individually associated with an NSG, hello associated NSG rules are applied in **flow order** according toohello direction of hello traffic being passed into or out of hello NIC:</span></span>

* <span data-ttu-id="3a7cd-127">**Příchozí provoz** jehož cílem je hello síťový adaptér v otázku nejprve prochází hello podsíť, aktivuje pravidla NSG hello podsíť, před předávání do hello síťový adaptér a potom aktivuje pravidla NSG hello seskupování.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-127">**Incoming traffic** whose destination is hello NIC in question flows first through hello subnet, triggering hello subnet’s NSG rules, before passing into hello NIC, then triggering hello NIC’s NSG rules.</span></span>
* <span data-ttu-id="3a7cd-128">**Odchozí přenosy** jejichž zdrojem je hello seskupování dotyčném toků první ze z hello síťového adaptéru, spouštění pravidla NSG hello seskupování, před prošla hello podsítě a potom aktivuje pravidla NSG hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-128">**Outgoing traffic** whose source is hello NIC in question flows first out from hello NIC, triggering hello NIC’s NSG rules, before passing through hello subnet, then triggering hello subnet’s NSG rules.</span></span>

<span data-ttu-id="3a7cd-129">Další informace o [skupin zabezpečení sítě](virtual-networks-nsg.md) a jak se používají na základě přidružení toosubnets, virtuálních počítačů a síťových karet...</span><span class="sxs-lookup"><span data-stu-id="3a7cd-129">Learn more about [Network Security Groups](virtual-networks-nsg.md) and how they are applied based on associations toosubnets, VMs, and NICs..</span></span>

## <a name="how-tooconfigure-a-multi-nic-vm-in-a-classic-deployment"></a><span data-ttu-id="3a7cd-130">Jak tooConfigure více virtuálních počítačů síťovou kartu v nasazení classic</span><span class="sxs-lookup"><span data-stu-id="3a7cd-130">How tooConfigure a multi NIC VM in a classic deployment</span></span>
<span data-ttu-id="3a7cd-131">níže uvedené pokyny Hello vám pomůže vytvořit více virtuálních počítačů síťovou kartu obsahující 3 síťové adaptéry: výchozí síťový adaptér a další dva síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-131">hello instructions below will help you create a multi NIC VM containing 3 NICs: a default NIC and two additional NICs.</span></span> <span data-ttu-id="3a7cd-132">Postup konfigurace Hello vytvoří virtuální počítač, který bude nakonfigurován podle toohello služby konfigurační soubor fragment níže:</span><span class="sxs-lookup"><span data-stu-id="3a7cd-132">hello configuration steps will create a VM that will be configured according toohello service configuration file fragment below:</span></span>

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over hello remainder section …
    </VirtualNetworkSite>


<span data-ttu-id="3a7cd-133">Budete potřebovat následující požadavky a teprve potom zkusili toorun hello příkazy prostředí PowerShell v příkladu hello hello.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-133">You need hello following prerequisites before trying toorun hello PowerShell commands in hello example.</span></span>

* <span data-ttu-id="3a7cd-134">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-134">An Azure subscription.</span></span>
* <span data-ttu-id="3a7cd-135">Nakonfigurované virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-135">A configured virtual network.</span></span> <span data-ttu-id="3a7cd-136">V tématu [Přehled virtuálních sítí](virtual-networks-overview.md) Další informace o virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-136">See [Virtual Network Overview](virtual-networks-overview.md) for more information about VNets.</span></span>
* <span data-ttu-id="3a7cd-137">nejnovější verzi prostředí Azure PowerShell Hello stáhnout a nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-137">hello latest version of Azure PowerShell downloaded and installed.</span></span> <span data-ttu-id="3a7cd-138">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3a7cd-138">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="3a7cd-139">toocreate virtuálního počítače s více síťovými kartami, dokončení hello zadáním každý příkaz v rámci jedné relace prostředí PowerShell následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3a7cd-139">toocreate a VM with multiple NICs, complete hello following steps by entering each command within a single PowerShell session:</span></span>

1. <span data-ttu-id="3a7cd-140">Vyberte bitovou kopii virtuálního počítače z Galerie obrázků virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-140">Select a VM image from Azure VM image gallery.</span></span> <span data-ttu-id="3a7cd-141">Všimněte si, že Image často mění a jsou dostupné podle oblasti.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-141">Note that images change frequently and are available by region.</span></span> <span data-ttu-id="3a7cd-142">Hello image zadanou v následujícím příkladu hello nesmí změnit nebo může být ve vaší oblasti, takže je nutné toospecify hello bitové kopie je nutné.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-142">hello image specified in hello example below may change or might not be in your region, so be sure toospecify hello image you need.</span></span>

    ```powershell
    $image = Get-AzureVMImage `
    -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"
    ```

2. <span data-ttu-id="3a7cd-143">Vytvoření konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-143">Create a VM configuration.</span></span>

    ```powershell
    $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
    -Image $image.ImageName –AvailabilitySetName "MyAVSet"
    ```

3. <span data-ttu-id="3a7cd-144">Vytvořte přihlašovací údaje správce výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-144">Create hello default administrator login.</span></span>

    ```powershell
    Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
    -Password "<YourAdminPassword>"
    ```

4. <span data-ttu-id="3a7cd-145">Přidejte další konfigurace virtuálního počítače toohello síťových karet.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-145">Add additional NICs toohello VM configuration.</span></span>

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
    -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
    Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
    -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm
    ```

5. <span data-ttu-id="3a7cd-146">Zadejte hello podsíť a IP adresu pro hello výchozí síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-146">Specify hello subnet and IP address for hello default NIC.</span></span>

    ```powershell
    Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
    Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm
    ```

6. <span data-ttu-id="3a7cd-147">Vytvořte hello virtuálního počítače ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-147">Create hello VM in your virtual network.</span></span>

    ```powershell
    New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm
    ```

    > [!NOTE]
    > <span data-ttu-id="3a7cd-148">Hello virtuální sítě, který zde určíte již musí existovat (jak je uvedeno v hello požadavky).</span><span class="sxs-lookup"><span data-stu-id="3a7cd-148">hello VNet that you specify here must already exist (as mentioned in hello prerequisites).</span></span> <span data-ttu-id="3a7cd-149">Následující příklad Hello Určuje virtuální síť s názvem **MultiNIC-VNet**.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-149">hello example below specifies a virtual network named **MultiNIC-VNet**.</span></span>
    >

## <a name="limitations"></a><span data-ttu-id="3a7cd-150">Omezení</span><span class="sxs-lookup"><span data-stu-id="3a7cd-150">Limitations</span></span>
<span data-ttu-id="3a7cd-151">Při použití více síťových adaptérů platí Hello následující omezení:</span><span class="sxs-lookup"><span data-stu-id="3a7cd-151">hello following limitations are applicable when using multiple NICs:</span></span>

* <span data-ttu-id="3a7cd-152">Virtuální počítače s více síťovými kartami, musí být vytvořený v Azure virtuální sítě (virtuální sítě).</span><span class="sxs-lookup"><span data-stu-id="3a7cd-152">VMs with multiple NICs must be created in Azure virtual networks (VNets).</span></span> <span data-ttu-id="3a7cd-153">Virtuální počítače non-VNet se nedá nakonfigurovat se několik síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-153">Non-VNet VMs cannot be configured with multiple NICs.</span></span>
* <span data-ttu-id="3a7cd-154">Všechny virtuální počítače ve skupině dostupnosti nastavena toouse nutné několik síťových adaptérů nebo jeden síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-154">All VMs in an availability set need toouse either multiple NICs or a single NIC.</span></span> <span data-ttu-id="3a7cd-155">Nemůže mít směs více virtuální síťový adaptér počítače a jeden síťový adaptér virtuálních počítačů v rámci skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-155">There cannot be a mixture of multiple NIC VMs and single NIC VMs within an availability set.</span></span> <span data-ttu-id="3a7cd-156">Stejná pravidla použít pro virtuální počítače v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-156">Same rules apply for VMs in a cloud service.</span></span> <span data-ttu-id="3a7cd-157">Pro víc virtuálních počítačů síťovou kartu, nejsou požadované toohave hello stejný počet síťových adaptérů, tak dlouho, dokud každá má alespoň dvě.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-157">For multiple NIC VMs, they aren't required toohave hello same number of NICs, as long as they each have at least two.</span></span>
* <span data-ttu-id="3a7cd-158">Virtuální počítač s jednu síťovou kartu nelze konfigurovat pomocí více síťových adaptérů (a naopak) po jejím nasazení, bez odstranit a znovu ji vytvořit.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-158">A VM with a single NIC cannot be configured with multi NICs (and vice-versa) once it is deployed, without deleting and re-creating it.</span></span>

## <a name="secondary-nics-access-tooother-subnets"></a><span data-ttu-id="3a7cd-159">Sekundární síťové adaptéry přístup tooother podsítě</span><span class="sxs-lookup"><span data-stu-id="3a7cd-159">Secondary NICs access tooother subnets</span></span>
<span data-ttu-id="3a7cd-160">Ve výchozím nastavení sekundární síťové adaptéry se nenakonfigurují s výchozí bránou, z důvodu toowhich hello přenosový tok v hello sekundární síťové adaptéry bude omezený toobe v rámci hello stejné podsíti.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-160">By default secondary NICs will not be configured with a default gateway, due toowhich hello traffic flow on hello secondary NICs will be limited toobe within hello same subnet.</span></span> <span data-ttu-id="3a7cd-161">Pokud uživatel hello tooenable sekundární síťové adaptéry tootalk mimo vlastní podsíti, potřebují tooadd položku v hello směrovací tabulky tooconfigure hello brány jako popsané dole.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-161">If hello users want tooenable secondary NICs tootalk outside their own subnet, they will need tooadd an entry in hello routing table tooconfigure hello gateway as described below.</span></span>

> [!NOTE]
> <span data-ttu-id="3a7cd-162">Virtuální počítače vytvořené před července 2015 může mít výchozí brána konfigurována pro všechny síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-162">VMs created before July 2015 may have a default gateway configured for all NICs.</span></span> <span data-ttu-id="3a7cd-163">Hello výchozí brána pro sekundární síťové adaptéry budou odebrány až tyto virtuální počítače se restartují.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-163">hello default gateway for secondary NICs will not be removed until these VMs are rebooted.</span></span> <span data-ttu-id="3a7cd-164">V operačních systémech, které používají model, směrování hello slabé hostitele například Linux můžete přerušení připojení k Internetu, pokud hello příchozí a odchozí provoz používat různé síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-164">In Operating systems that use hello weak host routing model, such as Linux, Internet connectivity can break if hello ingress and egress traffic use different NICs.</span></span>
> 

### <a name="configure-windows-vms"></a><span data-ttu-id="3a7cd-165">Konfigurace virtuálních počítačů Windows</span><span class="sxs-lookup"><span data-stu-id="3a7cd-165">Configure Windows VMs</span></span>
<span data-ttu-id="3a7cd-166">Předpokládejme, že máte virtuální počítač s Windows se dvěma síťovými adaptéry, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3a7cd-166">Suppose that you have a Windows VM with two NICs as follows:</span></span>

* <span data-ttu-id="3a7cd-167">Primární síťovou kartu IP adresa: 192.168.1.4</span><span class="sxs-lookup"><span data-stu-id="3a7cd-167">Primary NIC IP address: 192.168.1.4</span></span>
* <span data-ttu-id="3a7cd-168">Sekundární síťový adaptér IP adresa: 192.168.2.5</span><span class="sxs-lookup"><span data-stu-id="3a7cd-168">Secondary NIC IP address: 192.168.2.5</span></span>

<span data-ttu-id="3a7cd-169">Tabulka směrování IPv4 Hello pro tento virtuální počítač bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="3a7cd-169">hello IPv4 route table for this VM would look like this:</span></span>

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

<span data-ttu-id="3a7cd-170">Všimněte si, že tento hello výchozí trasa (0.0.0.0) je pouze k dispozici toohello primární síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-170">Notice that hello default route (0.0.0.0) is only available toohello primary NIC.</span></span> <span data-ttu-id="3a7cd-171">Nebudete moct tooaccess prostředky mimo hello podsíť pro hello sekundární síťový adaptér, jak vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="3a7cd-171">You will not be able tooaccess resources outside hello subnet for hello secondary NIC, as seen below:</span></span>

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

<span data-ttu-id="3a7cd-172">výchozí směrování na tooadd hello sekundární síťový adaptér, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="3a7cd-172">tooadd a default route on hello secondary NIC, follow hello steps below:</span></span>

1. <span data-ttu-id="3a7cd-173">Z příkazového řádku, spusťte příkaz hello níže číslo indexu hello tooidentify hello sekundární síťovou kartu:</span><span class="sxs-lookup"><span data-stu-id="3a7cd-173">From a command prompt, run hello command below tooidentify hello index number for hello secondary NIC:</span></span>
   
        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================
2. <span data-ttu-id="3a7cd-174">Všimněte si hello druhý záznam v tabulce hello, s indexem 27 (v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="3a7cd-174">Notice hello second entry in hello table, with an index of 27 (in this example).</span></span>
3. <span data-ttu-id="3a7cd-175">Z příkazového řádku hello, spusťte hello **přidat trasy** příkaz, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-175">From hello command prompt, run hello **route add** command as shown below.</span></span> <span data-ttu-id="3a7cd-176">V tomto příkladu jsou určení 192.168.2.1 jako hello výchozí brána pro hello sekundární síťovou kartu:</span><span class="sxs-lookup"><span data-stu-id="3a7cd-176">In this example, you are specifying 192.168.2.1 as hello default gateway for hello secondary NIC:</span></span>
   
        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27
4. <span data-ttu-id="3a7cd-177">tootest připojení, přejděte zpět toohello příkazového řádku a zkuste to tooping z jiné podsítě hello sekundární síťový adaptér jako uvedené int eh příkladu níže:</span><span class="sxs-lookup"><span data-stu-id="3a7cd-177">tootest connectivity, go back toohello command prompt and try tooping a different subnet from hello secondary NIC as shown int eh example below:</span></span>
   
        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5
   
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
5. <span data-ttu-id="3a7cd-178">Můžete také zkontrolovat, že vaše trasy tabulky toocheck hello nově přidaná postup, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="3a7cd-178">You can also check your route table toocheck hello newly added route, as shown below:</span></span>
   
        C:\Users\Administrator>route print
   
        ...
   
        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a><span data-ttu-id="3a7cd-179">Konfigurovat virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="3a7cd-179">Configure Linux VMs</span></span>
<span data-ttu-id="3a7cd-180">Pro virtuální počítače s Linuxem, protože výchozí chování hello používá slabé hostitele směrování, doporučujeme, abyste tento hello sekundární síťové adaptéry jsou toky s omezeným přístupem tootraffic pouze v rámci hello stejné podsíti.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-180">For Linux VMs, since hello default behavior uses weak host routing, we recommend that hello secondary NICs are restricted tootraffic flows only within hello same subnet.</span></span> <span data-ttu-id="3a7cd-181">Ale pokud určité scénáře potřebují připojení mimo hello podsíť, uživatelé měli povolit tooensure směrování na základě zásad, která hello příchozí a odchozí provoz používá hello stejné síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="3a7cd-181">However if certain scenarios demand connectivity outside hello subnet, users should enable policy based routing tooensure that hello ingress and egress traffic uses hello same NIC.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a7cd-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3a7cd-182">Next steps</span></span>
* <span data-ttu-id="3a7cd-183">Nasazení [MultiNIC virtuálních počítačů v aplikaci na vrstvě 2 scénář v nasazení Resource Manager](virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="3a7cd-183">Deploy [MultiNIC VMs in a 2-tier application scenario in a Resource Manager deployment](virtual-network-deploy-multinic-arm-template.md).</span></span>
* <span data-ttu-id="3a7cd-184">Nasazení [MultiNIC virtuálních počítačů v aplikaci na vrstvě 2 scénář v nasazení classic](virtual-network-deploy-multinic-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="3a7cd-184">Deploy [MultiNIC VMs in a 2-tier application scenario in a classic deployment](virtual-network-deploy-multinic-classic-ps.md).</span></span>

