---
title: "Jak začít pracovat s úložiště tabulek a Visual Studio připojené služby (ASP.NET Core) | Microsoft Docs"
description: "Jak začít pracovat s Azure Table storage v projektu ASP.NET Core v sadě Visual Studio po připojení k účtu úložiště pomocí sady Visual Studio připojené služby"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: c3c451d1-71ff-4222-a348-c41c98a02b85
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: b64d4f7e55977c7ce144987f7600e5ddcb25596c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-get-started-with-azure-table-storage-and-visual-studio-connected-services"></a><span data-ttu-id="2b54b-103">Jak začít pracovat s Azure Table storage a Visual Studio připojené služby</span><span class="sxs-lookup"><span data-stu-id="2b54b-103">How to get started with Azure Table storage and Visual Studio connected services</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="2b54b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="2b54b-104">Overview</span></span>
<span data-ttu-id="2b54b-105">Tento článek popisuje, jak začít používat Azure Table storage v sadě Visual Studio, po vytvoření a odkazuje pomocí sady Visual Studio účet úložiště Azure v projektu ASP.NET Core **přidat připojení služby** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2b54b-105">This article describes how get started using Azure Table storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using the Visual Studio **Add Connected Services** dialog.</span></span>

<span data-ttu-id="2b54b-106">Služba úložiště Azure Table umožňuje ukládat velké množství strukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="2b54b-106">The Azure Table storage service enables you to store large amounts of structured data.</span></span> <span data-ttu-id="2b54b-107">Služba je úložiště dat typu NoSQL, která přijímá ověřených volání z uvnitř i vně cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="2b54b-107">The service is a NoSQL data store that accepts authenticated calls from inside and outside the Azure cloud.</span></span> <span data-ttu-id="2b54b-108">Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat.</span><span class="sxs-lookup"><span data-stu-id="2b54b-108">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="2b54b-109">**Přidat připojení služby** operaci nainstaluje příslušné balíčky NuGet pro přístup k úložišti Azure ve vašem projektu a přidá připojovací řetězec pro účet úložiště v konfiguračních souborech projektu.</span><span class="sxs-lookup"><span data-stu-id="2b54b-109">The **Add Connected Services** operation installs the appropriate NuGet packages to access Azure storage in your project and adds the connection string for the storage account to your project configuration files.</span></span>

<span data-ttu-id="2b54b-110">Další obecné informace o používání Azure Table storage najdete v tématu [Začínáme s Azure Table storage pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="2b54b-110">For more general information about using Azure Table storage, see [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md).</span></span>

<span data-ttu-id="2b54b-111">Abyste mohli začít, musíte nejprve vytvořit tabulku ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2b54b-111">To get started, you first need to create a table in your storage account.</span></span> <span data-ttu-id="2b54b-112">Ukážeme vám postup vytvoření Azure tabulky v kódu.</span><span class="sxs-lookup"><span data-stu-id="2b54b-112">We'll show you how to create an Azure table in code.</span></span> <span data-ttu-id="2b54b-113">Jsme budete také ukazují, jak provádět základní tabulky a entity operace, jako je přidání, úprava, čtení a čtení entity tabulky.</span><span class="sxs-lookup"><span data-stu-id="2b54b-113">We'll also show you how to perform basic table and entity operations, such as adding, modifying, reading and reading table entities.</span></span> <span data-ttu-id="2b54b-114">Ukázky jsou napsané v jazyce C\# kód a použít knihovnu klienta služby Azure Storage pro .NET.</span><span class="sxs-lookup"><span data-stu-id="2b54b-114">The samples are written in C\# code and use the Azure Storage Client Library for .NET.</span></span>

<span data-ttu-id="2b54b-115">**Poznámka:** -některé z rozhraní API, která provádět volání se do úložiště Azure v ASP.NET Core jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="2b54b-115">**NOTE** - Some of the APIs that perform calls out to Azure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="2b54b-116">V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="2b54b-116">See [Asynchronous Programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="2b54b-117">Následující kód předpokládá, že asynchronní programování metody jsou používány.</span><span class="sxs-lookup"><span data-stu-id="2b54b-117">The code below assumes Async programming methods are being used.</span></span>

## <a name="access-tables-in-code"></a><span data-ttu-id="2b54b-118">Přístup k tabulky v kódu</span><span class="sxs-lookup"><span data-stu-id="2b54b-118">Access tables in code</span></span>
<span data-ttu-id="2b54b-119">Chcete-li získat přístup k tabulky v projektů ASP.NET Core, zahrnují následující položky pro všechny C# zdrojové soubory, které přístupu k Azure table storage.</span><span class="sxs-lookup"><span data-stu-id="2b54b-119">To access tables in ASP.NET Core projects, you need to include the following items to any C# source files that access Azure table storage.</span></span>

1. <span data-ttu-id="2b54b-120">Zajistěte, aby deklarace oboru názvů v horní části souboru C# zahrnují tyto **pomocí** příkazy.</span><span class="sxs-lookup"><span data-stu-id="2b54b-120">Make sure the namespace declarations at the top of the C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="2b54b-121">Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2b54b-121">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="2b54b-122">Použít následující kód k získání připojovací řetězec úložiště a informace o účtu úložiště z konfigurace služby Azure.</span><span class="sxs-lookup"><span data-stu-id="2b54b-122">Use the following code to get the your storage connection string and storage account information from the Azure service configuration.</span></span>
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
   
    <span data-ttu-id="2b54b-123">**Poznámka:** -používat všechny výše uvedené kódu před kód v následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="2b54b-123">**NOTE** - Use all of the above code in front of the code in the following samples.</span></span>
3. <span data-ttu-id="2b54b-124">Získání **CloudTableClient** objekt, který má odkazovat na tabulku objekty v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2b54b-124">Get a **CloudTableClient** object to reference the table objects in your storage account.</span></span>  
   
        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. <span data-ttu-id="2b54b-125">Získání **CloudTable** odkaz na objekt, který chcete odkazovat na konkrétní tabulky a entity.</span><span class="sxs-lookup"><span data-stu-id="2b54b-125">Get a **CloudTable** reference object to reference a specific table and entities.</span></span>
   
        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a><span data-ttu-id="2b54b-126">Vytvoří tabulku v kódu</span><span class="sxs-lookup"><span data-stu-id="2b54b-126">Create a table in code</span></span>
<span data-ttu-id="2b54b-127">Pokud chcete vytvořit tabulky Azure, stačí přidat volání **CreateIfNotExistsAsync()**.</span><span class="sxs-lookup"><span data-stu-id="2b54b-127">To create the Azure table, just add a call to **CreateIfNotExistsAsync()**.</span></span>

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="2b54b-128">Přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="2b54b-128">Add an entity to a table</span></span>
<span data-ttu-id="2b54b-129">Chcete-li přidat entitu do tabulky vytvořte třídu, která definuje vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="2b54b-129">To add an entity to a table you create a class that defines the properties of your entity.</span></span> <span data-ttu-id="2b54b-130">Následující kód definuje třídu entity, která je volána **CustomerEntity** která používá jméno zákazníka jako klíč řádku a jeho příjmení jako klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="2b54b-130">The following code defines an entity class called **CustomerEntity** that uses the customer's first name as the row key and last name as the partition key.</span></span>

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

<span data-ttu-id="2b54b-131">Operace s tabulkou zahrnující entity se provádějí pomocí **CloudTable** objektu jste vytvořili dříve v "Přístup tabulky v kódu".</span><span class="sxs-lookup"><span data-stu-id="2b54b-131">Table operations involving entities are done using the **CloudTable** object you created earlier in "Access tables in code."</span></span> <span data-ttu-id="2b54b-132">**TableOperation** objekt představuje operaci provést.</span><span class="sxs-lookup"><span data-stu-id="2b54b-132">The **TableOperation** object represents the operation to be done.</span></span> <span data-ttu-id="2b54b-133">Následující příklad kódu ukazuje, jak vytvořit **CloudTable** objektu a **CustomerEntity** objektu.</span><span class="sxs-lookup"><span data-stu-id="2b54b-133">The following code example shows how to create a **CloudTable** object and a **CustomerEntity** object.</span></span> <span data-ttu-id="2b54b-134">Příprava operaci **TableOperation** se vytvoří pro vložení entity zákazníka do tabulky.</span><span class="sxs-lookup"><span data-stu-id="2b54b-134">To prepare the operation, a **TableOperation** is created to insert the customer entity into the table.</span></span> <span data-ttu-id="2b54b-135">Nakonec se operace provede voláním CloudTable.ExecuteAsync.</span><span class="sxs-lookup"><span data-stu-id="2b54b-135">Finally, the operation is executed by calling CloudTable.ExecuteAsync.</span></span>

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="2b54b-136">Vložení dávky entit</span><span class="sxs-lookup"><span data-stu-id="2b54b-136">Insert a batch of entities</span></span>
<span data-ttu-id="2b54b-137">Do tabulky v rámci jedné zápisu operace můžete vložit více entit.</span><span class="sxs-lookup"><span data-stu-id="2b54b-137">You can insert multiple entities into a table in a single write operation.</span></span> <span data-ttu-id="2b54b-138">Následující příklad kódu vytvoří dva objekty entity ("Jeff Smith" a "Ben Smith"), se přidají do **TableBatchOperation** pomocí **vložit** metoda, a pak spustí operaci voláním CloudTable.ExecuteBatchAsync.</span><span class="sxs-lookup"><span data-stu-id="2b54b-138">The following code example creates two entity objects ("Jeff Smith" and "Ben Smith"), adds them to a **TableBatchOperation** object using the **Insert** method, and then starts the operation by calling CloudTable.ExecuteBatchAsync.</span></span>

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
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a><span data-ttu-id="2b54b-139">Získání všech entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="2b54b-139">Get all of the entities in a partition</span></span>
<span data-ttu-id="2b54b-140">Dotaz na tabulku pro všechny entity v oddílu, použijte **TableQuery** objektu.</span><span class="sxs-lookup"><span data-stu-id="2b54b-140">To query a table for all of the entities in a partition, use a **TableQuery** object.</span></span> <span data-ttu-id="2b54b-141">Následující příklad kódu určuje filtr pro entity, kde Smith je klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="2b54b-141">The following code example specifies a filter for entities where 'Smith' is the partition key.</span></span> <span data-ttu-id="2b54b-142">Tento příklad zobrazí pole každé entity z výsledků dotazu z konzoly.</span><span class="sxs-lookup"><span data-stu-id="2b54b-142">This example prints the fields of each entity in the query results to the console.</span></span>

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

## <a name="get-a-single-entity"></a><span data-ttu-id="2b54b-143">Získat jedné entity</span><span class="sxs-lookup"><span data-stu-id="2b54b-143">Get a single entity</span></span>
<span data-ttu-id="2b54b-144">Můžete napsat dotaz pro získání jedné konkrétní entity.</span><span class="sxs-lookup"><span data-stu-id="2b54b-144">You can write a query to get a single, specific entity.</span></span> <span data-ttu-id="2b54b-145">Následující kód používá **TableOperation** objekt, který chcete zadat zákazníka s názvem 'Ben Smith'.</span><span class="sxs-lookup"><span data-stu-id="2b54b-145">The following code uses a **TableOperation** object to specify a customer named 'Ben Smith'.</span></span> <span data-ttu-id="2b54b-146">Tato metoda vrátí pouze jednu entitu, místo do kolekce a vrácenou hodnotou při **TableResult.Result** je **CustomerEntity** objektu.</span><span class="sxs-lookup"><span data-stu-id="2b54b-146">This method returns just one entity, rather than a collection, and the returned value in **TableResult.Result** is a **CustomerEntity** object.</span></span> <span data-ttu-id="2b54b-147">Určení klíče oddílu a řádku v dotazu je nejrychlejší způsob, jak načíst jednu entitu ze **tabulky** služby.</span><span class="sxs-lookup"><span data-stu-id="2b54b-147">Specifying both partition and row keys in a query is the fastest way to retrieve a single entity from the **Table** service.</span></span>

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a><span data-ttu-id="2b54b-148">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="2b54b-148">Delete an entity</span></span>
<span data-ttu-id="2b54b-149">Entitu můžete odstranit po ji najít.</span><span class="sxs-lookup"><span data-stu-id="2b54b-149">You can delete an entity after you find it.</span></span> <span data-ttu-id="2b54b-150">Následující kód vyhledá entitu zákazníka s názvem "Ben Smith" a pokud ho najde, se odstraní.</span><span class="sxs-lookup"><span data-stu-id="2b54b-150">The following code looks for a customer entity named "Ben Smith" and if it finds it, it deletes it.</span></span>

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a><span data-ttu-id="2b54b-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2b54b-151">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

