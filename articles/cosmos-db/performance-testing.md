---
title: "aaaAzure Cosmos DB škálování a testování výkonu | Microsoft Docs"
description: "Zjistěte, jak tooperform škálování a výkon testování pomocí Azure Cosmos DB"
keywords: "Testování výkonu"
services: cosmos-db
author: arramac
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f4c96ebd-f53c-427d-a500-3f28fe7b11d0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: arramac
ms.openlocfilehash: 46d1217e11a39ee970a868de9a5c5dfcf52cedf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a>Výkonu a možností škálování testování pomocí Azure Cosmos DB
Testování výkonu a škálování je klíče krok při vývoji aplikace. Mnoho aplikací hello databázové vrstvy, má značný vliv hello celkový výkon a škálovatelnost, a je proto důležité součást výkonu testování. [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) je vytvořeného pro tento účel pro elastické škálování a předvídatelný výkon a proto skvělé přizpůsobit pro aplikace, které potřebují a vysoce výkonné databázové vrstvy. 

Tento článek je odkaz pro vývojáře implementace sad testů výkonu pro zatížení Cosmos DB nebo vyhodnocení Cosmos DB pro vysoce výkonné aplikace scénáře. Se zaměřují hlavně na test výkonnosti izolované databáze hello, ale také obsahuje osvědčené postupy pro výrobní aplikace.

Po přečtení tohoto článku, budete moct tooanswer hello následující otázky:   

* Kde najdu ukázkovou aplikaci klienta rozhraní .NET pro testování výkonu databáze Cosmos? 
* Jak dosáhnout vysoké propustnosti úrovně s Cosmos DB z mé aplikace klienta?

tooget spuštění s kódem, stáhněte si prosím hello projekt z [Azure Cosmos DB výkonu testování Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

> [!NOTE]
> Hello cílem této aplikace je toodemonstrate osvědčené postupy pro extrahování lepší výkon mimo Cosmos DB s malý počet klientských počítačů. To nebyl proveden toodemonstrate hello ve špičce kapacity hello služby, který můžete škálovat neomezeně rozšiřovat.
> 
> 

Pokud hledáte tooimprove možnosti konfigurace na straně klienta Cosmos DB výkonu, najdete v části [tipy pro zvýšení výkonu Azure Cosmos DB](performance-tips.md).

## <a name="run-hello-performance-testing-application"></a>Spuštění testování aplikace hello výkonu
Hello tooget nejrychlejší způsob, jak spustit je toocompile a spuštění hello .NET ukázková níže, jak je popsáno v následujících kroků hello. Můžete také zkontrolovat hello zdrojového kódu a implementovat podobné konfigurace tooyour vlastní klientské aplikace.

**Krok 1:** projekt hello stahování z [Azure Cosmos DB výkonu testování Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), nebo úložiště GitHub rozvětvení hello.

**Krok 2:** upravit hello nastavení adresa URL koncového bodu, authorizationkey tak, CollectionThroughput a DocumentTemplate (volitelné) v souboru App.config.

> [!NOTE]
> Než kolekce s vysokou propustnost, naleznete v toohello [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/) tooestimate hello náklady na kolekci. Azure účtuje poplatky za úložiště Cosmos databáze a propustnost nezávisle na hodinu, takže odstraněním nebo snížení hello propustnost vaší kolekce Azure Cosmos DB po testování můžete uložit náklady.
> 
> 

**Krok 3:** zkompilování a spuštění z příkazového řádku hello hello konzolovou aplikaci. Měli byste vidět výstup podobný hello následující:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Krok 4 (v případě potřeby):** hello propustnost hlášené (RU/s) z nástroje hello by měla být stejná nebo vyšší než hello zřízené propustnosti hello kolekce hello. V opačném případě se zvyšující hello DegreeOfParallelism v malých přírůstcích mohou pomoci při dosažení limitu hello. Pokud náhorních plošinách hello propustnost z vaší klientské aplikace, spouštění více instancí aplikace hello na hello stejné nebo různé počítače vám pomůže dosáhnout limit hello zřídit na hello různé instance. Pokud potřebujete pomoc s Tento krok, prosím, zápis e-mailu tooaskcosmosdb@microsoft.com nebo soubor lístek podpory z hello [portálu Azure](https://portal.azure.com).

Jakmile máte hello aplikaci spuštěnou, můžete zkusit jiný [indexování zásady](indexing-policies.md) a [úrovně konzistence](consistency-levels.md) toounderstand jejich dopad na propustnosti a latence. Můžete také zkontrolovat hello zdrojového kódu a implementovat podobné konfigurace tooyour vlastní testovacích sad nebo výrobní aplikace.

## <a name="next-steps"></a>Další kroky
V tomto článku jsme se podívali na tom, jak můžete provádět výkonu a možností škálování testování pomocí DB Cosmos pomocí konzolové aplikace .NET. Další informace o práci s Azure Cosmos DB naleznete toohello najdete pod odkazy níže.

* [Testování ukázkové Azure Cosmos DB výkonu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Tooimprove možnosti konfigurace klienta Azure Cosmos DB výkonu](performance-tips.md)
* [Dělení na straně serveru v Azure Cosmos DB](partition-data.md)


