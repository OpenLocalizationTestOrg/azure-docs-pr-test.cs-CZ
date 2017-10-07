---
title: "aaaEncode prostředek s Media Encoder Standard pomocí rozhraní .NET | Microsoft Docs"
description: "Toto téma ukazuje, jak toouse .NET tooencode assetu pomocí kodéru standardní média."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 03431b64-5518-478a-a1c2-1de345999274
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 25e274c3b67168f4afc8b8ab04af2d654c9dd6e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Kódování assetu pomocí kodéru Media Encoder Standard pomocí rozhraní .NET
Kódování úlohy jsou některé z nejběžnějších operací zpracování hello ve službě Media Services. Z jednoho kódování tooanother vytvoříte kódování úlohy tooconvert mediálních souborů. Při kódování, můžete použít hello předdefinované Media Encoder Media Services. Můžete také použít kodér poskytovanými partnerem Media Services; jsou k dispozici prostřednictvím Azure Marketplace hello kodéry třetích stran. 

Toto téma ukazuje, jak toouse .NET tooencode vaše prostředky s Media Encoder Standard (MES). Media Encoder Standard je konfigurován pomocí jedno z přednastavení kodér hello popsané [zde](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Doporučujeme tooalways zakódovat zdrojové soubory do sady souborů MP4 adaptivní přenosovou rychlostí a pak převést hello sadu toohello požadovaný formát pomocí hello [dynamické balení](media-services-dynamic-packaging-overview.md). 

Pokud výstupní asset používá šifrování úložiště, musíte nakonfigurovat zásady doručení assetu. Další informace najdete v části [konfigurace zásad doručení assetu](media-services-dotnet-configure-asset-delivery-policy.md).

> [!NOTE]
> Výstupní soubor s názvem, který obsahuje hello nejprve 32 znaků, který vytváří MES hello název vstupního souboru. Název Hello je založen na co jsou uvedeny v souboru přednastavené hello. Například "název souboru": "{Basename} _ {Index} {rozšíření}". {Basename} je nahrazena hello nejprve 32 znaků názvu hello vstupní soubor.
> 
> 

### <a name="mes-formats"></a>Formáty MES
[Formáty a kodeky](media-services-media-encoder-standard-formats.md)

### <a name="mes-presets"></a>Přednastavení MES
Media Encoder Standard je konfigurován pomocí jedno z přednastavení kodér hello popsané [zde](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Vstup a výstup metadat
Při kódování prostředek vstupní (nebo prostředky) pomocí MES, můžete získat prostředek výstup v hello kódování úspěšné dokončení této úlohy. Hello výstupní asset obsahuje video, zvuk, miniatur, manifest atd. hello předvolby kódování, které používáte.

Hello výstupní asset obsahuje také soubor s metadata o vstupní asset hello. Hello název souboru XML metadat hello má hello formátu: < asset_id > _metadata.xml (například 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), kde < asset_id > je hodnota ID hello hello vstupní asset. Hello schéma tato metadata vstupní XML je popsána [zde](media-services-input-metadata-schema.md).

Hello výstupní asset obsahuje také soubor s metadata o hello výstupní asset. Hello název souboru XML metadat hello má hello formátu: < source_file_name > _manifest.xml (například BigBuckBunny_manifest.xml). schéma Hello tato metadata výstup XML je popsána [zde](media-services-output-metadata-schema.md).

Pokud chcete, tooexamine buď ze souborů metadat hello dva, můžete vytvořit lokátor SAS a stáhnout hello souboru tooyour místního počítače. Příklad na tom, jak toocreate lokátoru SAS a stahování souboru pomocí hello Media Services můžete nalézt rozšíření sady SDK pro .NET.

## <a name="download-sample"></a>Stažení ukázky
Můžete získat a spustit ukázku, která ukazuje, jak tooencode s MES z [zde](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="net-sample-code"></a>Ukázkový kód rozhraní .NET

Následující ukázka kódu Hello používá sadu Media Services .NET SDK tooperform hello následující úlohy:

* Vytvořte úlohu kódování.
* Získání kodéru Media Encoder Standard toohello odkaz.
* Zadejte toouse hello [adaptivní datové proudy](media-services-autogen-bitrate-ladder-with-mes.md) přednastavené. 
* Přidejte jeden úlohy kódování toohello úloh. 
* Zadejte vstup hello asset toobe kódování.
* Vytvoření výstupní asset, který bude obsahovat kódovaný hello asset.
* Přidejte průběh úlohy toocheck hello událost obslužné rutiny.
* Odešlete úlohu hello.

#### <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia

Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md). 

#### <a name="example"></a>Příklad 

        using System;
        using System.Linq;
        using System.Configuration;
        using System.IO;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;

        namespace MediaEncoderStandardSample
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

                static void Main(string[] args)
                {
                    var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
                    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                    _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

                    // Get an uploaded asset.
                    var asset = _context.Assets.FirstOrDefault();

                    // Encode and generate hello output using hello "Adaptive Streaming" preset.
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

                    // Create a task with hello encoding details, using a string preset.
                    // In this case "Adaptive Streaming" preset is used.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "Adaptive Streaming",
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

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Další kroky
[Jak toogenerate miniaturu pomocí kodéru Media Encoder Standard s .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[kódování Přehled služby Media Services](media-services-encode-asset.md)

