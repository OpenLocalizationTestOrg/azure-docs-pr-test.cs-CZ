---
title: "Začínáme s fronty úložiště a Visual Studio připojené služby (cloudové služby) | Microsoft Docs"
description: "Jak začít používat Azure Queue storage v projektu cloudové služby v sadě Visual Studio po připojení k účtu úložiště pomocí sady Visual Studio připojené služby"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: da587aac-5e64-4e9a-8405-44cc1924881d
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: d0e6e3eab312f169b1d05ba16e2e293e103df1ec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a><span data-ttu-id="13f53-103">Začínáme s Azure Queue storage a Visual Studio připojené služby (projekty cloudových služeb)</span><span class="sxs-lookup"><span data-stu-id="13f53-103">Getting started with Azure Queue storage and Visual Studio connected services (cloud services projects)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="13f53-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="13f53-104">Overview</span></span>
<span data-ttu-id="13f53-105">Tento článek popisuje, jak začít používat Azure Queue storage v sadě Visual Studio, po vytvoření a účet úložiště Azure v projektu cloudové služby odkazuje pomocí sady Visual Studio **přidat připojení služby** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="13f53-105">This article describes how to get started using Azure Queue storage in Visual Studio after you have created or referenced an Azure storage account in a cloud services project by using the  Visual Studio **Add Connected Services** dialog.</span></span>

<span data-ttu-id="13f53-106">Ukážeme vám postup vytvoření fronty v kódu.</span><span class="sxs-lookup"><span data-stu-id="13f53-106">We'll show you how to create a queue in code.</span></span> <span data-ttu-id="13f53-107">Jsme budete také ukazují, jak provést základní fronty operací, jako je přidání, úprava, čtení a odebrání zprávy do fronty.</span><span class="sxs-lookup"><span data-stu-id="13f53-107">We'll also show you how to perform basic queue operations, such as adding, modifying, reading and removing queue messages.</span></span> <span data-ttu-id="13f53-108">Ukázky jsou napsané v C# – kód a použít [Microsoft Azure Storage Client Library pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="13f53-108">The samples are written in C# code and use the [Microsoft Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="13f53-109">**Přidat připojení služby** operaci nainstaluje příslušné balíčky NuGet pro přístup k úložišti Azure ve vašem projektu a přidá připojovací řetězec pro účet úložiště v konfiguračních souborech projektu.</span><span class="sxs-lookup"><span data-stu-id="13f53-109">The **Add Connected Services** operation installs the appropriate NuGet packages to access Azure storage in your project and adds the connection string for the storage account to your project configuration files.</span></span>

* <span data-ttu-id="13f53-110">V tématu [Začínáme s Azure Queue storage pomocí rozhraní .NET](storage-dotnet-how-to-use-queues.md) Další informace o práci s front v kódu.</span><span class="sxs-lookup"><span data-stu-id="13f53-110">See [Get started with Azure Queue storage using .NET](storage-dotnet-how-to-use-queues.md) for more information on manipulating queues in code.</span></span>
* <span data-ttu-id="13f53-111">V tématu [úložiště dokumentace](https://azure.microsoft.com/documentation/services/storage/) obecné informace o službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="13f53-111">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="13f53-112">V tématu [cloudové služby dokumentaci](https://azure.microsoft.com/documentation/services/cloud-services/) obecné informace o cloudových služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="13f53-112">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="13f53-113">V tématu [ASP.NET](http://www.asp.net) Další informace o programování aplikací ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="13f53-113">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

<span data-ttu-id="13f53-114">Azure Queue Storage je služba pro ukládání velkého počtu zpráv, ke které můžete získat přístup z jakéhokoli místa na světě prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="13f53-114">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in the world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="13f53-115">Zpráva s jednou frontou může mít velikost až 64 kB a jedna fronta můžete obsahovat miliony zpráv, až do dosažení celkové kapacity účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="13f53-115">A single queue message can be up to 64 KB in size, and a queue can contain millions of messages, up to the total capacity limit of a storage account.</span></span>

## <a name="access-queues-in-code"></a><span data-ttu-id="13f53-116">Přístup front v kódu</span><span class="sxs-lookup"><span data-stu-id="13f53-116">Access queues in code</span></span>
<span data-ttu-id="13f53-117">Chcete-li získat přístup k frontám v projektech Visual Studio cloudové služby, zahrnují následující položky pro všechny zdrojové soubory C#, která přistupují k Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="13f53-117">To access queues in Visual Studio Cloud Services projects, you need to include the following items to any C# source file that access Azure Queue storage.</span></span>

1. <span data-ttu-id="13f53-118">Zajistěte, aby deklarace oboru názvů v horní části souboru C# zahrnují tyto **pomocí** příkazy.</span><span class="sxs-lookup"><span data-stu-id="13f53-118">Make sure the namespace declarations at the top of the C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
2. <span data-ttu-id="13f53-119">Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="13f53-119">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="13f53-120">Použít následující kód k získání připojovací řetězec úložiště a informace o účtu úložiště z konfigurace služby Azure.</span><span class="sxs-lookup"><span data-stu-id="13f53-120">Use the following code to get the your storage connection string and storage account information from the Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. <span data-ttu-id="13f53-121">Získání **CloudQueueClient** objekt, který má odkazovat na objekty fronty ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="13f53-121">Get a **CloudQueueClient** object to reference the queue objects in your storage account.</span></span>  
   
        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. <span data-ttu-id="13f53-122">Získání **CloudQueue** objekt, který má odkazovat na konkrétní fronty.</span><span class="sxs-lookup"><span data-stu-id="13f53-122">Get a **CloudQueue** object to reference a specific queue.</span></span>
   
        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

<span data-ttu-id="13f53-123">**Poznámka:** používat všechny výše uvedené kódu před kód v následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="13f53-123">**NOTE:** Use all of the above code in front of the code in the following samples.</span></span>

## <a name="create-a-queue-in-code"></a><span data-ttu-id="13f53-124">Vytvoření fronty v kódu</span><span class="sxs-lookup"><span data-stu-id="13f53-124">Create a queue in code</span></span>
<span data-ttu-id="13f53-125">Chcete-li vytvořit frontu v kódu, stačí přidat volání **CreateIfNotExists**.</span><span class="sxs-lookup"><span data-stu-id="13f53-125">To create the queue in code, just add a call to **CreateIfNotExists**.</span></span>

    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a><span data-ttu-id="13f53-126">Přidání zprávy do fronty</span><span class="sxs-lookup"><span data-stu-id="13f53-126">Add a message to a queue</span></span>
<span data-ttu-id="13f53-127">Pokud chcete vložit zprávu do existující fronty, vytvořte novou **CloudQueueMessage** objekt a potom volání **AddMessage** metoda.</span><span class="sxs-lookup"><span data-stu-id="13f53-127">To insert a message into an existing queue, create a new **CloudQueueMessage** object, then call the **AddMessage** method.</span></span>

<span data-ttu-id="13f53-128">A **CloudQueueMessage** můžete vytvořit objekt z řetězce (ve formátu UTF-8) nebo pole bajtů.</span><span class="sxs-lookup"><span data-stu-id="13f53-128">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="13f53-129">Tady je příklad, který se vloží zprávu "Hello, World".</span><span class="sxs-lookup"><span data-stu-id="13f53-129">Here is an example which inserts the message 'Hello, World'.</span></span>

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a><span data-ttu-id="13f53-130">Čtení zprávy ve frontě</span><span class="sxs-lookup"><span data-stu-id="13f53-130">Read a message in a queue</span></span>
<span data-ttu-id="13f53-131">Pomocí volání metody **PeekMessage** můžete prohlížet zprávy ve frontě, aniž byste je z fronty odebrali.</span><span class="sxs-lookup"><span data-stu-id="13f53-131">You can peek at the message in the front of a queue without removing it from the queue by calling the **PeekMessage** method.</span></span>

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a><span data-ttu-id="13f53-132">Přečtěte si a odebrat zprávu ve frontě</span><span class="sxs-lookup"><span data-stu-id="13f53-132">Read and remove a message in a queue</span></span>
<span data-ttu-id="13f53-133">Můžete odebrat kódu (zrušte fronty) zprávu z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="13f53-133">Your code can remove (de-queue) a message from a queue in two steps.</span></span>

1. <span data-ttu-id="13f53-134">Volání **GetMessage** získat další zprávu ve frontě.</span><span class="sxs-lookup"><span data-stu-id="13f53-134">Call **GetMessage** to get the next message in a queue.</span></span> <span data-ttu-id="13f53-135">Zpráva vrácená metodou **GetMessage** se stane neviditelnou pro jakýkoli jiný kód, který čte zprávy z této fronty.</span><span class="sxs-lookup"><span data-stu-id="13f53-135">A message returned from **GetMessage** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="13f53-136">Ve výchozím nastavení tato zpráva zůstává neviditelná po dobu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="13f53-136">By default, this message stays invisible for 30 seconds.</span></span>
2. <span data-ttu-id="13f53-137">Chcete-li dokončit odebere ji z fronty, volejte **DeleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="13f53-137">To finish removing the message from the queue, call **DeleteMessage**.</span></span>

<span data-ttu-id="13f53-138">Tento dvoukrokový proces odebrání zprávy zaručuje, aby v případě, že se vašemu kódu nepodaří zprávu zpracovat z důvodu selhání hardwaru nebo softwaru, mohla stejnou zprávu získat jiná instance vašeho kódu a bylo možné to zkusit znovu.</span><span class="sxs-lookup"><span data-stu-id="13f53-138">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="13f53-139">Následující kód volání **DeleteMessage** pravým po zpracování zprávy.</span><span class="sxs-lookup"><span data-stu-id="13f53-139">The following code calls **DeleteMessage** right after the message has been processed.</span></span>

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-to-process-and-remove-queue-messages"></a><span data-ttu-id="13f53-140">Použít další možnosti pro zpracování a odebrat zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="13f53-140">Use additional options to process and remove queue messages</span></span>
<span data-ttu-id="13f53-141">Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="13f53-141">There are two ways you can customize message retrieval from a queue.</span></span>

* <span data-ttu-id="13f53-142">Můžete načíst dávku zpráv (až 32).</span><span class="sxs-lookup"><span data-stu-id="13f53-142">You can get a batch of messages (up to 32).</span></span>
* <span data-ttu-id="13f53-143">Můžete nastavit časový limit neviditelnosti delší nebo kratší, aby měl váš kód více nebo méně času na úplné zpracování jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="13f53-143">You can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span> <span data-ttu-id="13f53-144">V následujícím příkladu kódu se pomocí metody **GetMessages** získá 20 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="13f53-144">The following code example uses the **GetMessages** method to get 20 messages in one call.</span></span> <span data-ttu-id="13f53-145">Následně se každá zpráva zpracuje pomocí smyčky **foreach**.</span><span class="sxs-lookup"><span data-stu-id="13f53-145">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="13f53-146">Také se pro každou zprávu nastaví časový limit neviditelnosti 5 minut.</span><span class="sxs-lookup"><span data-stu-id="13f53-146">It also sets the invisibility timeout to five minutes for each message.</span></span> <span data-ttu-id="13f53-147">Pozor, 5minutový časový limit začíná pro všechny zprávy najednou, takže po uplynutí 5 minut od volání metody **GetMessages** pak budou všechny zprávy, které nebyly odstraněny, opět viditelné.</span><span class="sxs-lookup"><span data-stu-id="13f53-147">Note that the 5 minutes starts for all messages at the same time, so after 5 minutes have passed since the call to **GetMessages**, any messages which have not been deleted will become visible again.</span></span>

<span data-ttu-id="13f53-148">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="13f53-148">Here's an example:</span></span>

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete the message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-the-queue-length"></a><span data-ttu-id="13f53-149">Získání délky fronty</span><span class="sxs-lookup"><span data-stu-id="13f53-149">Get the queue length</span></span>
<span data-ttu-id="13f53-150">Podle potřeby můžete získat odhadovaný počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="13f53-150">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="13f53-151">Metoda **FetchAttributes** požádá Službu front o načtení atributů fronty, včetně počtu zpráv.</span><span class="sxs-lookup"><span data-stu-id="13f53-151">The **FetchAttributes** method asks the Queue service to retrieve the queue attributes, including the message count.</span></span> <span data-ttu-id="13f53-152">**ApproximateMethodCount** vlastnost vrací poslední hodnotu načtenou **FetchAttributes** metoda bez volání služby front.</span><span class="sxs-lookup"><span data-stu-id="13f53-152">The **ApproximateMethodCount** property returns the last value retrieved by the **FetchAttributes** method, without calling the Queue service.</span></span>

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-azure-queue-apis"></a><span data-ttu-id="13f53-153">Použití vzoru Async-Await s běžné fronty rozhraní API služby Azure</span><span class="sxs-lookup"><span data-stu-id="13f53-153">Use the Async-Await Pattern with common Azure Queue APIs</span></span>
<span data-ttu-id="13f53-154">Tento příklad ukazuje způsob použití vzoru Async-Await s běžné fronty rozhraní API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="13f53-154">This example shows how to use the Async-Await pattern with common Azure Queue APIs.</span></span> <span data-ttu-id="13f53-155">Ukázka volá asynchronní verzi každé z daných metod, to může zobrazit **asynchronní** po opravy jednotlivých metod.</span><span class="sxs-lookup"><span data-stu-id="13f53-155">The sample calls the async version of each of the given methods, this can be seen by the **Async** post-fix of each method.</span></span> <span data-ttu-id="13f53-156">Při použití asynchronní metody slouží async-await vzor pozastaví místní spuštění, dokud se nedokončí volání.</span><span class="sxs-lookup"><span data-stu-id="13f53-156">When an async method is used the async-await pattern suspends local execution until the call completes.</span></span> <span data-ttu-id="13f53-157">Toto chování umožňuje aktuálnímu vláknu provádět další činnosti, což přispívá k vyhnout se kritickým bodům výkonu a zlepšuje celkovou rychlost reakce aplikace.</span><span class="sxs-lookup"><span data-stu-id="13f53-157">This behavior allows the current thread to do other work which helps avoid performance bottlenecks and improves the overall responsiveness of your application.</span></span> <span data-ttu-id="13f53-158">Další podrobnosti o použití vzoru Async-Await v rozhraní .NET najdete v tématu [Async a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).</span><span class="sxs-lookup"><span data-stu-id="13f53-158">For more details on using the Async-Await pattern in .NET see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add the message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete the message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a><span data-ttu-id="13f53-159">Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="13f53-159">Delete a queue</span></span>
<span data-ttu-id="13f53-160">Pokud budete chtít odstranit frontu se všemi zprávami, které v ní jsou, zavolejte metodu **Delete** pro objekt fronty.</span><span class="sxs-lookup"><span data-stu-id="13f53-160">To delete a queue and all the messages contained in it, call the **Delete** method on the queue object.</span></span>

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a><span data-ttu-id="13f53-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="13f53-161">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

