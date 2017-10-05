---
title: "Začínáme s fronty úložiště a Visual Studio připojené služeb (webové úlohy projekty) | Microsoft Docs"
description: "Jak začít pracovat po připojení k účtu úložiště pomocí sady Visual Studio pomocí Azure Queue storage v projektu webové úlohy připojený služby."
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 5c3ef267-2a67-44e9-ab4a-1edd7015034f
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: abd4814c099620345e04833e14dafd38432064e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-webjob-projects"></a><span data-ttu-id="0a0f3-103">Začínáme s Azure Queue storage a Visual Studio připojené služeb (webové úlohy projekty)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-103">Getting started with Azure Queue storage and Visual Studio connected services (WebJob Projects)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="0a0f3-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="0a0f3-104">Overview</span></span>
<span data-ttu-id="0a0f3-105">Tento článek popisuje, jak začít používat Azure Queue storage v projektu Visual Studio Azure Webjobs po vytvoření a odkazuje pomocí sady Visual Studio účet úložiště Azure **přidat připojení služby** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-105">This article describes how get started using Azure Queue storage in a Visual Studio Azure WebJob project after you have created or referenced an Azure storage account by using the Visual Studio  **Add Connected Services** dialog box.</span></span> <span data-ttu-id="0a0f3-106">Při přidávání účtu úložiště do projektu úlohy WebJob pomocí sady Visual Studio **přidat připojení služby** dialogové okno, nejsou nainstalovány odpovídající balíčky NuGet pro úložiště Azure, odkazy na příslušné .NET jsou přidány do projektu a připojovací řetězce pro účet úložiště se aktualizují v souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-106">When you add a storage account to a WebJob project by using the Visual Studio **Add Connected Services** dialog, the appropriate Azure Storage NuGet packages are installed, the appropriate .NET references are added to the project, and connection strings for the storage account are updated in the App.config file.</span></span>  

<span data-ttu-id="0a0f3-107">Tento článek obsahuje C# ukázek kódu, které ukazují, jak používat Azure WebJobs SDK verze 1.x se službou Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-107">This article provides C# code samples that show how to use the Azure WebJobs SDK version 1.x with the Azure Queue storage service.</span></span>

<span data-ttu-id="0a0f3-108">Azure Queue Storage je služba pro ukládání velkého počtu zpráv, ke které můžete získat přístup z jakéhokoli místa na světě prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-108">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in the world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="0a0f3-109">Zpráva s jednou frontou může mít velikost až 64 kB a jedna fronta můžete obsahovat miliony zpráv, až do dosažení celkové kapacity účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-109">A single queue message can be up to 64 KB in size, and a queue can contain millions of messages, up to the total capacity limit of a storage account.</span></span> <span data-ttu-id="0a0f3-110">V tématu [Začínáme s Azure Queue Storage pomocí rozhraní .NET](storage-dotnet-how-to-use-queues.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-110">See [Get started with Azure Queue Storage using .NET](storage-dotnet-how-to-use-queues.md) for more information.</span></span> <span data-ttu-id="0a0f3-111">Další informace o technologii ASP.NET najdete v tématu [ASP.NET](http://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-111">For more information about ASP.NET, see [ASP.NET](http://www.asp.net).</span></span>

## <a name="how-to-trigger-a-function-when-a-queue-message-is-received"></a><span data-ttu-id="0a0f3-112">Postup aktivace funkce při příjmu zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="0a0f3-112">How to trigger a function when a queue message is received</span></span>
<span data-ttu-id="0a0f3-113">Chcete-li vytvořit funkci, která volá WebJobs SDK při příjmu zprávy fronty, použijte **QueueTrigger** atribut.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-113">To write a function that the WebJobs SDK calls when a queue message is received, use the **QueueTrigger** attribute.</span></span> <span data-ttu-id="0a0f3-114">Konstruktoru atributu přijímá řetězcový parametr, který určuje název fronty pro cyklické dotazování.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-114">The attribute constructor takes a string parameter that specifies the name of the queue to poll.</span></span> <span data-ttu-id="0a0f3-115">Chcete-li zjistit, jak nastavit název fronty dynamicky, podívejte se na [jak nastavit možnosti konfigurace](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-115">To see how to set the queue name dynamically, check out [How to set Configuration Options](#how-to-set-configuration-options).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="0a0f3-116">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="0a0f3-116">String queue messages</span></span>
<span data-ttu-id="0a0f3-117">V následujícím příkladu fronty obsahuje řetězec zprávu, takže **QueueTrigger** se použije pro parametr řetězec s názvem **logMessage** obsahující obsah zprávy ve frontě.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-117">In the following example, the queue contains a string message, so **QueueTrigger** is applied to a string parameter named **logMessage** which contains the content of the queue message.</span></span> <span data-ttu-id="0a0f3-118">Funkce [zapíše zprávu protokolu na řídicí panel](#how-to-write-logs).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-118">The function [writes a log message to the Dashboard](#how-to-write-logs).</span></span>

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

<span data-ttu-id="0a0f3-119">Kromě **řetězec**, tento parametr může být bajtové pole, **CloudQueueMessage** objekt nebo objektů POCO, které definujete.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-119">Besides **string**, the parameter may be a byte array, a **CloudQueueMessage** object, or a POCO  that you define.</span></span>

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="0a0f3-120">Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="0a0f3-120">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="0a0f3-121">V následujícím příkladu obsahuje zprávy ve frontě JSON pro **BlobInformation** objekt, který zahrnuje **BlobName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-121">In the following example, the queue message contains JSON for a **BlobInformation** object which includes a **BlobName** property.</span></span> <span data-ttu-id="0a0f3-122">Sada SDK automaticky deserializuje objekt.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-122">The SDK automatically deserializes the object.</span></span>

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

<span data-ttu-id="0a0f3-123">Sada SDK používá [balíček Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) k serializaci a deserializaci zprávy.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-123">The SDK uses the [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) to serialize and deserialize messages.</span></span> <span data-ttu-id="0a0f3-124">Pokud vytvoříte fronty zpráv v aplikaci, která nepoužívá WebJobs SDK, můžete napsat kód jako v následujícím příkladu pro vytvoření zprávy fronty objektů POCO, které mohou analyzovat sady SDK.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-124">If you create queue messages in a program that doesn't use the WebJobs SDK, you can write code like the following example to create a POCO queue message that the SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a><span data-ttu-id="0a0f3-125">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="0a0f3-125">Async functions</span></span>
<span data-ttu-id="0a0f3-126">Následující funkce asynchronní [zapíše do protokolu na řídicí panel](#how-to-write-logs).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-126">The following async function [writes a log to the Dashboard](#how-to-write-logs).</span></span>

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

<span data-ttu-id="0a0f3-127">Asynchronní funkce může trvat [token zrušení](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), jak je znázorněno v následujícím příkladu, který se zkopíruje do objektu blob.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-127">Async functions may take a [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), as shown in the following example which copies a blob.</span></span> <span data-ttu-id="0a0f3-128">(Další informace o **queueTrigger** zástupného symbolu, najdete v článku [objekty BLOB](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) části.)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-128">(For an explanation of the **queueTrigger** placeholder, see the [Blobs](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) section.)</span></span>

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

## <a name="types-the-queuetrigger-attribute-works-with"></a><span data-ttu-id="0a0f3-129">Typy atribut QueueTrigger pracuje s</span><span class="sxs-lookup"><span data-stu-id="0a0f3-129">Types the QueueTrigger attribute works with</span></span>
<span data-ttu-id="0a0f3-130">Můžete použít **QueueTrigger** s následujícími typy:</span><span class="sxs-lookup"><span data-stu-id="0a0f3-130">You can use **QueueTrigger** with the following types:</span></span>

* <span data-ttu-id="0a0f3-131">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="0a0f3-131">**string**</span></span>
* <span data-ttu-id="0a0f3-132">Typ objektů POCO serializovanou jako JSON</span><span class="sxs-lookup"><span data-stu-id="0a0f3-132">A POCO type serialized as JSON</span></span>
* <span data-ttu-id="0a0f3-133">**Byte]**</span><span class="sxs-lookup"><span data-stu-id="0a0f3-133">**byte[]**</span></span>
* <span data-ttu-id="0a0f3-134">**CloudQueueMessage**</span><span class="sxs-lookup"><span data-stu-id="0a0f3-134">**CloudQueueMessage**</span></span>

## <a name="polling-algorithm"></a><span data-ttu-id="0a0f3-135">Algoritmus dotazování</span><span class="sxs-lookup"><span data-stu-id="0a0f3-135">Polling algorithm</span></span>
<span data-ttu-id="0a0f3-136">Sada SDK implementuje náhodných exponenciální back vypnout algoritmus, aby se snížil dopad nečinnosti-fronty dotazování na transakce náklady na úložiště.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-136">The SDK implements a random exponential back-off algorithm to reduce the effect of idle-queue polling on storage transaction costs.</span></span>  <span data-ttu-id="0a0f3-137">Když se najde zprávu, sadu SDK vyčká dvou sekund a poté zkontroluje další zprávu; Pokud je nalezena žádná zpráva čeká před dalším pokusem o 4 sekundy.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-137">When a message is found, the SDK waits two seconds and then checks for another message; when no message is found it waits about four seconds before trying again.</span></span> <span data-ttu-id="0a0f3-138">Po následné neúspěšných pokusech o získání zprávu fronty doba čekání nadále zvýšit, dokud nedosáhne maximální čekací doba, výchozí nastavení je na jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-138">After subsequent failed attempts to get a queue message, the wait time continues to increase until it reaches the maximum wait time, which defaults to one minute.</span></span> <span data-ttu-id="0a0f3-139">[Maximální čekací doba je konfigurovatelná](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-139">[The maximum wait time is configurable](#how-to-set-configuration-options).</span></span>

## <a name="multiple-instances"></a><span data-ttu-id="0a0f3-140">Více instancí</span><span class="sxs-lookup"><span data-stu-id="0a0f3-140">Multiple instances</span></span>
<span data-ttu-id="0a0f3-141">Pokud vaše webová aplikace běží na několik instancí, nepřetržité webové úlohy se spustí na každém počítači, a každý počítač bude čekat na aktivační události a pokus o spuštění funkce.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-141">If your web app runs on multiple instances, a continuous WebJobs runs on each machine, and each machine will wait for triggers and attempt to run functions.</span></span> <span data-ttu-id="0a0f3-142">V některých případech to může vést k některé funkce dvakrát zpracování stejná data takže funkce by měl být idempotent (napsané tak, aby opakovaně volání se stejnými vstupními daty neobsahuje duplicitní výsledky).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-142">In some scenarios this can lead to some functions processing the same data twice, so functions should be idempotent (written so that calling them repeatedly with the same input data doesn't produce duplicate results).</span></span>  

## <a name="parallel-execution"></a><span data-ttu-id="0a0f3-143">Paralelní provádění</span><span class="sxs-lookup"><span data-stu-id="0a0f3-143">Parallel execution</span></span>
<span data-ttu-id="0a0f3-144">Pokud máte víc funkcí naslouchá na různých front, sadu SDK je zavolá paralelně přijetí zpráv současně.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-144">If you have multiple functions listening on different queues, the SDK will call them in parallel when messages are received simultaneously.</span></span>

<span data-ttu-id="0a0f3-145">Totéž platí při přijetí více zpráv pro jednu frontu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-145">The same is true when multiple messages are received for a single queue.</span></span> <span data-ttu-id="0a0f3-146">Ve výchozím nastavení sada SDK získá dávku 16 fronty zpráv v čase a provádí funkce, která je zpracuje paralelně.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-146">By default, the SDK gets a batch of 16 queue messages at a time and executes the function that processes them in parallel.</span></span> <span data-ttu-id="0a0f3-147">[Velikost dávky je možné konfigurovat](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-147">[The batch size is configurable](#how-to-set-configuration-options).</span></span> <span data-ttu-id="0a0f3-148">Když počet zpracovávaných získá na polovinu velikost dávky, sadu SDK získá další dávku a spustí zpracování těchto zpráv.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-148">When the number being processed gets down to half of the batch size, the SDK gets another batch and starts processing those messages.</span></span> <span data-ttu-id="0a0f3-149">Proto je maximální počet souběžných zpráv, které jsou zpracovány za funkce jeden a půl krát velikost dávky.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-149">Therefore the maximum number of concurrent messages being processed per function is one and a half times the batch size.</span></span> <span data-ttu-id="0a0f3-150">Toto omezení se vztahuje samostatně pro každou funkci, která má **QueueTrigger** atribut.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-150">This limit applies separately to each function that has a **QueueTrigger** attribute.</span></span> <span data-ttu-id="0a0f3-151">Pokud nechcete, aby paralelní zpracování zprávy přijaté v jedné frontě, nastavte velikost dávky na 1.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-151">If you don't want parallel execution for messages received on one queue, set the batch size to 1.</span></span>

## <a name="get-queue-or-queue-message-metadata"></a><span data-ttu-id="0a0f3-152">Získání fronty nebo fronty zpráv metadat</span><span class="sxs-lookup"><span data-stu-id="0a0f3-152">Get queue or queue message metadata</span></span>
<span data-ttu-id="0a0f3-153">Následující vlastnosti zprávy můžete získat tak, že přidáte parametry pro podpis metody:</span><span class="sxs-lookup"><span data-stu-id="0a0f3-153">You can get the following message properties by adding parameters to the method signature:</span></span>

* <span data-ttu-id="0a0f3-154">**Datový typ DateTimeOffset** expirationTime</span><span class="sxs-lookup"><span data-stu-id="0a0f3-154">**DateTimeOffset** expirationTime</span></span>
* <span data-ttu-id="0a0f3-155">**Datový typ DateTimeOffset** insertionTime</span><span class="sxs-lookup"><span data-stu-id="0a0f3-155">**DateTimeOffset** insertionTime</span></span>
* <span data-ttu-id="0a0f3-156">**Datový typ DateTimeOffset** nextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="0a0f3-156">**DateTimeOffset** nextVisibleTime</span></span>
* <span data-ttu-id="0a0f3-157">**řetězec** queueTrigger (obsahuje text zprávy)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-157">**string** queueTrigger (contains message text)</span></span>
* <span data-ttu-id="0a0f3-158">**řetězec** id</span><span class="sxs-lookup"><span data-stu-id="0a0f3-158">**string** id</span></span>
* <span data-ttu-id="0a0f3-159">**řetězec** vlastností popReceipt</span><span class="sxs-lookup"><span data-stu-id="0a0f3-159">**string** popReceipt</span></span>
* <span data-ttu-id="0a0f3-160">**int** dequeueCount</span><span class="sxs-lookup"><span data-stu-id="0a0f3-160">**int** dequeueCount</span></span>

<span data-ttu-id="0a0f3-161">Pokud chcete pracovat přímo s úložištěm Azure API, můžete také přidat **CloudStorageAccount** parametr.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-161">If you want to work directly with the Azure storage API, you can also add a **CloudStorageAccount** parameter.</span></span>

<span data-ttu-id="0a0f3-162">Následující příklad všechny tato metadata zapíše do protokolu informace o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-162">The following example writes all of this metadata to an INFO application log.</span></span> <span data-ttu-id="0a0f3-163">V příkladu logMessage a queueTrigger obsahovat obsah zprávy ve frontě.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-163">In the example, both logMessage and queueTrigger contain the content of the queue message.</span></span>

        public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
            DateTimeOffset expirationTime,
            DateTimeOffset insertionTime,
            DateTimeOffset nextVisibleTime,
            string id,
            string popReceipt,
            int dequeueCount,
            string queueTrigger,
            CloudStorageAccount cloudStorageAccount,
            TextWriter logger)
        {
            logger.WriteLine(
                "logMessage={0}\n" +
            "expirationTime={1}\ninsertionTime={2}\n" +
                "nextVisibleTime={3}\n" +
                "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
                "queue endpoint={7} queueTrigger={8}",
                logMessage, expirationTime,
                insertionTime,
                nextVisibleTime, id,
                popReceipt, dequeueCount,
                cloudStorageAccount.QueueEndpoint,
                queueTrigger);
        }

<span data-ttu-id="0a0f3-164">Tady je příklad protokolu o zapsaných správcem ukázkový kód:</span><span class="sxs-lookup"><span data-stu-id="0a0f3-164">Here is a sample log written by the sample code:</span></span>

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

## <a name="graceful-shutdown"></a><span data-ttu-id="0a0f3-165">Řádné vypnutí</span><span class="sxs-lookup"><span data-stu-id="0a0f3-165">Graceful shutdown</span></span>
<span data-ttu-id="0a0f3-166">Funkci, která běží v nepřetržitá webová úloha může přijmout **CancellationToken** parametr, který umožňuje operačního systému a upozorňovaly vytvářené webové úlohy je možné ukončit funkce.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-166">A function that runs in a continuous WebJob can accept a **CancellationToken** parameter which enables the operating system to notify the function when the WebJob is about to be terminated.</span></span> <span data-ttu-id="0a0f3-167">Toto oznámení můžete Ujistěte se, že funkce nepodporuje neočekávaně ukončí tak, aby data ponechá v nekonzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-167">You can use this notification to make sure the function doesn't terminate unexpectedly in a way that leaves data in an inconsistent state.</span></span>

<span data-ttu-id="0a0f3-168">Následující příklad ukazuje, jak zkontrolovat pro předcházení ukončení webové úlohy ve funkci.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-168">The following example shows how to check for impending WebJob termination in a function.</span></span>

    public static void GracefulShutdownDemo(
                [QueueTrigger("inputqueue")] string inputText,
                TextWriter logger,
                CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(1000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }

<span data-ttu-id="0a0f3-169">**Poznámka:** řídicí panel nemusí zobrazit správně, stav a výstup funkcí, které byly vypnuty.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-169">**Note:** The Dashboard might not correctly show the status and output of functions that have been shut down.</span></span>

<span data-ttu-id="0a0f3-170">Další informace najdete v tématu [WebJobs řádné vypnutí](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-170">For more information, see [WebJobs Graceful Shutdown](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span></span>   

## <a name="how-to-create-a-queue-message-while-processing-a-queue-message"></a><span data-ttu-id="0a0f3-171">Postup vytvoření fronty zpráv při zpracování zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="0a0f3-171">How to create a queue message while processing a queue message</span></span>
<span data-ttu-id="0a0f3-172">Chcete-li vytvořit funkci, která vytvoří novou zprávu fronty, použijte **fronty** atribut.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-172">To write a function that creates a new queue message, use the **Queue** attribute.</span></span> <span data-ttu-id="0a0f3-173">Jako **QueueTrigger**, kterou předáte název fronty jako řetězec, nebo můžete [nastavit název fronty dynamicky](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-173">Like **QueueTrigger**, you pass in the queue name as a string or you can [set the queue name dynamically](#how-to-set-configuration-options).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="0a0f3-174">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="0a0f3-174">String queue messages</span></span>
<span data-ttu-id="0a0f3-175">Následující ukázka kódu bez asynchronní vytvoří novou zprávu fronty ve frontě s názvem "outputqueue" se stejným obsahem jako zprávy ve frontě dostali frontu s názvem "inputqueue".</span><span class="sxs-lookup"><span data-stu-id="0a0f3-175">The following non-async code sample creates a new queue message in the queue named "outputqueue" with the same content as the queue message received in the queue named "inputqueue".</span></span> <span data-ttu-id="0a0f3-176">(Pro asynchronní použít funkce **IAsyncCollector<T>**  znázorněné později v této části.)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-176">(For async functions use **IAsyncCollector<T>** as shown later in this section.)</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="0a0f3-177">Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="0a0f3-177">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="0a0f3-178">Vytvoření fronty zprávu, která obsahuje objektů POCO spíše než řetězec, předá typ objektů POCO jako výstupní parametr k **fronty** konstruktoru atributu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-178">To create a queue message that contains a POCO rather than a string, pass the POCO type as an output parameter to the **Queue** attribute constructor.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

<span data-ttu-id="0a0f3-179">Sada SDK automaticky serializuje objekt do formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-179">The SDK automatically serializes the object to JSON.</span></span> <span data-ttu-id="0a0f3-180">Zprávu fronty je vytvořena vždy, i v případě, že objekt má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-180">A queue message is always created, even if the object is null.</span></span>

### <a name="create-multiple-messages-or-in-async-functions"></a><span data-ttu-id="0a0f3-181">Vytvoření více zpráv nebo asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="0a0f3-181">Create multiple messages or in async functions</span></span>
<span data-ttu-id="0a0f3-182">Pokud chcete vytvořit více zpráv, ujistěte se, typ parametru pro výstupní fronty **ICollector<T>**  nebo **IAsyncCollector<T>**, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-182">To create multiple messages, make the parameter type for the output queue **ICollector<T>** or **IAsyncCollector<T>**, as shown in the following example.</span></span>

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="0a0f3-183">Každou zprávu fronty je vytvořena ihned po **přidat** metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-183">Each queue message is created immediately when the **Add** method is called.</span></span>

### <a name="types-that-the-queue-attribute-works-with"></a><span data-ttu-id="0a0f3-184">Typy, které atribut fronty pracuje s</span><span class="sxs-lookup"><span data-stu-id="0a0f3-184">Types that the Queue attribute works with</span></span>
<span data-ttu-id="0a0f3-185">Můžete použít **fronty** atribut na následující typy parametrů:</span><span class="sxs-lookup"><span data-stu-id="0a0f3-185">You can use the **Queue** attribute on the following parameter types:</span></span>

* <span data-ttu-id="0a0f3-186">**na řetězce** (vytvoří zprávu fronty, pokud je hodnota parametru jinou hodnotu než null při ukončení funkce)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-186">**out string** (creates queue message if parameter value is non-null when the function ends)</span></span>
* <span data-ttu-id="0a0f3-187">**out byte []** (funguje jako **řetězec**)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-187">**out byte[]** (works like **string**)</span></span>
* <span data-ttu-id="0a0f3-188">**out CloudQueueMessage** (funguje jako **řetězec**)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-188">**out CloudQueueMessage** (works like **string**)</span></span>
* <span data-ttu-id="0a0f3-189">**out objektů POCO** (typu serializable vytvoří zprávu s objektem hodnotu null. Pokud má parametr hodnotu null při ukončení funkce)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-189">**out POCO** (a serializable type, creates a message with a null object if the paramter is null when the function ends)</span></span>
* <span data-ttu-id="0a0f3-190">**ICollector**</span><span class="sxs-lookup"><span data-stu-id="0a0f3-190">**ICollector**</span></span>
* <span data-ttu-id="0a0f3-191">**IAsyncCollector**</span><span class="sxs-lookup"><span data-stu-id="0a0f3-191">**IAsyncCollector**</span></span>
* <span data-ttu-id="0a0f3-192">**CloudQueue** (pro vytváření zpráv ručně přímo pomocí rozhraní API služby Azure Storage)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-192">**CloudQueue** (for creating messages manually using the Azure Storage API directly)</span></span>

### <a name="use-webjobs-sdk-attributes-in-the-body-of-a-function"></a><span data-ttu-id="0a0f3-193">Použití atributů WebJobs SDK v tělo funkce</span><span class="sxs-lookup"><span data-stu-id="0a0f3-193">Use WebJobs SDK attributes in the body of a function</span></span>
<span data-ttu-id="0a0f3-194">Pokud potřebujete nějakou práci ve vašem funkci před použitím atribut WebJobs SDK, jako **fronty**, **Blob**, nebo **tabulky**, můžete použít **IBinder** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-194">If you need to do some work in your function before using a WebJobs SDK attribute such as **Queue**, **Blob**, or **Table**, you can use the **IBinder** interface.</span></span>

<span data-ttu-id="0a0f3-195">Následující příklad přebírá zprávu vstupní fronty a vytvoří novou zprávu se stejným obsahem v výstupní fronty.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-195">The following example takes an input queue message and creates a new message with the same content in an output queue.</span></span> <span data-ttu-id="0a0f3-196">Název fronty výstupu je nastavena podle kódu v těle funkce.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-196">The output queue name is set by code in the body of the function.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

<span data-ttu-id="0a0f3-197">**IBinder** rozhraní lze také s **tabulky** a **Blob** atributy.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-197">The **IBinder** interface can also be used with the **Table** and **Blob** attributes.</span></span>

## <a name="how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message"></a><span data-ttu-id="0a0f3-198">Tom, jak číst a zapisovat objekty BLOB a tabulek při zpracování zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="0a0f3-198">How to read and write blobs and tables while processing a queue message</span></span>
<span data-ttu-id="0a0f3-199">**Blob** a **tabulky** atributů umožňují čtení a zápisu objektů BLOB a tabulek.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-199">The **Blob** and **Table** attributes enable you to read and write blobs and tables.</span></span> <span data-ttu-id="0a0f3-200">Ukázky v této části se vztahují na objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-200">The samples in this section apply to blobs.</span></span> <span data-ttu-id="0a0f3-201">Ukázky kódu, které ukazují, jak aktivovat procesy při objekty BLOB jsou vytvoření nebo aktualizaci, najdete v části [používání úložiště objektů blob v Azure pomocí WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)a ukázky kódu, které pro čtení a zápis tabulky, najdete v části [použití úložiště tabulek Azure s WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-201">For code samples that show how to trigger processes when blobs are created or updated, see [How to use Azure blob storage with the WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), and for code samples that read and write tables, see [How to use Azure table storage with the WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span></span>

### <a name="string-queue-messages-triggering-blob-operations"></a><span data-ttu-id="0a0f3-202">Zprávy fronty řetězec spuštění operace objektů blob</span><span class="sxs-lookup"><span data-stu-id="0a0f3-202">String queue messages triggering blob operations</span></span>
<span data-ttu-id="0a0f3-203">Pro zprávu fronty, který obsahuje řetězec **queueTrigger** je zástupný symbol v můžete použít **Blob** atributu **blobPath** parametr, který obsahuje obsah zprávy.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-203">For a queue message that contains a string, **queueTrigger** is a placeholder you can use in the **Blob** attribute's **blobPath** parameter that contains the contents of the message.</span></span>

<span data-ttu-id="0a0f3-204">Následující příklad používá **datového proudu** objekty ke čtení a zápisu objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-204">The following example uses **Stream** objects to read and write blobs.</span></span> <span data-ttu-id="0a0f3-205">Zprávy ve frontě je název objektu blob v kontejneru textblobs.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-205">The queue message is the name of a blob located in the textblobs container.</span></span> <span data-ttu-id="0a0f3-206">Kopie objektu blob s "-nové" připojenou k názvu se vytvoří ve stejném kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-206">A copy of the blob with "-new" appended to the name is created in the same container.</span></span>

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="0a0f3-207">**Blob** atributu konstruktoru trvá **blobPath** parametr, který určuje název kontejneru a objektů blob.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-207">The **Blob** attribute constructor takes a **blobPath** parameter that specifies the container and blob name.</span></span> <span data-ttu-id="0a0f3-208">Další informace o tento zástupný text najdete v tématu [používání úložiště objektů blob v Azure pomocí WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-208">For more information about this placeholder, see [How to use Azure blob storage with the WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md).</span></span>

<span data-ttu-id="0a0f3-209">Pokud atribut upraví **datového proudu** objektu určuje jiný parametr konstruktoru **FileAccess** režimu pro čtení, zápisu nebo čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-209">When the attribute decorates a **Stream** object, another constructor parameter specifies the **FileAccess** mode as read, write, or read/write.</span></span>

<span data-ttu-id="0a0f3-210">Následující příklad používá **CloudBlockBlob** objekt, který chcete odstranit objekt blob.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-210">The following example uses a **CloudBlockBlob** object to delete a blob.</span></span> <span data-ttu-id="0a0f3-211">Zprávy ve frontě je název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-211">The queue message is the name of the blob.</span></span>

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="0a0f3-212">Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="0a0f3-212">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="0a0f3-213">Pro objektů POCO uloží jako dokumenty JSON v zprávy ve frontě, můžete použít zástupné znaky, které název vlastnosti v objektu **fronty** atributu **blobPath** parametr.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-213">For a POCO stored as JSON in the queue message, you can use placeholders that name properties of the object in the **Queue** attribute's **blobPath** parameter.</span></span> <span data-ttu-id="0a0f3-214">Můžete taky názvy vlastností fronty metadata zástupnými symboly.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-214">You can also use queue metadata property names as placeholders.</span></span> <span data-ttu-id="0a0f3-215">V tématu [získat fronty nebo fronty zpráv metadata](#get-queue-or-queue-message-metadata).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-215">See [Get queue or queue message metadata](#get-queue-or-queue-message-metadata).</span></span>

<span data-ttu-id="0a0f3-216">Následující příklad zkopíruje objekt blob do nového objektu blob s jiné rozšíření.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-216">The following example copies a blob to a new blob with a different extension.</span></span> <span data-ttu-id="0a0f3-217">Zprávy ve frontě je **BlobInformation** objekt, který zahrnuje **BlobName** a **BlobNameWithoutExtension** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-217">The queue message is a **BlobInformation** object that includes **BlobName** and **BlobNameWithoutExtension** properties.</span></span> <span data-ttu-id="0a0f3-218">Názvy vlastností, které jsou použity jako zástupné symboly v cestě objektu blob pro **Blob** atributy.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-218">The property names are used as placeholders in the blob path for the **Blob** attributes.</span></span>

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="0a0f3-219">Sada SDK používá [balíček Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) k serializaci a deserializaci zprávy.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-219">The SDK uses the [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) to serialize and deserialize messages.</span></span> <span data-ttu-id="0a0f3-220">Pokud vytvoříte fronty zpráv v aplikaci, která nepoužívá WebJobs SDK, můžete napsat kód jako v následujícím příkladu pro vytvoření zprávy fronty objektů POCO, které mohou analyzovat sady SDK.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-220">If you create queue messages in a program that doesn't use the WebJobs SDK, you can write code like the following example to create a POCO queue message that the SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

<span data-ttu-id="0a0f3-221">Pokud potřebujete nějakou práci ve vašem funkci před vazby objektu blob na objekt, můžete pomocí atributu v těle funkce, jak je znázorněno v [pomocí WebJobs SDK atributy v těle funkce](#use-webjobs-sdk-attributes-in-the-body-of-a-function).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-221">If you need to do some work in your function before binding a blob to an object, you can use the attribute in the body of the function, as shown in [Use WebJobs SDK attributes in the body of a function](#use-webjobs-sdk-attributes-in-the-body-of-a-function).</span></span>

### <a name="types-you-can-use-the-blob-attribute-with"></a><span data-ttu-id="0a0f3-222">Typy, které můžete použít atribut objektů Blob s</span><span class="sxs-lookup"><span data-stu-id="0a0f3-222">Types you can use the Blob attribute with</span></span>
<span data-ttu-id="0a0f3-223">**Blob** atribut lze použít s následujícími typy:</span><span class="sxs-lookup"><span data-stu-id="0a0f3-223">The **Blob** attribute can be used with the following types:</span></span>

* <span data-ttu-id="0a0f3-224">**Datový proud** (pro čtení nebo zápisu, zadán pomocí parametru FileAccess konstruktor)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-224">**Stream** (read or write, specified by using the FileAccess constructor parameter)</span></span>
* <span data-ttu-id="0a0f3-225">**TextReader**</span><span class="sxs-lookup"><span data-stu-id="0a0f3-225">**TextReader**</span></span>
* <span data-ttu-id="0a0f3-226">**TextWriter**</span><span class="sxs-lookup"><span data-stu-id="0a0f3-226">**TextWriter**</span></span>
* <span data-ttu-id="0a0f3-227">**řetězec** (přečíst)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-227">**string** (read)</span></span>
* <span data-ttu-id="0a0f3-228">**na řetězce** (zapisovat; vytvoří objekt blob jenom v případě, že parametr řetězce je jinou hodnotu než null, pokud funkce vrátí hodnotu)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-228">**out string** (write; creates a blob only if the string parameter is non-null when the function returns)</span></span>
* <span data-ttu-id="0a0f3-229">Objektů POCO (čtení)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-229">POCO (read)</span></span>
* <span data-ttu-id="0a0f3-230">out objektů POCO (zápisu; vždycky vytvoří objekt blob, vytvoří jako objektu null, pokud parametr objektů POCO má hodnotu null, pokud funkce vrátí hodnotu)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-230">out POCO (write; always creates a blob, creates as null object if POCO parameter is null when the function returns)</span></span>
* <span data-ttu-id="0a0f3-231">**CloudBlobStream** (zápisu)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-231">**CloudBlobStream** (write)</span></span>
* <span data-ttu-id="0a0f3-232">**ICloudBlob** (čtení nebo zápisu)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-232">**ICloudBlob** (read or write)</span></span>
* <span data-ttu-id="0a0f3-233">**CloudBlockBlob** (čtení nebo zápisu)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-233">**CloudBlockBlob** (read or write)</span></span>
* <span data-ttu-id="0a0f3-234">**CloudPageBlob** (čtení nebo zápisu)</span><span class="sxs-lookup"><span data-stu-id="0a0f3-234">**CloudPageBlob** (read or write)</span></span>

## <a name="how-to-handle-poison-messages"></a><span data-ttu-id="0a0f3-235">Postupy: zpracování poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="0a0f3-235">How to handle poison messages</span></span>
<span data-ttu-id="0a0f3-236">Zprávy, jejichž obsah způsobí, že funkce, která se nezdaří se nazývají *škodlivých zpráv*.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-236">Messages whose content causes a function to fail are called *poison messages*.</span></span> <span data-ttu-id="0a0f3-237">Pokud se funkce nezdaří, zprávy ve frontě není odstraněna a nakonec je převzata znovu způsobuje cyklus opakovat.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-237">When the function fails, the queue message is not deleted and eventually is picked up again, causing the cycle to be repeated.</span></span> <span data-ttu-id="0a0f3-238">Sada SDK může automaticky přerušení na cyklus po omezený počet iterací, nebo můžete provést ručně.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-238">The SDK can automatically interrupt the cycle after a limited number of iterations, or you can do it manually.</span></span>

### <a name="automatic-poison-message-handling"></a><span data-ttu-id="0a0f3-239">Zpracování automatické poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="0a0f3-239">Automatic poison message handling</span></span>
<span data-ttu-id="0a0f3-240">Sada SDK bude volat funkci až 5 x zpracovat zprávu fronty.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-240">The SDK will call a function up to 5 times to process a queue message.</span></span> <span data-ttu-id="0a0f3-241">V případě selhání páté zkuste zpráva bude přesunuta do fronty poškozených.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-241">If the fifth try fails, the message is moved to a poison queue.</span></span> <span data-ttu-id="0a0f3-242">Uvidíte, jak nakonfigurovat maximální počet opakovaných pokusů v [jak nastavit možnosti konfigurace](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-242">You can see how to configure the maximum number of retries in [How to set configuration options](#how-to-set-configuration-options).</span></span>

<span data-ttu-id="0a0f3-243">Poškozených fronty jmenuje *{originalqueuename}*-poškozených.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-243">The poison queue is named *{originalqueuename}*-poison.</span></span> <span data-ttu-id="0a0f3-244">Můžete napsat, že je funkce, která se zpracování zpráv z fronty poškozených jejich protokolování nebo odesílání oznámení, že ruční pozornost.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-244">You can write a function to process messages from the poison queue by logging them or sending a notification that manual attention is needed.</span></span>

<span data-ttu-id="0a0f3-245">V následujícím příkladu **CopyBlob** funkce se nezdaří, pokud zpráva fronty obsahuje název objektu blob, který neexistuje.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-245">In the following example the **CopyBlob** function will fail when a queue message contains the name of a blob that doesn't exist.</span></span> <span data-ttu-id="0a0f3-246">Pokud k tomu dojde, je zpráva přesunuta do fronty copyblobqueue poison z fronty copyblobqueue.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-246">When that happens, the message is moved from the copyblobqueue queue to the copyblobqueue-poison queue.</span></span> <span data-ttu-id="0a0f3-247">**ProcessPoisonMessage** pak protokoly poškozená zpráva.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-247">The **ProcessPoisonMessage** then logs the poison message.</span></span>

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

        public static void ProcessPoisonMessage(
            [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
        {
            logger.WriteLine("Failed to copy blob, name=" + blobName);
        }

<span data-ttu-id="0a0f3-248">Následující obrázek znázorňuje výstup konzoly z těchto funkcí při zpracování poškozená zpráva.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-248">The following illustration shows console output from these functions when a poison message is processed.</span></span>

![Výstup konzoly pro zpracování poškozených zpráv](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### <a name="manual-poison-message-handling"></a><span data-ttu-id="0a0f3-250">Zpracování ručních poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="0a0f3-250">Manual poison message handling</span></span>
<span data-ttu-id="0a0f3-251">Počet, kolikrát byla převzata zprávu pro zpracování můžete získat tak, že přidáte **int** parametr s názvem **dequeueCount** na funkce.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-251">You can get the number of times a message has been picked up for processing by adding an **int** parameter named **dequeueCount** to your function.</span></span> <span data-ttu-id="0a0f3-252">Potom můžete zkontrolovat počet dequeue v kódu funkce a provádět vlastní nezpracovatelná zpráva zpracování Pokud počet překročí prahovou hodnotu, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-252">You can then check the dequeue count in function code and perform your own poison message handling when the number exceeds a threshold, as shown in the following example.</span></span>

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed to copy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a name="how-to-set-configuration-options"></a><span data-ttu-id="0a0f3-253">Jak nastavit možnosti konfigurace</span><span class="sxs-lookup"><span data-stu-id="0a0f3-253">How to set configuration options</span></span>
<span data-ttu-id="0a0f3-254">Můžete použít **JobHostConfiguration** typu a nastavte následující možnosti konfigurace:</span><span class="sxs-lookup"><span data-stu-id="0a0f3-254">You can use the **JobHostConfiguration** type to set the following configuration options:</span></span>

* <span data-ttu-id="0a0f3-255">Nastavte SDK připojovacích řetězců v kódu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-255">Set the SDK connection strings in code.</span></span>
* <span data-ttu-id="0a0f3-256">Konfigurace **QueueTrigger** nastavení, jako například maximální počet vyřazení z fronty.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-256">Configure **QueueTrigger** settings such as maximum dequeue count.</span></span>
* <span data-ttu-id="0a0f3-257">Získáte názvy fronty z konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-257">Get queue names from configuration.</span></span>

### <a name="set-sdk-connection-strings-in-code"></a><span data-ttu-id="0a0f3-258">Sada SDK připojovacích řetězců v kódu</span><span class="sxs-lookup"><span data-stu-id="0a0f3-258">Set SDK connection strings in code</span></span>
<span data-ttu-id="0a0f3-259">Nastavení SDK připojovacích řetězců v kódu můžete použít vlastní názvy řetězec připojení v konfiguračních souborů nebo proměnné prostředí, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-259">Setting the SDK connection strings in code enables you to use your own connection string names in configuration files or environment variables, as shown in the following example.</span></span>

        static void Main(string[] args)
        {
            var _storageConn = ConfigurationManager
                .ConnectionStrings["MyStorageConnection"].ConnectionString;

            var _dashboardConn = ConfigurationManager
                .ConnectionStrings["MyDashboardConnection"].ConnectionString;

            var _serviceBusConn = ConfigurationManager
                .ConnectionStrings["MyServiceBusConnection"].ConnectionString;

            JobHostConfiguration config = new JobHostConfiguration();
            config.StorageConnectionString = _storageConn;
            config.DashboardConnectionString = _dashboardConn;
            config.ServiceBusConnectionString = _serviceBusConn;
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="configure-queuetrigger--settings"></a><span data-ttu-id="0a0f3-260">Konfigurace nastavení QueueTrigger</span><span class="sxs-lookup"><span data-stu-id="0a0f3-260">Configure QueueTrigger  settings</span></span>
<span data-ttu-id="0a0f3-261">Můžete nakonfigurovat následující nastavení, které platí pro zpracování zprávy fronty:</span><span class="sxs-lookup"><span data-stu-id="0a0f3-261">You can configure the following settings that apply to the queue message processing:</span></span>

* <span data-ttu-id="0a0f3-262">Maximální počet zpráv fronty, které jsou zachyceny současně spouštění paralelní (výchozí hodnota je 16).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-262">The maximum number of queue messages that are picked up simultaneously to be executed in parallel (default is 16).</span></span>
* <span data-ttu-id="0a0f3-263">Maximální počet opakovaných pokusů, než odešle zprávu fronty poškozených fronty (výchozí hodnota je 5).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-263">The maximum number of retries before a queue message is sent to a poison queue (default is 5).</span></span>
* <span data-ttu-id="0a0f3-264">Maximální čekací dobu před znovu dotazování, když se fronta je prázdný (výchozí hodnota je 1 minuta).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-264">The maximum wait time before polling again when a queue is empty (default is 1 minute).</span></span>

<span data-ttu-id="0a0f3-265">Následující příklad ukazuje, jak nakonfigurovat tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="0a0f3-265">The following example shows how to configure these settings:</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="set-values-for-webjobs-sdk-constructor-parameters-in-code"></a><span data-ttu-id="0a0f3-266">Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu</span><span class="sxs-lookup"><span data-stu-id="0a0f3-266">Set values for WebJobs SDK constructor parameters in code</span></span>
<span data-ttu-id="0a0f3-267">Někdy chcete zadat název fronty, název objektu blob nebo kontejner, nebo tabulku název v kódu než pevný kódu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-267">Sometimes you want to specify a queue name, a blob name or container, or a table name in code rather than hard-code it.</span></span> <span data-ttu-id="0a0f3-268">Například můžete chtít zadat název fronty pro **QueueTrigger** do proměnné prostředí nebo soubor konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-268">For example, you might want to specify the queue name for **QueueTrigger** in a configuration file or environment variable.</span></span>

<span data-ttu-id="0a0f3-269">Můžete to udělat pomocí předávání **NameResolver** do objektu **JobHostConfiguration** typu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-269">You can do that by passing in a **NameResolver** object to the **JobHostConfiguration** type.</span></span> <span data-ttu-id="0a0f3-270">Zahrnout speciální zástupné symboly obklopená znaky procenta (%) v parametrech konstruktoru atributu WebJobs SDK a **NameResolver** kódu určuje skutečné hodnoty má být použit místo tyto zástupné symboly.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-270">You include special placeholders surrounded by percent (%) signs in WebJobs SDK attribute constructor parameters, and your **NameResolver** code specifies the actual values to be used in place of those placeholders.</span></span>

<span data-ttu-id="0a0f3-271">Předpokládejme například, že chcete použít frontu s názvem logqueuetest v testovacím prostředí a jednu s názvem logqueueprod v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-271">For example, suppose you want to use a queue named logqueuetest in the test environment and one named logqueueprod in production.</span></span> <span data-ttu-id="0a0f3-272">Místo názvu pevně fronty, které chcete určit název záznamu v **appSettings** kolekce, která by měla mít název skutečné fronty.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-272">Instead of a hard-coded queue name, you want to specify the name of an entry in the **appSettings** collection that would have the actual queue name.</span></span> <span data-ttu-id="0a0f3-273">Pokud **appSettings** klíč je logqueue, funkce může vypadat podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-273">If the **appSettings** key is logqueue, your function could look like the following example.</span></span>

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

<span data-ttu-id="0a0f3-274">Vaše **NameResolver** třídy, může získat název fronty z **appSettings** jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="0a0f3-274">Your **NameResolver** class could then get the queue name from **appSettings** as shown in the following example:</span></span>

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

<span data-ttu-id="0a0f3-275">Předáte **NameResolver** třídy v do **JobHost** objektu, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-275">You pass the **NameResolver** class in to the **JobHost** object as shown in the following example.</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

<span data-ttu-id="0a0f3-276">**Poznámka:** Queue, table a názvy objektů blob jsou vyřešeny pokaždé, když je volána funkce, ale překladu názvů kontejner objektů blob pouze při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-276">**Note:** Queue, table, and blob names are resolved each time a function is called, but blob container names are resolved only when the application starts.</span></span> <span data-ttu-id="0a0f3-277">Název kontejneru objektu blob nelze změnit, když úloha běží.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-277">You can't change blob container name while the job is running.</span></span>

## <a name="how-to-trigger-a-function-manually"></a><span data-ttu-id="0a0f3-278">Postup ruční aktivaci funkce</span><span class="sxs-lookup"><span data-stu-id="0a0f3-278">How to trigger a function manually</span></span>
<span data-ttu-id="0a0f3-279">Chcete-li aktivovat funkci ručně, použijte **volání** nebo **CallAsync** metodu **JobHost** objektu a **NoAutomaticTrigger** atribut na funkci, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-279">To trigger a function manually, use the **Call** or **CallAsync** method on the **JobHost** object and the **NoAutomaticTrigger** attribute on the function, as shown in the following example.</span></span>

        public class Program
        {
            static void Main(string[] args)
            {
                JobHost host = new JobHost();
                host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
            }

            [NoAutomaticTrigger]
            public static void CreateQueueMessage(
                TextWriter logger,
                string value,
                [Queue("outputqueue")] out string message)
            {
                message = value;
                logger.WriteLine("Creating queue message: ", message);
            }
        }

## <a name="how-to-write-logs"></a><span data-ttu-id="0a0f3-280">Jak napsat protokoly</span><span class="sxs-lookup"><span data-stu-id="0a0f3-280">How to write logs</span></span>
<span data-ttu-id="0a0f3-281">Řídicí panel zobrazuje protokoly na dvou místech: této webové stránky a stránky pro konkrétní vyvolání webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-281">The Dashboard shows logs in two places: the page for the WebJob, and the page for a particular WebJob invocation.</span></span>

![Protokoly v webová stránka](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![Protokoly stránce volání funkce](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

<span data-ttu-id="0a0f3-284">Výstup konzoly metody, kterou je možné volat ve funkci nebo v **Main()** metoda se zobrazí na stránce řídicího panelu pro vytvářené webové úlohy, není na stránce pro volání konkrétní metody.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-284">Output from Console methods that you call in a function or in the **Main()** method appears in the Dashboard page for the WebJob, not in the page for a particular method invocation.</span></span> <span data-ttu-id="0a0f3-285">Výstup z objekt TextWriter, kterou můžete získat z parametr ve vašem podpis metody se zobrazí na stránce řídicího panelu pro volání metody.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-285">Output from the TextWriter object that you get from a parameter in your method signature appears in the Dashboard page for a method invocation.</span></span>

<span data-ttu-id="0a0f3-286">Výstup konzoly, nelze ho propojit s volání konkrétní metody, protože konzole je jedním podprocesem, spuštěného mnoho funkcí úloha může být ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-286">Console output can't be linked to a particular method invocation because the Console is single-threaded, while many job functions may be running at the same time.</span></span> <span data-ttu-id="0a0f3-287">To je důvod, proč sada SDK poskytuje každé vyvolání funkce s objekt zapisovače svůj vlastní jedinečný protokolu.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-287">That's why the  SDK provides each function invocation with its own unique log writer object.</span></span>

<span data-ttu-id="0a0f3-288">Zápis [protokoly trasování aplikací](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), použijte **Console.Out** (vytvoří protokoly, které jsou označeny jako údaje) a **Console.Error** (vytvoří protokoly, které jsou označeny jako chyba).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-288">To write [application tracing logs](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), use **Console.Out** (creates logs marked as INFO) and **Console.Error** (creates logs marked as ERROR).</span></span> <span data-ttu-id="0a0f3-289">Další možností je použít [trasování nebo TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), který poskytuje podrobné, upozornění, a kritická úrovně kromě údaje a informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-289">An alternative is to use [Trace or TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), which provides Verbose, Warning, and Critical levels in addition to Info and Error.</span></span> <span data-ttu-id="0a0f3-290">Protokoly trasování aplikací se zobrazí v souborech protokolů webové aplikace, tabulky Azure, nebo v závislosti na tom, jak nakonfigurujete vaší webové aplikace Azure objektů BLOB Azure.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-290">Application tracing logs appear in the web app log files, Azure tables, or Azure blobs depending on how you configure your Azure web app.</span></span> <span data-ttu-id="0a0f3-291">Platí všechny výstup konzoly nejnovější protokoly 100 aplikací také se zobrazí na stránce řídicího panelu pro webové úlohy, není na stránce pro volání funkce.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-291">As is true of all Console output, the most recent 100 application logs also appear in the Dashboard page for the WebJob, not the page for a function invocation.</span></span>

<span data-ttu-id="0a0f3-292">Výstup konzoly se zobrazí v řídicím panelu pouze v případě, že je aplikace spuštěna v webové úlohy Azure není v případě, že program spuštěn místně nebo v některých dalších prostředí.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-292">Console output appears in the Dashboard only if the program is running in an Azure WebJob, not if the program is running locally or in some other environment.</span></span>

<span data-ttu-id="0a0f3-293">Protokolování můžete zakázat nastavením řídicí panel připojovací řetězec na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-293">You can disable logging by setting the Dashboard connection string to null.</span></span> <span data-ttu-id="0a0f3-294">Další informace najdete v tématu [jak nastavit možnosti konfigurace](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-294">For more information, see [How to set Configuration Options](#how-to-set-configuration-options).</span></span>

<span data-ttu-id="0a0f3-295">Následující příklad ukazuje několik způsobů, jak zápis protokolů:</span><span class="sxs-lookup"><span data-stu-id="0a0f3-295">The following example shows several ways to write logs:</span></span>

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

<span data-ttu-id="0a0f3-296">V řídicím WebJobs SDK na výstup **TextWriter** objekt zobrazí až když přejdete na stránku pro konkrétní funkce volání a vyberte **přepnutí výstup**:</span><span class="sxs-lookup"><span data-stu-id="0a0f3-296">In the WebJobs SDK Dashboard, the output from the **TextWriter** object shows up when you go to the page for a particular function invocation and select **Toggle Output**:</span></span>

![Vyvolání odkazu](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![Protokoly stránce volání funkce](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

<span data-ttu-id="0a0f3-299">Na řídicím panelu WebJobs SDK nejnovější 100 řádků konzoly výstup zobrazit až když přejdete na stránku pro webové úlohy (není pro volání funkce) a vyberte **přepnutí výstup**.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-299">In the WebJobs SDK Dashboard, the most recent 100 lines of Console output show up when you go to the page for the WebJob (not for the function invocation) and select **Toggle Output**.</span></span>

![Přepnutí výstup](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

<span data-ttu-id="0a0f3-301">V nepřetržitá webová úloha, aplikační protokoly objeví v/data/úlohy/průběžné/*{webjobname}*/job_log.txt v systému souborů webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-301">In a continuous WebJob, application logs show up in /data/jobs/continuous/*{webjobname}*/job_log.txt in the web app file system.</span></span>

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

<span data-ttu-id="0a0f3-302">V Azure blob vzhledu protokoly aplikace takto: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello, world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello, world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello, world!,</span><span class="sxs-lookup"><span data-stu-id="0a0f3-302">In an Azure blob the application logs look like this: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span></span>

<span data-ttu-id="0a0f3-303">A v tabulce Azure **Console.Out** a **Console.Error** protokoly vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="0a0f3-303">And in an Azure table the **Console.Out** and **Console.Error** logs look like this:</span></span>

![Informace o protokolu v tabulce](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![Protokol chyb v tabulce](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

## <a name="next-steps"></a><span data-ttu-id="0a0f3-306">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0a0f3-306">Next steps</span></span>
<span data-ttu-id="0a0f3-307">Tento článek poskytl ukázek kódu, které ukazují, jak zpracovat běžné scénáře pro práci s Azure fronty.</span><span class="sxs-lookup"><span data-stu-id="0a0f3-307">This article has provided code samples that show how to handle common scenarios for working with Azure queues.</span></span> <span data-ttu-id="0a0f3-308">Další informace o tom, jak používat Azure WebJobs a WebJobs SDK najdete v tématu [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="0a0f3-308">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

