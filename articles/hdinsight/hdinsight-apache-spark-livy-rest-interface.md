---
title: "cluster tooSpark aaaUse Livy Spark toosubmit úlohy v Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse Apache Spark REST API toosubmit Spark úlohy vzdáleně tooan cluster Azure HDInsight."
keywords: Apache spark rest api, livy spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2817b779-1594-486b-8759-489379ca907d
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: nitinme
ms.openlocfilehash: 417213b5f1db1522373188002fe05117faea5243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-spark-rest-api-toosubmit-remote-jobs-tooan-hdinsight-spark-cluster"></a><span data-ttu-id="757aa-104">Použití Apache Spark REST API toosubmit vzdálené úlohy tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="757aa-104">Use Apache Spark REST API toosubmit remote jobs tooan HDInsight Spark cluster</span></span>

<span data-ttu-id="757aa-105">Zjistěte, jak toouse Livy hello Apache Spark REST API, které je použité toosubmit vzdálené úlohy tooan clusteru Azure HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="757aa-105">Learn how toouse Livy, hello Apache Spark REST API, which is used toosubmit remote jobs tooan Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="757aa-106">Podrobnou dokumentaci najdete v tématu [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).</span><span class="sxs-lookup"><span data-stu-id="757aa-106">For detailed documentation, see [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).</span></span>

<span data-ttu-id="757aa-107">Můžete použít Livy toorun interaktivní Spark nutný nebo odeslání dávky toobe úlohy spusťte na Spark.</span><span class="sxs-lookup"><span data-stu-id="757aa-107">You can use Livy toorun interactive Spark shells or submit batch jobs toobe run on Spark.</span></span> <span data-ttu-id="757aa-108">V tomto článku bude zmíněn pomocí Livy toosubmit dávkových úloh.</span><span class="sxs-lookup"><span data-stu-id="757aa-108">This article talks about using Livy toosubmit batch jobs.</span></span> <span data-ttu-id="757aa-109">Hello fragmenty kódu v tomto článku pomocí cURL toomake koncový bod REST API volání toohello Livy Spark.</span><span class="sxs-lookup"><span data-stu-id="757aa-109">hello snippets in this article use cURL toomake REST API calls toohello Livy Spark endpoint.</span></span>

<span data-ttu-id="757aa-110">**Požadavky:**</span><span class="sxs-lookup"><span data-stu-id="757aa-110">**Prerequisites:**</span></span>

* <span data-ttu-id="757aa-111">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="757aa-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="757aa-112">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="757aa-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

* <span data-ttu-id="757aa-113">[cURL](http://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="757aa-113">[cURL](http://curl.haxx.se/).</span></span> <span data-ttu-id="757aa-114">Tento článek používá cURL toodemonstrate, jakým způsobem volá toomake REST API proti clusteru služby HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="757aa-114">This article uses cURL toodemonstrate how toomake REST API calls against an HDInsight Spark cluster.</span></span>

## <a name="submit-a-livy-spark-batch-job"></a><span data-ttu-id="757aa-115">Odeslání Livy Spark dávkovou úlohu</span><span class="sxs-lookup"><span data-stu-id="757aa-115">Submit a Livy Spark batch job</span></span>
<span data-ttu-id="757aa-116">Před odesláním dávkovou úlohu, musíte nahrát hello jar aplikace v úložišti clusteru hello přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="757aa-116">Before you submit a batch job, you must upload hello application jar on hello cluster storage associated with hello cluster.</span></span> <span data-ttu-id="757aa-117">Můžete použít [ **AzCopy**](../storage/common/storage-use-azcopy.md), nástroje příkazového řádku, toodo tak.</span><span class="sxs-lookup"><span data-stu-id="757aa-117">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command-line utility, toodo so.</span></span> <span data-ttu-id="757aa-118">Existují různé klienty tooupload data můžete použít.</span><span class="sxs-lookup"><span data-stu-id="757aa-118">There are various other clients you can use tooupload data.</span></span> <span data-ttu-id="757aa-119">Můžete najít další informace o nich v [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="757aa-119">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path tooapplication jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

<span data-ttu-id="757aa-120">**Příklady**:</span><span class="sxs-lookup"><span data-stu-id="757aa-120">**Examples**:</span></span>

* <span data-ttu-id="757aa-121">Pokud soubor jar hello v úložišti clusteru hello (WASB)</span><span class="sxs-lookup"><span data-stu-id="757aa-121">If hello jar file is on hello cluster storage (WASB)</span></span>
  
        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"
* <span data-ttu-id="757aa-122">Pokud chcete toopass hello jar filename a hello classname jako součást vstupní soubor (v tomto příkladu vstup.txt)</span><span class="sxs-lookup"><span data-stu-id="757aa-122">If you want toopass hello jar filename and hello classname as part of an input file (in this example, input.txt)</span></span>
  
        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-livy-spark-batches-running-on-hello-cluster"></a><span data-ttu-id="757aa-123">Informace o spuštěných v clusteru hello dávek Livy Spark</span><span class="sxs-lookup"><span data-stu-id="757aa-123">Get information on Livy Spark batches running on hello cluster</span></span>
    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

<span data-ttu-id="757aa-124">**Příklady**:</span><span class="sxs-lookup"><span data-stu-id="757aa-124">**Examples**:</span></span>

* <span data-ttu-id="757aa-125">Pokud chcete tooretrieve všechny dávky Livy Spark hello běžící v clusteru s hello:</span><span class="sxs-lookup"><span data-stu-id="757aa-125">If you want tooretrieve all hello Livy Spark batches running on hello cluster:</span></span>
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
* <span data-ttu-id="757aa-126">Pokud chcete, aby tooretrieve konkrétní dávky s danou ID dávky</span><span class="sxs-lookup"><span data-stu-id="757aa-126">If you want tooretrieve a specific batch with a given batchId</span></span>
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="delete-a-livy-spark-batch-job"></a><span data-ttu-id="757aa-127">Odstranit Livy Spark dávkovou úlohu</span><span class="sxs-lookup"><span data-stu-id="757aa-127">Delete a Livy Spark batch job</span></span>
    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

<span data-ttu-id="757aa-128">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="757aa-128">**Example**:</span></span>

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-spark-and-high-availability"></a><span data-ttu-id="757aa-129">Livy Spark a vysoká dostupnost</span><span class="sxs-lookup"><span data-stu-id="757aa-129">Livy Spark and high-availability</span></span>
<span data-ttu-id="757aa-130">Livy poskytuje vysokou dostupnost pro Spark úloh spuštěných v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="757aa-130">Livy provides high-availability for Spark jobs running on hello cluster.</span></span> <span data-ttu-id="757aa-131">Tady je několik příkladů.</span><span class="sxs-lookup"><span data-stu-id="757aa-131">Here is a couple of examples.</span></span>

* <span data-ttu-id="757aa-132">Pokud hello Livy služby přestane fungovat po odeslání úlohy vzdáleně tooa Spark cluster, hello úloha bude pokračovat toorun hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="757aa-132">If hello Livy service goes down after you have submitted a job remotely tooa Spark cluster, hello job continues toorun in hello background.</span></span> <span data-ttu-id="757aa-133">Při zálohování Livy obnoví stav hello hello úlohy a sestavy zpátky.</span><span class="sxs-lookup"><span data-stu-id="757aa-133">When Livy is back up, it restores hello status of hello job and reports it back.</span></span>
* <span data-ttu-id="757aa-134">Poznámkové bloky Jupyter pro HDInsight jsou zapnuté pomocí Livy v back-end hello.</span><span class="sxs-lookup"><span data-stu-id="757aa-134">Jupyter notebooks for HDInsight are powered by Livy in hello backend.</span></span> <span data-ttu-id="757aa-135">Pokud poznámkový blok běží úlohy Spark a získá hello Livy službu restartovat, pokračuje hello Poznámkový blok buněk kód toorun hello.</span><span class="sxs-lookup"><span data-stu-id="757aa-135">If a notebook is running a Spark job and hello Livy service gets restarted, hello notebook continues toorun hello code cells.</span></span> 

## <a name="show-me-an-example"></a><span data-ttu-id="757aa-136">Zobrazit příklad</span><span class="sxs-lookup"><span data-stu-id="757aa-136">Show me an example</span></span>
<span data-ttu-id="757aa-137">V této části jsme podívejte se na příklady toouse Livy Spark toosubmit dávkovou úlohu, sledovat průběh hello hello úlohy a poté jej odstraňte.</span><span class="sxs-lookup"><span data-stu-id="757aa-137">In this section, we look at examples toouse Livy Spark toosubmit batch job, monitor hello progress of hello job, and then delete it.</span></span> <span data-ttu-id="757aa-138">Hello používáme v tomto příkladu je aplikace hello jeden vyvinuté v článku hello [vytvořit samostatná Scala aplikace a toorun na clusteru HDInsight Spark](hdinsight-apache-spark-create-standalone-application.md).</span><span class="sxs-lookup"><span data-stu-id="757aa-138">hello application we use in this example is hello one developed in hello article [Create a standalone Scala application and toorun on HDInsight Spark cluster](hdinsight-apache-spark-create-standalone-application.md).</span></span> <span data-ttu-id="757aa-139">Zde Hello postup předpokládá, že:</span><span class="sxs-lookup"><span data-stu-id="757aa-139">hello steps here assume that:</span></span>

* <span data-ttu-id="757aa-140">Již jste zkopírovali přes hello aplikace jar toohello účtu úložiště přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="757aa-140">You have already copied over hello application jar toohello storage account associated with hello cluster.</span></span>
* <span data-ttu-id="757aa-141">Máte CuRL nainstalovat hello počítače, které se pokoušíte tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="757aa-141">You have CuRL installed on hello computer where you are trying these steps.</span></span>

<span data-ttu-id="757aa-142">Proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="757aa-142">Perform hello following steps:</span></span>

1. <span data-ttu-id="757aa-143">Dejte nám nejdřív ověřte, zda Livy Spark je spuštěna v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="757aa-143">Let us first verify that Livy Spark is running on hello cluster.</span></span> <span data-ttu-id="757aa-144">Jsme to provést tím, že získáme seznam spuštěných dávky.</span><span class="sxs-lookup"><span data-stu-id="757aa-144">We can do so by getting a list of running batches.</span></span> <span data-ttu-id="757aa-145">Pokud používáte úlohu pomocí Livy pro hello poprvé, by měl vrátit výstup hello nula.</span><span class="sxs-lookup"><span data-stu-id="757aa-145">If you are running a job using Livy for hello first time, hello output should return zero.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    <span data-ttu-id="757aa-146">Měli byste obdržet výstup podobný toohello, v následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="757aa-146">You should get an output similar toohello following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="757aa-147">Všimněte si, jak uvádí hello poslední řádek ve výstupu hello **celkem: 0**, která navrhuje žádné spuštěné dávky.</span><span class="sxs-lookup"><span data-stu-id="757aa-147">Notice how hello last line in hello output says **total:0**, which suggests no running batches.</span></span>

2. <span data-ttu-id="757aa-148">Dejte nám teď odešlete dávkovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="757aa-148">Let us now submit a batch job.</span></span> <span data-ttu-id="757aa-149">Následující fragment kódu Hello používá vstupní soubor (vstup.txt) toopass hello jar jméno a název třídy hello jako parametry.</span><span class="sxs-lookup"><span data-stu-id="757aa-149">hello following snippet uses an input file (input.txt) toopass hello jar name and hello class name as parameters.</span></span> <span data-ttu-id="757aa-150">Pokud používáte tyto kroky ze počítači se systémem Windows, není použití souboru vstupní hello doporučenému přístupu.</span><span class="sxs-lookup"><span data-stu-id="757aa-150">If you are running these steps from a Windows computer, using an input file is hello recommended approach.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    <span data-ttu-id="757aa-151">Hello parametry v souboru hello **vstup.txt** jsou definovány takto:</span><span class="sxs-lookup"><span data-stu-id="757aa-151">hello parameters in hello file **input.txt** are defined as follows:</span></span>
   
        { "file":"wasb:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }
   
    <span data-ttu-id="757aa-152">Měli byste vidět výstup podobný toohello, v následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="757aa-152">You should see an output similar toohello  following snippet:</span></span>
   
        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="757aa-153">Všimněte si, jak uvádí hello poslední řádek výstupu hello **stavu: spouštění**.</span><span class="sxs-lookup"><span data-stu-id="757aa-153">Notice how hello last line of hello output says **state:starting**.</span></span> <span data-ttu-id="757aa-154">Také uvádí, **id: 0**.</span><span class="sxs-lookup"><span data-stu-id="757aa-154">It also says, **id:0**.</span></span> <span data-ttu-id="757aa-155">Zde **0** je ID hello dávky.</span><span class="sxs-lookup"><span data-stu-id="757aa-155">Here, **0** is hello batch ID.</span></span>

3. <span data-ttu-id="757aa-156">Nyní můžete načíst hello stav této konkrétní batch pomocí ID hello dávky.</span><span class="sxs-lookup"><span data-stu-id="757aa-156">You can now retrieve hello status of this specific batch using hello batch ID.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    <span data-ttu-id="757aa-157">Měli byste vidět výstup podobný toohello, v následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="757aa-157">You should see an output similar toohello following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="757aa-158">nyní zobrazuje výstup Hello **stavu: Úspěch**, která navrhuje této hello úlohy se úspěšně dokončila.</span><span class="sxs-lookup"><span data-stu-id="757aa-158">hello output now shows **state:success**, which suggests that hello job was successfully completed.</span></span>

4. <span data-ttu-id="757aa-159">Pokud chcete, můžete nyní odstranit hello batch.</span><span class="sxs-lookup"><span data-stu-id="757aa-159">If you want, you can now delete hello batch.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    <span data-ttu-id="757aa-160">Měli byste vidět výstup podobný toohello, v následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="757aa-160">You should see an output similar toohello following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="757aa-161">Hello poslední řádek výstupu hello zobrazuje, že hello dávky byla úspěšně odstraněna.</span><span class="sxs-lookup"><span data-stu-id="757aa-161">hello last line of hello output shows that hello batch was successfully deleted.</span></span> <span data-ttu-id="757aa-162">Odstranění úlohy, když je spuštěná, také ukončí úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="757aa-162">Deleting a job, while it is running, also kills hello job.</span></span> <span data-ttu-id="757aa-163">Pokud odstraníte úlohu, která byla dokončena úspěšně, nebo v opačném odstraní informace o úlohách hello úplně.</span><span class="sxs-lookup"><span data-stu-id="757aa-163">If you delete a job that has completed, successfully or otherwise, it deletes hello job information completely.</span></span>

## <a name="using-livy-spark-on-hdinsight-35-clusters"></a><span data-ttu-id="757aa-164">Pomocí Livy Spark v clusterech HDInsight 3.5</span><span class="sxs-lookup"><span data-stu-id="757aa-164">Using Livy Spark on HDInsight 3.5 clusters</span></span>

<span data-ttu-id="757aa-165">Cluster HDInsight 3.5, ve výchozím nastavení, zakáže použití místního souboru cesty tooaccess ukázkových datových souborů nebo souborů JAR.</span><span class="sxs-lookup"><span data-stu-id="757aa-165">HDInsight 3.5 cluster, by default, disables use of local file paths tooaccess sample data files or jars.</span></span> <span data-ttu-id="757aa-166">Doporučujeme vám toouse hello `wasb://` cesta místo JAR tooaccess nebo ukázková data souborů z clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="757aa-166">We encourage you toouse hello `wasb://` path instead tooaccess jars or sample data files from hello cluster.</span></span> <span data-ttu-id="757aa-167">Pokud chcete toouse místní cestu, je nutné aktualizovat konfiguraci Ambari hello odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="757aa-167">If you do want toouse local path, you must update hello Ambari configuration accordingly.</span></span> <span data-ttu-id="757aa-168">toodo tak:</span><span class="sxs-lookup"><span data-stu-id="757aa-168">toodo so:</span></span>

1. <span data-ttu-id="757aa-169">Přejděte toohello Ambari portál pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="757aa-169">Go toohello Ambari portal for hello cluster.</span></span> <span data-ttu-id="757aa-170">je k dispozici v clusteru HDInsight na https:// Hello webové uživatelské rozhraní Ambari**CLUSTERNAME**. azurehdidnsight.net, kde CLUSTERNAME představuje název hello daného clusteru.</span><span class="sxs-lookup"><span data-stu-id="757aa-170">hello Ambari Web UI is available on your HDInsight cluster at https://**CLUSTERNAME**.azurehdidnsight.net, where CLUSTERNAME is hello name of your cluster.</span></span>

2. <span data-ttu-id="757aa-171">Z hello levé navigační, klikněte na tlačítko **Livy**a potom klikněte na **konfigurací**.</span><span class="sxs-lookup"><span data-stu-id="757aa-171">From hello left navigation, click **Livy**, and then click **Configs**.</span></span>

3. <span data-ttu-id="757aa-172">V části **livy výchozí** přidat název vlastnosti hello `livy.file.local-dir-whitelist` a nastavení jeho hodnoty příliš**"/"** Pokud chcete systém toofile tooallow úplný přístup.</span><span class="sxs-lookup"><span data-stu-id="757aa-172">Under **livy-default** add hello property name `livy.file.local-dir-whitelist` and set it's value too**"/"** if you want tooallow full access toofile system.</span></span> <span data-ttu-id="757aa-173">Pokud chcete tooallow přístup pouze tooa konkrétního adresáře, zadejte jako hodnotu hello hello cesta toothat adresáře.</span><span class="sxs-lookup"><span data-stu-id="757aa-173">If you want tooallow access only tooa specific directory, provide hello path toothat directory as hello value.</span></span>

## <a name="submitting-livy-jobs-for-a-cluster-within-an-azure-virtual-network"></a><span data-ttu-id="757aa-174">Odesílání úloh Livy pro cluster se v rámci virtuální sítě Azure</span><span class="sxs-lookup"><span data-stu-id="757aa-174">Submitting Livy jobs for a cluster within an Azure virtual network</span></span>

<span data-ttu-id="757aa-175">Pokud připojíte tooan clusteru HDInsight Spark z v rámci virtuální síť Azure, můžete přímo připojit tooLivy v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="757aa-175">If you connect tooan HDInsight Spark cluster from within an Azure Virtual Network, you can directly connect tooLivy on hello cluster.</span></span> <span data-ttu-id="757aa-176">V takovém případě hello adresu URL pro koncový bod Livy `http://<IP address of hello headnode>:8998/batches`.</span><span class="sxs-lookup"><span data-stu-id="757aa-176">In such a case, hello URL for Livy endpoint is `http://<IP address of hello headnode>:8998/batches`.</span></span> <span data-ttu-id="757aa-177">Zde **8998** je hello port, na kterém běží Livy na headnode hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="757aa-177">Here, **8998** is hello port on which Livy runs on hello cluster headnode.</span></span> <span data-ttu-id="757aa-178">Další informace o přístup ke službám na neveřejný portech najdete v tématu [porty používané služby Hadoop v HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="757aa-178">For more information on accessing services on non-public ports, see [Ports used by Hadoop services on HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="757aa-179">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="757aa-179">Troubleshooting</span></span>

<span data-ttu-id="757aa-180">Tady jsou některé problémy, které se mohou vyskytnout při používání Livy pro vzdálené úlohy odeslání tooSpark clustery.</span><span class="sxs-lookup"><span data-stu-id="757aa-180">Here are some issues that you might run into while using Livy for remote job submission tooSpark clusters.</span></span>

### <a name="using-an-external-jar-from-hello-additional-storage-is-not-supported"></a><span data-ttu-id="757aa-181">Použití externí jar z hello další úložiště není podporováno</span><span class="sxs-lookup"><span data-stu-id="757aa-181">Using an external jar from hello additional storage is not supported</span></span>

<span data-ttu-id="757aa-182">**Problém:** Pokud vaše úlohy Livy Spark odkazuje externí jar z účtu hello další úložiště přidruženého k hello clusteru, úloha hello selže.</span><span class="sxs-lookup"><span data-stu-id="757aa-182">**Problem:** If your Livy Spark job references an external jar from hello additional storage account associated with hello cluster, hello job fails.</span></span>

<span data-ttu-id="757aa-183">**Řešení:** Ujistěte se, že hello jar chcete toouse je k dispozici v hello výchozí úložiště přidruženého k hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="757aa-183">**Resolution:** Make sure that hello jar you want toouse is available in hello default storage associated with hello HDInsight cluster.</span></span>





## <a name="next-step"></a><span data-ttu-id="757aa-184">Další krok</span><span class="sxs-lookup"><span data-stu-id="757aa-184">Next step</span></span>

* [<span data-ttu-id="757aa-185">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="757aa-185">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="757aa-186">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="757aa-186">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

