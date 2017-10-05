---
title: "Začínáme s úložiště tabulek a Visual Studio připojené služby (cloudové služby) | Microsoft Docs"
description: "Jak začít používat Azure Table storage v projektu cloudové služby v sadě Visual Studio po připojení k účtu úložiště pomocí sady Visual Studio připojené služby"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: a3a11ed8-ba7f-4193-912b-e555f5b72184
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 3208ddb1a1246a5ff25d272bfc7d8ba842348a36
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a><span data-ttu-id="60e4f-103">Začínáme s Azure table storage a Visual Studio připojené služby (projekty cloudových služeb)</span><span class="sxs-lookup"><span data-stu-id="60e4f-103">Getting started with Azure table storage and Visual Studio connected services (cloud services projects)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="60e4f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="60e4f-104">Overview</span></span>
<span data-ttu-id="60e4f-105">Tento článek popisuje, jak začít používat úložiště tabulek Azure v sadě Visual Studio, po vytvoření a účet úložiště Azure v projektu cloudové služby odkazuje pomocí sady Visual Studio **přidat připojení služby** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60e4f-105">This article describes how to get started using Azure table storage in Visual Studio after you have created or referenced an Azure storage account in a cloud services project by using the Visual Studio **Add Connected Services** dialog.</span></span> <span data-ttu-id="60e4f-106">**Přidat připojení služby** operaci nainstaluje příslušné balíčky NuGet pro přístup k úložišti Azure ve vašem projektu a přidá připojovací řetězec pro účet úložiště v konfiguračních souborech projektu.</span><span class="sxs-lookup"><span data-stu-id="60e4f-106">The **Add Connected Services** operation installs the appropriate NuGet packages to access Azure storage in your project and adds the connection string for the storage account to your project configuration files.</span></span>

<span data-ttu-id="60e4f-107">Služba úložiště Azure Table umožňuje ukládat velké množství strukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="60e4f-107">The Azure Table storage service enables you to store large amounts of structured data.</span></span> <span data-ttu-id="60e4f-108">Služba je úložištěm dat typu NoSQL, která přijímá ověřených volání z uvnitř i vně cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="60e4f-108">The service is a NoSQL datastore that accepts authenticated calls from inside and outside the Azure cloud.</span></span> <span data-ttu-id="60e4f-109">Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat.</span><span class="sxs-lookup"><span data-stu-id="60e4f-109">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="60e4f-110">Abyste mohli začít, musíte nejprve vytvořit tabulku ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="60e4f-110">To get started, you first need to create a table in your storage account.</span></span> <span data-ttu-id="60e4f-111">Ukážeme vám postup vytvoření Azure tabulky v kódu a jak provádět základní tabulky a entity operace, jako je přidání, úprava, čtení a čtení entity tabulky.</span><span class="sxs-lookup"><span data-stu-id="60e4f-111">We'll show you how to create an Azure table in code, and also how to perform basic table and entity operations, such as adding, modifying, reading and reading table entities.</span></span> <span data-ttu-id="60e4f-112">Ukázky jsou napsané v jazyce C\# kód a použít [Klientská knihovna pro úložiště Microsoft Azure pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="60e4f-112">The samples are written in C\# code and use the [Microsoft Azure Storage client library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="60e4f-113">**Poznámka:** některé z rozhraní API, které provádějí volání se do úložiště Azure jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="60e4f-113">**NOTE:** Some of the APIs that perform calls out to Azure storage are asynchronous.</span></span> <span data-ttu-id="60e4f-114">V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="60e4f-114">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="60e4f-115">Následující kód předpokládá, že asynchronní programování metody jsou používány.</span><span class="sxs-lookup"><span data-stu-id="60e4f-115">The code below assumes async programming methods are being used.</span></span>

* <span data-ttu-id="60e4f-116">V tématu [Začínáme s Azure Table storage pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md) Další informace o programu manipulace s tabulky.</span><span class="sxs-lookup"><span data-stu-id="60e4f-116">See [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md) for more information on programmatically manipulating tables.</span></span>
* <span data-ttu-id="60e4f-117">V tématu [úložiště dokumentace](https://azure.microsoft.com/documentation/services/storage/) obecné informace o službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="60e4f-117">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="60e4f-118">V tématu [cloudové služby dokumentaci](https://azure.microsoft.com/documentation/services/cloud-services/) obecné informace o cloudových služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="60e4f-118">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="60e4f-119">V tématu [ASP.NET](http://www.asp.net) Další informace o programování aplikací ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="60e4f-119">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

## <a name="access-tables-in-code"></a><span data-ttu-id="60e4f-120">Přístup k tabulky v kódu</span><span class="sxs-lookup"><span data-stu-id="60e4f-120">Access tables in code</span></span>
<span data-ttu-id="60e4f-121">Chcete-li získat přístup k tabulky v projekty cloudových služeb, zahrnují následující položky pro všechny C# zdrojové soubory, které přístupu k Azure table storage.</span><span class="sxs-lookup"><span data-stu-id="60e4f-121">To access tables in cloud service projects, you need to include the following items to any C# source files that access Azure table storage.</span></span>

1. <span data-ttu-id="60e4f-122">Zajistěte, aby deklarace oboru názvů v horní části souboru C# zahrnují tyto **pomocí** příkazy.</span><span class="sxs-lookup"><span data-stu-id="60e4f-122">Make sure the namespace declarations at the top of the C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="60e4f-123">Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="60e4f-123">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="60e4f-124">Použijte následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure.</span><span class="sxs-lookup"><span data-stu-id="60e4f-124">Use the following code to get the storage connection string and storage account information from the Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
   > [!NOTE]
   > <span data-ttu-id="60e4f-125">V následující ukázky použijte všechny výše uvedené kódu před kód.</span><span class="sxs-lookup"><span data-stu-id="60e4f-125">Use all of the above code in front of the code in the following samples.</span></span>
   > 
   > 
3. <span data-ttu-id="60e4f-126">Získání **CloudTableClient** objekt, který má odkazovat na tabulku objekty v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="60e4f-126">Get a **CloudTableClient** object to reference the table objects in your storage account.</span></span>
   
         // Create the table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. <span data-ttu-id="60e4f-127">Získání **CloudTable** odkaz na objekt, který chcete odkazovat na konkrétní tabulky a entity.</span><span class="sxs-lookup"><span data-stu-id="60e4f-127">Get a **CloudTable** reference object to reference a specific table and entities.</span></span>
   
        // Get a reference to a table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a><span data-ttu-id="60e4f-128">Vytvoří tabulku v kódu</span><span class="sxs-lookup"><span data-stu-id="60e4f-128">Create a table in code</span></span>
<span data-ttu-id="60e4f-129">K vytvoření tabulky Azure, stačí přidat volání **CreateIfNotExistsAsync** do můžete po načtení **CloudTable** objektu, jak je popsáno v části "Přístup k tabulkám v kódu".</span><span class="sxs-lookup"><span data-stu-id="60e4f-129">To create the Azure table, just add a call to **CreateIfNotExistsAsync** to the after you get a **CloudTable** object as described in the "Access tables in code" section.</span></span>

    // Create the CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="60e4f-130">Přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="60e4f-130">Add an entity to a table</span></span>
<span data-ttu-id="60e4f-131">Když budete chtít do tabulky přidat entitu, vytvořte třídu, která definuje vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="60e4f-131">To add an entity to a table, create a class that defines the properties of your entity.</span></span> <span data-ttu-id="60e4f-132">Následující kód definuje třídu entity, která je volána **CustomerEntity** která používá jméno zákazníka jako klíč řádku a příjmení jako klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="60e4f-132">The following code defines an entity class called **CustomerEntity** that uses the customer's first name as the row key and the last name as the partition key.</span></span>

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

<span data-ttu-id="60e4f-133">Operace s tabulkou zahrnující entity se provádějí pomocí **CloudTable** objekt, který jste vytvořili dříve v "Přístup tabulky v kódu".</span><span class="sxs-lookup"><span data-stu-id="60e4f-133">Table operations involving entities are done using the **CloudTable** object that you created earlier in "Access tables in code."</span></span> <span data-ttu-id="60e4f-134">**TableOperation** objekt představuje operaci provést.</span><span class="sxs-lookup"><span data-stu-id="60e4f-134">The **TableOperation** object represents the operation to be done.</span></span> <span data-ttu-id="60e4f-135">Následující příklad kódu ukazuje, jak vytvořit **CloudTable** objektu a **CustomerEntity** objektu.</span><span class="sxs-lookup"><span data-stu-id="60e4f-135">The following code example shows how to create a **CloudTable** object and a **CustomerEntity** object.</span></span> <span data-ttu-id="60e4f-136">Příprava operaci **TableOperation** se vytvoří pro vložení entity zákazníka do tabulky.</span><span class="sxs-lookup"><span data-stu-id="60e4f-136">To prepare the operation, a **TableOperation** is created to insert the customer entity into the table.</span></span> <span data-ttu-id="60e4f-137">Nakonec se operace provede voláním **CloudTable.ExecuteAsync**.</span><span class="sxs-lookup"><span data-stu-id="60e4f-137">Finally, the operation is executed by calling **CloudTable.ExecuteAsync**.</span></span>

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="60e4f-138">Vložení dávky entit</span><span class="sxs-lookup"><span data-stu-id="60e4f-138">Insert a batch of entities</span></span>
<span data-ttu-id="60e4f-139">Do tabulky v rámci jedné zápisu operace můžete vložit více entit.</span><span class="sxs-lookup"><span data-stu-id="60e4f-139">You can insert multiple entities into a table in a single write operation.</span></span> <span data-ttu-id="60e4f-140">Následující příklad kódu vytvoří dva objekty entity ("Jeff Smith" a "Ben Smith"), se přidají do **TableBatchOperation** metodou vložení objektu, a pak spustí operaci voláním **CloudTable.ExecuteBatchAsync**.</span><span class="sxs-lookup"><span data-stu-id="60e4f-140">The following code example creates two entity objects ("Jeff Smith" and "Ben Smith"), adds them to a **TableBatchOperation** object using the Insert method, and then starts the operation by calling **CloudTable.ExecuteBatchAsync**.</span></span>

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

## <a name="get-all-of-the-entities-in-a-partition"></a><span data-ttu-id="60e4f-141">Získání všech entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="60e4f-141">Get all of the entities in a partition</span></span>
<span data-ttu-id="60e4f-142">Dotaz na tabulku pro všechny entity v oddílu, použijte **TableQuery** objektu.</span><span class="sxs-lookup"><span data-stu-id="60e4f-142">To query a table for all of the entities in a partition, use a **TableQuery** object.</span></span> <span data-ttu-id="60e4f-143">Následující příklad kódu určuje filtr pro entity, kde Smith je klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="60e4f-143">The following code example specifies a filter for entities where 'Smith' is the partition key.</span></span> <span data-ttu-id="60e4f-144">Tento příklad zobrazí pole každé entity z výsledků dotazu z konzoly.</span><span class="sxs-lookup"><span data-stu-id="60e4f-144">This example prints the fields of each entity in the query results to the console.</span></span>

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

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

    return View();


## <a name="get-a-single-entity"></a><span data-ttu-id="60e4f-145">Získat jedné entity</span><span class="sxs-lookup"><span data-stu-id="60e4f-145">Get a single entity</span></span>
<span data-ttu-id="60e4f-146">Můžete napsat dotaz pro získání jedné konkrétní entity.</span><span class="sxs-lookup"><span data-stu-id="60e4f-146">You can write a query to get a single, specific entity.</span></span> <span data-ttu-id="60e4f-147">Následující kód používá **TableOperation** objekt, který chcete zadat zákazníka s názvem 'Ben Smith'.</span><span class="sxs-lookup"><span data-stu-id="60e4f-147">The following code uses a **TableOperation** object to specify a customer named 'Ben Smith'.</span></span> <span data-ttu-id="60e4f-148">Tato metoda vrátí pouze jednu entitu, místo do kolekce a vrácenou hodnotou při **TableResult.Result** je **CustomerEntity** objektu.</span><span class="sxs-lookup"><span data-stu-id="60e4f-148">This method returns just one entity, rather than a collection, and the returned value in **TableResult.Result** is a **CustomerEntity** object.</span></span> <span data-ttu-id="60e4f-149">Určení klíče oddílu a řádku v dotazu je nejrychlejší způsob, jak načíst jednu entitu ze **tabulky** služby.</span><span class="sxs-lookup"><span data-stu-id="60e4f-149">Specifying both partition and row keys in a query is the fastest way to retrieve a single entity from the **Table** service.</span></span>

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a><span data-ttu-id="60e4f-150">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="60e4f-150">Delete an entity</span></span>
<span data-ttu-id="60e4f-151">Entitu můžete odstranit po ji najít.</span><span class="sxs-lookup"><span data-stu-id="60e4f-151">You can delete an entity after you find it.</span></span> <span data-ttu-id="60e4f-152">Následující kód vyhledá entitu zákazníka s názvem "Ben Smith", a pokud ho najde, se odstraní.</span><span class="sxs-lookup"><span data-stu-id="60e4f-152">The following code looks for a customer entity named "Ben Smith", and if it finds it, it deletes it.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="60e4f-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60e4f-153">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

