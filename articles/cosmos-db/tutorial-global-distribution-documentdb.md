---
title: "kurz globální distribuční aaaAzure Cosmos DB pro rozhraní API DocumentDB | Microsoft Docs"
description: "Zjistěte, jak pomocí globální distribuční databázi Cosmos Azure toosetup hello DocumentDB rozhraní API."
services: cosmos-db
keywords: "globální distribuční, documentdb"
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: a1d5f01faa62407fbbc9c078ef4a9589a1a29219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-documentdb-api"></a><span data-ttu-id="0adee-104">Jak toosetup Azure Cosmos DB globální distribuční pomocí rozhraní API DocumentDB hello</span><span class="sxs-lookup"><span data-stu-id="0adee-104">How toosetup Azure Cosmos DB global distribution using hello DocumentDB API</span></span>

<span data-ttu-id="0adee-105">V tomto článku ukážeme, jak toouse hello Azure portálu toosetup globální distribuční databázi Cosmos Azure a potom se připojte pomocí hello DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0adee-105">In this article, we show how toouse hello Azure portal toosetup Azure Cosmos DB global distribution and then connect using hello DocumentDB API.</span></span>

<span data-ttu-id="0adee-106">Tento článek se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="0adee-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="0adee-107">Nakonfigurujte globální distribuci pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0adee-107">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="0adee-108">Nakonfigurujte globální distribuci pomocí hello [DocumentDB rozhraní API](documentdb-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="0adee-108">Configure global distribution using hello [DocumentDB APIs](documentdb-introduction.md)</span></span>

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-documentdb-api"></a><span data-ttu-id="0adee-109">Připojení tooa upřednostňovaná oblast pomocí hello DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0adee-109">Connecting tooa preferred region using hello DocumentDB API</span></span>

<span data-ttu-id="0adee-110">V pořadí tootake výhody [globální distribuční](distribute-data-globally.md), klientské aplikace můžete zadat hello seřazené předvoleb seznam oblastí toobe použít tooperform dokumentu operace.</span><span class="sxs-lookup"><span data-stu-id="0adee-110">In order tootake advantage of [global distribution](distribute-data-globally.md), client applications can specify hello ordered preference list of regions toobe used tooperform document operations.</span></span> <span data-ttu-id="0adee-111">Tento krok můžete provést nastavením zásad pro připojení hello.</span><span class="sxs-lookup"><span data-stu-id="0adee-111">This can be done by setting hello connection policy.</span></span> <span data-ttu-id="0adee-112">V závislosti na konfiguraci účtu Azure Cosmos DB hello aktuální místní dostupnosti a seznamu předvoleb hello zadat, hello většina optimální koncového bodu, bude použita volba hello DocumentDB SDK tooperform zápisu a operace čtení.</span><span class="sxs-lookup"><span data-stu-id="0adee-112">Based on hello Azure Cosmos DB account configuration, current regional availability and hello preference list specified, hello most optimal endpoint will be chosen by hello DocumentDB SDK tooperform write and read operations.</span></span>

<span data-ttu-id="0adee-113">Tento seznam předvoleb je zadána při inicializaci připojení pomocí hello DocumentDB SDK.</span><span class="sxs-lookup"><span data-stu-id="0adee-113">This preference list is specified when initializing a connection using hello DocumentDB SDKs.</span></span> <span data-ttu-id="0adee-114">Hello sady SDK přijmout volitelný parametr "PreferredLocations" tedy uspořádaný seznam oblastí Azure.</span><span class="sxs-lookup"><span data-stu-id="0adee-114">hello SDKs accept an optional parameter "PreferredLocations" that is an ordered list of Azure regions.</span></span>

<span data-ttu-id="0adee-115">Hello SDK automaticky odesílat všechny zápisy toohello aktuální zápisu oblasti.</span><span class="sxs-lookup"><span data-stu-id="0adee-115">hello SDK will automatically send all writes toohello current write region.</span></span>

<span data-ttu-id="0adee-116">Všechny operace čtení odešle první dostupné oblasti toohello v seznamu PreferredLocations hello.</span><span class="sxs-lookup"><span data-stu-id="0adee-116">All reads will be sent toohello first available region in hello PreferredLocations list.</span></span> <span data-ttu-id="0adee-117">Pokud hello požadavek selže, hello klienta se nezdaří dolů hello seznamu toohello další oblasti a tak dále.</span><span class="sxs-lookup"><span data-stu-id="0adee-117">If hello request fails, hello client will fail down hello list toohello next region, and so on.</span></span>

<span data-ttu-id="0adee-118">Hello sady SDK se pouze pokusí tooread z oblastí hello zadaný v PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="0adee-118">hello SDKs will only attempt tooread from hello regions specified in PreferredLocations.</span></span> <span data-ttu-id="0adee-119">Ano například pokud hello databázového účtu je k dispozici v tři oblasti, ale hello klienta pouze určuje pro PreferredLocations dvou oblastí bez zápisu hello, pak žádný čtení se zpracuje mimo oblast hello zápisu, i v případě hello převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="0adee-119">So, for example, if hello Database Account is available in three regions, but hello client only specifies two of hello non-write regions for PreferredLocations, then no reads will be served out of hello write region, even in hello case of failover.</span></span>

<span data-ttu-id="0adee-120">aplikace Hello můžete ověřte hello aktuální koncový bod zápisu a čtení koncový bod zvolí hello SDK pomocí kontrola dvě vlastnosti WriteEndpoint a ReadEndpoint, k dispozici ve verzi sady SDK 1.8 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="0adee-120">hello application can verify hello current write endpoint and read endpoint chosen by hello SDK by checking two properties, WriteEndpoint and ReadEndpoint, available in SDK version 1.8 and above.</span></span>

<span data-ttu-id="0adee-121">Pokud není nastavena vlastnost PreferredLocations hello, bude z aktuální oblasti zápisu hello zpracovat všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="0adee-121">If hello PreferredLocations property is not set, all requests will be served from hello current write region.</span></span>

## <a name="net-sdk"></a><span data-ttu-id="0adee-122">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="0adee-122">.NET SDK</span></span>
<span data-ttu-id="0adee-123">Hello SDK můžete použít beze změn kódu.</span><span class="sxs-lookup"><span data-stu-id="0adee-123">hello SDK can be used without any code changes.</span></span> <span data-ttu-id="0adee-124">V takovém případě hello SDK automaticky přesměruje obě operace čtení a zapíše toohello aktuální zápisu oblasti.</span><span class="sxs-lookup"><span data-stu-id="0adee-124">In this case, hello SDK automatically directs both reads and writes toohello current write region.</span></span>

<span data-ttu-id="0adee-125">Ve verzi 1,8 a novější. hello .NET SDK hello ConnectionPolicy parametr pro konstruktor DocumentClient hello má vlastnost s názvem Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="0adee-125">In version 1.8 and later of hello .NET SDK, hello ConnectionPolicy parameter for hello DocumentClient constructor has a property called Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations.</span></span> <span data-ttu-id="0adee-126">Tato vlastnost je typu kolekce `<string>` a musí obsahovat seznam názvů oblast.</span><span class="sxs-lookup"><span data-stu-id="0adee-126">This property is of type Collection `<string>` and should contain a list of region names.</span></span> <span data-ttu-id="0adee-127">Hello řetězcové hodnoty jsou formátovány každý sloupec název oblasti hello na hello [oblasti Azure] [ regions] stránky, bez mezer před nebo po hello první a poslední znak v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="0adee-127">hello string values are formatted per hello Region Name column on hello [Azure Regions][regions] page, with no spaces before or after hello first and last character respectively.</span></span>

<span data-ttu-id="0adee-128">Hello aktuální zápisu a čtení koncové body k dispozici v DocumentClient.WriteEndpoint a DocumentClient.ReadEndpoint v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="0adee-128">hello current write and read endpoints are available in DocumentClient.WriteEndpoint and DocumentClient.ReadEndpoint respectively.</span></span>

> [!NOTE]
> <span data-ttu-id="0adee-129">Hello adresy URL pro koncové body hello by se neměla považovat jako dlohotrvající konstanty.</span><span class="sxs-lookup"><span data-stu-id="0adee-129">hello URLs for hello endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="0adee-130">Služba Hello může aktualizovat tyto v libovolném bodě.</span><span class="sxs-lookup"><span data-stu-id="0adee-130">hello service may update these at any point.</span></span> <span data-ttu-id="0adee-131">Tato změna zpracovává Hello SDK automaticky.</span><span class="sxs-lookup"><span data-stu-id="0adee-131">hello SDK handles this change automatically.</span></span>
>
>

```csharp
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;
  
ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect tooDocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejs-javascript-and-python-sdks"></a><span data-ttu-id="0adee-132">NodeJS, JavaScript a Python SDK</span><span class="sxs-lookup"><span data-stu-id="0adee-132">NodeJS, JavaScript, and Python SDKs</span></span>
<span data-ttu-id="0adee-133">Hello SDK můžete použít beze změn kódu.</span><span class="sxs-lookup"><span data-stu-id="0adee-133">hello SDK can be used without any code changes.</span></span> <span data-ttu-id="0adee-134">V takovém případě hello SDK bude automaticky nasměrovat jak čte a zapisuje toohello aktuální zápisu oblasti.</span><span class="sxs-lookup"><span data-stu-id="0adee-134">In this case, hello SDK will automatically direct both reads and writes toohello current write region.</span></span>

<span data-ttu-id="0adee-135">V verze 1,8 a později každý SDK hello ConnectionPolicy parametr pro konstruktor DocumentClient hello novou vlastnost s názvem DocumentClient.ConnectionPolicy.PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="0adee-135">In version 1.8 and later of each SDK, hello ConnectionPolicy parameter for hello DocumentClient constructor a new property called DocumentClient.ConnectionPolicy.PreferredLocations.</span></span> <span data-ttu-id="0adee-136">To je parametr pole řetězců, která přebírá seznam názvů oblast.</span><span class="sxs-lookup"><span data-stu-id="0adee-136">This is parameter is an array of strings that takes a list of region names.</span></span> <span data-ttu-id="0adee-137">názvy Hello jsou formátovány za hello oblast název sloupce v hello [oblasti Azure] [ regions] stránky.</span><span class="sxs-lookup"><span data-stu-id="0adee-137">hello names are formatted per hello Region Name column in hello [Azure Regions][regions] page.</span></span> <span data-ttu-id="0adee-138">Můžete použít také hello předdefinované konstanty v objektu pohodlí hello AzureDocuments.Regions</span><span class="sxs-lookup"><span data-stu-id="0adee-138">You can also use hello predefined constants in hello convenience object AzureDocuments.Regions</span></span>

<span data-ttu-id="0adee-139">Hello aktuální zápisu a čtení koncové body k dispozici v DocumentClient.getWriteEndpoint a DocumentClient.getReadEndpoint v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="0adee-139">hello current write and read endpoints are available in DocumentClient.getWriteEndpoint and DocumentClient.getReadEndpoint respectively.</span></span>

> [!NOTE]
> <span data-ttu-id="0adee-140">Hello adresy URL pro koncové body hello by se neměla považovat jako dlohotrvající konstanty.</span><span class="sxs-lookup"><span data-stu-id="0adee-140">hello URLs for hello endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="0adee-141">Služba Hello může aktualizovat tyto v libovolném bodě.</span><span class="sxs-lookup"><span data-stu-id="0adee-141">hello service may update these at any point.</span></span> <span data-ttu-id="0adee-142">Tato změna bude zpracována Hello SDK automaticky.</span><span class="sxs-lookup"><span data-stu-id="0adee-142">hello SDK will handle this change automatically.</span></span>
>
>

<span data-ttu-id="0adee-143">Níže je příklad kódu pro NodeJS/Javascript.</span><span class="sxs-lookup"><span data-stu-id="0adee-143">Below is a code example for NodeJS/Javascript.</span></span> <span data-ttu-id="0adee-144">Python a Java bude postupovat podle hello stejného vzoru.</span><span class="sxs-lookup"><span data-stu-id="0adee-144">Python and Java will follow hello same pattern.</span></span>

```java
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in hello following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize hello connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="rest"></a><span data-ttu-id="0adee-145">REST</span><span class="sxs-lookup"><span data-stu-id="0adee-145">REST</span></span>
<span data-ttu-id="0adee-146">Jakmile se databázový účet má k dispozici v několika oblastech, klienti mohou odesílat dotazy jeho dostupnost provedením požadavek GET na hello následující identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="0adee-146">Once a database account has been made available in multiple regions, clients can query its availability by performing a GET request on hello following URI.</span></span>

    https://{databaseaccount}.documents.azure.com/

<span data-ttu-id="0adee-147">Hello služby, vrátí se seznam oblastí a jejich odpovídající endpoint Azure Cosmos DB identifikátory URI pro repliky hello.</span><span class="sxs-lookup"><span data-stu-id="0adee-147">hello service will return a list of regions and their corresponding Azure Cosmos DB endpoint URIs for hello replicas.</span></span> <span data-ttu-id="0adee-148">aktuální oblast zápisu Hello budou uvedené v odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="0adee-148">hello current write region will be indicated in hello response.</span></span> <span data-ttu-id="0adee-149">Hello klienta můžete pak vybrat hello příslušný koncový bod pro všechny další požadavky REST API následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="0adee-149">hello client can then select hello appropriate endpoint for all further REST API requests as follows.</span></span>

<span data-ttu-id="0adee-150">Příklad odpovědi</span><span class="sxs-lookup"><span data-stu-id="0adee-150">Example response</span></span>

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


* <span data-ttu-id="0adee-151">Požadavky PUT, POST a DELETE musí přejít toohello uvedené zápisu identifikátoru URI</span><span class="sxs-lookup"><span data-stu-id="0adee-151">All PUT, POST and DELETE requests must go toohello indicated write URI</span></span>
* <span data-ttu-id="0adee-152">Získá všechny a ostatních jen pro čtení požadavků (například dotazy) může se stát, koncový bod tooany výběru hello klienta</span><span class="sxs-lookup"><span data-stu-id="0adee-152">All GETs and other read-only requests (for example queries) may go tooany endpoint of hello client’s choice</span></span>

<span data-ttu-id="0adee-153">Zapisovat pouze tooread oblasti požadavky selže s kódem chyby protokolu HTTP 403 "(zakázáno).</span><span class="sxs-lookup"><span data-stu-id="0adee-153">Write requests tooread-only regions will fail with HTTP error code 403 (“Forbidden”).</span></span>

<span data-ttu-id="0adee-154">Pokud oblast zápisu hello změní po zapíše hello klienta počáteční zjišťování fázi následné toohello předchozí zápisu oblast selže s kódem chyby protokolu HTTP 403 "(zakázáno).</span><span class="sxs-lookup"><span data-stu-id="0adee-154">If hello write region changes after hello client’s initial discovery phase, subsequent writes toohello previous write region will fail with HTTP error code 403 (“Forbidden”).</span></span> <span data-ttu-id="0adee-155">Hello Klient by pak získat hello seznam oblastí znovu tooget hello aktualizované zápisu oblast.</span><span class="sxs-lookup"><span data-stu-id="0adee-155">hello client should then GET hello list of regions again tooget hello updated write region.</span></span>

<span data-ttu-id="0adee-156">Je to, že dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0adee-156">That's it, that completes this tutorial.</span></span> <span data-ttu-id="0adee-157">Další informace jak toomanage hello konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="0adee-157">You can learn how toomanage hello consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="0adee-158">A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="0adee-158">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0adee-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0adee-159">Next steps</span></span>

<span data-ttu-id="0adee-160">V tomto kurzu provedete krok hello následující:</span><span class="sxs-lookup"><span data-stu-id="0adee-160">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0adee-161">Nakonfigurujte globální distribuci pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0adee-161">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="0adee-162">Nakonfigurujte globální distribuci pomocí hello DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0adee-162">Configure global distribution using hello DocumentDB APIs</span></span>

<span data-ttu-id="0adee-163">Nyní můžete přejít toohello další kurz toolearn jak hello toodevelop místně pomocí emulátoru místního Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0adee-163">You can now proceed toohello next tutorial toolearn how toodevelop locally using hello Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0adee-164">Vývoj místně pomocí emulátoru hello</span><span class="sxs-lookup"><span data-stu-id="0adee-164">Develop locally with hello emulator</span></span>](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

