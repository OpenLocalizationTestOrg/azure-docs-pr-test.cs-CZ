---
title: "Úrovně výkonu DocumentDB API | Microsoft Docs"
description: "Informace o tom, jak úrovně výkonu DocumentDB API umožňují rezervovat propustnosti na kontejneru na základě."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 7dc21c71-47e2-4e06-aa21-e84af52866f4
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c8d4733e57eb760dbb8e8ca96f6ba55671d1742f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="retiring-the-s1-s2-and-s3-performance-levels"></a>Vyřazení úrovní výkonu S1, S2 a S3

> [!IMPORTANT] 
> Úrovně výkonu S1, S2 a S3 popsané v tomto článku se postupně vyřazuje z provozu a nadále již nebudou k dispozici pro nové účty DocumentDB rozhraní API.
>

Tento článek obsahuje přehled úrovní výkonu S1, S2 a S3 a popisuje, jak kolekcí, které používají tyto úrovně výkonu se budou migrovat do kolekce tvořené jedním oddílem na 1. srpna 2017. Po přečtení tohoto článku, budete moct odpovězte si na následující otázky:

- [Proč se výkonu S1, S2 a S3 úrovně postupně vyřazuje z provozu?](#why-retired)
- [Jak kolekce tvořené jedním oddílem a dělené kolekce srovnání na S1, S2, úrovně výkonu S3?](#compare)
- [Co je třeba provést k zajištění nepřetržitého přístupu k mým datům?](#uninterrupted-access)
- [Jak se po dokončení migrace změní mé kolekce?](#collection-change)
- [Jak bude Moje fakturace změnit po I se migrovat do kolekce tvořené jedním oddílem?](#billing-change)
- [Co když je potřeba víc než 10 GB úložiště?](#more-storage-needed)
- [Můžete změnit mezi S1, S2 a S3 úrovně výkonu před 1. srpna 2017?](#change-before)
- [Jak poznám, že při migraci mé kolekce?](#when-migrated)
- [Jak provedu migraci z S1, S2, S3 úrovněmi výkonu kolekce tvořené jedním oddílem na vlastní?](#migrate-diy)
- [Jak se ovlivněny vám EA zákazníka?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a>Proč se výkonu S1, S2 a S3 úrovně postupně vyřazuje z provozu?

Úrovně výkonu S1, S2 a S3 nenabízejí kolekce DocumentDB API nabízí flexibilitu. Kapacita propustnosti i úložiště s S1, S2, úrovně výkonu S3, byly předem nastavené a nenabízí pružnost. Azure Cosmos DB teď nabízí přizpůsobit propustnost a úložiště nabízí mnohem větší flexibilitu v schopnost škálovat podle potřeby.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a>Jak kolekce tvořené jedním oddílem a dělené kolekce srovnání na S1, S2, úrovně výkonu S3?

Následující tabulka porovnává možnosti propustnost a úložiště, které jsou dostupné v kolekce tvořené jedním oddílem, dělené kolekce a S1, S2, úrovně výkonu S3. Tady je příklad oblasti USA – východ 2:

|   |Dělené kolekce|Kolekce tvořené jedním oddílem|S1|S2|S3|
|---|---|---|---|---|---|
|Maximální propustnost|Unlimited|10 tis. RU/s|250 RU/s|1 tis. RU/s|2.5 tis. RU/s|
|Minimální propustnost|2.5 tis. RU/s|400 RU/s|250 RU/s|1 tis. RU/s|2.5 tis. RU/s|
|Maximální velikost úložiště|Unlimited|10 GB|10 GB|10 GB|10 GB|
|Cena (každý měsíc)|Propustnost: $6 / 100 RU/s<br><br>Úložiště: $ 0,25/GB|Propustnost: $6 / 100 RU/s<br><br>Úložiště: $ 0,25/GB|25 USD|50 USD|100 USD|

Jste si již EA zákazníka? Pokud ano, najdete v části [jak mě I ovlivněná jsem EA zákazníka?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a>Co je třeba provést k zajištění nepřetržitého přístupu k mým datům?

Nothing, Cosmos DB zpracovává migrace za vás. Pokud máte kolekci S1, S2 nebo S3, aktuální kolekci budou migrovat do kolekce jednoho oddílu na 31 července 2017. 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a>Jak se po dokončení migrace změní mé kolekce?

Pokud máte kolekci S1, budou přeneseny do kolekce tvořené jedním oddílem s propustností 400 RU/s. 400 RU/s je k dispozici kolekce tvořené jedním oddílem nejnižší propustnost. Ale náklady pro 400 RU/s kolekce tvořené jedním oddílem je přibližně stejné jako byly platícího s S1 kolekce a 250. RU/s – tak nejsou platícího pro velmi 150 RU/s dostupné.

Pokud máte kolekci S2, budou přeneseny do kolekce tvořené jedním oddílem s 1 tis. RU/s. Zobrazí se žádná změna na vaší propustnost úroveň.

Pokud máte kolekci S3, budou přeneseny do kolekce tvořené jedním oddílem s 2,5 tis. RU/s. Zobrazí se žádná změna na vaší propustnost úroveň.

V každém z těchto případech se po migraci kolekce, bude možné přizpůsobit odpovídající úrovni vašeho propustnost, nebo ji škálovat nahoru a dolů tak, aby uživatelům poskytnout přístup s nízkou latencí. Změna úrovně propustnosti po migraci kolekce, jednoduše na portálu Azure otevřete účet Cosmos DB, klikněte na možnost škálování, zvolte kolekce a upravte úroveň propustnosti, jak je znázorněno na následujícím snímku obrazovky:

![Postup škálování propustnosti na portálu Azure](./media/performance-levels/portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-to-the-single-partition-collections"></a>Jak bude Moje fakturace změnit po I se migrovat do kolekce tvořené jedním oddílem?

Za předpokladu, že budete mít 10 S1 kolekcí, 1 GB úložiště pro každou, v oblasti USA – východ a migrace těchto 10 S1 kolekcí do 10 kolekce tvořené jedním oddílem v 400 RU za sekundu (minimální úroveň). Pokud necháte 10 kolekce tvořené jedním oddílem pro úplný měsíc, bude vaše faktura vypadat takto:

![Jak S1 ceny pro 10 kolekce porovnává 10 kolekce pomocí ceny pro kolekce tvořené jedním oddílem](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a>Co když je potřeba víc než 10 GB úložiště?

Jestli máte kolekci s úrovní výkonu S1, S2 nebo S3, nebo mít jeden oddíl kolekce, které mají všechny 10 GB úložiště, které jsou k dispozici, můžete migrovat data do oddílů kolekce s prakticky neomezené úložiště nástroj pro migraci dat DB Cosmos. Informace o výhodách dělenou kolekci najdete v tématu [dělení a škálování v Azure Cosmos DB](documentdb-partition-data.md). Informace o tom, jak migrovat S1, S2, S3 nebo kolekce jednoho oddílu do dělené kolekce najdete v tématu [migrace z jednoho oddílu do dělené kolekce](documentdb-partition-data.md#migrating-from-single-partition). 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-august-1-2017"></a>Můžete změnit mezi S1, S2 a S3 úrovně výkonu před 1. srpna 2017?

Pouze existující účty s výkonu S1, S2 a S3 bude moct změnit a změnit úroveň úrovně výkonu prostřednictvím portálu nebo prostřednictvím kódu programu. 2017 1 srpen úrovní výkonu S1, S2 a S3 bude k dispozici. Pokud změníte z S1, S3 nebo S3 do kolekce tvořené jedním oddílem, nelze vrátit k úrovní výkonu S1, S2 nebo S3.

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a>Jak poznám, že při migraci mé kolekce?

Na 31 července 2017 dojde k migraci. Pokud máte kolekci, která použije S1, S2 nebo S3 úrovně výkonu, Cosmos DB tým vás bude kontaktovat e-mailem před provedením migrace. Po migraci dokončení na 1. srpna 2017 portálu Azure vám ukáže, že vaše kolekce používá cenový model Standard.

![Způsob ověření kolekce byla migrována do cenová úroveň Standard](./media/performance-levels/portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a>Jak provedu migraci z S1, S2, S3 úrovněmi výkonu kolekce tvořené jedním oddílem na vlastní?

Můžete migrovat z úrovní výkonu S1, S2 a S3 kolekce tvořené jedním oddílem pomocí portálu Azure nebo prostřednictvím kódu programu. To provedete sami před srpen 1, abyste mohli využívat výhod propustnost flexibilní možnosti s kolekce tvořené jedním oddílem, nebo jsme kolekce můžete migrovat na 31 července 2017.

**Migrace do kolekce tvořené jedním oddílem pomocí portálu Azure**

1. V [ **portál Azure**](https://portal.azure.com), klikněte na tlačítko **Azure Cosmos DB**, pak vyberte účet Cosmos DB, který chcete upravit. 
 
    Pokud **Azure Cosmos DB** není na panelu vlevo klikněte na tlačítko >, přejděte k položce **databáze**, vyberte **Azure Cosmos DB**a potom vyberte účet DocumentDB.  

2. V nabídce prostředků v části **kontejnery**, klikněte na tlačítko **škálování**, vyberte kolekci, které chcete upravit v rozevíracím seznamu a pak klikněte na tlačítko **cenová úroveň**. Účty pomocí předem definovaných propustnost mít cenovou úroveň S1, S2 nebo S3.  V **zvolte cenovou úroveň** okně klikněte na tlačítko **standardní** změnit na uživatelem definované propustnost, a pak klikněte na **vyberte** uložte změny.

    ![Snímek obrazovky okna nastavení ukazující, kde chcete-li změnit hodnotu propustnosti](./media/performance-levels/change-performance-set-thoughput.png)

3. Zpět v **škálování** okně **cenová úroveň** se změní na **standardní** a **propustnost (RU/s)** pole se zobrazí se výchozí hodnota je 400. Nastavit propustnost mezi 400 a 10 000 [požadované jednotky](request-units.md)/second (RU/s). **Odhadované měsíčních nákladů** v dolní části stránky aktualizace automaticky zajistit odhad měsíční náklady. 

    >[!IMPORTANT] 
    > Po uložení změn a přesunout do cenová úroveň Standard, není možné vrátit zpět na úrovní výkonu S1, S2 nebo S3.

4. Klikněte na tlačítko **Uložit** uložte provedené změny.

    Pokud zjistíte, že potřebujete další propustnost (větší než 10 000 RU/s) nebo další úložiště (větší než 10GB) můžete vytvořit kolekci oddílů. K migraci kolekce tvořené jedním oddílem pro dělenou kolekci, najdete v části [migrace z jednoho oddílu do dělené kolekce](documentdb-partition-data.md#migrating-from-single-partition).

    > [!NOTE]
    > Změna z S1, S2 nebo S3 standardního může trvat až 2 minut.
    > 
    > 

**Migrace do kolekce tvořené jedním oddílem pomocí sady .NET SDK**

Další možností pro změnu úrovně výkonu vaší kolekce je pomocí naší sady SDK. Tato část se vztahuje pouze změna shromažďování výkonu úrovně pomocí našich [DocumentDB .NET API](documentdb-sdk-dotnet.md), ale proces je podobný pro naše dalších sadách SDK.

Zde je fragment kódu pro změnu propustnost kolekce do 5 000 jednotek žádosti za sekundu:
    
```C#
    //Fetch the resource to be updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set the throughput to 5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes to the database by replacing the original resource
    await client.ReplaceOfferAsync(offer);
```

Navštivte [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) zobrazení další příklady a další informace o našich nabídka metody:

* [**ReadOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [**ReadOffersFeedAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [**ReplaceOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a>Jak se ovlivněny vám EA zákazníka?

Zákazníci EA bude cena chránit až do konce jejich aktuální kontrakt.

## <a name="next-steps"></a>Další kroky
Další informace o cenách a správě dat pomocí Azure Cosmos DB najdete v těchto zdrojích:

1.  [Segmentace dat v databázi Cosmos](documentdb-partition-data.md). Vysvětlení rozdílu kontejneru tvořené jedním oddílem a oddílů kontejnery, jakož i tipy k implementaci strategie dělení bezproblémově škálování.
2.  [Ceny cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/). Další informace o náklady na zřizování propustnost a využívání úložiště.
3.  [Požadované jednotky](request-units.md). Pochopení spotřeby propustnosti na typy jiné operace, například pro čtení, zápisu, dotazů.
