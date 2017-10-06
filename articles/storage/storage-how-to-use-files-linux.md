---
title: aaaUse Azure File storage s Linuxem | Microsoft Docs
description: "Zjistěte, jak sdílet toomount soubor Azure prostřednictvím protokolu SMB v systému Linux."
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
ms.openlocfilehash: ed6ba9b98f121d6629d858320ca3760384303ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-file-storage-with-linux"></a>Používání Azure File storage s Linuxem
[Úložiště Azure File](storage-dotnet-how-to-use-files.md) je systém souborů cloudu snadno toouse společnosti Microsoft. Sdílené složky Azure může být připojen v Linuxových distribucích pomocí hello [cifs utils balíček](https://wiki.samba.org/index.php/LinuxCIFS_utils) z hello [Samba projektu](https://www.samba.org/). Tento článek ukazuje dva způsoby toomount sdílenou složku Azure File: na vyžádání pomocí hello `mount` příkazů a na spouštění pomocí vytváření položku v `/etc/fstab`.

> [!NOTE]  
> V pořadí toomount Azure sdílenou mimo hello oblast Azure, který je hostovaný, například místně nebo v jiné oblasti Azure, hello operačního systému musí podporovat funkci hello šifrování protokolu SMB 3.0. Šifrování funkce pro protokol SMB 3.0 pro Linux byla zavedena v 4.11 jádra. Tato funkce umožňuje připojení Azure sdílené položky z místní nebo jiné oblasti Azure. V době hello publikování tato funkce byla tooUbuntu přeneseny zpět z 16.04 a vyšší.


## <a name="prerequisities-for-mounting-an-azure-file-share-with-linux-and-hello-cifs-utils-package"></a>Prerequisities pro připojení Azure sdílené složky pro Linux a hello cifs utils balíček
* **Vyberte distribuce systému Linux, která může mít hello cifs utils balíček nainstalován**: Společnost Microsoft doporučuje hello následující Linuxových distribucích v galerii Azure image hello:

    * Ubuntu Server 14.04 +
    * RHEL 7 +
    * CentOS 7 +
    * Debian 8
    * openSUSE 13.2 +
    * SUSE Linux Enterprise Server 12

* <a id="install-cifs-utils"></a>**Hello cifs utils je nainstalovaný balíček**: hello cifs-utils můžete nainstalovat pomocí Správce balíčků hello hello distribucí Linux podle svého výběru. 

    Na **Ubuntu** a **na základě Debian** distribuce, použijte hello `apt-get` Správce balíčků:

    ```
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    Na **RHEL** a **CentOS**, použijte hello `yum` Správce balíčků:

    ```
    sudo yum install samba-client samba-common cifs-utils
    ```

    Na **openSUSE**, použijte hello `zypper` Správce balíčků:

    ```
    sudo zypper install samba*
    ```

    Na další distribuce pomocí hello odpovídající package manager nebo [zkompilovat ze zdroje](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).

* **Rozhodněte o oprávnění souboru nebo adresáře hello připojenou složku hello**: V následujících příkladech hello, použijeme 0777, toogive číst, zapisovat a spouštět uživatelé tooall oprávnění. Nahraďte ji s jinými [oprávnění chmod](https://en.wikipedia.org/wiki/Chmod) podle potřeby. 

* **Název účtu úložiště**: sdílenou složku Azure File toomount, bude nutné hello název účtu úložiště hello.

* **Klíč účtu úložiště**: sdílenou složku Azure File toomount, bude nutné hello klíč primární (nebo sekundární) úložiště. Klíče SAS aktuálně nejsou pro připojení podporovány.

* **Ujistěte se, je otevřený port 445**: SMB komunikuje přes port TCP 445 - zkontrolujte porty toosee, pokud brána firewall neblokuje TCP 445 z klientského počítače.

## <a name="mount-hello-azure-file-share-on-demand-with-mount"></a>Připojit hello Azure sdílená složka na vyžádání pomocí`mount`
1. **[Instalovat balíček hello cifs utils pro Linux distribuční](#install-cifs-utils)**.

2. **Vytvořte složku pro hello přípojného bodu**: to můžete provést libovolné místo v systému souborů hello.

    ```
    mkdir mymountpoint
    ```

3. **Použití hello připojení příkaz toomount hello sdílenou složku Azure File**: Mějte na paměti, tooreplace `<storage-account-name>`, `<share-name>`, a `<storage-account-key>` hello správné informace.

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> ./mymountpoint -o vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> Po dokončení pomocí hello sdílenou složku Azure, můžete použít `sudo umount ./mymountpoint` toounmount hello sdílené složky.

## <a name="create-a-persistent-mount-point-for-hello-azure-file-share-with-etcfstab"></a>Vytvořit bod trvalé připojení pro sdílenou složku Azure File hello s`/etc/fstab`
1. **[Instalovat balíček hello cifs utils pro Linux distribuční](#install-cifs-utils)**.

2. **Vytvořte složku pro hello přípojného bodu**: to můžete provést libovolné místo v systému souborů hello, ale potřebujete toonote hello absolutní cestu složky pro hello. Hello následující ukázka vytvoří složku v kořenovém adresáři.

    ```
    sudo mkdir /mymountpoint
    ```

3. **Použití hello následující příkaz tooappend hello následující řádek příliš`/etc/fstab`**: Mějte na paměti, tooreplace `<storage-account-name>`, `<share-name>`, a `<storage-account-key>` hello správné informace.

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> Můžete použít `sudo mount -a` sdílenou složku Azure File hello toomount po dokončení úprav `/etc/fstab` místo restartování.

## <a name="feedback"></a>Váš názor
Uživatelé systému Linux, chceme toohear od vás!

Hello Azure File storage pro skupiny uživatelů Linux poskytuje fórum pro vás zpětnou vazbu tooshare vyhodnotit a přijmout soubor úložiště v systému Linux. E-mailu [Azure File storage Linux uživatelé](mailto:azurefileslinuxusers@microsoft.com) skupiny toojoin hello uživatelů.

## <a name="next-steps"></a>Další kroky
Další informace o úložišti Azure File jsou dostupné na těchto odkazech.
* [REST API služby File – referenční informace](http://msdn.microsoft.com/library/azure/dn167006.aspx)
* [Použití Azure PowerShell s Azure storage](storage-powershell-guide-full.md)
* [Jak toouse AzCopy s Microsoft Azure storage](storage-use-azcopy.md)
* [Použití hello rozhraní příkazového řádku Azure s Azure storage](storage-azure-cli.md#create-and-manage-file-shares)
* [Nejčastější dotazy](storage-files-faq.md)
* [Řešení potíží](storage-troubleshoot-file-connection-problems.md)
