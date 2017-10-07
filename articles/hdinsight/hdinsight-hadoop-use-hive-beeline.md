---
title: aaaUse Beeline s Apache Hive - Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak toouse hello Beeline klienta toorun Hive dotazy s Hadoop v HDInsight. Beeline je nástroj pro práci s HiveServer2 prostřednictvím JDBC."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: beeline hive, hive beeline
ms.assetid: 3adfb1ba-8924-4a13-98db-10a67ab24fca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: e788ff39f33d928808cfcb83a92f62ac9ae8ca09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-beeline-client-with-apache-hive"></a>Použijte hello Beeline klienta s Apache Hive

Zjistěte, jak toouse [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) dotazuje toorun Hive v HDInsight.

Beeline je klient Hive, který je zahrnutý v hello hlavních uzlech clusteru HDInsight. Beeline používá JDBC tooconnect tooHiveServer2, službě hostované na clusteru HDInsight. Můžete také použít Beeline tooaccess Hive v HDInsight vzdáleně přes hello internet. Hello následující tabulka obsahuje připojovací řetězce pro použití s Beeline:

| Kde spouštíte Beeline z | Parametry |
| --- | --- | --- |
| Do SSH připojení tooa headnode nebo Microsoft edge uzlu | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
| Mimo cluster hello | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

> [!NOTE]
> Nahraďte `admin` s hello clusteru přihlašovací účet pro váš cluster.
>
> Nahraďte `password` s hello heslo pro účet přihlášení hello clusteru.
>
> Nahraďte `clustername` s názvem hello clusteru HDInsight.

## <a id="prereq"></a>Požadavky

* Systémem Linux Hadoop v clusteru HDInsight.

  > [!IMPORTANT]
  > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Klientem SSH nebo místní Beeline klienta. Většina hello kroky v tomto dokumentu předpokládají, že používáte Beeline z clusteru toohello relace SSH. Informace o spouštění Beeline z mimo hello clusteru najdete v tématu hello [vzdáleně pomocí Beeline](#remote) části.

    Další informace o používání SSH najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="beeline"></a>Použití Beeline

1. Při spouštění Beeline, musí zadat připojovací řetězec pro HiveServer2 v clusteru HDInsight. příkaz hello toorun z mimo hello clusteru, musíte taky zadat název účtu přihlášení clusteru hello (výchozí `admin`) a heslo. Použijte následující tabulku toofind hello připojovací řetězec formátu a parametry toouse hello:

    | Kde spouštíte Beeline z | Parametry |
    | --- | --- | --- |
    | Do SSH připojení tooa headnode nebo Microsoft edge uzlu | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
    | Mimo cluster hello | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

    Například hello následující příkaz lze použít toostart Beeline z clusteru toohello relace SSH:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    Tento příkaz spustí hello Beeline klienta a připojí tooHiveServer2 hello hlavního uzlu clusteru. Po dokončení příkazu hello přijedete do `jdbc:hive2://headnodehost:10001/>` řádku.

2. Příkazy beeline začínat `!` znak, třeba `!help` zobrazí nápovědu. Ale hello `!` lze vynechat pro některé příkazy. Například `help` funguje taky.

    Je `!sql`, což je použité tooexecute příkazy HiveQL. HiveQL se ale tak běžně používá, můžete vynechat hello předchozí `!sql`. odpovídají Hello následující dva příkazy:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    Na novém clusteru, je uvedena pouze jednu tabulku: **hivesampletable**.

3. Použijte následující příkaz toodisplay hello schéma pro hello hivesampletable hello:

    ```hiveql
    describe hivesampletable;
    ```

    Tento příkaz vrátí hello následující informace:

        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Tyto informace popisují hello sloupců v tabulce hello. Když jsme může provádět některé dotazy pro tato data, místo toho vytvoříme nové tabulky toodemonstrate jak tooload data do Hive a použijte schéma.

4. Zadejte následující příkazy toocreate tabulku s názvem hello **log4jLogs** pomocí ukázkových dat, které jsou součástí clusteru HDInsight hello:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    Tyto příkazy provádět hello následující akce:

    * `DROP TABLE`– Pokud hello tabulka existuje, je odstraněn.

    * `CREATE EXTERNAL TABLE`-Vytvoří **externí** tabulky v Hive. Externí tabulky pouze uložit definici tabulky hello Hive. Hello data je ponechán v hello původního umístění.

    * `ROW FORMAT`-Hello data formátování. V takovém případě hello pole v každém protokolu jsou oddělené mezerou.

    * `STORED AS TEXTFILE LOCATION`-Tam, kde jsou uložena hello data a jaký formát souboru.

    * `SELECT`– Počet všech řádků vybere kde sloupec **t4** obsahuje hodnotu hello **[Chyba]**. Tento dotaz vrací hodnotu **3** jsou tři řádky, které obsahují tuto hodnotu.

    * `INPUT__FILE__NAME LIKE '%.log'`-Hive pokusí tooapply hello schématu tooall soubory v adresáři hello. V takovém případě hello adresář obsahuje soubory, které neodpovídají schématu hello. tooprevent paměti data ve výsledcích hello, tento příkaz informuje Hive, jsme by měl vrátit pouze data ze souborů končící na. log.

  > [!NOTE]
  > Externí tabulky by měl použít, pokud očekáváte, že hello základní data toobe aktualizovat externího zdroje. Například procesu nahrávání automatizované dat nebo operace MapReduce.
  >
  > Vyřazení externí tabulku nemá **není** odstranit hello data, jenom hello definici tabulky.

    Hello výstup tohoto příkazu je podobné toohello následující text:

        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :

        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)

        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

5. použít tooexit Beeline, `!exit`.

## <a id="file"></a>Použít Beeline toorun soubor HiveQL

Pomocí následujících kroků toocreate soubor, spusťte ji pomocí Beeline hello.

1. Použití hello následující příkaz toocreate soubor s názvem **query.hql**:

    ```bash
    nano query.hql
    ```

2. Použijte následující text jako hello obsah souboru hello hello. Tento dotaz vytvoří novou tabulku "interní" s názvem **errorLogs**:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Tyto příkazy provádět hello následující akce:

    * **Vytvoření tabulka není v případě existuje** – Pokud hello tabulky již neexistuje, vytvoří se. Od hello **externí** – klíčové slovo není, tento příkaz vytvoří interní tabulku. Interní tabulky jsou uložené v hello Hive datového skladu a jsou zcela spravovány Hive.
    * **ULOŽENÉ ORC AS** – ukládá hello data ve formátu optimalizované řádek sloupcovém (ORC). Formát ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.
    * **PŘEPSAT INSERT... Vyberte** -vybere řádky z hello **log4jLogs** tabulku, která obsahují **[Chyba]**, pak vloží hello data do hello **errorLogs** tabulky.

    > [!NOTE]
    > Na rozdíl od externích tabulek se odstranit interní tabulku odstraní hello základní data.

3. toosave hello soubor, použijte **Ctrl**+**_X**, pak zadejte **Y**a v neposlední řadě **Enter**.

4. Použijte následující soubor hello toorun pomocí Beeline hello:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]
    > Hello `-i` parametr spustí Beeline, spustí hello příkazy v souboru query.hql hello. Po dokončení dotazu hello přijedete do hello `jdbc:hive2://headnodehost:10001/>` řádku. Můžete taky spustit soubor pomocí hello `-f` parametr, který ukončí Beeline po dokončení dotazu hello.

5. tooverify, který hello **errorLogs** tabulka byla vytvořena, použijte následující příkaz tooreturn všechny řádky z hello hello **errorLogs**:

    ```hiveql
    SELECT * from errorLogs;
    ```

    Tří řádků dat má být vrácen, všechny obsahující **[Chyba]** v t4 sloupce:

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a id="remote"></a>Použít Beeline vzdáleně

Pokud máte Beeline nainstalovány místně, nebo ji používají prostřednictvím Docker image, jako [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/), je nutné použít hello následující parametry:

* __Připojovací řetězec__:`-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`

* __Přihlašovací jméno clusteru__:`-n admin`

* __Heslo pro přihlášení clusteru__`-p 'password'`

Nahraďte hello `clustername` v hello připojovací řetězec s názvem hello clusteru HDInsight.

Nahraďte `admin` s názvem hello přihlášení clusteru a nahraďte `password` hello heslem pro váš cluster přihlášení.

## <a id="sparksql"></a>Beeline pomocí Spark

Spark poskytuje svou vlastní implementaci systému HiveServer2, což se často stává serveru Spark Thrift hello tooas zmíněných. Tato služba používá dotazů Spark SQL tooresolve místo Hive a může poskytovat lepší výkon v závislosti na svůj dotaz.

tooconnect toohello serveru Spark Thrift Spark na clusteru HDInsight, použití portu `10002` místo `10001`. Například, `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.

> [!IMPORTANT]
> serveru Spark Thrift Hello nejsou přímo přístupné přes hello Internetu. Můžete připojit z relace SSH tooit nebo pouze uvnitř hello stejné virtuální síti Azure jako hello clusteru HDInsight.

## <a id="summary"></a><a id="nextsteps"></a>Další kroky

Další obecné informace o Hive v HDInsight najdete v části hello následujícím dokumentu:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)

Další informace o jiných způsobech můžete pracovat s Hadoop v HDInsight naleznete v tématu hello následující dokumenty:

* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)

Pokud používáte Tez s Hive, zobrazit hello následující dokumenty:

* [Použití hello Tez uživatelského rozhraní na HDInsight se systémem Windows](hdinsight-debug-tez-ui.md)
* [Použití hello zobrazení Ambari Tez na HDInsight se systémem Linux](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
