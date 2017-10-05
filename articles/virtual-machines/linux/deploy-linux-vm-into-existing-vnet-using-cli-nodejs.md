---
title: "Nasadit virtuální počítače s Linuxem do existující síť s Azure CLI 1.0 | Microsoft Docs"
description: "Postup nasazení virtuálního počítače s Linuxem do existující virtuální síť pomocí Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 767a3f7cadba6b1e71e5a8f5995a9db090e419dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-the-azure-cli-10"></a>Jak nasadit virtuální počítač s Linuxem do existující virtuální síť Azure s Azure CLI 1.0

Tento článek ukazuje, jak pomocí Azure CLI 1.0 nasazení virtuálního počítače (VM) do existující virtuální síť (VNet). Požadavky:

- [Účet Azure](https://azure.microsoft.com/pricing/free-trial/)
- [Soubory veřejného a privátního klíče SSH](mac-create-ssh-keys.md)


## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy
K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

- [Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)
- [Azure CLI 2.0](deploy-linux-vm-into-existing-vnet-using-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků


## <a name="quick-commands"></a>Rychlé příkazy

Pokud budete potřebovat rychle dosáhnout, v následující části jsou příkazy potřebné. Podrobnější informace a kontext pro každý krok naleznete zbývající části dokumentu, [od zde](deploy-linux-vm-into-existing-vnet-using-cli.md#detailed-walkthrough).

Předběžných požadavků: NSG skupinu prostředků, virtuální síť, pomocí protokolu SSH příchozí, podsíť. Nahradí všechny příklady s vlastním nastavením.

### <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a>Virtuální počítač nasaďte do infrastruktury virtuální sítě

```azurecli
azure vm create myVM \
    -g myResourceGroup \
    -l eastus \
    -y linux \
    -Q Debian \
    -o mystorageaccount \
    -u myAdminUser \
    -M ~/.ssh/id_rsa.pub \
    -n myVM \
    -F myVNet \
    -j mySubnet \
    -N myVNic
```

## <a name="detailed-walkthrough"></a>Podrobný postup

Prostředky Azure, například virtuální sítě a skupiny zabezpečení sítě by měl být statické a dlouhodobě prostředky, které jsou nasazeny zřídka. Po nasazený virtuální síť, můžete použít znovu nová nasazení bez žádné negativní ovlivňuje infrastruktury. Vezměte v úvahu, že je tradiční hardwaru síťový přepínač virtuální sítě. Nebude potřeba ke konfiguraci nové hardwarové přepínače s každého nasazení. Pomocí správně nakonfigurovaných virtuální síť můžete nasadit nové servery do ní opakovaně s několika, pokud existuje, změny požadované za dobu životnosti virtuální sítě.

## <a name="create-the-resource-group"></a>Vytvoření skupiny prostředků

Nejprve vytvořte skupinu prostředků pro všechny objekty, které vytvoříte v tomto návodu uspořádání. Další informace o skupinách prostředků najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md)

```azurecli
azure group create myResourceGroup --location eastus
```

## <a name="create-the-vnet"></a>Vytvoření sítě VNet

Prvním krokem je vytvoření virtuální sítě ke spuštění virtuálních počítačů do. Virtuální sítě obsahuje jednu podsíť pro účely tohoto postupu. Další informace o virtuálních sítí Azure najdete v tématu [vytvoření virtuální sítě pomocí rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)

```azurecli
azure network vnet create myVNet \
    --resource-group myResourceGroup \
    --address-prefixes 10.10.0.0/24 \
    --location eastus
```

## <a name="create-the-network-security-group"></a>Vytvořit skupinu zabezpečení sítě

Podsíť je vytvořené za existující síti skupinu zabezpečení, vytvořit skupinu zabezpečení sítě před podsíť. Skupiny zabezpečení Azure sítě jsou ekvivalentní brány firewall síťové vrstvy. Další informace o skupinách zabezpečení sítě Azure, najdete v části [postup vytvoření skupiny zabezpečení sítě v rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)

```azurecli
azure network nsg create myNetworkSecurityGroup \
    --resource-group myResourceGroup \
    --location eastus
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Přidat pravidlo povolit příchozí SSH

Virtuální počítač potřebuje přístup z Internetu, aby bylo pravidlo povolující příchozí port 22 provoz předávané port 22 přes síť, ve virtuálním počítači.

```azurecli
azure network nsg rule create inboundSSH \
    --resource-group myResourceGroup \
    --nsg-name myNSG \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix Internet \
    --source-port-range 22 \
    --destination-address-prefix 10.10.0.0/24 \
    --destination-port-range 22
```

## <a name="add-a-subnet-to-the-vnet"></a>Přidat podsíť k virtuální síti

Virtuální počítače v rámci virtuální sítě musí být umístěny v podsíti. Každý virtuální sítě může mít několik podsítí. Vytvořte podsíť a přidružte skupinu zabezpečení sítě.

```azurecli
azure network vnet subnet create mySubNet \
    --resource-group myResourceGroup \
    --vnet-name myVNet \
    --address-prefix 10.10.0.0/26 \
    --network-security-group-name myNetworkSecurityGroup
```

Podsíť je nyní přidána uvnitř virtuální sítě a přidružené skupiny zabezpečení sítě a pravidla.


## <a name="add-a-vnic-to-the-subnet"></a>Přidání adaptéru VNic k podsíti

Virtuální síťové karty (VNics) jsou důležité, protože můžete znovu použít, je jejich připojením do různých virtuálních počítačů. Tento přístup zajišťuje VNic jako statické prostředek při virtuálních počítačů může být dočasné. Vytvoření adaptéru VNic a přidružte ji k podsíť vytvořená v předchozím kroku.

```azurecli
azure network nic create myVNic \
    --resource-group myResourceGroup \
    --location eastus \
    ---subnet-vnet-name myVNet \
    --subnet-name mySubNet
```

## <a name="deploy-the-vm-into-the-vnet-and-nsg"></a>Virtuální počítač nasaďte do virtuální sítě a NSG

Nyní máte virtuální síť a podsíť uvnitř této virtuální sítě a skupinu zabezpečení sítě funguje chránit podsíť, blokovat veškerý příchozí provoz, s výjimkou port 22 pro SSH. Virtuální počítač teď můžou být nasazené uvnitř této stávající síťové infrastruktuře.

Použití Azure CLI a `azure vm create` příkaz virtuálního počítače s Linuxem se nasadí do existující skupiny prostředků Azure, virtuální síť, podsíť a VNic. Další informace o nasazení dokončení virtuálního počítače pomocí rozhraní příkazového řádku najdete v tématu [vytvořit úplný prostředí Linux pomocí rozhraní příkazového řádku Azure](create-cli-complete.md)

```azurecli
azure vm create myVM \
    --resource-group myResourceGroup \
    --location eastus \
    --os-type linux \
    --image-urn Debian \
    --storage-account-name mystorageaccount \
    --admin-username myAdminUser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --vnet-name myVNet \
    --vnet-subnet-name mySubnet \
    --nic-name myVNic
```

Pomocí rozhraní příkazového řádku příznaky vyvolávající existujících prostředků dáte pokyn Azure k nasazení virtuálních počítačů uvnitř existující síť. Jakmile nasazených virtuálních sítí a podsítí, může být ponecháno jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.  

## <a name="next-steps"></a>Další kroky

* [Vytvoření konkrétního nasazení pomocí šablony Azure Resource Manageru](../windows/cli-deploy-templates.md)
* [Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md).
* [Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony](create-ssh-secured-vm-from-template.md)
