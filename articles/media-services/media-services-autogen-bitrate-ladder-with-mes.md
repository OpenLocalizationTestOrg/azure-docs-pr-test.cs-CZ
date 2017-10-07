---
title: "Azure Media Encoder Standard tooauto aaaUse-generovat přenosovou rychlostí žebříku | Microsoft Docs"
description: "Toto téma ukazuje, jak toouse Media Encoder Standard (MES) tooauto-generovat žebříku přenosovou rychlostí podle hello vstupní řešení a přenosovou rychlostí. Vstupní řešení Hello a přenosovou rychlostí se nikdy překročena. Například pokud hello vstup je 720p na 3 MB/s, bude výstup v nejlépe zůstanou 720p a začne tempem nižší než 3 MB/s."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 63ed95da-1b82-44b0-b8ff-eebd535bc5c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 5437f54ac28c42ddd4f9d1986549d6da6261c5da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#  <a name="use-azure-media-encoder-standard-tooauto-generate-a-bitrate-ladder"></a><span data-ttu-id="89f52-105">Pomocí Azure Media Encoder Standard tooauto-generovat žebříku přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="89f52-105">Use Azure Media Encoder Standard tooauto-generate a bitrate ladder</span></span>

## <a name="overview"></a><span data-ttu-id="89f52-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="89f52-106">Overview</span></span>

<span data-ttu-id="89f52-107">Toto téma ukazuje, jak toouse Media Encoder Standard (MES) tooauto-generovat žebříku přenosovou rychlostí (přenosovou rychlostí řešení páry) na základě hello vstupní řešení a přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="89f52-107">This topic shows how toouse Media Encoder Standard (MES) tooauto-generate a bitrate ladder (bitrate-resolution pairs) based on hello input resolution and bitrate.</span></span> <span data-ttu-id="89f52-108">Hello automaticky generovaný přednastavených se nikdy překročit hello vstupní řešení a přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="89f52-108">hello auto-generated preset will never exceed hello input resolution and bitrate.</span></span> <span data-ttu-id="89f52-109">Například pokud hello vstup je 720p na 3 MB/s, bude výstup v nejlépe zůstanou 720p a začne tempem nižší než 3 MB/s.</span><span class="sxs-lookup"><span data-stu-id="89f52-109">For example, if hello input is 720p at 3Mbps, output will remain 720p at best, and will start at rates lower than 3Mbps.</span></span>

### <a name="encoding-for-streaming-only"></a><span data-ttu-id="89f52-110">Kódování pro streamování pouze</span><span class="sxs-lookup"><span data-stu-id="89f52-110">Encoding for Streaming Only</span></span>

<span data-ttu-id="89f52-111">Pokud vaše záměrem tooencode svůj zdroj videa pouze pro streamování, volných prostředků můžete použít "Adaptivní datové proudy" hello přednastavení při vytvoření úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="89f52-111">If your intent is tooencode your source video only for streaming, then you shoud use hello "Adaptive Streaming" preset when creating an encoding task.</span></span> <span data-ttu-id="89f52-112">Při použití hello **adaptivní datové proudy** předvolby, hello MES kodér bude inteligentně cap žebříku přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="89f52-112">When using hello **Adaptive Streaming** preset, hello MES encoder will intelligently cap a bitrate ladder.</span></span> <span data-ttu-id="89f52-113">Ale nebudete moct toocontrol hello kódování náklady, protože hello služby určuje, kolik toouse vrstev a v jakém rozlišení.</span><span class="sxs-lookup"><span data-stu-id="89f52-113">However, you will not be able toocontrol hello encoding costs, since hello service determines how many layers toouse and at what resolution.</span></span> <span data-ttu-id="89f52-114">Vidíte příklady vytvoření MES v důsledku kódování s hello vrstev výstup **adaptivní datové proudy** přednastavení na konci hello v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="89f52-114">You can see examples of output layers produced by MES as a result of encoding with hello **Adaptive Streaming** preset at hello end of this topic.</span></span> <span data-ttu-id="89f52-115">Hello výstupní Asset bude obsahovat soubory MP4, kde audio a video nejsou prokládaný.</span><span class="sxs-lookup"><span data-stu-id="89f52-115">hello output Asset will contain MP4 files where audio and video are not interleaved.</span></span>

### <a name="encoding-for-streaming-and-progressive-download"></a><span data-ttu-id="89f52-116">Kódování pro streamování a progresivní stahování</span><span class="sxs-lookup"><span data-stu-id="89f52-116">Encoding for Streaming and Progressive Download</span></span>

<span data-ttu-id="89f52-117">Pokud vaše záměrem tooencode zdroj videa pro streamování a také soubory MP4 tooproduce progresivní stahování, pak volných prostředků můžete používat hello "Obsahu adaptivní více přenosovou rychlostí MP4" přednastavení při vytvoření úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="89f52-117">If your intent is tooencode your source video for streaming as well as tooproduce MP4 files for progressive download, then you shoud use hello "Content Adaptive Multiple Bitrate MP4" preset when creating an encoding task.</span></span> <span data-ttu-id="89f52-118">Při použití hello **obsahu adaptivní více MP4 přenosovou rychlostí** předvolby, hello MES kodér použije hello stejné kódování logiku, jak je uvedeno výše, ale teď hello výstupní asset bude obsahovat MP4 soubory tam, kde audio a video jsou prokládaná.</span><span class="sxs-lookup"><span data-stu-id="89f52-118">When using hello **Content Adaptive Multiple Bitrate MP4** preset, hello MES encoder will apply hello same encoding logic as above, but now hello output asset will contain MP4 files where audio and video are interleaved.</span></span> <span data-ttu-id="89f52-119">Jako soubor progresivní stahování můžete použít jednu z těchto souborů MP4 (například hello nejvyšší přenosovou rychlostí verze).</span><span class="sxs-lookup"><span data-stu-id="89f52-119">You can use one of these MP4 files (for example, hello highest bitrate version) as a progressive download file.</span></span>

## <span data-ttu-id="89f52-120"><a id="encoding_with_dotnet"></a>Kódování pomocí služby Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="89f52-120"><a id="encoding_with_dotnet"></a>Encoding with Media Services .NET SDK</span></span>

<span data-ttu-id="89f52-121">Následující ukázka kódu Hello používá sadu Media Services .NET SDK tooperform hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="89f52-121">hello following code example uses Media Services .NET SDK tooperform hello following tasks:</span></span>

- <span data-ttu-id="89f52-122">Vytvořte úlohu kódování.</span><span class="sxs-lookup"><span data-stu-id="89f52-122">Create an encoding job.</span></span>
- <span data-ttu-id="89f52-123">Získání kodéru Media Encoder Standard toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="89f52-123">Get a reference toohello Media Encoder Standard encoder.</span></span>
- <span data-ttu-id="89f52-124">Přidat úlohu kódování toohello úloh a zadejte toouse hello **adaptivní datové proudy** přednastavené.</span><span class="sxs-lookup"><span data-stu-id="89f52-124">Add an encoding task toohello job and specify toouse hello **Adaptive Streaming** preset.</span></span> 
- <span data-ttu-id="89f52-125">Vytvoření výstupní asset, který bude obsahovat kódovaný hello asset.</span><span class="sxs-lookup"><span data-stu-id="89f52-125">Create an output asset that will contain hello encoded asset.</span></span>
- <span data-ttu-id="89f52-126">Přidejte průběh úlohy toocheck hello událost obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="89f52-126">Add an event handler toocheck hello job progress.</span></span>
- <span data-ttu-id="89f52-127">Odešlete úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="89f52-127">Submit hello job.</span></span>

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="89f52-128">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="89f52-128">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="89f52-129">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="89f52-129">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="89f52-130">Příklad</span><span class="sxs-lookup"><span data-stu-id="89f52-130">Example</span></span>

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;

    namespace AdaptiveStreamingMESPresest
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

## <span data-ttu-id="89f52-131"><a id="output"></a>Výstup</span><span class="sxs-lookup"><span data-stu-id="89f52-131"><a id="output"></a>Output</span></span>

<span data-ttu-id="89f52-132">Tato část ukazuje tři příklady vytvoření MES v důsledku kódování s hello vrstev výstup **adaptivní datové proudy** přednastavené.</span><span class="sxs-lookup"><span data-stu-id="89f52-132">This section shows three examples of output layers produced by MES as a result of encoding with hello **Adaptive Streaming** preset.</span></span> 

### <a name="example-1"></a><span data-ttu-id="89f52-133">Příklad 1</span><span class="sxs-lookup"><span data-stu-id="89f52-133">Example 1</span></span>
<span data-ttu-id="89f52-134">Zdroj s výšky "1080" a "29.970" kmitočet snímků vytváří 6 video vrstvy:</span><span class="sxs-lookup"><span data-stu-id="89f52-134">Source with height "1080" and framerate "29.970" produces 6 video layers:</span></span>

|<span data-ttu-id="89f52-135">Vrstva</span><span class="sxs-lookup"><span data-stu-id="89f52-135">Layer</span></span>|<span data-ttu-id="89f52-136">Výška</span><span class="sxs-lookup"><span data-stu-id="89f52-136">Height</span></span>|<span data-ttu-id="89f52-137">Šířka</span><span class="sxs-lookup"><span data-stu-id="89f52-137">Width</span></span>|<span data-ttu-id="89f52-138">Bitrate(Kbps)</span><span class="sxs-lookup"><span data-stu-id="89f52-138">Bitrate(kbps)</span></span>|
|---|---|---|---|
|<span data-ttu-id="89f52-139">1</span><span class="sxs-lookup"><span data-stu-id="89f52-139">1</span></span>|<span data-ttu-id="89f52-140">1080</span><span class="sxs-lookup"><span data-stu-id="89f52-140">1080</span></span>|<span data-ttu-id="89f52-141">1920</span><span class="sxs-lookup"><span data-stu-id="89f52-141">1920</span></span>|<span data-ttu-id="89f52-142">6780</span><span class="sxs-lookup"><span data-stu-id="89f52-142">6780</span></span>|
|<span data-ttu-id="89f52-143">2</span><span class="sxs-lookup"><span data-stu-id="89f52-143">2</span></span>|<span data-ttu-id="89f52-144">720</span><span class="sxs-lookup"><span data-stu-id="89f52-144">720</span></span>|<span data-ttu-id="89f52-145">1280</span><span class="sxs-lookup"><span data-stu-id="89f52-145">1280</span></span>|<span data-ttu-id="89f52-146">3520</span><span class="sxs-lookup"><span data-stu-id="89f52-146">3520</span></span>|
|<span data-ttu-id="89f52-147">3</span><span class="sxs-lookup"><span data-stu-id="89f52-147">3</span></span>|<span data-ttu-id="89f52-148">540</span><span class="sxs-lookup"><span data-stu-id="89f52-148">540</span></span>|<span data-ttu-id="89f52-149">960</span><span class="sxs-lookup"><span data-stu-id="89f52-149">960</span></span>|<span data-ttu-id="89f52-150">2210</span><span class="sxs-lookup"><span data-stu-id="89f52-150">2210</span></span>|
|<span data-ttu-id="89f52-151">4</span><span class="sxs-lookup"><span data-stu-id="89f52-151">4</span></span>|<span data-ttu-id="89f52-152">360</span><span class="sxs-lookup"><span data-stu-id="89f52-152">360</span></span>|<span data-ttu-id="89f52-153">640</span><span class="sxs-lookup"><span data-stu-id="89f52-153">640</span></span>|<span data-ttu-id="89f52-154">1150</span><span class="sxs-lookup"><span data-stu-id="89f52-154">1150</span></span>|
|<span data-ttu-id="89f52-155">5</span><span class="sxs-lookup"><span data-stu-id="89f52-155">5</span></span>|<span data-ttu-id="89f52-156">270</span><span class="sxs-lookup"><span data-stu-id="89f52-156">270</span></span>|<span data-ttu-id="89f52-157">480</span><span class="sxs-lookup"><span data-stu-id="89f52-157">480</span></span>|<span data-ttu-id="89f52-158">720</span><span class="sxs-lookup"><span data-stu-id="89f52-158">720</span></span>|
|<span data-ttu-id="89f52-159">6</span><span class="sxs-lookup"><span data-stu-id="89f52-159">6</span></span>|<span data-ttu-id="89f52-160">180</span><span class="sxs-lookup"><span data-stu-id="89f52-160">180</span></span>|<span data-ttu-id="89f52-161">320</span><span class="sxs-lookup"><span data-stu-id="89f52-161">320</span></span>|<span data-ttu-id="89f52-162">380</span><span class="sxs-lookup"><span data-stu-id="89f52-162">380</span></span>|

### <a name="example-2"></a><span data-ttu-id="89f52-163">Příklad 2</span><span class="sxs-lookup"><span data-stu-id="89f52-163">Example 2</span></span>
<span data-ttu-id="89f52-164">Zdroj s výšky "720" a "23.970" kmitočet snímků vytváří 5 video vrstvy:</span><span class="sxs-lookup"><span data-stu-id="89f52-164">Source with height "720" and framerate "23.970" produces 5 video layers:</span></span>

|<span data-ttu-id="89f52-165">Vrstva</span><span class="sxs-lookup"><span data-stu-id="89f52-165">Layer</span></span>|<span data-ttu-id="89f52-166">Výška</span><span class="sxs-lookup"><span data-stu-id="89f52-166">Height</span></span>|<span data-ttu-id="89f52-167">Šířka</span><span class="sxs-lookup"><span data-stu-id="89f52-167">Width</span></span>|<span data-ttu-id="89f52-168">Bitrate(Kbps)</span><span class="sxs-lookup"><span data-stu-id="89f52-168">Bitrate(kbps)</span></span>|
|---|---|---|---|
|<span data-ttu-id="89f52-169">1</span><span class="sxs-lookup"><span data-stu-id="89f52-169">1</span></span>|<span data-ttu-id="89f52-170">720</span><span class="sxs-lookup"><span data-stu-id="89f52-170">720</span></span>|<span data-ttu-id="89f52-171">1280</span><span class="sxs-lookup"><span data-stu-id="89f52-171">1280</span></span>|<span data-ttu-id="89f52-172">2940</span><span class="sxs-lookup"><span data-stu-id="89f52-172">2940</span></span>|
|<span data-ttu-id="89f52-173">2</span><span class="sxs-lookup"><span data-stu-id="89f52-173">2</span></span>|<span data-ttu-id="89f52-174">540</span><span class="sxs-lookup"><span data-stu-id="89f52-174">540</span></span>|<span data-ttu-id="89f52-175">960</span><span class="sxs-lookup"><span data-stu-id="89f52-175">960</span></span>|<span data-ttu-id="89f52-176">1850</span><span class="sxs-lookup"><span data-stu-id="89f52-176">1850</span></span>|
|<span data-ttu-id="89f52-177">3</span><span class="sxs-lookup"><span data-stu-id="89f52-177">3</span></span>|<span data-ttu-id="89f52-178">360</span><span class="sxs-lookup"><span data-stu-id="89f52-178">360</span></span>|<span data-ttu-id="89f52-179">640</span><span class="sxs-lookup"><span data-stu-id="89f52-179">640</span></span>|<span data-ttu-id="89f52-180">960</span><span class="sxs-lookup"><span data-stu-id="89f52-180">960</span></span>|
|<span data-ttu-id="89f52-181">4</span><span class="sxs-lookup"><span data-stu-id="89f52-181">4</span></span>|<span data-ttu-id="89f52-182">270</span><span class="sxs-lookup"><span data-stu-id="89f52-182">270</span></span>|<span data-ttu-id="89f52-183">480</span><span class="sxs-lookup"><span data-stu-id="89f52-183">480</span></span>|<span data-ttu-id="89f52-184">600</span><span class="sxs-lookup"><span data-stu-id="89f52-184">600</span></span>|
|<span data-ttu-id="89f52-185">5</span><span class="sxs-lookup"><span data-stu-id="89f52-185">5</span></span>|<span data-ttu-id="89f52-186">180</span><span class="sxs-lookup"><span data-stu-id="89f52-186">180</span></span>|<span data-ttu-id="89f52-187">320</span><span class="sxs-lookup"><span data-stu-id="89f52-187">320</span></span>|<span data-ttu-id="89f52-188">320</span><span class="sxs-lookup"><span data-stu-id="89f52-188">320</span></span>|

### <a name="example-3"></a><span data-ttu-id="89f52-189">Příklad 3</span><span class="sxs-lookup"><span data-stu-id="89f52-189">Example 3</span></span>
<span data-ttu-id="89f52-190">Zdroj s výšky "360" a "29.970" kmitočet snímků vytváří vrstvy 3 videa:</span><span class="sxs-lookup"><span data-stu-id="89f52-190">Source with height "360" and framerate "29.970" produces 3 video layers:</span></span>

|<span data-ttu-id="89f52-191">Vrstva</span><span class="sxs-lookup"><span data-stu-id="89f52-191">Layer</span></span>|<span data-ttu-id="89f52-192">Výška</span><span class="sxs-lookup"><span data-stu-id="89f52-192">Height</span></span>|<span data-ttu-id="89f52-193">Šířka</span><span class="sxs-lookup"><span data-stu-id="89f52-193">Width</span></span>|<span data-ttu-id="89f52-194">Bitrate(Kbps)</span><span class="sxs-lookup"><span data-stu-id="89f52-194">Bitrate(kbps)</span></span>|
|---|---|---|---|
|<span data-ttu-id="89f52-195">1</span><span class="sxs-lookup"><span data-stu-id="89f52-195">1</span></span>|<span data-ttu-id="89f52-196">360</span><span class="sxs-lookup"><span data-stu-id="89f52-196">360</span></span>|<span data-ttu-id="89f52-197">640</span><span class="sxs-lookup"><span data-stu-id="89f52-197">640</span></span>|<span data-ttu-id="89f52-198">700</span><span class="sxs-lookup"><span data-stu-id="89f52-198">700</span></span>|
|<span data-ttu-id="89f52-199">2</span><span class="sxs-lookup"><span data-stu-id="89f52-199">2</span></span>|<span data-ttu-id="89f52-200">270</span><span class="sxs-lookup"><span data-stu-id="89f52-200">270</span></span>|<span data-ttu-id="89f52-201">480</span><span class="sxs-lookup"><span data-stu-id="89f52-201">480</span></span>|<span data-ttu-id="89f52-202">440</span><span class="sxs-lookup"><span data-stu-id="89f52-202">440</span></span>|
|<span data-ttu-id="89f52-203">3</span><span class="sxs-lookup"><span data-stu-id="89f52-203">3</span></span>|<span data-ttu-id="89f52-204">180</span><span class="sxs-lookup"><span data-stu-id="89f52-204">180</span></span>|<span data-ttu-id="89f52-205">320</span><span class="sxs-lookup"><span data-stu-id="89f52-205">320</span></span>|<span data-ttu-id="89f52-206">230</span><span class="sxs-lookup"><span data-stu-id="89f52-206">230</span></span>|
## <a name="media-services-learning-paths"></a><span data-ttu-id="89f52-207">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="89f52-207">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="89f52-208">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="89f52-208">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="89f52-209">Viz také</span><span class="sxs-lookup"><span data-stu-id="89f52-209">See Also</span></span>
[<span data-ttu-id="89f52-210">Kódování Přehled služby Media Services</span><span class="sxs-lookup"><span data-stu-id="89f52-210">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)

