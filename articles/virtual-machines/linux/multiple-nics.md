---
title: "virtuální počítač s Linuxem v Azure s více síťovými kartami aaaCreate | Microsoft Docs"
description: "Zjistěte, jak připojit toocreate virtuálního počítače s Linuxem s více síťovými kartami tooit pomocí šablony Azure CLI 2.0 nebo správce prostředků hello."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 2723405914777a5dce4354d4f5d8413e357f58e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>Jak toocreate virtuální počítač s Linuxem v Azure s více síťových rozhraní karty
Virtuální počítač (VM) můžete vytvořit v Azure, který má více tooit rozhraním (síťovým kartám) připojené virtuální sítě. Obvyklým scénářem je toohave různé podsítě pro připojení front-end a back-end nebo síť vyhrazený tooa monitorování nebo řešení zálohování. Tento článek popisuje, jak připojit tooit toocreate virtuálního počítače s více síťovými kartami a jak tooadd nebo odeberte síťové adaptéry ze stávajícího virtuálního počítače. Podrobné informace, včetně jak toocreate několik síťových adaptérů v rámci vlastní Bash skripty, další informace o [nasazení virtuálních počítačů více síťovými Kartami](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Různé [velikosti virtuálních počítačů](sizes.md) podporu různých počet síťových adaptérů, takže odpovídajícím způsobem upravit velikost virtuálního počítače.

Tento článek podrobně popisuje, jak toocreate a virtuálních počítačů s více síťovými kartami s hello 2.0 rozhraní příkazového řádku Azure. Můžete také provést tyto kroky hello [Azure CLI 1.0](multiple-nics-nodejs.md).


## <a name="create-supporting-resources"></a>Vytvoření doprovodné materiály
Instalace hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Názvy parametrů příklad zahrnuté *myResourceGroup*, *můj_účet_úložiště*, a *Můjvp*.

Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vytvoření virtuální sítě hello s [vytvoření sítě vnet az](/cli/azure/network/vnet#create). Hello následující příklad vytvoří virtuální síť s názvem *myVnet* a podsíť s názvem *mySubnetFrontEnd*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

Vytvořte podsíť pro provoz back-end hello s [az sítě vnet podsíť vytváření](/cli/azure/network/vnet/subnet#create). Hello následující příklad vytvoří podsíť s názvem *mySubnetBackEnd*:

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

Vytvořit skupinu zabezpečení sítě s [vytvořit az sítě nsg](/cli/azure/network/nsg#create). Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a>Vytvořit a nakonfigurovat několik síťových adaptérů
Vytvořte dva síťové adaptéry se [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create). Hello následující příklad vytvoří dva síťové adaptéry s názvem *myNic1* a *myNic2*, propojených hello skupinu zabezpečení sítě, s jednou síťovou KARTOU připojení tooeach podsítě:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic1 \
    --vnet-name myVnet \
    --subnet mySubnetFrontEnd \
    --network-security-group myNetworkSecurityGroup
az network nic create \
    --resource-group myResourceGroup \
    --name myNic2 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-hello-nics"></a>Vytvoření virtuálního počítače a připojte síťové adaptéry hello
Při vytváření hello virtuální počítač, zadejte síťové adaptéry hello jste vytvořili pomocí `--nics`. Musíte taky tootake pozor, když vyberete hello velikost virtuálního počítače. Existují omezení hello celkový počet síťových adaptérů, můžete přidat tooa virtuálních počítačů. Další informace o [velikosti virtuálního počítače s Linuxem](sizes.md). 

Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create). Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp*:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS3_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic1 myNic2
```

## <a name="add-a-nic-tooa-vm"></a>Přidat tooa síťový adaptér virtuálního počítače
předchozí kroky Hello vytvořili virtuální počítač s více síťovými kartami. Můžete také přidat existující virtuální počítač s hello Azure CLI 2.0 tooan síťových karet. 

Vytvořte další síťová karta s [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create). Hello následující příklad vytvoří síťový adaptér s názvem *myNic3* připojené podsítě toohello back-end a skupinu zabezpečení sítě vytvořené v předchozích krocích hello:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

tooadd tooan seskupování existující virtuální počítač, nejprve zrušit přidělení hello virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate). Hello následující příklad zruší přidělení hello virtuálního počítače s názvem *Můjvp*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Přidat hello síťový adaptér s [přidat síťový adaptér virtuálního počítače az](/cli/azure/vm/nic#add). Hello následující příklad přidá *myNic3* příliš*Můjvp*:

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

Spustit virtuální počítač s hello [az spuštění virtuálního počítače](/cli/azure/vm#start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

## <a name="remove-a-nic-from-a-vm"></a>Odeberte síťový adaptér z virtuálního počítače
tooremove síťový adaptér ze stávajícího virtuálního počítače, nejprve navrácení hello virtuálních počítačů s [az OM deallocate](/cli/azure/vm#deallocate). Hello následující příklad zruší přidělení hello virtuálního počítače s názvem *Můjvp*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Odebrat hello síťový adaptér s [odebrat seskupování virtuálních počítačů az](/cli/azure/vm/nic#remove). Hello následující příklad odebere *myNic3* z *Můjvp*:

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM 
    --nics myNic3
```

Spustit virtuální počítač s hello [az spuštění virtuálního počítače](/cli/azure/vm#start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a>Vytvořit několik síťových adaptérů pomocí šablony Resource Manageru
Šablony Azure Resource Manageru pomocí deklarativní toodefine soubory JSON prostředí. Můžete si přečíst [přehled nástroje Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Správce prostředků šablony poskytují způsob, jak toocreate více instancí prostředku během nasazení, jako je například vytváření několik síťových adaptérů. Používáte *kopie* toospecify hello počet instancí toocreate:

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Další informace o [vytváření více instancí pomocí *kopie*](../../resource-group-create-multiple.md). 

Můžete použít také `copyIndex()` toothen připojit číslo název prostředku tooa, což vám umožní toocreate `myNic1`, `myNic2`, atd. hello následující ukazuje příklad připojování hodnotu indexu hello:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Kompletní příklad, jak si můžete přečíst [vytváření několik síťových adaptérů pomocí šablony Resource Manageru](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Další kroky
Zkontrolujte [velikosti virtuálního počítače s Linuxem](sizes.md) při pokusu o toocreating virtuálního počítače s více síťovými kartami. Věnujte pozornost toohello maximální počet síťových adaptérů podporuje každý velikost virtuálního počítače. 
