---
title: aaaHow tooUse iOS SDK pro Azure Mobile Apps
description: Jak tooUse iOS SDK pro Azure Mobile Apps
services: app-service\mobile
documentationcenter: ios
author: ysxu
manager: yochayk
editor: 
ms.assetid: 4e8e45df-c36a-4a60-9ad4-393ec10b7eb9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: yuaxu
ms.openlocfilehash: fa299ab3f152bad12d821832fa9fb5495d1fa296
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-ios-client-library-for-azure-mobile-apps"></a><span data-ttu-id="af7ae-103">Jak tooUse iOS Klientská knihovna pro Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="af7ae-103">How tooUse iOS Client Library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="af7ae-104">Tato příručka je určena tooperform běžné scénáře s využitím hello nejnovější [Azure Mobile Apps iOS SDK][1].</span><span class="sxs-lookup"><span data-stu-id="af7ae-104">This guide teaches you tooperform common scenarios using hello latest [Azure Mobile Apps iOS SDK][1].</span></span> <span data-ttu-id="af7ae-105">Pokud jste nový tooAzure Mobile Apps, nejprve dokončit [Azure Mobile Apps rychlý Start] toocreate back-end, vytvořit tabulku a stažení projektu Xcode předdefinovaných iOS.</span><span class="sxs-lookup"><span data-stu-id="af7ae-105">If you are new tooAzure Mobile Apps, first complete [Azure Mobile Apps Quick Start] toocreate a backend, create a table, and download a pre-built iOS Xcode project.</span></span> <span data-ttu-id="af7ae-106">V této příručce se zaměříme na hello klienta iOS SDK.</span><span class="sxs-lookup"><span data-stu-id="af7ae-106">In this guide, we focus on hello client-side iOS SDK.</span></span> <span data-ttu-id="af7ae-107">toolearn Další informace o hello serverové SDK pro back-end hello najdete v tématu hello Server SDK HOWTOs.</span><span class="sxs-lookup"><span data-stu-id="af7ae-107">toolearn more about hello server-side SDK for hello backend, see hello Server SDK HOWTOs.</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="af7ae-108">Referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="af7ae-108">Reference documentation</span></span>
<span data-ttu-id="af7ae-109">Hello referenční dokumentaci k nástroji pro hello iOS klienta SDK se nachází zde: [Azure Mobile Apps iOS odkaz klienta][2].</span><span class="sxs-lookup"><span data-stu-id="af7ae-109">hello reference documentation for hello iOS client SDK is located here: [Azure Mobile Apps iOS Client Reference][2].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="af7ae-110">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="af7ae-110">Supported Platforms</span></span>
<span data-ttu-id="af7ae-111">Hello iOS SDK podporuje jazyka Objective-C projekty – projekty Swift 2.2 a Swift 2.3 projekty pro systém iOS verze 8.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="af7ae-111">hello iOS SDK supports Objective-C projects, Swift 2.2 projects, and Swift 2.3 projects for iOS versions 8.0 or later.</span></span>

<span data-ttu-id="af7ae-112">ověřování "serveru pohybu" Hello používá webové zobrazení pro hello uvedené uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="af7ae-112">hello "server-flow" authentication uses a WebView for hello presented UI.</span></span>  <span data-ttu-id="af7ae-113">Pokud hello zařízení není možné toopresent webové zobrazení uživatelského rozhraní, se vyžaduje jinou metodu ověřování, který je mimo rozsah hello hello produktu.</span><span class="sxs-lookup"><span data-stu-id="af7ae-113">If hello device is not able toopresent a WebView UI, then another method of authentication is required that is outside hello scope of hello product.</span></span>  
<span data-ttu-id="af7ae-114">Tato sada SDK není proto vhodný pro sledování type nebo podobně jako zařízení s omezeným přístupem.</span><span class="sxs-lookup"><span data-stu-id="af7ae-114">This SDK is thus not suitable for Watch-type or similarly restricted devices.</span></span>

## <span data-ttu-id="af7ae-115"><a name="Setup"></a>Instalační program a požadavky</span><span class="sxs-lookup"><span data-stu-id="af7ae-115"><a name="Setup"></a>Setup and Prerequisites</span></span>
<span data-ttu-id="af7ae-116">Tato příručka předpokládá, že jste vytvořili back-end s tabulkou.</span><span class="sxs-lookup"><span data-stu-id="af7ae-116">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="af7ae-117">Tato příručka předpokládá, že tato tabulka hello má stejné schéma jako hello tabulky v těchto kurzech.</span><span class="sxs-lookup"><span data-stu-id="af7ae-117">This guide assumes that hello table has the same schema as hello tables in those tutorials.</span></span> <span data-ttu-id="af7ae-118">Tento průvodce také předpokládá, že ve vašem kódu odkazujete `MicrosoftAzureMobile.framework` a import `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.</span><span class="sxs-lookup"><span data-stu-id="af7ae-118">This guide also assumes that in your code, you reference `MicrosoftAzureMobile.framework` and import `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.</span></span>

## <span data-ttu-id="af7ae-119"><a name="create-client"></a>Postupy: vytvoření klienta</span><span class="sxs-lookup"><span data-stu-id="af7ae-119"><a name="create-client"></a>How to: Create Client</span></span>
<span data-ttu-id="af7ae-120">Vytvoření tooaccess back-end Azure Mobile Apps ve vašem projektu, `MSClient`.</span><span class="sxs-lookup"><span data-stu-id="af7ae-120">tooaccess an Azure Mobile Apps backend in your project, create an `MSClient`.</span></span> <span data-ttu-id="af7ae-121">Nahraďte `AppUrl` se adresa URL aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="af7ae-121">Replace `AppUrl` with hello app URL.</span></span> <span data-ttu-id="af7ae-122">Můžete ponechat `gatewayURLString` a `applicationKey` prázdný.</span><span class="sxs-lookup"><span data-stu-id="af7ae-122">You may leave `gatewayURLString` and `applicationKey` empty.</span></span> <span data-ttu-id="af7ae-123">Pokud jste nastavili bránu pro ověřování, naplnit `gatewayURLString` s adresou URL hello brány.</span><span class="sxs-lookup"><span data-stu-id="af7ae-123">If you set up a gateway for authentication, populate `gatewayURLString` with hello gateway URL.</span></span>

<span data-ttu-id="af7ae-124">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-124">**Objective-C**:</span></span>

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

<span data-ttu-id="af7ae-125">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-125">**Swift**:</span></span>

```
let client = MSClient(applicationURLString: "AppUrl")
```


## <span data-ttu-id="af7ae-126"><a name="table-reference"></a>Postupy: vytvoření odkaz na tabulku</span><span class="sxs-lookup"><span data-stu-id="af7ae-126"><a name="table-reference"></a>How to: Create Table Reference</span></span>
<span data-ttu-id="af7ae-127">tooaccess nebo aktualizace dat, vytvořit tabulku odkaz toohello back-end.</span><span class="sxs-lookup"><span data-stu-id="af7ae-127">tooaccess or update data, create a reference toohello backend table.</span></span> <span data-ttu-id="af7ae-128">Nahraďte `TodoItem` s názvem hello tabulky</span><span class="sxs-lookup"><span data-stu-id="af7ae-128">Replace `TodoItem` with hello name of your table</span></span>

<span data-ttu-id="af7ae-129">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-129">**Objective-C**:</span></span>

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

<span data-ttu-id="af7ae-130">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-130">**Swift**:</span></span>

```
let table = client.tableWithName("TodoItem")
```


## <span data-ttu-id="af7ae-131"><a name="querying"></a>Postupy: dotaz na Data</span><span class="sxs-lookup"><span data-stu-id="af7ae-131"><a name="querying"></a>How to: Query Data</span></span>
<span data-ttu-id="af7ae-132">toocreate dotaz na databázi dotazu hello `MSTable` objektu.</span><span class="sxs-lookup"><span data-stu-id="af7ae-132">toocreate a database query, query hello `MSTable` object.</span></span> <span data-ttu-id="af7ae-133">Hello následující dotaz načte všechny položky hello ve `TodoItem` a protokoly hello text jednotlivých položek.</span><span class="sxs-lookup"><span data-stu-id="af7ae-133">hello following query gets all hello items in `TodoItem` and logs hello text of each item.</span></span>

<span data-ttu-id="af7ae-134">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-134">**Objective-C**:</span></span>

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

<span data-ttu-id="af7ae-135">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-135">**Swift**:</span></span>

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <span data-ttu-id="af7ae-136"><a name="filtering"></a>Postupy: Filtr nevrátil Data</span><span class="sxs-lookup"><span data-stu-id="af7ae-136"><a name="filtering"></a>How to: Filter Returned Data</span></span>
<span data-ttu-id="af7ae-137">výsledky toofilter celá řada možností k dispozici.</span><span class="sxs-lookup"><span data-stu-id="af7ae-137">toofilter results, there are many available options.</span></span>

<span data-ttu-id="af7ae-138">toofilter pomocí predikátu, použijte `NSPredicate` a `readWithPredicate`.</span><span class="sxs-lookup"><span data-stu-id="af7ae-138">toofilter using a predicate, use an `NSPredicate` and `readWithPredicate`.</span></span> <span data-ttu-id="af7ae-139">Následující Hello filtrování položek Todo pouze nedokončené toofind vrácená data.</span><span class="sxs-lookup"><span data-stu-id="af7ae-139">hello following filters returned data toofind only incomplete Todo items.</span></span>

<span data-ttu-id="af7ae-140">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-140">**Objective-C**:</span></span>

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query hello TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

<span data-ttu-id="af7ae-141">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-141">**Swift**:</span></span>

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query hello TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <span data-ttu-id="af7ae-142"><a name="query-object"></a>Postupy: použití MSQuery</span><span class="sxs-lookup"><span data-stu-id="af7ae-142"><a name="query-object"></a>How to: Use MSQuery</span></span>
<span data-ttu-id="af7ae-143">tooperform vytvoření složitých dotazů (včetně řazení a stránkování), `MSQuery` objektu, přímo nebo pomocí predikátu:</span><span class="sxs-lookup"><span data-stu-id="af7ae-143">tooperform a complex query (including sorting and paging), create an `MSQuery` object, directly or by using a predicate:</span></span>

<span data-ttu-id="af7ae-144">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-144">**Objective-C**:</span></span>

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

<span data-ttu-id="af7ae-145">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-145">**Swift**:</span></span>

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

<span data-ttu-id="af7ae-146">`MSQuery`můžete nastavit různé chování dotazu.</span><span class="sxs-lookup"><span data-stu-id="af7ae-146">`MSQuery` lets you control several query behaviors.</span></span>

* <span data-ttu-id="af7ae-147">Určení pořadí výsledků</span><span class="sxs-lookup"><span data-stu-id="af7ae-147">Specify order of results</span></span>
* <span data-ttu-id="af7ae-148">Omezení, která pole tooreturn</span><span class="sxs-lookup"><span data-stu-id="af7ae-148">Limit which fields tooreturn</span></span>
* <span data-ttu-id="af7ae-149">Omezit, kolik zaznamenává tooreturn</span><span class="sxs-lookup"><span data-stu-id="af7ae-149">Limit how many records tooreturn</span></span>
* <span data-ttu-id="af7ae-150">Zadejte celkový počet v odpovědi</span><span class="sxs-lookup"><span data-stu-id="af7ae-150">Specify total count in response</span></span>
* <span data-ttu-id="af7ae-151">Zadejte parametry řetězec vlastního dotazu v žádosti</span><span class="sxs-lookup"><span data-stu-id="af7ae-151">Specify custom query string parameters in request</span></span>
* <span data-ttu-id="af7ae-152">Použít další funkce</span><span class="sxs-lookup"><span data-stu-id="af7ae-152">Apply additional functions</span></span>

<span data-ttu-id="af7ae-153">Spuštění `MSQuery` dotazu voláním `readWithCompletion` hello objektu.</span><span class="sxs-lookup"><span data-stu-id="af7ae-153">Execute an `MSQuery` query by calling `readWithCompletion` on hello object.</span></span>

## <span data-ttu-id="af7ae-154"><a name="sorting"></a>Postupy: řazení dat s MSQuery</span><span class="sxs-lookup"><span data-stu-id="af7ae-154"><a name="sorting"></a>How to: Sort Data with MSQuery</span></span>
<span data-ttu-id="af7ae-155">výsledky toosort, podíváme se na příklad.</span><span class="sxs-lookup"><span data-stu-id="af7ae-155">toosort results, let's look at an example.</span></span> <span data-ttu-id="af7ae-156">vyvolání toosort ve vzestupném pole 'text' a pak podle 'dokončení' sestupném `MSQuery` takto:</span><span class="sxs-lookup"><span data-stu-id="af7ae-156">toosort by field 'text' ascending, then by 'complete' descending, invoke `MSQuery` like so:</span></span>

<span data-ttu-id="af7ae-157">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-157">**Objective-C**:</span></span>

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

<span data-ttu-id="af7ae-158">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-158">**Swift**:</span></span>

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <span data-ttu-id="af7ae-159"><a name="selecting"></a><a name="parameters"></a>Postupy: omezení polí a parametrů řetězce dotazu s MSQuery rozbalte</span><span class="sxs-lookup"><span data-stu-id="af7ae-159"><a name="selecting"></a><a name="parameters"></a>How to: Limit Fields and Expand Query String Parameters with MSQuery</span></span>
<span data-ttu-id="af7ae-160">toobe pole toolimit vrácených dotazem, zadejte hello názvy polí hello v hello **selectFields** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="af7ae-160">toolimit fields toobe returned in a query, specify hello names of hello fields in hello **selectFields** property.</span></span> <span data-ttu-id="af7ae-161">Tento příklad vrátí jenom hello text a dokončené pole:</span><span class="sxs-lookup"><span data-stu-id="af7ae-161">This example returns only hello text and completed fields:</span></span>

<span data-ttu-id="af7ae-162">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-162">**Objective-C**:</span></span>

```
query.selectFields = @[@"text", @"complete"];
```

<span data-ttu-id="af7ae-163">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-163">**Swift**:</span></span>

```
query.selectFields = ["text", "complete"]
```

<span data-ttu-id="af7ae-164">parametrů řetězce dotazu další tooinclude hello serveru požadavku (například, protože je používá vlastní skript na straně serveru), naplnit `query.parameters` takto:</span><span class="sxs-lookup"><span data-stu-id="af7ae-164">tooinclude additional query string parameters in hello server request (for example, because a custom server-side script uses them), populate `query.parameters` like so:</span></span>

<span data-ttu-id="af7ae-165">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-165">**Objective-C**:</span></span>

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

<span data-ttu-id="af7ae-166">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-166">**Swift**:</span></span>

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <span data-ttu-id="af7ae-167"><a name="paging"></a>Postupy: Konfigurace velikost stránky</span><span class="sxs-lookup"><span data-stu-id="af7ae-167"><a name="paging"></a>How to: Configure Page Size</span></span>
<span data-ttu-id="af7ae-168">S Azure Mobile Apps ovládací prvky velikost stránky hello hello počet záznamů, které jsou vyžádány současně z tabulek hello back-end.</span><span class="sxs-lookup"><span data-stu-id="af7ae-168">With Azure Mobile Apps, hello page size controls hello number of records that are pulled at a time from hello backend tables.</span></span> <span data-ttu-id="af7ae-169">A volání příliš`pull` dat by pak dávky dat, podle této velikosti stránky, dokud nejsou žádné další toopull záznamy.</span><span class="sxs-lookup"><span data-stu-id="af7ae-169">A call too`pull` data would then batch up data, based on this page size, until there are no more records toopull.</span></span>

<span data-ttu-id="af7ae-170">Je možné tooconfigure velikost stránky pomocí **MSPullSettings** jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="af7ae-170">It's possible tooconfigure a page size using **MSPullSettings** as shown below.</span></span> <span data-ttu-id="af7ae-171">Výchozí velikost stránky Hello je 50 a hello příklad změny too3.</span><span class="sxs-lookup"><span data-stu-id="af7ae-171">hello default page size is 50, and hello example below changes it too3.</span></span>

<span data-ttu-id="af7ae-172">Můžete nakonfigurovat velikost různé stránky z důvodů výkonu.</span><span class="sxs-lookup"><span data-stu-id="af7ae-172">You could configure a different page size for performance reasons.</span></span> <span data-ttu-id="af7ae-173">Pokud máte velké množství malých datových záznamů, snižuje velikost vysoké stránky hello dobu odezvy serveru.</span><span class="sxs-lookup"><span data-stu-id="af7ae-173">If you have a large number of small data records, a high page size reduces hello number of server round-trips.</span></span>

<span data-ttu-id="af7ae-174">Toto nastavení řídí pouze velikost hello stránky na straně klienta hello.</span><span class="sxs-lookup"><span data-stu-id="af7ae-174">This setting controls only hello page size on hello client side.</span></span> <span data-ttu-id="af7ae-175">Hello klient požaduje větší velikost stránky než back-end mobilní aplikace hello podporuje, je velikost stránky hello limitován hello maximální hello back-end je nakonfigurované toosupport.</span><span class="sxs-lookup"><span data-stu-id="af7ae-175">If hello client asks for a larger page size than hello Mobile Apps backend supports, hello page size is capped at hello maximum hello backend is configured toosupport.</span></span>

<span data-ttu-id="af7ae-176">Toto nastavení je také hello *číslo* datových záznamů, není hello *velikost v bajtech*.</span><span class="sxs-lookup"><span data-stu-id="af7ae-176">This setting is also hello *number* of data records, not hello *byte size*.</span></span>

<span data-ttu-id="af7ae-177">Pokud zvýšíte velikost stránky hello klienta, měli také zvýšit velikost stránky hello na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="af7ae-177">If you increase hello client page size, you should also increase hello page size on hello server.</span></span> <span data-ttu-id="af7ae-178">V tématu ["postup: Upravit velikost stránkování tabulky hello"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) pro hello kroky toodo to.</span><span class="sxs-lookup"><span data-stu-id="af7ae-178">See ["How to: Adjust hello table paging size"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for hello steps toodo this.</span></span>

<span data-ttu-id="af7ae-179">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-179">**Objective-C**:</span></span>

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                }
                           }];
```


<span data-ttu-id="af7ae-180">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-180">**Swift**:</span></span>

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <span data-ttu-id="af7ae-181"><a name="inserting"></a>Postupy: vkládání dat</span><span class="sxs-lookup"><span data-stu-id="af7ae-181"><a name="inserting"></a>How to: Insert Data</span></span>
<span data-ttu-id="af7ae-182">Vytvoření tooinsert řádku nové tabulky `NSDictionary` a vyvolání `table insert`.</span><span class="sxs-lookup"><span data-stu-id="af7ae-182">tooinsert a new table row, create a `NSDictionary` and invoke `table insert`.</span></span> <span data-ttu-id="af7ae-183">Pokud [dynamické schématu] je povoleno, hello Azure App Service mobilního back-endu automaticky vygeneruje nové sloupce podle hello `NSDictionary`.</span><span class="sxs-lookup"><span data-stu-id="af7ae-183">If [Dynamic Schema] is enabled, hello Azure App Service mobile backend automatically generates new columns based on hello `NSDictionary`.</span></span>

<span data-ttu-id="af7ae-184">Pokud `id` není uvedeno, back-end hello generuje automaticky nový jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="af7ae-184">If `id` is not provided, hello backend automatically generates a new unique ID.</span></span> <span data-ttu-id="af7ae-185">Zadejte vlastní `id` toouse e-mailové adresy, uživatelská jména nebo vlastní hodnoty jako ID.</span><span class="sxs-lookup"><span data-stu-id="af7ae-185">Provide your own `id` toouse email addresses, usernames, or your own custom values as ID.</span></span> <span data-ttu-id="af7ae-186">Poskytnutí vlastní ID může usnadňují spojení a logiku orientovanými na firmu databáze.</span><span class="sxs-lookup"><span data-stu-id="af7ae-186">Providing your own ID may ease joins and business-oriented database logic.</span></span>

<span data-ttu-id="af7ae-187">Hello `result` obsahuje hello novou položku, která byla vložena.</span><span class="sxs-lookup"><span data-stu-id="af7ae-187">hello `result` contains hello new item that was inserted.</span></span> <span data-ttu-id="af7ae-188">V závislosti na logice server může mít další nebo upravených dat porovnání toowhat byl předán toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="af7ae-188">Depending on your server logic, it may have additional or modified data compared toowhat was passed toohello server.</span></span>

<span data-ttu-id="af7ae-189">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-189">**Objective-C**:</span></span>

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

<span data-ttu-id="af7ae-190">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-190">**Swift**:</span></span>

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

## <span data-ttu-id="af7ae-191"><a name="modifying"></a>Postupy: Změna dat</span><span class="sxs-lookup"><span data-stu-id="af7ae-191"><a name="modifying"></a>How to: Modify Data</span></span>
<span data-ttu-id="af7ae-192">Upravit tooupdate stávající řádek, položku a volání `update`:</span><span class="sxs-lookup"><span data-stu-id="af7ae-192">tooupdate an existing row, modify an item and call `update`:</span></span>

<span data-ttu-id="af7ae-193">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-193">**Objective-C**:</span></span>

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

<span data-ttu-id="af7ae-194">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-194">**Swift**:</span></span>

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

<span data-ttu-id="af7ae-195">Můžete taky zadejte ID řádku hello a hello aktualizovat pole:</span><span class="sxs-lookup"><span data-stu-id="af7ae-195">Alternatively, supply hello row ID and hello updated field:</span></span>

<span data-ttu-id="af7ae-196">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-196">**Objective-C**:</span></span>

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

<span data-ttu-id="af7ae-197">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-197">**Swift**:</span></span>

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

<span data-ttu-id="af7ae-198">Minimálně, hello `id` musí být nastaven při provádění aktualizací.</span><span class="sxs-lookup"><span data-stu-id="af7ae-198">At minimum, hello `id` attribute must be set when making updates.</span></span>

## <span data-ttu-id="af7ae-199"><a name="deleting"></a>Postupy: odstranění dat</span><span class="sxs-lookup"><span data-stu-id="af7ae-199"><a name="deleting"></a>How to: Delete Data</span></span>
<span data-ttu-id="af7ae-200">vyvolání toodelete některou položku, `delete` s položkou hello:</span><span class="sxs-lookup"><span data-stu-id="af7ae-200">toodelete an item, invoke `delete` with hello item:</span></span>

<span data-ttu-id="af7ae-201">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-201">**Objective-C**:</span></span>

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

<span data-ttu-id="af7ae-202">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-202">**Swift**:</span></span>

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

<span data-ttu-id="af7ae-203">Můžete taky odstraňte tím, že poskytuje ID řádku:</span><span class="sxs-lookup"><span data-stu-id="af7ae-203">Alternatively, delete by providing a row ID:</span></span>

<span data-ttu-id="af7ae-204">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-204">**Objective-C**:</span></span>

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

<span data-ttu-id="af7ae-205">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-205">**Swift**:</span></span>

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

<span data-ttu-id="af7ae-206">Minimálně, hello `id` musí být nastaven při provádění odstraní.</span><span class="sxs-lookup"><span data-stu-id="af7ae-206">At minimum, hello `id` attribute must be set when making deletes.</span></span>

## <span data-ttu-id="af7ae-207"><a name="customapi"></a>Postupy: volání vlastního rozhraní API</span><span class="sxs-lookup"><span data-stu-id="af7ae-207"><a name="customapi"></a>How to: Call Custom API</span></span>
<span data-ttu-id="af7ae-208">Pomocí vlastního rozhraní API můžou zpřístupnit žádné funkce back-end.</span><span class="sxs-lookup"><span data-stu-id="af7ae-208">With a custom API, you can expose any backend functionality.</span></span> <span data-ttu-id="af7ae-209">Operace toomap tooa tabulka nemá.</span><span class="sxs-lookup"><span data-stu-id="af7ae-209">It doesn't have toomap tooa table operation.</span></span> <span data-ttu-id="af7ae-210">Nejen získáte větší kontrolu nad zasílání zpráv, můžete dokonce i pro čtení nebo nastavení hlavičky a změňte formát textu hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="af7ae-210">Not only do you gain more control over messaging, you can even read/set headers and change hello response body format.</span></span> <span data-ttu-id="af7ae-211">jak toocreate vlastní rozhraní API na back-end hello, číst toolearn [vlastní rozhraní API](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)</span><span class="sxs-lookup"><span data-stu-id="af7ae-211">toolearn how toocreate a custom API on hello backend, read [Custom APIs](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)</span></span>

<span data-ttu-id="af7ae-212">volání toocall vlastní rozhraní API, `MSClient.invokeAPI`.</span><span class="sxs-lookup"><span data-stu-id="af7ae-212">toocall a custom API, call `MSClient.invokeAPI`.</span></span> <span data-ttu-id="af7ae-213">Hello žádosti a odpovědi obsahu jsou považovány za JSON.</span><span class="sxs-lookup"><span data-stu-id="af7ae-213">hello request and response content are treated as JSON.</span></span> <span data-ttu-id="af7ae-214">toouse jiné typy médií [hello použijte jiné přetížení `invokeAPI` ] [ 5].</span><span class="sxs-lookup"><span data-stu-id="af7ae-214">toouse other media types, [use hello other overload of `invokeAPI`][5].</span></span>  <span data-ttu-id="af7ae-215">toomake `GET` žádosti o místo `POST` požádat, parametr sady `HTTPMethod` příliš`"GET"` a parametr `body` příliš`nil` (protože požadavky GET ještě nemá obsah zpráv.) Pokud vaše vlastní rozhraní API podporuje další příkazy HTTP, změňte `HTTPMethod` správně.</span><span class="sxs-lookup"><span data-stu-id="af7ae-215">toomake a `GET` request instead of a `POST` request, set parameter `HTTPMethod` too`"GET"` and parameter `body` too`nil` (since GET requests do not have message bodies.) If your custom API supports other HTTP verbs, change `HTTPMethod` appropriately.</span></span>

<span data-ttu-id="af7ae-216">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-216">**Objective-C**:</span></span>

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

<span data-ttu-id="af7ae-217">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-217">**Swift**:</span></span>

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

## <span data-ttu-id="af7ae-218"><a name="templates"></a>Postupy: registrace nabízených šablony toosend napříč platformami oznámení</span><span class="sxs-lookup"><span data-stu-id="af7ae-218"><a name="templates"></a>How to: Register push templates toosend cross-platform notifications</span></span>
<span data-ttu-id="af7ae-219">šablony tooregister předat šablon s vaší **client.push registerDeviceToken** metoda ve vaší klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="af7ae-219">tooregister templates, pass templates with your **client.push registerDeviceToken** method in your client app.</span></span>

<span data-ttu-id="af7ae-220">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-220">**Objective-C**:</span></span>

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

<span data-ttu-id="af7ae-221">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-221">**Swift**:</span></span>

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

<span data-ttu-id="af7ae-222">Vaše šablony typu NSDictionary a může obsahovat několik šablon v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="af7ae-222">Your templates are of type NSDictionary and can contain multiple templates in hello following format:</span></span>

<span data-ttu-id="af7ae-223">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-223">**Objective-C**:</span></span>

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

<span data-ttu-id="af7ae-224">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-224">**Swift**:</span></span>

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

<span data-ttu-id="af7ae-225">Všechny značky se odstraní z hello požadavku pro zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="af7ae-225">All tags are stripped from hello request for security.</span></span>  <span data-ttu-id="af7ae-226">tooadd značky tooinstallations nebo šablony v rámci instalace najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps][4].</span><span class="sxs-lookup"><span data-stu-id="af7ae-226">tooadd tags tooinstallations or templates within installations, see [Work with hello .NET backend server SDK for Azure Mobile Apps][4].</span></span>  <span data-ttu-id="af7ae-227">Pomocí těchto registrovaných šablon oznámení toosend pracovat s [rozhraní API centra oznámení][3].</span><span class="sxs-lookup"><span data-stu-id="af7ae-227">toosend notifications using these registered templates, work with [Notification Hubs APIs][3].</span></span>

## <span data-ttu-id="af7ae-228"><a name="errors"></a>Postupy: zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="af7ae-228"><a name="errors"></a>How to: Handle Errors</span></span>
<span data-ttu-id="af7ae-229">Při volání Azure App Service mobilního back-endu blok dokončení hello obsahuje `NSError` parametr.</span><span class="sxs-lookup"><span data-stu-id="af7ae-229">When you call an Azure App Service mobile backend, hello completion block contains an `NSError` parameter.</span></span> <span data-ttu-id="af7ae-230">Když dojde k chybě, tento parametr je nemá hodnotu nil.</span><span class="sxs-lookup"><span data-stu-id="af7ae-230">When an error occurs, this parameter is non-nil.</span></span> <span data-ttu-id="af7ae-231">V kódu zkontrolujte tento parametr a zpracování chyb hello podle potřeby, jak je předvedeno v hello předchozích fragmentů kódu.</span><span class="sxs-lookup"><span data-stu-id="af7ae-231">In your code, you should check this parameter and handle hello error as needed, as demonstrated in hello preceding code snippets.</span></span>

<span data-ttu-id="af7ae-232">soubor Hello [ `<WindowsAzureMobileServices/MSError.h>` ] [ 6] definuje hello konstanty `MSErrorResponseKey`, `MSErrorRequestKey`, a `MSErrorServerItemKey`.</span><span class="sxs-lookup"><span data-stu-id="af7ae-232">hello file [`<WindowsAzureMobileServices/MSError.h>`][6] defines hello constants `MSErrorResponseKey`, `MSErrorRequestKey`, and `MSErrorServerItemKey`.</span></span> <span data-ttu-id="af7ae-233">tooget další data související s toohello Chyba:</span><span class="sxs-lookup"><span data-stu-id="af7ae-233">tooget more data related toohello error:</span></span>

<span data-ttu-id="af7ae-234">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-234">**Objective-C**:</span></span>

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

<span data-ttu-id="af7ae-235">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-235">**Swift**:</span></span>

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

<span data-ttu-id="af7ae-236">Kromě toho hello soubor definuje konstanty pro každý kód chyby:</span><span class="sxs-lookup"><span data-stu-id="af7ae-236">In addition, hello file defines constants for each error code:</span></span>

<span data-ttu-id="af7ae-237">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-237">**Objective-C**:</span></span>

```
if (error.code == MSErrorPreconditionFailed) {
```

<span data-ttu-id="af7ae-238">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-238">**Swift**:</span></span>

```
if (error.code == MSErrorPreconditionFailed) {
```

## <span data-ttu-id="af7ae-239"><a name="adal"></a>Postupy: ověřuje uživatele pomocí hello knihovna ověřování Active Directory</span><span class="sxs-lookup"><span data-stu-id="af7ae-239"><a name="adal"></a>How to: Authenticate users with hello Active Directory Authentication Library</span></span>
<span data-ttu-id="af7ae-240">Můžete vytvořit hello Active Directory Authentication Library (ADAL) toosign uživatelů do vaší aplikace pomocí Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="af7ae-240">You can use hello Active Directory Authentication Library (ADAL) toosign users into your application using Azure Active Directory.</span></span> <span data-ttu-id="af7ae-241">Tok ověřování klientů pomocí zprostředkovatele identity SDK je vhodnější toousing hello `loginWithProvider:completion:` metoda.</span><span class="sxs-lookup"><span data-stu-id="af7ae-241">Client flow authentication using an identity provider SDK is preferable toousing hello `loginWithProvider:completion:` method.</span></span>  <span data-ttu-id="af7ae-242">Tok ověření klienta poskytuje více nativní UX chování a umožňuje další přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="af7ae-242">Client flow authentication provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="af7ae-243">Nakonfigurujte následující hello váš back-end mobilní aplikace při přihlášení AAD [jak tooconfigure aplikaci služby pro služby Active Directory přihlášení] [ 7] kurzu.</span><span class="sxs-lookup"><span data-stu-id="af7ae-243">Configure your mobile app backend for AAD sign-in by following hello [How tooconfigure App Service for Active Directory login][7] tutorial.</span></span> <span data-ttu-id="af7ae-244">Ujistěte se, že toocomplete hello volitelný krok registrace nativní klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="af7ae-244">Make sure toocomplete hello optional step of registering a native client application.</span></span> <span data-ttu-id="af7ae-245">Pro iOS, doporučujeme, abyste tento hello přesměrování je identifikátor URI hello formuláře `<app-scheme>://<bundle-id>`.</span><span class="sxs-lookup"><span data-stu-id="af7ae-245">For iOS, we recommend that hello redirect URI is of hello form `<app-scheme>://<bundle-id>`.</span></span> <span data-ttu-id="af7ae-246">Další informace najdete v tématu hello [rychlý start ADAL iOS][8].</span><span class="sxs-lookup"><span data-stu-id="af7ae-246">For more information, see hello [ADAL iOS quickstart][8].</span></span>
2. <span data-ttu-id="af7ae-247">Nainstalujte ADAL pomocí Cocoapods.</span><span class="sxs-lookup"><span data-stu-id="af7ae-247">Install ADAL using Cocoapods.</span></span> <span data-ttu-id="af7ae-248">Upravit vaše hello tooinclude Podfile následující definice, nahraďte **YOUR PROJECT** s názvem projektu Xcode hello:</span><span class="sxs-lookup"><span data-stu-id="af7ae-248">Edit your Podfile tooinclude hello following definition, replacing **YOUR-PROJECT** with hello name of your Xcode project:</span></span>

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   <span data-ttu-id="af7ae-249">a hello Pod:</span><span class="sxs-lookup"><span data-stu-id="af7ae-249">and hello Pod:</span></span>

        pod 'ADALiOS'
3. <span data-ttu-id="af7ae-250">Pomocí hello terminálu spustit `pod install` z adresáře hello obsahující projekt a pak otevřete pracovní prostor Xcode hello generované (ne hello projekt).</span><span class="sxs-lookup"><span data-stu-id="af7ae-250">Using hello Terminal, run `pod install` from hello directory containing your project, and then open hello generated Xcode workspace (not hello project).</span></span>
4. <span data-ttu-id="af7ae-251">Přidejte následující kód tooyour aplikace, podle toohello jazyk, který používáte hello.</span><span class="sxs-lookup"><span data-stu-id="af7ae-251">Add hello following code tooyour application, according toohello language you are using.</span></span> <span data-ttu-id="af7ae-252">V každé udělejte tyto náhrady:</span><span class="sxs-lookup"><span data-stu-id="af7ae-252">In each, make these replacements:</span></span>

   * <span data-ttu-id="af7ae-253">Nahraďte **INSERT. AUTORITY zde** s názvem hello hello klienta, ve kterém jste zřídili vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="af7ae-253">Replace **INSERT-AUTHORITY-HERE** with hello name of hello tenant in which you provisioned your application.</span></span> <span data-ttu-id="af7ae-254">Formát by měl být https://login.microsoftonline.com/contoso.onmicrosoft.com. Tuto hodnotu můžete zkopírovat z karty hello domény v Azure Active Directory v hello [portál Azure classic].</span><span class="sxs-lookup"><span data-stu-id="af7ae-254">The format should be https://login.microsoftonline.com/contoso.onmicrosoft.com. This value can be copied from hello Domain tab in your Azure Active Directory in hello [Azure classic portal].</span></span>
   * <span data-ttu-id="af7ae-255">Nahraďte **INSERT-RESOURCE-ID-zde** s ID klienta hello back-endu mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="af7ae-255">Replace **INSERT-RESOURCE-ID-HERE** with hello client ID for your mobile app backend.</span></span> <span data-ttu-id="af7ae-256">ID klienta můžete získat z hello **Upřesnit** v části **nastavení Azure Active Directory** hello portálu.</span><span class="sxs-lookup"><span data-stu-id="af7ae-256">You can obtain the client ID from hello **Advanced** tab under **Azure Active Directory Settings** in hello portal.</span></span>
   * <span data-ttu-id="af7ae-257">Nahraďte **INSERT klienta ID zde** s ID klienta hello jste zkopírovali ze hello nativní klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="af7ae-257">Replace **INSERT-CLIENT-ID-HERE** with hello client ID you copied from hello native client application.</span></span>
   * <span data-ttu-id="af7ae-258">Nahraďte **vložení PŘESMĚROVÁNÍ URI zde** s vaší lokality */.auth/login/done* koncový bod, pomocí hello schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af7ae-258">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using hello HTTPS scheme.</span></span> <span data-ttu-id="af7ae-259">Tato hodnota by mělo být podobné příliš*https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="af7ae-259">This value should be similar too*https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

<span data-ttu-id="af7ae-260">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-260">**Objective-C**:</span></span>

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


<span data-ttu-id="af7ae-261">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-261">**Swift**:</span></span>

    // add hello following imports tooyour bridging header:
    //        #import <ADALiOS/ADAuthenticationContext.h>
    //        #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <span data-ttu-id="af7ae-262"><a name="facebook-sdk"></a>Postupy: ověřuje uživatele pomocí hello Facebook SDK pro iOS</span><span class="sxs-lookup"><span data-stu-id="af7ae-262"><a name="facebook-sdk"></a>How to: Authenticate users with hello Facebook SDK for iOS</span></span>
<span data-ttu-id="af7ae-263">Hello Facebook SDK můžete použít pro uživatele iOS toosign do vaší aplikace pomocí služby Facebook.</span><span class="sxs-lookup"><span data-stu-id="af7ae-263">You can use hello Facebook SDK for iOS toosign users into your application using Facebook.</span></span>  <span data-ttu-id="af7ae-264">Použití tok ověření klienta, je vhodnější toousing hello `loginWithProvider:completion:` metoda.</span><span class="sxs-lookup"><span data-stu-id="af7ae-264">Using a client flow authentication is preferable toousing hello `loginWithProvider:completion:` method.</span></span>  <span data-ttu-id="af7ae-265">Hello klienta tok ověřování poskytuje více nativní UX chování a umožňuje další přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="af7ae-265">hello client flow authentication provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="af7ae-266">Konfigurace váš back-end mobilní aplikace pro Facebook přihlásit pomocí následujících [jak tooconfigure služby aplikace pro Facebook přihlášení] [ 9] kurzu.</span><span class="sxs-lookup"><span data-stu-id="af7ae-266">Configure your mobile app backend for Facebook sign-in by following the [How tooconfigure App Service for Facebook login][9] tutorial.</span></span>
2. <span data-ttu-id="af7ae-267">Nainstalujte hello Facebook SDK pro iOS pomocí následující hello [Facebook SDK pro iOS – Začínáme] [ 10] dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="af7ae-267">Install hello Facebook SDK for iOS by following hello [Facebook SDK for iOS - Getting Started][10] documentation.</span></span> <span data-ttu-id="af7ae-268">Místo vytváření aplikace, můžete přidat hello iOS platformy tooyour existující registraci.</span><span class="sxs-lookup"><span data-stu-id="af7ae-268">Instead of creating an app, you can add hello iOS platform tooyour existing registration.</span></span>
3. <span data-ttu-id="af7ae-269">Dokumentace pro Facebook obsahuje nějaký kód jazyka Objective-C v hello delegáta aplikace.</span><span class="sxs-lookup"><span data-stu-id="af7ae-269">Facebook's documentation includes some Objective-C code in hello App Delegate.</span></span> <span data-ttu-id="af7ae-270">Pokud používáte **Swift**, můžete použít následující překladů pro AppDelegate.swift hello:</span><span class="sxs-lookup"><span data-stu-id="af7ae-270">If you are using **Swift**, you can use hello following translations for AppDelegate.swift:</span></span>

        // Add hello following import tooyour bridging header:
        //        #import <FBSDKCoreKit/FBSDKCoreKit.h>

        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }
4. <span data-ttu-id="af7ae-271">V přidání tooadding `FBSDKCoreKit.framework` tooyour projektu, také přidat odkaz příliš`FBSDKLoginKit.framework` v hello stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="af7ae-271">In addition tooadding `FBSDKCoreKit.framework` tooyour project, also add a reference too`FBSDKLoginKit.framework` in hello same way.</span></span>
5. <span data-ttu-id="af7ae-272">Přidejte následující kód tooyour aplikace, podle toohello jazyk, který používáte hello.</span><span class="sxs-lookup"><span data-stu-id="af7ae-272">Add hello following code tooyour application, according toohello language you are using.</span></span>

<span data-ttu-id="af7ae-273">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-273">**Objective-C**:</span></span>

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {        
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

<span data-ttu-id="af7ae-274">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-274">**Swift**:</span></span>

    // Add hello following imports tooyour bridging header:
    //        #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //        #import <FBSDKCoreKit/FBSDKAccessToken.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <span data-ttu-id="af7ae-275"><a name="twitter-fabric"></a>Postupy: ověřuje uživatele pomocí služby Twitter prostředků infrastruktury pro iOS</span><span class="sxs-lookup"><span data-stu-id="af7ae-275"><a name="twitter-fabric"></a>How to: Authenticate users with Twitter Fabric for iOS</span></span>
<span data-ttu-id="af7ae-276">Prostředky infrastruktury můžete použít pro uživatele iOS toosign do vaší aplikace pomocí služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="af7ae-276">You can use Fabric for iOS toosign users into your application using Twitter.</span></span> <span data-ttu-id="af7ae-277">Tok ověření klienta je vhodnější toousing hello `loginWithProvider:completion:` metoda, protože poskytuje více nativní UX chování a umožňuje další přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="af7ae-277">Client Flow authentication is preferable toousing hello `loginWithProvider:completion:` method, as it provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="af7ae-278">Nakonfigurujte následující hello váš back-end mobilní aplikace při přihlášení Twitter [jak tooconfigure aplikaci služby pro přihlášení služby Twitter](app-service-mobile-how-to-configure-twitter-authentication.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="af7ae-278">Configure your mobile app backend for Twitter sign-in by following hello [How tooconfigure App Service for Twitter login](app-service-mobile-how-to-configure-twitter-authentication.md) tutorial.</span></span>
2. <span data-ttu-id="af7ae-279">Projekt tooyour prostředků infrastruktury můžete přidat následující hello [prostředků infrastruktury pro iOS – Začínáme] dokumentace a nastavení TwitterKit.</span><span class="sxs-lookup"><span data-stu-id="af7ae-279">Add Fabric tooyour project by following hello [Fabric for iOS - Getting Started] documentation and setting up TwitterKit.</span></span>

   > [!NOTE]
   > <span data-ttu-id="af7ae-280">Ve výchozím nastavení prostředků infrastruktury pro vás vytvoří aplikaci služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="af7ae-280">By default, Fabric creates a Twitter application for you.</span></span> <span data-ttu-id="af7ae-281">Vytváření aplikací tak, že zaregistrujete hello uživatelský klíč a tajný klíč příjemce, které jste vytvořili dříve pomocí hello následující fragmenty kódu se můžete vyhnout.</span><span class="sxs-lookup"><span data-stu-id="af7ae-281">You can avoid creating an application by registering hello Consumer Key and Consumer Secret you created earlier using hello following code snippets.</span></span>    <span data-ttu-id="af7ae-282">Alternativně můžete nahradit hello uživatelský klíč a zadejte tooApp služby s hello hodnoty můžete hodnoty uživatelský tajný klíč, najdete v části v hello [řídicí panel infrastruktury].</span><span class="sxs-lookup"><span data-stu-id="af7ae-282">Alternatively, you can replace hello Consumer Key and Consumer Secret values that you provide tooApp Service with hello values you see in hello [Fabric Dashboard].</span></span> <span data-ttu-id="af7ae-283">Pokud zvolíte tuto možnost, být jisti tooset hello zpětného volání adresy URL tooa hodnotu zástupného symbolu, jako třeba `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.</span><span class="sxs-lookup"><span data-stu-id="af7ae-283">If you choose this option, be sure tooset hello callback URL tooa placeholder value, such as `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.</span></span>
   >
   >

    <span data-ttu-id="af7ae-284">Pokud si zvolíte toouse hello tajné klíče, které jste vytvořili dříve, přidejte následující kód tooyour delegáta aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="af7ae-284">If you choose toouse hello secrets you created earlier, add hello following code tooyour App Delegate:</span></span>

    <span data-ttu-id="af7ae-285">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-285">**Objective-C**:</span></span>

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }

    <span data-ttu-id="af7ae-286">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-286">**Swift**:</span></span>

        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
3. <span data-ttu-id="af7ae-287">Přidejte následující kód tooyour aplikace, podle toohello jazyk, který používáte hello.</span><span class="sxs-lookup"><span data-stu-id="af7ae-287">Add hello following code tooyour application, according toohello language you are using.</span></span>

<span data-ttu-id="af7ae-288">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-288">**Objective-C**:</span></span>

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

<span data-ttu-id="af7ae-289">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-289">**Swift**:</span></span>

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <span data-ttu-id="af7ae-290"><a name="google-sdk"></a>Postupy: ověřuje uživatele pomocí hello Google přihlášení SDK pro iOS</span><span class="sxs-lookup"><span data-stu-id="af7ae-290"><a name="google-sdk"></a>How to: Authenticate users with hello Google Sign-In SDK for iOS</span></span>
<span data-ttu-id="af7ae-291">Hello Google přihlášení SDK můžete použít pro uživatele iOS toosign do vaší aplikace pomocí účtu Google.</span><span class="sxs-lookup"><span data-stu-id="af7ae-291">You can use hello Google Sign-In SDK for iOS toosign users into your application using a Google account.</span></span>  <span data-ttu-id="af7ae-292">Google nedávno vydal zásady zabezpečení OAuth tootheir změny.</span><span class="sxs-lookup"><span data-stu-id="af7ae-292">Google recently announced changes tootheir OAuth security policies.</span></span>  <span data-ttu-id="af7ae-293">Tyto změny zásad bude vyžadovat použití hello Google SDK v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="af7ae-293">These policy changes will require hello use of the Google SDK in hello future.</span></span>

1. <span data-ttu-id="af7ae-294">Nakonfigurujte následující hello váš back-end mobilní aplikace pro Google přihlásit [jak tooconfigure aplikaci služby pro Google přihlášení](app-service-mobile-how-to-configure-google-authentication.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="af7ae-294">Configure your mobile app backend for Google sign-in by following hello [How tooconfigure App Service for Google login](app-service-mobile-how-to-configure-google-authentication.md) tutorial.</span></span>
2. <span data-ttu-id="af7ae-295">Nainstalujte hello Google SDK pro iOS pomocí následující hello [přihlášení Google pro iOS – spuštění integrace](https://developers.google.com/identity/sign-in/ios/start-integrating) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="af7ae-295">Install hello Google SDK for iOS by following hello [Google Sign-In for iOS - Start integrating](https://developers.google.com/identity/sign-in/ios/start-integrating) documentation.</span></span> <span data-ttu-id="af7ae-296">Část "Ověřit s back-end Server" hello, můžete vynechat.</span><span class="sxs-lookup"><span data-stu-id="af7ae-296">You may skip hello "Authenticate with a Backend Server" section.</span></span>
3. <span data-ttu-id="af7ae-297">Přidejte následující tooyour delegáta hello `signIn:didSignInForUser:withError:` metoda, podle toohello jazyk používáte.</span><span class="sxs-lookup"><span data-stu-id="af7ae-297">Add hello following tooyour delegate's `signIn:didSignInForUser:withError:` method, according toohello language you are using.</span></span>

<span data-ttu-id="af7ae-298">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-298">**Objective-C**:</span></span>

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };

        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

<span data-ttu-id="af7ae-299">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-299">**Swift**:</span></span>

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

1. <span data-ttu-id="af7ae-300">Ujistěte se také přidat hello následující příliš`application:didFinishLaunchingWithOptions:` ve vaší aplikaci delegovat, nahraďte "SERVER_CLIENT_ID" hello stejné ID této jste použili tooconfigure služby App Service v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="af7ae-300">Make sure you also add hello following too`application:didFinishLaunchingWithOptions:` in your app delegate, replacing "SERVER_CLIENT_ID" with hello same ID that you used tooconfigure App Service in step 1.</span></span>

<span data-ttu-id="af7ae-301">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-301">**Objective-C**:</span></span>

         [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";

 <span data-ttu-id="af7ae-302">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-302">**Swift**:</span></span>

        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"


1. <span data-ttu-id="af7ae-303">Přidejte následující kód tooyour aplikace v UIViewController, který implementuje hello hello `GIDSignInUIDelegate` protokol, podle toohello jazyk používáte.</span><span class="sxs-lookup"><span data-stu-id="af7ae-303">Add hello following code tooyour application in a UIViewController that implements hello `GIDSignInUIDelegate` protocol, according toohello language you are using.</span></span>  <span data-ttu-id="af7ae-304">Jste se odhlásili před se přihlášení znovu a i když není nutné tooenter vaše přihlašovací údaje znovu, se zobrazí dialogové okno souhlasu.</span><span class="sxs-lookup"><span data-stu-id="af7ae-304">You are signed out before being signed in again, and although you don't need tooenter your credentials again, you see a consent dialog.</span></span>  <span data-ttu-id="af7ae-305">Tuto metodu volejte pouze hello relace tokenu vypršela.</span><span class="sxs-lookup"><span data-stu-id="af7ae-305">Only call this method when hello session token has expired.</span></span>

   <span data-ttu-id="af7ae-306">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-306">**Objective-C**:</span></span>

       #import <Google/SignIn.h>
       // ...
       - (void)authenticate
       {
               [GIDSignIn sharedInstance].uiDelegate = self;
               [[GIDSignIn sharedInstance] signOut];
               [[GIDSignIn sharedInstance] signIn];
        }

   <span data-ttu-id="af7ae-307">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="af7ae-307">**Swift**:</span></span>

       // ...
       func authenticate() {
           GIDSignIn.sharedInstance().uiDelegate = self
           GIDSignIn.sharedInstance().signOut()
           GIDSignIn.sharedInstance().signIn()
       }

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create hello Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data toohello user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize hello client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure Mobile Apps rychlý Start]: app-service-mobile-ios-get-started.md

[Add Mobile Services tooExisting App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts tooauthorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[dynamické schématu]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI toomanage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[řídicí panel infrastruktury]: https://www.fabric.io/home
[prostředků infrastruktury pro iOS – Začínáme]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
