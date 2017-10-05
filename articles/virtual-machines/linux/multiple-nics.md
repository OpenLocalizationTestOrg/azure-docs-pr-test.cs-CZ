---
title: "Vytvořte virtuální počítač s Linuxem v Azure s více síťovými kartami | Microsoft Docs"
description: "Naučte se vytvořit virtuální počítač s Linuxem s více síťovými kartami připojit se pomocí šablony Azure CLI 2.0 nebo správce prostředků."
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
ms.openlocfilehash: 8a2931e462079c101c91497d459d7d3126234244
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>Vytvoření virtuální počítač s Linuxem v Azure s více síťových karet
Virtuální počítač (VM) můžete vytvořit v Azure, který má více rozhraní virtuální sítě (NIC) je připojený. Obvyklým scénářem je mít různé podsítě pro připojení front-end a back-end nebo síť vyhrazený pro řešení monitorování nebo zálohování. Tento článek podrobně popisuje postup vytvoření virtuálního počítače s více síťovými kartami k němu připojen a postup přidání nebo odebrání síťové adaptéry ze stávajícího virtuálního počítače. Podrobné informace, včetně toho, jak vytvořit několik síťových adaptérů v rámci své vlastní skripty Bash, další informace o [nasazení virtuálních počítačů více síťovými Kartami](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Různé [velikosti virtuálních počítačů](sizes.md) podporu různých počet síťových adaptérů, takže odpovídajícím způsobem upravit velikost virtuálního počítače.

Tento článek podrobně popisuje postup vytvoření virtuálního počítače s více síťovými kartami s Azure CLI 2.0. K provedení těchto kroků můžete také využít [Azure CLI 1.0](multiple-nics-nodejs.md).


## <a name="create-supporting-resources"></a>Vytvoření doprovodné materiály
Nainstalujte si nejnovější verzi [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se k Azure účet pomocí [az přihlášení](/cli/azure/#login).

V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty. Názvy parametrů příklad zahrnuté *myResourceGroup*, *můj_účet_úložiště*, a *Můjvp*.

Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vytvoření virtuální sítě s [vytvoření sítě vnet az](/cli/azure/network/vnet#create). Následující příklad vytvoří virtuální síť s názvem *myVnet* a podsíť s názvem *mySubnetFrontEnd*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

Vytvořte podsíť pro provoz back-end s [az sítě vnet podsíť vytváření](/cli/azure/network/vnet/subnet#create). Následující příklad vytvoří podsíť s názvem *mySubnetBackEnd*:

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

Vytvořit skupinu zabezpečení sítě s [vytvořit az sítě nsg](/cli/azure/network/nsg#create). Následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a>Vytvořit a nakonfigurovat několik síťových adaptérů
Vytvořte dva síťové adaptéry se [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create). Následující příklad vytvoří dva síťové adaptéry s názvem *myNic1* a *myNic2*, připojené skupinu zabezpečení sítě s jednou síťovou KARTOU připojení ke každé podsítě:

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

## <a name="create-a-vm-and-attach-the-nics"></a>Vytvoření virtuálního počítače a připojte síťové karty
Při vytváření virtuálního počítače, zadejte tyto adaptéry jste vytvořili pomocí `--nics`. Musíte také ujistěte se, když vyberete velikost virtuálního počítače. Existují omezení pro celkový počet síťových adaptérů, které můžete přidat k virtuálnímu počítači. Další informace o [velikosti virtuálního počítače s Linuxem](sizes.md). 

Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create). Následující příklad vytvoří virtuální počítač s názvem *Můjvp*:

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

## <a name="add-a-nic-to-a-vm"></a>Přidat síťovou kartu k virtuálnímu počítači
V předchozím postupu vytvořili virtuální počítač s více síťovými kartami. Síťové adaptéry můžete také přidat na existující virtuální počítač s Azure CLI 2.0. 

Vytvořte další síťová karta s [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create). Následující příklad vytvoří síťový adaptér s názvem *myNic3* připojené k back-end podsíť a síť skupiny zabezpečení vytvořené v předchozích krocích:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

Chcete-li přidat síťový adaptér na existující virtuální počítač, nejprve zrušit přidělení virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate). Následující příklad zruší přidělení virtuálního počítače s názvem *Můjvp*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Přidat síťovou kartu s [přidat síťový adaptér virtuálního počítače az](/cli/azure/vm/nic#add). Následující příklad přidá *myNic3* k *Můjvp*:

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

Spusťte virtuální počítač s [spuštění virtuálního počítače az](/cli/azure/vm#start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

## <a name="remove-a-nic-from-a-vm"></a>Odeberte síťový adaptér z virtuálního počítače
Odebrat síťový adaptér ze stávajícího virtuálního počítače, nejprve zrušit přidělení virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate). Následující příklad zruší přidělení virtuálního počítače s názvem *Můjvp*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Odeberte na síťový adaptér s [odebrat seskupování virtuálních počítačů az](/cli/azure/vm/nic#remove). Následující příklad odebere *myNic3* z *Můjvp*:

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM 
    --nics myNic3
```

Spusťte virtuální počítač s [spuštění virtuálního počítače az](/cli/azure/vm#start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a>Vytvořit několik síťových adaptérů pomocí šablony Resource Manageru
Šablony Azure Resource Manageru použijte k definování prostředí deklarativní soubory JSON. Můžete si přečíst [přehled nástroje Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Správce prostředků šablony poskytují způsob, jak vytvořit více instancí prostředku během nasazení, jako je například vytváření několik síťových adaptérů. Používáte *kopie* k určení počtu instancí vytvořit:

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Další informace o [vytváření více instancí pomocí *kopie*](../../resource-group-create-multiple.md). 

Můžete použít také `copyIndex()` pak připojit k názvu zdroje, které vám umožní vytvořit číslo `myNic1`, `myNic2`atd. Na obrázku je příkladem připojením index hodnoty:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Kompletní příklad, jak si můžete přečíst [vytváření několik síťových adaptérů pomocí šablony Resource Manageru](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Další kroky
Zkontrolujte [velikosti virtuálního počítače s Linuxem](sizes.md) při pokusu o vytvoření virtuálního počítače s více síťovými kartami. Věnujte pozornost maximální počet síťových adaptérů podporuje každý velikost virtuálního počítače. 