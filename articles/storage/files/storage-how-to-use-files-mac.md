---
title: "Připojení sdílené složky Azure přes protokol SMB v systému macOS | Dokumentace Microsoftu"
description: "Zjistěte, jak připojit sdílenou složku Azure přes protokol SMB v systému macOS."
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
ms.openlocfilehash: bc3d67745afb8bbffe7ec3462e995104daff9632
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="mount-azure-file-share-over-smb-with-macos"></a><span data-ttu-id="ddcf5-103">Připojení sdílené složky Azure přes protokol SMB v systému macOS</span><span class="sxs-lookup"><span data-stu-id="ddcf5-103">Mount Azure File share over SMB with macOS</span></span>
<span data-ttu-id="ddcf5-104">[Azure File Storage](../storage-dotnet-how-to-use-files.md) je služba Microsoftu, která umožňuje vytváření a používání sdílených složek souborů sítě v Azure s využitím oborových standardů.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-104">[Azure File storage](../storage-dotnet-how-to-use-files.md) is Microsoft's service that enables you to create and use network file shares in the Azure using the industry standard.</span></span> <span data-ttu-id="ddcf5-105">Sdílené složky Azure je možné připojit v systémech macOS Sierra (10.12) a El Capitan (10.11).</span><span class="sxs-lookup"><span data-stu-id="ddcf5-105">Azure File shares can be mounted in macOS Sierra (10.12) and El Capitan (10.11).</span></span> <span data-ttu-id="ddcf5-106">Tento článek ukazuje dva různé způsoby připojení sdílené složky Azure v systému macOS – pomocí uživatelského rozhraní Finder a pomocí Terminálu.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-106">This article shows two different ways to mount an Azure File share on macOS with the Finder UI and using the Terminal.</span></span>

> [!Note]  
> <span data-ttu-id="ddcf5-107">Před připojením sdílené složky Azure přes protokol SMB doporučujeme zakázat podepisování paketů SMB.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-107">Before mounting an Azure File share over SMB, we recommend disabling SMB packet signing.</span></span> <span data-ttu-id="ddcf5-108">Pokud to neuděláte, můžete při přistupování ke sdílené složce Azure ze systému macOS dosahovat nízkého výkonu.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-108">Not doing so may yield poor performance when accessing the Azure File share from macOS.</span></span> <span data-ttu-id="ddcf5-109">Připojení SMB bude šifrované, takže to nijak neovlivní zabezpečení vašeho připojení.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-109">Your SMB connection will be encrypted, so this does not affect the security of your connection.</span></span> <span data-ttu-id="ddcf5-110">Podepisování paketů SMB zakážete spuštěním následujících příkazů v Terminálu, jak je popsáno v tomto článku [Apple support article on disabling SMB packet signing](https://support.apple.com/HT205926) (Článek podpory Apple o zakázání podepisování paketů SMB):</span><span class="sxs-lookup"><span data-stu-id="ddcf5-110">From the terminal, the following commands will disable SMB packet signing, as described by this [Apple support article on disabling SMB packet signing](https://support.apple.com/HT205926):</span></span>  
>    ```
>    sudo -s
>    echo "[default]" >> /etc/nsmb.conf
>    echo "signing_required=no" >> /etc/nsmb.conf
>    exit
>    ```

## <a name="prerequisites-for-mounting-an-azure-file-share-on-macos"></a><span data-ttu-id="ddcf5-111">Požadavky pro připojení sdílené složky Azure v systému macOS</span><span class="sxs-lookup"><span data-stu-id="ddcf5-111">Prerequisites for mounting an Azure File share on macOS</span></span>
* <span data-ttu-id="ddcf5-112">**Název účtu úložiště:** Pro připojení sdílené složky Azure budete potřebovat název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-112">**Storage account name**: To mount an Azure File share, you will need the name of the storage account.</span></span>

* <span data-ttu-id="ddcf5-113">**Klíč účtu úložiště:** Pro připojení sdílené složky Azure budete potřebovat primární (nebo sekundární) klíč úložiště.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-113">**Storage account key**: To mount an Azure File share, you will need the primary (or secondary) storage key.</span></span> <span data-ttu-id="ddcf5-114">Klíče SAS aktuálně nejsou pro připojení podporovány.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-114">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="ddcf5-115">**Ujistěte se, že je otevřený port 445:** Protokol SMB komunikuje přes port TCP 445.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-115">**Ensure port 445 is open**: SMB communicates over TCP port 445.</span></span> <span data-ttu-id="ddcf5-116">Na klientském počítači (Mac) zkontrolujte, že brána firewall neblokuje port TCP 445.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-116">On your client machine (the Mac), check to make sure your firewall is not blocking TCP port 445.</span></span>

## <a name="mount-an-azure-file-share-via-finder"></a><span data-ttu-id="ddcf5-117">Připojení sdílené složky Azure přes Finder</span><span class="sxs-lookup"><span data-stu-id="ddcf5-117">Mount an Azure File share via Finder</span></span>
1. <span data-ttu-id="ddcf5-118">**Otevřete Finder:** Finder je v systému macOS otevřený standardně, ale můžete se ujistit, že je aktuálně vybranou aplikací, kliknutím na ikonu obličeje macOS v Docku:</span><span class="sxs-lookup"><span data-stu-id="ddcf5-118">**Open Finder**: Finder is open on macOS by default, but you can ensure it is the currently selected application by clicking the "macOS face icon" on the dock:</span></span>  
    <span data-ttu-id="ddcf5-119">![Ikona obličeje macOS](./media/storage-how-to-use-files-mac/mount-via-finder-1.png)</span><span class="sxs-lookup"><span data-stu-id="ddcf5-119">![The macOS face icon](./media/storage-how-to-use-files-mac/mount-via-finder-1.png)</span></span>

2. <span data-ttu-id="ddcf5-120">**Z nabídky Go (Přejít) vyberte Connect to Server (Připojit k serveru):** Použijte cestu UNC z části [Požadavky](#preq) a převeďte počáteční dvojité zpětné lomítko (`\\`) na `smb://` a všechna ostatní zpětná lomítka (`\`) na lomítka (`/`).</span><span class="sxs-lookup"><span data-stu-id="ddcf5-120">**Select "Connect to Server" from the "Go" Menu**: Using the UNC path from the [prerequisites](#preq), convert the beginning double backslash (`\\`) to `smb://` and all other backslashes (`\`) to forwards slashes (`/`).</span></span> <span data-ttu-id="ddcf5-121">Odkaz by měl vypadat následovně: ![Dialogové okno Connect to Server (Připojit k serveru)](./media/storage-how-to-use-files-mac/mount-via-finder-2.png)</span><span class="sxs-lookup"><span data-stu-id="ddcf5-121">Your link should look like the following: ![The "Connect to Server" dialog](./media/storage-how-to-use-files-mac/mount-via-finder-2.png)</span></span>

3. <span data-ttu-id="ddcf5-122">**Po zobrazení výzvy k zadání uživatelského jména a hesla použijte název sdílené složky a klíč úložiště:** Po kliknutí na Connect (Připojit) v dialogovém okně Connect to Server (Připojit k serveru) budete vyzváni k zadání uživatelského jména a hesla (automaticky se vyplní uživatelské jméno macOS).</span><span class="sxs-lookup"><span data-stu-id="ddcf5-122">**Use the share name and storage account key when prompted for a username and password**: When you click "Connect" on the "Connect to Server" dialog, you will be prompted for the username and password (This will be autopopulated with your macOS username).</span></span> <span data-ttu-id="ddcf5-123">Máte možnost uložit název sdílené složky a klíč účtu úložiště do klíčenky macOS.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-123">You have the option of placing the share name/storage account key in your macOS Keychain.</span></span>

4. <span data-ttu-id="ddcf5-124">**Používejte sdílenou složku Azure, jak potřebujete:** Po nahrazení uživatelského jména a hesla za název sdílené složky a klíč účtu úložiště se sdílená složka připojí.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-124">**Use the Azure File share as desired**: After substituting the share name and storage account key in for the username and password, the share will be mounted.</span></span> <span data-ttu-id="ddcf5-125">Můžete ji používat stejně, jako běžně používáte místní složky nebo sdílené složky, včetně přetahování souborů do sdílené složky:</span><span class="sxs-lookup"><span data-stu-id="ddcf5-125">You may use this as you would normally use a local folder/file share, including dragging and dropping files into the file share:</span></span>

    ![Snímek připojené sdílené složky Azure](./media/storage-how-to-use-files-mac/mount-via-finder-3.png)

## <a name="mount-an-azure-file-share-via-terminal"></a><span data-ttu-id="ddcf5-127">Připojení sdílené složky Azure přes Terminál</span><span class="sxs-lookup"><span data-stu-id="ddcf5-127">Mount an Azure File share via Terminal</span></span>
1. <span data-ttu-id="ddcf5-128">Nahraďte `<storage-account-name>` názvem vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-128">Replace `<storage-account-name>` with the name of your storage account.</span></span> <span data-ttu-id="ddcf5-129">Po zobrazení výzvy zadejte klíč účtu úložiště a heslo.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-129">Provide Storage Account Key as password when prompted.</span></span> 

    ```
    mount_smbfs //<storage-account-name>@<storage-account-name>.file.core.windows.net/<share-name> <desired-mount-point>
    ```

2. <span data-ttu-id="ddcf5-130">**Používejte sdílenou složku Azure, jak potřebujete:** Sdílená složka Azure se připojí na přípojný bod zadaný v předchozím příkazu.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-130">**Use the Azure File share as desired**: The Azure File share will be mounted at the mount point specified by the previous command.</span></span>  

    ![Snímek připojené sdílené složky Azure](./media/storage-how-to-use-files-mac/mount-via-terminal-1.png)

## <a name="next-steps"></a><span data-ttu-id="ddcf5-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ddcf5-132">Next steps</span></span>
<span data-ttu-id="ddcf5-133">Další informace o úložišti Azure File jsou dostupné na těchto odkazech.</span><span class="sxs-lookup"><span data-stu-id="ddcf5-133">See these links for more information about Azure File storage.</span></span>

* <span data-ttu-id="ddcf5-134">[Apple Support Article – How to connect with File Sharing on your Mac](https://support.apple.com/HT204445) (Článek podpory Apple – Jak se na Macu připojit pomocí Sdílení souborů)</span><span class="sxs-lookup"><span data-stu-id="ddcf5-134">[Apple Support Article - How to connect with File Sharing on your Mac](https://support.apple.com/HT204445)</span></span>
* [<span data-ttu-id="ddcf5-135">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="ddcf5-135">FAQ</span></span>](../storage-files-faq.md)
* [<span data-ttu-id="ddcf5-136">Řešení potíží ve Windows</span><span class="sxs-lookup"><span data-stu-id="ddcf5-136">Troubleshooting on Windows</span></span>](storage-troubleshoot-windows-file-connection-problems.md)      
* [<span data-ttu-id="ddcf5-137">Řešení potíží v Linuxu</span><span class="sxs-lookup"><span data-stu-id="ddcf5-137">Troubleshooting on Linux</span></span>](storage-troubleshoot-linux-file-connection-problems.md)    