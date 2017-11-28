---
title: "aaaCreate virtuální počítač s statickou veřejnou IP adresu - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate virtuální počítač s statickou veřejnou IP adresu pomocí rozhraní příkazového řádku Azure (CLI) 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 55bc21b0-2a45-4943-a5e7-8d785d0d015c
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 486060463486462dd8336734a7ad23c4a2cba452
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-hello-azure-cli-20"></a><span data-ttu-id="48cdc-103">Vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="48cdc-103">Create a VM with a static public IP address using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="48cdc-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="48cdc-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="48cdc-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="48cdc-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="48cdc-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="48cdc-106">Azure CLI 2.0</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="48cdc-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="48cdc-107">Azure CLI 1.0</span></span>](virtual-network-deploy-static-pip-cli-nodejs.md)
> * [<span data-ttu-id="48cdc-108">Šablona</span><span class="sxs-lookup"><span data-stu-id="48cdc-108">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="48cdc-109">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="48cdc-109">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

<span data-ttu-id="48cdc-110">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="48cdc-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="48cdc-111">Tento článek se zabývá pomocí modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="48cdc-111">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <span data-ttu-id="48cdc-112"><a name = "create"></a>Vytvoření hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="48cdc-112"><a name = "create"></a>Create hello VM</span></span>

<span data-ttu-id="48cdc-113">Můžete dokončit tuto úlohu pomocí hello 2.0 rozhraní příkazového řádku Azure (v tomto článku) nebo hello [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="48cdc-113">You can complete this task using hello Azure CLI 2.0 (this article) or hello [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md).</span></span> <span data-ttu-id="48cdc-114">Hello hodnoty v "" pro proměnné hello hello kroky, které následují vytvořit prostředky s nastavením hello scénáře.</span><span class="sxs-lookup"><span data-stu-id="48cdc-114">hello values in "" for hello variables in hello steps that follow create resources with settings from hello scenario.</span></span> <span data-ttu-id="48cdc-115">Změnit hello hodnoty, jako je vhodné pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="48cdc-115">Change hello values, as appropriate, for your environment.</span></span>

1. <span data-ttu-id="48cdc-116">Nainstalujte hello [Azure CLI 2.0](/cli/azure/install-az-cli2) Pokud ještě nemáte nainstalováno.</span><span class="sxs-lookup"><span data-stu-id="48cdc-116">Install hello [Azure CLI 2.0](/cli/azure/install-az-cli2) if you don't already have it installed.</span></span>
2. <span data-ttu-id="48cdc-117">Vytvoření SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem pomocí kroků hello v hello [vytvoření SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="48cdc-117">Create an SSH public and private key pair for Linux VMs by completing hello steps in hello [Create an SSH public and private key pair for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
3. <span data-ttu-id="48cdc-118">Z příkazového prostředí, přihlášení pomocí příkazu hello `az login`.</span><span class="sxs-lookup"><span data-stu-id="48cdc-118">From a command shell, login with hello command `az login`.</span></span>
4. <span data-ttu-id="48cdc-119">Vytvořte spuštěním hello skript, který odpovídá na počítači se systémem Linux nebo Mac. hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="48cdc-119">Create hello VM by executing hello script that follows on a Linux or Mac computer.</span></span> <span data-ttu-id="48cdc-120">Hello Azure veřejnou IP adresu, virtuální sítě, síťové a prostředky virtuálních počítačů musí existovat v hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="48cdc-120">hello Azure public IP address, virtual network, network interface, and VM resources must all exist in hello same location.</span></span> <span data-ttu-id="48cdc-121">I když hello prostředky všechny nemají tooexist v hello stejnou skupinu prostředků, v hello následující skript dělají.</span><span class="sxs-lookup"><span data-stu-id="48cdc-121">Though hello resources don't all have tooexist in hello same resource group, in hello following script they do.</span></span>

```bash
RgName="IaaSStory"
Location="westus"

# Create a resource group.

az group create \
--name $RgName \
--location $Location

# Create a public IP address resource with a static IP address using hello --allocation-method Static option.
# If you do not specify this option, hello address is allocated dynamically. hello address is assigned toothe
# resource from a pool of IP adresses unique tooeach Azure region. hello DnsName must be unique within the
# Azure location it's created in. Download and view hello file from https://www.microsoft.com/en-us/download/details.aspx?id=41653#
# that lists hello ranges for each region.

PipName="PIPWEB1"
DnsName="iaasstoryws1"
az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--allocation-method Static \
--dns-name $DnsName

# Create a virtual network with one subnet

VnetName="TestVNet"
VnetPrefix="192.168.0.0/16"
SubnetName="FrontEnd"
SubnetPrefix="192.168.1.0/24"
az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $SubnetName \
--subnet-prefix $SubnetPrefix

# Create a network interface connected toohello VNet with a static private IP address and associate hello public IP address
# resource toohello NIC.

NicName="NICWEB1"
PrivateIpAddress="192.168.1.101"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $SubnetName \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress \
--public-ip-address $PipName

# Create a new VM with hello NIC

VmName="WEB1"

# Replace hello value for hello VmSize variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article.
VmSize="Standard_DS1"

# Replace hello value for hello OsImage variable with a value for *urn* from hello output returned by entering
# hello `az vm image list` command. 

OsImage="credativ:Debian:8:latest"
Username='adminuser'

# Replace hello following value with hello path tooyour public key file.
SshKeyValue="~/.ssh/id_rsa.pub"

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $NicName \
--admin-username $Username \
--ssh-key-value $SshKeyValue
# If creating a Windows VM, remove hello previous line and you'll be prompted for hello password you want tooconfigure for hello VM.
```

<span data-ttu-id="48cdc-122">Kromě toho toocreating virtuální počítač, hello skript vytvoří:</span><span class="sxs-lookup"><span data-stu-id="48cdc-122">In addition toocreating a VM, hello script creates:</span></span>
- <span data-ttu-id="48cdc-123">Jeden premium spravovat disku ve výchozím nastavení, ale máte další možnosti pro hello typ disku, které lze vytvořit.</span><span class="sxs-lookup"><span data-stu-id="48cdc-123">A single premium managed disk by default, but you have other options for hello disk type you can create.</span></span> <span data-ttu-id="48cdc-124">Čtení hello [vytvoření virtuálního počítače s Linuxem pomocí hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.</span><span class="sxs-lookup"><span data-stu-id="48cdc-124">Read hello [Create a Linux VM using hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article for details.</span></span>
- <span data-ttu-id="48cdc-125">Virtuální sítě, podsítě, seskupování a veřejnou IP adresu prostředky.</span><span class="sxs-lookup"><span data-stu-id="48cdc-125">Virtual network, subnet, NIC, and public IP address resources.</span></span> <span data-ttu-id="48cdc-126">Alternativně můžete použít *existující* virtuální sítě, podsítě, síťový adaptér nebo adresu prostředky veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="48cdc-126">Alternatively, you can use *existing* virtual network, subnet, NIC, or public IP address resources.</span></span> <span data-ttu-id="48cdc-127">toolearn jak toouse existující prostředky síťových místo vytvoření další zdroje informací, zadejte `az vm create -h`.</span><span class="sxs-lookup"><span data-stu-id="48cdc-127">toolearn how toouse existing network resources rather than creating additional resources, enter `az vm create -h`.</span></span>

## <span data-ttu-id="48cdc-128"><a name = "validate"></a>Ověření vytvoření virtuálních počítačů a veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="48cdc-128"><a name = "validate"></a>Validate VM creation and public IP address</span></span>

1. <span data-ttu-id="48cdc-129">Zadejte příkaz hello `az resource list --resouce-group IaaSStory --output table` toosee seznamu prostředků hello vytvořen skriptem hello.</span><span class="sxs-lookup"><span data-stu-id="48cdc-129">Enter hello command `az resource list --resouce-group IaaSStory --output table` toosee a list of hello resources created by hello script.</span></span> <span data-ttu-id="48cdc-130">Musí být pět prostředky vrátil výstup hello: Síťová rozhraní, disk, veřejnou IP adresu, virtuální sítě a virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="48cdc-130">There should be five resources in hello returned output: network interface, disk, public IP address, virtual network, and a virtual machine.</span></span>
2. <span data-ttu-id="48cdc-131">Zadejte příkaz hello `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`.</span><span class="sxs-lookup"><span data-stu-id="48cdc-131">Enter hello command `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`.</span></span> <span data-ttu-id="48cdc-132">Poznamenejte si hodnotu hello v hello vrátil výstup, **IpAddress** a že hodnota hello **PublicIpAllocationMethod** je *statické*.</span><span class="sxs-lookup"><span data-stu-id="48cdc-132">In hello returned output, note hello value of **IpAddress** and that hello value of **PublicIpAllocationMethod** is *Static*.</span></span>
3. <span data-ttu-id="48cdc-133">Před provedením hello následující příkaz, odeberte hello <>, nahraďte *uživatelské jméno* s názvem hello jste použili pro hello **uživatelské jméno** proměnné ve skriptu hello a nahradit *ipAddress* s hello **ipAddress** hello v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="48cdc-133">Before executing hello following command, remove hello <>, replace *Username* with hello name you used for hello **Username** variable in hello script, and replace *ipAddress* with hello **ipAddress** from hello previous step.</span></span> <span data-ttu-id="48cdc-134">Hello spusťte následující příkaz tooconnect toohello virtuálních počítačů: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span><span class="sxs-lookup"><span data-stu-id="48cdc-134">Run hello following command tooconnect toohello VM: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span></span> 

## <span data-ttu-id="48cdc-135"><a name= "clean-up"></a>Odebrat hello virtuálních počítačů a přidružených zdrojů.</span><span class="sxs-lookup"><span data-stu-id="48cdc-135"><a name= "clean-up"></a>Remove hello VM and associated resources</span></span>

<span data-ttu-id="48cdc-136">Doporučujeme vám odstranit hello prostředky vytvořené v tomto cvičení, pokud nebude je používat v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="48cdc-136">It's recommended that you delete hello resources created in this exercise if you won't use them in production.</span></span> <span data-ttu-id="48cdc-137">Virtuální počítač, veřejnou IP adresu a diskové prostředky platit poplatky, tak dlouho, dokud se jejich zřízení.</span><span class="sxs-lookup"><span data-stu-id="48cdc-137">VM, public IP address, and disk resources incur charges, as long as they're provisioned.</span></span> <span data-ttu-id="48cdc-138">prostředky hello tooremove vytvořené během tohoto cvičení dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="48cdc-138">tooremove hello resources created during this exercise, complete hello following steps:</span></span>

1. <span data-ttu-id="48cdc-139">tooview hello prostředky ve skupině prostředků hello, spusťte hello `az resource list --resource-group IaaSStory` příkaz.</span><span class="sxs-lookup"><span data-stu-id="48cdc-139">tooview hello resources in hello resource group, run hello `az resource list --resource-group IaaSStory` command.</span></span>
2. <span data-ttu-id="48cdc-140">Potvrďte, že neexistují žádné prostředky ve skupině prostředků hello, než hello prostředky vytvořen skriptem hello v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="48cdc-140">Confirm there are no resources in hello resource group, other than hello resources created by hello script in this article.</span></span> 
3. <span data-ttu-id="48cdc-141">všechny prostředky, které vytvoří v tomto cvičení spustit hello toodelete `az group delete -n IaaSStory` příkaz.</span><span class="sxs-lookup"><span data-stu-id="48cdc-141">toodelete all resources created in this exercise, run hello `az group delete -n IaaSStory` command.</span></span> <span data-ttu-id="48cdc-142">příkaz Hello odstraní skupinu prostředků hello a všechny hello prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="48cdc-142">hello command deletes hello resource group and all hello resources it contains.</span></span>

## <a name="next-steps"></a><span data-ttu-id="48cdc-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48cdc-143">Next steps</span></span>

<span data-ttu-id="48cdc-144">Síťové přenosy můžete procházet tooand z hello virtuálních počítačů vytvořena v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="48cdc-144">Any network traffic can flow tooand from hello VM created in this article.</span></span> <span data-ttu-id="48cdc-145">Můžete definovat příchozí a odchozí pravidla v rámci skupiny NSG, které omezují hello provoz, který může obtékat tooand z hello síťové rozhraní, hello podsíť nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="48cdc-145">You can define inbound and outbound rules within an NSG that limit hello traffic that can flow tooand from hello network interface, hello subnet, or both.</span></span> <span data-ttu-id="48cdc-146">Další informace o skupin Nsg, přečtěte si hello toolearn [NSG přehled](virtual-networks-nsg.md) článku.</span><span class="sxs-lookup"><span data-stu-id="48cdc-146">toolearn more about NSGs, read hello [NSG overview](virtual-networks-nsg.md) article.</span></span>
