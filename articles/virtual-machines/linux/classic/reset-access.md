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
# <a name="how-tooreset-a-linux-vm-password-or-ssh-key-fix-hello-ssh-configuration-and-check-disk-consistency-using-hello-vmaccess-extension"></a>Jak tooreset virtuálního počítače s Linuxem heslo nebo klíč SSH, opravte konfiguraci SSH hello a kontrola konzistence disku pomocí rozšíření VMAccess hello
Pokud tooa Linux virtuálního počítače na Azure se nelze připojit z důvodu zapomenuté heslo, klíčem nesprávný Secure Shell (SSH) nebo problém s konfigurací hello SSH, použití hello rozšíření VMAccessForLinux s hello rozhraní příkazového řádku Azure tooreset hello heslo nebo klíč SSH, opravte Hello konfiguraci SSH a kontrola konzistence disku. 

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

S hello rozhraní příkazového řádku Azure, použijte hello **sadu rozšíření virtuálního počítače azure** příkazu z vaší tooaccess příkazy rozhraní příkazového řádku (Bash, terminálu, příkazového řádku). Spustit **sadu rozšíření virtuálního počítače azure nápovědy** pro použití podrobné rozšíření.

S hello rozhraní příkazového řádku Azure, můžete provést hello následující úlohy:

* [Resetování hesla hello](#pwresetcli)
* [Resetovat klíč SSH hello](#sshkeyresetcli)
* [Resetování hello heslo a hello klíč SSH](#resetbothcli)
* [Vytvořit nový uživatelský účet sudo](#createnewsudocli)
* [Obnovte konfiguraci SSH hello](#sshconfigresetcli)
* [Odstranit uživatele](#deletecli)
* [Zobrazit stav hello hello rozšíření VMAccess](#statuscli)
* [Kontrola konzistence přidaných disků](#checkdisk)
* [Opravte přidaných disků na virtuálním počítačům s Linuxem](#repairdisk)

## <a name="prerequisites"></a>Požadavky
Budete potřebovat toodo hello následující:

* Budete potřebovat příliš[nainstalovat hello rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md) a [připojení odběru tooyour](../../../xplat-cli-connect.md) toouse Azure prostředky přidružené k účtu.
* Nastavte hello správného režimu pro model nasazení classic hello zadáním následujících hello hello příkazového řádku:
    ``` 
        azure config mode asm
    ```
* Pokud chcete, aby tooreset buď jeden mají nové heslo nebo sadu klíče SSH. Pokud chcete konfiguraci SSH hello tooreset tyto nepotřebujete.

## <a name="pwresetcli"></a>Resetování hesla hello
1. Vytvořte soubor do místního počítače s názvem PrivateConf.json se tyto řádky. Nahraďte **uživatelské_jméno** a  **myP@ssW0rd**  s vlastní uživatelské jméno a heslo a nastavit vlastní datum vypršení platnosti.

    ```   
        {
        "username":"myUserName",
        "password":"myP@ssW0rd",
        "expiration":"2020-01-01"
        }
    ```
        
2. Spusťte tento příkaz, přičemž hello název virtuálního počítače pro **Můjvp**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json
    ```

## <a name="sshkeyresetcli"></a>Resetovat klíč SSH hello
1. Vytvořte soubor s názvem PrivateConf.json se tyto obsah. Nahraďte hello **uživatelské_jméno** a **mySSHKey** hodnoty nahraďte svými vlastními informacemi.

    ```   
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey"
        }
    ```
2. Spusťte tento příkaz, přičemž hello název virtuálního počítače pro **Můjvp**.
   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Resetovat heslo hello i klíč SSH hello
1. Vytvořte soubor s názvem PrivateConf.json se tyto obsah. Nahraďte hello **uživatelské_jméno**, **mySSHKey** a  **myP@ssW0rd**  hodnoty nahraďte svými vlastními informacemi.

    ``` 
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey",
        "password":"myP@ssW0rd"
        }
    ```

2. Spusťte tento příkaz, přičemž hello název virtuálního počítače pro **Můjvp**.

    ```   
        azure vm extension set MyVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="createnewsudocli"></a>Vytvořit nový uživatelský účet sudo

Pokud jste zapomněli svoje uživatelské jméno, můžete použít VMAccess toocreate novou s autoritou sudo hello. V takovém případě hello existující uživatelské jméno a heslo se nezmění.

toocreate nového uživatele s přístupem k heslo, používat hello skript v sudo [resetovat heslo hello](#pwresetcli) a zadejte nové uživatelské jméno hello.

toocreate nového uživatele s přístupem klíče SSH, použijte skript hello v sudo [klíč SSH hello resetování](#sshkeyresetcli) a zadejte nové uživatelské jméno hello.

Můžete také použít [resetovat heslo hello a klíč SSH hello](#resetbothcli) toocreate nového uživatele s heslem a přístup ke klíčům SSH.

## <a name="sshconfigresetcli"></a>Obnovte konfiguraci SSH hello
Pokud konfiguraci SSH hello je ve stavu nežádoucí, můžete také ztratit přístup toohello virtuálních počítačů. Můžete použít hello VMAccess rozšíření tooreset hello konfigurace tooits výchozího stavu. toodo tedy stačí tooset hello "reset_ssh" klíč příliš "True". rozšíření Hello bude restartujte hello SSH server, otevřete port SSH hello na vašem virtuálním počítači a resetovat hodnoty toodefault konfigurace SSH hello. Hello uživatelský účet (název, hesla nebo klíče SSH) nebudou změněny.

> [!NOTE]
> je umístěn v /etc/ssh/sshd_config Hello SSH konfigurační soubor, který získá resetovat.
> 
> 

1. Vytvořte soubor s názvem PrivateConf.json tohoto obsahu.

    ```   
        {
        "reset_ssh":"True"
        }
    ```

2. Spusťte tento příkaz, přičemž hello název virtuálního počítače pro **Můjvp**. 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="deletecli"></a>Odstranit uživatele
Pokud chcete toodelete bez protokolování do toohello virtuálních počítačů přímo uživatelský účet, můžete tento skript.

1. Vytvořte soubor s názvem PrivateConf.json tohoto obsahu, nahraďte hello uživatele název tooremove pro **removeUserName**. 

    ```   
        {
        "remove_user":"removeUserName"
        }
    ```

2. Spusťte tento příkaz, přičemž hello název virtuálního počítače pro **Můjvp**. 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="statuscli"></a>Zobrazit stav hello hello rozšíření VMAccess
Stav hello toodisplay hello rozšíření VMAccess, spusťte tento příkaz.

```
        azure vm extension get
```

## <a name='checkdisk'></a>Kontrola konzistence přidaných disků
toorun fsck na všech discích ve virtuálním počítači Linux, budete potřebovat toodo hello následující:

1. Vytvořte soubor s názvem PublicConf.json tohoto obsahu. Zkontrolujte, zda že disk trvá logickou hodnotu pro jestli připojené disky toocheck tooyour virtuálního počítače nebo ne. 

    ```   
        {   
        "check_disk": "true"
        }
    ```

2. Spusťte tento příkaz tooexecute, nahraďte hello název virtuálního počítače pro **Můjvp**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 
    ```

## <a name='repairdisk'></a>Oprava disky
toorepair disky, které nejsou připojení nebo připojení chyby konfigurace, použijte na virtuální počítač Linux hello VMAccess rozšíření tooreset hello připojení konfiguraci. Nahraďte název hello disku pro **myDisk**.

1. Vytvořte soubor s názvem PublicConf.json tohoto obsahu. 

    ```   
        {
        "repair_disk":"true",
        "disk_name":"myDisk"
        }
    ```

2. Spusťte tento příkaz tooexecute, nahraďte hello název virtuálního počítače pro **Můjvp**.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json
    ```

## <a name="next-steps"></a>Další kroky
* Pokud chcete toouse rutin prostředí Azure PowerShell nebo Azure Resource Manager šablony tooreset hello heslo nebo klíč SSH, opravte hello konfiguraci SSH a kontrola konzistence disku, najdete v části hello [VMAccess rozšíření dokumentaci na Githubu](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 
* Můžete taky hello [portál Azure](https://portal.azure.com) nasazení tooreset hello heslo nebo klíč SSH virtuálního počítače s Linuxem v modelu nasazení classic hello. Hello portálu proveďte toothis nelze použít aktuálně, pro nasazený virtuální počítač s Linuxem v modelu nasazení Resource Manager hello.
* V tématu [o rozšíření virtuálního počítače a funkcích](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Další informace o použití rozšíření virtuálního počítače pro virtuální počítače Azure.

