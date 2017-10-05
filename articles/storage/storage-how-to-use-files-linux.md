---
title: "Používání Azure File storage s Linuxem | Microsoft Docs"
description: "Zjistěte, jak připojit sdílenou složku Azure přes protokol SMB v systému Linux."
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6edc37ce-698f-4d50-8fc1-591ad456175d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/8/2017
ms.author: renash
ms.openlocfilehash: 27b393a899c60a3a0393619f338a396dff659498
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-azure-file-storage-with-linux"></a>Používání Azure File storage s Linuxem
[Azure File Storage](storage-dotnet-how-to-use-files.md) je snadno použitelný cloudový systém souborů od Microsoftu. Sdílené složky Azure může být připojen v Linuxových distribucích pomocí [cifs utils balíček](https://wiki.samba.org/index.php/LinuxCIFS_utils) z [Samba projektu](https://www.samba.org/). Tento článek ukazuje dva způsoby, jak připojit sdílenou složku Azure: na vyžádání pomocí `mount` příkazů a na spouštění pomocí vytváření položku v `/etc/fstab`.

> [!NOTE]  
> Aby bylo možné připojit Azure sdílenou složku mimo Azure oblast, kterou je hostovaná v, jako jsou místní nebo v jiné oblasti Azure, operačního systému musí podporovat funkci šifrování protokolu SMB 3.0. Šifrování funkce pro protokol SMB 3.0 pro Linux byla zavedena v 4.11 jádra. Tato funkce umožňuje připojení Azure sdílené položky z místní nebo jiné oblasti Azure. V době publikování tato funkce byla přeneseny zpět do Ubuntu z 16.04 a vyšší.


## <a name="prerequisities-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a>Prerequisities pro připojení Azure File sdílet s Linux a cifs utils balíček
* **Vyberte distribuce systému Linux, která může mít tento balíček cifs utils nainstalován**: Společnost Microsoft doporučuje následující Linuxových distribucích v galerii Azure bitové kopie:

    * Ubuntu Server 14.04 +
    * RHEL 7 +
    * CentOS 7 +
    * Debian 8
    * openSUSE 13.2 +
    * SUSE Linux Enterprise Server 12

* <a id="install-cifs-utils"></a>**Instalaci balíčku cifs utils**: cifs-utils můžete nainstalovat pomocí Správce balíčků na distribučním Linux podle svého výběru. 

    Na **Ubuntu** a **na základě Debian** distribuce, použijte `apt-get` Správce balíčků:

    ```
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    Na **RHEL** a **CentOS**, použijte `yum` Správce balíčků:

    ```
    sudo yum install samba-client samba-common cifs-utils
    ```

    Na **openSUSE**, použijte `zypper` Správce balíčků:

    ```
    sudo zypper install samba*
    ```

    Na další distribuce pomocí příslušné package manager nebo [zkompilovat ze zdroje](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).

* **Rozhodněte o oprávnění souboru nebo adresáře připojenou složku**: V následujících příkladech používáme 0777, aby číst, zapisovat a spouštět oprávnění pro všechny uživatele. Nahraďte ji s jinými [oprávnění chmod](https://en.wikipedia.org/wiki/Chmod) podle potřeby. 

* **Název účtu úložiště:** Pro připojení sdílené složky Azure budete potřebovat název účtu úložiště.

* **Klíč účtu úložiště:** Pro připojení sdílené složky Azure budete potřebovat primární (nebo sekundární) klíč úložiště. Klíče SAS aktuálně nejsou pro připojení podporovány.

* **Ujistěte se, je otevřený port 445**: SMB komunikuje přes port TCP 445 - zkontrolujte, pokud chcete zobrazit, pokud brána firewall neblokuje TCP porty 445 z klientského počítače.

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>Připojení Azure File sdílet na vyžádání pomocí`mount`
1. **[Nainstalovat balíček cifs utils pro Linux distribuční](#install-cifs-utils)**.

2. **Vytvořte složku pro přípojného bodu**: to můžete provést libovolné místo v systému souborů.

    ```
    mkdir mymountpoint
    ```

3. **Použijte příkaz připojení připojit sdílenou složku Azure File**: Nezapomeňte nahradit `<storage-account-name>`, `<share-name>`, a `<storage-account-key>` správné informace.

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> ./mymountpoint -o vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> Po dokončení použití sdílené složky Azure File, můžete použít `sudo umount ./mymountpoint` o odpojení sdílené složky.

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>Vytvořit bod trvalé připojení pro Azure sdílené složky`/etc/fstab`
1. **[Nainstalovat balíček cifs utils pro Linux distribuční](#install-cifs-utils)**.

2. **Vytvořte složku pro přípojného bodu**: to můžete provést libovolné místo v systému souborů, ale je potřeba si absolutní cestu ke složce. Následující příklad vytvoří složku v kořenovém adresáři.

    ```
    sudo mkdir /mymountpoint
    ```

3. **Použijte následující příkaz pro připojení následující řádek do `/etc/fstab`** : Nezapomeňte nahradit `<storage-account-name>`, `<share-name>`, a `<storage-account-key>` správné informace.

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> Můžete použít `sudo mount -a` připojit sdílenou složku Azure File po dokončení úprav `/etc/fstab` místo restartování.

## <a name="feedback"></a>Váš názor
Linux uživatele, chceme slyšet váš názor!

Azure File storage pro skupiny uživatelů Linux poskytuje fórum můžete sdílet zpětnou vazbu, jak vyhodnotit a přijmout soubor úložiště v systému Linux. E-mailu [Azure File storage Linux uživatelé](mailto:azurefileslinuxusers@microsoft.com) o připojení ke skupině uživatelů.

## <a name="next-steps"></a>Další kroky
Další informace o úložišti Azure File jsou dostupné na těchto odkazech.
* [REST API služby File – referenční informace](http://msdn.microsoft.com/library/azure/dn167006.aspx)
* [Použití Azure PowerShell s Azure storage](storage-powershell-guide-full.md)
* [Použití nástroje AzCopy s Microsoft Azure storage](storage-use-azcopy.md)
* [Použití Azure CLI s Azure storage](storage-azure-cli.md#create-and-manage-file-shares)
* [Nejčastější dotazy](storage-files-faq.md)
* [Řešení potíží](storage-troubleshoot-file-connection-problems.md)
