---
title: "aaaGet začít s úložiště objektů blob a Visual Studio připojených služeb (webové úlohy projekty) | Microsoft Docs"
description: "Jak tooget spustit po připojení tooan úložiště Azure pomocí sady Visual Studio pomocí úložiště objektů Blob v projektu webové úlohy připojený služby."
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
ms.openlocfilehash: 29f2d5e19426d37d815cdf9a1e00abfb1e07ccf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a><span data-ttu-id="0b053-103">Začínáme s Azure Blob storage a Visual Studio připojené služeb (webové úlohy projekty)</span><span class="sxs-lookup"><span data-stu-id="0b053-103">Get started with Azure Blob storage and Visual Studio connected services (WebJob projects)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="0b053-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="0b053-104">Overview</span></span>
<span data-ttu-id="0b053-105">Tento článek obsahuje C# kódu ukázky, zobrazující jak tootrigger proces při vytvoření nebo aktualizaci objektu blob Azure.</span><span class="sxs-lookup"><span data-stu-id="0b053-105">This article provides C# code samples that show how tootrigger a process when an Azure blob is created or updated.</span></span> <span data-ttu-id="0b053-106">Ukázky kódu Hello použít hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) verze 1.x.</span><span class="sxs-lookup"><span data-stu-id="0b053-106">hello code samples use hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.</span></span> <span data-ttu-id="0b053-107">Když přidáte projektu úlohy WebJob tooa účet úložiště pomocí hello Visual Studio **přidat připojení služby** dialogu hello odpovídající balíček NuGet úložiště Azure je nainstalovaný a hello příslušné odkazy na rozhraní .NET jsou přidané toohello projekt a připojovací řetězce pro účet úložiště hello se aktualizují v souboru App.config hello.</span><span class="sxs-lookup"><span data-stu-id="0b053-107">When you add a storage account tooa WebJob project by using hello Visual Studio **Add Connected Services** dialog, hello appropriate Azure Storage NuGet package is installed, hello appropriate .NET references are added toohello project, and connection strings for hello storage account are updated in hello App.config file.</span></span>

## <a name="how-tootrigger-a-function-when-a-blob-is-created-or-updated"></a><span data-ttu-id="0b053-108">Jak tootrigger funkce při vytvoření nebo aktualizaci objektu blob</span><span class="sxs-lookup"><span data-stu-id="0b053-108">How tootrigger a function when a blob is created or updated</span></span>
<span data-ttu-id="0b053-109">Tato část uvádí, jak toouse hello **BlobTrigger** atribut.</span><span class="sxs-lookup"><span data-stu-id="0b053-109">This section shows how toouse hello **BlobTrigger** attribute.</span></span>

 <span data-ttu-id="0b053-110">**Poznámka:** hello WebJobs SDK kontroly protokolu soubory toowatch pro nové nebo změněné objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="0b053-110">**Note:** hello WebJobs SDK scans log files toowatch for new or changed blobs.</span></span> <span data-ttu-id="0b053-111">Tento proces je ze své podstaty pomalé. funkce nemusí získat aktivuje až několik minut nebo déle po vytvoření objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="0b053-111">This process is inherently slow; a function might not get triggered until several minutes or longer after hello blob is created.</span></span>  <span data-ttu-id="0b053-112">Pokud aplikace potřebuje objekty BLOB tooprocess okamžitě, hello doporučená metoda je toocreate zprávu fronty, při vytváření objektu blob hello a použijte hello [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) atribut místo hello **BlobTrigger** atribut hello funkce, která zpracovává hello objektů blob.</span><span class="sxs-lookup"><span data-stu-id="0b053-112">If your application needs tooprocess blobs immediately, hello recommended method is toocreate a queue message when you create hello blob, and use hello [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribute instead of hello **BlobTrigger** attribute on hello function that processes hello blob.</span></span>

### <a name="single-placeholder-for-blob-name-with-extension"></a><span data-ttu-id="0b053-113">Jeden zástupný symbol pro název objektu blob s příponou</span><span class="sxs-lookup"><span data-stu-id="0b053-113">Single placeholder for blob name with extension</span></span>
<span data-ttu-id="0b053-114">Hello následující ukázka kódu zkopíruje text objekty BLOB, které se zobrazují v hello *vstupní* kontejneru toohello *výstup* kontejneru:</span><span class="sxs-lookup"><span data-stu-id="0b053-114">hello following code sample copies text blobs that appear in hello *input* container toohello *output* container:</span></span>

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="0b053-115">konstruktoru atributu Hello přijímá řetězcový parametr, který určuje název kontejneru hello a zástupný symbol pro název objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="0b053-115">hello attribute constructor takes a string parameter that specifies hello container name and a placeholder for hello blob name.</span></span> <span data-ttu-id="0b053-116">V tomto příkladu, pokud objekt blob s názvem *Blob1.txt* je vytvořen v hello *vstupní* kontejneru hello funkce vytvoří objekt blob s názvem *Blob1.txt* v hello *výstup*  kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0b053-116">In this example, if a blob named *Blob1.txt* is created in hello *input* container, hello function creates a blob named *Blob1.txt* in hello *output* container.</span></span>

<span data-ttu-id="0b053-117">Vzor názvů s hello zástupný symbol název objektu blob, můžete určit, jak je znázorněno v hello následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="0b053-117">You can specify a name pattern with hello blob name placeholder, as shown in hello following code sample:</span></span>

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="0b053-118">Tento kód zkopíruje pouze objekty BLOB, které mají názvy počínaje "původní-".</span><span class="sxs-lookup"><span data-stu-id="0b053-118">This code copies only blobs that have names beginning with "original-".</span></span> <span data-ttu-id="0b053-119">Například *původní Blob1.txt* v hello *vstupní* kontejneru se zkopíruje příliš*kopie Blob1.txt* v hello *výstup* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0b053-119">For example, *original-Blob1.txt* in hello *input* container is copied too*copy-Blob1.txt* in hello *output* container.</span></span>

<span data-ttu-id="0b053-120">Pokud potřebujete toospecify vzor názvu pro názvy objektů blob, které mají v názvu hello složené závorky, dvakrát hello složené závorky.</span><span class="sxs-lookup"><span data-stu-id="0b053-120">If you need toospecify a name pattern for blob names that have curly braces in hello name, double hello curly braces.</span></span> <span data-ttu-id="0b053-121">Například, pokud chcete, aby toofind objektů BLOB v hello *bitové kopie* kontejner, který mají názvy takto:</span><span class="sxs-lookup"><span data-stu-id="0b053-121">For example, if you want toofind blobs in hello *images* container that have names like this:</span></span>

        {20140101}-soundfile.mp3

<span data-ttu-id="0b053-122">To lze používejte pro vaše vzor:</span><span class="sxs-lookup"><span data-stu-id="0b053-122">use this for your pattern:</span></span>

        images/{{20140101}}-{name}

<span data-ttu-id="0b053-123">V příkladu hello hello *název* hodnotu zástupného symbolu by *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="0b053-123">In hello example, hello *name* placeholder value would be *soundfile.mp3*.</span></span>

### <a name="separate-blob-name-and-extension-placeholders"></a><span data-ttu-id="0b053-124">Zástupné symboly název a příponu samostatných objektů blob</span><span class="sxs-lookup"><span data-stu-id="0b053-124">Separate blob name and extension placeholders</span></span>
<span data-ttu-id="0b053-125">Hello následující změny ukázkový kód hello příponu souboru jako kopíruje objekty BLOB, které se zobrazují v hello *vstupní* kontejneru toohello *výstup* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0b053-125">hello following code sample changes hello file extension as it copies blobs that appear in hello *input* container toohello *output* container.</span></span> <span data-ttu-id="0b053-126">Kód Hello protokoly hello rozšíření hello *vstupní* objektů blob a nastaví hello rozšíření hello *výstup* blob příliš*.txt*.</span><span class="sxs-lookup"><span data-stu-id="0b053-126">hello code logs hello extension of hello *input* blob and sets hello extension of hello *output* blob too*.txt*.</span></span>

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

## <a name="types-that-you-can-bind-tooblobs"></a><span data-ttu-id="0b053-127">Typy tooblobs můžete vytvořit vazbu</span><span class="sxs-lookup"><span data-stu-id="0b053-127">Types that you can bind tooblobs</span></span>
<span data-ttu-id="0b053-128">Můžete použít hello **BlobTrigger** atribut hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="0b053-128">You can use hello **BlobTrigger** attribute on hello following types:</span></span>

* <span data-ttu-id="0b053-129">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="0b053-129">**string**</span></span>
* <span data-ttu-id="0b053-130">**TextReader**</span><span class="sxs-lookup"><span data-stu-id="0b053-130">**TextReader**</span></span>
* <span data-ttu-id="0b053-131">**Datový proud**</span><span class="sxs-lookup"><span data-stu-id="0b053-131">**Stream**</span></span>
* <span data-ttu-id="0b053-132">**ICloudBlob**</span><span class="sxs-lookup"><span data-stu-id="0b053-132">**ICloudBlob**</span></span>
* <span data-ttu-id="0b053-133">**CloudBlockBlob**</span><span class="sxs-lookup"><span data-stu-id="0b053-133">**CloudBlockBlob**</span></span>
* <span data-ttu-id="0b053-134">**CloudPageBlob**</span><span class="sxs-lookup"><span data-stu-id="0b053-134">**CloudPageBlob**</span></span>
* <span data-ttu-id="0b053-135">Jiné typy deserializoval [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)</span><span class="sxs-lookup"><span data-stu-id="0b053-135">Other types deserialized by [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)</span></span>

<span data-ttu-id="0b053-136">Pokud chcete toowork přímo s hello účtu úložiště Azure, můžete také přidat **CloudStorageAccount** podpis metody toohello parametr.</span><span class="sxs-lookup"><span data-stu-id="0b053-136">If you want toowork directly with hello Azure storage account, you can also add a **CloudStorageAccount** parameter toohello method signature.</span></span>

## <a name="getting-text-blob-content-by-binding-toostring"></a><span data-ttu-id="0b053-137">Získávání obsahu objektu blob text toostring vazby</span><span class="sxs-lookup"><span data-stu-id="0b053-137">Getting text blob content by binding toostring</span></span>
<span data-ttu-id="0b053-138">Pokud se očekává text objekty BLOB, **BlobTrigger** může být použité tooa **řetězec** parametr.</span><span class="sxs-lookup"><span data-stu-id="0b053-138">If text blobs are expected, **BlobTrigger** can be applied tooa **string** parameter.</span></span> <span data-ttu-id="0b053-139">Hello následující ukázka kódu váže tooa objektů blob text **řetězec** parametr s názvem **logMessage**.</span><span class="sxs-lookup"><span data-stu-id="0b053-139">hello following code sample binds a text blob tooa **string** parameter named **logMessage**.</span></span> <span data-ttu-id="0b053-140">Funkce Hello používá tento parametr toowrite hello obsah objektu blob toohello hello řídicím panelu WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="0b053-140">hello function uses that parameter toowrite hello contents of hello blob toohello WebJobs SDK dashboard.</span></span>

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a><span data-ttu-id="0b053-141">Získávání serializovat obsah objektu blob pomocí ICloudBlobStreamBinder</span><span class="sxs-lookup"><span data-stu-id="0b053-141">Getting serialized blob content by using ICloudBlobStreamBinder</span></span>
<span data-ttu-id="0b053-142">Hello následující příklad kódu používá třídu, která implementuje **ICloudBlobStreamBinder** tooenable hello **BlobTrigger** atribut toobind blob toohello **WebImage** typu.</span><span class="sxs-lookup"><span data-stu-id="0b053-142">hello following code sample uses a class that implements **ICloudBlobStreamBinder** tooenable hello **BlobTrigger** attribute toobind a blob toohello **WebImage** type.</span></span>

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

<span data-ttu-id="0b053-143">Hello **WebImage** vazby kódu je součástí **WebImageBinder** třídu odvozenou od **ICloudBlobStreamBinder**.</span><span class="sxs-lookup"><span data-stu-id="0b053-143">hello **WebImage** binding code is provided in a **WebImageBinder** class that derives from **ICloudBlobStreamBinder**.</span></span>

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

## <a name="how-toohandle-poison-blobs"></a><span data-ttu-id="0b053-144">Jak toohandle poison objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="0b053-144">How toohandle poison blobs</span></span>
<span data-ttu-id="0b053-145">Když **BlobTrigger** funkce selže, hello SDK nazve je znovu, v případě, že hello selhání bylo způsobeno přechodné chybě.</span><span class="sxs-lookup"><span data-stu-id="0b053-145">When a **BlobTrigger** function fails, hello SDK calls it again, in case hello failure was caused by a transient error.</span></span> <span data-ttu-id="0b053-146">Pokud hello selhání je způsobena hello obsahu objektu hello blob, funkce hello selže pokaždé, když se pokusí o tooprocess hello blob.</span><span class="sxs-lookup"><span data-stu-id="0b053-146">If hello failure is caused by hello content of hello blob, hello function fails every time it tries tooprocess hello blob.</span></span> <span data-ttu-id="0b053-147">Ve výchozím nastavení hello SDK volá funkci too5 časů pro daný objekt blob.</span><span class="sxs-lookup"><span data-stu-id="0b053-147">By default, hello SDK calls a function up too5 times for a given blob.</span></span> <span data-ttu-id="0b053-148">V případě selhání páté zkuste hello hello SDK přidá zprávu tooa frontu s názvem *webjobs. blobtrigger poison*.</span><span class="sxs-lookup"><span data-stu-id="0b053-148">If hello fifth try fails, hello SDK adds a message tooa queue named *webjobs-blobtrigger-poison*.</span></span>

<span data-ttu-id="0b053-149">maximální počet opakovaných pokusů Hello je možné konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="0b053-149">hello maximum number of retries is configurable.</span></span> <span data-ttu-id="0b053-150">Hello stejné [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) nastavení se používá pro zpracování poškozených objektů blob a zpracování poškozených fronty zpráv.</span><span class="sxs-lookup"><span data-stu-id="0b053-150">hello same [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) setting is used for poison blob handling and poison queue message handling.</span></span>

<span data-ttu-id="0b053-151">uvítací zprávu fronty pro poškozených objekty BLOB je objekt JSON, který obsahuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="0b053-151">hello queue message for poison blobs is a JSON object that contains hello following properties:</span></span>

* <span data-ttu-id="0b053-152">FunctionId (ve formátu hello *{název webové úlohy}*. Funkce. *{Název funkce}*, například: WebJob1.Functions.CopyBlob)</span><span class="sxs-lookup"><span data-stu-id="0b053-152">FunctionId (in hello format *{WebJob name}*.Functions.*{Function name}*, for example: WebJob1.Functions.CopyBlob)</span></span>
* <span data-ttu-id="0b053-153">BlobType ("BlockBlob" nebo "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="0b053-153">BlobType ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="0b053-154">ContainerName</span><span class="sxs-lookup"><span data-stu-id="0b053-154">ContainerName</span></span>
* <span data-ttu-id="0b053-155">BlobName</span><span class="sxs-lookup"><span data-stu-id="0b053-155">BlobName</span></span>
* <span data-ttu-id="0b053-156">Značka ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="0b053-156">ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="0b053-157">V hello následující ukázka kódu, hello **CopyBlob** funkce obsahuje kód, který způsobuje, že toofail pokaždé, když je volána.</span><span class="sxs-lookup"><span data-stu-id="0b053-157">In hello following code sample, hello **CopyBlob** function has code that causes it toofail every time it's called.</span></span> <span data-ttu-id="0b053-158">Po hello SDK nazve je pro hello maximální počet opakovaných pokusů, vytvoření zprávu ve frontě hello poškozených objektů blob a zpracovává zprávy hello **LogPoisonBlob** funkce.</span><span class="sxs-lookup"><span data-stu-id="0b053-158">After hello SDK calls it for hello maximum number of retries, a message is created on hello poison blob queue, and that message is processed by hello **LogPoisonBlob** function.</span></span>

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

<span data-ttu-id="0b053-159">Hello SDK automaticky deserializuje uvítací zprávu JSON.</span><span class="sxs-lookup"><span data-stu-id="0b053-159">hello SDK automatically deserializes hello JSON message.</span></span> <span data-ttu-id="0b053-160">Tady je hello **PoisonBlobMessage** třídy:</span><span class="sxs-lookup"><span data-stu-id="0b053-160">Here is hello **PoisonBlobMessage** class:</span></span>

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a><span data-ttu-id="0b053-161">Algoritmus dotazování objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="0b053-161">Blob polling algorithm</span></span>
<span data-ttu-id="0b053-162">Hello WebJobs SDK kontroluje všechny kontejnery určeného **BlobTrigger** atributy při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b053-162">hello WebJobs SDK scans all containers specified by **BlobTrigger** attributes at application start.</span></span> <span data-ttu-id="0b053-163">V účtu úložiště velké tato kontrola může trvat nějakou dobu, proto chvíli může být před nebyly nalezeny nové objekty BLOB a **BlobTrigger** provedení funkce.</span><span class="sxs-lookup"><span data-stu-id="0b053-163">In a large storage account this scan can take some time, so it might be a while before new blobs are found and **BlobTrigger** functions are executed.</span></span>

<span data-ttu-id="0b053-164">toodetect nové nebo změněné objekty BLOB po spuštění aplikace, zaznamená hello SDK pravidelně čte z úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="0b053-164">toodetect new or changed blobs after application start, hello SDK periodically reads from hello blob storage logs.</span></span> <span data-ttu-id="0b053-165">Hello blob protokoly jsou do vyrovnávací paměti a pouze získat fyzicky zapisují každých 10 minut, nebo tak, tedy s sebou může být důležité zpoždění po objekt blob se vytvoří nebo aktualizuje před hello odpovídající **BlobTrigger** provádí funkce.</span><span class="sxs-lookup"><span data-stu-id="0b053-165">hello blob logs are buffered and only get physically written every 10 minutes or so, so there may be significant delay after a blob is created or updated before hello corresponding **BlobTrigger** function executes.</span></span>

<span data-ttu-id="0b053-166">Dojde k výjimce pro objekty BLOB, které vytvoříte pomocí hello **Blob** atribut.</span><span class="sxs-lookup"><span data-stu-id="0b053-166">There is an exception for blobs that you create by using hello **Blob** attribute.</span></span> <span data-ttu-id="0b053-167">Když hello WebJobs SDK vytvoří nový objekt blob, předá nový objekt blob hello okamžitě odpovídající tooany **BlobTrigger** funkce.</span><span class="sxs-lookup"><span data-stu-id="0b053-167">When hello WebJobs SDK creates a new blob, it passes hello new blob immediately tooany matching **BlobTrigger** functions.</span></span> <span data-ttu-id="0b053-168">Proto pokud máte řetěz blob vstupy a výstupy, hello SDK dokáže zpracovat efektivně.</span><span class="sxs-lookup"><span data-stu-id="0b053-168">Therefore if you have a chain of blob inputs and outputs, hello SDK can process them efficiently.</span></span> <span data-ttu-id="0b053-169">Pokud chcete s nízkou latencí systémem objektu blob služby zpracování funkce pro objekty BLOB, které jsou vytvořeny nebo aktualizovány jiným způsobem, doporučujeme používat, ale **QueueTrigger** místo **BlobTrigger**.</span><span class="sxs-lookup"><span data-stu-id="0b053-169">But if you want low latency running your blob processing functions for blobs that are created or updated by other means, we recommend using **QueueTrigger** rather than **BlobTrigger**.</span></span>

### <a name="blob-receipts"></a><span data-ttu-id="0b053-170">Potvrzení objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="0b053-170">Blob receipts</span></span>
<span data-ttu-id="0b053-171">Hello WebJobs SDK je zajištěno, že žádné **BlobTrigger** funkce získá volaná víc než jednou pro hello stejné nové nebo aktualizované objektů blob.</span><span class="sxs-lookup"><span data-stu-id="0b053-171">hello WebJobs SDK makes sure that no **BlobTrigger** function gets called more than once for hello same new or updated blob.</span></span> <span data-ttu-id="0b053-172">Dělá to pomocí zachování *blob potvrzení* v toodetermine pořadí, pokud byla zpracována na verzi daného objektu blob.</span><span class="sxs-lookup"><span data-stu-id="0b053-172">It does this by maintaining *blob receipts* in order toodetermine if a given blob version has been processed.</span></span>

<span data-ttu-id="0b053-173">Potvrzení BLOB jsou uloženy v kontejneru nazvaném *azure. webové úlohy hostitelů* v účtu úložiště Azure hello určeného hello AzureWebJobsStorage připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="0b053-173">Blob receipts are stored in a container named *azure-webjobs-hosts* in hello Azure storage account specified by hello AzureWebJobsStorage connection string.</span></span> <span data-ttu-id="0b053-174">Potvrzení o objekt blob má hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="0b053-174">A blob receipt has hello following  information:</span></span>

* <span data-ttu-id="0b053-175">Hello funkce, která byla volána pro objekt blob hello ("*{název webové úlohy}*. Funkce. *{Název funkce}*", například:"WebJob1.Functions.CopyBlob")</span><span class="sxs-lookup"><span data-stu-id="0b053-175">hello function that was called for hello blob ("*{WebJob name}*.Functions.*{Function name}*", for example: "WebJob1.Functions.CopyBlob")</span></span>
* <span data-ttu-id="0b053-176">název kontejneru Hello</span><span class="sxs-lookup"><span data-stu-id="0b053-176">hello container name</span></span>
* <span data-ttu-id="0b053-177">Typ objektu blob Hello ("BlockBlob" nebo "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="0b053-177">hello blob type ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="0b053-178">Název objektu blob Hello</span><span class="sxs-lookup"><span data-stu-id="0b053-178">hello blob name</span></span>
* <span data-ttu-id="0b053-179">Hello ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="0b053-179">hello ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="0b053-180">Pokud chcete tooforce opětovné zpracování objektu blob, můžete ručně odstranit hello přijetí objektů blob pro tento objekt blob z hello *azure. webové úlohy hostitelů* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0b053-180">If you want tooforce reprocessing of a blob, you can manually delete hello blob receipt for that blob from hello *azure-webjobs-hosts* container.</span></span>

## <a name="related-topics-covered-by-hello-queues-article"></a><span data-ttu-id="0b053-181">Související témata uvedených v článku fronty hello</span><span class="sxs-lookup"><span data-stu-id="0b053-181">Related topics covered by hello queues article</span></span>
<span data-ttu-id="0b053-182">Informace o tom, jak zpracování objektů blob toohandle aktivaci zprávu fronty, a to pro webové úlohy scénáře SDK není konkrétní tooblob zpracování, najdete v části [jak toouse Azure fronty úložiště s hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="0b053-182">For information about how toohandle blob processing triggered by a queue message, or for WebJobs SDK scenarios not specific tooblob processing, see [How toouse Azure queue storage with hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span>

<span data-ttu-id="0b053-183">Související témata popsaná v tomto článku hello následující:</span><span class="sxs-lookup"><span data-stu-id="0b053-183">Related topics covered in that article include hello following:</span></span>

* <span data-ttu-id="0b053-184">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="0b053-184">Async functions</span></span>
* <span data-ttu-id="0b053-185">Více instancí</span><span class="sxs-lookup"><span data-stu-id="0b053-185">Multiple instances</span></span>
* <span data-ttu-id="0b053-186">Řádné vypnutí</span><span class="sxs-lookup"><span data-stu-id="0b053-186">Graceful shutdown</span></span>
* <span data-ttu-id="0b053-187">Použití atributů WebJobs SDK v hello tělo funkce</span><span class="sxs-lookup"><span data-stu-id="0b053-187">Use WebJobs SDK attributes in hello body of a function</span></span>
* <span data-ttu-id="0b053-188">Nastavit hello SDK připojovacích řetězců v kódu.</span><span class="sxs-lookup"><span data-stu-id="0b053-188">Set hello SDK connection strings in code.</span></span>
* <span data-ttu-id="0b053-189">Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu</span><span class="sxs-lookup"><span data-stu-id="0b053-189">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="0b053-190">Konfigurace **MaxDequeueCount** pro zpracování poškozených objektů blob.</span><span class="sxs-lookup"><span data-stu-id="0b053-190">Configure **MaxDequeueCount** for poison blob handling.</span></span>
* <span data-ttu-id="0b053-191">Ruční aktivaci funkce</span><span class="sxs-lookup"><span data-stu-id="0b053-191">Trigger a function manually</span></span>
* <span data-ttu-id="0b053-192">Zápis protokolů</span><span class="sxs-lookup"><span data-stu-id="0b053-192">Write logs</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b053-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0b053-193">Next steps</span></span>
<span data-ttu-id="0b053-194">Tento článek poskytl ukázky kódu, které zobrazují jak objekty BLOB toohandle běžné scénáře pro práci s Azure.</span><span class="sxs-lookup"><span data-stu-id="0b053-194">This article has provided code samples that show how toohandle common scenarios for working with Azure blobs.</span></span> <span data-ttu-id="0b053-195">Další informace o tom, jak toouse Azure WebJobs a hello WebJobs SDK najdete v části [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="0b053-195">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

