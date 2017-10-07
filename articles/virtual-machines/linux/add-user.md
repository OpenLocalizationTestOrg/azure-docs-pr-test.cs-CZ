---
title: "aaaAdd tooa uživatele virtuálního počítače s Linuxem v Azure | Microsoft Docs"
description: "Přidejte uživatele tooa virtuálního počítače s Linuxem v Azure."
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
ms.openlocfilehash: eed050154adf0cbed2c168e7aa675bd3ded85fcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-user-tooan-azure-vm"></a>Přidat uživatele tooan virtuálního počítače Azure
Jedním z první úlohy hello na žádné nové virtuální počítače Linux je toocreate nového uživatele.  V tomto článku jsme provede procesem vytvoření sudo uživatelský účet, heslo hello nastavení, přidání SSH veřejných klíčů a nakonec použít `visudo` tooallow sudo bez zadání hesla.

Požadavky jsou: [účet Azure](https://azure.microsoft.com/pricing/free-trial/), [veřejné a soukromé klíče SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), skupinu prostředků Azure a hello rozhraní příkazového řádku Azure nainstalované a přepnout pomocí režimu Resource Manager tooAzure `azure config mode arm`.

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

# Copy hello SSH Public Key toohello new user
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers tooallow no password
# Execute visudo as root tooedit hello /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# toothis
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo tooexecute any command
%sudo   ALL=(ALL:ALL) ALL

# toothis
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify hello SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

## <a name="detailed-walkthrough"></a>Podrobný postup
### <a name="introduction"></a>Úvod
Jedním z hello první a nejběžnější úlohy na nový server je tooadd uživatelský účet.  By mělo být zakázáno přihlášení kořenové a nepoužívejte hello kořenového účtu samotné s server Linux, pouze sudo.  Udělení oprávnění pomocí příkazu "sudo" jej hello správný způsob tooadminister a používat Linux eskalace kořenové uživatele.

Pomocí příkazu hello `useradd` přidáváme uživatelské účty toohello virtuálního počítače s Linuxem.  Spuštění `useradd` upraví `/etc/passwd`, `/etc/shadow`, `/etc/group`, a `/etc/gshadow`.  Přidáváme příkazového řádku příznak toohello `useradd` příkaz tooalso přidat hello novou toohello správné sudo skupinu uživatelů v systému Linux.  I Zvu tě `useradd` vytvoří položku do `/etc/passwd` neudělí hello nový uživatelský účet heslo.  Vytváříme počátečního hesla pro nového uživatele hello pomocí jednoduchého hello `passwd` příkaz.  posledním krokem Hello je toomodify hello sudo pravidla tooallow příkazy tooexecute tohoto uživatele s oprávněními sudo bez nutnosti tooenter heslo pro každý příkaz.  Přihlášení pomocí soukromého klíče hello jsme se za předpokladu, že uživatelský účet je před nesprávnými účastníky a budou tooallow sudo přístup bez zadání hesla.  

### <a name="adding-a-single-sudo-user-tooan-azure-vm"></a>Přidávání jednoho sudo uživatele tooan virtuálního počítače Azure
Přihlaste se toohello virtuálního počítače Azure pomocí klíče SSH.  Pokud máte instalaci SSH veřejného klíče získat přístup, proveďte v tomto článku nejprve [pomocí ověření veřejného klíče s Azure](http://link.to/article).  

Hello `useradd` dokončení příkazu hello následující úlohy:

* Vytvořit nový uživatelský účet
* Vytvořit novou skupinu uživatelů s hello stejným názvem
* Přidat prázdná položka příliš`/etc/passwd`
* Přidat prázdná položka příliš`/etc/gpasswd`

Hello `-G` příkazového řádku příznak přidá hello novou účet toohello správné Linux skupinu uživatelů udělíte oprávnění eskalace kořenového hello nový uživatelský účet.

#### <a name="add-hello-user"></a>Přidat uživatele hello
```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a>Nastavení hesla
Hello `useradd` příkaz vytvoří hello uživatele a přidá položka tooboth `/etc/passwd` a `/etc/gpasswd` , ale ve skutečnosti nenastaví hello heslo.  Hello heslo je přidána toohello položky na základě hello `passwd` příkaz.

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

Uživatel s oprávněními sudo máme teď na serveru hello.

### <a name="add-your-ssh-public-key-toohello-new-user-account"></a>Přidejte veřejný klíč SSH toohello nového uživatelského účtu
Z počítače, použijte hello `ssh-copy-id` s hello nové heslo.

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-tooallow-sudo-usage-without-a-password"></a>Pomocí visudo tooallow sudo využití bez zadání hesla
Pomocí `visudo` tooedit hello `/etc/sudoers` souboru přidá několik vrstev ochranu proti nesprávně změny tohoto souboru důležité.  Při provádění `visudo`, hello `/etc/sudoers` soubor je uzamčené tooensure žádný jiný uživatel provádět změny, když je aktivně upravována.  Hello `/etc/sudoers` chyb, kterých se také zkontrolována souboru `visudo` při pokus toosave nebo ukončení, nelze uložit soubor porušený sudoers.

Připravili jsme uživatelé hello správné výchozí skupiny pro přístup k příkazu "sudo".  Teď se zaměříme tooenable sudo toouse tyto skupiny bez hesla.

```bash
# Execute visudo as root tooedit hello /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# toothis
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo tooexecute any command
%sudo   ALL=(ALL:ALL) ALL

# toothis
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### <a name="verify-hello-user-ssh-keys-and-sudo"></a>Ověření uživatele hello ssh klíče a sudo
```bash
# Verify hello SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
