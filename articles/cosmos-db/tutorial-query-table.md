---
title: "Postup dotazování dat v tabulce v Azure Cosmos DB? | Dokumentace Microsoftu"
description: "Naučte se tabulka dotaz na data v Azure Cosmos DB"
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
ms.openlocfilehash: e59cfa85c6bf584e44bdc6e88cc19d67df390041
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-how-to-query-table-data-by-using-the-table-api-preview"></a><span data-ttu-id="29d17-104">Azure Cosmos DB: Jak dotazovat data tabulky pomocí rozhraní API tabulky (preview)?</span><span class="sxs-lookup"><span data-stu-id="29d17-104">Azure Cosmos DB: How to query table data by using the Table API (preview)?</span></span>

<span data-ttu-id="29d17-105">Azure Cosmos DB [tabulky API](table-introduction.md) (preview) podporuje OData a [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) dotazy na data klíč hodnota (tabulky).</span><span class="sxs-lookup"><span data-stu-id="29d17-105">The Azure Cosmos DB [Table API](table-introduction.md) (preview) supports OData and [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) queries against key/value (table) data.</span></span>  

<span data-ttu-id="29d17-106">Tento článek obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="29d17-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="29d17-107">Dotazování na data s rozhraním API pro tabulky</span><span class="sxs-lookup"><span data-stu-id="29d17-107">Querying data with the Table API</span></span>

<span data-ttu-id="29d17-108">Dotazy v tomto článku použijte následující příklad `People` tabulky:</span><span class="sxs-lookup"><span data-stu-id="29d17-108">The queries in this article use the following sample `People` table:</span></span>

| <span data-ttu-id="29d17-109">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="29d17-109">PartitionKey</span></span> | <span data-ttu-id="29d17-110">RowKey</span><span class="sxs-lookup"><span data-stu-id="29d17-110">RowKey</span></span> | <span data-ttu-id="29d17-111">E-mail</span><span class="sxs-lookup"><span data-stu-id="29d17-111">Email</span></span> | <span data-ttu-id="29d17-112">Telefonní číslo</span><span class="sxs-lookup"><span data-stu-id="29d17-112">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="29d17-113">Tuleňů grónských</span><span class="sxs-lookup"><span data-stu-id="29d17-113">Harp</span></span> | <span data-ttu-id="29d17-114">Walter</span><span class="sxs-lookup"><span data-stu-id="29d17-114">Walter</span></span> | Walter@contoso.com| <span data-ttu-id="29d17-115">425-555-0101</span><span class="sxs-lookup"><span data-stu-id="29d17-115">425-555-0101</span></span> |
| <span data-ttu-id="29d17-116">Smith</span><span class="sxs-lookup"><span data-stu-id="29d17-116">Smith</span></span> | <span data-ttu-id="29d17-117">Ben</span><span class="sxs-lookup"><span data-stu-id="29d17-117">Ben</span></span> | Ben@contoso.com| <span data-ttu-id="29d17-118">425-555-0102</span><span class="sxs-lookup"><span data-stu-id="29d17-118">425-555-0102</span></span> |
| <span data-ttu-id="29d17-119">Smith</span><span class="sxs-lookup"><span data-stu-id="29d17-119">Smith</span></span> | <span data-ttu-id="29d17-120">Jeff</span><span class="sxs-lookup"><span data-stu-id="29d17-120">Jeff</span></span> | Jeff@contoso.com| <span data-ttu-id="29d17-121">425-555-0104</span><span class="sxs-lookup"><span data-stu-id="29d17-121">425-555-0104</span></span> | 

<span data-ttu-id="29d17-122">Protože Azure Cosmos DB není kompatibilní s rozhraním API Azure Table storage, najdete v části [dotazování tabulky a entity] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) podrobnosti o tom, jak dotaz podle následující tabulky ROZHRANÍ API.</span><span class="sxs-lookup"><span data-stu-id="29d17-122">Because Azure Cosmos DB is compatible with the Azure Table storage APIs, see [Querying Tables and Entities] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) for details on how to query by using the Table API.</span></span> 

<span data-ttu-id="29d17-123">Další informace o možnosti premium, které nabízí Azure Cosmos DB najdete v tématu [Cosmos databázi Azure: Tabulka API](table-introduction.md) a [vývoj s rozhraním API pro tabulky v rozhraní .NET](tutorial-develop-table-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="29d17-123">For more information on the premium capabilities that Azure Cosmos DB offers, see [Azure Cosmos DB: Table API](table-introduction.md) and [Develop with the Table API in .NET](tutorial-develop-table-dotnet.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="29d17-124">Požadavky</span><span class="sxs-lookup"><span data-stu-id="29d17-124">Prerequisites</span></span>

<span data-ttu-id="29d17-125">Pro tyto dotazy pro práci musí mít účet Azure Cosmos DB a mít data entity v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="29d17-125">For these queries to work, you must have an Azure Cosmos DB account and have entity data in the container.</span></span> <span data-ttu-id="29d17-126">Nemáte žádné těchto?</span><span class="sxs-lookup"><span data-stu-id="29d17-126">Don't have any of those?</span></span> <span data-ttu-id="29d17-127">Dokončení [rychlý start pětiminutovou](https://aka.ms/acdbtnetqs) nebo [vývojáře kurzu](https://aka.ms/acdbtabletut) k vytvoření účtu a naplnit databázi.</span><span class="sxs-lookup"><span data-stu-id="29d17-127">Complete the [five-minute quickstart](https://aka.ms/acdbtnetqs) or the [developer tutorial](https://aka.ms/acdbtabletut) to create an account and populate your database.</span></span>

## <a name="query-on-partitionkey-and-rowkey"></a><span data-ttu-id="29d17-128">Dotaz na klíč oddílu a RowKey</span><span class="sxs-lookup"><span data-stu-id="29d17-128">Query on PartitionKey and RowKey</span></span>
<span data-ttu-id="29d17-129">Protože vlastnosti PartitionKey a RowKey formuláři primární klíč entity, můžete k identifikaci entity speciální syntaxe:</span><span class="sxs-lookup"><span data-stu-id="29d17-129">Because the PartitionKey and RowKey properties form an entity's primary key, you can use the following special syntax to identify the entity:</span></span> 

<span data-ttu-id="29d17-130">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="29d17-130">**Query**</span></span>

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
<span data-ttu-id="29d17-131">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="29d17-131">**Results**</span></span>

| <span data-ttu-id="29d17-132">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="29d17-132">PartitionKey</span></span> | <span data-ttu-id="29d17-133">RowKey</span><span class="sxs-lookup"><span data-stu-id="29d17-133">RowKey</span></span> | <span data-ttu-id="29d17-134">E-mail</span><span class="sxs-lookup"><span data-stu-id="29d17-134">Email</span></span> | <span data-ttu-id="29d17-135">Telefonní číslo</span><span class="sxs-lookup"><span data-stu-id="29d17-135">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="29d17-136">Tuleňů grónských</span><span class="sxs-lookup"><span data-stu-id="29d17-136">Harp</span></span> | <span data-ttu-id="29d17-137">Walter</span><span class="sxs-lookup"><span data-stu-id="29d17-137">Walter</span></span> | Walter@contoso.com| <span data-ttu-id="29d17-138">425-555-0104</span><span class="sxs-lookup"><span data-stu-id="29d17-138">425-555-0104</span></span> |

<span data-ttu-id="29d17-139">Alternativně můžete tyto vlastnosti v rámci `$filter` možnost, jak je znázorněno v následující části.</span><span class="sxs-lookup"><span data-stu-id="29d17-139">Alternatively, you can specify these properties as part of the `$filter` option, as shown in the following section.</span></span> <span data-ttu-id="29d17-140">Všimněte si, že názvů vlastností klíče a hodnoty konstant jsou malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="29d17-140">Note that the key property names and constant values are case-sensitive.</span></span> <span data-ttu-id="29d17-141">Vlastnosti PartitionKey i RowKey jsou typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="29d17-141">Both the PartitionKey and RowKey properties are of type String.</span></span> 

## <a name="query-by-using-an-odata-filter"></a><span data-ttu-id="29d17-142">Dotazovat pomocí filtru OData</span><span class="sxs-lookup"><span data-stu-id="29d17-142">Query by using an OData filter</span></span>
<span data-ttu-id="29d17-143">Když jste vytváření řetězec filtru, berte v úvahu tato pravidla:</span><span class="sxs-lookup"><span data-stu-id="29d17-143">When you're constructing a filter string, keep these rules in mind:</span></span> 

* <span data-ttu-id="29d17-144">Logické operátory definované specifikací protokolu OData slouží k porovnání vlastnosti a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="29d17-144">Use the logical operators defined by the OData Protocol Specification to compare a property to a value.</span></span> <span data-ttu-id="29d17-145">Všimněte si, že nelze porovnat vlastnost, která má dynamické hodnoty.</span><span class="sxs-lookup"><span data-stu-id="29d17-145">Note that you can't compare a property to a dynamic value.</span></span> <span data-ttu-id="29d17-146">Jedna strana výrazu musí být konstanta.</span><span class="sxs-lookup"><span data-stu-id="29d17-146">One side of the expression must be a constant.</span></span> 
* <span data-ttu-id="29d17-147">Název vlastnosti, operátor a hodnotu konstanty musí být odděleny prostory kódovaná adresou URL.</span><span class="sxs-lookup"><span data-stu-id="29d17-147">The property name, operator, and constant value must be separated by URL-encoded spaces.</span></span> <span data-ttu-id="29d17-148">Mezeru je kódovaná jako adresa URL jako `%20`.</span><span class="sxs-lookup"><span data-stu-id="29d17-148">A space is URL-encoded as `%20`.</span></span> 
* <span data-ttu-id="29d17-149">Všechny části řetězec filtru rozlišují velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="29d17-149">All parts of the filter string are case-sensitive.</span></span> 
* <span data-ttu-id="29d17-150">Hodnota konstanty musí být stejného typu dat jako vlastnost v pořadí pro filtr vracet výsledky platný.</span><span class="sxs-lookup"><span data-stu-id="29d17-150">The constant value must be of the same data type as the property in order for the filter to return valid results.</span></span> <span data-ttu-id="29d17-151">Další informace o typech podporovaných vlastnost najdete v tématu [Principy datového modelu služby Table](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model).</span><span class="sxs-lookup"><span data-stu-id="29d17-151">For more information about supported property types, see [Understanding the Table Service Data Model](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model).</span></span> 

<span data-ttu-id="29d17-152">Tady je příklad dotazu, který ukazuje, jak filtrovat podle vlastnosti PartitionKey a e-mailu pomocí OData `$filter`.</span><span class="sxs-lookup"><span data-stu-id="29d17-152">Here's an example query that shows how to filter by the PartitionKey and Email properties by using an OData `$filter`.</span></span>

<span data-ttu-id="29d17-153">**Dotaz**</span><span class="sxs-lookup"><span data-stu-id="29d17-153">**Query**</span></span>

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

<span data-ttu-id="29d17-154">Další informace o tom, jak vytvořit filtr výrazů pro různé typy dat najdete v tématu [dotazování tabulky a entity](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).</span><span class="sxs-lookup"><span data-stu-id="29d17-154">For more information on how to construct filter expressions for various data types, see [Querying Tables and Entities](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).</span></span>

<span data-ttu-id="29d17-155">**Výsledky**</span><span class="sxs-lookup"><span data-stu-id="29d17-155">**Results**</span></span>

| <span data-ttu-id="29d17-156">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="29d17-156">PartitionKey</span></span> | <span data-ttu-id="29d17-157">RowKey</span><span class="sxs-lookup"><span data-stu-id="29d17-157">RowKey</span></span> | <span data-ttu-id="29d17-158">E-mail</span><span class="sxs-lookup"><span data-stu-id="29d17-158">Email</span></span> | <span data-ttu-id="29d17-159">Telefonní číslo</span><span class="sxs-lookup"><span data-stu-id="29d17-159">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="29d17-160">Ben</span><span class="sxs-lookup"><span data-stu-id="29d17-160">Ben</span></span> |<span data-ttu-id="29d17-161">Smith</span><span class="sxs-lookup"><span data-stu-id="29d17-161">Smith</span></span> | Ben@contoso.com| <span data-ttu-id="29d17-162">425-555-0102</span><span class="sxs-lookup"><span data-stu-id="29d17-162">425-555-0102</span></span> |

## <a name="query-by-using-linq"></a><span data-ttu-id="29d17-163">Dotazu pomocí LINQ</span><span class="sxs-lookup"><span data-stu-id="29d17-163">Query by using LINQ</span></span> 
<span data-ttu-id="29d17-164">Také můžete dotazovat pomocí LINQ, což znamená, že je odpovídající výrazy dotazu OData.</span><span class="sxs-lookup"><span data-stu-id="29d17-164">You can also query by using LINQ, which translates to the corresponding OData query expressions.</span></span> <span data-ttu-id="29d17-165">Tady je příklad toho, jak vytvořit dotazy pomocí .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="29d17-165">Here's an example of how to build queries by using the .NET SDK:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="29d17-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29d17-166">Next steps</span></span>

<span data-ttu-id="29d17-167">V tomto kurzu jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="29d17-167">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="29d17-168">Dozvěděli, jak dotazovat pomocí rozhraní API tabulky (preview)</span><span class="sxs-lookup"><span data-stu-id="29d17-168">Learned how to query by using the Table API (preview)</span></span> 

<span data-ttu-id="29d17-169">Nyní můžete přejít k dalším kurzu se dozvíte, jak se bude distribuovat globální data.</span><span class="sxs-lookup"><span data-stu-id="29d17-169">You can now proceed to the next tutorial to learn how to distribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="29d17-170">Globálně distribuci dat</span><span class="sxs-lookup"><span data-stu-id="29d17-170">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)
