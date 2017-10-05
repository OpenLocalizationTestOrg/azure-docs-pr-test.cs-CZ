---
title: "Přizpůsobení Media Encoder Standard přednastavení | Microsoft Docs"
description: "Toto téma ukazuje, jak provádět pokročilé kódování přizpůsobením Media Encoder Standard přednastavení úloh. Téma ukazuje, jak používat sadu Media Services .NET SDK k vytvoření úlohy a úlohy kódování. Také ukazuje, jak k poskytování vlastních předvoleb pro úlohy kódování."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ec95392f-d34a-4c22-a6df-5274eaac445b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: juliako
ms.openlocfilehash: b4d25f07349043da8cb745930fde3371c98f9960
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="customizing-media-encoder-standard-presets"></a><span data-ttu-id="9f815-105">Přizpůsobení Media Encoder Standard nastaví aplikace</span><span class="sxs-lookup"><span data-stu-id="9f815-105">Customizing Media Encoder Standard presets</span></span>

## <a name="overview"></a><span data-ttu-id="9f815-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="9f815-106">Overview</span></span>

<span data-ttu-id="9f815-107">Toto téma ukazuje, jak provádět pokročilé kódování s Media Encoder Standard (MES) pomocí vlastní přednastavení.</span><span class="sxs-lookup"><span data-stu-id="9f815-107">This topic shows how to perform advanced encoding with Media Encoder Standard (MES) using a custom preset.</span></span> <span data-ttu-id="9f815-108">Téma používá rozhraní .NET k vytvoření kódování úlohy a úlohy, která spustí tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="9f815-108">The topic uses .NET to create an encoding task and a job that executes this task.</span></span>  

<span data-ttu-id="9f815-109">V tomto tématu se zobrazí postup přizpůsobení přednastavení provedením [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) přednastavení a snížit počet vrstev.</span><span class="sxs-lookup"><span data-stu-id="9f815-109">In this topic you will see how to customize a preset by taking the [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) preset and reducing the number of layers.</span></span> <span data-ttu-id="9f815-110">[Přizpůsobení kodéru Media Encoder Standard přednastavení](media-services-advanced-encoding-with-mes.md) téma ukazuje vlastních předvoleb, které umožňuje provádět pokročilé úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="9f815-110">The [Customizing Media Encoder Standard presets](media-services-advanced-encoding-with-mes.md) topic demonstrates custom presets that can be used to perform advanced encoding tasks.</span></span>

## <span data-ttu-id="9f815-111"><a id="customizing_presets"></a>Přizpůsobení přednastavení MES</span><span class="sxs-lookup"><span data-stu-id="9f815-111"><a id="customizing_presets"></a> Customizing a MES preset</span></span>

### <a name="original-preset"></a><span data-ttu-id="9f815-112">Původní předvolbu</span><span class="sxs-lookup"><span data-stu-id="9f815-112">Original preset</span></span>

<span data-ttu-id="9f815-113">Uložit definované ve formátu JSON [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) téma v některé soubor s příponou .json.</span><span class="sxs-lookup"><span data-stu-id="9f815-113">Save the JSON defined in the [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) topic in some file with .json extension.</span></span> <span data-ttu-id="9f815-114">Například **CustomPreset_JSON.json**.</span><span class="sxs-lookup"><span data-stu-id="9f815-114">For example, **CustomPreset_JSON.json**.</span></span>

### <a name="customized-preset"></a><span data-ttu-id="9f815-115">Přizpůsobené přednastavených</span><span class="sxs-lookup"><span data-stu-id="9f815-115">Customized preset</span></span>

<span data-ttu-id="9f815-116">Otevřete **CustomPreset_JSON.json** souboru a odeberte první tři vrstvy ze **H264Layers** tak váš soubor bude vypadat takto.</span><span class="sxs-lookup"><span data-stu-id="9f815-116">Open the **CustomPreset_JSON.json** file and remove first three layers from **H264Layers** so your file looks like this.</span></span>

    
    {  
      "Version": 1.0,  
      "Codecs": [  
        {  
          "KeyFrameInterval": "00:00:02",  
          "H264Layers": [  
            {  
              "Profile": "Auto",  
              "Level": "auto",  
              "Bitrate": 1000,  
              "MaxBitrate": 1000,  
              "BufferWindow": "00:00:05",  
              "Width": 640,  
              "Height": 360,  
              "BFrames": 3,  
              "ReferenceFrames": 3,  
              "AdaptiveBFrame": true,  
              "Type": "H264Layer",  
              "FrameRate": "0/1"  
            },  
            {  
              "Profile": "Auto",  
              "Level": "auto",  
              "Bitrate": 650,  
              "MaxBitrate": 650,  
              "BufferWindow": "00:00:05",  
              "Width": 640,  
              "Height": 360,  
              "BFrames": 3,  
              "ReferenceFrames": 3,  
              "AdaptiveBFrame": true,  
              "Type": "H264Layer",  
              "FrameRate": "0/1"  
            },  
            {  
              "Profile": "Auto",  
              "Level": "auto",  
              "Bitrate": 400,  
              "MaxBitrate": 400,  
              "BufferWindow": "00:00:05",  
              "Width": 320,  
              "Height": 180,  
              "BFrames": 3,  
              "ReferenceFrames": 3,  
              "AdaptiveBFrame": true,  
              "Type": "H264Layer",  
              "FrameRate": "0/1"  
            }  
          ],  
          "Type": "H264Video"  
        },  
        {  
          "Profile": "AACLC",  
          "Channels": 2,  
          "SamplingRate": 48000,  
          "Bitrate": 128,  
          "Type": "AACAudio"  
        }  
      ],  
      "Outputs": [  
        {  
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",  
          "Format": {  
            "Type": "MP4Format"  
          }  
        }  
      ]  
    }  
    

## <span data-ttu-id="9f815-117"><a id="encoding_with_dotnet"></a>Kódování pomocí služby Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="9f815-117"><a id="encoding_with_dotnet"></a>Encoding with Media Services .NET SDK</span></span>

<span data-ttu-id="9f815-118">Následující příklad kódu používá sadu Media Services .NET SDK k provádění následujících úloh:</span><span class="sxs-lookup"><span data-stu-id="9f815-118">The following code example uses Media Services .NET SDK to perform the following tasks:</span></span>

- <span data-ttu-id="9f815-119">Vytvořte úlohu kódování.</span><span class="sxs-lookup"><span data-stu-id="9f815-119">Create an encoding job.</span></span>
- <span data-ttu-id="9f815-120">Získáte odkaz na kodéru Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="9f815-120">Get a reference to the Media Encoder Standard encoder.</span></span>
- <span data-ttu-id="9f815-121">Načtěte vlastní přednastavení JSON, který jste vytvořili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="9f815-121">Load the custom JSON preset that you created in the previous section.</span></span> 
  
        // Load the JSON from the local file.
        string configuration = File.ReadAllText(fileName);  

- <span data-ttu-id="9f815-122">Přidejte kódování úkolů do úlohy.</span><span class="sxs-lookup"><span data-stu-id="9f815-122">Add an encoding task to the job.</span></span> 
- <span data-ttu-id="9f815-123">Zadejte vstupní asset, který je zakódován.</span><span class="sxs-lookup"><span data-stu-id="9f815-123">Specify the input asset to be encoded.</span></span>
- <span data-ttu-id="9f815-124">Vytvoření výstupní asset, který bude obsahovat k zakódovanému assetu.</span><span class="sxs-lookup"><span data-stu-id="9f815-124">Create an output asset that will contain the encoded asset.</span></span>
- <span data-ttu-id="9f815-125">Přidání obslužné rutiny události zkontrolovat průběh úlohy.</span><span class="sxs-lookup"><span data-stu-id="9f815-125">Add an event handler to check the job progress.</span></span>
- <span data-ttu-id="9f815-126">Odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="9f815-126">Submit the job.</span></span>
   
#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="9f815-127">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="9f815-127">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="9f815-128">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="9f815-128">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="9f815-129">Příklad</span><span class="sxs-lookup"><span data-stu-id="9f815-129">Example</span></span>   

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;

    namespace CustomizeMESPresests
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

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            // Get an uploaded asset.
            var asset = _context.Assets.FirstOrDefault();

            // Encode and generate the output using custom presets.
            EncodeToAdaptiveBitrateMP4Set(asset);

            Console.ReadLine();
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Load the XML (or JSON) from the local file.
            string configuration = File.ReadAllText("CustomPreset_JSON.json");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            configuration,
            TaskOptions.None);

            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="9f815-130">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="9f815-130">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="9f815-131">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="9f815-131">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="9f815-132">Viz také</span><span class="sxs-lookup"><span data-stu-id="9f815-132">See Also</span></span>
[<span data-ttu-id="9f815-133">Kódování Přehled služby Media Services</span><span class="sxs-lookup"><span data-stu-id="9f815-133">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)

