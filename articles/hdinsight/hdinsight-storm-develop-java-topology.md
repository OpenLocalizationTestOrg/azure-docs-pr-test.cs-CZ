---
title: "aaaApache Storm ukázkové topologie Java - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toocreate topologií Apache Storm v jazyce Java vytvořením příklad word počet topologie."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "Apache storm, například apache storm storm java, příklad topologie storm"
ms.assetid: a8838f29-9c08-4fd9-99ef-26655d1bf6d7
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 54fa9dc3c93ddad83ac861f3101f50f80117d804
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-apache-storm-topology-in-java"></a>Vytvoření topologie Apache Storm v jazyce Java

Zjistěte, jak toocreate topologii založené na jazyce Java pro Apache Storm. Můžete vytvořit topologie Storm, který implementuje počtu slov aplikace. Použijete projekt Maven toobuild a balíček hello. Potom se dozvíte, jak toodefine hello topologie pomocí hello tok framework.

> [!NOTE]
> Tok framework Hello je k dispozici v Storm 0.10.0 nebo vyšší. Storm 0.10.0 je k dispozici s HDInsight 3.3 a 3.4.

Po dokončení kroků hello v tomto dokumentu, můžete nasadit tooApache hello topologie Storm v HDInsight.

> [!NOTE]
> Dokončené verzi příkladů topologie Storm hello vytvořené v tomto dokumentu je k dispozici na [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).

## <a name="prerequisites"></a>Požadavky

* [Java Developer Kit (JDK) verze 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

* [Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven je systém sestavení projektu pro projekty Java.

* Textového editoru nebo IDE.

## <a name="configure-environment-variables"></a>Nakonfigurujte proměnné prostředí

Hello následující proměnné prostředí může být nastaven při instalaci Java a hello JDK. Ale byste měli zkontrolovat, že existují a že obsahují hello správné hodnoty pro váš systém.

* **JAVA_HOME** -by měla odkazovat toohello adresáře, kde je nainstalován hello prostředí Java runtime (JRE). Například v distribuci systému Unix nebo Linux, musí mít hodnotu podobnou příliš`/usr/lib/jvm/java-7-oracle`. Windows neměl by mít hodnotu podobnou příliš`c:\Program Files (x86)\Java\jre1.7`

* **CESTA** -musí obsahovat hello následující cesty:

  * **JAVA_HOME** (nebo ekvivalentní cesta hello)

  * **JAVA_HOME\bin** (nebo ekvivalentní cesta hello)

  * Hello adresáře, kde je nainstalován Maven

## <a name="create-a-maven-project"></a>Vytvořte projekt Maven

Z příkazového řádku hello, použijte následující hello příkaz toocreate projekt Maven s názvem **WordCount**:

```bash
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false
```

> [!NOTE]
> Pokud používáte prostředí PowerShell, je třeba vložit`-D` parametry s dvojitými uvozovkami.
>
> `mvn archetype:generate "-DarchetypeArtifactId=maven-archetype-quickstart" "-DgroupId=com.microsoft.example" "-DartifactId=WordCount" "-DinteractiveMode=false"`

Tento příkaz vytvoří adresář s názvem `WordCount` hello aktuálního umístění, která obsahuje základní projekt Maven. Hello `WordCount` adresář obsahuje hello následující položky:

* `pom.xml`: Obsahuje nastavení pro projekt Maven hello.
* `src\main\java\com\microsoft\example`: Obsahuje kód aplikace.
* `src\test\java\com\microsoft\example`: Obsahuje testy pro vaši aplikaci. 

### <a name="remove-hello-generated-example-code"></a>Odebrat hello generované ukázkový kód

Odstraňte testovací hello generované hello souborů a aplikací:

* **src\test\java\com\microsoft\example\AppTest.Java**
* **src\main\java\com\microsoft\example\App.Java**

## <a name="add-maven-repositories"></a>Přidání úložiště Maven

HDInsight je založena na hello Hortonworks Data Platform (HDP), takže vám doporučujeme používat hello Hortonworks úložiště toodownload závislosti pro projekty Apache Storm. V hello __pom.xml__ soubor, přidejte následující XML po hello hello `<url>http://maven.apache.org</url>` řádku:

```xml
<repositories>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPReleases</id>
        <name>HDP Releases</name>
        <url>http://repo.hortonworks.com/content/repositories/releases/</url>
        <layout>default</layout>
    </repository>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPJetty</id>
        <name>Hadoop Jetty</name>
        <url>http://repo.hortonworks.com/content/repositories/jetty-hadoop/</url>
        <layout>default</layout>
    </repository>
</repositories>
```

## <a name="add-properties"></a>Přidání vlastnosti

Maven vám umožní hodnoty úrovni projektu toodefine názvem vlastnosti. V hello __pom.xml__, přidejte následující text po hello hello `</repositories>` řádku:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--
    This is a version of Storm from hello Hortonworks repository that is compatible with HDInsight.
    -->
    <storm.version>1.0.1.2.5.3.0-37</storm.version>
</properties>
```

Teď můžete použít tuto hodnotu v dalších částech hello `pom.xml`. Například při zadávání hello verzi komponenty Storm, můžete použít `${storm.version}` místo pevné kódování hodnotu.

## <a name="add-dependencies"></a>Přidat závislosti

Přidáte závislost pro komponenty Storm. Otevřete hello `pom.xml` souboru a přidejte následující kód v hello hello `<dependencies>` části:

```xml
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-core</artifactId>
    <version>${storm.version}</version>
    <!-- keep storm out of hello jar-with-dependencies -->
    <scope>provided</scope>
</dependency>
```

Při kompilaci, Maven používá tato informace toolook `storm-core` v úložišti Maven hello. Vypadá to nejdřív v úložišti hello v místním počítači. Nejsou-li hello soubory existuje, Maven je stáhne z veřejného úložiště Maven hello a ukládá je v místním úložišti hello.

> [!NOTE]
> Všimněte si hello `<scope>provided</scope>` řádek v této části. Toto nastavení určuje Maven tooexclude **storm základní** ze všech JAR souborů, které jsou vytvořit, protože je poskytované hello systému.

## <a name="build-configuration"></a>Konfigurace sestavení

Moduly plug-in maven povolit toocustomize hello fáze sestavení projektu hello. Například jak kompilace projektu hello nebo jak toopackage do soubor JAR. Otevřete hello `pom.xml` souboru a přidejte následující kód přímo nad hello hello `</project>` řádku.

```xml
<build>
    <plugins>
    </plugins>
    <resources>
    </resources>
</build>
```

Tato část je použité tooadd moduly plug-in, prostředky a další možnosti konfigurace sestavení. Úplný přehled hello **pom.xml** souborů najdete v tématu [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).

### <a name="add-plug-ins"></a>Přidat moduly plug-in

Topologií Apache Storm implementována v jazyce Java, hello [modulu plug-in Maven Exec](http://www.mojohaus.org/exec-maven-plugin/) je užitečné, protože umožňuje tooeasily místní spuštění hello topologie ve vašem vývojovém prostředí. Přidejte následující toohello hello `<plugins>` části hello `pom.xml` souboru modulu plug-in Exec Maven tooinclude hello:

```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <version>1.4.0</version>
    <executions>
    <execution>
    <goals>
        <goal>exec</goal>
    </goals>
    </execution>
    </executions>
    <configuration>
    <executable>java</executable>
    <includeProjectDependencies>true</includeProjectDependencies>
    <includePluginDependencies>false</includePluginDependencies>
    <classpathScope>compile</classpathScope>
    <mainClass>${storm.topology}</mainClass>
    <cleanupDaemonThreads>false</cleanupDaemonThreads> 
    </configuration>
</plugin>
```

Další užitečné modul plug-in je hello [Apache Maven kompilátoru modul plug-in](http://maven.apache.org/plugins/maven-compiler-plugin/), což je použité toochange možnosti kompilace. změny Hello hello verzi Javy, který Maven používá pro hello zdroj a cíl pro vaši aplikaci.

* Pro HDInsight __3.4 nebo starším__, nastavení hello zdroje a cíle too__1.7__ verze Java.

* Pro HDInsight __3.5__, nastavení hello zdroje a cíle too__1.8__ verze Java.

Přidejte následující text v hello hello `<plugins>` části hello `pom.xml` souboru modulu plug-in Apache Maven kompilátoru tooinclude hello. Tento příklad určuje 1.8, tak, aby hello cílová HDInsight verze 3.5.

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.3</version>
    <configuration>
    <source>1.8</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

### <a name="configure-resources"></a>Konfigurace prostředků

Hello oddílu prostředků můžete prostředky bez kódu tooinclude například konfigurační soubory, které jsou potřebné součásti v topologii hello. V tomto příkladu přidejte následující text v hello hello `<resources>` části hello se soubor pom.xml.

```xml
<resource>
    <directory>${basedir}/resources</directory>
    <filtering>false</filtering>
    <includes>
        <include>log4j2.xml</include>
    </includes>
</resource>
```

Tento příklad přidá adresáře hello prostředků v kořenovém hello hello projektu (`${basedir}`) jako umístění, která obsahuje prostředky a zahrnuje hello soubor s názvem `log4j2.xml`. Tento soubor je použité tooconfigure, jaké je do něj protokolují informace podle topologie hello.

## <a name="create-hello-topology"></a>Vytvoření topologie hello

Topologie založené na jazyce Java Apache Storm se skládá z tři součásti, které musíte napsat (nebo reference) jako závislost.

* **Spouts**: načte data z externího zdroje a vysílá datových proudů do topologie hello.

* **Bolts**: provádí zpracování datových proudů vysílaných funkcích spouts nebo jiné funkce bolts a vysílá jeden nebo více datových proudů.

* **Topologie**: definuje, jak hello spouts a funkce bolts jsou uspořádány a poskytuje hello vstupní bod pro topologii s hello.

### <a name="create-hello-spout"></a>Vytvoření hello spout

tooreduce požadavky pro nastavení externích datových zdrojů, hello následující spout jednoduše vyšle náhodných věty. Je upravenou verzi spout, který je zadán v rámci hello [počáteční příklady Storm](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).

> [!NOTE]
> Příklad funkcí spout, který čte z externího zdroje dat najdete v jednom z hello následující příklady:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): Příklad funkcí spout, který čte ze služby Twitter.
> * [Storm Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): spout, který čte z Kafka

Pro hello spout, vytvořte soubor s názvem `RandomSentenceSpout.java` v hello `src\main\java\com\microsoft\example` hello adresáře a použijte následující kód v jazyce Java jako obsah hello:

```java
package com.microsoft.example;

import org.apache.storm.spout.SpoutOutputCollector;
import org.apache.storm.task.TopologyContext;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseRichSpout;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Values;
import org.apache.storm.utils.Utils;

import java.util.Map;
import java.util.Random;

//This spout randomly emits sentences
public class RandomSentenceSpout extends BaseRichSpout {
  //Collector used tooemit output
  SpoutOutputCollector _collector;
  //Used toogenerate a random number
  Random _rand;

  //Open is called when an instance of hello class is created
  @Override
  public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
  //Set hello instance collector toohello one passed in
    _collector = collector;
    //For randomness
    _rand = new Random();
  }

  //Emit data toohello stream
  @Override
  public void nextTuple() {
  //Sleep for a bit
    Utils.sleep(100);
    //hello sentences that are randomly emitted
    String[] sentences = new String[]{ "hello cow jumped over hello moon", "an apple a day keeps hello doctor away",
        "four score and seven years ago", "snow white and hello seven dwarfs", "i am at two with nature" };
    //Randomly pick a sentence
    String sentence = sentences[_rand.nextInt(sentences.length)];
    //Emit hello sentence
    _collector.emit(new Values(sentence));
  }

  //Ack is not implemented since this is a basic example
  @Override
  public void ack(Object id) {
  }

  //Fail is not implemented since this is a basic example
  @Override
  public void fail(Object id) {
  }

  //Declare hello output fields. In this case, an sentence
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("sentence"));
  }
}
```

> [!NOTE]
> I když tato topologie používá jenom jeden spout, ostatní může mít několik, které data datového kanálu z různých zdrojů do topologie hello.

### <a name="create-hello-bolts"></a>Vytvoření funkce bolts hello

Funkce Bolts zpracovávat zpracování dat hello. Tato topologie používá dvě funkce bolts:

* **SplitSentence**: rozdělí věty hello vysílaných **RandomSentenceSpout** do jednotlivých slov.

* **WordCount**: Spočítá počet opakování jednotlivých slov došlo k chybě.

> [!NOTE]
> Funkce Bolts dělat cokoliv, například výpočet, trvalost nebo rozhovoru tooexternal součásti.

Vytvořte dva nové soubory, `SplitSentence.java` a `WordCount.java` v hello `src\main\java\com\microsoft\example` adresáře. Použijte následující text jako hello obsahu pro soubory hello hello:

#### <a name="splitsentence"></a>SplitSentence

```java
package com.microsoft.example;

import java.text.BreakIterator;

import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class SplitSentence extends BaseBasicBolt {

  //Execute is called tooprocess tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //Get hello sentence content from hello tuple
    String sentence = tuple.getString(0);
    //An iterator tooget each word
    BreakIterator boundary=BreakIterator.getWordInstance();
    //Give hello iterator hello sentence
    boundary.setText(sentence);
    //Find hello beginning first word
    int start=boundary.first();
    //Iterate over each word and emit it toohello output stream
    for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
      //get hello word
      String word=sentence.substring(start,end);
      //If a word is whitespace characters, replace it with empty
      word=word.replaceAll("\\s+","");
      //if it's an actual word, emit it
      if (!word.equals("")) {
        collector.emit(new Values(word));
      }
    }
  }

  //Declare that emitted tuples contain a word field
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word"));
  }
}
```

#### <a name="wordcount"></a>WordCount

```java
package com.microsoft.example;

import java.util.HashMap;
import java.util.Map;
import java.util.Iterator;

import org.apache.storm.Constants;
import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;
import org.apache.storm.Config;

// For logging
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class WordCount extends BaseBasicBolt {
  //Create logger for this class
  private static final Logger logger = LogManager.getLogger(WordCount.class);
  //For holding words and counts
  Map<String, Integer> counts = new HashMap<String, Integer>();
  //How often tooemit a count of words
  private Integer emitFrequency;

  // Default constructor
  public WordCount() {
      emitFrequency=5; // Default too60 seconds
  }

  // Constructor that sets emit frequency
  public WordCount(Integer frequency) {
      emitFrequency=frequency;
  }

  //Configure frequency of tick tuples for this bolt
  //This delivers a 'tick' tuple on a specific interval,
  //which is used tootrigger certain actions
  @Override
  public Map<String, Object> getComponentConfiguration() {
      Config conf = new Config();
      conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
      return conf;
  }

  //execute is called tooprocess tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //If it's a tick tuple, emit all words and counts
    if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
            && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
      for(String word : counts.keySet()) {
        Integer count = counts.get(word);
        collector.emit(new Values(word, count));
        logger.info("Emitting a count of " + count + " for word " + word);
      }
    } else {
      //Get hello word contents from hello tuple
      String word = tuple.getString(0);
      //Have we counted any already?
      Integer count = counts.get(word);
      if (count == null)
        count = 0;
      //Increment hello count and store it
      count++;
      counts.put(word, count);
    }
  }

  //Declare that this emits a tuple containing two fields; word and count
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word", "count"));
  }
}
```

### <a name="define-hello-topology"></a>Definovat topologii hello

topologie Hello sváže funkcích spouts hello a bolts společně na graf, který definuje, jak se data proudí mezi součástmi hello. Nabízí taky paralelismus pomocné parametry, které Storm používá při vytváření instancí komponent hello v rámci clusteru hello.

Hello následující obrázek je základní diagram hello grafu součástí této topologii.

![Diagram znázorňující hello spouts a bolts uspořádání](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

tooimplement hello topologie, vytvořte soubor s názvem `WordCountTopology.java` v hello `src\main\java\com\microsoft\example` adresáře. Použijte následující kód v jazyce Java jako hello obsah souboru hello hello:

```java
package com.microsoft.example;

import org.apache.storm.Config;
import org.apache.storm.LocalCluster;
import org.apache.storm.StormSubmitter;
import org.apache.storm.topology.TopologyBuilder;
import org.apache.storm.tuple.Fields;

import com.microsoft.example.RandomSentenceSpout;

public class WordCountTopology {

  //Entry point for hello topology
  public static void main(String[] args) throws Exception {
  //Used toobuild hello topology
    TopologyBuilder builder = new TopologyBuilder();
    //Add hello spout, with a name of 'spout'
    //and parallelism hint of 5 executors
    builder.setSpout("spout", new RandomSentenceSpout(), 5);
    //Add hello SplitSentence bolt, with a name of 'split'
    //and parallelism hint of 8 executors
    //shufflegrouping subscribes toohello spout, and equally distributes
    //tuples (sentences) across instances of hello SplitSentence bolt
    builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
    //Add hello counter, with a name of 'count'
    //and parallelism hint of 12 executors
    //fieldsgrouping subscribes toohello split bolt, and
    //ensures that hello same word is sent toohello same instance (group by field 'word')
    builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

    //new configuration
    Config conf = new Config();
    //Set toofalse toodisable debug information when
    // running in production on a cluster
    conf.setDebug(false);

    //If there are arguments, we are running on a cluster
    if (args != null && args.length > 0) {
      //parallelism hint tooset hello number of workers
      conf.setNumWorkers(3);
      //submit hello topology
      StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
    }
    //Otherwise, we are running locally
    else {
      //Cap hello maximum number of executors that can be spawned
      //for a component too3
      conf.setMaxTaskParallelism(3);
      //LocalCluster is used toorun locally
      LocalCluster cluster = new LocalCluster();
      //submit hello topology
      cluster.submitTopology("word-count", conf, builder.createTopology());
      //sleep
      Thread.sleep(10000);
      //shut down hello cluster
      cluster.shutdown();
    }
  }
}
```

### <a name="configure-logging"></a>Konfigurace protokolování

Storm používá Apache Log4j toolog informace. Pokud neprovedete konfiguraci protokolování, hello topologie vysílá diagnostické informace. Co je protokolováno, toocontrol vytvořte soubor s názvem `log4j2.xml` v hello `resources` adresáře. Použijte následující XML jako hello obsah souboru hello hello.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
<Appenders>
    <Console name="STDOUT" target="SYSTEM_OUT">
        <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
</Appenders>
<Loggers>
    <Logger name="com.microsoft.example" level="trace" additivity="false">
        <AppenderRef ref="STDOUT"/>
    </Logger>
    <Root level="error">
        <Appender-Ref ref="STDOUT"/>
    </Root>
</Loggers>
</Configuration>
```

Tato konfigurace XML nakonfiguruje nové protokoly pro hello `com.microsoft.example` třída, která obsahuje součásti hello v této topologii příklad. úroveň Hello nastavena tootrace pro tohoto protokolovacího nástroje, které jsou zaznamenány žádné informace o protokolování vygenerované součásti v této topologii.

Hello `<Root level="error">` části nakonfiguruje hello kořenové úrovně protokolování (vše, co není ve `com.microsoft.example`) informace o chybě tooonly protokolu.

Další informace o konfiguraci protokolování pro Log4j najdete v tématu [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).

> [!NOTE]
> Storm verze 0.10.0 a vyšší využití Log4j 2.x. Starší verze storm použít Log4j 1.x, který používá jiný formát pro konfiguraci protokolu. Informace o konfiguraci starší hello, najdete v části [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).

## <a name="test-hello-topology-locally"></a>Test hello místně topologie

Po uložení hello soubory, použijte následující příkaz tootest hello topologie místně hello.

```bash
mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology
```

Při jeho spuštění, hello topologie zobrazí informace o spuštění. Hello následující text je příklad výstupu počet word hello:

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Tento příklad protokol Určuje, že word hello "a" má byla vygenerované 113 časy. Hello počet i nadále toogo nahoru, dokud hello topologie běží, protože hello spout nepřetržitě vysílá hello stejné věty.

Mezi emisí slova a počty je interval 5 sekund. Hello **WordCount** nakonfigurována součást tooonly generuje informace, pokud dorazí značek řazené kolekce členů. Požaduje této značky řazených kolekcí členů jsou pouze doručovány každých pět sekund.

## <a name="convert-hello-topology-tooflux"></a>Převést tooFlux topologie hello

Tok je nové rozhraní Storm 0.10.0 k dispozici a vyšší, což vám umožní tooseparate konfiguraci z implementace. Vaše komponenty jsou stále definována v jazyce Java, ale hello topologie je definována pomocí souboru YAML. Můžete balíček definice výchozí topologie s projektem nebo používat samostatný soubor při odesílání topologie hello. Při odesílání topologie tooStorm hello, můžete použít proměnné prostředí nebo konfigurační soubory toopopulate hodnoty v hello YAML topologie definice.

soubor YAML Hello definuje hello součásti toouse hello topologie a hello tok dat mezi nimi. Můžete zahrnout soubor YAML jako součást soubor jar hello nebo externí soubor YAML můžete použít.

Další informace o toku najdete v tématu [tok framework (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

> [!WARNING]
> Kvůli tooa [chyb (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) s Storm 1.0.1, může být nutné tooinstall [Storm vývojového prostředí](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) toorun tok místně topologie.

1. Přesunout hello `WordCountTopology.java` souboru mimo projekt hello. Tento soubor dříve, definované hello topologie, ale není potřeba s tokem.

2. V hello `resources` adresáře, vytvořte soubor s názvem `topology.yaml`. Použijte hello následující text jako hello obsah tohoto souboru.

        name: "wordcount"       # friendly name for hello topology
        
        config:                 # Topology configuration
        topology.workers: 1     # Hint for hello number of workers toocreate
        
        spouts:                 # Spout definitions
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            parallelism: 1      # parallelism hint
        
        bolts:                  # Bolt definitions
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1
         
        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
                - 10
            parallelism: 1
        
        streams:                # Stream definitions
            - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            from: "sentence-spout"       # hello stream emitter
            to: "splitter-bolt"          # hello stream consumer
            grouping:                    # Grouping type
                type: SHUFFLE
          
            - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
                args: ["word"]           # field(s) toogroup on

3. Ujistěte se, hello následující změny toohello `pom.xml` souboru.
   
   * Přidejte následující nové závislosti v hello hello `<dependencies>` části:
     
        ```xml
        <!-- Add a dependency on hello Flux framework -->
        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>flux-core</artifactId>
            <version>${storm.version}</version>
        </dependency>
        ```
   * Přidejte následující modul plug-in toohello hello `<plugins>` části. Tento modul plug-in zpracovává hello vytvoření balíčku (soubor jar) pro projekt hello a platí konkrétní tooFlux některé transformace při vytváření balíčku hello.
     
        ```xml
        <!-- build an uber jar -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
                <transformers>
                    <!-- Keep us from getting a "can't overwrite file error" -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                    <!-- We're using Flux, so refer tooit as main -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>org.apache.storm.flux.Flux</mainClass>
                    </transformer>
                </transformers>
                <!-- Keep us from getting a bad signature error -->
                <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                            <exclude>META-INF/*.SF</exclude>
                            <exclude>META-INF/*.DSA</exclude>
                            <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                </filters>
            </configuration>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
        ```

   * V hello **modulu plug-in exec maven** `<configuration>` změňte hodnotu hello `<mainClass>` příliš`org.apache.storm.flux.Flux`. Toto nastavení umožňuje tok toohandle hello topologie spuštěn místně v vývoj.

   * V hello `<resources>` přidejte následující toohello hello `<includes>`. Tato konfigurace XML obsahuje hello YAML soubor, který definuje hello topologie v rámci projektu hello.

        ```xml
        <include>topology.yaml</include>
        ```

## <a name="test-hello-flux-topology-locally"></a>Test hello tok topologie místně

1. Použijte následující toocompile hello a provést hello tok topologie pomocí nástroje Maven:

    ```bash
    mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    ```

    Pokud používáte prostředí PowerShell, použijte následující příkaz hello:

    ```bash
    mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"
    ```

    > [!WARNING]
    > Pokud vaše topologie používá Storm 1.0.1 bits, tento příkaz se nezdaří. Tato chyba je způsobená [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055). Místo toho [nainstalovat Storm ve vašem vývojovém prostředí](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) a hello použijte následující informace.

    Pokud máte [nainstalovaný ve vašem vývojovém prostředí Storm](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), můžete použít následující příkazy místo hello:

    ```bash
    mvn compile package
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml
    ```

    Hello `--local` parametr spustí hello topologie v místním režimu ve vývojovém prostředí. Hello `-R /topology.yaml` parametr používá hello `topology.yaml` souborů prostředků z hello jar souboru toodefine hello topologie.

    Při jeho spuštění, hello topologie zobrazí informace o spuštění. Následující text Hello je příklad výstupu hello:

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs

    Mezi listy protokolovaných informací je prodlevu 10 sekund.

2. Vytvořit kopii hello `topology.yaml` souboru z projektu hello. Název hello nový soubor `newtopology.yaml`. V hello `newtopology.yaml` souboru, najít hello následující části a změnit hodnotu hello `10` příliš`5`. Tato změna změny hello interval mezi generování dávky aplikace word počty z too5 10 sekund.

    ```yaml
    - id: "counter-bolt"
    className: "com.microsoft.example.WordCount"
    constructorArgs:
    - 5
    parallelism: 1
    ```yaml

3. toorun hello topology, use hello following command:

    ```bash
    mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"
    ```

    Nebo, pokud máte Storm na vašem vývojovém prostředí:

    ```bash
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml
    ```

    Změna hello `/path/to/newtopology.yaml` toohello cestě toohello newtopology.yaml souboru jste vytvořili v předchozím kroku hello. Tento příkaz používá hello newtopology.yaml jako definice topologie hello. Vzhledem k tomu, že jsme nezahrnuli hello `compile` parametr Maven používá verzi hello projektu hello vytvořené v předchozích krocích.

    Jednou hello topologie spustí, měli byste zaznamenat, že hello čas mezi emitovaného dávky se změnila hodnota hello tooreflect v newtopology.yaml. Abyste viděli, že můžete změnit konfiguraci prostřednictvím soubor YAML bez nutnosti toorecompile hello topologie.

Další informace o těchto a dalších funkcí hello tok framework najdete v tématu [tok (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

## <a name="trident"></a>Trident

Trident má vysokou úroveň abstrakce, která je poskytována Storm. Podporuje stavová zpracování. Primární výhodou Trident Hello je, že ho může zaručit, že každou zprávu, která vstupuje do topologie hello je zpracovány pouze jednou. Bez použití Trident, topologie pouze zaručit alespoň jednou zpracování zpráv. Existují také další rozdíly, jako je například integrované komponenty, které lze použít místo vytvoření funkce bolts. Funkce bolts jsou ve skutečnosti nahrazovány obecného méně součástí, např. filtry, projekce a funkcí.

Trident aplikace lze vytvořit pomocí projekty Maven. Použít hello basic stejné kroky uvedenou výše v tomto článku – pouze hello kód se liší. Trident také (aktuálně) nelze zadat s hello tok framework.

Další informace o Trident naleznete v tématu hello [přehled API Trident](http://storm.apache.org/documentation/Trident-API-Overview.html).

Příklad aplikace Trident naleznete v části [Twitter trendů témata s Apache Storm v HDInsight](hdinsight-storm-twitter-trending.md).

## <a name="next-steps"></a>Další kroky

Jste se naučili, jak toocreate topologie Storm pomocí Java. Teď další postup:

* [Nasazení a správa topologií Apache Storm v HDInsight](hdinsight-storm-deploy-monitor-topology.md)

* [Vývoj topologie C# pro Apache Storm v HDInsight pomocí sady Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Můžete najít další příklad topologií Storm navštivte stránky [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).

