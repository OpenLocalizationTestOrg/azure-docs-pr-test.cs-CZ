---
title: "aaaCreate virtuálního počítače s více síťovými kartami - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate virtuálního počítače s více síťovými kartami pomocí Azure CLI 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8e906a4b-8583-4a97-9416-ee34cfa09a98
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ac0291a978e2c8682c69104915196cc6c4fcf8dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-hello-azure-cli-20"></a><span data-ttu-id="5557b-103">Vytvoření virtuálního počítače s více síťovými kartami pomocí hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="5557b-103">Create a VM with multiple NICs using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="5557b-104">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5557b-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="5557b-105">Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](virtual-network-deploy-multinic-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="5557b-105">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](virtual-network-deploy-multinic-classic-cli.md).</span></span>
>

## <span data-ttu-id="5557b-106"><a name="create"></a>Vytvoření hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5557b-106"><a name="create"></a>Create hello VM</span></span>

<span data-ttu-id="5557b-107">Můžete dokončit tuto úlohu pomocí hello 2.0 rozhraní příkazového řádku Azure (v tomto článku) nebo hello [Azure CLI 1.0](virtual-network-deploy-multinic-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="5557b-107">You can complete this task using hello Azure CLI 2.0 (this article) or hello [Azure CLI 1.0](virtual-network-deploy-multinic-cli-nodejs.md).</span></span> <span data-ttu-id="5557b-108">Hello hodnoty v "" pro proměnné hello hello kroky, které následují vytvořit prostředky s nastavením hello scénáře.</span><span class="sxs-lookup"><span data-stu-id="5557b-108">hello values in "" for hello variables in hello steps that follow create resources with settings from hello scenario.</span></span> <span data-ttu-id="5557b-109">Změnit hello hodnoty, jako je vhodné pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="5557b-109">Change hello values, as appropriate, for your environment.</span></span>

1. <span data-ttu-id="5557b-110">Nainstalujte hello [Azure CLI 2.0](/cli/azure/install-az-cli2) Pokud ještě nemáte nainstalováno.</span><span class="sxs-lookup"><span data-stu-id="5557b-110">Install hello [Azure CLI 2.0](/cli/azure/install-az-cli2) if you don't already have it installed.</span></span>
2. <span data-ttu-id="5557b-111">Vytvoření SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem pomocí kroků hello v hello [vytvoření SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5557b-111">Create an SSH public and private key pair for Linux VMs by completing hello steps in hello [Create an SSH public and private key pair for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
3. <span data-ttu-id="5557b-112">Z příkazového prostředí, přihlášení pomocí příkazu hello `az login`.</span><span class="sxs-lookup"><span data-stu-id="5557b-112">From a command shell, login with hello command `az login`.</span></span>
4. <span data-ttu-id="5557b-113">Vytvořte spuštěním hello skript, který odpovídá na počítači se systémem Linux nebo Mac. hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5557b-113">Create hello VM by executing hello script that follows on a Linux or Mac computer.</span></span> <span data-ttu-id="5557b-114">skript Hello vytvoří skupinu prostředků jednu virtuální síť (VNet) se dvěma podsítěmi, dva síťové adaptéry a virtuální počítač s hello dva síťové adaptéry připojené tooit.</span><span class="sxs-lookup"><span data-stu-id="5557b-114">hello script creates a resource group, one virtual network (VNet) with two subnets, two NICs, and a VM with hello two NICs attached tooit.</span></span> <span data-ttu-id="5557b-115">Jeden z hello síťových adaptérů je připojených tooone podsíť a je přiřazena statická IP adresa veřejné a privátní.</span><span class="sxs-lookup"><span data-stu-id="5557b-115">One of hello NICs is connected tooone subnet and is assigned a static public and private IP address.</span></span> <span data-ttu-id="5557b-116">Hello dalších síťových Adaptérů je připojených toohello jiné podsítě a je přiřazen statickou privátní IP adresou a žádné veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="5557b-116">hello other NIC is connected toohello other subnet and is assigned a static private IP address and no public IP address.</span></span> <span data-ttu-id="5557b-117">Hello síťový adaptér, veřejnou IP adresu, virtuální sítě a prostředky virtuálních počítačů musí existovat v hello stejné umístění a předplatného.</span><span class="sxs-lookup"><span data-stu-id="5557b-117">hello NIC, public IP address, virtual network, and VM resources must all exist in hello same location and subscription.</span></span> <span data-ttu-id="5557b-118">I když hello prostředky všechny nemají tooexist v hello stejnou skupinu prostředků, v hello následující skript dělají.</span><span class="sxs-lookup"><span data-stu-id="5557b-118">Though hello resources don't all have tooexist in hello same resource group, in hello following script they do.</span></span>

```bash
#!/bin/sh

RgName="Multi-NIC-VM"
Location="westus"

# Create a resource group.
az group create \
--name $RgName \
--location $Location

# hello address is assigned toohello resource from a pool of IP adresses unique tooeach Azure region. 
# Download and view hello file from https://www.microsoft.com/en-us/download/details.aspx?id=41653 that lists
# hello ranges for each region.

PipName="PIP-WEB"
az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--allocation-method Static

# Create a virtual network with one subnet

VnetName="VNet1"
VnetPrefix="10.0.0.0/16"
VnetSubnet1Name="Front-End"
VnetSubnet1Prefix="10.0.0.0/24"
az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $VnetSubnet1Name \
--subnet-prefix $VnetSubnet1Prefix

# Create a second subnet within hello VNet

VnetSubnet2Name="Back-end"
VnetSubnet2Prefix="10.0.1.0/24"
az network vnet subnet create \
--vnet-name $VnetName \
--resource-group $RgName \
--name $VnetSubnet2Name \
--address-prefix $VnetSubnet2Prefix

# Create a network interface connected tooone of hello subnets. hello NIC is assigned a single dynamic private and
# public IP address by default, but you can instead, assign static addresses, or no public IP address at all.
# You can also assign multiple private or public IP addresses tooeach NIC. toolearn more about IP addressing
# options for NICs, enter hello az network nic create -h command.

Nic1Name="NIC-FE"
PrivateIpAddress1="10.0.0.5"

az network nic create \
--name $Nic1Name \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet1Name \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress1 \
--public-ip-address $PipName

# Create a second network interface and connect it toohello other subnet. Though multiple NICs attached toohello same
# VM can be connected toodifferent subnets, hello subnets must all be within hello same VNet. Add additional NICs as necessary.

Nic2Name="NIC-BE"
PrivateIpAddress2="10.0.1.5"

az network nic create \
--name $Nic2Name \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet2Name \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress2

# Create a VM and attach hello two NICs.

VmName="WEB"

# Replace hello value for hello following **VmSize** variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article. Not all VM sizes support
# more than one NIC, so be sure tooselect a VM size that supports hello number of NICs you want tooattach toohello VM.
# You must create hello VM with at least two NICs if you want tooadd more after VM creation. If you create a VM with
# only one NIC, you can't add additional NICs toohello VM after VM creation, regardless of how many NICs hello VM supports.
# hello VM size specified in hello following variable supports two NICs.

VmSize="Standard_DS2"

# Replace hello value for hello OsImage variable value with a value for *urn* from hello output returned by entering the
# az vm image list command.

OsImage="credativ:Debian:8:latest"

Username="adminuser"

# Replace hello following value with hello path tooyour public key file.

SshKeyValue="~/.ssh/id_rsa.pub"

# Before executing hello following command, add variable names of additional NICs you may have added toohello script that
# you want tooattach toohello VM. If creating a Windows VM, remove hello **ssh-key-value** line and you'll be prompted for
# hello password you want tooconfigure for hello VM.

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $Nic1Name $Nic2Name \
--admin-username $Username \
--ssh-key-value $SshKeyValue
```

<span data-ttu-id="5557b-119">Kromě toho toocreating virtuálních počítačů se dvěma síťovými adaptéry, hello skript vytvoří:</span><span class="sxs-lookup"><span data-stu-id="5557b-119">In addition toocreating a VM with two NICs, hello script creates:</span></span>
- <span data-ttu-id="5557b-120">Jeden premium spravovat disku ve výchozím nastavení, ale máte další možnosti pro hello typ disku, které lze vytvořit.</span><span class="sxs-lookup"><span data-stu-id="5557b-120">A single premium managed disk by default, but you have other options for hello disk type you can create.</span></span> <span data-ttu-id="5557b-121">Čtení hello [vytvoření virtuálního počítače s Linuxem pomocí hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.</span><span class="sxs-lookup"><span data-stu-id="5557b-121">Read hello [Create a Linux VM using hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article for details.</span></span>
- <span data-ttu-id="5557b-122">Virtuální síť se dvěma podsítěmi a jednu veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="5557b-122">A virtual network with two subnets and a single public IP address.</span></span> <span data-ttu-id="5557b-123">Alternativně můžete použít *existující* virtuální sítě, podsítě, síťový adaptér nebo adresu prostředky veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="5557b-123">Alternatively, you can use *existing* virtual network, subnet, NIC, or public IP address resources.</span></span> <span data-ttu-id="5557b-124">toolearn jak toouse existující prostředky síťových místo vytvoření další zdroje informací, zadejte `az vm create -h`.</span><span class="sxs-lookup"><span data-stu-id="5557b-124">toolearn how toouse existing network resources rather than creating additional resources, enter `az vm create -h`.</span></span>

## <span data-ttu-id="5557b-125"><a name = "validate"></a>Ověření vytvoření virtuálních počítačů a síťových karet</span><span class="sxs-lookup"><span data-stu-id="5557b-125"><a name = "validate"></a>Validate VM creation and NICs</span></span>

1. <span data-ttu-id="5557b-126">Zadejte příkaz hello `az resource list --resouce-group Multi-NIC-VM --output table` toosee seznamu prostředků hello vytvořen skriptem hello.</span><span class="sxs-lookup"><span data-stu-id="5557b-126">Enter hello command `az resource list --resouce-group Multi-NIC-VM --output table` toosee a list of hello resources created by hello script.</span></span> <span data-ttu-id="5557b-127">Musí být šesti prostředky vrátil výstup hello: dva síťové adaptéry, jeden disk, jednu veřejnou IP adresu, jednu virtuální síť a virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5557b-127">There should be six resources in hello returned output: Two NICs, one disk, one public IP address, one virtual network, and a virtual machine.</span></span>
2. <span data-ttu-id="5557b-128">Zadejte příkaz hello `az network public-ip show --name PIP-WEB --resource-group Multi-NIC-VM --output table`.</span><span class="sxs-lookup"><span data-stu-id="5557b-128">Enter hello command `az network public-ip show --name PIP-WEB --resource-group Multi-NIC-VM --output table`.</span></span> <span data-ttu-id="5557b-129">Poznamenejte si hodnotu hello v hello vrátil výstup, **IpAddress** a že hodnota hello **PublicIpAllocationMethod** je *statické*.</span><span class="sxs-lookup"><span data-stu-id="5557b-129">In hello returned output, note hello value of **IpAddress** and that hello value of **PublicIpAllocationMethod** is *Static*.</span></span>
3. <span data-ttu-id="5557b-130">Než spustíte následující příkaz hello, odeberte hello <>, nahraďte *uživatelské jméno* s názvem hello jste použili pro hello **uživatelské jméno** proměnné ve skriptu hello a nahradit *ipAddress* s hello **ipAddress** hello v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="5557b-130">Before running hello following command, remove hello <>, replace *Username* with hello name you used for hello **Username** variable in hello script, and replace *ipAddress* with hello **ipAddress** from hello previous step.</span></span> <span data-ttu-id="5557b-131">Hello spusťte následující příkaz tooconnect toohello virtuálních počítačů: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span><span class="sxs-lookup"><span data-stu-id="5557b-131">Run hello following command tooconnect toohello VM: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span></span> 
4. <span data-ttu-id="5557b-132">Po připojení toohello virtuálního počítače, spusťte hello `sudo ifconfig` příkaz toosee *eth0* a *eth1* rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5557b-132">Once connected toohello VM, run hello `sudo ifconfig` command toosee *eth0* and *eth1* interfaces.</span></span> <span data-ttu-id="5557b-133">Každý síťový adaptér byl přiřazen hello statickou privátní IP adresy zadané ve skriptu hello servery Azure DHCP hello.</span><span class="sxs-lookup"><span data-stu-id="5557b-133">Each NIC has been assigned hello static private IP addresses specified in hello script by hello Azure DHCP servers.</span></span> <span data-ttu-id="5557b-134">Hello IP a MAC adres, které jsou přiřazené síťové adaptéry toohello neměňte dokud hello odstranění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5557b-134">hello IP and MAC addresses assigned toohello NICs do not change until hello VM is deleted.</span></span> <span data-ttu-id="5557b-135">Doporučujeme vám, že měnit IP adresy v rámci operačního systému, jak ji můžete vypnout počítač toohello připojení.</span><span class="sxs-lookup"><span data-stu-id="5557b-135">We recommend that you do not change IP addressing within an operating system, as it can disable connectivity toohello computer.</span></span> <span data-ttu-id="5557b-136">Veřejné IP adresy se nezobrazí v rámci hello operačního systému, jako jsou síťové adresy přeložit tooand z hello privátní IP adresy pomocí hello infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="5557b-136">Public IP addresses do not appear within hello operating system, as they are network address translated tooand from hello private IP address by hello Azure infrastructure.</span></span>

## <span data-ttu-id="5557b-137"><a name= "clean-up"></a>Odebrat hello virtuálních počítačů a přidružených zdrojů.</span><span class="sxs-lookup"><span data-stu-id="5557b-137"><a name= "clean-up"></a>Remove hello VM and associated resources</span></span>

<span data-ttu-id="5557b-138">Doporučujeme vám odstranit hello prostředky vytvořené v tomto cvičení, pokud nebude je používat v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5557b-138">It's recommended that you delete hello resources created in this exercise if you won't use them in production.</span></span> <span data-ttu-id="5557b-139">Virtuální počítač, veřejnou IP adresu a diskové prostředky platit poplatky, tak dlouho, dokud se jejich zřízení.</span><span class="sxs-lookup"><span data-stu-id="5557b-139">VM, public IP address, and disk resources incur charges, as long as they're provisioned.</span></span> <span data-ttu-id="5557b-140">prostředky hello tooremove vytvořené během tohoto cvičení dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5557b-140">tooremove hello resources created during this exercise, complete hello following steps:</span></span>
1. <span data-ttu-id="5557b-141">tooview hello prostředky ve skupině prostředků hello, spusťte hello `az resource list --resource-group Multi-NIC-VM` příkaz.</span><span class="sxs-lookup"><span data-stu-id="5557b-141">tooview hello resources in hello resource group, run hello `az resource list --resource-group Multi-NIC-VM` command.</span></span>
2. <span data-ttu-id="5557b-142">Potvrďte, že neexistují žádné prostředky ve skupině prostředků hello, než hello prostředky vytvořen skriptem hello v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="5557b-142">Confirm there are no resources in hello resource group, other than hello resources created by hello script in this article.</span></span> 
3. <span data-ttu-id="5557b-143">všechny prostředky, které vytvoří v tomto cvičení spustit hello toodelete `az group delete --name Multi-NIC-VM` příkaz.</span><span class="sxs-lookup"><span data-stu-id="5557b-143">toodelete all resources created in this exercise, run hello `az group delete --name Multi-NIC-VM` command.</span></span> <span data-ttu-id="5557b-144">příkaz Hello odstraní skupinu prostředků hello a všechny hello prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="5557b-144">hello command deletes hello resource group and all hello resources it contains.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5557b-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5557b-145">Next steps</span></span>

<span data-ttu-id="5557b-146">Síťové přenosy můžete procházet tooand z hello virtuálních počítačů vytvořena v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="5557b-146">Any network traffic can flow tooand from hello VM created in this article.</span></span> <span data-ttu-id="5557b-147">Můžete definovat příchozí a odchozí pravidla v rámci skupiny NSG, které omezí hello provoz, který může obtékat tooand z každé síťové rozhraní, každou podsíť nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="5557b-147">You can define inbound and outbound rules within an NSG that limit hello traffic that can flow tooand from each network interface, each subnet, or both.</span></span> <span data-ttu-id="5557b-148">Další informace o skupin Nsg, přečtěte si hello toolearn [NSG přehled](virtual-networks-nsg.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5557b-148">toolearn more about NSGs, read hello [NSG overview](virtual-networks-nsg.md) article.</span></span>
