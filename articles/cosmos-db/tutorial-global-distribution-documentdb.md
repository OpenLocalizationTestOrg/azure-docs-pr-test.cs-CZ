---
title: "Globální distribuční kurz pro Azure Cosmos DB pro rozhraní API DocumentDB | Microsoft Docs"
description: "Zjistěte, jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní API pro DocumentDB."
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
ms.openlocfilehash: f4d8efe9814bd28bb902567a23b541bc9b5414a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-documentdb-api"></a><span data-ttu-id="ecf8f-104">Jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní API DocumentDB</span><span class="sxs-lookup"><span data-stu-id="ecf8f-104">How to setup Azure Cosmos DB global distribution using the DocumentDB API</span></span>

<span data-ttu-id="ecf8f-105">V tomto článku jsme ukazují, jak nastavit globální distribuční databázi Cosmos Azure a potom se připojte pomocí rozhraní API DocumentDB pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-105">In this article, we show how to use the Azure portal to setup Azure Cosmos DB global distribution and then connect using the DocumentDB API.</span></span>

<span data-ttu-id="ecf8f-106">Tento článek obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="ecf8f-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="ecf8f-107">Nakonfigurujte globální distribuci pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ecf8f-107">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="ecf8f-108">Nakonfigurujte globální distribuční pomocí [DocumentDB rozhraní API](documentdb-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="ecf8f-108">Configure global distribution using the [DocumentDB APIs](documentdb-introduction.md)</span></span>

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-documentdb-api"></a><span data-ttu-id="ecf8f-109">Připojování k upřednostňovaná oblast pomocí rozhraní API DocumentDB</span><span class="sxs-lookup"><span data-stu-id="ecf8f-109">Connecting to a preferred region using the DocumentDB API</span></span>

<span data-ttu-id="ecf8f-110">Aby bylo možné využít výhod [globální distribuční](distribute-data-globally.md), klientské aplikace můžete zadat seznam seřazený předvoleb oblastí se používá k provádění operací dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-110">In order to take advantage of [global distribution](distribute-data-globally.md), client applications can specify the ordered preference list of regions to be used to perform document operations.</span></span> <span data-ttu-id="ecf8f-111">Tento krok můžete provést nastavením zásad pro připojení.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-111">This can be done by setting the connection policy.</span></span> <span data-ttu-id="ecf8f-112">Na základě konfigurace účtu Azure Cosmos DB, aktuální místní dostupnosti a seznamu předvoleb zadaný, optimální koncového bodu, bude použita volba DocumentDB SDK k provedení operace zápisu a operace čtení.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-112">Based on the Azure Cosmos DB account configuration, current regional availability and the preference list specified, the most optimal endpoint will be chosen by the DocumentDB SDK to perform write and read operations.</span></span>

<span data-ttu-id="ecf8f-113">Tento seznam předvoleb je zadána při inicializaci připojení pomocí DocumentDB SDK.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-113">This preference list is specified when initializing a connection using the DocumentDB SDKs.</span></span> <span data-ttu-id="ecf8f-114">Sady SDK přijmout volitelný parametr "PreferredLocations" tedy uspořádaný seznam oblastí Azure.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-114">The SDKs accept an optional parameter "PreferredLocations" that is an ordered list of Azure regions.</span></span>

<span data-ttu-id="ecf8f-115">Sada SDK automaticky odesílat všechny zápisy na aktuální zápisu oblast.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-115">The SDK will automatically send all writes to the current write region.</span></span>

<span data-ttu-id="ecf8f-116">Všechny operace čtení odešle první oblasti k dispozici v seznamu PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-116">All reads will be sent to the first available region in the PreferredLocations list.</span></span> <span data-ttu-id="ecf8f-117">Pokud se požadavek nezdaří, klient se nezdaří dolů v seznamu další oblasti a tak dále.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-117">If the request fails, the client will fail down the list to the next region, and so on.</span></span>

<span data-ttu-id="ecf8f-118">Sady SDK se pouze pokusí číst z oblastí, zadaný v PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-118">The SDKs will only attempt to read from the regions specified in PreferredLocations.</span></span> <span data-ttu-id="ecf8f-119">Ano například pokud databázový účet je k dispozici v tři oblasti, ale klient pouze určuje pro PreferredLocations dvou oblastí bez zápisu, pak žádný čtení se zpracuje mimo oblast zápisu, i v případě převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-119">So, for example, if the Database Account is available in three regions, but the client only specifies two of the non-write regions for PreferredLocations, then no reads will be served out of the write region, even in the case of failover.</span></span>

<span data-ttu-id="ecf8f-120">Aplikace můžete ověřit aktuální koncový bod zápisu a čtení koncový bod vybrali SDK kontrolou dvě vlastnosti WriteEndpoint a ReadEndpoint dostupné ve verzi sady SDK 1.8 a výše.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-120">The application can verify the current write endpoint and read endpoint chosen by the SDK by checking two properties, WriteEndpoint and ReadEndpoint, available in SDK version 1.8 and above.</span></span>

<span data-ttu-id="ecf8f-121">Pokud není nastavena vlastnost PreferredLocations, bude z oblasti aktuální zápisu zpracovat všechny požadavky.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-121">If the PreferredLocations property is not set, all requests will be served from the current write region.</span></span>

## <a name="net-sdk"></a><span data-ttu-id="ecf8f-122">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="ecf8f-122">.NET SDK</span></span>
<span data-ttu-id="ecf8f-123">Sady SDK můžete použít beze změn kódu.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-123">The SDK can be used without any code changes.</span></span> <span data-ttu-id="ecf8f-124">V takovém případě sady SDK automaticky přesměruje obě operace čtení a zapíše do aktuální oblasti zápisu.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-124">In this case, the SDK automatically directs both reads and writes to the current write region.</span></span>

<span data-ttu-id="ecf8f-125">Ve verzi 1,8 a později sady .NET SDK ConnectionPolicy parametr pro konstruktor DocumentClient má vlastnost s názvem Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-125">In version 1.8 and later of the .NET SDK, the ConnectionPolicy parameter for the DocumentClient constructor has a property called Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations.</span></span> <span data-ttu-id="ecf8f-126">Tato vlastnost je typu kolekce `<string>` a musí obsahovat seznam názvů oblast.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-126">This property is of type Collection `<string>` and should contain a list of region names.</span></span> <span data-ttu-id="ecf8f-127">Řetězcové hodnoty jsou formátovány na sloupci Název oblasti na [oblasti Azure] [ regions] stránky, bez mezer před nebo po první a poslední znak v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-127">The string values are formatted per the Region Name column on the [Azure Regions][regions] page, with no spaces before or after the first and last character respectively.</span></span>

<span data-ttu-id="ecf8f-128">V uvedeném pořadí jsou k dispozici v DocumentClient.WriteEndpoint a DocumentClient.ReadEndpoint aktuální zápisu a čtení koncové body.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-128">The current write and read endpoints are available in DocumentClient.WriteEndpoint and DocumentClient.ReadEndpoint respectively.</span></span>

> [!NOTE]
> <span data-ttu-id="ecf8f-129">Adresy URL pro koncové body by se neměla považovat jako dlohotrvající konstanty.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-129">The URLs for the endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="ecf8f-130">Služba může aktualizovat tyto v libovolném bodě.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-130">The service may update these at any point.</span></span> <span data-ttu-id="ecf8f-131">Sada SDK zpracovává tuto změnu automaticky.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-131">The SDK handles this change automatically.</span></span>
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

// connect to DocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejs-javascript-and-python-sdks"></a><span data-ttu-id="ecf8f-132">NodeJS, JavaScript a Python SDK</span><span class="sxs-lookup"><span data-stu-id="ecf8f-132">NodeJS, JavaScript, and Python SDKs</span></span>
<span data-ttu-id="ecf8f-133">Sady SDK můžete použít beze změn kódu.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-133">The SDK can be used without any code changes.</span></span> <span data-ttu-id="ecf8f-134">V takovém případě sada SDK bude automaticky přímé čtení i zápisy na aktuální zápisu oblast.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-134">In this case, the SDK will automatically direct both reads and writes to the current write region.</span></span>

<span data-ttu-id="ecf8f-135">Ve verzi 1,8 a novější každý SDK ConnectionPolicy parametr pro konstruktor DocumentClient novou vlastnost s názvem DocumentClient.ConnectionPolicy.PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-135">In version 1.8 and later of each SDK, the ConnectionPolicy parameter for the DocumentClient constructor a new property called DocumentClient.ConnectionPolicy.PreferredLocations.</span></span> <span data-ttu-id="ecf8f-136">To je parametr pole řetězců, která přebírá seznam názvů oblast.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-136">This is parameter is an array of strings that takes a list of region names.</span></span> <span data-ttu-id="ecf8f-137">Názvy jsou formátovány za ve sloupci Název oblasti [oblasti Azure] [ regions] stránky.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-137">The names are formatted per the Region Name column in the [Azure Regions][regions] page.</span></span> <span data-ttu-id="ecf8f-138">Také můžete použít předdefinované konstanty v objektu pohodlí AzureDocuments.Regions</span><span class="sxs-lookup"><span data-stu-id="ecf8f-138">You can also use the predefined constants in the convenience object AzureDocuments.Regions</span></span>

<span data-ttu-id="ecf8f-139">V uvedeném pořadí jsou k dispozici v DocumentClient.getWriteEndpoint a DocumentClient.getReadEndpoint aktuální zápisu a čtení koncové body.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-139">The current write and read endpoints are available in DocumentClient.getWriteEndpoint and DocumentClient.getReadEndpoint respectively.</span></span>

> [!NOTE]
> <span data-ttu-id="ecf8f-140">Adresy URL pro koncové body by se neměla považovat jako dlohotrvající konstanty.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-140">The URLs for the endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="ecf8f-141">Služba může aktualizovat tyto v libovolném bodě.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-141">The service may update these at any point.</span></span> <span data-ttu-id="ecf8f-142">Tato změna bude zpracována sady SDK automaticky.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-142">The SDK will handle this change automatically.</span></span>
>
>

<span data-ttu-id="ecf8f-143">Níže je příklad kódu pro NodeJS/Javascript.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-143">Below is a code example for NodeJS/Javascript.</span></span> <span data-ttu-id="ecf8f-144">Python a Java bude postupují stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-144">Python and Java will follow the same pattern.</span></span>

```java
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in the following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize the connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="rest"></a><span data-ttu-id="ecf8f-145">REST</span><span class="sxs-lookup"><span data-stu-id="ecf8f-145">REST</span></span>
<span data-ttu-id="ecf8f-146">Jakmile se databázový účet má k dispozici v několika oblastech, klienti mohou odesílat dotazy jeho dostupnost provedením požadavek GET na následující identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-146">Once a database account has been made available in multiple regions, clients can query its availability by performing a GET request on the following URI.</span></span>

    https://{databaseaccount}.documents.azure.com/

<span data-ttu-id="ecf8f-147">Služba, vrátí se seznam oblastí a jejich odpovídající Azure Cosmos DB koncový bod identifikátory URI pro repliky.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-147">The service will return a list of regions and their corresponding Azure Cosmos DB endpoint URIs for the replicas.</span></span> <span data-ttu-id="ecf8f-148">Aktuální oblasti zápisu budou uvedené v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-148">The current write region will be indicated in the response.</span></span> <span data-ttu-id="ecf8f-149">Klienta můžete pak vybrat příslušný koncový bod pro všechny další požadavky REST API následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-149">The client can then select the appropriate endpoint for all further REST API requests as follows.</span></span>

<span data-ttu-id="ecf8f-150">Příklad odpovědi</span><span class="sxs-lookup"><span data-stu-id="ecf8f-150">Example response</span></span>

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


* <span data-ttu-id="ecf8f-151">Požadavky PUT, POST a DELETE musí přejít do uvedeného zápisu identifikátoru URI</span><span class="sxs-lookup"><span data-stu-id="ecf8f-151">All PUT, POST and DELETE requests must go to the indicated write URI</span></span>
* <span data-ttu-id="ecf8f-152">Získá všechny a ostatních jen pro čtení požadavků (například dotazy) může přejděte na libovolný koncový bod podle svého výběru</span><span class="sxs-lookup"><span data-stu-id="ecf8f-152">All GETs and other read-only requests (for example queries) may go to any endpoint of the client’s choice</span></span>

<span data-ttu-id="ecf8f-153">Zápis, požadavky na jen pro čtení oblasti selže s kódem chyby protokolu HTTP 403 "(zakázáno).</span><span class="sxs-lookup"><span data-stu-id="ecf8f-153">Write requests to read-only regions will fail with HTTP error code 403 (“Forbidden”).</span></span>

<span data-ttu-id="ecf8f-154">Pokud oblasti zápisu změn po počáteční zjišťování fáze klienta, následné zápisy do oblasti předchozí zápis selže s kódem chyby protokolu HTTP 403 "(zakázáno).</span><span class="sxs-lookup"><span data-stu-id="ecf8f-154">If the write region changes after the client’s initial discovery phase, subsequent writes to the previous write region will fail with HTTP error code 403 (“Forbidden”).</span></span> <span data-ttu-id="ecf8f-155">Klient pak měli získat seznam oblastí znovu k získání oblasti aktualizované zápisu.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-155">The client should then GET the list of regions again to get the updated write region.</span></span>

<span data-ttu-id="ecf8f-156">Je to, že dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-156">That's it, that completes this tutorial.</span></span> <span data-ttu-id="ecf8f-157">Můžete naučit ke správě konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="ecf8f-157">You can learn how to manage the consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="ecf8f-158">A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="ecf8f-158">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ecf8f-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ecf8f-159">Next steps</span></span>

<span data-ttu-id="ecf8f-160">V tomto kurzu jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="ecf8f-160">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ecf8f-161">Nakonfigurujte globální distribuci pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ecf8f-161">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="ecf8f-162">Nakonfigurujte globální distribuční pomocí rozhraní API DocumentDB</span><span class="sxs-lookup"><span data-stu-id="ecf8f-162">Configure global distribution using the DocumentDB APIs</span></span>

<span data-ttu-id="ecf8f-163">Nyní můžete přejít k dalším kurzu se dozvíte, jak vyvíjet místně pomocí emulátoru místního Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ecf8f-163">You can now proceed to the next tutorial to learn how to develop locally using the Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ecf8f-164">Vývoj místně pomocí emulátoru</span><span class="sxs-lookup"><span data-stu-id="ecf8f-164">Develop locally with the emulator</span></span>](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

