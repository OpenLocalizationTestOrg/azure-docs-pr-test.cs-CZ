---
title: aaaIntroduction tooFreeBSD v Azure | Microsoft Docs
description: "Další informace o použití FreeBSD virtuální počítače v Azure"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 32b87a5f-d024-4da0-8bf0-77e233d1422b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: kyliel
ms.openlocfilehash: eab7aeda7f7ef893740b39c0250aacc29d6fd71b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toofreebsd-on-azure"></a><span data-ttu-id="06ff8-103">Úvod tooFreeBSD v Azure</span><span class="sxs-lookup"><span data-stu-id="06ff8-103">Introduction tooFreeBSD on Azure</span></span>
<span data-ttu-id="06ff8-104">Toto téma obsahuje přehled spuštěným virtuálním počítačem FreeBSD v Azure.</span><span class="sxs-lookup"><span data-stu-id="06ff8-104">This topic provides an overview of running a FreeBSD virtual machine in Azure.</span></span>

## <a name="overview"></a><span data-ttu-id="06ff8-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="06ff8-105">Overview</span></span>
<span data-ttu-id="06ff8-106">FreeBSD pro Microsoft Azure je, že že pokročilým operační systém používal toopower moderní serverů, stolních počítačů a embedded platformy.</span><span class="sxs-lookup"><span data-stu-id="06ff8-106">FreeBSD for Microsoft Azure is an advanced computer operating system used toopower modern servers, desktops, and embedded platforms.</span></span>

<span data-ttu-id="06ff8-107">Microsoft Corporation je zpřístupnění bitové kopie FreeBSD v Azure s hello [agenta hosta virtuálního počítače Azure](https://github.com/Azure/WALinuxAgent/) předem nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="06ff8-107">Microsoft Corporation is making images of FreeBSD available on Azure with hello [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) pre-configured.</span></span> <span data-ttu-id="06ff8-108">V současné době hello následující verze FreeBSD jsou nabízeny jako obrázky společností Microsoft:</span><span class="sxs-lookup"><span data-stu-id="06ff8-108">Currently, hello following FreeBSD versions are offered as images by Microsoft:</span></span>

- <span data-ttu-id="06ff8-109">10.3 uvolnění FreeBSD</span><span class="sxs-lookup"><span data-stu-id="06ff8-109">FreeBSD 10.3-RELEASE</span></span>
- <span data-ttu-id="06ff8-110">FreeBSD 11.0 – verze</span><span class="sxs-lookup"><span data-stu-id="06ff8-110">FreeBSD 11.0-RELEASE</span></span>

<span data-ttu-id="06ff8-111">Hello agent zodpovídá za komunikaci mezi hello FreeBSD virtuálních počítačů a hello prostředků infrastruktury Azure pro operace, jako je například zřizování hello virtuálního počítače na první použití (uživatelské jméno, heslo nebo klíč SSH, název hostitele, atd.) a povolení funkce pro selektivní rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="06ff8-111">hello agent is responsible for communication between hello FreeBSD VM and hello Azure fabric for operations such as provisioning hello VM on first use (user name, password or SSH key, host name, etc.) and enabling functionality for selective VM extensions.</span></span>

<span data-ttu-id="06ff8-112">Jako u budoucích verzích FreeBSD strategie hello je toostay aktuální a zpřístupnit hello nejnovější verze krátce po jsou publikovány nástrojem hello FreeBSD verze technickému týmu.</span><span class="sxs-lookup"><span data-stu-id="06ff8-112">As for future versions of FreeBSD, hello strategy is toostay current and make hello latest releases available shortly after they are published by hello FreeBSD release engineering team.</span></span>

## <a name="deploying-a-freebsd-virtual-machine"></a><span data-ttu-id="06ff8-113">Nasazení virtuálního počítače FreeBSD</span><span class="sxs-lookup"><span data-stu-id="06ff8-113">Deploying a FreeBSD virtual machine</span></span>
<span data-ttu-id="06ff8-114">Nasazení virtuálního počítače FreeBSD je jednoduchý proces pomocí bitové kopie z Azure Marketplace hello z hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="06ff8-114">Deploying a FreeBSD virtual machine is a straightforward process using an image from hello Azure Marketplace from hello Azure portal:</span></span>

- [<span data-ttu-id="06ff8-115">10.3 FreeBSD v hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="06ff8-115">FreeBSD 10.3 on hello Azure Marketplace</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
- [<span data-ttu-id="06ff8-116">FreeBSD 11.0 na hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="06ff8-116">FreeBSD 11.0 on hello Azure Marketplace</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/)

### <a name="create-a-freebsd-vm-through-azure-cli-20-on-freebsd"></a><span data-ttu-id="06ff8-117">Vytvoření virtuálního počítače FreeBSD prostřednictvím rozhraní příkazového řádku Azure 2.0 na FreeBSD</span><span class="sxs-lookup"><span data-stu-id="06ff8-117">Create a FreeBSD VM through Azure CLI 2.0 on FreeBSD</span></span>
<span data-ttu-id="06ff8-118">Je třeba nejprve tooinstall [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) i když následující příkaz na počítači FreeBSD.</span><span class="sxs-lookup"><span data-stu-id="06ff8-118">First you need tooinstall [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) though following command on a FreeBSD machine.</span></span>

```bash 
    curl -L https://aka.ms/InstallAzureCli | bash
```

<span data-ttu-id="06ff8-119">Pokud bash není nainstalovaný na počítači FreeBSD, spusťte následující příkaz před instalací hello.</span><span class="sxs-lookup"><span data-stu-id="06ff8-119">If bash is not installed on your FreeBSD machine, run following command before hello installation.</span></span> 

```
    sudo pkg install bash
```

<span data-ttu-id="06ff8-120">Pokud python není nainstalovaný na počítači FreeBSD, spusťte následující příkazy před instalací hello.</span><span class="sxs-lookup"><span data-stu-id="06ff8-120">If python is not installed on your FreeBSD machine, run following commands before hello installation.</span></span> 

```
    sudo pkg install python35
    cd /usr/local/bin 
    sudo rm /usr/local/bin/python 
    sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

<span data-ttu-id="06ff8-121">Během instalace hello se zobrazí výzva `Modify profile tooupdate your $PATH and enable shell/tab completion now? (Y/n)`.</span><span class="sxs-lookup"><span data-stu-id="06ff8-121">During hello installation, you are asked `Modify profile tooupdate your $PATH and enable shell/tab completion now? (Y/n)`.</span></span> <span data-ttu-id="06ff8-122">Pokud odpovíte `y` a zadejte `/etc/rc.conf` jako `a path tooan rc file tooupdate`, splňujete hello problém `ERROR: [Errno 13] Permission denied`.</span><span class="sxs-lookup"><span data-stu-id="06ff8-122">If you answer `y` and enter `/etc/rc.conf` as `a path tooan rc file tooupdate`, you may meet hello problem `ERROR: [Errno 13] Permission denied`.</span></span> <span data-ttu-id="06ff8-123">tooresolve tento problém byste měli udělit hello zápisu správné toocurrent uživatele vůči hello souboru `etc/rc.conf`.</span><span class="sxs-lookup"><span data-stu-id="06ff8-123">tooresolve this problem, you should grant hello write right toocurrent user against hello file `etc/rc.conf`.</span></span>

<span data-ttu-id="06ff8-124">Nyní můžete přihlásit Azure a vytvořit FreeBSD virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="06ff8-124">Now you can log in Azure and create your FreeBSD VM.</span></span> <span data-ttu-id="06ff8-125">Níže je toocreate příklad FreeBSD 11.0 virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="06ff8-125">Below is an example toocreate a FreeBSD 11.0 VM.</span></span> <span data-ttu-id="06ff8-126">Můžete také přidat parametr hello `--public-ip-address-dns-name` s globálně jedinečného názvu DNS pro nově vytvořený veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="06ff8-126">You can also add hello parameter `--public-ip-address-dns-name` with a globally unique DNS name for a newly created Public IP.</span></span> 

```azurecli
    az login 
    az group create -n myResourceGroup -l westus az vm create -n myFreeBSD11 -g myResourceGroup --image MicrosoftOSTC:FreeBSD:11.0:latest --admin-username azureuser --ssh-key-value /etc/ssh/ssh_host_rsa_key.pub 
```

<span data-ttu-id="06ff8-127">Potom se můžete přihlásit tooyour FreeBSD virtuálních počítačů prostřednictvím hello ip adresu, která vytištěn v výstup hello výše nasazení.</span><span class="sxs-lookup"><span data-stu-id="06ff8-127">Then you can log in tooyour FreeBSD VM through hello ip address that printed in hello output of above deployment.</span></span> 

```bash
    ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a><span data-ttu-id="06ff8-128">Rozšíření virtuálního počítače pro FreeBSD</span><span class="sxs-lookup"><span data-stu-id="06ff8-128">VM extensions for FreeBSD</span></span>
<span data-ttu-id="06ff8-129">Toto jsou podporované rozšíření virtuálního počítače v FreeBSD.</span><span class="sxs-lookup"><span data-stu-id="06ff8-129">Following are supported VM extensions in FreeBSD.</span></span>

### <a name="vmaccess"></a><span data-ttu-id="06ff8-130">VMAccess</span><span class="sxs-lookup"><span data-stu-id="06ff8-130">VMAccess</span></span>
<span data-ttu-id="06ff8-131">Hello [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) rozšíření můžete:</span><span class="sxs-lookup"><span data-stu-id="06ff8-131">hello [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) extension can:</span></span>

* <span data-ttu-id="06ff8-132">Resetovat heslo hello hello původní sudo uživatele.</span><span class="sxs-lookup"><span data-stu-id="06ff8-132">Reset hello password of hello original sudo user.</span></span>
* <span data-ttu-id="06ff8-133">Vytvořte nového uživatele sudo s zadané heslo hello.</span><span class="sxs-lookup"><span data-stu-id="06ff8-133">Create a new sudo user with hello password specified.</span></span>
* <span data-ttu-id="06ff8-134">Nastavit klíč veřejný hostitele hello klíčem hello zadána.</span><span class="sxs-lookup"><span data-stu-id="06ff8-134">Set hello public host key with hello key given.</span></span>
* <span data-ttu-id="06ff8-135">Resetujte klíč veřejný hostitele hello zadané během zřizování, pokud není k dispozici klíč hello hostitele virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="06ff8-135">Reset hello public host key provided during VM provisioning if hello host key is not provided.</span></span>
* <span data-ttu-id="06ff8-136">Otevřete port SSH hello (22) a obnovit hello sshd_config, pokud reset_ssh nastavena tootrue.</span><span class="sxs-lookup"><span data-stu-id="06ff8-136">Open hello SSH port (22) and restore hello sshd_config if reset_ssh is set tootrue.</span></span>
* <span data-ttu-id="06ff8-137">Odeberte hello stávajícího uživatele.</span><span class="sxs-lookup"><span data-stu-id="06ff8-137">Remove hello existing user.</span></span>
* <span data-ttu-id="06ff8-138">Zkontrolujte disky.</span><span class="sxs-lookup"><span data-stu-id="06ff8-138">Check disks.</span></span>
* <span data-ttu-id="06ff8-139">Přidání disku opravte.</span><span class="sxs-lookup"><span data-stu-id="06ff8-139">Repair an added disk.</span></span>

### <a name="customscript"></a><span data-ttu-id="06ff8-140">CustomScript</span><span class="sxs-lookup"><span data-stu-id="06ff8-140">CustomScript</span></span>
<span data-ttu-id="06ff8-141">Hello [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) rozšíření můžete:</span><span class="sxs-lookup"><span data-stu-id="06ff8-141">hello [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) extension can:</span></span>

* <span data-ttu-id="06ff8-142">Pokud je zadán, stáhněte hello přizpůsobit skripty z Azure Storage nebo veřejného externího úložiště (například Githubu).</span><span class="sxs-lookup"><span data-stu-id="06ff8-142">If provided, download hello customized scripts from Azure Storage or external public storage (for example, GitHub).</span></span>
* <span data-ttu-id="06ff8-143">Hello vstupní bod skript spusťte.</span><span class="sxs-lookup"><span data-stu-id="06ff8-143">Run hello entry point script.</span></span>
* <span data-ttu-id="06ff8-144">Vnořené příkazy podpory.</span><span class="sxs-lookup"><span data-stu-id="06ff8-144">Support inline commands.</span></span>
* <span data-ttu-id="06ff8-145">Automaticky převeďte nového řádku styl systému Windows v prostředí a skriptů Python.</span><span class="sxs-lookup"><span data-stu-id="06ff8-145">Convert Windows-style newline in shell and Python scripts automatically.</span></span>
* <span data-ttu-id="06ff8-146">Automaticky odeberte BOM v prostředí a skriptů Python.</span><span class="sxs-lookup"><span data-stu-id="06ff8-146">Remove BOM in shell and Python scripts automatically.</span></span>
* <span data-ttu-id="06ff8-147">Chrání citlivá data v CommandToExecute.</span><span class="sxs-lookup"><span data-stu-id="06ff8-147">Protect sensitive data in CommandToExecute.</span></span>

> [!NOTE]
> <span data-ttu-id="06ff8-148">Virtuální počítač FreeBSD podporuje pouze verzi CustomScript 1.x nyní.</span><span class="sxs-lookup"><span data-stu-id="06ff8-148">FreeBSD VM only supports CustomScript version 1.x by now.</span></span>  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a><span data-ttu-id="06ff8-149">Ověřování: uživatelská jména, hesla a klíče SSH</span><span class="sxs-lookup"><span data-stu-id="06ff8-149">Authentication: user names, passwords, and SSH keys</span></span>
<span data-ttu-id="06ff8-150">Při vytváření virtuálního počítače FreeBSD pomocí hello portálu Azure, je nutné zadat uživatelské jméno, heslo nebo veřejný klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="06ff8-150">When you're creating a FreeBSD virtual machine by using hello Azure portal, you must provide a user name, password, or SSH public key.</span></span>
<span data-ttu-id="06ff8-151">Uživatelská jména pro nasazení virtuálního počítače FreeBSD v Azure nesmí shodovat s názvy účtů systému (UID < 100) již existuje ve virtuálním počítači hello ("root", např.).</span><span class="sxs-lookup"><span data-stu-id="06ff8-151">User names for deploying a FreeBSD virtual machine on Azure must not match names of system accounts (UID <100) already present in hello virtual machine ("root", for example).</span></span>
<span data-ttu-id="06ff8-152">V současné době je podporován pouze hello RSA klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="06ff8-152">Currently, only hello RSA SSH key is supported.</span></span> <span data-ttu-id="06ff8-153">Víceřádkový klíč SSH musí začínat řetězcem `---- BEGIN SSH2 PUBLIC KEY ----` a končit `---- END SSH2 PUBLIC KEY ----`.</span><span class="sxs-lookup"><span data-stu-id="06ff8-153">A multiline SSH key must begin with `---- BEGIN SSH2 PUBLIC KEY ----` and end with `---- END SSH2 PUBLIC KEY ----`.</span></span>

## <a name="obtaining-superuser-privileges"></a><span data-ttu-id="06ff8-154">Získání oprávnění superuživatele</span><span class="sxs-lookup"><span data-stu-id="06ff8-154">Obtaining superuser privileges</span></span>
<span data-ttu-id="06ff8-155">Hello uživatelský účet, který je zadán během nasazení instance virtuálního počítače na platformě Azure je privilegovaný účet.</span><span class="sxs-lookup"><span data-stu-id="06ff8-155">hello user account that is specified during virtual machine instance deployment on Azure is a privileged account.</span></span> <span data-ttu-id="06ff8-156">Hello balíček sudo byl nainstalován do hello publikovaná FreeBSD image.</span><span class="sxs-lookup"><span data-stu-id="06ff8-156">hello package of sudo was installed in hello published FreeBSD image.</span></span>
<span data-ttu-id="06ff8-157">Poté, co jste přihlášeni prostřednictvím tento uživatelský účet, můžete spustit příkazy jako kořenová pomocí syntaxe příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="06ff8-157">After you're logged in through this user account, you can run commands as root by using hello command syntax.</span></span>

```
    $ sudo <COMMAND>
```

<span data-ttu-id="06ff8-158">Kořenové prostředí můžete volitelně můžete získat pomocí `sudo -s`.</span><span class="sxs-lookup"><span data-stu-id="06ff8-158">You can optionally obtain a root shell by using `sudo -s`.</span></span>

## <a name="known-issues"></a><span data-ttu-id="06ff8-159">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="06ff8-159">Known issues</span></span>
<span data-ttu-id="06ff8-160">Hello [agenta hosta virtuálního počítače Azure](https://github.com/Azure/WALinuxAgent/) verze 2.2.2 má [známý problém] (https://github.com/Azure/WALinuxAgent/pull/517), která způsobí selhání hello přidělení pro virtuální počítač FreeBSD v Azure.</span><span class="sxs-lookup"><span data-stu-id="06ff8-160">hello [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) version 2.2.2 has a [known issue] (https://github.com/Azure/WALinuxAgent/pull/517) that causes hello provision failure for FreeBSD VM on Azure.</span></span> <span data-ttu-id="06ff8-161">Hello oprava zaznamenaná [agenta hosta virtuálního počítače Azure](https://github.com/Azure/WALinuxAgent/) verze 2.2.3 a pozdějších verzích.</span><span class="sxs-lookup"><span data-stu-id="06ff8-161">hello fix was captured by [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) version 2.2.3 and later releases.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="06ff8-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="06ff8-162">Next steps</span></span>
* <span data-ttu-id="06ff8-163">Přejděte příliš[Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) toocreate FreeBSD virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="06ff8-163">Go too[Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) toocreate a FreeBSD VM.</span></span>
* <span data-ttu-id="06ff8-164">Pokud chcete vlastní FreeBSD tooAzure toobring, podívejte se příliš[vytvoření a nahrání virtuálního pevného disku FreeBSD tooAzure](linux/classic/freebsd-create-upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="06ff8-164">If you want toobring your own FreeBSD tooAzure, refer too[Create and upload a FreeBSD VHD tooAzure](linux/classic/freebsd-create-upload-vhd.md).</span></span>
