---
title: "aaaReceive události z Azure Event Hubs pomocí Apache Storm | Microsoft Docs"
description: "Začínáme přijetí ze služby Event Hubs pomocí Apache Storm"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: java
ms.devlang: multiple
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: a0ab860ee8d504a28aac380c504c928f0d6dbc1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="receive-events-from-event-hubs-using-apache-storm"></a><span data-ttu-id="a648a-103">Přijímat události ze služby Event Hubs pomocí Apache Storm</span><span class="sxs-lookup"><span data-stu-id="a648a-103">Receive events from Event Hubs using Apache Storm</span></span>

<span data-ttu-id="a648a-104">[Apache Storm](https://storm.incubator.apache.org) je distribuované v reálném čase výpočetní systém, který zjednodušuje spolehlivé zpracování bez vazby datových proudů.</span><span class="sxs-lookup"><span data-stu-id="a648a-104">[Apache Storm](https://storm.incubator.apache.org) is a distributed real-time computation system that simplifies reliable processing of unbounded streams of data.</span></span> <span data-ttu-id="a648a-105">Tato část uvádí, jak toouse Azure Event Hubs Storm spout tooreceive události ze služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="a648a-105">This section shows how toouse an Azure Event Hubs Storm spout tooreceive events from Event Hubs.</span></span> <span data-ttu-id="a648a-106">Pomocí Apache Storm, můžete události rozdělit mezi více procesů, které jsou hostované v různých uzlech.</span><span class="sxs-lookup"><span data-stu-id="a648a-106">Using Apache Storm, you can split events across multiple processes hosted in different nodes.</span></span> <span data-ttu-id="a648a-107">Hello Event Hubs integrace s Storm zjednodušuje příjmu událostí tím, že transparentně vytváření kontrolních bodů průběhu pomocí instalace Zookeeper na Storm, spravuje trvalé kontrolní body a paralelní příjemce událostí ze služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="a648a-107">hello Event Hubs integration with Storm simplifies event consumption by transparently checkpointing its progress using Storm's Zookeeper installation, managing persistent checkpoints and parallel receives from Event Hubs.</span></span>

<span data-ttu-id="a648a-108">Další informace o službě Event Hubs přijímat vzory najdete v tématu hello [Přehled služby Event Hubs][Event Hubs overview].</span><span class="sxs-lookup"><span data-stu-id="a648a-108">For more information about Event Hubs receive patterns, see hello [Event Hubs overview][Event Hubs overview].</span></span>

## <a name="create-project-and-add-code"></a><span data-ttu-id="a648a-109">Vytvořte projekt a přidejte kód</span><span class="sxs-lookup"><span data-stu-id="a648a-109">Create project and add code</span></span>

<span data-ttu-id="a648a-110">Tento kurz používá [HDInsight Storm] [ HDInsight Storm] instalace, která se dodává s hello Event Hubs spout již k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a648a-110">This tutorial uses an [HDInsight Storm][HDInsight Storm] installation, which comes with hello Event Hubs spout already available.</span></span>

1. <span data-ttu-id="a648a-111">Postupujte podle hello [HDInsight Storm - Začínáme](../hdinsight/hdinsight-storm-overview.md) postup toocreate nové HDInsight clusteru a připojte tooit přes vzdálenou plochu.</span><span class="sxs-lookup"><span data-stu-id="a648a-111">Follow hello [HDInsight Storm - Get Started](../hdinsight/hdinsight-storm-overview.md) procedure toocreate a new HDInsight cluster, and connect tooit via Remote Desktop.</span></span>
2. <span data-ttu-id="a648a-112">Kopírování hello `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` souboru tooyour místní vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="a648a-112">Copy hello `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` file tooyour local development environment.</span></span> <span data-ttu-id="a648a-113">Tato položka obsahuje hello události storm-funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="a648a-113">This contains hello events-storm-spout.</span></span>
3. <span data-ttu-id="a648a-114">Použijte následující příkaz tooinstall hello balíček do místního úložiště Maven hello hello.</span><span class="sxs-lookup"><span data-stu-id="a648a-114">Use hello following command tooinstall hello package into hello local Maven store.</span></span> <span data-ttu-id="a648a-115">To vám umožní tooadd je jako vodítko při hello Storm v pozdější fázi projektu.</span><span class="sxs-lookup"><span data-stu-id="a648a-115">This enables you tooadd it as a reference in hello Storm project in a later step.</span></span>

    ```shell
    mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar
    ```
4. <span data-ttu-id="a648a-116">V nástroji Eclipse vytvořte nový projekt Maven (klikněte na tlačítko **soubor**, pak **nový**, pak **projektu**).</span><span class="sxs-lookup"><span data-stu-id="a648a-116">In Eclipse, create a new Maven project (click **File**, then **New**, then **Project**).</span></span>
   
    ![][12]
5. <span data-ttu-id="a648a-117">Vyberte **používat výchozí umístění prostoru**, pak klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="a648a-117">Select **Use default Workspace location**, then click **Next**</span></span>
6. <span data-ttu-id="a648a-118">Vyberte hello **rychlý start maven archetype** archetype, pak klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="a648a-118">Select hello **maven-archetype-quickstart** archetype, then click **Next**</span></span>
7. <span data-ttu-id="a648a-119">Vložení **GroupId** a **ArtifactId**, pak klikněte na tlačítko **dokončit**</span><span class="sxs-lookup"><span data-stu-id="a648a-119">Insert a **GroupId** and **ArtifactId**, then click **Finish**</span></span>
8. <span data-ttu-id="a648a-120">V **pom.xml**, přidejte následující závislé položky ve hello hello `<dependency>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="a648a-120">In **pom.xml**, add hello following dependencies in hello `<dependency>` node.</span></span>

    ```xml  
    <dependency>
        <groupId>org.apache.storm</groupId>
        <artifactId>storm-core</artifactId>
        <version>0.9.2-incubating</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>com.microsoft.eventhubs</groupId>
        <artifactId>eventhubs-storm-spout</artifactId>
        <version>0.9</version>
    </dependency>
    <dependency>
        <groupId>com.netflix.curator</groupId>
        <artifactId>curator-framework</artifactId>
        <version>1.3.3</version>
        <exclusions>
            <exclusion>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
            </exclusion>
            <exclusion>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
            </exclusion>
        </exclusions>
        <scope>provided</scope>
    </dependency>
    ```

9. <span data-ttu-id="a648a-121">V hello **src** složky, vytvořte soubor s názvem **Config.properties** a kopírování hello následující obsah, nahraďte hello `receive rule key` a `event hub name` hodnoty:</span><span class="sxs-lookup"><span data-stu-id="a648a-121">In hello **src** folder, create a file called **Config.properties** and copy hello following content, substituting hello `receive rule key` and `event hub name` values:</span></span>

    ```java
    eventhubspout.username = ReceiveRule
    eventhubspout.password = {receive rule key}
    eventhubspout.namespace = ioteventhub-ns
    eventhubspout.entitypath = {event hub name}
    eventhubspout.partitions.count = 16
       
    # if not provided, will use storm's zookeeper settings
    # zookeeper.connectionstring=localhost:2181
       
    eventhubspout.checkpoint.interval = 10
    eventhub.receiver.credits = 10
    ```
    <span data-ttu-id="a648a-122">Hello hodnotu **eventhub.receiver.credits** Určuje, kolik události jsou zpracovat v dávce před uvolněním je toohello Storm kanálu.</span><span class="sxs-lookup"><span data-stu-id="a648a-122">hello value for **eventhub.receiver.credits** determines how many events are batched before releasing them toohello Storm pipeline.</span></span> <span data-ttu-id="a648a-123">Tento příklad pro hello zájmu jednoduchost, nastaví too10 tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a648a-123">For hello sake of simplicity, this example sets this value too10.</span></span> <span data-ttu-id="a648a-124">V produkčním prostředí je potřeba obvykle ho nastavit hodnoty toohigher; například 1024.</span><span class="sxs-lookup"><span data-stu-id="a648a-124">In production, it should usually be set toohigher values; for example, 1024.</span></span>
10. <span data-ttu-id="a648a-125">Vytvořte novou třídu s názvem **LoggerBolt** s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="a648a-125">Create a new class called **LoggerBolt** with hello following code:</span></span>
    
    ```java
    import java.util.Map;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import backtype.storm.task.OutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichBolt;
    import backtype.storm.tuple.Tuple;
    
    public class LoggerBolt extends BaseRichBolt {
        private OutputCollector collector;
        private static final Logger logger = LoggerFactory
                  .getLogger(LoggerBolt.class);
    
        @Override
        public void execute(Tuple tuple) {
            String value = tuple.getString(0);
            logger.info("Tuple value: " + value);
   
            collector.ack(tuple);
        }
   
        @Override
        public void prepare(Map map, TopologyContext context, OutputCollector collector) {
            this.collector = collector;
            this.count = 0;
        }
        
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            // no output fields
        }
    
    }
    ```
    
    <span data-ttu-id="a648a-126">Touto funkcí bolt Storm zaznamená obsah hello hello přijatých událostí.</span><span class="sxs-lookup"><span data-stu-id="a648a-126">This Storm bolt logs hello content of hello received events.</span></span> <span data-ttu-id="a648a-127">To lze snadno rozšířit toostore řazených kolekcí členů v úložišti služby.</span><span class="sxs-lookup"><span data-stu-id="a648a-127">This can easily be extended toostore tuples in a storage service.</span></span> <span data-ttu-id="a648a-128">Hello [HDInsight senzor analysis kurzu] používá tato stejné toostore data přístup do HBase.</span><span class="sxs-lookup"><span data-stu-id="a648a-128">hello [HDInsight sensor analysis tutorial] uses this same approach toostore data into HBase.</span></span>
11. <span data-ttu-id="a648a-129">Vytvoření třídy s názvem **LogTopology** s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="a648a-129">Create a class called **LogTopology** with hello following code:</span></span>
    
    ```java
    import java.io.FileReader;
    import java.util.Properties;
    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.generated.StormTopology;
    import backtype.storm.topology.TopologyBuilder;
    import com.microsoft.eventhubs.samples.EventCount;
    import com.microsoft.eventhubs.spout.EventHubSpout;
    import com.microsoft.eventhubs.spout.EventHubSpoutConfig;
        
    public class LogTopology {
        protected EventHubSpoutConfig spoutConfig;
        protected int numWorkers;
        
        protected void readEHConfig(String[] args) throws Exception {
            Properties properties = new Properties();
            if (args.length > 1) {
                properties.load(new FileReader(args[1]));
            } else {
                properties.load(EventCount.class.getClassLoader()
                        .getResourceAsStream("Config.properties"));
            }
        
            String username = properties.getProperty("eventhubspout.username");
            String password = properties.getProperty("eventhubspout.password");
            String namespaceName = properties
                    .getProperty("eventhubspout.namespace");
            String entityPath = properties.getProperty("eventhubspout.entitypath");
            String zkEndpointAddress = properties
                    .getProperty("zookeeper.connectionstring"); // opt
            int partitionCount = Integer.parseInt(properties
                    .getProperty("eventhubspout.partitions.count"));
            int checkpointIntervalInSeconds = Integer.parseInt(properties
                    .getProperty("eventhubspout.checkpoint.interval"));
            int receiverCredits = Integer.parseInt(properties
                    .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                // (opt)
            System.out.println("Eventhub spout config: ");
            System.out.println("  partition count: " + partitionCount);
            System.out.println("  checkpoint interval: "
                    + checkpointIntervalInSeconds);
            System.out.println("  receiver credits: " + receiverCredits);
     
            spoutConfig = new EventHubSpoutConfig(username, password,
                    namespaceName, entityPath, partitionCount, zkEndpointAddress,
                    checkpointIntervalInSeconds, receiverCredits);
        
            // set hello number of workers toobe hello same as partition number.
            // hello idea is toohave a spout and a logger bolt co-exist in one
            // worker tooavoid shuffling messages across workers in storm cluster.
            numWorkers = spoutConfig.getPartitionCount();
        
            if (args.length > 0) {
                // set topology name so that sample Trident topology can use it as
                // stream name.
                spoutConfig.setTopologyName(args[0]);
            }
        }
        
        protected StormTopology buildTopology() {
            TopologyBuilder topologyBuilder = new TopologyBuilder();
       
            EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
            topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                    spoutConfig.getPartitionCount()).setNumTasks(
                    spoutConfig.getPartitionCount());
            topologyBuilder
                    .setBolt("LoggerBolt", new LoggerBolt(),
                            spoutConfig.getPartitionCount())
                    .localOrShuffleGrouping("EventHubsSpout")
                    .setNumTasks(spoutConfig.getPartitionCount());
            return topologyBuilder.createTopology();
        }
        
        protected void runScenario(String[] args) throws Exception {
            boolean runLocal = true;
            readEHConfig(args);
            StormTopology topology = buildTopology();
            Config config = new Config();
            config.setDebug(false);
        
            if (runLocal) {
                config.setMaxTaskParallelism(2);
                LocalCluster localCluster = new LocalCluster();
                localCluster.submitTopology("test", config, topology);
                Thread.sleep(5000000);
                localCluster.shutdown();
            } else {
                config.setNumWorkers(numWorkers);
                StormSubmitter.submitTopology(args[0], config, topology);
            }
        }
        
        public static void main(String[] args) throws Exception {
            LogTopology topology = new LogTopology();
            topology.runScenario(args);
        }
    }
    ```

    <span data-ttu-id="a648a-130">Tato třída se vytvoří novou funkcí spout centra událostí pomocí vlastností hello v hello konfigurační soubor tooinstantiate ho.</span><span class="sxs-lookup"><span data-stu-id="a648a-130">This class creates a new Event Hubs spout, using hello properties in hello configuration file tooinstantiate it.</span></span> <span data-ttu-id="a648a-131">Je důležité, že toonote, který tento příklad vytvoří jako mnoho spouts úlohy jako hello počet oddílů v Centru událostí hello v pořadí toouse hello maximální paralelismus povolenou tohoto centra událostí.</span><span class="sxs-lookup"><span data-stu-id="a648a-131">It is important toonote that this example creates as many spouts tasks as hello number of partitions in hello event hub, in order toouse hello maximum parallelism allowed by that event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a648a-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a648a-132">Next steps</span></span>
<span data-ttu-id="a648a-133">Další informace o službě Event Hubs návštěvou hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="a648a-133">You can learn more about Event Hubs by visiting hello following links:</span></span>

* <span data-ttu-id="a648a-134">[Přehled služby Event Hubs][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="a648a-134">[Event Hubs overview][Event Hubs overview]</span></span>
* [<span data-ttu-id="a648a-135">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="a648a-135">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="a648a-136">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="a648a-136">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[HDInsight Storm]: ../hdinsight/hdinsight-storm-overview.md
[HDInsight senzor analysis kurzu]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md

<!-- Images -->

[12]: ./media/event-hubs-get-started-receive-storm/create-storm1.png
