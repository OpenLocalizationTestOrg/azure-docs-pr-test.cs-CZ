---
pageTitle: Synonyms in Azure Search (preview) | Microsoft Docs
description: "Předběžná dokumentace pro funkci hello synonyma (preview), v hello REST API služby Azure Search."
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
ms.openlocfilehash: 2695139d2b298fa2e7c1814715fdf96729f594ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="synonyms-in-azure-search-preview"></a><span data-ttu-id="ed6b1-102">Synonyma ve službě Azure Search (preview)</span><span class="sxs-lookup"><span data-stu-id="ed6b1-102">Synonyms in Azure Search (preview)</span></span>

<span data-ttu-id="ed6b1-103">Synonyma na vyhledávacích webech přidružit ekvivalentní podmínky, které implicitně rozbalte hello obor dotazu, bez nutnosti tooactually zadejte termín hello uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-103">Synonyms in search engines associate equivalent terms that implicitly expand hello scope of a query, without hello user having tooactually provide hello term.</span></span> <span data-ttu-id="ed6b1-104">Například dané hello termín "pes" a přidružení synonymum "PSA" a "štěněte", všechny dokumenty obsahující "pes", "PSA" nebo "štěněte" bude spadat do rozsahu hello hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-104">For example, given hello term "dog" and synonym associations of "canine" and "puppy", any documents containing "dog", "canine" or "puppy" will fall within hello scope of hello query.</span></span>

<span data-ttu-id="ed6b1-105">Ve službě Azure Search synonymum rozšíření se provádí v době dotazu.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-105">In Azure Search, synonym expansion is done at query time.</span></span> <span data-ttu-id="ed6b1-106">Můžete přidat synonymum mapy tooa služby s žádné přerušení tooexisting operace.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-106">You can add synonym maps tooa service with no disruption tooexisting operations.</span></span> <span data-ttu-id="ed6b1-107">Můžete přidat **synonymMaps** definici pole vlastnosti tooa bez nutnosti toorebuild hello index.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-107">You can add a  **synonymMaps** property tooa field definition without having toorebuild hello index.</span></span> <span data-ttu-id="ed6b1-108">Další informace najdete v tématu [aktualizace indexu](https://docs.microsoft.com/rest/api/searchservice/update-index).</span><span class="sxs-lookup"><span data-stu-id="ed6b1-108">For more information, see [Update Index](https://docs.microsoft.com/rest/api/searchservice/update-index).</span></span>

## <a name="feature-availability"></a><span data-ttu-id="ed6b1-109">Dostupnost funkcí</span><span class="sxs-lookup"><span data-stu-id="ed6b1-109">Feature availability</span></span>

<span data-ttu-id="ed6b1-110">Hello synonyma funkce je aktuálně ve verzi preview a podporována v pouze hello nejnovější verzi preview rozhraní api-version (verze api-version = 2016-09-01-Preview).</span><span class="sxs-lookup"><span data-stu-id="ed6b1-110">hello synonyms feature is currently in preview and only supported in hello latest preview api-version (api-version=2016-09-01-Preview).</span></span> <span data-ttu-id="ed6b1-111">Podpora webu Azure Portal se v současnosti neposkytuje.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-111">There is no Azure portal support at this time.</span></span> <span data-ttu-id="ed6b1-112">Protože hello rozhraní API verze je zadán u hello požadavku, je možné toocombine všeobecně dostupná (GA) a preview rozhraní API v hello stejná aplikace.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-112">Because hello API version is specified on hello request, it's possible toocombine generally available (GA) and preview APIs in hello same app.</span></span> <span data-ttu-id="ed6b1-113">Však může změnit preview, které nejsou v rámci smlouvy o úrovni služeb a funkcí rozhraní API, tak nedoporučujeme jejich používání v produkční aplikace.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-113">However, preview APIs are not under SLA and features may change, so we do not recommend using them in production applications.</span></span>

## <a name="how-toouse-synonyms-in-azure-search"></a><span data-ttu-id="ed6b1-114">Jak vyhledávat synonyma toouse v Azure</span><span class="sxs-lookup"><span data-stu-id="ed6b1-114">How toouse synonyms in Azure search</span></span>

<span data-ttu-id="ed6b1-115">Ve službě Azure Search podpory synonymum vychází synonymum mapy definovat a nahrát tooyour služby.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-115">In Azure Search, synonym support is based on synonym maps that you define and upload tooyour service.</span></span> <span data-ttu-id="ed6b1-116">Tyto mapy tvoří nezávislý prostředek (např. indexy nebo zdroje dat) a mohou být využívána žádné prohledávatelné pole v jakékoli indexu ve vyhledávací službě.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-116">These maps constitute an independent resource (like indexes or data sources), and can be used by any searchable field in any index in your search service.</span></span>

<span data-ttu-id="ed6b1-117">Mapuje synonymum a indexy jsou zachována nezávisle.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-117">Synonym maps and indexes are maintained independently.</span></span> <span data-ttu-id="ed6b1-118">Po definování mapu synonymum a nahrajte ho tooyour služby, můžete povolit funkci synonymum hello na pole tak, že přidáte novou vlastnost s názvem **synonymMaps** v definici pole hello.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-118">Once you define a synonym map and upload it tooyour service, you can enable hello synonym feature on a field by adding a new property called **synonymMaps** in hello field definition.</span></span> <span data-ttu-id="ed6b1-119">Vytvoření, aktualizace a odstranění mapu synonymum, je vždy celé dokumentu operaci, což znamená, že se nedají vytvořit, aktualizovat nebo odstranit části hello synonymum mapy postupně.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-119">Creating, updating, and deleting a synonym map is always a whole-document operation, meaning that you cannot create, update or delete parts of hello synonym map incrementally.</span></span> <span data-ttu-id="ed6b1-120">Aktualizace i jeden záznam nevyžaduje znovu načíst.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-120">Updating even a single entry requires a reload.</span></span>

<span data-ttu-id="ed6b1-121">Začlenění synonyma do vyhledávací aplikaci je dvoustupňový proces:</span><span class="sxs-lookup"><span data-stu-id="ed6b1-121">Incorporating synonyms into your search application is a two-step process:</span></span>

1.  <span data-ttu-id="ed6b1-122">Přidáte službu vyhledávání synonymum mapy tooyour prostřednictvím hello rozhraní API níže.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-122">Add a synonym map tooyour search service through hello APIs below.</span></span>  

2.  <span data-ttu-id="ed6b1-123">Konfigurovat mapu synonymum hello toouse prohledávatelné pole v definici indexu hello.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-123">Configure a searchable field toouse hello synonym map in hello index definition.</span></span>

### <a name="synonymmaps-resource-apis"></a><span data-ttu-id="ed6b1-124">Rozhraní API SynonymMaps prostředků</span><span class="sxs-lookup"><span data-stu-id="ed6b1-124">SynonymMaps Resource APIs</span></span>

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a><span data-ttu-id="ed6b1-125">Přidat nebo aktualizovat mapu synonymum v rámci služby, pomocí POST nebo BLOKOVAT.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-125">Add or update a synonym map under your service, using POST or PUT.</span></span>

<span data-ttu-id="ed6b1-126">Synonymum maps se odešlou toohello service pomocí Metoda POST nebo PUT.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-126">Synonym maps are uploaded toohello service via POST or PUT.</span></span> <span data-ttu-id="ed6b1-127">Každé pravidlo musí být odděleny hello znak nového řádku (\n).</span><span class="sxs-lookup"><span data-stu-id="ed6b1-127">Each rule must be delimited by hello new line character ('\n').</span></span> <span data-ttu-id="ed6b1-128">Můžete definovat too5, 000 pravidel na synonymum mapy ve bezplatné služby a 10 000 pravidel ve všech dalších skladových položek.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-128">You can define up too5,000 rules per synonym map in a free service and 10,000 rules in all other SKUs.</span></span> <span data-ttu-id="ed6b1-129">Každé pravidlo může obsahovat maximálně too20 rozšíření.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-129">Each rule can have up too20 expansions.</span></span>

<span data-ttu-id="ed6b1-130">V této verzi preview musí být synonymum mapy ve formátu hello Apache Solr, který je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-130">In this preview, synonym maps must be in hello Apache Solr format which is explained below.</span></span> <span data-ttu-id="ed6b1-131">Pokud máte existující slovník synonymum v jiném formátu a chcete toouse ji přímo, dejte nám vědět, na [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span><span class="sxs-lookup"><span data-stu-id="ed6b1-131">If you have an existing synonym dictionary in a different format and want toouse it directly, please let us know on [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span></span>

<span data-ttu-id="ed6b1-132">Můžete vytvořit nové mapování synonymum pomocí HTTP POST, stejně jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="ed6b1-132">You can create a new synonym map using HTTP POST, as in hello following example:</span></span>

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

<span data-ttu-id="ed6b1-133">Alternativně můžete použít PUT a zadejte název mapy synonymum hello na hello identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-133">Alternatively, you can use PUT and specify hello synonym map name on hello URI.</span></span> <span data-ttu-id="ed6b1-134">Pokud hello synonymum mapy neexistuje, bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-134">If hello synonym map does not exist, it will be created.</span></span>

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a><span data-ttu-id="ed6b1-135">Apache Solr synonymum formátu</span><span class="sxs-lookup"><span data-stu-id="ed6b1-135">Apache Solr synonym format</span></span>

<span data-ttu-id="ed6b1-136">Formát Solr Hello podporuje ekvivalentní a explicitní synonymum mapování.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-136">hello Solr format supports equivalent and explicit synonym mappings.</span></span> <span data-ttu-id="ed6b1-137">Pravidla mapování splňovat toohello s otevřeným zdrojem synonymum filtru specifikaci Apache Solr, popsané v tomto dokumentu: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter).</span><span class="sxs-lookup"><span data-stu-id="ed6b1-137">Mapping rules adhere toohello open source synonym filter specification of Apache Solr, described in this document: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter).</span></span> <span data-ttu-id="ed6b1-138">Zde je ukázka pravidla pro ekvivalentní synonyma.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-138">Below is a sample rule for equivalent synonyms.</span></span>
```
              USA, United States, United States of America
```

<span data-ttu-id="ed6b1-139">Pomocí výše uvedené pravidlo hello, vyhledávací dotaz "USA" bude rozšiřovat příliš "USA" nebo "USA" nebo "USA".</span><span class="sxs-lookup"><span data-stu-id="ed6b1-139">With hello rule above, a search query "USA" will expand too"USA" OR "United States" OR "United States of America".</span></span>

<span data-ttu-id="ed6b1-140">Šipka je označený jako explicitní mapování "= >".</span><span class="sxs-lookup"><span data-stu-id="ed6b1-140">Explicit mapping is denoted by an arrow "=>".</span></span> <span data-ttu-id="ed6b1-141">-Li zadána, termín posloupnost vyhledávací dotaz, který odpovídá hello levé straně "= >" bude nahrazena adresou hello alternativy na pravé straně hello.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-141">When specified, a term sequence of a search query that matches hello left hand side of "=>" will be replaced with hello alternatives on hello right hand side.</span></span> <span data-ttu-id="ed6b1-142">Zadané pravidlo hello níže, vyhledávací dotazy "Washington", "Wash."</span><span class="sxs-lookup"><span data-stu-id="ed6b1-142">Given hello rule below, search queries "Washington", "Wash."</span></span> <span data-ttu-id="ed6b1-143">nebo "WA" budou všechny možné přepsaná příliš "WA".</span><span class="sxs-lookup"><span data-stu-id="ed6b1-143">or "WA" will all be rewritten too"WA".</span></span> <span data-ttu-id="ed6b1-144">Explicitní mapování platí v určeném směru hello a není přepisování příliš hello dotazu "WA" "Washington" v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-144">Explicit mapping only applies in hello direction specified and does not rewrite hello query "WA" too"Washington" in this case.</span></span>
```
              Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a><span data-ttu-id="ed6b1-145">Seznam synonymum mapuje v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-145">List synonym maps under your service.</span></span>

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a><span data-ttu-id="ed6b1-146">Načíst synonymum mapu v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-146">Get a synonym map under your service.</span></span>

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a><span data-ttu-id="ed6b1-147">Odstranění mapy synonyma v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-147">Delete a synonyms map under your service.</span></span>

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

### <a name="configure-a-searchable-field-toouse-hello-synonym-map-in-hello-index-definition"></a><span data-ttu-id="ed6b1-148">Konfigurovat mapu synonymum hello toouse prohledávatelné pole v definici indexu hello.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-148">Configure a searchable field toouse hello synonym map in hello index definition.</span></span>

<span data-ttu-id="ed6b1-149">Nové vlastnosti pole **synonymMaps** lze použít toospecify toouse synonymum mapy pro prohledávatelné pole.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-149">A new field property **synonymMaps** can be used toospecify a synonym map toouse for a searchable field.</span></span> <span data-ttu-id="ed6b1-150">Synonymum mapy prostředků úrovně služby a lze odkazovat pomocí libovolného pole index v rámci služby hello.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-150">Synonym maps are service level resources and can be referenced by any field of an index under hello service.</span></span>

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

<span data-ttu-id="ed6b1-151">**synonymMaps** lze zadat pro prohledatelná pole typu hello 'Edm.String' nebo 'Collection(Edm.String)'.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-151">**synonymMaps** can be specified for searchable fields of hello type 'Edm.String' or 'Collection(Edm.String)'.</span></span>

> [!NOTE]
> <span data-ttu-id="ed6b1-152">V této verzi preview může mít pouze jeden synonymum mapovat na pole.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-152">In this preview, you can only have one synonym map per field.</span></span> <span data-ttu-id="ed6b1-153">Pokud chcete toouse více mapami synonymum, dejte nám vědět, na [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span><span class="sxs-lookup"><span data-stu-id="ed6b1-153">If you want toouse multiple synonym maps, please let us know on [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span></span>

## <a name="impact-of-synonyms-on-other-search-features"></a><span data-ttu-id="ed6b1-154">Dopad synonyma na jiných vyhledávacích funkcí</span><span class="sxs-lookup"><span data-stu-id="ed6b1-154">Impact of synonyms on other search features</span></span>

<span data-ttu-id="ed6b1-155">Funkce synonyma Hello přepíše původní dotaz hello s synonyma s hello operátor OR.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-155">hello synonyms feature rewrites hello original query with synonyms with hello OR operator.</span></span> <span data-ttu-id="ed6b1-156">Z tohoto důvodu přístupů zvýrazňování a vyhodnocování profily zachází s původní termín hello a synonyma jako ekvivalentní.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-156">For this reason, hit highlighting and scoring profiles treat hello original term and synonyms as equivalent.</span></span>

<span data-ttu-id="ed6b1-157">Funkce synonymum platí toosearch dotazy a nepoužije toofilters nebo omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-157">Synonym feature applies toosearch queries and does not apply toofilters or facets.</span></span> <span data-ttu-id="ed6b1-158">Podobně návrhy založen pouze na původní termín hello; synonymum shoduje se nezobrazí v odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-158">Similarly, suggestions are based only on hello original term; synonym matches do not appear in hello response.</span></span>

<span data-ttu-id="ed6b1-159">Synonymum rozšíření se nevztahují toowildcard hledaných termínů; Předpona, přibližné a podmínky regex nejsou rozšířit.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-159">Synonym expansions do not apply toowildcard search terms; prefix, fuzzy, and regex terms aren't expanded.</span></span>

## <a name="tips-for-building-a-synonym-map"></a><span data-ttu-id="ed6b1-160">Tipy pro sestavování synonymum mapy</span><span class="sxs-lookup"><span data-stu-id="ed6b1-160">Tips for building a synonym map</span></span>

- <span data-ttu-id="ed6b1-161">Mapu stručný a dobře navrženou synonymum je efektivnější než vyčerpávající seznam možných shod.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-161">A concise, well-designed synonym map is more efficient than an exhaustive list of possible matches.</span></span> <span data-ttu-id="ed6b1-162">Nadměrně velké nebo složité slovník trvat déle tooparse a ovlivnit latence dotazu hello, pokud dotaz hello rozšíří toomany synonyma.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-162">Excessively large or complex dictionaries take longer tooparse and affect hello query latency if hello query expands toomany synonyms.</span></span> <span data-ttu-id="ed6b1-163">Místo odhad, při které by mohly používat podmínky, můžete získat skutečný podmínky hello prostřednictvím [hledání sestava analýzy provozu](search-traffic-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="ed6b1-163">Rather than guess at which terms might be used, you can get hello actual terms via a [search traffic analysis report](search-traffic-analytics.md).</span></span>

- <span data-ttu-id="ed6b1-164">Jako předběžná verze i ověřování cvičení, povolit a pak bude pomocí této sestavy tooprecisely určit, jaké podmínky těžit z synonymum shoda a pak pokračujte toouse jej jako ověření, že je mapu synonymum generovala lepší výsledek.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-164">As both a preliminary and validation exercise, enable and then use this report tooprecisely determine which terms will benefit from a synonym match, and then continue toouse it as validation that your synonym map is producing a better outcome.</span></span> <span data-ttu-id="ed6b1-165">V sestavě hello předdefinované hello dlaždice "Nejčastější dotazy vyhledávání" a "nula výsledek vyhledávací dotazy" získáte hello potřebné informace.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-165">In hello predefined report, hello tiles "Most common search queries" and "Zero-result search queries" will give you hello necessary information.</span></span>

- <span data-ttu-id="ed6b1-166">Můžete vytvořit více mapami synonymum pro vyhledávací aplikaci (například podle jazyka, pokud vaše aplikace podporuje základní vícejazyčnou zákazníka).</span><span class="sxs-lookup"><span data-stu-id="ed6b1-166">You can create multiple synonym maps for your search application (for example, by language if your application supports a multi-lingual customer base).</span></span> <span data-ttu-id="ed6b1-167">V současné době pole lze použít pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-167">Currently, a field can only use one of them.</span></span> <span data-ttu-id="ed6b1-168">Vlastnosti synonymMaps můžete kdykoli aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-168">You can update a field's synonymMaps property at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed6b1-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed6b1-169">Next Steps</span></span>

- <span data-ttu-id="ed6b1-170">Pokud máte existující index ve vývojovém prostředí (mimo produkční), Experimentujte s toosee malé slovník jak hello přidání synonyma změní hello vyhledávání prostředí, včetně dopad na vyhodnocování profily, počtu zvýraznění a návrhy.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-170">If you have an existing index in a development (non-production) environment, experiment with a small dictionary toosee how hello addition of synonyms changes hello search experience, including impact on scoring profiles, hit highlighting, and suggestions.</span></span>

- <span data-ttu-id="ed6b1-171">[Povolit Analýza provozu vyhledávání](search-traffic-analytics.md) a hello pomocí předdefinované sestavy Power BI toolearn podmínky, které se používají většinu a které těch, které jsou návratový hello nulové dokumenty.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-171">[Enable search traffic analytics](search-traffic-analytics.md) and use hello predefined Power BI report toolearn which terms are used hello most, and which ones return zero documents.</span></span> <span data-ttu-id="ed6b1-172">Díky tyto přehledy, zkontrolovat, jestli hello slovník tooinclude synonyma pro neproduktivní dotazy, které by měl být řešení toodocuments v indexu.</span><span class="sxs-lookup"><span data-stu-id="ed6b1-172">Armed with these insights, revise hello dictionary tooinclude synonyms for unproductive queries that should be resolving toodocuments in your index.</span></span>
