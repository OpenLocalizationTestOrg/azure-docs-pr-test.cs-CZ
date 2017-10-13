---
title: "Indexery ve službě Azure Search | Dokumentace Microsoftu"
description: "Procházejte databázi SQL Azure, službu Azure Cosmos DB nebo úložiště Azure, extrahujte prohledávatelná data a naplňte jimi index služby Azure Search."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 34a7694c-8fd9-46b1-8900-cefdd7236323
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: heidist
ms.openlocfilehash: 1ea9dfcaec188bac2e92909f4811568b589357d4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="indexers-in-azure-search"></a>Indexery ve službě Azure Search
> [!div class="op_single_selector"]
>
> * [Přehled](search-indexer-overview.md)
> * [Azure Portal](search-import-data-portal.md)
> * [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
> * [Azure Cosmos DB](search-howto-index-documentdb.md)
> * [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
> * [Azure Table Storage](search-howto-indexing-azure-tables.md)
>
>

**Indexer** ve službě Azure Search je prohledávací modul, který extrahuje prohledávatelná data a metadata z externího zdroje dat a naplní index na základě mapování pole-pole mezi indexem a zdrojem dat. Tento přístup se někdy nazývá model Pull, protože služba si vyžádá data, aniž byste museli psát kód, který by vložil data do indexu bez vyžádání.

Můžete použít indexer jako jediný prostředek přijímání dat, nebo můžete použít kombinaci postupů, kdy se indexer použije k načtení jenom některých polí v indexu.

Indexery můžete spouštět na vyžádání nebo podle pravidelného plánu aktualizace dat, která může probíhat až každých 15 minut. Častější aktualizace vyžadují model Push, který aktualizuje data současně ve službě Azure Search i v externím zdroji dat.

## <a name="approaches-for-creating-and-managing-indexers"></a>Přístupy k vytváření a správě indexerů
Všeobecně dostupné indexery jako SQL Azure nebo Azure Cosmos DB můžete vytvořit a spravovat pomocí těchto přístupů:

* [Portál &gt; Průvodce importem dat](search-get-started-portal.md)
* [Rozhraní API služby REST](https://msdn.microsoft.com/library/azure/dn946891.aspx)
* [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx)

## <a name="basic-configuration-steps"></a>Postup základní konfigurace
Indexery můžou nabízet funkce, které jsou jedinečné pro daný zdroj dat. Z toho důvodu se budou některé aspekty konfigurace indexeru nebo zdroje dat lišit podle typu indexeru. Všechny indexery ale sdílejí stejné základní složení a požadavky. Níže najdete popis kroků společných pro všechny indexery.

### <a name="step-1-create-an-index"></a>Krok 1: Vytvoření indexu
Indexer automatizuje některé úkoly související s příjmem dat, ale vytváření indexu k nim nepatří. K základním požadavkům patří předdefinovaný index s poli, která odpovídají polím v externím zdroji dat. Další informace o strukturování indexu najdete v tématu [Vytvoření indexu (rozhraní API Azure Search REST)](https://msdn.microsoft.com/library/azure/dn798941.aspx).

### <a name="step-2-create-a-data-source"></a>Krok 2: Vytvoření zdroje dat
Indexer získává data ze **zdroje dat**, který obsahuje informace, jako je například připojovací řetězec. Aktuálně jsou podporované tyto zdroje dat:

* [Azure SQL Database nebo SQL Server na virtuálním počítači Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-documentdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) pro vybrané typy obsahu
* [Azure Table Storage](search-howto-indexing-azure-tables.md)

Zdroje dat se konfigurují a spravují nezávisle na indexerech, které je používají, což znamená, že několik indexerů může používat zdroj dat k načtení více indexů současně.

### <a name="step-3create-and-schedule-the-indexer"></a>Krok 3: Vytvoření a naplánování indexeru
Definice indexeru je konstrukce, která určuje index, zdroj dat a plán. Indexer můžete odkazovat na zdroj dat z jiné služby, pokud se tento zdroj dat nachází ve stejném předplatném. Další informace o strukturování indexeru najdete v tématu [Vytvoření indexeru (rozhraní API Azure Search REST)](https://msdn.microsoft.com/library/azure/dn946899.aspx).

## <a name="next-steps"></a>Další kroky
Teď jste získali základní představu. V dalším kroku se zaměříme na požadavky a úlohy specifické pro různé typy zdrojů dat.

* [Azure SQL Database nebo SQL Server na virtuálním počítači Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-documentdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Azure Table Storage](search-howto-indexing-azure-tables.md)
* [Indexování objektů blob CSV pomocí indexeru Azure Search Blob](search-howto-index-csv-blobs.md)
* [Indexování objektů blob JSON pomocí indexeru Azure Search Blob](search-howto-index-json-blobs.md)
