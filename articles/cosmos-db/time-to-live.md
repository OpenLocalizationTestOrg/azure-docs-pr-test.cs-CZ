---
title: "aaaExpire data v Azure Cosmos DB s časem toolive | Microsoft Docs"
description: "S TTL poskytuje Microsoft Azure Cosmos DB hello možnost toohave dokumenty z hello systému automaticky vymazány po určitou dobu."
services: cosmos-db
documentationcenter: 
keywords: "čas toolive"
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
ms.openlocfilehash: 51d8ec46add72c9624457316a4ccd1e23fb83ad0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expire-data-in-azure-cosmos-db-collections-automatically-with-time-toolive"></a><span data-ttu-id="5a537-104">Vypršení platnosti dat v kolekcích Azure Cosmos DB automaticky s časem toolive</span><span class="sxs-lookup"><span data-stu-id="5a537-104">Expire data in Azure Cosmos DB collections automatically with time toolive</span></span>
<span data-ttu-id="5a537-105">Aplikace můžete vytvářet a ukládat velká množství dat.</span><span class="sxs-lookup"><span data-stu-id="5a537-105">Applications can produce and store vast amounts of data.</span></span> <span data-ttu-id="5a537-106">Některé z těchto dat, jako například počítač generuje data, protokoly a uživatelské relace události, které informace jsou užitečné pouze omezenou dobu.</span><span class="sxs-lookup"><span data-stu-id="5a537-106">Some of this data, like machine generated event data, logs, and user session information is only useful for a finite period of time.</span></span> <span data-ttu-id="5a537-107">Jakmile hello data bude přebytečných toohello potřebám aplikace hello je bezpečné toopurge tato data a snížit požadavky na úložiště hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5a537-107">Once hello data becomes surplus toohello needs of hello application it is safe toopurge this data and reduce hello storage needs of an application.</span></span>

<span data-ttu-id="5a537-108">"Doba toolive" nebo TTL Microsoft Azure Cosmos DB poskytuje hello možnost toohave dokumenty automaticky odstraněna z databáze hello po určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="5a537-108">With "time toolive" or TTL, Microsoft Azure Cosmos DB provides hello ability toohave documents automatically purged from hello database after a period of time.</span></span> <span data-ttu-id="5a537-109">Výchozí doba Hello toolive můžete nastavit na úrovni kolekce hello a přepsán pro každý dokument.</span><span class="sxs-lookup"><span data-stu-id="5a537-109">hello default time toolive can be set at hello collection level, and overridden on a per-document basis.</span></span> <span data-ttu-id="5a537-110">Jakmile je nastavena hodnota TTL, jako výchozí kolekci nebo na úrovni dokumentu Cosmos DB automaticky odebere dokumenty, které existují po uplynutí této doby čas v sekundách, protože poslední změny.</span><span class="sxs-lookup"><span data-stu-id="5a537-110">Once TTL is set, either as a collection default or at a document level, Cosmos DB will automatically remove documents that exist after that period of time, in seconds, since they were last modified.</span></span>

<span data-ttu-id="5a537-111">Čas toolive v Cosmos DB používá posun vůči při poslední změny dokumentu hello.</span><span class="sxs-lookup"><span data-stu-id="5a537-111">Time toolive in Cosmos DB uses an offset against when hello document was last modified.</span></span> <span data-ttu-id="5a537-112">toodo toto používá hello `_ts` pole, která již existuje u každé dokumentu.</span><span class="sxs-lookup"><span data-stu-id="5a537-112">toodo this it uses hello `_ts` field which exists on every document.</span></span> <span data-ttu-id="5a537-113">pole _ts Hello je stylu systému unix epoch časové razítko představující hello datum a čas.</span><span class="sxs-lookup"><span data-stu-id="5a537-113">hello _ts field is a unix-style epoch timestamp representing hello date and time.</span></span> <span data-ttu-id="5a537-114">Hello `_ts` pole je aktualizováno pokaždé, když je změněn dokument.</span><span class="sxs-lookup"><span data-stu-id="5a537-114">hello `_ts` field is updated every time a document is modified.</span></span> 

## <a name="ttl-behavior"></a><span data-ttu-id="5a537-115">Hodnota TTL chování</span><span class="sxs-lookup"><span data-stu-id="5a537-115">TTL behavior</span></span>
<span data-ttu-id="5a537-116">Hello TTL funkce je řízena vlastnostmi TTL na dvou úrovních - hello kolekce úroveň a hello dokumentu.</span><span class="sxs-lookup"><span data-stu-id="5a537-116">hello TTL feature is controlled by TTL properties at two levels - hello collection level and hello document level.</span></span> <span data-ttu-id="5a537-117">Hello hodnoty jsou nastaveny v sekundách a jsou považovány za rozdílový z hello `_ts` dokument hello bylo naposledy změněno na.</span><span class="sxs-lookup"><span data-stu-id="5a537-117">hello values are set in seconds and are treated as a delta from hello `_ts` that hello document was last modified at.</span></span>

1. <span data-ttu-id="5a537-118">DefaultTTL pro kolekci hello</span><span class="sxs-lookup"><span data-stu-id="5a537-118">DefaultTTL for hello collection</span></span>
   
   * <span data-ttu-id="5a537-119">Pokud chybí (nebo set toonull), dokumenty nejsou automaticky odstraněny.</span><span class="sxs-lookup"><span data-stu-id="5a537-119">If missing (or set toonull), documents are not deleted automatically.</span></span>
   * <span data-ttu-id="5a537-120">Pokud je k dispozici a hello hodnota je "-1" = nekonečné – dokumenty nevyprší ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="5a537-120">If present and hello value is "-1" = infinite – documents don’t expire by default</span></span>
   * <span data-ttu-id="5a537-121">Pokud je k dispozici a hodnota hello je některé číslo ("n") – dokumenty vyprší "n" sekund po poslední změny</span><span class="sxs-lookup"><span data-stu-id="5a537-121">If present and hello value is some number ("n") – documents expire "n” seconds after last modification</span></span>
2. <span data-ttu-id="5a537-122">Hodnota TTL pro dokumenty hello:</span><span class="sxs-lookup"><span data-stu-id="5a537-122">TTL for hello documents:</span></span> 
   
   * <span data-ttu-id="5a537-123">Vlastnost se vztahuje pouze v případě, že je k dispozici pro kolekce nadřazených hello DefaultTTL.</span><span class="sxs-lookup"><span data-stu-id="5a537-123">Property is applicable only if DefaultTTL is present for hello parent collection.</span></span>
   * <span data-ttu-id="5a537-124">Přepíše hodnotu hello DefaultTTL pro kolekce nadřazených hello.</span><span class="sxs-lookup"><span data-stu-id="5a537-124">Overrides hello DefaultTTL value for hello parent collection.</span></span>

<span data-ttu-id="5a537-125">Jakmile hello dokumentu vypršela (`ttl`  +  `_ts` > = aktuální čas serveru), hello dokumentu je označena jako "prošlé".</span><span class="sxs-lookup"><span data-stu-id="5a537-125">As soon as hello document has expired (`ttl` + `_ts` >= current server time), hello document is marked as "expired”.</span></span> <span data-ttu-id="5a537-126">Žádná operace bude možné na těchto dokumentech po této době a jejich budou vyloučeny z hello výsledky všechny dotazy provést.</span><span class="sxs-lookup"><span data-stu-id="5a537-126">No operation will be allowed on these documents after this time and they will be excluded from hello results of any queries performed.</span></span> <span data-ttu-id="5a537-127">dokumenty Hello se fyzicky odstraní hello systému a jsou odstraněny hello pozadí tj později.</span><span class="sxs-lookup"><span data-stu-id="5a537-127">hello documents are physically deleted in hello system, and are deleted in hello background opportunistically at a later time.</span></span> <span data-ttu-id="5a537-128">Není to využívat žádné [jednotky žádosti (ruština)](request-units.md) z rozpočtu kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="5a537-128">This does not consume any [Request Units (RUs)](request-units.md) from hello collection budget.</span></span>

<span data-ttu-id="5a537-129">Hello výše logiku lze zobrazit v hello matice následující:</span><span class="sxs-lookup"><span data-stu-id="5a537-129">hello above logic can be shown in hello following matrix:</span></span>

|  | <span data-ttu-id="5a537-130">DefaultTTL chybí nebo není nastavení kolekce hello</span><span class="sxs-lookup"><span data-stu-id="5a537-130">DefaultTTL missing/not set on hello collection</span></span> | <span data-ttu-id="5a537-131">DefaultTTL = -1 na kolekci</span><span class="sxs-lookup"><span data-stu-id="5a537-131">DefaultTTL = -1 on collection</span></span> | <span data-ttu-id="5a537-132">DefaultTTL = "n" na kolekci</span><span class="sxs-lookup"><span data-stu-id="5a537-132">DefaultTTL = "n" on collection</span></span> |
| --- |:--- |:--- |:--- |
| <span data-ttu-id="5a537-133">Hodnota TTL chybí v dokumentu</span><span class="sxs-lookup"><span data-stu-id="5a537-133">TTL Missing on document</span></span> |<span data-ttu-id="5a537-134">Nic toooverride na úrovni dokumentu vzhledem k tomu, že dokument hello a kolekce nemají žádný koncept TTL.</span><span class="sxs-lookup"><span data-stu-id="5a537-134">Nothing toooverride at document level since both hello document and collection have no concept of TTL.</span></span> |<span data-ttu-id="5a537-135">Vypršení platnosti žádné dokumenty v této kolekci.</span><span class="sxs-lookup"><span data-stu-id="5a537-135">No documents in this collection will expire.</span></span> |<span data-ttu-id="5a537-136">Hello dokumenty v této kolekci vyprší po uplynutí intervalu n.</span><span class="sxs-lookup"><span data-stu-id="5a537-136">hello documents in this collection will expire when interval n elapses.</span></span> |
| <span data-ttu-id="5a537-137">Hodnota TTL = -1 v dokumentu</span><span class="sxs-lookup"><span data-stu-id="5a537-137">TTL = -1 on document</span></span> |<span data-ttu-id="5a537-138">Nic toooverride na úrovni dokumentu hello vzhledem k tomu, že kolekce hello nedefinuje hello DefaultTTL vlastnost, která můžete přepsat dokumentu.</span><span class="sxs-lookup"><span data-stu-id="5a537-138">Nothing toooverride at hello document level since hello collection doesn’t define hello DefaultTTL property that a document can override.</span></span> <span data-ttu-id="5a537-139">Hodnota TTL na dokument je zrušení interpretovaný hello systému.</span><span class="sxs-lookup"><span data-stu-id="5a537-139">TTL on a document is un-interpreted by hello system.</span></span> |<span data-ttu-id="5a537-140">Vypršení platnosti žádné dokumenty v této kolekci.</span><span class="sxs-lookup"><span data-stu-id="5a537-140">No documents in this collection will expire.</span></span> |<span data-ttu-id="5a537-141">Hello dokument s TTL =-1 v této kolekci nikdy nevyprší.</span><span class="sxs-lookup"><span data-stu-id="5a537-141">hello document with TTL=-1 in this collection will never expire.</span></span> <span data-ttu-id="5a537-142">Všechny ostatní dokumenty vyprší po intervalu "n".</span><span class="sxs-lookup"><span data-stu-id="5a537-142">All other documents will expire after "n" interval.</span></span> |
| <span data-ttu-id="5a537-143">Hodnota TTL = n dokumentu</span><span class="sxs-lookup"><span data-stu-id="5a537-143">TTL = n on document</span></span> |<span data-ttu-id="5a537-144">Nic toooverride na úrovni dokumentu hello.</span><span class="sxs-lookup"><span data-stu-id="5a537-144">Nothing toooverride at hello document level.</span></span> <span data-ttu-id="5a537-145">Hodnota TTL na dokument v zrušení interpretovaný hello systému.</span><span class="sxs-lookup"><span data-stu-id="5a537-145">TTL on a document in un-interpreted by hello system.</span></span> |<span data-ttu-id="5a537-146">Hello dokument s TTL = n vyprší po n interval v sekundách.</span><span class="sxs-lookup"><span data-stu-id="5a537-146">hello document with TTL = n will expire after interval n, in seconds.</span></span> <span data-ttu-id="5a537-147">Další dokumenty budou dědit interval-1 a jeho platnost nikdy nevypršela.</span><span class="sxs-lookup"><span data-stu-id="5a537-147">Other documents will inherit interval of -1 and never expire.</span></span> |<span data-ttu-id="5a537-148">Hello dokument s TTL = n vyprší po n interval v sekundách.</span><span class="sxs-lookup"><span data-stu-id="5a537-148">hello document with TTL = n will expire after interval n, in seconds.</span></span> <span data-ttu-id="5a537-149">Další dokumenty zdědí "n" interval z kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="5a537-149">Other documents will inherit "n" interval from hello collection.</span></span> |

## <a name="configuring-ttl"></a><span data-ttu-id="5a537-150">Konfigurace TTL</span><span class="sxs-lookup"><span data-stu-id="5a537-150">Configuring TTL</span></span>
<span data-ttu-id="5a537-151">Ve výchozím nastavení je čas toolive zakázané ve výchozím nastavení ve všech kolekcích Cosmos DB a na všechny dokumenty.</span><span class="sxs-lookup"><span data-stu-id="5a537-151">By default, time toolive is disabled by default in all Cosmos DB collections and on all documents.</span></span>

## <a name="enabling-ttl"></a><span data-ttu-id="5a537-152">Povolení TTL</span><span class="sxs-lookup"><span data-stu-id="5a537-152">Enabling TTL</span></span>
<span data-ttu-id="5a537-153">tooenable TTL na kolekci nebo hello dokumenty v kolekci, je nutné tooset hello DefaultTTL vlastnost kolekce tooeither -1 nebo nenulovou hodnotou kladné číslo.</span><span class="sxs-lookup"><span data-stu-id="5a537-153">tooenable TTL on a collection, or hello documents within a collection, you need tooset hello DefaultTTL property of a collection tooeither -1 or a non-zero positive number.</span></span> <span data-ttu-id="5a537-154">Nastavení hello DefaultTTL příliš-1 znamená, že ve výchozím nastavení budou všechny dokumenty v kolekci hello live navždy ale hello Cosmos DB služby měli sledovat tuto kolekci pro dokumenty, které mají přepsat toto výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="5a537-154">Setting hello DefaultTTL too-1 means that by default all documents in hello collection will live forever but hello Cosmos DB service should monitor this collection for documents that have overridden this default.</span></span>

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a><span data-ttu-id="5a537-155">Konfigurace výchozí hodnota TTL na kolekci</span><span class="sxs-lookup"><span data-stu-id="5a537-155">Configuring default TTL on a collection</span></span>
<span data-ttu-id="5a537-156">Možnost tooconfigure jste na úrovni kolekce toolive výchozí dobu.</span><span class="sxs-lookup"><span data-stu-id="5a537-156">You are able tooconfigure a default time toolive at a collection level.</span></span> <span data-ttu-id="5a537-157">tooset hello TTL na kolekci, je nutné tooprovide nenulovou hodnotou kladné číslo určující hello limit v sekundách, tooexpire všechny dokumenty v kolekci hello po hello poslední změny časové razítko hello dokumentu (`_ts`).</span><span class="sxs-lookup"><span data-stu-id="5a537-157">tooset hello TTL on a collection, you need tooprovide a non-zero positive number that indicates hello period, in seconds, tooexpire all documents in hello collection after hello last modified timestamp of hello document (`_ts`).</span></span> <span data-ttu-id="5a537-158">Nebo můžete nastavit hello výchozí příliš-1, což znamená, že bude ve výchozím nastavení po neomezenou dobu live všechny dokumenty v kolekci toohello vložit.</span><span class="sxs-lookup"><span data-stu-id="5a537-158">Or, you can set hello default too-1, which implies that all documents inserted in toohello collection will live indefinitely by default.</span></span>

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a><span data-ttu-id="5a537-159">Nastavení TTL v dokumentu</span><span class="sxs-lookup"><span data-stu-id="5a537-159">Setting TTL on a document</span></span>
<span data-ttu-id="5a537-160">Kromě toho toosetting výchozí hodnota TTL na kolekci můžete nastavit konkrétní TTL na úrovni dokumentu.</span><span class="sxs-lookup"><span data-stu-id="5a537-160">In addition toosetting a default TTL on a collection you can set specific TTL at a document level.</span></span> <span data-ttu-id="5a537-161">Tím se přepíše výchozí hello hello kolekce.</span><span class="sxs-lookup"><span data-stu-id="5a537-161">Doing this will override hello default of hello collection.</span></span>

* <span data-ttu-id="5a537-162">tooset hello TTL na dokument, musíte tooprovide nenulovou hodnotou kladné číslo, které označuje hello období, v sekundách, tooexpire hello dokumentu po hello poslední změny časové razítko hello dokumentu (`_ts`).</span><span class="sxs-lookup"><span data-stu-id="5a537-162">tooset hello TTL on a document, you need tooprovide a non-zero positive number which indicates hello period, in seconds, tooexpire hello document after hello last modified timestamp of hello document (`_ts`).</span></span>
* <span data-ttu-id="5a537-163">Pokud dokument neobsahuje žádné pole TTL, pak hello výchozí hello kolekce budou platit.</span><span class="sxs-lookup"><span data-stu-id="5a537-163">If a document has no TTL field, then hello default of hello collection will apply.</span></span>
* <span data-ttu-id="5a537-164">Pokud hodnota TTL je zakázána na úrovni kolekce hello, pole TTL hello hello dokumentu budou ignorovány, dokud hodnota TTL je povoleno znovu na kolekci hello.</span><span class="sxs-lookup"><span data-stu-id="5a537-164">If TTL is disabled at hello collection level, hello TTL field on hello document will be ignored until TTL is enabled again on hello collection.</span></span>

<span data-ttu-id="5a537-165">Zde je fragment ukazuje, jak tooset hello TTL vypršení platnosti v dokumentu:</span><span class="sxs-lookup"><span data-stu-id="5a537-165">Here's a snippet showing how tooset hello TTL expiration on a document:</span></span>

    // Include a property that serializes too"ttl" in JSON
    public class SalesOrder
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        
        [JsonProperty(PropertyName="cid")]
        public string CustomerId { get; set; }
        
        // used tooset expiration policy
        [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
        public int? TimeToLive { get; set; }
        
        //...
    }
    
    // Set hello value toohello expiration in seconds
    SalesOrder salesOrder = new SalesOrder
    {
        Id = "SO05",
        CustomerId = "CO18009186470",
        TimeToLive = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days 
    };


## <a name="extending-ttl-on-an-existing-document"></a><span data-ttu-id="5a537-166">Rozšíření TTL na stávající dokument</span><span class="sxs-lookup"><span data-stu-id="5a537-166">Extending TTL on an existing document</span></span>
<span data-ttu-id="5a537-167">Můžete resetovat hello TTL na dokument pomocí tohoto postupu všechny operace zápisu u dokumentu hello.</span><span class="sxs-lookup"><span data-stu-id="5a537-167">You can reset hello TTL on a document by doing any write operation on hello document.</span></span> <span data-ttu-id="5a537-168">Tato funkce bude nastavena hello `_ts` toohello aktuální čas a hello odpočítávání toohello dokumentů vypršení platnosti, jak pomocí hello `ttl`, bude spuštěno znovu.</span><span class="sxs-lookup"><span data-stu-id="5a537-168">Doing this will set hello `_ts` toohello current time, and hello countdown toohello document expiry, as set by hello `ttl`, will begin again.</span></span> <span data-ttu-id="5a537-169">Pokud chcete toochange hello `ttl` dokumentu, můžete aktualizovat hello pole, jako je tomu s jiné nastavit pole.</span><span class="sxs-lookup"><span data-stu-id="5a537-169">If you wish toochange hello `ttl` of a document, you can update hello field as you can do with any other settable field.</span></span>

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time toolive
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="removing-ttl-from-a-document"></a><span data-ttu-id="5a537-170">Odebrání TTL z dokumentu</span><span class="sxs-lookup"><span data-stu-id="5a537-170">Removing TTL from a document</span></span>
<span data-ttu-id="5a537-171">Pokud hodnotu TTL byla nastavena na dokument a již nechcete tooexpire tohoto dokumentu, potom můžete načíst hello dokumentu, odebrat hello TTL pole a nahraďte hello dokumentů na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="5a537-171">If a TTL has been set on a document and you no longer want that document tooexpire, then you can retrieve hello document, remove hello TTL field and replace hello document on hello server.</span></span> <span data-ttu-id="5a537-172">Pokud pole TTL hello je odebrán z dokumentu hello, použijí se výchozí hello hello kolekce.</span><span class="sxs-lookup"><span data-stu-id="5a537-172">When hello TTL field is removed from hello document, hello default of hello collection will be applied.</span></span> <span data-ttu-id="5a537-173">toostop dokumentu z vypršení platnosti a není dědí hello kolekce, pak je nutné tooset hello TTL hodnota příliš-1.</span><span class="sxs-lookup"><span data-stu-id="5a537-173">toostop a document from expiring and not inherit from hello collection then you need tooset hello TTL value too-1.</span></span>

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit hello default TTL of hello collection
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="disabling-ttl"></a><span data-ttu-id="5a537-174">Zakázání TTL</span><span class="sxs-lookup"><span data-stu-id="5a537-174">Disabling TTL</span></span>
<span data-ttu-id="5a537-175">toodisable TTL zcela na soubor a ukončete hello pozadí zpracovat z vyhledávání pro dokumenty s vypršenou platností, které hello DefaultTTL vlastnost v kolekci hello měla by být odstraněna.</span><span class="sxs-lookup"><span data-stu-id="5a537-175">toodisable TTL entirely on a collection and stop hello background process from looking for expired documents hello DefaultTTL property on hello collection should be deleted.</span></span> <span data-ttu-id="5a537-176">Odstraněním této vlastnosti se liší od jeho nastavení příliš-1.</span><span class="sxs-lookup"><span data-stu-id="5a537-176">Deleting this property is different from setting it too-1.</span></span> <span data-ttu-id="5a537-177">Nastavení příliš-1 znamená nové dokumenty přidat toohello kolekce bude navždy za provozu, ale je možné přepsat na konkrétní dokumenty v kolekci hello.</span><span class="sxs-lookup"><span data-stu-id="5a537-177">Setting too-1 means new documents added toohello collection will live forever but you can override this on specific documents in hello collection.</span></span> <span data-ttu-id="5a537-178">Odebrání tato vlastnost zcela z kolekce hello znamená, že žádné dokumenty vyprší, i když jsou dokumenty, které mají explicitně přepsat předchozí výchozí.</span><span class="sxs-lookup"><span data-stu-id="5a537-178">Removing this property entirely from hello collection means that no documents will expire, even if there are documents that have explicitly overridden a previous default.</span></span>

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);


## <a name="faq"></a><span data-ttu-id="5a537-179">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="5a537-179">FAQ</span></span>
<span data-ttu-id="5a537-180">**Co se TTL náklady mi?**</span><span class="sxs-lookup"><span data-stu-id="5a537-180">**What will TTL cost me?**</span></span>

<span data-ttu-id="5a537-181">Není k dispozici žádné další náklady toosetting hodnotu TTL na dokumentu.</span><span class="sxs-lookup"><span data-stu-id="5a537-181">There is no additional cost toosetting a TTL on a document.</span></span>

<span data-ttu-id="5a537-182">**Jak dlouho trvá toodelete dokumentu až po hello TTL?**</span><span class="sxs-lookup"><span data-stu-id="5a537-182">**How long will it take toodelete my document once hello TTL is up?**</span></span>

<span data-ttu-id="5a537-183">Hello dokumenty jsou vypršela ihned, jakmile hello TTL zapnutý a nebudou přístupné prostřednictvím CRUD nebo dotaz, rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="5a537-183">hello documents are expired immediately once hello TTL is up, and will not be accessible via CRUD or query APIs.</span></span> 

<span data-ttu-id="5a537-184">**Bude TTL na dokument mít žádný vliv na RU poplatky?**</span><span class="sxs-lookup"><span data-stu-id="5a537-184">**Will TTL on a document have any impact on RU charges?**</span></span>

<span data-ttu-id="5a537-185">Ne, bude mít žádný vliv na RU poplatky za odstranění dokumenty s vypršenou platností prostřednictvím TTL v Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5a537-185">No, there will be no impact on RU charges for deletions of expired documents via TTL in Cosmos DB.</span></span>

<span data-ttu-id="5a537-186">**Funkce TTL hello se projeví pouze v tooentire dokumenty, nebo může vypršet hodnoty vlastností pro jednotlivý dokument?**</span><span class="sxs-lookup"><span data-stu-id="5a537-186">**Does hello TTL feature only apply tooentire documents, or can I expire individual document property values?**</span></span>

<span data-ttu-id="5a537-187">Hodnota TTL platí toohello celý dokument.</span><span class="sxs-lookup"><span data-stu-id="5a537-187">TTL applies toohello entire document.</span></span> <span data-ttu-id="5a537-188">Pokud chcete tooexpire se doporučuje jenom části dokumentu a potom rozbalte část hello z hlavní dokumentu hello v samostatných "propojené" dokumentu tooa a pak použít TTL na tomto extrahované dokumentu.</span><span class="sxs-lookup"><span data-stu-id="5a537-188">If you would like tooexpire just a portion of a document, then it is recommended that you extract hello portion from hello main document in tooa separate "linked” document and then use TTL on that extracted document.</span></span>

<span data-ttu-id="5a537-189">**Má funkce TTL hello žádné zvláštní požadavky indexování?**</span><span class="sxs-lookup"><span data-stu-id="5a537-189">**Does hello TTL feature have any specific indexing requirements?**</span></span>

<span data-ttu-id="5a537-190">Ano.</span><span class="sxs-lookup"><span data-stu-id="5a537-190">Yes.</span></span> <span data-ttu-id="5a537-191">Hello kolekce musí mít [indexování sady zásad](indexing-policies.md) tooeither konzistentní nebo Lazy.</span><span class="sxs-lookup"><span data-stu-id="5a537-191">hello collection must have [indexing policy set](indexing-policies.md) tooeither Consistent or Lazy.</span></span> <span data-ttu-id="5a537-192">Při tooset DefaultTTL na kolekci s indexování sadu tooNone způsobí chybu, jak se snažíme tooturn vypnout indexování na kolekce, která má DefaultTTL, již nastaven.</span><span class="sxs-lookup"><span data-stu-id="5a537-192">Trying tooset DefaultTTL on a collection with indexing set tooNone will result in an error, as will trying tooturn off indexing on a collection that has a DefaultTTL already set.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a537-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a537-193">Next steps</span></span>
<span data-ttu-id="5a537-194">toolearn Další informace o databázi Cosmos Azure, najdete v toohello služby [ *dokumentace* ](https://azure.microsoft.com/documentation/services/cosmos-db/) stránky.</span><span class="sxs-lookup"><span data-stu-id="5a537-194">toolearn more about Azure Cosmos DB, refer toohello service [*documentation*](https://azure.microsoft.com/documentation/services/cosmos-db/) page.</span></span>

