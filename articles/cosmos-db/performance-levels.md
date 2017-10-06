---
title: "úrovně výkonu aaaDocumentDB rozhraní API | Microsoft Docs"
description: "Informace o tom, jak úrovně výkonu DocumentDB API umožňují tooreserve propustnosti na kontejneru na základě."
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
ms.openlocfilehash: 716bc11ae238dbb0feebf004ed8d5f8a7515ec6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="retiring-hello-s1-s2-and-s3-performance-levels"></a>Vyřazení úrovní výkonu S1, S2 a S3 hello

> [!IMPORTANT] 
> Hello S1, S2 a S3 úrovně výkonu popsané v tomto článku se postupně vyřazuje z provozu a nadále již nebudou k dispozici pro nové účty DocumentDB rozhraní API.
>

Tento článek obsahuje přehled úrovní výkonu S1, S2 a S3 a popisuje, jak hello kolekcí, které používají tyto úrovně výkonu budou migrované toosingle oddílu kolekce na 1. srpna 2017. Po přečtení tohoto článku, budete moct tooanswer hello následující otázky:

- [Proč se výkonu S1, S2 a S3 hello úrovně postupně vyřazuje z provozu?](#why-retired)
- [Jak kolekce tvořené jedním oddílem a dělené kolekce srovnání toohello S1, S2, úrovně výkonu S3?](#compare)
- [Co mám dělat potřebovat toodo tooensure bez přerušení přístup k datům toomy?](#uninterrupted-access)
- [Jak se po migraci hello změní mé kolekce?](#collection-change)
- [Jak bude Moje fakturace změnit po jsem migrované toosingle oddílu kolekcí?](#billing-change)
- [Co když je potřeba víc než 10 GB úložiště?](#more-storage-needed)
- [Můžete změnit mezi hello S1, S2 a S3 úrovně výkonu před 1. srpna 2017?](#change-before)
- [Jak poznám, že při migraci mé kolekce?](#when-migrated)
- [Jak migrovat z hello S1, S2, S3 oddílu kolekce toosingle úrovně výkonu na vlastní?](#migrate-diy)
- [Jak se ovlivněny vám EA zákazníka?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-hello-s1-s2-and-s3-performance-levels-being-retired"></a>Proč se výkonu S1, S2 a S3 hello úrovně postupně vyřazuje z provozu?

úrovně výkonu S1, S2 a S3 Hello to není flexibilitu hello nabídka, která kolekce DocumentDB API nabízí. Obě hello propustnost a úložnou kapacitu pomocí hello S1, S2, S3 úrovně výkonu, byly předem nastavené a nenabízí pružnost. Azure Cosmos DB teď nabízí možnost toocustomize hello propustnost a úložiště vám nabízí mnohem větší flexibilitu v vaší tooscale možnost podle potřeby.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-toohello-s1-s2-s3-performance-levels"></a>Jak kolekce tvořené jedním oddílem a dělené kolekce srovnání toohello S1, S2, úrovně výkonu S3?

Hello následující tabulka porovnává hello propustnost a úložiště možnosti dostupné v kolekce tvořené jedním oddílem, dělené kolekce a S1, S2, úrovně výkonu S3. Tady je příklad oblasti USA – východ 2:

|   |Dělené kolekce|Kolekce tvořené jedním oddílem|S1|S2|S3|
|---|---|---|---|---|---|
|Maximální propustnost|Unlimited|10 tis. RU/s|250 RU/s|1 tis. RU/s|2.5 tis. RU/s|
|Minimální propustnost|2.5 tis. RU/s|400 RU/s|250 RU/s|1 tis. RU/s|2.5 tis. RU/s|
|Maximální velikost úložiště|Unlimited|10 GB|10 GB|10 GB|10 GB|
|Cena (každý měsíc)|Propustnost: $6 / 100 RU/s<br><br>Úložiště: $ 0,25/GB|Propustnost: $6 / 100 RU/s<br><br>Úložiště: $ 0,25/GB|25 USD|50 USD|100 USD|

Jste si již EA zákazníka? Pokud ano, najdete v části [jak mě I ovlivněná jsem EA zákazníka?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-toodo-tooensure-uninterrupted-access-toomy-data"></a>Co mám dělat potřebovat toodo tooensure bez přerušení přístup k datům toomy?

Nothing, Cosmos DB zpracovává hello migrace za vás. Pokud máte kolekci S1, S2 nebo S3, bude vaše aktuální kolekci kolekce tvořené jedním oddílem migrované tooa na 31 července 2017. 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-hello-migration"></a>Jak se po migraci hello změní mé kolekce?

Pokud máte kolekci S1, budou migrované tooa kolekce tvořené jedním oddílem s propustností 400 RU/s. 400 RU/s je nejnižší propustnost hello k dispozici kolekce tvořené jedním oddílem. Ale hello náklady v hello kolekce tvořené jedním oddílem je přibližně hello stejně, jako měla platícího s kolekcí vzdálené aplikace S1 400 RU/s a 250 RU/s – tak nejsou platícího pro hello velmi 150 dostupné tooyou RU/s.

Pokud máte kolekci S2, budou migrované tooa kolekce tvořené jedním oddílem s 1 tis. RU/s. Zobrazí se úroveň propustnosti tooyour žádné změny.

Pokud máte kolekci S3, budou migrované tooa kolekce tvořené jedním oddílem s 2,5 tis. RU/s. Zobrazí se úroveň propustnosti tooyour žádné změny.

Ve všech těchto případech po migraci kolekce je budou moct toocustomize odpovídající úrovni vašeho propustnost nebo ho vertikálně a horizontálně škálovat jako potřebné tooprovide přístup s nízkou latencí tooyour uživatele. úroveň propustnosti hello toochange po migraci kolekce, jednoduše v hello portálu Azure otevřete účet Cosmos DB, klikněte na možnost škálování, zvolte kolekce a upravte hello úroveň propustnosti, jak ukazuje následující snímek obrazovky hello:

![Jak hello tooscale propustnost v portálu Azure](./media/performance-levels/portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-toohello-single-partition-collections"></a>Jak bude Moje fakturace změnit po jsem kolekce tvořené jedním oddílem migrované toohello?

Za předpokladu, že budete mít 10 S1 kolekcí, 1 GB úložiště pro každou, v oblasti USA – východ hello, a provedete migraci těchto 10 S1 kolekce too10 kolekce tvořené jedním oddílem v 400 RU za sekundu (hello minimální úroveň). Pokud necháte kolekce hello 10 tvořené jedním oddílem pro úplný měsíc, bude vaše faktura vypadat takto:

![Jak S1 ceny pro 10 kolekce porovná too10 kolekce pomocí ceny pro kolekce tvořené jedním oddílem](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a>Co když je potřeba víc než 10 GB úložiště?

Jestli máte kolekci s úrovní výkonu S1, S2 nebo S3, nebo mít jeden oddíl kolekce, které mají všechny 10 GB úložiště, které jsou k dispozici, můžete použít toomigrate nástroj pro migraci dat DB Cosmos hello vaše data tooa prakticky oddíly kolekci s neomezené úložiště. Informace o výhodách hello dělenou kolekci najdete v tématu [dělení a škálování v Azure Cosmos DB](documentdb-partition-data.md). Informace o toomigrate vaše S1, S2, S3 nebo kolekce tooa rozdělena na oddíly kolekce tvořené jedním oddílem, viz [migraci z kolekce jedním oddílem toopartitioned](documentdb-partition-data.md#migrating-from-single-partition). 

<a name="change-before"></a>

## <a name="can-i-change-between-hello-s1-s2-and-s3-performance-levels-before-august-1-2017"></a>Můžete změnit mezi hello S1, S2 a S3 úrovně výkonu před 1. srpna 2017?

Pouze existující účty s výkonu S1, S2 a S3 bude možné toochange a změnit úroveň úrovně výkonu prostřednictvím portálu hello nebo prostřednictvím kódu programu. 1. srpna 2017 hello S1, S2, a úrovně výkonu S3 nadále již nebudou dostupné. Pokud změníte z kolekce tvořené jedním oddílem tooa S1, S3 nebo S3, nemůže vrátit toohello úrovní výkonu S1, S2 nebo S3.

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a>Jak poznám, že při migraci mé kolekce?

na 31 července 2017 dojde k migraci Hello. Pokud máte kolekci používající hello S1, S2 nebo úrovně výkonu S3, hello Cosmos DB tým vás bude kontaktovat e-mailem před provedením migrace hello. Po migraci hello dokončení na 1. srpna 2017 hello portál Azure vám ukáže, že kolekce používá cenový model Standard.

![Jak tooconfirm kolekce má migrovat toohello standardní cenovou úroveň.](./media/performance-levels/portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-hello-s1-s2-s3-performance-levels-toosingle-partition-collections-on-my-own"></a>Jak migrovat z hello S1, S2, S3 oddílu kolekce toosingle úrovně výkonu na vlastní?

Můžete migrovat z hello S1, S2, a úrovně výkonu S3 toosingle oddílu kolekce pomocí hello portál Azure nebo prostřednictvím kódu programu. To provedete na vlastní před 1. srpna toobenefit z možností flexibilní propustnost hello k dispozici kolekce tvořené jedním oddílem, nebo jsme kolekce můžete migrovat na 31 července 2017.

**toomigrate toosingle oddílu kolekce pomocí hello portálu Azure**

1. V hello [ **portál Azure**](https://portal.azure.com), klikněte na tlačítko **Azure Cosmos DB**, pak vyberte toomodify účet hello Cosmos DB. 
 
    Pokud **Azure Cosmos DB** není na hello vlevo, klikněte na tlačítko >, posuňte se příliš**databáze**, vyberte **Azure Cosmos DB**a potom vyberte účet DocumentDB hello.  

2. V nabídce hello prostředků v části **kontejnery**, klikněte na tlačítko **škálování**hello rozevíracím seznamu vyberte hello kolekce toomodify a pak klikněte na tlačítko **cenová úroveň**. Účty pomocí předem definovaných propustnost mít cenovou úroveň S1, S2 nebo S3.  V hello **zvolte cenovou úroveň** okně klikněte na tlačítko **standardní** toochange definované toouser propustnost a potom klikněte na **vyberte** toosave změny.

    ![Snímek obrazovky okna nastavení hello znázorňující, kde toochange hello hodnotu propustnosti](./media/performance-levels/change-performance-set-thoughput.png)

3. Zpět v hello **škálování** okno, hello **cenová úroveň** mění příliš**standardní** a hello **propustnost (RU/s)** pole se zobrazí se Výchozí hodnota 400. Sada hello propustnost mezi 400 a 10 000 [požadované jednotky](request-units.md)/second (RU/s). Hello **odhadované měsíčních nákladů** dole hello hello stránka aktualizuje automaticky tooprovide odhad hello měsíční náklady. 

    >[!IMPORTANT] 
    > Po uložení změn a přesunout toohello standardní cenovou úroveň, nelze vrátit zpět toohello úrovní výkonu S1, S2 nebo S3.

4. Klikněte na tlačítko **Uložit** toosave změny.

    Pokud zjistíte, že potřebujete další propustnost (větší než 10 000 RU/s) nebo další úložiště (větší než 10GB) můžete vytvořit kolekci oddílů. toomigrate kolekce tooa rozdělena na oddíly kolekce tvořené jedním oddílem, najdete v části [migraci z kolekce jedním oddílem toopartitioned](documentdb-partition-data.md#migrating-from-single-partition).

    > [!NOTE]
    > Změna z S1, S2 nebo S3 tooStandard může trvat až too2 minut.
    > 
    > 

**hello toomigrate toosingle oddílu kolekce pomocí .NET SDK**

Další možností pro změnu úrovně výkonu vaší kolekce je pomocí naší sady SDK. Tato část se vztahuje pouze změna shromažďování výkonu úrovně pomocí našich [DocumentDB .NET API](documentdb-sdk-dotnet.md), ale hello proces je podobný pro naše dalších sadách SDK.

Zde je fragment kódu pro změnu hello kolekce propustnost too5, 000 jednotek žádosti za sekundu:
    
```C#
    //Fetch hello resource toobe updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set hello throughput too5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes toohello database by replacing hello original resource
    await client.ReplaceOfferAsync(offer);
```

Navštivte [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) tooview Další příklady a další informace o našich nabídka metody:

* [**ReadOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [**ReadOffersFeedAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [**ReplaceOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a>Jak se ovlivněny vám EA zákazníka?

Zákazníci EA bude cena chránit až hello konce jejich aktuální kontrakt.

## <a name="next-steps"></a>Další kroky
Další informace o cenách a správa dat s Azure Cosmos DB, toolearn těchto materiálech:

1.  [Segmentace dat v databázi Cosmos](documentdb-partition-data.md). Pochopení hello rozdíl mezi kontejneru tvořené jedním oddílem a oddílů kontejnery, jakož i tipy k rozdělení tooscale strategie implementace bezproblémově.
2.  [Ceny cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/). Další informace o hello náklady na zřizování propustnost a využívání úložiště.
3.  [Požadované jednotky](request-units.md). Pochopení spotřeby hello propustnosti na typy jiné operace, například pro čtení, zápisu, dotazů.
