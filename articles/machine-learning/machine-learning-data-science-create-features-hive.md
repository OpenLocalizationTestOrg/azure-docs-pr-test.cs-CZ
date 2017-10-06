---
title: "Funkce aaaCreate pro data v clusteru služby Hadoop pomocí dotazů Hive | Microsoft Docs"
description: "Příklady dotazů Hive, které generují funkce v dat uložených v clusteru Azure HDInsight Hadoop."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e8a94c71-979b-4707-b8fd-85b47d309a30
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 686282bf0fb84ea82758d3c5b7de2bd90f0cf159
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Vytvoření funkcí pro data v clusteru Hadoop pomocí dotazů Hivu
Tento dokument ukazuje, jak funkce toocreate pro data uložená v clusteru Azure HDInsight Hadoop pomocí dotazů Hive. Tyto dotazy Hive pomocí vložených Hive uživatelsky definovaných funkcí (UDF), skripty hello, pro které jsou k dispozici.

Funkce toocreate potřebných operací Hello může být náročná na paměť. Hello výkon dotazů Hive se stane více důležitých v takových případech a lze vylepšit optimalizace určitých parametrů. Hello ladění z těchto parametrů je popsané v poslední části hello.

Hello dotazy, které jsou uvedené příklady konkrétní toohello [NYC taxíkem cestě Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scénáře jsou také uvedeny v [úložiště GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Tyto dotazy už máte zadané schéma data a jsou připravené toobe odeslána toorun. V poslední části hello jsou také popsány parametry, které uživatelé můžete vyladit tak, aby je možné zlepšit výkon dotazů Hive hello.

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

To **nabídky** odkazy tootopics, které popisují, jak funkce toocreate pro data v různých prostředích. Tato úloha je krok v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že máte:

* Vytvořit účet úložiště Azure. Pokud budete potřebovat pokyny, najdete v části [vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Zřizuje přizpůsobené clusteru Hadoop s hello služba HDInsight.  Pokud budete potřebovat pokyny, najdete v části [přizpůsobit Azure HDInsight Hadoop clusterů pro pokročilé analýzy](machine-learning-data-science-customize-hadoop-cluster.md).
* Hello data nebyla nahrané tooHive tabulek v Azure HDInsight Hadoop clusterů. Pokud ne, postupujte podle [vytvořit a zatížení tabulky dat tooHive](machine-learning-data-science-move-hive-tables.md) tooupload data tooHive první tabulky.
* Povolit clusteru toohello vzdáleného přístupu. Pokud budete potřebovat pokyny, najdete v části [přístup hello uzlu clusteru Hadoop Head](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="hive-featureengineering"></a>Funkce generování
V této části jsou popsány několik příkladů hello způsoby, ve kterém může být funkce generování pomocí dotazů Hive. Po vygenerování můžete další funkce, můžete je přidat jako sloupce toohello existující tabulky nebo vytvořit novou tabulku s hello další funkce a primární klíč, který lze potom spojit s původní tabulky hello. Zde jsou uvedené příklady hello:

1. [Frekvence na základě funkce generování](#hive-frequencyfeature)
2. [Rizika kategorií proměnných v binární klasifikace](#hive-riskfeature)
3. [Extrahování funkce z pole data a času](#hive-datefeatures)
4. [Extrahování funkce z textové pole.](#hive-textfeatures)
5. [Vypočítat vzdálenost mezi GPS souřadnice](#hive-gpsdistance)

### <a name="hive-frequencyfeature"></a>Frekvence na základě funkce generování
Je často užitečné toocalculate hello frekvence hello úrovně kategorií proměnné nebo hello frekvencí určitých kombinací úrovně z několika kategorií proměnných. Uživatele můžete použít následující skript toocalculate hello těchto frekvencí:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


### <a name="hive-riskfeature"></a>Rizika kategorií proměnných v binární klasifikace
V binární klasifikaci potřebujeme tooconvert jiné než číselné kategorií proměnné do číselné funkce při modely hello používá pouze číselné funkce. To se provádí nahrazením každou úroveň jiné než číselné číselné riziko. V této části ukážeme některé obecné dotazy Hive, které výpočtu hodnoty rizika hello (pravděpodobnost protokolu) kategorií proměnné.

        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

V tomto příkladu proměnné `smooth_param1` a `smooth_param2` jsou vypočítávány nastavené toosmooth hello riziko hodnoty z dat hello. Rizika mít rozsahu -Inf a Inf. Rizika > 0 označuje, že je větší než 0,5 hello pravděpodobnost, že cíl hello rovna too1.

Po hello riziko tabulky se počítá, uživatelé můžete přiřadit tabulka tooa hodnot riziko podle propojení s tabulkou riziko hello. spojující dotaz Hive Hello zadaná v předchozí části.

### <a name="hive-datefeatures"></a>Extrahování funkce z pole data a času
Hive obsahuje sadu UDF pro zpracování pole data a času. V Hive, formátu data a času výchozí hello je ' rrrr MM-dd 00:00:00 ' (' pod hodnotou 1970-01-01 12:21:32, třeba). V této části ukážeme příklady, které extrahovat hello den měsíce, hello měsíc z pole data a času a další příklady, které jiného, než ve výchozím formátu hello výchozí formát tooa data a času řetězec převést řetězec ve formátu data a času.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Tento dotaz Hive se předpokládá, že hello *& č. 60; pole data a času >* je ve formátu data a času výchozí hello.

Pokud je pole data a času není ve formátu výchozí hello, nejprve musíte pole data a času hello tooconvert do Unix časové razítko a pak převést hello Unix časové razítko tooa data a času řetězec, který je ve výchozím nastavení hello formátu. Pokud data a času hello je ve výchozím formátu, uživatelé mohou nainstalovat hello vložených data a času UDF tooextract funkce.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of hello datetime field>'))
        from <databasename>.<tablename>;

V tomto dotazu, pokud hello *& č. 60; pole data a času >* má hello vzor jako *03/26/2015 12:04:39*, hello *' & č. 60; vzor pole data a času hello >'* by měla být `'MM/dd/yyyy HH:mm:ss'`. tootest, můžou uživatelé spouštět

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

Hello *hivesampletable* v tomto dotazu předinstalovaný na všech clusterech Azure HDInsight Hadoop ve výchozím nastavení při zřizování clusterů hello.

### <a name="hive-textfeatures"></a>Extrahování funkce z textová pole
Pokud je tabulka Hive hello textové pole, která obsahuje řetězec slova, která jsou oddělené mezerami, hello následující dotaz extrahuje hello délka řetězce hello a hello počet slova v řetězci hello.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <a name="hive-gpsdistance"></a>Vypočítat vzdálenosti mezi sady souřadnic, GPS
Hello dotazu uvedené v tomto oddílu může být přímo použité toohello NYC taxíkem cestě Data. účelem Hello tohoto dotazu je tooshow jak tooapply embedded matematické funkce funkcí toogenerate Hive.

Hello pole, které se používají v tomto dotazu jsou souřadnice GPS hello vyzvednutí a dropoff umístění s názvem *vyzvednutí\_zeměpisné délky*, *vyzvednutí\_zeměpisnou šířku*,  *dropoff\_zeměpisné délky*, a *dropoff\_zeměpisnou šířku*. Hello dotazy, které vypočítat hello přímé vzdálenost mezi hello vyzvednutí a dropoff souřadnice jsou:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

Hello matematické vzorce, které vypočítat hello vzdálenost mezi dvě souřadnice GPS naleznete na hello <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type skripty</a> lokality, autorem Petr Lapisu. V jeho Javascript hello funkce `toRad()` je právě *lat_or_lon*pí/180 *, která převede tooradians stupňů. Zde *lat_or_lon* hello šířky nebo délky. Vzhledem k tomu, že Hive neposkytuje funkce hello `atan2`, ale poskytuje funkce hello `atan`, hello `atan2` funkce je implementováno modulem `atan` funkce v hello výše dotazu Hive pomocí definice hello součástí <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank"> Wikipedia</a>.

![Vytvořit pracovní prostor](./media/machine-learning-data-science-create-features-hive/atan2new.png)

Úplný seznam Hive embedded těchto funkcích naleznete v hello **integrované funkce** část hello <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).  

## <a name="tuning"></a>Rozšířené témata: parametry Hive Tune tooImprove rychlost dotazu
Hello parametrů výchozí nastavení Hive clusteru nemusí být vhodný pro dotazy Hive hello a hello data, která jsou zpracování dotazů hello. V této části probereme některé parametry, které uživatelé mohli vyladit které hello výkon dotazů Hive. Uživatelé musí tooadd hello parametr ladění dotazy před hello dotazy pro zpracování dat.

1. **Místo haldy Java**: pro dotazy týkající se propojení rozsáhlých datových sad, nebo zpracování dlouho záznamů, **volné místo haldy** je jednou z běžných chyb hello. To lze ladit nastavením parametry *mapreduce.map.java.opts* a *mapreduce.task.io.sort.mb* toodesired hodnoty. Zde naleznete příklad:
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    Tento parametr přiděluje místo haldy tooJava paměti 4GB a také umožňuje řazení zefektivnit přidělením více paměti pro ni. Pokud jsou všechny úlohy selhání chyby související tooheap místa je vhodné tooplay s tyto přidělení.

1. **Velikost bloku systému souborů DFS** : Tento parametr nastavuje hello nejmenší jednotka data, která hello úložiště systému souborů. Jako příklad Pokud velikost bloku systému souborů DFS hello je 128MB, pak žádná data o velikosti menší než nebo vyšší too128MB je uložen v jeden blok, při data, která je větší než 128MB je vymezena navíc bloky. Volba velikosti velmi malé bloku způsobí, že velké režie v Hadoop vzhledem k tomu, že má uzel název hello tooprocess mnoho další požadavky toofind hello relevantní bloku, která se týkají toohello souboru. Doporučená nastavení, když zabývají gigabajtů (nebo vyšší) data:
   
        set dfs.block.size=128m;
2. **Optimalizace operace spojení v podregistru** : během operace spojení v hello mapy nebo snížit framework trvají obvykle místě v hello omezit fáze, někdy, lze dosáhnout značné zvýšení plánování spojení ve fázi mapy hello (také nazývané "mapjoins"). toodirect podregistru toodo to kdykoli je to možné, můžete nastavit jsme:
   
        set hive.auto.convert.join=true;
3. **Určení hello počet mappers tooHive** : při Hadoop umožňuje hello uživatele tooset hello počet přechodky, hello počet mappers obvykle nebyla nastavená uživatelem hello. Podvodné, která umožňuje určitý stupeň řízení na toto číslo je toochoose hello Hadoop proměnné, *mapred.min.split.size* a *mapred.max.split.size* jako hello velikost každé mapy je dáno úloh:
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    Obvykle hello výchozí hodnotu *mapred.min.split.size* 0, s *mapred.max.split.size* je **Long.MAX** a *dfs.block.size* 64 MB. Jak jsme můžete vidět, velikost dat daného hello ladění tyto parametry "nastavení" je umožňuje nám tootune hello počet mappers použít.
4. Několik dalších dalších **rozšířené možnosti** pro optimalizaci Hive výkonu jsou uvedená níže. Tyto umožňují tooset hello paměti přidělené toomap a snížit úlohy a mohou být užitečné při postupně je upravujte výkonu. Prosím mějte na paměti této hello *mapreduce.reduce.memory.mb* nemůže být větší než velikost fyzické paměti hello každý pracovní uzel v clusteru Hadoop hello.
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

