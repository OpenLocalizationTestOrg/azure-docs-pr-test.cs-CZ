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
# <a name="create-a-vm-with-multiple-nics-using-hello-azure-cli-20"></a>Vytvoření virtuálního počítače s více síťovými kartami pomocí hello 2.0 rozhraní příkazového řádku Azure

[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).  Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](virtual-network-deploy-multinic-classic-cli.md).
>

## <a name="create"></a>Vytvoření hello virtuálních počítačů

Můžete dokončit tuto úlohu pomocí hello 2.0 rozhraní příkazového řádku Azure (v tomto článku) nebo hello [Azure CLI 1.0](virtual-network-deploy-multinic-cli-nodejs.md). Hello hodnoty v "" pro proměnné hello hello kroky, které následují vytvořit prostředky s nastavením hello scénáře. Změnit hello hodnoty, jako je vhodné pro vaše prostředí.

1. Nainstalujte hello [Azure CLI 2.0](/cli/azure/install-az-cli2) Pokud ještě nemáte nainstalováno.
2. Vytvoření SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem pomocí kroků hello v hello [vytvoření SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. Z příkazového prostředí, přihlášení pomocí příkazu hello `az login`.
4. Vytvořte spuštěním hello skript, který odpovídá na počítači se systémem Linux nebo Mac. hello virtuálních počítačů. skript Hello vytvoří skupinu prostředků jednu virtuální síť (VNet) se dvěma podsítěmi, dva síťové adaptéry a virtuální počítač s hello dva síťové adaptéry připojené tooit. Jeden z hello síťových adaptérů je připojených tooone podsíť a je přiřazena statická IP adresa veřejné a privátní. Hello dalších síťových Adaptérů je připojených toohello jiné podsítě a je přiřazen statickou privátní IP adresou a žádné veřejnou IP adresu. Hello síťový adaptér, veřejnou IP adresu, virtuální sítě a prostředky virtuálních počítačů musí existovat v hello stejné umístění a předplatného. I když hello prostředky všechny nemají tooexist v hello stejnou skupinu prostředků, v hello následující skript dělají.

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

Kromě toho toocreating virtuálních počítačů se dvěma síťovými adaptéry, hello skript vytvoří:
- Jeden premium spravovat disku ve výchozím nastavení, ale máte další možnosti pro hello typ disku, které lze vytvořit. Čtení hello [vytvoření virtuálního počítače s Linuxem pomocí hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.
- Virtuální síť se dvěma podsítěmi a jednu veřejnou IP adresu. Alternativně můžete použít *existující* virtuální sítě, podsítě, síťový adaptér nebo adresu prostředky veřejné IP adresy. toolearn jak toouse existující prostředky síťových místo vytvoření další zdroje informací, zadejte `az vm create -h`.

## <a name = "validate"></a>Ověření vytvoření virtuálních počítačů a síťových karet

1. Zadejte příkaz hello `az resource list --resouce-group Multi-NIC-VM --output table` toosee seznamu prostředků hello vytvořen skriptem hello. Musí být šesti prostředky vrátil výstup hello: dva síťové adaptéry, jeden disk, jednu veřejnou IP adresu, jednu virtuální síť a virtuální počítač.
2. Zadejte příkaz hello `az network public-ip show --name PIP-WEB --resource-group Multi-NIC-VM --output table`. Poznamenejte si hodnotu hello v hello vrátil výstup, **IpAddress** a že hodnota hello **PublicIpAllocationMethod** je *statické*.
3. Než spustíte následující příkaz hello, odeberte hello <>, nahraďte *uživatelské jméno* s názvem hello jste použili pro hello **uživatelské jméno** proměnné ve skriptu hello a nahradit *ipAddress* s hello **ipAddress** hello v předchozím kroku. Hello spusťte následující příkaz tooconnect toohello virtuálních počítačů: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`. 
4. Po připojení toohello virtuálního počítače, spusťte hello `sudo ifconfig` příkaz toosee *eth0* a *eth1* rozhraní. Každý síťový adaptér byl přiřazen hello statickou privátní IP adresy zadané ve skriptu hello servery Azure DHCP hello. Hello IP a MAC adres, které jsou přiřazené síťové adaptéry toohello neměňte dokud hello odstranění virtuálního počítače. Doporučujeme vám, že měnit IP adresy v rámci operačního systému, jak ji můžete vypnout počítač toohello připojení. Veřejné IP adresy se nezobrazí v rámci hello operačního systému, jako jsou síťové adresy přeložit tooand z hello privátní IP adresy pomocí hello infrastruktury Azure.

## <a name= "clean-up"></a>Odebrat hello virtuálních počítačů a přidružených zdrojů.

Doporučujeme vám odstranit hello prostředky vytvořené v tomto cvičení, pokud nebude je používat v produkčním prostředí. Virtuální počítač, veřejnou IP adresu a diskové prostředky platit poplatky, tak dlouho, dokud se jejich zřízení. prostředky hello tooremove vytvořené během tohoto cvičení dokončení hello následující kroky:
1. tooview hello prostředky ve skupině prostředků hello, spusťte hello `az resource list --resource-group Multi-NIC-VM` příkaz.
2. Potvrďte, že neexistují žádné prostředky ve skupině prostředků hello, než hello prostředky vytvořen skriptem hello v tomto článku. 
3. všechny prostředky, které vytvoří v tomto cvičení spustit hello toodelete `az group delete --name Multi-NIC-VM` příkaz. příkaz Hello odstraní skupinu prostředků hello a všechny hello prostředky, které obsahuje.

## <a name="next-steps"></a>Další kroky

Síťové přenosy můžete procházet tooand z hello virtuálních počítačů vytvořena v tomto článku. Můžete definovat příchozí a odchozí pravidla v rámci skupiny NSG, které omezí hello provoz, který může obtékat tooand z každé síťové rozhraní, každou podsíť nebo obojí. Další informace o skupin Nsg, přečtěte si hello toolearn [NSG přehled](virtual-networks-nsg.md) článku.
