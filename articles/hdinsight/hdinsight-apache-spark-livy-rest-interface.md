---
title: "Používat k odesílání, úlohy Spark clusteru v Azure HDInsight Livy Spark | Microsoft Docs"
description: "Naučte se používat k odesílání úloh Spark vzdáleně do clusteru Azure HDInsight Apache Spark REST API."
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
ms.openlocfilehash: e1a28d69bbf40ea3134a7899a0d2fe70e5fc9e71
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-apache-spark-rest-api-to-submit-remote-jobs-to-an-hdinsight-spark-cluster"></a><span data-ttu-id="f69d4-104">Použití Apache Spark REST API vzdálené úlohy do clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="f69d4-104">Use Apache Spark REST API to submit remote jobs to an HDInsight Spark cluster</span></span>

<span data-ttu-id="f69d4-105">Další informace o použití Livy, Apache Spark REST API, které se používá k odesílání vzdálené úloh do clusteru Azure HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="f69d4-105">Learn how to use Livy, the Apache Spark REST API, which is used to submit remote jobs to an Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="f69d4-106">Podrobnou dokumentaci najdete v tématu [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).</span><span class="sxs-lookup"><span data-stu-id="f69d4-106">For detailed documentation, see [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).</span></span>

<span data-ttu-id="f69d4-107">Livy můžete použít ke spuštění interaktivních Spark nutný nebo odesílat dávkové úlohy můžou běžet na Spark.</span><span class="sxs-lookup"><span data-stu-id="f69d4-107">You can use Livy to run interactive Spark shells or submit batch jobs to be run on Spark.</span></span> <span data-ttu-id="f69d4-108">V tomto článku bude zmíněn pomocí Livy odesílat dávkové úlohy.</span><span class="sxs-lookup"><span data-stu-id="f69d4-108">This article talks about using Livy to submit batch jobs.</span></span> <span data-ttu-id="f69d4-109">Fragmenty kódu v tomto článku pomocí cURL provádět volání rozhraní REST API ke koncovému bodu Livy Spark.</span><span class="sxs-lookup"><span data-stu-id="f69d4-109">The snippets in this article use cURL to make REST API calls to the Livy Spark endpoint.</span></span>

<span data-ttu-id="f69d4-110">**Požadavky:**</span><span class="sxs-lookup"><span data-stu-id="f69d4-110">**Prerequisites:**</span></span>

* <span data-ttu-id="f69d4-111">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f69d4-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="f69d4-112">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="f69d4-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

* <span data-ttu-id="f69d4-113">[cURL](http://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="f69d4-113">[cURL](http://curl.haxx.se/).</span></span> <span data-ttu-id="f69d4-114">Tento článek používá cURL k ukazují, jak provádět volání rozhraní REST API proti clusteru služby HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="f69d4-114">This article uses cURL to demonstrate how to make REST API calls against an HDInsight Spark cluster.</span></span>

## <a name="submit-a-livy-spark-batch-job"></a><span data-ttu-id="f69d4-115">Odeslání Livy Spark dávkovou úlohu</span><span class="sxs-lookup"><span data-stu-id="f69d4-115">Submit a Livy Spark batch job</span></span>
<span data-ttu-id="f69d4-116">Před odesláním dávkovou úlohu, musíte nahrát jar aplikace v úložišti clusteru, který je přidružen ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="f69d4-116">Before you submit a batch job, you must upload the application jar on the cluster storage associated with the cluster.</span></span> <span data-ttu-id="f69d4-117">Můžete použít [ **AzCopy**](../storage/common/storage-use-azcopy.md), nástroj příkazového řádku, tak.</span><span class="sxs-lookup"><span data-stu-id="f69d4-117">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command-line utility, to do so.</span></span> <span data-ttu-id="f69d4-118">Existují různé klienty, které můžete použít k nahrání data.</span><span class="sxs-lookup"><span data-stu-id="f69d4-118">There are various other clients you can use to upload data.</span></span> <span data-ttu-id="f69d4-119">Můžete najít další informace o nich v [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="f69d4-119">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

<span data-ttu-id="f69d4-120">**Příklady**:</span><span class="sxs-lookup"><span data-stu-id="f69d4-120">**Examples**:</span></span>

* <span data-ttu-id="f69d4-121">Pokud je soubor jar v úložišti clusteru (WASB)</span><span class="sxs-lookup"><span data-stu-id="f69d4-121">If the jar file is on the cluster storage (WASB)</span></span>
  
        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"
* <span data-ttu-id="f69d4-122">Pokud chcete předat jako součást vstupní soubor jar název souboru a název třídy (v tomto příkladu vstup.txt)</span><span class="sxs-lookup"><span data-stu-id="f69d4-122">If you want to pass the jar filename and the classname as part of an input file (in this example, input.txt)</span></span>
  
        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-livy-spark-batches-running-on-the-cluster"></a><span data-ttu-id="f69d4-123">Informace o Livy Spark dávek běžící v clusteru</span><span class="sxs-lookup"><span data-stu-id="f69d4-123">Get information on Livy Spark batches running on the cluster</span></span>
    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

<span data-ttu-id="f69d4-124">**Příklady**:</span><span class="sxs-lookup"><span data-stu-id="f69d4-124">**Examples**:</span></span>

* <span data-ttu-id="f69d4-125">Pokud chcete načíst všechny dávky Livy Spark běžící v clusteru:</span><span class="sxs-lookup"><span data-stu-id="f69d4-125">If you want to retrieve all the Livy Spark batches running on the cluster:</span></span>
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
* <span data-ttu-id="f69d4-126">Pokud chcete načíst konkrétní dávky s danou ID dávky</span><span class="sxs-lookup"><span data-stu-id="f69d4-126">If you want to retrieve a specific batch with a given batchId</span></span>
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="delete-a-livy-spark-batch-job"></a><span data-ttu-id="f69d4-127">Odstranit Livy Spark dávkovou úlohu</span><span class="sxs-lookup"><span data-stu-id="f69d4-127">Delete a Livy Spark batch job</span></span>
    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

<span data-ttu-id="f69d4-128">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="f69d4-128">**Example**:</span></span>

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-spark-and-high-availability"></a><span data-ttu-id="f69d4-129">Livy Spark a vysoká dostupnost</span><span class="sxs-lookup"><span data-stu-id="f69d4-129">Livy Spark and high-availability</span></span>
<span data-ttu-id="f69d4-130">Livy poskytuje vysokou dostupnost pro Spark úloh spuštěných v clusteru.</span><span class="sxs-lookup"><span data-stu-id="f69d4-130">Livy provides high-availability for Spark jobs running on the cluster.</span></span> <span data-ttu-id="f69d4-131">Tady je několik příkladů.</span><span class="sxs-lookup"><span data-stu-id="f69d4-131">Here is a couple of examples.</span></span>

* <span data-ttu-id="f69d4-132">Pokud službu Livy přestane fungovat po odeslání úlohy vzdáleně na clusteru Spark, úloha dál běžet na pozadí.</span><span class="sxs-lookup"><span data-stu-id="f69d4-132">If the Livy service goes down after you have submitted a job remotely to a Spark cluster, the job continues to run in the background.</span></span> <span data-ttu-id="f69d4-133">Při zálohování Livy obnoví stav úlohy a sestavy zpátky.</span><span class="sxs-lookup"><span data-stu-id="f69d4-133">When Livy is back up, it restores the status of the job and reports it back.</span></span>
* <span data-ttu-id="f69d4-134">Poznámkové bloky Jupyter pro HDInsight jsou zapnuté pomocí Livy v back-end.</span><span class="sxs-lookup"><span data-stu-id="f69d4-134">Jupyter notebooks for HDInsight are powered by Livy in the backend.</span></span> <span data-ttu-id="f69d4-135">Pokud do poznámkového bloku běží úlohy Spark a získá restartována Livy, poznámkového bloku dál běžet buňky kódu.</span><span class="sxs-lookup"><span data-stu-id="f69d4-135">If a notebook is running a Spark job and the Livy service gets restarted, the notebook continues to run the code cells.</span></span> 

## <a name="show-me-an-example"></a><span data-ttu-id="f69d4-136">Zobrazit příklad</span><span class="sxs-lookup"><span data-stu-id="f69d4-136">Show me an example</span></span>
<span data-ttu-id="f69d4-137">V této části se podíváme na příklady použití Livy Spark k odeslání úlohy batch, sledovat průběh úlohy a poté jej odstraňte.</span><span class="sxs-lookup"><span data-stu-id="f69d4-137">In this section, we look at examples to use Livy Spark to submit batch job, monitor the progress of the job, and then delete it.</span></span> <span data-ttu-id="f69d4-138">Aplikace v tomto příkladu používáme je vyvinuté v článku [vytvořit samostatnou Scala aplikací a ke spuštění v clusteru HDInsight Spark](hdinsight-apache-spark-create-standalone-application.md).</span><span class="sxs-lookup"><span data-stu-id="f69d4-138">The application we use in this example is the one developed in the article [Create a standalone Scala application and to run on HDInsight Spark cluster](hdinsight-apache-spark-create-standalone-application.md).</span></span> <span data-ttu-id="f69d4-139">Tady postup předpokládá, že:</span><span class="sxs-lookup"><span data-stu-id="f69d4-139">The steps here assume that:</span></span>

* <span data-ttu-id="f69d4-140">Zkopíruje přes jar aplikací mít již k účtu úložiště, který je přidružen ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="f69d4-140">You have already copied over the application jar to the storage account associated with the cluster.</span></span>
* <span data-ttu-id="f69d4-141">Máte CuRL nainstalovaná na počítači, kde se pokoušíte tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="f69d4-141">You have CuRL installed on the computer where you are trying these steps.</span></span>

<span data-ttu-id="f69d4-142">Proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f69d4-142">Perform the following steps:</span></span>

1. <span data-ttu-id="f69d4-143">Dejte nám nejdřív ověřte, zda Livy Spark je spuštěna v clusteru.</span><span class="sxs-lookup"><span data-stu-id="f69d4-143">Let us first verify that Livy Spark is running on the cluster.</span></span> <span data-ttu-id="f69d4-144">Jsme to provést tím, že získáme seznam spuštěných dávky.</span><span class="sxs-lookup"><span data-stu-id="f69d4-144">We can do so by getting a list of running batches.</span></span> <span data-ttu-id="f69d4-145">Pokud používáte úlohu pomocí Livy poprvé, výstup by měl vrátit nula.</span><span class="sxs-lookup"><span data-stu-id="f69d4-145">If you are running a job using Livy for the first time, the output should return zero.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    <span data-ttu-id="f69d4-146">Měli byste obdržet výstup podobná následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="f69d4-146">You should get an output similar to the following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="f69d4-147">Všimněte si, jak uvádí poslední řádek ve výstupu **celkem: 0**, která navrhuje žádné spuštěné dávky.</span><span class="sxs-lookup"><span data-stu-id="f69d4-147">Notice how the last line in the output says **total:0**, which suggests no running batches.</span></span>

2. <span data-ttu-id="f69d4-148">Dejte nám teď odešlete dávkovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="f69d4-148">Let us now submit a batch job.</span></span> <span data-ttu-id="f69d4-149">Následující fragment kódu používá vstupní soubor (vstup.txt) jako parametry předat název jar a název třídy.</span><span class="sxs-lookup"><span data-stu-id="f69d4-149">The following snippet uses an input file (input.txt) to pass the jar name and the class name as parameters.</span></span> <span data-ttu-id="f69d4-150">Pokud používáte tyto kroky ze počítači se systémem Windows, pomocí vstupní soubor se o doporučený postup.</span><span class="sxs-lookup"><span data-stu-id="f69d4-150">If you are running these steps from a Windows computer, using an input file is the recommended approach.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    <span data-ttu-id="f69d4-151">Parametry v souboru **vstup.txt** jsou definovány takto:</span><span class="sxs-lookup"><span data-stu-id="f69d4-151">The parameters in the file **input.txt** are defined as follows:</span></span>
   
        { "file":"wasb:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }
   
    <span data-ttu-id="f69d4-152">Měli byste vidět výstup podobný následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="f69d4-152">You should see an output similar to the  following snippet:</span></span>
   
        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="f69d4-153">Všimněte si, jak uvádí poslední řádek výstupu **stavu: spouštění**.</span><span class="sxs-lookup"><span data-stu-id="f69d4-153">Notice how the last line of the output says **state:starting**.</span></span> <span data-ttu-id="f69d4-154">Také uvádí, **id: 0**.</span><span class="sxs-lookup"><span data-stu-id="f69d4-154">It also says, **id:0**.</span></span> <span data-ttu-id="f69d4-155">Zde **0** je ID dávky.</span><span class="sxs-lookup"><span data-stu-id="f69d4-155">Here, **0** is the batch ID.</span></span>

3. <span data-ttu-id="f69d4-156">Nyní můžete načíst stav této konkrétní batch pomocí ID dávky.</span><span class="sxs-lookup"><span data-stu-id="f69d4-156">You can now retrieve the status of this specific batch using the batch ID.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    <span data-ttu-id="f69d4-157">Měli byste vidět výstup podobný následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="f69d4-157">You should see an output similar to the following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="f69d4-158">Nyní ukazuje výstup **stavu: Úspěch**, který naznačuje, že úloha byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="f69d4-158">The output now shows **state:success**, which suggests that the job was successfully completed.</span></span>

4. <span data-ttu-id="f69d4-159">Pokud chcete, můžete nyní odstranit dávky.</span><span class="sxs-lookup"><span data-stu-id="f69d4-159">If you want, you can now delete the batch.</span></span>
   
        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    <span data-ttu-id="f69d4-160">Měli byste vidět výstup podobný následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="f69d4-160">You should see an output similar to the following snippet:</span></span>
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
   
    <span data-ttu-id="f69d4-161">Poslední řádek výstupu ukazuje, že dávka byla úspěšně odstraněna.</span><span class="sxs-lookup"><span data-stu-id="f69d4-161">The last line of the output shows that the batch was successfully deleted.</span></span> <span data-ttu-id="f69d4-162">Odstranění úlohy, když je spuštěná, také ukončí úlohu.</span><span class="sxs-lookup"><span data-stu-id="f69d4-162">Deleting a job, while it is running, also kills the job.</span></span> <span data-ttu-id="f69d4-163">Pokud odstraníte úlohu, která byla dokončena úspěšně, nebo jinak, odstraní informace o úlohách úplně.</span><span class="sxs-lookup"><span data-stu-id="f69d4-163">If you delete a job that has completed, successfully or otherwise, it deletes the job information completely.</span></span>

## <a name="using-livy-spark-on-hdinsight-35-clusters"></a><span data-ttu-id="f69d4-164">Pomocí Livy Spark v clusterech HDInsight 3.5</span><span class="sxs-lookup"><span data-stu-id="f69d4-164">Using Livy Spark on HDInsight 3.5 clusters</span></span>

<span data-ttu-id="f69d4-165">Cluster HDInsight 3.5, ve výchozím nastavení, zakáže použití místní cesty souborů přístup ukázkových datových souborů nebo souborů JAR.</span><span class="sxs-lookup"><span data-stu-id="f69d4-165">HDInsight 3.5 cluster, by default, disables use of local file paths to access sample data files or jars.</span></span> <span data-ttu-id="f69d4-166">Doporučujeme, abyste použili `wasb://` cesta místo toho k přístupu k JAR nebo vzorová data souborů z clusteru.</span><span class="sxs-lookup"><span data-stu-id="f69d4-166">We encourage you to use the `wasb://` path instead to access jars or sample data files from the cluster.</span></span> <span data-ttu-id="f69d4-167">Pokud chcete používat místní cestu, je nutné aktualizovat konfiguraci Ambari odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="f69d4-167">If you do want to use local path, you must update the Ambari configuration accordingly.</span></span> <span data-ttu-id="f69d4-168">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="f69d4-168">To do so:</span></span>

1. <span data-ttu-id="f69d4-169">Přejděte na portál Ambari pro cluster.</span><span class="sxs-lookup"><span data-stu-id="f69d4-169">Go to the Ambari portal for the cluster.</span></span> <span data-ttu-id="f69d4-170">Webové uživatelské rozhraní Ambari je k dispozici v clusteru HDInsight na https://**CLUSTERNAME**. azurehdidnsight.net, kde CLUSTERNAME představuje název clusteru.</span><span class="sxs-lookup"><span data-stu-id="f69d4-170">The Ambari Web UI is available on your HDInsight cluster at https://**CLUSTERNAME**.azurehdidnsight.net, where CLUSTERNAME is the name of your cluster.</span></span>

2. <span data-ttu-id="f69d4-171">V levém navigačním, klikněte na **Livy**a potom klikněte na **konfigurací**.</span><span class="sxs-lookup"><span data-stu-id="f69d4-171">From the left navigation, click **Livy**, and then click **Configs**.</span></span>

3. <span data-ttu-id="f69d4-172">V části **livy výchozí** přidejte název vlastnosti `livy.file.local-dir-whitelist` a jeho hodnotu nastavte **"/"** Pokud chcete povolit úplný přístup k systému souborů.</span><span class="sxs-lookup"><span data-stu-id="f69d4-172">Under **livy-default** add the property name `livy.file.local-dir-whitelist` and set it's value to **"/"** if you want to allow full access to file system.</span></span> <span data-ttu-id="f69d4-173">Pokud chcete povolit přístup pouze pro konkrétního adresáře, zadejte cestu k tomuto adresáři jako hodnota.</span><span class="sxs-lookup"><span data-stu-id="f69d4-173">If you want to allow access only to a specific directory, provide the path to that directory as the value.</span></span>

## <a name="submitting-livy-jobs-for-a-cluster-within-an-azure-virtual-network"></a><span data-ttu-id="f69d4-174">Odesílání úloh Livy pro cluster se v rámci virtuální sítě Azure</span><span class="sxs-lookup"><span data-stu-id="f69d4-174">Submitting Livy jobs for a cluster within an Azure virtual network</span></span>

<span data-ttu-id="f69d4-175">Pokud se připojit ke clusteru HDInsight Spark z v rámci virtuální síť Azure, které mohou připojovat přímo k Livy v clusteru.</span><span class="sxs-lookup"><span data-stu-id="f69d4-175">If you connect to an HDInsight Spark cluster from within an Azure Virtual Network, you can directly connect to Livy on the cluster.</span></span> <span data-ttu-id="f69d4-176">V takovém případě je adresa URL pro koncový bod Livy `http://<IP address of the headnode>:8998/batches`.</span><span class="sxs-lookup"><span data-stu-id="f69d4-176">In such a case, the URL for Livy endpoint is `http://<IP address of the headnode>:8998/batches`.</span></span> <span data-ttu-id="f69d4-177">Zde **8998** je port, na kterém běží Livy na headnode clusteru.</span><span class="sxs-lookup"><span data-stu-id="f69d4-177">Here, **8998** is the port on which Livy runs on the cluster headnode.</span></span> <span data-ttu-id="f69d4-178">Další informace o přístup ke službám na neveřejný portech najdete v tématu [porty používané služby Hadoop v HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="f69d4-178">For more information on accessing services on non-public ports, see [Ports used by Hadoop services on HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f69d4-179">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="f69d4-179">Troubleshooting</span></span>

<span data-ttu-id="f69d4-180">Tady jsou některé problémy, které se mohou vyskytnout při pro vzdálené úlohy odeslání clustery Spark pomocí Livy.</span><span class="sxs-lookup"><span data-stu-id="f69d4-180">Here are some issues that you might run into while using Livy for remote job submission to Spark clusters.</span></span>

### <a name="using-an-external-jar-from-the-additional-storage-is-not-supported"></a><span data-ttu-id="f69d4-181">Použití externí jar z další úložiště není podporováno</span><span class="sxs-lookup"><span data-stu-id="f69d4-181">Using an external jar from the additional storage is not supported</span></span>

<span data-ttu-id="f69d4-182">**Problém:** Pokud vaše úlohy Livy Spark odkazuje na externí jar z další úložiště účet přidružený ke clusteru, se nezdaří úlohy.</span><span class="sxs-lookup"><span data-stu-id="f69d4-182">**Problem:** If your Livy Spark job references an external jar from the additional storage account associated with the cluster, the job fails.</span></span>

<span data-ttu-id="f69d4-183">**Řešení:** Ujistěte se, že je k dispozici v výchozí úložiště přidružený k clusteru HDInsight jar, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="f69d4-183">**Resolution:** Make sure that the jar you want to use is available in the default storage associated with the HDInsight cluster.</span></span>





## <a name="next-step"></a><span data-ttu-id="f69d4-184">Další krok</span><span class="sxs-lookup"><span data-stu-id="f69d4-184">Next step</span></span>

* [<span data-ttu-id="f69d4-185">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="f69d4-185">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="f69d4-186">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f69d4-186">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

