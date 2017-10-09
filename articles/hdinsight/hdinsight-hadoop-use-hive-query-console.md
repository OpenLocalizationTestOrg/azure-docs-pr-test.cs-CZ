---
title: aaaUse Hadoop Hive na hello konzoly dotazu v HDInsight - Azure | Microsoft Docs
description: "Zjistěte, jak toouse hello webové konzole dotazu toorun dotazů Hive v HDInsight Hadoop clusteru z prohlížeče."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5ae074b0-f55e-472d-94a7-005b0e79f779
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 621882082c9a07655d34b8dc980b8e47dd04b745
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hello-query-console"></a>Spouštění dotazů Hive pomocí hello dotazu konzoly
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

V tomto článku se dozvíte, jak dotazů Hive toouse hello HDInsight dotazu konzoly toorun na HDInsight Hadoop clusteru z prohlížeče.

> [!IMPORTANT]
> Hello konzoly dotazu HDInsight je dostupná pouze na clustery HDInsight se systémem Windows. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> HDInsight 3.4 nebo větší, projděte si téma [spouštění dotazů Hive v zobrazení Ambari Hive](hdinsight-hadoop-use-hive-ambari-view.md) informace o spouštění dotazů Hive z webového prohlížeče.

## <a id="prereq"></a>Požadavky
toocomplete hello kroky v tomto článku, budete potřebovat následující hello.

* Cluster HDInsight Hadoop založené na systému Windows
* Moderní webový prohlížeč

## <a id="run"></a>Spouštění dotazů Hive pomocí hello dotazu konzoly
1. Otevřete webový prohlížeč a přejděte příliš**https://CLUSTERNAME.azurehdinsight.net**, kde **CLUSTERNAME** je hello název clusteru HDInsight. Pokud se zobrazí výzva, zadejte hello uživatelské jméno a heslo, které jste použili při vytvoření clusteru hello.
2. Hello odkazy v horní části hello hello stránky, vyberte **Hive Editor**. Zobrazí formulář, který lze použít tooenter hello HiveQL příkazy, které chcete toorun v clusteru HDInsight hello.

    ![Hello hive editor](./media/hdinsight-hadoop-use-hive-query-console/queryconsole.png)

    Nahraďte hello text `Select * from hivesampletable` s hello následující příkazy HiveQL:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Tyto příkazy provádět hello následující akce:

   * **VYŘAĎTE tabulku**: Odstraní tabulku hello a hello datový soubor, pokud tabulka hello již existuje.
   * **Vytvoření externí tabulky**: vytvoří novou tabulku "externí" v Hive. Externí tabulky uložit pouze definici tabulky hello Hive; Hello data je ponechán v hello původního umístění.

     > [!NOTE]
     > Externí tabulky by měl použít, pokud očekáváte, že hello základní data toobe aktualizovat externího zdroje (například procesu nahrávání automatizované dat), nebo jiná operace MapReduce, ale chcete, aby, že Hive dotazuje toouse hello nejnovější data.
     >
     > Vyřazení externí tabulku nemá **není** odstranit hello data, jenom hello definici tabulky.
     >
     >
   * **Řádek formátu**: informuje Hive formátování dat hello. V takovém případě hello pole v každém protokolu jsou oddělené mezerou.
   * **ULOŽENÉ umístění textový soubor AS**: informuje Hive, kde je hello data uložená (adresář hello příklad nebo data) a která je uložena jako text
   * **Vyberte**: Vyberte počet všech řádků kde sloupec **t4** obsahovat hodnotu hello **[Chyba]**. To by měla vrátit hodnotu **3** vzhledem k tomu, že existují tři řádky, které obsahují tuto hodnotu.
   * **INPUT__FILE__NAME jako '%.log'** -informuje Hive, který jsme by měl vrátit pouze data ze souborů končící na. log. To omezuje hello vyhledávání toohello sample.log souboru, který obsahuje hello data a udržuje ho z vrací data z jiných příkladu datové soubory, které neodpovídají hello schéma, které jsme definovali.
3. Klikněte na tlačítko **odeslání**. Hello **úlohy relace** v hello dolní části stránky hello by měl zobrazit podrobnosti úlohy hello.
4. Když hello **stav** pole změny příliš**dokončeno**, vyberte **zobrazit podrobnosti** pro úlohu hello. Na stránce s podrobnostmi o hello, hello **výstup úlohy** obsahuje `[ERROR]    3`. Můžete použít hello **Stáhnout** tlačítko pod toto pole toodownload soubor, který obsahuje hello výstup hello úlohy.

## <a id="summary"></a>Shrnutí
Jak vidíte, dotazy Hive v clusteru služby HDInsight hello dotazu Konzola poskytuje toorun snadno monitorovat stav úlohy hello a načíst výstup hello.

Vyberte toolearn Další informace o použití úlohy Hive toorun konzoly dotaz Hive, **Začínáme** hello horní části hello konzoly dotazu, pak použít hello vzorků, které jsou k dispozici. Každá ukázka vás provede procesem hello pomocí Hive tooanalyze data, včetně vysvětlení o příkazy HiveQL hello používá v ukázce hello.

## <a id="nextsteps"></a>Další kroky
Obecné informace o Hive v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)

Pokud používáte Tez s Hive, zobrazit hello následující dokumenty pro ladění informace:

* [Použití hello Tez uživatelského rozhraní na HDInsight se systémem Windows](hdinsight-debug-tez-ui.md)
* [Použití hello zobrazení Ambari Tez na HDInsight se systémem Linux](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
