---
title: "Postup generování miniatur pomocí kodéru Media Encoder Standard a .NET"
description: "Toto téma ukazuje, jak pomocí rozhraní .NET kódování assetu a vytváření miniatur současně pomocí kodéru Media Encoder Standard."
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
ms.openlocfilehash: 89d15cbdf71a140e78f34e07ff208776b7d4cab3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-generate-thumbnails-using-media-encoder-standard-with-net"></a><span data-ttu-id="31e3d-103">Postup generování miniatur pomocí kodéru Media Encoder Standard a .NET</span><span class="sxs-lookup"><span data-stu-id="31e3d-103">How to generate thumbnails using Media Encoder Standard with .NET</span></span>

<span data-ttu-id="31e3d-104">Můžete použít Media Encoder Standard generovat jednu nebo více miniatur z váš vstup videa v [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), nebo [BMP](https://en.wikipedia.org/wiki/BMP_file_format) bitové kopie formáty souborů.</span><span class="sxs-lookup"><span data-stu-id="31e3d-104">You can use Media Encoder Standard to generate one or more thumbnails from your input video in [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), or [BMP](https://en.wikipedia.org/wiki/BMP_file_format) image file formats.</span></span> <span data-ttu-id="31e3d-105">Můžete odeslat úlohy, které vytvářejí pouze obrázky, nebo můžete kombinovat miniatur generování s kódováním.</span><span class="sxs-lookup"><span data-stu-id="31e3d-105">You can submit Tasks that produce only images, or you can combine thumbnail generation with encoding.</span></span> <span data-ttu-id="31e3d-106">Toto téma obsahuje několik ukázkových XML a JSON miniatur přednastavení pro takové scénáře.</span><span class="sxs-lookup"><span data-stu-id="31e3d-106">This topic provides a few sample XML and JSON thumbnail presets for such scenarios.</span></span> <span data-ttu-id="31e3d-107">Na konci tohoto tématu, je [ukázkový kód](#code_sample) který ukazuje, jak používat sadu Media Services .NET SDK k provedení úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="31e3d-107">At the end of the topic, there is a [sample code](#code_sample) that shows how to use the Media Services .NET SDK to accomplish the encoding task.</span></span>

<span data-ttu-id="31e3d-108">Další informace o prvky, které se používají v ukázkové přednastavení, měli byste prostudovat [Media Encoder Standard schématu](media-services-mes-schema.md).</span><span class="sxs-lookup"><span data-stu-id="31e3d-108">For more details on the elements that are used in sample presets, you should review [Media Encoder Standard schema](media-services-mes-schema.md).</span></span>

<span data-ttu-id="31e3d-109">Projděte si [aspekty](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) části.</span><span class="sxs-lookup"><span data-stu-id="31e3d-109">Make sure to review the [Considerations](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) section.</span></span>

## <a name="example--single-png-file"></a><span data-ttu-id="31e3d-110">Příklad – jeden soubor PNG</span><span class="sxs-lookup"><span data-stu-id="31e3d-110">Example – single PNG file</span></span>

<span data-ttu-id="31e3d-111">Následující přednastavení JSON a XML lze vytvořit jednu výstupní soubor PNG mimo první několik sekund vstupní video, kde kodér díky best effort pokusem hledání "zajímavé" rámce.</span><span class="sxs-lookup"><span data-stu-id="31e3d-111">The following JSON and XML preset can be used to produce a single output PNG file out of the first few seconds of the input video, where the encoder makes a best-effort attempt at finding an “interesting” frame.</span></span> <span data-ttu-id="31e3d-112">Všimněte si, že dimenze výstup bitové kopie byly nastaveny na 100 %, znamená to bude odpovídat dimenze vstupní video.</span><span class="sxs-lookup"><span data-stu-id="31e3d-112">Note that the output image dimensions have been set to 100%, meaning these will match the dimensions of the input video.</span></span> <span data-ttu-id="31e3d-113">Všimněte si také, jak je "Format" nastavení "Výstupy" vyžadovanou pro shodu použití "PngLayers" v části "Kodeky".</span><span class="sxs-lookup"><span data-stu-id="31e3d-113">Note also how the “Format” setting in “Outputs” is required to match the use of “PngLayers” in the “Codecs” section.</span></span> 

### <a name="json-preset"></a><span data-ttu-id="31e3d-114">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="31e3d-114">JSON preset</span></span>

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
    
### <a name="xml-preset"></a><span data-ttu-id="31e3d-115">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="31e3d-115">XML preset</span></span>

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

## <a name="example--a-series-of-jpeg-images"></a><span data-ttu-id="31e3d-116">Příklad – řadu JPEG – obrázky</span><span class="sxs-lookup"><span data-stu-id="31e3d-116">Example – a series of JPEG images</span></span>

<span data-ttu-id="31e3d-117">Následující přednastavení JSON a XML lze vytvořit sady 10 Image v časová razítka 5 % 15 %,..., 95 % vstupní časová osa tam, kde je zadán velikost bitové kopie na jednu čtvrtletí, vstup videa.</span><span class="sxs-lookup"><span data-stu-id="31e3d-117">The following JSON and XML preset can be used to produce a set of 10 images at timestamps of 5%, 15%, …, 95% of the input timeline, where the image size is specified to be one quarter that of the input video.</span></span>

### <a name="json-preset"></a><span data-ttu-id="31e3d-118">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="31e3d-118">JSON preset</span></span>

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

### <a name="xml-preset"></a><span data-ttu-id="31e3d-119">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="31e3d-119">XML preset</span></span>
    
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

## <a name="example--one-image-at-a-specific-timestamp"></a><span data-ttu-id="31e3d-120">Příklad – jednu bitovou kopii v konkrétní časové razítko</span><span class="sxs-lookup"><span data-stu-id="31e3d-120">Example – one image at a specific timestamp</span></span>

<span data-ttu-id="31e3d-121">Následující přednastavení JSON a XML lze použít k vytvoření jedné JPEG image na 30 druhý značce vstupní videa.</span><span class="sxs-lookup"><span data-stu-id="31e3d-121">The following JSON and XML preset can be used to produce a single JPEG image at the 30 second mark of the input video.</span></span> <span data-ttu-id="31e3d-122">Tato předvolba očekává vstup být víc než 30 sekund doby trvání (jinak se nezdaří úlohy).</span><span class="sxs-lookup"><span data-stu-id="31e3d-122">This preset expects the input to be more than 30 seconds in duration (else the job will fail).</span></span>

### <a name="json-preset"></a><span data-ttu-id="31e3d-123">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="31e3d-123">JSON preset</span></span>

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
    
### <a name="xml-preset"></a><span data-ttu-id="31e3d-124">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="31e3d-124">XML preset</span></span>
    
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

## <span data-ttu-id="31e3d-125"><a id="code_sample"></a>Příklad – zakódovat video a generovat miniaturu</span><span class="sxs-lookup"><span data-stu-id="31e3d-125"><a id="code_sample"></a>Example – encode video and generate thumbnail</span></span>

<span data-ttu-id="31e3d-126">Následující příklad kódu používá sadu Media Services .NET SDK k provádění následujících úloh:</span><span class="sxs-lookup"><span data-stu-id="31e3d-126">The following code example uses Media Services .NET SDK to perform the following tasks:</span></span>

* <span data-ttu-id="31e3d-127">Vytvořte úlohu kódování.</span><span class="sxs-lookup"><span data-stu-id="31e3d-127">Create an encoding job.</span></span>
* <span data-ttu-id="31e3d-128">Získáte odkaz na kodéru Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="31e3d-128">Get a reference to the Media Encoder Standard encoder.</span></span>
* <span data-ttu-id="31e3d-129">Načíst přednastavení [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) nebo [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) obsahující kódování přednastavení a také informace potřebné k vytváření miniatur.</span><span class="sxs-lookup"><span data-stu-id="31e3d-129">Load the preset [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) or [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) that contain the encoding preset as well as information needed to generate thumbnails.</span></span> <span data-ttu-id="31e3d-130">To můžete uložit [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) nebo [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) na soubor a použít následující kód načíst soubor.</span><span class="sxs-lookup"><span data-stu-id="31e3d-130">You can save this  [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) or [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) in a file and use the following code to load the file.</span></span>
  
        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
* <span data-ttu-id="31e3d-131">Přidáte jednoho kódování úkolu do úlohy.</span><span class="sxs-lookup"><span data-stu-id="31e3d-131">Add a single encoding task to the job.</span></span> 
* <span data-ttu-id="31e3d-132">Zadejte vstupní asset, který je zakódován.</span><span class="sxs-lookup"><span data-stu-id="31e3d-132">Specify the input asset to be encoded.</span></span>
* <span data-ttu-id="31e3d-133">Vytvoření výstupní asset, který bude obsahovat k zakódovanému assetu.</span><span class="sxs-lookup"><span data-stu-id="31e3d-133">Create an output asset that will contain the encoded asset.</span></span>
* <span data-ttu-id="31e3d-134">Přidání obslužné rutiny události zkontrolovat průběh úlohy.</span><span class="sxs-lookup"><span data-stu-id="31e3d-134">Add an event handler to check the job progress.</span></span>
* <span data-ttu-id="31e3d-135">Odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="31e3d-135">Submit the job.</span></span>

<span data-ttu-id="31e3d-136">Najdete v článku [vývoj Media Services pomocí rozhraní .NET](media-services-dotnet-how-to-use.md) tématu pokyny o tom, jak nastavit svoje prostředí vývojářů.</span><span class="sxs-lookup"><span data-stu-id="31e3d-136">See the [Media Services development with .NET](media-services-dotnet-how-to-use.md) topic for directions on how to set up your dev environment.</span></span>

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
            // Read values from the App.config file.
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

            // Encode and generate the thumbnails.
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
            string configuration = File.ReadAllText("ThumbnailPreset_JSON.json");

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

## <span data-ttu-id="31e3d-137"><a id="json"></a>Miniatury přednastavených JSON</span><span class="sxs-lookup"><span data-stu-id="31e3d-137"><a id="json"></a>Thumbnail JSON preset</span></span>
<span data-ttu-id="31e3d-138">Informace o schématu najdete v tématu [to](https://msdn.microsoft.com/library/mt269962.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="31e3d-138">For information about schema, see [this](https://msdn.microsoft.com/library/mt269962.aspx) topic.</span></span>

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

## <span data-ttu-id="31e3d-139"><a id="xml"></a>Miniatury přednastavených XML</span><span class="sxs-lookup"><span data-stu-id="31e3d-139"><a id="xml"></a>Thumbnail XML preset</span></span>
<span data-ttu-id="31e3d-140">Informace o schématu najdete v tématu [to](https://msdn.microsoft.com/library/mt269962.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="31e3d-140">For information about schema, see [this](https://msdn.microsoft.com/library/mt269962.aspx) topic.</span></span>
    
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

## <a name="considerations"></a><span data-ttu-id="31e3d-141">Požadavky</span><span class="sxs-lookup"><span data-stu-id="31e3d-141">Considerations</span></span>
<span data-ttu-id="31e3d-142">Platí následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="31e3d-142">The following considerations apply:</span></span>

* <span data-ttu-id="31e3d-143">Použití explicitní časová razítka pro spuštění nebo krok nebo rozsah předpokládá, že se vstupní zdroj alespoň 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="31e3d-143">The use of explicit timestamps for Start/Step/Range assumes that the input source is at least 1 minute long.</span></span>
* <span data-ttu-id="31e3d-144">JPG nebo Png nebo BmpImage prvky mít počáteční, krok a rozsah řetězce atributy – to jde interpretovat jako:</span><span class="sxs-lookup"><span data-stu-id="31e3d-144">Jpg/Png/BmpImage elements have Start, Step and Range string attributes – these can be interpreted as:</span></span>
  
  * <span data-ttu-id="31e3d-145">Číslo rámce, pokud jsou nezáporná celá čísla, např.</span><span class="sxs-lookup"><span data-stu-id="31e3d-145">Frame Number if they are non-negative integers, eg.</span></span> <span data-ttu-id="31e3d-146">"Start": "120",</span><span class="sxs-lookup"><span data-stu-id="31e3d-146">"Start": "120",</span></span>
  * <span data-ttu-id="31e3d-147">Vzhledem ke zdrojové trvání Pokud vyjádřený jako konci %, např.</span><span class="sxs-lookup"><span data-stu-id="31e3d-147">Relative to source duration if expressed as %-suffixed, eg.</span></span> <span data-ttu-id="31e3d-148">"Start": "15 %", nebo</span><span class="sxs-lookup"><span data-stu-id="31e3d-148">"Start": "15%", OR</span></span>
  * <span data-ttu-id="31e3d-149">Časové razítko, pokud vyjádřený jako hh: mm:...</span><span class="sxs-lookup"><span data-stu-id="31e3d-149">Timestamp if expressed as HH:MM:SS…</span></span> <span data-ttu-id="31e3d-150">formát.</span><span class="sxs-lookup"><span data-stu-id="31e3d-150">format.</span></span> <span data-ttu-id="31e3d-151">Např.</span><span class="sxs-lookup"><span data-stu-id="31e3d-151">Eg.</span></span> <span data-ttu-id="31e3d-152">"Start": "00: 01:00"</span><span class="sxs-lookup"><span data-stu-id="31e3d-152">"Start" : "00:01:00"</span></span>
    
    <span data-ttu-id="31e3d-153">Můžete kombinovat a párovat zápisy, jako je prosím.</span><span class="sxs-lookup"><span data-stu-id="31e3d-153">You can mix and match notations as you please.</span></span>
    
    <span data-ttu-id="31e3d-154">Kromě toho spustit také podporuje speciální makra: {osvědčené}, která se pokusí určit první "zajímavé" snímek obsahu Poznámka: (krok a rozsah ignorují při spuštění je nastaven na {nejvhodnější})</span><span class="sxs-lookup"><span data-stu-id="31e3d-154">Additionally, Start also supports a special Macro:{Best}, which attempts to determine the first “interesting” frame of the content NOTE: (Step and Range are ignored when Start is set to {Best})</span></span>
  * <span data-ttu-id="31e3d-155">Výchozí nastavení: Spuštění: {nejlepší}</span><span class="sxs-lookup"><span data-stu-id="31e3d-155">Defaults: Start:{Best}</span></span>
* <span data-ttu-id="31e3d-156">Výstupní formát je třeba explicitně zadat pro každý formát obrázku: Jpg nebo Png nebo BmpFormat.</span><span class="sxs-lookup"><span data-stu-id="31e3d-156">Output format needs to be explicitly provided for each Image format: Jpg/Png/BmpFormat.</span></span> <span data-ttu-id="31e3d-157">Pokud jsou k dispozici, MES bude odpovídat JpgVideo k JpgFormat a tak dále.</span><span class="sxs-lookup"><span data-stu-id="31e3d-157">When present, MES will match JpgVideo to JpgFormat and so on.</span></span> <span data-ttu-id="31e3d-158">OutputFormat zavádí nové makro konkrétní kodek obrázků: {Index}, které musí být k dispozici (jednou a jen jednou) pro výstupní formáty bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="31e3d-158">OutputFormat introduces a new image-codec specific Macro: {Index}, which needs to be present (once and only once) for image output formats.</span></span>

## <a name="next-steps"></a><span data-ttu-id="31e3d-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31e3d-159">Next steps</span></span>

<span data-ttu-id="31e3d-160">Můžete zkontrolovat [úlohy průběh](media-services-check-job-progress.md) úlohy kódování je očekávána.</span><span class="sxs-lookup"><span data-stu-id="31e3d-160">You can check the [job progress](media-services-check-job-progress.md) while the encoding job is pending.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="31e3d-161">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="31e3d-161">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="31e3d-162">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="31e3d-162">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="31e3d-163">Viz také</span><span class="sxs-lookup"><span data-stu-id="31e3d-163">See Also</span></span>
[<span data-ttu-id="31e3d-164">Kódování Přehled služby Media Services</span><span class="sxs-lookup"><span data-stu-id="31e3d-164">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)

