---
title: "aaaManaging prostředky a entit v relaci pomocí sady Media Services .NET SDK"
description: "Zjistěte, jak toomanage prostředky a entit v relaci s hello sady Media Services SDK pro .NET."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 1bd8fd42-7306-463d-bfe5-f642802f1906
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: juliako
ms.openlocfilehash: 59a8543ffc6f7f30da2c67a6fcae09bc46da7a52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Správa prostředků a související entity pomocí služby Media Services .NET SDK
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-manage-entities.md)
> * [REST](media-services-rest-manage-entities.md)
> 
> 

Toto téma ukazuje, jak toomanage Azure Media Services entity s .NET. 

>[!NOTE]
> Od 1. dubna 2017 záznam všechny úlohy ve vašem účtu, který je starší než 90 dní se automaticky odstraní, společně s jeho přidružené záznamy úloh i v případě, že hello celkový počet záznamů je nižší než maximální kvóty hello. Například na 1. dubna 2017 záznam všechny úlohy ve vašem účtu, který je starší než 31. prosinci 2016, se automaticky odstraní. Pokud potřebujete tooarchive hello úloh informací, můžete použít kód hello popsaných v tomto tématu.

## <a name="prerequisites"></a>Požadavky

Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md). 

## <a name="get-an-asset-reference"></a>Získat odkaz na prostředek
Časté úlohy je tooget stávající prostředek odkaz tooan ve službě Media Services. Hello následující příklad kódu ukazuje, jak můžete získat odkaz na prostředek z kolekce hello prostředků na serveru hello objektu context, podle hello ID asset následující kód používá příklad Linq dotazu tooget existující IAsset objekt tooan odkazu.

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query tooget an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference hello asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();

        return asset;
    }

## <a name="list-all-assets"></a>Zobrazí seznam všech prostředků
S růstem hello počtu prostředků, které máte v úložišti je užitečné toolist vaše prostředky. Hello následující příklad kódu ukazuje, jak tooiterate prostřednictvím hello prostředky kolekce hello serveru kontextu objektu. S každou asset hello příklad kódu se zapisují taky některé jeho vlastnosti hodnoty toohello konzoly. Každý prostředek může například obsahovat mnoho mediálních souborů. Příklad kódu Hello vypisuje všechny soubory, které jsou spojené s každou asset.

    static void ListAssets()
    {
        string waitMessage = "Building hello list. This may take a few "
            + "seconds tooa few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);

        // Create a Stringbuilder toostore hello list that we build. 
        StringBuilder builder = new StringBuilder();

        foreach (IAsset asset in _context.Assets)
        {
            // Display hello collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");

            // Display hello files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }

        // Display output in console.
        Console.Write(builder.ToString());
    }

## <a name="get-a-job-reference"></a>Získejte odkaz na úlohu

Při práci s zpracování úlohy v kódu Media Services, často potřebují tooget, které úlohu existující tooan odkaz založenou na hello ID následující příklad kódu ukazuje, jak tooget odkaz tooan IJob objektu z kolekce úloh hello.

Může potřebovat tooget odkaz na úlohu při spouštění úlohy kódování dlouho běžící a nemusí toocheck hello stav úlohy na vlákno. V takových případech když hello metoda vrací výsledek z vlákna, musíte tooretrieve úlohu tooa aktualizovat odkaz.

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query tooget an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return hello job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();

        return job;
    }

## <a name="list-jobs-and-assets"></a>Seznam úloh a prostředky
Důležité související úkol je toolist prostředky s jejich přidruženou úlohu ve službě Media Services. Hello následující příklad kódu ukazuje, jak toolist každý objekt IJob a potom pro každou úlohu, se zobrazí vlastnosti hello úlohy, všechny související úkoly, všechny vstupní prostředky, a všechny výstupní prostředky. Hello kód v tomto příkladu může být užitečná pro mnoho dalších úkolů. Například pokud chcete toolist hello výstupní prostředky z jedné nebo více úloh kódování, které jste spustili dříve, tento kód ukazuje, jak tooaccess hello výstupní prostředky. Až budete mít výstupní asset tooan odkaz, abyste hello obsahu tooother uživatelům a aplikacím zajistit pak stáhnout, nebo zadáním adresy URL. 

Další informace o možnostech pro různé prostředky naleznete v části [poskytovat prostředky pomocí sady Media Services SDK pro .NET hello](media-services-deliver-streaming-content.md).

    // List all jobs on hello server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building hello list. This may take a few "
            + "seconds tooa few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);

        // Create a Stringbuilder toostore hello list that we build. 
        StringBuilder builder = new StringBuilder();

        foreach (IJob job in _context.Jobs)
        {
            // Display hello collection of jobs on hello server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");


            // For each job, display hello associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }

            // For each job, display hello list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {

                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }

            // For each job, display hello list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }

        }

        // Display output in console.
        Console.Write(builder.ToString());
    }

## <a name="list-all-access-policies"></a>Zobrazí seznam všech zásad přístupu
Ve službě Media Services můžete definovat zásady přístupu na prostředek nebo jeho soubory. Zásady přístupu definuje hello oprávnění pro soubor nebo prostředek (jaký typ přístupu a dobu trvání hello). V kódu Media Services obvykle definovat zásady přístupu vytvořením objektu IAccessPolicy a přiřadí se mu existující prostředek. Poté vytvoříte objekt ILocator, který vám umožňuje poskytovat přímý přístup tooassets ve službě Media Services. Hello projektu sady Visual Studio, který doprovází tato řada dokumentace obsahuje několik příkladů kódu, které ukazují, jak toocreate a přiřadit zásady a lokátory tooassets přístup.

Následující příklad ukazuje kód jak Hello toolist všechny zásady přístupu na hello server a ukazuje hello typ oprávnění spojená s každým. Jiné zásady přístupu tooview užitečný způsob, jak je toolist všechny objekty ILocator na hello serveru a pak pro každý Lokátor můžete vytvořit seznam svých zásad přidružené přístup pomocí jeho AccessPolicy vlastnosti.

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");

        }
    }
    
## <a name="limit-access-policies"></a>Zásady omezení přístupu 

>[!NOTE]
> Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy). Měli byste použít hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění, například zásady pro lokátory, které jsou určený tooremain zavedené po dlouhou dobu (bez odeslání zásady). 

Můžete například vytvořit obecné sady zásad s hello následující kód, který by spustit pouze jednou v aplikaci. Přihlaste se na ID tooa soubor protokolu pro pozdější použití:

    double year = 365.25;
    double week = 7;
    IAccessPolicy policyYear = _context.AccessPolicies.Create("One Year", TimeSpan.FromDays(year), AccessPermissions.Read);
    IAccessPolicy policy100Year = _context.AccessPolicies.Create("Hundred Years", TimeSpan.FromDays(year * 100), AccessPermissions.Read);
    IAccessPolicy policyWeek = _context.AccessPolicies.Create("One Week", TimeSpan.FromDays(week), AccessPermissions.Read);

    Console.WriteLine("One year policy ID is: " + policyYear.Id);
    Console.WriteLine("100 year policy ID is: " + policy100Year.Id);
    Console.WriteLine("One week policy ID is: " + policyWeek.Id);

Potom můžete hello stávající ID ve vašem kódu takto:

    const string policy1YearId = "nb:pid:UUID:2a4f0104-51a9-4078-ae26-c730f88d35cf";


    // Get hello standard policy for 1 year read only
    var tempPolicyId = from b in _context.AccessPolicies
                       where b.Id == policy1YearId
                       select b;
    IAccessPolicy policy1Year = tempPolicyId.FirstOrDefault();

    // Get hello existing asset
    var tempAsset = from a in _context.Assets
                where a.Id == assetID
                select a;
    IAsset asset = tempAsset.SingleOrDefault();

    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
        policy1Year,
        DateTime.UtcNow.AddMinutes(-5));
    Console.WriteLine("hello locator base path is " + originLocator.BaseUri.ToString());

## <a name="list-all-locators"></a>Zobrazí seznam všech lokátory
Lokátor je adresu URL, která poskytuje přímý cesta tooaccess prostředek, společně s asset toohello oprávnění podle definice zásady přístupu přidružený k lokátoru hello. Každý prostředek může mít na kolekci objektů ILocator u jeho vlastnost lokátory přidruženo. kontext server Hello má také lokátory kolekce, která obsahuje všechny lokátory.

Hello následující příklad kódu zobrazuje seznam všech lokátory na serveru hello. Pro každý Lokátor zobrazuje hello Id pro související asset hello a zásady přístupu. Zobrazí také hello typ oprávnění, datum vypršení platnosti hello a hello úplná cesta toohello asset.

Všimněte si, Lokátor cesty tooan asset je pouze základní adresa URL toohello asset. toocreate, které tooindividual přímé cesty souborů, ve kterém může vyhledat uživatele nebo aplikace, kód musíte přidat hello konkrétní soubor cesta toohello lokátoru cesty. Další informace o tom, toodo tento, najdete v tématu hello [poskytovat prostředky pomocí sady Media Services SDK pro .NET hello](media-services-deliver-streaming-content.md).

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // hello locator path is hello base or parent path (with included permissions) tooaccess  
            // hello media content of an asset. toocreate a full URL tooa specific media file, take 
            // hello locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a>Výčet prostřednictvím rozsáhlých kolekcí entit
Při dotazování entity, existuje omezení 1000 entit vrátí najednou, protože veřejné v2 REST omezí výsledky too1000 výsledky dotazu. Při vytváření výčtu prostřednictvím rozsáhlých kolekcí entit musíte toouse přeskočit a proveďte. 

Hello následující funkce smyčky prostřednictvím všechny úlohy hello ve hello zadat účtu Media Services. Služba Media Services vrátí 1000 úloh v kolekci úloh. Funkce Hello díky použití přeskočit a trvat toomake opravdu, všechny úlohy budou vyčísleny (v případě, že máte více než 1 000 úloh ve vašem účtu).

    static void ProcessJobs()
    {
        try
        {

            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;

            while (true)
            {
                // Loop through all Jobs (1000 at a time) in hello Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }

                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

## <a name="delete-an-asset"></a>Odstranit prostředek
Následující ukázka Hello Odstraní prostředek.

    static void DeleteAsset( IAsset asset)
    {
        // delete hello asset
        asset.Delete();

        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted hello Asset");

    }

## <a name="delete-a-job"></a>Odstranit úlohu
toodelete úlohy, je nutné zkontrolovat stav hello hello úlohy, které je uvedené ve vlastnosti stavu hello. Úlohy, které jsou po dokončení nebo zrušení můžete odstranit, zatímco úlohy, které jsou v některých stavech, jako jsou ve frontě, plánované nebo zpracování, je nutné nejprve zrušit, a pak můžete je odstranit.

Hello následující příklad kódu ukazuje metodu pro odstranění úlohy kontroly stavu úlohy a odstraněním když hello stavu dokončení nebo došlo ke zrušení. Tento kód závisí na hello předchozí části v tomto tématu pro získání úlohu tooa odkaz: Získejte odkaz na úlohu.

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;

        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);

            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync toodo async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }

        }
    }


## <a name="delete-an-access-policy"></a>Odstranit zásady přístupu
Hello následující příklad kódu ukazuje, jak tooget zásadu odkaz tooan přístupu na základě zásad Id a potom toodelete hello zásad.

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // toodelete a specific access policy, get a reference toohello policy.  
        // based on hello policy Id passed toohello method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();

        policy.Delete();

    }



## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

