---
title: "aaaConfigure SSHD ve virtuálních počítačích Azure Linux | Microsoft Docs"
description: "Konfigurace SSHD osvědčené postupy zabezpečení a toolockdown SSH tooAzure virtuální počítače s Linuxem."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/21/2016
ms.author: v-livech
ms.openlocfilehash: c2361be7199a24b129c06acfc899dd32f6e1d6fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-sshd-on-azure-linux-vms"></a><span data-ttu-id="2bd7b-103">Konfigurace SSHD na virtuálních počítačích Azure Linux</span><span class="sxs-lookup"><span data-stu-id="2bd7b-103">Configure SSHD on Azure Linux VMs</span></span>

<span data-ttu-id="2bd7b-104">Tento článek ukazuje, jak toolockdown hello SSH Server Linux, tooprovide osvědčené postupy zabezpečení a také toospeed až hello procesu přihlášení SSH pomocí klíče SSH místo hesla.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-104">This article shows how toolockdown hello SSH Server on Linux, tooprovide best practices security and also toospeed up hello SSH login process by using SSH keys instead of passwords.</span></span>  <span data-ttu-id="2bd7b-105">uzamčení toofurther SSHD přidáme toodisable hello kořenového uživatele nebudou moct toologin omezit hello uživatele, které jsou povoleny toologin prostřednictvím seznam schválených skupiny zakázat protokol SSH verze 1, nastavit minimální klíče bit a nakonfigurovat automatické odhlášení uživatelů nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-105">toofurther lockdown SSHD we are going toodisable hello root user from being able toologin, limit hello users that are allowed toologin via an approved group list, disabling SSH protocol version 1, set a minimum key bit, and configure auto-logout of idle users.</span></span>  <span data-ttu-id="2bd7b-106">Hello požadavky v tomto článku jsou: účet Azure ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)) a [SSH soubory veřejného a privátního klíče](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2bd7b-106">hello requirements for this article are: an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)) and [SSH public and private key files](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="2bd7b-107">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="2bd7b-107">Quick Commands</span></span>

<span data-ttu-id="2bd7b-108">Konfigurace `/etc/ssh/sshd_config` s hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="2bd7b-108">Configure `/etc/ssh/sshd_config` with hello following settings:</span></span>

### <a name="disable-password-logins"></a><span data-ttu-id="2bd7b-109">Zakázat přihlášení heslo</span><span class="sxs-lookup"><span data-stu-id="2bd7b-109">Disable password logins</span></span>

```bash
PasswordAuthentication no
```

### <a name="disable-login-by-hello-root-user"></a><span data-ttu-id="2bd7b-110">Zakázat přihlášení uživatele root hello</span><span class="sxs-lookup"><span data-stu-id="2bd7b-110">Disable login by hello root user</span></span>

```bash
PermitRootLogin no
```

### <a name="allowed-groups-list"></a><span data-ttu-id="2bd7b-111">Skupiny seznamu povolených aplikací</span><span class="sxs-lookup"><span data-stu-id="2bd7b-111">Allowed groups list</span></span>

```bash
AllowGroups wheel
```

### <a name="allowed-users-list"></a><span data-ttu-id="2bd7b-112">Seznam povolených uživatelů</span><span class="sxs-lookup"><span data-stu-id="2bd7b-112">Allowed users list</span></span>

```bash
AllowUsers ahmet ralph
```

### <a name="disable-ssh-protocol-version-1"></a><span data-ttu-id="2bd7b-113">Zakázat protokol SSH verze 1</span><span class="sxs-lookup"><span data-stu-id="2bd7b-113">Disable SSH protocol version 1</span></span>

```bash
Protocol 2
```

### <a name="minimum-key-bits"></a><span data-ttu-id="2bd7b-114">Minimální klíče služby bits</span><span class="sxs-lookup"><span data-stu-id="2bd7b-114">Minimum key bits</span></span>

```bash
ServerKeyBits 2048
```

### <a name="disconnect-idle-users"></a><span data-ttu-id="2bd7b-115">Odpojit neaktivní uživatele</span><span class="sxs-lookup"><span data-stu-id="2bd7b-115">Disconnect idle users</span></span>

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="2bd7b-116">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="2bd7b-116">Detailed Walkthrough</span></span>

<span data-ttu-id="2bd7b-117">SSHD je hello SSH serveru, který běží na hello virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-117">SSHD is hello SSH Server that runs on hello Linux VM.</span></span>  <span data-ttu-id="2bd7b-118">SSH je klient spuštěný v systému Windows z prostředí na vaše MacBook Linux pracovní stanice, nebo Bash.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-118">SSH is a client that runs from a shell on your MacBook, Linux workstation, or from a Bash on Windows.</span></span>  <span data-ttu-id="2bd7b-119">SSH je také protokol hello používá toosecure a zašifrování komunikace hello mezi hello vytvoření SSH také virtuální privátní sítě (virtuální privátní sítě) virtuálního počítače s Linuxem a pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-119">SSH is also hello protocol used toosecure and encrypt hello communication between your workstation and hello Linux VM making SSH also a VPN (Virtual Private Network).</span></span>

<span data-ttu-id="2bd7b-120">Tento článek je velmi důležité tookeep tooyour jeden přihlášení pro celý návodu hello otevřít virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-120">For this article, it is very important tookeep one login tooyour Linux VM open for hello entire walk-through.</span></span>  <span data-ttu-id="2bd7b-121">Po vytvoření připojení SSH zůstane jako otevřít relaci tak dlouho, dokud není okno hello zavřít.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-121">Once an SSH connection is established, it remains as an open session as long as hello window is not closed.</span></span>  <span data-ttu-id="2bd7b-122">Jeden Terminálové přihlášení, umožní pro toohello toobe provedené změny SSHD service bez uzamknutí Pokud narušující změně.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-122">Having one terminal logged in, allows for changes toobe made toohello SSHD service without being locked out if a breaking change is made.</span></span>  <span data-ttu-id="2bd7b-123">Pokud získat uzamčen virtuálním počítačům s Linuxem s konfigurací porušený SSHD, Azure nabízí možnost tooreset hello porušený SSHD konfigurace s hello [rozšíření pro přístup virtuálních počítačů Azure](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2bd7b-123">If you do get locked out of your Linux VM with a broken SSHD configuration, Azure offers hello ability tooreset a broken SSHD configuration with hello [Azure VM Access Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="2bd7b-124">Z tohoto důvodu jsme otevřít dvěma terminály a toohello SSH virtuálního počítače s Linuxem z obou z nich.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-124">For this reason we open two terminals and SSH toohello Linux VM from both of them.</span></span>  <span data-ttu-id="2bd7b-125">Jsme použijte hello první terminálu toomake hello změny tooSSHDs konfigurační soubor a restartujte službu SSHD hello.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-125">We use hello first terminal toomake hello changes tooSSHDs configuration file and restart hello SSHD service.</span></span>  <span data-ttu-id="2bd7b-126">Používáme hello druhý terminálu tootest těch, které změní po restartování služby hello.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-126">We use hello second terminal tootest those changes once hello service is restarted.</span></span>  <span data-ttu-id="2bd7b-127">Protože jsme zakazování SSH hesla a program předávající výhradně na klíče SSH, pokud nejsou správné klíče SSH a zavřete toohello hello připojení virtuálních počítačů, hello virtuálních počítačů bude trvale uzamčena a nebude možné toologin tooit ho budete vyžadovat toobe odstraněno a znovu vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-127">Because we are disabling SSH passwords and relying strictly on SSH keys, if your SSH keys are not correct and you close hello connection toohello VM, hello VM will be permanently locked and no one will be able toologin tooit requiring it toobe deleted and recreated.</span></span>

## <a name="disable-password-logins"></a><span data-ttu-id="2bd7b-128">Zakázat přihlášení heslo</span><span class="sxs-lookup"><span data-stu-id="2bd7b-128">Disable password logins</span></span>

<span data-ttu-id="2bd7b-129">Hello toosecure nejrychlejší způsob, jak jste virtuální počítač s Linuxem je toodisable heslo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-129">hello quickest way toosecure you Linux VM is toodisable password logins.</span></span>  <span data-ttu-id="2bd7b-130">Pokud jsou povolené přihlášení heslo, robotů procházení webové hello okamžitě začít pokus toobrute vynutit odhad hello heslo pro váš virtuální počítač s Linuxem pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-130">When password logins are enabled, bots crawling hello web will immediately start attempting toobrute force guess hello password for your Linux VM using SSH.</span></span>  <span data-ttu-id="2bd7b-131">Zakázáním heslo přihlášení úplně, povolí hello SSH serveru tooignore všech pokusů o přihlášení heslo.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-131">Disabling password logins completely, enables hello SSH server tooignore all password login attempts.</span></span>

```bash
PasswordAuthentication no
```

## <a name="disable-login-by-hello-root-user"></a><span data-ttu-id="2bd7b-132">Zakázat přihlášení uživatele root hello</span><span class="sxs-lookup"><span data-stu-id="2bd7b-132">Disable login by hello root user</span></span>

<span data-ttu-id="2bd7b-133">Následující osvědčené postupy Linux hello `root` uživatel by měl být nikdy přihlášení k přes SSH nebo pomocí `sudo su`.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-133">Following Linux best practices, hello `root` user should never be logged into over SSH or using `sudo su`.</span></span>  <span data-ttu-id="2bd7b-134">Všechny příkazy, které vyžadují oprávnění na úrovni kořenového by měl být vždy spustit prostřednictvím hello `sudo` příkaz, který protokoluje všechny akce pro budoucí auditování.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-134">All commands needing root level permissions should always be run through hello `sudo` command, which logs all actions for future auditing.</span></span>  <span data-ttu-id="2bd7b-135">Zakázání hello `root` zabezpečení osvědčené postupy krok zajistí jenom Autorizovaní uživatelé smějí tooSSH je uživatel přihlásit se pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-135">Disabling hello `root` user from logging in via SSH is a security best practices step that ensures only authorized users are allowed tooSSH.</span></span>

```bash
PermitRootLogin no
```

## <a name="allowed-groups-list"></a><span data-ttu-id="2bd7b-136">Skupiny seznamu povolených aplikací</span><span class="sxs-lookup"><span data-stu-id="2bd7b-136">Allowed groups list</span></span>

<span data-ttu-id="2bd7b-137">SSH nabízí metody omezení uživatelů a skupin, které jsou povolené nebo zakázané přihlásit se přes protokol SSH.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-137">SSH offers a method of restricting users and group that are allowed or disallowed from logging in over SSH.</span></span>  <span data-ttu-id="2bd7b-138">Tato funkce používá tooapprove seznamy nebo odepřít konkrétní uživatele a skupiny ze přihlášení.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-138">This feature uses lists tooapprove or deny specific users and groups from logging in.</span></span>  <span data-ttu-id="2bd7b-139">Nastavení hello wheel skupiny toohello `AllowGroups` seznamu omezuje schválené přihlášení přes SSH toojust uživatelské účty, které jsou ve skupině wheel hello.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-139">Setting hello wheel group toohello `AllowGroups` list restricts approved logins over SSH toojust user accounts that are in hello wheel group.</span></span>

```bash
AllowGroups wheel
```

## <a name="allowed-users-list"></a><span data-ttu-id="2bd7b-140">Seznam povolených uživatelů</span><span class="sxs-lookup"><span data-stu-id="2bd7b-140">Allowed users list</span></span>

<span data-ttu-id="2bd7b-141">Omezit uživatelům toojust přihlášení SSH je více určitým způsobem tooaccomplish hello stejná úloha, která `AllowGroups` je.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-141">Restricting SSH logins toojust users is a more specific way tooaccomplish hello same task that `AllowGroups` is.</span></span>  

```bash
AllowUsers ahmet ralph
```

## <a name="disable-ssh-protocol-version-1"></a><span data-ttu-id="2bd7b-142">Zakázat protokol SSH verze 1</span><span class="sxs-lookup"><span data-stu-id="2bd7b-142">Disable SSH protocol version 1</span></span>

<span data-ttu-id="2bd7b-143">Protokol SSH verze 1 je nezabezpečené a by mělo být zakázáno.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-143">SSH protocol version 1 is insecure and should be disabled.</span></span>  <span data-ttu-id="2bd7b-144">Protokol SSH verze 2 je hello aktuální verzi, která nabízí bezpečný tooSSH tooyour serveru.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-144">SSH protocol version 2 is hello current version that offers a secure way tooSSH tooyour server.</span></span>  <span data-ttu-id="2bd7b-145">Zakázání SSH verze 1 odmítne všechny SSH klienty, kteří se pokoušejí tooestablish připojení k serveru SSH hello pomocí SSH verze 1.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-145">Disabling SSH version 1 denies any SSH clients that are attempting tooestablish a connection with hello SSH server using SSH version 1.</span></span>  <span data-ttu-id="2bd7b-146">Pouze verze 2 připojení SSH. jsou povolené toonegotiate připojení k serveru SSH hello.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-146">Only SSH version 2 connections are allowed toonegotiate a connection with hello SSH server.</span></span>

```bash
Protocol 2
```

## <a name="minimum-key-bits"></a><span data-ttu-id="2bd7b-147">Minimální klíče služby bits</span><span class="sxs-lookup"><span data-stu-id="2bd7b-147">Minimum key bits</span></span>

<span data-ttu-id="2bd7b-148">Následující osvědčené postupy zabezpečení jsou zakázané heslo SSH přihlášení a toobe použít s serverem SSH hello tooauthenticate jsou povoleny pouze klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-148">Following security best practices, password SSH logins are disabled and only SSH keys are allowed toobe used tooauthenticate with hello SSH server.</span></span>  <span data-ttu-id="2bd7b-149">Tyto klíče SSH lze vytvořit pomocí jinou délku klíče, měřen v bitech.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-149">These SSH keys can be created using different length keys, measured in bits.</span></span>  <span data-ttu-id="2bd7b-150">Osvědčené postupy stavy, zda jsou klíče 2048 bitů hello minimální přijatelnou síly klíče.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-150">Best practices states that keys of 2048 bits in length are hello minimum acceptable key strength.</span></span>  <span data-ttu-id="2bd7b-151">Klíče menší než 2 048 bitů, teoreticky může být poškozený.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-151">Keys of less than 2048 bits could theoretically be broken.</span></span>  <span data-ttu-id="2bd7b-152">Nastavení hello `ServerKeyBits` příliš`2048` umožňuje všechna připojení pomocí klíče 2 048 bitů nebo větší a Odepřít připojení menší než 2 048 bitů.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-152">Setting hello `ServerKeyBits` too`2048` allows any connections using keys of 2048 bits or greater and deny connections of less than 2048 bits.</span></span>

```bash
ServerKeyBits 2048
```

## <a name="disconnect-idle-users"></a><span data-ttu-id="2bd7b-153">Odpojit neaktivní uživatele</span><span class="sxs-lookup"><span data-stu-id="2bd7b-153">Disconnect idle users</span></span>

<span data-ttu-id="2bd7b-154">SSH má hello možnost toodisconnect uživatele, kteří mají otevřená připojení, které zůstaly déle než nastavit dobu v sekundách nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-154">SSH has hello ability toodisconnect users that have open connections that have remained idle for more than a set period of seconds.</span></span>  <span data-ttu-id="2bd7b-155">Udržování otevřených relací tooonly uživatelům, kteří jsou aktivní omezení hello úniku hello virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-155">Keeping open sessions tooonly those users who are active limits hello exposure of hello Linux VM.</span></span>

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="restart-sshd"></a><span data-ttu-id="2bd7b-156">Restartujte SSHD</span><span class="sxs-lookup"><span data-stu-id="2bd7b-156">Restart SSHD</span></span>

<span data-ttu-id="2bd7b-157">nastavení hello tooenable v `/etc/ssh/sshd_config` restartujte hello SSHD procesu, který restartuje hello SSH server.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-157">tooenable hello settings in `/etc/ssh/sshd_config` restart hello SSHD process which restarts hello SSH server.</span></span>  <span data-ttu-id="2bd7b-158">okno terminálu Hello používáte toorestart hello SSH server zůstanou otevřené bez ztráty hello otevřít relaci SSH.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-158">hello terminal window you use toorestart hello SSH server remain open without losing hello open SSH session.</span></span>  <span data-ttu-id="2bd7b-159">Nastavení nové SSH serveru tootest hello použít druhý okno terminálu nebo kartě.  Pomocí samostatných terminálu tootest hello připojení SSH umožňuje toogo zpět a zkontrolujte další změny toohello `/etc/ssh/sshd_config` v první terminálu hello bez uzamknutí SSHD změnou ukončování řádků.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-159">tootest hello new SSH server settings use a second terminal window or tab.  Using a separate terminal tootest hello SSH connection allows you toogo back and make additional changes toohello `/etc/ssh/sshd_config` in hello first terminal, without being locked out by a breaking SSHD change.</span></span>  

### <a name="on-redhat-centos-and-fedora"></a><span data-ttu-id="2bd7b-160">Red Hat, Centos a Fedora</span><span class="sxs-lookup"><span data-stu-id="2bd7b-160">On Redhat, Centos and Fedora</span></span>

```bash
service sshd restart
```

### <a name="on-debian--ubuntu"></a><span data-ttu-id="2bd7b-161">Na Debian a Ubuntu</span><span class="sxs-lookup"><span data-stu-id="2bd7b-161">On Debian & Ubuntu</span></span>

```bash
service ssh restart
```

## <a name="reset-sshd-using-azure-reset-access"></a><span data-ttu-id="2bd7b-162">Resetování SSHD pomocí Azure resetování access</span><span class="sxs-lookup"><span data-stu-id="2bd7b-162">Reset SSHD using Azure reset-access</span></span>

<span data-ttu-id="2bd7b-163">Pokud jsou uzamčen z konfigurace SSHD toohello narušující změny můžete hello virtuální počítač Azure – rozšíření pro přístup tooreset hello SSHD back toohello původní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-163">If you are locked out from a breaking change toohello SSHD configuration you can use hello Azure VM access-extension tooreset hello SSHD configuration back toohello original configuration.</span></span>

<span data-ttu-id="2bd7b-164">Nahraďte všechny názvy příklad vlastními.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-164">Replace any example names with your own.</span></span>

```azurecli
azure vm reset-access \
--resource-group myResourceGroup \
--name myVM \
--reset-ssh
```

## <a name="install-fail2ban"></a><span data-ttu-id="2bd7b-165">Nainstalujte Fail2ban</span><span class="sxs-lookup"><span data-stu-id="2bd7b-165">Install Fail2ban</span></span>

<span data-ttu-id="2bd7b-166">Důrazně doporučujeme tooinstall a instalační program hello s otevřeným zdrojem aplikace Fail2ban, které bloky opakované pokusy o toologin tooyour virtuálního počítače s Linuxem přes protokol SSH pomocí hrubou silou.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-166">It is strongly recommended tooinstall and setup hello open source app Fail2ban, which blocks repeated attempts toologin tooyour Linux VM over SSH using brute force.</span></span>  <span data-ttu-id="2bd7b-167">Protokoly Fail2ban opakované pokusy o toologin převzal SSH a poté vytvoří pravidla brány firewall tooblock hello IP adresu, která jsou pokusy o hello pocházející z.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-167">Fail2ban logs repeated failed attempts toologin over SSH and then creates firewall rules tooblock hello IP address that hello attempts are originating from.</span></span>

* [<span data-ttu-id="2bd7b-168">Fail2ban domovské stránky</span><span class="sxs-lookup"><span data-stu-id="2bd7b-168">Fail2ban homepage</span></span>](http://www.fail2ban.org/wiki/index.php/Main_Page)

## <a name="next-steps"></a><span data-ttu-id="2bd7b-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2bd7b-169">Next Steps</span></span>

<span data-ttu-id="2bd7b-170">Teď, když jste nakonfigurovali a uzamčené hello SSH serveru na virtuálním počítačům s Linuxem existují další bezpečnostní osvědčené postupy, které můžete provést.</span><span class="sxs-lookup"><span data-stu-id="2bd7b-170">Now that you have configured and locked down hello SSH server on your Linux VM there are additional security best practices you can follow.</span></span>  

* [<span data-ttu-id="2bd7b-171">Spravovat uživatele, SSH a zkontrolujte nebo hello oprava disky na virtuální počítače Azure s Linuxem pomocí rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="2bd7b-171">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [<span data-ttu-id="2bd7b-172">Šifrování disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure hello</span><span class="sxs-lookup"><span data-stu-id="2bd7b-172">Encrypt disks on a Linux VM using hello Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [<span data-ttu-id="2bd7b-173">Přístup a zabezpečení v šablonách Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2bd7b-173">Access and security in Azure Resource Manager templates</span></span>](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
