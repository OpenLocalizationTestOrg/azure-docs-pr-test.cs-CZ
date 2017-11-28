---
title: "dvojice klíčů aaaCreate SSH pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Vytvořte bezpečně dvojici veřejného a privátního klíče SSH pro virtuální počítače Azure s Linuxem."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: rasquill
ms.openlocfilehash: c4c7cec77c9b48295f2a28c8179b30a4dc38a555
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-ssh-public-and-private-key-pair-for-linux-vms"></a><span data-ttu-id="e1321-103">Vytvoření páru veřejného a privátního klíče SSH pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="e1321-103">Create an SSH public and private key pair for Linux VMs</span></span>

<span data-ttu-id="e1321-104">Tento článek ukazuje, jak toogenerate protocol verze 2 RSA veřejné a privátní klíč SSH souboru toouse dvojice virtuálních počítačů Linux.</span><span class="sxs-lookup"><span data-stu-id="e1321-104">This article shows you how toogenerate an SSH protocol version 2 RSA public and private key file pair toouse with Linux VMs.</span></span>  <span data-ttu-id="e1321-105">S dvojici klíčů SSH můžete vytvořit virtuální počítače v Azure, která výchozí toousing klíče SSH k ověření, což eliminuje potřebu hello toolog hesla v.</span><span class="sxs-lookup"><span data-stu-id="e1321-105">With an SSH key pair, you can create Virtual Machines on Azure that default toousing SSH keys for authentication, eliminating hello need for passwords toolog in.</span></span>  <span data-ttu-id="e1321-106">Hesla můžete uhádnout a otevřete virtuální počítače až toorelentless hrubou silou pokusy o tooguess heslo.</span><span class="sxs-lookup"><span data-stu-id="e1321-106">Passwords can be guessed, and open your VMs up toorelentless brute force attempts tooguess your password.</span></span> <span data-ttu-id="e1321-107">Virtuální počítače vytvořené pomocí šablon Azure nebo hello `azure-cli` může obsahovat veřejný klíč SSH jako součást nasazení hello, odebrání krok konfigurace nasazení post zakázání heslo přihlášení SSH.</span><span class="sxs-lookup"><span data-stu-id="e1321-107">VMs created with Azure Templates or hello `azure-cli` can include your SSH public key as part of hello deployment, removing a post deployment configuration step of disabling password logins for SSH.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="e1321-108">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="e1321-108">Quick Commands</span></span>

<span data-ttu-id="e1321-109">Spuštěním následujících příkazů z prostředí Bash, nahraďte hello příklady vlastní volby hello.</span><span class="sxs-lookup"><span data-stu-id="e1321-109">Run hello following commands from a Bash shell, replacing hello examples with your own choices.</span></span>

<span data-ttu-id="e1321-110">Souboru veřejného klíče SSH se ve výchozím nastavení vytvoří v `~/.ssh/id_rsa.pub`.</span><span class="sxs-lookup"><span data-stu-id="e1321-110">Your SSH public key file is by default created at `~/.ssh/id_rsa.pub`.</span></span> <span data-ttu-id="e1321-111">Po zobrazení výzvy pomocí hello následující příkaz, byste měli vytvořit toosecure "tzn. přístupové heslo soukromého klíče.</span><span class="sxs-lookup"><span data-stu-id="e1321-111">When prompted using hello following command, you should create a "passphrase" toosecure your private key.</span></span> <span data-ttu-id="e1321-112">(heslo hello je, že heslo použít tooencrypt privátní klíč.)</span><span class="sxs-lookup"><span data-stu-id="e1321-112">(hello passphrase is a password used tooencrypt your private key.)</span></span>

```bash
ssh-keygen -t rsa -b 2048 
```

<span data-ttu-id="e1321-113">Přidejte hello nově vytvořený klíč příliš`ssh-agent`:</span><span class="sxs-lookup"><span data-stu-id="e1321-113">Add hello newly created key too`ssh-agent`:</span></span>

```bash
ssh-add ~/.ssh/id_rsa
```

> [!IMPORTANT] 
> <span data-ttu-id="e1321-114">Hello výše pracovní příkazy v operačních systémech Linux téměř všechny distribuce, ale nemusí pracovat v kontejnerech, protože hello prostředí může být výrazně omezené.</span><span class="sxs-lookup"><span data-stu-id="e1321-114">hello above commands work on Linux operating systems of almost all distributions, but do not necessarily work in containers, as hello environment can be radically constrained.</span></span> 

## <a name="detailed-walkthrough"></a><span data-ttu-id="e1321-115">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="e1321-115">Detailed Walkthrough</span></span>

<span data-ttu-id="e1321-116">Použití veřejného a privátního klíče SSH je hello nejjednodušší způsob, jak toolog v tooyour servery se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="e1321-116">Using SSH public and private keys is hello easiest way toolog in tooyour Linux servers.</span></span> <span data-ttu-id="e1321-117">[Šifrování veřejného klíče](https://en.wikipedia.org/wiki/Public-key_cryptography) poskytuje mnohem bezpečnější toolog způsob, jak v tooyour Linuxem nebo BSD v Azure než používání hesel, které můžou být hrubou silou mnohem snadněji.</span><span class="sxs-lookup"><span data-stu-id="e1321-117">[Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) provides a much more secure way toolog in tooyour Linux or BSD VM in Azure than passwords, which can be brute-forced far more easily.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e1321-118">Veřejný klíč je možné s kýmkoli sdílet. Pouze vy (nebo vaše místní infrastruktura zabezpečení) ale máte privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="e1321-118">Your public key can be shared with anyone; but only you (or your local security infrastructure) possess your private key.</span></span>  <span data-ttu-id="e1321-119">musí mít privátní klíč SSH Hello [velmi zabezpečeného hesla](https://www.xkcd.com/936/) (zdroj:[xkcd.com](https://xkcd.com)) toosafeguard ho.</span><span class="sxs-lookup"><span data-stu-id="e1321-119">hello SSH private key should have a [very secure password](https://www.xkcd.com/936/) (source:[xkcd.com](https://xkcd.com)) toosafeguard it.</span></span>  <span data-ttu-id="e1321-120">Toto heslo je právě tooaccess hello privátní klíč SSH a **není** hello heslo uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="e1321-120">This password is just tooaccess hello private SSH key and **is not** hello user account password.</span></span>  <span data-ttu-id="e1321-121">Když přidáte klíč SSH tooyour heslo, zašifruje hello privátního klíče pomocí standardu AES 128-bit, tak, aby hello privátní klíč nelze bez hesla toodecrypt hello ho.</span><span class="sxs-lookup"><span data-stu-id="e1321-121">When you add a password tooyour SSH key, it encrypts hello private key using 128-bit AES, so that hello private key is useless without hello password toodecrypt it.</span></span>  <span data-ttu-id="e1321-122">Pokud útočník stole privátní klíč a že klíč neměl heslo, nebudou moct toouse to privátního klíče toolog v tooany servery, které mají hello odpovídající veřejný klíč.</span><span class="sxs-lookup"><span data-stu-id="e1321-122">If an attacker stole your private key and that key did not have a password, they would be able toouse that private key toolog in tooany servers that have hello corresponding public key.</span></span>  <span data-ttu-id="e1321-123">Pokud je privátní klíč chráněný heslem, útočník ho nemůže použít, což znamená další úroveň zabezpečení pro vaši infrastrukturu v Azure.</span><span class="sxs-lookup"><span data-stu-id="e1321-123">If a private key is password protected it cannot be used by that attacker, providing an additional layer of security for your infrastructure on Azure.</span></span>

<span data-ttu-id="e1321-124">Tento článek vytvoří protokol SSH verze 2 RSA soubory veřejného a privátního klíče, které se doporučují pro nasazení na hello Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="e1321-124">This article creates SSH protocol version 2 RSA public and private key files, which are recommended for deployments on hello Resource Manager.</span></span>  <span data-ttu-id="e1321-125">*SSH-rsa* klíče, je nutné hello [portál](https://portal.azure.com) pro classic a nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="e1321-125">*ssh-rsa* keys are required on hello [portal](https://portal.azure.com) for both classic and Resource Manager deployments.</span></span>

## <a name="disable-ssh-passwords-by-using-ssh-keys"></a><span data-ttu-id="e1321-126">Zakázání hesel SSH použitím klíčů SSH</span><span class="sxs-lookup"><span data-stu-id="e1321-126">Disable SSH passwords by using SSH keys</span></span>

<span data-ttu-id="e1321-127">Azure vyžaduje minimálně 2048bitové veřejné a privátní klíče ve formátu ssh-rsa.</span><span class="sxs-lookup"><span data-stu-id="e1321-127">Azure requires at least 2048-bit, ssh-rsa format public and private keys.</span></span> <span data-ttu-id="e1321-128">použití klíče toocreate hello `ssh-keygen`, který zeptá na několik otázek a pak zapíše privátní klíč a odpovídající veřejný klíč.</span><span class="sxs-lookup"><span data-stu-id="e1321-128">toocreate hello keys use `ssh-keygen`, which asks a series of questions and then writes a private key and a matching public key.</span></span> <span data-ttu-id="e1321-129">Když je vytvořen virtuální počítač Azure, veřejný klíč hello zkopírován příliš`~/.ssh/authorized_keys`.</span><span class="sxs-lookup"><span data-stu-id="e1321-129">When an Azure VM is created, hello public key is copied too`~/.ssh/authorized_keys`.</span></span>  <span data-ttu-id="e1321-130">Klíče SSH v `~/.ssh/authorized_keys` jsou použité toochallenge hello toomatch hello odpovídající privátní klíč klienta na připojení přihlášení SSH.</span><span class="sxs-lookup"><span data-stu-id="e1321-130">SSH keys in `~/.ssh/authorized_keys` are used toochallenge hello client toomatch hello corresponding private key on an SSH login connection.</span></span>  <span data-ttu-id="e1321-131">Při vytváření virtuálního počítače Azure Linux pomocí klíče SSH pro ověřování Azure nakonfiguruje hello SSHD toonot serveru povolit přihlášení heslo, pouze klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="e1321-131">When an Azure Linux VM is created using SSH keys for authentication, Azure configures hello SSHD server toonot allow password logins, only SSH keys.</span></span>  <span data-ttu-id="e1321-132">Proto tak, že vytvoříte virtuální počítače Azure s Linuxem pomocí klíče SSH, můžete pomoct zabezpečené hello nasazení virtuálního počítače a uložit sami hello typické konfiguraci po nasazení krok zakázání hesla v konfiguračním souboru sshd_config hello.</span><span class="sxs-lookup"><span data-stu-id="e1321-132">Therefore, by creating Azure Linux VMs with SSH keys, you can help secure hello VM deployment and save yourself hello typical post-deployment configuration step of disabling passwords in hello sshd_config config file.</span></span>

## <a name="using-ssh-keygen"></a><span data-ttu-id="e1321-133">Použití příkazu ssh-keygen</span><span class="sxs-lookup"><span data-stu-id="e1321-133">Using ssh-keygen</span></span>

<span data-ttu-id="e1321-134">Tento příkaz vytvoří heslo zabezpečené (šifrovaný) pár klíčů SSH, které se používá RSA 2048 bitů a je označeno jako komentář tooeasily identifikaci.</span><span class="sxs-lookup"><span data-stu-id="e1321-134">This command creates a password secured (encrypted) SSH key pair using 2048-bit RSA and it is commented tooeasily identify it.</span></span>  

<span data-ttu-id="e1321-135">SSH klíče jsou ve výchozím nastavení uchovávány v hello `~/.ssh` adresáře.</span><span class="sxs-lookup"><span data-stu-id="e1321-135">SSH keys are by default kept in hello `~/.ssh` directory.</span></span>  <span data-ttu-id="e1321-136">Pokud jste `~/.ssh` adresář, hello `ssh-keygen` příkaz ji vytvoří, můžete s hello správná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="e1321-136">If you do not have a `~/.ssh` directory, hello `ssh-keygen` command creates it for you with hello correct permissions.</span></span>

```bash
ssh-keygen \
-t rsa \
-b 2048 
```

<span data-ttu-id="e1321-137">*Vysvětlení příkazu*</span><span class="sxs-lookup"><span data-stu-id="e1321-137">*Command explained*</span></span>

<span data-ttu-id="e1321-138">`ssh-keygen`= hello program použitý toocreate hello klíče</span><span class="sxs-lookup"><span data-stu-id="e1321-138">`ssh-keygen` = hello program used toocreate hello keys</span></span>

<span data-ttu-id="e1321-139">`-t rsa`= Typ klíče toocreate, což je formát RSA hello [wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)</span><span class="sxs-lookup"><span data-stu-id="e1321-139">`-t rsa` = type of key toocreate which is hello RSA format [wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)</span></span>

<span data-ttu-id="e1321-140">`-b 2048`= bity klíče hello</span><span class="sxs-lookup"><span data-stu-id="e1321-140">`-b 2048` = bits of hello key</span></span>


## <a name="classic-portal-and-x509-certs"></a><span data-ttu-id="e1321-141">Klasický portál a certifikáty X.509</span><span class="sxs-lookup"><span data-stu-id="e1321-141">Classic portal and X.509 certs</span></span>

<span data-ttu-id="e1321-142">Pokud používáte hello Azure [portálu classic](https://manage.windowsazure.com/), vyžaduje certifikátů X.509 pro hello klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="e1321-142">If you are using hello Azure [classic portal](https://manage.windowsazure.com/), it requires X.509 certs for hello SSH keys.</span></span>  <span data-ttu-id="e1321-143">Jiné typy veřejných klíčů SSH nejsou povolené, *musí* jít o certifikáty X.509.</span><span class="sxs-lookup"><span data-stu-id="e1321-143">No other types of SSH public keys are allowed, they *must* be X.509 certs.</span></span>

<span data-ttu-id="e1321-144">toocreate certifikát X.509 z existující soukromý klíč SSH-RSA:</span><span class="sxs-lookup"><span data-stu-id="e1321-144">toocreate an X.509 cert from your existing SSH-RSA private key:</span></span>

```bash
openssl req -x509 \
-key ~/.ssh/id_rsa \
-nodes \
-days 365 \
-newkey rsa:2048 \
-out ~/.ssh/id_rsa.pem
```

## <a name="classic-deploy-using-asm"></a><span data-ttu-id="e1321-145">Klasické nasazení pomocí `asm`</span><span class="sxs-lookup"><span data-stu-id="e1321-145">Classic deploy using `asm`</span></span>

<span data-ttu-id="e1321-146">Pokud používáte hello klasického nasazení modelu (Správa služby Azure CLI `asm`), můžete použít veřejný klíč SSH-RSA nebo RFC4716 naformátovaný klíče v **.pem** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e1321-146">If you are using hello classic deploy model (Azure service management CLI `asm`), you can use an SSH-RSA public key or an RFC4716 formatted key in a **.pem** container.</span></span>  <span data-ttu-id="e1321-147">veřejný klíč SSH-RSA Hello je, co byl vytvořen dříve v této konfigurace pomocí článku `ssh-keygen`.</span><span class="sxs-lookup"><span data-stu-id="e1321-147">hello SSH-RSA public key is what was created earlier in this article using `ssh-keygen`.</span></span>

<span data-ttu-id="e1321-148">toocreate RFC4716 naformátovaný klíče z existující veřejný klíč SSH:</span><span class="sxs-lookup"><span data-stu-id="e1321-148">toocreate a RFC4716 formatted key from an existing SSH public key:</span></span>

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a><span data-ttu-id="e1321-149">Příklad ssh-keygen</span><span class="sxs-lookup"><span data-stu-id="e1321-149">Example of ssh-keygen</span></span>

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@myserver"
Generating public/private rsa key pair.
Enter file in which toosave hello key (/home/ahmet/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ahmet/.ssh/id_rsa.
Your public key has been saved in /home/ahmet/.ssh/id_rsa.pub.
hello key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@myserver
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

<span data-ttu-id="e1321-150">Uložené soubory klíčů:</span><span class="sxs-lookup"><span data-stu-id="e1321-150">Saved key files:</span></span>

`Enter file in which toosave hello key (/home/ahmet/.ssh/id_rsa): ~/.ssh/id_rsa`

<span data-ttu-id="e1321-151">Hello název páru klíčů pro tento článek.</span><span class="sxs-lookup"><span data-stu-id="e1321-151">hello key pair name for this article.</span></span>  <span data-ttu-id="e1321-152">Pár klíčů s názvem **id_rsa** je výchozí hello a některé nástroje by se dalo očekávat hello **id_rsa** soubor privátního klíče jmenovat, takže je vhodné.</span><span class="sxs-lookup"><span data-stu-id="e1321-152">Having a key pair named **id_rsa** is hello default and some tools might expect hello **id_rsa** private key file name so having one is a good idea.</span></span> <span data-ttu-id="e1321-153">Hello directory `~/.ssh/` hello výchozí umístění pro páry klíčů SSH a konfigurační soubor SSH hello.</span><span class="sxs-lookup"><span data-stu-id="e1321-153">hello directory `~/.ssh/` is hello default location for SSH key pairs and hello SSH config file.</span></span>  <span data-ttu-id="e1321-154">Pokud není zadaný s úplnou cestou `ssh-keygen` bude vytvoření hello klíčů v hello aktuální pracovní adresář, není hello výchozí `~/.ssh`.</span><span class="sxs-lookup"><span data-stu-id="e1321-154">If not specified with a full path, `ssh-keygen` will create hello keys in hello current working directory, not hello default `~/.ssh`.</span></span>

<span data-ttu-id="e1321-155">Seznam hello `~/.ssh` adresáře.</span><span class="sxs-lookup"><span data-stu-id="e1321-155">A listing of hello `~/.ssh` directory.</span></span>

```bash
ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 rsa.pub
```

<span data-ttu-id="e1321-156">Heslo klíče:</span><span class="sxs-lookup"><span data-stu-id="e1321-156">Key Password:</span></span>

`Enter passphrase (empty for no passphrase):`

<span data-ttu-id="e1321-157">`ssh-keygen`označuje tooa heslo použité tooencrypt hello privátní klíč jako "přístupové heslo."</span><span class="sxs-lookup"><span data-stu-id="e1321-157">`ssh-keygen` refers tooa password used tooencrypt hello private key as "a passphrase."</span></span>  <span data-ttu-id="e1321-158">Je *důrazně* doporučená tooadd páry klíčů tooyour přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="e1321-158">It is *strongly* recommended tooadd a passphrase tooyour key pairs.</span></span> <span data-ttu-id="e1321-159">Bez přístupové heslo chránící hello privátní klíč každý, kdo má soubor klíče hello ho může použít toolog tooany server, který má odpovídající veřejný klíč hello.</span><span class="sxs-lookup"><span data-stu-id="e1321-159">Without a passphrase protecting hello private key, anyone with hello key file can use it toolog in tooany server that has hello corresponding public key.</span></span> <span data-ttu-id="e1321-160">Přidání přístupového hesla o nabízí další ochranu v případě, že někdo dokáže toogain přístup tooyour soubor privátního klíče, budete čas toochange hello klíče používají tooauthenticate můžete.</span><span class="sxs-lookup"><span data-stu-id="e1321-160">Adding a passphrase offers more protection in case someone is able toogain access tooyour private key file, giving you time toochange hello keys used tooauthenticate you.</span></span>

## <a name="using-ssh-agent-toostore-your-private-key-password"></a><span data-ttu-id="e1321-161">Pomocí ssh-agent toostore hesla k soukromému klíči</span><span class="sxs-lookup"><span data-stu-id="e1321-161">Using ssh-agent toostore your private key password</span></span>

<span data-ttu-id="e1321-162">přístupové heslo souboru tooavoid psát svůj privátní klíč v každém přihlašování přes SSH můžete použít `ssh-agent` toocache heslo soubor privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="e1321-162">tooavoid typing your private key file passphrase with every SSH login, you can use `ssh-agent` toocache your private key file password.</span></span> <span data-ttu-id="e1321-163">Pokud používáte algoritmu Mac, hello klíčenky v OSX bezpečně uloží hesla k privátním klíčům hello při vyvolání `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="e1321-163">If you are using a Mac, hello OSX Keychain securely stores hello private key passwords when you invoke `ssh-agent`.</span></span>

<span data-ttu-id="e1321-164">Ověřte a použít `ssh-agent` a `ssh-add` tooinform hello SSH systému o soubory klíčů hello tak, aby heslo hello nebudete potřebovat toobe používá interaktivně.</span><span class="sxs-lookup"><span data-stu-id="e1321-164">Verify and use `ssh-agent` and `ssh-add` tooinform hello SSH system about hello key files so that hello passphrase will not need toobe used interactively.</span></span>

```bash
eval "$(ssh-agent -s)"
```

<span data-ttu-id="e1321-165">Nyní přidejte privátní klíč hello příliš`ssh-agent` pomocí příkazu hello `ssh-add`.</span><span class="sxs-lookup"><span data-stu-id="e1321-165">Now add hello private key too`ssh-agent` using hello command `ssh-add`.</span></span>

```bash
ssh-add ~/.ssh/id_rsa
```

<span data-ttu-id="e1321-166">heslo soukromého klíče Hello jsou teď uložená v `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="e1321-166">hello private key password is now stored in `ssh-agent`.</span></span>

## <a name="using-ssh-copy-id-tooinstall-hello-new-key"></a><span data-ttu-id="e1321-167">Pomocí `ssh-copy-id` tooinstall hello nový klíč</span><span class="sxs-lookup"><span data-stu-id="e1321-167">Using `ssh-copy-id` tooinstall hello new key</span></span>
<span data-ttu-id="e1321-168">Pokud jste již vytvořili virtuální počítač můžete nainstalovat hello nové SSH veřejného klíče tooyour virtuálního počítače s Linuxem s hello následující příkaz, nahraďte hello virtuálních počítačů uživatelské jméno a adresa serveru hello vlastními hodnotami:</span><span class="sxs-lookup"><span data-stu-id="e1321-168">If you have already created a VM you can install hello new SSH public key tooyour Linux VM with hello following command, replacing hello VM username and hello server address with your own values:</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a><span data-ttu-id="e1321-169">Vytvoření a nakonfigurování konfiguračního souboru SSH</span><span class="sxs-lookup"><span data-stu-id="e1321-169">Create and configure an SSH config file</span></span>

<span data-ttu-id="e1321-170">Je nejlepší postup toocreate a nakonfigurovat `~/.ssh/config` souboru toospeed až přihlašování a pro optimalizaci vaše chování klienta SSH.</span><span class="sxs-lookup"><span data-stu-id="e1321-170">It is a best practice toocreate and configure an `~/.ssh/config` file toospeed up log ins and for optimizing your SSH client behavior.</span></span>

<span data-ttu-id="e1321-171">Hello následující příklad ukazuje standardní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e1321-171">hello following example shows a standard configuration.</span></span>

### <a name="create-hello-file"></a><span data-ttu-id="e1321-172">Vytvoření souboru hello</span><span class="sxs-lookup"><span data-stu-id="e1321-172">Create hello file</span></span>

```bash
touch ~/.ssh/config
```

### <a name="edit-hello-file-tooadd-hello-new-ssh-configuration"></a><span data-ttu-id="e1321-173">Úpravy hello souboru tooadd hello novou konfiguraci SSH:</span><span class="sxs-lookup"><span data-stu-id="e1321-173">Edit hello file tooadd hello new SSH configuration:</span></span>

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a><span data-ttu-id="e1321-174">Příklad souboru `~/.ssh/config`:</span><span class="sxs-lookup"><span data-stu-id="e1321-174">Example `~/.ssh/config` file:</span></span>

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

<span data-ttu-id="e1321-175">Tato poskytuje konfigurace SSH můžete částech pro každý server tooenable každý toohave svůj vlastní vyhrazený pár klíčů.</span><span class="sxs-lookup"><span data-stu-id="e1321-175">This SSH config gives you sections for each server tooenable each toohave its own dedicated key pair.</span></span> <span data-ttu-id="e1321-176">Hello výchozí nastavení (`Host *`) jsou pro všechny hostitele, kteří neodpovídají žádné hello konkrétních hostitelů vyšší nahoru v konfiguračním souboru hello.</span><span class="sxs-lookup"><span data-stu-id="e1321-176">hello default settings (`Host *`) are for any hosts that do not match any of hello specific hosts higher up in hello config file.</span></span>

### <a name="config-file-explained"></a><span data-ttu-id="e1321-177">Vysvětlení konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="e1321-177">Config file explained</span></span>

<span data-ttu-id="e1321-178">`Host`= Název hello hello hostitele volaného na hello terminálu.</span><span class="sxs-lookup"><span data-stu-id="e1321-178">`Host` = hello name of hello host being called on hello terminal.</span></span>  <span data-ttu-id="e1321-179">`ssh fedora22`informuje `SSH` toouse hello hodnoty v bloku nastavení hello s názvem bez přípony `Host fedora22` Poznámka: hostitele může být libovolný popisek, který je smysl pro vaše použití a nepředstavuje skutečný název hostitele hello žádného serveru.</span><span class="sxs-lookup"><span data-stu-id="e1321-179">`ssh fedora22` tells `SSH` toouse hello values in hello settings block labeled `Host fedora22`  NOTE: Host can be any label that is logical for your usage and does not represent hello actual hostname of any server.</span></span>

<span data-ttu-id="e1321-180">`Hostname 102.160.203.241`= hello IP adresu nebo název DNS pro server hello přistupuje.</span><span class="sxs-lookup"><span data-stu-id="e1321-180">`Hostname 102.160.203.241` = hello IP address or DNS name for hello server being accessed.</span></span>

<span data-ttu-id="e1321-181">`User ahmet`= toouse účet hello vzdáleného uživatele při přihlášení toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="e1321-181">`User ahmet` = hello remote user account toouse when logging in toohello server.</span></span>

<span data-ttu-id="e1321-182">`PubKeyAuthentication yes`= říká protokolu SSH, které mají být toouse toolog klíče SSH v.</span><span class="sxs-lookup"><span data-stu-id="e1321-182">`PubKeyAuthentication yes` = tells SSH you want toouse an SSH key toolog in.</span></span>

<span data-ttu-id="e1321-183">`IdentityFile /home/ahmet/.ssh/id_id_rsa`= hello SSH privátní klíč a odpovídající toouse veřejného klíče pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="e1321-183">`IdentityFile /home/ahmet/.ssh/id_id_rsa` = hello SSH private key and corresponding public key toouse for authentication.</span></span>

## <a name="ssh-into-linux-without-a-password"></a><span data-ttu-id="e1321-184">Přihlášení do Linuxu bez hesla pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="e1321-184">SSH into Linux without a password</span></span>

<span data-ttu-id="e1321-185">Nyní když máte dvojici klíčů SSH a nakonfigurovaný konfigurační soubor SSH, jsou možné toolog v tooyour virtuálního počítače s Linuxem rychle a bezpečně.</span><span class="sxs-lookup"><span data-stu-id="e1321-185">Now that you have an SSH key pair and a configured SSH config file, you are able toolog in tooyour Linux VM quickly and securely.</span></span> <span data-ttu-id="e1321-186">Hello poprvé přihlašujete tooa serveru pomocí SSH klíče hello příkazové řádky můžete pro hello přístupové heslo pro tento soubor klíče.</span><span class="sxs-lookup"><span data-stu-id="e1321-186">hello first time you log in tooa server using an SSH key hello command prompts you for hello passphrase for that key file.</span></span>

```bash
ssh fedora22
```

### <a name="command-explained"></a><span data-ttu-id="e1321-187">Vysvětlení příkazu</span><span class="sxs-lookup"><span data-stu-id="e1321-187">Command explained</span></span>

<span data-ttu-id="e1321-188">Když `ssh fedora22` se spustí SSH nejdříve vyhledá a načte všechna nastavení z hello `Host fedora22` blok a pak načte všechna hello zbývající nastavení z posledního bloku hello `Host *`.</span><span class="sxs-lookup"><span data-stu-id="e1321-188">When `ssh fedora22` is executed SSH first locates and loads any settings from hello `Host fedora22` block, and then loads all hello remaining settings from hello last block, `Host *`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1321-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e1321-189">Next Steps</span></span>

<span data-ttu-id="e1321-190">Dále si je toocreate virtuální počítače Azure s Linuxem pomocí hello nové veřejného klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="e1321-190">Next up is toocreate Azure Linux VMs using hello new SSH public key.</span></span>  <span data-ttu-id="e1321-191">Virtuální počítače Azure, které jsou vytvořeny pomocí veřejného klíče SSH jako hello přihlašovací údaje jsou lépe zabezpečené než virtuální počítače vytvořené pomocí hello výchozí přihlašovací metoda hesla.</span><span class="sxs-lookup"><span data-stu-id="e1321-191">Azure VMs that are created with an SSH public key as hello login are better secured than VMs created with hello default login method, passwords.</span></span>  <span data-ttu-id="e1321-192">Virtuální počítače Azure vytvořené pomocí klíčů SSH jsou ve výchozím nastavení nakonfigurované se zakázaným heslem, aby se vyloučily pokusy o rozluštění hesla (útokem hrubou silou).</span><span class="sxs-lookup"><span data-stu-id="e1321-192">Azure VMs created using SSH keys are by default configured with passwords disabled, avoiding brute-forced guessing attempts.</span></span> <span data-ttu-id="e1321-193">Pokud potřebujete další pomoc při vytváření dvojici klíčů SSH nebo vyžadovat další certifikáty, například jako pro použití s hello klasický portál, najdete v části [podrobné kroky toocreate páry klíčů SSH a certifikáty](create-ssh-keys-detailed.md).</span><span class="sxs-lookup"><span data-stu-id="e1321-193">If you need more assistance in creating your SSH key pair or require additional certificates, such as for use with hello classic portal, see [Detailed steps toocreate SSH key pairs and certificates](create-ssh-keys-detailed.md).</span></span>

* [<span data-ttu-id="e1321-194">Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure</span><span class="sxs-lookup"><span data-stu-id="e1321-194">Create a secure Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e1321-195">Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="e1321-195">Create a secure Linux VM using hello Azure portal</span></span>](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e1321-196">Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure hello</span><span class="sxs-lookup"><span data-stu-id="e1321-196">Create a secure Linux VM using hello Azure CLI</span></span>](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
