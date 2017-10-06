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
# <a name="apache-spark-streaming-process-data-from-azure-event-hubs-with-spark-cluster-on-hdinsight"></a>Apache Spark streamování: clusteru zpracování dat z Azure Event Hubs s Spark v HDInsight

V tomto článku vytvořit Apache Spark streamování vzorku, který zahrnuje hello následující kroky:

1. Použijete samostatné aplikace tooingest zprávy do centra událostí Azure.

2. S dva různé přístupy můžete načítat zprávy hello z centra událostí v reálném čase pomocí aplikace běžící v clusteru Spark v Azure HDInsight.

3. Vytvoření datových proudů analytické kanály toopersist data toodifferent úložných systémů nebo získáte přehledy z dat v chodu hello.

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="spark-streaming-concepts"></a>Vysílání datového proudu Spark koncepty

Podrobné vysvětlení vysílání datového proudu Spark, najdete v článku [vysílání datového proudu přehled Apache Spark](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). HDInsight přináší hello clusteru stejné streamování tooa funkce Spark v Azure.  

## <a name="what-does-this-solution-do"></a>Jakým způsobem tohoto řešení?

V tomto článku, toocreate streamování příklad Spark, proveďte následující kroky hello:

1. Vytvoření centra událostí Azure, která bude přijímat události datového proudu.

2. Spustit místní samostatná aplikace, která generuje události a nabízených oznámení toohello centra událostí Azure. Hello ukázkovou aplikaci, která k tomu je publikována v [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).

3. Vzdálené spuštění aplikace na streamování cluster Spark, který čte streamování událostí z centra událostí Azure a provádět různé zpracování dat nebo analýzu.

## <a name="create-an-azure-event-hub"></a>Vytvoření centra událostí Azure

1. Přihlaste se toohello [portálu Azure](https://ms.portal.azure.com)a klikněte na tlačítko **nový** v hello levém horním rohu úvodní obrazovka.

2. Klikněte na **Internet věcí** a pak na **Event Hubs**.

    ![Centra událostí vytvořit pro příklad vysílání datového proudu Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "centra událostí vytvořit pro příklad vysílání datového proudu Spark")

3. V hello **vytvoření oboru názvů** okno, zadejte název oboru názvů. Zvolte hello cenová úroveň (Basic nebo Standard). Také v který toocreate hello prostředek vyberte příslušné předplatné Azure, skupinu prostředků a umístění. Klikněte na tlačítko **vytvořit** toocreate hello oboru názvů.

      ![Zadejte název centra událostí příklad vysílání datového proudu Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "zadejte název centra událostí příklad vysílání datového proudu Spark")

    > [!NOTE]
    > Jste měli vyberte hello stejné **umístění** jako cluster Apache Spark v HDInsight tooreduce latenci a náklady.
    >
    >

4. V seznamu názvů hello Event Hubs klikněte na tlačítko hello nově vytvořený obor názvů.      


5. V okně hello obor názvů, klikněte na tlačítko **Event Hubs**a potom klikněte na **+ centra událostí** toocreate nového centra událostí.
   
    ![Centra událostí vytvořit pro příklad vysílání datového proudu Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "centra událostí vytvořit pro příklad vysílání datového proudu Spark")

6. Zadejte název pro vaše Centrum událostí, sada hello oddílu počet too10 a too1 uchování zpráv. Hello zpráv v tomto řešení jsme nejsou archivace, můžete nechat hello rest jako výchozí a pak klikněte na tlačítko **vytvořit**.
   
    ![Zadejte podrobnosti o události rozbočovače pro příklad vysílání datového proudu Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "zadejte podrobnosti o události rozbočovače pro příklad vysílání datového proudu Spark")

7. Hello nově vytvořený Centrum událostí je uvedena v okně centra událostí hello.
    
     ![Zobrazit centra událostí hello Spark streamování třeba](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "Centrum událostí zobrazení hello Spark streamování příklad")

8. Zpět v okně obor názvů hello (ne hello konkrétní Centrum událostí okno), klikněte na tlačítko **zásady sdíleného přístupu**a potom klikněte na **RootManageSharedAccessKey**.
    
     ![Nastavení zásad centra událostí, například hello Spark streamování](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "Spark streamování příklad zásady nastavení centra událostí pro hello")

9. Klikněte na tlačítko hello kopie tlačítko toocopy hello **RootManageSharedAccessKey** primární klíč a připojovací řetězec schránky toohello. Uložte tyto toouse později v kurzu hello.
    
     ![Zobrazit centra událostí zásad klíče pro streamování příklad hello Spark](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "centra událostí zobrazení zásad klíče pro hello Spark streamování příklad")

## <a name="send-messages-tooazure-event-hub-using-a-sample-scala-application"></a>Odeslání zprávy tooAzure centra událostí pomocí Scala ukázkové aplikace

V této části použijte samostatná místní Scala aplikace, která generuje datového proudu událostí a odešle ji tooAzure centra událostí, které jste vytvořili dříve. Tato aplikace je k dispozici na webu GitHub na [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer). Zde Hello kroky předpokládají, jste již forked toto úložiště GitHub.

1. Ujistěte se, že máte následující hello nainstalovaná na počítači hello kde spuštění této aplikace.

    * Java Development kit Oracle. Můžete nainstalovat z [zde](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
    * Apache Maven. Si můžete stáhnout z [zde](https://maven.apache.org/download.cgi). Jsou k dispozici pokyny tooinstall Maven [zde](https://maven.apache.org/install.html).

2. Otevřete příkazový řádek a přejděte toohello umístění, které jste naklonovali úložiště GitHub hello hello ukázka Scala aplikace a spusťte následující příkaz toobuild hello aplikace hello.

        mvn package

3. jar výstup Hello aplikace hello **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, se vytvořil v rámci **/target** adresáře. Můžete použít tento JAR později v tomto kompletní řešení článku tootest hello.

## <a name="create-application-tooreceive-messages-from-event-hub-into-a-spark-cluster"></a>Vytvoření aplikace tooreceive zprávy z centra událostí do clusteru Spark 

Máme dva přístupy tooconnect vysílání datového proudu Spark a Azure Event Hubs, na základě příjemce připojení a připojení na základě přímo DStream. Na základě přímo DStream je uvedený na ledna 2017 ve verzi hello 2.0.3. Protože to je další původce by mělo tooreplace hello původní na základě příjemce připojení a efektivní prostředků. Další informace uvedené v [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs). Přímé DStream podporuje pouze Spark 2.0 +.

### <a name="build-applications-with-hello-dependency-toospark-eventhubs-connector"></a>Vytváření aplikací s konektorem toospark-eventhubs závislostí hello

Také jsme publikuje hello pracovní verzi Spark-EventHubs v Githubu. toouse hello pracovní verze Spark EventHubs, hello prvním krokem je tooindicate Githubu jako hello zdrojové úložiště přidáním následující položku toopom.xml hello:

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

Poté můžete přidat hello následující závislost tooyour projektu tootake hello předběžné verze.

Závislost maven

```xml
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11 -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-streaming-eventhubs_2.11</artifactId>
    <version>2.0.4</version>
</dependency>
```

SBT závislostí

```
// https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11
libraryDependencies += "com.microsoft.azure" % "spark-streaming-eventhubs_2.11" % "2.0.4"
```

### <a name="direct-dstream-connection"></a>Přímé připojení DStream

Jar předem připraveného souboru, který obsahuje příklady použití přímé DStream lze stáhnout v [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).

soubor jar Hello obsahuje tři příklady, jejichž zdrojového kódu jsou k dispozici na [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).

Pořízení [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) jako příklad:

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

V hello výše například `eventhubParameters` jsou hello parametry konkrétní tooa jediné EventHubs instance a jestli máte toopass ho toohello `createDirectStreams` rozhraní API, která vytvoří obor názvů tooa přímé DStream objekt mapování Event Hubs. Přes hello přímé DStream objekt můžete volat jakéhokoli DStream rozhraní API poskytované framework API vysílání datového proudu Spark. V tomto příkladu jsme vypočítat hello frekvenci jednotlivých slov v rámci hello poslední 3 malých batch intervalech.

### <a name="receiver-based-connection"></a>Na základě příjemce připojení

Spark, streamování ukázková aplikace napsané v jazyce Scala, které přijímá události a trasy hello toodifferent cíle, které jsou k dispozici v [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples). Postupujte podle kroků hello aplikace hello tooupdate pro konfiguraci centra událostí a vytvořte jar výstup hello.

1. Spusťte IntelliJ IDEA a z hello spusťte obrazovky vyberte **rezervovat z verzí** a pak klikněte na **Git**.
   
    ![Apache Spark streamování příklad - get zdroje z Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark streamování příklad - get zdroje z Git")

2. V hello **klonování úložiště** dialogové okno, zadejte hello URL toohello Git úložiště tooclone z, zadejte tooclone directory hello k a pak klikněte na tlačítko **klon**.
   
    ![Apache Spark streamování příklad - klonování z Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark streamování příklad - klonování z Git")
3. Postupujte podle pokynů hello, dokud je zcela klonovat hello projektu. Stiskněte klávesu **Alt + 1** tooopen hello **zobrazení projektu**. Měl by vypadat následující hello.
   
    ![Apache Spark streamování příklad: zobrazení projektu](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark streamování příklad: zobrazení projektu")
4. Ujistěte se, že je kód aplikace hello kompilovat s Java8. tooensure tento, klikněte na tlačítko **soubor**, klikněte na tlačítko **strukturu projektu**a na hello **projektu** , zkontrolujte, zda je příliš nastavit úroveň projektu jazyka**8 - Lambdas, typu poznámky atd**.
   
    ![Apache Spark streamování příklad - Set kompilátoru](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark streamování příklad - Set kompilátoru")
5. Otevřete hello **pom.xml** a ověřte správnost hello Spark verze. V části `<properties>` uzlu, vyhledejte hello následující fragment kódu a ověření hello Spark verze.

        <scala.version>2.11.8</scala.version>
        <scala.compat.version>2.11.8</scala.compat.version>
        <scala.binary.version>2.11</scala.binary.version>
        <spark.version>2.0.0</spark.version>

6. aplikace Hello vyžaduje závislostí jar, nazývá **jar ovladač JDBC**. Toto je požadovaná toowrite hello zprávy přijaté z centra událostí do Azure SQL database. Můžete si stáhnout tuto jar (v4.1 nebo novější) z [zde](https://msdn.microsoft.com/sqlserver/aa937724.aspx). Přidáte odkaz na toothis jar hello projektu knihovny. Proveďte hello následující kroky:
     
     1. Z okna IntelliJ IDEA, kdy máte aplikaci hello otevřít, klikněte na tlačítko **soubor**, klikněte na tlačítko **strukturu projektu**a potom klikněte na **knihovny**. 
     2. Klikněte na tlačítko hello přidat ikonu (![přidat ikonu](./media/hdinsight-apache-spark-eventhub-streaming/add-icon.png)), klikněte na tlačítko **Java**a potom přejděte toohello umístění, kam jste stáhli jar ovladač JDBC hello. Postupujte podle hello výzvy tooadd hello jar soubor toohello projektu knihovny.

         ![přidejte chybějící závislosti](./media/hdinsight-apache-spark-eventhub-streaming/add-missing-dependency-jars.png "přidejte chybějící závislost JAR")
     3. Klikněte na tlačítko **Použít**.

7. Vytvořte soubor jar výstup hello. Proveďte následující kroky hello.

   1. V hello **strukturu projektu** dialogové okno, klikněte na tlačítko **artefakty** a pak klikněte na symbol plus – hello. Z rozbalovací dialogové hello, klikněte na tlačítko **JAR**a potom klikněte na **z modulů se závislostmi**.      
       
       ![Apache Spark streamování příklad - vytvořit JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark streamování příklad - vytvořit JAR")
   2. V hello **vytvořit JAR z modulů** dialogové okno pole, klikněte na tlačítko se třemi tečkami hello (![třemi tečkami](./media/hdinsight-apache-spark-eventhub-streaming/ellipsis.png)) proti hello **hlavní třídy**.
   3. V hello **vyberte třídu hlavní** dialogové okno pole, vyberte některou z dostupných tříd hello a pak klikněte na tlačítko **OK**.
      
       ![Apache Spark streamování příklad - vyberte třídu pro jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark streamování příklad - vyberte třídu pro jar")
   4. V hello **vytvořit JAR z modulů** dialogové okno zkontrolujte, zda tuto možnost hello příliš**extrahovat toohello cíl JAR** je vybrána a potom klikněte na **OK**. Tím se vytvoří jeden JAR s všechny závislosti.
      
       ![Apache Spark streamování příklad - vytvořit jar z modulů](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark streamování příklad - vytvořit jar z modulů")
   5. Hello **výstup rozložení** karta Vypíše seznam všech hello JAR, které jsou součástí projekt Maven hello. Můžete vybrat a odstranění hello těch, které jsou na kterém hello Scala aplikace nemá žádné přímé závislostí. Pro aplikace hello tady vytváříme, můžete odebrat všechny ale hello naposledy (**spark streamování data trvalost příklady zkompilovat výstup**). Vyberte hello JAR toodelete a pak klikněte na hello **odstranit** ikona (![odstranit ikonu](./media/hdinsight-apache-spark-eventhub-streaming/delete-icon.png)).
      
       ![Apache Spark streamování Příklad - odstranění extrahovat JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark streamování Příklad - odstranění extrahovat JAR")
      
       Zajistěte, aby **sestavení na Ujistěte se,** je políčko zaškrtnuto, což zajistí, že jar hello se vytvoří pokaždé, když je vytvořené nebo aktualizované hello projektu. Klikněte na tlačítko **Použít**.
   6. V hello **výstup rozložení** kartě vpravo dole hello hello **dostupné elementy** pole, máte hello SQL JDBC jar, zda jste přidali starší toohello projektu knihovny. Je třeba přidat tento toohello **výstup rozložení** kartě. Klikněte pravým tlačítkem na soubor jar hello a pak klikněte na **extrahovat do kořenové výstup**.
      
       ![Apache Spark streamování příklad - extract závislostí jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark streamování příklad - jar závislostí extrakce")  
      
       Hello **výstup rozložení** karta by teď měl vypadat takto.
      
       ![Apache Spark streamování příklad - karta finální výstup](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark streamování příklad - karta finální výstup")        
      
       V hello **strukturu projektu** dialogové okno, klikněte na tlačítko **použít** a pak klikněte na **OK**.    
   7. V řádku nabídek hello, klikněte na **sestavení**a potom klikněte na **zkontrolujte projektu**. Můžete také kliknout na **sestavení artefaktů** toocreate hello jar. Hello jar výstup se vytvořil v rámci **\classes\artifacts**.
      
       ![Apache Spark streamování příklad – výstupní JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "streamování příklad Apache Spark – výstupní JAR")

## <a name="run-hello-application-remotely-on-a-spark-cluster-using-livy"></a>Spuštění aplikace hello vzdáleně na clusteru Spark pomocí Livy

V tomto článku jste Livy toorun hello Apache Spark streamování aplikaci používat vzdáleně na clusteru Spark. Podrobné informace o tom, jak toouse Livy s HDInsight Spark clusteru, najdete v části [odeslání úlohy vzdáleně tooan Apache Spark clusteru v Azure HDInsight](hdinsight-apache-spark-livy-rest-interface.md). Před zahájením spuštěna hello Spark streamování aplikace, existuje několik věcí, které byste měli udělat:

1. Spouštění hello místní samostatné aplikace toogenerate událostí a odesílat tooEvent rozbočovače. Použijte následující příkaz toodo tak hello:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "mysbnamespace" --eventhubs-name "myeventhub" --policy-name "mysendpolicy" --policy-key "<policy key>" --message-length 32 --thread-count 32 --message-count -1

2. Kopírování hello streamování jar (**spark streamování data trvalost examples.jar**) toohello úložiště objektů Blob Azure přidruženého k hello clusteru. Díky tomu přístupné tooLivy jar hello. Můžete použít [ **AzCopy**](../storage/common/storage-use-azcopy.md), příkaz řádku nástroje pro toodo tak. Existuje mnoho dalších klientů můžete použít tooupload data. Můžete najít další informace o nich v [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).
3. Nainstalujte na hello počítače, kde běží tyto aplikace z CURL. Používáme hello tooinvoke CURL Livy koncové body toorun hello úlohy vzdáleně.

### <a name="run-hello-spark-streaming-application-tooreceive-hello-events-into-an-azure-storage-blob-as-text"></a>Spustit hello Spark streamování aplikací tooreceive hello události do objektu Blob úložiště Azure jako text

Otevřete příkazový řádek, přejděte toohello adresáře, kam jste nainstalovali CURL a spusťte následující příkaz (nahraďte název uživatelské jméno a heslo a cluster) hello:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputBlob.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Hello parametry v souboru hello **inputBlob.txt** jsou definovány takto:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsEventCount", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Dejte nám vědět, jaké jsou parametry hello ve vstupním souboru hello:

* **soubor** je soubor jar hello cesta toohello aplikace v účtu úložiště Azure hello přidruženého k hello clusteru.
* **Název třídy** je hello název třídy hello v hello jar.
* **argumentů** hello seznam argumentů potřebných třídou hello
* **numExecutors** je hello počet jader používané hello toorun Spark streamování aplikace. To by měl být vždy alespoň dvakrát hello počet oddílů centra událostí.
* **executorMemory**, **executorCores**, **driverMemory** jsou použité parametry tooassign požadované prostředky toohello streamování aplikace.

> [!NOTE]
> Není nutné toocreate hello výstupní složky (EventCheckpoint, EventCount/EventCount10) používané jako parametry. Hello streamování aplikací je pro vás vytvoří.
>
>

Když spustíte příkaz hello, byste měli vidět výstup jako hello následující:

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

Poznamenejte si ID dávky hello v hello poslední řádek výstupu hello (v tomto příkladu je '1'). tooverify, který hello aplikace úspěšně proběhne, můžete se podívat na vašem účtu úložiště Azure přidruženého k hello clusteru a měli byste vidět hello **/EventCount/EventCount10** složku vytvořit existuje. Tato složka by měla obsahovat objekty BLOB, které jsou zaznamenány hello počet událostí zpracovat v rámci hello časové období zadaná pro parametr hello **batch interval v sekundách**.

Hello Spark streamování aplikace bude dále toorun, dokud jste ho ukončit. toodo tedy použijte hello následující příkaz:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/1"

### <a name="run-hello-applications-tooreceive-hello-events-into-an-azure-storage-blob-as-json"></a>Spuštění aplikace hello tooreceive hello události do objektu Blob úložiště Azure jako JSON
Otevřete příkazový řádek, přejděte toohello adresáře, kam jste nainstalovali CURL a spusťte následující příkaz (nahraďte název uživatelské jméno a heslo a cluster) hello:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputJSON.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Hello parametry v souboru hello **inputJSON.txt** jsou definovány takto:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureBlobAsJSON", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-store-folder", "/EventStore10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Hello parametry jsou podobné toowhat, které jste zadali pro hello textový výstup hello předchozího kroku. Znovu není nutné toocreate hello výstupní složky (EventCheckpoint, EventCount/EventCount10) používané jako parametry. Hello streamování aplikací je pro vás vytvoří.

 Po spuštění příkazu hello, můžete se podívat na vašem účtu úložiště Azure přidruženého k hello clusteru a měli byste vidět hello **/EventStore10** složku vytvořit existuje. Otevřít libovolný soubor s předponou **část -** a měli byste vidět hello událostí zpracovaných ve formátu JSON.

### <a name="run-hello-applications-tooreceive-hello-events-into-a-hive-table"></a>Spouštění aplikací hello tooreceive hello události do tabulky Hive
toorun hello streamování aplikací Spark datových proudů událostí do podregistru tabulky budete potřebovat některé další součásti. Jsou to:

* datanucleus rozhraní api jdo 3.2.6.jar
* datanucleus. rdbms 3.2.9.jar
* datanucleus – základní – 3.2.10.jar
* Hive-site.xml

Hello **.jar** soubory jsou k dispozici v clusteru HDInsight Spark na `/usr/hdp/current/spark-client/lib`. Hello **hive-site.xml** je k dispozici na `/usr/hdp/current/spark-client/conf`.

Můžete použít [WinScp](http://winscp.net/eng/download.php) toocopy tyto soubory z místního počítače tooyour hello clusteru. Pak můžete použít nástroje pro toocopy těchto souborů přes účet úložiště tooyour přidruženého k hello clusteru. Další informace o tom, jak tooupload soubory toohello účtu úložiště najdete v tématu [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).

Po zkopírování přes hello soubory tooyour účtu úložiště Azure, otevřete příkazový řádek, přejděte toohello adresáře, kam jste nainstalovali CURL a spusťte následující příkaz (nahraďte název uživatelské jméno a heslo a cluster) hello:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputHive.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Hello parametry v souboru hello **inputHive.txt** jsou definovány takto:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToHiveTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-hive-table", "EventHiveTable10" ], "jars":["wasb:///example/jars/datanucleus-api-jdo-3.2.6.jar", "wasb:///example/jars/datanucleus-rdbms-3.2.9.jar", "wasb:///example/jars/datanucleus-core-3.2.10.jar"], "files":["wasb:///example/jars/hive-site.xml"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Hello parametry jsou podobné toowhat, které jste zadali pro textový výstup hello, v předchozích krocích hello. Znovu, není nutné toocreate hello výstupní složky (EventCheckpoint, EventCount/EventCount10) nebo hello výstupní tabulku Hive (EventHiveTable10), které se používají jako parametry. Hello streamování aplikací je pro vás vytvoří. Všimněte si, že hello **JAR** a **soubory** možnost obsahuje soubory .jar toohello cesty a hello hive-site.xml, který jste zkopírovali přes toohello účet úložiště.

byl úspěšně vytvořen tooverify, který hello tabulku hive, můžete SSH do clusteru hello a spouštění dotazů Hive. Pokyny najdete v tématu [používání Hive s Hadoop v HDInsight pomocí protokolu SSH](hdinsight-hadoop-use-hive-ssh.md). Po připojení pomocí protokolu SSH, můžete spustit následující příkaz tooverify hello tuto tabulku Hive hello, **EventHiveTable10**, je vytvořena.

    show tables;

Měli byste vidět výstup podobný toohello následující:

    OK
    eventhivetable10
    hivesampletable

Můžete také spustit dotaz SELECT tooview hello obsah hello tabulky.

    SELECT * FROM eventhivetable10 LIMIT 10;

Měli byste vidět výstup jako hello následující:

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


### <a name="run-hello-applications-tooreceive-hello-events-into-an-azure-sql-database-table"></a>Spouštění aplikací hello tooreceive hello události do tabulky databáze Azure SQL
Před spuštěním tohoto kroku, ujistěte se, že máte k Azure SQL database vytvořit. Pokyny najdete v tématu [vytvoření databáze SQL v minutách](../sql-database/sql-database-get-started.md). toocomplete v této části, musíte hodnoty pro název databáze, název databázového serveru a přihlašovací údaje správce databáze hello jako parametry. Přestože nepotřebujete toocreate hello databázové tabulky. který vytvoří Hello streamování aplikací Spark.

Otevřete příkazový řádek, přejděte toohello adresáře, kam jste nainstalovali CURL a spusťte následující příkaz hello:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputSQL.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Hello parametry v souboru hello **inputSQL.txt** jsou definovány takto:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureSQLTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--sql-server-fqdn", "<database-server-name>.database.windows.net", "--sql-database-name", "mysparkdatabase", "--database-username", "sparkdbadmin", "--database-password", "<put-password-here>", "--event-sql-table", "EventContent" ], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

tooverify, který hello aplikace spuštěna úspěšně, můžete připojit toohello Azure SQL database pomocí SQL Server Management Studio. Návod, jak toodo, který najdete v části [připojit tooSQL databáze s SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md). Až se připojené toohello databáze, můžete přejít toohello **EventContent** tabulky, který byl vytvořen hello streamování aplikace. Data hello tooget rychlé dotaz můžete spustit z tabulky hello. Spusťte následující dotaz hello:

    SELECT * FROM EventCount

Měli byste vidět výstup podobný toohello následující:

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


## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)
* [Návrh připojení na základě příjemce a přímé DStream](https://www.slideshare.net/NanZhu/seattle-sparkmeetup032317)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]: ../storage-create-storage-account/
