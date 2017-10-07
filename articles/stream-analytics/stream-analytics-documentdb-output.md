---
title: "výstup aaaJSON Stream Analytics | Microsoft Docs"
description: "Zjistěte, jak Stream Analytics můžete cílit na Azure Cosmos DB pro výstup JSON, archivace dat a nízkou latencí dotazy na nestrukturovaných dat JSON."
keywords: "Výstup JSON"
documentationcenter: 
services: stream-analytics,documentdb
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 5d2a61a6-0dbf-4f1b-80af-60a80eb25dd1
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: fa717818c839ecd7a60fcee33d22011990fd5878
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="target-azure-cosmos-db-for-json-output-from-stream-analytics"></a>Cíl Azure Cosmos DB pro výstup JSON ze služby Stream Analytics
Stream Analytics můžete cílit na [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) pro výstup JSON, povolení archivace a nízkou latencí dotazy dat na nestrukturovaných dat JSON. Tento dokument popisuje některé osvědčené postupy při implementaci této konfigurace.

Pro ty, kteří jsou obeznámeni s Cosmos DB, podívejte se na [studijní postup Azure Cosmos DB](https://azure.microsoft.com/documentation/learning-paths/documentdb/) tooget spuštěna. 

Poznámka: Rozhraní API DB Mongo na základě Cosmos DB kolekce není aktuálně podporována. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Základní informace o DB Cosmos jako cíl výstupu
Hello Azure Cosmos DB výstupu v Stream Analytics, který umožňuje zapisovat zpracování výsledků jako výstup JSON do vaší kolekcí Cosmos DB datového proudu. Stream Analytics nevytvoří kolekcí do databáze, místo toho byste museli toocreate je předem. Toto je, aby byly transparentní tooyou hello fakturace náklady Cosmos DB kolekcí, a tak, aby mohli vyladit výkon hello, konzistence a kapacitu vaší kolekce přímo pomocí hello [rozhraní API DB Cosmos](https://msdn.microsoft.com/library/azure/dn781481.aspx). Doporučujeme používat jednu databázi DB Cosmos za streamování úlohy toologically samostatné kolekce pro úlohu streamování.

Některé možnosti kolekce Cosmos DB hello jsou podrobně popsány níže.

## <a name="tune-consistency-availability-and-latency"></a>Vyladění konzistence, dostupností a latencí
toomatch vaše požadavky aplikací Cosmos DB vám umožní toofine tune hello databáze a kolekce a zkontrolujte kompromis mezi konzistencí, dostupností a latencí. V závislosti na tom, jaké úrovně konzistenci čtení potřeb scénář proti čtení a zápisu latence, že můžete úroveň konzistence na vašem účtu databáze. Také ve výchozím nastavení, Cosmos DB umožňuje synchronní indexování na každou kolekci tooyour operace CRUD. To je další užitečné možnost toocontrol hello zápisu nebo čtení výkonu do databáze. Cosmos. Další informace v tomto tématu najdete v tématu hello [změnit vaší databáze a dotaz úrovně konzistence](../documentdb/documentdb-consistency-levels.md) článku.

## <a name="upserts-from-stream-analytics"></a>Upserts ze služby Stream Analytics
Stream Analytics integrace s Cosmos DB umožňuje tooinsert nebo aktualizace záznamů v kolekci Cosmos DB na základě daného sloupce ID dokumentu. Toto je také odkazované tooas *Upsert*.

Stream Analytics využívá optimistické Upsert přístup, kde aktualizace jsou pouze provádět při vložení nezdaří z důvodu konfliktu tooa ID dokumentu. Tato aktualizace je prováděno pomocí Stream Analytics jako opravy, takže umožňuje částečné aktualizace toohello dokumentu, tj. přidání nových vlastností nebo nahrazení stávající vlastnosti se provádí postupně. Všimněte si, že změny v hello hodnoty vlastností pole v dokumentu JSON za následek hello celé pole získávání přepsat tj není pole hello sloučit.

## <a name="data-partitioning-in-cosmos-db"></a>Vytvoření oddílů dat v databázi systému Cosmos
Cosmos DB [oddíly kolekce](../cosmos-db/partition-data.md) jsou hello doporučenému přístupu pro dělení vaše data. 

Pro jeden Cosmos DB kolekce Stream Analytics stále můžete toopartition data podle schémat dotazů hello a požadavkům na výkon vaší aplikace. Každá kolekce může obsahovat až too10GB dat (maximum) a aktuálně neexistuje žádný způsob, jak tooscale nahoru (nebo přetečení) kolekce. Pro škálování, Stream Analytics můžete toowrite toomultiple kolekce s danou předponu (viz níže podrobnosti o použití). Stream Analytics používá hello konzistentní [Hash oddílu překladač](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) strategie na základě uživatele hello zadaný sloupec toopartition PartitionKey svoje záznamy o výstup. Hello počet kolekcí s hello zadané předpony v hello streamování čas spuštění úlohy se používá jako počet oddílů výstup hello, zapíše tooin paralelní úlohy hello toowhich (Cosmos DB kolekce = výstup oddíly). Pro jednu kolekci s Opožděné indexování to jenom vložení, o 0.4 MB/s zápisu propustnost je možné očekávat. Použití více kolekcí můžete povolit, můžete tooachieve vyšší propustnost a zvýšené kapacity.

Pokud máte v úmyslu počet oddílů hello tooincrease v hello budoucí, musíte toostop úlohy změny rozdělení hello dat z existující kolekce do nové kolekce a potom úlohy služby Stream Analytics hello restartování. Další podrobnosti o používání PartitionResolver a opětovné vytváření oddílů společně s ukázkový kód, bude součástí následné post. článek Hello [dělení a škálování v Cosmos DB](../documentdb/documentdb-partition-data.md) také podrobné informace o to.

## <a name="cosmos-db-settings-for-json-output"></a>Nastavení cosmos DB pro výstup JSON
Vytváření Cosmos DB jako výstupu v Stream Analytics generuje řádku informace, jak vidíte níže. Tato část obsahuje vysvětlení hello vlastnosti definice.

Dělené kolekce | Více kolekcí "Jednoho oddílu"
---|---
![documentdb stream analytics výstup obrazovky](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png) |  ![documentdb stream analytics výstup obrazovky](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)


  
> [!NOTE]
> Hello **více kolekcí "Jednoho oddílu"** scénář vyžaduje klíč oddílu a konfigurace je podporována. 

* **Výstup Alias** – alias toorefer tento výstup v dotazu ASA  
* **Název účtu** – hello název nebo identifikátor URI hello Cosmos DB účet koncový bod.  
* **Účet klíč** – hello sdílený přístupový klíč pro hello účet Cosmos DB.  
* **Databáze** – název databáze hello Cosmos DB.  
* **Vzor názvu** – název kolekce hello nebo jejich vzor toobe kolekce hello používá. Formát názvu kolekce Hello lze sestavit pomocí tokenu hello volitelné {partition}, na kterém oddíly začínají od 0. Ukázka platné vstupní hodnoty jsou následující:  
  1\) kolekce – jednu kolekci s názvem "Kolekce" musí existovat.  
  2\) kolekce {partition} – takových kolekcí, musí existovat – "MyCollection0", "MyCollection1", "MyCollection2" a tak dále.  
* **Klíč oddílu** – volitelné. To je potřeba jenom Pokud používáte token {vytvoření oddílu} ve vzoru názvu vaší kolekce. Název Hello hello pole ve výstupních událostech používaný toospecify hello klíče pro rozdělení výstupu do kolekcí. Pro výstup jedinou kolekci všechny libovolný výstupního sloupce lze použít například ID oddílu.  
* **ID dokumentu** – volitelné. Název Hello hello pole ve výstupních událostech používá toospecify hello primární klíč, na které insert nebo update jsou založené operace.  
