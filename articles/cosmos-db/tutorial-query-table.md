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
# <a name="azure-cosmos-db-how-tooquery-table-data-by-using-hello-table-api-preview"></a><span data-ttu-id="e9c2e-104">Azure Cosmos DB: Jak tooquery data tabulky pomocí hello tabulky rozhraní API (preview)?</span><span class="sxs-lookup"><span data-stu-id="e9c2e-104">Azure Cosmos DB: How tooquery table data by using hello Table API (preview)?</span></span>

<span data-ttu-id="e9c2e-105">Hello Azure Cosmos DB [tabulky API](table-introduction.md) (preview) podporuje OData a [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) dotazy na data klíč hodnota (tabulky).</span><span class="sxs-lookup"><span data-stu-id="e9c2e-105">hello Azure Cosmos DB [Table API](table-introduction.md) (preview) supports OData and [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) queries against key/value (table) data.</span></span>  

<span data-ttu-id="e9c2e-106">Tento článek se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="e9c2e-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="e9c2e-107">Dotazování na data s hello tabulky rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e9c2e-107">Querying data with hello Table API</span></span>

<span data-ttu-id="e9c2e-108">Hello dotazy v tomto článku použít hello následující ukázka `People` tabulky:</span><span class="sxs-lookup"><span data-stu-id="e9c2e-108">hello queries in this article use hello following sample `People` table:</span></span>

| <span data-ttu-id="e9c2e-109">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="e9c2e-109">PartitionKey</span></span> | <span data-ttu-id="e9c2e-110">RowKey</span><span class="sxs-lookup"><span data-stu-id="e9c2e-110">RowKey</span></span> | <span data-ttu-id="e9c2e-111">E-mail</span><span class="sxs-lookup"><span data-stu-id="e9c2e-111">Email</span></span> | <span data-ttu-id="e9c2e-112">Telefonní číslo</span><span class="sxs-lookup"><span data-stu-id="e9c2e-112">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e9c2e-113">Tuleňů grónských</span><span class="sxs-lookup"><span data-stu-id="e9c2e-113">Harp</span></span> | <span data-ttu-id="e9c2e-114">Walter</span><span class="sxs-lookup"><span data-stu-id="e9c2e-114">Walter</span></span> | Walter@contoso.com| <span data-ttu-id="e9c2e-115">425-555-0101</span><span class="sxs-lookup"><span data-stu-id="e9c2e-115">425-555-0101</span></span> |
| <span data-ttu-id="e9c2e-116">Smith</span><span class="sxs-lookup"><span data-stu-id="e9c2e-116">Smith</span></span> | <span data-ttu-id="e9c2e-117">Ben</span><span class="sxs-lookup"><span data-stu-id="e9c2e-117">Ben</span></span> | Ben@contoso.com| <span data-ttu-id="e9c2e-118">425-555-0102</span><span class="sxs-lookup"><span data-stu-id="e9c2e-118">425-555-0102</span></span> |
| <span data-ttu-id="e9c2e-119">Smith</span><span class="sxs-lookup"><span data-stu-id="e9c2e-119">Smith</span></span> | <span data-ttu-id="e9c2e-120">Jeff</span><span class="sxs-lookup"><span data-stu-id="e9c2e-120">Jeff</span></span> | Jeff@contoso.com| <span data-ttu-id="e9c2e-121">425-555-0104</span><span class="sxs-lookup"><span data-stu-id="e9c2e-121">425-555-0104</span></span> | 

<span data-ttu-id="e9c2e-122">Protože Azure Cosmos DB není kompatibilní s hello rozhraním API Azure Table storage, najdete v části [dotazování tabulky a entity] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) podrobnosti o tom, jak tooquery pomocí hello Tabulka rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-122">Because Azure Cosmos DB is compatible with hello Azure Table storage APIs, see [Querying Tables and Entities] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) for details on how tooquery by using hello Table API.</span></span> 

<span data-ttu-id="e9c2e-123">Další informace o možnosti hello premium, které nabízí Azure Cosmos DB najdete v tématu [Cosmos databázi Azure: Tabulka API](table-introduction.md) a [vývoj s hello rozhraní API pro tabulky v rozhraní .NET](tutorial-develop-table-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="e9c2e-123">For more information on hello premium capabilities that Azure Cosmos DB offers, see [Azure Cosmos DB: Table API](table-introduction.md) and [Develop with hello Table API in .NET](tutorial-develop-table-dotnet.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e9c2e-124">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e9c2e-124">Prerequisites</span></span>

<span data-ttu-id="e9c2e-125">Pro tyto dotazy toowork musíte mít účet Azure Cosmos DB a mít data entity v kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-125">For these queries toowork, you must have an Azure Cosmos DB account and have entity data in hello container.</span></span> <span data-ttu-id="e9c2e-126">Nemáte žádné těchto?</span><span class="sxs-lookup"><span data-stu-id="e9c2e-126">Don't have any of those?</span></span> <span data-ttu-id="e9c2e-127">Dokončení hello [rychlý start pětiminutovou](https://aka.ms/acdbtnetqs) nebo hello [vývojáře kurzu](https://aka.ms/acdbtabletut) toocreate účet a naplnit databázi.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-127">Complete hello [five-minute quickstart](https://aka.ms/acdbtnetqs) or hello [developer tutorial](https://aka.ms/acdbtabletut) toocreate an account and populate your database.</span></span>

## <a name="query-on-partitionkey-and-rowkey"></a><span data-ttu-id="e9c2e-128">Dotaz na klíč oddílu a RowKey</span><span class="sxs-lookup"><span data-stu-id="e9c2e-128">Query on PartitionKey and RowKey</span></span>
<span data-ttu-id="e9c2e-129">Důvodu, že vlastnosti PartitionKey a RowKey hello primární klíč entity, můžete použít následující zvláštní syntaxe tooidentify hello entity hello:</span><span class="sxs-lookup"><span data-stu-id="e9c2e-129">Because hello PartitionKey and RowKey properties form an entity's primary key, you can use hello following special syntax tooidentify hello entity:</span></span> 

<span data-ttu-id="e9c2e-130">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="e9c2e-130">**Query**</span></span>

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
<span data-ttu-id="e9c2e-131">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="e9c2e-131">**Results**</span></span>

| <span data-ttu-id="e9c2e-132">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="e9c2e-132">PartitionKey</span></span> | <span data-ttu-id="e9c2e-133">RowKey</span><span class="sxs-lookup"><span data-stu-id="e9c2e-133">RowKey</span></span> | <span data-ttu-id="e9c2e-134">E-mail</span><span class="sxs-lookup"><span data-stu-id="e9c2e-134">Email</span></span> | <span data-ttu-id="e9c2e-135">Telefonní číslo</span><span class="sxs-lookup"><span data-stu-id="e9c2e-135">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e9c2e-136">Tuleňů grónských</span><span class="sxs-lookup"><span data-stu-id="e9c2e-136">Harp</span></span> | <span data-ttu-id="e9c2e-137">Walter</span><span class="sxs-lookup"><span data-stu-id="e9c2e-137">Walter</span></span> | Walter@contoso.com| <span data-ttu-id="e9c2e-138">425-555-0104</span><span class="sxs-lookup"><span data-stu-id="e9c2e-138">425-555-0104</span></span> |

<span data-ttu-id="e9c2e-139">Alternativně můžete tyto vlastnosti jako součást hello `$filter` možnost, jak je znázorněno v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-139">Alternatively, you can specify these properties as part of hello `$filter` option, as shown in hello following section.</span></span> <span data-ttu-id="e9c2e-140">Všimněte si, že hello názvů vlastností klíče a hodnoty konstant jsou malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-140">Note that hello key property names and constant values are case-sensitive.</span></span> <span data-ttu-id="e9c2e-141">Hello PartitionKey i RowKey vlastnosti jsou typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-141">Both hello PartitionKey and RowKey properties are of type String.</span></span> 

## <a name="query-by-using-an-odata-filter"></a><span data-ttu-id="e9c2e-142">Dotazovat pomocí filtru OData</span><span class="sxs-lookup"><span data-stu-id="e9c2e-142">Query by using an OData filter</span></span>
<span data-ttu-id="e9c2e-143">Když jste vytváření řetězec filtru, berte v úvahu tato pravidla:</span><span class="sxs-lookup"><span data-stu-id="e9c2e-143">When you're constructing a filter string, keep these rules in mind:</span></span> 

* <span data-ttu-id="e9c2e-144">Logické operátory hello použití definované hello specifikace protokolu OData toocompare tooa hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-144">Use hello logical operators defined by hello OData Protocol Specification toocompare a property tooa value.</span></span> <span data-ttu-id="e9c2e-145">Všimněte si, že nelze porovnat hodnotu vlastnosti tooa dynamické.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-145">Note that you can't compare a property tooa dynamic value.</span></span> <span data-ttu-id="e9c2e-146">Jedna strana hello výrazu musí být konstanta.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-146">One side of hello expression must be a constant.</span></span> 
* <span data-ttu-id="e9c2e-147">Název vlastnosti Hello, operátor a hodnotu konstanty musí být oddělené mezerami kódovaná adresou URL.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-147">hello property name, operator, and constant value must be separated by URL-encoded spaces.</span></span> <span data-ttu-id="e9c2e-148">Mezeru je kódovaná jako adresa URL jako `%20`.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-148">A space is URL-encoded as `%20`.</span></span> 
* <span data-ttu-id="e9c2e-149">Všechny části řetězec filtru hello rozlišují velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-149">All parts of hello filter string are case-sensitive.</span></span> 
* <span data-ttu-id="e9c2e-150">Hello konstantní hodnota musí být hello stejný datový typ jako vlastnost hello v pořadí hello filtru tooreturn platný výsledků.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-150">hello constant value must be of hello same data type as hello property in order for hello filter tooreturn valid results.</span></span> <span data-ttu-id="e9c2e-151">Další informace o typech podporovaných vlastnost najdete v tématu [hello Principy datového modelu služby Table](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model).</span><span class="sxs-lookup"><span data-stu-id="e9c2e-151">For more information about supported property types, see [Understanding hello Table Service Data Model](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model).</span></span> 

<span data-ttu-id="e9c2e-152">Tady je příklad dotazu, který ukazuje, jak toofilter podle hello PartitionKey a e-mailu vlastnosti pomocí OData `$filter`.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-152">Here's an example query that shows how toofilter by hello PartitionKey and Email properties by using an OData `$filter`.</span></span>

<span data-ttu-id="e9c2e-153">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="e9c2e-153">**Query**</span></span>

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

<span data-ttu-id="e9c2e-154">Další informace o tom, jak filtrovat tooconstruct výrazy pro různé typy dat najdete v tématu [dotazování tabulky a entity](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).</span><span class="sxs-lookup"><span data-stu-id="e9c2e-154">For more information on how tooconstruct filter expressions for various data types, see [Querying Tables and Entities](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).</span></span>

<span data-ttu-id="e9c2e-155">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="e9c2e-155">**Results**</span></span>

| <span data-ttu-id="e9c2e-156">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="e9c2e-156">PartitionKey</span></span> | <span data-ttu-id="e9c2e-157">RowKey</span><span class="sxs-lookup"><span data-stu-id="e9c2e-157">RowKey</span></span> | <span data-ttu-id="e9c2e-158">E-mail</span><span class="sxs-lookup"><span data-stu-id="e9c2e-158">Email</span></span> | <span data-ttu-id="e9c2e-159">Telefonní číslo</span><span class="sxs-lookup"><span data-stu-id="e9c2e-159">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e9c2e-160">Ben</span><span class="sxs-lookup"><span data-stu-id="e9c2e-160">Ben</span></span> |<span data-ttu-id="e9c2e-161">Smith</span><span class="sxs-lookup"><span data-stu-id="e9c2e-161">Smith</span></span> | Ben@contoso.com| <span data-ttu-id="e9c2e-162">425-555-0102</span><span class="sxs-lookup"><span data-stu-id="e9c2e-162">425-555-0102</span></span> |

## <a name="query-by-using-linq"></a><span data-ttu-id="e9c2e-163">Dotazu pomocí LINQ</span><span class="sxs-lookup"><span data-stu-id="e9c2e-163">Query by using LINQ</span></span> 
<span data-ttu-id="e9c2e-164">Také můžete dotazovat pomocí LINQ, který překládá toohello odpovídající výrazy dotazu OData.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-164">You can also query by using LINQ, which translates toohello corresponding OData query expressions.</span></span> <span data-ttu-id="e9c2e-165">Tady je příklad jak hello toobuild dotazy pomocí .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="e9c2e-165">Here's an example of how toobuild queries by using hello .NET SDK:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="e9c2e-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e9c2e-166">Next steps</span></span>

<span data-ttu-id="e9c2e-167">V tomto kurzu provedete krok hello následující:</span><span class="sxs-lookup"><span data-stu-id="e9c2e-167">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e9c2e-168">Naučili, jak tooquery pomocí hello tabulky rozhraní API (preview)</span><span class="sxs-lookup"><span data-stu-id="e9c2e-168">Learned how tooquery by using hello Table API (preview)</span></span> 

<span data-ttu-id="e9c2e-169">Nyní můžete přejít toohello další kurz toolearn jak toodistribute data globálně.</span><span class="sxs-lookup"><span data-stu-id="e9c2e-169">You can now proceed toohello next tutorial toolearn how toodistribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e9c2e-170">Globálně distribuci dat</span><span class="sxs-lookup"><span data-stu-id="e9c2e-170">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)
