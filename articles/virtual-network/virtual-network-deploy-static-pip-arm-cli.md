---
title: "Vytvoření virtuálního počítače se statickou veřejnou IP adresu - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se vytvořit virtuální počítač se statickou veřejnou IP adresu pomocí rozhraní příkazového řádku Azure (CLI) 2.0."
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
ms.openlocfilehash: a4c32694949880037f01bb2b6b9779d2cbb9809c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-cli-20"></a><span data-ttu-id="f8e6d-103">Vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f8e6d-103">Create a VM with a static public IP address using the Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f8e6d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f8e6d-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="f8e6d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f8e6d-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="f8e6d-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f8e6d-106">Azure CLI 2.0</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="f8e6d-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f8e6d-107">Azure CLI 1.0</span></span>](virtual-network-deploy-static-pip-cli-nodejs.md)
> * [<span data-ttu-id="f8e6d-108">Šablona</span><span class="sxs-lookup"><span data-stu-id="f8e6d-108">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="f8e6d-109">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="f8e6d-109">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

<span data-ttu-id="f8e6d-110">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f8e6d-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="f8e6d-111">Tento článek se zabývá pomocí modelu nasazení Resource Manager, které společnost Microsoft doporučuje pro většinu nových nasazení místo modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-111">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <span data-ttu-id="f8e6d-112"><a name = "create"></a>Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="f8e6d-112"><a name = "create"></a>Create the VM</span></span>

<span data-ttu-id="f8e6d-113">Vám může tuto úlohu dokončit pomocí Azure CLI 2.0 (v tomto článku) nebo [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="f8e6d-113">You can complete this task using the Azure CLI 2.0 (this article) or the [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md).</span></span> <span data-ttu-id="f8e6d-114">Hodnoty v "" pro proměnné v krocích, které následují vytvořit prostředky s nastaveními z scénáře.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-114">The values in "" for the variables in the steps that follow create resources with settings from the scenario.</span></span> <span data-ttu-id="f8e6d-115">Změňte hodnoty, jako je vhodné pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-115">Change the values, as appropriate, for your environment.</span></span>

1. <span data-ttu-id="f8e6d-116">Nainstalujte [Azure CLI 2.0](/cli/azure/install-az-cli2) Pokud ještě nemáte nainstalováno.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-116">Install the [Azure CLI 2.0](/cli/azure/install-az-cli2) if you don't already have it installed.</span></span>
2. <span data-ttu-id="f8e6d-117">Vytvoření SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem pomocí kroků v [vytvoření SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f8e6d-117">Create an SSH public and private key pair for Linux VMs by completing the steps in the [Create an SSH public and private key pair for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
3. <span data-ttu-id="f8e6d-118">Z příkazového prostředí, přihlášení pomocí příkazu `az login`.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-118">From a command shell, login with the command `az login`.</span></span>
4. <span data-ttu-id="f8e6d-119">Vytvoření virtuálního počítače tak, že spustíte skript, který odpovídá na počítači se systémem Linux nebo Mac..</span><span class="sxs-lookup"><span data-stu-id="f8e6d-119">Create the VM by executing the script that follows on a Linux or Mac computer.</span></span> <span data-ttu-id="f8e6d-120">Azure veřejnou IP adresu, virtuální sítě, síťové a prostředky virtuálních počítačů musí všechny existovat ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-120">The Azure public IP address, virtual network, network interface, and VM resources must all exist in the same location.</span></span> <span data-ttu-id="f8e6d-121">Když prostředky nebudete mít existovat ve stejné skupině prostředků, v následující skript dělají.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-121">Though the resources don't all have to exist in the same resource group, in the following script they do.</span></span>

```bash
RgName="IaaSStory"
Location="westus"

# Create a resource group.

az group create \
--name $RgName \
--location $Location

# Create a public IP address resource with a static IP address using the --allocation-method Static option.
# If you do not specify this option, the address is allocated dynamically. The address is assigned to the
# resource from a pool of IP adresses unique to each Azure region. The DnsName must be unique within the
# Azure location it's created in. Download and view the file from https://www.microsoft.com/en-us/download/details.aspx?id=41653#
# that lists the ranges for each region.

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

# Create a network interface connected to the VNet with a static private IP address and associate the public IP address
# resource to the NIC.

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

# Create a new VM with the NIC

VmName="WEB1"

# Replace the value for the VmSize variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article.
VmSize="Standard_DS1"

# Replace the value for the OsImage variable with a value for *urn* from the output returned by entering
# the `az vm image list` command. 

OsImage="credativ:Debian:8:latest"
Username='adminuser'

# Replace the following value with the path to your public key file.
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
# If creating a Windows VM, remove the previous line and you'll be prompted for the password you want to configure for the VM.
```

<span data-ttu-id="f8e6d-122">Kromě vytvoření virtuálního počítače, vytvoří skript:</span><span class="sxs-lookup"><span data-stu-id="f8e6d-122">In addition to creating a VM, the script creates:</span></span>
- <span data-ttu-id="f8e6d-123">Jeden premium spravovat disku ve výchozím nastavení, ale máte další možnosti pro typ disku, které lze vytvořit.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-123">A single premium managed disk by default, but you have other options for the disk type you can create.</span></span> <span data-ttu-id="f8e6d-124">Pro čtení [vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-124">Read the [Create a Linux VM using the Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article for details.</span></span>
- <span data-ttu-id="f8e6d-125">Virtuální sítě, podsítě, seskupování a veřejnou IP adresu prostředky.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-125">Virtual network, subnet, NIC, and public IP address resources.</span></span> <span data-ttu-id="f8e6d-126">Alternativně můžete použít *existující* virtuální sítě, podsítě, síťový adaptér nebo adresu prostředky veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-126">Alternatively, you can use *existing* virtual network, subnet, NIC, or public IP address resources.</span></span> <span data-ttu-id="f8e6d-127">Chcete-li další informace o použití existující síťovým prostředkům, nikoli vytváření další zdroje informací, zadejte `az vm create -h`.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-127">To learn how to use existing network resources rather than creating additional resources, enter `az vm create -h`.</span></span>

## <span data-ttu-id="f8e6d-128"><a name = "validate"></a>Ověření vytvoření virtuálních počítačů a veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="f8e6d-128"><a name = "validate"></a>Validate VM creation and public IP address</span></span>

1. <span data-ttu-id="f8e6d-129">Zadejte příkaz `az resource list --resouce-group IaaSStory --output table` zobrazíte seznam prostředky vytvořené skriptem.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-129">Enter the command `az resource list --resouce-group IaaSStory --output table` to see a list of the resources created by the script.</span></span> <span data-ttu-id="f8e6d-130">Měla by existovat pět prostředky ve vrácené výstupu: Síťová rozhraní, disk, veřejnou IP adresu, virtuální sítě a virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-130">There should be five resources in the returned output: network interface, disk, public IP address, virtual network, and a virtual machine.</span></span>
2. <span data-ttu-id="f8e6d-131">Zadejte příkaz `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-131">Enter the command `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`.</span></span> <span data-ttu-id="f8e6d-132">Ve vrácené výstupu si poznamenejte hodnotu **IpAddress** a že hodnota **PublicIpAllocationMethod** je *statické*.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-132">In the returned output, note the value of **IpAddress** and that the value of **PublicIpAllocationMethod** is *Static*.</span></span>
3. <span data-ttu-id="f8e6d-133">Před spuštěním následujícího příkazu, odeberte <>, nahraďte *uživatelské jméno* s názvem, který jste použili pro **uživatelské jméno** proměnné ve skriptu a nahraďte *ipAddress*s **ipAddress** z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-133">Before executing the following command, remove the <>, replace *Username* with the name you used for the **Username** variable in the script, and replace *ipAddress* with the **ipAddress** from the previous step.</span></span> <span data-ttu-id="f8e6d-134">Spusťte následující příkaz pro připojení k virtuálnímu počítači: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-134">Run the following command to connect to the VM: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`.</span></span> 

## <span data-ttu-id="f8e6d-135"><a name= "clean-up"></a>Odebrat virtuální počítač a přidružených zdrojů.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-135"><a name= "clean-up"></a>Remove the VM and associated resources</span></span>

<span data-ttu-id="f8e6d-136">Doporučujeme vám odstranit prostředky vytvořené v tomto cvičení, pokud nebude je používat v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-136">It's recommended that you delete the resources created in this exercise if you won't use them in production.</span></span> <span data-ttu-id="f8e6d-137">Virtuální počítač, veřejnou IP adresu a diskové prostředky platit poplatky, tak dlouho, dokud se jejich zřízení.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-137">VM, public IP address, and disk resources incur charges, as long as they're provisioned.</span></span> <span data-ttu-id="f8e6d-138">Odebrat prostředky vytvořené během tohoto cvičení, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f8e6d-138">To remove the resources created during this exercise, complete the following steps:</span></span>

1. <span data-ttu-id="f8e6d-139">Chcete-li zobrazit prostředky ve skupině prostředků, spusťte `az resource list --resource-group IaaSStory` příkaz.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-139">To view the resources in the resource group, run the `az resource list --resource-group IaaSStory` command.</span></span>
2. <span data-ttu-id="f8e6d-140">Potvrďte, že neexistují žádné prostředky ve skupině prostředků, než prostředky vytvořené pomocí skriptu, který v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-140">Confirm there are no resources in the resource group, other than the resources created by the script in this article.</span></span> 
3. <span data-ttu-id="f8e6d-141">Chcete-li odstranit všechny prostředky, které jsou vytvořené v tomto cvičení, spusťte `az group delete -n IaaSStory` příkaz.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-141">To delete all resources created in this exercise, run the `az group delete -n IaaSStory` command.</span></span> <span data-ttu-id="f8e6d-142">Příkaz odstraní skupinu prostředků a všechny prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-142">The command deletes the resource group and all the resources it contains.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8e6d-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f8e6d-143">Next steps</span></span>

<span data-ttu-id="f8e6d-144">Síťové přenosy můžete procházet do a z virtuálního počítače vytvořit v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-144">Any network traffic can flow to and from the VM created in this article.</span></span> <span data-ttu-id="f8e6d-145">Můžete definovat příchozí a odchozí pravidla v rámci skupiny NSG, které omezit přenos, který může obtékat do a z rozhraní sítě, podsítě nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-145">You can define inbound and outbound rules within an NSG that limit the traffic that can flow to and from the network interface, the subnet, or both.</span></span> <span data-ttu-id="f8e6d-146">Další informace o skupinách Nsg, najdete [NSG přehled](virtual-networks-nsg.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f8e6d-146">To learn more about NSGs, read the [NSG overview](virtual-networks-nsg.md) article.</span></span>