---
title: "aaaGet začít s Azure Table storage pomocí rozhraní .NET | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu hello pomocí Azure Table storage, úložiště dat typu NoSQL."
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
ms.openlocfilehash: 9635079d61d874ff7f4bc9e7d610e0ad54b4fd6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-table-storage-using-net"></a>Začínáme s úložištěm Azure Table pomocí rozhraní .NET
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Azure Table storage je služba, která ukládá strukturované NoSQL úložiště dat v cloudu hello poskytnutí klíčů/atributů s návrhem. Úložiště Table nemá schéma, proto je snadno tooadapt data podle hello potřebují měnícím vaší aplikace. Přístup k datům tooTable úložiště je rychlý a nákladově efektivní pro mnoho typů aplikací je obvykle nižší náklady než tradiční SQL pro podobné objemy dat.

Flexibilních datových sad aplikace tabulky úložiště toostore jako uživatelská data můžete použít pro webové aplikace, adresářů, informací o zařízení nebo jiných typů metadat, které vaše služba vyžaduje. V tabulce můžete uložit libovolný počet entit a účet úložiště může obsahovat libovolný počet tabulek, až toohello limitu kapacity účtu úložiště hello.

### <a name="about-this-tutorial"></a>O tomto kurzu
Tento kurz ukazuje, jak toouse hello [Klientská knihovna pro úložiště Azure pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) v některé běžné scénáře Azure Table storage. Tyto scénáře jsou ilustrovány pomocí příkladu v jazyce C#, ve kterých se vytvoří a odstraní tabulka a také vkládají, aktualizují a odstraňují data v tabulce nebo se na ně zadávají dotazy.

## <a name="prerequisites"></a>Požadavky

Budete potřebovat hello následující toocomplete v tomto kurzu úspěšně:

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Klientská knihovna Azure Storage pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Azure Configuration Manager for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [Účet služby Azure Storage](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Další ukázky
Další příklady použití Table Storage najdete v článku [Začínáme s Azure Table Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/). Stáhněte hello ukázkovou aplikaci a potom ho spusťte nebo procházet kód hello na Githubu.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a>Přidání direktiv using
Přidejte následující hello **pomocí** direktivy toohello začátek hello `Program.cs` souboru:

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types
```

### <a name="parse-hello-connection-string"></a>Analyzovat hello připojovací řetězec
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-table-service-client"></a>Vytvoření klienta služby Table hello
Hello [CloudTableClient] [ dotnet_CloudTableClient] třída umožňuje tooretrieve tabulky a entity, které jsou uložené ve službě Table storage. Tady je klienta služby Table hello toocreate jedním ze způsobů:

```csharp
// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```

Teď je připraven toowrite kód, který načítá a zapisuje data tooTable úložiště.

## <a name="create-a-table"></a>Vytvoření tabulky
Tento příklad ukazuje, jak toocreate tabulky, pokud ještě neexistuje:

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

## <a name="add-an-entity-tooa-table"></a>Přidat tooa tabulka entity
Entity se mapují tooC # objekty pomocí vlastní třídy odvozené od [TableEntity][dotnet_TableEntity]. tooadd tooa tabulka entity, vytvořte třídu, která definuje hello vlastnosti vaší entity. Hello následující kód definuje třídu entity, která používá jméno hello zákazníka jako klíč řádku hello a příjmení jako klíč oddílu hello. Společně oddílu a klíč řádku jeho jednoznačné identifikaci v tabulce hello. Entity se stejným klíčem oddílu můžete zadat dotaz rychleji než entity s různými hello oddílu klíče, ale používání různých klíčů oddílů umožňuje větší škálovatelnost paralelních operací. Ukládat do tabulek toobe entit musí být podporované typu, například odvozeného z hello [TableEntity] [ dotnet_TableEntity] třídy. Vlastnosti entity, které byste chtěli toostore v tabulce musí být veřejné vlastnosti typu hello a podporují získání a nastavení hodnoty. Typ entity navíc *musí* zveřejňovat konstruktor bez parametrů.

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

Operace s tabulkou zahrnující entity se provádí prostřednictvím hello [CloudTable] [ dotnet_CloudTable] objekt, který jste předtím vytvořili v části "Vytvořit tabulku" hello. Hello toobe operaci provést, je reprezentována [TableOperation] [ dotnet_TableOperation] objektu. Hello následující příklad kódu ukazuje vytvoření hello hello [CloudTable] [ dotnet_CloudTable] objekt a potom **CustomerEntity** objektu. operace hello tooprepare, [TableOperation] [ dotnet_TableOperation] do tabulky hello je entita zákazník hello tooinsert vytvořen objekt. Nakonec hello operace provede voláním [CloudTable][dotnet_CloudTable].[ Spuštění][dotnet_CloudTable_Execute].

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

## <a name="insert-a-batch-of-entities"></a>Vložení dávky entit
V rámci jedné operace zápisu můžete do tabulky vložit dávku entit. Několik dalších poznámek ohledně dávkových operací:

* Můžete provádět aktualizace, odstranění a vložení v hello stejné jedna dávková operace.
* Jedna dávková operace může obsahovat až too100 entity.
* Všechny entity v jedné dávkové operaci musí mít hello stejným klíčem oddílu.
* I když je možné tooperform dotazu jako dávkovou operaci, musí být hello jediná operace v dávce hello.

Hello následující příklad kódu vytvoří dva objekty entity a přidá všechny příliš[TableBatchOperation] [ dotnet_TableBatchOperation] pomocí hello [vložit] [ dotnet_TableBatchOperation_Insert] Metoda. Potom [CloudTable][dotnet_CloudTable].[ ExecuteBatch] [ dotnet_CloudTable_ExecuteBatch] nazývá tooexecute hello operaci.

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

## <a name="retrieve-all-entities-in-a-partition"></a>Načtení všech entit v oddílu
tooquery tabulku pro všechny entity v oddílu, použití [TableQuery] [ dotnet_TableQuery] objektu. Hello následující příklad kódu určuje filtr pro entity, kde je: Váša' klíč oddílu hello. Tento příklad zobrazí pole každé entity v konzole toohello výsledky dotazu hello hello.

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

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Načtení rozsahu entit v oddílu
Pokud nechcete, aby tooquery všech entit v oddílu, můžete zadat rozsah kombinací hello filtru klíče oddílu s filtrem klíče řádku. Hello následující příklad kódu používá dva filtry tooget všech entit v oddílu "Smith, kde klíč řádku hello (jméno) začíná písmenem v hello abecedy před"E"a potom zobrazí výsledky dotazu hello.

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

## <a name="retrieve-a-single-entity"></a>Načtení jedné entity
Můžete napsat dotaz tooretrieve jedné konkrétní entity. Hello následující kód používá [TableOperation] [ dotnet_TableOperation] toospecify hello zákazníka, Ben Smith'. Tato metoda vrátí pouze jednu entitu místo kolekce a hello vrácená hodnota v [TableResult][dotnet_TableResult].[ Výsledek] [ dotnet_TableResult_Result] je **CustomerEntity** objektu. Určení klíče oddílu a řádku v dotazu je hello nejrychlejší způsob, jak tooretrieve jednu entitu ze služby Table hello.

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

## <a name="replace-an-entity"></a>Nahrazení entity
tooupdate entity, načtěte ji ze služby Table hello, upravte objekt entity hello a potom uložte změny hello zpět toohello služby Table. Hello následující kód změní telefonní číslo stávajícího zákazníka. Namísto volání metody [Insert][dotnet_TableOperation_Insert] tento kód používá metodu [Replace][dotnet_TableOperation_Replace]. [Nahraďte] [ dotnet_TableOperation_Replace] příčiny hello toobe entity na hello serveru plně nahradí, pokud došlo ke změně hello entit na serveru hello vzhledem k tomu, že byla načtena, v takovém případě hello se nezdaří. Tato chyba je tooprevent aplikace nechtěném přepsání změny provedené mezi hello načtení a aktualizací provedenou jinou součástí vaší aplikace. Dobrý den správné zpracování této chyby je tooretrieve hello entita znovu, provedené změny (Pokud je stále platný) a pak provedete další [nahradit] [ dotnet_TableOperation_Replace] operaci. Hello další části se dozvíte, jak toooverride toto chování.

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

## <a name="insert-or-replace-an-entity"></a>Vložení nebo nahrazení entity
[Nahraďte] [ dotnet_TableOperation_Replace] operace se nezdaří, pokud hello entity se změnil od načtení ze serveru hello. Kromě toho musí načíst hello entity ze serveru hello první v pořadí pro hello [nahradit] [ dotnet_TableOperation_Replace] toobe operace úspěšná. V některých případech ale nevíte Pokud hello entita existuje na serveru hello a hello aktuální hodnoty v ní uloženy jsou důležité. Vaše aktualizace by je měla všechny přepsat. tooaccomplish, byste použili [InsertOrReplace] [ dotnet_TableOperation_InsertOrReplace] operaci. Tato operace vloží entitu hello, pokud neexistuje, nebo ji nahradí, pokud ano, bez ohledu na to, kdy byla provedena poslední aktualizace hello.

Entitu zákazníka pro 'Petr František' v hello následující ukázka kódu, je vytvořen a vloženy do tabulky "osoby" hello. V dalším kroku používáme hello [InsertOrReplace] [ dotnet_TableOperation_InsertOrReplace] operace toosave entity s hello stejný klíč oddílu (Petr) a řádek klíče serveru toohello (František), tentokrát s jinou hodnotou pro hello telefonní číslo Vlastnost. Vzhledem k tomu, že používáme operaci [InsertOrReplace][dotnet_TableOperation_InsertOrReplace], budou nahrazeny všechny příslušné hodnoty vlastnosti. Ale pokud entity 'Petr František, kdyby již existuje v tabulce hello, ho by byly vloženy.

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

## <a name="query-a-subset-of-entity-properties"></a>Dotaz na podmnožinu vlastností entity
Dotaz na tabulku můžete načíst jenom několik z entity, místo všech vlastností entity hello. Tato technika, které se říká projekce, snižuje šířku pásma a může zlepšit výkon dotazů, zejména u velkých entit. dotaz Hello v hello následující kód vrátí pouze hello e-mailové adresy entit v tabulce hello. To se provádí pomocí dotazu [DynamicTableEntity][dotnet_DynamicTableEntity] a také [EntityResolver][dotnet_EntityResolver]. Další informace o projekce v hello [blogový příspěvek představení funkcí Upsert a projekce dotazu][blog_post_upsert]. Projekce nepodporuje emulátor úložiště hello, takže tento kód spustí pouze v případě, že používáte účet v služby Table hello.

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

## <a name="delete-an-entity"></a>Odstranění entity
Po jejím načtení pomocí hello můžete snadno odstranit entity stejného vzoru zobrazovaného pro aktualizaci entity. Hello následující kód načte a odstraní entitu zákazníka.

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

## <a name="delete-a-table"></a>Odstranění tabulky
Hello následující příklad kódu nakonec odstraní tabulku z účtu úložiště. Tabulka, která byla odstraněna budou k dispozici toobe znovu vytvořit pro následující hello odstranění časovém intervalu.

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

## <a name="retrieve-entities-in-pages-asynchronously"></a>Asynchronní načítání entit na stránkách
Pokud načítáte velký počet entit a chcete entity tooprocess nebo zobrazení, jako jsou načítány, místo, až se všechny tooreturn, můžete entity načíst pomocí segmentovaného dotazu. Tento příklad ukazuje, jak se pro velké sady výsledků tooreturn čekáte tooreturn výsledky na stránkách pomocí vzoru Async-Await hello tak, aby vám neblokovalo provádění. Další podrobnosti o použití hello vzoru Async-Await v rozhraní .NET, najdete v části [asynchronní programování s Async a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

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

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello služby Table storage, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště:

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.
* Další příklady Table Storage najdete v článku [Začínáme s Azure Table Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/).
* Zobrazení hello tabulky referenční dokumentaci ke službě kompletní informace o dostupných rozhraních API:
* [Klientská knihovna pro úložiště pro .NET – referenční informace](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
* [REST API – referenční informace](http://msdn.microsoft.com/library/azure/dd179355)
* Zjistěte, jak kód hello toosimplify napíšete toowork s Azure Storage pomocí hello [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
* Zobrazte další funkce příručky toolearn o dalších možnostech pro ukládání dat v Azure.
* [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md) toostore Nestrukturovaná data.
* [Připojit tooSQL databáze pomocí rozhraní .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) toostore relační data.

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
