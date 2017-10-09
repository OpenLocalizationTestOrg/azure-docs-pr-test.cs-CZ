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
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Ukázková data v tabulkách Azure HDInsight Hive
V tomto článku jsme popisují, jak toodown ukázková data ukládat do tabulek Azure HDInsight Hive pomocí dotazů Hive. Nemůžeme popisuje tři metody vzorkování často se používá použít:

* Uniform náhodné vzorkování
* Náhodné vzorkování podle skupin
* Vrstveného vzorkování

Následující Hello **nabídky** odkazy tootopics, které popisují, jak toosample data z různých prostředích úložiště.

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

**Proč ukázková data?**
Pokud datovou sadu hello plánování tooanalyze velká, je obvykle vhodné hello toodown ukázková data tooreduce ho tooa menší, ale reprezentativní a lepší správu bitlockeru velikost. To usnadňuje pochopení dat, zkoumání a funkce inženýrství. Jeho role v hello proces vědecké účely Team dat je rychlé vytváření prototypů tooenable hello zpracování dat funkcí a modelů strojového učení.

Tato úloha vzorkování je krok v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="how-toosubmit-hive-queries"></a>Jak toosubmit dotazů Hive
Z příkazového řádku Hadoop konzoly hello hello hlavního uzlu clusteru Hadoop hello lze odesílat dotazy Hive. toodo, přihlaste se k hlavnímu uzlu clusteru Hadoop hello hello otevře hello konzoly Hadoop příkazového řádku a odesílání dotazů Hive hello odtud. Pokyny v odesílání dotazů Hive v konzole hello Hadoop příkazového řádku najdete v tématu [jak tooSubmit dotazů Hive](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="uniform"></a>Uniform náhodné vzorkování
Uniform náhodné vzorkování znamená, že každý řádek v sadě dat hello má stejnou šanci se vzorků. To může být implementováno přidáním další pole rand() toohello datové sady v informacích o vnitřní dotazu "Vyberte" hello a hello vnější "Vyberte" dotaz na toto pole náhodných tuto podmínku.

Tady je příklad dotazu:

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

Zde `<sample rate, 0-1>` určuje hello podíl záznamy, uživatelé hello chcete toosample.

## <a name="group"></a>Náhodné vzorkování podle skupin
Když vzorkování kategorizovaná data, může být vhodné tooeither zahrnout nebo vyloučit všechny instance hello určité konkrétní hodnoty kategorií proměnné. Toto je pojmy "vzorkování skupinou".
Například pokud máte kategorií proměnné "Stavu", která obsahuje hodnoty NY, MA, certifikační Autority, ni, PA atd, záznamy hello stejného stavu se vždy společně, jestli jsou vzorkovat nebo ne.

Tady je příklad dotazu této ukázky podle skupiny:

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

## <a name="stratified"></a>Vrstveného vzorkování
S ohledem tooa kategorií proměnnou Pokud hello ukázky získat hodnoty to kategorií v hello je si náhodné vzorkování stejné poměr jako hello nadřazené naplnění, ze které hello byly získány ukázky. Pomocí hello stejné příklad, jak je uvedeno výše, předpokládá se, že data obsahují dílčí plnění státy, vyslovte ni má 100 připomínky, NY má 60 připomínky a WA má 300 připomínky. Pokud zadáte rychlost hello si vzorkování toobe 0,5, pak hello ukázka získat by měl mít přibližně 50, 30 a 150 připomínky ni, NY a WA v uvedeném pořadí.

Tady je příklad dotazu:

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


Informace pro pokročilejší metody vzorkování, které jsou k dispozici v Hive naleznete v tématu [LanguageManual vzorkování](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).

