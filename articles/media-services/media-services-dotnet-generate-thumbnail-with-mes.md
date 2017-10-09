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
# <a name="how-toogenerate-thumbnails-using-media-encoder-standard-with-net"></a>Jak toogenerate miniatur pomocí kodéru Media Encoder Standard s rozhraním .NET

Pomocí Media Encoder Standard toogenerate jeden nebo více miniatur z váš vstup videa v [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), nebo [BMP](https://en.wikipedia.org/wiki/BMP_file_format) bitové kopie formáty souborů. Můžete odeslat úlohy, které vytvářejí pouze obrázky, nebo můžete kombinovat miniatur generování s kódováním. Toto téma obsahuje několik ukázkových XML a JSON miniatur přednastavení pro takové scénáře. Na konci hello hello tématu, je [ukázkový kód](#code_sample) který ukazuje, jak toouse hello sady Media Services .NET SDK tooaccomplish hello kódování úloh.

Další informace o hello elementy, které se používají v ukázkové přednastavení, měli byste prostudovat [Media Encoder Standard schématu](media-services-mes-schema.md).

Ujistěte se, zda text hello tooreview [aspekty](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) části.

## <a name="example--single-png-file"></a>Příklad – jeden soubor PNG

Hello následující JSON a XML přednastavených lze použít tooproduce jediného výstupu PNG souboru mimo hello nejprve několik sekund hello vstupní video, kde hello kodér díky best effort pokusem hledání "zajímavé" rámce. Všimněte si, že dimenze image výstup hello byla nastavena too100 %, což znamená, že toto nastavení bude odpovídat hello rozměry vstupní video hello. Všimněte si také, jak je požadované nastavení "Format" hello "Výstupy" použití hello toomatch "PngLayers" v části "Kodeky" hello. 

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

Hello následující JSON a XML přednastavených lze použít tooproduce sadu 10 Image v časová razítka 5 % 15 %,..., 95 % hello vstupní časová osa, kde velikost bitové kopie hello je zadaný toobe jeden čtvrtletí, který hello vstupní video.

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

Hello následující přednastavených JSON a XML lze použít tooproduce jedné image JPEG v hello 30 sekundu označte vstupní videa hello. Tato předvolba očekává hello vstupní toobe více než 30 sekund doby trvání (else hello úlohy se nezdaří).

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

Následující ukázka kódu Hello používá sadu Media Services .NET SDK tooperform hello následující úlohy:

* Vytvořte úlohu kódování.
* Získání kodéru Media Encoder Standard toohello odkaz.
* Předvolby zatížení hello [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) nebo [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) obsahující hello kódování přednastavení stejně jako miniatury toogenerate potřebné informace. To můžete uložit [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) nebo [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) v souboru a použití hello následující kód tooload hello soubor.
  
        // Load hello XML (or JSON) from hello local file.
        string configuration = File.ReadAllText(fileName);  
* Přidejte jeden úlohy kódování toohello úloh. 
* Zadejte vstup hello asset toobe kódování.
* Vytvoření výstupní asset, který bude obsahovat kódovaný hello asset.
* Přidejte průběh úlohy toocheck hello událost obslužné rutiny.
* Odešlete úlohu hello.

V tématu hello [vývoj Media Services pomocí rozhraní .NET](media-services-dotnet-how-to-use.md) tématu pokyny o tom, tooset prostředí vývojářů.

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
použít Hello následující aspekty:

* použití Hello explicitní časová razítka pro spuštění nebo krok nebo rozsah předpokládá, že tento vstupní zdroj hello je dlouhé alespoň 1 minuta.
* JPG nebo Png nebo BmpImage prvky mít počáteční, krok a rozsah řetězce atributy – to jde interpretovat jako:
  
  * Číslo rámce, pokud jsou nezáporná celá čísla, např. "Start": "120",
  * Doba trvání relativní toosource Pokud vyjádřený jako konci %, např. "Start": "15 %", nebo
  * Časové razítko, pokud vyjádřený jako hh: mm:... formát. Např. "Start": "00: 01:00"
    
    Můžete kombinovat a párovat zápisy, jako je prosím.
    
    Kromě toho spustit také podporuje speciální makra: {osvědčené}, který pokusí toodetermine hello první "zajímavé" snímek obsahu hello Poznámka: (krok a rozsah ignorují při spuštění je nastaven příliš {nejlepší})
  * Výchozí nastavení: Spuštění: {nejlepší}
* Výstupní formát potřebuje toobe explicitně zadaná pro každý formát obrázku: Jpg nebo Png nebo BmpFormat. Pokud jsou k dispozici, MES bude odpovídat JpgVideo tooJpgFormat a tak dále. OutputFormat zavádí nové makro konkrétní kodek obrázků: {Index}, který potřebuje toobe přítomen (jednou a jen jednou) pro výstupní formáty bitové kopie.

## <a name="next-steps"></a>Další kroky

Můžete zkontrolovat hello [úlohy průběh](media-services-check-job-progress.md) při hello čeká na vyřízení úlohy kódování.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Viz také
[Kódování Přehled služby Media Services](media-services-encode-asset.md)

