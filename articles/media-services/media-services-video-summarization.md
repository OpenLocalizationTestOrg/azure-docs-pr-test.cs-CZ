---
title: "Použít miniatur videa Azure Media k vytvoření Videosouhrn | Microsoft Docs"
description: "Videosouhrn vám pomůžou vytvořit souhrnných informací o dlouho videa automaticky výběrem zajímavé fragmenty kódu z zdroj videa. To je užitečné, pokud byste chtěli poskytnout rychlý přehled toho, co očekávat při dlouhé video."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: a245529f-3150-4afc-93ec-e40d8a6b761d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/18/2017
ms.author: milanga;juliako;
ms.openlocfilehash: 5d5afdaf22ffea8f3b77a154acb5d0a8dda74405
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-media-video-thumbnails-to-create-a-video-summarization"></a><span data-ttu-id="6f300-104">Použít miniatur videa Azure Media k vytvoření Videosouhrn</span><span class="sxs-lookup"><span data-stu-id="6f300-104">Use Azure Media Video Thumbnails to Create a Video Summarization</span></span>
## <a name="overview"></a><span data-ttu-id="6f300-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="6f300-105">Overview</span></span>
<span data-ttu-id="6f300-106">**Miniatur videa Azure Media** procesor médií (PP) můžete vytvořit souhrn video, které jsou užitečné pro zákazníky, kteří právě chcete zobrazit náhled souhrn dlouhé video.</span><span class="sxs-lookup"><span data-stu-id="6f300-106">The **Azure Media Video Thumbnails** media processor (MP) enables you to create a summary of a video that is useful to customers who just want to preview a summary of a long video.</span></span> <span data-ttu-id="6f300-107">Například mohou zákazníci chtějí krátké "souhrn video" v tématu kdy myší na miniaturu.</span><span class="sxs-lookup"><span data-stu-id="6f300-107">For example, customers might want to see a short "summary video" when they hover over a thumbnail.</span></span> <span data-ttu-id="6f300-108">Podle postupně je upravujte parametry **miniatur videa v Azure Media** prostřednictvím konfigurace přednastavení, můžete použít sady MP výkonná snímek zřetězení a zjišťování technologie algorithmically generovat popisný subclip.</span><span class="sxs-lookup"><span data-stu-id="6f300-108">By tweaking the parameters of **Azure Media Video Thumbnails** through a configuration preset, you can use the MP's powerful shot detection and concatenation technology to algorithmically generate a descriptive subclip.</span></span>  

<span data-ttu-id="6f300-109">**Azure Media Video miniaturu** MP je aktuálně ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="6f300-109">The **Azure Media Video Thumbnail** MP is currently in Preview.</span></span>

<span data-ttu-id="6f300-110">Toto téma uvádí podrobnosti o **Azure Media Video miniaturu** a ukazuje, jak pomocí sady Media Services SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="6f300-110">This topic gives details about  **Azure Media Video Thumbnail** and shows how to use it with Media Services SDK for .NET.</span></span>

## <a name="limitations"></a><span data-ttu-id="6f300-111">Omezení</span><span class="sxs-lookup"><span data-stu-id="6f300-111">Limitations</span></span>

<span data-ttu-id="6f300-112">V některých případech Pokud videa není skládá z různých scény výstup bude pouze jeden snímek.</span><span class="sxs-lookup"><span data-stu-id="6f300-112">In some cases, if your video is not comprised of different scenes, the output will only be a single shot.</span></span>

## <a name="video-summary-example"></a><span data-ttu-id="6f300-113">Příklad videa souhrn</span><span class="sxs-lookup"><span data-stu-id="6f300-113">Video summary example</span></span>
<span data-ttu-id="6f300-114">Tady jsou některé příklady co můžete dělat procesor médií miniatur videa v Azure média:</span><span class="sxs-lookup"><span data-stu-id="6f300-114">Here are some examples of what the Azure Media Video Thumbnails media processor can do:</span></span>

### <a name="original-video"></a><span data-ttu-id="6f300-115">Původní video</span><span class="sxs-lookup"><span data-stu-id="6f300-115">Original video</span></span>
[<span data-ttu-id="6f300-116">Původní video</span><span class="sxs-lookup"><span data-stu-id="6f300-116">Original video</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Faed33834-ec2d-4788-88b5-a4505b3d032c%2FMicrosoft%27s%20HoloLens%20Live%20Demonstration.ism%2Fmanifest)

### <a name="video-thumbnail-result"></a><span data-ttu-id="6f300-117">Video miniatur výsledek</span><span class="sxs-lookup"><span data-stu-id="6f300-117">Video thumbnail result</span></span>
[<span data-ttu-id="6f300-118">Video miniatur výsledek</span><span class="sxs-lookup"><span data-stu-id="6f300-118">Video thumbnail result</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Ff5c91052-4232-41d4-b531-062e07b6a9ae%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

## <a name="task-configuration-preset"></a><span data-ttu-id="6f300-119">Konfigurace úlohy (přednastavených)</span><span class="sxs-lookup"><span data-stu-id="6f300-119">Task configuration (preset)</span></span>
<span data-ttu-id="6f300-120">Při vytváření miniatur videa úlohy s **miniatur videa Azure Media**, je nutné zadat jedno z přednastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6f300-120">When creating a video thumbnail task with **Azure Media Video Thumbnails**, you must specify a configuration preset.</span></span> <span data-ttu-id="6f300-121">Ukázka výše miniatur byl vytvořen s následující konfigurací základní JSON:</span><span class="sxs-lookup"><span data-stu-id="6f300-121">The above thumbnail sample was created with the following basic JSON configuration:</span></span>

    {"version":"1.0"}

<span data-ttu-id="6f300-122">V současné době můžete změnit následující parametry:</span><span class="sxs-lookup"><span data-stu-id="6f300-122">Currently, you can change the following parameters:</span></span>

| <span data-ttu-id="6f300-123">Param</span><span class="sxs-lookup"><span data-stu-id="6f300-123">Param</span></span> | <span data-ttu-id="6f300-124">Popis</span><span class="sxs-lookup"><span data-stu-id="6f300-124">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6f300-125">outputAudio</span><span class="sxs-lookup"><span data-stu-id="6f300-125">outputAudio</span></span> |<span data-ttu-id="6f300-126">Určuje, zda výsledné video obsahuje všechny zvukovém souboru.</span><span class="sxs-lookup"><span data-stu-id="6f300-126">Specifies whether or not the resultant video contains any audio.</span></span> <br/><span data-ttu-id="6f300-127">Povolené hodnoty jsou: True nebo False.</span><span class="sxs-lookup"><span data-stu-id="6f300-127">Allowed values are: True or False.</span></span> <span data-ttu-id="6f300-128">Výchozí hodnota je True.</span><span class="sxs-lookup"><span data-stu-id="6f300-128">Default is True.</span></span> |
| <span data-ttu-id="6f300-129">fadeInFadeOut</span><span class="sxs-lookup"><span data-stu-id="6f300-129">fadeInFadeOut</span></span> |<span data-ttu-id="6f300-130">Určuje, jestli Objevování přejde se používají mezi samostatnou pohybu miniatur.</span><span class="sxs-lookup"><span data-stu-id="6f300-130">Specifies whether or not fade transitions are used between the separate motion thumbnails.</span></span>  <br/><span data-ttu-id="6f300-131">Povolené hodnoty jsou: True nebo False.</span><span class="sxs-lookup"><span data-stu-id="6f300-131">Allowed values are: True or False.</span></span>  <span data-ttu-id="6f300-132">Výchozí hodnota je True.</span><span class="sxs-lookup"><span data-stu-id="6f300-132">Default is True.</span></span> |
| <span data-ttu-id="6f300-133">maxMotionThumbnailDurationInSecs</span><span class="sxs-lookup"><span data-stu-id="6f300-133">maxMotionThumbnailDurationInSecs</span></span> |<span data-ttu-id="6f300-134">Celé číslo, které určuje, jak dlouho musí být celé výsledné video.</span><span class="sxs-lookup"><span data-stu-id="6f300-134">Integer that specifies how long the entire resultant video shall be.</span></span>  <span data-ttu-id="6f300-135">Výchozí hodnota závisí na původní video doba trvání.</span><span class="sxs-lookup"><span data-stu-id="6f300-135">Default depends on original video duration.</span></span> |

<span data-ttu-id="6f300-136">Následující tabulka popisuje výchozí doba, kdy **maxMotionThumbnailInSecs** nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="6f300-136">The following table describes the default duration, when **maxMotionThumbnailInSecs** is not used.</span></span>

|  |  |  |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="6f300-137">Video doba trvání</span><span class="sxs-lookup"><span data-stu-id="6f300-137">Video duration</span></span> |<span data-ttu-id="6f300-138">d < 3 min</span><span class="sxs-lookup"><span data-stu-id="6f300-138">d < 3 min</span></span> |<span data-ttu-id="6f300-139">3 min < d < 15 minut</span><span class="sxs-lookup"><span data-stu-id="6f300-139">3 min < d < 15 min</span></span> |
| <span data-ttu-id="6f300-140">Doba trvání miniatur</span><span class="sxs-lookup"><span data-stu-id="6f300-140">Thumbnail duration</span></span> |<span data-ttu-id="6f300-141">15 sekundu (scény 2-3)</span><span class="sxs-lookup"><span data-stu-id="6f300-141">15 sec (2-3 scenes)</span></span> |<span data-ttu-id="6f300-142">30 sekund (scény 3 až 5)</span><span class="sxs-lookup"><span data-stu-id="6f300-142">30 sec (3-5 scenes)</span></span> |

<span data-ttu-id="6f300-143">Následujícím kódu JSON nastaví dostupné parametry.</span><span class="sxs-lookup"><span data-stu-id="6f300-143">The following JSON sets available parameters.</span></span>

    {
        "version": "1.0",
        "options": {
            "outputAudio": "true",
            "maxMotionThumbnailDurationInSecs": "10",
            "fadeInFadeOut": "true"
        }
    }

## <a name="net-sample-code"></a><span data-ttu-id="6f300-144">Ukázkový kód rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="6f300-144">.NET sample code</span></span>

<span data-ttu-id="6f300-145">Program zobrazí následující postup:</span><span class="sxs-lookup"><span data-stu-id="6f300-145">The following program shows how to:</span></span>

1. <span data-ttu-id="6f300-146">Vytvořte asset a nahrajte soubor média do assetu.</span><span class="sxs-lookup"><span data-stu-id="6f300-146">Create an asset and upload a media file into the asset.</span></span>
2. <span data-ttu-id="6f300-147">Vytvoří úlohu s video miniatur úloh podle konfigurační soubor, který obsahuje následující přednastavení json.</span><span class="sxs-lookup"><span data-stu-id="6f300-147">Creates a job with a video thumbnail task based on a configuration file that contains the following json preset.</span></span> 
   
        {                
            "version": "1.0",
            "options": {
                "outputAudio": "true",
                "maxMotionThumbnailDurationInSecs": "30",
                "fadeInFadeOut": "false"
            }
        }
3. <span data-ttu-id="6f300-148">Výstupní soubory stáhne.</span><span class="sxs-lookup"><span data-stu-id="6f300-148">Downloads the output files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="6f300-149">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="6f300-149">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="6f300-150">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="6f300-150">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="6f300-151">Příklad</span><span class="sxs-lookup"><span data-stu-id="6f300-151">Example</span></span>

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace VideoSummarization
    {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _AADTenantDomain =
                ConfigurationManager.AppSettings["AADTenantDomain"];
            private static readonly string _RESTAPIEndpoint =
                ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

            // Field for service context.
            private static CloudMediaContext _context = null;

            static void Main(string[] args)
            {
                var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);


                // Run the thumbnail job.
                var asset = RunVideoThumbnailJob(@"C:\supportFiles\VideoThumbnail\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoThumbnail\config.json");

                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoThumbnail\Output");
            }

            static IAsset RunVideoThumbnailJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Thumbnail Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Thumbnail Job");

                // Get a reference to Azure Media Video Thumbnails.
                string MediaProcessorName = "Azure Media Video Thumbnails";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Thumbnail Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify the input asset.
                task.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Video Thumbnail Output Asset", AssetCreationOptions.None);

                // Use the following event handler to check job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

                // Launch the job.
                job.Submit();

                // Check job execution and wait for job to finish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);

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
                    return null;
                }

                return job.OutputMediaAssets[0];
            }

            static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
            {
                IAsset asset = _context.Assets.Create(assetName, options);

                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                assetFile.Upload(filePath);

                return asset;
            }

            static void DownloadAsset(IAsset asset, string outputDirectory)
            {
                foreach (IAssetFile file in asset.AssetFiles)
                {
                    file.Download(Path.Combine(outputDirectory, file.Name));
                }
            }

            static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors
                    .Where(p => p.Name == mediaProcessorName)
                    .ToList()
                    .OrderBy(p => new Version(p.Version))
                    .LastOrDefault();

                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor",
                                                               mediaProcessorName));

                return processor;
            }

            static private void StateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);

                switch (e.CurrentState)
                {
                    case JobState.Finished:
                        Console.WriteLine();
                        Console.WriteLine("Job is finished.");
                        Console.WriteLine();
                        break;
                    case JobState.Canceling:
                    case JobState.Queued:
                    case JobState.Scheduled:
                    case JobState.Processing:
                        Console.WriteLine("Please wait...\n");
                        break;
                    case JobState.Canceled:
                    case JobState.Error:
                        // Cast sender as a job.
                        IJob job = (IJob)sender;
                        // Display or log error details as needed.
                        // LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }

        }
    }

### <a name="video-thumbnail-output"></a><span data-ttu-id="6f300-152">Miniatury výstupu videa</span><span class="sxs-lookup"><span data-stu-id="6f300-152">Video thumbnail output</span></span>
[<span data-ttu-id="6f300-153">Miniatury výstupu videa</span><span class="sxs-lookup"><span data-stu-id="6f300-153">Video thumbnail output</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Fd06f24dc-bc81-488e-a8d0-348b7dc41b56%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

## <a name="media-services-learning-paths"></a><span data-ttu-id="6f300-154">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="6f300-154">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6f300-155">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="6f300-155">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="6f300-156">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="6f300-156">Related links</span></span>
[<span data-ttu-id="6f300-157">Azure Media Services Analytics – přehled</span><span class="sxs-lookup"><span data-stu-id="6f300-157">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="6f300-158">Ukázky služby Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="6f300-158">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

