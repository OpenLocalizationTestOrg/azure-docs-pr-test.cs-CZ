---
title: "Úvod do FreeBSD v Azure | Microsoft Docs"
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
ms.openlocfilehash: d0fc5de34f7d9e5a607495eb97d9e35dc9eb21f9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-freebsd-on-azure"></a><span data-ttu-id="d49e3-103">Úvod do FreeBSD v Azure</span><span class="sxs-lookup"><span data-stu-id="d49e3-103">Introduction to FreeBSD on Azure</span></span>
<span data-ttu-id="d49e3-104">Toto téma obsahuje přehled spuštěným virtuálním počítačem FreeBSD v Azure.</span><span class="sxs-lookup"><span data-stu-id="d49e3-104">This topic provides an overview of running a FreeBSD virtual machine in Azure.</span></span>

## <a name="overview"></a><span data-ttu-id="d49e3-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="d49e3-105">Overview</span></span>
<span data-ttu-id="d49e3-106">FreeBSD pro Microsoft Azure je operační systém pokročilé počítače použít k power moderní serverů, stolních počítačů a vložených platformy.</span><span class="sxs-lookup"><span data-stu-id="d49e3-106">FreeBSD for Microsoft Azure is an advanced computer operating system used to power modern servers, desktops, and embedded platforms.</span></span>

<span data-ttu-id="d49e3-107">Microsoft Corporation je zpřístupnění bitové kopie FreeBSD v Azure pomocí [agenta hosta virtuálního počítače Azure](https://github.com/Azure/WALinuxAgent/) předem nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="d49e3-107">Microsoft Corporation is making images of FreeBSD available on Azure with the [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) pre-configured.</span></span> <span data-ttu-id="d49e3-108">V současné době jsou následující verze FreeBSD nabízí jako obrázky společností Microsoft:</span><span class="sxs-lookup"><span data-stu-id="d49e3-108">Currently, the following FreeBSD versions are offered as images by Microsoft:</span></span>

- <span data-ttu-id="d49e3-109">10.3 uvolnění FreeBSD</span><span class="sxs-lookup"><span data-stu-id="d49e3-109">FreeBSD 10.3-RELEASE</span></span>
- <span data-ttu-id="d49e3-110">FreeBSD 11.0 – verze</span><span class="sxs-lookup"><span data-stu-id="d49e3-110">FreeBSD 11.0-RELEASE</span></span>

<span data-ttu-id="d49e3-111">Agent je zodpovědná za komunikaci mezi FreeBSD virtuálních počítačů a prostředků infrastruktury Azure pro operace, jako je například zřizování virtuálních počítačů při prvním použití (uživatelské jméno, heslo nebo klíč SSH, název hostitele, atd.) a povolení funkce pro selektivní rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d49e3-111">The agent is responsible for communication between the FreeBSD VM and the Azure fabric for operations such as provisioning the VM on first use (user name, password or SSH key, host name, etc.) and enabling functionality for selective VM extensions.</span></span>

<span data-ttu-id="d49e3-112">Jako u budoucích verzích FreeBSD strategie je Udržujte aktuální stav a zpřístupní nejnovější verze krátce po jsou publikovány nástrojem FreeBSD verze technickému týmu.</span><span class="sxs-lookup"><span data-stu-id="d49e3-112">As for future versions of FreeBSD, the strategy is to stay current and make the latest releases available shortly after they are published by the FreeBSD release engineering team.</span></span>

## <a name="deploying-a-freebsd-virtual-machine"></a><span data-ttu-id="d49e3-113">Nasazení virtuálního počítače FreeBSD</span><span class="sxs-lookup"><span data-stu-id="d49e3-113">Deploying a FreeBSD virtual machine</span></span>
<span data-ttu-id="d49e3-114">Nasazení virtuálního počítače FreeBSD je jednoduchý proces pomocí bitovou kopii z Azure Marketplace z portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="d49e3-114">Deploying a FreeBSD virtual machine is a straightforward process using an image from the Azure Marketplace from the Azure portal:</span></span>

- [<span data-ttu-id="d49e3-115">FreeBSD 10.3 v Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="d49e3-115">FreeBSD 10.3 on the Azure Marketplace</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
- [<span data-ttu-id="d49e3-116">FreeBSD 11.0 v Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="d49e3-116">FreeBSD 11.0 on the Azure Marketplace</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/)

### <a name="create-a-freebsd-vm-through-azure-cli-20-on-freebsd"></a><span data-ttu-id="d49e3-117">Vytvoření virtuálního počítače FreeBSD prostřednictvím rozhraní příkazového řádku Azure 2.0 na FreeBSD</span><span class="sxs-lookup"><span data-stu-id="d49e3-117">Create a FreeBSD VM through Azure CLI 2.0 on FreeBSD</span></span>
<span data-ttu-id="d49e3-118">Nejdřív je potřeba nainstalovat [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) i když následující příkaz na počítači FreeBSD.</span><span class="sxs-lookup"><span data-stu-id="d49e3-118">First you need to install [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) though following command on a FreeBSD machine.</span></span>

```bash 
    curl -L https://aka.ms/InstallAzureCli | bash
```

<span data-ttu-id="d49e3-119">Pokud bash není nainstalovaný na počítači FreeBSD, spusťte následující příkaz před instalací.</span><span class="sxs-lookup"><span data-stu-id="d49e3-119">If bash is not installed on your FreeBSD machine, run following command before the installation.</span></span> 

```
    sudo pkg install bash
```

<span data-ttu-id="d49e3-120">Pokud python není nainstalovaný na počítači FreeBSD, spusťte následující příkazy před instalací.</span><span class="sxs-lookup"><span data-stu-id="d49e3-120">If python is not installed on your FreeBSD machine, run following commands before the installation.</span></span> 

```
    sudo pkg install python35
    cd /usr/local/bin 
    sudo rm /usr/local/bin/python 
    sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

<span data-ttu-id="d49e3-121">Během instalace se zobrazí výzva `Modify profile to update your $PATH and enable shell/tab completion now? (Y/n)`.</span><span class="sxs-lookup"><span data-stu-id="d49e3-121">During the installation, you are asked `Modify profile to update your $PATH and enable shell/tab completion now? (Y/n)`.</span></span> <span data-ttu-id="d49e3-122">Pokud odpovíte `y` a zadejte `/etc/rc.conf` jako `a path to an rc file to update`, splňujete problém `ERROR: [Errno 13] Permission denied`.</span><span class="sxs-lookup"><span data-stu-id="d49e3-122">If you answer `y` and enter `/etc/rc.conf` as `a path to an rc file to update`, you may meet the problem `ERROR: [Errno 13] Permission denied`.</span></span> <span data-ttu-id="d49e3-123">Chcete-li vyřešit tento problém, byste měli udělit zápis přímo na aktuální uživatel proti souboru `etc/rc.conf`.</span><span class="sxs-lookup"><span data-stu-id="d49e3-123">To resolve this problem, you should grant the write right to current user against the file `etc/rc.conf`.</span></span>

<span data-ttu-id="d49e3-124">Nyní můžete přihlásit Azure a vytvořit FreeBSD virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d49e3-124">Now you can log in Azure and create your FreeBSD VM.</span></span> <span data-ttu-id="d49e3-125">Dole je příklad k vytvoření virtuálního počítače s FreeBSD 11.0.</span><span class="sxs-lookup"><span data-stu-id="d49e3-125">Below is an example to create a FreeBSD 11.0 VM.</span></span> <span data-ttu-id="d49e3-126">Můžete také přidat parametr `--public-ip-address-dns-name` s globálně jedinečného názvu DNS pro nově vytvořený veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d49e3-126">You can also add the parameter `--public-ip-address-dns-name` with a globally unique DNS name for a newly created Public IP.</span></span> 

```azurecli
    az login 
    az group create -n myResourceGroup -l westus az vm create -n myFreeBSD11 -g myResourceGroup --image MicrosoftOSTC:FreeBSD:11.0:latest --admin-username azureuser --ssh-key-value /etc/ssh/ssh_host_rsa_key.pub 
```

<span data-ttu-id="d49e3-127">Potom můžete přihlásit k virtuálnímu počítači FreeBSD prostřednictvím ip adresy, které vytisknout ve výstupu výše nasazení.</span><span class="sxs-lookup"><span data-stu-id="d49e3-127">Then you can log in to your FreeBSD VM through the ip address that printed in the output of above deployment.</span></span> 

```bash
    ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a><span data-ttu-id="d49e3-128">Rozšíření virtuálního počítače pro FreeBSD</span><span class="sxs-lookup"><span data-stu-id="d49e3-128">VM extensions for FreeBSD</span></span>
<span data-ttu-id="d49e3-129">Toto jsou podporované rozšíření virtuálního počítače v FreeBSD.</span><span class="sxs-lookup"><span data-stu-id="d49e3-129">Following are supported VM extensions in FreeBSD.</span></span>

### <a name="vmaccess"></a><span data-ttu-id="d49e3-130">VMAccess</span><span class="sxs-lookup"><span data-stu-id="d49e3-130">VMAccess</span></span>
<span data-ttu-id="d49e3-131">[VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) rozšíření můžete:</span><span class="sxs-lookup"><span data-stu-id="d49e3-131">The [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) extension can:</span></span>

* <span data-ttu-id="d49e3-132">Resetování hesla původního uživatele sudo.</span><span class="sxs-lookup"><span data-stu-id="d49e3-132">Reset the password of the original sudo user.</span></span>
* <span data-ttu-id="d49e3-133">Vytvořte nového uživatele sudo s zadané heslo.</span><span class="sxs-lookup"><span data-stu-id="d49e3-133">Create a new sudo user with the password specified.</span></span>
* <span data-ttu-id="d49e3-134">Nastavte klíč veřejný hostitele s zadaný klíč.</span><span class="sxs-lookup"><span data-stu-id="d49e3-134">Set the public host key with the key given.</span></span>
* <span data-ttu-id="d49e3-135">Resetujte veřejný hostitele klíči poskytovaném při zřizování virtuálních počítačů, pokud klíč hostitele není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d49e3-135">Reset the public host key provided during VM provisioning if the host key is not provided.</span></span>
* <span data-ttu-id="d49e3-136">Otevřít port SSH (22) a obnovení sshd_config Pokud reset_ssh nastavena na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="d49e3-136">Open the SSH port (22) and restore the sshd_config if reset_ssh is set to true.</span></span>
* <span data-ttu-id="d49e3-137">Odeberte stávající uživatele.</span><span class="sxs-lookup"><span data-stu-id="d49e3-137">Remove the existing user.</span></span>
* <span data-ttu-id="d49e3-138">Zkontrolujte disky.</span><span class="sxs-lookup"><span data-stu-id="d49e3-138">Check disks.</span></span>
* <span data-ttu-id="d49e3-139">Přidání disku opravte.</span><span class="sxs-lookup"><span data-stu-id="d49e3-139">Repair an added disk.</span></span>

### <a name="customscript"></a><span data-ttu-id="d49e3-140">CustomScript</span><span class="sxs-lookup"><span data-stu-id="d49e3-140">CustomScript</span></span>
<span data-ttu-id="d49e3-141">[CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) rozšíření můžete:</span><span class="sxs-lookup"><span data-stu-id="d49e3-141">The [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) extension can:</span></span>

* <span data-ttu-id="d49e3-142">Pokud je zadán, stahovat vlastní skripty z Azure Storage nebo veřejného externího úložiště (například Githubu).</span><span class="sxs-lookup"><span data-stu-id="d49e3-142">If provided, download the customized scripts from Azure Storage or external public storage (for example, GitHub).</span></span>
* <span data-ttu-id="d49e3-143">Spusťte skript vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="d49e3-143">Run the entry point script.</span></span>
* <span data-ttu-id="d49e3-144">Vnořené příkazy podpory.</span><span class="sxs-lookup"><span data-stu-id="d49e3-144">Support inline commands.</span></span>
* <span data-ttu-id="d49e3-145">Automaticky převeďte nového řádku styl systému Windows v prostředí a skriptů Python.</span><span class="sxs-lookup"><span data-stu-id="d49e3-145">Convert Windows-style newline in shell and Python scripts automatically.</span></span>
* <span data-ttu-id="d49e3-146">Automaticky odeberte BOM v prostředí a skriptů Python.</span><span class="sxs-lookup"><span data-stu-id="d49e3-146">Remove BOM in shell and Python scripts automatically.</span></span>
* <span data-ttu-id="d49e3-147">Chrání citlivá data v CommandToExecute.</span><span class="sxs-lookup"><span data-stu-id="d49e3-147">Protect sensitive data in CommandToExecute.</span></span>

> [!NOTE]
> <span data-ttu-id="d49e3-148">Virtuální počítač FreeBSD podporuje pouze verzi CustomScript 1.x nyní.</span><span class="sxs-lookup"><span data-stu-id="d49e3-148">FreeBSD VM only supports CustomScript version 1.x by now.</span></span>  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a><span data-ttu-id="d49e3-149">Ověřování: uživatelská jména, hesla a klíče SSH</span><span class="sxs-lookup"><span data-stu-id="d49e3-149">Authentication: user names, passwords, and SSH keys</span></span>
<span data-ttu-id="d49e3-150">Při vytváření virtuálního počítače FreeBSD pomocí portálu Azure, je nutné zadat uživatelské jméno, heslo nebo veřejný klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="d49e3-150">When you're creating a FreeBSD virtual machine by using the Azure portal, you must provide a user name, password, or SSH public key.</span></span>
<span data-ttu-id="d49e3-151">Uživatelská jména pro nasazení virtuálního počítače FreeBSD v Azure nesmí shodovat s názvy účtů systému (UID < 100) již existuje ve virtuálním počítači ("root", např.).</span><span class="sxs-lookup"><span data-stu-id="d49e3-151">User names for deploying a FreeBSD virtual machine on Azure must not match names of system accounts (UID <100) already present in the virtual machine ("root", for example).</span></span>
<span data-ttu-id="d49e3-152">V současné době je podporován pouze se RSA klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="d49e3-152">Currently, only the RSA SSH key is supported.</span></span> <span data-ttu-id="d49e3-153">Víceřádkový klíč SSH musí začínat řetězcem `---- BEGIN SSH2 PUBLIC KEY ----` a končit `---- END SSH2 PUBLIC KEY ----`.</span><span class="sxs-lookup"><span data-stu-id="d49e3-153">A multiline SSH key must begin with `---- BEGIN SSH2 PUBLIC KEY ----` and end with `---- END SSH2 PUBLIC KEY ----`.</span></span>

## <a name="obtaining-superuser-privileges"></a><span data-ttu-id="d49e3-154">Získání oprávnění superuživatele</span><span class="sxs-lookup"><span data-stu-id="d49e3-154">Obtaining superuser privileges</span></span>
<span data-ttu-id="d49e3-155">Uživatelský účet, který je zadán během nasazení instance virtuálních počítačů v Azure je privilegovaný účet.</span><span class="sxs-lookup"><span data-stu-id="d49e3-155">The user account that is specified during virtual machine instance deployment on Azure is a privileged account.</span></span> <span data-ttu-id="d49e3-156">V publikované image FreeBSD byl nainstalován balíček sudo.</span><span class="sxs-lookup"><span data-stu-id="d49e3-156">The package of sudo was installed in the published FreeBSD image.</span></span>
<span data-ttu-id="d49e3-157">Poté, co jste přihlášeni prostřednictvím tento uživatelský účet, můžete spustit příkazy jako kořenová pomocí syntaxe příkazu.</span><span class="sxs-lookup"><span data-stu-id="d49e3-157">After you're logged in through this user account, you can run commands as root by using the command syntax.</span></span>

```
    $ sudo <COMMAND>
```

<span data-ttu-id="d49e3-158">Kořenové prostředí můžete volitelně můžete získat pomocí `sudo -s`.</span><span class="sxs-lookup"><span data-stu-id="d49e3-158">You can optionally obtain a root shell by using `sudo -s`.</span></span>

## <a name="known-issues"></a><span data-ttu-id="d49e3-159">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="d49e3-159">Known issues</span></span>
<span data-ttu-id="d49e3-160">[Agenta hosta virtuálního počítače Azure](https://github.com/Azure/WALinuxAgent/) verze 2.2.2 má [známý problém] (https://github.com/Azure/WALinuxAgent/pull/517), která způsobí selhání přidělení pro virtuální počítač FreeBSD v Azure.</span><span class="sxs-lookup"><span data-stu-id="d49e3-160">The [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) version 2.2.2 has a [known issue] (https://github.com/Azure/WALinuxAgent/pull/517) that causes the provision failure for FreeBSD VM on Azure.</span></span> <span data-ttu-id="d49e3-161">Oprava zaznamenaná [agenta hosta virtuálního počítače Azure](https://github.com/Azure/WALinuxAgent/) verze 2.2.3 a pozdějších verzích.</span><span class="sxs-lookup"><span data-stu-id="d49e3-161">The fix was captured by [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) version 2.2.3 and later releases.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="d49e3-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d49e3-162">Next steps</span></span>
* <span data-ttu-id="d49e3-163">Přejděte na [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) vytvoření FreeBSD virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d49e3-163">Go to [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) to create a FreeBSD VM.</span></span>
* <span data-ttu-id="d49e3-164">Pokud chcete, aby vlastní FreeBSD do Azure, podívejte se na [vytvoření a nahrání virtuálního pevného disku FreeBSD do Azure](linux/classic/freebsd-create-upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="d49e3-164">If you want to bring your own FreeBSD to Azure, refer to [Create and upload a FreeBSD VHD to Azure](linux/classic/freebsd-create-upload-vhd.md).</span></span>
