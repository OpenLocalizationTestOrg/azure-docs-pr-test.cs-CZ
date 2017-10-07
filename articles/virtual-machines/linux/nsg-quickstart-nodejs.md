---
title: "aaaOpen porty tooa virtuálního počítače s Linuxem pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Zjistěte, jak tooopen port / create tooyour koncový bod virtuálního počítače s Linuxem pomocí hello Azure resource manager nasazení modelu a hello Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 337c37d151f527b43d4852291159b2f70a00accc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="opening-ports-and-endpoints-tooa-linux-vm-in-azure-using-hello-azure-cli-10"></a>Hello otevírání portů a koncové body tooa virtuálního počítače s Linuxem v Azure pomocí Azure CLI 1.0
Otevřete port, nebo vytvořit koncový bod, tooa virtuální počítač (VM) v Azure vytvořením filtr sítě na podsíť nebo rozhraní sítě virtuálních počítačů. Tyto filtry, které řídí příchozí a odchozí přenosy, můžete umístit na skupinu zabezpečení sítě připojené toohello prostředku, který přijímá provoz hello. Použijeme Běžným příkladem webové přenosy na portu 80. Tento článek ukazuje, jak hello tooopen port tooa virtuální počítač pomocí Azure CLI 1.0.


## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](nsg-quickstart.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello


## <a name="quick-commands"></a>Rychlé příkazy
toocreate skupinu zabezpečení sítě a pravidel, je nutné [hello Azure CLI 1.0](../../cli-install-nodejs.md) nainstalovaná a pomocí režimu Resource Manager:

```azurecli
azure config mode arm
```

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Názvy parametrů příklad zahrnuté *myResourceGroup*, *myNetworkSecurityGroup*, a *myVnet*.

Vytvořte vaši skupinu zabezpečení sítě, zadáte vlastní názvy a umístění správně. Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup* v hello *eastus* umístění:

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Přidat pravidlo tooallow HTTP provoz tooyour webový server (nebo upravit vlastní scénáře, jako je připojení SSH přístupu nebo databáze). Hello následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRule* tooallow TCP přenosy na portu 80:

```azurecli
azure network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --destination-port-range 80 \
    --access allow
```

Hello skupinu zabezpečení sítě přidružení síťového rozhraní Virtuálního počítače (NIC). Hello následující příklad přiřadí stávající síťové karty s názvem *myNic* s hello skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --name myNic
```

Alternativně můžete přidružit vaše skupina zabezpečení sítě podsíť virtuální sítě spíše než jenom toohello síťové rozhraní na jeden virtuální počítač. Hello následující příklad přiřadí existující podsíť s názvem *mySubnet* v hello *myVnet* virtuální síť s hello skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```azurecli
azure network vnet subnet set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Další informace o skupinách zabezpečení sítě
Hello zde rychlé příkazy umožní tooget nahoru a spuštěna s průchodu tooyour přenosy virtuálních počítačů. Skupiny zabezpečení sítě zadejte mnoho funkcí a členitosti pro řízení přístupu tooyour prostředky. Další informace o [vytvoření skupiny zabezpečení sítě a seznamu ACL pravidla zde](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).

Skupiny zabezpečení sítě a pravidla seznamu ACL můžete definovat v rámci šablon Azure Resource Manager. Další informace o [vytvoření skupin zabezpečení sítě pomocí šablony](../../virtual-network/virtual-networks-create-nsg-arm-template.md).

Pokud potřebujete toouse předávání portů toomap interní port tooan jedinečný externí port na vašem virtuálním počítači, použijte nástroj pro vyrovnávání zatížení a pravidla překladu adres (NAT). Můžete například chcete tooexpose port TCP 8080 externě a mít přenášená tooTCP port 80 na virtuálním počítači. Dozvíte se o [zařízení na Vyrovnávání zatížení internetového](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Další kroky
V tomto příkladu jste vytvořili přenosem tooallow HTTP jednoduché pravidlo. Můžete najít informace o vytváření podrobnější prostředí v hello následující články:

* [Přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md)
* [Co je skupina zabezpečení sítě (NSG)?](../../virtual-network/virtual-networks-nsg.md)
* [Přehled Azure Resource Manageru pro vyrovnávání zatížení](../../load-balancer/load-balancer-arm.md)

