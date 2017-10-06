---
title: "AAA \"nahrání dat (rozhraní REST - API Azure Search) | Microsoft Docs\""
description: "Zjistěte, jak hello tooupload data tooan index Azure Search pomocí REST API."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: 8d0749fb-6e08-4a17-8cd3-1a215138abc6
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
ms.openlocfilehash: 6ba1336012d1f0f6d6d6c933e16aa879afb9b824
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-tooazure-search-using-hello-rest-api"></a><span data-ttu-id="3c409-103">Nahrávání dat tooAzure vyhledávání pomocí hello REST API</span><span class="sxs-lookup"><span data-stu-id="3c409-103">Upload data tooAzure Search using hello REST API</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="3c409-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3c409-104">Overview</span></span>](search-what-is-data-import.md)
> * [<span data-ttu-id="3c409-105">.NET</span><span class="sxs-lookup"><span data-stu-id="3c409-105">.NET</span></span>](search-import-data-dotnet.md)
> * [<span data-ttu-id="3c409-106">REST</span><span class="sxs-lookup"><span data-stu-id="3c409-106">REST</span></span>](search-import-data-rest-api.md)
>
>

<span data-ttu-id="3c409-107">Tento článek vám ukáže, jak toouse hello [REST API služby Azure Search](https://docs.microsoft.com/rest/api/searchservice/) tooimport data do indexu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="3c409-107">This article will show you how toouse hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/) tooimport data into an Azure Search index.</span></span>

<span data-ttu-id="3c409-108">Před zahájením tohoto názorného průvodce byste již měli mít [vytvořený index Azure Search](search-what-is-an-index.md).</span><span class="sxs-lookup"><span data-stu-id="3c409-108">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md).</span></span>

<span data-ttu-id="3c409-109">V pořadí toopush dokumentů do indexu pomocí hello REST API vydáte HTTP POST požadavek tooyour indexu na URL koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="3c409-109">In order toopush documents into your index using hello REST API, you will issue an HTTP POST request tooyour index's URL endpoint.</span></span> <span data-ttu-id="3c409-110">text Hello hello požadavek HTTP, že text je objekt JSON obsahující dokumenty hello toobe přidat, upravit nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="3c409-110">hello body of hello HTTP request body is a JSON object containing hello documents toobe added, modified, or deleted.</span></span>

## <a name="identify-your-azure-search-services-admin-api-key"></a><span data-ttu-id="3c409-111">Identifikace klíče rozhraní API správce služby Azure Search</span><span class="sxs-lookup"><span data-stu-id="3c409-111">Identify your Azure Search service's admin api-key</span></span>
<span data-ttu-id="3c409-112">Při odesílání požadavků HTTP na vaši službu pomocí REST API hello *každý* požadavku API musí zahrnovat hello klíč api-key generovaný pro hello službu vyhledávání jste zřídili.</span><span class="sxs-lookup"><span data-stu-id="3c409-112">When issuing HTTP requests against your service using hello REST API, *each* API request must include hello api-key that was generated for hello Search service you provisioned.</span></span> <span data-ttu-id="3c409-113">Platný klíč vytváří na základě žádosti mezi hello aplikace odesílání hello požadavku a hello služby, která ji zpracovává vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="3c409-113">Having a valid key establishes trust, on a per request basis, between hello application sending hello request and hello service that handles it.</span></span>

1. <span data-ttu-id="3c409-114">toofind klíče služby api Key, se můžete přihlásit toohello [portálu Azure](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="3c409-114">toofind your service's api-keys, you can sign in toohello [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="3c409-115">Okno služby Azure Search přejděte tooyour</span><span class="sxs-lookup"><span data-stu-id="3c409-115">Go tooyour Azure Search service's blade</span></span>
3. <span data-ttu-id="3c409-116">Klikněte na hello ikonu klíčů."</span><span class="sxs-lookup"><span data-stu-id="3c409-116">Click on hello "Keys" icon</span></span>

<span data-ttu-id="3c409-117">Vaše služba bude mít *klíče správce* a *klíče dotazů*.</span><span class="sxs-lookup"><span data-stu-id="3c409-117">Your service will have *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="3c409-118">Primární a sekundární *klíče správce* udělit úplná práva tooall operace, včetně hello možnost toomanage hello služby, vytvářet a odstraňovat indexy, indexery a zdroje dat..</span><span class="sxs-lookup"><span data-stu-id="3c409-118">Your primary and secondary *admin keys* grant full rights tooall operations, including hello ability toomanage hello service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="3c409-119">Existují dva klíče, aby mohl pokračovat toouse hello sekundární klíč, pokud se rozhodnete tooregenerate hello primární klíč a naopak.</span><span class="sxs-lookup"><span data-stu-id="3c409-119">There are two keys so that you can continue toouse hello secondary key if you decide tooregenerate hello primary key, and vice-versa.</span></span>
* <span data-ttu-id="3c409-120">Vaše *klíče dotazů* udělit oprávnění jen pro čtení tooindexes a dokumentům a obvykle distribuované tooclient aplikace, které vydávají požadavky hledání.</span><span class="sxs-lookup"><span data-stu-id="3c409-120">Your *query keys* grant read-only access tooindexes and documents, and are typically distributed tooclient applications that issue search requests.</span></span>

<span data-ttu-id="3c409-121">Pro účely hello importování dat do indexu, můžete použít buď vaší primární nebo sekundární klíč správce.</span><span class="sxs-lookup"><span data-stu-id="3c409-121">For hello purposes of importing data into an index, you can use either your primary or secondary admin key.</span></span>

## <a name="decide-which-indexing-action-toouse"></a><span data-ttu-id="3c409-122">Rozhodněte, které indexování toouse akce</span><span class="sxs-lookup"><span data-stu-id="3c409-122">Decide which indexing action toouse</span></span>
<span data-ttu-id="3c409-123">Pokud používáte hello REST API, vydáte požadavky HTTP POST s JSON požadavek těla tooyour indexu Azure Search na adresu URL koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="3c409-123">When using hello REST API, you will issue HTTP POST requests with JSON request bodies tooyour Azure Search index's endpoint URL.</span></span> <span data-ttu-id="3c409-124">Hello objekt JSON v požadavku HTTP bude obsahovat jedno pole JSON s názvem "value" s objekty JSON reprezentujícími dokumenty, které byste chtěli tooadd tooyour indexu, aktualizovat nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="3c409-124">hello JSON object in your HTTP request body will contain a single JSON array named "value" containing JSON objects representing documents you would like tooadd tooyour index, update, or delete.</span></span>

<span data-ttu-id="3c409-125">Každý objekt JSON v poli "value" hello představuje dokumentu toobe, indexovat.</span><span class="sxs-lookup"><span data-stu-id="3c409-125">Each JSON object in hello "value" array represents a document toobe indexed.</span></span> <span data-ttu-id="3c409-126">Každý z těchto objektů obsahuje klíč dokumentu hello a určuje akci hello potřeby indexování (odeslání, sloučení, odstranění atd.).</span><span class="sxs-lookup"><span data-stu-id="3c409-126">Each of these objects contains hello document's key and specifies hello desired indexing action (upload, merge, delete, etc).</span></span> <span data-ttu-id="3c409-127">Podle toho, která hello níže akce, který zvolíte musí být objekt pro každý dokument obsahovat pouze určitá pole:</span><span class="sxs-lookup"><span data-stu-id="3c409-127">Depending on which of hello below actions you choose, only certain fields must be included for each document:</span></span>

| @search.action | <span data-ttu-id="3c409-128">Popis</span><span class="sxs-lookup"><span data-stu-id="3c409-128">Description</span></span> | <span data-ttu-id="3c409-129">Potřebná pole pro každý dokument</span><span class="sxs-lookup"><span data-stu-id="3c409-129">Necessary fields for each document</span></span> | <span data-ttu-id="3c409-130">Poznámky</span><span class="sxs-lookup"><span data-stu-id="3c409-130">Notes</span></span> |
| --- | --- | --- | --- |
| `upload` |<span data-ttu-id="3c409-131">`upload` Akci je podobný tooan "upsert", kde hello dokument vložený, pokud je nový a aktualizovaný nebo nahrazený, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="3c409-131">An `upload` action is similar tooan "upsert" where hello document will be inserted if it is new and updated/replaced if it exists.</span></span> |<span data-ttu-id="3c409-132">klíč a další pole chcete toodefine</span><span class="sxs-lookup"><span data-stu-id="3c409-132">key, plus any other fields you wish toodefine</span></span> |<span data-ttu-id="3c409-133">Pokud aktualizujete nebo nahrazujete stávající dokument, bude každé pole, které není určený v požadavku hello mít nastavené příliš`null`.</span><span class="sxs-lookup"><span data-stu-id="3c409-133">When updating/replacing an existing document, any field that is not specified in hello request will have its field set too`null`.</span></span> <span data-ttu-id="3c409-134">K tomu dojde i v případě, že bylo hello pole dříve nastavené tooa jinou hodnotu než null.</span><span class="sxs-lookup"><span data-stu-id="3c409-134">This occurs even when hello field was previously set tooa non-null value.</span></span> |
| `merge` |<span data-ttu-id="3c409-135">Aktualizace stávající dokumentů s hello zadaná pole.</span><span class="sxs-lookup"><span data-stu-id="3c409-135">Updates an existing document with hello specified fields.</span></span> <span data-ttu-id="3c409-136">Pokud hello dokument v indexu hello neexistuje, sloučení hello se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="3c409-136">If hello document does not exist in hello index, hello merge will fail.</span></span> |<span data-ttu-id="3c409-137">klíč a další pole chcete toodefine</span><span class="sxs-lookup"><span data-stu-id="3c409-137">key, plus any other fields you wish toodefine</span></span> |<span data-ttu-id="3c409-138">Každé pole zadané ve sloučení nahradí stávající pole hello v dokumentu hello.</span><span class="sxs-lookup"><span data-stu-id="3c409-138">Any field you specify in a merge will replace hello existing field in hello document.</span></span> <span data-ttu-id="3c409-139">To zahrnuje i pole typu `Collection(Edm.String)`.</span><span class="sxs-lookup"><span data-stu-id="3c409-139">This includes fields of type `Collection(Edm.String)`.</span></span> <span data-ttu-id="3c409-140">Například pokud hello dokument obsahuje pole `tags` s hodnotou `["budget"]` a vy spustíte sloučení s hodnotou `["economy", "pool"]` pro `tags`, hello konečná hodnota hello `tags` pole bude `["economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="3c409-140">For example, if hello document contains a field `tags` with value `["budget"]` and you execute a merge with value `["economy", "pool"]` for `tags`, hello final value of hello `tags` field will be `["economy", "pool"]`.</span></span> <span data-ttu-id="3c409-141">Hodnota nebude `["budget", "economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="3c409-141">It will not be `["budget", "economy", "pool"]`.</span></span> |
| `mergeOrUpload` |<span data-ttu-id="3c409-142">Tato akce se chová jako `merge` Pokud dokument s hello zadaný klíč již existuje v indexu hello.</span><span class="sxs-lookup"><span data-stu-id="3c409-142">This action behaves like `merge` if a document with hello given key already exists in hello index.</span></span> <span data-ttu-id="3c409-143">Pokud hello dokument neexistuje, chová se jako `upload` s novým dokumentem.</span><span class="sxs-lookup"><span data-stu-id="3c409-143">If hello document does not exist, it behaves like `upload` with a new document.</span></span> |<span data-ttu-id="3c409-144">klíč a další pole chcete toodefine</span><span class="sxs-lookup"><span data-stu-id="3c409-144">key, plus any other fields you wish toodefine</span></span> |- |
| `delete` |<span data-ttu-id="3c409-145">Odebere zadaný dokument hello hello index.</span><span class="sxs-lookup"><span data-stu-id="3c409-145">Removes hello specified document from hello index.</span></span> |<span data-ttu-id="3c409-146">pouze klíč</span><span class="sxs-lookup"><span data-stu-id="3c409-146">key only</span></span> |<span data-ttu-id="3c409-147">Všechna zadaná pole kromě pole klíče hello budou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="3c409-147">Any fields you specify other than hello key field will be ignored.</span></span> <span data-ttu-id="3c409-148">Pokud chcete tooremove z dokumentu jednotlivá pole, použijte `merge` místo a jednoduše nastavte pole hello explicitně toonull.</span><span class="sxs-lookup"><span data-stu-id="3c409-148">If you want tooremove an individual field from a document, use `merge` instead and simply set hello field explicitly toonull.</span></span> |

## <a name="construct-your-http-request-and-request-body"></a><span data-ttu-id="3c409-149">Konstrukce požadavku HTTP a textu žádosti</span><span class="sxs-lookup"><span data-stu-id="3c409-149">Construct your HTTP request and request body</span></span>
<span data-ttu-id="3c409-150">Teď, když jste shromáždili hello potřebné hodnoty polí pro akce indexu, jsou připravené tooconstruct hello vlastní požadavek HTTP a JSON vaše data žádosti tooimport textu.</span><span class="sxs-lookup"><span data-stu-id="3c409-150">Now that you have gathered hello necessary field values for your index actions, you are ready tooconstruct hello actual HTTP request and JSON request body tooimport your data.</span></span>

#### <a name="request-and-request-headers"></a><span data-ttu-id="3c409-151">Požadavek a hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="3c409-151">Request and Request Headers</span></span>
<span data-ttu-id="3c409-152">V adrese URL hello, budete potřebovat tooprovide váš název služby, název indexu ("hotels" v tomto případě), a také hello správnou verzi rozhraní API (aktuální verze rozhraní API hello je `2016-09-01` v době publikování tohoto dokumentu hello).</span><span class="sxs-lookup"><span data-stu-id="3c409-152">In hello URL, you will need tooprovide your service name, index name ("hotels" in this case), as well as hello proper API version (hello current API version is `2016-09-01` at hello time of publishing this document).</span></span> <span data-ttu-id="3c409-153">Budete potřebovat toodefine hello `Content-Type` a `api-key` hlavičky požadavku.</span><span class="sxs-lookup"><span data-stu-id="3c409-153">You will need toodefine hello `Content-Type` and `api-key` request headers.</span></span> <span data-ttu-id="3c409-154">Pro pozdější hello použijte jeden z klíčů správce vaší služby.</span><span class="sxs-lookup"><span data-stu-id="3c409-154">For hello latter, use one of your service's admin keys.</span></span>

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a><span data-ttu-id="3c409-155">Text žádosti</span><span class="sxs-lookup"><span data-stu-id="3c409-155">Request Body</span></span>
```JSON
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
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close tootown hall and hello river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

<span data-ttu-id="3c409-156">V tomto případě používáme jako akce hledání `upload`, `mergeOrUpload`, a `delete`.</span><span class="sxs-lookup"><span data-stu-id="3c409-156">In this case, we are using `upload`, `mergeOrUpload`, and `delete` as our search actions.</span></span>

<span data-ttu-id="3c409-157">Předpokládejme, že je ukázkový index „hotels“ již naplněný řadou dokumentů.</span><span class="sxs-lookup"><span data-stu-id="3c409-157">Assume that this example "hotels" index is already populated with a number of documents.</span></span> <span data-ttu-id="3c409-158">Všimněte si, jak nebylo nutné toospecify všechny hello možná pole dokumentu při použití `mergeOrUpload` a jak jsme zadali pouze klíč dokumentu hello (`hotelId`) při použití `delete`.</span><span class="sxs-lookup"><span data-stu-id="3c409-158">Note how we did not have toospecify all hello possible document fields when using `mergeOrUpload` and how we only specified hello document key (`hotelId`) when using `delete`.</span></span>

<span data-ttu-id="3c409-159">Také Upozorňujeme, že jste může obsahovat jenom too1000 dokumentů (nebo 16 MB) v jedné žádosti indexování.</span><span class="sxs-lookup"><span data-stu-id="3c409-159">Also, note that you can only include up too1000 documents (or 16 MB) in a single indexing request.</span></span>

## <a name="understand-your-http-response-code"></a><span data-ttu-id="3c409-160">Pochopení kódu odpovědi HTTP</span><span class="sxs-lookup"><span data-stu-id="3c409-160">Understand your HTTP response code</span></span>
#### <a name="200"></a><span data-ttu-id="3c409-161">200</span><span class="sxs-lookup"><span data-stu-id="3c409-161">200</span></span>
<span data-ttu-id="3c409-162">Po odeslání úspěšné žádosti indexování obdržíte odpověď protokolu HTTP se stavovým kódem `200 OK`.</span><span class="sxs-lookup"><span data-stu-id="3c409-162">After submitting a successful indexing request you will receive an HTTP response with status code of `200 OK`.</span></span> <span data-ttu-id="3c409-163">Hello text JSON hello odpověď HTTP bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="3c409-163">hello JSON body of hello HTTP response will be as follows:</span></span>

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a><span data-ttu-id="3c409-164">207</span><span class="sxs-lookup"><span data-stu-id="3c409-164">207</span></span>
<span data-ttu-id="3c409-165">Stavový kód `207` se vrátí, pokud nebyla alespoň jedna položka úspěšně indexovaná.</span><span class="sxs-lookup"><span data-stu-id="3c409-165">A status code of `207` will be returned when at least one item was not successfully indexed.</span></span> <span data-ttu-id="3c409-166">Hello text JSON hello odpověď HTTP bude obsahovat informace o neúspěšných dokumentech hello.</span><span class="sxs-lookup"><span data-stu-id="3c409-166">hello JSON body of hello HTTP response will contain information about hello unsuccessful document(s).</span></span>

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "hello search service is too busy tooprocess this document. Please try again later."
        },
        ...
    ]
}
```

> [!NOTE]
> <span data-ttu-id="3c409-167">Často to znamená, že hello zatížení na hledání služby dosahuje bodu, kdy začnou požadavky indexování tooreturn `503` odpovědi.</span><span class="sxs-lookup"><span data-stu-id="3c409-167">This often means that hello load on your search service is reaching a point where indexing requests will begin tooreturn `503` responses.</span></span> <span data-ttu-id="3c409-168">V tom případě důrazně doporučujeme, aby se váš klientský kód stáhnul a chvíli počkal před tím, než to zkusí znovu.</span><span class="sxs-lookup"><span data-stu-id="3c409-168">In this case, we highly recommend that your client code back off and wait before retrying.</span></span> <span data-ttu-id="3c409-169">To vám poskytne hello systému některé toorecover čas, zvýšit hello šanci na úspěšné provedení dalších požadavků.</span><span class="sxs-lookup"><span data-stu-id="3c409-169">This will give hello system some time toorecover, increasing hello chances that future requests will succeed.</span></span> <span data-ttu-id="3c409-170">Rychlé opakování požadavků pouze prodlouží situaci hello.</span><span class="sxs-lookup"><span data-stu-id="3c409-170">Rapidly retrying your requests will only prolong hello situation.</span></span>
>
>

#### <a name="429"></a><span data-ttu-id="3c409-171">429</span><span class="sxs-lookup"><span data-stu-id="3c409-171">429</span></span>
<span data-ttu-id="3c409-172">Stavový kód `429` bude vrácen, pokud jste překročili kvótu hello počet dokumentů na index.</span><span class="sxs-lookup"><span data-stu-id="3c409-172">A status code of `429` will be returned when you have exceeded your quota on hello number of documents per index.</span></span>

#### <a name="503"></a><span data-ttu-id="3c409-173">503</span><span class="sxs-lookup"><span data-stu-id="3c409-173">503</span></span>
<span data-ttu-id="3c409-174">Stavový kód `503` bude vrácen, pokud žádná z položek hello v požadavku hello byly úspěšně indexovat.</span><span class="sxs-lookup"><span data-stu-id="3c409-174">A status code of `503` will be returned if none of hello items in hello request were successfully indexed.</span></span> <span data-ttu-id="3c409-175">Tato chyba znamená, že hello systém je velmi zatížen a váš požadavek nelze zpracovat v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="3c409-175">This error means that hello system is under heavy load and your request can't be processed at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="3c409-176">V tom případě důrazně doporučujeme, aby se váš klientský kód stáhnul a chvíli počkal před tím, než to zkusí znovu.</span><span class="sxs-lookup"><span data-stu-id="3c409-176">In this case, we highly recommend that your client code back off and wait before retrying.</span></span> <span data-ttu-id="3c409-177">To vám poskytne hello systému některé toorecover čas, zvýšit hello šanci na úspěšné provedení dalších požadavků.</span><span class="sxs-lookup"><span data-stu-id="3c409-177">This will give hello system some time toorecover, increasing hello chances that future requests will succeed.</span></span> <span data-ttu-id="3c409-178">Rychlé opakování požadavků pouze prodlouží situaci hello.</span><span class="sxs-lookup"><span data-stu-id="3c409-178">Rapidly retrying your requests will only prolong hello situation.</span></span>
>
>

<span data-ttu-id="3c409-179">Další informace o akcích dokumentu a úspěšných/neúspěšných odpovědích naleznete v tématu [Přidání, aktualizování nebo odstranění dokumentů](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents).</span><span class="sxs-lookup"><span data-stu-id="3c409-179">For more information on document actions and success/error responses, please see [Add, Update, or Delete Documents](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents).</span></span> <span data-ttu-id="3c409-180">Další informace o stavových kódech HTTP, které se mohou vrátit v případě selhání, naleznete v tématu [Stavové kódy HTTP (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span><span class="sxs-lookup"><span data-stu-id="3c409-180">For more information on other HTTP status codes that could be returned in case of failure, see [HTTP status codes (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c409-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c409-181">Next steps</span></span>
<span data-ttu-id="3c409-182">Po naplnění indexu Azure Search, bude připravená toostart vystavování toosearch dotazy pro dokumenty.</span><span class="sxs-lookup"><span data-stu-id="3c409-182">After populating your Azure Search index, you will be ready toostart issuing queries toosearch for documents.</span></span> <span data-ttu-id="3c409-183">Podrobnosti naleznete v tématu [Dotazování indexu Azure Search](search-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3c409-183">See [Query Your Azure Search Index](search-query-overview.md) for details.</span></span>
