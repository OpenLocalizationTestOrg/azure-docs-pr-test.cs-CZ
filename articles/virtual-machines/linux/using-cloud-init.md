---
title: "Použít cloudové init k přizpůsobení virtuálního počítače s Linuxem | Microsoft Docs"
description: "Jak používat cloudové init k přizpůsobení virtuálního počítače s Linuxem během vytváření pomocí Azure CLI 2.0"
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
ms.openlocfilehash: a7a6daad34525683579e25b9591ed28f2bf29c04
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-during-creation"></a>Slouží k přizpůsobení virtuálního počítače s Linuxem během vytváření init cloudu
Tento článek ukazuje, jak provádět skript cloudu init nastavit název hostitele, aktualizace nainstalované balíčky a správa uživatelských účtů s Azure CLI 2.0. Skripty cloud init se označují jako při vytvoření virtuálního počítače (VM) z příkazového řádku Azure. Podrobnější přehled o instalaci aplikací, zápisu konfigurační soubory a vložení klíče z Key Vault, najdete v části [v tomto kurzu](tutorial-automate-vm-deployment.md). K provedení těchto kroků můžete také využít [Azure CLI 1.0](using-cloud-init-nodejs.md).

## <a name="quick-commands"></a>Rychlé příkazy
Vytvoření cloudové init.txt skript, který nastaví název hostitele, aktualizuje všechny balíčky a přidá uživatele sudo do systému Linux.

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

Vytvořte skupinu prostředků ke spuštění virtuálních počítačů do s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init konfigurace během spouštění s `--custom-data` parametr.

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
Při spuštění nového virtuálního počítače s Linuxem se zobrazuje standardní virtuální počítač s Linuxem se nic, vlastní nebo připraveno k vašim potřebám. [Init cloudu](https://cloudinit.readthedocs.org) je standardní způsob vložení skriptu nebo konfigurace nastavení do tohoto virtuálního počítače s Linuxem, jako je spuštění pro první.

V Azure, existuje několik různých způsobů provést změny do virtuálního počítače s Linuxem, jako je právě nasazen nebo spuštěn.

* Vložit skripty s použitím init cloudu.
* Vložit skripty s použitím Azure [rozšíření VMAccess](using-vmaccess-extension.md).
* Šablony Azure pomocí init cloudu.
* K pomocí šablony Azure [CustomScriptExtention](extensions-customscript.md).

Chcete-li vložit skripty kdykoli po spuštění:

* SSH ke spuštění příkazů přímo
* Vložit skripty s použitím Azure [rozšíření VMAccess](using-vmaccess-extension.md), imperativní nebo šablony Azure
* Nástroje pro správu konfigurace, jako Ansible, Salt, Chef nebo Puppet.

> [!NOTE]
> Rozšíření VMAccess spustí skript jako kořenové stejným způsobem jako pomocí protokolu SSH můžete. Však pomocí rozšíření virtuálního počítače umožňuje několik funkcí této nabídky Azure, které mohou být užitečné, v závislosti na vašem scénáři.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Init cloudu dostupnosti pro virtuální počítač Azure rychlým vytvořením bitové kopie aliasy:
| Alias | Vydavatel | Nabídka | Skladová jednotka (SKU) | Verze | init cloudu |
|:--- |:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |nejnovější |Ne |
| CoreOS |CoreOS |CoreOS |Stable |nejnovější |Ano |
| Debian |credativ |Debian |8 |nejnovější |Ne |
| openSUSE |SUSE |openSUSE |13.2 |nejnovější |Ne |
| RHEL |Redhat |RHEL |7.2 |nejnovější |Ne |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |nejnovější |Ano |

Pracujeme s našimi partnery získat cloudu init zahrnuté a práci v bitové kopie, které poskytují do Azure.

## <a name="add-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>Přidat skript cloudu init vytvoření virtuálních počítačů pomocí Azure CLI
Spusťte skript cloudu init při vytváření virtuálního počítače v Azure, zadejte soubor init cloudu pomocí Azure CLI `--custom-data` přepínače.

Vytvořte skupinu prostředků ke spuštění virtuálních počítačů do s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init konfigurace během spouštění.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="create-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Vytvoření skriptu cloudu init nastavit název hostitele virtuálního počítače s Linuxem
Jedním z nejjednodušší a nejdůležitější nastavení pro všechny virtuální počítač s Linuxem bude název hostitele. Jsme tento parametr můžete snadno nastavit pomocí init cloudových – pomocí tohoto skriptu.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Příklad cloudu init skript s názvem `cloud_config_hostname.txt`.
```yaml
#cloud-config
hostname: myservername
```

Při prvním spuštění virtuálního počítače, nastaví tento skript cloudu init název hostitele na *NázevServeru*. Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init konfigurace během spouštění.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

Přihlášení a ověřte název hostitele nového virtuálního počítače.

```bash
ssh myVM
hostname
myservername
```

## <a name="create-a-cloud-init-script"></a>Vytvoření skriptu init cloudu
Pro zabezpečení budete chtít vaší virtuálního počítače s Ubuntu aktualizovat při prvním spuštění. Pomocí cloudu init, které jsme to udělat pomocí skriptu postupujte podle kroků, v závislosti na Linux distribuce, kterou používáte.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Ukázkový skript cloudu init `cloud_config_apt_upgrade.txt` pro Debian rodinu
```yaml
#cloud-config
apt_upgrade: true
```

Po Linux, jsou aktualizovány všechny nainstalované balíčky prostřednictvím **výstižný get**. Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init konfigurace během spouštění.

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
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="create-a-cloud-init-script-to-add-a-user-to-linux"></a>Vytvoření skriptu cloudu init přidání uživatele do systému Linux
Jedním z první úlohy na žádné nové virtuální počítače Linux je chcete přidat uživatele pro sebe nebo nepoužívejte *kořenové*. SSH klíče jsou vhodné pro zabezpečení a použitelnost a jejich přidání do *~/.ssh/authorized_keys* souboru pomocí tohoto skriptu init cloudu.

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

Po Linux, jsou uvedené uživatele vytvořen a přidán do skupiny sudo. Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init konfigurace během spouštění.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_add_users.txt
```

Přihlášení a ověření nově vytvořeného uživatele.

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
Init cloudu se stává stále jednu standardní způsob, jak upravit virtuálním počítačům s Linuxem na spuštění. Azure má také rozšíření virtuálních počítačů, které umožňují upravit vaše LinuxVM na spouštěcí nebo když je spuštěná. Například můžete použít Azure VMAccessExtension resetovat informace SSH nebo uživatele, když běží virtuální počítač. S inicializací cloudu je zapotřebí restartování k resetování hesla.

[O rozšíření virtuálního počítače a funkce](extensions-features.md)

[Spravovat uživatele, SSH a zkontrolujte nebo opravte disky na virtuálních počítačích Azure Linux pomocí rozšíření VMAccess](using-vmaccess-extension.md)