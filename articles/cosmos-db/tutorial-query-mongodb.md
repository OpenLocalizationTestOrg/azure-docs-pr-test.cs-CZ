---
title: "Azure Cosmos DB: Jak tooquery pomocí hello rozhraní API DocumentDB? | Dokumentace Microsoftu"
description: "Přečtěte si informace tooquery s hello DocumentDB rozhraní API pro Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: e3e5a49f7510942bcfb15330e5f86c5dd8b1e5d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-with-api-for-mongodb"></a><span data-ttu-id="d5148-104">Azure Cosmos DB: Jak tooquery s rozhraním API pro MongoDB?</span><span class="sxs-lookup"><span data-stu-id="d5148-104">Azure Cosmos DB: How tooquery with API for MongoDB?</span></span>

<span data-ttu-id="d5148-105">Hello Azure Cosmos DB [rozhraní API pro MongoDB](mongodb-introduction.md) podporuje [MongoDB prostředí dotazy](https://docs.mongodb.com/manual/tutorial/query-documents/).</span><span class="sxs-lookup"><span data-stu-id="d5148-105">hello Azure Cosmos DB [API for MongoDB](mongodb-introduction.md) supports [MongoDB shell queries](https://docs.mongodb.com/manual/tutorial/query-documents/).</span></span> 

<span data-ttu-id="d5148-106">Tento článek se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="d5148-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="d5148-107">Dotazování na data s MongoDB</span><span class="sxs-lookup"><span data-stu-id="d5148-107">Querying data with MongoDB</span></span>

## <a name="sample-document"></a><span data-ttu-id="d5148-108">Ukázka dokumentu</span><span class="sxs-lookup"><span data-stu-id="d5148-108">Sample document</span></span>

<span data-ttu-id="d5148-109">Hello dotazy v tomto článku použít hello následující ukázka dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d5148-109">hello queries in this article use hello following sample document.</span></span>

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
## <span data-ttu-id="d5148-110"><a id="examplequery1"></a>Příklad dotazu 1</span><span class="sxs-lookup"><span data-stu-id="d5148-110"><a id="examplequery1"></a> Example query 1</span></span> 

<span data-ttu-id="d5148-111">Zadána hello ukázka rodiny dokumentu výše hello následující dotaz vrátí hello dokumenty, kde pole id hello odpovídá `WakefieldFamily`.</span><span class="sxs-lookup"><span data-stu-id="d5148-111">Given hello sample family document above, hello following query returns hello documents where hello id field matches `WakefieldFamily`.</span></span>

<span data-ttu-id="d5148-112">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="d5148-112">**Query**</span></span>
    
    db.families.find({ id: “WakefieldFamily”})

<span data-ttu-id="d5148-113">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="d5148-113">**Results**</span></span>

    {
    "_id": "ObjectId(\"58f65e1198f3a12c7090e68c\")",
    "id": "WakefieldFamily",
    "parents": [
      {
        "familyName": "Wakefield",
        "givenName": "Robin"
      },
      {
        "familyName": "Miller",
        "givenName": "Ben"
      }
    ],
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ],
    "address": {
      "state": "NY",
      "county": "Manhattan",
      "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

## <span data-ttu-id="d5148-114"><a id="examplequery2"></a>Příklad dotazu 2</span><span class="sxs-lookup"><span data-stu-id="d5148-114"><a id="examplequery2"></a>Example query 2</span></span> 

<span data-ttu-id="d5148-115">Hello další dotaz vrátí všechny podřízené objekty hello řady hello.</span><span class="sxs-lookup"><span data-stu-id="d5148-115">hello next query returns all hello children in hello family.</span></span> 

<span data-ttu-id="d5148-116">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="d5148-116">**Query**</span></span>
    
    db.familes.find( { id: “WakefieldFamily” }, { children: true } )

<span data-ttu-id="d5148-117">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="d5148-117">**Results**</span></span>

    {
    "_id": "ObjectId("58f65e1198f3a12c7090e68c")",
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ]
    }


## <span data-ttu-id="d5148-118"><a id="examplequery3"></a>Příklad dotazu 3</span><span class="sxs-lookup"><span data-stu-id="d5148-118"><a id="examplequery3"></a>Example query 3</span></span> 

<span data-ttu-id="d5148-119">Hello další dotaz vrátí všechny hello rodiny, které jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="d5148-119">hello next query returns all hello families which are registered.</span></span> 

<span data-ttu-id="d5148-120">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="d5148-120">**Query**</span></span>
    
    db.families.find( { "isRegistered" : true })
<span data-ttu-id="d5148-121">**Výsledky** bude vrácen žádný dokument.</span><span class="sxs-lookup"><span data-stu-id="d5148-121">**Results** No document will be returned.</span></span> 

## <span data-ttu-id="d5148-122"><a id="examplequery4"></a>Příklad dotazu 4</span><span class="sxs-lookup"><span data-stu-id="d5148-122"><a id="examplequery4"></a>Example query 4</span></span>

<span data-ttu-id="d5148-123">Hello další dotaz vrátí všechny hello rodiny, které nejsou registrované.</span><span class="sxs-lookup"><span data-stu-id="d5148-123">hello next query returns all hello families which are not registered.</span></span> 

<span data-ttu-id="d5148-124">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="d5148-124">**Query**</span></span>
    
    db.families.find( { "isRegistered" : false })
<span data-ttu-id="d5148-125">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="d5148-125">**Results**</span></span>

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
<span data-ttu-id="d5148-126">}</span><span class="sxs-lookup"><span data-stu-id="d5148-126">}</span></span>

## <span data-ttu-id="d5148-127"><a id="examplequery5"></a>Příklad dotazu 5</span><span class="sxs-lookup"><span data-stu-id="d5148-127"><a id="examplequery5"></a>Example query 5</span></span>

<span data-ttu-id="d5148-128">Hello další dotaz vrátí všechny hello řady, která nejsou registrovaná a stavu je NY.</span><span class="sxs-lookup"><span data-stu-id="d5148-128">hello next query returns all hello families which are not registered and state is NY.</span></span> 

<span data-ttu-id="d5148-129">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="d5148-129">**Query**</span></span>
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

<span data-ttu-id="d5148-130">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="d5148-130">**Results**</span></span>

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
<span data-ttu-id="d5148-131">}</span><span class="sxs-lookup"><span data-stu-id="d5148-131">}</span></span>


## <span data-ttu-id="d5148-132"><a id="examplequery6"></a>Příklad dotazu 6</span><span class="sxs-lookup"><span data-stu-id="d5148-132"><a id="examplequery6"></a>Example query 6</span></span>

<span data-ttu-id="d5148-133">Hello další dotaz vrátí všechny hello rodiny, kde jsou podřízené objekty tříd 8.</span><span class="sxs-lookup"><span data-stu-id="d5148-133">hello next query returns all hello families where children grades are 8.</span></span>

<span data-ttu-id="d5148-134">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="d5148-134">**Query**</span></span>
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

<span data-ttu-id="d5148-135">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="d5148-135">**Results**</span></span>

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
<span data-ttu-id="d5148-136">}</span><span class="sxs-lookup"><span data-stu-id="d5148-136">}</span></span>

## <span data-ttu-id="d5148-137"><a id="examplequery7"></a>Příklad dotazu 7</span><span class="sxs-lookup"><span data-stu-id="d5148-137"><a id="examplequery7"></a>Example query 7</span></span>

<span data-ttu-id="d5148-138">Hello další dotaz vrátí všechny hello rodiny, kde je velikost, děti pole 3.</span><span class="sxs-lookup"><span data-stu-id="d5148-138">hello next query returns all hello families where size of children array is 3.</span></span>

<span data-ttu-id="d5148-139">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="d5148-139">**Query**</span></span>
  
      db.Family.find( {children: { $size:3} } )

<span data-ttu-id="d5148-140">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="d5148-140">**Results**</span></span>

<span data-ttu-id="d5148-141">Žádné výsledky, bude vrácen jako nemáme k dispozici více než 2 podřízené objekty.</span><span class="sxs-lookup"><span data-stu-id="d5148-141">No results will be returned as we do not have more than 2 children.</span></span> <span data-ttu-id="d5148-142">Jenom v případě, že je parametr 2 Tento dotaz bude úspěšné a vrátí hello celého dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d5148-142">Only when parameter is 2 this query will succeed and return hello full document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5148-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5148-143">Next steps</span></span>

<span data-ttu-id="d5148-144">V tomto kurzu provedete krok hello následující:</span><span class="sxs-lookup"><span data-stu-id="d5148-144">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d5148-145">Jak se naučili tooquery pomocí MongoDB</span><span class="sxs-lookup"><span data-stu-id="d5148-145">Learned how tooquery using MongoDB</span></span> 

<span data-ttu-id="d5148-146">Nyní můžete přejít toohello další kurz toolearn jak toodistribute data globálně.</span><span class="sxs-lookup"><span data-stu-id="d5148-146">You can now proceed toohello next tutorial toolearn how toodistribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d5148-147">Globálně distribuci dat</span><span class="sxs-lookup"><span data-stu-id="d5148-147">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)

