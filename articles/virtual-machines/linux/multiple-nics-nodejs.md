---
title: "virtuální počítač s Linuxem v Azure s více síťovými kartami aaaCreate | Microsoft Docs"
description: "Zjistěte, jak připojit toocreate virtuálního počítače s Linuxem s více síťovými kartami tooit pomocí rozhraní příkazového řádku Azure nebo správce prostředků šablony hello."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 457dab734ceeeefd35cddaf1ebb9ea0a82f4e207
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-multiple-nics-using-hello-azure-cli-10"></a>Vytvořit virtuální počítač s Linuxem s více síťovými kartami pomocí hello Azure CLI 1.0
Virtuální počítač (VM) můžete vytvořit v Azure, který má více tooit rozhraním (síťovým kartám) připojené virtuální sítě. Obvyklým scénářem je toohave různé podsítě pro připojení front-end a back-end nebo síť vyhrazený tooa monitorování nebo řešení zálohování. Tento článek obsahuje rychlý příkazy toocreate virtuálního počítače s více tooit síťové adaptéry připojené. Podrobné informace, včetně jak toocreate několik síťových adaptérů v rámci vlastní Bash skripty, další informace o [nasazení virtuálních počítačů více síťovými Kartami](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Různé [velikosti virtuálních počítačů](sizes.md) podporu různých počet síťových adaptérů, takže odpovídajícím způsobem upravit velikost virtuálního počítače.

> [!WARNING]
> Při vytvoření virtuálního počítače – nelze přidat stávající virtuální počítač s hello Azure CLI 1.0 tooan síťové adaptéry, je nutné připojit víc síťových karet. Můžete [přidat síťové adaptéry tooan existující virtuální počítač s hello Azure CLI 2.0](multiple-nics.md). Můžete také [vytvoření virtuálního počítače založené na původní virtuální disky hello](copy-vm.md) a vytvořte několik síťových adaptérů, jak nasadit hello virtuálních počítačů.


## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#create-supporting-resources) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](multiple-nics.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello


## <a name="create-supporting-resources"></a>Vytvoření doprovodné materiály
Ujistěte se, že máte hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) přihlášení a použití režimu Resource Manager:

```azurecli
azure config mode arm
```

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Názvy parametrů příklad zahrnuté *myResourceGroup*, *můj_účet_úložiště*, a *Můjvp*.

Nejprve vytvořte skupinu prostředků. Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:

```azurecli
azure group create myResourceGroup --location eastus
```

Vytvořte virtuální počítače toohold účet úložiště. Hello následující příklad vytvoří účet úložiště s názvem *můj_účet_úložiště*:

```azurecli
azure storage account create mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --kind Storage \
    --sku-name PLRS
```

Vytvořte virtuální počítače na tooconnect virtuální sítě. Hello následující příklad vytvoří virtuální síť s názvem *myVnet* s předponu adresy z *192.168.0.0/16*:

```azurecli
azure network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefixes 192.168.0.0/16
```

Vytvořte dvě virtuální sítě podsítě – jednu pro provoz front-endu a jednu pro provoz back-end. Hello následující příklad vytvoří dvě podsítě, s názvem *mySubnetFrontEnd* a *mySubnetBackEnd*:

```azurecli
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetFrontEnd \
    --address-prefix 192.168.1.0/24
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

## <a name="create-and-configure-multiple-nics"></a>Vytvořit a nakonfigurovat několik síťových adaptérů
Si můžete přečíst další informace o [nasazení pomocí hello rozhraní příkazového řádku Azure několik síťových adaptérů](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md), včetně skriptování hello proces ve smyčce přes toocreate všechny síťové adaptéry hello.

Hello následující příklad vytvoří dva síťové adaptéry s názvem *myNic1* a *myNic2*, s jednou síťovou KARTOU připojení tooeach podsítě:

```azurecli
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic1 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetFrontEnd
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic2 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetBackEnd
```

Obvykle můžete také vytvořit [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) nebo [nástroj pro vyrovnávání zatížení](../../load-balancer/load-balancer-overview.md) toohelp spravovat a distribuovat provoz do virtuálních počítačů. Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Vytvořit vazbu vaše síťové adaptéry toohello skupinu zabezpečení sítě pomocí `azure network nic set`. Hello následující příklad vytvoří vazbu *myNic1* a *myNic2* s *myNetworkSecurityGroup*:

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-hello-nics"></a>Vytvoření virtuálního počítače a připojte síťové adaptéry hello
Při vytváření hello virtuálních počítačů, je nyní zadat několik síťových adaptérů. Místo použití `--nic-name` tooprovide jednu síťovou kartu, místo toho použít `--nic-names` a zadejte čárkami oddělený seznam síťových adaptérů. Musíte taky tootake pozor, když vyberete hello velikost virtuálního počítače. Existují omezení hello celkový počet síťových adaptérů, můžete přidat tooa virtuálních počítačů. Další informace o [velikosti virtuálního počítače s Linuxem](sizes.md). Hello následující příklad ukazuje, jak toospecify několik síťových adaptérů a potom virtuální počítač velikost této podporuje použití více síťových adaptérů (*Standard_DS2_v2*):

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username azureuser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
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
Ujistěte se, že tooreview [velikosti virtuálního počítače s Linuxem](sizes.md) při pokusu o toocreating virtuálního počítače s více síťovými kartami. Věnujte pozornost toohello maximální počet síťových adaptérů podporuje každý velikost virtuálního počítače. 

Mějte na paměti, že nemůžete přidat další síťové adaptéry tooan existující virtuální počítač, je nutné vytvořit všechny síťové adaptéry hello při nasazování hello virtuálních počítačů. Vezměte v potaz při plánování vašeho nasazení toomake, že máte všechny potřebné hello síťové připojení z hello outset.

