---
title: "aaaReset heslo virtuálního počítače s Linuxem a SSH klíče z hello rozhraní příkazového řádku | Microsoft Docs"
description: "Jak toouse hello rozšíření VMAccess z rozhraní příkazového řádku Azure (CLI) tooreset hello virtuálního počítače s Linuxem heslo nebo klíč SSH, opravte konfiguraci SSH hello a kontrola konzistence disku"
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
ms.openlocfilehash: 1650ad64fb982627ae9f90b1a8209bb56bac7004
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-a-linux-vm-password-or-ssh-key-fix-hello-ssh-configuration-and-check-disk-consistency-using-hello-vmaccess-extension"></a><span data-ttu-id="39f24-103">Jak tooreset virtuálního počítače s Linuxem heslo nebo klíč SSH, opravte konfiguraci SSH hello a kontrola konzistence disku pomocí rozšíření VMAccess hello</span><span class="sxs-lookup"><span data-stu-id="39f24-103">How tooreset a Linux VM password or SSH key, fix hello SSH configuration, and check disk consistency using hello VMAccess extension</span></span>
<span data-ttu-id="39f24-104">Pokud tooa Linux virtuálního počítače na Azure se nelze připojit z důvodu zapomenuté heslo, klíčem nesprávný Secure Shell (SSH) nebo problém s konfigurací hello SSH, použití hello rozšíření VMAccessForLinux s hello rozhraní příkazového řádku Azure tooreset hello heslo nebo klíč SSH, opravte Hello konfiguraci SSH a kontrola konzistence disku.</span><span class="sxs-lookup"><span data-stu-id="39f24-104">If you can't connect tooa Linux virtual machine on Azure because of a forgotten password, an incorrect Secure Shell (SSH) key, or a problem with hello SSH configuration, use hello VMAccessForLinux extension with hello Azure CLI tooreset hello password or SSH key, fix hello SSH configuration, and check disk consistency.</span></span> 

> [!IMPORTANT] 
> <span data-ttu-id="39f24-105">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="39f24-105">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="39f24-106">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="39f24-106">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="39f24-107">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="39f24-107">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="39f24-108">Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span><span class="sxs-lookup"><span data-stu-id="39f24-108">Learn how too[perform these steps using hello Resource Manager model](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span></span>

<span data-ttu-id="39f24-109">S hello rozhraní příkazového řádku Azure, použijte hello **sadu rozšíření virtuálního počítače azure** příkazu z vaší tooaccess příkazy rozhraní příkazového řádku (Bash, terminálu, příkazového řádku).</span><span class="sxs-lookup"><span data-stu-id="39f24-109">With hello Azure CLI, you use hello **azure vm extension set** command from your command-line interface (Bash, Terminal, Command prompt) tooaccess commands.</span></span> <span data-ttu-id="39f24-110">Spustit **sadu rozšíření virtuálního počítače azure nápovědy** pro použití podrobné rozšíření.</span><span class="sxs-lookup"><span data-stu-id="39f24-110">Run **azure help vm extension set** for detailed extension usage.</span></span>

<span data-ttu-id="39f24-111">S hello rozhraní příkazového řádku Azure, můžete provést hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="39f24-111">With hello Azure CLI, you can do hello following tasks:</span></span>

* [<span data-ttu-id="39f24-112">Resetování hesla hello</span><span class="sxs-lookup"><span data-stu-id="39f24-112">Reset hello password</span></span>](#pwresetcli)
* [<span data-ttu-id="39f24-113">Resetovat klíč SSH hello</span><span class="sxs-lookup"><span data-stu-id="39f24-113">Reset hello SSH key</span></span>](#sshkeyresetcli)
* [<span data-ttu-id="39f24-114">Resetování hello heslo a hello klíč SSH</span><span class="sxs-lookup"><span data-stu-id="39f24-114">Reset hello password and hello SSH key</span></span>](#resetbothcli)
* [<span data-ttu-id="39f24-115">Vytvořit nový uživatelský účet sudo</span><span class="sxs-lookup"><span data-stu-id="39f24-115">Create a new sudo user account</span></span>](#createnewsudocli)
* [<span data-ttu-id="39f24-116">Obnovte konfiguraci SSH hello</span><span class="sxs-lookup"><span data-stu-id="39f24-116">Reset hello SSH configuration</span></span>](#sshconfigresetcli)
* [<span data-ttu-id="39f24-117">Odstranit uživatele</span><span class="sxs-lookup"><span data-stu-id="39f24-117">Delete a user</span></span>](#deletecli)
* [<span data-ttu-id="39f24-118">Zobrazit stav hello hello rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="39f24-118">Display hello status of hello VMAccess extension</span></span>](#statuscli)
* [<span data-ttu-id="39f24-119">Kontrola konzistence přidaných disků</span><span class="sxs-lookup"><span data-stu-id="39f24-119">Check consistency of added disks</span></span>](#checkdisk)
* [<span data-ttu-id="39f24-120">Opravte přidaných disků na virtuálním počítačům s Linuxem</span><span class="sxs-lookup"><span data-stu-id="39f24-120">Repair added disks on your Linux VM</span></span>](#repairdisk)

## <a name="prerequisites"></a><span data-ttu-id="39f24-121">Požadavky</span><span class="sxs-lookup"><span data-stu-id="39f24-121">Prerequisites</span></span>
<span data-ttu-id="39f24-122">Budete potřebovat toodo hello následující:</span><span class="sxs-lookup"><span data-stu-id="39f24-122">You will need toodo hello following:</span></span>

* <span data-ttu-id="39f24-123">Budete potřebovat příliš[nainstalovat hello rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md) a [připojení odběru tooyour](../../../xplat-cli-connect.md) toouse Azure prostředky přidružené k účtu.</span><span class="sxs-lookup"><span data-stu-id="39f24-123">You will need too[install hello Azure CLI](../../../cli-install-nodejs.md) and [connect tooyour subscription](../../../xplat-cli-connect.md) toouse Azure resources associated with your account.</span></span>
* <span data-ttu-id="39f24-124">Nastavte hello správného režimu pro model nasazení classic hello zadáním následujících hello hello příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="39f24-124">Set hello correct mode for hello classic deployment model by typing hello following at hello command prompt:</span></span>
    ``` 
        azure config mode asm
    ```
* <span data-ttu-id="39f24-125">Pokud chcete, aby tooreset buď jeden mají nové heslo nebo sadu klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="39f24-125">Have a new password or set of SSH keys, if you want tooreset either one.</span></span> <span data-ttu-id="39f24-126">Pokud chcete konfiguraci SSH hello tooreset tyto nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="39f24-126">You don't need these if you want tooreset hello SSH configuration.</span></span>

## <span data-ttu-id="39f24-127"><a name="pwresetcli"></a>Resetování hesla hello</span><span class="sxs-lookup"><span data-stu-id="39f24-127"><a name="pwresetcli"></a>Reset hello password</span></span>
1. <span data-ttu-id="39f24-128">Vytvořte soubor do místního počítače s názvem PrivateConf.json se tyto řádky.</span><span class="sxs-lookup"><span data-stu-id="39f24-128">Create a file on your local computer named PrivateConf.json with these lines.</span></span> <span data-ttu-id="39f24-129">Nahraďte **uživatelské_jméno** a  **myP@ssW0rd**  s vlastní uživatelské jméno a heslo a nastavit vlastní datum vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="39f24-129">Replace **myUserName** and **myP@ssW0rd** with your own user name and password and set your own date for expiration.</span></span>

    ```   
        {
        "username":"myUserName",
        "password":"myP@ssW0rd",
        "expiration":"2020-01-01"
        }
    ```
        
2. <span data-ttu-id="39f24-130">Spusťte tento příkaz, přičemž hello název virtuálního počítače pro **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="39f24-130">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json
    ```

## <span data-ttu-id="39f24-131"><a name="sshkeyresetcli"></a>Resetovat klíč SSH hello</span><span class="sxs-lookup"><span data-stu-id="39f24-131"><a name="sshkeyresetcli"></a>Reset hello SSH key</span></span>
1. <span data-ttu-id="39f24-132">Vytvořte soubor s názvem PrivateConf.json se tyto obsah.</span><span class="sxs-lookup"><span data-stu-id="39f24-132">Create a file named PrivateConf.json with these contents.</span></span> <span data-ttu-id="39f24-133">Nahraďte hello **uživatelské_jméno** a **mySSHKey** hodnoty nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="39f24-133">Replace hello **myUserName** and **mySSHKey** values with your own information.</span></span>

    ```   
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey"
        }
    ```
2. <span data-ttu-id="39f24-134">Spusťte tento příkaz, přičemž hello název virtuálního počítače pro **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="39f24-134">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span>
   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <span data-ttu-id="39f24-135"><a name="resetbothcli"></a>Resetovat heslo hello i klíč SSH hello</span><span class="sxs-lookup"><span data-stu-id="39f24-135"><a name="resetbothcli"></a>Reset both hello password and hello SSH key</span></span>
1. <span data-ttu-id="39f24-136">Vytvořte soubor s názvem PrivateConf.json se tyto obsah.</span><span class="sxs-lookup"><span data-stu-id="39f24-136">Create a file named PrivateConf.json with these contents.</span></span> <span data-ttu-id="39f24-137">Nahraďte hello **uživatelské_jméno**, **mySSHKey** a  **myP@ssW0rd**  hodnoty nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="39f24-137">Replace hello **myUserName**, **mySSHKey** and **myP@ssW0rd** values with your own information.</span></span>

    ``` 
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey",
        "password":"myP@ssW0rd"
        }
    ```

2. <span data-ttu-id="39f24-138">Spusťte tento příkaz, přičemž hello název virtuálního počítače pro **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="39f24-138">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set MyVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <span data-ttu-id="39f24-139"><a name="createnewsudocli"></a>Vytvořit nový uživatelský účet sudo</span><span class="sxs-lookup"><span data-stu-id="39f24-139"><a name="createnewsudocli"></a>Create a new sudo user account</span></span>

<span data-ttu-id="39f24-140">Pokud jste zapomněli svoje uživatelské jméno, můžete použít VMAccess toocreate novou s autoritou sudo hello.</span><span class="sxs-lookup"><span data-stu-id="39f24-140">If you forget your user name, you can use VMAccess toocreate a new one with hello sudo authority.</span></span> <span data-ttu-id="39f24-141">V takovém případě hello existující uživatelské jméno a heslo se nezmění.</span><span class="sxs-lookup"><span data-stu-id="39f24-141">In this case, hello existing user name and password will not be modified.</span></span>

<span data-ttu-id="39f24-142">toocreate nového uživatele s přístupem k heslo, používat hello skript v sudo [resetovat heslo hello](#pwresetcli) a zadejte nové uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="39f24-142">toocreate a new sudo user with password access, use hello script in [Reset hello password](#pwresetcli) and specify hello new user name.</span></span>

<span data-ttu-id="39f24-143">toocreate nového uživatele s přístupem klíče SSH, použijte skript hello v sudo [klíč SSH hello resetování](#sshkeyresetcli) a zadejte nové uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="39f24-143">toocreate a new sudo user with SSH key access, use hello script in [Reset hello SSH key](#sshkeyresetcli) and specify hello new user name.</span></span>

<span data-ttu-id="39f24-144">Můžete také použít [resetovat heslo hello a klíč SSH hello](#resetbothcli) toocreate nového uživatele s heslem a přístup ke klíčům SSH.</span><span class="sxs-lookup"><span data-stu-id="39f24-144">You can also use [Reset hello password and hello SSH key](#resetbothcli) toocreate a new user with both password and SSH key access.</span></span>

## <span data-ttu-id="39f24-145"><a name="sshconfigresetcli"></a>Obnovte konfiguraci SSH hello</span><span class="sxs-lookup"><span data-stu-id="39f24-145"><a name="sshconfigresetcli"></a>Reset hello SSH configuration</span></span>
<span data-ttu-id="39f24-146">Pokud konfiguraci SSH hello je ve stavu nežádoucí, můžete také ztratit přístup toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="39f24-146">If hello SSH configuration is in an undesired state, you might also lose access toohello VM.</span></span> <span data-ttu-id="39f24-147">Můžete použít hello VMAccess rozšíření tooreset hello konfigurace tooits výchozího stavu.</span><span class="sxs-lookup"><span data-stu-id="39f24-147">You can use hello VMAccess extension tooreset hello configuration tooits default state.</span></span> <span data-ttu-id="39f24-148">toodo tedy stačí tooset hello "reset_ssh" klíč příliš "True".</span><span class="sxs-lookup"><span data-stu-id="39f24-148">toodo so, you just need tooset hello “reset_ssh” key too“True”.</span></span> <span data-ttu-id="39f24-149">rozšíření Hello bude restartujte hello SSH server, otevřete port SSH hello na vašem virtuálním počítači a resetovat hodnoty toodefault konfigurace SSH hello.</span><span class="sxs-lookup"><span data-stu-id="39f24-149">hello extension will restart hello SSH server, open hello SSH port on your VM, and reset hello SSH configuration toodefault values.</span></span> <span data-ttu-id="39f24-150">Hello uživatelský účet (název, hesla nebo klíče SSH) nebudou změněny.</span><span class="sxs-lookup"><span data-stu-id="39f24-150">hello user account (name, password or SSH keys) will not be changed.</span></span>

> [!NOTE]
> <span data-ttu-id="39f24-151">je umístěn v /etc/ssh/sshd_config Hello SSH konfigurační soubor, který získá resetovat.</span><span class="sxs-lookup"><span data-stu-id="39f24-151">hello SSH configuration file that gets reset is located at /etc/ssh/sshd_config.</span></span>
> 
> 

1. <span data-ttu-id="39f24-152">Vytvořte soubor s názvem PrivateConf.json tohoto obsahu.</span><span class="sxs-lookup"><span data-stu-id="39f24-152">Create a file named PrivateConf.json with this content.</span></span>

    ```   
        {
        "reset_ssh":"True"
        }
    ```

2. <span data-ttu-id="39f24-153">Spusťte tento příkaz, přičemž hello název virtuálního počítače pro **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="39f24-153">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span> 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <span data-ttu-id="39f24-154"><a name="deletecli"></a>Odstranit uživatele</span><span class="sxs-lookup"><span data-stu-id="39f24-154"><a name="deletecli"></a>Delete a user</span></span>
<span data-ttu-id="39f24-155">Pokud chcete toodelete bez protokolování do toohello virtuálních počítačů přímo uživatelský účet, můžete tento skript.</span><span class="sxs-lookup"><span data-stu-id="39f24-155">If you want toodelete a user account without logging into toohello VM directly, you can use this script.</span></span>

1. <span data-ttu-id="39f24-156">Vytvořte soubor s názvem PrivateConf.json tohoto obsahu, nahraďte hello uživatele název tooremove pro **removeUserName**.</span><span class="sxs-lookup"><span data-stu-id="39f24-156">Create a file named PrivateConf.json with this content, substituting hello user name tooremove for **removeUserName**.</span></span> 

    ```   
        {
        "remove_user":"removeUserName"
        }
    ```

2. <span data-ttu-id="39f24-157">Spusťte tento příkaz, přičemž hello název virtuálního počítače pro **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="39f24-157">Run this command, substituting hello name of your virtual machine for **myVM**.</span></span> 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <span data-ttu-id="39f24-158"><a name="statuscli"></a>Zobrazit stav hello hello rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="39f24-158"><a name="statuscli"></a>Display hello status of hello VMAccess extension</span></span>
<span data-ttu-id="39f24-159">Stav hello toodisplay hello rozšíření VMAccess, spusťte tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="39f24-159">toodisplay hello status of hello VMAccess extension, run this command.</span></span>

```
        azure vm extension get
```

## <span data-ttu-id="39f24-160"><a name='checkdisk'></a>Kontrola konzistence přidaných disků</span><span class="sxs-lookup"><span data-stu-id="39f24-160"><a name='checkdisk'></a>Check consistency of added disks</span></span>
<span data-ttu-id="39f24-161">toorun fsck na všech discích ve virtuálním počítači Linux, budete potřebovat toodo hello následující:</span><span class="sxs-lookup"><span data-stu-id="39f24-161">toorun fsck on all disks in your Linux virtual machine, you will need toodo hello following:</span></span>

1. <span data-ttu-id="39f24-162">Vytvořte soubor s názvem PublicConf.json tohoto obsahu.</span><span class="sxs-lookup"><span data-stu-id="39f24-162">Create a file named PublicConf.json with this content.</span></span> <span data-ttu-id="39f24-163">Zkontrolujte, zda že disk trvá logickou hodnotu pro jestli připojené disky toocheck tooyour virtuálního počítače nebo ne.</span><span class="sxs-lookup"><span data-stu-id="39f24-163">Check Disk takes a boolean for whether toocheck disks attached tooyour virtual machine or not.</span></span> 

    ```   
        {   
        "check_disk": "true"
        }
    ```

2. <span data-ttu-id="39f24-164">Spusťte tento příkaz tooexecute, nahraďte hello název virtuálního počítače pro **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="39f24-164">Run this command tooexecute, substituting hello name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 
    ```

## <span data-ttu-id="39f24-165"><a name='repairdisk'></a>Oprava disky</span><span class="sxs-lookup"><span data-stu-id="39f24-165"><a name='repairdisk'></a>Repair disks</span></span>
<span data-ttu-id="39f24-166">toorepair disky, které nejsou připojení nebo připojení chyby konfigurace, použijte na virtuální počítač Linux hello VMAccess rozšíření tooreset hello připojení konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="39f24-166">toorepair disks that are not mounting or have mount configuration errors, use hello VMAccess extension tooreset hello mount configuration on your Linux virtual machine.</span></span> <span data-ttu-id="39f24-167">Nahraďte název hello disku pro **myDisk**.</span><span class="sxs-lookup"><span data-stu-id="39f24-167">Substituting hello name of your disk for **myDisk**.</span></span>

1. <span data-ttu-id="39f24-168">Vytvořte soubor s názvem PublicConf.json tohoto obsahu.</span><span class="sxs-lookup"><span data-stu-id="39f24-168">Create a file named PublicConf.json with this content.</span></span> 

    ```   
        {
        "repair_disk":"true",
        "disk_name":"myDisk"
        }
    ```

2. <span data-ttu-id="39f24-169">Spusťte tento příkaz tooexecute, nahraďte hello název virtuálního počítače pro **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="39f24-169">Run this command tooexecute, substituting hello name of your virtual machine for **myVM**.</span></span>

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json
    ```

## <a name="next-steps"></a><span data-ttu-id="39f24-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="39f24-170">Next steps</span></span>
* <span data-ttu-id="39f24-171">Pokud chcete toouse rutin prostředí Azure PowerShell nebo Azure Resource Manager šablony tooreset hello heslo nebo klíč SSH, opravte hello konfiguraci SSH a kontrola konzistence disku, najdete v části hello [VMAccess rozšíření dokumentaci na Githubu](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span><span class="sxs-lookup"><span data-stu-id="39f24-171">If you want toouse Azure PowerShell cmdlets or Azure Resource Manager templates tooreset hello password or SSH key, fix hello SSH configuration, and check disk consistency, see hello [VMAccess extension documentation on GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).</span></span> 
* <span data-ttu-id="39f24-172">Můžete taky hello [portál Azure](https://portal.azure.com) nasazení tooreset hello heslo nebo klíč SSH virtuálního počítače s Linuxem v modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="39f24-172">You can also use hello [Azure portal](https://portal.azure.com) tooreset hello password or SSH key of a Linux VM deployed in hello classic deployment model.</span></span> <span data-ttu-id="39f24-173">Hello portálu proveďte toothis nelze použít aktuálně, pro nasazený virtuální počítač s Linuxem v modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="39f24-173">You can't currently use hello portal do toothis for a Linux VM deployed in hello Resource Manager deployment model.</span></span>
* <span data-ttu-id="39f24-174">V tématu [o rozšíření virtuálního počítače a funkcích](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Další informace o použití rozšíření virtuálního počítače pro virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="39f24-174">See [About virtual machine extensions and features](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more about using VM extensions for Azure virtual machines.</span></span>

