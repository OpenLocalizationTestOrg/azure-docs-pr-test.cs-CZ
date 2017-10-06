---
title: "aaaUse Data Lake Store s Hadoop v prostředí Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak tooquery dat z Azure Data Lake Store a toostore výsledky analýzy."
keywords: "blob storage, hdfs, strukturovaná data, nestrukturovaná data, data lake store"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: jgao
ms.openlocfilehash: 89633218a37a2fe05043e05d61199dcc0252d7f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-data-lake-store-with-azure-hdinsight-clusters"></a><span data-ttu-id="66b3c-104">Použití služby Data Lake Store s clustery Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="66b3c-104">Use Data Lake Store with Azure HDInsight clusters</span></span>

<span data-ttu-id="66b3c-105">tooanalyze data v clusteru HDInsight, hello data můžete ukládat buď v [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md), nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="66b3c-105">tooanalyze data in HDInsight cluster, you can store hello data either in [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md), or both.</span></span> <span data-ttu-id="66b3c-106">Obě možnosti úložiště povolit toosafely odstranění clusterů HDInsight, jsou používány pro výpočty, aniž by se ztratila uživatelská data.</span><span class="sxs-lookup"><span data-stu-id="66b3c-106">Both storage options enable you toosafely delete HDInsight clusters that are used for computation without losing user data.</span></span>

<span data-ttu-id="66b3c-107">V tomto článku se dozvíte, jak služba Data Lake Store pracuje s clustery HDInsight.</span><span class="sxs-lookup"><span data-stu-id="66b3c-107">In this article, you learn how Data Lake Store works with HDInsight clusters.</span></span> <span data-ttu-id="66b3c-108">toolearn jak funguje Azure Storage s clustery HDInsight, najdete v části [použití služby Azure Storage s Azure HDInsight clustery](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="66b3c-108">toolearn how Azure Storage works with HDInsight clusters, see [Use Azure Storage with Azure HDInsight clusters](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="66b3c-109">Další informace o vytvoření clusteru HDInsight najdete v tématu [Vytváření clusterů Hadoop ve službě HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="66b3c-109">For more information about creating an HDInsight cluster, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

> [!NOTE]
> <span data-ttu-id="66b3c-110">Ke službě Data Lake Store se vždy přistupuje prostřednictvím zabezpečeného kanálu, takže se nepoužívá název schématu systému souborů `adls`.</span><span class="sxs-lookup"><span data-stu-id="66b3c-110">Data Lake Store is always accessed through a secure channel, so there is no `adls` filesystem scheme name.</span></span> <span data-ttu-id="66b3c-111">Vždy používáte `adl`.</span><span class="sxs-lookup"><span data-stu-id="66b3c-111">You always use `adl`.</span></span>
> 
> 

## <a name="availabilities-for-hdinsight-clusters"></a><span data-ttu-id="66b3c-112">Dostupnosti pro clustery HDInsight</span><span class="sxs-lookup"><span data-stu-id="66b3c-112">Availabilities for HDInsight clusters</span></span>

<span data-ttu-id="66b3c-113">Hadoop podporuje hodnoty z hello výchozí systém souborů.</span><span class="sxs-lookup"><span data-stu-id="66b3c-113">Hadoop supports a notion of hello default file system.</span></span> <span data-ttu-id="66b3c-114">Hello výchozí systém souborů znamená výchozí schéma a autoritu.</span><span class="sxs-lookup"><span data-stu-id="66b3c-114">hello default file system implies a default scheme and authority.</span></span> <span data-ttu-id="66b3c-115">Lze také použít tooresolve relativní cesty.</span><span class="sxs-lookup"><span data-stu-id="66b3c-115">It can also be used tooresolve relative paths.</span></span> <span data-ttu-id="66b3c-116">Během procesu vytváření clusteru HDInsight hello, můžete zadat kontejner objektů blob v Azure Storage jako hello výchozí systém souborů, nebo s HDInsight 3.5 a novější verze, můžete vybrat Azure Storage nebo Azure Data Lake Store jako hello výchozí systém souborů s několik výjimek.</span><span class="sxs-lookup"><span data-stu-id="66b3c-116">During hello HDInsight cluster creation process, you can specify a blob container in Azure Storage as hello default file system, or with HDInsight 3.5 and newer versions, you can select either Azure Storage or Azure Data Lake Store as hello default files system with a few exceptions.</span></span> 

<span data-ttu-id="66b3c-117">Clustery HDInsight můžou službu Data Lake Store využívat dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="66b3c-117">HDInsight clusters can use Data Lake Store in two ways:</span></span>

* <span data-ttu-id="66b3c-118">Jako výchozí úložiště hello</span><span class="sxs-lookup"><span data-stu-id="66b3c-118">As hello default storage</span></span>
* <span data-ttu-id="66b3c-119">Jako další úložiště, přičemž Azure Storage Blob je výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="66b3c-119">As additional storage, with Azure Storage Blob as default storage.</span></span>

<span data-ttu-id="66b3c-120">Od nyní jenom některé z hello HDInsight clusteru podporu typy nebo verze pomocí Data Lake Store jako výchozí úložiště a dalších účtů úložiště:</span><span class="sxs-lookup"><span data-stu-id="66b3c-120">As of now, only some of hello HDInsight cluster types/versions support using Data Lake Store as default storage and additional storage accounts:</span></span>

| <span data-ttu-id="66b3c-121">Typ clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="66b3c-121">HDInsight cluster type</span></span> | <span data-ttu-id="66b3c-122">Data Lake Store jako výchozí úložiště</span><span class="sxs-lookup"><span data-stu-id="66b3c-122">Data Lake Store as default storage</span></span> | <span data-ttu-id="66b3c-123">Data Lake Store jako další úložiště</span><span class="sxs-lookup"><span data-stu-id="66b3c-123">Data Lake Store as additional storage</span></span>| <span data-ttu-id="66b3c-124">Poznámky</span><span class="sxs-lookup"><span data-stu-id="66b3c-124">Notes</span></span> |
|------------------------|------------------------------------|---------------------------------------|------|
| <span data-ttu-id="66b3c-125">HDInsight verze 3.6</span><span class="sxs-lookup"><span data-stu-id="66b3c-125">HDInsight version 3.6</span></span> | <span data-ttu-id="66b3c-126">Ano</span><span class="sxs-lookup"><span data-stu-id="66b3c-126">Yes</span></span> | <span data-ttu-id="66b3c-127">Ano</span><span class="sxs-lookup"><span data-stu-id="66b3c-127">Yes</span></span> | |
| <span data-ttu-id="66b3c-128">HDInsight verze 3.5</span><span class="sxs-lookup"><span data-stu-id="66b3c-128">HDInsight version 3.5</span></span> | <span data-ttu-id="66b3c-129">Ano</span><span class="sxs-lookup"><span data-stu-id="66b3c-129">Yes</span></span> | <span data-ttu-id="66b3c-130">Ano</span><span class="sxs-lookup"><span data-stu-id="66b3c-130">Yes</span></span> | <span data-ttu-id="66b3c-131">S výjimkou hello HBase</span><span class="sxs-lookup"><span data-stu-id="66b3c-131">With hello exception of HBase</span></span>|
| <span data-ttu-id="66b3c-132">HDInsight verze 3.4</span><span class="sxs-lookup"><span data-stu-id="66b3c-132">HDInsight version 3.4</span></span> | <span data-ttu-id="66b3c-133">Ne</span><span class="sxs-lookup"><span data-stu-id="66b3c-133">No</span></span> | <span data-ttu-id="66b3c-134">Ano</span><span class="sxs-lookup"><span data-stu-id="66b3c-134">Yes</span></span> | |
| <span data-ttu-id="66b3c-135">HDInsight verze 3.3</span><span class="sxs-lookup"><span data-stu-id="66b3c-135">HDInsight version 3.3</span></span> | <span data-ttu-id="66b3c-136">Ne</span><span class="sxs-lookup"><span data-stu-id="66b3c-136">No</span></span> | <span data-ttu-id="66b3c-137">Ne</span><span class="sxs-lookup"><span data-stu-id="66b3c-137">No</span></span> | |
| <span data-ttu-id="66b3c-138">HDInsight verze 3.2</span><span class="sxs-lookup"><span data-stu-id="66b3c-138">HDInsight version 3.2</span></span> | <span data-ttu-id="66b3c-139">Ne</span><span class="sxs-lookup"><span data-stu-id="66b3c-139">No</span></span> | <span data-ttu-id="66b3c-140">Ano</span><span class="sxs-lookup"><span data-stu-id="66b3c-140">Yes</span></span> | |
| <span data-ttu-id="66b3c-141">HDInsight Premium (úroveň)</span><span class="sxs-lookup"><span data-stu-id="66b3c-141">HDInsight Premium (tier)</span></span>| <span data-ttu-id="66b3c-142">Ne</span><span class="sxs-lookup"><span data-stu-id="66b3c-142">No</span></span> | <span data-ttu-id="66b3c-143">Ne</span><span class="sxs-lookup"><span data-stu-id="66b3c-143">No</span></span> | |
| <span data-ttu-id="66b3c-144">Storm</span><span class="sxs-lookup"><span data-stu-id="66b3c-144">Storm</span></span> | | |<span data-ttu-id="66b3c-145">Data Lake Store toowrite dat můžete použít z topologie Storm.</span><span class="sxs-lookup"><span data-stu-id="66b3c-145">You can use Data Lake Store toowrite data from a Storm topology.</span></span> <span data-ttu-id="66b3c-146">Data Lake Store můžete také použít pro referenční data, která pak může číst topologie Storm.</span><span class="sxs-lookup"><span data-stu-id="66b3c-146">You can also use Data Lake Store for reference data that can then be read by a Storm topology.</span></span>|

<span data-ttu-id="66b3c-147">Pomocí Data Lake Store jako další úložiště účet ovlivnit výkon nebo hello možnost tooread nebo zápisu tooAzure úložiště z clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="66b3c-147">Using Data Lake Store as an additional storage account does not affect performance or hello ability tooread or write tooAzure storage from hello cluster.</span></span>


## <a name="use-data-lake-store-as-default-storage"></a><span data-ttu-id="66b3c-148">Použití služby Data Lake Store jako výchozího úložiště</span><span class="sxs-lookup"><span data-stu-id="66b3c-148">Use Data Lake Store as default storage</span></span>

<span data-ttu-id="66b3c-149">Po nasazení HDInsight s Data Lake Store jako výchozí úložiště hello clusteru související soubory jsou uložené v Data Lake Store v hello následující umístění:</span><span class="sxs-lookup"><span data-stu-id="66b3c-149">When HDInsight is deployed with Data Lake Store as default storage, hello cluster-related files are stored in Data Lake Store in hello following location:</span></span>

    adl://mydatalakestore/<cluster_root_path>/

<span data-ttu-id="66b3c-150">kde `<cluster_root_path>` je hello název složky vytvoříte v Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="66b3c-150">where `<cluster_root_path>` is hello name of a folder you create in Data Lake Store.</span></span> <span data-ttu-id="66b3c-151">Zadat cestu ke kořenové pro každý cluster, můžete použít stejný účet Data Lake Store pro více než jeden cluster hello.</span><span class="sxs-lookup"><span data-stu-id="66b3c-151">By specifying a root path for each cluster, you can use hello same Data Lake Store account for more than one cluster.</span></span> <span data-ttu-id="66b3c-152">Takže máte nastavení, kde:</span><span class="sxs-lookup"><span data-stu-id="66b3c-152">So, you can have a setup where:</span></span>

* <span data-ttu-id="66b3c-153">Cluster1 hello cestu můžete použít.`adl://mydatalakestore/cluster1storage`</span><span class="sxs-lookup"><span data-stu-id="66b3c-153">Cluster1 can use hello path `adl://mydatalakestore/cluster1storage`</span></span>
* <span data-ttu-id="66b3c-154">Cluster2 hello cestu můžete použít.`adl://mydatalakestore/cluster2storage`</span><span class="sxs-lookup"><span data-stu-id="66b3c-154">Cluster2 can use hello path `adl://mydatalakestore/cluster2storage`</span></span>

<span data-ttu-id="66b3c-155">Všimněte si, že obě hello clustery pomocí hello stejný účet Data Lake Store **mydatalakestore**.</span><span class="sxs-lookup"><span data-stu-id="66b3c-155">Notice that both hello clusters use hello same Data Lake Store account **mydatalakestore**.</span></span> <span data-ttu-id="66b3c-156">Každý cluster má přístup tooits vlastní kořenové systému souborů v Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="66b3c-156">Each cluster has access tooits own root filesystem in Data Lake Store.</span></span> <span data-ttu-id="66b3c-157">Hello prostředí nasazení portálu Azure na konkrétní vyzve vás toouse název složky, jako **/clusters/\<clustername >** pro kořenovou cestu hello.</span><span class="sxs-lookup"><span data-stu-id="66b3c-157">hello Azure portal deployment experience in particular prompts you toouse a folder name such as **/clusters/\<clustername>** for hello root path.</span></span>

<span data-ttu-id="66b3c-158">toobe možné toouse jako výchozí úložiště Data Lake Store, musí udělit hello služby hlavní přístup toohello následující cesty:</span><span class="sxs-lookup"><span data-stu-id="66b3c-158">toobe able toouse a Data Lake Store as default storage, you must grant hello service principal access toohello following paths:</span></span>

- <span data-ttu-id="66b3c-159">Hello kořenového účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="66b3c-159">hello Data Lake Store account root.</span></span>  <span data-ttu-id="66b3c-160">Například: adl://mydatalakestore/.</span><span class="sxs-lookup"><span data-stu-id="66b3c-160">For example: adl://mydatalakestore/.</span></span>
- <span data-ttu-id="66b3c-161">Hello složka pro všechny složky clusteru.</span><span class="sxs-lookup"><span data-stu-id="66b3c-161">hello folder for all cluster folders.</span></span>  <span data-ttu-id="66b3c-162">Například: adl://mydatalakestore/clusters.</span><span class="sxs-lookup"><span data-stu-id="66b3c-162">For example: adl://mydatalakestore/clusters.</span></span>
- <span data-ttu-id="66b3c-163">Hello složka pro hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="66b3c-163">hello folder for hello cluster.</span></span>  <span data-ttu-id="66b3c-164">Například: adl://mydatalakestore/clusters/cluster1storage.</span><span class="sxs-lookup"><span data-stu-id="66b3c-164">For example: adl://mydatalakestore/clusters/cluster1storage.</span></span>

<span data-ttu-id="66b3c-165">Další informace o vytvoření instančního objektu a udělení přístupu najdete v části [Konfigurace přístupu ke službě Data Lake Store](#configure-data-lake-store-access).</span><span class="sxs-lookup"><span data-stu-id="66b3c-165">For more information for creating service principal and grant access, see [Configure Data Lake store access](#configure-data-lake-store-access).</span></span>


## <a name="use-data-lake-store-as-additional-storage"></a><span data-ttu-id="66b3c-166">Použití služby Data Lake Store jako dalšího úložiště</span><span class="sxs-lookup"><span data-stu-id="66b3c-166">Use Data Lake Store as additional storage</span></span>

<span data-ttu-id="66b3c-167">Data Lake Store můžete použít jako další úložiště pro cluster hello také.</span><span class="sxs-lookup"><span data-stu-id="66b3c-167">You can use Data Lake Store as additional storage for hello cluster as well.</span></span> <span data-ttu-id="66b3c-168">V takových případech hello clusteru výchozí úložiště může být objektu Blob úložiště Azure nebo účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="66b3c-168">In such cases, hello cluster default storage can either be an Azure Storage Blob or a Data Lake Store account.</span></span> <span data-ttu-id="66b3c-169">Pokud používáte úloh HDInsight proti hello data uložená v Data Lake Store jako další úložiště, musíte použít soubory toohello hello plně kvalifikovanou cestu.</span><span class="sxs-lookup"><span data-stu-id="66b3c-169">If you are running HDInsight jobs against hello data stored in Data Lake Store as additional storage, you must use hello fully-qualified path toohello files.</span></span> <span data-ttu-id="66b3c-170">Například:</span><span class="sxs-lookup"><span data-stu-id="66b3c-170">For example:</span></span>

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

<span data-ttu-id="66b3c-171">Všimněte si, že neexistuje žádná **cluster_root_path** v adrese URL hello teď.</span><span class="sxs-lookup"><span data-stu-id="66b3c-171">Note that there's no **cluster_root_path** in hello URL now.</span></span> <span data-ttu-id="66b3c-172">Je to způsobeno Data Lake Store není výchozí úložiště v tomto případě tak, aby všechny potřebné toodo zadejte hello cesta toohello soubory.</span><span class="sxs-lookup"><span data-stu-id="66b3c-172">That's because Data Lake Store is not a default storage in this case so all you need toodo is provide hello path toohello files.</span></span>

<span data-ttu-id="66b3c-173">toobe možné toouse Data Lake Store jako další úložiště, potřebujete jenom toogrant hello služby hlavní toohello cesty přístupu níž jsou uloženy soubory.</span><span class="sxs-lookup"><span data-stu-id="66b3c-173">toobe able toouse a Data Lake Store as additional storage, you only need toogrant hello service principal access toohello paths where your files are stored.</span></span>  <span data-ttu-id="66b3c-174">Například:</span><span class="sxs-lookup"><span data-stu-id="66b3c-174">For example:</span></span>

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

<span data-ttu-id="66b3c-175">Další informace o vytvoření instančního objektu a udělení přístupu najdete v části [Konfigurace přístupu ke službě Data Lake Store](#configure-data-lake-store-access).</span><span class="sxs-lookup"><span data-stu-id="66b3c-175">For more information for creating service principal and grant access, see [Configure Data Lake store access](#configure-data-lake-store-access).</span></span>


## <a name="use-more-than-one-data-lake-store-accounts"></a><span data-ttu-id="66b3c-176">Použití více účtů Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="66b3c-176">Use more than one Data Lake Store accounts</span></span>

<span data-ttu-id="66b3c-177">Přidání účtu Data Lake Store jako další a přidání více než jeden Data Lake Store jsou účty dosáhnout tím, že oprávnění clusteru HDInsight hello na data v jedné nebo více účtů Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="66b3c-177">Adding a Data Lake Store account as additional and adding more than one Data Lake Store accounts are accomplished by giving hello HDInsight cluster permission on data in one ore more Data Lake Store accounts.</span></span> <span data-ttu-id="66b3c-178">Viz [Konfigurace přístupu ke službě Data Lake Store](#configure-data-lake-store-access).</span><span class="sxs-lookup"><span data-stu-id="66b3c-178">See [Configure Data Lake Store access](#configure-data-lake-store-access).</span></span>

## <a name="configure-data-lake-store-access"></a><span data-ttu-id="66b3c-179">Konfigurace přístupu ke službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="66b3c-179">Configure Data Lake store access</span></span>

<span data-ttu-id="66b3c-180">tooconfigure Data Lake store přístup z clusteru HDInsight, musí mít Azure Active directory (Azure AD) instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="66b3c-180">tooconfigure Data Lake store access from your HDInsight cluster, you must have an Azure Active directory (Azure AD) service principal.</span></span> <span data-ttu-id="66b3c-181">Instanční objekt může vytvořit pouze správce Azure AD.</span><span class="sxs-lookup"><span data-stu-id="66b3c-181">Only an Azure AD administrator can create a service principal.</span></span> <span data-ttu-id="66b3c-182">Hello instanční objekt musí být vytvořeny s certifikátem.</span><span class="sxs-lookup"><span data-stu-id="66b3c-182">hello service principal must be created with a certificate.</span></span> <span data-ttu-id="66b3c-183">Další informace najdete v části [Konfigurace přístupu ke službě Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#configure-data-lake-store-access) a [Vytvoření instančního objektu s certifikátem podepsaným svým držitelem](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).</span><span class="sxs-lookup"><span data-stu-id="66b3c-183">For more information, see [Configure Data Lake Store access](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#configure-data-lake-store-access), and [Create service principal with self-signed-certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).</span></span>

> [!NOTE]
> <span data-ttu-id="66b3c-184">Pokud chcete toouse Azure Data Lake Store jako další úložiště pro HDInsight cluster, důrazně doporučujeme, abyste to provedli při vytváření clusteru hello, jak je popsáno v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="66b3c-184">If you are going toouse Azure Data Lake Store as additional storage for HDInsight cluster, we strongly recommend that you do this while you create hello cluster as described in this article.</span></span> <span data-ttu-id="66b3c-185">Přidání Azure Data Lake Store jako další úložiště tooan stávající cluster HDInsight je složitý proces a tooerrors náchylné k chybám.</span><span class="sxs-lookup"><span data-stu-id="66b3c-185">Adding Azure Data Lake Store as additional storage tooan existing HDInsight cluster is a complicated process and prone tooerrors.</span></span>
>

## <a name="access-files-from-hello-cluster"></a><span data-ttu-id="66b3c-186">Přístup k souborům z clusteru hello</span><span class="sxs-lookup"><span data-stu-id="66b3c-186">Access files from hello cluster</span></span>

<span data-ttu-id="66b3c-187">Existuje několik způsobů hello souborů v Data Lake Store můžete přistupovat z clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="66b3c-187">There are several ways you can access hello files in Data Lake Store from an HDInsight cluster.</span></span>

* <span data-ttu-id="66b3c-188">**Pomocí plně kvalifikovaného názvu hello**.</span><span class="sxs-lookup"><span data-stu-id="66b3c-188">**Using hello fully qualified name**.</span></span> <span data-ttu-id="66b3c-189">S tímto přístupem, můžete zadat hello úplná cesta toohello souboru, které chcete tooaccess.</span><span class="sxs-lookup"><span data-stu-id="66b3c-189">With this approach, you provide hello full path toohello file that you want tooaccess.</span></span>

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* <span data-ttu-id="66b3c-190">**Pomocí formát zkrácení cesty hello**.</span><span class="sxs-lookup"><span data-stu-id="66b3c-190">**Using hello shortened path format**.</span></span> <span data-ttu-id="66b3c-191">S tímto přístupem je nahradit hello cesty až kořenový cluster toohello adl: / / /.</span><span class="sxs-lookup"><span data-stu-id="66b3c-191">With this approach, you replace hello path up toohello cluster root with adl:///.</span></span> <span data-ttu-id="66b3c-192">V předchozím příkladu hello, takže můžete nahradit `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` s `adl:///`.</span><span class="sxs-lookup"><span data-stu-id="66b3c-192">So, in hello example above, you can replace `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` with `adl:///`.</span></span>

        adl:///<file path>

* <span data-ttu-id="66b3c-193">**Pomocí relativní cesty hello**.</span><span class="sxs-lookup"><span data-stu-id="66b3c-193">**Using hello relative path**.</span></span> <span data-ttu-id="66b3c-194">S tímto přístupem zadáte pouze toohello hello relativní cestě k souboru, které chcete tooaccess.</span><span class="sxs-lookup"><span data-stu-id="66b3c-194">With this approach, you only provide hello relative path toohello file that you want tooaccess.</span></span> <span data-ttu-id="66b3c-195">Například, pokud je soubor toohello hello úplnou cestu:</span><span class="sxs-lookup"><span data-stu-id="66b3c-195">For example, if hello complete path toohello file is:</span></span>

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    <span data-ttu-id="66b3c-196">Dostanete hello stejný soubor sample.log pomocí tuto relativní cestu.</span><span class="sxs-lookup"><span data-stu-id="66b3c-196">You can access hello same sample.log file by using this relative path instead.</span></span>

        /example/data/sample.log

## <a name="create-hdinsight-clusters-with-access-toodata-lake-store"></a><span data-ttu-id="66b3c-197">Vytvoření clusterů HDInsight pomocí získat přístup k tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="66b3c-197">Create HDInsight clusters with access tooData Lake Store</span></span>

<span data-ttu-id="66b3c-198">Použijte následující odkazy pro podrobné pokyny v přístupu tooData Lake Store toocreate, které clusterů HDInsight pomocí hello.</span><span class="sxs-lookup"><span data-stu-id="66b3c-198">Use hello following links for detailed instructions on how toocreate HDInsight clusters with access tooData Lake Store.</span></span>

* [<span data-ttu-id="66b3c-199">Pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="66b3c-199">Using Portal</span></span>](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
* [<span data-ttu-id="66b3c-200">Pomocí PowerShellu (se službou Data Lake Store jako výchozím úložištěm)</span><span class="sxs-lookup"><span data-stu-id="66b3c-200">Using PowerShell (with Data Lake Store as default storage)</span></span>](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [<span data-ttu-id="66b3c-201">Pomocí PowerShellu (se službou Data Lake Store jako dalším úložištěm)</span><span class="sxs-lookup"><span data-stu-id="66b3c-201">Using PowerShell (with Data Lake Store as additional storage)</span></span>](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [<span data-ttu-id="66b3c-202">Pomocí šablon Azure</span><span class="sxs-lookup"><span data-stu-id="66b3c-202">Using Azure templates</span></span>](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)


## <a name="next-steps"></a><span data-ttu-id="66b3c-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66b3c-203">Next steps</span></span>
<span data-ttu-id="66b3c-204">V tomto článku jste se naučili jak toouse HDFS kompatibilní s Azure Data Lake Store s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="66b3c-204">In this article, you learned how toouse HDFS-compatible Azure Data Lake Store with HDInsight.</span></span> <span data-ttu-id="66b3c-205">To vám umožní toobuild, škálovatelnou a dlouhodobé, archivace řešení pro získávání dat a používání HDInsight toounlock hello informací uvnitř hello uložené strukturovaných a nestrukturovaných dat.</span><span class="sxs-lookup"><span data-stu-id="66b3c-205">This allows you toobuild scalable, long-term, archiving data acquisition solutions and use HDInsight toounlock hello information inside hello stored structured and unstructured data.</span></span>

<span data-ttu-id="66b3c-206">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="66b3c-206">For more information, see:</span></span>

* <span data-ttu-id="66b3c-207">[Začínáme se službou Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="66b3c-207">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* [<span data-ttu-id="66b3c-208">Začínáme se službou Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="66b3c-208">Get started with Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-get-started-portal.md)
* <span data-ttu-id="66b3c-209">[Nahrání dat tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="66b3c-209">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="66b3c-210">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="66b3c-210">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="66b3c-211">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="66b3c-211">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="66b3c-212">[Použijte Azure Storage sdílené přístupové podpisy toorestrict přístup toodata s HDInsight][hdinsight-use-sas]</span><span class="sxs-lookup"><span data-stu-id="66b3c-212">[Use Azure Storage Shared Access Signatures toorestrict access toodata with HDInsight][hdinsight-use-sas]</span></span>

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
