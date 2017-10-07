---
title: "dvojice klíčů aaaDetailed kroky toocreate SSH pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Přečtěte si informace toocreate další kroky SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem v Azure, společně s konkrétní certifikáty pro použití v odlišných situacích."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 6/28/2017
ms.author: danlep
ms.openlocfilehash: 9ac52ef4dc87e73b9c07ccc323adc955829e2014
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="detailed-walk-through-toocreate-an-ssh-key-pair-and-additional-certificates-for-a-linux-vm-in-azure"></a><span data-ttu-id="48638-103">Podrobné procházení prostřednictvím toocreate dvojici klíčů SSH a další certifikáty pro virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="48638-103">Detailed walk through toocreate an SSH key pair and additional certificates for a Linux VM in Azure</span></span>
<span data-ttu-id="48638-104">S dvojici klíčů SSH můžete vytvořit virtuální počítače v Azure, která výchozí toousing klíče SSH k ověření, což eliminuje potřebu hello toolog hesla v.</span><span class="sxs-lookup"><span data-stu-id="48638-104">With an SSH key pair, you can create Virtual Machines on Azure that default toousing SSH keys for authentication, eliminating hello need for passwords toolog in.</span></span> <span data-ttu-id="48638-105">Hesla můžete uhádnout a otevřete virtuální počítače až toorelentless hrubou silou pokusy o tooguess heslo.</span><span class="sxs-lookup"><span data-stu-id="48638-105">Passwords can be guessed, and open your VMs up toorelentless brute force attempts tooguess your password.</span></span> <span data-ttu-id="48638-106">Virtuální počítače vytvořené pomocí rozhraní příkazového řádku Azure nebo správce prostředků šablony hello může obsahovat veřejný klíč SSH jako součást nasazení hello, odebrání krok konfigurace nasazení post zakázání heslo přihlášení SSH.</span><span class="sxs-lookup"><span data-stu-id="48638-106">VMs created with hello Azure CLI or Resource Manager templates can include your SSH public key as part of hello deployment, removing a post deployment configuration step of disabling password logins for SSH.</span></span> <span data-ttu-id="48638-107">Tento článek obsahuje podrobné pokyny a další příklady generování certifikátů, například pro použití s virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="48638-107">This article provides detailed steps and additional examples of generating certificates, such as for use with Linux virtual machines.</span></span> <span data-ttu-id="48638-108">Pokud chcete, aby tooquickly vytvořit a použít dvojici klíčů SSH naleznete v tématu [jak spárujte toocreate veřejné a privátní klíč SSH pro virtuální počítače s Linuxem v Azure](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="48638-108">If you want tooquickly create and use an SSH key pair, see [How toocreate an SSH public and private key pair for Linux VMs in Azure](mac-create-ssh-keys.md).</span></span>

## <a name="understanding-ssh-keys"></a><span data-ttu-id="48638-109">Principy klíčů SSH</span><span class="sxs-lookup"><span data-stu-id="48638-109">Understanding SSH keys</span></span>

<span data-ttu-id="48638-110">Použití veřejného a privátního klíče SSH je hello nejjednodušší způsob, jak toolog v tooyour servery se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="48638-110">Using SSH public and private keys is hello easiest way toolog in tooyour Linux servers.</span></span> <span data-ttu-id="48638-111">[Šifrování veřejného klíče](https://en.wikipedia.org/wiki/Public-key_cryptography) poskytuje mnohem bezpečnější toolog způsob, jak v tooyour Linuxem nebo BSD v Azure než používání hesel, které můžou být hrubou silou mnohem snadněji.</span><span class="sxs-lookup"><span data-stu-id="48638-111">[Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) provides a much more secure way toolog in tooyour Linux or BSD VM in Azure than passwords, which can be brute-forced far more easily.</span></span>

<span data-ttu-id="48638-112">Veřejný klíč je možné s kýmkoli sdílet. Pouze vy (nebo vaše místní infrastruktura zabezpečení) ale máte privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="48638-112">Your public key can be shared with anyone; but only you (or your local security infrastructure) possess your private key.</span></span>  <span data-ttu-id="48638-113">musí mít privátní klíč SSH Hello [velmi zabezpečeného hesla](https://www.xkcd.com/936/) (zdroj:[xkcd.com](https://xkcd.com)) toosafeguard ho.</span><span class="sxs-lookup"><span data-stu-id="48638-113">hello SSH private key should have a [very secure password](https://www.xkcd.com/936/) (source:[xkcd.com](https://xkcd.com)) toosafeguard it.</span></span>  <span data-ttu-id="48638-114">Toto heslo je právě tooaccess hello SSH soubor privátního klíče a **není** hello heslo uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="48638-114">This password is just tooaccess hello private SSH key file and **is not** hello user account password.</span></span>  <span data-ttu-id="48638-115">Když přidáte klíč SSH tooyour heslo, zašifruje hello privátního klíče pomocí standardu AES 128-bit, tak, aby hello privátní klíč nelze bez hesla toodecrypt hello ho.</span><span class="sxs-lookup"><span data-stu-id="48638-115">When you add a password tooyour SSH key, it encrypts hello private key using 128-bit AES, so that hello private key is useless without hello password toodecrypt it.</span></span>  <span data-ttu-id="48638-116">Pokud útočník stole privátní klíč a že klíč neměl heslo, nebudou moct toouse to privátního klíče toolog v tooany servery, které mají hello odpovídající veřejný klíč.</span><span class="sxs-lookup"><span data-stu-id="48638-116">If an attacker stole your private key and that key did not have a password, they would be able toouse that private key toolog in tooany servers that have hello corresponding public key.</span></span>  <span data-ttu-id="48638-117">Pokud je privátní klíč chráněný heslem, útočník ho nemůže použít, což znamená další úroveň zabezpečení pro vaši infrastrukturu v Azure.</span><span class="sxs-lookup"><span data-stu-id="48638-117">If a private key is password protected it cannot be used by that attacker, providing an additional layer of security for your infrastructure on Azure.</span></span>

<span data-ttu-id="48638-118">Tento článek vytvoří protokol SSH verze 2 pár veřejného a privátního klíče souboru RSA (také odkazované tooas "ssh-rsa" klíče), které se doporučují pro nasazení s Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="48638-118">This article creates an SSH protocol version 2 RSA public and private key file pair (also referred tooas "ssh-rsa" keys), which are recommended for deployments with Azure Resource Manager.</span></span> <span data-ttu-id="48638-119">*SSH-rsa* klíče, je nutné hello [portál](https://portal.azure.com) pro classic a nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="48638-119">*ssh-rsa* keys are required on hello [portal](https://portal.azure.com) for both classic and Resource Manager deployments.</span></span>

## <a name="ssh-keys-use-and-benefits"></a><span data-ttu-id="48638-120">Použití a výhody klíčů SSH</span><span class="sxs-lookup"><span data-stu-id="48638-120">SSH keys use and benefits</span></span>

<span data-ttu-id="48638-121">Azure vyžaduje minimálně 2048bitové protokol SSH verze 2 formátu veřejného a privátního klíče RSA; Hello soubor veřejného klíče má hello `.pub` formátu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="48638-121">Azure requires at least 2048-bit, SSH protocol version 2 RSA format public and private keys; hello public key file has hello `.pub` container format.</span></span> <span data-ttu-id="48638-122">použití klíče toocreate hello `ssh-keygen`, který zeptá na několik otázek a pak zapíše privátní klíč a odpovídající veřejný klíč.</span><span class="sxs-lookup"><span data-stu-id="48638-122">toocreate hello keys use `ssh-keygen`, which asks a series of questions and then writes a private key and a matching public key.</span></span> <span data-ttu-id="48638-123">Když je vytvořen virtuální počítač Azure, Azure kopie hello veřejného klíče toohello `~/.ssh/authorized_keys` složky na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="48638-123">When an Azure VM is created, Azure copies hello public key toohello `~/.ssh/authorized_keys` folder on hello VM.</span></span> <span data-ttu-id="48638-124">Klíče SSH v `~/.ssh/authorized_keys` jsou použité toochallenge hello toomatch hello odpovídající privátní klíč klienta na připojení přihlášení SSH.</span><span class="sxs-lookup"><span data-stu-id="48638-124">SSH keys in `~/.ssh/authorized_keys` are used toochallenge hello client toomatch hello corresponding private key on an SSH login connection.</span></span>  <span data-ttu-id="48638-125">Při vytváření virtuálního počítače Azure Linux pomocí klíče SSH pro ověřování Azure nakonfiguruje hello SSHD toonot serveru povolit přihlášení heslo, pouze klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="48638-125">When an Azure Linux VM is created using SSH keys for authentication, Azure configures hello SSHD server toonot allow password logins, only SSH keys.</span></span>  <span data-ttu-id="48638-126">Proto tak, že vytvoříte virtuální počítače Azure s Linuxem pomocí klíče SSH, můžete pomoct zabezpečit hello nasazení virtuálního počítače a uložit sami hello typické konfiguraci po nasazení krok zakázání hesla v hello **sshd_config** souboru.</span><span class="sxs-lookup"><span data-stu-id="48638-126">Therefore, by creating Azure Linux VMs with SSH keys, you can help secure hello VM deployment and save yourself hello typical post-deployment configuration step of disabling passwords in hello **sshd_config** file.</span></span>

## <a name="using-ssh-keygen"></a><span data-ttu-id="48638-127">Použití příkazu ssh-keygen</span><span class="sxs-lookup"><span data-stu-id="48638-127">Using ssh-keygen</span></span>

<span data-ttu-id="48638-128">Tento příkaz vytvoří heslo zabezpečené (šifrovaný) pár klíčů SSH, které se používá RSA 2048 bitů a je označeno jako komentář tooeasily identifikaci.</span><span class="sxs-lookup"><span data-stu-id="48638-128">This command creates a password secured (encrypted) SSH key pair using 2048-bit RSA and it is commented tooeasily identify it.</span></span>  

<span data-ttu-id="48638-129">SSH klíče jsou ve výchozím nastavení uchovávány v hello `~/.ssh` adresáře.</span><span class="sxs-lookup"><span data-stu-id="48638-129">SSH keys are by default kept in hello `~/.ssh` directory.</span></span>  <span data-ttu-id="48638-130">Pokud jste `~/.ssh` adresář, hello `ssh-keygen` příkaz ji vytvoří, můžete s hello správná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="48638-130">If you do not have a `~/.ssh` directory, hello `ssh-keygen` command creates it for you with hello correct permissions.</span></span>

```bash
ssh-keygen \
    -t rsa \
    -b 2048 \
    -C "azureuser@myserver" \
    -f ~/.ssh/id_rsa \
    -N mypassword
```

<span data-ttu-id="48638-131">*Vysvětlení příkazu*</span><span class="sxs-lookup"><span data-stu-id="48638-131">*Command explained*</span></span>

<span data-ttu-id="48638-132">`ssh-keygen`= hello program použitý toocreate hello klíče</span><span class="sxs-lookup"><span data-stu-id="48638-132">`ssh-keygen` = hello program used toocreate hello keys</span></span>

<span data-ttu-id="48638-133">`-t rsa`= Typ klíče toocreate, což je formát RSA hello [wikipedia][parens na konci](`https://en.wikipedia.org/wiki/RSA_(cryptosystem) `) 
 `-b 2048` = bity klíče hello</span><span class="sxs-lookup"><span data-stu-id="48638-133">`-t rsa` = type of key toocreate which is hello RSA format [wikipedia][parens at end](`https://en.wikipedia.org/wiki/RSA_(cryptosystem) `)
`-b 2048` = bits of hello key</span></span>

<span data-ttu-id="48638-134">`-C "azureuser@myserver"`= Komentář identifikaci připojením toohello konec tooeasily hello soubor veřejného klíče.</span><span class="sxs-lookup"><span data-stu-id="48638-134">`-C "azureuser@myserver"` = a comment appended toohello end of hello public key file tooeasily identify it.</span></span>  <span data-ttu-id="48638-135">Za normálních okolností e-mail se používá jako komentář hello ale můžete použít, ať je nejvhodnější pro vaši infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="48638-135">Normally an email is used as hello comment but you can use whatever works best for your infrastructure.</span></span>

## <a name="classic-deploy-using-asm"></a><span data-ttu-id="48638-136">Klasické nasazení pomocí `asm`</span><span class="sxs-lookup"><span data-stu-id="48638-136">Classic deploy using `asm`</span></span>

<span data-ttu-id="48638-137">Pokud používáte model nasazení classic hello (`asm` režimu v hello rozhraní příkazového řádku), můžete použít veřejný klíč SSH-RSA nebo RFC4716 naformátovaný klíče v kontejneru pem.</span><span class="sxs-lookup"><span data-stu-id="48638-137">If you are using hello classic deployment model (`asm` mode in hello CLI), you can use an SSH-RSA public key or an RFC4716 formatted key in a pem container.</span></span>  <span data-ttu-id="48638-138">veřejný klíč SSH-RSA Hello je, co byl vytvořen dříve v této konfigurace pomocí článku `ssh-keygen`.</span><span class="sxs-lookup"><span data-stu-id="48638-138">hello SSH-RSA public key is what was created earlier in this article using `ssh-keygen`.</span></span>

<span data-ttu-id="48638-139">toocreate RFC4716 naformátovaný klíče z existující veřejný klíč SSH:</span><span class="sxs-lookup"><span data-stu-id="48638-139">toocreate a RFC4716 formatted key from an existing SSH public key:</span></span>

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a><span data-ttu-id="48638-140">Příklad ssh-keygen</span><span class="sxs-lookup"><span data-stu-id="48638-140">Example of ssh-keygen</span></span>

```bash
ssh-keygen -t rsa -b 2048 -C "azureuser@myserver"
Generating public/private rsa key pair.
Enter file in which toosave hello key (/home/azureuser/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/azureuser/.ssh/id_rsa.
Your public key has been saved in /home/azureuser/.ssh/id_rsa.pub.
hello key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 azureuser@myserver
hello keys randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

<span data-ttu-id="48638-141">Uložené soubory klíčů:</span><span class="sxs-lookup"><span data-stu-id="48638-141">Saved key files:</span></span>

`Enter file in which toosave hello key (/home/azureuser/.ssh/id_rsa): ~/.ssh/id_rsa`

<span data-ttu-id="48638-142">Hello název páru klíčů pro tento článek.</span><span class="sxs-lookup"><span data-stu-id="48638-142">hello key pair name for this article.</span></span>  <span data-ttu-id="48638-143">Pár klíčů s názvem **id_rsa** je výchozí hello a některé nástroje by se dalo očekávat hello **id_rsa** soubor privátního klíče jmenovat, takže je vhodné.</span><span class="sxs-lookup"><span data-stu-id="48638-143">Having a key pair named **id_rsa** is hello default and some tools might expect hello **id_rsa** private key file name so having one is a good idea.</span></span> <span data-ttu-id="48638-144">Hello directory `~/.ssh/` hello výchozí umístění pro páry klíčů SSH a konfigurační soubor SSH hello.</span><span class="sxs-lookup"><span data-stu-id="48638-144">hello directory `~/.ssh/` is hello default location for SSH key pairs and hello SSH config file.</span></span>  <span data-ttu-id="48638-145">Pokud není zadaný s úplnou cestou `ssh-keygen` vytvoří hello klíče v hello aktuální pracovní adresář, není hello výchozí `~/.ssh`.</span><span class="sxs-lookup"><span data-stu-id="48638-145">If not specified with a full path, `ssh-keygen` creates hello keys in hello current working directory, not hello default `~/.ssh`.</span></span>

<span data-ttu-id="48638-146">Seznam hello `~/.ssh` adresáře.</span><span class="sxs-lookup"><span data-stu-id="48638-146">A listing of hello `~/.ssh` directory.</span></span>

```bash
ls -al ~/.ssh
-rw------- 1 azureuser staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 azureuser staff   410 Aug 25 18:04 id_rsa.pub
```

<span data-ttu-id="48638-147">Heslo klíče:</span><span class="sxs-lookup"><span data-stu-id="48638-147">Key Password:</span></span>

`Enter passphrase (empty for no passphrase):`

<span data-ttu-id="48638-148">`ssh-keygen`označuje tooa heslo pro soubor privátního klíče hello jako "přístupové heslo."</span><span class="sxs-lookup"><span data-stu-id="48638-148">`ssh-keygen` refers tooa password for hello private key file as "a passphrase."</span></span>  <span data-ttu-id="48638-149">Je *důrazně* doporučená tooadd privátní klíč tooyour heslo.</span><span class="sxs-lookup"><span data-stu-id="48638-149">It is *strongly* recommended tooadd a password tooyour private key.</span></span> <span data-ttu-id="48638-150">Heslo chránící hello soubor klíče každý, kdo má hello souboru můžete použít bez ho toolog tooany serveru, který má odpovídající veřejný klíč hello.</span><span class="sxs-lookup"><span data-stu-id="48638-150">Without a password protecting hello key file, anyone with hello file can use it toolog in tooany server that has hello corresponding public key.</span></span> <span data-ttu-id="48638-151">Přidání hesla (heslo) nabízí další ochranu v případě, že někdo dokáže toogain přístup tooyour soubor privátního klíče, získáte čas toochange hello klíče používají tooauthenticate můžete.</span><span class="sxs-lookup"><span data-stu-id="48638-151">Adding a password (passphrase) offers more protection in case someone is able toogain access tooyour private key file, given you time toochange hello keys used tooauthenticate you.</span></span>

## <a name="using-ssh-agent-toostore-your-private-key-password"></a><span data-ttu-id="48638-152">Pomocí ssh-agent toostore hesla k soukromému klíči</span><span class="sxs-lookup"><span data-stu-id="48638-152">Using ssh-agent toostore your private key password</span></span>

<span data-ttu-id="48638-153">tooavoid zadáním hesla soubor privátního klíče každém přihlašování přes SSH můžete použít `ssh-agent` toocache heslo soubor privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="48638-153">tooavoid typing your private key file password with every SSH login, you can use `ssh-agent` toocache your private key file password.</span></span> <span data-ttu-id="48638-154">Pokud používáte algoritmu Mac, hello klíčenky v OSX bezpečně uloží hesla k privátním klíčům hello při vyvolání `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="48638-154">If you are using a Mac, hello OSX Keychain securely stores hello private key passwords when you invoke `ssh-agent`.</span></span>

<span data-ttu-id="48638-155">Ověřte a pomocí ssh-agent a ssh přidejte tooinform hello SSH systému o soubory klíčů hello tak, aby heslo hello nebudete potřebovat toobe používá interaktivně.</span><span class="sxs-lookup"><span data-stu-id="48638-155">Verify and use ssh-agent and ssh-add tooinform hello SSH system about hello key files so that hello passphrase will not need toobe used interactively.</span></span>

```bash
eval "$(ssh-agent -s)"
```

<span data-ttu-id="48638-156">Nyní přidejte privátní klíč hello příliš`ssh-agent` pomocí příkazu hello `ssh-add`.</span><span class="sxs-lookup"><span data-stu-id="48638-156">Now add hello private key too`ssh-agent` using hello command `ssh-add`.</span></span>

```bash
ssh-add ~/.ssh/id_rsa
```

<span data-ttu-id="48638-157">heslo soukromého klíče Hello jsou teď uložená v `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="48638-157">hello private key password is now stored in `ssh-agent`.</span></span>

## <a name="using-ssh-copy-id-toocopy-hello-key-tooan-existing-vm"></a><span data-ttu-id="48638-158">Pomocí `ssh-copy-id` toocopy hello klíče tooan existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="48638-158">Using `ssh-copy-id` toocopy hello key tooan existing VM</span></span>
<span data-ttu-id="48638-159">Pokud jste již vytvořili virtuální počítač můžete nainstalovat hello nové SSH veřejného klíče tooyour virtuálního počítače s Linuxem pomocí:</span><span class="sxs-lookup"><span data-stu-id="48638-159">If you have already created a VM you can install hello new SSH public key tooyour Linux VM with:</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a><span data-ttu-id="48638-160">Vytvoření a nakonfigurování konfiguračního souboru SSH</span><span class="sxs-lookup"><span data-stu-id="48638-160">Create and configure an SSH config file</span></span>

<span data-ttu-id="48638-161">Je doporučené osvědčené toocreate postup a nakonfigurovat `~/.ssh/config` souboru toospeed až přihlašování a pro optimalizaci vaše chování klienta SSH.</span><span class="sxs-lookup"><span data-stu-id="48638-161">It is a recommended best practice toocreate and configure an `~/.ssh/config` file toospeed up log ins and for optimizing your SSH client behavior.</span></span>

<span data-ttu-id="48638-162">Hello následující příklad ukazuje standardní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="48638-162">hello following example shows a standard configuration.</span></span>

### <a name="create-hello-file"></a><span data-ttu-id="48638-163">Vytvoření souboru hello</span><span class="sxs-lookup"><span data-stu-id="48638-163">Create hello file</span></span>

```bash
touch ~/.ssh/config
```

### <a name="edit-hello-file-tooadd-hello-new-ssh-configuration"></a><span data-ttu-id="48638-164">Úpravy hello souboru tooadd hello novou konfiguraci SSH:</span><span class="sxs-lookup"><span data-stu-id="48638-164">Edit hello file tooadd hello new SSH configuration:</span></span>

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a><span data-ttu-id="48638-165">Příklad souboru `~/.ssh/config`:</span><span class="sxs-lookup"><span data-stu-id="48638-165">Example `~/.ssh/config` file:</span></span>

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User ahmet
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

<span data-ttu-id="48638-166">Tato poskytuje konfigurace SSH můžete částech pro každý server tooenable každý toohave svůj vlastní vyhrazený pár klíčů.</span><span class="sxs-lookup"><span data-stu-id="48638-166">This SSH config gives you sections for each server tooenable each toohave its own dedicated key pair.</span></span> <span data-ttu-id="48638-167">Hello výchozí nastavení (`Host *`) jsou pro všechny hostitele, kteří neodpovídají žádné hello konkrétních hostitelů vyšší nahoru v konfiguračním souboru hello.</span><span class="sxs-lookup"><span data-stu-id="48638-167">hello default settings (`Host *`) are for any hosts that do not match any of hello specific hosts higher up in hello config file.</span></span>

### <a name="config-file-explained"></a><span data-ttu-id="48638-168">Vysvětlení konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="48638-168">Config file explained</span></span>

<span data-ttu-id="48638-169">`Host`= Název hello hello hostitele volaného na hello terminálu.</span><span class="sxs-lookup"><span data-stu-id="48638-169">`Host` = hello name of hello host being called on hello terminal.</span></span>  <span data-ttu-id="48638-170">`ssh fedora22`informuje `SSH` toouse hello hodnoty v bloku nastavení hello s názvem bez přípony `Host fedora22` Poznámka: hostitele může být libovolný popisek, který je smysl pro vaše použití a nepředstavuje skutečný název hostitele hello žádného serveru.</span><span class="sxs-lookup"><span data-stu-id="48638-170">`ssh fedora22` tells `SSH` toouse hello values in hello settings block labeled `Host fedora22`  NOTE: Host can be any label that is logical for your usage and does not represent hello actual hostname of any server.</span></span>

<span data-ttu-id="48638-171">`Hostname 102.160.203.241`= hello IP adresu nebo název DNS pro server hello přistupuje.</span><span class="sxs-lookup"><span data-stu-id="48638-171">`Hostname 102.160.203.241` = hello IP address or DNS name for hello server being accessed.</span></span>

<span data-ttu-id="48638-172">`User ahmet`= toouse účet hello vzdáleného uživatele při přihlášení toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="48638-172">`User ahmet` = hello remote user account toouse when logging in toohello server.</span></span>

<span data-ttu-id="48638-173">`PubKeyAuthentication yes`= říká protokolu SSH, které mají být toouse toolog klíče SSH v.</span><span class="sxs-lookup"><span data-stu-id="48638-173">`PubKeyAuthentication yes` = tells SSH you want toouse an SSH key toolog in.</span></span>

<span data-ttu-id="48638-174">`IdentityFile /home/ahmet/.ssh/id_id_rsa`= hello SSH privátní klíč a odpovídající toouse veřejného klíče pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="48638-174">`IdentityFile /home/ahmet/.ssh/id_id_rsa` = hello SSH private key and corresponding public key toouse for authentication.</span></span>

## <a name="ssh-into-linux-without-a-password"></a><span data-ttu-id="48638-175">Přihlášení do Linuxu bez hesla pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="48638-175">SSH into Linux without a password</span></span>

<span data-ttu-id="48638-176">Nyní když máte dvojici klíčů SSH a nakonfigurovaný konfigurační soubor SSH, jsou možné toolog v tooyour virtuálního počítače s Linuxem rychle a bezpečně.</span><span class="sxs-lookup"><span data-stu-id="48638-176">Now that you have an SSH key pair and a configured SSH config file, you are able toolog in tooyour Linux VM quickly and securely.</span></span> <span data-ttu-id="48638-177">Hello poprvé přihlašujete tooa serveru pomocí SSH klíče hello příkazové řádky můžete pro hello přístupové heslo pro tento soubor klíče.</span><span class="sxs-lookup"><span data-stu-id="48638-177">hello first time you log in tooa server using an SSH key hello command prompts you for hello passphrase for that key file.</span></span>

```bash
ssh fedora22
```

### <a name="command-explained"></a><span data-ttu-id="48638-178">Vysvětlení příkazu</span><span class="sxs-lookup"><span data-stu-id="48638-178">Command explained</span></span>

<span data-ttu-id="48638-179">Když `ssh fedora22` se spustí SSH nejdříve vyhledá a načte všechna nastavení z hello `Host fedora22` blok a pak načte všechna hello zbývající nastavení z posledního bloku hello `Host *`.</span><span class="sxs-lookup"><span data-stu-id="48638-179">When `ssh fedora22` is executed SSH first locates and loads any settings from hello `Host fedora22` block, and then loads all hello remaining settings from hello last block, `Host *`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="48638-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48638-180">Next Steps</span></span>

<span data-ttu-id="48638-181">Dále si je toocreate virtuální počítače Azure s Linuxem pomocí hello nové veřejného klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="48638-181">Next up is toocreate Azure Linux VMs using hello new SSH public key.</span></span>  <span data-ttu-id="48638-182">Virtuální počítače Azure, které jsou vytvořeny pomocí veřejného klíče SSH jako hello přihlašovací údaje jsou lépe zabezpečené než virtuální počítače vytvořené pomocí hello výchozí přihlašovací metoda hesla.</span><span class="sxs-lookup"><span data-stu-id="48638-182">Azure VMs that are created with an SSH public key as hello login are better secured than VMs created with hello default login method, passwords.</span></span>  <span data-ttu-id="48638-183">Virtuální počítače Azure vytvořené pomocí klíčů SSH jsou ve výchozím nastavení nakonfigurované se zakázaným heslem, aby se vyloučily pokusy o rozluštění hesla (útokem hrubou silou).</span><span class="sxs-lookup"><span data-stu-id="48638-183">Azure VMs created using SSH keys are by default configured with passwords disabled, avoiding brute-forced guessing attempts.</span></span>

* [<span data-ttu-id="48638-184">Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure</span><span class="sxs-lookup"><span data-stu-id="48638-184">Create a secure Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="48638-185">Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="48638-185">Create a secure Linux VM using hello Azure portal</span></span>](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="48638-186">Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure hello</span><span class="sxs-lookup"><span data-stu-id="48638-186">Create a secure Linux VM using hello Azure CLI</span></span>](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
