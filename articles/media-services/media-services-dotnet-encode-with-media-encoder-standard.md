---
title: "Kódování assetu pomocí kodéru Media Encoder Standard pomocí rozhraní .NET | Microsoft Docs"
description: "Toto téma ukazuje, jak pomocí rozhraní .NET kódování assetu pomocí kodéru standardní média."
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
ms.openlocfilehash: 929592368501c54277748bf46b2160c9058db3fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a><span data-ttu-id="ee51f-103">Kódování assetu pomocí kodéru Media Encoder Standard pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="ee51f-103">Encode an asset with Media Encoder Standard using .NET</span></span>
<span data-ttu-id="ee51f-104">Kódování úloh je jednou z nejběžnějších operací zpracování ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="ee51f-104">Encoding jobs are one of the most common processing operations in Media Services.</span></span> <span data-ttu-id="ee51f-105">K převodu mediálních souborů z jednoho kódování do druhého se využívají kódovací úlohy.</span><span class="sxs-lookup"><span data-stu-id="ee51f-105">You create encoding jobs to convert media files from one encoding to another.</span></span> <span data-ttu-id="ee51f-106">Při kódování, můžete použít předdefinované Media Encoder Media Services.</span><span class="sxs-lookup"><span data-stu-id="ee51f-106">When you encode, you can use the Media Services built-in Media Encoder.</span></span> <span data-ttu-id="ee51f-107">Můžete také použít kodér poskytovanými partnerem Media Services; třetí strany kodéry jsou k dispozici prostřednictvím Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ee51f-107">You can also use an encoder provided by a Media Services partner; third party encoders are available through the Azure Marketplace.</span></span> 

<span data-ttu-id="ee51f-108">Toto téma ukazuje, jak používat .NET určený ke kódování vaše prostředky s Media Encoder Standard (MES).</span><span class="sxs-lookup"><span data-stu-id="ee51f-108">This topic shows how to use .NET to encode your assets with Media Encoder Standard (MES).</span></span> <span data-ttu-id="ee51f-109">Media Encoder Standard je konfigurován pomocí jedno z přednastavení kodér popsané [zde](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="ee51f-109">Media Encoder Standard is configured using one of the encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

<span data-ttu-id="ee51f-110">Doporučuje se vždycky zakódovat zdrojové soubory do sady souborů MP4 adaptivní přenosovou rychlostí a pak sadu převést na požadovaný formát pomocí [dynamické balení](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ee51f-110">It is recommended to always encode your source files into an adaptive bitrate MP4 set and then convert the set to the desired format using the [Dynamic Packaging](media-services-dynamic-packaging-overview.md).</span></span> 

<span data-ttu-id="ee51f-111">Pokud výstupní asset používá šifrování úložiště, musíte nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="ee51f-111">If your output asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="ee51f-112">Další informace najdete v části [konfigurace zásad doručení assetu](media-services-dotnet-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="ee51f-112">For more information see [Configuring asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md).</span></span>

> [!NOTE]
> <span data-ttu-id="ee51f-113">MES vytvoří výstupní soubor s názvem, který obsahuje první 32 znaků z názvu vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="ee51f-113">MES produces an output file with a name that contains the first 32 characters of the input file name.</span></span> <span data-ttu-id="ee51f-114">Název podle zadaných v přednastavené souboru.</span><span class="sxs-lookup"><span data-stu-id="ee51f-114">The name is based on what is specified in the preset file.</span></span> <span data-ttu-id="ee51f-115">Například "název souboru": "{Basename} _ {Index} {rozšíření}".</span><span class="sxs-lookup"><span data-stu-id="ee51f-115">For example, "FileName": "{Basename}_{Index}{Extension}".</span></span> <span data-ttu-id="ee51f-116">{Basename} je nahrazena nejprve 32 znaků z názvu vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="ee51f-116">{Basename} is replaced by the first 32 characters of the input file name.</span></span>
> 
> 

### <a name="mes-formats"></a><span data-ttu-id="ee51f-117">Formáty MES</span><span class="sxs-lookup"><span data-stu-id="ee51f-117">MES Formats</span></span>
[<span data-ttu-id="ee51f-118">Formáty a kodeky</span><span class="sxs-lookup"><span data-stu-id="ee51f-118">Formats and codecs</span></span>](media-services-media-encoder-standard-formats.md)

### <a name="mes-presets"></a><span data-ttu-id="ee51f-119">Přednastavení MES</span><span class="sxs-lookup"><span data-stu-id="ee51f-119">MES Presets</span></span>
<span data-ttu-id="ee51f-120">Media Encoder Standard je konfigurován pomocí jedno z přednastavení kodér popsané [zde](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="ee51f-120">Media Encoder Standard is configured using one of the encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

### <a name="input-and-output-metadata"></a><span data-ttu-id="ee51f-121">Vstup a výstup metadat</span><span class="sxs-lookup"><span data-stu-id="ee51f-121">Input and output metadata</span></span>
<span data-ttu-id="ee51f-122">Při kódování prostředek vstupní (nebo prostředky) pomocí MES získat prostředek výstup na úspěšné dokončení této kódování úloh.</span><span class="sxs-lookup"><span data-stu-id="ee51f-122">When you encode an input asset (or assets) using MES, you get an output asset at the successful completion of that encode task.</span></span> <span data-ttu-id="ee51f-123">Výstupní asset obsahuje video, zvuk, miniatur, manifest atd., které můžete použít přednastavení kódování.</span><span class="sxs-lookup"><span data-stu-id="ee51f-123">The output asset contains video, audio, thumbnails, manifest, etc. based on the encoding preset you use.</span></span>

<span data-ttu-id="ee51f-124">Výstupní asset obsahuje také soubor s metadata o vstupní asset.</span><span class="sxs-lookup"><span data-stu-id="ee51f-124">The output asset also contains a file with metadata about the input asset.</span></span> <span data-ttu-id="ee51f-125">Název souboru XML metadat má následující formát: < asset_id > _metadata.xml (například 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), kde < asset_id > je hodnota ID vstupní prostředku.</span><span class="sxs-lookup"><span data-stu-id="ee51f-125">The name of the metadata XML file has the following format: <asset_id>_metadata.xml (for example, 41114ad3-eb5e-4c57-8d92-5354e2b7d4a4_metadata.xml), where <asset_id> is the AssetId value of the input asset.</span></span> <span data-ttu-id="ee51f-126">Schéma tato metadata vstupní XML je popsána [zde](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="ee51f-126">The schema of this input metadata XML is described [here](media-services-input-metadata-schema.md).</span></span>

<span data-ttu-id="ee51f-127">Výstupní asset obsahuje také soubor s metadata o výstupní asset.</span><span class="sxs-lookup"><span data-stu-id="ee51f-127">The output asset also contains a file with metadata about the output asset.</span></span> <span data-ttu-id="ee51f-128">Název souboru XML metadat má následující formát: < source_file_name > _manifest.xml (například BigBuckBunny_manifest.xml).</span><span class="sxs-lookup"><span data-stu-id="ee51f-128">The name of the metadata XML file has the following format: <source_file_name>_manifest.xml (for example, BigBuckBunny_manifest.xml).</span></span> <span data-ttu-id="ee51f-129">Schéma tato metadata výstup XML je popsána [zde](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="ee51f-129">The schema of this output metadata XML is described [here](media-services-output-metadata-schema.md).</span></span>

<span data-ttu-id="ee51f-130">Pokud chcete prozkoumat buď dva soubory metadat, můžete vytvořit lokátor SAS a stáhněte soubor do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="ee51f-130">If you want to examine either of the two metadata files, you can create a SAS locator and download the file to your local computer.</span></span> <span data-ttu-id="ee51f-131">Příklad najdete na tom, jak vytvořit lokátor SAS a stáhnout soubor pomocí rozšíření Media Services .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="ee51f-131">You can find an example on how to create a SAS locator and download a file Using the Media Services .NET SDK Extensions.</span></span>

## <a name="download-sample"></a><span data-ttu-id="ee51f-132">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="ee51f-132">Download sample</span></span>
<span data-ttu-id="ee51f-133">Můžete získat a spustit ukázku, která ukazuje, jak ke kódování s MES z [zde](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="ee51f-133">You can get and run a sample that shows how to encode with MES from [here](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="ee51f-134">Ukázkový kód rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="ee51f-134">.NET sample code</span></span>

<span data-ttu-id="ee51f-135">Následující příklad kódu používá sadu Media Services .NET SDK k provádění následujících úloh:</span><span class="sxs-lookup"><span data-stu-id="ee51f-135">The following code example uses Media Services .NET SDK to perform the following tasks:</span></span>

* <span data-ttu-id="ee51f-136">Vytvořte úlohu kódování.</span><span class="sxs-lookup"><span data-stu-id="ee51f-136">Create an encoding job.</span></span>
* <span data-ttu-id="ee51f-137">Získáte odkaz na kodéru Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="ee51f-137">Get a reference to the Media Encoder Standard encoder.</span></span>
* <span data-ttu-id="ee51f-138">Použít [adaptivní datové proudy](media-services-autogen-bitrate-ladder-with-mes.md) přednastavené.</span><span class="sxs-lookup"><span data-stu-id="ee51f-138">Specify to use the [Adaptive Streaming](media-services-autogen-bitrate-ladder-with-mes.md) preset.</span></span> 
* <span data-ttu-id="ee51f-139">Přidáte jednoho kódování úkolu do úlohy.</span><span class="sxs-lookup"><span data-stu-id="ee51f-139">Add a single encoding task to the job.</span></span> 
* <span data-ttu-id="ee51f-140">Zadejte vstupní asset, který je zakódován.</span><span class="sxs-lookup"><span data-stu-id="ee51f-140">Specify the input asset to be encoded.</span></span>
* <span data-ttu-id="ee51f-141">Vytvoření výstupní asset, který bude obsahovat k zakódovanému assetu.</span><span class="sxs-lookup"><span data-stu-id="ee51f-141">Create an output asset that will contain the encoded asset.</span></span>
* <span data-ttu-id="ee51f-142">Přidání obslužné rutiny události zkontrolovat průběh úlohy.</span><span class="sxs-lookup"><span data-stu-id="ee51f-142">Add an event handler to check the job progress.</span></span>
* <span data-ttu-id="ee51f-143">Odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="ee51f-143">Submit the job.</span></span>

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="ee51f-144">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="ee51f-144">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="ee51f-145">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="ee51f-145">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="ee51f-146">Příklad</span><span class="sxs-lookup"><span data-stu-id="ee51f-146">Example</span></span> 

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

                    // Encode and generate the output using the "Adaptive Streaming" preset.
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

                    // Create a task with the encoding details, using a string preset.
                    // In this case "Adaptive Streaming" preset is used.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "Adaptive Streaming",
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

## <a name="media-services-learning-paths"></a><span data-ttu-id="ee51f-147">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="ee51f-147">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="ee51f-148">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="ee51f-148">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="ee51f-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ee51f-149">Next steps</span></span>
<span data-ttu-id="ee51f-150">[Jak vygenerovat miniaturu pomocí kodéru Media Encoder Standard s .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[kódování Přehled služby Media Services](media-services-encode-asset.md)</span><span class="sxs-lookup"><span data-stu-id="ee51f-150">[How to generate thumbnail using Media Encoder Standard with .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services Encoding Overview](media-services-encode-asset.md)</span></span>

