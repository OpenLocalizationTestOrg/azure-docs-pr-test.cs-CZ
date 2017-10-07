---
title: aaaIntroduction tooFreeBSD v Azure | Microsoft Docs
description: "Další informace o použití FreeBSD virtuální počítače v Azure"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 32b87a5f-d024-4da0-8bf0-77e233d1422b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: kyliel
ms.openlocfilehash: 43ba7a70ed21e7fb8b331f4a26db0426e098c4aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toofreebsd-on-azure"></a>Úvod tooFreeBSD v Azure
Toto téma obsahuje přehled spuštěným virtuálním počítačem FreeBSD v Azure.

## <a name="overview"></a>Přehled
FreeBSD pro Microsoft Azure je, že že pokročilým operační systém používal toopower moderní serverů, stolních počítačů a embedded platformy.

Microsoft Corporation je zpřístupnění bitové kopie FreeBSD v Azure s hello [agenta hosta virtuálního počítače Azure](https://github.com/Azure/WALinuxAgent/) předem nakonfigurované. V současné době hello následující verze FreeBSD jsou nabízeny jako obrázky společností Microsoft:

- 10.3 uvolnění FreeBSD
- FreeBSD 11.0 – verze

Hello agent zodpovídá za komunikaci mezi hello FreeBSD virtuálních počítačů a hello prostředků infrastruktury Azure pro operace, jako je například zřizování hello virtuálního počítače na první použití (uživatelské jméno, heslo nebo klíč SSH, název hostitele, atd.) a povolení funkce pro selektivní rozšíření virtuálního počítače.

Jako u budoucích verzích FreeBSD strategie hello je toostay aktuální a zpřístupnit hello nejnovější verze krátce po jsou publikovány nástrojem hello FreeBSD verze technickému týmu.

## <a name="deploying-a-freebsd-virtual-machine"></a>Nasazení virtuálního počítače FreeBSD
Nasazení virtuálního počítače FreeBSD je jednoduchý proces pomocí bitové kopie z Azure Marketplace hello z hello portálu Azure:

- [10.3 FreeBSD v hello Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
- [FreeBSD 11.0 na hello Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/)

### <a name="create-a-freebsd-vm-through-azure-cli-20-on-freebsd"></a>Vytvoření virtuálního počítače FreeBSD prostřednictvím rozhraní příkazového řádku Azure 2.0 na FreeBSD
Je třeba nejprve tooinstall [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) i když následující příkaz na počítači FreeBSD.

```bash 
curl -L https://aka.ms/InstallAzureCli | bash
```

Pokud bash není nainstalovaný na počítači FreeBSD, spusťte následující příkaz před instalací hello. 

```bash
sudo pkg install bash
```

Pokud python není nainstalovaný na počítači FreeBSD, spusťte následující příkazy před instalací hello. 

```bash
sudo pkg install python35
cd /usr/local/bin 
sudo rm /usr/local/bin/python 
sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

Během instalace hello se zobrazí výzva `Modify profile tooupdate your $PATH and enable shell/tab completion now? (Y/n)`. Pokud odpovíte `y` a zadejte `/etc/rc.conf` jako `a path tooan rc file tooupdate`, splňujete hello problém `ERROR: [Errno 13] Permission denied`. tooresolve tento problém byste měli udělit hello zápisu správné toocurrent uživatele vůči hello souboru `etc/rc.conf`.

Nyní můžete přihlásit Azure a vytvořit FreeBSD virtuálního počítače. Níže je toocreate příklad FreeBSD 11.0 virtuálního počítače. Můžete také přidat parametr hello `--public-ip-address-dns-name` s globálně jedinečného názvu DNS pro nově vytvořený veřejnou IP adresu. 

```azurecli
az login 
az group create --name myResourceGroup --location eastus
az vm create --name myFreeBSD11 \
    --resource-group myResourceGroup \
    --image MicrosoftOSTC:FreeBSD:11.0:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

Potom se můžete přihlásit tooyour FreeBSD virtuálních počítačů prostřednictvím hello ip adresu, která vytištěn v výstup hello výše nasazení. 

```bash
ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a>Rozšíření virtuálního počítače pro FreeBSD
Toto jsou podporované rozšíření virtuálního počítače v FreeBSD.

### <a name="vmaccess"></a>VMAccess
Hello [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) rozšíření můžete:

* Resetovat heslo hello hello původní sudo uživatele.
* Vytvořte nového uživatele sudo s zadané heslo hello.
* Nastavit klíč veřejný hostitele hello klíčem hello zadána.
* Resetujte klíč veřejný hostitele hello zadané během zřizování, pokud není k dispozici klíč hello hostitele virtuálních počítačů.
* Otevřete port SSH hello (22) a obnovit hello sshd_config, pokud reset_ssh nastavena tootrue.
* Odeberte hello stávajícího uživatele.
* Zkontrolujte disky.
* Přidání disku opravte.

### <a name="customscript"></a>CustomScript
Hello [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) rozšíření můžete:

* Pokud je zadán, stáhněte hello přizpůsobit skripty z Azure Storage nebo veřejného externího úložiště (například Githubu).
* Hello vstupní bod skript spusťte.
* Vnořené příkazy podpory.
* Automaticky převeďte nového řádku styl systému Windows v prostředí a skriptů Python.
* Automaticky odeberte BOM v prostředí a skriptů Python.
* Chrání citlivá data v CommandToExecute.

> [!NOTE]
> Virtuální počítač FreeBSD podporuje pouze verzi CustomScript 1.x nyní.  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Ověřování: uživatelská jména, hesla a klíče SSH
Při vytváření virtuálního počítače FreeBSD pomocí hello portálu Azure, je nutné zadat uživatelské jméno, heslo nebo veřejný klíč SSH.
Uživatelská jména pro nasazení virtuálního počítače FreeBSD v Azure nesmí shodovat s názvy účtů systému (UID < 100) již existuje ve virtuálním počítači hello ("root", např.).
V současné době je podporován pouze hello RSA klíč SSH. Víceřádkový klíč SSH musí začínat řetězcem `---- BEGIN SSH2 PUBLIC KEY ----` a končit `---- END SSH2 PUBLIC KEY ----`.

## <a name="obtaining-superuser-privileges"></a>Získání oprávnění superuživatele
Hello uživatelský účet, který je zadán během nasazení instance virtuálního počítače na platformě Azure je privilegovaný účet. Hello balíček sudo byl nainstalován do hello publikovaná FreeBSD image.
Poté, co jste přihlášeni prostřednictvím tento uživatelský účet, můžete spustit příkazy jako kořenová pomocí syntaxe příkazu hello.

```
$ sudo <COMMAND>
```

Kořenové prostředí můžete volitelně můžete získat pomocí `sudo -s`.

## <a name="known-issues"></a>Známé problémy
Hello [agenta hosta virtuálního počítače Azure](https://github.com/Azure/WALinuxAgent/) verze 2.2.2 má [známý problém] (https://github.com/Azure/WALinuxAgent/pull/517), která způsobí selhání hello přidělení pro virtuální počítač FreeBSD v Azure. Hello oprava zaznamenaná [agenta hosta virtuálního počítače Azure](https://github.com/Azure/WALinuxAgent/) verze 2.2.3 a pozdějších verzích. 

## <a name="next-steps"></a>Další kroky
* Přejděte příliš[Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) toocreate FreeBSD virtuálního počítače.
* Pokud chcete vlastní FreeBSD tooAzure toobring, podívejte se příliš[vytvoření a nahrání virtuálního pevného disku FreeBSD tooAzure](classic/freebsd-create-upload-vhd.md).
