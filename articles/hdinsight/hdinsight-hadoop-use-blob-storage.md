---
title: "Dotazy na data z úložiště Azure kompatibilního se systémem HDFS – Azure HDInsight| Dokumentace Microsoftu"
description: "Zjistěte, jak zadávat dotazy na data ze služby Azure Storage a Azure Data Lake Store pro ukládání výsledků analýzy."
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
ms.openlocfilehash: a44c2b363f7ebb593b9a9c5bd9e0d4fc3b4c31bb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a><span data-ttu-id="82432-104">Použití úložiště Azure s clustery Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="82432-104">Use Azure storage with Azure HDInsight clusters</span></span>

<span data-ttu-id="82432-105">Pokud chcete analyzovat data v clusteru HDInsight, můžete je ukládat ve službě Azure Storage, Azure Data Lake Store nebo v obou.</span><span class="sxs-lookup"><span data-stu-id="82432-105">To analyze data in HDInsight cluster, you can store the data either in Azure Storage, Azure Data Lake Store, or both.</span></span> <span data-ttu-id="82432-106">Obě možnosti ukládání umožňují bezpečné odstranění clusterů HDInsight, které se používají pro výpočty, aniž by se ztratila uživatelská data.</span><span class="sxs-lookup"><span data-stu-id="82432-106">Both storage options enable you to safely delete HDInsight clusters that are used for computation without losing user data.</span></span>

<span data-ttu-id="82432-107">Hadoop podporuje hodnoty výchozího systému souborů.</span><span class="sxs-lookup"><span data-stu-id="82432-107">Hadoop supports a notion of the default file system.</span></span> <span data-ttu-id="82432-108">Výchozí systém souborů znamená výchozí schéma a autoritu.</span><span class="sxs-lookup"><span data-stu-id="82432-108">The default file system implies a default scheme and authority.</span></span> <span data-ttu-id="82432-109">Lze ho také použít k vyřešení relativní cesty.</span><span class="sxs-lookup"><span data-stu-id="82432-109">It can also be used to resolve relative paths.</span></span> <span data-ttu-id="82432-110">Během procesu vytváření clusteru HDInsight můžete jako výchozí systém souborů zadat kontejner objektů blob ve službě Azure Storage. U služby HDInsight 3.5 můžete s několika výjimkami jako výchozí systém souborů vybrat službu Azure Storage nebo Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="82432-110">During the HDInsight cluster creation process, you can specify a blob container in Azure Storage as the default file system, or with HDInsight 3.5, you can select either Azure Storage or Azure Data Lake Store as the default files system with a few exceptions.</span></span> <span data-ttu-id="82432-111">Informace o podpoře v případě, že použijete službu Data Lake Store jako výchozí i propojené úložiště, najdete v tématu [Dostupnost pro cluster HDInsight](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters).</span><span class="sxs-lookup"><span data-stu-id="82432-111">For the supportability of using Data Lake Store as both the default and linked storage, see [Availabilities for HDInsight cluster](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters).</span></span>

<span data-ttu-id="82432-112">V tomto článku se dozvíte, jak služba Azure Storage pracuje s clustery HDInsight.</span><span class="sxs-lookup"><span data-stu-id="82432-112">In this article, you learn how Azure Storage works with HDInsight clusters.</span></span> <span data-ttu-id="82432-113">Informace o tom, jak služba Data Lake Store pracuje s clustery HDInsight, najdete v tématu [Použití služby Azure Data Lake Store s clustery Azure HDInsight](hdinsight-hadoop-use-data-lake-store.md).</span><span class="sxs-lookup"><span data-stu-id="82432-113">To learn how Data Lake Store works with HDInsight clusters, see [Use Azure Data Lake Store with Azure HDInsight clusters](hdinsight-hadoop-use-data-lake-store.md).</span></span> <span data-ttu-id="82432-114">Další informace o vytvoření clusteru HDInsight najdete v tématu [Vytváření clusterů Hadoop ve službě HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="82432-114">For more information about creating an HDInsight cluster, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="82432-115">Azure Storage je robustní řešení úložiště pro obecné účely, které se jednoduše integruje se službou HDInsight.</span><span class="sxs-lookup"><span data-stu-id="82432-115">Azure storage is a robust, general-purpose storage solution that integrates seamlessly with HDInsight.</span></span> <span data-ttu-id="82432-116">HDInsight může jako výchozí systém souborů pro cluster používat kontejner objektů blob ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="82432-116">HDInsight can use a blob container in Azure Storage as the default file system for the cluster.</span></span> <span data-ttu-id="82432-117">Prostřednictvím rozhraní systému souborů Hadoop DFS (HDFS) může celá sada komponent ve službě HDInsight pracovat přímo se strukturovanými nebo nestrukturovanými daty uloženými jako objekty blob.</span><span class="sxs-lookup"><span data-stu-id="82432-117">Through a Hadoop distributed file system (HDFS) interface, the full set of components in HDInsight can operate directly on structured or unstructured data stored as blobs.</span></span>

> [!WARNING]
> <span data-ttu-id="82432-118">Při vytváření účtu služby Azure Storage je k dispozici několik možností.</span><span class="sxs-lookup"><span data-stu-id="82432-118">There are several options available when creating an Azure Storage account.</span></span> <span data-ttu-id="82432-119">Následující tabulka poskytuje informace o podporovaných možnostech se službou HDInsight:</span><span class="sxs-lookup"><span data-stu-id="82432-119">The following table provides information on what options are supported with HDInsight:</span></span>
> 
> | <span data-ttu-id="82432-120">Typ účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="82432-120">Storage account type</span></span> | <span data-ttu-id="82432-121">Úroveň úložiště</span><span class="sxs-lookup"><span data-stu-id="82432-121">Storage tier</span></span> | <span data-ttu-id="82432-122">Podporováno se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="82432-122">Supported with HDInsight</span></span> |
> | ------- | ------- | ------- |
> | <span data-ttu-id="82432-123">Účet služby Storage pro obecné účely</span><span class="sxs-lookup"><span data-stu-id="82432-123">General-purpose Storage Account</span></span> | <span data-ttu-id="82432-124">Standard</span><span class="sxs-lookup"><span data-stu-id="82432-124">Standard</span></span> | <span data-ttu-id="82432-125">__Ano__</span><span class="sxs-lookup"><span data-stu-id="82432-125">__Yes__</span></span> |
> | &nbsp; | <span data-ttu-id="82432-126">Premium</span><span class="sxs-lookup"><span data-stu-id="82432-126">Premium</span></span> | <span data-ttu-id="82432-127">Ne</span><span class="sxs-lookup"><span data-stu-id="82432-127">No</span></span> |
> | <span data-ttu-id="82432-128">Účet služby Blob Storage</span><span class="sxs-lookup"><span data-stu-id="82432-128">Blob Storage Account</span></span> | <span data-ttu-id="82432-129">Hot</span><span class="sxs-lookup"><span data-stu-id="82432-129">Hot</span></span> | <span data-ttu-id="82432-130">Ne</span><span class="sxs-lookup"><span data-stu-id="82432-130">No</span></span> |
> | &nbsp; | <span data-ttu-id="82432-131">Cool</span><span class="sxs-lookup"><span data-stu-id="82432-131">Cool</span></span> | <span data-ttu-id="82432-132">Ne</span><span class="sxs-lookup"><span data-stu-id="82432-132">No</span></span> |

<span data-ttu-id="82432-133">Nedoporučujeme používat výchozí kontejner objektů blob pro ukládání firemních dat.</span><span class="sxs-lookup"><span data-stu-id="82432-133">We do not recommend that you use the default blob container for storing business data.</span></span> <span data-ttu-id="82432-134">Ideální postup je výchozí kontejner objektů blob po každém použití odstranit a snížit tak náklady na úložiště.</span><span class="sxs-lookup"><span data-stu-id="82432-134">Deleting the default blob container after each use to reduce storage cost is a good practice.</span></span> <span data-ttu-id="82432-135">Mějte na paměti, že výchozí kontejner obsahuje protokoly aplikace a systémový protokol.</span><span class="sxs-lookup"><span data-stu-id="82432-135">Note that the default container contains application and system logs.</span></span> <span data-ttu-id="82432-136">Než odstraníte kontejner, nezapomeňte tyto protokoly načíst.</span><span class="sxs-lookup"><span data-stu-id="82432-136">Make sure to retrieve the logs before deleting the container.</span></span>

<span data-ttu-id="82432-137">Sdílení jednoho kontejneru objektů blob pro několik clusterů se nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="82432-137">Sharing one blob container for multiple clusters is not supported.</span></span>

## <a name="hdinsight-storage-architecture"></a><span data-ttu-id="82432-138">Architektura úložiště HDInsight</span><span class="sxs-lookup"><span data-stu-id="82432-138">HDInsight storage architecture</span></span>
<span data-ttu-id="82432-139">Následující diagram představuje abstraktní zobrazení architektury úložiště HDInsight, které používá službu Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="82432-139">The following diagram provides an abstract view of the HDInsight storage architecture of using Azure Storage:</span></span>

<span data-ttu-id="82432-140">![Clustery Hadoop používají rozhraní API HDFS pro přístup a ukládání strukturovaných i nestrukturovaných dat do služby Blob Storage.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "Architektura HDInsight Storage")</span><span class="sxs-lookup"><span data-stu-id="82432-140">![Hadoop clusters use the HDFS API to access and store structured and unstructured data in Blob storage.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Storage Architecture")</span></span>

<span data-ttu-id="82432-141">Služba HDInsight poskytuje přístup do systému souborů DFS, který je místně připojen k výpočetním uzlům.</span><span class="sxs-lookup"><span data-stu-id="82432-141">HDInsight provides access to the distributed file system that is locally attached to the compute nodes.</span></span> <span data-ttu-id="82432-142">Tento systém souborů je přístupný pomocí plně kvalifikovaného identifikátoru URI, například:</span><span class="sxs-lookup"><span data-stu-id="82432-142">This file system can be accessed by using the fully qualified URI, for example:</span></span>

    hdfs://<namenodehost>/<path>

<span data-ttu-id="82432-143">Služba HDInsight navíc umožňuje přístup k datům uloženým ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="82432-143">In addition, HDInsight allows you to access data that is stored in Azure Storage.</span></span> <span data-ttu-id="82432-144">Syntaxe je:</span><span class="sxs-lookup"><span data-stu-id="82432-144">The syntax is:</span></span>

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

<span data-ttu-id="82432-145">Při použití účtu Azure Storage s clustery HDInsight je potřeba zvážit tyto aspekty.</span><span class="sxs-lookup"><span data-stu-id="82432-145">Here are some considerations when using Azure Storage account with HDInsight clusters.</span></span>

* <span data-ttu-id="82432-146">**Kontejnery v účtech úložiště, které jsou připojeny ke clusteru:** Vzhledem k tomu, že název účtu a klíč jsou během vytváření přidružené  ke clusteru, máte plný přístup k objektům blob v těchto kontejnerech.</span><span class="sxs-lookup"><span data-stu-id="82432-146">**Containers in the storage accounts that are connected to a cluster:** Because the account name and key are associated with the cluster during creation, you have full access to the blobs in those containers.</span></span>

* <span data-ttu-id="82432-147">**Veřejné kontejnery nebo veřejné objekty blob v účtech úložiště, které NEJSOU připojené ke clusteru:** Máte oprávnění jen pro čtení objektů blob v kontejnerech.</span><span class="sxs-lookup"><span data-stu-id="82432-147">**Public containers or public blobs in storage accounts that are NOT connected to a cluster:** You have read-only permission to the blobs in the containers.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="82432-148">Veřejné kontejnery umožňují získat seznam všech objektů blob, které jsou k dispozici v tomto kontejneru a získat metadata kontejneru.</span><span class="sxs-lookup"><span data-stu-id="82432-148">Public containers allow you to get a list of all blobs that are available in that container and get container metadata.</span></span> <span data-ttu-id="82432-149">Veřejné objekty blob umožňují přístup k objektům blob jenom v případě, že znáte přesnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="82432-149">Public blobs allow you to access the blobs only if you know the exact URL.</span></span> <span data-ttu-id="82432-150">Další informace najdete v tématu <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Omezení přístupu ke kontejnerům a objektům blob</a>.</span><span class="sxs-lookup"><span data-stu-id="82432-150">For more information, see <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Restrict access to containers and blobs</a>.</span></span>
  > 
  > 
* <span data-ttu-id="82432-151">**Privátní kontejnery v účtech úložiště, které NEJSOU připojené ke clusteru:** Nemůžete získat přístup k objektům blob, dokud nedefinujete účet úložiště při odesílání úlohy WebHCat.</span><span class="sxs-lookup"><span data-stu-id="82432-151">**Private containers in storage accounts that are NOT connected to a cluster:** You can't access the blobs in the containers unless you define the storage account when you submit the WebHCat jobs.</span></span> <span data-ttu-id="82432-152">To se vysvětluje dále v tomhle článku.</span><span class="sxs-lookup"><span data-stu-id="82432-152">This is explained later in this article.</span></span>

<span data-ttu-id="82432-153">Účty úložiště, které se definují v procesu vytváření a jejich klíče jsou uloženy v %HADOOP_HOME%/conf/core-site.xml na uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="82432-153">The storage accounts that are defined in the creation process and their keys are stored in %HADOOP_HOME%/conf/core-site.xml on the cluster nodes.</span></span> <span data-ttu-id="82432-154">Výchozím chováním služby HDInsight je používání účtů úložiště, které jsou definovány v souboru core-site.xml.</span><span class="sxs-lookup"><span data-stu-id="82432-154">The default behavior of HDInsight is to use the storage accounts defined in the core-site.xml file.</span></span> <span data-ttu-id="82432-155">Toto nastavení můžete upravit pomocí [Ambari](./hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="82432-155">You can modify this setting using [Ambari](./hdinsight-hadoop-manage-ambari.md)</span></span>

<span data-ttu-id="82432-156">Více úloh WebHCat, včetně Hive, MapReduce, streamování Hadoop a Pig, může obsahovat popis účtů úložiště a spojených metadat.</span><span class="sxs-lookup"><span data-stu-id="82432-156">Multiple WebHCat jobs, including Hive, MapReduce, Hadoop streaming, and Pig, can carry a description of storage accounts and metadata with them.</span></span> <span data-ttu-id="82432-157">(To aktuálně funguje pro Pig s účty úložiště, ale ne pro metadata.) Více informací najdete v části [Použití clusteru HDInsight s alternativními účty úložiště a metaúložišti](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).</span><span class="sxs-lookup"><span data-stu-id="82432-157">(This currently works for Pig with storage accounts, but not for metadata.) For more information, see [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).</span></span>

<span data-ttu-id="82432-158">Objekty blob lze použít pro strukturovaná i nestrukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="82432-158">Blobs can be used for structured and unstructured data.</span></span> <span data-ttu-id="82432-159">Kontejnery objektů blob ukládají data jako páry klíč/hodnota a neexistuje žádná hierarchie adresářů.</span><span class="sxs-lookup"><span data-stu-id="82432-159">Blob containers store data as key/value pairs, and there is no directory hierarchy.</span></span> <span data-ttu-id="82432-160">V názvu klíče se dá použít lomítko (/), aby název klíče připomínal  cestu k souboru.</span><span class="sxs-lookup"><span data-stu-id="82432-160">However the slash character ( / ) can be used within the key name to make it appear as if a file is stored within a directory structure.</span></span> <span data-ttu-id="82432-161">Klíč k objektu blob může být například *input/log1.txt*.</span><span class="sxs-lookup"><span data-stu-id="82432-161">For example, a blob's key may be *input/log1.txt*.</span></span> <span data-ttu-id="82432-162">Žádný skutečný *vstupní* adresář neexistuje, ale vzhledem k lomítku v názvu klíče tento název připomíná zobrazení cesty k souboru.</span><span class="sxs-lookup"><span data-stu-id="82432-162">No actual *input* directory exists, but due to the presence of the slash character in the key name, it has the appearance of a file path.</span></span>

## <span data-ttu-id="82432-163"><a id="benefits"></a>Výhody služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="82432-163"><a id="benefits"></a>Benefits of Azure Storage</span></span>
<span data-ttu-id="82432-164">Předpokládaná výkonová náročnost společně umístěných výpočetních clusterů a prostředků úložiště je zmírněna tím, že výpočetní clustery jsou vytvořeny blízko prostředků účtu úložiště uvnitř oblasti Azure, kde vysokorychlostní síť umožňuje efektivní přístup výpočetních uzlů k datům ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="82432-164">The implied performance cost of not co-locating compute clusters and storage resources is mitigated by the way the compute clusters are created close to the storage account resources inside the Azure region, where the high-speed network makes it efficient for the compute nodes to access the data inside Azure storage.</span></span>

<span data-ttu-id="82432-165">S ukládáním dat ve službě Azure Storage namísto HDFS je spojeno několik výhod:</span><span class="sxs-lookup"><span data-stu-id="82432-165">There are several benefits associated with storing the data in Azure storage instead of HDFS:</span></span>

* <span data-ttu-id="82432-166">**Opakované použití dat a sdílení:** data v HDFS se nachází uvnitř výpočetního clusteru.</span><span class="sxs-lookup"><span data-stu-id="82432-166">**Data reuse and sharing:** The data in HDFS is located inside the compute cluster.</span></span> <span data-ttu-id="82432-167">Jenom aplikace, které mají přístup k výpočetnímu clusteru, můžou používat data pomocí rozhraní API HDFS.</span><span class="sxs-lookup"><span data-stu-id="82432-167">Only the applications that have access to the compute cluster can use the data by using HDFS APIs.</span></span> <span data-ttu-id="82432-168">Data ve službě Azure Storage jsou přístupná prostřednictvím rozhraní API HDFS nebo prostřednictvím [rozhraní REST API služby Blob Storage][blob-storage-restAPI].</span><span class="sxs-lookup"><span data-stu-id="82432-168">The data in Azure storage can be accessed either through the HDFS APIs or through the [Blob Storage REST APIs][blob-storage-restAPI].</span></span> <span data-ttu-id="82432-169">Proto s větším počtem aplikací (včetně jiných clusterů HDInsight) a nástrojů  se dají vytvářet a využívat data.</span><span class="sxs-lookup"><span data-stu-id="82432-169">Thus, a larger set of applications (including other HDInsight clusters) and tools can be used to produce and consume the data.</span></span>
* <span data-ttu-id="82432-170">**Archivace dat:** Ukládání dat ve službě Azure Storage umožňuje bezpečné odstranění clusterů HDInsight, které jsou používány pro výpočty, aniž by se ztratila uživatelská data.</span><span class="sxs-lookup"><span data-stu-id="82432-170">**Data archiving:** Storing data in Azure storage enables the HDInsight clusters used for computation to be safely deleted without losing user data.</span></span>
* <span data-ttu-id="82432-171">**Náklady na úložiště dat:** Ukládání dat v systému souborů DFS je z dlouhodobého hlediska dražší než ukládání dat ve službě Azure Storage, protože náklady na výpočetní cluster jsou vyšší než náklady na službu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="82432-171">**Data storage cost:** Storing data in DFS for the long term is more costly than storing the data in Azure storage because the cost of a compute cluster is higher than the cost of Azure storage.</span></span> <span data-ttu-id="82432-172">Navíc se data nemusí nahrávat znovu pro každou generaci výpočetních clusterů, náklady na nahrávání dat jsou tak nižší.</span><span class="sxs-lookup"><span data-stu-id="82432-172">In addition, because the data does not have to be reloaded for every compute cluster generation, you are also saving data loading costs.</span></span>
* <span data-ttu-id="82432-173">**Elastické škálování:** I když HDFS poskytuje škálovaný systém souborů, škála se určuje podle počtu uzlů, které vytvoříte pro svůj cluster.</span><span class="sxs-lookup"><span data-stu-id="82432-173">**Elastic scale-out:** Although HDFS provides you with a scaled-out file system, the scale is determined by the number of nodes that you create for your cluster.</span></span> <span data-ttu-id="82432-174">Změna škálování může být složitější než využití elastického škálování, které je automaticky k dispozici ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="82432-174">Changing the scale can become a more complicated process than relying on the elastic scaling capabilities that you get automatically in Azure storage.</span></span>
* <span data-ttu-id="82432-175">**Geografická replikace:** Službu Azure Storage je možné geograficky replikovat.</span><span class="sxs-lookup"><span data-stu-id="82432-175">**Geo-replication:** Your Azure storage can be geo-replicated.</span></span> <span data-ttu-id="82432-176">I když to přináší geografické obnovení a redundanci dat, převzetí služeb při selhání do geograficky replikovaného umístění vážně ovlivňuje výkon a může vést k dalším nákladům.</span><span class="sxs-lookup"><span data-stu-id="82432-176">Although this gives you geographic recovery and data redundancy, a failover to the geo-replicated location severely impacts your performance, and it may incur additional costs.</span></span> <span data-ttu-id="82432-177">Doporučujeme proto geografickou replikaci dobře zvážit a zvolit jen v případě, že hodnota dat je vyšší než náklady na celou operaci.</span><span class="sxs-lookup"><span data-stu-id="82432-177">So our recommendation is to choose the geo-replication wisely and only if the value of the data is worth the additional cost.</span></span>

<span data-ttu-id="82432-178">Některé úlohy a balíčky MapReduce můžou vytvořit mezilehlé výsledky, které ve službě Azure Storage ve skutečnosti uložit nechcete.</span><span class="sxs-lookup"><span data-stu-id="82432-178">Certain MapReduce jobs and packages may create intermediate results that you don't really want to store in Azure storage.</span></span> <span data-ttu-id="82432-179">V takovém případě můžete zvolit k uložení dat do místní HDFS.</span><span class="sxs-lookup"><span data-stu-id="82432-179">In that case, you can elect to store the data in the local HDFS.</span></span> <span data-ttu-id="82432-180">Ve skutečnosti služba HDInsight používá DFS pro některé z těchto mezilehlých výsledků v úlohách Hive a jiných procesech.</span><span class="sxs-lookup"><span data-stu-id="82432-180">In fact, HDInsight uses DFS for several of these intermediate results in Hive jobs and other processes.</span></span>

> [!NOTE]
> <span data-ttu-id="82432-181">Většina příkazů HDFS (například <b>ls</b>, <b>copyFromLocal</b> a <b>mkdir</b>) bude i nadále fungovat podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="82432-181">Most HDFS commands (for example, <b>ls</b>, <b>copyFromLocal</b> and <b>mkdir</b>) still work as expected.</span></span> <span data-ttu-id="82432-182">Jenom příkazy, které jsou specifické pro nativní implementaci HDFS (což se označuje jako DFS), jako je například <b>fschk</b> a <b>dfsadmin</b>, se budou chovat ve službě Azure Storage odlišně.</span><span class="sxs-lookup"><span data-stu-id="82432-182">Only the commands that are specific to the native HDFS implementation (which is referred to as DFS), such as <b>fschk</b> and <b>dfsadmin</b>, show different behavior in Azure storage.</span></span>
> 
> 

## <a name="create-blob-containers"></a><span data-ttu-id="82432-183">Vytvoření kontejnerů objektů Blob</span><span class="sxs-lookup"><span data-stu-id="82432-183">Create Blob containers</span></span>
<span data-ttu-id="82432-184">K použití objektů blob je třeba nejprve vytvořit [Účet služby Azure Storage][azure-storage-create].</span><span class="sxs-lookup"><span data-stu-id="82432-184">To use blobs, you first create an [Azure Storage account][azure-storage-create].</span></span> <span data-ttu-id="82432-185">V rámci tohoto procesu zadáte oblast Azure, ve které se účet úložiště vytvoří.</span><span class="sxs-lookup"><span data-stu-id="82432-185">As part of this, you specify an Azure region where the storage account is created.</span></span> <span data-ttu-id="82432-186">Účet úložiště a clusteru musí být uloženy ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="82432-186">The cluster and the storage account must be hosted in the same region.</span></span> <span data-ttu-id="82432-187">Databáze serveru SQL metaúložiště Hive a databáze serveru SQL metaúložiště Oozie musí být také umístěny ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="82432-187">The Hive metastore SQL Server database and Oozie metastore SQL Server database must also be located in the same region.</span></span>

<span data-ttu-id="82432-188">Bez ohledu na svoje umístění patří každý objekt blob, který vytvoříte, do kontejneru v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="82432-188">Wherever it lives, each blob you create belongs to a container in your Azure Storage account.</span></span> <span data-ttu-id="82432-189">Tento kontejner může být existující objekt blob, který se vytvořil mimo HDInsight, nebo to může být kontejner, který se vytvořil pro cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="82432-189">This container may be an existing blob that was created outside of HDInsight, or it may be a container that is created for an HDInsight cluster.</span></span>

<span data-ttu-id="82432-190">Výchozí kontejner objektu blob ukládá konkrétní informace, jako je historie úlohy a protokoly.</span><span class="sxs-lookup"><span data-stu-id="82432-190">The default Blob container stores cluster-specific information such as job history and logs.</span></span> <span data-ttu-id="82432-191">Výchozí kontejner objektu Blob nesdílejte s více clustery služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="82432-191">Don't share a default Blob container with multiple HDInsight clusters.</span></span> <span data-ttu-id="82432-192">Může dojít k poškození historie úlohy.</span><span class="sxs-lookup"><span data-stu-id="82432-192">This might corrupt job history.</span></span> <span data-ttu-id="82432-193">Doporučujeme použít jiný kontejner pro každý cluster a umístit sdílená data na propojený účet úložiště, zadaný v nasazení všech příslušných clusterů, nikoli na výchozí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="82432-193">It is recommended to use a different container for each cluster and put shared data on a linked storage account specified in deployment of all relevant clusters rather than the default storage account.</span></span> <span data-ttu-id="82432-194">Další informace o konfiguraci propojených účtů úložiště najdete v tématu [Tvorba clusterů HDInsight][hdinsight-creation].</span><span class="sxs-lookup"><span data-stu-id="82432-194">For more information on configuring linked storage accounts, see [Create HDInsight clusters][hdinsight-creation].</span></span> <span data-ttu-id="82432-195">Nicméně, po odstranění původního clusteru HDInsight můžete znovu použít výchozí kontejner úložiště.</span><span class="sxs-lookup"><span data-stu-id="82432-195">However you can reuse a default storage container after the original HDInsight cluster has been deleted.</span></span> <span data-ttu-id="82432-196">Pro clustery HBase můžete zachovat schéma a data tabulky HBase vytvořením nového clusteru HBase pomocí výchozího kontejneru objektů blob, který je používán odstraněným clusterem HBase.</span><span class="sxs-lookup"><span data-stu-id="82432-196">For HBase clusters, you can actually retain the HBase table schema and data by creating a new HBase cluster using the default blob container that is used by an HBase cluster that has been deleted.</span></span>

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

### <a name="use-the-azure-portal"></a><span data-ttu-id="82432-197">Použití webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="82432-197">Use the Azure portal</span></span>
<span data-ttu-id="82432-198">Při vytváření clusteru HDInsight z portálu máte možnost (jak je vidět níže) zadat podrobnosti účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="82432-198">When creating an HDInsight cluster from the Portal, you have the options (as shown below) to provide the storage account details.</span></span> <span data-ttu-id="82432-199">Můžete také určit, jestli chcete ke clusteru přidružit další účet úložiště, a pokud ano, zvolit jako další úložiště službu Data Lake Store nebo další Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="82432-199">You can also specify whether you want an additional storage account associated with the cluster, and if so, choose from Data Lake Store or another Azure Storage blob as the additional storage.</span></span>

![Zdroj dat pro vytvoření hadoopu HDInsight](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

> [!WARNING]
> <span data-ttu-id="82432-201">Použití dalšího účtu úložiště v jiném umístění, než je cluster HDInsight, není podporováno.</span><span class="sxs-lookup"><span data-stu-id="82432-201">Using an additional storage account in a different location than the HDInsight cluster is not supported.</span></span>


### <a name="use-azure-powershell"></a><span data-ttu-id="82432-202">Použití Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="82432-202">Use Azure PowerShell</span></span>
<span data-ttu-id="82432-203">Pokud jste [nainstalovali a nakonfigurovali Azure PowerShell][powershell-install], můžete použít následující z příkazového řádku Azure PowerShell k vytvoření účtu úložiště a kontejneru:</span><span class="sxs-lookup"><span data-stu-id="82432-203">If you [installed and configured Azure PowerShell][powershell-install], you can use the following from the Azure PowerShell prompt to create a storage account and container:</span></span>

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

### <a name="use-azure-cli"></a><span data-ttu-id="82432-204">Použití Azure CLI</span><span class="sxs-lookup"><span data-stu-id="82432-204">Use Azure CLI</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

<span data-ttu-id="82432-205">Pokud máte [nainstalováno a nakonfigurováno rozhraní příkazového řádku Azure CLI](../cli-install-nodejs.md), následující příkaz lze použít k účtu úložiště a kontejneru.</span><span class="sxs-lookup"><span data-stu-id="82432-205">If you have [installed and configured the Azure CLI](../cli-install-nodejs.md), the following command can be used to a storage account and container.</span></span>

    azure storage account create <storageaccountname> --type LRS

> [!NOTE]
> <span data-ttu-id="82432-206">Parametr `--type` určuje, jak bude účet úložiště replikován.</span><span class="sxs-lookup"><span data-stu-id="82432-206">The `--type` parameter indicates how the storage account is replicated.</span></span> <span data-ttu-id="82432-207">Další informace najdete v tématu [Replikace Azure Storage](../storage/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="82432-207">For more information, see [Azure Storage Replication](../storage/storage-redundancy.md).</span></span> <span data-ttu-id="82432-208">Nepoužívejte ZRS, protože nepodporuje objekt blob stránky, soubor, tabulku ani frontu.</span><span class="sxs-lookup"><span data-stu-id="82432-208">Don't use ZRS as ZRS doesn't support page blob, file, table, or queue.</span></span>
> 
> 

<span data-ttu-id="82432-209">Budete vyzváni k zadání geografické oblasti, ve které se vytvoří účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="82432-209">You are prompted to specify the geographic region that the storage account is created in.</span></span> <span data-ttu-id="82432-210">Účet úložiště byste měli vytvořit ve stejné oblasti, kterou chcete použít k vytvoření clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="82432-210">You should create the storage account in the same region that you plan on creating your HDInsight cluster.</span></span>

<span data-ttu-id="82432-211">Po vytvoření účtu úložiště použijte následující příkaz k načtení klíčů účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="82432-211">Once the storage account is created, use the following command to retrieve the storage account keys:</span></span>

    azure storage account keys list <storageaccountname>

<span data-ttu-id="82432-212">Když chcete vytvořit kontejner, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="82432-212">To create a container, use the following command:</span></span>

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="address-files-in-azure-storage"></a><span data-ttu-id="82432-213">Adresování souborů ve službě Azure Storage</span><span class="sxs-lookup"><span data-stu-id="82432-213">Address files in Azure storage</span></span>
<span data-ttu-id="82432-214">Schéma identifikátoru URI pro přístup k souborům ve službě Azure Storage ze služby HDInsight je:</span><span class="sxs-lookup"><span data-stu-id="82432-214">The URI scheme for accessing files in Azure storage from HDInsight is:</span></span>

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>

<span data-ttu-id="82432-215">Schéma identifikátoru URI poskytuje nezašifrovaný přístup (s předponou *wasb:*) a zašifrovaný přístup SSL (s *wasbs*).</span><span class="sxs-lookup"><span data-stu-id="82432-215">The URI scheme provides unencrypted access (with the *wasb:* prefix) and SSL encrypted access (with *wasbs*).</span></span> <span data-ttu-id="82432-216">Doporučujeme používat *wasbs* kdykoli je to možné, i v případě přístupu k datům, umístěným uvnitř stejné oblasti v Azure.</span><span class="sxs-lookup"><span data-stu-id="82432-216">We recommend using *wasbs* wherever possible, even when accessing data that lives inside the same region in Azure.</span></span>

<span data-ttu-id="82432-217">&lt;BlobStorageContainerName&gt; označuje název kontejneru objektů blob ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="82432-217">The &lt;BlobStorageContainerName&gt; identifies the name of the blob container in Azure storage.</span></span>
<span data-ttu-id="82432-218">&lt;StorageAccountName&gt; identifikuje název účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="82432-218">The &lt;StorageAccountName&gt; identifies the Azure Storage account name.</span></span> <span data-ttu-id="82432-219">Vyžaduje se plně kvalifikovaný název domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="82432-219">A fully qualified domain name (FQDN) is required.</span></span>

<span data-ttu-id="82432-220">Pokud nebyl zadán &lt;BlobStorageContainerName&gt; ani &lt;StorageAccountName&gt;, použije se výchozí systém souborů.</span><span class="sxs-lookup"><span data-stu-id="82432-220">If neither &lt;BlobStorageContainerName&gt; nor &lt;StorageAccountName&gt; has been specified, the default file system is used.</span></span> <span data-ttu-id="82432-221">Pro soubory ve výchozím systému souborů můžete použít relativní cestu nebo absolutní cestu.</span><span class="sxs-lookup"><span data-stu-id="82432-221">For the files on the default file system, you can use a relative path or an absolute path.</span></span> <span data-ttu-id="82432-222">Například soubor *hadoop-mapreduce-examples.jar*, který se dodává s clustery HDInsight, lze odkazovat pomocí jedné z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="82432-222">For example, the *hadoop-mapreduce-examples.jar* file that comes with HDInsight clusters can be referred to by using one of the following:</span></span>

    wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasb:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> <span data-ttu-id="82432-223">V clusterech HDInsight verze 2.1 a 1.6. je název souboru <i>hadoop-examples.jar</i></span><span class="sxs-lookup"><span data-stu-id="82432-223">The file name is <i>hadoop-examples.jar</i> in HDInsight versions 2.1 and 1.6 clusters.</span></span>
> 
> 

<span data-ttu-id="82432-224">&lt;Cesta&gt; je název cesty HDFS souboru nebo adresáře.</span><span class="sxs-lookup"><span data-stu-id="82432-224">The &lt;path&gt; is the file or directory HDFS path name.</span></span> <span data-ttu-id="82432-225">Vzhledem k tomu, že kontejnery ve službě Azure Storage jsou jednoduše úložiště párů klíč-hodnota, neexistuje žádný opravdový hierarchický systém souborů.</span><span class="sxs-lookup"><span data-stu-id="82432-225">Because containers in Azure storage are simply key-value stores, there is no true hierarchical file system.</span></span> <span data-ttu-id="82432-226">Lomítko ( / ) uvnitř klíče objektu blob se považuje za oddělovač adresářů.</span><span class="sxs-lookup"><span data-stu-id="82432-226">A slash character ( / ) inside a blob key is interpreted as a directory separator.</span></span> <span data-ttu-id="82432-227">Například název objektu blob pro *hadoop-mapreduce-examples.jar* je:</span><span class="sxs-lookup"><span data-stu-id="82432-227">For example, the blob name for *hadoop-mapreduce-examples.jar* is:</span></span>

    example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> <span data-ttu-id="82432-228">Při práci s objekty blob mimo HDInsight většina nástrojů nerozpozná formát WASB a místo toho očekávají základní formát cesty, jako je například `example/jars/hadoop-mapreduce-examples.jar`.</span><span class="sxs-lookup"><span data-stu-id="82432-228">When working with blobs outside of HDInsight, most utilities do not recognize the WASB format and instead expect a basic path format, such as `example/jars/hadoop-mapreduce-examples.jar`.</span></span>
> 
> 

## <a name="access-blobs"></a><span data-ttu-id="82432-229">Přístup k objektům blob</span><span class="sxs-lookup"><span data-stu-id="82432-229">Access blobs</span></span> 


### <span data-ttu-id="82432-230"><a name="access-blobs-using-azure-powershell"></a> Použití Azure Powershellu</span><span class="sxs-lookup"><span data-stu-id="82432-230"><a name="access-blobs-using-azure-powershell"></a> Use Azure PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="82432-231">Příkazy v této části jsou ukázkami základních příkladů použití prostředí PowerShell pro přístup k datům, uloženým v objektech blob.</span><span class="sxs-lookup"><span data-stu-id="82432-231">The commands in this section provide a basic example of using PowerShell to access data stored in blobs.</span></span> <span data-ttu-id="82432-232">Obsáhlejší a plnohodnotný příklad, přizpůsobený pro práci s HDInsight, najdete v části [Nástroje HDInsight](https://github.com/Blackmist/hdinsight-tools).</span><span class="sxs-lookup"><span data-stu-id="82432-232">For a more full-featured example that is customized for working with HDInsight, see the [HDInsight Tools](https://github.com/Blackmist/hdinsight-tools).</span></span>
> 
> 

<span data-ttu-id="82432-233">Pomocí následujícího příkazu můžete zobrazit seznam rutin týkajících se objektu blob:</span><span class="sxs-lookup"><span data-stu-id="82432-233">Use the following command to list the blob-related cmdlets:</span></span>

    Get-Command *blob*

![Seznam rutin prostředí PowerShell týkajících se objektu blob.][img-hdi-powershell-blobcommands]

#### <a name="upload-files"></a><span data-ttu-id="82432-235">Nahrání souborů</span><span class="sxs-lookup"><span data-stu-id="82432-235">Upload files</span></span>
<span data-ttu-id="82432-236">Viz [Nahrání dat do služby HDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="82432-236">See [Upload data to HDInsight][hdinsight-upload-data].</span></span>

#### <a name="download-files"></a><span data-ttu-id="82432-237">Stažení souborů</span><span class="sxs-lookup"><span data-stu-id="82432-237">Download files</span></span>
<span data-ttu-id="82432-238">Následující skript stáhne objekt blob bloku do aktuální složky.</span><span class="sxs-lookup"><span data-stu-id="82432-238">The following script downloads a block blob to the current folder.</span></span> <span data-ttu-id="82432-239">Před spuštěním skriptu změňte adresář na složku, ke které máte oprávnění k zápisu.</span><span class="sxs-lookup"><span data-stu-id="82432-239">Before running the script, change the directory to a folder where you have write permissions.</span></span>

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # The storage account used for the default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # The default file system container has the same name as the cluster.
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.

    # Use Add-AzureAccount if you haven't connected to your Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"

    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

    Write-Host "List the downloaded file ..." -ForegroundColor Green
    cat "./$blob"

<span data-ttu-id="82432-240">Při poskytnutí názvu skupiny prostředků a názvu clusteru můžete použít následující kód:</span><span class="sxs-lookup"><span data-stu-id="82432-240">Providing the resource group name and the cluster name, you can use the following code:</span></span>

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.

    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 

    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force


#### <a name="delete-files"></a><span data-ttu-id="82432-241">Odstranění souborů</span><span class="sxs-lookup"><span data-stu-id="82432-241">Delete files</span></span>
    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

#### <a name="list-files"></a><span data-ttu-id="82432-242">Zobrazení souborů</span><span class="sxs-lookup"><span data-stu-id="82432-242">List files</span></span>
    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

#### <a name="run-hive-queries-using-an-undefined-storage-account"></a><span data-ttu-id="82432-243">Spuštění dotazů Hive pomocí nedefinovaného účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="82432-243">Run Hive queries using an undefined storage account</span></span>
<span data-ttu-id="82432-244">Tento příklad ukazuje, jak zobrazit obsah složky z účtu úložiště, které není definováno během procesu vytváření.</span><span class="sxs-lookup"><span data-stu-id="82432-244">This example shows how to list a folder from storage account that is not defined during the creating process.</span></span>
<span data-ttu-id="82432-245">$clusterName = “<HDInsightClusterName>“</span><span class="sxs-lookup"><span data-stu-id="82432-245">$clusterName = "<HDInsightClusterName>"</span></span>

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasb://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"

### <a name="use-azure-cli"></a><span data-ttu-id="82432-246">Použití Azure CLI</span><span class="sxs-lookup"><span data-stu-id="82432-246">Use Azure CLI</span></span>
<span data-ttu-id="82432-247">Pomocí následujícího příkazu můžete zobrazit seznam příkazů týkajících se objektu blob:</span><span class="sxs-lookup"><span data-stu-id="82432-247">Use the following command to list the blob-related commands:</span></span>

    azure storage blob

<span data-ttu-id="82432-248">**Příklad použití Azure CLI pro nahrání souboru**</span><span class="sxs-lookup"><span data-stu-id="82432-248">**Example of using Azure CLI to upload a file**</span></span>

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="82432-249">**Příklad použití Azure CLI pro stažení souboru**</span><span class="sxs-lookup"><span data-stu-id="82432-249">**Example of using Azure CLI to download a file**</span></span>

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="82432-250">**Příklad použití Azure CLI pro odstranění souboru**</span><span class="sxs-lookup"><span data-stu-id="82432-250">**Example of using Azure CLI to delete a file**</span></span>

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="82432-251">**Příklad použití Azure CLI pro vytvoření seznamu souborů**</span><span class="sxs-lookup"><span data-stu-id="82432-251">**Example of using Azure CLI to list files**</span></span>

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="use-additional-storage-accounts"></a><span data-ttu-id="82432-252">Použití dalších účtů úložiště</span><span class="sxs-lookup"><span data-stu-id="82432-252">Use additional storage accounts</span></span>

<span data-ttu-id="82432-253">Při vytváření clusteru HDInsight zadáváte účet služby Azure Storage, který k němu chcete přidružit.</span><span class="sxs-lookup"><span data-stu-id="82432-253">While creating an HDInsight cluster, you specify the Azure Storage account you want to associate with it.</span></span> <span data-ttu-id="82432-254">Kromě tohoto účtu úložiště můžete během procesu vytváření nebo až po vytvoření clusteru přidat další účty úložiště ze stejného předplatného Azure nebo různých předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="82432-254">In addition to this storage account, you can add additional storage accounts from the same Azure subscription or different Azure subscriptions during the creation process or after a cluster has been created.</span></span> <span data-ttu-id="82432-255">Pokyny pro přidání dalších účtů úložiště najdete v tématu [Vytváření clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="82432-255">For instructions about adding additional storage accounts, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

> [!WARNING]
> <span data-ttu-id="82432-256">Použití dalšího účtu úložiště v jiném umístění, než je cluster HDInsight, není podporováno.</span><span class="sxs-lookup"><span data-stu-id="82432-256">Using an additional storage account in a different location than the HDInsight cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="82432-257">Další kroky</span><span class="sxs-lookup"><span data-stu-id="82432-257">Next steps</span></span>
<span data-ttu-id="82432-258">V tomto článku jste zjistili, jak používat HDFS kompatibilní úložiště Azure se službou HDInsight.</span><span class="sxs-lookup"><span data-stu-id="82432-258">In this article, you learned how to use HDFS-compatible Azure storage with HDInsight.</span></span> <span data-ttu-id="82432-259">To umožňuje vytvářet škálovatelná a dlouhodobá řešení pro získávání archivovaných dat a používat službu HDInsight k odemčení informací uvnitř uložených strukturovaných a nestrukturovaných dat.</span><span class="sxs-lookup"><span data-stu-id="82432-259">This allows you to build scalable, long-term, archiving data acquisition solutions and use HDInsight to unlock the information inside the stored structured and unstructured data.</span></span>

<span data-ttu-id="82432-260">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="82432-260">For more information, see:</span></span>

* <span data-ttu-id="82432-261">[Začínáme se službou Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="82432-261">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* [<span data-ttu-id="82432-262">Začínáme se službou Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="82432-262">Get started with Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-get-started-portal.md)
* <span data-ttu-id="82432-263">[Nahrání dat do služby HDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="82432-263">[Upload data to HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="82432-264">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="82432-264">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="82432-265">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="82432-265">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="82432-266">[Použití sdílených přístupových podpisů služby Azure Storage k omezení přístupu k datům pomocí HDInsight][hdinsight-use-sas]</span><span class="sxs-lookup"><span data-stu-id="82432-266">[Use Azure Storage Shared Access Signatures to restrict access to data with HDInsight][hdinsight-use-sas]</span></span>

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
