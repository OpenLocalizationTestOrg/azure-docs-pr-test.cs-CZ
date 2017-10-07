---
title: "aaaCreate kódování úloh služby Azure Media Services, který generuje fMP4 bloky | Microsoft Docs"
description: "Toto téma ukazuje, jak toocreate kódování úloh, který generuje fMP4 bloků. Když tato úloha je používána s hello Media Encoder Standard nebo kodéru Media Encoder Premium pracovního postupu, bude obsahovat hello výstupní asset fMP4 bloků místo soubory ISO MP4."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: b7029ac5-eadd-4a2f-8111-1fc460828981
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 388f3ccb9865b5c4e159af86d5a9ee2f4e3f6120
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#  <a name="create-an-encoding-task-that-generates-fmp4-chunks"></a><span data-ttu-id="972b4-104">Vytvoření kódování úlohy, který generuje fMP4 bloky</span><span class="sxs-lookup"><span data-stu-id="972b4-104">Create an encoding task that generates fMP4 chunks</span></span>

## <a name="overview"></a><span data-ttu-id="972b4-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="972b4-105">Overview</span></span>

<span data-ttu-id="972b4-106">Toto téma ukazuje, jak toocreate kódování úloh, který generuje fragmentovaný soubor MP4 bloky dat (fMP4) namísto soubory ISO MP4.</span><span class="sxs-lookup"><span data-stu-id="972b4-106">This topic shows how toocreate an encoding task that generates fragmented MP4 (fMP4) chunks instead of ISO MP4 files.</span></span> <span data-ttu-id="972b4-107">toogenerate fMP4 bloky dat, použijte hello **Media Encoder Standard** nebo **Media Encoder Premium pracovního postupu** toocreate kodér kódování úlohy a také určit  **AssetFormatOption.AdaptiveStreaming** možnost, jak ukazuje tento fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="972b4-107">toogenerate fMP4 chunks, use hello **Media Encoder Standard** or **Media Encoder Premium Workflow** encoder toocreate an encoding task and also specify **AssetFormatOption.AdaptiveStreaming** option, as shown in this code snippet:</span></span>  
    
    task.OutputAssets.AddNew(@"Output Asset containing fMP4 chunks", 
            options: AssetCreationOptions.None, 
            formatOption: AssetFormatOption.AdaptiveStreaming);


## <span data-ttu-id="972b4-108"><a id="encoding_with_dotnet"></a>Kódování pomocí služby Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="972b4-108"><a id="encoding_with_dotnet"></a>Encoding with Media Services .NET SDK</span></span>

<span data-ttu-id="972b4-109">Následující ukázka kódu Hello používá sadu Media Services .NET SDK tooperform hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="972b4-109">hello following code example uses Media Services .NET SDK tooperform hello following tasks:</span></span>

- <span data-ttu-id="972b4-110">Vytvořte úlohu kódování.</span><span class="sxs-lookup"><span data-stu-id="972b4-110">Create an encoding job.</span></span>
- <span data-ttu-id="972b4-111">Získat odkaz na toohello **Media Encoder Standard** kodér.</span><span class="sxs-lookup"><span data-stu-id="972b4-111">Get a reference toohello **Media Encoder Standard** encoder.</span></span>
- <span data-ttu-id="972b4-112">Přidat úlohu kódování toohello úloh a zadejte toouse hello **adaptivní datové proudy** přednastavené.</span><span class="sxs-lookup"><span data-stu-id="972b4-112">Add an encoding task toohello job and specify toouse hello **Adaptive Streaming** preset.</span></span> 
- <span data-ttu-id="972b4-113">Vytvoření výstupní asset, který bude obsahovat fMP4 bloky dat a soubor .ism.</span><span class="sxs-lookup"><span data-stu-id="972b4-113">Create an output asset that will contain fMP4 chunks and an .ism file.</span></span>
- <span data-ttu-id="972b4-114">Přidejte průběh úlohy toocheck hello událost obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="972b4-114">Add an event handler toocheck hello job progress.</span></span>
- <span data-ttu-id="972b4-115">Odešlete úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="972b4-115">Submit hello job.</span></span>

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="972b4-116">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="972b4-116">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="972b4-117">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="972b4-117">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="972b4-118">Příklad</span><span class="sxs-lookup"><span data-stu-id="972b4-118">Example</span></span>

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;

    namespace AdaptiveStreaming
    {
        class Program
        {
        // Read values from hello App.config file.
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

            // Get an uploaded asset.
            var asset = _context.Assets.FirstOrDefault();

            // Encode and generate hello output using hello "Adaptive Streaming" preset.
            EncodeToAdaptiveBitrateMP4Set(asset);

            Console.ReadLine();
        }
        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");

            // Get a media processor reference, and pass tooit hello name of hello 
            // processor toouse for hello specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify hello input asset toobe encoded.
            task.InputAssets.Add(asset);

            // Add an output asset toocontain hello results of hello job. 

            // This output is specified as AssetCreationOptions.None, which 
            // means hello output asset is not encrypted. 
            // It is also specified toouse AssetFormatOption.AdaptiveStreaming, 
            // which means hello output asset will contain fMP4 chunks.

            task.OutputAssets.AddNew(@"Output Asset containing fMP4 chunks",
            options: AssetCreationOptions.None,
            formatOption: AssetFormatOption.AdaptiveStreaming);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }
        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
            case JobState.Finished:
                Console.WriteLine();
                Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                break;
            default:
                break;
            }
        }
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }
        }
    }

## <a name="media-services-learning-paths"></a><span data-ttu-id="972b4-119">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="972b4-119">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="972b4-120">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="972b4-120">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="972b4-121">Viz také</span><span class="sxs-lookup"><span data-stu-id="972b4-121">See Also</span></span>
[<span data-ttu-id="972b4-122">Kódování Přehled služby Media Services</span><span class="sxs-lookup"><span data-stu-id="972b4-122">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)

