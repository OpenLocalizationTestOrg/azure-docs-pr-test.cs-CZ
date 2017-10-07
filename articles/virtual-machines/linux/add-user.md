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
# <a name="add-a-user-tooan-azure-vm"></a><span data-ttu-id="4c48e-103">Přidat uživatele tooan virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="4c48e-103">Add a user tooan Azure VM</span></span>
<span data-ttu-id="4c48e-104">Jedním z první úlohy hello na žádné nové virtuální počítače Linux je toocreate nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="4c48e-104">One of hello first tasks on any new Linux VM is toocreate a new user.</span></span>  <span data-ttu-id="4c48e-105">V tomto článku jsme provede procesem vytvoření sudo uživatelský účet, heslo hello nastavení, přidání SSH veřejných klíčů a nakonec použít `visudo` tooallow sudo bez zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="4c48e-105">In this article, we walk through creating a sudo user account, setting hello password, adding SSH Public Keys, and finally use `visudo` tooallow sudo without a password.</span></span>

<span data-ttu-id="4c48e-106">Požadavky jsou: [účet Azure](https://azure.microsoft.com/pricing/free-trial/), [veřejné a soukromé klíče SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), skupinu prostředků Azure a hello rozhraní příkazového řádku Azure nainstalované a přepnout pomocí režimu Resource Manager tooAzure `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="4c48e-106">Prerequisites are: [an Azure account](https://azure.microsoft.com/pricing/free-trial/), [SSH public and private keys](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), an Azure resource group, and hello Azure CLI installed and switched tooAzure Resource Manager mode using `azure config mode arm`.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="4c48e-107">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="4c48e-107">Quick Commands</span></span>
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

## <a name="detailed-walkthrough"></a><span data-ttu-id="4c48e-108">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="4c48e-108">Detailed Walkthrough</span></span>
### <a name="introduction"></a><span data-ttu-id="4c48e-109">Úvod</span><span class="sxs-lookup"><span data-stu-id="4c48e-109">Introduction</span></span>
<span data-ttu-id="4c48e-110">Jedním z hello první a nejběžnější úlohy na nový server je tooadd uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="4c48e-110">One of hello first and most common task with a new server is tooadd a user account.</span></span>  <span data-ttu-id="4c48e-111">By mělo být zakázáno přihlášení kořenové a nepoužívejte hello kořenového účtu samotné s server Linux, pouze sudo.</span><span class="sxs-lookup"><span data-stu-id="4c48e-111">Root logins should be disabled and hello root account itself should not be used with your Linux server, only sudo.</span></span>  <span data-ttu-id="4c48e-112">Udělení oprávnění pomocí příkazu "sudo" jej hello správný způsob tooadminister a používat Linux eskalace kořenové uživatele.</span><span class="sxs-lookup"><span data-stu-id="4c48e-112">Giving a user root escalation privileges using sudo it hello proper way tooadminister and use Linux.</span></span>

<span data-ttu-id="4c48e-113">Pomocí příkazu hello `useradd` přidáváme uživatelské účty toohello virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="4c48e-113">Using hello command `useradd` we are adding user accounts toohello Linux VM.</span></span>  <span data-ttu-id="4c48e-114">Spuštění `useradd` upraví `/etc/passwd`, `/etc/shadow`, `/etc/group`, a `/etc/gshadow`.</span><span class="sxs-lookup"><span data-stu-id="4c48e-114">Running `useradd` modifies `/etc/passwd`, `/etc/shadow`, `/etc/group`, and `/etc/gshadow`.</span></span>  <span data-ttu-id="4c48e-115">Přidáváme příkazového řádku příznak toohello `useradd` příkaz tooalso přidat hello novou toohello správné sudo skupinu uživatelů v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="4c48e-115">We are adding a command-line flag toohello `useradd` command tooalso add hello new user toohello proper sudo group on Linux.</span></span>  <span data-ttu-id="4c48e-116">I Zvu tě `useradd` vytvoří položku do `/etc/passwd` neudělí hello nový uživatelský účet heslo.</span><span class="sxs-lookup"><span data-stu-id="4c48e-116">Even thou `useradd` creates an entry into `/etc/passwd` it does not give hello new user account a password.</span></span>  <span data-ttu-id="4c48e-117">Vytváříme počátečního hesla pro nového uživatele hello pomocí jednoduchého hello `passwd` příkaz.</span><span class="sxs-lookup"><span data-stu-id="4c48e-117">We are creating an initial password for hello new user using hello simple `passwd` command.</span></span>  <span data-ttu-id="4c48e-118">posledním krokem Hello je toomodify hello sudo pravidla tooallow příkazy tooexecute tohoto uživatele s oprávněními sudo bez nutnosti tooenter heslo pro každý příkaz.</span><span class="sxs-lookup"><span data-stu-id="4c48e-118">hello last step is toomodify hello sudo rules tooallow that user tooexecute commands with sudo privileges without having tooenter a password for every command.</span></span>  <span data-ttu-id="4c48e-119">Přihlášení pomocí soukromého klíče hello jsme se za předpokladu, že uživatelský účet je před nesprávnými účastníky a budou tooallow sudo přístup bez zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="4c48e-119">Logging in using hello Private key we are assuming that user account is safe from bad actors and are going tooallow sudo access without a password.</span></span>  

### <a name="adding-a-single-sudo-user-tooan-azure-vm"></a><span data-ttu-id="4c48e-120">Přidávání jednoho sudo uživatele tooan virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="4c48e-120">Adding a single sudo user tooan Azure VM</span></span>
<span data-ttu-id="4c48e-121">Přihlaste se toohello virtuálního počítače Azure pomocí klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="4c48e-121">Log in toohello Azure VM using SSH keys.</span></span>  <span data-ttu-id="4c48e-122">Pokud máte instalaci SSH veřejného klíče získat přístup, proveďte v tomto článku nejprve [pomocí ověření veřejného klíče s Azure](http://link.to/article).</span><span class="sxs-lookup"><span data-stu-id="4c48e-122">If you have not setup SSH public key access, complete this article first [Using Public Key Authentication with Azure](http://link.to/article).</span></span>  

<span data-ttu-id="4c48e-123">Hello `useradd` dokončení příkazu hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="4c48e-123">hello `useradd` command completes hello following tasks:</span></span>

* <span data-ttu-id="4c48e-124">Vytvořit nový uživatelský účet</span><span class="sxs-lookup"><span data-stu-id="4c48e-124">create a new user account</span></span>
* <span data-ttu-id="4c48e-125">Vytvořit novou skupinu uživatelů s hello stejným názvem</span><span class="sxs-lookup"><span data-stu-id="4c48e-125">create a new user group with hello same name</span></span>
* <span data-ttu-id="4c48e-126">Přidat prázdná položka příliš`/etc/passwd`</span><span class="sxs-lookup"><span data-stu-id="4c48e-126">add a blank entry too`/etc/passwd`</span></span>
* <span data-ttu-id="4c48e-127">Přidat prázdná položka příliš`/etc/gpasswd`</span><span class="sxs-lookup"><span data-stu-id="4c48e-127">add a blank entry too`/etc/gpasswd`</span></span>

<span data-ttu-id="4c48e-128">Hello `-G` příkazového řádku příznak přidá hello novou účet toohello správné Linux skupinu uživatelů udělíte oprávnění eskalace kořenového hello nový uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="4c48e-128">hello `-G` command-line flag adds hello new user account toohello proper Linux group giving hello new user account root escalation privileges.</span></span>

#### <a name="add-hello-user"></a><span data-ttu-id="4c48e-129">Přidat uživatele hello</span><span class="sxs-lookup"><span data-stu-id="4c48e-129">Add hello user</span></span>
```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a><span data-ttu-id="4c48e-130">Nastavení hesla</span><span class="sxs-lookup"><span data-stu-id="4c48e-130">Set a password</span></span>
<span data-ttu-id="4c48e-131">Hello `useradd` příkaz vytvoří hello uživatele a přidá položka tooboth `/etc/passwd` a `/etc/gpasswd` , ale ve skutečnosti nenastaví hello heslo.</span><span class="sxs-lookup"><span data-stu-id="4c48e-131">hello `useradd` command creates hello user and adds an entry tooboth `/etc/passwd` and `/etc/gpasswd` but does not actually set hello password.</span></span>  <span data-ttu-id="4c48e-132">Hello heslo je přidána toohello položky na základě hello `passwd` příkaz.</span><span class="sxs-lookup"><span data-stu-id="4c48e-132">hello password is added toohello entry using hello `passwd` command.</span></span>

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

<span data-ttu-id="4c48e-133">Uživatel s oprávněními sudo máme teď na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="4c48e-133">We now have a user with sudo privileges on hello server.</span></span>

### <a name="add-your-ssh-public-key-toohello-new-user-account"></a><span data-ttu-id="4c48e-134">Přidejte veřejný klíč SSH toohello nového uživatelského účtu</span><span class="sxs-lookup"><span data-stu-id="4c48e-134">Add your SSH Public Key toohello new user account</span></span>
<span data-ttu-id="4c48e-135">Z počítače, použijte hello `ssh-copy-id` s hello nové heslo.</span><span class="sxs-lookup"><span data-stu-id="4c48e-135">From your machine, use hello `ssh-copy-id` command with hello new password.</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-tooallow-sudo-usage-without-a-password"></a><span data-ttu-id="4c48e-136">Pomocí visudo tooallow sudo využití bez zadání hesla</span><span class="sxs-lookup"><span data-stu-id="4c48e-136">Using visudo tooallow sudo usage without a password</span></span>
<span data-ttu-id="4c48e-137">Pomocí `visudo` tooedit hello `/etc/sudoers` souboru přidá několik vrstev ochranu proti nesprávně změny tohoto souboru důležité.</span><span class="sxs-lookup"><span data-stu-id="4c48e-137">Using `visudo` tooedit hello `/etc/sudoers` file adds a few layers of protection against incorrectly modifying this important file.</span></span>  <span data-ttu-id="4c48e-138">Při provádění `visudo`, hello `/etc/sudoers` soubor je uzamčené tooensure žádný jiný uživatel provádět změny, když je aktivně upravována.</span><span class="sxs-lookup"><span data-stu-id="4c48e-138">Upon executing `visudo`, hello `/etc/sudoers` file is locked tooensure no other user can make changes while it is actively being edited.</span></span>  <span data-ttu-id="4c48e-139">Hello `/etc/sudoers` chyb, kterých se také zkontrolována souboru `visudo` při pokus toosave nebo ukončení, nelze uložit soubor porušený sudoers.</span><span class="sxs-lookup"><span data-stu-id="4c48e-139">hello `/etc/sudoers` file is also checked for mistakes by `visudo` when you attempt toosave or exit so you cannot save a broken sudoers file.</span></span>

<span data-ttu-id="4c48e-140">Připravili jsme uživatelé hello správné výchozí skupiny pro přístup k příkazu "sudo".</span><span class="sxs-lookup"><span data-stu-id="4c48e-140">We already have users in hello correct default group for sudo access.</span></span>  <span data-ttu-id="4c48e-141">Teď se zaměříme tooenable sudo toouse tyto skupiny bez hesla.</span><span class="sxs-lookup"><span data-stu-id="4c48e-141">Now we are going tooenable those groups toouse sudo with no password.</span></span>

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

### <a name="verify-hello-user-ssh-keys-and-sudo"></a><span data-ttu-id="4c48e-142">Ověření uživatele hello ssh klíče a sudo</span><span class="sxs-lookup"><span data-stu-id="4c48e-142">Verify hello user, ssh keys, and sudo</span></span>
```bash
# Verify hello SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
