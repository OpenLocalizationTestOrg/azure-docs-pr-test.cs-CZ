---
pageTitle: Synonyms in Azure Search (preview) | Microsoft Docs
description: "Předběžná dokumentace pro funkci synonyma (preview), který je v rozhraní API REST služby Azure Search."
services: search
documentationCenter: 
authors: mhko
manager: pablocas
editor: 
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/07/2016
ms.author: nateko
ms.openlocfilehash: 739a0ad77c68ea74ec25bc80c7539ac8b3f18201
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="synonyms-in-azure-search-preview"></a><span data-ttu-id="1b10a-102">Synonyma ve službě Azure Search (preview)</span><span class="sxs-lookup"><span data-stu-id="1b10a-102">Synonyms in Azure Search (preview)</span></span>

<span data-ttu-id="1b10a-103">Synonyma na vyhledávacích webech přidružit ekvivalentní podmínky, které implicitně zvětšit rozsah dotazu, aniž by uživatel musel ve skutečnosti zadejte termín.</span><span class="sxs-lookup"><span data-stu-id="1b10a-103">Synonyms in search engines associate equivalent terms that implicitly expand the scope of a query, without the user having to actually provide the term.</span></span> <span data-ttu-id="1b10a-104">Například zadány termín "pes" a synonymum přidružení "canine" a "štěněte", všechny dokumenty obsahující "pes", "PSA" nebo "štěněte" bude spadat do rozsahu dotazu.</span><span class="sxs-lookup"><span data-stu-id="1b10a-104">For example, given the term "dog" and synonym associations of "canine" and "puppy", any documents containing "dog", "canine" or "puppy" will fall within the scope of the query.</span></span>

<span data-ttu-id="1b10a-105">Ve službě Azure Search synonymum rozšíření se provádí v době dotazu.</span><span class="sxs-lookup"><span data-stu-id="1b10a-105">In Azure Search, synonym expansion is done at query time.</span></span> <span data-ttu-id="1b10a-106">Synonymum maps můžete přidat na služby s bez přerušení na existující operace.</span><span class="sxs-lookup"><span data-stu-id="1b10a-106">You can add synonym maps to a service with no disruption to existing operations.</span></span> <span data-ttu-id="1b10a-107">Můžete přidat **synonymMaps** vlastnost do definice pole bez nutnosti znovu sestavte index.</span><span class="sxs-lookup"><span data-stu-id="1b10a-107">You can add a  **synonymMaps** property to a field definition without having to rebuild the index.</span></span> <span data-ttu-id="1b10a-108">Další informace najdete v tématu [aktualizace indexu](https://docs.microsoft.com/rest/api/searchservice/update-index).</span><span class="sxs-lookup"><span data-stu-id="1b10a-108">For more information, see [Update Index](https://docs.microsoft.com/rest/api/searchservice/update-index).</span></span>

## <a name="feature-availability"></a><span data-ttu-id="1b10a-109">Dostupnost funkcí</span><span class="sxs-lookup"><span data-stu-id="1b10a-109">Feature availability</span></span>

<span data-ttu-id="1b10a-110">Funkci synonyma je aktuálně ve verzi preview a podporuje jenom v nejnovější verzi preview rozhraní api-version (verze api-version = 2016-09-01-Preview).</span><span class="sxs-lookup"><span data-stu-id="1b10a-110">The synonyms feature is currently in preview and only supported in the latest preview api-version (api-version=2016-09-01-Preview).</span></span> <span data-ttu-id="1b10a-111">Podpora webu Azure Portal se v současnosti neposkytuje.</span><span class="sxs-lookup"><span data-stu-id="1b10a-111">There is no Azure portal support at this time.</span></span> <span data-ttu-id="1b10a-112">Verze rozhraní API je určen na žádost, je možné kombinovat všeobecně dostupná (GA) a zobrazit jejich náhled rozhraní API ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1b10a-112">Because the API version is specified on the request, it's possible to combine generally available (GA) and preview APIs in the same app.</span></span> <span data-ttu-id="1b10a-113">Však může změnit preview, které nejsou v rámci smlouvy o úrovni služeb a funkcí rozhraní API, tak nedoporučujeme jejich používání v produkční aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b10a-113">However, preview APIs are not under SLA and features may change, so we do not recommend using them in production applications.</span></span>

## <a name="how-to-use-synonyms-in-azure-search"></a><span data-ttu-id="1b10a-114">Jak používat synonyma ve službě Azure search</span><span class="sxs-lookup"><span data-stu-id="1b10a-114">How to use synonyms in Azure search</span></span>

<span data-ttu-id="1b10a-115">Ve službě Azure Search vychází synonymum podpory synonymum mapy, které můžete definovat a odeslat do služby.</span><span class="sxs-lookup"><span data-stu-id="1b10a-115">In Azure Search, synonym support is based on synonym maps that you define and upload to your service.</span></span> <span data-ttu-id="1b10a-116">Tyto mapy tvoří nezávislý prostředek (např. indexy nebo zdroje dat) a mohou být využívána žádné prohledávatelné pole v jakékoli indexu ve vyhledávací službě.</span><span class="sxs-lookup"><span data-stu-id="1b10a-116">These maps constitute an independent resource (like indexes or data sources), and can be used by any searchable field in any index in your search service.</span></span>

<span data-ttu-id="1b10a-117">Mapuje synonymum a indexy jsou zachována nezávisle.</span><span class="sxs-lookup"><span data-stu-id="1b10a-117">Synonym maps and indexes are maintained independently.</span></span> <span data-ttu-id="1b10a-118">Po definování mapu synonymum a nahrajte ho do vaší služby, můžete povolit funkci synonymum na pole tak, že přidáte novou vlastnost s názvem **synonymMaps** v definici pole.</span><span class="sxs-lookup"><span data-stu-id="1b10a-118">Once you define a synonym map and upload it to your service, you can enable the synonym feature on a field by adding a new property called **synonymMaps** in the field definition.</span></span> <span data-ttu-id="1b10a-119">Vytvoření, aktualizace a odstranění mapu synonymum, je vždy celé dokumentu operaci, což znamená, že nelze vytvořit, aktualizaci nebo odstranění částí mapy synonymum postupně.</span><span class="sxs-lookup"><span data-stu-id="1b10a-119">Creating, updating, and deleting a synonym map is always a whole-document operation, meaning that you cannot create, update or delete parts of the synonym map incrementally.</span></span> <span data-ttu-id="1b10a-120">Aktualizace i jeden záznam nevyžaduje znovu načíst.</span><span class="sxs-lookup"><span data-stu-id="1b10a-120">Updating even a single entry requires a reload.</span></span>

<span data-ttu-id="1b10a-121">Začlenění synonyma do vyhledávací aplikaci je dvoustupňový proces:</span><span class="sxs-lookup"><span data-stu-id="1b10a-121">Incorporating synonyms into your search application is a two-step process:</span></span>

1.  <span data-ttu-id="1b10a-122">Přidáte mapu synonymum do služby vyhledávání prostřednictvím rozhraní API níže.</span><span class="sxs-lookup"><span data-stu-id="1b10a-122">Add a synonym map to your search service through the APIs below.</span></span>  

2.  <span data-ttu-id="1b10a-123">Nakonfigurujte prohledávatelné pole, které chcete použít mapování synonymum v definici indexu.</span><span class="sxs-lookup"><span data-stu-id="1b10a-123">Configure a searchable field to use the synonym map in the index definition.</span></span>

### <a name="synonymmaps-resource-apis"></a><span data-ttu-id="1b10a-124">Rozhraní API SynonymMaps prostředků</span><span class="sxs-lookup"><span data-stu-id="1b10a-124">SynonymMaps Resource APIs</span></span>

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a><span data-ttu-id="1b10a-125">Přidat nebo aktualizovat mapu synonymum v rámci služby, pomocí POST nebo BLOKOVAT.</span><span class="sxs-lookup"><span data-stu-id="1b10a-125">Add or update a synonym map under your service, using POST or PUT.</span></span>

<span data-ttu-id="1b10a-126">Synonymum maps se odešlou do služby prostřednictvím POST nebo PUT.</span><span class="sxs-lookup"><span data-stu-id="1b10a-126">Synonym maps are uploaded to the service via POST or PUT.</span></span> <span data-ttu-id="1b10a-127">Každé pravidlo musí být odděleny znak nového řádku (\n).</span><span class="sxs-lookup"><span data-stu-id="1b10a-127">Each rule must be delimited by the new line character ('\n').</span></span> <span data-ttu-id="1b10a-128">Můžete definovat až 5 000 pravidel na synonymum mapy ve bezplatné služby a 10 000 pravidel ve všech dalších skladových položek.</span><span class="sxs-lookup"><span data-stu-id="1b10a-128">You can define up to 5,000 rules per synonym map in a free service and 10,000 rules in all other SKUs.</span></span> <span data-ttu-id="1b10a-129">Každé pravidlo může mít maximálně 20 rozšíření.</span><span class="sxs-lookup"><span data-stu-id="1b10a-129">Each rule can have up to 20 expansions.</span></span>

<span data-ttu-id="1b10a-130">V této verzi preview musí být synonymum mapy ve formátu Apache Solr, který je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="1b10a-130">In this preview, synonym maps must be in the Apache Solr format which is explained below.</span></span> <span data-ttu-id="1b10a-131">Pokud máte existující slovník synonymum v jiném formátu a chcete používat přímo, dejte nám vědět, na [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span><span class="sxs-lookup"><span data-stu-id="1b10a-131">If you have an existing synonym dictionary in a different format and want to use it directly, please let us know on [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span></span>

<span data-ttu-id="1b10a-132">Můžete vytvořit nové mapování synonymum pomocí HTTP POST, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1b10a-132">You can create a new synonym map using HTTP POST, as in the following example:</span></span>

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

<span data-ttu-id="1b10a-133">Alternativně můžete použít PUT a zadejte název mapy synonymum v identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="1b10a-133">Alternatively, you can use PUT and specify the synonym map name on the URI.</span></span> <span data-ttu-id="1b10a-134">Pokud mapy synonymum neexistuje, bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="1b10a-134">If the synonym map does not exist, it will be created.</span></span>

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a><span data-ttu-id="1b10a-135">Apache Solr synonymum formátu</span><span class="sxs-lookup"><span data-stu-id="1b10a-135">Apache Solr synonym format</span></span>

<span data-ttu-id="1b10a-136">Formát Solr podporuje ekvivalentní a explicitní synonymum mapování.</span><span class="sxs-lookup"><span data-stu-id="1b10a-136">The Solr format supports equivalent and explicit synonym mappings.</span></span> <span data-ttu-id="1b10a-137">Pravidla mapování splňovat specifikaci filtru synonymum s otevřeným zdrojem Apache Solr, popsané v tomto dokumentu: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter).</span><span class="sxs-lookup"><span data-stu-id="1b10a-137">Mapping rules adhere to the open source synonym filter specification of Apache Solr, described in this document: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter).</span></span> <span data-ttu-id="1b10a-138">Zde je ukázka pravidla pro ekvivalentní synonyma.</span><span class="sxs-lookup"><span data-stu-id="1b10a-138">Below is a sample rule for equivalent synonyms.</span></span>
```
              USA, United States, United States of America
```

<span data-ttu-id="1b10a-139">S tímto pravidlem výše, vyhledávací dotaz rozbalte "USA" možnost "USA" nebo "USA" nebo "USA".</span><span class="sxs-lookup"><span data-stu-id="1b10a-139">With the rule above, a search query "USA" will expand to "USA" OR "United States" OR "United States of America".</span></span>

<span data-ttu-id="1b10a-140">Šipka je označený jako explicitní mapování "= >".</span><span class="sxs-lookup"><span data-stu-id="1b10a-140">Explicit mapping is denoted by an arrow "=>".</span></span> <span data-ttu-id="1b10a-141">-Li zadána, termín posloupnost vyhledávací dotaz, který odpovídá levé straně "= >" bude nahrazena adresou alternativy na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="1b10a-141">When specified, a term sequence of a search query that matches the left hand side of "=>" will be replaced with the alternatives on the right hand side.</span></span> <span data-ttu-id="1b10a-142">Zadané pravidlo níže, vyhledávací dotazy "Washington", "Wash."</span><span class="sxs-lookup"><span data-stu-id="1b10a-142">Given the rule below, search queries "Washington", "Wash."</span></span> <span data-ttu-id="1b10a-143">nebo "WA" budou všechny být přepsána pro "WA".</span><span class="sxs-lookup"><span data-stu-id="1b10a-143">or "WA" will all be rewritten to "WA".</span></span> <span data-ttu-id="1b10a-144">Explicitní mapování pouze platí v určeném směru a není dotaz přepište "WA" na "Washington" v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="1b10a-144">Explicit mapping only applies in the direction specified and does not rewrite the query "WA" to "Washington" in this case.</span></span>
```
              Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a><span data-ttu-id="1b10a-145">Seznam synonymum mapuje v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="1b10a-145">List synonym maps under your service.</span></span>

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a><span data-ttu-id="1b10a-146">Načíst synonymum mapu v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="1b10a-146">Get a synonym map under your service.</span></span>

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a><span data-ttu-id="1b10a-147">Odstranění mapy synonyma v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="1b10a-147">Delete a synonyms map under your service.</span></span>

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

### <a name="configure-a-searchable-field-to-use-the-synonym-map-in-the-index-definition"></a><span data-ttu-id="1b10a-148">Nakonfigurujte prohledávatelné pole, které chcete použít mapování synonymum v definici indexu.</span><span class="sxs-lookup"><span data-stu-id="1b10a-148">Configure a searchable field to use the synonym map in the index definition.</span></span>

<span data-ttu-id="1b10a-149">Nové vlastnosti pole **synonymMaps** lze použít k určení synonymum mapu, která bude používat pro prohledávatelné pole.</span><span class="sxs-lookup"><span data-stu-id="1b10a-149">A new field property **synonymMaps** can be used to specify a synonym map to use for a searchable field.</span></span> <span data-ttu-id="1b10a-150">Synonymum mapy prostředků úrovně služby a lze odkazovat pomocí libovolného pole index v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="1b10a-150">Synonym maps are service level resources and can be referenced by any field of an index under the service.</span></span>

    POST https://[servicename].search.windows.net/indexes?api-version=2016-09-01-Preview
    api-key: [admin key]

    {
       "name":"myindex",
       "fields":[
          {
             "name":"id",
             "type":"Edm.String",
             "key":true
          },
          {
             "name":"name",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"en.lucene",
             "synonymMaps":[
                "mysynonymmap"
             ]
          },
          {
             "name":"name_jp",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"ja.microsoft",
             "synonymMaps":[
                "japanesesynonymmap"
             ]
          }
       ]
    }

<span data-ttu-id="1b10a-151">**synonymMaps** lze zadat pro prohledatelná pole typu 'Edm.String' nebo 'Collection(Edm.String)'.</span><span class="sxs-lookup"><span data-stu-id="1b10a-151">**synonymMaps** can be specified for searchable fields of the type 'Edm.String' or 'Collection(Edm.String)'.</span></span>

> [!NOTE]
> <span data-ttu-id="1b10a-152">V této verzi preview může mít pouze jeden synonymum mapovat na pole.</span><span class="sxs-lookup"><span data-stu-id="1b10a-152">In this preview, you can only have one synonym map per field.</span></span> <span data-ttu-id="1b10a-153">Pokud chcete použít více mapami synonymum, dejte nám vědět, na [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span><span class="sxs-lookup"><span data-stu-id="1b10a-153">If you want to use multiple synonym maps, please let us know on [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span></span>

## <a name="impact-of-synonyms-on-other-search-features"></a><span data-ttu-id="1b10a-154">Dopad synonyma na jiných vyhledávacích funkcí</span><span class="sxs-lookup"><span data-stu-id="1b10a-154">Impact of synonyms on other search features</span></span>

<span data-ttu-id="1b10a-155">Funkci synonyma přepíše původní dotaz s synonyma pomocí operátoru OR.</span><span class="sxs-lookup"><span data-stu-id="1b10a-155">The synonyms feature rewrites the original query with synonyms with the OR operator.</span></span> <span data-ttu-id="1b10a-156">Z tohoto důvodu přístupů zvýrazňování a vyhodnocování profily považovat za původní termín a synonyma ekvivalent.</span><span class="sxs-lookup"><span data-stu-id="1b10a-156">For this reason, hit highlighting and scoring profiles treat the original term and synonyms as equivalent.</span></span>

<span data-ttu-id="1b10a-157">Synonymum funkce se vztahuje na vyhledávací dotazy a nebude použitelná pro filtry, nebo omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="1b10a-157">Synonym feature applies to search queries and does not apply to filters or facets.</span></span> <span data-ttu-id="1b10a-158">Podobně návrhy založen pouze na původní termín; synonymum shoduje se nezobrazí v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1b10a-158">Similarly, suggestions are based only on the original term; synonym matches do not appear in the response.</span></span>

<span data-ttu-id="1b10a-159">Synonymum rozšíření se nevztahují na zástupný znak hledaných termínů; Předpona, přibližné a podmínky regex nejsou rozšířit.</span><span class="sxs-lookup"><span data-stu-id="1b10a-159">Synonym expansions do not apply to wildcard search terms; prefix, fuzzy, and regex terms aren't expanded.</span></span>

## <a name="tips-for-building-a-synonym-map"></a><span data-ttu-id="1b10a-160">Tipy pro sestavování synonymum mapy</span><span class="sxs-lookup"><span data-stu-id="1b10a-160">Tips for building a synonym map</span></span>

- <span data-ttu-id="1b10a-161">Mapu stručný a dobře navrženou synonymum je efektivnější než vyčerpávající seznam možných shod.</span><span class="sxs-lookup"><span data-stu-id="1b10a-161">A concise, well-designed synonym map is more efficient than an exhaustive list of possible matches.</span></span> <span data-ttu-id="1b10a-162">Nadměrně velké nebo složité slovník trvat delší dobu a analyzovat mají vliv na latenci dotazu, pokud dotaz zasahuje do mnoha synonyma.</span><span class="sxs-lookup"><span data-stu-id="1b10a-162">Excessively large or complex dictionaries take longer to parse and affect the query latency if the query expands to many synonyms.</span></span> <span data-ttu-id="1b10a-163">Místo odhad, při které by mohly používat podmínky, můžete získat skutečný podmínky prostřednictvím [hledání sestava analýzy provozu](search-traffic-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="1b10a-163">Rather than guess at which terms might be used, you can get the actual terms via a [search traffic analysis report](search-traffic-analytics.md).</span></span>

- <span data-ttu-id="1b10a-164">Jako předběžná verze i ověřování využijí, povolení a pak použít tato sestava přesněji určit podmínky, které bude využívat synonymum shody a pak ho nadále používat jako ověření, že je mapu synonymum generovala lepší výsledek.</span><span class="sxs-lookup"><span data-stu-id="1b10a-164">As both a preliminary and validation exercise, enable and then use this report to precisely determine which terms will benefit from a synonym match, and then continue to use it as validation that your synonym map is producing a better outcome.</span></span> <span data-ttu-id="1b10a-165">V předdefinované sestavě dlaždice "Nejčastější dotazy vyhledávání" a "nula výsledek vyhledávací dotazy" získáte potřebné informace.</span><span class="sxs-lookup"><span data-stu-id="1b10a-165">In the predefined report, the tiles "Most common search queries" and "Zero-result search queries" will give you the necessary information.</span></span>

- <span data-ttu-id="1b10a-166">Můžete vytvořit více mapami synonymum pro vyhledávací aplikaci (například podle jazyka, pokud vaše aplikace podporuje základní vícejazyčnou zákazníka).</span><span class="sxs-lookup"><span data-stu-id="1b10a-166">You can create multiple synonym maps for your search application (for example, by language if your application supports a multi-lingual customer base).</span></span> <span data-ttu-id="1b10a-167">V současné době pole lze použít pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="1b10a-167">Currently, a field can only use one of them.</span></span> <span data-ttu-id="1b10a-168">Vlastnosti synonymMaps můžete kdykoli aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="1b10a-168">You can update a field's synonymMaps property at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b10a-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1b10a-169">Next Steps</span></span>

- <span data-ttu-id="1b10a-170">Pokud máte existující index ve vývojovém prostředí (mimo produkční), Experimentujte s slovník malé najdete, jak přidání synonyma změny možnosti vyhledávání, včetně dopad na vyhodnocování profily, zvýrazňování a návrhy.</span><span class="sxs-lookup"><span data-stu-id="1b10a-170">If you have an existing index in a development (non-production) environment, experiment with a small dictionary to see how the addition of synonyms changes the search experience, including impact on scoring profiles, hit highlighting, and suggestions.</span></span>

- <span data-ttu-id="1b10a-171">[Povolit Analýza provozu vyhledávání](search-traffic-analytics.md) pomocí předdefinované sestavy Power BI další podmínky, které se používají na maximum a ty, které vrátí nulové dokumenty.</span><span class="sxs-lookup"><span data-stu-id="1b10a-171">[Enable search traffic analytics](search-traffic-analytics.md) and use the predefined Power BI report to learn which terms are used the most, and which ones return zero documents.</span></span> <span data-ttu-id="1b10a-172">Díky tyto přehledy, zkontrolovat, jestli slovníku zahrnout synonyma pro neproduktivní dotazy, které by měl být řešení na dokumenty v indexu.</span><span class="sxs-lookup"><span data-stu-id="1b10a-172">Armed with these insights, revise the dictionary to include synonyms for unproductive queries that should be resolving to documents in your index.</span></span>
