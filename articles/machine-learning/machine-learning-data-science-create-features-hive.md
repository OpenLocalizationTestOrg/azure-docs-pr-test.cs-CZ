---
title: "Vytvoření funkce pro data v clusteru služby Hadoop pomocí dotazů Hive | Microsoft Docs"
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
ms.openlocfilehash: e027a6ffcb63868be13432870e484c5cbf2eef4b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a><span data-ttu-id="8d599-103">Vytvoření funkcí pro data v clusteru Hadoop pomocí dotazů Hivu</span><span class="sxs-lookup"><span data-stu-id="8d599-103">Create features for data in an Hadoop cluster using Hive queries</span></span>
<span data-ttu-id="8d599-104">Tento dokument ukazuje, jak vytvořit funkcí pro data uložená v clusteru Azure HDInsight Hadoop pomocí dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="8d599-104">This document shows how to create features for data stored in an Azure HDInsight Hadoop cluster using Hive queries.</span></span> <span data-ttu-id="8d599-105">Tyto dotazy Hive pomocí vložených Hive uživatelsky definovaných funkcí (UDF), skripty, pro které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="8d599-105">These Hive queries use embedded Hive User Defined Functions (UDFs), the scripts for which are provided.</span></span>

<span data-ttu-id="8d599-106">Operace, které jsou nutné k vytvoření funkce může být náročná na paměť.</span><span class="sxs-lookup"><span data-stu-id="8d599-106">The operations needed to create features can be memory intensive.</span></span> <span data-ttu-id="8d599-107">Výkon dotazů Hive se stane více důležitých v takových případech a lze vylepšit optimalizace určitých parametrů.</span><span class="sxs-lookup"><span data-stu-id="8d599-107">The performance of Hive queries becomes more critical in such cases and can be improved by tuning certain parameters.</span></span> <span data-ttu-id="8d599-108">Ladění z těchto parametrů je popsané v poslední části.</span><span class="sxs-lookup"><span data-stu-id="8d599-108">The tuning of these parameters is discussed in the final section.</span></span>

<span data-ttu-id="8d599-109">Příklady dotazů, které jsou uvedené jsou specifické pro [NYC taxíkem cestě Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scénáře jsou také uvedeny v [úložiště GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span><span class="sxs-lookup"><span data-stu-id="8d599-109">Examples of the queries that are presented are specific to the [NYC Taxi Trip Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarios are also provided in [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span></span> <span data-ttu-id="8d599-110">Tyto dotazy už máte zadané schéma data a jsou připravené k odeslání do spustit.</span><span class="sxs-lookup"><span data-stu-id="8d599-110">These queries already have data schema specified and are ready to be submitted to run.</span></span> <span data-ttu-id="8d599-111">V poslední části jsou také popsány parametry, které uživatelé můžete vyladit tak, aby je možné zlepšit výkon dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="8d599-111">In the final section, parameters that users can tune so that the performance of Hive queries can be improved are  also discussed.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="8d599-112">To **nabídky** odkazy na témata, které popisují, jak vytvořit funkce pro data v různých prostředích.</span><span class="sxs-lookup"><span data-stu-id="8d599-112">This **menu** links to topics that describe how to create features for data in various environments.</span></span> <span data-ttu-id="8d599-113">Tato úloha je krok v [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="8d599-113">This task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8d599-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8d599-114">Prerequisites</span></span>
<span data-ttu-id="8d599-115">Tento článek předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="8d599-115">This article assumes that you have:</span></span>

* <span data-ttu-id="8d599-116">Vytvořit účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="8d599-116">Created an Azure storage account.</span></span> <span data-ttu-id="8d599-117">Pokud budete potřebovat pokyny, najdete v části [vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="8d599-117">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="8d599-118">Zřizuje přizpůsobené clusteru Hadoop se službou HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8d599-118">Provisioned a customized Hadoop cluster with the HDInsight service.</span></span>  <span data-ttu-id="8d599-119">Pokud budete potřebovat pokyny, najdete v části [přizpůsobit Azure HDInsight Hadoop clusterů pro pokročilé analýzy](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="8d599-119">If you need instructions, see [Customize Azure HDInsight Hadoop Clusters for Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="8d599-120">Data byla uložena do tabulek Hive v Azure HDInsight Hadoop clusterů.</span><span class="sxs-lookup"><span data-stu-id="8d599-120">The data has been uploaded to Hive tables in Azure HDInsight Hadoop clusters.</span></span> <span data-ttu-id="8d599-121">Pokud ne, postupujte podle [vytvoření a načtení dat do tabulek Hive](machine-learning-data-science-move-hive-tables.md) nejprve nahrát data do tabulek Hive.</span><span class="sxs-lookup"><span data-stu-id="8d599-121">If it has not, please follow [Create and load data to Hive tables](machine-learning-data-science-move-hive-tables.md) to upload data to Hive tables first.</span></span>
* <span data-ttu-id="8d599-122">Povolit pro vzdálený přístup ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d599-122">Enabled remote access to the cluster.</span></span> <span data-ttu-id="8d599-123">Pokud budete potřebovat pokyny, najdete v části [přístup hlavního uzlu Hadoop clusteru](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="8d599-123">If you need instructions, see [Access the Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

## <span data-ttu-id="8d599-124"><a name="hive-featureengineering"></a>Funkce generování</span><span class="sxs-lookup"><span data-stu-id="8d599-124"><a name="hive-featureengineering"></a>Feature Generation</span></span>
<span data-ttu-id="8d599-125">V této části jsou popsány několik příkladů, způsoby, ve kterém může být funkce generování pomocí dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="8d599-125">In this section, several examples of the ways in which features can be generating using Hive queries are described.</span></span> <span data-ttu-id="8d599-126">Po vygenerování můžete další funkce, můžete je přidat jako sloupce do existující tabulky nebo vytvořit novou tabulku s další funkce a primární klíč, který lze potom spojit s původní tabulky.</span><span class="sxs-lookup"><span data-stu-id="8d599-126">Once you have generated additional features, you can either add them as columns to the existing table or create a new table with the additional features and primary key, which can then be joined with the original table.</span></span> <span data-ttu-id="8d599-127">Zde jsou uvedené příklady:</span><span class="sxs-lookup"><span data-stu-id="8d599-127">Here are the examples presented:</span></span>

1. [<span data-ttu-id="8d599-128">Frekvence na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="8d599-128">Frequency based Feature Generation</span></span>](#hive-frequencyfeature)
2. [<span data-ttu-id="8d599-129">Rizika kategorií proměnných v binární klasifikace</span><span class="sxs-lookup"><span data-stu-id="8d599-129">Risks of Categorical Variables in Binary Classification</span></span>](#hive-riskfeature)
3. [<span data-ttu-id="8d599-130">Extrahování funkce z pole data a času</span><span class="sxs-lookup"><span data-stu-id="8d599-130">Extract features from Datetime Field</span></span>](#hive-datefeatures)
4. [<span data-ttu-id="8d599-131">Extrahování funkce z textové pole.</span><span class="sxs-lookup"><span data-stu-id="8d599-131">Extract features from Text Field</span></span>](#hive-textfeatures)
5. [<span data-ttu-id="8d599-132">Vypočítat vzdálenost mezi GPS souřadnice</span><span class="sxs-lookup"><span data-stu-id="8d599-132">Calculate distance between GPS coordinates</span></span>](#hive-gpsdistance)

### <span data-ttu-id="8d599-133"><a name="hive-frequencyfeature"></a>Frekvence na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="8d599-133"><a name="hive-frequencyfeature"></a>Frequency based Feature Generation</span></span>
<span data-ttu-id="8d599-134">Často je užitečné pro výpočet frekvence úrovní záznamu do kategorií proměnné nebo frekvencí určitých kombinací úrovně z několika kategorií proměnných.</span><span class="sxs-lookup"><span data-stu-id="8d599-134">It is often useful to calculate the frequencies of the levels of a categorical variable, or the frequencies of certain combinations of levels from multiple categorical variables.</span></span> <span data-ttu-id="8d599-135">Uživatele můžete použít následující skript k výpočtu těchto frekvencí:</span><span class="sxs-lookup"><span data-stu-id="8d599-135">Users can use the following script to calculate these frequencies:</span></span>

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


### <span data-ttu-id="8d599-136"><a name="hive-riskfeature"></a>Rizika kategorií proměnných v binární klasifikace</span><span class="sxs-lookup"><span data-stu-id="8d599-136"><a name="hive-riskfeature"></a>Risks of Categorical Variables in Binary Classification</span></span>
<span data-ttu-id="8d599-137">V binární klasifikaci je potřeba převést jiné než číselné kategorií proměnné číselnou funkce při modely používá pouze číselné funkce.</span><span class="sxs-lookup"><span data-stu-id="8d599-137">In binary classification, we need to convert non-numeric categorical variables into numeric features when the models being used only take numeric features.</span></span> <span data-ttu-id="8d599-138">To se provádí nahrazením každou úroveň jiné než číselné číselné riziko.</span><span class="sxs-lookup"><span data-stu-id="8d599-138">This is done by replacing each non-numeric level with a numeric risk.</span></span> <span data-ttu-id="8d599-139">V této části ukážeme některé obecné dotazy Hive, které výpočet hodnot riziko (pravděpodobnost protokolu) kategorií proměnné.</span><span class="sxs-lookup"><span data-stu-id="8d599-139">In this section, we show some generic Hive queries that calculate the risk values (log odds) of a categorical variable.</span></span>

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

<span data-ttu-id="8d599-140">V tomto příkladu proměnné `smooth_param1` a `smooth_param2` jsou nastaveny na funkce smooth hodnoty rizika vypočítanou ze data.</span><span class="sxs-lookup"><span data-stu-id="8d599-140">In this example, variables `smooth_param1` and `smooth_param2` are set to smooth the risk values calculated from the data.</span></span> <span data-ttu-id="8d599-141">Rizika mít rozsahu -Inf a Inf.</span><span class="sxs-lookup"><span data-stu-id="8d599-141">Risks have a range between -Inf and Inf.</span></span> <span data-ttu-id="8d599-142">Rizika > 0 označuje, že je větší než 0,5 pravděpodobnost, že cíl je rovno 1.</span><span class="sxs-lookup"><span data-stu-id="8d599-142">A risks > 0 indicates that the probability that the target is equal to 1 is greater than 0.5.</span></span>

<span data-ttu-id="8d599-143">Po riziko se počítá tabulky uživatele lze přiřadit hodnoty rizika do tabulky pomocí připojení s tabulkou riziko.</span><span class="sxs-lookup"><span data-stu-id="8d599-143">After the risk table is calculated, users can assign risk values to a table by joining it with the risk table.</span></span> <span data-ttu-id="8d599-144">Dotaz Hive spojující zadaná v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="8d599-144">The Hive joining query was provided in previous section.</span></span>

### <span data-ttu-id="8d599-145"><a name="hive-datefeatures"></a>Extrahování funkce z pole data a času</span><span class="sxs-lookup"><span data-stu-id="8d599-145"><a name="hive-datefeatures"></a>Extract features from Datetime Fields</span></span>
<span data-ttu-id="8d599-146">Hive obsahuje sadu UDF pro zpracování pole data a času.</span><span class="sxs-lookup"><span data-stu-id="8d599-146">Hive comes with a set of UDFs for processing datetime fields.</span></span> <span data-ttu-id="8d599-147">V Hive, je výchozí formát data a času, rrrr MM-dd 00:00:00 ' (' pod hodnotou 1970-01-01 12:21:32, třeba).</span><span class="sxs-lookup"><span data-stu-id="8d599-147">In Hive, the default datetime format is 'yyyy-MM-dd 00:00:00' ('1970-01-01 12:21:32' for example).</span></span> <span data-ttu-id="8d599-148">V této části ukážeme příklady, které extrahování dne v měsíci, měsíc z pole data a času a další příklady, které převést řetězec ve formátu data a času, jiné než výchozí formát řetězce data a času ve výchozím formátu.</span><span class="sxs-lookup"><span data-stu-id="8d599-148">In this section, we show examples that extract the day of a month, the month from a datetime field, and other examples that convert a datetime string in a format other than the default format to a datetime string in default format.</span></span>

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

<span data-ttu-id="8d599-149">Tento dotaz Hive předpokládá, že *& č. 60; pole data a času >* je ve výchozím formátu data a času.</span><span class="sxs-lookup"><span data-stu-id="8d599-149">This Hive query assumes that the *&#60;datetime field>* is in the default datetime format.</span></span>

<span data-ttu-id="8d599-150">Pokud je pole data a času není ve formátu výchozí, budete muset nejdřív převést pole data a času na časové razítko systému Unix a pak převést na řetězec data a času, který je ve výchozím formátu Unix časové razítko.</span><span class="sxs-lookup"><span data-stu-id="8d599-150">If a datetime field is not in the default format, you need to convert the datetime field into Unix time stamp first, and then convert the Unix time stamp to a datetime string that is in the default format.</span></span> <span data-ttu-id="8d599-151">Pokud hodnota datetime je ve výchozím formátu, uživatelé mohou nainstalovat vložená data a času k extrakci funkce UDF.</span><span class="sxs-lookup"><span data-stu-id="8d599-151">When the datetime is in default format, users can apply the embedded datetime UDFs to extract features.</span></span>

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

<span data-ttu-id="8d599-152">V tomto dotazu Pokud *& č. 60; pole data a času >* má vzor jako *03/26/2015 12:04:39*, *' & č. 60; vzor pole data a času >'* by měla být `'MM/dd/yyyy HH:mm:ss'`.</span><span class="sxs-lookup"><span data-stu-id="8d599-152">In this query, if the *&#60;datetime field>* has the pattern like *03/26/2015 12:04:39*, the *'&#60;pattern of the datetime field>'* should be `'MM/dd/yyyy HH:mm:ss'`.</span></span> <span data-ttu-id="8d599-153">Chcete-li otestovat ji, můžou uživatelé spouštět.</span><span class="sxs-lookup"><span data-stu-id="8d599-153">To test it, users can run</span></span>

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

<span data-ttu-id="8d599-154">*Hivesampletable* v tomto dotazu předinstalovaný na všech clusterech Azure HDInsight Hadoop ve výchozím nastavení při zřizování clusterů.</span><span class="sxs-lookup"><span data-stu-id="8d599-154">The *hivesampletable* in this query comes preinstalled on all Azure HDInsight Hadoop clusters by default when the clusters are provisioned.</span></span>

### <span data-ttu-id="8d599-155"><a name="hive-textfeatures"></a>Extrahování funkce z textová pole</span><span class="sxs-lookup"><span data-stu-id="8d599-155"><a name="hive-textfeatures"></a>Extract features from Text Fields</span></span>
<span data-ttu-id="8d599-156">Pokud tabulka Hive obsahuje textové pole, která obsahuje řetězec slova, která jsou oddělené mezerami, následující dotaz extrahuje délka řetězce a počet slova v řetězci.</span><span class="sxs-lookup"><span data-stu-id="8d599-156">When the Hive table has a text field that contains a string of words that are delimited by spaces, the following query extracts the length of the string, and the number of words in the string.</span></span>

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <span data-ttu-id="8d599-157"><a name="hive-gpsdistance"></a>Vypočítat vzdálenosti mezi sady souřadnic, GPS</span><span class="sxs-lookup"><span data-stu-id="8d599-157"><a name="hive-gpsdistance"></a>Calculate distances between sets of GPS coordinates</span></span>
<span data-ttu-id="8d599-158">Dotaz zadaný v této části je přímo použít pro Data NYC taxíkem cesty.</span><span class="sxs-lookup"><span data-stu-id="8d599-158">The query given in this section can be directly applied to the NYC Taxi Trip Data.</span></span> <span data-ttu-id="8d599-159">Účelem tohoto dotazu je ukazují, jak použít embedded matematické funkce v podregistru ke generování funkce.</span><span class="sxs-lookup"><span data-stu-id="8d599-159">The purpose of this query is to show how to apply an embedded mathematical functions in Hive to generate features.</span></span>

<span data-ttu-id="8d599-160">Pole, které se používají v tomto dotazu jsou souřadnice GPS vyzvednutí a dropoff umístění s názvem *vyzvednutí\_zeměpisné délky*, *vyzvednutí\_zeměpisnou šířku*,  *dropoff\_zeměpisné délky*, a *dropoff\_zeměpisnou šířku*.</span><span class="sxs-lookup"><span data-stu-id="8d599-160">The fields that are used in this query are the GPS coordinates of pickup and dropoff locations, named *pickup\_longitude*, *pickup\_latitude*, *dropoff\_longitude*, and *dropoff\_latitude*.</span></span> <span data-ttu-id="8d599-161">Dotazy, které vypočítat přímé vzdálenost mezi souřadnice vyzvednutí a dropoff jsou:</span><span class="sxs-lookup"><span data-stu-id="8d599-161">The queries that calculate the direct distance between the pickup and dropoff coordinates are:</span></span>

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

<span data-ttu-id="8d599-162">Matematické vzorce, které vypočítat vzdálenost mezi dvě souřadnice GPS naleznete na <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type skripty</a> lokality, autorem Petr Lapisu.</span><span class="sxs-lookup"><span data-stu-id="8d599-162">The mathematical equations that calculate the distance between two GPS coordinates can be found on the <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type Scripts</a> site, authored by Peter Lapisu.</span></span> <span data-ttu-id="8d599-163">V jeho Javascript, funkce `toRad()` je právě *lat_or_lon*pí/180 *, která převede radiánech stupňů.</span><span class="sxs-lookup"><span data-stu-id="8d599-163">In his Javascript, the function `toRad()` is just *lat_or_lon*pi/180*, which converts degrees to radians.</span></span> <span data-ttu-id="8d599-164">Zde *lat_or_lon* zeměpisné šířky nebo délky.</span><span class="sxs-lookup"><span data-stu-id="8d599-164">Here, *lat_or_lon* is the latitude or longitude.</span></span> <span data-ttu-id="8d599-165">Vzhledem k tomu, že Hive neposkytuje funkce `atan2`, ale poskytuje funkce `atan`, `atan2` funkce je implementováno modulem `atan` funkce ve výše uvedeném dotazu Hive pomocí definice součástí <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.</span><span class="sxs-lookup"><span data-stu-id="8d599-165">Since Hive does not provide the function `atan2`, but provides the function `atan`, the `atan2` function is implemented by `atan` function in the above Hive query using the definition provided in <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.</span></span>

![Vytvořit pracovní prostor](./media/machine-learning-data-science-create-features-hive/atan2new.png)

<span data-ttu-id="8d599-167">Úplný seznam Hive embedded těchto funkcích naleznete v **integrované funkce** části na <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).</span><span class="sxs-lookup"><span data-stu-id="8d599-167">A full list of Hive embedded UDFs can be found in the **Built-in Functions** section on the <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).</span></span>  

## <span data-ttu-id="8d599-168"><a name="tuning"></a>Rozšířené témata: optimalizace počítače parametry zvýšit rychlost dotazu Hive</span><span class="sxs-lookup"><span data-stu-id="8d599-168"><a name="tuning"></a> Advanced topics: Tune Hive Parameters to Improve Query Speed</span></span>
<span data-ttu-id="8d599-169">Výchozí nastavení parametrů clusteru Hive nemusí být vhodný pro dotazy Hive a data, která jsou zpracování dotazů.</span><span class="sxs-lookup"><span data-stu-id="8d599-169">The default parameter settings of Hive cluster might not be suitable for the Hive queries and the data that the queries are processing.</span></span> <span data-ttu-id="8d599-170">V této části probereme některé parametry, které uživatelé mohli vyladit které zlepšit výkon dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="8d599-170">In this section, we discuss some parameters that users can tune that improve the performance of Hive queries.</span></span> <span data-ttu-id="8d599-171">Uživatelé musí přidat parametr ladění dotazy před dotazy zpracování data.</span><span class="sxs-lookup"><span data-stu-id="8d599-171">Users need to add the parameter tuning queries before the queries of processing data.</span></span>

1. <span data-ttu-id="8d599-172">**Místo haldy Java**: pro dotazy týkající se propojení rozsáhlých datových sad, nebo zpracování dlouho záznamů, **volné místo haldy** je jednou z běžných chyb.</span><span class="sxs-lookup"><span data-stu-id="8d599-172">**Java heap space**: For queries involving joining large datasets, or processing long records, **running out of heap space** is one of the common error.</span></span> <span data-ttu-id="8d599-173">To lze ladit nastavením parametry *mapreduce.map.java.opts* a *mapreduce.task.io.sort.mb* požadované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8d599-173">This can be tuned by setting parameters *mapreduce.map.java.opts* and *mapreduce.task.io.sort.mb* to desired values.</span></span> <span data-ttu-id="8d599-174">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="8d599-174">Here is an example:</span></span>
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    <span data-ttu-id="8d599-175">Tento parametr přiděluje 4GB paměti do prostoru haldy Java a také umožňuje řazení zefektivnit přidělením více paměti pro ni.</span><span class="sxs-lookup"><span data-stu-id="8d599-175">This parameter allocates 4GB memory to Java heap space and also makes sorting more efficient by allocating more memory for it.</span></span> <span data-ttu-id="8d599-176">Je vhodné můžete experimentovat s tyto přidělení, pokud jsou všechny úlohy chyby související s haldy místa.</span><span class="sxs-lookup"><span data-stu-id="8d599-176">It is a good idea to play with these allocations if there are any job failure errors related to heap space.</span></span>

1. <span data-ttu-id="8d599-177">**Velikost bloku systému souborů DFS** : Tento parametr nastaví nejmenší jednotka data, která ukládá systému souborů.</span><span class="sxs-lookup"><span data-stu-id="8d599-177">**DFS block size** : This parameter sets the smallest unit of data that the file system stores.</span></span> <span data-ttu-id="8d599-178">Jako příklad Pokud velikost bloku systému souborů DFS je 128MB, pak žádná data o velikosti menší a než je 128MB uložen v jeden blok, při data, která je větší než 128MB je vymezena navíc bloky.</span><span class="sxs-lookup"><span data-stu-id="8d599-178">As an example, if the DFS block size is 128MB, then any data of size less than and up to 128MB is stored in a single block, while data that is larger than 128MB is allotted extra blocks.</span></span> <span data-ttu-id="8d599-179">Volba velikosti velmi malé bloku způsobí, že velké režie v Hadoop vzhledem k tomu, že název uzlu má zpracovat mnoho více žádostí o najít relevantní bloku, která se týkají do souboru.</span><span class="sxs-lookup"><span data-stu-id="8d599-179">Choosing a very small block size causes large overheads in Hadoop since the name node has to process many more requests to find the relevant block pertaining to the file.</span></span> <span data-ttu-id="8d599-180">Doporučená nastavení, když zabývají gigabajtů (nebo vyšší) data:</span><span class="sxs-lookup"><span data-stu-id="8d599-180">A recommended setting when dealing with gigabytes (or larger) data is :</span></span>
   
        set dfs.block.size=128m;
2. <span data-ttu-id="8d599-181">**Optimalizace operace spojení v podregistru** : během operace spojení v rámci mapy nebo snižte obvykle provést ve fázi snižte se v některých případech lze dosáhnout značné zvýšení plánování spojení ve fázi mapy (také nazývané "mapjoins").</span><span class="sxs-lookup"><span data-stu-id="8d599-181">**Optimizing join operation in Hive** : While join operations in the map/reduce framework typically take place in the reduce phase, sometimes, enormous gains can be achieved by scheduling joins in the map phase (also called "mapjoins").</span></span> <span data-ttu-id="8d599-182">Pro přesměrování Hive k tomu, kdykoli je to možné, můžete nastavit jsme:</span><span class="sxs-lookup"><span data-stu-id="8d599-182">To direct Hive to do this whenever possible, we can set :</span></span>
   
        set hive.auto.convert.join=true;
3. <span data-ttu-id="8d599-183">**Určující počet mappers k Hive** : při Hadoop umožňuje uživatelům nastavit počet přechodky, počet mappers obvykle nesmí být nastavený uživatelem.</span><span class="sxs-lookup"><span data-stu-id="8d599-183">**Specifying the number of mappers to Hive** : While Hadoop allows the user to set the number of reducers, the number of mappers is typically not be set by the user.</span></span> <span data-ttu-id="8d599-184">Základem, který umožňuje určitý stupeň řízení na toto číslo je volba proměnné Hadoop, *mapred.min.split.size* a *mapred.max.split.size* jako velikost každé mapy je dáno úloh:</span><span class="sxs-lookup"><span data-stu-id="8d599-184">A trick that allows some degree of control on this number is to choose the Hadoop variables, *mapred.min.split.size* and *mapred.max.split.size* as the size of each map task is determined by :</span></span>
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    <span data-ttu-id="8d599-185">Obvykle se výchozí hodnota *mapred.min.split.size* 0, s *mapred.max.split.size* je **Long.MAX** a *dfs.block.size* 64 MB.</span><span class="sxs-lookup"><span data-stu-id="8d599-185">Typically, the default value of *mapred.min.split.size* is 0, that of *mapred.max.split.size* is **Long.MAX** and that of *dfs.block.size* is 64MB.</span></span> <span data-ttu-id="8d599-186">Jak jsme můžete vidět, danou velikost dat ladění tyto parametry "nastavení" je umožňuje optimalizovat počet mappers použít.</span><span class="sxs-lookup"><span data-stu-id="8d599-186">As we can see, given the data size, tuning these parameters by "setting" them allows us to tune the number of mappers used.</span></span>
4. <span data-ttu-id="8d599-187">Několik dalších dalších **rozšířené možnosti** pro optimalizaci Hive výkonu jsou uvedená níže.</span><span class="sxs-lookup"><span data-stu-id="8d599-187">A few other more **advanced options** for optimizing Hive performance are mentioned below.</span></span> <span data-ttu-id="8d599-188">Ty vám umožní nastavit je paměť přidělená pro mapování a snížit úlohy a mohou být užitečné při postupně je upravujte výkonu.</span><span class="sxs-lookup"><span data-stu-id="8d599-188">These allow you to set the memory allocated to map and reduce tasks, and can be useful in tweaking performance.</span></span> <span data-ttu-id="8d599-189">Prosím mějte na paměti, *mapreduce.reduce.memory.mb* nemůže být větší než velikost fyzické paměti každý pracovní uzel v clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="8d599-189">Please keep in mind that the *mapreduce.reduce.memory.mb* cannot be greater than the physical memory size of each worker node in the Hadoop cluster.</span></span>
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

