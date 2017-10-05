---
title: "Začínáme s objektem blob storage a Visual Studio připojené služeb (webové úlohy projekty) | Microsoft Docs"
description: "Jak začít používat úložiště objektů Blob v projektu webové úlohy po připojení k Azure storage pomocí sady Visual Studio připojené služby."
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 324c9376-0225-4092-9825-5d1bd5550058
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: a50a265feff8c0aec28825eb0bc4e33585ea5a02
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a><span data-ttu-id="8642b-103">Začínáme s Azure Blob storage a Visual Studio připojené služeb (webové úlohy projekty)</span><span class="sxs-lookup"><span data-stu-id="8642b-103">Get started with Azure Blob storage and Visual Studio connected services (WebJob projects)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="8642b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="8642b-104">Overview</span></span>
<span data-ttu-id="8642b-105">Tento článek obsahuje C# ukázek kódu, které ukazují, jak spustit proces, při vytvoření nebo aktualizaci objektu blob Azure.</span><span class="sxs-lookup"><span data-stu-id="8642b-105">This article provides C# code samples that show how to trigger a process when an Azure blob is created or updated.</span></span> <span data-ttu-id="8642b-106">Kód – ukázky použití [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) verze 1.x.</span><span class="sxs-lookup"><span data-stu-id="8642b-106">The code samples use the [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.</span></span> <span data-ttu-id="8642b-107">Při přidávání účtu úložiště do projektu úlohy WebJob pomocí sady Visual Studio **přidat připojení služby** dialogu odpovídající balíček NuGet pro úložiště Azure je nainstalovaný, odkazy na příslušné .NET jsou přidány do projektu a připojovací řetězce pro účet úložiště se aktualizují v souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="8642b-107">When you add a storage account to a WebJob project by using the Visual Studio **Add Connected Services** dialog, the appropriate Azure Storage NuGet package is installed, the appropriate .NET references are added to the project, and connection strings for the storage account are updated in the App.config file.</span></span>

## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a><span data-ttu-id="8642b-108">Jak aktivovat funkci, když se vytvoří nebo aktualizuje objekt blob</span><span class="sxs-lookup"><span data-stu-id="8642b-108">How to trigger a function when a blob is created or updated</span></span>
<span data-ttu-id="8642b-109">Tato část ukazuje způsob použití **BlobTrigger** atribut.</span><span class="sxs-lookup"><span data-stu-id="8642b-109">This section shows how to use the **BlobTrigger** attribute.</span></span>

 <span data-ttu-id="8642b-110">**Poznámka:** WebJobs SDK kontroluje soubory protokolů, které chcete sledovat pro nové nebo změněné objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="8642b-110">**Note:** The WebJobs SDK scans log files to watch for new or changed blobs.</span></span> <span data-ttu-id="8642b-111">Tento proces je ze své podstaty pomalé. funkce nemusí získat aktivuje až několik minut nebo déle po vytvoření objektu blob.</span><span class="sxs-lookup"><span data-stu-id="8642b-111">This process is inherently slow; a function might not get triggered until several minutes or longer after the blob is created.</span></span>  <span data-ttu-id="8642b-112">Pokud aplikace potřebuje ke zpracování objektů BLOB okamžitě, doporučuje se vytvořit zprávu fronty, při vytváření objektu blob a použít [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) atribut místo **BlobTrigger** atribut na funkce, která zpracovává objektu blob.</span><span class="sxs-lookup"><span data-stu-id="8642b-112">If your application needs to process blobs immediately, the recommended method is to create a queue message when you create the blob, and use the [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribute instead of the **BlobTrigger** attribute on the function that processes the blob.</span></span>

### <a name="single-placeholder-for-blob-name-with-extension"></a><span data-ttu-id="8642b-113">Jeden zástupný symbol pro název objektu blob s příponou</span><span class="sxs-lookup"><span data-stu-id="8642b-113">Single placeholder for blob name with extension</span></span>
<span data-ttu-id="8642b-114">Následující ukázka kódu zkopíruje text objekty BLOB, které se zobrazují v *vstupní* kontejner, aby *výstup* kontejneru:</span><span class="sxs-lookup"><span data-stu-id="8642b-114">The following code sample copies text blobs that appear in the *input* container to the *output* container:</span></span>

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="8642b-115">Konstruktoru atributu přijímá řetězcový parametr, který určuje název kontejneru a zástupný symbol pro název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="8642b-115">The attribute constructor takes a string parameter that specifies the container name and a placeholder for the blob name.</span></span> <span data-ttu-id="8642b-116">V tomto příkladu, pokud objekt blob s názvem *Blob1.txt* je vytvořen v *vstupní* kontejneru, funkce vytvoří objekt blob s názvem *Blob1.txt* v *výstup* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="8642b-116">In this example, if a blob named *Blob1.txt* is created in the *input* container, the function creates a blob named *Blob1.txt* in the *output* container.</span></span>

<span data-ttu-id="8642b-117">Vzor názvů s zástupný symbol název objektu blob, můžete určit, jak znázorňuje následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="8642b-117">You can specify a name pattern with the blob name placeholder, as shown in the following code sample:</span></span>

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="8642b-118">Tento kód zkopíruje pouze objekty BLOB, které mají názvy počínaje "původní-".</span><span class="sxs-lookup"><span data-stu-id="8642b-118">This code copies only blobs that have names beginning with "original-".</span></span> <span data-ttu-id="8642b-119">Například *původní Blob1.txt* v *vstupní* kontejneru se zkopíruje do *kopie Blob1.txt* v *výstup* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="8642b-119">For example, *original-Blob1.txt* in the *input* container is copied to *copy-Blob1.txt* in the *output* container.</span></span>

<span data-ttu-id="8642b-120">Pokud budete muset zadat vzor názvu pro názvy objektů blob, které mají v názvu složené závorky, dvakrát složené závorky.</span><span class="sxs-lookup"><span data-stu-id="8642b-120">If you need to specify a name pattern for blob names that have curly braces in the name, double the curly braces.</span></span> <span data-ttu-id="8642b-121">Například, pokud chcete najít objektů BLOB v *bitové kopie* kontejner, který mají názvy takto:</span><span class="sxs-lookup"><span data-stu-id="8642b-121">For example, if you want to find blobs in the *images* container that have names like this:</span></span>

        {20140101}-soundfile.mp3

<span data-ttu-id="8642b-122">To lze používejte pro vaše vzor:</span><span class="sxs-lookup"><span data-stu-id="8642b-122">use this for your pattern:</span></span>

        images/{{20140101}}-{name}

<span data-ttu-id="8642b-123">V příkladu *název* hodnotu zástupného symbolu by *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="8642b-123">In the example, the *name* placeholder value would be *soundfile.mp3*.</span></span>

### <a name="separate-blob-name-and-extension-placeholders"></a><span data-ttu-id="8642b-124">Zástupné symboly název a příponu samostatných objektů blob</span><span class="sxs-lookup"><span data-stu-id="8642b-124">Separate blob name and extension placeholders</span></span>
<span data-ttu-id="8642b-125">Následující ukázka kódu změní přípona souboru, protože kopíruje objekty BLOB, které se zobrazují v *vstupní* kontejner, aby *výstup* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="8642b-125">The following code sample changes the file extension as it copies blobs that appear in the *input* container to the *output* container.</span></span> <span data-ttu-id="8642b-126">Kód protokoly rozšíření *vstupní* objektů blob a nastaví rozšíření *výstup* objektu blob do *.txt*.</span><span class="sxs-lookup"><span data-stu-id="8642b-126">The code logs the extension of the *input* blob and sets the extension of the *output* blob to *.txt*.</span></span>

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

## <a name="types-that-you-can-bind-to-blobs"></a><span data-ttu-id="8642b-127">Typy, které můžete vázat na objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="8642b-127">Types that you can bind to blobs</span></span>
<span data-ttu-id="8642b-128">Můžete použít **BlobTrigger** atribut u následujících typů:</span><span class="sxs-lookup"><span data-stu-id="8642b-128">You can use the **BlobTrigger** attribute on the following types:</span></span>

* <span data-ttu-id="8642b-129">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="8642b-129">**string**</span></span>
* <span data-ttu-id="8642b-130">**TextReader**</span><span class="sxs-lookup"><span data-stu-id="8642b-130">**TextReader**</span></span>
* <span data-ttu-id="8642b-131">**Datový proud**</span><span class="sxs-lookup"><span data-stu-id="8642b-131">**Stream**</span></span>
* <span data-ttu-id="8642b-132">**ICloudBlob**</span><span class="sxs-lookup"><span data-stu-id="8642b-132">**ICloudBlob**</span></span>
* <span data-ttu-id="8642b-133">**CloudBlockBlob**</span><span class="sxs-lookup"><span data-stu-id="8642b-133">**CloudBlockBlob**</span></span>
* <span data-ttu-id="8642b-134">**CloudPageBlob**</span><span class="sxs-lookup"><span data-stu-id="8642b-134">**CloudPageBlob**</span></span>
* <span data-ttu-id="8642b-135">Jiné typy deserializoval [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)</span><span class="sxs-lookup"><span data-stu-id="8642b-135">Other types deserialized by [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)</span></span>

<span data-ttu-id="8642b-136">Pokud chcete pracovat přímo s účtu úložiště Azure, můžete také přidat **CloudStorageAccount** parametru podpis metody.</span><span class="sxs-lookup"><span data-stu-id="8642b-136">If you want to work directly with the Azure storage account, you can also add a **CloudStorageAccount** parameter to the method signature.</span></span>

## <a name="getting-text-blob-content-by-binding-to-string"></a><span data-ttu-id="8642b-137">Získávání obsahu objektu blob text vazby na řetězec</span><span class="sxs-lookup"><span data-stu-id="8642b-137">Getting text blob content by binding to string</span></span>
<span data-ttu-id="8642b-138">Pokud se očekává text objekty BLOB, **BlobTrigger** lze použít pro **řetězec** parametr.</span><span class="sxs-lookup"><span data-stu-id="8642b-138">If text blobs are expected, **BlobTrigger** can be applied to a **string** parameter.</span></span> <span data-ttu-id="8642b-139">Následující ukázka kódu váže objekt blob text **řetězec** parametr s názvem **logMessage**.</span><span class="sxs-lookup"><span data-stu-id="8642b-139">The following code sample binds a text blob to a **string** parameter named **logMessage**.</span></span> <span data-ttu-id="8642b-140">Funkce pomocí tohoto parametru zapsat obsah objektu blob na řídicím panelu WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="8642b-140">The function uses that parameter to write the contents of the blob to the WebJobs SDK dashboard.</span></span>

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a><span data-ttu-id="8642b-141">Získávání serializovat obsah objektu blob pomocí ICloudBlobStreamBinder</span><span class="sxs-lookup"><span data-stu-id="8642b-141">Getting serialized blob content by using ICloudBlobStreamBinder</span></span>
<span data-ttu-id="8642b-142">Následující příklad kódu používá třídu, která implementuje **ICloudBlobStreamBinder** povolit **BlobTrigger** atributu bude vazba provedena objekt blob **WebImage** typu.</span><span class="sxs-lookup"><span data-stu-id="8642b-142">The following code sample uses a class that implements **ICloudBlobStreamBinder** to enable the **BlobTrigger** attribute to bind a blob to the **WebImage** type.</span></span>

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

<span data-ttu-id="8642b-143">**WebImage** vazby kódu je součástí **WebImageBinder** třídu odvozenou od **ICloudBlobStreamBinder**.</span><span class="sxs-lookup"><span data-stu-id="8642b-143">The **WebImage** binding code is provided in a **WebImageBinder** class that derives from **ICloudBlobStreamBinder**.</span></span>

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

## <a name="how-to-handle-poison-blobs"></a><span data-ttu-id="8642b-144">Postupy: zpracování poškozených objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="8642b-144">How to handle poison blobs</span></span>
<span data-ttu-id="8642b-145">Když **BlobTrigger** funkce selže, sadu SDK nazve je znovu, v případě selhání bylo způsobeno přechodné chybě.</span><span class="sxs-lookup"><span data-stu-id="8642b-145">When a **BlobTrigger** function fails, the SDK calls it again, in case the failure was caused by a transient error.</span></span> <span data-ttu-id="8642b-146">Pokud selhání je způsobena obsah objektu blob, funkce selže pokaždé, když se pokusí zpracovat objektu blob.</span><span class="sxs-lookup"><span data-stu-id="8642b-146">If the failure is caused by the content of the blob, the function fails every time it tries to process the blob.</span></span> <span data-ttu-id="8642b-147">Ve výchozím nastavení sadu SDK volá funkci až 5 výskyty pro daný objekt blob.</span><span class="sxs-lookup"><span data-stu-id="8642b-147">By default, the SDK calls a function up to 5 times for a given blob.</span></span> <span data-ttu-id="8642b-148">Pokud pátý zkuste selže, sadu SDK přidá zprávu do fronty s názvem *webjobs. blobtrigger poison*.</span><span class="sxs-lookup"><span data-stu-id="8642b-148">If the fifth try fails, the SDK adds a message to a queue named *webjobs-blobtrigger-poison*.</span></span>

<span data-ttu-id="8642b-149">Maximální počet opakovaných pokusů je možné konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="8642b-149">The maximum number of retries is configurable.</span></span> <span data-ttu-id="8642b-150">Stejné [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) nastavení se používá pro zpracování poškozených objektů blob a zpracování poškozených fronty zpráv.</span><span class="sxs-lookup"><span data-stu-id="8642b-150">The same [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) setting is used for poison blob handling and poison queue message handling.</span></span>

<span data-ttu-id="8642b-151">Zprávy ve frontě pro poškozených objekty BLOB je objekt JSON, který obsahuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="8642b-151">The queue message for poison blobs is a JSON object that contains the following properties:</span></span>

* <span data-ttu-id="8642b-152">FunctionId (ve formátu *{název webové úlohy}*. Funkce. *{Název funkce}*, například: WebJob1.Functions.CopyBlob)</span><span class="sxs-lookup"><span data-stu-id="8642b-152">FunctionId (in the format *{WebJob name}*.Functions.*{Function name}*, for example: WebJob1.Functions.CopyBlob)</span></span>
* <span data-ttu-id="8642b-153">BlobType ("BlockBlob" nebo "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="8642b-153">BlobType ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="8642b-154">ContainerName</span><span class="sxs-lookup"><span data-stu-id="8642b-154">ContainerName</span></span>
* <span data-ttu-id="8642b-155">BlobName</span><span class="sxs-lookup"><span data-stu-id="8642b-155">BlobName</span></span>
* <span data-ttu-id="8642b-156">Značka ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="8642b-156">ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="8642b-157">V následující ukázce kódu **CopyBlob** funkce obsahuje kód, který způsobuje, že selžou pokaždé, když je volána.</span><span class="sxs-lookup"><span data-stu-id="8642b-157">In the following code sample, the **CopyBlob** function has code that causes it to fail every time it's called.</span></span> <span data-ttu-id="8642b-158">Po nazve je sada SDK pro maximální počet opakovaných pokusů, vytvoření zprávu ve frontě poškozených objektů blob a zpracovává zprávy **LogPoisonBlob** funkce.</span><span class="sxs-lookup"><span data-stu-id="8642b-158">After the SDK calls it for the maximum number of retries, a message is created on the poison blob queue, and that message is processed by the **LogPoisonBlob** function.</span></span>

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

<span data-ttu-id="8642b-159">Sada SDK automaticky deserializuje zpráva JSON.</span><span class="sxs-lookup"><span data-stu-id="8642b-159">The SDK automatically deserializes the JSON message.</span></span> <span data-ttu-id="8642b-160">Tady je **PoisonBlobMessage** třídy:</span><span class="sxs-lookup"><span data-stu-id="8642b-160">Here is the **PoisonBlobMessage** class:</span></span>

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a><span data-ttu-id="8642b-161">Algoritmus dotazování objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="8642b-161">Blob polling algorithm</span></span>
<span data-ttu-id="8642b-162">Sada WebJobs SDK kontroluje všechny kontejnery určeného **BlobTrigger** atributy při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="8642b-162">The WebJobs SDK scans all containers specified by **BlobTrigger** attributes at application start.</span></span> <span data-ttu-id="8642b-163">V účtu úložiště velké tato kontrola může trvat nějakou dobu, proto chvíli může být před nebyly nalezeny nové objekty BLOB a **BlobTrigger** provedení funkce.</span><span class="sxs-lookup"><span data-stu-id="8642b-163">In a large storage account this scan can take some time, so it might be a while before new blobs are found and **BlobTrigger** functions are executed.</span></span>

<span data-ttu-id="8642b-164">Ke zjištění nové nebo změněné objekty BLOB po spuštění aplikace, sady SDK pravidelně čte z protokolů úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="8642b-164">To detect new or changed blobs after application start, the SDK periodically reads from the blob storage logs.</span></span> <span data-ttu-id="8642b-165">Objekt blob protokoly jsou do vyrovnávací paměti a pouze získat fyzicky zapisují každých 10 minut, nebo Ano, tedy s sebou může být důležité zpoždění po objekt blob se vytvoří nebo aktualizuje před odpovídající **BlobTrigger** provádí funkce.</span><span class="sxs-lookup"><span data-stu-id="8642b-165">The blob logs are buffered and only get physically written every 10 minutes or so, so there may be significant delay after a blob is created or updated before the corresponding **BlobTrigger** function executes.</span></span>

<span data-ttu-id="8642b-166">Dojde k výjimce pro objekty BLOB, které vytvoříte pomocí **Blob** atribut.</span><span class="sxs-lookup"><span data-stu-id="8642b-166">There is an exception for blobs that you create by using the **Blob** attribute.</span></span> <span data-ttu-id="8642b-167">Když WebJobs SDK vytvoří nový objekt blob, pak předá nový objekt blob okamžitě žádné odpovídající **BlobTrigger** funkce.</span><span class="sxs-lookup"><span data-stu-id="8642b-167">When the WebJobs SDK creates a new blob, it passes the new blob immediately to any matching **BlobTrigger** functions.</span></span> <span data-ttu-id="8642b-168">Proto pokud máte řetěz blob vstupy a výstupy, sadu SDK dokáže zpracovat efektivně.</span><span class="sxs-lookup"><span data-stu-id="8642b-168">Therefore if you have a chain of blob inputs and outputs, the SDK can process them efficiently.</span></span> <span data-ttu-id="8642b-169">Pokud chcete s nízkou latencí systémem objektu blob služby zpracování funkce pro objekty BLOB, které jsou vytvořeny nebo aktualizovány jiným způsobem, doporučujeme používat, ale **QueueTrigger** místo **BlobTrigger**.</span><span class="sxs-lookup"><span data-stu-id="8642b-169">But if you want low latency running your blob processing functions for blobs that are created or updated by other means, we recommend using **QueueTrigger** rather than **BlobTrigger**.</span></span>

### <a name="blob-receipts"></a><span data-ttu-id="8642b-170">Potvrzení objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="8642b-170">Blob receipts</span></span>
<span data-ttu-id="8642b-171">Sada WebJobs SDK je zajištěno, že žádné **BlobTrigger** funkce pro stejný objekt blob nové nebo aktualizované volala více než jednou.</span><span class="sxs-lookup"><span data-stu-id="8642b-171">The WebJobs SDK makes sure that no **BlobTrigger** function gets called more than once for the same new or updated blob.</span></span> <span data-ttu-id="8642b-172">Dělá to pomocí zachování *blob potvrzení* aby bylo možné zjistit, pokud byla zpracována na verzi daného objektu blob.</span><span class="sxs-lookup"><span data-stu-id="8642b-172">It does this by maintaining *blob receipts* in order to determine if a given blob version has been processed.</span></span>

<span data-ttu-id="8642b-173">Potvrzení BLOB jsou uloženy v kontejneru nazvaném *azure. webové úlohy hostitelů* v účtu úložiště Azure určeného AzureWebJobsStorage připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="8642b-173">Blob receipts are stored in a container named *azure-webjobs-hosts* in the Azure storage account specified by the AzureWebJobsStorage connection string.</span></span> <span data-ttu-id="8642b-174">Potvrzení o objektu blob obsahuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="8642b-174">A blob receipt has the following  information:</span></span>

* <span data-ttu-id="8642b-175">Funkce, která byla volána pro tento objekt blob ("*{název webové úlohy}*. Funkce. *{Název funkce}*", například:"WebJob1.Functions.CopyBlob")</span><span class="sxs-lookup"><span data-stu-id="8642b-175">The function that was called for the blob ("*{WebJob name}*.Functions.*{Function name}*", for example: "WebJob1.Functions.CopyBlob")</span></span>
* <span data-ttu-id="8642b-176">Název kontejneru</span><span class="sxs-lookup"><span data-stu-id="8642b-176">The container name</span></span>
* <span data-ttu-id="8642b-177">Typ objektu blob ("BlockBlob" nebo "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="8642b-177">The blob type ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="8642b-178">Název objektu blob</span><span class="sxs-lookup"><span data-stu-id="8642b-178">The blob name</span></span>
* <span data-ttu-id="8642b-179">Značky ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="8642b-179">The ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="8642b-180">Pokud chcete vynutit opětovné zpracování objektu blob, můžete ručně odstranit objekt blob příjmu pro tento objekt blob z *azure. webové úlohy hostitelů* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="8642b-180">If you want to force reprocessing of a blob, you can manually delete the blob receipt for that blob from the *azure-webjobs-hosts* container.</span></span>

## <a name="related-topics-covered-by-the-queues-article"></a><span data-ttu-id="8642b-181">Související témata uvedených v článku fronty</span><span class="sxs-lookup"><span data-stu-id="8642b-181">Related topics covered by the queues article</span></span>
<span data-ttu-id="8642b-182">Informace o způsobu zpracování objektů blob zpracování aktivovány zprávu fronty, nebo WebJobs SDK scénáře, které nejsou specifické do objektu blob zpracování, projděte si téma [používání úložiště fronty Azure pomocí WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="8642b-182">For information about how to handle blob processing triggered by a queue message, or for WebJobs SDK scenarios not specific to blob processing, see [How to use Azure queue storage with the WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span>

<span data-ttu-id="8642b-183">Související témata popsaná v tomto článku zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="8642b-183">Related topics covered in that article include the following:</span></span>

* <span data-ttu-id="8642b-184">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="8642b-184">Async functions</span></span>
* <span data-ttu-id="8642b-185">Více instancí</span><span class="sxs-lookup"><span data-stu-id="8642b-185">Multiple instances</span></span>
* <span data-ttu-id="8642b-186">Řádné vypnutí</span><span class="sxs-lookup"><span data-stu-id="8642b-186">Graceful shutdown</span></span>
* <span data-ttu-id="8642b-187">Použití atributů WebJobs SDK v tělo funkce</span><span class="sxs-lookup"><span data-stu-id="8642b-187">Use WebJobs SDK attributes in the body of a function</span></span>
* <span data-ttu-id="8642b-188">Nastavte SDK připojovacích řetězců v kódu.</span><span class="sxs-lookup"><span data-stu-id="8642b-188">Set the SDK connection strings in code.</span></span>
* <span data-ttu-id="8642b-189">Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu</span><span class="sxs-lookup"><span data-stu-id="8642b-189">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="8642b-190">Konfigurace **MaxDequeueCount** pro zpracování poškozených objektů blob.</span><span class="sxs-lookup"><span data-stu-id="8642b-190">Configure **MaxDequeueCount** for poison blob handling.</span></span>
* <span data-ttu-id="8642b-191">Ruční aktivaci funkce</span><span class="sxs-lookup"><span data-stu-id="8642b-191">Trigger a function manually</span></span>
* <span data-ttu-id="8642b-192">Zápis protokolů</span><span class="sxs-lookup"><span data-stu-id="8642b-192">Write logs</span></span>

## <a name="next-steps"></a><span data-ttu-id="8642b-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8642b-193">Next steps</span></span>
<span data-ttu-id="8642b-194">Tento článek poskytl ukázek kódu, které ukazují, jak zpracovat běžné scénáře pro práci s objekty BLOB Azure.</span><span class="sxs-lookup"><span data-stu-id="8642b-194">This article has provided code samples that show how to handle common scenarios for working with Azure blobs.</span></span> <span data-ttu-id="8642b-195">Další informace o tom, jak používat Azure WebJobs a WebJobs SDK najdete v tématu [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="8642b-195">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

