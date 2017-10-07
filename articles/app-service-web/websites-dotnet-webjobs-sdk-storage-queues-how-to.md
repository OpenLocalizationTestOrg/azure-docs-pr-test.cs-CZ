---
title: aaaHow toouse fronty Azure storage s hello WebJobs SDK
description: "Zjistěte, jak toouse Azure fronty úložiště s hello WebJobs SDK. Vytváření a odstraňování front; Vložit, prohlížet, získání a odstranění zprávy fronty a další."
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
ms.openlocfilehash: 49f844436b0453489800b2762a5c7dc30b9db805
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-queue-storage-with-hello-webjobs-sdk"></a><span data-ttu-id="aac85-104">Jak toouse Azure fronty úložiště s hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="aac85-104">How toouse Azure queue storage with hello WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="aac85-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="aac85-105">Overview</span></span>
<span data-ttu-id="aac85-106">Tato příručka obsahuje C# ukázek kódu, které ukazují, jak toouse hello Azure WebJobs SDK verze 1.x s hello fronty Azure storage service.</span><span class="sxs-lookup"><span data-stu-id="aac85-106">This guide provides C# code samples that show how toouse hello Azure WebJobs SDK version 1.x with hello Azure queue storage service.</span></span>

<span data-ttu-id="aac85-107">Hello Příručka předpokládá znáte [jak toocreate projekt webové úlohy v sadě Visual Studio se připojení řetězce daný účet úložiště bodu tooyour](websites-dotnet-webjobs-sdk-get-started.md) nebo příliš[více účtů úložiště](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="aac85-107">hello guide assumes you know [how toocreate a WebJob project in Visual Studio with connection strings that point tooyour storage account](websites-dotnet-webjobs-sdk-get-started.md) or too[multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

<span data-ttu-id="aac85-108">Většina fragmenty kódu hello zobrazit pouze funkce, není hello kód, který vytvoří hello `JobHost` objektu jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="aac85-108">Most of hello code snippets only show functions, not hello code that creates hello `JobHost` object as in this example:</span></span>

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

<span data-ttu-id="aac85-109">Příručka Hello obsahuje hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="aac85-109">hello guide includes hello following topics:</span></span>

* [<span data-ttu-id="aac85-110">Jak tootrigger funkce při příjmu zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="aac85-110">How tootrigger a function when a queue message is received</span></span>](#trigger)
  * <span data-ttu-id="aac85-111">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="aac85-111">String queue messages</span></span>
  * <span data-ttu-id="aac85-112">Zprávy fronty objektů POCO</span><span class="sxs-lookup"><span data-stu-id="aac85-112">POCO queue messages</span></span>
  * <span data-ttu-id="aac85-113">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="aac85-113">Async functions</span></span>
  * <span data-ttu-id="aac85-114">Typy hello QueueTrigger atribut pracuje s</span><span class="sxs-lookup"><span data-stu-id="aac85-114">Types hello QueueTrigger attribute works with</span></span>
  * <span data-ttu-id="aac85-115">Algoritmus dotazování</span><span class="sxs-lookup"><span data-stu-id="aac85-115">Polling algorithm</span></span>
  * <span data-ttu-id="aac85-116">Více instancí</span><span class="sxs-lookup"><span data-stu-id="aac85-116">Multiple instances</span></span>
  * <span data-ttu-id="aac85-117">Paralelní provádění</span><span class="sxs-lookup"><span data-stu-id="aac85-117">Parallel execution</span></span>
  * <span data-ttu-id="aac85-118">Získání fronty nebo fronty zpráv metadat</span><span class="sxs-lookup"><span data-stu-id="aac85-118">Get queue or queue message metadata</span></span>
  * <span data-ttu-id="aac85-119">Řádné vypnutí</span><span class="sxs-lookup"><span data-stu-id="aac85-119">Graceful shutdown</span></span>
* [<span data-ttu-id="aac85-120">Jak toocreate fronty zpráv při zpracování zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="aac85-120">How toocreate a queue message while processing a queue message</span></span>](#createqueue)
  * <span data-ttu-id="aac85-121">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="aac85-121">String queue messages</span></span>
  * <span data-ttu-id="aac85-122">Zprávy fronty objektů POCO</span><span class="sxs-lookup"><span data-stu-id="aac85-122">POCO queue messages</span></span>
  * <span data-ttu-id="aac85-123">Vytvoření více zpráv nebo asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="aac85-123">Create multiple messages or in async functions</span></span>
  * <span data-ttu-id="aac85-124">Typy hello fronty atribut pracuje s</span><span class="sxs-lookup"><span data-stu-id="aac85-124">Types hello Queue attribute works with</span></span>
  * <span data-ttu-id="aac85-125">Použití atributů WebJobs SDK v hello tělo funkce</span><span class="sxs-lookup"><span data-stu-id="aac85-125">Use WebJobs SDK attributes in hello body of a function</span></span>
* [<span data-ttu-id="aac85-126">Jak objekty při zpracování zprávy fronty BLOB tooread a zápis</span><span class="sxs-lookup"><span data-stu-id="aac85-126">How tooread and write blobs while processing a queue message</span></span>](#blobs)
  * <span data-ttu-id="aac85-127">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="aac85-127">String queue messages</span></span>
  * <span data-ttu-id="aac85-128">Zprávy fronty objektů POCO</span><span class="sxs-lookup"><span data-stu-id="aac85-128">POCO queue messages</span></span>
  * <span data-ttu-id="aac85-129">Typy hello Blob atribut pracuje s</span><span class="sxs-lookup"><span data-stu-id="aac85-129">Types hello Blob attribute works with</span></span>
* [<span data-ttu-id="aac85-130">Jak toohandle škodlivých zpráv</span><span class="sxs-lookup"><span data-stu-id="aac85-130">How toohandle poison messages</span></span>](#poison)
  * <span data-ttu-id="aac85-131">Zpracování automatické poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="aac85-131">Automatic poison message handling</span></span>
  * <span data-ttu-id="aac85-132">Zpracování ručních poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="aac85-132">Manual poison message handling</span></span>
* [<span data-ttu-id="aac85-133">Jak tooset možnosti konfigurace</span><span class="sxs-lookup"><span data-stu-id="aac85-133">How tooset configuration options</span></span>](#config)
  * <span data-ttu-id="aac85-134">Sada SDK připojovacích řetězců v kódu</span><span class="sxs-lookup"><span data-stu-id="aac85-134">Set SDK connection strings in code</span></span>
  * <span data-ttu-id="aac85-135">Konfigurace nastavení QueueTrigger</span><span class="sxs-lookup"><span data-stu-id="aac85-135">Configure QueueTrigger settings</span></span>
  * <span data-ttu-id="aac85-136">Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu</span><span class="sxs-lookup"><span data-stu-id="aac85-136">Set values for WebJobs SDK constructor parameters in code</span></span>
* [<span data-ttu-id="aac85-137">Jak tootrigger funkce ručně</span><span class="sxs-lookup"><span data-stu-id="aac85-137">How tootrigger a function manually</span></span>](#manual)
* [<span data-ttu-id="aac85-138">Jak toowrite protokoly</span><span class="sxs-lookup"><span data-stu-id="aac85-138">How toowrite logs</span></span>](#logs)
* [<span data-ttu-id="aac85-139">Jak toohandle chyby a nakonfigurujte časové limity</span><span class="sxs-lookup"><span data-stu-id="aac85-139">How toohandle errors and configure timeouts</span></span>](#errors)
* [<span data-ttu-id="aac85-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aac85-140">Next steps</span></span>](#nextsteps)

## <span data-ttu-id="aac85-141"><a id="trigger"></a>Jak tootrigger funkce při příjmu zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="aac85-141"><a id="trigger"></a> How tootrigger a function when a queue message is received</span></span>
<span data-ttu-id="aac85-142">volá funkci, která hello WebJobs SDK toowrite při příjmu zprávy fronty, použijte hello `QueueTrigger` atribut.</span><span class="sxs-lookup"><span data-stu-id="aac85-142">toowrite a function that hello WebJobs SDK calls when a queue message is received, use hello `QueueTrigger` attribute.</span></span> <span data-ttu-id="aac85-143">konstruktoru atributu Hello přijímá řetězcový parametr, který určuje název hello toopoll fronty hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-143">hello attribute constructor takes a string parameter that specifies hello name of hello queue toopoll.</span></span> <span data-ttu-id="aac85-144">Můžete také [nastavit název fronty hello dynamicky](#config).</span><span class="sxs-lookup"><span data-stu-id="aac85-144">You can also [set hello queue name dynamically](#config).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="aac85-145">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="aac85-145">String queue messages</span></span>
<span data-ttu-id="aac85-146">V následujícím příkladu hello, fronty hello obsahuje řetězec zprávu, takže `QueueTrigger` je použité tooa řetězec parametr s názvem `logMessage` obsahující hello obsah zprávy fronty hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-146">In hello following example, hello queue contains a string message, so `QueueTrigger` is applied tooa string parameter named `logMessage` which contains hello content of hello queue message.</span></span> <span data-ttu-id="aac85-147">Hello funkce [zapíše protokolu zpráv toohello řídicí panel](#logs).</span><span class="sxs-lookup"><span data-stu-id="aac85-147">hello function [writes a log message toohello Dashboard](#logs).</span></span>

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

<span data-ttu-id="aac85-148">Kromě `string`, hello parametr může být bajtové pole, `CloudQueueMessage` objekt nebo objektů POCO, které definujete.</span><span class="sxs-lookup"><span data-stu-id="aac85-148">Besides `string`, hello parameter may be a byte array, a `CloudQueueMessage` object, or a POCO  that you define.</span></span>

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="aac85-149">Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="aac85-149">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="aac85-150">V následujícím příkladu hello, obsahuje zprávu fronty hello JSON pro `BlobInformation` objekt, který zahrnuje `BlobName` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="aac85-150">In hello following example, hello queue message contains JSON for a `BlobInformation` object which includes a `BlobName` property.</span></span> <span data-ttu-id="aac85-151">Hello SDK automaticky deserializuje objekt hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-151">hello SDK automatically deserializes hello object.</span></span>

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

<span data-ttu-id="aac85-152">Hello SDK používá hello [balíček Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize a deserializaci zprávy.</span><span class="sxs-lookup"><span data-stu-id="aac85-152">hello SDK uses hello [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize and deserialize messages.</span></span> <span data-ttu-id="aac85-153">Pokud vytvoříte fronty zpráv v aplikaci, která nepoužívá hello WebJobs SDK, můžete napsat kód jako následující příklad toocreate zprávu fronty objektů POCO hello této hello, které mohou analyzovat SDK.</span><span class="sxs-lookup"><span data-stu-id="aac85-153">If you create queue messages in a program that doesn't use hello WebJobs SDK, you can write code like hello following example toocreate a POCO queue message that hello SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a><span data-ttu-id="aac85-154">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="aac85-154">Async functions</span></span>
<span data-ttu-id="aac85-155">Následující funkce asynchronní Hello [zapíše protokolu toohello řídicí panel](#logs).</span><span class="sxs-lookup"><span data-stu-id="aac85-155">hello following async function [writes a log toohello Dashboard](#logs).</span></span>

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

<span data-ttu-id="aac85-156">Asynchronní funkce může trvat [token zrušení](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), jak ukazuje následující příklad, který kopíruje objekt blob hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-156">Async functions may take a [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), as shown in hello following example which copies a blob.</span></span> <span data-ttu-id="aac85-157">(Další informace o hello `queueTrigger` zástupného symbolu, najdete v části hello [objekty BLOB](#blobs) části.)</span><span class="sxs-lookup"><span data-stu-id="aac85-157">(For an explanation of hello `queueTrigger` placeholder, see hello [Blobs](#blobs) section.)</span></span>

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <span data-ttu-id="aac85-158"><a id="qtattributetypes"></a>Typy hello QueueTrigger atribut pracuje s</span><span class="sxs-lookup"><span data-stu-id="aac85-158"><a id="qtattributetypes"></a> Types hello QueueTrigger attribute works with</span></span>
<span data-ttu-id="aac85-159">Můžete použít `QueueTrigger` s hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="aac85-159">You can use `QueueTrigger` with hello following types:</span></span>

* `string`
* <span data-ttu-id="aac85-160">Typ objektů POCO serializovanou jako JSON</span><span class="sxs-lookup"><span data-stu-id="aac85-160">A POCO type serialized as JSON</span></span>
* `byte[]`
* `CloudQueueMessage`

### <span data-ttu-id="aac85-161"><a id="polling"></a>Algoritmus dotazování</span><span class="sxs-lookup"><span data-stu-id="aac85-161"><a id="polling"></a> Polling algorithm</span></span>
<span data-ttu-id="aac85-162">Hello SDK implementuje efekt náhodných exponenciální back vypnout algoritmus tooreduce hello nečinnosti-fronty dotazování na transakce náklady na úložiště.</span><span class="sxs-lookup"><span data-stu-id="aac85-162">hello SDK implements a random exponential back-off algorithm tooreduce hello effect of idle-queue polling on storage transaction costs.</span></span>  <span data-ttu-id="aac85-163">Když se najde zprávu, hello SDK vyčká dvou sekund a poté zkontroluje další zprávu; Pokud je nalezena žádná zpráva čeká před dalším pokusem o 4 sekundy.</span><span class="sxs-lookup"><span data-stu-id="aac85-163">When a message is found, hello SDK waits two seconds and then checks for another message; when no message is found it waits about four seconds before trying again.</span></span> <span data-ttu-id="aac85-164">Po následné neúspěšných pokusů tooget zprávu fronty, doba čekání hello pokračovat tooincrease, dokud nebude dosaženo hello maximální doba čekání, které minutu tooone výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="aac85-164">After subsequent failed attempts tooget a queue message, hello wait time continues tooincrease until it reaches hello maximum wait time, which defaults tooone minute.</span></span> <span data-ttu-id="aac85-165">[Hello maximální doba čekání je možné konfigurovat](#config).</span><span class="sxs-lookup"><span data-stu-id="aac85-165">[hello maximum wait time is configurable](#config).</span></span>

### <span data-ttu-id="aac85-166"><a id="instances"></a>Více instancí</span><span class="sxs-lookup"><span data-stu-id="aac85-166"><a id="instances"></a> Multiple instances</span></span>
<span data-ttu-id="aac85-167">Pokud vaše webová aplikace běží na několik instancí, nepřetržitá webová úloha běží na každý počítač, a každý počítač bude čekat na aktivační události a toorun funkce.</span><span class="sxs-lookup"><span data-stu-id="aac85-167">If your web app runs on multiple instances, a continuous WebJob runs on each machine, and each machine will wait for triggers and attempt toorun functions.</span></span> <span data-ttu-id="aac85-168">Hello aktivace fronty WebJobs SDK automaticky zabrání funkci zpracování zpráv fronty vícekrát; Funkce nemají toobe zapsána toobe idempotent.</span><span class="sxs-lookup"><span data-stu-id="aac85-168">hello WebJobs SDK queue trigger automatically prevents a function from processing a queue message multiple times; functions do not have toobe written toobe idempotent.</span></span> <span data-ttu-id="aac85-169">Ale pokud chcete, aby tooensure že pouze jedna instance funkce spustí i v případě, že existuje více instancí hello hostitele webové aplikace, můžete použít hello `Singleton` atribut.</span><span class="sxs-lookup"><span data-stu-id="aac85-169">However, if you want tooensure that only one instance of a function runs even when there are multiple instances of hello host web app, you can use hello `Singleton` attribute.</span></span>

### <span data-ttu-id="aac85-170"><a id="parallel"></a>Paralelní provádění</span><span class="sxs-lookup"><span data-stu-id="aac85-170"><a id="parallel"></a> Parallel execution</span></span>
<span data-ttu-id="aac85-171">Pokud máte víc funkcí naslouchá na různých front, hello SDK je zavolá paralelně přijetí zpráv současně.</span><span class="sxs-lookup"><span data-stu-id="aac85-171">If you have multiple functions listening on different queues, hello SDK will call them in parallel when messages are received simultaneously.</span></span>

<span data-ttu-id="aac85-172">Hello totéž platí při přijetí více zpráv pro jednu frontu.</span><span class="sxs-lookup"><span data-stu-id="aac85-172">hello same is true when multiple messages are received for a single queue.</span></span> <span data-ttu-id="aac85-173">Ve výchozím nastavení hello SDK získá dávku 16 fronty zpráv v čase a provede hello funkce, která je zpracuje paralelně.</span><span class="sxs-lookup"><span data-stu-id="aac85-173">By default, hello SDK gets a batch of 16 queue messages at a time and executes hello function that processes them in parallel.</span></span> <span data-ttu-id="aac85-174">[velikost dávky Hello je možné konfigurovat](#config).</span><span class="sxs-lookup"><span data-stu-id="aac85-174">[hello batch size is configurable](#config).</span></span> <span data-ttu-id="aac85-175">Když hello počet zpracovávaných získá dolů toohalf hello velikost dávky, hello SDK získá další dávku a spustí zpracování těchto zpráv.</span><span class="sxs-lookup"><span data-stu-id="aac85-175">When hello number being processed gets down toohalf of hello batch size, hello SDK gets another batch and starts processing those messages.</span></span> <span data-ttu-id="aac85-176">Maximální počet souběžných zpráv, které jsou zpracovány za funkce hello je proto velikost dávky hello jeden a půl krát.</span><span class="sxs-lookup"><span data-stu-id="aac85-176">Therefore hello maximum number of concurrent messages being processed per function is one and a half times hello batch size.</span></span> <span data-ttu-id="aac85-177">Toto omezení se vztahuje samostatně tooeach funkce, který má `QueueTrigger` atribut.</span><span class="sxs-lookup"><span data-stu-id="aac85-177">This limit applies separately tooeach function that has a `QueueTrigger` attribute.</span></span>

<span data-ttu-id="aac85-178">Pokud nechcete, aby paralelní zpracování zprávy přijaté v jedné frontě, můžete nastavit too1 velikost dávky hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-178">If you don't want parallel execution for messages received on one queue, you can set hello batch size too1.</span></span> <span data-ttu-id="aac85-179">Viz také **větší kontrolu nad fronty zpracování** v [RTM Azure WebJobs SDK 1.1.0](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).</span><span class="sxs-lookup"><span data-stu-id="aac85-179">See also **More control over Queue processing** in [Azure WebJobs SDK 1.1.0 RTM](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).</span></span>

### <span data-ttu-id="aac85-180"><a id="queuemetadata"></a>Získání fronty nebo fronty zpráv metadat</span><span class="sxs-lookup"><span data-stu-id="aac85-180"><a id="queuemetadata"></a>Get queue or queue message metadata</span></span>
<span data-ttu-id="aac85-181">Můžete získat hello následující vlastnosti zprávy přidáním podpis metody toohello parametry:</span><span class="sxs-lookup"><span data-stu-id="aac85-181">You can get hello following message properties by adding parameters toohello method signature:</span></span>

* <span data-ttu-id="aac85-182">`DateTimeOffset`expirationTime</span><span class="sxs-lookup"><span data-stu-id="aac85-182">`DateTimeOffset` expirationTime</span></span>
* <span data-ttu-id="aac85-183">`DateTimeOffset`insertionTime</span><span class="sxs-lookup"><span data-stu-id="aac85-183">`DateTimeOffset` insertionTime</span></span>
* <span data-ttu-id="aac85-184">`DateTimeOffset`nextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="aac85-184">`DateTimeOffset` nextVisibleTime</span></span>
* <span data-ttu-id="aac85-185">`string`queueTrigger (obsahuje text zprávy)</span><span class="sxs-lookup"><span data-stu-id="aac85-185">`string` queueTrigger (contains message text)</span></span>
* <span data-ttu-id="aac85-186">`string`ID</span><span class="sxs-lookup"><span data-stu-id="aac85-186">`string` id</span></span>
* <span data-ttu-id="aac85-187">`string`Vlastnosti popReceipt</span><span class="sxs-lookup"><span data-stu-id="aac85-187">`string` popReceipt</span></span>
* <span data-ttu-id="aac85-188">`int`dequeueCount</span><span class="sxs-lookup"><span data-stu-id="aac85-188">`int` dequeueCount</span></span>

<span data-ttu-id="aac85-189">Pokud chcete toowork přímo s hello úložiště Azure API, můžete také přidat `CloudStorageAccount` parametr.</span><span class="sxs-lookup"><span data-stu-id="aac85-189">If you want toowork directly with hello Azure storage API, you can also add a `CloudStorageAccount` parameter.</span></span>

<span data-ttu-id="aac85-190">Hello následující příklad zapíše všechny tento protokol metadata tooan informace o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aac85-190">hello following example writes all of this metadata tooan INFO application log.</span></span> <span data-ttu-id="aac85-191">V příkladu hello logMessage a queueTrigger obsahovat hello obsah zprávy fronty hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-191">In hello example, both logMessage and queueTrigger contain hello content of hello queue message.</span></span>

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

<span data-ttu-id="aac85-192">Tady je příklad protokolu o zapsaných správcem hello ukázkový kód:</span><span class="sxs-lookup"><span data-stu-id="aac85-192">Here is a sample log written by hello sample code:</span></span>

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

### <span data-ttu-id="aac85-193"><a id="graceful"></a>Řádné vypnutí</span><span class="sxs-lookup"><span data-stu-id="aac85-193"><a id="graceful"></a>Graceful shutdown</span></span>
<span data-ttu-id="aac85-194">Funkci, která běží v nepřetržitá webová úloha může přijmout `CancellationToken` parametr, který umožňuje hello operačního systému toonotify hello funkce při hello webové úlohy je o toobe byla ukončena.</span><span class="sxs-lookup"><span data-stu-id="aac85-194">A function that runs in a continuous WebJob can accept a `CancellationToken` parameter which enables hello operating system toonotify hello function when hello WebJob is about toobe terminated.</span></span> <span data-ttu-id="aac85-195">Můžete vytvořit toto oznámení toomake se, že funkce hello není neočekávaně ukončí tak, aby data ponechá v nekonzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="aac85-195">You can use this notification toomake sure hello function doesn't terminate unexpectedly in a way that leaves data in an inconsistent state.</span></span>

<span data-ttu-id="aac85-196">Následující příklad ukazuje, jak Hello toocheck pro předcházení ukončení webové úlohy ve funkci.</span><span class="sxs-lookup"><span data-stu-id="aac85-196">hello following example shows how toocheck for impending WebJob termination in a function.</span></span>

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

<span data-ttu-id="aac85-197">**Poznámka:** hello řídicí panel nemusí zobrazit správně hello stav a výstup funkcí, které byly vypnuty.</span><span class="sxs-lookup"><span data-stu-id="aac85-197">**Note:** hello Dashboard might not correctly show hello status and output of functions that have been shut down.</span></span>

<span data-ttu-id="aac85-198">Další informace najdete v tématu [WebJobs řádné vypnutí](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span><span class="sxs-lookup"><span data-stu-id="aac85-198">For more information, see [WebJobs Graceful Shutdown](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span></span>   

## <span data-ttu-id="aac85-199"><a id="createqueue"></a>Jak toocreate fronty zpráv při zpracování zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="aac85-199"><a id="createqueue"></a> How toocreate a queue message while processing a queue message</span></span>
<span data-ttu-id="aac85-200">toowrite funkci, která vytvoří novou zprávu fronty, použijte hello `Queue` atribut.</span><span class="sxs-lookup"><span data-stu-id="aac85-200">toowrite a function that creates a new queue message, use hello `Queue` attribute.</span></span> <span data-ttu-id="aac85-201">Jako `QueueTrigger`, kterou předáte název fronty hello jako řetězec, nebo můžete [nastavit název fronty hello dynamicky](#config).</span><span class="sxs-lookup"><span data-stu-id="aac85-201">Like `QueueTrigger`, you pass in hello queue name as a string or you can [set hello queue name dynamically](#config).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="aac85-202">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="aac85-202">String queue messages</span></span>
<span data-ttu-id="aac85-203">Následující ukázka kódu bez asynchronní Hello vytvoří novou zprávu fronty ve frontě hello s názvem "outputqueue" se stejný obsah jako zprávu fronty hello dostali hello frontu s názvem "inputqueue" hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-203">hello following non-async code sample creates a new queue message in hello queue named "outputqueue" with hello same content as hello queue message received in hello queue named "inputqueue".</span></span> <span data-ttu-id="aac85-204">(Pro asynchronní použít funkce `IAsyncCollector<T>` znázorněné později v této části.)</span><span class="sxs-lookup"><span data-stu-id="aac85-204">(For async functions use `IAsyncCollector<T>` as shown later in this section.)</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="aac85-205">Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="aac85-205">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="aac85-206">toocreate zprávu fronty, která obsahuje objektů POCO spíše než řetězec průchodu hello objektů POCO, zadejte jako parametr toohello výstupu `Queue` konstruktoru atributu.</span><span class="sxs-lookup"><span data-stu-id="aac85-206">toocreate a queue message that contains a POCO rather than a string, pass hello POCO type as an output parameter toohello `Queue` attribute constructor.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

<span data-ttu-id="aac85-207">Hello SDK automaticky serializuje tooJSON objekt hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-207">hello SDK automatically serializes hello object tooJSON.</span></span> <span data-ttu-id="aac85-208">Zprávu fronty je vytvořena vždy, i když hello objekt má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="aac85-208">A queue message is always created, even if hello object is null.</span></span>

### <a name="create-multiple-messages-or-in-async-functions"></a><span data-ttu-id="aac85-209">Vytvoření více zpráv nebo asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="aac85-209">Create multiple messages or in async functions</span></span>
<span data-ttu-id="aac85-210">toocreate více zpráv, zkontrolujte typ parametru hello pro hello výstupní fronty `ICollector<T>` nebo `IAsyncCollector<T>`, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-210">toocreate multiple messages, make hello parameter type for hello output queue `ICollector<T>` or `IAsyncCollector<T>`, as shown in hello following example.</span></span>

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="aac85-211">Každou zprávu fronty se vytvoří okamžitě při hello `Add` metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="aac85-211">Each queue message is created immediately when hello `Add` method is called.</span></span>

### <a name="types-that-hello-queue-attribute-works-with"></a><span data-ttu-id="aac85-212">Typy tento atribut fronty hello funguje s</span><span class="sxs-lookup"><span data-stu-id="aac85-212">Types that hello Queue attribute works with</span></span>
<span data-ttu-id="aac85-213">Můžete použít hello `Queue` atribut hello následující typy parametrů:</span><span class="sxs-lookup"><span data-stu-id="aac85-213">You can use hello `Queue` attribute on hello following parameter types:</span></span>

* <span data-ttu-id="aac85-214">`out string`(Pokud je hodnota parametru jinou hodnotu než null při ukončení hello funkce vytvoří zprávu fronty)</span><span class="sxs-lookup"><span data-stu-id="aac85-214">`out string` (creates queue message if parameter value is non-null when hello function ends)</span></span>
* <span data-ttu-id="aac85-215">`out byte[]`(funguje jako `string`)</span><span class="sxs-lookup"><span data-stu-id="aac85-215">`out byte[]` (works like `string`)</span></span>
* <span data-ttu-id="aac85-216">`out CloudQueueMessage`(funguje jako `string`)</span><span class="sxs-lookup"><span data-stu-id="aac85-216">`out CloudQueueMessage` (works like `string`)</span></span>
* <span data-ttu-id="aac85-217">`out POCO`(typu serializable vytvoří zprávu s objektem hodnotu null. Pokud hello parametru má hodnotu null při ukončení hello funkce)</span><span class="sxs-lookup"><span data-stu-id="aac85-217">`out POCO` (a serializable type, creates a message with a null object if hello paramter is null when hello function ends)</span></span>
* `ICollector`
* `IAsyncCollector`
* <span data-ttu-id="aac85-218">`CloudQueue`(pro vytváření zpráv ručně přímo pomocí hello API pro Azure Storage)</span><span class="sxs-lookup"><span data-stu-id="aac85-218">`CloudQueue` (for creating messages manually using hello Azure Storage API directly)</span></span>

### <span data-ttu-id="aac85-219"><a id="ibinder"></a>Použití atributů WebJobs SDK v hello tělo funkce</span><span class="sxs-lookup"><span data-stu-id="aac85-219"><a id="ibinder"></a>Use WebJobs SDK attributes in hello body of a function</span></span>
<span data-ttu-id="aac85-220">Pokud potřebujete toodo některé fungovat ve vaší funkci před použitím atribut WebJobs SDK, jako `Queue`, `Blob`, nebo `Table`, můžete použít hello `IBinder` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="aac85-220">If you need toodo some work in your function before using a WebJobs SDK attribute such as `Queue`, `Blob`, or `Table`, you can use hello `IBinder` interface.</span></span>

<span data-ttu-id="aac85-221">Následující ukázka Hello přebírá zprávu vstupní fronty a vytvoří novou zprávu s hello stejný obsah v výstupní fronty.</span><span class="sxs-lookup"><span data-stu-id="aac85-221">hello following example takes an input queue message and creates a new message with hello same content in an output queue.</span></span> <span data-ttu-id="aac85-222">Název fronty výstup Hello je nastavena podle kódu v textu hello hello funkce.</span><span class="sxs-lookup"><span data-stu-id="aac85-222">hello output queue name is set by code in hello body of hello function.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

<span data-ttu-id="aac85-223">Hello `IBinder` rozhraní lze také s hello `Table` a `Blob` atributy.</span><span class="sxs-lookup"><span data-stu-id="aac85-223">hello `IBinder` interface can also be used with hello `Table` and `Blob` attributes.</span></span>

## <span data-ttu-id="aac85-224"><a id="blobs"></a>Jak objekty BLOB tooread a zápis a tabulky při zpracování zprávy fronty</span><span class="sxs-lookup"><span data-stu-id="aac85-224"><a id="blobs"></a> How tooread and write blobs and tables while processing a queue message</span></span>
<span data-ttu-id="aac85-225">Hello `Blob` a `Table` atributů umožňují tooread a zapisovat objekty BLOB a tabulek.</span><span class="sxs-lookup"><span data-stu-id="aac85-225">hello `Blob` and `Table` attributes enable you tooread and write blobs and tables.</span></span> <span data-ttu-id="aac85-226">Ukázky Hello v této části platí tooblobs.</span><span class="sxs-lookup"><span data-stu-id="aac85-226">hello samples in this section apply tooblobs.</span></span> <span data-ttu-id="aac85-227">Ukázky kódu, které ukazují, jak tootrigger zpracovává, když se objekty BLOB jsou vytvořeny nebo aktualizovány, najdete v části [jak toouse Azure blob storage s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)a ukázky kódu, které pro čtení a zápis tabulky, najdete v části [jak toouse tabulky Azure úložiště s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="aac85-227">For code samples that show how tootrigger processes when blobs are created or updated, see [How toouse Azure blob storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), and for code samples that read and write tables, see [How toouse Azure table storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span></span>

### <a name="string-queue-messages-triggering-blob-operations"></a><span data-ttu-id="aac85-228">Zprávy fronty řetězec spuštění operace objektů blob</span><span class="sxs-lookup"><span data-stu-id="aac85-228">String queue messages triggering blob operations</span></span>
<span data-ttu-id="aac85-229">Pro zprávu fronty, který obsahuje řetězec `queueTrigger` je zástupný symbol můžete použít v hello `Blob` atributu `blobPath` parametr, který obsahuje obsah hello uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="aac85-229">For a queue message that contains a string, `queueTrigger` is a placeholder you can use in hello `Blob` attribute's `blobPath` parameter that contains hello contents of hello message.</span></span>

<span data-ttu-id="aac85-230">Hello následující příklad používá `Stream` objekty tooread a zápisu objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="aac85-230">hello following example uses `Stream` objects tooread and write blobs.</span></span> <span data-ttu-id="aac85-231">uvítací zprávu fronty je hello název objektu blob umístěna v kontejneru textblobs hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-231">hello queue message is hello name of a blob located in hello textblobs container.</span></span> <span data-ttu-id="aac85-232">Kopie objektu blob hello s "-nové" připojením toohello jméno se vytvoří v hello stejné kontejneru.</span><span class="sxs-lookup"><span data-stu-id="aac85-232">A copy of hello blob with "-new" appended toohello name is created in hello same container.</span></span>

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="aac85-233">Hello `Blob` atributu konstruktoru trvá `blobPath` parametr, který určuje hello kontejneru a název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="aac85-233">hello `Blob` attribute constructor takes a `blobPath` parameter that specifies hello container and blob name.</span></span> <span data-ttu-id="aac85-234">Další informace o tento zástupný text najdete v tématu [jak toouse Azure blob storage s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),</span><span class="sxs-lookup"><span data-stu-id="aac85-234">For more information about this placeholder, see [How toouse Azure blob storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),</span></span>

<span data-ttu-id="aac85-235">Pokud atribut hello upraví `Stream` objektu jiný parametr konstruktoru určuje hello `FileAccess` režimu pro čtení, zápisu nebo čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="aac85-235">When hello attribute decorates a `Stream` object, another constructor parameter specifies hello `FileAccess` mode as read, write, or read/write.</span></span>

<span data-ttu-id="aac85-236">Hello následující příklad používá `CloudBlockBlob` objektu toodelete objekt blob.</span><span class="sxs-lookup"><span data-stu-id="aac85-236">hello following example uses a `CloudBlockBlob` object toodelete a blob.</span></span> <span data-ttu-id="aac85-237">uvítací zprávu fronty je hello název objektu hello blob.</span><span class="sxs-lookup"><span data-stu-id="aac85-237">hello queue message is hello name of hello blob.</span></span>

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <span data-ttu-id="aac85-238"><a id="pocoblobs"></a>Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="aac85-238"><a id="pocoblobs"></a> POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="aac85-239">Pro objektů POCO uloží jako dokumenty JSON v uvítací zprávu fronty, můžete použít zástupné znaky, které název vlastnosti objektu hello v hello `Queue` atributu `blobPath` parametr.</span><span class="sxs-lookup"><span data-stu-id="aac85-239">For a POCO stored as JSON in hello queue message, you can use placeholders that name properties of hello object in hello `Queue` attribute's `blobPath` parameter.</span></span> <span data-ttu-id="aac85-240">Můžete také použít [fronty názvy vlastností metadat](#queuemetadata) zástupnými symboly.</span><span class="sxs-lookup"><span data-stu-id="aac85-240">You can also use [queue metadata property names](#queuemetadata) as placeholders.</span></span>

<span data-ttu-id="aac85-241">Hello následujícím příkladu se zkopíruje objekt blob tooa nové blob se jiné rozšíření.</span><span class="sxs-lookup"><span data-stu-id="aac85-241">hello following example copies a blob tooa new blob with a different extension.</span></span> <span data-ttu-id="aac85-242">zpráva fronty Hello `BlobInformation` objekt, který zahrnuje `BlobName` a `BlobNameWithoutExtension` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="aac85-242">hello queue message is a `BlobInformation` object that includes `BlobName` and `BlobNameWithoutExtension` properties.</span></span> <span data-ttu-id="aac85-243">názvy vlastností Hello jsou použity jako zástupné symboly v cestě objektu blob hello pro hello `Blob` atributy.</span><span class="sxs-lookup"><span data-stu-id="aac85-243">hello property names are used as placeholders in hello blob path for hello `Blob` attributes.</span></span>

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="aac85-244">Hello SDK používá hello [balíček Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize a deserializaci zprávy.</span><span class="sxs-lookup"><span data-stu-id="aac85-244">hello SDK uses hello [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize and deserialize messages.</span></span> <span data-ttu-id="aac85-245">Pokud vytvoříte fronty zpráv v aplikaci, která nepoužívá hello WebJobs SDK, můžete napsat kód jako následující příklad toocreate zprávu fronty objektů POCO hello této hello, které mohou analyzovat SDK.</span><span class="sxs-lookup"><span data-stu-id="aac85-245">If you create queue messages in a program that doesn't use hello WebJobs SDK, you can write code like hello following example toocreate a POCO queue message that hello SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

<span data-ttu-id="aac85-246">Pokud potřebujete toodo některé fungovat ve vaší funkci před vazby tooan objekt blob, můžete použít atribut hello v textu hello hello funkce [jak je uvedeno výše pro atribut fronty hello](#ibinder).</span><span class="sxs-lookup"><span data-stu-id="aac85-246">If you need toodo some work in your function before binding a blob tooan object, you can use hello attribute in hello body of hello function, [as shown earlier for hello Queue attribute](#ibinder).</span></span>

### <span data-ttu-id="aac85-247"><a id="blobattributetypes"></a>Můžete použít hello typy objektů Blob atribut s</span><span class="sxs-lookup"><span data-stu-id="aac85-247"><a id="blobattributetypes"></a> Types you can use hello Blob attribute with</span></span>
<span data-ttu-id="aac85-248">Hello `Blob` atribut lze použít s hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="aac85-248">hello `Blob` attribute can be used with hello following types:</span></span>

* <span data-ttu-id="aac85-249">`Stream`(pro čtení nebo zápisu, zadat pomocí parametru konstruktoru FileAccess hello)</span><span class="sxs-lookup"><span data-stu-id="aac85-249">`Stream` (read or write, specified by using hello FileAccess constructor parameter)</span></span>
* `TextReader`
* `TextWriter`
* <span data-ttu-id="aac85-250">`string`(pro čtení)</span><span class="sxs-lookup"><span data-stu-id="aac85-250">`string` (read)</span></span>
* <span data-ttu-id="aac85-251">`out string`(zapisovat; vytvoří objekt blob jenom v případě, že parametr řetězce hello má jinou hodnotu než null při návratu hello funkce)</span><span class="sxs-lookup"><span data-stu-id="aac85-251">`out string` (write; creates a blob only if hello string parameter is non-null when hello function returns)</span></span>
* <span data-ttu-id="aac85-252">Objektů POCO (čtení)</span><span class="sxs-lookup"><span data-stu-id="aac85-252">POCO (read)</span></span>
* <span data-ttu-id="aac85-253">out objektů POCO (zápisu; vždycky vytvoří objekt blob, vytvoří jako objektu null, pokud parametr objektů POCO má hodnotu null, když se vrátí hello funkce)</span><span class="sxs-lookup"><span data-stu-id="aac85-253">out POCO (write; always creates a blob, creates as null object if POCO parameter is null when hello function returns)</span></span>
* <span data-ttu-id="aac85-254">`CloudBlobStream`(zápisu)</span><span class="sxs-lookup"><span data-stu-id="aac85-254">`CloudBlobStream` (write)</span></span>
* <span data-ttu-id="aac85-255">`ICloudBlob`(pro čtení nebo zápisu)</span><span class="sxs-lookup"><span data-stu-id="aac85-255">`ICloudBlob` (read or write)</span></span>
* <span data-ttu-id="aac85-256">`CloudBlockBlob`(pro čtení nebo zápisu)</span><span class="sxs-lookup"><span data-stu-id="aac85-256">`CloudBlockBlob` (read or write)</span></span>
* <span data-ttu-id="aac85-257">`CloudPageBlob`(pro čtení nebo zápisu)</span><span class="sxs-lookup"><span data-stu-id="aac85-257">`CloudPageBlob` (read or write)</span></span>

## <span data-ttu-id="aac85-258"><a id="poison"></a>Jak toohandle škodlivých zpráv</span><span class="sxs-lookup"><span data-stu-id="aac85-258"><a id="poison"></a> How toohandle poison messages</span></span>
<span data-ttu-id="aac85-259">Zprávy, jejichž obsah způsobí, že funkce toofail se nazývají *škodlivých zpráv*.</span><span class="sxs-lookup"><span data-stu-id="aac85-259">Messages whose content causes a function toofail are called *poison messages*.</span></span> <span data-ttu-id="aac85-260">Pokud funkce hello nezdaří, hello fronty zpráv není odstraněna a nakonec je převzata znovu, což způsobilo toobe cyklus hello opakuje.</span><span class="sxs-lookup"><span data-stu-id="aac85-260">When hello function fails, hello queue message is not deleted and eventually is picked up again, causing hello cycle toobe repeated.</span></span> <span data-ttu-id="aac85-261">Hello SDK můžete automaticky přerušení hello cyklus po omezený počet iterací, nebo můžete provést ručně.</span><span class="sxs-lookup"><span data-stu-id="aac85-261">hello SDK can automatically interrupt hello cycle after a limited number of iterations, or you can do it manually.</span></span>

### <a name="automatic-poison-message-handling"></a><span data-ttu-id="aac85-262">Zpracování automatické poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="aac85-262">Automatic poison message handling</span></span>
<span data-ttu-id="aac85-263">Hello SDK bude volat funkci až too5 časy tooprocess zprávu fronty.</span><span class="sxs-lookup"><span data-stu-id="aac85-263">hello SDK will call a function up too5 times tooprocess a queue message.</span></span> <span data-ttu-id="aac85-264">V případě selhání páté zkuste hello je uvítací zprávu přesunutý tooa poškozených fronty.</span><span class="sxs-lookup"><span data-stu-id="aac85-264">If hello fifth try fails, hello message is moved tooa poison queue.</span></span> <span data-ttu-id="aac85-265">[maximální počet opakovaných pokusů Hello je možné konfigurovat](#config).</span><span class="sxs-lookup"><span data-stu-id="aac85-265">[hello maximum number of retries is configurable](#config).</span></span>

<span data-ttu-id="aac85-266">Název fronty poškozených Hello *{originalqueuename}*-poškozených.</span><span class="sxs-lookup"><span data-stu-id="aac85-266">hello poison queue is named *{originalqueuename}*-poison.</span></span> <span data-ttu-id="aac85-267">Můžete napsat tooprocess funkce zprávy z fronty poškozených hello jejich protokolování nebo odeslání oznámení, že je potřeba ruční pozornost.</span><span class="sxs-lookup"><span data-stu-id="aac85-267">You can write a function tooprocess messages from hello poison queue by logging them or sending a notification that manual attention is needed.</span></span>

<span data-ttu-id="aac85-268">V následujícím příkladu hello hello `CopyBlob` funkce se nezdaří, pokud obsahuje zprávu fronty hello název objektu blob, který neexistuje.</span><span class="sxs-lookup"><span data-stu-id="aac85-268">In hello following example hello `CopyBlob` function will fail when a queue message contains hello name of a blob that doesn't exist.</span></span> <span data-ttu-id="aac85-269">Pokud k tomu dojde, hello zpráva bude přesunuta z hello copyblobqueue fronty toohello copyblobqueue poison fronty.</span><span class="sxs-lookup"><span data-stu-id="aac85-269">When that happens, hello message is moved from hello copyblobqueue queue toohello copyblobqueue-poison queue.</span></span> <span data-ttu-id="aac85-270">Hello `ProcessPoisonMessage` pak protokoly hello poškozená zpráva.</span><span class="sxs-lookup"><span data-stu-id="aac85-270">hello `ProcessPoisonMessage` then logs hello poison message.</span></span>

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

<span data-ttu-id="aac85-271">Hello následující obrázek ukazuje výstup konzoly z těchto funkcí při zpracování poškozená zpráva.</span><span class="sxs-lookup"><span data-stu-id="aac85-271">hello following illustration shows console output from these functions when a poison message is processed.</span></span>

![Výstup konzoly pro zpracování poškozených zpráv](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### <a name="manual-poison-message-handling"></a><span data-ttu-id="aac85-273">Zpracování ručních poškozených zpráv</span><span class="sxs-lookup"><span data-stu-id="aac85-273">Manual poison message handling</span></span>
<span data-ttu-id="aac85-274">Hello počet, kolikrát byla převzata zprávu pro zpracování můžete získat tak, že přidáte `int` parametr s názvem `dequeueCount` tooyour funkce.</span><span class="sxs-lookup"><span data-stu-id="aac85-274">You can get hello number of times a message has been picked up for processing by adding an `int` parameter named `dequeueCount` tooyour function.</span></span> <span data-ttu-id="aac85-275">Můžete pak zkontrolujte hello dequeue – počet v kódu funkce a provádět vlastní zpracování poškozených zpráv Pokud hello číslo překročí prahovou hodnotu, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-275">You can then check hello dequeue count in function code and perform your own poison message handling when hello number exceeds a threshold, as shown in hello following example.</span></span>

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

## <span data-ttu-id="aac85-276"><a id="config"></a>Jak tooset možnosti konfigurace</span><span class="sxs-lookup"><span data-stu-id="aac85-276"><a id="config"></a> How tooset configuration options</span></span>
<span data-ttu-id="aac85-277">Můžete použít hello `JobHostConfiguration` hello tooset typ následující možnosti konfigurace:</span><span class="sxs-lookup"><span data-stu-id="aac85-277">You can use hello `JobHostConfiguration` type tooset hello following configuration options:</span></span>

* <span data-ttu-id="aac85-278">Nastavit hello SDK připojovacích řetězců v kódu.</span><span class="sxs-lookup"><span data-stu-id="aac85-278">Set hello SDK connection strings in code.</span></span>
* <span data-ttu-id="aac85-279">Konfigurace `QueueTrigger` nastavení, jako například maximální počet vyřazení z fronty.</span><span class="sxs-lookup"><span data-stu-id="aac85-279">Configure `QueueTrigger` settings such as maximum dequeue count.</span></span>
* <span data-ttu-id="aac85-280">Získáte názvy fronty z konfigurace.</span><span class="sxs-lookup"><span data-stu-id="aac85-280">Get queue names from configuration.</span></span>

### <span data-ttu-id="aac85-281"><a id="setconnstr"></a>Sada SDK připojovacích řetězců v kódu</span><span class="sxs-lookup"><span data-stu-id="aac85-281"><a id="setconnstr"></a>Set SDK connection strings in code</span></span>
<span data-ttu-id="aac85-282">Nastavení hello SDK připojovacích řetězců v kódu umožňuje vám toouse vlastní názvy řetězec připojení v konfiguračních souborů nebo proměnné prostředí, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-282">Setting hello SDK connection strings in code enables you toouse your own connection string names in configuration files or environment variables, as shown in hello following example.</span></span>

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

### <span data-ttu-id="aac85-283"><a id="configqueue"></a>Konfigurace nastavení QueueTrigger</span><span class="sxs-lookup"><span data-stu-id="aac85-283"><a id="configqueue"></a>Configure QueueTrigger  settings</span></span>
<span data-ttu-id="aac85-284">Můžete nakonfigurovat následující nastavení, které se vztahují zpracování zprávy fronty toohello hello:</span><span class="sxs-lookup"><span data-stu-id="aac85-284">You can configure hello following settings that apply toohello queue message processing:</span></span>

* <span data-ttu-id="aac85-285">maximální počet zpráv fronty, které jsou zachyceny současně toobe paralelně Hello (výchozí hodnota je 16).</span><span class="sxs-lookup"><span data-stu-id="aac85-285">hello maximum number of queue messages that are picked up simultaneously toobe executed in parallel (default is 16).</span></span>
* <span data-ttu-id="aac85-286">Hello maximální počet opakovaných pokusů, než je odeslána zpráva fronty tooa poškozených fronty (výchozí hodnota je 5).</span><span class="sxs-lookup"><span data-stu-id="aac85-286">hello maximum number of retries before a queue message is sent tooa poison queue (default is 5).</span></span>
* <span data-ttu-id="aac85-287">Hello maximální doba čekání před znovu dotazování, když se fronta je prázdný (výchozí hodnota je 1 minuta).</span><span class="sxs-lookup"><span data-stu-id="aac85-287">hello maximum wait time before polling again when a queue is empty (default is 1 minute).</span></span>

<span data-ttu-id="aac85-288">Následující příklad ukazuje, jak Hello tooconfigure tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="aac85-288">hello following example shows how tooconfigure these settings:</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <span data-ttu-id="aac85-289"><a id="setnamesincode"></a>Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu</span><span class="sxs-lookup"><span data-stu-id="aac85-289"><a id="setnamesincode"></a>Set values for WebJobs SDK constructor parameters in code</span></span>
<span data-ttu-id="aac85-290">Občas můžete chtít toospecify název fronty, název objektu blob nebo kontejner, nebo tabulku název v kódu než pevný kódu.</span><span class="sxs-lookup"><span data-stu-id="aac85-290">Sometimes you want toospecify a queue name, a blob name or container, or a table name in code rather than hard-code it.</span></span> <span data-ttu-id="aac85-291">Například můžete název fronty hello toospecify `QueueTrigger` do proměnné prostředí nebo soubor konfigurace.</span><span class="sxs-lookup"><span data-stu-id="aac85-291">For example, you might want toospecify hello queue name for `QueueTrigger` in a configuration file or environment variable.</span></span>

<span data-ttu-id="aac85-292">Můžete to udělat pomocí předávání `NameResolver` objektu toohello `JobHostConfiguration` typu.</span><span class="sxs-lookup"><span data-stu-id="aac85-292">You can do that by passing in a `NameResolver` object toohello `JobHostConfiguration` type.</span></span> <span data-ttu-id="aac85-293">Zahrnout speciální zástupné symboly obklopená znaky procenta (%) v parametrech konstruktoru atributu WebJobs SDK a `NameResolver` kódu určuje toobe hello skutečné hodnoty, použijí se místo tyto zástupné symboly.</span><span class="sxs-lookup"><span data-stu-id="aac85-293">You include special placeholders surrounded by percent (%) signs in WebJobs SDK attribute constructor parameters, and your `NameResolver` code specifies hello actual values toobe used in place of those placeholders.</span></span>

<span data-ttu-id="aac85-294">Například předpokládejme, že chcete toouse frontu s názvem logqueuetest v testovacím prostředí hello a jednu s názvem logqueueprod v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="aac85-294">For example, suppose you want toouse a queue named logqueuetest in hello test environment and one named logqueueprod in production.</span></span> <span data-ttu-id="aac85-295">Místo názvu fronty pevně chcete toospecify hello název záznamu v hello `appSettings` kolekce, která by měla mít název skutečné fronty hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-295">Instead of a hard-coded queue name, you want toospecify hello name of an entry in hello `appSettings` collection that would have hello actual queue name.</span></span> <span data-ttu-id="aac85-296">Pokud hello `appSettings` klíč je logqueue, funkce by měl vypadat jako následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-296">If hello `appSettings` key is logqueue, your function could look like hello following example.</span></span>

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

<span data-ttu-id="aac85-297">Vaše `NameResolver` třídy, může získat název fronty hello z `appSettings` jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="aac85-297">Your `NameResolver` class could then get hello queue name from `appSettings` as shown in hello following example:</span></span>

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

<span data-ttu-id="aac85-298">Předat hello `NameResolver` třídy v toohello `JobHost` objektu, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-298">You pass hello `NameResolver` class in toohello `JobHost` object as shown in hello following example.</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

<span data-ttu-id="aac85-299">**Poznámka:** Queue, table a názvy objektů blob jsou vyřešeny pokaždé, když je volána funkce, ale jenom v případě, že spuštěním aplikace hello překladu názvů kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="aac85-299">**Note:** Queue, table, and blob names are resolved each time a function is called, but blob container names are resolved only when hello application starts.</span></span> <span data-ttu-id="aac85-300">Název kontejneru objektu blob nelze změnit, když je spuštěná úloha hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-300">You can't change blob container name while hello job is running.</span></span>

## <span data-ttu-id="aac85-301"><a id="manual"></a>Jak tootrigger funkce ručně</span><span class="sxs-lookup"><span data-stu-id="aac85-301"><a id="manual"></a>How tootrigger a function manually</span></span>
<span data-ttu-id="aac85-302">tootrigger funkce ručně, použijte hello `Call` nebo `CallAsync` metodu hello `JobHost` objekt a hello `NoAutomaticTrigger` atributu u hello funkce, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-302">tootrigger a function manually, use hello `Call` or `CallAsync` method on hello `JobHost` object and hello `NoAutomaticTrigger` attribute on hello function, as shown in hello following example.</span></span>

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

## <span data-ttu-id="aac85-303"><a id="logs"></a>Jak toowrite protokoly</span><span class="sxs-lookup"><span data-stu-id="aac85-303"><a id="logs"></a>How toowrite logs</span></span>
<span data-ttu-id="aac85-304">Hello řídicí panel zobrazuje protokoly na dvou místech: hello stránky pro webové úlohy hello a hello stránky pro konkrétní vyvolání webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="aac85-304">hello Dashboard shows logs in two places: hello page for hello WebJob, and hello page for a particular WebJob invocation.</span></span>

![Protokoly v webová stránka](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![Protokoly stránce volání funkce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

<span data-ttu-id="aac85-307">Výstup z konzoly metod, které volají ve funkci nebo v hello `Main()` metoda se zobrazí v hello stránka řídicího panelu pro hello webové úlohy, nikoli na stránku hello volání konkrétní metody.</span><span class="sxs-lookup"><span data-stu-id="aac85-307">Output from Console methods that you call in a function or in hello `Main()` method appears in hello Dashboard page for hello WebJob, not in hello page for a particular method invocation.</span></span> <span data-ttu-id="aac85-308">Výstup hello objekt TextWriter, kterou můžete získat z parametr ve vašem podpis metody se zobrazí v hello stránka řídicího panelu pro volání metody.</span><span class="sxs-lookup"><span data-stu-id="aac85-308">Output from hello TextWriter object that you get from a parameter in your method signature appears in hello Dashboard page for a method invocation.</span></span>

<span data-ttu-id="aac85-309">Výstup konzoly nemůže být volání konkrétní metody propojené tooa, protože hello konzoly je jedním podprocesem, zatímco mnoho funkcí úlohy může běžet v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="aac85-309">Console output can't be linked tooa particular method invocation because hello Console is single-threaded, while many job functions may be running at hello same time.</span></span> <span data-ttu-id="aac85-310">To je důvod, proč hello SDK poskytuje každé vyvolání funkce s objekt zapisovače svůj vlastní jedinečný protokolu.</span><span class="sxs-lookup"><span data-stu-id="aac85-310">That's why hello  SDK provides each function invocation with its own unique log writer object.</span></span>

<span data-ttu-id="aac85-311">toowrite [protokoly trasování aplikací](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), použijte `Console.Out` (vytvoří protokoly, které jsou označeny jako údaje) a `Console.Error` (vytvoří protokoly, které jsou označeny jako chyba).</span><span class="sxs-lookup"><span data-stu-id="aac85-311">toowrite [application tracing logs](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), use `Console.Out` (creates logs marked as INFO) and `Console.Error` (creates logs marked as ERROR).</span></span> <span data-ttu-id="aac85-312">Alternativou je toouse [trasování nebo TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), který poskytuje podrobné, upozornění, a kritická úrovně v přidání tooInfo a chyby.</span><span class="sxs-lookup"><span data-stu-id="aac85-312">An alternative is toouse [Trace or TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), which provides Verbose, Warning, and Critical levels in addition tooInfo and Error.</span></span> <span data-ttu-id="aac85-313">Protokoly trasování aplikací se zobrazí v souborech protokolů hello webové aplikace, tabulky Azure, nebo v závislosti na tom, jak nakonfigurujete vaší webové aplikace Azure objektů BLOB Azure.</span><span class="sxs-lookup"><span data-stu-id="aac85-313">Application tracing logs appear in hello web app log files, Azure tables, or Azure blobs depending on how you configure your Azure web app.</span></span> <span data-ttu-id="aac85-314">Jak platí všechny výstup konzoly hello nejnovější 100 aplikační protokoly se také zobrazí v hello stránka řídicího panelu pro hello webové úlohy, ne hello stránku pro volání funkce.</span><span class="sxs-lookup"><span data-stu-id="aac85-314">As is true of all Console output, hello most recent 100 application logs also appear in hello Dashboard page for hello WebJob, not hello page for a function invocation.</span></span>

<span data-ttu-id="aac85-315">Výstup konzoly se zobrazí v hello řídicí panel pouze v případě, že při spuštění programu hello v webové úlohy Azure, není-li hello program spuštěn místně nebo v některých dalších prostředí.</span><span class="sxs-lookup"><span data-stu-id="aac85-315">Console output appears in hello Dashboard only if hello program is running in an Azure WebJob, not if hello program is running locally or in some other environment.</span></span>

<span data-ttu-id="aac85-316">Zakázání protokolování řídicí panel pro scénáře vysoké propustnosti.</span><span class="sxs-lookup"><span data-stu-id="aac85-316">Disable dashboard logging for high throughput scenarios.</span></span> <span data-ttu-id="aac85-317">Ve výchozím nastavení hello SDK zapíše toostorage protokoly a tato aktivita může snížit výkon při zpracování velkého počtu zpráv.</span><span class="sxs-lookup"><span data-stu-id="aac85-317">By default, hello SDK writes logs toostorage, and this activity could degrade performance when you are processing many messages.</span></span> <span data-ttu-id="aac85-318">toodisable protokolování, nastavte hello řídicí panel připojovací řetězec toonull, jak je znázorněno v následující ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="aac85-318">toodisable logging, set hello dashboard connection string toonull as shown in hello following example.</span></span>

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = "";        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

<span data-ttu-id="aac85-319">Hello následující příklad znázorňuje několik způsobů toowrite protokoly:</span><span class="sxs-lookup"><span data-stu-id="aac85-319">hello following example shows several ways toowrite logs:</span></span>

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

<span data-ttu-id="aac85-320">V řídicím panelu WebJobs SDK hello, hello výstup hello `TextWriter` objektů, zobrazí se při návratu toohello stránky pro konkrétní funkce volání a klikněte na tlačítko **přepnutí výstup**:</span><span class="sxs-lookup"><span data-stu-id="aac85-320">In hello WebJobs SDK Dashboard, hello output from hello `TextWriter` object shows up when you go toohello page for a particular function invocation and click **Toggle Output**:</span></span>

![Klikněte na odkaz volání funkce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![Protokoly stránce volání funkce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

<span data-ttu-id="aac85-323">V řídicím panelu WebJobs SDK hello, hello nejnovější 100 řádků konzoly výstup zobrazit až když přejdete toohello stránku hello webové úlohy (není pro volání funkce hello) a klikněte na tlačítko **přepnutí výstup**.</span><span class="sxs-lookup"><span data-stu-id="aac85-323">In hello WebJobs SDK Dashboard, hello most recent 100 lines of Console output show up when you go toohello page for hello WebJob (not for hello function invocation) and click **Toggle Output**.</span></span>

![Klikněte na přepínač výstup](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

<span data-ttu-id="aac85-325">V nepřetržitá webová úloha, aplikační protokoly objeví v/data/úlohy/průběžné/*{webjobname}*/job_log.txt v systému souborů aplikace hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="aac85-325">In a continuous WebJob, application logs show up in /data/jobs/continuous/*{webjobname}*/job_log.txt in hello web app file system.</span></span>

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

<span data-ttu-id="aac85-326">V aplikaci Azure blob hello protokoly vypadat například takto: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello, world!, 2014-09-26T21:01:13, chyba, contosoadsnew, 491e54, 635473620738373502,0,17404,19,Console.Error - Hello, world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello, world!,</span><span class="sxs-lookup"><span data-stu-id="aac85-326">In an Azure blob hello application logs look like this: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span></span>

<span data-ttu-id="aac85-327">A v hello tabulky Azure `Console.Out` a `Console.Error` protokoly vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="aac85-327">And in an Azure table hello `Console.Out` and `Console.Error` logs look like this:</span></span>

![Informace o protokolu v tabulce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![Protokol chyb v tabulce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

<span data-ttu-id="aac85-330">Pokud chcete tooplug ve vlastní protokoly, najdete v části [v tomto příkladu](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="aac85-330">If you want tooplug in your own logger, see [this example](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).</span></span>

## <span data-ttu-id="aac85-331"><a id="errors"></a>Jak toohandle chyby a nakonfigurujte časové limity</span><span class="sxs-lookup"><span data-stu-id="aac85-331"><a id="errors"></a>How toohandle errors and configure timeouts</span></span>
<span data-ttu-id="aac85-332">Hello WebJobs SDK také zahrnuje [časový limit](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) atribut, který můžete použít toocause toobe funkce zruší, pokud se nedokončí ve stanoveném intervalu.</span><span class="sxs-lookup"><span data-stu-id="aac85-332">hello WebJobs SDK also includes a [Timeout](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) attribute that you can use toocause a function toobe canceled if doesn't complete within a specified amount of time.</span></span> <span data-ttu-id="aac85-333">A pokud chcete tooraise výstrahu, když příliš mnoho chyb dojít v zadaném časovém období, můžete použít hello `ErrorTrigger` atribut.</span><span class="sxs-lookup"><span data-stu-id="aac85-333">And if you want tooraise an alert when too many errors happen within a specified period of time, you can use hello `ErrorTrigger` attribute.</span></span> <span data-ttu-id="aac85-334">Tady je [ErrorTrigger příklad](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).</span><span class="sxs-lookup"><span data-stu-id="aac85-334">Here is an [ErrorTrigger example](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).</span></span>

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    too= "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors toohello Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

<span data-ttu-id="aac85-335">Můžete také dynamicky zakázat a povolit funkce toocontrol, zda se můžete spustit, a to pomocí konfigurace přepínače, které by mohly být nastavení aplikace nebo název proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="aac85-335">You can also dynamically disable and enable functions toocontrol whether they can be triggered, by using a configuration switch that could be an app setting or environment variable name.</span></span> <span data-ttu-id="aac85-336">Ukázkový kód, najdete v části hello `Disable` atribut [hello WebJobs SDK ukázky úložiště](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).</span><span class="sxs-lookup"><span data-stu-id="aac85-336">For sample code, see hello `Disable` attribute in [hello WebJobs SDK samples repository](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).</span></span>

## <span data-ttu-id="aac85-337"><a id="nextsteps"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="aac85-337"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="aac85-338">Tato příručka poskytl kódu ukázky, zobrazující jak toohandle běžné scénáře pro práci s Azure fronty.</span><span class="sxs-lookup"><span data-stu-id="aac85-338">This guide has provided code samples that show how toohandle common scenarios for working with Azure queues.</span></span> <span data-ttu-id="aac85-339">Další informace o tom, jak toouse Azure WebJobs a hello WebJobs SDK najdete v části [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="aac85-339">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>
