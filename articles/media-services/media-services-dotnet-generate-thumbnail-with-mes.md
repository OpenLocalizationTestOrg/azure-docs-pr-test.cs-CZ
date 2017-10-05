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
# <a name="how-to-generate-thumbnails-using-media-encoder-standard-with-net"></a>Postup generování miniatur pomocí kodéru Media Encoder Standard a .NET

Můžete použít Media Encoder Standard generovat jednu nebo více miniatur z váš vstup videa v [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), nebo [BMP](https://en.wikipedia.org/wiki/BMP_file_format) bitové kopie formáty souborů. Můžete odeslat úlohy, které vytvářejí pouze obrázky, nebo můžete kombinovat miniatur generování s kódováním. Toto téma obsahuje několik ukázkových XML a JSON miniatur přednastavení pro takové scénáře. Na konci tohoto tématu, je [ukázkový kód](#code_sample) který ukazuje, jak používat sadu Media Services .NET SDK k provedení úlohy kódování.

Další informace o prvky, které se používají v ukázkové přednastavení, měli byste prostudovat [Media Encoder Standard schématu](media-services-mes-schema.md).

Projděte si [aspekty](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) části.

## <a name="example--single-png-file"></a>Příklad – jeden soubor PNG

Následující přednastavení JSON a XML lze vytvořit jednu výstupní soubor PNG mimo první několik sekund vstupní video, kde kodér díky best effort pokusem hledání "zajímavé" rámce. Všimněte si, že dimenze výstup bitové kopie byly nastaveny na 100 %, znamená to bude odpovídat dimenze vstupní video. Všimněte si také, jak je "Format" nastavení "Výstupy" vyžadovanou pro shodu použití "PngLayers" v části "Kodeky". 

### <a name="json-preset"></a>Přednastavení JSON

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
    
### <a name="xml-preset"></a>Přednastavení XML

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

## <a name="example--a-series-of-jpeg-images"></a>Příklad – řadu JPEG – obrázky

Následující přednastavení JSON a XML lze vytvořit sady 10 Image v časová razítka 5 % 15 %,..., 95 % vstupní časová osa tam, kde je zadán velikost bitové kopie na jednu čtvrtletí, vstup videa.

### <a name="json-preset"></a>Přednastavení JSON

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

### <a name="xml-preset"></a>Přednastavení XML
    
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

## <a name="example--one-image-at-a-specific-timestamp"></a>Příklad – jednu bitovou kopii v konkrétní časové razítko

Následující přednastavení JSON a XML lze použít k vytvoření jedné JPEG image na 30 druhý značce vstupní videa. Tato předvolba očekává vstup být víc než 30 sekund doby trvání (jinak se nezdaří úlohy).

### <a name="json-preset"></a>Přednastavení JSON

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
    
### <a name="xml-preset"></a>Přednastavení XML
    
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

## <a id="code_sample"></a>Příklad – zakódovat video a generovat miniaturu

Následující příklad kódu používá sadu Media Services .NET SDK k provádění následujících úloh:

* Vytvořte úlohu kódování.
* Získáte odkaz na kodéru Media Encoder Standard.
* Načíst přednastavení [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) nebo [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) obsahující kódování přednastavení a také informace potřebné k vytváření miniatur. To můžete uložit [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) nebo [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) na soubor a použít následující kód načíst soubor.
  
        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
* Přidáte jednoho kódování úkolu do úlohy. 
* Zadejte vstupní asset, který je zakódován.
* Vytvoření výstupní asset, který bude obsahovat k zakódovanému assetu.
* Přidání obslužné rutiny události zkontrolovat průběh úlohy.
* Odeslání úlohy.

Najdete v článku [vývoj Media Services pomocí rozhraní .NET](media-services-dotnet-how-to-use.md) tématu pokyny o tom, jak nastavit svoje prostředí vývojářů.

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

## <a id="json"></a>Miniatury přednastavených JSON
Informace o schématu najdete v tématu [to](https://msdn.microsoft.com/library/mt269962.aspx) tématu.

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

## <a id="xml"></a>Miniatury přednastavených XML
Informace o schématu najdete v tématu [to](https://msdn.microsoft.com/library/mt269962.aspx) tématu.
    
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

## <a name="considerations"></a>Požadavky
Platí následující aspekty:

* Použití explicitní časová razítka pro spuštění nebo krok nebo rozsah předpokládá, že se vstupní zdroj alespoň 1 minuta.
* JPG nebo Png nebo BmpImage prvky mít počáteční, krok a rozsah řetězce atributy – to jde interpretovat jako:
  
  * Číslo rámce, pokud jsou nezáporná celá čísla, např. "Start": "120",
  * Vzhledem ke zdrojové trvání Pokud vyjádřený jako konci %, např. "Start": "15 %", nebo
  * Časové razítko, pokud vyjádřený jako hh: mm:... formát. Např. "Start": "00: 01:00"
    
    Můžete kombinovat a párovat zápisy, jako je prosím.
    
    Kromě toho spustit také podporuje speciální makra: {osvědčené}, která se pokusí určit první "zajímavé" snímek obsahu Poznámka: (krok a rozsah ignorují při spuštění je nastaven na {nejvhodnější})
  * Výchozí nastavení: Spuštění: {nejlepší}
* Výstupní formát je třeba explicitně zadat pro každý formát obrázku: Jpg nebo Png nebo BmpFormat. Pokud jsou k dispozici, MES bude odpovídat JpgVideo k JpgFormat a tak dále. OutputFormat zavádí nové makro konkrétní kodek obrázků: {Index}, které musí být k dispozici (jednou a jen jednou) pro výstupní formáty bitové kopie.

## <a name="next-steps"></a>Další kroky

Můžete zkontrolovat [úlohy průběh](media-services-check-job-progress.md) úlohy kódování je očekávána.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Viz také
[Kódování Přehled služby Media Services](media-services-encode-asset.md)

