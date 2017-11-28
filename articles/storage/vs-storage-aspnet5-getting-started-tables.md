---
title: "aaaHow tooget začít s úložiště tabulek a Visual Studio připojených služeb (ASP.NET Core) | Microsoft Docs"
description: "Jak tooget pracovat s Azure Table storage v projektu ASP.NET Core v sadě Visual Studio po připojení tooa účet úložiště pomocí sady Visual Studio připojené služby"
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
ms.openlocfilehash: 6a8fb6aa085d78a087fcd14adbc765a0d59e0308
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-started-with-azure-table-storage-and-visual-studio-connected-services"></a><span data-ttu-id="9d95b-103">Jak tooget pracovat s Azure Table storage a Visual Studio připojených služeb</span><span class="sxs-lookup"><span data-stu-id="9d95b-103">How tooget started with Azure Table storage and Visual Studio connected services</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="9d95b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="9d95b-104">Overview</span></span>
<span data-ttu-id="9d95b-105">Tento článek popisuje, jak získat spuštění pomocí Azure Table storage v sadě Visual Studio po vytvoření a odkazuje pomocí účtu úložiště Azure v projektu ASP.NET Core hello Visual Studio **přidat připojení služby** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9d95b-105">This article describes how get started using Azure Table storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using hello Visual Studio **Add Connected Services** dialog.</span></span>

<span data-ttu-id="9d95b-106">Hello služba úložiště Azure Table umožňuje toostore velkých objemů strukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="9d95b-106">hello Azure Table storage service enables you toostore large amounts of structured data.</span></span> <span data-ttu-id="9d95b-107">Služba Hello je úložiště dat typu NoSQL, která přijímá ověřených volání z vnitřní a vnější hello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="9d95b-107">hello service is a NoSQL data store that accepts authenticated calls from inside and outside hello Azure cloud.</span></span> <span data-ttu-id="9d95b-108">Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat.</span><span class="sxs-lookup"><span data-stu-id="9d95b-108">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="9d95b-109">Hello **přidat připojení služby** operaci nainstaluje hello odpovídající NuGet balíčky tooaccess úložiště Azure ve vašem projektu a přidá hello připojovací řetězec pro hello úložiště účet tooyour projektu konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="9d95b-109">hello **Add Connected Services** operation installs hello appropriate NuGet packages tooaccess Azure storage in your project and adds hello connection string for hello storage account tooyour project configuration files.</span></span>

<span data-ttu-id="9d95b-110">Další obecné informace o používání Azure Table storage najdete v tématu [Začínáme s Azure Table storage pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="9d95b-110">For more general information about using Azure Table storage, see [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md).</span></span>

<span data-ttu-id="9d95b-111">tooget spustit, musíte nejprve toocreate tabulku ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9d95b-111">tooget started, you first need toocreate a table in your storage account.</span></span> <span data-ttu-id="9d95b-112">Ukážeme vám jak toocreate Azure tabulky v kódu.</span><span class="sxs-lookup"><span data-stu-id="9d95b-112">We'll show you how toocreate an Azure table in code.</span></span> <span data-ttu-id="9d95b-113">Také ukážeme, jak tooperform základní tabulky a entity operace, jako je přidání, úprava, čtení a čtení tabulky entity.</span><span class="sxs-lookup"><span data-stu-id="9d95b-113">We'll also show you how tooperform basic table and entity operations, such as adding, modifying, reading and reading table entities.</span></span> <span data-ttu-id="9d95b-114">Hello ukázky jsou napsané v jazyce C\# kód a použít hello Klientská knihovna pro úložiště Azure pro .NET.</span><span class="sxs-lookup"><span data-stu-id="9d95b-114">hello samples are written in C\# code and use hello Azure Storage Client Library for .NET.</span></span>

<span data-ttu-id="9d95b-115">**Poznámka:** -některé hello rozhraní API, která provádět volání out tooAzure úložiště v ASP.NET Core jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="9d95b-115">**NOTE** - Some of hello APIs that perform calls out tooAzure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="9d95b-116">V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="9d95b-116">See [Asynchronous Programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="9d95b-117">Následující kód Hello předpokládá, že asynchronní programování metody jsou používány.</span><span class="sxs-lookup"><span data-stu-id="9d95b-117">hello code below assumes Async programming methods are being used.</span></span>

## <a name="access-tables-in-code"></a><span data-ttu-id="9d95b-118">Přístup k tabulky v kódu</span><span class="sxs-lookup"><span data-stu-id="9d95b-118">Access tables in code</span></span>
<span data-ttu-id="9d95b-119">tooaccess tabulky v projektů ASP.NET Core, musíte tooinclude hello následující položky tooany C# zdrojové soubory, které přístupu k Azure table storage.</span><span class="sxs-lookup"><span data-stu-id="9d95b-119">tooaccess tables in ASP.NET Core projects, you need tooinclude hello following items tooany C# source files that access Azure table storage.</span></span>

1. <span data-ttu-id="9d95b-120">Deklarace oborů názvů hello hello horní části souboru hello C# zahrnout tyto **pomocí** příkazy.</span><span class="sxs-lookup"><span data-stu-id="9d95b-120">Make sure hello namespace declarations at hello top of hello C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="9d95b-121">Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9d95b-121">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="9d95b-122">Následující kód tooget hello použití hello připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure hello.</span><span class="sxs-lookup"><span data-stu-id="9d95b-122">Use hello following code tooget hello your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
   
    <span data-ttu-id="9d95b-123">**Poznámka:** -používat všechny hello výše kódu před hello kódu v hello následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="9d95b-123">**NOTE** - Use all of hello above code in front of hello code in hello following samples.</span></span>
3. <span data-ttu-id="9d95b-124">Získání **CloudTableClient** objektu tooreference hello tabulka objektů ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9d95b-124">Get a **CloudTableClient** object tooreference hello table objects in your storage account.</span></span>  
   
        // Create hello table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. <span data-ttu-id="9d95b-125">Získání **CloudTable** odkazovat na objekt tooreference určité tabulky a entity.</span><span class="sxs-lookup"><span data-stu-id="9d95b-125">Get a **CloudTable** reference object tooreference a specific table and entities.</span></span>
   
        // Get a reference tooa table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a><span data-ttu-id="9d95b-126">Vytvoří tabulku v kódu</span><span class="sxs-lookup"><span data-stu-id="9d95b-126">Create a table in code</span></span>
<span data-ttu-id="9d95b-127">toocreate hello tabulky Azure, stačí přidat volání příliš**CreateIfNotExistsAsync()**.</span><span class="sxs-lookup"><span data-stu-id="9d95b-127">toocreate hello Azure table, just add a call too**CreateIfNotExistsAsync()**.</span></span>

    // Create hello CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="9d95b-128">Přidat tooa tabulka entity</span><span class="sxs-lookup"><span data-stu-id="9d95b-128">Add an entity tooa table</span></span>
<span data-ttu-id="9d95b-129">tooadd tooa tabulka entity vytvořit třídu, která definuje hello vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="9d95b-129">tooadd an entity tooa table you create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="9d95b-130">Hello následující kód definuje třídu entity názvem **CustomerEntity** že hello používá jméno zákazníka jako klíč řádku hello a příjmení jako klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="9d95b-130">hello following code defines an entity class called **CustomerEntity** that uses hello customer's first name as hello row key and last name as hello partition key.</span></span>

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

<span data-ttu-id="9d95b-131">Operace s tabulkou zahrnující entity se provádějí pomocí hello **CloudTable** objektu jste vytvořili dříve v "Přístup tabulky v kódu".</span><span class="sxs-lookup"><span data-stu-id="9d95b-131">Table operations involving entities are done using hello **CloudTable** object you created earlier in "Access tables in code."</span></span> <span data-ttu-id="9d95b-132">Hello **TableOperation** objekt představuje toobe hello operaci provést.</span><span class="sxs-lookup"><span data-stu-id="9d95b-132">hello **TableOperation** object represents hello operation toobe done.</span></span> <span data-ttu-id="9d95b-133">Následující příklad ukazuje kód jak Hello toocreate **CloudTable** objektu a **CustomerEntity** objektu.</span><span class="sxs-lookup"><span data-stu-id="9d95b-133">hello following code example shows how toocreate a **CloudTable** object and a **CustomerEntity** object.</span></span> <span data-ttu-id="9d95b-134">operace hello tooprepare, **TableOperation** se vytvoří entitu zákazníka hello tooinsert do tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="9d95b-134">tooprepare hello operation, a **TableOperation** is created tooinsert hello customer entity into hello table.</span></span> <span data-ttu-id="9d95b-135">Nakonec hello operace provede voláním CloudTable.ExecuteAsync.</span><span class="sxs-lookup"><span data-stu-id="9d95b-135">Finally, hello operation is executed by calling CloudTable.ExecuteAsync.</span></span>

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create hello TableOperation that inserts hello customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute hello insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="9d95b-136">Vložení dávky entit</span><span class="sxs-lookup"><span data-stu-id="9d95b-136">Insert a batch of entities</span></span>
<span data-ttu-id="9d95b-137">Do tabulky v rámci jedné zápisu operace můžete vložit více entit.</span><span class="sxs-lookup"><span data-stu-id="9d95b-137">You can insert multiple entities into a table in a single write operation.</span></span> <span data-ttu-id="9d95b-138">Hello následující příklad kódu vytvoří dva objekty entity ("Jeff Smith" a "Ben Smith"), se přidají tooa **TableBatchOperation** objekt, který používá hello **vložit** metoda a poté spustí operaci hello podle volání metody CloudTable.ExecuteBatchAsync.</span><span class="sxs-lookup"><span data-stu-id="9d95b-138">hello following code example creates two entity objects ("Jeff Smith" and "Ben Smith"), adds them tooa **TableBatchOperation** object using hello **Insert** method, and then starts hello operation by calling CloudTable.ExecuteBatchAsync.</span></span>

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
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-hello-entities-in-a-partition"></a><span data-ttu-id="9d95b-139">Získání všech hello entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="9d95b-139">Get all of hello entities in a partition</span></span>
<span data-ttu-id="9d95b-140">tooquery tabulku pro všechny hello entit v oddílu, použití **TableQuery** objektu.</span><span class="sxs-lookup"><span data-stu-id="9d95b-140">tooquery a table for all of hello entities in a partition, use a **TableQuery** object.</span></span> <span data-ttu-id="9d95b-141">Hello následující příklad kódu určuje filtr pro entity, kde je: Váša' klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="9d95b-141">hello following code example specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="9d95b-142">Tento příklad zobrazí pole každé entity v konzole toohello výsledky dotazu hello hello.</span><span class="sxs-lookup"><span data-stu-id="9d95b-142">This example prints hello fields of each entity in hello query results toohello console.</span></span>

    // Construct hello query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print hello fields for each customer.
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

## <a name="get-a-single-entity"></a><span data-ttu-id="9d95b-143">Získat jedné entity</span><span class="sxs-lookup"><span data-stu-id="9d95b-143">Get a single entity</span></span>
<span data-ttu-id="9d95b-144">Můžete napsat dotaz tooget jedné konkrétní entity.</span><span class="sxs-lookup"><span data-stu-id="9d95b-144">You can write a query tooget a single, specific entity.</span></span> <span data-ttu-id="9d95b-145">Hello následující kód používá **TableOperation** objektu toospecify zákazník s názvem 'Ben Smith'.</span><span class="sxs-lookup"><span data-stu-id="9d95b-145">hello following code uses a **TableOperation** object toospecify a customer named 'Ben Smith'.</span></span> <span data-ttu-id="9d95b-146">Tato metoda vrátí místo kolekce pouze jednu entitu a hello vrácená hodnota v **TableResult.Result** je **CustomerEntity** objektu.</span><span class="sxs-lookup"><span data-stu-id="9d95b-146">This method returns just one entity, rather than a collection, and hello returned value in **TableResult.Result** is a **CustomerEntity** object.</span></span> <span data-ttu-id="9d95b-147">Určení klíče oddílu a řádku v dotazu je hello nejrychlejší způsob, jak tooretrieve jedné entity z hello **tabulky** služby.</span><span class="sxs-lookup"><span data-stu-id="9d95b-147">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello **Table** service.</span></span>

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute hello retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print hello phone number of hello result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("hello phone number could not be retrieved.");

## <a name="delete-an-entity"></a><span data-ttu-id="9d95b-148">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="9d95b-148">Delete an entity</span></span>
<span data-ttu-id="9d95b-149">Entitu můžete odstranit po ji najít.</span><span class="sxs-lookup"><span data-stu-id="9d95b-149">You can delete an entity after you find it.</span></span> <span data-ttu-id="9d95b-150">Hello následující kód vyhledá entitu zákazníka s názvem "Ben Smith" a pokud ho najde, se odstraní.</span><span class="sxs-lookup"><span data-stu-id="9d95b-150">hello following code looks for a customer entity named "Ben Smith" and if it finds it, it deletes it.</span></span>

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute hello operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign hello result tooa CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create hello Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute hello operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete hello entity.");

## <a name="next-steps"></a><span data-ttu-id="9d95b-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9d95b-151">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

