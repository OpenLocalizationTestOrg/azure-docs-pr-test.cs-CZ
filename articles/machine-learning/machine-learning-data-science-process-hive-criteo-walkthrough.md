---
title: "Hello proces vědecké účely Team dat v akce – pomocí clusteru Azure HDInsight Hadoop na datovou sadu 1 TB | Microsoft Docs"
description: "Pomocí hello proces vědecké účely dat Team začátku do konce scénář nasazení HDInsight Hadoop clusteru toobuild a nasadit model pomocí velké veřejně dostupné datové sady (1 TB)"
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 72d958c4-3205-49b9-ad82-47998d400d2b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 59b2af02e7840cb60a4b5b2f2c8ab0611df198ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action---using-an-azure-hdinsight-hadoop-cluster-on-a-1-tb-dataset"></a>Hello proces vědecké účely Team dat v akce – pomocí clusteru Azure HDInsight Hadoop na 1 TB datové sady

V tomto návodu budeme demonstruje použití hello proces vědecké účely Team dat ve scénáři s začátku do konce s [clusteru Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) toostore, prozkoumejte, funkci pracovníka a veřejně dolů ukázková data z jednoho z hello k dispozici [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) datové sady. Tato data použijeme Azure Machine Learning toobuild model binární klasifikace. Také ukazují, jak toopublish jednu z těchto modelů jako webovou službu.

Je také možné toouse IPython poznámkového bloku tooaccomplish hello úlohy uvedené v tomto návodu. Uživatelé, kteří by jako tento postup by se měli obrátit tootry hello [Criteo návod pomocí připojení Hive ODBC](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) tématu.

## <a name="dataset"></a>Popis Criteo datové sady
Hello Criteo data jsou předpovědi klikněte na datovou sadu, která je přibližně 370 gzip komprimované TSV souborů (~1.3TB nekomprimovaným), která obsahuje více než miliardu 4.3 záznamů. Jsou převzaty z 24 dní klikněte na tlačítko dat, které poskytují [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/). Pro usnadnění práce hello z datových vědců jsme mít rozbalené data k dispozici toous tooexperiment s.

Každý záznam v této datové sadě obsahuje 40 sloupce:

* Hello první sloupec je sloupec popisek, který označuje, zda uživatel klikne **přidat** (hodnota 1) nebo není klikněte na jednu (hodnota 0)
* Další 13 sloupce jsou číselné, a
* poslední 26 jsou kategorií sloupců

Hello sloupce jsou anonymizovány a použijte řadu výčtové názvy: "Sloupec1" (pro sloupec popisek hello) příliš ' Col40 "(pro poslední sloupec kategorií hello).            

Tady je výňatek hello nejprve 20 sloupce dvě hodnoty (řádky) s tuto datovou sadu:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10    Col11    Col12    Col13    Col14    Col15            Col16            Col17            Col18            Col19        Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

V této datové sady je chybějící hodnoty ve sloupcích i hello číselné a kategorií. Jsme popisují jednoduchý způsob zpracování hello chybějící hodnoty. Další podrobné informace o datech hello jsou prozkoumali, když jsme ukládat do tabulek Hive.

**Definice:** *Interaktivní kurz (PEV.cenu):* Toto je procento hello kliknutí v hello data. V této datové sadě Criteo hello PEV.cenu je přibližně % 3.3 nebo 0.033.

## <a name="mltasks"></a>Příklady předpovědi úlohy
Dva ukázkové předpovědi problémy řešeny v tomto průvodci:

1. **Binární klasifikace**: předpovídá, zda uživatel klikli add:
   
   * Třída 0: Žádné klikněte na
   * Třídy 1: klikněte na
2. **Regrese**: předpovídá hello pravděpodobnost ad klikněte na z funkcí uživatelů.

## <a name="setup"></a>Nastavit až HDInsight Hadoop clusteru pro vědecké zpracování dat
**Poznámka:** se obvykle jedná **správce** úloh.

Nastavení prostředí vědecké zpracování dat Azure pro vytváření řešení prediktivní analýzy s clustery HDInsight v tři kroky:

1. [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md): Tento účet úložiště se použité toostore data v Azure Blob Storage. Zde jsou uložena data Hello používá v clusterech HDInsight.
2. [Přizpůsobení clusterů systému Hadoop HDInsight Azure pro vědecké zpracování dat](machine-learning-data-science-customize-hadoop-cluster.md): Tento krok vytvoří s 64-bit Anaconda Python 2.7 nainstalované na všech uzlech clusteru Azure HDInsight Hadoop. Existují dva důležité kroky (popsaný v tomto tématu) toocomplete při přizpůsobení hello clusteru HDInsight.
   
   * Je nutné propojit vytvořili v kroku 1 k vašemu clusteru HDInsight při vytvoření účtu úložiště hello. Tento účet úložiště se používá pro přístup k datům, která může být zpracována v rámci clusteru hello.
   * Po vytvoření je potřeba povolit vzdálený přístup toohello hlavního uzlu clusteru hello. Pamatovat přihlašovací údaje vzdáleného přístupu hello zde určíte (liší od nastavení zadané pro cluster hello při jeho vytváření): je potřebujete toocomplete hello následující postupy.
3. [Vytvořit pracovní prostor služby Azure ML](machine-learning-create-workspace.md): Tento Azure Machine Learning prostoru slouží k vytváření modelů machine learning po zkoumání počáteční data a dolů vzorkování na clusteru HDInsight hello.

## <a name="getdata"></a>Získání a využívat data z veřejné zdroje
Hello [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) datové sady je přístupná kliknutím na odkaz hello, přijetí hello podmínky použití a poskytnutím název. Zobrazí se zde snímek co vypadá takto:

![Přijměte podmínky Criteo](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

Klikněte na tlačítko **pokračovat tooDownload** tooread více informací o hello datovou sadu a jeho dostupnost.

Hello data se nachází v veřejné [úložiště objektů blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md) umístění: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. Hello "wasb" odkazuje tooAzure umístění úložiště objektů Blob. 

1. Hello dat v této veřejného objektu blob úložiště se skládá z tři podsložky rozbalené data.
   
   1. Hello podsložky *nezpracovanou nebo počet/* obsahuje hello první 21 dnů dat – dni\_00 tooday\_20
   2. Hello podsložky *nezpracovanou nebo train/* se skládá z jednoho dne dat, den\_21
   3. Hello podsložky *nezpracovaná/testovací/* se skládá ze dvou dnů dat, den\_22 a den\_23
2. Pro ty, kteří chtějí toostart s hello gzip nezpracovaná data, jsou taky dostupné dostupné ve složce hlavní hello *nezpracovanou nebo* jako day_NN.gz, kde NN přejde z 00 too23.

Tooaccess alternativní způsob prozkoumat a model dat, která nevyžaduje žádné místní stažení je popsáno dále v tomto návodu při vytváření tabulek Hive.

## <a name="login"></a>Přihlaste se headnode toohello clusteru
toolog v toohello headnode hello clusteru, použijte hello [portál Azure](https://ms.portal.azure.com) toolocate hello clusteru. Klikněte na ikonu Slon hello HDInsight na hello left a potom dvakrát klikněte na hello názvem vašeho clusteru. Přejděte toohello **konfigurace** kartě, dvakrát klikněte na ikonu připojit hello na hello dolní části stránky hello a zadejte své přihlašovací údaje vzdáleného přístupu při zobrazení výzvy. Tím přejdete toohello headnode hello clusteru.

Zde je typické prvním přihlášení headnode toohello clusteru, která bude vypadat takto:

![Přihlaste se toocluster](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)

Na levé straně hello vidíte hello "Hadoop příkazový řádek", což je naše Centrem pro zkoumání dat hello. Vidíme také dva užitečné adresy URL - "Hadoop Yarn stav" a "Hadoop název uzlu". Adresa URL Hello yarn stavu zobrazuje průběh úlohy a hello název uzlu adresa URL obsahuje údaje o konfiguraci clusteru hello.

Nyní jsme jsou nastavené a připravené toobegin první část hello návod: zkoumání dat pomocí Hive a získání dat připravené pro Azure Machine Learning.

## <a name="hive-db-tables"></a>Vytvoření databáze Hive a tabulky
toocreate Hive tabulky pro naší datové sadě Criteo otevřete hello ***Hadoop příkazového řádku*** hello plochy hello hlavního uzlu a zadejte adresář Hive hello zadáním příkazu hello

    cd %hive_home%\bin

> [!NOTE]
> Spusťte všechny příkazy Hive v tomto návodu z hello Hive bin / directory řádku. To postará o všech problémech, cesta automaticky. Používáme podmínky hello "Hive directory výzva", "Hive bin / directory řádku", "Hadoop příkazový řádek" a zcela zaměnitelným významem.
> 
> [!NOTE]
> tooexecute jakýkoli dotaz Hive jeden vždy použít hello následující příkazy:
> 
> 

        cd %hive_home%\bin
        hive

Po hello Hive REPL se zobrazí s "hive >" přihlásit, stačí vyjmout a vložit hello dotazu tooexecute ho.

Hello následující kód vytvoří databázi "criteo" a poté generuje 4 tabulky:

* *tabulku pro generování počty* založený na den dnů\_00 tooday\_20,
* *tabulky pro použití jako hello datovou sadu train* založený na den\_21, a
* dva *tabulky pro použití jako hello testovací datové sady* založený na den\_22 a den\_23 v uvedeném pořadí.

Protože jeden z hello dnů svátkem a chceme toodetermine, pokud hello model může zjistit rozdíly mezi svátek a bez svátek hello Interaktivní kurz jsme naše testovací datové sady rozdělit do dvou různých tabulek.

Hello skriptu [ukázka &#95; hive &#95; vytvoření &#95; criteo &#95; databázi &#95; a &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) se zde zobrazí ke zvýšení pohodlí:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

Jsme Upozorňujeme, že jsou všechny tyto tabulky externí jako můžeme jednoduše tooAzure bod umístění úložiště objektů Blob (wasb).

**Existují dva způsoby tooexecute ANY Hive dotaz, který jsme zmínili teď.**

1. **Pomocí příkazového řádku Hive REPL hello**: hello nejprve je tooissue příkaz "hive" a zkopírujte a vložte dotaz na hello Hive REPL příkazového řádku. toodo se:
   
        cd %hive_home%\bin
        hive
   
     Nyní v hello REPL příkazového řádku, vyjímání a vkládání hello dotazu se provede.
2. **Ukládání dotazuje tooa souborů a provádění příkazu hello**: hello druhou je toosave hello dotazy tooa .hql souboru ([ukázka &#95; hive &#95; vytvoření &#95; criteo &#95; databázi &#95; a &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) a pak problém hello následující příkaz tooexecute hello dotazu:
   
        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql

### <a name="confirm-database-and-table-creation"></a>Potvrďte vytváření databáze a tabulky
V dalším kroku potvrdíme hello vytvoření databáze hello s hello následující příkaz z hello Hive bin / directory řádku:

        hive -e "show databases;"

Díky tomu jsou:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Potvrdíte hello vytvoření nové databáze hello "criteo".

toosee tabulky, které jsme vytvořili, můžeme jednoduše příkaz hello zde z hello Hive bin / directory řádku:

        hive -e "show tables in criteo;"

Potom vidíme hello následující výstup:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

## <a name="exploration"></a>Zkoumání dat v Hive
Teď nemůžeme připraveni toodo některé základní data zkoumání v Hive. Začněte tím, že počítání hello počet příklady v hello train jsme test datových tabulek.

### <a name="number-of-train-examples"></a>Počet train příklady
Hello obsah [ukázka &#95; hive &#95; počet &#95; train &#95; tabulky &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) zde se zobrazují:

        SELECT COUNT(*) FROM criteo.criteo_train;

Dostaneme:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Alternativně jeden mohou také vystavovat hello následující příkaz z hello Hive bin / directory řádku:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-hello-two-test-datasets"></a>Počet příklady testů v testovací hello dvě datové sady
Jsme teď počet hello příklady v hello dvě testovací datové sady. Hello obsah [ukázka &#95; hive &#95; počet &#95; criteo &#95; testovací &#95; den &#95; 22 &#95; tabulky &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) jsou tady:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Dostaneme:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Obvyklým způsobem, může také říkáme hello skriptu z hello Hive bin / directory výzva po vydání příkazu hello:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Nakonec jsme zkontrolujte hello počet testovací příklady v hello testovací datové sady na základě dne\_23.

Hello příkaz toodo Toto je podobné toohello jeden jenom (odkazovat příliš[ukázka &#95; hive &#95; počet &#95; criteo &#95; testovací &#95; den &#95; 23 &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Díky tomu jsou:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-hello-train-dataset"></a>Popisek distribuce v datové sadě train hello
Popisek distribuční Hello v datové sadě train hello je určen. toosee toho jsme zobrazit obsah [ukázka &#95; hive &#95; criteo &#95; popisek &#95; distribuční &#95; train &#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Dostaneme distribuční popisek hello:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Všimněte si, že procento kladné popisky hello je přibližně 3.3 % (konzistentní s původní datové sady hello).

### <a name="histogram-distributions-of-some-numeric-variables-in-hello-train-dataset"></a>Histogram distribuce některých číselné proměnných ve hello cvičení datové sady
Můžeme použít nativní Hive na "histogram\_číselné" toofind na jaké hello distribuční číselné proměnných hello vypadá jako funkce. Tady jsou hello obsah [ukázka &#95; hive &#95; criteo &#95; histogram &#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Dostaneme hello následující:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

Hello LATERÁLNÍ zobrazení – rozbalit kombinace v podregistru slouží tooproduce SQL jako výstup místo obvyklé seznamu hello. Všimněte si, že v hello tuto tabulku, první sloupec hello odpovídá toohello bin center a hello druhý toohello bin četnost.

### <a name="approximate-percentiles-of-some-numeric-variables-in-hello-train-dataset"></a>Přibližná percentily některé číselné proměnné v hello cvičení datové sady
Také týkající se s číselnou proměnné je výpočet hello přibližnou percentily. Hive je nativní "percentilu\_přibl" tomu pro nás. Hello obsah [ukázka &#95; hive &#95; criteo &#95; přibližnou &#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) jsou:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Dostaneme:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

Jsme remark hello distribuce percentily obvykle je úzce související toohello histogram distribuční jakékoli číselné proměnné.         

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-hello-train-dataset"></a>Najít počet jedinečných hodnot pro některé kategorií sloupců v datové sadě train hello
Pokračováním hello zkoumání dat, nám teď najít, pro některé kategorií sloupce hello počet jedinečných hodnot, které jejich trvat. toodo toho jsme zobrazit obsah [ukázka &#95; hive &#95; criteo &#95; jedinečný &#95; hodnoty &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Dostaneme:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Jsme Všimněte si, že Col15 má jedinečné hodnoty 19M! Pomocí technik naïve jako "horkou jeden kódování" tooencode je nemožné takovéto vysokou dimenzí kategorií proměnné. Konkrétně jsme vysvětlují a ukázka efektivní a výkonné techniku zvanou [Learning s počty](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) k boji efektivně se tento problém.

Zde jsme ukončení prohlížením hello počet jedinečných hodnot pro některé kategorií sloupce také. Hello obsah [ukázka &#95; hive &#95; criteo &#95; jedinečný &#95; hodnoty &#95; několika &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) jsou:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Dostaneme:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Znovu vidíme, že s výjimkou Col20, všechny hello dalších sloupce mít mnoho jedinečné hodnoty.

### <a name="co-occurrence-counts-of-pairs-of-categorical-variables-in-hello-train-dataset"></a>Počty společné výskyt párů kategorií proměnných v datové sadě train hello

počty společné výskyt Hello párů kategorií proměnných je také určen. To se dá určit pomocí hello kódu v [ukázka &#95; hive &#95; criteo &#95; spárované &#95; kategorií &#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Jsme reverse počty hello pořadí podle jejich výskytu a podívejte se na horní hello 15 v tomto případě. To nám poskytuje:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a>Dolů ukázkových datových sad hello pro Azure Machine Learning
S prozkoumané hello datové sady a ukázán, jak jsme může tak, aby jsme mohou vytvářet modely v Azure Machine Learning se tento typ zkoumání pro všechny proměnné (včetně kombinací), jsme teď dolů ukázkových hello datových sad. Odvolat je tohoto problému hello zaměříme na to: danou sadu atributů příklad (funkce hodnoty z Col2 - Col40), jsme předpovědi, pokud je Sloupec1 0 (žádné kliknutím) nebo 1 (kliknutím).

toodown ukázkové naše train a testovací datové sady too1 % hello původní velikost, používáme nativní funkce RAND() na Hive. Hello další skript, [ukázka &#95; hive &#95; criteo &#95; převzorkovat &#95; train &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) tomu pro datovou sadu train hello:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Dostaneme:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Hello skriptu [ukázka &#95; hive &#95; criteo &#95; převzorkovat &#95; testovací &#95; den &#95; 22 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) nemá pro testovací data den\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Dostaneme:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Nakonec hello skriptu [ukázka &#95; hive &#95; criteo &#95; převzorkovat &#95; testovací &#95; den &#95; 23 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) nemá pro testovací data den\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Dostaneme:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

S tím, jsme jsou připravené toouse naše nižší vzorkovat natrénuje a otestuje datové sady pro vytváření modelů v Azure Machine Learning.

Před jsme přesunout na tooAzure Machine Learning, což je obavy hello počet tabulky je poslední důležité součásti. V další části dílčí hello probereme, to v některých podrobností.

## <a name="count"></a>Stručný diskuse v tabulce počet hello
Jak jsme viděli, několik kategorií proměnné mají velmi vysoká dimenzionalitu. V našem návodu Představujeme výkonné techniku zvanou [Learning s počty](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) tooencode tyto proměnné efektivní a výkonné způsobem. Další informace o této technice je v hello uvedeného odkazu.

[!NOTE]
>V tomto návodu zaměříme na pomocí compact reprezentace tooproduce počet tabulek dimenzí vysokou kategorií funkcí. Toto není hello pouze způsob tooencode kategorií funkcí. Další informace o jinými technikami, můžete také zajímat, uživatelé rezervovat [jeden hot kódování](http://en.wikipedia.org/wiki/One-hot) a [hashování](http://en.wikipedia.org/wiki/Feature_hashing).
>

počet tabulek toobuild na údaje o počtu hello, použijeme hello data hello složky nezpracovanou nebo počet. V hello modelování části ukážeme uživatelé jak se toobuild tyto počet tabulek pro kategorií funkce od začátku, případně toouse předdefinovaných počet tabulku pro jejich explorations. V co způsobem, když označujeme příliš "předem vytvořené tabulky počet", jsme znamená použití hello počet tabulek, které poskytujeme. Podrobné pokyny, jak tooaccess těchto tabulkách jsou uvedeny v následující části hello.

## <a name="aml"></a>Vytvoření modelu pomocí Azure Machine Learning
Naše model procesu v Azure Machine Learning vytváření zahrnuje následující kroky:

1. [Získat hello data z tabulek Hive do Azure Machine Learning](#step1)
2. [Vytvoření experimentu hello: Vyčištění dat hello a featurize s počet tabulek](#step2)
3. [Sestavení, train a score hello model](#step3)
4. [Vyhodnocení modelu hello](#step4)
5. [Publikování modelu hello jako webové služby](#step5)

Nyní jsme připravené toobuild modely v Azure Machine Learning studio. Naše dolů jen Vzorkovaná data se uloží jako tabulek Hive v clusteru hello. Používáme hello Azure Machine Learning **importovat Data** modulu tooread tato data. účet úložiště Hello pověření tooaccess hello tohoto clusteru jsou uvedené v jaké způsobem.

### <a name="step1"></a>Krok 1: Načíst data do Azure Machine Learning pomocí modulu hello importovat Data z tabulek Hive a vyberte pro experimentu strojového učení
Začněte výběrem **+ nový** -> **EXPERIMENTU** -> **prázdný Experiment**. Potom z hello **vyhledávání** pole hello nahoře vlevo, vyhledejte "Importovat Data". Přetažení hello **importovat Data** modulu v toohello experimentu plátno (hello střední část úvodní obrazovka) toouse hello modulu pro přístup k datům.

Toto je co hello **importovat Data** vypadá podobně jako při získávání dat z tabulky Hive hello:

![Umožňuje importovat Data získá data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

Pro hello **importovat Data** modulu, hello hodnoty hello parametry, které jsou k dispozici v hello obrázku jsou jenom příklady hello seřadit hodnoty, je nutné tooprovide. Tady je některé obecné pokyny na tom, jak nastavit toofill vnější hello parametr pro hello **importovat Data** modulu.

1. Volba "Dotaz Hive" pro **zdroje dat**
2. V hello **databázový dotaz Hive** pole jednoduchý příkaz SELECT * FROM < vaší\_databáze\_name.your\_tabulky\_název >-stačí.
3. **Identifikátor URI serveru Hcatalog**: Pokud je váš cluster "abc", pak toto je jednoduše: https://abc.azurehdinsight.net
4. **Název uživatelského účtu Hadoop**: uživatelské jméno hello zvolena hello při uvedení do provozu hello clusteru. (Není hello RAS uživatelské jméno!)
5. **Heslo uživatelského účtu Hadoop**: hello heslo pro uživatelské jméno hello zvolena hello při uvedení do provozu hello clusteru. (Není hello vzdáleného přístupu heslo!)
6. **Umístění výstupu dat**: Zvolte "Azure"
7. **Název účtu úložiště Azure**: hello účtu úložiště přidruženého k hello clusteru
8. **Klíč účtu úložiště Azure**: hello klíč účtu úložiště hello přidruženého k hello clusteru.
9. **Název kontejneru Azure**: Pokud je název clusteru hello "abc", pak toto je jednoduše "abc", obvykle.

Jednou hello **importovat Data** dokončení získání dat (uvidíte hello zelená značka na hello modulu), uložte tato data jako datové sady (s názvem podle svého výběru). Co to vypadá takto:

![Umožňuje importovat Data uložit data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

Hello pravým tlačítkem klikněte na výstupní port hello **importovat Data** modulu. Zobrazí se **uložit jako datovou sadu** možnost a **vizualizovat** možnost. Hello **vizualizovat** možnost, pokud kliknutí zobrazí 100 řádků hello dat, včetně pravém panelu, které jsou užitečné pro některé souhrnné statistiky. toosave dat, jednoduše vyberte **uložit jako datovou sadu** a postupujte podle pokynů.

Datová sada uložena hello tooselect pro použití v experimentu machine learning, vyhledejte hello datové sady používající hello **vyhledávání** pole ukazuje následující obrázek hello. Potom jednoduše typu out dáte název hello hello datovou sadu částečně hello tooaccess a přetáhněte hello datové sady na hlavní panel. Umístíte jej na hlavní panel hello vybere pro použití v machine learning modelování.

![Datová sada Drage na hlavní panel hello](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

> [!NOTE]
> To lze proveďte pro hello train i hello testovací datové sady. Nezapomeňte taky, název databáze hello toouse a názvy tabulek, které jste zadali pro tento účel. hodnoty Hello hello obrázku jsou výhradně pro obrázek purposes.* *
> 
> 

### <a name="step2"></a>Krok 2: Vytvoření jednoduchého experimentu v nástroji Azure Machine Learning toopredict klikne na tlačítko nebo bez kliknutí
Naše Azure ML experimentu vypadá takto:

![Machine Learning experimentu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

Nyní jsme zkontrolujte hello klíče součástí tohoto experimentu. Připomínáme potřebujeme toodrag naše uložené natrénuje a otestuje datové sady na plátno experimentu tooour nejdřív.

#### <a name="clean-missing-data"></a>Vyčištění chybějících dat
Hello **vyčištění chybějících dat** modul nemá naznačuje název: ho vyčistí chybějící data způsobem, který může být zadán uživatel. Vyhledávání na tento modul, vidíme toto:

![Vyčištění chybějících dat](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

Zde jsme zvolili tooreplace všechny chybějící hodnoty s 0. Existují další možnosti, které se můžete podívat prohlížením hello rozevíracích seznamů v modulu hello.

#### <a name="feature-engineering-on-hello-data"></a>Konstruování hello dat
Může existovat miliony jedinečné hodnoty pro některé kategorií funkce rozsáhlých datových sad. Pomocí metod naïve třeba jeden horkou kódování představující vysokou dimenzí kategorií funkce, je zcela nebude proveditelné. V tomto návodu ukážeme, jak toouse počet funkcí s použitím toogenerate moduly vestavěné Azure Machine Learning compact reprezentace těchto vysoce dimenzí kategorií proměnných. je menší velikost modelu, školení rychlejší a metriky výkonu, které jsou poměrně porovnatelný z hlediska toousing zprostředkovatele Hello konečný výsledek další techniky.

##### <a name="building-counting-transforms"></a>Vytváření počítání transformací
Funkce count toobuild, používáme hello **sestavení počítání transformace** modul, který je k dispozici v Azure Machine Learning. modul Hello vypadá takto:

![Sestavení modulu počítání transformace](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![sestavení počítání transformace modulu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)

> [!IMPORTANT] 
> V hello **počet sloupců** pole jsme zadejte tyto sloupce, které jsme si přejí tooperform počty. Obvykle jde o (jak je uvedeno) vysokou dimenzí kategorií sloupců. Při spuštění hello jsme uvedených tuto datovou sadu Criteo hello má 26 kategorií sloupců: z Col15 tooCol40. Zde jsme počet všem z nich a umožňují jejich indexy (z 15 too40 oddělených čárkami, jak je znázorněno).
> 

modul hello toouse v hello MapReduce režimu (vhodné pro velké datové sady), jsme potřebovat přístup ke clusteru HDInsight Hadoop tooan (pro zkoumání funkce lze znovu použít pro tento účel také hello) a přihlašovací údaje. Hello předchozí následující obrázky znázorňují, které hello vyplněné hodnoty vypadá jako (Nahraďte hodnoty hello ilustrativní s těmi, které jsou relevantní pro vlastní případ použití).

![Parametry modulu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

V předchozí obrázek hello, ukážeme, jak tooenter hello vstupní blob umístění. Toto umístění obsahuje data hello vyhrazena pro vytváření počet tabulek.

Po dokončení tohoto modulu můžeme uložit hello transformace pro později hello modulu kliknete pravým tlačítkem a výběrem hello **uložit jako transformace** možnost:

![Možnost "Uložit jako transformace"](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

V našem experimentu architektuře výše uvedeném hello datovou sadu "ytransform2" odpovídá přesněji tooa uložit počet transformace. Pro hello zbývající část tohoto experimentu, předpokládáme, že čtečka hello používá **sestavení počítání transformace** modulu v některé počty toogenerate data a pak můžete použít tyto počty toogenerate počet funkce na hello train a testovací datové sady.

##### <a name="choosing-what-count-features-tooinclude-as-part-of-hello-train-and-test-datasets"></a>Výběr co funkce tooinclude se počítají jako součást hello train a testovací datové sady
Jakmile jsme počet transformace připraveno, může uživatel hello zvolte jaké funkce tooinclude v jejich train a testovací datové sady používající hello **upravit parametry tabulky počet** modulu. Tento modul ukážeme právě tady pro úplnost, ale v zájmu jednoduchosti nepoužívejte ve skutečnosti ho v našem experimentu.

![Upravit parametry počet tabulky](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

V takovém případě jak je vidět, jsme vybrali toouse jenom hello protokolu pravděpodobnost a tooignore hello regrese sloupce. Můžeme také nastavit parametry, třeba prahová hodnota bin paměti hello, kolik tooadd částečně předchozí příklady vyhlazení, a jestli toouse žádné Laplacian noise nebo ne. Všechny tyto pokročilé funkce a je, že toobe poznamenat, že hello výchozí hodnoty jsou to dobrý výchozí bod pro uživatele, kteří jsou nový typ toothis funkce generování.

##### <a name="data-transformation-before-generating-hello-count-features"></a>Transformace dat před vygenerováním hello počet funkcí
Teď soustředit na důležité bod o transformace naše train a testovací data předchozí tooactually generování funkce count. Všimněte si, že existují dvě **spustit skript jazyka R** moduly používané před použijeme hello počet transformace tooour data.

![Spustit skript jazyka R moduly](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

Tady je skript jazyka R první hello:

![První skript jazyka R](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)

V tomto skriptu R jsme příliš přejmenovat naše toonames sloupce "Sloupec1" "Col40". Je to proto, že počet transformace hello očekává názvy tohoto formátu.

Ve skriptu R druhý hello, jsme vyvážit hello distribuční mezi kladné a záporné třídy (třídy 1 a 0) třídou převzorkování hello záporné. Hello R skript je zde ukázkou, jak toodo toto:

![Druhý skript jazyka R](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

V této jednoduché skript jazyka R používáme "pos\_záporné\_poměr" tooset hello množství rovnováhu mezi hello kladné a záporné třídy hello. To je důležité, že toodo vzhledem k tomu obvykle zlepšení nevyváženosti třída má výhody výkonu pro klasifikaci problémy kde hello třída distribuce je nesouměrně rozdělí (odvolání, že v našem případě obsahuje kladné a záporné třídu 96.7 % 3.3 %).

##### <a name="applying-hello-count-transformation-on-our-data"></a>Použití transformace počet hello na našich dat
Nakonec můžeme použít hello **použít transformaci** modulu tooapply hello počet transformace na našem train a testovací datové sady. Tento modul trvá transformace počet hello uložit jako jeden vstup a hello cvičení nebo testovací datové sady jako hello jiného vstupu a vrátí data pomocí funkce count. V tomto poli se zobrazí:

![Použít modul transformace](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-hello-count-features-look-like"></a>Výňatek jak vypadat hello počet funkcí
Je významné toosee, jaký počet funkcí hello vypadat v našem případě. Zde ukážeme výňatek tohoto:

![Počet funkcí](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

V této výňatek ze ukážeme, že pro hello sloupce, které jsme počítá na, jsme získat hello a přihlaste se pravděpodobnost relevantní backoffs tooany přidání.

Snažíme se teď připravena toobuild model Azure Machine Learning použití těchto transformovaných datových sad. V další části hello ukážeme, jak to lze provést.

### <a name="step3"></a>Krok 3: Vytvoření, školení a určení skóre modelu hello

#### <a name="choice-of-learner"></a>Volba student
Nejdřív potřebujeme toochoose student. Snažíme se probíhající toouse rozhodovací strom dvě třídy boosted jako naše student. Zde jsou hello výchozí možnosti pro tento student:

![Two-Class Boosted Decision Tree parametry](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

Pro naše experimentu jsme probíhající toochoose hello výchozí hodnoty. Jsme Všimněte si, že tento hello výchozí hodnoty jsou obvykle smysluplný a rychlé účaří dobře tooget na výkon. Můžete zlepšit výkon komínů parametry, pokud se rozhodnete tooonce, měli byste směrný plán.

#### <a name="train-hello-model"></a>Train hello model
Pro školení, můžeme jednoduše vyvolání **Train Model** modulu. Hello dva vstupy tooit jsou hello Two-Class Boosted Decision Tree Student a naší datové sadě vlaku. To je znázorněno zde:

![Modulu Train Model](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)

#### <a name="score-hello-model"></a>Určení skóre modelu hello
Jakmile jsme modulu trained model, jsme připraveni tooscore na hello testovací datové sady a tooevaluate jeho výkon. Provedeme to pomocí hello **Score Model** modulu znázorňuje následující obrázek, spolu s hello **Evaluate Model** modul:

![Modul Určení skóre modelu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)

### <a name="step4"></a>Krok 4: Vyhodnocení modelu hello
Nakonec rádi bychom znali tooanalyze modelu výkonu. Pro dva – třída (binární) klasifikaci problémy, je dobré míru obvykle hello AUC. toovisualize toho jsme spojit hello **Score Model** modulu tooan **Evaluate Model** modulu pro tento. Kliknutím na tlačítko **vizualizovat** na hello **Evaluate Model** modulu vypočítá obrázek jako hello jeden následující:

![Vyhodnocení modelu BDT modulu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

Binární soubor (nebo dvě třídy) je klasifikace problémy, dobrý míru přesnost předpovědi hello oblasti pod křivky (AUC). V následující ukážeme naše výsledků pomocí tohoto modelu na naše testovací datové sady. tooget tento, klikněte pravým tlačítkem na hello výstupní port modulu hello **Evaluate Model** modul a potom **vizualizovat**.

![Vizualizace modulu pro vyhodnocení modelu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step5"></a>Krok 5: Publikování hello model jako webovou službu
Hello možnost toopublish model Azure Machine Learning jako webové služby s minimální fuss je cenné funkce pro vytváření široce dostupné. Po dokončení, každý, kdo mohou být volání toohello webové služby s vstupní data, která potřebují předpovědi a webová služba hello používá hello model tooreturn těchto předpovědi.

toodo toho jsme nejprve uložit naše trained model jako objekt Trained Model. K tomu je potřeba pravým tlačítkem myši na hello **Train Model** modul a pomocí hello **uložit jako Trained Model** možnost.

Dále je třeba toocreate vstupní a výstupní porty pro naše webové služby:

* vstupní port vezme všechna data v hello stejné jako hello data, která potřebujeme předpovědi formuláři
* na výstupní port vrátí popisky vyhodnocení hello a pravděpodobnostech hello přidružené.

#### <a name="select-a-few-rows-of-data-for-hello-input-port"></a>Vyberte pár řádků dat pro vstupní port hello
Je vhodné toouse **použít transformaci SQL** modulu tooselect jenom 10 řádků tooserve jako hello vstupní port data. Vyberte pouze na tyto řádky dat pro naše vstupní port pomocí dotazu SQL hello znázorněno zde:

![Vstupní port dat](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Webová služba
Teď nemůžeme připraveni toorun malý experiment, kterou lze použít toopublish naše webové služby.

#### <a name="generate-input-data-for-webservice"></a>Generovat vstupní data pro webovou službu
Jako zeroth krok vzhledem k tomu, že tabulka počet hello je velký, jsme trvat pár řádků testovací data a vygenerovat výstupní data z něj pomocí funkce count. To může sloužit jako formát hello vstupní data pro naše webovou službu. To je znázorněno zde:

![Vytvoření vstupní data BDT](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

> [!NOTE]
> Pro formát hello vstupních dat, je nyní používána hello výstup hello **počet Featurizer** modulu. Jakmile to experimentovat dokončení spuštění, uložit výstup hello z hello **počet Featurizer** modulu jako datové sady. Tato datová sada se používá pro hello vstupní data v hello webovou službu.
> 
> 

#### <a name="scoring-experiment-for-publishing-webservice"></a>Vyhodnocování experimentu pro publikování webovou službu
Nejprve ukážeme, jak to vypadá. Základní struktura Hello je **Score Model** modul, který přijímá objekt naše trained model a pár řádků vstupních dat, generovaný v předchozích krocích hello pomocí hello **počet Featurizer** modulu. Používáme tooproject "Vyberte sloupců v datové sadě" hello Scored popisky a pravděpodobnostech skóre hello.

![Výběr sloupců v datové sadě](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

Všimněte si, jak hello **výběr sloupců v datové sadě** modul lze použít pro 'filtrování, data z datové sady. Ukážeme hello obsah zde:

![Filtrování s hello výběr sloupců v datové sadě modulu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

porty tooget hello blue vstup a výstup, klepněte na tlačítko **Příprava webservice** v pravé dolní části hello. Spuštění tohoto experimentu také umožňuje nám toopublish hello webové služby: klikněte na tlačítko hello **publikování webové služby** ikonu v hello dolní správné, uvedené tady:

![Publikování webové služby](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)

Po publikování hello webservice se nám získat přesměrovaného tooa stránky, která vypadá takto:

![Řídicí panel webové služby](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

Dva odkazy pro webovým službám vidíme na levé straně hello:

* Hello **požadavků a odpovědí** služby (nebo RRS) je určená pro jeden předpovědi a je co můžeme využívat tento Workshop.
* Hello **BATCH EXECUTION** služby (BES) se používá pro předpovědi batch a vyžaduje hello vstupních dat používá toomake předpovědi, které se nacházejí v úložišti objektů Blob Azure.

Kliknutím na odkaz hello **požadavků a odpovědí** trvá tooa stránky, která nám dává předem konzervované kódu v C#, python a R. Tento kód můžete pohodlně použít při výběru toohello volání webové služby. Všimněte si, že hello klíč rozhraní API na této stránce musí toobe používá k ověřování.

Je vhodné toocopy tento python code přes tooa nové buňky v poznámkovém bloku IPython hello.

Zde ukážeme segment kódu python s hello správný klíč rozhraní API.

![Kód jazyka Python](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)

Všimněte si, že klíč rozhraní API výchozí hello byl nahrazen s klíčem rozhraní API naše webovým službám. Kliknutím na tlačítko **spustit** v této buňce v IPython poznámkového bloku vypočítá hello následující odpověď:

![IPython odpovědi](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

Vidíme, že pro hello dvou testovacích příklady, které jsme požádáni o (v hello JSON architektura skript v jazyce python hello), se nám získat zpět odpovědi v podobě hello "Scored označení Scored Pravděpodobnostech". Všimněte si, že v tomto případě jsme zvolili hello výchozí hodnoty tento kód předdefinovaným hello poskytuje (0 je pro všechny číselné sloupce a hello řetězec "value" pro všechny sloupce kategorií).

Tím končí naše zobrazující začátku do konce návod jak toohandle rozsáhlé datové sady pomocí Azure Machine Learning. Můžeme začít s terabajt dat, sestavený předpovědi modelu a nasadit jako webovou službu v cloudu hello.

