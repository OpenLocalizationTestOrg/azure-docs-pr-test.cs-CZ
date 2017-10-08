---
title: "aaaHow tooget začít s úložiště tabulek a Visual Studio připojených služeb (ASP.NET Core) | Microsoft Docs"
description: "Jak tooget pracovat s Azure Table storage v projektu ASP.NET Core v sadě Visual Studio po připojení tooa účet úložiště pomocí sady Visual Studio připojené služby"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: c3c451d1-71ff-4222-a348-c41c98a02b85
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: e3eb3f3e65456108dd3cde7e3e470f98ba456e35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-started-with-azure-table-storage-and-visual-studio-connected-services"></a>Jak tooget pracovat s Azure Table storage a Visual Studio připojených služeb
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Přehled
Tento článek popisuje, jak získat spuštění pomocí Azure Table storage v sadě Visual Studio po vytvoření a odkazuje pomocí účtu úložiště Azure v projektu ASP.NET Core hello Visual Studio **přidat připojení služby** dialogové okno.

Hello služba úložiště Azure Table umožňuje toostore velkých objemů strukturovaná data. Služba Hello je úložiště dat typu NoSQL, která přijímá ověřených volání z vnitřní a vnější hello cloudu Azure. Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat.

Hello **přidat připojení služby** operaci nainstaluje hello odpovídající NuGet balíčky tooaccess úložiště Azure ve vašem projektu a přidá hello připojovací řetězec pro hello úložiště účet tooyour projektu konfigurační soubory.

Další obecné informace o používání Azure Table storage najdete v tématu [Začínáme s Azure Table storage pomocí rozhraní .NET](../storage/storage-dotnet-how-to-use-tables.md).

tooget spustit, musíte nejprve toocreate tabulku ve vašem účtu úložiště. Ukážeme vám jak toocreate Azure tabulky v kódu. Také ukážeme, jak tooperform základní tabulky a entity operace, jako je přidání, úprava, čtení a čtení tabulky entity. Hello ukázky jsou napsané v jazyce C\# kód a použít hello Klientská knihovna pro úložiště Azure pro .NET.

**Poznámka:** -některé hello rozhraní API, která provádět volání out tooAzure úložiště v ASP.NET Core jsou asynchronní. V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace. Následující kód Hello předpokládá, že asynchronní programování metody jsou používány.

## <a name="access-tables-in-code"></a>Přístup k tabulky v kódu
tooaccess tabulky v projektů ASP.NET Core, musíte tooinclude hello následující položky tooany C# zdrojové soubory, které přístupu k Azure table storage.

1. Deklarace oborů názvů hello hello horní části souboru hello C# zahrnout tyto **pomocí** příkazy.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Následující kód tooget hello použití hello připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure hello.
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
   
    **Poznámka:** -používat všechny hello výše kódu před hello kódu v hello následující ukázky.
3. Získání **CloudTableClient** objektu tooreference hello tabulka objektů ve vašem účtu úložiště.  
   
        // Create hello table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. Získání **CloudTable** odkazovat na objekt tooreference určité tabulky a entity.
   
        // Get a reference tooa table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Vytvoří tabulku v kódu
toocreate hello tabulky Azure, stačí přidat volání příliš**CreateIfNotExistsAsync()**.

    // Create hello CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-tooa-table"></a>Přidat tooa tabulka entity
tooadd tooa tabulka entity vytvořit třídu, která definuje hello vlastnosti vaší entity. Hello následující kód definuje třídu entity názvem **CustomerEntity** že hello používá jméno zákazníka jako klíč řádku hello a příjmení jako klíč oddílu hello.

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

Operace s tabulkou zahrnující entity se provádějí pomocí hello **CloudTable** objektu jste vytvořili dříve v "Přístup tabulky v kódu". Hello **TableOperation** objekt představuje toobe hello operaci provést. Následující příklad ukazuje kód jak Hello toocreate **CloudTable** objektu a **CustomerEntity** objektu. operace hello tooprepare, **TableOperation** se vytvoří entitu zákazníka hello tooinsert do tabulky hello. Nakonec hello operace provede voláním CloudTable.ExecuteAsync.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create hello TableOperation that inserts hello customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute hello insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Vložení dávky entit
Do tabulky v rámci jedné zápisu operace můžete vložit více entit. Hello následující příklad kódu vytvoří dva objekty entity ("Jeff Smith" a "Ben Smith"), se přidají tooa **TableBatchOperation** objekt, který používá hello **vložit** metoda a poté spustí operaci hello podle volání metody CloudTable.ExecuteBatchAsync.

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

## <a name="get-all-of-hello-entities-in-a-partition"></a>Získání všech hello entit v oddílu
tooquery tabulku pro všechny hello entit v oddílu, použití **TableQuery** objektu. Hello následující příklad kódu určuje filtr pro entity, kde je: Váša' klíč oddílu hello. Tento příklad zobrazí pole každé entity v konzole toohello výsledky dotazu hello hello.

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

## <a name="get-a-single-entity"></a>Získat jedné entity
Můžete napsat dotaz tooget jedné konkrétní entity. Hello následující kód používá **TableOperation** objektu toospecify zákazník s názvem 'Ben Smith'. Tato metoda vrátí místo kolekce pouze jednu entitu a hello vrácená hodnota v **TableResult.Result** je **CustomerEntity** objektu. Určení klíče oddílu a řádku v dotazu je hello nejrychlejší způsob, jak tooretrieve jedné entity z hello **tabulky** služby.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute hello retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print hello phone number of hello result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("hello phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Odstranění entity
Entitu můžete odstranit po ji najít. Hello následující kód vyhledá entitu zákazníka s názvem "Ben Smith" a pokud ho najde, se odstraní.

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

## <a name="next-steps"></a>Další kroky
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

