---
title: "aaaIndexers ve službě Azure Search | Microsoft Docs"
description: "Procházení databáze Azure SQL, Azure Cosmos DB nebo úložiště Azure tooextract prohledávatelná data a naplňte index Azure Search."
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
ms.openlocfilehash: 6a816252ec5d6032491a12651c05cb1fe77d3d1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
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

**Indexer** ve službě Azure Search je prohledávací modul, který extrahuje prohledávatelná data a metadata z externího zdroje dat a naplní index na základě mapování pole pole mezi hello index a zdroj dat. Tento přístup je někdy označují tooas model pull, protože hello služba si vyžádá data v bez nutnosti toowrite žádný kód, který by vložil data tooan index.

Můžete použít indexer jako hello jediný prostředek přijímání dat, nebo použít kombinaci technik, které patří hello použití indexeru pro načtení jenom některých hello polí v indexu.

Indexery můžete spouštět na vyžádání nebo podle pravidelného plánu aktualizace dat, která může probíhat až každých 15 minut. Častější aktualizace vyžadují model Push, který aktualizuje data současně ve službě Azure Search i v externím zdroji dat.

## <a name="approaches-for-creating-and-managing-indexers"></a>Přístupy k vytváření a správě indexerů
Všeobecně dostupné indexery jako SQL Azure nebo Azure Cosmos DB můžete vytvořit a spravovat pomocí těchto přístupů:

* [Portál &gt; Průvodce importem dat](search-get-started-portal.md)
* [Rozhraní API služby REST](https://msdn.microsoft.com/library/azure/dn946891.aspx)
* [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx)

## <a name="basic-configuration-steps"></a>Postup základní konfigurace
Indexery nabízejí funkce, které jsou jedinečné toohello zdroj dat. Z toho důvodu se budou některé aspekty konfigurace indexeru nebo zdroje dat lišit podle typu indexeru. Ale všechny indexery sdílet hello požadavky a stejný základní složení. Kroky, které jsou společné tooall, které jsou níže popsané indexery.

### <a name="step-1-create-an-index"></a>Krok 1: Vytvoření indexu
Indexer bude automatizovat některé úlohy související toodata přijímat, ale vytvoření indexu není jeden z nich. K základním požadavkům patří předdefinovaný index s poli, která odpovídají polím v externím zdroji dat. Další informace o strukturování indexu najdete v tématu [Vytvoření indexu (rozhraní API Azure Search REST)](https://msdn.microsoft.com/library/azure/dn798941.aspx).

### <a name="step-2-create-a-data-source"></a>Krok 2: Vytvoření zdroje dat
Indexer získává data ze **zdroje dat**, který obsahuje informace, jako je například připojovací řetězec. Aktuálně jsou podporovány hello následující zdroje dat:

* [Azure SQL Database nebo SQL Server na virtuálním počítači Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-documentdb.md)
* [Azure Blob storage](search-howto-indexing-azure-blob-storage.md), používá tooextract text z formátu PDF, dokumentů Office, HTML nebo XML
* [Azure Table Storage](search-howto-indexing-azure-tables.md)

Zdroje dat jsou nakonfigurované a spravované nezávisle na hello indexery, které je používají, což znamená zdroje dat mohou být využívána více tooload indexery více než jeden index najednou.

### <a name="step-3create-and-schedule-hello-indexer"></a>Krok 3: vytvoření a plánování hello indexeru
definice indexer Hello je konstrukce hello index, zdroj dat a plánu. Indexer můžete odkaz na zdroj dat z jiné služby, tak dlouho, dokud tento zdroj dat je z hello stejného předplatného. Další informace o strukturování indexeru najdete v tématu [Vytvoření indexeru (rozhraní API Azure Search REST)](https://msdn.microsoft.com/library/azure/dn946899.aspx).

## <a name="next-steps"></a>Další kroky
Teď, když máte hello základní představu, hello dalším krokem je tooreview požadavky a typ zdroje dat tooeach konkrétní úlohy.

* [Azure SQL Database nebo SQL Server na virtuálním počítači Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-documentdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md), používá tooextract text z formátu PDF, dokumentů Office, HTML nebo XML
* [Azure Table Storage](search-howto-indexing-azure-tables.md)
* [Indexování CSV objektům BLOB pomocí indexer Azure Search Blob hello](search-howto-index-csv-blobs.md)
* [Indexování objektů blob JSON pomocí indexeru Azure Search Blob](search-howto-index-json-blobs.md)
