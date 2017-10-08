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
# <a name="create-a-vm-with-a-static-public-ip-address-using-hello-azure-cli-20"></a>Vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí hello 2.0 rozhraní příkazového řádku Azure

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md)
> * [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md)
> * [Šablona](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (Classic)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Tento článek se zabývá pomocí modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello modelu nasazení classic.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name = "create"></a>Vytvoření hello virtuálních počítačů

Můžete dokončit tuto úlohu pomocí hello 2.0 rozhraní příkazového řádku Azure (v tomto článku) nebo hello [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md). Hello hodnoty v "" pro proměnné hello hello kroky, které následují vytvořit prostředky s nastavením hello scénáře. Změnit hello hodnoty, jako je vhodné pro vaše prostředí.

1. Nainstalujte hello [Azure CLI 2.0](/cli/azure/install-az-cli2) Pokud ještě nemáte nainstalováno.
2. Vytvoření SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem pomocí kroků hello v hello [vytvoření SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. Z příkazového prostředí, přihlášení pomocí příkazu hello `az login`.
4. Vytvořte spuštěním hello skript, který odpovídá na počítači se systémem Linux nebo Mac. hello virtuálních počítačů. Hello Azure veřejnou IP adresu, virtuální sítě, síťové a prostředky virtuálních počítačů musí existovat v hello stejné umístění. I když hello prostředky všechny nemají tooexist v hello stejnou skupinu prostředků, v hello následující skript dělají.

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

Kromě toho toocreating virtuální počítač, hello skript vytvoří:
- Jeden premium spravovat disku ve výchozím nastavení, ale máte další možnosti pro hello typ disku, které lze vytvořit. Čtení hello [vytvoření virtuálního počítače s Linuxem pomocí hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.
- Virtuální sítě, podsítě, seskupování a veřejnou IP adresu prostředky. Alternativně můžete použít *existující* virtuální sítě, podsítě, síťový adaptér nebo adresu prostředky veřejné IP adresy. toolearn jak toouse existující prostředky síťových místo vytvoření další zdroje informací, zadejte `az vm create -h`.

## <a name = "validate"></a>Ověření vytvoření virtuálních počítačů a veřejnou IP adresu

1. Zadejte příkaz hello `az resource list --resouce-group IaaSStory --output table` toosee seznamu prostředků hello vytvořen skriptem hello. Musí být pět prostředky vrátil výstup hello: Síťová rozhraní, disk, veřejnou IP adresu, virtuální sítě a virtuální počítač.
2. Zadejte příkaz hello `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`. Poznamenejte si hodnotu hello v hello vrátil výstup, **IpAddress** a že hodnota hello **PublicIpAllocationMethod** je *statické*.
3. Před provedením hello následující příkaz, odeberte hello <>, nahraďte *uživatelské jméno* s názvem hello jste použili pro hello **uživatelské jméno** proměnné ve skriptu hello a nahradit *ipAddress* s hello **ipAddress** hello v předchozím kroku. Hello spusťte následující příkaz tooconnect toohello virtuálních počítačů: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`. 

## <a name= "clean-up"></a>Odebrat hello virtuálních počítačů a přidružených zdrojů.

Doporučujeme vám odstranit hello prostředky vytvořené v tomto cvičení, pokud nebude je používat v produkčním prostředí. Virtuální počítač, veřejnou IP adresu a diskové prostředky platit poplatky, tak dlouho, dokud se jejich zřízení. prostředky hello tooremove vytvořené během tohoto cvičení dokončení hello následující kroky:

1. tooview hello prostředky ve skupině prostředků hello, spusťte hello `az resource list --resource-group IaaSStory` příkaz.
2. Potvrďte, že neexistují žádné prostředky ve skupině prostředků hello, než hello prostředky vytvořen skriptem hello v tomto článku. 
3. všechny prostředky, které vytvoří v tomto cvičení spustit hello toodelete `az group delete -n IaaSStory` příkaz. příkaz Hello odstraní skupinu prostředků hello a všechny hello prostředky, které obsahuje.

## <a name="next-steps"></a>Další kroky

Síťové přenosy můžete procházet tooand z hello virtuálních počítačů vytvořena v tomto článku. Můžete definovat příchozí a odchozí pravidla v rámci skupiny NSG, které omezují hello provoz, který může obtékat tooand z hello síťové rozhraní, hello podsíť nebo obojí. Další informace o skupin Nsg, přečtěte si hello toolearn [NSG přehled](virtual-networks-nsg.md) článku.
