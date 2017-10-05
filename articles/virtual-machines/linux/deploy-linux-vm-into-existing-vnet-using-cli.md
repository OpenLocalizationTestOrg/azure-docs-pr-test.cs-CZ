---
title: "Nasadit virtuální počítače s Linuxem do stávající sítě s 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak nasadit virtuální počítač s Linuxem do existující virtuální síť pomocí Azure CLI 2.0"
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
ms.devlang: azurecli
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 932fd74ec83f43b604382346ee2c273f5453fcd0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-the-azure-cli"></a>Jak nasadit virtuální počítač s Linuxem do existující virtuální síť Azure pomocí Azure CLI

Tento článek ukazuje, jak používat 2.0 rozhraní příkazového řádku Azure k nasazení virtuálního počítače (VM) do existující virtuální síť. Požadavky:

- [Účet Azure](https://azure.microsoft.com/pricing/free-trial/)
- [Soubory veřejného a privátního klíče SSH](mac-create-ssh-keys.md)

K provedení těchto kroků můžete také využít [Azure CLI 1.0](deploy-linux-vm-into-existing-vnet-using-cli-nodejs.md).


## <a name="quick-commands"></a>Rychlé příkazy
Pokud budete potřebovat rychle dosáhnout, v následující části jsou příkazy potřebné. Podrobnější informace a kontext pro každý krok naleznete zbývající části dokumentu, [od zde](#detailed-walkthrough).

K vytvoření tohoto vlastního prostředí, je třeba nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).

V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty. Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.

**Předběžných požadavků:** skupina prostředků Azure, virtuální síť a podsíť, příchozí skupinu zabezpečení sítě pomocí protokolu SSH a virtuální síťový adaptér.

### <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a>Virtuální počítač nasaďte do infrastruktury virtuální sítě

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

## <a name="detailed-walkthrough"></a>Podrobný postup

Prostředky Azure jako virtuální sítě a skupiny zabezpečení sítě by měl být statické a dlouhodobě prostředky, které jsou nasazeny zřídka. Po nasazený virtuální síť, můžete použít znovu nová nasazení bez žádné negativní ovlivňuje infrastruktury. Vezměte v úvahu virtuální sítě, že síťový přepínač tradiční hardwaru – nebude potřeba ke konfiguraci nové hardwarové přepínače s každého nasazení. Pomocí správně nakonfigurovaných virtuální síť můžete nasadit nové virtuální počítače do této virtuální sítě dokola s několika, pokud existuje, změny požadované za dobu životnosti virtuální sítě.

K vytvoření tohoto vlastního prostředí, je třeba nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).

V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty. Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.

## <a name="create-the-resource-group"></a>Vytvoření skupiny prostředků

Nejprve vytvořte skupinu prostředků Azure pro všechny objekty, které vytvoříte v tomto návodu uspořádání. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md). Vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění:

```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

## <a name="create-the-virtual-network"></a>Vytvoření virtuální sítě

Umožňuje vytvářet virtuální sítě Azure ke spuštění virtuálních počítačů do. Další informace o virtuálních sítích najdete v tématu [vytvoření virtuální sítě pomocí rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-vnet-arm-cli.md). Vytvoření virtuální sítě s [vytvoření sítě vnet az](/cli/azure/network/vnet#create). Následující příklad vytvoří virtuální síť s názvem *myVnet* a podsíť s názvem *mySubnet*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefix 10.10.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 10.10.1.0/24
```

## <a name="create-the-network-security-group"></a>Vytvořit skupinu zabezpečení sítě

Skupiny zabezpečení Azure sítě jsou ekvivalentní brány firewall síťové vrstvy. Další informace o skupinách zabezpečení sítě najdete v tématu [postup vytvoření skupiny zabezpečení sítě v Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md). Vytvořit skupinu zabezpečení sítě s [vytvořit az sítě nsg](/cli/azure/network/nsg#create). Následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Přidat pravidlo povolit příchozí SSH

Virtuální počítač potřebuje přístup z Internetu, aby bylo pravidlo povolující příchozí port 22 provoz předávané port 22 přes síť, ve virtuálním počítači. Přidat příchozí pravidlo pro skupinu zabezpečení sítě s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create). Následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRuleSSH*:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --priority 1000 \
    --protocol tcp \
    --destination-port-range 22 \
```

## <a name="attach-the-subnet-to-the-network-security-group"></a>Podsíť přiřadit skupinu zabezpečení sítě

Pravidla skupiny zabezpečení sítě je použít pro podsíť nebo konkrétní virtuální síťové rozhraní. Umožňuje připojit skupinu zabezpečení sítě k naší podsítě. Podsíť přiřadit skupinu zabezpečení sítě s [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update):

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="add-a-virtual-network-interface-card-to-the-subnet"></a>Přidat virtuální síťový adaptér k podsíti

Virtuální síťové karty (VNics) jsou důležité, protože můžete znovu použít, je jejich připojením do různých virtuálních počítačů. Tato opakované použití umožňuje zachovat adaptéru VNic jako statické prostředků virtuálních počítačů mohou být dočasné. Vytvoření adaptéru VNic a přidružte ji k podsíti s [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create). Následující příklad vytvoří adaptéru VNic s názvem *myNic*:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet
```

## <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a>Virtuální počítač nasaďte do infrastruktury virtuální sítě

Nyní máte virtuální síť a podsíť a skupinu zabezpečení sítě chránit podsíť, blokovat veškerý příchozí provoz, s výjimkou port 22 pro SSH. Virtuální počítač teď můžou být nasazené uvnitř této stávající síťové infrastruktuře.

Vytvoření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#create). Další informace o příznaky pomocí Azure CLI 2.0 používat a kompletní virtuální počítač nasadit, naleznete v části [vytvořit dokončení prostředí Linux pomocí příkazového řádku Azure CLI](create-cli-complete.md).

Následující příklad vytvoří virtuální počítač pomocí Azure spravované disků. Tyto disky jsou zpracovávány platformy Azure a nevyžadují, aby všechny přípravné nebo umístění pro uložení. Další informace o spravovaných discích najdete v tématu [Přehled služby Azure Managed Disks](../../storage/storage-managed-disks-overview.md). Pokud chcete používat nespravované disky, najdete v části Další poznámku níže.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

Pokud používáte spravovaných disků, tento krok přeskočte. Pokud chcete používat nespravované disky, budete muset přidat další parametry do příkazu budete pokračovat, vytvořit nespravované disky v účtu úložiště s názvem `mystorageaccount`: 

```azurecli
    --use-unmanaged-disk \
    --storage-account mystorageaccount
```

Pomocí rozhraní příkazového řádku příznaky vyvolávající existujících prostředků dáte pokyn Azure k nasazení virtuálních počítačů uvnitř existující síť. Jakmile jsou nasazené virtuální síť a podsíť, může být ponecháno jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure. V tomto příkladu není vytvořte a přiřaďte veřejnou IP adresu vnic, takže tento virtuální počítač není veřejně přístupné přes Internet. Další informace najdete v tématu [vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí rozhraní příkazového řádku Azure](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md).

## <a name="next-steps"></a>Další kroky
Další informace o způsoby, jak vytvořit virtuální počítače v Azure najdete v následujících zdrojích informací:

* [Vytvoření konkrétního nasazení pomocí šablony Azure Resource Manageru](../windows/cli-deploy-templates.md)
* [Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md).
* [Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony](create-ssh-secured-vm-from-template.md)
