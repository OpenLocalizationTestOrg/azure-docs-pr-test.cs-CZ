---
title: "kódování aaaAdvanced s pracovním postupem Premium Media Encoder | Microsoft Docs"
description: "Zjistěte, jak tooencode s pracovním postupem Premium kodér médií. Ukázky kódu jsou napsané v jazyce C# a použít hello sady Media Services SDK pro .NET."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 0f4c87ac-810a-4d42-8df8-923dff2016c6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 5a1c3d019a5c8fbf9bda2da751a7eff4c4907d97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Pokročilé kódování s pracovním postupem prostředí Media Encoder Premium
> [!NOTE]
> Procesor médií Media Encoder Premium pracovního postupu popsané v tomto tématu není k dispozici v Číně.
>
>

Máte dotazy k kodér premium e-mailu mepd na webu společnosti Microsoft.

## <a name="overview"></a>Přehled
Microsoft Azure Media Services představuje hello **Media Encoder Premium pracovního postupu** procesor médií. Tato záloha nabízí procesoru kódování funkcí pro vaše pracovní postupy premium na vyžádání.

Hello následující témata popisují podrobnosti související příliš**Media Encoder Premium pracovního postupu**:

* [Podporované formáty podle hello Media Encoder Premium pracovního postupu](media-services-premium-workflow-encoder-formats.md) – popisuje formáty souborů hello a kodeky podporované systémem **Media Encoder Premium pracovního postupu**.
* [Přehled a porovnání Azure na vyžádání média kodéry](media-services-encode-asset.md) porovná hello kódování možnosti **Media Encoder Premium pracovního postupu** a **Media Encoder Standard**.

Toto téma ukazuje, jak tooencode s **Media Encoder Premium pracovního postupu** pomocí rozhraní .NET.

Úlohy pro hello kódování **Media Encoder Premium pracovního postupu** vyžadují jiný konfigurační soubor, názvem souboru pracovního postupu. Tyto soubory mít příponu .workflow a jsou vytvořeny pomocí hello [Návrhář postupu provádění](media-services-workflow-designer.md) nástroj.

Můžete také získat hello výchozí soubory pracovního postupu [zde](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Hello složka obsahuje také popis hello těchto souborů.

soubory pracovního postupu Hello potřebovat účtu Media Services tooyour toobe nahrán jako prostředek, a tento prostředek by měl být předán toohello kódování úloh.

## <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia

Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md). 

## <a name="encoding-example"></a>Příklad kódování

Hello následující příklad ukazuje, jak tooencode s **Media Encoder Premium pracovního postupu**.

jsou prováděny Hello následující kroky:

1. Vytvořte asset a nahrajte soubor pracovního postupu.
2. Vytvořte asset a nahrajte soubor zdrojového média.
3. Získáte procesor "Media Encoder Premium Workflow" médií hello.
4. Vytvoření úlohy a úlohy.

    Ve většině případů je prázdný řetězec hello konfigurace pro úlohu hello (třeba v hello následující příklad). Existují některé pokročilé scénáře (které vyžadují tootooset runtime vlastnosti dynamicky) v takovém případě by poskytnout toohello řetězec XML pro kódování úloh. Mezi příklady takových scénářů jsou: vytváření překrytí, paralelní nebo po sobě jdoucích média ve hřbetu, titulkování.
5. Přidáte úloha toohello dva vstupní prostředky.

    1. 1. – asset hello pracovního postupu.
    2. 2. – asset videa hello.

    >[!NOTE]
    >asset Hello pracovního postupu je nutné přidat úloha toohello před hello média asset.
   Hello konfigurační řetězec pro tato úloha by měla být prázdná.
   
6. Odeslání úlohy kódování hello.

        using System;
        using System.Linq;
        using System.Configuration;
        using System.IO;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;

        namespace MediaEncoderPremiumWorkflowSample
        {
            class Program
            {
                private static readonly string _AADTenantDomain =
                    ConfigurationManager.AppSettings["AADTenantDomain"];
                private static readonly string _RESTAPIEndpoint =
                    ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

                // Field for service context.
                private static CloudMediaContext _context = null;

                private static readonly string _supportFiles =
                    Path.GetFullPath(@"../..\Media");

                private static readonly string _workflowFilePath =
                    Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");

                private static readonly string _singleMP4InputFilePath =
                    Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");

                static void Main(string[] args)
                {
                    var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
                    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                    _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

                    var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
                    var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
                    IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset);

                }

                static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
                {
                    var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

                    var fileName = Path.GetFileName(singleFilePath);

                    var assetFile = asset.AssetFiles.Create(fileName);

                    Console.WriteLine("Created assetFile {0}", assetFile.Name);

                    Console.WriteLine("Upload {0}", assetFile.Name);

                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);

                    return asset;
                }

                static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Premium Workflow encoding job");
                    // Get a media processor reference, and pass tooit hello name of the
                    // processor toouse for hello specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

                    // Create a task with hello encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                        processor,
                        "",
                        TaskOptions.None);

                    // Specify hello input asset toobe encoded.
                    task.InputAssets.Add(workflow);
                    task.InputAssets.Add(video); // we add one asset
                                                 // Add an output asset toocontain hello results of hello job.
                                                 // This output is specified as AssetCreationOptions.None, which
                                                 // means hello output asset is not encrypted.
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);

                    // Use hello following event handler toocheck job progress.  
                    job.StateChanged += new
                            EventHandler<JobStateChangedEventArgs>(StateChanged);

                    // Launch hello job.
                    job.Submit();

                    // Check job execution and wait for job toofinish.
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                    progressJobTask.Wait();

                    // If job state is Error hello event handling
                    // method for job progress should log errors.  Here we check
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        throw new Exception("\nExiting method due toojob error.");
                    }

                    return job.OutputMediaAssets[0];
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

Máte dotazy k kodér premium e-mailu mepd na webu společnosti Microsoft.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
