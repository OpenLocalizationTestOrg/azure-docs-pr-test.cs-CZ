---
title: "Přidání uživatele do virtuálního počítače s Linuxem v Azure | Microsoft Docs"
description: "Přidání uživatele do virtuálního počítače s Linuxem v Azure."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8aa633b-8b75-45d7-b61d-11ac112cedd5
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/18/2016
ms.author: v-livech
ms.openlocfilehash: a95157f57c0cbd1f2a9ed68a0fe83140d7c9ec40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-user-to-an-azure-vm"></a>Přidání uživatele do virtuálního počítače Azure
Jedním z první úlohy na žádné nové virtuální počítače Linux je vytvoření nového uživatele.  V tomto článku jsme provede procesem vytvoření sudo uživatelský účet, nastavení hesla, přidání SSH veřejných klíčů a nakonec použít `visudo` umožňující sudo bez zadání hesla.

Požadavky jsou: [účet Azure](https://azure.microsoft.com/pricing/free-trial/), [veřejné a soukromé klíče SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), skupinu prostředků Azure a rozhraní příkazového řádku Azure nainstalovaný a přepnout do režimu Azure Resource Manager pomocí `azure config mode arm`.

## <a name="quick-commands"></a>Rychlé příkazy
```bash
# Add a new user on RedHat family distros
sudo useradd -G wheel exampleUser

# Add a new user on Debian family distros
sudo useradd -G sudo exampleUser

# Set a password
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

# Copy the SSH Public Key to the new user
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers to allow no password
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify the SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

## <a name="detailed-walkthrough"></a>Podrobný postup
### <a name="introduction"></a>Úvod
Jedním z první a nejběžnější úlohy s nový server je k přidání uživatelského účtu.  By mělo být zakázáno přihlášení kořenové a kořenového účtu samotné nepoužívejte server Linux, pouze sudo.  Poskytnutí zvýšení oprávnění pomocí příkazu sudo kořenové uživatele je správný způsob, jak spravovat a použít Linux.

Pomocí příkazu `useradd` přidáváme uživatelské účty do virtuálního počítače s Linuxem.  Spuštění `useradd` upraví `/etc/passwd`, `/etc/shadow`, `/etc/group`, a `/etc/gshadow`.  Přidáváme příkazového řádku příznak, který `useradd` příkazu také přidat nového uživatele do skupiny správné sudo v systému Linux.  I Zvu tě `useradd` vytvoří položku do `/etc/passwd` není přidělit nový uživatelský účet heslo.  Vytváříme počátečního hesla pro nového uživatele pomocí jednoduché `passwd` příkaz.  Posledním krokem je upravit pravidla sudo umožňující tohoto uživatele k provádění příkazů s oprávněními sudo bez nutnosti zadávat heslo pro každý příkaz.  Přihlášení pomocí soukromého klíče jsme se za předpokladu, že uživatelský účet je před nesprávnými účastníky a se má povolit přístup sudo bez zadání hesla.  

### <a name="adding-a-single-sudo-user-to-an-azure-vm"></a>Přidání uživatele jeden sudo na virtuální počítač Azure
Přihlaste se k virtuálnímu počítači Azure pomocí klíče SSH.  Pokud máte instalaci SSH veřejného klíče získat přístup, proveďte v tomto článku nejprve [pomocí ověření veřejného klíče s Azure](http://link.to/article).  

`useradd` Příkaz dokončí následující úkoly:

* Vytvořit nový uživatelský účet
* Vytvořte novou skupinu uživatelů se stejným názvem
* prázdná položka k přidání`/etc/passwd`
* prázdná položka k přidání`/etc/gpasswd`

`-G` Příkazového řádku příznak přidá nový uživatelský účet do správné skupiny Linux udělíte oprávnění eskalace kořenového nový uživatelský účet.

#### <a name="add-the-user"></a>Přidejte uživatele
```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a>Nastavení hesla
`useradd` Příkaz vytvoří uživatele a přidá položku do obou `/etc/passwd` a `/etc/gpasswd` , ale ve skutečnosti nenastaví heslo.  Heslo je přidaný do položky pomocí `passwd` příkaz.

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

Uživatel s oprávněními sudo máme teď na serveru.

### <a name="add-your-ssh-public-key-to-the-new-user-account"></a>Přidat váš veřejný klíč SSH pro nový uživatelský účet
Z počítače, použijte `ssh-copy-id` příkaz pomocí nového hesla.

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-to-allow-sudo-usage-without-a-password"></a>Pomocí visudo umožňující použití sudo bez zadání hesla
Pomocí `visudo` upravit `/etc/sudoers` souboru přidá několik vrstev ochranu proti nesprávně změny tohoto souboru důležité.  Při provádění `visudo`, `/etc/sudoers` soubor je uzamčený, aby žádný jiný uživatel provádět změny, když je aktivně upravována.  `/etc/sudoers` Chyb, kterých se také zkontrolována souboru `visudo` při pokusu uložit nebo ukončení, nelze uložit soubor porušený sudoers.

Připravili jsme uživatelé ve skupině správné výchozí pro přístup k příkazu "sudo".  Teď se zaměříme chcete povolit tyto skupiny používat sudo bez hesla.

```bash
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### <a name="verify-the-user-ssh-keys-and-sudo"></a>Ověření uživatele, ssh klíče a sudo
```bash
# Verify the SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
