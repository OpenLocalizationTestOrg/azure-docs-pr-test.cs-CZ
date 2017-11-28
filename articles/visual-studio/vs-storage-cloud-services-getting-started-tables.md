---
title: "aaaGet začít s úložiště tabulek a Visual Studio připojených služeb (cloudové služby) | Microsoft Docs"
description: "Jak tooget spustit po připojení tooa účet úložiště pomocí sady Visual Studio připojené služby pomocí Azure Table storage v projektu cloudové služby v sadě Visual Studio"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: a3a11ed8-ba7f-4193-912b-e555f5b72184
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 36da6ed4a12a3595e7234482e3040ecee8c33b8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a><span data-ttu-id="87873-103">Začínáme s Azure table storage a Visual Studio připojené služby (projekty cloudových služeb)</span><span class="sxs-lookup"><span data-stu-id="87873-103">Getting started with Azure table storage and Visual Studio connected services (cloud services projects)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="87873-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="87873-104">Overview</span></span>
<span data-ttu-id="87873-105">Tento článek popisuje, jak tooget spuštění pomocí úložiště tabulek Azure v sadě Visual Studio, po vytvoření a účet úložiště Azure v projektu cloudové služby odkazuje pomocí sady Visual Studio hello **přidat připojení služby** dialogové okno .</span><span class="sxs-lookup"><span data-stu-id="87873-105">This article describes how tooget started using Azure table storage in Visual Studio after you have created or referenced an Azure storage account in a cloud services project by using hello Visual Studio **Add Connected Services** dialog.</span></span> <span data-ttu-id="87873-106">Hello **přidat připojení služby** operaci nainstaluje hello odpovídající NuGet balíčky tooaccess úložiště Azure ve vašem projektu a přidá hello připojovací řetězec pro hello úložiště účet tooyour projektu konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="87873-106">hello **Add Connected Services** operation installs hello appropriate NuGet packages tooaccess Azure storage in your project and adds hello connection string for hello storage account tooyour project configuration files.</span></span>

<span data-ttu-id="87873-107">Hello služba úložiště Azure Table umožňuje toostore velkých objemů strukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="87873-107">hello Azure Table storage service enables you toostore large amounts of structured data.</span></span> <span data-ttu-id="87873-108">Hello služba je úložištěm dat typu NoSQL, která přijímá ověřených volání z vnitřní a vnější hello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="87873-108">hello service is a NoSQL datastore that accepts authenticated calls from inside and outside hello Azure cloud.</span></span> <span data-ttu-id="87873-109">Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat.</span><span class="sxs-lookup"><span data-stu-id="87873-109">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="87873-110">tooget spustit, musíte nejprve toocreate tabulku ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="87873-110">tooget started, you first need toocreate a table in your storage account.</span></span> <span data-ttu-id="87873-111">Ukážeme vám jak toocreate Azure tabulky v kódu, a také jak tooperform základní tabulky a entity operace, jako je přidání, úprava, čtení a čtení tabulky entity.</span><span class="sxs-lookup"><span data-stu-id="87873-111">We'll show you how toocreate an Azure table in code, and also how tooperform basic table and entity operations, such as adding, modifying, reading and reading table entities.</span></span> <span data-ttu-id="87873-112">Hello ukázky jsou napsané v jazyce C\# kód a použít hello [Klientská knihovna pro úložiště Microsoft Azure pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="87873-112">hello samples are written in C\# code and use hello [Microsoft Azure Storage client library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="87873-113">**Poznámka:** některé hello rozhraní API, která provádět volání out tooAzure úložiště jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="87873-113">**NOTE:** Some of hello APIs that perform calls out tooAzure storage are asynchronous.</span></span> <span data-ttu-id="87873-114">V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="87873-114">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="87873-115">Následující kód Hello předpokládá, že asynchronní programování metody jsou používány.</span><span class="sxs-lookup"><span data-stu-id="87873-115">hello code below assumes async programming methods are being used.</span></span>

* <span data-ttu-id="87873-116">V tématu [Začínáme s Azure Table storage pomocí rozhraní .NET](../storage/storage-dotnet-how-to-use-tables.md) Další informace o programu manipulace s tabulky.</span><span class="sxs-lookup"><span data-stu-id="87873-116">See [Get started with Azure Table storage using .NET](../storage/storage-dotnet-how-to-use-tables.md) for more information on programmatically manipulating tables.</span></span>
* <span data-ttu-id="87873-117">V tématu [úložiště dokumentace](https://azure.microsoft.com/documentation/services/storage/) obecné informace o službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="87873-117">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="87873-118">V tématu [cloudové služby dokumentaci](https://azure.microsoft.com/documentation/services/cloud-services/) obecné informace o cloudových služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="87873-118">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="87873-119">V tématu [ASP.NET](http://www.asp.net) Další informace o programování aplikací ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="87873-119">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

## <a name="access-tables-in-code"></a><span data-ttu-id="87873-120">Přístup k tabulky v kódu</span><span class="sxs-lookup"><span data-stu-id="87873-120">Access tables in code</span></span>
<span data-ttu-id="87873-121">tooaccess tabulky v projekty cloudových služeb, je nutné tooinclude hello následující položky tooany C# zdrojové soubory, které přístupu k Azure table storage.</span><span class="sxs-lookup"><span data-stu-id="87873-121">tooaccess tables in cloud service projects, you need tooinclude hello following items tooany C# source files that access Azure table storage.</span></span>

1. <span data-ttu-id="87873-122">Deklarace oborů názvů hello hello horní části souboru hello C# zahrnout tyto **pomocí** příkazy.</span><span class="sxs-lookup"><span data-stu-id="87873-122">Make sure hello namespace declarations at hello top of hello C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="87873-123">Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="87873-123">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="87873-124">Použijte následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello hello.</span><span class="sxs-lookup"><span data-stu-id="87873-124">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
   > [!NOTE]
   > <span data-ttu-id="87873-125">Použijte všechny hello výše kódu před hello kódu v hello následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="87873-125">Use all of hello above code in front of hello code in hello following samples.</span></span>
   > 
   > 
3. <span data-ttu-id="87873-126">Získání **CloudTableClient** objektu tooreference hello tabulka objektů ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="87873-126">Get a **CloudTableClient** object tooreference hello table objects in your storage account.</span></span>
   
         // Create hello table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. <span data-ttu-id="87873-127">Získání **CloudTable** odkazovat na objekt tooreference určité tabulky a entity.</span><span class="sxs-lookup"><span data-stu-id="87873-127">Get a **CloudTable** reference object tooreference a specific table and entities.</span></span>
   
        // Get a reference tooa table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a><span data-ttu-id="87873-128">Vytvoří tabulku v kódu</span><span class="sxs-lookup"><span data-stu-id="87873-128">Create a table in code</span></span>
<span data-ttu-id="87873-129">toocreate hello tabulky Azure, stačí přidat volání příliš**CreateIfNotExistsAsync** toohello můžete po načtení **CloudTable** objektu, jak je popsáno v části "Přístup tabulky v kódu" hello.</span><span class="sxs-lookup"><span data-stu-id="87873-129">toocreate hello Azure table, just add a call too**CreateIfNotExistsAsync** toohello after you get a **CloudTable** object as described in hello "Access tables in code" section.</span></span>

    // Create hello CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="87873-130">Přidat tooa tabulka entity</span><span class="sxs-lookup"><span data-stu-id="87873-130">Add an entity tooa table</span></span>
<span data-ttu-id="87873-131">tooadd tooa tabulka entity, vytvořte třídu, která definuje hello vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="87873-131">tooadd an entity tooa table, create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="87873-132">Hello následující kód definuje třídu entity názvem **CustomerEntity** že hello používá jméno zákazníka jako klíč řádku hello a hello příjmení jako klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="87873-132">hello following code defines an entity class called **CustomerEntity** that uses hello customer's first name as hello row key and hello last name as hello partition key.</span></span>

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

<span data-ttu-id="87873-133">Operace s tabulkou zahrnující entity se provádějí pomocí hello **CloudTable** objekt, který jste vytvořili dříve v "Přístup tabulky v kódu".</span><span class="sxs-lookup"><span data-stu-id="87873-133">Table operations involving entities are done using hello **CloudTable** object that you created earlier in "Access tables in code."</span></span> <span data-ttu-id="87873-134">Hello **TableOperation** objekt představuje toobe hello operaci provést.</span><span class="sxs-lookup"><span data-stu-id="87873-134">hello **TableOperation** object represents hello operation toobe done.</span></span> <span data-ttu-id="87873-135">Následující příklad ukazuje kód jak Hello toocreate **CloudTable** objektu a **CustomerEntity** objektu.</span><span class="sxs-lookup"><span data-stu-id="87873-135">hello following code example shows how toocreate a **CloudTable** object and a **CustomerEntity** object.</span></span> <span data-ttu-id="87873-136">operace hello tooprepare, **TableOperation** se vytvoří entitu zákazníka hello tooinsert do tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="87873-136">tooprepare hello operation, a **TableOperation** is created tooinsert hello customer entity into hello table.</span></span> <span data-ttu-id="87873-137">Nakonec hello operace provede voláním **CloudTable.ExecuteAsync**.</span><span class="sxs-lookup"><span data-stu-id="87873-137">Finally, hello operation is executed by calling **CloudTable.ExecuteAsync**.</span></span>

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create hello TableOperation that inserts hello customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute hello insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="87873-138">Vložení dávky entit</span><span class="sxs-lookup"><span data-stu-id="87873-138">Insert a batch of entities</span></span>
<span data-ttu-id="87873-139">Do tabulky v rámci jedné zápisu operace můžete vložit více entit.</span><span class="sxs-lookup"><span data-stu-id="87873-139">You can insert multiple entities into a table in a single write operation.</span></span> <span data-ttu-id="87873-140">Hello následující příklad kódu vytvoří dva objekty entity ("Jeff Smith" a "Ben Smith"), se přidají tooa **TableBatchOperation** objektu pomocí hello metoda vložení, a pak spustí hello operaci voláním  **CloudTable.ExecuteBatchAsync**.</span><span class="sxs-lookup"><span data-stu-id="87873-140">hello following code example creates two entity objects ("Jeff Smith" and "Ben Smith"), adds them tooa **TableBatchOperation** object using hello Insert method, and then starts hello operation by calling **CloudTable.ExecuteBatchAsync**.</span></span>

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

## <a name="get-all-of-hello-entities-in-a-partition"></a><span data-ttu-id="87873-141">Získání všech hello entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="87873-141">Get all of hello entities in a partition</span></span>
<span data-ttu-id="87873-142">tooquery tabulku pro všechny hello entit v oddílu, použití **TableQuery** objektu.</span><span class="sxs-lookup"><span data-stu-id="87873-142">tooquery a table for all of hello entities in a partition, use a **TableQuery** object.</span></span> <span data-ttu-id="87873-143">Hello následující příklad kódu určuje filtr pro entity, kde je: Váša' klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="87873-143">hello following code example specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="87873-144">Tento příklad zobrazí pole každé entity v konzole toohello výsledky dotazu hello hello.</span><span class="sxs-lookup"><span data-stu-id="87873-144">This example prints hello fields of each entity in hello query results toohello console.</span></span>

    // Construct hello query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

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

    return View();


## <a name="get-a-single-entity"></a><span data-ttu-id="87873-145">Získat jedné entity</span><span class="sxs-lookup"><span data-stu-id="87873-145">Get a single entity</span></span>
<span data-ttu-id="87873-146">Můžete napsat dotaz tooget jedné konkrétní entity.</span><span class="sxs-lookup"><span data-stu-id="87873-146">You can write a query tooget a single, specific entity.</span></span> <span data-ttu-id="87873-147">Hello následující kód používá **TableOperation** objektu toospecify zákazník s názvem 'Ben Smith'.</span><span class="sxs-lookup"><span data-stu-id="87873-147">hello following code uses a **TableOperation** object toospecify a customer named 'Ben Smith'.</span></span> <span data-ttu-id="87873-148">Tato metoda vrátí místo kolekce pouze jednu entitu a hello vrácená hodnota v **TableResult.Result** je **CustomerEntity** objektu.</span><span class="sxs-lookup"><span data-stu-id="87873-148">This method returns just one entity, rather than a collection, and hello returned value in **TableResult.Result** is a **CustomerEntity** object.</span></span> <span data-ttu-id="87873-149">Určení klíče oddílu a řádku v dotazu je hello nejrychlejší způsob, jak tooretrieve jedné entity z hello **tabulky** služby.</span><span class="sxs-lookup"><span data-stu-id="87873-149">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello **Table** service.</span></span>

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute hello retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print hello phone number of hello result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("hello phone number could not be retrieved.");

## <a name="delete-an-entity"></a><span data-ttu-id="87873-150">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="87873-150">Delete an entity</span></span>
<span data-ttu-id="87873-151">Entitu můžete odstranit po ji najít.</span><span class="sxs-lookup"><span data-stu-id="87873-151">You can delete an entity after you find it.</span></span> <span data-ttu-id="87873-152">Hello následující kód vyhledá entitu zákazníka s názvem "Ben Smith", a pokud ho najde, se odstraní.</span><span class="sxs-lookup"><span data-stu-id="87873-152">hello following code looks for a customer entity named "Ben Smith", and if it finds it, it deletes it.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="87873-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87873-153">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

