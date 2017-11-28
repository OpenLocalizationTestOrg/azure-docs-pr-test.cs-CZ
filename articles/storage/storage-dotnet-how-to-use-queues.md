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
# <a name="get-started-with-azure-queue-storage-using-net"></a><span data-ttu-id="d0b93-104">Začínáme s úložištěm Azure Queue pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="d0b93-104">Get started with Azure Queue storage using .NET</span></span>
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

## <a name="overview"></a><span data-ttu-id="d0b93-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="d0b93-105">Overview</span></span>
<span data-ttu-id="d0b93-106">Úložiště Azure Queue zajišťuje cloudový přenos zpráv mezi součástmi aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0b93-106">Azure Queue storage provides cloud messaging between application components.</span></span> <span data-ttu-id="d0b93-107">Při navrhování aplikací pro škálování ve větším měřítku jsou jednotlivé součásti aplikací často nepropojené, aby je bylo možné škálovat nezávisle.</span><span class="sxs-lookup"><span data-stu-id="d0b93-107">In designing applications for scale, application components are often decoupled, so that they can scale independently.</span></span> <span data-ttu-id="d0b93-108">Fronty úložiště poskytuje asynchronní zasílání zpráv pro komunikaci mezi součástmi aplikací, zda jsou spuštěny v hello cloudu, na ploše hello, na místním serveru nebo na mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="d0b93-108">Queue storage delivers asynchronous messaging for communication between application components, whether they are running in hello cloud, on hello desktop, on an on-premises server, or on a mobile device.</span></span> <span data-ttu-id="d0b93-109">Queue Storage také podporuje správu asynchronních úloh a pracovní postupy procesů sestavování buildů.</span><span class="sxs-lookup"><span data-stu-id="d0b93-109">Queue storage also supports managing asynchronous tasks and building process work flows.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="d0b93-110">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="d0b93-110">About this tutorial</span></span>
<span data-ttu-id="d0b93-111">Tento kurz ukazuje, jak kód toowrite .NET pro některé běžné scénáře pomocí Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="d0b93-111">This tutorial shows how toowrite .NET code for some common scenarios using Azure Queue storage.</span></span> <span data-ttu-id="d0b93-112">Jsou například zahrnuty scénáře vytváření a odstraňování front a přidávání, čtení a odstraňování front zpráv.</span><span class="sxs-lookup"><span data-stu-id="d0b93-112">Scenarios covered include creating and deleting queues and adding, reading, and deleting queue messages.</span></span>

<span data-ttu-id="d0b93-113">**Odhadovaný čas toocomplete:** 45 minut</span><span class="sxs-lookup"><span data-stu-id="d0b93-113">**Estimated time toocomplete:** 45 minutes</span></span>

<span data-ttu-id="d0b93-114">**Požadavky:**</span><span class="sxs-lookup"><span data-stu-id="d0b93-114">**Prerequisities:**</span></span>

* [<span data-ttu-id="d0b93-115">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d0b93-115">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="d0b93-116">Klientská knihovna Azure Storage pro .NET</span><span class="sxs-lookup"><span data-stu-id="d0b93-116">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="d0b93-117">Azure Configuration Manager for .NET</span><span class="sxs-lookup"><span data-stu-id="d0b93-117">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* <span data-ttu-id="d0b93-118">[Účet úložiště Azure](storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="d0b93-118">An [Azure storage account](storage-create-storage-account.md#create-a-storage-account)</span></span>

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="d0b93-119">Přidání direktiv using</span><span class="sxs-lookup"><span data-stu-id="d0b93-119">Add using directives</span></span>
<span data-ttu-id="d0b93-120">Přidejte následující hello `using` direktivy toohello začátek hello `Program.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="d0b93-120">Add hello following `using` directives toohello top of hello `Program.cs` file:</span></span>

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types
```

### <a name="parse-hello-connection-string"></a><span data-ttu-id="d0b93-121">Analyzovat hello připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="d0b93-121">Parse hello connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-queue-service-client"></a><span data-ttu-id="d0b93-122">Vytvoření klienta služby front hello</span><span class="sxs-lookup"><span data-stu-id="d0b93-122">Create hello Queue service client</span></span>
<span data-ttu-id="d0b93-123">Hello **CloudQueueClient** třída umožňuje vám fronty tooretrieve uložené v rámci Queue storage.</span><span class="sxs-lookup"><span data-stu-id="d0b93-123">hello **CloudQueueClient** class enables you tooretrieve queues stored in Queue storage.</span></span> <span data-ttu-id="d0b93-124">Tady je jedním ze způsobů toocreate hello služby klienta:</span><span class="sxs-lookup"><span data-stu-id="d0b93-124">Here's one way toocreate hello service client:</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
```
    
<span data-ttu-id="d0b93-125">Teď je připraven toowrite kód, který načítá a zapisuje data tooQueue úložiště.</span><span class="sxs-lookup"><span data-stu-id="d0b93-125">Now you are ready toowrite code that reads data from and writes data tooQueue storage.</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="d0b93-126">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="d0b93-126">Create a queue</span></span>
<span data-ttu-id="d0b93-127">Tento příklad ukazuje, jak toocreate fronty, pokud ještě neexistuje:</span><span class="sxs-lookup"><span data-stu-id="d0b93-127">This example shows how toocreate a queue if it does not already exist:</span></span>

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

## <a name="insert-a-message-into-a-queue"></a><span data-ttu-id="d0b93-128">Vložení zprávy do fronty</span><span class="sxs-lookup"><span data-stu-id="d0b93-128">Insert a message into a queue</span></span>
<span data-ttu-id="d0b93-129">tooinsert zprávu do existující fronty, vytvořte nejdříve novou **CloudQueueMessage**.</span><span class="sxs-lookup"><span data-stu-id="d0b93-129">tooinsert a message into an existing queue, first create a new **CloudQueueMessage**.</span></span> <span data-ttu-id="d0b93-130">Pak zavolejte hello **AddMessage** metoda.</span><span class="sxs-lookup"><span data-stu-id="d0b93-130">Next, call hello **AddMessage** method.</span></span> <span data-ttu-id="d0b93-131">**CloudQueueMessage** je možné vytvořit buď z řetězce (ve formátu UTF-8), nebo z **bajtového** pole.</span><span class="sxs-lookup"><span data-stu-id="d0b93-131">A **CloudQueueMessage** can be created from either a string (in UTF-8 format) or a **byte** array.</span></span> <span data-ttu-id="d0b93-132">Zde je kód, který vytvoří frontu (pokud neexistuje) a vloží uvítací zprávu "Hello, World":</span><span class="sxs-lookup"><span data-stu-id="d0b93-132">Here is code which creates a queue (if it doesn't exist) and inserts hello message 'Hello, World':</span></span>

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

## <a name="peek-at-hello-next-message"></a><span data-ttu-id="d0b93-133">Zobrazení náhledu další zprávy hello</span><span class="sxs-lookup"><span data-stu-id="d0b93-133">Peek at hello next message</span></span>
<span data-ttu-id="d0b93-134">Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello pomocí volání hello **PeekMessage** metoda.</span><span class="sxs-lookup"><span data-stu-id="d0b93-134">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **PeekMessage** method.</span></span>

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

## <a name="change-hello-contents-of-a-queued-message"></a><span data-ttu-id="d0b93-135">Změna obsahu zpráv zařazených ve frontě hello</span><span class="sxs-lookup"><span data-stu-id="d0b93-135">Change hello contents of a queued message</span></span>
<span data-ttu-id="d0b93-136">Můžete změnit obsah zprávy přímo ve frontě hello hello.</span><span class="sxs-lookup"><span data-stu-id="d0b93-136">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="d0b93-137">Pokud zpráva představuje pracovní úlohu, můžete použít tuto funkci tooupdate stav hello pracovní úlohy.</span><span class="sxs-lookup"><span data-stu-id="d0b93-137">If the message represents a work task, you could use this feature tooupdate the status of hello work task.</span></span> <span data-ttu-id="d0b93-138">Hello následující kód aktualizuje zprávy fronty hello nový obsah, a nastaví hello tooextend časový limit viditelnosti jiné 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="d0b93-138">hello following code updates hello queue message with new contents, and sets hello visibility timeout tooextend another 60 seconds.</span></span> <span data-ttu-id="d0b93-139">Uloží hello stav práce spojený s uvítací zprávu a poskytuje další minutu toocontinue pracující na uvítací zprávu klienta hello.</span><span class="sxs-lookup"><span data-stu-id="d0b93-139">This saves hello state of work associated with hello message, and gives hello client another minute toocontinue working on hello message.</span></span> <span data-ttu-id="d0b93-140">Můžete použít tento postup tootrack vícekrokového pracovní postupy pro zprávy ve frontě, bez nutnosti toostart přes od začátku hello, pokud se nezdaří krok zpracování z důvodu selhání toohardware nebo softwaru.</span><span class="sxs-lookup"><span data-stu-id="d0b93-140">You could use this technique tootrack multi-step workflows on queue messages, without having toostart over from hello beginning if a processing step fails due toohardware or software failure.</span></span> <span data-ttu-id="d0b93-141">Obvykle byste udržovali také počet opakování, a pokud hello zpráva se pokus o více než  *n*  krát, odstranili byste ji.</span><span class="sxs-lookup"><span data-stu-id="d0b93-141">Typically, you would keep a retry count as well, and if hello message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="d0b93-142">Je to ochrana proti tomu, aby zpráva při každém pokusu o zpracování nevyvolala chyby aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0b93-142">This protects against a message that triggers an application error each time it is processed.</span></span>

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

## <a name="de-queue-hello-next-message"></a><span data-ttu-id="d0b93-143">Zrušte hello další zprávu ve frontě</span><span class="sxs-lookup"><span data-stu-id="d0b93-143">De-queue hello next message</span></span>
<span data-ttu-id="d0b93-144">Váš kód vyřazuje zprávy z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="d0b93-144">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="d0b93-145">Při volání **GetMessage**, získat hello další zprávu ve frontě.</span><span class="sxs-lookup"><span data-stu-id="d0b93-145">When you call **GetMessage**, you get hello next message in a queue.</span></span> <span data-ttu-id="d0b93-146">Zpráva vrácená metodou **GetMessage** stane neviditelnou tooany další kód, který čte zprávy z této fronty.</span><span class="sxs-lookup"><span data-stu-id="d0b93-146">A message returned from **GetMessage** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="d0b93-147">Ve výchozím nastavení tato zpráva zůstává neviditelná po dobu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="d0b93-147">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="d0b93-148">toofinish odebírání uvítací zprávu z fronty hello, musíte také zavolat **DeleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="d0b93-148">toofinish removing hello message from hello queue, you must also call **DeleteMessage**.</span></span> <span data-ttu-id="d0b93-149">Tento dvoukrokový proces odebrání zprávy zaručuje, pokud se váš kód nezdaří tooprocess, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejnou zprávu a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="d0b93-149">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="d0b93-150">Váš kód zavolá metodu **DeleteMessage** hned po zpracování zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="d0b93-150">Your code calls **DeleteMessage** right after hello message has been processed.</span></span>

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

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a><span data-ttu-id="d0b93-151">Použití vzoru Async-Await s běžnými rozhraním API Queue Storage</span><span class="sxs-lookup"><span data-stu-id="d0b93-151">Use Async-Await pattern with common Queue storage APIs</span></span>
<span data-ttu-id="d0b93-152">Tento příklad ukazuje, jak vzor toouse hello Async-Await s běžnými rozhraním API Queue storage.</span><span class="sxs-lookup"><span data-stu-id="d0b93-152">This example shows how toouse hello Async-Await pattern with common Queue storage APIs.</span></span> <span data-ttu-id="d0b93-153">Ukázka Hello volá hello asynchronní verzi každé z hello zadané metody, jak hello *asynchronní* příponu každé metody.</span><span class="sxs-lookup"><span data-stu-id="d0b93-153">hello sample calls hello asynchronous version of each of hello given methods, as indicated by hello *Async* suffix of each method.</span></span> <span data-ttu-id="d0b93-154">Při použití asynchronní metody se používá, hello async-await vzor pozastaví místní spuštění, dokud se nedokončí volání hello.</span><span class="sxs-lookup"><span data-stu-id="d0b93-154">When an async method is used, hello async-await pattern suspends local execution until hello call completes.</span></span> <span data-ttu-id="d0b93-155">Toto chování umožňuje hello aktuální vlákno toodo další činnosti, což pomáhá vyhnout se kritickým bodům výkonu a zlepšuje celkovou rychlost reakce aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="d0b93-155">This behavior allows hello current thread toodo other work, which helps avoid performance bottlenecks and improves hello overall responsiveness of your application.</span></span> <span data-ttu-id="d0b93-156">Další podrobnosti o použití hello vzoru Async-Await v rozhraní .NET najdete v části [Async a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span><span class="sxs-lookup"><span data-stu-id="d0b93-156">For more details on using hello Async-Await pattern in .NET see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

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
    
## <a name="leverage-additional-options-for-de-queuing-messages"></a><span data-ttu-id="d0b93-157">Využívání dalších možností pro vyřazování zpráv z fronty</span><span class="sxs-lookup"><span data-stu-id="d0b93-157">Leverage additional options for de-queuing messages</span></span>
<span data-ttu-id="d0b93-158">Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="d0b93-158">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="d0b93-159">Nejprve můžete načíst dávku zpráv (až too32).</span><span class="sxs-lookup"><span data-stu-id="d0b93-159">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="d0b93-160">Druhý můžete nastavit časový limit neviditelnosti delší nebo kratší, povolení váš kód více nebo méně času toofully zpracování jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="d0b93-160">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="d0b93-161">Hello následující příklad kódu používá **GetMessages** metoda tooget 20 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="d0b93-161">hello following code example uses the **GetMessages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="d0b93-162">Následně se každá zpráva zpracuje pomocí smyčky **foreach**.</span><span class="sxs-lookup"><span data-stu-id="d0b93-162">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="d0b93-163">Nastaví taky hello neviditelnosti časový limit toofive minut pro každou zprávu.</span><span class="sxs-lookup"><span data-stu-id="d0b93-163">It also sets hello invisibility timeout toofive minutes for each message.</span></span> <span data-ttu-id="d0b93-164">Všimněte si, že hello 5 minut spustí pro všechny zprávy s hello stejný čas, takže po 5 minut od volání hello příliš předané**GetMessages**, všechny zprávy, které nebyly odstraněny, opět viditelné.</span><span class="sxs-lookup"><span data-stu-id="d0b93-164">Note that hello 5 minutes starts for all messages at hello same time, so after 5 minutes have passed since hello call too**GetMessages**, any messages which have not been deleted will become visible again.</span></span>

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

## <a name="get-hello-queue-length"></a><span data-ttu-id="d0b93-165">Získání délky fronty hello</span><span class="sxs-lookup"><span data-stu-id="d0b93-165">Get hello queue length</span></span>
<span data-ttu-id="d0b93-166">Můžete získat odhad hello počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="d0b93-166">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="d0b93-167">**FetchAttributes** metoda požádá hello službu front o načtení atributů fronty hello, včetně počtu zpráv hello.</span><span class="sxs-lookup"><span data-stu-id="d0b93-167">The **FetchAttributes** method asks hello Queue service to retrieve hello queue attributes, including hello message count.</span></span> <span data-ttu-id="d0b93-168">Hello **ApproximateMessageCount** vlastnost vrací hello poslední hodnotu načtenou **FetchAttributes** metoda bez volání služby front hello.</span><span class="sxs-lookup"><span data-stu-id="d0b93-168">hello **ApproximateMessageCount** property returns hello last value retrieved by the **FetchAttributes** method, without calling hello Queue service.</span></span>

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

## <a name="delete-a-queue"></a><span data-ttu-id="d0b93-169">Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="d0b93-169">Delete a queue</span></span>
<span data-ttu-id="d0b93-170">toodelete frontu a všechny zprávy hello obsažené v něm, volejte **odstranit** metoda na objekt fronty hello.</span><span class="sxs-lookup"><span data-stu-id="d0b93-170">toodelete a queue and all hello messages contained in it, call the **Delete** method on hello queue object.</span></span>

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
    

## <a name="next-steps"></a><span data-ttu-id="d0b93-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0b93-171">Next steps</span></span>
<span data-ttu-id="d0b93-172">Teď, když jste se naučili základy hello fronty úložiště, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště.</span><span class="sxs-lookup"><span data-stu-id="d0b93-172">Now that you've learned hello basics of Queue storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="d0b93-173">Zobrazení hello fronty referenční dokumentaci ke službě kompletní informace o dostupných rozhraních API:</span><span class="sxs-lookup"><span data-stu-id="d0b93-173">View hello Queue service reference documentation for complete details about available APIs:</span></span>
  * [<span data-ttu-id="d0b93-174">Klientská knihovna pro úložiště pro .NET – referenční informace</span><span class="sxs-lookup"><span data-stu-id="d0b93-174">Storage Client Library for .NET reference</span></span>](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
  * [<span data-ttu-id="d0b93-175">REST API – referenční informace</span><span class="sxs-lookup"><span data-stu-id="d0b93-175">REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="d0b93-176">Zjistěte, jak kód hello toosimplify napíšete toowork s Azure Storage pomocí hello [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="d0b93-176">Learn how toosimplify hello code you write toowork with Azure Storage by using hello [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md).</span></span>
* <span data-ttu-id="d0b93-177">Zobrazte další funkce příručky toolearn o dalších možnostech pro ukládání dat v Azure.</span><span class="sxs-lookup"><span data-stu-id="d0b93-177">View more feature guides toolearn about additional options for storing data in Azure.</span></span>
  * <span data-ttu-id="d0b93-178">[Začínáme s Azure Table storage pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md) toostore strukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="d0b93-178">[Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md) toostore structured data.</span></span>
  * <span data-ttu-id="d0b93-179">[Začínáme s Azure Blob storage pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md) toostore Nestrukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="d0b93-179">[Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) toostore unstructured data.</span></span>
  * <span data-ttu-id="d0b93-180">[Připojit tooSQL databáze pomocí rozhraní .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) toostore relační data.</span><span class="sxs-lookup"><span data-stu-id="d0b93-180">[Connect tooSQL Database by using .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) toostore relational data.</span></span>

[Download and install hello Azure SDK for .NET]: /develop/net/
[.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
[Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
[Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
[Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
