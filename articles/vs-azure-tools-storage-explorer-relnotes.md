---
title: "Poznámky k verzi aaaMicrosoft Azure Storage Explorer (Preview) | Microsoft Docs"
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
ms.openlocfilehash: 44ff6dc8e2015f4eb71fa13098b832fbbf48ccac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-storage-explorer-preview-release-notes"></a><span data-ttu-id="bab56-103">Poznámky k verzi Microsoft Azure Storage Explorer (Preview)</span><span class="sxs-lookup"><span data-stu-id="bab56-103">Microsoft Azure Storage Explorer (Preview) release notes</span></span>

<span data-ttu-id="bab56-104">Tento článek obsahuje hello verze, kterou verzi poznámky pro Azure Storage Explorer 0.8.16 (Preview), a také poznámky k verzi pro předchozí verze.</span><span class="sxs-lookup"><span data-stu-id="bab56-104">This article contains hello release notes for Azure Storage Explorer 0.8.16 (Preview) release, as well as release notes for previous versions.</span></span>

<span data-ttu-id="bab56-105">[Microsoft Azure Storage Explorer (Preview)](./vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, která vám umožní tooeasily práci s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="bab56-105">[Microsoft Azure Storage Explorer (Preview)](./vs-azure-tools-storage-manage-with-storage-explorer.md) is a standalone app that enables you tooeasily work with Azure Storage data on Windows, macOS, and Linux.</span></span>

## <a name="version-0816-preview"></a><span data-ttu-id="bab56-106">Verze 0.8.16 (Preview)</span><span class="sxs-lookup"><span data-stu-id="bab56-106">Version 0.8.16 (Preview)</span></span>
<span data-ttu-id="bab56-107">8/21/2017</span><span class="sxs-lookup"><span data-stu-id="bab56-107">8/21/2017</span></span>

### <a name="download-azure-storage-explorer-0816-preview"></a><span data-ttu-id="bab56-108">Stažení Azure Storage Explorer 0.8.16 (Preview)</span><span class="sxs-lookup"><span data-stu-id="bab56-108">Download Azure Storage Explorer 0.8.16 (Preview)</span></span>
- [<span data-ttu-id="bab56-109">Azure Storage Explorer (Preview) 0.8.16 pro Windows</span><span class="sxs-lookup"><span data-stu-id="bab56-109">Azure Storage Explorer 0.8.16 (Preview) for Windows</span></span>](https://go.microsoft.com/fwlink/?LinkId=708343)
- [<span data-ttu-id="bab56-110">Azure Storage Explorer (Preview) 0.8.16 pro Mac</span><span class="sxs-lookup"><span data-stu-id="bab56-110">Azure Storage Explorer 0.8.16 (Preview) for Mac</span></span>](https://go.microsoft.com/fwlink/?LinkId=708342)
- [<span data-ttu-id="bab56-111">Azure Storage Explorer (Preview) 0.8.16 pro Linux</span><span class="sxs-lookup"><span data-stu-id="bab56-111">Azure Storage Explorer 0.8.16 (Preview) for Linux</span></span>](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a><span data-ttu-id="bab56-112">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-112">New</span></span>
* <span data-ttu-id="bab56-113">Při otevření objektu blob Storage Explorer zobrazí výzvu tooupload hello stáhnout soubor Pokud je zjištěna změna</span><span class="sxs-lookup"><span data-stu-id="bab56-113">When you open a blob, Storage Explorer will prompt you tooupload hello downloaded file if a change is detected</span></span>
* <span data-ttu-id="bab56-114">Rozšířené zásobník Azure přihlašování uživatelů</span><span class="sxs-lookup"><span data-stu-id="bab56-114">Enhanced Azure Stack sign-in experience</span></span>
* <span data-ttu-id="bab56-115">Vylepšené hello výkonu nahrávání nebo stahování mnoho malých souborů na hello stejný čas</span><span class="sxs-lookup"><span data-stu-id="bab56-115">Improved hello performance of uploading/downloading many small files at hello same time</span></span>


### <a name="fixes"></a><span data-ttu-id="bab56-116">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-116">Fixes</span></span>
* <span data-ttu-id="bab56-117">Pro některé typy objektů blob volba příliš "replace" při nahrávání došlo ke konfliktu někdy výsledkem by hello nahrávání Probíhá restartování.</span><span class="sxs-lookup"><span data-stu-id="bab56-117">For some blob types, choosing too"replace" during an upload conflict would sometimes result in hello upload being restarted.</span></span> 
* <span data-ttu-id="bab56-118">Ve verzi 0.8.15 by někdy nahrávání stalace v 99 %.</span><span class="sxs-lookup"><span data-stu-id="bab56-118">In version 0.8.15, uploads would sometimes stall at 99%.</span></span>
* <span data-ttu-id="bab56-119">Při nahrávání souborů tooa sdílené složky, pokud jste zvolili tooupload tooa directory, která ještě neexistuje, dojde k selhání odeslání.</span><span class="sxs-lookup"><span data-stu-id="bab56-119">When uploading files tooa file share, if you chose tooupload tooa directory which did not yet exist, your upload would fail.</span></span>
* <span data-ttu-id="bab56-120">Storage Explorer byla nesprávně generování časová razítka pro sdílené přístupové podpisy a tabulku.</span><span class="sxs-lookup"><span data-stu-id="bab56-120">Storage Explorer was incorrectly generating time stamps for shared access signatures and table queries.</span></span>


<span data-ttu-id="bab56-121">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-121">Known Issues</span></span>
* <span data-ttu-id="bab56-122">Pomocí názvu a klíče připojovací řetězec momentálně nefunguje.</span><span class="sxs-lookup"><span data-stu-id="bab56-122">Using a name and key connection string does not currently work.</span></span> <span data-ttu-id="bab56-123">Se problém bude vyřešený v příštím vydání hello.</span><span class="sxs-lookup"><span data-stu-id="bab56-123">It will be fixed in hello next release.</span></span> <span data-ttu-id="bab56-124">Do té doby můžete připojit pomocí názvu a klíče.</span><span class="sxs-lookup"><span data-stu-id="bab56-124">Until then you can use attach with name and key.</span></span>
* <span data-ttu-id="bab56-125">Pokud se pokusíte tooopen soubor s neplatným názvem souboru systému Windows, způsobí hello stažení souboru nebyla nalezena chyba.</span><span class="sxs-lookup"><span data-stu-id="bab56-125">If you try tooopen a file with an invalid Windows file name, hello download will result in a file not found error.</span></span>
* <span data-ttu-id="bab56-126">Po kliknutí na tlačítko Storno"na úlohu, může trvat nějakou dobu toocancel této úlohy.</span><span class="sxs-lookup"><span data-stu-id="bab56-126">After clicking "Cancel" on a task, it may take a while for that task toocancel.</span></span> <span data-ttu-id="bab56-127">Jedná se o omezení hello knihovně uzlu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="bab56-127">This is a limitation of hello Azure Storage Node library.</span></span>
* <span data-ttu-id="bab56-128">Po dokončení nahrávání objektů blob, se aktualizují hello kartu, která zahájila odesílání hello.</span><span class="sxs-lookup"><span data-stu-id="bab56-128">After completing a blob upload, hello tab which initiated hello upload is refreshed.</span></span> <span data-ttu-id="bab56-129">Toto je liší od předchozí chování a budou také způsobit toobe přesměrováni zpět hello kontejneru, který se v kořenovém adresáři toohello.</span><span class="sxs-lookup"><span data-stu-id="bab56-129">This is a change from previous behavior, and will also cause you toobe taken back toohello root of hello container you are in.</span></span>
* <span data-ttu-id="bab56-130">Pokud se rozhodnete hello nesprávný certifikát PIN kód nebo čipová karta, bude nutné toorestart v pořadí toohave Storage Explorer zapomněli rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="bab56-130">If you choose hello wrong PIN/Smartcard certificate, then you will need toorestart in order toohave Storage Explorer forget that decision.</span></span>
* <span data-ttu-id="bab56-131">panel nastavení Hello účtu může zobrazovat, je nutné, aby tooreenter pověření toofilter odběry.</span><span class="sxs-lookup"><span data-stu-id="bab56-131">hello account settings panel may show that you need tooreenter credentials toofilter subscriptions.</span></span>
* <span data-ttu-id="bab56-132">Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky.</span><span class="sxs-lookup"><span data-stu-id="bab56-132">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="bab56-133">Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entity.</span><span class="sxs-lookup"><span data-stu-id="bab56-133">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="bab56-134">I když zásobník Azure aktuálně nepodporuje sdílených složek, sdílených složek uzel se objeví stále v připojené účtu úložiště Azure zásobníku.</span><span class="sxs-lookup"><span data-stu-id="bab56-134">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span>
* <span data-ttu-id="bab56-135">Pro uživatele v Ubuntu 14.04, budete potřebovat tooensure RSZ zapnutý toodate – to můžete provést spuštěním hello následující příkazy a restartujte svůj počítač:</span><span class="sxs-lookup"><span data-stu-id="bab56-135">For users on Ubuntu 14.04, you will need tooensure GCC is up toodate - this can be done by running hello following commands, and then restarting your machine:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* <span data-ttu-id="bab56-136">Pro uživatele v Ubuntu č. 17.04 budete potřebovat tooinstall GConf – to můžete provést spuštění hello následující příkazy a restartujte svůj počítač:</span><span class="sxs-lookup"><span data-stu-id="bab56-136">For users on Ubuntu 17.04, you will need tooinstall GConf - this can be done by running hello following commands, and then restarting your machine:</span></span>

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0814-preview"></a><span data-ttu-id="bab56-137">Verze 0.8.14 (Preview)</span><span class="sxs-lookup"><span data-stu-id="bab56-137">Version 0.8.14 (Preview)</span></span>
<span data-ttu-id="bab56-138">06/22/2017</span><span class="sxs-lookup"><span data-stu-id="bab56-138">06/22/2017</span></span>

### <a name="download-azure-storage-explorer-0814-preview"></a><span data-ttu-id="bab56-139">Stažení Azure Storage Explorer 0.8.14 (Preview)</span><span class="sxs-lookup"><span data-stu-id="bab56-139">Download Azure Storage Explorer 0.8.14 (Preview)</span></span>
* [<span data-ttu-id="bab56-140">Stažení Azure Storage Explorer (Preview) 0.8.14 pro Windows</span><span class="sxs-lookup"><span data-stu-id="bab56-140">Download Azure Storage Explorer 0.8.14 (Preview) for Windows</span></span>](https://go.microsoft.com/fwlink/?LinkId=809306)
* [<span data-ttu-id="bab56-141">Stažení Azure Storage Explorer (Preview) 0.8.14 pro Mac</span><span class="sxs-lookup"><span data-stu-id="bab56-141">Download Azure Storage Explorer 0.8.14 (Preview) for Mac</span></span>](https://go.microsoft.com/fwlink/?LinkId=809307)
* [<span data-ttu-id="bab56-142">Stažení Azure Storage Explorer (Preview) 0.8.14 pro Linux</span><span class="sxs-lookup"><span data-stu-id="bab56-142">Download Azure Storage Explorer 0.8.14 (Preview) for Linux</span></span>](https://go.microsoft.com/fwlink/?LinkId=809308)

### <a name="new"></a><span data-ttu-id="bab56-143">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-143">New</span></span>

* <span data-ttu-id="bab56-144">Aktualizované verze too1.7.2 elektronovým v pořadí tootake výhod několik důležité aktualizace zabezpečení</span><span class="sxs-lookup"><span data-stu-id="bab56-144">Updated Electron version too1.7.2 in order tootake advantage of several critical security updates</span></span>
* <span data-ttu-id="bab56-145">Nyní můžete rychle k Průvodci odstraňováním potíží online hello z nabídky Nápověda hello</span><span class="sxs-lookup"><span data-stu-id="bab56-145">You can now quickly access hello online troubleshooting guide from hello help menu</span></span>
* <span data-ttu-id="bab56-146">Storage Explorer řešení potíží s [Průvodce][2]</span><span class="sxs-lookup"><span data-stu-id="bab56-146">Storage Explorer Troubleshooting [Guide][2]</span></span>
* <span data-ttu-id="bab56-147">[Pokyny] [ 3] na připojení tooan předplatné Azure zásobníku</span><span class="sxs-lookup"><span data-stu-id="bab56-147">[Instructions][3] on connecting tooan Azure Stack subscription</span></span>

### <a name="known-issues"></a><span data-ttu-id="bab56-148">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-148">Known Issues</span></span>

* <span data-ttu-id="bab56-149">Tlačítka v dialogovém okně potvrzení hello odstranit složku nezaregistrujete kliknutími myši hello v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="bab56-149">Buttons on hello delete folder confirmation dialog don't register with hello mouse clicks on Linux.</span></span> <span data-ttu-id="bab56-150">Alternativní řešení je toouse hello zadejte klíč</span><span class="sxs-lookup"><span data-stu-id="bab56-150">Workaround is toouse hello Enter key</span></span>
* <span data-ttu-id="bab56-151">Pokud se rozhodnete hello nesprávný certifikát PIN kód nebo čipová karta, bude nutné toorestart v pořadí toohave Storage Explorer zapomněli hello rozhodnutí</span><span class="sxs-lookup"><span data-stu-id="bab56-151">If you choose hello wrong PIN/Smartcard certificate then you will need toorestart in order toohave Storage Explorer forget hello decision</span></span>
* <span data-ttu-id="bab56-152">S více než 3 skupiny objektů BLOB nebo ukládání na hello stejné souborů čas může způsobit chyby</span><span class="sxs-lookup"><span data-stu-id="bab56-152">Having more than 3 groups of blobs or files uploading at hello same time may cause errors</span></span>
* <span data-ttu-id="bab56-153">panel nastavení Hello účtu může zobrazovat, je nutné, přihlašovací údaje tooreenter v pořadí toofilter odběrů</span><span class="sxs-lookup"><span data-stu-id="bab56-153">hello account settings panel may show that you need tooreenter credentials in order toofilter subscriptions</span></span>
* <span data-ttu-id="bab56-154">Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky.</span><span class="sxs-lookup"><span data-stu-id="bab56-154">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="bab56-155">Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entity.</span><span class="sxs-lookup"><span data-stu-id="bab56-155">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="bab56-156">I když zásobník Azure aktuálně nepodporuje sdílené složky, sdílené složky uzel se objeví stále v připojené účtu úložiště Azure zásobníku.</span><span class="sxs-lookup"><span data-stu-id="bab56-156">Although Azure Stack doesn't currently support File Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="bab56-157">Ubuntu 14.04 instalace potřeby RSZ verze aktualizovat nebo upgradovat – tooupgrade kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="bab56-157">Ubuntu 14.04 install needs gcc version updated or upgraded – steps tooupgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```




## <a name="previous-releases"></a><span data-ttu-id="bab56-158">Předchozí verze</span><span class="sxs-lookup"><span data-stu-id="bab56-158">Previous releases</span></span>

* [<span data-ttu-id="bab56-159">Verze 0.8.13</span><span class="sxs-lookup"><span data-stu-id="bab56-159">Version 0.8.13</span></span>](#version-0813)
* [<span data-ttu-id="bab56-160">Verze 0.8.12 / 0.8.11 / 0.8.10</span><span class="sxs-lookup"><span data-stu-id="bab56-160">Version 0.8.12 / 0.8.11 / 0.8.10</span></span>](#version-0812--0811--0810)
* [<span data-ttu-id="bab56-161">Verze 0.8.9 nebo 0.8.8</span><span class="sxs-lookup"><span data-stu-id="bab56-161">Version 0.8.9 / 0.8.8</span></span>](#version-089--088)
* [<span data-ttu-id="bab56-162">Verze 0.8.7</span><span class="sxs-lookup"><span data-stu-id="bab56-162">Version 0.8.7</span></span>](#version-087)
* [<span data-ttu-id="bab56-163">Verze 0.8.6</span><span class="sxs-lookup"><span data-stu-id="bab56-163">Version 0.8.6</span></span>](#version-086)
* [<span data-ttu-id="bab56-164">Verze 0.8.5</span><span class="sxs-lookup"><span data-stu-id="bab56-164">Version 0.8.5</span></span>](#version-085)
* [<span data-ttu-id="bab56-165">Verze 0.8.4</span><span class="sxs-lookup"><span data-stu-id="bab56-165">Version 0.8.4</span></span>](#version-084)
* [<span data-ttu-id="bab56-166">Verze 0.8.3</span><span class="sxs-lookup"><span data-stu-id="bab56-166">Version 0.8.3</span></span>](#version-083)
* [<span data-ttu-id="bab56-167">Verze 0.8.2</span><span class="sxs-lookup"><span data-stu-id="bab56-167">Version 0.8.2</span></span>](#version-082)
* [<span data-ttu-id="bab56-168">Verze 0.8.0</span><span class="sxs-lookup"><span data-stu-id="bab56-168">Version 0.8.0</span></span>](#version-080)
* [<span data-ttu-id="bab56-169">Verze 0.7.20160509.0</span><span class="sxs-lookup"><span data-stu-id="bab56-169">Version 0.7.20160509.0</span></span>](#version-07201605090)
* [<span data-ttu-id="bab56-170">Verze 0.7.20160325.0</span><span class="sxs-lookup"><span data-stu-id="bab56-170">Version 0.7.20160325.0</span></span>](#version-07201603250)
* [<span data-ttu-id="bab56-171">Verze 0.7.20160129.1</span><span class="sxs-lookup"><span data-stu-id="bab56-171">Version 0.7.20160129.1</span></span>](#version-07201601291)
* [<span data-ttu-id="bab56-172">Verze 0.7.20160105.0</span><span class="sxs-lookup"><span data-stu-id="bab56-172">Version 0.7.20160105.0</span></span>](#version-07201601050)
* [<span data-ttu-id="bab56-173">Verze 0.7.20151116.0</span><span class="sxs-lookup"><span data-stu-id="bab56-173">Version 0.7.20151116.0</span></span>](#version-07201511160)


### <a name="version-0813"></a><span data-ttu-id="bab56-174">Verze 0.8.13</span><span class="sxs-lookup"><span data-stu-id="bab56-174">Version 0.8.13</span></span>
<span data-ttu-id="bab56-175">05/12/2017</span><span class="sxs-lookup"><span data-stu-id="bab56-175">05/12/2017</span></span>

#### <a name="new"></a><span data-ttu-id="bab56-176">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-176">New</span></span>

* <span data-ttu-id="bab56-177">Storage Explorer řešení potíží s [Průvodce][2]</span><span class="sxs-lookup"><span data-stu-id="bab56-177">Storage Explorer Troubleshooting [Guide][2]</span></span>
* <span data-ttu-id="bab56-178">[Pokyny] [ 3] na připojení tooan předplatné Azure zásobníku</span><span class="sxs-lookup"><span data-stu-id="bab56-178">[Instructions][3] on connecting tooan Azure Stack subscription</span></span>

#### <a name="fixes"></a><span data-ttu-id="bab56-179">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-179">Fixes</span></span>

* <span data-ttu-id="bab56-180">Pevná: Odeslání souboru měl vysokou možnost, že nedostatku paměti</span><span class="sxs-lookup"><span data-stu-id="bab56-180">Fixed: File upload had a high chance of causing an out of memory error</span></span>
* <span data-ttu-id="bab56-181">Opravené: Můžete teď přihlásit pomocí PIN kódu nebo čipová karta</span><span class="sxs-lookup"><span data-stu-id="bab56-181">Fixed: You can now sign in with PIN/Smartcard</span></span>
* <span data-ttu-id="bab56-182">Pevná: Otevřete portálu teď funguje s Azure China, Azure v Německu, Azure US Government a Azure zásobníku</span><span class="sxs-lookup"><span data-stu-id="bab56-182">Fixed: Open in Portal now works with Azure China, Azure Germany, Azure US Government, and Azure Stack</span></span>
* <span data-ttu-id="bab56-183">Pevná: Při nahrávání kontejneru objektů blob tooa složky, "Neplatnou operaci" by někdy, dojde k chybě</span><span class="sxs-lookup"><span data-stu-id="bab56-183">Fixed: While uploading a folder tooa blob container, an "Illegal operation" error would sometimes occur</span></span>
* <span data-ttu-id="bab56-184">Pevná: Vybrat vše zakázal při správě snímků</span><span class="sxs-lookup"><span data-stu-id="bab56-184">Fixed: Select all was disabled while managing snapshots</span></span>
* <span data-ttu-id="bab56-185">Opravené: hello metadata objektu blob základní hello může získat přepsána po zobrazením vlastností hello jeho snímků</span><span class="sxs-lookup"><span data-stu-id="bab56-185">Fixed: hello metadata of hello base blob might get overwritten after viewing hello properties of its snapshots</span></span>

#### <a name="known-issues"></a><span data-ttu-id="bab56-186">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-186">Known Issues</span></span>

* <span data-ttu-id="bab56-187">Pokud se rozhodnete hello nesprávný certifikát PIN kód nebo čipová karta, bude nutné toorestart v pořadí toohave Storage Explorer zapomněli hello rozhodnutí</span><span class="sxs-lookup"><span data-stu-id="bab56-187">If you choose hello wrong PIN/Smartcard certificate then you will need toorestart in order toohave Storage Explorer forget hello decision</span></span>
* <span data-ttu-id="bab56-188">Při možnosti příchozí nebo odchozí, úroveň přiblížení hello na okamžik resetován toohello výchozí úroveň</span><span class="sxs-lookup"><span data-stu-id="bab56-188">While zoomed in or out, hello zoom level may momentarily reset toohello default level</span></span>
* <span data-ttu-id="bab56-189">S více než 3 skupiny objektů BLOB nebo ukládání na hello stejné souborů čas může způsobit chyby</span><span class="sxs-lookup"><span data-stu-id="bab56-189">Having more than 3 groups of blobs or files uploading at hello same time may cause errors</span></span>
* <span data-ttu-id="bab56-190">panel nastavení Hello účtu může zobrazovat, je nutné, přihlašovací údaje tooreenter v pořadí toofilter odběrů</span><span class="sxs-lookup"><span data-stu-id="bab56-190">hello account settings panel may show that you need tooreenter credentials in order toofilter subscriptions</span></span>
* <span data-ttu-id="bab56-191">Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky.</span><span class="sxs-lookup"><span data-stu-id="bab56-191">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="bab56-192">Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entity.</span><span class="sxs-lookup"><span data-stu-id="bab56-192">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="bab56-193">I když zásobník Azure aktuálně nepodporuje sdílených složek, sdílených složek uzel se objeví stále v připojené účtu úložiště Azure zásobníku.</span><span class="sxs-lookup"><span data-stu-id="bab56-193">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="bab56-194">Ubuntu 14.04 instalace potřeby RSZ verze aktualizovat nebo upgradovat – tooupgrade kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="bab56-194">Ubuntu 14.04 install needs gcc version updated or upgraded – steps tooupgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812--0811--0810"></a><span data-ttu-id="bab56-195">Verze 0.8.12 / 0.8.11 / 0.8.10</span><span class="sxs-lookup"><span data-stu-id="bab56-195">Version 0.8.12 / 0.8.11 / 0.8.10</span></span>
<span data-ttu-id="bab56-196">04/07/2017</span><span class="sxs-lookup"><span data-stu-id="bab56-196">04/07/2017</span></span>

#### <a name="new"></a><span data-ttu-id="bab56-197">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-197">New</span></span>

* <span data-ttu-id="bab56-198">Storage Explorer se teď automaticky zavře po instalaci aktualizace z upozornění na aktualizace hello</span><span class="sxs-lookup"><span data-stu-id="bab56-198">Storage Explorer will now automatically close when you install an update from hello update notification</span></span>
* <span data-ttu-id="bab56-199">Rychlý přístup na místě poskytuje lepší prostředí pro práci se často používané prostředky</span><span class="sxs-lookup"><span data-stu-id="bab56-199">In-place quick access provides an enhanced experience for working with your frequently accessed resources</span></span>
* <span data-ttu-id="bab56-200">V editoru hello kontejner objektů Blob nyní můžete vidět které virtuálního počítače patří zapůjčených objektů blob</span><span class="sxs-lookup"><span data-stu-id="bab56-200">In hello Blob Container editor, you can now see which virtual machine a leased blob belongs to</span></span>
* <span data-ttu-id="bab56-201">Nyní lze sbalit panel levé straně hello</span><span class="sxs-lookup"><span data-stu-id="bab56-201">You can now collapse hello left side panel</span></span>
* <span data-ttu-id="bab56-202">Zjišťování nyní běží v hello stejný čas jako soubor ke stažení</span><span class="sxs-lookup"><span data-stu-id="bab56-202">Discovery now runs at hello same time as download</span></span>
* <span data-ttu-id="bab56-203">Statistik použití v kontejneru objektů Blob, sdílené složky a tabulku editory toosee hello velikost prostředku nebo výběr hello</span><span class="sxs-lookup"><span data-stu-id="bab56-203">Use Statistics in hello Blob Container, File Share, and Table editors toosee hello size of your resource or selection</span></span>
* <span data-ttu-id="bab56-204">Můžete teď přihlášení tooAzure Active Directory (AAD) na základě účtů Azure zásobníku.</span><span class="sxs-lookup"><span data-stu-id="bab56-204">You can now sign-in tooAzure Active Directory (AAD) based Azure Stack accounts.</span></span> 
* <span data-ttu-id="bab56-205">Můžete teď archivu nahrávání souborů víc než 32 MB tooPremium úložiště účtů</span><span class="sxs-lookup"><span data-stu-id="bab56-205">You can now upload archive files over 32MB tooPremium storage accounts</span></span>
* <span data-ttu-id="bab56-206">Vylepšená podpora usnadnění přístupu</span><span class="sxs-lookup"><span data-stu-id="bab56-206">Improved accessibility support</span></span>
* <span data-ttu-id="bab56-207">Nyní můžete přidat důvěryhodný Base-64 kódovaný certifikáty X.509 SSL pomocí budete tooEdit -&gt; certifikáty SSL -&gt; Import certifikátů</span><span class="sxs-lookup"><span data-stu-id="bab56-207">You can now add trusted Base-64 encoded X.509 SSL certificates by going tooEdit -&gt; SSL Certificates -&gt; Import Certificates</span></span>

#### <a name="fixes"></a><span data-ttu-id="bab56-208">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-208">Fixes</span></span>

* <span data-ttu-id="bab56-209">Opravené: po aktualizovat přihlašovací údaje účtu, hello stromové zobrazení by někdy aktualizace automaticky</span><span class="sxs-lookup"><span data-stu-id="bab56-209">Fixed: after refreshing an account's credentials, hello tree view would sometimes not automatically refresh</span></span>
* <span data-ttu-id="bab56-210">Pevná: generování SAS pro emulátor fronty a tabulky by způsobilo neplatná adresa URL</span><span class="sxs-lookup"><span data-stu-id="bab56-210">Fixed: generating a SAS for emulator queues and tables would result in an invalid URL</span></span>
* <span data-ttu-id="bab56-211">Opravené: prémiové účty úložiště lze nyní rozbalit proxy server je povolen program</span><span class="sxs-lookup"><span data-stu-id="bab56-211">Fixed: premium storage accounts can now be expanded while a proxy is enabled</span></span>
* <span data-ttu-id="bab56-212">Pevná: hello použít tlačítko na hello účty, které stránky správy nebude fungovat, pokud jste měli 1 nebo 0 účty vybrané</span><span class="sxs-lookup"><span data-stu-id="bab56-212">Fixed: hello apply button on hello accounts management page would not work if you had 1 or 0 accounts selected</span></span>
* <span data-ttu-id="bab56-213">Pevné: odesílání objekty BLOB, které vyžadují řešení konfliktu může selhat - fixed v 0.8.11</span><span class="sxs-lookup"><span data-stu-id="bab56-213">Fixed: uploading blobs that require conflict resolutions may fail - fixed in 0.8.11</span></span> 
* <span data-ttu-id="bab56-214">Pevná: odeslání názoru byla porušena v 0.8.11 - fixed v 0.8.12</span><span class="sxs-lookup"><span data-stu-id="bab56-214">Fixed: sending feedback was broken in 0.8.11 - fixed in 0.8.12</span></span> 

#### <a name="known-issues"></a><span data-ttu-id="bab56-215">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-215">Known Issues</span></span>

* <span data-ttu-id="bab56-216">Po upgradu too0.8.10, budete potřebovat toorefresh všechny svoje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="bab56-216">After upgrading too0.8.10, you will need toorefresh all of your credentials.</span></span>
* <span data-ttu-id="bab56-217">Při možnosti příchozí nebo odchozí, úroveň přiblížení hello resetován na okamžik toohello výchozí úroveň.</span><span class="sxs-lookup"><span data-stu-id="bab56-217">While zoomed in or out, hello zoom level may momentarily reset toohello default level.</span></span>
* <span data-ttu-id="bab56-218">Více než 3 skupiny objektů BLOB nebo ukládání na hello stejné souborů čas může způsobit chyby.</span><span class="sxs-lookup"><span data-stu-id="bab56-218">Having more than 3 groups of blobs or files uploading at hello same time may cause errors.</span></span>
* <span data-ttu-id="bab56-219">panel nastavení Hello účtu může zobrazovat, je nutné, přihlašovací údaje tooreenter v pořadí toofilter předplatných.</span><span class="sxs-lookup"><span data-stu-id="bab56-219">hello account settings panel may show that you need tooreenter credentials in order toofilter subscriptions.</span></span>
* <span data-ttu-id="bab56-220">Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky.</span><span class="sxs-lookup"><span data-stu-id="bab56-220">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="bab56-221">Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entity.</span><span class="sxs-lookup"><span data-stu-id="bab56-221">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="bab56-222">I když zásobník Azure aktuálně nepodporuje sdílených složek, sdílených složek uzel se objeví stále v připojené účtu úložiště Azure zásobníku.</span><span class="sxs-lookup"><span data-stu-id="bab56-222">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="bab56-223">Ubuntu 14.04 instalace potřeby RSZ verze aktualizovat nebo upgradovat – tooupgrade kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="bab56-223">Ubuntu 14.04 install needs gcc version updated or upgraded – steps tooupgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-089--088"></a><span data-ttu-id="bab56-224">Verze 0.8.9 nebo 0.8.8</span><span class="sxs-lookup"><span data-stu-id="bab56-224">Version 0.8.9 / 0.8.8</span></span>
<span data-ttu-id="bab56-225">02/23/2017</span><span class="sxs-lookup"><span data-stu-id="bab56-225">02/23/2017</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/R6gonK3cYAc?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/SrRPCm94mfE?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a><span data-ttu-id="bab56-226">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-226">New</span></span>

* <span data-ttu-id="bab56-227">Storage Explorer 0.8.9 automaticky stáhne nejnovější verzi hello aktualizací.</span><span class="sxs-lookup"><span data-stu-id="bab56-227">Storage Explorer 0.8.9 will automatically download hello latest version for updates.</span></span>
* <span data-ttu-id="bab56-228">Opravy hotfix: portálu vygeneruje tooattach identifikátor URI SAS, které účet úložiště by mělo za následek chybu.</span><span class="sxs-lookup"><span data-stu-id="bab56-228">Hotfix: using a portal generated SAS URI tooattach a storage account would result in an error.</span></span>
* <span data-ttu-id="bab56-229">Teď můžete vytvářet, spravovat a zvýšit úroveň snímky objektů blob.</span><span class="sxs-lookup"><span data-stu-id="bab56-229">You can now create, manage, and promote blob snapshots.</span></span>
* <span data-ttu-id="bab56-230">Teď může přihlásit tooAzure účty Čína, Azure v Německu a Azure US Government.</span><span class="sxs-lookup"><span data-stu-id="bab56-230">You can now sign in tooAzure China, Azure Germany, and Azure US Government accounts.</span></span>
* <span data-ttu-id="bab56-231">Nyní můžete měnit úroveň přiblížení hello.</span><span class="sxs-lookup"><span data-stu-id="bab56-231">You can now change hello zoom level.</span></span> <span data-ttu-id="bab56-232">Pomocí možností hello v hello zobrazení nabídky tooZoom v oddálit a resetovat přiblížení.</span><span class="sxs-lookup"><span data-stu-id="bab56-232">Use hello options in hello View menu tooZoom In, Zoom Out, and Reset Zoom.</span></span>
* <span data-ttu-id="bab56-233">Uživatel metadat pro objekty BLOB a soubory jsou nyní podporovány znaky znakové sady Unicode.</span><span class="sxs-lookup"><span data-stu-id="bab56-233">Unicode characters are now supported in user metadata for blobs and files.</span></span>
* <span data-ttu-id="bab56-234">Vylepšení přístupnosti.</span><span class="sxs-lookup"><span data-stu-id="bab56-234">Accessibility improvements.</span></span>
* <span data-ttu-id="bab56-235">Poznámky k verzi Hello další verzi si můžete prohlížet hello aktualizace oznámení.</span><span class="sxs-lookup"><span data-stu-id="bab56-235">hello next version's release notes can be viewed from hello update notification.</span></span> <span data-ttu-id="bab56-236">Můžete také zobrazit hello aktuální poznámky k verzi z nabídky Nápověda hello.</span><span class="sxs-lookup"><span data-stu-id="bab56-236">You can also view hello current release notes from hello Help menu.</span></span>

#### <a name="fixes"></a><span data-ttu-id="bab56-237">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-237">Fixes</span></span>

* <span data-ttu-id="bab56-238">Opravené: číslo verze hello je nyní správně zobrazen v Ovládacích panelech v systému Windows</span><span class="sxs-lookup"><span data-stu-id="bab56-238">Fixed: hello version number is now correctly displayed in Control Panel on Windows</span></span>
* <span data-ttu-id="bab56-239">Pevná: vyhledávání už není omezené too50 000 uzly</span><span class="sxs-lookup"><span data-stu-id="bab56-239">Fixed: search is no longer limited too50,000 nodes</span></span>
* <span data-ttu-id="bab56-240">Opravené: nahrávání tooa sdílené složky spuštěné navždy hello cílový adresář již neexistovala</span><span class="sxs-lookup"><span data-stu-id="bab56-240">Fixed: upload tooa file share spun forever if hello destination directory did not already exist</span></span>
* <span data-ttu-id="bab56-241">Opravené: vylepšení stability dlouho nahrávání a stahování</span><span class="sxs-lookup"><span data-stu-id="bab56-241">Fixed: improved stability for long uploads and downloads</span></span>

#### <a name="known-issues"></a><span data-ttu-id="bab56-242">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-242">Known Issues</span></span>

* <span data-ttu-id="bab56-243">Při možnosti příchozí nebo odchozí, úroveň přiblížení hello resetován na okamžik toohello výchozí úroveň.</span><span class="sxs-lookup"><span data-stu-id="bab56-243">While zoomed in or out, hello zoom level may momentarily reset toohello default level.</span></span>
* <span data-ttu-id="bab56-244">Rychlý přístup pracuje pouze s odběru na základě položky.</span><span class="sxs-lookup"><span data-stu-id="bab56-244">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="bab56-245">V této verzi nepodporuje místních prostředků nebo prostředky, které jsou připojené prostřednictvím klíč nebo tokenu SAS.</span><span class="sxs-lookup"><span data-stu-id="bab56-245">Local resources or resources attached via key or SAS token are not supported in this release.</span></span>
* <span data-ttu-id="bab56-246">Rychlý přístup, může trvat několik sekund toonavigate toohello cílový prostředek, v závislosti na tom, kolik prostředků, které máte.</span><span class="sxs-lookup"><span data-stu-id="bab56-246">It may take Quick Access a few seconds toonavigate toohello target resource, depending on how many resources you have.</span></span>
* <span data-ttu-id="bab56-247">Více než 3 skupiny objektů BLOB nebo ukládání na hello stejné souborů čas může způsobit chyby.</span><span class="sxs-lookup"><span data-stu-id="bab56-247">Having more than 3 groups of blobs or files uploading at hello same time may cause errors.</span></span>

<span data-ttu-id="bab56-248">12/16/2016</span><span class="sxs-lookup"><span data-stu-id="bab56-248">12/16/2016</span></span>
### <a name="version-087"></a><span data-ttu-id="bab56-249">Verze 0.8.7</span><span class="sxs-lookup"><span data-stu-id="bab56-249">Version 0.8.7</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="bab56-250">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-250">New</span></span>

* <span data-ttu-id="bab56-251">Můžete určit, jak tooresolve konflikty od začátku hello aktualizace, stáhnout nebo zkopírujte relace v okně aktivity hello</span><span class="sxs-lookup"><span data-stu-id="bab56-251">You can choose how tooresolve conflicts at hello beginning of an update, download or copy session in hello Activities window</span></span>
* <span data-ttu-id="bab56-252">Najeďte myší na kartě toosee hello úplnou cestu prostředku úložiště hello</span><span class="sxs-lookup"><span data-stu-id="bab56-252">Hover over a tab toosee hello full path of hello storage resource</span></span>
* <span data-ttu-id="bab56-253">Když kliknete na kartě, synchronizuje s jeho umístění v hello levé navigační podokno</span><span class="sxs-lookup"><span data-stu-id="bab56-253">When you click on a tab, it synchronizes with its location in hello left side navigation pane</span></span>

#### <a name="fixes"></a><span data-ttu-id="bab56-254">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-254">Fixes</span></span>

* <span data-ttu-id="bab56-255">Opravené: Storage Explorer je nyní důvěryhodné aplikace v systému Mac</span><span class="sxs-lookup"><span data-stu-id="bab56-255">Fixed: Storage Explorer is now a trusted app on Mac</span></span>
* <span data-ttu-id="bab56-256">Opravené: Ubuntu 14.04 se znovu podporuje</span><span class="sxs-lookup"><span data-stu-id="bab56-256">Fixed: Ubuntu 14.04 is again supported</span></span>
* <span data-ttu-id="bab56-257">Opravené: Někdy hello přidat účet, který bliká uživatelského rozhraní při načítání odběrů</span><span class="sxs-lookup"><span data-stu-id="bab56-257">Fixed: Sometimes hello add account UI flashes when loading subscriptions</span></span>
* <span data-ttu-id="bab56-258">Opravené: Někdy ne všechny prostředky úložiště byly uvedené v hello levé navigační podokno</span><span class="sxs-lookup"><span data-stu-id="bab56-258">Fixed: Sometimes not all storage resources were listed in hello left side navigation pane</span></span>
* <span data-ttu-id="bab56-259">Opravené: podokna akcí hello někdy zobrazí prázdné akce</span><span class="sxs-lookup"><span data-stu-id="bab56-259">Fixed: hello action pane sometimes displayed empty actions</span></span>
* <span data-ttu-id="bab56-260">Opravené: velikost okna hello z hello posledním zavření relace je nyní uchována</span><span class="sxs-lookup"><span data-stu-id="bab56-260">Fixed: hello window size from hello last closed session is now retained</span></span>
* <span data-ttu-id="bab56-261">Pevná: Můžete otevřít několik karet pro hello stejného zdroje pomocí hello kontextové nabídky</span><span class="sxs-lookup"><span data-stu-id="bab56-261">Fixed: You can open multiple tabs for hello same resource using hello context menu</span></span>

#### <a name="known-issues"></a><span data-ttu-id="bab56-262">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-262">Known Issues</span></span>

* <span data-ttu-id="bab56-263">Rychlý přístup pracuje pouze s odběru na základě položky.</span><span class="sxs-lookup"><span data-stu-id="bab56-263">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="bab56-264">V této verzi nepodporuje místních prostředků nebo prostředky, které jsou připojené prostřednictvím klíč nebo tokenu SAS</span><span class="sxs-lookup"><span data-stu-id="bab56-264">Local resources or resources attached via key or SAS token are not supported in this release</span></span>
* <span data-ttu-id="bab56-265">Rychlý přístup může trvat několik sekund toonavigate toohello cílový prostředek, v závislosti na tom, kolik prostředků máte</span><span class="sxs-lookup"><span data-stu-id="bab56-265">It may take Quick Access a few seconds toonavigate toohello target resource, depending on how many resources you have</span></span>
* <span data-ttu-id="bab56-266">S více než 3 skupiny objektů BLOB nebo ukládání na hello stejné souborů čas může způsobit chyby</span><span class="sxs-lookup"><span data-stu-id="bab56-266">Having more than 3 groups of blobs or files uploading at hello same time may cause errors</span></span>
* <span data-ttu-id="bab56-267">Obslužné rutiny vyhledávání hledání mezi uzly přibližně 50 000 - poté výkonu může být ovlivněno nebo může způsobit, že neošetřené výjimky</span><span class="sxs-lookup"><span data-stu-id="bab56-267">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted or may cause unhandled exception</span></span>
* <span data-ttu-id="bab56-268">Pro hello první přihlášení pomocí hello Průzkumníka úložiště v systému macOS, může se zobrazit více výzev žádostí o řetězce klíčů tooaccess oprávnění uživatele.</span><span class="sxs-lookup"><span data-stu-id="bab56-268">For hello first time using hello Storage Explorer on macOS, you might see multiple prompts asking for user's permission tooaccess keychain.</span></span> <span data-ttu-id="bab56-269">Doporučujeme že vybrat vždy povolit tak nebude znovu objeví hello řádku</span><span class="sxs-lookup"><span data-stu-id="bab56-269">We suggest you select Always Allow so hello prompt won't show up again</span></span>

<span data-ttu-id="bab56-270">11/18/2016</span><span class="sxs-lookup"><span data-stu-id="bab56-270">11/18/2016</span></span>
### <a name="version-086"></a><span data-ttu-id="bab56-271">Verze 0.8.6</span><span class="sxs-lookup"><span data-stu-id="bab56-271">Version 0.8.6</span></span>

#### <a name="new"></a><span data-ttu-id="bab56-272">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-272">New</span></span>

* <span data-ttu-id="bab56-273">Můžete teď pin nejčastěji používá služby toohello rychlý přístup snadno navigace</span><span class="sxs-lookup"><span data-stu-id="bab56-273">You can now pin most frequently used services toohello Quick Access for easy navigation</span></span>
* <span data-ttu-id="bab56-274">Nyní lze otevřít více editory v různých kartách.</span><span class="sxs-lookup"><span data-stu-id="bab56-274">You can now open multiple editors in different tabs.</span></span> <span data-ttu-id="bab56-275">Jedním kliknutím tooopen dočasné karta; dvakrát klikněte na tlačítko tooopen trvalé kartě. Můžete také kliknutím na hello dočasné karta toomake ho trvalé karta</span><span class="sxs-lookup"><span data-stu-id="bab56-275">Single click tooopen a temporary tab; double click tooopen a permanent tab. You can also click on hello temporary tab toomake it a permanent tab</span></span>
* <span data-ttu-id="bab56-276">Jsme provedli znatelné výkonu a zlepšení stability pro odesílání a stahování souborů, zejména u velkých souborů na rychlé počítače</span><span class="sxs-lookup"><span data-stu-id="bab56-276">We have made noticeable performance and stability improvements for uploads and downloads, especially for large files on fast machines</span></span>
* <span data-ttu-id="bab56-277">Prázdné složky "virtuální" teď lze vytvořit v kontejnerech objektů blob</span><span class="sxs-lookup"><span data-stu-id="bab56-277">Empty "virtual" folders can now be created in blob containers</span></span>
* <span data-ttu-id="bab56-278">Mít znovu zavedli jsme vymezené vyhledávání s naší nové rozšířené dílčí řetězec hledání, takže Teď máte dvě možnosti pro vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="bab56-278">We have re-introduced scoped search with our new enhanced substring search, so you now have two options for searching:</span></span> 
    * <span data-ttu-id="bab56-279">Globální vyhledávání – zadejte do textového pole hledání hello jenom hledaný termín</span><span class="sxs-lookup"><span data-stu-id="bab56-279">Global search - just enter a search term into hello search textbox</span></span>
    * <span data-ttu-id="bab56-280">Vymezené vyhledávání – klikněte na hello lupy ikonu další tooa uzel, pak přidat konec hledání termín toohello hello cesty, nebo klikněte pravým tlačítkem a vyberte "Vyhledávání z zde"</span><span class="sxs-lookup"><span data-stu-id="bab56-280">Scoped search - click hello magnifying glass icon next tooa node, then add a search term toohello end of hello path, or right-click and select "Search from Here"</span></span>
* <span data-ttu-id="bab56-281">Jsme přidali různé motivy: Light (výchozí), světlý, vysoce kontrastní černá a Vysoký kontrast prázdné.</span><span class="sxs-lookup"><span data-stu-id="bab56-281">We have added various themes: Light (default), Dark, High Contrast Black, and High Contrast White.</span></span> <span data-ttu-id="bab56-282">Přejděte tooEdit -&gt; toochange motivy zúčastníte motivů</span><span class="sxs-lookup"><span data-stu-id="bab56-282">Go tooEdit -&gt; Themes toochange your theming preference</span></span>
* <span data-ttu-id="bab56-283">Můžete upravit vlastnosti objektů Blob a souborů</span><span class="sxs-lookup"><span data-stu-id="bab56-283">You can modify Blob and file properties</span></span>
* <span data-ttu-id="bab56-284">Teď podporujeme kódovaného (base64) a nekódovaného fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="bab56-284">We now support encoded (base64) and unencoded queue messages</span></span>
* <span data-ttu-id="bab56-285">V systému Linux, OS 64-bit je nyní vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="bab56-285">On Linux, a 64-bit OS is now required.</span></span> <span data-ttu-id="bab56-286">Pro tuto verzi podporujeme pouze 64bitové verze Ubuntu 16.04.1 LTS</span><span class="sxs-lookup"><span data-stu-id="bab56-286">For this release we only support 64-bit Ubuntu 16.04.1 LTS</span></span>
* <span data-ttu-id="bab56-287">Aktualizovali jsme naši logo!</span><span class="sxs-lookup"><span data-stu-id="bab56-287">We have updated our logo!</span></span>

#### <a name="fixes"></a><span data-ttu-id="bab56-288">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-288">Fixes</span></span>

* <span data-ttu-id="bab56-289">Pevná: Obrazovky zmrazení problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-289">Fixed: Screen freezing problems</span></span>
* <span data-ttu-id="bab56-290">Opravené: Rozšířené zabezpečení</span><span class="sxs-lookup"><span data-stu-id="bab56-290">Fixed: Enhanced security</span></span>
* <span data-ttu-id="bab56-291">Pevná: Zobrazovat může někdy duplicitní připojené účty</span><span class="sxs-lookup"><span data-stu-id="bab56-291">Fixed: Sometimes duplicate attached accounts could appear</span></span>
* <span data-ttu-id="bab56-292">Opravené: Objekt blob se typ obsahu definovaný může generovat výjimky</span><span class="sxs-lookup"><span data-stu-id="bab56-292">Fixed: A blob with an undefined content type could generate an exception</span></span>
* <span data-ttu-id="bab56-293">Opravené: Otevírání hello panely dotaz na prázdná tabulka nebylo možné</span><span class="sxs-lookup"><span data-stu-id="bab56-293">Fixed: Opening hello Query Panel on an empty table was not possible</span></span>
* <span data-ttu-id="bab56-294">Opravené: Se liší chyby ve vyhledávání</span><span class="sxs-lookup"><span data-stu-id="bab56-294">Fixed: Varies bugs in Search</span></span>
* <span data-ttu-id="bab56-295">Opravené: Vyšší hello počet prostředků, které jsou načteny z 50 too100 při kliknutí na "Více zatížení"</span><span class="sxs-lookup"><span data-stu-id="bab56-295">Fixed: Increased hello number of resources loaded from 50 too100 when clicking "Load More"</span></span>
* <span data-ttu-id="bab56-296">Pevná: Při prvním spuštění, pokud je účet přihlášeni, jsme teď vybrat všechna předplatná pro tento účet ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="bab56-296">Fixed: On first run, if an account is signed into, we now select all subscriptions for that account by default</span></span> 

#### <a name="known-issues"></a><span data-ttu-id="bab56-297">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-297">Known Issues</span></span>

* <span data-ttu-id="bab56-298">Tato verze hello Storage Explorer nefunguje na Ubuntu 14.04</span><span class="sxs-lookup"><span data-stu-id="bab56-298">This release of hello Storage Explorer does not run on Ubuntu 14.04</span></span>
* <span data-ttu-id="bab56-299">tooopen několik karet pro stejný prostředek, není nepřetržitě proveďte kliknutím na hello hello stejného zdroje.</span><span class="sxs-lookup"><span data-stu-id="bab56-299">tooopen multiple tabs for hello same resource, do not continuously click on hello same resource.</span></span> <span data-ttu-id="bab56-300">Klikněte na jiný prostředek a potom přejděte zpět a potom klikněte na původní tooopen prostředků hello ho znovu na jiné kartě</span><span class="sxs-lookup"><span data-stu-id="bab56-300">Click on another resource and then go back and then click on hello original resource tooopen it again in another tab</span></span> 
* <span data-ttu-id="bab56-301">Rychlý přístup pracuje pouze s odběru na základě položky.</span><span class="sxs-lookup"><span data-stu-id="bab56-301">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="bab56-302">V této verzi nepodporuje místních prostředků nebo prostředky, které jsou připojené prostřednictvím klíč nebo tokenu SAS</span><span class="sxs-lookup"><span data-stu-id="bab56-302">Local resources or resources attached via key or SAS token are not supported in this release</span></span>
* <span data-ttu-id="bab56-303">Rychlý přístup může trvat několik sekund toonavigate toohello cílový prostředek, v závislosti na tom, kolik prostředků máte</span><span class="sxs-lookup"><span data-stu-id="bab56-303">It may take Quick Access a few seconds toonavigate toohello target resource, depending on how many resources you have</span></span>
* <span data-ttu-id="bab56-304">S více než 3 skupiny objektů BLOB nebo ukládání na hello stejné souborů čas může způsobit chyby</span><span class="sxs-lookup"><span data-stu-id="bab56-304">Having more than 3 groups of blobs or files uploading at hello same time may cause errors</span></span>
* <span data-ttu-id="bab56-305">Obslužné rutiny vyhledávání hledání mezi uzly přibližně 50 000 - poté výkonu může být ovlivněno nebo může způsobit, že neošetřené výjimky</span><span class="sxs-lookup"><span data-stu-id="bab56-305">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted or may cause unhandled exception</span></span>

<span data-ttu-id="bab56-306">10/03/2016</span><span class="sxs-lookup"><span data-stu-id="bab56-306">10/03/2016</span></span>
### <a name="version-085"></a><span data-ttu-id="bab56-307">Verze 0.8.5</span><span class="sxs-lookup"><span data-stu-id="bab56-307">Version 0.8.5</span></span>

#### <a name="new"></a><span data-ttu-id="bab56-308">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-308">New</span></span>

* <span data-ttu-id="bab56-309">Můžete teď pomocí portálu vygeneruje SAS klíče tooattach tooStorage účtů a prostředků</span><span class="sxs-lookup"><span data-stu-id="bab56-309">Can now use Portal-generated SAS keys tooattach tooStorage Accounts and resources</span></span>

#### <a name="fixes"></a><span data-ttu-id="bab56-310">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-310">Fixes</span></span>

* <span data-ttu-id="bab56-311">Pevná: časování při hledání někdy způsobila uzly toobecome – rozšíření</span><span class="sxs-lookup"><span data-stu-id="bab56-311">Fixed: race condition during search sometimes caused nodes toobecome non-expandable</span></span>
* <span data-ttu-id="bab56-312">Opravené: "Použít protokol HTTP" nefunguje, pokud připojení tooStorage účtů s názvem účtu a klíč</span><span class="sxs-lookup"><span data-stu-id="bab56-312">Fixed: "Use HTTP" doesn't work when connecting tooStorage Accounts with account name and key</span></span>
* <span data-ttu-id="bab56-313">Opravené: Klíče SAS (ty speciálně generované portálu) vrátí chybu "koncové lomítko."</span><span class="sxs-lookup"><span data-stu-id="bab56-313">Fixed: SAS keys (specially Portal-generated ones) return a "trailing slash" error</span></span>
* <span data-ttu-id="bab56-314">Opravené: import tabulky problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-314">Fixed: table import issues</span></span>
    * <span data-ttu-id="bab56-315">Někdy měla vrátit klíč oddílu a klíč řádku</span><span class="sxs-lookup"><span data-stu-id="bab56-315">Sometimes partition key and row key were reversed</span></span>
    * <span data-ttu-id="bab56-316">Nelze tooread "null" klíče oddílů</span><span class="sxs-lookup"><span data-stu-id="bab56-316">Unable tooread "null" Partition Keys</span></span>

#### <a name="known-issues"></a><span data-ttu-id="bab56-317">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-317">Known Issues</span></span>

* <span data-ttu-id="bab56-318">Obslužné rutiny vyhledávání hledání mezi uzly přibližně 50 000 - poté může mít dopad na výkon</span><span class="sxs-lookup"><span data-stu-id="bab56-318">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted</span></span>
* <span data-ttu-id="bab56-319">Azure zásobník nepodporuje aktuálně soubory, tak při tooexpand soubory se zobrazí chyba</span><span class="sxs-lookup"><span data-stu-id="bab56-319">Azure Stack doesn't currently support Files, so trying tooexpand Files will show an error</span></span>

<span data-ttu-id="bab56-320">09/12/2016</span><span class="sxs-lookup"><span data-stu-id="bab56-320">09/12/2016</span></span>
### <a name="version-084"></a><span data-ttu-id="bab56-321">Verze 0.8.4</span><span class="sxs-lookup"><span data-stu-id="bab56-321">Version 0.8.4</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="bab56-322">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-322">New</span></span>

* <span data-ttu-id="bab56-323">Generovat přímé odkazy toostorage účty, kontejnery, front, tabulky, nebo sdílené složky pro sdílení a snadný přístup k prostředkům tooyour - Windows a podporu systému Mac OS</span><span class="sxs-lookup"><span data-stu-id="bab56-323">Generate direct links toostorage accounts, containers, queues, tables, or file shares for sharing and easy access tooyour resources - Windows and Mac OS support</span></span>
* <span data-ttu-id="bab56-324">Hledat kontejnery objektů blob, tabulek, front, sdílené soubory nebo účty úložiště ze hello vyhledávacího pole</span><span class="sxs-lookup"><span data-stu-id="bab56-324">Search for your blob containers, tables, queues, file shares, or storage accounts from hello search box</span></span>
* <span data-ttu-id="bab56-325">Nyní můžete seskupit klauzule v Tvůrce dotazů tabulky hello</span><span class="sxs-lookup"><span data-stu-id="bab56-325">You can now group clauses in hello table query builder</span></span>
* <span data-ttu-id="bab56-326">Přejmenujte a zkopírujte a vložte kontejnery objektů blob, sdílené složky, tabulky, objekty BLOB, objektů blob složek, souborů a adresářů z v rámci SAS připojené účty a kontejnery</span><span class="sxs-lookup"><span data-stu-id="bab56-326">Rename and copy/paste blob containers, file shares, tables, blobs, blob folders, files and directories from within SAS-attached accounts and containers</span></span>
* <span data-ttu-id="bab56-327">Přejmenování a kopírování objektu blob kontejnery a sdílené složky teď zachovat vlastnosti a metadata</span><span class="sxs-lookup"><span data-stu-id="bab56-327">Renaming and copying blob containers and file shares now preserve properties and metadata</span></span>

#### <a name="fixes"></a><span data-ttu-id="bab56-328">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-328">Fixes</span></span>

* <span data-ttu-id="bab56-329">Opravené: nelze upravit entity tabulky, pokud obsahují logická hodnota nebo binární vlastnosti</span><span class="sxs-lookup"><span data-stu-id="bab56-329">Fixed: cannot edit table entities if they contain boolean or binary properties</span></span>

#### <a name="known-issues"></a><span data-ttu-id="bab56-330">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-330">Known Issues</span></span>

* <span data-ttu-id="bab56-331">Obslužné rutiny vyhledávání hledání mezi uzly přibližně 50 000 - poté může mít dopad na výkon</span><span class="sxs-lookup"><span data-stu-id="bab56-331">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted</span></span>

<span data-ttu-id="bab56-332">08/03/2016</span><span class="sxs-lookup"><span data-stu-id="bab56-332">08/03/2016</span></span>
### <a name="version-083"></a><span data-ttu-id="bab56-333">Verze 0.8.3</span><span class="sxs-lookup"><span data-stu-id="bab56-333">Version 0.8.3</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="bab56-334">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-334">New</span></span>

* <span data-ttu-id="bab56-335">Přejmenujte kontejnery, tabulky, sdílené složky</span><span class="sxs-lookup"><span data-stu-id="bab56-335">Rename containers, tables, file shares</span></span>
* <span data-ttu-id="bab56-336">Vylepšené prostředí Tvůrce dotazů</span><span class="sxs-lookup"><span data-stu-id="bab56-336">Improved Query builder experience</span></span>
* <span data-ttu-id="bab56-337">Možnost toosave a zatížení dotazy</span><span class="sxs-lookup"><span data-stu-id="bab56-337">Ability toosave and load queries</span></span>
* <span data-ttu-id="bab56-338">Přímé odkazy toostorage účty nebo kontejnery, fronty, tabulky nebo sdílené složky pro sdílení a snadno přístup k prostředkům (pouze pro systém Windows - systému macOS podporu brzo!)</span><span class="sxs-lookup"><span data-stu-id="bab56-338">Direct links toostorage accounts or containers, queues, tables, or file shares for sharing and easily accessing your resources (Windows-only - macOS support coming soon!)</span></span>
* <span data-ttu-id="bab56-339">Možnost toomanage a nakonfigurovat pravidla CORS</span><span class="sxs-lookup"><span data-stu-id="bab56-339">Ability toomanage and configure CORS rules</span></span>

#### <a name="fixes"></a><span data-ttu-id="bab56-340">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-340">Fixes</span></span>

* <span data-ttu-id="bab56-341">Opravené: Accounts Microsoft vyžadovat opakované ověření každých 8 – 12 hodin</span><span class="sxs-lookup"><span data-stu-id="bab56-341">Fixed: Microsoft Accounts require re-authentication every 8-12 hours</span></span>

#### <a name="known-issues"></a><span data-ttu-id="bab56-342">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-342">Known Issues</span></span>

* <span data-ttu-id="bab56-343">Někdy hello uživatelského rozhraní se může objevit ukotvené – tím se maximalizuje okno hello pomůže tento problém vyřešit</span><span class="sxs-lookup"><span data-stu-id="bab56-343">Sometimes hello UI might appear frozen - maximizing hello window helps resolve this issue</span></span>
* <span data-ttu-id="bab56-344">instalace systému macOS může vyžadovat zvýšenými oprávněními</span><span class="sxs-lookup"><span data-stu-id="bab56-344">macOS install may require elevated permissions</span></span>
* <span data-ttu-id="bab56-345">Panel nastavení účtu může zobrazovat, je nutné, přihlašovací údaje tooreenter v pořadí toofilter odběrů</span><span class="sxs-lookup"><span data-stu-id="bab56-345">Account settings panel may show that you need tooreenter credentials in order toofilter subscriptions</span></span>
* <span data-ttu-id="bab56-346">Přejmenování sdílené složky, kontejnery objektů blob a tabulek nezachovává metadata nebo dalších vlastností hello kontejneru, například kvóta pro sdílenou složku, úroveň veřejného přístupu nebo zásady přístupu</span><span class="sxs-lookup"><span data-stu-id="bab56-346">Renaming file shares, blob containers, and tables does not preserve metadata or other properties on hello container, such as file share quota, public access level or access policies</span></span>
* <span data-ttu-id="bab56-347">Přejmenování objekty BLOB (samostatně nebo v kontejneru objektů blob přejmenovat) nezachovává snímky.</span><span class="sxs-lookup"><span data-stu-id="bab56-347">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="bab56-348">Během přejmenovat se zachovají všechny ostatní vlastnosti a metadat pro objekty BLOB, soubory a entit</span><span class="sxs-lookup"><span data-stu-id="bab56-348">All other properties and metadata for blobs, files and entities are preserved during a rename</span></span>
* <span data-ttu-id="bab56-349">Kopírování nebo přejmenování prostředky nefunguje v rámci SAS připojené účty</span><span class="sxs-lookup"><span data-stu-id="bab56-349">Copying or renaming resources does not work within SAS-attached accounts</span></span>

<span data-ttu-id="bab56-350">07/07/2016</span><span class="sxs-lookup"><span data-stu-id="bab56-350">07/07/2016</span></span>
### <a name="version-082"></a><span data-ttu-id="bab56-351">Verze 0.8.2</span><span class="sxs-lookup"><span data-stu-id="bab56-351">Version 0.8.2</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="bab56-352">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-352">New</span></span>

* <span data-ttu-id="bab56-353">Účty úložiště jsou seskupené podle odběry; vývoj pro úložiště a prostředky, které jsou připojené prostřednictvím klíč nebo SAS se zobrazují v uzlu (místní a připojené)</span><span class="sxs-lookup"><span data-stu-id="bab56-353">Storage Accounts are grouped by subscriptions; development storage and resources attached via key or SAS are shown under (Local and Attached) node</span></span>
* <span data-ttu-id="bab56-354">Odhlaste se z účtů v panelech "Nastavení účtu Azure"</span><span class="sxs-lookup"><span data-stu-id="bab56-354">Sign off from accounts in "Azure Account Settings" panel</span></span>
* <span data-ttu-id="bab56-355">Konfigurace tooenable proxy nastavení a správa přihlášení</span><span class="sxs-lookup"><span data-stu-id="bab56-355">Configure proxy settings tooenable and manage sign-in</span></span>
* <span data-ttu-id="bab56-356">Vytvoření a rozdělit zapůjčení objektů blob</span><span class="sxs-lookup"><span data-stu-id="bab56-356">Create and break blob leases</span></span>
* <span data-ttu-id="bab56-357">Kontejnery otevřete objektů blob, fronty, tabulky a soubory s jedním kliknutím</span><span class="sxs-lookup"><span data-stu-id="bab56-357">Open blob containers, queues, tables, and files with single-click</span></span>

#### <a name="fixes"></a><span data-ttu-id="bab56-358">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-358">Fixes</span></span>

* <span data-ttu-id="bab56-359">Pevná: Vložit s knihovnami .NET nebo Java zprávy fronty nejsou dekódovat správně z formátu base64.</span><span class="sxs-lookup"><span data-stu-id="bab56-359">Fixed: queue messages inserted with .NET or Java libraries are not properly decoded from base64</span></span>
* <span data-ttu-id="bab56-360">Opravené: $metrics tabulky nejsou zobrazeny pro účty úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="bab56-360">Fixed: $metrics tables are not shown for Blob Storage accounts</span></span>
* <span data-ttu-id="bab56-361">Opravené: uzel tabulky nefunguje pro místní úložiště (vývoj)</span><span class="sxs-lookup"><span data-stu-id="bab56-361">Fixed: tables node does not work for local (Development) storage</span></span>

#### <a name="known-issues"></a><span data-ttu-id="bab56-362">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-362">Known Issues</span></span>

* <span data-ttu-id="bab56-363">instalace systému macOS může vyžadovat zvýšenými oprávněními</span><span class="sxs-lookup"><span data-stu-id="bab56-363">macOS install may require elevated permissions</span></span>

<span data-ttu-id="bab56-364">06/15/2016</span><span class="sxs-lookup"><span data-stu-id="bab56-364">06/15/2016</span></span>
### <a name="version-080"></a><span data-ttu-id="bab56-365">Verze 0.8.0</span><span class="sxs-lookup"><span data-stu-id="bab56-365">Version 0.8.0</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ycfQhKztSIY?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="bab56-366">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-366">New</span></span>

* <span data-ttu-id="bab56-367">Podpora sdílení souborů: zobrazení, odesílání, stahování, kopírování souborů a adresářů, identifikátory URI SAS (vytvořit a připojit)</span><span class="sxs-lookup"><span data-stu-id="bab56-367">File share support: viewing, uploading, downloading, copying files and directories, SAS URIs (create and connect)</span></span>
* <span data-ttu-id="bab56-368">Vylepšené uživatelské prostředí pro připojení tooStorage s identifikátory URI SAS nebo klíče účtu</span><span class="sxs-lookup"><span data-stu-id="bab56-368">Improved user experience for connecting tooStorage with SAS URIs or account keys</span></span>
* <span data-ttu-id="bab56-369">Export výsledků dotazu tabulky</span><span class="sxs-lookup"><span data-stu-id="bab56-369">Export table query results</span></span>
* <span data-ttu-id="bab56-370">Změny pořadí sloupců tabulky a přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="bab56-370">Table column reordering and customization</span></span>
* <span data-ttu-id="bab56-371">Zobrazení $logs kontejnery objektů blob a tabulek $metrics pro účty úložiště pomocí povoleno metriky</span><span class="sxs-lookup"><span data-stu-id="bab56-371">Viewing $logs blob containers and $metrics tables for Storage Accounts with enabled metrics</span></span>
* <span data-ttu-id="bab56-372">Vylepšené export a import chování, teď obsahuje typ hodnoty vlastnosti</span><span class="sxs-lookup"><span data-stu-id="bab56-372">Improved export and import behavior, now includes property value type</span></span>

#### <a name="fixes"></a><span data-ttu-id="bab56-373">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-373">Fixes</span></span>

* <span data-ttu-id="bab56-374">Opravené: nahrávání nebo stahování velké objekty BLOB může způsobit neúplné nahrávání nebo stahování</span><span class="sxs-lookup"><span data-stu-id="bab56-374">Fixed: uploading or downloading large blobs can result in incomplete uploads/downloads</span></span>
* <span data-ttu-id="bab56-375">Opravené: úpravy, přidáním nebo importováním entitu s hodnotou číselných řetězců ("1") bude převedena toodouble</span><span class="sxs-lookup"><span data-stu-id="bab56-375">Fixed: editing, adding, or importing an entity with a numeric string value ("1") will convert it toodouble</span></span>
* <span data-ttu-id="bab56-376">Opravené: Uzel nelze tooexpand hello tabulky v hello místní vývojové prostředí</span><span class="sxs-lookup"><span data-stu-id="bab56-376">Fixed: Unable tooexpand hello table node in hello local development environment</span></span>

#### <a name="known-issues"></a><span data-ttu-id="bab56-377">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-377">Known Issues</span></span>

* <span data-ttu-id="bab56-378">$metrics tabulky nejsou viditelné pro účty úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="bab56-378">$metrics tables are not visible for Blob Storage accounts</span></span>
* <span data-ttu-id="bab56-379">Zprávy fronty přidány prostřednictvím kódu programu, se nemusí zobrazit správně Pokud hello zprávy jsou zakódován pomocí kódování Base64</span><span class="sxs-lookup"><span data-stu-id="bab56-379">Queue messages added programmatically may not be displayed correctly if hello messages are encoded using Base64 encoding</span></span>

<span data-ttu-id="bab56-380">05/17/2016</span><span class="sxs-lookup"><span data-stu-id="bab56-380">05/17/2016</span></span>
### <a name="version-07201605090"></a><span data-ttu-id="bab56-381">Verze 0.7.20160509.0</span><span class="sxs-lookup"><span data-stu-id="bab56-381">Version 0.7.20160509.0</span></span>

#### <a name="new"></a><span data-ttu-id="bab56-382">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-382">New</span></span>

* <span data-ttu-id="bab56-383">Lepší zpracování chyb pro aplikace, dojde k chybě</span><span class="sxs-lookup"><span data-stu-id="bab56-383">Better error handling for app crashes</span></span>

#### <a name="fixes"></a><span data-ttu-id="bab56-384">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-384">Fixes</span></span>

* <span data-ttu-id="bab56-385">Kde informační panel zpráv někdy nezobrazovat až při přihlašovací údaje nebyly potřeba opravené chyby</span><span class="sxs-lookup"><span data-stu-id="bab56-385">Fixed bug where InfoBar messages sometimes don't show up when sign-in credentials were required</span></span>

#### <a name="known-issues"></a><span data-ttu-id="bab56-386">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-386">Known Issues</span></span>

* <span data-ttu-id="bab56-387">Tabulky: Přidávání, úpravám nebo import entita, která má vlastnost s ambiguously číselnou hodnotu, jako je například "1" nebo "1.0", a hello toosend pokusů uživatele jej jako `Edm.String`, hello hodnotu, vrátí se znovu prostřednictvím klienta hello rozhraní API jako Edm.Double</span><span class="sxs-lookup"><span data-stu-id="bab56-387">Tables: Adding, editing, or importing an entity that has a property with an ambiguously numeric value, such as "1" or "1.0", and hello user tries toosend it as an `Edm.String`, hello value will come back through hello client API as an Edm.Double</span></span>

<span data-ttu-id="bab56-388">03/31/2016</span><span class="sxs-lookup"><span data-stu-id="bab56-388">03/31/2016</span></span>

### <a name="version-07201603250"></a><span data-ttu-id="bab56-389">Verze 0.7.20160325.0</span><span class="sxs-lookup"><span data-stu-id="bab56-389">Version 0.7.20160325.0</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/imbgBRHX65A?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a><span data-ttu-id="bab56-390">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-390">New</span></span>

* <span data-ttu-id="bab56-391">Tabulka podporu: zobrazení, dotazování, export, jejich import a operace CRUD pro entity</span><span class="sxs-lookup"><span data-stu-id="bab56-391">Table support: viewing, querying, export, import, and CRUD operations for entities</span></span>
* <span data-ttu-id="bab56-392">Podpora ve frontě: zobrazení, přidání, dequeueing zprávy</span><span class="sxs-lookup"><span data-stu-id="bab56-392">Queue support: viewing, adding, dequeueing messages</span></span>
* <span data-ttu-id="bab56-393">Generování identifikátory URI SAS pro účty úložiště</span><span class="sxs-lookup"><span data-stu-id="bab56-393">Generating SAS URIs for Storage Accounts</span></span>
* <span data-ttu-id="bab56-394">Připojení tooStorage účtů s identifikátory URI SAS</span><span class="sxs-lookup"><span data-stu-id="bab56-394">Connecting tooStorage Accounts with SAS URIs</span></span>
* <span data-ttu-id="bab56-395">Oznámení o aktualizacích pro budoucí aktualizace tooStorage Explorer</span><span class="sxs-lookup"><span data-stu-id="bab56-395">Update notifications for future updates tooStorage Explorer</span></span>
* <span data-ttu-id="bab56-396">Aktualizované vzhled a chování</span><span class="sxs-lookup"><span data-stu-id="bab56-396">Updated look and feel</span></span>

#### <a name="fixes"></a><span data-ttu-id="bab56-397">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-397">Fixes</span></span>

* <span data-ttu-id="bab56-398">Vylepšení výkonu a spolehlivosti</span><span class="sxs-lookup"><span data-stu-id="bab56-398">Performance and reliability improvements</span></span>

### <a name="known-issues-amp-mitigations"></a><span data-ttu-id="bab56-399">Známé problémy &amp; způsoby zmírnění rizik</span><span class="sxs-lookup"><span data-stu-id="bab56-399">Known Issues &amp; Mitigations</span></span>

* <span data-ttu-id="bab56-400">Stahování souborů velkého objektu blob nebude správně fungovat – doporučujeme použít AzCopy, když jsme tento problém vyřešit</span><span class="sxs-lookup"><span data-stu-id="bab56-400">Download of large blob files does not work correctly - we recommend using AzCopy while we address this issue</span></span> 
* <span data-ttu-id="bab56-401">Přihlašovací údaje účtu nebude načíst ani v mezipaměti, pokud hello domovskou složku nelze nalézt nebo nelze zapisovat</span><span class="sxs-lookup"><span data-stu-id="bab56-401">Account credentials will not be retrieved nor cached if hello home folder cannot be found or cannot be written to</span></span>
* <span data-ttu-id="bab56-402">Pokud jsme jsou přidání, úpravy nebo import entita, která má vlastnost s ambiguously číselnou hodnotu, jako je například "1" nebo "1.0", a uživatel hello pokusí toosend jej jako `Edm.String`, hello hodnotu, vrátí se znovu prostřednictvím klienta hello rozhraní API jako Edm.Double</span><span class="sxs-lookup"><span data-stu-id="bab56-402">If we are adding, editing, or importing an entity that has a property with an ambiguously numeric value, such as "1" or "1.0", and hello user tries toosend it as an `Edm.String`, hello value will come back through hello client API as an Edm.Double</span></span>
* <span data-ttu-id="bab56-403">Při importu souborů CSV pomocí víceřádkovým záznamy, může získat hello data chopped nebo kódována</span><span class="sxs-lookup"><span data-stu-id="bab56-403">When importing CSV files with multiline records, hello data may get chopped or scrambled</span></span>

<span data-ttu-id="bab56-404">02/03/2016</span><span class="sxs-lookup"><span data-stu-id="bab56-404">02/03/2016</span></span>

### <a name="version-07201601291"></a><span data-ttu-id="bab56-405">Verze 0.7.20160129.1</span><span class="sxs-lookup"><span data-stu-id="bab56-405">Version 0.7.20160129.1</span></span>

#### <a name="fixes"></a><span data-ttu-id="bab56-406">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-406">Fixes</span></span>

* <span data-ttu-id="bab56-407">Lepší celkový výkon při odesílání, stahování a kopírování objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="bab56-407">Improved overall performance when uploading, downloading and copying blobs</span></span>

<span data-ttu-id="bab56-408">01/14/2016</span><span class="sxs-lookup"><span data-stu-id="bab56-408">01/14/2016</span></span>

### <a name="version-07201601050"></a><span data-ttu-id="bab56-409">Verze 0.7.20160105.0</span><span class="sxs-lookup"><span data-stu-id="bab56-409">Version 0.7.20160105.0</span></span>

#### <a name="new"></a><span data-ttu-id="bab56-410">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-410">New</span></span>

* <span data-ttu-id="bab56-411">Podpora Linuxu (parita funkce tooOSX)</span><span class="sxs-lookup"><span data-stu-id="bab56-411">Linux support (parity features tooOSX)</span></span>
* <span data-ttu-id="bab56-412">Přidat kontejnery objektů blob s podpisy sdíleného přístupu (SAS) klíčem.</span><span class="sxs-lookup"><span data-stu-id="bab56-412">Add blob containers with Shared Access Signatures (SAS) key</span></span>
* <span data-ttu-id="bab56-413">Přidat účty úložiště pro Azure Čína</span><span class="sxs-lookup"><span data-stu-id="bab56-413">Add Storage Accounts for Azure China</span></span>
* <span data-ttu-id="bab56-414">Přidat účty úložiště s vlastní koncové body</span><span class="sxs-lookup"><span data-stu-id="bab56-414">Add Storage Accounts with custom endpoints</span></span>
* <span data-ttu-id="bab56-415">Otevírat a zobrazovat hello obsah textu a obrázků objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="bab56-415">Open and view hello contents text and picture blobs</span></span>
* <span data-ttu-id="bab56-416">Zobrazit a upravit vlastnosti objektů blob a metadat</span><span class="sxs-lookup"><span data-stu-id="bab56-416">View and edit blob properties and metadata</span></span>

#### <a name="fixes"></a><span data-ttu-id="bab56-417">Opravy</span><span class="sxs-lookup"><span data-stu-id="bab56-417">Fixes</span></span>

* <span data-ttu-id="bab56-418">Pevné: odeslání nebo stažení velkého počtu objektů BLOB (500 +) může v některých případech způsobit toohave aplikace hello bílé obrazovky</span><span class="sxs-lookup"><span data-stu-id="bab56-418">Fixed: uploading or download a large number of blobs (500+) may sometimes cause hello app toohave a white screen</span></span> 
* <span data-ttu-id="bab56-419">Opravené: při nastavení úrovně veřejný přístup kontejner objektů blob, nová hodnota hello se neaktualizuje dokud znovu nenastavíte hello zaměřením na kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="bab56-419">Fixed: when setting blob container public access level, hello new value is not updated until you re-set hello focus on hello container.</span></span> <span data-ttu-id="bab56-420">Navíc hello dialogu vždy použije jako výchozí příliš "žádné veřejný přístup" a ne hello skutečná aktuální hodnota.</span><span class="sxs-lookup"><span data-stu-id="bab56-420">Also, hello dialog always defaults too"No public access", and not hello actual current value.</span></span>
* <span data-ttu-id="bab56-421">Podpora lepší celkový klávesnice nebo usnadnění a uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="bab56-421">Better overall keyboard/accessibility and UI support</span></span>
* <span data-ttu-id="bab56-422">Cesta ke stránce Historie text zalomen během procesu je dlouhá s bílými oblasti</span><span class="sxs-lookup"><span data-stu-id="bab56-422">Breadcrumbs history text wraps when it's long with white space</span></span>
* <span data-ttu-id="bab56-423">Dialogové okno SAS podporuje ověřování vstupu</span><span class="sxs-lookup"><span data-stu-id="bab56-423">SAS dialog supports input validation</span></span>
* <span data-ttu-id="bab56-424">Místní úložiště pokračovat toobe k dispozici i v případě, že vypršela platnost přihlašovací údaje uživatele</span><span class="sxs-lookup"><span data-stu-id="bab56-424">Local storage continues toobe available even if user credentials have expired</span></span>
* <span data-ttu-id="bab56-425">Při odstranění kontejner otevřen objekt blob je uzavřený hello Průzkumník objektů blob na pravé straně hello</span><span class="sxs-lookup"><span data-stu-id="bab56-425">When an opened blob container is deleted, hello blob explorer on hello right side is closed</span></span>

#### <a name="known-issues"></a><span data-ttu-id="bab56-426">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-426">Known Issues</span></span>

* <span data-ttu-id="bab56-427">Linux instalace potřeby RSZ verze aktualizovat nebo upgradovat – tooupgrade kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="bab56-427">Linux install needs gcc version updated or upgraded – steps tooupgrade are below:</span></span> 
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

<span data-ttu-id="bab56-428">11/18/2015</span><span class="sxs-lookup"><span data-stu-id="bab56-428">11/18/2015</span></span>
### <a name="version-07201511160"></a><span data-ttu-id="bab56-429">Verze 0.7.20151116.0</span><span class="sxs-lookup"><span data-stu-id="bab56-429">Version 0.7.20151116.0</span></span>

#### <a name="new"></a><span data-ttu-id="bab56-430">Nový</span><span class="sxs-lookup"><span data-stu-id="bab56-430">New</span></span>

* <span data-ttu-id="bab56-431">systému macOS a verze systému Windows</span><span class="sxs-lookup"><span data-stu-id="bab56-431">macOS, and Windows versions</span></span>
* <span data-ttu-id="bab56-432">Přihlaste se tooview účtů úložiště – pomocí účtu organizace, Microsoft Account, 2FA atd.</span><span class="sxs-lookup"><span data-stu-id="bab56-432">Sign in tooview your Storage Accounts – use your Org Account, Microsoft Account, 2FA, etc.</span></span>
* <span data-ttu-id="bab56-433">Vývoj pro místní úložiště (pomocí emulátoru úložiště pouze pro systém Windows)</span><span class="sxs-lookup"><span data-stu-id="bab56-433">Local development storage (use storage emulator, Windows-only)</span></span>
* <span data-ttu-id="bab56-434">Azure Resource Manager a klasický podporu prostředků</span><span class="sxs-lookup"><span data-stu-id="bab56-434">Azure Resource Manager and Classic resource support</span></span>
* <span data-ttu-id="bab56-435">Vytvářet a odstraňovat objekty BLOB, fronty nebo tabulky</span><span class="sxs-lookup"><span data-stu-id="bab56-435">Create and delete blobs, queues, or tables</span></span>
* <span data-ttu-id="bab56-436">Vyhledejte konkrétní objekty BLOB, fronty nebo tabulky</span><span class="sxs-lookup"><span data-stu-id="bab56-436">Search for specific blobs, queues, or tables</span></span>
* <span data-ttu-id="bab56-437">Seznamte se s obsahem hello kontejnerů objektů blob</span><span class="sxs-lookup"><span data-stu-id="bab56-437">Explore hello contents of blob containers</span></span>
* <span data-ttu-id="bab56-438">Zobrazení a procházení adresářů</span><span class="sxs-lookup"><span data-stu-id="bab56-438">View and navigate through directories</span></span>
* <span data-ttu-id="bab56-439">Odesílání, stahování a odstraňování objektů BLOB a složky</span><span class="sxs-lookup"><span data-stu-id="bab56-439">Upload, download, and delete blobs and folders</span></span>
* <span data-ttu-id="bab56-440">Zobrazit a upravit vlastnosti objektů blob a metadat</span><span class="sxs-lookup"><span data-stu-id="bab56-440">View and edit blob properties and metadata</span></span>
* <span data-ttu-id="bab56-441">Generovat klíče SAS</span><span class="sxs-lookup"><span data-stu-id="bab56-441">Generate SAS keys</span></span>
* <span data-ttu-id="bab56-442">Spravovat a vytvořit uložené zásady přístupu (SAP)</span><span class="sxs-lookup"><span data-stu-id="bab56-442">Manage and create Stored Access Policies (SAP)</span></span>
* <span data-ttu-id="bab56-443">Hledat objekty BLOB podle předpony</span><span class="sxs-lookup"><span data-stu-id="bab56-443">Search for blobs by prefix</span></span>
* <span data-ttu-id="bab56-444">Přetáhněte severní šířky vyřaďte tooupload soubory nebo stáhnout</span><span class="sxs-lookup"><span data-stu-id="bab56-444">Drag 'n drop files tooupload or download</span></span>

#### <a name="known-issues"></a><span data-ttu-id="bab56-445">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="bab56-445">Known Issues</span></span>

* <span data-ttu-id="bab56-446">Při nastavení úrovně veřejný přístup kontejner objektů blob, nová hodnota hello není aktualizován, dokud znovu nenastavíte hello zaměřením na kontejneru hello</span><span class="sxs-lookup"><span data-stu-id="bab56-446">When setting blob container public access level, hello new value is not updated until you re-set hello focus on hello container</span></span>
* <span data-ttu-id="bab56-447">Když otevřete hello dialogové okno tooset hello veřejný přístup úroveň, vždy zobrazuje "Žádné veřejný přístup" jako výchozí hello a není hello skutečné aktuální hodnota</span><span class="sxs-lookup"><span data-stu-id="bab56-447">When you open hello dialog tooset hello public access level, it always shows "No public access" as hello default, and not hello actual current value</span></span>
* <span data-ttu-id="bab56-448">Nelze přejmenovat stažené objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="bab56-448">Cannot rename downloaded blobs</span></span>
* <span data-ttu-id="bab56-449">Položky protokolu aktivity se někdy "zablokuje" v v průběhu stavu, když dojde k chybě, a není zobrazí chyba hello</span><span class="sxs-lookup"><span data-stu-id="bab56-449">Activity log entries will sometimes get "stuck" in an in progress state when an error occurs, and hello error is not displayed</span></span>
* <span data-ttu-id="bab56-450">V některých případech havárie nebo oplátku zcela bílé při pokusu o tooupload nebo stáhnout velkého počtu objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="bab56-450">Sometimes crashes or turns completely white when trying tooupload or download a large number of blobs</span></span>
* <span data-ttu-id="bab56-451">Někdy zrušení operace kopírování nefunguje</span><span class="sxs-lookup"><span data-stu-id="bab56-451">Sometimes canceling a copy operation does not work</span></span>
* <span data-ttu-id="bab56-452">Při vytvoření kontejneru (objektů blob nebo fronty nebo tabulky), pokud vstupní neplatný název a pokračovat toocreate jiné v rámci různých kontejneru typu nelze nastavit fokus na nový typ hello</span><span class="sxs-lookup"><span data-stu-id="bab56-452">During creating a container (blob/queue/table), if you input an invalid name and proceed toocreate another under a different container type you cannot set focus on hello new type</span></span>
* <span data-ttu-id="bab56-453">Nelze vytvořit novou složku nebo přejmenujte složku</span><span class="sxs-lookup"><span data-stu-id="bab56-453">Can't create new folder or rename folder</span></span>




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md