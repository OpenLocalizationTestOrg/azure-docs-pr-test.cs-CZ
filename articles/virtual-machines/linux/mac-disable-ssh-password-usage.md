---
title: "hesla aaaDisable SSH na Linux virtuálního počítače tak, že nakonfigurujete SSHD | Microsoft Docs"
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
ms.openlocfilehash: fb67b2f5b8b3bf2ba214858940b04f2ea9013fb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a><span data-ttu-id="0b3e8-103">Zakázat konfigurací SSHD hesla SSH na Linux virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0b3e8-103">Disable SSH passwords on your Linux VM by configuring SSHD</span></span>
<span data-ttu-id="0b3e8-104">Tento článek se zaměřuje na toolock dolů hello přihlášení zabezpečení virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-104">This article focuses on how toolock down hello login security of your Linux VM.</span></span>  <span data-ttu-id="0b3e8-105">Při otevření hello portu SSH 22 toohello world robotů spustit při toologin podle účelem uhodnutí hesla.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-105">As soon as hello SSH port 22 is opened toohello world bots start trying toologin by guessing passwords.</span></span>  <span data-ttu-id="0b3e8-106">Co uděláme v tomto článku je zakázat přihlášení hesla přes SSH.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-106">What we will do in this article is disable password logins over SSH.</span></span>  <span data-ttu-id="0b3e8-107">Úplně odebráním hello možnost vynutit toouse hesla chráníme hello virtuálního počítače s Linuxem z tohoto typu hrubou útoku.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-107">By completely removing hello ability toouse passwords we protect hello Linux VM from this type of brute force attack.</span></span>  <span data-ttu-id="0b3e8-108">Hello přidat bonusové je nakonfigurujeme Linux SSHD tooonly povolit přihlášení prostřednictvím veřejné a privátní klíče SSH, zdaleka hello nejbezpečnější způsob toologin tooLinux.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-108">hello added bonus is we will configure Linux SSHD tooonly allow logins via SSH public & private keys, by far hello most secure way toologin tooLinux.</span></span>  <span data-ttu-id="0b3e8-109">Hello možných kombinací je by vyžadovaly tooguess hello privátní klíč je enormním způsobem a proto nedoporučuje robotů ze i bothering tootry toobrute force SSH klíče.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-109">hello possible combinations of it would require tooguess hello private key is immense and therefore discourages bots from even bothering tootry toobrute force SSH keys.</span></span>

## <a name="goals"></a><span data-ttu-id="0b3e8-110">Cíle</span><span class="sxs-lookup"><span data-stu-id="0b3e8-110">Goals</span></span>
* <span data-ttu-id="0b3e8-111">Konfigurace SSHD toodisallow:</span><span class="sxs-lookup"><span data-stu-id="0b3e8-111">Configure SSHD toodisallow:</span></span>
  * <span data-ttu-id="0b3e8-112">Heslo přihlášení</span><span class="sxs-lookup"><span data-stu-id="0b3e8-112">Password logins</span></span>
  * <span data-ttu-id="0b3e8-113">Přihlášení uživatele root</span><span class="sxs-lookup"><span data-stu-id="0b3e8-113">Root user login</span></span>
  * <span data-ttu-id="0b3e8-114">Ověřování výzvy a odezvy</span><span class="sxs-lookup"><span data-stu-id="0b3e8-114">Challenge-response authentication</span></span>
* <span data-ttu-id="0b3e8-115">Konfigurace SSHD tooallow:</span><span class="sxs-lookup"><span data-stu-id="0b3e8-115">Configure SSHD tooallow:</span></span>
  * <span data-ttu-id="0b3e8-116">pouze klíče přihlášení SSH</span><span class="sxs-lookup"><span data-stu-id="0b3e8-116">only SSH key logins</span></span>
* <span data-ttu-id="0b3e8-117">Restartujte SSHD, avšak zůstane přihlášen</span><span class="sxs-lookup"><span data-stu-id="0b3e8-117">Restart SSHD while still logged in</span></span>
* <span data-ttu-id="0b3e8-118">Konfigurace nového SSHD testovací hello</span><span class="sxs-lookup"><span data-stu-id="0b3e8-118">Test hello new SSHD configuration</span></span>

## <a name="introduction"></a><span data-ttu-id="0b3e8-119">Úvod</span><span class="sxs-lookup"><span data-stu-id="0b3e8-119">Introduction</span></span>
[<span data-ttu-id="0b3e8-120">SSH definované</span><span class="sxs-lookup"><span data-stu-id="0b3e8-120">SSH defined</span></span>](https://en.wikipedia.org/wiki/Secure_Shell)

<span data-ttu-id="0b3e8-121">SSHD je hello SSH serveru, který běží na hello virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-121">SSHD is hello SSH Server that runs on hello Linux VM.</span></span>  <span data-ttu-id="0b3e8-122">SSH je klienta, která se spouští z prostředí na pracovní stanici MacBook nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-122">SSH is a client that runs from a shell on your MacBook or Linux workstation.</span></span>  <span data-ttu-id="0b3e8-123">SSH je také protokol hello používá toosecure a zašifrování komunikace hello mezi hello virtuálního počítače s Linuxem a pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-123">SSH is also hello protocol used toosecure and encrypt hello communication between your workstation and hello Linux VM.</span></span>

<span data-ttu-id="0b3e8-124">Tento článek je velmi důležité tookeep jeden virtuální počítač s Linuxem otevřené pro celý hello provede tooyour přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-124">For this article it is very important tookeep one login tooyour Linux VM open for hello entire walk through.</span></span>  <span data-ttu-id="0b3e8-125">Z tohoto důvodu jsme se otevře dvěma terminály a toohello SSH virtuálního počítače s Linuxem z obou z nich.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-125">For this reason we will open two terminals and SSH toohello Linux VM from both of them.</span></span>  <span data-ttu-id="0b3e8-126">Jsme použije hello první terminálu toomake hello změny tooSSHDs konfigurační soubor a restartujte službu SSHD hello.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-126">We will use hello first terminal toomake hello changes tooSSHDs configuration file and restart hello SSHD service.</span></span>  <span data-ttu-id="0b3e8-127">Použijeme hello druhý terminálu tootest těch, které změní po restartování služby hello.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-127">We will use hello second terminal tootest those changes once hello service is restarted.</span></span>  <span data-ttu-id="0b3e8-128">Protože jsme zakazování SSH hesla a program předávající výhradně na klíče SSH, pokud nejsou správné klíče SSH a zavřete toohello hello připojení virtuálních počítačů, hello virtuálních počítačů bude trvale uzamčena a nebude možné toologin tooit ho budete vyžadovat toobe odstraněno a znovu vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-128">Because we are disabling SSH passwords and relying strictly on SSH keys, if your SSH keys are not correct and you close hello connection toohello VM, hello VM will be permanently locked and no one will be able toologin tooit requiring it toobe deleted and recreated.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b3e8-129">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0b3e8-129">Prerequisites</span></span>
* [<span data-ttu-id="0b3e8-130">Vytvoření klíčů SSH na Linuxu a Macu pro virtuální počítače s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="0b3e8-130">Create SSH keys on Linux and Mac for Linux VMs in Azure</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* <span data-ttu-id="0b3e8-131">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="0b3e8-131">Azure account</span></span>
  * [<span data-ttu-id="0b3e8-132">registrace bezplatné zkušební verze</span><span class="sxs-lookup"><span data-stu-id="0b3e8-132">free trial signup</span></span>](https://azure.microsoft.com/pricing/free-trial/)
  * [<span data-ttu-id="0b3e8-133">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0b3e8-133">Azure portal</span></span>](http://portal.azure.com)
* <span data-ttu-id="0b3e8-134">Virtuální počítač s Linuxem spuštěné v azure</span><span class="sxs-lookup"><span data-stu-id="0b3e8-134">Linux VM running on azure</span></span>
* <span data-ttu-id="0b3e8-135">SSH veřejné a privátní klíče RSA ve`~/.ssh/`</span><span class="sxs-lookup"><span data-stu-id="0b3e8-135">SSH public & private key pair in `~/.ssh/`</span></span>
* <span data-ttu-id="0b3e8-136">Veřejný klíč SSH v `~/.ssh/authorized_keys` na hello virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="0b3e8-136">SSH public key in `~/.ssh/authorized_keys` on hello Linux VM</span></span>
* <span data-ttu-id="0b3e8-137">Oprávnění Sudo v hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0b3e8-137">Sudo rights on hello VM</span></span>
* <span data-ttu-id="0b3e8-138">Port 22, otevřete</span><span class="sxs-lookup"><span data-stu-id="0b3e8-138">Port 22 open</span></span>

## <a name="quick-commands"></a><span data-ttu-id="0b3e8-139">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="0b3e8-139">Quick Commands</span></span>
<span data-ttu-id="0b3e8-140">*Po ostřílené Linux správci, kteří chtějí právě hello TLDR verze Začněte zde.  Pro všechny ostatní, který chce hello tuto část přeskočit podrobné vysvětlení a ukázce.*</span><span class="sxs-lookup"><span data-stu-id="0b3e8-140">*Seasoned Linux Admins who just want hello TLDR version start here.  For everyone else that wants hello detailed explanation and walk through skip this section.*</span></span>

```bash
sudo vim /etc/ssh/sshd_config
```

<span data-ttu-id="0b3e8-141">Upravte soubor konfigurace hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0b3e8-141">Edit hello config file as follows:</span></span>

```sh
# Change PasswordAuthentication toothis:
PasswordAuthentication no

# Change PubkeyAuthentication toothis:
PubkeyAuthentication yes

# Change PermitRootLogin toothis:
PermitRootLogin no

# Change ChallengeResponseAuthentication toothis:
ChallengeResponseAuthentication no
```

<span data-ttu-id="0b3e8-142">Restartujte službu SSHD hello.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-142">Restart hello SSHD service.</span></span> <span data-ttu-id="0b3e8-143">Na základě Debian distribucích:</span><span class="sxs-lookup"><span data-stu-id="0b3e8-143">On Debian-based distros:</span></span>

```bash
sudo service ssh restart
```

<span data-ttu-id="0b3e8-144">Na základě Red Hat distribucích:</span><span class="sxs-lookup"><span data-stu-id="0b3e8-144">On Red Hat-based distros:</span></span>

```bash
sudo service sshd restart
```

## <a name="detailed-walk-through"></a><span data-ttu-id="0b3e8-145">Podrobné procházení prostřednictvím</span><span class="sxs-lookup"><span data-stu-id="0b3e8-145">Detailed Walk Through</span></span>
<span data-ttu-id="0b3e8-146">Přihlášení toohello virtuálního počítače s Linuxem v terminálu 1 (T1).</span><span class="sxs-lookup"><span data-stu-id="0b3e8-146">Login toohello Linux VM on terminal 1 (T1).</span></span>  <span data-ttu-id="0b3e8-147">Přihlášení toohello virtuálního počítače s Linuxem v terminálu 2 (T2).</span><span class="sxs-lookup"><span data-stu-id="0b3e8-147">Login toohello Linux VM on terminal 2 (T2).</span></span>

<span data-ttu-id="0b3e8-148">V čase T2 přidáme tooedit hello SSHD konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-148">On T2 we are going tooedit hello SSHD configuration file.</span></span>  

```bash
sudo vim /etc/ssh/sshd_config
```

<span data-ttu-id="0b3e8-149">Zde jsme upravit právě hello nastavení toodisable hesla a povolit přihlášení klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-149">From here we will edit just hello settings toodisable passwords and enable SSH key logins.</span></span>  <span data-ttu-id="0b3e8-150">Existuje mnoho nastavení v tomto souboru, měli byste prozkoumat a toomake Linux & SSH jako zabezpečené, jako je třeba změnit.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-150">There are many settings in this file that you should research and change toomake Linux & SSH as secure as you need.</span></span>

#### <a name="disable-password-logins"></a><span data-ttu-id="0b3e8-151">Zakázat přihlášení heslo</span><span class="sxs-lookup"><span data-stu-id="0b3e8-151">Disable Password logins</span></span>

```sh
# Change PasswordAuthentication toothis:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a><span data-ttu-id="0b3e8-152">Povolit ověření veřejného klíče</span><span class="sxs-lookup"><span data-stu-id="0b3e8-152">Enable Public Key Authentication</span></span>

```sh
# Change PubkeyAuthentication toothis:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a><span data-ttu-id="0b3e8-153">Zakázat kořenové přihlášení</span><span class="sxs-lookup"><span data-stu-id="0b3e8-153">Disable Root Login</span></span>

```sh
# Change PermitRootLogin toothis:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a><span data-ttu-id="0b3e8-154">Zakázat ověřování výzvy a odezvy</span><span class="sxs-lookup"><span data-stu-id="0b3e8-154">Disable Challenge-response Authentication</span></span>
```sh
# Change ChallengeResponseAuthentication toothis:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a><span data-ttu-id="0b3e8-155">Restartujte SSHD</span><span class="sxs-lookup"><span data-stu-id="0b3e8-155">Restart SSHD</span></span>
<span data-ttu-id="0b3e8-156">Z prostředí hello T1 ověřte, že jste stále přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-156">From hello T1 shell verify that you are still logged in.</span></span>  <span data-ttu-id="0b3e8-157">To je důležité, takže můžete není zamknout z virtuálního počítače Pokud klíče SSH nejsou správné, vzhledem k tomu, že hesla jsou nyní zakázány.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-157">This is critical so you do not get locked out of your VM if your SSH keys are not correct since passwords are now disabled.</span></span>  <span data-ttu-id="0b3e8-158">Pokud jsou všechna nastavení nesprávné na virtuálním počítačům s Linuxem můžete použít T1 toofix sshd_config, které budete přihlášeni stále a SSH bude udržovat připojení hello zachování připojení při hello SSHD service restartujte.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-158">If any setting are incorrect on your Linux VM you can use T1 toofix sshd_config as you will still be logged in and SSH will keep hello connection alive during hello SSHD service restart.</span></span>

<span data-ttu-id="0b3e8-159">Z T2 spustit:</span><span class="sxs-lookup"><span data-stu-id="0b3e8-159">From T2 run:</span></span>

##### <a name="on-hello-debian-family"></a><span data-ttu-id="0b3e8-160">Na hello Debian rodiny</span><span class="sxs-lookup"><span data-stu-id="0b3e8-160">On hello Debian Family</span></span>
```bash
sudo service ssh restart
```

##### <a name="on-hello-redhat-family"></a><span data-ttu-id="0b3e8-161">Na hello rodiny RedHat</span><span class="sxs-lookup"><span data-stu-id="0b3e8-161">On hello RedHat Family</span></span>
```bash
sudo service sshd restart
```

<span data-ttu-id="0b3e8-162">Hesla jsou nyní zakázány na vašem virtuálním počítači, ochrana před hrubou silou pokusů o přihlášení heslo.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-162">Passwords are now disabled on your VM protecting it from brute force password login attempts.</span></span>  <span data-ttu-id="0b3e8-163">Pouze SSH klíče povolené bude možné toologin rychlejší a mnohem bezpečnější.</span><span class="sxs-lookup"><span data-stu-id="0b3e8-163">With only SSH Keys allowed you will be able toologin faster and much more secure.</span></span>

