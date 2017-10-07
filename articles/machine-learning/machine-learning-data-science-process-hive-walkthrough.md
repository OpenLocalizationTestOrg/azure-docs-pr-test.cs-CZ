---
title: "aaaExplore data v Hadoop clusteru a vytváření modelů v Azure Machine Learning | Microsoft Docs"
description: "Pomocí hello proces vědecké účely dat Team začátku do konce scénář nasazení HDInsight Hadoop clusteru toobuild a nasadit model použití veřejně dostupné datové sady."
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e9e76c91-d0f6-483d-bae7-2d3157b86aa0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: a371032e356ffc366af0d6fbe364af281b6efd19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a>Hello proces vědecké účely Team dat v akci: použití Azure HDInsight Hadoop clusterů
V tomto návodu použijeme hello [tým datové vědy procesu (TDSP)](data-science-process-overview.md) začátku do konce scénář pomocí [clusteru Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) toostore, prozkoumejte a veřejně funkci pracovníka data z hello k dispozici [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datovou sadu a toodown vzorová hello data. Modely hello dat jsou integrované s Azure Machine Learning toohandle binární a více třídami klasifikace a regrese prediktivní úlohy.

Návod, který ukazuje, jak toohandle větší datovou sadu (1 terabajt) o podobném scénáři pomocí HDInsight Hadoop clusterů pro zpracování dat najdete v tématu [Team Data vědecké účely proces – pomocí Azure HDInsight Hadoop clusterů v 1 TB datovou sadu](machine-learning-data-science-process-hive-criteo-walkthrough.md).

Je také možné toouse IPython poznámkového bloku tooaccomplish hello úloh vidění hello návod použití hello 1 TB datové sady. Uživatelé, kteří by jako tento postup by se měli obrátit tootry hello [Criteo návod pomocí připojení Hive ODBC](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) tématu.

## <a name="dataset"></a>Popis NYC taxíkem služebních cest datové sady
Hello NYC taxíkem cestě dat je přibližně 20GB komprimované hodnot oddělených čárkami (CSV) souborů (nekomprimovaným ~ 48GB), která obsahuje více než 173 milionů jednotlivých cest a hello tarify placené pro každou cestu. Každý záznam cestě zahrnuje hello vyzvednutí a odkládací umístění a čas, číslo licence anonymizovaná hackerský (ovladač) a číslo Medailon (taxi na jedinečné id). Hello dat obsahuje všechny služebních cest v hello roku 2013 a je součástí hello následující dvě datové sady pro každý měsíc:

1. Hello 'trip_data' CSV soubory obsahují podrobnosti o cestě, například na počtu cestujících, vyzvednutí a dropoff body, doba trvání cesty a délka cesty. Tady je několik ukázkových záznamů:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. Hello 'trip_fare' CSV soubory obsahují podrobnosti o tarif hello placené pro každou cestu, například typ platby, velikost tarif, příplatek a daně, tipy a mýtné a celkovou velikost hello placené. Tady je několik ukázkových záznamů:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Hello jedinečné klíče toojoin cestě\_dat a cesty\_tarif se skládá z pole hello: medailonu, hackerský\_licence a vyzvednutí\_data a času.

všechny tooget hello podrobnosti relevantní tooa konkrétní cesty, je dostatečná toojoin tři klíče: hello "medailonu", "zabezpečení\_licence" a "vyzvednutí\_data a času".

Jsme popisují některé další podrobné informace o datech hello, když jsme jejich uložení do tabulek Hive za chvíli.

## <a name="mltasks"></a>Příklady předpovědi úlohy
Když se blíží dat, určení hello druh předpovědi, že chcete toomake na základě jeho analýzy pomáhá vysvětlení hello úlohy, budete potřebovat tooinclude v procesu.
Tady jsou tři příklady předpovědi problémy, které jsme adresy v tomto návodu, jejichž formulování je založena na hello *tip\_velikost*:

1. **Binární klasifikace**: předpovědět, zda byl tip placené cesty, tj. *tip\_velikost* větší než $0 je pozitivní příklad, při *tip\_velikost* $ 0 je záporný příklad.
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0
2. **Více třídami klasifikace**: rozsah hello toopredict tip objemu zaplacení hello cesty. Jsme rozdělit hello *tip\_velikost* do pěti přihrádek nebo třídy:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. **Úloha regrese**: toopredict hello množství hello tip placené cesty.  

## <a name="setup"></a>Nastavení clusteru HDInsight Hadoop pro pokročilou analýzu
> [!NOTE]
> Obvykle se jedná **správce** úloh.
> 
> 

Můžete nastavit prostředí Azure pro pokročilou analýzu využívajícího clusteru služby HDInsight v tři kroky:

1. [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md): Tento účet úložiště se používá pro ukládání dat v Azure Blob Storage. Hello data použitá v clusterech HDInsight se také nachází zde.
2. [Přizpůsobit Azure HDInsight Hadoop clusterů pro hello pokročilé analýzy proces a technologie](machine-learning-data-science-customize-hadoop-cluster.md). Tento krok vytvoří cluster s 64-bit Anaconda Python 2.7 nainstalované na všech uzlech služby Azure HDInsight Hadoop. Existují dvě tooremember důležité kroky při přizpůsobení clusteru HDInsight.
   
   * Mějte na paměti, toolink hello úložiště účet vytvořený v kroku 1 k vašemu clusteru HDInsight při jejím vytváření. Tento účet úložiště je použité tooaccess data, která je zpracovat v rámci clusteru hello.
   * Po vytvoření clusteru hello povolte vzdálený přístup toohello hlavního uzlu clusteru hello. Přejděte toohello **konfigurace** a klikněte na **povolit vzdálené**. Tento krok určuje hello uživatelská pověření použitá pro vzdálené přihlášení.
3. [Vytvořit pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md): Tento Azure Machine Learning pracovního prostoru není modely použité toobuild strojové učení. Tato úloha je určeno po dokončení počáteční data zkoumání a dolů vzorkování pomocí hello clusteru HDInsight.

## <a name="getdata"></a>Získat hello data z veřejné zdroje
> [!NOTE]
> Obvykle se jedná **správce** úloh.
> 
> 

tooget hello [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datové sady z veřejného umístění, můžete použít některou z metod hello popsané v [tooand přesun dat z Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello data tooyour počítače.

Zde jsme popisují, jak pomocí nástroje AzCopy tootransfer hello soubory obsahující data. toodownload a nainstalujte AzCopy postupujte podle pokynů hello v [Začínáme s hello příkazového řádku Azcopy](../storage/common/storage-use-azcopy.md).

1. Z okna příkazového řádku, vydávání hello následující příkazy AzCopy, nahraďte *< path_to_data_folder >* s cílem požadované hello:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. Po dokončení kopírování hello celkem 24 komprimované soubory jsou ve složce data hello vybrali. Rozbalte hello stažené soubory toohello stejný adresář na místním počítači. Poznamenejte si hello složky, kde jsou uloženy soubory hello nekomprimovaným. Tato složka bude hello odkazované tooas *< cesta\_k\_unzipped_data\_soubory\>*  je následující.

## <a name="upload"></a>Nahrát hello toohello výchozí kontejner dat clusteru Azure HDInsight Hadoop
> [!NOTE]
> Obvykle se jedná **správce** úloh.
> 
> 

Následující příkazy AzCopy, nahraďte v hello hello následující parametry s hello skutečné hodnoty, které jste zadali při vytváření clusteru Hadoop hello a rozbalení hello datových souborů.

* ***& č. 60; path_to_data_folder >*** directory hello (spolu s cesta) na počítači, které obsahují hello rozbalené datových souborů  
* ***& č. 60; název účtu úložiště clusteru Hadoop >*** hello účtu úložiště přidruženého k vašemu clusteru HDInsight
* ***& č. 60; výchozí kontejner clusteru Hadoop >*** výchozí kontejner hello používá váš cluster. Všimněte si, že název hello hello výchozího kontejneru je obvykle hello stejný název jako samotný cluster hello. Například pokud hello clusteru se nazývá "abc123.azurehdinsight.net", je výchozí kontejner hello abc123.
* ***& č. 60; klíč účtu úložiště >*** hello klíč pro účet úložiště hello používá váš cluster

Z příkazového řádku nebo okno prostředí Windows PowerShell v počítači spusťte následující dva příkazy AzCopy hello.

Tento příkaz odešle data cestě hello příliš***nyctaxitripraw*** adresář v kontejneru výchozí hello clusteru Hadoop hello.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Tento příkaz odešle datový tarif hello příliš***nyctaxifareraw*** adresář v kontejneru výchozí hello clusteru Hadoop hello.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Hello data by teď v Azure Blob Storage a připravena toobe využívat v rámci clusteru HDInsight hello.

## <a name="#download-hql-files"></a>Přihlaste se k hlavnímu uzlu clusteru Hadoop hello a a připravit pro analýzu nahodilého dat
> [!NOTE]
> Obvykle se jedná **správce** úloh.
> 
> 

tooaccess hello hlavního uzlu clusteru hello pro analýzu nahodilého dat a dolů vzorkování hello dat, postupujte podle uvedených v postupu hello [přístup hello uzlu clusteru Hadoop Head](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

V tomto návodu budeme používat hlavně dotazy, které jsou napsané v [Hive](https://hive.apache.org/), jako SQL dotazovací jazyk, explorations tooperform předběžné data. dotazy Hive Hello jsou uložené v souborech .hql. Jsme pak dolů ukázkové toobe tato data použít Azure Machine Learning pro vytváření modelů.

tooprepare hello clusteru pro analýzu dat nahodilého, jsme stáhnout soubory .hql hello obsahující hello relevantní Hive skripty z [githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) tooa místního adresáře (C:\temp) hello hlavního uzlu. toodo se otevřete hello **příkazového řádku** uvnitř hello hlavního uzlu v clusteru a problém hello hello následující dva příkazy:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Tyto dva příkazy stáhne všechny soubory .hql potřebné v tomto návodu toohello místním adresáři ***C:\temp &#92;*** v hello hlavního uzlu.

## <a name="#hive-db-tables"></a>Vytvoření databáze Hive a tabulky rozdělena na oddíly po měsících
> [!NOTE]
> Obvykle se jedná **správce** úloh.
> 
> 

Snažíme se teď tabulek Hive připraven toocreate pro naší datové sadě taxíkem NYC.
V hello hlavního uzlu v clusteru Hadoop hello, otevřete hello ***Hadoop příkazového řádku*** hello plochy hello hlavního uzlu a zadejte adresář Hive hello zadáním příkazu hello

    cd %hive_home%\bin

> [!NOTE]
> **Spusťte všechny příkazy Hive v tomto návodu z hello výše Hive bin / directory řádku. To se postará o všech problémech, cesta automaticky. Používáme podmínky hello "Hive directory výzva", "Hive bin / directory řádku" a "Hadoop příkazový řádek" zcela zaměnitelným významem v tomto návodu.**
> 
> 

Hello Hive directory řádku zadejte následující příkaz v Hadoop příkazového řádku hello hlavního uzlu toosubmit hello Hive dotaz toocreate Hive databáze a tabulky hello:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Tady je hello obsah hello ***C:\temp\sample\_hive\_vytvořit\_db\_a\_tables.hql*** souboru, který vytvoří databázi Hive ***nyctaxidb *** a tabulky ***cestě*** a ***tarif***.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Tento skript Hive vytvoří dvě tabulky:

* Hello "cesty" tabulka obsahuje podrobnosti o cestě z každé pravé (podrobnosti o ovladači, výstupní čas, vzdálenost cesty a časy)
* Hello "tarif" tabulka obsahuje podrobnosti o tarif (tarif částka tip, mýtné a příplatky).

Potřebujete-li žádné další pomoc s tyto postupy nebo chcete tooinvestigate alternativní těch, najdete v tématu hello [dotazů odeslání Hive přímo z hello Hadoop příkazového řádku ](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="#load-data"></a>Načíst Data tabulky tooHive podle oddílů
> [!NOTE]
> Obvykle se jedná **správce** úloh.
> 
> 

Datová sada taxíkem Hello NYC má přirozené dělení po měsících, které používáme tooenable zpracování a dotazů rychlejší. Hello dole uvedené příkazy Powershellu (vystavené adresář hello Hive pomocí hello **Hadoop příkazového řádku**) načíst data toohello "cesty" a "tarif" tabulek Hive rozdělena na oddíly po měsících.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

Hello *ukázka\_hive\_načíst\_data\_podle\_partitions.hql* soubor obsahuje následující hello **NAČÍST** příkazy.

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Všimněte si, že počet dotazů Hive, kterou tady používáme v procesu zkoumání hello zahrnují vyhledávání na právě jeden oddíl nebo na pouze několik oddílů. Ale tyto dotazy by bylo možné spustit v rámci celého datového hello.

### <a name="#show-db"></a>Zobrazit databází v hello clusteru HDInsight Hadoop
tooshow hello databáze vytvořené v clusteru HDInsight Hadoop v okně hello Hadoop příkazového řádku spusťte následující příkaz v Hadoop příkazového řádku hello:

    hive -e "show databases;"

### <a name="#show-tables"></a>Zobrazení tabulek Hive hello v databázi nyctaxidb hello
tooshow hello tabulky v databázi nyctaxidb hello, spusťte následující příkaz v Hadoop příkazového řádku hello:

    hive -e "show tables in nyctaxidb;"

Může potvrdit, že hello tabulky jsou rozdělena na oddíly vydáním hello následující příkaz:

    hive -e "show partitions nyctaxidb.trip;"

Hello očekává, že výstupní jsou uvedeny níže:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Podobně jsme můžete zajistit, že tento hello tarif tabulka je rozdělena na oddíly vydáním níže uvedeného příkazu hello:

    hive -e "show partitions nyctaxidb.fare;"

Hello očekává, že výstupní jsou uvedeny níže:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Zkoumání dat a funkce analýzy v Hive
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Hello zkoumání dat a funkce engineering úlohy pro hello data načtená do tabulek Hive hello můžete udělat pomocí dotazů Hive. Zde jsou příklady takových úloh, že jsme vás provede procesem v této části:

* Zobrazte hello top 10 záznamů v obou tabulkách.
* Prozkoumejte data distribuce několik polí v různých časových oken.
* Prozkoumejte data quality hello zeměpisné šířky a délky polí.
* Generovat binární a více třídami klasifikační štítky podle hello **tip\_velikost**.
* Vygenerujte funkce computing hello přímé cestě vzdálenosti.

### <a name="exploration-view-hello-top-10-records-in-table-trip"></a>Zkoumání: Zobrazení hello top 10 záznamy v cestě tabulky
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

jaká data hello vypadá jako toosee, jsme zkontrolujte 10 záznamy ze všech tabulek. Spusťte hello samostatně následující dva dotazy z řádku directory hello Hive v hello Hadoop příkazového řádku konzoly tooinspect hello záznamy.

tooget hello top 10 záznamy v tabulce hello "cesty" od hello první měsíc:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

tooget hello top 10 záznamy v tabulce hello "tarif" z hello první měsíc:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Je často užitečné toosave hello záznamy tooa soubor pro pohodlný zobrazení. Malé změny toohello výše dotazu toho dosahuje:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-hello-number-of-records-in-each-of-hello-12-partitions"></a>Zkoumání: Zobrazit hello počet záznamů v každé hello 12 oddílů
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Zájmu je hello, jak se liší hello počet cest hello kalendářním roce. Seskupení podle měsíce umožňuje toosee této distribuce služebních cest, které bude vypadat takto.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

To nám dává výstup hello:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Zde hello první sloupec je hello měsíc a hello druhou je hello počet cest pro daný měsíc.

Také jsme můžete počet hello celkový počet záznamů v naší datové sadě cestě vydáním hello následující příkazového řádku adresář hello Hive.

    hive -e "select count(*) from nyctaxidb.trip;"

Dostaneme:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Pomocí příkazů podobné toothose pro hello cestě datové sady, jsme vydávat dotazy Hive z hello Hive directory řádku pro hello tarif datové sady toovalidate hello počet záznamů.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

To nám dává výstup hello:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Všimněte si, že pro obě sady dat se vrátí hello přesně stejný počet služebních cest za měsíc. To poskytuje hello první ověření, že hello byla data načtena správně.

Počítání hello celkový počet záznamů v sadě dat tarif hello lze provést pomocí příkazu hello níže hello Hive directory řádku:

    hive -e "select count(*) from nyctaxidb.fare;"

Dostaneme:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Celkový počet záznamů v obou tabulkách Hello je také hello stejné. To poskytuje druhý ověření, že hello byla data načtena správně.

### <a name="exploration-trip-distribution-by-medallion"></a>Zkoumání: Cestě distribuce podle Medailon
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Tento příklad identifikuje Medailon hello (taxi čísla) s více než 100 služebních cest v daném časovém období. výhody Hello dotazu z hello oddíly přístup k tabulce vzhledem k tomu, že je podmíněno proměnnou oddílu hello **měsíc**. Hello výsledky dotazu jsou zapsány tooa místního souboru queryoutput.tsv v `C:\temp` hello hlavního uzlu.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Tady je obsah hello *ukázka\_hive\_cestě\_počet\_podle\_medallion.hql* souboru kontroly.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Hello Medailon v sadě dat taxíkem NYC hello identifikuje jedinečný soubor cab. Abychom mohli identifikovat, které soubory CAB jsou "zaneprázdněný" žádostí, které z nich větší než počet cest provedené v konkrétním časovém období. Hello následující příklad určuje soubory CAB, které provádí víc než sto cest v hello první tři měsíce a uloží hello dotazu výsledky tooa místního souboru C:\temp\queryoutput.tsv.

Tady je obsah hello *ukázka\_hive\_cestě\_počet\_podle\_medallion.hql* souboru kontroly.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Z hello podregistru directory řádku následující příkaz hello problému:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Zkoumání: Cestě distribuce podle Medailon a hack_license
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Při prohlížení datové sady, chceme často tooexamine hello počet výskytů co skupin hodnot. Tato část uveďte příklad, jak toodo to pro soubory CAB a ovladače.

Hello *ukázka\_hive\_cestě\_počet\_podle\_Medailon\_license.hql* skupin souborů datový tarif hello nastavena na "Medailon" a "hack_license" a vrátí počet každé kombinaci. Níže jsou jeho obsah.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Tento dotaz vrací určitý ovladač kombinací seřazené podle sestupné počet cest a souborů cab.

Z hello Hive directory řádku, spusťte:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

výsledky dotazu Hello se zapisují tooa místního souboru C:\temp\queryoutput.tsv.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Zkoumání: Vyhodnocování kvality dat. kontrolou neplatné délky nebo zeměpisnou šířku záznamů
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Běžné cíl analýzy nahodilého dat je tooweed se neplatný nebo chybných záznamů. Hello příklad v této části určuje, zda buď hello zeměpisné šířky nebo délky pole obsahovat hodnotu daleko mimo hello NYC oblasti. Vzhledem k tomu, že je pravděpodobné, že tyto záznamy mají hodnoty chybné zeměpisné šířky, chceme tooeliminate je z všechna data, která je toobe používá pro modelování.

Tady je obsah hello *ukázka\_hive\_kvality\_assessment.hql* souboru kontroly.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Z hello Hive directory řádku, spusťte:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

Hello *-S* argument zahrnuté v tomto příkazu potlačí hello stav obrazovky výtisk úloh Hive mapy nebo snižte hello. To je užitečné, protože umožňuje hello obrazovky tisk hello Hive dotaz výstup čitelnější.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Zkoumání: Binární třída distribuce cestě tipy
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Pro problém binární klasifikace hello uvedených v hello [příklady úloh předpovědi](machine-learning-data-science-process-hive-walkthrough.md#mltasks) části, je užitečné tooknow, zda byl zadán tip, nebo ne. Toto rozdělení tipy je binární:

* Zadaný Tip (třída 1, tip\_částka > $0)  
* žádné tip (třída 0, tip\_velikost = 0).

Hello *ukázka\_hive\_vysypávány\_frequencies.hql* tomu souboru vidíte níže.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Z hello Hive directory řádku, spusťte:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-hello-multiclass-setting"></a>Zkoumání: Třída distribuce v nastavení více třídami hello
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Pro problém více třídami klasifikace hello uvedených v hello [příklady úloh předpovědi](machine-learning-data-science-process-hive-walkthrough.md#mltasks) tato datová sada také poskytuje vlastní tooa přirozené klasifikace, kde rádi bychom znali toopredict hello množství hello tipy pro zadaný oddíl. Můžeme použít přihrádek toodefine tip rozsahy v dotazu hello. tooget hello třída distribuce pro hello různých rozsahů tip, používáme hello *ukázka\_hive\_tip\_rozsah\_frequencies.hql* souboru. Níže jsou jeho obsah.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Spusťte následující příkaz z příkazového řádku Hadoop konzoly hello:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Zkoumání: Výpočetní přímé vzdálenost mezi dvěma umístěními zeměpisné šířky
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Míru přímé vzdálenost hello umožní nám toofind out hello nesoulad mezi ním hello skutečné dojít vzdálenost. Tato funkce jsme stimulují tak, že odkazuje osobní může být menší pravděpodobně tootip, pokud jejich rozmyslete si, že ovladač hello má záměrně provedenou je mnohem delší trasu.

porovnání hello toosee vzdálenost skutečné cesty a hello [Haversine vzdálenost](http://en.wikipedia.org/wiki/Haversine_formula) mezi dvěma body zeměpisné šířky (vzdálenost hello "dobře kruh"), používáme hello trigonometrické funkce dostupné v rámci Hive, tedy:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

V dotazu hello výše R je hello úhlu hello země v miles a platformy je převeden tooradians. Upozorňujeme, že jsou body zeměpisné šířky hello "filtrované" tooremove hodnoty, které jsou daleko od oblasti NYC hello.

V takovém případě zápisu jsme naše výsledky tooa adresář s názvem "queryoutputdir". Hello sekvence příkazů ukazuje následující obrázek nejprve vytvoří tento výstup adresář a potom spustí příkaz Hive hello.

Z hello Hive directory řádku, spusťte:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Hello výsledky dotazu jsou zapsány objektů BLOB too9 Azure ***queryoutputdir/000000\_0*** příliš ***queryoutputdir/000008\_0*** v kontejneru výchozí hello clusteru Hadoop hello.

velikost hello toosee hello jednotlivé objekty BLOB, spustíme hello hello Hive directory řádku následující příkaz:

    hdfs dfs -ls wasb:///queryoutputdir

toosee hello obsah daného souboru vyslovení 000000\_0, používáme na Hadoop `copyToLocal` příkaz proto.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> `copyToLocal`může být velmi pomalé pro velké soubory a nedoporučuje se používat pro použití s nimi.  
> 
> 

Hlavní výhodou, že máte tato data jsou umístěny v objektu blob Azure je, že jsme může zkoumat data hello v rámci Azure Machine Learning pomocí hello [importovat Data] [ import-data] modulu.

## <a name="#downsample"></a>Dolů modely ukázkových dat a sestavení v Azure Machine Learning
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Po hello nahodilého dat analytické fáze jsou jsme teď připravena toodown ukázková hello data pro vytváření modelů v Azure Machine Learning. V této části ukážeme, jak toouse podregistru dotaz toodown ukázka hello data, která je potom k němu přistupovat z hello [importovat Data] [ import-data] modulu v Azure Machine Learning.

### <a name="down-sampling-hello-data"></a>Dolů vzorkování dat hello
Existují dva kroky v tomto postupu. Nejdřív jsme připojit hello **nyctaxidb.trip** a **nyctaxidb.fare** tabulky na tři klíče, které jsou k dispozici ve všech záznamech: "medailonu", "zabezpečení\_licence", a "vyzvednutí\_data a času". Pak se vygeneruje štítek binární klasifikace **vysypávány** a klasifikaci s více třída popisek **tip\_– třída**.

hello možné toouse toobe dolů jen Vzorkovaná data přímo z hello [importovat Data] [ import-data] modulu v Azure Machine Learning, je nutné toostore hello výsledky hello výše dotazu tooan interní tabulku Hive. V co následuje můžeme vytvořit vnitřní tabulku Hive a naplnit její obsah se hello připojený a dolů jen Vzorkovaná data.

Hello dotazu použije standardní funkce Hive přímo hodinu hello toogenerate den, týden roku, den v týdnu (1 zastupuje pondělí a 7 zastupuje neděle) z hello "vyzvednutí\_data a času" pole a hello přímé vzdálenost mezi hello vyzvednutí a dropoff umístění. Uživatelé mohou odkazovat příliš[LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) pro úplný seznam takové funkce.

Hello dotazu pak dolů ukázky hello dat tak, aby výsledky dotazu hello můžete začlenit do hello Azure Machine Learning Studio. Pouze asi 1 % hello původní datové sady je importovat do hello Studio.

Níže jsou hello obsah *ukázka\_hive\_Příprava\_pro\_aml\_full.hql* soubor, který připraví dat pro model vytváření v Azure Machine Learning.

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of hello join into hello above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

toorun vyzvat tento dotaz z adresáře Hive hello:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Nyní je interní tabulku "nyctaxidb.nyctaxi_downsampled_dataset", který je přístupný pomocí hello [importovat Data] [ import-data] modulu z Azure Machine Learning. Kromě toho můžeme použít tuto datovou sadu pro vytváření modelů Machine Learning.  

### <a name="use-hello-import-data-module-in-azure-machine-learning-tooaccess-hello-down-sampled-data"></a>Používejte v Azure Machine Learning tooaccess hello dolů jen Vzorkovaná data modul importovat Data hello
Jako předpoklady pro vydávání dotazů Hive v hello [importovat Data] [ import-data] modul Azure Machine Learning, budeme potřebovat přístup k tooan Azure Machine Learning prostoru a k toohello pověření hello cluster a jeho přidružený účet úložiště.

Některé podrobnosti o hello [importovat Data] [ import-data] modul a hello tooinput parametry:

**Identifikátor URI serveru HCatalog**: Pokud je název clusteru hello abc123, pak toto je jednoduše: https://abc123.azurehdinsight.net

**Název uživatelského účtu Hadoop** : zvolené pro hello cluster hello uživatelské jméno (**není** hello RAS uživatelské jméno)

**Heslo účtu pož Hadoop** : heslo hello zvolené pro hello cluster (**není** heslo hello vzdáleného přístupu)

**Umístění výstupu dat** : to je zvolen toobe Azure.

**Název účtu úložiště Azure** : název účtu úložiště výchozí hello přidruženého k hello clusteru.

**Název kontejneru Azure** : Toto je název kontejneru výchozí hello hello clusteru a je obvykle hello stejný jako název clusteru hello. Pro cluster s názvem "abc123" jde jenom abc123.

> [!IMPORTANT]
> **Všechny tabulky jsme chcete tooquery pomocí hello [importovat Data] [ import-data] modulu v Azure Machine Learning musí být interní tabulku.** Tip pro určení toho, jestli tabulka T v databázi D.db interní tabulku je následující.
> 
> 

Z hello podregistru directory řádku problém hello příkaz:

    hdfs dfs -ls wasb:///D.db/T

Pokud tabulka hello je interní tabulku a je naplněna, musí zde zobrazí její obsah. Jiný způsob toodetermine, zda tabulka je interní tabulku je toouse hello Azure Storage Explorer. Pomocí něho toonavigate toohello výchozí název kontejneru hello clusteru a potom filtrovat podle názvu tabulky hello. Pokud hello tabulka a její obsah objeví, potvrdíte, že je interní tabulku.

Zde je snímek hello dotaz Hive a hello [importovat Data] [ import-data] modul:

![Dotaz Hive pro Import dat modul](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

Poznámka: od našich seznamu jen Vzorkovaná data nachází v kontejneru výchozí hello hello výsledný dotaz Hive z Azure Machine Learning je velmi jednoduchý a je jen "vybrat * z nyctaxidb.nyctaxi\_po převzorkování dolů\_data".

datovou sadu Hello nyní slouží jako výchozí bod pro vytváření modelů Machine Learning hello.

### <a name="mlmodel"></a>Vytvářet modely v Azure Machine Learning
Snažíme se teď může tooproceed toomodel sestavení a nasazení modelu v [Azure Machine Learning](https://studio.azureml.net). Hello data jsou připravena pro nás toouse v adresování identifikované výše hello předpovědi problémy:

**1. Binární klasifikace**: toopredict, jestli byl tip placené cesty.

**Použít student:** logistic regression Two-class

a. U tohoto problému našeho štítku cíl (nebo třída) je "vysypávány". Naše původní datové sady vzorků nižší má několik sloupců, které jsou nevracení cíl tohoto experimentu klasifikace. Konkrétně: tip\_třídy, tip\_velikostí a celkový počet\_částka zobrazení informace o hello cíl štítek, který není k dispozici na testování čas. Jsme odebrat tyto sloupce brány v úvahu pomocí hello [výběr sloupců v datové sadě] [ select-columns] modulu.

Hello snímku níže ukazuje naše toopredict experiment, jestli byl placené tip pro danou cestu.

![Experiment snímku](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

b. Pro tento experiment naše distribuce popisek cíl byly přibližně 1:1.

Hello snímku níže ukazuje hello distribuce popisky třída tip pro problém binární klasifikace hello.

![Distribuce popisků tip – třída](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

V důsledku toho jsme získat AUC 0.987, jak je znázorněno na následujícím obrázku hello.

![Hodnota AUC](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

**2. Více třídami klasifikace**: rozsah hello toopredict tip objemu zaplacení hello na cestu, pomocí hello dříve definované třídy.

**Použít student:** více třídami logistic regression

a. Pro tento problém popisek naše cíle (nebo třída) je "tip\_třída" které můžete provést jednu z pěti hodnot (0,1,2,3,4). Jako v případě binární klasifikace hello máme několik sloupců, které jsou nevracení cíl tohoto experimentu. Konkrétně: vysypávány, tip\_velikost, celkový počet\_částka zobrazení informace o hello cíl štítek, který není k dispozici na testování čas. Jsme odebrat tyto sloupce pomocí hello [výběr sloupců v datové sadě] [ select-columns] modulu.

snímek Hello níže znázorňuje naše toopredict experiment, ve které bin tip je pravděpodobně toofall (třída 0: tip = $0, 1 – třída: tip > $0 a tip < = 5, 2 – třída: tip > $5 a tip < = 10, třídy 3: tip > $10 a tip < = 20 Třída 4: tip > 20)

![Experiment snímku](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

Nyní ukážeme, jak vypadá naše skutečné testovací třída distribuční. Vidíte, že třída 0 a 1 třídy jsou běžně se vyskytujícím, hello ostatní třídy vyskytují jen vzácně.

![Testování distribuční – třída](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

b. Pro tento experiment používáme záměně matice toolook v našem přesnostmi předpovědi. Příklad najdete níž.

![Matice nedorozuměním](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

Všimněte si, že naše třída přesnostmi na třídách běžně se vyskytujícím hello je poměrně vhodný, hello modelu neprovádí dobrou práci "vzdělávání" na hello vzácnějších třídy.

**3. Úloha regrese**: toopredict hello množství tip placené cesty.

**Použít student:** Boosted rozhodovací strom

a. Pro tento problém popisek naše cíle (nebo třída) je "tip\_velikost". Naše nevracení cíl v tomto případě jsou: vysypávány, tip\_třída, celkový počet\_velikost; tyto proměnné odhalit informace o hello tip množství, které je obvykle dostupná na testování čas. Jsme odebrat tyto sloupce pomocí hello [výběr sloupců v datové sadě] [ select-columns] modulu.

Hello snímku belows ukazuje naše experimentu toopredict hello množství hello zadaný tip pro.

![Experiment snímku](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

b. V případě problémů regrese měřit hello přesnostmi naše předpovědí prohlížením hello spolehlivosti došlo k chybě v hello předpovědi, hello koeficient spolehlivosti a hello jako. Ukážeme tyto níže.

![Předpověď statistiky](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

Vidíme, že o hello je koeficient spolehlivosti 0.709, zdání přibližně 71 % odchylky hello se vysvětluje naše koeficienty modelu.

> [!IMPORTANT]
> Další informace o Azure Machine Learning toolearn a jak tooaccess a používat ji, vyhledejte příliš[co je Machine Learning?](machine-learning-what-is-machine-learning.md). Je velmi užitečná prostředku pro přehrávání s spoustu experimenty Machine Learning v Azure Machine Learning hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/). Galerie Hello vysvětluje škálu experimenty a poskytuje důkladné Úvod do hello řadu funkcí služby Azure Machine Learning.
> 
> 

## <a name="license-information"></a>Informace o licenci
Tento ukázkový postup a jeho doprovodné skriptů jsou sdíleny společností Microsoft v rámci licencí MIT hello. Zkontrolujte prosím soubor LICENSE.txt hello v adresáři hello hello ukázkový kód na Githubu další podrobnosti.

## <a name="references"></a>Odkazy
• [Stránce pro stažení Andrés Monroy NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC Taxi cestě dat podle Whong Jan](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC taxíkem a Limousine Komise výzkum a statistiky](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
