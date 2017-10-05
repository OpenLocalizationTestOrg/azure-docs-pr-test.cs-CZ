---
title: "Azure Cosmos DB: Provádět analýzy grafu pomocí Spark a Apache TinkerPop Gremlin | Microsoft Docs"
description: "Tento článek představuje pokyny pro nastavení a spuštění graf analýzy a paralelní výpočty v Azure DB Cosmos s Spark a TinkerPop SparkGraphComputer."
services: cosmosdb
documentationcenter: 
author: khdang
manager: shireest
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: gremlin
ms.topic: article
ms.date: 06/05/2017
ms.author: khdang
ms.openlocfilehash: 27c4d945e418b130c68cfde845571eb93658101e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-perform-graph-analytics-by-using-spark-and-apache-tinkerpop-gremlin"></a><span data-ttu-id="b8fb5-103">Azure Cosmos DB: Proveďte analýzy grafu pomocí Spark a Apache TinkerPop Gremlin</span><span class="sxs-lookup"><span data-stu-id="b8fb5-103">Azure Cosmos DB: Perform graph analytics by using Spark and Apache TinkerPop Gremlin</span></span>

<span data-ttu-id="b8fb5-104">[Azure Cosmos DB](introduction.md) je služba databáze globálně distribuované a více modelech od společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-104">[Azure Cosmos DB](introduction.md) is the globally distributed, multi-model database service from Microsoft.</span></span> <span data-ttu-id="b8fb5-105">Můžete vytvořit a dotazovat dokumentu, klíč/hodnota a graf databáze, z nichž všechny využívat funkce globální distribuce a škálování vodorovných základem Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-105">You can create and query document, key/value, and graph databases, all of which benefit from the global-distribution and horizontal-scale capabilities at the core of Azure Cosmos DB.</span></span> <span data-ttu-id="b8fb5-106">Azure Cosmos DB podporuje online zpracování úlohy graf (OLTP), které používají transakcí [Apache TinkerPop Gremlin](graph-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b8fb5-106">Azure Cosmos DB supports online transaction processing (OLTP) graph workloads that use [Apache TinkerPop Gremlin](graph-introduction.md).</span></span>

<span data-ttu-id="b8fb5-107">[Spark](http://spark.apache.org/) je projekt Apache Software Foundation, který se zaměřuje na zpracování dat pro obecné účely online analytického zpracování (OLAP).</span><span class="sxs-lookup"><span data-stu-id="b8fb5-107">[Spark](http://spark.apache.org/) is an Apache Software Foundation project that's focused on general-purpose online analytical processing (OLAP) data processing.</span></span> <span data-ttu-id="b8fb5-108">Spark poskytuje hybridní v paměti nebo disk – na základě distribuovaná výpočetní model, který je podobný modelu Hadoop MapReduce.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-108">Spark provides a hybrid in-memory/disk-based distributed computing model that is similar to the Hadoop MapReduce model.</span></span> <span data-ttu-id="b8fb5-109">Apache Spark v cloudu můžete nasadit pomocí [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span><span class="sxs-lookup"><span data-stu-id="b8fb5-109">You can deploy Apache Spark in the cloud by using [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span></span>

<span data-ttu-id="b8fb5-110">Kombinací Azure Cosmos DB a Spark můžete provádět úlohy OLTP a technologií OLAP při použití Gremlin.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-110">By combining Azure Cosmos DB and Spark, you can perform both OLTP and OLAP workloads when you use Gremlin.</span></span> <span data-ttu-id="b8fb5-111">Tento úvodní článek ukazuje, jak ke spouštění dotazů Gremlin Azure Cosmos DB v clusteru Azure HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-111">This quick-start article demonstrates how to run Gremlin queries against Azure Cosmos DB on an Azure HDInsight Spark cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b8fb5-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b8fb5-112">Prerequisites</span></span>

<span data-ttu-id="b8fb5-113">Než budete moct tuto ukázku spustit, je potřeba splnit následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-113">Before you can run this sample, you must have the following prerequisites:</span></span>
* <span data-ttu-id="b8fb5-114">Cluster Azure HDInsight Spark 2.0</span><span class="sxs-lookup"><span data-stu-id="b8fb5-114">Azure HDInsight Spark cluster 2.0</span></span>
* <span data-ttu-id="b8fb5-115">JDK 1.8 + (Pokud nemáte JDK, spusťte `apt-get install default-jdk`.)</span><span class="sxs-lookup"><span data-stu-id="b8fb5-115">JDK 1.8+ (If you don't have JDK, run `apt-get install default-jdk`.)</span></span>
* <span data-ttu-id="b8fb5-116">Maven (Pokud nemáte Maven, spusťte `apt-get install maven`.)</span><span class="sxs-lookup"><span data-stu-id="b8fb5-116">Maven (If you don't have Maven, run `apt-get install maven`.)</span></span>
* <span data-ttu-id="b8fb5-117">Předplatné Azure ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])</span><span class="sxs-lookup"><span data-stu-id="b8fb5-117">An Azure subscription ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])</span></span>

<span data-ttu-id="b8fb5-118">Informace o tom, jak nastavit clusteru Azure HDInsight Spark najdete v tématu [clusterů HDInsight zřizování](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="b8fb5-118">For information about how to set up an Azure HDInsight Spark cluster, see [Provisioning HDInsight clusters](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="create-an-azure-cosmos-db-database-account"></a><span data-ttu-id="b8fb5-119">Vytvoření účtu Azure Cosmos DB databáze</span><span class="sxs-lookup"><span data-stu-id="b8fb5-119">Create an Azure Cosmos DB database account</span></span>

<span data-ttu-id="b8fb5-120">Nejprve vytvořte účet databáze s rozhraní Graph API následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-120">First, create a database account with the Graph API by doing the following:</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="b8fb5-121">Přidání kolekce</span><span class="sxs-lookup"><span data-stu-id="b8fb5-121">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="get-apache-tinkerpop"></a><span data-ttu-id="b8fb5-122">Získat Apache TinkerPop</span><span class="sxs-lookup"><span data-stu-id="b8fb5-122">Get Apache TinkerPop</span></span>

<span data-ttu-id="b8fb5-123">Získáte Apache TinkerPop následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-123">Get Apache TinkerPop by doing the following:</span></span>

1. <span data-ttu-id="b8fb5-124">Vzdálené k hlavního uzlu clusteru HDInsight `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-124">Remote to the master node of the HDInsight cluster `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.</span></span>

2. <span data-ttu-id="b8fb5-125">Klonování TinkerPop3 zdrojový kód, ji vytvořit místně a nainstalujte ji do mezipaměti Maven.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-125">Clone the TinkerPop3 source code, build it locally, and install it to Maven cache.</span></span>

    ```bash
    git clone https://github.com/apache/tinkerpop.git
    cd tinkerpop
    mvn clean install
    ```

3. <span data-ttu-id="b8fb5-126">Instalace modulu plug-in Gremlin Spark</span><span class="sxs-lookup"><span data-stu-id="b8fb5-126">Install the Spark-Gremlin plug-in</span></span> 

    <span data-ttu-id="b8fb5-127">a.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-127">a.</span></span> <span data-ttu-id="b8fb5-128">Instalace modulu plug-in se zpracovává souborem hroznového.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-128">The installation of the plug-in is handled by Grape.</span></span> <span data-ttu-id="b8fb5-129">Naplnění úložiště informace o hroznového, můžete si stáhnout modul plug-in a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-129">Populate the repositories information for Grape so it can download the plug-in and its dependencies.</span></span> 

      <span data-ttu-id="b8fb5-130">Vytvoření hroznového konfiguračního souboru, pokud není přítomen v `~/.groovy/grapeConfig.xml`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-130">Create the grape configuration file if it's not present at `~/.groovy/grapeConfig.xml`.</span></span> <span data-ttu-id="b8fb5-131">Použijte následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-131">Use the following settings:</span></span>

    ```xml
    <ivysettings>
    <settings defaultResolver="downloadGrapes"/>
    <resolvers>
        <chain name="downloadGrapes">
        <filesystem name="cachedGrapes">
            <ivy pattern="${user.home}/.groovy/grapes/[organisation]/[module]/ivy-[revision].xml"/>
            <artifact pattern="${user.home}/.groovy/grapes/[organisation]/[module]/[type]s/[artifact]-[revision].[ext]"/>
        </filesystem>
        <ibiblio name="codehaus" root="http://repository.codehaus.org/" m2compatible="true"/>
        <ibiblio name="central" root="http://central.maven.org/maven2/" m2compatible="true"/>
        <ibiblio name="jitpack" root="https://jitpack.io" m2compatible="true"/>
        <ibiblio name="java.net2" root="http://download.java.net/maven/2/" m2compatible="true"/>
        <ibiblio name="apache-snapshots" root="http://repository.apache.org/snapshots/" m2compatible="true"/>
        <ibiblio name="local" root="file:${user.home}/.m2/repository/" m2compatible="true"/>
        </chain>
    </resolvers>
    </ivysettings>
    ``` 

    <span data-ttu-id="b8fb5-132">b.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-132">b.</span></span> <span data-ttu-id="b8fb5-133">Spusťte konzolu Gremlin `bin/gremlin.sh`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-133">Start Gremlin console `bin/gremlin.sh`.</span></span>
        
    <span data-ttu-id="b8fb5-134">c.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-134">c.</span></span> <span data-ttu-id="b8fb5-135">Instalace modulu plug-in Gremlin Spark s verzí 3.3.0-SNAPSHOT, který jste vytvořili v předchozích krocích:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-135">Install the Spark-Gremlin plug-in with version 3.3.0-SNAPSHOT, which you built in the previous steps:</span></span>

    ```bash
    $ bin/gremlin.sh

            \,,,/
            (o o)
    -----oOOo-(3)-oOOo-----
    plugin activated: tinkerpop.server
    plugin activated: tinkerpop.utilities
    plugin activated: tinkerpop.tinkergraph
    gremlin> :install org.apache.tinkerpop spark-gremlin 3.3.0-SNAPSHOT
    ==>loaded: [org.apache.tinkerpop, spark-gremlin, 3.3.0-SNAPSHOT] - restart the console to use [tinkerpop.spark]
    gremlin> :q
    $ bin/gremlin.sh

            \,,,/
            (o o)
    -----oOOo-(3)-oOOo-----
    plugin activated: tinkerpop.server
    plugin activated: tinkerpop.utilities
    plugin activated: tinkerpop.tinkergraph
    gremlin> :plugin use tinkerpop.spark
    ==>tinkerpop.spark activated
    ```

4. <span data-ttu-id="b8fb5-136">Zkontrolujte, zda `Hadoop-Gremlin` se aktivuje s `:plugin list`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-136">Check to see whether `Hadoop-Gremlin` is activated with `:plugin list`.</span></span> <span data-ttu-id="b8fb5-137">Zakázat tento modul plug-in, protože by mohl narušovat modulu plug-in Gremlin Spark `:plugin unuse tinkerpop.hadoop`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-137">Disable this plug-in, because it could interfere with the Spark-Gremlin plug-in `:plugin unuse tinkerpop.hadoop`.</span></span>

## <a name="prepare-tinkerpop3-dependencies"></a><span data-ttu-id="b8fb5-138">Příprava TinkerPop3 závislosti</span><span class="sxs-lookup"><span data-stu-id="b8fb5-138">Prepare TinkerPop3 dependencies</span></span>

<span data-ttu-id="b8fb5-139">Po TinkerPop3 jste vytvořili v předchozím kroku, tento proces také vyžádány všechny závislosti jar pro Spark a Hadoop v cílovém adresáři.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-139">When you built TinkerPop3 in the previous step, the process also pulled all jar dependencies for Spark and Hadoop in the target directory.</span></span> <span data-ttu-id="b8fb5-140">Použijte JAR, které jsou předem nainstalované s HDI a vyžádá další závislosti pouze podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-140">Use the jars that are pre-installed with HDI, and pull in additional dependencies only as necessary.</span></span>

1. <span data-ttu-id="b8fb5-141">Přejděte do adresáře Gremlin konzole cílové v `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-141">Go to the Gremlin Console target directory at `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`.</span></span> 

2. <span data-ttu-id="b8fb5-142">Přesunout všechny JAR pod `ext/` k `lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-142">Move all jars under `ext/` to `lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.</span></span>

3. <span data-ttu-id="b8fb5-143">Odeberte všechny jar knihovny pod `lib/` které nejsou v následujícím seznamu:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-143">Remove all jar libraries under `lib/` that are not in the following list:</span></span>

    ```bash
    # TinkerPop3
    gremlin-console-3.3.0-SNAPSHOT.jar
    gremlin-core-3.3.0-SNAPSHOT.jar       
    gremlin-groovy-3.3.0-SNAPSHOT.jar     
    gremlin-shaded-3.3.0-SNAPSHOT.jar     
    hadoop-gremlin-3.3.0-SNAPSHOT.jar     
    spark-gremlin-3.3.0-SNAPSHOT.jar      
    tinkergraph-gremlin-3.3.0-SNAPSHOT.jar

    # Gremlin depedencies
    asm-3.2.jar                                
    avro-1.7.4.jar                             
    caffeine-2.3.1.jar                         
    cglib-2.2.1-v20090111.jar                  
    gbench-0.4.3-groovy-2.4.jar                
    gprof-0.3.1-groovy-2.4.jar                 
    groovy-2.4.9-indy.jar                      
    groovy-2.4.9.jar                           
    groovy-console-2.4.9.jar                   
    groovy-groovysh-2.4.9-indy.jar             
    groovy-json-2.4.9-indy.jar                 
    groovy-jsr223-2.4.9-indy.jar               
    groovy-sql-2.4.9-indy.jar                  
    groovy-swing-2.4.9.jar                     
    groovy-templates-2.4.9.jar                 
    groovy-xml-2.4.9.jar                       
    hadoop-yarn-server-nodemanager-2.7.2.jar   
    hppc-0.7.1.jar                             
    javatuples-1.2.jar                         
    jaxb-impl-2.2.3-1.jar                      
    jbcrypt-0.4.jar                            
    jcabi-log-0.14.jar                         
    jcabi-manifests-1.1.jar                    
    jersey-core-1.9.jar                        
    jersey-guice-1.9.jar                       
    jersey-json-1.9.jar                        
    jettison-1.1.jar                           
    scalatest_2.11-2.2.6.jar                   
    servlet-api-2.5.jar                        
    snakeyaml-1.15.jar                         
    unused-1.0.0.jar                           
    xml-apis-1.3.04.jar                        
    ```

## <a name="get-the-azure-cosmos-db-spark-connector"></a><span data-ttu-id="b8fb5-144">Získat konektor Azure Cosmos DB Spark</span><span class="sxs-lookup"><span data-stu-id="b8fb5-144">Get the Azure Cosmos DB Spark connector</span></span>

1. <span data-ttu-id="b8fb5-145">Získat konektor Azure Cosmos DB Spark `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` a Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` z [konektoru služby Azure DB Spark Cosmos na Githubu](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).</span><span class="sxs-lookup"><span data-stu-id="b8fb5-145">Get the Azure Cosmos DB Spark connector `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` and Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` from [Azure Cosmos DB Spark Connector on GitHub](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).</span></span>

2. <span data-ttu-id="b8fb5-146">Alternativně můžete vytvořit ho místně.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-146">Alternatively, you can build it locally.</span></span> <span data-ttu-id="b8fb5-147">Protože nejnovější verzi Spark Gremlin byl sestaven s Spark 1.6.1 a není kompatibilní s Spark bodu 2.0.2, který je aktuálně používán konektor Azure Cosmos DB Spark, můžete sestavit kód nejnovější TinkerPop3 a nainstalujte JAR ručně.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-147">Because the latest version of Spark-Gremlin was built with Spark 1.6.1 and is not compatible with Spark 2.0.2, which is currently used in the Azure Cosmos DB Spark connector, you can build the latest TinkerPop3 code and install the jars manually.</span></span> <span data-ttu-id="b8fb5-148">Udělejte toto:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-148">Do the following:</span></span>

    <span data-ttu-id="b8fb5-149">a.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-149">a.</span></span> <span data-ttu-id="b8fb5-150">Klonování konektor Azure Cosmos DB Spark.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-150">Clone the Azure Cosmos DB Spark connector.</span></span>

    <span data-ttu-id="b8fb5-151">b.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-151">b.</span></span> <span data-ttu-id="b8fb5-152">Sestavení TinkerPop3 (neudělali v předchozích krocích).</span><span class="sxs-lookup"><span data-stu-id="b8fb5-152">Build TinkerPop3 (already done in previous steps).</span></span> <span data-ttu-id="b8fb5-153">Nainstalujte všechny TinkerPop 3.3.0-SNAPSHOT JAR místně.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-153">Install all TinkerPop 3.3.0-SNAPSHOT jars locally.</span></span>

    ```bash
    mvn install:install-file -Dfile="gremlin-core-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-core -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar
    mvn install:install-file -Dfile="gremlin-groovy-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-groovy -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="gremlin-shaded-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-shaded -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="hadoop-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=hadoop-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="spark-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=spark-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="tinkergraph-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=tinkergraph-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    ```

    <span data-ttu-id="b8fb5-154">c.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-154">c.</span></span> <span data-ttu-id="b8fb5-155">Aktualizace `tinkerpop.version` `azure-documentdb-spark/pom.xml` k `3.3.0-SNAPSHOT`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-155">Update `tinkerpop.version` `azure-documentdb-spark/pom.xml` to `3.3.0-SNAPSHOT`.</span></span>
    
    <span data-ttu-id="b8fb5-156">d.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-156">d.</span></span> <span data-ttu-id="b8fb5-157">Sestavení s Maven.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-157">Build with Maven.</span></span> <span data-ttu-id="b8fb5-158">Potřebné JAR jsou umístěny v `target` a `target/alternateLocation`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-158">The needed jars are placed in `target` and `target/alternateLocation`.</span></span>

    ```bash
    git clone https://github.com/Azure/azure-cosmosdb-spark.git
    cd azure-documentdb-spark
    mvn clean package
    ```

3. <span data-ttu-id="b8fb5-159">Zkopírujte dříve uvedených JAR do místního adresáře v ~ / azure-documentdb-spark:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-159">Copy the previously mentioned jars to a local directory at ~/azure-documentdb-spark:</span></span>

    ```bash
    $ azure-documentdb-spark:
    mkdir ~/azure-documentdb-spark
    cp target/azure-documentdb-spark-0.0.3-SNAPSHOT.jar ~/azure-documentdb-spark
    cp target/alternateLocation/azure-documentdb-1.10.0.jar ~/azure-documentdb-spark
    ```

## <a name="distribute-the-dependencies-to-the-spark-worker-nodes"></a><span data-ttu-id="b8fb5-160">Distribuovat závislosti k pracovním uzlům Spark</span><span class="sxs-lookup"><span data-stu-id="b8fb5-160">Distribute the dependencies to the Spark worker nodes</span></span> 

1. <span data-ttu-id="b8fb5-161">Protože transformaci dat grafu. závisí na TinkerPop3, je nutné distribuovat související závislosti pro všechny uzly pracovního procesu Spark.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-161">Because the transformation of graph data depends on TinkerPop3, you must distribute the related dependencies to all Spark worker nodes.</span></span>

2. <span data-ttu-id="b8fb5-162">Zkopírujte dříve uvedených Gremlin závislosti, CosmosDB Spark konektor jar a CosmosDB Java SDK k pracovním uzlům následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-162">Copy the previously mentioned Gremlin dependencies, the CosmosDB Spark connector jar, and CosmosDB Java SDK to the worker nodes by doing the following:</span></span>

    <span data-ttu-id="b8fb5-163">a.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-163">a.</span></span> <span data-ttu-id="b8fb5-164">Zkopírujte všechny JAR do `~/azure-documentdb-spark`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-164">Copy all the jars into `~/azure-documentdb-spark`.</span></span>

    ```bash
    $ /home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone:
    cp lib/* ~/azure-documentdb-spark
    ```

    <span data-ttu-id="b8fb5-165">b.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-165">b.</span></span> <span data-ttu-id="b8fb5-166">Získání seznamu všechny uzly pracovního procesu Spark, které můžete najít na řídicím panelu Ambari v `Spark2 Clients` v seznamu `Spark2` oddílu.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-166">Get the list of all Spark worker nodes, which you can find on Ambari Dashboard, in the `Spark2 Clients` list in the `Spark2` section.</span></span>

    <span data-ttu-id="b8fb5-167">c.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-167">c.</span></span> <span data-ttu-id="b8fb5-168">Zkopírujte tento adresář na všech uzlech.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-168">Copy that directory to each of the nodes.</span></span>

    ```bash
    scp -r ~/azure-documentdb-spark sshuser@wn0-cosmos:/home/sshuser
    scp -r ~/azure-documentdb-spark sshuser@wn1-cosmos:/home/sshuser
    ...
    ```
    
## <a name="set-up-the-environment-variables"></a><span data-ttu-id="b8fb5-169">Nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="b8fb5-169">Set up the environment variables</span></span>

1. <span data-ttu-id="b8fb5-170">Najděte verzi softwaru HDP clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-170">Find the HDP version of the Spark cluster.</span></span> <span data-ttu-id="b8fb5-171">Je název adresáře v rámci `/usr/hdp/` (například 2.5.4.2-7).</span><span class="sxs-lookup"><span data-stu-id="b8fb5-171">It is the directory name under `/usr/hdp/` (for example, 2.5.4.2-7).</span></span>

2. <span data-ttu-id="b8fb5-172">Nastavte hdp.version pro všechny uzly.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-172">Set hdp.version for all nodes.</span></span> <span data-ttu-id="b8fb5-173">Na řídicím panelu Ambari, přejděte na **YARN části** > **konfigurací** > **Upřesnit**, a poté proveďte následující:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-173">In Ambari Dashboard, go to **YARN section** > **Configs** > **Advanced**, and then do the following:</span></span> 
 
    <span data-ttu-id="b8fb5-174">a.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-174">a.</span></span> <span data-ttu-id="b8fb5-175">V `Custom yarn-site`, přidejte novou vlastnost `hdp.version` s hodnotou verze softwaru HDP na hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-175">In `Custom yarn-site`, add a new property `hdp.version` with the value of the HDP version on the master node.</span></span> 
     
    <span data-ttu-id="b8fb5-176">b.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-176">b.</span></span> <span data-ttu-id="b8fb5-177">Uložte konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-177">Save the configurations.</span></span> <span data-ttu-id="b8fb5-178">Existují upozornění, které můžete ignorovat.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-178">There are warnings, which you can ignore.</span></span> 
     
    <span data-ttu-id="b8fb5-179">c.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-179">c.</span></span> <span data-ttu-id="b8fb5-180">Restartujte služby YARN a Oozie jako ikony oznámení označují.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-180">Restart the YARN and Oozie services as the notification icons indicate.</span></span>

3. <span data-ttu-id="b8fb5-181">Nastavte následující proměnné prostředí v hlavním uzlu (Nahraďte hodnoty podle potřeby):</span><span class="sxs-lookup"><span data-stu-id="b8fb5-181">Set the following environment variables on the master node (replace the values as appropriate):</span></span>

    ```bash
    export HADOOP_GREMLIN_LIBS=/home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/ext/spark-gremlin/lib
    export CLASSPATH=$CLASSPATH:$HADOOP_CONF_DIR:/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    export HDP_VERSION=2.5.4.2-7
    export HADOOP_HOME=${HADOOP_HOME:-/usr/hdp/current/hadoop-client}
    ```

## <a name="prepare-the-graph-configuration"></a><span data-ttu-id="b8fb5-182">Příprava konfiguraci grafu</span><span class="sxs-lookup"><span data-stu-id="b8fb5-182">Prepare the graph configuration</span></span>

1. <span data-ttu-id="b8fb5-183">Vytvoření konfiguračního souboru s Azure Cosmos DB parametry připojení a nastavení Spark a umístí jej na `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-183">Create a configuration file with the Azure Cosmos DB connection parameters and Spark settings, and put it at `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.</span></span>

    ```
    gremlin.graph=org.apache.tinkerpop.gremlin.hadoop.structure.HadoopGraph
    gremlin.hadoop.jarsInDistributedCache=true
    gremlin.hadoop.defaultGraphComputer=org.apache.tinkerpop.gremlin.spark.process.computer.SparkGraphComputer

    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    gremlin.hadoop.graphWriter=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBOutputRDD

    ####################################
    # SparkGraphComputer Configuration #
    ####################################
    spark.master=yarn
    spark.executor.memory=3g
    spark.executor.instances=6
    spark.serializer=org.apache.spark.serializer.KryoSerializer
    spark.kryo.registrator=org.apache.tinkerpop.gremlin.spark.structure.io.gryo.GryoRegistrator
    gremlin.spark.persistContext=true

    # Classpath for the driver and executors
    spark.driver.extraClassPath=/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    spark.executor.extraClassPath=/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    
    ######################################
    # DocumentDB Spark connector         #
    ######################################
    spark.documentdb.connectionMode=Gateway
    spark.documentdb.schema_samplingratio=1.0
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    spark.documentdb.preferredRegions=FILLIN
    ```

2. <span data-ttu-id="b8fb5-184">Aktualizace `spark.driver.extraClassPath` a `spark.executor.extraClassPath` zahrnout do adresáře JAR distribuované v předchozím kroku, se v tomto případě `/home/sshuser/azure-documentdb-spark/*`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-184">Update the `spark.driver.extraClassPath` and `spark.executor.extraClassPath` to include the directory of the jars that you distributed in the previous step, in this case `/home/sshuser/azure-documentdb-spark/*`.</span></span>

3. <span data-ttu-id="b8fb5-185">Zadejte následující podrobnosti pro Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-185">Provide the following details for Azure Cosmos DB:</span></span>

    ```
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    # Optional
    #spark.documentdb.preferredRegions=West\ US;West\ US\ 2
    ```
   
## <a name="load-the-tinkerpop-graph-and-save-it-to-azure-cosmos-db"></a><span data-ttu-id="b8fb5-186">Načíst TinkerPop grafu a uložit ho do Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b8fb5-186">Load the TinkerPop graph, and save it to Azure Cosmos DB</span></span>
<span data-ttu-id="b8fb5-187">Abychom ukázali, jak se zachovat graf do Azure Cosmos databáze, tento příklad používá TinkerPop předdefinované TinkerPop moderní grafu.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-187">To demonstrate how to persist a graph into Azure Cosmos DB, this example uses the TinkerPop predefined TinkerPop modern graph.</span></span> <span data-ttu-id="b8fb5-188">Graf je uložený ve formátu Kryo a je k dispozici v úložišti TinkerPop.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-188">The graph is stored in Kryo format, and it's provided in the TinkerPop repository.</span></span>

1. <span data-ttu-id="b8fb5-189">Protože Gremlin je spuštěn v režimu YARN, je třeba zpřístupnit data grafu v systému souborů Hadoop.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-189">Because you are running Gremlin in YARN mode, you must make the graph data available in the Hadoop file system.</span></span> <span data-ttu-id="b8fb5-190">Použijte následující příkazy a aby byl adresář zkopírujte soubor místní grafu do ní.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-190">Use the following commands to make a directory and copy the local graph file into it.</span></span> 

    ```bash
    $ tinkerpop:
    hadoop fs -mkdir /graphData
    hadoop fs -copyFromLocal ~/tinkerpop/data/tinkerpop-modern.kryo /graphData/tinkerpop-modern.kryo
    ```

2. <span data-ttu-id="b8fb5-191">Dočasně aktualizovat `gremlin-spark.properties` soubor `GryoInputFormat` číst grafu.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-191">Temporarily update the `gremlin-spark.properties` file to use `GryoInputFormat` to read the graph.</span></span> <span data-ttu-id="b8fb5-192">Taky jednat `inputLocation` jako adresář vytvoříte, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-192">Also indicate `inputLocation` as the directory you create, as in the following:</span></span>

    ```
    gremlin.hadoop.graphReader=org.apache.tinkerpop.gremlin.hadoop.structure.io.gryo.GryoInputFormat
    gremlin.hadoop.inputLocation=/graphData/tinkerpop-modern.kryo
    ```

3. <span data-ttu-id="b8fb5-193">Spusťte konzolu Gremlin a pak vytvořte následující kroky výpočtu pro uložení dat do nakonfigurované kolekce Cosmos databázi Azure:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-193">Start Gremlin Console, and then create the following computation steps to persist data to the configured Azure Cosmos DB collection:</span></span>  

    <span data-ttu-id="b8fb5-194">a.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-194">a.</span></span> <span data-ttu-id="b8fb5-195">Vytvoření grafu `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-195">Create the graph `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.</span></span>

    <span data-ttu-id="b8fb5-196">b.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-196">b.</span></span> <span data-ttu-id="b8fb5-197">Použít SparkGraphComputer pro zápis `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-197">Use SparkGraphComputer for writing `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.</span></span>

    ```bash
    gremlin> graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")
    ==>hadoopgraph[gryoinputformat->documentdboutputrdd]
    gremlin> hg = graph.
                compute(SparkGraphComputer.class).
                result(GraphComputer.ResultGraph.NEW).
                persist(GraphComputer.Persist.EDGES).
                program(TraversalVertexProgram.build().
                    traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)), "gremlin-groovy", "g.V()").
                    create(graph)).
                submit().
                get() 
    ==>result[hadoopgraph[documentdbinputrdd->documentdboutputrdd],memory[size:1]]
    ```

4. <span data-ttu-id="b8fb5-198">V Průzkumníku dat můžete ověřit, že data se k databázi Azure Cosmos jako trvalý.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-198">From Data Explorer, you can verify that the data has been persisted to Azure Cosmos DB.</span></span>

## <a name="load-the-graph-from-azure-cosmos-db-and-run-gremlin-queries"></a><span data-ttu-id="b8fb5-199">Načtení grafu ze Azure Cosmos DB a spouštět dotazy Gremlin</span><span class="sxs-lookup"><span data-stu-id="b8fb5-199">Load the graph from Azure Cosmos DB, and run Gremlin queries</span></span>

1. <span data-ttu-id="b8fb5-200">Chcete-li načíst graf, upravte `gremlin-spark.properties` nastavit `graphReader` k `DocumentDBInputRDD`:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-200">To load the graph, edit `gremlin-spark.properties` to set `graphReader` to `DocumentDBInputRDD`:</span></span>

    ```
    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    ```

2. <span data-ttu-id="b8fb5-201">Načíst graf, procházet data a spouštět dotazy Gremlin ho následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-201">Load the graph, traverse the data, and run Gremlin queries with it by doing the following:</span></span>

    <span data-ttu-id="b8fb5-202">a.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-202">a.</span></span> <span data-ttu-id="b8fb5-203">Spusťte konzolu Gremlin `bin/gremlin.sh`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-203">Start the Gremlin Console `bin/gremlin.sh`.</span></span>

    <span data-ttu-id="b8fb5-204">b.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-204">b.</span></span> <span data-ttu-id="b8fb5-205">Vytvoření grafu s konfigurací `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-205">Create the graph with the configuration `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.</span></span>

    <span data-ttu-id="b8fb5-206">c.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-206">c.</span></span> <span data-ttu-id="b8fb5-207">Vytvoření grafu traversal s SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-207">Create a graph traversal with SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.</span></span>

    <span data-ttu-id="b8fb5-208">d.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-208">d.</span></span> <span data-ttu-id="b8fb5-209">Spusťte následující dotazy Gremlin grafu:</span><span class="sxs-lookup"><span data-stu-id="b8fb5-209">Run the following Gremlin graph queries:</span></span>

    ```bash
    gremlin> graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")
    ==>hadoopgraph[documentdbinputrdd->documentdboutputrdd]
    gremlin> g = graph.traversal().withComputer(SparkGraphComputer)
    ==>graphtraversalsource[hadoopgraph[documentdbinputrdd->documentdboutputrdd], sparkgraphcomputer]
    gremlin> g.V().count()
    ==>6
    gremlin> g.E().count()
    ==>6
    gremlin> g.V(1).out().values('name')
    ==>josh
    ==>vadas
    ==>lop
    gremlin> g.V().hasLabel('person').coalesce(values('nickname'), values('name'))
    ==>josh
    ==>peter
    ==>vadas
    ==>marko
    gremlin> g.V().hasLabel('person').
            choose(values('name')).
                option('marko', values('age')).
                option('josh', values('name')).
                option('vadas', valueMap()).
                option('peter', label())
    ==>josh
    ==>person
    ==>[name:[vadas],age:[27]]
    ==>29
    ```

> [!NOTE]
> <span data-ttu-id="b8fb5-210">Pokud chcete zobrazit podrobnější protokolování, nastavte úroveň v protokolu `conf/log4j-console.properties` na podrobnější úrovni.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-210">To see more detailed logging, set the log level in `conf/log4j-console.properties` to a more verbose level.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="b8fb5-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8fb5-211">Next steps</span></span>

<span data-ttu-id="b8fb5-212">V tomto článku úvodní jsme zjistili, jak pracovat s grafy kombinací Azure Cosmos DB a Spark.</span><span class="sxs-lookup"><span data-stu-id="b8fb5-212">In this quick-start article, you've learned how to work with graphs by combining Azure Cosmos DB and Spark.</span></span>

> [!div class="nextstepaction"]
