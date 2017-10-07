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
# <a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a><span data-ttu-id="13294-103">Správa prostředků a související entity pomocí služby Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="13294-103">Managing Assets and Related Entities with Media Services .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="13294-104">.NET</span><span class="sxs-lookup"><span data-stu-id="13294-104">.NET</span></span>](media-services-dotnet-manage-entities.md)
> * [<span data-ttu-id="13294-105">REST</span><span class="sxs-lookup"><span data-stu-id="13294-105">REST</span></span>](media-services-rest-manage-entities.md)
> 
> 

<span data-ttu-id="13294-106">Toto téma ukazuje, jak toomanage Azure Media Services entity s .NET.</span><span class="sxs-lookup"><span data-stu-id="13294-106">This topic shows how toomanage Azure Media Services entities with .NET.</span></span> 

>[!NOTE]
> <span data-ttu-id="13294-107">Od 1. dubna 2017 záznam všechny úlohy ve vašem účtu, který je starší než 90 dní se automaticky odstraní, společně s jeho přidružené záznamy úloh i v případě, že hello celkový počet záznamů je nižší než maximální kvóty hello.</span><span class="sxs-lookup"><span data-stu-id="13294-107">Starting April 1, 2017, any Job record in your account older than 90 days will be automatically deleted, along with its associated Task records, even if hello total number of records is below hello maximum quota.</span></span> <span data-ttu-id="13294-108">Například na 1. dubna 2017 záznam všechny úlohy ve vašem účtu, který je starší než 31. prosinci 2016, se automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="13294-108">For example, on April 1, 2017, any Job record in your account older than December 31, 2016, will be automatically deleted.</span></span> <span data-ttu-id="13294-109">Pokud potřebujete tooarchive hello úloh informací, můžete použít kód hello popsaných v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="13294-109">If you need tooarchive hello job/task information, you can use hello code described in this topic.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13294-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="13294-110">Prerequisites</span></span>

<span data-ttu-id="13294-111">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="13294-111">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="get-an-asset-reference"></a><span data-ttu-id="13294-112">Získat odkaz na prostředek</span><span class="sxs-lookup"><span data-stu-id="13294-112">Get an Asset Reference</span></span>
<span data-ttu-id="13294-113">Časté úlohy je tooget stávající prostředek odkaz tooan ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="13294-113">A frequent task is tooget a reference tooan existing asset in Media Services.</span></span> <span data-ttu-id="13294-114">Hello následující příklad kódu ukazuje, jak můžete získat odkaz na prostředek z kolekce hello prostředků na serveru hello objektu context, podle hello ID asset následující kód používá příklad Linq dotazu tooget existující IAsset objekt tooan odkazu.</span><span class="sxs-lookup"><span data-stu-id="13294-114">hello following code example shows how you can get an asset reference from hello Assets collection on hello server context object, based on an asset Id. hello following code example uses a Linq query tooget a reference tooan existing IAsset object.</span></span>

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

## <a name="list-all-assets"></a><span data-ttu-id="13294-115">Zobrazí seznam všech prostředků</span><span class="sxs-lookup"><span data-stu-id="13294-115">List All Assets</span></span>
<span data-ttu-id="13294-116">S růstem hello počtu prostředků, které máte v úložišti je užitečné toolist vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="13294-116">As hello number of assets you have in storage grows, it is helpful toolist your assets.</span></span> <span data-ttu-id="13294-117">Hello následující příklad kódu ukazuje, jak tooiterate prostřednictvím hello prostředky kolekce hello serveru kontextu objektu.</span><span class="sxs-lookup"><span data-stu-id="13294-117">hello following code example shows how tooiterate through hello Assets collection on hello server context object.</span></span> <span data-ttu-id="13294-118">S každou asset hello příklad kódu se zapisují taky některé jeho vlastnosti hodnoty toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="13294-118">With each asset, hello code example also writes some of its property values toohello console.</span></span> <span data-ttu-id="13294-119">Každý prostředek může například obsahovat mnoho mediálních souborů.</span><span class="sxs-lookup"><span data-stu-id="13294-119">For example, each asset can contain many media files.</span></span> <span data-ttu-id="13294-120">Příklad kódu Hello vypisuje všechny soubory, které jsou spojené s každou asset.</span><span class="sxs-lookup"><span data-stu-id="13294-120">hello code example writes out all files associated with each asset.</span></span>

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

## <a name="get-a-job-reference"></a><span data-ttu-id="13294-121">Získejte odkaz na úlohu</span><span class="sxs-lookup"><span data-stu-id="13294-121">Get a Job Reference</span></span>

<span data-ttu-id="13294-122">Při práci s zpracování úlohy v kódu Media Services, často potřebují tooget, které úlohu existující tooan odkaz založenou na hello ID následující příklad kódu ukazuje, jak tooget odkaz tooan IJob objektu z kolekce úloh hello.</span><span class="sxs-lookup"><span data-stu-id="13294-122">When you work with processing tasks in Media Services code, you often need tooget a reference tooan existing job based on an Id. hello following code example shows how tooget a reference tooan IJob object from hello Jobs collection.</span></span>

<span data-ttu-id="13294-123">Může potřebovat tooget odkaz na úlohu při spouštění úlohy kódování dlouho běžící a nemusí toocheck hello stav úlohy na vlákno.</span><span class="sxs-lookup"><span data-stu-id="13294-123">You may need tooget a job reference when starting a long-running encoding job, and need toocheck hello job status on a thread.</span></span> <span data-ttu-id="13294-124">V takových případech když hello metoda vrací výsledek z vlákna, musíte tooretrieve úlohu tooa aktualizovat odkaz.</span><span class="sxs-lookup"><span data-stu-id="13294-124">In cases like this, when hello method returns from a thread, you need tooretrieve a refreshed reference tooa job.</span></span>

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

## <a name="list-jobs-and-assets"></a><span data-ttu-id="13294-125">Seznam úloh a prostředky</span><span class="sxs-lookup"><span data-stu-id="13294-125">List Jobs and Assets</span></span>
<span data-ttu-id="13294-126">Důležité související úkol je toolist prostředky s jejich přidruženou úlohu ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="13294-126">An important related task is toolist assets with their associated job in Media Services.</span></span> <span data-ttu-id="13294-127">Hello následující příklad kódu ukazuje, jak toolist každý objekt IJob a potom pro každou úlohu, se zobrazí vlastnosti hello úlohy, všechny související úkoly, všechny vstupní prostředky, a všechny výstupní prostředky.</span><span class="sxs-lookup"><span data-stu-id="13294-127">hello following code example shows you how toolist each IJob object, and then for each job, it displays properties about hello job, all related tasks, all input assets, and all output assets.</span></span> <span data-ttu-id="13294-128">Hello kód v tomto příkladu může být užitečná pro mnoho dalších úkolů.</span><span class="sxs-lookup"><span data-stu-id="13294-128">hello code in this example can be useful for numerous other tasks.</span></span> <span data-ttu-id="13294-129">Například pokud chcete toolist hello výstupní prostředky z jedné nebo více úloh kódování, které jste spustili dříve, tento kód ukazuje, jak tooaccess hello výstupní prostředky.</span><span class="sxs-lookup"><span data-stu-id="13294-129">For example, if you want toolist hello output assets from one or more encoding jobs that you ran previously, this code shows how tooaccess hello output assets.</span></span> <span data-ttu-id="13294-130">Až budete mít výstupní asset tooan odkaz, abyste hello obsahu tooother uživatelům a aplikacím zajistit pak stáhnout, nebo zadáním adresy URL.</span><span class="sxs-lookup"><span data-stu-id="13294-130">When you have a reference tooan output asset, you can then deliver hello content tooother users or applications by downloading it, or providing URLs.</span></span> 

<span data-ttu-id="13294-131">Další informace o možnostech pro různé prostředky naleznete v části [poskytovat prostředky pomocí sady Media Services SDK pro .NET hello](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="13294-131">For more information on options for delivering assets, see [Deliver Assets with hello Media Services SDK for .NET](media-services-deliver-streaming-content.md).</span></span>

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

## <a name="list-all-access-policies"></a><span data-ttu-id="13294-132">Zobrazí seznam všech zásad přístupu</span><span class="sxs-lookup"><span data-stu-id="13294-132">List all Access Policies</span></span>
<span data-ttu-id="13294-133">Ve službě Media Services můžete definovat zásady přístupu na prostředek nebo jeho soubory.</span><span class="sxs-lookup"><span data-stu-id="13294-133">In Media Services, you can define an access policy on an asset or its files.</span></span> <span data-ttu-id="13294-134">Zásady přístupu definuje hello oprávnění pro soubor nebo prostředek (jaký typ přístupu a dobu trvání hello).</span><span class="sxs-lookup"><span data-stu-id="13294-134">An access policy defines hello permissions for a file or an asset (what type of access, and hello duration).</span></span> <span data-ttu-id="13294-135">V kódu Media Services obvykle definovat zásady přístupu vytvořením objektu IAccessPolicy a přiřadí se mu existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="13294-135">In your Media Services code, you typically define an access policy by creating an IAccessPolicy object and then associating it with an existing asset.</span></span> <span data-ttu-id="13294-136">Poté vytvoříte objekt ILocator, který vám umožňuje poskytovat přímý přístup tooassets ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="13294-136">Then you create a ILocator object, which lets you provide direct access tooassets in Media Services.</span></span> <span data-ttu-id="13294-137">Hello projektu sady Visual Studio, který doprovází tato řada dokumentace obsahuje několik příkladů kódu, které ukazují, jak toocreate a přiřadit zásady a lokátory tooassets přístup.</span><span class="sxs-lookup"><span data-stu-id="13294-137">hello Visual Studio project that accompanies this documentation series contains several code examples that show how toocreate and assign access policies and locators tooassets.</span></span>

<span data-ttu-id="13294-138">Následující příklad ukazuje kód jak Hello toolist všechny zásady přístupu na hello server a ukazuje hello typ oprávnění spojená s každým.</span><span class="sxs-lookup"><span data-stu-id="13294-138">hello following code example shows how toolist all access policies on hello server, and shows hello type of permissions associated with each.</span></span> <span data-ttu-id="13294-139">Jiné zásady přístupu tooview užitečný způsob, jak je toolist všechny objekty ILocator na hello serveru a pak pro každý Lokátor můžete vytvořit seznam svých zásad přidružené přístup pomocí jeho AccessPolicy vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="13294-139">Another useful way tooview access policies is toolist all ILocator objects on hello server, and then for each locator, you can list its associated access policy by using its AccessPolicy property.</span></span>

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
    
## <a name="limit-access-policies"></a><span data-ttu-id="13294-140">Zásady omezení přístupu</span><span class="sxs-lookup"><span data-stu-id="13294-140">Limit Access Policies</span></span> 

>[!NOTE]
> <span data-ttu-id="13294-141">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="13294-141">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="13294-142">Měli byste použít hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění, například zásady pro lokátory, které jsou určený tooremain zavedené po dlouhou dobu (bez odeslání zásady).</span><span class="sxs-lookup"><span data-stu-id="13294-142">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> 

<span data-ttu-id="13294-143">Můžete například vytvořit obecné sady zásad s hello následující kód, který by spustit pouze jednou v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="13294-143">For example, you can create a generic set of policies with hello following code that would only run one time in your application.</span></span> <span data-ttu-id="13294-144">Přihlaste se na ID tooa soubor protokolu pro pozdější použití:</span><span class="sxs-lookup"><span data-stu-id="13294-144">You can log IDs tooa log file for later use:</span></span>

    double year = 365.25;
    double week = 7;
    IAccessPolicy policyYear = _context.AccessPolicies.Create("One Year", TimeSpan.FromDays(year), AccessPermissions.Read);
    IAccessPolicy policy100Year = _context.AccessPolicies.Create("Hundred Years", TimeSpan.FromDays(year * 100), AccessPermissions.Read);
    IAccessPolicy policyWeek = _context.AccessPolicies.Create("One Week", TimeSpan.FromDays(week), AccessPermissions.Read);

    Console.WriteLine("One year policy ID is: " + policyYear.Id);
    Console.WriteLine("100 year policy ID is: " + policy100Year.Id);
    Console.WriteLine("One week policy ID is: " + policyWeek.Id);

<span data-ttu-id="13294-145">Potom můžete hello stávající ID ve vašem kódu takto:</span><span class="sxs-lookup"><span data-stu-id="13294-145">Then, you can use hello existing IDs in your code like this:</span></span>

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

## <a name="list-all-locators"></a><span data-ttu-id="13294-146">Zobrazí seznam všech lokátory</span><span class="sxs-lookup"><span data-stu-id="13294-146">List All Locators</span></span>
<span data-ttu-id="13294-147">Lokátor je adresu URL, která poskytuje přímý cesta tooaccess prostředek, společně s asset toohello oprávnění podle definice zásady přístupu přidružený k lokátoru hello.</span><span class="sxs-lookup"><span data-stu-id="13294-147">A locator is a URL that provides a direct path tooaccess an asset, along with permissions toohello asset as defined by hello locator's associated access policy.</span></span> <span data-ttu-id="13294-148">Každý prostředek může mít na kolekci objektů ILocator u jeho vlastnost lokátory přidruženo.</span><span class="sxs-lookup"><span data-stu-id="13294-148">Each asset can have a collection of ILocator objects associated with it on its Locators property.</span></span> <span data-ttu-id="13294-149">kontext server Hello má také lokátory kolekce, která obsahuje všechny lokátory.</span><span class="sxs-lookup"><span data-stu-id="13294-149">hello server context also has a Locators collection that contains all locators.</span></span>

<span data-ttu-id="13294-150">Hello následující příklad kódu zobrazuje seznam všech lokátory na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="13294-150">hello following code example lists all locators on hello server.</span></span> <span data-ttu-id="13294-151">Pro každý Lokátor zobrazuje hello Id pro související asset hello a zásady přístupu.</span><span class="sxs-lookup"><span data-stu-id="13294-151">For each locator, it shows hello Id for hello related asset and access policy.</span></span> <span data-ttu-id="13294-152">Zobrazí také hello typ oprávnění, datum vypršení platnosti hello a hello úplná cesta toohello asset.</span><span class="sxs-lookup"><span data-stu-id="13294-152">It also displays hello type of permissions, hello expiration date, and hello full path toohello asset.</span></span>

<span data-ttu-id="13294-153">Všimněte si, Lokátor cesty tooan asset je pouze základní adresa URL toohello asset.</span><span class="sxs-lookup"><span data-stu-id="13294-153">Note that a locator path tooan asset is only a base URL toohello asset.</span></span> <span data-ttu-id="13294-154">toocreate, které tooindividual přímé cesty souborů, ve kterém může vyhledat uživatele nebo aplikace, kód musíte přidat hello konkrétní soubor cesta toohello lokátoru cesty.</span><span class="sxs-lookup"><span data-stu-id="13294-154">toocreate a direct path tooindividual files that a user or application could browse to, your code must add hello specific file path toohello locator path.</span></span> <span data-ttu-id="13294-155">Další informace o tom, toodo tento, najdete v tématu hello [poskytovat prostředky pomocí sady Media Services SDK pro .NET hello](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="13294-155">For more information on how toodo this, see hello topic [Deliver Assets with hello Media Services SDK for .NET](media-services-deliver-streaming-content.md).</span></span>

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

## <a name="enumerating-through-large-collections-of-entities"></a><span data-ttu-id="13294-156">Výčet prostřednictvím rozsáhlých kolekcí entit</span><span class="sxs-lookup"><span data-stu-id="13294-156">Enumerating through large collections of entities</span></span>
<span data-ttu-id="13294-157">Při dotazování entity, existuje omezení 1000 entit vrátí najednou, protože veřejné v2 REST omezí výsledky too1000 výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="13294-157">When querying entities, there is a limit of 1000 entities returned at one time because public REST v2 limits query results too1000 results.</span></span> <span data-ttu-id="13294-158">Při vytváření výčtu prostřednictvím rozsáhlých kolekcí entit musíte toouse přeskočit a proveďte.</span><span class="sxs-lookup"><span data-stu-id="13294-158">You need toouse Skip and Take when enumerating through large collections of entities.</span></span> 

<span data-ttu-id="13294-159">Hello následující funkce smyčky prostřednictvím všechny úlohy hello ve hello zadat účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="13294-159">hello following function loops through all hello jobs in hello provided Media Services Account.</span></span> <span data-ttu-id="13294-160">Služba Media Services vrátí 1000 úloh v kolekci úloh.</span><span class="sxs-lookup"><span data-stu-id="13294-160">Media Services returns 1000 jobs in Jobs Collection.</span></span> <span data-ttu-id="13294-161">Funkce Hello díky použití přeskočit a trvat toomake opravdu, všechny úlohy budou vyčísleny (v případě, že máte více než 1 000 úloh ve vašem účtu).</span><span class="sxs-lookup"><span data-stu-id="13294-161">hello function makes use of Skip and Take toomake sure that all jobs are enumerated (in case you have more than 1000 jobs in your account).</span></span>

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

## <a name="delete-an-asset"></a><span data-ttu-id="13294-162">Odstranit prostředek</span><span class="sxs-lookup"><span data-stu-id="13294-162">Delete an Asset</span></span>
<span data-ttu-id="13294-163">Následující ukázka Hello Odstraní prostředek.</span><span class="sxs-lookup"><span data-stu-id="13294-163">hello following example deletes an asset.</span></span>

    static void DeleteAsset( IAsset asset)
    {
        // delete hello asset
        asset.Delete();

        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted hello Asset");

    }

## <a name="delete-a-job"></a><span data-ttu-id="13294-164">Odstranit úlohu</span><span class="sxs-lookup"><span data-stu-id="13294-164">Delete a Job</span></span>
<span data-ttu-id="13294-165">toodelete úlohy, je nutné zkontrolovat stav hello hello úlohy, které je uvedené ve vlastnosti stavu hello.</span><span class="sxs-lookup"><span data-stu-id="13294-165">toodelete a job, you must check hello state of hello job as indicated in hello State property.</span></span> <span data-ttu-id="13294-166">Úlohy, které jsou po dokončení nebo zrušení můžete odstranit, zatímco úlohy, které jsou v některých stavech, jako jsou ve frontě, plánované nebo zpracování, je nutné nejprve zrušit, a pak můžete je odstranit.</span><span class="sxs-lookup"><span data-stu-id="13294-166">Jobs that are finished or canceled can be deleted, while jobs that are in certain other states, such as queued, scheduled, or processing, must be canceled first, and then they can be deleted.</span></span>

<span data-ttu-id="13294-167">Hello následující příklad kódu ukazuje metodu pro odstranění úlohy kontroly stavu úlohy a odstraněním když hello stavu dokončení nebo došlo ke zrušení.</span><span class="sxs-lookup"><span data-stu-id="13294-167">hello following code example shows a method for deleting a job by checking job states and then deleting when hello state is finished or canceled.</span></span> <span data-ttu-id="13294-168">Tento kód závisí na hello předchozí části v tomto tématu pro získání úlohu tooa odkaz: Získejte odkaz na úlohu.</span><span class="sxs-lookup"><span data-stu-id="13294-168">This code depends on hello previous section in this topic for getting a reference tooa job: Get a job reference.</span></span>

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


## <a name="delete-an-access-policy"></a><span data-ttu-id="13294-169">Odstranit zásady přístupu</span><span class="sxs-lookup"><span data-stu-id="13294-169">Delete an Access Policy</span></span>
<span data-ttu-id="13294-170">Hello následující příklad kódu ukazuje, jak tooget zásadu odkaz tooan přístupu na základě zásad Id a potom toodelete hello zásad.</span><span class="sxs-lookup"><span data-stu-id="13294-170">hello following code example shows how tooget a reference tooan access policy based on a policy Id, and then toodelete hello policy.</span></span>

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



## <a name="media-services-learning-paths"></a><span data-ttu-id="13294-171">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="13294-171">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="13294-172">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="13294-172">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

