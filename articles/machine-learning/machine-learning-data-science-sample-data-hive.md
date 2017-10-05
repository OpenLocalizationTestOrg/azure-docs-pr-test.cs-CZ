---
title: "Ukázková data v tabulkách Azure HDInsight Hive | Microsoft Docs"
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
ms.openlocfilehash: d46297dfaf85976114fbf610803e5f1a997041e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a><span data-ttu-id="5ec83-103">Ukázková data v tabulkách Azure HDInsight Hive</span><span class="sxs-lookup"><span data-stu-id="5ec83-103">Sample data in Azure HDInsight Hive tables</span></span>
<span data-ttu-id="5ec83-104">V tomto článku jsme popisují, jak nižší sample data uložená v Azure HDInsight Hive tabulky pomocí dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="5ec83-104">In this article, we describe how to down-sample data stored in Azure HDInsight Hive tables using Hive queries.</span></span> <span data-ttu-id="5ec83-105">Nemůžeme popisuje tři metody vzorkování často se používá použít:</span><span class="sxs-lookup"><span data-stu-id="5ec83-105">We cover three popularly used sampling methods:</span></span>

* <span data-ttu-id="5ec83-106">Uniform náhodné vzorkování</span><span class="sxs-lookup"><span data-stu-id="5ec83-106">Uniform random sampling</span></span>
* <span data-ttu-id="5ec83-107">Náhodné vzorkování podle skupin</span><span class="sxs-lookup"><span data-stu-id="5ec83-107">Random sampling by groups</span></span>
* <span data-ttu-id="5ec83-108">Vrstveného vzorkování</span><span class="sxs-lookup"><span data-stu-id="5ec83-108">Stratified sampling</span></span>

<span data-ttu-id="5ec83-109">Následující **nabídky** odkazy na témata, které popisují, jak ukázková data z různých prostředích úložiště.</span><span class="sxs-lookup"><span data-stu-id="5ec83-109">The following **menu** links to topics that describe how to sample data from various storage environments.</span></span>

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="5ec83-110">**Proč ukázková data?**</span><span class="sxs-lookup"><span data-stu-id="5ec83-110">**Why sample your data?**</span></span>
<span data-ttu-id="5ec83-111">Pokud je velké datové sady, které chcete analyzovat, je obvykle vhodné nižší ukázková data, která mají snížit velikost menší, ale reprezentativní a lepší správu bitlockeru.</span><span class="sxs-lookup"><span data-stu-id="5ec83-111">If the dataset you plan to analyze is large, it's usually a good idea to down-sample the data to reduce it to a smaller but representative and more manageable size.</span></span> <span data-ttu-id="5ec83-112">To usnadňuje pochopení dat, zkoumání a funkce inženýrství.</span><span class="sxs-lookup"><span data-stu-id="5ec83-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="5ec83-113">Jeho role v procesu Team datové vědy je umožnit rychlé vytváření prototypů zpracování dat funkcí a modelů strojového učení.</span><span class="sxs-lookup"><span data-stu-id="5ec83-113">Its role in the Team Data Science Process is to enable fast prototyping of the data processing functions and machine learning models.</span></span>

<span data-ttu-id="5ec83-114">Tato úloha vzorkování je krok v [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="5ec83-114">This sampling task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="how-to-submit-hive-queries"></a><span data-ttu-id="5ec83-115">Postup odesílání dotazů Hive</span><span class="sxs-lookup"><span data-stu-id="5ec83-115">How to submit Hive queries</span></span>
<span data-ttu-id="5ec83-116">Z konzoly Hadoop příkazový řádek z hlavního uzlu clusteru Hadoop lze odesílat dotazy Hive.</span><span class="sxs-lookup"><span data-stu-id="5ec83-116">Hive queries can be submitted from the Hadoop Command Line console on the head node of the Hadoop cluster.</span></span> <span data-ttu-id="5ec83-117">K tomu, přihlaste se k hlavnímu uzlu clusteru Hadoop, otevřete konzolu nástroje příkazového řádku Hadoop a odesílání dotazů Hive z ní.</span><span class="sxs-lookup"><span data-stu-id="5ec83-117">To do this, log into the head node of the Hadoop cluster, open the Hadoop Command Line console, and submit the Hive queries from there.</span></span> <span data-ttu-id="5ec83-118">Pokyny k odesílání dotazů Hive v konzole nástroje příkazového řádku Hadoop, najdete v části [postup odesílání dotazů Hive](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="5ec83-118">For instructions on submitting Hive queries in the Hadoop Command Line console, see [How to Submit Hive Queries](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

## <span data-ttu-id="5ec83-119"><a name="uniform"></a>Uniform náhodné vzorkování</span><span class="sxs-lookup"><span data-stu-id="5ec83-119"><a name="uniform"></a> Uniform random sampling</span></span>
<span data-ttu-id="5ec83-120">Uniform náhodné vzorkování znamená, že každý řádek v sadě dat má stejnou šanci se vzorků.</span><span class="sxs-lookup"><span data-stu-id="5ec83-120">Uniform random sampling means that each row in the data set has an equal chance of being sampled.</span></span> <span data-ttu-id="5ec83-121">To lze provést přidáním další pole rand() se sada dat v informacích o vnitřní dotaz "Vyberte" a v vnější "Vyberte" dotaz tuto podmínku na tomto náhodných poli.</span><span class="sxs-lookup"><span data-stu-id="5ec83-121">This can be implemented by adding an extra field rand() to the data set in the inner "select" query, and in the outer "select" query that condition on that random field.</span></span>

<span data-ttu-id="5ec83-122">Tady je příklad dotazu:</span><span class="sxs-lookup"><span data-stu-id="5ec83-122">Here is an example query:</span></span>

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

<span data-ttu-id="5ec83-123">Zde `<sample rate, 0-1>` Určuje poměr záznamy, které uživatelé má zkusit.</span><span class="sxs-lookup"><span data-stu-id="5ec83-123">Here, `<sample rate, 0-1>` specifies the proportion of records that the users want to sample.</span></span>

## <span data-ttu-id="5ec83-124"><a name="group"></a>Náhodné vzorkování podle skupin</span><span class="sxs-lookup"><span data-stu-id="5ec83-124"><a name="group"></a> Random sampling by groups</span></span>
<span data-ttu-id="5ec83-125">Když vzorkování kategorizovaná data, můžete zahrnout nebo vyloučit všechny instance určité konkrétní hodnoty kategorií proměnné.</span><span class="sxs-lookup"><span data-stu-id="5ec83-125">When sampling categorical data, you may want to either include or exclude all of the instances of some particular value of a categorical variable.</span></span> <span data-ttu-id="5ec83-126">Toto je pojmy "vzorkování skupinou".</span><span class="sxs-lookup"><span data-stu-id="5ec83-126">This is what is meant by "sampling by group".</span></span>
<span data-ttu-id="5ec83-127">Například pokud máte kategorií proměnné "Stavu", která obsahuje hodnoty NY, MA, certifikační Autority, ni, PA atd, chcete záznamů stejného stavu, vždy se společně, jestli jsou vzorkovat nebo ne.</span><span class="sxs-lookup"><span data-stu-id="5ec83-127">For example, if you have a categorical variable "State", which has values NY, MA, CA, NJ, PA, etc, you want records of the same state be always together, whether they are sampled or not.</span></span>

<span data-ttu-id="5ec83-128">Tady je příklad dotazu této ukázky podle skupiny:</span><span class="sxs-lookup"><span data-stu-id="5ec83-128">Here is an example query that samples by group:</span></span>

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

## <span data-ttu-id="5ec83-129"><a name="stratified"></a>Vrstveného vzorkování</span><span class="sxs-lookup"><span data-stu-id="5ec83-129"><a name="stratified"></a>Stratified sampling</span></span>
<span data-ttu-id="5ec83-130">Náhodné vzorkování je si s ohledem na proměnnou kategorií Pokud ukázky získat hodnoty, kategorií, jsou ve stejné poměr jako nadřazené naplnění, ze kterého byly získány ukázky.</span><span class="sxs-lookup"><span data-stu-id="5ec83-130">Random sampling is stratified with respect to a categorical variable when the samples obtained have values of that categorical that are in the same ratio as in the parent population from which the samples were obtained.</span></span> <span data-ttu-id="5ec83-131">Pomocí příkladě jako výš, Předpokládejme, že data obsahují dílčí plnění státy, můžete ni má 100 připomínky, NY má 60 připomínky a WA má 300 připomínky.</span><span class="sxs-lookup"><span data-stu-id="5ec83-131">Using the same example as above, suppose your data has sub-populations by states, say NJ has 100 observations, NY has 60 observations, and WA has 300 observations.</span></span> <span data-ttu-id="5ec83-132">Pokud zadáte rychlost vrstveného vzorkování být 0,5, pak ukázka získat by měl mít přibližně 50, 30 a 150 připomínky ni, NY a WA v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="5ec83-132">If you specify the rate of stratified sampling to be 0.5, then the sample obtained should have approximately 50, 30, and 150 observations of NJ, NY, and WA respectively.</span></span>

<span data-ttu-id="5ec83-133">Tady je příklad dotazu:</span><span class="sxs-lookup"><span data-stu-id="5ec83-133">Here is an example query:</span></span>

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


<span data-ttu-id="5ec83-134">Informace pro pokročilejší metody vzorkování, které jsou k dispozici v Hive naleznete v tématu [LanguageManual vzorkování](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).</span><span class="sxs-lookup"><span data-stu-id="5ec83-134">For information on more advanced sampling methods that are available in Hive, see [LanguageManual Sampling](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).</span></span>

