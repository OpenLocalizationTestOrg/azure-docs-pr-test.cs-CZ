---
title: "aaaFull text search engine (Lucene) architektura ve službě Azure Search | Microsoft Docs"
description: "Vysvětlení Lucene dotaz zpracování a dokumentu načtení koncepty pro fulltextové vyhledávání, jako související tooAzure vyhledávání."
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
ms.openlocfilehash: c6d1bea8d40154fd9237b9e44584cdfcd193cbd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-full-text-search-works-in-azure-search"></a><span data-ttu-id="0eed1-103">Jak úplné textové vyhledávání funguje ve službě Azure Search</span><span class="sxs-lookup"><span data-stu-id="0eed1-103">How full text search works in Azure Search</span></span>

<span data-ttu-id="0eed1-104">Tento článek je pro vývojáře, kteří potřebují lépe pochopili, jak funguje Lucene fulltextové vyhledávání ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="0eed1-104">This article is for developers who need a deeper understanding of how Lucene full text search works in Azure Search.</span></span> <span data-ttu-id="0eed1-105">Pro dotazy text Azure Search ve většině scénářů bezproblémově dodá očekávané výsledky, ale někdy může získat výsledek, který se zdá, že "off" nějakým způsobem.</span><span class="sxs-lookup"><span data-stu-id="0eed1-105">For text queries, Azure Search will seamlessly deliver expected results in most scenarios, but occasionally you might get a result that seems "off" somehow.</span></span> <span data-ttu-id="0eed1-106">V těchto situacích s pozadí ve hello čtyři fáze provádění dotazů Lucene (dotaz analýzy, lexikální analýzy, dokumentů párování, vyhodnocování) vám může pomoct identifikovat konkrétní změny tooquery parametry nebo index konfigurace, která bude poskytovat hello požadovaný výsledek.</span><span class="sxs-lookup"><span data-stu-id="0eed1-106">In these situations, having a background in hello four stages of Lucene query execution (query parsing, lexical analysis, document matching, scoring) can help you identify specific changes tooquery parameters or index configuration that will deliver hello desired outcome.</span></span> 

> [!Note] 
> <span data-ttu-id="0eed1-107">Služba Azure Search používá Lucene pro fulltextové vyhledávání, ale Lucene integrace není vyčerpávající.</span><span class="sxs-lookup"><span data-stu-id="0eed1-107">Azure Search uses Lucene for full text search, but Lucene integration is not exhaustive.</span></span> <span data-ttu-id="0eed1-108">Jsme selektivně vystavení a rozšířit Lucene funkce tooenable hello scénáře důležité tooAzure vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="0eed1-108">We selectively expose and extend Lucene functionality tooenable hello scenarios important tooAzure Search.</span></span> 

## <a name="architecture-overview-and-diagram"></a><span data-ttu-id="0eed1-109">Přehled architektury a diagram</span><span class="sxs-lookup"><span data-stu-id="0eed1-109">Architecture overview and diagram</span></span>

<span data-ttu-id="0eed1-110">Zpracování textu v plném znění vyhledávací dotaz začíná analýza hello dotazu text tooextract hledaným výrazům.</span><span class="sxs-lookup"><span data-stu-id="0eed1-110">Processing a full text search query starts with parsing hello query text tooextract search terms.</span></span> <span data-ttu-id="0eed1-111">Vyhledávací Hello používá indexování dokumentů tooretrieve s odpovídající podmínky.</span><span class="sxs-lookup"><span data-stu-id="0eed1-111">hello search engine uses an index tooretrieve documents with matching terms.</span></span> <span data-ttu-id="0eed1-112">Konkrétní dotaz podmínky jsou někdy rozdělit a obnovených do nového formuláře toocast širší net přes co může považovat za potenciální shody.</span><span class="sxs-lookup"><span data-stu-id="0eed1-112">Individual query terms are sometimes broken down and reconstituted into new forms toocast a broader net over what could be considered as a potential match.</span></span> <span data-ttu-id="0eed1-113">Je sada výsledků dotazu je pak seřazené podle relevance skóre přiřazené tooeach jednotlivých odpovídající dokumentu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-113">A result set is then sorted by a relevance score assigned tooeach individual matching document.</span></span> <span data-ttu-id="0eed1-114">Ty hello horní části hello seznam hodnocení jsou vráceny toohello volající aplikace.</span><span class="sxs-lookup"><span data-stu-id="0eed1-114">Those at hello top of hello ranked list are returned toohello calling application.</span></span>

<span data-ttu-id="0eed1-115">Revidovat, provádění dotazů má čtyři fáze:</span><span class="sxs-lookup"><span data-stu-id="0eed1-115">Restated, query execution has four stages:</span></span> 

1. <span data-ttu-id="0eed1-116">Analýza dotazu</span><span class="sxs-lookup"><span data-stu-id="0eed1-116">Query parsing</span></span> 
2. <span data-ttu-id="0eed1-117">Lexikální analýzy</span><span class="sxs-lookup"><span data-stu-id="0eed1-117">Lexical analysis</span></span> 
3. <span data-ttu-id="0eed1-118">Načtení dokumentu</span><span class="sxs-lookup"><span data-stu-id="0eed1-118">Document retrieval</span></span> 
4. <span data-ttu-id="0eed1-119">Vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="0eed1-119">Scoring</span></span> 

<span data-ttu-id="0eed1-120">Hello obrázek níže znázorňuje hello komponenty používané tooprocess žádost o vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="0eed1-120">hello diagram below illustrates hello components used tooprocess a search request.</span></span> 

 ![Diagram architektury dotazů Lucene ve službě Azure Search][1]


| <span data-ttu-id="0eed1-122">Klíčové komponenty</span><span class="sxs-lookup"><span data-stu-id="0eed1-122">Key components</span></span> | <span data-ttu-id="0eed1-123">Funkční popis</span><span class="sxs-lookup"><span data-stu-id="0eed1-123">Functional description</span></span> | 
|----------------|------------------------|
|<span data-ttu-id="0eed1-124">**Dotaz analyzátory**</span><span class="sxs-lookup"><span data-stu-id="0eed1-124">**Query parsers**</span></span> | <span data-ttu-id="0eed1-125">Operátory dotazu nezávislá na infrastruktuře vyhledávacích dotazů a vytvořte hello dotazu struktury (stromové dotazu) toobe odeslané toohello vyhledávací web.</span><span class="sxs-lookup"><span data-stu-id="0eed1-125">Separate query terms from query operators and create hello query structure (a query tree) toobe sent toohello search engine.</span></span> |
|<span data-ttu-id="0eed1-126">**Analyzátory**</span><span class="sxs-lookup"><span data-stu-id="0eed1-126">**Analyzers**</span></span> | <span data-ttu-id="0eed1-127">Lexikální analýzám na vyhledávacích dotazů.</span><span class="sxs-lookup"><span data-stu-id="0eed1-127">Perform lexical analysis on query terms.</span></span> <span data-ttu-id="0eed1-128">Tento proces může zahrnovat transformace, odebrání nebo rozšiřování vyhledávacích dotazů.</span><span class="sxs-lookup"><span data-stu-id="0eed1-128">This process can involve transforming, removing, or expanding of query terms.</span></span> |
|<span data-ttu-id="0eed1-129">**Index**</span><span class="sxs-lookup"><span data-stu-id="0eed1-129">**Index**</span></span> | <span data-ttu-id="0eed1-130">Strukturu efektivní dat používá toostore a uspořádat prohledávatelné podmínky extrahovat z indexovaného dokumentů.</span><span class="sxs-lookup"><span data-stu-id="0eed1-130">An efficient data structure used toostore and organize searchable terms extracted from indexed documents.</span></span> |
|<span data-ttu-id="0eed1-131">**Vyhledávací web**</span><span class="sxs-lookup"><span data-stu-id="0eed1-131">**Search engine**</span></span> | <span data-ttu-id="0eed1-132">Načítá a skóre dokumentů na základě obsahu hello hello odpovídající obrácený index.</span><span class="sxs-lookup"><span data-stu-id="0eed1-132">Retrieves and scores matching documents based on hello contents of hello inverted index.</span></span> |

## <a name="anatomy-of-a-search-request"></a><span data-ttu-id="0eed1-133">Anatomie žádost o vyhledávání</span><span class="sxs-lookup"><span data-stu-id="0eed1-133">Anatomy of a search request</span></span>

<span data-ttu-id="0eed1-134">Žádost o vyhledávání je dokončení specifikaci co má být vrácen sadě výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-134">A search request is a complete specification of what should be returned in a result set.</span></span> <span data-ttu-id="0eed1-135">V nejjednodušší podobě je prázdný dotaz s žádná kritéria jakéhokoli druhu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-135">In simplest form, it is an empty query with no criteria of any kind.</span></span> <span data-ttu-id="0eed1-136">Realističtější příklad obsahuje parametry, několik dotazu podmínky, případně vymezená toocertain pole, může být výraz filtru a řazení pravidla.</span><span class="sxs-lookup"><span data-stu-id="0eed1-136">A more realistic example includes parameters, several query terms, perhaps scoped toocertain fields, with possibly a filter expression and ordering rules.</span></span>  

<span data-ttu-id="0eed1-137">Hello následující příklad je žádost o vyhledávání může odeslat tooAzure vyhledávání pomocí hello [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span><span class="sxs-lookup"><span data-stu-id="0eed1-137">hello following example is a search request you might send tooAzure Search using hello [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span></span>  

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

<span data-ttu-id="0eed1-138">Pro tento požadavek hello vyhledávacího webu hello následující:</span><span class="sxs-lookup"><span data-stu-id="0eed1-138">For this request, hello search engine does hello following:</span></span>

1. <span data-ttu-id="0eed1-139">Filtry dokumentů, kde je cena hello alespoň $60 a menší než 300 USD.</span><span class="sxs-lookup"><span data-stu-id="0eed1-139">Filters out documents where hello price is at least $60 and less than $300.</span></span>
2. <span data-ttu-id="0eed1-140">Provede dotaz hello.</span><span class="sxs-lookup"><span data-stu-id="0eed1-140">Executes hello query.</span></span> <span data-ttu-id="0eed1-141">V tomto příkladu hello vyhledávací dotaz se skládá z frází a podmínky: `"Spacious, air-condition* +\"Ocean view\""` (uživatelé obvykle nezadávejte interpunkční znaménka, ale včetně v příkladu hello umožňuje tooexplain jak ji zpracovat analyzátorů).</span><span class="sxs-lookup"><span data-stu-id="0eed1-141">In this example, hello search query consists of phrases and terms: `"Spacious, air-condition* +\"Ocean view\""` (users typically don't enter punctuation, but including it in hello example allows us tooexplain how analyzers handle it).</span></span> <span data-ttu-id="0eed1-142">Pro tento dotaz hello vyhledávacího webu vyhledá hello popis a název pole zadaný v `searchFields` pro dokumenty, které obsahují "Oceánu zobrazení" a dále na hello termín "velké" nebo na podmínky, které začínají předponou hello "air-condition".</span><span class="sxs-lookup"><span data-stu-id="0eed1-142">For this query, hello search engine scans hello description and title fields specified in `searchFields` for documents that contain "Ocean view", and additionally on hello term "spacious", or on terms that start with hello prefix "air-condition".</span></span> <span data-ttu-id="0eed1-143">Hello `searchMode` parametr je použité toomatch na všechny termín (výchozí) nebo všechny z nich pro případy, kdy není explicitně požadované termín (`+`).</span><span class="sxs-lookup"><span data-stu-id="0eed1-143">hello `searchMode` parameter is used toomatch on any term (default) or all of them, for cases where a term is not explicitly required (`+`).</span></span>
3. <span data-ttu-id="0eed1-144">Objednávky hello výslednou sadu hotels pomocí bezkontaktní tooa zadané geografické umístění a poté vrácen toohello volající aplikace.</span><span class="sxs-lookup"><span data-stu-id="0eed1-144">Orders hello resulting set of hotels by proximity tooa given geography location, and then returned toohello calling application.</span></span> 

<span data-ttu-id="0eed1-145">Hello většina Tento článek se týká zpracování hello *vyhledávací dotaz*: `"Spacious, air-condition* +\"Ocean view\""`.</span><span class="sxs-lookup"><span data-stu-id="0eed1-145">hello majority of this article is about processing of hello *search query*: `"Spacious, air-condition* +\"Ocean view\""`.</span></span> <span data-ttu-id="0eed1-146">Filtrování a řazení jsou mimo rozsah.</span><span class="sxs-lookup"><span data-stu-id="0eed1-146">Filtering and ordering are out of scope.</span></span> <span data-ttu-id="0eed1-147">Další informace najdete v tématu hello [referenční dokumentace rozhraní API pro vyhledávání](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span><span class="sxs-lookup"><span data-stu-id="0eed1-147">For more information, see hello [Search API reference documentation](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span></span>

<a name="stage1"></a>
## <a name="stage-1-query-parsing"></a><span data-ttu-id="0eed1-148">Fáze 1: Analýza dotazu</span><span class="sxs-lookup"><span data-stu-id="0eed1-148">Stage 1: Query parsing</span></span> 

<span data-ttu-id="0eed1-149">Jak jsme uvedli, řetězec dotazu hello je první řádek hello hello žádosti:</span><span class="sxs-lookup"><span data-stu-id="0eed1-149">As noted, hello query string is hello first line of hello request:</span></span> 

~~~~
 "search": "Spacious, air-condition* +\"Ocean view\"", 
~~~~

<span data-ttu-id="0eed1-150">Analyzátor odděluje operátory dotazu Hello (například `*` a `+` v příkladu hello) z hledání podmínky a deconstructs hello vyhledávací dotaz do *poddotazy* podporovaného typu:</span><span class="sxs-lookup"><span data-stu-id="0eed1-150">hello query parser separates operators (such as `*` and `+` in hello example) from search terms, and deconstructs hello search query into *subqueries* of a supported type:</span></span> 

+ <span data-ttu-id="0eed1-151">*Termín dotazu* pro samostatné podmínky (třeba velké)</span><span class="sxs-lookup"><span data-stu-id="0eed1-151">*term query* for standalone terms (like spacious)</span></span>
+ <span data-ttu-id="0eed1-152">*dotaz frázi* uvozovkách podmínek (podobně jako zobrazení oceánu)</span><span class="sxs-lookup"><span data-stu-id="0eed1-152">*phrase query* for quoted terms (like ocean view)</span></span>
+ <span data-ttu-id="0eed1-153">*Předpona dotazu* podmínek, za nímž následuje operátor předponu `*` (jako jsou air-condition)</span><span class="sxs-lookup"><span data-stu-id="0eed1-153">*prefix query* for terms followed by a prefix operator `*` (like air-condition)</span></span>

<span data-ttu-id="0eed1-154">Úplný seznam podporovaných dotazu typy najdete v části [sytnax dotazů Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)</span><span class="sxs-lookup"><span data-stu-id="0eed1-154">For a full list of supported query types see [Lucene query sytnax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)</span></span>

<span data-ttu-id="0eed1-155">Operátory přidružené poddotazu určit, zda dotaz hello "musí být" nebo "by měla být" splněna v pořadí pro dokument toobe považovány za shodné.</span><span class="sxs-lookup"><span data-stu-id="0eed1-155">Operators associated with a subquery determine whether hello query "must be" or "should be" satisfied in order for a document toobe considered a match.</span></span> <span data-ttu-id="0eed1-156">Například `+"Ocean view"` je "musí" kvůli toohello `+` operátor.</span><span class="sxs-lookup"><span data-stu-id="0eed1-156">For example, `+"Ocean view"` is "must" due toohello `+` operator.</span></span> 

<span data-ttu-id="0eed1-157">Analyzátor dotazu Hello ke změně struktury hello poddotazy do *dotazu stromu* (interní strukturu představující dotazu hello) předá v toohello vyhledávací web.</span><span class="sxs-lookup"><span data-stu-id="0eed1-157">hello query parser restructures hello subqueries into a *query tree* (an internal structure representing hello query) it passes on toohello search engine.</span></span> <span data-ttu-id="0eed1-158">V první fázi hello dotazu analýza hello dotazu stromu vypadat třeba takto.</span><span class="sxs-lookup"><span data-stu-id="0eed1-158">In hello first stage of query parsing, hello query tree looks like this.</span></span>  

 ![Logická hodnota dotazu searchmode všechny][2]

### <a name="supported-parsers-simple-and-full-lucene"></a><span data-ttu-id="0eed1-160">Podporované analyzátory: jednoduchý a úplné Lucene</span><span class="sxs-lookup"><span data-stu-id="0eed1-160">Supported parsers: Simple and Full Lucene</span></span> 

 <span data-ttu-id="0eed1-161">Služba Azure Search zpřístupní dvou různých dotazu jazyků, `simple` (výchozí) a `full`.</span><span class="sxs-lookup"><span data-stu-id="0eed1-161">Azure Search exposes two different query languages, `simple` (default) and `full`.</span></span> <span data-ttu-id="0eed1-162">Pomocí nastavení hello `queryType` parametr s vaši žádost o vyhledávání se zjistit analyzátor dotazu hello jazyk dotazu, který zvolíte, aby věděl, že může jak toointerpret hello operátory a syntaxe.</span><span class="sxs-lookup"><span data-stu-id="0eed1-162">By setting hello `queryType` parameter with your search request, you tell hello query parser which query language you choose so that it knows how toointerpret hello operators and syntax.</span></span> <span data-ttu-id="0eed1-163">Hello [jednoduchý dotaz jazyka](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) je intuitivní a robustní, často vhodný toointerpret uživatelský vstup jako-je bez zpracování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="0eed1-163">hello [Simple query langauge](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) is intuitive and robust, often suitable toointerpret user input as-is without client-side processing.</span></span> <span data-ttu-id="0eed1-164">Podporuje operátory dotazu známých z webové vyhledávacích webů.</span><span class="sxs-lookup"><span data-stu-id="0eed1-164">It supports query operators familiar from web search engines.</span></span> <span data-ttu-id="0eed1-165">Hello [úplné Lucene dotazovací jazyk](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), které dostanete tak, že nastavení `queryType=full`, rozšiřuje hello výchozí jednoduchý dotaz jazyk přidáním podpory pro další operátory a typy dotazů jako zástupný znak, přibližné, regulární výraz a dotazy v rámci pole.</span><span class="sxs-lookup"><span data-stu-id="0eed1-165">hello [Full Lucene query language](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), which you get by setting `queryType=full`, extends hello default Simple query language by adding support for more operators and query types like wildcard, fuzzy, regex, and field-scoped queries.</span></span> <span data-ttu-id="0eed1-166">Například by odeslaných za jednoduchá syntaxe dotazů regulární výraz vyhodnocen jako řetězec dotazu a není výraz.</span><span class="sxs-lookup"><span data-stu-id="0eed1-166">For example, a regular expression sent in Simple query syntax would be interpreted as a query string and not an expression.</span></span> <span data-ttu-id="0eed1-167">žádost o Hello příklad v tomto článku používá hello úplné Lucene dotazovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="0eed1-167">hello example request in this article uses hello Full Lucene query language.</span></span>

### <a name="impact-of-searchmode-on-hello-parser"></a><span data-ttu-id="0eed1-168">Dopad searchMode hello analyzátoru</span><span class="sxs-lookup"><span data-stu-id="0eed1-168">Impact of searchMode on hello parser</span></span> 

<span data-ttu-id="0eed1-169">Jiný parametr žádost o vyhledávání, který ovlivňuje analýza je hello `searchMode` parametr.</span><span class="sxs-lookup"><span data-stu-id="0eed1-169">Another search request parameter that affects parsing is hello `searchMode` parameter.</span></span> <span data-ttu-id="0eed1-170">Ovládá hello výchozí operátor pro logickou dotazy: všechny (výchozí) nebo všechny.</span><span class="sxs-lookup"><span data-stu-id="0eed1-170">It controls hello default operator for Boolean queries: any (default) or all.</span></span>  

<span data-ttu-id="0eed1-171">Když `searchMode=any`, který je hello výchozím hello místo odděleny velké a air-condition je nebo (`||`), provedení ekvivalentní hello ukázkový dotaz text:</span><span class="sxs-lookup"><span data-stu-id="0eed1-171">When `searchMode=any`, which is hello default, hello space delimiter between spacious and air-condition is OR (`||`), making hello sample query text equivalent to:</span></span> 

~~~~
Spacious,||air-condition*+"Ocean view" 
~~~~

<span data-ttu-id="0eed1-172">Explicitní operátory, jako například `+` v `+"Ocean view"`, jsou jednoznačné v dotazu Logická konstrukce (hello termín *musí* odpovídat).</span><span class="sxs-lookup"><span data-stu-id="0eed1-172">Explicit operators, such as `+` in `+"Ocean view"`, are unambiguous in boolean query construction (hello term *must* match).</span></span> <span data-ttu-id="0eed1-173">Jak toointerpret hello zbývající podmínky je méně zřejmé: velké a air-condition.</span><span class="sxs-lookup"><span data-stu-id="0eed1-173">Less obvious is how toointerpret hello remaining terms: spacious and air-condition.</span></span> <span data-ttu-id="0eed1-174">Měli hello vyhledávacího webu najít odpovídá na zobrazení oceánu *a* velké *a* air-condition?</span><span class="sxs-lookup"><span data-stu-id="0eed1-174">Should hello search engine find matches on ocean view *and* spacious *and* air-condition?</span></span> <span data-ttu-id="0eed1-175">Nebo by měl zjistit oceánu zobrazení plus *buď jeden* z hello zbývající podmínky?</span><span class="sxs-lookup"><span data-stu-id="0eed1-175">Or should it find ocean view plus *either one* of hello remaining terms?</span></span> 

<span data-ttu-id="0eed1-176">Ve výchozím nastavení (`searchMode=any`), vyhledávací hello předpokládá hello širší interpretace.</span><span class="sxs-lookup"><span data-stu-id="0eed1-176">By default (`searchMode=any`), hello search engine assumes hello broader interpretation.</span></span> <span data-ttu-id="0eed1-177">Buď pole *by měl* odpovídat, které odráží sémantiku "nebo".</span><span class="sxs-lookup"><span data-stu-id="0eed1-177">Either field *should* be matched, reflecting "or" semantics.</span></span> <span data-ttu-id="0eed1-178">Hello počátečního dotazu stromu zobrazené dříve, s hello dvě operace "by", ukazuje hello výchozí.</span><span class="sxs-lookup"><span data-stu-id="0eed1-178">hello initial query tree illustrated previously, with hello two "should" operations, shows hello default.</span></span>  

<span data-ttu-id="0eed1-179">Předpokládejme, že teď nastavujeme `searchMode=all`.</span><span class="sxs-lookup"><span data-stu-id="0eed1-179">Suppose that we now set `searchMode=all`.</span></span> <span data-ttu-id="0eed1-180">V takovém případě hello místo interpretována jako "a" operaci.</span><span class="sxs-lookup"><span data-stu-id="0eed1-180">In this case, hello space is interpreted as an "and" operation.</span></span> <span data-ttu-id="0eed1-181">Každý zbývající podmínky hello musí být součástí hello dokumentu tooqualify jako shoda.</span><span class="sxs-lookup"><span data-stu-id="0eed1-181">Each of hello remaining terms must both be present in hello document tooqualify as a match.</span></span> <span data-ttu-id="0eed1-182">Ukázkový dotaz výsledné Hello by interpretovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0eed1-182">hello resulting sample query would be interpreted as follows:</span></span> 

~~~~
+Spacious,+air-condition*+"Ocean view"  
~~~~

<span data-ttu-id="0eed1-183">Strom upravený dotaz pro tento dotaz bude následující, kde odpovídající dokument je hello průnik všechny tři poddotazy:</span><span class="sxs-lookup"><span data-stu-id="0eed1-183">A modified query tree for this query would be as follows, where a matching document is hello intersection of all three subqueries:</span></span> 

 ![Logická hodnota dotazu searchmode všechny][3]

> [!Note] 
> <span data-ttu-id="0eed1-185">Výběr `searchMode=any` přes `searchMode=all` je nejlepší rozhodnutí byly přijaty spuštěním reprezentativní dotazy.</span><span class="sxs-lookup"><span data-stu-id="0eed1-185">Choosing `searchMode=any` over `searchMode=all` is a decision best arrived at by running representative queries.</span></span> <span data-ttu-id="0eed1-186">Uživatelé, kteří jsou pravděpodobně tooinclude operátory (společný při vyhledávání dokument uloží) může intuitivnější výsledky pokud `searchMode=all` informuje konstrukce boolean dotazu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-186">Users who are likely tooinclude operators (common when searching document stores) might find results more intuitive if `searchMode=all` informs boolean query constructs.</span></span> <span data-ttu-id="0eed1-187">Další informace o tom hello vztahu mezi `searchMode` a operátory, najdete v části [jednoduchá syntaxe dotazů](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).</span><span class="sxs-lookup"><span data-stu-id="0eed1-187">For more about hello interplay between `searchMode` and operators, see [Simple query syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).</span></span>

<a name="stage2"></a>
## <a name="stage-2-lexical-analysis"></a><span data-ttu-id="0eed1-188">Fáze 2: Lexikální analysis</span><span class="sxs-lookup"><span data-stu-id="0eed1-188">Stage 2: Lexical analysis</span></span> 

<span data-ttu-id="0eed1-189">Lexikální analyzátorů proces *termín dotazy* a *fráze dotazy* po strukturovaná hello dotazu stromu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-189">Lexical analyzers process *term queries* and *phrase queries* after hello query tree is structured.</span></span> <span data-ttu-id="0eed1-190">Analyzátor přijme hello tooit zadané vstupy textu hello analyzátor, procesy hello text a pak zasílá zpět tokenizovaného podmínky, které toobe začleněná do hello stromu dotazu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-190">An analyzer accepts hello text inputs given tooit by hello parser, processes hello text, and then sends back tokenized terms toobe incorporated into hello query tree.</span></span> 

<span data-ttu-id="0eed1-191">Nejběžnější formulář Hello lexikální analýzy *lingvistické analysis* který transformuje vyhledávacích dotazů podle pravidla konkrétní tooa zadané jazyka:</span><span class="sxs-lookup"><span data-stu-id="0eed1-191">hello most common form of lexical analysis is *linguistic analysis* which transforms query terms based on rules specific tooa given language:</span></span> 

* <span data-ttu-id="0eed1-192">Snižuje formuláře dotazu termín toohello kořenové slova</span><span class="sxs-lookup"><span data-stu-id="0eed1-192">Reducing a query term toohello root form of a word</span></span> 
* <span data-ttu-id="0eed1-193">Odebrání nepotřebných slova (stopslova, jako je například "na" nebo "a" v angličtině)</span><span class="sxs-lookup"><span data-stu-id="0eed1-193">Removing non-essential words (stopwords, such as "the" or "and" in English)</span></span> 
* <span data-ttu-id="0eed1-194">Rozdělení složené slovo do součásti</span><span class="sxs-lookup"><span data-stu-id="0eed1-194">Breaking a composite word into component parts</span></span> 
* <span data-ttu-id="0eed1-195">Nižší velká slova velká a malá písmena</span><span class="sxs-lookup"><span data-stu-id="0eed1-195">Lower casing an upper case word</span></span> 

<span data-ttu-id="0eed1-196">Všechny tyto operace jsou obvykle tooerase rozdíly mezi zadávání textu hello poskytované hello uživatele a hello podmínky uložené v indexu hello.</span><span class="sxs-lookup"><span data-stu-id="0eed1-196">All of these operations tend tooerase differences between hello text input provided by hello user and hello terms stored in hello index.</span></span> <span data-ttu-id="0eed1-197">Tyto operace nad rámec zpracování textu a vyžaduje zevrubnou znalost jazyka hello sám sebe.</span><span class="sxs-lookup"><span data-stu-id="0eed1-197">Such operations go beyond text processing and require in-depth knowledge of hello language itself.</span></span> <span data-ttu-id="0eed1-198">tooadd podporuje tuto vrstvu lingvistické sledování, Azure vyhledávání dlouhý seznam [analyzátory jazyka](https://docs.microsoft.com/rest/api/searchservice/language-support) Lucene a společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0eed1-198">tooadd this layer of linguistic awareness, Azure Search supports a long list of [language analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support) from both Lucene and Microsoft.</span></span>

> [!Note]
> <span data-ttu-id="0eed1-199">Požadavky na Analysis rozsahu minimální tooelaborate v závislosti na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="0eed1-199">Analysis requirements can range from minimal tooelaborate depending on your scenario.</span></span> <span data-ttu-id="0eed1-200">Můžete řídit složitost lexikální analýzy hello výběrem jedné z analyzátorů hello předdefinované nebo vytvoření vlastních [vlastní analyzátor](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="0eed1-200">You can control complexity of lexical analysis by hello selecting one of hello predefined analyzers or by creating your own [custom analyzer](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search).</span></span> <span data-ttu-id="0eed1-201">Analyzátory nejsou vymezená toosearchable pole a jsou zadané jako součást definice pole.</span><span class="sxs-lookup"><span data-stu-id="0eed1-201">Analyzers are scoped toosearchable fields and are specified as part of a field definition.</span></span> <span data-ttu-id="0eed1-202">To vám umožní toovary lexikální analýzy na základě za pole.</span><span class="sxs-lookup"><span data-stu-id="0eed1-202">This allows you toovary lexical analysis on a per-field basis.</span></span> <span data-ttu-id="0eed1-203">Tento parametr nezadáte, hello *standardní* Lucene analyzer je použít.</span><span class="sxs-lookup"><span data-stu-id="0eed1-203">Unspecified, hello *standard* Lucene analyzer is used.</span></span>

<span data-ttu-id="0eed1-204">V našem příkladu předchozí tooanalysis hello počátečního dotazu stromu má hello termín "Spacious" s velká písmena "S" a čárku hello analyzátor dotazu interpretuje jako součást termín dotazu hello (čárkou nepovažuje operátor jazyk dotazu).</span><span class="sxs-lookup"><span data-stu-id="0eed1-204">In our example, prior tooanalysis, hello initial query tree has hello term "Spacious," with an uppercase "S" and a comma that hello query parser interprets as a part of hello query term (a comma is not considered a query language operator).</span></span>  

<span data-ttu-id="0eed1-205">Když analyzátor výchozí hello zpracovává hello termín, bude malá písmena "zobrazení oceánu" a "velké" a odeberte hello čárku.</span><span class="sxs-lookup"><span data-stu-id="0eed1-205">When hello default analyzer processes hello term, it will lowercase "ocean view" and "spacious", and remove hello comma character.</span></span> <span data-ttu-id="0eed1-206">Hello upravený dotaz stromu bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="0eed1-206">hello modified query tree will look as follows:</span></span> 

 ![Logická hodnota dotaz s analyzovaných podmínky][4]

### <a name="testing-analyzer-behaviors"></a><span data-ttu-id="0eed1-208">Testování Analyzátor chování</span><span class="sxs-lookup"><span data-stu-id="0eed1-208">Testing analyzer behaviors</span></span> 

<span data-ttu-id="0eed1-209">chování Hello analyzátor lze otestovat pomocí hello [analyzovat rozhraní API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer).</span><span class="sxs-lookup"><span data-stu-id="0eed1-209">hello behavior of an analyzer can be tested using hello [Analyze API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer).</span></span> <span data-ttu-id="0eed1-210">Zadejte text hello, který budete chtít tooanalyze toosee jaké podmínky zadané analyzátor vygeneruje.</span><span class="sxs-lookup"><span data-stu-id="0eed1-210">Provide hello text you want tooanalyze toosee what terms given analyzer will generate.</span></span> <span data-ttu-id="0eed1-211">Například toosee, jak by zpracovat standardní analyzátor hello hello text "air-condition", můžete vydat hello následující požadavek:</span><span class="sxs-lookup"><span data-stu-id="0eed1-211">For example, toosee how hello standard analyzer would process hello text "air-condition", you can issue hello following request:</span></span>

~~~~
{ 
    "text": "air-condition",
    "analyzer": "standard"
}
~~~~

<span data-ttu-id="0eed1-212">Standardní analyzátor Hello dělí do následujících dvou tokeny, zadávání poznámek o nich s atributy, jako je počáteční a koncové posunutí (používá se pro přístupů zvýraznění), jakož i jejich polohu (používá se pro párování frázi) hello vstupního textu hello:</span><span class="sxs-lookup"><span data-stu-id="0eed1-212">hello standard analyzer breaks hello input text into hello following two tokens, annotating them with attributes like start and end offsets (used for hit highlighting) as well as their position (used for phrase matching):</span></span>

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

### <a name="exceptions-toolexical-analysis"></a><span data-ttu-id="0eed1-213">Analýza toolexical výjimky</span><span class="sxs-lookup"><span data-stu-id="0eed1-213">Exceptions toolexical analysis</span></span> 

<span data-ttu-id="0eed1-214">Lexikální analýzy se vztahuje pouze tooquery typy, které vyžadují úplný podmínky – dotazu termín nebo frázi dotazu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-214">Lexical analysis applies only tooquery types that require complete terms – either a term query or a phrase query.</span></span> <span data-ttu-id="0eed1-215">Netýkají tooquery typů s neúplné podmínky – předpona dotazu, dotaz zástupný znak, regulární výraz dotazu – nebo tooa přibližné dotazu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-215">It doesn’t apply tooquery types with incomplete terms – prefix query, wildcard query, regex query – or tooa fuzzy query.</span></span> <span data-ttu-id="0eed1-216">Ty typy, včetně hello předponu dotazu s termín dotazů *air-condition\**  v našem příkladu se přidají přímo stromu dotazu toohello obcházení hello analysis fáze.</span><span class="sxs-lookup"><span data-stu-id="0eed1-216">Those query types, including hello prefix query with term *air-condition\** in our example, are added directly toohello query tree, bypassing hello analysis stage.</span></span> <span data-ttu-id="0eed1-217">Hello pouze transformaci u vyhledávacích dotazů těchto typů provést, je předpoklady.</span><span class="sxs-lookup"><span data-stu-id="0eed1-217">hello only transformation performed on query terms of those types is lowercasing.</span></span>

<a name="stage3"></a>
## <a name="stage-3-document-retrieval"></a><span data-ttu-id="0eed1-218">Fáze 3: Načtení dokumentu</span><span class="sxs-lookup"><span data-stu-id="0eed1-218">Stage 3: Document retrieval</span></span> 

<span data-ttu-id="0eed1-219">Načtení dokumentu odkazuje toofinding dokumentů v indexu hello s odpovídajícími položkami.</span><span class="sxs-lookup"><span data-stu-id="0eed1-219">Document retrieval refers toofinding documents with matching terms in hello index.</span></span> <span data-ttu-id="0eed1-220">Tato fáze odhalíte nejlépe v příkladu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-220">This stage is understood best through an example.</span></span> <span data-ttu-id="0eed1-221">Začněme s indexem hotels s hello následující jednoduché schématu:</span><span class="sxs-lookup"><span data-stu-id="0eed1-221">Let's start with a hotels index having hello following simple schema:</span></span> 

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

<span data-ttu-id="0eed1-222">Další Předpokládejme, že tento index obsahuje následující čtyři dokumenty hello:</span><span class="sxs-lookup"><span data-stu-id="0eed1-222">Further assume that this index contains hello following four documents:</span></span> 

~~~~
{ 
    "value": [
        {         
            "id": "1",         
            "title": "Hotel Atman",         
            "description": "Spacious rooms, ocean view, walking distance toohello beach."   
        },       
        {         
            "id": "2",         
            "title": "Beach Resort",        
            "description": "Located on hello north shore of hello island of Kauaʻi. Ocean view."     
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

<span data-ttu-id="0eed1-223">**Jak jsou indexované podmínky**</span><span class="sxs-lookup"><span data-stu-id="0eed1-223">**How terms are indexed**</span></span>

<span data-ttu-id="0eed1-224">načtení toounderstand pomáhá tooknow několik základní informace o indexování.</span><span class="sxs-lookup"><span data-stu-id="0eed1-224">toounderstand retrieval, it helps tooknow a few basics about indexing.</span></span> <span data-ttu-id="0eed1-225">jednotka Hello úložiště je obráceným index, jeden pro každé prohledávatelné pole.</span><span class="sxs-lookup"><span data-stu-id="0eed1-225">hello unit of storage is an inverted index, one for each searchable field.</span></span> <span data-ttu-id="0eed1-226">V rámci obráceným index je seřazený seznam všechny podmínky ze všech dokumentů.</span><span class="sxs-lookup"><span data-stu-id="0eed1-226">Within an inverted index is a sorted list of all terms from all documents.</span></span> <span data-ttu-id="0eed1-227">Každému termínu mapuje toohello seznam dokumentů, v nichž se vyskytuje, zřejmá jako v následujícím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="0eed1-227">Each term maps toohello list of documents in which it occurs, as evident in hello example below.</span></span>

<span data-ttu-id="0eed1-228">tooproduce hello podmínky v indexu obráceným hello vyhledávacího webu provede lexikální analýzu přes hello obsah dokumentů, podobně jako toowhat probíhá při zpracování dotazu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-228">tooproduce hello terms in an inverted index, hello search engine performs lexical analysis over hello content of documents, similar toowhat happens during query processing.</span></span> <span data-ttu-id="0eed1-229">Vstupní textové hodnoty jsou předávány tooan analyzátor, používají formát nižší, odstraní interpunkční znaménka, a tak dále, v závislosti na konfiguraci analyzátor hello.</span><span class="sxs-lookup"><span data-stu-id="0eed1-229">Text inputs are passed tooan analyzer, lower-cased, stripped of punctuation, and so forth, depending on hello analyzer configuration.</span></span> <span data-ttu-id="0eed1-230">Je běžné, ale nejsou vyžadovány, toouse hello stejné analyzátory pro vyhledávání a indexování operations tak, aby vypadal jako podmínky uvnitř hello index další podmínky dotazu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-230">It's common, but not required, toouse hello same analyzers for search and indexing operations so that query terms look more like terms inside hello index.</span></span>

> [!Note]
> <span data-ttu-id="0eed1-231">Služba Azure Search umožňuje určit různé analyzátory pro indexování a hledání prostřednictvím další `indexAnalyzer` a `searchAnalyzer` pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="0eed1-231">Azure Search lets you specify different analyzers for indexing and search via additional `indexAnalyzer` and `searchAnalyzer` field parameters.</span></span> <span data-ttu-id="0eed1-232">Pokud tento parametr nezadáte, hello analyzátor s hello `analyzer` vlastnost se používá pro indexování a vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="0eed1-232">If unspecified, hello analyzer set with hello `analyzer` property is used for both indexing and searching.</span></span>  

<span data-ttu-id="0eed1-233">**Převedený index pro příklad dokumenty**</span><span class="sxs-lookup"><span data-stu-id="0eed1-233">**Inverted index for example documents**</span></span>

<span data-ttu-id="0eed1-234">Vrácení tooour třeba pro hello **název** pole, index hello obrácený vypadat třeba takto:</span><span class="sxs-lookup"><span data-stu-id="0eed1-234">Returning tooour example, for hello **title** field, hello inverted index looks like this:</span></span>

| <span data-ttu-id="0eed1-235">Označení</span><span class="sxs-lookup"><span data-stu-id="0eed1-235">Term</span></span> | <span data-ttu-id="0eed1-236">Seznam dokumentů</span><span class="sxs-lookup"><span data-stu-id="0eed1-236">Document list</span></span> |
|------|---------------|
| <span data-ttu-id="0eed1-237">atman</span><span class="sxs-lookup"><span data-stu-id="0eed1-237">atman</span></span> | <span data-ttu-id="0eed1-238">1</span><span class="sxs-lookup"><span data-stu-id="0eed1-238">1</span></span> |
| <span data-ttu-id="0eed1-239">plážový</span><span class="sxs-lookup"><span data-stu-id="0eed1-239">beach</span></span> | <span data-ttu-id="0eed1-240">2</span><span class="sxs-lookup"><span data-stu-id="0eed1-240">2</span></span> |
| <span data-ttu-id="0eed1-241">hotelů</span><span class="sxs-lookup"><span data-stu-id="0eed1-241">hotel</span></span> | <span data-ttu-id="0eed1-242">1, 3</span><span class="sxs-lookup"><span data-stu-id="0eed1-242">1, 3</span></span> |
| <span data-ttu-id="0eed1-243">oceánu</span><span class="sxs-lookup"><span data-stu-id="0eed1-243">ocean</span></span> | <span data-ttu-id="0eed1-244">4</span><span class="sxs-lookup"><span data-stu-id="0eed1-244">4</span></span>  |
| <span data-ttu-id="0eed1-245">playa</span><span class="sxs-lookup"><span data-stu-id="0eed1-245">playa</span></span> | <span data-ttu-id="0eed1-246">3</span><span class="sxs-lookup"><span data-stu-id="0eed1-246">3</span></span> |
| <span data-ttu-id="0eed1-247">možnost</span><span class="sxs-lookup"><span data-stu-id="0eed1-247">resort</span></span> | <span data-ttu-id="0eed1-248">3</span><span class="sxs-lookup"><span data-stu-id="0eed1-248">3</span></span> |
| <span data-ttu-id="0eed1-249">Retreat</span><span class="sxs-lookup"><span data-stu-id="0eed1-249">retreat</span></span> | <span data-ttu-id="0eed1-250">4</span><span class="sxs-lookup"><span data-stu-id="0eed1-250">4</span></span> |

<span data-ttu-id="0eed1-251">V hello pole název, pouze *hotelů* se zobrazí v dva dokumenty: 1, 3.</span><span class="sxs-lookup"><span data-stu-id="0eed1-251">In hello title field, only *hotel* shows up in two documents: 1, 3.</span></span>

<span data-ttu-id="0eed1-252">Pro hello **popis** pole hello index je následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0eed1-252">For hello **description** field, hello index is as follows:</span></span>

| <span data-ttu-id="0eed1-253">Označení</span><span class="sxs-lookup"><span data-stu-id="0eed1-253">Term</span></span> | <span data-ttu-id="0eed1-254">Seznam dokumentů</span><span class="sxs-lookup"><span data-stu-id="0eed1-254">Document list</span></span> |
|------|---------------|
| <span data-ttu-id="0eed1-255">letecké</span><span class="sxs-lookup"><span data-stu-id="0eed1-255">air</span></span> | <span data-ttu-id="0eed1-256">3</span><span class="sxs-lookup"><span data-stu-id="0eed1-256">3</span></span>
| <span data-ttu-id="0eed1-257">a</span><span class="sxs-lookup"><span data-stu-id="0eed1-257">and</span></span> | <span data-ttu-id="0eed1-258">4</span><span class="sxs-lookup"><span data-stu-id="0eed1-258">4</span></span>
| <span data-ttu-id="0eed1-259">plážový</span><span class="sxs-lookup"><span data-stu-id="0eed1-259">beach</span></span> | <span data-ttu-id="0eed1-260">1</span><span class="sxs-lookup"><span data-stu-id="0eed1-260">1</span></span>
| <span data-ttu-id="0eed1-261">záleží</span><span class="sxs-lookup"><span data-stu-id="0eed1-261">conditioned</span></span> | <span data-ttu-id="0eed1-262">3</span><span class="sxs-lookup"><span data-stu-id="0eed1-262">3</span></span>
| <span data-ttu-id="0eed1-263">možnost</span><span class="sxs-lookup"><span data-stu-id="0eed1-263">comfortable</span></span> | <span data-ttu-id="0eed1-264">3</span><span class="sxs-lookup"><span data-stu-id="0eed1-264">3</span></span>
| <span data-ttu-id="0eed1-265">Vzdálenost</span><span class="sxs-lookup"><span data-stu-id="0eed1-265">distance</span></span> | <span data-ttu-id="0eed1-266">1</span><span class="sxs-lookup"><span data-stu-id="0eed1-266">1</span></span>
| <span data-ttu-id="0eed1-267">Island</span><span class="sxs-lookup"><span data-stu-id="0eed1-267">island</span></span> | <span data-ttu-id="0eed1-268">2</span><span class="sxs-lookup"><span data-stu-id="0eed1-268">2</span></span>
| <span data-ttu-id="0eed1-269">kauaʻi</span><span class="sxs-lookup"><span data-stu-id="0eed1-269">kauaʻi</span></span> | <span data-ttu-id="0eed1-270">2</span><span class="sxs-lookup"><span data-stu-id="0eed1-270">2</span></span>
| <span data-ttu-id="0eed1-271">nachází</span><span class="sxs-lookup"><span data-stu-id="0eed1-271">located</span></span> | <span data-ttu-id="0eed1-272">2</span><span class="sxs-lookup"><span data-stu-id="0eed1-272">2</span></span>
| <span data-ttu-id="0eed1-273">– sever</span><span class="sxs-lookup"><span data-stu-id="0eed1-273">north</span></span> | <span data-ttu-id="0eed1-274">2</span><span class="sxs-lookup"><span data-stu-id="0eed1-274">2</span></span>
| <span data-ttu-id="0eed1-275">oceánu</span><span class="sxs-lookup"><span data-stu-id="0eed1-275">ocean</span></span> | <span data-ttu-id="0eed1-276">1, 2, 3</span><span class="sxs-lookup"><span data-stu-id="0eed1-276">1, 2, 3</span></span>
| <span data-ttu-id="0eed1-277">z</span><span class="sxs-lookup"><span data-stu-id="0eed1-277">of</span></span> | <span data-ttu-id="0eed1-278">2</span><span class="sxs-lookup"><span data-stu-id="0eed1-278">2</span></span>
| <span data-ttu-id="0eed1-279">na</span><span class="sxs-lookup"><span data-stu-id="0eed1-279">on</span></span> |<span data-ttu-id="0eed1-280">2</span><span class="sxs-lookup"><span data-stu-id="0eed1-280">2</span></span>
| <span data-ttu-id="0eed1-281">quiet</span><span class="sxs-lookup"><span data-stu-id="0eed1-281">quiet</span></span> | <span data-ttu-id="0eed1-282">4</span><span class="sxs-lookup"><span data-stu-id="0eed1-282">4</span></span>
| <span data-ttu-id="0eed1-283">místnosti</span><span class="sxs-lookup"><span data-stu-id="0eed1-283">rooms</span></span>  | <span data-ttu-id="0eed1-284">1, 3</span><span class="sxs-lookup"><span data-stu-id="0eed1-284">1, 3</span></span>
| <span data-ttu-id="0eed1-285">secluded</span><span class="sxs-lookup"><span data-stu-id="0eed1-285">secluded</span></span> | <span data-ttu-id="0eed1-286">4</span><span class="sxs-lookup"><span data-stu-id="0eed1-286">4</span></span>
| <span data-ttu-id="0eed1-287">pobřeží</span><span class="sxs-lookup"><span data-stu-id="0eed1-287">shore</span></span> | <span data-ttu-id="0eed1-288">2</span><span class="sxs-lookup"><span data-stu-id="0eed1-288">2</span></span>
| <span data-ttu-id="0eed1-289">Velké</span><span class="sxs-lookup"><span data-stu-id="0eed1-289">spacious</span></span> | <span data-ttu-id="0eed1-290">1</span><span class="sxs-lookup"><span data-stu-id="0eed1-290">1</span></span>
| <span data-ttu-id="0eed1-291">Dobrý den</span><span class="sxs-lookup"><span data-stu-id="0eed1-291">hello</span></span> | <span data-ttu-id="0eed1-292">1, 2</span><span class="sxs-lookup"><span data-stu-id="0eed1-292">1, 2</span></span>
| <span data-ttu-id="0eed1-293">příliš</span><span class="sxs-lookup"><span data-stu-id="0eed1-293">too</span></span>| <span data-ttu-id="0eed1-294">1</span><span class="sxs-lookup"><span data-stu-id="0eed1-294">1</span></span>
| <span data-ttu-id="0eed1-295">zobrazit</span><span class="sxs-lookup"><span data-stu-id="0eed1-295">view</span></span> | <span data-ttu-id="0eed1-296">1, 2, 3</span><span class="sxs-lookup"><span data-stu-id="0eed1-296">1, 2, 3</span></span>
| <span data-ttu-id="0eed1-297">procházení</span><span class="sxs-lookup"><span data-stu-id="0eed1-297">walking</span></span> | <span data-ttu-id="0eed1-298">1</span><span class="sxs-lookup"><span data-stu-id="0eed1-298">1</span></span>
| <span data-ttu-id="0eed1-299">S</span><span class="sxs-lookup"><span data-stu-id="0eed1-299">with</span></span> | <span data-ttu-id="0eed1-300">3</span><span class="sxs-lookup"><span data-stu-id="0eed1-300">3</span></span>


<span data-ttu-id="0eed1-301">**Odpovídající vyhledávacích dotazů vůči indexované podmínky**</span><span class="sxs-lookup"><span data-stu-id="0eed1-301">**Matching query terms against indexed terms**</span></span>

<span data-ttu-id="0eed1-302">Indexy hello obrácený výše, umožňuje vrátit toohello ukázkový dotaz a v tématu Jak odpovídající dokumenty pro náš příklad dotazu nebyly nalezeny.</span><span class="sxs-lookup"><span data-stu-id="0eed1-302">Given hello inverted indices above, let’s return toohello sample query and see how matching documents are found for our example query.</span></span> <span data-ttu-id="0eed1-303">Odvolat že stromové struktuře poslední dotaz hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="0eed1-303">Recall that hello final query tree looks like this:</span></span> 

 ![Logická hodnota dotaz s analyzovaných podmínky][4]

<span data-ttu-id="0eed1-305">Během provádění dotazu jednotlivé dotazy jsou u spustit prohledatelná pole hello nezávisle.</span><span class="sxs-lookup"><span data-stu-id="0eed1-305">During query execution, individual queries are executed against hello searchable fields independently.</span></span> 

+ <span data-ttu-id="0eed1-306">TermQuery Vítejte "velké", odpovídá dokumentu 1 (hotelů Atman).</span><span class="sxs-lookup"><span data-stu-id="0eed1-306">hello TermQuery, "spacious", matches document 1 (Hotel Atman).</span></span> 

+ <span data-ttu-id="0eed1-307">Hello PrefixQuery, "air-condition *", se neshoduje se žádné dokumenty.</span><span class="sxs-lookup"><span data-stu-id="0eed1-307">hello PrefixQuery, "air-condition*", doesn't match any documents.</span></span> 

  <span data-ttu-id="0eed1-308">Toto je chování, které někdy confuses vývojáři.</span><span class="sxs-lookup"><span data-stu-id="0eed1-308">This is a behavior that sometimes confuses developers.</span></span> <span data-ttu-id="0eed1-309">I když hello termín klimatizovaným existuje v dokumentu hello, je rozdělený do dvou podmínky podle hello výchozí analyzátor.</span><span class="sxs-lookup"><span data-stu-id="0eed1-309">Although hello term air-conditioned exists in hello document, it is split into two terms by hello default analyzer.</span></span> <span data-ttu-id="0eed1-310">Odvolat, že předpona dotazy, které obsahují částečnou podmínky, nejsou analýza.</span><span class="sxs-lookup"><span data-stu-id="0eed1-310">Recall that prefix queries, which contain partial terms, are not analyzed.</span></span> <span data-ttu-id="0eed1-311">Proto podmínky s předponu "air-condition" jsou vyhledávány v hello obrácený index a nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="0eed1-311">Therefore terms with prefix "air-condition" are looked up in hello inverted index and not found.</span></span>

+ <span data-ttu-id="0eed1-312">PhraseQuery Hello "oceánu zobrazení", vyhledá podmínky hello "oceánu" a "Zobrazit" a zkontroluje hello blízko podmínkami hello původního dokumentu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-312">hello PhraseQuery, "ocean view", looks up hello terms "ocean" and "view" and checks hello proximity of terms in hello original document.</span></span> <span data-ttu-id="0eed1-313">Dokumenty, 1, 2 a 3 odpovídat tento dotaz do pole Popis hello.</span><span class="sxs-lookup"><span data-stu-id="0eed1-313">Documents 1, 2 and 3 match this query in hello description field.</span></span> <span data-ttu-id="0eed1-314">Všimněte si dokument 4 má hello oceánu termín v hlavě hello ale není považovány za shodné, jako jsme to potřebné pro frázi "zobrazení oceánu" hello, nikoli jednotlivých slov.</span><span class="sxs-lookup"><span data-stu-id="0eed1-314">Notice document 4 has hello term ocean in hello title but isn’t considered a match, as we're looking for hello "ocean view" phrase rather than individual words.</span></span> 

> [!Note]
> <span data-ttu-id="0eed1-315">Vyhledávací dotaz je spustit nezávisle pro všechna prohledatelná pole v indexu Azure Search, není-li omezit hello pole s hello hello `searchFields` parametr, jak je ukázáno v požadavku hello Příklad hledání.</span><span class="sxs-lookup"><span data-stu-id="0eed1-315">A search query is executed independently against all searchable fields in hello Azure Search index unless you limit hello fields set with hello `searchFields` parameter, as illustrated in hello example search request.</span></span> <span data-ttu-id="0eed1-316">Dokumenty, které odpovídají v některém z hello vybrané pole jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="0eed1-316">Documents that match in any of hello selected fields are returned.</span></span> 

<span data-ttu-id="0eed1-317">Hello dokumenty, které odpovídají na hello celou pro dotaz hello Nejistá, jsou 1, 2, 3.</span><span class="sxs-lookup"><span data-stu-id="0eed1-317">On hello whole, for hello query in question, hello documents that match are 1, 2, 3.</span></span> 

## <a name="stage-4-scoring"></a><span data-ttu-id="0eed1-318">Fáze 4: vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="0eed1-318">Stage 4: Scoring</span></span>  

<span data-ttu-id="0eed1-319">Každému dokumentu v sadě výsledků hledání je přiřazen relevance skóre.</span><span class="sxs-lookup"><span data-stu-id="0eed1-319">Every document in a search result set is assigned a relevance score.</span></span> <span data-ttu-id="0eed1-320">Funkce Hello skóre relevance hello je toorank vyšší tyto dokumenty, odpovězte osvědčených uživatel otázka jako vyjádřená hello vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="0eed1-320">hello function of hello relevance score is toorank higher those documents that best answer a user question as expressed by hello search query.</span></span> <span data-ttu-id="0eed1-321">výpočet skóre Hello je založen na statistické vlastnosti podmínky, které odpovídá.</span><span class="sxs-lookup"><span data-stu-id="0eed1-321">hello score is computed based on statistical properties of terms that matched.</span></span> <span data-ttu-id="0eed1-322">V hello je základní hello vyhodnocování vzorec [TF/IDF (termín frekvence inverzní dokumentu frekvenci)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).</span><span class="sxs-lookup"><span data-stu-id="0eed1-322">At hello core of hello scoring formula is [TF/IDF (term frequency-inverse document frequency)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).</span></span> <span data-ttu-id="0eed1-323">V dotazech obsahující výjimečná a běžné podmínky TF/IDF zvýší úroveň výsledky obsahující hello výjimečných podmínek.</span><span class="sxs-lookup"><span data-stu-id="0eed1-323">In queries containing rare and common terms, TF/IDF promotes results containing hello rare term.</span></span> <span data-ttu-id="0eed1-324">Například v hypotetický index s všechny články Wikipedia z dokumentů takový dotaz odpovídající hello *hello ředitel*, dokumentů, které vyhovují na *ředitel* jsou považovány za relevantní více než dokumenty na **.</span><span class="sxs-lookup"><span data-stu-id="0eed1-324">For example, in a hypothetical index with all Wikipedia articles, from documents that matched hello query *hello president*, documents matching on *president* are considered more relevant than documents matching on *the*.</span></span>


### <a name="scoring-example"></a><span data-ttu-id="0eed1-325">Příklad vyhodnocování</span><span class="sxs-lookup"><span data-stu-id="0eed1-325">Scoring example</span></span>

<span data-ttu-id="0eed1-326">Odvolat hello tři dokumenty, které odpovídá náš příklad dotazu:</span><span class="sxs-lookup"><span data-stu-id="0eed1-326">Recall hello three documents that matched our example query:</span></span>
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
      "description": "Spacious rooms, ocean view, walking distance toohello beach."
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
      "description": "Located on a cliff on hello north shore of hello island of Kauai. Ocean view."
    }
  ]
}
~~~~

<span data-ttu-id="0eed1-327">Nejlépe dokumentu 1 odpovídající hello dotazu, protože obě hello termín *velké* a požadované frázi hello *oceánu zobrazení* dojít do pole Popis hello.</span><span class="sxs-lookup"><span data-stu-id="0eed1-327">Document 1 matched hello query best because both hello term *spacious* and hello required phrase *ocean view* occur in hello description field.</span></span> <span data-ttu-id="0eed1-328">Následující dva dokumenty Hello odpovídat jenom hello frázi *oceánu zobrazení*.</span><span class="sxs-lookup"><span data-stu-id="0eed1-328">hello next two documents match only hello phrase *ocean view*.</span></span> <span data-ttu-id="0eed1-329">Ho může být překvapení této hello relevance skóre pro dokument 2 a 3 je jiné, i když se shodoval hello dotaz hello stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="0eed1-329">It might be surprising that hello relevance score for document 2 and 3 is different even though they matched hello query in hello same way.</span></span> <span data-ttu-id="0eed1-330">Je to proto hello vyhodnocování vzorec obsahuje více součástí než právě TF/IDF.</span><span class="sxs-lookup"><span data-stu-id="0eed1-330">It's because hello scoring formula has more components than just TF/IDF.</span></span> <span data-ttu-id="0eed1-331">V takovém případě dokumentu 3 byl přiřazen mírně vyšší skóre, protože jeho popis je kratší.</span><span class="sxs-lookup"><span data-stu-id="0eed1-331">In this case, document 3 was assigned a slightly higher score because its description is shorter.</span></span> <span data-ttu-id="0eed1-332">Další informace o [Lucene je praktické vyhodnocování vzorec](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) toounderstand, jak můžete ovlivnit délka pole a dalších faktorů hello relevance skóre.</span><span class="sxs-lookup"><span data-stu-id="0eed1-332">Learn about [Lucene's Practical Scoring Formula](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) toounderstand how field length and other factors can influence hello relevance score.</span></span>

<span data-ttu-id="0eed1-333">Některé typy (zástupný znak, předpony, regulární výraz) dotazů vždy přispívat konstantní skóre dokumentů toohello celkové skóre.</span><span class="sxs-lookup"><span data-stu-id="0eed1-333">Some query types (wildcard, prefix, regex) always contribute a constant score toohello overall document score.</span></span> <span data-ttu-id="0eed1-334">To umožňuje shodami prostřednictvím toobe rozšíření dotazu zahrnuté ve výsledcích hello, ale bez ovlivnění hello hodnocení.</span><span class="sxs-lookup"><span data-stu-id="0eed1-334">This allows matches found through query expansion toobe included in hello results, but without affecting hello ranking.</span></span> 

<span data-ttu-id="0eed1-335">Příklad ukazuje, proč to záleží.</span><span class="sxs-lookup"><span data-stu-id="0eed1-335">An example illustrates why this matters.</span></span> <span data-ttu-id="0eed1-336">Vyhledávání pomocí zástupných znaků, včetně předpony hledání, jsou nejednoznačné podle definice, protože vstup hello je částečné řetězce s potenciální odpovídá na velký počet různorodých podmínky (zvažte vstup "prohlídka *", s odpovídá na "kurzy", "tourettes" nalezena, a " tourmaline").</span><span class="sxs-lookup"><span data-stu-id="0eed1-336">Wildcard searches, including prefix searches, are ambiguous by definition because hello input is a partial string with potential matches on a very large number of disparate terms (consider an input of "tour*", with matches found on “tours”, “tourettes”, and “tourmaline”).</span></span> <span data-ttu-id="0eed1-337">Zadané hello povahu těchto výsledků, neexistuje žádný způsob odvození tooreasonably podmínky, které jsou vyšší hodnotu než jiné.</span><span class="sxs-lookup"><span data-stu-id="0eed1-337">Given hello nature of these results, there is no way tooreasonably infer which terms are more valuable than others.</span></span> <span data-ttu-id="0eed1-338">Z tohoto důvodu jsme ignorovat termín frekvence, při vyhodnocování výsledky v dotazech typy zástupný znak, předpony a regulární výraz.</span><span class="sxs-lookup"><span data-stu-id="0eed1-338">For this reason, we ignore term frequencies when scoring results in queries of types wildcard, prefix and regex.</span></span> <span data-ttu-id="0eed1-339">V žádosti o více částech hledání obsahující částečným i úplným podmínky, jsou součástí výsledků z částečné vstup hello s konstantou, stanovení skóre tooavoid odchylka směrem potenciálně neočekávané odpovídá.</span><span class="sxs-lookup"><span data-stu-id="0eed1-339">In a multi-part search request that includes partial and complete terms, results from hello partial input are incorporated with a constant score tooavoid bias towards potentially unexpected matches.</span></span>

### <a name="score-tuning"></a><span data-ttu-id="0eed1-340">Skóre ladění</span><span class="sxs-lookup"><span data-stu-id="0eed1-340">Score tuning</span></span>

<span data-ttu-id="0eed1-341">Existují dva způsoby tootune relevance skóre ve službě Azure Search:</span><span class="sxs-lookup"><span data-stu-id="0eed1-341">There are two ways tootune relevance scores in Azure Search:</span></span>

1. <span data-ttu-id="0eed1-342">**Vyhodnocování profily** povýšit dokumenty v hello seřazeny seznam výsledků na základě sady pravidel.</span><span class="sxs-lookup"><span data-stu-id="0eed1-342">**Scoring profiles** promote documents in hello ranked list of results based on a set of rules.</span></span> <span data-ttu-id="0eed1-343">V našem příkladu jsme zvažte dokumenty, které odpovídá v poli Název hello relevantnější než dokumenty, které odpovídá v poli Popis hello.</span><span class="sxs-lookup"><span data-stu-id="0eed1-343">In our example, we could consider documents that matched in hello title field more relevant than documents that matched in hello description field.</span></span> <span data-ttu-id="0eed1-344">Kromě toho Pokud index měli pole ceny pro každý hotelů, jsme může zvýšit úroveň dokumenty s nižší cenou.</span><span class="sxs-lookup"><span data-stu-id="0eed1-344">Additionally, if our index had a price field for each hotel, we could promote documents with lower price.</span></span> <span data-ttu-id="0eed1-345">Další informace jak příliš[přidat index vyhledávání tooa profily vyhodnocování.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)</span><span class="sxs-lookup"><span data-stu-id="0eed1-345">Learn more how too[add Scoring Profiles tooa search index.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)</span></span>
2. <span data-ttu-id="0eed1-346">**Zvýšení skóre termínu** (k dispozici pouze v syntaxe dotazů Lucene úplné hello) poskytuje zvýšení skóre operátor `^` , může být použita tooany části stromu hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-346">**Term boosting** (available only in hello Full Lucene query syntax) provides a boosting operator `^` that can be applied tooany part of hello query tree.</span></span> <span data-ttu-id="0eed1-347">V našem příkladu, namísto hledání na předponě hello *air-condition*\*, jeden může vyhledat přesnou termín buď hello *air-condition* nebo hello předponu, ale dokumenty, které odpovídají na hello přesný termín, jsou seřazeny vyšší použitím nárůst toohello termín dotazu: *letecké podmínku ^ 2 || AIR-condition**.</span><span class="sxs-lookup"><span data-stu-id="0eed1-347">In our example, instead of searching on hello prefix *air-condition*\*, one could search for either hello exact term *air-condition* or hello prefix, but documents that match on hello exact term are ranked higher by applying boost toohello term query: *air-condition^2||air-condition**.</span></span> <span data-ttu-id="0eed1-348">Další informace o [zvýšení skóre termínu](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).</span><span class="sxs-lookup"><span data-stu-id="0eed1-348">Learn more about [term boosting](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).</span></span>


### <a name="scoring-in-a-distributed-index"></a><span data-ttu-id="0eed1-349">Vyhodnocování v distribuované indexu</span><span class="sxs-lookup"><span data-stu-id="0eed1-349">Scoring in a distributed index</span></span>

<span data-ttu-id="0eed1-350">Všechny indexy ve službě Azure Search jsou automaticky rozdělí na několik horizontálních oddílů, což nám tooquickly distribuovat hello index mezi několika uzly při škálování služby registrace nebo snížit.</span><span class="sxs-lookup"><span data-stu-id="0eed1-350">All indexes in Azure Search are automatically split into multiple shards, allowing us tooquickly distribute hello index among multiple nodes during service scale up or scale down.</span></span> <span data-ttu-id="0eed1-351">Když je vydaný žádost o vyhledávání, jeho vydání před každou horizontálního oddílu nezávisle.</span><span class="sxs-lookup"><span data-stu-id="0eed1-351">When a search request is issued, it’s issued against each shard independently.</span></span> <span data-ttu-id="0eed1-352">Hello výsledky z každé horizontálního oddílu jsou pak sloučit a seřazené podle skóre (Pokud není definováno žádné jiné pořadí).</span><span class="sxs-lookup"><span data-stu-id="0eed1-352">hello results from each shard are then merged and ordered by score (if no other ordering is defined).</span></span> <span data-ttu-id="0eed1-353">Je důležité tooknow, který není mezi všechny horizontálních oddílů hello vyhodnocování funkce váhu dotazu termín frekvence před jeho frekvence inverzní dokumentu v všechny dokumenty v rámci hello horizontálních!</span><span class="sxs-lookup"><span data-stu-id="0eed1-353">It is important tooknow that hello scoring function weights query term frequency against its inverse document frequency in all documents within hello shard, not across all shards!</span></span>

<span data-ttu-id="0eed1-354">To znamená, relevance skóre *může* být různé pro identické dokumenty, pokud budou umístěny v různých horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="0eed1-354">This means a relevance score *could* be different for identical documents if they reside on different shards.</span></span> <span data-ttu-id="0eed1-355">Tyto rozdíly naštěstí zpravidla toodisappear růstem hello počet dokumentů v indexu hello kvůli toomore i termín distribuce.</span><span class="sxs-lookup"><span data-stu-id="0eed1-355">Fortunately, such differences tend toodisappear as hello number of documents in hello index grows due toomore even term distribution.</span></span> <span data-ttu-id="0eed1-356">Není možné tooassume, na které horizontálního oddílu se umístí všechny daného dokumentu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-356">It’s not possible tooassume on which shard any given document will be placed.</span></span> <span data-ttu-id="0eed1-357">Ale za předpokladu, že klíč dokumentu nemění, bude vždy přiřazena toohello stejné ID horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-357">However, assuming a document key doesn't change, it will always be assigned toohello same shard.</span></span>

<span data-ttu-id="0eed1-358">Obecně platí skóre dokumentů není hello nejlepší atribut pro řazení dokumenty, pokud stabilitu pořadí je důležité.</span><span class="sxs-lookup"><span data-stu-id="0eed1-358">In general, document score is not hello best attribute for ordering documents if order stability is important.</span></span> <span data-ttu-id="0eed1-359">Například zadány dvě dokument s identické skóre, není zaručeno, která jako první se objeví v při dalším spuštění hello stejný dotaz.</span><span class="sxs-lookup"><span data-stu-id="0eed1-359">For example, given two document with an identical score, there is no guarantee which one appears first in subsequent runs of hello same query.</span></span> <span data-ttu-id="0eed1-360">Dokument skóre pouze získat obecný přehled o dokument relevance relativní tooother dokumenty v sadě výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="0eed1-360">Document score should only give a general sense of document relevance relative tooother documents in hello results set.</span></span>

## <a name="conclusion"></a><span data-ttu-id="0eed1-361">Závěr</span><span class="sxs-lookup"><span data-stu-id="0eed1-361">Conclusion</span></span>

<span data-ttu-id="0eed1-362">Úspěch Hello internet vyhledávací weby vyvolalo očekávání pro fulltextové vyhledávání přes osobní data.</span><span class="sxs-lookup"><span data-stu-id="0eed1-362">hello success of internet search engines has raised expectations for full text search over private data.</span></span> <span data-ttu-id="0eed1-363">Pro téměř k libovolnému druhu vyhledávání nyní Očekáváme, že modul toounderstand hello naše záměr, i když jsou nesprávně zadaných podmínek nebo jsou neúplné.</span><span class="sxs-lookup"><span data-stu-id="0eed1-363">For almost any kind of search experience, we now expect hello engine toounderstand our intent, even when terms are misspelled or incomplete.</span></span> <span data-ttu-id="0eed1-364">Může i Očekáváme, že shody na základě téměř ekvivalentní termíny nebo synonyma, které jsme zadali ve skutečnosti.</span><span class="sxs-lookup"><span data-stu-id="0eed1-364">We might even expect matches based on near equivalent terms or synonyms that we never actually specified.</span></span>

<span data-ttu-id="0eed1-365">Z hlediska technické fulltextové vyhledávání je vysoce komplexní, vyžadují pokročilé analýzy lingvistické a systematicky tooprocessing způsoby, které generovat, rozbalte a transformace dotazu podmínky toodeliver výsledku relevantní.</span><span class="sxs-lookup"><span data-stu-id="0eed1-365">From a technical standpoint, full text search is highly complex, requiring sophisticated linguistic analysis and a systematic approach tooprocessing in ways that distill, expand, and transform query terms toodeliver a relevant result.</span></span> <span data-ttu-id="0eed1-366">Zadané hello vyplývajících složité kroky, je celá řada faktorů, které můžou ovlivnit hello výsledek dotazu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-366">Given hello inherent complexities, there are a lot of factors that can affect hello outcome of a query.</span></span> <span data-ttu-id="0eed1-367">Z tohoto důvodu Investujete hello čas toounderstand hello mechanismů fulltextové vyhledávání nabízí konkrétní výhody, při pokusu o toowork prostřednictvím neočekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="0eed1-367">For this reason, investing hello time toounderstand hello mechanics of full text search offers tangible benefits when trying toowork through unexpected results.</span></span>  

<span data-ttu-id="0eed1-368">Tento článek prozkoumali fulltextového vyhledávání v kontextu hello Azure Search.</span><span class="sxs-lookup"><span data-stu-id="0eed1-368">This article explored full text search in hello context of Azure Search.</span></span> <span data-ttu-id="0eed1-369">Věříme, že nabízí dostatečná pozadí toorecognize možné příčiny a řešení pro řešení běžných problémů s dotazu.</span><span class="sxs-lookup"><span data-stu-id="0eed1-369">We hope it gives you sufficient background toorecognize potential causes and resolutions for addressing common query problems.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0eed1-370">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0eed1-370">Next steps</span></span>

+ <span data-ttu-id="0eed1-371">Sestavení indexu ukázkový text hello, vyzkoušejte různé dotazy a zkontrolovat výsledky.</span><span class="sxs-lookup"><span data-stu-id="0eed1-371">Build hello sample index, try out different queries and review results.</span></span> <span data-ttu-id="0eed1-372">Pokyny najdete v tématu [sestavení a dotazování indexu portálu hello](search-get-started-portal.md#query-index).</span><span class="sxs-lookup"><span data-stu-id="0eed1-372">For instructions, see [Build and query an index in hello portal](search-get-started-portal.md#query-index).</span></span>

+ <span data-ttu-id="0eed1-373">Zkuste syntaxe další dotaz z hello [vyhledávání dokumentů](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) příklad části nebo z [jednoduchá syntaxe dotazů](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) v Průzkumník služby Search na portálu hello.</span><span class="sxs-lookup"><span data-stu-id="0eed1-373">Try additional query syntax from hello [Search Documents](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) example section or from [Simple query syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) in Search explorer in hello portal.</span></span>

+ <span data-ttu-id="0eed1-374">Zkontrolujte [vyhodnocování profily](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) Pokud chcete, aby tootune hodnocení ve vyhledávací aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0eed1-374">Review [scoring profiles](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) if you want tootune ranking in your search application.</span></span>

+ <span data-ttu-id="0eed1-375">Zjistěte, jak tooapply [lexikální analyzátory jazyka](https://docs.microsoft.com/rest/api/searchservice/language-support).</span><span class="sxs-lookup"><span data-stu-id="0eed1-375">Learn how tooapply [language-specific lexical analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support).</span></span>

+ <span data-ttu-id="0eed1-376">[Konfigurace vlastní analyzátorů](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) pro minimální zpracování nebo speciální zpracování na konkrétních polí.</span><span class="sxs-lookup"><span data-stu-id="0eed1-376">[Configure custom analyzers](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) for either minimal processing or specialized processing on specific fields.</span></span>

+ <span data-ttu-id="0eed1-377">[Porovnání standardní a anglické analyzátorů](http://alice.unearth.ai/))-souběžného na tento ukázkový web.</span><span class="sxs-lookup"><span data-stu-id="0eed1-377">[Compare standard and English analyzers](http://alice.unearth.ai/)) side-by-side on this demo web site.</span></span> 

## <a name="see-also"></a><span data-ttu-id="0eed1-378">Viz také</span><span class="sxs-lookup"><span data-stu-id="0eed1-378">See also</span></span>

[<span data-ttu-id="0eed1-379">Hledání dokumentů rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="0eed1-379">Search Documents REST API</span></span>](https://docs.microsoft.com/rest/api/searchservice/search-documents)

[<span data-ttu-id="0eed1-380">Jednoduchá syntaxe dotazů</span><span class="sxs-lookup"><span data-stu-id="0eed1-380">Simple query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)

[<span data-ttu-id="0eed1-381">Úplná syntaxe dotazů Lucene</span><span class="sxs-lookup"><span data-stu-id="0eed1-381">Full Lucene query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

[<span data-ttu-id="0eed1-382">Zpracování výsledků vyhledávání</span><span class="sxs-lookup"><span data-stu-id="0eed1-382">Handle search results</span></span>](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
[2]: ./media/search-lucene-query-architecture/azSearch-queryparsing-should2.png
[3]: ./media/search-lucene-query-architecture/azSearch-queryparsing-must2.png
[4]: ./media/search-lucene-query-architecture/azSearch-queryparsing-spacious2.png
