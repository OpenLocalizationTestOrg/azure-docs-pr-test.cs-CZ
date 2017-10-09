---
title: aaaCopy dat z Azure BLOB Storage do Data Lake Store | Microsoft Docs
description: "Použít AdlCopy nástroj toocopy dat z Azure úložiště objektů BLOB tooData Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: dc273ef8-96ef-47a6-b831-98e8a777a5c1
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: a3d4172eaefe7395cdef2fff72691bd70f642b78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-from-azure-storage-blobs-toodata-lake-store"></a><span data-ttu-id="ff57d-103">Kopírování dat z Azure úložiště objektů BLOB tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="ff57d-103">Copy data from Azure Storage Blobs tooData Lake Store</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ff57d-104">Pomocí DistCp</span><span class="sxs-lookup"><span data-stu-id="ff57d-104">Using DistCp</span></span>](data-lake-store-copy-data-wasb-distcp.md)
> * [<span data-ttu-id="ff57d-105">Pomocí AdlCopy</span><span class="sxs-lookup"><span data-stu-id="ff57d-105">Using AdlCopy</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
>
>

<span data-ttu-id="ff57d-106">Azure Data Lake Store poskytuje nástroj příkazového řádku, [AdlCopy](http://aka.ms/downloadadlcopy), toocopy data z hello následující zdroje:</span><span class="sxs-lookup"><span data-stu-id="ff57d-106">Azure Data Lake Store provides a command line tool, [AdlCopy](http://aka.ms/downloadadlcopy), toocopy data from hello following sources:</span></span>

* <span data-ttu-id="ff57d-107">Z Azure úložiště objektů BLOB do Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ff57d-107">From Azure Storage Blobs into Data Lake Store.</span></span> <span data-ttu-id="ff57d-108">Nelze použít AdlCopy toocopy dat z Data Lake Store tooAzure úložiště objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="ff57d-108">You cannot use AdlCopy toocopy data from Data Lake Store tooAzure Storage blobs.</span></span>
* <span data-ttu-id="ff57d-109">Mezi dvěma účty Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ff57d-109">Between two Azure Data Lake Store accounts.</span></span>

<span data-ttu-id="ff57d-110">Navíc můžete použít nástroj AdlCopy hello ve dvou různých režimech:</span><span class="sxs-lookup"><span data-stu-id="ff57d-110">Also, you can use hello AdlCopy tool in two different modes:</span></span>

* <span data-ttu-id="ff57d-111">**Samostatné**, kde nástroj hello používá Data Lake Store prostředky tooperform hello úloh.</span><span class="sxs-lookup"><span data-stu-id="ff57d-111">**Standalone**, where hello tool uses Data Lake Store resources tooperform hello task.</span></span>
* <span data-ttu-id="ff57d-112">**Pomocí účtu Data Lake Analytics**, kde hello jednotky přiřazen účet Data Lake Analytics tooyour jsou použité tooperform hello kopírování.</span><span class="sxs-lookup"><span data-stu-id="ff57d-112">**Using a Data Lake Analytics account**, where hello units assigned tooyour Data Lake Analytics account are used tooperform hello copy operation.</span></span> <span data-ttu-id="ff57d-113">Můžete chtít toouse tuto možnost při prohlížení tooperform hello kopírování úlohy předvídatelný způsobem.</span><span class="sxs-lookup"><span data-stu-id="ff57d-113">You might want toouse this option when you are looking tooperform hello copy tasks in a predictable manner.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff57d-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ff57d-114">Prerequisites</span></span>
<span data-ttu-id="ff57d-115">Před zahájením tohoto článku, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="ff57d-115">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="ff57d-116">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="ff57d-116">**An Azure subscription**.</span></span> <span data-ttu-id="ff57d-117">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ff57d-117">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="ff57d-118">**Objektů BLOB služby Azure Storage** kontejneru určitými daty.</span><span class="sxs-lookup"><span data-stu-id="ff57d-118">**Azure Storage Blobs** container with some data.</span></span>
* <span data-ttu-id="ff57d-119">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="ff57d-119">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="ff57d-120">Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ff57d-120">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="ff57d-121">**Účtu Azure Data Lake Analytics (volitelné)** -najdete v části [Začínáme s Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) pokyny, jak toocreate Data Lake ukládání účtu.</span><span class="sxs-lookup"><span data-stu-id="ff57d-121">**Azure Data Lake Analytics account (optional)** - See [Get started with Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) for instructions on how toocreate a Data Lake Store account.</span></span>
* <span data-ttu-id="ff57d-122">**Nástroj AdlCopy**.</span><span class="sxs-lookup"><span data-stu-id="ff57d-122">**AdlCopy tool**.</span></span> <span data-ttu-id="ff57d-123">Nainstalujte nástroj AdlCopy hello z [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).</span><span class="sxs-lookup"><span data-stu-id="ff57d-123">Install hello AdlCopy tool from [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).</span></span>

## <a name="syntax-of-hello-adlcopy-tool"></a><span data-ttu-id="ff57d-124">Syntaxe nástroje AdlCopy hello</span><span class="sxs-lookup"><span data-stu-id="ff57d-124">Syntax of hello AdlCopy tool</span></span>
<span data-ttu-id="ff57d-125">Pomocí následující syntaxe toowork nástrojem AdlCopy hello hello</span><span class="sxs-lookup"><span data-stu-id="ff57d-125">Use hello following syntax toowork with hello AdlCopy tool</span></span>

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern

<span data-ttu-id="ff57d-126">Parametry Hello v syntaxi hello jsou následující:</span><span class="sxs-lookup"><span data-stu-id="ff57d-126">hello parameters in hello syntax are described below:</span></span>

| <span data-ttu-id="ff57d-127">Možnost</span><span class="sxs-lookup"><span data-stu-id="ff57d-127">Option</span></span> | <span data-ttu-id="ff57d-128">Popis</span><span class="sxs-lookup"><span data-stu-id="ff57d-128">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ff57d-129">Zdroj</span><span class="sxs-lookup"><span data-stu-id="ff57d-129">Source</span></span> |<span data-ttu-id="ff57d-130">Určuje umístění hello hello zdroje dat v objektu blob úložiště Azure hello.</span><span class="sxs-lookup"><span data-stu-id="ff57d-130">Specifies hello location of hello source data in hello Azure storage blob.</span></span> <span data-ttu-id="ff57d-131">Hello zdroj může být kontejner objektů blob, objekt blob nebo jiný účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ff57d-131">hello source can be a blob container, a blob, or another Data Lake Store account.</span></span> |
| <span data-ttu-id="ff57d-132">Cíle</span><span class="sxs-lookup"><span data-stu-id="ff57d-132">Dest</span></span> |<span data-ttu-id="ff57d-133">Určuje toocopy cílové hello Data Lake Store k.</span><span class="sxs-lookup"><span data-stu-id="ff57d-133">Specifies hello Data Lake Store destination toocopy to.</span></span> |
| <span data-ttu-id="ff57d-134">SourceKey</span><span class="sxs-lookup"><span data-stu-id="ff57d-134">SourceKey</span></span> |<span data-ttu-id="ff57d-135">Určuje hello úložiště přístupový klíč pro zdrojový objekt blob úložiště Azure hello.</span><span class="sxs-lookup"><span data-stu-id="ff57d-135">Specifies hello storage access key for hello Azure storage blob source.</span></span> <span data-ttu-id="ff57d-136">To je potřeba, pouze pokud je zdroj hello kontejner objektů blob nebo objekt blob.</span><span class="sxs-lookup"><span data-stu-id="ff57d-136">This is required only if hello source is a blob container or a blob.</span></span> |
| <span data-ttu-id="ff57d-137">Účet</span><span class="sxs-lookup"><span data-stu-id="ff57d-137">Account</span></span> |<span data-ttu-id="ff57d-138">**Volitelné**.</span><span class="sxs-lookup"><span data-stu-id="ff57d-138">**Optional**.</span></span> <span data-ttu-id="ff57d-139">Použijte, pokud chcete úlohu kopírování toouse Azure Data Lake Analytics účet toorun hello.</span><span class="sxs-lookup"><span data-stu-id="ff57d-139">Use this if you want toouse Azure Data Lake Analytics account toorun hello copy job.</span></span> <span data-ttu-id="ff57d-140">Pokud použijete možnost /Account hello v syntaxi hello ale nezadávejte účet Data Lake Analytics, AdlCopy používá výchozí účet toorun hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="ff57d-140">If you use hello /Account option in hello syntax but do not specify a Data Lake Analytics account, AdlCopy uses a default account toorun hello job.</span></span> <span data-ttu-id="ff57d-141">Navíc pokud použijete tuto možnost, musíte přidat zdroj hello (Azure Storage Blob) a cíl (Azure Data Lake Store) jako zdroje dat pro váš účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="ff57d-141">Also, if you use this option, you must add hello source (Azure Storage Blob) and destination (Azure Data Lake Store) as data sources for your Data Lake Analytics account.</span></span> |
| <span data-ttu-id="ff57d-142">Jednotky</span><span class="sxs-lookup"><span data-stu-id="ff57d-142">Units</span></span> |<span data-ttu-id="ff57d-143">Určuje hello počet jednotek Data Lake Analytics, které se použijí pro úlohu kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="ff57d-143">Specifies hello number of Data Lake Analytics units that will be used for hello copy job.</span></span> <span data-ttu-id="ff57d-144">Tato možnost je povinná, pokud používáte hello **/účet** možnost účet Data Lake Analytics toospecify hello.</span><span class="sxs-lookup"><span data-stu-id="ff57d-144">This option is mandatory if you use hello **/Account** option toospecify hello Data Lake Analytics account.</span></span> |
| <span data-ttu-id="ff57d-145">vzor</span><span class="sxs-lookup"><span data-stu-id="ff57d-145">Pattern</span></span> |<span data-ttu-id="ff57d-146">Určuje vzor regulárního výrazu, která určuje, které toocopy objektů BLOB nebo soubory.</span><span class="sxs-lookup"><span data-stu-id="ff57d-146">Specifies a regex pattern that indicates which blobs or files toocopy.</span></span> <span data-ttu-id="ff57d-147">AdlCopy používá odpovídající malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="ff57d-147">AdlCopy uses case-sensitive matching.</span></span> <span data-ttu-id="ff57d-148">výchozím způsobem Hello používá při žádné vzor je zadán, je toocopy všechny položky.</span><span class="sxs-lookup"><span data-stu-id="ff57d-148">hello default pattern used when no pattern is specified is toocopy all items.</span></span> <span data-ttu-id="ff57d-149">Zadání více vzorů souborů není podporováno.</span><span class="sxs-lookup"><span data-stu-id="ff57d-149">Specifying multiple file patterns is not supported.</span></span> |

## <a name="use-adlcopy-as-standalone-toocopy-data-from-an-azure-storage-blob"></a><span data-ttu-id="ff57d-150">Použít AdlCopy (jako samostatná) toocopy data z objektu blob Azure Storage</span><span class="sxs-lookup"><span data-stu-id="ff57d-150">Use AdlCopy (as standalone) toocopy data from an Azure Storage blob</span></span>
1. <span data-ttu-id="ff57d-151">Otevřete příkazový řádek a přejděte toohello directory AdlCopy nainstalovanou, obvykle `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="ff57d-151">Open a command prompt and navigate toohello directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="ff57d-152">Spusťte následující příkaz toocopy hello konkrétní objekt blob z hello zdrojový kontejner tooa Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="ff57d-152">Run hello following command toocopy a specific blob from hello source container tooa Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    <span data-ttu-id="ff57d-153">Například:</span><span class="sxs-lookup"><span data-stu-id="ff57d-153">For example:</span></span>

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    >[AZURE.NOTE] <span data-ttu-id="ff57d-154">Hello syntaxi výše určuje hello souboru toobe zkopírovaný tooa složku v účtu Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="ff57d-154">hello syntax above specifies hello file toobe copied tooa folder in hello Data Lake Store account.</span></span> <span data-ttu-id="ff57d-155">Nástroj AdlCopy vytvoří složku, pokud název hello zadaná složka neexistuje.</span><span class="sxs-lookup"><span data-stu-id="ff57d-155">AdlCopy tool creates a folder if hello specified folder name does not exist.</span></span>

    <span data-ttu-id="ff57d-156">Bude výzvami tooenter hello přihlašovací údaje pro hello předplatné Azure, ve kterém máte účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ff57d-156">You will be prompted tooenter hello credentials for hello Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="ff57d-157">Zobrazí se výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="ff57d-157">You will see an output similar toohello following:</span></span>

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

1. <span data-ttu-id="ff57d-158">Všechny objekty BLOB hello můžete také zkopírovat z jednoho kontejneru typu toohello Data Lake Store účtu pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ff57d-158">You can also copy all hello blobs from one container toohello Data Lake Store account using hello following command:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    <span data-ttu-id="ff57d-159">Například:</span><span class="sxs-lookup"><span data-stu-id="ff57d-159">For example:</span></span>

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

### <a name="performance-considerations"></a><span data-ttu-id="ff57d-160">Otázky výkonu</span><span class="sxs-lookup"><span data-stu-id="ff57d-160">Performance considerations</span></span>

<span data-ttu-id="ff57d-161">Pokud kopírujete z účtu úložiště objektů Blob Azure, můžete být omezeny během kopírování na straně úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="ff57d-161">If you are copying from an Azure Blob Storage account, you may be throttled during copy on hello blob storage side.</span></span> <span data-ttu-id="ff57d-162">To způsobí snížení výkonu hello kopírování úlohy.</span><span class="sxs-lookup"><span data-stu-id="ff57d-162">This will degrade hello performance of your copy job.</span></span> <span data-ttu-id="ff57d-163">toolearn Další informace o omezení hello Azure Blob Storage, najdete v části omezení Azure Storage v [předplatného Azure a omezení služby](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="ff57d-163">toolearn more about hello limits of Azure Blob Storage, see Azure Storage limits at [Azure subscription and service limits](../azure-subscription-service-limits.md).</span></span>

## <a name="use-adlcopy-as-standalone-toocopy-data-from-another-data-lake-store-account"></a><span data-ttu-id="ff57d-164">Použít data toocopy AdlCopy (jako samostatná) z jiného účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ff57d-164">Use AdlCopy (as standalone) toocopy data from another Data Lake Store account</span></span>
<span data-ttu-id="ff57d-165">Můžete také použít AdlCopy toocopy data mezi dvěma účty Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ff57d-165">You can also use AdlCopy toocopy data between two Data Lake Store accounts.</span></span>

1. <span data-ttu-id="ff57d-166">Otevřete příkazový řádek a přejděte toohello directory AdlCopy nainstalovanou, obvykle `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="ff57d-166">Open a command prompt and navigate toohello directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="ff57d-167">Spusťte následující příkaz toocopy hello konkrétní soubor z jednoho tooanother účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ff57d-167">Run hello following command toocopy a specific file from one Data Lake Store account tooanother.</span></span>

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    <span data-ttu-id="ff57d-168">Například:</span><span class="sxs-lookup"><span data-stu-id="ff57d-168">For example:</span></span>

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

   > [!NOTE]
   > <span data-ttu-id="ff57d-169">Hello syntaxi výše určuje hello souboru toobe zkopírovaný tooa složku v účtu Data Lake Store cílové hello.</span><span class="sxs-lookup"><span data-stu-id="ff57d-169">hello syntax above specifies hello file toobe copied tooa folder in hello destination Data Lake Store account.</span></span> <span data-ttu-id="ff57d-170">Nástroj AdlCopy vytvoří složku, pokud název hello zadaná složka neexistuje.</span><span class="sxs-lookup"><span data-stu-id="ff57d-170">AdlCopy tool creates a folder if hello specified folder name does not exist.</span></span>
   >
   >

    <span data-ttu-id="ff57d-171">Bude výzvami tooenter hello přihlašovací údaje pro hello předplatné Azure, ve kterém máte účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ff57d-171">You will be prompted tooenter hello credentials for hello Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="ff57d-172">Zobrazí se výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="ff57d-172">You will see an output similar toohello following:</span></span>

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.
3. <span data-ttu-id="ff57d-173">Hello následující příkaz zkopíruje všechny soubory ze složky ve složce tooa hello zdroje Data Lake Store účtu v cílovém umístění hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ff57d-173">hello following command copies all files from a specific folder in hello source Data Lake Store account tooa folder in hello destination Data Lake Store account.</span></span>

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

### <a name="performance-considerations"></a><span data-ttu-id="ff57d-174">Otázky výkonu</span><span class="sxs-lookup"><span data-stu-id="ff57d-174">Performance considerations</span></span>

<span data-ttu-id="ff57d-175">Při použití AdlCopy jako samostatný nástroj, hello kopírování se spouští na sdílené, spravované prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="ff57d-175">When using AdlCopy as a standalone tool, hello copy is run on shared, Azure managed resources.</span></span> <span data-ttu-id="ff57d-176">Hello výkonu, které může dojít v tomto prostředí závisí na zatížení systému a dostupné prostředky.</span><span class="sxs-lookup"><span data-stu-id="ff57d-176">hello performance you may get in this environment depends on system load and available resources.</span></span> <span data-ttu-id="ff57d-177">Tento režim je nejvhodnější pro malé přenosy na základě ad hoc.</span><span class="sxs-lookup"><span data-stu-id="ff57d-177">This mode is best used for small transfers on an ad hoc basis.</span></span> <span data-ttu-id="ff57d-178">Žádné parametry potřebovat toobe přizpůsobená při použití AdlCopy jako samostatný nástroj.</span><span class="sxs-lookup"><span data-stu-id="ff57d-178">No parameters need toobe tuned when using AdlCopy as a standalone tool.</span></span>

## <a name="use-adlcopy-with-data-lake-analytics-account-toocopy-data"></a><span data-ttu-id="ff57d-179">Použít data toocopy AdlCopy (s účtem Data Lake Analytics)</span><span class="sxs-lookup"><span data-stu-id="ff57d-179">Use AdlCopy (with Data Lake Analytics account) toocopy data</span></span>
<span data-ttu-id="ff57d-180">Můžete také vaše Data Lake Analytics účet toorun hello AdlCopy úlohy toocopy dat z Azure úložiště objektů BLOB tooData Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ff57d-180">You can also use your Data Lake Analytics account toorun hello AdlCopy job toocopy data from Azure storage blobs tooData Lake Store.</span></span> <span data-ttu-id="ff57d-181">Obvykle použijete tuto možnost při toobe hello data přesunout je v dosahu hello gigabajtech a terabajtů a chcete, aby propustnost lepší a předvídatelný výkon.</span><span class="sxs-lookup"><span data-stu-id="ff57d-181">You would typically use this option when hello data toobe moved is in hello range of gigabytes and terabytes, and you want better and predictable performance throughput.</span></span>

<span data-ttu-id="ff57d-182">toouse vašeho účtu Data Lake Analytics pomocí AdlCopy toocopy z objektu Blob Azure Storage, zdroj hello (Azure Storage Blob) je nutné přidat jako zdroj dat pro váš účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="ff57d-182">toouse your Data Lake Analytics account with AdlCopy toocopy from an Azure Storage Blob, hello source (Azure Storage Blob) must be added as a data source for your Data Lake Analytics account.</span></span> <span data-ttu-id="ff57d-183">Pokyny k přidání účtu Data Lake Analytics tooyour další datové zdroje najdete v tématu [zdrojů dat účtu Správa Data Lake Analytics](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).</span><span class="sxs-lookup"><span data-stu-id="ff57d-183">For instructions on adding additional data sources tooyour Data Lake Analytics account, see [Manage Data Lake Analytics account data sources](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).</span></span>

> [!NOTE]
> <span data-ttu-id="ff57d-184">Pokud kopírujete z účtu Azure Data Lake Store jako zdroj hello pomocí účtu Data Lake Analytics, není nutné tooassociate hello účtu Data Lake Store s hello účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="ff57d-184">If you are copying from an Azure Data Lake Store account as hello source using a Data Lake Analytics account, you do not need tooassociate hello Data Lake Store account with hello Data Lake Analytics account.</span></span> <span data-ttu-id="ff57d-185">Hello požadavek tooassociate hello zdrojové úložiště s hello účtu Data Lake Analytics je pouze v případě, že zdroj hello je účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ff57d-185">hello requirement tooassociate hello source store with hello Data Lake Analytics account is only when hello source is an Azure Storage account.</span></span>
>
>

<span data-ttu-id="ff57d-186">Spusťte následující příkaz toocopy z účtu Data Lake Store tooa objektu blob úložiště Azure pomocí účtu Data Lake Analytics hello:</span><span class="sxs-lookup"><span data-stu-id="ff57d-186">Run hello following command toocopy from an Azure Storage blob tooa Data Lake Store account using Data Lake Analytics account:</span></span>

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

<span data-ttu-id="ff57d-187">Například:</span><span class="sxs-lookup"><span data-stu-id="ff57d-187">For example:</span></span>

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2

<span data-ttu-id="ff57d-188">Podobně spusťte následující příkaz toocopy z účtu Data Lake Store tooa objektu blob úložiště Azure pomocí účtu Data Lake Analytics hello:</span><span class="sxs-lookup"><span data-stu-id="ff57d-188">Similarly, run hello following command toocopy from an Azure Storage blob tooa Data Lake Store account using Data Lake Analytics account:</span></span>

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

### <a name="performance-considerations"></a><span data-ttu-id="ff57d-189">Otázky výkonu</span><span class="sxs-lookup"><span data-stu-id="ff57d-189">Performance considerations</span></span>

<span data-ttu-id="ff57d-190">Při kopírování dat v rozsahu hello terabajtů, AdlCopy pomocí účtu Azure Data Lake Analytics poskytuje lepší a více předvídatelný výkon.</span><span class="sxs-lookup"><span data-stu-id="ff57d-190">When copying data in hello range of terabytes, using AdlCopy with your own Azure Data Lake Analytics account provides better and more predictable performance.</span></span> <span data-ttu-id="ff57d-191">Hello parametr, který by měl být přizpůsobená je hello počet toouse Azure Data Lake Analytics jednotky pro úlohu kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="ff57d-191">hello parameter that should be tuned is hello number of Azure Data Lake Analytics Units toouse for hello copy job.</span></span> <span data-ttu-id="ff57d-192">Zvýšením počtu hello jednotek zvýší hello výkon vaší úlohu kopírování.</span><span class="sxs-lookup"><span data-stu-id="ff57d-192">Increasing hello number of units will increase hello performance of your copy job.</span></span> <span data-ttu-id="ff57d-193">Každý soubor toobe zkopírovat, můžete použít maximální jednu jednotku.</span><span class="sxs-lookup"><span data-stu-id="ff57d-193">Each file toobe copied can use maximum one unit.</span></span> <span data-ttu-id="ff57d-194">Určení další jednotky než hello počet kopírované soubory nebude zvýšit výkon.</span><span class="sxs-lookup"><span data-stu-id="ff57d-194">Specifying more units than hello number of files being copied will not increase performance.</span></span>

## <a name="use-adlcopy-toocopy-data-using-pattern-matching"></a><span data-ttu-id="ff57d-195">Použít data toocopy AdlCopy použití porovnávání vzorů</span><span class="sxs-lookup"><span data-stu-id="ff57d-195">Use AdlCopy toocopy data using pattern matching</span></span>
<span data-ttu-id="ff57d-196">V této části se dozvíte, jak toouse AdlCopy toocopy dat ze zdroje (v našem příkladu níže používáme Azure Storage Blob) účtu Data Lake Store cílové tooa použití porovnávání vzorů.</span><span class="sxs-lookup"><span data-stu-id="ff57d-196">In this section, you learn how toouse AdlCopy toocopy data from a source (in our example below we use Azure Storage Blob) tooa destination Data Lake Store account using pattern matching.</span></span> <span data-ttu-id="ff57d-197">Například můžete hello kroků toocopy všechny soubory s příponou CSV z hello zdroj blob toohello cíl.</span><span class="sxs-lookup"><span data-stu-id="ff57d-197">For example, you can use hello steps below toocopy all files with .csv extension from hello source blob toohello destination.</span></span>

1. <span data-ttu-id="ff57d-198">Otevřete příkazový řádek a přejděte toohello directory AdlCopy nainstalovanou, obvykle `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="ff57d-198">Open a command prompt and navigate toohello directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="ff57d-199">Spusťte následující příkaz toocopy hello všechny soubory s příponou *.csv, z konkrétní objekt blob z hello zdrojový kontejner tooa Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="ff57d-199">Run hello following command toocopy all files with *.csv extension from a specific blob from hello source container tooa Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    <span data-ttu-id="ff57d-200">Například:</span><span class="sxs-lookup"><span data-stu-id="ff57d-200">For example:</span></span>

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

## <a name="billing"></a><span data-ttu-id="ff57d-201">Fakturace</span><span class="sxs-lookup"><span data-stu-id="ff57d-201">Billing</span></span>
* <span data-ttu-id="ff57d-202">Pokud použijete nástroj AdlCopy hello jako samostatná, které bude platit pro odchozí náklady na přesunutí hello data, pokud není zdroj hello účet úložiště Azure ve stejné oblasti jako hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ff57d-202">If you use hello AdlCopy tool as standalone you will be billed for egress costs for moving data, if hello source Azure Storage account is not in hello same region as hello Data Lake Store.</span></span>
* <span data-ttu-id="ff57d-203">Pokud použijete nástroj AdlCopy hello s vaše Data Lake Analytics účet, standardní [Data Lake Analytics fakturace sazby](https://azure.microsoft.com/pricing/details/data-lake-analytics/) budou platit.</span><span class="sxs-lookup"><span data-stu-id="ff57d-203">If you use hello AdlCopy tool with your Data Lake Analytics account, standard [Data Lake Analytics billing rates](https://azure.microsoft.com/pricing/details/data-lake-analytics/) will apply.</span></span>

## <a name="considerations-for-using-adlcopy"></a><span data-ttu-id="ff57d-204">Důležité informace týkající se použití AdlCopy</span><span class="sxs-lookup"><span data-stu-id="ff57d-204">Considerations for using AdlCopy</span></span>
* <span data-ttu-id="ff57d-205">AdlCopy (pro verzi 1.0.5), podporuje kopírování dat ze zdrojů, které se souhrnně mít víc než tisíce souborů a složek.</span><span class="sxs-lookup"><span data-stu-id="ff57d-205">AdlCopy (for version 1.0.5), supports copying data from sources that collectively have more than thousands of files and folders.</span></span> <span data-ttu-id="ff57d-206">Ale pokud dojde k potížím kopírování velké datové sady, můžete distribuovat hello soubory a složky do jiné podřízené složky a hello cesta toothose dílčí složky místo toho použít jako zdroj hello.</span><span class="sxs-lookup"><span data-stu-id="ff57d-206">However, if you encounter issues copying a large dataset, you can distribute hello files/folders into different sub-folders and use hello path toothose sub-folders as hello source instead.</span></span>

## <a name="performance-considerations-for-using-adlcopy"></a><span data-ttu-id="ff57d-207">Důležité informace o výkonu pro používání AdlCopy</span><span class="sxs-lookup"><span data-stu-id="ff57d-207">Performance considerations for using AdlCopy</span></span>

<span data-ttu-id="ff57d-208">AdlCopy podporuje kopírování dat obsahující tisíce souborů a složek.</span><span class="sxs-lookup"><span data-stu-id="ff57d-208">AdlCopy supports copying data containing thousands of files and folders.</span></span> <span data-ttu-id="ff57d-209">Ale pokud dojde k potížím kopírování velké datové sady, můžete distribuovat hello soubory a složky do menší podsložky.</span><span class="sxs-lookup"><span data-stu-id="ff57d-209">However, if you encounter issues copying a large dataset, you can distribute hello files/folders into smaller sub-folders.</span></span> <span data-ttu-id="ff57d-210">AdlCopy bylo vytvořeno pro ad hoc kopie.</span><span class="sxs-lookup"><span data-stu-id="ff57d-210">AdlCopy was built for ad hoc copies.</span></span> <span data-ttu-id="ff57d-211">Pokud se pokoušíte toocopy dat pravidelně, měli byste zvážit použití [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) poskytuje úplné správy kolem operace kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="ff57d-211">If you are trying toocopy data on a recurring basis, you should consider using [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) that provides full management around hello copy operations.</span></span>

## <a name="release-notes"></a><span data-ttu-id="ff57d-212">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="ff57d-212">Release notes</span></span>
* <span data-ttu-id="ff57d-213">1.0.13 - li zkopírovat data toohello příkazy stejný účet Azure Data Lake Store napříč více adlcopy tooreenter svoje přihlašovací údaje pro každou spustit už nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="ff57d-213">1.0.13 - If you are copying data toohello same Azure Data Lake Store account across multiple adlcopy commands, you do not need tooreenter your credentials for each run anymore.</span></span> <span data-ttu-id="ff57d-214">Adlcopy nyní v mezipaměti tyto informace napříč více spustí.</span><span class="sxs-lookup"><span data-stu-id="ff57d-214">Adlcopy will now cache that information across multiple runs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff57d-215">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ff57d-215">Next steps</span></span>
* [<span data-ttu-id="ff57d-216">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ff57d-216">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="ff57d-217">Použití Azure Data Lake Analytics se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ff57d-217">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="ff57d-218">Použití Azure HDInsight se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ff57d-218">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
