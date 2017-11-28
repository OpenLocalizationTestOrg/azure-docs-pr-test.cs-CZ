---
title: "aaaGet začít s Azure Table storage pomocí rozhraní .NET | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu hello pomocí Azure Table storage, úložiště dat typu NoSQL."
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: fe46d883-7bed-49dd-980e-5c71df36adb3
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 04/10/2017
ms.author: mimig
ms.openlocfilehash: a3e9a4c6f6fd5e724535b86a3f99cd4c161de6de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-table-storage-using-net"></a><span data-ttu-id="d797b-103">Začínáme s úložištěm Azure Table pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="d797b-103">Get started with Azure Table storage using .NET</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

<span data-ttu-id="d797b-104">Azure Table storage je služba, která ukládá strukturované NoSQL úložiště dat v cloudu hello poskytnutí klíčů/atributů s návrhem.</span><span class="sxs-lookup"><span data-stu-id="d797b-104">Azure Table storage is a service that stores structured NoSQL data in hello cloud, providing a key/attribute store with a schemaless design.</span></span> <span data-ttu-id="d797b-105">Úložiště Table nemá schéma, proto je snadno tooadapt data podle hello potřebují měnícím vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d797b-105">Because Table storage is schemaless, it's easy tooadapt your data as hello needs of your application evolve.</span></span> <span data-ttu-id="d797b-106">Přístup k datům tooTable úložiště je rychlý a nákladově efektivní pro mnoho typů aplikací je obvykle nižší náklady než tradiční SQL pro podobné objemy dat.</span><span class="sxs-lookup"><span data-stu-id="d797b-106">Access tooTable storage data is fast and cost-effective for many types of applications, and is typically lower in cost than traditional SQL for similar volumes of data.</span></span>

<span data-ttu-id="d797b-107">Flexibilních datových sad aplikace tabulky úložiště toostore jako uživatelská data můžete použít pro webové aplikace, adresářů, informací o zařízení nebo jiných typů metadat, které vaše služba vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="d797b-107">You can use Table storage toostore flexible datasets like user data for web applications, address books, device information, or other types of metadata your service requires.</span></span> <span data-ttu-id="d797b-108">V tabulce můžete uložit libovolný počet entit a účet úložiště může obsahovat libovolný počet tabulek, až toohello limitu kapacity účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-108">You can store any number of entities in a table, and a storage account may contain any number of tables, up toohello capacity limit of hello storage account.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="d797b-109">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="d797b-109">About this tutorial</span></span>
<span data-ttu-id="d797b-110">Tento kurz ukazuje, jak toouse hello [Klientská knihovna pro úložiště Azure pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) v některé běžné scénáře Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="d797b-110">This tutorial shows you how toouse hello [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) in some common Azure Table storage scenarios.</span></span> <span data-ttu-id="d797b-111">Tyto scénáře jsou ilustrovány pomocí příkladu v jazyce C#, ve kterých se vytvoří a odstraní tabulka a také vkládají, aktualizují a odstraňují data v tabulce nebo se na ně zadávají dotazy.</span><span class="sxs-lookup"><span data-stu-id="d797b-111">These scenarios are presented using C# examples for creating and deleting a table, and inserting, updating, deleting, and querying table data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d797b-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d797b-112">Prerequisites</span></span>

<span data-ttu-id="d797b-113">Budete potřebovat hello následující toocomplete v tomto kurzu úspěšně:</span><span class="sxs-lookup"><span data-stu-id="d797b-113">You need hello following toocomplete this tutorial successfully:</span></span>

* [<span data-ttu-id="d797b-114">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d797b-114">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="d797b-115">Klientská knihovna Azure Storage pro .NET</span><span class="sxs-lookup"><span data-stu-id="d797b-115">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="d797b-116">Azure Configuration Manager for .NET</span><span class="sxs-lookup"><span data-stu-id="d797b-116">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [<span data-ttu-id="d797b-117">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d797b-117">Azure storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a><span data-ttu-id="d797b-118">Další ukázky</span><span class="sxs-lookup"><span data-stu-id="d797b-118">More samples</span></span>
<span data-ttu-id="d797b-119">Další příklady použití Table Storage najdete v článku [Začínáme s Azure Table Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="d797b-119">For additional examples using Table storage, see [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/).</span></span> <span data-ttu-id="d797b-120">Stáhněte hello ukázkovou aplikaci a potom ho spusťte nebo procházet kód hello na Githubu.</span><span class="sxs-lookup"><span data-stu-id="d797b-120">You can download hello sample application and run it, or browse hello code on GitHub.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="d797b-121">Přidání direktiv using</span><span class="sxs-lookup"><span data-stu-id="d797b-121">Add using directives</span></span>
<span data-ttu-id="d797b-122">Přidejte následující hello **pomocí** direktivy toohello začátek hello `Program.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="d797b-122">Add hello following **using** directives toohello top of hello `Program.cs` file:</span></span>

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types
```

### <a name="parse-hello-connection-string"></a><span data-ttu-id="d797b-123">Analyzovat hello připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="d797b-123">Parse hello connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-table-service-client"></a><span data-ttu-id="d797b-124">Vytvoření klienta služby Table hello</span><span class="sxs-lookup"><span data-stu-id="d797b-124">Create hello Table service client</span></span>
<span data-ttu-id="d797b-125">Hello [CloudTableClient] [ dotnet_CloudTableClient] třída umožňuje tooretrieve tabulky a entity, které jsou uložené ve službě Table storage.</span><span class="sxs-lookup"><span data-stu-id="d797b-125">hello [CloudTableClient][dotnet_CloudTableClient] class enables you tooretrieve tables and entities stored in Table storage.</span></span> <span data-ttu-id="d797b-126">Tady je klienta služby Table hello toocreate jedním ze způsobů:</span><span class="sxs-lookup"><span data-stu-id="d797b-126">Here's one way toocreate hello Table service client:</span></span>

```csharp
// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```

<span data-ttu-id="d797b-127">Teď je připraven toowrite kód, který načítá a zapisuje data tooTable úložiště.</span><span class="sxs-lookup"><span data-stu-id="d797b-127">Now you are ready toowrite code that reads data from and writes data tooTable storage.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="d797b-128">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="d797b-128">Create a table</span></span>
<span data-ttu-id="d797b-129">Tento příklad ukazuje, jak toocreate tabulky, pokud ještě neexistuje:</span><span class="sxs-lookup"><span data-stu-id="d797b-129">This example shows how toocreate a table if it does not already exist:</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Retrieve a reference toohello table.
CloudTable table = tableClient.GetTableReference("people");

// Create hello table if it doesn't exist.
table.CreateIfNotExists();
```

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="d797b-130">Přidat tooa tabulka entity</span><span class="sxs-lookup"><span data-stu-id="d797b-130">Add an entity tooa table</span></span>
<span data-ttu-id="d797b-131">Entity se mapují tooC # objekty pomocí vlastní třídy odvozené od [TableEntity][dotnet_TableEntity].</span><span class="sxs-lookup"><span data-stu-id="d797b-131">Entities map tooC# objects by using a custom class derived from [TableEntity][dotnet_TableEntity].</span></span> <span data-ttu-id="d797b-132">tooadd tooa tabulka entity, vytvořte třídu, která definuje hello vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="d797b-132">tooadd an entity tooa table, create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="d797b-133">Hello následující kód definuje třídu entity, která používá jméno hello zákazníka jako klíč řádku hello a příjmení jako klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-133">hello following code defines an entity class that uses hello customer's first name as hello row key and last name as hello partition key.</span></span> <span data-ttu-id="d797b-134">Společně oddílu a klíč řádku jeho jednoznačné identifikaci v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-134">Together, an entity's partition and row key uniquely identify it in hello table.</span></span> <span data-ttu-id="d797b-135">Entity se stejným klíčem oddílu můžete zadat dotaz rychleji než entity s různými hello oddílu klíče, ale používání různých klíčů oddílů umožňuje větší škálovatelnost paralelních operací.</span><span class="sxs-lookup"><span data-stu-id="d797b-135">Entities with hello same partition key can be queried faster than entities with different partition keys, but using diverse partition keys allows for greater scalability of parallel operations.</span></span> <span data-ttu-id="d797b-136">Ukládat do tabulek toobe entit musí být podporované typu, například odvozeného z hello [TableEntity] [ dotnet_TableEntity] třídy.</span><span class="sxs-lookup"><span data-stu-id="d797b-136">Entities toobe stored in tables must be of a supported type, for example derived from hello [TableEntity][dotnet_TableEntity] class.</span></span> <span data-ttu-id="d797b-137">Vlastnosti entity, které byste chtěli toostore v tabulce musí být veřejné vlastnosti typu hello a podporují získání a nastavení hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d797b-137">Entity properties you'd like toostore in a table must be public properties of hello type, and support both getting and setting of values.</span></span> <span data-ttu-id="d797b-138">Typ entity navíc *musí* zveřejňovat konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="d797b-138">Also, your entity type *must* expose a parameter-less constructor.</span></span>

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

<span data-ttu-id="d797b-139">Operace s tabulkou zahrnující entity se provádí prostřednictvím hello [CloudTable] [ dotnet_CloudTable] objekt, který jste předtím vytvořili v části "Vytvořit tabulku" hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-139">Table operations that involve entities are performed via hello [CloudTable][dotnet_CloudTable] object that you created earlier in hello "Create a table" section.</span></span> <span data-ttu-id="d797b-140">Hello toobe operaci provést, je reprezentována [TableOperation] [ dotnet_TableOperation] objektu.</span><span class="sxs-lookup"><span data-stu-id="d797b-140">hello operation toobe performed is represented by a [TableOperation][dotnet_TableOperation] object.</span></span> <span data-ttu-id="d797b-141">Hello následující příklad kódu ukazuje vytvoření hello hello [CloudTable] [ dotnet_CloudTable] objekt a potom **CustomerEntity** objektu.</span><span class="sxs-lookup"><span data-stu-id="d797b-141">hello following code example shows hello creation of hello [CloudTable][dotnet_CloudTable] object and then a **CustomerEntity** object.</span></span> <span data-ttu-id="d797b-142">operace hello tooprepare, [TableOperation] [ dotnet_TableOperation] do tabulky hello je entita zákazník hello tooinsert vytvořen objekt.</span><span class="sxs-lookup"><span data-stu-id="d797b-142">tooprepare hello operation, a [TableOperation][dotnet_TableOperation] object is created tooinsert hello customer entity into hello table.</span></span> <span data-ttu-id="d797b-143">Nakonec hello operace provede voláním [CloudTable][dotnet_CloudTable].[ Spuštění][dotnet_CloudTable_Execute].</span><span class="sxs-lookup"><span data-stu-id="d797b-143">Finally, hello operation is executed by calling [CloudTable][dotnet_CloudTable].[Execute][dotnet_CloudTable_Execute].</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create hello TableOperation object that inserts hello customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute hello insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="d797b-144">Vložení dávky entit</span><span class="sxs-lookup"><span data-stu-id="d797b-144">Insert a batch of entities</span></span>
<span data-ttu-id="d797b-145">V rámci jedné operace zápisu můžete do tabulky vložit dávku entit.</span><span class="sxs-lookup"><span data-stu-id="d797b-145">You can insert a batch of entities into a table in one write operation.</span></span> <span data-ttu-id="d797b-146">Několik dalších poznámek ohledně dávkových operací:</span><span class="sxs-lookup"><span data-stu-id="d797b-146">Some other notes on batch operations:</span></span>

* <span data-ttu-id="d797b-147">Můžete provádět aktualizace, odstranění a vložení v hello stejné jedna dávková operace.</span><span class="sxs-lookup"><span data-stu-id="d797b-147">You can perform updates, deletes, and inserts in hello same single batch operation.</span></span>
* <span data-ttu-id="d797b-148">Jedna dávková operace může obsahovat až too100 entity.</span><span class="sxs-lookup"><span data-stu-id="d797b-148">A single batch operation can include up too100 entities.</span></span>
* <span data-ttu-id="d797b-149">Všechny entity v jedné dávkové operaci musí mít hello stejným klíčem oddílu.</span><span class="sxs-lookup"><span data-stu-id="d797b-149">All entities in a single batch operation must have hello same partition key.</span></span>
* <span data-ttu-id="d797b-150">I když je možné tooperform dotazu jako dávkovou operaci, musí být hello jediná operace v dávce hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-150">While it is possible tooperform a query as a batch operation, it must be hello only operation in hello batch.</span></span>

<span data-ttu-id="d797b-151">Hello následující příklad kódu vytvoří dva objekty entity a přidá všechny příliš[TableBatchOperation] [ dotnet_TableBatchOperation] pomocí hello [vložit] [ dotnet_TableBatchOperation_Insert] Metoda.</span><span class="sxs-lookup"><span data-stu-id="d797b-151">hello following code example creates two entity objects and adds each too[TableBatchOperation][dotnet_TableBatchOperation] by using hello [Insert][dotnet_TableBatchOperation_Insert] method.</span></span> <span data-ttu-id="d797b-152">Potom [CloudTable][dotnet_CloudTable].[ ExecuteBatch] [ dotnet_CloudTable_ExecuteBatch] nazývá tooexecute hello operaci.</span><span class="sxs-lookup"><span data-stu-id="d797b-152">Then, [CloudTable][dotnet_CloudTable].[ExecuteBatch][dotnet_CloudTable_ExecuteBatch] is called tooexecute hello operation.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create hello batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it toohello table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it toohello table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities toohello batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute hello batch operation.
table.ExecuteBatch(batchOperation);
```

## <a name="retrieve-all-entities-in-a-partition"></a><span data-ttu-id="d797b-153">Načtení všech entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="d797b-153">Retrieve all entities in a partition</span></span>
<span data-ttu-id="d797b-154">tooquery tabulku pro všechny entity v oddílu, použití [TableQuery] [ dotnet_TableQuery] objektu.</span><span class="sxs-lookup"><span data-stu-id="d797b-154">tooquery a table for all entities in a partition, use a [TableQuery][dotnet_TableQuery] object.</span></span> <span data-ttu-id="d797b-155">Hello následující příklad kódu určuje filtr pro entity, kde je: Váša' klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-155">hello following code example specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="d797b-156">Tento příklad zobrazí pole každé entity v konzole toohello výsledky dotazu hello hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-156">This example prints hello fields of each entity in hello query results toohello console.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Construct hello query operation for all customer entities where PartitionKey="Smith".
TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

// Print hello fields for each customer.
foreach (CustomerEntity entity in table.ExecuteQuery(query))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-range-of-entities-in-a-partition"></a><span data-ttu-id="d797b-157">Načtení rozsahu entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="d797b-157">Retrieve a range of entities in a partition</span></span>
<span data-ttu-id="d797b-158">Pokud nechcete, aby tooquery všech entit v oddílu, můžete zadat rozsah kombinací hello filtru klíče oddílu s filtrem klíče řádku.</span><span class="sxs-lookup"><span data-stu-id="d797b-158">If you don't want tooquery all entities in a partition, you can specify a range by combining hello partition key filter with a row key filter.</span></span> <span data-ttu-id="d797b-159">Hello následující příklad kódu používá dva filtry tooget všech entit v oddílu "Smith, kde klíč řádku hello (jméno) začíná písmenem v hello abecedy před"E"a potom zobrazí výsledky dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-159">hello following code example uses two filters tooget all entities in partition 'Smith' where hello row key (first name) starts with a letter before 'E' in hello alphabet, then prints hello query results.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create hello table query.
TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

// Loop through hello results, displaying information about hello entity.
foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-single-entity"></a><span data-ttu-id="d797b-160">Načtení jedné entity</span><span class="sxs-lookup"><span data-stu-id="d797b-160">Retrieve a single entity</span></span>
<span data-ttu-id="d797b-161">Můžete napsat dotaz tooretrieve jedné konkrétní entity.</span><span class="sxs-lookup"><span data-stu-id="d797b-161">You can write a query tooretrieve a single, specific entity.</span></span> <span data-ttu-id="d797b-162">Hello následující kód používá [TableOperation] [ dotnet_TableOperation] toospecify hello zákazníka, Ben Smith'.</span><span class="sxs-lookup"><span data-stu-id="d797b-162">hello following code uses [TableOperation][dotnet_TableOperation] toospecify hello customer 'Ben Smith'.</span></span> <span data-ttu-id="d797b-163">Tato metoda vrátí pouze jednu entitu místo kolekce a hello vrácená hodnota v [TableResult][dotnet_TableResult].[ Výsledek] [ dotnet_TableResult_Result] je **CustomerEntity** objektu.</span><span class="sxs-lookup"><span data-stu-id="d797b-163">This method returns just one entity rather than a collection, and hello returned value in [TableResult][dotnet_TableResult].[Result][dotnet_TableResult_Result] is a **CustomerEntity** object.</span></span> <span data-ttu-id="d797b-164">Určení klíče oddílu a řádku v dotazu je hello nejrychlejší způsob, jak tooretrieve jednu entitu ze služby Table hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-164">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello Table service.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Print hello phone number of hello result.
if (retrievedResult.Result != null)
{
    Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
}
else
{
    Console.WriteLine("hello phone number could not be retrieved.");
}
```

## <a name="replace-an-entity"></a><span data-ttu-id="d797b-165">Nahrazení entity</span><span class="sxs-lookup"><span data-stu-id="d797b-165">Replace an entity</span></span>
<span data-ttu-id="d797b-166">tooupdate entity, načtěte ji ze služby Table hello, upravte objekt entity hello a potom uložte změny hello zpět toohello služby Table.</span><span class="sxs-lookup"><span data-stu-id="d797b-166">tooupdate an entity, retrieve it from hello Table service, modify hello entity object, and then save hello changes back toohello Table service.</span></span> <span data-ttu-id="d797b-167">Hello následující kód změní telefonní číslo stávajícího zákazníka.</span><span class="sxs-lookup"><span data-stu-id="d797b-167">hello following code changes an existing customer's phone number.</span></span> <span data-ttu-id="d797b-168">Namísto volání metody [Insert][dotnet_TableOperation_Insert] tento kód používá metodu [Replace][dotnet_TableOperation_Replace].</span><span class="sxs-lookup"><span data-stu-id="d797b-168">Instead of calling [Insert][dotnet_TableOperation_Insert], this code uses [Replace][dotnet_TableOperation_Replace].</span></span> <span data-ttu-id="d797b-169">[Nahraďte] [ dotnet_TableOperation_Replace] příčiny hello toobe entity na hello serveru plně nahradí, pokud došlo ke změně hello entit na serveru hello vzhledem k tomu, že byla načtena, v takovém případě hello se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="d797b-169">[Replace][dotnet_TableOperation_Replace] causes hello entity toobe fully replaced on hello server, unless hello entity on hello server has changed since it was retrieved, in which case hello operation will fail.</span></span> <span data-ttu-id="d797b-170">Tato chyba je tooprevent aplikace nechtěném přepsání změny provedené mezi hello načtení a aktualizací provedenou jinou součástí vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d797b-170">This failure is tooprevent your application from inadvertently overwriting a change made between hello retrieval and update by another component of your application.</span></span> <span data-ttu-id="d797b-171">Dobrý den správné zpracování této chyby je tooretrieve hello entita znovu, provedené změny (Pokud je stále platný) a pak provedete další [nahradit] [ dotnet_TableOperation_Replace] operaci.</span><span class="sxs-lookup"><span data-stu-id="d797b-171">hello proper handling of this failure is tooretrieve hello entity again, make your changes (if still valid), and then perform another [Replace][dotnet_TableOperation_Replace] operation.</span></span> <span data-ttu-id="d797b-172">Hello další části se dozvíte, jak toooverride toto chování.</span><span class="sxs-lookup"><span data-stu-id="d797b-172">hello next section will show you how toooverride this behavior.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign hello result tooa CustomerEntity object.
CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

if (updateEntity != null)
{
    // Change hello phone number.
    updateEntity.PhoneNumber = "425-555-0105";

    // Create hello Replace TableOperation.
    TableOperation updateOperation = TableOperation.Replace(updateEntity);

    // Execute hello operation.
    table.Execute(updateOperation);

    Console.WriteLine("Entity updated.");
}
else
{
    Console.WriteLine("Entity could not be retrieved.");
}
```

## <a name="insert-or-replace-an-entity"></a><span data-ttu-id="d797b-173">Vložení nebo nahrazení entity</span><span class="sxs-lookup"><span data-stu-id="d797b-173">Insert-or-replace an entity</span></span>
<span data-ttu-id="d797b-174">[Nahraďte] [ dotnet_TableOperation_Replace] operace se nezdaří, pokud hello entity se změnil od načtení ze serveru hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-174">[Replace][dotnet_TableOperation_Replace] operations will fail if hello entity has been changed since it was retrieved from hello server.</span></span> <span data-ttu-id="d797b-175">Kromě toho musí načíst hello entity ze serveru hello první v pořadí pro hello [nahradit] [ dotnet_TableOperation_Replace] toobe operace úspěšná.</span><span class="sxs-lookup"><span data-stu-id="d797b-175">Furthermore, you must retrieve hello entity from hello server first in order for hello [Replace][dotnet_TableOperation_Replace] operation toobe successful.</span></span> <span data-ttu-id="d797b-176">V některých případech ale nevíte Pokud hello entita existuje na serveru hello a hello aktuální hodnoty v ní uloženy jsou důležité.</span><span class="sxs-lookup"><span data-stu-id="d797b-176">Sometimes, however, you don't know if hello entity exists on hello server and hello current values stored in it are irrelevant.</span></span> <span data-ttu-id="d797b-177">Vaše aktualizace by je měla všechny přepsat.</span><span class="sxs-lookup"><span data-stu-id="d797b-177">Your update should overwrite them all.</span></span> <span data-ttu-id="d797b-178">tooaccomplish, byste použili [InsertOrReplace] [ dotnet_TableOperation_InsertOrReplace] operaci.</span><span class="sxs-lookup"><span data-stu-id="d797b-178">tooaccomplish this, you would use an [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] operation.</span></span> <span data-ttu-id="d797b-179">Tato operace vloží entitu hello, pokud neexistuje, nebo ji nahradí, pokud ano, bez ohledu na to, kdy byla provedena poslední aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-179">This operation inserts hello entity if it doesn't exist, or replaces it if it does, regardless of when hello last update was made.</span></span>

<span data-ttu-id="d797b-180">Entitu zákazníka pro 'Petr František' v hello následující ukázka kódu, je vytvořen a vloženy do tabulky "osoby" hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-180">In hello following code example, a customer entity for 'Fred Jones' is created and inserted into hello 'people' table.</span></span> <span data-ttu-id="d797b-181">V dalším kroku používáme hello [InsertOrReplace] [ dotnet_TableOperation_InsertOrReplace] operace toosave entity s hello stejný klíč oddílu (Petr) a řádek klíče serveru toohello (František), tentokrát s jinou hodnotou pro hello telefonní číslo Vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d797b-181">Next, we use hello [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] operation toosave an entity with hello same partition key (Jones) and row key (Fred) toohello server, this time with a different value for hello PhoneNumber property.</span></span> <span data-ttu-id="d797b-182">Vzhledem k tomu, že používáme operaci [InsertOrReplace][dotnet_TableOperation_InsertOrReplace], budou nahrazeny všechny příslušné hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d797b-182">Because we use [InsertOrReplace][dotnet_TableOperation_InsertOrReplace], all of its property values are replaced.</span></span> <span data-ttu-id="d797b-183">Ale pokud entity 'Petr František, kdyby již existuje v tabulce hello, ho by byly vloženy.</span><span class="sxs-lookup"><span data-stu-id="d797b-183">However, if a 'Fred Jones' entity hadn't already existed in hello table, it would have been inserted.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a customer entity.
CustomerEntity customer3 = new CustomerEntity("Jones", "Fred");
customer3.Email = "Fred@contoso.com";
customer3.PhoneNumber = "425-555-0106";

// Create hello TableOperation object that inserts hello customer entity.
TableOperation insertOperation = TableOperation.Insert(customer3);

// Execute hello operation.
table.Execute(insertOperation);

// Create another customer entity with hello same partition key and row key.
// We've already created a 'Fred Jones' entity and saved it toothe
// 'people' table, but here we're specifying a different value for the
// PhoneNumber property.
CustomerEntity customer4 = new CustomerEntity("Jones", "Fred");
customer4.Email = "Fred@contoso.com";
customer4.PhoneNumber = "425-555-0107";

// Create hello InsertOrReplace TableOperation.
TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(customer4);

// Execute hello operation. Because a 'Fred Jones' entity already exists in the
// 'people' table, its property values will be overwritten by those in this
// CustomerEntity. If 'Fred Jones' didn't already exist, hello entity would be
// added toohello table.
table.Execute(insertOrReplaceOperation);
```

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="d797b-184">Dotaz na podmnožinu vlastností entity</span><span class="sxs-lookup"><span data-stu-id="d797b-184">Query a subset of entity properties</span></span>
<span data-ttu-id="d797b-185">Dotaz na tabulku můžete načíst jenom několik z entity, místo všech vlastností entity hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-185">A table query can retrieve just a few properties from an entity instead of all hello entity properties.</span></span> <span data-ttu-id="d797b-186">Tato technika, které se říká projekce, snižuje šířku pásma a může zlepšit výkon dotazů, zejména u velkých entit.</span><span class="sxs-lookup"><span data-stu-id="d797b-186">This technique, called projection, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="d797b-187">dotaz Hello v hello následující kód vrátí pouze hello e-mailové adresy entit v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-187">hello query in hello following code returns only hello email addresses of entities in hello table.</span></span> <span data-ttu-id="d797b-188">To se provádí pomocí dotazu [DynamicTableEntity][dotnet_DynamicTableEntity] a také [EntityResolver][dotnet_EntityResolver].</span><span class="sxs-lookup"><span data-stu-id="d797b-188">This is done by using a query of [DynamicTableEntity][dotnet_DynamicTableEntity] and also [EntityResolver][dotnet_EntityResolver].</span></span> <span data-ttu-id="d797b-189">Další informace o projekce v hello [blogový příspěvek představení funkcí Upsert a projekce dotazu][blog_post_upsert].</span><span class="sxs-lookup"><span data-stu-id="d797b-189">You can learn more about projection in hello [Introducing Upsert and Query Projection blog post][blog_post_upsert].</span></span> <span data-ttu-id="d797b-190">Projekce nepodporuje emulátor úložiště hello, takže tento kód spustí pouze v případě, že používáte účet v služby Table hello.</span><span class="sxs-lookup"><span data-stu-id="d797b-190">Projection is not supported by hello storage emulator, so this code runs only when you're using an account in hello Table service.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Define hello query, and select only hello Email property.
TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

// Define an entity resolver toowork with hello entity after retrieval.
EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
{
    Console.WriteLine(projectedEmail);
}
```

## <a name="delete-an-entity"></a><span data-ttu-id="d797b-191">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="d797b-191">Delete an entity</span></span>
<span data-ttu-id="d797b-192">Po jejím načtení pomocí hello můžete snadno odstranit entity stejného vzoru zobrazovaného pro aktualizaci entity.</span><span class="sxs-lookup"><span data-stu-id="d797b-192">You can easily delete an entity after you have retrieved it by using hello same pattern shown for updating an entity.</span></span> <span data-ttu-id="d797b-193">Hello následující kód načte a odstraní entitu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="d797b-193">hello following code retrieves and deletes a customer entity.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that expects a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign hello result tooa CustomerEntity.
CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

// Create hello Delete TableOperation.
if (deleteEntity != null)
{
    TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

    // Execute hello operation.
    table.Execute(deleteOperation);

    Console.WriteLine("Entity deleted.");
}
else
{
    Console.WriteLine("Could not retrieve hello entity.");
}
```

## <a name="delete-a-table"></a><span data-ttu-id="d797b-194">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="d797b-194">Delete a table</span></span>
<span data-ttu-id="d797b-195">Hello následující příklad kódu nakonec odstraní tabulku z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d797b-195">Finally, hello following code example deletes a table from a storage account.</span></span> <span data-ttu-id="d797b-196">Tabulka, která byla odstraněna budou k dispozici toobe znovu vytvořit pro následující hello odstranění časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="d797b-196">A table that has been deleted will be unavailable toobe re-created for a period of time following hello deletion.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Delete hello table it if exists.
table.DeleteIfExists();
```

## <a name="retrieve-entities-in-pages-asynchronously"></a><span data-ttu-id="d797b-197">Asynchronní načítání entit na stránkách</span><span class="sxs-lookup"><span data-stu-id="d797b-197">Retrieve entities in pages asynchronously</span></span>
<span data-ttu-id="d797b-198">Pokud načítáte velký počet entit a chcete entity tooprocess nebo zobrazení, jako jsou načítány, místo, až se všechny tooreturn, můžete entity načíst pomocí segmentovaného dotazu.</span><span class="sxs-lookup"><span data-stu-id="d797b-198">If you are reading a large number of entities, and you want tooprocess/display entities as they are retrieved rather than waiting for them all tooreturn, you can retrieve entities by using a segmented query.</span></span> <span data-ttu-id="d797b-199">Tento příklad ukazuje, jak se pro velké sady výsledků tooreturn čekáte tooreturn výsledky na stránkách pomocí vzoru Async-Await hello tak, aby vám neblokovalo provádění.</span><span class="sxs-lookup"><span data-stu-id="d797b-199">This example shows how tooreturn results in pages by using hello Async-Await pattern so that execution is not blocked while you're waiting for a large set of results tooreturn.</span></span> <span data-ttu-id="d797b-200">Další podrobnosti o použití hello vzoru Async-Await v rozhraní .NET, najdete v části [asynchronní programování s Async a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).</span><span class="sxs-lookup"><span data-stu-id="d797b-200">For more details on using hello Async-Await pattern in .NET, see [Asynchronous programming with Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).</span></span>

```csharp
// Initialize a default TableQuery tooretrieve all hello entities in hello table.
TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

// Initialize hello continuation token toonull toostart from hello beginning of hello table.
TableContinuationToken continuationToken = null;

do
{
    // Retrieve a segment (up too1,000 entities).
    TableQuerySegment<CustomerEntity> tableQueryResult =
        await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

    // Assign hello new continuation token tootell hello service where to
    // continue on hello next iteration (or null if it has reached hello end).
    continuationToken = tableQueryResult.ContinuationToken;

    // Print hello number of rows retrieved.
    Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

// Loop until a null continuation token is received, indicating hello end of hello table.
} while(continuationToken != null);
```

## <a name="next-steps"></a><span data-ttu-id="d797b-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d797b-201">Next steps</span></span>
<span data-ttu-id="d797b-202">Teď, když jste se naučili základy hello služby Table storage, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště:</span><span class="sxs-lookup"><span data-stu-id="d797b-202">Now that you've learned hello basics of Table storage, follow these links toolearn about more complex storage tasks:</span></span>

* <span data-ttu-id="d797b-203">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="d797b-203">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="d797b-204">Další příklady Table Storage najdete v článku [Začínáme s Azure Table Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="d797b-204">See more Table storage samples in [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)</span></span>
* <span data-ttu-id="d797b-205">Zobrazení hello tabulky referenční dokumentaci ke službě kompletní informace o dostupných rozhraních API:</span><span class="sxs-lookup"><span data-stu-id="d797b-205">View hello Table service reference documentation for complete details about available APIs:</span></span>
* [<span data-ttu-id="d797b-206">Klientská knihovna pro úložiště pro .NET – referenční informace</span><span class="sxs-lookup"><span data-stu-id="d797b-206">Storage Client Library for .NET reference</span></span>](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
* [<span data-ttu-id="d797b-207">REST API – referenční informace</span><span class="sxs-lookup"><span data-stu-id="d797b-207">REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="d797b-208">Zjistěte, jak kód hello toosimplify napíšete toowork s Azure Storage pomocí hello [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="d797b-208">Learn how toosimplify hello code you write toowork with Azure Storage by using hello [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)</span></span>
* <span data-ttu-id="d797b-209">Zobrazte další funkce příručky toolearn o dalších možnostech pro ukládání dat v Azure.</span><span class="sxs-lookup"><span data-stu-id="d797b-209">View more feature guides toolearn about additional options for storing data in Azure.</span></span>
* <span data-ttu-id="d797b-210">[Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) toostore Nestrukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="d797b-210">[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) toostore unstructured data.</span></span>
* <span data-ttu-id="d797b-211">[Připojit tooSQL databáze pomocí rozhraní .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) toostore relační data.</span><span class="sxs-lookup"><span data-stu-id="d797b-211">[Connect tooSQL Database by using .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) toostore relational data.</span></span>

[Download and install hello Azure SDK for .NET]: /develop/net/
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
