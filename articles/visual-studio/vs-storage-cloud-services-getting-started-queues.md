---
title: "aaaGet začít s fronty úložiště a Visual Studio připojených služeb (cloudové služby) | Microsoft Docs"
description: "Jak tooget spustit po připojení tooa účet úložiště pomocí sady Visual Studio připojené služby pomocí Azure Queue storage v projektu cloudové služby v sadě Visual Studio"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: da587aac-5e64-4e9a-8405-44cc1924881d
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 1e90eeb826131cadca90dcb720c931fff5fedcb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a><span data-ttu-id="bd413-103">Začínáme s Azure Queue storage a Visual Studio připojené služby (projekty cloudových služeb)</span><span class="sxs-lookup"><span data-stu-id="bd413-103">Getting started with Azure Queue storage and Visual Studio connected services (cloud services projects)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="bd413-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="bd413-104">Overview</span></span>
<span data-ttu-id="bd413-105">Tento článek popisuje, jak tooget spuštění pomocí Azure Queue storage v sadě Visual Studio, po vytvoření a účet úložiště Azure v projektu cloudové služby odkazuje pomocí sady Visual Studio hello **přidat připojení služby** dialogové okno .</span><span class="sxs-lookup"><span data-stu-id="bd413-105">This article describes how tooget started using Azure Queue storage in Visual Studio after you have created or referenced an Azure storage account in a cloud services project by using hello  Visual Studio **Add Connected Services** dialog.</span></span>

<span data-ttu-id="bd413-106">Ukážeme vám jak toocreate fronty v kódu.</span><span class="sxs-lookup"><span data-stu-id="bd413-106">We'll show you how toocreate a queue in code.</span></span> <span data-ttu-id="bd413-107">Také ukážeme jak tooperform basic fronty operací, jako je přidání, úprava, čtení a odebrání zprávy fronty.</span><span class="sxs-lookup"><span data-stu-id="bd413-107">We'll also show you how tooperform basic queue operations, such as adding, modifying, reading and removing queue messages.</span></span> <span data-ttu-id="bd413-108">Hello ukázky jsou napsané v kódu jazyka C# a používají hello [Microsoft Azure Storage Client Library pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd413-108">hello samples are written in C# code and use hello [Microsoft Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="bd413-109">Hello **přidat připojení služby** operaci nainstaluje hello odpovídající NuGet balíčky tooaccess úložiště Azure ve vašem projektu a přidá hello připojovací řetězec pro hello úložiště účet tooyour projektu konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="bd413-109">hello **Add Connected Services** operation installs hello appropriate NuGet packages tooaccess Azure storage in your project and adds hello connection string for hello storage account tooyour project configuration files.</span></span>

* <span data-ttu-id="bd413-110">V tématu [Začínáme s Azure Queue storage pomocí rozhraní .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) Další informace o práci s front v kódu.</span><span class="sxs-lookup"><span data-stu-id="bd413-110">See [Get started with Azure Queue storage using .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) for more information on manipulating queues in code.</span></span>
* <span data-ttu-id="bd413-111">V tématu [úložiště dokumentace](https://azure.microsoft.com/documentation/services/storage/) obecné informace o službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bd413-111">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="bd413-112">V tématu [cloudové služby dokumentaci](https://azure.microsoft.com/documentation/services/cloud-services/) obecné informace o cloudových služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="bd413-112">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="bd413-113">V tématu [ASP.NET](http://www.asp.net) Další informace o programování aplikací ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bd413-113">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

<span data-ttu-id="bd413-114">Azure Queue storage je služba pro ukládání velkého počtu zpráv, které lze přistupovat z libovolné místo v hello, world prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bd413-114">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="bd413-115">Zpráva s jednou frontou může být až velikost too64 KB a jedna fronta můžete obsahovat miliony zpráv, až toohello limit celkové kapacity účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="bd413-115">A single queue message can be up too64 KB in size, and a queue can contain millions of messages, up toohello total capacity limit of a storage account.</span></span>

## <a name="access-queues-in-code"></a><span data-ttu-id="bd413-116">Přístup front v kódu</span><span class="sxs-lookup"><span data-stu-id="bd413-116">Access queues in code</span></span>
<span data-ttu-id="bd413-117">tooaccess fronty v projektech Visual Studio cloudové služby, je nutné tooinclude hello následující položky tooany C# zdrojový soubor, který přístup k Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="bd413-117">tooaccess queues in Visual Studio Cloud Services projects, you need tooinclude hello following items tooany C# source file that access Azure Queue storage.</span></span>

1. <span data-ttu-id="bd413-118">Deklarace oborů názvů hello hello horní části souboru hello C# zahrnout tyto **pomocí** příkazy.</span><span class="sxs-lookup"><span data-stu-id="bd413-118">Make sure hello namespace declarations at hello top of hello C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
2. <span data-ttu-id="bd413-119">Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="bd413-119">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="bd413-120">Následující kód tooget hello použití hello připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure hello.</span><span class="sxs-lookup"><span data-stu-id="bd413-120">Use hello following code tooget hello your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. <span data-ttu-id="bd413-121">Získání **CloudQueueClient** objektu tooreference hello queue – objekty v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="bd413-121">Get a **CloudQueueClient** object tooreference hello queue objects in your storage account.</span></span>  
   
        // Create hello queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. <span data-ttu-id="bd413-122">Získání **CloudQueue** objektu tooreference konkrétní fronty.</span><span class="sxs-lookup"><span data-stu-id="bd413-122">Get a **CloudQueue** object tooreference a specific queue.</span></span>
   
        // Get a reference tooa queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

<span data-ttu-id="bd413-123">**Poznámka:** používat všechny hello výše kódu před hello kódu v hello následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="bd413-123">**NOTE:** Use all of hello above code in front of hello code in hello following samples.</span></span>

## <a name="create-a-queue-in-code"></a><span data-ttu-id="bd413-124">Vytvoření fronty v kódu</span><span class="sxs-lookup"><span data-stu-id="bd413-124">Create a queue in code</span></span>
<span data-ttu-id="bd413-125">toocreate hello fronty v kódu, stačí přidat volání příliš**CreateIfNotExists**.</span><span class="sxs-lookup"><span data-stu-id="bd413-125">toocreate hello queue in code, just add a call too**CreateIfNotExists**.</span></span>

    // Create hello CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-tooa-queue"></a><span data-ttu-id="bd413-126">Přidat tooa fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="bd413-126">Add a message tooa queue</span></span>
<span data-ttu-id="bd413-127">tooinsert zprávu do existující fronty, vytvořte novou **CloudQueueMessage** objekt a potom volání hello **AddMessage** metoda.</span><span class="sxs-lookup"><span data-stu-id="bd413-127">tooinsert a message into an existing queue, create a new **CloudQueueMessage** object, then call hello **AddMessage** method.</span></span>

<span data-ttu-id="bd413-128">A **CloudQueueMessage** můžete vytvořit objekt z řetězce (ve formátu UTF-8) nebo pole bajtů.</span><span class="sxs-lookup"><span data-stu-id="bd413-128">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="bd413-129">Tady je příklad, který se vloží uvítací zprávu "Hello, World".</span><span class="sxs-lookup"><span data-stu-id="bd413-129">Here is an example which inserts hello message 'Hello, World'.</span></span>

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a><span data-ttu-id="bd413-130">Čtení zprávy ve frontě</span><span class="sxs-lookup"><span data-stu-id="bd413-130">Read a message in a queue</span></span>
<span data-ttu-id="bd413-131">Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello pomocí volání hello **PeekMessage** metoda.</span><span class="sxs-lookup"><span data-stu-id="bd413-131">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **PeekMessage** method.</span></span>

    // Peek at hello next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a><span data-ttu-id="bd413-132">Přečtěte si a odebrat zprávu ve frontě</span><span class="sxs-lookup"><span data-stu-id="bd413-132">Read and remove a message in a queue</span></span>
<span data-ttu-id="bd413-133">Můžete odebrat kódu (zrušte fronty) zprávu z fronty ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="bd413-133">Your code can remove (de-queue) a message from a queue in two steps.</span></span>

1. <span data-ttu-id="bd413-134">Volání **GetMessage** tooget hello další zprávu ve frontě.</span><span class="sxs-lookup"><span data-stu-id="bd413-134">Call **GetMessage** tooget hello next message in a queue.</span></span> <span data-ttu-id="bd413-135">Zpráva vrácená metodou **GetMessage** stane neviditelnou tooany další kód, který čte zprávy z této fronty.</span><span class="sxs-lookup"><span data-stu-id="bd413-135">A message returned from **GetMessage** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="bd413-136">Ve výchozím nastavení tato zpráva zůstává neviditelná po dobu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="bd413-136">By default, this message stays invisible for 30 seconds.</span></span>
2. <span data-ttu-id="bd413-137">odebrání uvítací zprávu z fronty hello, volání toofinish **DeleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="bd413-137">toofinish removing hello message from hello queue, call **DeleteMessage**.</span></span>

<span data-ttu-id="bd413-138">Tento dvoukrokový proces odebrání zprávy zaručuje, pokud se váš kód nezdaří tooprocess, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejnou zprávu a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="bd413-138">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="bd413-139">Hello následující kód volání **DeleteMessage** hned po zpracování zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="bd413-139">hello following code calls **DeleteMessage** right after hello message has been processed.</span></span>

    // Get hello next message in hello queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process hello message in less than 30 seconds

    // Then delete hello message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-tooprocess-and-remove-queue-messages"></a><span data-ttu-id="bd413-140">Použít další možnosti tooprocess a odebrání zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="bd413-140">Use additional options tooprocess and remove queue messages</span></span>
<span data-ttu-id="bd413-141">Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="bd413-141">There are two ways you can customize message retrieval from a queue.</span></span>

* <span data-ttu-id="bd413-142">Můžete načíst dávku zpráv (až too32).</span><span class="sxs-lookup"><span data-stu-id="bd413-142">You can get a batch of messages (up too32).</span></span>
* <span data-ttu-id="bd413-143">Můžete nastavit časový limit neviditelnosti delší nebo kratší, povolení váš kód více nebo méně času toofully zpracování jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="bd413-143">You can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="bd413-144">Hello následující příklad kódu používá **GetMessages** metoda tooget 20 zpráv v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="bd413-144">hello following code example uses the **GetMessages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="bd413-145">Následně se každá zpráva zpracuje pomocí smyčky **foreach**.</span><span class="sxs-lookup"><span data-stu-id="bd413-145">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="bd413-146">Nastaví taky hello neviditelnosti časový limit toofive minut pro každou zprávu.</span><span class="sxs-lookup"><span data-stu-id="bd413-146">It also sets hello invisibility timeout toofive minutes for each message.</span></span> <span data-ttu-id="bd413-147">Všimněte si, že hello 5 minut spustí pro všechny zprávy s hello stejný čas, takže po 5 minut od volání hello příliš předané**GetMessages**, všechny zprávy, které nebyly odstraněny, opět viditelné.</span><span class="sxs-lookup"><span data-stu-id="bd413-147">Note that hello 5 minutes starts for all messages at hello same time, so after 5 minutes have passed since hello call too**GetMessages**, any messages which have not been deleted will become visible again.</span></span>

<span data-ttu-id="bd413-148">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="bd413-148">Here's an example:</span></span>

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete hello message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-hello-queue-length"></a><span data-ttu-id="bd413-149">Získání délky fronty hello</span><span class="sxs-lookup"><span data-stu-id="bd413-149">Get hello queue length</span></span>
<span data-ttu-id="bd413-150">Můžete získat odhad hello počet zpráv ve frontě.</span><span class="sxs-lookup"><span data-stu-id="bd413-150">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="bd413-151">**FetchAttributes** metoda požádá hello službu front o načtení atributů fronty hello, včetně počtu zpráv hello.</span><span class="sxs-lookup"><span data-stu-id="bd413-151">The **FetchAttributes** method asks hello Queue service to retrieve hello queue attributes, including hello message count.</span></span> <span data-ttu-id="bd413-152">Hello **ApproximateMethodCount** vlastnost vrací hello poslední hodnotu načtenou **FetchAttributes** metoda bez volání služby front hello.</span><span class="sxs-lookup"><span data-stu-id="bd413-152">hello **ApproximateMethodCount** property returns hello last value retrieved by the **FetchAttributes** method, without calling hello Queue service.</span></span>

    // Fetch hello queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve hello cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-hello-async-await-pattern-with-common-azure-queue-apis"></a><span data-ttu-id="bd413-153">Hello použití vzoru Async-Await s běžné fronty rozhraní API služby Azure</span><span class="sxs-lookup"><span data-stu-id="bd413-153">Use hello Async-Await Pattern with common Azure Queue APIs</span></span>
<span data-ttu-id="bd413-154">Tento příklad ukazuje, jak vzor Async-Await hello toouse společné rozhraní API fronty Azure.</span><span class="sxs-lookup"><span data-stu-id="bd413-154">This example shows how toouse hello Async-Await pattern with common Azure Queue APIs.</span></span> <span data-ttu-id="bd413-155">Ukázka Hello volá hello asynchronní verzi každé z hello zadané metody, to může zobrazit hello **asynchronní** po opravy jednotlivých metod.</span><span class="sxs-lookup"><span data-stu-id="bd413-155">hello sample calls hello async version of each of hello given methods, this can be seen by hello **Async** post-fix of each method.</span></span> <span data-ttu-id="bd413-156">Při použití asynchronní metody je použité hello async-await vzor pozastaví místní spuštění, dokud se nedokončí volání hello.</span><span class="sxs-lookup"><span data-stu-id="bd413-156">When an async method is used hello async-await pattern suspends local execution until hello call completes.</span></span> <span data-ttu-id="bd413-157">Toto chování umožňuje hello aktuální vlákno toodo jinou práci, která pomáhá zabránit kritická místa výkonu a zlepšuje celkovou rychlost reakce aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="bd413-157">This behavior allows hello current thread toodo other work which helps avoid performance bottlenecks and improves hello overall responsiveness of your application.</span></span> <span data-ttu-id="bd413-158">Další podrobnosti o použití hello vzoru Async-Await v rozhraní .NET najdete v části [Async a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span><span class="sxs-lookup"><span data-stu-id="bd413-158">For more details on using hello Async-Await pattern in .NET see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

    // Create a message tooput in hello queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add hello message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue hello message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete hello message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a><span data-ttu-id="bd413-159">Odstranění fronty</span><span class="sxs-lookup"><span data-stu-id="bd413-159">Delete a queue</span></span>
<span data-ttu-id="bd413-160">fronty a všechny zprávy hello obsažené v něm volání hello toodelete **odstranit** metoda na objekt fronty hello.</span><span class="sxs-lookup"><span data-stu-id="bd413-160">toodelete a queue and all hello messages contained in it, call hello **Delete** method on hello queue object.</span></span>

    // Delete hello queue.
    messageQueue.Delete();

## <a name="next-steps"></a><span data-ttu-id="bd413-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bd413-161">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

