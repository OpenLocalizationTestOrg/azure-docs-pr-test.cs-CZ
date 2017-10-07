---
title: data grafu aaaHow tooquery v Azure Cosmos DB? | Dokumentace Microsoftu
description: "Další data grafu tooquery v Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
tags: 
ms.assetid: 8bde5c80-581c-4f70-acb4-9578873c92fa
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: denlee
ms.openlocfilehash: fdde881edd6c488e2fea51e5c9665e1d736009fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-with-hello-graph-api-preview"></a>Azure Cosmos DB: Jak tooquery s hello rozhraní Graph API (preview)?

Hello Azure Cosmos DB [rozhraní Graph API](graph-introduction.md) (preview) podporuje [Gremlin](https://docs.mongodb.com/manual/tutorial/query-documents/) dotazy. Tento článek obsahuje ukázkové dokumenty a dotazuje tooget, kterou jste zahájili. Podrobné referenční Gremlin je součástí hello [Gremlin podporu](gremlin-support.md) článku.

Tento článek se zabývá hello následující úlohy: 

> [!div class="checklist"]
> * Dotazování na data s Gremlin

## <a name="prerequisites"></a>Požadavky

Pro tyto dotazy toowork musíte mít účet Azure Cosmos DB a mít dat grafu v kontejneru hello. Nemáte žádné těchto? Dokončení hello [rychlý start 5 minut](create-graph-dotnet.md) nebo hello [vývojáře kurzu](tutorial-query-graph.md) toocreate účet a naplnit databázi. Můžete spustit následující dotazy pomocí hello hello [knihovny Azure Cosmos DB .NET grafu](graph-sdk-dotnet.md), [Gremlin konzoly](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), nebo vaše oblíbené Gremlin ovladač.

## <a name="count-vertices-in-hello-graph"></a>Počet vrcholy v grafu hello

Hello následující fragment kódu ukazuje, jak toocount hello počet vrcholy v grafu hello:

```
g.V().count()
```

## <a name="filters"></a>Filtry

Můžete nastavit filtry, pomocí na Gremlin `has` a `hasLabel` kroky a zkombinovat pomocí `and`, `or`, a `not` toobuild složitější filtry. Azure Cosmos DB poskytuje, bez ohledu na schéma indexu všech vlastností v rámci vrcholy a stupňů pro rychlé dotazy:

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a>Projekce

Můžete promítnout některé vlastnosti ve výsledcích dotazů hello pomocí hello `values` kroku:

```
g.V().hasLabel('person').values('firstName')
```

## <a name="find-related-edges-and-vertices"></a>Vyhledání souvisejících okraje a vrcholy

Zatím jste pouze viděli operátory dotazu, které fungují v některé z databází. Grafy jsou rychlé a efektivní pro operace průchodu, pokud potřebujete toonavigate toorelated okraje a vrcholy. Umožňuje najít všechny přátelích Thomas. Provedeme to pomocí na Gremlin `outE` krok toofind všechny hello odesílací okrajů z Thomas a pak z těchto hran pomocí Gremlin na procházení toohello v vrcholy `inV` krok:

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

Další dotaz Hello provádí dvěma segmenty směrování toofind všechny Thomas. "přátelích přátel,", voláním `outE` a `inV` dvakrát. 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

Můžete vytvořit složitější dotazy a implementovat logiku traversal výkonné grafu pomocí Gremlin, včetně směšovací filtru výrazů, provádění opakování ve smyčce pomocí hello `loop` kroku a implementuje podmíněného navigační pomocí hello `choose` krok. Další informace o co můžete dělat s [Gremlin podporu](gremlin-support.md)!

## <a name="next-steps"></a>Další kroky

V tomto kurzu provedete krok hello následující:

> [!div class="checklist"]
> * Jak se naučili tooquery pomocí grafu 

Nyní můžete přejít toohello další kurz toolearn jak toodistribute data globálně.

> [!div class="nextstepaction"]
> [Globálně distribuci dat](tutorial-global-distribution-documentdb.md)