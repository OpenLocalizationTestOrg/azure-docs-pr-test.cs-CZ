---
title: "aaaGet začít s Azure Queue storage pomocí rozhraní .NET | Microsoft Docs"
description: "Fronty Azure Queue poskytují spolehlivý asynchronní přenos zpráv mezi součástmi aplikace. Cloud zasílání zpráv umožňuje vaší aplikace součásti tooscale nezávisle."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: c0f82537-a613-4f01-b2ed-fc82e5eea2a7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/27/2017
ms.author: robinsh
ms.openlocfilehash: 36bbb40189a301cddbc2ded92d0623fa5e093eb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-queue-storage-using-net"></a>Začínáme s úložištěm Azure Queue pomocí rozhraní .NET
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

## <a name="overview"></a>Přehled
Úložiště Azure Queue zajišťuje cloudový přenos zpráv mezi součástmi aplikace. Při navrhování aplikací pro škálování ve větším měřítku jsou jednotlivé součásti aplikací často nepropojené, aby je bylo možné škálovat nezávisle. Fronty úložiště poskytuje asynchronní zasílání zpráv pro komunikaci mezi součástmi aplikací, zda jsou spuštěny v hello cloudu, na ploše hello, na místním serveru nebo na mobilním zařízení. Queue Storage také podporuje správu asynchronních úloh a pracovní postupy procesů sestavování buildů.

### <a name="about-this-tutorial"></a>O tomto kurzu
Tento kurz ukazuje, jak kód toowrite .NET pro některé běžné scénáře pomocí Azure Queue storage. Jsou například zahrnuty scénáře vytváření a odstraňování front a přidávání, čtení a odstraňování front zpráv.

**Odhadovaný čas toocomplete:** 45 minut

**Požadavky:**

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Klientská knihovna Azure Storage pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Azure Configuration Manager for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [Účet úložiště Azure](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a>Přidání direktiv using
Přidejte následující hello `using` direktivy toohello začátek hello `Program.cs` souboru:

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types
```

### <a name="parse-hello-connection-string"></a>Analyzovat hello připojovací řetězec
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-queue-service-client"></a>Vytvoření klienta služby front hello
Hello **CloudQueueClient** třída umožňuje vám fronty tooretrieve uložené v rámci Queue storage. Tady je jedním ze způsobů toocreate hello služby klienta:

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
```
    
Teď je připraven toowrite kód, který načítá a zapisuje data tooQueue úložiště.

## <a name="create-a-queue"></a>Vytvoření fronty
Tento příklad ukazuje, jak toocreate fronty, pokud ještě neexistuje:

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa container.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create hello queue if it doesn't already exist
queue.CreateIfNotExists();
```

## <a name="insert-a-message-into-a-queue"></a>Vložení zprávy do fronty
tooinsert zprávu do existující fronty, vytvořte nejdříve novou **CloudQueueMessage**. Pak zavolejte hello **AddMessage** metoda. **CloudQueueMessage** je možné vytvořit buď z řetězce (ve formátu UTF-8), nebo z **bajtového** pole. Zde je kód, který vytvoří frontu (pokud neexistuje) a vloží uvítací zprávu "Hello, World":

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create hello queue if it doesn't already exist.
queue.CreateIfNotExists();

// Create a message and add it toohello queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
queue.AddMessage(message);
```

## <a name="peek-at-hello-next-message"></a>Zobrazení náhledu další zprávy hello
Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello pomocí volání hello **PeekMessage** metoda.

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Peek at hello next message
CloudQueueMessage peekedMessage = queue.PeekMessage();

// Display message.
Console.WriteLine(peekedMessage.AsString);
```

## <a name="change-hello-contents-of-a-queued-message"></a>Změna obsahu zpráv zařazených ve frontě hello
Můžete změnit obsah zprávy přímo ve frontě hello hello. Pokud zpráva představuje pracovní úlohu, můžete použít tuto funkci tooupdate stav hello pracovní úlohy. Hello následující kód aktualizuje zprávy fronty hello nový obsah, a nastaví hello tooextend časový limit viditelnosti jiné 60 sekund. Uloží hello stav práce spojený s uvítací zprávu a poskytuje další minutu toocontinue pracující na uvítací zprávu klienta hello. Můžete použít tento postup tootrack vícekrokového pracovní postupy pro zprávy ve frontě, bez nutnosti toostart přes od začátku hello, pokud se nezdaří krok zpracování z důvodu selhání toohardware nebo softwaru. Obvykle byste udržovali také počet opakování, a pokud hello zpráva se pokus o více než  *n*  krát, odstranili byste ji. Je to ochrana proti tomu, aby zpráva při každém pokusu o zpracování nevyvolala chyby aplikace.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get hello message from hello queue and update hello message contents.
CloudQueueMessage message = queue.GetMessage();
message.SetMessageContent("Updated contents.");
queue.UpdateMessage(message,
    TimeSpan.FromSeconds(60.0),  // Make it invisible for another 60 seconds.
    MessageUpdateFields.Content | MessageUpdateFields.Visibility);
```

## <a name="de-queue-hello-next-message"></a>Zrušte hello další zprávu ve frontě
Váš kód vyřazuje zprávy z fronty ve dvou krocích. Při volání **GetMessage**, získat hello další zprávu ve frontě. Zpráva vrácená metodou **GetMessage** stane neviditelnou tooany další kód, který čte zprávy z této fronty. Ve výchozím nastavení tato zpráva zůstává neviditelná po dobu 30 sekund. toofinish odebírání uvítací zprávu z fronty hello, musíte také zavolat **DeleteMessage**. Tento dvoukrokový proces odebrání zprávy zaručuje, pokud se váš kód nezdaří tooprocess, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejnou zprávu a zkuste to znovu. Váš kód zavolá metodu **DeleteMessage** hned po zpracování zprávy hello.

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get hello next message
CloudQueueMessage retrievedMessage = queue.GetMessage();

//Process hello message in less than 30 seconds, and then delete hello message
queue.DeleteMessage(retrievedMessage);
```

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Použití vzoru Async-Await s běžnými rozhraním API Queue Storage
Tento příklad ukazuje, jak vzor toouse hello Async-Await s běžnými rozhraním API Queue storage. Ukázka Hello volá hello asynchronní verzi každé z hello zadané metody, jak hello *asynchronní* příponu každé metody. Při použití asynchronní metody se používá, hello async-await vzor pozastaví místní spuštění, dokud se nedokončí volání hello. Toto chování umožňuje hello aktuální vlákno toodo další činnosti, což pomáhá vyhnout se kritickým bodům výkonu a zlepšuje celkovou rychlost reakce aplikace hello. Další podrobnosti o použití hello vzoru Async-Await v rozhraní .NET najdete v části [Async a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

```csharp
// Create hello queue if it doesn't already exist
if(await queue.CreateIfNotExistsAsync())
{
    Console.WriteLine("Queue '{0}' Created", queue.Name);
}
else
{
    Console.WriteLine("Queue '{0}' Exists", queue.Name);
}

// Create a message tooput in hello queue
CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

// Async enqueue hello message
await queue.AddMessageAsync(cloudQueueMessage);
Console.WriteLine("Message added");

// Async dequeue hello message
CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

// Async delete hello message
await queue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Deleted message");
```
    
## <a name="leverage-additional-options-for-de-queuing-messages"></a>Využívání dalších možností pro vyřazování zpráv z fronty
Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.
Nejprve můžete načíst dávku zpráv (až too32). Druhý můžete nastavit časový limit neviditelnosti delší nebo kratší, povolení váš kód více nebo méně času toofully zpracování jednotlivých zpráv. Hello následující příklad kódu používá **GetMessages** metoda tooget 20 zpráv v jednom volání. Následně se každá zpráva zpracuje pomocí smyčky **foreach**. Nastaví taky hello neviditelnosti časový limit toofive minut pro každou zprávu. Všimněte si, že hello 5 minut spustí pro všechny zprávy s hello stejný čas, takže po 5 minut od volání hello příliš předané**GetMessages**, všechny zprávy, které nebyly odstraněny, opět viditelné.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
{
    // Process all messages in less than 5 minutes, deleting each message after processing.
    queue.DeleteMessage(message);
}
```

## <a name="get-hello-queue-length"></a>Získání délky fronty hello
Můžete získat odhad hello počet zpráv ve frontě. **FetchAttributes** metoda požádá hello službu front o načtení atributů fronty hello, včetně počtu zpráv hello. Hello **ApproximateMessageCount** vlastnost vrací hello poslední hodnotu načtenou **FetchAttributes** metoda bez volání služby front hello.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Fetch hello queue attributes.
queue.FetchAttributes();

// Retrieve hello cached approximate message count.
int? cachedMessageCount = queue.ApproximateMessageCount;

// Display number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="delete-a-queue"></a>Odstranění fronty
toodelete frontu a všechny zprávy hello obsažené v něm, volejte **odstranit** metoda na objekt fronty hello.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Delete hello queue.
queue.Delete();
```
    

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello fronty úložiště, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště.

* Zobrazení hello fronty referenční dokumentaci ke službě kompletní informace o dostupných rozhraních API:
  * [Klientská knihovna pro úložiště pro .NET – referenční informace](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
  * [REST API – referenční informace](http://msdn.microsoft.com/library/azure/dd179355)
* Zjistěte, jak kód hello toosimplify napíšete toowork s Azure Storage pomocí hello [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md).
* Zobrazte další funkce příručky toolearn o dalších možnostech pro ukládání dat v Azure.
  * [Začínáme s Azure Table storage pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md) toostore strukturovaná data.
  * [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md) toostore Nestrukturovaná data.
  * [Připojit tooSQL databáze pomocí rozhraní .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) toostore relační data.

[Download and install hello Azure SDK for .NET]: /develop/net/
[.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
[Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
[Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
[Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
