---
title: "Úvod tooAzure Cosmos DB: rozhraní API pro MongoDB | Microsoft Docs"
description: "Zjistěte, jak můžete použít Azure Cosmos DB toostore a dotaz ohromné objemy dokumentů JSON s nízkou latencí pomocí hello oblíbených rozhraní API operačních systémů MongoDB."
keywords: Co je MongoDB
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 4afaf40d-c560-42e0-83b4-a64d94671f0a
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: anhoh
ms.openlocfilehash: 1eb88014cc4809332e3f5c109a44a55814194934
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-cosmos-db-api-for-mongodb"></a>Úvod tooAzure Cosmos DB: rozhraní API pro MongoDB

[Azure Cosmos DB](../cosmos-db/introduction.md) je globálně distribuovaná databázová služba Microsoftu s více modely pro klíčové aplikace. Poskytuje Azure Cosmos DB [globální distribuční klíč](distribute-data-globally.md), [elastické škálování propustnost a úložiště](partition-data.md) po celém světě, jednociferné milisekundu latence v hello 99th percentilu, [pět dobře definované úrovně konzistence](consistency-levels.md)a zaručit vysoká dostupnost, všechny zálohovány pomocí [špičkový SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB [automaticky indexuje data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) aniž byste si toodeal se správou schéma a index. Zahrnuje více modelů a podporuje modely dokumentů, klíčových hodnot, grafů a sloupcových dat. 

![Azure Cosmos DB: MongoDB rozhraní API](./media/mongodb-introduction/cosmosdb-mongodb.png) 

Cosmos DB databáze můžete použít jako úložiště dat hello pro aplikací napsaných pro [MongoDB](https://docs.mongodb.com/manual/introduction/). To znamená, že pomocí stávající [ovladače](https://docs.mongodb.org/ecosystem/drivers/), vaše aplikace napsané pro MongoDB teď můžete komunikovat s Cosmos DB a používat Cosmos DB databáze místo databáze MongoDB. V mnoha případech můžete přepínat pomocí MongoDB tooCosmos DB jednoduše změnou připojovací řetězec. Pomocí této funkce lze snadno vytvářet a spouštět aplikace databázi MongoDB v hello Azure cloud s globální distribuční databázi Cosmos Azure a [komplexní špičkové SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db), přitom dál toouse známé dovedností a nástrojů pro MongoDB.


## <a name="what-is-hello-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>Co je hello výhodou používání Azure Cosmos DB pro MongoDB aplikace?

**Elasticky škálovatelná propustnost a úložiště:** snadno škálovat nahoru i dolů vaší toomeet databázi MongoDB aplikace potřebuje. Data se ukládají na discích SSD (solid-state drive), které nabízí nízkou a předvídatelnou latenci. Cosmos DB podporuje MongoDB kolekce, které je možné škálovat toovirtually neomezené velikosti úložiště a zřízené propustnosti. Je možné Elasticky škálovat Cosmos DB s předvídatelným výkonem bezproblémově růstem vaší aplikace. 

**Replikace více oblast:** Cosmos DB transparentně replikuje vaše datové oblasti tooall jste spojené s vaším účtem MongoDB, takže se budete toodevelop aplikace, které vyžadují toodata globální přístup při současném poskytování kompromisy mezi konzistence, dostupnosti a výkonu, všechny s odpovídající záruky. Cosmos DB poskytuje transparentní regionální převzetí služeb při selhání s více funkci rozhraní API a hello možnost tooelastically škálování propustnost a úložiště v rámci hello zeměkouli. Další informace v [distribuci dat globálně](distribute-data-globally.md).

**Kompatibilita MongoDB**: můžete použít existující MongoDB znalosti, kód aplikace a nástrojů. Můžete vyvíjet aplikace, které používají MongoDB a nasadit je tooproduction pomocí hello plně spravované služby globálně distribuované Cosmos DB.

**Žádný server správy**: nemáte toomanage a škálovat vaše databáze MongoDB. Cosmos DB je plně spravovaná služba, což znamená, že nemáte toomanage všechny infrastruktury nebo virtuální počítače sami. Je k dispozici v 30 + cosmos DB [oblasti Azure](https://azure.microsoft.com/regions/services/).

**Přizpůsobitelné úrovně konzistence:** vyberte jednu z pěti dobře definované konzistence úrovně tooachieve optimálního poměru mezi konzistencí a výkonem. Pro dotazy a operace čtení Cosmos DB nabízí pět úrovně konzistence: silnou, s ohraničenou odolností, založenou relace, konzistentní Předpona a případnou. Tyto úrovně konzistence podrobné, dobře definované povolit toomake zvolit vhodný poměr mezi konzistencí, dostupností a latencí. Další informace v [pomocí konzistence úrovně toomaximize dostupnosti a výkonu](consistency-levels.md).

**Automatické indexování**: ve výchozím nastavení, Cosmos DB automaticky indexuje všechny vlastnosti hello v rámci dokumenty ve vaší databázi MongoDB a nepodporuje očekávat ani nevyžaduje žádné schéma nebo vytváření sekundárních indexů.

**Podnikové úrovni** -Azure Cosmos DB podporuje více místní repliky toodeliver 99,99 % dostupnost a ochranu dat hello stěně místní a regionální selhání. Azure Cosmos DB má podnikové úrovni [dodržování předpisů certifikace](https://www.microsoft.com/trustcenter) a funkce zabezpečení. 

Další informace v této Azure videa s Scott Hanselman a Azure Cosmos DB hlavní inženýrství manažer Kirill Gavrylyuk pátek.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Introducing-Azure-Cosmos-DB/player]
> 

## <a name="how-tooget-started"></a>Způsob spuštění tooget

Postupujte podle hello MongoDB – elementy QuickStart toocreate účet Cosmos DB a migrovat vaše stávající aplikace toouse Mongo DB Cosmos DB nebo vytvořit nový:

* [Migrovat stávající webovou aplikaci Node.js MongoDB](create-mongodb-nodejs.md).
* [Vytvoření webové aplikace MongoDB API pomocí rozhraní .NET a hello portálu Azure](create-mongodb-dotnet.md)
* [Sestavení rozhraní API MongoDB konzolovou aplikaci Java a hello portálu Azure](create-mongodb-java.md)

## <a name="next-steps"></a>Další kroky

Informace o rozhraní API MongoDB Azure Cosmos DB je integrována do hello celkové Azure Cosmos DB dokumentaci, ale tady je několik tooget ukazatele, kterou jste zahájili:

* Postupujte podle hello [připojení tooa MongoDB účet](connect-mongodb-account.md) kurz toolearn jak tooget informací o vašem účtu připojovací řetězec.
* Postupujte podle hello [MongoChef použití s Azure Cosmos DB](mongodb-mongochef.md) kurz toolearn jak toocreate připojení mezi Azure Cosmos DB databáze a MongoDB aplikace v MongoChef.
* Postupujte podle hello [migrovat data tooAzure Cosmos DB s podporou protokolů pro MongoDB](mongodb-migrate.md) kurz tooimport data tooan rozhraní API pro databázi MongoDB.
* Připojit tooan rozhraní API pro použití účtu MongoDB [Robomongo](mongodb-robomongo.md).
* Zjistěte, kolik RUs vaše operace jsou pomocí hello [GetLastRequestStatistics příkazů a hello Azure portálu metriky](request-units.md#GetLastRequestStatistics).
* Zjistěte, jak příliš[konfigurace pro čtení předvolby pro globálně distribuované aplikace](../cosmos-db/tutorial-global-distribution-mongodb.md).
