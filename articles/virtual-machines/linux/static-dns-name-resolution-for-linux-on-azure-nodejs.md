---
title: "aaaUsing interní DNS pro virtuální počítač překlad v Azure | Microsoft Docs"
description: "Pomocí interní DNS pro překlad názvů virtuálních počítačů v Azure."
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
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: v-livech
ms.openlocfilehash: 94fd6577aa51ce5db4dc26649b415ddeeb410eb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-internal-dns-for-vm-name-resolution-on-azure"></a>Pomocí interní DNS pro překlad názvů virtuálních počítačů v Azure

Tento článek ukazuje, jak tooset statické interní DNS názvů pro virtuální počítače s Linuxem pomocí karty virtuální síťovou kartu (VNic) a štítek názvy DNS. Statické názvy DNS jsou použity pro trvalé infrastruktury služby jako server volaných sestavení, který se používá k tomuto dokumentu nebo Git serveru.

Hello požadavky jsou:

* [Účet Azure](https://azure.microsoft.com/pricing/free-trial/)
* [Soubory veřejného a privátního klíče SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](static-dns-name-resolution-for-linux-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello


## <a name="quick-commands"></a>Rychlé příkazy

Pokud je třeba tooquickly dosáhnout hello, hello následující část podrobně hello příkazy potřebné. Podrobnější informace a kontext pro každý krok naleznete hello zbytek dokumentu hello [od zde](#detailed-walkthrough).  

Předběžných požadavků: NSG skupinu prostředků, virtuální síť, pomocí protokolu SSH příchozí, podsíť.

### <a name="create-a-vnic-with-a-static-internal-dns-name"></a>Vytvoření adaptéru VNic pomocí statické interní název DNS

Hello `-r` rozhraní příkazového řádku příznak je pro popisek DNS hello nastavení, která poskytuje hello statické název DNS pro hello VNic.

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

### <a name="deploy-hello-vm-into-hello-vnet-nsg-and-connect-hello-vnic"></a>Nasazení hello virtuálních počítačů do hello sítě VNet, NSG a připojit hello VNic

Hello `-N` hello VNic toohello připojí nový virtuální počítač během nasazení tooAzure hello.

```azurecli
azure vm create jenkins \
-g myResourceGroup \
-l westus \
-y linux \
-Q Debian \
-o myStorageAcct \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub \
-F myVNet \
-j mySubnet \
-N jenkinsVNic
```

## <a name="detailed-walkthrough"></a>Podrobný postup

Úplné průběžnou integraci a průběžné nasazování (CiCd) vyžaduje určité toobe statickou nebo dlohotrvající servery infrastruktury v Azure.  Doporučujeme, aby Azure prostředky jako hello virtuální sítě (virtuální sítě) a skupiny zabezpečení sítě (Nsg), by měla být statická a dlouhodobě prostředky, které jsou nasazeny zřídka.  Po nasazený virtuální síť, můžete použít znovu nová nasazení bez jakékoli infrastruktury toohello má negativní vliv.  Přidání toothis statické sítě Git úložiště server a server automatizace volaných přináší CiCd tooyour vývoj nebo testovací prostředí.  

Interní názvy DNS jsou pouze přeložit uvnitř virtuální sítě Azure.  Protože se jedná o interní hello názvy DNS, nejsou přeložit toohello mimo Internetu, poskytuje dodatečné zabezpečení toohello infrastruktury.

_Nahradí všechny příklady vlastní názvy._

## <a name="create-hello-resource-group"></a>Vytvořte skupinu prostředků hello

Skupina prostředků je potřebné tooorganize všechno se nám vytvořit v tomto návodu.  Další informace o skupinách prostředků Azure najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure group create myResourceGroup \
--location westus
```

## <a name="create-hello-vnet"></a>Vytvoření hello virtuální sítě

prvním krokem Hello je toobuild hello toolaunch virtuální sítě virtuálních počítačů do.  Hello virtuální síť obsahuje jednu podsíť pro účely tohoto postupu.  Další informace o virtuálních sítí Azure najdete v tématu [vytvoření virtuální sítě pomocí hello rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure network vnet create myVNet \
--resource-group myResourceGroup \
--address-prefixes 10.10.0.0/24 \
--location westus
```

## <a name="create-hello-nsg"></a>Vytvoření hello NSG

Hello podsíť je postavený za existující skupinu zabezpečení sítě, takže jsme sestavení hello NSG před hello podsítě.  Skupiny Nsg Azure jsou brány firewall ekvivalentní tooa hello síťové vrstvy.  Další informace o Azure Nsg najdete v tématu [jak toocreate skupin Nsg v hello rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure network nsg create myNSG \
--resource-group myResourceGroup \
--location westus
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Přidat pravidlo povolit příchozí SSH

virtuální počítač s Linuxem Hello potřebuje přístup z hello je nutná internet, pravidlo povolující příchozí port 22 provoz toobe předána hello sítě tooport 22 na hello virtuálního počítače s Linuxem.

```azurecli
azure network nsg rule create inboundSSH \
--resource-group myResourceGroup \
--nsg-name myNSG \
--access Allow \
--protocol Tcp \
--direction Inbound \
--priority 100 \
--source-address-prefix * \
--source-port-range * \
--destination-address-prefix 10.10.0.0/24 \
--destination-port-range 22
```

## <a name="add-a-subnet-toohello-vnet"></a>Přidat toohello podsíť virtuální sítě

Virtuální počítače v rámci hello virtuální síť musí být umístěny v podsíti.  Každý virtuální sítě může mít několik podsítí.  Vytvořit hello podsíť a podsíť hello přidružit hello NSG tooadd toohello podsíť brány firewall.

```azurecli
azure network vnet subnet create mySubNet \
--resource-group myResourceGroup \
--vnet-name myVNet \
--address-prefix 10.10.0.0/26 \
--network-security-group-name myNSG
```

Hello podsíť je nyní přidána uvnitř hello virtuální sítě a přidružené hello NSG a pravidla NSG hello.

## <a name="creating-static-dns-names"></a>Vytvoření statické názvy DNS

Azure je velmi flexibilní, ale toouse názvy DNS pro překlad názvů virtuálních počítačů, je nutné je jako virtuální síťové karty (VNics) pomocí DNS označování toocreate.  VNics jsou důležité, protože můžete znovu použít, je jejich připojením toodifferent virtuálních počítačů, které udržuje hello VNic jako statické prostředek hello virtuální počítače mohou být dočasné.  Pomocí DNS označování na hello VNic jsme možné tooenable jednoduchý název řešení z jiných virtuálních počítačů v hello virtuální sítě.  Automatizační server hello tooaccess jiné virtuální počítače pomocí přeložit názvy umožňuje podle názvu DNS hello `Jenkins` nebo server hello Git jako `gitrepo`.  Vytvoření adaptéru VNic a přidružte ji k hello podsíť vytvořili v předchozím kroku hello.

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a>Nasazení hello virtuálních počítačů do hello virtuální sítě a NSG

Nyní je k dispozici virtuální síť, podsíť uvnitř této virtuální síti a funguje jako brána firewall tooprotect naše podsíť blokováním veškerý příchozí provoz, s výjimkou port 22 pro SSH skupina NSG.  Teď můžou být nasazené Hello virtuálních počítačů uvnitř této stávající síťové infrastruktuře.

Pomocí rozhraní příkazového řádku Azure hello a hello `azure vm create` příkaz, je nasazené toohello existující skupiny prostředků Azure, virtuální síť, podsíť a VNic hello virtuálního počítače s Linuxem.  Další informace o používání rozhraní příkazového řádku toodeploy hello dokončení virtuálních počítačů najdete v tématu [vytvořit úplný prostředí Linux pomocí hello rozhraní příkazového řádku Azure](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure vm create jenkins \
--resource-group myResourceGroup myVM \
--location westus \
--os-type linux \
--image-urn Debian \
--storage-account-name mystorageaccount \
--admin-username myAdminUser \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--vnet-name myVNet \
--vnet-subnet-name mySubnet \
--nic-name jenkinsVNic
```

Rozhraní příkazového řádku příznaky pomocí hello toocall se stávajícími prostředky, jsme pokyn Azure toodeploy hello virtuálních počítačů uvnitř hello stávající sítě.  tooreiterate, jakmile nasazených virtuálních sítí a podsítí, mohou být levá jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.  

## <a name="next-steps"></a>Další kroky
* [Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
