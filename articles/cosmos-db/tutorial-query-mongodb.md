---
title: "Azure Cosmos DB: Jak dotazovat pomocí rozhraní API DocumentDB? | Dokumentace Microsoftu"
description: "Postup dotazování pomocí DocumentDB rozhraní API pro Azure Cosmos DB"
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
ms.openlocfilehash: feffc553a9aa931d96cec71c101674fce08a466b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-how-to-query-with-api-for-mongodb"></a><span data-ttu-id="29498-104">Azure Cosmos DB: Jak dotazovat pomocí rozhraní API pro MongoDB?</span><span class="sxs-lookup"><span data-stu-id="29498-104">Azure Cosmos DB: How to query with API for MongoDB?</span></span>

<span data-ttu-id="29498-105">Azure Cosmos DB [rozhraní API pro MongoDB](mongodb-introduction.md) podporuje [MongoDB prostředí dotazy](https://docs.mongodb.com/manual/tutorial/query-documents/).</span><span class="sxs-lookup"><span data-stu-id="29498-105">The Azure Cosmos DB [API for MongoDB](mongodb-introduction.md) supports [MongoDB shell queries](https://docs.mongodb.com/manual/tutorial/query-documents/).</span></span> 

<span data-ttu-id="29498-106">Tento článek obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="29498-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="29498-107">Dotazování na data s MongoDB</span><span class="sxs-lookup"><span data-stu-id="29498-107">Querying data with MongoDB</span></span>

## <a name="sample-document"></a><span data-ttu-id="29498-108">Ukázka dokumentu</span><span class="sxs-lookup"><span data-stu-id="29498-108">Sample document</span></span>

<span data-ttu-id="29498-109">Dotazy v tomto článku použít následující ukázka dokumentu.</span><span class="sxs-lookup"><span data-stu-id="29498-109">The queries in this article use the following sample document.</span></span>

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
## <span data-ttu-id="29498-110"><a id="examplequery1"></a>Příklad dotazu 1</span><span class="sxs-lookup"><span data-stu-id="29498-110"><a id="examplequery1"></a> Example query 1</span></span> 

<span data-ttu-id="29498-111">Zadaný vzorek rodiny dokumentu výše, následující dotaz vrátí dokumenty kde pole id odpovídá `WakefieldFamily`.</span><span class="sxs-lookup"><span data-stu-id="29498-111">Given the sample family document above, the following query returns the documents where the id field matches `WakefieldFamily`.</span></span>

<span data-ttu-id="29498-112">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="29498-112">**Query**</span></span>
    
    db.families.find({ id: “WakefieldFamily”})

<span data-ttu-id="29498-113">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="29498-113">**Results**</span></span>

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

## <span data-ttu-id="29498-114"><a id="examplequery2"></a>Příklad dotazu 2</span><span class="sxs-lookup"><span data-stu-id="29498-114"><a id="examplequery2"></a>Example query 2</span></span> 

<span data-ttu-id="29498-115">Další dotaz vrátí všechny podřízené objekty řady.</span><span class="sxs-lookup"><span data-stu-id="29498-115">The next query returns all the children in the family.</span></span> 

<span data-ttu-id="29498-116">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="29498-116">**Query**</span></span>
    
    db.familes.find( { id: “WakefieldFamily” }, { children: true } )

<span data-ttu-id="29498-117">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="29498-117">**Results**</span></span>

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


## <span data-ttu-id="29498-118"><a id="examplequery3"></a>Příklad dotazu 3</span><span class="sxs-lookup"><span data-stu-id="29498-118"><a id="examplequery3"></a>Example query 3</span></span> 

<span data-ttu-id="29498-119">Další dotaz vrátí všechny rodiny, které jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="29498-119">The next query returns all the families which are registered.</span></span> 

<span data-ttu-id="29498-120">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="29498-120">**Query**</span></span>
    
    db.families.find( { "isRegistered" : true })
<span data-ttu-id="29498-121">**Výsledky** bude vrácen žádný dokument.</span><span class="sxs-lookup"><span data-stu-id="29498-121">**Results** No document will be returned.</span></span> 

## <span data-ttu-id="29498-122"><a id="examplequery4"></a>Příklad dotazu 4</span><span class="sxs-lookup"><span data-stu-id="29498-122"><a id="examplequery4"></a>Example query 4</span></span>

<span data-ttu-id="29498-123">Další dotaz vrátí všechny rodiny, které nejsou registrované.</span><span class="sxs-lookup"><span data-stu-id="29498-123">The next query returns all the families which are not registered.</span></span> 

<span data-ttu-id="29498-124">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="29498-124">**Query**</span></span>
    
    db.families.find( { "isRegistered" : false })
<span data-ttu-id="29498-125">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="29498-125">**Results**</span></span>

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
<span data-ttu-id="29498-126">}</span><span class="sxs-lookup"><span data-stu-id="29498-126">}</span></span>

## <span data-ttu-id="29498-127"><a id="examplequery5"></a>Příklad dotazu 5</span><span class="sxs-lookup"><span data-stu-id="29498-127"><a id="examplequery5"></a>Example query 5</span></span>

<span data-ttu-id="29498-128">Další dotaz vrátí všechny řady, která nejsou registrovaná a stavu je NY.</span><span class="sxs-lookup"><span data-stu-id="29498-128">The next query returns all the families which are not registered and state is NY.</span></span> 

<span data-ttu-id="29498-129">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="29498-129">**Query**</span></span>
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

<span data-ttu-id="29498-130">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="29498-130">**Results**</span></span>

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
<span data-ttu-id="29498-131">}</span><span class="sxs-lookup"><span data-stu-id="29498-131">}</span></span>


## <span data-ttu-id="29498-132"><a id="examplequery6"></a>Příklad dotazu 6</span><span class="sxs-lookup"><span data-stu-id="29498-132"><a id="examplequery6"></a>Example query 6</span></span>

<span data-ttu-id="29498-133">Další dotaz vrátí všechny rodiny, kde jsou podřízené objekty tříd 8.</span><span class="sxs-lookup"><span data-stu-id="29498-133">The next query returns all the families where children grades are 8.</span></span>

<span data-ttu-id="29498-134">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="29498-134">**Query**</span></span>
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

<span data-ttu-id="29498-135">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="29498-135">**Results**</span></span>

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
<span data-ttu-id="29498-136">}</span><span class="sxs-lookup"><span data-stu-id="29498-136">}</span></span>

## <span data-ttu-id="29498-137"><a id="examplequery7"></a>Příklad dotazu 7</span><span class="sxs-lookup"><span data-stu-id="29498-137"><a id="examplequery7"></a>Example query 7</span></span>

<span data-ttu-id="29498-138">Další dotaz vrátí všechny rodiny, kde je velikost, děti pole 3.</span><span class="sxs-lookup"><span data-stu-id="29498-138">The next query returns all the families where size of children array is 3.</span></span>

<span data-ttu-id="29498-139">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="29498-139">**Query**</span></span>
  
      db.Family.find( {children: { $size:3} } )

<span data-ttu-id="29498-140">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="29498-140">**Results**</span></span>

<span data-ttu-id="29498-141">Žádné výsledky, bude vrácen jako nemáme k dispozici více než 2 podřízené objekty.</span><span class="sxs-lookup"><span data-stu-id="29498-141">No results will be returned as we do not have more than 2 children.</span></span> <span data-ttu-id="29498-142">Jenom v případě, že je parametr 2 Tento dotaz bude úspěšné a vrátit celého dokumentu.</span><span class="sxs-lookup"><span data-stu-id="29498-142">Only when parameter is 2 this query will succeed and return the full document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="29498-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29498-143">Next steps</span></span>

<span data-ttu-id="29498-144">V tomto kurzu jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="29498-144">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="29498-145">Zjistili, jak k dotazování pomocí MongoDB</span><span class="sxs-lookup"><span data-stu-id="29498-145">Learned how to query using MongoDB</span></span> 

<span data-ttu-id="29498-146">Nyní můžete přejít k dalším kurzu se dozvíte, jak se bude distribuovat globální data.</span><span class="sxs-lookup"><span data-stu-id="29498-146">You can now proceed to the next tutorial to learn how to distribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="29498-147">Globálně distribuci dat</span><span class="sxs-lookup"><span data-stu-id="29498-147">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)

