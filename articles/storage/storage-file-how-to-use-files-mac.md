---
title: "aaaMount Azure sdílené složky přes protokol SMB systému macOS | Microsoft Docs"
description: "Zjistěte, jak sdílet toomount soubor Azure přes protokol SMB s systému macOS."
services: storage
documentationcenter: 
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
ms.openlocfilehash: 7b4924cb42247470521c1ae8b9d03ab1756996e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-share-over-smb-with-macos"></a>Připojení sdílené složky Azure přes protokol SMB v systému macOS
[Úložiště Azure File](storage-dotnet-how-to-use-files.md) služby společnosti Microsoft, která vám umožní toocreate a použití síťových sdílených souborů v hello Azure používá standardní hello. Sdílené složky Azure je možné připojit v systémech macOS Sierra (10.12) a El Capitan (10.11). Tento článek ukazuje dva různé způsoby toomount Azure sdílené složky v systému macOS s hello vyhledávací uživatelského rozhraní a pomocí hello terminálu.

> [!Note]  
> Před připojením sdílené složky Azure přes protokol SMB doporučujeme zakázat podepisování paketů SMB. Není tak může yield nízký výkon při přístupu ke službě Azure sdílenou složku hello ze systému macOS. Připojení SMB budou zašifrovaná, takže to nemá vliv na zabezpečení hello připojení. Z hello terminálu, hello následující příkazy vypne podepisování paketů SMB, jak je popsáno to [článek podpory od společnosti Apple při vypnutí podepisování paketů](https://support.apple.com/HT205926):  
>    ```
>    sudo -s
>    echo "[default]" >> /etc/nsmb.conf
>    echo "signing_required=no" >> /etc/nsmb.conf
>    exit
>    ```

## <a name="prerequisites-for-mounting-an-azure-file-share-on-macos"></a>Požadavky pro připojení sdílené složky Azure v systému macOS
* **Název účtu úložiště**: sdílenou složku Azure File toomount, bude nutné hello název účtu úložiště hello.

* **Klíč účtu úložiště**: sdílenou složku Azure File toomount, bude nutné hello klíč primární (nebo sekundární) úložiště. Klíče SAS aktuálně nejsou pro připojení podporovány.

* **Ujistěte se, že je otevřený port 445:** Protokol SMB komunikuje přes port TCP 445. Na klientském počítači (hello Mac) zkontrolujte toomake se, že brána firewall neblokuje TCP port 445.

## <a name="mount-an-azure-file-share-via-finder"></a>Připojení sdílené složky Azure přes Finder
1. **Otevřete vyhledávací**: vyhledávací je otevřen v systému macOS ve výchozím nastavení, ale můžete zajistit je hello aktuálně vybrané aplikace kliknutím hello "systému macOS čelí ikona" na ukotvení hello:  
    ![Ikona čelí systému macOS Hello](media/storage-file-how-to-use-files-mac/mount-via-finder-1.png)

2. **Vyberte "Connect tooServer" hello "Přejděte" nabídky**: pomocí cesty UNC hello z hello [požadavky](#preq), převést hello začátku dvojité zpětné lomítko (`\\`) příliš`smb://` a všechny ostatní zpětná lomítka (`\`) tooforwards lomítka (`/`). Odkaz na vaši by měl vypadat jako následující hello: ![hello "Connect tooServer" dialogové okno](./media/storage-file-how-to-use-files-mac/mount-via-finder-2.png)

3. **Použití hello sdílené složky a jeho název klíč účtu po zobrazení výzvy k zadání uživatelského jména a hesla**: Po kliknutí na tlačítko "Připojit" v dialogovém okně "Connect tooServer" hello, zobrazí se výzva k hello uživatelského jména a hesla (to se stane autopopulated s vašeho systému macOS uživatelské jméno). Máte možnost hello umístění klíč účtu úložiště název sdílené složky hello do vašeho systému macOS řetězce klíčů.

4. **Použijte sdílenou složku Azure File hello podle potřeby**: po nahrazení hello sdílené složky a jeho název klíč účtu v hello uživatelské jméno a heslo, se připojí hello sdílené složky. Můžete to použít jako běžně používáte místní složky nebo sdílené složky, včetně přetahování souborů do sdílené složky hello:

    ![Snímek připojené sdílené složky Azure](./media/storage-file-how-to-use-files-mac/mount-via-finder-3.png)

## <a name="mount-an-azure-file-share-via-terminal"></a>Připojení sdílené složky Azure přes Terminál
1. Nahraďte `<storage-account-name>` hello název účtu úložiště. Po zobrazení výzvy zadejte klíč účtu úložiště a heslo. 

    ```
    mount_smbfs //<storage-account-name>@<storage-account-name>.file.core.windows.net/<share-name> <desired-mount-point>
    ```

2. **Použijte sdílenou složku Azure File hello podle potřeby**: sdílenou složku Azure File hello se připojí na hello přípojného bodu určeného hello předchozí příkaz.  

    ![Snímek hello připojit sdílenou složku Azure File](./media/storage-file-how-to-use-files-mac/mount-via-terminal-1.png)

## <a name="next-steps"></a>Další kroky
Další informace o úložišti Azure File jsou dostupné na těchto odkazech.

* [Článek podpory od společnosti Apple - jak tooconnect s sdílení souborů na počítači Mac](https://support.apple.com/HT204445)
* [Nejčastější dotazy](storage-files-faq.md)
* [Řešení potíží](storage-troubleshoot-file-connection-problems.md)