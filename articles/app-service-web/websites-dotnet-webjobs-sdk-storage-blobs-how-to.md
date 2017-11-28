---
title: "aaaHow toouse s hello WebJobs SDK služby Azure blob storage"
description: "Zjistěte, jak toouse Azure blob storage s hello WebJobs SDK. Spustit proces, jakmile se zobrazí nový objekt blob v kontejneru a zpracovat \"poškozených objekty BLOB\"."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: 
ms.assetid: bf32f919-f7bc-4aaa-916e-461c02f2e26c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: b34ea8cffee7c0475641886150dee521130a3132
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-blob-storage-with-hello-webjobs-sdk"></a><span data-ttu-id="bd190-104">Jak toouse Azure blob storage s hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="bd190-104">How toouse Azure blob storage with hello WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="bd190-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="bd190-105">Overview</span></span>
<span data-ttu-id="bd190-106">Tato příručka obsahuje C# kódu ukázky, zobrazující jak tootrigger proces při vytvoření nebo aktualizaci objektu blob Azure.</span><span class="sxs-lookup"><span data-stu-id="bd190-106">This guide provides C# code samples that show how tootrigger a process when an Azure blob is created or updated.</span></span> <span data-ttu-id="bd190-107">Ukázky kódu Hello použití [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verze 1.x.</span><span class="sxs-lookup"><span data-stu-id="bd190-107">hello code samples use [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="bd190-108">Ukázky kódu, které ukazují, jak objekty BLOB toocreate, najdete v části [jak toouse Azure fronty úložiště s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="bd190-108">For code samples that show how toocreate blobs, see [How toouse Azure queue storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="bd190-109">Hello Příručka předpokládá znáte [jak toocreate projekt webové úlohy v sadě Visual Studio se připojení řetězce daný účet úložiště bodu tooyour](websites-dotnet-webjobs-sdk-get-started.md) nebo příliš[více účtů úložiště](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="bd190-109">hello guide assumes you know [how toocreate a WebJob project in Visual Studio with connection strings that point tooyour storage account](websites-dotnet-webjobs-sdk-get-started.md) or too[multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

## <span data-ttu-id="bd190-110"><a id="trigger"></a>Jak tootrigger funkce při vytvoření nebo aktualizaci objektu blob</span><span class="sxs-lookup"><span data-stu-id="bd190-110"><a id="trigger"></a> How tootrigger a function when a blob is created or updated</span></span>
<span data-ttu-id="bd190-111">Tato část uvádí, jak toouse hello `BlobTrigger` atribut.</span><span class="sxs-lookup"><span data-stu-id="bd190-111">This section shows how toouse hello `BlobTrigger` attribute.</span></span> 

> [!NOTE]
> <span data-ttu-id="bd190-112">Hello WebJobs SDK kontroly protokolu soubory toowatch pro nové nebo změněné objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="bd190-112">hello WebJobs SDK scans log files toowatch for new or changed blobs.</span></span> <span data-ttu-id="bd190-113">Tento proces není v reálném čase; funkce nemusí získat aktivuje až několik minut nebo déle po vytvoření objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="bd190-113">This process is not real-time; a function might not get triggered until several minutes or longer after hello blob is created.</span></span> <span data-ttu-id="bd190-114">Kromě toho [protokol úložiště jsou vytvořené na "maximální úsilí"](https://msdn.microsoft.com/library/azure/hh343262.aspx) základ; není zaručeno, že bude možné zaznamenat všechny události.</span><span class="sxs-lookup"><span data-stu-id="bd190-114">In addition, [storage logs are created on a "best efforts"](https://msdn.microsoft.com/library/azure/hh343262.aspx) basis; there is no guarantee that all events will be captured.</span></span> <span data-ttu-id="bd190-115">Za určitých podmínek může být načteni protokoly.</span><span class="sxs-lookup"><span data-stu-id="bd190-115">Under some conditions, logs might be missed.</span></span> <span data-ttu-id="bd190-116">Pokud hello rychlost a spolehlivost omezení aktivační události objektu blob nejsou přijatelné pro vaši aplikaci, hello doporučená metoda je toocreate zprávu fronty, při vytváření objektu blob hello a použijte hello [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) atribut místo Hello `BlobTrigger` atribut hello funkce, která zpracovává hello objektů blob.</span><span class="sxs-lookup"><span data-stu-id="bd190-116">If hello speed and reliability limitations of blob triggers are not acceptable for your application, hello recommended method is toocreate a queue message when you create hello blob, and use hello [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribute instead of hello `BlobTrigger` attribute on hello function that processes hello blob.</span></span>
> 
> 

### <a name="single-placeholder-for-blob-name-with-extension"></a><span data-ttu-id="bd190-117">Jeden zástupný symbol pro název objektu blob s příponou</span><span class="sxs-lookup"><span data-stu-id="bd190-117">Single placeholder for blob name with extension</span></span>
<span data-ttu-id="bd190-118">Hello následující ukázka kódu zkopíruje text objekty BLOB, které se zobrazují v hello *vstupní* kontejneru toohello *výstup* kontejneru:</span><span class="sxs-lookup"><span data-stu-id="bd190-118">hello following code sample copies text blobs that appear in hello *input* container toohello *output* container:</span></span>

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="bd190-119">konstruktoru atributu Hello přijímá řetězcový parametr, který určuje název kontejneru hello a zástupný symbol pro název objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="bd190-119">hello attribute constructor takes a string parameter that specifies hello container name and a placeholder for hello blob name.</span></span> <span data-ttu-id="bd190-120">V tomto příkladu, pokud objekt blob s názvem *Blob1.txt* je vytvořen v hello *vstupní* kontejneru hello funkce vytvoří objekt blob s názvem *Blob1.txt* v hello *výstup*  kontejneru.</span><span class="sxs-lookup"><span data-stu-id="bd190-120">In this example, if a blob named *Blob1.txt* is created in hello *input* container, hello function creates a blob named *Blob1.txt* in hello *output* container.</span></span> 

<span data-ttu-id="bd190-121">Vzor názvů s hello zástupný symbol název objektu blob, můžete určit, jak je znázorněno v hello následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="bd190-121">You can specify a name pattern with hello blob name placeholder, as shown in hello following code sample:</span></span>

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="bd190-122">Tento kód zkopíruje pouze objekty BLOB, které mají názvy počínaje "původní-".</span><span class="sxs-lookup"><span data-stu-id="bd190-122">This code copies only blobs that have names beginning with "original-".</span></span> <span data-ttu-id="bd190-123">Například *původní Blob1.txt* v hello *vstupní* kontejneru se zkopíruje příliš*kopie Blob1.txt* v hello *výstup* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="bd190-123">For example, *original-Blob1.txt* in hello *input* container is copied too*copy-Blob1.txt* in hello *output* container.</span></span>

<span data-ttu-id="bd190-124">Pokud potřebujete toospecify vzor názvu pro názvy objektů blob, které mají v názvu hello složené závorky, dvakrát hello složené závorky.</span><span class="sxs-lookup"><span data-stu-id="bd190-124">If you need toospecify a name pattern for blob names that have curly braces in hello name, double hello curly braces.</span></span> <span data-ttu-id="bd190-125">Například, pokud chcete, aby toofind objektů BLOB v hello *bitové kopie* kontejner, který mají názvy takto:</span><span class="sxs-lookup"><span data-stu-id="bd190-125">For example, if you want toofind blobs in hello *images* container that have names like this:</span></span>

        {20140101}-soundfile.mp3

<span data-ttu-id="bd190-126">To lze používejte pro vaše vzor:</span><span class="sxs-lookup"><span data-stu-id="bd190-126">use this for your pattern:</span></span>

        images/{{20140101}}-{name}

<span data-ttu-id="bd190-127">V příkladu hello hello *název* hodnotu zástupného symbolu by *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="bd190-127">In hello example, hello *name* placeholder value would be *soundfile.mp3*.</span></span> 

### <a name="separate-blob-name-and-extension-placeholders"></a><span data-ttu-id="bd190-128">Zástupné symboly název a příponu samostatných objektů blob</span><span class="sxs-lookup"><span data-stu-id="bd190-128">Separate blob name and extension placeholders</span></span>
<span data-ttu-id="bd190-129">Hello následující změny ukázkový kód hello příponu souboru jako kopíruje objekty BLOB, které se zobrazují v hello *vstupní* kontejneru toohello *výstup* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="bd190-129">hello following code sample changes hello file extension as it copies blobs that appear in hello *input* container toohello *output* container.</span></span> <span data-ttu-id="bd190-130">Kód Hello protokoly hello rozšíření hello *vstupní* objektů blob a nastaví hello rozšíření hello *výstup* blob příliš*.txt*.</span><span class="sxs-lookup"><span data-stu-id="bd190-130">hello code logs hello extension of hello *input* blob and sets hello extension of hello *output* blob too*.txt*.</span></span>

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <span data-ttu-id="bd190-131"><a id="types"></a>Typy tooblobs můžete vytvořit vazbu</span><span class="sxs-lookup"><span data-stu-id="bd190-131"><a id="types"></a> Types that you can bind tooblobs</span></span>
<span data-ttu-id="bd190-132">Můžete použít hello `BlobTrigger` atribut hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="bd190-132">You can use hello `BlobTrigger` attribute on hello following types:</span></span>

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* <span data-ttu-id="bd190-133">Jiné typy deserializoval [ICloudBlobStreamBinder](#icbsb)</span><span class="sxs-lookup"><span data-stu-id="bd190-133">Other types deserialized by [ICloudBlobStreamBinder](#icbsb)</span></span> 

<span data-ttu-id="bd190-134">Pokud chcete toowork přímo s hello účtu úložiště Azure, můžete také přidat `CloudStorageAccount` podpis metody toohello parametr.</span><span class="sxs-lookup"><span data-stu-id="bd190-134">If you want toowork directly with hello Azure storage account, you can also add a `CloudStorageAccount` parameter toohello method signature.</span></span>

<span data-ttu-id="bd190-135">Příklady najdete v tématu hello [blob vazby kódu v úložišti azure. webjobs sdk hello na webu GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="bd190-135">For examples, see hello [blob binding code in hello azure-webjobs-sdk repository on GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).</span></span>

## <span data-ttu-id="bd190-136"><a id="string"></a>Získávání obsahu objektu blob text toostring vazby</span><span class="sxs-lookup"><span data-stu-id="bd190-136"><a id="string"></a> Getting text blob content by binding toostring</span></span>
<span data-ttu-id="bd190-137">Pokud se očekává text objekty BLOB, `BlobTrigger` může být použité tooa `string` parametr.</span><span class="sxs-lookup"><span data-stu-id="bd190-137">If text blobs are expected, `BlobTrigger` can be applied tooa `string` parameter.</span></span> <span data-ttu-id="bd190-138">Hello následující ukázka kódu váže tooa objektů blob text `string` parametr s názvem `logMessage`.</span><span class="sxs-lookup"><span data-stu-id="bd190-138">hello following code sample binds a text blob tooa `string` parameter named `logMessage`.</span></span> <span data-ttu-id="bd190-139">Funkce Hello používá tento parametr toowrite hello obsah objektu blob toohello hello řídicím panelu WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="bd190-139">hello function uses that parameter toowrite hello contents of hello blob toohello WebJobs SDK dashboard.</span></span> 

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <span data-ttu-id="bd190-140"><a id="icbsb"></a>Získávání serializovat obsah objektu blob pomocí ICloudBlobStreamBinder</span><span class="sxs-lookup"><span data-stu-id="bd190-140"><a id="icbsb"></a> Getting serialized blob content by using ICloudBlobStreamBinder</span></span>
<span data-ttu-id="bd190-141">Hello následující příklad kódu používá třídu, která implementuje `ICloudBlobStreamBinder` tooenable hello `BlobTrigger` atribut toobind blob toohello `WebImage` typu.</span><span class="sxs-lookup"><span data-stu-id="bd190-141">hello following code sample uses a class that implements `ICloudBlobStreamBinder` tooenable hello `BlobTrigger` attribute toobind a blob toohello `WebImage` type.</span></span>

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK", 
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

<span data-ttu-id="bd190-142">Hello `WebImage` vazby kódu je součástí `WebImageBinder` třídu odvozenou od `ICloudBlobStreamBinder`.</span><span class="sxs-lookup"><span data-stu-id="bd190-142">hello `WebImage` binding code is provided in a `WebImageBinder` class that derives from `ICloudBlobStreamBinder`.</span></span>

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input, 
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="getting-hello-blob-path-for-hello-triggering-blob"></a><span data-ttu-id="bd190-143">Získávání hello blob cestu pro hello aktivován objektů blob</span><span class="sxs-lookup"><span data-stu-id="bd190-143">Getting hello blob path for hello triggering blob</span></span>
<span data-ttu-id="bd190-144">název kontejneru hello tooget a název objektu blob objektu blob hello, který má aktivoval hello funkce zahrnují `blobTrigger` řetězcový parametr v podpis funkce hello.</span><span class="sxs-lookup"><span data-stu-id="bd190-144">tooget hello container name and blob name of hello blob that has triggered hello function, include a `blobTrigger` string parameter in hello function signature.</span></span>

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <span data-ttu-id="bd190-145"><a id="poison"></a>Jak toohandle poison objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="bd190-145"><a id="poison"></a> How toohandle poison blobs</span></span>
<span data-ttu-id="bd190-146">Když `BlobTrigger` funkce selže, hello SDK nazve je znovu, v případě, že hello selhání bylo způsobeno přechodné chybě.</span><span class="sxs-lookup"><span data-stu-id="bd190-146">When a `BlobTrigger` function fails, hello SDK calls it again, in case hello failure was caused by a transient error.</span></span> <span data-ttu-id="bd190-147">Pokud hello selhání je způsobena hello obsahu objektu hello blob, funkce hello selže pokaždé, když se pokusí o tooprocess hello blob.</span><span class="sxs-lookup"><span data-stu-id="bd190-147">If hello failure is caused by hello content of hello blob, hello function fails every time it tries tooprocess hello blob.</span></span> <span data-ttu-id="bd190-148">Ve výchozím nastavení hello SDK volá funkci too5 časů pro daný objekt blob.</span><span class="sxs-lookup"><span data-stu-id="bd190-148">By default, hello SDK calls a function up too5 times for a given blob.</span></span> <span data-ttu-id="bd190-149">V případě selhání páté zkuste hello hello SDK přidá zprávu tooa frontu s názvem *webjobs. blobtrigger poison*.</span><span class="sxs-lookup"><span data-stu-id="bd190-149">If hello fifth try fails, hello SDK adds a message tooa queue named *webjobs-blobtrigger-poison*.</span></span>

<span data-ttu-id="bd190-150">maximální počet opakovaných pokusů Hello je možné konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="bd190-150">hello maximum number of retries is configurable.</span></span> <span data-ttu-id="bd190-151">Hello stejné [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) nastavení se používá pro zpracování poškozených objektů blob a zpracování poškozených fronty zpráv.</span><span class="sxs-lookup"><span data-stu-id="bd190-151">hello same [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) setting is used for poison blob handling and poison queue message handling.</span></span> 

<span data-ttu-id="bd190-152">uvítací zprávu fronty pro poškozených objekty BLOB je objekt JSON, který obsahuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="bd190-152">hello queue message for poison blobs is a JSON object that contains hello following properties:</span></span>

* <span data-ttu-id="bd190-153">FunctionId (ve formátu hello *{název webové úlohy}*. Funkce. *{Název funkce}*, například: WebJob1.Functions.CopyBlob)</span><span class="sxs-lookup"><span data-stu-id="bd190-153">FunctionId (in hello format *{WebJob name}*.Functions.*{Function name}*, for example: WebJob1.Functions.CopyBlob)</span></span>
* <span data-ttu-id="bd190-154">BlobType ("BlockBlob" nebo "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="bd190-154">BlobType ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="bd190-155">ContainerName</span><span class="sxs-lookup"><span data-stu-id="bd190-155">ContainerName</span></span>
* <span data-ttu-id="bd190-156">BlobName</span><span class="sxs-lookup"><span data-stu-id="bd190-156">BlobName</span></span>
* <span data-ttu-id="bd190-157">Značka ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="bd190-157">ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="bd190-158">V hello následující ukázka kódu, hello `CopyBlob` funkce obsahuje kód, který způsobuje, že toofail pokaždé, když je volána.</span><span class="sxs-lookup"><span data-stu-id="bd190-158">In hello following code sample, hello `CopyBlob` function has code that causes it toofail every time it's called.</span></span> <span data-ttu-id="bd190-159">Po hello SDK nazve je pro hello maximální počet opakovaných pokusů, vytvoření zprávu ve frontě hello poškozených objektů blob a zpracovává zprávy hello `LogPoisonBlob` funkce.</span><span class="sxs-lookup"><span data-stu-id="bd190-159">After hello SDK calls it for hello maximum number of retries, a message is created on hello poison blob queue, and that message is processed by hello `LogPoisonBlob` function.</span></span> 

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }

        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

<span data-ttu-id="bd190-160">Hello SDK automaticky deserializuje uvítací zprávu JSON.</span><span class="sxs-lookup"><span data-stu-id="bd190-160">hello SDK automatically deserializes hello JSON message.</span></span> <span data-ttu-id="bd190-161">Tady je hello `PoisonBlobMessage` třídy:</span><span class="sxs-lookup"><span data-stu-id="bd190-161">Here is hello `PoisonBlobMessage` class:</span></span> 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <span data-ttu-id="bd190-162"><a id="polling"></a>Algoritmus dotazování objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="bd190-162"><a id="polling"></a> Blob polling algorithm</span></span>
<span data-ttu-id="bd190-163">Hello WebJobs SDK kontroluje všechny kontejnery určeného `BlobTrigger` atributy při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd190-163">hello WebJobs SDK scans all containers specified by `BlobTrigger` attributes at application start.</span></span> <span data-ttu-id="bd190-164">V účtu úložiště velké tato kontrola může trvat nějakou dobu, proto chvíli může být před nebyly nalezeny nové objekty BLOB a `BlobTrigger` provedení funkce.</span><span class="sxs-lookup"><span data-stu-id="bd190-164">In a large storage account this scan can take some time, so it might be a while before new blobs are found and `BlobTrigger` functions are executed.</span></span>

<span data-ttu-id="bd190-165">toodetect nové nebo změněné objekty BLOB po spuštění aplikace, zaznamená hello SDK pravidelně čte z úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="bd190-165">toodetect new or changed blobs after application start, hello SDK periodically reads from hello blob storage logs.</span></span> <span data-ttu-id="bd190-166">Hello blob protokoly jsou do vyrovnávací paměti a pouze získat fyzicky zapisují každých 10 minut, nebo tak, tedy s sebou může být důležité zpoždění po objekt blob se vytvoří nebo aktualizuje před hello odpovídající `BlobTrigger` provádí funkce.</span><span class="sxs-lookup"><span data-stu-id="bd190-166">hello blob logs are buffered and only get physically written every 10 minutes or so, so there may be significant delay after a blob is created or updated before hello corresponding `BlobTrigger` function executes.</span></span> 

<span data-ttu-id="bd190-167">Dojde k výjimce pro objekty BLOB, které vytvoříte pomocí hello `Blob` atribut.</span><span class="sxs-lookup"><span data-stu-id="bd190-167">There is an exception for blobs that you create by using hello `Blob` attribute.</span></span> <span data-ttu-id="bd190-168">Když hello WebJobs SDK vytvoří nový objekt blob, předá nový objekt blob hello okamžitě odpovídající tooany `BlobTrigger` funkce.</span><span class="sxs-lookup"><span data-stu-id="bd190-168">When hello WebJobs SDK creates a new blob, it passes hello new blob immediately tooany matching `BlobTrigger` functions.</span></span> <span data-ttu-id="bd190-169">Proto pokud máte řetěz blob vstupy a výstupy, hello SDK dokáže zpracovat efektivně.</span><span class="sxs-lookup"><span data-stu-id="bd190-169">Therefore if you have a chain of blob inputs and outputs, hello SDK can process them efficiently.</span></span> <span data-ttu-id="bd190-170">Pokud chcete s nízkou latencí systémem objektu blob služby zpracování funkce pro objekty BLOB, které jsou vytvořeny nebo aktualizovány jiným způsobem, doporučujeme používat, ale `QueueTrigger` místo `BlobTrigger`.</span><span class="sxs-lookup"><span data-stu-id="bd190-170">But if you want low latency running your blob processing functions for blobs that are created or updated by other means, we recommend using `QueueTrigger` rather than `BlobTrigger`.</span></span>

### <span data-ttu-id="bd190-171"><a id="receipts"></a>Potvrzení objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="bd190-171"><a id="receipts"></a> Blob receipts</span></span>
<span data-ttu-id="bd190-172">Hello WebJobs SDK je zajištěno, že žádné `BlobTrigger` funkce získá volaná víc než jednou pro hello stejné nové nebo aktualizované objektů blob.</span><span class="sxs-lookup"><span data-stu-id="bd190-172">hello WebJobs SDK makes sure that no `BlobTrigger` function gets called more than once for hello same new or updated blob.</span></span> <span data-ttu-id="bd190-173">Dělá to pomocí zachování *blob potvrzení* v toodetermine pořadí, pokud byla zpracována na verzi daného objektu blob.</span><span class="sxs-lookup"><span data-stu-id="bd190-173">It does this by maintaining *blob receipts* in order toodetermine if a given blob version has been processed.</span></span>

<span data-ttu-id="bd190-174">Potvrzení BLOB jsou uloženy v kontejneru nazvaném *azure. webové úlohy hostitelů* v účtu úložiště Azure hello určeného hello AzureWebJobsStorage připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="bd190-174">Blob receipts are stored in a container named *azure-webjobs-hosts* in hello Azure storage account specified by hello AzureWebJobsStorage connection string.</span></span> <span data-ttu-id="bd190-175">Potvrzení o objekt blob má hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="bd190-175">A blob receipt has hello following  information:</span></span>

* <span data-ttu-id="bd190-176">Hello funkce, která byla volána pro objekt blob hello ("*{název webové úlohy}*. Funkce. *{Název funkce}*", například:"WebJob1.Functions.CopyBlob")</span><span class="sxs-lookup"><span data-stu-id="bd190-176">hello function that was called for hello blob ("*{WebJob name}*.Functions.*{Function name}*", for example: "WebJob1.Functions.CopyBlob")</span></span>
* <span data-ttu-id="bd190-177">název kontejneru Hello</span><span class="sxs-lookup"><span data-stu-id="bd190-177">hello container name</span></span>
* <span data-ttu-id="bd190-178">Typ objektu blob Hello ("BlockBlob" nebo "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="bd190-178">hello blob type ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="bd190-179">Název objektu blob Hello</span><span class="sxs-lookup"><span data-stu-id="bd190-179">hello blob name</span></span>
* <span data-ttu-id="bd190-180">Hello ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="bd190-180">hello ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="bd190-181">Pokud chcete tooforce opětovné zpracování objektu blob, můžete ručně odstranit hello přijetí objektů blob pro tento objekt blob z hello *azure. webové úlohy hostitelů* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="bd190-181">If you want tooforce reprocessing of a blob, you can manually delete hello blob receipt for that blob from hello *azure-webjobs-hosts* container.</span></span>

## <span data-ttu-id="bd190-182"><a id="queues"></a>Související témata uvedených v článku fronty hello</span><span class="sxs-lookup"><span data-stu-id="bd190-182"><a id="queues"></a>Related topics covered by hello queues article</span></span>
<span data-ttu-id="bd190-183">Informace o tom, jak zpracování objektů blob toohandle aktivaci zprávu fronty, a to pro webové úlohy scénáře SDK není konkrétní tooblob zpracování, najdete v části [jak toouse Azure fronty úložiště s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="bd190-183">For information about how toohandle blob processing triggered by a queue message, or for WebJobs SDK scenarios not specific tooblob processing, see [How toouse Azure queue storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="bd190-184">Související témata popsaná v tomto článku hello následující:</span><span class="sxs-lookup"><span data-stu-id="bd190-184">Related topics covered in that article include hello following:</span></span>

* <span data-ttu-id="bd190-185">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="bd190-185">Async functions</span></span>
* <span data-ttu-id="bd190-186">Více instancí</span><span class="sxs-lookup"><span data-stu-id="bd190-186">Multiple instances</span></span>
* <span data-ttu-id="bd190-187">Řádné vypnutí</span><span class="sxs-lookup"><span data-stu-id="bd190-187">Graceful shutdown</span></span>
* <span data-ttu-id="bd190-188">Použití atributů WebJobs SDK v hello tělo funkce</span><span class="sxs-lookup"><span data-stu-id="bd190-188">Use WebJobs SDK attributes in hello body of a function</span></span>
* <span data-ttu-id="bd190-189">Nastavit hello SDK připojovacích řetězců v kódu.</span><span class="sxs-lookup"><span data-stu-id="bd190-189">Set hello SDK connection strings in code.</span></span>
* <span data-ttu-id="bd190-190">Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu</span><span class="sxs-lookup"><span data-stu-id="bd190-190">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="bd190-191">Konfigurace `MaxDequeueCount` pro zpracování poškozených objektů blob.</span><span class="sxs-lookup"><span data-stu-id="bd190-191">Configure `MaxDequeueCount` for poison blob handling.</span></span>
* <span data-ttu-id="bd190-192">Ruční aktivaci funkce</span><span class="sxs-lookup"><span data-stu-id="bd190-192">Trigger a function manually</span></span>
* <span data-ttu-id="bd190-193">Zápis protokolů</span><span class="sxs-lookup"><span data-stu-id="bd190-193">Write logs</span></span>

## <span data-ttu-id="bd190-194"><a id="nextsteps"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="bd190-194"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="bd190-195">Tato příručka poskytl ukázky kódu, které zobrazují jak objekty BLOB toohandle běžné scénáře pro práci s Azure.</span><span class="sxs-lookup"><span data-stu-id="bd190-195">This guide has provided code samples that show how toohandle common scenarios for working with Azure blobs.</span></span> <span data-ttu-id="bd190-196">Další informace o tom, jak toouse Azure WebJobs a hello WebJobs SDK najdete v části [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="bd190-196">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

