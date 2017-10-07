---
title: "aaaPorts používá služby Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Seznam portů používaných běžící Hadoop v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: dd14aed9-ec25-4bb3-a20c-e29562735a7d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: larryfr
ms.openlocfilehash: 0abc5c1c678aa79816e3e82a74538d2fb6db40ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="ports-used-by-hadoop-services-on-hdinsight"></a>Porty používané služby Hadoop v HDInsight

Tento dokument obsahuje seznam hello porty používané při Hadoop služby spuštěné v clusterech HDInsight se systémem Linux. Nabízí taky informace o portech používaných tooconnect toohello clusteru pomocí protokolu SSH.

## <a name="public-ports-vs-non-public-ports"></a>Veřejné porty oproti neveřejný porty

HDInsight se systémem Linux, které clusterů pouze vystavit tři porty veřejně na hello Internetu; 22, 23 a 443. Tyto porty jsou použité toosecurely přístup hello clusteru pomocí SSH a služby, které jsou zveřejněné přes hello zabezpečený protokol HTTPS.

Interně HDInsight je implementována pomocí několika virtuálních počítačích Azure (hello uzlů v clusteru hello) spuštěné v Azure Virtual Network. Z virtuální sítě hello, dostanete porty nejsou viditelné prostřednictvím hello Internetu. Například pokud připojíte tooone hello hlavních uzlech pomocí protokolu SSH, z hlavního uzlu hello pak přímo přístupné služby spuštěné na uzlech clusteru hello.

> [!IMPORTANT]
> Pokud nezadáte virtuální síť Azure jako možnost konfigurace pro HDInsight, jeden se vytvoří automaticky. Ale nemůže připojit k jiné počítače (například vývojovém počítači klienta nebo jiných virtuálních počítačích Azure) toothis virtuální sítě.


toojoin další počítače toohello virtuální sítě, musíte nejprve vytvořit virtuální síť hello a pak zadat ho při vytváření clusteru HDInsight. Další informace najdete v tématu [možnosti rozšíření HDInsight pomocí virtuální síť Azure](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Veřejné porty

Hello hello všechny uzly v clusteru služby HDInsight jsou umístěny ve virtuální síti Azure a nelze přistupovat přímo z Internetu. Veřejné brány poskytuje toohello přístup k Internetu následující porty, které jsou společné pro všechny typy clusteru HDInsight.

| Služba | Port | Protocol (Protokol) | Popis |
| --- | --- | --- | --- | --- |
| sshd |22 |SSH |Připojí toosshd klienty na primární headnode hello. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md). |
| sshd |22 |SSH |Připojí toosshd klientů na uzlu edge hello. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md). |
| sshd |23 |SSH |Připojí toosshd klientů na sekundární headnode hello. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md). |
| Ambari |443 |HTTPS |Webovému uživatelskému rozhraní Ambari. V tématu [spravovat HDInsight pomocí hello webové uživatelské rozhraní Ambari](hdinsight-hadoop-manage-ambari.md) |
| Ambari |443 |HTTPS |Ambari REST API. V tématu [spravovat HDInsight pomocí Ambari REST API hello](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat |443 |HTTPS |HCatalog REST API. V tématu [použijte Hive s Curl](hdinsight-hadoop-use-pig-curl.md), [použijte Pig s Curl](hdinsight-hadoop-use-pig-curl.md), [používání nástroje MapReduce s Curl](hdinsight-hadoop-use-mapreduce-curl.md) |
| HiveServer2 |443 |ODBC |Připojí tooHive pomocí rozhraní ODBC. V tématu [tooHDInsight připojení aplikace Excel pomocí ovladače Microsoft ODBC hello](hdinsight-connect-excel-hive-odbc-driver.md). |
| HiveServer2 |443 |JDBC |Připojí pomocí JDBC tooHive. V tématu [připojit tooHive v HDInsight pomocí ovladač Hive JDBC hello](hdinsight-connect-hive-jdbc-driver.md) |

Následující Hello jsou k dispozici pro konkrétní cluster typy:

| Služba | Port | Protocol (Protokol) | Typ clusteru | Popis |
| --- | --- | --- | --- | --- |
| Stargate |443 |HTTPS |HBase |HBase REST API. V tématu [Začínáme používat HBase](hdinsight-hbase-tutorial-get-started-linux.md) |
| Livy |443 |HTTPS |Spark |Rozhraní API REST Spark. V tématu [úlohy odeslání Spark vzdáleně pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md) |
| Storm |443 |HTTPS |Storm |Storm webového uživatelského rozhraní. V tématu [nasadit a spravovat topologie Storm v HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md) |

### <a name="authentication"></a>Authentication

Veřejně zveřejněné na hello internet musí být ověřeny všechny služby:

| Port | Přihlašovací údaje |
| --- | --- |
| 22 nebo 23 |SSH přihlašovací údaje uživatele Hello zadané během vytváření clusteru |
| 443 |přihlašovací jméno Hello (výchozí: správce) a heslo, které byly nastavené při vytváření clusteru |

## <a name="non-public-ports"></a>Není veřejné porty

> [!NOTE]
> Některé služby jsou dostupné jenom na konkrétní clusteru typy. Například HBase je k dispozici pouze na typy clusteru HBase.

> [!IMPORTANT]
> Některé služby spustit pouze v jedné headnode najednou. Pokud pokus tooconnect toohello službu na primární headnode hello a zobrazí chybu 404, zkuste použít sekundární headnode hello.

### <a name="ambari"></a>Ambari

| Služba | Uzly | Port | Cesta adresy URL | Protocol (Protokol) | 
| --- | --- | --- | --- | --- |
| Webovému uživatelskému rozhraní Ambari | hlavních uzlech | 8080 | / | HTTP |
| Ambari REST API | hlavních uzlech | 8080 | / api/v1 | HTTP |

Příklady:

* Ambari REST API:`curl -u admin "http://10.0.0.11:8080/api/v1/clusters"`

### <a name="hdfs-ports"></a>HDFS porty

| Služba | Uzly | Port | Protocol (Protokol) | Popis |
| --- | --- | --- | --- | --- |
| NameNode webového uživatelského rozhraní |hlavních uzlech |30070 |HTTPS |Stav tooview webového uživatelského rozhraní |
| Služba NameNode metadat |hlavních uzlech |8020 |IPC |Metadatech systému souborů |
| DataNode |Všechny uzly pracovního procesu |30075 |HTTPS |Stav tooview webového uživatelského rozhraní, protokoly, atd. |
| DataNode |Všechny uzly pracovního procesu |30010 |&nbsp; |Přenos dat |
| DataNode |Všechny uzly pracovního procesu |30020 |IPC |Operace s metadaty |
| Sekundární NameNode |hlavních uzlech |50090 |HTTP |Kontrolní bod pro NameNode metadata |

### <a name="yarn-ports"></a>YARN porty

| Služba | Uzly | Port | Protocol (Protokol) | Popis |
| --- | --- | --- | --- | --- |
| Správce prostředků webového uživatelského rozhraní |hlavních uzlech |8088 |HTTP |Webové uživatelské rozhraní pro Resource Manager |
| Správce prostředků webového uživatelského rozhraní |hlavních uzlech |8090 |HTTPS |Webové uživatelské rozhraní pro Resource Manager |
| Rozhraní správce prostředků správce |hlavních uzlech |8141 |IPC |Pro odesílání aplikace (Hive, Pig, Hive serveru atd.) |
| Správce prostředků plánovače |hlavních uzlech |8030 |HTTP |Rozhraní pro správu |
| Rozhraní správce prostředků aplikace |hlavních uzlech |8050 |HTTP |Adresa rozhraní správce aplikace hello |
| NodeManager |Všechny uzly pracovního procesu |30050 |&nbsp; |Hello adresu hello kontejneru manager |
| NodeManager webového uživatelského rozhraní |Všechny uzly pracovního procesu |30060 |HTTP |Rozhraní správce prostředků |
| Časová osa adresa |hlavních uzlech |10200 |RPC |Hello časová osa služby RPC. |
| Časová osa webového uživatelského rozhraní |hlavních uzlech |8181 |HTTP |Hello časová osa služby webového uživatelského rozhraní |

### <a name="hive-ports"></a>Porty Hive

| Služba | Uzly | Port | Protocol (Protokol) | Popis |
| --- | --- | --- | --- | --- |
| HiveServer2 |hlavních uzlech |10001 |Thrift |Služby pro připojení tooHive (Thrift/JDBC) |
| Metaúložiště Hive |hlavních uzlech |9083 |Thrift |Služba pro připojování tooHive metadata (Thrift/JDBC) |

### <a name="webhcat-ports"></a>Porty WebHCat

| Služba | Uzly | Port | Protocol (Protokol) | Popis |
| --- | --- | --- | --- | --- |
| WebHCat server |hlavních uzlech |30111 |HTTP |Webové rozhraní API nad HCatalog a další služby Hadoop |

### <a name="mapreduce-ports"></a>MapReduce porty

| Služba | Uzly | Port | Protocol (Protokol) | Popis |
| --- | --- | --- | --- | --- |
| JobHistory |hlavních uzlech |19888 |HTTP |MapReduce JobHistory webového uživatelského rozhraní |
| JobHistory |hlavních uzlech |10020 |&nbsp; |MapReduce JobHistory serveru |
| ShuffleHandler |&nbsp; |13562 |&nbsp; |Přenosy zprostředkující mapy výstupy toorequesting přechodky |

### <a name="oozie"></a>Oozie

| Služba | Uzly | Port | Protocol (Protokol) | Popis |
| --- | --- | --- | --- | --- |
| Oozie serveru |hlavních uzlech |11000 |HTTP |Adresa URL služby Oozie |
| Oozie serveru |hlavních uzlech |11001 |HTTP |Port pro Oozie správce |

### <a name="ambari-metrics"></a>Metriky Ambari

| Služba | Uzly | Port | Protocol (Protokol) | Popis |
| --- | --- | --- | --- | --- |
| Časová osa (historie aplikace) |hlavních uzlech |6188 |HTTP |Hello časová osa služby webového uživatelského rozhraní |
| Časová osa (historie aplikace) |hlavních uzlech |30200 |RPC |Hello časová osa služby webového uživatelského rozhraní |

### <a name="hbase-ports"></a>Porty HBase

| Služba | Uzly | Port | Protocol (Protokol) | Popis |
| --- | --- | --- | --- | --- |
| HMaster |hlavních uzlech |16000 |&nbsp; |&nbsp; |
| Informace o HMaster webového uživatelského rozhraní |hlavních uzlech |16010 |HTTP |Hello port pro hello HBase hlavního webového uživatelského rozhraní |
| Oblast serveru |Všechny uzly pracovního procesu |16020 |&nbsp; |&nbsp; |
| &nbsp; |&nbsp; |2181 |&nbsp; |Hello portu, které využívají klienti tooconnect tooZooKeeper |

### <a name="kafka-ports"></a>Kafka porty

| Služba | Uzly | Port | Protocol (Protokol) | Popis |
| --- | --- | --- | --- | --- |
| Zprostředkovatel |Pracovní uzly |9092 |[Kafka přenosový protokol](http://kafka.apache.org/protocol.html) |Použít pro komunikaci klienta |
| &nbsp; |Uzly zookeeper |2181 |&nbsp; |Hello portu, které využívají klienti tooconnect tooZookeeper |

### <a name="spark-ports"></a>Spark porty

| Služba | Uzly | Port | Protocol (Protokol) | Cesta adresy URL | Popis |
| --- | --- | --- | --- | --- | --- |
| Spark Thrift servery |hlavních uzlech |10002 |Thrift | &nbsp; | Služby pro připojení tooSpark SQL (Thrift/JDBC) |
| Livy server | hlavních uzlech | 8998 | HTTP | /batches | Služba pro příkazy, úlohy a aplikace |

Příklady:

* Livy: `curl "http://10.0.0.11:8998/batches"`. V tomto příkladu `10.0.0.11` je IP adresa hello hello headnode, který je hostitelem hello Livy služby.
