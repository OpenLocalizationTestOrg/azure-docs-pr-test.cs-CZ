---
title: "Azure Media Encoder Standard tooauto aaaUse-generovat přenosovou rychlostí žebříku | Microsoft Docs"
description: "Toto téma ukazuje, jak toouse Media Encoder Standard (MES) tooauto-generovat žebříku přenosovou rychlostí podle hello vstupní řešení a přenosovou rychlostí. Vstupní řešení Hello a přenosovou rychlostí se nikdy překročena. Například pokud hello vstup je 720p na 3 MB/s, bude výstup v nejlépe zůstanou 720p a začne tempem nižší než 3 MB/s."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 63ed95da-1b82-44b0-b8ff-eebd535bc5c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 5437f54ac28c42ddd4f9d1986549d6da6261c5da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#  <a name="use-azure-media-encoder-standard-tooauto-generate-a-bitrate-ladder"></a>Pomocí Azure Media Encoder Standard tooauto-generovat žebříku přenosovou rychlostí

## <a name="overview"></a>Přehled

Toto téma ukazuje, jak toouse Media Encoder Standard (MES) tooauto-generovat žebříku přenosovou rychlostí (přenosovou rychlostí řešení páry) na základě hello vstupní řešení a přenosovou rychlostí. Hello automaticky generovaný přednastavených se nikdy překročit hello vstupní řešení a přenosovou rychlostí. Například pokud hello vstup je 720p na 3 MB/s, bude výstup v nejlépe zůstanou 720p a začne tempem nižší než 3 MB/s.

### <a name="encoding-for-streaming-only"></a>Kódování pro streamování pouze

Pokud vaše záměrem tooencode svůj zdroj videa pouze pro streamování, volných prostředků můžete použít "Adaptivní datové proudy" hello přednastavení při vytvoření úlohy kódování. Při použití hello **adaptivní datové proudy** předvolby, hello MES kodér bude inteligentně cap žebříku přenosovou rychlostí. Ale nebudete moct toocontrol hello kódování náklady, protože hello služby určuje, kolik toouse vrstev a v jakém rozlišení. Vidíte příklady vytvoření MES v důsledku kódování s hello vrstev výstup **adaptivní datové proudy** přednastavení na konci hello v tomto tématu. Hello výstupní Asset bude obsahovat soubory MP4, kde audio a video nejsou prokládaný.

### <a name="encoding-for-streaming-and-progressive-download"></a>Kódování pro streamování a progresivní stahování

Pokud vaše záměrem tooencode zdroj videa pro streamování a také soubory MP4 tooproduce progresivní stahování, pak volných prostředků můžete používat hello "Obsahu adaptivní více přenosovou rychlostí MP4" přednastavení při vytvoření úlohy kódování. Při použití hello **obsahu adaptivní více MP4 přenosovou rychlostí** předvolby, hello MES kodér použije hello stejné kódování logiku, jak je uvedeno výše, ale teď hello výstupní asset bude obsahovat MP4 soubory tam, kde audio a video jsou prokládaná. Jako soubor progresivní stahování můžete použít jednu z těchto souborů MP4 (například hello nejvyšší přenosovou rychlostí verze).

## <a id="encoding_with_dotnet"></a>Kódování pomocí služby Media Services .NET SDK

Následující ukázka kódu Hello používá sadu Media Services .NET SDK tooperform hello následující úlohy:

- Vytvořte úlohu kódování.
- Získání kodéru Media Encoder Standard toohello odkaz.
- Přidat úlohu kódování toohello úloh a zadejte toouse hello **adaptivní datové proudy** přednastavené. 
- Vytvoření výstupní asset, který bude obsahovat kódovaný hello asset.
- Přidejte průběh úlohy toocheck hello událost obslužné rutiny.
- Odešlete úlohu hello.

#### <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia

Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md). 

#### <a name="example"></a>Příklad

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;

    namespace AdaptiveStreamingMESPresest
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

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
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

## <a id="output"></a>Výstup

Tato část ukazuje tři příklady vytvoření MES v důsledku kódování s hello vrstev výstup **adaptivní datové proudy** přednastavené. 

### <a name="example-1"></a>Příklad 1
Zdroj s výšky "1080" a "29.970" kmitočet snímků vytváří 6 video vrstvy:

|Vrstva|Výška|Šířka|Bitrate(Kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Příklad 2
Zdroj s výšky "720" a "23.970" kmitočet snímků vytváří 5 video vrstvy:

|Vrstva|Výška|Šířka|Bitrate(Kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Příklad 3
Zdroj s výšky "360" a "29.970" kmitočet snímků vytváří vrstvy 3 videa:

|Vrstva|Výška|Šířka|Bitrate(Kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|
## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Viz také
[Kódování Přehled služby Media Services](media-services-encode-asset.md)

