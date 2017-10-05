---
title: "Konfigurace SSHD na virtuálních počítačích Azure Linux | Microsoft Docs"
description: "Nakonfigurujte SSHD pro osvědčené postupy zabezpečení a uzamčení SSH na Linux virtuálních počítačích Azure."
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
ms.openlocfilehash: 0195c385b00ab92b2b92ce8ff00983a0d91bf3a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-sshd-on-azure-linux-vms"></a><span data-ttu-id="6c906-103">Konfigurace SSHD na virtuálních počítačích Azure Linux</span><span class="sxs-lookup"><span data-stu-id="6c906-103">Configure SSHD on Azure Linux VMs</span></span>

<span data-ttu-id="6c906-104">Tento článek ukazuje, jak mají uzamčení SSH serveru v systému Linux, osvědčené postupy zabezpečení a také urychlit proces přihlášení SSH pomocí protokolu SSH klíče místo hesla.</span><span class="sxs-lookup"><span data-stu-id="6c906-104">This article shows how to lockdown the SSH Server on Linux, to provide best practices security and also to speed up the SSH login process by using SSH keys instead of passwords.</span></span>  <span data-ttu-id="6c906-105">Pro další uzamčení SSHD přidáme zakázat kořenového uživatele nebudou moci přihlásit, omezit uživatele, kteří mohou k přihlášení pomocí seznam schválených skupin, zakázat protokol SSH verze 1, nastavit minimální klíče bit a nakonfigurujte automaticky odhlášení uživatelů nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="6c906-105">To further lockdown SSHD we are going to disable the root user from being able to login, limit the users that are allowed to login via an approved group list, disabling SSH protocol version 1, set a minimum key bit, and configure auto-logout of idle users.</span></span>  <span data-ttu-id="6c906-106">Požadavky na tomto článku jsou: účet Azure ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)) a [SSH soubory veřejného a privátního klíče](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6c906-106">The requirements for this article are: an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)) and [SSH public and private key files](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="6c906-107">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="6c906-107">Quick Commands</span></span>

<span data-ttu-id="6c906-108">Konfigurace `/etc/ssh/sshd_config` s následujícím nastavením:</span><span class="sxs-lookup"><span data-stu-id="6c906-108">Configure `/etc/ssh/sshd_config` with the following settings:</span></span>

### <a name="disable-password-logins"></a><span data-ttu-id="6c906-109">Zakázat přihlášení heslo</span><span class="sxs-lookup"><span data-stu-id="6c906-109">Disable password logins</span></span>

```bash
PasswordAuthentication no
```

### <a name="disable-login-by-the-root-user"></a><span data-ttu-id="6c906-110">Zakázat přihlášení uživatele root</span><span class="sxs-lookup"><span data-stu-id="6c906-110">Disable login by the root user</span></span>

```bash
PermitRootLogin no
```

### <a name="allowed-groups-list"></a><span data-ttu-id="6c906-111">Skupiny seznamu povolených aplikací</span><span class="sxs-lookup"><span data-stu-id="6c906-111">Allowed groups list</span></span>

```bash
AllowGroups wheel
```

### <a name="allowed-users-list"></a><span data-ttu-id="6c906-112">Seznam povolených uživatelů</span><span class="sxs-lookup"><span data-stu-id="6c906-112">Allowed users list</span></span>

```bash
AllowUsers ahmet ralph
```

### <a name="disable-ssh-protocol-version-1"></a><span data-ttu-id="6c906-113">Zakázat protokol SSH verze 1</span><span class="sxs-lookup"><span data-stu-id="6c906-113">Disable SSH protocol version 1</span></span>

```bash
Protocol 2
```

### <a name="minimum-key-bits"></a><span data-ttu-id="6c906-114">Minimální klíče služby bits</span><span class="sxs-lookup"><span data-stu-id="6c906-114">Minimum key bits</span></span>

```bash
ServerKeyBits 2048
```

### <a name="disconnect-idle-users"></a><span data-ttu-id="6c906-115">Odpojit neaktivní uživatele</span><span class="sxs-lookup"><span data-stu-id="6c906-115">Disconnect idle users</span></span>

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="6c906-116">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="6c906-116">Detailed Walkthrough</span></span>

<span data-ttu-id="6c906-117">SSHD je SSH Server, který běží na virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="6c906-117">SSHD is the SSH Server that runs on the Linux VM.</span></span>  <span data-ttu-id="6c906-118">SSH je klient spuštěný v systému Windows z prostředí na vaše MacBook Linux pracovní stanice, nebo Bash.</span><span class="sxs-lookup"><span data-stu-id="6c906-118">SSH is a client that runs from a shell on your MacBook, Linux workstation, or from a Bash on Windows.</span></span>  <span data-ttu-id="6c906-119">SSH je také protokol použitý k zabezpečení a zašifrování komunikace mezi pracovní stanice a virtuálního počítače s Linuxem vytvoření SSH také virtuální privátní sítě (virtuální privátní sítě).</span><span class="sxs-lookup"><span data-stu-id="6c906-119">SSH is also the protocol used to secure and encrypt the communication between your workstation and the Linux VM making SSH also a VPN (Virtual Private Network).</span></span>

<span data-ttu-id="6c906-120">V tomto článku je velmi důležité udržovat jeden přihlášení k virtuálním počítačům s Linuxem otevřené pro celý návodu.</span><span class="sxs-lookup"><span data-stu-id="6c906-120">For this article, it is very important to keep one login to your Linux VM open for the entire walk-through.</span></span>  <span data-ttu-id="6c906-121">Po vytvoření připojení SSH zůstane jako otevřít relaci tak dlouho, dokud není okno zavřeno.</span><span class="sxs-lookup"><span data-stu-id="6c906-121">Once an SSH connection is established, it remains as an open session as long as the window is not closed.</span></span>  <span data-ttu-id="6c906-122">Jeden Terminálové přihlášení, umožní změny prováděné SSHD service bez uzamknutí Pokud narušující změně.</span><span class="sxs-lookup"><span data-stu-id="6c906-122">Having one terminal logged in, allows for changes to be made to the SSHD service without being locked out if a breaking change is made.</span></span>  <span data-ttu-id="6c906-123">Pokud jste zamknout z virtuálního počítače Linux s konfigurací porušený SSHD, Azure nabízí možnost obnovit poškozený SSHD konfigurace s [rozšíření pro přístup virtuálních počítačů Azure](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6c906-123">If you do get locked out of your Linux VM with a broken SSHD configuration, Azure offers the ability to reset a broken SSHD configuration with the [Azure VM Access Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="6c906-124">Z tohoto důvodu jsme otevřít dva terminály a SSH pro virtuální počítač s Linuxem z obou z nich.</span><span class="sxs-lookup"><span data-stu-id="6c906-124">For this reason we open two terminals and SSH to the Linux VM from both of them.</span></span>  <span data-ttu-id="6c906-125">Pro provedení změn SSHDs konfigurační soubor a restartujte službu SSHD používáme první terminálu.</span><span class="sxs-lookup"><span data-stu-id="6c906-125">We use the first terminal to make the changes to SSHDs configuration file and restart the SSHD service.</span></span>  <span data-ttu-id="6c906-126">Druhý terminálu používáme k testování tyto změny po restartování služby.</span><span class="sxs-lookup"><span data-stu-id="6c906-126">We use the second terminal to test those changes once the service is restarted.</span></span>  <span data-ttu-id="6c906-127">Protože jsme zakazování SSH hesla a program předávající výhradně na klíče SSH, pokud nejsou správné klíče SSH a zavřete připojení k virtuálnímu počítači, virtuální počítač bude trvale uzamčena a nebude moci přihlásit k němu nutnosti ho odstranit a znovu vytvoří.</span><span class="sxs-lookup"><span data-stu-id="6c906-127">Because we are disabling SSH passwords and relying strictly on SSH keys, if your SSH keys are not correct and you close the connection to the VM, the VM will be permanently locked and no one will be able to login to it requiring it to be deleted and recreated.</span></span>

## <a name="disable-password-logins"></a><span data-ttu-id="6c906-128">Zakázat přihlášení heslo</span><span class="sxs-lookup"><span data-stu-id="6c906-128">Disable password logins</span></span>

<span data-ttu-id="6c906-129">Nejrychlejší způsob, jak zabezpečit virtuálního počítače s Linuxem je zakázat přihlášení heslo.</span><span class="sxs-lookup"><span data-stu-id="6c906-129">The quickest way to secure you Linux VM is to disable password logins.</span></span>  <span data-ttu-id="6c906-130">Pokud jsou povolené heslo přihlášení, robotů procházení webu bude okamžitě počáteční pokus o hrubou silou uhodnout heslo pro váš virtuální počítač s Linuxem pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="6c906-130">When password logins are enabled, bots crawling the web will immediately start attempting to brute force guess the password for your Linux VM using SSH.</span></span>  <span data-ttu-id="6c906-131">Zakázání heslo přihlášení úplně, umožňuje serveru SSH ignorovat všechny pokusů o přihlášení heslo.</span><span class="sxs-lookup"><span data-stu-id="6c906-131">Disabling password logins completely, enables the SSH server to ignore all password login attempts.</span></span>

```bash
PasswordAuthentication no
```

## <a name="disable-login-by-the-root-user"></a><span data-ttu-id="6c906-132">Zakázat přihlášení uživatele root</span><span class="sxs-lookup"><span data-stu-id="6c906-132">Disable login by the root user</span></span>

<span data-ttu-id="6c906-133">Následující osvědčené postupy Linux, `root` uživatel by měl být nikdy přihlášení k přes SSH nebo pomocí `sudo su`.</span><span class="sxs-lookup"><span data-stu-id="6c906-133">Following Linux best practices, the `root` user should never be logged into over SSH or using `sudo su`.</span></span>  <span data-ttu-id="6c906-134">Všechny příkazy, které vyžadují oprávnění na úrovni kořenového by měl být vždy spustit `sudo` příkaz, který protokoluje všechny akce pro budoucí auditování.</span><span class="sxs-lookup"><span data-stu-id="6c906-134">All commands needing root level permissions should always be run through the `sudo` command, which logs all actions for future auditing.</span></span>  <span data-ttu-id="6c906-135">Zakázání `root` uživatele přihlásit se pomocí protokolu SSH je zabezpečení nejlepší postupy krok, které zajišťuje, aby jenom Autorizovaní uživatelé smějí SSH.</span><span class="sxs-lookup"><span data-stu-id="6c906-135">Disabling the `root` user from logging in via SSH is a security best practices step that ensures only authorized users are allowed to SSH.</span></span>

```bash
PermitRootLogin no
```

## <a name="allowed-groups-list"></a><span data-ttu-id="6c906-136">Skupiny seznamu povolených aplikací</span><span class="sxs-lookup"><span data-stu-id="6c906-136">Allowed groups list</span></span>

<span data-ttu-id="6c906-137">SSH nabízí metody omezení uživatelů a skupin, které jsou povolené nebo zakázané přihlásit se přes protokol SSH.</span><span class="sxs-lookup"><span data-stu-id="6c906-137">SSH offers a method of restricting users and group that are allowed or disallowed from logging in over SSH.</span></span>  <span data-ttu-id="6c906-138">Tato funkce používá seznamy schválí nebo zamítne konkrétní uživatele a skupiny z přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6c906-138">This feature uses lists to approve or deny specific users and groups from logging in.</span></span>  <span data-ttu-id="6c906-139">Nastavení skupiny kolečka `AllowGroups` seznamu omezuje schválené přihlášení přes protokol SSH pouze uživatelské účty, které jsou ve skupině kolečka.</span><span class="sxs-lookup"><span data-stu-id="6c906-139">Setting the wheel group to the `AllowGroups` list restricts approved logins over SSH to just user accounts that are in the wheel group.</span></span>

```bash
AllowGroups wheel
```

## <a name="allowed-users-list"></a><span data-ttu-id="6c906-140">Seznam povolených uživatelů</span><span class="sxs-lookup"><span data-stu-id="6c906-140">Allowed users list</span></span>

<span data-ttu-id="6c906-141">Omezení přihlášení SSH jenom uživatelům je konkrétnější způsob, jak provést stejný úloha, která `AllowGroups` je.</span><span class="sxs-lookup"><span data-stu-id="6c906-141">Restricting SSH logins to just users is a more specific way to accomplish the same task that `AllowGroups` is.</span></span>  

```bash
AllowUsers ahmet ralph
```

## <a name="disable-ssh-protocol-version-1"></a><span data-ttu-id="6c906-142">Zakázat protokol SSH verze 1</span><span class="sxs-lookup"><span data-stu-id="6c906-142">Disable SSH protocol version 1</span></span>

<span data-ttu-id="6c906-143">Protokol SSH verze 1 je nezabezpečené a by mělo být zakázáno.</span><span class="sxs-lookup"><span data-stu-id="6c906-143">SSH protocol version 1 is insecure and should be disabled.</span></span>  <span data-ttu-id="6c906-144">Protokol SSH verze 2 je aktuální verze, která nabízí bezpečný způsob SSH na váš server.</span><span class="sxs-lookup"><span data-stu-id="6c906-144">SSH protocol version 2 is the current version that offers a secure way to SSH to your server.</span></span>  <span data-ttu-id="6c906-145">Zakázání SSH verze 1 odmítne všechny SSH klienty, kteří se pokoušejí navázat spojení se serverem SSH pomocí SSH verze 1.</span><span class="sxs-lookup"><span data-stu-id="6c906-145">Disabling SSH version 1 denies any SSH clients that are attempting to establish a connection with the SSH server using SSH version 1.</span></span>  <span data-ttu-id="6c906-146">Připojení k serveru SSH jsou povoleny pouze připojení SSH verze 2.</span><span class="sxs-lookup"><span data-stu-id="6c906-146">Only SSH version 2 connections are allowed to negotiate a connection with the SSH server.</span></span>

```bash
Protocol 2
```

## <a name="minimum-key-bits"></a><span data-ttu-id="6c906-147">Minimální klíče služby bits</span><span class="sxs-lookup"><span data-stu-id="6c906-147">Minimum key bits</span></span>

<span data-ttu-id="6c906-148">Následující osvědčené postupy zabezpečení přihlášení SSH heslo jsou zakázána a který se má použít k ověření serveru SSH jsou povoleny pouze klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="6c906-148">Following security best practices, password SSH logins are disabled and only SSH keys are allowed to be used to authenticate with the SSH server.</span></span>  <span data-ttu-id="6c906-149">Tyto klíče SSH lze vytvořit pomocí jinou délku klíče, měřen v bitech.</span><span class="sxs-lookup"><span data-stu-id="6c906-149">These SSH keys can be created using different length keys, measured in bits.</span></span>  <span data-ttu-id="6c906-150">Osvědčené postupy stavy, zda jsou klíče 2048 bitů minimální přijatelnou síly klíče.</span><span class="sxs-lookup"><span data-stu-id="6c906-150">Best practices states that keys of 2048 bits in length are the minimum acceptable key strength.</span></span>  <span data-ttu-id="6c906-151">Klíče menší než 2 048 bitů, teoreticky může být poškozený.</span><span class="sxs-lookup"><span data-stu-id="6c906-151">Keys of less than 2048 bits could theoretically be broken.</span></span>  <span data-ttu-id="6c906-152">Nastavení `ServerKeyBits` k `2048` umožňuje všechna připojení pomocí klíče 2 048 bitů nebo větší a Odepřít připojení menší než 2 048 bitů.</span><span class="sxs-lookup"><span data-stu-id="6c906-152">Setting the `ServerKeyBits` to `2048` allows any connections using keys of 2048 bits or greater and deny connections of less than 2048 bits.</span></span>

```bash
ServerKeyBits 2048
```

## <a name="disconnect-idle-users"></a><span data-ttu-id="6c906-153">Odpojit neaktivní uživatele</span><span class="sxs-lookup"><span data-stu-id="6c906-153">Disconnect idle users</span></span>

<span data-ttu-id="6c906-154">SSH má možnost Odpojit uživatele, kteří mají otevřená připojení, které zůstaly déle než nastavit dobu v sekundách nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="6c906-154">SSH has the ability to disconnect users that have open connections that have remained idle for more than a set period of seconds.</span></span>  <span data-ttu-id="6c906-155">Udržování otevřených relací pouze na uživatele, kteří jsou aktivní omezuje zranitelnost virtuálního počítače systému Linux.</span><span class="sxs-lookup"><span data-stu-id="6c906-155">Keeping open sessions to only those users who are active limits the exposure of the Linux VM.</span></span>

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="restart-sshd"></a><span data-ttu-id="6c906-156">Restartujte SSHD</span><span class="sxs-lookup"><span data-stu-id="6c906-156">Restart SSHD</span></span>

<span data-ttu-id="6c906-157">Chcete-li povolit nastavení v `/etc/ssh/sshd_config` restartujte SSHD procesu, který restartuje SSH server.</span><span class="sxs-lookup"><span data-stu-id="6c906-157">To enable the settings in `/etc/ssh/sshd_config` restart the SSHD process which restarts the SSH server.</span></span>  <span data-ttu-id="6c906-158">Bez ztráty otevřít relaci SSH zůstanou otevřené okno terminálu, který používáte pro restartování serveru SSH.</span><span class="sxs-lookup"><span data-stu-id="6c906-158">The terminal window you use to restart the SSH server remain open without losing the open SSH session.</span></span>  <span data-ttu-id="6c906-159">K otestování nové SSH nastavení serveru použijte druhý terminálu okně nebo záložce.</span><span class="sxs-lookup"><span data-stu-id="6c906-159">To test the new SSH server settings use a second terminal window or tab.</span></span>  <span data-ttu-id="6c906-160">Použití samostatných terminál k testování připojení SSH můžete přejít zpět a provést další změny `/etc/ssh/sshd_config` v první terminálu, bez uzamknutí SSHD změnou ukončování řádků.</span><span class="sxs-lookup"><span data-stu-id="6c906-160">Using a separate terminal to test the SSH connection allows you to go back and make additional changes to the `/etc/ssh/sshd_config` in the first terminal, without being locked out by a breaking SSHD change.</span></span>  

### <a name="on-redhat-centos-and-fedora"></a><span data-ttu-id="6c906-161">Red Hat, Centos a Fedora</span><span class="sxs-lookup"><span data-stu-id="6c906-161">On Redhat, Centos and Fedora</span></span>

```bash
service sshd restart
```

### <a name="on-debian--ubuntu"></a><span data-ttu-id="6c906-162">Na Debian a Ubuntu</span><span class="sxs-lookup"><span data-stu-id="6c906-162">On Debian & Ubuntu</span></span>

```bash
service ssh restart
```

## <a name="reset-sshd-using-azure-reset-access"></a><span data-ttu-id="6c906-163">Resetování SSHD pomocí Azure resetování access</span><span class="sxs-lookup"><span data-stu-id="6c906-163">Reset SSHD using Azure reset-access</span></span>

<span data-ttu-id="6c906-164">Pokud jste se z narušující změně uzamčen SSHD konfigurace můžete použít rozšíření přístup k virtuálnímu počítači Azure resetování konfigurace SSHD zpět do původní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6c906-164">If you are locked out from a breaking change to the SSHD configuration you can use the Azure VM access-extension to reset the SSHD configuration back to the original configuration.</span></span>

<span data-ttu-id="6c906-165">Nahraďte všechny názvy příklad vlastními.</span><span class="sxs-lookup"><span data-stu-id="6c906-165">Replace any example names with your own.</span></span>

```azurecli
azure vm reset-access \
--resource-group myResourceGroup \
--name myVM \
--reset-ssh
```

## <a name="install-fail2ban"></a><span data-ttu-id="6c906-166">Nainstalujte Fail2ban</span><span class="sxs-lookup"><span data-stu-id="6c906-166">Install Fail2ban</span></span>

<span data-ttu-id="6c906-167">Důrazně doporučujeme při instalaci a nastavení aplikace s otevřeným zdrojem Fail2ban, která blokuje opakovaných pokusech o přihlášení k virtuálním počítačům s Linuxem přes protokol SSH pomocí hrubou silou.</span><span class="sxs-lookup"><span data-stu-id="6c906-167">It is strongly recommended to install and setup the open source app Fail2ban, which blocks repeated attempts to login to your Linux VM over SSH using brute force.</span></span>  <span data-ttu-id="6c906-168">Fail2ban protokoly opakovaných neúspěšných pokusech o přihlášení přes SSH a pak vytvoří pravidla brány firewall pro blokování IP adresu, která jsou na pokusy pocházející z.</span><span class="sxs-lookup"><span data-stu-id="6c906-168">Fail2ban logs repeated failed attempts to login over SSH and then creates firewall rules to block the IP address that the attempts are originating from.</span></span>

* [<span data-ttu-id="6c906-169">Fail2ban domovské stránky</span><span class="sxs-lookup"><span data-stu-id="6c906-169">Fail2ban homepage</span></span>](http://www.fail2ban.org/wiki/index.php/Main_Page)

## <a name="next-steps"></a><span data-ttu-id="6c906-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6c906-170">Next Steps</span></span>

<span data-ttu-id="6c906-171">Teď, když jste nakonfigurovali a uzamčené serverem SSH na Linux virtuálního počítače neexistují další bezpečnostní osvědčené postupy, které můžete provést.</span><span class="sxs-lookup"><span data-stu-id="6c906-171">Now that you have configured and locked down the SSH server on your Linux VM there are additional security best practices you can follow.</span></span>  

* [<span data-ttu-id="6c906-172">Spravovat uživatele, SSH a zkontrolujte nebo opravte disky na virtuálních počítačích Azure Linux pomocí rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="6c906-172">Manage users, SSH, and check or repair disks on Azure Linux VMs using the VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [<span data-ttu-id="6c906-173">Šifrování disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="6c906-173">Encrypt disks on a Linux VM using the Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [<span data-ttu-id="6c906-174">Přístup a zabezpečení v šablonách Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6c906-174">Access and security in Azure Resource Manager templates</span></span>](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
