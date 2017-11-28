---
title: "Úplný text search engine (Lucene) architektura ve službě Azure Search | Microsoft Docs"
description: "Vysvětlení Lucene dotaz zpracování a dokumentu načtení koncepty pro fulltextové vyhledávání v souvislosti s Azure Search."
services: search
manager: jhubbard
author: yahnoosh
documentationcenter: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/06/2017
ms.author: jlembicz
ms.openlocfilehash: 9b7adf78271407963ed1d4b34a7760d707b5fc3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-full-text-search-works-in-azure-search"></a><span data-ttu-id="b732c-103">Jak úplné textové vyhledávání funguje ve službě Azure Search</span><span class="sxs-lookup"><span data-stu-id="b732c-103">How full text search works in Azure Search</span></span>

<span data-ttu-id="b732c-104">Tento článek je pro vývojáře, kteří potřebují lépe pochopili, jak funguje Lucene fulltextové vyhledávání ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="b732c-104">This article is for developers who need a deeper understanding of how Lucene full text search works in Azure Search.</span></span> <span data-ttu-id="b732c-105">Pro dotazy text Azure Search ve většině scénářů bezproblémově dodá očekávané výsledky, ale někdy může získat výsledek, který se zdá, že "off" nějakým způsobem.</span><span class="sxs-lookup"><span data-stu-id="b732c-105">For text queries, Azure Search will seamlessly deliver expected results in most scenarios, but occasionally you might get a result that seems "off" somehow.</span></span> <span data-ttu-id="b732c-106">V těchto situacích s pozadí ve čtyři fáze provádění dotazů Lucene (dotaz analýzy, lexikální analýzy, dokumentů párování, vyhodnocování) můžete identifikovat konkrétní změny parametry dotazu nebo konfigurace indexu, která bude poskytovat požadovanou výsledek.</span><span class="sxs-lookup"><span data-stu-id="b732c-106">In these situations, having a background in the four stages of Lucene query execution (query parsing, lexical analysis, document matching, scoring) can help you identify specific changes to query parameters or index configuration that will deliver the desired outcome.</span></span> 

> [!Note] 
> <span data-ttu-id="b732c-107">Služba Azure Search používá Lucene pro fulltextové vyhledávání, ale Lucene integrace není vyčerpávající.</span><span class="sxs-lookup"><span data-stu-id="b732c-107">Azure Search uses Lucene for full text search, but Lucene integration is not exhaustive.</span></span> <span data-ttu-id="b732c-108">Jsme selektivně vystavit a rozšiřovat funkce Lucene k povolení scénářů důležité do služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="b732c-108">We selectively expose and extend Lucene functionality to enable the scenarios important to Azure Search.</span></span> 

## <a name="architecture-overview-and-diagram"></a><span data-ttu-id="b732c-109">Přehled architektury a diagram</span><span class="sxs-lookup"><span data-stu-id="b732c-109">Architecture overview and diagram</span></span>

<span data-ttu-id="b732c-110">Zpracování textu v plném znění vyhledávací dotaz začíná Analýza textu dotazu k extrakci hledaný text.</span><span class="sxs-lookup"><span data-stu-id="b732c-110">Processing a full text search query starts with parsing the query text to extract search terms.</span></span> <span data-ttu-id="b732c-111">Na vyhledávací web používá index k získávání dokumentů s odpovídajícími položkami.</span><span class="sxs-lookup"><span data-stu-id="b732c-111">The search engine uses an index to retrieve documents with matching terms.</span></span> <span data-ttu-id="b732c-112">Konkrétní dotaz podmínky jsou někdy rozdělit a rekonstituovaných do nové formuláře přetypovat širší net přes co může považovat za potenciální shody.</span><span class="sxs-lookup"><span data-stu-id="b732c-112">Individual query terms are sometimes broken down and reconstituted into new forms to cast a broader net over what could be considered as a potential match.</span></span> <span data-ttu-id="b732c-113">Je sada výsledků dotazu je pak seřazené podle skóre relevance přiřazené jednotlivé odpovídající dokumenty.</span><span class="sxs-lookup"><span data-stu-id="b732c-113">A result set is then sorted by a relevance score assigned to each individual matching document.</span></span> <span data-ttu-id="b732c-114">Ty v horní části seznamu seřazený se vrátíte na volající aplikace.</span><span class="sxs-lookup"><span data-stu-id="b732c-114">Those at the top of the ranked list are returned to the calling application.</span></span>

<span data-ttu-id="b732c-115">Revidovat, provádění dotazů má čtyři fáze:</span><span class="sxs-lookup"><span data-stu-id="b732c-115">Restated, query execution has four stages:</span></span> 

1. <span data-ttu-id="b732c-116">Analýza dotazu</span><span class="sxs-lookup"><span data-stu-id="b732c-116">Query parsing</span></span> 
2. <span data-ttu-id="b732c-117">Lexikální analýzy</span><span class="sxs-lookup"><span data-stu-id="b732c-117">Lexical analysis</span></span> 
3. <span data-ttu-id="b732c-118">Načtení dokumentu</span><span class="sxs-lookup"><span data-stu-id="b732c-118">Document retrieval</span></span> 
4. <span data-ttu-id="b732c-119">Vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="b732c-119">Scoring</span></span> 

<span data-ttu-id="b732c-120">Následující diagram znázorňuje komponenty, používá ke zpracování žádost o vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="b732c-120">The diagram below illustrates the components used to process a search request.</span></span> 

 ![Diagram architektury dotazů Lucene ve službě Azure Search][1]


| <span data-ttu-id="b732c-122">Klíčové komponenty</span><span class="sxs-lookup"><span data-stu-id="b732c-122">Key components</span></span> | <span data-ttu-id="b732c-123">Funkční popis</span><span class="sxs-lookup"><span data-stu-id="b732c-123">Functional description</span></span> | 
|----------------|------------------------|
|<span data-ttu-id="b732c-124">**Dotaz analyzátory**</span><span class="sxs-lookup"><span data-stu-id="b732c-124">**Query parsers**</span></span> | <span data-ttu-id="b732c-125">Operátory dotazu nezávislá na infrastruktuře vyhledávacích dotazů a vytvořit strukturu dotazu (stromu dotazu) k odeslání na vyhledávací web.</span><span class="sxs-lookup"><span data-stu-id="b732c-125">Separate query terms from query operators and create the query structure (a query tree) to be sent to the search engine.</span></span> |
|<span data-ttu-id="b732c-126">**Analyzátory**</span><span class="sxs-lookup"><span data-stu-id="b732c-126">**Analyzers**</span></span> | <span data-ttu-id="b732c-127">Lexikální analýzám na vyhledávacích dotazů.</span><span class="sxs-lookup"><span data-stu-id="b732c-127">Perform lexical analysis on query terms.</span></span> <span data-ttu-id="b732c-128">Tento proces může zahrnovat transformace, odebrání nebo rozšiřování vyhledávacích dotazů.</span><span class="sxs-lookup"><span data-stu-id="b732c-128">This process can involve transforming, removing, or expanding of query terms.</span></span> |
|<span data-ttu-id="b732c-129">**Index**</span><span class="sxs-lookup"><span data-stu-id="b732c-129">**Index**</span></span> | <span data-ttu-id="b732c-130">Efektivní datová struktura, které slouží k ukládání a uspořádání prohledávatelné podmínky extrahovat z indexovaného dokumentů.</span><span class="sxs-lookup"><span data-stu-id="b732c-130">An efficient data structure used to store and organize searchable terms extracted from indexed documents.</span></span> |
|<span data-ttu-id="b732c-131">**Vyhledávací web**</span><span class="sxs-lookup"><span data-stu-id="b732c-131">**Search engine**</span></span> | <span data-ttu-id="b732c-132">Načte a skóre na základě obsahu obráceným indexu odpovídající dokumenty.</span><span class="sxs-lookup"><span data-stu-id="b732c-132">Retrieves and scores matching documents based on the contents of the inverted index.</span></span> |

## <a name="anatomy-of-a-search-request"></a><span data-ttu-id="b732c-133">Anatomie žádost o vyhledávání</span><span class="sxs-lookup"><span data-stu-id="b732c-133">Anatomy of a search request</span></span>

<span data-ttu-id="b732c-134">Žádost o vyhledávání je dokončení specifikaci co má být vrácen sadě výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="b732c-134">A search request is a complete specification of what should be returned in a result set.</span></span> <span data-ttu-id="b732c-135">V nejjednodušší podobě je prázdný dotaz s žádná kritéria jakéhokoli druhu.</span><span class="sxs-lookup"><span data-stu-id="b732c-135">In simplest form, it is an empty query with no criteria of any kind.</span></span> <span data-ttu-id="b732c-136">Realističtější příklad obsahuje parametry, několika vyhledávacích dotazů, případně omezená na určitá pole s pravděpodobně výraz filtru a řazení pravidla.</span><span class="sxs-lookup"><span data-stu-id="b732c-136">A more realistic example includes parameters, several query terms, perhaps scoped to certain fields, with possibly a filter expression and ordering rules.</span></span>  

<span data-ttu-id="b732c-137">Následující příklad je žádost o vyhledávání můžete odeslat pomocí Azure Search [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span><span class="sxs-lookup"><span data-stu-id="b732c-137">The following example is a search request you might send to Azure Search using the [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span></span>  

~~~~
POST /indexes/hotels/docs/search?api-version=2016-09-01 
{  
    "search": "Spacious, air-condition* +\"Ocean view\"",  
    "searchFields": "description, title",  
    "searchMode": "any",
    "filter": "price ge 60 and price lt 300",  
    "orderby": "geo.distance(location, geography'POINT(-159.476235 22.227659)')", 
    "queryType": "full" 
 } 
~~~~

<span data-ttu-id="b732c-138">Pro tento požadavek na vyhledávací web provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="b732c-138">For this request, the search engine does the following:</span></span>

1. <span data-ttu-id="b732c-139">Filtruje dokumentů, jejichž cena je alespoň $60 a menší než 300 USD.</span><span class="sxs-lookup"><span data-stu-id="b732c-139">Filters out documents where the price is at least $60 and less than $300.</span></span>
2. <span data-ttu-id="b732c-140">Provede daný dotaz.</span><span class="sxs-lookup"><span data-stu-id="b732c-140">Executes the query.</span></span> <span data-ttu-id="b732c-141">V tomto příkladu dotaz vyhledávání se skládá z frází a podmínky: `"Spacious, air-condition* +\"Ocean view\""` (uživatelé obvykle nezadávejte interpunkční znaménka, ale včetně v příkladu umožňuje vysvětlují, jak ji zpracovat analyzátorů).</span><span class="sxs-lookup"><span data-stu-id="b732c-141">In this example, the search query consists of phrases and terms: `"Spacious, air-condition* +\"Ocean view\""` (users typically don't enter punctuation, but including it in the example allows us to explain how analyzers handle it).</span></span> <span data-ttu-id="b732c-142">Pro tento dotaz na vyhledávací web vyhledá popis a název pole zadaný v `searchFields` pro dokumenty, které obsahují "Oceánu zobrazení" a dále na termín "velké" nebo na podmínky, které začínají předponou "air-condition".</span><span class="sxs-lookup"><span data-stu-id="b732c-142">For this query, the search engine scans the description and title fields specified in `searchFields` for documents that contain "Ocean view", and additionally on the term "spacious", or on terms that start with the prefix "air-condition".</span></span> <span data-ttu-id="b732c-143">`searchMode` Parametr se používá k odpovídat na všechny termín (výchozí) nebo všechny z nich pro případy, kdy není explicitně požadované termín (`+`).</span><span class="sxs-lookup"><span data-stu-id="b732c-143">The `searchMode` parameter is used to match on any term (default) or all of them, for cases where a term is not explicitly required (`+`).</span></span>
3. <span data-ttu-id="b732c-144">Příkazy, které výsledná sada hotels podle blízkosti umístění dané Geografie a poté vrácen volající aplikace.</span><span class="sxs-lookup"><span data-stu-id="b732c-144">Orders the resulting set of hotels by proximity to a given geography location, and then returned to the calling application.</span></span> 

<span data-ttu-id="b732c-145">Většina Tento článek se týká zpracování *vyhledávací dotaz*: `"Spacious, air-condition* +\"Ocean view\""`.</span><span class="sxs-lookup"><span data-stu-id="b732c-145">The majority of this article is about processing of the *search query*: `"Spacious, air-condition* +\"Ocean view\""`.</span></span> <span data-ttu-id="b732c-146">Filtrování a řazení jsou mimo rozsah.</span><span class="sxs-lookup"><span data-stu-id="b732c-146">Filtering and ordering are out of scope.</span></span> <span data-ttu-id="b732c-147">Další informace najdete v tématu [referenční dokumentace rozhraní API pro vyhledávání](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span><span class="sxs-lookup"><span data-stu-id="b732c-147">For more information, see the [Search API reference documentation](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span></span>

<a name="stage1"></a>
## <a name="stage-1-query-parsing"></a><span data-ttu-id="b732c-148">Fáze 1: Analýza dotazu</span><span class="sxs-lookup"><span data-stu-id="b732c-148">Stage 1: Query parsing</span></span> 

<span data-ttu-id="b732c-149">Jak jsme uvedli, řetězec dotazu je první řádek žádosti:</span><span class="sxs-lookup"><span data-stu-id="b732c-149">As noted, the query string is the first line of the request:</span></span> 

~~~~
 "search": "Spacious, air-condition* +\"Ocean view\"", 
~~~~

<span data-ttu-id="b732c-150">Analyzátor dotazu odděluje operátory (například `*` a `+` v příkladu) z hledání podmínky a deconstructs vyhledávací dotaz do *poddotazy* podporovaného typu:</span><span class="sxs-lookup"><span data-stu-id="b732c-150">The query parser separates operators (such as `*` and `+` in the example) from search terms, and deconstructs the search query into *subqueries* of a supported type:</span></span> 

+ <span data-ttu-id="b732c-151">*Termín dotazu* pro samostatné podmínky (třeba velké)</span><span class="sxs-lookup"><span data-stu-id="b732c-151">*term query* for standalone terms (like spacious)</span></span>
+ <span data-ttu-id="b732c-152">*dotaz frázi* uvozovkách podmínek (podobně jako zobrazení oceánu)</span><span class="sxs-lookup"><span data-stu-id="b732c-152">*phrase query* for quoted terms (like ocean view)</span></span>
+ <span data-ttu-id="b732c-153">*Předpona dotazu* podmínek, za nímž následuje operátor předponu `*` (jako jsou air-condition)</span><span class="sxs-lookup"><span data-stu-id="b732c-153">*prefix query* for terms followed by a prefix operator `*` (like air-condition)</span></span>

<span data-ttu-id="b732c-154">Úplný seznam podporovaných dotazu typy najdete v části [sytnax dotazů Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)</span><span class="sxs-lookup"><span data-stu-id="b732c-154">For a full list of supported query types see [Lucene query sytnax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)</span></span>

<span data-ttu-id="b732c-155">Operátory přidružené poddotazu určit, zda dotaz "musí být" nebo "by měla být" uspokojit, aby dokument být považovány za shodné.</span><span class="sxs-lookup"><span data-stu-id="b732c-155">Operators associated with a subquery determine whether the query "must be" or "should be" satisfied in order for a document to be considered a match.</span></span> <span data-ttu-id="b732c-156">Například `+"Ocean view"` je "musí" kvůli `+` operátor.</span><span class="sxs-lookup"><span data-stu-id="b732c-156">For example, `+"Ocean view"` is "must" due to the `+` operator.</span></span> 

<span data-ttu-id="b732c-157">Analyzátor dotazu ke změně struktury poddotazy do *dotazu stromu* (interní strukturu představující dotazu) předává vyhledávací web.</span><span class="sxs-lookup"><span data-stu-id="b732c-157">The query parser restructures the subqueries into a *query tree* (an internal structure representing the query) it passes on to the search engine.</span></span> <span data-ttu-id="b732c-158">V první fázi dotazu analýzy stromu dotazu vypadat třeba takto.</span><span class="sxs-lookup"><span data-stu-id="b732c-158">In the first stage of query parsing, the query tree looks like this.</span></span>  

 ![Logická hodnota dotazu searchmode všechny][2]

### <a name="supported-parsers-simple-and-full-lucene"></a><span data-ttu-id="b732c-160">Podporované analyzátory: jednoduchý a úplné Lucene</span><span class="sxs-lookup"><span data-stu-id="b732c-160">Supported parsers: Simple and Full Lucene</span></span> 

 <span data-ttu-id="b732c-161">Služba Azure Search zpřístupní dvou různých dotazu jazyků, `simple` (výchozí) a `full`.</span><span class="sxs-lookup"><span data-stu-id="b732c-161">Azure Search exposes two different query languages, `simple` (default) and `full`.</span></span> <span data-ttu-id="b732c-162">Nastavením `queryType` parametr s vaši žádost o vyhledávání se zjistit analyzátor dotazu jazyk dotazu, který zvolíte, aby věděl, že může interpretace operátory a syntaxe.</span><span class="sxs-lookup"><span data-stu-id="b732c-162">By setting the `queryType` parameter with your search request, you tell the query parser which query language you choose so that it knows how to interpret the operators and syntax.</span></span> <span data-ttu-id="b732c-163">[Jednoduchý dotaz jazyka](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) je intuitivní a robustní, často vhodný interpretace vstupu uživatele jako-je bez zpracování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="b732c-163">The [Simple query langauge](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) is intuitive and robust, often suitable to interpret user input as-is without client-side processing.</span></span> <span data-ttu-id="b732c-164">Podporuje operátory dotazu známých z webové vyhledávacích webů.</span><span class="sxs-lookup"><span data-stu-id="b732c-164">It supports query operators familiar from web search engines.</span></span> <span data-ttu-id="b732c-165">[Úplné Lucene dotazovací jazyk](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), které dostanete tak, že nastavení `queryType=full`, rozšiřuje výchozí jazyk jednoduchý dotaz přidáním podpory pro další operátory a typy dotazů jako zástupný znak, přibližné, regulární výraz a dotazy v rámci pole.</span><span class="sxs-lookup"><span data-stu-id="b732c-165">The [Full Lucene query language](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), which you get by setting `queryType=full`, extends the default Simple query language by adding support for more operators and query types like wildcard, fuzzy, regex, and field-scoped queries.</span></span> <span data-ttu-id="b732c-166">Například by odeslaných za jednoduchá syntaxe dotazů regulární výraz vyhodnocen jako řetězec dotazu a není výraz.</span><span class="sxs-lookup"><span data-stu-id="b732c-166">For example, a regular expression sent in Simple query syntax would be interpreted as a query string and not an expression.</span></span> <span data-ttu-id="b732c-167">Žádost o příklad v tomto článku používá úplné Lucene dotazovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="b732c-167">The example request in this article uses the Full Lucene query language.</span></span>

### <a name="impact-of-searchmode-on-the-parser"></a><span data-ttu-id="b732c-168">Dopad searchMode analyzátoru</span><span class="sxs-lookup"><span data-stu-id="b732c-168">Impact of searchMode on the parser</span></span> 

<span data-ttu-id="b732c-169">Je jiný parametr žádost o vyhledávání, který ovlivňuje analýza `searchMode` parametr.</span><span class="sxs-lookup"><span data-stu-id="b732c-169">Another search request parameter that affects parsing is the `searchMode` parameter.</span></span> <span data-ttu-id="b732c-170">Ovládá výchozí operátor pro logickou dotazy: všechny (výchozí) nebo všechny.</span><span class="sxs-lookup"><span data-stu-id="b732c-170">It controls the default operator for Boolean queries: any (default) or all.</span></span>  

<span data-ttu-id="b732c-171">Když `searchMode=any`, což je výchozí, místo oddělovač mezi velké a air-condition nebo (`||`), provedení ekvivalentní ukázkový text dotazu:</span><span class="sxs-lookup"><span data-stu-id="b732c-171">When `searchMode=any`, which is the default, the space delimiter between spacious and air-condition is OR (`||`), making the sample query text equivalent to:</span></span> 

~~~~
Spacious,||air-condition*+"Ocean view" 
~~~~

<span data-ttu-id="b732c-172">Explicitní operátory, jako například `+` v `+"Ocean view"`, jsou jednoznačné v dotazu Logická konstrukce (termín *musí* odpovídat).</span><span class="sxs-lookup"><span data-stu-id="b732c-172">Explicit operators, such as `+` in `+"Ocean view"`, are unambiguous in boolean query construction (the term *must* match).</span></span> <span data-ttu-id="b732c-173">Méně zřejmé je interpretace zbývající podmínky: velké a air-condition.</span><span class="sxs-lookup"><span data-stu-id="b732c-173">Less obvious is how to interpret the remaining terms: spacious and air-condition.</span></span> <span data-ttu-id="b732c-174">Měli vyhledávacího webu najít odpovídá na zobrazení oceánu *a* velké *a* air-condition?</span><span class="sxs-lookup"><span data-stu-id="b732c-174">Should the search engine find matches on ocean view *and* spacious *and* air-condition?</span></span> <span data-ttu-id="b732c-175">Nebo by měl zjistit oceánu zobrazení plus *buď jeden* zbývající podmínek?</span><span class="sxs-lookup"><span data-stu-id="b732c-175">Or should it find ocean view plus *either one* of the remaining terms?</span></span> 

<span data-ttu-id="b732c-176">Ve výchozím nastavení (`searchMode=any`), na vyhledávací web předpokládá širší interpretace.</span><span class="sxs-lookup"><span data-stu-id="b732c-176">By default (`searchMode=any`), the search engine assumes the broader interpretation.</span></span> <span data-ttu-id="b732c-177">Buď pole *by měl* odpovídat, které odráží sémantiku "nebo".</span><span class="sxs-lookup"><span data-stu-id="b732c-177">Either field *should* be matched, reflecting "or" semantics.</span></span> <span data-ttu-id="b732c-178">Stromu počátečního dotazu zobrazeny dříve, s dva "by měl" operations, jsou uvedena výchozí.</span><span class="sxs-lookup"><span data-stu-id="b732c-178">The initial query tree illustrated previously, with the two "should" operations, shows the default.</span></span>  

<span data-ttu-id="b732c-179">Předpokládejme, že teď nastavujeme `searchMode=all`.</span><span class="sxs-lookup"><span data-stu-id="b732c-179">Suppose that we now set `searchMode=all`.</span></span> <span data-ttu-id="b732c-180">V takovém případě se místo interpretována jako operace "a".</span><span class="sxs-lookup"><span data-stu-id="b732c-180">In this case, the space is interpreted as an "and" operation.</span></span> <span data-ttu-id="b732c-181">Každý zbývající podmínek musí být přítomen v dokumentu ke kvalifikaci jako shoda.</span><span class="sxs-lookup"><span data-stu-id="b732c-181">Each of the remaining terms must both be present in the document to qualify as a match.</span></span> <span data-ttu-id="b732c-182">Výsledný ukázkový dotaz by interpretovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b732c-182">The resulting sample query would be interpreted as follows:</span></span> 

~~~~
+Spacious,+air-condition*+"Ocean view"  
~~~~

<span data-ttu-id="b732c-183">Strom upravený dotaz pro tento dotaz bude následující, kde odpovídající dokument je průnik všechny tři poddotazy:</span><span class="sxs-lookup"><span data-stu-id="b732c-183">A modified query tree for this query would be as follows, where a matching document is the intersection of all three subqueries:</span></span> 

 ![Logická hodnota dotazu searchmode všechny][3]

> [!Note] 
> <span data-ttu-id="b732c-185">Výběr `searchMode=any` přes `searchMode=all` je nejlepší rozhodnutí byly přijaty spuštěním reprezentativní dotazy.</span><span class="sxs-lookup"><span data-stu-id="b732c-185">Choosing `searchMode=any` over `searchMode=all` is a decision best arrived at by running representative queries.</span></span> <span data-ttu-id="b732c-186">Uživatelé, kteří jsou pravděpodobně patří operátory (společný při vyhledávání dokument uloží) může intuitivnější výsledky pokud `searchMode=all` informuje konstrukce boolean dotazu.</span><span class="sxs-lookup"><span data-stu-id="b732c-186">Users who are likely to include operators (common when searching document stores) might find results more intuitive if `searchMode=all` informs boolean query constructs.</span></span> <span data-ttu-id="b732c-187">Další informace o vztahu mezi `searchMode` a operátory, najdete v části [jednoduchá syntaxe dotazů](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).</span><span class="sxs-lookup"><span data-stu-id="b732c-187">For more about the interplay between `searchMode` and operators, see [Simple query syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).</span></span>

<a name="stage2"></a>
## <a name="stage-2-lexical-analysis"></a><span data-ttu-id="b732c-188">Fáze 2: Lexikální analysis</span><span class="sxs-lookup"><span data-stu-id="b732c-188">Stage 2: Lexical analysis</span></span> 

<span data-ttu-id="b732c-189">Lexikální analyzátorů proces *termín dotazy* a *fráze dotazy* po strukturovaná stromu dotazu.</span><span class="sxs-lookup"><span data-stu-id="b732c-189">Lexical analyzers process *term queries* and *phrase queries* after the query tree is structured.</span></span> <span data-ttu-id="b732c-190">Analyzátor přijímá vstupní textové hodnoty uvedené ve analyzátor, zpracuje text a pak odešle zpět tokenizovaného podmínky mají být zahrnuty do stromu dotazu.</span><span class="sxs-lookup"><span data-stu-id="b732c-190">An analyzer accepts the text inputs given to it by the parser, processes the text, and then sends back tokenized terms to be incorporated into the query tree.</span></span> 

<span data-ttu-id="b732c-191">Nejběžnější formu lexikální analýzy je *lingvistické analysis* který transformací dotaz podmínky na základě pravidel, které jsou specifické pro daný jazyk:</span><span class="sxs-lookup"><span data-stu-id="b732c-191">The most common form of lexical analysis is *linguistic analysis* which transforms query terms based on rules specific to a given language:</span></span> 

* <span data-ttu-id="b732c-192">Snižuje termín dotazu do formuláře kořenové slova</span><span class="sxs-lookup"><span data-stu-id="b732c-192">Reducing a query term to the root form of a word</span></span> 
* <span data-ttu-id="b732c-193">Odebrání nepotřebných slova (stopslova, jako je například "na" nebo "a" v angličtině)</span><span class="sxs-lookup"><span data-stu-id="b732c-193">Removing non-essential words (stopwords, such as "the" or "and" in English)</span></span> 
* <span data-ttu-id="b732c-194">Rozdělení složené slovo do součásti</span><span class="sxs-lookup"><span data-stu-id="b732c-194">Breaking a composite word into component parts</span></span> 
* <span data-ttu-id="b732c-195">Nižší velká slova velká a malá písmena</span><span class="sxs-lookup"><span data-stu-id="b732c-195">Lower casing an upper case word</span></span> 

<span data-ttu-id="b732c-196">Všechny tyto operace jsou obvykle vymazat rozdíly mezi text vstup uživatelem a podmínky, které jsou uloženy v indexu.</span><span class="sxs-lookup"><span data-stu-id="b732c-196">All of these operations tend to erase differences between the text input provided by the user and the terms stored in the index.</span></span> <span data-ttu-id="b732c-197">Tyto operace nad rámec zpracování textu a vyžaduje zevrubnou znalost jazyka sám sebe.</span><span class="sxs-lookup"><span data-stu-id="b732c-197">Such operations go beyond text processing and require in-depth knowledge of the language itself.</span></span> <span data-ttu-id="b732c-198">Pokud chcete přidat tuto vrstvu lingvistické sledování, podporuje Azure Search dlouhý seznam [analyzátory jazyka](https://docs.microsoft.com/rest/api/searchservice/language-support) Lucene a společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b732c-198">To add this layer of linguistic awareness, Azure Search supports a long list of [language analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support) from both Lucene and Microsoft.</span></span>

> [!Note]
> <span data-ttu-id="b732c-199">Analýza požadavků může být v rozsahu od minimální do vypracovala v závislosti na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="b732c-199">Analysis requirements can range from minimal to elaborate depending on your scenario.</span></span> <span data-ttu-id="b732c-200">Můžete řídit složitost lexikální analýzy vyberete jeden z předdefinovaných analyzátorů nebo vytvoření vlastních [vlastní analyzátor](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="b732c-200">You can control complexity of lexical analysis by the selecting one of the predefined analyzers or by creating your own [custom analyzer](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search).</span></span> <span data-ttu-id="b732c-201">Analyzátory jsou omezená na prohledatelná pole a jsou zadané jako součást definice pole.</span><span class="sxs-lookup"><span data-stu-id="b732c-201">Analyzers are scoped to searchable fields and are specified as part of a field definition.</span></span> <span data-ttu-id="b732c-202">To umožňuje měnit lexikální analýzy na základě za pole.</span><span class="sxs-lookup"><span data-stu-id="b732c-202">This allows you to vary lexical analysis on a per-field basis.</span></span> <span data-ttu-id="b732c-203">Tento parametr zadán, *standardní* Lucene analyzer je použít.</span><span class="sxs-lookup"><span data-stu-id="b732c-203">Unspecified, the *standard* Lucene analyzer is used.</span></span>

<span data-ttu-id="b732c-204">V našem příkladu před analýzy stromu počátečního dotazu má termín "Spacious, velká písmena"S"a čárkami, který analyzátor dotazu interpretuje jako součást termín dotazu (čárkou nepovažuje operátor dotazu jazyka)".</span><span class="sxs-lookup"><span data-stu-id="b732c-204">In our example, prior to analysis, the initial query tree has the term "Spacious," with an uppercase "S" and a comma that the query parser interprets as a part of the query term (a comma is not considered a query language operator).</span></span>  

<span data-ttu-id="b732c-205">Když analyzátor výchozí zpracovává termín, bude malá písmena "zobrazení oceánu" a "velké" a odeberte čárku.</span><span class="sxs-lookup"><span data-stu-id="b732c-205">When the default analyzer processes the term, it will lowercase "ocean view" and "spacious", and remove the comma character.</span></span> <span data-ttu-id="b732c-206">Stromu upravené dotazu bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="b732c-206">The modified query tree will look as follows:</span></span> 

 ![Logická hodnota dotaz s analyzovaných podmínky][4]

### <a name="testing-analyzer-behaviors"></a><span data-ttu-id="b732c-208">Testování Analyzátor chování</span><span class="sxs-lookup"><span data-stu-id="b732c-208">Testing analyzer behaviors</span></span> 

<span data-ttu-id="b732c-209">Chování analyzátor lze otestovat pomocí [analyzovat rozhraní API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer).</span><span class="sxs-lookup"><span data-stu-id="b732c-209">The behavior of an analyzer can be tested using the [Analyze API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer).</span></span> <span data-ttu-id="b732c-210">Zadejte text, který chcete analyzovat Pokud chcete zobrazit, co bude generovat podmínky zadané analyzátor.</span><span class="sxs-lookup"><span data-stu-id="b732c-210">Provide the text you want to analyze to see what terms given analyzer will generate.</span></span> <span data-ttu-id="b732c-211">Například informace o tom, jak by standardní analyzátor zpracovat text "air-condition", můžete použít následující požadavek:</span><span class="sxs-lookup"><span data-stu-id="b732c-211">For example, to see how the standard analyzer would process the text "air-condition", you can issue the following request:</span></span>

~~~~
{ 
    "text": "air-condition",
    "analyzer": "standard"
}
~~~~

<span data-ttu-id="b732c-212">Standardní analyzátor dělí vstupního textu do následujících dvou tokenů, zadávání poznámek o nich s atributy, jako je počáteční a koncové posunutí (používá se pro přístupů zvýraznění), jakož i jejich polohu (používá se pro párování frázi):</span><span class="sxs-lookup"><span data-stu-id="b732c-212">The standard analyzer breaks the input text into the following two tokens, annotating them with attributes like start and end offsets (used for hit highlighting) as well as their position (used for phrase matching):</span></span>

~~~~
{  
  "tokens": [
    {
      "token": "air",
      "startOffset": 0,
      "endOffset": 3,
      "position": 0
    },
    {
      "token": "condition",
      "startOffset": 4,
      "endOffset": 13,
      "position": 1
    }
  ]
}
~~~~

### <a name="exceptions-to-lexical-analysis"></a><span data-ttu-id="b732c-213">Výjimky lexikální analýzy</span><span class="sxs-lookup"><span data-stu-id="b732c-213">Exceptions to lexical analysis</span></span> 

<span data-ttu-id="b732c-214">Lexikální analýzy se vztahuje pouze na typy dotazů, které vyžadují úplný podmínky – dotazu termín nebo frázi dotazu.</span><span class="sxs-lookup"><span data-stu-id="b732c-214">Lexical analysis applies only to query types that require complete terms – either a term query or a phrase query.</span></span> <span data-ttu-id="b732c-215">Netýká typy dotazů s neúplné podmínkami – předpona dotazu, dotaz zástupný znak, regulární výraz dotazu – nebo přibližné dotazu.</span><span class="sxs-lookup"><span data-stu-id="b732c-215">It doesn’t apply to query types with incomplete terms – prefix query, wildcard query, regex query – or to a fuzzy query.</span></span> <span data-ttu-id="b732c-216">Ty typy, včetně předponu dotazu s termín dotazů *air-condition\**  v našem příkladu jsou přidány přímo do stromu dotazu obcházení fázi analýzy.</span><span class="sxs-lookup"><span data-stu-id="b732c-216">Those query types, including the prefix query with term *air-condition\** in our example, are added directly to the query tree, bypassing the analysis stage.</span></span> <span data-ttu-id="b732c-217">Pouze transformaci u vyhledávacích dotazů těchto typů provést, je předpoklady.</span><span class="sxs-lookup"><span data-stu-id="b732c-217">The only transformation performed on query terms of those types is lowercasing.</span></span>

<a name="stage3"></a>
## <a name="stage-3-document-retrieval"></a><span data-ttu-id="b732c-218">Fáze 3: Načtení dokumentu</span><span class="sxs-lookup"><span data-stu-id="b732c-218">Stage 3: Document retrieval</span></span> 

<span data-ttu-id="b732c-219">Načtení dokumentu odkazuje na hledání dokumentů s odpovídajícím podmínky v indexu.</span><span class="sxs-lookup"><span data-stu-id="b732c-219">Document retrieval refers to finding documents with matching terms in the index.</span></span> <span data-ttu-id="b732c-220">Tato fáze odhalíte nejlépe v příkladu.</span><span class="sxs-lookup"><span data-stu-id="b732c-220">This stage is understood best through an example.</span></span> <span data-ttu-id="b732c-221">Začněme s indexem hotels s následující jednoduché schématu:</span><span class="sxs-lookup"><span data-stu-id="b732c-221">Let's start with a hotels index having the following simple schema:</span></span> 

~~~~
{   
    "name": "hotels",     
    "fields": [     
        { "name": "id", "type": "Edm.String", "key": true, "searchable": false },     
        { "name": "title", "type": "Edm.String", "searchable": true },     
        { "name": "description", "type": "Edm.String", "searchable": true }
    ] 
} 
~~~~

<span data-ttu-id="b732c-222">Další Předpokládejme, že tento index obsahuje následující čtyři dokumenty:</span><span class="sxs-lookup"><span data-stu-id="b732c-222">Further assume that this index contains the following four documents:</span></span> 

~~~~
{ 
    "value": [
        {         
            "id": "1",         
            "title": "Hotel Atman",         
            "description": "Spacious rooms, ocean view, walking distance to the beach."   
        },       
        {         
            "id": "2",         
            "title": "Beach Resort",        
            "description": "Located on the north shore of the island of Kauaʻi. Ocean view."     
        },       
        {         
            "id": "3",         
            "title": "Playa Hotel",         
            "description": "Comfortable, air-conditioned rooms with ocean view."
        },       
        {         
            "id": "4",         
            "title": "Ocean Retreat",         
            "description": "Quiet and secluded"
        }    
    ]
}
~~~~

<span data-ttu-id="b732c-223">**Jak jsou indexované podmínky**</span><span class="sxs-lookup"><span data-stu-id="b732c-223">**How terms are indexed**</span></span>

<span data-ttu-id="b732c-224">Zjistit, načtení, je důležité znát několik základní informace o indexování.</span><span class="sxs-lookup"><span data-stu-id="b732c-224">To understand retrieval, it helps to know a few basics about indexing.</span></span> <span data-ttu-id="b732c-225">Jednotka úložiště je obráceným index, jeden pro každé prohledávatelné pole.</span><span class="sxs-lookup"><span data-stu-id="b732c-225">The unit of storage is an inverted index, one for each searchable field.</span></span> <span data-ttu-id="b732c-226">V rámci obráceným index je seřazený seznam všechny podmínky ze všech dokumentů.</span><span class="sxs-lookup"><span data-stu-id="b732c-226">Within an inverted index is a sorted list of all terms from all documents.</span></span> <span data-ttu-id="b732c-227">Každému termínu se mapuje na seznam dokumentů, v nichž se vyskytuje, zřejmá jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="b732c-227">Each term maps to the list of documents in which it occurs, as evident in the example below.</span></span>

<span data-ttu-id="b732c-228">K vytvoření podmínky převedený index, provede na vyhledávací web lexikální analýzu přes obsah dokumentů, podobně jako co se stane, že při zpracování dotazu.</span><span class="sxs-lookup"><span data-stu-id="b732c-228">To produce the terms in an inverted index, the search engine performs lexical analysis over the content of documents, similar to what happens during query processing.</span></span> <span data-ttu-id="b732c-229">Text vstupy jsou předaný analyzátor použita nižší odstraní interpunkční znaménka, a tak dále, v závislosti na konfiguraci analyzátor.</span><span class="sxs-lookup"><span data-stu-id="b732c-229">Text inputs are passed to an analyzer, lower-cased, stripped of punctuation, and so forth, depending on the analyzer configuration.</span></span> <span data-ttu-id="b732c-230">Je běžné, ale nejsou vyžadovány, používat stejné analyzátory pro vyhledávání a indexování operations tak, aby vypadal jako podmínky uvnitř index další podmínky dotazu.</span><span class="sxs-lookup"><span data-stu-id="b732c-230">It's common, but not required, to use the same analyzers for search and indexing operations so that query terms look more like terms inside the index.</span></span>

> [!Note]
> <span data-ttu-id="b732c-231">Služba Azure Search umožňuje určit různé analyzátory pro indexování a hledání prostřednictvím další `indexAnalyzer` a `searchAnalyzer` pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="b732c-231">Azure Search lets you specify different analyzers for indexing and search via additional `indexAnalyzer` and `searchAnalyzer` field parameters.</span></span> <span data-ttu-id="b732c-232">Pokud tento parametr zadán, analyzátor nastavit `analyzer` vlastnost se používá pro indexování a vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="b732c-232">If unspecified, the analyzer set with the `analyzer` property is used for both indexing and searching.</span></span>  

<span data-ttu-id="b732c-233">**Převedený index pro příklad dokumenty**</span><span class="sxs-lookup"><span data-stu-id="b732c-233">**Inverted index for example documents**</span></span>

<span data-ttu-id="b732c-234">Návrat na našem příkladu pro **název** pole, obráceným index vypadat třeba takto:</span><span class="sxs-lookup"><span data-stu-id="b732c-234">Returning to our example, for the **title** field, the inverted index looks like this:</span></span>

| <span data-ttu-id="b732c-235">Označení</span><span class="sxs-lookup"><span data-stu-id="b732c-235">Term</span></span> | <span data-ttu-id="b732c-236">Seznam dokumentů</span><span class="sxs-lookup"><span data-stu-id="b732c-236">Document list</span></span> |
|------|---------------|
| <span data-ttu-id="b732c-237">atman</span><span class="sxs-lookup"><span data-stu-id="b732c-237">atman</span></span> | <span data-ttu-id="b732c-238">1</span><span class="sxs-lookup"><span data-stu-id="b732c-238">1</span></span> |
| <span data-ttu-id="b732c-239">plážový</span><span class="sxs-lookup"><span data-stu-id="b732c-239">beach</span></span> | <span data-ttu-id="b732c-240">2</span><span class="sxs-lookup"><span data-stu-id="b732c-240">2</span></span> |
| <span data-ttu-id="b732c-241">hotelů</span><span class="sxs-lookup"><span data-stu-id="b732c-241">hotel</span></span> | <span data-ttu-id="b732c-242">1, 3</span><span class="sxs-lookup"><span data-stu-id="b732c-242">1, 3</span></span> |
| <span data-ttu-id="b732c-243">oceánu</span><span class="sxs-lookup"><span data-stu-id="b732c-243">ocean</span></span> | <span data-ttu-id="b732c-244">4</span><span class="sxs-lookup"><span data-stu-id="b732c-244">4</span></span>  |
| <span data-ttu-id="b732c-245">playa</span><span class="sxs-lookup"><span data-stu-id="b732c-245">playa</span></span> | <span data-ttu-id="b732c-246">3</span><span class="sxs-lookup"><span data-stu-id="b732c-246">3</span></span> |
| <span data-ttu-id="b732c-247">možnost</span><span class="sxs-lookup"><span data-stu-id="b732c-247">resort</span></span> | <span data-ttu-id="b732c-248">3</span><span class="sxs-lookup"><span data-stu-id="b732c-248">3</span></span> |
| <span data-ttu-id="b732c-249">Retreat</span><span class="sxs-lookup"><span data-stu-id="b732c-249">retreat</span></span> | <span data-ttu-id="b732c-250">4</span><span class="sxs-lookup"><span data-stu-id="b732c-250">4</span></span> |

<span data-ttu-id="b732c-251">V poli s názvem pouze *hotelů* se zobrazí v dva dokumenty: 1, 3.</span><span class="sxs-lookup"><span data-stu-id="b732c-251">In the title field, only *hotel* shows up in two documents: 1, 3.</span></span>

<span data-ttu-id="b732c-252">Pro **popis** pole indexu je následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b732c-252">For the **description** field, the index is as follows:</span></span>

| <span data-ttu-id="b732c-253">Označení</span><span class="sxs-lookup"><span data-stu-id="b732c-253">Term</span></span> | <span data-ttu-id="b732c-254">Seznam dokumentů</span><span class="sxs-lookup"><span data-stu-id="b732c-254">Document list</span></span> |
|------|---------------|
| <span data-ttu-id="b732c-255">letecké</span><span class="sxs-lookup"><span data-stu-id="b732c-255">air</span></span> | <span data-ttu-id="b732c-256">3</span><span class="sxs-lookup"><span data-stu-id="b732c-256">3</span></span>
| <span data-ttu-id="b732c-257">a</span><span class="sxs-lookup"><span data-stu-id="b732c-257">and</span></span> | <span data-ttu-id="b732c-258">4</span><span class="sxs-lookup"><span data-stu-id="b732c-258">4</span></span>
| <span data-ttu-id="b732c-259">plážový</span><span class="sxs-lookup"><span data-stu-id="b732c-259">beach</span></span> | <span data-ttu-id="b732c-260">1</span><span class="sxs-lookup"><span data-stu-id="b732c-260">1</span></span>
| <span data-ttu-id="b732c-261">záleží</span><span class="sxs-lookup"><span data-stu-id="b732c-261">conditioned</span></span> | <span data-ttu-id="b732c-262">3</span><span class="sxs-lookup"><span data-stu-id="b732c-262">3</span></span>
| <span data-ttu-id="b732c-263">možnost</span><span class="sxs-lookup"><span data-stu-id="b732c-263">comfortable</span></span> | <span data-ttu-id="b732c-264">3</span><span class="sxs-lookup"><span data-stu-id="b732c-264">3</span></span>
| <span data-ttu-id="b732c-265">Vzdálenost</span><span class="sxs-lookup"><span data-stu-id="b732c-265">distance</span></span> | <span data-ttu-id="b732c-266">1</span><span class="sxs-lookup"><span data-stu-id="b732c-266">1</span></span>
| <span data-ttu-id="b732c-267">Island</span><span class="sxs-lookup"><span data-stu-id="b732c-267">island</span></span> | <span data-ttu-id="b732c-268">2</span><span class="sxs-lookup"><span data-stu-id="b732c-268">2</span></span>
| <span data-ttu-id="b732c-269">kauaʻi</span><span class="sxs-lookup"><span data-stu-id="b732c-269">kauaʻi</span></span> | <span data-ttu-id="b732c-270">2</span><span class="sxs-lookup"><span data-stu-id="b732c-270">2</span></span>
| <span data-ttu-id="b732c-271">nachází</span><span class="sxs-lookup"><span data-stu-id="b732c-271">located</span></span> | <span data-ttu-id="b732c-272">2</span><span class="sxs-lookup"><span data-stu-id="b732c-272">2</span></span>
| <span data-ttu-id="b732c-273">– sever</span><span class="sxs-lookup"><span data-stu-id="b732c-273">north</span></span> | <span data-ttu-id="b732c-274">2</span><span class="sxs-lookup"><span data-stu-id="b732c-274">2</span></span>
| <span data-ttu-id="b732c-275">oceánu</span><span class="sxs-lookup"><span data-stu-id="b732c-275">ocean</span></span> | <span data-ttu-id="b732c-276">1, 2, 3</span><span class="sxs-lookup"><span data-stu-id="b732c-276">1, 2, 3</span></span>
| <span data-ttu-id="b732c-277">z</span><span class="sxs-lookup"><span data-stu-id="b732c-277">of</span></span> | <span data-ttu-id="b732c-278">2</span><span class="sxs-lookup"><span data-stu-id="b732c-278">2</span></span>
| <span data-ttu-id="b732c-279">na</span><span class="sxs-lookup"><span data-stu-id="b732c-279">on</span></span> |<span data-ttu-id="b732c-280">2</span><span class="sxs-lookup"><span data-stu-id="b732c-280">2</span></span>
| <span data-ttu-id="b732c-281">quiet</span><span class="sxs-lookup"><span data-stu-id="b732c-281">quiet</span></span> | <span data-ttu-id="b732c-282">4</span><span class="sxs-lookup"><span data-stu-id="b732c-282">4</span></span>
| <span data-ttu-id="b732c-283">místnosti</span><span class="sxs-lookup"><span data-stu-id="b732c-283">rooms</span></span>  | <span data-ttu-id="b732c-284">1, 3</span><span class="sxs-lookup"><span data-stu-id="b732c-284">1, 3</span></span>
| <span data-ttu-id="b732c-285">secluded</span><span class="sxs-lookup"><span data-stu-id="b732c-285">secluded</span></span> | <span data-ttu-id="b732c-286">4</span><span class="sxs-lookup"><span data-stu-id="b732c-286">4</span></span>
| <span data-ttu-id="b732c-287">pobřeží</span><span class="sxs-lookup"><span data-stu-id="b732c-287">shore</span></span> | <span data-ttu-id="b732c-288">2</span><span class="sxs-lookup"><span data-stu-id="b732c-288">2</span></span>
| <span data-ttu-id="b732c-289">Velké</span><span class="sxs-lookup"><span data-stu-id="b732c-289">spacious</span></span> | <span data-ttu-id="b732c-290">1</span><span class="sxs-lookup"><span data-stu-id="b732c-290">1</span></span>
| <span data-ttu-id="b732c-291">na</span><span class="sxs-lookup"><span data-stu-id="b732c-291">the</span></span> | <span data-ttu-id="b732c-292">1, 2</span><span class="sxs-lookup"><span data-stu-id="b732c-292">1, 2</span></span>
| <span data-ttu-id="b732c-293">na</span><span class="sxs-lookup"><span data-stu-id="b732c-293">to</span></span> | <span data-ttu-id="b732c-294">1</span><span class="sxs-lookup"><span data-stu-id="b732c-294">1</span></span>
| <span data-ttu-id="b732c-295">zobrazit</span><span class="sxs-lookup"><span data-stu-id="b732c-295">view</span></span> | <span data-ttu-id="b732c-296">1, 2, 3</span><span class="sxs-lookup"><span data-stu-id="b732c-296">1, 2, 3</span></span>
| <span data-ttu-id="b732c-297">procházení</span><span class="sxs-lookup"><span data-stu-id="b732c-297">walking</span></span> | <span data-ttu-id="b732c-298">1</span><span class="sxs-lookup"><span data-stu-id="b732c-298">1</span></span>
| <span data-ttu-id="b732c-299">S</span><span class="sxs-lookup"><span data-stu-id="b732c-299">with</span></span> | <span data-ttu-id="b732c-300">3</span><span class="sxs-lookup"><span data-stu-id="b732c-300">3</span></span>


<span data-ttu-id="b732c-301">**Odpovídající vyhledávacích dotazů vůči indexované podmínky**</span><span class="sxs-lookup"><span data-stu-id="b732c-301">**Matching query terms against indexed terms**</span></span>

<span data-ttu-id="b732c-302">Zadané obráceným indexy výše, můžeme vraťte se na ukázkový dotaz a v tématu Jak odpovídající dokumenty pro náš příklad dotazu nebyly nalezeny.</span><span class="sxs-lookup"><span data-stu-id="b732c-302">Given the inverted indices above, let’s return to the sample query and see how matching documents are found for our example query.</span></span> <span data-ttu-id="b732c-303">Odvolat, aby stromu poslední dotaz vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="b732c-303">Recall that the final query tree looks like this:</span></span> 

 ![Logická hodnota dotaz s analyzovaných podmínky][4]

<span data-ttu-id="b732c-305">Během provádění dotazu jednotlivé dotazy jsou u spustit prohledatelná pole nezávisle.</span><span class="sxs-lookup"><span data-stu-id="b732c-305">During query execution, individual queries are executed against the searchable fields independently.</span></span> 

+ <span data-ttu-id="b732c-306">"Velké", odpovídá TermQuery, dokumentů 1 (hotelů Atman).</span><span class="sxs-lookup"><span data-stu-id="b732c-306">The TermQuery, "spacious", matches document 1 (Hotel Atman).</span></span> 

+ <span data-ttu-id="b732c-307">PrefixQuery, "air-condition *", se neshoduje se žádné dokumenty.</span><span class="sxs-lookup"><span data-stu-id="b732c-307">The PrefixQuery, "air-condition*", doesn't match any documents.</span></span> 

  <span data-ttu-id="b732c-308">Toto je chování, které někdy confuses vývojáři.</span><span class="sxs-lookup"><span data-stu-id="b732c-308">This is a behavior that sometimes confuses developers.</span></span> <span data-ttu-id="b732c-309">I když termín klimatizovaným existuje v dokumentu, je rozdělený do dvou podmínky podle výchozí analyzátor.</span><span class="sxs-lookup"><span data-stu-id="b732c-309">Although the term air-conditioned exists in the document, it is split into two terms by the default analyzer.</span></span> <span data-ttu-id="b732c-310">Odvolat, že předpona dotazy, které obsahují částečnou podmínky, nejsou analýza.</span><span class="sxs-lookup"><span data-stu-id="b732c-310">Recall that prefix queries, which contain partial terms, are not analyzed.</span></span> <span data-ttu-id="b732c-311">Proto jsou podmínky s předponou "air-condition" prohledávat převedený index a nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="b732c-311">Therefore terms with prefix "air-condition" are looked up in the inverted index and not found.</span></span>

+ <span data-ttu-id="b732c-312">PhraseQuery, "oceánu zobrazení" podmínky "oceánu" a "Zobrazit" a, zkontroluje blízko podmínky v původním dokumentu.</span><span class="sxs-lookup"><span data-stu-id="b732c-312">The PhraseQuery, "ocean view", looks up the terms "ocean" and "view" and checks the proximity of terms in the original document.</span></span> <span data-ttu-id="b732c-313">Dokumenty, 1, 2 a 3 odpovídat tento dotaz do pole Popis.</span><span class="sxs-lookup"><span data-stu-id="b732c-313">Documents 1, 2 and 3 match this query in the description field.</span></span> <span data-ttu-id="b732c-314">Všimněte si dokument 4 má oceánu termín v názvu, ale není považovány za shodné, jako jsme to potřebné pro frázi "zobrazení oceánu" místo jednotlivých slov.</span><span class="sxs-lookup"><span data-stu-id="b732c-314">Notice document 4 has the term ocean in the title but isn’t considered a match, as we're looking for the "ocean view" phrase rather than individual words.</span></span> 

> [!Note]
> <span data-ttu-id="b732c-315">Vyhledávací dotaz je spustit nezávisle pro všechna prohledatelná pole v indexu Azure Search není-li omezit pole nastavit `searchFields` parametr, jak ukazuje následující příklad požadavek hledání.</span><span class="sxs-lookup"><span data-stu-id="b732c-315">A search query is executed independently against all searchable fields in the Azure Search index unless you limit the fields set with the `searchFields` parameter, as illustrated in the example search request.</span></span> <span data-ttu-id="b732c-316">Dokumenty, které odpovídají v žádném z vybraných polí jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="b732c-316">Documents that match in any of the selected fields are returned.</span></span> 

<span data-ttu-id="b732c-317">Celkově v dotazu, jsou dokumenty, které odpovídají 1, 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b732c-317">On the whole, for the query in question, the documents that match are 1, 2, 3.</span></span> 

## <a name="stage-4-scoring"></a><span data-ttu-id="b732c-318">Fáze 4: vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="b732c-318">Stage 4: Scoring</span></span>  

<span data-ttu-id="b732c-319">Každému dokumentu v sadě výsledků hledání je přiřazen relevance skóre.</span><span class="sxs-lookup"><span data-stu-id="b732c-319">Every document in a search result set is assigned a relevance score.</span></span> <span data-ttu-id="b732c-320">Funkce skóre relevance je vyšší pořadí těchto dokumentů, které nejlépe odpověď na otázku uživatele jako vyjádřená vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="b732c-320">The function of the relevance score is to rank higher those documents that best answer a user question as expressed by the search query.</span></span> <span data-ttu-id="b732c-321">Výpočet skóre je založen na statistické vlastnosti podmínky, které odpovídá.</span><span class="sxs-lookup"><span data-stu-id="b732c-321">The score is computed based on statistical properties of terms that matched.</span></span> <span data-ttu-id="b732c-322">Základem vyhodnocování vzorec je [TF/IDF (termín frekvence inverzní dokumentu frekvenci)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).</span><span class="sxs-lookup"><span data-stu-id="b732c-322">At the core of the scoring formula is [TF/IDF (term frequency-inverse document frequency)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).</span></span> <span data-ttu-id="b732c-323">V dotazech obsahující výjimečná a běžné podmínky TF/IDF zvýší úroveň výsledky obsahující výjimečných podmínek.</span><span class="sxs-lookup"><span data-stu-id="b732c-323">In queries containing rare and common terms, TF/IDF promotes results containing the rare term.</span></span> <span data-ttu-id="b732c-324">Například v hypotetický index s všechny články Wikipedia z dokumentů odpovídající zadaným dotaz *ředitel*, dokumentů, které vyhovují na *ředitel* jsou považovány za relevantní více než dokumenty porovnávání s **.</span><span class="sxs-lookup"><span data-stu-id="b732c-324">For example, in a hypothetical index with all Wikipedia articles, from documents that matched the query *the president*, documents matching on *president* are considered more relevant than documents matching on *the*.</span></span>


### <a name="scoring-example"></a><span data-ttu-id="b732c-325">Příklad vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="b732c-325">Scoring example</span></span>

<span data-ttu-id="b732c-326">Odvolat tři dokumenty, které odpovídá náš příklad dotazu:</span><span class="sxs-lookup"><span data-stu-id="b732c-326">Recall the three documents that matched our example query:</span></span>
~~~~
search=Spacious, air-condition* +"Ocean view"  
~~~~
~~~~
{  
  "value": [
    {
      "@search.score": 0.25610128,
      "id": "1",
      "title": "Hotel Atman",
      "description": "Spacious rooms, ocean view, walking distance to the beach."
    },
    {
      "@search.score": 0.08951007,
      "id": "3",
      "title": "Playa Hotel",
      "description": "Comfortable, air-conditioned rooms with ocean view."
    },
    {
      "@search.score": 0.05967338,
      "id": "2",
      "title": "Ocean Resort",
      "description": "Located on a cliff on the north shore of the island of Kauai. Ocean view."
    }
  ]
}
~~~~

<span data-ttu-id="b732c-327">Dokument 1 namapovat dotaz nejlépe, protože obě termín *velké* a požadované frázi *oceánu zobrazení* dojít do pole Popis.</span><span class="sxs-lookup"><span data-stu-id="b732c-327">Document 1 matched the query best because both the term *spacious* and the required phrase *ocean view* occur in the description field.</span></span> <span data-ttu-id="b732c-328">Následující dva dokumenty odpovídat jenom fráze *oceánu zobrazení*.</span><span class="sxs-lookup"><span data-stu-id="b732c-328">The next two documents match only the phrase *ocean view*.</span></span> <span data-ttu-id="b732c-329">Může to být překvapivé, relevance skóre pro dokument 2 a 3 je jiné, přestože se shodoval dotaz stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="b732c-329">It might be surprising that the relevance score for document 2 and 3 is different even though they matched the query in the same way.</span></span> <span data-ttu-id="b732c-330">Je to proto, že vyhodnocování vzorec obsahuje více součástí než právě TF/IDF.</span><span class="sxs-lookup"><span data-stu-id="b732c-330">It's because the scoring formula has more components than just TF/IDF.</span></span> <span data-ttu-id="b732c-331">V takovém případě dokumentu 3 byl přiřazen mírně vyšší skóre, protože jeho popis je kratší.</span><span class="sxs-lookup"><span data-stu-id="b732c-331">In this case, document 3 was assigned a slightly higher score because its description is shorter.</span></span> <span data-ttu-id="b732c-332">Další informace o [Lucene je praktické vyhodnocování vzorec](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) pochopit, jak můžete skóre relevance ovlivnit délka pole a dalších faktorů.</span><span class="sxs-lookup"><span data-stu-id="b732c-332">Learn about [Lucene's Practical Scoring Formula](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) to understand how field length and other factors can influence the relevance score.</span></span>

<span data-ttu-id="b732c-333">Některé typy (zástupný znak, předpony, regulární výraz) dotazů vždy přispívat konstantní skóre na celkové skóre dokumentu.</span><span class="sxs-lookup"><span data-stu-id="b732c-333">Some query types (wildcard, prefix, regex) always contribute a constant score to the overall document score.</span></span> <span data-ttu-id="b732c-334">To umožňuje odpovídá nalezen prostřednictvím rozšíření dotazu mají být zahrnuty do výsledků, ale bez vlivu hodnocení.</span><span class="sxs-lookup"><span data-stu-id="b732c-334">This allows matches found through query expansion to be included in the results, but without affecting the ranking.</span></span> 

<span data-ttu-id="b732c-335">Příklad ukazuje, proč to záleží.</span><span class="sxs-lookup"><span data-stu-id="b732c-335">An example illustrates why this matters.</span></span> <span data-ttu-id="b732c-336">Vyhledávání pomocí zástupných znaků, včetně předpony hledání, jsou nejednoznačné podle definice, protože vstup je částečné řetězce s potenciální odpovídá na velký počet různorodých podmínky (zvažte vstup "prohlídka *", s odpovídá na "kurzy", "tourettes" nalezena, a " tourmaline").</span><span class="sxs-lookup"><span data-stu-id="b732c-336">Wildcard searches, including prefix searches, are ambiguous by definition because the input is a partial string with potential matches on a very large number of disparate terms (consider an input of "tour*", with matches found on “tours”, “tourettes”, and “tourmaline”).</span></span> <span data-ttu-id="b732c-337">Vzhledem k povaze těchto výsledků, neexistuje žádný způsob, jak to bude přiměřeně infer – které podmínky jsou cenné více než jiné.</span><span class="sxs-lookup"><span data-stu-id="b732c-337">Given the nature of these results, there is no way to reasonably infer which terms are more valuable than others.</span></span> <span data-ttu-id="b732c-338">Z tohoto důvodu jsme ignorovat termín frekvence, při vyhodnocování výsledky v dotazech typy zástupný znak, předpony a regulární výraz.</span><span class="sxs-lookup"><span data-stu-id="b732c-338">For this reason, we ignore term frequencies when scoring results in queries of types wildcard, prefix and regex.</span></span> <span data-ttu-id="b732c-339">V žádosti o více částech hledání obsahující částečným i úplným podmínky jsou součástí výsledků z částečné vstup s konstantní skóre, aby se zabránilo odchylka směrem potenciálně neočekávané odpovídá.</span><span class="sxs-lookup"><span data-stu-id="b732c-339">In a multi-part search request that includes partial and complete terms, results from the partial input are incorporated with a constant score to avoid bias towards potentially unexpected matches.</span></span>

### <a name="score-tuning"></a><span data-ttu-id="b732c-340">Skóre ladění</span><span class="sxs-lookup"><span data-stu-id="b732c-340">Score tuning</span></span>

<span data-ttu-id="b732c-341">Existují dva způsoby, jak ladit relevance skóre ve službě Azure Search:</span><span class="sxs-lookup"><span data-stu-id="b732c-341">There are two ways to tune relevance scores in Azure Search:</span></span>

1. <span data-ttu-id="b732c-342">**Vyhodnocování profily** povýšit dokumenty v seřazený seznam výsledků na základě sady pravidel.</span><span class="sxs-lookup"><span data-stu-id="b732c-342">**Scoring profiles** promote documents in the ranked list of results based on a set of rules.</span></span> <span data-ttu-id="b732c-343">V našem příkladu jsme zvažte dokumenty, které odpovídá v poli Název relevantnější než dokumenty, které odpovídá do pole Popis.</span><span class="sxs-lookup"><span data-stu-id="b732c-343">In our example, we could consider documents that matched in the title field more relevant than documents that matched in the description field.</span></span> <span data-ttu-id="b732c-344">Kromě toho Pokud index měli pole ceny pro každý hotelů, jsme může zvýšit úroveň dokumenty s nižší cenou.</span><span class="sxs-lookup"><span data-stu-id="b732c-344">Additionally, if our index had a price field for each hotel, we could promote documents with lower price.</span></span> <span data-ttu-id="b732c-345">Zjistěte, jak Další [profily vyhodnocování přidat do indexu vyhledávání.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)</span><span class="sxs-lookup"><span data-stu-id="b732c-345">Learn more how to [add Scoring Profiles to a search index.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)</span></span>
2. <span data-ttu-id="b732c-346">**Zvýšení skóre termínu** (k dispozici pouze v syntaxe dotazů Lucene úplná) poskytuje zvýšení skóre operátor `^` , který lze použít na všechny části stromu dotazu.</span><span class="sxs-lookup"><span data-stu-id="b732c-346">**Term boosting** (available only in the Full Lucene query syntax) provides a boosting operator `^` that can be applied to any part of the query tree.</span></span> <span data-ttu-id="b732c-347">V našem příkladu, namísto hledání na předponě *air-condition*\*, jeden může hledat buď přesnou termín *air-condition* nebo předponu, ale dokumenty, které odpovídají na přesný termín řazeny výše použitím nárůst termín dotazu: *letecké podmínku ^ 2 || AIR-condition**.</span><span class="sxs-lookup"><span data-stu-id="b732c-347">In our example, instead of searching on the prefix *air-condition*\*, one could search for either the exact term *air-condition* or the prefix, but documents that match on the exact term are ranked higher by applying boost to the term query: *air-condition^2||air-condition**.</span></span> <span data-ttu-id="b732c-348">Další informace o [zvýšení skóre termínu](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).</span><span class="sxs-lookup"><span data-stu-id="b732c-348">Learn more about [term boosting](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).</span></span>


### <a name="scoring-in-a-distributed-index"></a><span data-ttu-id="b732c-349">Vyhodnocování v distribuované indexu</span><span class="sxs-lookup"><span data-stu-id="b732c-349">Scoring in a distributed index</span></span>

<span data-ttu-id="b732c-350">Všechny indexy ve službě Azure Search se automaticky rozdělí do víc horizontálních oddílů, abychom mohli rychle distribuovat index mezi několika uzly při škálování služby nahoru i dolů.</span><span class="sxs-lookup"><span data-stu-id="b732c-350">All indexes in Azure Search are automatically split into multiple shards, allowing us to quickly distribute the index among multiple nodes during service scale up or scale down.</span></span> <span data-ttu-id="b732c-351">Když je vydaný žádost o vyhledávání, jeho vydání před každou horizontálního oddílu nezávisle.</span><span class="sxs-lookup"><span data-stu-id="b732c-351">When a search request is issued, it’s issued against each shard independently.</span></span> <span data-ttu-id="b732c-352">Výsledky z každé horizontálního oddílu jsou pak sloučit a seřazené podle skóre (Pokud není definováno žádné jiné pořadí).</span><span class="sxs-lookup"><span data-stu-id="b732c-352">The results from each shard are then merged and ordered by score (if no other ordering is defined).</span></span> <span data-ttu-id="b732c-353">Je důležité vědět, že vyhodnocování vah funkce dotazování termín frekvence před jeho frekvence inverzní dokumentu v všechny dokumenty v rámci horizontálního oddílu, není mezi všechny horizontálních oddílů!</span><span class="sxs-lookup"><span data-stu-id="b732c-353">It is important to know that the scoring function weights query term frequency against its inverse document frequency in all documents within the shard, not across all shards!</span></span>

<span data-ttu-id="b732c-354">To znamená, relevance skóre *může* být různé pro identické dokumenty, pokud budou umístěny v různých horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="b732c-354">This means a relevance score *could* be different for identical documents if they reside on different shards.</span></span> <span data-ttu-id="b732c-355">Naštěstí tyto rozdíly jsou obvykle zmizí z důvodu další i termín distribuční růstem počet dokumentů v indexu.</span><span class="sxs-lookup"><span data-stu-id="b732c-355">Fortunately, such differences tend to disappear as the number of documents in the index grows due to more even term distribution.</span></span> <span data-ttu-id="b732c-356">Není možné předpokládají, na které horizontálního oddílu se umístí všechny daného dokumentu.</span><span class="sxs-lookup"><span data-stu-id="b732c-356">It’s not possible to assume on which shard any given document will be placed.</span></span> <span data-ttu-id="b732c-357">Ale za předpokladu, že klíč dokumentu nemění, jej bude vždy přiřadit stejné ID horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="b732c-357">However, assuming a document key doesn't change, it will always be assigned to the same shard.</span></span>

<span data-ttu-id="b732c-358">Obecně platí skóre dokumentů není nejlepší atribut pro řazení dokumenty, pokud stabilitu pořadí je důležité.</span><span class="sxs-lookup"><span data-stu-id="b732c-358">In general, document score is not the best attribute for ordering documents if order stability is important.</span></span> <span data-ttu-id="b732c-359">Například zadány dvě dokument s identické skóre, neexistuje žádná záruka, která jako první se objeví v následných spustí stejný dotaz.</span><span class="sxs-lookup"><span data-stu-id="b732c-359">For example, given two document with an identical score, there is no guarantee which one appears first in subsequent runs of the same query.</span></span> <span data-ttu-id="b732c-360">V sadě výsledků skóre dokumentu pouze získat obecný přehled o dokument relevance relativně k jiné dokumenty.</span><span class="sxs-lookup"><span data-stu-id="b732c-360">Document score should only give a general sense of document relevance relative to other documents in the results set.</span></span>

## <a name="conclusion"></a><span data-ttu-id="b732c-361">Závěr</span><span class="sxs-lookup"><span data-stu-id="b732c-361">Conclusion</span></span>

<span data-ttu-id="b732c-362">Úspěch internet vyhledávací weby vyvolalo očekávání pro fulltextové vyhledávání přes osobní data.</span><span class="sxs-lookup"><span data-stu-id="b732c-362">The success of internet search engines has raised expectations for full text search over private data.</span></span> <span data-ttu-id="b732c-363">Pro téměř k libovolnému druhu vyhledávání nyní Očekáváme, že modul pochopit naše záměr, i když podmínky jsou uvedeny překlepu nebo jsou neúplné.</span><span class="sxs-lookup"><span data-stu-id="b732c-363">For almost any kind of search experience, we now expect the engine to understand our intent, even when terms are misspelled or incomplete.</span></span> <span data-ttu-id="b732c-364">Může i Očekáváme, že shody na základě téměř ekvivalentní termíny nebo synonyma, které jsme zadali ve skutečnosti.</span><span class="sxs-lookup"><span data-stu-id="b732c-364">We might even expect matches based on near equivalent terms or synonyms that we never actually specified.</span></span>

<span data-ttu-id="b732c-365">Z hlediska technické fulltextové vyhledávání je vysoce komplexní, vyžadují pokročilé analýzy lingvistické a systematicky pro zpracování způsoby, které generovat, rozbalit a transformace vyhledávacích dotazů k poskytování výsledku relevantní.</span><span class="sxs-lookup"><span data-stu-id="b732c-365">From a technical standpoint, full text search is highly complex, requiring sophisticated linguistic analysis and a systematic approach to processing in ways that distill, expand, and transform query terms to deliver a relevant result.</span></span> <span data-ttu-id="b732c-366">Vzhledem ke vyplývajících složitosti, je celá řada faktorů, které můžou ovlivnit výsledek dotazu.</span><span class="sxs-lookup"><span data-stu-id="b732c-366">Given the inherent complexities, there are a lot of factors that can affect the outcome of a query.</span></span> <span data-ttu-id="b732c-367">Z tohoto důvodu začne investovat dobu pochopit mechanismů fulltextové vyhledávání nabízí konkrétní výhody, při pokusu o fungovat prostřednictvím neočekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="b732c-367">For this reason, investing the time to understand the mechanics of full text search offers tangible benefits when trying to work through unexpected results.</span></span>  

<span data-ttu-id="b732c-368">Tento článek prozkoumali fulltextového vyhledávání v rámci Azure Search.</span><span class="sxs-lookup"><span data-stu-id="b732c-368">This article explored full text search in the context of Azure Search.</span></span> <span data-ttu-id="b732c-369">Věříme, že poskytuje dostatečnou pozadí rozpoznat možné příčiny a řešení pro řešení běžných problémů s dotazu.</span><span class="sxs-lookup"><span data-stu-id="b732c-369">We hope it gives you sufficient background to recognize potential causes and resolutions for addressing common query problems.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b732c-370">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b732c-370">Next steps</span></span>

+ <span data-ttu-id="b732c-371">Sestavení indexu ukázkové, vyzkoušejte různé dotazy a zkontrolovat výsledky.</span><span class="sxs-lookup"><span data-stu-id="b732c-371">Build the sample index, try out different queries and review results.</span></span> <span data-ttu-id="b732c-372">Pokyny najdete v tématu [sestavení a dotazování indexu na portálu](search-get-started-portal.md#query-index).</span><span class="sxs-lookup"><span data-stu-id="b732c-372">For instructions, see [Build and query an index in the portal](search-get-started-portal.md#query-index).</span></span>

+ <span data-ttu-id="b732c-373">Zkuste syntaxe další dotazu z [vyhledávání dokumentů](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) příklad části nebo z [jednoduchá syntaxe dotazů](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) v Průzkumník služby Search na portálu.</span><span class="sxs-lookup"><span data-stu-id="b732c-373">Try additional query syntax from the [Search Documents](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) example section or from [Simple query syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) in Search explorer in the portal.</span></span>

+ <span data-ttu-id="b732c-374">Zkontrolujte [vyhodnocování profily](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) Pokud chcete ladit hodnocení v aplikaci vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="b732c-374">Review [scoring profiles](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) if you want to tune ranking in your search application.</span></span>

+ <span data-ttu-id="b732c-375">Zjistěte, jak použít [lexikální analyzátory jazyka](https://docs.microsoft.com/rest/api/searchservice/language-support).</span><span class="sxs-lookup"><span data-stu-id="b732c-375">Learn how to apply [language-specific lexical analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support).</span></span>

+ <span data-ttu-id="b732c-376">[Konfigurace vlastní analyzátorů](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) pro minimální zpracování nebo speciální zpracování na konkrétních polí.</span><span class="sxs-lookup"><span data-stu-id="b732c-376">[Configure custom analyzers](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) for either minimal processing or specialized processing on specific fields.</span></span>

+ <span data-ttu-id="b732c-377">[Porovnání standardní a anglické analyzátorů](http://alice.unearth.ai/))-souběžného na tento ukázkový web.</span><span class="sxs-lookup"><span data-stu-id="b732c-377">[Compare standard and English analyzers](http://alice.unearth.ai/)) side-by-side on this demo web site.</span></span> 

## <a name="see-also"></a><span data-ttu-id="b732c-378">Viz také</span><span class="sxs-lookup"><span data-stu-id="b732c-378">See also</span></span>

[<span data-ttu-id="b732c-379">Hledání dokumentů rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="b732c-379">Search Documents REST API</span></span>](https://docs.microsoft.com/rest/api/searchservice/search-documents)

[<span data-ttu-id="b732c-380">Jednoduchá syntaxe dotazů</span><span class="sxs-lookup"><span data-stu-id="b732c-380">Simple query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)

[<span data-ttu-id="b732c-381">Úplná syntaxe dotazů Lucene</span><span class="sxs-lookup"><span data-stu-id="b732c-381">Full Lucene query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

[<span data-ttu-id="b732c-382">Zpracování výsledků vyhledávání</span><span class="sxs-lookup"><span data-stu-id="b732c-382">Handle search results</span></span>](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
[2]: ./media/search-lucene-query-architecture/azSearch-queryparsing-should2.png
[3]: ./media/search-lucene-query-architecture/azSearch-queryparsing-must2.png
[4]: ./media/search-lucene-query-architecture/azSearch-queryparsing-spacious2.png
