---
title: aaaHow tooquery s SQL v Azure Cosmos DB? | Dokumentace Microsoftu
description: "Další informace tooquery pomocí dat DocumentDB pomocí SQL v Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: tutorial-develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: d3dc51acf92cb78d4f4d9dbac7ec54b1382431cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-using-sql"></a>Azure Cosmos DB: Jak tooquery pomocí SQL?

Hello Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) podporuje dotazování dokumentů pomocí SQL. Tento článek obsahuje ukázkové dokumentu a dva ukázkové dotazy SQL a výsledky.

Tento článek se zabývá hello následující úlohy: 

> [!div class="checklist"]
> * Dotazování na data pomocí SQL

## <a name="sample-document"></a>Ukázka dokumentu

dotazy SQL Hello v tomto článku použít hello následující ukázka dokumentu.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```
## <a name="where-can-i-run-sql-queries"></a>Kde je můžete spouštět dotazy SQL?

Můžete spouštět dotazy pomocí hello Průzkumníku dat v hello portál Azure, prostřednictvím hello [REST API a sadám SDK](documentdb-sdk-dotnet.md)a to i v hello [Query playground](https://www.documentdb.com/sql/demo), která se spouští dotazy na existující sady ukázková data.

Další informace o dotazech SQL najdete v tématu:
* [Dotaz SQL a syntaxe SQL](documentdb-sql-query.md)

## <a name="prerequisites"></a>Požadavky

Tento kurz předpokládá, že máte účet Azure Cosmos databáze a kolekce. Nemáte žádné těchto? Dokončení hello [rychlý start 5 minut](create-mongodb-nodejs.md) nebo hello [vývojáře kurzu](tutorial-develop-mongodb.md) toocreate účet a kolekce.

## <a name="example-query-1"></a>Příklad dotazu 1

Zadané hello ukázka rodiny dokumentu výše, následující dotaz SQL vrátí hello dokumenty kde pole id hello odpovídá `WakefieldFamily`. Vzhledem k tomu, že je `SELECT *` příkaz hello výstup hello dotazu je hello dokončení dokumentu JSON:

**Dotaz**

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

**Výsledky**

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

## <a name="example-query-2"></a>Příklad dotazu 2

Hello další dotaz vrátí všechny názvy daným hello podřízených prvků řady hello shoduje s id `WakefieldFamily` seřazené podle jejich úrovni.

**Dotaz**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

**Výsledky**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a>Další kroky

V tomto kurzu provedete krok hello následující:

> [!div class="checklist"]
> * Jak se naučili tooquery pomocí SQL  

Nyní můžete přejít toohello další kurz toolearn jak toodistribute data globálně.

> [!div class="nextstepaction"]
> [Globálně distribuci dat](tutorial-global-distribution-documentdb.md)

