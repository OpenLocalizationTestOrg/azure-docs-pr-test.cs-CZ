---
title: "aaaDigitize textu pomocí Azure Media Analytics rozpoznávání znaků | Microsoft Docs"
description: "Rozpoznávání Azure Media Analytics znaků (optické rozpoznávání znaků) umožňuje tooconvert textového obsahu v video soubory do upravovat, vyhledávat digitální textu.  To vám umožní tooautomate hello extrakce smysluplný metadata z hello signál video média."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 307c196e-3a50-4f4b-b982-51585448ffc6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 0476c3ba3942b2c5182a34a429909adbf5c75ac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-media-analytics-tooconvert-text-content-in-video-files-into-digital-text"></a>Pomocí Azure Media Analytics tooconvert textového obsahu v video soubory do digitální textu
## <a name="overview"></a>Přehled
Pokud potřebujete tooextract textového obsahu z video soubory a generovat upravovat, vyhledávat digitální text, měli byste použít rozpoznávání Azure Media Analytics znaků (optické rozpoznávání znaků). Tento procesor médií Azure zjistí textového obsahu v video soubory a vygeneruje textových souborů pro vaše použití. Rozpoznávání znaků umožňuje vám tooautomate hello extrakce smysluplný metadata z hello signál video média.

Při použití ve spojení s vyhledávacího webu, můžete snadno indexu médiu podle textu a vylepšit možnosti rozpoznání hello obsahu. To je velmi užitečné v vysoce textovou video, jako je záznam videa nebo snímek obrazovky prezentace prezentace. Hello procesor médií rozpoznávání znaků Azure je optimalizovaná pro digitální text.

Hello **rozpoznávání Azure Media znaků** procesor médií je aktuálně ve verzi Preview.

Toto téma uvádí podrobnosti o **rozpoznávání Azure Media znaků** a ukazuje, jak toouse ho pomocí sady Media Services SDK pro .NET. Další informace a příklady naleznete v tématu [tomto blogu](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

## <a name="ocr-input-files"></a>Vstupní soubory rozpoznávání znaků
Video soubory. V současné době jsou podporovány následující formáty hello: MP4, MOV a WMV.

## <a name="task-configuration"></a>Konfigurace úlohy
Konfigurace úlohy (přednastavených). Při vytváření úlohy s **rozpoznávání Azure Media znaků**, je nutné zadat konfiguraci přednastavení pomocí XML nebo JSON. 

>[!NOTE]
>modul rozpoznávání znaků Hello pouze vezme oblast bitové kopie s minimální pixelů toomaximum 32000 40 pixelů jako platnou hodnotu v obou výšky a šířky.
>

### <a name="attribute-descriptions"></a>Atribut popisy
| Název atributu | Popis |
| --- | --- |
|AdvancedOutput| Pokud jste nastavili AdvancedOutput tootrue, budou obsahovat výstup JSON hello poziční data pro každou jednoho slova (v přidání toophrases a oblasti). Pokud nechcete, aby toosee tyto podrobnosti, nastavte příznak toofalse hello. Hello výchozí hodnota je false. Další informace najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).|
| Jazyk |(volitelné) popisuje hello jazyk textu, pro které toolook. Jedna z následujících hello: AutoDetect (výchozí), Arabské, ChineseSimplified, ChineseTraditional, čeština dánština, holandština, angličtina, finština, francouzština, němčina, řečtina, maďarština, italština, japonština, korejština, norština, polština, portugalština, rumunština, ruština, SerbianCyrillic, SerbianLatin, slovenština, španělština, švédština, turečtina. |
| TextOrientation |(volitelné) popisuje hello orientaci textu, pro které toolook.  "Left" prostředky, které hello horní části všechna písmena jsou odkazoval směrem doleva hello.  Výchozí text (např., které lze nalézt v podobě knihy) je možné volat "Nahoru" orientované.  Jedna z následujících hello: AutoDetect (výchozí), až, vpravo, dolů, doleva. |
| TimeInterval |(volitelné) popisuje hello vzorkovací frekvenci.  Výchozí hodnota je každou sekundu 1/2.<br/>Formát JSON – hh: mm:. Služby Zabezpečené úložiště (výchozí 00:00:00.500)<br/>Formát XML – doba trvání primitivní W3C XSD (výchozí PT0.5) |
| DetectRegions |(volitelné) Pole objektů DetectRegion určení oblasti v rámci video hello v textu, který toodetect.<br/>Objekt DetectRegion je tvořen hello následující čtyři celočíselné hodnoty:<br/>Vlevo – pixelů z levého okraje hello<br/>TOP – pixelů z hello horní margin<br/>Šířka – Šířka hello oblast v pixelech<br/>Výška – výšku oblasti hello v pixelech |

#### <a name="json-preset-example"></a>Příklad přednastavené JSON

    {
        "Version":1.0, 
        "Options": 
        {
            "AdvancedOutput":"true",
            "Language":"English", 
            "TimeInterval":"00:00:01.5",
            "TextOrientation":"Up",
            "DetectRegions": [
                    {
                       "Left": 10,
                       "Top": 10,
                       "Width": 100,
                       "Height": 50
                    }
             ]
        }
    }


#### <a name="xml-preset-example"></a>Příklad přednastavené XML
    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <AdvancedOutput>true</AdvancedOutput>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>10</Left>
                   <Top>10</Top>
                   <Width>100</Width>
                   <Height>50</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

## <a name="ocr-output-files"></a>Rozpoznávání znaků výstupní soubory
výstup Hello hello rozpoznávání znaků média procesoru je soubor JSON.

### <a name="elements-of-hello-output-json-file"></a>Elementy výstupního souboru JSON, hello
výstup Hello Video rozpoznávání znaků poskytuje segmentované čas data hello znaků, které jsou součástí videa.  Atributy, jako je například jazyk nebo orientaci toohone-in můžete použít na přesně hello slova, že máte zájem analýza. 

výstup Hello obsahuje hello následující atributy:

| Element | Popis |
| --- | --- |
| Časová osa |"rysky" za sekundu hello videa |
| Posun |časového posunu pro časová razítka. Ve verzi 1.0 rozhraní API, Video bude vždy 0. |
| kmitočet snímků |Počet snímků za sekundu hello video |
| Šířka |Šířka hello videa v pixelech |
| Výška |Výška hello videa v pixelech |
| fragmenty |pole založené na čase bloků dat videa, do které hello metadata blokové |
| start |Počáteční čas fragment v "rysky" |
| Doba trvání |Délka fragment v "rysky" |
| interval |Interval jednotlivých událostí v rámci hello zadaný fragment |
| stránka events |pole obsahující oblastí |
| Oblast |objekt představující zjistil slova nebo fráze |
| Jazyk |jazyk textu hello zjistil v rámci oblasti |
| orientace |orientaci textu hello zjistil v rámci oblasti |
| řádky |pole řádků textu zjistil v rámci oblasti |
| Text |vlastní text Hello |

### <a name="json-output-example"></a>Příklad výstupu JSON
Hello následující příklad výstupu obsahuje obecné informace video hello a několik video fragmenty. V každé video fragmentu obsahuje každou oblast, který je zjišťován pomocí MP rozpoznávání znaků s jazykem hello a jeho orientaci textu. Hello oblast také obsahuje každý řádek aplikace word v této oblasti s textem hello řádku, pozice řádku hello a každý word informace (word obsahu, pozice a spolehlivosti) v tomto řádku. Následuje příklad Hello a umístíte některé vložené komentáře.

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // hello time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // hello detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including hello text and hello position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="net-sample-code"></a>Ukázkový kód rozhraní .NET

ukazuje programu Hello následující postup:

1. Vytvořte asset a nahrajte soubor média do hello asset.
2. Vytvoření úlohy se souborem konfigurace nebo přednastavených rozpoznávání znaků.
3. Stáhněte soubory JSON výstup hello. 
   
#### <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia

Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md). 

#### <a name="example"></a>Příklad

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace OCR
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

                // Run hello OCR job.
                var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                            @"C:\supportFiles\OCR\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
            }

            static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My OCR Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My OCR Job");

                // Get a reference tooAzure Media OCR.
                string MediaProcessorName = "Azure Media OCR";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My OCR Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);

                // Use hello following event handler toocheck job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

                // Launch hello job.
                job.Submit();

                // Check job execution and wait for job toofinish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);

                progressJobTask.Wait();

                // If job state is Error, hello event handling
                // method for job progress should log errors.  Here we check
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                    Console.WriteLine(string.Format("Error: {0}. {1}",
                                                    error.Code,
                                                    error.Message));
                    return null;
                }

                return job.OutputMediaAssets[0];
            }

            static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
            {
                IAsset asset = _context.Assets.Create(assetName, options);

                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                assetFile.Upload(filePath);

                return asset;
            }

            static void DownloadAsset(IAsset asset, string outputDirectory)
            {
                foreach (IAssetFile file in asset.AssetFiles)
                {
                    file.Download(Path.Combine(outputDirectory, file.Name));
                }
            }

            static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors
                    .Where(p => p.Name == mediaProcessorName)
                    .ToList()
                    .OrderBy(p => new Version(p.Version))
                    .LastOrDefault();

                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor",
                                                               mediaProcessorName));

                return processor;
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

        }
    }

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Související odkazy
[Azure Media Services Analytics – přehled](media-services-analytics-overview.md)

