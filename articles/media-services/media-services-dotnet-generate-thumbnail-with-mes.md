---
title: "miniatury toogenerate aaaHow pomocí kodéru Media Encoder Standard s rozhraním .NET"
description: "Toto téma ukazuje, jak toouse .NET tooencode prostředek a vytváření miniatur na hello současně pomocí kodéru Media Encoder Standard."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: b8dab73a-1d91-4b6d-9741-a92ad39fc3f7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: juliako
ms.openlocfilehash: 23d3e4d9bf64a688d45499c045f19d2792167990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toogenerate-thumbnails-using-media-encoder-standard-with-net"></a><span data-ttu-id="05e12-103">Jak toogenerate miniatur pomocí kodéru Media Encoder Standard s rozhraním .NET</span><span class="sxs-lookup"><span data-stu-id="05e12-103">How toogenerate thumbnails using Media Encoder Standard with .NET</span></span>

<span data-ttu-id="05e12-104">Pomocí Media Encoder Standard toogenerate jeden nebo více miniatur z váš vstup videa v [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), nebo [BMP](https://en.wikipedia.org/wiki/BMP_file_format) bitové kopie formáty souborů.</span><span class="sxs-lookup"><span data-stu-id="05e12-104">You can use Media Encoder Standard toogenerate one or more thumbnails from your input video in [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), or [BMP](https://en.wikipedia.org/wiki/BMP_file_format) image file formats.</span></span> <span data-ttu-id="05e12-105">Můžete odeslat úlohy, které vytvářejí pouze obrázky, nebo můžete kombinovat miniatur generování s kódováním.</span><span class="sxs-lookup"><span data-stu-id="05e12-105">You can submit Tasks that produce only images, or you can combine thumbnail generation with encoding.</span></span> <span data-ttu-id="05e12-106">Toto téma obsahuje několik ukázkových XML a JSON miniatur přednastavení pro takové scénáře.</span><span class="sxs-lookup"><span data-stu-id="05e12-106">This topic provides a few sample XML and JSON thumbnail presets for such scenarios.</span></span> <span data-ttu-id="05e12-107">Na konci hello hello tématu, je [ukázkový kód](#code_sample) který ukazuje, jak toouse hello sady Media Services .NET SDK tooaccomplish hello kódování úloh.</span><span class="sxs-lookup"><span data-stu-id="05e12-107">At hello end of hello topic, there is a [sample code](#code_sample) that shows how toouse hello Media Services .NET SDK tooaccomplish hello encoding task.</span></span>

<span data-ttu-id="05e12-108">Další informace o hello elementy, které se používají v ukázkové přednastavení, měli byste prostudovat [Media Encoder Standard schématu](media-services-mes-schema.md).</span><span class="sxs-lookup"><span data-stu-id="05e12-108">For more details on hello elements that are used in sample presets, you should review [Media Encoder Standard schema](media-services-mes-schema.md).</span></span>

<span data-ttu-id="05e12-109">Ujistěte se, zda text hello tooreview [aspekty](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) části.</span><span class="sxs-lookup"><span data-stu-id="05e12-109">Make sure tooreview hello [Considerations](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) section.</span></span>

## <a name="example--single-png-file"></a><span data-ttu-id="05e12-110">Příklad – jeden soubor PNG</span><span class="sxs-lookup"><span data-stu-id="05e12-110">Example – single PNG file</span></span>

<span data-ttu-id="05e12-111">Hello následující JSON a XML přednastavených lze použít tooproduce jediného výstupu PNG souboru mimo hello nejprve několik sekund hello vstupní video, kde hello kodér díky best effort pokusem hledání "zajímavé" rámce.</span><span class="sxs-lookup"><span data-stu-id="05e12-111">hello following JSON and XML preset can be used tooproduce a single output PNG file out of hello first few seconds of hello input video, where hello encoder makes a best-effort attempt at finding an “interesting” frame.</span></span> <span data-ttu-id="05e12-112">Všimněte si, že dimenze image výstup hello byla nastavena too100 %, což znamená, že toto nastavení bude odpovídat hello rozměry vstupní video hello.</span><span class="sxs-lookup"><span data-stu-id="05e12-112">Note that hello output image dimensions have been set too100%, meaning these will match hello dimensions of hello input video.</span></span> <span data-ttu-id="05e12-113">Všimněte si také, jak je požadované nastavení "Format" hello "Výstupy" použití hello toomatch "PngLayers" v části "Kodeky" hello.</span><span class="sxs-lookup"><span data-stu-id="05e12-113">Note also how hello “Format” setting in “Outputs” is required toomatch hello use of “PngLayers” in hello “Codecs” section.</span></span> 

### <a name="json-preset"></a><span data-ttu-id="05e12-114">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="05e12-114">JSON preset</span></span>

    {
      "Version": 1.0,
      "Codecs": [
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": "100%",
              "Height": "100%"
            }
          ],
          "Start": "{Best}",
          "Type": "PngImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        }
      ]
    }
    
### <a name="xml-preset"></a><span data-ttu-id="05e12-115">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="05e12-115">XML preset</span></span>

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <PngImage Start="{Best}">
          <PngLayers>
            <PngLayer>
              <Width>100%</Width>
              <Height>100%</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

## <a name="example--a-series-of-jpeg-images"></a><span data-ttu-id="05e12-116">Příklad – řadu JPEG – obrázky</span><span class="sxs-lookup"><span data-stu-id="05e12-116">Example – a series of JPEG images</span></span>

<span data-ttu-id="05e12-117">Hello následující JSON a XML přednastavených lze použít tooproduce sadu 10 Image v časová razítka 5 % 15 %,..., 95 % hello vstupní časová osa, kde velikost bitové kopie hello je zadaný toobe jeden čtvrtletí, který hello vstupní video.</span><span class="sxs-lookup"><span data-stu-id="05e12-117">hello following JSON and XML preset can be used tooproduce a set of 10 images at timestamps of 5%, 15%, …, 95% of hello input timeline, where hello image size is specified toobe one quarter that of hello input video.</span></span>

### <a name="json-preset"></a><span data-ttu-id="05e12-118">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="05e12-118">JSON preset</span></span>

    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "25%",
              "Height": "25%"
            }
          ],
          "Start": "5%",
          "Step": "1",
          "Range": "1",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
      ]
    }

### <a name="xml-preset"></a><span data-ttu-id="05e12-119">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="05e12-119">XML preset</span></span>
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <JpgImage Start="5%" Step="10%" Range="96%">
          <JpgLayers>
            <JpgLayer>
              <Width>25%</Width>
              <Height>25%</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>

## <a name="example--one-image-at-a-specific-timestamp"></a><span data-ttu-id="05e12-120">Příklad – jednu bitovou kopii v konkrétní časové razítko</span><span class="sxs-lookup"><span data-stu-id="05e12-120">Example – one image at a specific timestamp</span></span>

<span data-ttu-id="05e12-121">Hello následující přednastavených JSON a XML lze použít tooproduce jedné image JPEG v hello 30 sekundu označte vstupní videa hello.</span><span class="sxs-lookup"><span data-stu-id="05e12-121">hello following JSON and XML preset can be used tooproduce a single JPEG image at hello 30 second mark of hello input video.</span></span> <span data-ttu-id="05e12-122">Tato předvolba očekává hello vstupní toobe více než 30 sekund doby trvání (else hello úlohy se nezdaří).</span><span class="sxs-lookup"><span data-stu-id="05e12-122">This preset expects hello input toobe more than 30 seconds in duration (else hello job will fail).</span></span>

### <a name="json-preset"></a><span data-ttu-id="05e12-123">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="05e12-123">JSON preset</span></span>

    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "25%",
              "Height": "25%"
            }
          ],
          "Start": "00:00:30",
          "Step": "1",
          "Range": "1",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
      ]
    }
    
### <a name="xml-preset"></a><span data-ttu-id="05e12-124">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="05e12-124">XML preset</span></span>
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <JpgImage Start="00:00:30" Step="00:00:02" Range="00:00:01">
          <JpgLayers>
            <JpgLayer>
              <Width>25%</Width>
              <Height>25%</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>

## <span data-ttu-id="05e12-125"><a id="code_sample"></a>Příklad – zakódovat video a generovat miniaturu</span><span class="sxs-lookup"><span data-stu-id="05e12-125"><a id="code_sample"></a>Example – encode video and generate thumbnail</span></span>

<span data-ttu-id="05e12-126">Následující ukázka kódu Hello používá sadu Media Services .NET SDK tooperform hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="05e12-126">hello following code example uses Media Services .NET SDK tooperform hello following tasks:</span></span>

* <span data-ttu-id="05e12-127">Vytvořte úlohu kódování.</span><span class="sxs-lookup"><span data-stu-id="05e12-127">Create an encoding job.</span></span>
* <span data-ttu-id="05e12-128">Získání kodéru Media Encoder Standard toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="05e12-128">Get a reference toohello Media Encoder Standard encoder.</span></span>
* <span data-ttu-id="05e12-129">Předvolby zatížení hello [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) nebo [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) obsahující hello kódování přednastavení stejně jako miniatury toogenerate potřebné informace.</span><span class="sxs-lookup"><span data-stu-id="05e12-129">Load hello preset [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) or [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) that contain hello encoding preset as well as information needed toogenerate thumbnails.</span></span> <span data-ttu-id="05e12-130">To můžete uložit [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) nebo [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) v souboru a použití hello následující kód tooload hello soubor.</span><span class="sxs-lookup"><span data-stu-id="05e12-130">You can save this  [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) or [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) in a file and use hello following code tooload hello file.</span></span>
  
        // Load hello XML (or JSON) from hello local file.
        string configuration = File.ReadAllText(fileName);  
* <span data-ttu-id="05e12-131">Přidejte jeden úlohy kódování toohello úloh.</span><span class="sxs-lookup"><span data-stu-id="05e12-131">Add a single encoding task toohello job.</span></span> 
* <span data-ttu-id="05e12-132">Zadejte vstup hello asset toobe kódování.</span><span class="sxs-lookup"><span data-stu-id="05e12-132">Specify hello input asset toobe encoded.</span></span>
* <span data-ttu-id="05e12-133">Vytvoření výstupní asset, který bude obsahovat kódovaný hello asset.</span><span class="sxs-lookup"><span data-stu-id="05e12-133">Create an output asset that will contain hello encoded asset.</span></span>
* <span data-ttu-id="05e12-134">Přidejte průběh úlohy toocheck hello událost obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="05e12-134">Add an event handler toocheck hello job progress.</span></span>
* <span data-ttu-id="05e12-135">Odešlete úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="05e12-135">Submit hello job.</span></span>

<span data-ttu-id="05e12-136">V tématu hello [vývoj Media Services pomocí rozhraní .NET](media-services-dotnet-how-to-use.md) tématu pokyny o tom, tooset prostředí vývojářů.</span><span class="sxs-lookup"><span data-stu-id="05e12-136">See hello [Media Services development with .NET](media-services-dotnet-how-to-use.md) topic for directions on how tooset up your dev environment.</span></span>

        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;

        namespace EncodeAndGenerateThumbnails
        {
        class Program
        {
            // Read values from hello App.config file.
            private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
            private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

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

            // Encode and generate hello thumbnails.
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

            // Load hello XML (or JSON) from hello local file.
            string configuration = File.ReadAllText("ThumbnailPreset_JSON.json");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                processor,
                configuration,
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

## <span data-ttu-id="05e12-137"><a id="json"></a>Miniatury přednastavených JSON</span><span class="sxs-lookup"><span data-stu-id="05e12-137"><a id="json"></a>Thumbnail JSON preset</span></span>
<span data-ttu-id="05e12-138">Informace o schématu najdete v tématu [to](https://msdn.microsoft.com/library/mt269962.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="05e12-138">For information about schema, see [this](https://msdn.microsoft.com/library/mt269962.aspx) topic.</span></span>

    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
    
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "100%",
              "Height": "100%"
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Resolution}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <span data-ttu-id="05e12-139"><a id="xml"></a>Miniatury přednastavených XML</span><span class="sxs-lookup"><span data-stu-id="05e12-139"><a id="xml"></a>Thumbnail XML preset</span></span>
<span data-ttu-id="05e12-140">Informace o schématu najdete v tématu [to](https://msdn.microsoft.com/library/mt269962.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="05e12-140">For information about schema, see [this](https://msdn.microsoft.com/library/mt269962.aspx) topic.</span></span>
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>100%</Width>
              <Height>100%/Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Resolution}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>

## <a name="considerations"></a><span data-ttu-id="05e12-141">Požadavky</span><span class="sxs-lookup"><span data-stu-id="05e12-141">Considerations</span></span>
<span data-ttu-id="05e12-142">použít Hello následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="05e12-142">hello following considerations apply:</span></span>

* <span data-ttu-id="05e12-143">použití Hello explicitní časová razítka pro spuštění nebo krok nebo rozsah předpokládá, že tento vstupní zdroj hello je dlouhé alespoň 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="05e12-143">hello use of explicit timestamps for Start/Step/Range assumes that hello input source is at least 1 minute long.</span></span>
* <span data-ttu-id="05e12-144">JPG nebo Png nebo BmpImage prvky mít počáteční, krok a rozsah řetězce atributy – to jde interpretovat jako:</span><span class="sxs-lookup"><span data-stu-id="05e12-144">Jpg/Png/BmpImage elements have Start, Step and Range string attributes – these can be interpreted as:</span></span>
  
  * <span data-ttu-id="05e12-145">Číslo rámce, pokud jsou nezáporná celá čísla, např.</span><span class="sxs-lookup"><span data-stu-id="05e12-145">Frame Number if they are non-negative integers, eg.</span></span> <span data-ttu-id="05e12-146">"Start": "120",</span><span class="sxs-lookup"><span data-stu-id="05e12-146">"Start": "120",</span></span>
  * <span data-ttu-id="05e12-147">Doba trvání relativní toosource Pokud vyjádřený jako konci %, např.</span><span class="sxs-lookup"><span data-stu-id="05e12-147">Relative toosource duration if expressed as %-suffixed, eg.</span></span> <span data-ttu-id="05e12-148">"Start": "15 %", nebo</span><span class="sxs-lookup"><span data-stu-id="05e12-148">"Start": "15%", OR</span></span>
  * <span data-ttu-id="05e12-149">Časové razítko, pokud vyjádřený jako hh: mm:...</span><span class="sxs-lookup"><span data-stu-id="05e12-149">Timestamp if expressed as HH:MM:SS…</span></span> <span data-ttu-id="05e12-150">formát.</span><span class="sxs-lookup"><span data-stu-id="05e12-150">format.</span></span> <span data-ttu-id="05e12-151">Např.</span><span class="sxs-lookup"><span data-stu-id="05e12-151">Eg.</span></span> <span data-ttu-id="05e12-152">"Start": "00: 01:00"</span><span class="sxs-lookup"><span data-stu-id="05e12-152">"Start" : "00:01:00"</span></span>
    
    <span data-ttu-id="05e12-153">Můžete kombinovat a párovat zápisy, jako je prosím.</span><span class="sxs-lookup"><span data-stu-id="05e12-153">You can mix and match notations as you please.</span></span>
    
    <span data-ttu-id="05e12-154">Kromě toho spustit také podporuje speciální makra: {osvědčené}, který pokusí toodetermine hello první "zajímavé" snímek obsahu hello Poznámka: (krok a rozsah ignorují při spuštění je nastaven příliš {nejlepší})</span><span class="sxs-lookup"><span data-stu-id="05e12-154">Additionally, Start also supports a special Macro:{Best}, which attempts toodetermine hello first “interesting” frame of hello content NOTE: (Step and Range are ignored when Start is set too{Best})</span></span>
  * <span data-ttu-id="05e12-155">Výchozí nastavení: Spuštění: {nejlepší}</span><span class="sxs-lookup"><span data-stu-id="05e12-155">Defaults: Start:{Best}</span></span>
* <span data-ttu-id="05e12-156">Výstupní formát potřebuje toobe explicitně zadaná pro každý formát obrázku: Jpg nebo Png nebo BmpFormat.</span><span class="sxs-lookup"><span data-stu-id="05e12-156">Output format needs toobe explicitly provided for each Image format: Jpg/Png/BmpFormat.</span></span> <span data-ttu-id="05e12-157">Pokud jsou k dispozici, MES bude odpovídat JpgVideo tooJpgFormat a tak dále.</span><span class="sxs-lookup"><span data-stu-id="05e12-157">When present, MES will match JpgVideo tooJpgFormat and so on.</span></span> <span data-ttu-id="05e12-158">OutputFormat zavádí nové makro konkrétní kodek obrázků: {Index}, který potřebuje toobe přítomen (jednou a jen jednou) pro výstupní formáty bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="05e12-158">OutputFormat introduces a new image-codec specific Macro: {Index}, which needs toobe present (once and only once) for image output formats.</span></span>

## <a name="next-steps"></a><span data-ttu-id="05e12-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="05e12-159">Next steps</span></span>

<span data-ttu-id="05e12-160">Můžete zkontrolovat hello [úlohy průběh](media-services-check-job-progress.md) při hello čeká na vyřízení úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="05e12-160">You can check hello [job progress](media-services-check-job-progress.md) while hello encoding job is pending.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="05e12-161">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="05e12-161">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="05e12-162">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="05e12-162">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="05e12-163">Viz také</span><span class="sxs-lookup"><span data-stu-id="05e12-163">See Also</span></span>
[<span data-ttu-id="05e12-164">Kódování Přehled služby Media Services</span><span class="sxs-lookup"><span data-stu-id="05e12-164">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)

