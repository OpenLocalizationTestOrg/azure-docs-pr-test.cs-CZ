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
# <a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a><span data-ttu-id="0f18c-103">Vytvoření funkcí pro data v clusteru Hadoop pomocí dotazů Hivu</span><span class="sxs-lookup"><span data-stu-id="0f18c-103">Create features for data in an Hadoop cluster using Hive queries</span></span>
<span data-ttu-id="0f18c-104">Tento dokument ukazuje, jak funkce toocreate pro data uložená v clusteru Azure HDInsight Hadoop pomocí dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="0f18c-104">This document shows how toocreate features for data stored in an Azure HDInsight Hadoop cluster using Hive queries.</span></span> <span data-ttu-id="0f18c-105">Tyto dotazy Hive pomocí vložených Hive uživatelsky definovaných funkcí (UDF), skripty hello, pro které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0f18c-105">These Hive queries use embedded Hive User Defined Functions (UDFs), hello scripts for which are provided.</span></span>

<span data-ttu-id="0f18c-106">Funkce toocreate potřebných operací Hello může být náročná na paměť.</span><span class="sxs-lookup"><span data-stu-id="0f18c-106">hello operations needed toocreate features can be memory intensive.</span></span> <span data-ttu-id="0f18c-107">Hello výkon dotazů Hive se stane více důležitých v takových případech a lze vylepšit optimalizace určitých parametrů.</span><span class="sxs-lookup"><span data-stu-id="0f18c-107">hello performance of Hive queries becomes more critical in such cases and can be improved by tuning certain parameters.</span></span> <span data-ttu-id="0f18c-108">Hello ladění z těchto parametrů je popsané v poslední části hello.</span><span class="sxs-lookup"><span data-stu-id="0f18c-108">hello tuning of these parameters is discussed in hello final section.</span></span>

<span data-ttu-id="0f18c-109">Hello dotazy, které jsou uvedené příklady konkrétní toohello [NYC taxíkem cestě Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scénáře jsou také uvedeny v [úložiště GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span><span class="sxs-lookup"><span data-stu-id="0f18c-109">Examples of hello queries that are presented are specific toohello [NYC Taxi Trip Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarios are also provided in [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span></span> <span data-ttu-id="0f18c-110">Tyto dotazy už máte zadané schéma data a jsou připravené toobe odeslána toorun.</span><span class="sxs-lookup"><span data-stu-id="0f18c-110">These queries already have data schema specified and are ready toobe submitted toorun.</span></span> <span data-ttu-id="0f18c-111">V poslední části hello jsou také popsány parametry, které uživatelé můžete vyladit tak, aby je možné zlepšit výkon dotazů Hive hello.</span><span class="sxs-lookup"><span data-stu-id="0f18c-111">In hello final section, parameters that users can tune so that hello performance of Hive queries can be improved are  also discussed.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="0f18c-112">To **nabídky** odkazy tootopics, které popisují, jak funkce toocreate pro data v různých prostředích.</span><span class="sxs-lookup"><span data-stu-id="0f18c-112">This **menu** links tootopics that describe how toocreate features for data in various environments.</span></span> <span data-ttu-id="0f18c-113">Tato úloha je krok v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="0f18c-113">This task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0f18c-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0f18c-114">Prerequisites</span></span>
<span data-ttu-id="0f18c-115">Tento článek předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="0f18c-115">This article assumes that you have:</span></span>

* <span data-ttu-id="0f18c-116">Vytvořit účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="0f18c-116">Created an Azure storage account.</span></span> <span data-ttu-id="0f18c-117">Pokud budete potřebovat pokyny, najdete v části [vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="0f18c-117">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="0f18c-118">Zřizuje přizpůsobené clusteru Hadoop s hello služba HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0f18c-118">Provisioned a customized Hadoop cluster with hello HDInsight service.</span></span>  <span data-ttu-id="0f18c-119">Pokud budete potřebovat pokyny, najdete v části [přizpůsobit Azure HDInsight Hadoop clusterů pro pokročilé analýzy](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="0f18c-119">If you need instructions, see [Customize Azure HDInsight Hadoop Clusters for Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="0f18c-120">Hello data nebyla nahrané tooHive tabulek v Azure HDInsight Hadoop clusterů.</span><span class="sxs-lookup"><span data-stu-id="0f18c-120">hello data has been uploaded tooHive tables in Azure HDInsight Hadoop clusters.</span></span> <span data-ttu-id="0f18c-121">Pokud ne, postupujte podle [vytvořit a zatížení tabulky dat tooHive](machine-learning-data-science-move-hive-tables.md) tooupload data tooHive první tabulky.</span><span class="sxs-lookup"><span data-stu-id="0f18c-121">If it has not, please follow [Create and load data tooHive tables](machine-learning-data-science-move-hive-tables.md) tooupload data tooHive tables first.</span></span>
* <span data-ttu-id="0f18c-122">Povolit clusteru toohello vzdáleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="0f18c-122">Enabled remote access toohello cluster.</span></span> <span data-ttu-id="0f18c-123">Pokud budete potřebovat pokyny, najdete v části [přístup hello uzlu clusteru Hadoop Head](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="0f18c-123">If you need instructions, see [Access hello Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

## <span data-ttu-id="0f18c-124"><a name="hive-featureengineering"></a>Funkce generování</span><span class="sxs-lookup"><span data-stu-id="0f18c-124"><a name="hive-featureengineering"></a>Feature Generation</span></span>
<span data-ttu-id="0f18c-125">V této části jsou popsány několik příkladů hello způsoby, ve kterém může být funkce generování pomocí dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="0f18c-125">In this section, several examples of hello ways in which features can be generating using Hive queries are described.</span></span> <span data-ttu-id="0f18c-126">Po vygenerování můžete další funkce, můžete je přidat jako sloupce toohello existující tabulky nebo vytvořit novou tabulku s hello další funkce a primární klíč, který lze potom spojit s původní tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="0f18c-126">Once you have generated additional features, you can either add them as columns toohello existing table or create a new table with hello additional features and primary key, which can then be joined with hello original table.</span></span> <span data-ttu-id="0f18c-127">Zde jsou uvedené příklady hello:</span><span class="sxs-lookup"><span data-stu-id="0f18c-127">Here are hello examples presented:</span></span>

1. [<span data-ttu-id="0f18c-128">Frekvence na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="0f18c-128">Frequency based Feature Generation</span></span>](#hive-frequencyfeature)
2. [<span data-ttu-id="0f18c-129">Rizika kategorií proměnných v binární klasifikace</span><span class="sxs-lookup"><span data-stu-id="0f18c-129">Risks of Categorical Variables in Binary Classification</span></span>](#hive-riskfeature)
3. [<span data-ttu-id="0f18c-130">Extrahování funkce z pole data a času</span><span class="sxs-lookup"><span data-stu-id="0f18c-130">Extract features from Datetime Field</span></span>](#hive-datefeatures)
4. [<span data-ttu-id="0f18c-131">Extrahování funkce z textové pole.</span><span class="sxs-lookup"><span data-stu-id="0f18c-131">Extract features from Text Field</span></span>](#hive-textfeatures)
5. [<span data-ttu-id="0f18c-132">Vypočítat vzdálenost mezi GPS souřadnice</span><span class="sxs-lookup"><span data-stu-id="0f18c-132">Calculate distance between GPS coordinates</span></span>](#hive-gpsdistance)

### <span data-ttu-id="0f18c-133"><a name="hive-frequencyfeature"></a>Frekvence na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="0f18c-133"><a name="hive-frequencyfeature"></a>Frequency based Feature Generation</span></span>
<span data-ttu-id="0f18c-134">Je často užitečné toocalculate hello frekvence hello úrovně kategorií proměnné nebo hello frekvencí určitých kombinací úrovně z několika kategorií proměnných.</span><span class="sxs-lookup"><span data-stu-id="0f18c-134">It is often useful toocalculate hello frequencies of hello levels of a categorical variable, or hello frequencies of certain combinations of levels from multiple categorical variables.</span></span> <span data-ttu-id="0f18c-135">Uživatele můžete použít následující skript toocalculate hello těchto frekvencí:</span><span class="sxs-lookup"><span data-stu-id="0f18c-135">Users can use hello following script toocalculate these frequencies:</span></span>

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


### <span data-ttu-id="0f18c-136"><a name="hive-riskfeature"></a>Rizika kategorií proměnných v binární klasifikace</span><span class="sxs-lookup"><span data-stu-id="0f18c-136"><a name="hive-riskfeature"></a>Risks of Categorical Variables in Binary Classification</span></span>
<span data-ttu-id="0f18c-137">V binární klasifikaci potřebujeme tooconvert jiné než číselné kategorií proměnné do číselné funkce při modely hello používá pouze číselné funkce.</span><span class="sxs-lookup"><span data-stu-id="0f18c-137">In binary classification, we need tooconvert non-numeric categorical variables into numeric features when hello models being used only take numeric features.</span></span> <span data-ttu-id="0f18c-138">To se provádí nahrazením každou úroveň jiné než číselné číselné riziko.</span><span class="sxs-lookup"><span data-stu-id="0f18c-138">This is done by replacing each non-numeric level with a numeric risk.</span></span> <span data-ttu-id="0f18c-139">V této části ukážeme některé obecné dotazy Hive, které výpočtu hodnoty rizika hello (pravděpodobnost protokolu) kategorií proměnné.</span><span class="sxs-lookup"><span data-stu-id="0f18c-139">In this section, we show some generic Hive queries that calculate hello risk values (log odds) of a categorical variable.</span></span>

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

<span data-ttu-id="0f18c-140">V tomto příkladu proměnné `smooth_param1` a `smooth_param2` jsou vypočítávány nastavené toosmooth hello riziko hodnoty z dat hello.</span><span class="sxs-lookup"><span data-stu-id="0f18c-140">In this example, variables `smooth_param1` and `smooth_param2` are set toosmooth hello risk values calculated from hello data.</span></span> <span data-ttu-id="0f18c-141">Rizika mít rozsahu -Inf a Inf.</span><span class="sxs-lookup"><span data-stu-id="0f18c-141">Risks have a range between -Inf and Inf.</span></span> <span data-ttu-id="0f18c-142">Rizika > 0 označuje, že je větší než 0,5 hello pravděpodobnost, že cíl hello rovna too1.</span><span class="sxs-lookup"><span data-stu-id="0f18c-142">A risks > 0 indicates that hello probability that hello target is equal too1 is greater than 0.5.</span></span>

<span data-ttu-id="0f18c-143">Po hello riziko tabulky se počítá, uživatelé můžete přiřadit tabulka tooa hodnot riziko podle propojení s tabulkou riziko hello.</span><span class="sxs-lookup"><span data-stu-id="0f18c-143">After hello risk table is calculated, users can assign risk values tooa table by joining it with hello risk table.</span></span> <span data-ttu-id="0f18c-144">spojující dotaz Hive Hello zadaná v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="0f18c-144">hello Hive joining query was provided in previous section.</span></span>

### <span data-ttu-id="0f18c-145"><a name="hive-datefeatures"></a>Extrahování funkce z pole data a času</span><span class="sxs-lookup"><span data-stu-id="0f18c-145"><a name="hive-datefeatures"></a>Extract features from Datetime Fields</span></span>
<span data-ttu-id="0f18c-146">Hive obsahuje sadu UDF pro zpracování pole data a času.</span><span class="sxs-lookup"><span data-stu-id="0f18c-146">Hive comes with a set of UDFs for processing datetime fields.</span></span> <span data-ttu-id="0f18c-147">V Hive, formátu data a času výchozí hello je ' rrrr MM-dd 00:00:00 ' (' pod hodnotou 1970-01-01 12:21:32, třeba).</span><span class="sxs-lookup"><span data-stu-id="0f18c-147">In Hive, hello default datetime format is 'yyyy-MM-dd 00:00:00' ('1970-01-01 12:21:32' for example).</span></span> <span data-ttu-id="0f18c-148">V této části ukážeme příklady, které extrahovat hello den měsíce, hello měsíc z pole data a času a další příklady, které jiného, než ve výchozím formátu hello výchozí formát tooa data a času řetězec převést řetězec ve formátu data a času.</span><span class="sxs-lookup"><span data-stu-id="0f18c-148">In this section, we show examples that extract hello day of a month, hello month from a datetime field, and other examples that convert a datetime string in a format other than hello default format tooa datetime string in default format.</span></span>

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

<span data-ttu-id="0f18c-149">Tento dotaz Hive se předpokládá, že hello *& č. 60; pole data a času >* je ve formátu data a času výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="0f18c-149">This Hive query assumes that hello *&#60;datetime field>* is in hello default datetime format.</span></span>

<span data-ttu-id="0f18c-150">Pokud je pole data a času není ve formátu výchozí hello, nejprve musíte pole data a času hello tooconvert do Unix časové razítko a pak převést hello Unix časové razítko tooa data a času řetězec, který je ve výchozím nastavení hello formátu.</span><span class="sxs-lookup"><span data-stu-id="0f18c-150">If a datetime field is not in hello default format, you need tooconvert hello datetime field into Unix time stamp first, and then convert hello Unix time stamp tooa datetime string that is in hello default format.</span></span> <span data-ttu-id="0f18c-151">Pokud data a času hello je ve výchozím formátu, uživatelé mohou nainstalovat hello vložených data a času UDF tooextract funkce.</span><span class="sxs-lookup"><span data-stu-id="0f18c-151">When hello datetime is in default format, users can apply hello embedded datetime UDFs tooextract features.</span></span>

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of hello datetime field>'))
        from <databasename>.<tablename>;

<span data-ttu-id="0f18c-152">V tomto dotazu, pokud hello *& č. 60; pole data a času >* má hello vzor jako *03/26/2015 12:04:39*, hello *' & č. 60; vzor pole data a času hello >'* by měla být `'MM/dd/yyyy HH:mm:ss'`.</span><span class="sxs-lookup"><span data-stu-id="0f18c-152">In this query, if hello *&#60;datetime field>* has hello pattern like *03/26/2015 12:04:39*, hello *'&#60;pattern of hello datetime field>'* should be `'MM/dd/yyyy HH:mm:ss'`.</span></span> <span data-ttu-id="0f18c-153">tootest, můžou uživatelé spouštět</span><span class="sxs-lookup"><span data-stu-id="0f18c-153">tootest it, users can run</span></span>

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

<span data-ttu-id="0f18c-154">Hello *hivesampletable* v tomto dotazu předinstalovaný na všech clusterech Azure HDInsight Hadoop ve výchozím nastavení při zřizování clusterů hello.</span><span class="sxs-lookup"><span data-stu-id="0f18c-154">hello *hivesampletable* in this query comes preinstalled on all Azure HDInsight Hadoop clusters by default when hello clusters are provisioned.</span></span>

### <span data-ttu-id="0f18c-155"><a name="hive-textfeatures"></a>Extrahování funkce z textová pole</span><span class="sxs-lookup"><span data-stu-id="0f18c-155"><a name="hive-textfeatures"></a>Extract features from Text Fields</span></span>
<span data-ttu-id="0f18c-156">Pokud je tabulka Hive hello textové pole, která obsahuje řetězec slova, která jsou oddělené mezerami, hello následující dotaz extrahuje hello délka řetězce hello a hello počet slova v řetězci hello.</span><span class="sxs-lookup"><span data-stu-id="0f18c-156">When hello Hive table has a text field that contains a string of words that are delimited by spaces, hello following query extracts hello length of hello string, and hello number of words in hello string.</span></span>

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <span data-ttu-id="0f18c-157"><a name="hive-gpsdistance"></a>Vypočítat vzdálenosti mezi sady souřadnic, GPS</span><span class="sxs-lookup"><span data-stu-id="0f18c-157"><a name="hive-gpsdistance"></a>Calculate distances between sets of GPS coordinates</span></span>
<span data-ttu-id="0f18c-158">Hello dotazu uvedené v tomto oddílu může být přímo použité toohello NYC taxíkem cestě Data.</span><span class="sxs-lookup"><span data-stu-id="0f18c-158">hello query given in this section can be directly applied toohello NYC Taxi Trip Data.</span></span> <span data-ttu-id="0f18c-159">účelem Hello tohoto dotazu je tooshow jak tooapply embedded matematické funkce funkcí toogenerate Hive.</span><span class="sxs-lookup"><span data-stu-id="0f18c-159">hello purpose of this query is tooshow how tooapply an embedded mathematical functions in Hive toogenerate features.</span></span>

<span data-ttu-id="0f18c-160">Hello pole, které se používají v tomto dotazu jsou souřadnice GPS hello vyzvednutí a dropoff umístění s názvem *vyzvednutí\_zeměpisné délky*, *vyzvednutí\_zeměpisnou šířku*,  *dropoff\_zeměpisné délky*, a *dropoff\_zeměpisnou šířku*.</span><span class="sxs-lookup"><span data-stu-id="0f18c-160">hello fields that are used in this query are hello GPS coordinates of pickup and dropoff locations, named *pickup\_longitude*, *pickup\_latitude*, *dropoff\_longitude*, and *dropoff\_latitude*.</span></span> <span data-ttu-id="0f18c-161">Hello dotazy, které vypočítat hello přímé vzdálenost mezi hello vyzvednutí a dropoff souřadnice jsou:</span><span class="sxs-lookup"><span data-stu-id="0f18c-161">hello queries that calculate hello direct distance between hello pickup and dropoff coordinates are:</span></span>

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

<span data-ttu-id="0f18c-162">Hello matematické vzorce, které vypočítat hello vzdálenost mezi dvě souřadnice GPS naleznete na hello <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type skripty</a> lokality, autorem Petr Lapisu.</span><span class="sxs-lookup"><span data-stu-id="0f18c-162">hello mathematical equations that calculate hello distance between two GPS coordinates can be found on hello <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type Scripts</a> site, authored by Peter Lapisu.</span></span> <span data-ttu-id="0f18c-163">V jeho Javascript hello funkce `toRad()` je právě *lat_or_lon*pí/180 *, která převede tooradians stupňů.</span><span class="sxs-lookup"><span data-stu-id="0f18c-163">In his Javascript, hello function `toRad()` is just *lat_or_lon*pi/180*, which converts degrees tooradians.</span></span> <span data-ttu-id="0f18c-164">Zde *lat_or_lon* hello šířky nebo délky.</span><span class="sxs-lookup"><span data-stu-id="0f18c-164">Here, *lat_or_lon* is hello latitude or longitude.</span></span> <span data-ttu-id="0f18c-165">Vzhledem k tomu, že Hive neposkytuje funkce hello `atan2`, ale poskytuje funkce hello `atan`, hello `atan2` funkce je implementováno modulem `atan` funkce v hello výše dotazu Hive pomocí definice hello součástí <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank"> Wikipedia</a>.</span><span class="sxs-lookup"><span data-stu-id="0f18c-165">Since Hive does not provide hello function `atan2`, but provides hello function `atan`, hello `atan2` function is implemented by `atan` function in hello above Hive query using hello definition provided in <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.</span></span>

![Vytvořit pracovní prostor](./media/machine-learning-data-science-create-features-hive/atan2new.png)

<span data-ttu-id="0f18c-167">Úplný seznam Hive embedded těchto funkcích naleznete v hello **integrované funkce** část hello <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).</span><span class="sxs-lookup"><span data-stu-id="0f18c-167">A full list of Hive embedded UDFs can be found in hello **Built-in Functions** section on hello <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).</span></span>  

## <span data-ttu-id="0f18c-168"><a name="tuning"></a>Rozšířené témata: parametry Hive Tune tooImprove rychlost dotazu</span><span class="sxs-lookup"><span data-stu-id="0f18c-168"><a name="tuning"></a> Advanced topics: Tune Hive Parameters tooImprove Query Speed</span></span>
<span data-ttu-id="0f18c-169">Hello parametrů výchozí nastavení Hive clusteru nemusí být vhodný pro dotazy Hive hello a hello data, která jsou zpracování dotazů hello.</span><span class="sxs-lookup"><span data-stu-id="0f18c-169">hello default parameter settings of Hive cluster might not be suitable for hello Hive queries and hello data that hello queries are processing.</span></span> <span data-ttu-id="0f18c-170">V této části probereme některé parametry, které uživatelé mohli vyladit které hello výkon dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="0f18c-170">In this section, we discuss some parameters that users can tune that improve hello performance of Hive queries.</span></span> <span data-ttu-id="0f18c-171">Uživatelé musí tooadd hello parametr ladění dotazy před hello dotazy pro zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="0f18c-171">Users need tooadd hello parameter tuning queries before hello queries of processing data.</span></span>

1. <span data-ttu-id="0f18c-172">**Místo haldy Java**: pro dotazy týkající se propojení rozsáhlých datových sad, nebo zpracování dlouho záznamů, **volné místo haldy** je jednou z běžných chyb hello.</span><span class="sxs-lookup"><span data-stu-id="0f18c-172">**Java heap space**: For queries involving joining large datasets, or processing long records, **running out of heap space** is one of hello common error.</span></span> <span data-ttu-id="0f18c-173">To lze ladit nastavením parametry *mapreduce.map.java.opts* a *mapreduce.task.io.sort.mb* toodesired hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0f18c-173">This can be tuned by setting parameters *mapreduce.map.java.opts* and *mapreduce.task.io.sort.mb* toodesired values.</span></span> <span data-ttu-id="0f18c-174">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="0f18c-174">Here is an example:</span></span>
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    <span data-ttu-id="0f18c-175">Tento parametr přiděluje místo haldy tooJava paměti 4GB a také umožňuje řazení zefektivnit přidělením více paměti pro ni.</span><span class="sxs-lookup"><span data-stu-id="0f18c-175">This parameter allocates 4GB memory tooJava heap space and also makes sorting more efficient by allocating more memory for it.</span></span> <span data-ttu-id="0f18c-176">Pokud jsou všechny úlohy selhání chyby související tooheap místa je vhodné tooplay s tyto přidělení.</span><span class="sxs-lookup"><span data-stu-id="0f18c-176">It is a good idea tooplay with these allocations if there are any job failure errors related tooheap space.</span></span>

1. <span data-ttu-id="0f18c-177">**Velikost bloku systému souborů DFS** : Tento parametr nastavuje hello nejmenší jednotka data, která hello úložiště systému souborů.</span><span class="sxs-lookup"><span data-stu-id="0f18c-177">**DFS block size** : This parameter sets hello smallest unit of data that hello file system stores.</span></span> <span data-ttu-id="0f18c-178">Jako příklad Pokud velikost bloku systému souborů DFS hello je 128MB, pak žádná data o velikosti menší než nebo vyšší too128MB je uložen v jeden blok, při data, která je větší než 128MB je vymezena navíc bloky.</span><span class="sxs-lookup"><span data-stu-id="0f18c-178">As an example, if hello DFS block size is 128MB, then any data of size less than and up too128MB is stored in a single block, while data that is larger than 128MB is allotted extra blocks.</span></span> <span data-ttu-id="0f18c-179">Volba velikosti velmi malé bloku způsobí, že velké režie v Hadoop vzhledem k tomu, že má uzel název hello tooprocess mnoho další požadavky toofind hello relevantní bloku, která se týkají toohello souboru.</span><span class="sxs-lookup"><span data-stu-id="0f18c-179">Choosing a very small block size causes large overheads in Hadoop since hello name node has tooprocess many more requests toofind hello relevant block pertaining toohello file.</span></span> <span data-ttu-id="0f18c-180">Doporučená nastavení, když zabývají gigabajtů (nebo vyšší) data:</span><span class="sxs-lookup"><span data-stu-id="0f18c-180">A recommended setting when dealing with gigabytes (or larger) data is :</span></span>
   
        set dfs.block.size=128m;
2. <span data-ttu-id="0f18c-181">**Optimalizace operace spojení v podregistru** : během operace spojení v hello mapy nebo snížit framework trvají obvykle místě v hello omezit fáze, někdy, lze dosáhnout značné zvýšení plánování spojení ve fázi mapy hello (také nazývané "mapjoins").</span><span class="sxs-lookup"><span data-stu-id="0f18c-181">**Optimizing join operation in Hive** : While join operations in hello map/reduce framework typically take place in hello reduce phase, sometimes, enormous gains can be achieved by scheduling joins in hello map phase (also called "mapjoins").</span></span> <span data-ttu-id="0f18c-182">toodirect podregistru toodo to kdykoli je to možné, můžete nastavit jsme:</span><span class="sxs-lookup"><span data-stu-id="0f18c-182">toodirect Hive toodo this whenever possible, we can set :</span></span>
   
        set hive.auto.convert.join=true;
3. <span data-ttu-id="0f18c-183">**Určení hello počet mappers tooHive** : při Hadoop umožňuje hello uživatele tooset hello počet přechodky, hello počet mappers obvykle nebyla nastavená uživatelem hello.</span><span class="sxs-lookup"><span data-stu-id="0f18c-183">**Specifying hello number of mappers tooHive** : While Hadoop allows hello user tooset hello number of reducers, hello number of mappers is typically not be set by hello user.</span></span> <span data-ttu-id="0f18c-184">Podvodné, která umožňuje určitý stupeň řízení na toto číslo je toochoose hello Hadoop proměnné, *mapred.min.split.size* a *mapred.max.split.size* jako hello velikost každé mapy je dáno úloh:</span><span class="sxs-lookup"><span data-stu-id="0f18c-184">A trick that allows some degree of control on this number is toochoose hello Hadoop variables, *mapred.min.split.size* and *mapred.max.split.size* as hello size of each map task is determined by :</span></span>
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    <span data-ttu-id="0f18c-185">Obvykle hello výchozí hodnotu *mapred.min.split.size* 0, s *mapred.max.split.size* je **Long.MAX** a *dfs.block.size* 64 MB.</span><span class="sxs-lookup"><span data-stu-id="0f18c-185">Typically, hello default value of *mapred.min.split.size* is 0, that of *mapred.max.split.size* is **Long.MAX** and that of *dfs.block.size* is 64MB.</span></span> <span data-ttu-id="0f18c-186">Jak jsme můžete vidět, velikost dat daného hello ladění tyto parametry "nastavení" je umožňuje nám tootune hello počet mappers použít.</span><span class="sxs-lookup"><span data-stu-id="0f18c-186">As we can see, given hello data size, tuning these parameters by "setting" them allows us tootune hello number of mappers used.</span></span>
4. <span data-ttu-id="0f18c-187">Několik dalších dalších **rozšířené možnosti** pro optimalizaci Hive výkonu jsou uvedená níže.</span><span class="sxs-lookup"><span data-stu-id="0f18c-187">A few other more **advanced options** for optimizing Hive performance are mentioned below.</span></span> <span data-ttu-id="0f18c-188">Tyto umožňují tooset hello paměti přidělené toomap a snížit úlohy a mohou být užitečné při postupně je upravujte výkonu.</span><span class="sxs-lookup"><span data-stu-id="0f18c-188">These allow you tooset hello memory allocated toomap and reduce tasks, and can be useful in tweaking performance.</span></span> <span data-ttu-id="0f18c-189">Prosím mějte na paměti této hello *mapreduce.reduce.memory.mb* nemůže být větší než velikost fyzické paměti hello každý pracovní uzel v clusteru Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="0f18c-189">Please keep in mind that hello *mapreduce.reduce.memory.mb* cannot be greater than hello physical memory size of each worker node in hello Hadoop cluster.</span></span>
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

