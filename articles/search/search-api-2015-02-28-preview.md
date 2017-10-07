---
title: "aaaAzure vyhledávání REST API verze 2015-02-28-verze Preview služby | Microsoft Docs"
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
ms.openlocfilehash: 63cb30b7f43a42552c6744ea087afea947857224
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a><span data-ttu-id="ba4ea-103">Rozhraní API REST služby vyhledávání systému Azure: Verze 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-103">Azure Search Service REST API: Version 2015-02-28-Preview</span></span>
<span data-ttu-id="ba4ea-104">Tento článek je hello referenční dokumentaci k nástroji pro `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-104">This article is hello reference documentation for `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="ba4ea-105">Tato verze preview rozšiřuje hello aktuální verzi všeobecně dostupná, [rozhraní api-version = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), tím, že poskytuje hello následující povolenými experimentálními funkcemi:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-105">This preview extends hello current generally available version, [api-version=2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), by providing hello following experimental features:</span></span>

* <span data-ttu-id="ba4ea-106">`moreLikeThis`parametr v hello dotazu [vyhledávání dokumentů](#SearchDocs) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-106">`moreLikeThis` query parameter in hello [Search Documents](#SearchDocs) API.</span></span> <span data-ttu-id="ba4ea-107">Najde jiné dokumenty, které jsou relevantní tooanother určitého dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-107">It finds other documents that are relevant tooanother specific document.</span></span>

<span data-ttu-id="ba4ea-108">Několik dalších částí hello `2015-02-28-Preview` REST API popsané samostatně.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-108">A few additional parts of hello `2015-02-28-Preview` REST API are documented separately.</span></span> <span data-ttu-id="ba4ea-109">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-109">These include:</span></span>

* [<span data-ttu-id="ba4ea-110">Vyhodnocování profily</span><span class="sxs-lookup"><span data-stu-id="ba4ea-110">Scoring Profiles</span></span>](search-api-scoring-profiles-2015-02-28-preview.md)
* [<span data-ttu-id="ba4ea-111">Indexery</span><span class="sxs-lookup"><span data-stu-id="ba4ea-111">Indexers</span></span>](search-api-indexers-2015-02-28-preview.md)

<span data-ttu-id="ba4ea-112">Služba Azure Search je k dispozici v několika verzích.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-112">Azure Search service is available in multiple versions.</span></span> <span data-ttu-id="ba4ea-113">Naleznete příliš[verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-113">Please refer too[Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details.</span></span>

## <a name="apis-in-this-document"></a><span data-ttu-id="ba4ea-114">Rozhraní API v tomto dokumentu</span><span class="sxs-lookup"><span data-stu-id="ba4ea-114">APIs in this document</span></span>
<span data-ttu-id="ba4ea-115">Rozhraní API služby Azure Search podporuje dva syntaxe adresy URL pro operace rozhraní API: jednoduchý a OData (viz [podporu pro OData (API služby Azure Search)](http://msdn.microsoft.com/library/azure/dn798932.aspx) podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-115">Azure Search service API supports two URL syntaxes for API operations: simple and OData (see [Support for OData (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) for details).</span></span> <span data-ttu-id="ba4ea-116">Hello následující seznam obsahuje jednoduchý syntaktický hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-116">hello following list shows hello simple syntax.</span></span>

[<span data-ttu-id="ba4ea-117">Vytvoření indexu</span><span class="sxs-lookup"><span data-stu-id="ba4ea-117">Create Index</span></span>](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[<span data-ttu-id="ba4ea-118">Aktualizovat Index</span><span class="sxs-lookup"><span data-stu-id="ba4ea-118">Update Index</span></span>](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="ba4ea-119">Získat Index</span><span class="sxs-lookup"><span data-stu-id="ba4ea-119">Get Index</span></span>](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="ba4ea-120">Výpis indexy</span><span class="sxs-lookup"><span data-stu-id="ba4ea-120">Listing Indexes</span></span>](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[<span data-ttu-id="ba4ea-121">Získat statistiku indexu</span><span class="sxs-lookup"><span data-stu-id="ba4ea-121">Get Index Statistics</span></span>](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[<span data-ttu-id="ba4ea-122">Analyzátor testu</span><span class="sxs-lookup"><span data-stu-id="ba4ea-122">Test Analyzer</span></span>](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[<span data-ttu-id="ba4ea-123">Odstranění indexu</span><span class="sxs-lookup"><span data-stu-id="ba4ea-123">Delete an Index</span></span>](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="ba4ea-124">Přidat, odstranit a aktualizovat Data v indexu</span><span class="sxs-lookup"><span data-stu-id="ba4ea-124">Add, Delete, and Update Data within an Index</span></span>](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[<span data-ttu-id="ba4ea-125">Vyhledávání dokumentů</span><span class="sxs-lookup"><span data-stu-id="ba4ea-125">Search Documents</span></span>](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[<span data-ttu-id="ba4ea-126">Vyhledávání dokumentů</span><span class="sxs-lookup"><span data-stu-id="ba4ea-126">Lookup Document</span></span>](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[<span data-ttu-id="ba4ea-127">Počet dokumentů</span><span class="sxs-lookup"><span data-stu-id="ba4ea-127">Count Documents</span></span>](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[<span data-ttu-id="ba4ea-128">Návrhy</span><span class="sxs-lookup"><span data-stu-id="ba4ea-128">Suggestions</span></span>](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

- - -
<a name="IndexOps"></a>

## <a name="index-operations"></a><span data-ttu-id="ba4ea-129">Operace indexu</span><span class="sxs-lookup"><span data-stu-id="ba4ea-129">Index Operations</span></span>
<span data-ttu-id="ba4ea-130">Můžete vytvořit a spravovat indexy ve službě Azure Search pomocí jednoduchých požadavků HTTP (POST, GET, PUT, DELETE) s daným indexem prostředků.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-130">You can create and manage indexes in Azure Search service via simple HTTP requests (POST, GET, PUT, DELETE) against a given index resource.</span></span> <span data-ttu-id="ba4ea-131">toocreate na index, nejprve POST dokument JSON, který popisuje schéma indexu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-131">toocreate an index, you first POST a JSON document that describes hello index schema.</span></span> <span data-ttu-id="ba4ea-132">Hello schéma definuje pole hello hello index, jejich datové typy a jejich použití (například v fulltextové vyhledávání, řazení a filtrů používání omezujících vlastností).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-132">hello schema defines hello fields of hello index, their data types, and how they can be used (for example, in full-text searches, filters, sorting, or faceting).</span></span> <span data-ttu-id="ba4ea-133">Definuje také vyhodnocování profily, trochu a další atributy tooconfigure hello chování hello indexu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-133">It also defines scoring profiles, suggesters and other attributes tooconfigure hello behavior of hello index.</span></span>

<span data-ttu-id="ba4ea-134">Hello následující příklad uvádí ukázku schématu, používá pro vyhledávání na hotelů informace s pole s popisem hello definované ve dvou jazycích.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-134">hello following example provides an illustration of a schema used for searching on hotel information with hello Description field defined in two languages.</span></span> <span data-ttu-id="ba4ea-135">Všimněte si, jak atributy řídit, jak se používá pole hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-135">Notice how attributes control how hello field is used.</span></span> <span data-ttu-id="ba4ea-136">Například hello `hotelId` slouží jako klíč dokumentu hello (`"key": true`) a je vyloučen z fulltextové vyhledávání (`"searchable": false`).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-136">For example hello `hotelId` is used as hello document key (`"key": true`) and is excluded from full-text searches (`"searchable": false`).</span></span>

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

<span data-ttu-id="ba4ea-137">Po vytvoření indexu hello budete odesílat dokumenty, které naplnit hello index.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-137">After hello index is created, you'll upload documents that populate hello index.</span></span> <span data-ttu-id="ba4ea-138">V tématu [přidat nebo aktualizovat dokumenty](#AddOrUpdateDocuments) pro tento další krok.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-138">See [Add or Update Documents](#AddOrUpdateDocuments) for this next step.</span></span>

<span data-ttu-id="ba4ea-139">Úvodní video tooindexing ve službě Azure Search, najdete v části hello [Channel 9 cloudu zahrnují díl na Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-139">For a video introduction tooindexing in Azure Search, see hello [Channel 9 Cloud Cover episode on Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).</span></span>

<a name="CreateIndex"></a>

## <a name="create-index"></a><span data-ttu-id="ba4ea-140">Vytvoření indexu</span><span class="sxs-lookup"><span data-stu-id="ba4ea-140">Create Index</span></span>
<span data-ttu-id="ba4ea-141">Index je primární prostředek hello uspořádání a vyhledávání dokumentů ve službě Azure Search, podobně jako toohow tabulku organizuje záznamů v databázi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-141">An index is hello primary means of organizing and searching documents in Azure Search, similar toohow a table organizes records in a database.</span></span> <span data-ttu-id="ba4ea-142">Každý index obsahuje kolekci dokumentů, že všechny odpovídat schéma indexu toohello (názvy polí, datové typy a vlastnosti), ale indexy určují také další konstrukce (trochu, vyhodnocování profily a možnosti CORS), které definují jiného chování vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-142">Each index has a collection of documents that all conform toohello index schema (field names, data types, and properties), but indexes also specify additional constructs (suggesters, scoring profiles, and CORS options) that define other search behaviors.</span></span>

<span data-ttu-id="ba4ea-143">Můžete vytvořit nový index v rámci služby Azure Search pomocí požadavek HTTP POST nebo PUT.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-143">You can create a new index within an Azure Search service using an HTTP POST or PUT request.</span></span> <span data-ttu-id="ba4ea-144">text Hello hello požadavku je schématu JSON, který určuje hello index a informace o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-144">hello body of hello request is a JSON schema that specifies hello index and configuration information.</span></span>

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="ba4ea-145">Alternativně můžete použít PUT a zadejte název indexu hello na hello identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-145">Alternatively, you can use PUT and specify hello index name on hello URI.</span></span> <span data-ttu-id="ba4ea-146">Pokud hello indexu neexistuje, bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-146">If hello index does not exist, it will be created.</span></span>

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

<span data-ttu-id="ba4ea-147">Vytvoření indexu určuje hello struktura hello dokumenty uložené a použít v operace hledání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-147">Creating an index determines hello structure of hello documents stored and used in search operations.</span></span> <span data-ttu-id="ba4ea-148">Naplnění indexu hello je samostatný operace.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-148">Populating hello index is a separate operation.</span></span> <span data-ttu-id="ba4ea-149">Pro tento krok, můžete použít [indexer](https://msdn.microsoft.com/library/azure/mt183328.aspx) (k dispozici pro podporované zdroje dat) nebo [přidání, aktualizace nebo odstranění dokumentů](https://msdn.microsoft.com/library/azure/dn798930.aspx) operaci.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-149">For this step, you can use an [indexer](https://msdn.microsoft.com/library/azure/mt183328.aspx) (available for supported data sources) or an [Add, Update, or Delete Documents](https://msdn.microsoft.com/library/azure/dn798930.aspx) operation.</span></span> <span data-ttu-id="ba4ea-150">index Hello obrácený se vygeneruje, když jsou odeslány hello dokumenty.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-150">hello inverted index is generated when hello documents are posted.</span></span>

<span data-ttu-id="ba4ea-151">**Poznámka:**: maximální počet indexů povolené hello se liší podle cenové úrovně.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-151">**Note**: hello maximum number of indexes allowed varies by pricing tier.</span></span> <span data-ttu-id="ba4ea-152">bezplatné služby Hello umožňuje až too3 indexy.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-152">hello free service allows up too3 indexes.</span></span> <span data-ttu-id="ba4ea-153">Standardní služby umožňuje 50 indexy jednu službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-153">Standard service allows 50 indexes per Search service.</span></span> <span data-ttu-id="ba4ea-154">V tématu [limity a omezení](http://msdn.microsoft.com/library/azure/dn798934.aspx) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-154">See [Limits and constraints](http://msdn.microsoft.com/library/azure/dn798934.aspx) for details.</span></span>

<span data-ttu-id="ba4ea-155">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-155">**Request**</span></span>

<span data-ttu-id="ba4ea-156">Je požadován pro všechny žádosti o služby protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-156">HTTPS is required for all service requests.</span></span> <span data-ttu-id="ba4ea-157">Hello **Create Index** požadavek lze sestavit pomocí Metoda POST nebo PUT metody.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-157">hello **Create Index** request can be constructed using either a POST or PUT method.</span></span> <span data-ttu-id="ba4ea-158">Pokud používáte POST, je nutné zadat název indexu v textu žádosti hello společně s definice schématu indexu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-158">When using POST, you must provide an index name in hello request body along with hello index schema definition.</span></span> <span data-ttu-id="ba4ea-159">Pomocí PUT název indexu hello je součástí adresy URL hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-159">With PUT, hello index name is part of hello URL.</span></span> <span data-ttu-id="ba4ea-160">Pokud hello indexu neexistuje, vytvoří se.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-160">If hello index doesn't exist, it is created.</span></span> <span data-ttu-id="ba4ea-161">Pokud již existuje, je aktualizovaná toohello novou definici.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-161">If it already exists, it is updated toohello new definition.</span></span>

<span data-ttu-id="ba4ea-162">Název indexu Hello musí být malá písmena, začínat písmenem nebo číslicí, mít žádné lomítka nebo tečky a být kratší než 128 znaků.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-162">hello index name must be lower case, start with a letter or number, have no slashes or dots, and be less than 128 characters.</span></span> <span data-ttu-id="ba4ea-163">Po spuštění název indexu hello s písmenem nebo číslicí, může obsahovat hello zbytek hello název jakékoli písmeno, čísla a pomlčky, dokud nejsou po sobě jdoucí pomlčky hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-163">After starting hello index name with a letter or number, hello rest of hello name can include any letter, number and dashes, as long as hello dashes are not consecutive.</span></span>

<span data-ttu-id="ba4ea-164">Hello `api-version` je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-164">hello `api-version` is required.</span></span> <span data-ttu-id="ba4ea-165">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) seznam dostupných verzí.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-165">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for a list of available versions.</span></span>

<span data-ttu-id="ba4ea-166">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-166">**Request Headers**</span></span>

<span data-ttu-id="ba4ea-167">Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-167">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="ba4ea-168">`Content-Type`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-168">`Content-Type`: Required.</span></span> <span data-ttu-id="ba4ea-169">Tuto možnost nastavíte příliš`application/json`</span><span class="sxs-lookup"><span data-stu-id="ba4ea-169">Set this too`application/json`</span></span>
* <span data-ttu-id="ba4ea-170">`api-key`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-170">`api-key`: Required.</span></span> <span data-ttu-id="ba4ea-171">Hello `api-key` se používá ke</span><span class="sxs-lookup"><span data-stu-id="ba4ea-171">hello `api-key` is used to</span></span>
* <span data-ttu-id="ba4ea-172">ověřit službu vyhledávání tooyour hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-172">authenticate hello request tooyour Search service.</span></span> <span data-ttu-id="ba4ea-173">Je řetězcová hodnota, jedinečné tooyour služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-173">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="ba4ea-174">Hello **Create Index** musí zahrnovat požadavek `api-key` záhlaví nastavit klíč správce tooyour (jako klíč dotazu názvem na rozdíl od tooa).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-174">hello **Create Index** request must include an `api-key` header set tooyour admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="ba4ea-175">Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-175">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="ba4ea-176">Můžete získat i hello název služby a `api-key` z řídicího panelu služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-176">You can get both hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="ba4ea-177">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-177">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="ba4ea-178"><a name="RequestData"></a>
**Syntaxe požadavku textu**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-178"><a name="RequestData"></a>
**Request Body Syntax**</span></span>

<span data-ttu-id="ba4ea-179">text Hello hello žádosti obsahuje definici schématu, která zahrnuje hello seznam datových polí v rámci dokumenty, kteří budou dodáni do tohoto indexu, datové typy, atributy a také volitelný seznam vyhodnocování profily, které jsou používané tooscore odpovídající dokumenty v Doba dotazu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-179">hello body of hello request contains a schema definition, which includes hello list of data fields within documents that will be fed into this index, data types, attributes, as well as an optional list of scoring profiles that are used tooscore matching documents at query time.</span></span>

<span data-ttu-id="ba4ea-180">Všimněte si, že pro požadavek POST musíte zadat název indexu hello v textu žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-180">Note that for a POST request, you must specify hello index name in hello request body.</span></span>

<span data-ttu-id="ba4ea-181">Může existovat pouze jedno pole s klíčem v indexu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-181">There can only be one key field in hello index.</span></span> <span data-ttu-id="ba4ea-182">Má toobe pole řetězce.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-182">It has toobe a string field.</span></span> <span data-ttu-id="ba4ea-183">Toto pole představuje jedinečný identifikátor každého dokumentu uloženého v rámci hello index hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-183">This field represents hello unique identifier for each document stored within hello index.</span></span>

<span data-ttu-id="ba4ea-184">hlavní části Hello indexu zahrnout hello následující:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-184">hello main parts of an index include hello following:</span></span>

* `name`
* <span data-ttu-id="ba4ea-185">`fields`které budou dodáni do tohoto indexu, včetně názvu, datový typ a vlastnosti, které definují povolené akce na tomto poli.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-185">`fields` that will be fed into this index, including name, data type, and properties that define allowable actions on that field.</span></span>
* <span data-ttu-id="ba4ea-186">`suggesters`použít pro automatické dokončování nebo našeptávání dotazů.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-186">`suggesters` used for auto-complete or type-ahead queries.</span></span>
* <span data-ttu-id="ba4ea-187">`scoringProfiles`používá pro určení rozsahu skóre vlastní vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-187">`scoringProfiles` used for custom search score ranking.</span></span> <span data-ttu-id="ba4ea-188">V tématu [přidejte vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-188">See [Add scoring profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for details.</span></span>
* <span data-ttu-id="ba4ea-189">`analyzers`, `charFilters`, `tokenizers`, `tokenFilters` používá toodefine, jak jsou dokumenty či dotazy rozdělen do indexovanou/prohledávatelné tokeny.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-189">`analyzers`, `charFilters`, `tokenizers`, `tokenFilters` used toodefine how your documents/queries are broken into indexable/searchable tokens.</span></span> <span data-ttu-id="ba4ea-190">V tématu [Analysis ve službě Azure Search](https://aka.ms//azsanalysis) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-190">See [Analysis in Azure Search](https://aka.ms//azsanalysis) for details.</span></span>
* <span data-ttu-id="ba4ea-191">`defaultScoringProfile`použít výchozí hello toooverwrite vyhodnocování chování.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-191">`defaultScoringProfile` used toooverwrite hello default scoring behaviors.</span></span>
* <span data-ttu-id="ba4ea-192">`corsOptions`dotazy cross-origin tooallow oproti indexu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-192">`corsOptions` tooallow cross-origin queries against your index.</span></span>

<span data-ttu-id="ba4ea-193">Syntaxe Hello strukturování datová část požadavku hello je následující.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-193">hello syntax for structuring hello request payload is as follows.</span></span> <span data-ttu-id="ba4ea-194">Ukázková žádost je k dispozici další na v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-194">A sample request is provided further on in this topic.</span></span>

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
          "analyzer": "name of hello analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of hello search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of hello indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in hello future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies toosearchable fields) {
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
                "boostingDuration": "..." (value representing timespan leading toonow over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field, see "scoringParameter" for syntax details)
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

<span data-ttu-id="ba4ea-195">**Atributy indexu**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-195">**Index Attributes**</span></span>

<span data-ttu-id="ba4ea-196">Hello následující atributy lze nastavit při vytváření indexu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-196">hello following attributes can be set when creating an index.</span></span> <span data-ttu-id="ba4ea-197">Podrobnosti o vyhodnocování a vyhodnocování profilů najdete v tématu [přidat vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-197">For details about scoring and scoring profiles, see [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span></span>

<span data-ttu-id="ba4ea-198">`name`-Nastaví hello název pole hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-198">`name` - Sets hello name of hello field.</span></span>

<span data-ttu-id="ba4ea-199">`type`-Nastaví hello datový typ pro pole hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-199">`type` - Sets hello data type for hello field.</span></span>

<span data-ttu-id="ba4ea-200">`searchable`-Označí pole hello fulltextového vyhledávání možné.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-200">`searchable` - Marks hello field as full-text search-able.</span></span> <span data-ttu-id="ba4ea-201">To znamená, že ho určitým analysis například dělení slov během indexování.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-201">This means it will undergo analysis such as word-breaking during indexing.</span></span> <span data-ttu-id="ba4ea-202">Pokud nastavíte `searchable` tooa hodnota pole jako "slunečného dne", interně je rozdělit na jednotlivé tokeny hello "slunečného" a "den".</span><span class="sxs-lookup"><span data-stu-id="ba4ea-202">If you set a `searchable` field tooa value like "sunny day", internally it will be split into hello individual tokens "sunny" and "day".</span></span> <span data-ttu-id="ba4ea-203">To umožňuje fulltextové vyhledávání pro tyto podmínky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-203">This enables full-text searches for these terms.</span></span> <span data-ttu-id="ba4ea-204">Pole typu `Edm.String` nebo `Collection(Edm.String)` jsou `searchable` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-204">Fields of type `Edm.String` or `Collection(Edm.String)` are `searchable` by default.</span></span> <span data-ttu-id="ba4ea-205">Pole dalších typů nelze `searchable`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-205">Fields of other types cannot be `searchable`.</span></span>

* <span data-ttu-id="ba4ea-206">**Poznámka:**: `searchable` pole využívat místo navíc v indexu, protože Azure Search budou ukládat další tokenizovaná verzi hello hodnotu pole pro fulltextové vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-206">**Note**: `searchable` fields consume extra space in your index since Azure Search will store an additional tokenized version of hello field value for full-text searches.</span></span> <span data-ttu-id="ba4ea-207">Pokud chcete místo toosave indexu a není nutné pole toobe, zahrnuté ve vyhledávání, nastavte `searchable` příliš`false`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-207">If you want toosave space in your index and you don't need a field toobe included in searches, set `searchable` too`false`.</span></span>

<span data-ttu-id="ba4ea-208">`filterable`– Umožňuje toobe hello pole odkazovaná v `$filter` dotazy.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-208">`filterable` - Allows hello field toobe referenced in `$filter` queries.</span></span> <span data-ttu-id="ba4ea-209">`filterable`se liší od `searchable` v tom, jak jsou zpracovávány řetězce.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-209">`filterable` differs from `searchable` in how strings are handled.</span></span> <span data-ttu-id="ba4ea-210">Pole typu `Edm.String` nebo `Collection(Edm.String)` , které jsou `filterable` nejsou prováděny dělení slov, tak porovnání jsou pro jenom přesné shody.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-210">Fields of type `Edm.String` or `Collection(Edm.String)` that are `filterable` do not undergo word-breaking, so comparisons are for exact matches only.</span></span> <span data-ttu-id="ba4ea-211">Například pokud nastavíte toto pole `f` příliš "slunečného dne" `$filter=f eq 'sunny'` se najít žádné shody, ale `$filter=f eq 'sunny day'` bude.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-211">For example, if you set such a field `f` too"sunny day", `$filter=f eq 'sunny'` will find no matches, but `$filter=f eq 'sunny day'` will.</span></span> <span data-ttu-id="ba4ea-212">Všechna pole jsou `filterable` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-212">All fields are `filterable` by default.</span></span>

<span data-ttu-id="ba4ea-213">`sortable`-Ve výchozím nastavení systému hello řadí výsledky podle skóre, ale v mnoha prostředí budou uživatelé chtít toosort pole v dokumentech hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-213">`sortable` - By default hello system sorts results by score, but in many experiences users will want toosort by fields in hello documents.</span></span> <span data-ttu-id="ba4ea-214">Pole typu `Collection(Edm.String)` nemůže být `sortable`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-214">Fields of type `Collection(Edm.String)` cannot be `sortable`.</span></span> <span data-ttu-id="ba4ea-215">Všechna ostatní pole jsou `sortable` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-215">All other fields are `sortable` by default.</span></span>

<span data-ttu-id="ba4ea-216">`facetable`-Obvykle používaných v prezentace výsledky hledání, který obsahuje počet přístupů podle kategorie (pro příklad, vyhledejte digitální fotoaparáty a přístupů viz značku, megapixely, ceny, atd.).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-216">`facetable`- Typically used in a presentation of search results that includes hit count by category (for example, search for digital cameras and see hits by brand, by megapixels, by price, etc.).</span></span> <span data-ttu-id="ba4ea-217">Tuto možnost nelze použít s pole typu `Edm.GeographyPoint`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-217">This option cannot be used with fields of type `Edm.GeographyPoint`.</span></span> <span data-ttu-id="ba4ea-218">Všechna ostatní pole jsou `facetable` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-218">All other fields are `facetable` by default.</span></span>

* <span data-ttu-id="ba4ea-219">**Poznámka:**: pole typu `Edm.String` , které jsou `filterable`, `sortable`, nebo `facetable` může být maximálně 32 KB délku.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-219">**Note**: Fields of type `Edm.String` that are `filterable`, `sortable`, or `facetable` can be at most 32KB in length.</span></span> <span data-ttu-id="ba4ea-220">Je to proto, že tato pole jsou považovány za jeden hledaný termín a hello maximální délka termín, který se ve službě Azure Search je 32KB.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-220">This is because such fields are treated as a single search term, and hello maximum length of a term in Azure Search is 32KB.</span></span> <span data-ttu-id="ba4ea-221">Pokud potřebujete další text než tento jeden řetězec pole toostore, budete potřebovat tooexplicitly nastavit `filterable`, `sortable`, a `facetable` příliš`false` v definici indexu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-221">If you need toostore more text than this in a single string field, you will need tooexplicitly set `filterable`, `sortable`, and `facetable` too`false` in your index definition.</span></span>
* <span data-ttu-id="ba4ea-222">**Poznámka:**: Pokud má pole žádná z výše uvedených hello atributy nastavit příliš`true` (`searchable`, `filterable`, `sortable`, nebo`facetable`) pole hello je efektivně vyloučen z indexu hello obrácený.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-222">**Note**: If a field has none of hello above attributes set too`true` (`searchable`, `filterable`, `sortable`,  or`facetable`) hello field is effectively excluded from hello inverted index.</span></span> <span data-ttu-id="ba4ea-223">Tato možnost je užitečná pro pole, které nejsou používány v dotazech, ale je potřeba mít ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-223">This option is useful for fields that are not used in queries, but are needed in search results.</span></span> <span data-ttu-id="ba4ea-224">Kromě těchto polí z indexu hello zvyšuje výkon.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-224">Excluding such fields from hello index improves performance.</span></span>

<span data-ttu-id="ba4ea-225">`key`-Značky hello pole jako obsahující jedinečné identifikátory pro dokumenty v indexu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-225">`key` - Marks hello field as containing unique identifiers for documents within hello index.</span></span> <span data-ttu-id="ba4ea-226">Právě jedno pole musí být zvolena jako hello `key` pole a musí být typu `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-226">Exactly one field must be chosen as hello `key` field and it must be of type `Edm.String`.</span></span> <span data-ttu-id="ba4ea-227">Klíčové pole můžou být použité toolook dokumentů přímo prostřednictvím hello [rozhraní API pro vyhledávání](#LookupAPI).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-227">Key fields can be used toolook up documents directly via hello [Lookup API](#LookupAPI).</span></span>

<span data-ttu-id="ba4ea-228">`retrievable`-Nastaví, zda může být hello pole vrácené ve výsledku hledání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-228">`retrievable` - Sets whether hello field can be returned in a search result.</span></span>  <span data-ttu-id="ba4ea-229">To je užitečné, když chcete toouse pole (například okraje) jako filtr, řazení, nebo vyhodnocování mechanismus, ale nechcete, aby hello pole toobe viditelné toohello koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-229">This is useful when you want toouse a field (for example, margin) as a filter, sorting, or scoring mechanism but do not want hello field toobe visible toohello end user.</span></span> <span data-ttu-id="ba4ea-230">Tento atribut musí být `true` pro `key` pole.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-230">This attribute must be `true` for `key` fields.</span></span>

<span data-ttu-id="ba4ea-231">`analyzer`-Nastaví hello název toouse hello analyzátor pro pole hello v čas při vyhledávání a indexování čas.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-231">`analyzer` - Sets hello name of hello analyzer toouse for hello field at search time and indexing time.</span></span> <span data-ttu-id="ba4ea-232">Hello je parametr povoleno nastaven hodnot najdete v části [analyzátorů](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-232">For hello allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="ba4ea-233">Tuto možnost lze použít pouze s `searchable` pole a nelze nastavit společně s buď `searchAnalyzer` nebo `indexAnalyzer`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-233">This option can be used only with `searchable` fields and it can't be set together with either `searchAnalyzer` or `indexAnalyzer`.</span></span>  <span data-ttu-id="ba4ea-234">Jakmile je zvoleno hello analyzátor, nelze změnit pro pole hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-234">Once hello analyzer is chosen, it cannot be changed for hello field.</span></span>

<span data-ttu-id="ba4ea-235">`searchAnalyzer`-Nastaví hello název hello analyzátor používá během hledání pro pole hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-235">`searchAnalyzer` - Sets hello name of hello analyzer used at search time for hello field.</span></span> <span data-ttu-id="ba4ea-236">Hello je parametr povoleno nastaven hodnot najdete v části [analyzátorů](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-236">For hello allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="ba4ea-237">Tuto možnost lze použít pouze s `searchable` pole.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-237">This option can be used only with `searchable` fields.</span></span> <span data-ttu-id="ba4ea-238">Je nutné ji nastavit společně s `indexAnalyzer` a nelze ji nastavit společně s hello `analyzer` možnost.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-238">It must be set together with `indexAnalyzer` and it cannot be set together with hello `analyzer` option.</span></span> <span data-ttu-id="ba4ea-239">Tento analyzátor mohou být aktualizovány na stávající pole.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-239">This analyzer can be updated on an existing field.</span></span>

<span data-ttu-id="ba4ea-240">`indexAnalyzer`-Nastaví hello název hello analyzátor používá během indexování pro pole hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-240">`indexAnalyzer` - Sets hello name of hello analyzer used at indexing time for hello field.</span></span> <span data-ttu-id="ba4ea-241">Hello je parametr povoleno nastaven hodnot najdete v části [analyzátorů](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-241">For hello allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="ba4ea-242">Tuto možnost lze použít pouze s `searchable` pole.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-242">This option can be used only with `searchable` fields.</span></span> <span data-ttu-id="ba4ea-243">Je nutné ji nastavit společně s `searchAnalyzer` a nelze ji nastavit společně s hello `analyzer` možnost.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-243">It must be set together with `searchAnalyzer` and it cannot be set together with hello `analyzer` option.</span></span> <span data-ttu-id="ba4ea-244">Jakmile je zvoleno hello analyzátor, nelze změnit pro pole hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-244">Once hello analyzer is chosen, it cannot be changed for hello field.</span></span>

<span data-ttu-id="ba4ea-245">`suggesters`-Nastaví hello režim hledání a pole, která jsou hello zdroj obsahu hello návrhy.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-245">`suggesters` - Sets hello search mode and fields that are hello source of hello content for suggestions.</span></span> <span data-ttu-id="ba4ea-246">V tématu [trochu](#Suggesters) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-246">See [Suggesters](#Suggesters) for details.</span></span>

<span data-ttu-id="ba4ea-247">`scoringProfiles`-Definuje vlastní vyhodnocování chování, které umožňují ovlivnit položky zobrazovat na vyšších pozicích ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-247">`scoringProfiles` - Defines custom scoring behaviors that let you influence which items appear higher in search results.</span></span> <span data-ttu-id="ba4ea-248">Vyhodnocování profily jsou tvořeny váhu pole a funkce.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-248">Scoring profiles are made up of field weights and functions.</span></span> <span data-ttu-id="ba4ea-249">V tématu [přidat vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) Další informace o atributech hello používá v profil vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-249">See [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for more information about hello attributes used in a scoring profile.</span></span>

<span data-ttu-id="ba4ea-250"><!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Jazyková podpora**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-250"><!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Language support**</span></span>

<span data-ttu-id="ba4ea-251">Prohledatelná pole podstoupit analysis která nejvíc často zahrnuje dělení slov, normalizaci text a filtrování podmínky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-251">Searchable fields undergo analysis that most frequently involves word-breaking, text normalization, and filtering out terms.</span></span> <span data-ttu-id="ba4ea-252">Ve výchozím nastavení jsou prohledatelná pole ve službě Azure Search analyzovaný se hello [Apache Lucene standardní analyzátor](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) která dělí text do elementů následující["Segmentace Unicode Text"](http://unicode.org/reports/tr29/) pravidla.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-252">By default, searchable fields in Azure Search are analyzed with hello [Apache Lucene Standard analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) which breaks text into elements following the["Unicode Text Segmentation"](http://unicode.org/reports/tr29/) rules.</span></span> <span data-ttu-id="ba4ea-253">Kromě toho standardní analyzátor hello převede všechny znaky tootheir malá formuláře.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-253">Additionally, hello standard analyzer converts all characters tootheir lower case form.</span></span> <span data-ttu-id="ba4ea-254">Indexované dokumenty a hledaných termínů projít hello analysis během indexování a zpracování dotazu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-254">Both indexed documents and search terms go through hello analysis during indexing and query processing.</span></span>

<span data-ttu-id="ba4ea-255">Služba Azure Search podporuje různé jazyky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-255">Azure Search supports a variety of languages.</span></span> <span data-ttu-id="ba4ea-256">Každý jazyk vyžaduje analyzer nestandardní text, který účty pro charakteristiky daného jazyka.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-256">Each language requires a non-standard text analyzer which accounts for characteristics of a given language.</span></span> <span data-ttu-id="ba4ea-257">Služba Azure Search nabízí dva typy analyzátorů:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-257">Azure Search offers two types of analyzers:</span></span>

* <span data-ttu-id="ba4ea-258">35 analyzátorů zajišťoval Lucene.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-258">35 analyzers backed by Lucene.</span></span>
* <span data-ttu-id="ba4ea-259">50 analyzátorů zajišťoval proprietární přirozeného jazyka Microsoft zpracování technologie používané v kanceláři a Bing.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-259">50 analyzers backed by proprietary Microsoft natural language processing technology used in Office and Bing.</span></span>

<span data-ttu-id="ba4ea-260">Někteří vývojáři preferovat hello známější, jednoduchý a open-source řešení Lucene.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-260">Some developers might prefer hello more familiar, simple, open-source solution of Lucene.</span></span> <span data-ttu-id="ba4ea-261">Lucene analyzátorů je rychlejší, ale analyzátorů Microsoft hello mít rozšířené možnosti, jako je například Lematizace, decompounding (v jazycích jako němčina, dánština, holandština, švédština, norština, estonština, dokončit, maďarština, slovenština) a entity rozpoznávání (adresy URL aplikace word e-mailů, čísla a kalendářní data).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-261">Lucene analyzers are faster, but hello Microsoft analyzers have advanced capabilities, such as lemmatization, word decompounding (in languages like German, Danish, Dutch, Swedish, Norwegian, Estonian, Finish, Hungarian, Slovak) and entity recognition (URLs, emails, dates, numbers).</span></span> <span data-ttu-id="ba4ea-262">Pokud je to možné byste měli spustit porovnání obou hello společnosti Microsoft a Lucene analyzátorů toodecide které z nich je lepší přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-262">If possible, you should run comparisons of both hello Microsoft and Lucene analyzers toodecide which one is a better fit.</span></span>

<span data-ttu-id="ba4ea-263">***Jejich porovnání***</span><span class="sxs-lookup"><span data-stu-id="ba4ea-263">***How they compare***</span></span>

<span data-ttu-id="ba4ea-264">Hello Lucene analyzátor pro angličtinu rozšiřuje standardní analyzátor hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-264">hello Lucene analyzer for English extends hello standard analyzer.</span></span> <span data-ttu-id="ba4ea-265">Se odebere z slova Přivlastňovací pád (koncové společnosti), platí rozklad dle [silné rozklad algoritmus](http://tartarus.org/~martin/PorterStemmer/)a také odebere Angličtina [zastavit slova](http://en.wikipedia.org/wiki/Stop_words).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-265">It removes possessives (trailing 's) from words, applies stemming as per [Porter Stemming algorithm](http://tartarus.org/~martin/PorterStemmer/), and removes English [stop words](http://en.wikipedia.org/wiki/Stop_words).</span></span>

<span data-ttu-id="ba4ea-266">Porovnání provede analyzátor Microsoft hello Lematizace místo rozklad.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-266">In comparison, hello Microsoft analyzer performs lemmatization instead of stemming.</span></span> <span data-ttu-id="ba4ea-267">Znamená to, se může zpracovat tvary a nestandardní word forms mnohem lepší co má za následek více relevantní výsledky hledání (sledovat modul 7 [Azure Search MVA prezentace](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-267">It means it can handle inflected and irregular word forms much better what results in more relevant search results (watch module 7 of [Azure Search MVA presentation](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) for more details).</span></span>

<span data-ttu-id="ba4ea-268">Indexování s Microsoft analyzátorů je v průměru toothree dvakrát nižší než jejich ekvivalenty Lucene, v závislosti na jazyk hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-268">Indexing with Microsoft analyzers is on average two toothree times slower than their Lucene equivalents, depending on hello language.</span></span> <span data-ttu-id="ba4ea-269">Pro dotazy Průměrná velikost nesmí být výrazně ovlivněn výkon vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-269">Search performance should not be significantly affected for average size queries.</span></span>

<span data-ttu-id="ba4ea-270">***Konfigurace***</span><span class="sxs-lookup"><span data-stu-id="ba4ea-270">***Configuration***</span></span>

<span data-ttu-id="ba4ea-271">Pro každé pole v definici indexu hello můžete nastavit hello `analyzer` název vlastnosti tooan analyzátor, který určuje, které jazyk a dodavatele.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-271">For each field in hello index definition, you can set hello `analyzer` property tooan analyzer name that specifies which language and vendor.</span></span> <span data-ttu-id="ba4ea-272">Hello stejné Analyzátor se použijí při indexování a hledání toto pole.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-272">hello same analyzer will be applied when indexing and searching for that field.</span></span>
<span data-ttu-id="ba4ea-273">Například můžete mít samostatné pole pro angličtinu, francouzštinu a španělština hotelů popisy, které existují vedle sebe v hello stejný index.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-273">For example, you can have separate fields for English, French, and Spanish hotel descriptions that exist side-by-side in hello same index.</span></span> <span data-ttu-id="ba4ea-274">Použití hello [parametr dotazu 'searchFields'](#SearchQueryParameters) toospecify které pole pro specifický jazyk toosearch proti v dotazech.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-274">Use hello ['searchFields' query parameter](#SearchQueryParameters) toospecify which language-specific field toosearch against in your queries.</span></span> <span data-ttu-id="ba4ea-275">Můžete zkontrolovat dotazu příklady, které zahrnují hello `analyzer` vlastnost [vyhledávání dokumentů](#SearchDocs).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-275">You can review query examples that include hello `analyzer` property in [Search Documents](#SearchDocs).</span></span> 

<span data-ttu-id="ba4ea-276">***Analyzátor seznamu***</span><span class="sxs-lookup"><span data-stu-id="ba4ea-276">***Analyzer list***</span></span>

<span data-ttu-id="ba4ea-277">Níže je seznam podporovaných jazyků společně s názvy analyzátor Lucene a Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-277">Below is hello list of supported languages together with Lucene and Microsoft analyzer names.</span></span>

<table style="font-size:12">
    <tr>
        <th><span data-ttu-id="ba4ea-278">Jazyk</span><span class="sxs-lookup"><span data-stu-id="ba4ea-278">Language</span></span></th>
        <th><span data-ttu-id="ba4ea-279">Název analyzátor Microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-279">Microsoft analyzer name</span></span></th>
        <th><span data-ttu-id="ba4ea-280">Název analyzátor Lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-280">Lucene analyzer name</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-281">arabština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-281">Arabic</span></span></td>
        <td><span data-ttu-id="ba4ea-282">ar.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-282">ar.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-283">ar.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-283">ar.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-284">Arménština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-284">Armenian</span></span></td>
        <td></td>
        <td><span data-ttu-id="ba4ea-285">hy.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-285">hy.lucene</span></span></td>
      </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-286">Bengálském</span><span class="sxs-lookup"><span data-stu-id="ba4ea-286">Bangla</span></span></td>
        <td><span data-ttu-id="ba4ea-287">Bn.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-287">bn.microsoft</span></span></td>
        <td></td>
    </tr>
      <tr>
        <td><span data-ttu-id="ba4ea-288">Baskičtina</span><span class="sxs-lookup"><span data-stu-id="ba4ea-288">Basque</span></span></td>
        <td></td>
        <td><span data-ttu-id="ba4ea-289">EU.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-289">eu.lucene</span></span></td>
    </tr>
      <tr>
         <td><span data-ttu-id="ba4ea-290">Bulharština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-290">Bulgarian</span></span></td>
        <td><span data-ttu-id="ba4ea-291">BG.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-291">bg.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-292">BG.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-292">bg.lucene</span></span></td>
      </tr>
      <tr>
        <td><span data-ttu-id="ba4ea-293">Katalánština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-293">Catalan</span></span></td>
        <td><span data-ttu-id="ba4ea-294">CA.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-294">ca.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-295">CA.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-295">ca.lucene</span></span></td>          
      </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-296">Zjednodušená čínština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-296">Chinese Simplified</span></span></td>
        <td><span data-ttu-id="ba4ea-297">zh-Hans.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-297">zh-Hans.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-298">zh-Hans.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-298">zh-Hans.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-299">Tradiční čínština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-299">Chinese Traditional</span></span></td>
        <td><span data-ttu-id="ba4ea-300">zh-Hant.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-300">zh-Hant.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-301">zh-Hant.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-301">zh-Hant.lucene</span></span></td>        
    <tr>
    <tr>
        <td><span data-ttu-id="ba4ea-302">Chorvatština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-302">Croatian</span></span></td>
        <td><span data-ttu-id="ba4ea-303">hr.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-303">hr.microsoft</span></span></td>
        <td/></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-304">čeština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-304">Czech</span></span></td>
        <td><span data-ttu-id="ba4ea-305">cs.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-305">cs.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-306">cs.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-306">cs.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="ba4ea-307">dánština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-307">Danish</span></span></td>
        <td><span data-ttu-id="ba4ea-308">da.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-308">da.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-309">da.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-309">da.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="ba4ea-310">holandština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-310">Dutch</span></span></td>
        <td><span data-ttu-id="ba4ea-311">NL.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-311">nl.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-312">NL.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-312">nl.lucene</span></span></td>    
    </tr>    
    <tr>
        <td><span data-ttu-id="ba4ea-313">Angličtina</span><span class="sxs-lookup"><span data-stu-id="ba4ea-313">English</span></span></td>        
        <td><span data-ttu-id="ba4ea-314">en.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-314">en.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-315">en.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-315">en.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-316">Estonština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-316">Estonian</span></span></td>
        <td><span data-ttu-id="ba4ea-317">et.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-317">et.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-318">finština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-318">Finnish</span></span></td>
        <td><span data-ttu-id="ba4ea-319">Fi.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-319">fi.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-320">Fi.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-320">fi.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="ba4ea-321">francouzština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-321">French</span></span></td>
        <td><span data-ttu-id="ba4ea-322">FR.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-322">fr.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-323">FR.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-323">fr.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-324">Galicijština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-324">Galician</span></span></td>
        <td></td>
        <td><span data-ttu-id="ba4ea-325">GL.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-325">gl.lucene</span></span></td>        
      </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-326">němčina</span><span class="sxs-lookup"><span data-stu-id="ba4ea-326">German</span></span></td>
        <td><span data-ttu-id="ba4ea-327">de.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-327">de.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-328">de.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-328">de.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-329">řečtina</span><span class="sxs-lookup"><span data-stu-id="ba4ea-329">Greek</span></span></td>
        <td><span data-ttu-id="ba4ea-330">El.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-330">el.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-331">El.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-331">el.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-332">Gudžarátština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-332">Gujarati</span></span></td>
        <td><span data-ttu-id="ba4ea-333">gu.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-333">gu.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-334">Hebrejština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-334">Hebrew</span></span></td>
        <td><span data-ttu-id="ba4ea-335">he.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-335">he.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-336">Hindština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-336">Hindi</span></span></td>
        <td><span data-ttu-id="ba4ea-337">Hi.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-337">hi.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-338">Hi.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-338">hi.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-339">maďarština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-339">Hungarian</span></span></td>        
        <td><span data-ttu-id="ba4ea-340">HU.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-340">hu.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-341">HU.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-341">hu.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-342">Islandština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-342">Icelandic</span></span></td>
        <td><span data-ttu-id="ba4ea-343">is.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-343">is.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-344">Indonéština (Bahasa)</span><span class="sxs-lookup"><span data-stu-id="ba4ea-344">Indonesian (Bahasa)</span></span></td>
        <td><span data-ttu-id="ba4ea-345">ID.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-345">id.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-346">ID.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-346">id.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-347">Irština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-347">Irish</span></span></td>
        <td></td>
          <td><span data-ttu-id="ba4ea-348">GA.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-348">ga.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-349">italština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-349">Italian</span></span></td>
        <td><span data-ttu-id="ba4ea-350">IT.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-350">it.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-351">IT.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-351">it.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-352">japonština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-352">Japanese</span></span></td>
        <td><span data-ttu-id="ba4ea-353">ja.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-353">ja.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-354">ja.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-354">ja.lucene</span></span></td>

    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-355">Kannadština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-355">Kannada</span></span></td>
        <td><span data-ttu-id="ba4ea-356">Ka.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-356">ka.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-357">korejština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-357">Korean</span></span></td>
        <td><span data-ttu-id="ba4ea-358">Ko.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-358">ko.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-359">Ko.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-359">ko.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-360">Lotyština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-360">Latvian</span></span></td>        
        <td><span data-ttu-id="ba4ea-361">LV.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-361">lv.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-362">LV.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-362">lv.lucene</span></span></td>    
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-363">Litevština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-363">Lithuanian</span></span></td>
        <td><span data-ttu-id="ba4ea-364">lt.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-364">lt.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-365">Malajalámština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-365">Malayalam</span></span></td>
        <td><span data-ttu-id="ba4ea-366">ml.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-366">ml.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-367">Malajština (Latina)</span><span class="sxs-lookup"><span data-stu-id="ba4ea-367">Malay (Latin)</span></span></td>
        <td><span data-ttu-id="ba4ea-368">MS.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-368">ms.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-369">Maráthština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-369">Marathi</span></span></td>
        <td><span data-ttu-id="ba4ea-370">MR.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-370">mr.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-371">norština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-371">Norwegian</span></span></td>
        <td><span data-ttu-id="ba4ea-372">NB.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-372">nb.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-373">No.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-373">no.lucene</span></span></td>        
    </tr>
      <tr>
        <td><span data-ttu-id="ba4ea-374">Perština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-374">Persian</span></span></td>
        <td></td>
        <td><span data-ttu-id="ba4ea-375">fa.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-375">fa.lucene</span></span></td>        
      </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-376">polština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-376">Polish</span></span></td>
        <td><span data-ttu-id="ba4ea-377">PL.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-377">pl.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-378">PL.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-378">pl.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-379">Portugalština (Brazílie)</span><span class="sxs-lookup"><span data-stu-id="ba4ea-379">Portuguese (Brazil)</span></span></td>
        <td><span data-ttu-id="ba4ea-380">PT-Br.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-380">pt-Br.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-381">PT-Br.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-381">pt-Br.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-382">Portugalština (Portugalsko)</span><span class="sxs-lookup"><span data-stu-id="ba4ea-382">Portuguese (Portugal)</span></span></td>
        <td><span data-ttu-id="ba4ea-383">PT-Pt.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-383">pt-Pt.microsoft</span></span></td>        
        <td><span data-ttu-id="ba4ea-384">PT-Pt.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-384">pt-Pt.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-385">Paňdžábština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-385">Punjabi</span></span></td>
        <td><span data-ttu-id="ba4ea-386">Pa.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-386">pa.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-387">rumunština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-387">Romanian</span></span></td>
        <td><span data-ttu-id="ba4ea-388">ro.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-388">ro.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-389">ro.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-389">ro.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-390">ruština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-390">Russian</span></span></td>
        <td><span data-ttu-id="ba4ea-391">RU.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-391">ru.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-392">RU.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-392">ru.lucene</span></span></td>    
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-393">Srbština (cyrilice)</span><span class="sxs-lookup"><span data-stu-id="ba4ea-393">Serbian (Cyrillic)</span></span></td>
        <td><span data-ttu-id="ba4ea-394">SR-cyrillic.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-394">sr-cyrillic.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-395">Srbština (Latina)</span><span class="sxs-lookup"><span data-stu-id="ba4ea-395">Serbian (Latin)</span></span></td>
        <td><span data-ttu-id="ba4ea-396">SR-latin.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-396">sr-latin.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-397">Slovenština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-397">Slovak</span></span></td>
        <td><span data-ttu-id="ba4ea-398">Sk.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-398">sk.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-399">Slovinština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-399">Slovenian</span></span></td>
        <td><span data-ttu-id="ba4ea-400">SL.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-400">sl.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-401">španělština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-401">Spanish</span></span></td>
        <td><span data-ttu-id="ba4ea-402">ES.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-402">es.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-403">ES.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-403">es.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-404">švédština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-404">Swedish</span></span></td>
        <td><span data-ttu-id="ba4ea-405">Sv.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-405">sv.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-406">Sv.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-406">sv.lucene</span></span></td>
    </tr>

    <tr>
        <td><span data-ttu-id="ba4ea-407">Tamilština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-407">Tamil</span></span></td>
        <td><span data-ttu-id="ba4ea-408">ta.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-408">ta.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-409">Telugština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-409">Telugu</span></span></td>
        <td><span data-ttu-id="ba4ea-410">te.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-410">te.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-411">Thajština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-411">Thai</span></span></td>
        <td><span data-ttu-id="ba4ea-412">TH.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-412">th.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-413">TH.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-413">th.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-414">turečtina</span><span class="sxs-lookup"><span data-stu-id="ba4ea-414">Turkish</span></span></td>
        <td><span data-ttu-id="ba4ea-415">TR.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-415">tr.microsoft</span></span></td>
        <td><span data-ttu-id="ba4ea-416">TR.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-416">tr.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-417">Ukrajinština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-417">Ukrainian</span></span></td>
        <td><span data-ttu-id="ba4ea-418">UK.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-418">uk.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-419">Urdština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-419">Urdu</span></span></td>
        <td><span data-ttu-id="ba4ea-420">Your.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-420">ur.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-421">Vietnamština</span><span class="sxs-lookup"><span data-stu-id="ba4ea-421">Vietnamese</span></span></td>
        <td><span data-ttu-id="ba4ea-422">VI.microsoft</span><span class="sxs-lookup"><span data-stu-id="ba4ea-422">vi.microsoft</span></span></td>
        <td></td>
    </tr>
    <td colspan="3"><span data-ttu-id="ba4ea-423">Kromě toho poskytuje Azure Search bez ohledu na jazyk analyzátor konfigurace</span><span class="sxs-lookup"><span data-stu-id="ba4ea-423">Additionally Azure Search provides language-agnostic analyzer configurations</span></span></td>
    <tr>
        <td><span data-ttu-id="ba4ea-424">Skládání standardní ASCII</span><span class="sxs-lookup"><span data-stu-id="ba4ea-424">Standard ASCII Folding</span></span></td>
        <td><span data-ttu-id="ba4ea-425">standardasciifolding.lucene</span><span class="sxs-lookup"><span data-stu-id="ba4ea-425">standardasciifolding.lucene</span></span></td>
        <td>
        <ul>
            <li><span data-ttu-id="ba4ea-426">Segmentace text Unicode (standardní Tokenizátor)</span><span class="sxs-lookup"><span data-stu-id="ba4ea-426">Unicode text segmentation (Standard Tokenizer)</span></span></li>
            <li><span data-ttu-id="ba4ea-427">Skládání filtru ASCII – převede znaky znakové sady Unicode, které nepatří do jejich ekvivalenty ASCII toohello sadu nejprve 127 znaků ASCII.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-427">ASCII folding filter - converts Unicode characters that don't belong toohello set of first 127 ASCII characters into their ASCII equivalents.</span></span> <span data-ttu-id="ba4ea-428">To je užitečné pro odebrání znaky s diakritikou.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-428">This is useful for removing diacritics.</span></span></li>
        </ul>
        </td>
    </tr>
</table>

<span data-ttu-id="ba4ea-429">Všechny analyzátory s názvy opatřen poznámkou <i>lucene</i> se používá technologii [analyzátory jazyka Apache Lucene](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-429">All analyzers with names annotated with <i>lucene</i> are powered by [Apache Lucene's language analyzers](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html).</span></span> <span data-ttu-id="ba4ea-430">Další informace o hello ASCII skládání filtru najdete [zde](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-430">More information about hello ASCII folding filter can be found [here](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).</span></span>

<span data-ttu-id="ba4ea-431">**Moduly pro návrhy**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-431">**Suggesters**</span></span>

<span data-ttu-id="ba4ea-432">A `suggester` definuje, která pole v indexu jsou použité toosupport automatického dokončování v hledání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-432">A `suggester` defines which fields in an index are used toosupport auto-complete in searches.</span></span> <span data-ttu-id="ba4ea-433">Obvykle jsou částečné vyhledávání řetězců odeslána toohello [návrhy API](#Suggestions) při hello uživatele je zadáním vyhledávací dotaz a hello rozhraní API vrací sadu navrhované frází.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-433">Typically partial search strings are sent toohello [Suggestions API](#Suggestions) while hello user is typing a search query, and hello API returns a set of suggested phrases.</span></span> <span data-ttu-id="ba4ea-434">Modulu pro návrhy, kterou definujete v indexu hello určuje pole, která jsou použité toobuild hello našeptávání hledaný text.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-434">A suggester that you define in hello index determines which fields are used toobuild hello type-ahead search terms.</span></span> <span data-ttu-id="ba4ea-435">V tématu [trochu](#Suggesters) podrobnosti o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-435">See [Suggesters](#Suggesters) for configuration details.</span></span>

<span data-ttu-id="ba4ea-436">**Profily skórování**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-436">**Scoring profiles**</span></span>

<span data-ttu-id="ba4ea-437">A `scoringProfile` definuje vlastní vyhodnocování chování, které umožňují ovlivnit položky zobrazovat na vyšších pozicích ve výsledcích hledání hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-437">A `scoringProfile` defines custom scoring behaviors that let you influence which items appear higher in hello search results.</span></span> <span data-ttu-id="ba4ea-438">Vyhodnocování profily jsou tvořeny váhu pole a funkce.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-438">Scoring profiles are made up of field weights and functions.</span></span> <span data-ttu-id="ba4ea-439">toouse je zadat profil v řetězci dotazu hello podle názvu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-439">toouse them, you specify a profile by name on hello query string.</span></span>

<span data-ttu-id="ba4ea-440">Výchozí profil vyhodnocování funguje za hello scény toocompute skóre vyhledávání pro každou položku v sadě výsledků.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-440">A default scoring profile operates behind hello scenes toocompute a search score for every item in a result set.</span></span> <span data-ttu-id="ba4ea-441">Můžete použít hello interní, nepojmenované profil vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-441">You can use hello internal, unnamed scoring profile.</span></span> <span data-ttu-id="ba4ea-442">Alternativně nastavte `defaultScoringProfile` toouse vlastní profil jako výchozí hello, vyvolá se vždy, když není zadaný vlastního profilu v řetězci dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-442">Alternatively, set `defaultScoringProfile` toouse a custom profile as hello default, invoked whenever a custom profile is not specified on hello query string.</span></span>

<span data-ttu-id="ba4ea-443">V tématu [přidat vyhodnocování profily indexu vyhledávání tooa (služby REST API Azure Search)](search-api-scoring-profiles-2015-02-28-preview.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-443">See [Add scoring profiles tooa search index (Azure Search Service REST API)](search-api-scoring-profiles-2015-02-28-preview.md) for details.</span></span>

<span data-ttu-id="ba4ea-444">**Možnosti CORS**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-444">**CORS Options**</span></span>

<span data-ttu-id="ba4ea-445">Javascript na straně klienta nelze volat všechny rozhraní API ve výchozím nastavení, protože prohlížeč hello zabrání všech žádostí napříč zdroji.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-445">Client-side Javascript cannot call any APIs by default since hello browser will prevent all cross-origin requests.</span></span> <span data-ttu-id="ba4ea-446">Povolte CORS (sdílení prostředků různého původu) tak, že nastavení hello `corsOptions` atribut tooallow cross-origin dotazy tooyour index.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-446">Enable CORS (Cross-Origin Resource Sharing) by setting hello `corsOptions` attribute tooallow cross-origin queries tooyour index.</span></span> <span data-ttu-id="ba4ea-447">Všimněte si, že pouze dotaz rozhraní API pro podporu CORS z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-447">Note that only query APIs support CORS for security reasons.</span></span> <span data-ttu-id="ba4ea-448">pro CORS lze nastavit Hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-448">hello following options can be set for CORS:</span></span>

* <span data-ttu-id="ba4ea-449">`allowedOrigins`(povinné): Toto je seznam zdroje, kterým se udělí přístup tooyour index.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-449">`allowedOrigins` (required): This is a list of origins that will be granted access tooyour index.</span></span> <span data-ttu-id="ba4ea-450">To znamená, že žádný kód Javascript z těchto zdroje bude povoleno tooquery indexu (za předpokladu, že poskytuje správný klíč API hello).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-450">This means that any Javascript code served from those origins will be allowed tooquery your index (assuming it provides hello correct API key).</span></span> <span data-ttu-id="ba4ea-451">Každý původ je obvykle ve formátu hello `protocol://fully-qualified-domain-name:port` i když hello port je často vynechán.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-451">Each origin is typically of hello form `protocol://fully-qualified-domain-name:port` although hello port is often omitted.</span></span> <span data-ttu-id="ba4ea-452">V tématu [v tomto článku](http://go.microsoft.com/fwlink/?LinkId=330822) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-452">See [this article](http://go.microsoft.com/fwlink/?LinkId=330822) for more details.</span></span>
  * <span data-ttu-id="ba4ea-453">Pokud chcete zdroje tooall přístup tooallow, zahrnout `*` jako jedna položka v hello `allowedOrigins` pole.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-453">If you want tooallow access tooall origins, include `*` as a single item in hello `allowedOrigins` array.</span></span> <span data-ttu-id="ba4ea-454">Všimněte si, že **to není doporučeno, postup pro produkční vyhledávací služby.**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-454">Note that **this is not recommended practice for production search services.**</span></span> <span data-ttu-id="ba4ea-455">Však může být užitečné pro vývoj nebo účely ladění.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-455">However, it may be useful for development or debugging purposes.</span></span>
* <span data-ttu-id="ba4ea-456">`maxAgeInSeconds`(volitelné): v prohlížečích se používá tato hodnota toodetermine hello doba trvání (v sekundách) toocache CORS předběžných odpovědí.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-456">`maxAgeInSeconds` (optional): Browsers use this value toodetermine hello duration (in seconds) toocache CORS preflight responses.</span></span> <span data-ttu-id="ba4ea-457">Toto musí být nezáporné celé číslo.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-457">This must be a non-negative integer.</span></span> <span data-ttu-id="ba4ea-458">Hello větší má hodnotu, bude hello lepší výkon, ale hello déle bude taky trvat vliv tootake změny zásad CORS.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-458">hello larger this value is, hello better performance will be, but hello longer it will take for CORS policy changes tootake effect.</span></span> <span data-ttu-id="ba4ea-459">Pokud není nastavena, použije se výchozí hodnota 5 minut.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-459">If it is not set, a default duration of 5 minutes will be used.</span></span>

<span data-ttu-id="ba4ea-460"><a name="CreateUpdateIndexExample"></a>
**Příklad text žádosti**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-460"><a name="CreateUpdateIndexExample"></a>
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

<span data-ttu-id="ba4ea-461">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-461">**Response**</span></span>

<span data-ttu-id="ba4ea-462">Pro úspěšné žádosti: "201 – vytvořeno".</span><span class="sxs-lookup"><span data-stu-id="ba4ea-462">For a successful request: "201 Created".</span></span>

<span data-ttu-id="ba4ea-463">Ve výchozím nastavení bude obsahovat text odpovědi hello hello JSON pro definici indexu hello, který byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-463">By default hello response body will contain hello JSON for hello index definition that was created.</span></span> <span data-ttu-id="ba4ea-464">Pokud hello `Prefer` hlavička požadavku je nastaven příliš`return=minimal`, bude text odpovědi hello prázdný a hello úspěch stavový kód bude "204 žádný obsah" místo "201 – vytvořeno".</span><span class="sxs-lookup"><span data-stu-id="ba4ea-464">If hello `Prefer` request header is set too`return=minimal`, hello response body will be empty and hello success status code will be "204 No Content" instead of "201 Created".</span></span> <span data-ttu-id="ba4ea-465">To platí bez ohledu na to, jestli byl PUT nebo POST použité toocreate hello index.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-465">This is true regardless of whether PUT or POST was used toocreate hello index.</span></span>

<span data-ttu-id="ba4ea-466">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-466">**Remarks**</span></span>

<span data-ttu-id="ba4ea-467">V současné době je omezená podpora aktualizace schématu indexu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-467">Currently, there is limited support for index schema updates.</span></span> <span data-ttu-id="ba4ea-468">Všechny aktualizace schémat, které by vyžadovaly přeindexování například změny typu polí nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-468">Any schema updates that would require re-indexing such as changing field types are not currently supported.</span></span> <span data-ttu-id="ba4ea-469">Ale existující pole nemůžete změnit ani odstranit, můžete kdykoli přidat nová pole existující index tooan kdykoli.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-469">Although existing fields cannot be changed or deleted, new fields can be added tooan existing index at any time.</span></span> <span data-ttu-id="ba4ea-470">Při přidání nové pole mít všechny stávající dokumenty v indexu hello automaticky pro toto pole hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-470">When a new field is added, all existing documents in hello index will automatically have a null value for that field.</span></span> <span data-ttu-id="ba4ea-471">Žádné další úložný prostor využijí dokud nové dokumenty jsou přidána toohello index.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-471">No additional storage space will be consumed until new documents are added toohello index.</span></span>

<a name="Suggesters"></a>

## <a name="suggesters"></a><span data-ttu-id="ba4ea-472">Moduly pro návrhy</span><span class="sxs-lookup"><span data-stu-id="ba4ea-472">Suggesters</span></span>
<span data-ttu-id="ba4ea-473">Funkce návrhy Hello ve službě Azure Search je funkce našeptávání nebo automatické dokončování dotazů, které poskytuje seznam potenciální hledaný text v odpovědi toopartial řetězec vstupy do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-473">hello suggestions feature in Azure Search is a type-ahead or auto-complete query capability, providing a list of potential search terms in response toopartial string inputs entered into a search box.</span></span> <span data-ttu-id="ba4ea-474">Pravděpodobně jste zaznamenali dotazů při používání komerční vyhledávací stroje: zadáním ".NET" v Bing vytvoří seznam podmínek pro ".NET 4.5", "rozhraní .NET Framework 3,5", a tak dále.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-474">You've probably noticed query suggestions when using commercial web search engines: typing ".NET" in Bing produces a list of terms for ".NET 4.5", ".NET Framework 3.5", and so forth.</span></span> <span data-ttu-id="ba4ea-475">Pokud používáte službu vyhledávání hello REST API, implementace návrhy ve vlastní aplikaci Azure Search vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-475">When using hello Search service REST API, implementing suggestions in a custom Azure Search application requires hello following:</span></span>

* <span data-ttu-id="ba4ea-476">Povolit návrhy přidáním **modulu pro návrhy** konstrukce v indexu, udělíte hello název, režim hledání a seznam polí, pro kterou našeptávání je volána.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-476">Enable suggestions by adding a **suggester** construction in your index, giving hello name, search mode, and a list of fields for which type-ahead is invoked.</span></span> <span data-ttu-id="ba4ea-477">Například pokud zadáte "mesto" jako pole zdroje, zadáte řetězec částečné vyhledávání "Sea" bude mít za následek "Seattle", "Pobřežního" a "Seatac" (všechny tři, jsou názvy skutečné města) nabízí jako uživatel toohello návrhy dotazu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-477">For example, if you specify "cityName" as a source field, typing a partial search string of "Sea" will result in "Seattle", "Seaside", and "Seatac" (all three are actual city names) offered up as query suggestions toohello user.</span></span>
* <span data-ttu-id="ba4ea-478">Vyvolat návrhy, volání hello [API návrhy](#Suggestions) v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-478">Invoke suggestions by calling hello [Suggestions API](#Suggestions) in your application code.</span></span> <span data-ttu-id="ba4ea-479">Částečné řetězce se obvykle odesílají toohello služby při hello uživatele je zadáním vyhledávací dotaz a toto rozhraní API vrací sadu navrhované frází.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-479">Typically partial search strings are sent toohello service while hello user is typing a search query, and this API returns a set of suggested phrases.</span></span>

<span data-ttu-id="ba4ea-480">Tento článek vysvětluje, jak tooconfigure **modulu pro návrhy**.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-480">This article explains how tooconfigure a **suggester**.</span></span> <span data-ttu-id="ba4ea-481">Také byste měli revidovat hello [návrhy API](#Suggestions) podrobnosti o tom, jak se používá modulu pro návrhy.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-481">You should also review hello [Suggestions API](#Suggestions) for details on how a suggester is used.</span></span>

<span data-ttu-id="ba4ea-482">**Využití**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-482">**Usage**</span></span>

<span data-ttu-id="ba4ea-483">`Suggesters`jsou vytvořené v indexu hello a fungují lépe, když používá toosuggest konkrétní dokumenty místo přijít podmínky či fráze.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-483">`Suggesters` are created in hello index and work best when used toosuggest specific documents rather than loose terms or phrases.</span></span> <span data-ttu-id="ba4ea-484">Hello nejlepší candidate pole jsou produktů, názvů a jiné poměrně krátké věty, které můžete identifikovat položku.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-484">hello best candidate fields are titles, names, and other relatively short phrases that can identify an item.</span></span> <span data-ttu-id="ba4ea-485">Méně účinné jsou opakovaných pole, kategorie a značky, nebo velmi dlouhé pole jako pole popisy nebo komentáře.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-485">Less effective are repetitive fields, such as categories and tags, or very long fields such as descriptions or comments fields.</span></span>

<span data-ttu-id="ba4ea-486">Jako součást definice indexu hello, můžete přidat jeden modulu pro návrhy toohello `suggesters` kolekce.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-486">As part of hello index definition, you can add a single suggester toohello `suggesters` collection.</span></span> <span data-ttu-id="ba4ea-487">Vlastnosti, které definují modulu pro návrhy zahrnout hello následující:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-487">Properties that define a suggester include hello following:</span></span>

* <span data-ttu-id="ba4ea-488">`name`: hello název modulu pro návrhy hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-488">`name`: hello name of hello suggester.</span></span> <span data-ttu-id="ba4ea-489">Při volání metody hello použijete hello název modulu pro návrhy hello `suggest` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-489">You use hello name of hello suggester when calling hello `suggest` API.</span></span>
* <span data-ttu-id="ba4ea-490">`searchMode`: hello toosearch strategie použitá pro kandidátských frází.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-490">`searchMode`: hello strategy used toosearch for candidate phrases.</span></span> <span data-ttu-id="ba4ea-491">je technologie Hello jediný momentálně podporovaný režim `analyzingInfixMatching`, který provádí flexibilní porovnávání frází na začátku hello nebo uprostřed vět hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-491">hello only mode currently supported is `analyzingInfixMatching`, which performs flexible matching of phrases at hello beginning or in hello middle of sentences.</span></span>
* <span data-ttu-id="ba4ea-492">`sourceFields`: Seznam jeden nebo více polí, které jsou hello zdroj obsahu hello návrhy.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-492">`sourceFields`: A list of one or more fields that are hello source of hello content for suggestions.</span></span> <span data-ttu-id="ba4ea-493">Pouze pole typu `Edm.String` a `Collection(Edm.String)` může být zdrojů pro návrhy.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-493">Only fields of type `Edm.String` and `Collection(Edm.String)` may be sources for suggestions.</span></span> <span data-ttu-id="ba4ea-494">Lze použít pouze pole, které nemají vlastní analyzátor jazyka nastavit.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-494">Only fields that don't have a custom language analyzer set can be used.</span></span>

<span data-ttu-id="ba4ea-495">**Příklad modulu pro návrhy**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-495">**Suggester Example**</span></span>

<span data-ttu-id="ba4ea-496">Modulu pro návrhy je součástí hello index.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-496">A suggester is part of hello index.</span></span> <span data-ttu-id="ba4ea-497">V hello může existovat pouze jedna modulu pro návrhy `suggesters` kolekce v aktuální verzi hello vedle hello polí kolekce a `scoringProfiles`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-497">Only one suggester can exist in hello `suggesters` collection in hello current version, alongside hello fields collection and `scoringProfiles`.</span></span>

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
> <span data-ttu-id="ba4ea-498">Pokud jste použili hello verze verzi public preview služby Azure Search, `suggesters` nahrazuje starší logická vlastnost (`"suggestions": false`), předpona návrhy podporována pouze pro krátké řetězce (3-25 znaků).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-498">If you used hello public preview version of Azure Search, `suggesters` replaces an older boolean property (`"suggestions": false`) that only supported prefix suggestions for short strings (3-25 characters).</span></span> <span data-ttu-id="ba4ea-499">Čím jsou nahrazené, `suggesters`, podporuje infix odpovídající, který vyhledá odpovídající podmínky od hello začátku nebo uprostřed hello pole obsahu, lepší toleranci chyb v řetězci pro vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-499">Its replacement, `suggesters`, supports infix matching that finds matching terms at hello beginning or in hello middle of field content, with better tolerance for mistakes in search strings.</span></span> <span data-ttu-id="ba4ea-500">Od verze hello všeobecně dostupná, to je nyní hello pouze implementace hello návrhů rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-500">Starting with hello generally available release, this is now hello only implementation of hello suggestions API.</span></span> <span data-ttu-id="ba4ea-501">Hello starší `suggestions` vlastnost, která byla zavedena v `api-version=2014-07-31-Preview` toowork pokračuje v této verzi, ale není v provozu v hello `2015-02-28` nebo novější verze Azure Search.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-501">hello older `suggestions` property that was introduced in `api-version=2014-07-31-Preview` continues toowork in that version, but is not operational in hello `2015-02-28` or later versions of Azure Search.</span></span>
> 
> 

<a name="UpdateIndex"></a>

## <a name="update-index"></a><span data-ttu-id="ba4ea-502">Aktualizovat Index</span><span class="sxs-lookup"><span data-stu-id="ba4ea-502">Update Index</span></span>
<span data-ttu-id="ba4ea-503">Můžete aktualizovat existující index v rámci služby Azure Search pomocí požadavek HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-503">You can update an existing index within Azure Search using an HTTP PUT request.</span></span> <span data-ttu-id="ba4ea-504">Aktualizace můžou zahrnovat přidáte nové pole toohello existující schéma, úprava možnosti CORS a úprava profily vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-504">Updates can include adding new fields toohello existing schema, modifying CORS options, and modifying scoring profiles.</span></span> <span data-ttu-id="ba4ea-505">V tématu [přidat vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-505">See [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for more information.</span></span> <span data-ttu-id="ba4ea-506">Zadáte název hello hello index tooupdate v identifikátoru URI žádosti hello:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-506">You specify hello name of hello index tooupdate on hello request URI:</span></span>

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="ba4ea-507">**Důležité:** podpora pro aktualizace schématu indexu je omezená toooperations, který není zapotřebí nové sestavení indexu vyhledávání hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-507">**Important:** Support for index schema updates is limited toooperations that don't require rebuilding hello search index.</span></span> <span data-ttu-id="ba4ea-508">Všechny aktualizace schémat, které by vyžadovaly přeindexování, například změny typu polí nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-508">Any schema updates that would require re-indexing, such as changing field types, are not currently supported.</span></span> <span data-ttu-id="ba4ea-509">Kdykoli můžete přidat nová pole, ale existující pole nemůžete změnit ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-509">New fields can be added at any time, although existing fields cannot be changed or deleted.</span></span> <span data-ttu-id="ba4ea-510">Hello totéž platí i příliš`suggesters`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-510">hello same applies too`suggesters`.</span></span> <span data-ttu-id="ba4ea-511">Může kdykoli přidat nová pole, jsou přidány tooa modulu pro návrhy na hello čas hello pole, ale pole nelze odebrat z `suggesters` a příliš nelze přidat stávající pole`suggesters`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-511">New fields may be added tooa suggester at hello time hello fields are added, but fields cannot be removed from `suggesters` and existing fields cannot be added too`suggesters`.</span></span>

<span data-ttu-id="ba4ea-512">Při přidávání nového indexu pole tooan, všechny stávající dokumenty v indexu hello bude mít automaticky pro toto pole hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-512">When adding a new field tooan index, all existing documents in hello index will automatically have a null value for that field.</span></span> <span data-ttu-id="ba4ea-513">Žádné další úložný prostor využijí dokud nové dokumenty jsou přidána toohello index.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-513">No additional storage space will be consumed until new documents are added toohello index.</span></span>

<span data-ttu-id="ba4ea-514">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-514">**Request**</span></span>

<span data-ttu-id="ba4ea-515">Je požadován pro všechny žádosti o služby protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-515">HTTPS is required for all service requests.</span></span> <span data-ttu-id="ba4ea-516">Hello **aktualizace indexu** požadavku je vytvořený pomocí HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-516">hello **Update Index** request is constructed using HTTP PUT.</span></span> <span data-ttu-id="ba4ea-517">Pomocí PUT název indexu hello je součástí adresy URL hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-517">With PUT, hello index name is part of hello URL.</span></span> <span data-ttu-id="ba4ea-518">Pokud hello indexu neexistuje, vytvoří se.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-518">If hello index doesn't exist, it is created.</span></span> <span data-ttu-id="ba4ea-519">Pokud už existuje hello index, je aktualizovaná toohello novou definici.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-519">If hello index already exists, it is updated toohello new definition.</span></span>

<span data-ttu-id="ba4ea-520">Název indexu Hello musí být malá písmena, začínat písmenem nebo číslicí, mít žádné lomítka nebo tečky a být kratší než 128 znaků.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-520">hello index name must be lower case, start with a letter or number, have no slashes or dots, and be less than 128 characters.</span></span> <span data-ttu-id="ba4ea-521">Po spuštění název indexu hello s písmenem nebo číslicí, může obsahovat hello zbytek hello název jakékoli písmeno, čísla a pomlčky, dokud nejsou po sobě jdoucí pomlčky hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-521">After starting hello index name with a letter or number, hello rest of hello name can include any letter, number and dashes, as long as hello dashes are not consecutive.</span></span>

<span data-ttu-id="ba4ea-522">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-522">`api-version=[string]` (required).</span></span> <span data-ttu-id="ba4ea-523">verze preview Hello je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-523">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="ba4ea-524">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-524">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="ba4ea-525">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-525">**Request Headers**</span></span>

<span data-ttu-id="ba4ea-526">Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-526">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="ba4ea-527">`Content-Type`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-527">`Content-Type`: Required.</span></span> <span data-ttu-id="ba4ea-528">Tuto možnost nastavíte příliš`application/json`</span><span class="sxs-lookup"><span data-stu-id="ba4ea-528">Set this too`application/json`</span></span>
* <span data-ttu-id="ba4ea-529">`api-key`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-529">`api-key`: Required.</span></span> <span data-ttu-id="ba4ea-530">Hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-530">hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="ba4ea-531">Je řetězcová hodnota, jedinečné tooyour služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-531">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="ba4ea-532">Hello **aktualizace indexu** musí zahrnovat požadavek `api-key` záhlaví nastavit klíč správce tooyour (jako klíč dotazu názvem na rozdíl od tooa).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-532">hello **Update Index** request must include an `api-key` header set tooyour admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="ba4ea-533">Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-533">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="ba4ea-534">Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-534">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="ba4ea-535">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-535">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="ba4ea-536">**Syntaxe požadavku textu**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-536">**Request Body Syntax**</span></span>

<span data-ttu-id="ba4ea-537">Při aktualizaci existujícího indexu, textu hello musí zahrnovat hello původní definici schématu, plus hello nová pole, který chcete přidat, jakož i hello upravit vyhodnocování profily, trochu a možnosti CORS, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-537">When updating an existing index, hello body must include hello original schema definition, plus hello new fields you are adding, as well as hello modified scoring profiles, suggesters and CORS options, if any.</span></span> <span data-ttu-id="ba4ea-538">Pokud nejsou změny hello vyhodnocování profily a možnosti CORS, je nutné zahrnout hello původních z vytvoření indexu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-538">If you are not modifying hello scoring profiles and CORS options, you must include hello originals from when hello index was created.</span></span> <span data-ttu-id="ba4ea-539">Obecně je hello nejlepší toouse vzor pro aktualizace definice indexu hello tooretrieve s GET, upravte ho a pak jej aktualizovat s PUT.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-539">In general hello best pattern toouse for updates is tooretrieve hello index definition with a GET, modify it, then update it with PUT.</span></span>

<span data-ttu-id="ba4ea-540">Syntaxe schématu Hello používá toocreate, který je zde uveden index pro práci v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-540">hello schema syntax used toocreate an index is reproduced here for convenience.</span></span> <span data-ttu-id="ba4ea-541">V tématu [Create Index](#CreateIndex) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-541">See [Create Index](#CreateIndex) for more details.</span></span>

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
          "analyzer": "name of hello analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of hello search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of hello indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in hello future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies toosearchable fields) {
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
                "boostingDuration": "..." (value representing timespan leading toonow over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field, see "scoringParameter" for syntax details)
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


<span data-ttu-id="ba4ea-542">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-542">**Response**</span></span>

<span data-ttu-id="ba4ea-543">Pro úspěšné žádosti: "204 žádný obsah".</span><span class="sxs-lookup"><span data-stu-id="ba4ea-543">For a successful request: "204 No Content".</span></span>

<span data-ttu-id="ba4ea-544">Ve výchozím nastavení bude text odpovědi hello prázdný.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-544">By default hello response body will be empty.</span></span> <span data-ttu-id="ba4ea-545">Nicméně, pokud hello `Prefer` hlavička požadavku je nastaven příliš`return=representation`, hello odpovědi bude obsahovat hello JSON pro hello definici indexu, která byla aktualizována.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-545">However, if hello `Prefer` request header is set too`return=representation`, hello response body will contain hello JSON for hello index definition that was updated.</span></span> <span data-ttu-id="ba4ea-546">V takovém případě bude kód stavu úspěch hello "200 OK".</span><span class="sxs-lookup"><span data-stu-id="ba4ea-546">In this case, hello success status code will be "200 OK".</span></span>

<span data-ttu-id="ba4ea-547">**Aktualizace definice indexu s vlastní analyzátory**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-547">**Updating index definition with custom analyzers**</span></span>

<span data-ttu-id="ba4ea-548">Jakmile je definovány analyzátor, tokenizátor, token filtru nebo filtr char, nemůže být upraven.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-548">Once an analyzer, a tokenizer, a token filter or a char filter is defined, it cannot be modified.</span></span> <span data-ttu-id="ba4ea-549">Nové lze přidat existující index tooan pouze v případě hello `allowIndexDowntime` příznak nastaven tootrue v žádosti o aktualizaci indexu hello:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-549">New ones can be added tooan existing index only if hello `allowIndexDowntime` flag is set tootrue in hello index update request:</span></span> 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

<span data-ttu-id="ba4ea-550">Všimněte si, že bude tuto operaci put indexu v režimu offline pro alespoň několik sekund vaší indexování a dotaz požadavků toofail.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-550">Note that this operation will put your index offline for at least a few seconds, causing your indexing and query requests toofail.</span></span> <span data-ttu-id="ba4ea-551">Výkon a zápisu dostupnost hello index může být zasažená po dobu několika minut po aktualizaci hello indexu nebo delší dobu velmi velký indexy.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-551">Performance and write availability of hello index can be impaired for several minutes after hello index is updated, or longer for very large indexes.</span></span>

<a name="ListIndexes"></a>

## <a name="list-indexes"></a><span data-ttu-id="ba4ea-552">Seznam indexů</span><span class="sxs-lookup"><span data-stu-id="ba4ea-552">List Indexes</span></span>
<span data-ttu-id="ba4ea-553">Hello **seznamu indexy** operace vrátí seznam hodnot hello indexy aktuálně ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-553">hello **List Indexes** operation returns a list of hello indexes currently in your Azure Search service.</span></span>

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="ba4ea-554">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-554">**Request**</span></span>

<span data-ttu-id="ba4ea-555">Je požadován pro všechny žádosti o služby protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-555">HTTPS is required for all service requests.</span></span> <span data-ttu-id="ba4ea-556">Hello **seznamu indexy** požadavek se dá vytvořit pomocí metody GET hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-556">hello **List Indexes** request can be constructed using hello GET method.</span></span>

<span data-ttu-id="ba4ea-557">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-557">`api-version=[string]` (required).</span></span> <span data-ttu-id="ba4ea-558">verze preview Hello je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-558">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="ba4ea-559">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-559">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="ba4ea-560">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-560">**Request Headers**</span></span>

<span data-ttu-id="ba4ea-561">Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-561">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="ba4ea-562">`api-key`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-562">`api-key`: Required.</span></span> <span data-ttu-id="ba4ea-563">Hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-563">hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="ba4ea-564">Je řetězcová hodnota, jedinečné tooyour služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-564">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="ba4ea-565">Hello **seznamu indexy** musí zahrnovat požadavek `api-key` klíč správce tooan sady (jako klíč dotazu názvem na rozdíl od tooa).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-565">hello **List Indexes** request must include an `api-key` set tooan admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="ba4ea-566">Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-566">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="ba4ea-567">Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-567">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="ba4ea-568">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-568">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="ba4ea-569">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-569">**Request Body**</span></span>

<span data-ttu-id="ba4ea-570">Žádné.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-570">None.</span></span>

<span data-ttu-id="ba4ea-571">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-571">**Response**</span></span>

<span data-ttu-id="ba4ea-572">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-572">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="ba4ea-573">Tady je odpovědi na příkladu:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-573">Here is an example response body:</span></span>

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

<span data-ttu-id="ba4ea-574">Všimněte si, že můžete filtrovat hello odpovědi dolů toojust hello vlastnosti, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-574">Note that you can filter hello response down toojust hello properties you're interested in.</span></span> <span data-ttu-id="ba4ea-575">Například pokud chcete pouze seznam názvů index, použijte hello OData `$select` dotazu možnost:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-575">For example, if you want only a list of index names, use hello OData `$select` query option:</span></span>

    GET /indexes?api-version=2015-02-28-Preview&$select=name

<span data-ttu-id="ba4ea-576">V takovém případě hello odpověď z hello výše příklad by měly vypadat následovně:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-576">In this case, hello response from hello above example would appear as follows:</span></span>

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

<span data-ttu-id="ba4ea-577">Toto je užitečné toosave šířky pásma, pokud máte spoustu indexy ve vyhledávací službě.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-577">This is a useful technique toosave bandwidth if you have a lot of indexes in your Search service.</span></span>

<a name="GetIndex"></a>

## <a name="get-index"></a><span data-ttu-id="ba4ea-578">Získat Index</span><span class="sxs-lookup"><span data-stu-id="ba4ea-578">Get Index</span></span>
<span data-ttu-id="ba4ea-579">Hello **získat Index** operaci získá definici indexu hello z Azure Search.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-579">hello **Get Index** operation gets hello index definition from Azure Search.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="ba4ea-580">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-580">**Request**</span></span>

<span data-ttu-id="ba4ea-581">Protokol HTTPS je vyžadována pro žádosti o služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-581">HTTPS is required for service requests.</span></span> <span data-ttu-id="ba4ea-582">Hello **získat Index** požadavek se dá vytvořit pomocí metody GET hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-582">hello **Get Index** request can be constructed using hello GET method.</span></span>

<span data-ttu-id="ba4ea-583">v identifikátoru URI žádosti hello Hello [název indexu] Určuje, které tooreturn indexu z kolekce indexů hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-583">hello [index name] in hello request URI specifies which index tooreturn from hello indexes collection.</span></span>

<span data-ttu-id="ba4ea-584">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-584">`api-version=[string]` (required).</span></span> <span data-ttu-id="ba4ea-585">verze preview Hello je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-585">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="ba4ea-586">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-586">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="ba4ea-587">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-587">**Request Headers**</span></span>

<span data-ttu-id="ba4ea-588">Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-588">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="ba4ea-589">`api-key`: hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-589">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="ba4ea-590">Je řetězcová hodnota, jedinečné tooyour služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-590">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="ba4ea-591">Hello **získat Index** musí zahrnovat požadavek `api-key` klíč správce tooan sady (jako klíč dotazu názvem na rozdíl od tooa).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-591">hello **Get Index** request must include an `api-key` set tooan admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="ba4ea-592">Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-592">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="ba4ea-593">Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-593">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="ba4ea-594">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-594">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="ba4ea-595">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-595">**Request Body**</span></span>

<span data-ttu-id="ba4ea-596">Žádné.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-596">None.</span></span>

<span data-ttu-id="ba4ea-597">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-597">**Response**</span></span>

<span data-ttu-id="ba4ea-598">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-598">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="ba4ea-599">Viz příklad hello JSON v [vytvářet a aktualizovat Index](#CreateUpdateIndexExample) příklad datové části odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-599">See hello example JSON in [Creating and Updating an Index](#CreateUpdateIndexExample) for an example of hello response payload.</span></span>

<a name="DeleteIndex"></a>

## <a name="delete-index"></a><span data-ttu-id="ba4ea-600">Odstranit Index</span><span class="sxs-lookup"><span data-stu-id="ba4ea-600">Delete Index</span></span>
<span data-ttu-id="ba4ea-601">Hello **odstranit Index** operace odebere index a související dokumenty ze služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-601">hello **Delete Index** operation removes an index and associated documents from your Azure Search service.</span></span> <span data-ttu-id="ba4ea-602">Název indexu hello můžete získat z řídicího panelu služby hello ve hello portál Azure nebo z hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-602">You can get hello index name from hello service dashboard in hello Azure portal, or from hello API.</span></span> <span data-ttu-id="ba4ea-603">V tématu [seznamu indexy](#ListIndexes) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-603">See [List Indexes](#ListIndexes) for details.</span></span>

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="ba4ea-604">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-604">**Request**</span></span>

<span data-ttu-id="ba4ea-605">Protokol HTTPS je vyžadována pro žádosti o služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-605">HTTPS is required for service requests.</span></span> <span data-ttu-id="ba4ea-606">Hello **odstranit Index** požadavek se dá vytvořit pomocí hello metodu DELETE.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-606">hello **Delete Index** request can be constructed using hello DELETE method.</span></span>

<span data-ttu-id="ba4ea-607">v identifikátoru URI žádosti hello Hello [název indexu] Určuje, které toodelete indexu z kolekce indexů hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-607">hello [index name] in hello request URI specifies which index toodelete from hello indexes collection.</span></span>

<span data-ttu-id="ba4ea-608">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-608">`api-version=[string]` (required).</span></span> <span data-ttu-id="ba4ea-609">verze preview Hello je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-609">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="ba4ea-610">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-610">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="ba4ea-611">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-611">**Request Headers**</span></span>

<span data-ttu-id="ba4ea-612">Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-612">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="ba4ea-613">`api-key`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-613">`api-key`: Required.</span></span> <span data-ttu-id="ba4ea-614">Hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-614">hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="ba4ea-615">Je řetězcovou hodnotu, adresa URL služby jedinečný tooyour.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-615">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="ba4ea-616">Hello **odstranit Index** musí zahrnovat požadavek `api-key` záhlaví nastavit klíč správce tooyour (jako klíč dotazu názvem na rozdíl od tooa).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-616">hello **Delete Index** request must include an `api-key` header set tooyour admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="ba4ea-617">Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-617">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="ba4ea-618">Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-618">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="ba4ea-619">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-619">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="ba4ea-620">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-620">**Request Body**</span></span>

<span data-ttu-id="ba4ea-621">Žádné.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-621">None.</span></span>

<span data-ttu-id="ba4ea-622">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-622">**Response**</span></span>

<span data-ttu-id="ba4ea-623">: Stav 204 ne obsahu je vrácen kód pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-623">Status Code: 204 No Content is returned for a successful response.</span></span>

<a name="GetIndexStats"></a>

## <a name="get-index-statistics"></a><span data-ttu-id="ba4ea-624">Získat statistiku indexu</span><span class="sxs-lookup"><span data-stu-id="ba4ea-624">Get Index Statistics</span></span>
<span data-ttu-id="ba4ea-625">Hello **získat statistiku Index** operace z Azure Search vrátí počet dokumentů pro aktuální index hello plus využití úložiště.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-625">hello **Get Index Statistics** operation returns from Azure Search a document count for hello current index, plus storage usage.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> <span data-ttu-id="ba4ea-626">Statistika na dokument počet a velikost úložiště se shromažďují každých několik minut, ne v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-626">Statistics on document count and storage size are collected every few minutes, not in real time.</span></span> <span data-ttu-id="ba4ea-627">Statistiky hello vrácený toto rozhraní API proto nemusí odrážet změny způsobené poslední operace indexování.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-627">Therefore, hello statistics returned by this API may not reflect changes caused by recent indexing operations.</span></span>
> 
> 

<span data-ttu-id="ba4ea-628">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-628">**Request**</span></span>

<span data-ttu-id="ba4ea-629">Je požadován pro všechny požadavky služby protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-629">HTTPS is required for all services requests.</span></span> <span data-ttu-id="ba4ea-630">Hello **získat statistiku Index** požadavek se dá vytvořit pomocí metody GET hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-630">hello **Get Index Statistics** request can be constructed using hello GET method.</span></span>

<span data-ttu-id="ba4ea-631">Hello [název indexu] v identifikátoru URI žádosti hello informuje hello služby tooreturn index statistiku pro hello zadaným indexem.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-631">hello [index name] in hello request URI tells hello service tooreturn index statistics for hello specified index.</span></span>

<span data-ttu-id="ba4ea-632">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-632">`api-version=[string]` (required).</span></span> <span data-ttu-id="ba4ea-633">verze preview Hello je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-633">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="ba4ea-634">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-634">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="ba4ea-635">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-635">**Request Headers**</span></span>

<span data-ttu-id="ba4ea-636">Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-636">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="ba4ea-637">`api-key`: hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-637">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="ba4ea-638">Je řetězcová hodnota, jedinečné tooyour služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-638">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="ba4ea-639">Hello **získat statistiku Index** musí zahrnovat požadavek `api-key` klíč správce tooan sady (jako klíč dotazu názvem na rozdíl od tooa).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-639">hello **Get Index Statistics** request must include an `api-key` set tooan admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="ba4ea-640">Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-640">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="ba4ea-641">Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-641">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="ba4ea-642">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-642">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="ba4ea-643">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-643">**Request Body**</span></span>

<span data-ttu-id="ba4ea-644">Žádné.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-644">None.</span></span>

<span data-ttu-id="ba4ea-645">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-645">**Response**</span></span>

<span data-ttu-id="ba4ea-646">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-646">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="ba4ea-647">text odpovědi Hello je ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-647">hello response body is in hello following format:</span></span>

    {
      "documentCount": number,
      "storageSize": number (size of hello index in bytes)
    }

<a name="TestAnalyzer"></a>

## <a name="test-analyzer"></a><span data-ttu-id="ba4ea-648">Analyzátor testu</span><span class="sxs-lookup"><span data-stu-id="ba4ea-648">Test Analyzer</span></span>
<span data-ttu-id="ba4ea-649">Hello **analyzovat rozhraní API** ukazuje, jak analyzátor dělí text do tokenů.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-649">hello **Analyze API** shows how an analyzer breaks text into tokens.</span></span>

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="ba4ea-650">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-650">**Request**</span></span>

<span data-ttu-id="ba4ea-651">Je požadován pro všechny požadavky služby protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-651">HTTPS is required for all services requests.</span></span> <span data-ttu-id="ba4ea-652">Hello **analyzovat rozhraní API** požadavek se dá vytvořit pomocí metody POST hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-652">hello **Analyze API** request can be constructed using hello POST method.</span></span>

<span data-ttu-id="ba4ea-653">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-653">`api-version=[string]` (required).</span></span> <span data-ttu-id="ba4ea-654">verze preview Hello je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-654">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="ba4ea-655">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-655">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="ba4ea-656">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-656">**Request Headers**</span></span>

<span data-ttu-id="ba4ea-657">Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-657">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="ba4ea-658">`api-key`: hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-658">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="ba4ea-659">Je řetězcová hodnota, jedinečné tooyour služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-659">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="ba4ea-660">Hello **analyzovat rozhraní API** musí zahrnovat požadavek `api-key` klíč správce tooan sady (jako klíč dotazu názvem na rozdíl od tooa).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-660">hello **Analyze API** request must include an `api-key` set tooan admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="ba4ea-661">Budete také potřebovat hello název indexu a hello název tooconstruct hello žádost o adresu URL služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-661">You will also need hello index name and hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="ba4ea-662">Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-662">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="ba4ea-663">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-663">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="ba4ea-664">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-664">**Request Body**</span></span>

    {
      "text": "Text tooanalyze",
      "analyzer": "analyzer_name"
    }

<span data-ttu-id="ba4ea-665">nebo</span><span class="sxs-lookup"><span data-stu-id="ba4ea-665">or</span></span>

    {
      "text": "Text tooanalyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

<span data-ttu-id="ba4ea-666">Hello `analyzer_name`, `tokenizer_name`, `token_filter_name` a `char_filter_name` nutnost toobe platné názvy předdefinované nebo vlastní analyzátorů, tokenizers, tokenu filtry a filtry znakový hello index.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-666">hello `analyzer_name`, `tokenizer_name`, `token_filter_name` and `char_filter_name` need toobe valid names of predefined or custom analyzers, tokenizers, token filters and char filters for hello index.</span></span> <span data-ttu-id="ba4ea-667">informace o procesu hello lexikální analýzy najdete toolearn [Analysis ve službě Azure Search](https://aka.ms/azsanalysis).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-667">toolearn more about hello process of lexical analysis see [Analysis in Azure Search](https://aka.ms/azsanalysis).</span></span>

<span data-ttu-id="ba4ea-668">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-668">**Response**</span></span>

<span data-ttu-id="ba4ea-669">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-669">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="ba4ea-670">text odpovědi Hello je ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-670">hello response body is in hello following format:</span></span>

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of hello first character of hello token),
          "endOffset": number (index of hello last character of hello token),
          "position": number (position of hello token in hello input text)
        },
        ...
      ]
    }

<span data-ttu-id="ba4ea-671">**Analýza příklad rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-671">**Analyze API example**</span></span>

<span data-ttu-id="ba4ea-672">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-672">**Request**</span></span>

    {
      "text": "Text tooanalyze",
      "analyzer": "standard"
    }

<span data-ttu-id="ba4ea-673">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-673">**Response**</span></span>

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

## <a name="document-operations"></a><span data-ttu-id="ba4ea-674">Operace dokumentu</span><span class="sxs-lookup"><span data-stu-id="ba4ea-674">Document Operations</span></span>
<span data-ttu-id="ba4ea-675">Ve službě Azure Search index je uložená v cloudu hello a vyplněny dokumentů JSON, můžete nahrávat na server služby toohello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-675">In Azure Search, an index is stored in hello cloud and populated using JSON documents that you upload toohello service.</span></span> <span data-ttu-id="ba4ea-676">Všechny hello dokumenty, které můžete nahrávat na server tvoří hello souhrnu dat hledání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-676">All hello documents that you upload comprise hello corpus of your search data.</span></span> <span data-ttu-id="ba4ea-677">Dokumenty obsahují pole, z nichž některé jsou tokenizovaného do hledaných termínů jako jejich uložení.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-677">Documents contain fields, some of which are tokenized into search terms as they are uploaded.</span></span> <span data-ttu-id="ba4ea-678">Hello `/docs` segment adresy URL v hello rozhraní API služby Azure Search představuje kolekci hello dokumentů v indexu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-678">hello `/docs` URL segment in hello Azure Search API represents hello collection of documents in an index.</span></span> <span data-ttu-id="ba4ea-679">Všechny operace provést na kolekci hello, například odeslání, sloučení, odstranění nebo dotazování dokumentů proběhla v kontextu hello jeden index, takže hello adresy URL pro tyto operace bude vždy začínat `/indexes/[index name]/docs` pro název daného indexu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-679">All operations performed on hello collection such as uploading, merging, deleting, or querying documents take place in hello context of a single index, so hello URLs for these operations will always start with `/indexes/[index name]/docs` for a given index name.</span></span>

<span data-ttu-id="ba4ea-680">Kód aplikace musíte vygenerovat buď tooAzure tooupload dokumenty JSON vyhledávání nebo můžete použít [indexer](https://msdn.microsoft.com/library/dn946891.aspx) tooload dokumentů. Pokud je zdroj dat hello Azure SQL Database nebo Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-680">Your application code must either generate JSON documents tooupload tooAzure Search or you can use an [indexer](https://msdn.microsoft.com/library/dn946891.aspx) tooload documents if hello data source is either Azure SQL Database or Azure Cosmos DB.</span></span> <span data-ttu-id="ba4ea-681">Obvykle indexy budou naplněny z jedné datové sady, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-681">Typically, indexes are populated from a single dataset that you provide.</span></span>

<span data-ttu-id="ba4ea-682">Měli byste mít jeden dokument pro každou položku, které chcete toosearch.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-682">You should plan on having one document for each item that you want toosearch.</span></span> <span data-ttu-id="ba4ea-683">Film pronájem aplikace může mít jeden dokument na film, storefront aplikace může mít jeden dokument za SKU, online výukových kurzů aplikace může mít jeden dokument za kurzu, firma research může mít jeden dokument pro každý academic dokumentu v jejich úložiště a tak dále.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-683">A movie rental application might have one document per movie, a storefront application might have one document per SKU, an online courseware application might have one document per course, a research firm might have one document for each academic paper in their repository, and so on.</span></span>

<span data-ttu-id="ba4ea-684">Dokumenty obsahovat jeden či více polí.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-684">Documents consist of one or more fields.</span></span> <span data-ttu-id="ba4ea-685">Pole může obsahovat text, který je tokenizovaného do podmínek vyhledávání ve službě Azure Search, jakož i bez tokenizovaného nebo jiné než textové hodnoty, které mohou být používány filtry, nebo profily vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-685">Fields can contain text that is tokenized by Azure Search into search terms, as well as non-tokenized or non-text values that can be used in filters or scoring profiles.</span></span> <span data-ttu-id="ba4ea-686">Hello názvy, datové typy a funkce vyhledávání, které jsou podporovány pro každé pole určuje schéma indexu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-686">hello names, data types, and search features supported for each field are determined by hello index schema.</span></span> <span data-ttu-id="ba4ea-687">Jedno z polí hello ve schématu každý index musí být určeny jako ID a každý dokument musí mít hodnotu pro hello ID pole, která jednoznačně identifikuje tento dokument v indexu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-687">One of hello fields in each index schema must be designated as an ID, and each document must have a value for hello ID field that uniquely identifies that document in hello index.</span></span> <span data-ttu-id="ba4ea-688">Všechna ostatní pole dokumentu jsou volitelné a bude použita výchozí hodnota null tooa Pokud nezadaný.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-688">All other document fields are optional and will default tooa null value if left unspecified.</span></span> <span data-ttu-id="ba4ea-689">Všimněte si, že hodnoty null nemusí provádět žádné místo v indexu vyhledávání hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-689">Note that null values do not take up space in hello search index.</span></span>

<span data-ttu-id="ba4ea-690">Předtím, než můžete odesílat dokumenty, musí již jste vytvořili hello index hello služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-690">Before you can upload documents, you must have already created hello index on hello service.</span></span> <span data-ttu-id="ba4ea-691">V tématu [Create Index](#CreateIndex) podrobnosti o tento první krok.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-691">See [Create Index](#CreateIndex) for details about this first step.</span></span>

<a name="AddOrUpdateDocuments"></a>

## <a name="add-update-or-delete-documents"></a><span data-ttu-id="ba4ea-692">Přidání, aktualizace nebo odstranění dokumentů</span><span class="sxs-lookup"><span data-stu-id="ba4ea-692">Add, Update, or Delete Documents</span></span>
<span data-ttu-id="ba4ea-693">Můžete nahrát, sloučení, sloučení nebo odeslání nebo odstranění dokumentů ze zadaného indexu pomocí HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-693">You can upload, merge, merge-or-upload or delete documents from a specified index using HTTP POST.</span></span> <span data-ttu-id="ba4ea-694">Pro velké množství aktualizací se doporučuje dávkování dokumentů (too1000 dokumenty na jednu dávku) nebo o 16 MB na jednu dávku.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-694">For large numbers of updates, batching of documents (up too1000 documents per batch or about 16 MB per batch) is recommended.</span></span>

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="ba4ea-695">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-695">**Request**</span></span>

<span data-ttu-id="ba4ea-696">Je požadován pro všechny žádosti o služby protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-696">HTTPS is required for all service requests.</span></span> <span data-ttu-id="ba4ea-697">Můžete nahrát, sloučení, sloučení nebo odeslání nebo odstranění dokumentů ze zadaného indexu pomocí HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-697">You can upload, merge, merge-or-upload or delete documents from a specified index using HTTP POST.</span></span>

<span data-ttu-id="ba4ea-698">[název indexu] obsahuje identifikátor URI požadavku Hello zadání které dokumenty toopost index.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-698">hello request URI includes [index name], specifying which index toopost documents.</span></span> <span data-ttu-id="ba4ea-699">Najednou můžete účtovat pouze index tooone dokumenty.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-699">You can only post documents tooone index at a time.</span></span>

<span data-ttu-id="ba4ea-700">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-700">`api-version=[string]` (required).</span></span> <span data-ttu-id="ba4ea-701">verze preview Hello je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-701">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="ba4ea-702">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-702">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="ba4ea-703">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-703">**Request Headers**</span></span>

<span data-ttu-id="ba4ea-704">Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-704">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="ba4ea-705">`Content-Type`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-705">`Content-Type`: Required.</span></span> <span data-ttu-id="ba4ea-706">Tuto možnost nastavíte příliš`application/json`</span><span class="sxs-lookup"><span data-stu-id="ba4ea-706">Set this too`application/json`</span></span>
* <span data-ttu-id="ba4ea-707">`api-key`: Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-707">`api-key`: Required.</span></span> <span data-ttu-id="ba4ea-708">Hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-708">hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="ba4ea-709">Je řetězcová hodnota, jedinečné tooyour služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-709">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="ba4ea-710">Hello **přidat dokumenty** musí zahrnovat požadavek `api-key` záhlaví nastavit klíč správce tooyour (jako klíč dotazu názvem na rozdíl od tooa).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-710">hello **Add Documents** request must include an `api-key` header set tooyour admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="ba4ea-711">Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-711">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="ba4ea-712">Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-712">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="ba4ea-713">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-713">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="ba4ea-714">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-714">**Request Body**</span></span>

<span data-ttu-id="ba4ea-715">Hello textu hello požadavku obsahuje jeden nebo více dokumentů toobe indexované.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-715">hello body of hello request contains one or more documents toobe indexed.</span></span> <span data-ttu-id="ba4ea-716">Dokumenty jsou identifikovány jedinečný klíč.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-716">Documents are identified by a unique key.</span></span> <span data-ttu-id="ba4ea-717">Každý dokument je přidružené akci: odeslání, sloučení, mergeOrUpload nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-717">Each document is associated with an action: upload, merge, mergeOrUpload or delete.</span></span> <span data-ttu-id="ba4ea-718">Odeslání požadavků musí obsahovat data dokumentu hello jako sada párů klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-718">Upload requests must include hello document data as a set of key/value pairs.</span></span>

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
> <span data-ttu-id="ba4ea-719">Dokument klíče může obsahovat pouze písmena, číslice, pomlčky ("-"), podtržítka ("_") a symboly rovná se ("=").</span><span class="sxs-lookup"><span data-stu-id="ba4ea-719">Document keys can only contain letters, numbers, dashes ("-"), underscores ("_"), and equal signs ("=").</span></span> <span data-ttu-id="ba4ea-720">Další podrobnosti najdete v tématu [pravidla po pojmenování](https://msdn.microsoft.com/library/azure/dn857353.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-720">For more details, see [Naming rules](https://msdn.microsoft.com/library/azure/dn857353.aspx).</span></span>
> 
> 

<span data-ttu-id="ba4ea-721">**Akce dokumentu**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-721">**Document Actions**</span></span>

* <span data-ttu-id="ba4ea-722">`upload`: Nahrávání akci je podobný tooan "upsert", kde hello dokument vložený, pokud je nový a aktualizovaný nebo nahrazený, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-722">`upload`: An upload action is similar tooan "upsert" where hello document will be inserted if it is new and updated/replaced if it exists.</span></span> <span data-ttu-id="ba4ea-723">Všimněte si, že všechna pole jsou nahrazena v případě aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-723">Note that all fields are replaced in hello update case.</span></span>
* <span data-ttu-id="ba4ea-724">`merge`: Aktualizace sloučení existující dokumentů s hello zadaná pole.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-724">`merge`: Merge updates an existing document with hello specified fields.</span></span> <span data-ttu-id="ba4ea-725">Pokud hello dokument neexistuje, sloučení hello selže.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-725">If hello document doesn't exist, hello merge will fail.</span></span> <span data-ttu-id="ba4ea-726">Každé pole zadané ve sloučení nahradí stávající pole hello v dokumentu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-726">Any field you specify in a merge will replace hello existing field in hello document.</span></span> <span data-ttu-id="ba4ea-727">To zahrnuje i pole typu `Collection(Edm.String)`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-727">This includes fields of type `Collection(Edm.String)`.</span></span> <span data-ttu-id="ba4ea-728">Například pokud hello dokument obsahuje pole "značky" s hodnotou `["budget"]` a vy spustíte sloučení s hodnotou `["economy", "pool"]` pro "značky" hello konečná hodnota pole značky"hello" bude `["economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-728">For example, if hello document contains a field "tags" with value `["budget"]` and you execute a merge with value `["economy", "pool"]` for "tags", hello final value of hello "tags" field will be `["economy", "pool"]`.</span></span> <span data-ttu-id="ba4ea-729">Zruší **není** být `["budget", "economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-729">It will **not** be `["budget", "economy", "pool"]`.</span></span>
* <span data-ttu-id="ba4ea-730">`mergeOrUpload`: chová jako `merge` Pokud dokument s hello zadaný klíč již existuje v indexu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-730">`mergeOrUpload`: behaves like `merge` if a document with hello given key already exists in hello index.</span></span> <span data-ttu-id="ba4ea-731">Pokud hello dokument neexistuje, chová se jako `upload` s novým dokumentem.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-731">If hello document does not exist it behaves like `upload` with a new document.</span></span>
* <span data-ttu-id="ba4ea-732">`delete`: Odstranění odebere hello indexu zadaný dokument hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-732">`delete`: Delete removes hello specified document from hello index.</span></span> <span data-ttu-id="ba4ea-733">Všimněte si, že všechna zadaná pole v `delete` operaci než hello pole klíče budou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-733">Note that any fields you specify in a `delete` operation other than hello key field will be ignored.</span></span> <span data-ttu-id="ba4ea-734">Pokud chcete tooremove z dokumentu jednotlivá pole, použijte `merge` místo a jednoduše nastavte hello pole explicitně příliš`null`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-734">If you want tooremove an individual field from a document, use `merge` instead and simply set hello field explicitly too`null`.</span></span>

<span data-ttu-id="ba4ea-735">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-735">**Response**</span></span>

<span data-ttu-id="ba4ea-736">Pro úspěšné odpovědi, což znamená, že všechny položky byly úspěšně indexované je vrátit stavovým kódem 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-736">Status code 200 (OK) is returned for a successful response, meaning that all items have been successfully indexed.</span></span> <span data-ttu-id="ba4ea-737">To vyplývá z hello `status` vlastnost Probíhá nastavení tootrue pro všechny položky, jakož i hello `statusCode` vlastnost Probíhá nastavení tooeither 201 (pro nově nahraném dokumenty) nebo 200 (pro sloučené nebo odstraněné dokumenty):</span><span class="sxs-lookup"><span data-stu-id="ba4ea-737">This is indicated by hello `status` property being set tootrue for all items, as well as hello `statusCode` property being set tooeither 201 (for newly uploaded documents) or 200 (for merged or deleted documents):</span></span>

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

<span data-ttu-id="ba4ea-738">Stavový kód 207 (více Status) je vrácena, pokud nebyla alespoň jedna položka úspěšně indexovaná.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-738">Status code 207 (Multi-Status) is returned when at least one item was not successfully indexed.</span></span> <span data-ttu-id="ba4ea-739">Položky, které nebyly indexovány mají hello `status` pole nastaveno toofalse.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-739">Items that have not been indexed have hello `status` field set toofalse.</span></span> <span data-ttu-id="ba4ea-740">Hello `errorMessage` a `statusCode` vlastnosti označí hello důvod hello indexování Chyba:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-740">hello `errorMessage` and `statusCode` properties will indicate hello reason for hello indexing error:</span></span>

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "hello search service is too busy tooprocess this document. Please try again later.",
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
          "errorMessage": "Index is temporarily unavailable because it was updated with hello 'allowIndexDowntime' flag set too'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

<span data-ttu-id="ba4ea-741">Hello následující tabulka vysvětluje různé každý dokument stavové kódy, které mohou být vráceny v odpovědi hello hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-741">hello following table explains hello various per-document status codes that can be returned in hello response.</span></span> <span data-ttu-id="ba4ea-742">Všimněte si, že některé naznačují potíže s hello požádat samostatně, zatímco ostatní označují dočasné chybový stav.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-742">Note that some indicate problems with hello request itself, while others indicate temporary error conditions.</span></span> <span data-ttu-id="ba4ea-743">Hello pozdější opakujte po prodlevě.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-743">hello latter you should retry after a delay.</span></span>

<table style="font-size:12">
    <tr>
        <th><span data-ttu-id="ba4ea-744">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="ba4ea-744">Status code</span></span></th>
        <th><span data-ttu-id="ba4ea-745">Význam</span><span class="sxs-lookup"><span data-stu-id="ba4ea-745">Meaning</span></span></th>
        <th><span data-ttu-id="ba4ea-746">Opakovatelná</span><span class="sxs-lookup"><span data-stu-id="ba4ea-746">Retryable</span></span></th>
        <th><span data-ttu-id="ba4ea-747">Poznámky</span><span class="sxs-lookup"><span data-stu-id="ba4ea-747">Notes</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-748">200</span><span class="sxs-lookup"><span data-stu-id="ba4ea-748">200</span></span></td>
        <td><span data-ttu-id="ba4ea-749">Dokument byl úspěšně upravit nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-749">Document was successfully modified or deleted.</span></span></td>
        <td><span data-ttu-id="ba4ea-750">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="ba4ea-750">n/a</span></span></td>
        <td><span data-ttu-id="ba4ea-751">Operace odstranění se <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-751">Delete operations are <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span></span> <span data-ttu-id="ba4ea-752">To znamená i v případě, že klíč dokumentu neexistuje v indexu hello, pokusu o operaci odstranění s tímto klíčem bude mít za následek 200 stavový kód.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-752">That is, even if a document key does not exist in hello index, attempting a delete operation with that key will result in a 200 status code.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-753">201</span><span class="sxs-lookup"><span data-stu-id="ba4ea-753">201</span></span></td>
        <td><span data-ttu-id="ba4ea-754">Dokument byl úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-754">Document was successfully created.</span></span></td>
        <td><span data-ttu-id="ba4ea-755">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="ba4ea-755">n/a</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-756">400</span><span class="sxs-lookup"><span data-stu-id="ba4ea-756">400</span></span></td>
        <td><span data-ttu-id="ba4ea-757">Došlo k chybě v hello dokumentu, která zabránila z indexování.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-757">There was an error in hello document that prevented it from being indexed.</span></span></td>
        <td><span data-ttu-id="ba4ea-758">Ne</span><span class="sxs-lookup"><span data-stu-id="ba4ea-758">No</span></span></td>
        <td><span data-ttu-id="ba4ea-759">Hello chybovou zprávu ve hello odpovědi bude určení toho, co se hello dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-759">hello error message in hello response will indicate what is wrong with hello document.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-760">404</span><span class="sxs-lookup"><span data-stu-id="ba4ea-760">404</span></span></td>
        <td><span data-ttu-id="ba4ea-761">Hello dokument nelze sloučit, protože neexistuje hello zadaný klíč v indexu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-761">hello document could not be merged because hello given key doesn't exist in hello index.</span></span></td>
        <td><span data-ttu-id="ba4ea-762">Ne</span><span class="sxs-lookup"><span data-stu-id="ba4ea-762">No</span></span></td>
        <td><span data-ttu-id="ba4ea-763">Této chybě nedochází pro nahrávání vzhledem k tomu, že se vytváření nových dokumentů a nespustí se pro odstranění vzhledem k tomu, že jsou <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-763">This error does not occur for uploads since they create new documents, and it does not occur for deletes because they are <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-764">409</span><span class="sxs-lookup"><span data-stu-id="ba4ea-764">409</span></span></td>
        <td><span data-ttu-id="ba4ea-765">Při pokusu o tooindex dokument byl zjištěn konflikt verzí.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-765">A version conflict was detected when attempting tooindex a document.</span></span></td>
        <td><span data-ttu-id="ba4ea-766">Ano</span><span class="sxs-lookup"><span data-stu-id="ba4ea-766">Yes</span></span></td>
        <td><span data-ttu-id="ba4ea-767">Tomu může dojít, když se pokoušíte tooindex hello stejné dokumentu více než jednou současně.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-767">This can happen when you're trying tooindex hello same document more than once concurrently.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-768">422</span><span class="sxs-lookup"><span data-stu-id="ba4ea-768">422</span></span></td>
        <td><span data-ttu-id="ba4ea-769">Hello index je dočasně nedostupný, protože byla aktualizována s hello allowIndexDowntime příznak sadu too'true'.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-769">hello index is temporarily unavailable because it was updated with hello 'allowIndexDowntime' flag set too'true'.</span></span></td>
        <td><span data-ttu-id="ba4ea-770">Ano</span><span class="sxs-lookup"><span data-stu-id="ba4ea-770">Yes</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="ba4ea-771">503</span><span class="sxs-lookup"><span data-stu-id="ba4ea-771">503</span></span></td>
        <td><span data-ttu-id="ba4ea-772">Vyhledávací služba je dočasně nedostupná, pravděpodobně z důvodu tooheavy zatížení.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-772">Your search service is temporarily unavailable, possibly due tooheavy load.</span></span></td>
        <td><span data-ttu-id="ba4ea-773">Ano</span><span class="sxs-lookup"><span data-stu-id="ba4ea-773">Yes</span></span></td>
        <td><span data-ttu-id="ba4ea-774">Váš kód čekat, než v tomto případě nebo riziko prodloužit nedostupnost služby hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-774">Your code should wait before retrying in this case or you risk prolonging hello service unavailability.</span></span></td>
    </tr>
</table> 

<span data-ttu-id="ba4ea-775">**Poznámka:**: Pokud váš klientský kód často zaznamená 207 odpověď, jedním z možných důvodů je, že systém hello je zatížení.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-775">**Note**: If your client code frequently encounters a 207 response, one possible reason is that hello system is under load.</span></span> <span data-ttu-id="ba4ea-776">Můžete to ověřit kontrolou hello `statusCode` vlastnost 503.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-776">You can confirm this by checking hello `statusCode` property for 503.</span></span> <span data-ttu-id="ba4ea-777">Pokud se jedná o hello případ, doporučujeme ***omezení indexování požadavky***.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-777">If this is hello case, we recommend ***throttling indexing requests***.</span></span> <span data-ttu-id="ba4ea-778">Jinak hodnota pokud není subside indexování provoz, hello systém může spustit odmítat všechny požadavky s 503 chyby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-778">Otherwise, if indexing traffic doesn't subside, hello system could start rejecting all requests with 503 errors.</span></span>

<span data-ttu-id="ba4ea-779">Kód stavu 429 označuje, že jste překročili kvótu hello počet dokumentů na index.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-779">Status code 429 indicates that you have exceeded your quota on hello number of documents per index.</span></span> <span data-ttu-id="ba4ea-780">Musíte vytvořit nový index nebo upgrade na vyšší limity kapacity.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-780">You must either create a new index or upgrade for higher capacity limits.</span></span>

<span data-ttu-id="ba4ea-781">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-781">**Example:**</span></span>

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

## <a name="search-documents"></a><span data-ttu-id="ba4ea-782">Vyhledávání dokumentů</span><span class="sxs-lookup"><span data-stu-id="ba4ea-782">Search Documents</span></span>
<span data-ttu-id="ba4ea-783">A **vyhledávání** operace se objeví jako požadavek GET nebo POST a určuje parametry, které poskytují hello kritéria pro výběr odpovídající dokumenty.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-783">A **Search** operation is issued as a GET or POST request and specifies parameters that give hello criteria for selecting matching documents.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

<span data-ttu-id="ba4ea-784">**Když toouse POST místo GET**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-784">**When toouse POST instead of GET**</span></span>

<span data-ttu-id="ba4ea-785">Při použití metody GET protokolu HTTP hello toocall **vyhledávání** rozhraní API, musíte vědět, že nesmí překročit délka hello adresy URL žádosti hello 8 KB toobe.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-785">When you use HTTP GET toocall hello **Search** API, you need toobe aware that hello length of hello request URL cannot exceed 8 KB.</span></span> <span data-ttu-id="ba4ea-786">Je to obvykle dostatečně pro většinu aplikací.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-786">This is usually enough for most applications.</span></span> <span data-ttu-id="ba4ea-787">Některé aplikace však vytvořit velmi velké dotazy nebo výrazy filtru OData.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-787">However, some applications produce very large queries or OData filter expressions.</span></span> <span data-ttu-id="ba4ea-788">Pro tyto aplikace pomocí HTTP POST je lepší volbou, protože umožňuje větší filtry a dotazy, než metoda GET.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-788">For these applications, using HTTP POST is a better choice because it allows larger filters and queries than GET.</span></span> <span data-ttu-id="ba4ea-789">S POST, počet hello termíny nebo klauzule v dotazu je hello omezení faktoru, není hello velikost hello nezpracovaná dotazu vzhledem k tomu, že omezení velikosti hello požadavku pro metodu POST je přibližně 16 MB.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-789">With POST, hello number of terms or clauses in a query is hello limiting factor, not hello size of hello raw query since hello request size limit for POST is approximately 16 MB.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-790">I když limit velikosti požadavek POST hello je moc velká, nemůže být libovolně komplexní vyhledávací dotazy a výrazy filtru.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-790">Even though hello POST request size limit is very large, search queries and filter expressions cannot be arbitrarily complex.</span></span> <span data-ttu-id="ba4ea-791">V tématu [syntaxe dotazů Lucene](https://msdn.microsoft.com/library/mt589323.aspx) a [syntaxe výrazu OData](https://msdn.microsoft.com/library/dn798921.aspx) pro další informace o omezeních složitost vyhledávání dotazu a filtr.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-791">See [Lucene query syntax](https://msdn.microsoft.com/library/mt589323.aspx) and [OData expression syntax](https://msdn.microsoft.com/library/dn798921.aspx) for more information about search query and filter complexity limitations.</span></span>
> 
> 

<span data-ttu-id="ba4ea-792">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-792">**Request**</span></span>

<span data-ttu-id="ba4ea-793">Protokol HTTPS je vyžadována pro žádosti o služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-793">HTTPS is required for service requests.</span></span> <span data-ttu-id="ba4ea-794">Hello **vyhledávání** požadavek se dá vytvořit pomocí hello GET nebo POST metody.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-794">hello **Search** request can be constructed using hello GET or POST methods.</span></span>

<span data-ttu-id="ba4ea-795">identifikátor URI požadavku Hello Určuje, které tooquery indexu, pro všechny dokumenty, které odpovídají hello parametry.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-795">hello request URI specifies which index tooquery, for all documents that match hello parameters.</span></span> <span data-ttu-id="ba4ea-796">Parametry jsou zadány na hello řetězec dotazu v případě hello požadavků GET a v žádosti o hello textu v případě hello POST požadavky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-796">Parameters are specified on hello query string in hello case of GET requests, and in hello request body in hello case of POST requests.</span></span>

<span data-ttu-id="ba4ea-797">Jako osvědčený postup při vytváření požadavky GET, mějte na paměti, příliš[kódování URL](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specifického dotazu parametry při volání metody hello REST API přímo.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-797">As a best practice when creating GET requests, remember too[URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specific query parameters when calling hello REST API directly.</span></span> <span data-ttu-id="ba4ea-798">Pro **vyhledávání** operace, to zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-798">For **Search** operations, this includes:</span></span>

* `$filter`
* `facet`
* `highlightPreTag`
* `highlightPostTag`
* `search`
* `moreLikeThis`

<span data-ttu-id="ba4ea-799">Kódování URL se doporučuje jenom na hello výše parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-799">URL encoding is only recommended on hello above query parameters.</span></span> <span data-ttu-id="ba4ea-800">Pokud jste omylem kódování URL hello řetězec dotazu celý, (vše za hello?), dojde k přerušení požadavky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-800">If you inadvertently URL-encode hello entire query string (everything after hello ?), requests will break.</span></span>

<span data-ttu-id="ba4ea-801">Navíc kódování URL je nutné pouze při volání metody hello získat přímo pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-801">Also, URL encoding is only necessary when calling hello REST API directly using GET.</span></span> <span data-ttu-id="ba4ea-802">Žádné kódování URL je nezbytné při volání metody **vyhledávání** pomocí POST, nebo při použití hello [klientské knihovny .NET](https://msdn.microsoft.com/library/dn951165.aspx), která zpracovává kódování URL za vás.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-802">No URL encoding is necessary when calling **Search** using POST, or when using hello [.NET client library](https://msdn.microsoft.com/library/dn951165.aspx), which handles URL encoding for you.</span></span>

<span data-ttu-id="ba4ea-803"><a name="SearchQueryParameters"></a>
**Parametry dotazu**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-803"><a name="SearchQueryParameters"></a>
**Query Parameters**</span></span>

<span data-ttu-id="ba4ea-804">**Hledání** přijímá několik parametrů, které poskytují kritéria dotazu a také určit chování vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-804">**Search** accepts several parameters that provide query criteria and also specify search behavior.</span></span> <span data-ttu-id="ba4ea-805">Zadejte tyto parametry v adrese URL hello řetězec dotazu při volání metody **vyhledávání** prostřednictvím GET a jako vlastnosti JSON v textu žádosti hello při volání metody **vyhledávání** přes POST.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-805">You provide these parameters in hello URL query string when calling **Search** via GET, and as JSON properties in hello request body when calling **Search** via POST.</span></span> <span data-ttu-id="ba4ea-806">Hello syntaxe pro některé parametry se mírně liší mezi GET a POST.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-806">hello syntax for some parameters is slightly different between GET and POST.</span></span> <span data-ttu-id="ba4ea-807">Tyto rozdíly jsou popsány podle vhodnosti níže:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-807">These differences are noted as applicable below:</span></span>

<span data-ttu-id="ba4ea-808">`search=[string]`(volitelné) – hello toosearch text pro.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-808">`search=[string]` (optional) - hello text toosearch for.</span></span> <span data-ttu-id="ba4ea-809">Všechny `searchable` prohledány jsou ve výchozím nastavení není-li `searchFields` je zadán.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-809">All `searchable` fields are searched by default unless `searchFields` is specified.</span></span> <span data-ttu-id="ba4ea-810">Při hledání `searchable` tokenizovaného polí a hledaný text hello, sám sebe, tak více podmínek je možné oddělit prázdné znaky (například: `search=hello world`).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-810">When searching `searchable` fields, hello search text itself is tokenized, so multiple terms can be separated by white space (for example: `search=hello world`).</span></span> <span data-ttu-id="ba4ea-811">použít všechny termín toomatch `*` (může to být užitečné pro dotazy Booleovský filtr).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-811">toomatch any term, use `*` (this can be useful for boolean filter queries).</span></span> <span data-ttu-id="ba4ea-812">Tento parametr vynechá má stejné jako jeho nastavení příliš ovlivňuje hello`*`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-812">Omitting this parameter has hello same effect as setting it too`*`.</span></span> <span data-ttu-id="ba4ea-813">V tématu [jednoduchá syntaxe dotazů](https://msdn.microsoft.com/library/dn798920.aspx) pro konkrétní na syntaxe vyhledávání hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-813">See [Simple Query Syntax](https://msdn.microsoft.com/library/dn798920.aspx) for specifics on hello search syntax.</span></span>

* <span data-ttu-id="ba4ea-814">**Poznámka:**: hello výsledky mohou být někdy překvapivé při dotazování přes `searchable` pole.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-814">**Note**: hello results can sometimes be surprising when querying over `searchable` fields.</span></span> <span data-ttu-id="ba4ea-815">tokenizátor Hello obsahuje logiku toohandle případech běžné tooEnglish text jako apostrofy, čárky v čísel atd. Například `search=123,456` bude shodovat s jeden termín 123,456 místo jednotlivých podmínky hello 123 a 456, protože čárkami jsou použity jako oddělovače tisíc pro velké počty v angličtině.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-815">hello tokenizer includes logic toohandle cases common tooEnglish text like apostrophes, commas in numbers, etc. For example, `search=123,456` will match a single term 123,456 rather than hello individual terms 123 and 456, since commas are used as thousand-separators for large numbers in English.</span></span> <span data-ttu-id="ba4ea-816">Z tohoto důvodu doporučujeme používat mezer spíše než interpunkce tooseparate podmínky v hello `search` parametr.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-816">For this reason, we recommend using white space rather than punctuation tooseparate terms in hello `search` parameter.</span></span>

<span data-ttu-id="ba4ea-817">`searchMode=any|all`(volitelné, použije se výchozí hodnota příliš`any`) – ať některého nebo všech hello hledaných termínů musí odpovídat pořadí toocount hello dokumentu jako shoda.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-817">`searchMode=any|all` (optional, defaults too`any`) - whether any or all of hello search terms must be matched in order toocount hello document as a match.</span></span>

<span data-ttu-id="ba4ea-818">`searchFields=[string]`(volitelné) – seznam hello toosearch názvy polí oddělených čárkou pro hello zadaný text.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-818">`searchFields=[string]` (optional) - hello list of comma-separated field names toosearch for hello specified text.</span></span> <span data-ttu-id="ba4ea-819">Cílová pole. musí být označen jako `searchable`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-819">Target fields must be marked as `searchable`.</span></span>

<span data-ttu-id="ba4ea-820">`queryType=simple|full`(volitelné, použije se výchozí hodnota příliš`simple`) – Pokud je sada příliš "jednoduchý" hledaný text interpretovat pomocí jednoduchého dotazovací jazyk, který umožňuje symbolů, jako +, * a "".</span><span class="sxs-lookup"><span data-stu-id="ba4ea-820">`queryType=simple|full` (optional, defaults too`simple`) - when set too"simple" search text is interpreted using a simple query language that allows for symbols such as +, * and "".</span></span> <span data-ttu-id="ba4ea-821">Dotazy jsou vyhodnocovány napříč všechna prohledatelná pole (nebo pole uvedené v `searchFields`) v každém dokumentu ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-821">Queries are evaluated across all searchable fields (or fields indicated in `searchFields`) in each document by default.</span></span> <span data-ttu-id="ba4ea-822">Pokud je typ dotazu hello nastavená příliš`full` hledaný text interpretována pomocí jazyka dotazů Lucene hello, což umožňuje specifické pole a vyvážené hledání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-822">When hello query type is set too`full` search text is interpreted using hello Lucene query language which allows field-specific and weighted searches.</span></span> <span data-ttu-id="ba4ea-823">V tématu [jednoduchá syntaxe dotazů](https://msdn.microsoft.com/library/dn798920.aspx) a [syntaxe dotazů Lucene](https://msdn.microsoft.com/library/mt589323.aspx) pro konkrétní na syntaxe vyhledávání hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-823">See [Simple Query Syntax](https://msdn.microsoft.com/library/dn798920.aspx) and [Lucene Query Syntax](https://msdn.microsoft.com/library/mt589323.aspx) for specifics on hello search syntaxes.</span></span> 

> [!NOTE]
> <span data-ttu-id="ba4ea-824">Rozsah vyhledávání v hello Lucene dotazovací jazyk nepodporuje považuje $filter který nabízí podobné funkce.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-824">Range search in hello Lucene query language is not supported in favor of $filter which offers similar functionality.</span></span>
> 
> 

<span data-ttu-id="ba4ea-825">`moreLikeThis=[key]`(volitelné) **Důležité:** tato funkce je k dispozici v `2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-825">`moreLikeThis=[key]` (optional) **Important:** This feature is only available in `2015-02-28-Preview`.</span></span> <span data-ttu-id="ba4ea-826">Tuto možnost nelze použít v dotazu, který obsahuje parametr hledání textu hello `search=[string]`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-826">This option cannot be used in a query that contains hello text search parameter, `search=[string]`.</span></span> <span data-ttu-id="ba4ea-827">Hello `moreLikeThis` parametr Vyhledá dokumenty, které jsou podobné toohello dokumentu určeného klíč dokumentu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-827">hello `moreLikeThis` parameter finds documents that are similar toohello document specified by hello document key.</span></span> <span data-ttu-id="ba4ea-828">Při hledání požadavku s `moreLikeThis`, vygeneruje se seznam podmínek vyhledávání, na základě frekvence hello a vzácnost podmínkami hello zdrojový dokument.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-828">When a search request is made with `moreLikeThis`, a list of search terms is generated based on hello frequency and rarity of terms in hello source document.</span></span> <span data-ttu-id="ba4ea-829">Tyto podmínky jsou pak použít toomake hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-829">Those terms are then used toomake hello request.</span></span> <span data-ttu-id="ba4ea-830">Ve výchozím nastavení, hello obsah všech `searchable` pole jsou považovány za Pokud `searchFields` je použité toorestrict pole, která vyhledávají.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-830">By default, hello contents of all `searchable` fields are considered unless `searchFields` is used toorestrict which fields are searched.</span></span>  

<span data-ttu-id="ba4ea-831">`$skip=#`(volitelné) – hello počet vyhledávání výsledků tooskip; Nemůže být vyšší než 100 000.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-831">`$skip=#` (optional) - hello number of search results tooskip; Cannot be greater than 100,000.</span></span> <span data-ttu-id="ba4ea-832">Pokud potřebujete tooscan dokumentů v pořadí, ale nemůžete použít `$skip` z důvodu omezení toothis, zvažte použití `$orderby` na klíč zcela seřazené a `$filter` s rozsahem dotazu místo.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-832">If you need tooscan documents in sequence but cannot use `$skip` due toothis limitation, consider using `$orderby` on a totally-ordered key and `$filter` with a range query instead.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-833">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `skip` místo `$skip`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-833">When calling **Search** using POST, this parameter is named `skip` instead of `$skip`.</span></span>
> 
> 

<span data-ttu-id="ba4ea-834">`$top=#`(volitelné) – hello počet vyhledávání výsledků tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-834">`$top=#` (optional) - hello number of search results tooretrieve.</span></span> <span data-ttu-id="ba4ea-835">To lze použít ve spojení s `$skip` tooimplement klienta stránkování výsledků vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-835">This can be used in conjunction with `$skip` tooimplement client-side paging of search results.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-836">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `top` místo `$top`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-836">When calling **Search** using POST, this parameter is named `top` instead of `$top`.</span></span>
> 
> 

<span data-ttu-id="ba4ea-837">`$count=true|false`(volitelné, použije se výchozí hodnota příliš`false`)-určuje, zda toofetch hello celkový počet výsledků.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-837">`$count=true|false` (optional, defaults too`false`) - Specifies whether toofetch hello total count of results.</span></span> <span data-ttu-id="ba4ea-838">Toto je počet hello všechny dokumenty, které odpovídají hello `search` a `$filter` parametry, ignoruje `$top` a `$skip`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-838">This is hello count of all documents that match hello `search` and `$filter` parameters, ignoring `$top` and `$skip`.</span></span> <span data-ttu-id="ba4ea-839">Nastavení této hodnoty příliš`true` může mít dopad na výkon.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-839">Setting this value too`true` may have a performance impact.</span></span> <span data-ttu-id="ba4ea-840">Všimněte si, že vrácený počet hello je sblížení.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-840">Note that hello count returned is an approximation.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-841">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `count` místo `$count`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-841">When calling **Search** using POST, this parameter is named `count` instead of `$count`.</span></span>
> 
> 

<span data-ttu-id="ba4ea-842">`$orderby=[string]`(volitelné) – seznam oddělený čárkami výrazy toosort hello výsledků podle.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-842">`$orderby=[string]` (optional) - A list of comma-separated expressions toosort hello results by.</span></span> <span data-ttu-id="ba4ea-843">Každý výraz může být název pole nebo volání toohello `geo.distance()` funkce.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-843">Each expression can be either a field name or a call toohello `geo.distance()` function.</span></span> <span data-ttu-id="ba4ea-844">Každý výraz může následovat `asc` tooindicated vzestupné řazení a `desc` tooindicate sestupně.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-844">Each expression can be followed by `asc` tooindicated ascending, and `desc` tooindicate descending.</span></span> <span data-ttu-id="ba4ea-845">Výchozí Hello je vzestupné pořadí.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-845">hello default is ascending order.</span></span> <span data-ttu-id="ba4ea-846">TIES bude rozděleno podle hello shodu skóre dokumentů.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-846">Ties will be broken by hello match scores of documents.</span></span> <span data-ttu-id="ba4ea-847">Pokud žádné `$orderby` je zadána výchozí pořadí hello sestupné podle skóre shodu dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-847">If no `$orderby` is specified, hello default sort order is descending by document match score.</span></span> <span data-ttu-id="ba4ea-848">Existuje limit 32 klauzule pro `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-848">There is a limit of 32 clauses for `$orderby`.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-849">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `orderby` místo `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-849">When calling **Search** using POST, this parameter is named `orderby` instead of `$orderby`.</span></span>
> 
> 

<span data-ttu-id="ba4ea-850">`$select=[string]`(volitelné) – seznam tooretrieve polí oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-850">`$select=[string]` (optional) - A list of comma-separated fields tooretrieve.</span></span> <span data-ttu-id="ba4ea-851">Pokud tento parametr zadán, jsou zahrnuty všechna pole označeno získat ve schématu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-851">If unspecified, all fields marked as retrievable in hello schema are included.</span></span> <span data-ttu-id="ba4ea-852">Můžete také explicitní žádost o všechna pole nastavením tento parametr příliš`*`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-852">You can also explicitly request all fields by setting this parameter too`*`.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-853">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `select` místo `$select`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-853">When calling **Search** using POST, this parameter is named `select` instead of `$select`.</span></span>
> 
> 

<span data-ttu-id="ba4ea-854">`facet=[string]`(nula nebo více) – pole toofacet podle.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-854">`facet=[string]` (zero or more) - A field toofacet by.</span></span> <span data-ttu-id="ba4ea-855">Řetězec hello může volitelně obsahovat parametry toocustomize hello používání faset vyjádřený jako textový soubor s oddělovači `name:value` páry.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-855">Optionally hello string may contain parameters toocustomize hello faceting expressed as comma-separated `name:value` pairs.</span></span> <span data-ttu-id="ba4ea-856">Jsou platné parametry:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-856">Valid parameters are:</span></span>

* <span data-ttu-id="ba4ea-857">`count`(maximální počet omezující vlastnosti podmínky; výchozí hodnota je 10).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-857">`count` (max number of facet terms; default is 10).</span></span> <span data-ttu-id="ba4ea-858">Neexistuje žádné maximální, ale vyšší hodnoty zpoplatněná odpovídající snížení výkonu, zejména pokud Fasetové pole hello obsahuje velký počet jedinečných podmínky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-858">There is no maximum, but higher values incur a corresponding performance penalty, especially if hello faceted field contains a large number of unique terms.</span></span>
  * <span data-ttu-id="ba4ea-859">Příklad: `facet=category,count:5` získá hello nejvyšší pěti kategorií ve výsledcích omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-859">For example: `facet=category,count:5` gets hello top five categories in facet results.</span></span>  
  * <span data-ttu-id="ba4ea-860">**Poznámka:**: Pokud hello `count` parametru je menší než počet hello jedinečný podmínky, nemusí být přesné výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-860">**Note**: If hello `count` parameter is less than hello number of unique terms, hello results may not be accurate.</span></span> <span data-ttu-id="ba4ea-861">Toto je z důvodu toohello způsob používání omezujících vlastností dotazy jsou rozmístěny v horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-861">This is due toohello way faceting queries are distributed across shards.</span></span> <span data-ttu-id="ba4ea-862">Zvýšení `count` obvykle zvyšuje hello přesnost počtů hello termín, ale v snížený výkon.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-862">Increasing `count` generally increases hello accuracy of hello term counts, but at a performance cost.</span></span>
* <span data-ttu-id="ba4ea-863">`sort`(mezi `count` toosort *sestupném* podle počtu, `-count` toosort *vzestupném* podle počtu, `value` toosort *vzestupném* podle hodnoty, nebo `-value` toosort *sestupném* podle hodnoty)</span><span class="sxs-lookup"><span data-stu-id="ba4ea-863">`sort` (one of `count` toosort *descending* by count, `-count` toosort *ascending* by count, `value` toosort *ascending* by value, or `-value` toosort *descending* by value)</span></span>
  * <span data-ttu-id="ba4ea-864">Příklad: `facet=category,count:3,sort:count` získá hello nejvyšší tří kategorií ve výsledcích omezující vlastnosti v sestupném pořadí podle počtu hello dokumentů s každý název města.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-864">For example: `facet=category,count:3,sort:count` gets hello top three categories in facet results in descending order by hello number of documents with each city name.</span></span> <span data-ttu-id="ba4ea-865">Například pokud hello nejvyšší tří kategorií jsou rozpočtu, Motel a možnost a nároky má 5 přístupů, Motel má 6 a možnost má 4, pak hello kbelíků bude v pořadí hello Motel, rozpočtu, možnost.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-865">For example, if hello top three categories are Budget, Motel, and Luxury, and Budget has 5 hits, Motel has 6, and Luxury has 4, then hello buckets will be in hello order Motel, Budget, Luxury.</span></span>
  * <span data-ttu-id="ba4ea-866">Příklad: `facet=rating,sort:-value` vytváří kbelíků pro všechny možné hodnocení v sestupném pořadí podle hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-866">For example: `facet=rating,sort:-value` produces buckets for all possible ratings, in descending order by value.</span></span> <span data-ttu-id="ba4ea-867">Například pokud hello hodnocení ze 1 too5, kbelíků hello bude objednáno 5, 4, 3, 2, 1, bez ohledu na to, kolik dokumenty odpovídají každé hodnocení.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-867">For example, if hello ratings are from 1 too5, hello buckets will be ordered 5, 4, 3, 2, 1, irrespective of how many documents match each rating.</span></span>
* <span data-ttu-id="ba4ea-868">`values`(oddělený kanálu číselný nebo `Edm.DateTimeOffset` hodnoty určující dynamickou sadu hodnot omezující vlastnosti položky)</span><span class="sxs-lookup"><span data-stu-id="ba4ea-868">`values` (pipe-delimited numeric or `Edm.DateTimeOffset` values specifying a dynamic set of facet entry values)</span></span>
  * <span data-ttu-id="ba4ea-869">Příklad: `facet=baseRate,values:10|20` vytvoří tři kbelíků: jeden pro základní míra 0 až toobut není včetně 10, jeden pro 10 až toobut bez zahrnutí 20 a jeden pro 20 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-869">For example: `facet=baseRate,values:10|20` produces three buckets: One for base rate 0 up toobut not including 10, one for 10 up toobut not including 20, and one for 20 or higher.</span></span>
  * <span data-ttu-id="ba4ea-870">Příklad: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` vytvoří dvě kbelíků: jeden pro hotels renovovanou před. února 2010 a jeden pro hotels renovovanou února 1. 2010 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-870">For example: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` produces two buckets: One for hotels renovated before February 2010, and one for hotels renovated February 1st, 2010 or later.</span></span>
* <span data-ttu-id="ba4ea-871">`interval`(interval celé číslo větší než 0. v případě čísel nebo `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` pro hodnoty času datum)</span><span class="sxs-lookup"><span data-stu-id="ba4ea-871">`interval` (integer interval greater than 0 for numbers, or `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` for date time values)</span></span>
  * <span data-ttu-id="ba4ea-872">Příklad: `facet=baseRate,interval:100` vytváří kbelíků založené na základní míra rozsahy velikost 100.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-872">For example: `facet=baseRate,interval:100` produces buckets based on base rate ranges of size 100.</span></span> <span data-ttu-id="ba4ea-873">Například pokud základní sazby všechny až 60 $ $600, budou existovat intervaly 0-100, 100 200, 200 300, 300 400, 400-500 a 500 600.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-873">For example, if base rates are all between $60 and $600, there will be buckets for 0-100, 100-200, 200-300, 300-400, 400-500, and 500-600.</span></span>
  * <span data-ttu-id="ba4ea-874">Příklad: `facet=lastRenovationDate,interval:year` vytváří jeden sady pro každý rok po hotels byly renovovanou.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-874">For example: `facet=lastRenovationDate,interval:year` produces one bucket for each year when hotels were renovated.</span></span>
* <span data-ttu-id="ba4ea-875">`timeoffset`([+-] hh: mm, [+-] hh: mm, nebo [+-] hh) `timeoffset` je volitelný.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-875">`timeoffset` ([+-]hh:mm, [+-]hhmm, or [+-]hh) `timeoffset` is optional.</span></span> <span data-ttu-id="ba4ea-876">Můžete sloučit pouze s hello `interval` možnost a pouze tehdy, když použité tooa pole typu `Edm.DateTimeOffset`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-876">It can only be combined with hello `interval` option, and only when applied tooa field of type `Edm.DateTimeOffset`.</span></span> <span data-ttu-id="ba4ea-877">Hodnota Hello určuje hello UTC Čas posunutí tooaccount pro nastavení času hranice.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-877">hello value specifies hello UTC time offset tooaccount for in setting time boundaries.</span></span>
  * <span data-ttu-id="ba4ea-878">Příklad: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` používá hello den hranice, která začíná na UTC 01:00:00 (půlnoc v hello cílové časové pásmo)</span><span class="sxs-lookup"><span data-stu-id="ba4ea-878">For example: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` uses hello day boundary that starts at 01:00:00 UTC (midnight in hello target time zone)</span></span>
* <span data-ttu-id="ba4ea-879">**Poznámka:**: `count` a `sort` lze spojit do hello stejné omezující vlastnost specifikace, ale nelze kombinovat s `interval` nebo `values`, a `interval` a `values` nemůže být společně kombinovány.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-879">**Note**: `count` and `sort` can be combined in hello same facet specification, but they cannot be combined with `interval` or `values`, and `interval` and `values` cannot be combined together.</span></span>
* <span data-ttu-id="ba4ea-880">**Poznámka:**: omezující vlastnosti Interval na datum a čas se vypočítávají podle času UTC, pokud `timeoffset` není zadán.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-880">**Note**: Interval facets on date time are computed based on UTC time if `timeoffset` is not specified.</span></span> <span data-ttu-id="ba4ea-881">Příklad: pro `facet=lastRenovationDate,interval:day`, hello den hranic začíná na 00:00:00 UTC.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-881">For example: for `facet=lastRenovationDate,interval:day`, hello day boundary starts at 00:00:00 UTC.</span></span> 

> [!NOTE]
> <span data-ttu-id="ba4ea-882">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `facets` místo `facet`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-882">When calling **Search** using POST, this parameter is named `facets` instead of `facet`.</span></span> <span data-ttu-id="ba4ea-883">Také můžete zadat jako pole JSON řetězců, kde každý řetězec je výraz samostatné omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-883">Also, you specify it as a JSON array of strings where each string is a separate facet expression.</span></span>
> 
> 

<span data-ttu-id="ba4ea-884">`$filter=[string]`(volitelné) – výraz strukturovaných vyhledávání v standardní syntaxi OData.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-884">`$filter=[string]` (optional) - A structured search expression in standard OData syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-885">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `filter` místo `$filter`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-885">When calling **Search** using POST, this parameter is named `filter` instead of `$filter`.</span></span>
> 
> 

<span data-ttu-id="ba4ea-886">`highlight=[string]`(volitelné) – označuje sadu názvů polí oddělených čárkami, které používá pro stiskněte klávesu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-886">`highlight=[string]` (optional) - A set of comma-separated field names used for hit highlights.</span></span> <span data-ttu-id="ba4ea-887">Pouze `searchable` pole lze použít pro přístupů zvýraznění.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-887">Only `searchable` fields can be used for hit highlighting.</span></span>

<span data-ttu-id="ba4ea-888">`highlightPreTag=[string]`(volitelné, použije se výchozí hodnota příliš`<em>`) – řetězec značky, která přidá toohit označuje.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-888">`highlightPreTag=[string]` (optional, defaults too`<em>`) - A string tag that prepends toohit highlights.</span></span> <span data-ttu-id="ba4ea-889">Musí být nastavena s `highlightPostTag`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-889">Must be set with `highlightPostTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-890">Při volání metody **vyhledávání** pomocí GET, vyhrazené znaky v adrese URL hello musí být kódovaný v procentech (například místo # % 23).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-890">When calling **Search** using GET, reserved characters in hello URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="ba4ea-891">`highlightPostTag=[string]`(volitelné, použije se výchozí hodnota příliš`</em>`)-značku řetězec, který připojí toohit označuje.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-891">`highlightPostTag=[string]` (optional, defaults too`</em>`) - a string tag that appends toohit highlights.</span></span> <span data-ttu-id="ba4ea-892">Musí být nastavena s `highlightPreTag`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-892">Must be set with `highlightPreTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-893">Při volání metody **vyhledávání** pomocí GET, vyhrazené znaky v adrese URL hello musí být kódovaný v procentech (například místo # % 23).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-893">When calling **Search** using GET, reserved characters in hello URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="ba4ea-894">`scoringProfile=[string]`(volitelné) – název hello vyhodnocování tooevaluate profil odpovídat skóre pro odpovídající dokumenty ve výsledcích hello toosort pořadí.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-894">`scoringProfile=[string]` (optional) - hello name of a scoring profile tooevaluate match scores for matching documents in order toosort hello results.</span></span>

<span data-ttu-id="ba4ea-895">`scoringParameter=[string]`(nula nebo více) – označuje hello hodnoty pro každý parametr definovaný ve funkci vyhodnocování (například `referencePointParameter`) formátu hello `name-value1,value2,...`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-895">`scoringParameter=[string]` (zero or more) - Indicates hello values for each parameter defined in a scoring function (for example, `referencePointParameter`) using hello format `name-value1,value2,...`.</span></span>

* <span data-ttu-id="ba4ea-896">Například pokud hello vyhodnocování profil definuje funkci s parametr s názvem "mylocation" hello zadat parametr řetězce dotazu by `&scoringParameter=mylocation--122.2,44.8`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-896">For example, if hello scoring profile defines a function with a parameter called "mylocation" hello query string option would be `&scoringParameter=mylocation--122.2,44.8`.</span></span> <span data-ttu-id="ba4ea-897">první dash Hello hello název odděluje od hello seznam hodnot, zatímco druhý dash hello je součástí hello první hodnota (zeměpisné délky v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-897">hello first dash separates hello name from hello value list, while hello second dash is part of hello first value (longitude in this example).</span></span>
* <span data-ttu-id="ba4ea-898">Pro výpočet skóre parametry, jako je značky zvyšovat skóre, který může obsahovat čárky můžete vyhnuli tyto hodnoty v seznamu hello pomocí jednoduchých uvozovek a být.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-898">For scoring parameters such as for tag boosting that can contain commas, you can escape any such values in hello list using single quotes.</span></span> <span data-ttu-id="ba4ea-899">Pokud hello hodnoty, sami obsahovat jednoduché uvozovky, můžete je vyhnuli předchozího.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-899">If hello values themselves contain single quotes, you can escape them by doubling.</span></span>
  * <span data-ttu-id="ba4ea-900">Například pokud máte značku zvyšovat skóre parametr s názvem "mytag" a chcete tooboost na hello značky hodnoty "Hello, O'Brien" a "Smith", hello dotazu by možnost řetězce `&scoringParameter=mytag-'Hello, O''Brien',Smith`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-900">For example, if you have a tag boosting parameter called "mytag" and you want tooboost on hello tag values "Hello, O'Brien" and "Smith", hello query string option would be `&scoringParameter=mytag-'Hello, O''Brien',Smith`.</span></span> <span data-ttu-id="ba4ea-901">Všimněte si, že uvozovky jsou pouze požadované hodnoty, které obsahují čárkami.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-901">Note that quotes are only required for values that contain commas.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-902">Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `scoringParameters` místo `scoringParameter`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-902">When calling **Search** using POST, this parameter is named `scoringParameters` instead of `scoringParameter`.</span></span> <span data-ttu-id="ba4ea-903">Navíc je zadat jako pole JSON řetězců, kde je každý řetězec samostatné `name-values` pár.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-903">Also, you specify it as a JSON array of strings where each string is a separate `name-values` pair.</span></span>
> 
> 

<span data-ttu-id="ba4ea-904">`minimumCoverage`(volitelné, použije se výchozí hodnota too100) - číslo v rozsahu od 0 do 100 určující procento hello hello index, který musí být pokryté komponentami vyhledávací dotaz v pořadí pro hello dotazu toobe hlášené jako úspěšné.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-904">`minimumCoverage` (optional, defaults too100) - a number between 0 and 100 indicating hello percentage of hello index that must be covered by a search query in order for hello query toobe reported as a success.</span></span> <span data-ttu-id="ba4ea-905">Ve výchozím nastavení, musí být k dispozici celý index hello nebo `Search` vrátí stavový kód protokolu HTTP 503.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-905">By default, hello entire index must be available or `Search` will return HTTP status code 503.</span></span> <span data-ttu-id="ba4ea-906">Pokud nastavíte `minimumCoverage` a `Search` úspěšné, se vrátí HTTP 200 a zahrnout `@search.coverage` hodnotu v odpovědi hello označující hello procento hello index, který je zahrnutý v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-906">If you set `minimumCoverage` and `Search` succeeds, it will return HTTP 200 and include a `@search.coverage` value in hello response indicating hello percentage of hello index that was included in hello query.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-907">Nastavení této hodnoty parametru tooa menší než 100 může být užitečná pro zajištění dostupnosti vyhledávání i pro služby s pouze jednu repliku.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-907">Setting this parameter tooa value lower than 100 can be useful for ensuring search availability even for services with only one replica.</span></span> <span data-ttu-id="ba4ea-908">Ne všechny odpovídající dokumenty jsou však zaručena toobe existuje ve výsledcích hledání hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-908">However, not all matching documents are guaranteed toobe present in hello search results.</span></span> <span data-ttu-id="ba4ea-909">Pokud hledání odvolání je důležitější tooyour aplikace než dostupnosti, pak je nejlepší tooleave `minimumCoverage` na výchozí hodnotě 100.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-909">If search recall is more important tooyour application than availability, then it's best tooleave `minimumCoverage` at its default value of 100.</span></span>
> 
> 

<span data-ttu-id="ba4ea-910">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-910">`api-version=[string]` (required).</span></span> <span data-ttu-id="ba4ea-911">verze preview Hello je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-911">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="ba4ea-912">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-912">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="ba4ea-913">Poznámka: Pro tuto operaci hello `api-version` je zadána jako parametr dotazu v URL hello bez ohledu na to, jestli volání **vyhledávání** s GET nebo POST.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-913">Note: For this operation, hello `api-version` is specified as a query parameter in hello URL regardless of whether you call **Search** with GET or POST.</span></span>

<span data-ttu-id="ba4ea-914">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-914">**Request Headers**</span></span>

<span data-ttu-id="ba4ea-915">Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-915">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="ba4ea-916">`api-key`: hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-916">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="ba4ea-917">Je řetězcovou hodnotu, adresa URL služby jedinečný tooyour.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-917">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="ba4ea-918">Hello **vyhledávání** žádosti můžete zadat buď klíč správce, nebo klíč dotazu pro `api-key`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-918">hello **Search** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="ba4ea-919">Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-919">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="ba4ea-920">Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-920">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="ba4ea-921">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-921">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="ba4ea-922">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-922">**Request Body**</span></span>

<span data-ttu-id="ba4ea-923">U metody GET: žádné.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-923">For GET: None.</span></span>

<span data-ttu-id="ba4ea-924">Pro metodu POST:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-924">For POST:</span></span>

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered toodeclare query successful; default 100),
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

<span data-ttu-id="ba4ea-925">**Pokračování částečné vyhledávání odpovědí**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-925">**Continuation of Partial Search Responses**</span></span>

<span data-ttu-id="ba4ea-926">Někdy Azure Search nemůže vrátit, že všechny hello požadovaný má za následek odpověď o jedné vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-926">Sometimes Azure Search can't return all hello requested results in a single Search response.</span></span> <span data-ttu-id="ba4ea-927">K tomu může dojít různých důvodů, například kdy hello dotazu vyžadovali příliš mnoho dokumentů není zadáním `$top` nebo zadáte hodnotu `$top` , protože je příliš velký.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-927">This can happen for different reasons, such as when hello query requests too many documents by not specifying `$top` or specifying a value for `$top` that is too large.</span></span> <span data-ttu-id="ba4ea-928">V takových případech bude obsahovat Azure Search hello `@odata.nextLink` poznámky v textu hello odpověď a také `@search.nextPageParameters` Pokud byl požadavek POST.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-928">In such cases, Azure Search will include hello `@odata.nextLink` annotation in hello response body, and also `@search.nextPageParameters` if it was a POST request.</span></span> <span data-ttu-id="ba4ea-929">Můžete hello hodnoty těchto poznámek tooformulate jiné vyhledávání požadavku tooget hello další části hello hledání odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-929">You can use hello values of these annotations tooformulate another Search request tooget hello next part of hello search response.</span></span> <span data-ttu-id="ba4ea-930">Tento postup se nazývá ***pokračování*** hello původní žádost vyhledávání a hello poznámky se obvykle označují jako ***pokračování tokeny***.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-930">This is called a ***continuation*** of hello original Search request, and hello annotations are generally called ***continuation tokens***.</span></span> <span data-ttu-id="ba4ea-931">V tématu [hello příklad](#SearchResponse) podrobnosti o syntaxi hello těchto poznámek a jejich umístění v textu odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-931">See [hello example below](#SearchResponse) for details on hello syntax of these annotations and where they appear in hello response body.</span></span> 

<span data-ttu-id="ba4ea-932">Hello důvodů, proč Azure Search může vrátit pokračování tokeny jsou toochange závisí na implementaci a předmět.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-932">hello reasons why Azure Search might return continuation tokens are implementation-specific and subject toochange.</span></span> <span data-ttu-id="ba4ea-933">Robustní klientů musí být vždy připraven toohandle případy, kdy jsou vráceny méně dokumenty, než se očekává a je zahrnuté toocontinue načítání dokumenty token pokračování.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-933">Robust clients should always be ready toohandle cases where fewer documents than expected are returned and a continuation token is included toocontinue retrieving documents.</span></span> <span data-ttu-id="ba4ea-934">Také Upozorňujeme, že je nutné použít hello stejnou metodu HTTP jako původní žádost hello v toocontinue pořadí.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-934">Also note that you must use hello same HTTP method as hello original request in order toocontinue.</span></span> <span data-ttu-id="ba4ea-935">Například pokud jste odeslali požadavek GET, všechny žádosti pokračování odeslání musíte taky použít GET (a stejně tak pro metodu POST).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-935">For example, if you sent a GET request, any continuation requests you send must also use GET (and likewise for POST).</span></span>

<span data-ttu-id="ba4ea-936"><a name="SearchResponse"></a>
**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-936"><a name="SearchResponse"></a>
**Response**</span></span>

<span data-ttu-id="ba4ea-937">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-937">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      "@odata.count": # (if $count=true was provided in hello query),
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "@search.facets": { (if faceting was specified in hello query)
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
      "@search.nextPageParameters": { (request body toofetch hello next page of results if not all results could be returned in this response and Search was called with POST)
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
      "@odata.nextLink": (URL toofetch hello next page of results if not all results could be returned in this response; Applies tooboth GET and POST)
    }

<span data-ttu-id="ba4ea-938">**Příklady:**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-938">**Examples:**</span></span>

<span data-ttu-id="ba4ea-939">Další příklady můžete najít na hello [syntaxe výrazu OData pro službu Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-939">You can find additional examples on hello [OData Expression Syntax for Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) page.</span></span>

1)    <span data-ttu-id="ba4ea-940">Hledání hello Index seřazeny sestupně podle data.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-940">Search hello Index sorted descending by date.</span></span>

    <span data-ttu-id="ba4ea-941">GET /indexes/hotels/docs? hledání = * & $orderby = lastRenovationDate desc & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-941">GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="ba4ea-942">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "orderby": "lastRenovationDate desc"}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-942">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "orderby": "lastRenovationDate desc" }</span></span>

2)    <span data-ttu-id="ba4ea-943">V rámci Fasetové vyhledávání v rejstříku hello a načíst omezující vlastnosti pro kategorií, hodnocení, značky, jakož i položky s baseRate v konkrétních oblastech:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-943">In a faceted search, search hello index and retrieve facets for categories, rating, tags, as well as items with baseRate in specific ranges:</span></span>

    <span data-ttu-id="ba4ea-944">GET /indexes/hotels/docs? hledání = test & omezující vlastnost = kategorie & omezující vlastnost = hodnocení & omezující vlastnost = značky & omezující vlastnost = baseRate, hodnoty: 80 | 150 | 220 & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-944">GET /indexes/hotels/docs?search=test&facet=category&facet=rating&facet=tags&facet=baseRate,values:80|150|220&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="ba4ea-945">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "test", "omezující vlastnosti": ["kategorie", "hodnocení", "značky", "baseRate, hodnoty: 80 | 150 | 220"]}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-945">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "test", "facets": [ "category", "rating", "tags", "baseRate,values:80|150|220" ] }</span></span>

3)    <span data-ttu-id="ba4ea-946">Pomocí filtru, hello předchozí Fasetové dotazu výsledky zúžit po hello uživatel klikne na hodnocení 3 a kategorie "Motel":</span><span class="sxs-lookup"><span data-stu-id="ba4ea-946">Using a filter, narrow down hello previous faceted query results after hello user clicks on rating 3 and category "Motel":</span></span>

    <span data-ttu-id="ba4ea-947">GET /indexes/hotels/docs? hledání = test & omezující vlastnost = značky & omezující vlastnost = baseRate, hodnoty: 80 | 150 | 220 & $filter = hodnocení eq 3 a kategorie eq "Motel" & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-947">GET /indexes/hotels/docs?search=test&facet=tags&facet=baseRate,values:80|150|220&$filter=rating eq 3 and category eq 'Motel'&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="ba4ea-948">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "test", "omezující vlastnosti": ["značky", "baseRate, hodnoty: 80 | 150 | 220"], "filtr": "hodnocení eq 3 a kategorie eq"Motel""}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-948">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "test", "facets": [ "tags", "baseRate,values:80|150|220" ], "filter": "rating eq 3 and category eq 'Motel'" }</span></span>

4) <span data-ttu-id="ba4ea-949">V rámci Fasetové vyhledávání nastavte horní limit na jedinečný podmínky vrácených dotazem.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-949">In a faceted search, set an upper limit on unique terms returned in a query.</span></span> <span data-ttu-id="ba4ea-950">Hello výchozí hodnota je 10, ale můžete zvýšit nebo snížit tuto hodnotu pomocí hello `count` parametr na hello `facet` atribut:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-950">hello default is 10, but you can increase or decrease this value using hello `count` parameter on hello `facet` attribute:</span></span>

    <span data-ttu-id="ba4ea-951">GET /indexes/hotels/docs? hledání = test & omezující vlastnost = města, počet: 5 & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-951">GET /indexes/hotels/docs?search=test&facet=city,count:5&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="ba4ea-952">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "test", "omezující vlastnosti": ["města, počet: 5"]}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-952">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview  {    "search": "test",    "facets": [ "city,count:5" ]  }</span></span>

5)    <span data-ttu-id="ba4ea-953">Hledání hello Index v rámci konkrétních polí; Například pro specifický jazyk pole:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-953">Search hello Index within specific fields; For example, a language-specific field:</span></span>

    <span data-ttu-id="ba4ea-954">GET /indexes/hotels/docs? hledání = hôtel & searchFields = description_fr & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-954">GET /indexes/hotels/docs?search=hôtel&searchFields=description_fr&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="ba4ea-955">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "hôtel", "searchFields": "description_fr"}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-955">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "hôtel", "searchFields": "description_fr" }</span></span>

6) <span data-ttu-id="ba4ea-956">Hledání hello Index napříč více polí.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-956">Search hello Index across multiple fields.</span></span> <span data-ttu-id="ba4ea-957">Například můžete ukládat a dotaz prohledatelná pole v několika jazycích, vše v rámci hello stejný index.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-957">For example, you can store and query searchable fields in multiple languages, all within hello same index.</span></span>  <span data-ttu-id="ba4ea-958">Pokud angličtinu a francouzštinu popisy současně existovat ve hello stejné dokumentu, můžete se vrátit any nebo all v hello výsledků dotazu:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-958">If English and French descriptions co-exist in hello same document, you can return any or all in hello query results:</span></span>

    <span data-ttu-id="ba4ea-959">GET /indexes/hotels/docs? hledání = hotelů & searchFields = popis, description_fr & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-959">GET /indexes/hotels/docs?search=hotel&searchFields=description,description_fr&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="ba4ea-960">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "hotelů", "searchFields": "popis, description_fr"}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-960">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview  {    "search": "hotel",    "searchFields": "description, description_fr"  }</span></span>

<span data-ttu-id="ba4ea-961">Všimněte si, že dotaz lze použít pouze jeden index najednou.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-961">Note that you can only query one index at a time.</span></span> <span data-ttu-id="ba4ea-962">Nevytvářejte více indexů pro jednotlivé jazyky, pokud máte v plánu tooquery, jeden v čase.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-962">Do not create multiple indexes for each language unless you plan tooquery one at a time.</span></span>

7)    <span data-ttu-id="ba4ea-963">Stránkování - Get hello 1 stránku položek (velikost stránky je 10):</span><span class="sxs-lookup"><span data-stu-id="ba4ea-963">Paging - Get hello 1st page of items (page size is 10):</span></span>

    <span data-ttu-id="ba4ea-964">GET /indexes/hotels/docs? hledání = * & $skip = 0 & $top = 10 & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-964">GET /indexes/hotels/docs?search=*&$skip=0&$top=10&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="ba4ea-965">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "přeskočení": 0, "top": 10}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-965">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "skip": 0, "top": 10 }</span></span>

8)    <span data-ttu-id="ba4ea-966">Stránkování - Get hello 2. stránka položky (velikost stránky je 10):</span><span class="sxs-lookup"><span data-stu-id="ba4ea-966">Paging - Get hello 2nd page of items (page size is 10):</span></span>

    <span data-ttu-id="ba4ea-967">GET /indexes/hotels/docs? hledání = * & $skip = 10 & $top = 10 & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-967">GET /indexes/hotels/docs?search=*&$skip=10&$top=10&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="ba4ea-968">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "přeskočení": "top" 10: 10}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-968">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "skip": 10, "top": 10 }</span></span>

9)    <span data-ttu-id="ba4ea-969">Načtěte konkrétní sadu pole:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-969">Retrieve a specific set of fields:</span></span>

    <span data-ttu-id="ba4ea-970">GET /indexes/hotels/docs? hledání = * & $select = hotelName, popis a rozhraní api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-970">GET /indexes/hotels/docs?search=*&$select=hotelName,description&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="ba4ea-971">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "Vyberte": "hotelName, popis"}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-971">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "select": "hotelName, description" }</span></span>

10)  <span data-ttu-id="ba4ea-972">Načtení dokumenty odpovídající výraz konkrétní filtru:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-972">Retrieve documents matching a specific filter expression:</span></span>

    <span data-ttu-id="ba4ea-973">GET /indexes/hotels/docs? $filter = (baseRate ge 60 a baseRate lt 300) nebo hotelName eq 'Zvláštní zůstat' & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-973">GET /indexes/hotels/docs?$filter=(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="ba4ea-974">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"filtr": "(baseRate ge 60 a baseRate lt 300) nebo hotelName eq"Zvláštní zůstat""}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-974">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {  "filter": "(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'" }</span></span>

11) <span data-ttu-id="ba4ea-975">Hello index a nevrací fragmenty se přístupů označuje</span><span class="sxs-lookup"><span data-stu-id="ba4ea-975">Search hello index and return fragments with hit highlights</span></span>

    <span data-ttu-id="ba4ea-976">GET /indexes/hotels/docs? hledání = něco & zvýrazněte = Popis & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-976">GET /indexes/hotels/docs?search=something&highlight=description&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="ba4ea-977">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "něco co", "highlight": "Popis"}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-977">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "highlight": "description" }</span></span>

12) <span data-ttu-id="ba4ea-978">Hledání hello indexu a vrátit dokumenty seřadit z blíže toofarther směrem od referenčního místa</span><span class="sxs-lookup"><span data-stu-id="ba4ea-978">Search hello index and return documents sorted from closer toofarther away from a reference location</span></span>

    <span data-ttu-id="ba4ea-979">GET /indexes/hotels/docs? vyhledávání něco = & $orderby=geo.distance (umístění, geography'POINT(-122.12315 47.88121)') & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-979">GET /indexes/hotels/docs?search=something&$orderby=geo.distance(location, geography'POINT(-122.12315 47.88121)')&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="ba4ea-980">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "něco co", "orderby": "geo.distance (umístění, geography'POINT(-122.12315 47.88121)')"}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-980">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "orderby": "geo.distance(location, geography'POINT(-122.12315 47.88121)')" }</span></span>

13) <span data-ttu-id="ba4ea-981">Hledání hello index za předpokladu, že je profil vyhodnocování názvem "geo" s dvě funkce pro výpočet skóre podle vzdálenosti, jeden definování parametr s názvem "currentLocation" a jeden definování parametr s názvem "lastLocation"</span><span class="sxs-lookup"><span data-stu-id="ba4ea-981">Search hello index assuming there's a scoring profile called "geo" with two distance scoring functions, one defining a parameter called "currentLocation" and one defining a parameter called "lastLocation"</span></span>

    <span data-ttu-id="ba4ea-982">GET /indexes/hotels/docs? vyhledávání něco = & scoringProfile = geograficky & scoringParameter = currentLocation – 122.123,44.77233 & scoringParameter = lastLocation – 121.499,44.2113 & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-982">GET /indexes/hotels/docs?search=something&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&scoringParameter=lastLocation--121.499,44.2113&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="ba4ea-983">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "něco co", "scoringProfile": "geo", "scoringParameters": ["currentLocation – 122.123,44.77233", "lastLocation – 121.499,44.2113"]}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-983">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "scoringProfile": "geo",   "scoringParameters": [ "currentLocation--122.123,44.77233", "lastLocation--121.499,44.2113" ] }</span></span>

14) <span data-ttu-id="ba4ea-984">Najít dokumenty v indexu pomocí hello [jednoduchá syntaxe dotazů](https://msdn.microsoft.com/library/dn798920.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-984">Find documents in hello index using [simple query syntax](https://msdn.microsoft.com/library/dn798920.aspx).</span></span> <span data-ttu-id="ba4ea-985">Tento dotaz vrací hotels kde prohledatelná pole obsahovat podmínky hello "pohodlí" a "umístění", ale ne "motel":</span><span class="sxs-lookup"><span data-stu-id="ba4ea-985">This query returns hotels where searchable fields contain hello terms "comfort" and "location" but not "motel":</span></span>

    <span data-ttu-id="ba4ea-986">GET /indexes/hotels/docs? hledání = pohodlí + umístění-motel & searchMode = all & verze api-version = 2015-02-28-Preview</span><span class="sxs-lookup"><span data-stu-id="ba4ea-986">GET /indexes/hotels/docs?search=comfort +location -motel&searchMode=all&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="ba4ea-987">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "pohodlí + umístění-motel", "searchMode": "vše"}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-987">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "comfort +location -motel",   "searchMode": "all" }</span></span>

<span data-ttu-id="ba4ea-988">Všimněte si použití hello `searchMode=all` výše.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-988">Note hello use of `searchMode=all` above.</span></span> <span data-ttu-id="ba4ea-989">Tento parametr včetně přepíše výchozí hello `searchMode=any`, zajistit který `-motel` znamená "A není" místo "Nebo není".</span><span class="sxs-lookup"><span data-stu-id="ba4ea-989">Including this parameter overrides hello default of `searchMode=any`, ensuring that `-motel` means "AND NOT" instead of "OR NOT".</span></span> <span data-ttu-id="ba4ea-990">Bez `searchMode=all`, získáte "Nebo není" který rozbalí než omezuje výsledky hledání, a to může být counter-intuitive toosome uživatelé.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-990">Without `searchMode=all`, you get "OR NOT" which expands rather than restricts search results, and this can be counter-intuitive toosome users.</span></span>

15) <span data-ttu-id="ba4ea-991">Najít dokumenty v indexu pomocí hello [syntaxe dotazů lucene](https://msdn.microsoft.com/library/mt589323.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-991">Find documents in hello index using [lucene query syntax](https://msdn.microsoft.com/library/mt589323.aspx).</span></span> <span data-ttu-id="ba4ea-992">Tento dotaz vrací hotels kde hello pole kategorie obsahuje hello termín "rozpočet" a všechna prohledatelná pole obsahující hello frázi "nedávno renovovanou".</span><span class="sxs-lookup"><span data-stu-id="ba4ea-992">This query returns hotels where hello category field contains hello term "budget" and all searchable fields containing hello phrase "recently renovated".</span></span> <span data-ttu-id="ba4ea-993">Dokumenty obsahující hello frázi "nedávno renovovanou" jsou řazeny výše v důsledku hello termín nárůst hodnotu (3)</span><span class="sxs-lookup"><span data-stu-id="ba4ea-993">Documents containing hello phrase "recently renovated" are ranked higher as a result of hello term boost value (3)</span></span>

    <span data-ttu-id="ba4ea-994">GET /indexes/hotels/docs? hledání = kategorie: nároky a \"nedávno renovovanou\"^ 3 & searchMode = all & verze api-version = 2015-02-28-Preview & Typ úplné =</span><span class="sxs-lookup"><span data-stu-id="ba4ea-994">GET /indexes/hotels/docs?search=category:budget AND \"recently renovated\"^3&searchMode=all&api-version=2015-02-28-Preview&querytype=full</span></span>

    <span data-ttu-id="ba4ea-995">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "kategorie: nároky a \"nedávno renovovanou\"^ 3", "typ": "úplná", "searchMode": "vše"}</span><span class="sxs-lookup"><span data-stu-id="ba4ea-995">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "category:budget AND \"recently renovated\"^3",   "queryType": "full",   "searchMode": "all" }</span></span>

<a name="LookupAPI"></a>

## <a name="lookup-document"></a><span data-ttu-id="ba4ea-996">Vyhledávání dokumentů</span><span class="sxs-lookup"><span data-stu-id="ba4ea-996">Lookup Document</span></span>
<span data-ttu-id="ba4ea-997">Hello **vyhledávání dokumentů** operace dokumentu načte z Azure Search.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-997">hello **Lookup Document** operation retrieves a document from Azure Search.</span></span> <span data-ttu-id="ba4ea-998">To je užitečné, když uživatel klikne na konkrétní výsledek a chcete, aby toolook si konkrétní podrobnosti o tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-998">This is useful when a user clicks on a specific search result, and you want toolook up specific details about that document.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

<span data-ttu-id="ba4ea-999">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-999">**Request**</span></span>

<span data-ttu-id="ba4ea-1000">Protokol HTTPS je vyžadována pro žádosti o služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1000">HTTPS is required for service requests.</span></span> <span data-ttu-id="ba4ea-1001">Hello **vyhledávání dokumentů** požadavek konstruovat následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1001">hello **Lookup Document** request can be constructed as follows.</span></span>

    GET /indexes/[index name]/docs/key?[query parameters]

<span data-ttu-id="ba4ea-1002">Alternativně můžete hello tradiční OData syntaxe vyhledávání klíčů:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1002">Alternatively, you can use hello traditional OData syntax for key lookup:</span></span>

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

<span data-ttu-id="ba4ea-1003">žádost o Hello URI obsahuje [název indexu] a [klíče], které tooretrieve dokumentu z index, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1003">hello request URI includes an [index name] and [key], specifying which document tooretrieve from which index.</span></span> <span data-ttu-id="ba4ea-1004">Najednou můžete získat pouze jeden dokument.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1004">You can only get one document at a time.</span></span> <span data-ttu-id="ba4ea-1005">Použití **vyhledávání** tooget více dokumentů v jedné žádosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1005">Use **Search** tooget multiple documents in a single request.</span></span>

<span data-ttu-id="ba4ea-1006">**Parametry dotazu**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1006">**Query Parameters**</span></span>

<span data-ttu-id="ba4ea-1007">`$select=[string]`(volitelné) – seznam tooretrieve polí oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1007">`$select=[string]` (optional) - a list of comma-separated fields tooretrieve.</span></span> <span data-ttu-id="ba4ea-1008">Pokud tento parametr nezadáte, nebo nastavte příliš`*`, všechna pole, které jsou označeny jako získat ve schématu hello jsou součástí hello projekce.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1008">If unspecified or set too`*`, all fields marked as retrievable in hello schema are included in hello projection.</span></span>

<span data-ttu-id="ba4ea-1009">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1009">`api-version=[string]` (required).</span></span> <span data-ttu-id="ba4ea-1010">verze preview Hello je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1010">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="ba4ea-1011">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1011">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="ba4ea-1012">Poznámka: Pro tuto operaci hello `api-version` je zadána jako parametr dotazu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1012">Note: For this operation, hello `api-version` is specified as a query parameter.</span></span>

<span data-ttu-id="ba4ea-1013">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1013">**Request Headers**</span></span>

<span data-ttu-id="ba4ea-1014">Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1014">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="ba4ea-1015">`api-key`: hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1015">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="ba4ea-1016">Je řetězcovou hodnotu, adresa URL služby jedinečný tooyour.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1016">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="ba4ea-1017">Hello **vyhledávání dokumentů** žádosti můžete zadat buď klíč správce, nebo klíč dotazu pro `api-key`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1017">hello **Lookup Document** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="ba4ea-1018">Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1018">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="ba4ea-1019">Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1019">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="ba4ea-1020">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1020">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="ba4ea-1021">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1021">**Request Body**</span></span>

<span data-ttu-id="ba4ea-1022">Žádné.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1022">None.</span></span>

<span data-ttu-id="ba4ea-1023">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1023">**Response**</span></span>

<span data-ttu-id="ba4ea-1024">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1024">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      field_name: field_value (fields matching hello default or specified projection)
    }

<span data-ttu-id="ba4ea-1025">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1025">**Example**</span></span>

<span data-ttu-id="ba4ea-1026">Vyhledávání hello dokument, který má klíč: 2.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1026">Lookup hello document that has key '2'</span></span>

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

<span data-ttu-id="ba4ea-1027">Dokument hello vyhledávání, který má klíč pomocí syntaxe OData 3:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1027">Lookup hello document that has key '3' using OData syntax:</span></span>

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>

## <a name="count-documents"></a><span data-ttu-id="ba4ea-1028">Počet dokumentů</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1028">Count Documents</span></span>
<span data-ttu-id="ba4ea-1029">Hello **počet dokumentů** operace načte počet hello počet dokumentů v indexu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1029">hello **Count Documents** operation retrieves a count of hello number of documents in a search index.</span></span> <span data-ttu-id="ba4ea-1030">Hello `$count` syntaxe je součástí hello protokolu OData.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1030">hello `$count` syntax is part of hello OData protocol.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

<span data-ttu-id="ba4ea-1031">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1031">**Request**</span></span>

<span data-ttu-id="ba4ea-1032">Protokol HTTPS je vyžadována pro žádosti o služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1032">HTTPS is required for service requests.</span></span> <span data-ttu-id="ba4ea-1033">Hello **počet dokumentů** požadavek se dá vytvořit pomocí metody GET hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1033">hello **Count Documents** request can be constructed using hello GET method.</span></span>

<span data-ttu-id="ba4ea-1034">Hello [název indexu] v identifikátoru URI žádosti hello informuje hello služby tooreturn počet všechny položky v kolekci dokumenty hello hello zadaným indexem.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1034">hello [index name] in hello request URI tells hello service tooreturn a count of all items in hello docs collection of hello specified index.</span></span>

<span data-ttu-id="ba4ea-1035">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1035">`api-version=[string]` (required).</span></span> <span data-ttu-id="ba4ea-1036">verze preview Hello je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1036">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="ba4ea-1037">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1037">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="ba4ea-1038">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1038">**Request Headers**</span></span>

<span data-ttu-id="ba4ea-1039">Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1039">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="ba4ea-1040">`Accept`: Tato hodnota musí být nastavena příliš`text/plain`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1040">`Accept`: This value must be set too`text/plain`.</span></span>
* <span data-ttu-id="ba4ea-1041">`api-key`: hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1041">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="ba4ea-1042">Je řetězcovou hodnotu, adresa URL služby jedinečný tooyour.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1042">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="ba4ea-1043">Hello **počet dokumentů** žádosti můžete zadat buď klíč správce, nebo klíč dotazu pro `api-key`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1043">hello **Count Documents** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="ba4ea-1044">Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1044">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="ba4ea-1045">Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1045">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="ba4ea-1046">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1046">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="ba4ea-1047">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1047">**Request Body**</span></span>

<span data-ttu-id="ba4ea-1048">Žádné.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1048">None.</span></span>

<span data-ttu-id="ba4ea-1049">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1049">**Response**</span></span>

<span data-ttu-id="ba4ea-1050">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1050">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="ba4ea-1051">text odpovědi Hello obsahuje hodnotu počtu hello jako celé číslo ve formátu v prostém textu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1051">hello response body contains hello count value as an integer formatted in plain text.</span></span>

<a name="Suggestions"></a>

## <a name="suggestions"></a><span data-ttu-id="ba4ea-1052">Návrhy</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1052">Suggestions</span></span>
<span data-ttu-id="ba4ea-1053">Hello **návrhy** operace načte návrhy založené na vstupu částečné vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1053">hello **Suggestions** operation retrieves suggestions based on partial search input.</span></span> <span data-ttu-id="ba4ea-1054">Ho se obvykle používá v našeptávání návrhy vyhledávání polí tooprovide jako uživatelé zadávají hledaný text.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1054">It's typically used in search boxes tooprovide type-ahead suggestions as users are entering search terms.</span></span>

<span data-ttu-id="ba4ea-1055">Požadavky návrhu zaměřena na cíl dokumenty návrhy, takže hello navrhované, že text může být opakována v případě více dokumentů candidate odpovídat hello stejné vyhledávání vstup.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1055">Suggestion requests aim at suggesting target documents, so hello suggested text may be repeated if multiple candidate documents match hello same search input.</span></span> <span data-ttu-id="ba4ea-1056">Můžete použít `$select` tooretrieve jiných dokumentů pole (včetně klíč dokumentu hello) tak, aby můžete zjistit, který dokument je hello zdroj pro každý návrhu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1056">You can use `$select` tooretrieve other document fields (including hello document key) so that you can tell which document is hello source for each suggestion.</span></span>

<span data-ttu-id="ba4ea-1057">A **návrhy** operaci se objeví jako požadavek GET nebo POST.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1057">A **Suggestions** operation is issued as a GET or POST request.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

<span data-ttu-id="ba4ea-1058">**Když toouse POST místo GET**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1058">**When toouse POST instead of GET**</span></span>

<span data-ttu-id="ba4ea-1059">Při použití metody GET protokolu HTTP hello toocall **návrhy** rozhraní API, musíte vědět, že nesmí překročit délka hello adresy URL žádosti hello 8 KB toobe.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1059">When you use HTTP GET toocall hello **Suggestions** API, you need toobe aware that hello length of hello request URL cannot exceed 8 KB.</span></span> <span data-ttu-id="ba4ea-1060">Je to obvykle dostatečně pro většinu aplikací.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1060">This is usually enough for most applications.</span></span> <span data-ttu-id="ba4ea-1061">Některé aplikace však vytvořit velmi velké dotazy, konkrétně výrazy filtru OData.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1061">However, some applications produce very large queries, specifically OData filter expressions.</span></span> <span data-ttu-id="ba4ea-1062">Pro tyto aplikace pomocí HTTP POST je lepší volbou, protože umožňuje větší filtry než metoda GET.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1062">For these applications, using HTTP POST is a better choice because it allows larger filters than GET.</span></span> <span data-ttu-id="ba4ea-1063">S POST hello počet klauzulí ve filtru je hello omezující faktor, není hello velikost řetězec nezpracovaná filtru hello vzhledem k tomu, že omezení velikosti hello požadavku pro metodu POST je přibližně 16 MB.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1063">With POST, hello number of clauses in a filter is hello limiting factor, not hello size of hello raw filter string since hello request size limit for POST is approximately 16 MB.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-1064">I když limit velikosti požadavek POST hello je moc velká, nemůže být libovolně komplexní výrazech filtru.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1064">Even though hello POST request size limit is very large, filter expressions cannot be arbitrarily complex.</span></span> <span data-ttu-id="ba4ea-1065">V tématu [syntaxe výrazu OData](https://msdn.microsoft.com/library/dn798921.aspx) pro další informace o omezeních složitost filtru.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1065">See [OData expression syntax](https://msdn.microsoft.com/library/dn798921.aspx) for more information about filter complexity limitations.</span></span>
> 
> 

<span data-ttu-id="ba4ea-1066">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1066">**Request**</span></span>

<span data-ttu-id="ba4ea-1067">Protokol HTTPS je vyžadována pro žádosti o služby.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1067">HTTPS is required for service requests.</span></span> <span data-ttu-id="ba4ea-1068">Hello **návrhy** požadavek se dá vytvořit pomocí hello GET nebo POST metody.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1068">hello **Suggestions** request can be constructed using hello GET or POST methods.</span></span>

<span data-ttu-id="ba4ea-1069">identifikátor URI požadavku Hello Určuje název hello hello index tooquery.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1069">hello request URI specifies hello name of hello index tooquery.</span></span> <span data-ttu-id="ba4ea-1070">Parametry, jako je například hello částečně vstupní hledaný termín, jsou zadány na hello řetězec dotazu v případě hello požadavků GET a v žádosti o hello textu v případě hello POST požadavky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1070">Parameters, such as hello partially input search term, are specified on hello query string in hello case of GET requests, and in hello request body in hello case of POST requests.</span></span>

<span data-ttu-id="ba4ea-1071">Jako osvědčený postup při vytváření požadavky GET, mějte na paměti, příliš[kódování URL](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specifického dotazu parametry při volání metody hello REST API přímo.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1071">As a best practice when creating GET requests, remember too[URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specific query parameters when calling hello REST API directly.</span></span> <span data-ttu-id="ba4ea-1072">Pro **návrhy** operace, to zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1072">For **Suggestions** operations, this includes:</span></span>

* `$filter`
* `highlightPreTag`
* `highlightPostTag`
* `search`

<span data-ttu-id="ba4ea-1073">Kódování URL se doporučuje jenom na hello výše parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1073">URL encoding is only recommended on hello above query parameters.</span></span> <span data-ttu-id="ba4ea-1074">Pokud jste omylem kódování URL hello řetězec dotazu celý, (vše za hello?), dojde k přerušení požadavky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1074">If you inadvertently URL-encode hello entire query string (everything after hello ?), requests will break.</span></span>

<span data-ttu-id="ba4ea-1075">Navíc kódování URL je nutné pouze při volání metody hello získat přímo pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1075">Also, URL encoding is only necessary when calling hello REST API directly using GET.</span></span> <span data-ttu-id="ba4ea-1076">Žádné kódování URL je nezbytné při volání metody **návrhy** pomocí POST, nebo při použití hello [klientské knihovny .NET](https://msdn.microsoft.com/library/dn951165.aspx), která zpracovává kódování URL za vás.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1076">No URL encoding is necessary when calling **Suggestions** using POST, or when using hello [.NET client library](https://msdn.microsoft.com/library/dn951165.aspx), which handles URL encoding for you.</span></span>

<span data-ttu-id="ba4ea-1077">**Parametry dotazu**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1077">**Query Parameters**</span></span>

<span data-ttu-id="ba4ea-1078">**Návrhy** přijímá několik parametrů, které poskytují kritéria dotazu a také určit chování vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1078">**Suggestions** accepts several parameters that provide query criteria and also specify search behavior.</span></span> <span data-ttu-id="ba4ea-1079">Zadejte tyto parametry v adrese URL hello řetězec dotazu při volání metody **návrhy** prostřednictvím GET a jako vlastnosti JSON v textu žádosti hello při volání metody **návrhy** přes POST.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1079">You provide these parameters in hello URL query string when calling **Suggestions** via GET, and as JSON properties in hello request body when calling **Suggestions** via POST.</span></span> <span data-ttu-id="ba4ea-1080">Hello syntaxe pro některé parametry se mírně liší mezi GET a POST.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1080">hello syntax for some parameters is slightly different between GET and POST.</span></span> <span data-ttu-id="ba4ea-1081">Tyto rozdíly jsou popsány podle vhodnosti níže:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1081">These differences are noted as applicable below:</span></span>

<span data-ttu-id="ba4ea-1082">`search=[string]`-hello vyhledávací text toouse toosuggest dotazy.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1082">`search=[string]` - hello search text toouse toosuggest queries.</span></span> <span data-ttu-id="ba4ea-1083">Musí být minimálně 1 znak a maximálně 100 znaků.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1083">Must be at least 1 character, and no more than 100 characters.</span></span>

<span data-ttu-id="ba4ea-1084">`highlightPreTag=[string]`(volitelné) – značku řetězec, který přidá toosearch přístupů.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1084">`highlightPreTag=[string]` (optional) - a string tag that prepends toosearch hits.</span></span> <span data-ttu-id="ba4ea-1085">Musí být nastavena s `highlightPostTag`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1085">Must be set with `highlightPostTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-1086">Při volání metody **návrhy** pomocí GET, vyhrazené znaky v adrese URL hello musí být kódovaný v procentech (například místo # % 23).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1086">When calling **Suggestions** using GET, reserved characters in hello URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="ba4ea-1087">`highlightPostTag=[string]`(volitelné) – značku řetězec, který připojí toosearch přístupů.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1087">`highlightPostTag=[string]` (optional) - a string tag that appends toosearch hits.</span></span> <span data-ttu-id="ba4ea-1088">Musí být nastavena s `highlightPreTag`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1088">Must be set with `highlightPreTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-1089">Při volání metody **návrhy** pomocí GET, vyhrazené znaky v adrese URL hello musí být kódovaný v procentech (například místo # % 23).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1089">When calling **Suggestions** using GET, reserved characters in hello URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="ba4ea-1090">`suggesterName=[string]`-hello název modulu pro návrhy hello jako zadaný v hello `suggesters` kolekce, která je součástí definice indexu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1090">`suggesterName=[string]` - hello name of hello suggester as specified in hello `suggesters` collection that's part of hello index definition.</span></span> <span data-ttu-id="ba4ea-1091">A `suggester` určuje pole, která hledat podmínky navrhované dotazu.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1091">A `suggester` determines which fields are scanned for suggested query terms.</span></span> <span data-ttu-id="ba4ea-1092">V tématu [trochu](#Suggesters) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1092">See [Suggesters](#Suggesters) for details.</span></span>

<span data-ttu-id="ba4ea-1093">`fuzzy=[boolean]`(volitelné, výchozí = false) – Pokud nastavíte tootrue toto rozhraní API najdete návrhy i tehdy, je nahrazovanou nebo chybí znak v hello hledaný text.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1093">`fuzzy=[boolean]` (optional, default = false) - when set tootrue this API will find suggestions even if there's a substituted or missing character in hello search text.</span></span> <span data-ttu-id="ba4ea-1094">Zatímco to poskytuje lepší podmínky v některých scénářích kompromisnímu výkon, protože vyhledávání s fuzzy logikou návrhu pomalejší a spotřebovávat více prostředků.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1094">While this provides a better experience in some scenarios it comes at a performance cost as fuzzy suggestion searches are slower and consume more resources.</span></span>

<span data-ttu-id="ba4ea-1095">`searchFields=[string]`(volitelné) – seznam hello toosearch názvy polí oddělených čárkou pro hello zadaný hledaný text.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1095">`searchFields=[string]` (optional) - hello list of comma-separated field names toosearch for hello specified search text.</span></span> <span data-ttu-id="ba4ea-1096">Cílová pole. musí být povolen pro návrhy.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1096">Target fields must be enabled for suggestions.</span></span>

<span data-ttu-id="ba4ea-1097">`$top=#`(volitelné, výchozí = 5)-hello počet tooretrieve návrhy.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1097">`$top=#` (optional, default = 5) - hello number of suggestions tooretrieve.</span></span> <span data-ttu-id="ba4ea-1098">Musí být číslo v rozsahu od 1 do 100.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1098">Must be a number between 1 and 100.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-1099">Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `top` místo `$top`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1099">When calling **Suggestions** using POST, this parameter is named `top` instead of `$top`.</span></span>
> 
> 

<span data-ttu-id="ba4ea-1100">`$filter=[string]`(volitelné) – výraz, který filtruje hello dokumenty za návrhy.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1100">`$filter=[string]` (optional) - an expression that filters hello documents considered for suggestions.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-1101">Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `filter` místo `$filter`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1101">When calling **Suggestions** using POST, this parameter is named `filter` instead of `$filter`.</span></span>
> 
> 

<span data-ttu-id="ba4ea-1102">`$orderby=[string]`(volitelné) – seznam oddělený čárkami výrazy toosort hello výsledků podle.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1102">`$orderby=[string]` (optional) - a list of comma-separated expressions toosort hello results by.</span></span> <span data-ttu-id="ba4ea-1103">Každý výraz může být název pole nebo volání toohello `geo.distance()` funkce.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1103">Each expression can be either a field name or a call toohello `geo.distance()` function.</span></span> <span data-ttu-id="ba4ea-1104">Každý výraz může následovat `asc` tooindicated vzestupné řazení a `desc` tooindicate sestupně.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1104">Each expression can be followed by `asc` tooindicated ascending, and `desc` tooindicate descending.</span></span> <span data-ttu-id="ba4ea-1105">Výchozí Hello je vzestupné pořadí.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1105">hello default is ascending order.</span></span> <span data-ttu-id="ba4ea-1106">Existuje limit 32 klauzule pro `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1106">There is a limit of 32 clauses for `$orderby`.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-1107">Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `orderby` místo `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1107">When calling **Suggestions** using POST, this parameter is named `orderby` instead of `$orderby`.</span></span>
> 
> 

<span data-ttu-id="ba4ea-1108">`$select=[string]`(volitelné) – seznam tooretrieve polí oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1108">`$select=[string]` (optional) - a list of comma-separated fields tooretrieve.</span></span> <span data-ttu-id="ba4ea-1109">Pokud tento parametr nezadáte, hello pouze klíč dokumentu a návrhu je vrácen.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1109">If unspecified, only hello document key and suggestion text is returned.</span></span> <span data-ttu-id="ba4ea-1110">Všechna pole můžete explicitně žádostí nastavením tento parametr příliš`*`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1110">You can explicitly request all fields by setting this parameter too`*`.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-1111">Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `select` místo `$select`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1111">When calling **Suggestions** using POST, this parameter is named `select` instead of `$select`.</span></span>
> 
> 

<span data-ttu-id="ba4ea-1112">`minimumCoverage`(volitelné, použije se výchozí hodnota too80) - číslo v rozsahu od 0 do 100 určující procento hello hello index, který musí být předmětem dotazu návrhy aby hello dotazu toobe hlášené jako úspěšné.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1112">`minimumCoverage` (optional, defaults too80) - a number between 0 and 100 indicating hello percentage of hello index that must be covered by a suggestions query in order for hello query toobe reported as a success.</span></span> <span data-ttu-id="ba4ea-1113">Ve výchozím nastavení, musí být k dispozici alespoň 80 % hello index nebo `Suggest` vrátí stavový kód protokolu HTTP 503.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1113">By default, at least 80% of hello index must be available or `Suggest` will return HTTP status code 503.</span></span> <span data-ttu-id="ba4ea-1114">Pokud nastavíte `minimumCoverage` a `Suggest` úspěšné, se vrátí HTTP 200 a zahrnout `@search.coverage` hodnotu v odpovědi hello označující hello procento hello index, který je zahrnutý v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1114">If you set `minimumCoverage` and `Suggest` succeeds, it will return HTTP 200 and include a `@search.coverage` value in hello response indicating hello percentage of hello index that was included in hello query.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4ea-1115">Nastavení této hodnoty parametru tooa menší než 100 může být užitečná pro zajištění dostupnosti vyhledávání i pro služby s pouze jednu repliku.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1115">Setting this parameter tooa value lower than 100 can be useful for ensuring search availability even for services with only one replica.</span></span> <span data-ttu-id="ba4ea-1116">Ne všechny odpovídající návrh je však zaručena toobe součástí hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1116">However, not all matching suggestions are guaranteed toobe present in hello results.</span></span> <span data-ttu-id="ba4ea-1117">Pokud odvolání je důležitější tooyour aplikace než dostupnost, je jeho osvědčené není toolower `minimumCoverage` pod jeho výchozí hodnota 80.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1117">If recall is more important tooyour application than availability, then it's best not toolower `minimumCoverage` below its default value of 80.</span></span>
> 
> 

<span data-ttu-id="ba4ea-1118">`api-version=[string]`(povinné).</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1118">`api-version=[string]` (required).</span></span> <span data-ttu-id="ba4ea-1119">verze preview Hello je `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1119">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="ba4ea-1120">V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1120">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="ba4ea-1121">Poznámka: Pro tuto operaci hello `api-version` je zadána jako parametr dotazu v URL hello bez ohledu na to, jestli volání **návrhy** s GET nebo POST.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1121">Note: For this operation, hello `api-version` is specified as a query parameter in hello URL regardless of whether you call **Suggestions** with GET or POST.</span></span>

<span data-ttu-id="ba4ea-1122">**Hlavičky požadavku**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1122">**Request Headers**</span></span>

<span data-ttu-id="ba4ea-1123">Hello následující seznam popisuje hello požadované a volitelné hlaviček odpovědi</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1123">hello following list describes hello required and optional request headers</span></span>

* <span data-ttu-id="ba4ea-1124">`api-key`: hello `api-key` je použité tooauthenticate hello požadavek tooyour službu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1124">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="ba4ea-1125">Je řetězcovou hodnotu, adresa URL služby jedinečný tooyour.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1125">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="ba4ea-1126">Hello **návrhy** žádosti můžete zadat buď klíč správce, nebo klíč dotazu jako hello `api-key`.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1126">hello **Suggestions** request can specify either an admin key or query key as hello `api-key`.</span></span>

<span data-ttu-id="ba4ea-1127">Budete také potřebovat hello služby název tooconstruct hello adrese URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1127">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="ba4ea-1128">Můžete získat název služby hello a `api-key` z řídicího panelu služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1128">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="ba4ea-1129">V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) nápovědu navigace stránky.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1129">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="ba4ea-1130">**Text žádosti**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1130">**Request Body**</span></span>

<span data-ttu-id="ba4ea-1131">U metody GET: žádné.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1131">For GET: None.</span></span>

<span data-ttu-id="ba4ea-1132">Pro metodu POST:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1132">For POST:</span></span>

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered toodeclare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

<span data-ttu-id="ba4ea-1133">**Odpověď**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1133">**Response**</span></span>

<span data-ttu-id="ba4ea-1134">Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1134">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

<span data-ttu-id="ba4ea-1135">Pokud je možnost projekce hello použité tooretrieve pole, která jsou součástí každý element pole hello:</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1135">If hello projection option is used tooretrieve fields they are included in each element of hello array:</span></span>

    {
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

<span data-ttu-id="ba4ea-1136">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1136">**Example**</span></span>

<span data-ttu-id="ba4ea-1137">Načtení 5 návrhy, kde je hello částečné vyhledávání vstup 'lux.</span><span class="sxs-lookup"><span data-stu-id="ba4ea-1137">Retrieve 5 suggestions where hello partial search input is 'lux'</span></span>

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
