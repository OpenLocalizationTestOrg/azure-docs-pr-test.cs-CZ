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
ms.openlocfilehash: 71aaec8a77b770fe147b783c0ab9f86830176bec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-share-over-smb-with-macos"></a><span data-ttu-id="c885c-103">Připojení sdílené složky Azure přes protokol SMB v systému macOS</span><span class="sxs-lookup"><span data-stu-id="c885c-103">Mount Azure File share over SMB with macOS</span></span>
<span data-ttu-id="c885c-104">[Úložiště Azure File](../storage-dotnet-how-to-use-files.md) služby společnosti Microsoft, která vám umožní toocreate a použití síťových sdílených souborů v hello Azure používá standardní hello.</span><span class="sxs-lookup"><span data-stu-id="c885c-104">[Azure File storage](../storage-dotnet-how-to-use-files.md) is Microsoft's service that enables you toocreate and use network file shares in hello Azure using hello industry standard.</span></span> <span data-ttu-id="c885c-105">Sdílené složky Azure je možné připojit v systémech macOS Sierra (10.12) a El Capitan (10.11).</span><span class="sxs-lookup"><span data-stu-id="c885c-105">Azure File shares can be mounted in macOS Sierra (10.12) and El Capitan (10.11).</span></span> <span data-ttu-id="c885c-106">Tento článek ukazuje dva různé způsoby toomount Azure sdílené složky v systému macOS s hello vyhledávací uživatelského rozhraní a pomocí hello terminálu.</span><span class="sxs-lookup"><span data-stu-id="c885c-106">This article shows two different ways toomount an Azure File share on macOS with hello Finder UI and using hello Terminal.</span></span>

> [!Note]  
> <span data-ttu-id="c885c-107">Před připojením sdílené složky Azure přes protokol SMB doporučujeme zakázat podepisování paketů SMB.</span><span class="sxs-lookup"><span data-stu-id="c885c-107">Before mounting an Azure File share over SMB, we recommend disabling SMB packet signing.</span></span> <span data-ttu-id="c885c-108">Není tak může yield nízký výkon při přístupu ke službě Azure sdílenou složku hello ze systému macOS.</span><span class="sxs-lookup"><span data-stu-id="c885c-108">Not doing so may yield poor performance when accessing hello Azure File share from macOS.</span></span> <span data-ttu-id="c885c-109">Připojení SMB budou zašifrovaná, takže to nemá vliv na zabezpečení hello připojení.</span><span class="sxs-lookup"><span data-stu-id="c885c-109">Your SMB connection will be encrypted, so this does not affect hello security of your connection.</span></span> <span data-ttu-id="c885c-110">Z hello terminálu, hello následující příkazy vypne podepisování paketů SMB, jak je popsáno to [článek podpory od společnosti Apple při vypnutí podepisování paketů](https://support.apple.com/HT205926):</span><span class="sxs-lookup"><span data-stu-id="c885c-110">From hello terminal, hello following commands will disable SMB packet signing, as described by this [Apple support article on disabling SMB packet signing](https://support.apple.com/HT205926):</span></span>  
>    ```
>    sudo -s
>    echo "[default]" >> /etc/nsmb.conf
>    echo "signing_required=no" >> /etc/nsmb.conf
>    exit
>    ```

## <a name="prerequisites-for-mounting-an-azure-file-share-on-macos"></a><span data-ttu-id="c885c-111">Požadavky pro připojení sdílené složky Azure v systému macOS</span><span class="sxs-lookup"><span data-stu-id="c885c-111">Prerequisites for mounting an Azure File share on macOS</span></span>
* <span data-ttu-id="c885c-112">**Název účtu úložiště**: sdílenou složku Azure File toomount, bude nutné hello název účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="c885c-112">**Storage account name**: toomount an Azure File share, you will need hello name of hello storage account.</span></span>

* <span data-ttu-id="c885c-113">**Klíč účtu úložiště**: sdílenou složku Azure File toomount, bude nutné hello klíč primární (nebo sekundární) úložiště.</span><span class="sxs-lookup"><span data-stu-id="c885c-113">**Storage account key**: toomount an Azure File share, you will need hello primary (or secondary) storage key.</span></span> <span data-ttu-id="c885c-114">Klíče SAS aktuálně nejsou pro připojení podporovány.</span><span class="sxs-lookup"><span data-stu-id="c885c-114">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="c885c-115">**Ujistěte se, že je otevřený port 445:** Protokol SMB komunikuje přes port TCP 445.</span><span class="sxs-lookup"><span data-stu-id="c885c-115">**Ensure port 445 is open**: SMB communicates over TCP port 445.</span></span> <span data-ttu-id="c885c-116">Na klientském počítači (hello Mac) zkontrolujte toomake se, že brána firewall neblokuje TCP port 445.</span><span class="sxs-lookup"><span data-stu-id="c885c-116">On your client machine (hello Mac), check toomake sure your firewall is not blocking TCP port 445.</span></span>

## <a name="mount-an-azure-file-share-via-finder"></a><span data-ttu-id="c885c-117">Připojení sdílené složky Azure přes Finder</span><span class="sxs-lookup"><span data-stu-id="c885c-117">Mount an Azure File share via Finder</span></span>
1. <span data-ttu-id="c885c-118">**Otevřete vyhledávací**: vyhledávací je otevřen v systému macOS ve výchozím nastavení, ale můžete zajistit je hello aktuálně vybrané aplikace kliknutím hello "systému macOS čelí ikona" na ukotvení hello:</span><span class="sxs-lookup"><span data-stu-id="c885c-118">**Open Finder**: Finder is open on macOS by default, but you can ensure it is hello currently selected application by clicking hello "macOS face icon" on hello dock:</span></span>  
    <span data-ttu-id="c885c-119">![Ikona čelí systému macOS Hello](./media/storage-how-to-use-files-mac/mount-via-finder-1.png)</span><span class="sxs-lookup"><span data-stu-id="c885c-119">![hello macOS face icon](./media/storage-how-to-use-files-mac/mount-via-finder-1.png)</span></span>

2. <span data-ttu-id="c885c-120">**Vyberte "Connect tooServer" hello "Přejděte" nabídky**: pomocí cesty UNC hello z hello [požadavky](#preq), převést hello začátku dvojité zpětné lomítko (`\\`) příliš`smb://` a všechny ostatní zpětná lomítka (`\`) tooforwards lomítka (`/`).</span><span class="sxs-lookup"><span data-stu-id="c885c-120">**Select "Connect tooServer" from hello "Go" Menu**: Using hello UNC path from hello [prerequisites](#preq), convert hello beginning double backslash (`\\`) too`smb://` and all other backslashes (`\`) tooforwards slashes (`/`).</span></span> <span data-ttu-id="c885c-121">Odkaz na vaši by měl vypadat jako následující hello: ![hello "Connect tooServer" dialogové okno](./media/storage-how-to-use-files-mac/mount-via-finder-2.png)</span><span class="sxs-lookup"><span data-stu-id="c885c-121">Your link should look like hello following: ![hello "Connect tooServer" dialog](./media/storage-how-to-use-files-mac/mount-via-finder-2.png)</span></span>

3. <span data-ttu-id="c885c-122">**Použití hello sdílené složky a jeho název klíč účtu po zobrazení výzvy k zadání uživatelského jména a hesla**: Po kliknutí na tlačítko "Připojit" v dialogovém okně "Connect tooServer" hello, zobrazí se výzva k hello uživatelského jména a hesla (to se stane autopopulated s vašeho systému macOS uživatelské jméno).</span><span class="sxs-lookup"><span data-stu-id="c885c-122">**Use hello share name and storage account key when prompted for a username and password**: When you click "Connect" on hello "Connect tooServer" dialog, you will be prompted for hello username and password (This will be autopopulated with your macOS username).</span></span> <span data-ttu-id="c885c-123">Máte možnost hello umístění klíč účtu úložiště název sdílené složky hello do vašeho systému macOS řetězce klíčů.</span><span class="sxs-lookup"><span data-stu-id="c885c-123">You have hello option of placing hello share name/storage account key in your macOS Keychain.</span></span>

4. <span data-ttu-id="c885c-124">**Použijte sdílenou složku Azure File hello podle potřeby**: po nahrazení hello sdílené složky a jeho název klíč účtu v hello uživatelské jméno a heslo, se připojí hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="c885c-124">**Use hello Azure File share as desired**: After substituting hello share name and storage account key in for hello username and password, hello share will be mounted.</span></span> <span data-ttu-id="c885c-125">Můžete to použít jako běžně používáte místní složky nebo sdílené složky, včetně přetahování souborů do sdílené složky hello:</span><span class="sxs-lookup"><span data-stu-id="c885c-125">You may use this as you would normally use a local folder/file share, including dragging and dropping files into hello file share:</span></span>

    ![Snímek připojené sdílené složky Azure](./media/storage-how-to-use-files-mac/mount-via-finder-3.png)

## <a name="mount-an-azure-file-share-via-terminal"></a><span data-ttu-id="c885c-127">Připojení sdílené složky Azure přes Terminál</span><span class="sxs-lookup"><span data-stu-id="c885c-127">Mount an Azure File share via Terminal</span></span>
1. <span data-ttu-id="c885c-128">Nahraďte `<storage-account-name>` hello název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c885c-128">Replace `<storage-account-name>` with hello name of your storage account.</span></span> <span data-ttu-id="c885c-129">Po zobrazení výzvy zadejte klíč účtu úložiště a heslo.</span><span class="sxs-lookup"><span data-stu-id="c885c-129">Provide Storage Account Key as password when prompted.</span></span> 

    ```
    mount_smbfs //<storage-account-name>@<storage-account-name>.file.core.windows.net/<share-name> <desired-mount-point>
    ```

2. <span data-ttu-id="c885c-130">**Použijte sdílenou složku Azure File hello podle potřeby**: sdílenou složku Azure File hello se připojí na hello přípojného bodu určeného hello předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="c885c-130">**Use hello Azure File share as desired**: hello Azure File share will be mounted at hello mount point specified by hello previous command.</span></span>  

    ![Snímek hello připojit sdílenou složku Azure File](./media/storage-how-to-use-files-mac/mount-via-terminal-1.png)

## <a name="next-steps"></a><span data-ttu-id="c885c-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c885c-132">Next steps</span></span>
<span data-ttu-id="c885c-133">Další informace o úložišti Azure File jsou dostupné na těchto odkazech.</span><span class="sxs-lookup"><span data-stu-id="c885c-133">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="c885c-134">Článek podpory od společnosti Apple - jak tooconnect s sdílení souborů na počítači Mac</span><span class="sxs-lookup"><span data-stu-id="c885c-134">Apple Support Article - How tooconnect with File Sharing on your Mac</span></span>](https://support.apple.com/HT204445)
* [<span data-ttu-id="c885c-135">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="c885c-135">FAQ</span></span>](../storage-files-faq.md)
* [<span data-ttu-id="c885c-136">Řešení potíží ve Windows</span><span class="sxs-lookup"><span data-stu-id="c885c-136">Troubleshooting on Windows</span></span>](storage-troubleshoot-windows-file-connection-problems.md)      
* [<span data-ttu-id="c885c-137">Řešení potíží v Linuxu</span><span class="sxs-lookup"><span data-stu-id="c885c-137">Troubleshooting on Linux</span></span>](storage-troubleshoot-linux-file-connection-problems.md)    