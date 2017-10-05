---
title: "Řešení potíží Storm pomocí Azure HDInsight | Microsoft Docs"
description: "Získejte odpovědi na časté otázky týkající se používání Apache Storm s Azure HDInsight."
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
ms.openlocfilehash: 70a3d762431d90acdd6ed2a432a569f34d0ce447
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-storm-by-using-azure-hdinsight"></a><span data-ttu-id="ad733-104">Řešení potíží Storm pomocí Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="ad733-104">Troubleshoot Storm by using Azure HDInsight</span></span>

<span data-ttu-id="ad733-105">Další informace o hlavních problémů a jejich řešení pro práci s Apache Storm datové části v Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="ad733-105">Learn about the top issues and their resolutions for working with Apache Storm payloads in Apache Ambari.</span></span>

## <a name="how-do-i-access-the-storm-ui-on-a-cluster"></a><span data-ttu-id="ad733-106">Jak získám přístup k rozhraní Storm v clusteru</span><span class="sxs-lookup"><span data-stu-id="ad733-106">How do I access the Storm UI on a cluster</span></span>
<span data-ttu-id="ad733-107">Máte dvě možnosti pro přístup k rozhraní Storm z prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="ad733-107">You have two options for accessing the Storm UI from a browser:</span></span>

### <a name="ambari-ui"></a><span data-ttu-id="ad733-108">Uživatelské rozhraní Ambari</span><span class="sxs-lookup"><span data-stu-id="ad733-108">Ambari UI</span></span>
1. <span data-ttu-id="ad733-109">Přejděte na řídicí panel Ambari.</span><span class="sxs-lookup"><span data-stu-id="ad733-109">Go to the Ambari dashboard.</span></span>
2. <span data-ttu-id="ad733-110">V seznamu služeb vyberte **Storm**.</span><span class="sxs-lookup"><span data-stu-id="ad733-110">In the list of services, select **Storm**.</span></span>
3. <span data-ttu-id="ad733-111">V **rychlé odkazy** nabídce vyberte možnost **uživatelské rozhraní Storm**.</span><span class="sxs-lookup"><span data-stu-id="ad733-111">In the **Quick Links** menu, select **Storm UI**.</span></span>

### <a name="direct-link"></a><span data-ttu-id="ad733-112">Přímý odkaz</span><span class="sxs-lookup"><span data-stu-id="ad733-112">Direct link</span></span>
<span data-ttu-id="ad733-113">Uživatelské rozhraní Storm na následující adrese URL se můžete dostat:</span><span class="sxs-lookup"><span data-stu-id="ad733-113">You can access the Storm UI at the following URL:</span></span>

<span data-ttu-id="ad733-114">https://\<název DNS clusteru\>/stormui</span><span class="sxs-lookup"><span data-stu-id="ad733-114">https://\<cluster DNS name\>/stormui</span></span>

<span data-ttu-id="ad733-115">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ad733-115">Example:</span></span>

 <span data-ttu-id="ad733-116">https://stormcluster.azurehdinsight.NET/stormui</span><span class="sxs-lookup"><span data-stu-id="ad733-116">https://stormcluster.azurehdinsight.net/stormui</span></span>

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another"></a><span data-ttu-id="ad733-117">Jak převést Storm událostí hub spout kontrolního bodu informace z jednoho topologie do jiného</span><span class="sxs-lookup"><span data-stu-id="ad733-117">How do I transfer Storm event hub spout checkpoint information from one topology to another</span></span>

<span data-ttu-id="ad733-118">Když budete vyvíjet topologie, které čtou z Azure Event Hubs pomocí centra událostí HDInsight Storm spout souboru .jar, musíte nasadit topologii, která má stejný název na novém clusteru.</span><span class="sxs-lookup"><span data-stu-id="ad733-118">When you develop topologies that read from Azure Event Hubs by using the HDInsight Storm event hub spout .jar file, you must deploy a topology that has the same name on a new cluster.</span></span> <span data-ttu-id="ad733-119">Však musí zachovat data kontrolního bodu, která byla zapsána do Apache ZooKeeper v původním clusteru.</span><span class="sxs-lookup"><span data-stu-id="ad733-119">However, you must retain the checkpoint data that was committed to Apache ZooKeeper on the old cluster.</span></span>

### <a name="where-checkpoint-data-is-stored"></a><span data-ttu-id="ad733-120">Uložení dat kontrolního bodu</span><span class="sxs-lookup"><span data-stu-id="ad733-120">Where checkpoint data is stored</span></span>
<span data-ttu-id="ad733-121">Data kontrolního bodu pro odsazení jsou uložena ve funkcích spout centra událostí v ZooKeeper v dvě kořenové cesty:</span><span class="sxs-lookup"><span data-stu-id="ad733-121">Checkpoint data for offsets is stored by the event hub spout in ZooKeeper in two root paths:</span></span>
- <span data-ttu-id="ad733-122">Netransakční spout kontrolní body jsou uloženy v /eventhubspout.</span><span class="sxs-lookup"><span data-stu-id="ad733-122">Nontransactional spout checkpoints are stored in /eventhubspout.</span></span>
- <span data-ttu-id="ad733-123">Data kontrolního bodu transakcí spout se ukládají vstupně -transakcí.</span><span class="sxs-lookup"><span data-stu-id="ad733-123">Transactional spout checkpoint data is stored in /transactional.</span></span>

### <a name="how-to-restore"></a><span data-ttu-id="ad733-124">Postup obnovení</span><span class="sxs-lookup"><span data-stu-id="ad733-124">How to restore</span></span>
<span data-ttu-id="ad733-125">Skripty a knihovny, které slouží k exportu dat mimo ZooKeeper a poté importovat data zpět do ZooKeeper s novým názvem získáte v tématu [příklady Storm v HDInsight](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).</span><span class="sxs-lookup"><span data-stu-id="ad733-125">To get the scripts and libraries that you use to export data out of ZooKeeper and then import the data back to ZooKeeper with a new name, see [HDInsight Storm examples](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).</span></span>

<span data-ttu-id="ad733-126">Složku lib má .jar soubory, které obsahují implementaci pro operace exportu/importu.</span><span class="sxs-lookup"><span data-stu-id="ad733-126">The lib folder has .jar files that contain the implementation for the export/import operation.</span></span> <span data-ttu-id="ad733-127">Složka bash obsahuje ukázkový skript, který ukazuje, jak exportovat data ze serveru ZooKeeper v původním clusteru a importujte ho na server ZooKeeper v novém clusteru.</span><span class="sxs-lookup"><span data-stu-id="ad733-127">The bash folder has an example script that demonstrates how to export data from the ZooKeeper server on the old cluster, and then import it back to the ZooKeeper server on the new cluster.</span></span>

<span data-ttu-id="ad733-128">Spustit [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) skript z uzly ZooKeeper na exportovat a importovat data.</span><span class="sxs-lookup"><span data-stu-id="ad733-128">Run the [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) script from the ZooKeeper nodes to export and then import data.</span></span> <span data-ttu-id="ad733-129">Skript aktualizujte na správnou verzi Hortonworks Data Platform (HDP).</span><span class="sxs-lookup"><span data-stu-id="ad733-129">Update the script to the correct Hortonworks Data Platform (HDP) version.</span></span> <span data-ttu-id="ad733-130">(Tvrdě pracujeme na tom Obecné v prostředí HDInsight tyto skripty.</span><span class="sxs-lookup"><span data-stu-id="ad733-130">(We are working on making these scripts generic in HDInsight.</span></span> <span data-ttu-id="ad733-131">Obecné skripty můžete spustit z libovolného uzlu v clusteru bez úprav uživatelem.)</span><span class="sxs-lookup"><span data-stu-id="ad733-131">Generic scripts can run from any node on the cluster without modifications by the user.)</span></span>

<span data-ttu-id="ad733-132">Příkaz export zapíše metadata na cestu Apache Hadoop Distributed File System (HDFS) (v úložišti Azure Blob Storage nebo Azure Data Lake Store) v umístění, které nastavíte.</span><span class="sxs-lookup"><span data-stu-id="ad733-132">The export command writes the metadata to an Apache Hadoop Distributed File System (HDFS) path (in an Azure Blob Storage or Azure Data Lake Store store) at a location that you set.</span></span>

### <a name="examples"></a><span data-ttu-id="ad733-133">Příklady</span><span class="sxs-lookup"><span data-stu-id="ad733-133">Examples</span></span>

#### <a name="export-offset-metadata"></a><span data-ttu-id="ad733-134">Export metadat posunutí</span><span class="sxs-lookup"><span data-stu-id="ad733-134">Export offset metadata</span></span>
1. <span data-ttu-id="ad733-135">Přejděte do ZooKeeper clusteru v clusteru, ze kterého posun kontrolního bodu musí být exportován pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="ad733-135">Use SSH to go to the ZooKeeper cluster on the cluster from which the checkpoint offset needs to be exported.</span></span>
2. <span data-ttu-id="ad733-136">Spusťte následující příkaz pro export ZooKeeper posunutí data do cesty HDFS /stormmetadta/zkdata (po aktualizaci softwaru HDP řetězec verze):</span><span class="sxs-lookup"><span data-stu-id="ad733-136">Run the following command (after you update the HDP version string) to export ZooKeeper offset data to the /stormmetadta/zkdata HDFS path:</span></span>

    ```apache   
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a><span data-ttu-id="ad733-137">Import metadat posunutí</span><span class="sxs-lookup"><span data-stu-id="ad733-137">Import offset metadata</span></span>
1. <span data-ttu-id="ad733-138">Přejděte do ZooKeeper clusteru v clusteru, ze kterého posun kontrolního bodu musí být exportován pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="ad733-138">Use SSH to go to the ZooKeeper cluster on the cluster from which the checkpoint offset needs to be exported.</span></span>
2. <span data-ttu-id="ad733-139">Spusťte následující příkaz pro import dat posunutí ZooKeeper z /stormmetadata/zkdata cesty HDFS ZooKeeper server v cílovém clusteru (po aktualizaci softwaru HDP řetězec verze):</span><span class="sxs-lookup"><span data-stu-id="ad733-139">Run the following command (after you update the HDP version string) to import ZooKeeper offset data from the HDFS path /stormmetadata/zkdata to the ZooKeeper server on the target cluster:</span></span>

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```
   
#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-the-beginning-or-from-a-timestamp-that-the-user-chooses"></a><span data-ttu-id="ad733-140">Odstranit posunutí metadata tak, aby topologie spustit od začátku, nebo ze časové razítko, které uživatel vybere zpracování dat</span><span class="sxs-lookup"><span data-stu-id="ad733-140">Delete offset metadata so that topologies can start processing data from the beginning, or from a timestamp that the user chooses</span></span>
1. <span data-ttu-id="ad733-141">Přejděte do ZooKeeper clusteru v clusteru, ze kterého posun kontrolního bodu musí být exportován pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="ad733-141">Use SSH to go to the ZooKeeper cluster on the cluster from which the checkpoint offset needs to be exported.</span></span>
2. <span data-ttu-id="ad733-142">Spusťte následující příkaz k odstranění všech dat posunutí ZooKeeper v aktuální clusteru (po aktualizaci softwaru HDP řetězec verze):</span><span class="sxs-lookup"><span data-stu-id="ad733-142">Run the following command (after you update the HDP version string) to delete all ZooKeeper offset data in the current cluster:</span></span>

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a><span data-ttu-id="ad733-143">Jak najít binární soubory Storm v clusteru</span><span class="sxs-lookup"><span data-stu-id="ad733-143">How do I locate Storm binaries on a cluster</span></span>
<span data-ttu-id="ad733-144">Storm binární soubory pro aktuální zásobník HDP jsou /usr/hdp/current/storm-client.</span><span class="sxs-lookup"><span data-stu-id="ad733-144">Storm binaries for the current HDP stack are in /usr/hdp/current/storm-client.</span></span> <span data-ttu-id="ad733-145">Umístění je stejný pro hlavních uzlech i pro uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="ad733-145">The location is the same both for head nodes and for worker nodes.</span></span>
 
<span data-ttu-id="ad733-146">Může existovat více binární soubory pro konkrétní verze softwaru HDP v /usr/hdp (například /usr/hdp/2.5.0.1233/storm).</span><span class="sxs-lookup"><span data-stu-id="ad733-146">There might be multiple binaries for specific HDP versions in /usr/hdp (for example, /usr/hdp/2.5.0.1233/storm).</span></span> <span data-ttu-id="ad733-147">Složka /usr/hdp/current/storm-client je symlinked na nejnovější verzi, která běží na clusteru.</span><span class="sxs-lookup"><span data-stu-id="ad733-147">The /usr/hdp/current/storm-client folder is symlinked to the latest version that is running on the cluster.</span></span>

<span data-ttu-id="ad733-148">Další informace najdete v tématu [připojit ke clusteru HDInsight pomocí protokolu SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) a [Storm](http://storm.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ad733-148">For more information, see [Connect to an HDInsight cluster by using SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) and [Storm](http://storm.apache.org/).</span></span>
 
## <a name="how-do-i-determine-the-deployment-topology-of-a-storm-cluster"></a><span data-ttu-id="ad733-149">Jak je možné zjistit topologii nasazení clusteru Storm</span><span class="sxs-lookup"><span data-stu-id="ad733-149">How do I determine the deployment topology of a Storm cluster</span></span>
<span data-ttu-id="ad733-150">Nejdřív určete všechny součásti, které jsou nainstalované s HDInsight Storm.</span><span class="sxs-lookup"><span data-stu-id="ad733-150">First, identify all components that are installed with HDInsight Storm.</span></span> <span data-ttu-id="ad733-151">Storm cluster se skládá ze čtyř kategorií uzlu:</span><span class="sxs-lookup"><span data-stu-id="ad733-151">A Storm cluster consists of four node categories:</span></span>

* <span data-ttu-id="ad733-152">Uzly brány</span><span class="sxs-lookup"><span data-stu-id="ad733-152">Gateway nodes</span></span>
* <span data-ttu-id="ad733-153">hlavních uzlech</span><span class="sxs-lookup"><span data-stu-id="ad733-153">Head nodes</span></span>
* <span data-ttu-id="ad733-154">Uzly zooKeeper</span><span class="sxs-lookup"><span data-stu-id="ad733-154">ZooKeeper nodes</span></span>
* <span data-ttu-id="ad733-155">Pracovní uzly</span><span class="sxs-lookup"><span data-stu-id="ad733-155">Worker nodes</span></span>
 
### <a name="gateway-nodes"></a><span data-ttu-id="ad733-156">Uzly brány</span><span class="sxs-lookup"><span data-stu-id="ad733-156">Gateway nodes</span></span>
<span data-ttu-id="ad733-157">Uzel brány je služba reverzní proxy server, která umožňuje veřejný přístup k aktivní službě správy Ambari a brány.</span><span class="sxs-lookup"><span data-stu-id="ad733-157">A gateway node is a gateway and reverse proxy service that enables public access to an active Ambari management service.</span></span> <span data-ttu-id="ad733-158">Také obstará Ambari vedoucí volba.</span><span class="sxs-lookup"><span data-stu-id="ad733-158">It also handles Ambari leader election.</span></span>
 
### <a name="head-nodes"></a><span data-ttu-id="ad733-159">hlavních uzlech</span><span class="sxs-lookup"><span data-stu-id="ad733-159">Head nodes</span></span>
<span data-ttu-id="ad733-160">Storm hlavních uzlech spusťte následující služby:</span><span class="sxs-lookup"><span data-stu-id="ad733-160">Storm head nodes run the following services:</span></span>
* <span data-ttu-id="ad733-161">Nimbus</span><span class="sxs-lookup"><span data-stu-id="ad733-161">Nimbus</span></span>
* <span data-ttu-id="ad733-162">Ambari serveru</span><span class="sxs-lookup"><span data-stu-id="ad733-162">Ambari server</span></span>
* <span data-ttu-id="ad733-163">Ambari metriky serveru</span><span class="sxs-lookup"><span data-stu-id="ad733-163">Ambari Metrics server</span></span>
* <span data-ttu-id="ad733-164">Kolekce Ambari metriky</span><span class="sxs-lookup"><span data-stu-id="ad733-164">Ambari Metrics Collector</span></span>
 
### <a name="zookeeper-nodes"></a><span data-ttu-id="ad733-165">Uzly zooKeeper</span><span class="sxs-lookup"><span data-stu-id="ad733-165">ZooKeeper nodes</span></span>
<span data-ttu-id="ad733-166">HDInsight se dodává s třemi uzly ZooKeeper kvora.</span><span class="sxs-lookup"><span data-stu-id="ad733-166">HDInsight comes with a three-node ZooKeeper quorum.</span></span> <span data-ttu-id="ad733-167">Velikost kvora je pevná a nejde ho překonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="ad733-167">The quorum size is fixed, and cannot be reconfigured.</span></span>
 
<span data-ttu-id="ad733-168">Služby Storm v clusteru jsou nakonfigurovány k automatickému využití ZooKeeper kvora.</span><span class="sxs-lookup"><span data-stu-id="ad733-168">Storm services in the cluster are configured to automatically use the ZooKeeper quorum.</span></span>
 
### <a name="worker-nodes"></a><span data-ttu-id="ad733-169">Pracovní uzly</span><span class="sxs-lookup"><span data-stu-id="ad733-169">Worker nodes</span></span>
<span data-ttu-id="ad733-170">Storm uzlů pracovního procesu spouštění následujících služeb:</span><span class="sxs-lookup"><span data-stu-id="ad733-170">Storm worker nodes run the following services:</span></span>
* <span data-ttu-id="ad733-171">Dohledový uzel</span><span class="sxs-lookup"><span data-stu-id="ad733-171">Supervisor</span></span>
* <span data-ttu-id="ad733-172">Pracovní Java virtuálních počítačů (JVMs) pro spuštění topologie</span><span class="sxs-lookup"><span data-stu-id="ad733-172">Worker Java virtual machines (JVMs), for running topologies</span></span>
* <span data-ttu-id="ad733-173">Ambari agenta</span><span class="sxs-lookup"><span data-stu-id="ad733-173">Ambari agent</span></span>
 
## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a><span data-ttu-id="ad733-174">Jak najdu Storm binární soubory funkcí spout centra událostí pro vývoj</span><span class="sxs-lookup"><span data-stu-id="ad733-174">How do I locate Storm event hub spout binaries for development</span></span>
 
<span data-ttu-id="ad733-175">Další informace o pomocí topologie Storm event hub spout .jar soubory najdete v následujících zdrojích informací.</span><span class="sxs-lookup"><span data-stu-id="ad733-175">For more information about using Storm event hub spout .jar files with your topology, see the following resources.</span></span>
 
### <a name="java-based-topology"></a><span data-ttu-id="ad733-176">Topologie založené na jazyce Java</span><span class="sxs-lookup"><span data-stu-id="ad733-176">Java-based topology</span></span>
[<span data-ttu-id="ad733-177">Zpracování událostí z Azure Event Hubs se Storm v HDInsight (Java)</span><span class="sxs-lookup"><span data-stu-id="ad733-177">Process events from Azure Event Hubs with Storm on HDInsight (Java)</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-java-event-hub-topology)
 
### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a><span data-ttu-id="ad733-178">C# – na základě topologie (Mono u clusterů Linux Storm HDInsight 3.4 +)</span><span class="sxs-lookup"><span data-stu-id="ad733-178">C#-based topology (Mono on HDInsight 3.4+ Linux Storm clusters)</span></span>
[<span data-ttu-id="ad733-179">Zpracování událostí z Azure Event Hubs se Stormem v HDInsight (C#)</span><span class="sxs-lookup"><span data-stu-id="ad733-179">Process events from Azure Event Hubs with Storm on HDInsight (C#)</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)
 
### <a name="latest-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a><span data-ttu-id="ad733-180">Nejnovější Storm event hub spout binární soubory pro clustery Linux Storm HDInsight 3.5 +</span><span class="sxs-lookup"><span data-stu-id="ad733-180">Latest Storm event hub spout binaries for HDInsight 3.5+ Linux Storm clusters</span></span>
<span data-ttu-id="ad733-181">Naučte se používat nejnovější spout Storm události rozbočovače, který funguje s clustery Linux Storm HDInsight 3.5 +, najdete v tématu mvn úložišti [souboru readme](https://github.com/hdinsight/mvn-repo/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="ad733-181">To learn how to use the latest Storm event hub spout that works with HDInsight 3.5+ Linux Storm clusters, see the mvn-repo [readme file](https://github.com/hdinsight/mvn-repo/blob/master/README.md).</span></span>
 
### <a name="source-code-examples"></a><span data-ttu-id="ad733-182">Příklady zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="ad733-182">Source code examples</span></span>
<span data-ttu-id="ad733-183">V tématu [příklady](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) o tom, jak číst a zapisovat z centra událostí Azure pomocí topologií Apache Storm (napsanou v jazyce Java) v clusteru Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ad733-183">See [examples](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) of how to read and write from Azure Event Hub using an Apache Storm topology (written in Java) on an Azure HDInsight cluster.</span></span>
 
## <a name="how-do-i-locate-storm-log4j-configuration-files-on-clusters"></a><span data-ttu-id="ad733-184">Jak najdu Storm Log4J konfigurační soubory v clusterech</span><span class="sxs-lookup"><span data-stu-id="ad733-184">How do I locate Storm Log4J configuration files on clusters</span></span>
 
<span data-ttu-id="ad733-185">K identifikaci Storm služeb Apache Log4J konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="ad733-185">To identify Apache Log4J configuration files for Storm services.</span></span>
 
### <a name="on-head-nodes"></a><span data-ttu-id="ad733-186">Na hlavních uzlech</span><span class="sxs-lookup"><span data-stu-id="ad733-186">On head nodes</span></span>
<span data-ttu-id="ad733-187">Konfigurace Nimbus Log4J je pro čtení z USR/hdp/\<verze softwaru HDP\>/storm/log4j2/cluster.xml.</span><span class="sxs-lookup"><span data-stu-id="ad733-187">The Nimbus Log4J configuration is read from /usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span></span>
 
### <a name="on-worker-nodes"></a><span data-ttu-id="ad733-188">V pracovním uzlům</span><span class="sxs-lookup"><span data-stu-id="ad733-188">On worker nodes</span></span>
<span data-ttu-id="ad733-189">Supervisor Log4J konfigurace je pro čtení z USR/hdp/\<verze softwaru HDP\>/storm/log4j2/cluster.xml.</span><span class="sxs-lookup"><span data-stu-id="ad733-189">The supervisor Log4J configuration is read from /usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span></span>
 
<span data-ttu-id="ad733-190">Konfigurační soubor pracovního procesu Log4J je načten z USR/hdp/\<verze softwaru HDP\>/storm/log4j2/worker.xml.</span><span class="sxs-lookup"><span data-stu-id="ad733-190">The worker Log4J configuration file is read from /usr/hdp/\<HDP version\>/storm/log4j2/worker.xml.</span></span>
 
<span data-ttu-id="ad733-191">Příklady: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml</span><span class="sxs-lookup"><span data-stu-id="ad733-191">Examples: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml</span></span>

