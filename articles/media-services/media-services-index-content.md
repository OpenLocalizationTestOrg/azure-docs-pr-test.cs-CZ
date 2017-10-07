---
title: "aaaIndexing mediálních souborů pomocí Azure Media Indexer"
description: "Azure Media Indexer vám umožní toomake obsah souborů médií s možností vyhledávání a toogenerate fulltextové přepis pro a klíčová slova. Toto téma ukazuje, jak toouse Media Indexer."
services: media-services
documentationcenter: 
author: Asolanki
manager: cfowler
editor: 
ms.assetid: 827a56b2-58a5-4044-8d5c-3e5356488271
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/20/2017
ms.author: adsolank;juliako;johndeu
ms.openlocfilehash: c1bed774e302e61ca3510668645dc2015b434a0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-media-files-with-azure-media-indexer"></a>Indexování mediálních souborů pomocí Azure Media Indexer
Azure Media Indexer vám umožní toomake obsah souborů médií s možností vyhledávání a toogenerate fulltextové přepis pro a klíčová slova. Mediální soubory můžete zpracovávat po jednom nebo v dávkách.  

> [!IMPORTANT]
> Během indexování obsahu, ujistěte se, že toouse mediálních souborů, které mají velmi jasně řeči (bez pozadí Hudba, šumu, efekty nebo mikrofon hiss). Některé příklady příslušný obsah: zaznamenávají schůzek, přednášek nebo prezentací. Hello následující obsah nemusí být vhodný pro indexování: filmy, televizní pořady cokoli s smíšený zvuk a zvukové efekty špatně zaznamenány obsah s hluku na pozadí (hiss).
> 
> 

Úlohu indexování může generovat následující výstupy hello:

* Uzavřený titulek soubory v hello následující formáty: **SAMI**, **TTML**, a **WebVTT**.
  
    Soubory titulků zahrnují značku Recognizability, která skóre indexování úlohy podle jak rozpoznatelném hello řeči v video zdroj hello je volána.  Hodnota hello Recognizability tooscreen výstupní soubory můžete použít pro použitelnost. Nízké skóre bude znamenat nedostatečný indexování výsledky z důvodu tooaudio kvality.
* Soubor – klíčové slovo (XML).
* Zvuk indexování soubor blob (AIB) pro použití se systémem SQL server.
  
    Další informace najdete v tématu [pomocí AIB souborů pomocí Azure Media Indexer a SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).

Toto téma ukazuje, jak toocreate indexování úlohy příliš**indexu prostředek** a **indexu více souborů**.

Hello nejnovější aktualizace Azure Media Indexer, najdete v části [Media Services blogy](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Pomocí souborů manifestu a konfigurace pro indexování úlohy
Můžete zadat další podrobnosti pro indexování úkoly pomocí úlohy konfigurace. Například můžete zadat které toouse metadat pro váš soubor média. Tato metadata používají hello jazyk modul tooexpand jeho termínů a výrazně zvyšuje přesnost rozpoznávání řeči hello.  Můžete je také možné toospecify požadovaný výstupní soubory.

Můžete také zpracovat více mediálních souborů najednou pomocí souboru manifestu.

Další informace najdete v tématu [přednastavení úloh pro Azure Media Indexer](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Index prostředek
Hello následující metodu odešle soubor média jako prostředek a vytvoří prostředek hello tooindex úlohy.

Všimněte si, že pokud není zadán žádný konfigurační soubor, soubor média hello indexování s všechna výchozí nastavení.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload hello input media file toostorage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference toohello Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

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
            Console.WriteLine("Exiting method due toojob error.");
            return false;
        }

        // Download hello job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
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
<!-- __ -->
### <a id="output_files"></a>Výstupní soubory
Ve výchozím nastavení generuje úlohu indexování hello následující výstupní soubory. v první výstupní asset hello budou uložené soubory Hello.

Když je více než jeden soubor vstupními médii, Indexer vygeneruje soubor manifestu pro výstupy úlohy hello s názvem 'JobResult.txt'. Pro každý soubor vstupními médii hello výsledné AIB, SAMI, TTML, WebVTT a soubory – klíčové slovo, jsou postupně číslované a s názvem pomocí hello "Alias".

| Název souboru | Popis |
| --- | --- |
| **InputFileName.aib** |Zvukový soubor indexování objektů blob. <br/><br/> Zvukový soubor indexování Blob (AIB) je binární soubor, který lze vyhledat v systému Microsoft SQL server pomocí fulltextové vyhledávání.  soubor AIB Hello je mnohem snazší než hello jednoduché titulek soubory, protože obsahuje alternativami pro jednotlivých slov, což mnohem širší možnosti vyhledávání. <br/> <br/>Vyžaduje instalaci hello aplikace hello rozšíření Indexer SQL na počítači spuštěné služby Microsoft SQL server 2008 nebo novější. Hledání hello AIB pomocí nástroje Microsoft SQL server fulltextové vyhledávání poskytuje než hledání hello uzavřený titulek soubory generované WAMI přesnější výsledky hledání. To je proto hello AIB obsahuje slovo alternativy, které zvuk podobné, zatímco hello uzavřený titulek soubory obsahují hello nejvyšší slovo spolehlivosti pro každý segment zvuk hello. Pokud je hledání mluvené slovo upmost důležitost, doporučujeme toouse hello AIB ve spojení s Microsoft SQL Server.<br/><br/> toodownload hello rozšíření, klikněte na tlačítko <a href="http://aka.ms/indexersql">Azure Media Indexer SQL rozšíření</a>. <br/><br/>Je rovněž možné tooutilize jiných vyhledávacích webů, jako je Apache Lucene/Solr toosimply index hello videa na základě hello uzavřený popisek a – klíčové slovo soubory XML, ale to bude mít za následek méně přesné výsledky hledání. |
| **InputFileName.smi**<br/>**InputFileName.ttml**<br/>**InputFileName.vtt** |Uzavřené soubory popisek (kopie) v SAMI, TTML a WebVTT formáty.<br/><br/>Můžou být použité toomake zvuk a video soubory přístupné toopeople s postižení sluchu.<br/><br/>Uzavřené titulek soubory zahrnují značku <b>Recognizability</b> která skóre úlohu indexování podle jak rozpoznatelném hello řeči v video zdroj hello je.  Můžete použít hodnotu hello <b>Recognizability</b> tooscreen výstupní soubory pro použitelnost. Nízké skóre bude znamenat nedostatečný indexování výsledky z důvodu tooaudio kvality. |
| **InputFileName.kw.xml<br/>InputFileName.info** |– Klíčové slovo a informace o soubory. <br/><br/>Soubor – klíčové slovo je soubor XML, který obsahuje klíčová slova extrahuje z obsahu hello rozpoznávání řeči, četnost a informace o posunu. <br/><br/>Informace o soubor je soubor ve formátu prostého textu, který obsahuje podrobné informace o jednotlivých termín rozpoznána. první řádek Hello je speciální a obsahuje hello Recognizability skóre. Každý další řádek je karta oddělený seznam hello následující data: spuštění doba, čas ukončení, slovo nebo frázi, spolehlivosti. Hello doby jsou uvedeny v sekundách a spolehlivosti hello je zadána jako číslo od 0-1. <br/><br/>Příklad řádku: "word 1,20 1,45 0.67" <br/><br/>Tyto soubory lze použít pro z mnoha důvodů, jako jsou například, tooperform řeči analýzy, nebo zveřejněné toosearch motorů například Bing, Google nebo Microsoft SharePoint toomake hello soubory médií zjistitelná, nebo i použité toodeliver relevantnější reklamy. |
| **JobResult.txt** |Výstup manifestu, pouze při indexování více souborů, který obsahuje hello následující informace:<br/><br/><table border="1"><tr><th>InputFile</th><th>Alias</th><th>MediaLength</th><th>Chyba</th></tr><tr><td>a.MP4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.MP4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.MP4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/> |

Není-li všechny vstupní mediální soubory jsou úspěšně indexovaná, úlohy indexování hello selže s kódem chyby 4000. Další informace najdete v tématu [kódy chyb](#error_codes).

## <a name="index-multiple-files"></a>Index více souborů
Hello následující metoda odešle více souborů médií jako prostředek a vytvoří úlohu tooindex všechny tyto soubory v dávce.

Soubor manifestu s příponou .lst hello je vytvořený a nahrává do hello asset. Soubor manifestu Hello obsahuje hello seznam všech souborů asset hello. Další informace najdete v tématu [přednastavení úloh pro Azure Media Indexer](https://msdn.microsoft.com/library/dn783454.aspx).

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload toostorage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all hello asset file names and upload toostorage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference toohello Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with hello encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify hello input asset toobe indexed.
        task.InputAssets.Add(asset);

        // Add an output asset toocontain hello results of hello job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

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
            Console.WriteLine("Exiting method due toojob error.");
            return false;
        }

        // Download hello job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Částečně úspěšně úlohy
Není-li všechny vstupní mediální soubory jsou úspěšně indexovaná, úlohy indexování hello selže s kódem chyby 4000. Další informace najdete v tématu [kódy chyb](#error_codes).

Hello stejné výstupy (jako úspěšně úloh) jsou generovány. Je možné odkazovat toohello výstupní soubor manifestu toofind, které položky se nezdařilo vstupní soubory, podle toohello hodnoty ve sloupcích chyby. Pro vstupní soubory, které se nezdařily hello výsledné AIB, SAMI, TTML, WebVTT a soubory – klíčové slovo není vygeneruje.

### <a id="preset"></a>Přednastavení úloh pro Azure Media Indexer
Hello zpracování z Azure Media Indexer lze přizpůsobit zadáním nepovinná úloha přednastavení spolu s hello úloh.  Hello následující text popisuje hello formát tento konfigurační soubor xml.

| Name (Název) | Vyžadovat | Popis |
| --- | --- | --- |
| **vstup** |False |Asset soubory, které chcete tooindex.</p><p>Azure Media Indexer podporuje následující formáty souborů médií hello: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>Můžete zadat název souboru hello (s) v hello **název** nebo **seznamu** atribut hello **vstupní** – element (jak je znázorněno níže). Pokud nezadáte, které tooindex souboru asset, primární soubor hello vydán. Pokud není nastaven žádný soubor primární asset, je indexovaný hello první soubor v hello vstupní asset.</p><p>tooexplicitly zadejte název souboru hello asset, proveďte:<br/>`<input name="TestFile.wmv">`<br/><br/>Můžete také indexu více souborů asset najednou (až too10 soubory). toodo toto:<br/><br/><ol class="ordered"><li><p>Vytvořte textový soubor (soubor manifestu) a dejte mu .lst rozšíření. </p></li><li><p>Přidejte seznam všech názvů souborů asset hello v souboru manifestu toothis vstupní asset. </p></li><li><p>Přidáte prostředek toohello souboru thanifest (nahrávání).  </p></li><li><p>Zadejte název hello souboru manifestu hello v atributu seznamu hello vstup.<br/>`<input list="input.lst">`</li></ol><br/><br/>Poznámka: Pokud chcete přidat víc než 10 souboru manifestu toohello soubory, hello indexování úlohy selže s kódem chyby 2006 hello. |
| **metadata** |False |Metadata pro hello zadat asset soubory použít pro přizpůsobení termínů.  Užitečné tooprepare Indexer toorecognize nestandardní termínů slova jako jsou podstatná jména správné.<br/>`<metadata key="..." value="..."/>` <br/><br/>Můžete zadat **hodnoty** pro předdefinované **klíče**. Aktuálně jsou podporovány hello následující klíče:<br/><br/>"title" a "Popis" - použity pro slovník přizpůsobení tootweak hello jazyk modelu pro úlohu a přesnosti rozpoznávání řeči.  hodnoty Hello počáteční hodnoty Internet hledání toofind kontextově relevantní textu dokumentů, pomocí hello obsah tooaugment hello interní slovník pro dobu trvání hello vaše úlohy indexování.<br/>`<metadata key="title" value="[Title of hello media file]" />`<br/>`<metadata key="description" value="[Description of hello media file] />"` |
| **funkce** <br/><br/> Přidaná do verze 1.2. Funkce hello podporována pouze v současné době je rozpoznávání řeči ("ASR"). |False |funkce rozpoznávání řeči Hello má hello následující nastavení klíče:<table><tr><th><p>Klíč</p></th>        <th><p>Popis</p></th><th><p>Příklad hodnoty</p></th></tr><tr><td><p>Jazyk</p></td><td><p>toobe přirozeného jazyka Hello rozpoznán v hello multimediálních souborů.</p></td><td><p>Angličtina, španělština</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>středníky oddělený seznam hello potřeby výstup titulek formátů (pokud existuje)</p></td><td><p>ttml; sami; webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Logický příznak určující, zda je soubor AIB požadované (pro použití se službou SQL Server a hello zákazníka Indexer IFilter).  Další informace najdete v tématu <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">pomocí AIB souborů pomocí Azure Media Indexer a SQL Server</a>.</p></td><td><p>True; False</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Logický příznak určující, zda je požadovaný soubor XML – klíčové slovo.</p></td><td><p>True; False. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Logický příznak určující, zda tooforce úplné titulky (bez ohledu na úroveň spolehlivosti).  </p><p>Výchozí hodnota je nastavena hodnota false, v takovém případě slova a slovní spojení, které mají méně než 50 % budou vynechaný hello poslední popisek výstupy a nahrazuje symbol tří teček ("...").  výpustky Hello jsou užitečné pro ovládací prvek popisek kvality a auditování.</p></td><td><p>True; False. </p></td></tr></table> |

### <a id="error_codes"></a>Kódy chyb
V případě hello chyby by měl sestav Azure Media Indexer zpět mezi hello následující kódy chyb:

| Kód | Name (Název) | Možné příčiny |
| --- | --- | --- |
| 2000 |Neplatná konfigurace |Neplatná konfigurace |
| 2001 |Neplatné vstupní prostředky |Chybí vstupní prostředky nebo prázdný asset. |
| 2002 |Neplatný manifest |Manifest je prázdný nebo obsahuje neplatné položky manifestu. |
| 2003 |Soubor média se nezdařilo toodownload |Neplatná adresa URL v souboru manifestu. |
| 2004 |Nepodporovaný protokol |Protokol adresy URL média není podporován. |
| 2005 |Nepodporovaný typ souboru |Typ souboru vstupní média není podporován. |
| 2006 |Příliš mnoho vstupní soubory |V manifestu vstupní hello jsou víc než 10 soubory. |
| 3000 |Soubor média se nezdařilo toodecode |Kodeků média není podporovaný. <br/>nebo<br/> Soubor poškozená média <br/>nebo<br/> Žádné zvukové datový proud v vstupními médii. |
| 4000 |Batch indexování byla částečně úspěšná |Některé hello vstupní médium, které soubory se nepodařilo toobe indexované. Další informace najdete v tématu <a href="#output_files">výstupní soubory</a>. |
| ostatní |Vnitřní chyby |Obraťte se na tým podpory. indexer@microsoft.com |

## <a id="supported_languages"></a>Podporované jazyky
Aktuálně jsou podporované jazyky hello angličtinu a slovenštinu. Další informace najdete v tématu [hello v1.2 verze příspěvku na blogu](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Související odkazy
[Azure Media Services Analytics – přehled](media-services-analytics-overview.md)

[AIB soubory pomocí Azure Media Indexer a SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Indexování mediálních souborů pomocí Azure Media Indexer 2 Preview](media-services-process-content-with-indexer2.md)

