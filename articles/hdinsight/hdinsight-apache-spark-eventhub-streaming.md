---
title: "Použití Apache Spark streamování pomocí Event Hubs v Azure HDInsight | Microsoft Docs"
description: "Sestavení Apache Spark streamování ukázku odesílat datový proud do centra událostí Azure a pak přijímat události v clusteru HDInsight Spark pomocí scala aplikace."
keywords: "Apache spark streamování, vysílání datového proudu spark, ukázka spark, apache spark streamování například ukázka azure centra událostí, spark ukázka"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 68894e75-3ffa-47bd-8982-96cdad38b7d0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.openlocfilehash: 175a2ad70b1f554d05846eb62fb685d4f259af7e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="apache-spark-streaming-process-data-from-azure-event-hubs-with-spark-cluster-on-hdinsight"></a><span data-ttu-id="0127f-104">Apache Spark streamování: clusteru zpracování dat z Azure Event Hubs s Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="0127f-104">Apache Spark streaming: Process data from Azure Event Hubs with Spark cluster on HDInsight</span></span>

<span data-ttu-id="0127f-105">V tomto článku vytvořit Apache Spark streamování vzorku, který zahrnuje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0127f-105">In this article, you create an Apache Spark streaming sample that involves the following steps:</span></span>

1. <span data-ttu-id="0127f-106">Použijete samostatné aplikace k ingestování zpráv do centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="0127f-106">You use a standalone application to ingest messages into an Azure Event Hub.</span></span>

2. <span data-ttu-id="0127f-107">S dva různé přístupy můžete načítat zprávy z centra událostí v reálném čase pomocí aplikace běžící v clusteru Spark v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0127f-107">With two different approaches, you retrieve the messages from Event Hub in real-time using an application running in Spark cluster on Azure HDInsight.</span></span>

3. <span data-ttu-id="0127f-108">Vytvoření datových proudů analytické kanály pro uložení dat do jiného úložiště systémů nebo získáte přehledy z dat za chodu.</span><span class="sxs-lookup"><span data-stu-id="0127f-108">You build streaming analytic pipelines to persist data to different storage systems, or get insights from data on the fly.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0127f-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0127f-109">Prerequisites</span></span>

* <span data-ttu-id="0127f-110">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="0127f-110">An Azure subscription.</span></span> <span data-ttu-id="0127f-111">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="0127f-111">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="0127f-112">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0127f-112">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="0127f-113">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="0127f-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="spark-streaming-concepts"></a><span data-ttu-id="0127f-114">Vysílání datového proudu Spark koncepty</span><span class="sxs-lookup"><span data-stu-id="0127f-114">Spark Streaming concepts</span></span>

<span data-ttu-id="0127f-115">Podrobné vysvětlení vysílání datového proudu Spark, najdete v článku [vysílání datového proudu přehled Apache Spark](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview).</span><span class="sxs-lookup"><span data-stu-id="0127f-115">For a detailed explanation of Spark streaming, see [Apache Spark streaming overview](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview).</span></span> <span data-ttu-id="0127f-116">HDInsight přináší stejné součásti pro datové proudy pro cluster Spark v Azure.</span><span class="sxs-lookup"><span data-stu-id="0127f-116">HDInsight brings the same streaming features to a Spark cluster on Azure.</span></span>  

## <a name="what-does-this-solution-do"></a><span data-ttu-id="0127f-117">Jakým způsobem tohoto řešení?</span><span class="sxs-lookup"><span data-stu-id="0127f-117">What does this solution do?</span></span>

<span data-ttu-id="0127f-118">V tomto článku Pokud chcete vytvořit vysílání datového proudu příklad Spark, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0127f-118">In this article, to create a Spark streaming example, perform the following steps:</span></span>

1. <span data-ttu-id="0127f-119">Vytvoření centra událostí Azure, která bude přijímat události datového proudu.</span><span class="sxs-lookup"><span data-stu-id="0127f-119">Create an Azure Event Hub that will receive a stream of events.</span></span>

2. <span data-ttu-id="0127f-120">Spusťte místní samostatná aplikace, která generuje události a doručí do centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="0127f-120">Run a local standalone application that generates events and pushes it to the Azure Event Hub.</span></span> <span data-ttu-id="0127f-121">Ukázkové aplikace, k tomu je publikována v [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span><span class="sxs-lookup"><span data-stu-id="0127f-121">The sample application that does this is published at [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span></span>

3. <span data-ttu-id="0127f-122">Vzdálené spuštění aplikace na streamování cluster Spark, který čte streamování událostí z centra událostí Azure a provádět různé zpracování dat nebo analýzu.</span><span class="sxs-lookup"><span data-stu-id="0127f-122">Run a streaming application remotely on a Spark cluster that reads streaming events from Azure Event Hub and perform various data processing/analysis.</span></span>

## <a name="create-an-azure-event-hub"></a><span data-ttu-id="0127f-123">Vytvoření centra událostí Azure</span><span class="sxs-lookup"><span data-stu-id="0127f-123">Create an Azure Event Hub</span></span>

1. <span data-ttu-id="0127f-124">Přihlaste se k [portálu Azure](https://ms.portal.azure.com)a klikněte na tlačítko **nový** v levém horním rohu obrazovky.</span><span class="sxs-lookup"><span data-stu-id="0127f-124">Log on to the [Azure Portal](https://ms.portal.azure.com), and click **New** at the top left of the screen.</span></span>

2. <span data-ttu-id="0127f-125">Klikněte na **Internet věcí** a pak na **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="0127f-125">Click **Internet of Things**, then click **Event Hubs**.</span></span>

    <span data-ttu-id="0127f-126">![Centra událostí vytvořit pro příklad vysílání datového proudu Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "centra událostí vytvořit pro příklad vysílání datového proudu Spark")</span><span class="sxs-lookup"><span data-stu-id="0127f-126">![Create event hub for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "Create event hub for Spark streaming example")</span></span>

3. <span data-ttu-id="0127f-127">V okně **Vytvořit obor názvů** zadejte název oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="0127f-127">In the **Create namespace** blade, enter a namespace name.</span></span> <span data-ttu-id="0127f-128">Vyberte cenovou úroveň (Basic nebo Standard).</span><span class="sxs-lookup"><span data-stu-id="0127f-128">choose the pricing tier (Basic or Standard).</span></span> <span data-ttu-id="0127f-129">Zvolte také předplatné Azure, skupinu prostředků a umístění, ve kterém se má prostředek vytvořit.</span><span class="sxs-lookup"><span data-stu-id="0127f-129">Also, choose an Azure subscription, resource group, and location in which to create the resource.</span></span> <span data-ttu-id="0127f-130">Kliknutím na **Vytvořit** vytvoříte obor názvů.</span><span class="sxs-lookup"><span data-stu-id="0127f-130">Click **Create** to create the namespace.</span></span>

      <span data-ttu-id="0127f-131">![Zadejte název centra událostí příklad vysílání datového proudu Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "zadejte název centra událostí příklad vysílání datového proudu Spark")</span><span class="sxs-lookup"><span data-stu-id="0127f-131">![Provide an event hub name for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "Provide an event hub name for Spark streaming example")</span></span>

    > [!NOTE]
    > <span data-ttu-id="0127f-132">Měli byste vybrat stejné **umístění** jako cluster Apache Spark v HDInsight a snižuje tak latenci a náklady.</span><span class="sxs-lookup"><span data-stu-id="0127f-132">You should select the same **Location** as your Apache Spark cluster in HDInsight to reduce latency and costs.</span></span>
    >
    >

4. <span data-ttu-id="0127f-133">V seznamu oborů názvů Event Hubs klikněte na nově vytvořený obor názvů.</span><span class="sxs-lookup"><span data-stu-id="0127f-133">In the Event Hubs namespace list, click the newly-created namespace.</span></span>      


5. <span data-ttu-id="0127f-134">V okně obor názvů, klikněte na tlačítko **Event Hubs**a potom klikněte na **+ centra událostí** k vytvoření nového centra událostí.</span><span class="sxs-lookup"><span data-stu-id="0127f-134">In the namespace blade, click **Event Hubs**, and then click **+ Event Hub** to create a new Event Hub.</span></span>
   
    <span data-ttu-id="0127f-135">![Centra událostí vytvořit pro příklad vysílání datového proudu Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "centra událostí vytvořit pro příklad vysílání datového proudu Spark")</span><span class="sxs-lookup"><span data-stu-id="0127f-135">![Create event hub for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "Create event hub for Spark streaming example")</span></span>

6. <span data-ttu-id="0127f-136">Zadejte název pro vaše Centrum událostí, nastavte počet oddílů na 10 a uchování zpráv na 1.</span><span class="sxs-lookup"><span data-stu-id="0127f-136">Type a name for your Event Hub, set the partition count to 10, and message retention to 1.</span></span> <span data-ttu-id="0127f-137">Abyste mohli Ponechejte zbývající jako výchozí a pak klikněte na tlačítko jsme nejsou archivace zpráv v tomto řešení **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0127f-137">We are not archiving the messages in this solution so you can leave the rest as default, and then click **Create**.</span></span>
   
    <span data-ttu-id="0127f-138">![Zadejte podrobnosti o události rozbočovače pro příklad vysílání datového proudu Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "zadejte podrobnosti o události rozbočovače pro příklad vysílání datového proudu Spark")</span><span class="sxs-lookup"><span data-stu-id="0127f-138">![Provide event hub details for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "Provide event hub details for Spark streaming example")</span></span>

7. <span data-ttu-id="0127f-139">Nově vytvořený Centrum událostí je uvedena v okně centra událostí.</span><span class="sxs-lookup"><span data-stu-id="0127f-139">The newly created Event Hub is listed in the Event Hub blade.</span></span>
    
     <span data-ttu-id="0127f-140">![Zobrazit centra událostí pro vysílání datového proudu příklad Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "Centrum událostí zobrazení vysílání datového proudu příklad Spark")</span><span class="sxs-lookup"><span data-stu-id="0127f-140">![View Event Hub for the Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "View Event Hub for the Spark streaming example")</span></span>

8. <span data-ttu-id="0127f-141">Po návratu do okna oboru názvů (ne v okně konkrétního centra událostí) klikněte na **Zásady sdíleného přístupu** a poté klikněte na **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="0127f-141">Back in the namespace blade (not the specific Event Hub blade), click **Shared access policies**, and then click **RootManageSharedAccessKey**.</span></span>
    
     <span data-ttu-id="0127f-142">![Nastavení centra událostí zásad pro vysílání datového proudu příklad Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "zásady nastavení centra událostí pro vysílání datového proudu příklad Spark")</span><span class="sxs-lookup"><span data-stu-id="0127f-142">![Set Event Hub policies for the Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "Set Event Hub policies for the Spark streaming example")</span></span>

9. <span data-ttu-id="0127f-143">Klikněte na tlačítko Kopírovat zkopírovat **RootManageSharedAccessKey** primární klíč a připojovací řetězec do schránky.</span><span class="sxs-lookup"><span data-stu-id="0127f-143">Click the copy button to copy the **RootManageSharedAccessKey** primary key and connection string to the clipboard.</span></span> <span data-ttu-id="0127f-144">Uložte do použít později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0127f-144">Save these to use later in the tutorial.</span></span>
    
     <span data-ttu-id="0127f-145">![Zobrazit centra událostí zásad klíče pro vysílání datového proudu příklad Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "centra událostí zobrazení zásad klíče pro streamování příklad Spark")</span><span class="sxs-lookup"><span data-stu-id="0127f-145">![View Event Hub policy keys for the Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "View Event Hub policy keys for the Spark streaming example")</span></span>

## <a name="send-messages-to-azure-event-hub-using-a-sample-scala-application"></a><span data-ttu-id="0127f-146">Odeslání zprávy do centra událostí Azure pomocí Scala ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="0127f-146">Send messages to Azure Event Hub using a sample Scala application</span></span>

<span data-ttu-id="0127f-147">V této části použijte samostatná místní Scala aplikace, která generuje datového proudu událostí a odešle ji do centra událostí Azure, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="0127f-147">In this section you use a standalone local Scala application that generates a stream of events and sends it to Azure Event Hub that you created earlier.</span></span> <span data-ttu-id="0127f-148">Tato aplikace je k dispozici na webu GitHub na [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).</span><span class="sxs-lookup"><span data-stu-id="0127f-148">This application is available on GitHub at [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).</span></span> <span data-ttu-id="0127f-149">Zde postup předpokládá, že jste již forked toto úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="0127f-149">The steps here assume that you have already forked this GitHub repository.</span></span>

1. <span data-ttu-id="0127f-150">Ujistěte se, že máte k dispozici následující nainstalovaná na počítači, kde spuštění této aplikace.</span><span class="sxs-lookup"><span data-stu-id="0127f-150">Make sure you have the following installed on the computer where you run this application.</span></span>

    * <span data-ttu-id="0127f-151">Java Development kit Oracle.</span><span class="sxs-lookup"><span data-stu-id="0127f-151">Oracle Java Development kit.</span></span> <span data-ttu-id="0127f-152">Můžete nainstalovat z [zde](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="0127f-152">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
    * <span data-ttu-id="0127f-153">Apache Maven.</span><span class="sxs-lookup"><span data-stu-id="0127f-153">Apache Maven.</span></span> <span data-ttu-id="0127f-154">Si můžete stáhnout z [zde](https://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="0127f-154">You can download it from [here](https://maven.apache.org/download.cgi).</span></span> <span data-ttu-id="0127f-155">Pokyny k instalaci Maven jsou k dispozici [zde](https://maven.apache.org/install.html).</span><span class="sxs-lookup"><span data-stu-id="0127f-155">Instructions to install Maven are available [here](https://maven.apache.org/install.html).</span></span>

2. <span data-ttu-id="0127f-156">Otevřete příkazový řádek a přejděte do umístění, které jste naklonovali úložiště GitHub pro ukázkovou aplikaci Scala a spusťte následující příkaz pro sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="0127f-156">Open a command prompt and navigate to the location you cloned the GitHub repo for the sample Scala application and run the following command to build the application.</span></span>

        mvn package

3. <span data-ttu-id="0127f-157">Výstup jar pro aplikaci, **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, se vytvořil v rámci **/target** adresáře.</span><span class="sxs-lookup"><span data-stu-id="0127f-157">The output jar for the application, **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, is created under **/target** directory.</span></span> <span data-ttu-id="0127f-158">Tato JAR později v tomto článku použijete k testování úplného řešení.</span><span class="sxs-lookup"><span data-stu-id="0127f-158">You use this JAR later in this article to test the complete solution.</span></span>

## <a name="create-application-to-receive-messages-from-event-hub-into-a-spark-cluster"></a><span data-ttu-id="0127f-159">Vytvoření aplikace pro příjem zpráv z centra událostí do clusteru Spark</span><span class="sxs-lookup"><span data-stu-id="0127f-159">Create application to receive messages from Event Hub into a Spark cluster</span></span> 

<span data-ttu-id="0127f-160">Máme dva přístupy k vysílání datového proudu Spark a Azure Event Hubs, na základě příjemce připojení a připojení na základě přímo DStream připojit.</span><span class="sxs-lookup"><span data-stu-id="0127f-160">We have two approaches to connect Spark Streaming and Azure Event Hubs, Receiver-based connection and Direct-DStream-based connection.</span></span> <span data-ttu-id="0127f-161">Na základě přímo DStream byla zavedená v ledna 2017 v 2.0.3 vydání.</span><span class="sxs-lookup"><span data-stu-id="0127f-161">Direct-DStream-based is introduced on Jan of 2017, in the 2.0.3 release.</span></span> <span data-ttu-id="0127f-162">Je předpokládané nahradit původní připojení na základě příjemce, protože to je další původce a efektivní prostředků.</span><span class="sxs-lookup"><span data-stu-id="0127f-162">It is supposed to replace the original receiver-based connection as it is more performant and resource-efficient.</span></span> <span data-ttu-id="0127f-163">Další informace uvedené v [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs).</span><span class="sxs-lookup"><span data-stu-id="0127f-163">More details found in [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs).</span></span> <span data-ttu-id="0127f-164">Přímé DStream podporuje pouze Spark 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="0127f-164">Direct DStream only supports Spark 2.0+.</span></span>

### <a name="build-applications-with-the-dependency-to-spark-eventhubs-connector"></a><span data-ttu-id="0127f-165">Vytváření aplikací s závislostí do spark eventhubs konektoru</span><span class="sxs-lookup"><span data-stu-id="0127f-165">Build applications with the dependency to spark-eventhubs connector</span></span>

<span data-ttu-id="0127f-166">Pracovní verze Spark EventHubs jsme také bude publikovat na webu GitHub.</span><span class="sxs-lookup"><span data-stu-id="0127f-166">We will also publish the staging version of Spark-EventHubs in GitHub.</span></span> <span data-ttu-id="0127f-167">Pokud chcete použít pracovní verze Spark EventHubs, první krok je určit Githubu jako zdrojové úložiště přidáním následující položku na pom.xml:</span><span class="sxs-lookup"><span data-stu-id="0127f-167">To use the staging version of Spark-EventHubs, the first step is to indicate GitHub as the source repo by adding the following entry to pom.xml:</span></span>

```xml
<repository>
      <id>spark-eventhubs</id>
      <url>https://raw.github.com/hdinsight/spark-eventhubs/maven-repo/</url>
      <snapshots>
        <enabled>true</enabled>
        <updatePolicy>always</updatePolicy>
      </snapshots>
</repository>
```

<span data-ttu-id="0127f-168">Potom můžete přidat následující závislost do projektu provést předběžné verze.</span><span class="sxs-lookup"><span data-stu-id="0127f-168">You can then add the following dependency to your project to take the pre-released version.</span></span>

<span data-ttu-id="0127f-169">Závislost maven</span><span class="sxs-lookup"><span data-stu-id="0127f-169">Maven Dependency</span></span>

```xml
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11 -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-streaming-eventhubs_2.11</artifactId>
    <version>2.0.4</version>
</dependency>
```

<span data-ttu-id="0127f-170">SBT závislostí</span><span class="sxs-lookup"><span data-stu-id="0127f-170">SBT Dependency</span></span>

```
// https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11
libraryDependencies += "com.microsoft.azure" % "spark-streaming-eventhubs_2.11" % "2.0.4"
```

### <a name="direct-dstream-connection"></a><span data-ttu-id="0127f-171">Přímé připojení DStream</span><span class="sxs-lookup"><span data-stu-id="0127f-171">Direct DStream Connection</span></span>

<span data-ttu-id="0127f-172">Jar předem připraveného souboru, který obsahuje příklady použití přímé DStream lze stáhnout v [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).</span><span class="sxs-lookup"><span data-stu-id="0127f-172">A pre-built jar file containing examples using Direct DStream can be downloaded in [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).</span></span>

<span data-ttu-id="0127f-173">Na soubor jar obsahuje tři příklady, jejichž zdrojového kódu jsou k dispozici na [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).</span><span class="sxs-lookup"><span data-stu-id="0127f-173">The jar file contains three examples whose source code are available at [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).</span></span>

<span data-ttu-id="0127f-174">Pořízení [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) jako příklad:</span><span class="sxs-lookup"><span data-stu-id="0127f-174">Taking [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) as an example:</span></span>

```scala
private def createStreamingContext(
  sparkCheckpointDir: String,
  batchDuration: Int,
  namespace: String,
  eventHunName: String,
  eventhubParams: Map[String, String],
  progressDir: String) = {
val ssc = new StreamingContext(new SparkContext(), Seconds(batchDuration))
ssc.checkpoint(sparkCheckpointDir)
val inputDirectStream = EventHubsUtils.createDirectStreams(
  ssc,
  namespace,
  progressDir,
  Map(eventHunName -> eventhubParams))

inputDirectStream.map(receivedRecord => (new String(receivedRecord.getBody), 1)).
  reduceByKeyAndWindow((v1, v2) => v1 + v2, (v1, v2) => v1 - v2, Seconds(batchDuration * 3),
    Seconds(batchDuration)).print()

ssc
}

def main(args: Array[String]): Unit = {

if (args.length != 8) {
  println("Usage: program progressDir PolicyName PolicyKey EventHubNamespace EventHubName" +
    " BatchDuration(seconds) Spark_Checkpoint_Directory maxRate")
  sys.exit(1)
}

val progressDir = args(0)
val policyName = args(1)
val policykey = args(2)
val namespace = args(3)
val name = args(4)
val batchDuration = args(5).toInt
val sparkCheckpointDir = args(6)
val maxRate = args(7)

val eventhubParameters = Map[String, String] (
  "eventhubs.policyname" -> policyName,
  "eventhubs.policykey" -> policykey,
  "eventhubs.namespace" -> namespace,
  "eventhubs.name" -> name,
  "eventhubs.partition.count" -> "32",
  "eventhubs.consumergroup" -> "$Default",
  "eventhubs.maxRate" -> s"$maxRate"
)

val ssc = StreamingContext.getOrCreate(sparkCheckpointDir,
  () => createStreamingContext(sparkCheckpointDir, batchDuration, namespace, name,
    eventhubParameters, progressDir))

ssc.start()
ssc.awaitTermination()
}
```

<span data-ttu-id="0127f-175">V předchozím příkladu `eventhubParameters` jsou specifické pro jednu instanci EventHubs parametry a budete muset předejte jej `createDirectStreams` rozhraní API, která vytvoří přímé DStream objekt mapování do oboru názvů služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="0127f-175">In the above example, `eventhubParameters` are the parameters specific to a single EventHubs instance and you have to pass it to the `createDirectStreams` API which constructs a Direct DStream object mapping to a Event Hubs namespace.</span></span> <span data-ttu-id="0127f-176">Prostřednictvím přímé DStream objekt můžete volat jakéhokoli DStream rozhraní API poskytované framework API vysílání datového proudu Spark.</span><span class="sxs-lookup"><span data-stu-id="0127f-176">Over the Direct DStream object, you can call any DStream API provided by Spark Streaming API framework.</span></span> <span data-ttu-id="0127f-177">V tomto příkladu jsme vypočítat frekvenci jednotlivých slov v rámci poslední 3 malých batch intervalů.</span><span class="sxs-lookup"><span data-stu-id="0127f-177">In this example, we calculate the frequency of each word within the last 3 micro batch intervals.</span></span>

### <a name="receiver-based-connection"></a><span data-ttu-id="0127f-178">Na základě příjemce připojení</span><span class="sxs-lookup"><span data-stu-id="0127f-178">Receiver-based Connection</span></span>

<span data-ttu-id="0127f-179">Spark, streamování ukázková aplikace napsané v jazyce Scala, která přijímá události a směrovat na různé cíle, je k dispozici na [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span><span class="sxs-lookup"><span data-stu-id="0127f-179">A Spark streaming example application written in Scala, which receives events and route the to different destinations, is available at [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span></span> <span data-ttu-id="0127f-180">Použijte následující postup a aktualizovat aplikaci pro konfiguraci centra událostí vytvořit jar výstup.</span><span class="sxs-lookup"><span data-stu-id="0127f-180">Follow the steps below to update the application for your Event Hub configuration and create the output jar.</span></span>

1. <span data-ttu-id="0127f-181">Spusťte IntelliJ IDEA a úvodní obrazovka Vyberte **rezervovat z verzí** a pak klikněte na **Git**.</span><span class="sxs-lookup"><span data-stu-id="0127f-181">Launch IntelliJ IDEA and from the launch screen select **Check out from Version Control** and then click **Git**.</span></span>
   
    <span data-ttu-id="0127f-182">![Apache Spark streamování příklad - get zdroje z Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark streamování příklad - get zdroje z Git")</span><span class="sxs-lookup"><span data-stu-id="0127f-182">![Apache Spark streaming example - get sources from Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark streaming example - get sources from Git")</span></span>

2. <span data-ttu-id="0127f-183">V **klonování úložiště** dialogovém okně zadejte adresu URL do úložiště Git clone z, zadejte adresář pro klonování, a potom klikněte na **klon**.</span><span class="sxs-lookup"><span data-stu-id="0127f-183">In the **Clone Repository** dialog box, provide the URL to the Git repository to clone from, specify the directory to clone to, and then click **Clone**.</span></span>
   
    <span data-ttu-id="0127f-184">![Apache Spark streamování příklad - klonování z Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark streamování příklad - klonování z Git")</span><span class="sxs-lookup"><span data-stu-id="0127f-184">![Apache Spark streaming example - clone from Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark streaming example - clone from Git")</span></span>
3. <span data-ttu-id="0127f-185">Postupujte podle pokynů, dokud je zcela klonovat projektu.</span><span class="sxs-lookup"><span data-stu-id="0127f-185">Follow the prompts till the project is completely cloned.</span></span> <span data-ttu-id="0127f-186">Stiskněte klávesu **Alt + 1** otevřete **zobrazení projektu**.</span><span class="sxs-lookup"><span data-stu-id="0127f-186">Press **Alt + 1** to open the **Project View**.</span></span> <span data-ttu-id="0127f-187">Měl by vypadat jako následující.</span><span class="sxs-lookup"><span data-stu-id="0127f-187">It should resemble the following.</span></span>
   
    <span data-ttu-id="0127f-188">![Apache Spark streamování příklad: zobrazení projektu](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark streamování příklad: zobrazení projektu")</span><span class="sxs-lookup"><span data-stu-id="0127f-188">![Apache Spark streaming example - Project View](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark streaming example - Project View")</span></span>
4. <span data-ttu-id="0127f-189">Ujistěte se, že je s Java8 zkompilován kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="0127f-189">Make sure the application code is compiled with Java8.</span></span> <span data-ttu-id="0127f-190">Aby, klikněte na tlačítko **soubor**, klikněte na tlačítko **strukturu projektu**a na **projektu** , zkontrolujte, zda projekt jazyka level nastavená na **8 - Lambdas, typu poznámky atd**.</span><span class="sxs-lookup"><span data-stu-id="0127f-190">To ensure this, click **File**, click **Project Structure**, and on the **Project** tab, make sure Project language level is set to **8 - Lambdas, type annotations, etc.**.</span></span>
   
    <span data-ttu-id="0127f-191">![Apache Spark streamování příklad - Set kompilátoru](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark streamování příklad - Set kompilátoru")</span><span class="sxs-lookup"><span data-stu-id="0127f-191">![Apache Spark streaming example - Set compiler](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark streaming example - Set compiler")</span></span>
5. <span data-ttu-id="0127f-192">Otevřete **pom.xml** a zkontrolujte, zda je správná verze Spark.</span><span class="sxs-lookup"><span data-stu-id="0127f-192">Open the **pom.xml** and make sure the Spark version is correct.</span></span> <span data-ttu-id="0127f-193">V části `<properties>` uzlu, vyhledejte následující fragment kódu a ověřit verzi Spark.</span><span class="sxs-lookup"><span data-stu-id="0127f-193">Under `<properties>` node, look for the following snippet and verify the Spark version.</span></span>

        <scala.version>2.11.8</scala.version>
        <scala.compat.version>2.11.8</scala.compat.version>
        <scala.binary.version>2.11</scala.binary.version>
        <spark.version>2.0.0</spark.version>

6. <span data-ttu-id="0127f-194">Aplikace vyžaduje závislostí jar, nazývá **jar ovladač JDBC**.</span><span class="sxs-lookup"><span data-stu-id="0127f-194">The application requires a dependency jar called **JDBC driver jar**.</span></span> <span data-ttu-id="0127f-195">To se vyžaduje k zápisu zprávy přijaté z centra událostí do Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="0127f-195">This is required to write the messages received from Event Hub into an Azure SQL database.</span></span> <span data-ttu-id="0127f-196">Můžete si stáhnout tuto jar (v4.1 nebo novější) z [zde](https://msdn.microsoft.com/sqlserver/aa937724.aspx).</span><span class="sxs-lookup"><span data-stu-id="0127f-196">You can download this jar (v4.1 or later) from [here](https://msdn.microsoft.com/sqlserver/aa937724.aspx).</span></span> <span data-ttu-id="0127f-197">Přidáte odkaz na tento jar do projektu knihovny.</span><span class="sxs-lookup"><span data-stu-id="0127f-197">Add reference to this jar in the project library.</span></span> <span data-ttu-id="0127f-198">Proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0127f-198">Perform the following steps:</span></span>
     
     1. <span data-ttu-id="0127f-199">Z okna IntelliJ IDEA, kdy máte aplikaci otevřít, klikněte na tlačítko **soubor**, klikněte na tlačítko **strukturu projektu**a potom klikněte na **knihovny**.</span><span class="sxs-lookup"><span data-stu-id="0127f-199">From IntelliJ IDEA window where you have the application open, click **File**, click **Project Structure**, and then click **Libraries**.</span></span> 
     2. <span data-ttu-id="0127f-200">Klikněte na ikonu Přidat (![ikonu Přidat](./media/hdinsight-apache-spark-eventhub-streaming/add-icon.png)), klikněte na tlačítko **Java**a pak přejděte do umístění, kam jste stáhli jar ovladač JDBC.</span><span class="sxs-lookup"><span data-stu-id="0127f-200">Click the add icon (![add icon](./media/hdinsight-apache-spark-eventhub-streaming/add-icon.png)), click **Java**, and then navigate to the location where you downloaded the JDBC driver jar.</span></span> <span data-ttu-id="0127f-201">Postupujte podle pokynů pro přidání na soubor jar do projektu knihovny.</span><span class="sxs-lookup"><span data-stu-id="0127f-201">Follow the prompts to add the jar file to the project library.</span></span>

         <span data-ttu-id="0127f-202">![přidejte chybějící závislosti](./media/hdinsight-apache-spark-eventhub-streaming/add-missing-dependency-jars.png "přidejte chybějící závislost JAR")</span><span class="sxs-lookup"><span data-stu-id="0127f-202">![add missing dependencies](./media/hdinsight-apache-spark-eventhub-streaming/add-missing-dependency-jars.png "Add missing dependency jars")</span></span>
     3. <span data-ttu-id="0127f-203">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="0127f-203">Click **Apply**.</span></span>

7. <span data-ttu-id="0127f-204">Vytvoření výstupního souboru jar.</span><span class="sxs-lookup"><span data-stu-id="0127f-204">Create the output jar file.</span></span> <span data-ttu-id="0127f-205">Proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="0127f-205">Perform the following steps.</span></span>

   1. <span data-ttu-id="0127f-206">V **strukturu projektu** dialogové okno, klikněte na tlačítko **artefakty** a pak klikněte na plus symbol.</span><span class="sxs-lookup"><span data-stu-id="0127f-206">In the **Project Structure** dialog box, click **Artifacts** and then click the plus symbol.</span></span> <span data-ttu-id="0127f-207">V dialogovém okně automaticky otevírané okno klikněte na **JAR**a potom klikněte na **z modulů se závislostmi**.</span><span class="sxs-lookup"><span data-stu-id="0127f-207">From the pop-up dialog box, click **JAR**, and then click **From modules with dependencies**.</span></span>      
       
       <span data-ttu-id="0127f-208">![Apache Spark streamování příklad - vytvořit JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark streamování příklad - vytvořit JAR")</span><span class="sxs-lookup"><span data-stu-id="0127f-208">![Apache Spark streaming example - create JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark streaming example - create JAR")</span></span>
   2. <span data-ttu-id="0127f-209">V **vytvořit JAR z modulů** dialogové okno pole, klikněte na tlačítko se třemi tečkami (![třemi tečkami](./media/hdinsight-apache-spark-eventhub-streaming/ellipsis.png)) proti **hlavní třídy**.</span><span class="sxs-lookup"><span data-stu-id="0127f-209">In the **Create JAR from Modules** dialog box, click the ellipsis (![ellipsis](./media/hdinsight-apache-spark-eventhub-streaming/ellipsis.png)) against the **Main Class**.</span></span>
   3. <span data-ttu-id="0127f-210">V **vyberte třídu hlavní** dialogové okno pole, vyberte některou z dostupných tříd a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="0127f-210">In the **Select Main Class** dialog box, select any of the available classes and then click **OK**.</span></span>
      
       <span data-ttu-id="0127f-211">![Apache Spark streamování příklad - vyberte třídu pro jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark streamování příklad - vyberte třídu pro jar")</span><span class="sxs-lookup"><span data-stu-id="0127f-211">![Apache Spark streaming example - select class for jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark streaming example - select class for jar")</span></span>
   4. <span data-ttu-id="0127f-212">V **vytvořit JAR z modulů** dialogové okno pole, ujistěte se, že možnost **extrahovat k cíli JAR** je vybrána a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="0127f-212">In the **Create JAR from Modules** dialog box, make sure that the option to **extract to the target JAR** is selected, and then click **OK**.</span></span> <span data-ttu-id="0127f-213">Tím se vytvoří jeden JAR s všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="0127f-213">This creates a single JAR with all dependencies.</span></span>
      
       <span data-ttu-id="0127f-214">![Apache Spark streamování příklad - vytvořit jar z modulů](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark streamování příklad - vytvořit jar z modulů")</span><span class="sxs-lookup"><span data-stu-id="0127f-214">![Apache Spark streaming example - create jar from modules](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark streaming example - create jar from modules")</span></span>
   5. <span data-ttu-id="0127f-215">**Výstup rozložení** karta Vypíše seznam všech JAR, které jsou součástí na projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="0127f-215">The **Output Layout** tab lists all the jars that are included as part of the Maven project.</span></span> <span data-ttu-id="0127f-216">Můžete vybrat a odstranit ty, na kterém aplikace Scala má žádné přímé závislost.</span><span class="sxs-lookup"><span data-stu-id="0127f-216">You can select and delete the ones on which the Scala application has no direct dependency.</span></span> <span data-ttu-id="0127f-217">Pro aplikaci tady vytváříme, můžete odebrat všechny uzly s výjimkou poslední (**spark streamování data trvalost příklady zkompilovat výstup**).</span><span class="sxs-lookup"><span data-stu-id="0127f-217">For the application we are creating here, you can remove all but the last one (**spark-streaming-data-persistence-examples compile output**).</span></span> <span data-ttu-id="0127f-218">Vyberte JAR odstranit a potom klikněte na **odstranit** ikona (![odstranit ikonu](./media/hdinsight-apache-spark-eventhub-streaming/delete-icon.png)).</span><span class="sxs-lookup"><span data-stu-id="0127f-218">Select the jars to delete and then click the **Delete** icon (![delete icon](./media/hdinsight-apache-spark-eventhub-streaming/delete-icon.png)).</span></span>
      
       <span data-ttu-id="0127f-219">![Apache Spark streamování Příklad - odstranění extrahovat JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark streamování Příklad - odstranění extrahovat JAR")</span><span class="sxs-lookup"><span data-stu-id="0127f-219">![Apache Spark streaming example - delete extracted jars](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark streaming example - delete extracted jars")</span></span>
      
       <span data-ttu-id="0127f-220">Zajistěte, aby **sestavení na Ujistěte se,** je políčko zaškrtnuto, který zajistí, aby jar vytvořit pokaždé, když je vytvořené nebo aktualizované projektu.</span><span class="sxs-lookup"><span data-stu-id="0127f-220">Make sure **Build on make** box is selected, which ensures that the jar is created every time the project is built or updated.</span></span> <span data-ttu-id="0127f-221">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="0127f-221">Click **Apply**.</span></span>
   6. <span data-ttu-id="0127f-222">V **výstup rozložení** kartě přímo v dolní části **dostupné elementy** pole, máte jar SQL JDBC, který jste dříve přidali do projektu knihovny.</span><span class="sxs-lookup"><span data-stu-id="0127f-222">In the **Output Layout** tab, right at the bottom of the **Available Elements** box, you have the SQL JDBC jar that you added earlier to the project library.</span></span> <span data-ttu-id="0127f-223">Je nutné přidat do **výstup rozložení** kartě. Klikněte pravým tlačítkem na soubor jar a pak klikněte na tlačítko **extrahovat do kořenové výstup**.</span><span class="sxs-lookup"><span data-stu-id="0127f-223">You must add this to the **Output Layout** tab. Right-click the jar file, and then click **Extract Into Output Root**.</span></span>
      
       <span data-ttu-id="0127f-224">![Apache Spark streamování příklad - extract závislostí jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark streamování příklad - jar závislostí extrakce")</span><span class="sxs-lookup"><span data-stu-id="0127f-224">![Apache Spark streaming example - extract dependency jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark streaming example - extract dependency jar")</span></span>  
      
       <span data-ttu-id="0127f-225">**Výstup rozložení** karta by teď měl vypadat takto.</span><span class="sxs-lookup"><span data-stu-id="0127f-225">The **Output Layout** tab should now look like this.</span></span>
      
       <span data-ttu-id="0127f-226">![Apache Spark streamování příklad - karta finální výstup](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark streamování příklad - karta finální výstup")</span><span class="sxs-lookup"><span data-stu-id="0127f-226">![Apache Spark streaming example - final output tab](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark streaming example - final output tab")</span></span>        
      
       <span data-ttu-id="0127f-227">V **strukturu projektu** dialogové okno, klikněte na tlačítko **použít** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="0127f-227">In the **Project Structure** dialog box, click **Apply** and then click **OK**.</span></span>    
   7. <span data-ttu-id="0127f-228">V řádku nabídek klikněte na **sestavení**a potom klikněte na **zkontrolujte projektu**.</span><span class="sxs-lookup"><span data-stu-id="0127f-228">From the menu bar, click **Build**, and then click **Make Project**.</span></span> <span data-ttu-id="0127f-229">Můžete také kliknout na **sestavení artefaktů** vytvořit jar.</span><span class="sxs-lookup"><span data-stu-id="0127f-229">You can also click **Build Artifacts** to create the jar.</span></span> <span data-ttu-id="0127f-230">Výstup jar se vytvořil v rámci **\classes\artifacts**.</span><span class="sxs-lookup"><span data-stu-id="0127f-230">The output jar is created under **\classes\artifacts**.</span></span>
      
       <span data-ttu-id="0127f-231">![Apache Spark streamování příklad – výstupní JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "streamování příklad Apache Spark – výstupní JAR")</span><span class="sxs-lookup"><span data-stu-id="0127f-231">![Apache Spark streaming example - output JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "Apache Spark streaming example - output JAR")</span></span>

## <a name="run-the-application-remotely-on-a-spark-cluster-using-livy"></a><span data-ttu-id="0127f-232">Vzdálené spuštění aplikace na clusteru Spark pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="0127f-232">Run the application remotely on a Spark cluster using Livy</span></span>

<span data-ttu-id="0127f-233">V tomto článku pomocí Livy ke spuštění Apache Spark streamování aplikací vzdáleně na clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="0127f-233">In this article you use Livy to run the Apache Spark streaming application remotely on a Spark cluster.</span></span> <span data-ttu-id="0127f-234">Podrobné informace o tom, jak používat Livy s clusterem HDInsight Spark, naleznete v části [clusteru odeslání úloh vzdáleně Apache Spark v Azure HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="0127f-234">For detailed discussion on how to use Livy with HDInsight Spark cluster, see [Submit jobs remotely to an Apache Spark cluster on Azure HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span></span> <span data-ttu-id="0127f-235">Předtím, než můžete začít spouštět vysílání datového proudu aplikací Spark, existuje několik věcí, které byste měli udělat:</span><span class="sxs-lookup"><span data-stu-id="0127f-235">Before you can start running the Spark streaming application, there are a couple of things you should do:</span></span>

1. <span data-ttu-id="0127f-236">Spustit místní samostatnou aplikaci generovat události a odesílat do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="0127f-236">Start the local standalone application to generate events and sent to Event Hub.</span></span> <span data-ttu-id="0127f-237">Uděláte to tak, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0127f-237">Use the following command to do so:</span></span>

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "mysbnamespace" --eventhubs-name "myeventhub" --policy-name "mysendpolicy" --policy-key "<policy key>" --message-length 32 --thread-count 32 --message-count -1

2. <span data-ttu-id="0127f-238">Zkopírujte streamování jar (**spark streamování data trvalost examples.jar**) do úložiště objektů Azure Blob, který je přidružen ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="0127f-238">Copy the streaming jar (**spark-streaming-data-persistence-examples.jar**) to the Azure Blob storage associated with the cluster.</span></span> <span data-ttu-id="0127f-239">Díky tomu jar pro Livy dostupné.</span><span class="sxs-lookup"><span data-stu-id="0127f-239">This makes the jar accessible to Livy.</span></span> <span data-ttu-id="0127f-240">Můžete použít [ **AzCopy**](../storage/common/storage-use-azcopy.md), nástroj příkazového řádku, tak.</span><span class="sxs-lookup"><span data-stu-id="0127f-240">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command line utility, to do so.</span></span> <span data-ttu-id="0127f-241">Existuje mnoho dalších klientů, které můžete použít k nahrání data.</span><span class="sxs-lookup"><span data-stu-id="0127f-241">There are a lot of other clients you can use to upload data.</span></span> <span data-ttu-id="0127f-242">Můžete najít další informace o nich v [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="0127f-242">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>
3. <span data-ttu-id="0127f-243">Nainstalujte na počítače, kde běží tyto aplikace z CURL.</span><span class="sxs-lookup"><span data-stu-id="0127f-243">Install CURL on the computer where you are running these applications from.</span></span> <span data-ttu-id="0127f-244">Používáme CURL k vyvolání Livy koncových bodů ke spouštění úloh na dálku.</span><span class="sxs-lookup"><span data-stu-id="0127f-244">We use CURL to invoke the Livy endpoints to run the jobs remotely.</span></span>

### <a name="run-the-spark-streaming-application-to-receive-the-events-into-an-azure-storage-blob-as-text"></a><span data-ttu-id="0127f-245">Spustit Spark streamování aplikace na příjem událostí do objektu Blob úložiště Azure jako text</span><span class="sxs-lookup"><span data-stu-id="0127f-245">Run the Spark streaming application to receive the events into an Azure Storage Blob as text</span></span>

<span data-ttu-id="0127f-246">Otevřete příkazový řádek, přejděte do adresáře, kam jste nainstalovali CURL a spusťte následující příkaz (uživatelské jméno a heslo a cluster nahraďte název):</span><span class="sxs-lookup"><span data-stu-id="0127f-246">Open a command prompt, navigate to the directory where you installed CURL, and run the following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputBlob.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="0127f-247">Parametry v souboru **inputBlob.txt** jsou definovány takto:</span><span class="sxs-lookup"><span data-stu-id="0127f-247">The parameters in the file **inputBlob.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsEventCount", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="0127f-248">Dejte nám vědět, co parametry ve vstupním souboru jsou:</span><span class="sxs-lookup"><span data-stu-id="0127f-248">Let us understand what the parameters in the input file are:</span></span>

* <span data-ttu-id="0127f-249">**soubor** je cesta k souboru aplikace jar na účet úložiště Azure přidružený ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="0127f-249">**file** is the path to the application jar file on the Azure storage account associated with the cluster.</span></span>
* <span data-ttu-id="0127f-250">**Název třídy** je název třídy ve jar.</span><span class="sxs-lookup"><span data-stu-id="0127f-250">**className** is the name of the class in the jar.</span></span>
* <span data-ttu-id="0127f-251">**argumentů** je seznam argumentů, které vyžadují třídy</span><span class="sxs-lookup"><span data-stu-id="0127f-251">**args** is the list of arguments required by the class</span></span>
* <span data-ttu-id="0127f-252">**numExecutors** je počet jader používané Spark ke spuštění streamování aplikace.</span><span class="sxs-lookup"><span data-stu-id="0127f-252">**numExecutors** is the number of cores used by Spark to run the streaming application.</span></span> <span data-ttu-id="0127f-253">To by měl být vždy alespoň dvakrát počet oddílů centra událostí.</span><span class="sxs-lookup"><span data-stu-id="0127f-253">This should always be at least twice the number of Event Hub partitions.</span></span>
* <span data-ttu-id="0127f-254">**executorMemory**, **executorCores**, **driverMemory** jsou parametry sloužící k přidělování požadované prostředky streamování aplikace.</span><span class="sxs-lookup"><span data-stu-id="0127f-254">**executorMemory**, **executorCores**, **driverMemory** are parameters used to assign required resources to the streaming application.</span></span>

> [!NOTE]
> <span data-ttu-id="0127f-255">Není nutné k vytvoření výstupní složky (EventCheckpoint, EventCount/EventCount10), které se používají jako parametry.</span><span class="sxs-lookup"><span data-stu-id="0127f-255">You do not need to create the output folders (EventCheckpoint, EventCount/EventCount10) that are used as parameters.</span></span> <span data-ttu-id="0127f-256">Je vytváří streamování aplikace.</span><span class="sxs-lookup"><span data-stu-id="0127f-256">The streaming application creates them for you.</span></span>
>
>

<span data-ttu-id="0127f-257">Když spustíte příkaz, byste měli vidět výstup takto:</span><span class="sxs-lookup"><span data-stu-id="0127f-257">When you run the command, you should see an output like the following:</span></span>

    < HTTP/1.1 201 Created
    < Content-Type: application/json; charset=UTF-8
    < Location: /18
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Tue, 01 Dec 2015 05:39:10 GMT
    < Content-Length: 37
    <
    {"id":1,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

<span data-ttu-id="0127f-258">Poznamenejte si ID dávky v poslední řádek výstupu (v tomto příkladu je '1').</span><span class="sxs-lookup"><span data-stu-id="0127f-258">Make a note of the batch ID in the last line of the output (in this example it is '1').</span></span> <span data-ttu-id="0127f-259">Pokud chcete ověřit, že aplikace bude spuštěna úspěšně, můžete se podívat na účtu úložiště Azure, který je přidružen ke clusteru a měli byste vidět **/EventCount/EventCount10** složku vytvořit existuje.</span><span class="sxs-lookup"><span data-stu-id="0127f-259">To verify that the application runs successfully, you can look at your Azure storage account associated with the cluster and you should see the **/EventCount/EventCount10** folder created there.</span></span> <span data-ttu-id="0127f-260">Tato složka by měla obsahovat objekty BLOB, které jsou zaznamenány počet událostí, které jsou zpracovány v časovém období zadaná pro parametr **batch interval v sekundách**.</span><span class="sxs-lookup"><span data-stu-id="0127f-260">This folder should contain blobs that captures the number of events processed within the time period specified for the parameter **batch-interval-in-seconds**.</span></span>

<span data-ttu-id="0127f-261">Vysílání datového proudu aplikací Spark budou nadále spustit, dokud jste ho ukončit.</span><span class="sxs-lookup"><span data-stu-id="0127f-261">The Spark streaming application will continue to run until you kill it.</span></span> <span data-ttu-id="0127f-262">Uděláte to tak, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0127f-262">To do so, use the following command:</span></span>

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/1"

### <a name="run-the-applications-to-receive-the-events-into-an-azure-storage-blob-as-json"></a><span data-ttu-id="0127f-263">Spuštění aplikace, které chcete přijímat události do objektu Blob úložiště Azure jako JSON</span><span class="sxs-lookup"><span data-stu-id="0127f-263">Run the applications to receive the events into an Azure Storage Blob as JSON</span></span>
<span data-ttu-id="0127f-264">Otevřete příkazový řádek, přejděte do adresáře, kam jste nainstalovali CURL a spusťte následující příkaz (uživatelské jméno a heslo a cluster nahraďte název):</span><span class="sxs-lookup"><span data-stu-id="0127f-264">Open a command prompt, navigate to the directory where you installed CURL, and run the following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputJSON.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="0127f-265">Parametry v souboru **inputJSON.txt** jsou definovány takto:</span><span class="sxs-lookup"><span data-stu-id="0127f-265">The parameters in the file **inputJSON.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureBlobAsJSON", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-store-folder", "/EventStore10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="0127f-266">Parametry jsou podobná zadaný pro výstup textu, v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="0127f-266">The parameters are similar to what you specified for the text output, in the previous step.</span></span> <span data-ttu-id="0127f-267">Znovu není potřeba vytvořit výstupní složky (EventCheckpoint, EventCount/EventCount10), které se používají jako parametry.</span><span class="sxs-lookup"><span data-stu-id="0127f-267">Again, you do not need to create the output folders (EventCheckpoint, EventCount/EventCount10) that are used as parameters.</span></span> <span data-ttu-id="0127f-268">Je vytváří streamování aplikace.</span><span class="sxs-lookup"><span data-stu-id="0127f-268">The streaming application creates them for you.</span></span>

 <span data-ttu-id="0127f-269">Po spuštění příkazu, můžete se podívat na účtu úložiště Azure, který je přidružen ke clusteru a měli byste vidět **/EventStore10** složku vytvořit existuje.</span><span class="sxs-lookup"><span data-stu-id="0127f-269">After you run the command, you can look at your Azure storage account associated with the cluster and you should see the **/EventStore10** folder created there.</span></span> <span data-ttu-id="0127f-270">Otevřít libovolný soubor s předponou **část -** a měli byste vidět událostí zpracovaných ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="0127f-270">Open any file prefixed with **part-** and you should see the events processed in a JSON format.</span></span>

### <a name="run-the-applications-to-receive-the-events-into-a-hive-table"></a><span data-ttu-id="0127f-271">Spuštění aplikace, které chcete přijímat události do tabulky Hive</span><span class="sxs-lookup"><span data-stu-id="0127f-271">Run the applications to receive the events into a Hive table</span></span>
<span data-ttu-id="0127f-272">Spustit vysílání datového proudu aplikace, která datové proudy událostí do tabulky Hive Spark budete potřebovat některé další součásti.</span><span class="sxs-lookup"><span data-stu-id="0127f-272">To run the Spark streaming application that streams events into a Hive table you need some additional components.</span></span> <span data-ttu-id="0127f-273">Jsou to:</span><span class="sxs-lookup"><span data-stu-id="0127f-273">These are:</span></span>

* <span data-ttu-id="0127f-274">datanucleus rozhraní api jdo 3.2.6.jar</span><span class="sxs-lookup"><span data-stu-id="0127f-274">datanucleus-api-jdo-3.2.6.jar</span></span>
* <span data-ttu-id="0127f-275">datanucleus. rdbms 3.2.9.jar</span><span class="sxs-lookup"><span data-stu-id="0127f-275">datanucleus-rdbms-3.2.9.jar</span></span>
* <span data-ttu-id="0127f-276">datanucleus – základní – 3.2.10.jar</span><span class="sxs-lookup"><span data-stu-id="0127f-276">datanucleus-core-3.2.10.jar</span></span>
* <span data-ttu-id="0127f-277">Hive-site.xml</span><span class="sxs-lookup"><span data-stu-id="0127f-277">hive-site.xml</span></span>

<span data-ttu-id="0127f-278">**.Jar** soubory jsou k dispozici v clusteru HDInsight Spark na `/usr/hdp/current/spark-client/lib`.</span><span class="sxs-lookup"><span data-stu-id="0127f-278">The **.jar** files are available on your HDInsight Spark cluster at `/usr/hdp/current/spark-client/lib`.</span></span> <span data-ttu-id="0127f-279">**Hive-site.xml** je k dispozici na `/usr/hdp/current/spark-client/conf`.</span><span class="sxs-lookup"><span data-stu-id="0127f-279">The **hive-site.xml** is available at `/usr/hdp/current/spark-client/conf`.</span></span>

<span data-ttu-id="0127f-280">Můžete použít [WinScp](http://winscp.net/eng/download.php) zkopírujte tyto soubory z clusteru do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="0127f-280">You can use [WinScp](http://winscp.net/eng/download.php) to copy over these files from the cluster to your local computer.</span></span> <span data-ttu-id="0127f-281">Potom můžete nástroje pro kopírování těchto souborů přes do účtu úložiště, který je přidružen ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="0127f-281">You can then use tools to copy these files over to your storage account associated with the cluster.</span></span> <span data-ttu-id="0127f-282">Další informace o tom, jak nahrání souborů do účtu úložiště najdete v tématu [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="0127f-282">For more information on how to upload files to the storage account, see [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

<span data-ttu-id="0127f-283">Jakmile jste zkopírovali přes souborů do účtu úložiště Azure, otevřete příkazový řádek, přejděte do adresáře, kam jste nainstalovali CURL a spusťte následující příkaz (uživatelské jméno a heslo a cluster nahraďte název):</span><span class="sxs-lookup"><span data-stu-id="0127f-283">Once you have copied over the files to your Azure storage account, open a command prompt, navigate to the directory where you installed CURL, and run the following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputHive.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="0127f-284">Parametry v souboru **inputHive.txt** jsou definovány takto:</span><span class="sxs-lookup"><span data-stu-id="0127f-284">The parameters in the file **inputHive.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToHiveTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-hive-table", "EventHiveTable10" ], "jars":["wasb:///example/jars/datanucleus-api-jdo-3.2.6.jar", "wasb:///example/jars/datanucleus-rdbms-3.2.9.jar", "wasb:///example/jars/datanucleus-core-3.2.10.jar"], "files":["wasb:///example/jars/hive-site.xml"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="0127f-285">Parametry jsou podobná zadaný pro výstup textu, v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="0127f-285">The parameters are similar to what you specified for the text output, in the previous steps.</span></span> <span data-ttu-id="0127f-286">Znovu, není potřeba vytvořit výstupní složky (EventCheckpoint, EventCount/EventCount10) nebo výstupní tabulku Hive (EventHiveTable10), které se používají jako parametry.</span><span class="sxs-lookup"><span data-stu-id="0127f-286">Again, you do not need to create the output folders (EventCheckpoint, EventCount/EventCount10) or the output Hive table (EventHiveTable10) that are used as parameters.</span></span> <span data-ttu-id="0127f-287">Je vytváří streamování aplikace.</span><span class="sxs-lookup"><span data-stu-id="0127f-287">The streaming application creates them for you.</span></span> <span data-ttu-id="0127f-288">Všimněte si, že **JAR** a **soubory** možnost zahrne cesty k souborům .jar a hive-site.xml, který jste zkopírovali do účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="0127f-288">Note that the **jars** and **files** option includes paths to the .jar files and the hive-site.xml that you copied over to the storage account.</span></span>

<span data-ttu-id="0127f-289">Chcete-li ověřit, že byl úspěšně vytvořen tabulku hive, můžete SSH do clusteru a spouštění dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="0127f-289">To verify that the hive table was successfully created, you can SSH into the cluster and run Hive queries.</span></span> <span data-ttu-id="0127f-290">Pokyny najdete v tématu [používání Hive s Hadoop v HDInsight pomocí protokolu SSH](hdinsight-hadoop-use-hive-ssh.md).</span><span class="sxs-lookup"><span data-stu-id="0127f-290">For instructions, see [Use Hive with Hadoop in HDInsight with SSH](hdinsight-hadoop-use-hive-ssh.md).</span></span> <span data-ttu-id="0127f-291">Po připojení pomocí protokolu SSH, spuštěním následujícího příkazu ověřte, že v tabulce Hive **EventHiveTable10**, je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="0127f-291">Once you are connected using SSH, you can run the following command to verify that the Hive table, **EventHiveTable10**, is created.</span></span>

    show tables;

<span data-ttu-id="0127f-292">Zobrazený výstup by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="0127f-292">You should see an output similar to the following:</span></span>

    OK
    eventhivetable10
    hivesampletable

<span data-ttu-id="0127f-293">Můžete také spustit dotaz SELECT, který chcete zobrazit obsah v tabulce.</span><span class="sxs-lookup"><span data-stu-id="0127f-293">You can also run a SELECT query to view the contents of the table.</span></span>

    SELECT * FROM eventhivetable10 LIMIT 10;

<span data-ttu-id="0127f-294">Zobrazený výstup by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="0127f-294">You should see an output like the following:</span></span>

    ZN90apUSQODDTx7n6Toh6jDbuPngqT4c
    sor2M7xsFwmaRW8W8NDwMneFNMrOVkW1
    o2HcsU735ejSi2bGEcbUSB4btCFmI1lW
    TLuibq4rbj0T9st9eEzIWJwNGtMWYoYS
    HKCpPlWFWAJILwR69MAq863nCWYzDEw6
    Mvx0GQOPYvPR7ezBEpIHYKTKiEhYammQ
    85dRppSBSbZgThLr1s0GMgKqynDUqudr
    5LAWkNqorLj3ZN9a2mfWr9rZqeXKN4pF
    ulf9wSFNjD7BZXCyunozecov9QpEIYmJ
    vWzM3nvOja8DhYcwn0n5eTfOItZ966pa
    Time taken: 4.434 seconds, Fetched: 10 row(s)


### <a name="run-the-applications-to-receive-the-events-into-an-azure-sql-database-table"></a><span data-ttu-id="0127f-295">Spuštění aplikace, které chcete přijímat události do tabulky databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="0127f-295">Run the applications to receive the events into an Azure SQL database table</span></span>
<span data-ttu-id="0127f-296">Před spuštěním tohoto kroku, ujistěte se, že máte k Azure SQL database vytvořit.</span><span class="sxs-lookup"><span data-stu-id="0127f-296">Before running this step, make sure you have an Azure SQL database created.</span></span> <span data-ttu-id="0127f-297">Pokyny najdete v tématu [vytvoření databáze SQL v minutách](../sql-database/sql-database-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0127f-297">For instructions, see [Create a SQL database in minutes](../sql-database/sql-database-get-started.md).</span></span> <span data-ttu-id="0127f-298">K dokončení této části, potřebujete hodnoty pro název databáze, název databázového serveru a přihlašovací údaje správce databáze jako parametry.</span><span class="sxs-lookup"><span data-stu-id="0127f-298">To complete this section, you need values for database name, database server name, and the database administrator credentials as parameters.</span></span> <span data-ttu-id="0127f-299">Není nutné, když vytvoření databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="0127f-299">You do not need to create the database table though.</span></span> <span data-ttu-id="0127f-300">Vysílání datového proudu aplikací Spark vytvoří, které pro vás.</span><span class="sxs-lookup"><span data-stu-id="0127f-300">The Spark streaming application creates that for you.</span></span>

<span data-ttu-id="0127f-301">Otevřete příkazový řádek, přejděte do adresáře, kam jste nainstalovali CURL a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0127f-301">Open a command prompt, navigate to the directory where you installed CURL, and run the following command:</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputSQL.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="0127f-302">Parametry v souboru **inputSQL.txt** jsou definovány takto:</span><span class="sxs-lookup"><span data-stu-id="0127f-302">The parameters in the file **inputSQL.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureSQLTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--sql-server-fqdn", "<database-server-name>.database.windows.net", "--sql-database-name", "mysparkdatabase", "--database-username", "sparkdbadmin", "--database-password", "<put-password-here>", "--event-sql-table", "EventContent" ], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="0127f-303">Pokud chcete ověřit, že aplikace bude spuštěna úspěšně, můžete připojit k službě Azure SQL database pomocí SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="0127f-303">To verify that the application runs successfully, you can connect to the Azure SQL database using SQL Server Management Studio.</span></span> <span data-ttu-id="0127f-304">Pokyny o tom, jak to udělat najdete v tématu [připojit k SQL Database přes SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="0127f-304">For instructions on how to do that, see [Connect to SQL Database with SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md).</span></span> <span data-ttu-id="0127f-305">Po připojení k databázi, můžete přejít na **EventContent** tabulku, která byla vytvořena streamování aplikací.</span><span class="sxs-lookup"><span data-stu-id="0127f-305">Once you are connected to the database, you can navigate to the **EventContent** table that was created by the streaming application.</span></span> <span data-ttu-id="0127f-306">Můžete spustit Rychlý dotaz pro získání dat z tabulky.</span><span class="sxs-lookup"><span data-stu-id="0127f-306">You can run a quick query to get the data from the table.</span></span> <span data-ttu-id="0127f-307">Spusťte následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="0127f-307">Run the following query:</span></span>

    SELECT * FROM EventCount

<span data-ttu-id="0127f-308">Zobrazený výstup by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="0127f-308">You should see output similar to the following:</span></span>

    00046b0f-2552-4980-9c3f-8bba5647c8ee
    000b7530-12f9-4081-8e19-90acd26f9c0c
    000bc521-9c1b-4a42-ab08-dc1893b83f3b
    00123a2a-e00d-496a-9104-108920955718
    0017c68f-7a4e-452d-97ad-5cb1fe5ba81b
    001KsmqL2gfu5ZcuQuTqTxQvVyGCqPp9
    001vIZgOStka4DXtud0e3tX7XbfMnZrN
    00220586-3e1a-4d2d-a89b-05c5892e541a
    0029e309-9e54-4e1b-84be-cd04e6fce5ec
    003333cf-874f-4045-9da3-9f98c2b4ea49
    0043c07e-8d73-420a-9af7-1fcb94575356
    004a11a9-0c2c-4bc0-a7d5-2e0ebd947ab9


## <span data-ttu-id="0127f-309"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="0127f-309"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="0127f-310">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="0127f-310">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)
* [<span data-ttu-id="0127f-311">Návrh připojení na základě příjemce a přímé DStream</span><span class="sxs-lookup"><span data-stu-id="0127f-311">Design of Receiver-based Connection and Direct DStream</span></span>](https://www.slideshare.net/NanZhu/seattle-sparkmeetup032317)

### <a name="scenarios"></a><span data-ttu-id="0127f-312">Scénáře</span><span class="sxs-lookup"><span data-stu-id="0127f-312">Scenarios</span></span>
* [<span data-ttu-id="0127f-313">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="0127f-313">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="0127f-314">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="0127f-314">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="0127f-315">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="0127f-315">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="0127f-316">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="0127f-316">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="0127f-317">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="0127f-317">Create and run applications</span></span>
* [<span data-ttu-id="0127f-318">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="0127f-318">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="0127f-319">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="0127f-319">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="0127f-320">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="0127f-320">Tools and extensions</span></span>
* [<span data-ttu-id="0127f-321">Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="0127f-321">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="0127f-322">Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark</span><span class="sxs-lookup"><span data-stu-id="0127f-322">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="0127f-323">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="0127f-323">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="0127f-324">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="0127f-324">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="0127f-325">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="0127f-325">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="0127f-326">Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="0127f-326">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="0127f-327">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="0127f-327">Manage resources</span></span>
* [<span data-ttu-id="0127f-328">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="0127f-328">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="0127f-329">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="0127f-329">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]: ../storage-create-storage-account/
