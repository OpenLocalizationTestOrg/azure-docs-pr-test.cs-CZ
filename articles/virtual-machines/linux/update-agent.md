---
title: aaaUpdate hello Azure Linux Agent z webu GitHub | Microsoft Docs
description: "Zjistěte, jak tooupdate Azure Linux Agent pro váš virtuální počítač s Linuxem v Azure toohello nejnovější verzi z webu GitHub"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: f1f19300-987d-4f29-9393-9aba866f049c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: mingzhan
ms.openlocfilehash: 4ce7c56efc1e6563e6415f7687573f9fb9e7b4c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-hello-azure-linux-agent-on-a-vm"></a>Jak tooupdate hello Azure Linux Agent na virtuálním počítači

tooupdate vaše [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) na virtuální počítač s Linuxem v Azure, musíte už mít:

- Spuštěného virtuálního počítače s Linuxem v Azure.
- Toothat připojení virtuálního počítače s Linuxem pomocí protokolu SSH.

Vždy zkontrolujte pro balíček v úložišti distro Linux hello nejdřív. Je možné hello balíčku k dispozici nemusí být hello nejnovější verzi, ale povolení automatických aktualizací zajistí hello agenta systému Linux bude vždycky získat nejnovější aktualizace hello. Budete mít problémy instalace z hello balíček správci, byste se měli obrátit podporu od dodavatele distro hello.

## <a name="updating-hello-azure-linux-agent"></a>Aktualizace hello Azure Linux Agent

## <a name="ubuntu"></a>Ubuntu

#### <a name="check-your-current-package-version"></a>Zkontrolujte aktuální verzi balíčku

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a>Aktualizace mezipaměti balíčku

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a>Nainstalujte nejnovější verzi balíčku hello

```bash
sudo apt-get install walinuxagent
```

#### <a name="ensure-auto-update-is-enabled"></a>Ujistěte se, že je povolena automatická aktualizace

Nejdřív zkontrolujte toosee, je-li aktivní:

```bash
cat /etc/waagent.conf
```

Najít 'AutoUpdate.Enabled'. Pokud se zobrazí tento výstup, je povoleno:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

tooenable spustit:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Restartujte službu příkaz waagent hello

#### <a name="restart-agent-for-1404"></a>Restartovat agenta 14.04

```bash
initctl restart walinuxagent
```

#### <a name="restart-agent-for-1604--1704"></a>Restartovat agenta 16.04 / 17.04

```bash
systemctl restart walinuxagent.service
```

## <a name="debian"></a>Debian

### <a name="debian-7-wheezy"></a>Debian 7 "Wheezy"

#### <a name="check-your-current-package-version"></a>Zkontrolujte aktuální verzi balíčku

```bash
dpkg -l | grep waagent
```

#### <a name="update-package-cache"></a>Aktualizace mezipaměti balíčku

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a>Nainstalujte nejnovější verzi balíčku hello

```bash
sudo apt-get install waagent
```

#### <a name="enable-agent-auto-update"></a>Povolit automatické aktualizace agenta
Tato verze Debian nemá na verzi > = 2.0.16, proto není k dispozici pro něj automatických aktualizací. výstup Hello hello výše příkazu se zobrazí, pokud balíček hello je aktuální.

### <a name="debian-8-jessie--debian-9-stretch"></a>Debian 8 "Klára" / Debian 9 "Stretch"

#### <a name="check-your-current-package-version"></a>Zkontrolujte aktuální verzi balíčku

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a>Aktualizace mezipaměti balíčku

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a>Nainstalujte nejnovější verzi balíčku hello

```bash
sudo apt-get install waagent
```
#### <a name="ensure-auto-update-is-enabled"></a>Ujistěte se, že je povolena automatická aktualizace 

Nejdřív zkontrolujte toosee, je-li aktivní:

```bash
cat /etc/waagent.conf
```

Najít 'AutoUpdate.Enabled'. Pokud se zobrazí tento výstup, je povoleno:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

tooenable spustit:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Restartujte službu příkaz waagent hello

```
sudo systemctl restart walinuxagent.service
```

## <a name="redhat--centos"></a>Red Hat nebo CentOS

### <a name="rhelcentos-6"></a>RHEL nebo CentOS 6

#### <a name="check-your-current-package-version"></a>Zkontrolujte aktuální verzi balíčku

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a>Zkontrolujte dostupné aktualizace

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-hello-latest-package-version"></a>Nainstalujte nejnovější verzi balíčku hello

```bash
sudo yum install WALinuxAgent
```

#### <a name="ensure-auto-update-is-enabled"></a>Ujistěte se, že je povolena automatická aktualizace 

Nejdřív zkontrolujte toosee, je-li aktivní:

```bash
cat /etc/waagent.conf
```

Najít 'AutoUpdate.Enabled'. Pokud se zobrazí tento výstup, je povoleno:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

tooenable spustit:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Restartujte službu příkaz waagent hello

```
sudo service waagent restart
```

### <a name="rhelcentos-7"></a>RHEL nebo CentOS 7

#### <a name="check-your-current-package-version"></a>Zkontrolujte aktuální verzi balíčku

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a>Zkontrolujte dostupné aktualizace

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-hello-latest-package-version"></a>Nainstalujte nejnovější verzi balíčku hello

```bash
sudo yum install WALinuxAgent  
```

#### <a name="ensure-auto-update-is-enabled"></a>Ujistěte se, že je povolena automatická aktualizace 

Nejdřív zkontrolujte toosee, je-li aktivní:

```bash
cat /etc/waagent.conf
```

Najít 'AutoUpdate.Enabled'. Pokud se zobrazí tento výstup, je povoleno:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

tooenable spustit:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Restartujte službu příkaz waagent hello

```bash
sudo systemctl restart waagent.service
```

## <a name="suse-sles"></a>SUSE SLES

### <a name="suse-sles-11-sp4"></a>SUSE SLES 11 SP4

#### <a name="check-your-current-package-version"></a>Zkontrolujte aktuální verzi balíčku

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a>Zkontrolujte dostupné aktualizace

Hello výše výstupu se zobrazí, pokud je balíček hello až toodate.

#### <a name="install-hello-latest-package-version"></a>Nainstalujte nejnovější verzi balíčku hello

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a>Ujistěte se, že je povolena automatická aktualizace 

Nejdřív zkontrolujte toosee, je-li aktivní:

```bash
cat /etc/waagent.conf
```

Najít 'AutoUpdate.Enabled'. Pokud se zobrazí tento výstup, je povoleno:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

tooenable spustit:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Restartujte službu příkaz waagent hello

```bash
sudo /etc/init.d/waagent restart
```

### <a name="suse-sles-12-sp2"></a>SUSE SLES 12 SP2

#### <a name="check-your-current-package-version"></a>Zkontrolujte aktuální verzi balíčku

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a>Zkontrolujte dostupné aktualizace

Ve výstupu hello z výše uvedených hello to vám ukáže, pokud je balíček hello maximálně datum.

#### <a name="install-hello-latest-package-version"></a>Nainstalujte nejnovější verzi balíčku hello

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a>Ujistěte se, že je povolena automatická aktualizace 

Nejdřív zkontrolujte toosee, je-li aktivní:

```bash
cat /etc/waagent.conf
```

Najít 'AutoUpdate.Enabled'. Pokud se zobrazí tento výstup, je povoleno:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

tooenable spustit:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Restartujte službu příkaz waagent hello

```bash
sudo systemctl restart waagent.service
```

## <a name="oracle-6-and-7"></a>Oracle 6 a 7

Pro Oracle Linux, ujistěte se, že hello `Addons` je povoleno úložiště. Vybrat soubor hello tooedit `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) nebo `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux) a změňte řádek hello `enabled=0` příliš`enabled=1` pod **[ol6_addons]** nebo **[ol7_addons]** v tomto souboru.

Potom tooinstall hello nejnovější verzi hello Azure Linux Agent, typ:

```bash
sudo yum install WALinuxAgent
```

Pokud nenajdete hello rozšíření úložiště můžete jednoduše přidat tyto řádky na konci hello .repo souboru podle verze tooyour Oracle Linux:

Pro virtuální počítače, Oracle Linux 6:

```sh
[ol6_addons]
name=Add-Ons for Oracle Linux $releasever ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
gpgcheck=1
enabled=1
```

Pro virtuální počítače, Oracle Linux 7:

```sh
[ol7_addons]
name=Oracle Linux $releasever Add ons ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0
```

Poté zadejte:

```bash
sudo yum update WALinuxAgent
```

Obvykle to je všechno, co potřebujete, ale pokud z nějakého důvodu že potřebujete tooinstall z https://github.com přímo, hello použijte následující kroky.


## <a name="update-hello-linux-agent-when-no-agent-package-exists-for-distribution"></a>Hello aktualizace agenta systému Linux při žádný balíček agenta existuje pro distribuci

Nainstalujte wget (existují zde některých distribucích, který nemusíte instalovat ve výchozím nastavení, jako je například Redhat a CentOS, Oracle Linux verze 6.4 a 6.5) tak, že zadáte `sudo yum install wget` na příkazovém řádku hello.

### <a name="1-download-hello-latest-version"></a>1. Stažení nejnovější verze hello
Otevřete [hello verzi Azure Linux Agent v Githubu](https://github.com/Azure/WALinuxAgent/releases) ve webové stránky a zjistěte hello nejnovější číslo verze. (Vaší aktuální verzí můžete vyhledat zadáním `waagent --version`.)

#### <a name="for-version-22x-or-later-type"></a>Pro verzi 2.2.x nebo novější, zadejte:
```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.x.zip
unzip v2.2.x.zip.zip
cd WALinuxAgent-2.2.x
```

Hello následující řádek používá verzi 2.2.0 jako příklad:

```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.14.zip
unzip v2.2.14.zip  
cd WALinuxAgent-2.2.14
```

### <a name="2-install-hello-azure-linux-agent"></a>2. Nainstalujte hello Azure Linux Agent

#### <a name="for-version-22x-use"></a>Pro verzi 2.2.x, použijte:
Může být nutné balíček hello tooinstall `setuptools` nejprve – Viz [zde](https://pypi.python.org/pypi/setuptools). Potom spusťte:

```bash
sudo python setup.py install
```

#### <a name="ensure-auto-update-is-enabled"></a>Ujistěte se, že je povolena automatická aktualizace

Nejdřív zkontrolujte toosee, je-li aktivní:

```bash
cat /etc/waagent.conf
```

Najít 'AutoUpdate.Enabled'. Pokud se zobrazí tento výstup, je povoleno:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

tooenable spustit:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="3-restart-hello-waagent-service"></a>3. Restartujte službu příkaz waagent hello
Pro většinu distribucích systému Linux:

```bash
sudo service waagent restart
```

Ubuntu použijte:

```bash
sudo service walinuxagent restart
```

Pro jádro operačního systému použijte:

```bash
sudo systemctl restart waagent
```

### <a name="4-confirm-hello-azure-linux-agent-version"></a>4. Ověřit verzi Azure Linux Agent hello
    
```bash
waagent -version
```

Pro jádro operačního systému nemusí fungovat hello výše příkaz.

Uvidíte, že tento hello Azure Linux Agent byla aktualizována toohello novou verzi.

Další informace týkající se hello Azure Linux Agent najdete v tématu [Azure Linux Agent README](https://github.com/Azure/WALinuxAgent).
