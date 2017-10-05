---
title: "Začínáme s fronty úložiště a Visual Studio připojené služby (ASP.NET Core) | Microsoft Docs"
description: "Jak začít používat fronty Azure storage v projektu ASP.NET Core v sadě Visual Studio"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 4622496544ce6e1057ac68a2e9946917573e997e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a><span data-ttu-id="6807e-103">Začínáme s fronty úložiště a Visual Studio připojené služby (ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="6807e-103">Get started with queue storage and Visual Studio connected services (ASP.NET Core)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="6807e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="6807e-104">Overview</span></span>
<span data-ttu-id="6807e-105">Tento článek popisuje, jak začít používat Azure Queue storage v sadě Visual Studio, po vytvoření a odkazuje pomocí sady Visual Studio účet úložiště Azure v projektu ASP.NET Core **přidat připojení služby** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6807e-105">This article describes how to get started using Azure Queue storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using the Visual Studio **Add Connected Services** dialog.</span></span> <span data-ttu-id="6807e-106">**Přidat připojení služby** operaci nainstaluje příslušné balíčky NuGet pro přístup k úložišti Azure ve vašem projektu a přidá připojovací řetězec pro účet úložiště v konfiguračních souborech projektu.</span><span class="sxs-lookup"><span data-stu-id="6807e-106">The **Add Connected Services** operation installs the appropriate NuGet packages to access Azure storage in your project and adds the connection string for the storage account to your project configuration files.</span></span>

<span data-ttu-id="6807e-107">Azure queue storage je služba pro ukládání velkého počtu zpráv, které jsou přístupné odkudkoli na světě prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6807e-107">Azure queue storage is a service for storing large numbers of messages that can be accessed from anywhere in the world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="6807e-108">Zpráva s jednou frontou může být až 64 kilobajtů (KB) velikost a jedna fronta můžete obsahovat miliony zpráv, až do limitu celkové kapacity účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="6807e-108">A single queue message can be up to 64 kilobytes (KB) in size, and a queue can contain millions of messages, up to the total capacity limit of a storage account.</span></span>

<span data-ttu-id="6807e-109">Abyste mohli začít, musíte nejprve vytvořit fronty Azure ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="6807e-109">To get started, you first need to create an Azure queue in your storage account.</span></span> <span data-ttu-id="6807e-110">Ukážeme vám postup vytvoření fronty v kódu.</span><span class="sxs-lookup"><span data-stu-id="6807e-110">We'll show you how to create a queue in code.</span></span> <span data-ttu-id="6807e-111">Jsme budete také ukazují, jak provést základní fronty operací, jako je přidání, úprava, čtení a odebrání zprávy do fronty.</span><span class="sxs-lookup"><span data-stu-id="6807e-111">We'll also show you how to perform basic queue operations, such as adding, modifying, reading, and removing queue messages.</span></span> <span data-ttu-id="6807e-112">Ukázky jsou napsané v jazyce C\# kód a použít knihovnu klienta služby Azure Storage pro .NET.</span><span class="sxs-lookup"><span data-stu-id="6807e-112">The samples are written in C\# code and use the Azure Storage Client Library for .NET.</span></span> <span data-ttu-id="6807e-113">Další informace o technologii ASP.NET najdete v tématu [ASP.NET](http://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="6807e-113">For more information about ASP.NET, see [ASP.NET](http://www.asp.net).</span></span>

<span data-ttu-id="6807e-114">**Poznámka:** některé z rozhraní API, která provádět volání do úložiště Azure v ASP.NET Core jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="6807e-114">**NOTE:** Some of the APIs that perform calls to Azure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="6807e-115">V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="6807e-115">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="6807e-116">Následující kód předpokládá, že asynchronní programování metody jsou používány.</span><span class="sxs-lookup"><span data-stu-id="6807e-116">The code below assumes async programming methods are being used.</span></span>

* <span data-ttu-id="6807e-117">V tématu [Začínáme s Azure Queue storage pomocí rozhraní .NET](storage-dotnet-how-to-use-queues.md) Další informace o programu manipulace s fronty.</span><span class="sxs-lookup"><span data-stu-id="6807e-117">See [Get started with Azure Queue storage using .NET](storage-dotnet-how-to-use-queues.md) for more information on programmatically manipulating queues.</span></span>
* <span data-ttu-id="6807e-118">V tématu [úložiště dokumentace](https://azure.microsoft.com/documentation/services/storage/) obecné informace o službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6807e-118">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="6807e-119">V tématu [cloudové služby dokumentaci](https://azure.microsoft.com/documentation/services/cloud-services/) obecné informace o cloudových služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="6807e-119">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="6807e-120">V tématu [ASP.NET](http://www.asp.net) Další informace o programování aplikací ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6807e-120">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

## <a name="access-queues-in-code"></a><span data-ttu-id="6807e-121">Přístup front v kódu</span><span class="sxs-lookup"><span data-stu-id="6807e-121">Access queues in code</span></span>
<span data-ttu-id="6807e-122">Chcete-li získat přístup k frontám projektů ASP.NET Core, zahrnují následující položky do zdrojového souboru C#, která používá fronty Azure storage.</span><span class="sxs-lookup"><span data-stu-id="6807e-122">To access queues in ASP.NET Core projects, you need to include the following items to any C# source file that accesses Azure queue storage.</span></span>

1. <span data-ttu-id="6807e-123">Zajistěte, aby deklarace oboru názvů v horní části souboru C# zahrnují tyto **pomocí** příkazy.</span><span class="sxs-lookup"><span data-stu-id="6807e-123">Make sure the namespace declarations at the top of the C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="6807e-124">Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="6807e-124">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="6807e-125">Použít následující kód k získání připojovací řetězec úložiště a informace o účtu úložiště z konfigurace služby Azure.</span><span class="sxs-lookup"><span data-stu-id="6807e-125">Use the following code to get the your storage connection string and storage account information from the Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. <span data-ttu-id="6807e-126">Získání **CloudQueueClient** objekt, který má odkazovat na objekty fronty ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="6807e-126">Get a **CloudQueueClient** object to reference the queue objects in your storage account.</span></span>  
   
        // Create the CloudQueueClient object for the storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. <span data-ttu-id="6807e-127">Získání **CloudQueue** objekt, který má odkazovat na konkrétní fronty.</span><span class="sxs-lookup"><span data-stu-id="6807e-127">Get a **CloudQueue** object to reference a specific queue.</span></span>
   
        // Get a reference to the CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

<span data-ttu-id="6807e-128">**Poznámka:** používat všechny výše uvedené kódu před kód v následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="6807e-128">**NOTE:** Use all of the above code in front of the code in the following samples.</span></span>

### <a name="create-a-queue-in-code"></a><span data-ttu-id="6807e-129">Vytvoření fronty v kódu</span><span class="sxs-lookup"><span data-stu-id="6807e-129">Create a queue in code</span></span>
<span data-ttu-id="6807e-130">Pokud chcete vytvořit fronty Azure v kódu, stačí přidat volání **CreateIfNotExistsAsync**.</span><span class="sxs-lookup"><span data-stu-id="6807e-130">To create the Azure queue in code, just add a call to **CreateIfNotExistsAsync**.</span></span>

    // Create the CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

## <a name="add-a-message-to-a-queue"></a><span data-ttu-id="6807e-131">Přidání zprávy do fronty</span><span class="sxs-lookup"><span data-stu-id="6807e-131">Add a message to a queue</span></span>
<span data-ttu-id="6807e-132">Pokud chcete vložit zprávu do existující fronty, vytvořte novou **CloudQueueMessage** objekt a potom volání **AddMessageAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="6807e-132">To insert a message into an existing queue, create a new **CloudQueueMessage** object, then call the **AddMessageAsync** method.</span></span>

<span data-ttu-id="6807e-133">A **CloudQueueMessage** můžete vytvořit objekt z řetězce (ve formátu UTF-8) nebo pole bajtů.</span><span class="sxs-lookup"><span data-stu-id="6807e-133">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="6807e-134">Tady je příklad, který se vloží zprávu "Hello, World".</span><span class="sxs-lookup"><span data-stu-id="6807e-134">Here is an example which inserts the message 'Hello, World'.</span></span>

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

## <a name="read-a-message-in-a-queue"></a><span data-ttu-id="6807e-135">Čtení zprávy ve frontě</span><span class="sxs-lookup"><span data-stu-id="6807e-135">Read a message in a queue</span></span>
<span data-ttu-id="6807e-136">Můžete prohlížet zprávy ve frontě bez odebere ji z fronty voláním **PeekMessageAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="6807e-136">You can peek at the message in the front of a queue without removing it from the queue by calling the **PeekMessageAsync** method.</span></span>

    // Peek the next message in the queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


## <a name="read-and-remove-a-message-in-a-queue"></a><span data-ttu-id="6807e-137">Přečtěte si a odebrat zprávu ve frontě</span><span class="sxs-lookup"><span data-stu-id="6807e-137">Read and remove a message in a queue</span></span>
<span data-ttu-id="6807e-138">Váš kód můžete odebrat (dequeue –) zprávu z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="6807e-138">Your code can remove (dequeue) a message from a queue in two steps.</span></span>

1. <span data-ttu-id="6807e-139">Volání **GetMessageAsync** získat další zprávu ve frontě.</span><span class="sxs-lookup"><span data-stu-id="6807e-139">Call **GetMessageAsync** to get the next message in a queue.</span></span> <span data-ttu-id="6807e-140">Zpráva vrácená metodou **GetMessageAsync** stane neviditelnou pro jakýkoli jiný kód čtení zpráv z této fronty.</span><span class="sxs-lookup"><span data-stu-id="6807e-140">A message returned from **GetMessageAsync** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="6807e-141">Ve výchozím nastavení tato zpráva zůstává neviditelná po dobu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="6807e-141">By default, this message stays invisible for 30 seconds.</span></span>
2. <span data-ttu-id="6807e-142">Chcete-li dokončit odebere ji z fronty, volejte **DeleteMessageAsync**.</span><span class="sxs-lookup"><span data-stu-id="6807e-142">To finish removing the message from the queue, call **DeleteMessageAsync**.</span></span>

<span data-ttu-id="6807e-143">Tento dvoukrokový proces odebrání zprávy zaručuje, aby v případě, že se vašemu kódu nepodaří zprávu zpracovat z důvodu selhání hardwaru nebo softwaru, mohla stejnou zprávu získat jiná instance vašeho kódu a bylo možné to zkusit znovu.</span><span class="sxs-lookup"><span data-stu-id="6807e-143">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="6807e-144">Následující kód volání **DeleteMessageAsync** pravým po zpracování zprávy.</span><span class="sxs-lookup"><span data-stu-id="6807e-144">The following code calls **DeleteMessageAsync** right after the message has been processed.</span></span>

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process the message in less than 30 seconds.

    // Then delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a><span data-ttu-id="6807e-145">Využívání dalších možností pro vyřazení zprávy</span><span class="sxs-lookup"><span data-stu-id="6807e-145">Leverage additional options for dequeuing messages</span></span>
<span data-ttu-id="6807e-146">Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="6807e-146">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="6807e-147">Za prvé si můžete načíst dávku zpráv (až 32).</span><span class="sxs-lookup"><span data-stu-id="6807e-147">First, you can get a batch of messages (up to 32).</span></span> <span data-ttu-id="6807e-148">Za druhé si můžete nastavit delší nebo kratší časový limit neviditelnosti, aby měl váš kód více nebo méně času na úplné zpracování jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="6807e-148">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span> <span data-ttu-id="6807e-149">V následujícím příkladu kódu se pomocí metody **GetMessages** získá 20 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="6807e-149">The following code example uses the **GetMessages** method to get 20 messages in one call.</span></span> <span data-ttu-id="6807e-150">Následně se každá zpráva zpracuje pomocí smyčky **foreach**.</span><span class="sxs-lookup"><span data-stu-id="6807e-150">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="6807e-151">Také nastaví časový limit neviditelnosti 5 minut pro každou zprávu.</span><span class="sxs-lookup"><span data-stu-id="6807e-151">It also sets the invisibility timeout to 5 minutes for each message.</span></span> <span data-ttu-id="6807e-152">Všimněte si, že spuštění 5 minut pro všechny zprávy ve stejnou dobu, takže po 5 minutách mít předán po zavolání **GetMessages**, všechny zprávy, které nebyly odstraněny opět viditelné.</span><span class="sxs-lookup"><span data-stu-id="6807e-152">Note that the 5 minutes start for all messages at the same time, so after 5 minutes have passed after the call to **GetMessages**, any messages which have not been deleted become visible again.</span></span>

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a><span data-ttu-id="6807e-153">Získání délky fronty</span><span class="sxs-lookup"><span data-stu-id="6807e-153">Get the queue length</span></span>
<span data-ttu-id="6807e-154">Podle potřeby můžete získat odhadovaný počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="6807e-154">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="6807e-155">**FetchAttributes** metoda požádá službu front o načtení atributů fronty, včetně počtu zpráv.</span><span class="sxs-lookup"><span data-stu-id="6807e-155">The **FetchAttributes** method asks the queue service to retrieve the queue attributes, including the message count.</span></span> <span data-ttu-id="6807e-156">**ApproximateMethodCount** vlastnost vrací poslední hodnotu načtenou **FetchAttributes** metoda bez volání služby front.</span><span class="sxs-lookup"><span data-stu-id="6807e-156">The **ApproximateMethodCount** property returns the last value retrieved by the **FetchAttributes** method, without calling the queue service.</span></span>

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display the number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a><span data-ttu-id="6807e-157">Použití vzoru Async-Await s obecné frontě rozhraní API</span><span class="sxs-lookup"><span data-stu-id="6807e-157">Use the Async-Await pattern with common queue APIs</span></span>
<span data-ttu-id="6807e-158">Tento příklad ukazuje způsob použití vzoru Async-Await s obecné frontě rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6807e-158">This example shows how to use the Async-Await pattern with common queue APIs.</span></span> <span data-ttu-id="6807e-159">Ukázka volá asynchronní verzi každé z daných metod.</span><span class="sxs-lookup"><span data-stu-id="6807e-159">The sample calls the async version of each of the given methods.</span></span> <span data-ttu-id="6807e-160">To může zobrazit opravu asynchronní po každé metody.</span><span class="sxs-lookup"><span data-stu-id="6807e-160">This can be seen by the Async post-fix of each method.</span></span> <span data-ttu-id="6807e-161">Při použití asynchronní metody pozastaví vzoru Async-Await místní provádění až do dokončení volání.</span><span class="sxs-lookup"><span data-stu-id="6807e-161">When an async method is used, the Async-Await pattern suspends local execution until the call is completed.</span></span> <span data-ttu-id="6807e-162">Toto chování umožňuje aktuálnímu vláknu provádět další činnosti, což přispívá k vyhnout se kritickým bodům výkonu a zlepšuje celkovou rychlost reakce aplikace.</span><span class="sxs-lookup"><span data-stu-id="6807e-162">This behavior allows the current thread to do other work which helps avoid performance bottlenecks and improves the overall responsiveness of your application.</span></span> <span data-ttu-id="6807e-163">Další podrobnosti o použití vzoru Async-Await v rozhraní .NET najdete v tématu [Async a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span><span class="sxs-lookup"><span data-stu-id="6807e-163">For more details on using the Async-Await pattern in .NET, see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

    // Create a message to add to the queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a><span data-ttu-id="6807e-164">Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="6807e-164">Delete a queue</span></span>
<span data-ttu-id="6807e-165">Pokud budete chtít odstranit frontu se všemi zprávami, které v ní jsou, zavolejte metodu **Delete** pro objekt fronty.</span><span class="sxs-lookup"><span data-stu-id="6807e-165">To delete a queue and all the messages contained in it, call the **Delete** method on the queue object.</span></span>

    // Delete the queue.
    messageQueue.Delete();


## <a name="next-steps"></a><span data-ttu-id="6807e-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6807e-166">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

