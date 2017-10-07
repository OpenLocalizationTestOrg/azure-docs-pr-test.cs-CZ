---
title: "aaaTroubleshoot Storm pomocí Azure HDInsight | Microsoft Docs"
description: "Získejte odpovědi toocommon dotazy týkající se používání Apache Storm v prostředí Azure HDInsight."
keywords: "Azure HDInsight, Storm, – nejčastější dotazy, řešení potíží s průvodce, běžné problémy"
services: Azure HDInsight
documentationcenter: na
author: raviperi
manager: 
editor: 
ms.assetid: 74E51183-3EF4-4C67-AA60-6E12FAC999B5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: raviperi
ms.openlocfilehash: 51bcb3dc28eff5ee7bb33252fb2ec71a88ed8e09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-storm-by-using-azure-hdinsight"></a>Řešení potíží Storm pomocí Azure HDInsight

Další informace o hello nejčastější problémy a jejich řešení pro práci s Apache Storm datové části v Apache Ambari.

## <a name="how-do-i-access-hello-storm-ui-on-a-cluster"></a>Přístupu hello uživatelské rozhraní Storm v clusteru
Máte dvě možnosti pro přístup k hello uživatelské rozhraní Storm z prohlížeče:

### <a name="ambari-ui"></a>Uživatelské rozhraní Ambari
1. Přejděte toohello Ambari řídicího panelu.
2. V seznamu hello služeb vyberte **Storm**.
3. V hello **rychlé odkazy** nabídce vyberte možnost **uživatelské rozhraní Storm**.

### <a name="direct-link"></a>Přímý odkaz
Máte přístup hello uživatelské rozhraní Storm v hello následující adresu URL:

https://\<název DNS clusteru\>/stormui

Příklad:

 https://stormcluster.azurehdinsight.NET/stormui

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-tooanother"></a>Jak převést Storm událostí hub spout kontrolního bodu informace z jednoho topologie tooanother

Když budete vyvíjet topologie, které čtou z Azure Event Hubs pomocí hello souboru .jar spout centra událostí HDInsight Storm, je nutné nasadit topologii, která má stejný název na novém clusteru hello. Však musí zachovat data hello kontrolního bodu, která byla potvrzena tooApache ZooKeeper na původním clusteru hello.

### <a name="where-checkpoint-data-is-stored"></a>Uložení dat kontrolního bodu
Data kontrolního bodu pro odsazení se ukládají pomocí hello spout události rozbočovače v ZooKeeper v dvě kořenové cesty:
- Netransakční spout kontrolní body jsou uloženy v /eventhubspout.
- Data kontrolního bodu transakcí spout se ukládají vstupně -transakcí.

### <a name="how-toorestore"></a>Jak toorestore
tooget hello skripty a knihovny, můžete použít tooexport dat mimo ZooKeeper a následným importem hello back tooZooKeeper data s novým názvem, najdete v části [příklady Storm v HDInsight](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).

složku lib Hello má .jar soubory, které obsahují hello implementaci pro operace exportu/importu hello. Hello bash složka obsahuje ukázkový skript, který ukazuje, jak tooexport data z hello ZooKeeper server na původním clusteru hello a importujte ho back toohello ZooKeeper server na novém clusteru hello.

Spustit hello [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) skript z tooexport uzly ZooKeeper hello a pak importovat data. Aktualizace hello toohello správné Hortonworks Data Platform (HDP) verze skriptu. (Tvrdě pracujeme na tom Obecné v prostředí HDInsight tyto skripty. Obecné skripty můžete spustit z libovolného uzlu v clusteru hello bez úprav uživatelem hello.)

příkaz export Hello zapíše hello metadata tooan Apache Hadoop Distributed File System (HDFS) cesta (v úložišti Azure Blob Storage nebo Azure Data Lake Store) v umístění, které nastavíte.

### <a name="examples"></a>Příklady

#### <a name="export-offset-metadata"></a>Export metadat posunutí
1. Používání SSH toogo toohello ZooKeeper clusteru v clusteru hello z které hello kontrolního bodu musí posun toobe exportovali.
2. Hello spusťte následující příkaz (po aktualizaci hello řetězec verze softwaru HDP) tooexport ZooKeeper Posun dat toohello /stormmetadta/zkdata HDFS cesta:

    ```apache   
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a>Import metadat posunutí
1. Používání SSH toogo toohello ZooKeeper clusteru v clusteru hello z které hello kontrolního bodu musí posun toobe exportovali.
2. Spuštění hello následující příkaz (po aktualizaci řetězec verze softwaru HDP hello) tooimport ZooKeeper Posun dat z hello HDFS cesta/stormmetadata/zkdata toohello ZooKeeper serveru na cílový cluster hello:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```
   
#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-hello-beginning-or-from-a-timestamp-that-hello-user-chooses"></a>Odstranit posunutí metadata tak, aby topologie můžete spustit zpracování dat z počáteční hello nebo z časovým razítkem zvolí tento uživatel hello
1. Používání SSH toogo toohello ZooKeeper clusteru v clusteru hello z které hello kontrolního bodu musí posun toobe exportovali.
2. Spuštění hello následující příkaz (po aktualizaci řetězec verze softwaru HDP hello) toodelete všechny ZooKeeper Posun dat v aktuálním clusteru hello:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a>Jak najít binární soubory Storm v clusteru
Storm binární soubory pro aktuální zásobník HDP hello jsou /usr/hdp/current/storm-client. umístění Hello je hello stejné pro hlavních uzlech i pro uzly pracovního procesu.
 
Může existovat více binární soubory pro konkrétní verze softwaru HDP v /usr/hdp (například /usr/hdp/2.5.0.1233/storm). Složka /usr/hdp/current/storm-client Hello je toohello symlinked nejnovější se verzi, která běží na clusteru hello.

Další informace najdete v tématu [clusteru HDInsight tooan připojit pomocí protokolu SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) a [Storm](http://storm.apache.org/).
 
## <a name="how-do-i-determine-hello-deployment-topology-of-a-storm-cluster"></a>Jak zjistím hello topologie nasazení clusteru Storm
Nejdřív určete všechny součásti, které jsou nainstalované s HDInsight Storm. Storm cluster se skládá ze čtyř kategorií uzlu:

* Uzly brány
* hlavních uzlech
* Uzly zooKeeper
* Pracovní uzly
 
### <a name="gateway-nodes"></a>Uzly brány
Uzel brány je brány a služba reverzní proxy server, která umožňuje veřejný přístup tooan active Ambari management service. Také obstará Ambari vedoucí volba.
 
### <a name="head-nodes"></a>hlavních uzlech
Storm hlavních uzlech spusťte hello následující služby:
* Nimbus
* Ambari serveru
* Ambari metriky serveru
* Kolekce Ambari metriky
 
### <a name="zookeeper-nodes"></a>Uzly zooKeeper
HDInsight se dodává s třemi uzly ZooKeeper kvora. velikost kvora Hello je pevná a nejde ho překonfigurovat.
 
Služby Storm v clusteru hello jsou nakonfigurované tooautomatically použití hello ZooKeeper kvora.
 
### <a name="worker-nodes"></a>Pracovní uzly
Pracovní uzly Storm spusťte hello následující služby:
* Dohledový uzel
* Pracovní Java virtuálních počítačů (JVMs) pro spuštění topologie
* Ambari agenta
 
## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a>Jak najdu Storm binární soubory funkcí spout centra událostí pro vývoj
 
Další informace o pomocí topologie Storm event hub spout .jar soubory najdete v části hello následující prostředky.
 
### <a name="java-based-topology"></a>Topologie založené na jazyce Java
[Zpracování událostí z Azure Event Hubs se Storm v HDInsight (Java)](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-java-event-hub-topology)
 
### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a>C# – na základě topologie (Mono u clusterů Linux Storm HDInsight 3.4 +)
[Zpracování událostí z Azure Event Hubs se Stormem v HDInsight (C#)](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)
 
### <a name="latest-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a>Nejnovější Storm event hub spout binární soubory pro clustery Linux Storm HDInsight 3.5 +
toolearn způsobu toouse hello nejnovější Storm událostí hub spout který funguje s HDInsight 3.5 + clusterů Storm se Linux, najdete v hello mvn úložišti [souboru readme](https://github.com/hdinsight/mvn-repo/blob/master/README.md).
 
### <a name="source-code-examples"></a>Příklady zdrojového kódu
V tématu [příklady](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) jak tooread a zápis z centra událostí Azure pomocí topologií Apache Storm (napsanou v jazyce Java) v clusteru Azure HDInsight.
 
## <a name="how-do-i-locate-storm-log4j-configuration-files-on-clusters"></a>Jak najdu Storm Log4J konfigurační soubory v clusterech
 
tooidentify Apache Log4J konfigurační soubory pro služby Storm.
 
### <a name="on-head-nodes"></a>Na hlavních uzlech
Konfigurace Hello Nimbus Log4J je pro čtení z USR/hdp/\<verze softwaru HDP\>/storm/log4j2/cluster.xml.
 
### <a name="on-worker-nodes"></a>V pracovním uzlům
Hello nadřízeného Log4J konfigurace je pro čtení z USR/hdp/\<verze softwaru HDP\>/storm/log4j2/cluster.xml.
 
Hello pracovní Log4J konfigurační soubor je načten z USR/hdp/\<verze softwaru HDP\>/storm/log4j2/worker.xml.
 
Příklady: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml

