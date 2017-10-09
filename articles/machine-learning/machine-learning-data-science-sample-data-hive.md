---
title: "aaaSample data v tabulkách Azure HDInsight Hive | Microsoft Docs"
description: "Dolů vzorkování dat v Azure HDInsight (Hadopop) tabulek Hive"
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: f31e8d01-0fd4-4a10-b1a7-35de3c327521
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 5f86df9b5a18facc875f437abfb004dbe3a06ea4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a><span data-ttu-id="51fa5-103">Ukázková data v tabulkách Azure HDInsight Hive</span><span class="sxs-lookup"><span data-stu-id="51fa5-103">Sample data in Azure HDInsight Hive tables</span></span>
<span data-ttu-id="51fa5-104">V tomto článku jsme popisují, jak toodown ukázková data ukládat do tabulek Azure HDInsight Hive pomocí dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="51fa5-104">In this article, we describe how toodown-sample data stored in Azure HDInsight Hive tables using Hive queries.</span></span> <span data-ttu-id="51fa5-105">Nemůžeme popisuje tři metody vzorkování často se používá použít:</span><span class="sxs-lookup"><span data-stu-id="51fa5-105">We cover three popularly used sampling methods:</span></span>

* <span data-ttu-id="51fa5-106">Uniform náhodné vzorkování</span><span class="sxs-lookup"><span data-stu-id="51fa5-106">Uniform random sampling</span></span>
* <span data-ttu-id="51fa5-107">Náhodné vzorkování podle skupin</span><span class="sxs-lookup"><span data-stu-id="51fa5-107">Random sampling by groups</span></span>
* <span data-ttu-id="51fa5-108">Vrstveného vzorkování</span><span class="sxs-lookup"><span data-stu-id="51fa5-108">Stratified sampling</span></span>

<span data-ttu-id="51fa5-109">Následující Hello **nabídky** odkazy tootopics, které popisují, jak toosample data z různých prostředích úložiště.</span><span class="sxs-lookup"><span data-stu-id="51fa5-109">hello following **menu** links tootopics that describe how toosample data from various storage environments.</span></span>

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="51fa5-110">**Proč ukázková data?**</span><span class="sxs-lookup"><span data-stu-id="51fa5-110">**Why sample your data?**</span></span>
<span data-ttu-id="51fa5-111">Pokud datovou sadu hello plánování tooanalyze velká, je obvykle vhodné hello toodown ukázková data tooreduce ho tooa menší, ale reprezentativní a lepší správu bitlockeru velikost.</span><span class="sxs-lookup"><span data-stu-id="51fa5-111">If hello dataset you plan tooanalyze is large, it's usually a good idea toodown-sample hello data tooreduce it tooa smaller but representative and more manageable size.</span></span> <span data-ttu-id="51fa5-112">To usnadňuje pochopení dat, zkoumání a funkce inženýrství.</span><span class="sxs-lookup"><span data-stu-id="51fa5-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="51fa5-113">Jeho role v hello proces vědecké účely Team dat je rychlé vytváření prototypů tooenable hello zpracování dat funkcí a modelů strojového učení.</span><span class="sxs-lookup"><span data-stu-id="51fa5-113">Its role in hello Team Data Science Process is tooenable fast prototyping of hello data processing functions and machine learning models.</span></span>

<span data-ttu-id="51fa5-114">Tato úloha vzorkování je krok v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="51fa5-114">This sampling task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="how-toosubmit-hive-queries"></a><span data-ttu-id="51fa5-115">Jak toosubmit dotazů Hive</span><span class="sxs-lookup"><span data-stu-id="51fa5-115">How toosubmit Hive queries</span></span>
<span data-ttu-id="51fa5-116">Z příkazového řádku Hadoop konzoly hello hello hlavního uzlu clusteru Hadoop hello lze odesílat dotazy Hive.</span><span class="sxs-lookup"><span data-stu-id="51fa5-116">Hive queries can be submitted from hello Hadoop Command Line console on hello head node of hello Hadoop cluster.</span></span> <span data-ttu-id="51fa5-117">toodo, přihlaste se k hlavnímu uzlu clusteru Hadoop hello hello otevře hello konzoly Hadoop příkazového řádku a odesílání dotazů Hive hello odtud.</span><span class="sxs-lookup"><span data-stu-id="51fa5-117">toodo this, log into hello head node of hello Hadoop cluster, open hello Hadoop Command Line console, and submit hello Hive queries from there.</span></span> <span data-ttu-id="51fa5-118">Pokyny v odesílání dotazů Hive v konzole hello Hadoop příkazového řádku najdete v tématu [jak tooSubmit dotazů Hive](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="51fa5-118">For instructions on submitting Hive queries in hello Hadoop Command Line console, see [How tooSubmit Hive Queries](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

## <span data-ttu-id="51fa5-119"><a name="uniform"></a>Uniform náhodné vzorkování</span><span class="sxs-lookup"><span data-stu-id="51fa5-119"><a name="uniform"></a> Uniform random sampling</span></span>
<span data-ttu-id="51fa5-120">Uniform náhodné vzorkování znamená, že každý řádek v sadě dat hello má stejnou šanci se vzorků.</span><span class="sxs-lookup"><span data-stu-id="51fa5-120">Uniform random sampling means that each row in hello data set has an equal chance of being sampled.</span></span> <span data-ttu-id="51fa5-121">To může být implementováno přidáním další pole rand() toohello datové sady v informacích o vnitřní dotazu "Vyberte" hello a hello vnější "Vyberte" dotaz na toto pole náhodných tuto podmínku.</span><span class="sxs-lookup"><span data-stu-id="51fa5-121">This can be implemented by adding an extra field rand() toohello data set in hello inner "select" query, and in hello outer "select" query that condition on that random field.</span></span>

<span data-ttu-id="51fa5-122">Tady je příklad dotazu:</span><span class="sxs-lookup"><span data-stu-id="51fa5-122">Here is an example query:</span></span>

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

<span data-ttu-id="51fa5-123">Zde `<sample rate, 0-1>` určuje hello podíl záznamy, uživatelé hello chcete toosample.</span><span class="sxs-lookup"><span data-stu-id="51fa5-123">Here, `<sample rate, 0-1>` specifies hello proportion of records that hello users want toosample.</span></span>

## <span data-ttu-id="51fa5-124"><a name="group"></a>Náhodné vzorkování podle skupin</span><span class="sxs-lookup"><span data-stu-id="51fa5-124"><a name="group"></a> Random sampling by groups</span></span>
<span data-ttu-id="51fa5-125">Když vzorkování kategorizovaná data, může být vhodné tooeither zahrnout nebo vyloučit všechny instance hello určité konkrétní hodnoty kategorií proměnné.</span><span class="sxs-lookup"><span data-stu-id="51fa5-125">When sampling categorical data, you may want tooeither include or exclude all of hello instances of some particular value of a categorical variable.</span></span> <span data-ttu-id="51fa5-126">Toto je pojmy "vzorkování skupinou".</span><span class="sxs-lookup"><span data-stu-id="51fa5-126">This is what is meant by "sampling by group".</span></span>
<span data-ttu-id="51fa5-127">Například pokud máte kategorií proměnné "Stavu", která obsahuje hodnoty NY, MA, certifikační Autority, ni, PA atd, záznamy hello stejného stavu se vždy společně, jestli jsou vzorkovat nebo ne.</span><span class="sxs-lookup"><span data-stu-id="51fa5-127">For example, if you have a categorical variable "State", which has values NY, MA, CA, NJ, PA, etc, you want records of hello same state be always together, whether they are sampled or not.</span></span>

<span data-ttu-id="51fa5-128">Tady je příklad dotazu této ukázky podle skupiny:</span><span class="sxs-lookup"><span data-stu-id="51fa5-128">Here is an example query that samples by group:</span></span>

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <span data-ttu-id="51fa5-129"><a name="stratified"></a>Vrstveného vzorkování</span><span class="sxs-lookup"><span data-stu-id="51fa5-129"><a name="stratified"></a>Stratified sampling</span></span>
<span data-ttu-id="51fa5-130">S ohledem tooa kategorií proměnnou Pokud hello ukázky získat hodnoty to kategorií v hello je si náhodné vzorkování stejné poměr jako hello nadřazené naplnění, ze které hello byly získány ukázky.</span><span class="sxs-lookup"><span data-stu-id="51fa5-130">Random sampling is stratified with respect tooa categorical variable when hello samples obtained have values of that categorical that are in hello same ratio as in hello parent population from which hello samples were obtained.</span></span> <span data-ttu-id="51fa5-131">Pomocí hello stejné příklad, jak je uvedeno výše, předpokládá se, že data obsahují dílčí plnění státy, vyslovte ni má 100 připomínky, NY má 60 připomínky a WA má 300 připomínky.</span><span class="sxs-lookup"><span data-stu-id="51fa5-131">Using hello same example as above, suppose your data has sub-populations by states, say NJ has 100 observations, NY has 60 observations, and WA has 300 observations.</span></span> <span data-ttu-id="51fa5-132">Pokud zadáte rychlost hello si vzorkování toobe 0,5, pak hello ukázka získat by měl mít přibližně 50, 30 a 150 připomínky ni, NY a WA v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="51fa5-132">If you specify hello rate of stratified sampling toobe 0.5, then hello sample obtained should have approximately 50, 30, and 150 observations of NJ, NY, and WA respectively.</span></span>

<span data-ttu-id="51fa5-133">Tady je příklad dotazu:</span><span class="sxs-lookup"><span data-stu-id="51fa5-133">Here is an example query:</span></span>

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
              rank() over (partition by state order by rand()) as state_rank
          from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


<span data-ttu-id="51fa5-134">Informace pro pokročilejší metody vzorkování, které jsou k dispozici v Hive naleznete v tématu [LanguageManual vzorkování](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).</span><span class="sxs-lookup"><span data-stu-id="51fa5-134">For information on more advanced sampling methods that are available in Hive, see [LanguageManual Sampling](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).</span></span>

