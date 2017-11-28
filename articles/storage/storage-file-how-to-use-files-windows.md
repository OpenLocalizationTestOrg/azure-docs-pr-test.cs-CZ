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
# <a name="mount-an-azure-file-share-and-access-hello-share-in-windows"></a><span data-ttu-id="d26dc-103">Připojit Azure sdílené složky a sdílené složky hello přístup v systému Windows</span><span class="sxs-lookup"><span data-stu-id="d26dc-103">Mount an Azure File share and access hello share in Windows</span></span>
<span data-ttu-id="d26dc-104">[Úložiště Azure File](storage-dotnet-how-to-use-files.md) je systém souborů cloudu snadno toouse společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d26dc-104">[Azure File storage](storage-dotnet-how-to-use-files.md) is Microsoft's easy toouse cloud file system.</span></span> <span data-ttu-id="d26dc-105">Sdílené složky Azure je možné připojit v systémech Windows a Windows Server.</span><span class="sxs-lookup"><span data-stu-id="d26dc-105">Azure File shares can be mounted in Windows and Windows Server.</span></span> <span data-ttu-id="d26dc-106">Tento článek ukazuje tři různé způsoby toomount Azure sdílené složky v systému Windows: s hello uživatelské rozhraní Průzkumníka souborů pomocí prostředí PowerShell a prostřednictvím hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d26dc-106">This article shows three different ways toomount an Azure File share on Windows: with hello File Explorer UI, via PowerShell, and via hello Command Prompt.</span></span> 

<span data-ttu-id="d26dc-107">V pořadí toomount Azure File sdílet mimo hello oblast Azure, které je umístěn v, jako jsou místní nebo v jiné oblasti Azure hello operačního systému musí podporovat SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="d26dc-107">In order toomount an Azure File share outside of hello Azure region it is hosted in, such as on-premises or in a different Azure region, hello OS must support SMB 3.0.</span></span> 

<span data-ttu-id="d26dc-108">V závislosti na verzi operačního systému je možné sdílenou složku Azure připojit na počítači se systémem Windows v místním prostředí nebo na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="d26dc-108">Azure File share can be mounted on Windows machine either on-premises or in Azure VM depending on OS version.</span></span> <span data-ttu-id="d26dc-109">Níže uvedená tabulka ukazuje hello</span><span class="sxs-lookup"><span data-stu-id="d26dc-109">Below table illustrates hello</span></span> 

| <span data-ttu-id="d26dc-110">Verze systému Windows</span><span class="sxs-lookup"><span data-stu-id="d26dc-110">Windows Version</span></span>        | <span data-ttu-id="d26dc-111">Verze protokolu SMB</span><span class="sxs-lookup"><span data-stu-id="d26dc-111">SMB Version</span></span> |<span data-ttu-id="d26dc-112">Možnost připojit na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="d26dc-112">Mountable On Azure VM</span></span>|<span data-ttu-id="d26dc-113">Možnost připojit v místním prostředí</span><span class="sxs-lookup"><span data-stu-id="d26dc-113">Mountable On-Premise</span></span>|
|------------------------|-------------|---------------------|---------------------|
| <span data-ttu-id="d26dc-114">Windows 7</span><span class="sxs-lookup"><span data-stu-id="d26dc-114">Windows 7</span></span>              | <span data-ttu-id="d26dc-115">SMB 2.1</span><span class="sxs-lookup"><span data-stu-id="d26dc-115">SMB 2.1</span></span>     | <span data-ttu-id="d26dc-116">Ano</span><span class="sxs-lookup"><span data-stu-id="d26dc-116">Yes</span></span>                 | <span data-ttu-id="d26dc-117">Ne</span><span class="sxs-lookup"><span data-stu-id="d26dc-117">No</span></span>                  |
| <span data-ttu-id="d26dc-118">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="d26dc-118">Windows Server 2008 R2</span></span> | <span data-ttu-id="d26dc-119">SMB 2.1</span><span class="sxs-lookup"><span data-stu-id="d26dc-119">SMB 2.1</span></span>     | <span data-ttu-id="d26dc-120">Ano</span><span class="sxs-lookup"><span data-stu-id="d26dc-120">Yes</span></span>                 | <span data-ttu-id="d26dc-121">Ne</span><span class="sxs-lookup"><span data-stu-id="d26dc-121">No</span></span>                  |
| <span data-ttu-id="d26dc-122">Windows 8</span><span class="sxs-lookup"><span data-stu-id="d26dc-122">Windows 8</span></span>              | <span data-ttu-id="d26dc-123">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="d26dc-123">SMB 3.0</span></span>     | <span data-ttu-id="d26dc-124">Ano</span><span class="sxs-lookup"><span data-stu-id="d26dc-124">Yes</span></span>                 | <span data-ttu-id="d26dc-125">Ano</span><span class="sxs-lookup"><span data-stu-id="d26dc-125">Yes</span></span>                 |
| <span data-ttu-id="d26dc-126">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="d26dc-126">Windows Server 2012</span></span>    | <span data-ttu-id="d26dc-127">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="d26dc-127">SMB 3.0</span></span>     | <span data-ttu-id="d26dc-128">Ano</span><span class="sxs-lookup"><span data-stu-id="d26dc-128">Yes</span></span>                 | <span data-ttu-id="d26dc-129">Ano</span><span class="sxs-lookup"><span data-stu-id="d26dc-129">Yes</span></span>                 |
| <span data-ttu-id="d26dc-130">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="d26dc-130">Windows Server 2012 R2</span></span> | <span data-ttu-id="d26dc-131">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="d26dc-131">SMB 3.0</span></span>     | <span data-ttu-id="d26dc-132">Ano</span><span class="sxs-lookup"><span data-stu-id="d26dc-132">Yes</span></span>                 | <span data-ttu-id="d26dc-133">Ano</span><span class="sxs-lookup"><span data-stu-id="d26dc-133">Yes</span></span>                 |
| <span data-ttu-id="d26dc-134">Windows 10</span><span class="sxs-lookup"><span data-stu-id="d26dc-134">Windows 10</span></span>             | <span data-ttu-id="d26dc-135">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="d26dc-135">SMB 3.0</span></span>     | <span data-ttu-id="d26dc-136">Ano</span><span class="sxs-lookup"><span data-stu-id="d26dc-136">Yes</span></span>                 | <span data-ttu-id="d26dc-137">Ano</span><span class="sxs-lookup"><span data-stu-id="d26dc-137">Yes</span></span>                 |

> [!Note]  
> <span data-ttu-id="d26dc-138">Doporučujeme, aby pořízení hello nejnovější KB pro vaši verzi systému Windows.</span><span class="sxs-lookup"><span data-stu-id="d26dc-138">We always recommend taking hello most recent KB for your version of Windows.</span></span>

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a><span data-ttu-id="d26dc-139"></a>Požadavky pro připojení sdílené složky Azure v systému Windows</span><span class="sxs-lookup"><span data-stu-id="d26dc-139"></a>Prerequisites for Mounting Azure File Share with Windows</span></span> 
* <span data-ttu-id="d26dc-140">**Název účtu úložiště**: sdílenou složku Azure File toomount, bude nutné hello název účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="d26dc-140">**Storage Account Name**: toomount an Azure File share, you will need hello name of hello storage account.</span></span>

* <span data-ttu-id="d26dc-141">**Klíč účtu úložiště**: sdílenou složku Azure File toomount, bude nutné hello klíč primární (nebo sekundární) úložiště.</span><span class="sxs-lookup"><span data-stu-id="d26dc-141">**Storage Account Key**: toomount an Azure File share, you will need hello primary (or secondary) storage key.</span></span> <span data-ttu-id="d26dc-142">Klíče SAS aktuálně nejsou pro připojení podporovány.</span><span class="sxs-lookup"><span data-stu-id="d26dc-142">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="d26dc-143">**Ujistěte se, že je otevřený port 445:** Azure File Storage používá protokol SMB.</span><span class="sxs-lookup"><span data-stu-id="d26dc-143">**Ensure port 445 is open**: Azure File storage uses SMB protocol.</span></span> <span data-ttu-id="d26dc-144">Toosee SMB komunikuje přes port TCP 445 - zkontrolujte, zda brána firewall neblokuje porty TCP 445 z klientského počítače.</span><span class="sxs-lookup"><span data-stu-id="d26dc-144">SMB communicates over TCP port 445 - check toosee if your firewall is not blocking TCP ports 445 from client machine.</span></span>

## <a name="mount-hello-azure-file-share-with-file-explorer"></a><span data-ttu-id="d26dc-145">Připojit sdílenou složku Azure File hello pomocí Průzkumníka souborů</span><span class="sxs-lookup"><span data-stu-id="d26dc-145">Mount hello Azure File share with File Explorer</span></span>
> [!Note]  
> <span data-ttu-id="d26dc-146">Všimněte si, že hello následující pokyny se zobrazí na Windows 10 a mohou poněkud lišit na starší verze.</span><span class="sxs-lookup"><span data-stu-id="d26dc-146">Note that hello following instructions are shown on Windows 10 and may differ slightly on older releases.</span></span> 

1. <span data-ttu-id="d26dc-147">**Otevřete Průzkumníka souborů**: To lze provést otevírání z hello nabídce Start, nebo stiskněte Win + E zástupce.</span><span class="sxs-lookup"><span data-stu-id="d26dc-147">**Open File Explorer**: This can be done by opening from hello Start Menu, or by pressing Win+E shortcut.</span></span>

2. <span data-ttu-id="d26dc-148">**Přejděte toohello položku "Tento počítač" na levé straně hello okna hello. Tím se změní hello nabídky pásu karet hello k dispozici. V nabídce hello počítač, vyberte možnost "Připojit síťovou jednotku"**.</span><span class="sxs-lookup"><span data-stu-id="d26dc-148">**Navigate toohello "This PC" item on hello left-hand side of hello window. This will change hello menus available in hello ribbon. Under hello Computer menu, select "Map Network Drive"**.</span></span>
    
    ![Snímek obrazovky hello "Připojit síťovou jednotku" rozevírací nabídky](media/storage-file-how-to-use-files-windows/1_MountOnWindows10.png)

3. <span data-ttu-id="d26dc-150">**Cesta UNC hello kopírování z podokna "Připojit" hello v hello portál Azure**: podrobný popis jak toofind tyto informace můžete najít [zde](storage-file-how-to-use-files-portal.md#connect-to-file-share).</span><span class="sxs-lookup"><span data-stu-id="d26dc-150">**Copy hello UNC path from hello "Connect" pane in hello Azure portal**: A detailed description of how toofind this information can be found [here](storage-file-how-to-use-files-portal.md#connect-to-file-share).</span></span>

    ![cesta UNC Hello hello Azure File storage připojit podokně](media/storage-file-how-to-use-files-windows/portal_netuse_connect.png)

4. <span data-ttu-id="d26dc-152">**Vyberte písmeno jednotky hello a zadejte cestu UNC hello.**</span><span class="sxs-lookup"><span data-stu-id="d26dc-152">**Select hello Drive letter and enter hello UNC path.**</span></span> 
    
    ![Snímek obrazovky dialogového okna "Připojit síťovou jednotku" hello](media/storage-file-how-to-use-files-windows/2_MountOnWindows10.png)

5. <span data-ttu-id="d26dc-154">**Použití hello název účtu úložiště se přidá jako předpona `Azure\` jako uživatelské jméno hello a klíč účtu úložiště jako hello heslo.**</span><span class="sxs-lookup"><span data-stu-id="d26dc-154">**Use hello Storage Account Name prepended with `Azure\` as hello username and a Storage Account Key as hello password.**</span></span>
    
    ![Snímek obrazovky dialogu pro přihlašovací údaje hello sítě](media/storage-file-how-to-use-files-windows/3_MountOnWindows10.png)

6. <span data-ttu-id="d26dc-156">**Používejte sdílenou složku Azure, jak potřebujete**.</span><span class="sxs-lookup"><span data-stu-id="d26dc-156">**Use Azure File share as desired**.</span></span>
    
    ![Sdílená složka Azure je teď připojená](media/storage-file-how-to-use-files-windows/4_MountOnWindows10.png)

7. <span data-ttu-id="d26dc-158">**Když jsou připravené toodismount (nebo odpojit) hello Azure sdílené složky, můžete tak učinit kliknete pravým tlačítkem na položku hello hello sdílené složky v části hello "umístění v síti" v Průzkumníku souborů a výběrem "Odpojení"**.</span><span class="sxs-lookup"><span data-stu-id="d26dc-158">**When you are ready toodismount (or disconnect) hello Azure File share, you can do so by right clicking on hello entry for hello share under hello "Network locations" in File Explorer and selecting "Disconnect"**.</span></span>

## <a name="mount-hello-azure-file-share-with-powershell"></a><span data-ttu-id="d26dc-159">Připojit sdílenou složku Azure File hello pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d26dc-159">Mount hello Azure File share with PowerShell</span></span>
1. <span data-ttu-id="d26dc-160">**Použití hello následující příkaz sdílenou složku Azure File hello toomount**: Mějte na paměti, tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` hello správné informace.</span><span class="sxs-lookup"><span data-stu-id="d26dc-160">**Use hello following command toomount hello Azure File share**: Remember tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` with hello proper information.</span></span>

    ```PowerShell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\<share-name>" -Credential $credential
    ```

2. <span data-ttu-id="d26dc-161">**Použijte sdílenou složku Azure File hello podle potřeby**.</span><span class="sxs-lookup"><span data-stu-id="d26dc-161">**Use hello Azure File share as desired**.</span></span>

3. <span data-ttu-id="d26dc-162">**Jakmile budete hotovi, odpojte hello Azure sdílenou složku pomocí hello následující příkaz**.</span><span class="sxs-lookup"><span data-stu-id="d26dc-162">**When you are finished, dismount hello Azure File share using hello following command**.</span></span>

    ```PowerShell
    Remove-PSDrive -Name <desired-drive-letter>
    ```

> [!Note]  
> <span data-ttu-id="d26dc-163">Můžete použít hello `-Persist` parametr na `New-PSDrive` toomake hello zbytku viditelné toohello sdílenou složku Azure File hello operačního systému při připojené.</span><span class="sxs-lookup"><span data-stu-id="d26dc-163">You may use hello `-Persist` parameter on `New-PSDrive` toomake hello Azure File share visible toohello rest of hello OS while mounted.</span></span>

## <a name="mount-hello-azure-file-share-with-command-prompt"></a><span data-ttu-id="d26dc-164">Připojit sdílenou složku Azure File hello pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="d26dc-164">Mount hello Azure File share with Command Prompt</span></span>
1. <span data-ttu-id="d26dc-165">**Použití hello následující příkaz sdílenou složku Azure File hello toomount**: Mějte na paměti, tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` hello správné informace.</span><span class="sxs-lookup"><span data-stu-id="d26dc-165">**Use hello following command toomount hello Azure File share**: Remember tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` with hello proper information.</span></span>

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. <span data-ttu-id="d26dc-166">**Použijte sdílenou složku Azure File hello podle potřeby**.</span><span class="sxs-lookup"><span data-stu-id="d26dc-166">**Use hello Azure File share as desired**.</span></span>

3. <span data-ttu-id="d26dc-167">**Jakmile budete hotovi, odpojte hello Azure sdílenou složku pomocí hello následující příkaz**.</span><span class="sxs-lookup"><span data-stu-id="d26dc-167">**When you are finished, dismount hello Azure File share using hello following command**.</span></span>

    ```
    net use <desired-drive-letter>: /delete
    ```

> [!Note]  
> <span data-ttu-id="d26dc-168">Můžete nakonfigurovat hello Azure File znovu připojit ke sdílené složce tooautomatically restartování zachování hello přihlašovacích údajů v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="d26dc-168">You can configure hello Azure File share tooautomatically reconnect on reboot by persisting hello credentials in Windows.</span></span> <span data-ttu-id="d26dc-169">Následující příkaz Hello zachová hello přihlašovací údaje:</span><span class="sxs-lookup"><span data-stu-id="d26dc-169">hello following command will persist hello credentials:</span></span>
>   ```
>   cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>
>   ```

## <a name="next-steps"></a><span data-ttu-id="d26dc-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d26dc-170">Next steps</span></span>
<span data-ttu-id="d26dc-171">Další informace o úložišti Azure File jsou dostupné na těchto odkazech.</span><span class="sxs-lookup"><span data-stu-id="d26dc-171">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="d26dc-172">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="d26dc-172">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="d26dc-173">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="d26dc-173">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a><span data-ttu-id="d26dc-174">Koncepční články a videa</span><span class="sxs-lookup"><span data-stu-id="d26dc-174">Conceptual articles and videos</span></span>
* [<span data-ttu-id="d26dc-175">Azure File Storage: hladký cloudový souborový systém SMB pro Windows a Linux</span><span class="sxs-lookup"><span data-stu-id="d26dc-175">Azure File storage: a frictionless cloud SMB file system for Windows and Linux</span></span>](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [<span data-ttu-id="d26dc-176">Jak toouse Azure File storage s Linuxem</span><span class="sxs-lookup"><span data-stu-id="d26dc-176">How toouse Azure File storage with Linux</span></span>](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-file-storage"></a><span data-ttu-id="d26dc-177">Podpora nástrojů pro službu Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="d26dc-177">Tooling support for Azure File storage</span></span>
* [<span data-ttu-id="d26dc-178">Použití Azure Powershell s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d26dc-178">Using Azure PowerShell with Azure Storage</span></span>](storage-powershell-guide-full.md)
* [<span data-ttu-id="d26dc-179">Jak toouse AzCopy s Microsoft Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d26dc-179">How toouse AzCopy with Microsoft Azure Storage</span></span>](storage-use-azcopy.md)
* [<span data-ttu-id="d26dc-180">Použití hello rozhraní příkazového řádku Azure s Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d26dc-180">Using hello Azure CLI with Azure Storage</span></span>](storage-azure-cli.md#create-and-manage-file-shares)
* [<span data-ttu-id="d26dc-181">Řešení potíží se službou Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="d26dc-181">Troubleshooting Azure File storage problems</span></span>](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="blog-posts"></a><span data-ttu-id="d26dc-182">Příspěvky na blozích</span><span class="sxs-lookup"><span data-stu-id="d26dc-182">Blog posts</span></span>
* [<span data-ttu-id="d26dc-183">Úložiště Azure File je nyní dostupné pro veřejnost</span><span class="sxs-lookup"><span data-stu-id="d26dc-183">Azure File storage is now generally available</span></span>](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [<span data-ttu-id="d26dc-184">Uvnitř Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="d26dc-184">Inside Azure File storage</span></span>](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [<span data-ttu-id="d26dc-185">Představujeme službu Microsoft Azure File</span><span class="sxs-lookup"><span data-stu-id="d26dc-185">Introducing Microsoft Azure File Service</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [<span data-ttu-id="d26dc-186">Migrace dat tooAzure souboru</span><span class="sxs-lookup"><span data-stu-id="d26dc-186">Migrating data tooAzure File </span></span>](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a><span data-ttu-id="d26dc-187">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="d26dc-187">Reference</span></span>
* [<span data-ttu-id="d26dc-188">Klientská knihovna Storage pro .NET – referenční informace</span><span class="sxs-lookup"><span data-stu-id="d26dc-188">Storage Client Library for .NET reference</span></span>](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [<span data-ttu-id="d26dc-189">REST API služby File – referenční informace</span><span class="sxs-lookup"><span data-stu-id="d26dc-189">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)
