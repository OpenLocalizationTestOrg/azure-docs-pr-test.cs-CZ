---
title: "aaaOpen porty tooa virtuálního počítače s Linuxem pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Zjistěte, jak tooopen port / create tooyour koncový bod virtuálního počítače s Linuxem pomocí hello Azure resource manager nasazení modelu a hello 2.0 rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: c79b31206e97558171609cf033bb3cb3370777c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="open-ports-and-endpoints-tooa-linux-vm-with-hello-azure-cli"></a>Otevřete porty a koncové body tooa virtuálního počítače s Linuxem se hello rozhraní příkazového řádku Azure
Otevřete port, nebo vytvořit koncový bod, tooa virtuální počítač (VM) v Azure vytvořením filtr sítě na podsíť nebo rozhraní sítě virtuálních počítačů. Tyto filtry, které řídí příchozí a odchozí přenosy, můžete umístit na skupinu zabezpečení sítě připojené toohello prostředku, který přijímá provoz hello. Použijeme Běžným příkladem webové přenosy na portu 80. Tento článek ukazuje, jak tooopen port tooa virtuálních počítačů s hello 2.0 rozhraní příkazového řádku Azure. Můžete také provést tyto kroky hello [Azure CLI 1.0](nsg-quickstart-nodejs.md).


## <a name="quick-commands"></a>Rychlé příkazy
Skupina zabezpečení sítě toocreate a pravidel, je nutné hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Zahrnout názvy parametrů příklad *myResourceGroup*, *myNetworkSecurityGroup*, a *myVnet*.

Vytvořit skupinu zabezpečení sítě hello s [vytvořit az sítě nsg](/cli/azure/network/nsg#create). Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup* v hello *eastus* umístění:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Přidat pravidlo s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) tooallow HTTP provozu tooyour webový server (nebo upravit vlastní scénáře, jako je připojení SSH přístupu nebo databáze). Hello následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRule* tooallow TCP přenosy na portu 80:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```

Přidružení hello skupinu zabezpečení sítě Virtuálního počítače síťovým rozhraním (NIC) [aktualizace seskupování sítě az](/cli/azure/network/nic#update). Hello následující příklad přiřadí stávající síťové karty s názvem *myNic* s hello skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

Alternativně můžete přiřadit skupině zabezpečení sítě podsíť virtuální sítě s [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update) místo právě toohello síťové rozhraní na jeden virtuální počítač. Hello následující příklad přiřadí existující podsíť s názvem *mySubnet* v hello *myVnet* virtuální síť s hello skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a>Další informace o skupinách zabezpečení sítě
Hello zde rychlé příkazy umožní tooget nahoru a spuštěna s průchodu tooyour přenosy virtuálních počítačů. Skupiny zabezpečení sítě zadejte mnoho funkcí a členitosti pro řízení přístupu tooyour prostředky. Další informace o [vytvoření skupiny zabezpečení sítě a seznamu ACL pravidla zde](tutorial-virtual-network.md#secure-network-traffic).

Pro vysokou dostupnost webové aplikace měli byste umístit virtuální počítače za pro vyrovnávání zatížení Azure. Nástroj pro vyrovnávání zatížení Hello distribuuje provoz tooVMs ke skupině zabezpečení sítě, která poskytuje filtrování provozu. Další informace najdete v tématu [jak tooload vyrovnávání Linux virtuálního počítače v Azure toocreate vysoce dostupné aplikace](tutorial-load-balancer.md).

## <a name="next-steps"></a>Další kroky
V tomto příkladu jste vytvořili přenosem tooallow HTTP jednoduché pravidlo. Můžete najít informace o vytváření podrobnější prostředí v hello následující články:

* [Přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md)
* [Co je skupina zabezpečení sítě (NSG)?](../../virtual-network/virtual-networks-nsg.md)
