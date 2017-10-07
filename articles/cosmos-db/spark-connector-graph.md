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
ms.openlocfilehash: 0be5c9b12cdba4a428c809d00e1e68785a9ec1ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-perform-graph-analytics-by-using-spark-and-apache-tinkerpop-gremlin"></a><span data-ttu-id="ad13a-103">Azure Cosmos DB: Proveďte analýzy grafu pomocí Spark a Apache TinkerPop Gremlin</span><span class="sxs-lookup"><span data-stu-id="ad13a-103">Azure Cosmos DB: Perform graph analytics by using Spark and Apache TinkerPop Gremlin</span></span>

<span data-ttu-id="ad13a-104">[Azure Cosmos DB](introduction.md) je hello globálně distribuované a více modelech databáze služby společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ad13a-104">[Azure Cosmos DB](introduction.md) is hello globally distributed, multi-model database service from Microsoft.</span></span> <span data-ttu-id="ad13a-105">Můžete vytvořit a dotaz dokumentu, klíč/hodnota a databáze grafu, všechny z nich využívat hello globální distribuce a škálování vodorovných funkce jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ad13a-105">You can create and query document, key/value, and graph databases, all of which benefit from hello global-distribution and horizontal-scale capabilities at hello core of Azure Cosmos DB.</span></span> <span data-ttu-id="ad13a-106">Azure Cosmos DB podporuje online zpracování úlohy graf (OLTP), které používají transakcí [Apache TinkerPop Gremlin](graph-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ad13a-106">Azure Cosmos DB supports online transaction processing (OLTP) graph workloads that use [Apache TinkerPop Gremlin](graph-introduction.md).</span></span>

<span data-ttu-id="ad13a-107">[Spark](http://spark.apache.org/) je projekt Apache Software Foundation, který se zaměřuje na zpracování dat pro obecné účely online analytického zpracování (OLAP).</span><span class="sxs-lookup"><span data-stu-id="ad13a-107">[Spark](http://spark.apache.org/) is an Apache Software Foundation project that's focused on general-purpose online analytical processing (OLAP) data processing.</span></span> <span data-ttu-id="ad13a-108">Spark poskytuje hybridní v paměti nebo disk – na základě distribuovaná výpočetní model, který je podobný toohello model Hadoop MapReduce.</span><span class="sxs-lookup"><span data-stu-id="ad13a-108">Spark provides a hybrid in-memory/disk-based distributed computing model that is similar toohello Hadoop MapReduce model.</span></span> <span data-ttu-id="ad13a-109">Apache Spark v cloudu hello můžete nasadit pomocí [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span><span class="sxs-lookup"><span data-stu-id="ad13a-109">You can deploy Apache Spark in hello cloud by using [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span></span>

<span data-ttu-id="ad13a-110">Kombinací Azure Cosmos DB a Spark můžete provádět úlohy OLTP a technologií OLAP při použití Gremlin.</span><span class="sxs-lookup"><span data-stu-id="ad13a-110">By combining Azure Cosmos DB and Spark, you can perform both OLTP and OLAP workloads when you use Gremlin.</span></span> <span data-ttu-id="ad13a-111">Tento úvodní článek ukazuje, jak toorun Gremlin dotazy pro Azure Cosmos DB v clusteru Azure HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="ad13a-111">This quick-start article demonstrates how toorun Gremlin queries against Azure Cosmos DB on an Azure HDInsight Spark cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ad13a-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ad13a-112">Prerequisites</span></span>

<span data-ttu-id="ad13a-113">Před spuštěním této ukázce, musíte mít hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="ad13a-113">Before you can run this sample, you must have hello following prerequisites:</span></span>
* <span data-ttu-id="ad13a-114">Cluster Azure HDInsight Spark 2.0</span><span class="sxs-lookup"><span data-stu-id="ad13a-114">Azure HDInsight Spark cluster 2.0</span></span>
* <span data-ttu-id="ad13a-115">JDK 1.8 + (Pokud nemáte JDK, spusťte `apt-get install default-jdk`.)</span><span class="sxs-lookup"><span data-stu-id="ad13a-115">JDK 1.8+ (If you don't have JDK, run `apt-get install default-jdk`.)</span></span>
* <span data-ttu-id="ad13a-116">Maven (Pokud nemáte Maven, spusťte `apt-get install maven`.)</span><span class="sxs-lookup"><span data-stu-id="ad13a-116">Maven (If you don't have Maven, run `apt-get install maven`.)</span></span>
* <span data-ttu-id="ad13a-117">Předplatné Azure ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])</span><span class="sxs-lookup"><span data-stu-id="ad13a-117">An Azure subscription ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])</span></span>

<span data-ttu-id="ad13a-118">Informace o tom najdete v části tooset do clusteru Azure HDInsight Spark, [clusterů HDInsight zřizování](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="ad13a-118">For information about how tooset up an Azure HDInsight Spark cluster, see [Provisioning HDInsight clusters](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="create-an-azure-cosmos-db-database-account"></a><span data-ttu-id="ad13a-119">Vytvoření účtu Azure Cosmos DB databáze</span><span class="sxs-lookup"><span data-stu-id="ad13a-119">Create an Azure Cosmos DB database account</span></span>

<span data-ttu-id="ad13a-120">Nejprve vytvořte účet databáze s hello rozhraní Graph API pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="ad13a-120">First, create a database account with hello Graph API by doing hello following:</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="ad13a-121">Přidání kolekce</span><span class="sxs-lookup"><span data-stu-id="ad13a-121">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="get-apache-tinkerpop"></a><span data-ttu-id="ad13a-122">Získat Apache TinkerPop</span><span class="sxs-lookup"><span data-stu-id="ad13a-122">Get Apache TinkerPop</span></span>

<span data-ttu-id="ad13a-123">Apache TinkerPop získáte pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="ad13a-123">Get Apache TinkerPop by doing hello following:</span></span>

1. <span data-ttu-id="ad13a-124">Vzdálené toohello hlavního uzlu clusteru HDInsight hello `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-124">Remote toohello master node of hello HDInsight cluster `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.</span></span>

2. <span data-ttu-id="ad13a-125">Klonování hello TinkerPop3 zdrojový kód, ji vytvořit místně a nainstalujte ji tooMaven mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ad13a-125">Clone hello TinkerPop3 source code, build it locally, and install it tooMaven cache.</span></span>

    ```bash
    git clone https://github.com/apache/tinkerpop.git
    cd tinkerpop
    mvn clean install
    ```

3. <span data-ttu-id="ad13a-126">Instalace modulu plug-in Spark Gremlin hello</span><span class="sxs-lookup"><span data-stu-id="ad13a-126">Install hello Spark-Gremlin plug-in</span></span> 

    <span data-ttu-id="ad13a-127">a.</span><span class="sxs-lookup"><span data-stu-id="ad13a-127">a.</span></span> <span data-ttu-id="ad13a-128">Instalace modulu plug-in hello Hello se zpracovává souborem hroznového.</span><span class="sxs-lookup"><span data-stu-id="ad13a-128">hello installation of hello plug-in is handled by Grape.</span></span> <span data-ttu-id="ad13a-129">Naplnění hello úložiště informace hroznového tak může stahovat hello modulů plug-in a jeho závislosti.</span><span class="sxs-lookup"><span data-stu-id="ad13a-129">Populate hello repositories information for Grape so it can download hello plug-in and its dependencies.</span></span> 

      <span data-ttu-id="ad13a-130">Vytvořit hello hroznového konfigurační soubor, pokud není přítomen v `~/.groovy/grapeConfig.xml`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-130">Create hello grape configuration file if it's not present at `~/.groovy/grapeConfig.xml`.</span></span> <span data-ttu-id="ad13a-131">Použijte hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="ad13a-131">Use hello following settings:</span></span>

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

    <span data-ttu-id="ad13a-132">b.</span><span class="sxs-lookup"><span data-stu-id="ad13a-132">b.</span></span> <span data-ttu-id="ad13a-133">Spusťte konzolu Gremlin `bin/gremlin.sh`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-133">Start Gremlin console `bin/gremlin.sh`.</span></span>
        
    <span data-ttu-id="ad13a-134">c.</span><span class="sxs-lookup"><span data-stu-id="ad13a-134">c.</span></span> <span data-ttu-id="ad13a-135">Instalace modulu plug-in Spark Gremlin hello s verzí 3.3.0-SNAPSHOT, který jste vytvořili v předchozích krocích hello:</span><span class="sxs-lookup"><span data-stu-id="ad13a-135">Install hello Spark-Gremlin plug-in with version 3.3.0-SNAPSHOT, which you built in hello previous steps:</span></span>

    ```bash
    $ bin/gremlin.sh

            \,,,/
            (o o)
    -----oOOo-(3)-oOOo-----
    plugin activated: tinkerpop.server
    plugin activated: tinkerpop.utilities
    plugin activated: tinkerpop.tinkergraph
    gremlin> :install org.apache.tinkerpop spark-gremlin 3.3.0-SNAPSHOT
    ==>loaded: [org.apache.tinkerpop, spark-gremlin, 3.3.0-SNAPSHOT] - restart hello console toouse [tinkerpop.spark]
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

4. <span data-ttu-id="ad13a-136">Zkontrolujte, zda toosee `Hadoop-Gremlin` se aktivuje s `:plugin list`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-136">Check toosee whether `Hadoop-Gremlin` is activated with `:plugin list`.</span></span> <span data-ttu-id="ad13a-137">Tento modul plug-in, protože ho může narušovat hello Spark Gremlin modulu plug-in zakázat `:plugin unuse tinkerpop.hadoop`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-137">Disable this plug-in, because it could interfere with hello Spark-Gremlin plug-in `:plugin unuse tinkerpop.hadoop`.</span></span>

## <a name="prepare-tinkerpop3-dependencies"></a><span data-ttu-id="ad13a-138">Příprava TinkerPop3 závislosti</span><span class="sxs-lookup"><span data-stu-id="ad13a-138">Prepare TinkerPop3 dependencies</span></span>

<span data-ttu-id="ad13a-139">Po TinkerPop3 jste vytvořili v předchozím kroku hello, proces hello také vyžádány všechny závislosti jar pro Spark a Hadoop v hello cílový adresář.</span><span class="sxs-lookup"><span data-stu-id="ad13a-139">When you built TinkerPop3 in hello previous step, hello process also pulled all jar dependencies for Spark and Hadoop in hello target directory.</span></span> <span data-ttu-id="ad13a-140">Použijte hello JAR, které jsou předem nainstalované s HDI a vyžádá další závislosti pouze podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="ad13a-140">Use hello jars that are pre-installed with HDI, and pull in additional dependencies only as necessary.</span></span>

1. <span data-ttu-id="ad13a-141">Přejděte toohello Gremlin konzole cílové adresář v `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-141">Go toohello Gremlin Console target directory at `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`.</span></span> 

2. <span data-ttu-id="ad13a-142">Přesunout všechny JAR pod `ext/` příliš`lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-142">Move all jars under `ext/` too`lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.</span></span>

3. <span data-ttu-id="ad13a-143">Odeberte všechny jar knihovny pod `lib/` , jsou hello není v následujícím seznamu:</span><span class="sxs-lookup"><span data-stu-id="ad13a-143">Remove all jar libraries under `lib/` that are not in hello following list:</span></span>

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

## <a name="get-hello-azure-cosmos-db-spark-connector"></a><span data-ttu-id="ad13a-144">Získat konektor Azure Cosmos DB Spark hello</span><span class="sxs-lookup"><span data-stu-id="ad13a-144">Get hello Azure Cosmos DB Spark connector</span></span>

1. <span data-ttu-id="ad13a-145">Získat konektor Azure Cosmos DB Spark hello `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` a Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` z [konektoru služby Azure DB Spark Cosmos na Githubu](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).</span><span class="sxs-lookup"><span data-stu-id="ad13a-145">Get hello Azure Cosmos DB Spark connector `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` and Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` from [Azure Cosmos DB Spark Connector on GitHub](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).</span></span>

2. <span data-ttu-id="ad13a-146">Alternativně můžete vytvořit ho místně.</span><span class="sxs-lookup"><span data-stu-id="ad13a-146">Alternatively, you can build it locally.</span></span> <span data-ttu-id="ad13a-147">Protože hello nejnovější verzi Spark Gremlin byl sestaven s Spark 1.6.1 a není kompatibilní s Spark bodu 2.0.2, který je aktuálně používán hello Azure Cosmos DB Spark konektor, můžete sestavit hello nejnovější TinkerPop3 kód a hello JAR nainstalovat ručně.</span><span class="sxs-lookup"><span data-stu-id="ad13a-147">Because hello latest version of Spark-Gremlin was built with Spark 1.6.1 and is not compatible with Spark 2.0.2, which is currently used in hello Azure Cosmos DB Spark connector, you can build hello latest TinkerPop3 code and install hello jars manually.</span></span> <span data-ttu-id="ad13a-148">Hello následující:</span><span class="sxs-lookup"><span data-stu-id="ad13a-148">Do hello following:</span></span>

    <span data-ttu-id="ad13a-149">a.</span><span class="sxs-lookup"><span data-stu-id="ad13a-149">a.</span></span> <span data-ttu-id="ad13a-150">Klonování konektor Azure Cosmos DB Spark hello.</span><span class="sxs-lookup"><span data-stu-id="ad13a-150">Clone hello Azure Cosmos DB Spark connector.</span></span>

    <span data-ttu-id="ad13a-151">b.</span><span class="sxs-lookup"><span data-stu-id="ad13a-151">b.</span></span> <span data-ttu-id="ad13a-152">Sestavení TinkerPop3 (neudělali v předchozích krocích).</span><span class="sxs-lookup"><span data-stu-id="ad13a-152">Build TinkerPop3 (already done in previous steps).</span></span> <span data-ttu-id="ad13a-153">Nainstalujte všechny TinkerPop 3.3.0-SNAPSHOT JAR místně.</span><span class="sxs-lookup"><span data-stu-id="ad13a-153">Install all TinkerPop 3.3.0-SNAPSHOT jars locally.</span></span>

    ```bash
    mvn install:install-file -Dfile="gremlin-core-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-core -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar
    mvn install:install-file -Dfile="gremlin-groovy-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-groovy -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="gremlin-shaded-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-shaded -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="hadoop-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=hadoop-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="spark-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=spark-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="tinkergraph-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=tinkergraph-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    ```

    <span data-ttu-id="ad13a-154">c.</span><span class="sxs-lookup"><span data-stu-id="ad13a-154">c.</span></span> <span data-ttu-id="ad13a-155">Aktualizace `tinkerpop.version` `azure-documentdb-spark/pom.xml` příliš`3.3.0-SNAPSHOT`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-155">Update `tinkerpop.version` `azure-documentdb-spark/pom.xml` too`3.3.0-SNAPSHOT`.</span></span>
    
    <span data-ttu-id="ad13a-156">d.</span><span class="sxs-lookup"><span data-stu-id="ad13a-156">d.</span></span> <span data-ttu-id="ad13a-157">Sestavení s Maven.</span><span class="sxs-lookup"><span data-stu-id="ad13a-157">Build with Maven.</span></span> <span data-ttu-id="ad13a-158">Hello potřebné JAR jsou umístěny v `target` a `target/alternateLocation`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-158">hello needed jars are placed in `target` and `target/alternateLocation`.</span></span>

    ```bash
    git clone https://github.com/Azure/azure-cosmosdb-spark.git
    cd azure-documentdb-spark
    mvn clean package
    ```

3. <span data-ttu-id="ad13a-159">Kopírování hello už jsme zmínili JAR tooa místní adresář v ~ / azure-documentdb-spark:</span><span class="sxs-lookup"><span data-stu-id="ad13a-159">Copy hello previously mentioned jars tooa local directory at ~/azure-documentdb-spark:</span></span>

    ```bash
    $ azure-documentdb-spark:
    mkdir ~/azure-documentdb-spark
    cp target/azure-documentdb-spark-0.0.3-SNAPSHOT.jar ~/azure-documentdb-spark
    cp target/alternateLocation/azure-documentdb-1.10.0.jar ~/azure-documentdb-spark
    ```

## <a name="distribute-hello-dependencies-toohello-spark-worker-nodes"></a><span data-ttu-id="ad13a-160">Distribuovat hello závislosti toohello Spark pracovní uzly</span><span class="sxs-lookup"><span data-stu-id="ad13a-160">Distribute hello dependencies toohello Spark worker nodes</span></span> 

1. <span data-ttu-id="ad13a-161">Protože hello transformaci dat grafu. závisí na TinkerPop3, je nutné distribuovat hello související závislosti tooall Spark pracovním uzlům.</span><span class="sxs-lookup"><span data-stu-id="ad13a-161">Because hello transformation of graph data depends on TinkerPop3, you must distribute hello related dependencies tooall Spark worker nodes.</span></span>

2. <span data-ttu-id="ad13a-162">Kopírování hello výše Gremlin závislosti, hello jar konektor CosmosDB Spark a uzlů pracovního procesu toohello CosmosDB Java SDK pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="ad13a-162">Copy hello previously mentioned Gremlin dependencies, hello CosmosDB Spark connector jar, and CosmosDB Java SDK toohello worker nodes by doing hello following:</span></span>

    <span data-ttu-id="ad13a-163">a.</span><span class="sxs-lookup"><span data-stu-id="ad13a-163">a.</span></span> <span data-ttu-id="ad13a-164">Kopírování všech souborů JAR hello do `~/azure-documentdb-spark`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-164">Copy all hello jars into `~/azure-documentdb-spark`.</span></span>

    ```bash
    $ /home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone:
    cp lib/* ~/azure-documentdb-spark
    ```

    <span data-ttu-id="ad13a-165">b.</span><span class="sxs-lookup"><span data-stu-id="ad13a-165">b.</span></span> <span data-ttu-id="ad13a-166">Získání seznamu hello všechny uzly pracovního procesu Spark, které můžete najít na řídicím panelu Ambari v hello `Spark2 Clients` seznamu v hello `Spark2` části.</span><span class="sxs-lookup"><span data-stu-id="ad13a-166">Get hello list of all Spark worker nodes, which you can find on Ambari Dashboard, in hello `Spark2 Clients` list in hello `Spark2` section.</span></span>

    <span data-ttu-id="ad13a-167">c.</span><span class="sxs-lookup"><span data-stu-id="ad13a-167">c.</span></span> <span data-ttu-id="ad13a-168">Zkopírujte tento adresář tooeach hello uzlů.</span><span class="sxs-lookup"><span data-stu-id="ad13a-168">Copy that directory tooeach of hello nodes.</span></span>

    ```bash
    scp -r ~/azure-documentdb-spark sshuser@wn0-cosmos:/home/sshuser
    scp -r ~/azure-documentdb-spark sshuser@wn1-cosmos:/home/sshuser
    ...
    ```
    
## <a name="set-up-hello-environment-variables"></a><span data-ttu-id="ad13a-169">Nastavení proměnných prostředí hello</span><span class="sxs-lookup"><span data-stu-id="ad13a-169">Set up hello environment variables</span></span>

1. <span data-ttu-id="ad13a-170">Najděte hello HDP verzi clusteru Spark hello.</span><span class="sxs-lookup"><span data-stu-id="ad13a-170">Find hello HDP version of hello Spark cluster.</span></span> <span data-ttu-id="ad13a-171">Je název adresáře hello pod `/usr/hdp/` (například 2.5.4.2-7).</span><span class="sxs-lookup"><span data-stu-id="ad13a-171">It is hello directory name under `/usr/hdp/` (for example, 2.5.4.2-7).</span></span>

2. <span data-ttu-id="ad13a-172">Nastavte hdp.version pro všechny uzly.</span><span class="sxs-lookup"><span data-stu-id="ad13a-172">Set hdp.version for all nodes.</span></span> <span data-ttu-id="ad13a-173">Ambari řídicím panelu přejděte příliš**YARN části** > **konfigurací** > **Upřesnit**a pak hello následující:</span><span class="sxs-lookup"><span data-stu-id="ad13a-173">In Ambari Dashboard, go too**YARN section** > **Configs** > **Advanced**, and then do hello following:</span></span> 
 
    <span data-ttu-id="ad13a-174">a.</span><span class="sxs-lookup"><span data-stu-id="ad13a-174">a.</span></span> <span data-ttu-id="ad13a-175">V `Custom yarn-site`, přidejte novou vlastnost `hdp.version` s hodnotou hello verzí softwaru HDP hello na hlavní uzel hello.</span><span class="sxs-lookup"><span data-stu-id="ad13a-175">In `Custom yarn-site`, add a new property `hdp.version` with hello value of hello HDP version on hello master node.</span></span> 
     
    <span data-ttu-id="ad13a-176">b.</span><span class="sxs-lookup"><span data-stu-id="ad13a-176">b.</span></span> <span data-ttu-id="ad13a-177">Ukládání konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="ad13a-177">Save hello configurations.</span></span> <span data-ttu-id="ad13a-178">Existují upozornění, které můžete ignorovat.</span><span class="sxs-lookup"><span data-stu-id="ad13a-178">There are warnings, which you can ignore.</span></span> 
     
    <span data-ttu-id="ad13a-179">c.</span><span class="sxs-lookup"><span data-stu-id="ad13a-179">c.</span></span> <span data-ttu-id="ad13a-180">Restartujte služby YARN a Oozie hello jako ikony oznámení hello označují.</span><span class="sxs-lookup"><span data-stu-id="ad13a-180">Restart hello YARN and Oozie services as hello notification icons indicate.</span></span>

3. <span data-ttu-id="ad13a-181">Sada hello následující proměnné prostředí v hello hlavního uzlu (nahraďte hello hodnoty podle potřeby):</span><span class="sxs-lookup"><span data-stu-id="ad13a-181">Set hello following environment variables on hello master node (replace hello values as appropriate):</span></span>

    ```bash
    export HADOOP_GREMLIN_LIBS=/home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/ext/spark-gremlin/lib
    export CLASSPATH=$CLASSPATH:$HADOOP_CONF_DIR:/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    export HDP_VERSION=2.5.4.2-7
    export HADOOP_HOME=${HADOOP_HOME:-/usr/hdp/current/hadoop-client}
    ```

## <a name="prepare-hello-graph-configuration"></a><span data-ttu-id="ad13a-182">Příprava hello grafu konfigurace</span><span class="sxs-lookup"><span data-stu-id="ad13a-182">Prepare hello graph configuration</span></span>

1. <span data-ttu-id="ad13a-183">Vytvoření konfiguračního souboru s hello Azure Cosmos DB parametry připojení a nastavení z Spark a umístí jej na `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-183">Create a configuration file with hello Azure Cosmos DB connection parameters and Spark settings, and put it at `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.</span></span>

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

    # Classpath for hello driver and executors
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

2. <span data-ttu-id="ad13a-184">Aktualizace hello `spark.driver.extraClassPath` a `spark.executor.extraClassPath` tooinclude hello adresáře souborů JAR hello distribuované v předchozím kroku hello se v tomto případě `/home/sshuser/azure-documentdb-spark/*`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-184">Update hello `spark.driver.extraClassPath` and `spark.executor.extraClassPath` tooinclude hello directory of hello jars that you distributed in hello previous step, in this case `/home/sshuser/azure-documentdb-spark/*`.</span></span>

3. <span data-ttu-id="ad13a-185">Zadejte následující podrobnosti pro Azure Cosmos DB hello:</span><span class="sxs-lookup"><span data-stu-id="ad13a-185">Provide hello following details for Azure Cosmos DB:</span></span>

    ```
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    # Optional
    #spark.documentdb.preferredRegions=West\ US;West\ US\ 2
    ```
   
## <a name="load-hello-tinkerpop-graph-and-save-it-tooazure-cosmos-db"></a><span data-ttu-id="ad13a-186">Načíst hello TinkerPop grafu a uložit ho tooAzure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ad13a-186">Load hello TinkerPop graph, and save it tooAzure Cosmos DB</span></span>
<span data-ttu-id="ad13a-187">toodemonstrate jak toopersist graf do Azure Cosmos databáze, tento příklad používá hello TinkerPop předdefinované TinkerPop moderní grafu.</span><span class="sxs-lookup"><span data-stu-id="ad13a-187">toodemonstrate how toopersist a graph into Azure Cosmos DB, this example uses hello TinkerPop predefined TinkerPop modern graph.</span></span> <span data-ttu-id="ad13a-188">Graf Hello je uložený ve formátu Kryo a je součástí hello TinkerPop úložiště.</span><span class="sxs-lookup"><span data-stu-id="ad13a-188">hello graph is stored in Kryo format, and it's provided in hello TinkerPop repository.</span></span>

1. <span data-ttu-id="ad13a-189">Protože Gremlin je spuštěn v režimu YARN, je třeba zpřístupnit data grafu hello v hello systému souborů Hadoop.</span><span class="sxs-lookup"><span data-stu-id="ad13a-189">Because you are running Gremlin in YARN mode, you must make hello graph data available in hello Hadoop file system.</span></span> <span data-ttu-id="ad13a-190">Použití hello následující příkazy toomake adresáře a kopírovat hello grafu místní soubor do ní.</span><span class="sxs-lookup"><span data-stu-id="ad13a-190">Use hello following commands toomake a directory and copy hello local graph file into it.</span></span> 

    ```bash
    $ tinkerpop:
    hadoop fs -mkdir /graphData
    hadoop fs -copyFromLocal ~/tinkerpop/data/tinkerpop-modern.kryo /graphData/tinkerpop-modern.kryo
    ```

2. <span data-ttu-id="ad13a-191">Dočasně aktualizovat hello `gremlin-spark.properties` souboru toouse `GryoInputFormat` tooread hello grafu.</span><span class="sxs-lookup"><span data-stu-id="ad13a-191">Temporarily update hello `gremlin-spark.properties` file toouse `GryoInputFormat` tooread hello graph.</span></span> <span data-ttu-id="ad13a-192">Taky jednat `inputLocation` jako hello directory vytvoříte, stejně jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="ad13a-192">Also indicate `inputLocation` as hello directory you create, as in hello following:</span></span>

    ```
    gremlin.hadoop.graphReader=org.apache.tinkerpop.gremlin.hadoop.structure.io.gryo.GryoInputFormat
    gremlin.hadoop.inputLocation=/graphData/tinkerpop-modern.kryo
    ```

3. <span data-ttu-id="ad13a-193">Spusťte konzolu Gremlin a pak vytvořte hello následující výpočetní kroky toopersist data toohello nakonfigurované Azure Cosmos DB kolekci:</span><span class="sxs-lookup"><span data-stu-id="ad13a-193">Start Gremlin Console, and then create hello following computation steps toopersist data toohello configured Azure Cosmos DB collection:</span></span>  

    <span data-ttu-id="ad13a-194">a.</span><span class="sxs-lookup"><span data-stu-id="ad13a-194">a.</span></span> <span data-ttu-id="ad13a-195">Vytvoření grafu hello `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-195">Create hello graph `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.</span></span>

    <span data-ttu-id="ad13a-196">b.</span><span class="sxs-lookup"><span data-stu-id="ad13a-196">b.</span></span> <span data-ttu-id="ad13a-197">Použít SparkGraphComputer pro zápis `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-197">Use SparkGraphComputer for writing `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.</span></span>

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

4. <span data-ttu-id="ad13a-198">V Průzkumníku dat můžete ověřit, že tooAzure Cosmos DB jako trvalý, že hello data byla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="ad13a-198">From Data Explorer, you can verify that hello data has been persisted tooAzure Cosmos DB.</span></span>

## <a name="load-hello-graph-from-azure-cosmos-db-and-run-gremlin-queries"></a><span data-ttu-id="ad13a-199">Načíst hello grafu z databáze Cosmos Azure a spouštět dotazy Gremlin</span><span class="sxs-lookup"><span data-stu-id="ad13a-199">Load hello graph from Azure Cosmos DB, and run Gremlin queries</span></span>

1. <span data-ttu-id="ad13a-200">Graf hello tooload, upravit `gremlin-spark.properties` tooset `graphReader` příliš`DocumentDBInputRDD`:</span><span class="sxs-lookup"><span data-stu-id="ad13a-200">tooload hello graph, edit `gremlin-spark.properties` tooset `graphReader` too`DocumentDBInputRDD`:</span></span>

    ```
    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    ```

2. <span data-ttu-id="ad13a-201">Načíst hello grafu, procházení hello data a spouštět dotazy Gremlin ho pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="ad13a-201">Load hello graph, traverse hello data, and run Gremlin queries with it by doing hello following:</span></span>

    <span data-ttu-id="ad13a-202">a.</span><span class="sxs-lookup"><span data-stu-id="ad13a-202">a.</span></span> <span data-ttu-id="ad13a-203">Spustit hello Gremlin konzoly `bin/gremlin.sh`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-203">Start hello Gremlin Console `bin/gremlin.sh`.</span></span>

    <span data-ttu-id="ad13a-204">b.</span><span class="sxs-lookup"><span data-stu-id="ad13a-204">b.</span></span> <span data-ttu-id="ad13a-205">Vytvoření grafu hello s konfigurací hello `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-205">Create hello graph with hello configuration `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.</span></span>

    <span data-ttu-id="ad13a-206">c.</span><span class="sxs-lookup"><span data-stu-id="ad13a-206">c.</span></span> <span data-ttu-id="ad13a-207">Vytvoření grafu traversal s SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.</span><span class="sxs-lookup"><span data-stu-id="ad13a-207">Create a graph traversal with SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.</span></span>

    <span data-ttu-id="ad13a-208">d.</span><span class="sxs-lookup"><span data-stu-id="ad13a-208">d.</span></span> <span data-ttu-id="ad13a-209">Spusťte následující dotazy grafu Gremlin hello:</span><span class="sxs-lookup"><span data-stu-id="ad13a-209">Run hello following Gremlin graph queries:</span></span>

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
> <span data-ttu-id="ad13a-210">toosee podrobnější protokolování, nastavte úroveň protokolu hello `conf/log4j-console.properties` tooa podrobnější úrovni.</span><span class="sxs-lookup"><span data-stu-id="ad13a-210">toosee more detailed logging, set hello log level in `conf/log4j-console.properties` tooa more verbose level.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="ad13a-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ad13a-211">Next steps</span></span>

<span data-ttu-id="ad13a-212">V tomto článku úvodní když jste se naučili jak grafech toowork s kombinací Azure Cosmos DB a Spark.</span><span class="sxs-lookup"><span data-stu-id="ad13a-212">In this quick-start article, you've learned how toowork with graphs by combining Azure Cosmos DB and Spark.</span></span>

> [!div class="nextstepaction"]
