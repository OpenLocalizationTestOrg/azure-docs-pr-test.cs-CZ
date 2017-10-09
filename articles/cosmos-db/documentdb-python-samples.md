---
title: "Příklady Python aaaDocumentDB rozhraní API pro Azure Cosmos DB | Microsoft Docs"
description: "Najít příklady Python pro běžné úlohy v Azure DB Cosmos, včetně operace CRUD na githubu."
keywords: "Příklady Python"
services: cosmos-db
author: moderakh
manager: jhubbard
editor: monicar
documentationcenter: python
ms.assetid: 7f4f8db3-e9db-4645-92ef-7819d486a349
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2016
ms.author: moderakh
ms.openlocfilehash: d8f240782b0997f2d32b68d310dc6f4ff6cb36d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-python-examples"></a>Azure Cosmos DB Python příklady
> [!div class="op_single_selector"]
> * [Příklady rozhraní .NET](documentdb-dotnet-samples.md)
> * [Příklady Node.js](documentdb-nodejs-samples.md)
> * [Příklady Python](documentdb-python-samples.md)
> * [Galerie ukázkový kód Azure](https://azure.microsoft.com/documentation/samples/?service=documentdb)
> 
> 

Ukázka řešení, která provádět operace CRUD a dalších běžných operací s prostředky Azure Cosmos DB jsou součástí hello [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) úložiště GitHub. Tento článek obsahuje:

* Soubory projektu odkazy toohello úlohy v každé hello Python příkladu. 
* Odkazy toohello související obsah referenční dokumentace rozhraní API.

**Požadavky**

1. Je třeba tyto příklady Python toouse účet Azure:
   * Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/): získáte kredity, můžete použít tootry na placené služby Azure a i po jejich použití až můžete hello účet ponechat a používat bezplatné služby Azure, jako jsou weby. Platební karty nikdy odečte, není-li explicitně změnit nastavení a požádejte toobe účtovat.
     * Můžete [aktivovat výhody předplatitele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): Visual Studio vaše předplatné vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.
2. Musíte taky hello [Python SDK](documentdb-sdk-python.md). 
   
   > [!NOTE]
   > Každá ukázka je samostatný, nastaví sám a vyčistí po sám sebe. Jako takový hello ukázky vydat více volání příliš[document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Pokaždé, když k tomu je vaše předplatné bude účtován na 1 hodinu využití za hello úroveň výkonu kolekce hello vytváří. 
   > 
   > 

## <a name="database-examples"></a>Příklady databáze
Hello [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) souboru hello [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) projektu ukazuje, jak tooperform hello následující úlohy.

| Úkol | API – referenční informace |
| --- | --- |
| [Vytvoření databáze](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) |[document_client. Metodu CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Dotaz na účet pro databázi](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) |[document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Čtení databáze podle Id](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) |[document_client. ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Seznam databází pro účet](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) |[document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Odstranění databáze](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) |[document_client. Metodu DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |

## <a name="collection-examples"></a>Příklady kolekce
Hello [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) souboru hello [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) projektu ukazuje, jak tooperform hello následující úlohy.

| Úkol | API – referenční informace |
| --- | --- |
| [Vytvoření kolekce](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) |[document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Přečtěte si seznam všech kolekcí v databázi](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) |[document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Získání kolekce podle Id](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) |[document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Získat úroveň výkonu kolekce](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) |[DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Změnit úroveň výkonu kolekce](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) |[document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Odstranit kolekci](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) |[document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |

