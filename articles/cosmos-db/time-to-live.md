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
# <a name="expire-data-in-azure-cosmos-db-collections-automatically-with-time-toolive"></a>Vypršení platnosti dat v kolekcích Azure Cosmos DB automaticky s časem toolive
Aplikace můžete vytvářet a ukládat velká množství dat. Některé z těchto dat, jako například počítač generuje data, protokoly a uživatelské relace události, které informace jsou užitečné pouze omezenou dobu. Jakmile hello data bude přebytečných toohello potřebám aplikace hello je bezpečné toopurge tato data a snížit požadavky na úložiště hello aplikace.

"Doba toolive" nebo TTL Microsoft Azure Cosmos DB poskytuje hello možnost toohave dokumenty automaticky odstraněna z databáze hello po určitou dobu. Výchozí doba Hello toolive můžete nastavit na úrovni kolekce hello a přepsán pro každý dokument. Jakmile je nastavena hodnota TTL, jako výchozí kolekci nebo na úrovni dokumentu Cosmos DB automaticky odebere dokumenty, které existují po uplynutí této doby čas v sekundách, protože poslední změny.

Čas toolive v Cosmos DB používá posun vůči při poslední změny dokumentu hello. toodo toto používá hello `_ts` pole, která již existuje u každé dokumentu. pole _ts Hello je stylu systému unix epoch časové razítko představující hello datum a čas. Hello `_ts` pole je aktualizováno pokaždé, když je změněn dokument. 

## <a name="ttl-behavior"></a>Hodnota TTL chování
Hello TTL funkce je řízena vlastnostmi TTL na dvou úrovních - hello kolekce úroveň a hello dokumentu. Hello hodnoty jsou nastaveny v sekundách a jsou považovány za rozdílový z hello `_ts` dokument hello bylo naposledy změněno na.

1. DefaultTTL pro kolekci hello
   
   * Pokud chybí (nebo set toonull), dokumenty nejsou automaticky odstraněny.
   * Pokud je k dispozici a hello hodnota je "-1" = nekonečné – dokumenty nevyprší ve výchozím nastavení
   * Pokud je k dispozici a hodnota hello je některé číslo ("n") – dokumenty vyprší "n" sekund po poslední změny
2. Hodnota TTL pro dokumenty hello: 
   
   * Vlastnost se vztahuje pouze v případě, že je k dispozici pro kolekce nadřazených hello DefaultTTL.
   * Přepíše hodnotu hello DefaultTTL pro kolekce nadřazených hello.

Jakmile hello dokumentu vypršela (`ttl`  +  `_ts` > = aktuální čas serveru), hello dokumentu je označena jako "prošlé". Žádná operace bude možné na těchto dokumentech po této době a jejich budou vyloučeny z hello výsledky všechny dotazy provést. dokumenty Hello se fyzicky odstraní hello systému a jsou odstraněny hello pozadí tj později. Není to využívat žádné [jednotky žádosti (ruština)](request-units.md) z rozpočtu kolekce hello.

Hello výše logiku lze zobrazit v hello matice následující:

|  | DefaultTTL chybí nebo není nastavení kolekce hello | DefaultTTL = -1 na kolekci | DefaultTTL = "n" na kolekci |
| --- |:--- |:--- |:--- |
| Hodnota TTL chybí v dokumentu |Nic toooverride na úrovni dokumentu vzhledem k tomu, že dokument hello a kolekce nemají žádný koncept TTL. |Vypršení platnosti žádné dokumenty v této kolekci. |Hello dokumenty v této kolekci vyprší po uplynutí intervalu n. |
| Hodnota TTL = -1 v dokumentu |Nic toooverride na úrovni dokumentu hello vzhledem k tomu, že kolekce hello nedefinuje hello DefaultTTL vlastnost, která můžete přepsat dokumentu. Hodnota TTL na dokument je zrušení interpretovaný hello systému. |Vypršení platnosti žádné dokumenty v této kolekci. |Hello dokument s TTL =-1 v této kolekci nikdy nevyprší. Všechny ostatní dokumenty vyprší po intervalu "n". |
| Hodnota TTL = n dokumentu |Nic toooverride na úrovni dokumentu hello. Hodnota TTL na dokument v zrušení interpretovaný hello systému. |Hello dokument s TTL = n vyprší po n interval v sekundách. Další dokumenty budou dědit interval-1 a jeho platnost nikdy nevypršela. |Hello dokument s TTL = n vyprší po n interval v sekundách. Další dokumenty zdědí "n" interval z kolekce hello. |

## <a name="configuring-ttl"></a>Konfigurace TTL
Ve výchozím nastavení je čas toolive zakázané ve výchozím nastavení ve všech kolekcích Cosmos DB a na všechny dokumenty.

## <a name="enabling-ttl"></a>Povolení TTL
tooenable TTL na kolekci nebo hello dokumenty v kolekci, je nutné tooset hello DefaultTTL vlastnost kolekce tooeither -1 nebo nenulovou hodnotou kladné číslo. Nastavení hello DefaultTTL příliš-1 znamená, že ve výchozím nastavení budou všechny dokumenty v kolekci hello live navždy ale hello Cosmos DB služby měli sledovat tuto kolekci pro dokumenty, které mají přepsat toto výchozí nastavení.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a>Konfigurace výchozí hodnota TTL na kolekci
Možnost tooconfigure jste na úrovni kolekce toolive výchozí dobu. tooset hello TTL na kolekci, je nutné tooprovide nenulovou hodnotou kladné číslo určující hello limit v sekundách, tooexpire všechny dokumenty v kolekci hello po hello poslední změny časové razítko hello dokumentu (`_ts`). Nebo můžete nastavit hello výchozí příliš-1, což znamená, že bude ve výchozím nastavení po neomezenou dobu live všechny dokumenty v kolekci toohello vložit.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a>Nastavení TTL v dokumentu
Kromě toho toosetting výchozí hodnota TTL na kolekci můžete nastavit konkrétní TTL na úrovni dokumentu. Tím se přepíše výchozí hello hello kolekce.

* tooset hello TTL na dokument, musíte tooprovide nenulovou hodnotou kladné číslo, které označuje hello období, v sekundách, tooexpire hello dokumentu po hello poslední změny časové razítko hello dokumentu (`_ts`).
* Pokud dokument neobsahuje žádné pole TTL, pak hello výchozí hello kolekce budou platit.
* Pokud hodnota TTL je zakázána na úrovni kolekce hello, pole TTL hello hello dokumentu budou ignorovány, dokud hodnota TTL je povoleno znovu na kolekci hello.

Zde je fragment ukazuje, jak tooset hello TTL vypršení platnosti v dokumentu:

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


## <a name="extending-ttl-on-an-existing-document"></a>Rozšíření TTL na stávající dokument
Můžete resetovat hello TTL na dokument pomocí tohoto postupu všechny operace zápisu u dokumentu hello. Tato funkce bude nastavena hello `_ts` toohello aktuální čas a hello odpočítávání toohello dokumentů vypršení platnosti, jak pomocí hello `ttl`, bude spuštěno znovu. Pokud chcete toochange hello `ttl` dokumentu, můžete aktualizovat hello pole, jako je tomu s jiné nastavit pole.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time toolive
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="removing-ttl-from-a-document"></a>Odebrání TTL z dokumentu
Pokud hodnotu TTL byla nastavena na dokument a již nechcete tooexpire tohoto dokumentu, potom můžete načíst hello dokumentu, odebrat hello TTL pole a nahraďte hello dokumentů na serveru hello. Pokud pole TTL hello je odebrán z dokumentu hello, použijí se výchozí hello hello kolekce. toostop dokumentu z vypršení platnosti a není dědí hello kolekce, pak je nutné tooset hello TTL hodnota příliš-1.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit hello default TTL of hello collection
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="disabling-ttl"></a>Zakázání TTL
toodisable TTL zcela na soubor a ukončete hello pozadí zpracovat z vyhledávání pro dokumenty s vypršenou platností, které hello DefaultTTL vlastnost v kolekci hello měla by být odstraněna. Odstraněním této vlastnosti se liší od jeho nastavení příliš-1. Nastavení příliš-1 znamená nové dokumenty přidat toohello kolekce bude navždy za provozu, ale je možné přepsat na konkrétní dokumenty v kolekci hello. Odebrání tato vlastnost zcela z kolekce hello znamená, že žádné dokumenty vyprší, i když jsou dokumenty, které mají explicitně přepsat předchozí výchozí.

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);


## <a name="faq"></a>Nejčastější dotazy
**Co se TTL náklady mi?**

Není k dispozici žádné další náklady toosetting hodnotu TTL na dokumentu.

**Jak dlouho trvá toodelete dokumentu až po hello TTL?**

Hello dokumenty jsou vypršela ihned, jakmile hello TTL zapnutý a nebudou přístupné prostřednictvím CRUD nebo dotaz, rozhraní API. 

**Bude TTL na dokument mít žádný vliv na RU poplatky?**

Ne, bude mít žádný vliv na RU poplatky za odstranění dokumenty s vypršenou platností prostřednictvím TTL v Cosmos DB.

**Funkce TTL hello se projeví pouze v tooentire dokumenty, nebo může vypršet hodnoty vlastností pro jednotlivý dokument?**

Hodnota TTL platí toohello celý dokument. Pokud chcete tooexpire se doporučuje jenom části dokumentu a potom rozbalte část hello z hlavní dokumentu hello v samostatných "propojené" dokumentu tooa a pak použít TTL na tomto extrahované dokumentu.

**Má funkce TTL hello žádné zvláštní požadavky indexování?**

Ano. Hello kolekce musí mít [indexování sady zásad](indexing-policies.md) tooeither konzistentní nebo Lazy. Při tooset DefaultTTL na kolekci s indexování sadu tooNone způsobí chybu, jak se snažíme tooturn vypnout indexování na kolekce, která má DefaultTTL, již nastaven.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o databázi Cosmos Azure, najdete v toohello služby [ *dokumentace* ](https://azure.microsoft.com/documentation/services/cosmos-db/) stránky.

