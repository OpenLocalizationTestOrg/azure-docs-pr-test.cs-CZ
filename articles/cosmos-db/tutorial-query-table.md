---
title: "aaaHow tooquery data tabulky v databázi Cosmos Azure? | Dokumentace Microsoftu"
description: "Další informace tooquery dat v tabulce v Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: kanshiG
manager: jhubbard
editor: 
tags: 
ms.assetid: 14bcb94e-583c-46f7-9ea8-db010eb2ab43
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: govindk
ms.openlocfilehash: 32526c3488c589c5be3a4a2f174aa769570f0c0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-table-data-by-using-hello-table-api-preview"></a>Azure Cosmos DB: Jak tooquery data tabulky pomocí hello tabulky rozhraní API (preview)?

Hello Azure Cosmos DB [tabulky API](table-introduction.md) (preview) podporuje OData a [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) dotazy na data klíč hodnota (tabulky).  

Tento článek se zabývá hello následující úlohy: 

> [!div class="checklist"]
> * Dotazování na data s hello tabulky rozhraní API

Hello dotazy v tomto článku použít hello následující ukázka `People` tabulky:

| Klíč oddílu | RowKey | E-mail | Telefonní číslo |
| --- | --- | --- | --- |
| Tuleňů grónských | Walter | Walter@contoso.com| 425-555-0101 |
| Smith | Ben | Ben@contoso.com| 425-555-0102 |
| Smith | Jeff | Jeff@contoso.com| 425-555-0104 | 

Protože Azure Cosmos DB není kompatibilní s hello rozhraním API Azure Table storage, najdete v části [dotazování tabulky a entity] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) podrobnosti o tom, jak tooquery pomocí hello Tabulka rozhraní API. 

Další informace o možnosti hello premium, které nabízí Azure Cosmos DB najdete v tématu [Cosmos databázi Azure: Tabulka API](table-introduction.md) a [vývoj s hello rozhraní API pro tabulky v rozhraní .NET](tutorial-develop-table-dotnet.md). 

## <a name="prerequisites"></a>Požadavky

Pro tyto dotazy toowork musíte mít účet Azure Cosmos DB a mít data entity v kontejneru hello. Nemáte žádné těchto? Dokončení hello [rychlý start pětiminutovou](https://aka.ms/acdbtnetqs) nebo hello [vývojáře kurzu](https://aka.ms/acdbtabletut) toocreate účet a naplnit databázi.

## <a name="query-on-partitionkey-and-rowkey"></a>Dotaz na klíč oddílu a RowKey
Důvodu, že vlastnosti PartitionKey a RowKey hello primární klíč entity, můžete použít následující zvláštní syntaxe tooidentify hello entity hello: 

**Dotaz**

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
**Výsledky**

| Klíč oddílu | RowKey | E-mail | Telefonní číslo |
| --- | --- | --- | --- |
| Tuleňů grónských | Walter | Walter@contoso.com| 425-555-0104 |

Alternativně můžete tyto vlastnosti jako součást hello `$filter` možnost, jak je znázorněno v následující části hello. Všimněte si, že hello názvů vlastností klíče a hodnoty konstant jsou malá a velká písmena. Hello PartitionKey i RowKey vlastnosti jsou typu řetězec. 

## <a name="query-by-using-an-odata-filter"></a>Dotazovat pomocí filtru OData
Když jste vytváření řetězec filtru, berte v úvahu tato pravidla: 

* Logické operátory hello použití definované hello specifikace protokolu OData toocompare tooa hodnotu vlastnosti. Všimněte si, že nelze porovnat hodnotu vlastnosti tooa dynamické. Jedna strana hello výrazu musí být konstanta. 
* Název vlastnosti Hello, operátor a hodnotu konstanty musí být oddělené mezerami kódovaná adresou URL. Mezeru je kódovaná jako adresa URL jako `%20`. 
* Všechny části řetězec filtru hello rozlišují velká a malá písmena. 
* Hello konstantní hodnota musí být hello stejný datový typ jako vlastnost hello v pořadí hello filtru tooreturn platný výsledků. Další informace o typech podporovaných vlastnost najdete v tématu [hello Principy datového modelu služby Table](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). 

Tady je příklad dotazu, který ukazuje, jak toofilter podle hello PartitionKey a e-mailu vlastnosti pomocí OData `$filter`.

**Dotaz**

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

Další informace o tom, jak filtrovat tooconstruct výrazy pro různé typy dat najdete v tématu [dotazování tabulky a entity](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).

**Výsledky**

| Klíč oddílu | RowKey | E-mail | Telefonní číslo |
| --- | --- | --- | --- |
| Ben |Smith | Ben@contoso.com| 425-555-0102 |

## <a name="query-by-using-linq"></a>Dotazu pomocí LINQ 
Také můžete dotazovat pomocí LINQ, který překládá toohello odpovídající výrazy dotazu OData. Tady je příklad jak hello toobuild dotazy pomocí .NET SDK:

```csharp
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("people");

TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
    .Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition(PartitionKey, QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition(Email, QueryComparisons.Equal,"Ben@contoso.com")
    ));

await table.ExecuteQuerySegmentedAsync<CustomerEntity>(query, null);
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu provedete krok hello následující:

> [!div class="checklist"]
> * Naučili, jak tooquery pomocí hello tabulky rozhraní API (preview) 

Nyní můžete přejít toohello další kurz toolearn jak toodistribute data globálně.

> [!div class="nextstepaction"]
> [Globálně distribuci dat](tutorial-global-distribution-documentdb.md)
