---
title: "Služba Azure Search REST API verze 2015-02-28-verze Preview služby | Microsoft Docs"
description: "Azure Search služby REST API verze 2015-02-28-Preview zahrnuje povolenými experimentálními funkcemi, jako je například analyzátory přirozeného jazyka a moreLikeThis hledání."
services: search
documentationcenter: na
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 3dba3bf8-9c83-42f6-82bc-04727bd11037
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/01/2017
ms.author: brjohnst
ms.openlocfilehash: e6ad5c964bfa8421be2706cb4015980e01a271b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a><span data-ttu-id="4fd4a-103">Rozhraní API REST služby vyhledávání systému Azure: Verze 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-103">Azure Search Service REST API: Version 2015-02-28-Preview</span></span>
<span data-ttu-id="4fd4a-104">Tento článek je referenční dokumentaci k nástroji pro `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-104">This article is the reference documentation for `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="4fd4a-105">Tato verze preview rozšiřuje všeobecně dostupná verzí [rozhraní api-version = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), tím, že poskytuje následující povolenými experimentálními funkcemi:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-105">This preview extends the current generally available version, [api-version=2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), by providing the following experimental features:</span></span>

* <span data-ttu-id="4fd4a-106">`moreLikeThis`parametr v dotazu [vyhledávání dokumentů](#SearchDocs) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-106">`moreLikeThis` query parameter in the [Search Documents](#SearchDocs) API.</span></span> <span data-ttu-id="4fd4a-107">Najde jiné dokumenty, které jsou relevantní pro konkrétní jiného dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-107">It finds other documents that are relevant to another specific document.</span></span>

<span data-ttu-id="4fd4a-108">Několik dalších částí `2015-02-28-Preview` REST API popsané samostatně.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-108">A few additional parts of the `2015-02-28-Preview` REST API are documented separately.</span></span> <span data-ttu-id="4fd4a-109">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-109">These include:</span></span>

* [<span data-ttu-id="4fd4a-110">Vyhodnocování profily</span><span class="sxs-lookup"><span data-stu-id="4fd4a-110">Scoring Profiles</span></span>](search-api-scoring-profiles-2015-02-28-preview.md)
* [<span data-ttu-id="4fd4a-111">Indexery</span><span class="sxs-lookup"><span data-stu-id="4fd4a-111">Indexers</span></span>](search-api-indexers-2015-02-28-preview.md)

<span data-ttu-id="4fd4a-112">Služba Azure Search je k dispozici v několika verzích.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-112">Azure Search service is available in multiple versions.</span></span> <span data-ttu-id="4fd4a-113">Naleznete [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-113">Please refer to [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details.</span></span>

## <a name="apis-in-this-document"></a><span data-ttu-id="4fd4a-114">Rozhraní API v tomto dokumentu</span><span class="sxs-lookup"><span data-stu-id="4fd4a-114">APIs in this document</span></span>
<span data-ttu-id="4fd4a-115">Rozhraní API služby Azure Search podporuje dva syntaxe adresy URL pro operace rozhraní API: jednoduchý a OData (viz [podporu pro OData (API služby Azure Search)](http://msdn.microsoft.com/library/azure/dn798932.aspx) podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-115">Azure Search service API supports two URL syntaxes for API operations: simple and OData (see [Support for OData (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) for details).</span></span> <span data-ttu-id="4fd4a-116">V následujícím seznamu jsou jednoduché syntaxe.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-116">The following list shows the simple syntax.</span></span>

[<span data-ttu-id="4fd4a-117">Vytvoření indexu</span><span class="sxs-lookup"><span data-stu-id="4fd4a-117">Create Index</span></span>](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[<span data-ttu-id="4fd4a-118">Aktualizovat Index</span><span class="sxs-lookup"><span data-stu-id="4fd4a-118">Update Index</span></span>](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="4fd4a-119">Získat Index</span><span class="sxs-lookup"><span data-stu-id="4fd4a-119">Get Index</span></span>](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="4fd4a-120">Výpis indexy</span><span class="sxs-lookup"><span data-stu-id="4fd4a-120">Listing Indexes</span></span>](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[<span data-ttu-id="4fd4a-121">Získat statistiku indexu</span><span class="sxs-lookup"><span data-stu-id="4fd4a-121">Get Index Statistics</span></span>](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[<span data-ttu-id="4fd4a-122">Analyzátor testu</span><span class="sxs-lookup"><span data-stu-id="4fd4a-122">Test Analyzer</span></span>](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[<span data-ttu-id="4fd4a-123">Odstranění indexu</span><span class="sxs-lookup"><span data-stu-id="4fd4a-123">Delete an Index</span></span>](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="4fd4a-124">Přidat, odstranit a aktualizovat Data v indexu</span><span class="sxs-lookup"><span data-stu-id="4fd4a-124">Add, Delete, and Update Data within an Index</span></span>](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[<span data-ttu-id="4fd4a-125">Vyhledávání dokumentů</span><span class="sxs-lookup"><span data-stu-id="4fd4a-125">Search Documents</span></span>](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[<span data-ttu-id="4fd4a-126">Vyhledávání dokumentů</span><span class="sxs-lookup"><span data-stu-id="4fd4a-126">Lookup Document</span></span>](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[<span data-ttu-id="4fd4a-127">Počet dokumentů</span><span class="sxs-lookup"><span data-stu-id="4fd4a-127">Count Documents</span></span>](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[<span data-ttu-id="4fd4a-128">Návrhy</span><span class="sxs-lookup"><span data-stu-id="4fd4a-128">Suggestions</span></span>](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

- - -
<a name="IndexOps"></a>

## <a name="index-operations"></a><span data-ttu-id="4fd4a-129">Operace indexu</span><span class="sxs-lookup"><span data-stu-id="4fd4a-129">Index Operations</span></span>
<span data-ttu-id="4fd4a-130">Můžete vytvořit a spravovat indexy ve službě Azure Search pomocí jednoduchých požadavků HTTP (POST, GET, PUT, DELETE) s daným indexem prostředků.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-130">You can create and manage indexes in Azure Search service via simple HTTP requests (POST, GET, PUT, DELETE) against a given index resource.</span></span> <span data-ttu-id="4fd4a-131">Pokud chcete vytvořit index, nejprve POST dokument JSON, který popisuje schéma indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-131">To create an index, you first POST a JSON document that describes the index schema.</span></span> <span data-ttu-id="4fd4a-132">Schéma definuje pole index, jejich datové typy a jejich použití (například v fulltextové vyhledávání, řazení a filtrů používání omezujících vlastností).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-132">The schema defines the fields of the index, their data types, and how they can be used (for example, in full-text searches, filters, sorting, or faceting).</span></span> <span data-ttu-id="4fd4a-133">Definuje také vyhodnocování profily, trochu a další atributy, které umožňuje nakonfigurovat chování indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-133">It also defines scoring profiles, suggesters and other attributes to configure the behavior of the index.</span></span>

<span data-ttu-id="4fd4a-134">Následující příklad uvádí ukázku schématu, používá pro vyhledávání na hotelů informace s pole popisu definované ve dvou jazycích.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-134">The following example provides an illustration of a schema used for searching on hotel information with the Description field defined in two languages.</span></span> <span data-ttu-id="4fd4a-135">Všimněte si, jak atributy řídit, jak se používá pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-135">Notice how attributes control how the field is used.</span></span> <span data-ttu-id="4fd4a-136">Například `hotelId` slouží jako klíč dokumentu (`"key": true`) a je vyloučen z fulltextové vyhledávání (`"searchable": false`).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-136">For example the `hotelId` is used as the document key (`"key": true`) and is excluded from full-text searches (`"searchable": false`).</span></span>

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

<span data-ttu-id="4fd4a-137">Po vytvoření indexu budete odesílat dokumenty, které naplňte index.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-137">After the index is created, you'll upload documents that populate the index.</span></span> <span data-ttu-id="4fd4a-138">V tématu [přidat nebo aktualizovat dokumenty](#AddOrUpdateDocuments) pro tento další krok.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-138">See [Add or Update Documents](#AddOrUpdateDocuments) for this next step.</span></span>

<span data-ttu-id="4fd4a-139">Video Úvod do indexu Azure Search, najdete v článku [Channel 9 cloudu zahrnují díl na Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-139">For a video introduction to indexing in Azure Search, see the [Channel 9 Cloud Cover episode on Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).</span></span>

<a name="CreateIndex"></a>

## <a name="create-index"></a><span data-ttu-id="4fd4a-140">Vytvoření indexu</span><span class="sxs-lookup"><span data-stu-id="4fd4a-140">Create Index</span></span>
<span data-ttu-id="4fd4a-141">Index je hlavním prostředkem, uspořádání a vyhledávání dokumentů ve službě Azure Search je podobná jak tabulku organizuje záznamů v databázi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-141">An index is the primary means of organizing and searching documents in Azure Search, similar to how a table organizes records in a database.</span></span> <span data-ttu-id="4fd4a-142">Každý index obsahuje kolekci dokumentů, že všechny odpovídat na schéma indexu (názvy polí, datové typy a vlastnosti), ale indexy určují také další konstrukce (trochu, vyhodnocování profily a možnosti CORS), které definují jiného chování vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-142">Each index has a collection of documents that all conform to the index schema (field names, data types, and properties), but indexes also specify additional constructs (suggesters, scoring profiles, and CORS options) that define other search behaviors.</span></span>

<span data-ttu-id="4fd4a-143">Můžete vytvořit nový index v rámci služby Azure Search pomocí požadavek HTTP POST nebo PUT.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-143">You can create a new index within an Azure Search service using an HTTP POST or PUT request.</span></span> <span data-ttu-id="4fd4a-144">Text žádosti je schématu JSON, která určuje informace index a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-144">The body of the request is a JSON schema that specifies the index and configuration information.</span></span>

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="4fd4a-145">Alternativně můžete použít PUT a zadejte název indexu v identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-145">Alternatively, you can use PUT and specify the index name on the URI.</span></span> <span data-ttu-id="4fd4a-146">Pokud index neexistuje, bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-146">If the index does not exist, it will be created.</span></span>

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

<span data-ttu-id="4fd4a-147">Vytvoření indexu určuje strukturu dokumenty uložené a použít v operace hledání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-147">Creating an index determines the structure of the documents stored and used in search operations.</span></span> <span data-ttu-id="4fd4a-148">Naplňování indexu je samostatný operace.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-148">Populating the index is a separate operation.</span></span> <span data-ttu-id="4fd4a-149">Pro tento krok, můžete použít [indexer](https://msdn.microsoft.com/library/azure/mt183328.aspx) (k dispozici pro podporované zdroje dat) nebo [přidání, aktualizace nebo odstranění dokumentů](https://msdn.microsoft.com/library/azure/dn798930.aspx) operaci.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-149">For this step, you can use an [indexer](https://msdn.microsoft.com/library/azure/mt183328.aspx) (available for supported data sources) or an [Add, Update, or Delete Documents](https://msdn.microsoft.com/library/azure/dn798930.aspx) operation.</span></span> <span data-ttu-id="4fd4a-150">Převedený index se vygeneruje, když jsou odeslány dokumenty.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-150">The inverted index is generated when the documents are posted.</span></span>

<span data-ttu-id="4fd4a-151">**Poznámka:**: maximální počet indexů povoleno se liší podle cenové úrovně.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-151">**Note**: The maximum number of indexes allowed varies by pricing tier.</span></span> <span data-ttu-id="4fd4a-152">Bezplatné služby umožňuje až 3 indexy.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-152">The free service allows up to 3 indexes.</span></span> <span data-ttu-id="4fd4a-153">Standardní služby umožňuje 50 indexy jednu službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-153">Standard service allows 50 indexes per Search service.</span></span> <span data-ttu-id="4fd4a-154">V tématu [limity a omezení](http://msdn.microsoft.com/library/azure/dn798934.aspx) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-154">See [Limits and constraints](http://msdn.microsoft.com/library/azure/dn798934.aspx) for details.</span></span>

<span data-ttu-id="4fd4a-155">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-155">**Request**</span></span>

<span data-ttu-id="4fd4a-156">Je požadován pro všechny žádosti o služby protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-156">HTTPS is required for all service requests.</span></span> <span data-ttu-id="4fd4a-157">**Create Index** požadavek lze sestavit pomocí Metoda POST nebo PUT metody.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-157">The **Create Index** request can be constructed using either a POST or PUT method.</span></span> <span data-ttu-id="4fd4a-158">Pokud používáte POST, je nutné zadat název indexu v těle žádosti spolu s definice schématu indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-158">When using POST, you must provide an index name in the request body along with the index schema definition.</span></span> <span data-ttu-id="4fd4a-159">Pomocí PUT název indexu je součástí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-159">With PUT, the index name is part of the URL.</span></span> <span data-ttu-id="4fd4a-160">Pokud index neexistuje, vytvoří se.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-160">If the index doesn't exist, it is created.</span></span> <span data-ttu-id="4fd4a-161">Pokud již existuje, je aktualizován na novou definici.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-161">If it already exists, it is updated to the new definition.</span></span>

<span data-ttu-id="4fd4a-162">Název indexu musí být malá písmena, začínat písmenem nebo číslicí, mít žádné lomítka nebo tečky a být kratší než 128 znaků.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-162">The index name must be lower case, start with a letter or number, have no slashes or dots, and be less than 128 characters.</span></span> <span data-ttu-id="4fd4a-163">Po spuštění název indexu s písmenem nebo číslicí, může obsahovat zbytek název jakékoli písmeno, čísla a pomlčky, dokud nejsou po sobě jdoucí pomlčky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-163">After starting the index name with a letter or number, the rest of the name can include any letter, number and dashes, as long as the dashes are not consecutive.</span></span>

<span data-ttu-id="4fd4a-164">`api-version` Je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-164">The `api-version` is required.</span></span> <span data-ttu-id="4fd4a-165">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) seznam dostupných verzí.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-165">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for a list of available versions.</span></span>

<span data-ttu-id="4fd4a-166">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-166">**Request Headers**</span></span>

<span data-ttu-id="4fd4a-167">Následující seznam popisuje hlavičky žádosti požadované a volitelné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-167">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="4fd4a-168">`Content-Type`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-168">`Content-Type`: Required.</span></span> <span data-ttu-id="4fd4a-169">Tuto možnost nastavíte na`application/json`</span><span class="sxs-lookup"><span data-stu-id="4fd4a-169">Set this to `application/json`</span></span>
* <span data-ttu-id="4fd4a-170">`api-key`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-170">`api-key`: Required.</span></span> <span data-ttu-id="4fd4a-171">`api-key` Se používá ke</span><span class="sxs-lookup"><span data-stu-id="4fd4a-171">The `api-key` is used to</span></span>
* <span data-ttu-id="4fd4a-172">ověřit požadavek na vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-172">authenticate the request to your Search service.</span></span> <span data-ttu-id="4fd4a-173">Je řetězcovou hodnotu, jedinečné pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-173">It is a string value, unique to your service.</span></span> <span data-ttu-id="4fd4a-174">**Create Index** musí zahrnovat požadavek `api-key` záhlaví nastavit klíče správce (na rozdíl od klíč dotazů).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-174">The **Create Index** request must include an `api-key` header set to your admin key (as opposed to a query key).</span></span>

<span data-ttu-id="4fd4a-175">Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-175">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="4fd4a-176">Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-176">You can get both the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="4fd4a-177">V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-177">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="4fd4a-178"><a name="RequestData"></a>
**Syntaxe požadavku textu**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-178"><a name="RequestData"></a>
**Request Body Syntax**</span></span>

<span data-ttu-id="4fd4a-179">Text žádosti obsahuje definici schématu, který obsahuje seznam datových polí v rámci dokumenty, kteří budou dodáni do tohoto indexu, datové typy, atributy, jakož i volitelný seznam vyhodnocování profily, které se používají ke stanovení skóre odpovídající dokumenty v době dotazů .</span><span class="sxs-lookup"><span data-stu-id="4fd4a-179">The body of the request contains a schema definition, which includes the list of data fields within documents that will be fed into this index, data types, attributes, as well as an optional list of scoring profiles that are used to score matching documents at query time.</span></span>

<span data-ttu-id="4fd4a-180">Všimněte si, že pro požadavek POST musíte zadat název indexu v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-180">Note that for a POST request, you must specify the index name in the request body.</span></span>

<span data-ttu-id="4fd4a-181">Může existovat pouze jedno pole s klíčem v indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-181">There can only be one key field in the index.</span></span> <span data-ttu-id="4fd4a-182">Musí být řetězcové pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-182">It has to be a string field.</span></span> <span data-ttu-id="4fd4a-183">Toto pole představuje jedinečný identifikátor každého dokumentu uloženého v indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-183">This field represents the unique identifier for each document stored within the index.</span></span>

<span data-ttu-id="4fd4a-184">Hlavní části indexu zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-184">The main parts of an index include the following:</span></span>

* `name`
* <span data-ttu-id="4fd4a-185">`fields`které budou dodáni do tohoto indexu, včetně názvu, datový typ a vlastnosti, které definují povolené akce na tomto poli.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-185">`fields` that will be fed into this index, including name, data type, and properties that define allowable actions on that field.</span></span>
* <span data-ttu-id="4fd4a-186">`suggesters`použít pro automatické dokončování nebo našeptávání dotazů.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-186">`suggesters` used for auto-complete or type-ahead queries.</span></span>
* <span data-ttu-id="4fd4a-187">`scoringProfiles`používá pro určení rozsahu skóre vlastní vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-187">`scoringProfiles` used for custom search score ranking.</span></span> <span data-ttu-id="4fd4a-188">V tématu [přidejte vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-188">See [Add scoring profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for details.</span></span>
* <span data-ttu-id="4fd4a-189">`analyzers`, `charFilters`, `tokenizers`, `tokenFilters` používá k definování, jak jsou dokumenty či dotazy rozdělen do indexovanou/prohledávatelné tokeny.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-189">`analyzers`, `charFilters`, `tokenizers`, `tokenFilters` used to define how your documents/queries are broken into indexable/searchable tokens.</span></span> <span data-ttu-id="4fd4a-190">V tématu [Analysis ve službě Azure Search](https://aka.ms//azsanalysis) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-190">See [Analysis in Azure Search](https://aka.ms//azsanalysis) for details.</span></span>
* <span data-ttu-id="4fd4a-191">`defaultScoringProfile`umožňuje přepsat výchozí chování vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-191">`defaultScoringProfile` used to overwrite the default scoring behaviors.</span></span>
* <span data-ttu-id="4fd4a-192">`corsOptions`tak, aby dotazy cross-origin oproti indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-192">`corsOptions` to allow cross-origin queries against your index.</span></span>

<span data-ttu-id="4fd4a-193">Syntaxe pro vytváření struktury datová část požadavku je následující.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-193">The syntax for structuring the request payload is as follows.</span></span> <span data-ttu-id="4fd4a-194">Ukázková žádost je k dispozici další na v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-194">A sample request is provided further on in this topic.</span></span>

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

<span data-ttu-id="4fd4a-195">**Atributy indexu**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-195">**Index Attributes**</span></span>

<span data-ttu-id="4fd4a-196">Následující atributy můžete nastavit při vytváření indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-196">The following attributes can be set when creating an index.</span></span> <span data-ttu-id="4fd4a-197">Podrobnosti o vyhodnocování a vyhodnocování profilů najdete v tématu [přidat vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-197">For details about scoring and scoring profiles, see [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span></span>

<span data-ttu-id="4fd4a-198">`name`-Nastaví název pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-198">`name` - Sets the name of the field.</span></span>

<span data-ttu-id="4fd4a-199">`type`-Nastaví datový typ pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-199">`type` - Sets the data type for the field.</span></span>

<span data-ttu-id="4fd4a-200">`searchable`-Označí pole jako fulltextově možnost vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-200">`searchable` - Marks the field as full-text search-able.</span></span> <span data-ttu-id="4fd4a-201">To znamená, že ho určitým analysis například dělení slov během indexování.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-201">This means it will undergo analysis such as word-breaking during indexing.</span></span> <span data-ttu-id="4fd4a-202">Pokud nastavíte `searchable` pole na hodnotu jako "slunečného dne", interně jej bude možné rozdělit na jednotlivé tokeny "slunečného" a "den".</span><span class="sxs-lookup"><span data-stu-id="4fd4a-202">If you set a `searchable` field to a value like "sunny day", internally it will be split into the individual tokens "sunny" and "day".</span></span> <span data-ttu-id="4fd4a-203">To umožňuje fulltextové vyhledávání pro tyto podmínky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-203">This enables full-text searches for these terms.</span></span> <span data-ttu-id="4fd4a-204">Pole typu `Edm.String` nebo `Collection(Edm.String)` jsou `searchable` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-204">Fields of type `Edm.String` or `Collection(Edm.String)` are `searchable` by default.</span></span> <span data-ttu-id="4fd4a-205">Pole dalších typů nelze `searchable`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-205">Fields of other types cannot be `searchable`.</span></span>

* <span data-ttu-id="4fd4a-206">**Poznámka:**: `searchable` pole využívat místo navíc v indexu, protože Azure Search budou ukládat další tokenizovaná verzi hodnotu pole pro fulltextové vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-206">**Note**: `searchable` fields consume extra space in your index since Azure Search will store an additional tokenized version of the field value for full-text searches.</span></span> <span data-ttu-id="4fd4a-207">Pokud chcete ušetřit místo v indexu a nepotřebujete pole mají být zahrnuty do hledání, nastavte `searchable` k `false`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-207">If you want to save space in your index and you don't need a field to be included in searches, set `searchable` to `false`.</span></span>

<span data-ttu-id="4fd4a-208">`filterable`– Umožňuje na pole odkazovat ve `$filter` dotazy.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-208">`filterable` - Allows the field to be referenced in `$filter` queries.</span></span> <span data-ttu-id="4fd4a-209">`filterable`se liší od `searchable` v tom, jak jsou zpracovávány řetězce.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-209">`filterable` differs from `searchable` in how strings are handled.</span></span> <span data-ttu-id="4fd4a-210">Pole typu `Edm.String` nebo `Collection(Edm.String)` , které jsou `filterable` nejsou prováděny dělení slov, tak porovnání jsou pro jenom přesné shody.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-210">Fields of type `Edm.String` or `Collection(Edm.String)` that are `filterable` do not undergo word-breaking, so comparisons are for exact matches only.</span></span> <span data-ttu-id="4fd4a-211">Například pokud nastavíte toto pole `f` "slunečného den" `$filter=f eq 'sunny'` se najít žádné shody, ale `$filter=f eq 'sunny day'` bude.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-211">For example, if you set such a field `f` to "sunny day", `$filter=f eq 'sunny'` will find no matches, but `$filter=f eq 'sunny day'` will.</span></span> <span data-ttu-id="4fd4a-212">Všechna pole jsou `filterable` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-212">All fields are `filterable` by default.</span></span>

<span data-ttu-id="4fd4a-213">`sortable`-Ve výchozím nastavení systém řadí výsledky podle skóre, ale v mnoha prostředí budou uživatelé chtít seřadit podle pole v dokumentech.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-213">`sortable` - By default the system sorts results by score, but in many experiences users will want to sort by fields in the documents.</span></span> <span data-ttu-id="4fd4a-214">Pole typu `Collection(Edm.String)` nemůže být `sortable`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-214">Fields of type `Collection(Edm.String)` cannot be `sortable`.</span></span> <span data-ttu-id="4fd4a-215">Všechna ostatní pole jsou `sortable` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-215">All other fields are `sortable` by default.</span></span>

<span data-ttu-id="4fd4a-216">`facetable`-Obvykle používaných v prezentace výsledky hledání, který obsahuje počet přístupů podle kategorie (pro příklad, vyhledejte digitální fotoaparáty a přístupů viz značku, megapixely, ceny, atd.).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-216">`facetable`- Typically used in a presentation of search results that includes hit count by category (for example, search for digital cameras and see hits by brand, by megapixels, by price, etc.).</span></span> <span data-ttu-id="4fd4a-217">Tuto možnost nelze použít s pole typu `Edm.GeographyPoint`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-217">This option cannot be used with fields of type `Edm.GeographyPoint`.</span></span> <span data-ttu-id="4fd4a-218">Všechna ostatní pole jsou `facetable` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-218">All other fields are `facetable` by default.</span></span>

* <span data-ttu-id="4fd4a-219">**Poznámka:**: pole typu `Edm.String` , které jsou `filterable`, `sortable`, nebo `facetable` může být maximálně 32 KB délku.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-219">**Note**: Fields of type `Edm.String` that are `filterable`, `sortable`, or `facetable` can be at most 32KB in length.</span></span> <span data-ttu-id="4fd4a-220">Je to proto, že tato pole jsou považovány za jeden hledaný termín a maximální délka termín, který se ve službě Azure Search je 32KB.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-220">This is because such fields are treated as a single search term, and the maximum length of a term in Azure Search is 32KB.</span></span> <span data-ttu-id="4fd4a-221">Pokud potřebujete ukládat další text než tento v poli jednoho řetězce, je potřeba explicitně nastavit `filterable`, `sortable`, a `facetable` k `false` v definici indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-221">If you need to store more text than this in a single string field, you will need to explicitly set `filterable`, `sortable`, and `facetable` to `false` in your index definition.</span></span>
* <span data-ttu-id="4fd4a-222">**Poznámka:**: Pokud má pole žádná z výše uvedených atributů nastavena na hodnotu `true` (`searchable`, `filterable`, `sortable`, nebo`facetable`) pole je efektivně vyloučen z obráceným index.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-222">**Note**: If a field has none of the above attributes set to `true` (`searchable`, `filterable`, `sortable`,  or`facetable`) the field is effectively excluded from the inverted index.</span></span> <span data-ttu-id="4fd4a-223">Tato možnost je užitečná pro pole, které nejsou používány v dotazech, ale je potřeba mít ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-223">This option is useful for fields that are not used in queries, but are needed in search results.</span></span> <span data-ttu-id="4fd4a-224">Kromě těchto polí z indexu zvyšuje výkon.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-224">Excluding such fields from the index improves performance.</span></span>

<span data-ttu-id="4fd4a-225">`key`-Označí pole jako obsahující jedinečné identifikátory pro dokumenty v indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-225">`key` - Marks the field as containing unique identifiers for documents within the index.</span></span> <span data-ttu-id="4fd4a-226">Právě jedno pole musí být zvolena jako `key` pole a musí být typu `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-226">Exactly one field must be chosen as the `key` field and it must be of type `Edm.String`.</span></span> <span data-ttu-id="4fd4a-227">Klíčová pole lze použít k vyhledání dokumentů přímo prostřednictvím [rozhraní API pro vyhledávání](#LookupAPI).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-227">Key fields can be used to look up documents directly via the [Lookup API](#LookupAPI).</span></span>

<span data-ttu-id="4fd4a-228">`retrievable`-Nastaví, zda může být pole vrácené ve výsledku hledání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-228">`retrievable` - Sets whether the field can be returned in a search result.</span></span>  <span data-ttu-id="4fd4a-229">To je užitečné, když chcete pole (například okraje) použít jako filtr, řazení a vyhodnocování mechanismus, ale nechcete, aby pole, které chcete být viditelné pro koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-229">This is useful when you want to use a field (for example, margin) as a filter, sorting, or scoring mechanism but do not want the field to be visible to the end user.</span></span> <span data-ttu-id="4fd4a-230">Tento atribut musí být `true` pro `key` pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-230">This attribute must be `true` for `key` fields.</span></span>

<span data-ttu-id="4fd4a-231">`analyzer`-Nastaví název analyzátor použití pole na čas při vyhledávání a indexování čas.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-231">`analyzer` - Sets the name of the analyzer to use for the field at search time and indexing time.</span></span> <span data-ttu-id="4fd4a-232">Pro sadu povolených hodnot najdete v části [analyzátorů](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-232">For the allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="4fd4a-233">Tuto možnost lze použít pouze s `searchable` pole a nelze nastavit společně s buď `searchAnalyzer` nebo `indexAnalyzer`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-233">This option can be used only with `searchable` fields and it can't be set together with either `searchAnalyzer` or `indexAnalyzer`.</span></span>  <span data-ttu-id="4fd4a-234">Jakmile analyzátor jste vybrali, nelze změnit pro pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-234">Once the analyzer is chosen, it cannot be changed for the field.</span></span>

<span data-ttu-id="4fd4a-235">`searchAnalyzer`-Nastaví název analyzátor používá během hledání pro pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-235">`searchAnalyzer` - Sets the name of the analyzer used at search time for the field.</span></span> <span data-ttu-id="4fd4a-236">Pro sadu povolených hodnot najdete v části [analyzátorů](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-236">For the allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="4fd4a-237">Tuto možnost lze použít pouze s `searchable` pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-237">This option can be used only with `searchable` fields.</span></span> <span data-ttu-id="4fd4a-238">Je nutné ji nastavit společně s `indexAnalyzer` a nelze ji nastavit společně s `analyzer` možnost.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-238">It must be set together with `indexAnalyzer` and it cannot be set together with the `analyzer` option.</span></span> <span data-ttu-id="4fd4a-239">Tento analyzátor mohou být aktualizovány na stávající pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-239">This analyzer can be updated on an existing field.</span></span>

<span data-ttu-id="4fd4a-240">`indexAnalyzer`-Nastaví název analyzátor používá během indexování pro pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-240">`indexAnalyzer` - Sets the name of the analyzer used at indexing time for the field.</span></span> <span data-ttu-id="4fd4a-241">Pro sadu povolených hodnot najdete v části [analyzátorů](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-241">For the allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="4fd4a-242">Tuto možnost lze použít pouze s `searchable` pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-242">This option can be used only with `searchable` fields.</span></span> <span data-ttu-id="4fd4a-243">Je nutné ji nastavit společně s `searchAnalyzer` a nelze ji nastavit společně s `analyzer` možnost.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-243">It must be set together with `searchAnalyzer` and it cannot be set together with the `analyzer` option.</span></span> <span data-ttu-id="4fd4a-244">Jakmile analyzátor jste vybrali, nelze změnit pro pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-244">Once the analyzer is chosen, it cannot be changed for the field.</span></span>

<span data-ttu-id="4fd4a-245">`suggesters`-Nastaví režim hledání a pole, které jsou zdroj obsahu pro návrhy.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-245">`suggesters` - Sets the search mode and fields that are the source of the content for suggestions.</span></span> <span data-ttu-id="4fd4a-246">V tématu [trochu](#Suggesters) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-246">See [Suggesters](#Suggesters) for details.</span></span>

<span data-ttu-id="4fd4a-247">`scoringProfiles`-Definuje vlastní vyhodnocování chování, které umožňují ovlivnit položky zobrazovat na vyšších pozicích ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-247">`scoringProfiles` - Defines custom scoring behaviors that let you influence which items appear higher in search results.</span></span> <span data-ttu-id="4fd4a-248">Vyhodnocování profily jsou tvořeny váhu pole a funkce.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-248">Scoring profiles are made up of field weights and functions.</span></span> <span data-ttu-id="4fd4a-249">V tématu [přidat vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) Další informace o atributech použít v profil vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-249">See [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for more information about the attributes used in a scoring profile.</span></span>

<span data-ttu-id="4fd4a-250"><!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Jazyková podpora**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-250"><!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Language support**</span></span>

<span data-ttu-id="4fd4a-251">Prohledatelná pole podstoupit analysis která nejvíc často zahrnuje dělení slov, normalizaci text a filtrování podmínky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-251">Searchable fields undergo analysis that most frequently involves word-breaking, text normalization, and filtering out terms.</span></span> <span data-ttu-id="4fd4a-252">Ve výchozím nastavení jsou prohledatelná pole ve službě Azure Search analyzovaný se [Apache Lucene standardní analyzátor](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) která dělí text do elementů následující["Segmentace Unicode Text"](http://unicode.org/reports/tr29/) pravidla.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-252">By default, searchable fields in Azure Search are analyzed with the [Apache Lucene Standard analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) which breaks text into elements following the["Unicode Text Segmentation"](http://unicode.org/reports/tr29/) rules.</span></span> <span data-ttu-id="4fd4a-253">Kromě toho standardní analyzátor převede všechny znaky na jejich formulář malá písmena.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-253">Additionally, the standard analyzer converts all characters to their lower case form.</span></span> <span data-ttu-id="4fd4a-254">Indexované dokumenty a hledaných termínů projít analýza během indexování a zpracování dotazu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-254">Both indexed documents and search terms go through the analysis during indexing and query processing.</span></span>

<span data-ttu-id="4fd4a-255">Služba Azure Search podporuje různé jazyky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-255">Azure Search supports a variety of languages.</span></span> <span data-ttu-id="4fd4a-256">Každý jazyk vyžaduje analyzer nestandardní text, který účty pro charakteristiky daného jazyka.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-256">Each language requires a non-standard text analyzer which accounts for characteristics of a given language.</span></span> <span data-ttu-id="4fd4a-257">Služba Azure Search nabízí dva typy analyzátorů:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-257">Azure Search offers two types of analyzers:</span></span>

* <span data-ttu-id="4fd4a-258">35 analyzátorů zajišťoval Lucene.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-258">35 analyzers backed by Lucene.</span></span>
* <span data-ttu-id="4fd4a-259">50 analyzátorů zajišťoval proprietární přirozeného jazyka Microsoft zpracování technologie používané v kanceláři a Bing.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-259">50 analyzers backed by proprietary Microsoft natural language processing technology used in Office and Bing.</span></span>

<span data-ttu-id="4fd4a-260">Někteří vývojáři preferovat známější, jednoduchý a open-source řešení Lucene.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-260">Some developers might prefer the more familiar, simple, open-source solution of Lucene.</span></span> <span data-ttu-id="4fd4a-261">Lucene analyzátorů je rychlejší, ale Microsoft analyzátorů mít rozšířené možnosti, například Lematizace, word decompounding (v jazycích jako němčina, dánština, holandština, švédština, norština, estonština, dokončit, maďarština, slovenština) a entity rozpoznávání (adresy URL. e-mailů, kalendářních dat, čísel).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-261">Lucene analyzers are faster, but the Microsoft analyzers have advanced capabilities, such as lemmatization, word decompounding (in languages like German, Danish, Dutch, Swedish, Norwegian, Estonian, Finish, Hungarian, Slovak) and entity recognition (URLs, emails, dates, numbers).</span></span> <span data-ttu-id="4fd4a-262">Pokud je to možné byste měli spustit porovnání společnosti Microsoft a Lucene analyzátorů rozhodnout, které z nich je lepší přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-262">If possible, you should run comparisons of both the Microsoft and Lucene analyzers to decide which one is a better fit.</span></span>

<span data-ttu-id="4fd4a-263">***Jejich porovnání***</span><span class="sxs-lookup"><span data-stu-id="4fd4a-263">***How they compare***</span></span>

<span data-ttu-id="4fd4a-264">Analyzátor Lucene pro angličtinu rozšiřuje standardní analyzátor.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-264">The Lucene analyzer for English extends the standard analyzer.</span></span> <span data-ttu-id="4fd4a-265">Se odebere z slova Přivlastňovací pád (koncové společnosti), platí rozklad dle [silné rozklad algoritmus](http://tartarus.org/~martin/PorterStemmer/)a také odebere Angličtina [zastavit slova](http://en.wikipedia.org/wiki/Stop_words).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-265">It removes possessives (trailing 's) from words, applies stemming as per [Porter Stemming algorithm](http://tartarus.org/~martin/PorterStemmer/), and removes English [stop words](http://en.wikipedia.org/wiki/Stop_words).</span></span>

<span data-ttu-id="4fd4a-266">Porovnání provede Microsoft analyzer Lematizace místo rozklad.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-266">In comparison, the Microsoft analyzer performs lemmatization instead of stemming.</span></span> <span data-ttu-id="4fd4a-267">Znamená to, se může zpracovat tvary a nestandardní word forms mnohem lepší co má za následek více relevantní výsledky hledání (sledovat modul 7 [Azure Search MVA prezentace](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-267">It means it can handle inflected and irregular word forms much better what results in more relevant search results (watch module 7 of [Azure Search MVA presentation](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) for more details).</span></span>

<span data-ttu-id="4fd4a-268">Indexování s Microsoft analyzátorů je v průměru dvakrát až třikrát pomalejší než jejich ekvivalenty Lucene, v závislosti na jazyk.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-268">Indexing with Microsoft analyzers is on average two to three times slower than their Lucene equivalents, depending on the language.</span></span> <span data-ttu-id="4fd4a-269">Pro dotazy Průměrná velikost nesmí být výrazně ovlivněn výkon vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-269">Search performance should not be significantly affected for average size queries.</span></span>

<span data-ttu-id="4fd4a-270">***Konfigurace***</span><span class="sxs-lookup"><span data-stu-id="4fd4a-270">***Configuration***</span></span>

<span data-ttu-id="4fd4a-271">Pro každé pole v definici indexu můžete nastavit `analyzer` vlastnost na název analyzátor, který určuje, které jazyk a dodavatele.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-271">For each field in the index definition, you can set the `analyzer` property to an analyzer name that specifies which language and vendor.</span></span> <span data-ttu-id="4fd4a-272">Stejné Analyzátor se použijí, když indexování a hledání toto pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-272">The same analyzer will be applied when indexing and searching for that field.</span></span>
<span data-ttu-id="4fd4a-273">Například můžete mít samostatné pole pro angličtinu, francouzštinu a španělština hotelů popisy, které existují souběžného ve stejném indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-273">For example, you can have separate fields for English, French, and Spanish hotel descriptions that exist side-by-side in the same index.</span></span> <span data-ttu-id="4fd4a-274">Použití [parametr dotazu 'searchFields'](#SearchQueryParameters) k určení, které pro specifický jazyk pole pro vyhledávání proti v dotazech.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-274">Use the ['searchFields' query parameter](#SearchQueryParameters) to specify which language-specific field to search against in your queries.</span></span> <span data-ttu-id="4fd4a-275">Příklady dotazů, které zahrnují můžete zkontrolovat `analyzer` vlastnost [vyhledávání dokumentů](#SearchDocs).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-275">You can review query examples that include the `analyzer` property in [Search Documents](#SearchDocs).</span></span> 

<span data-ttu-id="4fd4a-276">***Analyzátor seznamu***</span><span class="sxs-lookup"><span data-stu-id="4fd4a-276">***Analyzer list***</span></span>

<span data-ttu-id="4fd4a-277">Níže je seznam podporovaných jazyků společně s názvy analyzátor Lucene a společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-277">Below is the list of supported languages together with Lucene and Microsoft analyzer names.</span></span>

<table style="font-size:12">
    <tr>
        <th><span data-ttu-id="4fd4a-278">Jazyk</span><span class="sxs-lookup"><span data-stu-id="4fd4a-278">Language</span></span></th>
        <th><span data-ttu-id="4fd4a-279">Název analyzátor Microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-279">Microsoft analyzer name</span></span></th>
        <th><span data-ttu-id="4fd4a-280">Název analyzátor Lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-280">Lucene analyzer name</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-281">arabština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-281">Arabic</span></span></td>
        <td><span data-ttu-id="4fd4a-282">ar.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-282">ar.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-283">ar.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-283">ar.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-284">Arménština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-284">Armenian</span></span></td>
        <td></td>
        <td><span data-ttu-id="4fd4a-285">hy.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-285">hy.lucene</span></span></td>
      </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-286">Bengálském</span><span class="sxs-lookup"><span data-stu-id="4fd4a-286">Bangla</span></span></td>
        <td><span data-ttu-id="4fd4a-287">Bn.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-287">bn.microsoft</span></span></td>
        <td></td>
    </tr>
      <tr>
        <td><span data-ttu-id="4fd4a-288">Baskičtina</span><span class="sxs-lookup"><span data-stu-id="4fd4a-288">Basque</span></span></td>
        <td></td>
        <td><span data-ttu-id="4fd4a-289">EU.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-289">eu.lucene</span></span></td>
    </tr>
      <tr>
         <td><span data-ttu-id="4fd4a-290">Bulharština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-290">Bulgarian</span></span></td>
        <td><span data-ttu-id="4fd4a-291">BG.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-291">bg.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-292">BG.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-292">bg.lucene</span></span></td>
      </tr>
      <tr>
        <td><span data-ttu-id="4fd4a-293">Katalánština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-293">Catalan</span></span></td>
        <td><span data-ttu-id="4fd4a-294">CA.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-294">ca.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-295">CA.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-295">ca.lucene</span></span></td>          
      </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-296">Zjednodušená čínština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-296">Chinese Simplified</span></span></td>
        <td><span data-ttu-id="4fd4a-297">zh-Hans.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-297">zh-Hans.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-298">zh-Hans.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-298">zh-Hans.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-299">Tradiční čínština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-299">Chinese Traditional</span></span></td>
        <td><span data-ttu-id="4fd4a-300">zh-Hant.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-300">zh-Hant.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-301">zh-Hant.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-301">zh-Hant.lucene</span></span></td>        
    <tr>
    <tr>
        <td><span data-ttu-id="4fd4a-302">Chorvatština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-302">Croatian</span></span></td>
        <td><span data-ttu-id="4fd4a-303">hr.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-303">hr.microsoft</span></span></td>
        <td/></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-304">čeština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-304">Czech</span></span></td>
        <td><span data-ttu-id="4fd4a-305">cs.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-305">cs.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-306">cs.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-306">cs.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="4fd4a-307">dánština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-307">Danish</span></span></td>
        <td><span data-ttu-id="4fd4a-308">da.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-308">da.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-309">da.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-309">da.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="4fd4a-310">holandština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-310">Dutch</span></span></td>
        <td><span data-ttu-id="4fd4a-311">NL.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-311">nl.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-312">NL.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-312">nl.lucene</span></span></td>    
    </tr>    
    <tr>
        <td><span data-ttu-id="4fd4a-313">Angličtina</span><span class="sxs-lookup"><span data-stu-id="4fd4a-313">English</span></span></td>        
        <td><span data-ttu-id="4fd4a-314">en.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-314">en.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-315">en.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-315">en.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-316">Estonština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-316">Estonian</span></span></td>
        <td><span data-ttu-id="4fd4a-317">et.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-317">et.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-318">finština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-318">Finnish</span></span></td>
        <td><span data-ttu-id="4fd4a-319">Fi.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-319">fi.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-320">Fi.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-320">fi.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="4fd4a-321">francouzština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-321">French</span></span></td>
        <td><span data-ttu-id="4fd4a-322">FR.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-322">fr.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-323">FR.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-323">fr.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-324">Galicijština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-324">Galician</span></span></td>
        <td></td>
        <td><span data-ttu-id="4fd4a-325">GL.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-325">gl.lucene</span></span></td>        
      </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-326">němčina</span><span class="sxs-lookup"><span data-stu-id="4fd4a-326">German</span></span></td>
        <td><span data-ttu-id="4fd4a-327">de.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-327">de.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-328">de.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-328">de.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-329">řečtina</span><span class="sxs-lookup"><span data-stu-id="4fd4a-329">Greek</span></span></td>
        <td><span data-ttu-id="4fd4a-330">El.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-330">el.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-331">El.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-331">el.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-332">Gudžarátština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-332">Gujarati</span></span></td>
        <td><span data-ttu-id="4fd4a-333">gu.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-333">gu.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-334">Hebrejština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-334">Hebrew</span></span></td>
        <td><span data-ttu-id="4fd4a-335">he.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-335">he.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-336">Hindština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-336">Hindi</span></span></td>
        <td><span data-ttu-id="4fd4a-337">Hi.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-337">hi.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-338">Hi.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-338">hi.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-339">maďarština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-339">Hungarian</span></span></td>        
        <td><span data-ttu-id="4fd4a-340">HU.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-340">hu.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-341">HU.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-341">hu.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-342">Islandština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-342">Icelandic</span></span></td>
        <td><span data-ttu-id="4fd4a-343">is.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-343">is.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-344">Indonéština (Bahasa)</span><span class="sxs-lookup"><span data-stu-id="4fd4a-344">Indonesian (Bahasa)</span></span></td>
        <td><span data-ttu-id="4fd4a-345">ID.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-345">id.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-346">ID.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-346">id.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-347">Irština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-347">Irish</span></span></td>
        <td></td>
          <td><span data-ttu-id="4fd4a-348">GA.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-348">ga.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-349">italština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-349">Italian</span></span></td>
        <td><span data-ttu-id="4fd4a-350">IT.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-350">it.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-351">IT.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-351">it.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-352">japonština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-352">Japanese</span></span></td>
        <td><span data-ttu-id="4fd4a-353">ja.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-353">ja.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-354">ja.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-354">ja.lucene</span></span></td>

    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-355">Kannadština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-355">Kannada</span></span></td>
        <td><span data-ttu-id="4fd4a-356">Ka.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-356">ka.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-357">korejština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-357">Korean</span></span></td>
        <td><span data-ttu-id="4fd4a-358">Ko.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-358">ko.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-359">Ko.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-359">ko.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-360">Lotyština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-360">Latvian</span></span></td>        
        <td><span data-ttu-id="4fd4a-361">LV.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-361">lv.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-362">LV.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-362">lv.lucene</span></span></td>    
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-363">Litevština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-363">Lithuanian</span></span></td>
        <td><span data-ttu-id="4fd4a-364">lt.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-364">lt.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-365">Malajalámština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-365">Malayalam</span></span></td>
        <td><span data-ttu-id="4fd4a-366">ml.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-366">ml.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-367">Malajština (Latina)</span><span class="sxs-lookup"><span data-stu-id="4fd4a-367">Malay (Latin)</span></span></td>
        <td><span data-ttu-id="4fd4a-368">MS.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-368">ms.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-369">Maráthština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-369">Marathi</span></span></td>
        <td><span data-ttu-id="4fd4a-370">MR.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-370">mr.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-371">norština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-371">Norwegian</span></span></td>
        <td><span data-ttu-id="4fd4a-372">NB.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-372">nb.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-373">No.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-373">no.lucene</span></span></td>        
    </tr>
      <tr>
        <td><span data-ttu-id="4fd4a-374">Perština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-374">Persian</span></span></td>
        <td></td>
        <td><span data-ttu-id="4fd4a-375">fa.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-375">fa.lucene</span></span></td>        
      </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-376">polština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-376">Polish</span></span></td>
        <td><span data-ttu-id="4fd4a-377">PL.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-377">pl.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-378">PL.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-378">pl.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-379">Portugalština (Brazílie)</span><span class="sxs-lookup"><span data-stu-id="4fd4a-379">Portuguese (Brazil)</span></span></td>
        <td><span data-ttu-id="4fd4a-380">PT-Br.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-380">pt-Br.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-381">PT-Br.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-381">pt-Br.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-382">Portugalština (Portugalsko)</span><span class="sxs-lookup"><span data-stu-id="4fd4a-382">Portuguese (Portugal)</span></span></td>
        <td><span data-ttu-id="4fd4a-383">PT-Pt.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-383">pt-Pt.microsoft</span></span></td>        
        <td><span data-ttu-id="4fd4a-384">PT-Pt.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-384">pt-Pt.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-385">Paňdžábština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-385">Punjabi</span></span></td>
        <td><span data-ttu-id="4fd4a-386">Pa.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-386">pa.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-387">rumunština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-387">Romanian</span></span></td>
        <td><span data-ttu-id="4fd4a-388">ro.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-388">ro.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-389">ro.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-389">ro.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-390">ruština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-390">Russian</span></span></td>
        <td><span data-ttu-id="4fd4a-391">RU.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-391">ru.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-392">RU.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-392">ru.lucene</span></span></td>    
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-393">Srbština (cyrilice)</span><span class="sxs-lookup"><span data-stu-id="4fd4a-393">Serbian (Cyrillic)</span></span></td>
        <td><span data-ttu-id="4fd4a-394">SR-cyrillic.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-394">sr-cyrillic.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-395">Srbština (Latina)</span><span class="sxs-lookup"><span data-stu-id="4fd4a-395">Serbian (Latin)</span></span></td>
        <td><span data-ttu-id="4fd4a-396">SR-latin.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-396">sr-latin.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-397">Slovenština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-397">Slovak</span></span></td>
        <td><span data-ttu-id="4fd4a-398">Sk.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-398">sk.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-399">Slovinština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-399">Slovenian</span></span></td>
        <td><span data-ttu-id="4fd4a-400">SL.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-400">sl.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-401">španělština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-401">Spanish</span></span></td>
        <td><span data-ttu-id="4fd4a-402">ES.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-402">es.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-403">ES.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-403">es.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-404">švédština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-404">Swedish</span></span></td>
        <td><span data-ttu-id="4fd4a-405">Sv.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-405">sv.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-406">Sv.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-406">sv.lucene</span></span></td>
    </tr>

    <tr>
        <td><span data-ttu-id="4fd4a-407">Tamilština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-407">Tamil</span></span></td>
        <td><span data-ttu-id="4fd4a-408">ta.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-408">ta.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-409">Telugština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-409">Telugu</span></span></td>
        <td><span data-ttu-id="4fd4a-410">te.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-410">te.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-411">Thajština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-411">Thai</span></span></td>
        <td><span data-ttu-id="4fd4a-412">TH.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-412">th.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-413">TH.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-413">th.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-414">turečtina</span><span class="sxs-lookup"><span data-stu-id="4fd4a-414">Turkish</span></span></td>
        <td><span data-ttu-id="4fd4a-415">TR.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-415">tr.microsoft</span></span></td>
        <td><span data-ttu-id="4fd4a-416">TR.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-416">tr.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-417">Ukrajinština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-417">Ukrainian</span></span></td>
        <td><span data-ttu-id="4fd4a-418">UK.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-418">uk.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-419">Urdština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-419">Urdu</span></span></td>
        <td><span data-ttu-id="4fd4a-420">Your.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-420">ur.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-421">Vietnamština</span><span class="sxs-lookup"><span data-stu-id="4fd4a-421">Vietnamese</span></span></td>
        <td><span data-ttu-id="4fd4a-422">VI.microsoft</span><span class="sxs-lookup"><span data-stu-id="4fd4a-422">vi.microsoft</span></span></td>
        <td></td>
    </tr>
    <td colspan="3"><span data-ttu-id="4fd4a-423">Kromě toho poskytuje Azure Search bez ohledu na jazyk analyzátor konfigurace</span><span class="sxs-lookup"><span data-stu-id="4fd4a-423">Additionally Azure Search provides language-agnostic analyzer configurations</span></span></td>
    <tr>
        <td><span data-ttu-id="4fd4a-424">Skládání standardní ASCII</span><span class="sxs-lookup"><span data-stu-id="4fd4a-424">Standard ASCII Folding</span></span></td>
        <td><span data-ttu-id="4fd4a-425">standardasciifolding.lucene</span><span class="sxs-lookup"><span data-stu-id="4fd4a-425">standardasciifolding.lucene</span></span></td>
        <td>
        <ul>
            <li><span data-ttu-id="4fd4a-426">Segmentace text Unicode (standardní Tokenizátor)</span><span class="sxs-lookup"><span data-stu-id="4fd4a-426">Unicode text segmentation (Standard Tokenizer)</span></span></li>
            <li><span data-ttu-id="4fd4a-427">Skládání filtru ASCII – převede znaky znakové sady Unicode, které nepatří do sady nejprve 127 znaků ASCII do jejich ekvivalenty ASCII.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-427">ASCII folding filter - converts Unicode characters that don't belong to the set of first 127 ASCII characters into their ASCII equivalents.</span></span> <span data-ttu-id="4fd4a-428">To je užitečné pro odebrání znaky s diakritikou.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-428">This is useful for removing diacritics.</span></span></li>
        </ul>
        </td>
    </tr>
</table>

<span data-ttu-id="4fd4a-429">Všechny analyzátory s názvy opatřen poznámkou <i>lucene</i> se používá technologii [analyzátory jazyka Apache Lucene](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-429">All analyzers with names annotated with <i>lucene</i> are powered by [Apache Lucene's language analyzers](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html).</span></span> <span data-ttu-id="4fd4a-430">Další informace o ASCII skládání filtru najdete [zde](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-430">More information about the ASCII folding filter can be found [here](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).</span></span>

<span data-ttu-id="4fd4a-431">**Moduly pro návrhy**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-431">**Suggesters**</span></span>

<span data-ttu-id="4fd4a-432">A `suggester` definuje pole v indexu, která se používají pro podporu automatického dokončování v hledání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-432">A `suggester` defines which fields in an index are used to support auto-complete in searches.</span></span> <span data-ttu-id="4fd4a-433">Obvykle jsou odesílány částečné vyhledávání řetězce [návrhy API](#Suggestions) při uživatele je zadáním vyhledávací dotaz a rozhraní API vrací sadu navrhované frází.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-433">Typically partial search strings are sent to the [Suggestions API](#Suggestions) while the user is typing a search query, and the API returns a set of suggested phrases.</span></span> <span data-ttu-id="4fd4a-434">Modulu pro návrhy, kterou definujete v indexu určuje pole, která slouží k vytváření našeptávání hledaným výrazům.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-434">A suggester that you define in the index determines which fields are used to build the type-ahead search terms.</span></span> <span data-ttu-id="4fd4a-435">V tématu [trochu](#Suggesters) podrobnosti o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-435">See [Suggesters](#Suggesters) for configuration details.</span></span>

<span data-ttu-id="4fd4a-436">**Profily skórování**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-436">**Scoring profiles**</span></span>

<span data-ttu-id="4fd4a-437">A `scoringProfile` definuje vlastní vyhodnocování chování, které umožňují ovlivnit položky zobrazovat na vyšších pozicích ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-437">A `scoringProfile` defines custom scoring behaviors that let you influence which items appear higher in the search results.</span></span> <span data-ttu-id="4fd4a-438">Vyhodnocování profily jsou tvořeny váhu pole a funkce.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-438">Scoring profiles are made up of field weights and functions.</span></span> <span data-ttu-id="4fd4a-439">Jejich použití, zadejte profil podle názvu na řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-439">To use them, you specify a profile by name on the query string.</span></span>

<span data-ttu-id="4fd4a-440">Výchozí profil vyhodnocování funguje na pozadí k výpočtu skóre vyhledávání pro každou položku v sadě výsledků.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-440">A default scoring profile operates behind the scenes to compute a search score for every item in a result set.</span></span> <span data-ttu-id="4fd4a-441">Můžete použít interní, nepojmenované profil vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-441">You can use the internal, unnamed scoring profile.</span></span> <span data-ttu-id="4fd4a-442">Alternativně nastavte `defaultScoringProfile` chcete použít vlastní profil jako výchozí, vyvolá vždy, když není zadaný vlastního profilu v řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-442">Alternatively, set `defaultScoringProfile` to use a custom profile as the default, invoked whenever a custom profile is not specified on the query string.</span></span>

<span data-ttu-id="4fd4a-443">V tématu [vyhodnocování profily přidat do indexu vyhledávání (služby REST API Azure Search)](search-api-scoring-profiles-2015-02-28-preview.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-443">See [Add scoring profiles to a search index (Azure Search Service REST API)](search-api-scoring-profiles-2015-02-28-preview.md) for details.</span></span>

<span data-ttu-id="4fd4a-444">**Možnosti CORS**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-444">**CORS Options**</span></span>

<span data-ttu-id="4fd4a-445">Javascript na straně klienta nelze volat všechny rozhraní API ve výchozím nastavení, protože prohlížeč zabrání všech žádostí napříč zdroji.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-445">Client-side Javascript cannot call any APIs by default since the browser will prevent all cross-origin requests.</span></span> <span data-ttu-id="4fd4a-446">Povolení CORS (sdílení prostředků různého původu) nastavením `corsOptions` atribut tak, aby dotazy nepůvodního zdroje do indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-446">Enable CORS (Cross-Origin Resource Sharing) by setting the `corsOptions` attribute to allow cross-origin queries to your index.</span></span> <span data-ttu-id="4fd4a-447">Všimněte si, že pouze dotaz rozhraní API pro podporu CORS z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-447">Note that only query APIs support CORS for security reasons.</span></span> <span data-ttu-id="4fd4a-448">Tyto možnosti můžete nastavit pro CORS:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-448">The following options can be set for CORS:</span></span>

* <span data-ttu-id="4fd4a-449">`allowedOrigins`(povinné): Toto je seznam původů, které bude udělen přístup k indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-449">`allowedOrigins` (required): This is a list of origins that will be granted access to your index.</span></span> <span data-ttu-id="4fd4a-450">To znamená, že žádný kód Javascript zpracování těchto původem se bude moct dotazování indexu (za předpokladu, že poskytuje správný klíč rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-450">This means that any Javascript code served from those origins will be allowed to query your index (assuming it provides the correct API key).</span></span> <span data-ttu-id="4fd4a-451">Každý původ je obvykle ve formátu `protocol://fully-qualified-domain-name:port` i když port je často vynechán.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-451">Each origin is typically of the form `protocol://fully-qualified-domain-name:port` although the port is often omitted.</span></span> <span data-ttu-id="4fd4a-452">V tématu [v tomto článku](http://go.microsoft.com/fwlink/?LinkId=330822) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-452">See [this article](http://go.microsoft.com/fwlink/?LinkId=330822) for more details.</span></span>
  * <span data-ttu-id="4fd4a-453">Pokud chcete povolit přístup k všechny původy, zahrnují `*` jako jedna položka v `allowedOrigins` pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-453">If you want to allow access to all origins, include `*` as a single item in the `allowedOrigins` array.</span></span> <span data-ttu-id="4fd4a-454">Všimněte si, že **to není doporučeno, postup pro produkční vyhledávací služby.**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-454">Note that **this is not recommended practice for production search services.**</span></span> <span data-ttu-id="4fd4a-455">Však může být užitečné pro vývoj nebo účely ladění.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-455">However, it may be useful for development or debugging purposes.</span></span>
* <span data-ttu-id="4fd4a-456">`maxAgeInSeconds`(volitelné): prohlížeče tato hodnota slouží k určení doby (v sekundách) k předběžných odpovědí CORS mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-456">`maxAgeInSeconds` (optional): Browsers use this value to determine the duration (in seconds) to cache CORS preflight responses.</span></span> <span data-ttu-id="4fd4a-457">Toto musí být nezáporné celé číslo.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-457">This must be a non-negative integer.</span></span> <span data-ttu-id="4fd4a-458">Čím větší je tato hodnota, tím lepší bude výkon, ale tím déle bude taky trvat se projeví změny zásad CORS.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-458">The larger this value is, the better performance will be, but the longer it will take for CORS policy changes to take effect.</span></span> <span data-ttu-id="4fd4a-459">Pokud není nastavena, použije se výchozí hodnota 5 minut.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-459">If it is not set, a default duration of 5 minutes will be used.</span></span>

<span data-ttu-id="4fd4a-460"><a name="CreateUpdateIndexExample"></a>
**Příklad text žádosti**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-460"><a name="CreateUpdateIndexExample"></a>
**Request Body Example**</span></span>

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

<span data-ttu-id="4fd4a-461">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-461">**Response**</span></span>

<span data-ttu-id="4fd4a-462">Pro úspěšné žádosti: "201 – vytvořeno".</span><span class="sxs-lookup"><span data-stu-id="4fd4a-462">For a successful request: "201 Created".</span></span>

<span data-ttu-id="4fd4a-463">Ve výchozím nastavení bude obsahovat text odpovědi JSON pro definici indexu, která byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-463">By default the response body will contain the JSON for the index definition that was created.</span></span> <span data-ttu-id="4fd4a-464">Pokud `Prefer` hlavička požadavku je nastaven na `return=minimal`, text odpovědi bude prázdný, a bude kód stavu úspěch "204 žádný obsah" místo "201 – vytvořeno".</span><span class="sxs-lookup"><span data-stu-id="4fd4a-464">If the `Prefer` request header is set to `return=minimal`, the response body will be empty and the success status code will be "204 No Content" instead of "201 Created".</span></span> <span data-ttu-id="4fd4a-465">To platí bez ohledu na to, jestli PUT nebo POST byla použita k vytvoření indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-465">This is true regardless of whether PUT or POST was used to create the index.</span></span>

<span data-ttu-id="4fd4a-466">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-466">**Remarks**</span></span>

<span data-ttu-id="4fd4a-467">V současné době je omezená podpora aktualizace schématu indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-467">Currently, there is limited support for index schema updates.</span></span> <span data-ttu-id="4fd4a-468">Všechny aktualizace schémat, které by vyžadovaly přeindexování například změny typu polí nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-468">Any schema updates that would require re-indexing such as changing field types are not currently supported.</span></span> <span data-ttu-id="4fd4a-469">Ale existující pole nemůžete změnit ani odstranit, můžete přidat nová pole do stávajícího indexu kdykoli.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-469">Although existing fields cannot be changed or deleted, new fields can be added to an existing index at any time.</span></span> <span data-ttu-id="4fd4a-470">Při přidání nové pole, všechny stávající dokumenty v indexu bude mít automaticky pro toto pole hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-470">When a new field is added, all existing documents in the index will automatically have a null value for that field.</span></span> <span data-ttu-id="4fd4a-471">Žádné další úložiště budou využívat, dokud jsou přidána nových dokumentů do indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-471">No additional storage space will be consumed until new documents are added to the index.</span></span>

<a name="Suggesters"></a>

## <a name="suggesters"></a><span data-ttu-id="4fd4a-472">Moduly pro návrhy</span><span class="sxs-lookup"><span data-stu-id="4fd4a-472">Suggesters</span></span>
<span data-ttu-id="4fd4a-473">Návrhy funkce ve službě Azure Search je funkce našeptávání nebo automatické dokončování dotazů, které poskytuje seznam potenciální hledaný text v reakci na částečné řetězce vstupy do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-473">The suggestions feature in Azure Search is a type-ahead or auto-complete query capability, providing a list of potential search terms in response to partial string inputs entered into a search box.</span></span> <span data-ttu-id="4fd4a-474">Pravděpodobně jste zaznamenali dotazů při používání komerční vyhledávací stroje: zadáním ".NET" v Bing vytvoří seznam podmínek pro ".NET 4.5", "rozhraní .NET Framework 3,5", a tak dále.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-474">You've probably noticed query suggestions when using commercial web search engines: typing ".NET" in Bing produces a list of terms for ".NET 4.5", ".NET Framework 3.5", and so forth.</span></span> <span data-ttu-id="4fd4a-475">Pokud používáte službu vyhledávání REST API, implementace návrhy ve vlastní aplikaci Azure Search vyžaduje následující:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-475">When using the Search service REST API, implementing suggestions in a custom Azure Search application requires the following:</span></span>

* <span data-ttu-id="4fd4a-476">Povolit návrhy přidáním **modulu pro návrhy** konstrukce v indexu, poskytnutí názvu, režim hledání a seznam polí, pro kterou našeptávání je volána.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-476">Enable suggestions by adding a **suggester** construction in your index, giving the name, search mode, and a list of fields for which type-ahead is invoked.</span></span> <span data-ttu-id="4fd4a-477">Například pokud zadáte "mesto" jako zdrojové pole zadáte řetězec částečné vyhledávání "Sea" bude mít za následek "Seattle", "Pobřežního" a "Seatac" (všechny tři, jsou názvy skutečné města) jako dotazů nabízen uživateli.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-477">For example, if you specify "cityName" as a source field, typing a partial search string of "Sea" will result in "Seattle", "Seaside", and "Seatac" (all three are actual city names) offered up as query suggestions to the user.</span></span>
* <span data-ttu-id="4fd4a-478">Vyvolání návrhy voláním [API návrhy](#Suggestions) v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-478">Invoke suggestions by calling the [Suggestions API](#Suggestions) in your application code.</span></span> <span data-ttu-id="4fd4a-479">Částečné řetězce jsou obvykle při uživatele je zadáním vyhledávací dotaz a toto rozhraní API vrací sadu navrhované frází odešle do služby.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-479">Typically partial search strings are sent to the service while the user is typing a search query, and this API returns a set of suggested phrases.</span></span>

<span data-ttu-id="4fd4a-480">Tento článek vysvětluje, jak nakonfigurovat **modulu pro návrhy**.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-480">This article explains how to configure a **suggester**.</span></span> <span data-ttu-id="4fd4a-481">Také byste měli revidovat [návrhy API](#Suggestions) podrobnosti o tom, jak se používá modulu pro návrhy.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-481">You should also review the [Suggestions API](#Suggestions) for details on how a suggester is used.</span></span>

<span data-ttu-id="4fd4a-482">**Využití**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-482">**Usage**</span></span>

<span data-ttu-id="4fd4a-483">`Suggesters`jsou vytvořené v indexu a pracovní nejlépe, když se použije pro návrh konkrétní dokumenty místo přijít podmínky či fráze.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-483">`Suggesters` are created in the index and work best when used to suggest specific documents rather than loose terms or phrases.</span></span> <span data-ttu-id="4fd4a-484">Pole nejlepší candidate jsou produktů, názvů a jiné poměrně krátké věty, které můžete identifikovat položku.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-484">The best candidate fields are titles, names, and other relatively short phrases that can identify an item.</span></span> <span data-ttu-id="4fd4a-485">Méně účinné jsou opakovaných pole, kategorie a značky, nebo velmi dlouhé pole jako pole popisy nebo komentáře.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-485">Less effective are repetitive fields, such as categories and tags, or very long fields such as descriptions or comments fields.</span></span>

<span data-ttu-id="4fd4a-486">Jako součást definice indexu, můžete přidat jednoho modulu pro návrhy na `suggesters` kolekce.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-486">As part of the index definition, you can add a single suggester to the `suggesters` collection.</span></span> <span data-ttu-id="4fd4a-487">Vlastnosti, které definují modulu pro návrhy zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-487">Properties that define a suggester include the following:</span></span>

* <span data-ttu-id="4fd4a-488">`name`: Název modulu pro návrhy.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-488">`name`: The name of the suggester.</span></span> <span data-ttu-id="4fd4a-489">Při volání metody použijete název modulu pro návrhy `suggest` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-489">You use the name of the suggester when calling the `suggest` API.</span></span>
* <span data-ttu-id="4fd4a-490">`searchMode`: Strategie použitá k vyhledání kandidátských frází.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-490">`searchMode`: The strategy used to search for candidate phrases.</span></span> <span data-ttu-id="4fd4a-491">Jediný momentálně podporovaný režim je `analyzingInfixMatching`, který provádí flexibilní porovnávání frází na začátku nebo uprostřed vět.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-491">The only mode currently supported is `analyzingInfixMatching`, which performs flexible matching of phrases at the beginning or in the middle of sentences.</span></span>
* <span data-ttu-id="4fd4a-492">`sourceFields`: Seznam minimálně jedno pole, které jsou zdroj obsahu pro návrhy.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-492">`sourceFields`: A list of one or more fields that are the source of the content for suggestions.</span></span> <span data-ttu-id="4fd4a-493">Pouze pole typu `Edm.String` a `Collection(Edm.String)` může být zdrojů pro návrhy.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-493">Only fields of type `Edm.String` and `Collection(Edm.String)` may be sources for suggestions.</span></span> <span data-ttu-id="4fd4a-494">Lze použít pouze pole, které nemají vlastní analyzátor jazyka nastavit.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-494">Only fields that don't have a custom language analyzer set can be used.</span></span>

<span data-ttu-id="4fd4a-495">**Příklad modulu pro návrhy**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-495">**Suggester Example**</span></span>

<span data-ttu-id="4fd4a-496">Modulu pro návrhy je součástí indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-496">A suggester is part of the index.</span></span> <span data-ttu-id="4fd4a-497">V může existovat pouze jedna modulu pro návrhy `suggesters` kolekce v aktuální verzi, spolu s kolekci polí a `scoringProfiles`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-497">Only one suggester can exist in the `suggesters` collection in the current version, alongside the fields collection and `scoringProfiles`.</span></span>

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [!NOTE]
> <span data-ttu-id="4fd4a-498">Pokud jste použili verze verzi public preview služby Azure Search, `suggesters` nahrazuje starší logická vlastnost (`"suggestions": false`), předpona návrhy podporována pouze pro krátké řetězce (3-25 znaků).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-498">If you used the public preview version of Azure Search, `suggesters` replaces an older boolean property (`"suggestions": false`) that only supported prefix suggestions for short strings (3-25 characters).</span></span> <span data-ttu-id="4fd4a-499">Čím jsou nahrazené, `suggesters`, podporuje infix odpovídající, který vyhledá odpovídající podmínky na začátku nebo uprostřed pole obsahu, lepší toleranci chyb v řetězci pro vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-499">Its replacement, `suggesters`, supports infix matching that finds matching terms at the beginning or in the middle of field content, with better tolerance for mistakes in search strings.</span></span> <span data-ttu-id="4fd4a-500">Od verze všeobecně dostupná, to je nyní pouze implementace návrhy rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-500">Starting with the generally available release, this is now the only implementation of the suggestions API.</span></span> <span data-ttu-id="4fd4a-501">Starší `suggestions` vlastnost, která byla zavedena v `api-version=2014-07-31-Preview` dál v této verzi fungovat, ale není v provozu `2015-02-28` nebo novější verze Azure Search.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-501">The older `suggestions` property that was introduced in `api-version=2014-07-31-Preview` continues to work in that version, but is not operational in the `2015-02-28` or later versions of Azure Search.</span></span>
> 
> 

<a name="UpdateIndex"></a>

## <a name="update-index"></a><span data-ttu-id="4fd4a-502">Aktualizovat Index</span><span class="sxs-lookup"><span data-stu-id="4fd4a-502">Update Index</span></span>
<span data-ttu-id="4fd4a-503">Můžete aktualizovat existující index v rámci služby Azure Search pomocí požadavek HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-503">You can update an existing index within Azure Search using an HTTP PUT request.</span></span> <span data-ttu-id="4fd4a-504">Aktualizace můžou zahrnovat přidávání nové pole do stávajícího schématu, úprava možnosti CORS a úprava profily vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-504">Updates can include adding new fields to the existing schema, modifying CORS options, and modifying scoring profiles.</span></span> <span data-ttu-id="4fd4a-505">V tématu [přidat vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-505">See [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for more information.</span></span> <span data-ttu-id="4fd4a-506">Zadáte název indexu při aktualizaci v identifikátoru URI požadavku:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-506">You specify the name of the index to update on the request URI:</span></span>

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="4fd4a-507">**Důležité:** podpora pro aktualizace schématu indexu je omezena na operace, které nevyžadují nové sestavení indexu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-507">**Important:** Support for index schema updates is limited to operations that don't require rebuilding the search index.</span></span> <span data-ttu-id="4fd4a-508">Všechny aktualizace schémat, které by vyžadovaly přeindexování, například změny typu polí nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-508">Any schema updates that would require re-indexing, such as changing field types, are not currently supported.</span></span> <span data-ttu-id="4fd4a-509">Kdykoli můžete přidat nová pole, ale existující pole nemůžete změnit ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-509">New fields can be added at any time, although existing fields cannot be changed or deleted.</span></span> <span data-ttu-id="4fd4a-510">Totéž platí i pro `suggesters`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-510">The same applies to `suggesters`.</span></span> <span data-ttu-id="4fd4a-511">Nová pole, mohou být přidány do modulu pro návrhy v době jsou přidány pole, ale pole nelze odebrat z `suggesters` a existující pole nelze přidat do `suggesters`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-511">New fields may be added to a suggester at the time the fields are added, but fields cannot be removed from `suggesters` and existing fields cannot be added to `suggesters`.</span></span>

<span data-ttu-id="4fd4a-512">Při přidávání nové pole do indexu, všechny stávající dokumenty v indexu bude mít automaticky pro toto pole hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-512">When adding a new field to an index, all existing documents in the index will automatically have a null value for that field.</span></span> <span data-ttu-id="4fd4a-513">Žádné další úložiště budou využívat, dokud jsou přidána nových dokumentů do indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-513">No additional storage space will be consumed until new documents are added to the index.</span></span>

<span data-ttu-id="4fd4a-514">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-514">**Request**</span></span>

<span data-ttu-id="4fd4a-515">Je požadován pro všechny žádosti o služby protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-515">HTTPS is required for all service requests.</span></span> <span data-ttu-id="4fd4a-516">**Aktualizace indexu** požadavku je vytvořený pomocí HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-516">The **Update Index** request is constructed using HTTP PUT.</span></span> <span data-ttu-id="4fd4a-517">Pomocí PUT název indexu je součástí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-517">With PUT, the index name is part of the URL.</span></span> <span data-ttu-id="4fd4a-518">Pokud index neexistuje, vytvoří se.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-518">If the index doesn't exist, it is created.</span></span> <span data-ttu-id="4fd4a-519">Pokud už existuje index, se aktualizuje na novou definici.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-519">If the index already exists, it is updated to the new definition.</span></span>

<span data-ttu-id="4fd4a-520">Název indexu musí být malá písmena, začínat písmenem nebo číslicí, mít žádné lomítka nebo tečky a být kratší než 128 znaků.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-520">The index name must be lower case, start with a letter or number, have no slashes or dots, and be less than 128 characters.</span></span> <span data-ttu-id="4fd4a-521">Po spuštění název indexu s písmenem nebo číslicí, může obsahovat zbytek název jakékoli písmeno, čísla a pomlčky, dokud nejsou po sobě jdoucí pomlčky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-521">After starting the index name with a letter or number, the rest of the name can include any letter, number and dashes, as long as the dashes are not consecutive.</span></span>

<span data-ttu-id="4fd4a-522">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-522">`api-version=[string]` (required).</span></span> <span data-ttu-id="4fd4a-523">Verze preview je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-523">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="4fd4a-524">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-524">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="4fd4a-525">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-525">**Request Headers**</span></span>

<span data-ttu-id="4fd4a-526">Následující seznam popisuje hlavičky žádosti požadované a volitelné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-526">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="4fd4a-527">`Content-Type`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-527">`Content-Type`: Required.</span></span> <span data-ttu-id="4fd4a-528">Tuto možnost nastavíte na`application/json`</span><span class="sxs-lookup"><span data-stu-id="4fd4a-528">Set this to `application/json`</span></span>
* <span data-ttu-id="4fd4a-529">`api-key`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-529">`api-key`: Required.</span></span> <span data-ttu-id="4fd4a-530">`api-key` Se používá k ověření požadavku na vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-530">The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="4fd4a-531">Je řetězcovou hodnotu, jedinečné pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-531">It is a string value, unique to your service.</span></span> <span data-ttu-id="4fd4a-532">**Aktualizace indexu** musí zahrnovat požadavek `api-key` záhlaví nastavit klíče správce (na rozdíl od klíč dotazů).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-532">The **Update Index** request must include an `api-key` header set to your admin key (as opposed to a query key).</span></span>

<span data-ttu-id="4fd4a-533">Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-533">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="4fd4a-534">Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-534">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="4fd4a-535">V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-535">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="4fd4a-536">**Syntaxe požadavku textu**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-536">**Request Body Syntax**</span></span>

<span data-ttu-id="4fd4a-537">Při aktualizaci existujícího indexu se text musí obsahovat původní definici schématu, a nová pole, který chcete přidat, jakož i upravené vyhodnocování profily, trochu a možnosti CORS, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-537">When updating an existing index, the body must include the original schema definition, plus the new fields you are adding, as well as the modified scoring profiles, suggesters and CORS options, if any.</span></span> <span data-ttu-id="4fd4a-538">Pokud nejsou změny možnosti vyhodnocování profily a CORS, je nutné zahrnout původní z vytvoření indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-538">If you are not modifying the scoring profiles and CORS options, you must include the originals from when the index was created.</span></span> <span data-ttu-id="4fd4a-539">Obecné doporučené vzor pro aktualizace je načíst definici indexu s GET, upravte ho a potom jej aktualizovat s PUT.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-539">In general the best pattern to use for updates is to retrieve the index definition with a GET, modify it, then update it with PUT.</span></span>

<span data-ttu-id="4fd4a-540">Syntaxe schématu použít k vytvoření indexu je zde uveden ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-540">The schema syntax used to create an index is reproduced here for convenience.</span></span> <span data-ttu-id="4fd4a-541">V tématu [Create Index](#CreateIndex) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-541">See [Create Index](#CreateIndex) for more details.</span></span>

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


<span data-ttu-id="4fd4a-542">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-542">**Response**</span></span>

<span data-ttu-id="4fd4a-543">Pro úspěšné žádosti: "204 žádný obsah".</span><span class="sxs-lookup"><span data-stu-id="4fd4a-543">For a successful request: "204 No Content".</span></span>

<span data-ttu-id="4fd4a-544">Ve výchozím nastavení bude prázdný text odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-544">By default the response body will be empty.</span></span> <span data-ttu-id="4fd4a-545">Ale pokud `Prefer` hlavička požadavku je nastaven na `return=representation`, text odpovědi bude obsahovat JSON pro definici indexu, která byla aktualizována.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-545">However, if the `Prefer` request header is set to `return=representation`, the response body will contain the JSON for the index definition that was updated.</span></span> <span data-ttu-id="4fd4a-546">V takovém případě bude kód stavu úspěch "200 OK".</span><span class="sxs-lookup"><span data-stu-id="4fd4a-546">In this case, the success status code will be "200 OK".</span></span>

<span data-ttu-id="4fd4a-547">**Aktualizace definice indexu s vlastní analyzátory**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-547">**Updating index definition with custom analyzers**</span></span>

<span data-ttu-id="4fd4a-548">Jakmile je definovány analyzátor, tokenizátor, token filtru nebo filtr char, nemůže být upraven.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-548">Once an analyzer, a tokenizer, a token filter or a char filter is defined, it cannot be modified.</span></span> <span data-ttu-id="4fd4a-549">Nové lze přidat do existujícího indexu, pouze pokud `allowIndexDowntime` je příznak nastaven na hodnotu true v indexu žádost o aktualizaci:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-549">New ones can be added to an existing index only if the `allowIndexDowntime` flag is set to true in the index update request:</span></span> 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

<span data-ttu-id="4fd4a-550">Všimněte si, že tato operace bude put indexu offline pro alespoň několik sekund, způsobuje vaší indexování a požadavků na dotazy k selhání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-550">Note that this operation will put your index offline for at least a few seconds, causing your indexing and query requests to fail.</span></span> <span data-ttu-id="4fd4a-551">Výkon a zápisu dostupnost index může být zasažená několik minut po aktualizaci indexu nebo delší dobu velmi velký indexy.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-551">Performance and write availability of the index can be impaired for several minutes after the index is updated, or longer for very large indexes.</span></span>

<a name="ListIndexes"></a>

## <a name="list-indexes"></a><span data-ttu-id="4fd4a-552">Seznam indexů</span><span class="sxs-lookup"><span data-stu-id="4fd4a-552">List Indexes</span></span>
<span data-ttu-id="4fd4a-553">**Seznamu indexy** operace vrátí seznam hodnot indexy aktuálně ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-553">The **List Indexes** operation returns a list of the indexes currently in your Azure Search service.</span></span>

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="4fd4a-554">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-554">**Request**</span></span>

<span data-ttu-id="4fd4a-555">Je požadován pro všechny žádosti o služby protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-555">HTTPS is required for all service requests.</span></span> <span data-ttu-id="4fd4a-556">**Seznamu indexy** požadavek se dá vytvořit pomocí metody GET.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-556">The **List Indexes** request can be constructed using the GET method.</span></span>

<span data-ttu-id="4fd4a-557">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-557">`api-version=[string]` (required).</span></span> <span data-ttu-id="4fd4a-558">Verze preview je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-558">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="4fd4a-559">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-559">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="4fd4a-560">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-560">**Request Headers**</span></span>

<span data-ttu-id="4fd4a-561">Následující seznam popisuje hlavičky žádosti požadované a volitelné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-561">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="4fd4a-562">`api-key`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-562">`api-key`: Required.</span></span> <span data-ttu-id="4fd4a-563">`api-key` Se používá k ověření požadavku na vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-563">The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="4fd4a-564">Je řetězcovou hodnotu, jedinečné pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-564">It is a string value, unique to your service.</span></span> <span data-ttu-id="4fd4a-565">**Seznamu indexy** musí zahrnovat požadavek `api-key` nastavit na klíč správce (na rozdíl od klíč dotazů).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-565">The **List Indexes** request must include an `api-key` set to an admin key (as opposed to a query key).</span></span>

<span data-ttu-id="4fd4a-566">Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-566">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="4fd4a-567">Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-567">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="4fd4a-568">V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-568">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="4fd4a-569">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-569">**Request Body**</span></span>

<span data-ttu-id="4fd4a-570">Žádné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-570">None.</span></span>

<span data-ttu-id="4fd4a-571">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-571">**Response**</span></span>

<span data-ttu-id="4fd4a-572">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-572">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="4fd4a-573">Tady je odpovědi na příkladu:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-573">Here is an example response body:</span></span>

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

<span data-ttu-id="4fd4a-574">Všimněte si, že můžete filtrovat odpovědi dolů pouze vlastnosti, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-574">Note that you can filter the response down to just the properties you're interested in.</span></span> <span data-ttu-id="4fd4a-575">Například pokud chcete pouze seznam názvů index, použijte prostředí OData `$select` dotazu možnost:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-575">For example, if you want only a list of index names, use the OData `$select` query option:</span></span>

    GET /indexes?api-version=2015-02-28-Preview&$select=name

<span data-ttu-id="4fd4a-576">V takovém případě odpověď z výše uvedeném příkladu by měly vypadat následovně:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-576">In this case, the response from the above example would appear as follows:</span></span>

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

<span data-ttu-id="4fd4a-577">To je užitečné pro ušetří šířku pásma, pokud máte spoustu indexy ve vyhledávací službě.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-577">This is a useful technique to save bandwidth if you have a lot of indexes in your Search service.</span></span>

<a name="GetIndex"></a>

## <a name="get-index"></a><span data-ttu-id="4fd4a-578">Získat Index</span><span class="sxs-lookup"><span data-stu-id="4fd4a-578">Get Index</span></span>
<span data-ttu-id="4fd4a-579">**Získat Index** operaci získá definici indexu z Azure Search.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-579">The **Get Index** operation gets the index definition from Azure Search.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="4fd4a-580">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-580">**Request**</span></span>

<span data-ttu-id="4fd4a-581">Protokol HTTPS je vyžadována pro žádosti o služby.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-581">HTTPS is required for service requests.</span></span> <span data-ttu-id="4fd4a-582">**Získat Index** požadavek se dá vytvořit pomocí metody GET.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-582">The **Get Index** request can be constructed using the GET method.</span></span>

<span data-ttu-id="4fd4a-583">[Název indexu] v identifikátoru URI požadavku určuje index, který mají být vráceny od kolekce indexů.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-583">The [index name] in the request URI specifies which index to return from the indexes collection.</span></span>

<span data-ttu-id="4fd4a-584">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-584">`api-version=[string]` (required).</span></span> <span data-ttu-id="4fd4a-585">Verze preview je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-585">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="4fd4a-586">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-586">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="4fd4a-587">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-587">**Request Headers**</span></span>

<span data-ttu-id="4fd4a-588">Následující seznam popisuje hlavičky žádosti požadované a volitelné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-588">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="4fd4a-589">`api-key`: `api-key` Se používá k ověření požadavku na vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-589">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="4fd4a-590">Je řetězcovou hodnotu, jedinečné pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-590">It is a string value, unique to your service.</span></span> <span data-ttu-id="4fd4a-591">**Získat Index** musí zahrnovat požadavek `api-key` nastavit na klíč správce (na rozdíl od klíč dotazů).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-591">The **Get Index** request must include an `api-key` set to an admin key (as opposed to a query key).</span></span>

<span data-ttu-id="4fd4a-592">Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-592">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="4fd4a-593">Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-593">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="4fd4a-594">V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-594">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="4fd4a-595">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-595">**Request Body**</span></span>

<span data-ttu-id="4fd4a-596">Žádné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-596">None.</span></span>

<span data-ttu-id="4fd4a-597">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-597">**Response**</span></span>

<span data-ttu-id="4fd4a-598">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-598">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="4fd4a-599">Podívejte se na příklad JSON v [vytvářet a aktualizovat Index](#CreateUpdateIndexExample) příklad datové části odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-599">See the example JSON in [Creating and Updating an Index](#CreateUpdateIndexExample) for an example of the response payload.</span></span>

<a name="DeleteIndex"></a>

## <a name="delete-index"></a><span data-ttu-id="4fd4a-600">Odstranit Index</span><span class="sxs-lookup"><span data-stu-id="4fd4a-600">Delete Index</span></span>
<span data-ttu-id="4fd4a-601">**Odstranit Index** operace odebere index a související dokumenty ze služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-601">The **Delete Index** operation removes an index and associated documents from your Azure Search service.</span></span> <span data-ttu-id="4fd4a-602">Název indexu můžete získat z řídicího panelu služby na portálu Azure nebo z rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-602">You can get the index name from the service dashboard in the Azure portal, or from the API.</span></span> <span data-ttu-id="4fd4a-603">V tématu [seznamu indexy](#ListIndexes) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-603">See [List Indexes](#ListIndexes) for details.</span></span>

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="4fd4a-604">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-604">**Request**</span></span>

<span data-ttu-id="4fd4a-605">Protokol HTTPS je vyžadována pro žádosti o služby.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-605">HTTPS is required for service requests.</span></span> <span data-ttu-id="4fd4a-606">**Odstranit Index** požadavek se dá vytvořit pomocí metodu DELETE.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-606">The **Delete Index** request can be constructed using the DELETE method.</span></span>

<span data-ttu-id="4fd4a-607">[Název indexu] v identifikátoru URI požadavku určuje index, který chcete odstranit z kolekce indexů.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-607">The [index name] in the request URI specifies which index to delete from the indexes collection.</span></span>

<span data-ttu-id="4fd4a-608">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-608">`api-version=[string]` (required).</span></span> <span data-ttu-id="4fd4a-609">Verze preview je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-609">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="4fd4a-610">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-610">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="4fd4a-611">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-611">**Request Headers**</span></span>

<span data-ttu-id="4fd4a-612">Následující seznam popisuje hlavičky žádosti požadované a volitelné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-612">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="4fd4a-613">`api-key`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-613">`api-key`: Required.</span></span> <span data-ttu-id="4fd4a-614">`api-key` Se používá k ověření požadavku na vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-614">The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="4fd4a-615">Je hodnotu řetězce pro adresu URL služby jedinečné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-615">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="4fd4a-616">**Odstranit Index** musí zahrnovat požadavek `api-key` záhlaví nastavit klíče správce (na rozdíl od klíč dotazů).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-616">The **Delete Index** request must include an `api-key` header set to your admin key (as opposed to a query key).</span></span>

<span data-ttu-id="4fd4a-617">Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-617">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="4fd4a-618">Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-618">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="4fd4a-619">V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-619">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="4fd4a-620">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-620">**Request Body**</span></span>

<span data-ttu-id="4fd4a-621">Žádné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-621">None.</span></span>

<span data-ttu-id="4fd4a-622">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-622">**Response**</span></span>

<span data-ttu-id="4fd4a-623">: Stav 204 ne obsahu je vrácen kód pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-623">Status Code: 204 No Content is returned for a successful response.</span></span>

<a name="GetIndexStats"></a>

## <a name="get-index-statistics"></a><span data-ttu-id="4fd4a-624">Získat statistiku indexu</span><span class="sxs-lookup"><span data-stu-id="4fd4a-624">Get Index Statistics</span></span>
<span data-ttu-id="4fd4a-625">**Získat statistiku Index** operace z Azure Search vrátí počet dokumentů pro aktuální index a využití úložiště.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-625">The **Get Index Statistics** operation returns from Azure Search a document count for the current index, plus storage usage.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> <span data-ttu-id="4fd4a-626">Statistika na dokument počet a velikost úložiště se shromažďují každých několik minut, ne v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-626">Statistics on document count and storage size are collected every few minutes, not in real time.</span></span> <span data-ttu-id="4fd4a-627">Statistiky vrácený toto rozhraní API proto nemusí odrážet změny způsobené poslední operace indexování.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-627">Therefore, the statistics returned by this API may not reflect changes caused by recent indexing operations.</span></span>
> 
> 

<span data-ttu-id="4fd4a-628">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-628">**Request**</span></span>

<span data-ttu-id="4fd4a-629">Je požadován pro všechny požadavky služby protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-629">HTTPS is required for all services requests.</span></span> <span data-ttu-id="4fd4a-630">**Získat statistiku Index** požadavek se dá vytvořit pomocí metody GET.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-630">The **Get Index Statistics** request can be constructed using the GET method.</span></span>

<span data-ttu-id="4fd4a-631">[Název indexu] v identifikátoru URI požadavku informuje službu, kterou chcete vrátit index statistiky pro zadaný index.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-631">The [index name] in the request URI tells the service to return index statistics for the specified index.</span></span>

<span data-ttu-id="4fd4a-632">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-632">`api-version=[string]` (required).</span></span> <span data-ttu-id="4fd4a-633">Verze preview je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-633">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="4fd4a-634">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-634">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="4fd4a-635">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-635">**Request Headers**</span></span>

<span data-ttu-id="4fd4a-636">Následující seznam popisuje hlavičky žádosti požadované a volitelné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-636">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="4fd4a-637">`api-key`: `api-key` Se používá k ověření požadavku na vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-637">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="4fd4a-638">Je řetězcovou hodnotu, jedinečné pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-638">It is a string value, unique to your service.</span></span> <span data-ttu-id="4fd4a-639">**Získat statistiku Index** musí zahrnovat požadavek `api-key` nastavit na klíč správce (na rozdíl od klíč dotazů).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-639">The **Get Index Statistics** request must include an `api-key` set to an admin key (as opposed to a query key).</span></span>

<span data-ttu-id="4fd4a-640">Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-640">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="4fd4a-641">Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-641">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="4fd4a-642">V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-642">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="4fd4a-643">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-643">**Request Body**</span></span>

<span data-ttu-id="4fd4a-644">Žádné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-644">None.</span></span>

<span data-ttu-id="4fd4a-645">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-645">**Response**</span></span>

<span data-ttu-id="4fd4a-646">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-646">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="4fd4a-647">Text odpovědi je v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-647">The response body is in the following format:</span></span>

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>

## <a name="test-analyzer"></a><span data-ttu-id="4fd4a-648">Analyzátor testu</span><span class="sxs-lookup"><span data-stu-id="4fd4a-648">Test Analyzer</span></span>
<span data-ttu-id="4fd4a-649">**Analyzovat rozhraní API** ukazuje, jak analyzátor dělí text do tokenů.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-649">The **Analyze API** shows how an analyzer breaks text into tokens.</span></span>

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="4fd4a-650">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-650">**Request**</span></span>

<span data-ttu-id="4fd4a-651">Je požadován pro všechny požadavky služby protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-651">HTTPS is required for all services requests.</span></span> <span data-ttu-id="4fd4a-652">**Analyzovat rozhraní API** požadavek se dá vytvořit pomocí metody POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-652">The **Analyze API** request can be constructed using the POST method.</span></span>

<span data-ttu-id="4fd4a-653">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-653">`api-version=[string]` (required).</span></span> <span data-ttu-id="4fd4a-654">Verze preview je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-654">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="4fd4a-655">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-655">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="4fd4a-656">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-656">**Request Headers**</span></span>

<span data-ttu-id="4fd4a-657">Následující seznam popisuje hlavičky žádosti požadované a volitelné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-657">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="4fd4a-658">`api-key`: `api-key` Se používá k ověření požadavku na vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-658">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="4fd4a-659">Je řetězcovou hodnotu, jedinečné pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-659">It is a string value, unique to your service.</span></span> <span data-ttu-id="4fd4a-660">**Analyzovat rozhraní API** musí zahrnovat požadavek `api-key` nastavit na klíč správce (na rozdíl od klíč dotazů).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-660">The **Analyze API** request must include an `api-key` set to an admin key (as opposed to a query key).</span></span>

<span data-ttu-id="4fd4a-661">Budete také potřebovat název indexu a název služby můžete vytvořit adresu URL požadavku.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-661">You will also need the index name and the service name to construct the request URL.</span></span> <span data-ttu-id="4fd4a-662">Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-662">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="4fd4a-663">V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-663">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="4fd4a-664">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-664">**Request Body**</span></span>

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

<span data-ttu-id="4fd4a-665">nebo</span><span class="sxs-lookup"><span data-stu-id="4fd4a-665">or</span></span>

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

<span data-ttu-id="4fd4a-666">`analyzer_name`, `tokenizer_name`, `token_filter_name` a `char_filter_name` musí být platné názvy předdefinované nebo vlastní analyzátorů, tokenizers, token filtry a filtry znak pro index.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-666">The `analyzer_name`, `tokenizer_name`, `token_filter_name` and `char_filter_name` need to be valid names of predefined or custom analyzers, tokenizers, token filters and char filters for the index.</span></span> <span data-ttu-id="4fd4a-667">Další informace o procesu analýzy lexikální najdete [Analysis ve službě Azure Search](https://aka.ms/azsanalysis).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-667">To learn more about the process of lexical analysis see [Analysis in Azure Search](https://aka.ms/azsanalysis).</span></span>

<span data-ttu-id="4fd4a-668">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-668">**Response**</span></span>

<span data-ttu-id="4fd4a-669">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-669">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="4fd4a-670">Text odpovědi je v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-670">The response body is in the following format:</span></span>

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

<span data-ttu-id="4fd4a-671">**Analýza příklad rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-671">**Analyze API example**</span></span>

<span data-ttu-id="4fd4a-672">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-672">**Request**</span></span>

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

<span data-ttu-id="4fd4a-673">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-673">**Response**</span></span>

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

- - -
<a name="DocOps"></a>

## <a name="document-operations"></a><span data-ttu-id="4fd4a-674">Operace dokumentu</span><span class="sxs-lookup"><span data-stu-id="4fd4a-674">Document Operations</span></span>
<span data-ttu-id="4fd4a-675">Ve službě Azure Search index je uložená v cloudu a vyplněny dokumentů JSON, které odešlete do této služby.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-675">In Azure Search, an index is stored in the cloud and populated using JSON documents that you upload to the service.</span></span> <span data-ttu-id="4fd4a-676">Všechny dokumenty, které můžete nahrávat na server tvoří souhrnu dat hledání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-676">All the documents that you upload comprise the corpus of your search data.</span></span> <span data-ttu-id="4fd4a-677">Dokumenty obsahují pole, z nichž některé jsou tokenizovaného do hledaných termínů jako jejich uložení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-677">Documents contain fields, some of which are tokenized into search terms as they are uploaded.</span></span> <span data-ttu-id="4fd4a-678">`/docs` Segment adresy URL v rozhraní API služby Azure Search představuje kolekci dokumentů v indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-678">The `/docs` URL segment in the Azure Search API represents the collection of documents in an index.</span></span> <span data-ttu-id="4fd4a-679">Všechny operace provést na kolekci, například odeslání, sloučení, odstranění nebo dotazování dokumentů proveďte umístit v rámci jedné index, proto adresy URL pro tyto operace bude vždy začínat `/indexes/[index name]/docs` pro název daného indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-679">All operations performed on the collection such as uploading, merging, deleting, or querying documents take place in the context of a single index, so the URLs for these operations will always start with `/indexes/[index name]/docs` for a given index name.</span></span>

<span data-ttu-id="4fd4a-680">Kód aplikace musíte vygenerovat buď dokumentů JSON nahrát do služby Azure Search, nebo můžete použít [indexer](https://msdn.microsoft.com/library/dn946891.aspx) načíst dokumenty, pokud je zdroj dat databázi SQL Azure nebo Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-680">Your application code must either generate JSON documents to upload to Azure Search or you can use an [indexer](https://msdn.microsoft.com/library/dn946891.aspx) to load documents if the data source is either Azure SQL Database or Azure Cosmos DB.</span></span> <span data-ttu-id="4fd4a-681">Obvykle indexy budou naplněny z jedné datové sady, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-681">Typically, indexes are populated from a single dataset that you provide.</span></span>

<span data-ttu-id="4fd4a-682">Měli byste mít jeden dokument pro každou položku, kterou chcete vyhledat.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-682">You should plan on having one document for each item that you want to search.</span></span> <span data-ttu-id="4fd4a-683">Film pronájem aplikace může mít jeden dokument na film, storefront aplikace může mít jeden dokument za SKU, online výukových kurzů aplikace může mít jeden dokument za kurzu, firma research může mít jeden dokument pro každý academic dokumentu v jejich úložiště a tak dále.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-683">A movie rental application might have one document per movie, a storefront application might have one document per SKU, an online courseware application might have one document per course, a research firm might have one document for each academic paper in their repository, and so on.</span></span>

<span data-ttu-id="4fd4a-684">Dokumenty obsahovat jeden či více polí.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-684">Documents consist of one or more fields.</span></span> <span data-ttu-id="4fd4a-685">Pole může obsahovat text, který je tokenizovaného do podmínek vyhledávání ve službě Azure Search, jakož i bez tokenizovaného nebo jiné než textové hodnoty, které mohou být používány filtry, nebo profily vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-685">Fields can contain text that is tokenized by Azure Search into search terms, as well as non-tokenized or non-text values that can be used in filters or scoring profiles.</span></span> <span data-ttu-id="4fd4a-686">Názvy, datové typy a funkce vyhledávání, které jsou podporovány pro každé pole určuje schéma indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-686">The names, data types, and search features supported for each field are determined by the index schema.</span></span> <span data-ttu-id="4fd4a-687">Jedno z polí ve schématu každý index musí být určeny jako ID a každý dokument musí mít hodnotu v poli ID, která jednoznačně identifikuje tento dokument v indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-687">One of the fields in each index schema must be designated as an ID, and each document must have a value for the ID field that uniquely identifies that document in the index.</span></span> <span data-ttu-id="4fd4a-688">Všechna ostatní pole dokumentu jsou volitelné a použije výchozí hodnotu null, pokud nezadaný.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-688">All other document fields are optional and will default to a null value if left unspecified.</span></span> <span data-ttu-id="4fd4a-689">Všimněte si, že hodnoty null nemusí provádět žádné místo v indexu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-689">Note that null values do not take up space in the search index.</span></span>

<span data-ttu-id="4fd4a-690">Předtím, než můžete odesílat dokumenty, musí již jste vytvořili index ve službě.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-690">Before you can upload documents, you must have already created the index on the service.</span></span> <span data-ttu-id="4fd4a-691">V tématu [Create Index](#CreateIndex) podrobnosti o tento první krok.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-691">See [Create Index](#CreateIndex) for details about this first step.</span></span>

<a name="AddOrUpdateDocuments"></a>

## <a name="add-update-or-delete-documents"></a><span data-ttu-id="4fd4a-692">Přidání, aktualizace nebo odstranění dokumentů</span><span class="sxs-lookup"><span data-stu-id="4fd4a-692">Add, Update, or Delete Documents</span></span>
<span data-ttu-id="4fd4a-693">Můžete nahrát, sloučení, sloučení nebo odeslání nebo odstranění dokumentů ze zadaného indexu pomocí HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-693">You can upload, merge, merge-or-upload or delete documents from a specified index using HTTP POST.</span></span> <span data-ttu-id="4fd4a-694">Pro velké množství aktualizací se doporučuje dávkování dokumentů až (1000 dokumentů na jednu dávku) nebo o 16 MB na jednu dávku.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-694">For large numbers of updates, batching of documents (up to 1000 documents per batch or about 16 MB per batch) is recommended.</span></span>

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="4fd4a-695">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-695">**Request**</span></span>

<span data-ttu-id="4fd4a-696">Je požadován pro všechny žádosti o služby protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-696">HTTPS is required for all service requests.</span></span> <span data-ttu-id="4fd4a-697">Můžete nahrát, sloučení, sloučení nebo odeslání nebo odstranění dokumentů ze zadaného indexu pomocí HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-697">You can upload, merge, merge-or-upload or delete documents from a specified index using HTTP POST.</span></span>

<span data-ttu-id="4fd4a-698">[Název indexu] obsahuje identifikátoru URI požadavku zadání index, který o odeslání dokumentů.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-698">The request URI includes [index name], specifying which index to post documents.</span></span> <span data-ttu-id="4fd4a-699">Najednou můžete pouze odeslání dokumentů pro jeden index.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-699">You can only post documents to one index at a time.</span></span>

<span data-ttu-id="4fd4a-700">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-700">`api-version=[string]` (required).</span></span> <span data-ttu-id="4fd4a-701">Verze preview je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-701">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="4fd4a-702">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-702">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="4fd4a-703">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-703">**Request Headers**</span></span>

<span data-ttu-id="4fd4a-704">Následující seznam popisuje hlavičky žádosti požadované a volitelné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-704">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="4fd4a-705">`Content-Type`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-705">`Content-Type`: Required.</span></span> <span data-ttu-id="4fd4a-706">Tuto možnost nastavíte na`application/json`</span><span class="sxs-lookup"><span data-stu-id="4fd4a-706">Set this to `application/json`</span></span>
* <span data-ttu-id="4fd4a-707">`api-key`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-707">`api-key`: Required.</span></span> <span data-ttu-id="4fd4a-708">`api-key` Se používá k ověření požadavku na vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-708">The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="4fd4a-709">Je řetězcovou hodnotu, jedinečné pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-709">It is a string value, unique to your service.</span></span> <span data-ttu-id="4fd4a-710">**Přidat dokumenty** musí zahrnovat požadavek `api-key` záhlaví nastavit klíče správce (na rozdíl od klíč dotazů).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-710">The **Add Documents** request must include an `api-key` header set to your admin key (as opposed to a query key).</span></span>

<span data-ttu-id="4fd4a-711">Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-711">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="4fd4a-712">Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-712">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="4fd4a-713">V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-713">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="4fd4a-714">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-714">**Request Body**</span></span>

<span data-ttu-id="4fd4a-715">Text žádosti obsahuje jeden nebo více dokumentů indexovaných.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-715">The body of the request contains one or more documents to be indexed.</span></span> <span data-ttu-id="4fd4a-716">Dokumenty jsou identifikovány jedinečný klíč.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-716">Documents are identified by a unique key.</span></span> <span data-ttu-id="4fd4a-717">Každý dokument je přidružené akci: odeslání, sloučení, mergeOrUpload nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-717">Each document is associated with an action: upload, merge, mergeOrUpload or delete.</span></span> <span data-ttu-id="4fd4a-718">Žádostí o nahrání musí obsahovat data dokumentu jako sada párů klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-718">Upload requests must include the document data as a set of key/value pairs.</span></span>

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [!NOTE]
> <span data-ttu-id="4fd4a-719">Dokument klíče může obsahovat pouze písmena, číslice, pomlčky ("-"), podtržítka ("_") a symboly rovná se ("=").</span><span class="sxs-lookup"><span data-stu-id="4fd4a-719">Document keys can only contain letters, numbers, dashes ("-"), underscores ("_"), and equal signs ("=").</span></span> <span data-ttu-id="4fd4a-720">Další podrobnosti najdete v tématu [pravidla po pojmenování](https://msdn.microsoft.com/library/azure/dn857353.aspx).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-720">For more details, see [Naming rules](https://msdn.microsoft.com/library/azure/dn857353.aspx).</span></span>
> 
> 

<span data-ttu-id="4fd4a-721">**Akce dokumentu**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-721">**Document Actions**</span></span>

* <span data-ttu-id="4fd4a-722">`upload`: Nahrávání akce je podobná "upsert", kde bude dokument vložený, pokud je nový a aktualizovaný nebo nahrazený, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-722">`upload`: An upload action is similar to an "upsert" where the document will be inserted if it is new and updated/replaced if it exists.</span></span> <span data-ttu-id="4fd4a-723">Všimněte si, že všechna pole jsou nahrazena v případě, že aktualizace.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-723">Note that all fields are replaced in the update case.</span></span>
* <span data-ttu-id="4fd4a-724">`merge`: Sloučení aktualizuje stávající dokument se zadanými poli.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-724">`merge`: Merge updates an existing document with the specified fields.</span></span> <span data-ttu-id="4fd4a-725">Pokud dokument neexistuje, sloučení selže.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-725">If the document doesn't exist, the merge will fail.</span></span> <span data-ttu-id="4fd4a-726">Každé pole zadané ve sloučení nahradí stávající pole v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-726">Any field you specify in a merge will replace the existing field in the document.</span></span> <span data-ttu-id="4fd4a-727">To zahrnuje i pole typu `Collection(Edm.String)`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-727">This includes fields of type `Collection(Edm.String)`.</span></span> <span data-ttu-id="4fd4a-728">Například pokud dokument obsahuje pole "značky" s hodnotou `["budget"]` a vy spustíte sloučení s hodnotou `["economy", "pool"]` pro "značky", bude konečná hodnota pole "značky" `["economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-728">For example, if the document contains a field "tags" with value `["budget"]` and you execute a merge with value `["economy", "pool"]` for "tags", the final value of the "tags" field will be `["economy", "pool"]`.</span></span> <span data-ttu-id="4fd4a-729">Zruší **není** být `["budget", "economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-729">It will **not** be `["budget", "economy", "pool"]`.</span></span>
* <span data-ttu-id="4fd4a-730">`mergeOrUpload`: chová jako `merge` Pokud již dokument s daným klíčem v indexu existuje.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-730">`mergeOrUpload`: behaves like `merge` if a document with the given key already exists in the index.</span></span> <span data-ttu-id="4fd4a-731">Pokud dokument neexistuje, chová se jako `upload` s novým dokumentem.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-731">If the document does not exist it behaves like `upload` with a new document.</span></span>
* <span data-ttu-id="4fd4a-732">`delete`: Odstranění odebere z indexu zadaný dokument.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-732">`delete`: Delete removes the specified document from the index.</span></span> <span data-ttu-id="4fd4a-733">Všimněte si, že všechna zadaná pole v `delete` operaci kromě pole klíče budou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-733">Note that any fields you specify in a `delete` operation other than the key field will be ignored.</span></span> <span data-ttu-id="4fd4a-734">Pokud chcete odebrat z dokumentu jednotlivá pole, použijte `merge` místo a jednoduše nastavte hodnotu pole explicitně na `null`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-734">If you want to remove an individual field from a document, use `merge` instead and simply set the field explicitly to `null`.</span></span>

<span data-ttu-id="4fd4a-735">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-735">**Response**</span></span>

<span data-ttu-id="4fd4a-736">Pro úspěšné odpovědi, což znamená, že všechny položky byly úspěšně indexované je vrátit stavovým kódem 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-736">Status code 200 (OK) is returned for a successful response, meaning that all items have been successfully indexed.</span></span> <span data-ttu-id="4fd4a-737">To vyplývá z `status` vlastnost je nastavená na hodnotu true pro všechny položky, stejně jako `statusCode` nastavenou na hodnotu 201 (pro nově nahraném dokumenty) nebo 200 (pro sloučené nebo odstraněné dokumenty):</span><span class="sxs-lookup"><span data-stu-id="4fd4a-737">This is indicated by the `status` property being set to true for all items, as well as the `statusCode` property being set to either 201 (for newly uploaded documents) or 200 (for merged or deleted documents):</span></span>

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

<span data-ttu-id="4fd4a-738">Stavový kód 207 (více Status) je vrácena, pokud nebyla alespoň jedna položka úspěšně indexovaná.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-738">Status code 207 (Multi-Status) is returned when at least one item was not successfully indexed.</span></span> <span data-ttu-id="4fd4a-739">Položky, které nebyly indexovány mají `status` pole nastaveno na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-739">Items that have not been indexed have the `status` field set to false.</span></span> <span data-ttu-id="4fd4a-740">`errorMessage` a `statusCode` vlastnosti označí důvod pro indexování chybu:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-740">The `errorMessage` and `statusCode` properties will indicate the reason for the indexing error:</span></span>

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

<span data-ttu-id="4fd4a-741">Následující tabulka vysvětluje různé každý dokument stavové kódy, které mohou být vráceny v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-741">The following table explains the various per-document status codes that can be returned in the response.</span></span> <span data-ttu-id="4fd4a-742">Mějte na paměti některé naznačují potíže s na žádost, zatímco ostatní označují dočasné chybový stav.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-742">Note that some indicate problems with the request itself, while others indicate temporary error conditions.</span></span> <span data-ttu-id="4fd4a-743">K tomu, které by měl opakovat po prodlevě.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-743">The latter you should retry after a delay.</span></span>

<table style="font-size:12">
    <tr>
        <th><span data-ttu-id="4fd4a-744">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="4fd4a-744">Status code</span></span></th>
        <th><span data-ttu-id="4fd4a-745">Význam</span><span class="sxs-lookup"><span data-stu-id="4fd4a-745">Meaning</span></span></th>
        <th><span data-ttu-id="4fd4a-746">Opakovatelná</span><span class="sxs-lookup"><span data-stu-id="4fd4a-746">Retryable</span></span></th>
        <th><span data-ttu-id="4fd4a-747">Poznámky</span><span class="sxs-lookup"><span data-stu-id="4fd4a-747">Notes</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-748">200</span><span class="sxs-lookup"><span data-stu-id="4fd4a-748">200</span></span></td>
        <td><span data-ttu-id="4fd4a-749">Dokument byl úspěšně upravit nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-749">Document was successfully modified or deleted.</span></span></td>
        <td><span data-ttu-id="4fd4a-750">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="4fd4a-750">n/a</span></span></td>
        <td><span data-ttu-id="4fd4a-751">Operace odstranění se <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-751">Delete operations are <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span></span> <span data-ttu-id="4fd4a-752">To znamená i v případě, že klíč dokumentu v indexu neexistuje, pokusu o operaci odstranění s tímto klíčem bude mít za následek 200 stavový kód.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-752">That is, even if a document key does not exist in the index, attempting a delete operation with that key will result in a 200 status code.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-753">201</span><span class="sxs-lookup"><span data-stu-id="4fd4a-753">201</span></span></td>
        <td><span data-ttu-id="4fd4a-754">Dokument byl úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-754">Document was successfully created.</span></span></td>
        <td><span data-ttu-id="4fd4a-755">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="4fd4a-755">n/a</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-756">400</span><span class="sxs-lookup"><span data-stu-id="4fd4a-756">400</span></span></td>
        <td><span data-ttu-id="4fd4a-757">V dokumentu, který brání probíhá indexování došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-757">There was an error in the document that prevented it from being indexed.</span></span></td>
        <td><span data-ttu-id="4fd4a-758">Ne</span><span class="sxs-lookup"><span data-stu-id="4fd4a-758">No</span></span></td>
        <td><span data-ttu-id="4fd4a-759">Chybová zpráva v odpovědi bude určení toho, co se v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-759">The error message in the response will indicate what is wrong with the document.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-760">404</span><span class="sxs-lookup"><span data-stu-id="4fd4a-760">404</span></span></td>
        <td><span data-ttu-id="4fd4a-761">Dokument nelze sloučit, protože neexistuje daným klíčem v indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-761">The document could not be merged because the given key doesn't exist in the index.</span></span></td>
        <td><span data-ttu-id="4fd4a-762">Ne</span><span class="sxs-lookup"><span data-stu-id="4fd4a-762">No</span></span></td>
        <td><span data-ttu-id="4fd4a-763">Této chybě nedochází pro nahrávání vzhledem k tomu, že se vytváření nových dokumentů a nespustí se pro odstranění vzhledem k tomu, že jsou <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-763">This error does not occur for uploads since they create new documents, and it does not occur for deletes because they are <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-764">409</span><span class="sxs-lookup"><span data-stu-id="4fd4a-764">409</span></span></td>
        <td><span data-ttu-id="4fd4a-765">Při pokusu o indexu dokument byl zjištěn konflikt verzí.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-765">A version conflict was detected when attempting to index a document.</span></span></td>
        <td><span data-ttu-id="4fd4a-766">Ano</span><span class="sxs-lookup"><span data-stu-id="4fd4a-766">Yes</span></span></td>
        <td><span data-ttu-id="4fd4a-767">Tomu může dojít, když se pokoušíte více než jednou souběžně indexu stejného dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-767">This can happen when you're trying to index the same document more than once concurrently.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-768">422</span><span class="sxs-lookup"><span data-stu-id="4fd4a-768">422</span></span></td>
        <td><span data-ttu-id="4fd4a-769">Index je dočasně nedostupný, protože byla aktualizována 'allowIndexDowntime' příznak nastaven na hodnotu "true".</span><span class="sxs-lookup"><span data-stu-id="4fd4a-769">The index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'.</span></span></td>
        <td><span data-ttu-id="4fd4a-770">Ano</span><span class="sxs-lookup"><span data-stu-id="4fd4a-770">Yes</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="4fd4a-771">503</span><span class="sxs-lookup"><span data-stu-id="4fd4a-771">503</span></span></td>
        <td><span data-ttu-id="4fd4a-772">Vyhledávací služba je dočasně nedostupná, pravděpodobně z důvodu velké zatížení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-772">Your search service is temporarily unavailable, possibly due to heavy load.</span></span></td>
        <td><span data-ttu-id="4fd4a-773">Ano</span><span class="sxs-lookup"><span data-stu-id="4fd4a-773">Yes</span></span></td>
        <td><span data-ttu-id="4fd4a-774">Váš kód čekat, než v tomto případě nebo riziko prodloužit nedostupnost služby.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-774">Your code should wait before retrying in this case or you risk prolonging the service unavailability.</span></span></td>
    </tr>
</table> 

<span data-ttu-id="4fd4a-775">**Poznámka:**: Pokud váš klientský kód často zaznamená 207 odpověď, jedním z možných důvodů je, že systém je zatížení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-775">**Note**: If your client code frequently encounters a 207 response, one possible reason is that the system is under load.</span></span> <span data-ttu-id="4fd4a-776">Můžete to ověřit kontrolou `statusCode` vlastnost 503.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-776">You can confirm this by checking the `statusCode` property for 503.</span></span> <span data-ttu-id="4fd4a-777">Pokud je to tento případ, doporučujeme ***omezení indexování požadavky***.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-777">If this is the case, we recommend ***throttling indexing requests***.</span></span> <span data-ttu-id="4fd4a-778">Jinak hodnota Pokud indexování provozu nepodporuje subside, systém může spustit odmítat všechny požadavky s 503 chyby.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-778">Otherwise, if indexing traffic doesn't subside, the system could start rejecting all requests with 503 errors.</span></span>

<span data-ttu-id="4fd4a-779">Kód stavu 429 označuje, že jste překročili kvótu pro počet dokumentů na index.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-779">Status code 429 indicates that you have exceeded your quota on the number of documents per index.</span></span> <span data-ttu-id="4fd4a-780">Musíte vytvořit nový index nebo upgrade na vyšší limity kapacity.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-780">You must either create a new index or upgrade for higher capacity limits.</span></span>

<span data-ttu-id="4fd4a-781">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-781">**Example:**</span></span>

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
          "hotelName": "Fancy Stay",
          "category": "Luxury",
          "tags": ["pool", "view", "wifi", "concierge"],
          "parkingIncluded": false,
          "smokingAllowed": false,
          "lastRenovationDate": "2010-06-27T00:00:00Z",
          "rating": 5,
          "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
          "@search.action": "upload",
          "hotelId": "2",
          "baseRate": 79.99,
          "description": "Cheapest hotel in town",
          "description_fr": "Hôtel le moins cher en ville",
          "hotelName": "Roach Motel",
          "category": "Budget",
          "tags": ["motel", "budget"],
          "parkingIncluded": true,
          "smokingAllowed": true,
          "lastRenovationDate": "1982-04-28T00:00:00Z",
          "rating": 1,
          "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
- - -
<a name="SearchDocs"></a>

## <a name="search-documents"></a><span data-ttu-id="4fd4a-782">Vyhledávání dokumentů</span><span class="sxs-lookup"><span data-stu-id="4fd4a-782">Search Documents</span></span>
<span data-ttu-id="4fd4a-783">A **vyhledávání** operace se objeví jako požadavek GET nebo POST a určuje parametry, které poskytují kritéria pro výběr odpovídající dokumenty.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-783">A **Search** operation is issued as a GET or POST request and specifies parameters that give the criteria for selecting matching documents.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

<span data-ttu-id="4fd4a-784">**Kdy použít POST místo GET**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-784">**When to use POST instead of GET**</span></span>

<span data-ttu-id="4fd4a-785">Při použití metody GET protokolu HTTP k volání **vyhledávání** rozhraní API, musíte vědět, že nesmí překročit délka adresy URL žádosti 8 KB.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-785">When you use HTTP GET to call the **Search** API, you need to be aware that the length of the request URL cannot exceed 8 KB.</span></span> <span data-ttu-id="4fd4a-786">Je to obvykle dostatečně pro většinu aplikací.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-786">This is usually enough for most applications.</span></span> <span data-ttu-id="4fd4a-787">Některé aplikace však vytvořit velmi velké dotazy nebo výrazy filtru OData.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-787">However, some applications produce very large queries or OData filter expressions.</span></span> <span data-ttu-id="4fd4a-788">Pro tyto aplikace pomocí HTTP POST je lepší volbou, protože umožňuje větší filtry a dotazy, než metoda GET.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-788">For these applications, using HTTP POST is a better choice because it allows larger filters and queries than GET.</span></span> <span data-ttu-id="4fd4a-789">S POST počet termíny nebo klauzule v dotazu je omezující faktor, ne podle velikosti vytvořeného nezpracovaná dotaz vzhledem k tomu, že je přibližně 16 MB omezení velikosti pro metodu POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-789">With POST, the number of terms or clauses in a query is the limiting factor, not the size of the raw query since the request size limit for POST is approximately 16 MB.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-790">I když maximální velikost požadavku POST je velmi velké, nemůže být libovolně komplexní dotazy vyhledávání a výrazy filtru.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-790">Even though the POST request size limit is very large, search queries and filter expressions cannot be arbitrarily complex.</span></span> <span data-ttu-id="4fd4a-791">V tématu [syntaxe dotazů Lucene](https://msdn.microsoft.com/library/mt589323.aspx) a [syntaxe výrazu OData](https://msdn.microsoft.com/library/dn798921.aspx) pro další informace o omezeních složitost vyhledávání dotazu a filtr.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-791">See [Lucene query syntax](https://msdn.microsoft.com/library/mt589323.aspx) and [OData expression syntax](https://msdn.microsoft.com/library/dn798921.aspx) for more information about search query and filter complexity limitations.</span></span>
> 
> 

<span data-ttu-id="4fd4a-792">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-792">**Request**</span></span>

<span data-ttu-id="4fd4a-793">Protokol HTTPS je vyžadována pro žádosti o služby.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-793">HTTPS is required for service requests.</span></span> <span data-ttu-id="4fd4a-794">**Vyhledávání** požadavek se dá vytvořit pomocí metody GET nebo POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-794">The **Search** request can be constructed using the GET or POST methods.</span></span>

<span data-ttu-id="4fd4a-795">Identifikátoru URI požadavku určuje index, který do dotazu, pro všechny dokumenty, které odpovídají parametry.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-795">The request URI specifies which index to query, for all documents that match the parameters.</span></span> <span data-ttu-id="4fd4a-796">V řetězci dotazu v případě požadavky GET jsou zadány parametry a v žádosti subjekt v případě POST požadavky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-796">Parameters are specified on the query string in the case of GET requests, and in the request body in the case of POST requests.</span></span>

<span data-ttu-id="4fd4a-797">Jako osvědčený postup při vytváření požadavky GET, nezapomeňte [kódování URL](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) parametry specifického dotazu při přímé volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-797">As a best practice when creating GET requests, remember to [URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specific query parameters when calling the REST API directly.</span></span> <span data-ttu-id="4fd4a-798">Pro **vyhledávání** operace, to zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-798">For **Search** operations, this includes:</span></span>

* `$filter`
* `facet`
* `highlightPreTag`
* `highlightPostTag`
* `search`
* `moreLikeThis`

<span data-ttu-id="4fd4a-799">Kódování URL se doporučuje jenom na výše uvedené parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-799">URL encoding is only recommended on the above query parameters.</span></span> <span data-ttu-id="4fd4a-800">Pokud jste omylem kódování URL řetězec dotazu celý (vše za?), by došlo k přerušení požadavky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-800">If you inadvertently URL-encode the entire query string (everything after the ?), requests will break.</span></span>

<span data-ttu-id="4fd4a-801">Navíc kódování URL je nutné pouze při volání rozhraní REST API přímo pomocí GET.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-801">Also, URL encoding is only necessary when calling the REST API directly using GET.</span></span> <span data-ttu-id="4fd4a-802">Žádné kódování URL je nezbytné při volání metody **vyhledávání** pomocí POST, nebo při použití [klientské knihovny .NET](https://msdn.microsoft.com/library/dn951165.aspx), který zpracovává kódování URL za vás.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-802">No URL encoding is necessary when calling **Search** using POST, or when using the [.NET client library](https://msdn.microsoft.com/library/dn951165.aspx), which handles URL encoding for you.</span></span>

<span data-ttu-id="4fd4a-803"><a name="SearchQueryParameters"></a>
**Parametry dotazu**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-803"><a name="SearchQueryParameters"></a>
**Query Parameters**</span></span>

<span data-ttu-id="4fd4a-804">**Hledání** přijímá několik parametrů, které poskytují kritéria dotazu a také určit chování vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-804">**Search** accepts several parameters that provide query criteria and also specify search behavior.</span></span> <span data-ttu-id="4fd4a-805">Zadejte tyto parametry v adrese URL řetězec dotazu při volání metody **vyhledávání** prostřednictvím GET a jako vlastnosti JSON v textu požadavku při volání metody **vyhledávání** přes POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-805">You provide these parameters in the URL query string when calling **Search** via GET, and as JSON properties in the request body when calling **Search** via POST.</span></span> <span data-ttu-id="4fd4a-806">Syntaxe pro některé parametry se mírně liší mezi GET a POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-806">The syntax for some parameters is slightly different between GET and POST.</span></span> <span data-ttu-id="4fd4a-807">Tyto rozdíly jsou popsány podle vhodnosti níže:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-807">These differences are noted as applicable below:</span></span>

<span data-ttu-id="4fd4a-808">`search=[string]`(volitelné) - text k vyhledání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-808">`search=[string]` (optional) - The text to search for.</span></span> <span data-ttu-id="4fd4a-809">Všechny `searchable` prohledány jsou ve výchozím nastavení není-li `searchFields` je zadán.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-809">All `searchable` fields are searched by default unless `searchFields` is specified.</span></span> <span data-ttu-id="4fd4a-810">Při hledání `searchable` tokenizovaného polí a vlastní text vyhledávání, tak více podmínek je možné oddělit prázdné znaky (například: `search=hello world`).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-810">When searching `searchable` fields, the search text itself is tokenized, so multiple terms can be separated by white space (for example: `search=hello world`).</span></span> <span data-ttu-id="4fd4a-811">Vyhledat všechny termín, použijte `*` (může to být užitečné pro dotazy Booleovský filtr).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-811">To match any term, use `*` (this can be useful for boolean filter queries).</span></span> <span data-ttu-id="4fd4a-812">Tento parametr vynechá má stejný účinek jako jeho nastavení na hodnotu `*`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-812">Omitting this parameter has the same effect as setting it to `*`.</span></span> <span data-ttu-id="4fd4a-813">V tématu [jednoduchá syntaxe dotazů](https://msdn.microsoft.com/library/dn798920.aspx) pro konkrétní na syntaxe vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-813">See [Simple Query Syntax](https://msdn.microsoft.com/library/dn798920.aspx) for specifics on the search syntax.</span></span>

* <span data-ttu-id="4fd4a-814">**Poznámka:**: výsledky mohou být někdy překvapivé při dotazování přes `searchable` pole.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-814">**Note**: The results can sometimes be surprising when querying over `searchable` fields.</span></span> <span data-ttu-id="4fd4a-815">Tokenizátor obsahuje logiku pro zpracování případů, které jsou společné pro angličtinu text jako apostrofy, čárky ve čísel atd. Například `search=123,456` bude shodovat s jeden termín 123,456 místo jednotlivých podmínky 123 a 456, protože čárkami jsou použity jako oddělovače tisíc pro velké počty v angličtině.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-815">The tokenizer includes logic to handle cases common to English text like apostrophes, commas in numbers, etc. For example, `search=123,456` will match a single term 123,456 rather than the individual terms 123 and 456, since commas are used as thousand-separators for large numbers in English.</span></span> <span data-ttu-id="4fd4a-816">Z tohoto důvodu doporučujeme používat mezer spíše než interpunkce oddělit podmínkami `search` parametr.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-816">For this reason, we recommend using white space rather than punctuation to separate terms in the `search` parameter.</span></span>

<span data-ttu-id="4fd4a-817">`searchMode=any|all`(volitelné, použije se výchozí hodnota `any`) – ať některého nebo všech hledané termíny musí odpovídat k sečtení dokumentu jako shoda.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-817">`searchMode=any|all` (optional, defaults to `any`) - whether any or all of the search terms must be matched in order to count the document as a match.</span></span>

<span data-ttu-id="4fd4a-818">`searchFields=[string]`(volitelné) – seznam názvů oddělených čárkami pole pro vyhledávání pro zadaný text.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-818">`searchFields=[string]` (optional) - The list of comma-separated field names to search for the specified text.</span></span> <span data-ttu-id="4fd4a-819">Cílová pole. musí být označen jako `searchable`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-819">Target fields must be marked as `searchable`.</span></span>

<span data-ttu-id="4fd4a-820">`queryType=simple|full`(volitelné, použije se výchozí hodnota `simple`) – Pokud je nastaven na "jednoduchý" hledaný text interpretovat pomocí jednoduchého dotazovací jazyk, který umožňuje pro symboly, například +, * a "".</span><span class="sxs-lookup"><span data-stu-id="4fd4a-820">`queryType=simple|full` (optional, defaults to `simple`) - when set to "simple" search text is interpreted using a simple query language that allows for symbols such as +, * and "".</span></span> <span data-ttu-id="4fd4a-821">Dotazy jsou vyhodnocovány napříč všechna prohledatelná pole (nebo pole uvedené v `searchFields`) v každém dokumentu ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-821">Queries are evaluated across all searchable fields (or fields indicated in `searchFields`) in each document by default.</span></span> <span data-ttu-id="4fd4a-822">Když je typ dotazu nastavený na `full` hledaný text interpretována pomocí jazyka dotazů Lucene, což umožňuje specifické pole a vyvážené hledání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-822">When the query type is set to `full` search text is interpreted using the Lucene query language which allows field-specific and weighted searches.</span></span> <span data-ttu-id="4fd4a-823">V tématu [jednoduchá syntaxe dotazů](https://msdn.microsoft.com/library/dn798920.aspx) a [syntaxe dotazů Lucene](https://msdn.microsoft.com/library/mt589323.aspx) pro konkrétní na syntaxe vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-823">See [Simple Query Syntax](https://msdn.microsoft.com/library/dn798920.aspx) and [Lucene Query Syntax](https://msdn.microsoft.com/library/mt589323.aspx) for specifics on the search syntaxes.</span></span> 

> [!NOTE]
> <span data-ttu-id="4fd4a-824">Rozsah vyhledávání v Lucene dotazovací jazyk nepodporuje považuje $filter který nabízí podobné funkce.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-824">Range search in the Lucene query language is not supported in favor of $filter which offers similar functionality.</span></span>
> 
> 

<span data-ttu-id="4fd4a-825">`moreLikeThis=[key]`(volitelné) **Důležité:** tato funkce je k dispozici v `2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-825">`moreLikeThis=[key]` (optional) **Important:** This feature is only available in `2015-02-28-Preview`.</span></span> <span data-ttu-id="4fd4a-826">Tuto možnost nelze použít v dotazu, který obsahuje parametr hledání textu `search=[string]`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-826">This option cannot be used in a query that contains the text search parameter, `search=[string]`.</span></span> <span data-ttu-id="4fd4a-827">`moreLikeThis` Parametr Vyhledá dokumenty, které jsou podobné dokumentu určeného klíč dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-827">The `moreLikeThis` parameter finds documents that are similar to the document specified by the document key.</span></span> <span data-ttu-id="4fd4a-828">Při hledání požadavku s `moreLikeThis`, vygeneruje se seznam podmínek vyhledávání, na základě frekvence a vzácnost podmínek ve zdrojovém dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-828">When a search request is made with `moreLikeThis`, a list of search terms is generated based on the frequency and rarity of terms in the source document.</span></span> <span data-ttu-id="4fd4a-829">Tyto podmínky jsou poté použít k vytvoření žádosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-829">Those terms are then used to make the request.</span></span> <span data-ttu-id="4fd4a-830">Ve výchozím nastavení, obsah všech `searchable` pole jsou považovány za Pokud `searchFields` se používá k omezení, které prohledány.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-830">By default, the contents of all `searchable` fields are considered unless `searchFields` is used to restrict which fields are searched.</span></span>  

<span data-ttu-id="4fd4a-831">`$skip=#`(volitelné) – počet výsledků hledání tak, aby přeskočil; Nemůže být vyšší než 100 000.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-831">`$skip=#` (optional) - the number of search results to skip; Cannot be greater than 100,000.</span></span> <span data-ttu-id="4fd4a-832">Pokud potřebujete ke kontrole dokumentů v pořadí, ale nemůžete použít `$skip` kvůli tomuto omezení, zvažte použití `$orderby` na klíč zcela seřazené a `$filter` s rozsahem dotazu místo.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-832">If you need to scan documents in sequence but cannot use `$skip` due to this limitation, consider using `$orderby` on a totally-ordered key and `$filter` with a range query instead.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-833">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `skip` místo `$skip`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-833">When calling **Search** using POST, this parameter is named `skip` instead of `$skip`.</span></span>
> 
> 

<span data-ttu-id="4fd4a-834">`$top=#`(volitelné) – počet výsledků hledání pro načtení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-834">`$top=#` (optional) - the number of search results to retrieve.</span></span> <span data-ttu-id="4fd4a-835">To lze použít ve spojení s `$skip` implementaci klienta stránkování výsledků vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-835">This can be used in conjunction with `$skip` to implement client-side paging of search results.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-836">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `top` místo `$top`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-836">When calling **Search** using POST, this parameter is named `top` instead of `$top`.</span></span>
> 
> 

<span data-ttu-id="4fd4a-837">`$count=true|false`(volitelné, použije se výchozí hodnota `false`)-určuje, jestli se načíst celkový počet výsledků.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-837">`$count=true|false` (optional, defaults to `false`) - Specifies whether to fetch the total count of results.</span></span> <span data-ttu-id="4fd4a-838">Toto je počet všechny dokumenty, které odpovídají `search` a `$filter` parametry, ignoruje `$top` a `$skip`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-838">This is the count of all documents that match the `search` and `$filter` parameters, ignoring `$top` and `$skip`.</span></span> <span data-ttu-id="4fd4a-839">Nastavení této hodnoty na `true` může mít dopad na výkon.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-839">Setting this value to `true` may have a performance impact.</span></span> <span data-ttu-id="4fd4a-840">Všimněte si, že vrácený počet je sblížení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-840">Note that the count returned is an approximation.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-841">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `count` místo `$count`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-841">When calling **Search** using POST, this parameter is named `count` instead of `$count`.</span></span>
> 
> 

<span data-ttu-id="4fd4a-842">`$orderby=[string]`(volitelné) – seznam výrazů textový soubor s oddělovači na výsledky seřaďte podle.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-842">`$orderby=[string]` (optional) - A list of comma-separated expressions to sort the results by.</span></span> <span data-ttu-id="4fd4a-843">Každý výraz může být název pole nebo volání `geo.distance()` funkce.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-843">Each expression can be either a field name or a call to the `geo.distance()` function.</span></span> <span data-ttu-id="4fd4a-844">Každý výraz může následovat `asc` uvést vzestupné řazení a `desc` k označení sestupném.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-844">Each expression can be followed by `asc` to indicated ascending, and `desc` to indicate descending.</span></span> <span data-ttu-id="4fd4a-845">Výchozí hodnota je vzestupné pořadí.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-845">The default is ascending order.</span></span> <span data-ttu-id="4fd4a-846">TIES bude rozděleno podle skóre shodu dokumentů.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-846">Ties will be broken by the match scores of documents.</span></span> <span data-ttu-id="4fd4a-847">Pokud žádné `$orderby` je zadána, je výchozí pořadí řazení sestupně podle skóre shodu dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-847">If no `$orderby` is specified, the default sort order is descending by document match score.</span></span> <span data-ttu-id="4fd4a-848">Existuje limit 32 klauzule pro `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-848">There is a limit of 32 clauses for `$orderby`.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-849">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `orderby` místo `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-849">When calling **Search** using POST, this parameter is named `orderby` instead of `$orderby`.</span></span>
> 
> 

<span data-ttu-id="4fd4a-850">`$select=[string]`(volitelné) – seznam polí, oddělených čárkami k načtení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-850">`$select=[string]` (optional) - A list of comma-separated fields to retrieve.</span></span> <span data-ttu-id="4fd4a-851">Pokud tento parametr zadán, jsou zahrnuty všechny pole označené jako získat ve schématu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-851">If unspecified, all fields marked as retrievable in the schema are included.</span></span> <span data-ttu-id="4fd4a-852">Můžete také explicitní žádost o všechna pole nastavením tohoto parametru na `*`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-852">You can also explicitly request all fields by setting this parameter to `*`.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-853">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `select` místo `$select`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-853">When calling **Search** using POST, this parameter is named `select` instead of `$select`.</span></span>
> 
> 

<span data-ttu-id="4fd4a-854">`facet=[string]`(nula nebo více) – pole do omezující vlastnosti podle.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-854">`facet=[string]` (zero or more) - A field to facet by.</span></span> <span data-ttu-id="4fd4a-855">Řetězec může volitelně obsahovat parametry k přizpůsobení používání faset, vyjádřené jako textový soubor s oddělovači `name:value` páry.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-855">Optionally the string may contain parameters to customize the faceting expressed as comma-separated `name:value` pairs.</span></span> <span data-ttu-id="4fd4a-856">Jsou platné parametry:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-856">Valid parameters are:</span></span>

* <span data-ttu-id="4fd4a-857">`count`(maximální počet omezující vlastnosti podmínky; výchozí hodnota je 10).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-857">`count` (max number of facet terms; default is 10).</span></span> <span data-ttu-id="4fd4a-858">Neexistuje žádné maximální, ale vyšší hodnoty zpoplatněná odpovídající snížení výkonu, zejména pokud pole Fasetové obsahuje velký počet jedinečných podmínky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-858">There is no maximum, but higher values incur a corresponding performance penalty, especially if the faceted field contains a large number of unique terms.</span></span>
  * <span data-ttu-id="4fd4a-859">Příklad: `facet=category,count:5` získá horní pěti kategorií ve výsledcích omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-859">For example: `facet=category,count:5` gets the top five categories in facet results.</span></span>  
  * <span data-ttu-id="4fd4a-860">**Poznámka:**: Pokud `count` parametru je menší než počet jedinečných podmínky, nemusí být přesné výsledky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-860">**Note**: If the `count` parameter is less than the number of unique terms, the results may not be accurate.</span></span> <span data-ttu-id="4fd4a-861">Příčinou je způsob používání omezujících vlastností dotazy jsou rozmístěny v horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-861">This is due to the way faceting queries are distributed across shards.</span></span> <span data-ttu-id="4fd4a-862">Zvýšení `count` obvykle zvyšuje přesnost počtů termín, ale v snížený výkon.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-862">Increasing `count` generally increases the accuracy of the term counts, but at a performance cost.</span></span>
* <span data-ttu-id="4fd4a-863">`sort`(jeden z `count` seřadit *sestupném* podle počtu, `-count` seřadit *vzestupném* podle počtu, `value` seřadit *vzestupném* hodnotu nebo `-value` seřadit *sestupném* podle hodnoty)</span><span class="sxs-lookup"><span data-stu-id="4fd4a-863">`sort` (one of `count` to sort *descending* by count, `-count` to sort *ascending* by count, `value` to sort *ascending* by value, or `-value` to sort *descending* by value)</span></span>
  * <span data-ttu-id="4fd4a-864">Příklad: `facet=category,count:3,sort:count` získá první tři kategorie ve výsledcích omezující vlastnosti v sestupném pořadí podle počtu dokumentů s každý název města.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-864">For example: `facet=category,count:3,sort:count` gets the top three categories in facet results in descending order by the number of documents with each city name.</span></span> <span data-ttu-id="4fd4a-865">Například pokud jsou hlavní tří kategorií rozpočtu, Motel a možnost a nároky má 5 přístupů, Motel má 6 a možnost má 4, pak kbelíků bude v pořadí Motel, rozpočtu, možnost.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-865">For example, if the top three categories are Budget, Motel, and Luxury, and Budget has 5 hits, Motel has 6, and Luxury has 4, then the buckets will be in the order Motel, Budget, Luxury.</span></span>
  * <span data-ttu-id="4fd4a-866">Příklad: `facet=rating,sort:-value` vytváří kbelíků pro všechny možné hodnocení v sestupném pořadí podle hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-866">For example: `facet=rating,sort:-value` produces buckets for all possible ratings, in descending order by value.</span></span> <span data-ttu-id="4fd4a-867">Například pokud hodnocení od 1 do 5, kbelíků bude objednáno 5, 4, 3, 2, 1, bez ohledu na to, kolik dokumenty odpovídají každé hodnocení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-867">For example, if the ratings are from 1 to 5, the buckets will be ordered 5, 4, 3, 2, 1, irrespective of how many documents match each rating.</span></span>
* <span data-ttu-id="4fd4a-868">`values`(oddělený kanálu číselný nebo `Edm.DateTimeOffset` hodnoty určující dynamickou sadu hodnot omezující vlastnosti položky)</span><span class="sxs-lookup"><span data-stu-id="4fd4a-868">`values` (pipe-delimited numeric or `Edm.DateTimeOffset` values specifying a dynamic set of facet entry values)</span></span>
  * <span data-ttu-id="4fd4a-869">Příklad: `facet=baseRate,values:10|20` vytvoří tři kbelíků: jeden pro základní míra 0 až do, ale není včetně 10, jeden pro 10 až s výjimkou 20 a jeden pro 20 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-869">For example: `facet=baseRate,values:10|20` produces three buckets: One for base rate 0 up to but not including 10, one for 10 up to but not including 20, and one for 20 or higher.</span></span>
  * <span data-ttu-id="4fd4a-870">Příklad: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` vytvoří dvě kbelíků: jeden pro hotels renovovanou před. února 2010 a jeden pro hotels renovovanou února 1. 2010 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-870">For example: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` produces two buckets: One for hotels renovated before February 2010, and one for hotels renovated February 1st, 2010 or later.</span></span>
* <span data-ttu-id="4fd4a-871">`interval`(interval celé číslo větší než 0. v případě čísel nebo `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` pro hodnoty času datum)</span><span class="sxs-lookup"><span data-stu-id="4fd4a-871">`interval` (integer interval greater than 0 for numbers, or `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` for date time values)</span></span>
  * <span data-ttu-id="4fd4a-872">Příklad: `facet=baseRate,interval:100` vytváří kbelíků založené na základní míra rozsahy velikost 100.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-872">For example: `facet=baseRate,interval:100` produces buckets based on base rate ranges of size 100.</span></span> <span data-ttu-id="4fd4a-873">Například pokud základní sazby všechny až 60 $ $600, budou existovat intervaly 0-100, 100 200, 200 300, 300 400, 400-500 a 500 600.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-873">For example, if base rates are all between $60 and $600, there will be buckets for 0-100, 100-200, 200-300, 300-400, 400-500, and 500-600.</span></span>
  * <span data-ttu-id="4fd4a-874">Příklad: `facet=lastRenovationDate,interval:year` vytváří jeden sady pro každý rok po hotels byly renovovanou.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-874">For example: `facet=lastRenovationDate,interval:year` produces one bucket for each year when hotels were renovated.</span></span>
* <span data-ttu-id="4fd4a-875">`timeoffset`([+-] hh: mm, [+-] hh: mm, nebo [+-] hh) `timeoffset` je volitelný.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-875">`timeoffset` ([+-]hh:mm, [+-]hhmm, or [+-]hh) `timeoffset` is optional.</span></span> <span data-ttu-id="4fd4a-876">Můžete sloučit pouze s `interval` možnost a pouze v případě, že se použije pro pole typu `Edm.DateTimeOffset`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-876">It can only be combined with the `interval` option, and only when applied to a field of type `Edm.DateTimeOffset`.</span></span> <span data-ttu-id="4fd4a-877">Hodnota určuje posunem od standardu UTC čas k účtu pro nastavení času hranice.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-877">The value specifies the UTC time offset to account for in setting time boundaries.</span></span>
  * <span data-ttu-id="4fd4a-878">Příklad: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` používá den hranic, která se spouští v 01:00:00 UTC (půlnoc v časovém pásmu cíl)</span><span class="sxs-lookup"><span data-stu-id="4fd4a-878">For example: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` uses the day boundary that starts at 01:00:00 UTC (midnight in the target time zone)</span></span>
* <span data-ttu-id="4fd4a-879">**Poznámka:**: `count` a `sort` lze spojit do stejné omezující vlastnost specifikace, ale nelze kombinovat s `interval` nebo `values`, a `interval` a `values` nemůže být společně kombinovány.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-879">**Note**: `count` and `sort` can be combined in the same facet specification, but they cannot be combined with `interval` or `values`, and `interval` and `values` cannot be combined together.</span></span>
* <span data-ttu-id="4fd4a-880">**Poznámka:**: omezující vlastnosti Interval na datum a čas se vypočítávají podle času UTC, pokud `timeoffset` není zadán.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-880">**Note**: Interval facets on date time are computed based on UTC time if `timeoffset` is not specified.</span></span> <span data-ttu-id="4fd4a-881">Příklad: pro `facet=lastRenovationDate,interval:day`, den hranic začíná na 00:00:00 UTC.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-881">For example: for `facet=lastRenovationDate,interval:day`, the day boundary starts at 00:00:00 UTC.</span></span> 

> [!NOTE]
> <span data-ttu-id="4fd4a-882">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `facets` místo `facet`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-882">When calling **Search** using POST, this parameter is named `facets` instead of `facet`.</span></span> <span data-ttu-id="4fd4a-883">Také můžete zadat jako pole JSON řetězců, kde každý řetězec je výraz samostatné omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-883">Also, you specify it as a JSON array of strings where each string is a separate facet expression.</span></span>
> 
> 

<span data-ttu-id="4fd4a-884">`$filter=[string]`(volitelné) – výraz strukturovaných vyhledávání v standardní syntaxi OData.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-884">`$filter=[string]` (optional) - A structured search expression in standard OData syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-885">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `filter` místo `$filter`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-885">When calling **Search** using POST, this parameter is named `filter` instead of `$filter`.</span></span>
> 
> 

<span data-ttu-id="4fd4a-886">`highlight=[string]`(volitelné) – označuje sadu názvů polí oddělených čárkami, které používá pro stiskněte klávesu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-886">`highlight=[string]` (optional) - A set of comma-separated field names used for hit highlights.</span></span> <span data-ttu-id="4fd4a-887">Pouze `searchable` pole lze použít pro přístupů zvýraznění.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-887">Only `searchable` fields can be used for hit highlighting.</span></span>

<span data-ttu-id="4fd4a-888">`highlightPreTag=[string]`(volitelné, použije se výchozí hodnota `<em>`) – řetězec značky, která přidá narazí označuje.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-888">`highlightPreTag=[string]` (optional, defaults to `<em>`) - A string tag that prepends to hit highlights.</span></span> <span data-ttu-id="4fd4a-889">Musí být nastavena s `highlightPostTag`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-889">Must be set with `highlightPostTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-890">Při volání metody **vyhledávání** pomocí GET, vyhrazené znaky v adrese URL musí být kódovaný v procentech (například místo # % 23).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-890">When calling **Search** using GET, reserved characters in the URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="4fd4a-891">`highlightPostTag=[string]`(volitelné, použije se výchozí hodnota `</em>`)-tag řetězec, který připojí k průchodu označuje.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-891">`highlightPostTag=[string]` (optional, defaults to `</em>`) - a string tag that appends to hit highlights.</span></span> <span data-ttu-id="4fd4a-892">Musí být nastavena s `highlightPreTag`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-892">Must be set with `highlightPreTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-893">Při volání metody **vyhledávání** pomocí GET, vyhrazené znaky v adrese URL musí být kódovaný v procentech (například místo # % 23).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-893">When calling **Search** using GET, reserved characters in the URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="4fd4a-894">`scoringProfile=[string]`(volitelné) – název profilu vyhodnocování vyhodnotit odpovídat skóre pro párování dokumenty k řazení výsledků.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-894">`scoringProfile=[string]` (optional) - The name of a scoring profile to evaluate match scores for matching documents in order to sort the results.</span></span>

<span data-ttu-id="4fd4a-895">`scoringParameter=[string]`(nula nebo více) – označuje hodnoty pro každý parametr definovaný ve funkci vyhodnocování (například `referencePointParameter`) ve formátu `name-value1,value2,...`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-895">`scoringParameter=[string]` (zero or more) - Indicates the values for each parameter defined in a scoring function (for example, `referencePointParameter`) using the format `name-value1,value2,...`.</span></span>

* <span data-ttu-id="4fd4a-896">Například pokud profil vyhodnocování definuje funkci parametr s názvem "mylocation" parametr řetězce dotazu by `&scoringParameter=mylocation--122.2,44.8`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-896">For example, if the scoring profile defines a function with a parameter called "mylocation" the query string option would be `&scoringParameter=mylocation--122.2,44.8`.</span></span> <span data-ttu-id="4fd4a-897">První Čárka odděluje název ze seznamu hodnotu druhý čárka je součást první hodnota (zeměpisné délky v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-897">The first dash separates the name from the value list, while the second dash is part of the first value (longitude in this example).</span></span>
* <span data-ttu-id="4fd4a-898">Pro výpočet skóre parametry, jako je značky zvyšovat skóre, který může obsahovat čárky můžete vyhnuli tyto hodnoty v seznamu pomocí jednoduchých uvozovek a být.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-898">For scoring parameters such as for tag boosting that can contain commas, you can escape any such values in the list using single quotes.</span></span> <span data-ttu-id="4fd4a-899">Pokud hodnoty samotné obsahovat jednoduché uvozovky, můžete je vyhnuli předchozího.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-899">If the values themselves contain single quotes, you can escape them by doubling.</span></span>
  * <span data-ttu-id="4fd4a-900">Například pokud máte značku zvyšovat skóre parametr s názvem "mytag" a vy chcete zvýšit ve značce hodnoty "Hello, O'Brien" a "Smith", dotaz možnost řetězce by `&scoringParameter=mytag-'Hello, O''Brien',Smith`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-900">For example, if you have a tag boosting parameter called "mytag" and you want to boost on the tag values "Hello, O'Brien" and "Smith", the query string option would be `&scoringParameter=mytag-'Hello, O''Brien',Smith`.</span></span> <span data-ttu-id="4fd4a-901">Všimněte si, že uvozovky jsou pouze požadované hodnoty, které obsahují čárkami.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-901">Note that quotes are only required for values that contain commas.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-902">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `scoringParameters` místo `scoringParameter`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-902">When calling **Search** using POST, this parameter is named `scoringParameters` instead of `scoringParameter`.</span></span> <span data-ttu-id="4fd4a-903">Navíc je zadat jako pole JSON řetězců, kde je každý řetězec samostatné `name-values` pár.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-903">Also, you specify it as a JSON array of strings where each string is a separate `name-values` pair.</span></span>
> 
> 

<span data-ttu-id="4fd4a-904">`minimumCoverage`(volitelné, výchozí hodnota je 100)-číslo mezi 0 a 100 procentuální hodnota indexu, která musí být předmětem vyhledávací dotaz v pořadí pro dotaz na hlášené jako úspěšné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-904">`minimumCoverage` (optional, defaults to 100) - a number between 0 and 100 indicating the percentage of the index that must be covered by a search query in order for the query to be reported as a success.</span></span> <span data-ttu-id="4fd4a-905">Ve výchozím nastavení, musí být k dispozici celý index nebo `Search` vrátí stavový kód protokolu HTTP 503.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-905">By default, the entire index must be available or `Search` will return HTTP status code 503.</span></span> <span data-ttu-id="4fd4a-906">Pokud nastavíte `minimumCoverage` a `Search` úspěšné, se vrátí HTTP 200 a zahrnout `@search.coverage` hodnotu v odpovědi procentuální hodnota indexu, pro který je zahrnutý v dotazu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-906">If you set `minimumCoverage` and `Search` succeeds, it will return HTTP 200 and include a `@search.coverage` value in the response indicating the percentage of the index that was included in the query.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-907">Nastavení tohoto parametru na hodnotu nižší než 100 může být užitečná pro zajištění dostupnosti vyhledávání i pro služby s pouze jednu repliku.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-907">Setting this parameter to a value lower than 100 can be useful for ensuring search availability even for services with only one replica.</span></span> <span data-ttu-id="4fd4a-908">Ne všechny odpovídající dokumenty jsou však zaručena nacházet ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-908">However, not all matching documents are guaranteed to be present in the search results.</span></span> <span data-ttu-id="4fd4a-909">Pokud se pro vaše aplikace důležitější než dostupnosti pro vyvolání vyhledávání, pak je vhodné ponechat `minimumCoverage` na výchozí hodnotě 100.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-909">If search recall is more important to your application than availability, then it's best to leave `minimumCoverage` at its default value of 100.</span></span>
> 
> 

<span data-ttu-id="4fd4a-910">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-910">`api-version=[string]` (required).</span></span> <span data-ttu-id="4fd4a-911">Verze preview je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-911">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="4fd4a-912">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-912">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="4fd4a-913">Poznámka: pro tuto operaci `api-version` je zadána jako parametr dotazu v adrese URL bez ohledu na to, jestli volání **vyhledávání** s GET nebo POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-913">Note: For this operation, the `api-version` is specified as a query parameter in the URL regardless of whether you call **Search** with GET or POST.</span></span>

<span data-ttu-id="4fd4a-914">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-914">**Request Headers**</span></span>

<span data-ttu-id="4fd4a-915">Následující seznam popisuje hlavičky žádosti požadované a volitelné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-915">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="4fd4a-916">`api-key`: `api-key` Se používá k ověření požadavku na vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-916">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="4fd4a-917">Je hodnotu řetězce pro adresu URL služby jedinečné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-917">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="4fd4a-918">**Vyhledávání** žádosti můžete zadat buď klíč správce, nebo klíč dotazu pro `api-key`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-918">The **Search** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="4fd4a-919">Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-919">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="4fd4a-920">Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-920">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="4fd4a-921">V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-921">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="4fd4a-922">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-922">**Request Body**</span></span>

<span data-ttu-id="4fd4a-923">U metody GET: žádné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-923">For GET: None.</span></span>

<span data-ttu-id="4fd4a-924">Pro metodu POST:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-924">For POST:</span></span>

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

<span data-ttu-id="4fd4a-925">**Pokračování částečné vyhledávání odpovědí**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-925">**Continuation of Partial Search Responses**</span></span>

<span data-ttu-id="4fd4a-926">Někdy Azure Search nemůže vrátit všechny požadované výsledky v odpověď o jedné vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-926">Sometimes Azure Search can't return all the requested results in a single Search response.</span></span> <span data-ttu-id="4fd4a-927">K tomu může dojít různých důvodů, například kdy dotaz vyžadovali příliš mnoho dokumentů není zadáním `$top` nebo zadáte hodnotu `$top` , protože je příliš velký.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-927">This can happen for different reasons, such as when the query requests too many documents by not specifying `$top` or specifying a value for `$top` that is too large.</span></span> <span data-ttu-id="4fd4a-928">V takových případech bude obsahovat Azure Search `@odata.nextLink` poznámky v textu odpovědi a také `@search.nextPageParameters` Pokud byl požadavek POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-928">In such cases, Azure Search will include the `@odata.nextLink` annotation in the response body, and also `@search.nextPageParameters` if it was a POST request.</span></span> <span data-ttu-id="4fd4a-929">Hodnoty těchto poznámek můžete formulovali jiného vyhledávání požadavek na načtení v další části hledání odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-929">You can use the values of these annotations to formulate another Search request to get the next part of the search response.</span></span> <span data-ttu-id="4fd4a-930">Tento postup se nazývá ***pokračování*** původní hledání požadavku a poznámky se obvykle označují jako ***pokračování tokeny***.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-930">This is called a ***continuation*** of the original Search request, and the annotations are generally called ***continuation tokens***.</span></span> <span data-ttu-id="4fd4a-931">V tématu [níže uvedený příklad](#SearchResponse) podrobnosti o syntaxi těchto poznámek a jejich umístění v textu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-931">See [the example below](#SearchResponse) for details on the syntax of these annotations and where they appear in the response body.</span></span> 

<span data-ttu-id="4fd4a-932">Z důvodů, proč Azure Search může vrátit pokračování tokeny jsou specifické pro implementaci a může se změnit.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-932">The reasons why Azure Search might return continuation tokens are implementation-specific and subject to change.</span></span> <span data-ttu-id="4fd4a-933">Robustní klientů musí být vždy připraven pro zpracování případy, kdy jsou vráceny méně dokumenty, než se očekává a je zahrnuté pokračovat v načítání dokumenty token pokračování.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-933">Robust clients should always be ready to handle cases where fewer documents than expected are returned and a continuation token is included to continue retrieving documents.</span></span> <span data-ttu-id="4fd4a-934">Všimněte si také, že musí používat stejnou metodu HTTP jako původní žádost aby bylo možné pokračovat.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-934">Also note that you must use the same HTTP method as the original request in order to continue.</span></span> <span data-ttu-id="4fd4a-935">Například pokud jste odeslali požadavek GET, všechny žádosti pokračování odeslání musíte taky použít GET (a stejně tak pro metodu POST).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-935">For example, if you sent a GET request, any continuation requests you send must also use GET (and likewise for POST).</span></span>

<span data-ttu-id="4fd4a-936"><a name="SearchResponse"></a>
**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-936"><a name="SearchResponse"></a>
**Response**</span></span>

<span data-ttu-id="4fd4a-937">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-937">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

<span data-ttu-id="4fd4a-938">**Příklady:**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-938">**Examples:**</span></span>

<span data-ttu-id="4fd4a-939">Další příklady naleznete na [syntaxe výrazu OData pro službu Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-939">You can find additional examples on the [OData Expression Syntax for Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) page.</span></span>

1)    <span data-ttu-id="4fd4a-940">Vyhledávání v indexu seřazeny sestupně podle data.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-940">Search the Index sorted descending by date.</span></span>

    <span data-ttu-id="4fd4a-941">GET /indexes/hotels/docs? hledání = * & $orderby = lastRenovationDate desc & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-941">GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="4fd4a-942">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "orderby": "lastRenovationDate desc"}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-942">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "orderby": "lastRenovationDate desc" }</span></span>

2)    <span data-ttu-id="4fd4a-943">V rámci Fasetové vyhledávání v rejstříku a načíst omezující vlastnosti pro kategorií, hodnocení, značky, jakož i položky s baseRate v konkrétních oblastech:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-943">In a faceted search, search the index and retrieve facets for categories, rating, tags, as well as items with baseRate in specific ranges:</span></span>

    <span data-ttu-id="4fd4a-944">GET /indexes/hotels/docs? hledání = test & omezující vlastnost = kategorie & omezující vlastnost = hodnocení & omezující vlastnost = značky & omezující vlastnost = baseRate, hodnoty: 80 | 150 | 220 & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-944">GET /indexes/hotels/docs?search=test&facet=category&facet=rating&facet=tags&facet=baseRate,values:80|150|220&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="4fd4a-945">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "test", "omezující vlastnosti": ["kategorie", "hodnocení", "značky", "baseRate, hodnoty: 80 | 150 | 220"]}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-945">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "test", "facets": [ "category", "rating", "tags", "baseRate,values:80|150|220" ] }</span></span>

3)    <span data-ttu-id="4fd4a-946">Pomocí filtru, zúžit předchozí výsledky dotazu Fasetové poté, co uživatel klikne na hodnocení 3 a kategorie "Motel":</span><span class="sxs-lookup"><span data-stu-id="4fd4a-946">Using a filter, narrow down the previous faceted query results after the user clicks on rating 3 and category "Motel":</span></span>

    <span data-ttu-id="4fd4a-947">GET /indexes/hotels/docs? hledání = test & omezující vlastnost = značky & omezující vlastnost = baseRate, hodnoty: 80 | 150 | 220 & $filter = hodnocení eq 3 a kategorie eq "Motel" & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-947">GET /indexes/hotels/docs?search=test&facet=tags&facet=baseRate,values:80|150|220&$filter=rating eq 3 and category eq 'Motel'&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="4fd4a-948">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "test", "omezující vlastnosti": ["značky", "baseRate, hodnoty: 80 | 150 | 220"], "filtr": "hodnocení eq 3 a kategorie eq"Motel""}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-948">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "test", "facets": [ "tags", "baseRate,values:80|150|220" ], "filter": "rating eq 3 and category eq 'Motel'" }</span></span>

4) <span data-ttu-id="4fd4a-949">V rámci Fasetové vyhledávání nastavte horní limit na jedinečný podmínky vrácených dotazem.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-949">In a faceted search, set an upper limit on unique terms returned in a query.</span></span> <span data-ttu-id="4fd4a-950">Výchozí hodnota je 10, ale můžete zvýšit nebo snížit, tato hodnota pomocí `count` parametr na `facet` atribut:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-950">The default is 10, but you can increase or decrease this value using the `count` parameter on the `facet` attribute:</span></span>

    <span data-ttu-id="4fd4a-951">GET /indexes/hotels/docs? hledání = test & omezující vlastnost = města, počet: 5 & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-951">GET /indexes/hotels/docs?search=test&facet=city,count:5&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="4fd4a-952">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "test", "omezující vlastnosti": ["města, počet: 5"]}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-952">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview  {    "search": "test",    "facets": [ "city,count:5" ]  }</span></span>

5)    <span data-ttu-id="4fd4a-953">Vyhledávání v indexu v rámci konkrétních polí; Například pro specifický jazyk pole:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-953">Search the Index within specific fields; For example, a language-specific field:</span></span>

    <span data-ttu-id="4fd4a-954">GET /indexes/hotels/docs? hledání = hôtel & searchFields = description_fr & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-954">GET /indexes/hotels/docs?search=hôtel&searchFields=description_fr&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="4fd4a-955">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "hôtel", "searchFields": "description_fr"}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-955">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "hôtel", "searchFields": "description_fr" }</span></span>

6) <span data-ttu-id="4fd4a-956">Index vyhledávání napříč více polí.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-956">Search the Index across multiple fields.</span></span> <span data-ttu-id="4fd4a-957">Můžete například ukládat a dotaz prohledatelná pole v několika jazycích, všechny ve stejném indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-957">For example, you can store and query searchable fields in multiple languages, all within the same index.</span></span>  <span data-ttu-id="4fd4a-958">Pokud angličtinu a francouzštinu popisy existují společně ve stejném dokumentu, můžete se vrátit některého nebo všech výsledků dotazu:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-958">If English and French descriptions co-exist in the same document, you can return any or all in the query results:</span></span>

    <span data-ttu-id="4fd4a-959">GET /indexes/hotels/docs? hledání = hotelů & searchFields = popis, description_fr & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-959">GET /indexes/hotels/docs?search=hotel&searchFields=description,description_fr&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="4fd4a-960">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "hotelů", "searchFields": "popis, description_fr"}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-960">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview  {    "search": "hotel",    "searchFields": "description, description_fr"  }</span></span>

<span data-ttu-id="4fd4a-961">Všimněte si, že dotaz lze použít pouze jeden index najednou.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-961">Note that you can only query one index at a time.</span></span> <span data-ttu-id="4fd4a-962">Nevytvářejte více indexů pro jednotlivé jazyky, pokud máte v plánu dotazu po jednom.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-962">Do not create multiple indexes for each language unless you plan to query one at a time.</span></span>

7)    <span data-ttu-id="4fd4a-963">Stránkování - Get na 1. stránku položek (velikost stránky je 10):</span><span class="sxs-lookup"><span data-stu-id="4fd4a-963">Paging - Get the 1st page of items (page size is 10):</span></span>

    <span data-ttu-id="4fd4a-964">GET /indexes/hotels/docs? hledání = * & $skip = 0 & $top = 10 & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-964">GET /indexes/hotels/docs?search=*&$skip=0&$top=10&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="4fd4a-965">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "přeskočení": 0, "top": 10}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-965">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "skip": 0, "top": 10 }</span></span>

8)    <span data-ttu-id="4fd4a-966">Stránkování - Get na 2. stránku položek (velikost stránky je 10):</span><span class="sxs-lookup"><span data-stu-id="4fd4a-966">Paging - Get the 2nd page of items (page size is 10):</span></span>

    <span data-ttu-id="4fd4a-967">GET /indexes/hotels/docs? hledání = * & $skip = 10 & $top = 10 & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-967">GET /indexes/hotels/docs?search=*&$skip=10&$top=10&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="4fd4a-968">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "přeskočení": "top" 10: 10}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-968">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "skip": 10, "top": 10 }</span></span>

9)    <span data-ttu-id="4fd4a-969">Načtěte konkrétní sadu pole:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-969">Retrieve a specific set of fields:</span></span>

    <span data-ttu-id="4fd4a-970">GET /indexes/hotels/docs? hledání = * & $select = hotelName, popis a rozhraní api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-970">GET /indexes/hotels/docs?search=*&$select=hotelName,description&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="4fd4a-971">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "Vyberte": "hotelName, popis"}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-971">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "select": "hotelName, description" }</span></span>

10)  <span data-ttu-id="4fd4a-972">Načtení dokumenty odpovídající výraz konkrétní filtru:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-972">Retrieve documents matching a specific filter expression:</span></span>

    <span data-ttu-id="4fd4a-973">GET /indexes/hotels/docs? $filter = (baseRate ge 60 a baseRate lt 300) nebo hotelName eq 'Zvláštní zůstat' & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-973">GET /indexes/hotels/docs?$filter=(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="4fd4a-974">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"filtr": "(baseRate ge 60 a baseRate lt 300) nebo hotelName eq"Zvláštní zůstat""}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-974">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {  "filter": "(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'" }</span></span>

11) <span data-ttu-id="4fd4a-975">Hledání fragmenty index a vraťte se přístupů označuje</span><span class="sxs-lookup"><span data-stu-id="4fd4a-975">Search the index and return fragments with hit highlights</span></span>

    <span data-ttu-id="4fd4a-976">GET /indexes/hotels/docs? hledání = něco & zvýrazněte = Popis & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-976">GET /indexes/hotels/docs?search=something&highlight=description&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="4fd4a-977">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "něco co", "highlight": "Popis"}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-977">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "highlight": "description" }</span></span>

12) <span data-ttu-id="4fd4a-978">Vyhledávání v indexu a vrátit dokumenty seřadit z blíže dále od odkazem na umístění</span><span class="sxs-lookup"><span data-stu-id="4fd4a-978">Search the index and return documents sorted from closer to farther away from a reference location</span></span>

    <span data-ttu-id="4fd4a-979">GET /indexes/hotels/docs? vyhledávání něco = & $orderby=geo.distance (umístění, geography'POINT(-122.12315 47.88121)') & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-979">GET /indexes/hotels/docs?search=something&$orderby=geo.distance(location, geography'POINT(-122.12315 47.88121)')&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="4fd4a-980">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "něco co", "orderby": "geo.distance (umístění, geography'POINT(-122.12315 47.88121)')"}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-980">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "orderby": "geo.distance(location, geography'POINT(-122.12315 47.88121)')" }</span></span>

13) <span data-ttu-id="4fd4a-981">Vyhledávání v indexu za předpokladu, že je profil vyhodnocování názvem "geo" s dvě funkce pro výpočet skóre podle vzdálenosti, jeden definování parametr s názvem "currentLocation" a jeden definování parametr s názvem "lastLocation"</span><span class="sxs-lookup"><span data-stu-id="4fd4a-981">Search the index assuming there's a scoring profile called "geo" with two distance scoring functions, one defining a parameter called "currentLocation" and one defining a parameter called "lastLocation"</span></span>

    <span data-ttu-id="4fd4a-982">GET /indexes/hotels/docs? vyhledávání něco = & scoringProfile = geograficky & scoringParameter = currentLocation – 122.123,44.77233 & scoringParameter = lastLocation – 121.499,44.2113 & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-982">GET /indexes/hotels/docs?search=something&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&scoringParameter=lastLocation--121.499,44.2113&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="4fd4a-983">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "něco co", "scoringProfile": "geo", "scoringParameters": ["currentLocation – 122.123,44.77233", "lastLocation – 121.499,44.2113"]}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-983">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "scoringProfile": "geo",   "scoringParameters": [ "currentLocation--122.123,44.77233", "lastLocation--121.499,44.2113" ] }</span></span>

14) <span data-ttu-id="4fd4a-984">Hledání dokumentů do indexu pomocí [jednoduchá syntaxe dotazů](https://msdn.microsoft.com/library/dn798920.aspx).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-984">Find documents in the index using [simple query syntax](https://msdn.microsoft.com/library/dn798920.aspx).</span></span> <span data-ttu-id="4fd4a-985">Tento dotaz vrací hotels kde prohledatelná pole obsahovat podmínky "pohodlí" a "umístění", ale ne "motel":</span><span class="sxs-lookup"><span data-stu-id="4fd4a-985">This query returns hotels where searchable fields contain the terms "comfort" and "location" but not "motel":</span></span>

    <span data-ttu-id="4fd4a-986">GET /indexes/hotels/docs? hledání = pohodlí + umístění-motel & searchMode = all & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="4fd4a-986">GET /indexes/hotels/docs?search=comfort +location -motel&searchMode=all&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="4fd4a-987">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "pohodlí + umístění-motel", "searchMode": "vše"}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-987">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "comfort +location -motel",   "searchMode": "all" }</span></span>

<span data-ttu-id="4fd4a-988">Všimněte si použití `searchMode=all` výše.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-988">Note the use of `searchMode=all` above.</span></span> <span data-ttu-id="4fd4a-989">Tento parametr včetně přepíše výchozí hodnoty `searchMode=any`, zajistit který `-motel` znamená "A není" místo "Nebo není".</span><span class="sxs-lookup"><span data-stu-id="4fd4a-989">Including this parameter overrides the default of `searchMode=any`, ensuring that `-motel` means "AND NOT" instead of "OR NOT".</span></span> <span data-ttu-id="4fd4a-990">Bez `searchMode=all`, získáte "Nebo není" který rozbalí než omezuje výsledky hledání, a to může být counter-intuitive u některých uživatelů.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-990">Without `searchMode=all`, you get "OR NOT" which expands rather than restricts search results, and this can be counter-intuitive to some users.</span></span>

15) <span data-ttu-id="4fd4a-991">Hledání dokumentů do indexu pomocí [syntaxe dotazů lucene](https://msdn.microsoft.com/library/mt589323.aspx).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-991">Find documents in the index using [lucene query syntax](https://msdn.microsoft.com/library/mt589323.aspx).</span></span> <span data-ttu-id="4fd4a-992">Tento dotaz vrací hotels, kde pole kategorie obsahuje termín "rozpočet" a všechna prohledatelná pole obsahující frázi "nedávno renovovanou".</span><span class="sxs-lookup"><span data-stu-id="4fd4a-992">This query returns hotels where the category field contains the term "budget" and all searchable fields containing the phrase "recently renovated".</span></span> <span data-ttu-id="4fd4a-993">Dokumenty obsahující frázi "nedávno renovovanou" jsou řazeny výše v důsledku hodnotu nárůst termín (3)</span><span class="sxs-lookup"><span data-stu-id="4fd4a-993">Documents containing the phrase "recently renovated" are ranked higher as a result of the term boost value (3)</span></span>

    <span data-ttu-id="4fd4a-994">GET /indexes/hotels/docs? hledání = kategorie: nároky a \"nedávno renovovanou\"^ 3 & searchMode = all & verze api-version = 2015-02-28-Preview & Typ úplné =</span><span class="sxs-lookup"><span data-stu-id="4fd4a-994">GET /indexes/hotels/docs?search=category:budget AND \"recently renovated\"^3&searchMode=all&api-version=2015-02-28-Preview&querytype=full</span></span>

    <span data-ttu-id="4fd4a-995">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "kategorie: nároky a \"nedávno renovovanou\"^ 3", "typ": "úplná", "searchMode": "vše"}</span><span class="sxs-lookup"><span data-stu-id="4fd4a-995">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "category:budget AND \"recently renovated\"^3",   "queryType": "full",   "searchMode": "all" }</span></span>

<a name="LookupAPI"></a>

## <a name="lookup-document"></a><span data-ttu-id="4fd4a-996">Vyhledávání dokumentů</span><span class="sxs-lookup"><span data-stu-id="4fd4a-996">Lookup Document</span></span>
<span data-ttu-id="4fd4a-997">**Vyhledávání dokumentů** operace dokumentu načte z Azure Search.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-997">The **Lookup Document** operation retrieves a document from Azure Search.</span></span> <span data-ttu-id="4fd4a-998">To je užitečné, když uživatel klikne na konkrétní výsledek a chcete vyhledat konkrétní podrobnosti o tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-998">This is useful when a user clicks on a specific search result, and you want to look up specific details about that document.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

<span data-ttu-id="4fd4a-999">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-999">**Request**</span></span>

<span data-ttu-id="4fd4a-1000">Protokol HTTPS je vyžadována pro žádosti o služby.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1000">HTTPS is required for service requests.</span></span> <span data-ttu-id="4fd4a-1001">**Vyhledávání dokumentů** požadavek konstruovat následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1001">The **Lookup Document** request can be constructed as follows.</span></span>

    GET /indexes/[index name]/docs/key?[query parameters]

<span data-ttu-id="4fd4a-1002">Alternativně můžete tradiční syntaxe OData pro vyhledávání klíčů:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1002">Alternatively, you can use the traditional OData syntax for key lookup:</span></span>

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

<span data-ttu-id="4fd4a-1003">Identifikátoru URI požadavku zahrnuje [název indexu] a [klíče], který dokument k načtení z index, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1003">The request URI includes an [index name] and [key], specifying which document to retrieve from which index.</span></span> <span data-ttu-id="4fd4a-1004">Najednou můžete získat pouze jeden dokument.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1004">You can only get one document at a time.</span></span> <span data-ttu-id="4fd4a-1005">Použití **vyhledávání** získat více dokumentů v jedné žádosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1005">Use **Search** to get multiple documents in a single request.</span></span>

<span data-ttu-id="4fd4a-1006">**Parametry dotazu**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1006">**Query Parameters**</span></span>

<span data-ttu-id="4fd4a-1007">`$select=[string]`(volitelné) – seznam polí, oddělených čárkami k načtení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1007">`$select=[string]` (optional) - a list of comma-separated fields to retrieve.</span></span> <span data-ttu-id="4fd4a-1008">Pokud tento parametr nezadáte, nebo nastavte `*`, všechna pole, které jsou označeny jako získat ve schématu jsou zahrnuty v projekci.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1008">If unspecified or set to `*`, all fields marked as retrievable in the schema are included in the projection.</span></span>

<span data-ttu-id="4fd4a-1009">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1009">`api-version=[string]` (required).</span></span> <span data-ttu-id="4fd4a-1010">Verze preview je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1010">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="4fd4a-1011">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1011">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="4fd4a-1012">Poznámka: pro tuto operaci `api-version` je zadána jako parametr dotazu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1012">Note: For this operation, the `api-version` is specified as a query parameter.</span></span>

<span data-ttu-id="4fd4a-1013">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1013">**Request Headers**</span></span>

<span data-ttu-id="4fd4a-1014">Následující seznam popisuje hlavičky žádosti požadované a volitelné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1014">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="4fd4a-1015">`api-key`: `api-key` Se používá k ověření požadavku na vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1015">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="4fd4a-1016">Je hodnotu řetězce pro adresu URL služby jedinečné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1016">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="4fd4a-1017">**Vyhledávání dokumentů** žádosti můžete zadat buď klíč správce, nebo klíč dotazu pro `api-key`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1017">The **Lookup Document** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="4fd4a-1018">Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1018">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="4fd4a-1019">Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1019">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="4fd4a-1020">V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1020">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="4fd4a-1021">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1021">**Request Body**</span></span>

<span data-ttu-id="4fd4a-1022">Žádné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1022">None.</span></span>

<span data-ttu-id="4fd4a-1023">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1023">**Response**</span></span>

<span data-ttu-id="4fd4a-1024">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1024">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      field_name: field_value (fields matching the default or specified projection)
    }

<span data-ttu-id="4fd4a-1025">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1025">**Example**</span></span>

<span data-ttu-id="4fd4a-1026">Vyhledávání dokumentu, který má klíč: 2.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1026">Lookup the document that has key '2'</span></span>

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

<span data-ttu-id="4fd4a-1027">Vyhledávání dokumentu, který má klíč pomocí syntaxe OData 3:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1027">Lookup the document that has key '3' using OData syntax:</span></span>

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>

## <a name="count-documents"></a><span data-ttu-id="4fd4a-1028">Počet dokumentů</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1028">Count Documents</span></span>
<span data-ttu-id="4fd4a-1029">**Počet dokumentů** operace načte počet dokumentů v indexu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1029">The **Count Documents** operation retrieves a count of the number of documents in a search index.</span></span> <span data-ttu-id="4fd4a-1030">`$count` Syntaxe je součástí protokolu OData.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1030">The `$count` syntax is part of the OData protocol.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

<span data-ttu-id="4fd4a-1031">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1031">**Request**</span></span>

<span data-ttu-id="4fd4a-1032">Protokol HTTPS je vyžadována pro žádosti o služby.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1032">HTTPS is required for service requests.</span></span> <span data-ttu-id="4fd4a-1033">**Počet dokumentů** požadavek se dá vytvořit pomocí metody GET.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1033">The **Count Documents** request can be constructed using the GET method.</span></span>

<span data-ttu-id="4fd4a-1034">[Název indexu] v identifikátoru URI požadavku informuje službu, kterou chcete vrátit počet všechny položky v kolekci dokumenty, které se zadaným indexem.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1034">The [index name] in the request URI tells the service to return a count of all items in the docs collection of the specified index.</span></span>

<span data-ttu-id="4fd4a-1035">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1035">`api-version=[string]` (required).</span></span> <span data-ttu-id="4fd4a-1036">Verze preview je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1036">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="4fd4a-1037">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1037">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="4fd4a-1038">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1038">**Request Headers**</span></span>

<span data-ttu-id="4fd4a-1039">Následující seznam popisuje hlavičky žádosti požadované a volitelné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1039">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="4fd4a-1040">`Accept`: Tato hodnota musí být nastavena na `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1040">`Accept`: This value must be set to `text/plain`.</span></span>
* <span data-ttu-id="4fd4a-1041">`api-key`: `api-key` Se používá k ověření požadavku na vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1041">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="4fd4a-1042">Je hodnotu řetězce pro adresu URL služby jedinečné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1042">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="4fd4a-1043">**Počet dokumentů** žádosti můžete zadat buď klíč správce, nebo klíč dotazu pro `api-key`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1043">The **Count Documents** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="4fd4a-1044">Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1044">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="4fd4a-1045">Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1045">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="4fd4a-1046">V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1046">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="4fd4a-1047">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1047">**Request Body**</span></span>

<span data-ttu-id="4fd4a-1048">Žádné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1048">None.</span></span>

<span data-ttu-id="4fd4a-1049">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1049">**Response**</span></span>

<span data-ttu-id="4fd4a-1050">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1050">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="4fd4a-1051">Text odpovědi obsahuje hodnotu počtu jako celé číslo ve formátu v prostém textu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1051">The response body contains the count value as an integer formatted in plain text.</span></span>

<a name="Suggestions"></a>

## <a name="suggestions"></a><span data-ttu-id="4fd4a-1052">Návrhy</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1052">Suggestions</span></span>
<span data-ttu-id="4fd4a-1053">**Návrhy** operace načte návrhy založené na vstupu částečné vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1053">The **Suggestions** operation retrieves suggestions based on partial search input.</span></span> <span data-ttu-id="4fd4a-1054">Obvykle se používá v oknech vyhledávání zajistit našeptávání návrhy, jak uživatelé zadávají hledaný text.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1054">It's typically used in search boxes to provide type-ahead suggestions as users are entering search terms.</span></span>

<span data-ttu-id="4fd4a-1055">Požadavky na návrh cílem návrhy cílové dokumenty, tak navrhované text může být opakována v případě více dokumentů candidate odpovídají stejné hledání vstup.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1055">Suggestion requests aim at suggesting target documents, so the suggested text may be repeated if multiple candidate documents match the same search input.</span></span> <span data-ttu-id="4fd4a-1056">Můžete použít `$select` pro načtení další pole dokumentu (včetně klíč dokumentu), aby se dá zjistit, který dokument je zdrojem pro každý návrhu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1056">You can use `$select` to retrieve other document fields (including the document key) so that you can tell which document is the source for each suggestion.</span></span>

<span data-ttu-id="4fd4a-1057">A **návrhy** operaci se objeví jako požadavek GET nebo POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1057">A **Suggestions** operation is issued as a GET or POST request.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

<span data-ttu-id="4fd4a-1058">**Kdy použít POST místo GET**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1058">**When to use POST instead of GET**</span></span>

<span data-ttu-id="4fd4a-1059">Při použití metody GET protokolu HTTP k volání **návrhy** rozhraní API, musíte vědět, že nesmí překročit délka adresy URL žádosti 8 KB.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1059">When you use HTTP GET to call the **Suggestions** API, you need to be aware that the length of the request URL cannot exceed 8 KB.</span></span> <span data-ttu-id="4fd4a-1060">Je to obvykle dostatečně pro většinu aplikací.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1060">This is usually enough for most applications.</span></span> <span data-ttu-id="4fd4a-1061">Některé aplikace však vytvořit velmi velké dotazy, konkrétně výrazy filtru OData.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1061">However, some applications produce very large queries, specifically OData filter expressions.</span></span> <span data-ttu-id="4fd4a-1062">Pro tyto aplikace pomocí HTTP POST je lepší volbou, protože umožňuje větší filtry než metoda GET.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1062">For these applications, using HTTP POST is a better choice because it allows larger filters than GET.</span></span> <span data-ttu-id="4fd4a-1063">S POST počet klauzulí ve filtru je omezující faktor, ne podle velikosti vytvořeného řetězec nezpracovaná filtru vzhledem k tomu, že je přibližně 16 MB omezení velikosti pro metodu POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1063">With POST, the number of clauses in a filter is the limiting factor, not the size of the raw filter string since the request size limit for POST is approximately 16 MB.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-1064">I když maximální velikost požadavku POST je velmi velké, nemůže být libovolně komplexní výrazech filtru.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1064">Even though the POST request size limit is very large, filter expressions cannot be arbitrarily complex.</span></span> <span data-ttu-id="4fd4a-1065">V tématu [syntaxe výrazu OData](https://msdn.microsoft.com/library/dn798921.aspx) pro další informace o omezeních složitost filtru.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1065">See [OData expression syntax](https://msdn.microsoft.com/library/dn798921.aspx) for more information about filter complexity limitations.</span></span>
> 
> 

<span data-ttu-id="4fd4a-1066">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1066">**Request**</span></span>

<span data-ttu-id="4fd4a-1067">Protokol HTTPS je vyžadována pro žádosti o služby.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1067">HTTPS is required for service requests.</span></span> <span data-ttu-id="4fd4a-1068">**Návrhy** požadavek se dá vytvořit pomocí metody GET nebo POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1068">The **Suggestions** request can be constructed using the GET or POST methods.</span></span>

<span data-ttu-id="4fd4a-1069">Identifikátoru URI požadavku určuje název indexu dotazu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1069">The request URI specifies the name of the index to query.</span></span> <span data-ttu-id="4fd4a-1070">V řetězci dotazu v případě požadavky GET jsou zadány parametry, jako je například částečně vstupní hledaný termín, a v žádosti subjekt v případě POST požadavky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1070">Parameters, such as the partially input search term, are specified on the query string in the case of GET requests, and in the request body in the case of POST requests.</span></span>

<span data-ttu-id="4fd4a-1071">Jako osvědčený postup při vytváření požadavky GET, nezapomeňte [kódování URL](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) parametry specifického dotazu při přímé volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1071">As a best practice when creating GET requests, remember to [URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specific query parameters when calling the REST API directly.</span></span> <span data-ttu-id="4fd4a-1072">Pro **návrhy** operace, to zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1072">For **Suggestions** operations, this includes:</span></span>

* `$filter`
* `highlightPreTag`
* `highlightPostTag`
* `search`

<span data-ttu-id="4fd4a-1073">Kódování URL se doporučuje jenom na výše uvedené parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1073">URL encoding is only recommended on the above query parameters.</span></span> <span data-ttu-id="4fd4a-1074">Pokud jste omylem kódování URL řetězec dotazu celý (vše za?), by došlo k přerušení požadavky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1074">If you inadvertently URL-encode the entire query string (everything after the ?), requests will break.</span></span>

<span data-ttu-id="4fd4a-1075">Navíc kódování URL je nutné pouze při volání rozhraní REST API přímo pomocí GET.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1075">Also, URL encoding is only necessary when calling the REST API directly using GET.</span></span> <span data-ttu-id="4fd4a-1076">Žádné kódování URL je nezbytné při volání metody **návrhy** pomocí POST, nebo při použití [klientské knihovny .NET](https://msdn.microsoft.com/library/dn951165.aspx), který zpracovává kódování URL za vás.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1076">No URL encoding is necessary when calling **Suggestions** using POST, or when using the [.NET client library](https://msdn.microsoft.com/library/dn951165.aspx), which handles URL encoding for you.</span></span>

<span data-ttu-id="4fd4a-1077">**Parametry dotazu**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1077">**Query Parameters**</span></span>

<span data-ttu-id="4fd4a-1078">**Návrhy** přijímá několik parametrů, které poskytují kritéria dotazu a také určit chování vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1078">**Suggestions** accepts several parameters that provide query criteria and also specify search behavior.</span></span> <span data-ttu-id="4fd4a-1079">Zadejte tyto parametry v adrese URL řetězec dotazu při volání metody **návrhy** prostřednictvím GET a jako vlastnosti JSON v textu požadavku při volání metody **návrhy** přes POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1079">You provide these parameters in the URL query string when calling **Suggestions** via GET, and as JSON properties in the request body when calling **Suggestions** via POST.</span></span> <span data-ttu-id="4fd4a-1080">Syntaxe pro některé parametry se mírně liší mezi GET a POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1080">The syntax for some parameters is slightly different between GET and POST.</span></span> <span data-ttu-id="4fd4a-1081">Tyto rozdíly jsou popsány podle vhodnosti níže:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1081">These differences are noted as applicable below:</span></span>

<span data-ttu-id="4fd4a-1082">`search=[string]`-vyhledávání text, který má používat pro návrh dotazy.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1082">`search=[string]` - the search text to use to suggest queries.</span></span> <span data-ttu-id="4fd4a-1083">Musí být minimálně 1 znak a maximálně 100 znaků.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1083">Must be at least 1 character, and no more than 100 characters.</span></span>

<span data-ttu-id="4fd4a-1084">`highlightPreTag=[string]`(volitelné) – řetězec značek, které přidá k vyhledání přístupů.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1084">`highlightPreTag=[string]` (optional) - a string tag that prepends to search hits.</span></span> <span data-ttu-id="4fd4a-1085">Musí být nastavena s `highlightPostTag`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1085">Must be set with `highlightPostTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-1086">Při volání metody **návrhy** pomocí GET, vyhrazené znaky v adrese URL musí být kódovaný v procentech (například místo # % 23).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1086">When calling **Suggestions** using GET, reserved characters in the URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="4fd4a-1087">`highlightPostTag=[string]`(volitelné) – řetězec značek, které připojí k vyhledání přístupů.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1087">`highlightPostTag=[string]` (optional) - a string tag that appends to search hits.</span></span> <span data-ttu-id="4fd4a-1088">Musí být nastavena s `highlightPreTag`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1088">Must be set with `highlightPreTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-1089">Při volání metody **návrhy** pomocí GET, vyhrazené znaky v adrese URL musí být kódovaný v procentech (například místo # % 23).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1089">When calling **Suggestions** using GET, reserved characters in the URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="4fd4a-1090">`suggesterName=[string]`-Název modulu pro návrhy zadané v `suggesters` kolekce, která je součástí definice indexu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1090">`suggesterName=[string]` - the name of the suggester as specified in the `suggesters` collection that's part of the index definition.</span></span> <span data-ttu-id="4fd4a-1091">A `suggester` určuje pole, která hledat podmínky navrhované dotazu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1091">A `suggester` determines which fields are scanned for suggested query terms.</span></span> <span data-ttu-id="4fd4a-1092">V tématu [trochu](#Suggesters) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1092">See [Suggesters](#Suggesters) for details.</span></span>

<span data-ttu-id="4fd4a-1093">`fuzzy=[boolean]`(volitelné, výchozí = false) – Pokud je vlastnost nastavena na hodnotu true, toto rozhraní API najdete návrhy i tehdy, je nahrazovanou nebo chybí znak v hledaný text.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1093">`fuzzy=[boolean]` (optional, default = false) - when set to true this API will find suggestions even if there's a substituted or missing character in the search text.</span></span> <span data-ttu-id="4fd4a-1094">Zatímco to poskytuje lepší podmínky v některých scénářích kompromisnímu výkon, protože vyhledávání s fuzzy logikou návrhu pomalejší a spotřebovávat více prostředků.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1094">While this provides a better experience in some scenarios it comes at a performance cost as fuzzy suggestion searches are slower and consume more resources.</span></span>

<span data-ttu-id="4fd4a-1095">`searchFields=[string]`(volitelné) – seznam názvů oddělených čárkami pole pro vyhledávání pro zadaný hledaný text.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1095">`searchFields=[string]` (optional) - the list of comma-separated field names to search for the specified search text.</span></span> <span data-ttu-id="4fd4a-1096">Cílová pole. musí být povolen pro návrhy.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1096">Target fields must be enabled for suggestions.</span></span>

<span data-ttu-id="4fd4a-1097">`$top=#`(volitelné, výchozí = 5)-počet návrhy pro načtení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1097">`$top=#` (optional, default = 5) - the number of suggestions to retrieve.</span></span> <span data-ttu-id="4fd4a-1098">Musí být číslo v rozsahu od 1 do 100.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1098">Must be a number between 1 and 100.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-1099">Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `top` místo `$top`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1099">When calling **Suggestions** using POST, this parameter is named `top` instead of `$top`.</span></span>
> 
> 

<span data-ttu-id="4fd4a-1100">`$filter=[string]`(volitelné) – výraz, který filtruje dokumenty za návrhy.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1100">`$filter=[string]` (optional) - an expression that filters the documents considered for suggestions.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-1101">Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `filter` místo `$filter`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1101">When calling **Suggestions** using POST, this parameter is named `filter` instead of `$filter`.</span></span>
> 
> 

<span data-ttu-id="4fd4a-1102">`$orderby=[string]`(volitelné) – seznam výrazů textový soubor s oddělovači na výsledky seřaďte podle.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1102">`$orderby=[string]` (optional) - a list of comma-separated expressions to sort the results by.</span></span> <span data-ttu-id="4fd4a-1103">Každý výraz může být název pole nebo volání `geo.distance()` funkce.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1103">Each expression can be either a field name or a call to the `geo.distance()` function.</span></span> <span data-ttu-id="4fd4a-1104">Každý výraz může následovat `asc` uvést vzestupné řazení a `desc` k označení sestupném.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1104">Each expression can be followed by `asc` to indicated ascending, and `desc` to indicate descending.</span></span> <span data-ttu-id="4fd4a-1105">Výchozí hodnota je vzestupné pořadí.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1105">The default is ascending order.</span></span> <span data-ttu-id="4fd4a-1106">Existuje limit 32 klauzule pro `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1106">There is a limit of 32 clauses for `$orderby`.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-1107">Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `orderby` místo `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1107">When calling **Suggestions** using POST, this parameter is named `orderby` instead of `$orderby`.</span></span>
> 
> 

<span data-ttu-id="4fd4a-1108">`$select=[string]`(volitelné) – seznam polí, oddělených čárkami k načtení.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1108">`$select=[string]` (optional) - a list of comma-separated fields to retrieve.</span></span> <span data-ttu-id="4fd4a-1109">Pokud tento parametr nezadáte, pouze dokumentu klíč a návrhu je vrácen.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1109">If unspecified, only the document key and suggestion text is returned.</span></span> <span data-ttu-id="4fd4a-1110">Všechna pole můžete explicitně žádostí nastavením tohoto parametru na `*`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1110">You can explicitly request all fields by setting this parameter to `*`.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-1111">Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `select` místo `$select`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1111">When calling **Suggestions** using POST, this parameter is named `select` instead of `$select`.</span></span>
> 
> 

<span data-ttu-id="4fd4a-1112">`minimumCoverage`(volitelné, výchozí hodnota je 80)-číslo mezi 0 a 100 procentuální hodnota indexu, která musí být předmětem dotazu návrhy v pořadí pro dotaz na hlášené jako úspěšné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1112">`minimumCoverage` (optional, defaults to 80) - a number between 0 and 100 indicating the percentage of the index that must be covered by a suggestions query in order for the query to be reported as a success.</span></span> <span data-ttu-id="4fd4a-1113">Ve výchozím nastavení, musí být k dispozici alespoň 80 % indexu nebo `Suggest` vrátí stavový kód protokolu HTTP 503.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1113">By default, at least 80% of the index must be available or `Suggest` will return HTTP status code 503.</span></span> <span data-ttu-id="4fd4a-1114">Pokud nastavíte `minimumCoverage` a `Suggest` úspěšné, se vrátí HTTP 200 a zahrnout `@search.coverage` hodnotu v odpovědi procentuální hodnota indexu, pro který je zahrnutý v dotazu.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1114">If you set `minimumCoverage` and `Suggest` succeeds, it will return HTTP 200 and include a `@search.coverage` value in the response indicating the percentage of the index that was included in the query.</span></span>

> [!NOTE]
> <span data-ttu-id="4fd4a-1115">Nastavení tohoto parametru na hodnotu nižší než 100 může být užitečná pro zajištění dostupnosti vyhledávání i pro služby s pouze jednu repliku.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1115">Setting this parameter to a value lower than 100 can be useful for ensuring search availability even for services with only one replica.</span></span> <span data-ttu-id="4fd4a-1116">Ne všechny odpovídající návrh je však zaručena nacházet ve výsledcích.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1116">However, not all matching suggestions are guaranteed to be present in the results.</span></span> <span data-ttu-id="4fd4a-1117">Pokud se pro vaše aplikace důležitější než dostupnosti pro vyvolání, pak je nejlepší nižší `minimumCoverage` pod jeho výchozí hodnota 80.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1117">If recall is more important to your application than availability, then it's best not to lower `minimumCoverage` below its default value of 80.</span></span>
> 
> 

<span data-ttu-id="4fd4a-1118">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1118">`api-version=[string]` (required).</span></span> <span data-ttu-id="4fd4a-1119">Verze preview je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1119">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="4fd4a-1120">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1120">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="4fd4a-1121">Poznámka: pro tuto operaci `api-version` je zadána jako parametr dotazu v adrese URL bez ohledu na to, jestli volání **návrhy** s GET nebo POST.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1121">Note: For this operation, the `api-version` is specified as a query parameter in the URL regardless of whether you call **Suggestions** with GET or POST.</span></span>

<span data-ttu-id="4fd4a-1122">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1122">**Request Headers**</span></span>

<span data-ttu-id="4fd4a-1123">Následující seznam popisuje hlavičky žádosti požadované a volitelné</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1123">The following list describes the required and optional request headers</span></span>

* <span data-ttu-id="4fd4a-1124">`api-key`: `api-key` Se používá k ověření požadavku na vaši službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1124">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="4fd4a-1125">Je hodnotu řetězce pro adresu URL služby jedinečné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1125">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="4fd4a-1126">**Návrhy** žádosti můžete zadat buď klíč správce, nebo klíč dotazu, jako `api-key`.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1126">The **Suggestions** request can specify either an admin key or query key as the `api-key`.</span></span>

<span data-ttu-id="4fd4a-1127">Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1127">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="4fd4a-1128">Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1128">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="4fd4a-1129">V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1129">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="4fd4a-1130">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1130">**Request Body**</span></span>

<span data-ttu-id="4fd4a-1131">U metody GET: žádné.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1131">For GET: None.</span></span>

<span data-ttu-id="4fd4a-1132">Pro metodu POST:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1132">For POST:</span></span>

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

<span data-ttu-id="4fd4a-1133">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1133">**Response**</span></span>

<span data-ttu-id="4fd4a-1134">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1134">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

<span data-ttu-id="4fd4a-1135">Pokud možnost projekce slouží k načtení pole, která jsou součástí každý element pole:</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1135">If the projection option is used to retrieve fields they are included in each element of the array:</span></span>

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

<span data-ttu-id="4fd4a-1136">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1136">**Example**</span></span>

<span data-ttu-id="4fd4a-1137">Načtení 5 návrhy, kde je vstupní částečné vyhledávání 'lux.</span><span class="sxs-lookup"><span data-stu-id="4fd4a-1137">Retrieve 5 suggestions where the partial search input is 'lux'</span></span>

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
