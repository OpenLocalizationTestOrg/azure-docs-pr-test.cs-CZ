---
title: "toocustomize aaaUsing init cloudu virtuálního počítače s Linuxem během vytváření v Azure | Microsoft Docs"
description: "Jak cloud init toocustomize toouse a virtuální počítač s Linuxem během vytváření s hello Azure CLI 1.0"
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
ms.date: 10/26/2016
ms.author: v-livech
ms.openlocfilehash: b9f480bd04029956d0593bbef931795733cbc2f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-cloud-init-toocustomize-a-linux-vm-during-creation-with-hello-azure-cli-10"></a>Použít během vytváření s hello Azure CLI 1.0 toocustomize init cloudu virtuálního počítače s Linuxem
Tento článek ukazuje, jak toomake tooset skriptu cloudu init hello název hostitele, aktualizace nainstalované balíčky a správa uživatelských účtů.  Hello cloudu init skripty se nazývají během hello vytvoření virtuálního počítače z příkazového řádku Azure.  článek Hello vyžaduje:

* účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/))
* Hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) přihlášení `azure login`.
* Hello rozhraní příkazového řádku Azure *musí být v* režimu Azure Resource Manager `azure config mode arm`.

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello

## <a name="quick-commands"></a>Rychlé příkazy
Vytvoření cloudové init.txt skript, který nastaví hello název hostitele, aktualizuje všechny balíčky a přidá tooLinux uživatele sudo.

```sh
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
Vytvořte virtuální počítače do toolaunch skupiny prostředků.

```azurecli
azure group create myResourceGroup westus
```

Vytvoření virtuálního počítače s Linuxem pomocí cloudu init tooconfigure ji během spouštění.

```azurecli
azure vm create \
  -g myResourceGroup \
  -n myVM \
  -l westus \
  -y Linux \
  -f myVMnic \
  -F myVNet \
  -P 10.0.0.0/22 \
  -j mySubnet \
  -k 10.0.0.0/24 \
  -Q canonical:ubuntuserver:14.04.2-LTS:latest \
  -M ~/.ssh/id_rsa.pub \
  -u myAdminUser \
  -C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Podrobný postup
### <a name="introduction"></a>Úvod
Při spuštění nového virtuálního počítače s Linuxem se zobrazuje standardní virtuální počítač s Linuxem se nic, vlastní nebo připraveno k vašim potřebám. [Init cloudu](https://cloudinit.readthedocs.org) je standardní způsob tooinject skriptu nebo konfigurace nastavení do tohoto virtuálního počítače s Linuxem, jako je spuštění pro službu hello poprvé.

V Azure, existují tři různé způsoby toomake změny do virtuálního počítače s Linuxem jako je právě nasazen nebo spuštěn.

* Vložit skripty s použitím init cloudu.
* Vložit skripty s použitím hello Azure [rozšíření VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Šablony Azure pomocí init cloudu.
* K pomocí šablony Azure [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

skripty tooinject kdykoli po spuštění:

* SSH toorun přímo příkazy
* Vložit skripty s použitím hello Azure [rozšíření VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), imperativní nebo šablony Azure
* Nástroje pro správu konfigurace, jako Ansible, Salt, Chef nebo Puppet.

> [!NOTE]
> : Rozšíření VMAccess spustí skript jako kořenový v hello stejný způsobem pomocí protokolu SSH můžete.  Však pomocí rozšíření virtuálního počítače hello povolí několik funkcí této nabídky Azure, které mohou být užitečné, v závislosti na vašem scénáři.
> 
> 

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Init cloudu dostupnosti pro virtuální počítač Azure rychlým vytvořením bitové kopie aliasy:
| Alias | Vydavatel | Nabídka | Skladová jednotka (SKU) | Verze | init cloudu |
|:--- |:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |nejnovější |Ne |
| CoreOS |CoreOS |CoreOS |Stable |nejnovější |Ano |
| Debian |credativ |Debian |8 |nejnovější |Ne |
| openSUSE |SUSE |openSUSE |13.2 |nejnovější |Ne |
| RHEL |Redhat |RHEL |7.2 |nejnovější |Ne |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |nejnovější |Ano |

Společnost Microsoft se práce s našich partnerů tooget cloudu inicializací zahrnuté a práci v hello bitové kopie, aby umožňovala tooAzure.

## <a name="adding-a-cloud-init-script-toohello-vm-creation-with-hello-azure-cli"></a>Přidání cloudu init skriptu toohello vytvoření virtuálních počítačů s hello rozhraní příkazového řádku Azure
toolaunch skript cloudu init při vytváření virtuálního počítače v Azure, zadejte soubor cloudu init hello pomocí rozhraní příkazového řádku Azure hello `--custom-data` přepínače.

Vytvořte virtuální počítače do toolaunch skupiny prostředků.

```azurecli
azure group create myResourceGroup westus
```

Vytvoření virtuálního počítače s Linuxem pomocí cloudu init tooconfigure ji během spouštění.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubnet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-tooset-hello-hostname-of-a-linux-vm"></a>Vytváření cloudu init skriptu tooset hello název hostitele virtuálního počítače s Linuxem
Jedním z nejdůležitějších kroků nastavení pro všechny virtuální počítač s Linuxem a hello nejjednodušší by hello název hostitele. Jsme tento parametr můžete snadno nastavit pomocí init cloudových – pomocí tohoto skriptu.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Příklad cloudu init skript s názvem `cloud_config_hostname.txt`.
```sh
#cloud-config
hostname: myservername
```

Během hello počáteční spouštění hello virtuálních počítačů, tento skript cloudu init nastaví název hostitele hello příliš`myservername`.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_hostname.txt
```

Přihlášení a ověřte název hostitele hello hello nového virtuálního počítače.

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-tooupdate-linux"></a>Vytváření cloudu init skriptu tooupdate Linux
Pro zabezpečení budete chtít vaší tooupdate virtuálního počítače s Ubuntu na hello při prvním spuštění.  Pomocí cloudu init můžeme udělat, aby s hello podle skriptu, v závislosti na distribuční hello Linux, který používáte.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-hello-debian-family"></a>Ukázkový skript cloudu init `cloud_config_apt_upgrade.txt` pro hello Debian rodiny
```sh
#cloud-config
apt_upgrade: true
```

Po Linux, jsou aktualizovány všechny balíčky hello nainstalovaná prostřednictvím `apt-get`.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_apt_upgrade.txt
```

Přihlášení a ověření, jsou aktualizovány všechny balíčky.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
hello following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 tooremove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-tooadd-a-user-toolinux"></a>Vytváření cloudu init skriptu tooadd tooLinux uživatele
Jedním z první úlohy hello na žádné nové virtuální počítače Linux je tooadd uživatele pro sebe nebo pomocí tooavoid `root`. SSH klíče jsou vhodné pro zabezpečení a použitelnost a přidají se toohello `~/.ssh/authorized_keys` souboru pomocí tohoto skriptu init cloudu.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Ukázkový skript cloudu init `cloud_config_add_users.txt` pro Debian řadu
```sh
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Po Linux, všichni uživatelé hello uvedené jsou skupiny vytvořené a přidané toohello sudo.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_add_users.txt
```

Přihlášení a ověření hello nově vytvořeného uživatele.

```bash
ssh myVM
cat /etc/group
```

Výstup

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Další kroky
Init cloudu se stává stále jednu standardní způsob toomodify virtuálním počítačům s Linuxem na spuštění. Azure má také rozšíření virtuálních počítačů, které umožňují toomodify vaše LinuxVM na spouštěcí nebo když je spuštěná. Například můžete hello Azure VMAccessExtension tooreset SSH nebo informace o uživateli při hello virtuálních počítačů. S inicializací cloudu bude třeba heslo hello tooreset restartování.

[O rozšíření virtuálního počítače a funkce](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Spravovat uživatele, SSH a zkontrolujte nebo hello oprava disky na virtuální počítače Azure s Linuxem pomocí rozšíření VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

