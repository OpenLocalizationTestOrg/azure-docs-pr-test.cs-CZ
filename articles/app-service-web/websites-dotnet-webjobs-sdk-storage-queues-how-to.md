---
title: "Používání Azure Queue Storage se sadou WebJobs SDK"
description: "Další informace o použití úložiště fronty Azure pomocí WebJobs SDK. Vytváření a odstraňování front; Vložit, prohlížet, získání a odstranění zprávy fronty a další."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: dbfac5d9-f4a0-4e3e-9ecc-af3d7bf80463
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 63b987a2c9471f2929b8d2dd605323910d2ad43b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-queue-storage-with-the-webjobs-sdk"></a><span data-ttu-id="1debc-104">Používání Azure Queue Storage se sadou WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="1debc-104">How to use Azure queue storage with the WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="1debc-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="1debc-105">Overview</span></span>
<span data-ttu-id="1debc-106">Tato příručka obsahuje C# ukázek kódu, které ukazují, jak používat Azure WebJobs SDK verze 1.x pomocí služby Azure queue storage.</span><span class="sxs-lookup"><span data-stu-id="1debc-106">This guide provides C# code samples that show how to use the Azure WebJobs SDK version 1.x with the Azure queue storage service.</span></span>

<span data-ttu-id="1debc-107">V Průvodci se předpokládá, víte, [postup vytvoření projektu úlohy WebJob v sadě Visual Studio s připojením řetězce, které odkazují na účtu úložiště](websites-dotnet-webjobs-sdk-get-started.md) nebo [více účtů úložiště](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="1debc-107">The guide assumes you know [how to create a WebJob project in Visual Studio with connection strings that point to your storage account](websites-dotnet-webjobs-sdk-get-started.md) or to [multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

<span data-ttu-id="1debc-108">Většina fragmenty kódu zobrazit pouze funkce, není kód, který vytvoří `JobHost` objektu jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1debc-108">Most of the code snippets only show functions, not the code that creates the `JobHost` object as in this example:</span></span>

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

<span data-ttu-id="1debc-109">V Průvodci obsahuje následující témata:</span><span class="sxs-lookup"><span data-stu-id="1debc-109">The guide includes the following topics:</span></span>

* [<span data-ttu-id="1debc-110">Postup aktivace funkce při příjmu zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="1debc-110">How to trigger a function when a queue message is received</span></span>](#trigger)
  * <span data-ttu-id="1debc-111">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="1debc-111">String queue messages</span></span>
  * <span data-ttu-id="1debc-112">Zprávy fronty objektů POCO</span><span class="sxs-lookup"><span data-stu-id="1debc-112">POCO queue messages</span></span>
  * <span data-ttu-id="1debc-113">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="1debc-113">Async functions</span></span>
  * <span data-ttu-id="1debc-114">Typy atribut QueueTrigger pracuje s</span><span class="sxs-lookup"><span data-stu-id="1debc-114">Types the QueueTrigger attribute works with</span></span>
  * <span data-ttu-id="1debc-115">Algoritmus dotazování</span><span class="sxs-lookup"><span data-stu-id="1debc-115">Polling algorithm</span></span>
  * <span data-ttu-id="1debc-116">Více instancí</span><span class="sxs-lookup"><span data-stu-id="1debc-116">Multiple instances</span></span>
  * <span data-ttu-id="1debc-117">Paralelní provádění</span><span class="sxs-lookup"><span data-stu-id="1debc-117">Parallel execution</span></span>
  * <span data-ttu-id="1debc-118">Získání fronty nebo fronty zpráv metadat</span><span class="sxs-lookup"><span data-stu-id="1debc-118">Get queue or queue message metadata</span></span>
  * <span data-ttu-id="1debc-119">Řádné vypnutí</span><span class="sxs-lookup"><span data-stu-id="1debc-119">Graceful shutdown</span></span>
* [<span data-ttu-id="1debc-120">Postup vytvoření fronty zpráv při zpracování zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="1debc-120">How to create a queue message while processing a queue message</span></span>](#createqueue)
  * <span data-ttu-id="1debc-121">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="1debc-121">String queue messages</span></span>
  * <span data-ttu-id="1debc-122">Zprávy fronty objektů POCO</span><span class="sxs-lookup"><span data-stu-id="1debc-122">POCO queue messages</span></span>
  * <span data-ttu-id="1debc-123">Vytvoření více zpráv nebo asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="1debc-123">Create multiple messages or in async functions</span></span>
  * <span data-ttu-id="1debc-124">Typy atribut fronty pracuje s</span><span class="sxs-lookup"><span data-stu-id="1debc-124">Types the Queue attribute works with</span></span>
  * <span data-ttu-id="1debc-125">Použití atributů WebJobs SDK v tělo funkce</span><span class="sxs-lookup"><span data-stu-id="1debc-125">Use WebJobs SDK attributes in the body of a function</span></span>
* [<span data-ttu-id="1debc-126">Postup pro čtení a zápis objektů BLOB při zpracování zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="1debc-126">How to read and write blobs while processing a queue message</span></span>](#blobs)
  * <span data-ttu-id="1debc-127">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="1debc-127">String queue messages</span></span>
  * <span data-ttu-id="1debc-128">Zprávy fronty objektů POCO</span><span class="sxs-lookup"><span data-stu-id="1debc-128">POCO queue messages</span></span>
  * <span data-ttu-id="1debc-129">Typy objektů Blob atribut funguje s</span><span class="sxs-lookup"><span data-stu-id="1debc-129">Types the Blob attribute works with</span></span>
* [<span data-ttu-id="1debc-130">Postupy: zpracování poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="1debc-130">How to handle poison messages</span></span>](#poison)
  * <span data-ttu-id="1debc-131">Zpracování automatické poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="1debc-131">Automatic poison message handling</span></span>
  * <span data-ttu-id="1debc-132">Zpracování ručních poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="1debc-132">Manual poison message handling</span></span>
* [<span data-ttu-id="1debc-133">Jak nastavit možnosti konfigurace</span><span class="sxs-lookup"><span data-stu-id="1debc-133">How to set configuration options</span></span>](#config)
  * <span data-ttu-id="1debc-134">Sada SDK připojovacích řetězců v kódu</span><span class="sxs-lookup"><span data-stu-id="1debc-134">Set SDK connection strings in code</span></span>
  * <span data-ttu-id="1debc-135">Konfigurace nastavení QueueTrigger</span><span class="sxs-lookup"><span data-stu-id="1debc-135">Configure QueueTrigger settings</span></span>
  * <span data-ttu-id="1debc-136">Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu</span><span class="sxs-lookup"><span data-stu-id="1debc-136">Set values for WebJobs SDK constructor parameters in code</span></span>
* [<span data-ttu-id="1debc-137">Postup ruční aktivaci funkce</span><span class="sxs-lookup"><span data-stu-id="1debc-137">How to trigger a function manually</span></span>](#manual)
* [<span data-ttu-id="1debc-138">Jak napsat protokoly</span><span class="sxs-lookup"><span data-stu-id="1debc-138">How to write logs</span></span>](#logs)
* [<span data-ttu-id="1debc-139">Zpracování chyb a konfiguraci časových limitů</span><span class="sxs-lookup"><span data-stu-id="1debc-139">How to handle errors and configure timeouts</span></span>](#errors)
* [<span data-ttu-id="1debc-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1debc-140">Next steps</span></span>](#nextsteps)

## <span data-ttu-id="1debc-141"><a id="trigger"></a>Postup aktivace funkce při příjmu zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="1debc-141"><a id="trigger"></a> How to trigger a function when a queue message is received</span></span>
<span data-ttu-id="1debc-142">Chcete-li vytvořit funkci, která volá WebJobs SDK při příjmu zprávy fronty, použijte `QueueTrigger` atribut.</span><span class="sxs-lookup"><span data-stu-id="1debc-142">To write a function that the WebJobs SDK calls when a queue message is received, use the `QueueTrigger` attribute.</span></span> <span data-ttu-id="1debc-143">Konstruktoru atributu přijímá řetězcový parametr, který určuje název fronty pro cyklické dotazování.</span><span class="sxs-lookup"><span data-stu-id="1debc-143">The attribute constructor takes a string parameter that specifies the name of the queue to poll.</span></span> <span data-ttu-id="1debc-144">Můžete také [nastavit název fronty dynamicky](#config).</span><span class="sxs-lookup"><span data-stu-id="1debc-144">You can also [set the queue name dynamically](#config).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="1debc-145">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="1debc-145">String queue messages</span></span>
<span data-ttu-id="1debc-146">V následujícím příkladu fronty obsahuje řetězec zprávu, takže `QueueTrigger` se použije pro parametr řetězec s názvem `logMessage` obsahující obsah zprávy ve frontě.</span><span class="sxs-lookup"><span data-stu-id="1debc-146">In the following example, the queue contains a string message, so `QueueTrigger` is applied to a string parameter named `logMessage` which contains the content of the queue message.</span></span> <span data-ttu-id="1debc-147">Funkce [zapíše zprávu protokolu na řídicí panel](#logs).</span><span class="sxs-lookup"><span data-stu-id="1debc-147">The function [writes a log message to the Dashboard](#logs).</span></span>

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

<span data-ttu-id="1debc-148">Kromě `string`, tento parametr může být bajtové pole, `CloudQueueMessage` objekt nebo objektů POCO, které definujete.</span><span class="sxs-lookup"><span data-stu-id="1debc-148">Besides `string`, the parameter may be a byte array, a `CloudQueueMessage` object, or a POCO  that you define.</span></span>

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="1debc-149">Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="1debc-149">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="1debc-150">V následujícím příkladu obsahuje zprávy ve frontě JSON pro `BlobInformation` objekt, který zahrnuje `BlobName` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1debc-150">In the following example, the queue message contains JSON for a `BlobInformation` object which includes a `BlobName` property.</span></span> <span data-ttu-id="1debc-151">Sada SDK automaticky deserializuje objekt.</span><span class="sxs-lookup"><span data-stu-id="1debc-151">The SDK automatically deserializes the object.</span></span>

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

<span data-ttu-id="1debc-152">Sada SDK používá [balíček Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) k serializaci a deserializaci zprávy.</span><span class="sxs-lookup"><span data-stu-id="1debc-152">The SDK uses the [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) to serialize and deserialize messages.</span></span> <span data-ttu-id="1debc-153">Pokud vytvoříte fronty zpráv v aplikaci, která nepoužívá WebJobs SDK, můžete napsat kód jako v následujícím příkladu pro vytvoření zprávy fronty objektů POCO, které mohou analyzovat sady SDK.</span><span class="sxs-lookup"><span data-stu-id="1debc-153">If you create queue messages in a program that doesn't use the WebJobs SDK, you can write code like the following example to create a POCO queue message that the SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a><span data-ttu-id="1debc-154">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="1debc-154">Async functions</span></span>
<span data-ttu-id="1debc-155">Následující funkce asynchronní [zapíše do protokolu na řídicí panel](#logs).</span><span class="sxs-lookup"><span data-stu-id="1debc-155">The following async function [writes a log to the Dashboard](#logs).</span></span>

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

<span data-ttu-id="1debc-156">Asynchronní funkce může trvat [token zrušení](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), jak je znázorněno v následujícím příkladu, který se zkopíruje do objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1debc-156">Async functions may take a [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), as shown in the following example which copies a blob.</span></span> <span data-ttu-id="1debc-157">(Další informace o `queueTrigger` zástupného symbolu, najdete v článku [objekty BLOB](#blobs) části.)</span><span class="sxs-lookup"><span data-stu-id="1debc-157">(For an explanation of the `queueTrigger` placeholder, see the [Blobs](#blobs) section.)</span></span>

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <span data-ttu-id="1debc-158"><a id="qtattributetypes"></a>Typy atribut QueueTrigger pracuje s</span><span class="sxs-lookup"><span data-stu-id="1debc-158"><a id="qtattributetypes"></a> Types the QueueTrigger attribute works with</span></span>
<span data-ttu-id="1debc-159">Můžete použít `QueueTrigger` s následujícími typy:</span><span class="sxs-lookup"><span data-stu-id="1debc-159">You can use `QueueTrigger` with the following types:</span></span>

* `string`
* <span data-ttu-id="1debc-160">Typ objektů POCO serializovanou jako JSON</span><span class="sxs-lookup"><span data-stu-id="1debc-160">A POCO type serialized as JSON</span></span>
* `byte[]`
* `CloudQueueMessage`

### <span data-ttu-id="1debc-161"><a id="polling"></a>Algoritmus dotazování</span><span class="sxs-lookup"><span data-stu-id="1debc-161"><a id="polling"></a> Polling algorithm</span></span>
<span data-ttu-id="1debc-162">Sada SDK implementuje náhodných exponenciální back vypnout algoritmus, aby se snížil dopad nečinnosti-fronty dotazování na transakce náklady na úložiště.</span><span class="sxs-lookup"><span data-stu-id="1debc-162">The SDK implements a random exponential back-off algorithm to reduce the effect of idle-queue polling on storage transaction costs.</span></span>  <span data-ttu-id="1debc-163">Když se najde zprávu, sadu SDK vyčká dvou sekund a poté zkontroluje další zprávu; Pokud je nalezena žádná zpráva čeká před dalším pokusem o 4 sekundy.</span><span class="sxs-lookup"><span data-stu-id="1debc-163">When a message is found, the SDK waits two seconds and then checks for another message; when no message is found it waits about four seconds before trying again.</span></span> <span data-ttu-id="1debc-164">Po následné neúspěšných pokusech o získání zprávu fronty doba čekání nadále zvýšit, dokud nedosáhne maximální čekací doba, výchozí nastavení je na jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="1debc-164">After subsequent failed attempts to get a queue message, the wait time continues to increase until it reaches the maximum wait time, which defaults to one minute.</span></span> <span data-ttu-id="1debc-165">[Maximální čekací doba je konfigurovatelná](#config).</span><span class="sxs-lookup"><span data-stu-id="1debc-165">[The maximum wait time is configurable](#config).</span></span>

### <span data-ttu-id="1debc-166"><a id="instances"></a>Více instancí</span><span class="sxs-lookup"><span data-stu-id="1debc-166"><a id="instances"></a> Multiple instances</span></span>
<span data-ttu-id="1debc-167">Pokud vaše webová aplikace běží na několik instancí, nepřetržitá webová úloha běží na každý počítač, a každý počítač bude čekat na aktivační události a pokus o spuštění funkce.</span><span class="sxs-lookup"><span data-stu-id="1debc-167">If your web app runs on multiple instances, a continuous WebJob runs on each machine, and each machine will wait for triggers and attempt to run functions.</span></span> <span data-ttu-id="1debc-168">Aktivace fronty WebJobs SDK automaticky zabrání funkci zpracování zpráv fronty vícekrát; má být proveden zápis idempotent nemají funkce.</span><span class="sxs-lookup"><span data-stu-id="1debc-168">The WebJobs SDK queue trigger automatically prevents a function from processing a queue message multiple times; functions do not have to be written to be idempotent.</span></span> <span data-ttu-id="1debc-169">Ale pokud chcete zajistit, že pouze jedna instance funkce spustí i v případě, že existuje více instancí hostitele webové aplikace, můžete použít `Singleton` atribut.</span><span class="sxs-lookup"><span data-stu-id="1debc-169">However, if you want to ensure that only one instance of a function runs even when there are multiple instances of the host web app, you can use the `Singleton` attribute.</span></span>

### <span data-ttu-id="1debc-170"><a id="parallel"></a>Paralelní provádění</span><span class="sxs-lookup"><span data-stu-id="1debc-170"><a id="parallel"></a> Parallel execution</span></span>
<span data-ttu-id="1debc-171">Pokud máte víc funkcí naslouchá na různých front, sadu SDK je zavolá paralelně přijetí zpráv současně.</span><span class="sxs-lookup"><span data-stu-id="1debc-171">If you have multiple functions listening on different queues, the SDK will call them in parallel when messages are received simultaneously.</span></span>

<span data-ttu-id="1debc-172">Totéž platí při přijetí více zpráv pro jednu frontu.</span><span class="sxs-lookup"><span data-stu-id="1debc-172">The same is true when multiple messages are received for a single queue.</span></span> <span data-ttu-id="1debc-173">Ve výchozím nastavení sada SDK získá dávku 16 fronty zpráv v čase a provádí funkce, která je zpracuje paralelně.</span><span class="sxs-lookup"><span data-stu-id="1debc-173">By default, the SDK gets a batch of 16 queue messages at a time and executes the function that processes them in parallel.</span></span> <span data-ttu-id="1debc-174">[Velikost dávky je možné konfigurovat](#config).</span><span class="sxs-lookup"><span data-stu-id="1debc-174">[The batch size is configurable](#config).</span></span> <span data-ttu-id="1debc-175">Když počet zpracovávaných získá na polovinu velikost dávky, sadu SDK získá další dávku a spustí zpracování těchto zpráv.</span><span class="sxs-lookup"><span data-stu-id="1debc-175">When the number being processed gets down to half of the batch size, the SDK gets another batch and starts processing those messages.</span></span> <span data-ttu-id="1debc-176">Proto je maximální počet souběžných zpráv, které jsou zpracovány za funkce jeden a půl krát velikost dávky.</span><span class="sxs-lookup"><span data-stu-id="1debc-176">Therefore the maximum number of concurrent messages being processed per function is one and a half times the batch size.</span></span> <span data-ttu-id="1debc-177">Toto omezení se vztahuje samostatně pro každou funkci, která má `QueueTrigger` atribut.</span><span class="sxs-lookup"><span data-stu-id="1debc-177">This limit applies separately to each function that has a `QueueTrigger` attribute.</span></span>

<span data-ttu-id="1debc-178">Pokud nechcete, aby paralelní zpracování zprávy přijaté v jedné frontě, můžete nastavit velikost dávky na 1.</span><span class="sxs-lookup"><span data-stu-id="1debc-178">If you don't want parallel execution for messages received on one queue, you can set the batch size to 1.</span></span> <span data-ttu-id="1debc-179">Viz také **větší kontrolu nad fronty zpracování** v [RTM Azure WebJobs SDK 1.1.0](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).</span><span class="sxs-lookup"><span data-stu-id="1debc-179">See also **More control over Queue processing** in [Azure WebJobs SDK 1.1.0 RTM](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).</span></span>

### <span data-ttu-id="1debc-180"><a id="queuemetadata"></a>Získání fronty nebo fronty zpráv metadat</span><span class="sxs-lookup"><span data-stu-id="1debc-180"><a id="queuemetadata"></a>Get queue or queue message metadata</span></span>
<span data-ttu-id="1debc-181">Následující vlastnosti zprávy můžete získat tak, že přidáte parametry pro podpis metody:</span><span class="sxs-lookup"><span data-stu-id="1debc-181">You can get the following message properties by adding parameters to the method signature:</span></span>

* <span data-ttu-id="1debc-182">`DateTimeOffset`expirationTime</span><span class="sxs-lookup"><span data-stu-id="1debc-182">`DateTimeOffset` expirationTime</span></span>
* <span data-ttu-id="1debc-183">`DateTimeOffset`insertionTime</span><span class="sxs-lookup"><span data-stu-id="1debc-183">`DateTimeOffset` insertionTime</span></span>
* <span data-ttu-id="1debc-184">`DateTimeOffset`nextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="1debc-184">`DateTimeOffset` nextVisibleTime</span></span>
* <span data-ttu-id="1debc-185">`string`queueTrigger (obsahuje text zprávy)</span><span class="sxs-lookup"><span data-stu-id="1debc-185">`string` queueTrigger (contains message text)</span></span>
* <span data-ttu-id="1debc-186">`string`ID</span><span class="sxs-lookup"><span data-stu-id="1debc-186">`string` id</span></span>
* <span data-ttu-id="1debc-187">`string`Vlastnosti popReceipt</span><span class="sxs-lookup"><span data-stu-id="1debc-187">`string` popReceipt</span></span>
* <span data-ttu-id="1debc-188">`int`dequeueCount</span><span class="sxs-lookup"><span data-stu-id="1debc-188">`int` dequeueCount</span></span>

<span data-ttu-id="1debc-189">Pokud chcete pracovat přímo s úložištěm Azure API, můžete také přidat `CloudStorageAccount` parametr.</span><span class="sxs-lookup"><span data-stu-id="1debc-189">If you want to work directly with the Azure storage API, you can also add a `CloudStorageAccount` parameter.</span></span>

<span data-ttu-id="1debc-190">Následující příklad všechny tato metadata zapíše do protokolu informace o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1debc-190">The following example writes all of this metadata to an INFO application log.</span></span> <span data-ttu-id="1debc-191">V příkladu logMessage a queueTrigger obsahovat obsah zprávy ve frontě.</span><span class="sxs-lookup"><span data-stu-id="1debc-191">In the example, both logMessage and queueTrigger contain the content of the queue message.</span></span>

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

<span data-ttu-id="1debc-192">Tady je příklad protokolu o zapsaných správcem ukázkový kód:</span><span class="sxs-lookup"><span data-stu-id="1debc-192">Here is a sample log written by the sample code:</span></span>

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

### <span data-ttu-id="1debc-193"><a id="graceful"></a>Řádné vypnutí</span><span class="sxs-lookup"><span data-stu-id="1debc-193"><a id="graceful"></a>Graceful shutdown</span></span>
<span data-ttu-id="1debc-194">Funkci, která běží v nepřetržitá webová úloha může přijmout `CancellationToken` parametr, který umožňuje operačního systému a upozorňovaly vytvářené webové úlohy je možné ukončit funkce.</span><span class="sxs-lookup"><span data-stu-id="1debc-194">A function that runs in a continuous WebJob can accept a `CancellationToken` parameter which enables the operating system to notify the function when the WebJob is about to be terminated.</span></span> <span data-ttu-id="1debc-195">Toto oznámení můžete Ujistěte se, že funkce nepodporuje neočekávaně ukončí tak, aby data ponechá v nekonzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="1debc-195">You can use this notification to make sure the function doesn't terminate unexpectedly in a way that leaves data in an inconsistent state.</span></span>

<span data-ttu-id="1debc-196">Následující příklad ukazuje, jak zkontrolovat pro předcházení ukončení webové úlohy ve funkci.</span><span class="sxs-lookup"><span data-stu-id="1debc-196">The following example shows how to check for impending WebJob termination in a function.</span></span>

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

<span data-ttu-id="1debc-197">**Poznámka:** řídicí panel nemusí zobrazit správně, stav a výstup funkcí, které byly vypnuty.</span><span class="sxs-lookup"><span data-stu-id="1debc-197">**Note:** The Dashboard might not correctly show the status and output of functions that have been shut down.</span></span>

<span data-ttu-id="1debc-198">Další informace najdete v tématu [WebJobs řádné vypnutí](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span><span class="sxs-lookup"><span data-stu-id="1debc-198">For more information, see [WebJobs Graceful Shutdown](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span></span>   

## <span data-ttu-id="1debc-199"><a id="createqueue"></a>Postup vytvoření fronty zpráv při zpracování zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="1debc-199"><a id="createqueue"></a> How to create a queue message while processing a queue message</span></span>
<span data-ttu-id="1debc-200">Chcete-li vytvořit funkci, která vytvoří novou zprávu fronty, použijte `Queue` atribut.</span><span class="sxs-lookup"><span data-stu-id="1debc-200">To write a function that creates a new queue message, use the `Queue` attribute.</span></span> <span data-ttu-id="1debc-201">Jako `QueueTrigger`, kterou předáte název fronty jako řetězec, nebo můžete [nastavit název fronty dynamicky](#config).</span><span class="sxs-lookup"><span data-stu-id="1debc-201">Like `QueueTrigger`, you pass in the queue name as a string or you can [set the queue name dynamically](#config).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="1debc-202">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="1debc-202">String queue messages</span></span>
<span data-ttu-id="1debc-203">Následující ukázka kódu bez asynchronní vytvoří novou zprávu fronty ve frontě s názvem "outputqueue" se stejným obsahem jako zprávy ve frontě dostali frontu s názvem "inputqueue".</span><span class="sxs-lookup"><span data-stu-id="1debc-203">The following non-async code sample creates a new queue message in the queue named "outputqueue" with the same content as the queue message received in the queue named "inputqueue".</span></span> <span data-ttu-id="1debc-204">(Pro asynchronní použít funkce `IAsyncCollector<T>` znázorněné později v této části.)</span><span class="sxs-lookup"><span data-stu-id="1debc-204">(For async functions use `IAsyncCollector<T>` as shown later in this section.)</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="1debc-205">Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="1debc-205">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="1debc-206">Vytvoření fronty zprávu, která obsahuje objektů POCO spíše než řetězec, předá typ objektů POCO jako výstupní parametr k `Queue` konstruktoru atributu.</span><span class="sxs-lookup"><span data-stu-id="1debc-206">To create a queue message that contains a POCO rather than a string, pass the POCO type as an output parameter to the `Queue` attribute constructor.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

<span data-ttu-id="1debc-207">Sada SDK automaticky serializuje objekt do formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="1debc-207">The SDK automatically serializes the object to JSON.</span></span> <span data-ttu-id="1debc-208">Zprávu fronty je vytvořena vždy, i v případě, že objekt má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="1debc-208">A queue message is always created, even if the object is null.</span></span>

### <a name="create-multiple-messages-or-in-async-functions"></a><span data-ttu-id="1debc-209">Vytvoření více zpráv nebo asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="1debc-209">Create multiple messages or in async functions</span></span>
<span data-ttu-id="1debc-210">Pokud chcete vytvořit více zpráv, ujistěte se, typ parametru pro výstupní fronty `ICollector<T>` nebo `IAsyncCollector<T>`, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="1debc-210">To create multiple messages, make the parameter type for the output queue `ICollector<T>` or `IAsyncCollector<T>`, as shown in the following example.</span></span>

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="1debc-211">Každou zprávu fronty je vytvořena ihned po `Add` metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="1debc-211">Each queue message is created immediately when the `Add` method is called.</span></span>

### <a name="types-that-the-queue-attribute-works-with"></a><span data-ttu-id="1debc-212">Typy, které atribut fronty pracuje s</span><span class="sxs-lookup"><span data-stu-id="1debc-212">Types that the Queue attribute works with</span></span>
<span data-ttu-id="1debc-213">Můžete použít `Queue` atribut na následující typy parametrů:</span><span class="sxs-lookup"><span data-stu-id="1debc-213">You can use the `Queue` attribute on the following parameter types:</span></span>

* <span data-ttu-id="1debc-214">`out string`(Pokud je hodnota parametru jinou hodnotu než null při ukončení funkce vytvoří zprávu fronty)</span><span class="sxs-lookup"><span data-stu-id="1debc-214">`out string` (creates queue message if parameter value is non-null when the function ends)</span></span>
* <span data-ttu-id="1debc-215">`out byte[]`(funguje jako `string`)</span><span class="sxs-lookup"><span data-stu-id="1debc-215">`out byte[]` (works like `string`)</span></span>
* <span data-ttu-id="1debc-216">`out CloudQueueMessage`(funguje jako `string`)</span><span class="sxs-lookup"><span data-stu-id="1debc-216">`out CloudQueueMessage` (works like `string`)</span></span>
* <span data-ttu-id="1debc-217">`out POCO`(typu serializable vytvoří zprávu s objektem hodnotu null. Pokud má parametr hodnotu null při ukončení funkce)</span><span class="sxs-lookup"><span data-stu-id="1debc-217">`out POCO` (a serializable type, creates a message with a null object if the paramter is null when the function ends)</span></span>
* `ICollector`
* `IAsyncCollector`
* <span data-ttu-id="1debc-218">`CloudQueue`(pro vytváření zpráv ručně přímo pomocí rozhraní API služby Azure Storage)</span><span class="sxs-lookup"><span data-stu-id="1debc-218">`CloudQueue` (for creating messages manually using the Azure Storage API directly)</span></span>

### <span data-ttu-id="1debc-219"><a id="ibinder"></a>Použití atributů WebJobs SDK v tělo funkce</span><span class="sxs-lookup"><span data-stu-id="1debc-219"><a id="ibinder"></a>Use WebJobs SDK attributes in the body of a function</span></span>
<span data-ttu-id="1debc-220">Pokud potřebujete nějakou práci ve vašem funkci před použitím atribut WebJobs SDK, jako `Queue`, `Blob`, nebo `Table`, můžete použít `IBinder` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1debc-220">If you need to do some work in your function before using a WebJobs SDK attribute such as `Queue`, `Blob`, or `Table`, you can use the `IBinder` interface.</span></span>

<span data-ttu-id="1debc-221">Následující příklad přebírá zprávu vstupní fronty a vytvoří novou zprávu se stejným obsahem v výstupní fronty.</span><span class="sxs-lookup"><span data-stu-id="1debc-221">The following example takes an input queue message and creates a new message with the same content in an output queue.</span></span> <span data-ttu-id="1debc-222">Název fronty výstupu je nastavena podle kódu v těle funkce.</span><span class="sxs-lookup"><span data-stu-id="1debc-222">The output queue name is set by code in the body of the function.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

<span data-ttu-id="1debc-223">`IBinder` Rozhraní lze také s `Table` a `Blob` atributy.</span><span class="sxs-lookup"><span data-stu-id="1debc-223">The `IBinder` interface can also be used with the `Table` and `Blob` attributes.</span></span>

## <span data-ttu-id="1debc-224"><a id="blobs"></a>Tom, jak číst a zapisovat objekty BLOB a tabulek při zpracování zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="1debc-224"><a id="blobs"></a> How to read and write blobs and tables while processing a queue message</span></span>
<span data-ttu-id="1debc-225">`Blob` a `Table` atributů umožňují čtení a zápisu objektů BLOB a tabulek.</span><span class="sxs-lookup"><span data-stu-id="1debc-225">The `Blob` and `Table` attributes enable you to read and write blobs and tables.</span></span> <span data-ttu-id="1debc-226">Ukázky v této části se vztahují na objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="1debc-226">The samples in this section apply to blobs.</span></span> <span data-ttu-id="1debc-227">Ukázky kódu, které ukazují, jak aktivovat procesy při objekty BLOB jsou vytvoření nebo aktualizaci, najdete v části [používání úložiště objektů blob v Azure pomocí WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)a ukázky kódu, které pro čtení a zápis tabulky, najdete v části [použití úložiště tabulek Azure s WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="1debc-227">For code samples that show how to trigger processes when blobs are created or updated, see [How to use Azure blob storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), and for code samples that read and write tables, see [How to use Azure table storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span></span>

### <a name="string-queue-messages-triggering-blob-operations"></a><span data-ttu-id="1debc-228">Zprávy fronty řetězec spuštění operace objektů blob</span><span class="sxs-lookup"><span data-stu-id="1debc-228">String queue messages triggering blob operations</span></span>
<span data-ttu-id="1debc-229">Pro zprávu fronty, který obsahuje řetězec `queueTrigger` je zástupný symbol v můžete použít `Blob` atributu `blobPath` parametr, který obsahuje obsah zprávy.</span><span class="sxs-lookup"><span data-stu-id="1debc-229">For a queue message that contains a string, `queueTrigger` is a placeholder you can use in the `Blob` attribute's `blobPath` parameter that contains the contents of the message.</span></span>

<span data-ttu-id="1debc-230">Následující příklad používá `Stream` objekty ke čtení a zápisu objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="1debc-230">The following example uses `Stream` objects to read and write blobs.</span></span> <span data-ttu-id="1debc-231">Zprávy ve frontě je název objektu blob v kontejneru textblobs.</span><span class="sxs-lookup"><span data-stu-id="1debc-231">The queue message is the name of a blob located in the textblobs container.</span></span> <span data-ttu-id="1debc-232">Kopie objektu blob s "-nové" připojenou k názvu se vytvoří ve stejném kontejneru.</span><span class="sxs-lookup"><span data-stu-id="1debc-232">A copy of the blob with "-new" appended to the name is created in the same container.</span></span>

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="1debc-233">`Blob` Atributu konstruktoru trvá `blobPath` parametr, který určuje název kontejneru a objektů blob.</span><span class="sxs-lookup"><span data-stu-id="1debc-233">The `Blob` attribute constructor takes a `blobPath` parameter that specifies the container and blob name.</span></span> <span data-ttu-id="1debc-234">Další informace o tento zástupný text najdete v tématu [používání úložiště objektů blob v Azure pomocí WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),</span><span class="sxs-lookup"><span data-stu-id="1debc-234">For more information about this placeholder, see [How to use Azure blob storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),</span></span>

<span data-ttu-id="1debc-235">Pokud atribut upraví `Stream` objektu určuje jiný parametr konstruktoru `FileAccess` režimu pro čtení, zápisu nebo čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="1debc-235">When the attribute decorates a `Stream` object, another constructor parameter specifies the `FileAccess` mode as read, write, or read/write.</span></span>

<span data-ttu-id="1debc-236">Následující příklad používá `CloudBlockBlob` objekt, který chcete odstranit objekt blob.</span><span class="sxs-lookup"><span data-stu-id="1debc-236">The following example uses a `CloudBlockBlob` object to delete a blob.</span></span> <span data-ttu-id="1debc-237">Zprávy ve frontě je název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1debc-237">The queue message is the name of the blob.</span></span>

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <span data-ttu-id="1debc-238"><a id="pocoblobs"></a>Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="1debc-238"><a id="pocoblobs"></a> POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="1debc-239">Pro objektů POCO uloží jako dokumenty JSON v zprávy ve frontě, můžete použít zástupné znaky, které název vlastnosti v objektu `Queue` atributu `blobPath` parametr.</span><span class="sxs-lookup"><span data-stu-id="1debc-239">For a POCO stored as JSON in the queue message, you can use placeholders that name properties of the object in the `Queue` attribute's `blobPath` parameter.</span></span> <span data-ttu-id="1debc-240">Můžete také použít [fronty názvy vlastností metadat](#queuemetadata) zástupnými symboly.</span><span class="sxs-lookup"><span data-stu-id="1debc-240">You can also use [queue metadata property names](#queuemetadata) as placeholders.</span></span>

<span data-ttu-id="1debc-241">Následující příklad zkopíruje objekt blob do nového objektu blob s jiné rozšíření.</span><span class="sxs-lookup"><span data-stu-id="1debc-241">The following example copies a blob to a new blob with a different extension.</span></span> <span data-ttu-id="1debc-242">Zprávy ve frontě je `BlobInformation` objekt, který zahrnuje `BlobName` a `BlobNameWithoutExtension` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="1debc-242">The queue message is a `BlobInformation` object that includes `BlobName` and `BlobNameWithoutExtension` properties.</span></span> <span data-ttu-id="1debc-243">Názvy vlastností, které jsou použity jako zástupné symboly v cestě objektu blob pro `Blob` atributy.</span><span class="sxs-lookup"><span data-stu-id="1debc-243">The property names are used as placeholders in the blob path for the `Blob` attributes.</span></span>

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="1debc-244">Sada SDK používá [balíček Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) k serializaci a deserializaci zprávy.</span><span class="sxs-lookup"><span data-stu-id="1debc-244">The SDK uses the [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) to serialize and deserialize messages.</span></span> <span data-ttu-id="1debc-245">Pokud vytvoříte fronty zpráv v aplikaci, která nepoužívá WebJobs SDK, můžete napsat kód jako v následujícím příkladu pro vytvoření zprávy fronty objektů POCO, které mohou analyzovat sady SDK.</span><span class="sxs-lookup"><span data-stu-id="1debc-245">If you create queue messages in a program that doesn't use the WebJobs SDK, you can write code like the following example to create a POCO queue message that the SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

<span data-ttu-id="1debc-246">Pokud potřebujete nějakou práci ve vašem funkci před vazby objektu blob na objekt, můžete použít atribut v těle funkce, [jak je uvedeno výše pro atribut fronty](#ibinder).</span><span class="sxs-lookup"><span data-stu-id="1debc-246">If you need to do some work in your function before binding a blob to an object, you can use the attribute in the body of the function, [as shown earlier for the Queue attribute](#ibinder).</span></span>

### <span data-ttu-id="1debc-247"><a id="blobattributetypes"></a>Typy, které můžete použít atribut objektů Blob s</span><span class="sxs-lookup"><span data-stu-id="1debc-247"><a id="blobattributetypes"></a> Types you can use the Blob attribute with</span></span>
<span data-ttu-id="1debc-248">`Blob` Atribut lze použít s následujícími typy:</span><span class="sxs-lookup"><span data-stu-id="1debc-248">The `Blob` attribute can be used with the following types:</span></span>

* <span data-ttu-id="1debc-249">`Stream`(pro čtení nebo zápisu, zadán pomocí parametru FileAccess konstruktor)</span><span class="sxs-lookup"><span data-stu-id="1debc-249">`Stream` (read or write, specified by using the FileAccess constructor parameter)</span></span>
* `TextReader`
* `TextWriter`
* <span data-ttu-id="1debc-250">`string`(pro čtení)</span><span class="sxs-lookup"><span data-stu-id="1debc-250">`string` (read)</span></span>
* <span data-ttu-id="1debc-251">`out string`(zapisovat; vytvoří objekt blob jenom v případě, že parametr řetězce je jinou hodnotu než null, pokud funkce vrátí hodnotu)</span><span class="sxs-lookup"><span data-stu-id="1debc-251">`out string` (write; creates a blob only if the string parameter is non-null when the function returns)</span></span>
* <span data-ttu-id="1debc-252">Objektů POCO (čtení)</span><span class="sxs-lookup"><span data-stu-id="1debc-252">POCO (read)</span></span>
* <span data-ttu-id="1debc-253">out objektů POCO (zápisu; vždycky vytvoří objekt blob, vytvoří jako objektu null, pokud parametr objektů POCO má hodnotu null, pokud funkce vrátí hodnotu)</span><span class="sxs-lookup"><span data-stu-id="1debc-253">out POCO (write; always creates a blob, creates as null object if POCO parameter is null when the function returns)</span></span>
* <span data-ttu-id="1debc-254">`CloudBlobStream`(zápisu)</span><span class="sxs-lookup"><span data-stu-id="1debc-254">`CloudBlobStream` (write)</span></span>
* <span data-ttu-id="1debc-255">`ICloudBlob`(pro čtení nebo zápisu)</span><span class="sxs-lookup"><span data-stu-id="1debc-255">`ICloudBlob` (read or write)</span></span>
* <span data-ttu-id="1debc-256">`CloudBlockBlob`(pro čtení nebo zápisu)</span><span class="sxs-lookup"><span data-stu-id="1debc-256">`CloudBlockBlob` (read or write)</span></span>
* <span data-ttu-id="1debc-257">`CloudPageBlob`(pro čtení nebo zápisu)</span><span class="sxs-lookup"><span data-stu-id="1debc-257">`CloudPageBlob` (read or write)</span></span>

## <span data-ttu-id="1debc-258"><a id="poison"></a>Postupy: zpracování poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="1debc-258"><a id="poison"></a> How to handle poison messages</span></span>
<span data-ttu-id="1debc-259">Zprávy, jejichž obsah způsobí, že funkce, která se nezdaří se nazývají *škodlivých zpráv*.</span><span class="sxs-lookup"><span data-stu-id="1debc-259">Messages whose content causes a function to fail are called *poison messages*.</span></span> <span data-ttu-id="1debc-260">Pokud se funkce nezdaří, zprávy ve frontě není odstraněna a nakonec je převzata znovu způsobuje cyklus opakovat.</span><span class="sxs-lookup"><span data-stu-id="1debc-260">When the function fails, the queue message is not deleted and eventually is picked up again, causing the cycle to be repeated.</span></span> <span data-ttu-id="1debc-261">Sada SDK může automaticky přerušení na cyklus po omezený počet iterací, nebo můžete provést ručně.</span><span class="sxs-lookup"><span data-stu-id="1debc-261">The SDK can automatically interrupt the cycle after a limited number of iterations, or you can do it manually.</span></span>

### <a name="automatic-poison-message-handling"></a><span data-ttu-id="1debc-262">Zpracování automatické poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="1debc-262">Automatic poison message handling</span></span>
<span data-ttu-id="1debc-263">Sada SDK bude volat funkci až 5 x zpracovat zprávu fronty.</span><span class="sxs-lookup"><span data-stu-id="1debc-263">The SDK will call a function up to 5 times to process a queue message.</span></span> <span data-ttu-id="1debc-264">V případě selhání páté zkuste zpráva bude přesunuta do fronty poškozených.</span><span class="sxs-lookup"><span data-stu-id="1debc-264">If the fifth try fails, the message is moved to a poison queue.</span></span> <span data-ttu-id="1debc-265">[Maximální počet opakovaných pokusů se dá konfigurovat](#config).</span><span class="sxs-lookup"><span data-stu-id="1debc-265">[The maximum number of retries is configurable](#config).</span></span>

<span data-ttu-id="1debc-266">Poškozených fronty jmenuje *{originalqueuename}*-poškozených.</span><span class="sxs-lookup"><span data-stu-id="1debc-266">The poison queue is named *{originalqueuename}*-poison.</span></span> <span data-ttu-id="1debc-267">Můžete napsat, že je funkce, která se zpracování zpráv z fronty poškozených jejich protokolování nebo odesílání oznámení, že ruční pozornost.</span><span class="sxs-lookup"><span data-stu-id="1debc-267">You can write a function to process messages from the poison queue by logging them or sending a notification that manual attention is needed.</span></span>

<span data-ttu-id="1debc-268">V následujícím příkladu `CopyBlob` funkce se nezdaří, pokud zpráva fronty obsahuje název objektu blob, který neexistuje.</span><span class="sxs-lookup"><span data-stu-id="1debc-268">In the following example the `CopyBlob` function will fail when a queue message contains the name of a blob that doesn't exist.</span></span> <span data-ttu-id="1debc-269">Pokud k tomu dojde, je zpráva přesunuta do fronty copyblobqueue poison z fronty copyblobqueue.</span><span class="sxs-lookup"><span data-stu-id="1debc-269">When that happens, the message is moved from the copyblobqueue queue to the copyblobqueue-poison queue.</span></span> <span data-ttu-id="1debc-270">`ProcessPoisonMessage` Pak protokoly poškozená zpráva.</span><span class="sxs-lookup"><span data-stu-id="1debc-270">The `ProcessPoisonMessage` then logs the poison message.</span></span>

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

<span data-ttu-id="1debc-271">Následující obrázek znázorňuje výstup konzoly z těchto funkcí při zpracování poškozená zpráva.</span><span class="sxs-lookup"><span data-stu-id="1debc-271">The following illustration shows console output from these functions when a poison message is processed.</span></span>

![Výstup konzoly pro zpracování poškozených zpráv](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### <a name="manual-poison-message-handling"></a><span data-ttu-id="1debc-273">Zpracování ručních poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="1debc-273">Manual poison message handling</span></span>
<span data-ttu-id="1debc-274">Počet, kolikrát byla převzata zprávu pro zpracování můžete získat tak, že přidáte `int` parametr s názvem `dequeueCount` na funkce.</span><span class="sxs-lookup"><span data-stu-id="1debc-274">You can get the number of times a message has been picked up for processing by adding an `int` parameter named `dequeueCount` to your function.</span></span> <span data-ttu-id="1debc-275">Potom můžete zkontrolovat počet dequeue v kódu funkce a provádět vlastní nezpracovatelná zpráva zpracování Pokud počet překročí prahovou hodnotu, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="1debc-275">You can then check the dequeue count in function code and perform your own poison message handling when the number exceeds a threshold, as shown in the following example.</span></span>

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

## <span data-ttu-id="1debc-276"><a id="config"></a>Jak nastavit možnosti konfigurace</span><span class="sxs-lookup"><span data-stu-id="1debc-276"><a id="config"></a> How to set configuration options</span></span>
<span data-ttu-id="1debc-277">Můžete použít `JobHostConfiguration` typ a nastavte následující možnosti konfigurace:</span><span class="sxs-lookup"><span data-stu-id="1debc-277">You can use the `JobHostConfiguration` type to set the following configuration options:</span></span>

* <span data-ttu-id="1debc-278">Nastavte SDK připojovacích řetězců v kódu.</span><span class="sxs-lookup"><span data-stu-id="1debc-278">Set the SDK connection strings in code.</span></span>
* <span data-ttu-id="1debc-279">Konfigurace `QueueTrigger` nastavení, jako například maximální počet vyřazení z fronty.</span><span class="sxs-lookup"><span data-stu-id="1debc-279">Configure `QueueTrigger` settings such as maximum dequeue count.</span></span>
* <span data-ttu-id="1debc-280">Získáte názvy fronty z konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1debc-280">Get queue names from configuration.</span></span>

### <span data-ttu-id="1debc-281"><a id="setconnstr"></a>Sada SDK připojovacích řetězců v kódu</span><span class="sxs-lookup"><span data-stu-id="1debc-281"><a id="setconnstr"></a>Set SDK connection strings in code</span></span>
<span data-ttu-id="1debc-282">Nastavení SDK připojovacích řetězců v kódu můžete použít vlastní názvy řetězec připojení v konfiguračních souborů nebo proměnné prostředí, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="1debc-282">Setting the SDK connection strings in code enables you to use your own connection string names in configuration files or environment variables, as shown in the following example.</span></span>

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

### <span data-ttu-id="1debc-283"><a id="configqueue"></a>Konfigurace nastavení QueueTrigger</span><span class="sxs-lookup"><span data-stu-id="1debc-283"><a id="configqueue"></a>Configure QueueTrigger  settings</span></span>
<span data-ttu-id="1debc-284">Můžete nakonfigurovat následující nastavení, které platí pro zpracování zprávy fronty:</span><span class="sxs-lookup"><span data-stu-id="1debc-284">You can configure the following settings that apply to the queue message processing:</span></span>

* <span data-ttu-id="1debc-285">Maximální počet zpráv fronty, které jsou zachyceny současně spouštění paralelní (výchozí hodnota je 16).</span><span class="sxs-lookup"><span data-stu-id="1debc-285">The maximum number of queue messages that are picked up simultaneously to be executed in parallel (default is 16).</span></span>
* <span data-ttu-id="1debc-286">Maximální počet opakovaných pokusů, než odešle zprávu fronty poškozených fronty (výchozí hodnota je 5).</span><span class="sxs-lookup"><span data-stu-id="1debc-286">The maximum number of retries before a queue message is sent to a poison queue (default is 5).</span></span>
* <span data-ttu-id="1debc-287">Maximální čekací dobu před znovu dotazování, když se fronta je prázdný (výchozí hodnota je 1 minuta).</span><span class="sxs-lookup"><span data-stu-id="1debc-287">The maximum wait time before polling again when a queue is empty (default is 1 minute).</span></span>

<span data-ttu-id="1debc-288">Následující příklad ukazuje, jak nakonfigurovat tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="1debc-288">The following example shows how to configure these settings:</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <span data-ttu-id="1debc-289"><a id="setnamesincode"></a>Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu</span><span class="sxs-lookup"><span data-stu-id="1debc-289"><a id="setnamesincode"></a>Set values for WebJobs SDK constructor parameters in code</span></span>
<span data-ttu-id="1debc-290">Někdy chcete zadat název fronty, název objektu blob nebo kontejner, nebo tabulku název v kódu než pevný kódu.</span><span class="sxs-lookup"><span data-stu-id="1debc-290">Sometimes you want to specify a queue name, a blob name or container, or a table name in code rather than hard-code it.</span></span> <span data-ttu-id="1debc-291">Například můžete chtít zadat název fronty pro `QueueTrigger` do proměnné prostředí nebo soubor konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1debc-291">For example, you might want to specify the queue name for `QueueTrigger` in a configuration file or environment variable.</span></span>

<span data-ttu-id="1debc-292">Můžete to udělat pomocí předávání `NameResolver` do objektu `JobHostConfiguration` typu.</span><span class="sxs-lookup"><span data-stu-id="1debc-292">You can do that by passing in a `NameResolver` object to the `JobHostConfiguration` type.</span></span> <span data-ttu-id="1debc-293">Zahrnout speciální zástupné symboly obklopená znaky procenta (%) v parametrech konstruktoru atributu WebJobs SDK a `NameResolver` kódu určuje skutečné hodnoty má být použit místo tyto zástupné symboly.</span><span class="sxs-lookup"><span data-stu-id="1debc-293">You include special placeholders surrounded by percent (%) signs in WebJobs SDK attribute constructor parameters, and your `NameResolver` code specifies the actual values to be used in place of those placeholders.</span></span>

<span data-ttu-id="1debc-294">Předpokládejme například, že chcete použít frontu s názvem logqueuetest v testovacím prostředí a jednu s názvem logqueueprod v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1debc-294">For example, suppose you want to use a queue named logqueuetest in the test environment and one named logqueueprod in production.</span></span> <span data-ttu-id="1debc-295">Místo názvu pevně fronty, které chcete určit název záznamu v `appSettings` kolekce, která by měla mít název skutečné fronty.</span><span class="sxs-lookup"><span data-stu-id="1debc-295">Instead of a hard-coded queue name, you want to specify the name of an entry in the `appSettings` collection that would have the actual queue name.</span></span> <span data-ttu-id="1debc-296">Pokud `appSettings` klíč je logqueue, funkce může vypadat podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="1debc-296">If the `appSettings` key is logqueue, your function could look like the following example.</span></span>

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

<span data-ttu-id="1debc-297">Vaše `NameResolver` třídy, může získat název fronty z `appSettings` jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1debc-297">Your `NameResolver` class could then get the queue name from `appSettings` as shown in the following example:</span></span>

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

<span data-ttu-id="1debc-298">Předáte `NameResolver` třídy v do `JobHost` objektu, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="1debc-298">You pass the `NameResolver` class in to the `JobHost` object as shown in the following example.</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

<span data-ttu-id="1debc-299">**Poznámka:** Queue, table a názvy objektů blob jsou vyřešeny pokaždé, když je volána funkce, ale překladu názvů kontejner objektů blob pouze při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="1debc-299">**Note:** Queue, table, and blob names are resolved each time a function is called, but blob container names are resolved only when the application starts.</span></span> <span data-ttu-id="1debc-300">Název kontejneru objektu blob nelze změnit, když úloha běží.</span><span class="sxs-lookup"><span data-stu-id="1debc-300">You can't change blob container name while the job is running.</span></span>

## <span data-ttu-id="1debc-301"><a id="manual"></a>Postup ruční aktivaci funkce</span><span class="sxs-lookup"><span data-stu-id="1debc-301"><a id="manual"></a>How to trigger a function manually</span></span>
<span data-ttu-id="1debc-302">Chcete-li aktivovat funkci ručně, použijte `Call` nebo `CallAsync` metodu `JobHost` objektu a `NoAutomaticTrigger` atribut na funkci, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="1debc-302">To trigger a function manually, use the `Call` or `CallAsync` method on the `JobHost` object and the `NoAutomaticTrigger` attribute on the function, as shown in the following example.</span></span>

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

## <span data-ttu-id="1debc-303"><a id="logs"></a>Jak napsat protokoly</span><span class="sxs-lookup"><span data-stu-id="1debc-303"><a id="logs"></a>How to write logs</span></span>
<span data-ttu-id="1debc-304">Řídicí panel zobrazuje protokoly na dvou místech: této webové stránky a stránky pro konkrétní vyvolání webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="1debc-304">The Dashboard shows logs in two places: the page for the WebJob, and the page for a particular WebJob invocation.</span></span>

![Protokoly v webová stránka](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![Protokoly stránce volání funkce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

<span data-ttu-id="1debc-307">Výstup konzoly metody, kterou je možné volat ve funkci nebo v `Main()` metoda se zobrazí na stránce řídicího panelu pro vytvářené webové úlohy, není na stránce pro volání konkrétní metody.</span><span class="sxs-lookup"><span data-stu-id="1debc-307">Output from Console methods that you call in a function or in the `Main()` method appears in the Dashboard page for the WebJob, not in the page for a particular method invocation.</span></span> <span data-ttu-id="1debc-308">Výstup z objekt TextWriter, kterou můžete získat z parametr ve vašem podpis metody se zobrazí na stránce řídicího panelu pro volání metody.</span><span class="sxs-lookup"><span data-stu-id="1debc-308">Output from the TextWriter object that you get from a parameter in your method signature appears in the Dashboard page for a method invocation.</span></span>

<span data-ttu-id="1debc-309">Výstup konzoly, nelze ho propojit s volání konkrétní metody, protože konzole je jedním podprocesem, spuštěného mnoho funkcí úloha může být ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="1debc-309">Console output can't be linked to a particular method invocation because the Console is single-threaded, while many job functions may be running at the same time.</span></span> <span data-ttu-id="1debc-310">To je důvod, proč sada SDK poskytuje každé vyvolání funkce s objekt zapisovače svůj vlastní jedinečný protokolu.</span><span class="sxs-lookup"><span data-stu-id="1debc-310">That's why the  SDK provides each function invocation with its own unique log writer object.</span></span>

<span data-ttu-id="1debc-311">Zápis [protokoly trasování aplikací](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), použijte `Console.Out` (vytvoří protokoly, které jsou označeny jako údaje) a `Console.Error` (vytvoří protokoly, které jsou označeny jako chyba).</span><span class="sxs-lookup"><span data-stu-id="1debc-311">To write [application tracing logs](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), use `Console.Out` (creates logs marked as INFO) and `Console.Error` (creates logs marked as ERROR).</span></span> <span data-ttu-id="1debc-312">Další možností je použít [trasování nebo TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), který poskytuje podrobné, upozornění, a kritická úrovně kromě údaje a informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="1debc-312">An alternative is to use [Trace or TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), which provides Verbose, Warning, and Critical levels in addition to Info and Error.</span></span> <span data-ttu-id="1debc-313">Protokoly trasování aplikací se zobrazí v souborech protokolů webové aplikace, tabulky Azure, nebo v závislosti na tom, jak nakonfigurujete vaší webové aplikace Azure objektů BLOB Azure.</span><span class="sxs-lookup"><span data-stu-id="1debc-313">Application tracing logs appear in the web app log files, Azure tables, or Azure blobs depending on how you configure your Azure web app.</span></span> <span data-ttu-id="1debc-314">Platí všechny výstup konzoly nejnovější protokoly 100 aplikací také se zobrazí na stránce řídicího panelu pro webové úlohy, není na stránce pro volání funkce.</span><span class="sxs-lookup"><span data-stu-id="1debc-314">As is true of all Console output, the most recent 100 application logs also appear in the Dashboard page for the WebJob, not the page for a function invocation.</span></span>

<span data-ttu-id="1debc-315">Výstup konzoly se zobrazí v řídicím panelu pouze v případě, že je aplikace spuštěna v webové úlohy Azure není v případě, že program spuštěn místně nebo v některých dalších prostředí.</span><span class="sxs-lookup"><span data-stu-id="1debc-315">Console output appears in the Dashboard only if the program is running in an Azure WebJob, not if the program is running locally or in some other environment.</span></span>

<span data-ttu-id="1debc-316">Zakázání protokolování řídicí panel pro scénáře vysoké propustnosti.</span><span class="sxs-lookup"><span data-stu-id="1debc-316">Disable dashboard logging for high throughput scenarios.</span></span> <span data-ttu-id="1debc-317">Ve výchozím nastavení sadu SDK zapisuje protokoly do úložiště a tato aktivita může snížit výkon při zpracování velkého počtu zpráv.</span><span class="sxs-lookup"><span data-stu-id="1debc-317">By default, the SDK writes logs to storage, and this activity could degrade performance when you are processing many messages.</span></span> <span data-ttu-id="1debc-318">Chcete-li zakázat protokolování, nastavte řídicí panel připojovací řetězec na hodnotu null, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="1debc-318">To disable logging, set the dashboard connection string to null as shown in the following example.</span></span>

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = "";        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

<span data-ttu-id="1debc-319">Následující příklad ukazuje několik způsobů, jak zápis protokolů:</span><span class="sxs-lookup"><span data-stu-id="1debc-319">The following example shows several ways to write logs:</span></span>

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

<span data-ttu-id="1debc-320">V řídicím WebJobs SDK na výstup `TextWriter` objekt zobrazí až když přejdete na stránku pro konkrétní funkce volání a klikněte na tlačítko **přepnutí výstup**:</span><span class="sxs-lookup"><span data-stu-id="1debc-320">In the WebJobs SDK Dashboard, the output from the `TextWriter` object shows up when you go to the page for a particular function invocation and click **Toggle Output**:</span></span>

![Klikněte na odkaz volání funkce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![Protokoly stránce volání funkce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

<span data-ttu-id="1debc-323">Na řídicím panelu WebJobs SDK nejnovější 100 řádků konzoly výstup zobrazit až když přejdete na stránku pro webové úlohy (není pro volání funkce) a klikněte na tlačítko **přepnutí výstup**.</span><span class="sxs-lookup"><span data-stu-id="1debc-323">In the WebJobs SDK Dashboard, the most recent 100 lines of Console output show up when you go to the page for the WebJob (not for the function invocation) and click **Toggle Output**.</span></span>

![Klikněte na přepínač výstup](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

<span data-ttu-id="1debc-325">V nepřetržitá webová úloha, aplikační protokoly objeví v/data/úlohy/průběžné/*{webjobname}*/job_log.txt v systému souborů webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1debc-325">In a continuous WebJob, application logs show up in /data/jobs/continuous/*{webjobname}*/job_log.txt in the web app file system.</span></span>

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

<span data-ttu-id="1debc-326">V Azure blob vzhledu protokoly aplikace takto: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello, world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello, world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello, world!,</span><span class="sxs-lookup"><span data-stu-id="1debc-326">In an Azure blob the application logs look like this: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span></span>

<span data-ttu-id="1debc-327">A v tabulce Azure `Console.Out` a `Console.Error` protokoly vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="1debc-327">And in an Azure table the `Console.Out` and `Console.Error` logs look like this:</span></span>

![Informace o protokolu v tabulce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![Protokol chyb v tabulce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

<span data-ttu-id="1debc-330">Pokud chcete zařadit vlastní protokoly, přečtěte si téma [v tomto příkladu](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="1debc-330">If you want to plug in your own logger, see [this example](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).</span></span>

## <span data-ttu-id="1debc-331"><a id="errors"></a>Zpracování chyb a konfiguraci časových limitů</span><span class="sxs-lookup"><span data-stu-id="1debc-331"><a id="errors"></a>How to handle errors and configure timeouts</span></span>
<span data-ttu-id="1debc-332">Také zahrnuje WebJobs SDK [časový limit](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) atribut, který můžete použít, aby funkce, která se zruší, pokud se nedokončí ve stanoveném intervalu.</span><span class="sxs-lookup"><span data-stu-id="1debc-332">The WebJobs SDK also includes a [Timeout](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) attribute that you can use to cause a function to be canceled if doesn't complete within a specified amount of time.</span></span> <span data-ttu-id="1debc-333">A pokud chcete vygenerovat výstrahu v případě příliš mnoho chyb dojít v zadaném časovém období, můžete použít `ErrorTrigger` atribut.</span><span class="sxs-lookup"><span data-stu-id="1debc-333">And if you want to raise an alert when too many errors happen within a specified period of time, you can use the `ErrorTrigger` attribute.</span></span> <span data-ttu-id="1debc-334">Tady je [ErrorTrigger příklad](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).</span><span class="sxs-lookup"><span data-stu-id="1debc-334">Here is an [ErrorTrigger example](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).</span></span>

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    To = "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors to the Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

<span data-ttu-id="1debc-335">Můžete také dynamicky zakázat a povolit funkce řízení, zda se můžete spustit, a to pomocí konfigurace přepínače, které by mohly být nastavení aplikace nebo název proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="1debc-335">You can also dynamically disable and enable functions to control whether they can be triggered, by using a configuration switch that could be an app setting or environment variable name.</span></span> <span data-ttu-id="1debc-336">Ukázkový kód, najdete `Disable` atribut [WebJobs SDK ukázky úložiště](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).</span><span class="sxs-lookup"><span data-stu-id="1debc-336">For sample code, see the `Disable` attribute in [the WebJobs SDK samples repository](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).</span></span>

## <span data-ttu-id="1debc-337"><a id="nextsteps"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="1debc-337"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="1debc-338">Tato příručka poskytl ukázek kódu, které ukazují, jak zpracovat běžné scénáře pro práci s Azure fronty.</span><span class="sxs-lookup"><span data-stu-id="1debc-338">This guide has provided code samples that show how to handle common scenarios for working with Azure queues.</span></span> <span data-ttu-id="1debc-339">Další informace o tom, jak používat Azure WebJobs a WebJobs SDK najdete v tématu [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="1debc-339">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>
