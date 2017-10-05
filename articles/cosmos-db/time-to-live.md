---
title: "Vypršení platnosti dat v Azure Cosmos DB s Hodnota time to live | Microsoft Docs"
description: "Microsoft Azure Cosmos DB s TTL, poskytuje schopnost dokumenty z systém automaticky vymazány po určitou dobu."
services: cosmos-db
documentationcenter: 
keywords: Hodnota Time to live
author: arramac
manager: jhubbard
editor: 
ms.assetid: 25fcbbda-71f7-414a-bf57-d8671358ca3f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: arramac
ms.openlocfilehash: 6f1c43ca0113dc7579b0fc3743d3314c16ce78a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="expire-data-in-azure-cosmos-db-collections-automatically-with-time-to-live"></a><span data-ttu-id="82501-104">Vypršení platnosti dat v kolekcích Azure Cosmos DB automaticky s Hodnota time to live</span><span class="sxs-lookup"><span data-stu-id="82501-104">Expire data in Azure Cosmos DB collections automatically with time to live</span></span>
<span data-ttu-id="82501-105">Aplikace můžete vytvářet a ukládat velká množství dat.</span><span class="sxs-lookup"><span data-stu-id="82501-105">Applications can produce and store vast amounts of data.</span></span> <span data-ttu-id="82501-106">Některé z těchto dat, jako například počítač generuje data, protokoly a uživatelské relace události, které informace jsou užitečné pouze omezenou dobu.</span><span class="sxs-lookup"><span data-stu-id="82501-106">Some of this data, like machine generated event data, logs, and user session information is only useful for a finite period of time.</span></span> <span data-ttu-id="82501-107">Jakmile se změní na data přebytečných potřebám aplikace je bezpečné mazání tato data a snížit požadavkům na ukládání aplikace.</span><span class="sxs-lookup"><span data-stu-id="82501-107">Once the data becomes surplus to the needs of the application it is safe to purge this data and reduce the storage needs of an application.</span></span>

<span data-ttu-id="82501-108">Microsoft Azure Cosmos DB s "time to live" nebo TTL, poskytuje schopnost dokumenty automaticky odstraněna z databáze po určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="82501-108">With "time to live" or TTL, Microsoft Azure Cosmos DB provides the ability to have documents automatically purged from the database after a period of time.</span></span> <span data-ttu-id="82501-109">Výchozí doba TTL můžete nastavit na úrovni kolekce a přepsán pro každý dokument.</span><span class="sxs-lookup"><span data-stu-id="82501-109">The default time to live can be set at the collection level, and overridden on a per-document basis.</span></span> <span data-ttu-id="82501-110">Jakmile je nastavena hodnota TTL, jako výchozí kolekci nebo na úrovni dokumentu Cosmos DB automaticky odebere dokumenty, které existují po uplynutí této doby čas v sekundách, protože poslední změny.</span><span class="sxs-lookup"><span data-stu-id="82501-110">Once TTL is set, either as a collection default or at a document level, Cosmos DB will automatically remove documents that exist after that period of time, in seconds, since they were last modified.</span></span>

<span data-ttu-id="82501-111">Hodnota Time to live v Cosmos DB používá posun vůči při poslední změny dokumentu.</span><span class="sxs-lookup"><span data-stu-id="82501-111">Time to live in Cosmos DB uses an offset against when the document was last modified.</span></span> <span data-ttu-id="82501-112">K tomu použije `_ts` pole, která již existuje u každé dokumentu.</span><span class="sxs-lookup"><span data-stu-id="82501-112">To do this it uses the `_ts` field which exists on every document.</span></span> <span data-ttu-id="82501-113">Pole _ts je časové razítko epoch stylu systému unix představující datum a čas.</span><span class="sxs-lookup"><span data-stu-id="82501-113">The _ts field is a unix-style epoch timestamp representing the date and time.</span></span> <span data-ttu-id="82501-114">`_ts` Pole je aktualizováno pokaždé, když je změněn dokument.</span><span class="sxs-lookup"><span data-stu-id="82501-114">The `_ts` field is updated every time a document is modified.</span></span> 

## <a name="ttl-behavior"></a><span data-ttu-id="82501-115">Hodnota TTL chování</span><span class="sxs-lookup"><span data-stu-id="82501-115">TTL behavior</span></span>
<span data-ttu-id="82501-116">Hodnota TTL funkce je řízena vlastnostmi TTL na dvou úrovních - úrovni kolekce a na úrovni dokumentu.</span><span class="sxs-lookup"><span data-stu-id="82501-116">The TTL feature is controlled by TTL properties at two levels - the collection level and the document level.</span></span> <span data-ttu-id="82501-117">Hodnoty jsou nastavené v sekundách a jsou považovány za rozdílů z `_ts` posledního dokumentu modifikace.</span><span class="sxs-lookup"><span data-stu-id="82501-117">The values are set in seconds and are treated as a delta from the `_ts` that the document was last modified at.</span></span>

1. <span data-ttu-id="82501-118">DefaultTTL pro kolekci</span><span class="sxs-lookup"><span data-stu-id="82501-118">DefaultTTL for the collection</span></span>
   
   * <span data-ttu-id="82501-119">Pokud chybí (nebo nastaveno na hodnotu null), dokumenty nejsou automaticky odstraněny.</span><span class="sxs-lookup"><span data-stu-id="82501-119">If missing (or set to null), documents are not deleted automatically.</span></span>
   * <span data-ttu-id="82501-120">Pokud je přítomen a hodnota je "-1" = nekonečné – dokumenty nevyprší ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="82501-120">If present and the value is "-1" = infinite – documents don’t expire by default</span></span>
   * <span data-ttu-id="82501-121">Pokud je přítomen a hodnota je některé číslo ("n") – dokumenty vyprší "n" sekund po poslední změny</span><span class="sxs-lookup"><span data-stu-id="82501-121">If present and the value is some number ("n") – documents expire "n” seconds after last modification</span></span>
2. <span data-ttu-id="82501-122">Hodnota TTL pro dokumenty:</span><span class="sxs-lookup"><span data-stu-id="82501-122">TTL for the documents:</span></span> 
   
   * <span data-ttu-id="82501-123">Vlastnost se vztahuje pouze v případě DefaultTTL je k dispozici pro její nadřazená kolekce.</span><span class="sxs-lookup"><span data-stu-id="82501-123">Property is applicable only if DefaultTTL is present for the parent collection.</span></span>
   * <span data-ttu-id="82501-124">Přepíše hodnotu DefaultTTL pro její nadřazená kolekce.</span><span class="sxs-lookup"><span data-stu-id="82501-124">Overrides the DefaultTTL value for the parent collection.</span></span>

<span data-ttu-id="82501-125">Jakmile vypršela platnost dokumentu (`ttl`  +  `_ts` > = aktuální čas serveru), dokument je označena jako "prošlé".</span><span class="sxs-lookup"><span data-stu-id="82501-125">As soon as the document has expired (`ttl` + `_ts` >= current server time), the document is marked as "expired”.</span></span> <span data-ttu-id="82501-126">Žádná operace bude možné na těchto dokumentech po této době a jejich budou vyloučeny z výsledků všechny dotazy provést.</span><span class="sxs-lookup"><span data-stu-id="82501-126">No operation will be allowed on these documents after this time and they will be excluded from the results of any queries performed.</span></span> <span data-ttu-id="82501-127">Dokumenty jsou fyzicky odstraněn v systému, včetně jsou na pozadí tj později.</span><span class="sxs-lookup"><span data-stu-id="82501-127">The documents are physically deleted in the system, and are deleted in the background opportunistically at a later time.</span></span> <span data-ttu-id="82501-128">Není to využívat žádné [jednotky žádosti (ruština)](request-units.md) z rozpočtu kolekce.</span><span class="sxs-lookup"><span data-stu-id="82501-128">This does not consume any [Request Units (RUs)](request-units.md) from the collection budget.</span></span>

<span data-ttu-id="82501-129">Výše uvedené logiku lze zobrazit v matici následující:</span><span class="sxs-lookup"><span data-stu-id="82501-129">The above logic can be shown in the following matrix:</span></span>

|  | <span data-ttu-id="82501-130">DefaultTTL chybí nebo není nastaven na kolekci</span><span class="sxs-lookup"><span data-stu-id="82501-130">DefaultTTL missing/not set on the collection</span></span> | <span data-ttu-id="82501-131">DefaultTTL = -1 na kolekci</span><span class="sxs-lookup"><span data-stu-id="82501-131">DefaultTTL = -1 on collection</span></span> | <span data-ttu-id="82501-132">DefaultTTL = "n" na kolekci</span><span class="sxs-lookup"><span data-stu-id="82501-132">DefaultTTL = "n" on collection</span></span> |
| --- |:--- |:--- |:--- |
| <span data-ttu-id="82501-133">Hodnota TTL chybí v dokumentu</span><span class="sxs-lookup"><span data-stu-id="82501-133">TTL Missing on document</span></span> |<span data-ttu-id="82501-134">Nic k přepsání na úrovni dokumentu vzhledem k tomu, že dokument i kolekce nemají žádný koncept TTL.</span><span class="sxs-lookup"><span data-stu-id="82501-134">Nothing to override at document level since both the document and collection have no concept of TTL.</span></span> |<span data-ttu-id="82501-135">Vypršení platnosti žádné dokumenty v této kolekci.</span><span class="sxs-lookup"><span data-stu-id="82501-135">No documents in this collection will expire.</span></span> |<span data-ttu-id="82501-136">Dokumenty v této kolekci vyprší po uplynutí intervalu n.</span><span class="sxs-lookup"><span data-stu-id="82501-136">The documents in this collection will expire when interval n elapses.</span></span> |
| <span data-ttu-id="82501-137">Hodnota TTL = -1 v dokumentu</span><span class="sxs-lookup"><span data-stu-id="82501-137">TTL = -1 on document</span></span> |<span data-ttu-id="82501-138">Nic k přepsání na úrovni dokumentu od kolekce nedefinuje vlastnost DefaultTTL, který můžete přepsat dokumentu.</span><span class="sxs-lookup"><span data-stu-id="82501-138">Nothing to override at the document level since the collection doesn’t define the DefaultTTL property that a document can override.</span></span> <span data-ttu-id="82501-139">Hodnota TTL na dokument je zrušení interpretovaný v systému.</span><span class="sxs-lookup"><span data-stu-id="82501-139">TTL on a document is un-interpreted by the system.</span></span> |<span data-ttu-id="82501-140">Vypršení platnosti žádné dokumenty v této kolekci.</span><span class="sxs-lookup"><span data-stu-id="82501-140">No documents in this collection will expire.</span></span> |<span data-ttu-id="82501-141">Dokument s TTL =-1 v této kolekci nikdy nevyprší.</span><span class="sxs-lookup"><span data-stu-id="82501-141">The document with TTL=-1 in this collection will never expire.</span></span> <span data-ttu-id="82501-142">Všechny ostatní dokumenty vyprší po intervalu "n".</span><span class="sxs-lookup"><span data-stu-id="82501-142">All other documents will expire after "n" interval.</span></span> |
| <span data-ttu-id="82501-143">Hodnota TTL = n dokumentu</span><span class="sxs-lookup"><span data-stu-id="82501-143">TTL = n on document</span></span> |<span data-ttu-id="82501-144">Nic k přepsání na úrovni dokumentu.</span><span class="sxs-lookup"><span data-stu-id="82501-144">Nothing to override at the document level.</span></span> <span data-ttu-id="82501-145">Hodnota TTL na dokument v zrušení interpretovaný v systému.</span><span class="sxs-lookup"><span data-stu-id="82501-145">TTL on a document in un-interpreted by the system.</span></span> |<span data-ttu-id="82501-146">Dokument s TTL = n vyprší po n interval v sekundách.</span><span class="sxs-lookup"><span data-stu-id="82501-146">The document with TTL = n will expire after interval n, in seconds.</span></span> <span data-ttu-id="82501-147">Další dokumenty budou dědit interval-1 a jeho platnost nikdy nevypršela.</span><span class="sxs-lookup"><span data-stu-id="82501-147">Other documents will inherit interval of -1 and never expire.</span></span> |<span data-ttu-id="82501-148">Dokument s TTL = n vyprší po n interval v sekundách.</span><span class="sxs-lookup"><span data-stu-id="82501-148">The document with TTL = n will expire after interval n, in seconds.</span></span> <span data-ttu-id="82501-149">Další dokumenty zdědí "n" interval z kolekce.</span><span class="sxs-lookup"><span data-stu-id="82501-149">Other documents will inherit "n" interval from the collection.</span></span> |

## <a name="configuring-ttl"></a><span data-ttu-id="82501-150">Konfigurace TTL</span><span class="sxs-lookup"><span data-stu-id="82501-150">Configuring TTL</span></span>
<span data-ttu-id="82501-151">Ve výchozím nastavení je hodnota time to live zakázané ve výchozím nastavení ve všech kolekcích Cosmos DB a na všechny dokumenty.</span><span class="sxs-lookup"><span data-stu-id="82501-151">By default, time to live is disabled by default in all Cosmos DB collections and on all documents.</span></span>

## <a name="enabling-ttl"></a><span data-ttu-id="82501-152">Povolení TTL</span><span class="sxs-lookup"><span data-stu-id="82501-152">Enabling TTL</span></span>
<span data-ttu-id="82501-153">Chcete-li povolit TTL na kolekci nebo dokumenty v rámci kolekce, nastavte vlastnost DefaultTTL kolekce na -1 nebo nenulovou hodnotou kladné číslo.</span><span class="sxs-lookup"><span data-stu-id="82501-153">To enable TTL on a collection, or the documents within a collection, you need to set the DefaultTTL property of a collection to either -1 or a non-zero positive number.</span></span> <span data-ttu-id="82501-154">Nastavení DefaultTTL-1 znamená, který ve výchozím nastavení se všechny dokumenty v kolekci bude navždy za provozu, ale služba Cosmos DB měli sledovat tuto kolekci pro dokumenty, které mají přepsat toto výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="82501-154">Setting the DefaultTTL to -1 means that by default all documents in the collection will live forever but the Cosmos DB service should monitor this collection for documents that have overridden this default.</span></span>

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a><span data-ttu-id="82501-155">Konfigurace výchozí hodnota TTL na kolekci</span><span class="sxs-lookup"><span data-stu-id="82501-155">Configuring default TTL on a collection</span></span>
<span data-ttu-id="82501-156">Budete moci konfigurovat výchozí čas TTL na úrovni kolekce.</span><span class="sxs-lookup"><span data-stu-id="82501-156">You are able to configure a default time to live at a collection level.</span></span> <span data-ttu-id="82501-157">Pokud chcete nastavit interval TTL, ZÍSKÁ na kolekci, je třeba zadat nenulovou hodnotou kladné číslo určující časový interval v sekundách, po poslední úpravy časové razítko dokumentu vyprší všechny dokumenty v kolekci (`_ts`).</span><span class="sxs-lookup"><span data-stu-id="82501-157">To set the TTL on a collection, you need to provide a non-zero positive number that indicates the period, in seconds, to expire all documents in the collection after the last modified timestamp of the document (`_ts`).</span></span> <span data-ttu-id="82501-158">Nebo můžete nastavit výchozí hodnoty-1, což znamená, že bude ve výchozím nastavení po neomezenou dobu live všechny dokumenty v vložit do kolekce.</span><span class="sxs-lookup"><span data-stu-id="82501-158">Or, you can set the default to -1, which implies that all documents inserted in to the collection will live indefinitely by default.</span></span>

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a><span data-ttu-id="82501-159">Nastavení TTL v dokumentu</span><span class="sxs-lookup"><span data-stu-id="82501-159">Setting TTL on a document</span></span>
<span data-ttu-id="82501-160">Kromě nastavení výchozí hodnota TTL na kolekci můžete nastavit konkrétní TTL na úrovni dokumentu.</span><span class="sxs-lookup"><span data-stu-id="82501-160">In addition to setting a default TTL on a collection you can set specific TTL at a document level.</span></span> <span data-ttu-id="82501-161">Tím se přepíše výchozí kolekce.</span><span class="sxs-lookup"><span data-stu-id="82501-161">Doing this will override the default of the collection.</span></span>

* <span data-ttu-id="82501-162">Pokud chcete nastavit interval TTL, ZÍSKÁ v dokumentu, je třeba zadat nenulové kladné číslo, který určuje časový interval v sekundách, po kterou vyprší dokumentu po poslední úpravy časové razítko dokumentu (`_ts`).</span><span class="sxs-lookup"><span data-stu-id="82501-162">To set the TTL on a document, you need to provide a non-zero positive number which indicates the period, in seconds, to expire the document after the last modified timestamp of the document (`_ts`).</span></span>
* <span data-ttu-id="82501-163">Pokud dokument neobsahuje žádné pole TTL, bude použita výchozí hodnota kolekce.</span><span class="sxs-lookup"><span data-stu-id="82501-163">If a document has no TTL field, then the default of the collection will apply.</span></span>
* <span data-ttu-id="82501-164">Pokud je hodnota TTL vypnutá na úrovni kolekce, pole TTL v dokumentu budou ignorovány, dokud hodnota TTL je povoleno znovu na kolekci.</span><span class="sxs-lookup"><span data-stu-id="82501-164">If TTL is disabled at the collection level, the TTL field on the document will be ignored until TTL is enabled again on the collection.</span></span>

<span data-ttu-id="82501-165">Zde je fragment ukazuje, jak nastavit dobu platnosti TTL na dokumentu:</span><span class="sxs-lookup"><span data-stu-id="82501-165">Here's a snippet showing how to set the TTL expiration on a document:</span></span>

    // Include a property that serializes to "ttl" in JSON
    public class SalesOrder
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        
        [JsonProperty(PropertyName="cid")]
        public string CustomerId { get; set; }
        
        // used to set expiration policy
        [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
        public int? TimeToLive { get; set; }
        
        //...
    }
    
    // Set the value to the expiration in seconds
    SalesOrder salesOrder = new SalesOrder
    {
        Id = "SO05",
        CustomerId = "CO18009186470",
        TimeToLive = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days 
    };


## <a name="extending-ttl-on-an-existing-document"></a><span data-ttu-id="82501-166">Rozšíření TTL na stávající dokument</span><span class="sxs-lookup"><span data-stu-id="82501-166">Extending TTL on an existing document</span></span>
<span data-ttu-id="82501-167">Hodnota TTL dokumentu můžete resetovat pomocí tohoto postupu všechny operace zápisu v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="82501-167">You can reset the TTL on a document by doing any write operation on the document.</span></span> <span data-ttu-id="82501-168">To bude tato hodnota nastavena `_ts` aktuální čas a začne odpočítávat čas do vypršení platnosti dokumentu, jak pomocí `ttl`, bude spuštěno znovu.</span><span class="sxs-lookup"><span data-stu-id="82501-168">Doing this will set the `_ts` to the current time, and the countdown to the document expiry, as set by the `ttl`, will begin again.</span></span> <span data-ttu-id="82501-169">Pokud chcete změnit `ttl` dokumentu, můžete aktualizovat pole, jako je tomu s jiné nastavit pole.</span><span class="sxs-lookup"><span data-stu-id="82501-169">If you wish to change the `ttl` of a document, you can update the field as you can do with any other settable field.</span></span>

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time to live
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="removing-ttl-from-a-document"></a><span data-ttu-id="82501-170">Odebrání TTL z dokumentu</span><span class="sxs-lookup"><span data-stu-id="82501-170">Removing TTL from a document</span></span>
<span data-ttu-id="82501-171">Pokud hodnotu TTL byla nastavena na dokument a již nechcete vypršení platnosti tohoto dokumentu, pak můžete načtení dokumentu, odeberte pole TTL a nahraďte dokumentů na serveru.</span><span class="sxs-lookup"><span data-stu-id="82501-171">If a TTL has been set on a document and you no longer want that document to expire, then you can retrieve the document, remove the TTL field and replace the document on the server.</span></span> <span data-ttu-id="82501-172">Pokud pole TTL je odebrán z dokumentu, použijí se výchozí kolekce.</span><span class="sxs-lookup"><span data-stu-id="82501-172">When the TTL field is removed from the document, the default of the collection will be applied.</span></span> <span data-ttu-id="82501-173">Zastavení dokumentu z vypršení platnosti a není dědit z kolekce bude nutné nastavit hodnotu TTL na hodnotu -1.</span><span class="sxs-lookup"><span data-stu-id="82501-173">To stop a document from expiring and not inherit from the collection then you need to set the TTL value to -1.</span></span>

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit the default TTL of the collection
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="disabling-ttl"></a><span data-ttu-id="82501-174">Zakázání TTL</span><span class="sxs-lookup"><span data-stu-id="82501-174">Disabling TTL</span></span>
<span data-ttu-id="82501-175">Měla by být odstraněna zakázat TTL zcela na kolekci a ukončit proces na pozadí z vyhledávání pro dokumenty s vypršenou platností DefaultTTL vlastnost v kolekci.</span><span class="sxs-lookup"><span data-stu-id="82501-175">To disable TTL entirely on a collection and stop the background process from looking for expired documents the DefaultTTL property on the collection should be deleted.</span></span> <span data-ttu-id="82501-176">Odstraněním této vlastnosti se liší od nastavení na hodnotu -1.</span><span class="sxs-lookup"><span data-stu-id="82501-176">Deleting this property is different from setting it to -1.</span></span> <span data-ttu-id="82501-177">Nastavení pro nové dokumenty-1 znamená přidat do kolekce bude navždy za provozu, ale je možné přepsat na konkrétní dokumenty v kolekci.</span><span class="sxs-lookup"><span data-stu-id="82501-177">Setting to -1 means new documents added to the collection will live forever but you can override this on specific documents in the collection.</span></span> <span data-ttu-id="82501-178">Odebrání tato vlastnost zcela z kolekce znamená, že žádné dokumenty vyprší, i když jsou dokumenty, které mají explicitně přepsat předchozí výchozí.</span><span class="sxs-lookup"><span data-stu-id="82501-178">Removing this property entirely from the collection means that no documents will expire, even if there are documents that have explicitly overridden a previous default.</span></span>

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);


## <a name="faq"></a><span data-ttu-id="82501-179">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="82501-179">FAQ</span></span>
<span data-ttu-id="82501-180">**Co se TTL náklady mi?**</span><span class="sxs-lookup"><span data-stu-id="82501-180">**What will TTL cost me?**</span></span>

<span data-ttu-id="82501-181">Není k dispozici bez dalších nákladů na dokument nastavení hodnotu TTL.</span><span class="sxs-lookup"><span data-stu-id="82501-181">There is no additional cost to setting a TTL on a document.</span></span>

<span data-ttu-id="82501-182">**Jak dlouho bude trvat až po hodnotu TTL odstranění dokumentu?**</span><span class="sxs-lookup"><span data-stu-id="82501-182">**How long will it take to delete my document once the TTL is up?**</span></span>

<span data-ttu-id="82501-183">Dokumenty jsou platnost vypršela ihned, jakmile interval TTL, ZÍSKÁ zapnutý a nebudou přístupné prostřednictvím CRUD nebo dotaz, rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="82501-183">The documents are expired immediately once the TTL is up, and will not be accessible via CRUD or query APIs.</span></span> 

<span data-ttu-id="82501-184">**Bude TTL na dokument mít žádný vliv na RU poplatky?**</span><span class="sxs-lookup"><span data-stu-id="82501-184">**Will TTL on a document have any impact on RU charges?**</span></span>

<span data-ttu-id="82501-185">Ne, bude mít žádný vliv na RU poplatky za odstranění dokumenty s vypršenou platností prostřednictvím TTL v Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="82501-185">No, there will be no impact on RU charges for deletions of expired documents via TTL in Cosmos DB.</span></span>

<span data-ttu-id="82501-186">**Funkci TTL se vztahují pouze na celý dokumenty, nebo může vypršet hodnoty vlastností pro jednotlivý dokument?**</span><span class="sxs-lookup"><span data-stu-id="82501-186">**Does the TTL feature only apply to entire documents, or can I expire individual document property values?**</span></span>

<span data-ttu-id="82501-187">Hodnota TTL se vztahuje na celý dokument.</span><span class="sxs-lookup"><span data-stu-id="82501-187">TTL applies to the entire document.</span></span> <span data-ttu-id="82501-188">Pokud chcete vyprší pouze část dokumentu, pak se doporučuje rozbalte část z hlavní dokumentu v samostatných "propojené" dokumentu a pak použít TTL na tomto extrahované dokumentu.</span><span class="sxs-lookup"><span data-stu-id="82501-188">If you would like to expire just a portion of a document, then it is recommended that you extract the portion from the main document in to a separate "linked” document and then use TTL on that extracted document.</span></span>

<span data-ttu-id="82501-189">**Má funkci TTL žádné zvláštní požadavky indexování?**</span><span class="sxs-lookup"><span data-stu-id="82501-189">**Does the TTL feature have any specific indexing requirements?**</span></span>

<span data-ttu-id="82501-190">Ano.</span><span class="sxs-lookup"><span data-stu-id="82501-190">Yes.</span></span> <span data-ttu-id="82501-191">Kolekce musí mít [indexování sady zásad](indexing-policies.md) konzistentní nebo Lazy.</span><span class="sxs-lookup"><span data-stu-id="82501-191">The collection must have [indexing policy set](indexing-policies.md) to either Consistent or Lazy.</span></span> <span data-ttu-id="82501-192">Při pokusu o nastavení DefaultTTL kolekce s indexování nastaven na žádný způsobí chybu, protože se pokouší vypnout indexování na kolekce, která má DefaultTTL, již nastaven.</span><span class="sxs-lookup"><span data-stu-id="82501-192">Trying to set DefaultTTL on a collection with indexing set to None will result in an error, as will trying to turn off indexing on a collection that has a DefaultTTL already set.</span></span>

## <a name="next-steps"></a><span data-ttu-id="82501-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="82501-193">Next steps</span></span>
<span data-ttu-id="82501-194">Další informace o databázi Cosmos Azure, najdete v tématu službu [ *dokumentace* ](https://azure.microsoft.com/documentation/services/cosmos-db/) stránky.</span><span class="sxs-lookup"><span data-stu-id="82501-194">To learn more about Azure Cosmos DB, refer to the service [*documentation*](https://azure.microsoft.com/documentation/services/cosmos-db/) page.</span></span>

