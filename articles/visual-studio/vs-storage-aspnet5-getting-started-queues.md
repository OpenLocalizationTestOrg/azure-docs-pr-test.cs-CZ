---
title: "aaaGet začít s fronty úložiště a Visual Studio připojených služeb (ASP.NET Core) | Microsoft Docs"
description: "Způsob tooget spuštění pomocí fronty Azure storage v projektu ASP.NET Core v sadě Visual Studio"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 90c1ebcb6a2eac6bc4f6b69133bdf3135d359a72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a><span data-ttu-id="64813-103">Začínáme s fronty úložiště a Visual Studio připojené služby (ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="64813-103">Get started with queue storage and Visual Studio connected services (ASP.NET Core)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="64813-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="64813-104">Overview</span></span>
<span data-ttu-id="64813-105">Tento článek popisuje, jak tooget spuštění pomocí Azure Queue storage v sadě Visual Studio, po vytvoření a odkazuje pomocí sady Visual Studio hello účet úložiště Azure v projektu ASP.NET Core **přidat připojení služby** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="64813-105">This article describes how tooget started using Azure Queue storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using hello Visual Studio **Add Connected Services** dialog.</span></span> <span data-ttu-id="64813-106">Hello **přidat připojení služby** operaci nainstaluje hello odpovídající NuGet balíčky tooaccess úložiště Azure ve vašem projektu a přidá hello připojovací řetězec pro hello úložiště účet tooyour projektu konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="64813-106">hello **Add Connected Services** operation installs hello appropriate NuGet packages tooaccess Azure storage in your project and adds hello connection string for hello storage account tooyour project configuration files.</span></span>

<span data-ttu-id="64813-107">Azure queue storage je služba pro ukládání velkého počtu zpráv, které lze přistupovat z libovolné místo v hello, world prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="64813-107">Azure queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="64813-108">Zpráva s jednou frontou může být až too64 kilobajtů (KB) velikost a jedna fronta můžete obsahovat miliony zpráv, až toohello limit celkové kapacity účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="64813-108">A single queue message can be up too64 kilobytes (KB) in size, and a queue can contain millions of messages, up toohello total capacity limit of a storage account.</span></span>

<span data-ttu-id="64813-109">tooget spustit, musíte nejprve toocreate fronty Azure ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="64813-109">tooget started, you first need toocreate an Azure queue in your storage account.</span></span> <span data-ttu-id="64813-110">Ukážeme vám jak toocreate fronty v kódu.</span><span class="sxs-lookup"><span data-stu-id="64813-110">We'll show you how toocreate a queue in code.</span></span> <span data-ttu-id="64813-111">Také ukážeme jak tooperform basic fronty operací, jako je přidání, úprava, čtení a odebrání zprávy do fronty.</span><span class="sxs-lookup"><span data-stu-id="64813-111">We'll also show you how tooperform basic queue operations, such as adding, modifying, reading, and removing queue messages.</span></span> <span data-ttu-id="64813-112">Hello ukázky jsou napsané v jazyce C\# kód a použít hello Klientská knihovna pro úložiště Azure pro .NET.</span><span class="sxs-lookup"><span data-stu-id="64813-112">hello samples are written in C\# code and use hello Azure Storage Client Library for .NET.</span></span> <span data-ttu-id="64813-113">Další informace o technologii ASP.NET najdete v tématu [ASP.NET](http://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="64813-113">For more information about ASP.NET, see [ASP.NET](http://www.asp.net).</span></span>

<span data-ttu-id="64813-114">**Poznámka:** některé hello rozhraní API, která provádět volání tooAzure úložiště v ASP.NET Core jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="64813-114">**NOTE:** Some of hello APIs that perform calls tooAzure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="64813-115">V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="64813-115">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="64813-116">Následující kód Hello předpokládá, že asynchronní programování metody jsou používány.</span><span class="sxs-lookup"><span data-stu-id="64813-116">hello code below assumes async programming methods are being used.</span></span>

* <span data-ttu-id="64813-117">V tématu [Začínáme s Azure Queue storage pomocí rozhraní .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) Další informace o programu manipulace s fronty.</span><span class="sxs-lookup"><span data-stu-id="64813-117">See [Get started with Azure Queue storage using .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) for more information on programmatically manipulating queues.</span></span>
* <span data-ttu-id="64813-118">V tématu [úložiště dokumentace](https://azure.microsoft.com/documentation/services/storage/) obecné informace o službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="64813-118">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="64813-119">V tématu [cloudové služby dokumentaci](https://azure.microsoft.com/documentation/services/cloud-services/) obecné informace o cloudových služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="64813-119">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="64813-120">V tématu [ASP.NET](http://www.asp.net) Další informace o programování aplikací ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="64813-120">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

## <a name="access-queues-in-code"></a><span data-ttu-id="64813-121">Přístup front v kódu</span><span class="sxs-lookup"><span data-stu-id="64813-121">Access queues in code</span></span>
<span data-ttu-id="64813-122">fronty tooaccess projektů ASP.NET Core, potřebujete následující hello tooinclude položky tooany C# zdrojový soubor, který přistupuje k Azure queue storage.</span><span class="sxs-lookup"><span data-stu-id="64813-122">tooaccess queues in ASP.NET Core projects, you need tooinclude hello following items tooany C# source file that accesses Azure queue storage.</span></span>

1. <span data-ttu-id="64813-123">Deklarace oborů názvů hello hello horní části souboru hello C# zahrnout tyto **pomocí** příkazy.</span><span class="sxs-lookup"><span data-stu-id="64813-123">Make sure hello namespace declarations at hello top of hello C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="64813-124">Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="64813-124">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="64813-125">Následující kód tooget hello použití hello připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure hello.</span><span class="sxs-lookup"><span data-stu-id="64813-125">Use hello following code tooget hello your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. <span data-ttu-id="64813-126">Získání **CloudQueueClient** objektu tooreference hello queue – objekty v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="64813-126">Get a **CloudQueueClient** object tooreference hello queue objects in your storage account.</span></span>  
   
        // Create hello CloudQueueClient object for hello storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. <span data-ttu-id="64813-127">Získání **CloudQueue** objektu tooreference konkrétní fronty.</span><span class="sxs-lookup"><span data-stu-id="64813-127">Get a **CloudQueue** object tooreference a specific queue.</span></span>
   
        // Get a reference toohello CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

<span data-ttu-id="64813-128">**Poznámka:** používat všechny hello výše kódu před hello kódu v hello následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="64813-128">**NOTE:** Use all of hello above code in front of hello code in hello following samples.</span></span>

### <a name="create-a-queue-in-code"></a><span data-ttu-id="64813-129">Vytvoření fronty v kódu</span><span class="sxs-lookup"><span data-stu-id="64813-129">Create a queue in code</span></span>
<span data-ttu-id="64813-130">toocreate hello fronty Azure v kódu, stačí přidat volání příliš**CreateIfNotExistsAsync**.</span><span class="sxs-lookup"><span data-stu-id="64813-130">toocreate hello Azure queue in code, just add a call too**CreateIfNotExistsAsync**.</span></span>

    // Create hello CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

## <a name="add-a-message-tooa-queue"></a><span data-ttu-id="64813-131">Přidat tooa fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="64813-131">Add a message tooa queue</span></span>
<span data-ttu-id="64813-132">tooinsert zprávu do existující fronty, vytvořte novou **CloudQueueMessage** objekt a potom volání hello **AddMessageAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="64813-132">tooinsert a message into an existing queue, create a new **CloudQueueMessage** object, then call hello **AddMessageAsync** method.</span></span>

<span data-ttu-id="64813-133">A **CloudQueueMessage** můžete vytvořit objekt z řetězce (ve formátu UTF-8) nebo pole bajtů.</span><span class="sxs-lookup"><span data-stu-id="64813-133">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="64813-134">Tady je příklad, který se vloží uvítací zprávu "Hello, World".</span><span class="sxs-lookup"><span data-stu-id="64813-134">Here is an example which inserts hello message 'Hello, World'.</span></span>

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

## <a name="read-a-message-in-a-queue"></a><span data-ttu-id="64813-135">Čtení zprávy ve frontě</span><span class="sxs-lookup"><span data-stu-id="64813-135">Read a message in a queue</span></span>
<span data-ttu-id="64813-136">Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello pomocí volání hello **PeekMessageAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="64813-136">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **PeekMessageAsync** method.</span></span>

    // Peek hello next message in hello queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


## <a name="read-and-remove-a-message-in-a-queue"></a><span data-ttu-id="64813-137">Přečtěte si a odebrat zprávu ve frontě</span><span class="sxs-lookup"><span data-stu-id="64813-137">Read and remove a message in a queue</span></span>
<span data-ttu-id="64813-138">Váš kód můžete odebrat (dequeue –) zprávu z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="64813-138">Your code can remove (dequeue) a message from a queue in two steps.</span></span>

1. <span data-ttu-id="64813-139">Volání **GetMessageAsync** tooget hello další zprávu ve frontě.</span><span class="sxs-lookup"><span data-stu-id="64813-139">Call **GetMessageAsync** tooget hello next message in a queue.</span></span> <span data-ttu-id="64813-140">Zpráva vrácená metodou **GetMessageAsync** stane neviditelnou tooany další kód, který čte zprávy z této fronty.</span><span class="sxs-lookup"><span data-stu-id="64813-140">A message returned from **GetMessageAsync** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="64813-141">Ve výchozím nastavení tato zpráva zůstává neviditelná po dobu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="64813-141">By default, this message stays invisible for 30 seconds.</span></span>
2. <span data-ttu-id="64813-142">odebrání uvítací zprávu z fronty hello, volání toofinish **DeleteMessageAsync**.</span><span class="sxs-lookup"><span data-stu-id="64813-142">toofinish removing hello message from hello queue, call **DeleteMessageAsync**.</span></span>

<span data-ttu-id="64813-143">Tento dvoukrokový proces odebrání zprávy zaručuje, pokud se váš kód nezdaří tooprocess, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejnou zprávu a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="64813-143">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="64813-144">Hello následující kód volání **DeleteMessageAsync** hned po zpracování zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="64813-144">hello following code calls **DeleteMessageAsync** right after hello message has been processed.</span></span>

    // Get hello next message in hello queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process hello message in less than 30 seconds.

    // Then delete hello message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a><span data-ttu-id="64813-145">Využívání dalších možností pro vyřazení zprávy</span><span class="sxs-lookup"><span data-stu-id="64813-145">Leverage additional options for dequeuing messages</span></span>
<span data-ttu-id="64813-146">Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="64813-146">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="64813-147">Nejprve můžete načíst dávku zpráv (až too32).</span><span class="sxs-lookup"><span data-stu-id="64813-147">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="64813-148">Druhý můžete nastavit časový limit neviditelnosti delší nebo kratší, povolení váš kód více nebo méně času toofully zpracování jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="64813-148">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="64813-149">Hello následující příklad kódu používá **GetMessages** metoda tooget 20 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="64813-149">hello following code example uses the **GetMessages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="64813-150">Následně se každá zpráva zpracuje pomocí smyčky **foreach**.</span><span class="sxs-lookup"><span data-stu-id="64813-150">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="64813-151">Nastaví taky hello neviditelnosti časový limit too5 minut pro každou zprávu.</span><span class="sxs-lookup"><span data-stu-id="64813-151">It also sets hello invisibility timeout too5 minutes for each message.</span></span> <span data-ttu-id="64813-152">Všimněte si, že počáteční hello 5 minut pro všechny zprávy v hello stejný čas, takže po 5 minut po volání hello příliš předané**GetMessages**, všechny zprávy, které nebyly odstraněny opět viditelné.</span><span class="sxs-lookup"><span data-stu-id="64813-152">Note that hello 5 minutes start for all messages at hello same time, so after 5 minutes have passed after hello call too**GetMessages**, any messages which have not been deleted become visible again.</span></span>

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-hello-queue-length"></a><span data-ttu-id="64813-153">Získání délky fronty hello</span><span class="sxs-lookup"><span data-stu-id="64813-153">Get hello queue length</span></span>
<span data-ttu-id="64813-154">Můžete získat odhad hello počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="64813-154">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="64813-155">**FetchAttributes** metoda požádá hello službu front o načtení atributů fronty hello, včetně počtu zpráv hello.</span><span class="sxs-lookup"><span data-stu-id="64813-155">The **FetchAttributes** method asks hello queue service to retrieve hello queue attributes, including hello message count.</span></span> <span data-ttu-id="64813-156">Hello **ApproximateMethodCount** vlastnost vrací hello poslední hodnotu načtenou **FetchAttributes** metoda bez volání služby front hello.</span><span class="sxs-lookup"><span data-stu-id="64813-156">hello **ApproximateMethodCount** property returns hello last value retrieved by the **FetchAttributes** method, without calling hello queue service.</span></span>

    // Fetch hello queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve hello cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display hello number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-hello-async-await-pattern-with-common-queue-apis"></a><span data-ttu-id="64813-157">Použití vzoru Async-Await hello s obecné frontě rozhraní API</span><span class="sxs-lookup"><span data-stu-id="64813-157">Use hello Async-Await pattern with common queue APIs</span></span>
<span data-ttu-id="64813-158">Tento příklad ukazuje, jak toouse hello vzoru Async-Await s obecné frontě rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="64813-158">This example shows how toouse hello Async-Await pattern with common queue APIs.</span></span> <span data-ttu-id="64813-159">Hello Ukázka volání hello asynchronní verzi každé z hello zadané metody.</span><span class="sxs-lookup"><span data-stu-id="64813-159">hello sample calls hello async version of each of hello given methods.</span></span> <span data-ttu-id="64813-160">To může zobrazit hello asynchronní po opravy jednotlivých metod.</span><span class="sxs-lookup"><span data-stu-id="64813-160">This can be seen by hello Async post-fix of each method.</span></span> <span data-ttu-id="64813-161">Při použití asynchronní metody pozastaví hello vzoru Async-Await místní provádění až do dokončení volání hello.</span><span class="sxs-lookup"><span data-stu-id="64813-161">When an async method is used, hello Async-Await pattern suspends local execution until hello call is completed.</span></span> <span data-ttu-id="64813-162">Toto chování umožňuje hello aktuální vlákno toodo jinou práci, která pomáhá zabránit kritická místa výkonu a zlepšuje celkovou rychlost reakce aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="64813-162">This behavior allows hello current thread toodo other work which helps avoid performance bottlenecks and improves hello overall responsiveness of your application.</span></span> <span data-ttu-id="64813-163">Další podrobnosti o použití hello vzoru Async-Await v rozhraní .NET, najdete v části [Async a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span><span class="sxs-lookup"><span data-stu-id="64813-163">For more details on using hello Async-Await pattern in .NET, see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

    // Create a message tooadd toohello queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue hello message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue hello message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete hello message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a><span data-ttu-id="64813-164">Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="64813-164">Delete a queue</span></span>
<span data-ttu-id="64813-165">toodelete frontu a všechny zprávy hello obsažené v něm, volejte **odstranit** metoda na objekt fronty hello.</span><span class="sxs-lookup"><span data-stu-id="64813-165">toodelete a queue and all hello messages contained in it, call the **Delete** method on hello queue object.</span></span>

    // Delete hello queue.
    messageQueue.Delete();


## <a name="next-steps"></a><span data-ttu-id="64813-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="64813-166">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

