---
title: "Začínáme s Azure Table Storage pomocí rozhraní .NET | Dokumentace Microsoftu"
description: "Ukládejte si strukturovaná data v cloudu pomocí Azure Table Storage, úložiště dat typu NoSQL."
services: storage
documentationcenter: .net
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: fe46d883-7bed-49dd-980e-5c71df36adb3
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 04/10/2017
ms.author: marsma
ms.openlocfilehash: 16a9dad1b01fdbef5ec8949bf9ff25497f33d994
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-azure-table-storage-using-net"></a><span data-ttu-id="5dd3c-103">Začínáme s úložištěm Azure Table pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="5dd3c-103">Get started with Azure Table storage using .NET</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

<span data-ttu-id="5dd3c-104">Azure Table Storage je služba, která ukládá strukturovaná data NoSQL do cloudu a poskytuje úložiště klíčů/atributů s návrhem bez použití schématu.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-104">Azure Table storage is a service that stores structured NoSQL data in the cloud, providing a key/attribute store with a schemaless design.</span></span> <span data-ttu-id="5dd3c-105">Vzhledem k tomu, že je Table Storage bez schématu, je snadné data přizpůsobovat měnícím se potřebám vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-105">Because Table storage is schemaless, it's easy to adapt your data as the needs of your application evolve.</span></span> <span data-ttu-id="5dd3c-106">Přístup k datům Table Storage je pro mnoho typů aplikací rychlý a nákladově efektivní a pro podobné objemy dat obvykle znamená nižší náklady než tradiční SQL.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-106">Access to Table storage data is fast and cost-effective for many types of applications, and is typically lower in cost than traditional SQL for similar volumes of data.</span></span>

<span data-ttu-id="5dd3c-107">Table Storage můžete používat k ukládání flexibilních datových sad, například uživatelských dat pro webové aplikace, adresářů, informací o zařízení nebo dalších typů metadat, které vaše služba vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-107">You can use Table storage to store flexible datasets like user data for web applications, address books, device information, or other types of metadata your service requires.</span></span> <span data-ttu-id="5dd3c-108">V tabulce můžete uložit libovolný počet entit a účet úložiště může obsahovat libovolný počet tabulek, až do limitu kapacity účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-108">You can store any number of entities in a table, and a storage account may contain any number of tables, up to the capacity limit of the storage account.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="5dd3c-109">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="5dd3c-109">About this tutorial</span></span>
<span data-ttu-id="5dd3c-110">V tomto kurzu se dozvíte, jak používat [klientskou knihovnu Azure Storage pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) v některých běžných scénářích Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-110">This tutorial shows you how to use the [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) in some common Azure Table storage scenarios.</span></span> <span data-ttu-id="5dd3c-111">Tyto scénáře jsou ilustrovány pomocí příkladu v jazyce C#, ve kterých se vytvoří a odstraní tabulka a také vkládají, aktualizují a odstraňují data v tabulce nebo se na ně zadávají dotazy.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-111">These scenarios are presented using C# examples for creating and deleting a table, and inserting, updating, deleting, and querying table data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5dd3c-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5dd3c-112">Prerequisites</span></span>

<span data-ttu-id="5dd3c-113">Pro úspěšné absolvování tohoto kurzu potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="5dd3c-113">You need the following to complete this tutorial successfully:</span></span>

* [<span data-ttu-id="5dd3c-114">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5dd3c-114">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="5dd3c-115">Klientská knihovna Azure Storage pro .NET</span><span class="sxs-lookup"><span data-stu-id="5dd3c-115">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="5dd3c-116">Azure Configuration Manager for .NET</span><span class="sxs-lookup"><span data-stu-id="5dd3c-116">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [<span data-ttu-id="5dd3c-117">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="5dd3c-117">Azure storage account</span></span>](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a><span data-ttu-id="5dd3c-118">Další ukázky</span><span class="sxs-lookup"><span data-stu-id="5dd3c-118">More samples</span></span>
<span data-ttu-id="5dd3c-119">Další příklady použití Table Storage najdete v článku [Začínáme s Azure Table Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="5dd3c-119">For additional examples using Table storage, see [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/).</span></span> <span data-ttu-id="5dd3c-120">Můžete si stáhnout a spustit ukázkovou aplikaci nebo si prohlédnout kód na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-120">You can download the sample application and run it, or browse the code on GitHub.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="5dd3c-121">Přidání direktiv using</span><span class="sxs-lookup"><span data-stu-id="5dd3c-121">Add using directives</span></span>
<span data-ttu-id="5dd3c-122">Na začátek souboru `Program.cs` přidejte následující direktivy **using**:</span><span class="sxs-lookup"><span data-stu-id="5dd3c-122">Add the following **using** directives to the top of the `Program.cs` file:</span></span>

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types
```

### <a name="parse-the-connection-string"></a><span data-ttu-id="5dd3c-123">Analýza připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="5dd3c-123">Parse the connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a><span data-ttu-id="5dd3c-124">Vytvoření klienta služby Table</span><span class="sxs-lookup"><span data-stu-id="5dd3c-124">Create the Table service client</span></span>
<span data-ttu-id="5dd3c-125">Třída [CloudTableClient][dotnet_CloudTableClient] vám umožňuje načítat tabulky a entity, které jsou uložené ve službě Table Storage.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-125">The [CloudTableClient][dotnet_CloudTableClient] class enables you to retrieve tables and entities stored in Table storage.</span></span> <span data-ttu-id="5dd3c-126">Jeden ze způsobů, jak vytvořit klienta služby Table:</span><span class="sxs-lookup"><span data-stu-id="5dd3c-126">Here's one way to create the Table service client:</span></span>

```csharp
// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```

<span data-ttu-id="5dd3c-127">Teď můžete napsat kód, který bude číst data z Table Storage a bude je tam také zapisovat.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-127">Now you are ready to write code that reads data from and writes data to Table storage.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="5dd3c-128">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="5dd3c-128">Create a table</span></span>
<span data-ttu-id="5dd3c-129">Tento příklad ukazuje, jak vytvořit tabulku, pokud ještě neexistuje:</span><span class="sxs-lookup"><span data-stu-id="5dd3c-129">This example shows how to create a table if it does not already exist:</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Retrieve a reference to the table.
CloudTable table = tableClient.GetTableReference("people");

// Create the table if it doesn't exist.
table.CreateIfNotExists();
```

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="5dd3c-130">Přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="5dd3c-130">Add an entity to a table</span></span>
<span data-ttu-id="5dd3c-131">Entity se mapují na objekty jazyka C# pomocí vlastní třídy odvozené z [TableEntity][dotnet_TableEntity].</span><span class="sxs-lookup"><span data-stu-id="5dd3c-131">Entities map to C# objects by using a custom class derived from [TableEntity][dotnet_TableEntity].</span></span> <span data-ttu-id="5dd3c-132">Když budete chtít do tabulky přidat entitu, vytvořte třídu, která definuje vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-132">To add an entity to a table, create a class that defines the properties of your entity.</span></span> <span data-ttu-id="5dd3c-133">Následující kód definuje třídu entity, která používá jméno zákazníka jako klíč řádku a jeho příjmení jako klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-133">The following code defines an entity class that uses the customer's first name as the row key and last name as the partition key.</span></span> <span data-ttu-id="5dd3c-134">V tabulce ji pak jednoznačně identifikuje kombinace klíče oddílu a řádku entity.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-134">Together, an entity's partition and row key uniquely identify it in the table.</span></span> <span data-ttu-id="5dd3c-135">Na entity se stejným klíčem oddílu je možné se (v porovnání s těmi, které mají různé klíče oddílů) rychleji dotazovat, ale používání různých klíčů oddílů umožňuje větší škálovatelnost paralelních operací.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-135">Entities with the same partition key can be queried faster than entities with different partition keys, but using diverse partition keys allows for greater scalability of parallel operations.</span></span> <span data-ttu-id="5dd3c-136">Entity, které se mají ukládat do tabulek, musí být podporovaného typu, například odvozené ze třídy [TableEntity][dotnet_TableEntity].</span><span class="sxs-lookup"><span data-stu-id="5dd3c-136">Entities to be stored in tables must be of a supported type, for example derived from the [TableEntity][dotnet_TableEntity] class.</span></span> <span data-ttu-id="5dd3c-137">Vlastnosti entity, které chcete uložit do tabulky, musí být veřejné vlastnosti typu a musí podporovat získávání i nastavování hodnot.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-137">Entity properties you'd like to store in a table must be public properties of the type, and support both getting and setting of values.</span></span> <span data-ttu-id="5dd3c-138">Typ entity navíc *musí* zveřejňovat konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-138">Also, your entity type *must* expose a parameter-less constructor.</span></span>

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

<span data-ttu-id="5dd3c-139">Operace s tabulkou zahrnující entity se provádí prostřednictvím objektu [CloudTable][dotnet_CloudTable], který jste vytvořili dříve v části Vytvoření tabulky.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-139">Table operations that involve entities are performed via the [CloudTable][dotnet_CloudTable] object that you created earlier in the "Create a table" section.</span></span> <span data-ttu-id="5dd3c-140">Operace, která se má provést, je reprezentovaná objektem [TableOperation][dotnet_TableOperation].</span><span class="sxs-lookup"><span data-stu-id="5dd3c-140">The operation to be performed is represented by a [TableOperation][dotnet_TableOperation] object.</span></span> <span data-ttu-id="5dd3c-141">Následující příklad kódu ukazuje vytvoření objektu [CloudTable][dotnet_CloudTable] a následně objektu **CustomerEntity**.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-141">The following code example shows the creation of the [CloudTable][dotnet_CloudTable] object and then a **CustomerEntity** object.</span></span> <span data-ttu-id="5dd3c-142">V rámci přípravy na operaci je vytvořen objekt [TableOperation][dotnet_TableOperation] pro vložení entity zákazníka do tabulky.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-142">To prepare the operation, a [TableOperation][dotnet_TableOperation] object is created to insert the customer entity into the table.</span></span> <span data-ttu-id="5dd3c-143">Nakonec se operace provede voláním metody [CloudTable][dotnet_CloudTable].[Execute][dotnet_CloudTable_Execute].</span><span class="sxs-lookup"><span data-stu-id="5dd3c-143">Finally, the operation is executed by calling [CloudTable][dotnet_CloudTable].[Execute][dotnet_CloudTable_Execute].</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="5dd3c-144">Vložení dávky entit</span><span class="sxs-lookup"><span data-stu-id="5dd3c-144">Insert a batch of entities</span></span>
<span data-ttu-id="5dd3c-145">V rámci jedné operace zápisu můžete do tabulky vložit dávku entit.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-145">You can insert a batch of entities into a table in one write operation.</span></span> <span data-ttu-id="5dd3c-146">Několik dalších poznámek ohledně dávkových operací:</span><span class="sxs-lookup"><span data-stu-id="5dd3c-146">Some other notes on batch operations:</span></span>

* <span data-ttu-id="5dd3c-147">V rámci jedné dávkové operace můžete provádět aktualizace, odstranění a vložení.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-147">You can perform updates, deletes, and inserts in the same single batch operation.</span></span>
* <span data-ttu-id="5dd3c-148">Jedna dávková operace může obsahovat až 100 entit.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-148">A single batch operation can include up to 100 entities.</span></span>
* <span data-ttu-id="5dd3c-149">Všechny entity v jedné dávkové operaci musí mít stejný klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-149">All entities in a single batch operation must have the same partition key.</span></span>
* <span data-ttu-id="5dd3c-150">Dotaz je sice také možné provést jako dávkovou operaci, musí to být ale jediná operace v dávce.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-150">While it is possible to perform a query as a batch operation, it must be the only operation in the batch.</span></span>

<span data-ttu-id="5dd3c-151">Následující příklad kódu vytvoří dva objekty entity a každý z nich přidá do [TableBatchOperation][dotnet_TableBatchOperation] pomocí metody [Insert][dotnet_TableBatchOperation_Insert].</span><span class="sxs-lookup"><span data-stu-id="5dd3c-151">The following code example creates two entity objects and adds each to [TableBatchOperation][dotnet_TableBatchOperation] by using the [Insert][dotnet_TableBatchOperation_Insert] method.</span></span> <span data-ttu-id="5dd3c-152">Pak se volá metoda [CloudTable][dotnet_CloudTable].[ExecuteBatch][dotnet_CloudTable_ExecuteBatch] pro provedení operace.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-152">Then, [CloudTable][dotnet_CloudTable].[ExecuteBatch][dotnet_CloudTable_ExecuteBatch] is called to execute the operation.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create the batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it to the table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it to the table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities to the batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute the batch operation.
table.ExecuteBatch(batchOperation);
```

## <a name="retrieve-all-entities-in-a-partition"></a><span data-ttu-id="5dd3c-153">Načtení všech entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="5dd3c-153">Retrieve all entities in a partition</span></span>
<span data-ttu-id="5dd3c-154">Pokud chcete zadat dotaz na tabulku pro všechny entity v oddílu, použijte objekt [TableQuery][dotnet_TableQuery].</span><span class="sxs-lookup"><span data-stu-id="5dd3c-154">To query a table for all entities in a partition, use a [TableQuery][dotnet_TableQuery] object.</span></span> <span data-ttu-id="5dd3c-155">Následující příklad kódu určuje filtr pro entity, kde Smith je klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-155">The following code example specifies a filter for entities where 'Smith' is the partition key.</span></span> <span data-ttu-id="5dd3c-156">Tento příklad zobrazí pole každé entity z výsledků dotazu z konzoly.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-156">This example prints the fields of each entity in the query results to the console.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Construct the query operation for all customer entities where PartitionKey="Smith".
TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

// Print the fields for each customer.
foreach (CustomerEntity entity in table.ExecuteQuery(query))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-range-of-entities-in-a-partition"></a><span data-ttu-id="5dd3c-157">Načtení rozsahu entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="5dd3c-157">Retrieve a range of entities in a partition</span></span>
<span data-ttu-id="5dd3c-158">Pokud nechcete, aby se zadával dotaz na všechny entity v oddílu, můžete určit rozsah nakombinováním filtru klíče oddílu s filtrem klíče řádku.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-158">If you don't want to query all entities in a partition, you can specify a range by combining the partition key filter with a row key filter.</span></span> <span data-ttu-id="5dd3c-159">Následující příklad kódu používá dva filtry k získání všech entit v oddílu Smith, kde klíč řádku (jméno) začíná písmenem abecedy před písmenem E, a potom zobrazí výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-159">The following code example uses two filters to get all entities in partition 'Smith' where the row key (first name) starts with a letter before 'E' in the alphabet, then prints the query results.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create the table query.
TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

// Loop through the results, displaying information about the entity.
foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-single-entity"></a><span data-ttu-id="5dd3c-160">Načtení jedné entity</span><span class="sxs-lookup"><span data-stu-id="5dd3c-160">Retrieve a single entity</span></span>
<span data-ttu-id="5dd3c-161">Můžete napsat dotaz pro načtení jedné konkrétní entity.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-161">You can write a query to retrieve a single, specific entity.</span></span> <span data-ttu-id="5dd3c-162">Následující kód používá objekt [TableOperation][dotnet_TableOperation] k určení zákazníka Ben Smith.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-162">The following code uses [TableOperation][dotnet_TableOperation] to specify the customer 'Ben Smith'.</span></span> <span data-ttu-id="5dd3c-163">Tato metoda vrátí místo kolekce pouze jednu entitu a vrácenou hodnotou při volání metody [TableResult][dotnet_TableResult].[Result][dotnet_TableResult_Result] je objekt **CustomerEntity**.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-163">This method returns just one entity rather than a collection, and the returned value in [TableResult][dotnet_TableResult].[Result][dotnet_TableResult_Result] is a **CustomerEntity** object.</span></span> <span data-ttu-id="5dd3c-164">Určení jak klíčů oddílu, tak klíčů řádků v dotazu představuje nejrychlejší způsob, jak načíst jednu entitu ze služby Table.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-164">Specifying both partition and row keys in a query is the fastest way to retrieve a single entity from the Table service.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Print the phone number of the result.
if (retrievedResult.Result != null)
{
    Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
}
else
{
    Console.WriteLine("The phone number could not be retrieved.");
}
```

## <a name="replace-an-entity"></a><span data-ttu-id="5dd3c-165">Nahrazení entity</span><span class="sxs-lookup"><span data-stu-id="5dd3c-165">Replace an entity</span></span>
<span data-ttu-id="5dd3c-166">Pokud chcete entitu aktualizovat, načtěte ji ze služby Table, upravte objekt entity a potom uložte změny zpět do služby Table.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-166">To update an entity, retrieve it from the Table service, modify the entity object, and then save the changes back to the Table service.</span></span> <span data-ttu-id="5dd3c-167">Následující kód změní telefonní číslo stávajícího zákazníka.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-167">The following code changes an existing customer's phone number.</span></span> <span data-ttu-id="5dd3c-168">Namísto volání metody [Insert][dotnet_TableOperation_Insert] tento kód používá metodu [Replace][dotnet_TableOperation_Replace].</span><span class="sxs-lookup"><span data-stu-id="5dd3c-168">Instead of calling [Insert][dotnet_TableOperation_Insert], this code uses [Replace][dotnet_TableOperation_Replace].</span></span> <span data-ttu-id="5dd3c-169">Metoda [Replace][dotnet_TableOperation_Replace] způsobí, že entita se na serveru plně nahradí, pokud se entita na serveru od načtení nezměnila, protože v takovém případě se operace nezdaří.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-169">[Replace][dotnet_TableOperation_Replace] causes the entity to be fully replaced on the server, unless the entity on the server has changed since it was retrieved, in which case the operation will fail.</span></span> <span data-ttu-id="5dd3c-170">Toto selhání zabrání vaší aplikaci v nechtěném přepsání změny provedené mezi načtením a aktualizací provedenou jinou součástí vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-170">This failure is to prevent your application from inadvertently overwriting a change made between the retrieval and update by another component of your application.</span></span> <span data-ttu-id="5dd3c-171">Toto selhání by se mělo správně zpracovat tak, že entitu znovu načtete, provedete požadované změny (pokud je stále ještě chcete provést) a pak provedete další operaci nahrazení ([Replace][dotnet_TableOperation_Replace]).</span><span class="sxs-lookup"><span data-stu-id="5dd3c-171">The proper handling of this failure is to retrieve the entity again, make your changes (if still valid), and then perform another [Replace][dotnet_TableOperation_Replace] operation.</span></span> <span data-ttu-id="5dd3c-172">V další části si ukážeme, jak toto chování potlačit.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-172">The next section will show you how to override this behavior.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign the result to a CustomerEntity object.
CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

if (updateEntity != null)
{
    // Change the phone number.
    updateEntity.PhoneNumber = "425-555-0105";

    // Create the Replace TableOperation.
    TableOperation updateOperation = TableOperation.Replace(updateEntity);

    // Execute the operation.
    table.Execute(updateOperation);

    Console.WriteLine("Entity updated.");
}
else
{
    Console.WriteLine("Entity could not be retrieved.");
}
```

## <a name="insert-or-replace-an-entity"></a><span data-ttu-id="5dd3c-173">Vložení nebo nahrazení entity</span><span class="sxs-lookup"><span data-stu-id="5dd3c-173">Insert-or-replace an entity</span></span>
<span data-ttu-id="5dd3c-174">Operace [Replace][dotnet_TableOperation_Replace] se nezdaří, pokud byla entita od načtení ze serveru změněna.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-174">[Replace][dotnet_TableOperation_Replace] operations will fail if the entity has been changed since it was retrieved from the server.</span></span> <span data-ttu-id="5dd3c-175">Kromě toho musíte entitu nejdřív načíst ze serveru, aby operace [Replace][dotnet_TableOperation_Replace] proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-175">Furthermore, you must retrieve the entity from the server first in order for the [Replace][dotnet_TableOperation_Replace] operation to be successful.</span></span> <span data-ttu-id="5dd3c-176">V některých případech ale nevíte, jestli entita existuje na serveru a jestli jsou hodnoty, které jsou v ní aktuálně uložené, relevantní.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-176">Sometimes, however, you don't know if the entity exists on the server and the current values stored in it are irrelevant.</span></span> <span data-ttu-id="5dd3c-177">Vaše aktualizace by je měla všechny přepsat.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-177">Your update should overwrite them all.</span></span> <span data-ttu-id="5dd3c-178">K tomu použijete operaci [InsertOrReplace][dotnet_TableOperation_InsertOrReplace].</span><span class="sxs-lookup"><span data-stu-id="5dd3c-178">To accomplish this, you would use an [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] operation.</span></span> <span data-ttu-id="5dd3c-179">Tato operace vloží entitu, pokud neexistuje, nebo ji nahradí, pokud existuje, a to bez ohledu na to, kdy byla provedena poslední aktualizace.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-179">This operation inserts the entity if it doesn't exist, or replaces it if it does, regardless of when the last update was made.</span></span>

<span data-ttu-id="5dd3c-180">V následujícím příkladu kódu se vytvoří entita zákazníka Fred Jones a vloží se do tabulky people.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-180">In the following code example, a customer entity for 'Fred Jones' is created and inserted into the 'people' table.</span></span> <span data-ttu-id="5dd3c-181">V dalším kroku použijeme operaci [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] k uložení entity se stejným klíčem oddílu (Jones) a klíčem řádku (Fred) na server, tentokrát s jinou hodnotou pro vlastnost PhoneNumber.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-181">Next, we use the [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] operation to save an entity with the same partition key (Jones) and row key (Fred) to the server, this time with a different value for the PhoneNumber property.</span></span> <span data-ttu-id="5dd3c-182">Vzhledem k tomu, že používáme operaci [InsertOrReplace][dotnet_TableOperation_InsertOrReplace], budou nahrazeny všechny příslušné hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-182">Because we use [InsertOrReplace][dotnet_TableOperation_InsertOrReplace], all of its property values are replaced.</span></span> <span data-ttu-id="5dd3c-183">Pokud by ovšem entita Fred Jones v tabulce neexistovala, vložila by se.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-183">However, if a 'Fred Jones' entity hadn't already existed in the table, it would have been inserted.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a customer entity.
CustomerEntity customer3 = new CustomerEntity("Jones", "Fred");
customer3.Email = "Fred@contoso.com";
customer3.PhoneNumber = "425-555-0106";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer3);

// Execute the operation.
table.Execute(insertOperation);

// Create another customer entity with the same partition key and row key.
// We've already created a 'Fred Jones' entity and saved it to the
// 'people' table, but here we're specifying a different value for the
// PhoneNumber property.
CustomerEntity customer4 = new CustomerEntity("Jones", "Fred");
customer4.Email = "Fred@contoso.com";
customer4.PhoneNumber = "425-555-0107";

// Create the InsertOrReplace TableOperation.
TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(customer4);

// Execute the operation. Because a 'Fred Jones' entity already exists in the
// 'people' table, its property values will be overwritten by those in this
// CustomerEntity. If 'Fred Jones' didn't already exist, the entity would be
// added to the table.
table.Execute(insertOrReplaceOperation);
```

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="5dd3c-184">Dotaz na podmnožinu vlastností entity</span><span class="sxs-lookup"><span data-stu-id="5dd3c-184">Query a subset of entity properties</span></span>
<span data-ttu-id="5dd3c-185">Dotaz na tabulku může místo všech vlastností entity načíst jenom několik z nich.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-185">A table query can retrieve just a few properties from an entity instead of all the entity properties.</span></span> <span data-ttu-id="5dd3c-186">Tato technika, které se říká projekce, snižuje šířku pásma a může zlepšit výkon dotazů, zejména u velkých entit.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-186">This technique, called projection, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="5dd3c-187">Dotaz v následujícím kódu vrátí pouze e-mailové adresy entit v tabulce.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-187">The query in the following code returns only the email addresses of entities in the table.</span></span> <span data-ttu-id="5dd3c-188">To se provádí pomocí dotazu [DynamicTableEntity][dotnet_DynamicTableEntity] a také [EntityResolver][dotnet_EntityResolver].</span><span class="sxs-lookup"><span data-stu-id="5dd3c-188">This is done by using a query of [DynamicTableEntity][dotnet_DynamicTableEntity] and also [EntityResolver][dotnet_EntityResolver].</span></span> <span data-ttu-id="5dd3c-189">Další informace o projekcích najdete v [blogovém příspěvku představení funkcí Upsert a projekce dotazu][blog_post_upsert].</span><span class="sxs-lookup"><span data-stu-id="5dd3c-189">You can learn more about projection in the [Introducing Upsert and Query Projection blog post][blog_post_upsert].</span></span> <span data-ttu-id="5dd3c-190">Emulátor úložiště projekci nepodporuje, takže tento kód bude možné spustit pouze v případě, že používáte účet ve službě Table.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-190">Projection is not supported by the storage emulator, so this code runs only when you're using an account in the Table service.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Define the query, and select only the Email property.
TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

// Define an entity resolver to work with the entity after retrieval.
EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
{
    Console.WriteLine(projectedEmail);
}
```

## <a name="delete-an-entity"></a><span data-ttu-id="5dd3c-191">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="5dd3c-191">Delete an entity</span></span>
<span data-ttu-id="5dd3c-192">Entitu můžete po jejím načtení snadno odstranit, a to pomocí stejného vzoru zobrazovaného pro aktualizaci entity.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-192">You can easily delete an entity after you have retrieved it by using the same pattern shown for updating an entity.</span></span> <span data-ttu-id="5dd3c-193">Následující kód načte a odstraní entitu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-193">The following code retrieves and deletes a customer entity.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that expects a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign the result to a CustomerEntity.
CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

// Create the Delete TableOperation.
if (deleteEntity != null)
{
    TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

    // Execute the operation.
    table.Execute(deleteOperation);

    Console.WriteLine("Entity deleted.");
}
else
{
    Console.WriteLine("Could not retrieve the entity.");
}
```

## <a name="delete-a-table"></a><span data-ttu-id="5dd3c-194">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="5dd3c-194">Delete a table</span></span>
<span data-ttu-id="5dd3c-195">Následující příklad kódu nakonec odstraní tabulku z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-195">Finally, the following code example deletes a table from a storage account.</span></span> <span data-ttu-id="5dd3c-196">Tabulku, která byla odstraněna, nebude možné po odstranění nějakou dobu znovu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-196">A table that has been deleted will be unavailable to be re-created for a period of time following the deletion.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Delete the table it if exists.
table.DeleteIfExists();
```

## <a name="retrieve-entities-in-pages-asynchronously"></a><span data-ttu-id="5dd3c-197">Asynchronní načítání entit na stránkách</span><span class="sxs-lookup"><span data-stu-id="5dd3c-197">Retrieve entities in pages asynchronously</span></span>
<span data-ttu-id="5dd3c-198">Pokud načítáte velký počet entit a chcete entity zpracovávat/zobrazovat tak, jak jsou načítány, a nečekat, až se všechny vrátí, můžete entity načíst pomocí segmentovaného dotazu.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-198">If you are reading a large number of entities, and you want to process/display entities as they are retrieved rather than waiting for them all to return, you can retrieve entities by using a segmented query.</span></span> <span data-ttu-id="5dd3c-199">Tento příklad ukazuje, jak vracet výsledky na stránkách pomocí vzoru Async-Await, aby čekání na vrácení velké sady výsledků neblokovalo provádění.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-199">This example shows how to return results in pages by using the Async-Await pattern so that execution is not blocked while you're waiting for a large set of results to return.</span></span> <span data-ttu-id="5dd3c-200">Další podrobnosti o použití vzoru Async-Await v rozhraní .NET najdete v tématu [Asynchronní programování s Async a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).</span><span class="sxs-lookup"><span data-stu-id="5dd3c-200">For more details on using the Async-Await pattern in .NET, see [Asynchronous programming with Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).</span></span>

```csharp
// Initialize a default TableQuery to retrieve all the entities in the table.
TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

// Initialize the continuation token to null to start from the beginning of the table.
TableContinuationToken continuationToken = null;

do
{
    // Retrieve a segment (up to 1,000 entities).
    TableQuerySegment<CustomerEntity> tableQueryResult =
        await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

    // Assign the new continuation token to tell the service where to
    // continue on the next iteration (or null if it has reached the end).
    continuationToken = tableQueryResult.ContinuationToken;

    // Print the number of rows retrieved.
    Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

// Loop until a null continuation token is received, indicating the end of the table.
} while(continuationToken != null);
```

## <a name="next-steps"></a><span data-ttu-id="5dd3c-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5dd3c-201">Next steps</span></span>
<span data-ttu-id="5dd3c-202">Teď, když jste se naučili základy používání služby Table Storage, podívejte se na následujících odkazech na další informace o složitějších úlohách úložiště:</span><span class="sxs-lookup"><span data-stu-id="5dd3c-202">Now that you've learned the basics of Table storage, follow these links to learn about more complex storage tasks:</span></span>

* <span data-ttu-id="5dd3c-203">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-203">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="5dd3c-204">Další příklady Table Storage najdete v článku [Začínáme s Azure Table Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="5dd3c-204">See more Table storage samples in [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)</span></span>
* <span data-ttu-id="5dd3c-205">Projděte si referenční dokumentaci ke službě Table, kde najdete úplné podrobnosti o dostupných rozhraních API:</span><span class="sxs-lookup"><span data-stu-id="5dd3c-205">View the Table service reference documentation for complete details about available APIs:</span></span>
* [<span data-ttu-id="5dd3c-206">Klientská knihovna Storage pro .NET – referenční informace</span><span class="sxs-lookup"><span data-stu-id="5dd3c-206">Storage Client Library for .NET reference</span></span>](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
* [<span data-ttu-id="5dd3c-207">REST API – referenční informace</span><span class="sxs-lookup"><span data-stu-id="5dd3c-207">REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="5dd3c-208">Zjistěte, jak můžete zjednodušit kód, který vytváříte, aby fungoval s Azure Storage, pomocí sady [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="5dd3c-208">Learn how to simplify the code you write to work with Azure Storage by using the [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)</span></span>
* <span data-ttu-id="5dd3c-209">Projděte si další průvodce funkcemi, kde najdete další informace o dalších možnostech pro ukládání dat v Azure.</span><span class="sxs-lookup"><span data-stu-id="5dd3c-209">View more feature guides to learn about additional options for storing data in Azure.</span></span>
* <span data-ttu-id="5dd3c-210">[Začínáme s Azure Blob Storage pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md) pro ukládání nestrukturovaných dat</span><span class="sxs-lookup"><span data-stu-id="5dd3c-210">[Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) to store unstructured data.</span></span>
* <span data-ttu-id="5dd3c-211">[Připojení k SQL Database s použitím rozhraní .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) pro uložení relačních dat</span><span class="sxs-lookup"><span data-stu-id="5dd3c-211">[Connect to SQL Database by using .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) to store relational data.</span></span>

[Download and install the Azure SDK for .NET]: /develop/net/
[Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

[blog_post_upsert]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx

[dotnet_api_ref]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[dotnet_CloudTableClient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtableclient.aspx
[dotnet_CloudTable]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.aspx
[dotnet_CloudTable_Execute]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.execute.aspx
[dotnet_CloudTable_ExecuteBatch]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.executebatch.aspx
[dotnet_DynamicTableEntity]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.dynamictableentity.aspx
[dotnet_EntityResolver]: https://msdn.microsoft.com/library/jj733144.aspx
[dotnet_TableBatchOperation]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx
[dotnet_TableBatchOperation_Insert]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.insert.aspx
[dotnet_TableEntity]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableentity.aspx
[dotnet_TableOperation]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.aspx
[dotnet_TableOperation_Insert]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.insert.aspx
[dotnet_TableOperation_InsertOrReplace]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.insertorreplace.aspx
[dotnet_TableOperation_Replace]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.replace.aspx
[dotnet_TableQuery]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablequery.aspx
[dotnet_TableResult]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableresult.aspx
[dotnet_TableResult_Result]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableresult.result.aspx
