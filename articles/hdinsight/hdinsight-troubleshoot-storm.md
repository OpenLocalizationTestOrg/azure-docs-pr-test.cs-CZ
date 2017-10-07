---
title: "aaaTroubleshoot Storm pomocí Azure HDInsight | Microsoft Docs"
description: "Získejte odpovědi toocommon dotazy týkající se používání Apache Storm v prostředí Azure HDInsight."
keywords: "Azure HDInsight, Storm, – nejčastější dotazy, řešení potíží s průvodce, běžné problémy"
services: Azure HDInsight
documentationcenter: na
author: raviperi
manager: 
editor: 
ms.assetid: 74E51183-3EF4-4C67-AA60-6E12FAC999B5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: raviperi
ms.openlocfilehash: 51bcb3dc28eff5ee7bb33252fb2ec71a88ed8e09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-storm-by-using-azure-hdinsight"></a><span data-ttu-id="7ef47-104">Řešení potíží Storm pomocí Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="7ef47-104">Troubleshoot Storm by using Azure HDInsight</span></span>

<span data-ttu-id="7ef47-105">Další informace o hello nejčastější problémy a jejich řešení pro práci s Apache Storm datové části v Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="7ef47-105">Learn about hello top issues and their resolutions for working with Apache Storm payloads in Apache Ambari.</span></span>

## <a name="how-do-i-access-hello-storm-ui-on-a-cluster"></a><span data-ttu-id="7ef47-106">Přístupu hello uživatelské rozhraní Storm v clusteru</span><span class="sxs-lookup"><span data-stu-id="7ef47-106">How do I access hello Storm UI on a cluster</span></span>
<span data-ttu-id="7ef47-107">Máte dvě možnosti pro přístup k hello uživatelské rozhraní Storm z prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="7ef47-107">You have two options for accessing hello Storm UI from a browser:</span></span>

### <a name="ambari-ui"></a><span data-ttu-id="7ef47-108">Uživatelské rozhraní Ambari</span><span class="sxs-lookup"><span data-stu-id="7ef47-108">Ambari UI</span></span>
1. <span data-ttu-id="7ef47-109">Přejděte toohello Ambari řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="7ef47-109">Go toohello Ambari dashboard.</span></span>
2. <span data-ttu-id="7ef47-110">V seznamu hello služeb vyberte **Storm**.</span><span class="sxs-lookup"><span data-stu-id="7ef47-110">In hello list of services, select **Storm**.</span></span>
3. <span data-ttu-id="7ef47-111">V hello **rychlé odkazy** nabídce vyberte možnost **uživatelské rozhraní Storm**.</span><span class="sxs-lookup"><span data-stu-id="7ef47-111">In hello **Quick Links** menu, select **Storm UI**.</span></span>

### <a name="direct-link"></a><span data-ttu-id="7ef47-112">Přímý odkaz</span><span class="sxs-lookup"><span data-stu-id="7ef47-112">Direct link</span></span>
<span data-ttu-id="7ef47-113">Máte přístup hello uživatelské rozhraní Storm v hello následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="7ef47-113">You can access hello Storm UI at hello following URL:</span></span>

<span data-ttu-id="7ef47-114">https://\<název DNS clusteru\>/stormui</span><span class="sxs-lookup"><span data-stu-id="7ef47-114">https://\<cluster DNS name\>/stormui</span></span>

<span data-ttu-id="7ef47-115">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7ef47-115">Example:</span></span>

 <span data-ttu-id="7ef47-116">https://stormcluster.azurehdinsight.NET/stormui</span><span class="sxs-lookup"><span data-stu-id="7ef47-116">https://stormcluster.azurehdinsight.net/stormui</span></span>

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-tooanother"></a><span data-ttu-id="7ef47-117">Jak převést Storm událostí hub spout kontrolního bodu informace z jednoho topologie tooanother</span><span class="sxs-lookup"><span data-stu-id="7ef47-117">How do I transfer Storm event hub spout checkpoint information from one topology tooanother</span></span>

<span data-ttu-id="7ef47-118">Když budete vyvíjet topologie, které čtou z Azure Event Hubs pomocí hello souboru .jar spout centra událostí HDInsight Storm, je nutné nasadit topologii, která má stejný název na novém clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="7ef47-118">When you develop topologies that read from Azure Event Hubs by using hello HDInsight Storm event hub spout .jar file, you must deploy a topology that has hello same name on a new cluster.</span></span> <span data-ttu-id="7ef47-119">Však musí zachovat data hello kontrolního bodu, která byla potvrzena tooApache ZooKeeper na původním clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="7ef47-119">However, you must retain hello checkpoint data that was committed tooApache ZooKeeper on hello old cluster.</span></span>

### <a name="where-checkpoint-data-is-stored"></a><span data-ttu-id="7ef47-120">Uložení dat kontrolního bodu</span><span class="sxs-lookup"><span data-stu-id="7ef47-120">Where checkpoint data is stored</span></span>
<span data-ttu-id="7ef47-121">Data kontrolního bodu pro odsazení se ukládají pomocí hello spout události rozbočovače v ZooKeeper v dvě kořenové cesty:</span><span class="sxs-lookup"><span data-stu-id="7ef47-121">Checkpoint data for offsets is stored by hello event hub spout in ZooKeeper in two root paths:</span></span>
- <span data-ttu-id="7ef47-122">Netransakční spout kontrolní body jsou uloženy v /eventhubspout.</span><span class="sxs-lookup"><span data-stu-id="7ef47-122">Nontransactional spout checkpoints are stored in /eventhubspout.</span></span>
- <span data-ttu-id="7ef47-123">Data kontrolního bodu transakcí spout se ukládají vstupně -transakcí.</span><span class="sxs-lookup"><span data-stu-id="7ef47-123">Transactional spout checkpoint data is stored in /transactional.</span></span>

### <a name="how-toorestore"></a><span data-ttu-id="7ef47-124">Jak toorestore</span><span class="sxs-lookup"><span data-stu-id="7ef47-124">How toorestore</span></span>
<span data-ttu-id="7ef47-125">tooget hello skripty a knihovny, můžete použít tooexport dat mimo ZooKeeper a následným importem hello back tooZooKeeper data s novým názvem, najdete v části [příklady Storm v HDInsight](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).</span><span class="sxs-lookup"><span data-stu-id="7ef47-125">tooget hello scripts and libraries that you use tooexport data out of ZooKeeper and then import hello data back tooZooKeeper with a new name, see [HDInsight Storm examples](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).</span></span>

<span data-ttu-id="7ef47-126">složku lib Hello má .jar soubory, které obsahují hello implementaci pro operace exportu/importu hello.</span><span class="sxs-lookup"><span data-stu-id="7ef47-126">hello lib folder has .jar files that contain hello implementation for hello export/import operation.</span></span> <span data-ttu-id="7ef47-127">Hello bash složka obsahuje ukázkový skript, který ukazuje, jak tooexport data z hello ZooKeeper server na původním clusteru hello a importujte ho back toohello ZooKeeper server na novém clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="7ef47-127">hello bash folder has an example script that demonstrates how tooexport data from hello ZooKeeper server on hello old cluster, and then import it back toohello ZooKeeper server on hello new cluster.</span></span>

<span data-ttu-id="7ef47-128">Spustit hello [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) skript z tooexport uzly ZooKeeper hello a pak importovat data.</span><span class="sxs-lookup"><span data-stu-id="7ef47-128">Run hello [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) script from hello ZooKeeper nodes tooexport and then import data.</span></span> <span data-ttu-id="7ef47-129">Aktualizace hello toohello správné Hortonworks Data Platform (HDP) verze skriptu.</span><span class="sxs-lookup"><span data-stu-id="7ef47-129">Update hello script toohello correct Hortonworks Data Platform (HDP) version.</span></span> <span data-ttu-id="7ef47-130">(Tvrdě pracujeme na tom Obecné v prostředí HDInsight tyto skripty.</span><span class="sxs-lookup"><span data-stu-id="7ef47-130">(We are working on making these scripts generic in HDInsight.</span></span> <span data-ttu-id="7ef47-131">Obecné skripty můžete spustit z libovolného uzlu v clusteru hello bez úprav uživatelem hello.)</span><span class="sxs-lookup"><span data-stu-id="7ef47-131">Generic scripts can run from any node on hello cluster without modifications by hello user.)</span></span>

<span data-ttu-id="7ef47-132">příkaz export Hello zapíše hello metadata tooan Apache Hadoop Distributed File System (HDFS) cesta (v úložišti Azure Blob Storage nebo Azure Data Lake Store) v umístění, které nastavíte.</span><span class="sxs-lookup"><span data-stu-id="7ef47-132">hello export command writes hello metadata tooan Apache Hadoop Distributed File System (HDFS) path (in an Azure Blob Storage or Azure Data Lake Store store) at a location that you set.</span></span>

### <a name="examples"></a><span data-ttu-id="7ef47-133">Příklady</span><span class="sxs-lookup"><span data-stu-id="7ef47-133">Examples</span></span>

#### <a name="export-offset-metadata"></a><span data-ttu-id="7ef47-134">Export metadat posunutí</span><span class="sxs-lookup"><span data-stu-id="7ef47-134">Export offset metadata</span></span>
1. <span data-ttu-id="7ef47-135">Používání SSH toogo toohello ZooKeeper clusteru v clusteru hello z které hello kontrolního bodu musí posun toobe exportovali.</span><span class="sxs-lookup"><span data-stu-id="7ef47-135">Use SSH toogo toohello ZooKeeper cluster on hello cluster from which hello checkpoint offset needs toobe exported.</span></span>
2. <span data-ttu-id="7ef47-136">Hello spusťte následující příkaz (po aktualizaci hello řetězec verze softwaru HDP) tooexport ZooKeeper Posun dat toohello /stormmetadta/zkdata HDFS cesta:</span><span class="sxs-lookup"><span data-stu-id="7ef47-136">Run hello following command (after you update hello HDP version string) tooexport ZooKeeper offset data toohello /stormmetadta/zkdata HDFS path:</span></span>

    ```apache   
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a><span data-ttu-id="7ef47-137">Import metadat posunutí</span><span class="sxs-lookup"><span data-stu-id="7ef47-137">Import offset metadata</span></span>
1. <span data-ttu-id="7ef47-138">Používání SSH toogo toohello ZooKeeper clusteru v clusteru hello z které hello kontrolního bodu musí posun toobe exportovali.</span><span class="sxs-lookup"><span data-stu-id="7ef47-138">Use SSH toogo toohello ZooKeeper cluster on hello cluster from which hello checkpoint offset needs toobe exported.</span></span>
2. <span data-ttu-id="7ef47-139">Spuštění hello následující příkaz (po aktualizaci řetězec verze softwaru HDP hello) tooimport ZooKeeper Posun dat z hello HDFS cesta/stormmetadata/zkdata toohello ZooKeeper serveru na cílový cluster hello:</span><span class="sxs-lookup"><span data-stu-id="7ef47-139">Run hello following command (after you update hello HDP version string) tooimport ZooKeeper offset data from hello HDFS path /stormmetadata/zkdata toohello ZooKeeper server on hello target cluster:</span></span>

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```
   
#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-hello-beginning-or-from-a-timestamp-that-hello-user-chooses"></a><span data-ttu-id="7ef47-140">Odstranit posunutí metadata tak, aby topologie můžete spustit zpracování dat z počáteční hello nebo z časovým razítkem zvolí tento uživatel hello</span><span class="sxs-lookup"><span data-stu-id="7ef47-140">Delete offset metadata so that topologies can start processing data from hello beginning, or from a timestamp that hello user chooses</span></span>
1. <span data-ttu-id="7ef47-141">Používání SSH toogo toohello ZooKeeper clusteru v clusteru hello z které hello kontrolního bodu musí posun toobe exportovali.</span><span class="sxs-lookup"><span data-stu-id="7ef47-141">Use SSH toogo toohello ZooKeeper cluster on hello cluster from which hello checkpoint offset needs toobe exported.</span></span>
2. <span data-ttu-id="7ef47-142">Spuštění hello následující příkaz (po aktualizaci řetězec verze softwaru HDP hello) toodelete všechny ZooKeeper Posun dat v aktuálním clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="7ef47-142">Run hello following command (after you update hello HDP version string) toodelete all ZooKeeper offset data in hello current cluster:</span></span>

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a><span data-ttu-id="7ef47-143">Jak najít binární soubory Storm v clusteru</span><span class="sxs-lookup"><span data-stu-id="7ef47-143">How do I locate Storm binaries on a cluster</span></span>
<span data-ttu-id="7ef47-144">Storm binární soubory pro aktuální zásobník HDP hello jsou /usr/hdp/current/storm-client.</span><span class="sxs-lookup"><span data-stu-id="7ef47-144">Storm binaries for hello current HDP stack are in /usr/hdp/current/storm-client.</span></span> <span data-ttu-id="7ef47-145">umístění Hello je hello stejné pro hlavních uzlech i pro uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="7ef47-145">hello location is hello same both for head nodes and for worker nodes.</span></span>
 
<span data-ttu-id="7ef47-146">Může existovat více binární soubory pro konkrétní verze softwaru HDP v /usr/hdp (například /usr/hdp/2.5.0.1233/storm).</span><span class="sxs-lookup"><span data-stu-id="7ef47-146">There might be multiple binaries for specific HDP versions in /usr/hdp (for example, /usr/hdp/2.5.0.1233/storm).</span></span> <span data-ttu-id="7ef47-147">Složka /usr/hdp/current/storm-client Hello je toohello symlinked nejnovější se verzi, která běží na clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="7ef47-147">hello /usr/hdp/current/storm-client folder is symlinked toohello latest version that is running on hello cluster.</span></span>

<span data-ttu-id="7ef47-148">Další informace najdete v tématu [clusteru HDInsight tooan připojit pomocí protokolu SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) a [Storm](http://storm.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7ef47-148">For more information, see [Connect tooan HDInsight cluster by using SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) and [Storm](http://storm.apache.org/).</span></span>
 
## <a name="how-do-i-determine-hello-deployment-topology-of-a-storm-cluster"></a><span data-ttu-id="7ef47-149">Jak zjistím hello topologie nasazení clusteru Storm</span><span class="sxs-lookup"><span data-stu-id="7ef47-149">How do I determine hello deployment topology of a Storm cluster</span></span>
<span data-ttu-id="7ef47-150">Nejdřív určete všechny součásti, které jsou nainstalované s HDInsight Storm.</span><span class="sxs-lookup"><span data-stu-id="7ef47-150">First, identify all components that are installed with HDInsight Storm.</span></span> <span data-ttu-id="7ef47-151">Storm cluster se skládá ze čtyř kategorií uzlu:</span><span class="sxs-lookup"><span data-stu-id="7ef47-151">A Storm cluster consists of four node categories:</span></span>

* <span data-ttu-id="7ef47-152">Uzly brány</span><span class="sxs-lookup"><span data-stu-id="7ef47-152">Gateway nodes</span></span>
* <span data-ttu-id="7ef47-153">hlavních uzlech</span><span class="sxs-lookup"><span data-stu-id="7ef47-153">Head nodes</span></span>
* <span data-ttu-id="7ef47-154">Uzly zooKeeper</span><span class="sxs-lookup"><span data-stu-id="7ef47-154">ZooKeeper nodes</span></span>
* <span data-ttu-id="7ef47-155">Pracovní uzly</span><span class="sxs-lookup"><span data-stu-id="7ef47-155">Worker nodes</span></span>
 
### <a name="gateway-nodes"></a><span data-ttu-id="7ef47-156">Uzly brány</span><span class="sxs-lookup"><span data-stu-id="7ef47-156">Gateway nodes</span></span>
<span data-ttu-id="7ef47-157">Uzel brány je brány a služba reverzní proxy server, která umožňuje veřejný přístup tooan active Ambari management service.</span><span class="sxs-lookup"><span data-stu-id="7ef47-157">A gateway node is a gateway and reverse proxy service that enables public access tooan active Ambari management service.</span></span> <span data-ttu-id="7ef47-158">Také obstará Ambari vedoucí volba.</span><span class="sxs-lookup"><span data-stu-id="7ef47-158">It also handles Ambari leader election.</span></span>
 
### <a name="head-nodes"></a><span data-ttu-id="7ef47-159">hlavních uzlech</span><span class="sxs-lookup"><span data-stu-id="7ef47-159">Head nodes</span></span>
<span data-ttu-id="7ef47-160">Storm hlavních uzlech spusťte hello následující služby:</span><span class="sxs-lookup"><span data-stu-id="7ef47-160">Storm head nodes run hello following services:</span></span>
* <span data-ttu-id="7ef47-161">Nimbus</span><span class="sxs-lookup"><span data-stu-id="7ef47-161">Nimbus</span></span>
* <span data-ttu-id="7ef47-162">Ambari serveru</span><span class="sxs-lookup"><span data-stu-id="7ef47-162">Ambari server</span></span>
* <span data-ttu-id="7ef47-163">Ambari metriky serveru</span><span class="sxs-lookup"><span data-stu-id="7ef47-163">Ambari Metrics server</span></span>
* <span data-ttu-id="7ef47-164">Kolekce Ambari metriky</span><span class="sxs-lookup"><span data-stu-id="7ef47-164">Ambari Metrics Collector</span></span>
 
### <a name="zookeeper-nodes"></a><span data-ttu-id="7ef47-165">Uzly zooKeeper</span><span class="sxs-lookup"><span data-stu-id="7ef47-165">ZooKeeper nodes</span></span>
<span data-ttu-id="7ef47-166">HDInsight se dodává s třemi uzly ZooKeeper kvora.</span><span class="sxs-lookup"><span data-stu-id="7ef47-166">HDInsight comes with a three-node ZooKeeper quorum.</span></span> <span data-ttu-id="7ef47-167">velikost kvora Hello je pevná a nejde ho překonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="7ef47-167">hello quorum size is fixed, and cannot be reconfigured.</span></span>
 
<span data-ttu-id="7ef47-168">Služby Storm v clusteru hello jsou nakonfigurované tooautomatically použití hello ZooKeeper kvora.</span><span class="sxs-lookup"><span data-stu-id="7ef47-168">Storm services in hello cluster are configured tooautomatically use hello ZooKeeper quorum.</span></span>
 
### <a name="worker-nodes"></a><span data-ttu-id="7ef47-169">Pracovní uzly</span><span class="sxs-lookup"><span data-stu-id="7ef47-169">Worker nodes</span></span>
<span data-ttu-id="7ef47-170">Pracovní uzly Storm spusťte hello následující služby:</span><span class="sxs-lookup"><span data-stu-id="7ef47-170">Storm worker nodes run hello following services:</span></span>
* <span data-ttu-id="7ef47-171">Dohledový uzel</span><span class="sxs-lookup"><span data-stu-id="7ef47-171">Supervisor</span></span>
* <span data-ttu-id="7ef47-172">Pracovní Java virtuálních počítačů (JVMs) pro spuštění topologie</span><span class="sxs-lookup"><span data-stu-id="7ef47-172">Worker Java virtual machines (JVMs), for running topologies</span></span>
* <span data-ttu-id="7ef47-173">Ambari agenta</span><span class="sxs-lookup"><span data-stu-id="7ef47-173">Ambari agent</span></span>
 
## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a><span data-ttu-id="7ef47-174">Jak najdu Storm binární soubory funkcí spout centra událostí pro vývoj</span><span class="sxs-lookup"><span data-stu-id="7ef47-174">How do I locate Storm event hub spout binaries for development</span></span>
 
<span data-ttu-id="7ef47-175">Další informace o pomocí topologie Storm event hub spout .jar soubory najdete v části hello následující prostředky.</span><span class="sxs-lookup"><span data-stu-id="7ef47-175">For more information about using Storm event hub spout .jar files with your topology, see hello following resources.</span></span>
 
### <a name="java-based-topology"></a><span data-ttu-id="7ef47-176">Topologie založené na jazyce Java</span><span class="sxs-lookup"><span data-stu-id="7ef47-176">Java-based topology</span></span>
[<span data-ttu-id="7ef47-177">Zpracování událostí z Azure Event Hubs se Storm v HDInsight (Java)</span><span class="sxs-lookup"><span data-stu-id="7ef47-177">Process events from Azure Event Hubs with Storm on HDInsight (Java)</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-java-event-hub-topology)
 
### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a><span data-ttu-id="7ef47-178">C# – na základě topologie (Mono u clusterů Linux Storm HDInsight 3.4 +)</span><span class="sxs-lookup"><span data-stu-id="7ef47-178">C#-based topology (Mono on HDInsight 3.4+ Linux Storm clusters)</span></span>
[<span data-ttu-id="7ef47-179">Zpracování událostí z Azure Event Hubs se Stormem v HDInsight (C#)</span><span class="sxs-lookup"><span data-stu-id="7ef47-179">Process events from Azure Event Hubs with Storm on HDInsight (C#)</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)
 
### <a name="latest-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a><span data-ttu-id="7ef47-180">Nejnovější Storm event hub spout binární soubory pro clustery Linux Storm HDInsight 3.5 +</span><span class="sxs-lookup"><span data-stu-id="7ef47-180">Latest Storm event hub spout binaries for HDInsight 3.5+ Linux Storm clusters</span></span>
<span data-ttu-id="7ef47-181">toolearn způsobu toouse hello nejnovější Storm událostí hub spout který funguje s HDInsight 3.5 + clusterů Storm se Linux, najdete v hello mvn úložišti [souboru readme](https://github.com/hdinsight/mvn-repo/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="7ef47-181">toolearn how toouse hello latest Storm event hub spout that works with HDInsight 3.5+ Linux Storm clusters, see hello mvn-repo [readme file](https://github.com/hdinsight/mvn-repo/blob/master/README.md).</span></span>
 
### <a name="source-code-examples"></a><span data-ttu-id="7ef47-182">Příklady zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="7ef47-182">Source code examples</span></span>
<span data-ttu-id="7ef47-183">V tématu [příklady](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) jak tooread a zápis z centra událostí Azure pomocí topologií Apache Storm (napsanou v jazyce Java) v clusteru Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7ef47-183">See [examples](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) of how tooread and write from Azure Event Hub using an Apache Storm topology (written in Java) on an Azure HDInsight cluster.</span></span>
 
## <a name="how-do-i-locate-storm-log4j-configuration-files-on-clusters"></a><span data-ttu-id="7ef47-184">Jak najdu Storm Log4J konfigurační soubory v clusterech</span><span class="sxs-lookup"><span data-stu-id="7ef47-184">How do I locate Storm Log4J configuration files on clusters</span></span>
 
<span data-ttu-id="7ef47-185">tooidentify Apache Log4J konfigurační soubory pro služby Storm.</span><span class="sxs-lookup"><span data-stu-id="7ef47-185">tooidentify Apache Log4J configuration files for Storm services.</span></span>
 
### <a name="on-head-nodes"></a><span data-ttu-id="7ef47-186">Na hlavních uzlech</span><span class="sxs-lookup"><span data-stu-id="7ef47-186">On head nodes</span></span>
<span data-ttu-id="7ef47-187">Konfigurace Hello Nimbus Log4J je pro čtení z USR/hdp/\<verze softwaru HDP\>/storm/log4j2/cluster.xml.</span><span class="sxs-lookup"><span data-stu-id="7ef47-187">hello Nimbus Log4J configuration is read from /usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span></span>
 
### <a name="on-worker-nodes"></a><span data-ttu-id="7ef47-188">V pracovním uzlům</span><span class="sxs-lookup"><span data-stu-id="7ef47-188">On worker nodes</span></span>
<span data-ttu-id="7ef47-189">Hello nadřízeného Log4J konfigurace je pro čtení z USR/hdp/\<verze softwaru HDP\>/storm/log4j2/cluster.xml.</span><span class="sxs-lookup"><span data-stu-id="7ef47-189">hello supervisor Log4J configuration is read from /usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span></span>
 
<span data-ttu-id="7ef47-190">Hello pracovní Log4J konfigurační soubor je načten z USR/hdp/\<verze softwaru HDP\>/storm/log4j2/worker.xml.</span><span class="sxs-lookup"><span data-stu-id="7ef47-190">hello worker Log4J configuration file is read from /usr/hdp/\<HDP version\>/storm/log4j2/worker.xml.</span></span>
 
<span data-ttu-id="7ef47-191">Příklady: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml</span><span class="sxs-lookup"><span data-stu-id="7ef47-191">Examples: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml</span></span>

