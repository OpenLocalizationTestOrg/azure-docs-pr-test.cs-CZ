---
title: "aaaIndexing mediálních souborů pomocí Azure Media Indexer 2 Preview | Microsoft Docs"
description: "Azure Media Indexer vám umožní toomake obsah souborů médií s možností vyhledávání a toogenerate fulltextové přepis pro a klíčová slova. Toto téma ukazuje, jak zobrazit náhled toouse Media Indexer 2."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 85d25525-a498-44eb-ae3a-2ca5ceb8e53d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: adsolank;juliako;
ms.openlocfilehash: f83fa0db58b828ffa29933d68ce108b4906dcd78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Indexování mediálních souborů pomocí Azure Media Indexer 2 Preview
## <a name="overview"></a>Přehled
Hello **Azure Media Indexer 2 Preview** procesor médií (PP) vám umožní toomake mediálních souborů a obsah s možností vyhledávání, a také generování uzavřené titulků sleduje. Porovnání toohello předchozí verze [Azure Media Indexer](media-services-index-content.md), **Azure Media Indexer 2 Preview** provede rychlejší indexování a nabízí širší jazyková podpora. Mezi podporované jazyky patří angličtina, španělština, francouzština, němčina, italština, čínština (Mandarínština, zjednodušená), portugalština, Arabské a japonštině.

Hello **Azure Media Indexer 2 Preview** MP je aktuálně ve verzi Preview.

Toto téma ukazuje, jak toocreate indexování úlohy s **Azure Media Indexer 2 Preview**.

> [!NOTE]
> použít Hello následující aspekty:
> 
> Indexer 2 není podporována v Číně Azure a Azure Government.
> 
> Během indexování obsahu, ujistěte se, že toouse mediálních souborů, které mají velmi jasně řeči (bez pozadí Hudba, šumu, efekty nebo mikrofon hiss). Některé příklady příslušný obsah: zaznamenávají schůzek, přednášek nebo prezentací. Hello následující obsah nemusí být vhodný pro indexování: filmy, televizní pořady cokoli s smíšený zvuk a zvukové efekty špatně zaznamenány obsah s hluku na pozadí (hiss).
> 
> 

Toto téma uvádí podrobnosti o **Azure Media Indexer 2 Preview** a ukazuje, jak toouse ho pomocí sady Media Services SDK pro .NET

## <a name="input-and-output-files"></a>Vstupní a výstupní soubory
### <a name="input-files"></a>Vstupní soubory
Soubory zvuku a videa

### <a name="output-files"></a>Výstupní soubory
Indexování úlohy mohou vytvářet soubory titulků v hello následujících formátů:  

* **SAMI**
* **TTML**
* **WebVTT**

Uzavřené popisek (kopie) soubory v těchto formátů lze použít toomake zvuk a video soubory přístupné toopeople s postižení sluchu.

## <a name="task-configuration-preset"></a>Konfigurace úlohy (přednastavených)
Při vytváření indexovat úloh s **Azure Media Indexer 2 Preview**, je nutné zadat jedno z přednastavení konfigurace.

Hello následujícím kódu JSON nastaví dostupné parametry.

    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }

## <a name="supported-languages"></a>Podporované jazyky
Azure Media Indexer 2 ve verzi Preview podporuje řeči na text pro následující jazyky (při zadávání název jazyka hello do hello konfiguraci úloh, použijte znak 4 kódu v závorkách, jak je uvedeno níže) hello:

* Angličtina [EnUs]
* Španělština [EsEs]
* Čínština (Mandarínština, zjednodušené) [ZhCn]
* Francouzština [FrFr]
* Němčina [DeDe]
* Italština [ItIt]
* Portugalština [PtBr]
* Arabština (egyptského) [ArEg]
* Japonština [JaJp]
* Ruština [RuRu]
* Britské angličtina [EnGb]
* Španělština mexickými [EsMx] 

## <a name="supported-file-types"></a>Podporované typy souborů

Informace o typech souborů podporovaných najdete v tématu hello [podporované kodeky/formáty](media-services-media-encoder-standard-formats.md#input-containerfile-formats) části.

## <a name="net-sample-code"></a>Ukázkový kód rozhraní .NET

ukazuje programu Hello následující postup:

1. Vytvořte asset a nahrajte soubor média do hello asset.
2. Vytvořte úlohu indexování úlohy podle konfigurační soubor, který obsahuje následující json přednastavených hello.
   
        {
          "version":"1.0",
          "Features":
            [
               {
               "Options": {
                    "Formats":["WebVtt","ttml"],
                    "Language":"enUs",
                    "Type":"RecoOptions"
               },
               "Type":"SpReco"
            }]
        }
3. Stáhněte soubory výstup hello. 
   
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

    namespace IndexContent
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

                // Run indexing job.
                var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                            @"C:\supportFiles\Indexer\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
            }

            static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Indexing Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Indexing Job");

                // Get a reference tooAzure Media Indexer 2 Preview.
                string MediaProcessorName = "Azure Media Indexer 2 Preview";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Indexing Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset toobe indexed.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

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

[Ukázky služby Azure Media Analytics](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

