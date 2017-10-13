---
title: "Indexování mediálních souborů pomocí Azure Media Indexer 2 Preview | Microsoft Docs"
description: "Azure Media Indexer umožňuje zajistit obsah souborů médií s možností vyhledávání a ke generování fulltextového přepis pro skryté titulky nebo klíčová slova. Toto téma ukazuje způsob použití Media Indexer 2 Preview."
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
ms.openlocfilehash: 0afdd1c04e50215a55fb92c70b1210d1f80d8e3f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Indexování mediálních souborů pomocí Azure Media Indexer 2 Preview
## <a name="overview"></a>Přehled
**Azure Media Indexer 2 Preview** procesor médií (PP) umožňuje vytvoření mediálních souborů a obsah s možností vyhledávání, jakož i generovat uzavřené titulků sleduje. Ve srovnání s předchozí verzi [Azure Media Indexer](media-services-index-content.md), **Azure Media Indexer 2 Preview** provede rychlejší indexování a nabízí širší jazyková podpora. Mezi podporované jazyky patří angličtina, španělština, francouzština, němčina, italština, čínština (Mandarínština, zjednodušená), portugalština, Arabské a japonštině.

**Azure Media Indexer 2 Preview** MP je aktuálně ve verzi Preview.

Toto téma ukazuje, jak vytvořit indexování úlohy s **Azure Media Indexer 2 Preview**.

> [!NOTE]
> Platí následující aspekty:
> 
> Indexer 2 není podporována v Číně Azure a Azure Government.
> 
> Během indexování obsah, nezapomeňte použít mediálních souborů, které mají velmi jasně řeči (bez pozadí Hudba, šumu, efekty nebo mikrofon hiss). Některé příklady příslušný obsah: zaznamenávají schůzek, přednášek nebo prezentací. Následující obsah nemusí být vhodný pro indexování: filmy, televizní pořady cokoli s smíšený zvuk a zvukové efekty špatně zaznamenány obsah s hluku na pozadí (hiss).
> 
> 

Toto téma uvádí podrobnosti o **Azure Media Indexer 2 Preview** a ukazuje, jak pomocí sady Media Services SDK pro .NET

## <a name="input-and-output-files"></a>Vstupní a výstupní soubory
### <a name="input-files"></a>Vstupní soubory
Soubory zvuku a videa

### <a name="output-files"></a>Výstupní soubory
Úlohu indexování může generovat soubory titulků v následujících formátech:  

* **SAMI**
* **TTML**
* **WebVTT**

Popisek (kopie) soubory v těchto formátů lze Zpřístupněte soubory audia a videa pro osoby s postižením sluchu uzavřít.

## <a name="task-configuration-preset"></a>Konfigurace úlohy (přednastavených)
Při vytváření indexovat úloh s **Azure Media Indexer 2 Preview**, je nutné zadat jedno z přednastavení konfigurace.

Následujícím kódu JSON nastaví dostupné parametry.

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
Azure Media Indexer 2 ve verzi Preview podporuje řeči na text pro následující jazyky (při zadávání názvu jazyka v konfiguraci úloh, použijte znak 4 kódu v závorkách, jak je uvedeno níže):

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

Informace o typech souborů podporovaných najdete v tématu [podporované kodeky/formáty](media-services-media-encoder-standard-formats.md#input-containerfile-formats) části.

## <a name="net-sample-code"></a>Ukázkový kód rozhraní .NET

Program zobrazí následující postup:

1. Vytvořte asset a nahrajte soubor média do assetu.
2. Vytvořte úlohu indexování úlohy podle konfigurační soubor, který obsahuje následující přednastavení json.
   
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
3. Stáhněte výstupní soubory. 
   
#### <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia

Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md). 

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
            // Read values from the App.config file.
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

                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
            }

            static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Indexing Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Indexing Job");

                // Get a reference to Azure Media Indexer 2 Preview.
                string MediaProcessorName = "Azure Media Indexer 2 Preview";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Indexing Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify the input asset to be indexed.
                task.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

                // Use the following event handler to check job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

                // Launch the job.
                job.Submit();

                // Check job execution and wait for job to finish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);

                progressJobTask.Wait();

                // If job state is Error, the event handling
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

