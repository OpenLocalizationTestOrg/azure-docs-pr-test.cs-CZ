---
title: "Vytvoření virtuálního počítače (klasické) s více síťovými kartami pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak vytvořit a nakonfigurovat virtuální počítače s více síťovými kartami pomocí prostředí PowerShell."
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
ms.openlocfilehash: 68ccc1cac22e593b099729fe68c6bee63df44d9b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-classic-with-multiple-nics"></a><span data-ttu-id="decce-103">Vytvoření virtuálního počítače (klasické) s více síťovými kartami</span><span class="sxs-lookup"><span data-stu-id="decce-103">Create a VM (Classic) with multiple NICs</span></span>
<span data-ttu-id="decce-104">Můžete vytvořit virtuální počítače (VM) v Azure a připojit více síťových rozhraní (NIC) ke každému z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="decce-104">You can create virtual machines (VMs) in Azure and attach multiple network interfaces (NICs) to each of your VMs.</span></span> <span data-ttu-id="decce-105">Několik síťových adaptérů jsou nutné pro mnoho síťových virtuálních zařízení, například doručení aplikace a řešení optimalizace sítě WAN.</span><span class="sxs-lookup"><span data-stu-id="decce-105">Multiple NICs are a requirement for many network virtual appliances, such as application delivery and WAN optimization solutions.</span></span> <span data-ttu-id="decce-106">Několik síťových adaptérů také poskytují izolaci provozu mezi síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="decce-106">Multiple NICs also provide isolation of traffic between NICs.</span></span>

![Síťový adaptér více virtuálních počítačů](./media/virtual-networks-multiple-nics/IC757773.png)

<span data-ttu-id="decce-108">Obrázek zobrazuje virtuálního počítače se síťovými adaptéry, tři, že každý z nich připojený k jiné podsíti.</span><span class="sxs-lookup"><span data-stu-id="decce-108">The figure shows a VM with three NICs, each connected to a different subnet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="decce-109">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="decce-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="decce-110">Tento článek se věnuje použití klasického modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="decce-110">This article covers using the classic deployment model.</span></span> <span data-ttu-id="decce-111">Společnost Microsoft doporučuje, aby většina nových nasazení používala Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="decce-111">Microsoft recommends that most new deployments use Resource Manager.</span></span>

* <span data-ttu-id="decce-112">Internetový VIP (nasazení classic) je podporován pouze na síťový adaptér "default".</span><span class="sxs-lookup"><span data-stu-id="decce-112">Internet-facing VIP (classic deployments) is only supported on the "default" NIC.</span></span> <span data-ttu-id="decce-113">Existuje pouze jeden virtuální IP adresy IP na výchozí síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="decce-113">There is only one VIP to the IP of the default NIC.</span></span>
* <span data-ttu-id="decce-114">V tomto okamžiku nejsou podporovány adresy Instance úroveň veřejné IP (LPIP) (nasazení classic) pro více virtuálních počítačů síťovou kartu.</span><span class="sxs-lookup"><span data-stu-id="decce-114">At this time, Instance Level Public IP (LPIP) addresses (classic deployments) are not supported for multi NIC VMs.</span></span>
* <span data-ttu-id="decce-115">Pořadí síťových rozhraní ve virtuálním počítači je náhodné a může se také měnit následkem aktualizací infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="decce-115">The order of the NICs from inside the VM will be random, and could also change across Azure infrastructure updates.</span></span> <span data-ttu-id="decce-116">Ale IP adresy a odpovídající ethernetová adresa MAC adres se zůstávají stejné.</span><span class="sxs-lookup"><span data-stu-id="decce-116">However, the IP addresses, and the corresponding ethernet MAC addresses will remain the same.</span></span> <span data-ttu-id="decce-117">Předpokládejme například, **Eth1** 10.1.0.100 IP adresu a adresu MAC 00-0D-3A-B0-39-0D; po aktualizaci infrastruktury Azure a restartování, může změnit tak, aby **Eth2**, ale IP a MAC párování zůstane stejný.</span><span class="sxs-lookup"><span data-stu-id="decce-117">For example, assume **Eth1** has IP address 10.1.0.100 and MAC address 00-0D-3A-B0-39-0D; after an Azure infrastructure update and reboot, it could be changed to **Eth2**, but the IP and MAC pairing will remain the same.</span></span> <span data-ttu-id="decce-118">Pokud se restartování spouštěná zákazníka, zůstane stejný pořadí síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="decce-118">When a restart is customer-initiated, the NIC order will remain the same.</span></span>
* <span data-ttu-id="decce-119">Adresa pro každý síťový adaptér na každý virtuální počítač se musí nacházet v podsíti, několik síťových adaptérů na jeden virtuální počítač můžete ke každé možné přiřadit adresy, které jsou ve stejné podsíti.</span><span class="sxs-lookup"><span data-stu-id="decce-119">The address for each NIC on each VM must be located in a subnet, multiple NICs on a single VM can each be assigned addresses that are in the same subnet.</span></span>
* <span data-ttu-id="decce-120">Velikost virtuálního počítače určuje počet síťových ADAPTÉRŮ, který můžete vytvořit pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="decce-120">The VM size determines the number of NICS that you can create for a VM.</span></span> <span data-ttu-id="decce-121">Odkaz [systému Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) články k určení, kolik SÍŤOVÝCH každý velikost virtuálního počítače podporuje velikostí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="decce-121">Reference the [Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) VM sizes articles to determine how many NICS each VM size supports.</span></span> 

## <a name="network-security-groups-nsgs"></a><span data-ttu-id="decce-122">Skupiny zabezpečení sítě (Nsg)</span><span class="sxs-lookup"><span data-stu-id="decce-122">Network Security Groups (NSGs)</span></span>
<span data-ttu-id="decce-123">V nasazení Resource Manager může být všechny síťové adaptéry na virtuálním počítači související s skupina zabezpečení sítě (NSG), včetně všech síťových adaptérů na virtuálním počítači, který má několik síťových adaptérů povoleno.</span><span class="sxs-lookup"><span data-stu-id="decce-123">In a Resource Manager deployment, any NIC on a VM may be associated with a Network Security Group (NSG), including any NICs on a VM that has multiple NICs enabled.</span></span> <span data-ttu-id="decce-124">Pokud síťový adaptér je přiřazena adresa v podsíti, kde je přidružen skupinu NSG podsítě, potom pravidla v NSG podsítě platí i pro tuto síťovou kartu.</span><span class="sxs-lookup"><span data-stu-id="decce-124">If a NIC is assigned an address within a subnet where the subnet is associated with an NSG, then the rules in the subnet’s NSG also apply to that NIC.</span></span> <span data-ttu-id="decce-125">Kromě přidružení podsítí k skupin Nsg, lze přiřadit síťový adaptér s skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="decce-125">In addition to associating subnets with NSGs, you can also associate a NIC with an NSG.</span></span>

<span data-ttu-id="decce-126">Pokud podsíť je přidružen skupinu NSG a síťovou kartu v této podsíti jednotlivě souvisí s skupinu NSG, přidružená pravidla NSG se použijí v **toku pořadí** podle provoz předávány do nebo z síťový adaptér:</span><span class="sxs-lookup"><span data-stu-id="decce-126">If a subnet is associated with an NSG, and a NIC within that subnet is individually associated with an NSG, the associated NSG rules are applied in **flow order** according to the direction of the traffic being passed into or out of the NIC:</span></span>

* <span data-ttu-id="decce-127">**Příchozí provoz** jehož cílem je síťový adaptér v otázku nejprve prochází podsíť, která aktivuje pravidla NSG podsítě, před předáním do síťového adaptéru a potom aktivuje pravidla NSG na síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="decce-127">**Incoming traffic** whose destination is the NIC in question flows first through the subnet, triggering the subnet’s NSG rules, before passing into the NIC, then triggering the NIC’s NSG rules.</span></span>
* <span data-ttu-id="decce-128">**Odchozí přenosy** jejichž zdrojem je na síťový adaptér toků první ze ze síťového adaptéru, spouštění pravidla NSG na síťový adaptér, před prošla podsíť a potom aktivuje pravidla NSG podsítě.</span><span class="sxs-lookup"><span data-stu-id="decce-128">**Outgoing traffic** whose source is the NIC in question flows first out from the NIC, triggering the NIC’s NSG rules, before passing through the subnet, then triggering the subnet’s NSG rules.</span></span>

<span data-ttu-id="decce-129">Další informace o [skupin zabezpečení sítě](virtual-networks-nsg.md) a jak se používají podle přidružení podsítě virtuálních počítačů a síťových karet...</span><span class="sxs-lookup"><span data-stu-id="decce-129">Learn more about [Network Security Groups](virtual-networks-nsg.md) and how they are applied based on associations to subnets, VMs, and NICs..</span></span>

## <a name="how-to-configure-a-multi-nic-vm-in-a-classic-deployment"></a><span data-ttu-id="decce-130">Jak nakonfigurovat více virtuálních počítačů síťovou kartu v nasazení classic</span><span class="sxs-lookup"><span data-stu-id="decce-130">How to Configure a multi NIC VM in a classic deployment</span></span>
<span data-ttu-id="decce-131">Následující pokyny vám pomůže vytvořit více virtuálních počítačů síťovou kartu obsahující 3 síťové adaptéry: výchozí síťový adaptér a další dva síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="decce-131">The instructions below will help you create a multi NIC VM containing 3 NICs: a default NIC and two additional NICs.</span></span> <span data-ttu-id="decce-132">Postup konfigurace vytvoří virtuální počítač, který bude nakonfigurovaný podle fragment souboru konfigurace služby níže:</span><span class="sxs-lookup"><span data-stu-id="decce-132">The configuration steps will create a VM that will be configured according to the service configuration file fragment below:</span></span>

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
    … Skip over the remainder section …
    </VirtualNetworkSite>


<span data-ttu-id="decce-133">Před pokusem o spuštění příkazů prostředí PowerShell v příkladu potřebujete následující požadavky.</span><span class="sxs-lookup"><span data-stu-id="decce-133">You need the following prerequisites before trying to run the PowerShell commands in the example.</span></span>

* <span data-ttu-id="decce-134">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="decce-134">An Azure subscription.</span></span>
* <span data-ttu-id="decce-135">Nakonfigurované virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="decce-135">A configured virtual network.</span></span> <span data-ttu-id="decce-136">V tématu [Přehled virtuálních sítí](virtual-networks-overview.md) Další informace o virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="decce-136">See [Virtual Network Overview](virtual-networks-overview.md) for more information about VNets.</span></span>
* <span data-ttu-id="decce-137">Nejnovější verzi prostředí Azure PowerShell stáhnout a nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="decce-137">The latest version of Azure PowerShell downloaded and installed.</span></span> <span data-ttu-id="decce-138">Viz téma [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="decce-138">See [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="decce-139">Vytvoření virtuálního počítače s více síťovými kartami, proveďte následující kroky zadáním každý příkaz v rámci jedné relace prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="decce-139">To create a VM with multiple NICs, complete the following steps by entering each command within a single PowerShell session:</span></span>

1. <span data-ttu-id="decce-140">Vyberte bitovou kopii virtuálního počítače z Galerie obrázků virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="decce-140">Select a VM image from Azure VM image gallery.</span></span> <span data-ttu-id="decce-141">Všimněte si, že Image často mění a jsou dostupné podle oblasti.</span><span class="sxs-lookup"><span data-stu-id="decce-141">Note that images change frequently and are available by region.</span></span> <span data-ttu-id="decce-142">Image zadaná v následujícím příkladu nesmí změnit nebo může být ve vaší oblasti, takže je nutné zadat bitovou kopii, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="decce-142">The image specified in the example below may change or might not be in your region, so be sure to specify the image you need.</span></span>

    ```powershell
    $image = Get-AzureVMImage `
    -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"
    ```

2. <span data-ttu-id="decce-143">Vytvoření konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="decce-143">Create a VM configuration.</span></span>

    ```powershell
    $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
    -Image $image.ImageName –AvailabilitySetName "MyAVSet"
    ```

3. <span data-ttu-id="decce-144">Vytvoří výchozí přihlášení správce.</span><span class="sxs-lookup"><span data-stu-id="decce-144">Create the default administrator login.</span></span>

    ```powershell
    Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
    -Password "<YourAdminPassword>"
    ```

4. <span data-ttu-id="decce-145">Přidejte další síťové adaptéry ke konfiguraci virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="decce-145">Add additional NICs to the VM configuration.</span></span>

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
    -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
    Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
    -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm
    ```

5. <span data-ttu-id="decce-146">Zadejte podsíť a IP adresu pro síťový adaptér výchozí.</span><span class="sxs-lookup"><span data-stu-id="decce-146">Specify the subnet and IP address for the default NIC.</span></span>

    ```powershell
    Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
    Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm
    ```

6. <span data-ttu-id="decce-147">Vytvoření virtuálního počítače ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="decce-147">Create the VM in your virtual network.</span></span>

    ```powershell
    New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm
    ```

    > [!NOTE]
    > <span data-ttu-id="decce-148">Virtuální síť, který zde určíte již musí existovat (jak je uvedeno v požadavky).</span><span class="sxs-lookup"><span data-stu-id="decce-148">The VNet that you specify here must already exist (as mentioned in the prerequisites).</span></span> <span data-ttu-id="decce-149">Následující příklad určuje virtuální síť s názvem **MultiNIC-VNet**.</span><span class="sxs-lookup"><span data-stu-id="decce-149">The example below specifies a virtual network named **MultiNIC-VNet**.</span></span>
    >

## <a name="limitations"></a><span data-ttu-id="decce-150">Omezení</span><span class="sxs-lookup"><span data-stu-id="decce-150">Limitations</span></span>
<span data-ttu-id="decce-151">Při použití více síťových adaptérů platí následující omezení:</span><span class="sxs-lookup"><span data-stu-id="decce-151">The following limitations are applicable when using multiple NICs:</span></span>

* <span data-ttu-id="decce-152">Virtuální počítače s více síťovými kartami, musí být vytvořený v Azure virtuální sítě (virtuální sítě).</span><span class="sxs-lookup"><span data-stu-id="decce-152">VMs with multiple NICs must be created in Azure virtual networks (VNets).</span></span> <span data-ttu-id="decce-153">Virtuální počítače non-VNet se nedá nakonfigurovat se několik síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="decce-153">Non-VNet VMs cannot be configured with multiple NICs.</span></span>
* <span data-ttu-id="decce-154">Všechny virtuální počítače v nastavení dostupnosti je potřeba použít několik síťových adaptérů nebo jeden síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="decce-154">All VMs in an availability set need to use either multiple NICs or a single NIC.</span></span> <span data-ttu-id="decce-155">Nemůže mít směs více virtuální síťový adaptér počítače a jeden síťový adaptér virtuálních počítačů v rámci skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="decce-155">There cannot be a mixture of multiple NIC VMs and single NIC VMs within an availability set.</span></span> <span data-ttu-id="decce-156">Stejná pravidla použít pro virtuální počítače v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="decce-156">Same rules apply for VMs in a cloud service.</span></span> <span data-ttu-id="decce-157">Pro víc virtuálních počítačů síťovou kartu nemusí mít stejný počet síťových adaptérů, tak dlouho, dokud každá má alespoň dvě.</span><span class="sxs-lookup"><span data-stu-id="decce-157">For multiple NIC VMs, they aren't required to have the same number of NICs, as long as they each have at least two.</span></span>
* <span data-ttu-id="decce-158">Virtuální počítač s jednu síťovou kartu nelze konfigurovat pomocí více síťových adaptérů (a naopak) po jejím nasazení, bez odstranit a znovu ji vytvořit.</span><span class="sxs-lookup"><span data-stu-id="decce-158">A VM with a single NIC cannot be configured with multi NICs (and vice-versa) once it is deployed, without deleting and re-creating it.</span></span>

## <a name="secondary-nics-access-to-other-subnets"></a><span data-ttu-id="decce-159">Sekundární síťové adaptéry přístup do jiných podsítí</span><span class="sxs-lookup"><span data-stu-id="decce-159">Secondary NICs access to other subnets</span></span>
<span data-ttu-id="decce-160">Ve výchozím nastavení se nenakonfigurují sekundární síťové adaptéry s výchozí bránou, kvůli které bude tok přenosů na sekundární síťové adaptéry omezenou být ve stejné podsíti.</span><span class="sxs-lookup"><span data-stu-id="decce-160">By default secondary NICs will not be configured with a default gateway, due to which the traffic flow on the secondary NICs will be limited to be within the same subnet.</span></span> <span data-ttu-id="decce-161">Pokud uživatele chcete povolit sekundární síťové adaptéry, aby komunikoval mimo vlastní podsíti, se bude nutné přidat položku do směrovací tabulky, pokud chcete konfigurovat bránu, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="decce-161">If the users want to enable secondary NICs to talk outside their own subnet, they will need to add an entry in the routing table to configure the gateway as described below.</span></span>

> [!NOTE]
> <span data-ttu-id="decce-162">Virtuální počítače vytvořené před července 2015 může mít výchozí brána konfigurována pro všechny síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="decce-162">VMs created before July 2015 may have a default gateway configured for all NICs.</span></span> <span data-ttu-id="decce-163">Výchozí brána pro sekundární síťové adaptéry budou odebrány až tyto virtuální počítače se restartují.</span><span class="sxs-lookup"><span data-stu-id="decce-163">The default gateway for secondary NICs will not be removed until these VMs are rebooted.</span></span> <span data-ttu-id="decce-164">V operačních systémech, které používají model směrování slabé hostitele, jako je například Linux můžete přerušení připojení k Internetu, pokud příchozí a odchozí provoz používat různé síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="decce-164">In Operating systems that use the weak host routing model, such as Linux, Internet connectivity can break if the ingress and egress traffic use different NICs.</span></span>
> 

### <a name="configure-windows-vms"></a><span data-ttu-id="decce-165">Konfigurace virtuálních počítačů Windows</span><span class="sxs-lookup"><span data-stu-id="decce-165">Configure Windows VMs</span></span>
<span data-ttu-id="decce-166">Předpokládejme, že máte virtuální počítač s Windows se dvěma síťovými adaptéry, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="decce-166">Suppose that you have a Windows VM with two NICs as follows:</span></span>

* <span data-ttu-id="decce-167">Primární síťovou kartu IP adresa: 192.168.1.4</span><span class="sxs-lookup"><span data-stu-id="decce-167">Primary NIC IP address: 192.168.1.4</span></span>
* <span data-ttu-id="decce-168">Sekundární síťový adaptér IP adresa: 192.168.2.5</span><span class="sxs-lookup"><span data-stu-id="decce-168">Secondary NIC IP address: 192.168.2.5</span></span>

<span data-ttu-id="decce-169">Tabulka směrování IPv4 pro tento virtuální počítač bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="decce-169">The IPv4 route table for this VM would look like this:</span></span>

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

<span data-ttu-id="decce-170">Všimněte si, že výchozí trasa (0.0.0.0) je dostupná jenom na primární síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="decce-170">Notice that the default route (0.0.0.0) is only available to the primary NIC.</span></span> <span data-ttu-id="decce-171">Nebudete mít přístup k prostředkům mimo podsíť sekundárního síťového adaptéru, jak vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="decce-171">You will not be able to access resources outside the subnet for the secondary NIC, as seen below:</span></span>

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

<span data-ttu-id="decce-172">Přidat výchozí trasu na sekundární síťový adaptér, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="decce-172">To add a default route on the secondary NIC, follow the steps below:</span></span>

1. <span data-ttu-id="decce-173">Z příkazového řádku spusťte následující příkaz k identifikaci číslo indexu pro sekundární síťový adaptér:</span><span class="sxs-lookup"><span data-stu-id="decce-173">From a command prompt, run the command below to identify the index number for the secondary NIC:</span></span>
   
        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================
2. <span data-ttu-id="decce-174">Všimněte si, druhý záznam v tabulce s indexem 27 (v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="decce-174">Notice the second entry in the table, with an index of 27 (in this example).</span></span>
3. <span data-ttu-id="decce-175">Z příkazového řádku, spusťte **přidat trasy** příkaz, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="decce-175">From the command prompt, run the **route add** command as shown below.</span></span> <span data-ttu-id="decce-176">V tomto příkladu jsou určení 192.168.2.1 jako výchozí brána pro sekundární síťový adaptér:</span><span class="sxs-lookup"><span data-stu-id="decce-176">In this example, you are specifying 192.168.2.1 as the default gateway for the secondary NIC:</span></span>
   
        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27
4. <span data-ttu-id="decce-177">K testování připojení, přejděte zpět na příkazovém řádku a zkuste příkazem ping otestovat jiné podsíti z sekundárního síťového adaptéru jako uvedené int eh následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="decce-177">To test connectivity, go back to the command prompt and try to ping a different subnet from the secondary NIC as shown int eh example below:</span></span>
   
        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5
   
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
5. <span data-ttu-id="decce-178">Můžete také zkontrolovat tabulky tras Zkontrolujte nově přidané postup, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="decce-178">You can also check your route table to check the newly added route, as shown below:</span></span>
   
        C:\Users\Administrator>route print
   
        ...
   
        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a><span data-ttu-id="decce-179">Konfigurovat virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="decce-179">Configure Linux VMs</span></span>
<span data-ttu-id="decce-180">Pro virtuální počítače s Linuxem protože používá výchozí chování slabé hostitele směrování, doporučujeme, aby sekundární síťové adaptéry jsou omezeny na přenosové toky pouze ve stejné podsíti.</span><span class="sxs-lookup"><span data-stu-id="decce-180">For Linux VMs, since the default behavior uses weak host routing, we recommend that the secondary NICs are restricted to traffic flows only within the same subnet.</span></span> <span data-ttu-id="decce-181">Pokud některé scénáře potřebují připojení mimo podsíť, ale měli uživatelé povolit na základě zásad směrování zajistit, že příchozí a odchozí provoz používá stejný síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="decce-181">However if certain scenarios demand connectivity outside the subnet, users should enable policy based routing to ensure that the ingress and egress traffic uses the same NIC.</span></span>

## <a name="next-steps"></a><span data-ttu-id="decce-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="decce-182">Next steps</span></span>
* <span data-ttu-id="decce-183">Nasazení [MultiNIC virtuálních počítačů v aplikaci na vrstvě 2 scénář v nasazení Resource Manager](virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="decce-183">Deploy [MultiNIC VMs in a 2-tier application scenario in a Resource Manager deployment](virtual-network-deploy-multinic-arm-template.md).</span></span>
* <span data-ttu-id="decce-184">Nasazení [MultiNIC virtuálních počítačů v aplikaci na vrstvě 2 scénář v nasazení classic](virtual-network-deploy-multinic-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="decce-184">Deploy [MultiNIC VMs in a 2-tier application scenario in a classic deployment](virtual-network-deploy-multinic-classic-ps.md).</span></span>

