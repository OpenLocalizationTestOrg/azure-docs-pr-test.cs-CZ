---
title: aaaQuery indexu Azure Search | Microsoft Docs
description: "Sestavení vyhledávacího dotazu ve službě Azure search a pomocí vyhledávání parametrů toofilter a řazení výsledků vyhledávání."
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 69205d7a-363f-4b92-a53f-6ca818a3d2c7
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 04/26/2017
ms.author: ashmaka
ms.openlocfilehash: 4a5ffffe179695fc09446760e21a738dd36c29b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="query-your-azure-search-index"></a>Dotazování indexu Azure Search
> [!div class="op_single_selector"]
> * [Přehled](search-query-overview.md)
> * [Azure Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Při odesílání tooAzure požadavky search vyhledávání, existuje několik parametrů, které lze zadat vedle hello vlastních slov zadaných do vyhledávacího pole hello vaší aplikace. Tyto parametry dotazu vám umožňují tooachieve větší kontrolu nad hello [fulltextové vyhledávání prostředí](search-lucene-query-architecture.md).

Níže je seznam se stručným vysvětlením běžných použití parametrů dotazu hello ve službě Azure Search. Úplné vysvětlení parametrů dotazu a jejich chování naleznete v tématu hello podrobné stránky pro hello [REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) a [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters#microsoft_azure_search_models_searchparameters#properties_summary).

## <a name="types-of-queries"></a>Typy dotazů
Služba Azure Search nabízí mnoho možností toocreate velmi výkonných dotazů. jsou Hello dva hlavní typy dotazů, které budete používat `search` a `filter`. A `search` dotaz vyhledává jeden nebo více výrazů ve všech *prohledávatelné* polích v indexu a funguje hello způsob, jak jste zvyklí u vyhledávacích webů Google nebo Bing toowork. Dotaz `filter` vyhodnocuje logický výraz na všech *filtrovatelných* polích v indexu. Na rozdíl od `search` dotazy, `filter` porovnávají přesný obsah pole, to znamená, že jsou velká a malá písmena pro pole řetězce hello.

Vyhledávání a filtrování lze používat společně nebo samostatně. Pokud budete používat společně, filtr hello je použita první toohello celý index, a pak se provádí vyhledávání hello na výsledky hello hello filtru. Filtrů tak může být užitečné výkonu dotazu tooimprove zmenšením hello sadu dokumenty, které hello tooprocess musí dotaz vyhledávání.

Hello syntaxe pro výrazy filtru je podmnožinou hello [jazyka filtrování OData](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search). Pro vyhledávací dotazy můžete použít buď hello [zjednodušenou syntaxi](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) nebo hello [syntaxe dotazů Lucene](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) kterých pojednáváme níže.

### <a name="simple-query-syntax"></a>Jednoduchá syntaxe dotazů
Hello [jednoduchá syntaxe dotazů](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) je hello výchozím jazykem dotazů ve službě Azure Search. Hello jednoduchá syntaxe dotazů podporuje mnoho běžných operátorů hledání, včetně hello AND, OR, ne, fráze, přípony a operátorů.

### <a name="lucene-query-syntax"></a>Syntaxe dotazů Lucene
Hello [syntaxe dotazů Lucene](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) vám umožní toouse hello velmi rozšířený a jazyk výrazovou dotazů vyvinutý jako součást [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html).

Pomocí této syntaxe dotazů můžete dosáhnout tooeasily hello následující možnosti: [dotazy v rámci pole](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fields), [přibližné vyhledávání](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fuzzy), [vyhledávání blízkých výrazů](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_proximity), [ zvýšení skóre termínu](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_termboost), [hledání regulárního výrazu](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_regex), [hledání pomocí zástupných znaků](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_wildcard), [Základy syntaxe](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_syntax), a [dotazy s logické operátory](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_boolean).

## <a name="ordering-results"></a>Řazení výsledků
Když přijímáte výsledky vyhledávacího dotazu, můžete požádat, aby služba Azure Search vracela výsledky hello seřazené podle hodnot v určitém poli. Ve výchozím nastavení, služba Azure Search řadí výsledky hledání hello podle pořadí hello skóre vyhledávání každého dokumentu, který je odvozený od [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

Pokud chcete výsledky seřazené podle hodnoty místo skóre vyhledávání hello tooreturn Azure Search, můžete použít hello `orderby` parametr vyhledávání. Zadávat lze hodnotu hello hello `orderby` pole tooinclude parametru názvy a volá toohello [ `geo.distance()` funkce](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search) pro geoprostorové hodnoty. Každý výraz může následovat `asc` tooindicate, že výsledky jsou požadovány ve vzestupném pořadí, a `desc` tooindicate, že výsledky jsou požadovány v sestupném pořadí. používá výchozí Hello vzestupné pořadí.

## <a name="paging"></a>Stránkování
Služba Azure Search umožňuje snadno tooimplement stránkování výsledků vyhledávání. Pomocí hello `top` a `skip` parametry, můžete plynule vydávat vyhledávací požadavky, které vám umožňují tooreceive hello celkový sadu výsledků vyhledávání ve spravovatelných, seřazených podmnožinách, které umožňují dobré vyhledávání postupy v uživatelském rozhraní. Při získávání těchto menších podmnožin výsledků, můžete také získat hello počet dokumentů v hello celkový sadu výsledků vyhledávání.

Další informace o stránkování výsledků vyhledávání naleznete v článku hello [jak výsledky hledání toopage ve službě Azure Search](search-pagination-page-layout.md).

## <a name="hit-highlighting"></a>Zvýrazňování položek
Ve službě Azure Search, zdůraznění hello přesné části výsledků vyhledávání, které odpovídají hello vyhledávací dotaz je umožněno pomocí hello `highlight`, `highlightPreTag`, a `highlightPostTag` parametry. Můžete určit, které *prohledávatelné* polí má být odpovídající text zdůrazňují stejně vrátí hello přesný řetězec značky tooappend toohello počáteční a koncové hello odpovídající text tohoto Azure Search.

## <a name="try-out-query-syntax"></a>Vyzkoušení syntaxe dotazů

Hello nejlepší způsob, jak toounderstand rozdílů v syntaxi je odesílání dotazů a kontrolou výsledků.

+ Použití [Průzkumník služby Search](search-explorer.md) v hello portálu Azure. Nasazením [hello ukázkový index](search-get-started-portal.md), můžete dát dotaz na hello index během pár minut pomocí nástroje hello portálu.

+ Použití [Fiddler](search-fiddler.md) nebo Chrome Postman toosubmit dotazy tooan index, že jste nahráli tooyour službu vyhledávání. Oba nástroje podporovat volání REST, tooan koncový bod protokolu HTTP. 
