---
title: "aaaDeploy virtuální počítače s Linuxem do stávající sítě s 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toodeploy virtuální počítač s Linuxem do existující virtuální sítě pomocí Azure CLI 2.0"
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
ms.openlocfilehash: 0df44b3437002df050db56f3b3899083fb49d803
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-cli"></a>Jak toodeploy virtuální počítač s Linuxem do existující virtuální síť Azure s hello rozhraní příkazového řádku Azure

Tento článek ukazuje, jak toouse hello Azure CLI 2.0 toodeploy virtuální počítač (VM) do existující virtuální síť. Hello požadavky jsou:

- [Účet Azure](https://azure.microsoft.com/pricing/free-trial/)
- [Soubory veřejného a privátního klíče SSH](mac-create-ssh-keys.md)

Můžete také provést tyto kroky hello [Azure CLI 1.0](deploy-linux-vm-into-existing-vnet-using-cli-nodejs.md).


## <a name="quick-commands"></a>Rychlé příkazy
Pokud je třeba tooquickly dosáhnout hello, hello následující část podrobně hello příkazy potřebné. Podrobnější informace a kontext pro každý krok naleznete hello zbytek dokumentu hello [od zde](#detailed-walkthrough).

toocreate tento vlastní prostředí budete potřebovat hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.

**Předběžných požadavků:** skupina prostředků Azure, virtuální síť a podsíť, příchozí skupinu zabezpečení sítě pomocí protokolu SSH a virtuální síťový adaptér.

### <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a>Nasazení hello virtuálních počítačů do infrastruktury virtuální sítě hello

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

Prostředky Azure jako virtuální sítě a skupiny zabezpečení sítě by měl být statické a dlouhodobě prostředky, které jsou nasazeny zřídka. Po nasazený virtuální síť, můžete použít znovu nová nasazení bez jakékoli infrastruktury toohello má negativní vliv. Vezměte v úvahu virtuální sítě, že síťový přepínač tradiční hardwaru – nebude potřeba tooconfigure zcela nový hardware přepínač s každého nasazení. Správně nakonfigurována virtuální síť, můžete pokračovat toodeploy nové virtuální počítače do této virtuální sítě s několika opakovaně, pokud existuje, nevyžadují změny během hello doby trvání hello virtuální sítě.

toocreate tento vlastní prostředí budete potřebovat hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.

## <a name="create-hello-resource-group"></a>Vytvořte skupinu prostředků hello

Nejprve vytvořte tooorganize skupiny prostředků Azure. všechny objekty, které vytvoříte v tomto návodu. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md). Vytvořte skupinu prostředků hello s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:

```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

## <a name="create-hello-virtual-network"></a>Vytvoření virtuální sítě hello

Umožňuje vytvářet virtuální počítače do toolaunch hello virtuální síti Azure. Další informace o virtuálních sítích najdete v tématu [vytvoření virtuální sítě pomocí rozhraní příkazového řádku Azure hello](../../virtual-network/virtual-networks-create-vnet-arm-cli.md). Vytvoření virtuální sítě hello s [vytvoření sítě vnet az](/cli/azure/network/vnet#create). Hello následující příklad vytvoří virtuální síť s názvem *myVnet* a podsíť s názvem *mySubnet*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefix 10.10.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 10.10.1.0/24
```

## <a name="create-hello-network-security-group"></a>Vytvořit skupinu zabezpečení sítě hello

Skupiny zabezpečení Azure sítě jsou ekvivalentní tooa brány firewall hello síťové vrstvy. Další informace o skupinách zabezpečení sítě najdete v tématu [jak skupiny zabezpečení sítě toocreate ve hello rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-nsg-arm-cli.md). Vytvořit skupinu zabezpečení sítě hello s [vytvořit az sítě nsg](/cli/azure/network/nsg#create). Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Přidat pravidlo povolit příchozí SSH

Hello virtuálního počítače potřebuje přístup z hello je nutná Internetu, aby pravidlo povolující příchozí port 22 provoz toobe předána hello sítě tooport 22 na hello virtuálních počítačů. Přidat příchozí pravidlo pro skupinu zabezpečení sítě hello s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create). Hello následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRuleSSH*:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --priority 1000 \
    --protocol tcp \
    --destination-port-range 22 \
```

## <a name="attach-hello-subnet-toohello-network-security-group"></a>Připojit skupinu zabezpečení sítě toohello podsítí hello

Hello pravidel skupiny zabezpečení sítě můžou být použité tooa podsíť nebo konkrétní virtuální síťové rozhraní. Umožňuje připojit hello sítě zabezpečení skupiny tooour podsítě. Připojit vaše podsíť toohello skupina zabezpečení sítě se [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update):

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="add-a-virtual-network-interface-card-toohello-subnet"></a>Přidat podsíť virtuální sítě rozhraní karty toohello

Virtuální síťové karty (VNics) jsou důležité, protože můžete znovu použít, je jejich připojením toodifferent virtuálních počítačů. Tato opakované použití vám umožní tookeep hello VNic jako statické prostředek hello virtuální počítače mohou být dočasné. Vytvoření adaptéru VNic a přidružte ji k podsíti hello s [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create). Hello následující příklad vytvoří adaptéru VNic s názvem *myNic*:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet
```

## <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a>Nasazení hello virtuálních počítačů do infrastruktury virtuální sítě hello

Nyní máte virtuální síť a podsíť a podsíť hello tooprotect skupiny zabezpečení sítě pomocí blokovat veškerý příchozí provoz, s výjimkou port 22 pro SSH. Teď můžou být nasazené Hello virtuálních počítačů uvnitř této stávající síťové infrastruktuře.

Vytvoření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#create). Další informace o hello flags toouse s hello Azure CLI 2.0 toodeploy dokončení virtuálních počítačů, najdete v části [vytvořit úplný prostředí Linux pomocí příkazového řádku Azure CLI hello](create-cli-complete.md).

Hello následující ukázka vytvoří virtuální počítač pomocí Azure spravované disků. Tyto disky jsou zpracovávány hello platformy Azure a nevyžadují, aby všechny přípravné nebo umístění toostore je. Další informace o spravovaných discích najdete v tématu [Přehled služby Azure Managed Disks](../../storage/storage-managed-disks-overview.md). Pokud chcete toouse nespravované disky, najdete v části hello Další poznámka níže.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

Pokud používáte spravovaných disků, tento krok přeskočte. Pokud chcete disky toouse nespravované, potřebujete následující další parametry toohello pokračováním příkaz toocreate nespravované disky v hello účet úložiště s názvem hello tooadd `mystorageaccount`: 

```azurecli
    --use-unmanaged-disk \
    --storage-account mystorageaccount
```

Rozhraní příkazového řádku příznaky pomocí hello toocall se stávajícími prostředky, dáte pokyn Azure toodeploy hello virtuálních počítačů uvnitř hello stávající sítě. Jakmile jsou nasazené virtuální síť a podsíť, může být ponecháno jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure. V tomto příkladu není vytvořte a přiřaďte toohello veřejnou adresu IP VNic, takže tento virtuální počítač není veřejně přístupné přes hello Internet. Další informace najdete v tématu [vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí rozhraní příkazového řádku Azure hello](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md).

## <a name="next-steps"></a>Další kroky
Další informace o způsobech toocreate virtuálních počítačů v Azure najdete v tématu hello následující prostředky:

* [Použijte toocreate šablony Azure Resource Manager konkrétní nasazení](../windows/cli-deploy-templates.md)
* [Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md).
* [Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony](create-ssh-secured-vm-from-template.md)
