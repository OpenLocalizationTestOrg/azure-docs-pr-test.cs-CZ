---
title: "kódování aaaAdvanced s pracovním postupem Premium Media Encoder | Microsoft Docs"
description: "Zjistěte, jak tooencode s pracovním postupem Premium kodér médií. Ukázky kódu jsou napsané v jazyce C# a použít hello sady Media Services SDK pro .NET."
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
ms.openlocfilehash: 5a1c3d019a5c8fbf9bda2da751a7eff4c4907d97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-encoding-with-media-encoder-premium-workflow"></a><span data-ttu-id="06015-104">Pokročilé kódování s pracovním postupem prostředí Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="06015-104">Advanced encoding with Media Encoder Premium Workflow</span></span>
> [!NOTE]
> <span data-ttu-id="06015-105">Procesor médií Media Encoder Premium pracovního postupu popsané v tomto tématu není k dispozici v Číně.</span><span class="sxs-lookup"><span data-stu-id="06015-105">Media Encoder Premium Workflow media processor discussed in this topic is not available in China.</span></span>
>
>

<span data-ttu-id="06015-106">Máte dotazy k kodér premium e-mailu mepd na webu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="06015-106">For premium encoder questions, email mepd at Microsoft.com.</span></span>

## <a name="overview"></a><span data-ttu-id="06015-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="06015-107">Overview</span></span>
<span data-ttu-id="06015-108">Microsoft Azure Media Services představuje hello **Media Encoder Premium pracovního postupu** procesor médií.</span><span class="sxs-lookup"><span data-stu-id="06015-108">Microsoft Azure Media Services is introducing hello **Media Encoder Premium Workflow** media processor.</span></span> <span data-ttu-id="06015-109">Tato záloha nabízí procesoru kódování funkcí pro vaše pracovní postupy premium na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="06015-109">This processor offers advance encoding capabilities for your premium on-demand workflows.</span></span>

<span data-ttu-id="06015-110">Hello následující témata popisují podrobnosti související příliš**Media Encoder Premium pracovního postupu**:</span><span class="sxs-lookup"><span data-stu-id="06015-110">hello following topics outline details related too**Media Encoder Premium Workflow**:</span></span>

* <span data-ttu-id="06015-111">[Podporované formáty podle hello Media Encoder Premium pracovního postupu](media-services-premium-workflow-encoder-formats.md) – popisuje formáty souborů hello a kodeky podporované systémem **Media Encoder Premium pracovního postupu**.</span><span class="sxs-lookup"><span data-stu-id="06015-111">[Formats Supported by hello Media Encoder Premium Workflow](media-services-premium-workflow-encoder-formats.md) – Discusses hello file formats and codecs supported by **Media Encoder Premium Workflow**.</span></span>
* <span data-ttu-id="06015-112">[Přehled a porovnání Azure na vyžádání média kodéry](media-services-encode-asset.md) porovná hello kódování možnosti **Media Encoder Premium pracovního postupu** a **Media Encoder Standard**.</span><span class="sxs-lookup"><span data-stu-id="06015-112">[Overview and comparison of Azure on demand media encoders](media-services-encode-asset.md) compares hello encoding capabilities of **Media Encoder Premium Workflow** and **Media Encoder Standard**.</span></span>

<span data-ttu-id="06015-113">Toto téma ukazuje, jak tooencode s **Media Encoder Premium pracovního postupu** pomocí rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="06015-113">This topic demonstrates how tooencode with **Media Encoder Premium Workflow** using .NET.</span></span>

<span data-ttu-id="06015-114">Úlohy pro hello kódování **Media Encoder Premium pracovního postupu** vyžadují jiný konfigurační soubor, názvem souboru pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="06015-114">Encoding tasks for hello **Media Encoder Premium Workflow** require a separate configuration file, called a Workflow file.</span></span> <span data-ttu-id="06015-115">Tyto soubory mít příponu .workflow a jsou vytvořeny pomocí hello [Návrhář postupu provádění](media-services-workflow-designer.md) nástroj.</span><span class="sxs-lookup"><span data-stu-id="06015-115">These files have a .workflow extension and are created using hello [Workflow Designer](media-services-workflow-designer.md) tool.</span></span>

<span data-ttu-id="06015-116">Můžete také získat hello výchozí soubory pracovního postupu [zde](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows).</span><span class="sxs-lookup"><span data-stu-id="06015-116">You can also get hello default workflow files [here](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows).</span></span> <span data-ttu-id="06015-117">Hello složka obsahuje také popis hello těchto souborů.</span><span class="sxs-lookup"><span data-stu-id="06015-117">hello folder also contains hello description of these files.</span></span>

<span data-ttu-id="06015-118">soubory pracovního postupu Hello potřebovat účtu Media Services tooyour toobe nahrán jako prostředek, a tento prostředek by měl být předán toohello kódování úloh.</span><span class="sxs-lookup"><span data-stu-id="06015-118">hello workflow files need toobe uploaded tooyour Media Services account as an Asset, and this Asset should be passed in toohello encoding Task.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="06015-119">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="06015-119">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="06015-120">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="06015-120">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="encoding-example"></a><span data-ttu-id="06015-121">Příklad kódování</span><span class="sxs-lookup"><span data-stu-id="06015-121">Encoding example</span></span>

<span data-ttu-id="06015-122">Hello následující příklad ukazuje, jak tooencode s **Media Encoder Premium pracovního postupu**.</span><span class="sxs-lookup"><span data-stu-id="06015-122">hello following example demonstrates how tooencode with **Media Encoder Premium Workflow**.</span></span>

<span data-ttu-id="06015-123">jsou prováděny Hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="06015-123">hello following steps are performed:</span></span>

1. <span data-ttu-id="06015-124">Vytvořte asset a nahrajte soubor pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="06015-124">Create an asset and upload a workflow file.</span></span>
2. <span data-ttu-id="06015-125">Vytvořte asset a nahrajte soubor zdrojového média.</span><span class="sxs-lookup"><span data-stu-id="06015-125">Create an asset and upload a source media file.</span></span>
3. <span data-ttu-id="06015-126">Získáte procesor "Media Encoder Premium Workflow" médií hello.</span><span class="sxs-lookup"><span data-stu-id="06015-126">Get hello "Media Encoder Premium Workflow" media processor.</span></span>
4. <span data-ttu-id="06015-127">Vytvoření úlohy a úlohy.</span><span class="sxs-lookup"><span data-stu-id="06015-127">Create a job and a task.</span></span>

    <span data-ttu-id="06015-128">Ve většině případů je prázdný řetězec hello konfigurace pro úlohu hello (třeba v hello následující příklad).</span><span class="sxs-lookup"><span data-stu-id="06015-128">In most cases, hello configuration string for hello task is empty (like in hello following example).</span></span> <span data-ttu-id="06015-129">Existují některé pokročilé scénáře (které vyžadují tootooset runtime vlastnosti dynamicky) v takovém případě by poskytnout toohello řetězec XML pro kódování úloh.</span><span class="sxs-lookup"><span data-stu-id="06015-129">There are some advanced scenarios (that require you tootooset runtime properties dynamically) in which case you would provide an XML string toohello encoding task.</span></span> <span data-ttu-id="06015-130">Mezi příklady takových scénářů jsou: vytváření překrytí, paralelní nebo po sobě jdoucích média ve hřbetu, titulkování.</span><span class="sxs-lookup"><span data-stu-id="06015-130">Examples of such scenarios are: creating an overlay, parallel or sequential media stitching, subtitling.</span></span>
5. <span data-ttu-id="06015-131">Přidáte úloha toohello dva vstupní prostředky.</span><span class="sxs-lookup"><span data-stu-id="06015-131">Add two input assets toohello task.</span></span>

    1. <span data-ttu-id="06015-132">1. – asset hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="06015-132">1st – hello workflow asset.</span></span>
    2. <span data-ttu-id="06015-133">2. – asset videa hello.</span><span class="sxs-lookup"><span data-stu-id="06015-133">2nd – hello video asset.</span></span>

    >[!NOTE]
    ><span data-ttu-id="06015-134">asset Hello pracovního postupu je nutné přidat úloha toohello před hello média asset.</span><span class="sxs-lookup"><span data-stu-id="06015-134">hello workflow asset must be added toohello task before hello media asset.</span></span>
   <span data-ttu-id="06015-135">Hello konfigurační řetězec pro tato úloha by měla být prázdná.</span><span class="sxs-lookup"><span data-stu-id="06015-135">hello configuration string for this task should be empty.</span></span>
   
6. <span data-ttu-id="06015-136">Odeslání úlohy kódování hello.</span><span class="sxs-lookup"><span data-stu-id="06015-136">Submit hello encoding job.</span></span>

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
                    // Get a media processor reference, and pass tooit hello name of the
                    // processor toouse for hello specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

                    // Create a task with hello encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                        processor,
                        "",
                        TaskOptions.None);

                    // Specify hello input asset toobe encoded.
                    task.InputAssets.Add(workflow);
                    task.InputAssets.Add(video); // we add one asset
                                                 // Add an output asset toocontain hello results of hello job.
                                                 // This output is specified as AssetCreationOptions.None, which
                                                 // means hello output asset is not encrypted.
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);

                    // Use hello following event handler toocheck job progress.  
                    job.StateChanged += new
                            EventHandler<JobStateChangedEventArgs>(StateChanged);

                    // Launch hello job.
                    job.Submit();

                    // Check job execution and wait for job toofinish.
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                    progressJobTask.Wait();

                    // If job state is Error hello event handling
                    // method for job progress should log errors.  Here we check
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        throw new Exception("\nExiting method due toojob error.");
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

<span data-ttu-id="06015-137">Máte dotazy k kodér premium e-mailu mepd na webu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="06015-137">For premium encoder questions, email mepd at Microsoft.com.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="06015-138">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="06015-138">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="06015-139">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="06015-139">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
