---
title: "Dotazování indexu (portál – Azure Search) | Dokumentace Microsoftu"
description: "Vydejte vyhledávací dotaz v Průzkumníku služby Search na webu Azure Portal."
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 8e524188-73a7-44db-9e64-ae8bf66b05d3
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 07/10/2017
ms.author: ashmaka
ms.openlocfilehash: dd68d8ed073bf7b8666ddef35a2f1f84df690b4b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="query-an-azure-search-index-using-search-explorer-in-the-azure-portal"></a>Dotazování indexu služby Azure Search pomocí průzkumníka služby Search na webu Azure Portal
> [!div class="op_single_selector"]
> * [Přehled](search-query-overview.md)
> * [Azure Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Tento článek vám ukáže postup dotazování indexu služby Azure Search pomocí **průzkumníka služby Search** na webu Azure Portal. Pomocí průzkumníka služby Search můžete odesílat jednoduché nebo úplné řetězce dotazů Lucene do jakéhokoli existujícího indexu v rámci služby.

## <a name="open-the-service-dashboard"></a>Otevření řídicího panelu služby
1. Klikněte na **Všechny prostředky** na panelu odkazů na levé straně webu [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).
2. Vyberte službu Azure Search.

## <a name="select-an-index"></a>Výběr indexu

Na dlaždici **Indexy** vyberte index, který chcete prohledat.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>Otevření průzkumníka služby Search

Kliknutím na dlaždici Průzkumník služby Search otevřete vysouvací panel hledání a podokno výsledků.

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a>Spusťte hledání

Pokud používáte průzkumníka služby Search, můžete dotaz formulovat zadáním [parametrů dotazu](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).

1. V části **Řetězec dotazu** zadejte dotaz a potom stiskněte **Hledat**. 

   Řetězec dotazu se automaticky parsuje do správné adresy URL žádosti za účelem odeslání žádosti HTTP do rozhraní REST API služby Azure Search.   
   
   K vytvoření žádosti můžete použít jakoukoli platnou syntaxi jednoduchého nebo úplného dotazu Lucene. Znak `*` je ekvivalentní prázdnému nebo nespecifikovanému hledání, které vrátí všechny dokumenty bez zvláštního pořadí.

2. V části **Výsledky** jsou výsledky dotazu uvedené ve stejném nezpracovaném formátu JSON jako datová část vrácená v textu odpovědi HTTP při vydávání žádostí prostřednictvím kódu programu.

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a>Další kroky

V následujících zdrojích najdete další informace o syntaxi dotazů a příklady.

 + [Jednoduchá syntaxe dotazů](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Syntaxe dotazů Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Příklady syntaxe dotazů Lucene](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [Syntaxe výrazů filtru OData](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 