---
title: "Poznámky k verzi Microsoft Azure Storage Explorer (Preview) | Microsoft Docs"
description: "Poznámky k verzi pro Microsoft Azure Storage Explorer (Preview)"
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.devlang: multiple
ms.topic: release-notes
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: cawa
ms.openlocfilehash: 63a24f6b153390533bba0888fd1051508c65bf6e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="microsoft-azure-storage-explorer-preview-release-notes"></a><span data-ttu-id="7b9b7-103">Poznámky k verzi Microsoft Azure Storage Explorer (Preview)</span><span class="sxs-lookup"><span data-stu-id="7b9b7-103">Microsoft Azure Storage Explorer (Preview) release notes</span></span>

<span data-ttu-id="7b9b7-104">Tento článek obsahuje verze, kterou verzi poznámky pro Azure Storage Explorer 0.8.16 (Preview), a také poznámky k verzi pro předchozí verze.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-104">This article contains the release notes for Azure Storage Explorer 0.8.16 (Preview) release, as well as release notes for previous versions.</span></span>

<span data-ttu-id="7b9b7-105">[Microsoft Azure Storage Explorer (Preview)](./vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, která umožňuje snadno pracovat s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-105">[Microsoft Azure Storage Explorer (Preview)](./vs-azure-tools-storage-manage-with-storage-explorer.md) is a standalone app that enables you to easily work with Azure Storage data on Windows, macOS, and Linux.</span></span>

## <a name="version-0816-preview"></a><span data-ttu-id="7b9b7-106">Verze 0.8.16 (Preview)</span><span class="sxs-lookup"><span data-stu-id="7b9b7-106">Version 0.8.16 (Preview)</span></span>
<span data-ttu-id="7b9b7-107">8/21/2017</span><span class="sxs-lookup"><span data-stu-id="7b9b7-107">8/21/2017</span></span>

### <a name="download-azure-storage-explorer-0816-preview"></a><span data-ttu-id="7b9b7-108">Stažení Azure Storage Explorer 0.8.16 (Preview)</span><span class="sxs-lookup"><span data-stu-id="7b9b7-108">Download Azure Storage Explorer 0.8.16 (Preview)</span></span>
- [<span data-ttu-id="7b9b7-109">Azure Storage Explorer (Preview) 0.8.16 pro Windows</span><span class="sxs-lookup"><span data-stu-id="7b9b7-109">Azure Storage Explorer 0.8.16 (Preview) for Windows</span></span>](https://go.microsoft.com/fwlink/?LinkId=708343)
- [<span data-ttu-id="7b9b7-110">Azure Storage Explorer (Preview) 0.8.16 pro Mac</span><span class="sxs-lookup"><span data-stu-id="7b9b7-110">Azure Storage Explorer 0.8.16 (Preview) for Mac</span></span>](https://go.microsoft.com/fwlink/?LinkId=708342)
- [<span data-ttu-id="7b9b7-111">Azure Storage Explorer (Preview) 0.8.16 pro Linux</span><span class="sxs-lookup"><span data-stu-id="7b9b7-111">Azure Storage Explorer 0.8.16 (Preview) for Linux</span></span>](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a><span data-ttu-id="7b9b7-112">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-112">New</span></span>
* <span data-ttu-id="7b9b7-113">Při otevření objektu blob Storage Explorer zobrazí výzvu k nahrání stažený soubor, pokud je zjištěna změna</span><span class="sxs-lookup"><span data-stu-id="7b9b7-113">When you open a blob, Storage Explorer will prompt you to upload the downloaded file if a change is detected</span></span>
* <span data-ttu-id="7b9b7-114">Rozšířené zásobník Azure přihlašování uživatelů</span><span class="sxs-lookup"><span data-stu-id="7b9b7-114">Enhanced Azure Stack sign-in experience</span></span>
* <span data-ttu-id="7b9b7-115">Zvýšení výkonu nahrávání nebo stahování mnoho malých souborů ve stejnou dobu</span><span class="sxs-lookup"><span data-stu-id="7b9b7-115">Improved the performance of uploading/downloading many small files at the same time</span></span>


### <a name="fixes"></a><span data-ttu-id="7b9b7-116">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-116">Fixes</span></span>
* <span data-ttu-id="7b9b7-117">Pro některé typy objektů blob rozhodnete "replace" při nahrávání došlo ke konfliktu někdy výsledkem by nahrávání Probíhá restartování.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-117">For some blob types, choosing to "replace" during an upload conflict would sometimes result in the upload being restarted.</span></span> 
* <span data-ttu-id="7b9b7-118">Ve verzi 0.8.15 by někdy nahrávání stalace v 99 %.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-118">In version 0.8.15, uploads would sometimes stall at 99%.</span></span>
* <span data-ttu-id="7b9b7-119">Při nahrávání souborů do sdílené složky, pokud jste se rozhodli odeslat do adresáře, která nebyla ještě neexistuje, dojde k selhání odeslání.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-119">When uploading files to a file share, if you chose to upload to a directory which did not yet exist, your upload would fail.</span></span>
* <span data-ttu-id="7b9b7-120">Storage Explorer byla nesprávně generování časová razítka pro sdílené přístupové podpisy a tabulku.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-120">Storage Explorer was incorrectly generating time stamps for shared access signatures and table queries.</span></span>


<span data-ttu-id="7b9b7-121">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-121">Known Issues</span></span>
* <span data-ttu-id="7b9b7-122">Pomocí názvu a klíče připojovací řetězec momentálně nefunguje.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-122">Using a name and key connection string does not currently work.</span></span> <span data-ttu-id="7b9b7-123">Ho bude vyřešený v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-123">It will be fixed in the next release.</span></span> <span data-ttu-id="7b9b7-124">Do té doby můžete připojit pomocí názvu a klíče.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-124">Until then you can use attach with name and key.</span></span>
* <span data-ttu-id="7b9b7-125">Pokud se pokusíte otevřít soubor s neplatným názvem souboru systému Windows, povede ke stažení souboru nebyla nalezena chyba.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-125">If you try to open a file with an invalid Windows file name, the download will result in a file not found error.</span></span>
* <span data-ttu-id="7b9b7-126">Po kliknutí na tlačítko Storno"na úlohu, může trvat nějakou dobu tuto úlohu zrušit.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-126">After clicking "Cancel" on a task, it may take a while for that task to cancel.</span></span> <span data-ttu-id="7b9b7-127">Jedná se o omezení knihovny uzlu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-127">This is a limitation of the Azure Storage Node library.</span></span>
* <span data-ttu-id="7b9b7-128">Po dokončení nahrávání objektů blob, je aktualizovat na kartě, který inicializován nahrávání.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-128">After completing a blob upload, the tab which initiated the upload is refreshed.</span></span> <span data-ttu-id="7b9b7-129">Toto je liší od předchozí chování a také způsobí přesměrováni zpět do kořenového adresáře, které jsou v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-129">This is a change from previous behavior, and will also cause you to be taken back to the root of the container you are in.</span></span>
* <span data-ttu-id="7b9b7-130">Pokud si zvolíte nesprávný certifikát PIN kód nebo čipová karta, je potřeba restartovat, aby bylo možné používat Storage Explorer zapomněli rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-130">If you choose the wrong PIN/Smartcard certificate, then you will need to restart in order to have Storage Explorer forget that decision.</span></span>
* <span data-ttu-id="7b9b7-131">Panel nastavení účtu může zobrazovat, je potřeba zadat přihlašovací údaje k filtrování odběry.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-131">The account settings panel may show that you need to reenter credentials to filter subscriptions.</span></span>
* <span data-ttu-id="7b9b7-132">Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-132">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="7b9b7-133">Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entity.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-133">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="7b9b7-134">I když zásobník Azure aktuálně nepodporuje sdílených složek, sdílených složek uzel se objeví stále v připojené účtu úložiště Azure zásobníku.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-134">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span>
* <span data-ttu-id="7b9b7-135">Pro uživatele v Ubuntu 14.04, budete muset zajistit RSZ je aktuální – tento krok můžete provést spuštěním následujících příkazů a restartujte svůj počítač:</span><span class="sxs-lookup"><span data-stu-id="7b9b7-135">For users on Ubuntu 14.04, you will need to ensure GCC is up to date - this can be done by running the following commands, and then restarting your machine:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* <span data-ttu-id="7b9b7-136">Pro uživatele v Ubuntu č. 17.04 budete muset nainstalovat GConf – to můžete provést spuštěním následujících příkazů a restartujte svůj počítač:</span><span class="sxs-lookup"><span data-stu-id="7b9b7-136">For users on Ubuntu 17.04, you will need to install GConf - this can be done by running the following commands, and then restarting your machine:</span></span>

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0814-preview"></a><span data-ttu-id="7b9b7-137">Verze 0.8.14 (Preview)</span><span class="sxs-lookup"><span data-stu-id="7b9b7-137">Version 0.8.14 (Preview)</span></span>
<span data-ttu-id="7b9b7-138">06/22/2017</span><span class="sxs-lookup"><span data-stu-id="7b9b7-138">06/22/2017</span></span>

### <a name="download-azure-storage-explorer-0814-preview"></a><span data-ttu-id="7b9b7-139">Stažení Azure Storage Explorer 0.8.14 (Preview)</span><span class="sxs-lookup"><span data-stu-id="7b9b7-139">Download Azure Storage Explorer 0.8.14 (Preview)</span></span>
* [<span data-ttu-id="7b9b7-140">Stažení Azure Storage Explorer (Preview) 0.8.14 pro Windows</span><span class="sxs-lookup"><span data-stu-id="7b9b7-140">Download Azure Storage Explorer 0.8.14 (Preview) for Windows</span></span>](https://go.microsoft.com/fwlink/?LinkId=809306)
* [<span data-ttu-id="7b9b7-141">Stažení Azure Storage Explorer (Preview) 0.8.14 pro Mac</span><span class="sxs-lookup"><span data-stu-id="7b9b7-141">Download Azure Storage Explorer 0.8.14 (Preview) for Mac</span></span>](https://go.microsoft.com/fwlink/?LinkId=809307)
* [<span data-ttu-id="7b9b7-142">Stažení Azure Storage Explorer (Preview) 0.8.14 pro Linux</span><span class="sxs-lookup"><span data-stu-id="7b9b7-142">Download Azure Storage Explorer 0.8.14 (Preview) for Linux</span></span>](https://go.microsoft.com/fwlink/?LinkId=809308)

### <a name="new"></a><span data-ttu-id="7b9b7-143">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-143">New</span></span>

* <span data-ttu-id="7b9b7-144">Aktualizovaná verze elektronovým k 1.7.2 chcete využít několik důležité aktualizace zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7b9b7-144">Updated Electron version to 1.7.2 in order to take advantage of several critical security updates</span></span>
* <span data-ttu-id="7b9b7-145">Nyní můžete rychle k online průvodci odstraňováním potíží z nabídky Nápověda</span><span class="sxs-lookup"><span data-stu-id="7b9b7-145">You can now quickly access the online troubleshooting guide from the help menu</span></span>
* <span data-ttu-id="7b9b7-146">Storage Explorer řešení potíží s [Průvodce][2]</span><span class="sxs-lookup"><span data-stu-id="7b9b7-146">Storage Explorer Troubleshooting [Guide][2]</span></span>
* <span data-ttu-id="7b9b7-147">[Pokyny] [ 3] o připojení k předplatnému Azure zásobníku</span><span class="sxs-lookup"><span data-stu-id="7b9b7-147">[Instructions][3] on connecting to an Azure Stack subscription</span></span>

### <a name="known-issues"></a><span data-ttu-id="7b9b7-148">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-148">Known Issues</span></span>

* <span data-ttu-id="7b9b7-149">Tlačítka v dialogovém okně Odstranit složku potvrzení nezaregistrujete kliknutími myši v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-149">Buttons on the delete folder confirmation dialog don't register with the mouse clicks on Linux.</span></span> <span data-ttu-id="7b9b7-150">Alternativní řešení je použití klávesy Enter</span><span class="sxs-lookup"><span data-stu-id="7b9b7-150">Workaround is to use the Enter key</span></span>
* <span data-ttu-id="7b9b7-151">Pokud zvolíte nesprávný certifikát PIN kód nebo čipová karta pak budete muset restartovat, aby bylo možné používat Storage Explorer zapomněli rozhodnutí</span><span class="sxs-lookup"><span data-stu-id="7b9b7-151">If you choose the wrong PIN/Smartcard certificate then you will need to restart in order to have Storage Explorer forget the decision</span></span>
* <span data-ttu-id="7b9b7-152">S více než 3 skupiny objektů BLOB nebo ve stejnou dobu ukládání souborů může způsobit chyby</span><span class="sxs-lookup"><span data-stu-id="7b9b7-152">Having more than 3 groups of blobs or files uploading at the same time may cause errors</span></span>
* <span data-ttu-id="7b9b7-153">Panel nastavení účtu může zobrazovat, je potřeba zadat přihlašovací údaje, chcete-li filtrovat odběrů</span><span class="sxs-lookup"><span data-stu-id="7b9b7-153">The account settings panel may show that you need to reenter credentials in order to filter subscriptions</span></span>
* <span data-ttu-id="7b9b7-154">Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-154">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="7b9b7-155">Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entity.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-155">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="7b9b7-156">I když zásobník Azure aktuálně nepodporuje sdílené složky, sdílené složky uzel se objeví stále v připojené účtu úložiště Azure zásobníku.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-156">Although Azure Stack doesn't currently support File Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="7b9b7-157">Ubuntu 14.04 instalace musí aktualizovat verzi RSZ nebo upgradovat – kroky pro upgrade jsou níže:</span><span class="sxs-lookup"><span data-stu-id="7b9b7-157">Ubuntu 14.04 install needs gcc version updated or upgraded – steps to upgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```




## <a name="previous-releases"></a><span data-ttu-id="7b9b7-158">Předchozí verze</span><span class="sxs-lookup"><span data-stu-id="7b9b7-158">Previous releases</span></span>

* [<span data-ttu-id="7b9b7-159">Verze 0.8.13</span><span class="sxs-lookup"><span data-stu-id="7b9b7-159">Version 0.8.13</span></span>](#version-0813)
* [<span data-ttu-id="7b9b7-160">Verze 0.8.12 / 0.8.11 / 0.8.10</span><span class="sxs-lookup"><span data-stu-id="7b9b7-160">Version 0.8.12 / 0.8.11 / 0.8.10</span></span>](#version-0812--0811--0810)
* [<span data-ttu-id="7b9b7-161">Verze 0.8.9 nebo 0.8.8</span><span class="sxs-lookup"><span data-stu-id="7b9b7-161">Version 0.8.9 / 0.8.8</span></span>](#version-089--088)
* [<span data-ttu-id="7b9b7-162">Verze 0.8.7</span><span class="sxs-lookup"><span data-stu-id="7b9b7-162">Version 0.8.7</span></span>](#version-087)
* [<span data-ttu-id="7b9b7-163">Verze 0.8.6</span><span class="sxs-lookup"><span data-stu-id="7b9b7-163">Version 0.8.6</span></span>](#version-086)
* [<span data-ttu-id="7b9b7-164">Verze 0.8.5</span><span class="sxs-lookup"><span data-stu-id="7b9b7-164">Version 0.8.5</span></span>](#version-085)
* [<span data-ttu-id="7b9b7-165">Verze 0.8.4</span><span class="sxs-lookup"><span data-stu-id="7b9b7-165">Version 0.8.4</span></span>](#version-084)
* [<span data-ttu-id="7b9b7-166">Verze 0.8.3</span><span class="sxs-lookup"><span data-stu-id="7b9b7-166">Version 0.8.3</span></span>](#version-083)
* [<span data-ttu-id="7b9b7-167">Verze 0.8.2</span><span class="sxs-lookup"><span data-stu-id="7b9b7-167">Version 0.8.2</span></span>](#version-082)
* [<span data-ttu-id="7b9b7-168">Verze 0.8.0</span><span class="sxs-lookup"><span data-stu-id="7b9b7-168">Version 0.8.0</span></span>](#version-080)
* [<span data-ttu-id="7b9b7-169">Verze 0.7.20160509.0</span><span class="sxs-lookup"><span data-stu-id="7b9b7-169">Version 0.7.20160509.0</span></span>](#version-07201605090)
* [<span data-ttu-id="7b9b7-170">Verze 0.7.20160325.0</span><span class="sxs-lookup"><span data-stu-id="7b9b7-170">Version 0.7.20160325.0</span></span>](#version-07201603250)
* [<span data-ttu-id="7b9b7-171">Verze 0.7.20160129.1</span><span class="sxs-lookup"><span data-stu-id="7b9b7-171">Version 0.7.20160129.1</span></span>](#version-07201601291)
* [<span data-ttu-id="7b9b7-172">Verze 0.7.20160105.0</span><span class="sxs-lookup"><span data-stu-id="7b9b7-172">Version 0.7.20160105.0</span></span>](#version-07201601050)
* [<span data-ttu-id="7b9b7-173">Verze 0.7.20151116.0</span><span class="sxs-lookup"><span data-stu-id="7b9b7-173">Version 0.7.20151116.0</span></span>](#version-07201511160)


### <a name="version-0813"></a><span data-ttu-id="7b9b7-174">Verze 0.8.13</span><span class="sxs-lookup"><span data-stu-id="7b9b7-174">Version 0.8.13</span></span>
<span data-ttu-id="7b9b7-175">05/12/2017</span><span class="sxs-lookup"><span data-stu-id="7b9b7-175">05/12/2017</span></span>

#### <a name="new"></a><span data-ttu-id="7b9b7-176">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-176">New</span></span>

* <span data-ttu-id="7b9b7-177">Storage Explorer řešení potíží s [Průvodce][2]</span><span class="sxs-lookup"><span data-stu-id="7b9b7-177">Storage Explorer Troubleshooting [Guide][2]</span></span>
* <span data-ttu-id="7b9b7-178">[Pokyny] [ 3] o připojení k předplatnému Azure zásobníku</span><span class="sxs-lookup"><span data-stu-id="7b9b7-178">[Instructions][3] on connecting to an Azure Stack subscription</span></span>

#### <a name="fixes"></a><span data-ttu-id="7b9b7-179">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-179">Fixes</span></span>

* <span data-ttu-id="7b9b7-180">Pevná: Odeslání souboru měl vysokou možnost, že nedostatku paměti</span><span class="sxs-lookup"><span data-stu-id="7b9b7-180">Fixed: File upload had a high chance of causing an out of memory error</span></span>
* <span data-ttu-id="7b9b7-181">Opravené: Můžete teď přihlásit pomocí PIN kódu nebo čipová karta</span><span class="sxs-lookup"><span data-stu-id="7b9b7-181">Fixed: You can now sign in with PIN/Smartcard</span></span>
* <span data-ttu-id="7b9b7-182">Pevná: Otevřete portálu teď funguje s Azure China, Azure v Německu, Azure US Government a Azure zásobníku</span><span class="sxs-lookup"><span data-stu-id="7b9b7-182">Fixed: Open in Portal now works with Azure China, Azure Germany, Azure US Government, and Azure Stack</span></span>
* <span data-ttu-id="7b9b7-183">Pevná: Při odesílání do složky na kontejner objektů blob, "Neplatnou operaci" by někdy dojde k chybě</span><span class="sxs-lookup"><span data-stu-id="7b9b7-183">Fixed: While uploading a folder to a blob container, an "Illegal operation" error would sometimes occur</span></span>
* <span data-ttu-id="7b9b7-184">Pevná: Vybrat vše zakázal při správě snímků</span><span class="sxs-lookup"><span data-stu-id="7b9b7-184">Fixed: Select all was disabled while managing snapshots</span></span>
* <span data-ttu-id="7b9b7-185">Opravené: Může získat metadata objektu blob základní přepsána po z jeho snímků vlastností</span><span class="sxs-lookup"><span data-stu-id="7b9b7-185">Fixed: The metadata of the base blob might get overwritten after viewing the properties of its snapshots</span></span>

#### <a name="known-issues"></a><span data-ttu-id="7b9b7-186">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-186">Known Issues</span></span>

* <span data-ttu-id="7b9b7-187">Pokud zvolíte nesprávný certifikát PIN kód nebo čipová karta pak budete muset restartovat, aby bylo možné používat Storage Explorer zapomněli rozhodnutí</span><span class="sxs-lookup"><span data-stu-id="7b9b7-187">If you choose the wrong PIN/Smartcard certificate then you will need to restart in order to have Storage Explorer forget the decision</span></span>
* <span data-ttu-id="7b9b7-188">Při možnosti příchozí nebo odchozí, může úroveň přiblížení na okamžik resetovat na výchozí úrovni</span><span class="sxs-lookup"><span data-stu-id="7b9b7-188">While zoomed in or out, the zoom level may momentarily reset to the default level</span></span>
* <span data-ttu-id="7b9b7-189">S více než 3 skupiny objektů BLOB nebo ve stejnou dobu ukládání souborů může způsobit chyby</span><span class="sxs-lookup"><span data-stu-id="7b9b7-189">Having more than 3 groups of blobs or files uploading at the same time may cause errors</span></span>
* <span data-ttu-id="7b9b7-190">Panel nastavení účtu může zobrazovat, je potřeba zadat přihlašovací údaje, chcete-li filtrovat odběrů</span><span class="sxs-lookup"><span data-stu-id="7b9b7-190">The account settings panel may show that you need to reenter credentials in order to filter subscriptions</span></span>
* <span data-ttu-id="7b9b7-191">Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-191">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="7b9b7-192">Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entity.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-192">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="7b9b7-193">I když zásobník Azure aktuálně nepodporuje sdílených složek, sdílených složek uzel se objeví stále v připojené účtu úložiště Azure zásobníku.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-193">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="7b9b7-194">Ubuntu 14.04 instalace musí aktualizovat verzi RSZ nebo upgradovat – kroky pro upgrade jsou níže:</span><span class="sxs-lookup"><span data-stu-id="7b9b7-194">Ubuntu 14.04 install needs gcc version updated or upgraded – steps to upgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812--0811--0810"></a><span data-ttu-id="7b9b7-195">Verze 0.8.12 / 0.8.11 / 0.8.10</span><span class="sxs-lookup"><span data-stu-id="7b9b7-195">Version 0.8.12 / 0.8.11 / 0.8.10</span></span>
<span data-ttu-id="7b9b7-196">04/07/2017</span><span class="sxs-lookup"><span data-stu-id="7b9b7-196">04/07/2017</span></span>

#### <a name="new"></a><span data-ttu-id="7b9b7-197">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-197">New</span></span>

* <span data-ttu-id="7b9b7-198">Storage Explorer se teď automaticky zavře po instalaci aktualizace z oznámení o aktualizaci</span><span class="sxs-lookup"><span data-stu-id="7b9b7-198">Storage Explorer will now automatically close when you install an update from the update notification</span></span>
* <span data-ttu-id="7b9b7-199">Rychlý přístup na místě poskytuje lepší prostředí pro práci se často používané prostředky</span><span class="sxs-lookup"><span data-stu-id="7b9b7-199">In-place quick access provides an enhanced experience for working with your frequently accessed resources</span></span>
* <span data-ttu-id="7b9b7-200">V editoru kontejner objektů Blob se nyní zobrazí které nástroj virtual machine zapůjčených blob patří do</span><span class="sxs-lookup"><span data-stu-id="7b9b7-200">In the Blob Container editor, you can now see which virtual machine a leased blob belongs to</span></span>
* <span data-ttu-id="7b9b7-201">Nyní lze sbalit na levé straně panelu</span><span class="sxs-lookup"><span data-stu-id="7b9b7-201">You can now collapse the left side panel</span></span>
* <span data-ttu-id="7b9b7-202">Zjišťování se teď spustí ve stejnou dobu jako soubor ke stažení</span><span class="sxs-lookup"><span data-stu-id="7b9b7-202">Discovery now runs at the same time as download</span></span>
* <span data-ttu-id="7b9b7-203">Statistik použití v kontejneru objektů Blob, sdílené složky a tabulku editory zobrazíte velikost prostředku nebo výběr</span><span class="sxs-lookup"><span data-stu-id="7b9b7-203">Use Statistics in the Blob Container, File Share, and Table editors to see the size of your resource or selection</span></span>
* <span data-ttu-id="7b9b7-204">Můžete se teď může přihlásit k Azure Active Directory (AAD) na základě účtů Azure zásobníku.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-204">You can now sign-in to Azure Active Directory (AAD) based Azure Stack accounts.</span></span> 
* <span data-ttu-id="7b9b7-205">Teď můžete nahrát archivní soubory přes 32MB na prémiové účty úložiště</span><span class="sxs-lookup"><span data-stu-id="7b9b7-205">You can now upload archive files over 32MB to Premium storage accounts</span></span>
* <span data-ttu-id="7b9b7-206">Vylepšená podpora usnadnění přístupu</span><span class="sxs-lookup"><span data-stu-id="7b9b7-206">Improved accessibility support</span></span>
* <span data-ttu-id="7b9b7-207">Nyní můžete přidat důvěryhodný Base-64 kódovaný certifikáty X.509 SSL přejdete na Upravit -&gt; certifikáty SSL -&gt; Import certifikátů</span><span class="sxs-lookup"><span data-stu-id="7b9b7-207">You can now add trusted Base-64 encoded X.509 SSL certificates by going to Edit -&gt; SSL Certificates -&gt; Import Certificates</span></span>

#### <a name="fixes"></a><span data-ttu-id="7b9b7-208">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-208">Fixes</span></span>

* <span data-ttu-id="7b9b7-209">Pevné: po aktualizovat přihlašovací údaje účtu, stromovém zobrazení by někdy aktualizace automaticky</span><span class="sxs-lookup"><span data-stu-id="7b9b7-209">Fixed: after refreshing an account's credentials, the tree view would sometimes not automatically refresh</span></span>
* <span data-ttu-id="7b9b7-210">Pevná: generování SAS pro emulátor fronty a tabulky by způsobilo neplatná adresa URL</span><span class="sxs-lookup"><span data-stu-id="7b9b7-210">Fixed: generating a SAS for emulator queues and tables would result in an invalid URL</span></span>
* <span data-ttu-id="7b9b7-211">Opravené: prémiové účty úložiště lze nyní rozbalit proxy server je povolen program</span><span class="sxs-lookup"><span data-stu-id="7b9b7-211">Fixed: premium storage accounts can now be expanded while a proxy is enabled</span></span>
* <span data-ttu-id="7b9b7-212">Pevné: tlačítka použít na stránce Správa účtů nebude fungovat, pokud jste měli 1 nebo 0 účty vybrané</span><span class="sxs-lookup"><span data-stu-id="7b9b7-212">Fixed: the apply button on the accounts management page would not work if you had 1 or 0 accounts selected</span></span>
* <span data-ttu-id="7b9b7-213">Pevné: odesílání objekty BLOB, které vyžadují řešení konfliktu může selhat - fixed v 0.8.11</span><span class="sxs-lookup"><span data-stu-id="7b9b7-213">Fixed: uploading blobs that require conflict resolutions may fail - fixed in 0.8.11</span></span> 
* <span data-ttu-id="7b9b7-214">Pevná: odeslání názoru byla porušena v 0.8.11 - fixed v 0.8.12</span><span class="sxs-lookup"><span data-stu-id="7b9b7-214">Fixed: sending feedback was broken in 0.8.11 - fixed in 0.8.12</span></span> 

#### <a name="known-issues"></a><span data-ttu-id="7b9b7-215">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-215">Known Issues</span></span>

* <span data-ttu-id="7b9b7-216">Po upgradu na 0.8.10, musíte aktualizovat všechny svoje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-216">After upgrading to 0.8.10, you will need to refresh all of your credentials.</span></span>
* <span data-ttu-id="7b9b7-217">Při možnosti příchozí nebo odchozí, úroveň přiblížení může na okamžik resetovat na výchozí úrovni.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-217">While zoomed in or out, the zoom level may momentarily reset to the default level.</span></span>
* <span data-ttu-id="7b9b7-218">Více než 3 skupiny objektů BLOB nebo ve stejnou dobu ukládání souborů může způsobit chyby.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-218">Having more than 3 groups of blobs or files uploading at the same time may cause errors.</span></span>
* <span data-ttu-id="7b9b7-219">Panel nastavení účtu může zobrazovat, je potřeba zadat přihlašovací údaje, chcete-li filtrovat odběry.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-219">The account settings panel may show that you need to reenter credentials in order to filter subscriptions.</span></span>
* <span data-ttu-id="7b9b7-220">Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-220">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="7b9b7-221">Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entity.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-221">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="7b9b7-222">I když zásobník Azure aktuálně nepodporuje sdílených složek, sdílených složek uzel se objeví stále v připojené účtu úložiště Azure zásobníku.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-222">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="7b9b7-223">Ubuntu 14.04 instalace musí aktualizovat verzi RSZ nebo upgradovat – kroky pro upgrade jsou níže:</span><span class="sxs-lookup"><span data-stu-id="7b9b7-223">Ubuntu 14.04 install needs gcc version updated or upgraded – steps to upgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-089--088"></a><span data-ttu-id="7b9b7-224">Verze 0.8.9 nebo 0.8.8</span><span class="sxs-lookup"><span data-stu-id="7b9b7-224">Version 0.8.9 / 0.8.8</span></span>
<span data-ttu-id="7b9b7-225">02/23/2017</span><span class="sxs-lookup"><span data-stu-id="7b9b7-225">02/23/2017</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/R6gonK3cYAc?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/SrRPCm94mfE?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a><span data-ttu-id="7b9b7-226">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-226">New</span></span>

* <span data-ttu-id="7b9b7-227">Storage Explorer 0.8.9 automaticky stáhne nejnovější aktualizace.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-227">Storage Explorer 0.8.9 will automatically download the latest version for updates.</span></span>
* <span data-ttu-id="7b9b7-228">Opravy hotfix: portálu generovaný identifikátor URI SAS pro připojení k účtu úložiště by způsobilo chybu.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-228">Hotfix: using a portal generated SAS URI to attach a storage account would result in an error.</span></span>
* <span data-ttu-id="7b9b7-229">Teď můžete vytvářet, spravovat a zvýšit úroveň snímky objektů blob.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-229">You can now create, manage, and promote blob snapshots.</span></span>
* <span data-ttu-id="7b9b7-230">Můžete teď přihlášení k účtům Azure China Azure v Německu a Azure US Government.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-230">You can now sign in to Azure China, Azure Germany, and Azure US Government accounts.</span></span>
* <span data-ttu-id="7b9b7-231">Nyní můžete měnit úroveň přiblížení.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-231">You can now change the zoom level.</span></span> <span data-ttu-id="7b9b7-232">Pomocí možností v nabídce zobrazení přiblížit, oddálit a resetovat přiblížení.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-232">Use the options in the View menu to Zoom In, Zoom Out, and Reset Zoom.</span></span>
* <span data-ttu-id="7b9b7-233">Uživatel metadat pro objekty BLOB a soubory jsou nyní podporovány znaky znakové sady Unicode.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-233">Unicode characters are now supported in user metadata for blobs and files.</span></span>
* <span data-ttu-id="7b9b7-234">Vylepšení přístupnosti.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-234">Accessibility improvements.</span></span>
* <span data-ttu-id="7b9b7-235">Poznámky k verzi na další verzi si můžete prohlížet oznámení o aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-235">The next version's release notes can be viewed from the update notification.</span></span> <span data-ttu-id="7b9b7-236">Můžete také zobrazit aktuální poznámky k verzi z nabídky Nápověda.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-236">You can also view the current release notes from the Help menu.</span></span>

#### <a name="fixes"></a><span data-ttu-id="7b9b7-237">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-237">Fixes</span></span>

* <span data-ttu-id="7b9b7-238">Opravené: číslo verze je nyní správně zobrazen v Ovládacích panelech v systému Windows</span><span class="sxs-lookup"><span data-stu-id="7b9b7-238">Fixed: the version number is now correctly displayed in Control Panel on Windows</span></span>
* <span data-ttu-id="7b9b7-239">Opravené: vyhledávání již není omezeno na 50 000 uzly</span><span class="sxs-lookup"><span data-stu-id="7b9b7-239">Fixed: search is no longer limited to 50,000 nodes</span></span>
* <span data-ttu-id="7b9b7-240">Opravené: nahrát do sdílené složky navždy spouštějí, pokud cílový adresář ještě neexistovaly</span><span class="sxs-lookup"><span data-stu-id="7b9b7-240">Fixed: upload to a file share spun forever if the destination directory did not already exist</span></span>
* <span data-ttu-id="7b9b7-241">Opravené: vylepšení stability dlouho nahrávání a stahování</span><span class="sxs-lookup"><span data-stu-id="7b9b7-241">Fixed: improved stability for long uploads and downloads</span></span>

#### <a name="known-issues"></a><span data-ttu-id="7b9b7-242">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-242">Known Issues</span></span>

* <span data-ttu-id="7b9b7-243">Při možnosti příchozí nebo odchozí, úroveň přiblížení může na okamžik resetovat na výchozí úrovni.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-243">While zoomed in or out, the zoom level may momentarily reset to the default level.</span></span>
* <span data-ttu-id="7b9b7-244">Rychlý přístup pracuje pouze s odběru na základě položky.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-244">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="7b9b7-245">V této verzi nepodporuje místních prostředků nebo prostředky, které jsou připojené prostřednictvím klíč nebo tokenu SAS.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-245">Local resources or resources attached via key or SAS token are not supported in this release.</span></span>
* <span data-ttu-id="7b9b7-246">Rychlý přístup může trvat a přejděte do cílového prostředku, v závislosti na tom, kolik prostředků, máte několik sekund.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-246">It may take Quick Access a few seconds to navigate to the target resource, depending on how many resources you have.</span></span>
* <span data-ttu-id="7b9b7-247">Více než 3 skupiny objektů BLOB nebo ve stejnou dobu ukládání souborů může způsobit chyby.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-247">Having more than 3 groups of blobs or files uploading at the same time may cause errors.</span></span>

<span data-ttu-id="7b9b7-248">12/16/2016</span><span class="sxs-lookup"><span data-stu-id="7b9b7-248">12/16/2016</span></span>
### <a name="version-087"></a><span data-ttu-id="7b9b7-249">Verze 0.8.7</span><span class="sxs-lookup"><span data-stu-id="7b9b7-249">Version 0.8.7</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="7b9b7-250">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-250">New</span></span>

* <span data-ttu-id="7b9b7-251">Můžete vybrat, jak se vyřešit konflikty na začátku relaci aktualizace, stažení nebo kopírování v okně aktivity</span><span class="sxs-lookup"><span data-stu-id="7b9b7-251">You can choose how to resolve conflicts at the beginning of an update, download or copy session in the Activities window</span></span>
* <span data-ttu-id="7b9b7-252">Najeďte myší na kartě zobrazit úplnou cestu prostředků úložiště</span><span class="sxs-lookup"><span data-stu-id="7b9b7-252">Hover over a tab to see the full path of the storage resource</span></span>
* <span data-ttu-id="7b9b7-253">Když kliknete na kartě, synchronizuje s jeho umístění v navigačním podokně levé straně</span><span class="sxs-lookup"><span data-stu-id="7b9b7-253">When you click on a tab, it synchronizes with its location in the left side navigation pane</span></span>

#### <a name="fixes"></a><span data-ttu-id="7b9b7-254">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-254">Fixes</span></span>

* <span data-ttu-id="7b9b7-255">Opravené: Storage Explorer je nyní důvěryhodné aplikace v systému Mac</span><span class="sxs-lookup"><span data-stu-id="7b9b7-255">Fixed: Storage Explorer is now a trusted app on Mac</span></span>
* <span data-ttu-id="7b9b7-256">Opravené: Ubuntu 14.04 se znovu podporuje</span><span class="sxs-lookup"><span data-stu-id="7b9b7-256">Fixed: Ubuntu 14.04 is again supported</span></span>
* <span data-ttu-id="7b9b7-257">Opravené: Někdy přidat účet uživatelského rozhraní bliká při načítání odběrů</span><span class="sxs-lookup"><span data-stu-id="7b9b7-257">Fixed: Sometimes the add account UI flashes when loading subscriptions</span></span>
* <span data-ttu-id="7b9b7-258">Opravené: Někdy ne všechny prostředky úložiště byly uvedené v navigačním podokně levé straně</span><span class="sxs-lookup"><span data-stu-id="7b9b7-258">Fixed: Sometimes not all storage resources were listed in the left side navigation pane</span></span>
* <span data-ttu-id="7b9b7-259">Pevná: V podokně Akce někdy zobrazí prázdné akce</span><span class="sxs-lookup"><span data-stu-id="7b9b7-259">Fixed: The action pane sometimes displayed empty actions</span></span>
* <span data-ttu-id="7b9b7-260">Opravené: Velikost okna z poslední uzavřené relace je nyní uchována</span><span class="sxs-lookup"><span data-stu-id="7b9b7-260">Fixed: The window size from the last closed session is now retained</span></span>
* <span data-ttu-id="7b9b7-261">Opravené: Můžete otevřít několik karet pro stejný prostředek pomocí místní nabídky</span><span class="sxs-lookup"><span data-stu-id="7b9b7-261">Fixed: You can open multiple tabs for the same resource using the context menu</span></span>

#### <a name="known-issues"></a><span data-ttu-id="7b9b7-262">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-262">Known Issues</span></span>

* <span data-ttu-id="7b9b7-263">Rychlý přístup pracuje pouze s odběru na základě položky.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-263">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="7b9b7-264">V této verzi nepodporuje místních prostředků nebo prostředky, které jsou připojené prostřednictvím klíč nebo tokenu SAS</span><span class="sxs-lookup"><span data-stu-id="7b9b7-264">Local resources or resources attached via key or SAS token are not supported in this release</span></span>
* <span data-ttu-id="7b9b7-265">Rychlý přístup může trvat a několik sekund, přejděte na cílový prostředek, v závislosti na tom, kolik prostředků máte</span><span class="sxs-lookup"><span data-stu-id="7b9b7-265">It may take Quick Access a few seconds to navigate to the target resource, depending on how many resources you have</span></span>
* <span data-ttu-id="7b9b7-266">S více než 3 skupiny objektů BLOB nebo ve stejnou dobu ukládání souborů může způsobit chyby</span><span class="sxs-lookup"><span data-stu-id="7b9b7-266">Having more than 3 groups of blobs or files uploading at the same time may cause errors</span></span>
* <span data-ttu-id="7b9b7-267">Obslužné rutiny vyhledávání hledání mezi uzly přibližně 50 000 - poté výkonu může být ovlivněno nebo může způsobit, že neošetřené výjimky</span><span class="sxs-lookup"><span data-stu-id="7b9b7-267">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted or may cause unhandled exception</span></span>
* <span data-ttu-id="7b9b7-268">Pro první přihlášení pomocí Průzkumníka úložiště v systému macOS může se zobrazit více výzev žádostí o oprávnění uživatele pro přístup k řetězci klíčů.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-268">For the first time using the Storage Explorer on macOS, you might see multiple prompts asking for user's permission to access keychain.</span></span> <span data-ttu-id="7b9b7-269">Doporučujeme že vybrat vždy povolit tak nebude znovu objeví řádku</span><span class="sxs-lookup"><span data-stu-id="7b9b7-269">We suggest you select Always Allow so the prompt won't show up again</span></span>

<span data-ttu-id="7b9b7-270">11/18/2016</span><span class="sxs-lookup"><span data-stu-id="7b9b7-270">11/18/2016</span></span>
### <a name="version-086"></a><span data-ttu-id="7b9b7-271">Verze 0.8.6</span><span class="sxs-lookup"><span data-stu-id="7b9b7-271">Version 0.8.6</span></span>

#### <a name="new"></a><span data-ttu-id="7b9b7-272">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-272">New</span></span>

* <span data-ttu-id="7b9b7-273">Můžete teď pin nejčastěji používá služby pro rychlý přístup snadno navigace</span><span class="sxs-lookup"><span data-stu-id="7b9b7-273">You can now pin most frequently used services to the Quick Access for easy navigation</span></span>
* <span data-ttu-id="7b9b7-274">Nyní lze otevřít více editory v různých kartách.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-274">You can now open multiple editors in different tabs.</span></span> <span data-ttu-id="7b9b7-275">Jedním kliknutím zobrazíte dočasné kartu; Poklikejte na kartě trvalé. Můžete také kliknutím na kartu dočasné aby trvalé karta</span><span class="sxs-lookup"><span data-stu-id="7b9b7-275">Single click to open a temporary tab; double click to open a permanent tab. You can also click on the temporary tab to make it a permanent tab</span></span>
* <span data-ttu-id="7b9b7-276">Jsme provedli znatelné výkonu a zlepšení stability pro odesílání a stahování souborů, zejména u velkých souborů na rychlé počítače</span><span class="sxs-lookup"><span data-stu-id="7b9b7-276">We have made noticeable performance and stability improvements for uploads and downloads, especially for large files on fast machines</span></span>
* <span data-ttu-id="7b9b7-277">Prázdné složky "virtuální" teď lze vytvořit v kontejnerech objektů blob</span><span class="sxs-lookup"><span data-stu-id="7b9b7-277">Empty "virtual" folders can now be created in blob containers</span></span>
* <span data-ttu-id="7b9b7-278">Mít znovu zavedli jsme vymezené vyhledávání s naší nové rozšířené dílčí řetězec hledání, takže Teď máte dvě možnosti pro vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="7b9b7-278">We have re-introduced scoped search with our new enhanced substring search, so you now have two options for searching:</span></span> 
    * <span data-ttu-id="7b9b7-279">Globální vyhledávání – stačí do textového pole hledání zadejte hledaný termín</span><span class="sxs-lookup"><span data-stu-id="7b9b7-279">Global search - just enter a search term into the search textbox</span></span>
    * <span data-ttu-id="7b9b7-280">Prohledejte – klikněte na ikonu lupy vedle uzlu, pak přidejte hledaný termín na konec cesty, nebo klikněte pravým tlačítkem a vyberte "Vyhledávání z zde"</span><span class="sxs-lookup"><span data-stu-id="7b9b7-280">Scoped search - click the magnifying glass icon next to a node, then add a search term to the end of the path, or right-click and select "Search from Here"</span></span>
* <span data-ttu-id="7b9b7-281">Jsme přidali různé motivy: Light (výchozí), světlý, vysoce kontrastní černá a Vysoký kontrast prázdné.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-281">We have added various themes: Light (default), Dark, High Contrast Black, and High Contrast White.</span></span> <span data-ttu-id="7b9b7-282">Přejděte na Úpravy -&gt; motivy, chcete-li změnit vaši volbu motivů</span><span class="sxs-lookup"><span data-stu-id="7b9b7-282">Go to Edit -&gt; Themes to change your theming preference</span></span>
* <span data-ttu-id="7b9b7-283">Můžete upravit vlastnosti objektů Blob a souborů</span><span class="sxs-lookup"><span data-stu-id="7b9b7-283">You can modify Blob and file properties</span></span>
* <span data-ttu-id="7b9b7-284">Teď podporujeme kódovaného (base64) a nekódovaného fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="7b9b7-284">We now support encoded (base64) and unencoded queue messages</span></span>
* <span data-ttu-id="7b9b7-285">V systému Linux, OS 64-bit je nyní vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-285">On Linux, a 64-bit OS is now required.</span></span> <span data-ttu-id="7b9b7-286">Pro tuto verzi podporujeme pouze 64bitové verze Ubuntu 16.04.1 LTS</span><span class="sxs-lookup"><span data-stu-id="7b9b7-286">For this release we only support 64-bit Ubuntu 16.04.1 LTS</span></span>
* <span data-ttu-id="7b9b7-287">Aktualizovali jsme naši logo!</span><span class="sxs-lookup"><span data-stu-id="7b9b7-287">We have updated our logo!</span></span>

#### <a name="fixes"></a><span data-ttu-id="7b9b7-288">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-288">Fixes</span></span>

* <span data-ttu-id="7b9b7-289">Pevná: Obrazovky zmrazení problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-289">Fixed: Screen freezing problems</span></span>
* <span data-ttu-id="7b9b7-290">Opravené: Rozšířené zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7b9b7-290">Fixed: Enhanced security</span></span>
* <span data-ttu-id="7b9b7-291">Pevná: Zobrazovat může někdy duplicitní připojené účty</span><span class="sxs-lookup"><span data-stu-id="7b9b7-291">Fixed: Sometimes duplicate attached accounts could appear</span></span>
* <span data-ttu-id="7b9b7-292">Opravené: Objekt blob se typ obsahu definovaný může generovat výjimky</span><span class="sxs-lookup"><span data-stu-id="7b9b7-292">Fixed: A blob with an undefined content type could generate an exception</span></span>
* <span data-ttu-id="7b9b7-293">Pevná: Otevření panelu dotaz na prázdná tabulka nebylo možné</span><span class="sxs-lookup"><span data-stu-id="7b9b7-293">Fixed: Opening the Query Panel on an empty table was not possible</span></span>
* <span data-ttu-id="7b9b7-294">Opravené: Se liší chyby ve vyhledávání</span><span class="sxs-lookup"><span data-stu-id="7b9b7-294">Fixed: Varies bugs in Search</span></span>
* <span data-ttu-id="7b9b7-295">Pevné: Zvýšit počet prostředků načtených z 50 do 100 při kliknutí na "Více zatížení"</span><span class="sxs-lookup"><span data-stu-id="7b9b7-295">Fixed: Increased the number of resources loaded from 50 to 100 when clicking "Load More"</span></span>
* <span data-ttu-id="7b9b7-296">Pevná: Při prvním spuštění, pokud je účet přihlášeni, jsme teď vybrat všechna předplatná pro tento účet ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="7b9b7-296">Fixed: On first run, if an account is signed into, we now select all subscriptions for that account by default</span></span> 

#### <a name="known-issues"></a><span data-ttu-id="7b9b7-297">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-297">Known Issues</span></span>

* <span data-ttu-id="7b9b7-298">Tato verze Storage Explorer nefunguje na Ubuntu 14.04</span><span class="sxs-lookup"><span data-stu-id="7b9b7-298">This release of the Storage Explorer does not run on Ubuntu 14.04</span></span>
* <span data-ttu-id="7b9b7-299">Chcete-li spustit několik karet pro stejný prostředek, neklikejte na nepřetržitě se stejným prostředkem.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-299">To open multiple tabs for the same resource, do not continuously click on the same resource.</span></span> <span data-ttu-id="7b9b7-300">Klikněte na jiný prostředek a potom přejděte zpět a potom klikněte na původní prostředek, který ho znovu otevřete na jiné kartě</span><span class="sxs-lookup"><span data-stu-id="7b9b7-300">Click on another resource and then go back and then click on the original resource to open it again in another tab</span></span> 
* <span data-ttu-id="7b9b7-301">Rychlý přístup pracuje pouze s odběru na základě položky.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-301">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="7b9b7-302">V této verzi nepodporuje místních prostředků nebo prostředky, které jsou připojené prostřednictvím klíč nebo tokenu SAS</span><span class="sxs-lookup"><span data-stu-id="7b9b7-302">Local resources or resources attached via key or SAS token are not supported in this release</span></span>
* <span data-ttu-id="7b9b7-303">Rychlý přístup může trvat a několik sekund, přejděte na cílový prostředek, v závislosti na tom, kolik prostředků máte</span><span class="sxs-lookup"><span data-stu-id="7b9b7-303">It may take Quick Access a few seconds to navigate to the target resource, depending on how many resources you have</span></span>
* <span data-ttu-id="7b9b7-304">S více než 3 skupiny objektů BLOB nebo ve stejnou dobu ukládání souborů může způsobit chyby</span><span class="sxs-lookup"><span data-stu-id="7b9b7-304">Having more than 3 groups of blobs or files uploading at the same time may cause errors</span></span>
* <span data-ttu-id="7b9b7-305">Obslužné rutiny vyhledávání hledání mezi uzly přibližně 50 000 - poté výkonu může být ovlivněno nebo může způsobit, že neošetřené výjimky</span><span class="sxs-lookup"><span data-stu-id="7b9b7-305">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted or may cause unhandled exception</span></span>

<span data-ttu-id="7b9b7-306">10/03/2016</span><span class="sxs-lookup"><span data-stu-id="7b9b7-306">10/03/2016</span></span>
### <a name="version-085"></a><span data-ttu-id="7b9b7-307">Verze 0.8.5</span><span class="sxs-lookup"><span data-stu-id="7b9b7-307">Version 0.8.5</span></span>

#### <a name="new"></a><span data-ttu-id="7b9b7-308">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-308">New</span></span>

* <span data-ttu-id="7b9b7-309">Teď můžete připojit k účtům úložiště a prostředky použít klíče generované portál SAS</span><span class="sxs-lookup"><span data-stu-id="7b9b7-309">Can now use Portal-generated SAS keys to attach to Storage Accounts and resources</span></span>

#### <a name="fixes"></a><span data-ttu-id="7b9b7-310">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-310">Fixes</span></span>

* <span data-ttu-id="7b9b7-311">Pevná: časování při hledání někdy způsobila uzly k jiné rozšíření</span><span class="sxs-lookup"><span data-stu-id="7b9b7-311">Fixed: race condition during search sometimes caused nodes to become non-expandable</span></span>
* <span data-ttu-id="7b9b7-312">Opravené: "Použít protokol HTTP" nefunguje při připojování k účtům úložiště s názvem účtu a klíč</span><span class="sxs-lookup"><span data-stu-id="7b9b7-312">Fixed: "Use HTTP" doesn't work when connecting to Storage Accounts with account name and key</span></span>
* <span data-ttu-id="7b9b7-313">Opravené: Klíče SAS (ty speciálně generované portálu) vrátí chybu "koncové lomítko."</span><span class="sxs-lookup"><span data-stu-id="7b9b7-313">Fixed: SAS keys (specially Portal-generated ones) return a "trailing slash" error</span></span>
* <span data-ttu-id="7b9b7-314">Opravené: import tabulky problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-314">Fixed: table import issues</span></span>
    * <span data-ttu-id="7b9b7-315">Někdy měla vrátit klíč oddílu a klíč řádku</span><span class="sxs-lookup"><span data-stu-id="7b9b7-315">Sometimes partition key and row key were reversed</span></span>
    * <span data-ttu-id="7b9b7-316">Nelze číst "null" klíče oddílů</span><span class="sxs-lookup"><span data-stu-id="7b9b7-316">Unable to read "null" Partition Keys</span></span>

#### <a name="known-issues"></a><span data-ttu-id="7b9b7-317">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-317">Known Issues</span></span>

* <span data-ttu-id="7b9b7-318">Obslužné rutiny vyhledávání hledání mezi uzly přibližně 50 000 - poté může mít dopad na výkon</span><span class="sxs-lookup"><span data-stu-id="7b9b7-318">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted</span></span>
* <span data-ttu-id="7b9b7-319">Azure zásobník nepodporuje aktuálně soubory, tak při pokusu o rozbalení souborů se zobrazí chyba</span><span class="sxs-lookup"><span data-stu-id="7b9b7-319">Azure Stack doesn't currently support Files, so trying to expand Files will show an error</span></span>

<span data-ttu-id="7b9b7-320">09/12/2016</span><span class="sxs-lookup"><span data-stu-id="7b9b7-320">09/12/2016</span></span>
### <a name="version-084"></a><span data-ttu-id="7b9b7-321">Verze 0.8.4</span><span class="sxs-lookup"><span data-stu-id="7b9b7-321">Version 0.8.4</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="7b9b7-322">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-322">New</span></span>

* <span data-ttu-id="7b9b7-323">Generovat přímé odkazy na účty úložiště, kontejnery, fronty, tabulky nebo sdílené složky pro sdílení a podporu snadný přístup k prostředkům – Windows a Mac OS</span><span class="sxs-lookup"><span data-stu-id="7b9b7-323">Generate direct links to storage accounts, containers, queues, tables, or file shares for sharing and easy access to your resources - Windows and Mac OS support</span></span>
* <span data-ttu-id="7b9b7-324">Hledat kontejnery objektů blob, tabulek, front, sdílené složky nebo účty úložiště ze do vyhledávacího pole</span><span class="sxs-lookup"><span data-stu-id="7b9b7-324">Search for your blob containers, tables, queues, file shares, or storage accounts from the search box</span></span>
* <span data-ttu-id="7b9b7-325">Nyní můžete seskupit klauzule v Tvůrce dotazů tabulky</span><span class="sxs-lookup"><span data-stu-id="7b9b7-325">You can now group clauses in the table query builder</span></span>
* <span data-ttu-id="7b9b7-326">Přejmenujte a zkopírujte a vložte kontejnery objektů blob, sdílené složky, tabulky, objekty BLOB, objektů blob složek, souborů a adresářů z v rámci SAS připojené účty a kontejnery</span><span class="sxs-lookup"><span data-stu-id="7b9b7-326">Rename and copy/paste blob containers, file shares, tables, blobs, blob folders, files and directories from within SAS-attached accounts and containers</span></span>
* <span data-ttu-id="7b9b7-327">Přejmenování a kopírování objektu blob kontejnery a sdílené složky teď zachovat vlastnosti a metadata</span><span class="sxs-lookup"><span data-stu-id="7b9b7-327">Renaming and copying blob containers and file shares now preserve properties and metadata</span></span>

#### <a name="fixes"></a><span data-ttu-id="7b9b7-328">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-328">Fixes</span></span>

* <span data-ttu-id="7b9b7-329">Opravené: nelze upravit entity tabulky, pokud obsahují logická hodnota nebo binární vlastnosti</span><span class="sxs-lookup"><span data-stu-id="7b9b7-329">Fixed: cannot edit table entities if they contain boolean or binary properties</span></span>

#### <a name="known-issues"></a><span data-ttu-id="7b9b7-330">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-330">Known Issues</span></span>

* <span data-ttu-id="7b9b7-331">Obslužné rutiny vyhledávání hledání mezi uzly přibližně 50 000 - poté může mít dopad na výkon</span><span class="sxs-lookup"><span data-stu-id="7b9b7-331">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted</span></span>

<span data-ttu-id="7b9b7-332">08/03/2016</span><span class="sxs-lookup"><span data-stu-id="7b9b7-332">08/03/2016</span></span>
### <a name="version-083"></a><span data-ttu-id="7b9b7-333">Verze 0.8.3</span><span class="sxs-lookup"><span data-stu-id="7b9b7-333">Version 0.8.3</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="7b9b7-334">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-334">New</span></span>

* <span data-ttu-id="7b9b7-335">Přejmenujte kontejnery, tabulky, sdílené složky</span><span class="sxs-lookup"><span data-stu-id="7b9b7-335">Rename containers, tables, file shares</span></span>
* <span data-ttu-id="7b9b7-336">Vylepšené prostředí Tvůrce dotazů</span><span class="sxs-lookup"><span data-stu-id="7b9b7-336">Improved Query builder experience</span></span>
* <span data-ttu-id="7b9b7-337">Možnost uložení a načtení dotazů</span><span class="sxs-lookup"><span data-stu-id="7b9b7-337">Ability to save and load queries</span></span>
* <span data-ttu-id="7b9b7-338">Přímé odkazy na úložiště účtů nebo kontejnerů, fronty, tabulky nebo sdílené složky pro sdílení a snadno přístup k prostředkům (pouze pro systém Windows - systému macOS podporu brzo!)</span><span class="sxs-lookup"><span data-stu-id="7b9b7-338">Direct links to storage accounts or containers, queues, tables, or file shares for sharing and easily accessing your resources (Windows-only - macOS support coming soon!)</span></span>
* <span data-ttu-id="7b9b7-339">Schopnost spravovat a konfigurovat pravidla CORS</span><span class="sxs-lookup"><span data-stu-id="7b9b7-339">Ability to manage and configure CORS rules</span></span>

#### <a name="fixes"></a><span data-ttu-id="7b9b7-340">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-340">Fixes</span></span>

* <span data-ttu-id="7b9b7-341">Opravené: Accounts Microsoft vyžadovat opakované ověření každých 8 – 12 hodin</span><span class="sxs-lookup"><span data-stu-id="7b9b7-341">Fixed: Microsoft Accounts require re-authentication every 8-12 hours</span></span>

#### <a name="known-issues"></a><span data-ttu-id="7b9b7-342">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-342">Known Issues</span></span>

* <span data-ttu-id="7b9b7-343">Někdy uživatelského rozhraní se může objevit ukotvené – tím se maximalizuje okno pomůže tento problém vyřešit</span><span class="sxs-lookup"><span data-stu-id="7b9b7-343">Sometimes the UI might appear frozen - maximizing the window helps resolve this issue</span></span>
* <span data-ttu-id="7b9b7-344">instalace systému macOS může vyžadovat zvýšenými oprávněními</span><span class="sxs-lookup"><span data-stu-id="7b9b7-344">macOS install may require elevated permissions</span></span>
* <span data-ttu-id="7b9b7-345">Panel nastavení účtu může zobrazovat, je potřeba zadat přihlašovací údaje, chcete-li filtrovat odběrů</span><span class="sxs-lookup"><span data-stu-id="7b9b7-345">Account settings panel may show that you need to reenter credentials in order to filter subscriptions</span></span>
* <span data-ttu-id="7b9b7-346">Přejmenování sdílené složky, kontejnery objektů blob a tabulek nezachovává metadata nebo dalších vlastností kontejneru, například kvóta pro sdílenou složku, úroveň veřejného přístupu nebo zásady přístupu</span><span class="sxs-lookup"><span data-stu-id="7b9b7-346">Renaming file shares, blob containers, and tables does not preserve metadata or other properties on the container, such as file share quota, public access level or access policies</span></span>
* <span data-ttu-id="7b9b7-347">Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-347">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="7b9b7-348">Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entit</span><span class="sxs-lookup"><span data-stu-id="7b9b7-348">All other properties and metadata for blobs, files and entities are preserved during a rename</span></span>
* <span data-ttu-id="7b9b7-349">Kopírování nebo přejmenování prostředky nefunguje v rámci SAS připojené účty</span><span class="sxs-lookup"><span data-stu-id="7b9b7-349">Copying or renaming resources does not work within SAS-attached accounts</span></span>

<span data-ttu-id="7b9b7-350">07/07/2016</span><span class="sxs-lookup"><span data-stu-id="7b9b7-350">07/07/2016</span></span>
### <a name="version-082"></a><span data-ttu-id="7b9b7-351">Verze 0.8.2</span><span class="sxs-lookup"><span data-stu-id="7b9b7-351">Version 0.8.2</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="7b9b7-352">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-352">New</span></span>

* <span data-ttu-id="7b9b7-353">Účty úložiště jsou seskupené podle odběry; vývoj pro úložiště a prostředky, které jsou připojené prostřednictvím klíč nebo SAS se zobrazují v uzlu (místní a připojené)</span><span class="sxs-lookup"><span data-stu-id="7b9b7-353">Storage Accounts are grouped by subscriptions; development storage and resources attached via key or SAS are shown under (Local and Attached) node</span></span>
* <span data-ttu-id="7b9b7-354">Odhlaste se z účtů v panelech "Nastavení účtu Azure"</span><span class="sxs-lookup"><span data-stu-id="7b9b7-354">Sign off from accounts in "Azure Account Settings" panel</span></span>
* <span data-ttu-id="7b9b7-355">Konfigurace nastavení proxy serveru povolit a spravovat přihlášení</span><span class="sxs-lookup"><span data-stu-id="7b9b7-355">Configure proxy settings to enable and manage sign-in</span></span>
* <span data-ttu-id="7b9b7-356">Vytvoření a rozdělit zapůjčení objektů blob</span><span class="sxs-lookup"><span data-stu-id="7b9b7-356">Create and break blob leases</span></span>
* <span data-ttu-id="7b9b7-357">Kontejnery otevřete objektů blob, fronty, tabulky a soubory s jedním kliknutím</span><span class="sxs-lookup"><span data-stu-id="7b9b7-357">Open blob containers, queues, tables, and files with single-click</span></span>

#### <a name="fixes"></a><span data-ttu-id="7b9b7-358">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-358">Fixes</span></span>

* <span data-ttu-id="7b9b7-359">Pevná: Vložit s knihovnami .NET nebo Java zprávy fronty nejsou dekódovat správně z formátu base64.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-359">Fixed: queue messages inserted with .NET or Java libraries are not properly decoded from base64</span></span>
* <span data-ttu-id="7b9b7-360">Opravené: $metrics tabulky nejsou zobrazeny pro účty úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="7b9b7-360">Fixed: $metrics tables are not shown for Blob Storage accounts</span></span>
* <span data-ttu-id="7b9b7-361">Opravené: uzel tabulky nefunguje pro místní úložiště (vývoj)</span><span class="sxs-lookup"><span data-stu-id="7b9b7-361">Fixed: tables node does not work for local (Development) storage</span></span>

#### <a name="known-issues"></a><span data-ttu-id="7b9b7-362">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-362">Known Issues</span></span>

* <span data-ttu-id="7b9b7-363">instalace systému macOS může vyžadovat zvýšenými oprávněními</span><span class="sxs-lookup"><span data-stu-id="7b9b7-363">macOS install may require elevated permissions</span></span>

<span data-ttu-id="7b9b7-364">06/15/2016</span><span class="sxs-lookup"><span data-stu-id="7b9b7-364">06/15/2016</span></span>
### <a name="version-080"></a><span data-ttu-id="7b9b7-365">Verze 0.8.0</span><span class="sxs-lookup"><span data-stu-id="7b9b7-365">Version 0.8.0</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ycfQhKztSIY?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="7b9b7-366">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-366">New</span></span>

* <span data-ttu-id="7b9b7-367">Podpora sdílení souborů: zobrazení, odesílání, stahování, kopírování souborů a adresářů, identifikátory URI SAS (vytvořit a připojit)</span><span class="sxs-lookup"><span data-stu-id="7b9b7-367">File share support: viewing, uploading, downloading, copying files and directories, SAS URIs (create and connect)</span></span>
* <span data-ttu-id="7b9b7-368">Vylepšené uživatelské prostředí pro připojování k úložišti s identifikátory URI SAS nebo účet klíče</span><span class="sxs-lookup"><span data-stu-id="7b9b7-368">Improved user experience for connecting to Storage with SAS URIs or account keys</span></span>
* <span data-ttu-id="7b9b7-369">Export výsledků dotazu tabulky</span><span class="sxs-lookup"><span data-stu-id="7b9b7-369">Export table query results</span></span>
* <span data-ttu-id="7b9b7-370">Změny pořadí sloupců tabulky a přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="7b9b7-370">Table column reordering and customization</span></span>
* <span data-ttu-id="7b9b7-371">Zobrazení $logs kontejnery objektů blob a tabulek $metrics pro účty úložiště pomocí povoleno metriky</span><span class="sxs-lookup"><span data-stu-id="7b9b7-371">Viewing $logs blob containers and $metrics tables for Storage Accounts with enabled metrics</span></span>
* <span data-ttu-id="7b9b7-372">Vylepšené export a import chování, teď obsahuje typ hodnoty vlastnosti</span><span class="sxs-lookup"><span data-stu-id="7b9b7-372">Improved export and import behavior, now includes property value type</span></span>

#### <a name="fixes"></a><span data-ttu-id="7b9b7-373">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-373">Fixes</span></span>

* <span data-ttu-id="7b9b7-374">Opravené: nahrávání nebo stahování velké objekty BLOB může způsobit neúplné nahrávání nebo stahování</span><span class="sxs-lookup"><span data-stu-id="7b9b7-374">Fixed: uploading or downloading large blobs can result in incomplete uploads/downloads</span></span>
* <span data-ttu-id="7b9b7-375">Opravené: úpravy, přidáním nebo importováním entitu s hodnotou číselných řetězců ("1") bude převedena do double</span><span class="sxs-lookup"><span data-stu-id="7b9b7-375">Fixed: editing, adding, or importing an entity with a numeric string value ("1") will convert it to double</span></span>
* <span data-ttu-id="7b9b7-376">Pevná: Nelze rozbalte uzel v místním vývojovém prostředí</span><span class="sxs-lookup"><span data-stu-id="7b9b7-376">Fixed: Unable to expand the table node in the local development environment</span></span>

#### <a name="known-issues"></a><span data-ttu-id="7b9b7-377">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-377">Known Issues</span></span>

* <span data-ttu-id="7b9b7-378">$metrics tabulky nejsou viditelné pro účty úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="7b9b7-378">$metrics tables are not visible for Blob Storage accounts</span></span>
* <span data-ttu-id="7b9b7-379">Zprávy fronty přidány prostřednictvím kódu programu, se nemusí zobrazit správně Pokud zprávy jsou zakódován pomocí kódování Base64</span><span class="sxs-lookup"><span data-stu-id="7b9b7-379">Queue messages added programmatically may not be displayed correctly if the messages are encoded using Base64 encoding</span></span>

<span data-ttu-id="7b9b7-380">05/17/2016</span><span class="sxs-lookup"><span data-stu-id="7b9b7-380">05/17/2016</span></span>
### <a name="version-07201605090"></a><span data-ttu-id="7b9b7-381">Verze 0.7.20160509.0</span><span class="sxs-lookup"><span data-stu-id="7b9b7-381">Version 0.7.20160509.0</span></span>

#### <a name="new"></a><span data-ttu-id="7b9b7-382">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-382">New</span></span>

* <span data-ttu-id="7b9b7-383">Lepší zpracování chyb pro aplikace, dojde k chybě</span><span class="sxs-lookup"><span data-stu-id="7b9b7-383">Better error handling for app crashes</span></span>

#### <a name="fixes"></a><span data-ttu-id="7b9b7-384">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-384">Fixes</span></span>

* <span data-ttu-id="7b9b7-385">Kde informační panel zpráv někdy nezobrazovat až při přihlašovací údaje nebyly potřeba opravené chyby</span><span class="sxs-lookup"><span data-stu-id="7b9b7-385">Fixed bug where InfoBar messages sometimes don't show up when sign-in credentials were required</span></span>

#### <a name="known-issues"></a><span data-ttu-id="7b9b7-386">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-386">Known Issues</span></span>

* <span data-ttu-id="7b9b7-387">Tabulky: Přidávání, úpravy nebo import entita, která má vlastnost s ambiguously číselnou hodnotu, jako je například "1" nebo "1.0" a uživatel se pokusí odeslat jako `Edm.String`, hodnotu, vrátí se znovu prostřednictvím rozhraní API jako Edm.Double klienta</span><span class="sxs-lookup"><span data-stu-id="7b9b7-387">Tables: Adding, editing, or importing an entity that has a property with an ambiguously numeric value, such as "1" or "1.0", and the user tries to send it as an `Edm.String`, the value will come back through the client API as an Edm.Double</span></span>

<span data-ttu-id="7b9b7-388">03/31/2016</span><span class="sxs-lookup"><span data-stu-id="7b9b7-388">03/31/2016</span></span>

### <a name="version-07201603250"></a><span data-ttu-id="7b9b7-389">Verze 0.7.20160325.0</span><span class="sxs-lookup"><span data-stu-id="7b9b7-389">Version 0.7.20160325.0</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/imbgBRHX65A?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a><span data-ttu-id="7b9b7-390">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-390">New</span></span>

* <span data-ttu-id="7b9b7-391">Tabulka podporu: zobrazení, dotazování, export, jejich import a operace CRUD pro entity</span><span class="sxs-lookup"><span data-stu-id="7b9b7-391">Table support: viewing, querying, export, import, and CRUD operations for entities</span></span>
* <span data-ttu-id="7b9b7-392">Podpora ve frontě: zobrazení, přidání, dequeueing zprávy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-392">Queue support: viewing, adding, dequeueing messages</span></span>
* <span data-ttu-id="7b9b7-393">Generování identifikátory URI SAS pro účty úložiště</span><span class="sxs-lookup"><span data-stu-id="7b9b7-393">Generating SAS URIs for Storage Accounts</span></span>
* <span data-ttu-id="7b9b7-394">Připojování k účtům úložiště s identifikátory URI SAS</span><span class="sxs-lookup"><span data-stu-id="7b9b7-394">Connecting to Storage Accounts with SAS URIs</span></span>
* <span data-ttu-id="7b9b7-395">Oznámení o aktualizacích pro budoucí aktualizace Průzkumníka úložiště</span><span class="sxs-lookup"><span data-stu-id="7b9b7-395">Update notifications for future updates to Storage Explorer</span></span>
* <span data-ttu-id="7b9b7-396">Aktualizované vzhled a chování</span><span class="sxs-lookup"><span data-stu-id="7b9b7-396">Updated look and feel</span></span>

#### <a name="fixes"></a><span data-ttu-id="7b9b7-397">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-397">Fixes</span></span>

* <span data-ttu-id="7b9b7-398">Vylepšení výkonu a spolehlivosti</span><span class="sxs-lookup"><span data-stu-id="7b9b7-398">Performance and reliability improvements</span></span>

### <a name="known-issues-amp-mitigations"></a><span data-ttu-id="7b9b7-399">Známé problémy &amp; způsoby zmírnění rizik</span><span class="sxs-lookup"><span data-stu-id="7b9b7-399">Known Issues &amp; Mitigations</span></span>

* <span data-ttu-id="7b9b7-400">Stahování souborů velkého objektu blob nebude správně fungovat – doporučujeme použít AzCopy, když jsme tento problém vyřešit</span><span class="sxs-lookup"><span data-stu-id="7b9b7-400">Download of large blob files does not work correctly - we recommend using AzCopy while we address this issue</span></span> 
* <span data-ttu-id="7b9b7-401">Přihlašovací údaje účtu nebude načíst ani v mezipaměti, pokud se na domovskou složku nelze nalézt nebo nelze zapisovat</span><span class="sxs-lookup"><span data-stu-id="7b9b7-401">Account credentials will not be retrieved nor cached if the home folder cannot be found or cannot be written to</span></span>
* <span data-ttu-id="7b9b7-402">Pokud jsme jsou přidání, úpravy nebo import entita, která má vlastnost s ambiguously číselnou hodnotu, jako je například "1" nebo "1.0", a uživatel se pokusí odeslat jako `Edm.String`, hodnotu, vrátí se znovu prostřednictvím rozhraní API jako Edm.Double klienta</span><span class="sxs-lookup"><span data-stu-id="7b9b7-402">If we are adding, editing, or importing an entity that has a property with an ambiguously numeric value, such as "1" or "1.0", and the user tries to send it as an `Edm.String`, the value will come back through the client API as an Edm.Double</span></span>
* <span data-ttu-id="7b9b7-403">Při importu souborů CSV pomocí víceřádkovým záznamy, může získat data chopped nebo kódována</span><span class="sxs-lookup"><span data-stu-id="7b9b7-403">When importing CSV files with multiline records, the data may get chopped or scrambled</span></span>

<span data-ttu-id="7b9b7-404">02/03/2016</span><span class="sxs-lookup"><span data-stu-id="7b9b7-404">02/03/2016</span></span>

### <a name="version-07201601291"></a><span data-ttu-id="7b9b7-405">Verze 0.7.20160129.1</span><span class="sxs-lookup"><span data-stu-id="7b9b7-405">Version 0.7.20160129.1</span></span>

#### <a name="fixes"></a><span data-ttu-id="7b9b7-406">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-406">Fixes</span></span>

* <span data-ttu-id="7b9b7-407">Lepší celkový výkon při odesílání, stahování a kopírování objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="7b9b7-407">Improved overall performance when uploading, downloading and copying blobs</span></span>

<span data-ttu-id="7b9b7-408">01/14/2016</span><span class="sxs-lookup"><span data-stu-id="7b9b7-408">01/14/2016</span></span>

### <a name="version-07201601050"></a><span data-ttu-id="7b9b7-409">Verze 0.7.20160105.0</span><span class="sxs-lookup"><span data-stu-id="7b9b7-409">Version 0.7.20160105.0</span></span>

#### <a name="new"></a><span data-ttu-id="7b9b7-410">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-410">New</span></span>

* <span data-ttu-id="7b9b7-411">Podpora Linuxu (parity funkcí OSX)</span><span class="sxs-lookup"><span data-stu-id="7b9b7-411">Linux support (parity features to OSX)</span></span>
* <span data-ttu-id="7b9b7-412">Přidat kontejnery objektů blob s podpisy sdíleného přístupu (SAS) klíčem.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-412">Add blob containers with Shared Access Signatures (SAS) key</span></span>
* <span data-ttu-id="7b9b7-413">Přidat účty úložiště pro Azure Čína</span><span class="sxs-lookup"><span data-stu-id="7b9b7-413">Add Storage Accounts for Azure China</span></span>
* <span data-ttu-id="7b9b7-414">Přidat účty úložiště s vlastní koncové body</span><span class="sxs-lookup"><span data-stu-id="7b9b7-414">Add Storage Accounts with custom endpoints</span></span>
* <span data-ttu-id="7b9b7-415">Otevřít a zobrazit obsah textu a obrázků objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="7b9b7-415">Open and view the contents text and picture blobs</span></span>
* <span data-ttu-id="7b9b7-416">Zobrazit a upravit vlastnosti objektů blob a metadat</span><span class="sxs-lookup"><span data-stu-id="7b9b7-416">View and edit blob properties and metadata</span></span>

#### <a name="fixes"></a><span data-ttu-id="7b9b7-417">Opravy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-417">Fixes</span></span>

* <span data-ttu-id="7b9b7-418">Pevná: nahrávání nebo stahování velkého počtu objektů BLOB (500 +) může v některých případech způsobit aplikaci bílé obrazovku.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-418">Fixed: uploading or download a large number of blobs (500+) may sometimes cause the app to have a white screen</span></span> 
* <span data-ttu-id="7b9b7-419">Opravené: při nastavení úrovně veřejný přístup kontejner objektů blob, nová hodnota se neaktualizuje dokud znovu nenastavíte zaměřuje na kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-419">Fixed: when setting blob container public access level, the new value is not updated until you re-set the focus on the container.</span></span> <span data-ttu-id="7b9b7-420">Navíc dialogu vždy použije jako výchozí "Žádné veřejný přístup" a ne skutečné aktuální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-420">Also, the dialog always defaults to "No public access", and not the actual current value.</span></span>
* <span data-ttu-id="7b9b7-421">Podpora lepší celkový klávesnice nebo usnadnění a uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="7b9b7-421">Better overall keyboard/accessibility and UI support</span></span>
* <span data-ttu-id="7b9b7-422">Cesta ke stránce Historie text zalomen během procesu je dlouhá s bílými oblasti</span><span class="sxs-lookup"><span data-stu-id="7b9b7-422">Breadcrumbs history text wraps when it's long with white space</span></span>
* <span data-ttu-id="7b9b7-423">Dialogové okno SAS podporuje ověřování vstupu</span><span class="sxs-lookup"><span data-stu-id="7b9b7-423">SAS dialog supports input validation</span></span>
* <span data-ttu-id="7b9b7-424">Místní úložiště bude nadále k dispozici i v případě, že vypršela platnost přihlašovací údaje uživatele</span><span class="sxs-lookup"><span data-stu-id="7b9b7-424">Local storage continues to be available even if user credentials have expired</span></span>
* <span data-ttu-id="7b9b7-425">Při odstranění kontejner objektů blob otevřenou Průzkumník objektů blob na pravé straně je uzavřený</span><span class="sxs-lookup"><span data-stu-id="7b9b7-425">When an opened blob container is deleted, the blob explorer on the right side is closed</span></span>

#### <a name="known-issues"></a><span data-ttu-id="7b9b7-426">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-426">Known Issues</span></span>

* <span data-ttu-id="7b9b7-427">Instalace Linux musí RSZ verze aktualizovat nebo upgradovat – kroky pro upgrade jsou níže:</span><span class="sxs-lookup"><span data-stu-id="7b9b7-427">Linux install needs gcc version updated or upgraded – steps to upgrade are below:</span></span> 
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

<span data-ttu-id="7b9b7-428">11/18/2015</span><span class="sxs-lookup"><span data-stu-id="7b9b7-428">11/18/2015</span></span>
### <a name="version-07201511160"></a><span data-ttu-id="7b9b7-429">Verze 0.7.20151116.0</span><span class="sxs-lookup"><span data-stu-id="7b9b7-429">Version 0.7.20151116.0</span></span>

#### <a name="new"></a><span data-ttu-id="7b9b7-430">Nový</span><span class="sxs-lookup"><span data-stu-id="7b9b7-430">New</span></span>

* <span data-ttu-id="7b9b7-431">systému macOS a verze systému Windows</span><span class="sxs-lookup"><span data-stu-id="7b9b7-431">macOS, and Windows versions</span></span>
* <span data-ttu-id="7b9b7-432">Přihlaste se k zobrazení účtů úložiště – pomocí účtu organizace, Microsoft Account, 2FA atd.</span><span class="sxs-lookup"><span data-stu-id="7b9b7-432">Sign in to view your Storage Accounts – use your Org Account, Microsoft Account, 2FA, etc.</span></span>
* <span data-ttu-id="7b9b7-433">Vývoj pro místní úložiště (pomocí emulátoru úložiště pouze pro systém Windows)</span><span class="sxs-lookup"><span data-stu-id="7b9b7-433">Local development storage (use storage emulator, Windows-only)</span></span>
* <span data-ttu-id="7b9b7-434">Azure Resource Manager a klasický podporu prostředků</span><span class="sxs-lookup"><span data-stu-id="7b9b7-434">Azure Resource Manager and Classic resource support</span></span>
* <span data-ttu-id="7b9b7-435">Vytvářet a odstraňovat objekty BLOB, fronty nebo tabulky</span><span class="sxs-lookup"><span data-stu-id="7b9b7-435">Create and delete blobs, queues, or tables</span></span>
* <span data-ttu-id="7b9b7-436">Vyhledejte konkrétní objekty BLOB, fronty nebo tabulky</span><span class="sxs-lookup"><span data-stu-id="7b9b7-436">Search for specific blobs, queues, or tables</span></span>
* <span data-ttu-id="7b9b7-437">Prozkoumejte obsah kontejnery objektů blob</span><span class="sxs-lookup"><span data-stu-id="7b9b7-437">Explore the contents of blob containers</span></span>
* <span data-ttu-id="7b9b7-438">Zobrazení a procházení adresářů</span><span class="sxs-lookup"><span data-stu-id="7b9b7-438">View and navigate through directories</span></span>
* <span data-ttu-id="7b9b7-439">Odesílání, stahování a odstraňování objektů BLOB a složky</span><span class="sxs-lookup"><span data-stu-id="7b9b7-439">Upload, download, and delete blobs and folders</span></span>
* <span data-ttu-id="7b9b7-440">Zobrazit a upravit vlastnosti objektů blob a metadat</span><span class="sxs-lookup"><span data-stu-id="7b9b7-440">View and edit blob properties and metadata</span></span>
* <span data-ttu-id="7b9b7-441">Generovat klíče SAS</span><span class="sxs-lookup"><span data-stu-id="7b9b7-441">Generate SAS keys</span></span>
* <span data-ttu-id="7b9b7-442">Spravovat a vytvořit uložené zásady přístupu (SAP)</span><span class="sxs-lookup"><span data-stu-id="7b9b7-442">Manage and create Stored Access Policies (SAP)</span></span>
* <span data-ttu-id="7b9b7-443">Hledat objekty BLOB podle předpony</span><span class="sxs-lookup"><span data-stu-id="7b9b7-443">Search for blobs by prefix</span></span>
* <span data-ttu-id="7b9b7-444">Přetáhněte n vyřadit souborů k odeslání nebo stažení</span><span class="sxs-lookup"><span data-stu-id="7b9b7-444">Drag 'n drop files to upload or download</span></span>

#### <a name="known-issues"></a><span data-ttu-id="7b9b7-445">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="7b9b7-445">Known Issues</span></span>

* <span data-ttu-id="7b9b7-446">Při nastavení úrovně veřejný přístup kontejner objektů blob, nová hodnota není aktualizován, dokud znovu nenastavíte zaměřuje na kontejneru</span><span class="sxs-lookup"><span data-stu-id="7b9b7-446">When setting blob container public access level, the new value is not updated until you re-set the focus on the container</span></span>
* <span data-ttu-id="7b9b7-447">Když otevřete dialogové okno nastavit úroveň veřejný přístup, vždy zobrazuje "Žádné veřejný přístup" jako výchozí a ne skutečné aktuální hodnota</span><span class="sxs-lookup"><span data-stu-id="7b9b7-447">When you open the dialog to set the public access level, it always shows "No public access" as the default, and not the actual current value</span></span>
* <span data-ttu-id="7b9b7-448">Nelze přejmenovat stažené objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="7b9b7-448">Cannot rename downloaded blobs</span></span>
* <span data-ttu-id="7b9b7-449">Položky protokolu aktivity se někdy "zablokuje" v v průběhu stavu, když dojde k chybě, a není zobrazit chyba</span><span class="sxs-lookup"><span data-stu-id="7b9b7-449">Activity log entries will sometimes get "stuck" in an in progress state when an error occurs, and the error is not displayed</span></span>
* <span data-ttu-id="7b9b7-450">Někdy dojde k chybě nebo změní zcela bílé při pokusu o odeslání nebo stažení velkého počtu objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="7b9b7-450">Sometimes crashes or turns completely white when trying to upload or download a large number of blobs</span></span>
* <span data-ttu-id="7b9b7-451">Někdy zrušení operace kopírování nefunguje</span><span class="sxs-lookup"><span data-stu-id="7b9b7-451">Sometimes canceling a copy operation does not work</span></span>
* <span data-ttu-id="7b9b7-452">Při vytvoření kontejneru (objektů blob nebo fronty nebo tabulky), pokud vstupní neplatný název a pokračovat ve vytváření jiné v rámci typu jiný kontejner nelze nastavit fokus na nový typ</span><span class="sxs-lookup"><span data-stu-id="7b9b7-452">During creating a container (blob/queue/table), if you input an invalid name and proceed to create another under a different container type you cannot set focus on the new type</span></span>
* <span data-ttu-id="7b9b7-453">Nelze vytvořit novou složku nebo přejmenujte složku</span><span class="sxs-lookup"><span data-stu-id="7b9b7-453">Can't create new folder or rename folder</span></span>




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md