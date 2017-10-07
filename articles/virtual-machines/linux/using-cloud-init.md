---
title: "toocustomize aaaUse init cloudu virtuálního počítače s Linuxem | Microsoft Docs"
description: "Jak cloud init toocustomize toouse a virtuální počítač s Linuxem během vytváření s hello 2.0 rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: e7297e162fc73f0da42f195bec2fcbe23b310c1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-cloud-init-toocustomize-a-linux-vm-during-creation"></a>Použít během vytváření toocustomize init cloudu virtuálního počítače s Linuxem
Tento článek ukazuje, jak toomake tooset skriptu cloudu init hello název hostitele, aktualizace nainstalované balíčky a spravovat uživatelské účty s hello 2.0 rozhraní příkazového řádku Azure. Hello cloudu init skriptů jsou volány při vytvoření virtuálního počítače (VM) z příkazového řádku Azure. Podrobnější přehled o instalaci aplikací, zápisu konfigurační soubory a vložení klíče z Key Vault, najdete v části [v tomto kurzu](tutorial-automate-vm-deployment.md). Můžete také provést tyto kroky hello [Azure CLI 1.0](using-cloud-init-nodejs.md).

## <a name="quick-commands"></a>Rychlé příkazy
Vytvoření cloudové init.txt skript, který nastaví hello název hostitele, aktualizuje všechny balíčky a přidá tooLinux uživatele sudo.

```yaml
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

Vytvoření toolaunch skupiny prostředků virtuálních počítačů do s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří hello skupinu prostředků s názvem *myResourceGroup*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init tooconfigure ji během spouštění s hello `--custom-data` parametr.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Podrobný postup
Při spuštění nového virtuálního počítače s Linuxem se zobrazuje standardní virtuální počítač s Linuxem se nic, vlastní nebo připraveno k vašim potřebám. [Init cloudu](https://cloudinit.readthedocs.org) je standardní způsob tooinject skriptu nebo konfigurace nastavení do tohoto virtuálního počítače s Linuxem, jako je spuštění pro službu hello poprvé.

V Azure, je několik různých způsobů toomake změny do virtuálního počítače s Linuxem jak je právě nasazen nebo spuštěn.

* Vložit skripty s použitím init cloudu.
* Vložit skripty s použitím hello Azure [rozšíření VMAccess](using-vmaccess-extension.md).
* Šablony Azure pomocí init cloudu.
* K pomocí šablony Azure [CustomScriptExtention](extensions-customscript.md).

skripty tooinject kdykoli po spuštění:

* SSH toorun přímo příkazy
* Vložit skripty s použitím hello Azure [rozšíření VMAccess](using-vmaccess-extension.md), imperativní nebo šablony Azure
* Nástroje pro správu konfigurace, jako Ansible, Salt, Chef nebo Puppet.

> [!NOTE]
> Rozšíření VMAccess provádí skript jako kořenový v hello stejný způsobem pomocí protokolu SSH můžete. Však pomocí rozšíření virtuálního počítače hello povolí několik funkcí této nabídky Azure, které mohou být užitečné, v závislosti na vašem scénáři.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Init cloudu dostupnosti pro virtuální počítač Azure rychlým vytvořením bitové kopie aliasy:
| Alias | Vydavatel | Nabídka | Skladová jednotka (SKU) | Verze | init cloudu |
|:--- |:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |nejnovější |Ne |
| CoreOS |CoreOS |CoreOS |Stable |nejnovější |Ano |
| Debian |credativ |Debian |8 |nejnovější |Ne |
| openSUSE |SUSE |openSUSE |13.2 |nejnovější |Ne |
| RHEL |Redhat |RHEL |7.2 |nejnovější |Ne |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |nejnovější |Ano |

Snažíme se práce s našich partnerů tooget cloudu inicializací zahrnuté a práci v hello bitové kopie, aby umožňovala tooAzure.

## <a name="add-a-cloud-init-script-toohello-vm-creation-with-hello-azure-cli"></a>Přidejte cloudu init skriptu toohello vytvoření virtuálních počítačů s hello rozhraní příkazového řádku Azure
toolaunch skript cloudu init při vytváření virtuálního počítače v Azure, zadejte soubor cloudu init hello pomocí rozhraní příkazového řádku Azure hello `--custom-data` přepínače.

Vytvoření toolaunch skupiny prostředků virtuálních počítačů do s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří hello skupinu prostředků s názvem *myResourceGroup*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init tooconfigure ji během spouštění.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="create-a-cloud-init-script-tooset-hello-hostname-of-a-linux-vm"></a>Vytvoření cloudu init skriptu tooset hello název hostitele virtuálního počítače s Linuxem
Jedním z nejdůležitějších kroků nastavení pro všechny virtuální počítač s Linuxem a hello nejjednodušší by hello název hostitele. Jsme tento parametr můžete snadno nastavit pomocí init cloudových – pomocí tohoto skriptu.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Příklad cloudu init skript s názvem `cloud_config_hostname.txt`.
```yaml
#cloud-config
hostname: myservername
```

Během hello počáteční spouštění hello virtuálních počítačů, tento skript cloudu init nastaví název hostitele hello příliš*NázevServeru*. Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init tooconfigure ji během spouštění.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

Přihlášení a ověřte název hostitele hello hello nového virtuálního počítače.

```bash
ssh myVM
hostname
myservername
```

## <a name="create-a-cloud-init-script"></a>Vytvoření skriptu init cloudu
Pro zabezpečení budete chtít vaší tooupdate virtuálního počítače s Ubuntu na hello při prvním spuštění. Pomocí cloudu init můžeme udělat, aby s hello podle skriptu, v závislosti na distribuční hello Linux, který používáte.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-hello-debian-family"></a>Ukázkový skript cloudu init `cloud_config_apt_upgrade.txt` pro hello Debian rodiny
```yaml
#cloud-config
apt_upgrade: true
```

Po Linux, jsou aktualizovány všechny balíčky hello nainstalovaná prostřednictvím **výstižný get**. Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init tooconfigure ji během spouštění.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
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

## <a name="create-a-cloud-init-script-tooadd-a-user-toolinux"></a>Vytvoření cloudu init skriptu tooadd tooLinux uživatele
Jedním z první úlohy hello na žádné nové virtuální počítače Linux je tooadd uživatele pro sebe nebo pomocí tooavoid *kořenové*. SSH klíče jsou vhodné pro zabezpečení a použitelnost a přidají se toohello *~/.ssh/authorized_keys* souboru pomocí tohoto skriptu init cloudu.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Ukázkový skript cloudu init `cloud_config_add_users.txt` pro Debian řadu
```yaml
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Po Linux, všichni uživatelé hello uvedené jsou skupiny vytvořené a přidané toohello sudo. Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init tooconfigure ji během spouštění.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
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

[O rozšíření virtuálního počítače a funkce](extensions-features.md)

[Spravovat uživatele, SSH a zkontrolujte nebo hello oprava disky na virtuální počítače Azure s Linuxem pomocí rozšíření VMAccess](using-vmaccess-extension.md)
