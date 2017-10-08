---
title: "aaaUse interní DNS pro virtuální počítač překlad s hello 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Jak virtuální sítě toocreate kartami a používat interní DNS pro překlad názvů virtuálních počítačů v Azure s hello 2.0 rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 02/16/2017
ms.author: v-livech
ms.openlocfilehash: b3c4bfd3ab698f7b25d763ba9e60dd7984f6269d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-virtual-network-interface-cards-and-use-internal-dns-for-vm-name-resolution-on-azure"></a>Vytvořit virtuální síťové karty a používat interní DNS pro překlad názvů virtuálních počítačů v Azure
Tento článek ukazuje, jak tooset statické interní názvy DNS pro virtuální počítače s Linuxem pomocí virtuální síťové adaptéry (vNics) a názvy DNS popisek s hello 2.0 rozhraní příkazového řádku Azure. Můžete také provést tyto kroky hello [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Statické názvy DNS jsou použity pro trvalé infrastruktury služby jako server volaných sestavení, který se používá k tomuto dokumentu nebo Git serveru.

Hello požadavky jsou:

* [Účet Azure](https://azure.microsoft.com/pricing/free-trial/)
* [Soubory veřejného a privátního klíče SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Rychlé příkazy
Pokud je třeba tooquickly dosáhnout hello, hello následující část podrobně hello příkazy potřebné. Podrobnější informace a kontext pro každý krok lze nalézt v hello zbytek dokumentu hello [od zde](#detailed-walkthrough). Tyto kroky tooperform, budete potřebovat hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Předběžných požadavků: Skupinu prostředků, virtuální síť a podsíť, skupinu zabezpečení sítě pomocí protokolu SSH příchozí.

### <a name="create-a-virtual-network-interface-card-with-a-static-internal-dns-name"></a>Vytvořit virtuální síťový adaptér se statickou interní název DNS
Vytvoření hello vNic s [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create). Hello `--internal-dns-name` rozhraní příkazového řádku příznak je pro popisek DNS hello nastavení, která poskytuje hello statické název DNS pro virtuální síťová karta hello (vNic). Hello následující příklad vytvoří adaptéru vNic s názvem `myNic`, ho připojí toohello `myVnet` virtuální síti a vytvoří interní záznam název DNS názvem `jenkins`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

### <a name="deploy-a-vm-and-connect-hello-vnic"></a>Nasadit virtuální počítač a připojte hello vNic
Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create). Hello `--nics` příznak během nasazení tooAzure hello připojuje hello vNic toohello virtuálních počítačů. Hello následující příklad vytvoří virtuální počítač s názvem `myVM` s Azure spravované disky a připojí hello vNic s názvem `myNic` z hello předchozím kroku:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="detailed-walkthrough"></a>Podrobný postup

Úplné průběžnou integraci a průběžné nasazování (CiCd) vyžaduje určité toobe statickou nebo dlohotrvající servery infrastruktury v Azure. Doporučujeme, aby Azure prostředky jako hello virtuální sítě a skupiny zabezpečení sítě jsou statické a dlouhodobě prostředky, které jsou nasazeny zřídka. Po nasazený virtuální síť, můžete použít znovu nová nasazení bez jakékoli infrastruktury toohello má negativní vliv. Později můžete přidat server úložiště Git nebo automatizační server volaných přináší CiCd toothis virtuální sítě pro vaše prostředí pro vývoj nebo testování.  

Interní názvy DNS jsou pouze přeložit uvnitř virtuální sítě Azure. Protože se jedná o interní hello názvy DNS, nejsou přeložit toohello mimo Internetu, poskytuje dodatečné zabezpečení toohello infrastruktury.

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Zahrnout názvy parametrů příklad `myResourceGroup`, `myNic`, a `myVM`.

## <a name="create-hello-resource-group"></a>Vytvořte skupinu prostředků hello
Nejprve vytvořte skupinu prostředků hello s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westus` umístění:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-hello-virtual-network"></a>Vytvoření virtuální sítě hello

dalším krokem Hello je toobuild hello toolaunch virtuální sítě virtuálních počítačů do. virtuální síť Hello obsahuje jednu podsíť pro účely tohoto postupu. Další informace o virtuálních sítí Azure najdete v tématu [vytvoření virtuální sítě pomocí rozhraní příkazového řádku Azure hello](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Vytvoření virtuální sítě hello s [vytvoření sítě vnet az](/cli/azure/network/vnet#create). Hello následující příklad vytvoří virtuální síť s názvem `myVnet` a podsíť s názvem `mySubnet`:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-hello-network-security-group"></a>Vytvoření hello skupinu zabezpečení sítě
Skupiny zabezpečení Azure sítě jsou ekvivalentní tooa brány firewall hello síťové vrstvy. Další informace o skupinách zabezpečení sítě najdete v tématu [jak toocreate skupin Nsg v hello rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Vytvořit skupinu zabezpečení sítě hello s [vytvořit az sítě nsg](/cli/azure/network/nsg#create). Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-rule-tooallow-ssh"></a>Přidat příchozí pravidlo tooallow SSH
Přidat příchozí pravidlo pro skupinu zabezpečení sítě hello s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create). Hello následující příklad vytvoří pravidlo s názvem `myRuleAllowSSH`:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myRuleAllowSSH \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 22 \
    --access allow
```

## <a name="associate-hello-subnet-with-hello-network-security-group"></a>Hello podsíť přidružit hello skupinu zabezpečení sítě
tooassociate hello podsíť s hello skupinu zabezpečení sítě používat [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update). Hello následující příklad přiřadí název podsítě hello `mySubnet` s hello skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```


## <a name="create-hello-virtual-network-interface-card-and-static-dns-names"></a>Vytvořte virtuální síťová karta hello a statické názvy DNS
Azure je velmi flexibilní, ale toouse názvy DNS pro rozlišení názvu virtuálního počítače, je nutné toocreate virtuální síťové karty (vNics) zahrnující název DNS. vNics jsou důležité, protože můžete znovu použít, je jejich připojením toodifferent virtuálních počítačů přes hello infrastruktury životního cyklu. Tento přístup zajišťuje hello vNic jako statické prostředek hello virtuální počítače mohou být dočasné. Pomocí DNS označování na kartě vNic hello jsme možné tooenable jednoduchý název řešení z jiných virtuálních počítačů v hello virtuální sítě. Automatizační server hello tooaccess jiné virtuální počítače pomocí přeložit názvy umožňuje podle názvu DNS hello `Jenkins` nebo server hello Git jako `gitrepo`.  

Vytvoření hello vNic s [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create). Hello následující příklad vytvoří adaptéru vNic s názvem `myNic`, ho připojí toohello `myVnet` virtuální síť s názvem `myVnet`a vytvoří interní záznam název DNS názvem `jenkins`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

## <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a>Nasazení hello virtuálních počítačů do infrastruktury virtuální sítě hello
Máme teď virtuální síť a podsíť, funguje jako brána firewall tooprotect naše podsíť blokováním veškerý příchozí provoz, s výjimkou port 22 pro SSH a adaptéru vNic skupina zabezpečení sítě. Teď můžete nasadit virtuální počítač uvnitř této stávající síťové infrastruktuře.

Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create). Hello následující příklad vytvoří virtuální počítač s názvem `myVM` s Azure spravované disky a připojí hello vNic s názvem `myNic` z hello předchozím kroku:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Rozhraní příkazového řádku příznaky pomocí hello toocall se stávajícími prostředky, jsme pokyn Azure toodeploy hello virtuálních počítačů uvnitř hello stávající sítě. tooreiterate, jakmile nasazených virtuálních sítí a podsítí, mohou být levá jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.  

## <a name="next-steps"></a>Další kroky
* [Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
