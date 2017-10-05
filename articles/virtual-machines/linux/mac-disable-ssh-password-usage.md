---
title: "Zakažte hesla SSH na Linux virtuálního počítače tak, že konfigurace SSHD | Microsoft Docs"
description: "Zakázáním heslo přihlášení SSH Zabezpečte virtuálním počítačům s Linuxem v Azure."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 46137640-a7d2-40e5-a1e9-9effef7eb190
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/26/2016
ms.author: v-livech
ms.openlocfilehash: dc45a1cdce29cef061acc5c7e5b15d9d89265cd9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a><span data-ttu-id="4d76a-103">Zakázat konfigurací SSHD hesla SSH na Linux virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4d76a-103">Disable SSH passwords on your Linux VM by configuring SSHD</span></span>
<span data-ttu-id="4d76a-104">Tento článek se zaměřuje na zamknutí přihlášení zabezpečení virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="4d76a-104">This article focuses on how to lock down the login security of your Linux VM.</span></span>  <span data-ttu-id="4d76a-105">Jakmile SSH port 22, je otevřené pro spuštění robotů world pokusu o přihlášení pomocí účelem uhodnutí hesla.</span><span class="sxs-lookup"><span data-stu-id="4d76a-105">As soon as the SSH port 22 is opened to the world bots start trying to login by guessing passwords.</span></span>  <span data-ttu-id="4d76a-106">Co uděláme v tomto článku je zakázat přihlášení hesla přes SSH.</span><span class="sxs-lookup"><span data-stu-id="4d76a-106">What we will do in this article is disable password logins over SSH.</span></span>  <span data-ttu-id="4d76a-107">Možnost používat hesla odebráním úplně jsme chránit virtuálního počítače s Linuxem z tohoto typu útoku hrubou silou.</span><span class="sxs-lookup"><span data-stu-id="4d76a-107">By completely removing the ability to use passwords we protect the Linux VM from this type of brute force attack.</span></span>  <span data-ttu-id="4d76a-108">Přidání bonusové je že nakonfigurujeme SSHD Linux a Povolit jenom přihlášení prostřednictvím veřejné a privátní klíče SSH, zdaleka nejbezpečnější způsob, jak přihlášení do systému Linux.</span><span class="sxs-lookup"><span data-stu-id="4d76a-108">The added bonus is we will configure Linux SSHD to only allow logins via SSH public & private keys, by far the most secure way to login to Linux.</span></span>  <span data-ttu-id="4d76a-109">Možné kombinace by vyžadovaly tak snadno uhodnout privátní klíč je enormním způsobem a proto nedoporučuje robotů ze i bothering pokusí hrubou silou klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="4d76a-109">The possible combinations of it would require to guess the private key is immense and therefore discourages bots from even bothering to try to brute force SSH keys.</span></span>

## <a name="goals"></a><span data-ttu-id="4d76a-110">Cíle</span><span class="sxs-lookup"><span data-stu-id="4d76a-110">Goals</span></span>
* <span data-ttu-id="4d76a-111">Nakonfigurujte SSHD tak, aby zakázala:</span><span class="sxs-lookup"><span data-stu-id="4d76a-111">Configure SSHD to disallow:</span></span>
  * <span data-ttu-id="4d76a-112">Heslo přihlášení</span><span class="sxs-lookup"><span data-stu-id="4d76a-112">Password logins</span></span>
  * <span data-ttu-id="4d76a-113">Přihlášení uživatele root</span><span class="sxs-lookup"><span data-stu-id="4d76a-113">Root user login</span></span>
  * <span data-ttu-id="4d76a-114">Ověřování výzvy a odezvy</span><span class="sxs-lookup"><span data-stu-id="4d76a-114">Challenge-response authentication</span></span>
* <span data-ttu-id="4d76a-115">Nakonfigurujte SSHD umožňuje:</span><span class="sxs-lookup"><span data-stu-id="4d76a-115">Configure SSHD to allow:</span></span>
  * <span data-ttu-id="4d76a-116">pouze klíče přihlášení SSH</span><span class="sxs-lookup"><span data-stu-id="4d76a-116">only SSH key logins</span></span>
* <span data-ttu-id="4d76a-117">Restartujte SSHD, avšak zůstane přihlášen</span><span class="sxs-lookup"><span data-stu-id="4d76a-117">Restart SSHD while still logged in</span></span>
* <span data-ttu-id="4d76a-118">Otestovat novou konfiguraci SSHD</span><span class="sxs-lookup"><span data-stu-id="4d76a-118">Test the new SSHD configuration</span></span>

## <a name="introduction"></a><span data-ttu-id="4d76a-119">Úvod</span><span class="sxs-lookup"><span data-stu-id="4d76a-119">Introduction</span></span>
[<span data-ttu-id="4d76a-120">SSH definované</span><span class="sxs-lookup"><span data-stu-id="4d76a-120">SSH defined</span></span>](https://en.wikipedia.org/wiki/Secure_Shell)

<span data-ttu-id="4d76a-121">SSHD je SSH Server, který běží na virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="4d76a-121">SSHD is the SSH Server that runs on the Linux VM.</span></span>  <span data-ttu-id="4d76a-122">SSH je klienta, která se spouští z prostředí na pracovní stanici MacBook nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="4d76a-122">SSH is a client that runs from a shell on your MacBook or Linux workstation.</span></span>  <span data-ttu-id="4d76a-123">SSH je také protokol použitý k zabezpečení a zašifrování komunikace mezi pracovní stanice a virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="4d76a-123">SSH is also the protocol used to secure and encrypt the communication between your workstation and the Linux VM.</span></span>

<span data-ttu-id="4d76a-124">V tomto článku je velmi důležité udržovat jeden přihlášení k virtuálním počítačům s Linuxem otevřete pro celý procházení prostřednictvím.</span><span class="sxs-lookup"><span data-stu-id="4d76a-124">For this article it is very important to keep one login to your Linux VM open for the entire walk through.</span></span>  <span data-ttu-id="4d76a-125">Z tohoto důvodu jsme se otevře dvě terminály a SSH pro virtuální počítač s Linuxem z obou z nich.</span><span class="sxs-lookup"><span data-stu-id="4d76a-125">For this reason we will open two terminals and SSH to the Linux VM from both of them.</span></span>  <span data-ttu-id="4d76a-126">První terminálu použijeme k provedení změn SSHDs konfigurační soubor a restartujte službu SSHD.</span><span class="sxs-lookup"><span data-stu-id="4d76a-126">We will use the first terminal to make the changes to SSHDs configuration file and restart the SSHD service.</span></span>  <span data-ttu-id="4d76a-127">Druhý terminálu použijeme k testování tyto změny po restartování služby.</span><span class="sxs-lookup"><span data-stu-id="4d76a-127">We will use the second terminal to test those changes once the service is restarted.</span></span>  <span data-ttu-id="4d76a-128">Protože jsme zakazování SSH hesla a program předávající výhradně na klíče SSH, pokud nejsou správné klíče SSH a zavřete připojení k virtuálnímu počítači, virtuální počítač bude trvale uzamčena a nebude moci přihlásit k němu nutnosti ho odstranit a znovu vytvoří.</span><span class="sxs-lookup"><span data-stu-id="4d76a-128">Because we are disabling SSH passwords and relying strictly on SSH keys, if your SSH keys are not correct and you close the connection to the VM, the VM will be permanently locked and no one will be able to login to it requiring it to be deleted and recreated.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4d76a-129">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4d76a-129">Prerequisites</span></span>
* [<span data-ttu-id="4d76a-130">Vytvoření klíčů SSH na Linuxu a Macu pro virtuální počítače s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="4d76a-130">Create SSH keys on Linux and Mac for Linux VMs in Azure</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* <span data-ttu-id="4d76a-131">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="4d76a-131">Azure account</span></span>
  * [<span data-ttu-id="4d76a-132">registrace bezplatné zkušební verze</span><span class="sxs-lookup"><span data-stu-id="4d76a-132">free trial signup</span></span>](https://azure.microsoft.com/pricing/free-trial/)
  * [<span data-ttu-id="4d76a-133">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4d76a-133">Azure portal</span></span>](http://portal.azure.com)
* <span data-ttu-id="4d76a-134">Virtuální počítač s Linuxem spuštěné v azure</span><span class="sxs-lookup"><span data-stu-id="4d76a-134">Linux VM running on azure</span></span>
* <span data-ttu-id="4d76a-135">SSH veřejné a privátní klíče RSA ve`~/.ssh/`</span><span class="sxs-lookup"><span data-stu-id="4d76a-135">SSH public & private key pair in `~/.ssh/`</span></span>
* <span data-ttu-id="4d76a-136">Veřejný klíč SSH v `~/.ssh/authorized_keys` na virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="4d76a-136">SSH public key in `~/.ssh/authorized_keys` on the Linux VM</span></span>
* <span data-ttu-id="4d76a-137">Sudo práva na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="4d76a-137">Sudo rights on the VM</span></span>
* <span data-ttu-id="4d76a-138">Port 22, otevřete</span><span class="sxs-lookup"><span data-stu-id="4d76a-138">Port 22 open</span></span>

## <a name="quick-commands"></a><span data-ttu-id="4d76a-139">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="4d76a-139">Quick Commands</span></span>
<span data-ttu-id="4d76a-140">*Po ostřílené Linux správci, kteří právě má verzi TLDR Začněte zde.  Pro všechny ostatní, který chce tuto část přeskočit podrobné vysvětlení a ukázce.*</span><span class="sxs-lookup"><span data-stu-id="4d76a-140">*Seasoned Linux Admins who just want the TLDR version start here.  For everyone else that wants the detailed explanation and walk through skip this section.*</span></span>

```bash
sudo vim /etc/ssh/sshd_config
```

<span data-ttu-id="4d76a-141">Upravte konfigurační soubor následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4d76a-141">Edit the config file as follows:</span></span>

```sh
# Change PasswordAuthentication to this:
PasswordAuthentication no

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes

# Change PermitRootLogin to this:
PermitRootLogin no

# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

<span data-ttu-id="4d76a-142">Restartujte službu SSHD.</span><span class="sxs-lookup"><span data-stu-id="4d76a-142">Restart the SSHD service.</span></span> <span data-ttu-id="4d76a-143">Na základě Debian distribucích:</span><span class="sxs-lookup"><span data-stu-id="4d76a-143">On Debian-based distros:</span></span>

```bash
sudo service ssh restart
```

<span data-ttu-id="4d76a-144">Na základě Red Hat distribucích:</span><span class="sxs-lookup"><span data-stu-id="4d76a-144">On Red Hat-based distros:</span></span>

```bash
sudo service sshd restart
```

## <a name="detailed-walk-through"></a><span data-ttu-id="4d76a-145">Podrobné procházení prostřednictvím</span><span class="sxs-lookup"><span data-stu-id="4d76a-145">Detailed Walk Through</span></span>
<span data-ttu-id="4d76a-146">Přihlášení k virtuálního počítače s Linuxem v terminálu 1 (T1).</span><span class="sxs-lookup"><span data-stu-id="4d76a-146">Login to the Linux VM on terminal 1 (T1).</span></span>  <span data-ttu-id="4d76a-147">Přihlášení k virtuálního počítače s Linuxem v terminálu 2 (T2).</span><span class="sxs-lookup"><span data-stu-id="4d76a-147">Login to the Linux VM on terminal 2 (T2).</span></span>

<span data-ttu-id="4d76a-148">V čase T2 přidáme upravovat soubor konfigurace SSHD.</span><span class="sxs-lookup"><span data-stu-id="4d76a-148">On T2 we are going to edit the SSHD configuration file.</span></span>  

```bash
sudo vim /etc/ssh/sshd_config
```

<span data-ttu-id="4d76a-149">Odsud upravíme nastavení hesla zakázání a povolení přihlášení klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="4d76a-149">From here we will edit just the settings to disable passwords and enable SSH key logins.</span></span>  <span data-ttu-id="4d76a-150">Existuje mnoho nastavení v tento soubor, který by měl prozkoumat a změňte zabezpečit Linux & SSH jako podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="4d76a-150">There are many settings in this file that you should research and change to make Linux & SSH as secure as you need.</span></span>

#### <a name="disable-password-logins"></a><span data-ttu-id="4d76a-151">Zakázat přihlášení heslo</span><span class="sxs-lookup"><span data-stu-id="4d76a-151">Disable Password logins</span></span>

```sh
# Change PasswordAuthentication to this:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a><span data-ttu-id="4d76a-152">Povolit ověření veřejného klíče</span><span class="sxs-lookup"><span data-stu-id="4d76a-152">Enable Public Key Authentication</span></span>

```sh
# Change PubkeyAuthentication to this:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a><span data-ttu-id="4d76a-153">Zakázat kořenové přihlášení</span><span class="sxs-lookup"><span data-stu-id="4d76a-153">Disable Root Login</span></span>

```sh
# Change PermitRootLogin to this:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a><span data-ttu-id="4d76a-154">Zakázat ověřování výzvy a odezvy</span><span class="sxs-lookup"><span data-stu-id="4d76a-154">Disable Challenge-response Authentication</span></span>
```sh
# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a><span data-ttu-id="4d76a-155">Restartujte SSHD</span><span class="sxs-lookup"><span data-stu-id="4d76a-155">Restart SSHD</span></span>
<span data-ttu-id="4d76a-156">Z prostředí T1 ověřte, že jste stále přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4d76a-156">From the T1 shell verify that you are still logged in.</span></span>  <span data-ttu-id="4d76a-157">To je důležité, takže můžete není zamknout z virtuálního počítače Pokud klíče SSH nejsou správné, vzhledem k tomu, že hesla jsou nyní zakázány.</span><span class="sxs-lookup"><span data-stu-id="4d76a-157">This is critical so you do not get locked out of your VM if your SSH keys are not correct since passwords are now disabled.</span></span>  <span data-ttu-id="4d76a-158">Pokud všechna nastavení jsou nesprávná na virtuálním počítačům s Linuxem T1 můžete opravit sshd_config budete přihlášeni stále a SSH ponechá připojení aktivní během SSHD službu restartujte.</span><span class="sxs-lookup"><span data-stu-id="4d76a-158">If any setting are incorrect on your Linux VM you can use T1 to fix sshd_config as you will still be logged in and SSH will keep the connection alive during the SSHD service restart.</span></span>

<span data-ttu-id="4d76a-159">Z T2 spustit:</span><span class="sxs-lookup"><span data-stu-id="4d76a-159">From T2 run:</span></span>

##### <a name="on-the-debian-family"></a><span data-ttu-id="4d76a-160">V Debian řady</span><span class="sxs-lookup"><span data-stu-id="4d76a-160">On the Debian Family</span></span>
```bash
sudo service ssh restart
```

##### <a name="on-the-redhat-family"></a><span data-ttu-id="4d76a-161">V dané rodině RedHat</span><span class="sxs-lookup"><span data-stu-id="4d76a-161">On the RedHat Family</span></span>
```bash
sudo service sshd restart
```

<span data-ttu-id="4d76a-162">Hesla jsou nyní zakázány na vašem virtuálním počítači, ochrana před hrubou silou pokusů o přihlášení heslo.</span><span class="sxs-lookup"><span data-stu-id="4d76a-162">Passwords are now disabled on your VM protecting it from brute force password login attempts.</span></span>  <span data-ttu-id="4d76a-163">Pouze SSH klíče povolená, že nebudete moci přihlásit rychlejší a mnohem bezpečnější.</span><span class="sxs-lookup"><span data-stu-id="4d76a-163">With only SSH Keys allowed you will be able to login faster and much more secure.</span></span>

