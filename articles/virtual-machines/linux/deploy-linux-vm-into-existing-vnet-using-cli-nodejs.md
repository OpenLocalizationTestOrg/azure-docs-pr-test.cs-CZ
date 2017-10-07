---
title: "aaaDeploy virtuální počítače s Linuxem do existující síť s Azure CLI 1.0 | Microsoft Docs"
description: "Jak hello toodeploy virtuálního počítače s Linuxem do existující virtuální sítě pomocí Azure CLI 1.0"
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
ms.openlocfilehash: e660f1563d386efc7788bd236f8b067145ea09bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-cli-10"></a>Jak toodeploy virtuální počítač s Linuxem do existující virtuální síť Azure s hello Azure CLI 1.0

Tento článek ukazuje, jak toouse Azure CLI 1.0 toodeploy virtuální počítač (VM) do existující virtuální síť (VNet). Hello požadavky jsou:

- [Účet Azure](https://azure.microsoft.com/pricing/free-trial/)
- [Soubory veřejného a privátního klíče SSH](mac-create-ssh-keys.md)


## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](deploy-linux-vm-into-existing-vnet-using-cli.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello


## <a name="quick-commands"></a>Rychlé příkazy

Pokud je třeba tooquickly dosáhnout hello, hello následující část podrobně hello příkazy potřebné. Podrobnější informace a kontext pro každý krok naleznete hello zbytek dokumentu hello [od zde](deploy-linux-vm-into-existing-vnet-using-cli.md#detailed-walkthrough).

Předběžných požadavků: NSG skupinu prostředků, virtuální síť, pomocí protokolu SSH příchozí, podsíť. Nahradí všechny příklady s vlastním nastavením.

### <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a>Nasazení hello virtuálních počítačů do infrastruktury virtuální sítě hello

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

Prostředky Azure, například hello virtuálních sítí a skupiny zabezpečení sítě by měl být statické a dlouhodobě prostředky, které jsou nasazeny zřídka. Po nasazený virtuální síť, můžete použít znovu nová nasazení bez jakékoli infrastruktury toohello má negativní vliv. Vezměte v úvahu, že je tradiční hardwaru síťový přepínač virtuální sítě. Nebude potřeba tooconfigure zcela nový hardware přepínač s každého nasazení. Pomocí správně nakonfigurovaných virtuální síť můžete pokračovat toodeploy nových serverů do ní opakovaně v několika, pokud existuje, změny během hello doby trvání hello virtuální sítě.

## <a name="create-hello-resource-group"></a>Vytvořte skupinu prostředků hello

Nejprve vytvořte tooorganize skupiny prostředků všechny objekty, které vytvoříte v tomto návodu. Další informace o skupinách prostředků najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md)

```azurecli
azure group create myResourceGroup --location eastus
```

## <a name="create-hello-vnet"></a>Vytvoření hello virtuální sítě

prvním krokem Hello je toobuild hello toolaunch virtuální sítě virtuálních počítačů do. Hello virtuální síť obsahuje jednu podsíť pro účely tohoto postupu. Další informace o virtuálních sítí Azure najdete v tématu [vytvoření virtuální sítě pomocí hello rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)

```azurecli
azure network vnet create myVNet \
    --resource-group myResourceGroup \
    --address-prefixes 10.10.0.0/24 \
    --location eastus
```

## <a name="create-hello-network-security-group"></a>Vytvořit skupinu zabezpečení sítě hello

podsíť Hello vychází za existující skupinu zabezpečení sítě, vytvořit skupinu zabezpečení sítě hello před hello podsítě. Skupiny zabezpečení Azure sítě jsou ekvivalentní tooa brány firewall hello síťové vrstvy. Další informace o skupinách zabezpečení sítě Azure, najdete v části [jak skupiny zabezpečení sítě toocreate ve hello rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)

```azurecli
azure network nsg create myNetworkSecurityGroup \
    --resource-group myResourceGroup \
    --location eastus
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Přidat pravidlo povolit příchozí SSH

Hello virtuálního počítače potřebuje přístup z hello je nutná internet, pravidlo povolující příchozí port 22 provoz toobe předána hello sítě tooport 22 na hello virtuálních počítačů.

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

## <a name="add-a-subnet-toohello-vnet"></a>Přidat toohello podsíť virtuální sítě

Virtuální počítače v rámci hello virtuální síť musí být umístěny v podsíti. Každý virtuální sítě může mít několik podsítí. Vytvořte podsíť hello a přidružte skupinu zabezpečení sítě hello.

```azurecli
azure network vnet subnet create mySubNet \
    --resource-group myResourceGroup \
    --vnet-name myVNet \
    --address-prefix 10.10.0.0/26 \
    --network-security-group-name myNetworkSecurityGroup
```

Hello podsíť je nyní přidána uvnitř hello virtuální sítě a přidružené skupiny zabezpečení sítě hello a pravidla.


## <a name="add-a-vnic-toohello-subnet"></a>Přidat podsíť toohello VNic

Virtuální síťové karty (VNics) jsou důležité, protože můžete znovu použít, je jejich připojením toodifferent virtuálních počítačů. Tento přístup zajišťuje hello VNic jako statické prostředek hello virtuální počítače mohou být dočasné. Vytvoření adaptéru VNic a přidružte ji k hello podsíť vytvořená v předchozím kroku hello.

```azurecli
azure network nic create myVNic \
    --resource-group myResourceGroup \
    --location eastus \
    ---subnet-vnet-name myVNet \
    --subnet-name mySubNet
```

## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a>Nasazení hello virtuálních počítačů do hello virtuální sítě a NSG

Nyní máte virtuální síť a podsíť uvnitř této virtuální sítě a skupinu zabezpečení sítě může tooprotect hello podsítě ve blokovat veškerý příchozí provoz, s výjimkou port 22 pro SSH. Teď můžou být nasazené Hello virtuálních počítačů uvnitř této stávající síťové infrastruktuře.

Pomocí rozhraní příkazového řádku Azure hello a hello `azure vm create` příkaz, je nasazené toohello existující skupiny prostředků Azure, virtuální síť, podsíť a VNic hello virtuálního počítače s Linuxem. Další informace o používání rozhraní příkazového řádku toodeploy hello dokončení virtuálních počítačů najdete v tématu [vytvořit úplný prostředí Linux pomocí hello rozhraní příkazového řádku Azure](create-cli-complete.md)

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

Rozhraní příkazového řádku příznaky pomocí hello toocall se stávajícími prostředky, dáte pokyn Azure toodeploy hello virtuálních počítačů uvnitř hello stávající sítě. Jakmile nasazených virtuálních sítí a podsítí, může být ponecháno jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.  

## <a name="next-steps"></a>Další kroky

* [Použijte toocreate šablony Azure Resource Manager konkrétní nasazení](../windows/cli-deploy-templates.md)
* [Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md).
* [Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony](create-ssh-secured-vm-from-template.md)
