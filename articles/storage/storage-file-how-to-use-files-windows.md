---
title: "aaaMount sdílenou složku Azure a přístup hello sdílené složky v systému Windows | Microsoft Docs"
description: "Připojte Azure sdílené složky a sdílené složky hello přístup v systému Windows."
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: 15ac468d9d7b8e0a195b024926ed4dd9790360d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="mount-an-azure-file-share-and-access-hello-share-in-windows"></a>Připojit Azure sdílené složky a sdílené složky hello přístup v systému Windows
[Úložiště Azure File](storage-dotnet-how-to-use-files.md) je systém souborů cloudu snadno toouse společnosti Microsoft. Sdílené složky Azure je možné připojit v systémech Windows a Windows Server. Tento článek ukazuje tři různé způsoby toomount Azure sdílené složky v systému Windows: s hello uživatelské rozhraní Průzkumníka souborů pomocí prostředí PowerShell a prostřednictvím hello příkazového řádku. 

V pořadí toomount Azure File sdílet mimo hello oblast Azure, které je umístěn v, jako jsou místní nebo v jiné oblasti Azure hello operačního systému musí podporovat SMB 3.0. 

V závislosti na verzi operačního systému je možné sdílenou složku Azure připojit na počítači se systémem Windows v místním prostředí nebo na virtuálním počítači Azure. Níže uvedená tabulka ukazuje hello 

| Verze systému Windows        | Verze protokolu SMB |Možnost připojit na virtuálním počítači Azure|Možnost připojit v místním prostředí|
|------------------------|-------------|---------------------|---------------------|
| Windows 7              | SMB 2.1     | Ano                 | Ne                  |
| Windows Server 2008 R2 | SMB 2.1     | Ano                 | Ne                  |
| Windows 8              | SMB 3.0     | Ano                 | Ano                 |
| Windows Server 2012    | SMB 3.0     | Ano                 | Ano                 |
| Windows Server 2012 R2 | SMB 3.0     | Ano                 | Ano                 |
| Windows 10             | SMB 3.0     | Ano                 | Ano                 |

> [!Note]  
> Doporučujeme, aby pořízení hello nejnovější KB pro vaši verzi systému Windows.

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a></a>Požadavky pro připojení sdílené složky Azure v systému Windows 
* **Název účtu úložiště**: sdílenou složku Azure File toomount, bude nutné hello název účtu úložiště hello.

* **Klíč účtu úložiště**: sdílenou složku Azure File toomount, bude nutné hello klíč primární (nebo sekundární) úložiště. Klíče SAS aktuálně nejsou pro připojení podporovány.

* **Ujistěte se, že je otevřený port 445:** Azure File Storage používá protokol SMB. Toosee SMB komunikuje přes port TCP 445 - zkontrolujte, zda brána firewall neblokuje porty TCP 445 z klientského počítače.

## <a name="mount-hello-azure-file-share-with-file-explorer"></a>Připojit sdílenou složku Azure File hello pomocí Průzkumníka souborů
> [!Note]  
> Všimněte si, že hello následující pokyny se zobrazí na Windows 10 a mohou poněkud lišit na starší verze. 

1. **Otevřete Průzkumníka souborů**: To lze provést otevírání z hello nabídce Start, nebo stiskněte Win + E zástupce.

2. **Přejděte toohello položku "Tento počítač" na levé straně hello okna hello. Tím se změní hello nabídky pásu karet hello k dispozici. V nabídce hello počítač, vyberte možnost "Připojit síťovou jednotku"**.
    
    ![Snímek obrazovky hello "Připojit síťovou jednotku" rozevírací nabídky](media/storage-file-how-to-use-files-windows/1_MountOnWindows10.png)

3. **Cesta UNC hello kopírování z podokna "Připojit" hello v hello portál Azure**: podrobný popis jak toofind tyto informace můžete najít [zde](storage-file-how-to-use-files-portal.md#connect-to-file-share).

    ![cesta UNC Hello hello Azure File storage připojit podokně](media/storage-file-how-to-use-files-windows/portal_netuse_connect.png)

4. **Vyberte písmeno jednotky hello a zadejte cestu UNC hello.** 
    
    ![Snímek obrazovky dialogového okna "Připojit síťovou jednotku" hello](media/storage-file-how-to-use-files-windows/2_MountOnWindows10.png)

5. **Použití hello název účtu úložiště se přidá jako předpona `Azure\` jako uživatelské jméno hello a klíč účtu úložiště jako hello heslo.**
    
    ![Snímek obrazovky dialogu pro přihlašovací údaje hello sítě](media/storage-file-how-to-use-files-windows/3_MountOnWindows10.png)

6. **Používejte sdílenou složku Azure, jak potřebujete**.
    
    ![Sdílená složka Azure je teď připojená](media/storage-file-how-to-use-files-windows/4_MountOnWindows10.png)

7. **Když jsou připravené toodismount (nebo odpojit) hello Azure sdílené složky, můžete tak učinit kliknete pravým tlačítkem na položku hello hello sdílené složky v části hello "umístění v síti" v Průzkumníku souborů a výběrem "Odpojení"**.

## <a name="mount-hello-azure-file-share-with-powershell"></a>Připojit sdílenou složku Azure File hello pomocí prostředí PowerShell
1. **Použití hello následující příkaz sdílenou složku Azure File hello toomount**: Mějte na paměti, tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` hello správné informace.

    ```PowerShell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\<share-name>" -Credential $credential
    ```

2. **Použijte sdílenou složku Azure File hello podle potřeby**.

3. **Jakmile budete hotovi, odpojte hello Azure sdílenou složku pomocí hello následující příkaz**.

    ```PowerShell
    Remove-PSDrive -Name <desired-drive-letter>
    ```

> [!Note]  
> Můžete použít hello `-Persist` parametr na `New-PSDrive` toomake hello zbytku viditelné toohello sdílenou složku Azure File hello operačního systému při připojené.

## <a name="mount-hello-azure-file-share-with-command-prompt"></a>Připojit sdílenou složku Azure File hello pomocí příkazového řádku
1. **Použití hello následující příkaz sdílenou složku Azure File hello toomount**: Mějte na paměti, tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` hello správné informace.

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. **Použijte sdílenou složku Azure File hello podle potřeby**.

3. **Jakmile budete hotovi, odpojte hello Azure sdílenou složku pomocí hello následující příkaz**.

    ```
    net use <desired-drive-letter>: /delete
    ```

> [!Note]  
> Můžete nakonfigurovat hello Azure File znovu připojit ke sdílené složce tooautomatically restartování zachování hello přihlašovacích údajů v systému Windows. Následující příkaz Hello zachová hello přihlašovací údaje:
>   ```
>   cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>
>   ```

## <a name="next-steps"></a>Další kroky
Další informace o úložišti Azure File jsou dostupné na těchto odkazech.

* [Nejčastější dotazy](storage-files-faq.md)
* [Řešení potíží](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a>Koncepční články a videa
* [Azure File Storage: hladký cloudový souborový systém SMB pro Windows a Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Jak toouse Azure File storage s Linuxem](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-file-storage"></a>Podpora nástrojů pro službu Azure File Storage
* [Použití Azure Powershell s Azure Storage](storage-powershell-guide-full.md)
* [Jak toouse AzCopy s Microsoft Azure Storage](storage-use-azcopy.md)
* [Použití hello rozhraní příkazového řádku Azure s Azure Storage](storage-azure-cli.md#create-and-manage-file-shares)
* [Řešení potíží se službou Azure File Storage](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="blog-posts"></a>Příspěvky na blozích
* [Úložiště Azure File je nyní dostupné pro veřejnost](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Uvnitř Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Představujeme službu Microsoft Azure File](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrace dat tooAzure souboru](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referenční informace
* [Klientská knihovna Storage pro .NET – referenční informace](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [REST API služby File – referenční informace](http://msdn.microsoft.com/library/azure/dn167006.aspx)
