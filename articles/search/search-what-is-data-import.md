---
title: "nahrávání aaaData ve službě Azure Search | Microsoft Docs"
description: "Zjistěte, jak tooupload data tooan indexu ve službě Azure Search."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: aa8d47c1-4ae6-4209-a8ce-48d5a9474707
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: ashmaka
ms.openlocfilehash: a95eae94f72c1d0926804ff7e1152f21773fcabf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-tooazure-search"></a>Nahrání dat tooAzure vyhledávání
> [!div class="op_single_selector"]
> * [Přehled](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

Existují dva způsoby toopopulate indexu s daty. Hello první možností je ručně vkládání dat do indexu hello hello Azure Search pomocí [REST API](search-import-data-rest-api.md) nebo [.NET SDK](search-import-data-dotnet.md). Druhá možnost Hello je příliš[bodu na podporovaný zdroj dat](search-indexer-overview.md) tooyour indexu a nechat službu Azure Search automaticky pro vyžádání obsahu v datech hello.

## <a name="push-data-tooan-index"></a>Push index tooan dat
Tento postup znamená tooprogrammatically odesílání vaše data tooAzure vyhledávání toomake je k dispozici pro vyhledávání. U aplikací vyžadujících velmi nízkou latenci (například potřebovat vyhledat operations toobe synchronizované s dynamickými databázemi zásob) je hello push model vaší jedinou možností.

Můžete použít hello [REST API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) nebo [.NET SDK](search-import-data-dotnet.md) toopush data tooan index. Není aktuálně podporováno pro vkládání dat prostřednictvím portálu hello.

Tento přístup je flexibilnější než hello pull model, protože můžete nahrávat dokumenty samostatně nebo v dávkách (až too1000 za dávka nebo 16 MB, bude první z těchto omezení). Hello push model také umožňuje tooupload dokumenty tooAzure hledání bez ohledu na to, kde je vaše data.

Formát dat Hello rozumí Azure Search je JSON a všechny dokumenty v datové sadě hello musí mít pole, které mapují toofields definovaná ve schématu váš index. 

## <a name="pull-data-into-an-index"></a>Získání dat do indexu
Hello pull model prochází na podporovaný zdroj dat a automaticky nahrává hello data do indexu. Ve službě Azure Search je tato schopnost implementovaná prostřednictvím *indexerů*, aktuálně dostupných pro služby [Blob Storage](search-howto-indexing-azure-blob-storage.md), [Table Storage](search-howto-indexing-azure-tables.md), [Azure Cosmos DB](http://aka.ms/documentdb-search-indexer), [Azure SQL Database a SQL Server na virtuálních počítačích Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md). 

Indexery připojení zdroje dat index tooa (obvykle tabulka, zobrazení nebo ekvivalentní struktura) a mapování zdroje tooequivalent pole v indexu hello. Během provádění hello řádků je automaticky transformovaných tooJSON a načtena do zadaného indexu hello. Všechny indexery podporovat, plánování, takže můžete určit, jak často bude hello data toobe aktualizovat. Většina indexery zadejte pokud ji podporuje hello zdroj dat sledování změn. Sledování změn a odstranění tooexisting dokumenty kromě toorecognizing nových dokumentů, indexery odeberte hello nutné tooactively spravovat hello data v indexu. 

Funkce indexeru je zpřístupněná v hello [portál Azure](search-import-data-portal.md), hello [REST API](/rest/api/searchservice/Indexer-operations)a hello [.NET SDK](/dotnet/api/microsoft.azure.search.indexersoperations). 

Portál hello toousing výhod je, že Azure Search můžete obvykle vygenerujte výchozí index schéma pro vás čtení metadat hello sady hello zdroje dat. Vygenerovaný index hello můžete upravit, dokud zpracovává hello index, po které hello pouze úpravy schématu povoleny, jsou ty, které nevyžadují novou indexaci. Pokud chcete, aby toomake dopad změny hello hello schématu přímo, potřebovali byste toorebuild hello index. 

Po naplnění indexu hello můžete použít **Průzkumník služby Search** hello panelu portálu příkazů jako krok ověření.

## <a name="query-an-index-using-search-explorer"></a>Dotazování indexu pomocí Průzkumník služby Search

Rychlý způsob tooperform předběžné kontroly na odeslání dokumentu hello je toouse **Průzkumník služby Search** hello portálu. Průzkumník Hello umožňuje dotazování indexu bez nutnosti toowrite žádný kód. Hello vyhledávání je založeno na výchozích nastavení, jako je například hello [jednoduchý syntaktický](/rest/api/searchservice/simple-query-syntax-in-azure-search) a výchozí [parametr dotazu searchMode](/rest/api/searchservice/search-documents). Výsledky jsou vráceny ve formátu JSON, tak, aby si můžete prohlédnout celý dokument hello.

> [!TIP]
> Řada [ukázky kódu Azure Search](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) zahrnout vložené nebo snadno dostupné datové sady, nabídky tooget snadný způsob spuštění. portál Hello také poskytuje ukázkové indexeru a zdroj dat, který se skládá z nemovitosti malé datové sady (s názvem "realestate-us-sample"). Při spuštění indexeru předkonfigurované hello na zdroj dat ukázkový text hello, index je vytvořen a načíst s dokumenty, které může být dotazován v Průzkumník služby Search nebo kód, který můžete psát.
