---
title: "Jak používat Úložiště objektů blob v Azure pomocí WebJobs SDK"
description: "Naučte se používat úložiště objektů blob v Azure pomocí WebJobs SDK. Spustit proces, jakmile se zobrazí nový objekt blob v kontejneru a zpracovat \"poškozených objekty BLOB\"."
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
ms.openlocfilehash: e0a792ccdf8097d5cde254d6d4690a64838378ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a><span data-ttu-id="cfec4-104">Jak používat Úložiště objektů blob v Azure pomocí WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="cfec4-104">How to use Azure blob storage with the WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="cfec4-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="cfec4-105">Overview</span></span>
<span data-ttu-id="cfec4-106">Tato příručka obsahuje C# ukázek kódu, které ukazují, jak spustit proces, při vytvoření nebo aktualizaci objektu blob Azure.</span><span class="sxs-lookup"><span data-stu-id="cfec4-106">This guide provides C# code samples that show how to trigger a process when an Azure blob is created or updated.</span></span> <span data-ttu-id="cfec4-107">Kód – ukázky použití [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verze 1.x.</span><span class="sxs-lookup"><span data-stu-id="cfec4-107">The code samples use [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="cfec4-108">Ukázky kódu, které ukazují, jak vytvořit objekty BLOB, najdete v části [používání úložiště fronty Azure pomocí WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="cfec4-108">For code samples that show how to create blobs, see [How to use Azure queue storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="cfec4-109">V Průvodci se předpokládá, víte, [postup vytvoření projektu úlohy WebJob v sadě Visual Studio s připojením řetězce, které odkazují na účtu úložiště](websites-dotnet-webjobs-sdk-get-started.md) nebo [více účtů úložiště](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="cfec4-109">The guide assumes you know [how to create a WebJob project in Visual Studio with connection strings that point to your storage account](websites-dotnet-webjobs-sdk-get-started.md) or to [multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

## <span data-ttu-id="cfec4-110"><a id="trigger"></a>Jak aktivovat funkci, když se vytvoří nebo aktualizuje objekt blob</span><span class="sxs-lookup"><span data-stu-id="cfec4-110"><a id="trigger"></a> How to trigger a function when a blob is created or updated</span></span>
<span data-ttu-id="cfec4-111">Tato část ukazuje způsob použití `BlobTrigger` atribut.</span><span class="sxs-lookup"><span data-stu-id="cfec4-111">This section shows how to use the `BlobTrigger` attribute.</span></span> 

> [!NOTE]
> <span data-ttu-id="cfec4-112">Sada WebJobs SDK kontroluje soubory protokolů, které chcete sledovat pro nové nebo změněné objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="cfec4-112">The WebJobs SDK scans log files to watch for new or changed blobs.</span></span> <span data-ttu-id="cfec4-113">Tento proces není v reálném čase; funkce nemusí získat aktivuje až několik minut nebo déle po vytvoření objektu blob.</span><span class="sxs-lookup"><span data-stu-id="cfec4-113">This process is not real-time; a function might not get triggered until several minutes or longer after the blob is created.</span></span> <span data-ttu-id="cfec4-114">Kromě toho [protokol úložiště jsou vytvořené na "maximální úsilí"](https://msdn.microsoft.com/library/azure/hh343262.aspx) základ; není zaručeno, že bude možné zaznamenat všechny události.</span><span class="sxs-lookup"><span data-stu-id="cfec4-114">In addition, [storage logs are created on a "best efforts"](https://msdn.microsoft.com/library/azure/hh343262.aspx) basis; there is no guarantee that all events will be captured.</span></span> <span data-ttu-id="cfec4-115">Za určitých podmínek může být načteni protokoly.</span><span class="sxs-lookup"><span data-stu-id="cfec4-115">Under some conditions, logs might be missed.</span></span> <span data-ttu-id="cfec4-116">Pokud omezení rychlosti a spolehlivost služby aktivačních událostí objekt blob nejsou přijatelné pro vaši aplikaci, doporučuje se vytvořit zprávu fronty, při vytváření objektu blob a použít [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) atribut místo `BlobTrigger` atribut na funkce, která zpracovává objektu blob.</span><span class="sxs-lookup"><span data-stu-id="cfec4-116">If the speed and reliability limitations of blob triggers are not acceptable for your application, the recommended method is to create a queue message when you create the blob, and use the [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribute instead of the `BlobTrigger` attribute on the function that processes the blob.</span></span>
> 
> 

### <a name="single-placeholder-for-blob-name-with-extension"></a><span data-ttu-id="cfec4-117">Jeden zástupný symbol pro název objektu blob s příponou</span><span class="sxs-lookup"><span data-stu-id="cfec4-117">Single placeholder for blob name with extension</span></span>
<span data-ttu-id="cfec4-118">Následující ukázka kódu zkopíruje text objekty BLOB, které se zobrazují v *vstupní* kontejner, aby *výstup* kontejneru:</span><span class="sxs-lookup"><span data-stu-id="cfec4-118">The following code sample copies text blobs that appear in the *input* container to the *output* container:</span></span>

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="cfec4-119">Konstruktoru atributu přijímá řetězcový parametr, který určuje název kontejneru a zástupný symbol pro název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="cfec4-119">The attribute constructor takes a string parameter that specifies the container name and a placeholder for the blob name.</span></span> <span data-ttu-id="cfec4-120">V tomto příkladu, pokud objekt blob s názvem *Blob1.txt* je vytvořen v *vstupní* kontejneru, funkce vytvoří objekt blob s názvem *Blob1.txt* v *výstup* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="cfec4-120">In this example, if a blob named *Blob1.txt* is created in the *input* container, the function creates a blob named *Blob1.txt* in the *output* container.</span></span> 

<span data-ttu-id="cfec4-121">Vzor názvů s zástupný symbol název objektu blob, můžete určit, jak znázorňuje následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="cfec4-121">You can specify a name pattern with the blob name placeholder, as shown in the following code sample:</span></span>

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="cfec4-122">Tento kód zkopíruje pouze objekty BLOB, které mají názvy počínaje "původní-".</span><span class="sxs-lookup"><span data-stu-id="cfec4-122">This code copies only blobs that have names beginning with "original-".</span></span> <span data-ttu-id="cfec4-123">Například *původní Blob1.txt* v *vstupní* kontejneru se zkopíruje do *kopie Blob1.txt* v *výstup* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="cfec4-123">For example, *original-Blob1.txt* in the *input* container is copied to *copy-Blob1.txt* in the *output* container.</span></span>

<span data-ttu-id="cfec4-124">Pokud budete muset zadat vzor názvu pro názvy objektů blob, které mají v názvu složené závorky, dvakrát složené závorky.</span><span class="sxs-lookup"><span data-stu-id="cfec4-124">If you need to specify a name pattern for blob names that have curly braces in the name, double the curly braces.</span></span> <span data-ttu-id="cfec4-125">Například, pokud chcete najít objektů BLOB v *bitové kopie* kontejner, který mají názvy takto:</span><span class="sxs-lookup"><span data-stu-id="cfec4-125">For example, if you want to find blobs in the *images* container that have names like this:</span></span>

        {20140101}-soundfile.mp3

<span data-ttu-id="cfec4-126">To lze používejte pro vaše vzor:</span><span class="sxs-lookup"><span data-stu-id="cfec4-126">use this for your pattern:</span></span>

        images/{{20140101}}-{name}

<span data-ttu-id="cfec4-127">V příkladu *název* hodnotu zástupného symbolu by *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="cfec4-127">In the example, the *name* placeholder value would be *soundfile.mp3*.</span></span> 

### <a name="separate-blob-name-and-extension-placeholders"></a><span data-ttu-id="cfec4-128">Zástupné symboly název a příponu samostatných objektů blob</span><span class="sxs-lookup"><span data-stu-id="cfec4-128">Separate blob name and extension placeholders</span></span>
<span data-ttu-id="cfec4-129">Následující ukázka kódu změní přípona souboru, protože kopíruje objekty BLOB, které se zobrazují v *vstupní* kontejner, aby *výstup* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="cfec4-129">The following code sample changes the file extension as it copies blobs that appear in the *input* container to the *output* container.</span></span> <span data-ttu-id="cfec4-130">Kód protokoly rozšíření *vstupní* objektů blob a nastaví rozšíření *výstup* objektu blob do *.txt*.</span><span class="sxs-lookup"><span data-stu-id="cfec4-130">The code logs the extension of the *input* blob and sets the extension of the *output* blob to *.txt*.</span></span>

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

## <span data-ttu-id="cfec4-131"><a id="types"></a>Typy, které můžete vázat na objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="cfec4-131"><a id="types"></a> Types that you can bind to blobs</span></span>
<span data-ttu-id="cfec4-132">Můžete použít `BlobTrigger` atribut u následujících typů:</span><span class="sxs-lookup"><span data-stu-id="cfec4-132">You can use the `BlobTrigger` attribute on the following types:</span></span>

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
* <span data-ttu-id="cfec4-133">Jiné typy deserializoval [ICloudBlobStreamBinder](#icbsb)</span><span class="sxs-lookup"><span data-stu-id="cfec4-133">Other types deserialized by [ICloudBlobStreamBinder](#icbsb)</span></span> 

<span data-ttu-id="cfec4-134">Pokud chcete pracovat přímo s účtu úložiště Azure, můžete také přidat `CloudStorageAccount` parametru podpis metody.</span><span class="sxs-lookup"><span data-stu-id="cfec4-134">If you want to work directly with the Azure storage account, you can also add a `CloudStorageAccount` parameter to the method signature.</span></span>

<span data-ttu-id="cfec4-135">Příklady najdete v tématu [blob vazby kódu v úložišti azure. webjobs sdk na webu GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="cfec4-135">For examples, see the [blob binding code in the azure-webjobs-sdk repository on GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).</span></span>

## <span data-ttu-id="cfec4-136"><a id="string"></a>Získávání obsahu objektu blob text vazby na řetězec</span><span class="sxs-lookup"><span data-stu-id="cfec4-136"><a id="string"></a> Getting text blob content by binding to string</span></span>
<span data-ttu-id="cfec4-137">Pokud se očekává text objekty BLOB, `BlobTrigger` lze použít pro `string` parametr.</span><span class="sxs-lookup"><span data-stu-id="cfec4-137">If text blobs are expected, `BlobTrigger` can be applied to a `string` parameter.</span></span> <span data-ttu-id="cfec4-138">Následující ukázka kódu váže objekt blob text `string` parametr s názvem `logMessage`.</span><span class="sxs-lookup"><span data-stu-id="cfec4-138">The following code sample binds a text blob to a `string` parameter named `logMessage`.</span></span> <span data-ttu-id="cfec4-139">Funkce pomocí tohoto parametru zapsat obsah objektu blob na řídicím panelu WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="cfec4-139">The function uses that parameter to write the contents of the blob to the WebJobs SDK dashboard.</span></span> 

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <span data-ttu-id="cfec4-140"><a id="icbsb"></a>Získávání serializovat obsah objektu blob pomocí ICloudBlobStreamBinder</span><span class="sxs-lookup"><span data-stu-id="cfec4-140"><a id="icbsb"></a> Getting serialized blob content by using ICloudBlobStreamBinder</span></span>
<span data-ttu-id="cfec4-141">Následující příklad kódu používá třídu, která implementuje `ICloudBlobStreamBinder` povolit `BlobTrigger` atributu bude vazba provedena objekt blob `WebImage` typu.</span><span class="sxs-lookup"><span data-stu-id="cfec4-141">The following code sample uses a class that implements `ICloudBlobStreamBinder` to enable the `BlobTrigger` attribute to bind a blob to the `WebImage` type.</span></span>

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

<span data-ttu-id="cfec4-142">`WebImage` Vazby kódu je součástí `WebImageBinder` třídu odvozenou od `ICloudBlobStreamBinder`.</span><span class="sxs-lookup"><span data-stu-id="cfec4-142">The `WebImage` binding code is provided in a `WebImageBinder` class that derives from `ICloudBlobStreamBinder`.</span></span>

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

## <a name="getting-the-blob-path-for-the-triggering-blob"></a><span data-ttu-id="cfec4-143">Získání cesty objektu blob pro spouštěcí objektů blob</span><span class="sxs-lookup"><span data-stu-id="cfec4-143">Getting the blob path for the triggering blob</span></span>
<span data-ttu-id="cfec4-144">Chcete-li získat název kontejneru a název objektu blob objektu blob, který má aktivoval funkce, zahrnují `blobTrigger` řetězcový parametr v podpis funkce.</span><span class="sxs-lookup"><span data-stu-id="cfec4-144">To get the container name and blob name of the blob that has triggered the function, include a `blobTrigger` string parameter in the function signature.</span></span>

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <span data-ttu-id="cfec4-145"><a id="poison"></a>Postupy: zpracování poškozených objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="cfec4-145"><a id="poison"></a> How to handle poison blobs</span></span>
<span data-ttu-id="cfec4-146">Když `BlobTrigger` funkce selže, sadu SDK nazve je znovu, v případě selhání bylo způsobeno přechodné chybě.</span><span class="sxs-lookup"><span data-stu-id="cfec4-146">When a `BlobTrigger` function fails, the SDK calls it again, in case the failure was caused by a transient error.</span></span> <span data-ttu-id="cfec4-147">Pokud selhání je způsobena obsah objektu blob, funkce selže pokaždé, když se pokusí zpracovat objektu blob.</span><span class="sxs-lookup"><span data-stu-id="cfec4-147">If the failure is caused by the content of the blob, the function fails every time it tries to process the blob.</span></span> <span data-ttu-id="cfec4-148">Ve výchozím nastavení sadu SDK volá funkci až 5 výskyty pro daný objekt blob.</span><span class="sxs-lookup"><span data-stu-id="cfec4-148">By default, the SDK calls a function up to 5 times for a given blob.</span></span> <span data-ttu-id="cfec4-149">Pokud pátý zkuste selže, sadu SDK přidá zprávu do fronty s názvem *webjobs. blobtrigger poison*.</span><span class="sxs-lookup"><span data-stu-id="cfec4-149">If the fifth try fails, the SDK adds a message to a queue named *webjobs-blobtrigger-poison*.</span></span>

<span data-ttu-id="cfec4-150">Maximální počet opakovaných pokusů je možné konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="cfec4-150">The maximum number of retries is configurable.</span></span> <span data-ttu-id="cfec4-151">Stejné [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) nastavení se používá pro zpracování poškozených objektů blob a zpracování poškozených fronty zpráv.</span><span class="sxs-lookup"><span data-stu-id="cfec4-151">The same [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) setting is used for poison blob handling and poison queue message handling.</span></span> 

<span data-ttu-id="cfec4-152">Zprávy ve frontě pro poškozených objekty BLOB je objekt JSON, který obsahuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="cfec4-152">The queue message for poison blobs is a JSON object that contains the following properties:</span></span>

* <span data-ttu-id="cfec4-153">FunctionId (ve formátu *{název webové úlohy}*. Funkce. *{Název funkce}*, například: WebJob1.Functions.CopyBlob)</span><span class="sxs-lookup"><span data-stu-id="cfec4-153">FunctionId (in the format *{WebJob name}*.Functions.*{Function name}*, for example: WebJob1.Functions.CopyBlob)</span></span>
* <span data-ttu-id="cfec4-154">BlobType ("BlockBlob" nebo "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="cfec4-154">BlobType ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="cfec4-155">ContainerName</span><span class="sxs-lookup"><span data-stu-id="cfec4-155">ContainerName</span></span>
* <span data-ttu-id="cfec4-156">BlobName</span><span class="sxs-lookup"><span data-stu-id="cfec4-156">BlobName</span></span>
* <span data-ttu-id="cfec4-157">Značka ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="cfec4-157">ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="cfec4-158">V následující ukázce kódu `CopyBlob` funkce obsahuje kód, který způsobuje, že selžou pokaždé, když je volána.</span><span class="sxs-lookup"><span data-stu-id="cfec4-158">In the following code sample, the `CopyBlob` function has code that causes it to fail every time it's called.</span></span> <span data-ttu-id="cfec4-159">Po nazve je sada SDK pro maximální počet opakovaných pokusů, vytvoření zprávu ve frontě poškozených objektů blob a zpracovává zprávy `LogPoisonBlob` funkce.</span><span class="sxs-lookup"><span data-stu-id="cfec4-159">After the SDK calls it for the maximum number of retries, a message is created on the poison blob queue, and that message is processed by the `LogPoisonBlob` function.</span></span> 

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

<span data-ttu-id="cfec4-160">Sada SDK automaticky deserializuje zpráva JSON.</span><span class="sxs-lookup"><span data-stu-id="cfec4-160">The SDK automatically deserializes the JSON message.</span></span> <span data-ttu-id="cfec4-161">Tady je `PoisonBlobMessage` třídy:</span><span class="sxs-lookup"><span data-stu-id="cfec4-161">Here is the `PoisonBlobMessage` class:</span></span> 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <span data-ttu-id="cfec4-162"><a id="polling"></a>Algoritmus dotazování objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="cfec4-162"><a id="polling"></a> Blob polling algorithm</span></span>
<span data-ttu-id="cfec4-163">Sada WebJobs SDK kontroluje všechny kontejnery určeného `BlobTrigger` atributy při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="cfec4-163">The WebJobs SDK scans all containers specified by `BlobTrigger` attributes at application start.</span></span> <span data-ttu-id="cfec4-164">V účtu úložiště velké tato kontrola může trvat nějakou dobu, proto chvíli může být před nebyly nalezeny nové objekty BLOB a `BlobTrigger` provedení funkce.</span><span class="sxs-lookup"><span data-stu-id="cfec4-164">In a large storage account this scan can take some time, so it might be a while before new blobs are found and `BlobTrigger` functions are executed.</span></span>

<span data-ttu-id="cfec4-165">Ke zjištění nové nebo změněné objekty BLOB po spuštění aplikace, sady SDK pravidelně čte z protokolů úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="cfec4-165">To detect new or changed blobs after application start, the SDK periodically reads from the blob storage logs.</span></span> <span data-ttu-id="cfec4-166">Objekt blob protokoly jsou do vyrovnávací paměti a pouze získat fyzicky zapisují každých 10 minut, nebo Ano, tedy s sebou může být důležité zpoždění po objekt blob se vytvoří nebo aktualizuje před odpovídající `BlobTrigger` provádí funkce.</span><span class="sxs-lookup"><span data-stu-id="cfec4-166">The blob logs are buffered and only get physically written every 10 minutes or so, so there may be significant delay after a blob is created or updated before the corresponding `BlobTrigger` function executes.</span></span> 

<span data-ttu-id="cfec4-167">Dojde k výjimce pro objekty BLOB, které vytvoříte pomocí `Blob` atribut.</span><span class="sxs-lookup"><span data-stu-id="cfec4-167">There is an exception for blobs that you create by using the `Blob` attribute.</span></span> <span data-ttu-id="cfec4-168">Když WebJobs SDK vytvoří nový objekt blob, pak předá nový objekt blob okamžitě žádné odpovídající `BlobTrigger` funkce.</span><span class="sxs-lookup"><span data-stu-id="cfec4-168">When the WebJobs SDK creates a new blob, it passes the new blob immediately to any matching `BlobTrigger` functions.</span></span> <span data-ttu-id="cfec4-169">Proto pokud máte řetěz blob vstupy a výstupy, sadu SDK dokáže zpracovat efektivně.</span><span class="sxs-lookup"><span data-stu-id="cfec4-169">Therefore if you have a chain of blob inputs and outputs, the SDK can process them efficiently.</span></span> <span data-ttu-id="cfec4-170">Pokud chcete s nízkou latencí systémem objektu blob služby zpracování funkce pro objekty BLOB, které jsou vytvořeny nebo aktualizovány jiným způsobem, doporučujeme používat, ale `QueueTrigger` místo `BlobTrigger`.</span><span class="sxs-lookup"><span data-stu-id="cfec4-170">But if you want low latency running your blob processing functions for blobs that are created or updated by other means, we recommend using `QueueTrigger` rather than `BlobTrigger`.</span></span>

### <span data-ttu-id="cfec4-171"><a id="receipts"></a>Potvrzení objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="cfec4-171"><a id="receipts"></a> Blob receipts</span></span>
<span data-ttu-id="cfec4-172">Sada WebJobs SDK je zajištěno, že žádné `BlobTrigger` funkce pro stejný objekt blob nové nebo aktualizované volala více než jednou.</span><span class="sxs-lookup"><span data-stu-id="cfec4-172">The WebJobs SDK makes sure that no `BlobTrigger` function gets called more than once for the same new or updated blob.</span></span> <span data-ttu-id="cfec4-173">Dělá to pomocí zachování *blob potvrzení* aby bylo možné zjistit, pokud byla zpracována na verzi daného objektu blob.</span><span class="sxs-lookup"><span data-stu-id="cfec4-173">It does this by maintaining *blob receipts* in order to determine if a given blob version has been processed.</span></span>

<span data-ttu-id="cfec4-174">Potvrzení BLOB jsou uloženy v kontejneru nazvaném *azure. webové úlohy hostitelů* v účtu úložiště Azure určeného AzureWebJobsStorage připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="cfec4-174">Blob receipts are stored in a container named *azure-webjobs-hosts* in the Azure storage account specified by the AzureWebJobsStorage connection string.</span></span> <span data-ttu-id="cfec4-175">Potvrzení o objektu blob obsahuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="cfec4-175">A blob receipt has the following  information:</span></span>

* <span data-ttu-id="cfec4-176">Funkce, která byla volána pro tento objekt blob ("*{název webové úlohy}*. Funkce. *{Název funkce}*", například:"WebJob1.Functions.CopyBlob")</span><span class="sxs-lookup"><span data-stu-id="cfec4-176">The function that was called for the blob ("*{WebJob name}*.Functions.*{Function name}*", for example: "WebJob1.Functions.CopyBlob")</span></span>
* <span data-ttu-id="cfec4-177">Název kontejneru</span><span class="sxs-lookup"><span data-stu-id="cfec4-177">The container name</span></span>
* <span data-ttu-id="cfec4-178">Typ objektu blob ("BlockBlob" nebo "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="cfec4-178">The blob type ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="cfec4-179">Název objektu blob</span><span class="sxs-lookup"><span data-stu-id="cfec4-179">The blob name</span></span>
* <span data-ttu-id="cfec4-180">Značky ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="cfec4-180">The ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="cfec4-181">Pokud chcete vynutit opětovné zpracování objektu blob, můžete ručně odstranit objekt blob příjmu pro tento objekt blob z *azure. webové úlohy hostitelů* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="cfec4-181">If you want to force reprocessing of a blob, you can manually delete the blob receipt for that blob from the *azure-webjobs-hosts* container.</span></span>

## <span data-ttu-id="cfec4-182"><a id="queues"></a>Související témata uvedených v článku fronty</span><span class="sxs-lookup"><span data-stu-id="cfec4-182"><a id="queues"></a>Related topics covered by the queues article</span></span>
<span data-ttu-id="cfec4-183">Informace o způsobu zpracování objektů blob zpracování aktivovány zprávu fronty, nebo WebJobs SDK scénáře, které nejsou specifické do objektu blob zpracování, projděte si téma [používání úložiště fronty Azure pomocí WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="cfec4-183">For information about how to handle blob processing triggered by a queue message, or for WebJobs SDK scenarios not specific to blob processing, see [How to use Azure queue storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="cfec4-184">Související témata popsaná v tomto článku zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="cfec4-184">Related topics covered in that article include the following:</span></span>

* <span data-ttu-id="cfec4-185">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="cfec4-185">Async functions</span></span>
* <span data-ttu-id="cfec4-186">Více instancí</span><span class="sxs-lookup"><span data-stu-id="cfec4-186">Multiple instances</span></span>
* <span data-ttu-id="cfec4-187">Řádné vypnutí</span><span class="sxs-lookup"><span data-stu-id="cfec4-187">Graceful shutdown</span></span>
* <span data-ttu-id="cfec4-188">Použití atributů WebJobs SDK v tělo funkce</span><span class="sxs-lookup"><span data-stu-id="cfec4-188">Use WebJobs SDK attributes in the body of a function</span></span>
* <span data-ttu-id="cfec4-189">Nastavte SDK připojovacích řetězců v kódu.</span><span class="sxs-lookup"><span data-stu-id="cfec4-189">Set the SDK connection strings in code.</span></span>
* <span data-ttu-id="cfec4-190">Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu</span><span class="sxs-lookup"><span data-stu-id="cfec4-190">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="cfec4-191">Konfigurace `MaxDequeueCount` pro zpracování poškozených objektů blob.</span><span class="sxs-lookup"><span data-stu-id="cfec4-191">Configure `MaxDequeueCount` for poison blob handling.</span></span>
* <span data-ttu-id="cfec4-192">Ruční aktivaci funkce</span><span class="sxs-lookup"><span data-stu-id="cfec4-192">Trigger a function manually</span></span>
* <span data-ttu-id="cfec4-193">Zápis protokolů</span><span class="sxs-lookup"><span data-stu-id="cfec4-193">Write logs</span></span>

## <span data-ttu-id="cfec4-194"><a id="nextsteps"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="cfec4-194"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="cfec4-195">Tato příručka poskytl ukázek kódu, které ukazují, jak zpracovat běžné scénáře pro práci s objekty BLOB Azure.</span><span class="sxs-lookup"><span data-stu-id="cfec4-195">This guide has provided code samples that show how to handle common scenarios for working with Azure blobs.</span></span> <span data-ttu-id="cfec4-196">Další informace o tom, jak používat Azure WebJobs a WebJobs SDK najdete v tématu [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="cfec4-196">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

