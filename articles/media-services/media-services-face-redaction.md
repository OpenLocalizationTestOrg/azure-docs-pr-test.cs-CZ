---
title: "aaaRedact řezy s Azure Media Analytics | Microsoft Docs"
description: "Toto téma ukazuje, jak tooredact otočená s Azure media analytics."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 5b6d8b8c-5f4d-4fef-b3d6-dc22c6b5a0f5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako;
ms.openlocfilehash: 1f5688a8c6374151c526a9c702b904d8c3e46164
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="redact-faces-with-azure-media-analytics"></a>Redigovat řezy s Azure Media Analytics
## <a name="overview"></a>Přehled
**Azure Media Redactor** je [Azure Media Analytics](media-services-analytics-overview.md) procesor médií (PP), která nabízí redigování škálovatelné řez v cloudu hello. Vzhled redigování umožňuje vám toomodify videa v pořadí tooblur řezy vybrané osob. Můžete chtít toouse hello vzhled redigování služby ve veřejné scénáře zabezpečení a média zprávy. Pár minut záznamů, která obsahuje více řezy může trvat hodiny tooredact ručně, ale s této služby hello vzhled redigování proces bude vyžadovat několika jednoduchých kroků. Další informace najdete v tématu [to](https://azure.microsoft.com/blog/azure-media-redactor/) blogu.

Toto téma uvádí podrobnosti o **Azure Media Redactor** a ukazuje, jak toouse ho pomocí sady Media Services SDK pro .NET.

Hello **Azure Media Redactor** MP je aktuálně ve verzi Preview. Je k dispozici ve všech veřejných oblastí Azure a také datových centrech US Government a Číně. Tato verze preview je aktuálně zdarma. 

## <a name="face-redaction-modes"></a>Vzhled redigování režimy
Funguje obličeje redigování zjišťuje tyto řezy v každém snímku videa a sledování hello vzhled objektu obě dopředný a zpětně v čase, tak, aby hello stejné individuální může být hranice z jiných úhly také. Hello automatizované redigování proces je velmi složitý a vždy nevytváří 100 % požadované výstup z tohoto důvodu, které jste s několika způsoby Media Analytics poskytuje toomodify hello závěrečný výstup.

V režimu plně automatické přidání tooa je dva průchodu pracovního postupu, které umožňuje hello výběr/deaktivuje-selection nalezen ploch prostřednictvím seznam identifikátorů. Toomake libovolný za rámce úpravy hello MP také používá soubor metadat ve formátu JSON. Tento pracovní postup je rozdělená do **analyzovat** a **Redact** režimy. Zkombinováním hello dva režimy při jednom průchodu používající obě úlohy v úloh. Tento režim je volána **kombinované**.

### <a name="combined-mode"></a>Kombinovaná režimu
Vznikne tak zredigované mp4 automaticky bez jakékoli ruční vstup.

| Krok | Název souboru | Poznámky |
| --- | --- | --- |
| Vstupní prostředek |foo.bar |Video ve formátu WMV, MOV nebo MP4 |
| Vstupní konfigurace |Předvolby úlohy konfigurace |{'version':'1.0 ', 'možnosti': {"režim": "kombinovanou"}} |
| Výstupní asset |foo_redacted.MP4 |Video s stírá použít |

#### <a name="input-example"></a>Vstupní příklad:
[přehrát toto video](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

#### <a name="output-example"></a>Příklad výstupu:
[přehrát toto video](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

### <a name="analyze-mode"></a>Analýza režimu
Hello **analyzovat** průchodu hello dva průchodu pracovního postupu přebírá vstup videa a vytvoří soubor JSON místa vzhled a obrázků jpg jednotlivých zjistila řez.

| Krok | Název souboru | Poznámky |
| --- | --- | --- |
| Vstupní prostředek |foo.bar |Video ve formátu WMV, MPV nebo MP4 |
| Vstupní konfigurace |Předvolby úlohy konfigurace |{'version':'1.0 ', 'možnosti': {'režimu': 'analyzovat.}} |
| Výstupní asset |foo_annotations.JSON |Poznámky data vzhled umístění ve formátu JSON. To se dá upravit pomocí hello uživatele toomodify hello stírá ohraničujícího polí. Viz následující ukázka. |
| Výstupní asset |foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg] |Oříznutý jpg jednotlivých zjistil vzhled, kde hello číslo označuje hello labelId řezu hello |

#### <a name="output-example"></a>Příklad výstupu:

    {
      "version": 1,
      "timescale": 24000,
      "offset": 0,
      "framerate": 23.976,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 48048,
          "interval": 1001,
          "events": [
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [
              {
                "index": 13,
                "id": 1138,
                "x": 0.29537,
                "y": -0.18987,
                "width": 0.36239,
                "height": 0.80335
              },
              {
                "index": 13,
                "id": 2028,
                "x": 0.60427,
                "y": 0.16098,
                "width": 0.26958,
                "height": 0.57943
              }
            ],

    … truncated

### <a name="redact-mode"></a>Redigovat režimu
Hello druhého průchodu hello pracovního postupu přebírá větší počet vstupních hodnot, které musí zkombinovat do jednoho datového zdroje.

To zahrnuje seznam ID tooblur hello původní video a poznámky hello JSON. Tento režim používá hello poznámky tooapply stírá na vstupní video hello.

výstup hello analyzovat průchodu Hello nezahrnuje původní video hello. Hello video musí toobe nahraje do vstupní asset hello hello Redact režimu úlohy a vybrat jako primární soubor hello.

| Krok | Název souboru | Poznámky |
| --- | --- | --- |
| Vstupní prostředek |foo.bar |Video ve formátu WMV, MPV nebo MP4. Stejné jako v kroku 1 videa. |
| Vstupní prostředek |foo_annotations.JSON |Soubor metadat poznámky z první fázi, pomocí volitelné úpravy. |
| Vstupní prostředek |foo_IDList.txt (volitelné) |Volitelné nový řádek oddělený seznam ID tooredact řez. Pokud necháte prázdnou, tato rozostří všechny řezy. |
| Vstupní konfigurace |Předvolby úlohy konfigurace |{'version':'1.0 ', 'možnosti': {'režimu': 'redigovat.}} |
| Výstupní asset |foo_redacted.MP4 |Video s stírá použít podle poznámky |

#### <a name="example-output"></a>Příklad výstupu
Toto je výstup hello IDList s jedno ID vybrané.

[přehrát toto video](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

Příklad foo_IDList.txt
 
     1
     2
     3

## <a name="blur-types"></a>Rozostření typy

V hello **kombinované** nebo **Redact** režimu, existují 5 rozostření různé režimy můžete vybrat z prostřednictvím hello JSON vstupu konfigurace: **nízká**, **Med**, **Vysokou**, **ladění**, a **černé**. Ve výchozím nastavení **Med** se používá.

Můžete najít, že ukázky hello rozostření typy níže.

### <a name="example-json"></a>Příklad JSON:

    {'version':'1.0', 'options': {'Mode': 'Combined', 'BlurType': 'High'}}

#### <a name="low"></a>Nízký

![Nízký](./media/media-services-face-redaction/blur1.png)
 
#### <a name="med"></a>Med

![Med](./media/media-services-face-redaction/blur2.png)

#### <a name="high"></a>Vysoký

![Vysoký](./media/media-services-face-redaction/blur3.png)

#### <a name="debug"></a>Ladění

![Ladění](./media/media-services-face-redaction/blur4.png)

#### <a name="black"></a>Černá

![Černá](./media/media-services-face-redaction/blur5.png)

## <a name="elements-of-hello-output-json-file"></a>Elementy výstupního souboru JSON, hello

Hello redigování MP poskytuje vysokou přesnost vzhled umístění zjišťování a sledování, které může zjistit, až too64 lidské řezy v snímek videa. Čelní řezy zadejte hello nejlepších výsledků při straně řezy a malé řezy (menší než nebo roven hodnotě too24x24 pixelů) jsou náročné.

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

## <a name="net-sample-code"></a>Ukázkový kód rozhraní .NET

ukazuje programu Hello následující postup:

1. Vytvořte asset a nahrajte soubor média do hello asset.
2. Vytvořte úlohu s úkolem redigování vzhled podle konfigurační soubor, který obsahuje následující json přednastavených hello. 
   
        {'version':'1.0', 'options': {'mode':'combined'}}
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

    namespace FaceRedaction
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

            // Run hello FaceRedaction job.
            var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                        @"C:\supportFiles\FaceRedaction\config.json");

            // Download hello job output asset.
            DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
        }

        static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload hello input media file toostorage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Face Redaction Input Asset",
            AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Face Redaction Job");

            // Get a reference tooAzure Media Redactor.
            string MediaProcessorName = "Azure Media Redactor";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from hello specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with hello encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Face Redaction Task",
            processor,
            configuration,
            TaskOptions.None);

            // Specify hello input asset.
            task.InputAssets.Add(asset);

            // Add an output asset toocontain hello results of hello job.
            task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);

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

## <a name="next-steps"></a>Další kroky

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Související odkazy
[Azure Media Services Analytics – přehled](media-services-analytics-overview.md)

[Ukázky služby Azure Media Analytics](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

