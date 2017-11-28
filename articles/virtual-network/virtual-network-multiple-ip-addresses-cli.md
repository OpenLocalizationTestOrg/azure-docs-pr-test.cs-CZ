---
title: "aaaVM s více IP adres pomocí hello 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak tooassign více IP adres tooa virtuálního počítače pomocí hello 2.0 rozhraní příkazového řádku Azure | Správce prostředků."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: annahar
ms.openlocfilehash: 15efd853cc7c31bacb64ed052dabedd3fe4d3079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-hello-azure-cli-20"></a><span data-ttu-id="1952b-103">Přiřadit více IP adres toovirtual počítačů pomocí hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="1952b-103">Assign multiple IP addresses toovirtual machines using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

<span data-ttu-id="1952b-104">Tento článek vysvětluje, jak toocreate virtuální počítač (VM) prostřednictvím pomocí modelu nasazení Azure Resource Manager hello hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="1952b-104">This article explains how toocreate a virtual machine (VM) through hello Azure Resource Manager deployment model using hello Azure CLI 2.0.</span></span> <span data-ttu-id="1952b-105">Tooresources vytvořené pomocí modelu nasazení classic hello nelze přiřadit více IP adres.</span><span class="sxs-lookup"><span data-stu-id="1952b-105">Multiple IP addresses cannot be assigned tooresources created through hello classic deployment model.</span></span> <span data-ttu-id="1952b-106">Další informace o modelech nasazení Azure, přečtěte si hello toolearn [pochopit modely nasazení](../resource-manager-deployment-model.md) článku.</span><span class="sxs-lookup"><span data-stu-id="1952b-106">toolearn more about Azure deployment models, read hello [Understand deployment models](../resource-manager-deployment-model.md) article.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <span data-ttu-id="1952b-107"><a name = "create"></a>Vytvoření virtuálního počítače s více IP adres</span><span class="sxs-lookup"><span data-stu-id="1952b-107"><a name = "create"></a>Create a VM with multiple IP addresses</span></span>

<span data-ttu-id="1952b-108">Můžete dokončit tuto úlohu pomocí hello 2.0 rozhraní příkazového řádku Azure (v tomto článku) nebo hello [Azure CLI 1.0](virtual-network-multiple-ip-addresses-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="1952b-108">You can complete this task using hello Azure CLI 2.0 (this article) or hello [Azure CLI 1.0](virtual-network-multiple-ip-addresses-cli-nodejs.md).</span></span> <span data-ttu-id="1952b-109">Změnit hello hodnoty, jako je vhodné pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="1952b-109">Change hello values, as appropriate, for your environment.</span></span> <span data-ttu-id="1952b-110">Hello kroky, které následují vysvětlují, jak toocreate příklad virtuálního počítače s více IP adres, jak je popsáno v hello scénáři.</span><span class="sxs-lookup"><span data-stu-id="1952b-110">hello steps that follow explain how toocreate an example VM with multiple IP addresses, as described in hello scenario.</span></span> <span data-ttu-id="1952b-111">Měnit hodnoty proměnných v "" a typy IP adres podle potřeby týkající se vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="1952b-111">Change variable values in "" and IP address types as required for your implementation.</span></span> 

1. <span data-ttu-id="1952b-112">Nainstalujte hello [Azure CLI 2.0](/cli/azure/install-az-cli2) Pokud ještě nemáte nainstalováno.</span><span class="sxs-lookup"><span data-stu-id="1952b-112">Install hello [Azure CLI 2.0](/cli/azure/install-az-cli2) if you don't already have it installed.</span></span>
2. <span data-ttu-id="1952b-113">Vytvoření SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem pomocí kroků hello v hello [vytvoření SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1952b-113">Create an SSH public and private key pair for Linux VMs by completing hello steps in hello [Create an SSH public and private key pair for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
3. <span data-ttu-id="1952b-114">Z příkazového prostředí, přihlášení pomocí příkazu hello `az login` a vyberte předplatné hello používáte.</span><span class="sxs-lookup"><span data-stu-id="1952b-114">From a command shell, login with hello command `az login` and select hello subscription you're using.</span></span>
4. <span data-ttu-id="1952b-115">Vytvořte spuštěním hello skript, který odpovídá na počítači se systémem Linux nebo Mac. hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1952b-115">Create hello VM by executing hello script that follows on a Linux or Mac computer.</span></span> <span data-ttu-id="1952b-116">skript Hello vytvoří skupinu prostředků, jednu virtuální síť (VNet), jeden síťový adaptér s tři konfigurace protokolu IP a virtuální počítač s tooit hello dva síťové adaptéry připojené.</span><span class="sxs-lookup"><span data-stu-id="1952b-116">hello script creates a resource group, one virtual network (VNet), one NIC with three IP configurations, and a VM with hello two NICs attached tooit.</span></span> <span data-ttu-id="1952b-117">Hello síťový adaptér, veřejnou IP adresu, virtuální sítě a prostředky virtuálních počítačů musí existovat v hello stejné umístění a předplatného.</span><span class="sxs-lookup"><span data-stu-id="1952b-117">hello NIC, public IP address, virtual network, and VM resources must all exist in hello same location and subscription.</span></span> <span data-ttu-id="1952b-118">I když hello prostředky všechny nemají tooexist v hello stejnou skupinu prostředků, v hello následující skript dělají.</span><span class="sxs-lookup"><span data-stu-id="1952b-118">Though hello resources don't all have tooexist in hello same resource group, in hello following script they do.</span></span>

```bash
    
#!/bin/sh
    
RgName="myResourceGroup"
Location="westcentralus"
az group create --name $RgName --location $Location
    
# Create a public IP address resource with a static IP address using hello `--allocation-method Static` option. If you
# do not specify this option, hello address is allocated dynamically. hello address is assigned toohello resource from a pool
# of IP adresses unique tooeach Azure region. Download and view hello file from
# https://www.microsoft.com/en-us/download/details.aspx?id=41653 that lists hello ranges for each region.

PipName="myPublicIP"

# This name must be unique within an Azure location.
DnsName="myDNSName"

az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--dns-name $DnsName\
--allocation-method Static

# Create a virtual network with one subnet

VnetName="myVnet"
VnetPrefix="10.0.0.0/16"
VnetSubnetName="mySubnet"
VnetSubnetPrefix="10.0.0.0/24"

az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $VnetSubnetName \
--subnet-prefix $VnetSubnetPrefix

# Create a network interface connected toohello subnet and associate hello public IP address tooit. Azure will create the
# first IP configuration with a static private IP address and will associate hello public IP address resource tooit.

NicName="MyNic1"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet1Name \
--private-ip-address 10.0.0.4
--vnet-name $VnetName \
--public-ip-address $PipName
    
# Create a second public IP address, a second IP configuration, and associate it toohello NIC. This configuration has a
# static public IP address and a static private IP address.

az network public-ip create \
--resource-group $RgName \
--location $Location \
--name myPublicIP2 \
--dns-name mypublicdns2 \
--allocation-method Static

az network nic ip-config create \
--resource-group $RgName \
--nic-name $NicName \
--name IPConfig-2 \
--private-ip-address 10.0.0.5 \
--public-ip-name myPublicIP2

# Create a third IP configuration, and associate it toohello NIC. This configuration has  static private IP address and # no public IP address.

azure network nic ip-config create \
--resource-group $RgName \
--nic-name $NicName \
--private-ip-address 10.0.0.6 \
--name IPConfig-3

# Note: Though this article assigns all IP configurations tooa single NIC, you can also assign multiple IP configurations
# tooany NIC in a VM. toolearn how toocreate a VM with multiple NICs, read hello Create a VM with multiple NICs 
# article: https://docs.microsoft.com/azure/virtual-network/virtual-network-deploy-multinic-arm-cli.

# Create a VM and attach hello NIC.

VmName="myVm"

# Replace hello value for hello following **VmSize** variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes rticle. hello script fails if hello VM size
# is not supported in hello location you select. Run hello `azure vm sizes --location estcentralus` command tooget a full list
# of VMs in US West Central, for example.

VmSize="Standard_DS1"

# Replace hello value for hello OsImage variable value with a value for *urn* from hello utput returned by entering the
# `az vm image list` command.

OsImage="credativ:Debian:8:latest"

Username="adminuser"

# Replace hello following value with hello path tooyour public key file. If you're creating a Windows VM, remove hello following
# line and you'll be prompted for hello password you want tooconfigure for hello VM.

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
```

<span data-ttu-id="1952b-119">Kromě toho toocreating virtuální počítač s síťový adaptér s 3: Konfigurace protokolu IP, hello skript vytvoří:</span><span class="sxs-lookup"><span data-stu-id="1952b-119">In addition toocreating a VM with a NIC with 3 IP configurations, hello script creates:</span></span>

- <span data-ttu-id="1952b-120">Jeden premium spravovat disku ve výchozím nastavení, ale máte další možnosti pro hello typ disku, které lze vytvořit.</span><span class="sxs-lookup"><span data-stu-id="1952b-120">A single premium managed disk by default, but you have other options for hello disk type you can create.</span></span> <span data-ttu-id="1952b-121">Čtení hello [vytvoření virtuálního počítače s Linuxem pomocí hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.</span><span class="sxs-lookup"><span data-stu-id="1952b-121">Read hello [Create a Linux VM using hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article for details.</span></span>
- <span data-ttu-id="1952b-122">Virtuální síť s jednou podsítí a dvě veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1952b-122">A virtual network with one subnet and two public IP addresses.</span></span> <span data-ttu-id="1952b-123">Alternativně můžete použít *existující* virtuální sítě, podsítě, síťový adaptér nebo adresu prostředky veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1952b-123">Alternatively, you can use *existing* virtual network, subnet, NIC, or public IP address resources.</span></span> <span data-ttu-id="1952b-124">toolearn jak toouse existující prostředky síťových místo vytvoření další zdroje informací, zadejte `az vm create -h`.</span><span class="sxs-lookup"><span data-stu-id="1952b-124">toolearn how toouse existing network resources rather than creating additional resources, enter `az vm create -h`.</span></span>

<span data-ttu-id="1952b-125">Veřejné IP adresy mají nominální poplatek.</span><span class="sxs-lookup"><span data-stu-id="1952b-125">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="1952b-126">více informací o IP adresy, ceny, toolearn číst hello [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses) stránky.</span><span class="sxs-lookup"><span data-stu-id="1952b-126">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="1952b-127">Existuje limit toohello, počet veřejné IP adresy, které lze použít v předplatném.</span><span class="sxs-lookup"><span data-stu-id="1952b-127">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="1952b-128">Další informace o omezení hello, přečtěte si hello toolearn [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.</span><span class="sxs-lookup"><span data-stu-id="1952b-128">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

<span data-ttu-id="1952b-129">Po vytvoření hello virtuálního počítače zadejte hello `az network nic show --name MyNic1 --resource-group myResourceGroup` konfigurace příkaz tooview hello síťových karet.</span><span class="sxs-lookup"><span data-stu-id="1952b-129">After hello VM is created, enter hello `az network nic show --name MyNic1 --resource-group myResourceGroup` command tooview hello NIC configuration.</span></span> <span data-ttu-id="1952b-130">Zadejte hello `az network nic ip-config list --nic-name MyNic1 --resource-group myResourceGroup --output table` tooview seznam konfigurace protokolu IP hello přidružené toohello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="1952b-130">Enter hello `az network nic ip-config list --nic-name MyNic1 --resource-group myResourceGroup --output table` tooview a list of hello IP configurations associated toohello NIC.</span></span>

<span data-ttu-id="1952b-131">Přidat hello privátní IP adresy toohello virtuálních počítačů operačního systému pomocí kroků hello operačního systému v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="1952b-131">Add hello private IP addresses toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span>

## <span data-ttu-id="1952b-132"><a name="add"></a>Přidat tooa IP adresy virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="1952b-132"><a name="add"></a>Add IP addresses tooa VM</span></span>

<span data-ttu-id="1952b-133">Můžete přidat další privátní a veřejné IP adresy tooan stávající síťové karty pomocí hello kroků, které následují.</span><span class="sxs-lookup"><span data-stu-id="1952b-133">You can add additional private and public IP addresses tooan existing NIC by completing hello steps that follow.</span></span> <span data-ttu-id="1952b-134">Příklady Hello stavějí hello [scénář](#Scenario) popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="1952b-134">hello examples build upon hello [scenario](#Scenario) described in this article.</span></span>

1. <span data-ttu-id="1952b-135">Otevřete příkazové okno a dokončení hello zbývající kroky v této části v rámci jedné relace.</span><span class="sxs-lookup"><span data-stu-id="1952b-135">Open a command shell and complete hello remaining steps in this section within a single session.</span></span> <span data-ttu-id="1952b-136">Pokud ještě nemáte rozhraní příkazového řádku Azure, instalaci a konfiguraci, dokončení hello kroky v hello [instalace Azure CLI 2.0](/cli/azure/install-az-cli2?toc=%2fazure%2fvirtual-network%2ftoc.json) tooyour článek a přihlašovací účet Azure s hello `az-login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="1952b-136">If you don't already have Azure CLI installed and configured, complete hello steps in hello [Azure CLI 2.0 installation](/cli/azure/install-az-cli2?toc=%2fazure%2fvirtual-network%2ftoc.json) article and login tooyour Azure account with hello `az-login` command.</span></span>

2. <span data-ttu-id="1952b-137">Proveďte kroky hello v jednom z hello následující části, podle požadavků:</span><span class="sxs-lookup"><span data-stu-id="1952b-137">Complete hello steps in one of hello following sections, based on your requirements:</span></span>

    <span data-ttu-id="1952b-138">**Přidejte privátní IP adresy**</span><span class="sxs-lookup"><span data-stu-id="1952b-138">**Add a private IP address**</span></span>
    
    <span data-ttu-id="1952b-139">tooadd tooa privátní adresy IP síťový adaptér, je nutné vytvořit konfiguraci IP adres pomocí hello příkazu, který následuje dále.</span><span class="sxs-lookup"><span data-stu-id="1952b-139">tooadd a private IP address tooa NIC, you must create an IP configuration using hello command that follows.</span></span> <span data-ttu-id="1952b-140">Hello statická IP adresa musí být nepoužívané adresa podsítě hello.</span><span class="sxs-lookup"><span data-stu-id="1952b-140">hello static IP address must be an unused address for hello subnet.</span></span>

    ```bash
    az network nic ip-config create \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --private-ip-address 10.0.0.7 \
    --name IPConfig-4
    ```
    
    <span data-ttu-id="1952b-141">Vytvořte tolik konfigurace, jak požadujete, pomocí konfigurace jedinečné názvy a privátní IP adresy (pro konfigurace s statické IP adresy).</span><span class="sxs-lookup"><span data-stu-id="1952b-141">Create as many configurations as you require, using unique configuration names and private IP addresses (for configurations with static IP addresses).</span></span>

    <span data-ttu-id="1952b-142">**Přidejte veřejnou IP adresu**</span><span class="sxs-lookup"><span data-stu-id="1952b-142">**Add a public IP address**</span></span>
    
    <span data-ttu-id="1952b-143">Veřejná IP adresa se přidá přidružením tooeither na novou konfiguraci protokolu IP nebo existující konfiguraci IP adres.</span><span class="sxs-lookup"><span data-stu-id="1952b-143">A public IP address is added by associating it tooeither a new IP configuration or an existing IP configuration.</span></span> <span data-ttu-id="1952b-144">Kroky hello v jedné z hello částí, které následují, potřebujete.</span><span class="sxs-lookup"><span data-stu-id="1952b-144">Complete hello steps in one of hello sections that follow, as you require.</span></span>

    <span data-ttu-id="1952b-145">Veřejné IP adresy mají nominální poplatek.</span><span class="sxs-lookup"><span data-stu-id="1952b-145">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="1952b-146">více informací o IP adresy, ceny, toolearn číst hello [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses) stránky.</span><span class="sxs-lookup"><span data-stu-id="1952b-146">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="1952b-147">Existuje limit toohello, počet veřejné IP adresy, které lze použít v předplatném.</span><span class="sxs-lookup"><span data-stu-id="1952b-147">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="1952b-148">Další informace o omezení hello, přečtěte si hello toolearn [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.</span><span class="sxs-lookup"><span data-stu-id="1952b-148">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

    - <span data-ttu-id="1952b-149">**Přidružení hello prostředků tooa nová konfigurace IP**</span><span class="sxs-lookup"><span data-stu-id="1952b-149">**Associate hello resource tooa new IP configuration**</span></span>
    
        <span data-ttu-id="1952b-150">Vždy, když přidáte veřejnou IP adresu v novou konfiguraci protokolu IP, musíte taky přidat privátní IP adresy, protože všechny konfigurace protokolu IP, musí mít privátní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1952b-150">Whenever you add a public IP address in a new IP configuration, you must also add a private IP address, because all IP configurations must have a private IP address.</span></span> <span data-ttu-id="1952b-151">Můžete přidat existující prostředek veřejné IP adresy, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="1952b-151">You can either add an existing public IP address resource, or create a new one.</span></span> <span data-ttu-id="1952b-152">toocreate novou, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="1952b-152">toocreate a new one, enter hello following command:</span></span>
    
        ```bash
        az network public-ip create \
        --resource-group myResourceGroup \
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3
        ```

        <span data-ttu-id="1952b-153">toocreate na novou konfiguraci protokolu IP se statickou privátní IP adresou a hello přidružené *myPublicIP3* veřejných IP adres prostředků, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="1952b-153">toocreate a new IP configuration with a static private IP address and hello associated *myPublicIP3* public IP address resource, enter hello following command:</span></span>

        ```bash
        az network nic ip-config create \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-5 \
        --private-ip-address 10.0.0.8
        --public-ip-address myPublicIP3
        ```

    - <span data-ttu-id="1952b-154">**Přidružení hello tooan stávající IP konfigurace prostředků** prostředek veřejné IP adresy lze pouze přidružené tooan konfigurace protokolu IP, který ho přidružené neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="1952b-154">**Associate hello resource tooan existing IP configuration** A public IP address resource can only be associated tooan IP configuration that doesn't already have one associated.</span></span> <span data-ttu-id="1952b-155">Můžete určit, zda konfiguraci IP adres má přidružené veřejnou IP adresu zadáním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1952b-155">You can determine whether an IP configuration has an associated public IP address by entering hello following command:</span></span>

        ```bash
        az network nic ip-config list \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --query "[?provisioningState=='Succeeded'].{ Name: name, PublicIpAddressId: publicIpAddress.id }" --output table
        ```

        <span data-ttu-id="1952b-156">Vrátí výstup:</span><span class="sxs-lookup"><span data-stu-id="1952b-156">Returned output:</span></span>
    
            Name        PublicIpAddressId
            
            ipconfig1   /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
            IPConfig-2  /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
            IPConfig-3

        <span data-ttu-id="1952b-157">Od hello **PublicIpAddressId** sloupec pro *IpConfig 3* je prázdná v hello výstupu žádný prostředek veřejné IP adresy je aktuálně přidružené tooit.</span><span class="sxs-lookup"><span data-stu-id="1952b-157">Since hello **PublicIpAddressId** column for *IpConfig-3* is blank in hello output, no public IP address resource is currently associated tooit.</span></span> <span data-ttu-id="1952b-158">Můžete přidat existující veřejnou IP adresu prostředku tooIpConfig-3, nebo zadejte následující příkaz toocreate jeden hello:</span><span class="sxs-lookup"><span data-stu-id="1952b-158">You can add an existing public IP address resource tooIpConfig-3, or enter hello following command toocreate one:</span></span>

        ```bash
        az network public-ip create \
        --resource-group  myResourceGroup
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3 \
        --allocation-method Static
        ```
    
        <span data-ttu-id="1952b-159">Zadejte následující příkaz tooassociate hello veřejných IP adres toohello stávající IP konfigurace prostředků s názvem hello *IPConfig 3*:</span><span class="sxs-lookup"><span data-stu-id="1952b-159">Enter hello following command tooassociate hello public IP address resource toohello existing IP configuration named *IPConfig-3*:</span></span>
    
        ```bash
        az network nic ip-config update \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-3 \
        --public-ip myPublicIP3
        ```

3. <span data-ttu-id="1952b-160">Zobrazení hello privátních IP adres a hello veřejných IP adres ID prostředků přiřazen toohello hello síťový adaptér tak, že zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1952b-160">View hello private IP addresses and hello public IP address resource Ids assigned toohello NIC by entering hello following command:</span></span>

    ```bash
    az network nic ip-config list \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --query "[?provisioningState=='Succeeded'].{ Name: name, PrivateIpAddress: privateIpAddress, PrivateIpAllocationMethod: privateIpAllocationMethod, PublicIpAddressId: publicIpAddress.id }" --output table
    ```

    <span data-ttu-id="1952b-161">Vrátí výstup:</span><span class="sxs-lookup"><span data-stu-id="1952b-161">Returned output:</span></span> <br>
    
        Name        PrivateIpAddress    PrivateIpAllocationMethod   PublicIpAddressId
        
        ipconfig1   10.0.0.4            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
        IPConfig-2  10.0.0.5            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
        IPConfig-3  10.0.0.6            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP3
    

4. <span data-ttu-id="1952b-162">Přidat hello privátních IP adres jste přidali toohello seskupování toohello virtuálních počítačů operačního systému podle následujících pokynů hello v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="1952b-162">Add hello private IP addresses you added toohello NIC toohello VM operating system by following hello instructions in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="1952b-163">Nepřidávejte hello veřejné IP adresy toohello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="1952b-163">Do not add hello public IP addresses toohello operating system.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
