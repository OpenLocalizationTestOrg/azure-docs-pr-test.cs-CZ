---
title: "aaaUse Apache Spark streamování pomocí Event Hubs v Azure HDInsight | Microsoft Docs"
description: "Vytvoření ukázkové streamování Apache Spark na tom, jak toosend dat stream tooAzure centra událostí a poté zobrazí tyto události v clusteru HDInsight Spark pomocí scala aplikace."
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
ms.openlocfilehash: 10cc5884047b3b8249fe8a8822a16a19780a4af3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apache-spark-streaming-process-data-from-azure-event-hubs-with-spark-cluster-on-hdinsight"></a><span data-ttu-id="272c2-104">Apache Spark streamování: clusteru zpracování dat z Azure Event Hubs s Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="272c2-104">Apache Spark streaming: Process data from Azure Event Hubs with Spark cluster on HDInsight</span></span>

<span data-ttu-id="272c2-105">V tomto článku vytvořit Apache Spark streamování vzorku, který zahrnuje hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="272c2-105">In this article, you create an Apache Spark streaming sample that involves hello following steps:</span></span>

1. <span data-ttu-id="272c2-106">Použijete samostatné aplikace tooingest zprávy do centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="272c2-106">You use a standalone application tooingest messages into an Azure Event Hub.</span></span>

2. <span data-ttu-id="272c2-107">S dva různé přístupy můžete načítat zprávy hello z centra událostí v reálném čase pomocí aplikace běžící v clusteru Spark v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="272c2-107">With two different approaches, you retrieve hello messages from Event Hub in real-time using an application running in Spark cluster on Azure HDInsight.</span></span>

3. <span data-ttu-id="272c2-108">Vytvoření datových proudů analytické kanály toopersist data toodifferent úložných systémů nebo získáte přehledy z dat v chodu hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-108">You build streaming analytic pipelines toopersist data toodifferent storage systems, or get insights from data on hello fly.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="272c2-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="272c2-109">Prerequisites</span></span>

* <span data-ttu-id="272c2-110">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="272c2-110">An Azure subscription.</span></span> <span data-ttu-id="272c2-111">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="272c2-111">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="272c2-112">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="272c2-112">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="272c2-113">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="272c2-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="spark-streaming-concepts"></a><span data-ttu-id="272c2-114">Vysílání datového proudu Spark koncepty</span><span class="sxs-lookup"><span data-stu-id="272c2-114">Spark Streaming concepts</span></span>

<span data-ttu-id="272c2-115">Podrobné vysvětlení vysílání datového proudu Spark, najdete v článku [vysílání datového proudu přehled Apache Spark](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview).</span><span class="sxs-lookup"><span data-stu-id="272c2-115">For a detailed explanation of Spark streaming, see [Apache Spark streaming overview](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview).</span></span> <span data-ttu-id="272c2-116">HDInsight přináší hello clusteru stejné streamování tooa funkce Spark v Azure.</span><span class="sxs-lookup"><span data-stu-id="272c2-116">HDInsight brings hello same streaming features tooa Spark cluster on Azure.</span></span>  

## <a name="what-does-this-solution-do"></a><span data-ttu-id="272c2-117">Jakým způsobem tohoto řešení?</span><span class="sxs-lookup"><span data-stu-id="272c2-117">What does this solution do?</span></span>

<span data-ttu-id="272c2-118">V tomto článku, toocreate streamování příklad Spark, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="272c2-118">In this article, toocreate a Spark streaming example, perform hello following steps:</span></span>

1. <span data-ttu-id="272c2-119">Vytvoření centra událostí Azure, která bude přijímat události datového proudu.</span><span class="sxs-lookup"><span data-stu-id="272c2-119">Create an Azure Event Hub that will receive a stream of events.</span></span>

2. <span data-ttu-id="272c2-120">Spustit místní samostatná aplikace, která generuje události a nabízených oznámení toohello centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="272c2-120">Run a local standalone application that generates events and pushes it toohello Azure Event Hub.</span></span> <span data-ttu-id="272c2-121">Hello ukázkovou aplikaci, která k tomu je publikována v [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span><span class="sxs-lookup"><span data-stu-id="272c2-121">hello sample application that does this is published at [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span></span>

3. <span data-ttu-id="272c2-122">Vzdálené spuštění aplikace na streamování cluster Spark, který čte streamování událostí z centra událostí Azure a provádět různé zpracování dat nebo analýzu.</span><span class="sxs-lookup"><span data-stu-id="272c2-122">Run a streaming application remotely on a Spark cluster that reads streaming events from Azure Event Hub and perform various data processing/analysis.</span></span>

## <a name="create-an-azure-event-hub"></a><span data-ttu-id="272c2-123">Vytvoření centra událostí Azure</span><span class="sxs-lookup"><span data-stu-id="272c2-123">Create an Azure Event Hub</span></span>

1. <span data-ttu-id="272c2-124">Přihlaste se toohello [portálu Azure](https://ms.portal.azure.com)a klikněte na tlačítko **nový** v hello levém horním rohu úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="272c2-124">Log on toohello [Azure Portal](https://ms.portal.azure.com), and click **New** at hello top left of hello screen.</span></span>

2. <span data-ttu-id="272c2-125">Klikněte na **Internet věcí** a pak na **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="272c2-125">Click **Internet of Things**, then click **Event Hubs**.</span></span>

    <span data-ttu-id="272c2-126">![Centra událostí vytvořit pro příklad vysílání datového proudu Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "centra událostí vytvořit pro příklad vysílání datového proudu Spark")</span><span class="sxs-lookup"><span data-stu-id="272c2-126">![Create event hub for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "Create event hub for Spark streaming example")</span></span>

3. <span data-ttu-id="272c2-127">V hello **vytvoření oboru názvů** okno, zadejte název oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="272c2-127">In hello **Create namespace** blade, enter a namespace name.</span></span> <span data-ttu-id="272c2-128">Zvolte hello cenová úroveň (Basic nebo Standard).</span><span class="sxs-lookup"><span data-stu-id="272c2-128">choose hello pricing tier (Basic or Standard).</span></span> <span data-ttu-id="272c2-129">Také v který toocreate hello prostředek vyberte příslušné předplatné Azure, skupinu prostředků a umístění.</span><span class="sxs-lookup"><span data-stu-id="272c2-129">Also, choose an Azure subscription, resource group, and location in which toocreate hello resource.</span></span> <span data-ttu-id="272c2-130">Klikněte na tlačítko **vytvořit** toocreate hello oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="272c2-130">Click **Create** toocreate hello namespace.</span></span>

      <span data-ttu-id="272c2-131">![Zadejte název centra událostí příklad vysílání datového proudu Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "zadejte název centra událostí příklad vysílání datového proudu Spark")</span><span class="sxs-lookup"><span data-stu-id="272c2-131">![Provide an event hub name for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "Provide an event hub name for Spark streaming example")</span></span>

    > [!NOTE]
    > <span data-ttu-id="272c2-132">Jste měli vyberte hello stejné **umístění** jako cluster Apache Spark v HDInsight tooreduce latenci a náklady.</span><span class="sxs-lookup"><span data-stu-id="272c2-132">You should select hello same **Location** as your Apache Spark cluster in HDInsight tooreduce latency and costs.</span></span>
    >
    >

4. <span data-ttu-id="272c2-133">V seznamu názvů hello Event Hubs klikněte na tlačítko hello nově vytvořený obor názvů.</span><span class="sxs-lookup"><span data-stu-id="272c2-133">In hello Event Hubs namespace list, click hello newly-created namespace.</span></span>      


5. <span data-ttu-id="272c2-134">V okně hello obor názvů, klikněte na tlačítko **Event Hubs**a potom klikněte na **+ centra událostí** toocreate nového centra událostí.</span><span class="sxs-lookup"><span data-stu-id="272c2-134">In hello namespace blade, click **Event Hubs**, and then click **+ Event Hub** toocreate a new Event Hub.</span></span>
   
    <span data-ttu-id="272c2-135">![Centra událostí vytvořit pro příklad vysílání datového proudu Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "centra událostí vytvořit pro příklad vysílání datového proudu Spark")</span><span class="sxs-lookup"><span data-stu-id="272c2-135">![Create event hub for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "Create event hub for Spark streaming example")</span></span>

6. <span data-ttu-id="272c2-136">Zadejte název pro vaše Centrum událostí, sada hello oddílu počet too10 a too1 uchování zpráv.</span><span class="sxs-lookup"><span data-stu-id="272c2-136">Type a name for your Event Hub, set hello partition count too10, and message retention too1.</span></span> <span data-ttu-id="272c2-137">Hello zpráv v tomto řešení jsme nejsou archivace, můžete nechat hello rest jako výchozí a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="272c2-137">We are not archiving hello messages in this solution so you can leave hello rest as default, and then click **Create**.</span></span>
   
    <span data-ttu-id="272c2-138">![Zadejte podrobnosti o události rozbočovače pro příklad vysílání datového proudu Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "zadejte podrobnosti o události rozbočovače pro příklad vysílání datového proudu Spark")</span><span class="sxs-lookup"><span data-stu-id="272c2-138">![Provide event hub details for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "Provide event hub details for Spark streaming example")</span></span>

7. <span data-ttu-id="272c2-139">Hello nově vytvořený Centrum událostí je uvedena v okně centra událostí hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-139">hello newly created Event Hub is listed in hello Event Hub blade.</span></span>
    
     <span data-ttu-id="272c2-140">![Zobrazit centra událostí hello Spark streamování třeba](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "Centrum událostí zobrazení hello Spark streamování příklad")</span><span class="sxs-lookup"><span data-stu-id="272c2-140">![View Event Hub for hello Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "View Event Hub for hello Spark streaming example")</span></span>

8. <span data-ttu-id="272c2-141">Zpět v okně obor názvů hello (ne hello konkrétní Centrum událostí okno), klikněte na tlačítko **zásady sdíleného přístupu**a potom klikněte na **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="272c2-141">Back in hello namespace blade (not hello specific Event Hub blade), click **Shared access policies**, and then click **RootManageSharedAccessKey**.</span></span>
    
     <span data-ttu-id="272c2-142">![Nastavení zásad centra událostí, například hello Spark streamování](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "Spark streamování příklad zásady nastavení centra událostí pro hello")</span><span class="sxs-lookup"><span data-stu-id="272c2-142">![Set Event Hub policies for hello Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "Set Event Hub policies for hello Spark streaming example")</span></span>

9. <span data-ttu-id="272c2-143">Klikněte na tlačítko hello kopie tlačítko toocopy hello **RootManageSharedAccessKey** primární klíč a připojovací řetězec schránky toohello.</span><span class="sxs-lookup"><span data-stu-id="272c2-143">Click hello copy button toocopy hello **RootManageSharedAccessKey** primary key and connection string toohello clipboard.</span></span> <span data-ttu-id="272c2-144">Uložte tyto toouse později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-144">Save these toouse later in hello tutorial.</span></span>
    
     <span data-ttu-id="272c2-145">![Zobrazit centra událostí zásad klíče pro streamování příklad hello Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "centra událostí zobrazení zásad klíče pro hello Spark streamování příklad")</span><span class="sxs-lookup"><span data-stu-id="272c2-145">![View Event Hub policy keys for hello Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "View Event Hub policy keys for hello Spark streaming example")</span></span>

## <a name="send-messages-tooazure-event-hub-using-a-sample-scala-application"></a><span data-ttu-id="272c2-146">Odeslání zprávy tooAzure centra událostí pomocí Scala ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="272c2-146">Send messages tooAzure Event Hub using a sample Scala application</span></span>

<span data-ttu-id="272c2-147">V této části použijte samostatná místní Scala aplikace, která generuje datového proudu událostí a odešle ji tooAzure centra událostí, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="272c2-147">In this section you use a standalone local Scala application that generates a stream of events and sends it tooAzure Event Hub that you created earlier.</span></span> <span data-ttu-id="272c2-148">Tato aplikace je k dispozici na webu GitHub na [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).</span><span class="sxs-lookup"><span data-stu-id="272c2-148">This application is available on GitHub at [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).</span></span> <span data-ttu-id="272c2-149">Zde Hello kroky předpokládají, jste již forked toto úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="272c2-149">hello steps here assume that you have already forked this GitHub repository.</span></span>

1. <span data-ttu-id="272c2-150">Ujistěte se, že máte následující hello nainstalovaná na počítači hello kde spuštění této aplikace.</span><span class="sxs-lookup"><span data-stu-id="272c2-150">Make sure you have hello following installed on hello computer where you run this application.</span></span>

    * <span data-ttu-id="272c2-151">Java Development kit Oracle.</span><span class="sxs-lookup"><span data-stu-id="272c2-151">Oracle Java Development kit.</span></span> <span data-ttu-id="272c2-152">Můžete nainstalovat z [zde](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="272c2-152">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
    * <span data-ttu-id="272c2-153">Apache Maven.</span><span class="sxs-lookup"><span data-stu-id="272c2-153">Apache Maven.</span></span> <span data-ttu-id="272c2-154">Si můžete stáhnout z [zde](https://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="272c2-154">You can download it from [here](https://maven.apache.org/download.cgi).</span></span> <span data-ttu-id="272c2-155">Jsou k dispozici pokyny tooinstall Maven [zde](https://maven.apache.org/install.html).</span><span class="sxs-lookup"><span data-stu-id="272c2-155">Instructions tooinstall Maven are available [here](https://maven.apache.org/install.html).</span></span>

2. <span data-ttu-id="272c2-156">Otevřete příkazový řádek a přejděte toohello umístění, které jste naklonovali úložiště GitHub hello hello ukázka Scala aplikace a spusťte následující příkaz toobuild hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-156">Open a command prompt and navigate toohello location you cloned hello GitHub repo for hello sample Scala application and run hello following command toobuild hello application.</span></span>

        mvn package

3. <span data-ttu-id="272c2-157">jar výstup Hello aplikace hello **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, se vytvořil v rámci **/target** adresáře.</span><span class="sxs-lookup"><span data-stu-id="272c2-157">hello output jar for hello application, **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, is created under **/target** directory.</span></span> <span data-ttu-id="272c2-158">Můžete použít tento JAR později v tomto kompletní řešení článku tootest hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-158">You use this JAR later in this article tootest hello complete solution.</span></span>

## <a name="create-application-tooreceive-messages-from-event-hub-into-a-spark-cluster"></a><span data-ttu-id="272c2-159">Vytvoření aplikace tooreceive zprávy z centra událostí do clusteru Spark</span><span class="sxs-lookup"><span data-stu-id="272c2-159">Create application tooreceive messages from Event Hub into a Spark cluster</span></span> 

<span data-ttu-id="272c2-160">Máme dva přístupy tooconnect vysílání datového proudu Spark a Azure Event Hubs, na základě příjemce připojení a připojení na základě přímo DStream.</span><span class="sxs-lookup"><span data-stu-id="272c2-160">We have two approaches tooconnect Spark Streaming and Azure Event Hubs, Receiver-based connection and Direct-DStream-based connection.</span></span> <span data-ttu-id="272c2-161">Na základě přímo DStream je uvedený na ledna 2017 ve verzi hello 2.0.3.</span><span class="sxs-lookup"><span data-stu-id="272c2-161">Direct-DStream-based is introduced on Jan of 2017, in hello 2.0.3 release.</span></span> <span data-ttu-id="272c2-162">Protože to je další původce by mělo tooreplace hello původní na základě příjemce připojení a efektivní prostředků.</span><span class="sxs-lookup"><span data-stu-id="272c2-162">It is supposed tooreplace hello original receiver-based connection as it is more performant and resource-efficient.</span></span> <span data-ttu-id="272c2-163">Další informace uvedené v [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs).</span><span class="sxs-lookup"><span data-stu-id="272c2-163">More details found in [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs).</span></span> <span data-ttu-id="272c2-164">Přímé DStream podporuje pouze Spark 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="272c2-164">Direct DStream only supports Spark 2.0+.</span></span>

### <a name="build-applications-with-hello-dependency-toospark-eventhubs-connector"></a><span data-ttu-id="272c2-165">Vytváření aplikací s konektorem toospark-eventhubs závislostí hello</span><span class="sxs-lookup"><span data-stu-id="272c2-165">Build applications with hello dependency toospark-eventhubs connector</span></span>

<span data-ttu-id="272c2-166">Také jsme publikuje hello pracovní verzi Spark-EventHubs v Githubu.</span><span class="sxs-lookup"><span data-stu-id="272c2-166">We will also publish hello staging version of Spark-EventHubs in GitHub.</span></span> <span data-ttu-id="272c2-167">toouse hello pracovní verze Spark EventHubs, hello prvním krokem je tooindicate Githubu jako hello zdrojové úložiště přidáním následující položku toopom.xml hello:</span><span class="sxs-lookup"><span data-stu-id="272c2-167">toouse hello staging version of Spark-EventHubs, hello first step is tooindicate GitHub as hello source repo by adding hello following entry toopom.xml:</span></span>

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

<span data-ttu-id="272c2-168">Poté můžete přidat hello následující závislost tooyour projektu tootake hello předběžné verze.</span><span class="sxs-lookup"><span data-stu-id="272c2-168">You can then add hello following dependency tooyour project tootake hello pre-released version.</span></span>

<span data-ttu-id="272c2-169">Závislost maven</span><span class="sxs-lookup"><span data-stu-id="272c2-169">Maven Dependency</span></span>

```xml
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11 -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-streaming-eventhubs_2.11</artifactId>
    <version>2.0.4</version>
</dependency>
```

<span data-ttu-id="272c2-170">SBT závislostí</span><span class="sxs-lookup"><span data-stu-id="272c2-170">SBT Dependency</span></span>

```
// https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11
libraryDependencies += "com.microsoft.azure" % "spark-streaming-eventhubs_2.11" % "2.0.4"
```

### <a name="direct-dstream-connection"></a><span data-ttu-id="272c2-171">Přímé připojení DStream</span><span class="sxs-lookup"><span data-stu-id="272c2-171">Direct DStream Connection</span></span>

<span data-ttu-id="272c2-172">Jar předem připraveného souboru, který obsahuje příklady použití přímé DStream lze stáhnout v [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).</span><span class="sxs-lookup"><span data-stu-id="272c2-172">A pre-built jar file containing examples using Direct DStream can be downloaded in [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).</span></span>

<span data-ttu-id="272c2-173">soubor jar Hello obsahuje tři příklady, jejichž zdrojového kódu jsou k dispozici na [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).</span><span class="sxs-lookup"><span data-stu-id="272c2-173">hello jar file contains three examples whose source code are available at [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).</span></span>

<span data-ttu-id="272c2-174">Pořízení [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) jako příklad:</span><span class="sxs-lookup"><span data-stu-id="272c2-174">Taking [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) as an example:</span></span>

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

<span data-ttu-id="272c2-175">V hello výše například `eventhubParameters` jsou hello parametry konkrétní tooa jediné EventHubs instance a jestli máte toopass ho toohello `createDirectStreams` rozhraní API, která vytvoří obor názvů tooa přímé DStream objekt mapování Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="272c2-175">In hello above example, `eventhubParameters` are hello parameters specific tooa single EventHubs instance and you have toopass it toohello `createDirectStreams` API which constructs a Direct DStream object mapping tooa Event Hubs namespace.</span></span> <span data-ttu-id="272c2-176">Přes hello přímé DStream objekt můžete volat jakéhokoli DStream rozhraní API poskytované framework API vysílání datového proudu Spark.</span><span class="sxs-lookup"><span data-stu-id="272c2-176">Over hello Direct DStream object, you can call any DStream API provided by Spark Streaming API framework.</span></span> <span data-ttu-id="272c2-177">V tomto příkladu jsme vypočítat hello frekvenci jednotlivých slov v rámci hello poslední 3 malých batch intervalech.</span><span class="sxs-lookup"><span data-stu-id="272c2-177">In this example, we calculate hello frequency of each word within hello last 3 micro batch intervals.</span></span>

### <a name="receiver-based-connection"></a><span data-ttu-id="272c2-178">Na základě příjemce připojení</span><span class="sxs-lookup"><span data-stu-id="272c2-178">Receiver-based Connection</span></span>

<span data-ttu-id="272c2-179">Spark, streamování ukázková aplikace napsané v jazyce Scala, které přijímá události a trasy hello toodifferent cíle, které jsou k dispozici v [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span><span class="sxs-lookup"><span data-stu-id="272c2-179">A Spark streaming example application written in Scala, which receives events and route hello toodifferent destinations, is available at [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span></span> <span data-ttu-id="272c2-180">Postupujte podle kroků hello aplikace hello tooupdate pro konfiguraci centra událostí a vytvořte jar výstup hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-180">Follow hello steps below tooupdate hello application for your Event Hub configuration and create hello output jar.</span></span>

1. <span data-ttu-id="272c2-181">Spusťte IntelliJ IDEA a z hello spusťte obrazovky vyberte **rezervovat z verzí** a pak klikněte na **Git**.</span><span class="sxs-lookup"><span data-stu-id="272c2-181">Launch IntelliJ IDEA and from hello launch screen select **Check out from Version Control** and then click **Git**.</span></span>
   
    <span data-ttu-id="272c2-182">![Apache Spark streamování příklad - get zdroje z Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark streamování příklad - get zdroje z Git")</span><span class="sxs-lookup"><span data-stu-id="272c2-182">![Apache Spark streaming example - get sources from Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark streaming example - get sources from Git")</span></span>

2. <span data-ttu-id="272c2-183">V hello **klonování úložiště** dialogové okno, zadejte hello URL toohello Git úložiště tooclone z, zadejte tooclone directory hello k a pak klikněte na tlačítko **klon**.</span><span class="sxs-lookup"><span data-stu-id="272c2-183">In hello **Clone Repository** dialog box, provide hello URL toohello Git repository tooclone from, specify hello directory tooclone to, and then click **Clone**.</span></span>
   
    <span data-ttu-id="272c2-184">![Apache Spark streamování příklad - klonování z Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark streamování příklad - klonování z Git")</span><span class="sxs-lookup"><span data-stu-id="272c2-184">![Apache Spark streaming example - clone from Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark streaming example - clone from Git")</span></span>
3. <span data-ttu-id="272c2-185">Postupujte podle pokynů hello, dokud je zcela klonovat hello projektu.</span><span class="sxs-lookup"><span data-stu-id="272c2-185">Follow hello prompts till hello project is completely cloned.</span></span> <span data-ttu-id="272c2-186">Stiskněte klávesu **Alt + 1** tooopen hello **zobrazení projektu**.</span><span class="sxs-lookup"><span data-stu-id="272c2-186">Press **Alt + 1** tooopen hello **Project View**.</span></span> <span data-ttu-id="272c2-187">Měl by vypadat následující hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-187">It should resemble hello following.</span></span>
   
    <span data-ttu-id="272c2-188">![Apache Spark streamování příklad: zobrazení projektu](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark streamování příklad: zobrazení projektu")</span><span class="sxs-lookup"><span data-stu-id="272c2-188">![Apache Spark streaming example - Project View](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark streaming example - Project View")</span></span>
4. <span data-ttu-id="272c2-189">Ujistěte se, že je kód aplikace hello kompilovat s Java8.</span><span class="sxs-lookup"><span data-stu-id="272c2-189">Make sure hello application code is compiled with Java8.</span></span> <span data-ttu-id="272c2-190">tooensure tento, klikněte na tlačítko **soubor**, klikněte na tlačítko **strukturu projektu**a na hello **projektu** , zkontrolujte, zda je příliš nastavit úroveň projektu jazyka**8 - Lambdas, typu poznámky atd**.</span><span class="sxs-lookup"><span data-stu-id="272c2-190">tooensure this, click **File**, click **Project Structure**, and on hello **Project** tab, make sure Project language level is set too**8 - Lambdas, type annotations, etc.**.</span></span>
   
    <span data-ttu-id="272c2-191">![Apache Spark streamování příklad - Set kompilátoru](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark streamování příklad - Set kompilátoru")</span><span class="sxs-lookup"><span data-stu-id="272c2-191">![Apache Spark streaming example - Set compiler](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark streaming example - Set compiler")</span></span>
5. <span data-ttu-id="272c2-192">Otevřete hello **pom.xml** a ověřte správnost hello Spark verze.</span><span class="sxs-lookup"><span data-stu-id="272c2-192">Open hello **pom.xml** and make sure hello Spark version is correct.</span></span> <span data-ttu-id="272c2-193">V části `<properties>` uzlu, vyhledejte hello následující fragment kódu a ověření hello Spark verze.</span><span class="sxs-lookup"><span data-stu-id="272c2-193">Under `<properties>` node, look for hello following snippet and verify hello Spark version.</span></span>

        <scala.version>2.11.8</scala.version>
        <scala.compat.version>2.11.8</scala.compat.version>
        <scala.binary.version>2.11</scala.binary.version>
        <spark.version>2.0.0</spark.version>

6. <span data-ttu-id="272c2-194">aplikace Hello vyžaduje závislostí jar, nazývá **jar ovladač JDBC**.</span><span class="sxs-lookup"><span data-stu-id="272c2-194">hello application requires a dependency jar called **JDBC driver jar**.</span></span> <span data-ttu-id="272c2-195">Toto je požadovaná toowrite hello zprávy přijaté z centra událostí do Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="272c2-195">This is required toowrite hello messages received from Event Hub into an Azure SQL database.</span></span> <span data-ttu-id="272c2-196">Můžete si stáhnout tuto jar (v4.1 nebo novější) z [zde](https://msdn.microsoft.com/sqlserver/aa937724.aspx).</span><span class="sxs-lookup"><span data-stu-id="272c2-196">You can download this jar (v4.1 or later) from [here](https://msdn.microsoft.com/sqlserver/aa937724.aspx).</span></span> <span data-ttu-id="272c2-197">Přidáte odkaz na toothis jar hello projektu knihovny.</span><span class="sxs-lookup"><span data-stu-id="272c2-197">Add reference toothis jar in hello project library.</span></span> <span data-ttu-id="272c2-198">Proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="272c2-198">Perform hello following steps:</span></span>
     
     1. <span data-ttu-id="272c2-199">Z okna IntelliJ IDEA, kdy máte aplikaci hello otevřít, klikněte na tlačítko **soubor**, klikněte na tlačítko **strukturu projektu**a potom klikněte na **knihovny**.</span><span class="sxs-lookup"><span data-stu-id="272c2-199">From IntelliJ IDEA window where you have hello application open, click **File**, click **Project Structure**, and then click **Libraries**.</span></span> 
     2. <span data-ttu-id="272c2-200">Klikněte na tlačítko hello přidat ikonu (![přidat ikonu](./media/hdinsight-apache-spark-eventhub-streaming/add-icon.png)), klikněte na tlačítko **Java**a potom přejděte toohello umístění, kam jste stáhli jar ovladač JDBC hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-200">Click hello add icon (![add icon](./media/hdinsight-apache-spark-eventhub-streaming/add-icon.png)), click **Java**, and then navigate toohello location where you downloaded hello JDBC driver jar.</span></span> <span data-ttu-id="272c2-201">Postupujte podle hello výzvy tooadd hello jar soubor toohello projektu knihovny.</span><span class="sxs-lookup"><span data-stu-id="272c2-201">Follow hello prompts tooadd hello jar file toohello project library.</span></span>

         <span data-ttu-id="272c2-202">![přidejte chybějící závislosti](./media/hdinsight-apache-spark-eventhub-streaming/add-missing-dependency-jars.png "přidejte chybějící závislost JAR")</span><span class="sxs-lookup"><span data-stu-id="272c2-202">![add missing dependencies](./media/hdinsight-apache-spark-eventhub-streaming/add-missing-dependency-jars.png "Add missing dependency jars")</span></span>
     3. <span data-ttu-id="272c2-203">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="272c2-203">Click **Apply**.</span></span>

7. <span data-ttu-id="272c2-204">Vytvořte soubor jar výstup hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-204">Create hello output jar file.</span></span> <span data-ttu-id="272c2-205">Proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-205">Perform hello following steps.</span></span>

   1. <span data-ttu-id="272c2-206">V hello **strukturu projektu** dialogové okno, klikněte na tlačítko **artefakty** a pak klikněte na symbol plus – hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-206">In hello **Project Structure** dialog box, click **Artifacts** and then click hello plus symbol.</span></span> <span data-ttu-id="272c2-207">Z rozbalovací dialogové hello, klikněte na tlačítko **JAR**a potom klikněte na **z modulů se závislostmi**.</span><span class="sxs-lookup"><span data-stu-id="272c2-207">From hello pop-up dialog box, click **JAR**, and then click **From modules with dependencies**.</span></span>      
       
       <span data-ttu-id="272c2-208">![Apache Spark streamování příklad - vytvořit JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark streamování příklad - vytvořit JAR")</span><span class="sxs-lookup"><span data-stu-id="272c2-208">![Apache Spark streaming example - create JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark streaming example - create JAR")</span></span>
   2. <span data-ttu-id="272c2-209">V hello **vytvořit JAR z modulů** dialogové okno pole, klikněte na tlačítko se třemi tečkami hello (![třemi tečkami](./media/hdinsight-apache-spark-eventhub-streaming/ellipsis.png)) proti hello **hlavní třídy**.</span><span class="sxs-lookup"><span data-stu-id="272c2-209">In hello **Create JAR from Modules** dialog box, click hello ellipsis (![ellipsis](./media/hdinsight-apache-spark-eventhub-streaming/ellipsis.png)) against hello **Main Class**.</span></span>
   3. <span data-ttu-id="272c2-210">V hello **vyberte třídu hlavní** dialogové okno pole, vyberte některou z dostupných tříd hello a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="272c2-210">In hello **Select Main Class** dialog box, select any of hello available classes and then click **OK**.</span></span>
      
       <span data-ttu-id="272c2-211">![Apache Spark streamování příklad - vyberte třídu pro jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark streamování příklad - vyberte třídu pro jar")</span><span class="sxs-lookup"><span data-stu-id="272c2-211">![Apache Spark streaming example - select class for jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark streaming example - select class for jar")</span></span>
   4. <span data-ttu-id="272c2-212">V hello **vytvořit JAR z modulů** dialogové okno zkontrolujte, zda tuto možnost hello příliš**extrahovat toohello cíl JAR** je vybrána a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="272c2-212">In hello **Create JAR from Modules** dialog box, make sure that hello option too**extract toohello target JAR** is selected, and then click **OK**.</span></span> <span data-ttu-id="272c2-213">Tím se vytvoří jeden JAR s všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="272c2-213">This creates a single JAR with all dependencies.</span></span>
      
       <span data-ttu-id="272c2-214">![Apache Spark streamování příklad - vytvořit jar z modulů](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark streamování příklad - vytvořit jar z modulů")</span><span class="sxs-lookup"><span data-stu-id="272c2-214">![Apache Spark streaming example - create jar from modules](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark streaming example - create jar from modules")</span></span>
   5. <span data-ttu-id="272c2-215">Hello **výstup rozložení** karta Vypíše seznam všech hello JAR, které jsou součástí projekt Maven hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-215">hello **Output Layout** tab lists all hello jars that are included as part of hello Maven project.</span></span> <span data-ttu-id="272c2-216">Můžete vybrat a odstranění hello těch, které jsou na kterém hello Scala aplikace nemá žádné přímé závislostí.</span><span class="sxs-lookup"><span data-stu-id="272c2-216">You can select and delete hello ones on which hello Scala application has no direct dependency.</span></span> <span data-ttu-id="272c2-217">Pro aplikace hello tady vytváříme, můžete odebrat všechny ale hello naposledy (**spark streamování data trvalost příklady zkompilovat výstup**).</span><span class="sxs-lookup"><span data-stu-id="272c2-217">For hello application we are creating here, you can remove all but hello last one (**spark-streaming-data-persistence-examples compile output**).</span></span> <span data-ttu-id="272c2-218">Vyberte hello JAR toodelete a pak klikněte na hello **odstranit** ikona (![odstranit ikonu](./media/hdinsight-apache-spark-eventhub-streaming/delete-icon.png)).</span><span class="sxs-lookup"><span data-stu-id="272c2-218">Select hello jars toodelete and then click hello **Delete** icon (![delete icon](./media/hdinsight-apache-spark-eventhub-streaming/delete-icon.png)).</span></span>
      
       <span data-ttu-id="272c2-219">![Apache Spark streamování Příklad - odstranění extrahovat JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark streamování Příklad - odstranění extrahovat JAR")</span><span class="sxs-lookup"><span data-stu-id="272c2-219">![Apache Spark streaming example - delete extracted jars](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark streaming example - delete extracted jars")</span></span>
      
       <span data-ttu-id="272c2-220">Zajistěte, aby **sestavení na Ujistěte se,** je políčko zaškrtnuto, což zajistí, že jar hello se vytvoří pokaždé, když je vytvořené nebo aktualizované hello projektu.</span><span class="sxs-lookup"><span data-stu-id="272c2-220">Make sure **Build on make** box is selected, which ensures that hello jar is created every time hello project is built or updated.</span></span> <span data-ttu-id="272c2-221">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="272c2-221">Click **Apply**.</span></span>
   6. <span data-ttu-id="272c2-222">V hello **výstup rozložení** kartě vpravo dole hello hello **dostupné elementy** pole, máte hello SQL JDBC jar, zda jste přidali starší toohello projektu knihovny.</span><span class="sxs-lookup"><span data-stu-id="272c2-222">In hello **Output Layout** tab, right at hello bottom of hello **Available Elements** box, you have hello SQL JDBC jar that you added earlier toohello project library.</span></span> <span data-ttu-id="272c2-223">Je třeba přidat tento toohello **výstup rozložení** kartě. Klikněte pravým tlačítkem na soubor jar hello a pak klikněte na **extrahovat do kořenové výstup**.</span><span class="sxs-lookup"><span data-stu-id="272c2-223">You must add this toohello **Output Layout** tab. Right-click hello jar file, and then click **Extract Into Output Root**.</span></span>
      
       <span data-ttu-id="272c2-224">![Apache Spark streamování příklad - extract závislostí jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark streamování příklad - jar závislostí extrakce")</span><span class="sxs-lookup"><span data-stu-id="272c2-224">![Apache Spark streaming example - extract dependency jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark streaming example - extract dependency jar")</span></span>  
      
       <span data-ttu-id="272c2-225">Hello **výstup rozložení** karta by teď měl vypadat takto.</span><span class="sxs-lookup"><span data-stu-id="272c2-225">hello **Output Layout** tab should now look like this.</span></span>
      
       <span data-ttu-id="272c2-226">![Apache Spark streamování příklad - karta finální výstup](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark streamování příklad - karta finální výstup")</span><span class="sxs-lookup"><span data-stu-id="272c2-226">![Apache Spark streaming example - final output tab](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark streaming example - final output tab")</span></span>        
      
       <span data-ttu-id="272c2-227">V hello **strukturu projektu** dialogové okno, klikněte na tlačítko **použít** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="272c2-227">In hello **Project Structure** dialog box, click **Apply** and then click **OK**.</span></span>    
   7. <span data-ttu-id="272c2-228">V řádku nabídek hello, klikněte na **sestavení**a potom klikněte na **zkontrolujte projektu**.</span><span class="sxs-lookup"><span data-stu-id="272c2-228">From hello menu bar, click **Build**, and then click **Make Project**.</span></span> <span data-ttu-id="272c2-229">Můžete také kliknout na **sestavení artefaktů** toocreate hello jar.</span><span class="sxs-lookup"><span data-stu-id="272c2-229">You can also click **Build Artifacts** toocreate hello jar.</span></span> <span data-ttu-id="272c2-230">Hello jar výstup se vytvořil v rámci **\classes\artifacts**.</span><span class="sxs-lookup"><span data-stu-id="272c2-230">hello output jar is created under **\classes\artifacts**.</span></span>
      
       <span data-ttu-id="272c2-231">![Apache Spark streamování příklad – výstupní JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "streamování příklad Apache Spark – výstupní JAR")</span><span class="sxs-lookup"><span data-stu-id="272c2-231">![Apache Spark streaming example - output JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "Apache Spark streaming example - output JAR")</span></span>

## <a name="run-hello-application-remotely-on-a-spark-cluster-using-livy"></a><span data-ttu-id="272c2-232">Spuštění aplikace hello vzdáleně na clusteru Spark pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="272c2-232">Run hello application remotely on a Spark cluster using Livy</span></span>

<span data-ttu-id="272c2-233">V tomto článku jste Livy toorun hello Apache Spark streamování aplikaci používat vzdáleně na clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="272c2-233">In this article you use Livy toorun hello Apache Spark streaming application remotely on a Spark cluster.</span></span> <span data-ttu-id="272c2-234">Podrobné informace o tom, jak toouse Livy s HDInsight Spark clusteru, najdete v části [odeslání úlohy vzdáleně tooan Apache Spark clusteru v Azure HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="272c2-234">For detailed discussion on how toouse Livy with HDInsight Spark cluster, see [Submit jobs remotely tooan Apache Spark cluster on Azure HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span></span> <span data-ttu-id="272c2-235">Před zahájením spuštěna hello Spark streamování aplikace, existuje několik věcí, které byste měli udělat:</span><span class="sxs-lookup"><span data-stu-id="272c2-235">Before you can start running hello Spark streaming application, there are a couple of things you should do:</span></span>

1. <span data-ttu-id="272c2-236">Spouštění hello místní samostatné aplikace toogenerate událostí a odesílat tooEvent rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="272c2-236">Start hello local standalone application toogenerate events and sent tooEvent Hub.</span></span> <span data-ttu-id="272c2-237">Použijte následující příkaz toodo tak hello:</span><span class="sxs-lookup"><span data-stu-id="272c2-237">Use hello following command toodo so:</span></span>

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "mysbnamespace" --eventhubs-name "myeventhub" --policy-name "mysendpolicy" --policy-key "<policy key>" --message-length 32 --thread-count 32 --message-count -1

2. <span data-ttu-id="272c2-238">Kopírování hello streamování jar (**spark streamování data trvalost examples.jar**) toohello úložiště objektů Blob Azure přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="272c2-238">Copy hello streaming jar (**spark-streaming-data-persistence-examples.jar**) toohello Azure Blob storage associated with hello cluster.</span></span> <span data-ttu-id="272c2-239">Díky tomu přístupné tooLivy jar hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-239">This makes hello jar accessible tooLivy.</span></span> <span data-ttu-id="272c2-240">Můžete použít [ **AzCopy**](../storage/common/storage-use-azcopy.md), příkaz řádku nástroje pro toodo tak.</span><span class="sxs-lookup"><span data-stu-id="272c2-240">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command line utility, toodo so.</span></span> <span data-ttu-id="272c2-241">Existuje mnoho dalších klientů můžete použít tooupload data.</span><span class="sxs-lookup"><span data-stu-id="272c2-241">There are a lot of other clients you can use tooupload data.</span></span> <span data-ttu-id="272c2-242">Můžete najít další informace o nich v [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="272c2-242">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>
3. <span data-ttu-id="272c2-243">Nainstalujte na hello počítače, kde běží tyto aplikace z CURL.</span><span class="sxs-lookup"><span data-stu-id="272c2-243">Install CURL on hello computer where you are running these applications from.</span></span> <span data-ttu-id="272c2-244">Používáme hello tooinvoke CURL Livy koncové body toorun hello úlohy vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="272c2-244">We use CURL tooinvoke hello Livy endpoints toorun hello jobs remotely.</span></span>

### <a name="run-hello-spark-streaming-application-tooreceive-hello-events-into-an-azure-storage-blob-as-text"></a><span data-ttu-id="272c2-245">Spustit hello Spark streamování aplikací tooreceive hello události do objektu Blob úložiště Azure jako text</span><span class="sxs-lookup"><span data-stu-id="272c2-245">Run hello Spark streaming application tooreceive hello events into an Azure Storage Blob as text</span></span>

<span data-ttu-id="272c2-246">Otevřete příkazový řádek, přejděte toohello adresáře, kam jste nainstalovali CURL a spusťte následující příkaz (nahraďte název uživatelské jméno a heslo a cluster) hello:</span><span class="sxs-lookup"><span data-stu-id="272c2-246">Open a command prompt, navigate toohello directory where you installed CURL, and run hello following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputBlob.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="272c2-247">Hello parametry v souboru hello **inputBlob.txt** jsou definovány takto:</span><span class="sxs-lookup"><span data-stu-id="272c2-247">hello parameters in hello file **inputBlob.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsEventCount", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="272c2-248">Dejte nám vědět, jaké jsou parametry hello ve vstupním souboru hello:</span><span class="sxs-lookup"><span data-stu-id="272c2-248">Let us understand what hello parameters in hello input file are:</span></span>

* <span data-ttu-id="272c2-249">**soubor** je soubor jar hello cesta toohello aplikace v účtu úložiště Azure hello přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="272c2-249">**file** is hello path toohello application jar file on hello Azure storage account associated with hello cluster.</span></span>
* <span data-ttu-id="272c2-250">**Název třídy** je hello název třídy hello v hello jar.</span><span class="sxs-lookup"><span data-stu-id="272c2-250">**className** is hello name of hello class in hello jar.</span></span>
* <span data-ttu-id="272c2-251">**argumentů** hello seznam argumentů potřebných třídou hello</span><span class="sxs-lookup"><span data-stu-id="272c2-251">**args** is hello list of arguments required by hello class</span></span>
* <span data-ttu-id="272c2-252">**numExecutors** je hello počet jader používané hello toorun Spark streamování aplikace.</span><span class="sxs-lookup"><span data-stu-id="272c2-252">**numExecutors** is hello number of cores used by Spark toorun hello streaming application.</span></span> <span data-ttu-id="272c2-253">To by měl být vždy alespoň dvakrát hello počet oddílů centra událostí.</span><span class="sxs-lookup"><span data-stu-id="272c2-253">This should always be at least twice hello number of Event Hub partitions.</span></span>
* <span data-ttu-id="272c2-254">**executorMemory**, **executorCores**, **driverMemory** jsou použité parametry tooassign požadované prostředky toohello streamování aplikace.</span><span class="sxs-lookup"><span data-stu-id="272c2-254">**executorMemory**, **executorCores**, **driverMemory** are parameters used tooassign required resources toohello streaming application.</span></span>

> [!NOTE]
> <span data-ttu-id="272c2-255">Není nutné toocreate hello výstupní složky (EventCheckpoint, EventCount/EventCount10) používané jako parametry.</span><span class="sxs-lookup"><span data-stu-id="272c2-255">You do not need toocreate hello output folders (EventCheckpoint, EventCount/EventCount10) that are used as parameters.</span></span> <span data-ttu-id="272c2-256">Hello streamování aplikací je pro vás vytvoří.</span><span class="sxs-lookup"><span data-stu-id="272c2-256">hello streaming application creates them for you.</span></span>
>
>

<span data-ttu-id="272c2-257">Když spustíte příkaz hello, byste měli vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="272c2-257">When you run hello command, you should see an output like hello following:</span></span>

    < HTTP/1.1 201 Created
    < Content-Type: application/json; charset=UTF-8
    < Location: /18
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Tue, 01 Dec 2015 05:39:10 GMT
    < Content-Length: 37
    <
    {"id":1,"state":"starting","log":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact

<span data-ttu-id="272c2-258">Poznamenejte si ID dávky hello v hello poslední řádek výstupu hello (v tomto příkladu je '1').</span><span class="sxs-lookup"><span data-stu-id="272c2-258">Make a note of hello batch ID in hello last line of hello output (in this example it is '1').</span></span> <span data-ttu-id="272c2-259">tooverify, který hello aplikace úspěšně proběhne, můžete se podívat na vašem účtu úložiště Azure přidruženého k hello clusteru a měli byste vidět hello **/EventCount/EventCount10** složku vytvořit existuje.</span><span class="sxs-lookup"><span data-stu-id="272c2-259">tooverify that hello application runs successfully, you can look at your Azure storage account associated with hello cluster and you should see hello **/EventCount/EventCount10** folder created there.</span></span> <span data-ttu-id="272c2-260">Tato složka by měla obsahovat objekty BLOB, které jsou zaznamenány hello počet událostí zpracovat v rámci hello časové období zadaná pro parametr hello **batch interval v sekundách**.</span><span class="sxs-lookup"><span data-stu-id="272c2-260">This folder should contain blobs that captures hello number of events processed within hello time period specified for hello parameter **batch-interval-in-seconds**.</span></span>

<span data-ttu-id="272c2-261">Hello Spark streamování aplikace bude dále toorun, dokud jste ho ukončit.</span><span class="sxs-lookup"><span data-stu-id="272c2-261">hello Spark streaming application will continue toorun until you kill it.</span></span> <span data-ttu-id="272c2-262">toodo tedy použijte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="272c2-262">toodo so, use hello following command:</span></span>

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/1"

### <a name="run-hello-applications-tooreceive-hello-events-into-an-azure-storage-blob-as-json"></a><span data-ttu-id="272c2-263">Spuštění aplikace hello tooreceive hello události do objektu Blob úložiště Azure jako JSON</span><span class="sxs-lookup"><span data-stu-id="272c2-263">Run hello applications tooreceive hello events into an Azure Storage Blob as JSON</span></span>
<span data-ttu-id="272c2-264">Otevřete příkazový řádek, přejděte toohello adresáře, kam jste nainstalovali CURL a spusťte následující příkaz (nahraďte název uživatelské jméno a heslo a cluster) hello:</span><span class="sxs-lookup"><span data-stu-id="272c2-264">Open a command prompt, navigate toohello directory where you installed CURL, and run hello following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputJSON.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="272c2-265">Hello parametry v souboru hello **inputJSON.txt** jsou definovány takto:</span><span class="sxs-lookup"><span data-stu-id="272c2-265">hello parameters in hello file **inputJSON.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureBlobAsJSON", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-store-folder", "/EventStore10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="272c2-266">Hello parametry jsou podobné toowhat, které jste zadali pro hello textový výstup hello předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="272c2-266">hello parameters are similar toowhat you specified for hello text output, in hello previous step.</span></span> <span data-ttu-id="272c2-267">Znovu není nutné toocreate hello výstupní složky (EventCheckpoint, EventCount/EventCount10) používané jako parametry.</span><span class="sxs-lookup"><span data-stu-id="272c2-267">Again, you do not need toocreate hello output folders (EventCheckpoint, EventCount/EventCount10) that are used as parameters.</span></span> <span data-ttu-id="272c2-268">Hello streamování aplikací je pro vás vytvoří.</span><span class="sxs-lookup"><span data-stu-id="272c2-268">hello streaming application creates them for you.</span></span>

 <span data-ttu-id="272c2-269">Po spuštění příkazu hello, můžete se podívat na vašem účtu úložiště Azure přidruženého k hello clusteru a měli byste vidět hello **/EventStore10** složku vytvořit existuje.</span><span class="sxs-lookup"><span data-stu-id="272c2-269">After you run hello command, you can look at your Azure storage account associated with hello cluster and you should see hello **/EventStore10** folder created there.</span></span> <span data-ttu-id="272c2-270">Otevřít libovolný soubor s předponou **část -** a měli byste vidět hello událostí zpracovaných ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="272c2-270">Open any file prefixed with **part-** and you should see hello events processed in a JSON format.</span></span>

### <a name="run-hello-applications-tooreceive-hello-events-into-a-hive-table"></a><span data-ttu-id="272c2-271">Spouštění aplikací hello tooreceive hello události do tabulky Hive</span><span class="sxs-lookup"><span data-stu-id="272c2-271">Run hello applications tooreceive hello events into a Hive table</span></span>
<span data-ttu-id="272c2-272">toorun hello streamování aplikací Spark datových proudů událostí do podregistru tabulky budete potřebovat některé další součásti.</span><span class="sxs-lookup"><span data-stu-id="272c2-272">toorun hello Spark streaming application that streams events into a Hive table you need some additional components.</span></span> <span data-ttu-id="272c2-273">Jsou to:</span><span class="sxs-lookup"><span data-stu-id="272c2-273">These are:</span></span>

* <span data-ttu-id="272c2-274">datanucleus rozhraní api jdo 3.2.6.jar</span><span class="sxs-lookup"><span data-stu-id="272c2-274">datanucleus-api-jdo-3.2.6.jar</span></span>
* <span data-ttu-id="272c2-275">datanucleus. rdbms 3.2.9.jar</span><span class="sxs-lookup"><span data-stu-id="272c2-275">datanucleus-rdbms-3.2.9.jar</span></span>
* <span data-ttu-id="272c2-276">datanucleus – základní – 3.2.10.jar</span><span class="sxs-lookup"><span data-stu-id="272c2-276">datanucleus-core-3.2.10.jar</span></span>
* <span data-ttu-id="272c2-277">Hive-site.xml</span><span class="sxs-lookup"><span data-stu-id="272c2-277">hive-site.xml</span></span>

<span data-ttu-id="272c2-278">Hello **.jar** soubory jsou k dispozici v clusteru HDInsight Spark na `/usr/hdp/current/spark-client/lib`.</span><span class="sxs-lookup"><span data-stu-id="272c2-278">hello **.jar** files are available on your HDInsight Spark cluster at `/usr/hdp/current/spark-client/lib`.</span></span> <span data-ttu-id="272c2-279">Hello **hive-site.xml** je k dispozici na `/usr/hdp/current/spark-client/conf`.</span><span class="sxs-lookup"><span data-stu-id="272c2-279">hello **hive-site.xml** is available at `/usr/hdp/current/spark-client/conf`.</span></span>

<span data-ttu-id="272c2-280">Můžete použít [WinScp](http://winscp.net/eng/download.php) toocopy tyto soubory z místního počítače tooyour hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="272c2-280">You can use [WinScp](http://winscp.net/eng/download.php) toocopy over these files from hello cluster tooyour local computer.</span></span> <span data-ttu-id="272c2-281">Pak můžete použít nástroje pro toocopy těchto souborů přes účet úložiště tooyour přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="272c2-281">You can then use tools toocopy these files over tooyour storage account associated with hello cluster.</span></span> <span data-ttu-id="272c2-282">Další informace o tom, jak tooupload soubory toohello účtu úložiště najdete v tématu [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="272c2-282">For more information on how tooupload files toohello storage account, see [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

<span data-ttu-id="272c2-283">Po zkopírování přes hello soubory tooyour účtu úložiště Azure, otevřete příkazový řádek, přejděte toohello adresáře, kam jste nainstalovali CURL a spusťte následující příkaz (nahraďte název uživatelské jméno a heslo a cluster) hello:</span><span class="sxs-lookup"><span data-stu-id="272c2-283">Once you have copied over hello files tooyour Azure storage account, open a command prompt, navigate toohello directory where you installed CURL, and run hello following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputHive.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="272c2-284">Hello parametry v souboru hello **inputHive.txt** jsou definovány takto:</span><span class="sxs-lookup"><span data-stu-id="272c2-284">hello parameters in hello file **inputHive.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToHiveTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-hive-table", "EventHiveTable10" ], "jars":["wasb:///example/jars/datanucleus-api-jdo-3.2.6.jar", "wasb:///example/jars/datanucleus-rdbms-3.2.9.jar", "wasb:///example/jars/datanucleus-core-3.2.10.jar"], "files":["wasb:///example/jars/hive-site.xml"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="272c2-285">Hello parametry jsou podobné toowhat, které jste zadali pro textový výstup hello, v předchozích krocích hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-285">hello parameters are similar toowhat you specified for hello text output, in hello previous steps.</span></span> <span data-ttu-id="272c2-286">Znovu, není nutné toocreate hello výstupní složky (EventCheckpoint, EventCount/EventCount10) nebo hello výstupní tabulku Hive (EventHiveTable10), které se používají jako parametry.</span><span class="sxs-lookup"><span data-stu-id="272c2-286">Again, you do not need toocreate hello output folders (EventCheckpoint, EventCount/EventCount10) or hello output Hive table (EventHiveTable10) that are used as parameters.</span></span> <span data-ttu-id="272c2-287">Hello streamování aplikací je pro vás vytvoří.</span><span class="sxs-lookup"><span data-stu-id="272c2-287">hello streaming application creates them for you.</span></span> <span data-ttu-id="272c2-288">Všimněte si, že hello **JAR** a **soubory** možnost obsahuje soubory .jar toohello cesty a hello hive-site.xml, který jste zkopírovali přes toohello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="272c2-288">Note that hello **jars** and **files** option includes paths toohello .jar files and hello hive-site.xml that you copied over toohello storage account.</span></span>

<span data-ttu-id="272c2-289">byl úspěšně vytvořen tooverify, který hello tabulku hive, můžete SSH do clusteru hello a spouštění dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="272c2-289">tooverify that hello hive table was successfully created, you can SSH into hello cluster and run Hive queries.</span></span> <span data-ttu-id="272c2-290">Pokyny najdete v tématu [používání Hive s Hadoop v HDInsight pomocí protokolu SSH](hdinsight-hadoop-use-hive-ssh.md).</span><span class="sxs-lookup"><span data-stu-id="272c2-290">For instructions, see [Use Hive with Hadoop in HDInsight with SSH](hdinsight-hadoop-use-hive-ssh.md).</span></span> <span data-ttu-id="272c2-291">Po připojení pomocí protokolu SSH, můžete spustit následující příkaz tooverify hello tuto tabulku Hive hello, **EventHiveTable10**, je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="272c2-291">Once you are connected using SSH, you can run hello following command tooverify that hello Hive table, **EventHiveTable10**, is created.</span></span>

    show tables;

<span data-ttu-id="272c2-292">Měli byste vidět výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="272c2-292">You should see an output similar toohello following:</span></span>

    OK
    eventhivetable10
    hivesampletable

<span data-ttu-id="272c2-293">Můžete také spustit dotaz SELECT tooview hello obsah hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="272c2-293">You can also run a SELECT query tooview hello contents of hello table.</span></span>

    SELECT * FROM eventhivetable10 LIMIT 10;

<span data-ttu-id="272c2-294">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="272c2-294">You should see an output like hello following:</span></span>

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


### <a name="run-hello-applications-tooreceive-hello-events-into-an-azure-sql-database-table"></a><span data-ttu-id="272c2-295">Spouštění aplikací hello tooreceive hello události do tabulky databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="272c2-295">Run hello applications tooreceive hello events into an Azure SQL database table</span></span>
<span data-ttu-id="272c2-296">Před spuštěním tohoto kroku, ujistěte se, že máte k Azure SQL database vytvořit.</span><span class="sxs-lookup"><span data-stu-id="272c2-296">Before running this step, make sure you have an Azure SQL database created.</span></span> <span data-ttu-id="272c2-297">Pokyny najdete v tématu [vytvoření databáze SQL v minutách](../sql-database/sql-database-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="272c2-297">For instructions, see [Create a SQL database in minutes](../sql-database/sql-database-get-started.md).</span></span> <span data-ttu-id="272c2-298">toocomplete v této části, musíte hodnoty pro název databáze, název databázového serveru a přihlašovací údaje správce databáze hello jako parametry.</span><span class="sxs-lookup"><span data-stu-id="272c2-298">toocomplete this section, you need values for database name, database server name, and hello database administrator credentials as parameters.</span></span> <span data-ttu-id="272c2-299">Přestože nepotřebujete toocreate hello databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="272c2-299">You do not need toocreate hello database table though.</span></span> <span data-ttu-id="272c2-300">který vytvoří Hello streamování aplikací Spark.</span><span class="sxs-lookup"><span data-stu-id="272c2-300">hello Spark streaming application creates that for you.</span></span>

<span data-ttu-id="272c2-301">Otevřete příkazový řádek, přejděte toohello adresáře, kam jste nainstalovali CURL a spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="272c2-301">Open a command prompt, navigate toohello directory where you installed CURL, and run hello following command:</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputSQL.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="272c2-302">Hello parametry v souboru hello **inputSQL.txt** jsou definovány takto:</span><span class="sxs-lookup"><span data-stu-id="272c2-302">hello parameters in hello file **inputSQL.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureSQLTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--sql-server-fqdn", "<database-server-name>.database.windows.net", "--sql-database-name", "mysparkdatabase", "--database-username", "sparkdbadmin", "--database-password", "<put-password-here>", "--event-sql-table", "EventContent" ], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="272c2-303">tooverify, který hello aplikace spuštěna úspěšně, můžete připojit toohello Azure SQL database pomocí SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="272c2-303">tooverify that hello application runs successfully, you can connect toohello Azure SQL database using SQL Server Management Studio.</span></span> <span data-ttu-id="272c2-304">Návod, jak toodo, který najdete v části [připojit tooSQL databáze s SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="272c2-304">For instructions on how toodo that, see [Connect tooSQL Database with SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md).</span></span> <span data-ttu-id="272c2-305">Až se připojené toohello databáze, můžete přejít toohello **EventContent** tabulky, který byl vytvořen hello streamování aplikace.</span><span class="sxs-lookup"><span data-stu-id="272c2-305">Once you are connected toohello database, you can navigate toohello **EventContent** table that was created by hello streaming application.</span></span> <span data-ttu-id="272c2-306">Data hello tooget rychlé dotaz můžete spustit z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="272c2-306">You can run a quick query tooget hello data from hello table.</span></span> <span data-ttu-id="272c2-307">Spusťte následující dotaz hello:</span><span class="sxs-lookup"><span data-stu-id="272c2-307">Run hello following query:</span></span>

    SELECT * FROM EventCount

<span data-ttu-id="272c2-308">Měli byste vidět výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="272c2-308">You should see output similar toohello following:</span></span>

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


## <span data-ttu-id="272c2-309"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="272c2-309"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="272c2-310">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="272c2-310">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)
* [<span data-ttu-id="272c2-311">Návrh připojení na základě příjemce a přímé DStream</span><span class="sxs-lookup"><span data-stu-id="272c2-311">Design of Receiver-based Connection and Direct DStream</span></span>](https://www.slideshare.net/NanZhu/seattle-sparkmeetup032317)

### <a name="scenarios"></a><span data-ttu-id="272c2-312">Scénáře</span><span class="sxs-lookup"><span data-stu-id="272c2-312">Scenarios</span></span>
* [<span data-ttu-id="272c2-313">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="272c2-313">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="272c2-314">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="272c2-314">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="272c2-315">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="272c2-315">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="272c2-316">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="272c2-316">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="272c2-317">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="272c2-317">Create and run applications</span></span>
* [<span data-ttu-id="272c2-318">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="272c2-318">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="272c2-319">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="272c2-319">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="272c2-320">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="272c2-320">Tools and extensions</span></span>
* [<span data-ttu-id="272c2-321">Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="272c2-321">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="272c2-322">Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace</span><span class="sxs-lookup"><span data-stu-id="272c2-322">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="272c2-323">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="272c2-323">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="272c2-324">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="272c2-324">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="272c2-325">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="272c2-325">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="272c2-326">Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="272c2-326">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="272c2-327">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="272c2-327">Manage resources</span></span>
* [<span data-ttu-id="272c2-328">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="272c2-328">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="272c2-329">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="272c2-329">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]: ../storage-create-storage-account/
