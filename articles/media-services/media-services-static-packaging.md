---
title: "aaaUsing Azure Media Balíčkovač tooaccomplish statické balení úlohy | Microsoft Docs"
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
ms.openlocfilehash: 64e8ba217bcd3074f5819ac3b74d2969432db5d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-media-packager-tooaccomplish-static-packaging-tasks"></a><span data-ttu-id="9a371-103">Pomocí Azure Media Balíčkovač tooaccomplish statické balení úloh</span><span class="sxs-lookup"><span data-stu-id="9a371-103">Using Azure Media Packager tooaccomplish static packaging tasks</span></span>
> [!NOTE]
> <span data-ttu-id="9a371-104">Hello konci životnosti datum programu Microsoft Azure Media Balíčkovač a Microsoft Azure Media modulu pro šifrování se rozšířily tooMarch 1, 2017.</span><span class="sxs-lookup"><span data-stu-id="9a371-104">hello end of life date for Microsoft Azure Media Packager and Microsoft Azure Media Encryptor has been extended tooMarch 1, 2017.</span></span> <span data-ttu-id="9a371-105">Před tímto datem hello funkce tyto procesorů přidá tooMedia Encoder Standard (MES).</span><span class="sxs-lookup"><span data-stu-id="9a371-105">Before that date, hello functionalities of these processors will be added tooMedia Encoder Standard (MES).</span></span> <span data-ttu-id="9a371-106">Zákazníci se poskytnout s pokyny, jak toomigrate jejich tooMES toosend úlohy pracovních postupů.</span><span class="sxs-lookup"><span data-stu-id="9a371-106">Customers will be provided with instructions on how toomigrate their workflows toosend Jobs tooMES.</span></span> <span data-ttu-id="9a371-107">Formát možnosti převodu a šifrování může být také k dispozici prostřednictvím dynamické balením a dynamickým šifrováním.</span><span class="sxs-lookup"><span data-stu-id="9a371-107">Format conversion and encryption capabilities may also be available through dynamic packaging and dynamic encryption.</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="9a371-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="9a371-108">Overview</span></span>
<span data-ttu-id="9a371-109">V pořadí toodeliver digitální video přes hello internet musí komprimovat hello média.</span><span class="sxs-lookup"><span data-stu-id="9a371-109">In order toodeliver digital video over hello internet you must compress hello media.</span></span> <span data-ttu-id="9a371-110">Digitální video soubory jsou velmi velké a může být příliš velký toodeliver přes hello internet nebo pro vaše zákazníky zařízení toodisplay správně.</span><span class="sxs-lookup"><span data-stu-id="9a371-110">Digital video files are quite large and may be too big toodeliver over hello internet or for your customers’ devices toodisplay properly.</span></span> <span data-ttu-id="9a371-111">Kódování je proces hello komprimace videa a zvuku, takže vaši zákazníci mohou zobrazit médiu.</span><span class="sxs-lookup"><span data-stu-id="9a371-111">Encoding is hello process of compressing video and audio so your customers can view your media.</span></span> <span data-ttu-id="9a371-112">Po zakódování video může být umístěn do kontejnerů jiný soubor.</span><span class="sxs-lookup"><span data-stu-id="9a371-112">Once a video has been encoded it can be placed into different file containers.</span></span> <span data-ttu-id="9a371-113">balení se nazývá Hello proces uvádění kódovaného média do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="9a371-113">hello process of placing encoded media into a container is called packaging.</span></span> <span data-ttu-id="9a371-114">Můžete například využít soubor MP4 a převádět je do technologie Smooth Streaming nebo HLS obsah pomocí hello Balíčkovač média Azure.</span><span class="sxs-lookup"><span data-stu-id="9a371-114">For example, you can take an MP4 file and convert it into Smooth Streaming or HLS content by using hello Azure Media Packager.</span></span> 

<span data-ttu-id="9a371-115">Služba Media Services podporuje dynamických a statických balení.</span><span class="sxs-lookup"><span data-stu-id="9a371-115">Media Services supports dynamic and static packaging.</span></span> <span data-ttu-id="9a371-116">Při používání statické balení musíte toocreate kopii vašeho obsahu v každé formátu vyžaduje vaše zákazníky.</span><span class="sxs-lookup"><span data-stu-id="9a371-116">When using static packaging you need toocreate a copy of your content in each format required by your customers.</span></span> <span data-ttu-id="9a371-117">S dynamické balení, stačí je toocreate asset, který obsahuje sadu souborů MP4 nebo technologie Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="9a371-117">With dynamic packaging all you need is toocreate an asset that contains a set of adaptive bitrate MP4 or Smooth Streaming files.</span></span> <span data-ttu-id="9a371-118">Potom podle hello formátu určeného v manifestu hello nebo fragmentovat požadavku, hello streamování na vyžádání, server se zajistit, že uživatelé datového proudu hello hello protokolu, kterou si vyberou.</span><span class="sxs-lookup"><span data-stu-id="9a371-118">Then, based on hello specified format in hello manifest or fragment request, hello On-Demand Streaming server will ensure that your users receive hello stream in hello protocol they have chosen.</span></span> <span data-ttu-id="9a371-119">V důsledku toho stačí pouze toostore a platím hello souborů v jednom úložném formátu a služba Media Services bude sestavovat a dodávat hello odpovídající reakci na požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="9a371-119">As a result, you only need toostore and pay for hello files in single storage format and Media Services service will build and serve hello appropriate response based on requests from a client.</span></span>

> [!NOTE]
> <span data-ttu-id="9a371-120">Je doporučeno toouse [dynamické balení](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9a371-120">It is recommended toouse [dynamic packaging](media-services-dynamic-packaging-overview.md).</span></span>
> 
> 

<span data-ttu-id="9a371-121">Existují však některé scénáře, které vyžadují statické balení:</span><span class="sxs-lookup"><span data-stu-id="9a371-121">However, there are some scenarios that require static packaging:</span></span> 

* <span data-ttu-id="9a371-122">Ověřování s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi zakódovaných pomocí externí kodéry (například pomocí kodéry třetích stran).</span><span class="sxs-lookup"><span data-stu-id="9a371-122">Validating adaptive bitrate MP4s encoded with external encoders (for example, using third party encoders).</span></span>

<span data-ttu-id="9a371-123">Můžete také použít statické balení tooperform hello následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="9a371-123">You can also use static packaging tooperform hello following tasks.</span></span> <span data-ttu-id="9a371-124">Doporučujeme ale toouse dynamické šifrování.</span><span class="sxs-lookup"><span data-stu-id="9a371-124">However it is recommended toouse dynamic encryption.</span></span>

* <span data-ttu-id="9a371-125">Pomocí statické šifrování tooprotect Smooth a MPEG DASH s technologií PlayReady</span><span class="sxs-lookup"><span data-stu-id="9a371-125">Using static encryption tooprotect your Smooth and MPEG DASH with PlayReady</span></span>
* <span data-ttu-id="9a371-126">Použití statické šifrování tooprotect HLSv3 s AES-128</span><span class="sxs-lookup"><span data-stu-id="9a371-126">Using static encryption tooprotect HLSv3 with AES-128</span></span>
* <span data-ttu-id="9a371-127">Použití statické šifrování tooprotect HLSv3 s technologií PlayReady</span><span class="sxs-lookup"><span data-stu-id="9a371-127">Using static encryption tooprotect HLSv3 with PlayReady</span></span>

## <a name="validating-adaptive-bitrate-mp4s-encoded-with-external-encoders"></a><span data-ttu-id="9a371-128">Ověřování s adaptivní přenosovou rychlostí soubory MP4 rychlostmi zakódovaných pomocí externí kodéry</span><span class="sxs-lookup"><span data-stu-id="9a371-128">Validating Adaptive Bitrate MP4s Encoded with External Encoders</span></span>
<span data-ttu-id="9a371-129">Pokud chcete toouse sadu souborů MP4 s adaptivní přenosovou rychlostí (více přenosovými rychlostmi), které nebyly kódovaný s kodéry Media Services, by měl ověřit vaše soubory před další zpracování.</span><span class="sxs-lookup"><span data-stu-id="9a371-129">If you want toouse a set of adaptive bitrate (multi-bitrate) MP4 files that were not encoded with Media Services' encoders, you should validate your files before further processing.</span></span> <span data-ttu-id="9a371-130">Hello Balíčkovač Media Services můžete ověřit asset, který obsahuje sadu souborů MP4 a zkontrolujte, zda text hello asset může být zabalené tooSmooth Streaming nebo HLS.</span><span class="sxs-lookup"><span data-stu-id="9a371-130">hello Media Services Packager can validate an asset that contains a set of MP4 files and check whether hello asset can be packaged tooSmooth Streaming or HLS.</span></span> <span data-ttu-id="9a371-131">Pokud se úloha ověření hello nezdaří, hello úlohu, která byla zpracování úloh hello dokončí s chybou.</span><span class="sxs-lookup"><span data-stu-id="9a371-131">If hello validation task fails, hello job that was processing hello task will complete with an error.</span></span> <span data-ttu-id="9a371-132">Hello XML, který definuje hello přednastavení úloh ověření hello naleznete v hello [přednastavení úloh pro Azure Media Balíčkovač](http://msdn.microsoft.com/library/azure/hh973635.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="9a371-132">hello XML that defines hello preset for hello validation task can be found in hello [Task Preset for Azure Media Packager](http://msdn.microsoft.com/library/azure/hh973635.aspx) topic.</span></span>

> [!NOTE]
> <span data-ttu-id="9a371-133">Pomocí Media Encoder Standard tooproduce hello nebo hello Media Services Balíčkovač toovalidate obsah v pořadí tooavoid runtime problémy.</span><span class="sxs-lookup"><span data-stu-id="9a371-133">Use hello Media Encoder Standard tooproduce or hello Media Services Packager toovalidate your content in order tooavoid runtime issues.</span></span> <span data-ttu-id="9a371-134">Pokud server streamingu na vyžádání hello není možné tooparse vaší zdrojové soubory v době běhu, zobrazí se chyba HTTP 1.1 "415 Nepodporovaný typ média".</span><span class="sxs-lookup"><span data-stu-id="9a371-134">If hello On-Demand Streaming server is not able tooparse your source files at runtime, you will receive HTTP 1.1 error “415 Unsupported Media Type”.</span></span> <span data-ttu-id="9a371-135">Zdrojové soubory vašeho opakovaně způsobuje hello server toofail tooparse ovlivňuje výkon serveru hello streamování na vyžádání a může snížit tooserving dostupné šířky pásma hello ostatní požadavky.</span><span class="sxs-lookup"><span data-stu-id="9a371-135">Repeatedly causing hello server toofail tooparse your source files affects performance of hello On-Demand Streaming server and may reduce hello bandwidth available tooserving other requests.</span></span> <span data-ttu-id="9a371-136">Azure Media Services nabízí o úrovni služeb smlouvy (SLA) na jeho streamování na vyžádání služby; této dohody však nelze dodržet, pokud hello server došlo ke zneužití způsobem hello popsané výše.</span><span class="sxs-lookup"><span data-stu-id="9a371-136">Azure Media Services offers a Service Level Agreement (SLA) on its On-Demand Streaming services; however, this SLA cannot be honored if hello server is misused in hello fashion described above.</span></span>
> 
> 

<span data-ttu-id="9a371-137">Tato část uvádí, jak tooprocess hello úloha ověření.</span><span class="sxs-lookup"><span data-stu-id="9a371-137">This section shows how tooprocess hello validation task.</span></span> <span data-ttu-id="9a371-138">Také ukazuje, jak toosee hello stavu a hello chybová zpráva hello úlohy, která je dokončena s JobStatus.Error.</span><span class="sxs-lookup"><span data-stu-id="9a371-138">It also shows how toosee hello status and hello error message of hello job that completes with JobStatus.Error.</span></span>

<span data-ttu-id="9a371-139">toovalidate vaše MP4 soubory s Balíčkovač Media Services, musíte vytvořit svůj vlastní soubor manifestu (.ism) a nahrát ho spolu s hello zdrojové soubory do hello účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="9a371-139">toovalidate your MP4 files with Media Services Packager, you must create your own manifest (.ism) file and upload it together with hello source files into hello Media Services account.</span></span> <span data-ttu-id="9a371-140">Níže je ukázkový soubor .ism hello vyprodukované hello Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="9a371-140">Below is a sample of hello .ism file produced by hello Media Encoder Standard.</span></span> <span data-ttu-id="9a371-141">názvy souborů Hello rozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="9a371-141">hello file names are case sensitive.</span></span> <span data-ttu-id="9a371-142">Také zkontrolujte, zda text hello v souboru .ism hello kódovaný pomocí znakové sady UTF-8.</span><span class="sxs-lookup"><span data-stu-id="9a371-142">Also, make sure hello text in hello .ism file is encoded with UTF-8.</span></span>

    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
    <!-- Tells hello server that these input files are MP4s – specific tooDynamic Packaging -->
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

<span data-ttu-id="9a371-143">Jakmile máte hello s adaptivní přenosovou rychlostí sady souborů MP4 můžete využít výhod dynamického balení.</span><span class="sxs-lookup"><span data-stu-id="9a371-143">Once you have hello adaptive bitrate MP4 set you can take advantage of Dynamic Packaging.</span></span> <span data-ttu-id="9a371-144">Dynamické balení vám umožní toodeliver datových proudů hello zadaný protokol bez další balení.</span><span class="sxs-lookup"><span data-stu-id="9a371-144">Dynamic Packaging allows you toodeliver streams in hello specified protocol without further packaging.</span></span> <span data-ttu-id="9a371-145">Další informace najdete v tématu [dynamické balení](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9a371-145">For more information, see [dynamic packaging](media-services-dynamic-packaging-overview.md).</span></span>

<span data-ttu-id="9a371-146">Hello následující kód Ukázka používá rozšíření Azure Media Services .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="9a371-146">hello following code sample uses Azure Media Services .NET SDK Extensions.</span></span>  <span data-ttu-id="9a371-147">Ujistěte se, že tooupdate hello kódu toopoint toohello složky kde jsou umístěné vaše vstupní soubory MP4 a soubor .ism.</span><span class="sxs-lookup"><span data-stu-id="9a371-147">Make sure tooupdate hello code toopoint toohello folder where your input MP4 files and .ism file are located.</span></span> <span data-ttu-id="9a371-148">A také toowhere MediaPackager_ValidateTask.xml je soubor uložený.</span><span class="sxs-lookup"><span data-stu-id="9a371-148">And also toowhere your MediaPackager_ValidateTask.xml file is located.</span></span> <span data-ttu-id="9a371-149">Tento soubor XML je definována v [přednastavení úloh pro Azure Media Balíčkovač](http://msdn.microsoft.com/library/azure/hh973635.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="9a371-149">This XML file is defined in [Task Preset for Azure Media Packager](http://msdn.microsoft.com/library/azure/hh973635.aspx) topic.</span></span>

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

            // hello MultibitrateMP4Files folder should also
            // contain hello .ism manifest file.
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
                // Create and cache hello Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Use hello cached credentials toocreate CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Ingest a set of multibitrate MP4s.
                //
                // Use hello SDK extension method toocreate a new asset by 
                // uploading files from a local directory.
                IAsset multibitrateMP4sAsset = _context.Assets.CreateFromFolder(
                    _multibitrateMP4s,
                    AssetCreationOptions.None,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });

                // Use Azure Media Packager toovalidate hello files.
                IAsset validatedMP4s =
                    ValidateMultibitrateMP4s(multibitrateMP4sAsset);

                // Publish hello asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    validatedMP4s,
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));

                                     // Get hello streaming URLs.
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
                IJob job = _context.Jobs.Create("MP4 validation and converstion tooSmooth Stream job.");

                // Read hello task configuration data into a string. 
                string configMp4Validation = File.ReadAllText(Path.Combine(
                        _configurationXMLFiles,
                        "MediaPackager_ValidateTask.xml"));

                // Get hello SDK extension method too get a reference toohello Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Create a task with hello conversion details, using hello configuration data. 
                ITask task = job.Tasks.AddNew("Mp4 Validation Task",
                    processor,
                    configMp4Validation,
                    TaskOptions.None);

                // Specify hello input asset toobe validated.
                task.InputAssets.Add(multibitrateMP4sAsset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is in hello clear (unencrypted). 
                task.OutputAssets.AddNew("Validated output asset",
                        AssetCreationOptions.None);

                // Submit hello job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // If hello validation task fails and job completes with JobState.Error,
                // display hello error message and throw an exception.
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
                    throw new Exception("hello specified multi-bitrate MP4 set is not valid.");
                }


                return job.OutputMediaAssets[0];
            }

            static void SetISMFileAsPrimary(IAsset asset)
            {
                var ismAssetFiles = asset.AssetFiles.ToList().
                    Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).ToArray();

                // hello following code assigns hello first .ism file as hello primary file in hello asset.
                // An asset should have one .ism file.  
                ismAssetFiles.First().IsPrimary = true;
                ismAssetFiles.First().Update();
            }
        }
    }

## <a name="using-static-encryption-tooprotect-your-smooth-and-mpeg-dash-with-playready"></a><span data-ttu-id="9a371-150">Pomocí statické šifrování tooProtect Smooth a MPEG DASH s technologií PlayReady</span><span class="sxs-lookup"><span data-stu-id="9a371-150">Using Static Encryption tooProtect your Smooth and MPEG DASH with PlayReady</span></span>
<span data-ttu-id="9a371-151">Pokud chcete svůj obsah pomocí PlayReady tooprotect, máte možnost volby použití [dynamického šifrování](media-services-protect-with-drm.md) (hello doporučená možnost) nebo statické šifrování (jak je popsáno v této části).</span><span class="sxs-lookup"><span data-stu-id="9a371-151">If you want tooprotect your content with PlayReady, you have a choice of using [dynamic encryption](media-services-protect-with-drm.md) (hello recommended option) or static encryption (as described in this section).</span></span>

<span data-ttu-id="9a371-152">Příklad Hello v této části kóduje soubor mezzanine (v tomto případě MP4) do souborů MP4.</span><span class="sxs-lookup"><span data-stu-id="9a371-152">hello example in this section encodes a mezzanine file (in this case MP4) into adaptive bitrate MP4 files.</span></span> <span data-ttu-id="9a371-153">Ji pak balíčky soubory MP4 s rychlostmi do technologie Smooth Streaming a potom šifruje technologie Smooth Streaming s technologií PlayReady.</span><span class="sxs-lookup"><span data-stu-id="9a371-153">It then packages MP4s into Smooth Streaming and then encrypts Smooth Streaming with PlayReady.</span></span> <span data-ttu-id="9a371-154">Výsledkem jsou možné toostream technologie Smooth Streaming nebo MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="9a371-154">As a result you are able toostream Smooth Streaming or MPEG DASH.</span></span>

<span data-ttu-id="9a371-155">Služba Media Services nyní poskytuje službu k doručování licencí PlayReady společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="9a371-155">Media Services now provides a service for delivering Microsoft PlayReady licenses.</span></span> <span data-ttu-id="9a371-156">Hello příklad v tomto článku ukazuje, jak tooconfigure hello Media Services PlayReady licence služby doručení (viz metoda ConfigureLicenseDeliveryService hello definované v hello kódu níže).</span><span class="sxs-lookup"><span data-stu-id="9a371-156">hello example in this article shows how tooconfigure hello Media Services PlayReady license delivery service (see hello ConfigureLicenseDeliveryService method defined in hello code below).</span></span> <span data-ttu-id="9a371-157">Další informace o službě doručování licencí Media Services PlayReady najdete v tématu [pomocí dynamického šifrování PlayReady a službu doručování licencí](media-services-protect-with-drm.md).</span><span class="sxs-lookup"><span data-stu-id="9a371-157">For more information about Media Services PlayReady license delivery service, see [Using PlayReady Dynamic Encryption and License Delivery Service](media-services-protect-with-drm.md).</span></span>

> [!NOTE]
> <span data-ttu-id="9a371-158">toodeliver MPEG DASH šifrované pomocí technologie PlayReady, ujistěte se, že toouse CENC možnosti nastavením vlastností useSencBox a adjustSubSamples hello (popsané v hello [přednastavení úloh pro Azure Media modulu pro šifrování](http://msdn.microsoft.com/library/azure/hh973610.aspx) tématu) tootrue.</span><span class="sxs-lookup"><span data-stu-id="9a371-158">toodeliver MPEG DASH encrypted with PlayReady, make sure toouse CENC options by setting hello useSencBox and adjustSubSamples properties (described in hello [Task Preset for Azure Media Encryptor](http://msdn.microsoft.com/library/azure/hh973610.aspx) topic) tootrue.</span></span>  
> 
> 

<span data-ttu-id="9a371-159">Ujistěte se, zda text hello tooupdate následující kód toopoint toohello složku, kde je umístěn vaše vstupní soubor MP4.</span><span class="sxs-lookup"><span data-stu-id="9a371-159">Make sure tooupdate hello following code toopoint toohello folder where your input MP4 file is located.</span></span>

<span data-ttu-id="9a371-160">A také toowhere MediaPackager_MP4ToSmooth.xml a MediaEncryptor_PlayReadyProtection.xml soubory jsou umístěny.</span><span class="sxs-lookup"><span data-stu-id="9a371-160">And also toowhere your MediaPackager_MP4ToSmooth.xml and MediaEncryptor_PlayReadyProtection.xml files are located.</span></span> <span data-ttu-id="9a371-161">MediaPackager_MP4ToSmooth.xml je definována v [přednastavení úloh pro Azure Media Balíčkovač](http://msdn.microsoft.com/library/azure/hh973635.aspx) a MediaEncryptor_PlayReadyProtection.xml je definována v hello [přednastavení úloh pro Azure Media modulu pro šifrování](http://msdn.microsoft.com/library/azure/hh973610.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="9a371-161">MediaPackager_MP4ToSmooth.xml is defined in [Task Preset for Azure Media Packager](http://msdn.microsoft.com/library/azure/hh973635.aspx) and MediaEncryptor_PlayReadyProtection.xml is defined in hello [Task Preset for Azure Media Encryptor](http://msdn.microsoft.com/library/azure/hh973610.aspx) topic.</span></span> 

<span data-ttu-id="9a371-162">Příklad Hello definuje hello UpdatePlayReadyConfigurationXMLFile metodu, můžete použít soubor MediaEncryptor_PlayReadyProtection.xml hello toodynamically aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9a371-162">hello example defines hello UpdatePlayReadyConfigurationXMLFile method that you can use toodynamically update hello MediaEncryptor_PlayReadyProtection.xml file.</span></span> <span data-ttu-id="9a371-163">Pokud máte hello klíče počáteční hodnoty k dispozici, můžete hello CommonEncryption.GeneratePlayReadyContentKey metoda toogenerate hello klíč obsahu na základě hello keySeedValue a KeyId hodnot.</span><span class="sxs-lookup"><span data-stu-id="9a371-163">If you have hello key seed available, you can use hello CommonEncryption.GeneratePlayReadyContentKey method toogenerate hello content key based on hello keySeedValue and KeyId values.</span></span>

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
                // Create and cache hello Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Use hello cached credentials toocreate CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Encoding and encrypting assets //////////////////////
                // Load a single MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);

                // Encode an MP4 file tooa set of multibitrate MP4s.
                // Then, package a set of MP4s tooclear Smooth Streaming.
                IAsset clearSmoothStreamAsset =
                    ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);

                // Create a common encryption content key that is used 
                // a) tooset hello key values in hello MediaEncryptor_PlayReadyProtection.xml file
                //    that is used for encryption.
                // b) tooconfigure hello license delivery service and 
                //
                Guid keyId;
                byte[] contentKey;

                IContentKey key = CreateCommonEncryptionKey(out keyId, out contentKey);

                // hello content key authorization policy must be configured by you 
                // and met by hello client in order for hello PlayReady license
                // toobe delivered toohello client. 
                // In this example hello Media Services PlayReady license delivery service is used.
                ConfigureLicenseDeliveryService(key);

                // Get hello Media Services PlayReady license delivery URL.
                // This URL will be assigned toohello licenseAcquisitionUrl property 
                // of hello MediaEncryptor_PlayReadyProtection.xml file.
                Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

                // Update hello MediaEncryptor_PlayReadyProtection.xml file with hello key and URL info.
                UpdatePlayReadyConfigurationXMLFile(keyId, contentKey, acquisitionUrl);


                // Encrypt your clear Smooth Streaming tooSmooth Streaming with PlayReady.
                IAsset outputAsset = CreateSmoothStreamEncryptedWithPlayReady(clearSmoothStreamAsset);


                // You can use hello http://smf.cloudapp.net/healthmonitor player 
                // tootest hello smoothStreamURL URL.
                string smoothStreamURL = outputAsset.GetSmoothStreamingUri().ToString();
                Console.WriteLine("Smooth Streaming URL:");
                Console.WriteLine(smoothStreamURL);

                // You can use hello http://dashif.org/reference/players/javascript/ player 
                // tootest hello dashURL URL.
                string dashURL = outputAsset.GetMpegDashUri().ToString();
                Console.WriteLine("MPEG DASH URL:");
                Console.WriteLine(dashURL);
            }

            /// <summary>
            /// Creates a job with 2 tasks: 
            /// 1 task - encodes a single MP4 toomultibitrate MP4s,
            /// 2 task - packages MP4s tooSmooth Streaming.
            /// </summary>
            /// <returns>hello output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 tooSmooth Streaming.");

                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set tooClear Smooth Stream.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);

                // Submit hello job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // Get hello output asset that contains hello Smooth Streaming asset.
                return job.OutputMediaAssets[1];
            }

            /// <summary>
            /// Encrypts Smooth Stream with PlayReady.
            /// Then creates a Smooth Streaming Url.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>hello output asset.</returns>
            public static IAsset CreateSmoothStreamEncryptedWithPlayReady(IAsset clearSmoothStreamAsset)
            {
                // Create a job.
                IJob job = _context.Jobs.Create("Encrypt tooPlayReady Smooth Streaming.");

                // Add task 1 - Encrypt Smooth Streaming with PlayReady 
                IAsset encryptedSmoothAsset =
                    EncryptSmoothStreamWithPlayReadyTask(job, clearSmoothStreamAsset);

                // Submit hello job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // hello OutputMediaAssets[0] contains hello desired asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    job.OutputMediaAssets[0],
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));

                return job.OutputMediaAssets[0];
            }

            /// <summary>
            /// Create a common encryption content key that is used 
            /// tooset hello key values in hello MediaEncryptor_PlayReadyProtection.xml file
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

                // Prepare hello encryption task template
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

                // Update hello "value" property.
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
            /// <param name="fileDir">hello location of hello files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify hello following encryption options for hello AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded tooAzure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: hello files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use hello SDK extension method toocreate a new asset by 
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
            /// Creates a task tooencode tooAdaptive Bitrate. 
            /// Adds hello new task tooa job.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset EncodeMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);

                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 tooAdaptive Bitrate Task",
                   encoder,
                   "Adaptive Streaming",
                   TaskOptions.None);

                // Specify hello input Asset
                adpativeBitrateTask.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is in hello clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s",
                                        AssetCreationOptions.None);

                return abrAsset;
            }

            /// <summary>
            /// Creates a task tooconvert hello MP4 file(s) tooa Smooth Streaming asset.
            /// Adds hello new task tooa job.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles,
                            "MediaPackager_MP4toSmooth.xml"));

                // Create a new Task tooconvert adaptive bitrate tooSmooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 tooSmooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);

                // Specify hello input Asset, which is hello output Asset from hello first task
                smoothStreamingTask.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is in hello clear (unencrypted).
                IAsset smoothOutputAsset =
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Stream",
                        AssetCreationOptions.None);

                return smoothOutputAsset;
            }


            /// <summary>
            /// Creates a task tooencrypt Smooth Streaming with PlayReady.
            /// Note: toodeliver DASH, make sure tooset hello useSencBox and adjustSubSamples 
            /// configuration properties tootrue. 
            /// In this example, MediaEncryptor_PlayReadyProtection.xml contains configuration.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset EncryptSmoothStreamWithPlayReadyTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Azure Media Encryptor.
                IMediaProcessor playreadyProcessor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaEncryptor);

                // Read hello configuration XML.
                //
                // Note that hello configuration defined in MediaEncryptor_PlayReadyProtection.xml
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

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.CommonEncryptionProtected.
                IAsset playreadyAsset = playreadyTask.OutputAssets.AddNew(
                                                "PlayReady Smooth Streaming",
                                                AssetCreationOptions.CommonEncryptionProtected);

                return playreadyAsset;
            }

            /// <summary>
            /// Configures authorization policy for hello content key. 
            /// </summary>
            /// <param name="contentKey">hello content key.</param>
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

                // Associate hello content key authorization policy with hello content key.
                contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                contentKey = contentKey.UpdateAsync().Result;
            }

            static private string ConfigurePlayReadyLicenseTemplate()
            {
                // hello following code configures PlayReady License Template using .NET classes
                // and returns hello XML string.

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

## <a name="using-static-encryption-tooprotect-hlsv3-with-aes-128"></a><span data-ttu-id="9a371-164">Použití statické šifrování tooProtect HLSv3 s AES-128</span><span class="sxs-lookup"><span data-stu-id="9a371-164">Using Static Encryption tooProtect HLSv3 with AES-128</span></span>
<span data-ttu-id="9a371-165">Pokud chcete tooencrypt vaše HLS s AES-128, máte možnost volby použití dynamického šifrování (hello doporučená možnost) nebo statické šifrování (jak je uvedené v této části).</span><span class="sxs-lookup"><span data-stu-id="9a371-165">If you want tooencrypt your HLS with AES-128, you have a choice of using dynamic encryption (hello recommended option) or static encryption (as shown in this section).</span></span> <span data-ttu-id="9a371-166">Pokud se rozhodnete toouse dynamické šifrování, najdete v části [pomocí dynamického šifrování AES-128 a služba pro přenos klíče](media-services-protect-with-aes128.md).</span><span class="sxs-lookup"><span data-stu-id="9a371-166">If you decide toouse dynamic encryption, see [Using AES-128 Dynamic Encryption and Key Delivery Service](media-services-protect-with-aes128.md).</span></span>

> [!NOTE]
> <span data-ttu-id="9a371-167">V pořadí tooconvert svůj obsah do HLS, je nutné nejprve převést nebo zakódovat svůj obsah do technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="9a371-167">In order tooconvert your content into HLS, you must first convert/encode your content into Smooth Streaming.</span></span>
> <span data-ttu-id="9a371-168">Navíc pro hello HLS tooget šifrováním pomocí standardu AES Ujistěte se, že tooset hello následující vlastnosti v souboru MediaPackager_SmoothToHLS.xml: Sada hello šifrování tootrue vlastnost, nastavte hodnotu klíče hello a hello keyuri hodnotu toopoint tooyour authentication\ autorizace serveru.</span><span class="sxs-lookup"><span data-stu-id="9a371-168">Also, for hello HLS tooget encrypted with AES make sure tooset hello following properties in your MediaPackager_SmoothToHLS.xml file: set hello encrypt property tootrue, set hello key value, and hello keyuri value toopoint tooyour authentication\authorization server.</span></span>
> <span data-ttu-id="9a371-169">Media Services vytvoříte soubor klíče a jeho následné uložení do kontejneru asset hello.</span><span class="sxs-lookup"><span data-stu-id="9a371-169">Media Services will create a key file and place it in hello asset container.</span></span> <span data-ttu-id="9a371-170">Měli zkopírovat hello /asset-containerguid/*.key souboru tooyour serveru (nebo vytvořit svůj vlastní soubor klíče) a potom odstraňte soubor *.key hello z kontejneru asset hello.</span><span class="sxs-lookup"><span data-stu-id="9a371-170">You should copy hello /asset-containerguid/*.key file tooyour server (or create your own key file) and then delete hello *.key file from hello asset container.</span></span>
> 
> 

<span data-ttu-id="9a371-171">Příklad Hello v této části kóduje soubor mezzanine (v tomto případě MP4) do multibitrate soubory MP4 a pak balíčky soubory MP4 s rychlostmi do technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="9a371-171">hello example in this section encodes a mezzanine file (in this case MP4) into multibitrate MP4 files and then packages MP4s into Smooth Streaming.</span></span> <span data-ttu-id="9a371-172">Ji pak balíčky technologie Smooth Streaming do HTTP Live Streaming (HLS) šifrován Advanced Encryption (Standard AES) datového proudu 128bitové šifrování.</span><span class="sxs-lookup"><span data-stu-id="9a371-172">It then packages Smooth Streaming into HTTP Live Streaming (HLS) encrypted with Advanced Encryption Standard (AES) 128-bit stream encryption.</span></span> <span data-ttu-id="9a371-173">Ujistěte se, zda text hello tooupdate následující kód toopoint toohello složku, kde je umístěn vaše vstupní soubor MP4.</span><span class="sxs-lookup"><span data-stu-id="9a371-173">Make sure tooupdate hello following code toopoint toohello folder where your input MP4 file is located.</span></span> <span data-ttu-id="9a371-174">A také toowhere MediaPackager_MP4ToSmooth.xml a MediaPackager_SmoothToHLS.xml konfigurační soubory jsou umístěny.</span><span class="sxs-lookup"><span data-stu-id="9a371-174">And also toowhere your MediaPackager_MP4ToSmooth.xml and MediaPackager_SmoothToHLS.xml configuration files are located.</span></span> <span data-ttu-id="9a371-175">Hello definice pro tyto soubory můžete najít v hello [přednastavení úloh pro Azure Media Balíčkovač](http://msdn.microsoft.com/library/azure/hh973635.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="9a371-175">You can find hello definition for these files in hello [Task Preset for Azure Media Packager](http://msdn.microsoft.com/library/azure/hh973635.aspx) topic.</span></span>

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
            // Paths toosupport files (within hello above base path). You can use 
            // hello provided sample media files from hello "SupportFiles" folder, or 
            // provide paths tooyour own media files below toorun these samples.

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
                // Create and cache hello Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName, 
                                _mediaServicesAccountKey);
                // Use hello cached credentials toocreate CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Encoding and encrypting assets //////////////////////

                // Load an MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);

                // Encode an MP4 file tooa set of multibitrate MP4s.
                // Then, package a set of MP4s tooclear Smooth Streaming.
                IAsset clearSmoothStreamAsset = ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);

                // Create HLS encrypted with AES.
                IAsset HLSEncryptedWithAESAsset = CreateHLSEncryptedWithAES(clearSmoothStreamAsset);

                // You can use hello following player tootest hello HLS with AES stream.
                // http://apps.microsoft.com/windows/app/3ivx-hls-player/f79ce7d0-2993-4658-bc4e-83dc182a0614 
                string hlsWithAESURL = HLSEncryptedWithAESAsset.GetHlsUri().ToString();
                Console.WriteLine("HLS with AES URL:");
                Console.WriteLine(hlsWithAESURL);
            }


            /// <summary>
            /// Creates a job with 2 tasks: 
            /// 1 task - encodes a single MP4 toomultibitrate MP4s,
            /// 2 task - packages MP4s tooSmooth Streaming.
            /// </summary>
            /// <returns>hello output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 tooSmooth Streaming.");

                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeSingleMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set tooClear Smooth Streaming.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);

                // Submit hello job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // Get hello output asset that contains Smooth Streaming.
                return job.OutputMediaAssets[1];
            }

            /// <summary>
            /// Encrypts an HLS with AES-128.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>hello output asset.</returns>
            public static IAsset CreateHLSEncryptedWithAES(IAsset clearSmoothStreamAsset)
            {
                IJob job = _context.Jobs.Create("Encrypt tooHLS with AES.");

                // Add task 1 - Package clear Smooth Streaming tooHLS with AES.
                PackageSmoothStreamToHLS(job, clearSmoothStreamAsset);

                // Submit hello job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // hello OutputMediaAssets[0] contains hello desired asset.
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
            /// <param name="fileDir">hello location of hello files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify hello following encryption options for hello AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded tooAzure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: hello files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use hello SDK extension method toocreate a new asset by 
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
            /// Creates a task tooencode tooAdaptive Bitrate. 
            /// Adds hello new task tooa job.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset EncodeSingleMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);

                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 tooAdaptive Bitrate Task",
                   encoder,
                   "Adaptive Streaming",
                   TaskOptions.None);

                // Specify hello input Asset
                adpativeBitrateTask.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is in hello clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s", 
                                        AssetCreationOptions.None);

                return abrAsset;
            }

            /// <summary>
            /// Creates a task tooconvert hello MP4 file(s) tooa Smooth Streaming asset.
            /// Adds hello new task tooa job.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles, 
                            "MediaPackager_MP4toSmooth.xml"));

                // Create a new Task tooconvert adaptive bitrate tooSmooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 tooSmooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);

                // Specify hello input Asset, which is hello output Asset from hello first task
                smoothStreamingTask.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is in hello clear (unencrypted).
                IAsset smoothOutputAsset = 
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Streaming", 
                        AssetCreationOptions.None);

                return smoothOutputAsset;
            }

            /// <summary>
            /// Converts Smooth Streaming tooHLS.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello Smooth Streaming asset.</param>
            /// <returns>hello asset that was packaged tooHLS.</returns>
            private static IAsset PackageSmoothStreamToHLS(IJob job, IAsset smoothStreamAsset)
            {
                // Get hello SDK extension method too get a reference toohello Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Read hello configuration data into a string. 
                // For hello HLS tooget encrypted with AES make sure tooset the
                // encrypt configuration property tootrue.
                //
                // In production, it is recommended toodo hello following:
                //    Set a Key url for your authn/authz server.
                //    Copy hello /asset-containerguid/*.key file tooyour server (or craft a key file for yourself).
                //    Delete *.key from hello asset container.
                //
                string configuration = File.ReadAllText(Path.Combine(_configurationXMLFiles, @"MediaPackager_SmoothToHLS.xml"));

                // Create a task with hello encoding details, using a configuration file.
                ITask task = job.Tasks.AddNew("My Smooth Streaming tooHLS Task",
                   processor,
                   configuration,
                   TaskOptions.ProtectedConfiguration);

                // Specify hello input asset toobe encoded.
                task.InputAssets.Add(smoothStreamAsset);

                // Add an output asset toocontain hello results of hello job. 
                IAsset outputAsset = 
                    task.OutputAssets.AddNew("HLS asset", AssetCreationOptions.None);


                return outputAsset;
            }
        }
    }

## <a name="using-static-encryption-tooprotect-hlsv3-with-playready"></a><span data-ttu-id="9a371-176">Použití statické šifrování tooProtect HLSv3 s technologií PlayReady</span><span class="sxs-lookup"><span data-stu-id="9a371-176">Using Static Encryption tooProtect HLSv3 with PlayReady</span></span>
<span data-ttu-id="9a371-177">Pokud chcete svůj obsah pomocí PlayReady tooprotect, máte možnost volby použití [dynamického šifrování](media-services-protect-with-drm.md) (hello doporučená možnost) nebo statické šifrování (jak je popsáno v této části).</span><span class="sxs-lookup"><span data-stu-id="9a371-177">If you want tooprotect your content with PlayReady, you have a choice of using [dynamic encryption](media-services-protect-with-drm.md) (hello recommended option) or static encryption (as described in this section).</span></span>

> [!NOTE]
> <span data-ttu-id="9a371-178">V pořadí tooprotect svůj obsah pomocí PlayReady můžete musí nejdřív převést nebo zakódovat svůj obsah do formátu, technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="9a371-178">In order tooprotect your content using PlayReady you must first convert/encode your content into a Smooth Streaming format.</span></span>
> 
> 

<span data-ttu-id="9a371-179">Příklad Hello v této části kóduje soubor mezzanine (v tomto případě MP4) do souborů multibitrate MP4.</span><span class="sxs-lookup"><span data-stu-id="9a371-179">hello example in this section encodes a mezzanine file (in this case MP4) into multibitrate MP4 files.</span></span> <span data-ttu-id="9a371-180">Potom balíčky soubory MP4 s rychlostmi do technologie Smooth Streaming a šifruje technologie Smooth Streaming s technologií PlayReady.</span><span class="sxs-lookup"><span data-stu-id="9a371-180">It then packages MP4s into Smooth Streaming and encrypts Smooth Streaming with PlayReady.</span></span> <span data-ttu-id="9a371-181">tooproduce HTTP Live Streaming (HLS) šifrovat pomocí PlayReady, technologie Smooth Streaming PlayReady asset hello musí toobe zabalené do HLS.</span><span class="sxs-lookup"><span data-stu-id="9a371-181">tooproduce HTTP Live Streaming (HLS) encrypted with PlayReady, hello PlayReady Smooth Streaming asset needs toobe packaged into HLS.</span></span> <span data-ttu-id="9a371-182">Toto téma ukazuje, jak tooperform všechny tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="9a371-182">This topic demonstrates how tooperform all these steps.</span></span>

<span data-ttu-id="9a371-183">Služba Media Services nyní poskytuje službu k doručování licencí PlayReady společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="9a371-183">Media Services now provides a service for delivering Microsoft PlayReady licenses.</span></span> <span data-ttu-id="9a371-184">Hello příklad v tomto článku ukazuje, jak tooconfigure hello Media Services PlayReady licence služby doručení (viz hello **ConfigureLicenseDeliveryService** metoda definované v hello kódu níže).</span><span class="sxs-lookup"><span data-stu-id="9a371-184">hello example in this article shows how tooconfigure hello Media Services PlayReady license delivery service (see hello **ConfigureLicenseDeliveryService** method defined in hello code below).</span></span> 

<span data-ttu-id="9a371-185">Ujistěte se, zda text hello tooupdate následující kód toopoint toohello složku, kde je umístěn vaše vstupní soubor MP4.</span><span class="sxs-lookup"><span data-stu-id="9a371-185">Make sure tooupdate hello following code toopoint toohello folder where your input MP4 file is located.</span></span> <span data-ttu-id="9a371-186">A také toowhere MediaPackager_MP4ToSmooth.xml, MediaPackager_SmoothToHLS.xml a MediaEncryptor_PlayReadyProtection.xml soubory jsou umístěny.</span><span class="sxs-lookup"><span data-stu-id="9a371-186">And also toowhere your MediaPackager_MP4ToSmooth.xml, MediaPackager_SmoothToHLS.xml, and MediaEncryptor_PlayReadyProtection.xml files are located.</span></span> <span data-ttu-id="9a371-187">MediaPackager_MP4ToSmooth.xml a MediaPackager_SmoothToHLS.xml jsou definovány v [přednastavení úloh pro Azure Media Balíčkovač](http://msdn.microsoft.com/library/azure/hh973635.aspx) a MediaEncryptor_PlayReadyProtection.xml je definována v hello [přednastavení úloh pro Azure. Modul pro šifrování média](http://msdn.microsoft.com/library/azure/hh973610.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="9a371-187">MediaPackager_MP4ToSmooth.xml and MediaPackager_SmoothToHLS.xml are defined in [Task Preset for Azure Media Packager](http://msdn.microsoft.com/library/azure/hh973635.aspx) and MediaEncryptor_PlayReadyProtection.xml is defined in hello [Task Preset for Azure Media Encryptor](http://msdn.microsoft.com/library/azure/hh973610.aspx) topic.</span></span>

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
            // Paths toosupport files (within hello above base path). You can use 
            // hello provided sample media files from hello "SupportFiles" folder, or 
            // provide paths tooyour own media files below toorun these samples.

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
                // Create and cache hello Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used hello chached credentials toocreate CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Load an MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);

                // Encode an MP4 file tooa set of multibitrate MP4s.
                // Then, package a set of MP4s tooclear Smooth Streaming.
                IAsset clearSmoothStreamAsset = ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);

                // Create a common encryption content key that is used 
                // a) tooset hello key values in hello MediaEncryptor_PlayReadyProtection.xml file
                //    that is used for encryption.
                // b) tooconfigure hello license delivery service and 
                //
                Guid keyId;
                byte[] contentKey;

                IContentKey key = CreateCommonEncryptionKey(out keyId, out contentKey);

                // hello content key authorization policy must be configured by you 
                // and met by hello client in order for hello PlayReady license
                // toobe delivered toohello client. 
                // In this example hello Media Services PlayReady license delivery service is used.
                ConfigureLicenseDeliveryService(key);

                // Get hello Media Services PlayReady license delivery URL.
                // This URL will be assigned toohello licenseAcquisitionUrl property 
                // of hello MediaEncryptor_PlayReadyProtection.xml file.
                Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

                // Update hello MediaEncryptor_PlayReadyProtection.xml file with hello key and URL info.
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
            /// 1 task - encodes a single MP4 toomultibitrate MP4s,
            /// 2 task - packages MP4s tooSmooth Streaming.
            /// </summary>
            /// <returns>hello output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 tooSmooth Streaming.");

                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeSingleMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set tooClear Smooth Streaming.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);

                // Submit hello job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // Get hello output asset that contains Smooth Streaming.
                return job.OutputMediaAssets[1];
            }

            /// <summary>
            /// Create a common encryption content key that is used 
            /// tooset hello key values in hello MediaEncryptor_PlayReadyProtection.xml file
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

                // Prepare hello encryption task template
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

                // Update hello "value" property.
                if (licenseAcquisitionUrlEl != null)
                    licenseAcquisitionUrlEl.Attribute("value").SetValue(licenseAcquisitionUrl.ToString());

                if (contentKeyEl != null)
                    contentKeyEl.Attribute("value").SetValue(Convert.ToBase64String(keyValue));

                if (keyIdEl != null)
                    keyIdEl.Attribute("value").SetValue(keyId);

                doc.Save(xmlFileName);
            }

            /// <summary>
            // Encrypts clear Smooth Streaming tooSmooth Streaming with PlayReady.
            // Then, packages hello PlayReady Smooth Streaming tooHLS with PlayReady.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>hello output asset.</returns>
            public static IAsset CreateHLSEncryptedWithPlayReady(IAsset clearSmoothStreamAsset)
            {
                IJob job = _context.Jobs.Create("Encrypt tooHLS with PlayReady.");

                // Add task 1 - Encrypt Smooth Streaming with PlayReady 
                IAsset encryptedSmoothAsset =
                    EncryptSmoothStreamWithPlayReadyTask(job, clearSmoothStreamAsset);

                // Add task 2 - Package tooHLS with PlayReady.
                PackageSmoothStreamToHLS(job, encryptedSmoothAsset);

                // Submit hello job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // Since we had two tasks, hello OutputMediaAssets[1]
                // contains hello desired asset.
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
            /// <param name="fileDir">hello location of hello files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify hello following encryption options for hello AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded tooAzure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: hello files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use hello SDK extension method toocreate a new asset by 
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
            /// Creates a task tooencode tooAdaptive Bitrate. 
            /// Adds hello new task tooa job.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset EncodeSingleMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);

                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 tooAdaptive Bitrate Task",
                   encoder,
                   "Adaptive Streaming",
                   TaskOptions.None);

                // Specify hello input Asset
                adpativeBitrateTask.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is in hello clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s",
                                        AssetCreationOptions.None);

                return abrAsset;
            }

            /// <summary>
            /// Creates a task tooconvert hello MP4 file(s) tooa Smooth Streaming asset.
            /// Adds hello new task tooa job.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles,
                            "MediaPackager_MP4toSmooth.xml"));

                // Create a new Task tooconvert adaptive bitrate tooSmooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 tooSmooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);

                // Specify hello input Asset, which is hello output Asset from hello first task
                smoothStreamingTask.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is in hello clear (unencrypted).
                IAsset smoothOutputAsset =
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Streaming",
                        AssetCreationOptions.None);

                return smoothOutputAsset;
            }


            /// <summary>
            /// Converts Smooth Stream tooHLS.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello Smooth Stream asset.</param>
            /// <returns>hello asset that was packaged tooHLS.</returns>
            private static IAsset PackageSmoothStreamToHLS(IJob job, IAsset smoothStreamAsset)
            {
                // Get hello SDK extension method too get a reference toohello Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Read hello configuration data into a string. 
                //
                string configuration = File.ReadAllText(
                            Path.Combine(_configurationXMLFiles,
                                        @"MediaPackager_SmoothToHLS.xml"));

                // Create a task with hello encoding details, using a configuration file.
                ITask task = job.Tasks.AddNew("My Smooth tooHLS Task",
                   processor,
                   configuration,
                   TaskOptions.ProtectedConfiguration);

                // Specify hello input asset toobe encoded.
                task.InputAssets.Add(smoothStreamAsset);

                // Add an output asset toocontain hello results of hello job. 
                IAsset outputAsset =
                    task.OutputAssets.AddNew("HLS asset", AssetCreationOptions.None);


                return outputAsset;
            }

            /// <summary>
            /// Creates a task tooencrypt Smooth Streaming with PlayReady.
            /// Note: Do deliver DASH, make sure tooset hello useSencBox and adjustSubSamples 
            /// configuration properties tootrue.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset EncryptSmoothStreamWithPlayReadyTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Azure Media Encryptor.
                IMediaProcessor playreadyProcessor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaEncryptor);

                // Read hello configuration XML.
                //
                // Note that hello configuration defined in MediaEncryptor_PlayReadyProtection.xml
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

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.CommonEncryptionProtected.
                IAsset playreadyAsset = playreadyTask.OutputAssets.AddNew(
                                                "PlayReady Smooth Streaming",
                                                AssetCreationOptions.CommonEncryptionProtected);


                return playreadyAsset;
            }


            /// <summary>
            /// Configures authorization policy for hello content key. 
            /// </summary>
            /// <param name="contentKey">hello content key.</param>
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

                // Associate hello content key authorization policy with hello content key.
                contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                contentKey = contentKey.UpdateAsync().Result;
            }

            static private string ConfigurePlayReadyLicenseTemplate()
            {
                // hello following code configures PlayReady License Template using .NET classes
                // and returns hello XML string.

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="9a371-188">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="9a371-188">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="9a371-189">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="9a371-189">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

