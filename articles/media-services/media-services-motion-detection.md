---
title: aaaDetect pohyby s Azure Media Analytics | Microsoft Docs
description: "Hello detektor pohybu médií Azure media procesoru (PP) umožňuje tooefficiently můžete identifikovat části týkající se v rámci souboru jinak dlouhé a bezproblémové video."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: d144f813-1a55-442f-a895-5c4cb6d0aeae
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: milanga;juliako;
ms.openlocfilehash: cb431375c92222053ed2239dd4e45767524dab68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="detect-motions-with-azure-media-analytics"></a>Zjištění pohyby s Azure Media Analytics
## <a name="overview"></a>Přehled
Hello **detektor pohybu médií Azure** média procesoru (PP) umožňuje tooefficiently můžete identifikovat části týkající se v rámci souboru jinak dlouhé a bezproblémové video. Detekce pohybu lze použít na statické fotoaparát záznamů tooidentify části hello videa kde dojde k pohybu. Vygeneruje soubor JSON obsahující metadata s časová razítka a hello ohraničujícího oblasti, kde došlo k události hello.

Cílem směrem zabezpečení video informační kanály, tato technologie je možné toocategorize pohybu do příslušné události a falešně pozitivních například stínů a osvětlení změny. To umožňuje toogenerate výstrahy zabezpečení z fotoaparátu kanály bez nevyžádané pošty s nekonečná důležité události, aniž by byly situacích možné tooextract zájmu z extrémně dlouhé sledováním videa.

Hello **detektor pohybu médií Azure** MP je aktuálně ve verzi Preview.

Toto téma uvádí podrobnosti o **detektor pohybu médií Azure** a ukazuje, jak toouse ho pomocí sady Media Services SDK pro .NET

## <a name="motion-detector-input-files"></a>Vstupní soubory detekce pohybu
Video soubory. V současné době jsou podporovány následující formáty hello: MP4, MOV a WMV.

## <a name="task-configuration-preset"></a>Konfigurace úlohy (přednastavených)
Při vytváření úlohy s **detektor pohybu médií Azure**, je nutné zadat jedno z přednastavení konfigurace. 

### <a name="parameters"></a>Parametry
Můžete použít hello následující parametry:

| Name (Název) | Možnosti | Popis | Výchozí |
| --- | --- | --- | --- |
| sensitivityLevel |Řetězce: "nízká", 'střední', 'vysoká. |Nastaví úroveň hello velkých a malých písmen na které pohyby se použije v hlášení. Upravte tuto tooadjust množství falešně pozitivních zjištění. |"střední" |
| frameSamplingValue |Kladné celé číslo |Nastaví frekvenci hello na kterých běží algoritmus. každý snímek se rovná 1, 2 znamená každé 2 rámce a tak dále. |1 |
| detectLightChange |Logická hodnota: "PRAVDA", "Nepravda" |Nastaví, zda světla změny jsou hlášeny ve výsledcích hello |"Nepravda" |
| mergeTimeThreshold |Čas xs: Hh: mm:<br/>Příklad: 00:00:03 |Určuje časový interval hello mezi pohybu událostí, kde bude 2 události kombinaci a nahlášena jako 1. |00:00:00 |
| detectionZones |Pole detekce zóny:<br/>-Detekce zóny je pole 3 nebo více bodů<br/>– Bod je x a y souřadnice z 0 too1. |Popisuje hello seznam detekce polygonálních zón toobe použít.<br/>Výsledky se ohlásí hello zón jako ID, s hello první z nich vrácení 'id': 0 |Jedné oblasti, které zahrnuje celou rámce hello. |

### <a name="json-example"></a>Příklad JSON
    {
      "version": "1.0",
      "options": {
        "sensitivityLevel": "medium",
        "frameSamplingValue": 1,
        "detectLightChange": "False",
        "mergeTimeThreshold":
        "00:00:02",
        "detectionZones": [
          [
            {"x": 0, "y": 0},
            {"x": 0.5, "y": 0},
            {"x": 0, "y": 1}
           ],
          [
            {"x": 0.3, "y": 0.3},
            {"x": 0.55, "y": 0.3},
            {"x": 0.8, "y": 0.3},
            {"x": 0.8, "y": 0.55},
            {"x": 0.8, "y": 0.8},
            {"x": 0.55, "y": 0.8},
            {"x": 0.3, "y": 0.8},
            {"x": 0.3, "y": 0.55}
          ]
        ]
      }
    }


## <a name="motion-detector-output-files"></a>Pohybu detektor výstupní soubory
Úlohu detekce pohybu vrátí soubor JSON v hello výstupní asset, který popisuje hello pohybu výstrahy a jejich kategorie, v rámci hello video. Hello soubor bude obsahovat informace o hello čas a dobu trvání pohybu v hello video zjištěn.

Hello pohybu detektor API poskytuje indikátory po nejsou objekty v pohybu v pevné pozadí videa (například sledováním video). Hello detektor pohybu je vyškolení tooreduce falešné výstrahy, jako je například osvětlení a stínové změny. Aktuální omezení algoritmů hello zahrnují noci vize videa, poloprůhledné objekty a malé objekty.

### <a id="output_elements"></a>Elementy výstupního souboru JSON, hello
> [!NOTE]
> V nejnovější verzi hello formátu JSON výstup hello se změnil a pro někteří zákazníci mohou představovat narušující změně.
> 
> 

Hello následující tabulka popisuje prvky souboru JSON výstup hello.

| Element | Popis |
| --- | --- |
| Verze |Vztahuje se toohello verzi hello Video rozhraní API. aktuální verze Hello je 2. |
| Časová osa |"Rysky" za sekundu hello videa. |
| Posun |Posun Hello čas pro časová razítka v "rysky". Ve verzi 1.0 rozhraní API, Video bude vždy 0. V budoucích scénáře, které podporujeme, tato hodnota může změnit. |
| kmitočet snímků |Počet snímků za sekundu hello videa. |
| Šířka, Výška |Odkazuje toohello šířka a výška hello videa v pixelech. |
| Start |Hello spustit časové razítko v "rysky". |
| Doba trvání |Délka Hello hello událost v "rysky". |
| Interval |interval Hello každou položku v hello událost v "rysky". |
| Události |Každý fragment událostí obsahuje hello pohybu během této doby trvání. |
| Typ |V aktuální verzi hello je to vždy (2) pro obecné pohybu. Tento popisek poskytuje rozhraní API Video hello flexibilitu toocategorize pohybu v budoucích verzích. |
| RegionID |Jak je popsáno výše, bude vždy 0 v této verzi. Tento popisek poskytuje rozhraní API Video hello flexibilitu toofind pohybu v různých oblastech v budoucích verzích. |
| Oblasti |Odkazuje toohello oblasti videa, kde se zajímáte o pohybu. <br/><br/>-"id" představuje hello oblast oblasti – v této verzi je jen jeden, ID 0. <br/>-"typ" představuje hello tvar hello oblast, která se zajímáte o pro pohybu. V současné době jsou podporovány "Obdélník" a "mnohoúhelníku".<br/> Pokud jste zadali "Obdélník", hello oblast má dimenzí v X, Y, šířka a výška. Hello X a Y souřadnice představují hello levém horním XY souřadnice oblasti hello v normalizovaný měřítkem 0.0 too1.0. Hello šířky a výšky představují hello velikost hello oblast v normalizovaný měřítkem 0.0 too1.0. V aktuální verzi hello X, Y, šířka a výška vždy stanoví na 0, 0 a 1, 1. <br/>Pokud jste zadali "mnohoúhelníku", má oblast hello dimenzí v bodech. <br/> |
| fragmenty |Hello metadata je blokové až do různých segmentů názvem fragmenty. Každý fragment obsahuje počáteční, doba trvání, číslo intervalu a událostí. Fragment s žádné události znamená, že žádné pohybu byl nalezen během této počáteční čas a dobu trvání. |
| Hranaté závorky] |Každá závorka představuje jeden interval hello události. Byla zjištěna prázdný závorky pro tento interval znamená, že žádné pohybu. |
| Umístění |Tento nový záznam v části události uvádí hello umístění, kde došlo k pohybu hello. Toto je konkrétnější než hello detekce zóny. |

Následující Hello je příklad výstupu JSON

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],

    …
## <a name="limitations"></a>Omezení
* vstupní video formáty Hello podporována jsou MP4, MOV a WMV.
* Detekce pohybu je optimalizovaná pro stojící pozadí videa. algoritmus Hello se zaměřuje na snížení falešné výstrahy, jako je například změny osvětlení a stínů.
* Některé pohybu nemusí být detekována z důvodu problémů tootechnical; například noci vize videa, poloprůhledné objekty a malé objekty.

## <a name="net-sample-code"></a>Ukázkový kód rozhraní .NET

ukazuje programu Hello následující postup:

1. Vytvořte asset a nahrajte soubor média do hello asset.
2. Vytvořte úlohu s úkolem detekce pohybu na videu podle konfigurační soubor, který obsahuje následující json přednastavených hello. 
   
        {
          "Version": "1.0",
          "Options": {
            "SensitivityLevel": "medium",
            "FrameSamplingValue": 1,
            "DetectLightChange": "False",
            "MergeTimeThreshold":
            "00:00:02",
            "DetectionZones": [
              [
                {"x": 0, "y": 0},
                {"x": 0.5, "y": 0},
                {"x": 0, "y": 1}
               ],
              [
                {"x": 0.3, "y": 0.3},
                {"x": 0.55, "y": 0.3},
                {"x": 0.8, "y": 0.3},
                {"x": 0.8, "y": 0.55},
                {"x": 0.8, "y": 0.8},
                {"x": 0.55, "y": 0.8},
                {"x": 0.3, "y": 0.8},
                {"x": 0.3, "y": 0.55}
              ]
            ]
          }
        }
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

    namespace VideoMotionDetection
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

                // Run hello VideoMotionDetection job.
                var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoMotionDetection\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
            }

            static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Motion Detection Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Motion Detection Job");

                // Get a reference tooAzure Media Motion Detector.
                string MediaProcessorName = "Azure Media Motion Detector";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);

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
[Blog Azure detektor pohybu Media Services](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Media Services Analytics – přehled](media-services-analytics-overview.md)

[Ukázky služby Azure Media Analytics](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

