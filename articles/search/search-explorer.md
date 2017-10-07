---
title: "AAA \"dotazování indexu (portál – Azure Search) | Microsoft Docs\""
description: "Vydejte vyhledávací dotaz v portálu Azure hello Průzkumník služby Search."
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
ms.openlocfilehash: 56bab3ef8a66eeb053fbbeb6d322acb6824fb34b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="query-an-azure-search-index-using-search-explorer-in-hello-azure-portal"></a>Dotazování indexu Azure Search pomocí Průzkumník služby Search v hello portálu Azure
> [!div class="op_single_selector"]
> * [Přehled](search-query-overview.md)
> * [Azure Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Tento článek ukazuje, jak index tooquery Azure Search pomocí **Průzkumník služby Search** v hello portálu Azure. Průzkumník služby Search toosubmit jednoduchý nebo úplné Lucene dotazu řetězce tooany existujícího indexu můžete použít ve službě.

## <a name="open-hello-service-dashboard"></a>Řídicí panel služby otevřete hello
1. Klikněte na tlačítko **všechny prostředky** panelu hello přechod na levé straně hello hello [portál Azure](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).
2. Vyberte službu Azure Search.

## <a name="select-an-index"></a>Výběr indexu

Vyberte hello index chcete toosearch z hello **indexy** dlaždici.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>Otevření průzkumníka služby Search

Klikněte na hello panelu vyhledávání otevřete hello tooslide dlaždici Průzkumník služby Search a podokně výsledků.

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a>Spusťte hledání

Pokud používáte hello Průzkumník služby Search, můžete zadat [parametrů dotazu](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) tooformulate hello dotazu.

1. V části **Řetězec dotazu** zadejte dotaz a potom stiskněte **Hledat**. 

   řetězec dotazu Hello se automaticky Parsuje do správné požadavek hello toosubmit adresy URL žádosti HTTP hello REST API služby Azure Search.   
   
   Můžete vytvořit žádné platné jednoduchý nebo úplné Lucene syntaxe toocreate hello požadavku dotazu. Hello `*` znak je ekvivalentní tooan prázdný nebo nezadanou vyhledávání, který vrátí všechny dokumenty v žádné konkrétní pořadí.

2. V **výsledky**, výsledky dotazu jsou uvedeny v nezpracovaném formátu JSON, identické toohello datové části, vrátí se v textu odpovědi HTTP při vydávání žádostí prostřednictvím kódu programu.

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a>Další kroky

Hello následující prostředky poskytují další dotaz informace ze syntaxe a příklady.

 + [Jednoduchá syntaxe dotazů](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Syntaxe dotazů Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Příklady syntaxe dotazů Lucene](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [Syntaxe výrazů filtru OData](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 