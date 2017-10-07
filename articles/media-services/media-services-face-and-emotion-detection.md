---
title: aaaDetect vzhled a emoce s Azure Media Analytics | Microsoft Docs
description: "Toto téma ukazuje, jak toodetect otočená a emoce s Azure Media Analytics."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 5ca4692c-23f1-451d-9d82-cbc8bf0fd707
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/18/2017
ms.author: milanga;juliako;
ms.openlocfilehash: f58d81d82dde08a694cdb4d92c6bab6a40a9c157
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="detect-face-and-emotion-with-azure-media-analytics"></a>Zjistit vzhled a emoce s Azure Media Analytics
## <a name="overview"></a>Přehled
Hello **Azure Media vzhled detektor** procesor médií (PP) vám umožní toocount, sledovat pohybů a i zapojení cílové skupiny měřidla a reakce prostřednictvím obličeje výrazy. Tato služba obsahuje dvě funkce: 

* **Vzhled detekce**
  
    Vzhled zjišťování vyhledá a sleduje lidského tyto řezy v rámci video. Více řezy lze zjistit a následně je sledovat jako pohyb se s hello čas a umístění metadata vrácená v souboru JSON. Při sledování pokusí toogive konzistentní toohello ID stejné čelí, zatímco je osoba hello pohyb na obrazovce, přestože bráněno nebo stručně nechte hello rámce.
  
  > [!NOTE]
  > Tato služba neprovede rozpoznávání obličeje. Osoba, která zůstane hello rámce nebo se stane nelze blokovat. pro příliš dlouho přidělí nové ID když se vrátí.
  > 
  > 
* **Emocí**
  
    Emocí je volitelná součást hello vzhled detekce média procesor, který vrátí analýzy na více duševní atributů z hello řezy detekuje, včetně štěstí, sadness, obavy, anger a další. 

Hello **Azure Media vzhled detektor** MP je aktuálně ve verzi Preview.

Toto téma uvádí podrobnosti o **Azure Media vzhled detektor** a ukazuje, jak toouse ho pomocí sady Media Services SDK pro .NET.

## <a name="face-detector-input-files"></a>Čelí detektor vstupní soubory
Video soubory. V současné době jsou podporovány následující formáty hello: MP4, MOV a WMV.

## <a name="face-detector-output-files"></a>Čelí detektor výstupní soubory
Hello vzhled detekce a sledování rozhraní API poskytuje vysokou přesnost vzhled umístění zjišťování a sledování, které může zjistit, až too64 lidské řezy v video. Čelní řezy zadejte hello nejlepších výsledků při straně řezy a malé řezy (menší než nebo roven hodnotě too24x24 pixelů) nemusí být jako přesné.

Hello zjištěné a sledovaných řezy jsou vráceny pomocí souřadnic (vlevo, top, šířku a výšku) určující hello umístění řezy hello obrázku v pixelech, jakož i vzhled ID číslo označující hello to jednotlivých sledování. Čísla ID vzhled jsou náchylné k chybám tooreset okolností při čelní vzhled hello dojde ke ztrátě nebo překryté hello rámce, výsledkem některé jednotlivce získávání přiřazeno více ID.

## <a id="output_elements"></a>Elementy výstupního souboru JSON, hello

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

Detektor vzhled používá techniky fragmentace (kde hello metadata může dojít k rozdělení v založené na čase bloky a můžete si stáhnout pouze to, co potřebujete) a segmentace (kde hello události jsou k rozdělení v případě, že získají příliš velký). Některé jednoduché výpočty můžete transformovat hello data. Například, pokud událost zahájená 6300 (rysky), s časovou 2997 (rysky za sekundu) a kmitočet snímků 29,97 (snímků za sekundu), pak:

* Spuštění nebo časový rámec = 2.1 sekund
* Sekund x Framerate = 63 rámce

## <a name="face-detection-input-and-output-example"></a>Čelí detekce vstup a výstup příklad
### <a name="input-video"></a>Vstupní video
[Vstupní Video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Konfigurace úlohy (přednastavených)
Při vytváření úlohy s **Azure Media vzhled detektor**, je nutné zadat jedno z přednastavení konfigurace. Hello následující přednastavených konfigurace platí jenom pro zjišťování řez.

    {
      "version":"1.0",
      "options":{
          "TrackingMode": "Fast"
      }
    }

#### <a name="attribute-descriptions"></a>Atribut popisy
| Název atributu | Popis |
| --- | --- |
| Režim |Rychlé - se rychlé zpracování rychlostí, ale méně přesný (výchozí).|

### <a name="json-output"></a>Výstup JSON
Následující příklad výstupu JSON Hello byl zkrácen.

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

## <a name="emotion-detection-input-and-output-example"></a>Emocí vstup a výstup příklad
### <a name="input-video"></a>Vstupní video
[Vstupní Video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Konfigurace úlohy (přednastavených)
Při vytváření úlohy s **Azure Media vzhled detektor**, je nutné zadat jedno z přednastavení konfigurace. Následující konfigurace přednastavených Hello určuje toocreate JSON v závislosti na emocí hello.

    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


#### <a name="attribute-descriptions"></a>Atribut popisy
| Název atributu | Popis |
| --- | --- |
| Režim |Řezy: Pouze čelí detekce.<br/>PerFaceEmotion: Vrátí rozpoznávání emocí úrovně nezávisle pro každý řez zjišťování.<br/>AggregateEmotion: Návratový průměrná rozpoznávání emocí úrovně hodnoty pro všechny tyto řezy v rámečku. |
| AggregateEmotionWindowMs |Použijte, pokud je vybrána AggregateEmotion režimu. Určuje délku hello video použité tooproduce každý výsledek agregační v milisekundách. |
| AggregateEmotionIntervalMs |Použijte, pokud je vybrána AggregateEmotion režimu. Určuje s výsledky jaká frekvence tooproduce agregace. |

#### <a name="aggregate-defaults"></a>Agregační výchozí hodnoty
Níže jsou doporučené hodnoty pro agregační oddílové hello a nastavení intervalu. AggregateEmotionWindowMs by měl být delší než AggregateEmotionIntervalMs.

|| Výchozí hodnoty (s) | Min(s) | Max(s) |
|--- | --- | --- | --- |
| AggregateEmotionWindowMs |0.5 |2 |0.25|
| AggregateEmotionIntervalMs |0.5 |1 |0.25|

### <a name="json-output"></a>Výstup JSON
Výstup pro agregační rozpoznávání emocí úrovně (zkrácený) ve formátu JSON:

    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,

## <a name="limitations"></a>Omezení
* vstupní video formáty Hello podporována jsou MP4, MOV a WMV.
* rozsah velikost Hello rozpoznat řez je 24 x 24 too2048x2048 pixelů. Hello řezy mimo tento rozsah nebudou zjištěna.
* Pro každý video hello maximální počet řezy vrátil je 64.
* Některé řezy nemusí být detekována z důvodu problémů tootechnical; například velké vzhled úhly (head pozice) a velké NF pásmová. Čelní a téměř čelní strany mít nejlepších výsledků dosáhnete hello.

## <a name="net-sample-code"></a>Ukázkový kód rozhraní .NET

ukazuje programu Hello následující postup:

1. Vytvořte asset a nahrajte soubor média do hello asset.
2. Vytvořte úlohu s úkolem detekce vzhled podle konfigurační soubor, který obsahuje následující json přednastavených hello. 
   
        {
            "version": "1.0"
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

    namespace FaceDetection
    {
        class Program
        {
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

                // Run hello FaceDetection job.
                var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                            @"C:\supportFiles\FaceDetection\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
            }

            static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Face Detection Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Face Detection Job");

                // Get a reference tooAzure Media Face Detector.
                string MediaProcessorName = "Azure Media Face Detector";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Face Detection Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);

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

[Ukázky služby Azure Media Analytics](http://amslabs.azurewebsites.net/demos/Analytics.html)

