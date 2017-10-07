---
title: "aaaUse Hadoop Hive a Vzdálená plocha v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak tooconnect tooHadoop clusteru v prostředí HDInsight pomocí vzdálené plochy a pak spouštění dotazů Hive pomocí hello Hive rozhraní příkazového řádku."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c228e35-d58a-4f22-917a-1d20c9da89b4
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: f86ffc1be33a8b0b2346d1a1388e5dfa6d0f8777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Použijte Hive s Hadoop v HDInsight pomocí vzdálené plochy
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

V tomto článku se dozvíte, jak tooconnect tooan HDInsight clusteru pomocí vzdálené plochy a poté spusťte Hive dotazuje pomocí hello Hive rozhraní příkazového řádku (CLI).

> [!IMPORTANT]
> Vzdálená plocha je dostupná pouze na clustery HDInsight, které používají Windows jako hello operační systém. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> HDInsight 3.4 nebo větší, projděte si téma [používání Hive s HDInsight a Beeline](hdinsight-hadoop-use-hive-beeline.md) informace o spouštění dotazů Hive přímo na clusteru hello z příkazového řádku.

## <a id="prereq"></a>Požadavky
toocomplete hello kroky v tomto článku, budete potřebovat následující hello:

* Cluster HDInsight se systémem Windows (Hadoop v HDInsight)
* Klientský počítač se systémem Windows 10, Windows 8 nebo Windows 7

## <a id="connect"></a>Připojit pomocí vzdálené plochy
Povolení vzdálené plochy pro hello HDInsight cluster a pak připojit tooit podle následujících pokynů hello [připojit pomocí protokolu RDP clustery tooHDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hive"></a>Použijte příkaz Hive hello
Když připojíte toohello desktop pro hello HDInsight cluster, použijte následující kroky toowork s Hive hello:

1. Z plochy hello HDInsight, spusťte hello **Hadoop příkazového řádku**.
2. Zadejte následující příkaz toostart hello Hive CLI hello:

        %hive_home%\bin\hive

    Pokud bylo zahájeno hello rozhraní příkazového řádku, zobrazí se řádku hello Hive rozhraní příkazového řádku: `hive>`.
3. Pomocí hello rozhraní příkazového řádku, zadejte následující příkazy toocreate novou tabulku s názvem hello **log4jLogs** pomocí ukázkových dat:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Tyto příkazy provádět hello následující akce:

   * **VYŘAĎTE tabulku**: Odstraní tabulku hello a hello datový soubor, pokud tabulka hello již existuje.
   * **Vytvoření externí tabulky**: vytvoří novou tabulku "externí" v Hive. Externí tabulky uložit pouze definici tabulky hello Hive (hello dat je ponecháno v původním umístění hello).

     > [!NOTE]
     > Externí tabulky by měl použít, pokud očekáváte, že hello základní data toobe aktualizovat externího zdroje (například procesu nahrávání automatizované dat), nebo jiná operace MapReduce, ale chcete, aby, že Hive dotazuje toouse hello nejnovější data.
     >
     > Vyřazení externí tabulku nemá **není** odstranit hello data, jenom hello definici tabulky.
     >
     >
   * **Řádek formátu**: informuje Hive formátování dat hello. V takovém případě hello pole v každém protokolu jsou oddělené mezerou.
   * **ULOŽENÉ umístění textový soubor AS**: informuje Hive, kde je hello data uložená (adresář hello příklad nebo data) a která je uložena jako text.
   * **Vyberte**: počet všech řádků vybere kde sloupec **t4** obsahuje hodnotu hello **[Chyba]**. To by měla vrátit hodnotu **3** vzhledem k tomu, že existují tři řádky, které obsahují tuto hodnotu.
   * **INPUT__FILE__NAME jako '%.log'** -informuje Hive, který jsme by měl vrátit pouze data ze souborů končící na. log. To omezuje hello vyhledávání toohello sample.log souboru, který obsahuje hello data a udržuje ho z vrací data z jiných příkladu datové soubory, které neodpovídají hello schéma, které jsme definovali.
4. Hello použijte následující příkazy toocreate novou "interní" tabulku s názvem **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Tyto příkazy provádět hello následující akce:

   * **Vytvoření tabulka není v případě existuje**: vytvoří tabulku, pokud ještě neexistuje. Protože hello **externí** – klíčové slovo nepoužívá, jde o interní tabulku, která je uložena v datovém skladu hello Hive a je zcela spravuje Hive.

     > [!NOTE]
     > Na rozdíl od **externí** tabulky, vyřazení interní tabulku taky odstraní hello základní data.
     >
     >
   * **ULOŽENÉ ORC AS**: ukládá hello data ve sloupcovém formátu (ORC) optimalizované řádek. Toto je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.
   * **PŘEPSAT INSERT... Vyberte**: vybere řádky z hello **log4jLogs** tabulku, která obsahují **[Chyba]**, pak vloží hello data do hello **errorLogs** tabulky.

     tooverify, který pouze řádků, které obsahují **[Chyba]** ve sloupci t4 byly uložené toohello **errorLogs** tabulky, použijte následující příkaz tooreturn všechny řádky z hello hello **errorLogs**:

       Vybrat * z errorLogs;

     Tří řádků dat má být vrácen, všechny obsahující **[Chyba]** v t4 sloupce.

## <a id="summary"></a>Shrnutí
Jak vidíte, hello hello Hive příkaz poskytuje toointeractively snadný způsob spouštění dotazů Hive v clusteru HDInsight, monitorování hello stav úlohy a načíst výstup hello.

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





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
