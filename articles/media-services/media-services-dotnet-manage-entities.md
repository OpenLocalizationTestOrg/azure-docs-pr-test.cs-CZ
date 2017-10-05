---
title: "Správa prostředků a související entity pomocí služby Media Services .NET SDK"
description: "Naučte se spravovat prostředky a entit v relaci pomocí sady Media Services SDK pro .NET."
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
ms.openlocfilehash: 5efe16a09808267d0797521f9e1df2b60aec9cbb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a><span data-ttu-id="161a7-103">Správa prostředků a související entity pomocí služby Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="161a7-103">Managing Assets and Related Entities with Media Services .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="161a7-104">.NET</span><span class="sxs-lookup"><span data-stu-id="161a7-104">.NET</span></span>](media-services-dotnet-manage-entities.md)
> * [<span data-ttu-id="161a7-105">REST</span><span class="sxs-lookup"><span data-stu-id="161a7-105">REST</span></span>](media-services-rest-manage-entities.md)
> 
> 

<span data-ttu-id="161a7-106">Toto téma ukazuje, jak spravovat entit služby Azure Media Services pomocí rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="161a7-106">This topic shows how to manage Azure Media Services entities with .NET.</span></span> 

>[!NOTE]
> <span data-ttu-id="161a7-107">Od 1. dubna 2017 se automaticky odstraní libovolný záznam úlohy ve vašem účtu, který je starší než 90 dní. Spolu s ním se odstraní přidružené záznamy úkolů, a to i v případě, že celkový počet záznamů je nižší než maximální kvóta.</span><span class="sxs-lookup"><span data-stu-id="161a7-107">Starting April 1, 2017, any Job record in your account older than 90 days will be automatically deleted, along with its associated Task records, even if the total number of records is below the maximum quota.</span></span> <span data-ttu-id="161a7-108">Například na 1. dubna 2017 záznam všechny úlohy ve vašem účtu, který je starší než 31. prosinci 2016, se automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="161a7-108">For example, on April 1, 2017, any Job record in your account older than December 31, 2016, will be automatically deleted.</span></span> <span data-ttu-id="161a7-109">Pokud potřebujete úloh informace archivovat, můžete použít kód popsaných v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="161a7-109">If you need to archive the job/task information, you can use the code described in this topic.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="161a7-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="161a7-110">Prerequisites</span></span>

<span data-ttu-id="161a7-111">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="161a7-111">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="get-an-asset-reference"></a><span data-ttu-id="161a7-112">Získat odkaz na prostředek</span><span class="sxs-lookup"><span data-stu-id="161a7-112">Get an Asset Reference</span></span>
<span data-ttu-id="161a7-113">Časté úlohy je získat odkaz na prostředek existující ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="161a7-113">A frequent task is to get a reference to an existing asset in Media Services.</span></span> <span data-ttu-id="161a7-114">Následující příklad kódu ukazuje, jak můžete získat odkaz na prostředek z kolekce prostředků na serveru objekt kontextu, v závislosti na prostředek ID. Následující příklad kódu používá k získání odkazu na existující objekt IAsset dotaz Linq.</span><span class="sxs-lookup"><span data-stu-id="161a7-114">The following code example shows how you can get an asset reference from the Assets collection on the server context object, based on an asset Id. The following code example uses a Linq query to get a reference to an existing IAsset object.</span></span>

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();

        return asset;
    }

## <a name="list-all-assets"></a><span data-ttu-id="161a7-115">Zobrazí seznam všech prostředků</span><span class="sxs-lookup"><span data-stu-id="161a7-115">List All Assets</span></span>
<span data-ttu-id="161a7-116">S růstem počtu prostředků, které máte v úložišti je užitečné k zobrazení seznamu vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="161a7-116">As the number of assets you have in storage grows, it is helpful to list your assets.</span></span> <span data-ttu-id="161a7-117">Následující příklad kódu ukazuje, jak k iteraci v rámci kolekce prostředky na objekt kontextu serveru.</span><span class="sxs-lookup"><span data-stu-id="161a7-117">The following code example shows how to iterate through the Assets collection on the server context object.</span></span> <span data-ttu-id="161a7-118">S každou asset příklad kódu se zapisují taky do některé z jeho hodnot vlastností ke konzole.</span><span class="sxs-lookup"><span data-stu-id="161a7-118">With each asset, the code example also writes some of its property values to the console.</span></span> <span data-ttu-id="161a7-119">Každý prostředek může například obsahovat mnoho mediálních souborů.</span><span class="sxs-lookup"><span data-stu-id="161a7-119">For example, each asset can contain many media files.</span></span> <span data-ttu-id="161a7-120">Příklad kódu vypisuje všechny soubory, které jsou spojené s každou asset.</span><span class="sxs-lookup"><span data-stu-id="161a7-120">The code example writes out all files associated with each asset.</span></span>

    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);

        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();

        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");

            // Display the files associated with each asset. 
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

## <a name="get-a-job-reference"></a><span data-ttu-id="161a7-121">Získejte odkaz na úlohu</span><span class="sxs-lookup"><span data-stu-id="161a7-121">Get a Job Reference</span></span>

<span data-ttu-id="161a7-122">Při práci s zpracování úlohy v kódu Media Services, můžete často potřebují k získání odkazu ke stávající úloze podle Id. Následující příklad kódu ukazuje, jak odkazovat na objekt IJob z kolekce úloh.</span><span class="sxs-lookup"><span data-stu-id="161a7-122">When you work with processing tasks in Media Services code, you often need to get a reference to an existing job based on an Id. The following code example shows how to get a reference to an IJob object from the Jobs collection.</span></span>

<span data-ttu-id="161a7-123">Musíte získat odkaz na úlohu při spouštění úlohy kódování dlouho běžící a potřebují kontrolovat stav úlohy na vlákno.</span><span class="sxs-lookup"><span data-stu-id="161a7-123">You may need to get a job reference when starting a long-running encoding job, and need to check the job status on a thread.</span></span> <span data-ttu-id="161a7-124">V takových případech když se metoda vrátí z vlákna, budete muset načíst aktualizovat odkaz na úlohu.</span><span class="sxs-lookup"><span data-stu-id="161a7-124">In cases like this, when the method returns from a thread, you need to retrieve a refreshed reference to a job.</span></span>

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();

        return job;
    }

## <a name="list-jobs-and-assets"></a><span data-ttu-id="161a7-125">Seznam úloh a prostředky</span><span class="sxs-lookup"><span data-stu-id="161a7-125">List Jobs and Assets</span></span>
<span data-ttu-id="161a7-126">Seznam prostředků s jejich přidruženou úlohu ve službě Media Services je důležitá související úloha.</span><span class="sxs-lookup"><span data-stu-id="161a7-126">An important related task is to list assets with their associated job in Media Services.</span></span> <span data-ttu-id="161a7-127">Následující příklad kódu ukazuje, jak zobrazit každý objekt IJob a potom pro každou úlohu, se zobrazí vlastnosti úlohy, všechny související úkoly, všechny vstupní prostředky a všechny prostředky výstup.</span><span class="sxs-lookup"><span data-stu-id="161a7-127">The following code example shows you how to list each IJob object, and then for each job, it displays properties about the job, all related tasks, all input assets, and all output assets.</span></span> <span data-ttu-id="161a7-128">Kód v tomto příkladu může být užitečné pro mnoho dalších úkolů.</span><span class="sxs-lookup"><span data-stu-id="161a7-128">The code in this example can be useful for numerous other tasks.</span></span> <span data-ttu-id="161a7-129">Například pokud chcete do seznamu prostředků výstup z jednoho nebo více úloh kódování, které jste spustili dříve, tento kód ukazuje, jak pro přístup k výstupu prostředky.</span><span class="sxs-lookup"><span data-stu-id="161a7-129">For example, if you want to list the output assets from one or more encoding jobs that you ran previously, this code shows how to access the output assets.</span></span> <span data-ttu-id="161a7-130">Až budete mít odkaz na výstupní asset, abyste pak zajistit obsah na jiné uživatele nebo aplikace stáhnout nebo poskytnutím adresy URL.</span><span class="sxs-lookup"><span data-stu-id="161a7-130">When you have a reference to an output asset, you can then deliver the content to other users or applications by downloading it, or providing URLs.</span></span> 

<span data-ttu-id="161a7-131">Další informace o možnostech pro různé prostředky naleznete v části [poskytovat prostředky pomocí sady Media Services SDK pro .NET](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="161a7-131">For more information on options for delivering assets, see [Deliver Assets with the Media Services SDK for .NET](media-services-deliver-streaming-content.md).</span></span>

    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);

        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();

        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");


            // For each job, display the associated tasks (a job  
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

            // For each job, display the list of input media assets.
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

            // For each job, display the list of output media assets.
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

## <a name="list-all-access-policies"></a><span data-ttu-id="161a7-132">Zobrazí seznam všech zásad přístupu</span><span class="sxs-lookup"><span data-stu-id="161a7-132">List all Access Policies</span></span>
<span data-ttu-id="161a7-133">Ve službě Media Services můžete definovat zásady přístupu na prostředek nebo jeho soubory.</span><span class="sxs-lookup"><span data-stu-id="161a7-133">In Media Services, you can define an access policy on an asset or its files.</span></span> <span data-ttu-id="161a7-134">Zásady přístupu definuje oprávnění pro soubor nebo prostředek (jaký typ přístupu, a jeho trvání).</span><span class="sxs-lookup"><span data-stu-id="161a7-134">An access policy defines the permissions for a file or an asset (what type of access, and the duration).</span></span> <span data-ttu-id="161a7-135">V kódu Media Services obvykle definovat zásady přístupu vytvořením objektu IAccessPolicy a přiřadí se mu existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="161a7-135">In your Media Services code, you typically define an access policy by creating an IAccessPolicy object and then associating it with an existing asset.</span></span> <span data-ttu-id="161a7-136">Poté vytvoříte objekt ILocator, který vám umožňuje poskytuje přímý přístup k prostředkům ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="161a7-136">Then you create a ILocator object, which lets you provide direct access to assets in Media Services.</span></span> <span data-ttu-id="161a7-137">Projekt Visual Studio, který doprovází tato řada dokumentace obsahuje několik příkladů kódu, které ukazují, jak vytvořit a přiřadit zásady přístupu a lokátory k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="161a7-137">The Visual Studio project that accompanies this documentation series contains several code examples that show how to create and assign access policies and locators to assets.</span></span>

<span data-ttu-id="161a7-138">Následující příklad kódu ukazuje, jak zobrazit seznam všech zásad přístupu na serveru a zobrazuje typ oprávnění spojená s každým.</span><span class="sxs-lookup"><span data-stu-id="161a7-138">The following code example shows how to list all access policies on the server, and shows the type of permissions associated with each.</span></span> <span data-ttu-id="161a7-139">Další užitečné možností zobrazení zásady přístupu je seznam všech objektů ILocator na serveru, a pak pro každý Lokátor můžete vytvořit seznam svých zásad přidružené přístup pomocí jeho AccessPolicy vlastnost.</span><span class="sxs-lookup"><span data-stu-id="161a7-139">Another useful way to view access policies is to list all ILocator objects on the server, and then for each locator, you can list its associated access policy by using its AccessPolicy property.</span></span>

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
    
## <a name="limit-access-policies"></a><span data-ttu-id="161a7-140">Zásady omezení přístupu</span><span class="sxs-lookup"><span data-stu-id="161a7-140">Limit Access Policies</span></span> 

>[!NOTE]
> <span data-ttu-id="161a7-141">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="161a7-141">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="161a7-142">Pokud vždy používáte stejné dny / přístupová oprávnění, například zásady pro lokátory, které mají zůstat na místě po dlouhou dobu (zásady bez odeslání), měli byste použít stejné ID zásad.</span><span class="sxs-lookup"><span data-stu-id="161a7-142">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> 

<span data-ttu-id="161a7-143">Můžete například vytvořit obecné sady zásad s následující kód, který by spustit pouze jednou v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="161a7-143">For example, you can create a generic set of policies with the following code that would only run one time in your application.</span></span> <span data-ttu-id="161a7-144">ID může přihlásit do souboru protokolu pro pozdější použití:</span><span class="sxs-lookup"><span data-stu-id="161a7-144">You can log IDs to a log file for later use:</span></span>

    double year = 365.25;
    double week = 7;
    IAccessPolicy policyYear = _context.AccessPolicies.Create("One Year", TimeSpan.FromDays(year), AccessPermissions.Read);
    IAccessPolicy policy100Year = _context.AccessPolicies.Create("Hundred Years", TimeSpan.FromDays(year * 100), AccessPermissions.Read);
    IAccessPolicy policyWeek = _context.AccessPolicies.Create("One Week", TimeSpan.FromDays(week), AccessPermissions.Read);

    Console.WriteLine("One year policy ID is: " + policyYear.Id);
    Console.WriteLine("100 year policy ID is: " + policy100Year.Id);
    Console.WriteLine("One week policy ID is: " + policyWeek.Id);

<span data-ttu-id="161a7-145">Pak můžete použít existující ID ve vašem kódu takto:</span><span class="sxs-lookup"><span data-stu-id="161a7-145">Then, you can use the existing IDs in your code like this:</span></span>

    const string policy1YearId = "nb:pid:UUID:2a4f0104-51a9-4078-ae26-c730f88d35cf";


    // Get the standard policy for 1 year read only
    var tempPolicyId = from b in _context.AccessPolicies
                       where b.Id == policy1YearId
                       select b;
    IAccessPolicy policy1Year = tempPolicyId.FirstOrDefault();

    // Get the existing asset
    var tempAsset = from a in _context.Assets
                where a.Id == assetID
                select a;
    IAsset asset = tempAsset.SingleOrDefault();

    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
        policy1Year,
        DateTime.UtcNow.AddMinutes(-5));
    Console.WriteLine("The locator base path is " + originLocator.BaseUri.ToString());

## <a name="list-all-locators"></a><span data-ttu-id="161a7-146">Zobrazí seznam všech lokátory</span><span class="sxs-lookup"><span data-stu-id="161a7-146">List All Locators</span></span>
<span data-ttu-id="161a7-147">Lokátor je adresu URL, která poskytuje přímý cestu pro přístup k assetu, společně s oprávnění pro daný prostředek podle definice zásady přidružené přístup lokátoru.</span><span class="sxs-lookup"><span data-stu-id="161a7-147">A locator is a URL that provides a direct path to access an asset, along with permissions to the asset as defined by the locator's associated access policy.</span></span> <span data-ttu-id="161a7-148">Každý prostředek může mít na kolekci objektů ILocator u jeho vlastnost lokátory přidruženo.</span><span class="sxs-lookup"><span data-stu-id="161a7-148">Each asset can have a collection of ILocator objects associated with it on its Locators property.</span></span> <span data-ttu-id="161a7-149">Kontext server má také lokátory kolekce, která obsahuje všechny lokátory.</span><span class="sxs-lookup"><span data-stu-id="161a7-149">The server context also has a Locators collection that contains all locators.</span></span>

<span data-ttu-id="161a7-150">Následující příklad kódu zobrazuje seznam všech lokátory na serveru.</span><span class="sxs-lookup"><span data-stu-id="161a7-150">The following code example lists all locators on the server.</span></span> <span data-ttu-id="161a7-151">Pro každý Lokátor zobrazuje Id související zásady asset a přístup.</span><span class="sxs-lookup"><span data-stu-id="161a7-151">For each locator, it shows the Id for the related asset and access policy.</span></span> <span data-ttu-id="161a7-152">Také zobrazuje typ oprávnění, datum vypršení platnosti a úplnou cestu pro daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="161a7-152">It also displays the type of permissions, the expiration date, and the full path to the asset.</span></span>

<span data-ttu-id="161a7-153">Všimněte si, že Lokátor cesty pro prostředek jenom základní adresu URL pro daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="161a7-153">Note that a locator path to an asset is only a base URL to the asset.</span></span> <span data-ttu-id="161a7-154">Vytvořit přímé cestu pro jednotlivé soubory, které by mohly vyhledejte uživatele nebo aplikace, musí váš kód přidejte cestu konkrétní soubor lokátoru cesty.</span><span class="sxs-lookup"><span data-stu-id="161a7-154">To create a direct path to individual files that a user or application could browse to, your code must add the specific file path to the locator path.</span></span> <span data-ttu-id="161a7-155">Další informace o tom, jak to udělat, najdete v tématu [poskytovat prostředky pomocí sady Media Services SDK pro .NET](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="161a7-155">For more information on how to do this, see the topic [Deliver Assets with the Media Services SDK for .NET](media-services-deliver-streaming-content.md).</span></span>

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
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a><span data-ttu-id="161a7-156">Výčet prostřednictvím rozsáhlých kolekcí entit</span><span class="sxs-lookup"><span data-stu-id="161a7-156">Enumerating through large collections of entities</span></span>
<span data-ttu-id="161a7-157">Při dotazování entity, existuje omezení 1000 entit vrátí najednou, protože veřejné v2 REST omezí výsledky dotazu a 1000 výsledky.</span><span class="sxs-lookup"><span data-stu-id="161a7-157">When querying entities, there is a limit of 1000 entities returned at one time because public REST v2 limits query results to 1000 results.</span></span> <span data-ttu-id="161a7-158">Budete muset použít přeskočit a proveďte při vytváření výčtu prostřednictvím rozsáhlých kolekcí entit.</span><span class="sxs-lookup"><span data-stu-id="161a7-158">You need to use Skip and Take when enumerating through large collections of entities.</span></span> 

<span data-ttu-id="161a7-159">Následující funkce projde všechny úlohy v zadané účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="161a7-159">The following function loops through all the jobs in the provided Media Services Account.</span></span> <span data-ttu-id="161a7-160">Služba Media Services vrátí 1000 úloh v kolekci úloh.</span><span class="sxs-lookup"><span data-stu-id="161a7-160">Media Services returns 1000 jobs in Jobs Collection.</span></span> <span data-ttu-id="161a7-161">Funkce využívá přeskočit a provést, abyste měli jistotu, že všechny úlohy budou vyčísleny (v případě, že máte více než 1 000 úloh ve vašem účtu).</span><span class="sxs-lookup"><span data-stu-id="161a7-161">The function makes use of Skip and Take to make sure that all jobs are enumerated (in case you have more than 1000 jobs in your account).</span></span>

    static void ProcessJobs()
    {
        try
        {

            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;

            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
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

## <a name="delete-an-asset"></a><span data-ttu-id="161a7-162">Odstranit prostředek</span><span class="sxs-lookup"><span data-stu-id="161a7-162">Delete an Asset</span></span>
<span data-ttu-id="161a7-163">Následující příklad odstraní prostředek.</span><span class="sxs-lookup"><span data-stu-id="161a7-163">The following example deletes an asset.</span></span>

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();

        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");

    }

## <a name="delete-a-job"></a><span data-ttu-id="161a7-164">Odstranit úlohu</span><span class="sxs-lookup"><span data-stu-id="161a7-164">Delete a Job</span></span>
<span data-ttu-id="161a7-165">Pokud chcete odstranit úlohu, je nutné zkontrolovat stav úlohy, které je uvedené ve vlastnosti stavu.</span><span class="sxs-lookup"><span data-stu-id="161a7-165">To delete a job, you must check the state of the job as indicated in the State property.</span></span> <span data-ttu-id="161a7-166">Úlohy, které jsou po dokončení nebo zrušení můžete odstranit, zatímco úlohy, které jsou v některých stavech, jako jsou ve frontě, plánované nebo zpracování, je nutné nejprve zrušit, a pak můžete je odstranit.</span><span class="sxs-lookup"><span data-stu-id="161a7-166">Jobs that are finished or canceled can be deleted, while jobs that are in certain other states, such as queued, scheduled, or processing, must be canceled first, and then they can be deleted.</span></span>

<span data-ttu-id="161a7-167">Následující příklad kódu ukazuje metodu pro odstranění úlohy stavy úlohy a pak odstranění, když se stav Dokončeno nebo došlo ke zrušení.</span><span class="sxs-lookup"><span data-stu-id="161a7-167">The following code example shows a method for deleting a job by checking job states and then deleting when the state is finished or canceled.</span></span> <span data-ttu-id="161a7-168">Tento kód závisí na předchozí části v tomto tématu pro získání odkaz na úlohu: Získejte odkaz na úlohu.</span><span class="sxs-lookup"><span data-stu-id="161a7-168">This code depends on the previous section in this topic for getting a reference to a job: Get a job reference.</span></span>

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
                    // You can also call job.DeleteAsync to do async deletes.
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


## <a name="delete-an-access-policy"></a><span data-ttu-id="161a7-169">Odstranit zásady přístupu</span><span class="sxs-lookup"><span data-stu-id="161a7-169">Delete an Access Policy</span></span>
<span data-ttu-id="161a7-170">Následující příklad kódu ukazuje, jak získat odkaz na zásady přístupu na základě zásad Id a pak ji odstraňte.</span><span class="sxs-lookup"><span data-stu-id="161a7-170">The following code example shows how to get a reference to an access policy based on a policy Id, and then to delete the policy.</span></span>

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();

        policy.Delete();

    }



## <a name="media-services-learning-paths"></a><span data-ttu-id="161a7-171">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="161a7-171">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="161a7-172">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="161a7-172">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

