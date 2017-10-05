---
title: "Resetovat heslo pro virtuální počítač s Linuxem a klíč SSH z rozhraní příkazového řádku | Microsoft Docs"
description: "Jak používat rozšíření VMAccess z rozhraní příkazového řádku Azure (CLI) k obnovení virtuálního počítače s Linuxem heslo nebo klíč SSH, opravit konfiguraci SSH a kontrola konzistence disku"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: d975eb70-5ff1-40d1-a634-8dd2646dcd17
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: cynthn
ms.openlocfilehash: 74765877e7836d6878284b350a25d8355dc83d7d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a><span data-ttu-id="fea39-103">Jak obnovit virtuální počítač s Linuxem heslo nebo klíč SSH, opravit konfiguraci SSH a kontrola konzistence disku pomocí rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="fea39-103">How to reset a Linux VM password or SSH key, fix the SSH configuration, and check disk consistency using the VMAccess extension</span></span>
<span data-ttu-id="fea39-104">Pokud se nemůžete připojit k virtuální počítač s Linuxem v Azure z důvodu zapomenuté heslo, nesprávného klíče Secure Shell (SSH), nebo problém s konfigurací SSH, používají rozšíření VMAccessForLinux s Azure CLI resetovat heslo nebo klíč SSH, opravte SSH Konfigurace a kontrolu konzistence disku.</span><span class="sxs-lookup"><span data-stu-id="fea39-104">If you can't connect to a Linux virtual machine on Azure because of a forgotten password, an incorrect Secure Shell (SSH) key, or a problem with the SSH configuration, use the VMAccessForLinux extension with the Azure CLI to reset the password or SSH key, fix the SSH configuration, and check disk consistency.</span></span> 

> [!IMPORTANT] 
> <span data-ttu-id="fea39-105">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="fea39-105">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="fea39-106">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="fea39-106">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="fea39-107">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="fea39-107">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="fea39-108">Zjistěte, jak [provést tento postup pomocí modelu Resource Manageru](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span><span class="sxs-lookup"><span data-stu-id="fea39-108">Learn how to [perform these steps using the Resource Manager model](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span></span>

<span data-ttu-id="fea39-109">Pomocí Azure CLI, můžete použít **sadu rozšíření virtuálního počítače azure** příkaz vaše rozhraní příkazového řádku (Bash, terminálu, příkazového řádku) přístup k příkazům.</span><span class="sxs-lookup"><span data-stu-id="fea39-109">With the Azure CLI, you use the **azure vm extension set** command from your command-line interface (Bash, Terminal, Command prompt) to access commands.</span></span> <span data-ttu-id="fea39-110">Spustit **sadu rozšíření virtuálního počítače azure nápovědy** pro použití podrobné rozšíření.</span><span class="sxs-lookup"><span data-stu-id="fea39-110">Run **azure help vm extension set** for detailed extension usage.</span></span>

<span data-ttu-id="fea39-111">Pomocí rozhraní příkazového řádku Azure lze provádět následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="fea39-111">With the Azure CLI, you can do the following tasks:</span></span>

* [<span data-ttu-id="fea39-112">Resetování hesla</span><span class="sxs-lookup"><span data-stu-id="fea39-112">Reset the password</span></span>](#pwresetcli)
* [<span data-ttu-id="fea39-113">Resetovat klíč SSH</span><span class="sxs-lookup"><span data-stu-id="fea39-113">Reset the SSH key</span></span>](#sshkeyresetcli)
* [<span data-ttu-id="fea39-114">Resetovat heslo a klíč SSH</span><span class="sxs-lookup"><span data-stu-id="fea39-114">Reset the password and the SSH key</span></span>](#resetbothcli)
* [<span data-ttu-id="fea39-115">Vytvořit nový uživatelský účet sudo</span><span class="sxs-lookup"><span data-stu-id="fea39-115">Create a new sudo user account</span></span>](#createnewsudocli)
* [<span data-ttu-id="fea39-116">Resetování konfigurace SSH</span><span class="sxs-lookup"><span data-stu-id="fea39-116">Reset the SSH configuration</span></span>](#sshconfigresetcli)
* [<span data-ttu-id="fea39-117">Odstranit uživatele</span><span class="sxs-lookup"><span data-stu-id="fea39-117">Delete a user</span></span>](#deletecli)
* [<span data-ttu-id="fea39-118">Zobrazuje stav rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="fea39-118">Display the status of the VMAccess extension</span></span>](#statuscli)
* [<span data-ttu-id="fea39-119">Kontrola konzistence přidaných disků</span><span class="sxs-lookup"><span data-stu-id="fea39-119">Check consistency of added disks</span></span>](#checkdisk)
* [<span data-ttu-id="fea39-120">Opravte přidaných disků na virtuálním počítačům s Linuxem</span><span class="sxs-lookup"><span data-stu-id="fea39-120">Repair added disks on your Linux VM</span></span>](#repairdisk)

## <a name="prerequisites"></a><span data-ttu-id="fea39-121">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fea39-121">Prerequisites</span></span>
<span data-ttu-id="fea39-122">Musíte provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="fea39-122">You will need to do the following:</span></span>

* <span data-ttu-id="fea39-123">Budete muset [nainstalovat Azure CLI](../../../cli-install-nodejs.md) a [připojení k vašemu předplatnému](../../../xplat-cli-connect.md) používat prostředky Azure, které jsou spojené s vaším účtem.</span><span class="sxs-lookup"><span data-stu-id="fea39-123">You will need to [install the Azure CLI](../../../cli-install-nodejs.md) and [connect to your subscription](../../../xplat-cli-connect.md) to use Azure resources associated with your account.</span></span>
* <span data-ttu-id="fea39-124">Nastavte do správného režimu pro model nasazení classic pomocí následujícího příkazu na příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="fea39-124">Set the correct mode for the classic deployment model by typing the following at the command prompt:</span></span>
    ``` 
        azure config mode asm
    ```
* <span data-ttu-id="fea39-125">Pokud chcete obnovit některý mají nové heslo nebo sadu klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="fea39-125">Have a new password or set of SSH keys, if you want to reset either one.</span></span> <span data-ttu-id="fea39-126">Pokud chcete resetovat konfiguraci SSH tyto nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="fea39-126">You don't need these if you want to reset the SSH configuration.</span></span>

## <span data-ttu-id="fea39-127"><a name="pwresetcli"></a>Resetování hesla</span><span class="sxs-lookup"><span data-stu-id="fea39-127"><a name="pwresetcli"></a>Reset the password</span></span>
1. <span data-ttu-id="fea39-128">Vytvořte soubor do místního počítače s názvem PrivateConf.json se tyto řádky.</span><span class="sxs-lookup"><span data-stu-id="fea39-128">Create a file on your local computer named PrivateConf.json with these lines.</span></span> <span data-ttu-id="fea39-129">Nahraďte **uživatelské_jméno** a  **myP@ssW0rd**  s vlastní uživatelské jméno a heslo a nastavit vlastní datum vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="fea39-129">Replace **myUserName** and **myP@ssW0rd** with your own user name and password and set your own date for expiration.</span></span>

    ```   
        {
        "username":"myUserName",
        "password":"myP@ssW0rd",
        "expiration":"2020-01-01"
        }
    ```
        
2. <span data-ttu-id="fea39-130">Spusťte tento příkaz, nahraďte název virtuálního počítače pro **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="fea39-130">Run this command, substituting the name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json
    ```

## <span data-ttu-id="fea39-131"><a name="sshkeyresetcli"></a>Resetovat klíč SSH</span><span class="sxs-lookup"><span data-stu-id="fea39-131"><a name="sshkeyresetcli"></a>Reset the SSH key</span></span>
1. <span data-ttu-id="fea39-132">Vytvořte soubor s názvem PrivateConf.json se tyto obsah.</span><span class="sxs-lookup"><span data-stu-id="fea39-132">Create a file named PrivateConf.json with these contents.</span></span> <span data-ttu-id="fea39-133">Nahraďte **uživatelské_jméno** a **mySSHKey** hodnoty nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="fea39-133">Replace the **myUserName** and **mySSHKey** values with your own information.</span></span>

    ```   
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey"
        }
    ```
2. <span data-ttu-id="fea39-134">Spusťte tento příkaz, nahraďte název virtuálního počítače pro **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="fea39-134">Run this command, substituting the name of your virtual machine for **myVM**.</span></span>
   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <span data-ttu-id="fea39-135"><a name="resetbothcli"></a>Resetovat heslo a klíč SSH</span><span class="sxs-lookup"><span data-stu-id="fea39-135"><a name="resetbothcli"></a>Reset both the password and the SSH key</span></span>
1. <span data-ttu-id="fea39-136">Vytvořte soubor s názvem PrivateConf.json se tyto obsah.</span><span class="sxs-lookup"><span data-stu-id="fea39-136">Create a file named PrivateConf.json with these contents.</span></span> <span data-ttu-id="fea39-137">Nahraďte **uživatelské_jméno**, **mySSHKey** a  **myP@ssW0rd**  hodnoty nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="fea39-137">Replace the **myUserName**, **mySSHKey** and **myP@ssW0rd** values with your own information.</span></span>

    ``` 
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey",
        "password":"myP@ssW0rd"
        }
    ```

2. <span data-ttu-id="fea39-138">Spusťte tento příkaz, nahraďte název virtuálního počítače pro **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="fea39-138">Run this command, substituting the name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set MyVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <span data-ttu-id="fea39-139"><a name="createnewsudocli"></a>Vytvořit nový uživatelský účet sudo</span><span class="sxs-lookup"><span data-stu-id="fea39-139"><a name="createnewsudocli"></a>Create a new sudo user account</span></span>

<span data-ttu-id="fea39-140">Pokud jste zapomněli svoje uživatelské jméno, můžete vytvořit nový s autoritou sudo VMAccess.</span><span class="sxs-lookup"><span data-stu-id="fea39-140">If you forget your user name, you can use VMAccess to create a new one with the sudo authority.</span></span> <span data-ttu-id="fea39-141">V takovém případě existující uživatelské jméno a heslo se nezmění.</span><span class="sxs-lookup"><span data-stu-id="fea39-141">In this case, the existing user name and password will not be modified.</span></span>

<span data-ttu-id="fea39-142">Pokud chcete vytvořit nového uživatele sudo s přístupem heslo, použijte skript v [resetování hesla](#pwresetcli) a zadejte nové uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="fea39-142">To create a new sudo user with password access, use the script in [Reset the password](#pwresetcli) and specify the new user name.</span></span>

<span data-ttu-id="fea39-143">Pokud chcete vytvořit nového uživatele sudo s přístupem klíče SSH, použijte skript v [resetovat klíč SSH](#sshkeyresetcli) a zadejte nové uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="fea39-143">To create a new sudo user with SSH key access, use the script in [Reset the SSH key](#sshkeyresetcli) and specify the new user name.</span></span>

<span data-ttu-id="fea39-144">Můžete také použít [resetovat heslo a klíč SSH](#resetbothcli) k vytvoření nového uživatele s heslem a přístup ke klíčům SSH.</span><span class="sxs-lookup"><span data-stu-id="fea39-144">You can also use [Reset the password and the SSH key](#resetbothcli) to create a new user with both password and SSH key access.</span></span>

## <span data-ttu-id="fea39-145"><a name="sshconfigresetcli"></a>Resetování konfigurace SSH</span><span class="sxs-lookup"><span data-stu-id="fea39-145"><a name="sshconfigresetcli"></a>Reset the SSH configuration</span></span>
<span data-ttu-id="fea39-146">Pokud konfiguraci SSH je ve stavu nežádoucí, můžete také ztratit přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="fea39-146">If the SSH configuration is in an undesired state, you might also lose access to the VM.</span></span> <span data-ttu-id="fea39-147">Můžete použít rozšíření VMAccess k resetování konfigurace do výchozího stavu.</span><span class="sxs-lookup"><span data-stu-id="fea39-147">You can use the VMAccess extension to reset the configuration to its default state.</span></span> <span data-ttu-id="fea39-148">Uděláte to tak, stačí nastavit klíč "reset_ssh" na "True".</span><span class="sxs-lookup"><span data-stu-id="fea39-148">To do so, you just need to set the “reset_ssh” key to “True”.</span></span> <span data-ttu-id="fea39-149">Rozšíření restartujte SSH server, otevřít port SSH na vašem virtuálním počítači a obnovit výchozí hodnoty v konfiguraci SSH.</span><span class="sxs-lookup"><span data-stu-id="fea39-149">The extension will restart the SSH server, open the SSH port on your VM, and reset the SSH configuration to default values.</span></span> <span data-ttu-id="fea39-150">Uživatelský účet (název, hesla nebo klíče SSH) nebudou změněny.</span><span class="sxs-lookup"><span data-stu-id="fea39-150">The user account (name, password or SSH keys) will not be changed.</span></span>

> [!NOTE]
> <span data-ttu-id="fea39-151">Konfigurační soubor SSH, který získá resetovat se nachází v /etc/ssh/sshd_config.</span><span class="sxs-lookup"><span data-stu-id="fea39-151">The SSH configuration file that gets reset is located at /etc/ssh/sshd_config.</span></span>
> 
> 

1. <span data-ttu-id="fea39-152">Vytvořte soubor s názvem PrivateConf.json tohoto obsahu.</span><span class="sxs-lookup"><span data-stu-id="fea39-152">Create a file named PrivateConf.json with this content.</span></span>

    ```   
        {
        "reset_ssh":"True"
        }
    ```

2. <span data-ttu-id="fea39-153">Spusťte tento příkaz, nahraďte název virtuálního počítače pro **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="fea39-153">Run this command, substituting the name of your virtual machine for **myVM**.</span></span> 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <span data-ttu-id="fea39-154"><a name="deletecli"></a>Odstranit uživatele</span><span class="sxs-lookup"><span data-stu-id="fea39-154"><a name="deletecli"></a>Delete a user</span></span>
<span data-ttu-id="fea39-155">Pokud chcete odstranit účet uživatele bez přihlásíte do virtuálního počítače přímo, můžete tento skript.</span><span class="sxs-lookup"><span data-stu-id="fea39-155">If you want to delete a user account without logging into to the VM directly, you can use this script.</span></span>

1. <span data-ttu-id="fea39-156">Vytvořte soubor s názvem PrivateConf.json tohoto obsahu, nahraďte uživatelské jméno pro odebrání **removeUserName**.</span><span class="sxs-lookup"><span data-stu-id="fea39-156">Create a file named PrivateConf.json with this content, substituting the user name to remove for **removeUserName**.</span></span> 

    ```   
        {
        "remove_user":"removeUserName"
        }
    ```

2. <span data-ttu-id="fea39-157">Spusťte tento příkaz, nahraďte název virtuálního počítače pro **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="fea39-157">Run this command, substituting the name of your virtual machine for **myVM**.</span></span> 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <span data-ttu-id="fea39-158"><a name="statuscli"></a>Zobrazuje stav rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="fea39-158"><a name="statuscli"></a>Display the status of the VMAccess extension</span></span>
<span data-ttu-id="fea39-159">Chcete-li zobrazit stav rozšíření VMAccess, spusťte tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="fea39-159">To display the status of the VMAccess extension, run this command.</span></span>

```
        azure vm extension get
```

## <span data-ttu-id="fea39-160"><a name='checkdisk'></a>Kontrola konzistence přidaných disků</span><span class="sxs-lookup"><span data-stu-id="fea39-160"><a name='checkdisk'></a>Check consistency of added disks</span></span>
<span data-ttu-id="fea39-161">Pokud chcete spustit fsck na všech discích ve virtuálním počítači Linux, musíte provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="fea39-161">To run fsck on all disks in your Linux virtual machine, you will need to do the following:</span></span>

1. <span data-ttu-id="fea39-162">Vytvořte soubor s názvem PublicConf.json tohoto obsahu.</span><span class="sxs-lookup"><span data-stu-id="fea39-162">Create a file named PublicConf.json with this content.</span></span> <span data-ttu-id="fea39-163">Zkontrolujte, zda že disk trvá logická hodnota označuje, jestli chcete zkontrolovat disky připojené k virtuálnímu počítači, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="fea39-163">Check Disk takes a boolean for whether to check disks attached to your virtual machine or not.</span></span> 

    ```   
        {   
        "check_disk": "true"
        }
    ```

2. <span data-ttu-id="fea39-164">Spusťte tento příkaz k provedení, nahraďte název virtuálního počítače pro **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="fea39-164">Run this command to execute, substituting the name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 
    ```

## <span data-ttu-id="fea39-165"><a name='repairdisk'></a>Oprava disky</span><span class="sxs-lookup"><span data-stu-id="fea39-165"><a name='repairdisk'></a>Repair disks</span></span>
<span data-ttu-id="fea39-166">K opravě disky, které nejsou připojení nebo připojení chyby konfigurace, resetujte konfiguraci připojení na virtuální počítač Linux pomocí rozšíření VMAccess.</span><span class="sxs-lookup"><span data-stu-id="fea39-166">To repair disks that are not mounting or have mount configuration errors, use the VMAccess extension to reset the mount configuration on your Linux virtual machine.</span></span> <span data-ttu-id="fea39-167">Nahraďte název disku pro **myDisk**.</span><span class="sxs-lookup"><span data-stu-id="fea39-167">Substituting the name of your disk for **myDisk**.</span></span>

1. <span data-ttu-id="fea39-168">Vytvořte soubor s názvem PublicConf.json tohoto obsahu.</span><span class="sxs-lookup"><span data-stu-id="fea39-168">Create a file named PublicConf.json with this content.</span></span> 

    ```   
        {
        "repair_disk":"true",
        "disk_name":"myDisk"
        }
    ```

2. <span data-ttu-id="fea39-169">Spusťte tento příkaz k provedení, nahraďte název virtuálního počítače pro **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="fea39-169">Run this command to execute, substituting the name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json
    ```

## <a name="next-steps"></a><span data-ttu-id="fea39-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fea39-170">Next steps</span></span>
* <span data-ttu-id="fea39-171">Pokud chcete resetovat heslo nebo klíč SSH pomocí rutin prostředí Azure PowerShell nebo šablon Azure Resource Manageru, opravte SSH konfigurace a kontrolu konzistence disku, najdete [VMAccess rozšíření dokumentaci na Githubu](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span><span class="sxs-lookup"><span data-stu-id="fea39-171">If you want to use Azure PowerShell cmdlets or Azure Resource Manager templates to reset the password or SSH key, fix the SSH configuration, and check disk consistency, see the [VMAccess extension documentation on GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span></span> 
* <span data-ttu-id="fea39-172">Můžete také [portál Azure](https://portal.azure.com) resetovat heslo nebo klíč SSH virtuálních počítačů Linux nasazené v modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="fea39-172">You can also use the [Azure portal](https://portal.azure.com) to reset the password or SSH key of a Linux VM deployed in the classic deployment model.</span></span> <span data-ttu-id="fea39-173">Nemůžete použít aktuálně portálu proveďte to pro virtuální počítač s Linuxem nasazené v modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="fea39-173">You can't currently use the portal do to this for a Linux VM deployed in the Resource Manager deployment model.</span></span>
* <span data-ttu-id="fea39-174">V tématu [o rozšíření virtuálního počítače a funkcích](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Další informace o použití rozšíření virtuálního počítače pro virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="fea39-174">See [About virtual machine extensions and features](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more about using VM extensions for Azure virtual machines.</span></span>

