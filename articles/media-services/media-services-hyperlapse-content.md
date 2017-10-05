---
title: "Zpracování mediálních souborů pomocí Azure Media Hyperlapse | Microsoft Docs"
description: "Azure Media Hyperlapse vytvoří smooth vypršelo čas videa z první, kdo nebo akce fotoaparát obsah. Toto téma ukazuje způsob použití Media Indexer."
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
ms.openlocfilehash: 02f634c2af04b6b372642ab0e6a17a5d29f16450
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a><span data-ttu-id="adde3-104">Zpracování mediálních souborů pomocí Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="adde3-104">Hyperlapse Media Files with Azure Media Hyperlapse</span></span>
<span data-ttu-id="adde3-105">Azure Media Hyperlapse je média procesoru (PP) vytvářející smooth vypršelo čas videa z první osoba nebo akce fotoaparát obsahu.</span><span class="sxs-lookup"><span data-stu-id="adde3-105">Azure Media Hyperlapse is a Media Processor (MP) that creates smooth time-lapsed videos from first-person or action-camera content.</span></span>  <span data-ttu-id="adde3-106">Na stejné úrovni cloudu k [plochy Hyperlapse Pro a mobilního telefonu Hyperlapse Microsoft Research](http://aka.ms/hyperlapse), Microsoft Hyperlapse pro službu Azure Media Services využívá masivním měřítku na platformě Azure Media Services média zpracování vodorovně škálování a paralelní hromadné Hyperlapse zpracování.</span><span class="sxs-lookup"><span data-stu-id="adde3-106">The cloud-based sibling to [Microsoft Research's desktop Hyperlapse Pro and phone-based Hyperlapse Mobile](http://aka.ms/hyperlapse), Microsoft Hyperlapse for Azure Media Services utilizes the massive scale of the Azure Media Services Media Processing platform to horizontally scale and parallelize bulk Hyperlapse processing.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="adde3-107">Microsoft Hyperlapse slouží nejlépe pracovat na první, kdo obsahu s přesunutí fotoaparát.</span><span class="sxs-lookup"><span data-stu-id="adde3-107">Microsoft Hyperlapse is designed to work best on first-person content with a moving camera.</span></span>  <span data-ttu-id="adde3-108">I když stále kamer můžete i nadále fungovat, výkonu a kvalitě procesor médií Azure Media Hyperlapse nemůže zaručit pro jiné typy obsahu.</span><span class="sxs-lookup"><span data-stu-id="adde3-108">Although still-camera footage can still work, the performance and quality of the Azure Media Hyperlapse Media Processor cannot be guaranteed for other types of content.</span></span>  <span data-ttu-id="adde3-109">Další informace o Microsoft Hyperlapse pro Azure Media Services a zobrazit některé ukázkové video, podívejte se [úvodní příspěvku na blogu](http://aka.ms/azurehyperlapseblog) z verze public preview.</span><span class="sxs-lookup"><span data-stu-id="adde3-109">To learn more about Microsoft Hyperlapse for Azure Media Services and see some example videos, check out the [introductory blog post](http://aka.ms/azurehyperlapseblog) from the public preview.</span></span>
> 
> 

<span data-ttu-id="adde3-110">Azure Media Hyperlapse úlohy vezme jako vstupní asset soubor MP4, MOV nebo WMV společně s konfiguračního souboru, který určuje, které snímky video by měla být vypršelo čas a jaké rychlosti (například první 10 000 rámců na 2 x).</span><span class="sxs-lookup"><span data-stu-id="adde3-110">An Azure Media Hyperlapse job takes as input an MP4, MOV, or WMV asset file along with a configuration file that specifies which frames of video should be time-lapsed and to what speed (e.g. first 10,000 frames at 2x).</span></span>  <span data-ttu-id="adde3-111">Výstup je stabilizované a čas vypršelo interpretace vstupu videa.</span><span class="sxs-lookup"><span data-stu-id="adde3-111">The output is a stabilized and time-lapsed rendition of the input video.</span></span>

<span data-ttu-id="adde3-112">Nejnovější aktualizace Azure Media Hyperlapse, najdete v části [Media Services blogy](https://azure.microsoft.com/blog/topics/media-services/).</span><span class="sxs-lookup"><span data-stu-id="adde3-112">For the latest Azure Media Hyperlapse updates, see [Media Services blogs](https://azure.microsoft.com/blog/topics/media-services/).</span></span>

## <a name="hyperlapse-an-asset"></a><span data-ttu-id="adde3-113">Hyperlapse prostředek</span><span class="sxs-lookup"><span data-stu-id="adde3-113">Hyperlapse an asset</span></span>
<span data-ttu-id="adde3-114">Nejdřív je potřeba nahrát požadované vstupní soubor k Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="adde3-114">First you will need to upload your desired input file to Azure Media Services.</span></span>  <span data-ttu-id="adde3-115">Další informace o konceptech, které jsou spojené s odesílání a správu obsahu, najdete [správy obsahu článku](media-services-portal-vod-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="adde3-115">To learn more about the concepts involved with uploading and managing content, read the [content management article](media-services-portal-vod-get-started.md).</span></span>

### <span data-ttu-id="adde3-116"><a id="configuration"></a>Konfigurace předvolbu pro Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="adde3-116"><a id="configuration"></a>Configuration Preset for Hyperlapse</span></span>
<span data-ttu-id="adde3-117">Jakmile vašeho obsahu v váš účet Media Services, musíte vytvořit přednastavených vaší konfigurace.</span><span class="sxs-lookup"><span data-stu-id="adde3-117">Once your content is in your Media Services account, you will need to construct your configuration preset.</span></span>  <span data-ttu-id="adde3-118">Následující tabulka vysvětluje pole definované uživatelem:</span><span class="sxs-lookup"><span data-stu-id="adde3-118">The following table explains the user-specified fields:</span></span>

| <span data-ttu-id="adde3-119">Pole</span><span class="sxs-lookup"><span data-stu-id="adde3-119">Field</span></span> | <span data-ttu-id="adde3-120">Popis</span><span class="sxs-lookup"><span data-stu-id="adde3-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="adde3-121">StartFrame</span><span class="sxs-lookup"><span data-stu-id="adde3-121">StartFrame</span></span> |<span data-ttu-id="adde3-122">Rámečku, na kterém by měl začínat Microsoft Hyperlapse zpracování.</span><span class="sxs-lookup"><span data-stu-id="adde3-122">The frame upon which the Microsoft Hyperlapse processing should begin.</span></span> |
| <span data-ttu-id="adde3-123">NumFrames</span><span class="sxs-lookup"><span data-stu-id="adde3-123">NumFrames</span></span> |<span data-ttu-id="adde3-124">Počet snímků při zpracování</span><span class="sxs-lookup"><span data-stu-id="adde3-124">The number of frames to process</span></span> |
| <span data-ttu-id="adde3-125">Rychlost</span><span class="sxs-lookup"><span data-stu-id="adde3-125">Speed</span></span> |<span data-ttu-id="adde3-126">Faktor, ke které má být urychlení vstupní video.</span><span class="sxs-lookup"><span data-stu-id="adde3-126">The factor with which to speed up the input video.</span></span> |

<span data-ttu-id="adde3-127">Následuje příklad vyhovující konfiguračního souboru XML a JSON:</span><span class="sxs-lookup"><span data-stu-id="adde3-127">The following is an example of a conformant configuration file in XML and JSON:</span></span>

<span data-ttu-id="adde3-128">**Přednastavení XML:**</span><span class="sxs-lookup"><span data-stu-id="adde3-128">**XML preset:**</span></span>

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

<span data-ttu-id="adde3-129">**Přednastavení JSON:**</span><span class="sxs-lookup"><span data-stu-id="adde3-129">**JSON preset:**</span></span>

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

### <span data-ttu-id="adde3-130"><a id="sample_code"></a>Microsoft Hyperlapse pomocí .NET SDK služby AMS</span><span class="sxs-lookup"><span data-stu-id="adde3-130"><a id="sample_code"></a> Microsoft Hyperlapse with the AMS .NET SDK</span></span>
<span data-ttu-id="adde3-131">Následující metoda odešle soubor média jako prostředek a vytvoří úlohu s procesorem Azure Media Hyperlapse média.</span><span class="sxs-lookup"><span data-stu-id="adde3-131">The following method uploads a media file as an asset and creates a job with the Azure Media Hyperlapse Media Processor.</span></span>

> [!NOTE]
> <span data-ttu-id="adde3-132">CloudMediaContext byste již měli mít v oboru s názvem "context" pro tento kód pracovat.</span><span class="sxs-lookup"><span data-stu-id="adde3-132">You should already have a CloudMediaContext in scope with the name "context" for this code to work.</span></span>  <span data-ttu-id="adde3-133">Další informace o tom, najdete [správy obsahu článku](media-services-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="adde3-133">To learn more about this, read the [content management article](media-services-dotnet-get-started.md).</span></span>
> 
> [!NOTE]
> <span data-ttu-id="adde3-134">Argument řetězce "hyperConfig" musí být konfiguraci vyhovující přednastavení v XML nebo JSON, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="adde3-134">The string argument "hyperConfig" is expected to be a conformant configuration preset in either JSON or XML as described above.</span></span>
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

            // If job state is Error, the event handling
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

### <span data-ttu-id="adde3-135"><a id="file_types"></a>Podporované typy souborů</span><span class="sxs-lookup"><span data-stu-id="adde3-135"><a id="file_types"></a>Supported File types</span></span>
* <span data-ttu-id="adde3-136">MP4</span><span class="sxs-lookup"><span data-stu-id="adde3-136">MP4</span></span>
* <span data-ttu-id="adde3-137">MOV</span><span class="sxs-lookup"><span data-stu-id="adde3-137">MOV</span></span>
* <span data-ttu-id="adde3-138">WMV</span><span class="sxs-lookup"><span data-stu-id="adde3-138">WMV</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="adde3-139">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="adde3-139">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="adde3-140">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="adde3-140">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="adde3-141">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="adde3-141">Related links</span></span>
[<span data-ttu-id="adde3-142">Azure Media Services Analytics – přehled</span><span class="sxs-lookup"><span data-stu-id="adde3-142">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="adde3-143">Ukázky služby Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="adde3-143">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

