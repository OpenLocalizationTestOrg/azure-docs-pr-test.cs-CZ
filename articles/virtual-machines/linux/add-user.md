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
# <a name="add-a-user-to-an-azure-vm"></a><span data-ttu-id="6328f-103">Přidání uživatele do virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="6328f-103">Add a user to an Azure VM</span></span>
<span data-ttu-id="6328f-104">Jedním z první úlohy na žádné nové virtuální počítače Linux je vytvoření nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="6328f-104">One of the first tasks on any new Linux VM is to create a new user.</span></span>  <span data-ttu-id="6328f-105">V tomto článku jsme provede procesem vytvoření sudo uživatelský účet, nastavení hesla, přidání SSH veřejných klíčů a nakonec použít `visudo` umožňující sudo bez zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="6328f-105">In this article, we walk through creating a sudo user account, setting the password, adding SSH Public Keys, and finally use `visudo` to allow sudo without a password.</span></span>

<span data-ttu-id="6328f-106">Požadavky jsou: [účet Azure](https://azure.microsoft.com/pricing/free-trial/), [veřejné a soukromé klíče SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), skupinu prostředků Azure a rozhraní příkazového řádku Azure nainstalovaný a přepnout do režimu Azure Resource Manager pomocí `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="6328f-106">Prerequisites are: [an Azure account](https://azure.microsoft.com/pricing/free-trial/), [SSH public and private keys](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), an Azure resource group, and the Azure CLI installed and switched to Azure Resource Manager mode using `azure config mode arm`.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="6328f-107">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="6328f-107">Quick Commands</span></span>
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

## <a name="detailed-walkthrough"></a><span data-ttu-id="6328f-108">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="6328f-108">Detailed Walkthrough</span></span>
### <a name="introduction"></a><span data-ttu-id="6328f-109">Úvod</span><span class="sxs-lookup"><span data-stu-id="6328f-109">Introduction</span></span>
<span data-ttu-id="6328f-110">Jedním z první a nejběžnější úlohy s nový server je k přidání uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="6328f-110">One of the first and most common task with a new server is to add a user account.</span></span>  <span data-ttu-id="6328f-111">By mělo být zakázáno přihlášení kořenové a kořenového účtu samotné nepoužívejte server Linux, pouze sudo.</span><span class="sxs-lookup"><span data-stu-id="6328f-111">Root logins should be disabled and the root account itself should not be used with your Linux server, only sudo.</span></span>  <span data-ttu-id="6328f-112">Poskytnutí zvýšení oprávnění pomocí příkazu sudo kořenové uživatele je správný způsob, jak spravovat a použít Linux.</span><span class="sxs-lookup"><span data-stu-id="6328f-112">Giving a user root escalation privileges using sudo it the proper way to administer and use Linux.</span></span>

<span data-ttu-id="6328f-113">Pomocí příkazu `useradd` přidáváme uživatelské účty do virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="6328f-113">Using the command `useradd` we are adding user accounts to the Linux VM.</span></span>  <span data-ttu-id="6328f-114">Spuštění `useradd` upraví `/etc/passwd`, `/etc/shadow`, `/etc/group`, a `/etc/gshadow`.</span><span class="sxs-lookup"><span data-stu-id="6328f-114">Running `useradd` modifies `/etc/passwd`, `/etc/shadow`, `/etc/group`, and `/etc/gshadow`.</span></span>  <span data-ttu-id="6328f-115">Přidáváme příkazového řádku příznak, který `useradd` příkazu také přidat nového uživatele do skupiny správné sudo v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="6328f-115">We are adding a command-line flag to the `useradd` command to also add the new user to the proper sudo group on Linux.</span></span>  <span data-ttu-id="6328f-116">I Zvu tě `useradd` vytvoří položku do `/etc/passwd` není přidělit nový uživatelský účet heslo.</span><span class="sxs-lookup"><span data-stu-id="6328f-116">Even thou `useradd` creates an entry into `/etc/passwd` it does not give the new user account a password.</span></span>  <span data-ttu-id="6328f-117">Vytváříme počátečního hesla pro nového uživatele pomocí jednoduché `passwd` příkaz.</span><span class="sxs-lookup"><span data-stu-id="6328f-117">We are creating an initial password for the new user using the simple `passwd` command.</span></span>  <span data-ttu-id="6328f-118">Posledním krokem je upravit pravidla sudo umožňující tohoto uživatele k provádění příkazů s oprávněními sudo bez nutnosti zadávat heslo pro každý příkaz.</span><span class="sxs-lookup"><span data-stu-id="6328f-118">The last step is to modify the sudo rules to allow that user to execute commands with sudo privileges without having to enter a password for every command.</span></span>  <span data-ttu-id="6328f-119">Přihlášení pomocí soukromého klíče jsme se za předpokladu, že uživatelský účet je před nesprávnými účastníky a se má povolit přístup sudo bez zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="6328f-119">Logging in using the Private key we are assuming that user account is safe from bad actors and are going to allow sudo access without a password.</span></span>  

### <a name="adding-a-single-sudo-user-to-an-azure-vm"></a><span data-ttu-id="6328f-120">Přidání uživatele jeden sudo na virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="6328f-120">Adding a single sudo user to an Azure VM</span></span>
<span data-ttu-id="6328f-121">Přihlaste se k virtuálnímu počítači Azure pomocí klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="6328f-121">Log in to the Azure VM using SSH keys.</span></span>  <span data-ttu-id="6328f-122">Pokud máte instalaci SSH veřejného klíče získat přístup, proveďte v tomto článku nejprve [pomocí ověření veřejného klíče s Azure](http://link.to/article).</span><span class="sxs-lookup"><span data-stu-id="6328f-122">If you have not setup SSH public key access, complete this article first [Using Public Key Authentication with Azure](http://link.to/article).</span></span>  

<span data-ttu-id="6328f-123">`useradd` Příkaz dokončí následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="6328f-123">The `useradd` command completes the following tasks:</span></span>

* <span data-ttu-id="6328f-124">Vytvořit nový uživatelský účet</span><span class="sxs-lookup"><span data-stu-id="6328f-124">create a new user account</span></span>
* <span data-ttu-id="6328f-125">Vytvořte novou skupinu uživatelů se stejným názvem</span><span class="sxs-lookup"><span data-stu-id="6328f-125">create a new user group with the same name</span></span>
* <span data-ttu-id="6328f-126">prázdná položka k přidání`/etc/passwd`</span><span class="sxs-lookup"><span data-stu-id="6328f-126">add a blank entry to `/etc/passwd`</span></span>
* <span data-ttu-id="6328f-127">prázdná položka k přidání`/etc/gpasswd`</span><span class="sxs-lookup"><span data-stu-id="6328f-127">add a blank entry to `/etc/gpasswd`</span></span>

<span data-ttu-id="6328f-128">`-G` Příkazového řádku příznak přidá nový uživatelský účet do správné skupiny Linux udělíte oprávnění eskalace kořenového nový uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="6328f-128">The `-G` command-line flag adds the new user account to the proper Linux group giving the new user account root escalation privileges.</span></span>

#### <a name="add-the-user"></a><span data-ttu-id="6328f-129">Přidejte uživatele</span><span class="sxs-lookup"><span data-stu-id="6328f-129">Add the user</span></span>
```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a><span data-ttu-id="6328f-130">Nastavení hesla</span><span class="sxs-lookup"><span data-stu-id="6328f-130">Set a password</span></span>
<span data-ttu-id="6328f-131">`useradd` Příkaz vytvoří uživatele a přidá položku do obou `/etc/passwd` a `/etc/gpasswd` , ale ve skutečnosti nenastaví heslo.</span><span class="sxs-lookup"><span data-stu-id="6328f-131">The `useradd` command creates the user and adds an entry to both `/etc/passwd` and `/etc/gpasswd` but does not actually set the password.</span></span>  <span data-ttu-id="6328f-132">Heslo je přidaný do položky pomocí `passwd` příkaz.</span><span class="sxs-lookup"><span data-stu-id="6328f-132">The password is added to the entry using the `passwd` command.</span></span>

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

<span data-ttu-id="6328f-133">Uživatel s oprávněními sudo máme teď na serveru.</span><span class="sxs-lookup"><span data-stu-id="6328f-133">We now have a user with sudo privileges on the server.</span></span>

### <a name="add-your-ssh-public-key-to-the-new-user-account"></a><span data-ttu-id="6328f-134">Přidat váš veřejný klíč SSH pro nový uživatelský účet</span><span class="sxs-lookup"><span data-stu-id="6328f-134">Add your SSH Public Key to the new user account</span></span>
<span data-ttu-id="6328f-135">Z počítače, použijte `ssh-copy-id` příkaz pomocí nového hesla.</span><span class="sxs-lookup"><span data-stu-id="6328f-135">From your machine, use the `ssh-copy-id` command with the new password.</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-to-allow-sudo-usage-without-a-password"></a><span data-ttu-id="6328f-136">Pomocí visudo umožňující použití sudo bez zadání hesla</span><span class="sxs-lookup"><span data-stu-id="6328f-136">Using visudo to allow sudo usage without a password</span></span>
<span data-ttu-id="6328f-137">Pomocí `visudo` upravit `/etc/sudoers` souboru přidá několik vrstev ochranu proti nesprávně změny tohoto souboru důležité.</span><span class="sxs-lookup"><span data-stu-id="6328f-137">Using `visudo` to edit the `/etc/sudoers` file adds a few layers of protection against incorrectly modifying this important file.</span></span>  <span data-ttu-id="6328f-138">Při provádění `visudo`, `/etc/sudoers` soubor je uzamčený, aby žádný jiný uživatel provádět změny, když je aktivně upravována.</span><span class="sxs-lookup"><span data-stu-id="6328f-138">Upon executing `visudo`, the `/etc/sudoers` file is locked to ensure no other user can make changes while it is actively being edited.</span></span>  <span data-ttu-id="6328f-139">`/etc/sudoers` Chyb, kterých se také zkontrolována souboru `visudo` při pokusu uložit nebo ukončení, nelze uložit soubor porušený sudoers.</span><span class="sxs-lookup"><span data-stu-id="6328f-139">The `/etc/sudoers` file is also checked for mistakes by `visudo` when you attempt to save or exit so you cannot save a broken sudoers file.</span></span>

<span data-ttu-id="6328f-140">Připravili jsme uživatelé ve skupině správné výchozí pro přístup k příkazu "sudo".</span><span class="sxs-lookup"><span data-stu-id="6328f-140">We already have users in the correct default group for sudo access.</span></span>  <span data-ttu-id="6328f-141">Teď se zaměříme chcete povolit tyto skupiny používat sudo bez hesla.</span><span class="sxs-lookup"><span data-stu-id="6328f-141">Now we are going to enable those groups to use sudo with no password.</span></span>

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

### <a name="verify-the-user-ssh-keys-and-sudo"></a><span data-ttu-id="6328f-142">Ověření uživatele, ssh klíče a sudo</span><span class="sxs-lookup"><span data-stu-id="6328f-142">Verify the user, ssh keys, and sudo</span></span>
```bash
# Verify the SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
