---
title: "aaaQuery data z HDFS kompatibilního úložiště Azure - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak tooquery dat z úložiště Azure a Azure Data Lake Store toostore výsledky analýzy."
keywords: blob storage, hdfs, structured data, unstructured data, data lake store, Hadoop input, Hadoop output, hadoop storage, hdfs input, hdfs output, hdfs storage, wasb azure
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1d2e65f2-16de-449e-915f-3ffbc230f815
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: jgao
ms.openlocfilehash: 1032d60424b65e3c0c54a25c7c15970b017a788f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a><span data-ttu-id="42c3c-104">Použití úložiště Azure s clustery Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="42c3c-104">Use Azure storage with Azure HDInsight clusters</span></span>

<span data-ttu-id="42c3c-105">tooanalyze data v clusteru HDInsight, můžete uložit data hello buď do úložiště Azure, Azure Data Lake Store nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="42c3c-105">tooanalyze data in HDInsight cluster, you can store hello data either in Azure Storage, Azure Data Lake Store, or both.</span></span> <span data-ttu-id="42c3c-106">Obě možnosti úložiště povolit toosafely odstranění clusterů HDInsight, jsou používány pro výpočty, aniž by se ztratila uživatelská data.</span><span class="sxs-lookup"><span data-stu-id="42c3c-106">Both storage options enable you toosafely delete HDInsight clusters that are used for computation without losing user data.</span></span>

<span data-ttu-id="42c3c-107">Hadoop podporuje hodnoty z hello výchozí systém souborů.</span><span class="sxs-lookup"><span data-stu-id="42c3c-107">Hadoop supports a notion of hello default file system.</span></span> <span data-ttu-id="42c3c-108">Hello výchozí systém souborů znamená výchozí schéma a autoritu.</span><span class="sxs-lookup"><span data-stu-id="42c3c-108">hello default file system implies a default scheme and authority.</span></span> <span data-ttu-id="42c3c-109">Lze také použít tooresolve relativní cesty.</span><span class="sxs-lookup"><span data-stu-id="42c3c-109">It can also be used tooresolve relative paths.</span></span> <span data-ttu-id="42c3c-110">Během procesu vytváření clusteru HDInsight hello zadáte kontejner objektů blob v Azure Storage jako hello výchozí systém souborů, nebo s HDInsight 3.5, můžete vybrat Azure Storage nebo Azure Data Lake Store jako hello výchozí systém souborů s několika výjimkami.</span><span class="sxs-lookup"><span data-stu-id="42c3c-110">During hello HDInsight cluster creation process, you can specify a blob container in Azure Storage as hello default file system, or with HDInsight 3.5, you can select either Azure Storage or Azure Data Lake Store as hello default files system with a few exceptions.</span></span> <span data-ttu-id="42c3c-111">Hello možnosti použití Data Lake Store jako výchozí hello a propojené úložiště, najdete v části [dostupnosti pro HDInsight cluster](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters).</span><span class="sxs-lookup"><span data-stu-id="42c3c-111">For hello supportability of using Data Lake Store as both hello default and linked storage, see [Availabilities for HDInsight cluster](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters).</span></span>

<span data-ttu-id="42c3c-112">V tomto článku se dozvíte, jak služba Azure Storage pracuje s clustery HDInsight.</span><span class="sxs-lookup"><span data-stu-id="42c3c-112">In this article, you learn how Azure Storage works with HDInsight clusters.</span></span> <span data-ttu-id="42c3c-113">toolearn jak funguje Data Lake Store s clustery HDInsight, najdete v části [clusterů pomocí Azure Data Lake Store s Azure HDInsight](hdinsight-hadoop-use-data-lake-store.md).</span><span class="sxs-lookup"><span data-stu-id="42c3c-113">toolearn how Data Lake Store works with HDInsight clusters, see [Use Azure Data Lake Store with Azure HDInsight clusters](hdinsight-hadoop-use-data-lake-store.md).</span></span> <span data-ttu-id="42c3c-114">Další informace o vytvoření clusteru HDInsight najdete v tématu [Vytváření clusterů Hadoop ve službě HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="42c3c-114">For more information about creating an HDInsight cluster, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="42c3c-115">Azure Storage je robustní řešení úložiště pro obecné účely, které se jednoduše integruje se službou HDInsight.</span><span class="sxs-lookup"><span data-stu-id="42c3c-115">Azure storage is a robust, general-purpose storage solution that integrates seamlessly with HDInsight.</span></span> <span data-ttu-id="42c3c-116">HDInsight můžete použít kontejner objektů blob v Azure Storage jako hello výchozí systém souborů pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="42c3c-116">HDInsight can use a blob container in Azure Storage as hello default file system for hello cluster.</span></span> <span data-ttu-id="42c3c-117">Pomocí rozhraní Hadoop system (HDFS) souborů DFS hello úplnou sadu součásti v HDInsight můžete pracovat přímo strukturovaných nebo nestrukturovaných dat uložené jako objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="42c3c-117">Through a Hadoop distributed file system (HDFS) interface, hello full set of components in HDInsight can operate directly on structured or unstructured data stored as blobs.</span></span>

> [!WARNING]
> <span data-ttu-id="42c3c-118">Při vytváření účtu služby Azure Storage je k dispozici několik možností.</span><span class="sxs-lookup"><span data-stu-id="42c3c-118">There are several options available when creating an Azure Storage account.</span></span> <span data-ttu-id="42c3c-119">Hello následující tabulka obsahuje informace o jaké možnosti jsou podporovány v prostředí HDInsight:</span><span class="sxs-lookup"><span data-stu-id="42c3c-119">hello following table provides information on what options are supported with HDInsight:</span></span>
> 
> | <span data-ttu-id="42c3c-120">Typ účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="42c3c-120">Storage account type</span></span> | <span data-ttu-id="42c3c-121">Úroveň úložiště</span><span class="sxs-lookup"><span data-stu-id="42c3c-121">Storage tier</span></span> | <span data-ttu-id="42c3c-122">Podporováno se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="42c3c-122">Supported with HDInsight</span></span> |
> | ------- | ------- | ------- |
> | <span data-ttu-id="42c3c-123">Účet služby Storage pro obecné účely</span><span class="sxs-lookup"><span data-stu-id="42c3c-123">General-purpose Storage Account</span></span> | <span data-ttu-id="42c3c-124">Standard</span><span class="sxs-lookup"><span data-stu-id="42c3c-124">Standard</span></span> | <span data-ttu-id="42c3c-125">__Ano__</span><span class="sxs-lookup"><span data-stu-id="42c3c-125">__Yes__</span></span> |
> | &nbsp; | <span data-ttu-id="42c3c-126">Premium</span><span class="sxs-lookup"><span data-stu-id="42c3c-126">Premium</span></span> | <span data-ttu-id="42c3c-127">Ne</span><span class="sxs-lookup"><span data-stu-id="42c3c-127">No</span></span> |
> | <span data-ttu-id="42c3c-128">Účet služby Blob Storage</span><span class="sxs-lookup"><span data-stu-id="42c3c-128">Blob Storage Account</span></span> | <span data-ttu-id="42c3c-129">Hot</span><span class="sxs-lookup"><span data-stu-id="42c3c-129">Hot</span></span> | <span data-ttu-id="42c3c-130">Ne</span><span class="sxs-lookup"><span data-stu-id="42c3c-130">No</span></span> |
> | &nbsp; | <span data-ttu-id="42c3c-131">Cool</span><span class="sxs-lookup"><span data-stu-id="42c3c-131">Cool</span></span> | <span data-ttu-id="42c3c-132">Ne</span><span class="sxs-lookup"><span data-stu-id="42c3c-132">No</span></span> |

<span data-ttu-id="42c3c-133">Nedoporučujeme používat k ukládání firemních dat hello výchozí kontejner objektu blob.</span><span class="sxs-lookup"><span data-stu-id="42c3c-133">We do not recommend that you use hello default blob container for storing business data.</span></span> <span data-ttu-id="42c3c-134">Odstranění výchozího kontejneru blob hello po každé použití tooreduce náklady na úložiště je vhodné.</span><span class="sxs-lookup"><span data-stu-id="42c3c-134">Deleting hello default blob container after each use tooreduce storage cost is a good practice.</span></span> <span data-ttu-id="42c3c-135">Všimněte si, že hello výchozí kontejner obsahuje aplikace a systému protokoly.</span><span class="sxs-lookup"><span data-stu-id="42c3c-135">Note that hello default container contains application and system logs.</span></span> <span data-ttu-id="42c3c-136">Ujistěte se, že tooretrieve hello protokoly před odstraněním hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="42c3c-136">Make sure tooretrieve hello logs before deleting hello container.</span></span>

<span data-ttu-id="42c3c-137">Sdílení jednoho kontejneru objektů blob pro několik clusterů se nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="42c3c-137">Sharing one blob container for multiple clusters is not supported.</span></span>

## <a name="hdinsight-storage-architecture"></a><span data-ttu-id="42c3c-138">Architektura úložiště HDInsight</span><span class="sxs-lookup"><span data-stu-id="42c3c-138">HDInsight storage architecture</span></span>
<span data-ttu-id="42c3c-139">Hello následující diagram představuje abstraktní zobrazení Dobrý den architektura úložiště HDInsight pomocí Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="42c3c-139">hello following diagram provides an abstract view of hello HDInsight storage architecture of using Azure Storage:</span></span>

<span data-ttu-id="42c3c-140">![Clusterů systému Hadoop pomocí rozhraní API HDFS tooaccess hello a ukládání strukturovaných a nestrukturovaných dat v úložišti objektů Blob. ] (./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "Architektura úložiště HDInsight")</span><span class="sxs-lookup"><span data-stu-id="42c3c-140">![Hadoop clusters use hello HDFS API tooaccess and store structured and unstructured data in Blob storage.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Storage Architecture")</span></span>

<span data-ttu-id="42c3c-141">HDInsight poskytuje, aby byl systém souborů toohello distribuované přístup, který je místně připojen toohello výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="42c3c-141">HDInsight provides access toohello distributed file system that is locally attached toohello compute nodes.</span></span> <span data-ttu-id="42c3c-142">V tomto systému souborů je přístupný pomocí hello plně kvalifikovaný identifikátor URI, třeba:</span><span class="sxs-lookup"><span data-stu-id="42c3c-142">This file system can be accessed by using hello fully qualified URI, for example:</span></span>

    hdfs://<namenodehost>/<path>

<span data-ttu-id="42c3c-143">Kromě toho HDInsight umožňuje tooaccess data, která je uložená ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="42c3c-143">In addition, HDInsight allows you tooaccess data that is stored in Azure Storage.</span></span> <span data-ttu-id="42c3c-144">Hello syntaxe je:</span><span class="sxs-lookup"><span data-stu-id="42c3c-144">hello syntax is:</span></span>

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

<span data-ttu-id="42c3c-145">Při použití účtu Azure Storage s clustery HDInsight je potřeba zvážit tyto aspekty.</span><span class="sxs-lookup"><span data-stu-id="42c3c-145">Here are some considerations when using Azure Storage account with HDInsight clusters.</span></span>

* <span data-ttu-id="42c3c-146">**Kontejnery v účtech úložiště hello, které jsou připojené tooa clusteru:** protože hello název účtu a klíč jsou přidružené hello clusteru během vytváření, máte plný přístup toohello objektů BLOB v těchto kontejnerech.</span><span class="sxs-lookup"><span data-stu-id="42c3c-146">**Containers in hello storage accounts that are connected tooa cluster:** Because hello account name and key are associated with hello cluster during creation, you have full access toohello blobs in those containers.</span></span>

* <span data-ttu-id="42c3c-147">**Veřejné kontejnery nebo veřejné objekty BLOB v účtech úložiště, které nejsou připojené tooa clusteru:** máte oprávnění jen pro čtení toohello objekty BLOB v kontejnerech hello.</span><span class="sxs-lookup"><span data-stu-id="42c3c-147">**Public containers or public blobs in storage accounts that are NOT connected tooa cluster:** You have read-only permission toohello blobs in hello containers.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="42c3c-148">Veřejné kontejnery umožňují tooget seznam všech objektů BLOB, které jsou k dispozici v tomto kontejneru a získat metadata kontejneru.</span><span class="sxs-lookup"><span data-stu-id="42c3c-148">Public containers allow you tooget a list of all blobs that are available in that container and get container metadata.</span></span> <span data-ttu-id="42c3c-149">Veřejné objekty BLOB umožní objekty BLOB hello tooaccess pouze v případě, že znáte přesnou adresu URL hello.</span><span class="sxs-lookup"><span data-stu-id="42c3c-149">Public blobs allow you tooaccess hello blobs only if you know hello exact URL.</span></span> <span data-ttu-id="42c3c-150">Další informace najdete v tématu <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">omezit přístup toocontainers a objekty BLOB</a>.</span><span class="sxs-lookup"><span data-stu-id="42c3c-150">For more information, see <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Restrict access toocontainers and blobs</a>.</span></span>
  > 
  > 
* <span data-ttu-id="42c3c-151">**Privátní kontejnery v účtech úložiště, které nejsou připojené tooa clusteru:** hello objekty BLOB v kontejnerech hello nemáte přístup, dokud nedefinujete účet úložiště hello při odesílání úlohy WebHCat hello.</span><span class="sxs-lookup"><span data-stu-id="42c3c-151">**Private containers in storage accounts that are NOT connected tooa cluster:** You can't access hello blobs in hello containers unless you define hello storage account when you submit hello WebHCat jobs.</span></span> <span data-ttu-id="42c3c-152">To se vysvětluje dále v tomhle článku.</span><span class="sxs-lookup"><span data-stu-id="42c3c-152">This is explained later in this article.</span></span>

<span data-ttu-id="42c3c-153">Hello účty úložiště, které jsou definovány v procesu vytváření hello a jejich klíče jsou uloženy v %HADOOP_HOME%/conf/core-site.xml na uzlech clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="42c3c-153">hello storage accounts that are defined in hello creation process and their keys are stored in %HADOOP_HOME%/conf/core-site.xml on hello cluster nodes.</span></span> <span data-ttu-id="42c3c-154">Hello výchozím chováním služby HDInsight je účty úložiště hello toouse definované v souboru core-site.xml hello.</span><span class="sxs-lookup"><span data-stu-id="42c3c-154">hello default behavior of HDInsight is toouse hello storage accounts defined in hello core-site.xml file.</span></span> <span data-ttu-id="42c3c-155">Toto nastavení můžete upravit pomocí [Ambari](./hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="42c3c-155">You can modify this setting using [Ambari](./hdinsight-hadoop-manage-ambari.md)</span></span>

<span data-ttu-id="42c3c-156">Více úloh WebHCat, včetně Hive, MapReduce, streamování Hadoop a Pig, může obsahovat popis účtů úložiště a spojených metadat.</span><span class="sxs-lookup"><span data-stu-id="42c3c-156">Multiple WebHCat jobs, including Hive, MapReduce, Hadoop streaming, and Pig, can carry a description of storage accounts and metadata with them.</span></span> <span data-ttu-id="42c3c-157">(To aktuálně funguje pro Pig s účty úložiště, ale ne pro metadata.) Více informací najdete v části [Použití clusteru HDInsight s alternativními účty úložiště a metaúložišti](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).</span><span class="sxs-lookup"><span data-stu-id="42c3c-157">(This currently works for Pig with storage accounts, but not for metadata.) For more information, see [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).</span></span>

<span data-ttu-id="42c3c-158">Objekty blob lze použít pro strukturovaná i nestrukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="42c3c-158">Blobs can be used for structured and unstructured data.</span></span> <span data-ttu-id="42c3c-159">Kontejnery objektů blob ukládají data jako páry klíč/hodnota a neexistuje žádná hierarchie adresářů.</span><span class="sxs-lookup"><span data-stu-id="42c3c-159">Blob containers store data as key/value pairs, and there is no directory hierarchy.</span></span> <span data-ttu-id="42c3c-160">Ale hello lomítko (/) lze použít v rámci hello toomake název klíče se zobrazí, jako kdyby je soubor uložit do do struktury adresářů.</span><span class="sxs-lookup"><span data-stu-id="42c3c-160">However hello slash character ( / ) can be used within hello key name toomake it appear as if a file is stored within a directory structure.</span></span> <span data-ttu-id="42c3c-161">Klíč k objektu blob může být například *input/log1.txt*.</span><span class="sxs-lookup"><span data-stu-id="42c3c-161">For example, a blob's key may be *input/log1.txt*.</span></span> <span data-ttu-id="42c3c-162">Žádný skutečný *vstupní* adresář existuje, ale z důvodu přítomnosti toohello hello lomítku v názvu klíče hello, má hello vzhled cestu k souboru.</span><span class="sxs-lookup"><span data-stu-id="42c3c-162">No actual *input* directory exists, but due toohello presence of hello slash character in hello key name, it has hello appearance of a file path.</span></span>

## <span data-ttu-id="42c3c-163"><a id="benefits"></a>Výhody služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="42c3c-163"><a id="benefits"></a>Benefits of Azure Storage</span></span>
<span data-ttu-id="42c3c-164">Hello znamenalo, že způsob hello hello výpočetní clustery jsou vytvořeny zavřít toohello prostředků účtu úložiště uvnitř oblasti Azure, kde vysokorychlostní síť hello umožňuje hello zmírnit výkonová náročnost společně vyhledáním výpočetní clustery a prostředky úložiště efektivní pro výpočetní uzly hello tooaccess hello data do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="42c3c-164">hello implied performance cost of not co-locating compute clusters and storage resources is mitigated by hello way hello compute clusters are created close toohello storage account resources inside hello Azure region, where hello high-speed network makes it efficient for hello compute nodes tooaccess hello data inside Azure storage.</span></span>

<span data-ttu-id="42c3c-165">Existuje více výhod spojených s ukládáním hello dat v úložišti Azure místo HDFS:</span><span class="sxs-lookup"><span data-stu-id="42c3c-165">There are several benefits associated with storing hello data in Azure storage instead of HDFS:</span></span>

* <span data-ttu-id="42c3c-166">**Opakované použití dat a sdílení:** hello data v HDFS se nachází uvnitř výpočetního clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="42c3c-166">**Data reuse and sharing:** hello data in HDFS is located inside hello compute cluster.</span></span> <span data-ttu-id="42c3c-167">Pouze hello aplikace, které mají přístup toohello výpočetní cluster může používat hello data pomocí rozhraní API HDFS.</span><span class="sxs-lookup"><span data-stu-id="42c3c-167">Only hello applications that have access toohello compute cluster can use hello data by using HDFS APIs.</span></span> <span data-ttu-id="42c3c-168">Hello data v úložišti Azure je přístupná prostřednictvím hello rozhraní API HDFS nebo prostřednictvím hello [rozhraní API REST úložiště objektů Blob][blob-storage-restAPI].</span><span class="sxs-lookup"><span data-stu-id="42c3c-168">hello data in Azure storage can be accessed either through hello HDFS APIs or through hello [Blob Storage REST APIs][blob-storage-restAPI].</span></span> <span data-ttu-id="42c3c-169">Proto s větším počtem aplikací (včetně jiných clusterů HDInsight) a nástroje může být použité tooproduce a využívají hello data.</span><span class="sxs-lookup"><span data-stu-id="42c3c-169">Thus, a larger set of applications (including other HDInsight clusters) and tools can be used tooproduce and consume hello data.</span></span>
* <span data-ttu-id="42c3c-170">**Archivace dat:** ukládání dat v úložišti Azure umožňuje clustery HDInsight hello používá pro výpočet toobe bezpečně odstranit, aniž by se ztratila uživatelská data.</span><span class="sxs-lookup"><span data-stu-id="42c3c-170">**Data archiving:** Storing data in Azure storage enables hello HDInsight clusters used for computation toobe safely deleted without losing user data.</span></span>
* <span data-ttu-id="42c3c-171">**Náklady na úložiště dat:** ukládání dat v systému souborů DFS pro hello dlouhodobého hlediska dražší než ukládání hello dat v úložišti Azure, protože hello náklady na výpočetním clusteru je vyšší než hello náklady na úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="42c3c-171">**Data storage cost:** Storing data in DFS for hello long term is more costly than storing hello data in Azure storage because hello cost of a compute cluster is higher than hello cost of Azure storage.</span></span> <span data-ttu-id="42c3c-172">Kromě toho protože hello dat nemá toobe znovu pro každou generaci výpočetních clusterů, můžete se také ukládání dat načítání náklady.</span><span class="sxs-lookup"><span data-stu-id="42c3c-172">In addition, because hello data does not have toobe reloaded for every compute cluster generation, you are also saving data loading costs.</span></span>
* <span data-ttu-id="42c3c-173">**Elastické škálování:** i když HDFS poskytuje rozšířené souborové systému, hello škála se určuje podle hello počet uzlů, které vytvoříte pro svůj cluster.</span><span class="sxs-lookup"><span data-stu-id="42c3c-173">**Elastic scale-out:** Although HDFS provides you with a scaled-out file system, hello scale is determined by hello number of nodes that you create for your cluster.</span></span> <span data-ttu-id="42c3c-174">Změna měřítka hello se může stát složitější než využití elastického hello škálování, které jste získali automaticky v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="42c3c-174">Changing hello scale can become a more complicated process than relying on hello elastic scaling capabilities that you get automatically in Azure storage.</span></span>
* <span data-ttu-id="42c3c-175">**Geografická replikace:** Službu Azure Storage je možné geograficky replikovat.</span><span class="sxs-lookup"><span data-stu-id="42c3c-175">**Geo-replication:** Your Azure storage can be geo-replicated.</span></span> <span data-ttu-id="42c3c-176">I když to vám dává geografické obnovení a redundanci dat, převzetí služeb při selhání toohello geograficky replikovaného umístění vážně ovlivňuje výkon a mohou být účtovány další poplatky.</span><span class="sxs-lookup"><span data-stu-id="42c3c-176">Although this gives you geographic recovery and data redundancy, a failover toohello geo-replicated location severely impacts your performance, and it may incur additional costs.</span></span> <span data-ttu-id="42c3c-177">Doporučujeme proto toochoose hello geografickou replikaci dobře a pouze v případě, že hodnota hello hello dat je vhodné hello dalších poplatků.</span><span class="sxs-lookup"><span data-stu-id="42c3c-177">So our recommendation is toochoose hello geo-replication wisely and only if hello value of hello data is worth hello additional cost.</span></span>

<span data-ttu-id="42c3c-178">Některé úlohy a balíčky MapReduce můžou vytvořit mezilehlé výsledky, že nechcete, aby skutečně toostore v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="42c3c-178">Certain MapReduce jobs and packages may create intermediate results that you don't really want toostore in Azure storage.</span></span> <span data-ttu-id="42c3c-179">V tomto případě můžete vybrat, zda toostore hello data v hello místní HDFS.</span><span class="sxs-lookup"><span data-stu-id="42c3c-179">In that case, you can elect toostore hello data in hello local HDFS.</span></span> <span data-ttu-id="42c3c-180">Ve skutečnosti služba HDInsight používá DFS pro některé z těchto mezilehlých výsledků v úlohách Hive a jiných procesech.</span><span class="sxs-lookup"><span data-stu-id="42c3c-180">In fact, HDInsight uses DFS for several of these intermediate results in Hive jobs and other processes.</span></span>

> [!NOTE]
> <span data-ttu-id="42c3c-181">Většina příkazů HDFS (například <b>ls</b>, <b>copyFromLocal</b> a <b>mkdir</b>) bude i nadále fungovat podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="42c3c-181">Most HDFS commands (for example, <b>ls</b>, <b>copyFromLocal</b> and <b>mkdir</b>) still work as expected.</span></span> <span data-ttu-id="42c3c-182">Pouze hello příkazy, které jsou specifické toohello nativní implementaci HDFS (což je odkazované tooas systému souborů DFS), například <b>fschk</b> a <b>dfsadmin</b>, zobrazit různé chování v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="42c3c-182">Only hello commands that are specific toohello native HDFS implementation (which is referred tooas DFS), such as <b>fschk</b> and <b>dfsadmin</b>, show different behavior in Azure storage.</span></span>
> 
> 

## <a name="create-blob-containers"></a><span data-ttu-id="42c3c-183">Vytvoření kontejnerů objektů Blob</span><span class="sxs-lookup"><span data-stu-id="42c3c-183">Create Blob containers</span></span>
<span data-ttu-id="42c3c-184">toouse objekty BLOB, nejdřív vytvoříte [účet úložiště Azure][azure-storage-create].</span><span class="sxs-lookup"><span data-stu-id="42c3c-184">toouse blobs, you first create an [Azure Storage account][azure-storage-create].</span></span> <span data-ttu-id="42c3c-185">Jako součást tohoto postupu zadejte oblast Azure, kde se má vytvořit účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="42c3c-185">As part of this, you specify an Azure region where hello storage account is created.</span></span> <span data-ttu-id="42c3c-186">Hello clusteru a účet úložiště hello musí být uloženy ve hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="42c3c-186">hello cluster and hello storage account must be hosted in hello same region.</span></span> <span data-ttu-id="42c3c-187">databáze serveru SQL metaúložiště Hive Hello a metaúložiště Oozie databáze musí být také umístěny v systému SQL Server hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="42c3c-187">hello Hive metastore SQL Server database and Oozie metastore SQL Server database must also be located in hello same region.</span></span>

<span data-ttu-id="42c3c-188">Bez ohledu na jeho žije, patří každý objekt blob, které vytvoříte tooa kontejneru v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="42c3c-188">Wherever it lives, each blob you create belongs tooa container in your Azure Storage account.</span></span> <span data-ttu-id="42c3c-189">Tento kontejner může být existující objekt blob, který se vytvořil mimo HDInsight, nebo to může být kontejner, který se vytvořil pro cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="42c3c-189">This container may be an existing blob that was created outside of HDInsight, or it may be a container that is created for an HDInsight cluster.</span></span>

<span data-ttu-id="42c3c-190">Hello výchozí kontejner objektu Blob ukládá informace o specifických pro cluster například historie úlohy a protokoly.</span><span class="sxs-lookup"><span data-stu-id="42c3c-190">hello default Blob container stores cluster-specific information such as job history and logs.</span></span> <span data-ttu-id="42c3c-191">Výchozí kontejner objektu Blob nesdílejte s více clustery služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="42c3c-191">Don't share a default Blob container with multiple HDInsight clusters.</span></span> <span data-ttu-id="42c3c-192">Může dojít k poškození historie úlohy.</span><span class="sxs-lookup"><span data-stu-id="42c3c-192">This might corrupt job history.</span></span> <span data-ttu-id="42c3c-193">Doporučujeme toouse jiný kontejner pro každý cluster a umístit sdílená data na propojený účet úložiště zadaný v nasazení všech příslušných clusterů, nikoli hello výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="42c3c-193">It is recommended toouse a different container for each cluster and put shared data on a linked storage account specified in deployment of all relevant clusters rather than hello default storage account.</span></span> <span data-ttu-id="42c3c-194">Další informace o konfiguraci propojených účtů úložiště najdete v tématu [Tvorba clusterů HDInsight][hdinsight-creation].</span><span class="sxs-lookup"><span data-stu-id="42c3c-194">For more information on configuring linked storage accounts, see [Create HDInsight clusters][hdinsight-creation].</span></span> <span data-ttu-id="42c3c-195">Ale můžete znovu použít výchozí kontejner úložiště po odstranění původního clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="42c3c-195">However you can reuse a default storage container after hello original HDInsight cluster has been deleted.</span></span> <span data-ttu-id="42c3c-196">Pro clustery HBase můžete zachovat hello schématu tabulky HBase a data vytvořením nového clusteru HBase pomocí hello výchozí kontejner objektu blob používaný clusterem HBase, který byl odstraněn.</span><span class="sxs-lookup"><span data-stu-id="42c3c-196">For HBase clusters, you can actually retain hello HBase table schema and data by creating a new HBase cluster using hello default blob container that is used by an HBase cluster that has been deleted.</span></span>

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

### <a name="use-hello-azure-portal"></a><span data-ttu-id="42c3c-197">Hello použití portálu Azure</span><span class="sxs-lookup"><span data-stu-id="42c3c-197">Use hello Azure portal</span></span>
<span data-ttu-id="42c3c-198">Při vytváření clusteru služby HDInsight z hello portál, máte hello možnosti (jak je znázorněno níže) tooprovide hello podrobnosti o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="42c3c-198">When creating an HDInsight cluster from hello Portal, you have hello options (as shown below) tooprovide hello storage account details.</span></span> <span data-ttu-id="42c3c-199">Můžete také, zda má účet další úložiště přidruženého k hello clusteru a pokud ano, vybírat Data Lake Store nebo jiný objekt blob úložiště Azure jako dodatečné úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="42c3c-199">You can also specify whether you want an additional storage account associated with hello cluster, and if so, choose from Data Lake Store or another Azure Storage blob as hello additional storage.</span></span>

![Zdroj dat pro vytvoření hadoopu HDInsight](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

> [!WARNING]
> <span data-ttu-id="42c3c-201">Použití účtu další úložiště v jiném umístění než hello HDInsight cluster není podporováno.</span><span class="sxs-lookup"><span data-stu-id="42c3c-201">Using an additional storage account in a different location than hello HDInsight cluster is not supported.</span></span>


### <a name="use-azure-powershell"></a><span data-ttu-id="42c3c-202">Použití Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="42c3c-202">Use Azure PowerShell</span></span>
<span data-ttu-id="42c3c-203">Pokud jste [instalace a konfigurace prostředí Azure PowerShell][powershell-install], můžete použít hello následujících z hello prostředí Azure PowerShell výzva toocreate účtu úložiště a kontejneru:</span><span class="sxs-lookup"><span data-stu-id="42c3c-203">If you [installed and configured Azure PowerShell][powershell-install], you can use hello following from hello Azure PowerShell prompt toocreate a storage account and container:</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS 

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

### <a name="use-azure-cli"></a><span data-ttu-id="42c3c-204">Použití Azure CLI</span><span class="sxs-lookup"><span data-stu-id="42c3c-204">Use Azure CLI</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

<span data-ttu-id="42c3c-205">Pokud máte [nainstalováno a nakonfigurováno rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md), hello následující příkaz, může být použité tooa účtu úložiště a kontejneru.</span><span class="sxs-lookup"><span data-stu-id="42c3c-205">If you have [installed and configured hello Azure CLI](../cli-install-nodejs.md), hello following command can be used tooa storage account and container.</span></span>

    azure storage account create <storageaccountname> --type LRS

> [!NOTE]
> <span data-ttu-id="42c3c-206">Hello `--type` parametr určuje, jak se replikují hello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="42c3c-206">hello `--type` parameter indicates how hello storage account is replicated.</span></span> <span data-ttu-id="42c3c-207">Další informace najdete v tématu [Replikace Azure Storage](../storage/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="42c3c-207">For more information, see [Azure Storage Replication](../storage/storage-redundancy.md).</span></span> <span data-ttu-id="42c3c-208">Nepoužívejte ZRS, protože nepodporuje objekt blob stránky, soubor, tabulku ani frontu.</span><span class="sxs-lookup"><span data-stu-id="42c3c-208">Don't use ZRS as ZRS doesn't support page blob, file, table, or queue.</span></span>
> 
> 

<span data-ttu-id="42c3c-209">Jste hello výzvami toospecify zeměpisnou se oblast, která je vytvoření účtu úložiště hello v.</span><span class="sxs-lookup"><span data-stu-id="42c3c-209">You are prompted toospecify hello geographic region that hello storage account is created in.</span></span> <span data-ttu-id="42c3c-210">Měli byste vytvořit účet úložiště hello v hello stejné oblasti, kterou chcete použít k vytvoření clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="42c3c-210">You should create hello storage account in hello same region that you plan on creating your HDInsight cluster.</span></span>

<span data-ttu-id="42c3c-211">Po vytvoření účtu úložiště hello použijte hello následující klíče účtu úložiště hello tooretrieve příkaz:</span><span class="sxs-lookup"><span data-stu-id="42c3c-211">Once hello storage account is created, use hello following command tooretrieve hello storage account keys:</span></span>

    azure storage account keys list <storageaccountname>

<span data-ttu-id="42c3c-212">toocreate kontejner, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="42c3c-212">toocreate a container, use hello following command:</span></span>

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="address-files-in-azure-storage"></a><span data-ttu-id="42c3c-213">Adresování souborů ve službě Azure Storage</span><span class="sxs-lookup"><span data-stu-id="42c3c-213">Address files in Azure storage</span></span>
<span data-ttu-id="42c3c-214">Schéma identifikátoru URI Hello pro přístup k souborům v úložišti Azure z prostředí HDInsight je:</span><span class="sxs-lookup"><span data-stu-id="42c3c-214">hello URI scheme for accessing files in Azure storage from HDInsight is:</span></span>

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>

<span data-ttu-id="42c3c-215">Hello schéma identifikátoru URI poskytuje nezašifrovaný přístup (s hello *wasb:* předponu) a zašifrovaný přístup SSL (s *wasbs*).</span><span class="sxs-lookup"><span data-stu-id="42c3c-215">hello URI scheme provides unencrypted access (with hello *wasb:* prefix) and SSL encrypted access (with *wasbs*).</span></span> <span data-ttu-id="42c3c-216">Doporučujeme používat *wasbs* kdykoli je to možné, i když hello přístupem dat, které je umístěn uvnitř stejné oblasti v Azure.</span><span class="sxs-lookup"><span data-stu-id="42c3c-216">We recommend using *wasbs* wherever possible, even when accessing data that lives inside hello same region in Azure.</span></span>

<span data-ttu-id="42c3c-217">Hello &lt;BlobStorageContainerName&gt; identifikuje název hello hello kontejner objektů blob v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="42c3c-217">hello &lt;BlobStorageContainerName&gt; identifies hello name of hello blob container in Azure storage.</span></span>
<span data-ttu-id="42c3c-218">Hello &lt;StorageAccountName&gt; identifikuje název účtu úložiště Azure hello.</span><span class="sxs-lookup"><span data-stu-id="42c3c-218">hello &lt;StorageAccountName&gt; identifies hello Azure Storage account name.</span></span> <span data-ttu-id="42c3c-219">Vyžaduje se plně kvalifikovaný název domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="42c3c-219">A fully qualified domain name (FQDN) is required.</span></span>

<span data-ttu-id="42c3c-220">Pokud ani &lt;BlobStorageContainerName&gt; ani &lt;StorageAccountName&gt; byl zadán, hello výchozí systém souborů se používá.</span><span class="sxs-lookup"><span data-stu-id="42c3c-220">If neither &lt;BlobStorageContainerName&gt; nor &lt;StorageAccountName&gt; has been specified, hello default file system is used.</span></span> <span data-ttu-id="42c3c-221">U souborů hello na hello výchozí systém souborů můžete použít relativní cestu nebo absolutní cesta.</span><span class="sxs-lookup"><span data-stu-id="42c3c-221">For hello files on hello default file system, you can use a relative path or an absolute path.</span></span> <span data-ttu-id="42c3c-222">Například hello *hadoop-mapreduce-examples.jar* soubor, který se dodává s clustery HDInsight lze odkazované tooby pomocí jedné z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="42c3c-222">For example, hello *hadoop-mapreduce-examples.jar* file that comes with HDInsight clusters can be referred tooby using one of hello following:</span></span>

    wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasb:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> <span data-ttu-id="42c3c-223">Název souboru Hello je <i>hadoop-examples.jar</i> v clusterech HDInsight verze 2.1 a 1.6.</span><span class="sxs-lookup"><span data-stu-id="42c3c-223">hello file name is <i>hadoop-examples.jar</i> in HDInsight versions 2.1 and 1.6 clusters.</span></span>
> 
> 

<span data-ttu-id="42c3c-224">Hello &lt;cesta&gt; je název cesty HDFS hello pro soubor nebo adresář.</span><span class="sxs-lookup"><span data-stu-id="42c3c-224">hello &lt;path&gt; is hello file or directory HDFS path name.</span></span> <span data-ttu-id="42c3c-225">Vzhledem k tomu, že kontejnery ve službě Azure Storage jsou jednoduše úložiště párů klíč-hodnota, neexistuje žádný opravdový hierarchický systém souborů.</span><span class="sxs-lookup"><span data-stu-id="42c3c-225">Because containers in Azure storage are simply key-value stores, there is no true hierarchical file system.</span></span> <span data-ttu-id="42c3c-226">Lomítko ( / ) uvnitř klíče objektu blob se považuje za oddělovač adresářů.</span><span class="sxs-lookup"><span data-stu-id="42c3c-226">A slash character ( / ) inside a blob key is interpreted as a directory separator.</span></span> <span data-ttu-id="42c3c-227">Například název hello objektu blob pro *hadoop-mapreduce-examples.jar* je:</span><span class="sxs-lookup"><span data-stu-id="42c3c-227">For example, hello blob name for *hadoop-mapreduce-examples.jar* is:</span></span>

    example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> <span data-ttu-id="42c3c-228">Při práci s objekty BLOB mimo HDInsight, většina nástrojů nerozpoznávají hello formát WASB a místo toho očekávají základní formát cesty, jako například `example/jars/hadoop-mapreduce-examples.jar`.</span><span class="sxs-lookup"><span data-stu-id="42c3c-228">When working with blobs outside of HDInsight, most utilities do not recognize hello WASB format and instead expect a basic path format, such as `example/jars/hadoop-mapreduce-examples.jar`.</span></span>
> 
> 

## <a name="access-blobs"></a><span data-ttu-id="42c3c-229">Přístup k objektům blob</span><span class="sxs-lookup"><span data-stu-id="42c3c-229">Access blobs</span></span> 


### <span data-ttu-id="42c3c-230"><a name="access-blobs-using-azure-powershell"></a> Použití Azure Powershellu</span><span class="sxs-lookup"><span data-stu-id="42c3c-230"><a name="access-blobs-using-azure-powershell"></a> Use Azure PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="42c3c-231">Hello příkazy v této části jsou ukázkami základních příkladů použití prostředí PowerShell tooaccess data ukládají do objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="42c3c-231">hello commands in this section provide a basic example of using PowerShell tooaccess data stored in blobs.</span></span> <span data-ttu-id="42c3c-232">Více plnohodnotný příklad, který je přizpůsobený pro práci s HDInsight, naleznete v části hello [nástroje HDInsight](https://github.com/Blackmist/hdinsight-tools).</span><span class="sxs-lookup"><span data-stu-id="42c3c-232">For a more full-featured example that is customized for working with HDInsight, see hello [HDInsight Tools](https://github.com/Blackmist/hdinsight-tools).</span></span>
> 
> 

<span data-ttu-id="42c3c-233">Použijte následující příkaz toolist hello týkajících se objektu blob rutiny hello:</span><span class="sxs-lookup"><span data-stu-id="42c3c-233">Use hello following command toolist hello blob-related cmdlets:</span></span>

    Get-Command *blob*

![Seznam rutin prostředí PowerShell týkajících se objektu blob.][img-hdi-powershell-blobcommands]

#### <a name="upload-files"></a><span data-ttu-id="42c3c-235">Nahrání souborů</span><span class="sxs-lookup"><span data-stu-id="42c3c-235">Upload files</span></span>
<span data-ttu-id="42c3c-236">V tématu [nahrát data tooHDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="42c3c-236">See [Upload data tooHDInsight][hdinsight-upload-data].</span></span>

#### <a name="download-files"></a><span data-ttu-id="42c3c-237">Stažení souborů</span><span class="sxs-lookup"><span data-stu-id="42c3c-237">Download files</span></span>
<span data-ttu-id="42c3c-238">Hello následující skript stáhne aktuální složku toohello objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="42c3c-238">hello following script downloads a block blob toohello current folder.</span></span> <span data-ttu-id="42c3c-239">Před spouštění skriptu hello změňte složku tooa hello kde máte oprávnění k zápisu.</span><span class="sxs-lookup"><span data-stu-id="42c3c-239">Before running hello script, change hello directory tooa folder where you have write permissions.</span></span>

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # hello storage account used for hello default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # hello default file system container has hello same name as hello cluster.
    $blob = "example/data/sample.log" # hello name of hello blob toobe downloaded.

    # Use Add-AzureAccount if you haven't connected tooyour Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"

    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

    Write-Host "Download hello blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

    Write-Host "List hello downloaded file ..." -ForegroundColor Green
    cat "./$blob"

<span data-ttu-id="42c3c-240">Poskytuje název skupiny prostředků hello a hello název clusteru, můžete použít následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="42c3c-240">Providing hello resource group name and hello cluster name, you can use hello following code:</span></span>

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # hello name of hello blob toobe downloaded.

    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 

    Write-Host "Download hello blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force


#### <a name="delete-files"></a><span data-ttu-id="42c3c-241">Odstranění souborů</span><span class="sxs-lookup"><span data-stu-id="42c3c-241">Delete files</span></span>
    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

#### <a name="list-files"></a><span data-ttu-id="42c3c-242">Zobrazení souborů</span><span class="sxs-lookup"><span data-stu-id="42c3c-242">List files</span></span>
    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

#### <a name="run-hive-queries-using-an-undefined-storage-account"></a><span data-ttu-id="42c3c-243">Spuštění dotazů Hive pomocí nedefinovaného účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="42c3c-243">Run Hive queries using an undefined storage account</span></span>
<span data-ttu-id="42c3c-244">Tento příklad ukazuje, jak hello toolist složky z účtu úložiště, které není definováno během procesu vytváření.</span><span class="sxs-lookup"><span data-stu-id="42c3c-244">This example shows how toolist a folder from storage account that is not defined during hello creating process.</span></span>
<span data-ttu-id="42c3c-245">$clusterName = “<HDInsightClusterName>“</span><span class="sxs-lookup"><span data-stu-id="42c3c-245">$clusterName = "<HDInsightClusterName>"</span></span>

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasb://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"

### <a name="use-azure-cli"></a><span data-ttu-id="42c3c-246">Použití Azure CLI</span><span class="sxs-lookup"><span data-stu-id="42c3c-246">Use Azure CLI</span></span>
<span data-ttu-id="42c3c-247">Použijte následující příkaz toolist hello týkajících se objektu blob příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="42c3c-247">Use hello following command toolist hello blob-related commands:</span></span>

    azure storage blob

<span data-ttu-id="42c3c-248">**Příklad použití Azure CLI tooupload soubor**</span><span class="sxs-lookup"><span data-stu-id="42c3c-248">**Example of using Azure CLI tooupload a file**</span></span>

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="42c3c-249">**Příklad použití Azure CLI toodownload soubor**</span><span class="sxs-lookup"><span data-stu-id="42c3c-249">**Example of using Azure CLI toodownload a file**</span></span>

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="42c3c-250">**Příklad použití Azure CLI toodelete soubor**</span><span class="sxs-lookup"><span data-stu-id="42c3c-250">**Example of using Azure CLI toodelete a file**</span></span>

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="42c3c-251">**Příklad použití Azure CLI toolist soubory**</span><span class="sxs-lookup"><span data-stu-id="42c3c-251">**Example of using Azure CLI toolist files**</span></span>

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="use-additional-storage-accounts"></a><span data-ttu-id="42c3c-252">Použití dalších účtů úložiště</span><span class="sxs-lookup"><span data-stu-id="42c3c-252">Use additional storage accounts</span></span>

<span data-ttu-id="42c3c-253">Při vytváření clusteru služby HDInsight, zadejte účet služby Azure Storage hello, že chcete tooassociate s ním.</span><span class="sxs-lookup"><span data-stu-id="42c3c-253">While creating an HDInsight cluster, you specify hello Azure Storage account you want tooassociate with it.</span></span> <span data-ttu-id="42c3c-254">Kromě toho toothis účet úložiště, můžete přidat další úložiště účtů z hello stejné předplatné Azure nebo různých předplatných Azure během procesu vytváření hello nebo po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="42c3c-254">In addition toothis storage account, you can add additional storage accounts from hello same Azure subscription or different Azure subscriptions during hello creation process or after a cluster has been created.</span></span> <span data-ttu-id="42c3c-255">Pokyny pro přidání dalších účtů úložiště najdete v tématu [Vytváření clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="42c3c-255">For instructions about adding additional storage accounts, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

> [!WARNING]
> <span data-ttu-id="42c3c-256">Použití účtu další úložiště v jiném umístění než hello HDInsight cluster není podporováno.</span><span class="sxs-lookup"><span data-stu-id="42c3c-256">Using an additional storage account in a different location than hello HDInsight cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42c3c-257">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42c3c-257">Next steps</span></span>
<span data-ttu-id="42c3c-258">V tomto článku jste se naučili jak toouse HDFS kompatibilní úložiště Azure s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="42c3c-258">In this article, you learned how toouse HDFS-compatible Azure storage with HDInsight.</span></span> <span data-ttu-id="42c3c-259">To vám umožní toobuild, škálovatelnou a dlouhodobé, archivace řešení pro získávání dat a používání HDInsight toounlock hello informací uvnitř hello uložené strukturovaných a nestrukturovaných dat.</span><span class="sxs-lookup"><span data-stu-id="42c3c-259">This allows you toobuild scalable, long-term, archiving data acquisition solutions and use HDInsight toounlock hello information inside hello stored structured and unstructured data.</span></span>

<span data-ttu-id="42c3c-260">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="42c3c-260">For more information, see:</span></span>

* <span data-ttu-id="42c3c-261">[Začínáme se službou Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="42c3c-261">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* [<span data-ttu-id="42c3c-262">Začínáme se službou Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="42c3c-262">Get started with Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-get-started-portal.md)
* <span data-ttu-id="42c3c-263">[Nahrání dat tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="42c3c-263">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="42c3c-264">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="42c3c-264">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="42c3c-265">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="42c3c-265">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="42c3c-266">[Použijte Azure Storage sdílené přístupové podpisy toorestrict přístup toodata s HDInsight][hdinsight-use-sas]</span><span class="sxs-lookup"><span data-stu-id="42c3c-266">[Use Azure Storage Shared Access Signatures toorestrict access toodata with HDInsight][hdinsight-use-sas]</span></span>

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]:../storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  
