---
title: "aaaVM s více IP adres pomocí hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak tooassign více IP adres tooa virtuálního počítače pomocí hello 1.0 rozhraní příkazového řádku Azure | Správce prostředků."
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
ms.openlocfilehash: 83ad48e67309fb21d5aca967d4f2c01afdc0b5cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-azure-cli-10"></a>Přiřadit více IP adres toovirtual počítačů pomocí Azure CLI 1.0

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Tento článek vysvětluje, jak toocreate virtuální počítač (VM) prostřednictvím pomocí modelu nasazení Azure Resource Manager hello hello Azure CLI 1.0. Tooresources vytvořené pomocí modelu nasazení classic hello nelze přiřadit více IP adres. Další informace o modelech nasazení Azure, přečtěte si hello toolearn [pochopit modely nasazení](../resource-manager-deployment-model.md) článku.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Vytvoření virtuálního počítače s více IP adres

Můžete dokončit tuto úlohu pomocí hello 1.0 rozhraní příkazového řádku Azure (v tomto článku) nebo hello [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md). Hello kroky, které následují vysvětlují, jak toocreate příklad virtuálního počítače s více IP adres, jak je popsáno v hello scénáři. Změnit názvy proměnných a typy IP adres podle potřeby týkající se vaší implementace.

1. Instalace a konfigurace hello Azure CLI 1.0 podle následující hello kroky v hello [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článek a přihlaste se k účtu Azure s hello `azure-login` příkaz.

2. Vytvořte skupinu prostředků:
    
    ```azurecli
    RgName=myResourceGroup
    Location=westcentralus
    azure group create --name $RgName --location $Location
    ```
3. Vytvořte virtuální síť:

    ```azurecli
    azure network vnet create --resource-group $RgName --location $Location --name myVNet \
    --address-prefixes 10.0.0.0/16
    ```
4. Vytvořte podsíť ve virtuální síti hello:

    ```azurecli
    azure network vnet subnet create --name mySubnet --resource-group $RgName --vnet-name myVNet \
    --address-prefix 10.0.0.0/24
    ```
    
5. Vytvořte účet úložiště pro hello virtuálních počítačů. Před spuštěním hello následující příkaz, nahraďte *můj_účet_úložiště* s jedinečným názvem. Název Hello musí být jedinečný v rámci Azure:

    ```azurecli
    az storage account create --resource-group $RgName --location $Location --name mystorageaccount \
    --kind Storage --sku Standard_LRS
    ```

6. Vytvořte hello konfigurace protokolu IP, síťový adaptér a přiřaďte hello IP konfigurace toohello síťový adaptér. Můžete přidat, odebrat ani změnit hello konfigurace podle potřeby. Hello následující konfigurace jsou popsané v hello scénáři:

    **IPConfig-1**

    Zadejte hello příkazy, které následují toocreate:

    - Prostředek veřejné IP adresy s statickou veřejnou IP adresu
    - Síťový adaptér, přiřazování hello veřejnou IP adresu a statické tooit privátní adresy IP.
    
    Nahraďte *mypublicdns* s názvem, který je v rámci hello umístění Azure jedinečný.

      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP1 \
      --domain-name-label mypublicdns --allocation-method Static
        
      azure network nic create --resource-group $RgName --location $Location --name myNic1 \
      --private-ip-address 10.0.0.4 --subnet-name mySubnet --subnet-vnet-name myVNet \
      --subnet-name mySubnet --public-ip-name myPublicIP1
      ```

      > [!NOTE]
      > Veřejné IP adresy mají nominální poplatek. více informací o IP adresy, ceny, toolearn číst hello [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses) stránky. Existuje limit toohello, počet veřejné IP adresy, které lze použít v předplatném. Další informace o omezení hello, přečtěte si hello toolearn [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.

    **IPConfig-2**

     Zadejte následující příkazy toocreate hello nový prostředek veřejné IP adresy a nové konfigurace IP adresy se statickou veřejnou IP adresu a statickou privátní IP adresou:
    
      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP2
      --domain-name-label mypublicdns2 --allocation-method Static

      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --name IPConfig-2
      --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
      ```

    **IPConfig – 3**

    Zadejte následující příkazy toocreate konfiguraci IP adres se statickou privátní IP adresou a žádné veřejnou IP adresu hello:

      ```azurecli
      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --private-ip-address 10.0.0.6 \
      --name IPConfig-3
      ```

    >[!NOTE] 
    >Když v tomto článku přiřadí všechny tooa konfigurace IP jednu síťovou kartu, je také možné přiřadit více IP konfigurace tooany síťový adaptér ve virtuálním počítači. toolearn jak toocreate virtuálního počítače s více síťovými kartami, přečtěte si text hello vytvořit virtuální počítač s více síťových adaptérů článku.

7. Vytvoření virtuálního počítače s Linuxem 

    ```azurecli
    az vm create --resource-group $RgName --name myVM1 --location $Location --nics myNic1 \
    --image UbuntuLTS --ssh-key-value ~/.ssh/id_rsa.pub --admin-username azureuser
    ```

    například toochange hello v2 tooStandard DS2 velikost virtuálního počítače, jednoduše přidejte následující vlastnost hello `--vm-size Standard_DS3_v2` toohello `azure vm create` příkazu v kroku 6.

8. Zadejte následující příkaz tooview hello seskupování hello a hello související konfigurace protokolu IP:

    ```azurecli
    azure network nic show --resource-group $RgName --name myNic1
    ```
9. Přidat hello privátní IP adresy toohello virtuálních počítačů operačního systému pomocí kroků hello operačního systému v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku.

## <a name="add"></a>Přidat tooa IP adresy virtuálních počítačů

Můžete přidat další privátní a veřejné IP adresy tooan stávající síťové karty pomocí hello kroků, které následují. Příklady Hello stavějí hello [scénář](#Scenario) popsané v tomto článku.

1. Otevřete rozhraní příkazového řádku Azure a dokončení hello zbývající kroky v této části v rámci jedné relace rozhraní příkazového řádku. Pokud ještě nemáte rozhraní příkazového řádku Azure, instalaci a konfiguraci, dokončení hello kroky v hello [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článek a přihlaste se k účtu Azure.

2. Proveďte kroky hello v jednom z hello následující části, podle požadavků:

    - **Přidejte privátní IP adresy**
    
        tooadd tooa privátní adresy IP síťový adaptér, je nutné vytvořit konfiguraci IP adres pomocí níže uvedeného příkazu hello. Hello statickou adresu musí obsahovat adresu nepoužívané hello podsítě.

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 \
        --private-ip-address 10.0.0.7 --name IPConfig-4
        ```
        Vytvořte tolik konfigurace, jak požadujete, pomocí konfigurace jedinečné názvy a privátní IP adresy (pro konfigurace s statické IP adresy).

    - **Přidejte veřejnou IP adresu**
    
        Veřejná IP adresa se přidá přidružením tooeither na novou konfiguraci protokolu IP nebo existující konfiguraci IP adres. Kroky hello v jedné z hello částí, které následují, potřebujete.

        > [!NOTE]
        > Veřejné IP adresy mají nominální poplatek. více informací o IP adresy, ceny, toolearn číst hello [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses) stránky. Existuje limit toohello, počet veřejné IP adresy, které lze použít v předplatném. Další informace o omezení hello, přečtěte si hello toolearn [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.
        >

        **Přidružení hello prostředků tooa nová konfigurace IP**
    
        Vždy, když přidáte veřejnou IP adresu v novou konfiguraci protokolu IP, musíte taky přidat privátní IP adresy, protože všechny konfigurace protokolu IP, musí mít privátní IP adresy. Můžete přidat existující prostředek veřejné IP adresy, nebo vytvořte novou. toocreate novou, zadejte následující příkaz hello:

        ```azurecli
        azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 \
        --domain-name-label mypublicdns3
        ```

        toocreate na novou konfiguraci protokolu IP se statickou privátní IP adresou a hello přidružené *myPublicIP3* veřejných IP adres prostředků, zadejte následující příkaz hello:

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 \
        --private-ip-address 10.0.0.8 --public-ip-name myPublicIP3
        ```

        **Přidružení hello prostředků tooan existující konfigurace IP**

        Prostředek veřejné IP adresy lze pouze přidružené tooan konfigurace protokolu IP, který ho přidružené neobsahuje. Můžete určit, zda konfiguraci IP adres má přidružené veřejnou IP adresu zadáním hello následující příkaz:

        ```azurecli
        azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
        ```

        Vyhledejte podobné toohello řádku, ten, který následuje pro IPConfig 3 v hello vrátil výstup:

        ```         
        Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet
        ```
          
        Od hello **veřejnou IP adresu** sloupec pro *IpConfig 3* je prázdné, žádný prostředek veřejné IP adresy je aktuálně přidružené tooit. Můžete přidat existující veřejnou IP adresu prostředku tooIpConfig-3, nebo zadejte následující příkaz toocreate jeden hello:

        ```azurecli
        azure network public-ip create --resource-group  myResourceGroup --location westcentralus \
        --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
        ```

        Zadejte následující příkaz tooassociate hello veřejných IP adres toohello stávající IP konfigurace prostředků s názvem hello *IPConfig 3*:
        ```azurecli
        azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 \
        --public-ip-name myPublicIP3
        ```

3. Zobrazení hello privátních IP adres a hello veřejnou IP adresu prostředky přiřazené toohello hello síťový adaptér tak, že zadáte následující příkaz:

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

      Výstup je podobné toohello následující vrácen Hello:
      ```
      Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        
      default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
      IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
      IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet  myPublicIP3
      ```
4. Přidat hello privátních IP adres jste přidali toohello seskupování toohello virtuálních počítačů operačního systému podle následujících pokynů hello v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku. Nepřidávejte hello veřejné IP adresy toohello operačního systému.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
