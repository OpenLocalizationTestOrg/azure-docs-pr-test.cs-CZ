---
title: "Pomocí Azure Media Balíčkovač k provádění úloh statické balení | Microsoft Docs"
description: "Toto téma ukazuje různé úlohy, které jsou provedeny s Balíčkovač média Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 0582628e-a525-4a78-90ac-9f7fc1cd909f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/17/2017
ms.author: juliako
ms.openlocfilehash: cd36e46821eb85db523a5c84ec44895f68cc60e1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-azure-media-packager-to-accomplish-static-packaging-tasks"></a><span data-ttu-id="71730-103">Pomocí Azure Media Balíčkovač k provádění úloh statické balení</span><span class="sxs-lookup"><span data-stu-id="71730-103">Using Azure Media Packager to accomplish static packaging tasks</span></span>
> [!NOTE]
> <span data-ttu-id="71730-104">Koncové datum životnosti pro Microsoft Azure Media Balíčkovač a Microsoft Azure Media modulu pro šifrování se rozšířily na 1. března 2017.</span><span class="sxs-lookup"><span data-stu-id="71730-104">The end of life date for Microsoft Azure Media Packager and Microsoft Azure Media Encryptor has been extended to March 1, 2017.</span></span> <span data-ttu-id="71730-105">Před tímto datem funkce tyto procesorů se přidají k Media Encoder Standard (MES).</span><span class="sxs-lookup"><span data-stu-id="71730-105">Before that date, the functionalities of these processors will be added to Media Encoder Standard (MES).</span></span> <span data-ttu-id="71730-106">Zákazníci bude poskytnuta s pokyny o tom, jak migrovat své pracovní postupy k odesílání úloh do MES.</span><span class="sxs-lookup"><span data-stu-id="71730-106">Customers will be provided with instructions on how to migrate their workflows to send Jobs to MES.</span></span> <span data-ttu-id="71730-107">Formát možnosti převodu a šifrování může být také k dispozici prostřednictvím dynamické balením a dynamickým šifrováním.</span><span class="sxs-lookup"><span data-stu-id="71730-107">Format conversion and encryption capabilities may also be available through dynamic packaging and dynamic encryption.</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="71730-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="71730-108">Overview</span></span>
<span data-ttu-id="71730-109">Aby bylo možné poskytovat digitální video přes internet nutné médium komprimovat.</span><span class="sxs-lookup"><span data-stu-id="71730-109">In order to deliver digital video over the internet you must compress the media.</span></span> <span data-ttu-id="71730-110">Digitální video soubory jsou poměrně velké, může být příliš velký pro doručení přes internet nebo pro zařízení vašich zákazníků a zobrazeny správně.</span><span class="sxs-lookup"><span data-stu-id="71730-110">Digital video files are quite large and may be too big to deliver over the internet or for your customers’ devices to display properly.</span></span> <span data-ttu-id="71730-111">Kódování je proces komprimace videa a zvuku, takže vaši zákazníci mohou zobrazit médiu.</span><span class="sxs-lookup"><span data-stu-id="71730-111">Encoding is the process of compressing video and audio so your customers can view your media.</span></span> <span data-ttu-id="71730-112">Po zakódování video může být umístěn do kontejnerů jiný soubor.</span><span class="sxs-lookup"><span data-stu-id="71730-112">Once a video has been encoded it can be placed into different file containers.</span></span> <span data-ttu-id="71730-113">Proces uvádění kódovaného média do kontejneru se nazývá balení.</span><span class="sxs-lookup"><span data-stu-id="71730-113">The process of placing encoded media into a container is called packaging.</span></span> <span data-ttu-id="71730-114">Můžete například využít soubor MP4 a převádět je do technologie Smooth Streaming nebo HLS obsah pomocí Balíčkovač média Azure.</span><span class="sxs-lookup"><span data-stu-id="71730-114">For example, you can take an MP4 file and convert it into Smooth Streaming or HLS content by using the Azure Media Packager.</span></span> 

<span data-ttu-id="71730-115">Služba Media Services podporuje dynamických a statických balení.</span><span class="sxs-lookup"><span data-stu-id="71730-115">Media Services supports dynamic and static packaging.</span></span> <span data-ttu-id="71730-116">Při použití statické balení, budete muset vytvořit kopii vašeho obsahu v každé formátu vyžadovanou vašich zákazníků.</span><span class="sxs-lookup"><span data-stu-id="71730-116">When using static packaging you need to create a copy of your content in each format required by your customers.</span></span> <span data-ttu-id="71730-117">Při dynamickém balení, stačí je vytvoření asset, který obsahuje sadu souborů MP4 nebo technologie Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="71730-117">With dynamic packaging all you need is to create an asset that contains a set of adaptive bitrate MP4 or Smooth Streaming files.</span></span> <span data-ttu-id="71730-118">Pak na základě formátu určeného v manifestu nebo fragmentu požadavek streamingu na vyžádání serveru zajistí, aby vaši uživatelé datový proud obdrželi v protokolu, kterou si vyberou.</span><span class="sxs-lookup"><span data-stu-id="71730-118">Then, based on the specified format in the manifest or fragment request, the On-Demand Streaming server will ensure that your users receive the stream in the protocol they have chosen.</span></span> <span data-ttu-id="71730-119">Díky tomu pak stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="71730-119">As a result, you only need to store and pay for the files in single storage format and Media Services service will build and serve the appropriate response based on requests from a client.</span></span>

> [!NOTE]
> <span data-ttu-id="71730-120">Doporučuje se použít [dynamické balení](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="71730-120">It is recommended to use [dynamic packaging](media-services-dynamic-packaging-overview.md).</span></span>
> 
> 

<span data-ttu-id="71730-121">Existují však některé scénáře, které vyžadují statické balení:</span><span class="sxs-lookup"><span data-stu-id="71730-121">However, there are some scenarios that require static packaging:</span></span> 

* <span data-ttu-id="71730-122">Ověřování s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi zakódovaných pomocí externí kodéry (například pomocí kodéry třetích stran).</span><span class="sxs-lookup"><span data-stu-id="71730-122">Validating adaptive bitrate MP4s encoded with external encoders (for example, using third party encoders).</span></span>

<span data-ttu-id="71730-123">Statické balení také můžete provádět následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="71730-123">You can also use static packaging to perform the following tasks.</span></span> <span data-ttu-id="71730-124">Doporučujeme ale používat dynamické šifrování.</span><span class="sxs-lookup"><span data-stu-id="71730-124">However it is recommended to use dynamic encryption.</span></span>

* <span data-ttu-id="71730-125">Použití statické šifrování k ochraně Smooth a MPEG DASH s technologií PlayReady</span><span class="sxs-lookup"><span data-stu-id="71730-125">Using static encryption to protect your Smooth and MPEG DASH with PlayReady</span></span>
* <span data-ttu-id="71730-126">Použití statické šifrování k ochraně HLSv3 s AES-128</span><span class="sxs-lookup"><span data-stu-id="71730-126">Using static encryption to protect HLSv3 with AES-128</span></span>
* <span data-ttu-id="71730-127">Použití statické šifrování k ochraně HLSv3 s technologií PlayReady</span><span class="sxs-lookup"><span data-stu-id="71730-127">Using static encryption to protect HLSv3 with PlayReady</span></span>

## <a name="validating-adaptive-bitrate-mp4s-encoded-with-external-encoders"></a><span data-ttu-id="71730-128">Ověřování s adaptivní přenosovou rychlostí soubory MP4 rychlostmi zakódovaných pomocí externí kodéry</span><span class="sxs-lookup"><span data-stu-id="71730-128">Validating Adaptive Bitrate MP4s Encoded with External Encoders</span></span>
<span data-ttu-id="71730-129">Pokud chcete použít sadu souborů MP4 s adaptivní přenosovou rychlostí (více přenosovými rychlostmi), které nebyly kódovaný s kodéry Media Services, by měl ověřit vaše soubory před další zpracování.</span><span class="sxs-lookup"><span data-stu-id="71730-129">If you want to use a set of adaptive bitrate (multi-bitrate) MP4 files that were not encoded with Media Services' encoders, you should validate your files before further processing.</span></span> <span data-ttu-id="71730-130">Balíčkovač služby média můžete ověřit asset, který obsahuje sadu souborů MP4 a zkontrolujte, zda prostředku se dá zabalit technologie Smooth Streaming nebo HLS.</span><span class="sxs-lookup"><span data-stu-id="71730-130">The Media Services Packager can validate an asset that contains a set of MP4 files and check whether the asset can be packaged to Smooth Streaming or HLS.</span></span> <span data-ttu-id="71730-131">Pokud se úloha ověření nezdaří, se dokončí úlohu, která zpracovává úlohu se k chybě.</span><span class="sxs-lookup"><span data-stu-id="71730-131">If the validation task fails, the job that was processing the task will complete with an error.</span></span> <span data-ttu-id="71730-132">Kód XML, který definuje předvolbu pro úlohu ověřování najdete v [přednastavení úloh pro Azure Media Balíčkovač](http://msdn.microsoft.com/library/azure/hh973635.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="71730-132">The XML that defines the preset for the validation task can be found in the [Task Preset for Azure Media Packager](http://msdn.microsoft.com/library/azure/hh973635.aspx) topic.</span></span>

> [!NOTE]
> <span data-ttu-id="71730-133">Použít k vytvoření Media Encoder Standard nebo Balíčkovač služby média k ověření obsahu aby se zabránilo runtime problémy.</span><span class="sxs-lookup"><span data-stu-id="71730-133">Use the Media Encoder Standard to produce or the Media Services Packager to validate your content in order to avoid runtime issues.</span></span> <span data-ttu-id="71730-134">Pokud server streamingu na vyžádání není schopen analyzovat zdrojové soubory vašeho za běhu, zobrazí se chyba HTTP 1.1 "415 Nepodporovaný typ média".</span><span class="sxs-lookup"><span data-stu-id="71730-134">If the On-Demand Streaming server is not able to parse your source files at runtime, you will receive HTTP 1.1 error “415 Unsupported Media Type”.</span></span> <span data-ttu-id="71730-135">Opakovaně způsobuje selhání analyzovat zdrojové soubory vašeho serveru ovlivňuje výkon serveru streamingu na vyžádání a může snížit šířku pásma, které jsou k dispozici pro obsluhující ostatní požadavky.</span><span class="sxs-lookup"><span data-stu-id="71730-135">Repeatedly causing the server to fail to parse your source files affects performance of the On-Demand Streaming server and may reduce the bandwidth available to serving other requests.</span></span> <span data-ttu-id="71730-136">Azure Media Services nabízí o úrovni služeb smlouvy (SLA) na jeho streamování na vyžádání služby; Tato smlouva SLA nepůjde dodržet však v případě serveru je zneužití způsobem popsané výše.</span><span class="sxs-lookup"><span data-stu-id="71730-136">Azure Media Services offers a Service Level Agreement (SLA) on its On-Demand Streaming services; however, this SLA cannot be honored if the server is misused in the fashion described above.</span></span>
> 
> 

<span data-ttu-id="71730-137">V této části ukazuje, jak ke zpracování úloh ověření.</span><span class="sxs-lookup"><span data-stu-id="71730-137">This section shows how to process the validation task.</span></span> <span data-ttu-id="71730-138">Také ukazuje, jak zobrazit stav a úlohy, která je dokončena s JobStatus.Error chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="71730-138">It also shows how to see the status and the error message of the job that completes with JobStatus.Error.</span></span>

<span data-ttu-id="71730-139">Pro ověření, vaše soubory MP4 s Balíčkovač Media Services, musíte vytvořit vlastní soubor manifestu (.ism) a nahrát ho spolu s zdrojových souborů do účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="71730-139">To validate your MP4 files with Media Services Packager, you must create your own manifest (.ism) file and upload it together with the source files into the Media Services account.</span></span> <span data-ttu-id="71730-140">Níže je ukázkový soubor .ism produkovaný Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="71730-140">Below is a sample of the .ism file produced by the Media Encoder Standard.</span></span> <span data-ttu-id="71730-141">Názvy souborů jsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="71730-141">The file names are case sensitive.</span></span> <span data-ttu-id="71730-142">Taky se ujistěte, že je text v souboru .ism zakódovaných pomocí znakové sady UTF-8.</span><span class="sxs-lookup"><span data-stu-id="71730-142">Also, make sure the text in the .ism file is encoded with UTF-8.</span></span>

    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
    <!-- Tells the server that these input files are MP4s – specific to Dynamic Packaging -->
        <meta name="formats" content="mp4" /> 
      </head>
      <body>
        <switch>
          <video src="BigBuckBunny_1000.mp4" />
          <video src="BigBuckBunny_1500.mp4" />
          <video src="BigBuckBunny_2250.mp4" />
          <video src="BigBuckBunny_3400.mp4" />
          <video src="BigBuckBunny_400.mp4" />
          <video src="BigBuckBunny_650.mp4" />
          <audio src="BigBuckBunny_400.mp4" />
        </switch>
      </body>
    </smil>

<span data-ttu-id="71730-143">Až budete mít s adaptivní přenosovou rychlostí sady souborů MP4 můžete využít výhod dynamického balení.</span><span class="sxs-lookup"><span data-stu-id="71730-143">Once you have the adaptive bitrate MP4 set you can take advantage of Dynamic Packaging.</span></span> <span data-ttu-id="71730-144">Dynamické balení umožňuje doručovat datové proudy v zadaný protokol bez další balení.</span><span class="sxs-lookup"><span data-stu-id="71730-144">Dynamic Packaging allows you to deliver streams in the specified protocol without further packaging.</span></span> <span data-ttu-id="71730-145">Další informace najdete v tématu [dynamické balení](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="71730-145">For more information, see [dynamic packaging](media-services-dynamic-packaging-overview.md).</span></span>

<span data-ttu-id="71730-146">Následující příklad kódu používá rozšíření Azure Media Services .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="71730-146">The following code sample uses Azure Media Services .NET SDK Extensions.</span></span>  <span data-ttu-id="71730-147">Nezapomeňte aktualizovat kód tak, aby odkazoval na složku, kde jsou umístěné vaše vstupní soubory MP4 a soubor .ism.</span><span class="sxs-lookup"><span data-stu-id="71730-147">Make sure to update the code to point to the folder where your input MP4 files and .ism file are located.</span></span> <span data-ttu-id="71730-148">A také kde je umístěn soubor MediaPackager_ValidateTask.xml.</span><span class="sxs-lookup"><span data-stu-id="71730-148">And also to where your MediaPackager_ValidateTask.xml file is located.</span></span> <span data-ttu-id="71730-149">Tento soubor XML je definována v [přednastavení úloh pro Azure Media Balíčkovač](http://msdn.microsoft.com/library/azure/hh973635.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="71730-149">This XML file is defined in [Task Preset for Azure Media Packager](http://msdn.microsoft.com/library/azure/hh973635.aspx) topic.</span></span>

    using Microsoft.WindowsAzure.MediaServices.Client;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Xml.Linq;

    namespace MediaServicesStaticPackaging
    {
        class Program
        {
            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");

            // The MultibitrateMP4Files folder should also
            // contain the .ism manifest file.
            private static readonly string _multibitrateMP4s =
                Path.Combine(_mediaFiles, @"MultibitrateMP4Files");

            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations";

            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;

            // Media Services account information.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];

            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Use the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Ingest a set of multibitrate MP4s.
                //
                // Use the SDK extension method to create a new asset by 
                // uploading files from a local directory.
                IAsset multibitrateMP4sAsset = _context.Assets.CreateFromFolder(
                    _multibitrateMP4s,
                    AssetCreationOptions.None,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });

                // Use Azure Media Packager to validate the files.
                IAsset validatedMP4s =
                    ValidateMultibitrateMP4s(multibitrateMP4sAsset);

                // Publish the asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    validatedMP4s,
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));

                                     // Get the streaming URLs.
                Console.WriteLine("Smooth Streaming URL:");
                Console.WriteLine(validatedMP4s.GetSmoothStreamingUri().ToString());
                Console.WriteLine("MPEG DASH URL:");
                Console.WriteLine(validatedMP4s.GetMpegDashUri().ToString());
                Console.WriteLine("HLS URL:");
                Console.WriteLine(validatedMP4s.GetHlsUri().ToString());
            }

            public static IAsset ValidateMultibitrateMP4s(IAsset multibitrateMP4sAsset)
            {
                // Set .ism as a primary file 
                // in a multibitrate MP4 set.
                SetISMFileAsPrimary(multibitrateMP4sAsset);

                // Create a new job.
                IJob job = _context.Jobs.Create("MP4 validation and converstion to Smooth Stream job.");

                // Read the task configuration data into a string. 
                string configMp4Validation = File.ReadAllText(Path.Combine(
                        _configurationXMLFiles,
                        "MediaPackager_ValidateTask.xml"));

                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Create a task with the conversion details, using the configuration data. 
                ITask task = job.Tasks.AddNew("Mp4 Validation Task",
                    processor,
                    configMp4Validation,
                    TaskOptions.None);

                // Specify the input asset to be validated.
                task.InputAssets.Add(multibitrateMP4sAsset);

                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted). 
                task.OutputAssets.AddNew("Validated output asset",
                        AssetCreationOptions.None);

                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // If the validation task fails and job completes with JobState.Error,
                // display the error message and throw an exception.
                if (job.State == JobState.Error)
                {
                    Console.WriteLine("  Job ID: " + job.Id);
                    Console.WriteLine("  Name: " + job.Name);
                    Console.WriteLine("  State: " + job.State);

                    foreach (var jobTask in job.Tasks)
                    {
                        Console.WriteLine("  Task Id: " + jobTask.Id);
                        Console.WriteLine("  Name: " + jobTask.Name);
                        Console.WriteLine("  Progress: " + jobTask.Progress);
                        Console.WriteLine("  Configuration: " + jobTask.Configuration);
                        Console.WriteLine("  Running time: " + jobTask.RunningDuration);
                        if (jobTask.ErrorDetails != null)
                        {
                            foreach (var errordetail in jobTask.ErrorDetails)
                            {

                                Console.WriteLine("  Error Message:" + errordetail.Message);
                                Console.WriteLine("  Error Code:" + errordetail.Code);
                            }
                        }
                    }
                    throw new Exception("The specified multi-bitrate MP4 set is not valid.");
                }


                return job.OutputMediaAssets[0];
            }

            static void SetISMFileAsPrimary(IAsset asset)
            {
                var ismAssetFiles = asset.AssetFiles.ToList().
                    Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).ToArray();

                // The following code assigns the first .ism file as the primary file in the asset.
                // An asset should have one .ism file.  
                ismAssetFiles.First().IsPrimary = true;
                ismAssetFiles.First().Update();
            }
        }
    }

## <a name="using-static-encryption-to-protect-your-smooth-and-mpeg-dash-with-playready"></a><span data-ttu-id="71730-150">Použití statické šifrování k ochraně vaší Smooth a MPEG DASH s technologií PlayReady</span><span class="sxs-lookup"><span data-stu-id="71730-150">Using Static Encryption to Protect your Smooth and MPEG DASH with PlayReady</span></span>
<span data-ttu-id="71730-151">Pokud chcete chránit svůj obsah pomocí PlayReady, máte možnost volby použití [dynamického šifrování](media-services-protect-with-drm.md) (doporučená možnost) nebo statické šifrování (jak je popsáno v této části).</span><span class="sxs-lookup"><span data-stu-id="71730-151">If you want to protect your content with PlayReady, you have a choice of using [dynamic encryption](media-services-protect-with-drm.md) (the recommended option) or static encryption (as described in this section).</span></span>

<span data-ttu-id="71730-152">Příklad v této části kóduje soubor mezzanine (v této rozlišují MP4) do souborů MP4.</span><span class="sxs-lookup"><span data-stu-id="71730-152">The example in this section encodes a mezzanine file (in this case MP4) into adaptive bitrate MP4 files.</span></span> <span data-ttu-id="71730-153">Ji pak balíčky soubory MP4 s rychlostmi do technologie Smooth Streaming a potom šifruje technologie Smooth Streaming s technologií PlayReady.</span><span class="sxs-lookup"><span data-stu-id="71730-153">It then packages MP4s into Smooth Streaming and then encrypts Smooth Streaming with PlayReady.</span></span> <span data-ttu-id="71730-154">Výsledkem je budete moci stream technologie Smooth Streaming nebo MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="71730-154">As a result you are able to stream Smooth Streaming or MPEG DASH.</span></span>

<span data-ttu-id="71730-155">Služba Media Services nyní poskytuje službu k doručování licencí PlayReady společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="71730-155">Media Services now provides a service for delivering Microsoft PlayReady licenses.</span></span> <span data-ttu-id="71730-156">Příklad v tomto článku ukazuje, jak nakonfigurovat službu doručování licencí Media Services PlayReady (viz metodu ConfigureLicenseDeliveryService definované v kódu níže).</span><span class="sxs-lookup"><span data-stu-id="71730-156">The example in this article shows how to configure the Media Services PlayReady license delivery service (see the ConfigureLicenseDeliveryService method defined in the code below).</span></span> <span data-ttu-id="71730-157">Další informace o službě doručování licencí Media Services PlayReady najdete v tématu [pomocí dynamického šifrování PlayReady a službu doručování licencí](media-services-protect-with-drm.md).</span><span class="sxs-lookup"><span data-stu-id="71730-157">For more information about Media Services PlayReady license delivery service, see [Using PlayReady Dynamic Encryption and License Delivery Service](media-services-protect-with-drm.md).</span></span>

> [!NOTE]
> <span data-ttu-id="71730-158">K poskytování MPEG DASH šifrované pomocí PlayReady, nezapomeňte použít možnosti šifrování CENC nastavením vlastnosti useSencBox a adjustSubSamples (popsané v [přednastavení úloh pro Azure Media modulu pro šifrování](http://msdn.microsoft.com/library/azure/hh973610.aspx) tématu) na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="71730-158">To deliver MPEG DASH encrypted with PlayReady, make sure to use CENC options by setting the useSencBox and adjustSubSamples properties (described in the [Task Preset for Azure Media Encryptor](http://msdn.microsoft.com/library/azure/hh973610.aspx) topic) to true.</span></span>  
> 
> 

<span data-ttu-id="71730-159">Nezapomeňte aktualizovat následující kód tak, aby odkazoval na složku, kde je umístěna vaše vstupní soubor MP4.</span><span class="sxs-lookup"><span data-stu-id="71730-159">Make sure to update the following code to point to the folder where your input MP4 file is located.</span></span>

<span data-ttu-id="71730-160">A také k umístění souborů MediaPackager_MP4ToSmooth.xml a MediaEncryptor_PlayReadyProtection.xml.</span><span class="sxs-lookup"><span data-stu-id="71730-160">And also to where your MediaPackager_MP4ToSmooth.xml and MediaEncryptor_PlayReadyProtection.xml files are located.</span></span> <span data-ttu-id="71730-161">MediaPackager_MP4ToSmooth.xml je definována v [přednastavení úloh pro Azure Media Balíčkovač](http://msdn.microsoft.com/library/azure/hh973635.aspx) a MediaEncryptor_PlayReadyProtection.xml je definována v [přednastavení úloh pro Azure Media modulu pro šifrování](http://msdn.microsoft.com/library/azure/hh973610.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="71730-161">MediaPackager_MP4ToSmooth.xml is defined in [Task Preset for Azure Media Packager](http://msdn.microsoft.com/library/azure/hh973635.aspx) and MediaEncryptor_PlayReadyProtection.xml is defined in the [Task Preset for Azure Media Encryptor](http://msdn.microsoft.com/library/azure/hh973610.aspx) topic.</span></span> 

<span data-ttu-id="71730-162">V příkladu definuje UpdatePlayReadyConfigurationXMLFile metodu, která vám pomůže dynamicky aktualizovat soubor MediaEncryptor_PlayReadyProtection.xml.</span><span class="sxs-lookup"><span data-stu-id="71730-162">The example defines the UpdatePlayReadyConfigurationXMLFile method that you can use to dynamically update the MediaEncryptor_PlayReadyProtection.xml file.</span></span> <span data-ttu-id="71730-163">Pokud máte k dispozici klíče počáteční hodnoty, můžete použít metodu CommonEncryption.GeneratePlayReadyContentKey vygenerovat klíč k obsahu na základě keySeedValue a KeyId hodnot.</span><span class="sxs-lookup"><span data-stu-id="71730-163">If you have the key seed available, you can use the CommonEncryption.GeneratePlayReadyContentKey method to generate the content key based on the keySeedValue and KeyId values.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Xml.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

    namespace PlayReadyStaticEncryptAndKeyDeliverySvc
    {
        class Program
        {

            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");

            private static readonly string _singleMP4File =
                Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations\";


            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;

            // Media Services account information.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServiceAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServiceAccountKey"];

            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Use the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Encoding and encrypting assets //////////////////////
                // Load a single MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);

                // Encode an MP4 file to a set of multibitrate MP4s.
                // Then, package a set of MP4s to clear Smooth Streaming.
                IAsset clearSmoothStreamAsset =
                    ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);

                // Create a common encryption content key that is used 
                // a) to set the key values in the MediaEncryptor_PlayReadyProtection.xml file
                //    that is used for encryption.
                // b) to configure the license delivery service and 
                //
                Guid keyId;
                byte[] contentKey;

                IContentKey key = CreateCommonEncryptionKey(out keyId, out contentKey);

                // The content key authorization policy must be configured by you 
                // and met by the client in order for the PlayReady license
                // to be delivered to the client. 
                // In this example the Media Services PlayReady license delivery service is used.
                ConfigureLicenseDeliveryService(key);

                // Get the Media Services PlayReady license delivery URL.
                // This URL will be assigned to the licenseAcquisitionUrl property 
                // of the MediaEncryptor_PlayReadyProtection.xml file.
                Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

                // Update the MediaEncryptor_PlayReadyProtection.xml file with the key and URL info.
                UpdatePlayReadyConfigurationXMLFile(keyId, contentKey, acquisitionUrl);


                // Encrypt your clear Smooth Streaming to Smooth Streaming with PlayReady.
                IAsset outputAsset = CreateSmoothStreamEncryptedWithPlayReady(clearSmoothStreamAsset);


                // You can use the http://smf.cloudapp.net/healthmonitor player 
                // to test the smoothStreamURL URL.
                string smoothStreamURL = outputAsset.GetSmoothStreamingUri().ToString();
                Console.WriteLine("Smooth Streaming URL:");
                Console.WriteLine(smoothStreamURL);

                // You can use the http://dashif.org/reference/players/javascript/ player 
                // to test the dashURL URL.
                string dashURL = outputAsset.GetMpegDashUri().ToString();
                Console.WriteLine("MPEG DASH URL:");
                Console.WriteLine(dashURL);
            }

            /// <summary>
            /// Creates a job with 2 tasks: 
            /// 1 task - encodes a single MP4 to multibitrate MP4s,
            /// 2 task - packages MP4s to Smooth Streaming.
            /// </summary>
            /// <returns>The output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 to Smooth Streaming.");

                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set to Clear Smooth Stream.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);

                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // Get the output asset that contains the Smooth Streaming asset.
                return job.OutputMediaAssets[1];
            }

            /// <summary>
            /// Encrypts Smooth Stream with PlayReady.
            /// Then creates a Smooth Streaming Url.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>The output asset.</returns>
            public static IAsset CreateSmoothStreamEncryptedWithPlayReady(IAsset clearSmoothStreamAsset)
            {
                // Create a job.
                IJob job = _context.Jobs.Create("Encrypt to PlayReady Smooth Streaming.");

                // Add task 1 - Encrypt Smooth Streaming with PlayReady 
                IAsset encryptedSmoothAsset =
                    EncryptSmoothStreamWithPlayReadyTask(job, clearSmoothStreamAsset);

                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // The OutputMediaAssets[0] contains the desired asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    job.OutputMediaAssets[0],
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));

                return job.OutputMediaAssets[0];
            }

            /// <summary>
            /// Create a common encryption content key that is used 
            /// to set the key values in the MediaEncryptor_PlayReadyProtection.xml file
            /// that is used for encryption.
            /// </summary>
            /// <param name="keyId"></param>
            /// <param name="contentKey"></param>
            /// <returns></returns>
            public static IContentKey CreateCommonEncryptionKey(out Guid keyId, out byte[] contentKey)
            {
                keyId = Guid.NewGuid();
                contentKey = GetRandomBuffer(16);

                IContentKey key = _context.ContentKeys.Create(
                                        keyId,
                                        contentKey,
                                        "ContentKey",
                                        ContentKeyType.CommonEncryption);

                return key;
            }

            /// <summary>
            /// Update your configuration .xml file dynamically.
            /// </summary>
            public static void UpdatePlayReadyConfigurationXMLFile(Guid keyId, byte[] keyValue, Uri licenseAcquisitionUrl)
            {
                string xmlFileName = Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml");

                XNamespace xmlns = "http://schemas.microsoft.com/iis/media/v4/TM/TaskDefinition#";

                // Prepare the encryption task template
                XDocument doc = XDocument.Load(xmlFileName);

                var licenseAcquisitionUrlEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "licenseAcquisitionUrl")
                        .FirstOrDefault();
                var contentKeyEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "contentKey")
                        .FirstOrDefault();
                var keyIdEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "keyId")
                        .FirstOrDefault();

                // Update the "value" property.
                if (licenseAcquisitionUrlEl != null)
                    licenseAcquisitionUrlEl.Attribute("value").SetValue(licenseAcquisitionUrl.ToString());

                if (contentKeyEl != null)
                    contentKeyEl.Attribute("value").SetValue(Convert.ToBase64String(keyValue));

                if (keyIdEl != null)
                    keyIdEl.Attribute("value").SetValue(keyId);

                doc.Save(xmlFileName);
            }

            /// <summary>
            /// Uploads a single file.
            /// </summary>
            /// <param name="fileDir">The location of the files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify the following encryption options for the AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded to Azure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: The files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use the SDK extension method to create a new asset by 
                // uploading a mezzanine file from a local path.
                IAsset asset = _context.Assets.CreateFromFile(
                    fileDir,
                    assetCreationOptions,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });

                return asset;
            }

            /// <summary>
            /// Creates a task to encode to Adaptive Bitrate. 
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncodeMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);

                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 to Adaptive Bitrate Task",
                   encoder,
                   "Adaptive Streaming",
                   TaskOptions.None);

                // Specify the input Asset
                adpativeBitrateTask.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s",
                                        AssetCreationOptions.None);

                return abrAsset;
            }

            /// <summary>
            /// Creates a task to convert the MP4 file(s) to a Smooth Streaming asset.
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles,
                            "MediaPackager_MP4toSmooth.xml"));

                // Create a new Task to convert adaptive bitrate to Smooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 to Smooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);

                // Specify the input Asset, which is the output Asset from the first task
                smoothStreamingTask.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset smoothOutputAsset =
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Stream",
                        AssetCreationOptions.None);

                return smoothOutputAsset;
            }


            /// <summary>
            /// Creates a task to encrypt Smooth Streaming with PlayReady.
            /// Note: To deliver DASH, make sure to set the useSencBox and adjustSubSamples 
            /// configuration properties to true. 
            /// In this example, MediaEncryptor_PlayReadyProtection.xml contains configuration.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncryptSmoothStreamWithPlayReadyTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Encryptor.
                IMediaProcessor playreadyProcessor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaEncryptor);

                // Read the configuration XML.
                //
                // Note that the configuration defined in MediaEncryptor_PlayReadyProtection.xml
                // is using keySeedValue. It is recommended that you do this only for testing 
                // and not in production. For more information, see 
                // http://msdn.microsoft.com/library/windowsazure/dn189154.aspx.
                //
                string configPlayReady = File.ReadAllText(Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml"));

                ITask playreadyTask = job.Tasks.AddNew("My PlayReady Task",
                   playreadyProcessor,
                   configPlayReady,
                   TaskOptions.ProtectedConfiguration);

                playreadyTask.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.CommonEncryptionProtected.
                IAsset playreadyAsset = playreadyTask.OutputAssets.AddNew(
                                                "PlayReady Smooth Streaming",
                                                AssetCreationOptions.CommonEncryptionProtected);

                return playreadyAsset;
            }

            /// <summary>
            /// Configures authorization policy for the content key. 
            /// </summary>
            /// <param name="contentKey">The content key.</param>
            static public void ConfigureLicenseDeliveryService(IContentKey contentKey)
            {
                // Create ContentKeyAuthorizationPolicy with Open restrictions 
                // and create authorization policy          

                List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                    new ContentKeyAuthorizationPolicyRestriction 
                    { 
                        Name = "Open", 
                        KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                        Requirements = null
                    }
                };

                // Configure PlayReady license template.
                string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

                IContentKeyAuthorizationPolicyOption policyOption =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.PlayReadyLicense,
                            restrictions, newLicenseTemplate);

                IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                            ContentKeyAuthorizationPolicies.
                            CreateAsync("Deliver Common Content Key with no restrictions").
                            Result;


                contentKeyAuthorizationPolicy.Options.Add(policyOption);

                // Associate the content key authorization policy with the content key.
                contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                contentKey = contentKey.UpdateAsync().Result;
            }

            static private string ConfigurePlayReadyLicenseTemplate()
            {
                // The following code configures PlayReady License Template using .NET classes
                // and returns the XML string.

                PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
                PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();

                responseTemplate.LicenseTemplates.Add(licenseTemplate);

                return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
            }

            static private byte[] GetRandomBuffer(int length)
            {
                var returnValue = new byte[length];

                using (var rng =
                    new System.Security.Cryptography.RNGCryptoServiceProvider())
                {
                    rng.GetBytes(returnValue);
                }

                return returnValue;
            }
        }
    }

## <a name="using-static-encryption-to-protect-hlsv3-with-aes-128"></a><span data-ttu-id="71730-164">Použití statické šifrování k ochraně HLSv3 s AES-128</span><span class="sxs-lookup"><span data-stu-id="71730-164">Using Static Encryption to Protect HLSv3 with AES-128</span></span>
<span data-ttu-id="71730-165">Pokud chcete zašifrovat vaší HLS s AES-128, máte možnost volby použití dynamického šifrování (doporučená možnost) nebo statické šifrování (jak je znázorněno v této části).</span><span class="sxs-lookup"><span data-stu-id="71730-165">If you want to encrypt your HLS with AES-128, you have a choice of using dynamic encryption (the recommended option) or static encryption (as shown in this section).</span></span> <span data-ttu-id="71730-166">Pokud se rozhodnete používat dynamické šifrování, najdete v části [pomocí dynamického šifrování AES-128 a služba pro přenos klíče](media-services-protect-with-aes128.md).</span><span class="sxs-lookup"><span data-stu-id="71730-166">If you decide to use dynamic encryption, see [Using AES-128 Dynamic Encryption and Key Delivery Service](media-services-protect-with-aes128.md).</span></span>

> [!NOTE]
> <span data-ttu-id="71730-167">Chcete-li převést obsah na HLS, můžete musí nejdřív převést nebo zakódovat svůj obsah do technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="71730-167">In order to convert your content into HLS, you must first convert/encode your content into Smooth Streaming.</span></span>
> <span data-ttu-id="71730-168">Navíc pro HLS získat šifrované pomocí standardu AES nezapomeňte nastavit následující vlastnosti v souboru MediaPackager_SmoothToHLS.xml: nastavte na hodnotu true, nastavte hodnotu klíče a hodnoty keyuri tak, aby odkazovaly na váš server authentication\authorization vlastnost šifrovat.</span><span class="sxs-lookup"><span data-stu-id="71730-168">Also, for the HLS to get encrypted with AES make sure to set the following properties in your MediaPackager_SmoothToHLS.xml file: set the encrypt property to true, set the key value, and the keyuri value to point to your authentication\authorization server.</span></span>
> <span data-ttu-id="71730-169">Media Services vytvoříte soubor klíče a jeho následné uložení do kontejneru asset.</span><span class="sxs-lookup"><span data-stu-id="71730-169">Media Services will create a key file and place it in the asset container.</span></span> <span data-ttu-id="71730-170">Měli zkopírovat soubor /asset-containerguid/*.key na server (nebo vytvořit svůj vlastní soubor klíče) a pak odstraňte soubor *.key z kontejneru asset.</span><span class="sxs-lookup"><span data-stu-id="71730-170">You should copy the /asset-containerguid/*.key file to your server (or create your own key file) and then delete the *.key file from the asset container.</span></span>
> 
> 

<span data-ttu-id="71730-171">Příklad v této části kóduje soubor mezzanine (v tomto případě MP4) do multibitrate soubory MP4 a pak balíčky soubory MP4 s rychlostmi do technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="71730-171">The example in this section encodes a mezzanine file (in this case MP4) into multibitrate MP4 files and then packages MP4s into Smooth Streaming.</span></span> <span data-ttu-id="71730-172">Ji pak balíčky technologie Smooth Streaming do HTTP Live Streaming (HLS) šifrován Advanced Encryption (Standard AES) datového proudu 128bitové šifrování.</span><span class="sxs-lookup"><span data-stu-id="71730-172">It then packages Smooth Streaming into HTTP Live Streaming (HLS) encrypted with Advanced Encryption Standard (AES) 128-bit stream encryption.</span></span> <span data-ttu-id="71730-173">Nezapomeňte aktualizovat následující kód tak, aby odkazoval na složku, kde je umístěna vaše vstupní soubor MP4.</span><span class="sxs-lookup"><span data-stu-id="71730-173">Make sure to update the following code to point to the folder where your input MP4 file is located.</span></span> <span data-ttu-id="71730-174">A také k umístění MediaPackager_MP4ToSmooth.xml a MediaPackager_SmoothToHLS.xml konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="71730-174">And also to where your MediaPackager_MP4ToSmooth.xml and MediaPackager_SmoothToHLS.xml configuration files are located.</span></span> <span data-ttu-id="71730-175">Můžete najít definici těchto souborů v [přednastavení úloh pro Azure Media Balíčkovač](http://msdn.microsoft.com/library/azure/hh973635.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="71730-175">You can find the definition for these files in the [Task Preset for Azure Media Packager](http://msdn.microsoft.com/library/azure/hh973635.aspx) topic.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Xml.Linq;

    namespace MediaServicesContentProtection
    {
        class Program
        {
            // Paths to support files (within the above base path). You can use 
            // the provided sample media files from the "SupportFiles" folder, or 
            // provide paths to your own media files below to run these samples.

            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");

            private static readonly string _singleMP4File =
                Path.Combine(_mediaFiles, @"SingleMP4\BigBuckBunny.mp4");

            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations\";

            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;

            // Media Services account information.
            private static readonly string _mediaServicesAccountName = 
                ConfigurationManager.AppSettings["MediaServiceAccountName"];
            private static readonly string _mediaServicesAccountKey = 
                ConfigurationManager.AppSettings["MediaServiceAccountKey"];

            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName, 
                                _mediaServicesAccountKey);
                // Use the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Encoding and encrypting assets //////////////////////

                // Load an MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);

                // Encode an MP4 file to a set of multibitrate MP4s.
                // Then, package a set of MP4s to clear Smooth Streaming.
                IAsset clearSmoothStreamAsset = ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);

                // Create HLS encrypted with AES.
                IAsset HLSEncryptedWithAESAsset = CreateHLSEncryptedWithAES(clearSmoothStreamAsset);

                // You can use the following player to test the HLS with AES stream.
                // http://apps.microsoft.com/windows/app/3ivx-hls-player/f79ce7d0-2993-4658-bc4e-83dc182a0614 
                string hlsWithAESURL = HLSEncryptedWithAESAsset.GetHlsUri().ToString();
                Console.WriteLine("HLS with AES URL:");
                Console.WriteLine(hlsWithAESURL);
            }


            /// <summary>
            /// Creates a job with 2 tasks: 
            /// 1 task - encodes a single MP4 to multibitrate MP4s,
            /// 2 task - packages MP4s to Smooth Streaming.
            /// </summary>
            /// <returns>The output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 to Smooth Streaming.");

                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeSingleMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set to Clear Smooth Streaming.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);

                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // Get the output asset that contains Smooth Streaming.
                return job.OutputMediaAssets[1];
            }

            /// <summary>
            /// Encrypts an HLS with AES-128.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>The output asset.</returns>
            public static IAsset CreateHLSEncryptedWithAES(IAsset clearSmoothStreamAsset)
            {
                IJob job = _context.Jobs.Create("Encrypt to HLS with AES.");

                // Add task 1 - Package clear Smooth Streaming to HLS with AES.
                PackageSmoothStreamToHLS(job, clearSmoothStreamAsset);

                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // The OutputMediaAssets[0] contains the desired asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    job.OutputMediaAssets[0],
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));

                return job.OutputMediaAssets[0];
            }

            /// <summary>
            /// Uploads a single file.
            /// </summary>
            /// <param name="fileDir">The location of the files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify the following encryption options for the AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded to Azure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: The files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use the SDK extension method to create a new asset by 
                // uploading a mezzanine file from a local path.
                IAsset asset = _context.Assets.CreateFromFile(
                    fileDir,
                    assetCreationOptions,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });

                return asset;
            }

            /// <summary>
            /// Creates a task to encode to Adaptive Bitrate. 
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncodeSingleMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);

                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 to Adaptive Bitrate Task",
                   encoder,
                   "Adaptive Streaming",
                   TaskOptions.None);

                // Specify the input Asset
                adpativeBitrateTask.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s", 
                                        AssetCreationOptions.None);

                return abrAsset;
            }

            /// <summary>
            /// Creates a task to convert the MP4 file(s) to a Smooth Streaming asset.
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles, 
                            "MediaPackager_MP4toSmooth.xml"));

                // Create a new Task to convert adaptive bitrate to Smooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 to Smooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);

                // Specify the input Asset, which is the output Asset from the first task
                smoothStreamingTask.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset smoothOutputAsset = 
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Streaming", 
                        AssetCreationOptions.None);

                return smoothOutputAsset;
            }

            /// <summary>
            /// Converts Smooth Streaming to HLS.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The Smooth Streaming asset.</param>
            /// <returns>The asset that was packaged to HLS.</returns>
            private static IAsset PackageSmoothStreamToHLS(IJob job, IAsset smoothStreamAsset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Read the configuration data into a string. 
                // For the HLS to get encrypted with AES make sure to set the
                // encrypt configuration property to true.
                //
                // In production, it is recommended to do the following:
                //    Set a Key url for your authn/authz server.
                //    Copy the /asset-containerguid/*.key file to your server (or craft a key file for yourself).
                //    Delete *.key from the asset container.
                //
                string configuration = File.ReadAllText(Path.Combine(_configurationXMLFiles, @"MediaPackager_SmoothToHLS.xml"));

                // Create a task with the encoding details, using a configuration file.
                ITask task = job.Tasks.AddNew("My Smooth Streaming to HLS Task",
                   processor,
                   configuration,
                   TaskOptions.ProtectedConfiguration);

                // Specify the input asset to be encoded.
                task.InputAssets.Add(smoothStreamAsset);

                // Add an output asset to contain the results of the job. 
                IAsset outputAsset = 
                    task.OutputAssets.AddNew("HLS asset", AssetCreationOptions.None);


                return outputAsset;
            }
        }
    }

## <a name="using-static-encryption-to-protect-hlsv3-with-playready"></a><span data-ttu-id="71730-176">Použití statické šifrování k ochraně HLSv3 s technologií PlayReady</span><span class="sxs-lookup"><span data-stu-id="71730-176">Using Static Encryption to Protect HLSv3 with PlayReady</span></span>
<span data-ttu-id="71730-177">Pokud chcete chránit svůj obsah pomocí PlayReady, máte možnost volby použití [dynamického šifrování](media-services-protect-with-drm.md) (doporučená možnost) nebo statické šifrování (jak je popsáno v této části).</span><span class="sxs-lookup"><span data-stu-id="71730-177">If you want to protect your content with PlayReady, you have a choice of using [dynamic encryption](media-services-protect-with-drm.md) (the recommended option) or static encryption (as described in this section).</span></span>

> [!NOTE]
> <span data-ttu-id="71730-178">Pokud chcete chránit obsah pomocí PlayReady můžete musí nejdřív převést nebo zakódovat svůj obsah do formátu, technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="71730-178">In order to protect your content using PlayReady you must first convert/encode your content into a Smooth Streaming format.</span></span>
> 
> 

<span data-ttu-id="71730-179">Příklad v této části kóduje soubor mezzanine (v této rozlišují MP4) do souborů multibitrate MP4.</span><span class="sxs-lookup"><span data-stu-id="71730-179">The example in this section encodes a mezzanine file (in this case MP4) into multibitrate MP4 files.</span></span> <span data-ttu-id="71730-180">Potom balíčky soubory MP4 s rychlostmi do technologie Smooth Streaming a šifruje technologie Smooth Streaming s technologií PlayReady.</span><span class="sxs-lookup"><span data-stu-id="71730-180">It then packages MP4s into Smooth Streaming and encrypts Smooth Streaming with PlayReady.</span></span> <span data-ttu-id="71730-181">K vytvoření HTTP Live Streaming (HLS) šifrovat pomocí PlayReady, technologie Smooth Streaming PlayReady asset musí zabalené do HLS.</span><span class="sxs-lookup"><span data-stu-id="71730-181">To produce HTTP Live Streaming (HLS) encrypted with PlayReady, the PlayReady Smooth Streaming asset needs to be packaged into HLS.</span></span> <span data-ttu-id="71730-182">Toto téma ukazuje, jak provést tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="71730-182">This topic demonstrates how to perform all these steps.</span></span>

<span data-ttu-id="71730-183">Služba Media Services nyní poskytuje službu k doručování licencí PlayReady společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="71730-183">Media Services now provides a service for delivering Microsoft PlayReady licenses.</span></span> <span data-ttu-id="71730-184">Příklad v tomto článku ukazuje, jak nakonfigurovat službu doručování licencí Media Services PlayReady (najdete v článku **ConfigureLicenseDeliveryService** metoda definované v kódu níže).</span><span class="sxs-lookup"><span data-stu-id="71730-184">The example in this article shows how to configure the Media Services PlayReady license delivery service (see the **ConfigureLicenseDeliveryService** method defined in the code below).</span></span> 

<span data-ttu-id="71730-185">Nezapomeňte aktualizovat následující kód tak, aby odkazoval na složku, kde je umístěna vaše vstupní soubor MP4.</span><span class="sxs-lookup"><span data-stu-id="71730-185">Make sure to update the following code to point to the folder where your input MP4 file is located.</span></span> <span data-ttu-id="71730-186">A také k umístění MediaPackager_MP4ToSmooth.xml, MediaPackager_SmoothToHLS.xml a MediaEncryptor_PlayReadyProtection.xml souborů.</span><span class="sxs-lookup"><span data-stu-id="71730-186">And also to where your MediaPackager_MP4ToSmooth.xml, MediaPackager_SmoothToHLS.xml, and MediaEncryptor_PlayReadyProtection.xml files are located.</span></span> <span data-ttu-id="71730-187">MediaPackager_MP4ToSmooth.xml a MediaPackager_SmoothToHLS.xml jsou definovány v [přednastavení úloh pro Azure Media Balíčkovač](http://msdn.microsoft.com/library/azure/hh973635.aspx) a MediaEncryptor_PlayReadyProtection.xml je definována v [přednastavení úloh pro Azure Media Modul pro šifrování](http://msdn.microsoft.com/library/azure/hh973610.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="71730-187">MediaPackager_MP4ToSmooth.xml and MediaPackager_SmoothToHLS.xml are defined in [Task Preset for Azure Media Packager](http://msdn.microsoft.com/library/azure/hh973635.aspx) and MediaEncryptor_PlayReadyProtection.xml is defined in the [Task Preset for Azure Media Encryptor](http://msdn.microsoft.com/library/azure/hh973610.aspx) topic.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Xml.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

    namespace MediaServicesContentProtection
    {
        class Program
        {
            // Paths to support files (within the above base path). You can use 
            // the provided sample media files from the "SupportFiles" folder, or 
            // provide paths to your own media files below to run these samples.

            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");

            private static readonly string _singleMP4File =
                Path.Combine(_mediaFiles, @"SingleMP4\BigBuckBunny.mp4");

            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations\";


            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;

            // Media Services account information.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServiceAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServiceAccountKey"];

            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Load an MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);

                // Encode an MP4 file to a set of multibitrate MP4s.
                // Then, package a set of MP4s to clear Smooth Streaming.
                IAsset clearSmoothStreamAsset = ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);

                // Create a common encryption content key that is used 
                // a) to set the key values in the MediaEncryptor_PlayReadyProtection.xml file
                //    that is used for encryption.
                // b) to configure the license delivery service and 
                //
                Guid keyId;
                byte[] contentKey;

                IContentKey key = CreateCommonEncryptionKey(out keyId, out contentKey);

                // The content key authorization policy must be configured by you 
                // and met by the client in order for the PlayReady license
                // to be delivered to the client. 
                // In this example the Media Services PlayReady license delivery service is used.
                ConfigureLicenseDeliveryService(key);

                // Get the Media Services PlayReady license delivery URL.
                // This URL will be assigned to the licenseAcquisitionUrl property 
                // of the MediaEncryptor_PlayReadyProtection.xml file.
                Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

                // Update the MediaEncryptor_PlayReadyProtection.xml file with the key and URL info.
                UpdatePlayReadyConfigurationXMLFile(keyId, contentKey, acquisitionUrl);

                // Create HLS encrypted with PlayReady.
                IAsset playReadyHLSAsset = CreateHLSEncryptedWithPlayReady(clearSmoothStreamAsset);
                //
                string hlsWithPlayReadyURL = playReadyHLSAsset.GetHlsUri().ToString();
                Console.WriteLine("HLS with PlayReady URL:");
                Console.WriteLine(hlsWithPlayReadyURL);
            }

            /// <summary>
            /// Creates a job with 2 tasks: 
            /// 1 task - encodes a single MP4 to multibitrate MP4s,
            /// 2 task - packages MP4s to Smooth Streaming.
            /// </summary>
            /// <returns>The output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 to Smooth Streaming.");

                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeSingleMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set to Clear Smooth Streaming.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);

                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // Get the output asset that contains Smooth Streaming.
                return job.OutputMediaAssets[1];
            }

            /// <summary>
            /// Create a common encryption content key that is used 
            /// to set the key values in the MediaEncryptor_PlayReadyProtection.xml file
            /// that is used for encryption.
            /// </summary>
            /// <param name="keyId"></param>
            /// <param name="contentKey"></param>
            /// <returns></returns>
            public static IContentKey CreateCommonEncryptionKey(out Guid keyId, out byte[] contentKey)
            {
                keyId = Guid.NewGuid();
                contentKey = GetRandomBuffer(16);

                IContentKey key = _context.ContentKeys.Create(
                                        keyId,
                                        contentKey,
                                        "ContentKey",
                                        ContentKeyType.CommonEncryption);

                return key;
            }

            /// <summary>
            /// Update your configuration .xml file dynamically.
            /// </summary>
            public static void UpdatePlayReadyConfigurationXMLFile(Guid keyId, byte[] keyValue, Uri licenseAcquisitionUrl)
            {
                string xmlFileName = Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml");

                XNamespace xmlns = "http://schemas.microsoft.com/iis/media/v4/TM/TaskDefinition#";

                // Prepare the encryption task template
                XDocument doc = XDocument.Load(xmlFileName);

                var licenseAcquisitionUrlEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "licenseAcquisitionUrl")
                        .FirstOrDefault();
                var contentKeyEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "contentKey")
                        .FirstOrDefault();
                var keyIdEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "keyId")
                        .FirstOrDefault();

                // Update the "value" property.
                if (licenseAcquisitionUrlEl != null)
                    licenseAcquisitionUrlEl.Attribute("value").SetValue(licenseAcquisitionUrl.ToString());

                if (contentKeyEl != null)
                    contentKeyEl.Attribute("value").SetValue(Convert.ToBase64String(keyValue));

                if (keyIdEl != null)
                    keyIdEl.Attribute("value").SetValue(keyId);

                doc.Save(xmlFileName);
            }

            /// <summary>
            // Encrypts clear Smooth Streaming to Smooth Streaming with PlayReady.
            // Then, packages the PlayReady Smooth Streaming to HLS with PlayReady.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>The output asset.</returns>
            public static IAsset CreateHLSEncryptedWithPlayReady(IAsset clearSmoothStreamAsset)
            {
                IJob job = _context.Jobs.Create("Encrypt to HLS with PlayReady.");

                // Add task 1 - Encrypt Smooth Streaming with PlayReady 
                IAsset encryptedSmoothAsset =
                    EncryptSmoothStreamWithPlayReadyTask(job, clearSmoothStreamAsset);

                // Add task 2 - Package to HLS with PlayReady.
                PackageSmoothStreamToHLS(job, encryptedSmoothAsset);

                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // Since we had two tasks, the OutputMediaAssets[1]
                // contains the desired asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    job.OutputMediaAssets[1],
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));

                return job.OutputMediaAssets[1];
            }

            /// <summary>
            /// Uploads a single file.
            /// </summary>
            /// <param name="fileDir">The location of the files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify the following encryption options for the AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded to Azure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: The files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use the SDK extension method to create a new asset by 
                // uploading a mezzanine file from a local path.
                IAsset asset = _context.Assets.CreateFromFile(
                    fileDir,
                    assetCreationOptions,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });


                return asset;

            }
            /// <summary>
            /// Creates a task to encode to Adaptive Bitrate. 
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncodeSingleMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);

                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 to Adaptive Bitrate Task",
                   encoder,
                   "Adaptive Streaming",
                   TaskOptions.None);

                // Specify the input Asset
                adpativeBitrateTask.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s",
                                        AssetCreationOptions.None);

                return abrAsset;
            }

            /// <summary>
            /// Creates a task to convert the MP4 file(s) to a Smooth Streaming asset.
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles,
                            "MediaPackager_MP4toSmooth.xml"));

                // Create a new Task to convert adaptive bitrate to Smooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 to Smooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);

                // Specify the input Asset, which is the output Asset from the first task
                smoothStreamingTask.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset smoothOutputAsset =
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Streaming",
                        AssetCreationOptions.None);

                return smoothOutputAsset;
            }


            /// <summary>
            /// Converts Smooth Stream to HLS.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The Smooth Stream asset.</param>
            /// <returns>The asset that was packaged to HLS.</returns>
            private static IAsset PackageSmoothStreamToHLS(IJob job, IAsset smoothStreamAsset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Read the configuration data into a string. 
                //
                string configuration = File.ReadAllText(
                            Path.Combine(_configurationXMLFiles,
                                        @"MediaPackager_SmoothToHLS.xml"));

                // Create a task with the encoding details, using a configuration file.
                ITask task = job.Tasks.AddNew("My Smooth to HLS Task",
                   processor,
                   configuration,
                   TaskOptions.ProtectedConfiguration);

                // Specify the input asset to be encoded.
                task.InputAssets.Add(smoothStreamAsset);

                // Add an output asset to contain the results of the job. 
                IAsset outputAsset =
                    task.OutputAssets.AddNew("HLS asset", AssetCreationOptions.None);


                return outputAsset;
            }

            /// <summary>
            /// Creates a task to encrypt Smooth Streaming with PlayReady.
            /// Note: Do deliver DASH, make sure to set the useSencBox and adjustSubSamples 
            /// configuration properties to true.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncryptSmoothStreamWithPlayReadyTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Encryptor.
                IMediaProcessor playreadyProcessor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaEncryptor);

                // Read the configuration XML.
                //
                // Note that the configuration defined in MediaEncryptor_PlayReadyProtection.xml
                // is using keySeedValue. It is recommended that you do this only for testing 
                // and not in production. For more information, see 
                // http://msdn.microsoft.com/library/windowsazure/dn189154.aspx.
                //
                string configPlayReady = File.ReadAllText(Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml"));

                ITask playreadyTask = job.Tasks.AddNew("My PlayReady Task",
                   playreadyProcessor,
                   configPlayReady,
                   TaskOptions.ProtectedConfiguration);

                playreadyTask.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.CommonEncryptionProtected.
                IAsset playreadyAsset = playreadyTask.OutputAssets.AddNew(
                                                "PlayReady Smooth Streaming",
                                                AssetCreationOptions.CommonEncryptionProtected);


                return playreadyAsset;
            }


            /// <summary>
            /// Configures authorization policy for the content key. 
            /// </summary>
            /// <param name="contentKey">The content key.</param>
            static public void ConfigureLicenseDeliveryService(IContentKey contentKey)
            {
                // Create ContentKeyAuthorizationPolicy with Open restrictions 
                // and create authorization policy          

                List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                    new ContentKeyAuthorizationPolicyRestriction 
                    { 
                        Name = "Open", 
                        KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                        Requirements = null
                    }
                };

                // Configure PlayReady license template.
                string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

                IContentKeyAuthorizationPolicyOption policyOption =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.PlayReadyLicense,
                            restrictions, newLicenseTemplate);

                IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                            ContentKeyAuthorizationPolicies.
                            CreateAsync("Deliver Common Content Key with no restrictions").
                            Result;


                contentKeyAuthorizationPolicy.Options.Add(policyOption);

                // Associate the content key authorization policy with the content key.
                contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                contentKey = contentKey.UpdateAsync().Result;
            }

            static private string ConfigurePlayReadyLicenseTemplate()
            {
                // The following code configures PlayReady License Template using .NET classes
                // and returns the XML string.

                PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
                PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();

                responseTemplate.LicenseTemplates.Add(licenseTemplate);

                return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
            }
            static private byte[] GetRandomBuffer(int length)
            {
                var returnValue = new byte[length];

                using (var rng =
                    new System.Security.Cryptography.RNGCryptoServiceProvider())
                {
                    rng.GetBytes(returnValue);
                }

                return returnValue;
            }

        }
    }

## <a name="media-services-learning-paths"></a><span data-ttu-id="71730-188">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="71730-188">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="71730-189">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="71730-189">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

