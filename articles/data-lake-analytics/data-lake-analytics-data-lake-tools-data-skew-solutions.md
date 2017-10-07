---
title: "problémy aaaResolve zkosení dat pomocí nástroje Azure Data Lake pro Visual Studio | Microsoft Docs"
description: "Řešení potíží s možná řešení problémů zkosení dat pomocí nástroje Azure Data Lake pro Visual Studio."
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/16/2016
ms.author: yanacai
ms.openlocfilehash: 3909fbd89eb40f061268cb7128f7fa84a3c33de7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resolve-data-skew-problems-by-using-azure-data-lake-tools-for-visual-studio"></a>Data zkosení problémy můžete vyřešit pomocí nástroje Azure Data Lake pro Visual Studio

## <a name="what-is-data-skew"></a>Co je dat zkreslit?

Stručně jsme uvedli, zkosení dat je přepsání reprezentována hodnota. Představte si, zda jste přiřadili 50 referentů daň tooaudit daň vrátí, jeden revizor pro každý stav USA. referentů Wyoming Hello, protože existuje naplnění hello je malý, má málo toodo. V kalifornské ale revizor hello je udržováno velmi zaneprázdněny z důvodu stavu hello velké naplnění.
    ![Příklad dat zkosení problém](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)

V našem scénáři hello nerovnoměrně distribuci dat mezi všechny daň kontrolorů, což znamená, že některé referentů musí fungovat více než jiné. Při vlastní úlohu často docházet situacích jako hello daň revizor zde ukázkový. V další technické podmínky získá jednoho vrcholu mnohem víc dat než jeho partnerské uzly situaci, která vytváří hello Vrchol pracovní více než jiné – které nakonec zpomaluje celé úlohy hello. Co je zhoršení, hello úlohy se nemusí podařit, protože může mít vrcholy, například 5 hodin runtime omezení a omezení 6 GB paměti.

## <a name="resolving-data-skew-problems"></a>Řešení problémů s zkosení dat

Azure nástrojů Data Lake pro Visual Studio může pomoct zjistit, jestli vaše úlohy došlo k problému s zkosení data. Pokud existuje problém, abyste ho mohli vyřešit tak, že zkusíte hello řešení v této části.

## <a name="solution-1-improve-table-partitioning"></a>Řešení 1: Zlepšení, vytváření oddílů tabulky

### <a name="option-1-filter-hello-skewed-key-value-in-advance"></a>Možnost 1: Filtr hello nesouměrně rozdělí hodnota klíče předem

Pokud nemá vliv na obchodní logiku, můžete předem filtrovat hello vyšší frekvence hodnot. Například pokud je celá řada 000 000 000 ve sloupci identifikátoru GUID, nemusí chcete tooaggregate tuto hodnotu. Předtím, než agregační, můžete napsat "kde GUID! ="000 000 000"" toofilter hello vysoká frekvence hodnotu.

### <a name="option-2-pick-a-different-partition-or-distribution-key"></a>Možnost 2: Vyberte jiný klíč oddílu, nebo distribuce

V předchozím příkladu hello Pokud chcete pouze toocheck hello daň auditu zatížení všechny přes hello země, distribuci dat hello můžete zvýšit tak, že vyberete hello identifikační číslo jako klíč. Výběr jiný oddíl nebo distribučního klíče můžete někdy distribuovat hello dat více rovnoměrně, ale potřebujete toomake jistotu, že tato volba nemá vliv obchodní logiky. Například toocalculate hello daň součet pro každý stav, můžete toodesignate _stavu_ jako klíč oddílu hello. Pokud budete pokračovat tooexperience tento problém, použijte možnost 3.

### <a name="option-3-add-more-partition-or-distribution-keys"></a>Možnost 3: Přidejte další oddíl nebo distribučního klíče

Místo použití pouze _stavu_ jako klíč oddílu, můžete použít více než jeden klíč pro dělení. Zvažte například přidávání _PSČ_ jako další oddíl klíče tooreduce data-partition velikosti a více rovnoměrně distribuovat hello data.

### <a name="option-4-use-round-robin-distribution"></a>Možnost 4: Použití distribučních kruhového dotazování

Pokud nemůžete najít příslušný klíč pro oddíl a distribuci, můžete zkusit distribuční toouse kruhového dotazování. Kruhové dotazování distribuční zpracovává všechny řádky stejně a náhodně umístí je do odpovídajících intervalů. Získá rovnoměrně Hello data, ale ztratí polohu informace, nevýhodou, který může také snížit výkon úlohy pro některé operace. Kromě toho agregace pro klíč zkreslilo hello Chcete přesto provést, se uchová hello data zkosení problém. toolearn Další informace o distribuci kruhového dotazování, distribuce tabulky hello U-SQL najdete v článku tématu [CREATE TABLE (U-SQL): vytvoření tabulky se schématem](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).

## <a name="solution-2-improve-hello-query-plan"></a>Řešení 2: Plán dotazu hello zlepšit

### <a name="option-1-use-hello-create-statistics-statement"></a>Možnost 1: Příkaz CREATE STATISTICS hello použít

U-SQL obsahuje příkaz CREATE STATISTICS hello tabulky. Tento příkaz dává Optimalizátor dotazů toohello Další informace o hello vlastností dat, třeba hodnotu rozdělení, které jsou uložené v tabulce. Pro většinu dotazů Optimalizátor dotazů hello již generuje hello nezbytné statistiku pro plán dotazu vysoké kvality. V některých případech může být nutné tooimprove výkon dotazů, tak, že vytvoříte další statistiky s CREATE STATISTICS nebo změnou návrhu dotazu hello. Další informace najdete v tématu hello [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) stránky.

Příklad kódu:

    CREATE STATISTICS IF NOT EXISTS stats_SampleTable_date ON SampleDB.dbo.SampleTable(date) WITH FULLSCAN;

>[!NOTE]
>Statistické informace se automaticky neaktualizuje. Pokud aktualizujete hello data v tabulce bez nutnosti znovu vytvářet hello statistiky, může odmítnout výkon dotazů hello.

### <a name="option-2-use-skewfactor"></a>Možnost 2: Použijte SKEWFACTOR

Pokud chcete toosum hello daň pro každý stav, je nutné použít Seskupit podle stavu, postup, který není vyhnout potížím data zkosení hello. Však může poskytnout nápovědu dat ve vaší tooidentify dotazu, který data zkreslit v klíče, aby hello Optimalizátor můžete připravit plán spuštění za vás.

Obvykle můžete nastavit parametr hello jako 0,5 a 1, s 0,5 znamená velkou zkosení nevěnuje význam zkosení a 1. Protože pomocný parametr hello ovlivňuje plán spuštění optimalizace pro aktuální příkaz hello a všechny podřízené příkazy, před zda pomocný parametr tooadd hello hello potenciální nesouměrně rozdělí key-wise agregace.

    SKEWFACTOR (columns) = x

    Provides a hint that hello given columns have a skew factor x from 0 (no skew) through 1 (very heavy skew).

Příklad kódu:

    //Add a SKEWFACTOR hint.
    @Impressions =
        SELECT * FROM
        searchDM.SML.PageView(@start, @end) AS PageView
        OPTION(SKEWFACTOR(Query)=0.5)
        ;

    //Query 1 for key: Query, ClientId
    @Sessions =
        SELECT
            ClientId,
            Query,
            SUM(PageClicks) AS Clicks
        FROM
            @Impressions
        GROUP BY
            Query, ClientId
        ;

    //Query 2 for Key: Query
    @Display =
        SELECT * FROM @Sessions
            INNER JOIN @Campaigns
                ON @Sessions.Query == @Campaigns.Query
        ;   

### <a name="option-3-use-rowcount"></a>Možnost 3: Použití počtu řádků  
Kromě toho tooSKEWFACTOR pro konkrétního klíče nesouměrně rozdělí připojit případech, pokud víte, že hello ostatní sady řádků připojené k je malá, se dá zjistit hello Optimalizátor přidáním pomocný parametr počtu řádků v příkazu U-SQL hello před spojení. Tímto způsobem Optimalizátor můžete vybrat všesměrového vysílání spojení strategie toohelp zlepšit výkon. Uvědomte si, že atribut ROWCOUNT hello data zkosení problém nevyřeší, ale může nabídnout další pomoc.

    OPTION(ROWCOUNT = n)

    Identify a small row set before JOIN by providing an estimated integer row count.

Příklad kódu:

    //Unstructured (24-hour daily log impressions)
    @Huge   = EXTRACT ClientId int, ...
                FROM @"wasb://ads@wcentralus/2015/10/30/{*}.nif"
                ;

    //Small subset (that is, ForgetMe opt out)
    @Small  = SELECT * FROM @Huge
                WHERE Bing.ForgetMe(x,y,z)
                OPTION(ROWCOUNT=500)
                ;

    //Result (not enough information toodetermine simple broadcast JOIN)
    @Remove = SELECT * FROM Bing.Sessions
                INNER JOIN @Small ON Sessions.Client == @Small.Client
                ;

## <a name="solution-3-improve-hello-user-defined-reducer-and-combiner"></a>Řešení 3: Zlepšení hello uživatelem definované reduktorem a kombinační

Někdy může zapisovat toodeal uživatelem definovaný operátor s logikou složitý proces a kvalitně reduktorem a kombinační může zmírnit data zkosení problém v některých případech.

### <a name="option-1-use-a-recursive-reducer-if-possible"></a>Možnost 1: Použití rekurzivní reduktorem, pokud je to možné

Ve výchozím nastavení uživatelem definované reduktorem běží v režimu tohoto nerekurzivního, což znamená, že snížit pracovní pro klíč je distribuován do jednoho vrcholu. Ale pokud vaše data se nesouměrně rozdělí, hello obrovských sad dat mohou být zpracovány v jednom vrchol a spustit po dlouhou dobu.

tooimprove výkon, můžete přidat atribut ve vašem kódu toodefine reduktorem toorun v režimu rekurzivní. Potom obrovských sad dat hello dají distribuované toomultiple vrcholy a spouštět souběžně, což urychlí úlohu.

toochange toorecursive reduktorem tohoto nerekurzivního, musíte se, že je vaše algoritmus asociativní toomake. Například součet hello je asociativní a hello Medián není. Musíte taky toomake jistotu, že hello vstup a výstup reduktorem zachovat hello stejné schéma.

Atribut rekurzivní reduktorem:

    [SqlUserDefinedReducer(IsRecursive = true)]

Příklad kódu:

    [SqlUserDefinedReducer(IsRecursive = true)]
    public class TopNReducer : IReducer
    {
        public override IEnumerable<IRow>
            Reduce(IRowset input, IUpdatableRow output)
        {
            //Your reducer code goes here.
        }
    }

### <a name="option-2-use-row-level-combiner-mode-if-possible"></a>Možnost 2: Použití režimu kombinační na úrovni řádků, pokud je to možné

Podobně jako toohello ROWCOUNT nápovědu pro konkrétní připojení k nesouměrně rozdělí klíč případů, kombinační režimu pokusí toodistribute obrovské nesouměrně rozdělí klíč hodnota nastaví toomultiple vrcholy tak, aby pracovní hello mohou být provedeny souběžně. Režim kombinační nelze vyřešit problémy zkosení data, ale může nabídnout některé další nápovědu k nastaví velký nesouměrně rozdělí klíč hodnota.

Ve výchozím režimu kombinační hello je úplná, což znamená, že hello zbývajících sady řádků a nemůže být oddělené pravého řádku sady. Nastavení režimu hello jako doleva nebo doprava nebo vnitřní umožňuje připojení k úrovni řádků. systém Hello odděluje hello odpovídající řádek sady a distribuuje je do více vrcholy, které spustit souběžně. Než začnete konfigurovat hello kombinační režimu, dávejte ale pozor, že je možné oddělit tooensure, který hello odpovídající sady řádků.

Příklad Hello zobrazuje sadu oddělených levého řádku. Každý řádek výstupu závisí na jeden vstupní řádek zleva hello a potenciálně závisí na všechny řádky z hello přímo s hello stejnou hodnotu klíče. Pokud nastavíte režim kombinační hello jako vlevo, hello systému odděluje hello obrovské doleva-sada řádků do malé a přiřadí je toomultiple vrcholy.

![Obrázek kombinační režimu](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/combiner-mode-illustration.png)

>[!NOTE]
>Pokud jste nastavili hello nesprávný kombinační režimu, kombinace hello sice méně efektivní a hello výsledky mohou být další potíže.

Atributy kombinační režimu:

- [SqlUserDefinedCombiner(Mode=CombinerMode.Full)]: Every output row potentially depends on all hello input rows from left and right with hello same key value.

- SqlUserDefinedCombiner(Mode=CombinerMode.Left): Každý řádek výstupu závisí na jednom řádku vstupní zleva hello (a potenciálně všechny řádky z hello přímo s hello stejnou hodnotu klíče).

- qlUserDefinedCombiner(Mode=CombinerMode.Right): každý řádek výstupu závisí na jeden vstupní řádek z hello vpravo (a potenciálně všechny řádky z levé hello s hello stejnou hodnotu klíče).

- SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Každý řádek výstupu závisí na jeden vstupní řádek z hello vlevo a hello přímo s hello stejnou hodnotu.

Příklad kódu:

    [SqlUserDefinedCombiner(Mode = CombinerMode.Right)]
    public class WatsonDedupCombiner : ICombiner
    {
        public override IEnumerable<IRow>
            Combine(IRowset left, IRowset right, IUpdatableRow output)
        {
        //Your combiner code goes here.
        }
    }
