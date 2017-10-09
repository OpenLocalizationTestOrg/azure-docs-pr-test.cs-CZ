---
title: aaaWhat je Apache Hive a HiveQL - Azure HDInsight | Microsoft Docs
description: "Apache Hive je systém datového skladu pro Hadoop. Můžete dát dotaz na data uložená v Hive pomocí HiveQL, které podobné tooTransact-SQL. V tomto dokumentu, zjistěte, jak toouse Hive a HiveQL s Azure HDInsight."
keywords: "hiveql, co je hive, hadoop hiveql, hive, co je hive zjistěte, jak toouse hive,"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2c10f989-7636-41bf-b7f7-c4b67ec0814f
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: a2772312263895ff99b499898264c2e6d5e816e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a>Co je Apache Hive a HiveQL v Azure HDInsight?

[Apache Hive](http://hive.apache.org/) je systém datového skladu pro Hadoop. Hive umožňuje shrnutí dat, dotazy a analýzy dat. Dotazy Hive jsou zapsány ve HiveQL, který je podobný tooSQL dotaz jazyka.

Hive můžete tooproject struktura na do značné míry Nestrukturovaná data. Po definování hello struktura, můžete použít HiveQL tooquery hello dat bez znalosti Java nebo MapReduce.

HDInsight poskytuje několik typů clusteru, které jsou přizpůsobená pro konkrétní úlohy. Hello následující typy clusteru se nejčastěji používají pro dotazy Hive:

* __Interaktivní Hive__: cluster A Hadoop, který poskytuje [nízká latence analytického zpracování (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) doby odezvy tooimprove funkce pro interaktivní dotazy. Další informace najdete v tématu hello [začínat interaktivní Hive v HDInsight](hdinsight-hadoop-use-interactive-hive.md) dokumentu.

* __Hadoop__: cluster A Hadoop, který je přizpůsobená pro dávkové zpracování úlohy. Další informace najdete v tématu hello [začněte s Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) dokumentu.

* __Spark__: Apache Spark obsahuje integrovanou funkci pro práci s Hive. Další informace najdete v tématu hello [začínat Spark v HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) dokumentu.

* __HBase__: HiveQL lze použít tooquery data uložená v HBase. Další informace najdete v tématu hello [začínat HBase v HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) dokumentu.

## <a name="how-toouse-hive"></a>Jak toouse Hive

Použijte následující tabulku toodiscover jak toouse Hive s HDInsight hello:

| **Tuto metodu použijte** Pokud chcete... | .. .an **interaktivní** prostředí | ... **batch** zpracování | .. při to **clusteru operačního systému** | .. .from to **klientský operační systém** |
|:--- |:---:|:---:|:--- |:--- |
| [Zobrazení Hive](hdinsight-hadoop-use-hive-ambari-view.md) |✔ |✔ |Linux |Žádné (prohlížeč na základě) |
| [Beeline klienta](hdinsight-hadoop-use-hive-beeline.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X nebo systému Windows |
| [REST API](hdinsight-hadoop-use-hive-curl.md) |&nbsp; |✔ |Linux nebo Windows * |Linux, Unix, Mac OS X nebo systému Windows |
| [Nástroje HDInsight pro Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |&nbsp; |✔ |Linux nebo Windows * |Windows |
| [Prostředí Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md) |&nbsp; |✔ |Linux nebo Windows * |Windows |

> [!IMPORTANT]
> \*Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Pokud používáte cluster HDInsight se systémem Windows, můžete použít hello [dotazu konzoly](hdinsight-hadoop-use-hive-query-console.md) z prohlížeče nebo [vzdálené plochy](hdinsight-hadoop-use-hive-remote-desktop.md) toorun dotazů Hive.

## <a name="hiveql-language-reference"></a>Referenční dokumentace jazyka HiveQL

Referenční dokumentace jazyka HiveQL je k dispozici v hello [jazyk ručně (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).

## <a name="hive-and-data-structure"></a>Hive a datové struktury

Tom, jak toowork s strukturovaná a částečně strukturovaných dat plně chápe Hive. Například textové soubory kde hello pole jsou oddělená konkrétní znaků. Následující příkaz HiveQL Hello vytvoří tabulku nad daty oddělených mezerami:

```hiveql
CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

Hive podporuje i vlastní **serializátor/deserializers (SerDe)** složitý nebo nepravidelně strukturovaných dat. Další informace najdete v tématu hello [jak toouse vlastní SerDe JSON s HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) dokumentu.

Další informace o souboru formátů podporovaných Hive naleznete v tématu hello [jazyk ručně (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)

## <a name="hive-internal-tables-vs-external-tables"></a>Interní tabulky vs externích tabulek Hive

Existují dva typy tabulek, které můžete vytvořit pomocí Hive:

* __Interní__: Data jsou uložena v hello Hive datového skladu. Hello datového skladu je umístěn v `/hive/warehouse/` na hello výchozí úložiště pro hello cluster.

    Použití interní tabulky, když:

    * Data jsou dočasné.
    * Chcete Hive toomanage hello životní cyklus hello tabulky a data.

* __Externí__: Data jsou uložena mimo hello datového skladu. Hello data se uloží na jakékoli úložiště dostupné hello cluster.

    Použijte externí tabulky, když:

    * Hello dat se používá i mimo Hive. Například hello datové soubory jsou aktualizovány jiným procesem (které nenutí hello soubory.)
    * Data je tooremain v hello základní umístění, i po vyřazení hello tabulky.
    * Je nutné do vlastního umístění, jako je například účet jiné než výchozí úložiště.
    * Program než hive spravuje hello formát dat, umístění atd.

Další informace najdete v tématu hello [Hive interní a externí tabulky ÚVOD] [ cindygross-hive-tables] příspěvku na blogu.

## <a name="user-defined-functions-udf"></a>Uživatelem definované funkce (UDF)

Hive lze také prostřednictvím rozšířit **uživatelsky definované funkce (UDF)**. Uživatelem definovanou FUNKCI umožňuje tooimplement funkce nebo logiky, která není snadno modelován v HiveQL. Příklad použití UDF s Hive naleznete v části hello následující dokumenty:

* [Uživatelem definované funkce Java pomocí Hive](hdinsight-hadoop-hive-java-udf.md)

* [Uživatelem definované funkce Python pomocí Hive a Pig](hdinsight-python.md)

* [C# uživatelem definované funkce pomocí Hive a Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Jak tooadd vlastní Hive uživatelem definované funkce tooHDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Na příkladu Hive uživatelsky definované funkce tooconvert formátu data a času tooHive časové razítko](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a id="data"></a>Příklad dat

Hive v HDInsight obsahuje předem načtený vnitřní tabulku s názvem `hivesampletable`. HDInsight také poskytuje příklad datových sad, které lze použít s Hive. Tyto sady dat se ukládají do hello `/example/data` a `/HdiSamples` adresáře. Tyto adresáře neexistují v hello výchozí úložiště pro cluster.

## <a id="job"></a>Příklad dotazu Hive

Hello následující sloupce projektu příkazy HiveQL do hello `/example/data/sample.log` souboru:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

V předchozím příkladu hello provádět příkazy HiveQL hello hello následující akce:

* `set hive.execution.engine=tez;`: Nastaví hello toouse spuštění modulu Tez. Pomocí Tez místo MapReduce můžete poskytnout zvýšení výkonu dotazů. Další informace o Tez naleznete v tématu hello [použití rozhraní Apache Tez pro zlepšení výkonu](#usetez) části.

    > [!NOTE]
    > Tento příkaz je jenom když používáte cluster HDInsight se systémem Windows je vyžadováno. Tez je hello výchozí spouštěcí modul pro HDInsight se systémem Linux.

* `DROP TABLE`: Pokud hello tabulka již existuje, odstraňte jej.

* `CREATE EXTERNAL TABLE`: Vytvoří novou **externí** tabulky v Hive. Externí tabulky pouze uložit definici tabulky hello Hive. Hello data je ponechán v původním umístění hello a v původním formátu hello.

* `ROW FORMAT`: Určuje způsob formátování hello data Hive. V takovém případě hello pole v každém protokolu jsou oddělené mezerou.

* `STORED AS TEXTFILE LOCATION`: Informuje Hive kde hello data se ukládají (hello `example/data` adresáře) a která je uložena jako text. Hello dat může být v jednom souboru nebo rozloženy více souborů v adresáři hello.

* `SELECT`: Vybere počet všech řádků kde hello sloupec **t4** obsahuje hodnotu hello **[Chyba]**. Tento příkaz vrátí hodnotu **3** vzhledem k tomu, že existují tři řádky, které obsahují tuto hodnotu.

* `INPUT__FILE__NAME LIKE '%.log'`-Hive pokusí tooapply hello schématu tooall soubory v adresáři hello. V takovém případě hello adresář obsahuje soubory, které neodpovídají schématu hello. tooprevent paměti data ve výsledcích hello, tento příkaz informuje Hive, jsme by měl vrátit pouze data ze souborů končící na. log.

> [!NOTE]
> Externí tabulky by měl použít, pokud očekáváte, že hello základní data toobe aktualizovat externího zdroje. Například procesu nahrávání automatizované dat, nebo operaci MapReduce.
>
> Vyřazení externí tabulku nemá **není** odstranit hello data, odstraní pouze hello definici tabulky.

toocreate **interní** místo externí tabulky, použijte následující HiveQL hello:

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Tyto příkazy provádět hello následující akce:

* `CREATE TABLE IF NOT EXISTS`: Pokud hello tabulka neexistuje, vytvořte ho. Protože hello **externí** – klíčové slovo není, tento příkaz vytvoří interní tabulku. Tabulka Hello je uložena v datovém skladu Hive hello a je zcela spravuje Hive.

* `STORED AS ORC`: Ukládá hello data ve formátu optimalizované řádek sloupcovém (ORC). ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.

* `INSERT OVERWRITE ... SELECT`: Vybere řádky z hello **log4jLogs** tabulku, která obsahuje **[Chyba]**, a poté vloží hello data do hello **errorLogs** tabulky.

> [!NOTE]
> Na rozdíl od externích tabulek se odstranit interní tabulku také odstraní hello základní data.

## <a name="improve-hive-query-performance"></a>Zlepšení výkonu dotazů Hive

### <a id="usetez"></a>Apache Tez

[Apache Tez](http://tez.apache.org) je rozhraní, které umožňuje data náročné aplikace, například Hive, toorun mnohem efektivněji ve velkém měřítku. Ve výchozím nastavení pro clustery HDInsight se systémem Linux je povolené tez.

> [!NOTE]
> Tez není momentálně vypnuto ve výchozím nastavení pro clustery HDInsight se systémem Windows a musí být povolena. tootake výhod Tez hello následující hodnota musí být nastavena pro dotaz Hive:
>
> `set hive.execution.engine=tez;`
>
> Tez je hello výchozí modul pro clustery HDInsight se systémem Linux.

Hello [Hive na dokumentech návrhu Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) obsahuje podrobnosti o hello volby implementace a vyladění konfigurace.

tooaid při ladění úloh proběhla za použití Tez, HDInsight poskytuje následující uživatelská webové rozhraní, které vám umožňují tooview podrobnosti o úlohách Tez hello:

* [Použití hello zobrazení Ambari Tez na HDInsight se systémem Linux](hdinsight-debug-ambari-tez-view.md)

* [Použití hello Tez uživatelského rozhraní na HDInsight se systémem Windows](hdinsight-debug-tez-ui.md)

### <a name="low-latency-analytical-processing-llap"></a>Nízká latence analytického zpracování (LLAP)

[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (někdy označované jako Live dlouhé a proces) je nová funkce v podregistru 2.0, který umožňuje ukládání do mezipaměti v paměti dotazů. LLAP umožňuje mnohem rychlejší, až dotazů Hive příliš[26 x rychlejší než Hive 1.x v některých případech](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).

HDInsight poskytuje LLAP v hello interaktivní Hive typ clusteru. Další informace najdete v tématu hello [začínat interaktivní Hive](hdinsight-hadoop-use-interactive-hive.md) dokumentu.

## <a name="hive-jobs-and-sql-server-integration-services"></a>Úlohy Hive a SQL Server Integration Services

Pomocí integrace služby SSIS (SQL Server) toorun úlohy Hive. Hello Azure Feature Pack pro službu SSIS poskytuje hello následující součásti, které pracují se úlohy Hive v HDInsight.

* [Úloha Azure HDInsight Hive][hivetask]

* [Správce připojení k Azure předplatného][connectionmanager]

Další informace o hello Azure Feature Pack pro služby SSIS [sem][ssispack].

## <a id="nextsteps"></a>Další kroky

Teď, když jste se naučili novinky Hive a jak toouse ho s Hadoop v HDInsight, hello použijte následující odkazy tooexplore jiné způsoby toowork s Azure HDInsight.

* [Nahrání dat tooHDInsight][hdinsight-upload-data]
* [Použití Pigu se službou HDInsight][hdinsight-use-pig]
* [Použijte úlohy MapReduce s HDInsight][hdinsight-use-mapreduce]

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
