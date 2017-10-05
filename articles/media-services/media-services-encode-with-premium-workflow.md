---
title: "Pokročilé kódování s pracovním postupem Premium Media Encoder | Microsoft Docs"
description: "Zjistěte, jak ke kódování s pracovním postupem Premium kodér médií. Ukázky kódu jsou napsané v jazyce C# a pomocí sady Media Services SDK pro .NET."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 0f4c87ac-810a-4d42-8df8-923dff2016c6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 2b03853bf07e05c07fd730d5e8a8563963887921
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="advanced-encoding-with-media-encoder-premium-workflow"></a><span data-ttu-id="1bac3-104">Pokročilé kódování s pracovním postupem prostředí Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="1bac3-104">Advanced encoding with Media Encoder Premium Workflow</span></span>
> [!NOTE]
> <span data-ttu-id="1bac3-105">Procesor médií Media Encoder Premium pracovního postupu popsané v tomto tématu není k dispozici v Číně.</span><span class="sxs-lookup"><span data-stu-id="1bac3-105">Media Encoder Premium Workflow media processor discussed in this topic is not available in China.</span></span>
>
>

<span data-ttu-id="1bac3-106">Máte dotazy k kodér premium e-mailu mepd na webu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1bac3-106">For premium encoder questions, email mepd at Microsoft.com.</span></span>

## <a name="overview"></a><span data-ttu-id="1bac3-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="1bac3-107">Overview</span></span>
<span data-ttu-id="1bac3-108">Představuje Microsoft Azure Media Services **Media Encoder Premium pracovního postupu** procesor médií.</span><span class="sxs-lookup"><span data-stu-id="1bac3-108">Microsoft Azure Media Services is introducing the **Media Encoder Premium Workflow** media processor.</span></span> <span data-ttu-id="1bac3-109">Tato záloha nabízí procesoru kódování funkcí pro vaše pracovní postupy premium na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="1bac3-109">This processor offers advance encoding capabilities for your premium on-demand workflows.</span></span>

<span data-ttu-id="1bac3-110">Následující témata popisují podrobnosti související s **Media Encoder Premium pracovního postupu**:</span><span class="sxs-lookup"><span data-stu-id="1bac3-110">The following topics outline details related to **Media Encoder Premium Workflow**:</span></span>

* <span data-ttu-id="1bac3-111">[Formáty podporované v pracovním postupu Media Encoder Premium](media-services-premium-workflow-encoder-formats.md) – formáty popisuje soubor a nepodporuje kodeky **Media Encoder Premium pracovního postupu**.</span><span class="sxs-lookup"><span data-stu-id="1bac3-111">[Formats Supported by the Media Encoder Premium Workflow](media-services-premium-workflow-encoder-formats.md) – Discusses the file formats and codecs supported by **Media Encoder Premium Workflow**.</span></span>
* <span data-ttu-id="1bac3-112">[Přehled a porovnání Azure na vyžádání média kodéry](media-services-encode-asset.md) porovná kódování možnosti **Media Encoder Premium pracovního postupu** a **Media Encoder Standard**.</span><span class="sxs-lookup"><span data-stu-id="1bac3-112">[Overview and comparison of Azure on demand media encoders](media-services-encode-asset.md) compares the encoding capabilities of **Media Encoder Premium Workflow** and **Media Encoder Standard**.</span></span>

<span data-ttu-id="1bac3-113">Toto téma ukazuje, jak ke kódování s **Media Encoder Premium pracovního postupu** pomocí rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="1bac3-113">This topic demonstrates how to encode with **Media Encoder Premium Workflow** using .NET.</span></span>

<span data-ttu-id="1bac3-114">Úlohy pro kódování **Media Encoder Premium pracovního postupu** vyžadují jiný konfigurační soubor, názvem souboru pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="1bac3-114">Encoding tasks for the **Media Encoder Premium Workflow** require a separate configuration file, called a Workflow file.</span></span> <span data-ttu-id="1bac3-115">Tyto soubory mají příponu .workflow a jsou vytvořené pomocí [Návrhář postupu provádění](media-services-workflow-designer.md) nástroj.</span><span class="sxs-lookup"><span data-stu-id="1bac3-115">These files have a .workflow extension and are created using the [Workflow Designer](media-services-workflow-designer.md) tool.</span></span>

<span data-ttu-id="1bac3-116">Můžete také získat výchozí soubory pracovního postupu [zde](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows).</span><span class="sxs-lookup"><span data-stu-id="1bac3-116">You can also get the default workflow files [here](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows).</span></span> <span data-ttu-id="1bac3-117">Složka obsahuje také popis těchto souborů.</span><span class="sxs-lookup"><span data-stu-id="1bac3-117">The folder also contains the description of these files.</span></span>

<span data-ttu-id="1bac3-118">Soubory pracovního postupu musí být nahrán do vašeho účtu Media Services jako prostředek, a tento prostředek by měl být předané do úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="1bac3-118">The workflow files need to be uploaded to your Media Services account as an Asset, and this Asset should be passed in to the encoding Task.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="1bac3-119">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="1bac3-119">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="1bac3-120">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="1bac3-120">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="encoding-example"></a><span data-ttu-id="1bac3-121">Příklad kódování</span><span class="sxs-lookup"><span data-stu-id="1bac3-121">Encoding example</span></span>

<span data-ttu-id="1bac3-122">Následující příklad ukazuje, jak ke kódování s **Media Encoder Premium pracovního postupu**.</span><span class="sxs-lookup"><span data-stu-id="1bac3-122">The following example demonstrates how to encode with **Media Encoder Premium Workflow**.</span></span>

<span data-ttu-id="1bac3-123">Jsou prováděny následovně:</span><span class="sxs-lookup"><span data-stu-id="1bac3-123">The following steps are performed:</span></span>

1. <span data-ttu-id="1bac3-124">Vytvořte asset a nahrajte soubor pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="1bac3-124">Create an asset and upload a workflow file.</span></span>
2. <span data-ttu-id="1bac3-125">Vytvořte asset a nahrajte soubor zdrojového média.</span><span class="sxs-lookup"><span data-stu-id="1bac3-125">Create an asset and upload a source media file.</span></span>
3. <span data-ttu-id="1bac3-126">Získáte procesor "Media Encoder Premium Workflow" médií.</span><span class="sxs-lookup"><span data-stu-id="1bac3-126">Get the "Media Encoder Premium Workflow" media processor.</span></span>
4. <span data-ttu-id="1bac3-127">Vytvoření úlohy a úlohy.</span><span class="sxs-lookup"><span data-stu-id="1bac3-127">Create a job and a task.</span></span>

    <span data-ttu-id="1bac3-128">Ve většině případů je prázdný řetězec konfigurace pro úlohu (jako v následujícím příkladu).</span><span class="sxs-lookup"><span data-stu-id="1bac3-128">In most cases, the configuration string for the task is empty (like in the following example).</span></span> <span data-ttu-id="1bac3-129">Existují některé pokročilé scénáře (které vyžadují, abyste na dynamické nastavení vlastnosti runtime) v takovém případě by zadáte řetězec XML úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="1bac3-129">There are some advanced scenarios (that require you to to set runtime properties dynamically) in which case you would provide an XML string to the encoding task.</span></span> <span data-ttu-id="1bac3-130">Mezi příklady takových scénářů jsou: vytváření překrytí, paralelní nebo po sobě jdoucích média ve hřbetu, titulkování.</span><span class="sxs-lookup"><span data-stu-id="1bac3-130">Examples of such scenarios are: creating an overlay, parallel or sequential media stitching, subtitling.</span></span>
5. <span data-ttu-id="1bac3-131">Přidejte dva vstupní prostředky do úlohy.</span><span class="sxs-lookup"><span data-stu-id="1bac3-131">Add two input assets to the task.</span></span>

    1. <span data-ttu-id="1bac3-132">1. – asset pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="1bac3-132">1st – the workflow asset.</span></span>
    2. <span data-ttu-id="1bac3-133">2. – asset videa.</span><span class="sxs-lookup"><span data-stu-id="1bac3-133">2nd – the video asset.</span></span>

    >[!NOTE]
    ><span data-ttu-id="1bac3-134">Asset pracovního postupu je nutné přidat úloha před asset média.</span><span class="sxs-lookup"><span data-stu-id="1bac3-134">The workflow asset must be added to the task before the media asset.</span></span>
   <span data-ttu-id="1bac3-135">Řetězec konfigurace pro tato úloha by měla být prázdná.</span><span class="sxs-lookup"><span data-stu-id="1bac3-135">The configuration string for this task should be empty.</span></span>
   
6. <span data-ttu-id="1bac3-136">Odeslání úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="1bac3-136">Submit the encoding job.</span></span>

        using System;
        using System.Linq;
        using System.Configuration;
        using System.IO;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;

        namespace MediaEncoderPremiumWorkflowSample
        {
            class Program
            {
                private static readonly string _AADTenantDomain =
                    ConfigurationManager.AppSettings["AADTenantDomain"];
                private static readonly string _RESTAPIEndpoint =
                    ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

                // Field for service context.
                private static CloudMediaContext _context = null;

                private static readonly string _supportFiles =
                    Path.GetFullPath(@"../..\Media");

                private static readonly string _workflowFilePath =
                    Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");

                private static readonly string _singleMP4InputFilePath =
                    Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");

                static void Main(string[] args)
                {
                    var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
                    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                    _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

                    var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
                    var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
                    IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset);

                }

                static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
                {
                    var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

                    var fileName = Path.GetFileName(singleFilePath);

                    var assetFile = asset.AssetFiles.Create(fileName);

                    Console.WriteLine("Created assetFile {0}", assetFile.Name);

                    Console.WriteLine("Upload {0}", assetFile.Name);

                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);

                    return asset;
                }

                static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Premium Workflow encoding job");
                    // Get a media processor reference, and pass to it the name of the
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                        processor,
                        "",
                        TaskOptions.None);

                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(workflow);
                    task.InputAssets.Add(video); // we add one asset
                                                 // Add an output asset to contain the results of the job.
                                                 // This output is specified as AssetCreationOptions.None, which
                                                 // means the output asset is not encrypted.
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);

                    // Use the following event handler to check job progress.  
                    job.StateChanged += new
                            EventHandler<JobStateChangedEventArgs>(StateChanged);

                    // Launch the job.
                    job.Submit();

                    // Check job execution and wait for job to finish.
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                    progressJobTask.Wait();

                    // If job state is Error the event handling
                    // method for job progress should log errors.  Here we check
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        throw new Exception("\nExiting method due to job error.");
                    }

                    return job.OutputMediaAssets[0];
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

<span data-ttu-id="1bac3-137">Máte dotazy k kodér premium e-mailu mepd na webu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1bac3-137">For premium encoder questions, email mepd at Microsoft.com.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="1bac3-138">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="1bac3-138">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="1bac3-139">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="1bac3-139">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
