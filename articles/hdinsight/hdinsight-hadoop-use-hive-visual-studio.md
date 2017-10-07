---
title: "aaaHive pomocí nástrojů Data Lake (Hadoop) pro Visual Studio – Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak nástroje toouse hello Data Lake pro Visual Studio toorun Apache Hive dotazy s Apache Hadoop v Azure HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2b3e672a-1195-4fa5-afb7-b7b73937bfbe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/07/2017
ms.author: larryfr
ms.openlocfilehash: dc76974c02cf68bcf701b2b155842c9e9c5cb988
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hello-data-lake-tools-for-visual-studio"></a>Spouštění dotazů Hive pomocí hello nástrojů Data Lake pro Visual Studio

Zjistěte, jak nástroje toouse hello Data Lake pro Visual Studio tooquery Apache Hive. nástroje Data Lake Hello umožňují tooeasily vytvoření, odeslání a sledování tooHadoop dotazy Hive v Azure HDInsight.

## <a id="prereq"></a>Požadavky

* Cluster Azure HDInsight (Hadoop v HDInsight)

  > [!IMPORTANT]
  > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio (jeden hello následující verze):

    * Visual Studio 2013 Community/Professional/Premium/Ultimate s aktualizací 4

    * Visual Studio 2015 (všechny edice)

    * Visual Studio 2017 (všechny edice)

* Nástroje HDInsight pro Visual Studio nebo nástroje Azure Data Lake pro Visual Studio. V tématu [začněte používat nástroje Visual Studio Hadoop pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) informace o instalaci a konfiguraci nástroje hello.

## <a id="run"></a>Spouštění dotazů Hive pomocí hello Visual Studio

1. Otevřete **Visual Studio** a vyberte **nový** > **projektu** > **Azure Data Lake**  >   **HIVE** > **podregistru aplikace**. Zadejte název pro tento projekt.

2. Otevřete hello **Script.hql** souboru, který je vytvořen s tímto projektu a vložte následující příkazy HiveQL hello:

   ```hiveql
   set hive.execution.engine=tez;
   DROP TABLE log4jLogs;
   CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
   STORED AS TEXTFILE LOCATION '/example/data/';
   SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   ```

    Tyto příkazy provádět hello následující akce:

   * `DROP TABLE`: Pokud hello tabulka existuje, tento příkaz se odstraní.

   * `CREATE EXTERNAL TABLE`: Vytvoří novou tabulku "externí" v Hive. Externí tabulky pouze uložit definici tabulky hello Hive (hello dat je ponecháno v původním umístění hello).

     > [!NOTE]
     > Externí tabulky by měl použít, pokud očekáváte, že hello základní data toobe aktualizovat externího zdroje. Například MapReduce úlohy nebo služby Azure.
     >
     > Vyřazení externí tabulku nemá **není** odstranit hello data, jenom hello definici tabulky.

   * `ROW FORMAT`: Určuje způsob formátování hello data Hive. V takovém případě hello pole v každém protokolu jsou oddělené mezerou.

   * `STORED AS TEXTFILE LOCATION`: Informuje Hive kde hello data se ukládají (adresář hello příklad nebo data) a která je uložena jako text.

   * `SELECT`: Vyberte počet všech řádků kde sloupec `t4` obsahuje hodnotu hello `[ERROR]`. Tento příkaz vrátí hodnotu `3` vzhledem k tomu, že existují tři řádky, které obsahují tuto hodnotu.

   * `INPUT__FILE__NAME LIKE '%.log'`-Informuje Hive, jsme by měl vrátit pouze data ze souborů končící na. log. Tuto klauzuli omezuje hello vyhledávání toohello sample.log soubor, který obsahuje hello data.

3. Hello nástrojů vyberte hello **clusteru HDInsight** chcete toouse pro tento dotaz. Vyberte **odeslání** toorun hello příkazy jako úlohy Hive.

   ![Odeslání panelu](./media/hdinsight-hadoop-use-hive-visual-studio/toolbar.png)

4. Hello **Souhrn úlohy Hive** se zobrazí a zobrazí informace o hello spuštění úlohy. Použití hello **aktualizovat** odkaz toorefresh hello informace o úlohách, dokud hello **stav úlohy** změní příliš**dokončeno**.

   ![Souhrn úlohy zobrazení dokončené úlohy](./media/hdinsight-hadoop-use-hive-visual-studio/jobsummary.png)

5. Použití hello **výstup úlohy** odkaz výstup hello tooview této úlohy. Zobrazuje `[ERROR] 3`, což je hello hodnoty vrácené tímto dotazem.

6. Můžete také spustit dotazů Hive bez vytvoření projektu. Pomocí **Průzkumníka serveru**, rozbalte položku **Azure** > **HDInsight**, klikněte pravým tlačítkem na serveru HDInsight a pak vyberte **napsat dotaz Hive** .

7. V hello **temp.hql** dokument, který se zobrazí, přidejte následující příkazy HiveQL hello:

   ```hiveql
   set hive.execution.engine=tez;
   CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
   INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   ```

    Tyto příkazy provádět hello následující akce:

   * `CREATE TABLE IF NOT EXISTS`: Vytvoří tabulku, pokud ještě neexistuje. Protože hello `EXTERNAL` – klíčové slovo není, tento příkaz vytvoří interní tabulku. Interní tabulky jsou uložené v hello Hive datového skladu a jsou spravovány nástrojem Hive.

     > [!NOTE]
     > Na rozdíl od `EXTERNAL` tabulky, vyřazení interní tabulku taky odstraní hello základní data.

   * `STORED AS ORC`: Hello úložiště dat ve sloupcovém formátu (ORC) optimalizované řádek. ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.

   * `INSERT OVERWRITE ... SELECT`: Vybere řádky z hello `log4jLogs` tabulku, která obsahují `[ERROR]`, pak vloží hello data do hello `errorLogs` tabulky.

8. Hello nástrojů vyberte **odeslání** toorun hello úlohy. Použití hello **stav úlohy** toodetermine tuto úlohu hello byla úspěšně dokončena.

9. tooverify, který hello úlohy vytvoření hello tabulky, použijte **Průzkumníka serveru** a rozbalte **Azure** > **HDInsight** > clusteru HDInsight > **Databáze hive** > **výchozí**. Hello **errorLogs** tabulky a hello **log4jLogs** tabulce jsou uvedeny.

## <a id="nextsteps"></a>Další kroky

Jak vidíte, zadejte hello nástroje HDInsight pro Visual Studio snadno toowork s dotazy Hive v HDInsight.

Obecné informace o Hive v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)

* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)

Další informace o hello nástroje HDInsight pro Visual Studio:

* [Začínáme s nástroje HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md)

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

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
