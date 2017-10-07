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
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-hello-azure-cli-20"></a>Přiřadit více IP adres toovirtual počítačů pomocí hello 2.0 rozhraní příkazového řádku Azure

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Tento článek vysvětluje, jak toocreate virtuální počítač (VM) prostřednictvím pomocí modelu nasazení Azure Resource Manager hello hello 2.0 rozhraní příkazového řádku Azure. Tooresources vytvořené pomocí modelu nasazení classic hello nelze přiřadit více IP adres. Další informace o modelech nasazení Azure, přečtěte si hello toolearn [pochopit modely nasazení](../resource-manager-deployment-model.md) článku.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Vytvoření virtuálního počítače s více IP adres

Můžete dokončit tuto úlohu pomocí hello 2.0 rozhraní příkazového řádku Azure (v tomto článku) nebo hello [Azure CLI 1.0](virtual-network-multiple-ip-addresses-cli-nodejs.md). Změnit hello hodnoty, jako je vhodné pro vaše prostředí. Hello kroky, které následují vysvětlují, jak toocreate příklad virtuálního počítače s více IP adres, jak je popsáno v hello scénáři. Měnit hodnoty proměnných v "" a typy IP adres podle potřeby týkající se vaší implementace. 

1. Nainstalujte hello [Azure CLI 2.0](/cli/azure/install-az-cli2) Pokud ještě nemáte nainstalováno.
2. Vytvoření SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem pomocí kroků hello v hello [vytvoření SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. Z příkazového prostředí, přihlášení pomocí příkazu hello `az login` a vyberte předplatné hello používáte.
4. Vytvořte spuštěním hello skript, který odpovídá na počítači se systémem Linux nebo Mac. hello virtuálních počítačů. skript Hello vytvoří skupinu prostředků, jednu virtuální síť (VNet), jeden síťový adaptér s tři konfigurace protokolu IP a virtuální počítač s tooit hello dva síťové adaptéry připojené. Hello síťový adaptér, veřejnou IP adresu, virtuální sítě a prostředky virtuálních počítačů musí existovat v hello stejné umístění a předplatného. I když hello prostředky všechny nemají tooexist v hello stejnou skupinu prostředků, v hello následující skript dělají.

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

Kromě toho toocreating virtuální počítač s síťový adaptér s 3: Konfigurace protokolu IP, hello skript vytvoří:

- Jeden premium spravovat disku ve výchozím nastavení, ale máte další možnosti pro hello typ disku, které lze vytvořit. Čtení hello [vytvoření virtuálního počítače s Linuxem pomocí hello Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.
- Virtuální síť s jednou podsítí a dvě veřejné IP adresy. Alternativně můžete použít *existující* virtuální sítě, podsítě, síťový adaptér nebo adresu prostředky veřejné IP adresy. toolearn jak toouse existující prostředky síťových místo vytvoření další zdroje informací, zadejte `az vm create -h`.

Veřejné IP adresy mají nominální poplatek. více informací o IP adresy, ceny, toolearn číst hello [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses) stránky. Existuje limit toohello, počet veřejné IP adresy, které lze použít v předplatném. Další informace o omezení hello, přečtěte si hello toolearn [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.

Po vytvoření hello virtuálního počítače zadejte hello `az network nic show --name MyNic1 --resource-group myResourceGroup` konfigurace příkaz tooview hello síťových karet. Zadejte hello `az network nic ip-config list --nic-name MyNic1 --resource-group myResourceGroup --output table` tooview seznam konfigurace protokolu IP hello přidružené toohello síťový adaptér.

Přidat hello privátní IP adresy toohello virtuálních počítačů operačního systému pomocí kroků hello operačního systému v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku.

## <a name="add"></a>Přidat tooa IP adresy virtuálních počítačů

Můžete přidat další privátní a veřejné IP adresy tooan stávající síťové karty pomocí hello kroků, které následují. Příklady Hello stavějí hello [scénář](#Scenario) popsané v tomto článku.

1. Otevřete příkazové okno a dokončení hello zbývající kroky v této části v rámci jedné relace. Pokud ještě nemáte rozhraní příkazového řádku Azure, instalaci a konfiguraci, dokončení hello kroky v hello [instalace Azure CLI 2.0](/cli/azure/install-az-cli2?toc=%2fazure%2fvirtual-network%2ftoc.json) tooyour článek a přihlašovací účet Azure s hello `az-login` příkaz.

2. Proveďte kroky hello v jednom z hello následující části, podle požadavků:

    **Přidejte privátní IP adresy**
    
    tooadd tooa privátní adresy IP síťový adaptér, je nutné vytvořit konfiguraci IP adres pomocí hello příkazu, který následuje dále. Hello statická IP adresa musí být nepoužívané adresa podsítě hello.

    ```bash
    az network nic ip-config create \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --private-ip-address 10.0.0.7 \
    --name IPConfig-4
    ```
    
    Vytvořte tolik konfigurace, jak požadujete, pomocí konfigurace jedinečné názvy a privátní IP adresy (pro konfigurace s statické IP adresy).

    **Přidejte veřejnou IP adresu**
    
    Veřejná IP adresa se přidá přidružením tooeither na novou konfiguraci protokolu IP nebo existující konfiguraci IP adres. Kroky hello v jedné z hello částí, které následují, potřebujete.

    Veřejné IP adresy mají nominální poplatek. více informací o IP adresy, ceny, toolearn číst hello [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses) stránky. Existuje limit toohello, počet veřejné IP adresy, které lze použít v předplatném. Další informace o omezení hello, přečtěte si hello toolearn [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.

    - **Přidružení hello prostředků tooa nová konfigurace IP**
    
        Vždy, když přidáte veřejnou IP adresu v novou konfiguraci protokolu IP, musíte taky přidat privátní IP adresy, protože všechny konfigurace protokolu IP, musí mít privátní IP adresy. Můžete přidat existující prostředek veřejné IP adresy, nebo vytvořte novou. toocreate novou, zadejte následující příkaz hello:
    
        ```bash
        az network public-ip create \
        --resource-group myResourceGroup \
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3
        ```

        toocreate na novou konfiguraci protokolu IP se statickou privátní IP adresou a hello přidružené *myPublicIP3* veřejných IP adres prostředků, zadejte následující příkaz hello:

        ```bash
        az network nic ip-config create \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-5 \
        --private-ip-address 10.0.0.8
        --public-ip-address myPublicIP3
        ```

    - **Přidružení hello tooan stávající IP konfigurace prostředků** prostředek veřejné IP adresy lze pouze přidružené tooan konfigurace protokolu IP, který ho přidružené neobsahuje. Můžete určit, zda konfiguraci IP adres má přidružené veřejnou IP adresu zadáním hello následující příkaz:

        ```bash
        az network nic ip-config list \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --query "[?provisioningState=='Succeeded'].{ Name: name, PublicIpAddressId: publicIpAddress.id }" --output table
        ```

        Vrátí výstup:
    
            Name        PublicIpAddressId
            
            ipconfig1   /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
            IPConfig-2  /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
            IPConfig-3

        Od hello **PublicIpAddressId** sloupec pro *IpConfig 3* je prázdná v hello výstupu žádný prostředek veřejné IP adresy je aktuálně přidružené tooit. Můžete přidat existující veřejnou IP adresu prostředku tooIpConfig-3, nebo zadejte následující příkaz toocreate jeden hello:

        ```bash
        az network public-ip create \
        --resource-group  myResourceGroup
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3 \
        --allocation-method Static
        ```
    
        Zadejte následující příkaz tooassociate hello veřejných IP adres toohello stávající IP konfigurace prostředků s názvem hello *IPConfig 3*:
    
        ```bash
        az network nic ip-config update \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-3 \
        --public-ip myPublicIP3
        ```

3. Zobrazení hello privátních IP adres a hello veřejných IP adres ID prostředků přiřazen toohello hello síťový adaptér tak, že zadáte následující příkaz:

    ```bash
    az network nic ip-config list \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --query "[?provisioningState=='Succeeded'].{ Name: name, PrivateIpAddress: privateIpAddress, PrivateIpAllocationMethod: privateIpAllocationMethod, PublicIpAddressId: publicIpAddress.id }" --output table
    ```

    Vrátí výstup: <br>
    
        Name        PrivateIpAddress    PrivateIpAllocationMethod   PublicIpAddressId
        
        ipconfig1   10.0.0.4            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
        IPConfig-2  10.0.0.5            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
        IPConfig-3  10.0.0.6            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP3
    

4. Přidat hello privátních IP adres jste přidali toohello seskupování toohello virtuálních počítačů operačního systému podle následujících pokynů hello v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku. Nepřidávejte hello veřejné IP adresy toohello operačního systému.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
