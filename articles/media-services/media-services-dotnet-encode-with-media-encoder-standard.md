---
title: "aaaEncode prostředek s Media Encoder Standard pomocí rozhraní .NET | Microsoft Docs"
description: "Toto téma ukazuje, jak toouse .NET tooencode assetu pomocí kodéru standardní média."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 03431b64-5518-478a-a1c2-1de345999274
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 25e274c3b67168f4afc8b8ab04af2d654c9dd6e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a><span data-ttu-id="c203f-103">Kódování assetu pomocí kodéru Media Encoder Standard pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="c203f-103">Encode an asset with Media Encoder Standard using .NET</span></span>
<span data-ttu-id="c203f-104">Kódování úlohy jsou některé z nejběžnějších operací zpracování hello ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="c203f-104">Encoding jobs are one of hello most common processing operations in Media Services.</span></span> <span data-ttu-id="c203f-105">Z jednoho kódování tooanother vytvoříte kódování úlohy tooconvert mediálních souborů.</span><span class="sxs-lookup"><span data-stu-id="c203f-105">You create encoding jobs tooconvert media files from one encoding tooanother.</span></span> <span data-ttu-id="c203f-106">Při kódování, můžete použít hello předdefinované Media Encoder Media Services.</span><span class="sxs-lookup"><span data-stu-id="c203f-106">When you encode, you can use hello Media Services built-in Media Encoder.</span></span> <span data-ttu-id="c203f-107">Můžete také použít kodér poskytovanými partnerem Media Services; jsou k dispozici prostřednictvím Azure Marketplace hello kodéry třetích stran.</span><span class="sxs-lookup"><span data-stu-id="c203f-107">You can also use an encoder provided by a Media Services partner; third party encoders are available through hello Azure Marketplace.</span></span> 

<span data-ttu-id="c203f-108">Toto téma ukazuje, jak toouse .NET tooencode vaše prostředky s Media Encoder Standard (MES).</span><span class="sxs-lookup"><span data-stu-id="c203f-108">This topic shows how toouse .NET tooencode your assets with Media Encoder Standard (MES).</span></span> <span data-ttu-id="c203f-109">Media Encoder Standard je konfigurován pomocí jedno z přednastavení kodér hello popsané [zde](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="c203f-109">Media Encoder Standard is configured using one of hello encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

<span data-ttu-id="c203f-110">Doporučujeme tooalways zakódovat zdrojové soubory do sady souborů MP4 adaptivní přenosovou rychlostí a pak převést hello sadu toohello požadovaný formát pomocí hello [dynamické balení](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c203f-110">It is recommended tooalways encode your source files into an adaptive bitrate MP4 set and then convert hello set toohello desired format using hello [Dynamic Packaging](media-services-dynamic-packaging-overview.md).</span></span> 

<span data-ttu-id="c203f-111">Pokud výstupní asset používá šifrování úložiště, musíte nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="c203f-111">If your output asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="c203f-112">Další informace najdete v části [konfigurace zásad doručení assetu](media-services-dotnet-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="c203f-112">For more information see [Configuring asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c203f-113">Výstupní soubor s názvem, který obsahuje hello nejprve 32 znaků, který vytváří MES hello název vstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="c203f-113">MES produces an output file with a name that contains hello first 32 characters of hello input file name.</span></span> <span data-ttu-id="c203f-114">Název Hello je založen na co jsou uvedeny v souboru přednastavené hello.</span><span class="sxs-lookup"><span data-stu-id="c203f-114">hello name is based on what is specified in hello preset file.</span></span> <span data-ttu-id="c203f-115">Například "název souboru": "{Basename} _ {Index} {rozšíření}".</span><span class="sxs-lookup"><span data-stu-id="c203f-115">For example, "FileName": "{Basename}_{Index}{Extension}".</span></span> <span data-ttu-id="c203f-116">{Basename} je nahrazena hello nejprve 32 znaků názvu hello vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="c203f-116">{Basename} is replaced by hello first 32 characters of hello input file name.</span></span>
> 
> 

### <a name="mes-formats"></a><span data-ttu-id="c203f-117">Formáty MES</span><span class="sxs-lookup"><span data-stu-id="c203f-117">MES Formats</span></span>
[<span data-ttu-id="c203f-118">Formáty a kodeky</span><span class="sxs-lookup"><span data-stu-id="c203f-118">Formats and codecs</span></span>](media-services-media-encoder-standard-formats.md)

### <a name="mes-presets"></a><span data-ttu-id="c203f-119">Přednastavení MES</span><span class="sxs-lookup"><span data-stu-id="c203f-119">MES Presets</span></span>
<span data-ttu-id="c203f-120">Media Encoder Standard je konfigurován pomocí jedno z přednastavení kodér hello popsané [zde](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="c203f-120">Media Encoder Standard is configured using one of hello encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

### <a name="input-and-output-metadata"></a><span data-ttu-id="c203f-121">Vstup a výstup metadat</span><span class="sxs-lookup"><span data-stu-id="c203f-121">Input and output metadata</span></span>
<span data-ttu-id="c203f-122">Při kódování prostředek vstupní (nebo prostředky) pomocí MES, můžete získat prostředek výstup v hello kódování úspěšné dokončení této úlohy.</span><span class="sxs-lookup"><span data-stu-id="c203f-122">When you encode an input asset (or assets) using MES, you get an output asset at hello successful completion of that encode task.</span></span> <span data-ttu-id="c203f-123">Hello výstupní asset obsahuje video, zvuk, miniatur, manifest atd. hello předvolby kódování, které používáte.</span><span class="sxs-lookup"><span data-stu-id="c203f-123">hello output asset contains video, audio, thumbnails, manifest, etc. based on hello encoding preset you use.</span></span>

<span data-ttu-id="c203f-124">Hello výstupní asset obsahuje také soubor s metadata o vstupní asset hello.</span><span class="sxs-lookup"><span data-stu-id="c203f-124">hello output asset also contains a file with metadata about hello input asset.</span></span> <span data-ttu-id="c203f-125">Hello název souboru XML metadat hello má hello formátu: < asset_id > _metadata.xml (například 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), kde < asset_id > je hodnota ID hello hello vstupní asset.</span><span class="sxs-lookup"><span data-stu-id="c203f-125">hello name of hello metadata XML file has hello following format: <asset_id>_metadata.xml (for example, 41114ad3-eb5e-4c57-8d92-5354e2b7d4a4_metadata.xml), where <asset_id> is hello AssetId value of hello input asset.</span></span> <span data-ttu-id="c203f-126">Hello schéma tato metadata vstupní XML je popsána [zde](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="c203f-126">hello schema of this input metadata XML is described [here](media-services-input-metadata-schema.md).</span></span>

<span data-ttu-id="c203f-127">Hello výstupní asset obsahuje také soubor s metadata o hello výstupní asset.</span><span class="sxs-lookup"><span data-stu-id="c203f-127">hello output asset also contains a file with metadata about hello output asset.</span></span> <span data-ttu-id="c203f-128">Hello název souboru XML metadat hello má hello formátu: < source_file_name > _manifest.xml (například BigBuckBunny_manifest.xml).</span><span class="sxs-lookup"><span data-stu-id="c203f-128">hello name of hello metadata XML file has hello following format: <source_file_name>_manifest.xml (for example, BigBuckBunny_manifest.xml).</span></span> <span data-ttu-id="c203f-129">schéma Hello tato metadata výstup XML je popsána [zde](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="c203f-129">hello schema of this output metadata XML is described [here](media-services-output-metadata-schema.md).</span></span>

<span data-ttu-id="c203f-130">Pokud chcete, tooexamine buď ze souborů metadat hello dva, můžete vytvořit lokátor SAS a stáhnout hello souboru tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="c203f-130">If you want tooexamine either of hello two metadata files, you can create a SAS locator and download hello file tooyour local computer.</span></span> <span data-ttu-id="c203f-131">Příklad na tom, jak toocreate lokátoru SAS a stahování souboru pomocí hello Media Services můžete nalézt rozšíření sady SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="c203f-131">You can find an example on how toocreate a SAS locator and download a file Using hello Media Services .NET SDK Extensions.</span></span>

## <a name="download-sample"></a><span data-ttu-id="c203f-132">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="c203f-132">Download sample</span></span>
<span data-ttu-id="c203f-133">Můžete získat a spustit ukázku, která ukazuje, jak tooencode s MES z [zde](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="c203f-133">You can get and run a sample that shows how tooencode with MES from [here](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="c203f-134">Ukázkový kód rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="c203f-134">.NET sample code</span></span>

<span data-ttu-id="c203f-135">Následující ukázka kódu Hello používá sadu Media Services .NET SDK tooperform hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="c203f-135">hello following code example uses Media Services .NET SDK tooperform hello following tasks:</span></span>

* <span data-ttu-id="c203f-136">Vytvořte úlohu kódování.</span><span class="sxs-lookup"><span data-stu-id="c203f-136">Create an encoding job.</span></span>
* <span data-ttu-id="c203f-137">Získání kodéru Media Encoder Standard toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="c203f-137">Get a reference toohello Media Encoder Standard encoder.</span></span>
* <span data-ttu-id="c203f-138">Zadejte toouse hello [adaptivní datové proudy](media-services-autogen-bitrate-ladder-with-mes.md) přednastavené.</span><span class="sxs-lookup"><span data-stu-id="c203f-138">Specify toouse hello [Adaptive Streaming](media-services-autogen-bitrate-ladder-with-mes.md) preset.</span></span> 
* <span data-ttu-id="c203f-139">Přidejte jeden úlohy kódování toohello úloh.</span><span class="sxs-lookup"><span data-stu-id="c203f-139">Add a single encoding task toohello job.</span></span> 
* <span data-ttu-id="c203f-140">Zadejte vstup hello asset toobe kódování.</span><span class="sxs-lookup"><span data-stu-id="c203f-140">Specify hello input asset toobe encoded.</span></span>
* <span data-ttu-id="c203f-141">Vytvoření výstupní asset, který bude obsahovat kódovaný hello asset.</span><span class="sxs-lookup"><span data-stu-id="c203f-141">Create an output asset that will contain hello encoded asset.</span></span>
* <span data-ttu-id="c203f-142">Přidejte průběh úlohy toocheck hello událost obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="c203f-142">Add an event handler toocheck hello job progress.</span></span>
* <span data-ttu-id="c203f-143">Odešlete úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="c203f-143">Submit hello job.</span></span>

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="c203f-144">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="c203f-144">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="c203f-145">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="c203f-145">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="c203f-146">Příklad</span><span class="sxs-lookup"><span data-stu-id="c203f-146">Example</span></span> 

        using System;
        using System.Linq;
        using System.Configuration;
        using System.IO;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;

        namespace MediaEncoderStandardSample
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

                    // Create a task with hello encoding details, using a string preset.
                    // In this case "Adaptive Streaming" preset is used.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "Adaptive Streaming",
                        TaskOptions.None);

                    // Specify hello input asset toobe encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset toocontain hello results of hello job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means hello output asset is not encrypted. 
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

## <a name="media-services-learning-paths"></a><span data-ttu-id="c203f-147">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="c203f-147">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c203f-148">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="c203f-148">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="c203f-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c203f-149">Next steps</span></span>
<span data-ttu-id="c203f-150">[Jak toogenerate miniaturu pomocí kodéru Media Encoder Standard s .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[kódování Přehled služby Media Services](media-services-encode-asset.md)</span><span class="sxs-lookup"><span data-stu-id="c203f-150">[How toogenerate thumbnail using Media Encoder Standard with .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services Encoding Overview](media-services-encode-asset.md)</span></span>

