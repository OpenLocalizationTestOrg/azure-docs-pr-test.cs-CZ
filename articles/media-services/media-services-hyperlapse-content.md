---
title: "aaaHyperlapse mediálních souborů pomocí Azure Media Hyperlapse | Microsoft Docs"
description: "Azure Media Hyperlapse vytvoří smooth vypršelo čas videa z první, kdo nebo akce fotoaparát obsah. Toto téma ukazuje, jak toouse Media Indexer."
services: media-services
documentationcenter: 
author: asolanki
manager: johndeu
editor: 
ms.assetid: 37d54db6-9cf3-4ae9-b3c6-0d29c744e965
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/02/2017
ms.author: adsolank
ms.openlocfilehash: 85bb07206d0ca2f5b2fd0767e6ed4904195d3ab6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a><span data-ttu-id="85787-104">Zpracování mediálních souborů pomocí Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="85787-104">Hyperlapse Media Files with Azure Media Hyperlapse</span></span>
<span data-ttu-id="85787-105">Azure Media Hyperlapse je média procesoru (PP) vytvářející smooth vypršelo čas videa z první osoba nebo akce fotoaparát obsahu.</span><span class="sxs-lookup"><span data-stu-id="85787-105">Azure Media Hyperlapse is a Media Processor (MP) that creates smooth time-lapsed videos from first-person or action-camera content.</span></span>  <span data-ttu-id="85787-106">Hello založené na cloudu na stejné úrovni příliš[plochy Hyperlapse Pro a mobilního telefonu Hyperlapse Microsoft Research](http://aka.ms/hyperlapse), Hyperlapse Microsoft pro službu Azure Media Services využívá hello masivní škálování hello Azure Media Services média Zpracování platformy toohorizontally škálování a paralelní hromadné Hyperlapse zpracování.</span><span class="sxs-lookup"><span data-stu-id="85787-106">hello cloud-based sibling too[Microsoft Research's desktop Hyperlapse Pro and phone-based Hyperlapse Mobile](http://aka.ms/hyperlapse), Microsoft Hyperlapse for Azure Media Services utilizes hello massive scale of hello Azure Media Services Media Processing platform toohorizontally scale and parallelize bulk Hyperlapse processing.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="85787-107">Microsoft Hyperlapse je navrženou toowork nejlepší na první, kdo obsah s přesunutí fotoaparát.</span><span class="sxs-lookup"><span data-stu-id="85787-107">Microsoft Hyperlapse is designed toowork best on first-person content with a moving camera.</span></span>  <span data-ttu-id="85787-108">I když stále kamer můžete i nadále fungovat, hello výkonu a kvalitě z hello procesor Azure Media Hyperlapse médií nemůže zaručit pro jiné typy obsahu.</span><span class="sxs-lookup"><span data-stu-id="85787-108">Although still-camera footage can still work, hello performance and quality of hello Azure Media Hyperlapse Media Processor cannot be guaranteed for other types of content.</span></span>  <span data-ttu-id="85787-109">Další informace o Microsoft Hyperlapse pro Azure Media Services toolearn a najdete některé ukázkové video, podívejte se na hello [úvodní příspěvku na blogu](http://aka.ms/azurehyperlapseblog) z hello verzi public preview.</span><span class="sxs-lookup"><span data-stu-id="85787-109">toolearn more about Microsoft Hyperlapse for Azure Media Services and see some example videos, check out hello [introductory blog post](http://aka.ms/azurehyperlapseblog) from hello public preview.</span></span>
> 
> 

<span data-ttu-id="85787-110">Azure Media Hyperlapse úlohy vezme jako vstupní soubor MP4, MOV nebo WMV asset spolu s konfigurační soubor, který určuje, které snímky video by měla být vypršelo čas a toowhat rychlosti (například první 10 000 rámců na 2 x).</span><span class="sxs-lookup"><span data-stu-id="85787-110">An Azure Media Hyperlapse job takes as input an MP4, MOV, or WMV asset file along with a configuration file that specifies which frames of video should be time-lapsed and toowhat speed (e.g. first 10,000 frames at 2x).</span></span>  <span data-ttu-id="85787-111">výstup Hello je stabilizované a čas vypršelo interpretace hello vstupní videa.</span><span class="sxs-lookup"><span data-stu-id="85787-111">hello output is a stabilized and time-lapsed rendition of hello input video.</span></span>

<span data-ttu-id="85787-112">Nejnovější aktualizace Azure Media Hyperlapse hello najdete v části [Media Services blogy](https://azure.microsoft.com/blog/topics/media-services/).</span><span class="sxs-lookup"><span data-stu-id="85787-112">For hello latest Azure Media Hyperlapse updates, see [Media Services blogs](https://azure.microsoft.com/blog/topics/media-services/).</span></span>

## <a name="hyperlapse-an-asset"></a><span data-ttu-id="85787-113">Hyperlapse prostředek</span><span class="sxs-lookup"><span data-stu-id="85787-113">Hyperlapse an asset</span></span>
<span data-ttu-id="85787-114">Nejprve budete potřebovat tooupload vaše požadované vstupní soubor tooAzure Media Services.</span><span class="sxs-lookup"><span data-stu-id="85787-114">First you will need tooupload your desired input file tooAzure Media Services.</span></span>  <span data-ttu-id="85787-115">Další informace o toolearn hello koncepty, které jsou spojené s odesílání a správu obsahu, přečtěte si hello [správy obsahu článku](media-services-portal-vod-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="85787-115">toolearn more about hello concepts involved with uploading and managing content, read hello [content management article](media-services-portal-vod-get-started.md).</span></span>

### <span data-ttu-id="85787-116"><a id="configuration"></a>Konfigurace předvolbu pro Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="85787-116"><a id="configuration"></a>Configuration Preset for Hyperlapse</span></span>
<span data-ttu-id="85787-117">Jakmile vašeho obsahu v váš účet Media Services, budete potřebovat tooconstruct přednastavení konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="85787-117">Once your content is in your Media Services account, you will need tooconstruct your configuration preset.</span></span>  <span data-ttu-id="85787-118">Hello následující tabulka vysvětluje hello zadán uživatel pole:</span><span class="sxs-lookup"><span data-stu-id="85787-118">hello following table explains hello user-specified fields:</span></span>

| <span data-ttu-id="85787-119">Pole</span><span class="sxs-lookup"><span data-stu-id="85787-119">Field</span></span> | <span data-ttu-id="85787-120">Popis</span><span class="sxs-lookup"><span data-stu-id="85787-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="85787-121">StartFrame</span><span class="sxs-lookup"><span data-stu-id="85787-121">StartFrame</span></span> |<span data-ttu-id="85787-122">Hello rámečku, při které hello Microsoft Hyperlapse by měl začínat zpracování.</span><span class="sxs-lookup"><span data-stu-id="85787-122">hello frame upon which hello Microsoft Hyperlapse processing should begin.</span></span> |
| <span data-ttu-id="85787-123">NumFrames</span><span class="sxs-lookup"><span data-stu-id="85787-123">NumFrames</span></span> |<span data-ttu-id="85787-124">Hello počet tooprocess rámce</span><span class="sxs-lookup"><span data-stu-id="85787-124">hello number of frames tooprocess</span></span> |
| <span data-ttu-id="85787-125">Rychlost</span><span class="sxs-lookup"><span data-stu-id="85787-125">Speed</span></span> |<span data-ttu-id="85787-126">Hello faktory s které toospeed až vstupní video hello.</span><span class="sxs-lookup"><span data-stu-id="85787-126">hello factor with which toospeed up hello input video.</span></span> |

<span data-ttu-id="85787-127">Hello tady je příklad vyhovující konfiguračního souboru XML a JSON:</span><span class="sxs-lookup"><span data-stu-id="85787-127">hello following is an example of a conformant configuration file in XML and JSON:</span></span>

<span data-ttu-id="85787-128">**Přednastavení XML:**</span><span class="sxs-lookup"><span data-stu-id="85787-128">**XML preset:**</span></span>

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

<span data-ttu-id="85787-129">**Přednastavení JSON:**</span><span class="sxs-lookup"><span data-stu-id="85787-129">**JSON preset:**</span></span>

    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }

### <span data-ttu-id="85787-130"><a id="sample_code"></a>Hyperlapse Microsoft s hello AMS .NET SDK</span><span class="sxs-lookup"><span data-stu-id="85787-130"><a id="sample_code"></a> Microsoft Hyperlapse with hello AMS .NET SDK</span></span>
<span data-ttu-id="85787-131">Hello následující metodu odešle soubor média jako prostředek a vytvoří úlohu s hello procesor Azure Media Hyperlapse médií.</span><span class="sxs-lookup"><span data-stu-id="85787-131">hello following method uploads a media file as an asset and creates a job with hello Azure Media Hyperlapse Media Processor.</span></span>

> [!NOTE]
> <span data-ttu-id="85787-132">V oboru s hello název "context" pro tento kód toowork byste již měli mít CloudMediaContext.</span><span class="sxs-lookup"><span data-stu-id="85787-132">You should already have a CloudMediaContext in scope with hello name "context" for this code toowork.</span></span>  <span data-ttu-id="85787-133">Další informace o této, přečtěte si hello toolearn [správy obsahu článku](media-services-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="85787-133">toolearn more about this, read hello [content management article](media-services-dotnet-get-started.md).</span></span>
> 
> [!NOTE]
> <span data-ttu-id="85787-134">argument řetězce Hello "hyperConfig" je očekávané toobe vyhovující konfigurace přednastavení v JSON nebo XML, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="85787-134">hello string argument "hyperConfig" is expected toobe a conformant configuration preset in either JSON or XML as described above.</span></span>
> 
> 

        static bool RunHyperlapseJob(string input, string output, string hyperConfig)
        {
            // create asset with input file
            IAsset asset = context
            .Assets
            .CreateAssetAndUploadSingleFile(input, "My Hyperlapse Input", AssetCreationOptions.None);

            // grab instances of Azure Media Hyperlapse MP
            IMediaProcessor mp = context
            .MediaProcessors
            .GetLatestMediaProcessorByName("Azure Media Hyperlapse");

            // create Job with Hyperlapse task
            IJob job = context
            .Jobs
            .Create(String.Format("Hyperlapse {0}", input));

            if (String.IsNullOrEmpty(hyperConfig))
            {
            // config cannot be empty
            return false;
            }

            hyperConfig = File.ReadAllText(hyperConfig);

            ITask hyperlapseTask = job.Tasks.AddNew("Hyperlapse task",
            mp,
            hyperConfig,
            TaskOptions.None);
            hyperlapseTask.InputAssets.Add(asset);
            hyperlapseTask.OutputAssets.AddNew("Hyperlapse output",
            AssetCreationOptions.None);

            job.Submit();

            // Create progress printing and querying tasks
            Task progressPrintTask = new Task(() =>
            {

            IJob jobQuery = null;
            do
            {
                var progressContext = context;
                jobQuery = progressContext.Jobs
                .Where(j => j.Id == job.Id)
                .First();
                Console.WriteLine(string.Format("{0}\t{1}\t{2}",
                DateTime.Now,
                jobQuery.State,
                jobQuery.Tasks[0].Progress));
                Thread.Sleep(10000);
            }
            while (jobQuery.State != JobState.Finished &&
                                   jobQuery.State != JobState.Error &&
                                   jobQuery.State != JobState.Canceled);
                });
                
            progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
            progressJobTask.Wait();

            // If job state is Error, hello event handling
            // method for job progress should log errors.  Here we check
            // for error state and exit if needed.
            if (job.State == JobState.Error)
            {
                ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                Console.WriteLine(string.Format("Error: {0}. {1}",
                                                error.Code,
                                                error.Message));  
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }

### <span data-ttu-id="85787-135"><a id="file_types"></a>Podporované typy souborů</span><span class="sxs-lookup"><span data-stu-id="85787-135"><a id="file_types"></a>Supported File types</span></span>
* <span data-ttu-id="85787-136">MP4</span><span class="sxs-lookup"><span data-stu-id="85787-136">MP4</span></span>
* <span data-ttu-id="85787-137">MOV</span><span class="sxs-lookup"><span data-stu-id="85787-137">MOV</span></span>
* <span data-ttu-id="85787-138">WMV</span><span class="sxs-lookup"><span data-stu-id="85787-138">WMV</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="85787-139">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="85787-139">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="85787-140">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="85787-140">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="85787-141">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="85787-141">Related links</span></span>
[<span data-ttu-id="85787-142">Azure Media Services Analytics – přehled</span><span class="sxs-lookup"><span data-stu-id="85787-142">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="85787-143">Ukázky služby Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="85787-143">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

