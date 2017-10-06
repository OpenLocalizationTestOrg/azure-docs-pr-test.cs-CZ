---
title: "Příklady dotazů aaaLucene pro službu Azure Search | Microsoft Docs"
description: "Syntaxe dotazů Lucene přibližné vyhledávání, vyhledávání blízkých výrazů, zvyšovat skóre termín, regulární výraz vyhledávání a hledání pomocí zástupných znaků."
services: search
documentationcenter: 
author: LiamCa
manager: pablocas
editor: 
tags: Lucene query analyzer syntax
ms.assetid: 147f360d-a5ce-4d7b-a909-c8b65bfb748c
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/21/2017
ms.author: liamca
ms.openlocfilehash: d859cf697ef9485a49e9e0591b68e812ffa55f1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Příklady syntaxe dotazů Lucene pro tvorbu dotazů ve službě Azure Search
Při vytváření dotazů pro službu Azure Search, můžete použít buď výchozí hello [jednoduchá syntaxe dotazů](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) nebo alternativní hello [analyzátor dotazů Lucene ve službě Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search). Hello analyzátor dotazů Lucene podporuje složitější konstruktory dotazu, jako jsou dotazy v rámci pole, přibližné vyhledávání, vyhledávání blízkých výrazů, termín zvyšovat skóre a hledání regulárního výrazu.

V tomto článku můžete krokovat příklady demonstrující operace dotazů, které jsou k dispozici při použití úplnou syntaxí hello.

## <a name="viewing-hello-examples-in-jsfiddle"></a>Zobrazení hello příklady v JSFiddle

Všechny příklady hello v tomto článku jsou spustitelné dotazy, které spouštění předem načtený index vyhledávání v [JSFiddle](https://jsfiddle.net), editoru online kód pro testování skriptu a HTML. 

toorun, klikněte pravým tlačítkem na hello dotazu Příklad adresy URL tooopen JSFiddle v samostatném okně prohlížeče.

> [!NOTE]
> Hello následující příklady využít indexu vyhledávání, který se skládá z úloh k dispozici v datové sadě poskytované hello [OpenData New Yorku města](https://nycopendata.socrata.com/) initiative. Tato data by se neměla považovat aktuální nebo dokončení. Hello index je na izolovaného prostoru služby poskytované společností Microsoft. Není nutné předplatného Azure nebo Azure Search tootry tyto dotazy.
>


## <a name="how-tooinvoke-full-lucene-parsing"></a>Jak tooinvoke úplné Lucene analýza

Všechny příklady hello v tomto článku zadejte hello **typ = úplné** parametr, označující úplnou syntaxí této hello, zpracovává hello analyzátor dotazů Lucene vyhledávání. 

**Příklad 1** – hello klikněte pravým tlačítkem na následující tooopen fragment dotazu se v prohlížeči nové stránky, která načte JSFiddle a spustí hello dotazu:

* [& Typ = úplné & hledání = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

V hello nové okno prohlížeče jsou hello JavaScript zdroje a výstupu protokolu HTML zobrazí vedle sebe. skript Hello odkazuje na celý dotaz (ne jenom hello fragment, jak je znázorněno v odkazu hello). Hello celý dotaz se zobrazí v hello adresy URL pro každý příklad. 

Tento dotaz vrací dokumenty z New Yorku úlohy index (nycjobs načíst izolovaného prostoru služby). Jako stručný výtah hello dotaz Určuje, že se vrátí jenom obchodní názvy. úplné podkladového dotazu Hello vypadá takto:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

Hello **searchFields** parametr omezuje hello vyhledávání toojust hello firmy název pole. Hello **typ** je nastaven příliš**úplné**, což dává pokyn Azure Search toouse hello analyzátor dotazů Lucene pro tento dotaz.

> [!NOTE]
> Pro informace o zpracování dotazu, viz [jak úplné textové vyhledávání funguje ve službě Azure Search](search-lucene-query-architecture.md). Další informace o parametry hledání najdete v tématu [vyhledávání dokumentů (služby REST API Azure Search)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).
>

### <a name="fielded-query-operation"></a>Operace fielded dotazů
Můžete upravit zadáním hello příklady v tomto článku **fieldname:searchterm** konstrukce toodefine operace fielded dotazů, kde hello pole je jednoho slova a hello hledaný termín je také slovo nebo frázi, Volitelně můžete s logickými operátory. Mezi příklady patří hello následující:

* business_title:(senior NOT junior)
* Stav: ("Brno" a "Nové Jersey")

Být zda tooput více řetězce v uvozovkách, pokud chcete, aby oba řetězce toobe vyhodnotit jako jedna entita, jako v tomto případě hledání dvě odlišné města pole umístění hello. Také se ujistěte, operátor hello je velkými písmeny, jak můžete vidět s není a AND.

zadaný v poli Hello **fieldname:searchterm** musí být prohledávatelné pole. V tématu [Create Index (služby REST API Azure Search)](https://docs.microsoft.com/rest/api/searchservice/create-index) podrobnosti o tom, jak se používají atributy indexu v definice polí.

**Příklad 2** – klikněte pravým tlačítkem na hello následující dotaz fragment kódu tento dotaz hledá obchodní produkty s hello termín senior v nich, ale ne méně zkušení:

* [& Typ = úplné & hledání = business_title:senior není méně zkušení](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="fuzzy-search-example"></a>Příklad přibližné vyhledávání
Vyhledá přibližné vyhledávání shody v podmínkách, které mají podobné konstrukce. Za [Lucene dokumentace](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), přibližné vyhledávání jsou založené na [Damerau Levenshtein vzdálenost](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

toodo přibližné vyhledávání připojit tildou hello "~" symbol na konci hello jednoho slova s volitelný parametr, hodnotu mezi 0 a 2, která určuje vzdálenost upravit hello. Například "blue ~" nebo "blue ~ 1" by vrátit blue, modré a pojidlem.

**Příklad 3** --klikněte pravým tlačítkem na hello následující dotaz fragment kódu. Tento dotaz vyhledává úlohy se přidružení termín hello (kde je je zadáno chybně):

* [& Typ = úplné & hledání = business_title:asosiate ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

> [!Note]
> Přibližné dotazy nejsou [analyzovali](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis), což může být překvapivé, pokud očekáváte, rozklad nebo Lematizace. Lexikální analýzy se provádí pouze na dokončení podmínky (dotazu termín nebo frázi dotazu). Typy dotazů s podmínkami nekompletní (předponu dotazu, dotaz zástupný znak, regulární výraz dotazu, přibližné dotazu) se přidají přímo toohello dotazu stromu obcházení hello analysis fáze. Hello pouze transformace provést podle podmínek neúplné dotazu je předpoklady.
>

## <a name="proximity-search-example"></a>Příklad hledání bezkontaktní komunikace
Hledání blízkosti jsou použité toofind termíny, které jsou blízko sebe v dokumentu. Vložit tildou "~" na konci hello fráze následuje hello počet slova, která vytvoření hranice blízkosti hello. Například "hotelů letiště" ~ 5 najdete hello podmínky ubytovací a letiště v rámci 5 slova vzájemně v dokumentu.

**Příklad 4** – klikněte pravým tlačítkem na hello dotazu. Hledání pro úlohy s hello termín "senior analytik", kde jsou oddělené oddělovačem více než jedno slovo:

* [& Typ = úplné & hledání = business_title: "senior analytik" ~ 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Příklad 5** – zkuste ji znovu odebrat hello slova mezi hello termín "senior analytik".

* [& Typ = úplné & hledání = business_title: "senior analytik" ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting-examples"></a>Termín zvyšovat skóre příklady
Termín zvyšovat skóre odkazuje tooranking vyšší, pokud obsahuje hello dokumentu boosted termín, relativní toodocuments, které neobsahují termín hello. To se liší od vyhodnocování profily, profily vyhodnocování zvýšení určitá pole, nikoli konkrétní podmínky. Hello následující příklad pomáhá ilustrují hello rozdíly.

Vezměte v úvahu vyhodnocování profil, který zvyšuje odpovídá určitá pole, jako například **genre** v příkladu musicstoreindex hello. Zvyšovat skóre podmínek může být použité toofurther nárůst určité podmínky vyhledávání vyšší než jiné. Například "rock ^ 2 elektronické" bude zvýšení dokumentů, které obsahují hello hledaný text v hello **genre** pole vyšší než ostatní prohledávatelné pole v indexu hello. Kromě toho dokumentů, které obsahují hello hledaný termín "rock" se určí vyšší než hello jiných hledaný termín "elektronického" v důsledku hello termín nárůst hodnotu (2).

tooboost zadat termín, použijte hello pomocí kurzoru, "^", symbol s faktorem zesílení (číslo) na konci hello hello podmínek, které hledáte. Hello vyšší faktor zesílení hello hello relevantnější termín hello bude relativní tooother hledaný text. Ve výchozím nastavení je hello faktor zesílení 1. I když faktor zesílení hello musí být kladná, může být menší než 1 (například 0,2).

**Příklad 6** – klikněte pravým tlačítkem na hello dotazu. Vyhledejte úlohy se hello termín "počítač analytik", kde vidíte nebyly nalezeny žádné výsledky s počítači slova a analytických ještě analytik úlohy jsou v horní části hello hello výsledků.

* [& Typ = úplné & hledání = business_title:computer analytika](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Příklad 7** – zkuste to znovu, tento čas zvyšovat skóre výsledků s počítačem termín hello přes hello termín analytik pokud obě slova nejsou k dispozici.

* [& Typ = úplné & hledání = business_title:computer ^ 2 analytika](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression-example"></a>Příklad regulární výraz
Hledání regulárního výrazu najde shoda na základě hello obsahu mezi lomítka "/", jako zdokumentovaný v hello [Třída RegExp](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

**Příklad 8** – klikněte pravým tlačítkem na hello dotazu. Vyhledejte úlohy se termín hello vyšší nebo nižší.

* `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

Hello adresy URL v tomto příkladu nebude správně vykreslovat stránku hello. Jako alternativní řešení zkopírujte následující adresu URL hello a vložte jej do adresu URL prohlížeče hello:`http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`

## <a name="wildcard-search-example"></a>Příklad hledání zástupný znak
Obecně rozpoznaná syntaxe můžete použít pro více (\*) nebo jednoduchého vyhledávání pomocí zástupných znaků znaku (?). Všimněte si dotazů Lucene hello analyzátor podporuje hello těchto symbolů s jeden termín a ne frázi.

**Příklad 9** – klikněte pravým tlačítkem na hello dotazu. Vyhledejte úlohy, které obsahují hello předponu "programové, což by mělo zahrnovat obchodní názvy s podmínkami hello programování a programátory v ní.

* [& Typ = úplné & $select = business_title & hledání = business_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26queryType=full%26$select=business_title%26search=business_title:prog*)

Nelze použít * nebo? symbol jako první znak hello hledání.

## <a name="next-steps"></a>Další kroky
Zkuste zadat hello analyzátor dotazů Lucene ve vašem kódu. Hello následující odkazy popisují, jak tooset až vyhledávání dotazuje na rozhraní .NET a hello REST API. odkazy Hello syntaxí hello výchozí jednoduchý, budete potřebovat tooapply zkušenosti z tohoto článku toospecify hello **typ**.

* [Dotazování indexu Azure Search pomocí .NET SDK hello](search-query-dotnet.md)
* [Dotazování indexu Azure Search pomocí REST API hello](search-query-rest-api.md)

## <a name="see-also"></a>Viz také

 [Jak úplné textové vyhledávání funguje ve službě Azure Search](search-lucene-query-architecture.md)