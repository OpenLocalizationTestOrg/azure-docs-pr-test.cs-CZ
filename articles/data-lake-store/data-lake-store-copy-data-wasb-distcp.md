---
title: "aaaCopy tooand data z WASB do Data Lake Store pomocí Distcp | Microsoft Docs"
description: "Použití Distcp nástroj toocopy data tooand z Azure úložiště objektů BLOB tooData Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ae2e9506-69dd-4b95-8759-4dadca37ea70
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: ec23410bbab0f82449a475412bc3b097c4a8fccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-distcp-toocopy-data-between-azure-storage-blobs-and-data-lake-store"></a><span data-ttu-id="23940-103">Použití Distcp toocopy dat mezi objektů BLOB služby Azure Storage a Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="23940-103">Use Distcp toocopy data between Azure Storage Blobs and Data Lake Store</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="23940-104">Pomocí DistCp</span><span class="sxs-lookup"><span data-stu-id="23940-104">Using DistCp</span></span>](data-lake-store-copy-data-wasb-distcp.md)
> * [<span data-ttu-id="23940-105">Pomocí AdlCopy</span><span class="sxs-lookup"><span data-stu-id="23940-105">Using AdlCopy</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
>
>

<span data-ttu-id="23940-106">Po vytvoření clusteru služby HDInsight, který má přístup k účtu Data Lake Store tooa, můžete použít nástroje ekosystém Hadoop jako Distcp toocopy data **tooand z** úložišti clusteru HDInsight (WASB) do účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23940-106">Once you have created an HDInsight cluster that has access tooa Data Lake Store account, you can use Hadoop ecosystem tools like Distcp toocopy data **tooand from** an HDInsight cluster storage (WASB) into a Data Lake Store account.</span></span> <span data-ttu-id="23940-107">Tento článek obsahuje pokyny, jak tooachieve to.</span><span class="sxs-lookup"><span data-stu-id="23940-107">This article provides instructions on how tooachieve this.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="23940-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="23940-108">Prerequisites</span></span>
<span data-ttu-id="23940-109">Před zahájením tohoto článku, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="23940-109">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="23940-110">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="23940-110">**An Azure subscription**.</span></span> <span data-ttu-id="23940-111">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="23940-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="23940-112">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="23940-112">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="23940-113">Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="23940-113">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="23940-114">**Azure HDInsight cluster** s tooa přístup k účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23940-114">**Azure HDInsight cluster** with access tooa Data Lake Store account.</span></span> <span data-ttu-id="23940-115">V tématu [vytvoření clusteru HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="23940-115">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="23940-116">Ujistěte se, že povolení vzdálené plochy pro hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="23940-116">Make sure you enable Remote Desktop for hello cluster.</span></span>

## <a name="do-you-learn-fast-with-videos"></a><span data-ttu-id="23940-117">Pomáhají vám při učení videa?</span><span class="sxs-lookup"><span data-stu-id="23940-117">Do you learn fast with videos?</span></span>
<span data-ttu-id="23940-118">[Přehrát toto video](https://mix.office.com/watch/1liuojvdx6sie) o toocopy dat mezi objektů BLOB služby Azure Storage a Data Lake Store pomocí DistCp.</span><span class="sxs-lookup"><span data-stu-id="23940-118">[Watch this video](https://mix.office.com/watch/1liuojvdx6sie) on how toocopy data between Azure Storage Blobs and Data Lake Store using DistCp.</span></span>

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a><span data-ttu-id="23940-119">Použití Distcp z clusteru služby HDInsight Linux</span><span class="sxs-lookup"><span data-stu-id="23940-119">Use Distcp from an HDInsight Linux cluster</span></span>

<span data-ttu-id="23940-120">Cluster služby HDInsight se dodává s hello Distcp nástroj, který lze použít toocopy data z různých zdrojů do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="23940-120">An HDInsight cluster comes with hello Distcp utility, which can be used toocopy data from different sources into an HDInsight cluster.</span></span> <span data-ttu-id="23940-121">Pokud jste nakonfigurovali hello HDInsight clusteru toouse Data Lake Store jako další úložiště, hello Distcp nástroj lze použít tooand dat na více systémů pole toocopy z účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23940-121">If you have configured hello HDInsight cluster toouse Data Lake Store as an additional storage, hello Distcp utility can be used out-of-the-box toocopy data tooand from a Data Lake Store account as well.</span></span> <span data-ttu-id="23940-122">V této části podíváme na tom, jak toouse hello Distcp nástroj.</span><span class="sxs-lookup"><span data-stu-id="23940-122">In this section we look at how toouse hello Distcp utility.</span></span>

1. <span data-ttu-id="23940-123">Z plochy použijte SSH tooconnect toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="23940-123">From your desktop, use SSH tooconnect toohello cluster.</span></span> <span data-ttu-id="23940-124">V tématu [clusteru HDInsight se systémem Linux tooa Connect](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="23940-124">See [Connect tooa Linux-based HDInsight cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> <span data-ttu-id="23940-125">Spusťte příkazy hello z řádku SSH hello.</span><span class="sxs-lookup"><span data-stu-id="23940-125">Run hello commands from hello SSH prompt.</span></span>

2. <span data-ttu-id="23940-126">Ověřte, zda máte přístup hello Azure úložiště objektů BLOB (WASB).</span><span class="sxs-lookup"><span data-stu-id="23940-126">Verify whether you can access hello Azure Storage Blobs (WASB).</span></span> <span data-ttu-id="23940-127">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="23940-127">Run hello following command:</span></span>

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    <span data-ttu-id="23940-128">To by mělo poskytovat seznam obsah v hello storage blob.</span><span class="sxs-lookup"><span data-stu-id="23940-128">This should provide a list of contents in hello storage blob.</span></span>
3. <span data-ttu-id="23940-129">Stejně tak ověřte, zda máte přístup z clusteru hello hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23940-129">Similarly, verify whether you can access hello Data Lake Store account from hello cluster.</span></span> <span data-ttu-id="23940-130">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="23940-130">Run hello following command:</span></span>

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    <span data-ttu-id="23940-131">To by mělo poskytovat seznam souborů a složek v účtu Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="23940-131">This should provide a list of files/folders in hello Data Lake Store account.</span></span>
4. <span data-ttu-id="23940-132">Použití Distcp toocopy data z WASB tooa účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23940-132">Use Distcp toocopy data from WASB tooa Data Lake Store account.</span></span>

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    <span data-ttu-id="23940-133">Tím dojde ke zkopírování hello obsah hello **/příklad/data/gutenberg/** složky v WASB příliš**/myfolder** v hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23940-133">This will copy hello contents of hello **/example/data/gutenberg/** folder in WASB too**/myfolder** in hello Data Lake Store account.</span></span>
5. <span data-ttu-id="23940-134">Podobně použití Distcp toocopy data z tooWASB účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23940-134">Similarly, use Distcp toocopy data from Data Lake Store account tooWASB.</span></span>

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    <span data-ttu-id="23940-135">Tím dojde ke zkopírování obsahu hello **/myfolder** v hello Data Lake Store účtu příliš**/příklad/data/gutenberg/** složky v WASB.</span><span class="sxs-lookup"><span data-stu-id="23940-135">This will copy hello contents of **/myfolder** in hello Data Lake Store account too**/example/data/gutenberg/** folder in WASB.</span></span>

## <a name="performance-considerations-while-using-distcp"></a><span data-ttu-id="23940-136">Faktory ovlivňující výkon při použití DistCp</span><span class="sxs-lookup"><span data-stu-id="23940-136">Performance considerations while using DistCp</span></span>

<span data-ttu-id="23940-137">Vzhledem k tomu, že je nejnižší DistCp členitost je jeden soubor, nastavení hello maximální počet souběžných kopie je nejdůležitější parametr toooptimize hello ho s Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23940-137">Because DistCp’s lowest granularity is a single file, setting hello maximum number of simultaneous copies is hello most important parameter toooptimize it against Data Lake Store.</span></span> <span data-ttu-id="23940-138">Toto se řídí nastavení hello počtu mappers ('m ') parametr na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="23940-138">This is controlled by setting hello number of mappers (‘m’) parameter on hello command line.</span></span> <span data-ttu-id="23940-139">Tento parametr určuje maximální počet mappers, které budou použité toocopy data hello.</span><span class="sxs-lookup"><span data-stu-id="23940-139">This parameter specifies hello maximum number of mappers that will be used toocopy data.</span></span> <span data-ttu-id="23940-140">Výchozí hodnota je 20.</span><span class="sxs-lookup"><span data-stu-id="23940-140">Default value is 20.</span></span>

<span data-ttu-id="23940-141">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="23940-141">**Example**</span></span>

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder -m 100

### <a name="how-do-i-determine-hello-number-of-mappers-toouse"></a><span data-ttu-id="23940-142">Jak je možné zjistit počet hello mappers toouse?</span><span class="sxs-lookup"><span data-stu-id="23940-142">How do I determine hello number of mappers toouse?</span></span>

<span data-ttu-id="23940-143">Tady je několik rad, kterými se můžete řídit.</span><span class="sxs-lookup"><span data-stu-id="23940-143">Here's some guidance that you can use.</span></span>

* <span data-ttu-id="23940-144">**Krok 1: Určení celkové paměti YARN** -hello prvním krokem je toodetermine hello YARN paměti k dispozici toohello clusteru kde spuštění úlohy DistCp hello.</span><span class="sxs-lookup"><span data-stu-id="23940-144">**Step 1: Determine total YARN memory** - hello first step is toodetermine hello YARN memory available toohello cluster where you run hello DistCp job.</span></span> <span data-ttu-id="23940-145">Tyto informace jsou k dispozici v portálu Ambari hello přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="23940-145">This information is available in hello Ambari portal associated with hello cluster.</span></span> <span data-ttu-id="23940-146">Přejděte tooYARN a zobrazit hello konfigurací karta toosee hello YARN paměti.</span><span class="sxs-lookup"><span data-stu-id="23940-146">Navigate tooYARN and view hello Configs tab toosee hello YARN memory.</span></span> <span data-ttu-id="23940-147">tooget hello celkové YARN paměti, násobkem hello YARN paměti na jeden uzel hello počtu uzlů, že máte v clusteru.</span><span class="sxs-lookup"><span data-stu-id="23940-147">tooget hello total YARN memory, multiply hello YARN memory per node with hello number of nodes you have in your cluster.</span></span>

* <span data-ttu-id="23940-148">**Krok 2: Vypočítat hello počet mappers** -hello hodnotu **m** je rovna toohello podíl celkové paměti YARN dělený hello velikost YARN kontejneru.</span><span class="sxs-lookup"><span data-stu-id="23940-148">**Step 2: Calculate hello number of mappers** - hello value of **m** is equal toohello quotient of total YARN memory divided by hello YARN container size.</span></span> <span data-ttu-id="23940-149">Hello informace o velikosti kontejneru YARN je k dispozici také hello Ambari portálu.</span><span class="sxs-lookup"><span data-stu-id="23940-149">hello YARN container size information is available in hello Ambari portal as well.</span></span> <span data-ttu-id="23940-150">Přejděte tooYARN a zobrazení hello konfigurací kartě hello velikost YARN kontejner se zobrazí v tomto okně.</span><span class="sxs-lookup"><span data-stu-id="23940-150">Navigate tooYARN and view hello Configs tab. hello YARN container size is displayed in this window.</span></span> <span data-ttu-id="23940-151">Hello rovnice tooarrive v hello počet mappers (**m**) je</span><span class="sxs-lookup"><span data-stu-id="23940-151">hello equation tooarrive at hello number of mappers (**m**) is</span></span>

        m = (number of nodes * YARN memory for each node) / YARN container size

<span data-ttu-id="23940-152">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="23940-152">**Example**</span></span>

<span data-ttu-id="23940-153">Předpokládejme, že máte 4 D14v2s uzly v clusteru hello a pokoušíte tootransfer 10TB dat z 10 jiné složky.</span><span class="sxs-lookup"><span data-stu-id="23940-153">Let’s assume that you have a 4 D14v2s nodes in hello cluster and you are trying tootransfer 10TB of data from 10 different folders.</span></span> <span data-ttu-id="23940-154">Každý hello složek obsahovat různých objemy dat a velikosti souborů hello v rámci každé složky se liší.</span><span class="sxs-lookup"><span data-stu-id="23940-154">Each of hello folders contain varying amounts of data and hello file sizes within each folder are different.</span></span>

* <span data-ttu-id="23940-155">Celkové paměti YARN - z hello Ambari portál zjistíte, že hello YARN paměti je 96GB pro D14 uzel.</span><span class="sxs-lookup"><span data-stu-id="23940-155">Total YARN memory - From hello Ambari portal you determine that hello YARN memory is 96GB for a D14 node.</span></span> <span data-ttu-id="23940-156">Ano je celková paměť YARN u clusteru se 4 uzly:</span><span class="sxs-lookup"><span data-stu-id="23940-156">So, total YARN memory for 4 node cluster is:</span></span> 

        YARN memory = 4 * 96GB = 384GB

* <span data-ttu-id="23940-157">Počet mappers - z hello Ambari portál zjistíte, že tento hello YARN kontejneru velikost je 3072 pro uzel clusteru s podporou D14.</span><span class="sxs-lookup"><span data-stu-id="23940-157">Number of mappers - From hello Ambari portal you determine that hello YARN container size is 3072 for a D14 cluster node.</span></span> <span data-ttu-id="23940-158">Ano počet mappers je:</span><span class="sxs-lookup"><span data-stu-id="23940-158">So, number of mappers is:</span></span>

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

<span data-ttu-id="23940-159">Pokud jiné aplikace používají paměť, potom můžete pomocí tooonly část paměti YARN váš cluster pro DistCp.</span><span class="sxs-lookup"><span data-stu-id="23940-159">If other applications are using memory, then you can choose tooonly use a portion of your cluster’s YARN memory for DistCp.</span></span>

### <a name="copying-large-datasets"></a><span data-ttu-id="23940-160">Kopírování rozsáhlých datových sad</span><span class="sxs-lookup"><span data-stu-id="23940-160">Copying large datasets</span></span>

<span data-ttu-id="23940-161">Při přesunutí hello velikost toobe hello datové sady je moc velké (například > 1TB) nebo pokud máte mnoho různých složek, měli byste zvážit použití více DistCp úloh.</span><span class="sxs-lookup"><span data-stu-id="23940-161">When hello size of hello dataset toobe moved is very large (e.g. >1TB) or if you have many different folders, you should consider using multiple DistCp jobs.</span></span> <span data-ttu-id="23940-162">Je pravděpodobné žádné výkonnější, ale bude snažte se úlohy hello, takže pokud se všechny úlohy nezdaří, bude stačí toorestart této konkrétní úlohy a spíš než celý projekt hello.</span><span class="sxs-lookup"><span data-stu-id="23940-162">There is likely no performance gain, but it will spread out hello jobs so that if any job fails, you will only need toorestart that specific job rather than hello entire job.</span></span>

### <a name="limitations"></a><span data-ttu-id="23940-163">Omezení</span><span class="sxs-lookup"><span data-stu-id="23940-163">Limitations</span></span>

* <span data-ttu-id="23940-164">DistCp pokusí mappers toocreate, které se podobají velikost toooptimize výkonu.</span><span class="sxs-lookup"><span data-stu-id="23940-164">DistCp tries toocreate mappers that are similar in size toooptimize performance.</span></span> <span data-ttu-id="23940-165">Zvýšení hello počet mappers nemusí vždy zvýšit výkon.</span><span class="sxs-lookup"><span data-stu-id="23940-165">Increasing hello number of mappers may not always increase performance.</span></span>

* <span data-ttu-id="23940-166">DistCp je omezená tooonly jeden mapper na soubor.</span><span class="sxs-lookup"><span data-stu-id="23940-166">DistCp is limited tooonly one mapper per file.</span></span> <span data-ttu-id="23940-167">Proto by neměl mít další mappers, než kolik máte soubory.</span><span class="sxs-lookup"><span data-stu-id="23940-167">Therefore, you should not have more mappers than you have files.</span></span> <span data-ttu-id="23940-168">Vzhledem k tomu, že DistCp lze přiřadit pouze jeden mapper tooa souboru, toto nastavení omezuje hello množství souběžnosti, kterou lze použít toocopy velkých souborů.</span><span class="sxs-lookup"><span data-stu-id="23940-168">Since DistCp can only assign one mapper tooa file, this limits hello amount of concurrency that can be used toocopy large files.</span></span>

* <span data-ttu-id="23940-169">Pokud máte malý počet velkých souborů, pak je třeba rozdělit do 256MB souboru bloky toogive jste další potenciální souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="23940-169">If you have a small number of large files, then you should split them into 256MB file chunks toogive you more potential concurrency.</span></span> 
 
* <span data-ttu-id="23940-170">Pokud kopírujete z účtu úložiště objektů Blob Azure, může vaše úlohu kopírování omezeny na straně úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="23940-170">If you are copying from an Azure Blob Storage account, your copy job may be throttled on hello blob storage side.</span></span> <span data-ttu-id="23940-171">To způsobí snížení výkonu hello kopírování úlohy.</span><span class="sxs-lookup"><span data-stu-id="23940-171">This will degrade hello performance of your copy job.</span></span> <span data-ttu-id="23940-172">toolearn Další informace o omezení hello Azure Blob Storage, najdete v části omezení Azure Storage v [předplatného Azure a omezení služby](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="23940-172">toolearn more about hello limits of Azure Blob Storage, see Azure Storage limits at [Azure subscription and service limits](../azure-subscription-service-limits.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="23940-173">Viz také</span><span class="sxs-lookup"><span data-stu-id="23940-173">See also</span></span>
* [<span data-ttu-id="23940-174">Kopírování dat z Azure úložiště objektů BLOB tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="23940-174">Copy data from Azure Storage Blobs tooData Lake Store</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
* [<span data-ttu-id="23940-175">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="23940-175">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="23940-176">Použití Azure Data Lake Analytics se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="23940-176">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="23940-177">Použití Azure HDInsight se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="23940-177">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
