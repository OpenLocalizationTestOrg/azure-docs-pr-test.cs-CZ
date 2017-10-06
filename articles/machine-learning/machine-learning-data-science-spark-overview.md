---
title: "aaaOverview vědecké zpracování dat pomocí Spark v Azure HDInsight | Microsoft Docs"
description: "Sada nástrojů Spark MLlib Hello přináší modelování prostředí HDInsight možnosti toohello distribuován významnou strojové učení."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: a4e1de99-a554-4240-9647-2c6d669593c8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: 515705684a46917c2741bf063d439b1cda016abb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Přehled vědecké zpracování dat pomocí Spark v Azure HDInsight
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Tato sada témata ukazuje, jak toouse HDInsight Spark toocomplete běžné vědecké zpracování dat úlohy, jako je například přijímání dat, funkce analýzy, modelování a vyhodnocení modelu. použít data Hello je ukázka hello 2013 NYC taxíkem služební cestě a tarif datovou sadu. modely Hello vytvářeny zahrnují logistic a lineární regrese, náhodné doménové struktury a přechodu boosted stromy. Hello témata také ukazují, jak toostore tyto modely v Azure blob storage (WASB) a jak tooscore a vyhodnotit jejich prediktivní výkonu. Pokročilejší témata zahrnují, jak může být modely Trénink pomocí sweeping křížové ověření a technologie hyper parametr. Toto přehledové téma odkazuje také hello témata, které popisují, jak tooset až hello clusteru Spark, které potřebujete toocomplete hello kroky v zadané návody hello. 

## <a name="spark-and-mllib"></a>Spark a MLlib
[Spark](http://spark.apache.org/) představuje rozhraní open-source paralelní zpracování, které podporuje v paměti zpracovává tooboost hello výkonu velkých objemů dat analytických aplikací. modul zpracování Spark Hello je vytvořené pro rychlost, snadné použití a sofistikované analytics. Možnosti v paměti distribuované výpočtů Spark díky správnou volbu pro iterativní algoritmy hello používá v machine learning a grafů výpočty. [MLlib](http://spark.apache.org/mllib/) je modelování Spark škálovatelné machine learning knihovny, která přináší hello algoritmické funkce toothis distribuovaném prostředí. 

## <a name="hdinsight-spark"></a>Spark v HDInsight
[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) hello Azure hostovaný nabídky Spark open source. Zahrnuje taky podporu **poznámkové bloky Jupyter PySpark** na clusteru Spark hello, která se může spustit interaktivních dotazů Spark SQL pro transformaci, filtrování a vizualizace dat uložených v Azure BLOB (WASB). PySpark je hello Python rozhraní API pro Spark. fragmenty kódu Hello, které poskytují hello řešení a zobrazit hello relevantní zobrazuje data hello toovisualize zde spustit v poznámkové bloky Jupyter nainstalovat hello clustery Spark. Hello modelování kroky v těchto tématech obsahovat kód, který ukazuje, jak tootrain, vyhodnotit, ukládat a používat každý typ modelu. 

## <a name="setup-spark-clusters-and-jupyter-notebooks"></a>Instalační program: Clustery Spark a poznámkové bloky Jupyter
Kroky instalace a kódu jsou uvedené v tomto názorném postupu pro používání HDInsight Spark 1.6. Ale poznámkové bloky Jupyter jsou k dispozici pro clustery HDInsight Spark 1.6 a Spark 2.0. Popis toothem hello poznámkových bloků a odkazy jsou uvedeny v hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) hello Githubu úložiště, které je obsahují. Kromě toho hello kód sem a v poznámkových bloků hello propojené je obecný a by měla fungovat v jakémkoliv clusteru Spark. Pokud nepoužíváte HDInsight Spark, může být kroky instalace a správy clusteru hello mírně lišit od co se zobrazí tady. Pro usnadnění práce uvádíme hello odkazy toohello poznámkové bloky Jupyter pro Spark 1.6 (toobe spustit v jádra pySpark hello Dobrý den Poznámkový blok Jupyter serveru) a Spark 2.0 (toobe spustit v hello pySpark3 jádra systému hello Poznámkový blok Jupyter serveru):

### <a name="spark-16-notebooks"></a>Spark 1.6 poznámkových bloků
Tyto poznámkových bloků jsou toobe spustit v jádra pySpark hello serveru poznámkového bloku Jupyter.

- [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): obsahuje informace o tooperform zkoumání dat, modelování a vyhodnocování se několik různých algoritmů.
- [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): obsahuje témata v poznámkovém bloku #1 a modelu vývoj pomocí hyperparameter ladění a křížové ověření.
- [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb): ukazuje, jak toooperationalize uložené model používá Python v HDInsight clustery.

### <a name="spark-20-notebooks"></a>Spark 2.0 poznámkových bloků
Tyto poznámkových bloků jsou toobe spustit v hello pySpark3 jádra serveru poznámkového bloku Jupyter.

- [Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-Exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Tento soubor obsahuje informace o jak tooperform zkoumání dat, modelování a vyhodnocování v rámci Spark 2.0 clusterů pomocí hello NYC taxíkem cesty tarif popsané na sadu dat a [zde](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data). Tento poznámkový blok, může být to dobrý výchozí bod pro zkoumání rychle hello kódu, které uvádíme Spark 2.0. Podrobnější Poznámkový blok analyzuje hello NYC taxíkem data, najdete v části Další poznámkového bloku hello v tomto seznamu. Naleznete v poznámkách hello následující tohoto seznamu, které porovnávají tyto poznámkových bloků. 
- [Spark2.0 pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): Tento soubor ukazuje, jak tooperform data wrangling (Spark SQL a dataframe operations), zkoumání, modelování a vyhodnocování pomocí hello NYC taxíkem služební cestě a tarif sady dat popsané [ Zde](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).
- [Spark2.0 pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): Tento soubor ukazuje, jak tooperform data wrangling (Spark SQL a dataframe operations), zkoumání, modelování a vyhodnocování pomocí hello dobře známé letecká společnost na čas odeslání datové sady z 2011 a 2012. Datová sada letecká společnost hello jsme integrované s hello letiště počasí data (např. větru, teploty, výška atd.) toomodeling předchozí, tak tyto funkce počasí můžou být součástí modelu hello.

<!-- -->

> [!NOTE]
> Datová sada letecká společnost Hello byl přidán poznámkových bloků toohello Spark 2.0 toobetter ilustrují použití hello algoritmů klasifikace. Viz následující odkazy na informace o datové sady na čas odeslání letecká společnost a datovou sadu počasí hello:

>- Letecká společnost na čas odeslání dat: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)

>- Data o počasí letiště: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/) 
> 
> 

<!-- -->

<!-- -->

> [!NOTE]
Hello poznámkových bloků Spark 2.0 na hello NYC taxíkem a letecká společnost letu zpoždění-sady dat může trvat 10 minut nebo další toorun (v závislosti na velikosti hello clusteru HDI). Hello první poznámkového bloku v hello výše seznamu zobrazí mnoho aspektů hello zkoumání dat, vizualizace a ML školení v poznámkovém bloku, která přebírá méně času toorun s nižší vzorkovat NYC datové sady, ve které hello taxíkem a tarif soubory byly předem připojený k modelu: [ Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-Exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) trvá tento poznámkový blok mnohem kratší dobu toofinish (v minutách 2-3) a může být dobrou výchozí bod pro zkoumání rychle hello kód máme k dispozici pro Spark 2.0. 

<!-- -->

Pokyny v operationalization hello Spark 2.0 model a model spotřeby pro vyhodnocování najdete v tématu hello [Spark 1.6 dokumentu o spotřebě](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) příklad osnovy hello kroky. toouse to na Spark 2.0, nahraďte soubor kód Python hello s [tento soubor](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).

### <a name="prerequisites"></a>Požadavky
Hello následující postupy jsou související tooSpark 1.6. Pro verzi hello Spark 2.0 hello použití poznámkových bloků popsané a propojené toopreviously. 

1 musíte mít předplatné Azure. Pokud není již nemáte, přečtěte si téma [získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. toocomplete clusteru Spark 1.6 je nutné tento návod. toocreate, najdete v části hello pokyny uvedené v [Začínáme: Vytvořte Apache Spark v Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). typ clusteru Hello a verze je určené z hello **vybrat typ clusteru** nabídky. 

![Konfigurace clusteru](./media/machine-learning-data-science-spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [!NOTE]
> Téma, které ukazuje, jak toouse Scala spíše než Python toocomplete úloh na začátku do konce dat proces vědecké účely, najdete v části hello [vědecké zpracování dat pomocí Spark v Azure Scala](machine-learning-data-science-process-scala-walkthrough.md).
> 
> 

<!-- -->

> [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

## <a name="hello-nyc-2013-taxi-data"></a>Hello NYC 2013 taxíkem dat
Hello NYC taxíkem cestě dat je přibližně 20 GB komprimované hodnot oddělených čárkami (CSV) souborů (nekomprimovaným ~ 48 GB), která obsahuje více než 173 milionů jednotlivých cest a hello tarify placené pro každou cestu. Každý záznam cestě zahrnuje hello vyzvednutí a odkládací umístění a čas, číslo licence anonymizovaná hackerský (ovladač) a číslo Medailon (taxi na jedinečné id). Hello dat obsahuje všechny služebních cest v hello roku 2013 a je součástí hello následující dvě datové sady pro každý měsíc:

1. Hello 'trip_data' CSV soubory obsahují podrobnosti o cestě, například na počtu cestujících, vyzvednutí a body dropoff dojít doba trvání a délka cesty. Tady je několik ukázkových záznamů:
   
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

Přesměrovali jsme ukázku 0,1 % těchto souborů a cestu připojeného k hello\_dat a cesty\_jízdenky CVS soubory do toouse jednu datovou sadu jako vstupní datové sady hello v tomto návodu. Hello jedinečné klíče toojoin cestě\_dat a cesty\_tarif se skládá z pole hello: medailonu, hackerský\_licence a vyzvednutí\_data a času. Každý záznam hello datová sada obsahuje následující atributy představující výlet NYC taxíkem hello:

| Pole | Stručný popis |
| --- | --- |
| Medailon |Anonymizovaná taxíkem Medailon (taxi jedinečné id) |
| hack_license |Anonymizovaná číslo licence Hackney znaků CR |
| vendor_id |Id dodavatele taxíkem |
| rate_code |Míra taxíkem NYC tarif |
| store_and_fwd_flag |Úložiště a předat dál příznak |
| pickup_datetime |Vyzvednutí datum a čas |
| dropoff_datetime |Dropoff datum a čas |
| pickup_hour |Vyzvedne, až hodinu |
| pickup_week |Vyzvednutí týden roku hello |
| den v týdnu |Den v týdnu (rozsah 1-7) |
| passenger_count |Počet cestujících v cestě taxíkem |
| trip_time_in_secs |Času v sekundách |
| trip_distance |Vzdálenost služební cestě, kterou urazit v miles |
| pickup_longitude |Vyzvednutí zeměpisná délka |
| pickup_latitude |Vyzvednutí zeměpisná šířka |
| dropoff_longitude |Zeměpisná délka Dropoff |
| dropoff_latitude |Zeměpisná šířka Dropoff |
| direct_distance |Přímá vzdálenost mezi vybrat nahoru a dropoff umístění |
| payment_type |Typ platby (certifikační autority, platební karty atd.) |
| fare_amount |Tarif částka v |
| Příplatek. |Příplatek. |
| mta_tax |Daň prostředí MTA |
| tip_amount |Velikost tipu |
| tolls_amount |Velikost mýtné |
| total_amount |Celková velikost |
| vysypávány |Šikmý (0 nebo 1 – Ne nebo Ano) |
| tip_class |Tip – třída (0: 0, 1: $0-5, 2: $6-10, 3: $11-20, 4: > 20) |

## <a name="execute-code-from-a-jupyter-notebook-on-hello-spark-cluster"></a>Spustí kód z poznámkového bloku Jupyter v clusteru Spark hello
Můžete spustit hello Poznámkový blok Jupyter z hello portálu Azure. Najít váš cluster Spark na řídicím panelu a klikněte na stránce Správa tooenter pro váš cluster. Klikněte na tlačítko tooopen hello poznámkového bloku spojené s clusterem Spark hello, **řídicí panely clusteru** -> **Poznámkový blok Jupyter** .

![Řídicí panely clusteru](./media/machine-learning-data-science-spark-overview/spark-jupyter-on-portal.png)

Můžete také vyhledat příliš***https://CLUSTERNAME.azurehdinsight.net/jupyter*** tooaccess hello poznámkové bloky Jupyter. Nahraďte název hello vlastní cluster hello CLUSTERNAME součástí této adresy URL. Potřebujete hello heslo pro poznámkové bloky hello tooaccess účet správce.

![Procházet poznámkové bloky Jupyter](./media/machine-learning-data-science-spark-overview/spark-jupyter-notebook.png)

Vyberte PySpark toosee adresář, který obsahuje několik příkladů předem zabalené poznámkových bloků, které používají hello PySpark API.hello poznámkových bloků, které obsahují hello ukázky kódu pro tuto sadu Spark tématu jsou k dispozici na [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)

Můžete nahrát poznámkových bloků hello přímo z [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark) server poznámkového bloku Jupyter toohello na váš cluster Spark. Na domovské stránce hello vaší Jupyter, klikněte na tlačítko hello **nahrát** tlačítko na hello právo součástí úvodní obrazovka. Otevře se Průzkumník souborů. Sem můžete vložit hello GitHub (nezpracovaná obsah) URL hello poznámkového bloku a klikněte na tlačítko **otevřete**. 

V seznamu souborů Jupyter s zobrazí název souboru hello **nahrát** tlačítko znovu. Klikněte na tlačítko to **nahrát** tlačítko. Nyní jste importovali hello Poznámkový blok. Opakujte tyto kroky tooupload hello jiných poznámkových bloků z tohoto návodu.

> [!TIP]
> Můžete kliknout pravým tlačítkem hello odkazy na prohlížeč a vyberte **Kopírovat odkaz** tooget hello githubu nezpracované obsahu adresy URL. Tuto adresu URL můžete vložit do hello Jupyter nahrát soubor explorer dialogové okno.
> 
> 

Nyní můžete:

* Zobrazit kód hello kliknutím hello Poznámkový blok.
* Spusťte jednotlivých buněk tak, že stisknete **zadejte SHIFT**.
* Kliknutím na spustit celý poznámkový blok hello **buňky** -> **spustit**.
* Použijte automatické vizualizace hello dotazů.

> [!TIP]
> jádra PySpark Hello automaticky vizualizuje výstup hello dotazů SQL (HiveQL). Budete mít možnost tooselect hello mezi několika různých typů vizualizace (tabulky, kruhový, řádku, oblasti nebo panelu) pomocí hello **typ** tlačítka nabídky v poznámkovém bloku hello:
> 
> 

![Křivka ROC logistic regression pro obecný přístup](./media/machine-learning-data-science-spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Co dále?
Teď, když jsou vytvořeny pomocí clusteru služby HDInsight Spark a odeslali hello poznámkové bloky Jupyter, jste připravené toowork prostřednictvím hello témata, které odpovídají toohello tři poznámkových bloků PySpark. Ukazují jak tooexplore vaše data a poté jak toocreate a využívat modelů. Hello advanced zkoumání dat a modelování poznámkového bloku ukazuje, jak tooinclude (vymetání) křížového ověřování, technologie hyper parametr komínů a vyhodnocení modelu. 

**Zkoumání dat a modelování pomocí Spark:** prozkoumat hello datovou sadu a vytvářet, stanovení skóre a vyhodnotit hello machine learning modely projdete hello [vytvořit binární klasifikace a regrese modely pro data s hello Spark Sada nástrojů MLlib](machine-learning-data-science-spark-data-exploration-modeling.md) tématu.

**Model spotřeby:** toolearn způsobu tooscore hello klasifikace a regrese modely vytvořené v tomto tématu najdete v [skóre a vyhodnocení modelů learning vytvořené Spark počítač](machine-learning-data-science-spark-model-consumption.md).

**Křížové ověření a hyperparameter (vymetání) komínů**: najdete v části [Advanced zkoumání dat a modelování pomocí Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) na tom, jak může být modely Trénink pomocí (vymetání) křížové ověření a technologie hyper parametr komínů

