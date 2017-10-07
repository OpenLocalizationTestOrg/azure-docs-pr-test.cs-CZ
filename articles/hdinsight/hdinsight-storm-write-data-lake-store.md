---
title: aaaApache Storm zapsat tooStorage nebo Data Lake Store - Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak toouse hello Apache Storm toowrite toohello HDFS kompatibilní úložiště pro HDInsight. Azure Storage nebo Azure Data Lake Store zadejte hello HDFS comptabile úložiště pro HDInsight. Tento dokument a přidružené příkladu hello ukazují, jak součást HdfsBolt hello lze použít toowrite toohello výchozí úložiště cluster Storm v HDInsight."
services: hdinsight
documentationcenter: na
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 1df98653-a6c8-4662-a8c6-5d288fc4f3a6
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/19/2017
ms.author: larryfr
ms.openlocfilehash: d76159a9ecd1be18e519511cfdb3bcfd18ae4d33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="write-toohdfs-from-apache-storm-on-hdinsight"></a>Zápis tooHDFS z Apache Storm v HDInsight

Zjistěte, jak toouse Storm toowrite data toohello HDFS kompatibilní úložiště používané Apache Storm v HDInsight. HDInsight můžete je používat jako úložiště HDFS comptabile úložiště Azure Storage a Azure Data Lake. Storm poskytuje [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) komponenta, která zapisuje data tooHDFS. Tento dokument obsahuje informace o zápis z hello HdfsBolt tooeither typu úložiště. 

> [!IMPORTANT]
> Příklad Hello topologie použitý v tomto dokumentu využívá součásti, které jsou součástí Storm v HDInsight. Změna toowork s Azure Data Lake Store při použití s další clustery Apache Storm může požadovat.

## <a name="get-hello-code"></a>Získat kód hello

je k dispozici ke stažení z projektu Hello obsahující tato topologie [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).

toocompile tento projekt, musíte hello následující konfigurace pro vývojové prostředí:

* [Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) nebo vyšší. HDInsight 3.5 nebo vyšší vyžadují Java 8.

* [Maven 3.x](https://maven.apache.org/download.cgi)

Hello následující proměnné prostředí může být nastaven při instalaci Java a hello JDK na pracovní stanici. Ale byste měli zkontrolovat, že existují a že obsahují hello správné hodnoty pro váš systém.

* `JAVA_HOME`-by měla odkazovat toohello adresář, kam hello JDK je nainstalovat.
* `PATH`-musí obsahovat hello následující cesty:
  
    * `JAVA_HOME`(nebo ekvivalentní cesta hello).
    * `JAVA_HOME\bin`(nebo ekvivalentní cesta hello).
    * Hello adresář, kde je nainstalován Maven.

## <a name="how-toouse-hello-hdfsbolt-with-hdinsight"></a>Jak toouse hello HdfsBolt s HDInsight

> [!IMPORTANT]
> Před použitím hello HdfsBolt se Storm v HDInsight, musíte nejprve použít souborů jar toocopy požadované akce skriptu do hello `extpath` pro Storm. Další informace najdete v tématu hello [konfigurovat hello clusteru](#configure) části.

Hello HdfsBolt používá schéma souboru hello jak poskytnout toounderstand toowrite tooHDFS. S HDInsight použijte jednu z následujících schémat hello:

* `wasb://`: Používá se účtu úložiště Azure.
* `adl://`: Použít s Azure Data Lake Store.

Hello následující tabulka obsahuje příklady použití hello souboru schématu pro různé scénáře:

| Schéma | Poznámky |
| ----- | ----- |
| `wasb:///` | Hello výchozí účet úložiště je kontejner objektů blob v účtu Azure Storage |
| `adl:///` | Hello výchozí účet úložiště je adresář v Azure Data Lake Store. Při vytváření clusteru zadejte hello adresář v Data Lake Store, který je hello kořenovém clusteru hello HDFS. Například hello `/clusters/myclustername/` adresáře. |
| `wasb://CONTAINER@ACCOUNT.blob.core.windows.net/` | Účet úložiště Azure (Další) jiné než výchozí přidruženého k hello clusteru. |
| `adl://STORENAME/` | kořenový adresář Hello hello používá hello cluster Data Lake Store. Toto schéma umožňuje tooaccess data, která se nachází mimo hello adresář, který obsahuje systém souborů clusteru hello. |

Další informace najdete v tématu hello [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) odkaz na Apache.org.

### <a name="example-configuration"></a>Příklad konfigurace

Hello následující YAML je výňatek ze hello `resources/writetohdfs.yaml` zahrnutý v příkladu hello. Tento soubor definuje topologie Storm hello pomocí hello [tok](https://storm.apache.org/releases/1.1.0/flux.html) framework pro Apache Storm.

```yaml
components:
  - id: "syncPolicy"
    className: "org.apache.storm.hdfs.bolt.sync.CountSyncPolicy"
    constructorArgs:
      - 1000

  - id: "rotationPolicy"
    className: "org.apache.storm.hdfs.bolt.rotation.NoRotationPolicy"

  - id: "fileNameFormat"
    className: "org.apache.storm.hdfs.bolt.format.DefaultFileNameFormat"
    configMethods:
      - name: "withPath"
        args: ["${hdfs.write.dir}"]
      - name: "withExtension"
        args: [".txt"]

  - id: "recordFormat"
    className: "org.apache.storm.hdfs.bolt.format.DelimitedRecordFormat"
    configMethods:
      - name: "withFieldDelimiter"
        args: ["|"]

# spout definitions
spouts:
  - id: "tick-spout"
    className: "com.microsoft.example.TickSpout"
    parallelism: 1


# bolt definitions
bolts:
  - id: "hdfs-bolt"
    className: "org.apache.storm.hdfs.bolt.HdfsBolt"
    configMethods:
      - name: "withConfigKey"
        args: ["hdfs.config"]
      - name: "withFsUrl"
        args: ["${hdfs.url}"]
      - name: "withFileNameFormat"
        args: [ref: "fileNameFormat"]
      - name: "withRecordFormat"
        args: [ref: "recordFormat"]
      - name: "withRotationPolicy"
        args: [ref: "rotationPolicy"]
      - name: "withSyncPolicy"
        args: [ref: "syncPolicy"]
```

Tato YAML definuje hello následující položky:

* `syncPolicy`: Definuje, když jsou soubory synchronizována Vyprázdněné toohello systému souborů. V tomto příkladu každých 1000 řazené kolekce členů.
* `fileNameFormat`: Definuje hello cestu a název vzor toouse při zápisu souborů. V tomto příkladu je poskytnuta cesta hello za běhu pomocí filtru, a přípona souboru hello je `.txt`.
* `recordFormat`: Definuje hello interní formát souborů hello zapsána. V tomto příkladu pole jsou oddělená hello `|` znak.
* `rotationPolicy`: Určuje, kdy toorotate soubory. V tomto příkladu se provádí bez otočení.
* `hdfs-bolt`: Používá hello předchozí komponenty jako parametry konfigurace pro hello `HdfsBolt` třídy.

Další informace o hello tok framework najdete v tématu [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).

## <a name="configure-hello-cluster"></a>Konfigurace clusteru hello

Ve výchozím nastavení Storm v HDInsight nezahrnuje hello komponenty, který HdfsBolt používá toocommunicate s Azure Storage nebo Data Lake Store v Storm je cesta pro třídy. Použití hello následující skript akce tooadd tyto součásti toohello `extlib` adresář pro Storm v clusteru:

| Identifikátor URI skriptu | Uzly tooapply jeho | Parametry || `https://000aarperiscus.blob.core.windows.net/certs/stormextlib.sh` | Nimbus, nadřízeného | None |

Informace o použití tohoto skriptu k vašemu clusteru najdete v tématu hello [HDInsight přizpůsobit clustery pomocí akcí skriptů](./hdinsight-hadoop-customize-cluster-linux.md) dokumentu.

## <a name="build-and-package-hello-topology"></a>Sestavení a balíček hello topologie

1. Stáhnout hello příklad projektu ze [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) tooyour vývojové prostředí.

2. Z příkazového řádku stáhnou Terminálové nebo relace prostředí, změna adresáře toohello kořenovém hello projektu. toobuild a balíček hello topologie, použijte následující příkaz hello:
   
        mvn compile package
   
    Po dokončení sestavení hello a balení, je nový adresář s názvem `target`, který obsahuje soubor s názvem `StormToHdfs-1.0-SNAPSHOT.jar`. Tento soubor obsahuje topologie hello zkompilovat.

## <a name="deploy-and-run-hello-topology"></a>Nasazení a spuštění hello topologie

1. Použijte následující příkaz toocopy hello topologie toohello clusteru HDInsight hello. Nahraďte **uživatele** s uživatelským jménem SSH hello jste použili při vytváření clusteru hello. Nahraďte **CLUSTERNAME** s názvem hello hello clusteru.
   
        scp target\StormToHdfs-1.0-SNAPSHOT.jar USER@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs1.0-SNAPSHOT.jar
   
    Po zobrazení výzvy zadejte heslo hello použité při vytváření uživatele SSH hello hello clusteru. Pokud jste použili veřejný klíč místo hesla, může být nutné toouse hello `-i` parametr toospecify hello cesta toohello odpovídající privátní klíč.
   
   > [!NOTE]
   > Další informace o používání `scp` s HDInsight, najdete v části [použití SSH s HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md).

2. Po dokončení nahrávání hello použijte hello následující tooconnect toohello HDInsight clusteru pomocí protokolu SSH. Nahraďte **uživatele** s uživatelským jménem SSH hello jste použili při vytváření clusteru hello. Nahraďte **CLUSTERNAME** s názvem hello hello clusteru.
   
        ssh USER@CLUSTERNAME-ssh.azurehdinsight.net
   
    Po zobrazení výzvy zadejte heslo hello použité při vytváření uživatele SSH hello hello clusteru. Pokud jste použili veřejný klíč místo hesla, může být nutné toouse hello `-i` parametr toospecify hello cesta toohello odpovídající privátní klíč.
   
   Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Po připojení, použijte hello následující příkaz toocreate soubor s názvem `dev.properties`:

        nano dev.properties

4. Použití hello následující text jako hello obsah hello `dev.properties` souboru:

        hdfs.write.dir: /stormdata/
        hdfs.url: wasb:///

    > [!IMPORTANT]
    > Tento příklad předpokládá, že váš cluster používá účet úložiště Azure jako hello výchozí úložiště. Pokud váš cluster používá Azure Data Lake Store, použijte `hdfs.url: adl:///` místo.
    
    toosave hello soubor, použijte __kombinaci kláves Ctrl + X__, pak __Y__a v neposlední řadě __Enter__. Hello hodnoty v tomto souboru nastavit adresu URL úložiště Data Lake hello a hello název adresáře, která data se zapisují do.

3. Použijte následující příkaz toostart hello topologie hello:
   
        storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties

    Tento příkaz spustí hello topologie pomocí hello tok framework odesláním uzel Nimbus toohello hello clusteru. topologie Hello je definována hello `writetohdfs.yaml` zahrnutý v hello jar. Hello `dev.properties` soubor je předán jako filtr a hodnoty hello obsažené v souboru hello se čtou podle topologie hello.

## <a name="view-output-data"></a>Zobrazení výstupní data

tooview hello data, hello použijte následující příkaz:

    hdfs dfs -ls /stormdata/

Zobrazí se seznam hello soubory vytvořené v této topologii.

Hello následujícím seznamu je příklad dat hello retuned podle předchozích příkazů hello:

    Found 30 items
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-0-1488568403092.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-1-1488568404567.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-10-1488568408678.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-11-1488568411636.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-12-1488568411884.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-13-1488568412603.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-14-1488568415055.txt

## <a name="stop-hello-topology"></a>Zastavení topologie hello

Topologie Storm spustit, dokud nebude zastaven nebo odstranění clusteru hello. toostop hello topologie, hello použijte následující příkaz:

    storm kill hdfswriter

## <a name="delete-your-cluster"></a>Odstranění clusteru

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili, jak toouse Storm toowrite tooAzure úložiště a Azure Data Lake Store, zjišťovat, jiné [Storm příklady pro HDInsight](hdinsight-storm-example-topology.md).

