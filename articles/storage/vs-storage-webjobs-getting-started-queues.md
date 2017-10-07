---
title: "aaaGetting začít s fronty úložiště a Visual Studio připojených služeb (webové úlohy projekty) | Microsoft Docs"
description: "Jak tooget spustit po připojení tooa účet úložiště pomocí sady Visual Studio připojené služby pomocí Azure Queue storage v projektu webové úlohy."
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
ms.openlocfilehash: 720e96fda734c31e1b1d68d4f95aa9531a20a3f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-webjob-projects"></a><span data-ttu-id="b5ef5-103">Začínáme s Azure Queue storage a Visual Studio připojené služeb (webové úlohy projekty)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-103">Getting started with Azure Queue storage and Visual Studio connected services (WebJob Projects)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="b5ef5-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b5ef5-104">Overview</span></span>
<span data-ttu-id="b5ef5-105">Tento článek popisuje, jak získat spuštění pomocí Azure Queue storage v projektu Visual Studio Azure Webjobs po vytvoření a odkazuje pomocí účtu úložiště Azure hello Visual Studio **přidat připojení služby** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-105">This article describes how get started using Azure Queue storage in a Visual Studio Azure WebJob project after you have created or referenced an Azure storage account by using hello Visual Studio  **Add Connected Services** dialog box.</span></span> <span data-ttu-id="b5ef5-106">Když přidáte projektu úlohy WebJob tooa účet úložiště pomocí hello Visual Studio **přidat připojení služby** dialogové okno, jsou nainstalovány hello odpovídající balíčky NuGet úložiště Azure, jsou přidané toohello hello příslušné odkazy na rozhraní .NET projekt a připojovací řetězce pro účet úložiště hello se aktualizují v souboru App.config hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-106">When you add a storage account tooa WebJob project by using hello Visual Studio **Add Connected Services** dialog, hello appropriate Azure Storage NuGet packages are installed, hello appropriate .NET references are added toohello project, and connection strings for hello storage account are updated in hello App.config file.</span></span>  

<span data-ttu-id="b5ef5-107">Tento článek obsahuje C# ukázek kódu, které ukazují, jak toouse hello Azure WebJobs SDK verze 1.x s hello služby Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-107">This article provides C# code samples that show how toouse hello Azure WebJobs SDK version 1.x with hello Azure Queue storage service.</span></span>

<span data-ttu-id="b5ef5-108">Azure Queue storage je služba pro ukládání velkého počtu zpráv, které lze přistupovat z libovolné místo v hello, world prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-108">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="b5ef5-109">Zpráva s jednou frontou může být až velikost too64 KB a jedna fronta můžete obsahovat miliony zpráv, až toohello limit celkové kapacity účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-109">A single queue message can be up too64 KB in size, and a queue can contain millions of messages, up toohello total capacity limit of a storage account.</span></span> <span data-ttu-id="b5ef5-110">V tématu [Začínáme s Azure Queue Storage pomocí rozhraní .NET](storage-dotnet-how-to-use-queues.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-110">See [Get started with Azure Queue Storage using .NET](storage-dotnet-how-to-use-queues.md) for more information.</span></span> <span data-ttu-id="b5ef5-111">Další informace o technologii ASP.NET najdete v tématu [ASP.NET](http://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-111">For more information about ASP.NET, see [ASP.NET](http://www.asp.net).</span></span>

## <a name="how-tootrigger-a-function-when-a-queue-message-is-received"></a><span data-ttu-id="b5ef5-112">Jak tootrigger funkce při příjmu zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="b5ef5-112">How tootrigger a function when a queue message is received</span></span>
<span data-ttu-id="b5ef5-113">volá funkci, která hello WebJobs SDK toowrite při příjmu zprávy fronty, použijte hello **QueueTrigger** atribut.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-113">toowrite a function that hello WebJobs SDK calls when a queue message is received, use hello **QueueTrigger** attribute.</span></span> <span data-ttu-id="b5ef5-114">konstruktoru atributu Hello přijímá řetězcový parametr, který určuje název hello toopoll fronty hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-114">hello attribute constructor takes a string parameter that specifies hello name of hello queue toopoll.</span></span> <span data-ttu-id="b5ef5-115">toosee jak tooset hello název fronty dynamicky, podívejte se na [jak tooset možnosti konfigurace](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-115">toosee how tooset hello queue name dynamically, check out [How tooset Configuration Options](#how-to-set-configuration-options).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="b5ef5-116">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="b5ef5-116">String queue messages</span></span>
<span data-ttu-id="b5ef5-117">V následujícím příkladu hello, fronty hello obsahuje řetězec zprávu, takže **QueueTrigger** je použité tooa řetězec parametr s názvem **logMessage** obsahující hello obsah zprávy fronty hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-117">In hello following example, hello queue contains a string message, so **QueueTrigger** is applied tooa string parameter named **logMessage** which contains hello content of hello queue message.</span></span> <span data-ttu-id="b5ef5-118">Hello funkce [zapíše protokolu zpráv toohello řídicí panel](#how-to-write-logs).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-118">hello function [writes a log message toohello Dashboard](#how-to-write-logs).</span></span>

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

<span data-ttu-id="b5ef5-119">Kromě **řetězec**, hello parametr může být bajtové pole, **CloudQueueMessage** objekt nebo objektů POCO, které definujete.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-119">Besides **string**, hello parameter may be a byte array, a **CloudQueueMessage** object, or a POCO  that you define.</span></span>

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="b5ef5-120">Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="b5ef5-120">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="b5ef5-121">V následujícím příkladu hello, obsahuje zprávu fronty hello JSON pro **BlobInformation** objekt, který zahrnuje **BlobName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-121">In hello following example, hello queue message contains JSON for a **BlobInformation** object which includes a **BlobName** property.</span></span> <span data-ttu-id="b5ef5-122">Hello SDK automaticky deserializuje objekt hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-122">hello SDK automatically deserializes hello object.</span></span>

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

<span data-ttu-id="b5ef5-123">Hello SDK používá hello [balíček Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize a deserializaci zprávy.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-123">hello SDK uses hello [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize and deserialize messages.</span></span> <span data-ttu-id="b5ef5-124">Pokud vytvoříte fronty zpráv v aplikaci, která nepoužívá hello WebJobs SDK, můžete napsat kód jako následující příklad toocreate zprávu fronty objektů POCO hello této hello, které mohou analyzovat SDK.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-124">If you create queue messages in a program that doesn't use hello WebJobs SDK, you can write code like hello following example toocreate a POCO queue message that hello SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a><span data-ttu-id="b5ef5-125">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="b5ef5-125">Async functions</span></span>
<span data-ttu-id="b5ef5-126">Následující funkce asynchronní Hello [zapíše protokolu toohello řídicí panel](#how-to-write-logs).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-126">hello following async function [writes a log toohello Dashboard](#how-to-write-logs).</span></span>

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

<span data-ttu-id="b5ef5-127">Asynchronní funkce může trvat [token zrušení](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), jak ukazuje následující příklad, který kopíruje objekt blob hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-127">Async functions may take a [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), as shown in hello following example which copies a blob.</span></span> <span data-ttu-id="b5ef5-128">(Další informace o hello **queueTrigger** zástupného symbolu, najdete v části hello [objekty BLOB](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) části.)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-128">(For an explanation of hello **queueTrigger** placeholder, see hello [Blobs](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) section.)</span></span>

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

## <a name="types-hello-queuetrigger-attribute-works-with"></a><span data-ttu-id="b5ef5-129">Typy hello QueueTrigger atribut pracuje s</span><span class="sxs-lookup"><span data-stu-id="b5ef5-129">Types hello QueueTrigger attribute works with</span></span>
<span data-ttu-id="b5ef5-130">Můžete použít **QueueTrigger** s hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="b5ef5-130">You can use **QueueTrigger** with hello following types:</span></span>

* <span data-ttu-id="b5ef5-131">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="b5ef5-131">**string**</span></span>
* <span data-ttu-id="b5ef5-132">Typ objektů POCO serializovanou jako JSON</span><span class="sxs-lookup"><span data-stu-id="b5ef5-132">A POCO type serialized as JSON</span></span>
* <span data-ttu-id="b5ef5-133">**Byte]**</span><span class="sxs-lookup"><span data-stu-id="b5ef5-133">**byte[]**</span></span>
* <span data-ttu-id="b5ef5-134">**CloudQueueMessage**</span><span class="sxs-lookup"><span data-stu-id="b5ef5-134">**CloudQueueMessage**</span></span>

## <a name="polling-algorithm"></a><span data-ttu-id="b5ef5-135">Algoritmus dotazování</span><span class="sxs-lookup"><span data-stu-id="b5ef5-135">Polling algorithm</span></span>
<span data-ttu-id="b5ef5-136">Hello SDK implementuje efekt náhodných exponenciální back vypnout algoritmus tooreduce hello nečinnosti-fronty dotazování na transakce náklady na úložiště.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-136">hello SDK implements a random exponential back-off algorithm tooreduce hello effect of idle-queue polling on storage transaction costs.</span></span>  <span data-ttu-id="b5ef5-137">Když se najde zprávu, hello SDK vyčká dvou sekund a poté zkontroluje další zprávu; Pokud je nalezena žádná zpráva čeká před dalším pokusem o 4 sekundy.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-137">When a message is found, hello SDK waits two seconds and then checks for another message; when no message is found it waits about four seconds before trying again.</span></span> <span data-ttu-id="b5ef5-138">Po následné neúspěšných pokusů tooget zprávu fronty, doba čekání hello pokračovat tooincrease, dokud nebude dosaženo hello maximální doba čekání, které minutu tooone výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-138">After subsequent failed attempts tooget a queue message, hello wait time continues tooincrease until it reaches hello maximum wait time, which defaults tooone minute.</span></span> <span data-ttu-id="b5ef5-139">[Hello maximální doba čekání je možné konfigurovat](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-139">[hello maximum wait time is configurable](#how-to-set-configuration-options).</span></span>

## <a name="multiple-instances"></a><span data-ttu-id="b5ef5-140">Více instancí</span><span class="sxs-lookup"><span data-stu-id="b5ef5-140">Multiple instances</span></span>
<span data-ttu-id="b5ef5-141">Pokud vaše webová aplikace běží na několik instancí, nepřetržité webové úlohy se spustí na každém počítači, a každý počítač bude čekat na aktivační události a toorun funkce.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-141">If your web app runs on multiple instances, a continuous WebJobs runs on each machine, and each machine will wait for triggers and attempt toorun functions.</span></span> <span data-ttu-id="b5ef5-142">V některých případech to může vést toosome funkce zpracování hello dvakrát stejná data, takže funkce by mělo být idempotent (zapsání, který volání opakovaně s hello stejnými vstupními daty neobsahuje duplicitní výsledky).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-142">In some scenarios this can lead toosome functions processing hello same data twice, so functions should be idempotent (written so that calling them repeatedly with hello same input data doesn't produce duplicate results).</span></span>  

## <a name="parallel-execution"></a><span data-ttu-id="b5ef5-143">Paralelní provádění</span><span class="sxs-lookup"><span data-stu-id="b5ef5-143">Parallel execution</span></span>
<span data-ttu-id="b5ef5-144">Pokud máte víc funkcí naslouchá na různých front, hello SDK je zavolá paralelně přijetí zpráv současně.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-144">If you have multiple functions listening on different queues, hello SDK will call them in parallel when messages are received simultaneously.</span></span>

<span data-ttu-id="b5ef5-145">Hello totéž platí při přijetí více zpráv pro jednu frontu.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-145">hello same is true when multiple messages are received for a single queue.</span></span> <span data-ttu-id="b5ef5-146">Ve výchozím nastavení hello SDK získá dávku 16 fronty zpráv v čase a provede hello funkce, která je zpracuje paralelně.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-146">By default, hello SDK gets a batch of 16 queue messages at a time and executes hello function that processes them in parallel.</span></span> <span data-ttu-id="b5ef5-147">[velikost dávky Hello je možné konfigurovat](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-147">[hello batch size is configurable](#how-to-set-configuration-options).</span></span> <span data-ttu-id="b5ef5-148">Když hello počet zpracovávaných získá dolů toohalf hello velikost dávky, hello SDK získá další dávku a spustí zpracování těchto zpráv.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-148">When hello number being processed gets down toohalf of hello batch size, hello SDK gets another batch and starts processing those messages.</span></span> <span data-ttu-id="b5ef5-149">Maximální počet souběžných zpráv, které jsou zpracovány za funkce hello je proto velikost dávky hello jeden a půl krát.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-149">Therefore hello maximum number of concurrent messages being processed per function is one and a half times hello batch size.</span></span> <span data-ttu-id="b5ef5-150">Toto omezení se vztahuje samostatně tooeach funkce, který má **QueueTrigger** atribut.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-150">This limit applies separately tooeach function that has a **QueueTrigger** attribute.</span></span> <span data-ttu-id="b5ef5-151">Pokud nechcete, aby paralelní zpracování zprávy přijaté v jedné frontě, nastavte too1 velikost dávky hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-151">If you don't want parallel execution for messages received on one queue, set hello batch size too1.</span></span>

## <a name="get-queue-or-queue-message-metadata"></a><span data-ttu-id="b5ef5-152">Získání fronty nebo fronty zpráv metadat</span><span class="sxs-lookup"><span data-stu-id="b5ef5-152">Get queue or queue message metadata</span></span>
<span data-ttu-id="b5ef5-153">Můžete získat hello následující vlastnosti zprávy přidáním podpis metody toohello parametry:</span><span class="sxs-lookup"><span data-stu-id="b5ef5-153">You can get hello following message properties by adding parameters toohello method signature:</span></span>

* <span data-ttu-id="b5ef5-154">**Datový typ DateTimeOffset** expirationTime</span><span class="sxs-lookup"><span data-stu-id="b5ef5-154">**DateTimeOffset** expirationTime</span></span>
* <span data-ttu-id="b5ef5-155">**Datový typ DateTimeOffset** insertionTime</span><span class="sxs-lookup"><span data-stu-id="b5ef5-155">**DateTimeOffset** insertionTime</span></span>
* <span data-ttu-id="b5ef5-156">**Datový typ DateTimeOffset** nextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="b5ef5-156">**DateTimeOffset** nextVisibleTime</span></span>
* <span data-ttu-id="b5ef5-157">**řetězec** queueTrigger (obsahuje text zprávy)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-157">**string** queueTrigger (contains message text)</span></span>
* <span data-ttu-id="b5ef5-158">**řetězec** id</span><span class="sxs-lookup"><span data-stu-id="b5ef5-158">**string** id</span></span>
* <span data-ttu-id="b5ef5-159">**řetězec** vlastností popReceipt</span><span class="sxs-lookup"><span data-stu-id="b5ef5-159">**string** popReceipt</span></span>
* <span data-ttu-id="b5ef5-160">**int** dequeueCount</span><span class="sxs-lookup"><span data-stu-id="b5ef5-160">**int** dequeueCount</span></span>

<span data-ttu-id="b5ef5-161">Pokud chcete toowork přímo s hello úložiště Azure API, můžete také přidat **CloudStorageAccount** parametr.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-161">If you want toowork directly with hello Azure storage API, you can also add a **CloudStorageAccount** parameter.</span></span>

<span data-ttu-id="b5ef5-162">Hello následující příklad zapíše všechny tento protokol metadata tooan informace o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-162">hello following example writes all of this metadata tooan INFO application log.</span></span> <span data-ttu-id="b5ef5-163">V příkladu hello logMessage a queueTrigger obsahovat hello obsah zprávy fronty hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-163">In hello example, both logMessage and queueTrigger contain hello content of hello queue message.</span></span>

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

<span data-ttu-id="b5ef5-164">Tady je příklad protokolu o zapsaných správcem hello ukázkový kód:</span><span class="sxs-lookup"><span data-stu-id="b5ef5-164">Here is a sample log written by hello sample code:</span></span>

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

## <a name="graceful-shutdown"></a><span data-ttu-id="b5ef5-165">Řádné vypnutí</span><span class="sxs-lookup"><span data-stu-id="b5ef5-165">Graceful shutdown</span></span>
<span data-ttu-id="b5ef5-166">Funkci, která běží v nepřetržitá webová úloha může přijmout **CancellationToken** parametr, který umožňuje hello operačního systému toonotify hello funkce při hello webové úlohy je o toobe byla ukončena.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-166">A function that runs in a continuous WebJob can accept a **CancellationToken** parameter which enables hello operating system toonotify hello function when hello WebJob is about toobe terminated.</span></span> <span data-ttu-id="b5ef5-167">Můžete vytvořit toto oznámení toomake se, že funkce hello není neočekávaně ukončí tak, aby data ponechá v nekonzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-167">You can use this notification toomake sure hello function doesn't terminate unexpectedly in a way that leaves data in an inconsistent state.</span></span>

<span data-ttu-id="b5ef5-168">Následující příklad ukazuje, jak Hello toocheck pro předcházení ukončení webové úlohy ve funkci.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-168">hello following example shows how toocheck for impending WebJob termination in a function.</span></span>

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

<span data-ttu-id="b5ef5-169">**Poznámka:** hello řídicí panel nemusí zobrazit správně hello stav a výstup funkcí, které byly vypnuty.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-169">**Note:** hello Dashboard might not correctly show hello status and output of functions that have been shut down.</span></span>

<span data-ttu-id="b5ef5-170">Další informace najdete v tématu [WebJobs řádné vypnutí](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-170">For more information, see [WebJobs Graceful Shutdown](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span></span>   

## <a name="how-toocreate-a-queue-message-while-processing-a-queue-message"></a><span data-ttu-id="b5ef5-171">Jak toocreate fronty zpráv při zpracování zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="b5ef5-171">How toocreate a queue message while processing a queue message</span></span>
<span data-ttu-id="b5ef5-172">toowrite funkci, která vytvoří novou zprávu fronty, použijte hello **fronty** atribut.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-172">toowrite a function that creates a new queue message, use hello **Queue** attribute.</span></span> <span data-ttu-id="b5ef5-173">Jako **QueueTrigger**, kterou předáte název fronty hello jako řetězec, nebo můžete [nastavit název fronty hello dynamicky](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-173">Like **QueueTrigger**, you pass in hello queue name as a string or you can [set hello queue name dynamically](#how-to-set-configuration-options).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="b5ef5-174">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="b5ef5-174">String queue messages</span></span>
<span data-ttu-id="b5ef5-175">Následující ukázka kódu bez asynchronní Hello vytvoří novou zprávu fronty ve frontě hello s názvem "outputqueue" se stejný obsah jako zprávu fronty hello dostali hello frontu s názvem "inputqueue" hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-175">hello following non-async code sample creates a new queue message in hello queue named "outputqueue" with hello same content as hello queue message received in hello queue named "inputqueue".</span></span> <span data-ttu-id="b5ef5-176">(Pro asynchronní použít funkce **IAsyncCollector<T>**  znázorněné později v této části.)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-176">(For async functions use **IAsyncCollector<T>** as shown later in this section.)</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="b5ef5-177">Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="b5ef5-177">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="b5ef5-178">toocreate zprávu fronty, která obsahuje objektů POCO spíše než řetězec průchodu hello objektů POCO, zadejte jako parametr toohello výstupu **fronty** konstruktoru atributu.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-178">toocreate a queue message that contains a POCO rather than a string, pass hello POCO type as an output parameter toohello **Queue** attribute constructor.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

<span data-ttu-id="b5ef5-179">Hello SDK automaticky serializuje tooJSON objekt hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-179">hello SDK automatically serializes hello object tooJSON.</span></span> <span data-ttu-id="b5ef5-180">Zprávu fronty je vytvořena vždy, i když hello objekt má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-180">A queue message is always created, even if hello object is null.</span></span>

### <a name="create-multiple-messages-or-in-async-functions"></a><span data-ttu-id="b5ef5-181">Vytvoření více zpráv nebo asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="b5ef5-181">Create multiple messages or in async functions</span></span>
<span data-ttu-id="b5ef5-182">toocreate více zpráv, zkontrolujte typ parametru hello pro hello výstupní fronty **ICollector<T>**  nebo **IAsyncCollector<T>**, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-182">toocreate multiple messages, make hello parameter type for hello output queue **ICollector<T>** or **IAsyncCollector<T>**, as shown in hello following example.</span></span>

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="b5ef5-183">Každou zprávu fronty se vytvoří okamžitě při hello **přidat** metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-183">Each queue message is created immediately when hello **Add** method is called.</span></span>

### <a name="types-that-hello-queue-attribute-works-with"></a><span data-ttu-id="b5ef5-184">Typy tento atribut fronty hello funguje s</span><span class="sxs-lookup"><span data-stu-id="b5ef5-184">Types that hello Queue attribute works with</span></span>
<span data-ttu-id="b5ef5-185">Můžete použít hello **fronty** atribut hello následující typy parametrů:</span><span class="sxs-lookup"><span data-stu-id="b5ef5-185">You can use hello **Queue** attribute on hello following parameter types:</span></span>

* <span data-ttu-id="b5ef5-186">**na řetězce** (vytvoří zprávu fronty, pokud je hodnota parametru jinou hodnotu než null při ukončení hello funkce)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-186">**out string** (creates queue message if parameter value is non-null when hello function ends)</span></span>
* <span data-ttu-id="b5ef5-187">**out byte []** (funguje jako **řetězec**)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-187">**out byte[]** (works like **string**)</span></span>
* <span data-ttu-id="b5ef5-188">**out CloudQueueMessage** (funguje jako **řetězec**)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-188">**out CloudQueueMessage** (works like **string**)</span></span>
* <span data-ttu-id="b5ef5-189">**out objektů POCO** (typu serializable vytvoří zprávu s objektem hodnotu null. Pokud hello parametru má hodnotu null při ukončení hello funkce)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-189">**out POCO** (a serializable type, creates a message with a null object if hello paramter is null when hello function ends)</span></span>
* <span data-ttu-id="b5ef5-190">**ICollector**</span><span class="sxs-lookup"><span data-stu-id="b5ef5-190">**ICollector**</span></span>
* <span data-ttu-id="b5ef5-191">**IAsyncCollector**</span><span class="sxs-lookup"><span data-stu-id="b5ef5-191">**IAsyncCollector**</span></span>
* <span data-ttu-id="b5ef5-192">**CloudQueue** (pro vytváření zpráv ručně pomocí hello API pro Azure Storage přímo)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-192">**CloudQueue** (for creating messages manually using hello Azure Storage API directly)</span></span>

### <a name="use-webjobs-sdk-attributes-in-hello-body-of-a-function"></a><span data-ttu-id="b5ef5-193">Použití atributů WebJobs SDK v hello tělo funkce</span><span class="sxs-lookup"><span data-stu-id="b5ef5-193">Use WebJobs SDK attributes in hello body of a function</span></span>
<span data-ttu-id="b5ef5-194">Pokud potřebujete toodo některé fungovat ve vaší funkci před použitím atribut WebJobs SDK, jako **fronty**, **Blob**, nebo **tabulky**, můžete použít hello **IBinder** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-194">If you need toodo some work in your function before using a WebJobs SDK attribute such as **Queue**, **Blob**, or **Table**, you can use hello **IBinder** interface.</span></span>

<span data-ttu-id="b5ef5-195">Následující ukázka Hello přebírá zprávu vstupní fronty a vytvoří novou zprávu s hello stejný obsah v výstupní fronty.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-195">hello following example takes an input queue message and creates a new message with hello same content in an output queue.</span></span> <span data-ttu-id="b5ef5-196">Název fronty výstup Hello je nastavena podle kódu v textu hello hello funkce.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-196">hello output queue name is set by code in hello body of hello function.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

<span data-ttu-id="b5ef5-197">Hello **IBinder** rozhraní lze také s hello **tabulky** a **Blob** atributy.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-197">hello **IBinder** interface can also be used with hello **Table** and **Blob** attributes.</span></span>

## <a name="how-tooread-and-write-blobs-and-tables-while-processing-a-queue-message"></a><span data-ttu-id="b5ef5-198">Jak objekty BLOB tooread a zápis a tabulky při zpracování zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="b5ef5-198">How tooread and write blobs and tables while processing a queue message</span></span>
<span data-ttu-id="b5ef5-199">Hello **Blob** a **tabulky** atributů umožňují tooread a zapisovat objekty BLOB a tabulek.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-199">hello **Blob** and **Table** attributes enable you tooread and write blobs and tables.</span></span> <span data-ttu-id="b5ef5-200">Ukázky Hello v této části platí tooblobs.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-200">hello samples in this section apply tooblobs.</span></span> <span data-ttu-id="b5ef5-201">Ukázky kódu, které ukazují, jak tootrigger zpracovává, když se objekty BLOB jsou vytvořeny nebo aktualizovány, najdete v části [jak toouse Azure blob storage s hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)a ukázky kódu, které pro čtení a zápis tabulky, najdete v části [jak toouse tabulky Azure úložiště s hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-201">For code samples that show how tootrigger processes when blobs are created or updated, see [How toouse Azure blob storage with hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), and for code samples that read and write tables, see [How toouse Azure table storage with hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span></span>

### <a name="string-queue-messages-triggering-blob-operations"></a><span data-ttu-id="b5ef5-202">Zprávy fronty řetězec spuštění operace objektů blob</span><span class="sxs-lookup"><span data-stu-id="b5ef5-202">String queue messages triggering blob operations</span></span>
<span data-ttu-id="b5ef5-203">Pro zprávu fronty, který obsahuje řetězec **queueTrigger** je zástupný symbol můžete použít v hello **Blob** atributu **blobPath** parametr, který obsahuje obsah hello uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-203">For a queue message that contains a string, **queueTrigger** is a placeholder you can use in hello **Blob** attribute's **blobPath** parameter that contains hello contents of hello message.</span></span>

<span data-ttu-id="b5ef5-204">Hello následující příklad používá **datového proudu** objekty tooread a zápisu objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-204">hello following example uses **Stream** objects tooread and write blobs.</span></span> <span data-ttu-id="b5ef5-205">uvítací zprávu fronty je hello název objektu blob umístěna v kontejneru textblobs hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-205">hello queue message is hello name of a blob located in hello textblobs container.</span></span> <span data-ttu-id="b5ef5-206">Kopie objektu blob hello s "-nové" připojením toohello jméno se vytvoří v hello stejné kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-206">A copy of hello blob with "-new" appended toohello name is created in hello same container.</span></span>

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="b5ef5-207">Hello **Blob** atributu konstruktoru trvá **blobPath** parametr, který určuje hello kontejneru a název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-207">hello **Blob** attribute constructor takes a **blobPath** parameter that specifies hello container and blob name.</span></span> <span data-ttu-id="b5ef5-208">Další informace o tento zástupný text najdete v tématu [jak toouse Azure blob storage s hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-208">For more information about this placeholder, see [How toouse Azure blob storage with hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md).</span></span>

<span data-ttu-id="b5ef5-209">Pokud atribut hello upraví **datového proudu** objektu jiný parametr konstruktoru určuje hello **FileAccess** režimu pro čtení, zápisu nebo čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-209">When hello attribute decorates a **Stream** object, another constructor parameter specifies hello **FileAccess** mode as read, write, or read/write.</span></span>

<span data-ttu-id="b5ef5-210">Hello následující příklad používá **CloudBlockBlob** objektu toodelete objekt blob.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-210">hello following example uses a **CloudBlockBlob** object toodelete a blob.</span></span> <span data-ttu-id="b5ef5-211">uvítací zprávu fronty je hello název objektu hello blob.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-211">hello queue message is hello name of hello blob.</span></span>

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="b5ef5-212">Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="b5ef5-212">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="b5ef5-213">Pro objektů POCO uloží jako dokumenty JSON v uvítací zprávu fronty, můžete použít zástupné znaky, které název vlastnosti objektu hello v hello **fronty** atributu **blobPath** parametr.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-213">For a POCO stored as JSON in hello queue message, you can use placeholders that name properties of hello object in hello **Queue** attribute's **blobPath** parameter.</span></span> <span data-ttu-id="b5ef5-214">Můžete taky názvy vlastností fronty metadata zástupnými symboly.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-214">You can also use queue metadata property names as placeholders.</span></span> <span data-ttu-id="b5ef5-215">V tématu [získat fronty nebo fronty zpráv metadata](#get-queue-or-queue-message-metadata).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-215">See [Get queue or queue message metadata](#get-queue-or-queue-message-metadata).</span></span>

<span data-ttu-id="b5ef5-216">Hello následujícím příkladu se zkopíruje objekt blob tooa nové blob se jiné rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-216">hello following example copies a blob tooa new blob with a different extension.</span></span> <span data-ttu-id="b5ef5-217">zpráva fronty Hello **BlobInformation** objekt, který zahrnuje **BlobName** a **BlobNameWithoutExtension** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-217">hello queue message is a **BlobInformation** object that includes **BlobName** and **BlobNameWithoutExtension** properties.</span></span> <span data-ttu-id="b5ef5-218">názvy vlastností Hello jsou použity jako zástupné symboly v cestě objektu blob hello pro hello **Blob** atributy.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-218">hello property names are used as placeholders in hello blob path for hello **Blob** attributes.</span></span>

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="b5ef5-219">Hello SDK používá hello [balíček Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize a deserializaci zprávy.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-219">hello SDK uses hello [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize and deserialize messages.</span></span> <span data-ttu-id="b5ef5-220">Pokud vytvoříte fronty zpráv v aplikaci, která nepoužívá hello WebJobs SDK, můžete napsat kód jako následující příklad toocreate zprávu fronty objektů POCO hello této hello, které mohou analyzovat SDK.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-220">If you create queue messages in a program that doesn't use hello WebJobs SDK, you can write code like hello following example toocreate a POCO queue message that hello SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

<span data-ttu-id="b5ef5-221">Pokud potřebujete toodo některé fungovat ve vaší funkci před vazby tooan objekt blob, můžete použít atribut hello v textu hello hello funkce, jak je znázorněno v [pomocí WebJobs SDK atributy v textu hello funkce](#use-webjobs-sdk-attributes-in-the-body-of-a-function).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-221">If you need toodo some work in your function before binding a blob tooan object, you can use hello attribute in hello body of hello function, as shown in [Use WebJobs SDK attributes in hello body of a function](#use-webjobs-sdk-attributes-in-the-body-of-a-function).</span></span>

### <a name="types-you-can-use-hello-blob-attribute-with"></a><span data-ttu-id="b5ef5-222">Můžete použít hello typy objektů Blob atribut s</span><span class="sxs-lookup"><span data-stu-id="b5ef5-222">Types you can use hello Blob attribute with</span></span>
<span data-ttu-id="b5ef5-223">Hello **Blob** atribut lze použít s hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="b5ef5-223">hello **Blob** attribute can be used with hello following types:</span></span>

* <span data-ttu-id="b5ef5-224">**Datový proud** (pro čtení nebo zápisu, zadat pomocí parametru konstruktoru FileAccess hello)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-224">**Stream** (read or write, specified by using hello FileAccess constructor parameter)</span></span>
* <span data-ttu-id="b5ef5-225">**TextReader**</span><span class="sxs-lookup"><span data-stu-id="b5ef5-225">**TextReader**</span></span>
* <span data-ttu-id="b5ef5-226">**TextWriter**</span><span class="sxs-lookup"><span data-stu-id="b5ef5-226">**TextWriter**</span></span>
* <span data-ttu-id="b5ef5-227">**řetězec** (přečíst)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-227">**string** (read)</span></span>
* <span data-ttu-id="b5ef5-228">**na řetězce** (zapisovat; vytvoří objekt blob jenom v případě, že parametr řetězce hello má jinou hodnotu než null při návratu hello funkce)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-228">**out string** (write; creates a blob only if hello string parameter is non-null when hello function returns)</span></span>
* <span data-ttu-id="b5ef5-229">Objektů POCO (čtení)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-229">POCO (read)</span></span>
* <span data-ttu-id="b5ef5-230">out objektů POCO (zápisu; vždycky vytvoří objekt blob, vytvoří jako objektu null, pokud parametr objektů POCO má hodnotu null, když se vrátí hello funkce)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-230">out POCO (write; always creates a blob, creates as null object if POCO parameter is null when hello function returns)</span></span>
* <span data-ttu-id="b5ef5-231">**CloudBlobStream** (zápisu)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-231">**CloudBlobStream** (write)</span></span>
* <span data-ttu-id="b5ef5-232">**ICloudBlob** (čtení nebo zápisu)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-232">**ICloudBlob** (read or write)</span></span>
* <span data-ttu-id="b5ef5-233">**CloudBlockBlob** (čtení nebo zápisu)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-233">**CloudBlockBlob** (read or write)</span></span>
* <span data-ttu-id="b5ef5-234">**CloudPageBlob** (čtení nebo zápisu)</span><span class="sxs-lookup"><span data-stu-id="b5ef5-234">**CloudPageBlob** (read or write)</span></span>

## <a name="how-toohandle-poison-messages"></a><span data-ttu-id="b5ef5-235">Jak toohandle škodlivých zpráv</span><span class="sxs-lookup"><span data-stu-id="b5ef5-235">How toohandle poison messages</span></span>
<span data-ttu-id="b5ef5-236">Zprávy, jejichž obsah způsobí, že funkce toofail se nazývají *škodlivých zpráv*.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-236">Messages whose content causes a function toofail are called *poison messages*.</span></span> <span data-ttu-id="b5ef5-237">Pokud funkce hello nezdaří, hello fronty zpráv není odstraněna a nakonec je převzata znovu, což způsobilo toobe cyklus hello opakuje.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-237">When hello function fails, hello queue message is not deleted and eventually is picked up again, causing hello cycle toobe repeated.</span></span> <span data-ttu-id="b5ef5-238">Hello SDK můžete automaticky přerušení hello cyklus po omezený počet iterací, nebo můžete provést ručně.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-238">hello SDK can automatically interrupt hello cycle after a limited number of iterations, or you can do it manually.</span></span>

### <a name="automatic-poison-message-handling"></a><span data-ttu-id="b5ef5-239">Zpracování automatické poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="b5ef5-239">Automatic poison message handling</span></span>
<span data-ttu-id="b5ef5-240">Hello SDK bude volat funkci až too5 časy tooprocess zprávu fronty.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-240">hello SDK will call a function up too5 times tooprocess a queue message.</span></span> <span data-ttu-id="b5ef5-241">V případě selhání páté zkuste hello je uvítací zprávu přesunutý tooa poškozených fronty.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-241">If hello fifth try fails, hello message is moved tooa poison queue.</span></span> <span data-ttu-id="b5ef5-242">Uvidíte, jak tooconfigure hello maximální počet opakovaných pokusů v [jak možnosti konfigurace tooset](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-242">You can see how tooconfigure hello maximum number of retries in [How tooset configuration options](#how-to-set-configuration-options).</span></span>

<span data-ttu-id="b5ef5-243">Název fronty poškozených Hello *{originalqueuename}*-poškozených.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-243">hello poison queue is named *{originalqueuename}*-poison.</span></span> <span data-ttu-id="b5ef5-244">Můžete napsat tooprocess funkce zprávy z fronty poškozených hello jejich protokolování nebo odeslání oznámení, že je potřeba ruční pozornost.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-244">You can write a function tooprocess messages from hello poison queue by logging them or sending a notification that manual attention is needed.</span></span>

<span data-ttu-id="b5ef5-245">V následujícím příkladu hello hello **CopyBlob** funkce se nezdaří, pokud obsahuje zprávu fronty hello název objektu blob, který neexistuje.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-245">In hello following example hello **CopyBlob** function will fail when a queue message contains hello name of a blob that doesn't exist.</span></span> <span data-ttu-id="b5ef5-246">Pokud k tomu dojde, hello zpráva bude přesunuta z hello copyblobqueue fronty toohello copyblobqueue poison fronty.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-246">When that happens, hello message is moved from hello copyblobqueue queue toohello copyblobqueue-poison queue.</span></span> <span data-ttu-id="b5ef5-247">Hello **ProcessPoisonMessage** pak protokoly hello poškozená zpráva.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-247">hello **ProcessPoisonMessage** then logs hello poison message.</span></span>

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
            logger.WriteLine("Failed toocopy blob, name=" + blobName);
        }

<span data-ttu-id="b5ef5-248">Hello následující obrázek ukazuje výstup konzoly z těchto funkcí při zpracování poškozená zpráva.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-248">hello following illustration shows console output from these functions when a poison message is processed.</span></span>

![Výstup konzoly pro zpracování poškozených zpráv](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### <a name="manual-poison-message-handling"></a><span data-ttu-id="b5ef5-250">Zpracování ručních poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="b5ef5-250">Manual poison message handling</span></span>
<span data-ttu-id="b5ef5-251">Hello počet, kolikrát byla převzata zprávu pro zpracování můžete získat tak, že přidáte **int** parametr s názvem **dequeueCount** tooyour funkce.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-251">You can get hello number of times a message has been picked up for processing by adding an **int** parameter named **dequeueCount** tooyour function.</span></span> <span data-ttu-id="b5ef5-252">Můžete pak zkontrolujte hello dequeue – počet v kódu funkce a provádět vlastní zpracování poškozených zpráv Pokud hello číslo překročí prahovou hodnotu, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-252">You can then check hello dequeue count in function code and perform your own poison message handling when hello number exceeds a threshold, as shown in hello following example.</span></span>

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed toocopy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a name="how-tooset-configuration-options"></a><span data-ttu-id="b5ef5-253">Jak tooset možnosti konfigurace</span><span class="sxs-lookup"><span data-stu-id="b5ef5-253">How tooset configuration options</span></span>
<span data-ttu-id="b5ef5-254">Můžete použít hello **JobHostConfiguration** hello tooset typ následující možnosti konfigurace:</span><span class="sxs-lookup"><span data-stu-id="b5ef5-254">You can use hello **JobHostConfiguration** type tooset hello following configuration options:</span></span>

* <span data-ttu-id="b5ef5-255">Nastavit hello SDK připojovacích řetězců v kódu.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-255">Set hello SDK connection strings in code.</span></span>
* <span data-ttu-id="b5ef5-256">Konfigurace **QueueTrigger** nastavení, jako například maximální počet vyřazení z fronty.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-256">Configure **QueueTrigger** settings such as maximum dequeue count.</span></span>
* <span data-ttu-id="b5ef5-257">Získáte názvy fronty z konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-257">Get queue names from configuration.</span></span>

### <a name="set-sdk-connection-strings-in-code"></a><span data-ttu-id="b5ef5-258">Sada SDK připojovacích řetězců v kódu</span><span class="sxs-lookup"><span data-stu-id="b5ef5-258">Set SDK connection strings in code</span></span>
<span data-ttu-id="b5ef5-259">Nastavení hello SDK připojovacích řetězců v kódu umožňuje vám toouse vlastní názvy řetězec připojení v konfiguračních souborů nebo proměnné prostředí, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-259">Setting hello SDK connection strings in code enables you toouse your own connection string names in configuration files or environment variables, as shown in hello following example.</span></span>

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

### <a name="configure-queuetrigger--settings"></a><span data-ttu-id="b5ef5-260">Konfigurace nastavení QueueTrigger</span><span class="sxs-lookup"><span data-stu-id="b5ef5-260">Configure QueueTrigger  settings</span></span>
<span data-ttu-id="b5ef5-261">Můžete nakonfigurovat následující nastavení, které se vztahují zpracování zprávy fronty toohello hello:</span><span class="sxs-lookup"><span data-stu-id="b5ef5-261">You can configure hello following settings that apply toohello queue message processing:</span></span>

* <span data-ttu-id="b5ef5-262">maximální počet zpráv fronty, které jsou zachyceny současně toobe paralelně Hello (výchozí hodnota je 16).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-262">hello maximum number of queue messages that are picked up simultaneously toobe executed in parallel (default is 16).</span></span>
* <span data-ttu-id="b5ef5-263">Hello maximální počet opakovaných pokusů, než je odeslána zpráva fronty tooa poškozených fronty (výchozí hodnota je 5).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-263">hello maximum number of retries before a queue message is sent tooa poison queue (default is 5).</span></span>
* <span data-ttu-id="b5ef5-264">Hello maximální doba čekání před znovu dotazování, když se fronta je prázdný (výchozí hodnota je 1 minuta).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-264">hello maximum wait time before polling again when a queue is empty (default is 1 minute).</span></span>

<span data-ttu-id="b5ef5-265">Následující příklad ukazuje, jak Hello tooconfigure tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="b5ef5-265">hello following example shows how tooconfigure these settings:</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="set-values-for-webjobs-sdk-constructor-parameters-in-code"></a><span data-ttu-id="b5ef5-266">Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu</span><span class="sxs-lookup"><span data-stu-id="b5ef5-266">Set values for WebJobs SDK constructor parameters in code</span></span>
<span data-ttu-id="b5ef5-267">Občas můžete chtít toospecify název fronty, název objektu blob nebo kontejner, nebo tabulku název v kódu než pevný kódu.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-267">Sometimes you want toospecify a queue name, a blob name or container, or a table name in code rather than hard-code it.</span></span> <span data-ttu-id="b5ef5-268">Například můžete název fronty hello toospecify **QueueTrigger** do proměnné prostředí nebo soubor konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-268">For example, you might want toospecify hello queue name for **QueueTrigger** in a configuration file or environment variable.</span></span>

<span data-ttu-id="b5ef5-269">Můžete to udělat pomocí předávání **NameResolver** objektu toohello **JobHostConfiguration** typu.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-269">You can do that by passing in a **NameResolver** object toohello **JobHostConfiguration** type.</span></span> <span data-ttu-id="b5ef5-270">Zahrnout speciální zástupné symboly obklopená znaky procenta (%) v parametrech konstruktoru atributu WebJobs SDK a **NameResolver** kódu určuje toobe hello skutečné hodnoty, použijí se místo tyto zástupné symboly.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-270">You include special placeholders surrounded by percent (%) signs in WebJobs SDK attribute constructor parameters, and your **NameResolver** code specifies hello actual values toobe used in place of those placeholders.</span></span>

<span data-ttu-id="b5ef5-271">Například předpokládejme, že chcete toouse frontu s názvem logqueuetest v testovacím prostředí hello a jednu s názvem logqueueprod v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-271">For example, suppose you want toouse a queue named logqueuetest in hello test environment and one named logqueueprod in production.</span></span> <span data-ttu-id="b5ef5-272">Místo názvu fronty pevně chcete toospecify hello název záznamu v hello **appSettings** kolekce, která by měla mít název skutečné fronty hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-272">Instead of a hard-coded queue name, you want toospecify hello name of an entry in hello **appSettings** collection that would have hello actual queue name.</span></span> <span data-ttu-id="b5ef5-273">Pokud hello **appSettings** klíč je logqueue, funkce by měl vypadat jako následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-273">If hello **appSettings** key is logqueue, your function could look like hello following example.</span></span>

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

<span data-ttu-id="b5ef5-274">Vaše **NameResolver** třídy, může získat název fronty hello z **appSettings** jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="b5ef5-274">Your **NameResolver** class could then get hello queue name from **appSettings** as shown in hello following example:</span></span>

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

<span data-ttu-id="b5ef5-275">Předat hello **NameResolver** třídy v toohello **JobHost** objektu, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-275">You pass hello **NameResolver** class in toohello **JobHost** object as shown in hello following example.</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

<span data-ttu-id="b5ef5-276">**Poznámka:** Queue, table a názvy objektů blob jsou vyřešeny pokaždé, když je volána funkce, ale jenom v případě, že spuštěním aplikace hello překladu názvů kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-276">**Note:** Queue, table, and blob names are resolved each time a function is called, but blob container names are resolved only when hello application starts.</span></span> <span data-ttu-id="b5ef5-277">Název kontejneru objektu blob nelze změnit, když je spuštěná úloha hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-277">You can't change blob container name while hello job is running.</span></span>

## <a name="how-tootrigger-a-function-manually"></a><span data-ttu-id="b5ef5-278">Jak tootrigger funkce ručně</span><span class="sxs-lookup"><span data-stu-id="b5ef5-278">How tootrigger a function manually</span></span>
<span data-ttu-id="b5ef5-279">tootrigger funkce ručně, použijte hello **volání** nebo **CallAsync** metodu hello **JobHost** objekt a hello **NoAutomaticTrigger** atribut hello funkce, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-279">tootrigger a function manually, use hello **Call** or **CallAsync** method on hello **JobHost** object and hello **NoAutomaticTrigger** attribute on hello function, as shown in hello following example.</span></span>

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

## <a name="how-toowrite-logs"></a><span data-ttu-id="b5ef5-280">Jak toowrite protokoly</span><span class="sxs-lookup"><span data-stu-id="b5ef5-280">How toowrite logs</span></span>
<span data-ttu-id="b5ef5-281">Hello řídicí panel zobrazuje protokoly na dvou místech: hello stránky pro webové úlohy hello a hello stránky pro konkrétní vyvolání webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-281">hello Dashboard shows logs in two places: hello page for hello WebJob, and hello page for a particular WebJob invocation.</span></span>

![Protokoly v webová stránka](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![Protokoly stránce volání funkce](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

<span data-ttu-id="b5ef5-284">Výstup z konzoly metod, které volají ve funkci nebo v hello **Main()** metoda se zobrazí v hello stránka řídicího panelu pro hello webové úlohy, nikoli na stránku hello volání konkrétní metody.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-284">Output from Console methods that you call in a function or in hello **Main()** method appears in hello Dashboard page for hello WebJob, not in hello page for a particular method invocation.</span></span> <span data-ttu-id="b5ef5-285">Výstup hello objekt TextWriter, kterou můžete získat z parametr ve vašem podpis metody se zobrazí v hello stránka řídicího panelu pro volání metody.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-285">Output from hello TextWriter object that you get from a parameter in your method signature appears in hello Dashboard page for a method invocation.</span></span>

<span data-ttu-id="b5ef5-286">Výstup konzoly nemůže být volání konkrétní metody propojené tooa, protože hello konzoly je jedním podprocesem, zatímco mnoho funkcí úlohy může běžet v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-286">Console output can't be linked tooa particular method invocation because hello Console is single-threaded, while many job functions may be running at hello same time.</span></span> <span data-ttu-id="b5ef5-287">To je důvod, proč hello SDK poskytuje každé vyvolání funkce s objekt zapisovače svůj vlastní jedinečný protokolu.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-287">That's why hello  SDK provides each function invocation with its own unique log writer object.</span></span>

<span data-ttu-id="b5ef5-288">toowrite [protokoly trasování aplikací](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), použijte **Console.Out** (vytvoří protokoly, které jsou označeny jako údaje) a **Console.Error** (vytvoří protokoly, které jsou označeny jako chyba).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-288">toowrite [application tracing logs](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), use **Console.Out** (creates logs marked as INFO) and **Console.Error** (creates logs marked as ERROR).</span></span> <span data-ttu-id="b5ef5-289">Alternativou je toouse [trasování nebo TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), který poskytuje podrobné, upozornění, a kritická úrovně v přidání tooInfo a chyby.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-289">An alternative is toouse [Trace or TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), which provides Verbose, Warning, and Critical levels in addition tooInfo and Error.</span></span> <span data-ttu-id="b5ef5-290">Protokoly trasování aplikací se zobrazí v souborech protokolů hello webové aplikace, tabulky Azure, nebo v závislosti na tom, jak nakonfigurujete vaší webové aplikace Azure objektů BLOB Azure.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-290">Application tracing logs appear in hello web app log files, Azure tables, or Azure blobs depending on how you configure your Azure web app.</span></span> <span data-ttu-id="b5ef5-291">Jak platí všechny výstup konzoly hello nejnovější 100 aplikační protokoly se také zobrazí v hello stránka řídicího panelu pro hello webové úlohy, ne hello stránku pro volání funkce.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-291">As is true of all Console output, hello most recent 100 application logs also appear in hello Dashboard page for hello WebJob, not hello page for a function invocation.</span></span>

<span data-ttu-id="b5ef5-292">Výstup konzoly se zobrazí v hello řídicí panel pouze v případě, že při spuštění programu hello v webové úlohy Azure, není-li hello program spuštěn místně nebo v některých dalších prostředí.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-292">Console output appears in hello Dashboard only if hello program is running in an Azure WebJob, not if hello program is running locally or in some other environment.</span></span>

<span data-ttu-id="b5ef5-293">Protokolování můžete zakázat nastavením hello řídicí panel připojovací řetězec toonull.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-293">You can disable logging by setting hello Dashboard connection string toonull.</span></span> <span data-ttu-id="b5ef5-294">Další informace najdete v tématu [jak tooset možnosti konfigurace](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-294">For more information, see [How tooset Configuration Options](#how-to-set-configuration-options).</span></span>

<span data-ttu-id="b5ef5-295">Hello následující příklad znázorňuje několik způsobů toowrite protokoly:</span><span class="sxs-lookup"><span data-stu-id="b5ef5-295">hello following example shows several ways toowrite logs:</span></span>

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

<span data-ttu-id="b5ef5-296">V řídicím panelu WebJobs SDK hello, hello výstup hello **TextWriter** objektů, zobrazí se při návratu toohello stránky pro konkrétní funkce volání a vyberte **přepnutí výstup**:</span><span class="sxs-lookup"><span data-stu-id="b5ef5-296">In hello WebJobs SDK Dashboard, hello output from hello **TextWriter** object shows up when you go toohello page for a particular function invocation and select **Toggle Output**:</span></span>

![Vyvolání odkazu](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![Protokoly stránce volání funkce](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

<span data-ttu-id="b5ef5-299">V řídicím panelu WebJobs SDK hello, hello nejnovější 100 řádků konzoly výstup zobrazit si při přejděte toohello stránku hello webové úlohy (není pro volání funkce hello) a vyberte možnost **přepnutí výstup**.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-299">In hello WebJobs SDK Dashboard, hello most recent 100 lines of Console output show up when you go toohello page for hello WebJob (not for hello function invocation) and select **Toggle Output**.</span></span>

![Přepnutí výstup](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

<span data-ttu-id="b5ef5-301">V nepřetržitá webová úloha, aplikační protokoly objeví v/data/úlohy/průběžné/*{webjobname}*/job_log.txt v systému souborů aplikace hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-301">In a continuous WebJob, application logs show up in /data/jobs/continuous/*{webjobname}*/job_log.txt in hello web app file system.</span></span>

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

<span data-ttu-id="b5ef5-302">V aplikaci Azure blob hello protokoly vypadat například takto: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello, world!, 2014-09-26T21:01:13, chyba, contosoadsnew, 491e54, 635473620738373502,0,17404,19,Console.Error - Hello, world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello, world!,</span><span class="sxs-lookup"><span data-stu-id="b5ef5-302">In an Azure blob hello application logs look like this: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span></span>

<span data-ttu-id="b5ef5-303">A v hello tabulky Azure **Console.Out** a **Console.Error** protokoly vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="b5ef5-303">And in an Azure table hello **Console.Out** and **Console.Error** logs look like this:</span></span>

![Informace o protokolu v tabulce](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![Protokol chyb v tabulce](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

## <a name="next-steps"></a><span data-ttu-id="b5ef5-306">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5ef5-306">Next steps</span></span>
<span data-ttu-id="b5ef5-307">Tento článek poskytl kódu ukázky, zobrazující jak toohandle běžné scénáře pro práci s Azure fronty.</span><span class="sxs-lookup"><span data-stu-id="b5ef5-307">This article has provided code samples that show how toohandle common scenarios for working with Azure queues.</span></span> <span data-ttu-id="b5ef5-308">Další informace o tom, jak toouse Azure WebJobs a hello WebJobs SDK najdete v části [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="b5ef5-308">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

