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
# <a name="azure-cosmos-db-perform-graph-analytics-by-using-spark-and-apache-tinkerpop-gremlin"></a>Azure Cosmos DB: Proveďte analýzy grafu pomocí Spark a Apache TinkerPop Gremlin

[Azure Cosmos DB](introduction.md) je hello globálně distribuované a více modelech databáze služby společnosti Microsoft. Můžete vytvořit a dotaz dokumentu, klíč/hodnota a databáze grafu, všechny z nich využívat hello globální distribuce a škálování vodorovných funkce jádrem hello Azure Cosmos DB. Azure Cosmos DB podporuje online zpracování úlohy graf (OLTP), které používají transakcí [Apache TinkerPop Gremlin](graph-introduction.md).

[Spark](http://spark.apache.org/) je projekt Apache Software Foundation, který se zaměřuje na zpracování dat pro obecné účely online analytického zpracování (OLAP). Spark poskytuje hybridní v paměti nebo disk – na základě distribuovaná výpočetní model, který je podobný toohello model Hadoop MapReduce. Apache Spark v cloudu hello můžete nasadit pomocí [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).

Kombinací Azure Cosmos DB a Spark můžete provádět úlohy OLTP a technologií OLAP při použití Gremlin. Tento úvodní článek ukazuje, jak toorun Gremlin dotazy pro Azure Cosmos DB v clusteru Azure HDInsight Spark.

## <a name="prerequisites"></a>Požadavky

Před spuštěním této ukázce, musíte mít hello následující požadavky:
* Cluster Azure HDInsight Spark 2.0
* JDK 1.8 + (Pokud nemáte JDK, spusťte `apt-get install default-jdk`.)
* Maven (Pokud nemáte Maven, spusťte `apt-get install maven`.)
* Předplatné Azure ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])

Informace o tom najdete v části tooset do clusteru Azure HDInsight Spark, [clusterů HDInsight zřizování](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

## <a name="create-an-azure-cosmos-db-database-account"></a>Vytvoření účtu Azure Cosmos DB databáze

Nejprve vytvořte účet databáze s hello rozhraní Graph API pomocí tohoto postupu hello následující:

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Přidání kolekce

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="get-apache-tinkerpop"></a>Získat Apache TinkerPop

Apache TinkerPop získáte pomocí tohoto postupu hello následující:

1. Vzdálené toohello hlavního uzlu clusteru HDInsight hello `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.

2. Klonování hello TinkerPop3 zdrojový kód, ji vytvořit místně a nainstalujte ji tooMaven mezipaměti.

    ```bash
    git clone https://github.com/apache/tinkerpop.git
    cd tinkerpop
    mvn clean install
    ```

3. Instalace modulu plug-in Spark Gremlin hello 

    a. Instalace modulu plug-in hello Hello se zpracovává souborem hroznového. Naplnění hello úložiště informace hroznového tak může stahovat hello modulů plug-in a jeho závislosti. 

      Vytvořit hello hroznového konfigurační soubor, pokud není přítomen v `~/.groovy/grapeConfig.xml`. Použijte hello následující nastavení:

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

    b. Spusťte konzolu Gremlin `bin/gremlin.sh`.
        
    c. Instalace modulu plug-in Spark Gremlin hello s verzí 3.3.0-SNAPSHOT, který jste vytvořili v předchozích krocích hello:

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

4. Zkontrolujte, zda toosee `Hadoop-Gremlin` se aktivuje s `:plugin list`. Tento modul plug-in, protože ho může narušovat hello Spark Gremlin modulu plug-in zakázat `:plugin unuse tinkerpop.hadoop`.

## <a name="prepare-tinkerpop3-dependencies"></a>Příprava TinkerPop3 závislosti

Po TinkerPop3 jste vytvořili v předchozím kroku hello, proces hello také vyžádány všechny závislosti jar pro Spark a Hadoop v hello cílový adresář. Použijte hello JAR, které jsou předem nainstalované s HDI a vyžádá další závislosti pouze podle potřeby.

1. Přejděte toohello Gremlin konzole cílové adresář v `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`. 

2. Přesunout všechny JAR pod `ext/` příliš`lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.

3. Odeberte všechny jar knihovny pod `lib/` , jsou hello není v následujícím seznamu:

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

## <a name="get-hello-azure-cosmos-db-spark-connector"></a>Získat konektor Azure Cosmos DB Spark hello

1. Získat konektor Azure Cosmos DB Spark hello `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` a Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` z [konektoru služby Azure DB Spark Cosmos na Githubu](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).

2. Alternativně můžete vytvořit ho místně. Protože hello nejnovější verzi Spark Gremlin byl sestaven s Spark 1.6.1 a není kompatibilní s Spark bodu 2.0.2, který je aktuálně používán hello Azure Cosmos DB Spark konektor, můžete sestavit hello nejnovější TinkerPop3 kód a hello JAR nainstalovat ručně. Hello následující:

    a. Klonování konektor Azure Cosmos DB Spark hello.

    b. Sestavení TinkerPop3 (neudělali v předchozích krocích). Nainstalujte všechny TinkerPop 3.3.0-SNAPSHOT JAR místně.

    ```bash
    mvn install:install-file -Dfile="gremlin-core-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-core -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar
    mvn install:install-file -Dfile="gremlin-groovy-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-groovy -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="gremlin-shaded-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-shaded -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="hadoop-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=hadoop-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="spark-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=spark-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="tinkergraph-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=tinkergraph-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    ```

    c. Aktualizace `tinkerpop.version` `azure-documentdb-spark/pom.xml` příliš`3.3.0-SNAPSHOT`.
    
    d. Sestavení s Maven. Hello potřebné JAR jsou umístěny v `target` a `target/alternateLocation`.

    ```bash
    git clone https://github.com/Azure/azure-cosmosdb-spark.git
    cd azure-documentdb-spark
    mvn clean package
    ```

3. Kopírování hello už jsme zmínili JAR tooa místní adresář v ~ / azure-documentdb-spark:

    ```bash
    $ azure-documentdb-spark:
    mkdir ~/azure-documentdb-spark
    cp target/azure-documentdb-spark-0.0.3-SNAPSHOT.jar ~/azure-documentdb-spark
    cp target/alternateLocation/azure-documentdb-1.10.0.jar ~/azure-documentdb-spark
    ```

## <a name="distribute-hello-dependencies-toohello-spark-worker-nodes"></a>Distribuovat hello závislosti toohello Spark pracovní uzly 

1. Protože hello transformaci dat grafu. závisí na TinkerPop3, je nutné distribuovat hello související závislosti tooall Spark pracovním uzlům.

2. Kopírování hello výše Gremlin závislosti, hello jar konektor CosmosDB Spark a uzlů pracovního procesu toohello CosmosDB Java SDK pomocí tohoto postupu hello následující:

    a. Kopírování všech souborů JAR hello do `~/azure-documentdb-spark`.

    ```bash
    $ /home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone:
    cp lib/* ~/azure-documentdb-spark
    ```

    b. Získání seznamu hello všechny uzly pracovního procesu Spark, které můžete najít na řídicím panelu Ambari v hello `Spark2 Clients` seznamu v hello `Spark2` části.

    c. Zkopírujte tento adresář tooeach hello uzlů.

    ```bash
    scp -r ~/azure-documentdb-spark sshuser@wn0-cosmos:/home/sshuser
    scp -r ~/azure-documentdb-spark sshuser@wn1-cosmos:/home/sshuser
    ...
    ```
    
## <a name="set-up-hello-environment-variables"></a>Nastavení proměnných prostředí hello

1. Najděte hello HDP verzi clusteru Spark hello. Je název adresáře hello pod `/usr/hdp/` (například 2.5.4.2-7).

2. Nastavte hdp.version pro všechny uzly. Ambari řídicím panelu přejděte příliš**YARN části** > **konfigurací** > **Upřesnit**a pak hello následující: 
 
    a. V `Custom yarn-site`, přidejte novou vlastnost `hdp.version` s hodnotou hello verzí softwaru HDP hello na hlavní uzel hello. 
     
    b. Ukládání konfigurace hello. Existují upozornění, které můžete ignorovat. 
     
    c. Restartujte služby YARN a Oozie hello jako ikony oznámení hello označují.

3. Sada hello následující proměnné prostředí v hello hlavního uzlu (nahraďte hello hodnoty podle potřeby):

    ```bash
    export HADOOP_GREMLIN_LIBS=/home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/ext/spark-gremlin/lib
    export CLASSPATH=$CLASSPATH:$HADOOP_CONF_DIR:/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    export HDP_VERSION=2.5.4.2-7
    export HADOOP_HOME=${HADOOP_HOME:-/usr/hdp/current/hadoop-client}
    ```

## <a name="prepare-hello-graph-configuration"></a>Příprava hello grafu konfigurace

1. Vytvoření konfiguračního souboru s hello Azure Cosmos DB parametry připojení a nastavení z Spark a umístí jej na `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.

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

2. Aktualizace hello `spark.driver.extraClassPath` a `spark.executor.extraClassPath` tooinclude hello adresáře souborů JAR hello distribuované v předchozím kroku hello se v tomto případě `/home/sshuser/azure-documentdb-spark/*`.

3. Zadejte následující podrobnosti pro Azure Cosmos DB hello:

    ```
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    # Optional
    #spark.documentdb.preferredRegions=West\ US;West\ US\ 2
    ```
   
## <a name="load-hello-tinkerpop-graph-and-save-it-tooazure-cosmos-db"></a>Načíst hello TinkerPop grafu a uložit ho tooAzure Cosmos DB
toodemonstrate jak toopersist graf do Azure Cosmos databáze, tento příklad používá hello TinkerPop předdefinované TinkerPop moderní grafu. Graf Hello je uložený ve formátu Kryo a je součástí hello TinkerPop úložiště.

1. Protože Gremlin je spuštěn v režimu YARN, je třeba zpřístupnit data grafu hello v hello systému souborů Hadoop. Použití hello následující příkazy toomake adresáře a kopírovat hello grafu místní soubor do ní. 

    ```bash
    $ tinkerpop:
    hadoop fs -mkdir /graphData
    hadoop fs -copyFromLocal ~/tinkerpop/data/tinkerpop-modern.kryo /graphData/tinkerpop-modern.kryo
    ```

2. Dočasně aktualizovat hello `gremlin-spark.properties` souboru toouse `GryoInputFormat` tooread hello grafu. Taky jednat `inputLocation` jako hello directory vytvoříte, stejně jako hello následující:

    ```
    gremlin.hadoop.graphReader=org.apache.tinkerpop.gremlin.hadoop.structure.io.gryo.GryoInputFormat
    gremlin.hadoop.inputLocation=/graphData/tinkerpop-modern.kryo
    ```

3. Spusťte konzolu Gremlin a pak vytvořte hello následující výpočetní kroky toopersist data toohello nakonfigurované Azure Cosmos DB kolekci:  

    a. Vytvoření grafu hello `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.

    b. Použít SparkGraphComputer pro zápis `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.

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

4. V Průzkumníku dat můžete ověřit, že tooAzure Cosmos DB jako trvalý, že hello data byla úspěšně.

## <a name="load-hello-graph-from-azure-cosmos-db-and-run-gremlin-queries"></a>Načíst hello grafu z databáze Cosmos Azure a spouštět dotazy Gremlin

1. Graf hello tooload, upravit `gremlin-spark.properties` tooset `graphReader` příliš`DocumentDBInputRDD`:

    ```
    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    ```

2. Načíst hello grafu, procházení hello data a spouštět dotazy Gremlin ho pomocí tohoto postupu hello následující:

    a. Spustit hello Gremlin konzoly `bin/gremlin.sh`.

    b. Vytvoření grafu hello s konfigurací hello `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.

    c. Vytvoření grafu traversal s SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.

    d. Spusťte následující dotazy grafu Gremlin hello:

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
> toosee podrobnější protokolování, nastavte úroveň protokolu hello `conf/log4j-console.properties` tooa podrobnější úrovni.
>

## <a name="next-steps"></a>Další kroky

V tomto článku úvodní když jste se naučili jak grafech toowork s kombinací Azure Cosmos DB a Spark.

> [!div class="nextstepaction"]
