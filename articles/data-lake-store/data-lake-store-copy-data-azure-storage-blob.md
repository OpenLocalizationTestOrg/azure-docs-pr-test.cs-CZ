---
title: "Ke zkopírování dat z úložiště objektů BLOB Azure do Data Lake Store | Microsoft Docs"
description: "Pomocí nástroje AdlCopy ke zkopírování dat z úložiště objektů BLOB Azure do Data Lake Store"
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
ms.openlocfilehash: 68f44991432a76c2ef1c79ec6dffdea4c62bcb17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a><span data-ttu-id="3b613-103">Kopírování dat z Azure Storage Blob do služby Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="3b613-103">Copy data from Azure Storage Blobs to Data Lake Store</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3b613-104">Pomocí DistCp</span><span class="sxs-lookup"><span data-stu-id="3b613-104">Using DistCp</span></span>](data-lake-store-copy-data-wasb-distcp.md)
> * [<span data-ttu-id="3b613-105">Pomocí AdlCopy</span><span class="sxs-lookup"><span data-stu-id="3b613-105">Using AdlCopy</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
>
>

<span data-ttu-id="3b613-106">Azure Data Lake Store poskytuje nástroj příkazového řádku, [AdlCopy](http://aka.ms/downloadadlcopy), ke zkopírování dat z následujících zdrojů:</span><span class="sxs-lookup"><span data-stu-id="3b613-106">Azure Data Lake Store provides a command line tool, [AdlCopy](http://aka.ms/downloadadlcopy), to copy data from the following sources:</span></span>

* <span data-ttu-id="3b613-107">Z Azure úložiště objektů BLOB do Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3b613-107">From Azure Storage Blobs into Data Lake Store.</span></span> <span data-ttu-id="3b613-108">AdlCopy nelze použít ke zkopírování dat z Data Lake Store do objektů BLOB služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3b613-108">You cannot use AdlCopy to copy data from Data Lake Store to Azure Storage blobs.</span></span>
* <span data-ttu-id="3b613-109">Mezi dvěma účty Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3b613-109">Between two Azure Data Lake Store accounts.</span></span>

<span data-ttu-id="3b613-110">Navíc můžete použít nástroj AdlCopy ve dvou různých režimech:</span><span class="sxs-lookup"><span data-stu-id="3b613-110">Also, you can use the AdlCopy tool in two different modes:</span></span>

* <span data-ttu-id="3b613-111">**Samostatné**, kde nástroj používá Data Lake Store prostředky k provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="3b613-111">**Standalone**, where the tool uses Data Lake Store resources to perform the task.</span></span>
* <span data-ttu-id="3b613-112">**Pomocí účtu Data Lake Analytics**, kdy se jednotky přiřazené k vašemu účtu Data Lake Analytics používá k provedení operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="3b613-112">**Using a Data Lake Analytics account**, where the units assigned to your Data Lake Analytics account are used to perform the copy operation.</span></span> <span data-ttu-id="3b613-113">Můžete chtít tuto možnost použijte, pokud chcete provádět úlohy kopírování předvídatelný způsobem.</span><span class="sxs-lookup"><span data-stu-id="3b613-113">You might want to use this option when you are looking to perform the copy tasks in a predictable manner.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b613-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3b613-114">Prerequisites</span></span>
<span data-ttu-id="3b613-115">Je nutné, abyste před zahájením tohoto článku měli tyto položky:</span><span class="sxs-lookup"><span data-stu-id="3b613-115">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="3b613-116">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="3b613-116">**An Azure subscription**.</span></span> <span data-ttu-id="3b613-117">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3b613-117">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="3b613-118">**Objektů BLOB služby Azure Storage** kontejneru určitými daty.</span><span class="sxs-lookup"><span data-stu-id="3b613-118">**Azure Storage Blobs** container with some data.</span></span>
* <span data-ttu-id="3b613-119">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="3b613-119">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="3b613-120">Pokyny o tom, jak vytvořit najdete v tématu [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="3b613-120">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="3b613-121">**Účtu Azure Data Lake Analytics (volitelné)** -najdete v části [Začínáme s Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) pokyny o tom, jak vytvořit účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3b613-121">**Azure Data Lake Analytics account (optional)** - See [Get started with Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) for instructions on how to create a Data Lake Store account.</span></span>
* <span data-ttu-id="3b613-122">**Nástroj AdlCopy**.</span><span class="sxs-lookup"><span data-stu-id="3b613-122">**AdlCopy tool**.</span></span> <span data-ttu-id="3b613-123">Nainstalujte nástroj AdlCopy z [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).</span><span class="sxs-lookup"><span data-stu-id="3b613-123">Install the AdlCopy tool from [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).</span></span>

## <a name="syntax-of-the-adlcopy-tool"></a><span data-ttu-id="3b613-124">Syntaxe nástroje AdlCopy</span><span class="sxs-lookup"><span data-stu-id="3b613-124">Syntax of the AdlCopy tool</span></span>
<span data-ttu-id="3b613-125">Použijte následující syntaxi pro práci s nástrojem AdlCopy</span><span class="sxs-lookup"><span data-stu-id="3b613-125">Use the following syntax to work with the AdlCopy tool</span></span>

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern

<span data-ttu-id="3b613-126">Parametry v syntaxi jsou následující:</span><span class="sxs-lookup"><span data-stu-id="3b613-126">The parameters in the syntax are described below:</span></span>

| <span data-ttu-id="3b613-127">Možnost</span><span class="sxs-lookup"><span data-stu-id="3b613-127">Option</span></span> | <span data-ttu-id="3b613-128">Popis</span><span class="sxs-lookup"><span data-stu-id="3b613-128">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3b613-129">Zdroj</span><span class="sxs-lookup"><span data-stu-id="3b613-129">Source</span></span> |<span data-ttu-id="3b613-130">Určuje umístění zdrojových dat v objektu blob úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="3b613-130">Specifies the location of the source data in the Azure storage blob.</span></span> <span data-ttu-id="3b613-131">Zdrojem může být kontejner objektů blob, objekt blob nebo jiný účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3b613-131">The source can be a blob container, a blob, or another Data Lake Store account.</span></span> |
| <span data-ttu-id="3b613-132">Cíle</span><span class="sxs-lookup"><span data-stu-id="3b613-132">Dest</span></span> |<span data-ttu-id="3b613-133">Určuje cíl Data Lake Store pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="3b613-133">Specifies the Data Lake Store destination to copy to.</span></span> |
| <span data-ttu-id="3b613-134">SourceKey</span><span class="sxs-lookup"><span data-stu-id="3b613-134">SourceKey</span></span> |<span data-ttu-id="3b613-135">Určuje přístupový klíč úložiště pro zdrojový objekt blob úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="3b613-135">Specifies the storage access key for the Azure storage blob source.</span></span> <span data-ttu-id="3b613-136">To je potřeba, pouze pokud je zdroj kontejner objektů blob nebo objekt blob.</span><span class="sxs-lookup"><span data-stu-id="3b613-136">This is required only if the source is a blob container or a blob.</span></span> |
| <span data-ttu-id="3b613-137">Účet</span><span class="sxs-lookup"><span data-stu-id="3b613-137">Account</span></span> |<span data-ttu-id="3b613-138">**Volitelné**.</span><span class="sxs-lookup"><span data-stu-id="3b613-138">**Optional**.</span></span> <span data-ttu-id="3b613-139">Použijte, pokud chcete spustit úlohu kopírování pomocí účtu Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="3b613-139">Use this if you want to use Azure Data Lake Analytics account to run the copy job.</span></span> <span data-ttu-id="3b613-140">Pokud použijete možnost /Account v syntaxi, ale nezadávejte účet Data Lake Analytics, AdlCopy používá výchozí účet pro spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="3b613-140">If you use the /Account option in the syntax but do not specify a Data Lake Analytics account, AdlCopy uses a default account to run the job.</span></span> <span data-ttu-id="3b613-141">Navíc pokud použijete tuto možnost, musíte přidat zdroj (Azure Storage Blob) a cíl (Azure Data Lake Store) jako zdroje dat pro váš účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="3b613-141">Also, if you use this option, you must add the source (Azure Storage Blob) and destination (Azure Data Lake Store) as data sources for your Data Lake Analytics account.</span></span> |
| <span data-ttu-id="3b613-142">Jednotky</span><span class="sxs-lookup"><span data-stu-id="3b613-142">Units</span></span> |<span data-ttu-id="3b613-143">Určuje počet jednotek Data Lake Analytics, které se použijí pro úlohu kopírování.</span><span class="sxs-lookup"><span data-stu-id="3b613-143">Specifies the number of Data Lake Analytics units that will be used for the copy job.</span></span> <span data-ttu-id="3b613-144">Tato možnost je povinná, pokud použijete **/účet** možnost zadat účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="3b613-144">This option is mandatory if you use the **/Account** option to specify the Data Lake Analytics account.</span></span> |
| <span data-ttu-id="3b613-145">vzor</span><span class="sxs-lookup"><span data-stu-id="3b613-145">Pattern</span></span> |<span data-ttu-id="3b613-146">Určuje vzor regulárního výrazu, která určuje, které objekty BLOB nebo připojené soubory.</span><span class="sxs-lookup"><span data-stu-id="3b613-146">Specifies a regex pattern that indicates which blobs or files to copy.</span></span> <span data-ttu-id="3b613-147">AdlCopy používá odpovídající malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="3b613-147">AdlCopy uses case-sensitive matching.</span></span> <span data-ttu-id="3b613-148">Výchozím způsobem používaným při zadat žádné vzor je zkopírujte všechny položky.</span><span class="sxs-lookup"><span data-stu-id="3b613-148">The default pattern used when no pattern is specified is to copy all items.</span></span> <span data-ttu-id="3b613-149">Zadání více vzorů souborů není podporováno.</span><span class="sxs-lookup"><span data-stu-id="3b613-149">Specifying multiple file patterns is not supported.</span></span> |

## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a><span data-ttu-id="3b613-150">Použít AdlCopy (jako samostatná) ke zkopírování dat z objektu blob Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3b613-150">Use AdlCopy (as standalone) to copy data from an Azure Storage blob</span></span>
1. <span data-ttu-id="3b613-151">Otevřete příkazový řádek a přejděte do adresáře, kde AdlCopy je nainstalován, obvykle `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="3b613-151">Open a command prompt and navigate to the directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="3b613-152">Spusťte následující příkaz pro kopírování konkrétní objekt blob z kontejneru zdroje do Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="3b613-152">Run the following command to copy a specific blob from the source container to a Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    <span data-ttu-id="3b613-153">Například:</span><span class="sxs-lookup"><span data-stu-id="3b613-153">For example:</span></span>

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    >[AZURE.NOTE] <span data-ttu-id="3b613-154">Syntaxi výše Určuje soubor, který se má zkopírovat do složky v účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3b613-154">The syntax above specifies the file to be copied to a folder in the Data Lake Store account.</span></span> <span data-ttu-id="3b613-155">Nástroj AdlCopy vytvoří složku, pokud zadaný název složky neexistuje.</span><span class="sxs-lookup"><span data-stu-id="3b613-155">AdlCopy tool creates a folder if the specified folder name does not exist.</span></span>

    <span data-ttu-id="3b613-156">Zobrazí se výzva k zadání přihlašovacích údajů pro předplatné Azure, ve kterém máte účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3b613-156">You will be prompted to enter the credentials for the Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="3b613-157">Zobrazí se výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="3b613-157">You will see an output similar to the following:</span></span>

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

1. <span data-ttu-id="3b613-158">Všechny objekty BLOB můžete také zkopírovat z jednoho kontejneru typu do účtu Data Lake Store pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="3b613-158">You can also copy all the blobs from one container to the Data Lake Store account using the following command:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    <span data-ttu-id="3b613-159">Například:</span><span class="sxs-lookup"><span data-stu-id="3b613-159">For example:</span></span>

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

### <a name="performance-considerations"></a><span data-ttu-id="3b613-160">Otázky výkonu</span><span class="sxs-lookup"><span data-stu-id="3b613-160">Performance considerations</span></span>

<span data-ttu-id="3b613-161">Pokud kopírujete z účtu úložiště objektů Blob Azure, můžete být omezeny během kopírování na straně úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="3b613-161">If you are copying from an Azure Blob Storage account, you may be throttled during copy on the blob storage side.</span></span> <span data-ttu-id="3b613-162">To způsobí snížení výkonu úlohu kopírování.</span><span class="sxs-lookup"><span data-stu-id="3b613-162">This will degrade the performance of your copy job.</span></span> <span data-ttu-id="3b613-163">Další informace o omezení Azure Blob Storage najdete v tématu omezení Azure Storage v [předplatného Azure a omezení služby](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="3b613-163">To learn more about the limits of Azure Blob Storage, see Azure Storage limits at [Azure subscription and service limits](../azure-subscription-service-limits.md).</span></span>

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a><span data-ttu-id="3b613-164">Použít AdlCopy (jako samostatná) ke zkopírování dat z jiného účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="3b613-164">Use AdlCopy (as standalone) to copy data from another Data Lake Store account</span></span>
<span data-ttu-id="3b613-165">Můžete taky AdlCopy ke kopírování dat mezi dvěma účty Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3b613-165">You can also use AdlCopy to copy data between two Data Lake Store accounts.</span></span>

1. <span data-ttu-id="3b613-166">Otevřete příkazový řádek a přejděte do adresáře, kde AdlCopy je nainstalován, obvykle `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="3b613-166">Open a command prompt and navigate to the directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="3b613-167">Spusťte následující příkaz pro kopírování konkrétní soubor z jednoho účtu Data Lake Store do jiného.</span><span class="sxs-lookup"><span data-stu-id="3b613-167">Run the following command to copy a specific file from one Data Lake Store account to another.</span></span>

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    <span data-ttu-id="3b613-168">Například:</span><span class="sxs-lookup"><span data-stu-id="3b613-168">For example:</span></span>

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

   > [!NOTE]
   > <span data-ttu-id="3b613-169">Syntaxi výše Určuje soubor, který se má zkopírovat do složky v cílovém účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3b613-169">The syntax above specifies the file to be copied to a folder in the destination Data Lake Store account.</span></span> <span data-ttu-id="3b613-170">Nástroj AdlCopy vytvoří složku, pokud zadaný název složky neexistuje.</span><span class="sxs-lookup"><span data-stu-id="3b613-170">AdlCopy tool creates a folder if the specified folder name does not exist.</span></span>
   >
   >

    <span data-ttu-id="3b613-171">Zobrazí se výzva k zadání přihlašovacích údajů pro předplatné Azure, ve kterém máte účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3b613-171">You will be prompted to enter the credentials for the Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="3b613-172">Zobrazí se výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="3b613-172">You will see an output similar to the following:</span></span>

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.
3. <span data-ttu-id="3b613-173">Následující příkaz zkopíruje všechny soubory z určité složky ve zdrojovém účtu Data Lake Store do složky v cílovém účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3b613-173">The following command copies all files from a specific folder in the source Data Lake Store account to a folder in the destination Data Lake Store account.</span></span>

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

### <a name="performance-considerations"></a><span data-ttu-id="3b613-174">Otázky výkonu</span><span class="sxs-lookup"><span data-stu-id="3b613-174">Performance considerations</span></span>

<span data-ttu-id="3b613-175">Při použití AdlCopy jako samostatný nástroj, kopie je spustit na sdílené, spravované prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="3b613-175">When using AdlCopy as a standalone tool, the copy is run on shared, Azure managed resources.</span></span> <span data-ttu-id="3b613-176">Výkon, které může dojít v tomto prostředí závisí na zatížení systému a dostupné prostředky.</span><span class="sxs-lookup"><span data-stu-id="3b613-176">The performance you may get in this environment depends on system load and available resources.</span></span> <span data-ttu-id="3b613-177">Tento režim je nejvhodnější pro malé přenosy na základě ad hoc.</span><span class="sxs-lookup"><span data-stu-id="3b613-177">This mode is best used for small transfers on an ad hoc basis.</span></span> <span data-ttu-id="3b613-178">Žádné parametry musí být přizpůsobená při použití AdlCopy jako samostatný nástroj.</span><span class="sxs-lookup"><span data-stu-id="3b613-178">No parameters need to be tuned when using AdlCopy as a standalone tool.</span></span>

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a><span data-ttu-id="3b613-179">Pomocí AdlCopy (s účtem Data Lake Analytics) ke kopírování dat</span><span class="sxs-lookup"><span data-stu-id="3b613-179">Use AdlCopy (with Data Lake Analytics account) to copy data</span></span>
<span data-ttu-id="3b613-180">Váš účet Data Lake Analytics můžete taky spustit úlohu AdlCopy ke zkopírování dat z Azure úložiště objektů BLOB do Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3b613-180">You can also use your Data Lake Analytics account to run the AdlCopy job to copy data from Azure storage blobs to Data Lake Store.</span></span> <span data-ttu-id="3b613-181">Tato možnost by obvykle použijte, pokud data, která mají být přesunuta, je v rozsahu gigabajtech a terabajty a chcete, aby propustnost lepší a předvídatelný výkon.</span><span class="sxs-lookup"><span data-stu-id="3b613-181">You would typically use this option when the data to be moved is in the range of gigabytes and terabytes, and you want better and predictable performance throughput.</span></span>

<span data-ttu-id="3b613-182">Pomocí účtu Data Lake Analytics s AdlCopy zkopírovat z objektu Blob úložiště Azure, je nutné přidat zdroj (Azure Storage Blob) jako zdroj dat pro váš účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="3b613-182">To use your Data Lake Analytics account with AdlCopy to copy from an Azure Storage Blob, the source (Azure Storage Blob) must be added as a data source for your Data Lake Analytics account.</span></span> <span data-ttu-id="3b613-183">Pokyny k přidání dalších zdrojů dat do účtu Data Lake Analytics najdete v tématu [zdrojů dat účtu Správa Data Lake Analytics](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).</span><span class="sxs-lookup"><span data-stu-id="3b613-183">For instructions on adding additional data sources to your Data Lake Analytics account, see [Manage Data Lake Analytics account data sources](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).</span></span>

> [!NOTE]
> <span data-ttu-id="3b613-184">Pokud kopírujete z účtu Azure Data Lake Store jako zdroj pomocí účtu Data Lake Analytics, není potřeba přidružit účet Data Lake Store účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="3b613-184">If you are copying from an Azure Data Lake Store account as the source using a Data Lake Analytics account, you do not need to associate the Data Lake Store account with the Data Lake Analytics account.</span></span> <span data-ttu-id="3b613-185">Požadavek na zdrojové úložiště přidružit účet Data Lake Analytics je jenom v případě, že zdroj je účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="3b613-185">The requirement to associate the source store with the Data Lake Analytics account is only when the source is an Azure Storage account.</span></span>
>
>

<span data-ttu-id="3b613-186">Spusťte následující příkaz pro kopírování z objektu blob Azure Storage k účtu Data Lake Store pomocí účtu Data Lake Analytics:</span><span class="sxs-lookup"><span data-stu-id="3b613-186">Run the following command to copy from an Azure Storage blob to a Data Lake Store account using Data Lake Analytics account:</span></span>

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

<span data-ttu-id="3b613-187">Například:</span><span class="sxs-lookup"><span data-stu-id="3b613-187">For example:</span></span>

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2

<span data-ttu-id="3b613-188">Podobně spusťte následující příkaz pro kopírování z objektu blob Azure Storage k účtu Data Lake Store pomocí účtu Data Lake Analytics:</span><span class="sxs-lookup"><span data-stu-id="3b613-188">Similarly, run the following command to copy from an Azure Storage blob to a Data Lake Store account using Data Lake Analytics account:</span></span>

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

### <a name="performance-considerations"></a><span data-ttu-id="3b613-189">Otázky výkonu</span><span class="sxs-lookup"><span data-stu-id="3b613-189">Performance considerations</span></span>

<span data-ttu-id="3b613-190">Při kopírování dat v rozsahu terabajtů, AdlCopy pomocí účtu Azure Data Lake Analytics poskytuje lepší a více předvídatelný výkon.</span><span class="sxs-lookup"><span data-stu-id="3b613-190">When copying data in the range of terabytes, using AdlCopy with your own Azure Data Lake Analytics account provides better and more predictable performance.</span></span> <span data-ttu-id="3b613-191">Parametr, který by měl být přizpůsobená je počet Azure Data Lake Analytics jednotky pro úlohu kopírování.</span><span class="sxs-lookup"><span data-stu-id="3b613-191">The parameter that should be tuned is the number of Azure Data Lake Analytics Units to use for the copy job.</span></span> <span data-ttu-id="3b613-192">Zvýšením počtu jednotek zvýší výkon vaší úlohu kopírování.</span><span class="sxs-lookup"><span data-stu-id="3b613-192">Increasing the number of units will increase the performance of your copy job.</span></span> <span data-ttu-id="3b613-193">Každý soubor, který se má zkopírovat, můžete použít maximální jednu jednotku.</span><span class="sxs-lookup"><span data-stu-id="3b613-193">Each file to be copied can use maximum one unit.</span></span> <span data-ttu-id="3b613-194">Určení další jednotky, než je počet souborů, které jsou kopírovány nebude zvýšit výkon.</span><span class="sxs-lookup"><span data-stu-id="3b613-194">Specifying more units than the number of files being copied will not increase performance.</span></span>

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a><span data-ttu-id="3b613-195">Použít AdlCopy ke zkopírování dat pomocí shoda vzoru</span><span class="sxs-lookup"><span data-stu-id="3b613-195">Use AdlCopy to copy data using pattern matching</span></span>
<span data-ttu-id="3b613-196">V této části můžete další informace o použití AdlCopy ke kopírování dat ze zdroje (v našem příkladu níže používáme Azure Storage Blob) na účet Data Lake Store cílové použití porovnávání vzorů.</span><span class="sxs-lookup"><span data-stu-id="3b613-196">In this section, you learn how to use AdlCopy to copy data from a source (in our example below we use Azure Storage Blob) to a destination Data Lake Store account using pattern matching.</span></span> <span data-ttu-id="3b613-197">Například můžete použít následující postup pro kopírování všech souborů s příponou CSV z zdrojový objekt blob do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="3b613-197">For example, you can use the steps below to copy all files with .csv extension from the source blob to the destination.</span></span>

1. <span data-ttu-id="3b613-198">Otevřete příkazový řádek a přejděte do adresáře, kde AdlCopy je nainstalován, obvykle `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="3b613-198">Open a command prompt and navigate to the directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="3b613-199">Spusťte následující příkaz pro kopírování všech souborů s příponou *.csv z konkrétní objekt blob z kontejneru zdroje do Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="3b613-199">Run the following command to copy all files with *.csv extension from a specific blob from the source container to a Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    <span data-ttu-id="3b613-200">Například:</span><span class="sxs-lookup"><span data-stu-id="3b613-200">For example:</span></span>

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

## <a name="billing"></a><span data-ttu-id="3b613-201">Fakturace</span><span class="sxs-lookup"><span data-stu-id="3b613-201">Billing</span></span>
* <span data-ttu-id="3b613-202">Pokud použijete nástroj AdlCopy jako samostatné vám bude účtován pro náklady na celkový výstup pro přesun dat, pokud není zdrojový účet úložiště Azure ve stejné oblasti jako Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3b613-202">If you use the AdlCopy tool as standalone you will be billed for egress costs for moving data, if the source Azure Storage account is not in the same region as the Data Lake Store.</span></span>
* <span data-ttu-id="3b613-203">Pokud použijete nástroj AdlCopy s vaše Data Lake Analytics účet, standardní [Data Lake Analytics fakturace sazby](https://azure.microsoft.com/pricing/details/data-lake-analytics/) budou platit.</span><span class="sxs-lookup"><span data-stu-id="3b613-203">If you use the AdlCopy tool with your Data Lake Analytics account, standard [Data Lake Analytics billing rates](https://azure.microsoft.com/pricing/details/data-lake-analytics/) will apply.</span></span>

## <a name="considerations-for-using-adlcopy"></a><span data-ttu-id="3b613-204">Důležité informace týkající se použití AdlCopy</span><span class="sxs-lookup"><span data-stu-id="3b613-204">Considerations for using AdlCopy</span></span>
* <span data-ttu-id="3b613-205">AdlCopy (pro verzi 1.0.5), podporuje kopírování dat ze zdrojů, které se souhrnně mít víc než tisíce souborů a složek.</span><span class="sxs-lookup"><span data-stu-id="3b613-205">AdlCopy (for version 1.0.5), supports copying data from sources that collectively have more than thousands of files and folders.</span></span> <span data-ttu-id="3b613-206">Ale pokud dojde k potížím kopírování velké datové sady, můžete distribuovat soubory a složky do jiné podřízené složky a místo toho použijte cestu pro tyto podsložky jako zdroj.</span><span class="sxs-lookup"><span data-stu-id="3b613-206">However, if you encounter issues copying a large dataset, you can distribute the files/folders into different sub-folders and use the path to those sub-folders as the source instead.</span></span>

## <a name="performance-considerations-for-using-adlcopy"></a><span data-ttu-id="3b613-207">Důležité informace o výkonu pro používání AdlCopy</span><span class="sxs-lookup"><span data-stu-id="3b613-207">Performance considerations for using AdlCopy</span></span>

<span data-ttu-id="3b613-208">AdlCopy podporuje kopírování dat obsahující tisíce souborů a složek.</span><span class="sxs-lookup"><span data-stu-id="3b613-208">AdlCopy supports copying data containing thousands of files and folders.</span></span> <span data-ttu-id="3b613-209">Ale pokud dojde k potížím kopírování velké datové sady, můžete distribuovat soubory a složky do menší podsložky.</span><span class="sxs-lookup"><span data-stu-id="3b613-209">However, if you encounter issues copying a large dataset, you can distribute the files/folders into smaller sub-folders.</span></span> <span data-ttu-id="3b613-210">AdlCopy bylo vytvořeno pro ad hoc kopie.</span><span class="sxs-lookup"><span data-stu-id="3b613-210">AdlCopy was built for ad hoc copies.</span></span> <span data-ttu-id="3b613-211">Pokud chcete zkopírovat data na základě opakování, měli byste zvážit použití [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) poskytuje úplné správy kolem operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="3b613-211">If you are trying to copy data on a recurring basis, you should consider using [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) that provides full management around the copy operations.</span></span>

## <a name="release-notes"></a><span data-ttu-id="3b613-212">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="3b613-212">Release notes</span></span>
* <span data-ttu-id="3b613-213">1.0.13 - li zkopírovat data na stejný účet Azure Data Lake Store napříč více příkazů adlcopy, není nutné znovu zadat přihlašovací údaje při každém spuštění už.</span><span class="sxs-lookup"><span data-stu-id="3b613-213">1.0.13 - If you are copying data to the same Azure Data Lake Store account across multiple adlcopy commands, you do not need to reenter your credentials for each run anymore.</span></span> <span data-ttu-id="3b613-214">Adlcopy nyní v mezipaměti tyto informace napříč více spustí.</span><span class="sxs-lookup"><span data-stu-id="3b613-214">Adlcopy will now cache that information across multiple runs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b613-215">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3b613-215">Next steps</span></span>
* [<span data-ttu-id="3b613-216">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="3b613-216">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="3b613-217">Použití Azure Data Lake Analytics se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="3b613-217">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="3b613-218">Použití Azure HDInsight se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="3b613-218">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
