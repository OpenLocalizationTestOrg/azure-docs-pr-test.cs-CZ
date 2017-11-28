---
title: "aaaDevelop Azure Functions pomocí služby Media Services"
description: "Toto téma ukazuje, jak hello toostart vývoj Azure Functions Media Services prostřednictvím portálu Azure."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 51bdcb01-1846-4e1f-bd90-70020ab471b0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/21/2017
ms.author: juliako
ms.openlocfilehash: 3b2c2fb498fea399c862dfbdb63033d06cabf6d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#<a name="develop-azure-functions-with-media-services"></a><span data-ttu-id="e6291-103">Vývoj řešení Azure Functions pomocí služby Media Services</span><span class="sxs-lookup"><span data-stu-id="e6291-103">Develop Azure Functions with Media Services</span></span>

<span data-ttu-id="e6291-104">Toto téma ukazuje, jak tooget začít s vytvářením Azure Functions, která pomocí služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="e6291-104">This topic shows you how tooget started with creating Azure Functions that use Media Services.</span></span> <span data-ttu-id="e6291-105">Hello Azure funkci definovanou v tomto tématu monitoruje kontejner účet úložiště s názvem **vstupní** pro nové soubory MP4.</span><span class="sxs-lookup"><span data-stu-id="e6291-105">hello Azure Function defined in this topic monitors a storage account container named **input** for new MP4 files.</span></span> <span data-ttu-id="e6291-106">Jakmile soubor do kontejneru úložiště hello vyřazen, aktivační události objektu blob hello provede hello funkce.</span><span class="sxs-lookup"><span data-stu-id="e6291-106">Once a file is dropped into hello storage container, hello blob trigger will execute hello function.</span></span>

<span data-ttu-id="e6291-107">Pokud chcete tooexplore a nasadit existující funkce Azure, který pomocí Azure Media Services, podívejte se na [funkce Azure Media Services](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span><span class="sxs-lookup"><span data-stu-id="e6291-107">If you want tooexplore and deploy existing Azure Functions that use Azure Media Services, check out [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span></span> <span data-ttu-id="e6291-108">Toto úložiště obsahuje příklady, které používají služby Media Services tooshow pracovní postupy související tooingesting obsahu přímo z úložiště objektů blob, kódování a zápis obsahu zpět tooblob úložiště.</span><span class="sxs-lookup"><span data-stu-id="e6291-108">This repository contains examples that use Media Services tooshow workflows related tooingesting content directly from blob storage, encoding, and writing content back tooblob storage.</span></span> <span data-ttu-id="e6291-109">Také obsahuje příklady jak toomonitor úlohy oznámení prostřednictvím Webhooky a fronty Azure.</span><span class="sxs-lookup"><span data-stu-id="e6291-109">It also includes examples of how toomonitor job notifications via WebHooks and Azure Queues.</span></span> <span data-ttu-id="e6291-110">Také můžete vyvíjet funkcí podle hello příklady v hello [funkce Azure Media Services](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) úložiště.</span><span class="sxs-lookup"><span data-stu-id="e6291-110">You can also develop your Functions based on hello examples in hello [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) repository.</span></span> <span data-ttu-id="e6291-111">Funkce hello toodeploy, stiskněte klávesu hello **nasazení tooAzure** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e6291-111">toodeploy hello functions, press hello **Deploy tooAzure** button.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6291-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e6291-112">Prerequisites</span></span>

- <span data-ttu-id="e6291-113">Před vytvořením první funkce, je nutné toohave aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="e6291-113">Before you can create your first function, you need toohave an active Azure account.</span></span> <span data-ttu-id="e6291-114">Pokud ještě nemáte účet Azure, [můžete použít bezplatné účty](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="e6291-114">If you don't already have an Azure account, [free accounts are available](https://azure.microsoft.com/free/).</span></span>
- <span data-ttu-id="e6291-115">Pokud chcete toocreate Azure Functions, který provedení akce v účtu Azure Media Services (AMS) nebo naslouchání tooevents odeslaných službou Media Services, měli byste vytvořit účet AMS, jak je popsáno [zde](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="e6291-115">If you are going toocreate Azure Functions that perform actions on your Azure Media Services (AMS) account or listen tooevents sent by Media Services, you should create an AMS account, as described [here](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="e6291-116">Pochopení [jak toouse Azure funkce](../azure-functions/functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e6291-116">Understanding of [how toouse Azure functions](../azure-functions/functions-overview.md).</span></span> <span data-ttu-id="e6291-117">Projděte si také téma:</span><span class="sxs-lookup"><span data-stu-id="e6291-117">Also, review:</span></span>
    - [<span data-ttu-id="e6291-118">Řešení Azure functions vazby HTTP a webhooku</span><span class="sxs-lookup"><span data-stu-id="e6291-118">Azure functions HTTP and webhook bindings</span></span>](../azure-functions/functions-triggers-bindings.md)
    - [<span data-ttu-id="e6291-119">Jak nastavení aplikace tooconfigure funkce Azure</span><span class="sxs-lookup"><span data-stu-id="e6291-119">How tooconfigure Azure Function app settings</span></span>](../azure-functions/functions-how-to-use-azure-function-app-settings.md)
    
## <a name="considerations"></a><span data-ttu-id="e6291-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e6291-120">Considerations</span></span>

-  <span data-ttu-id="e6291-121">Azure Functions spuštěna v rámci plánu spotřeby hello mít omezení vypršení časového limitu 5 minut.</span><span class="sxs-lookup"><span data-stu-id="e6291-121">Azure Functions running under hello Consumption plan have 5 minutes timeout limit.</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="e6291-122">Vytvoření Function App</span><span class="sxs-lookup"><span data-stu-id="e6291-122">Create a function app</span></span>

1. <span data-ttu-id="e6291-123">Přejděte toohello [portál Azure](http://portal.azure.com) a přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="e6291-123">Go toohello [Azure portal](http://portal.azure.com) and sign-in with your Azure account.</span></span>
2. <span data-ttu-id="e6291-124">Vytvoření aplikace pro funkce, jak je popsáno [zde](../azure-functions/functions-create-function-app-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e6291-124">Create a function app as described [here](../azure-functions/functions-create-function-app-portal.md).</span></span>

>[!NOTE]
> <span data-ttu-id="e6291-125">Účet úložiště, který určíte v hello **StorageConnection** proměnnou prostředí (viz další krok hello) by měl být ve hello stejné oblasti jako vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6291-125">A storage account that you specify in hello **StorageConnection** environment variable (see hello next step) should be in hello same region as your app.</span></span>

## <a name="configure-function-app-settings"></a><span data-ttu-id="e6291-126">Konfigurovat nastavení aplikace – funkce</span><span class="sxs-lookup"><span data-stu-id="e6291-126">Configure function app settings</span></span>

<span data-ttu-id="e6291-127">Při vývoji funkce Media Services, je užitečný tooadd proměnné prostředí, které se používají v rámci funkcí.</span><span class="sxs-lookup"><span data-stu-id="e6291-127">When developing Media Services functions, it is handy tooadd environment variables that will be used throughout your functions.</span></span> <span data-ttu-id="e6291-128">tooconfigure nastavení aplikace, klikněte na odkaz hello nakonfigurovat nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6291-128">tooconfigure app settings, click hello Configure App Settings link.</span></span> <span data-ttu-id="e6291-129">Další informace najdete v tématu [jak nastavení aplikace funkce Azure tooconfigure](../azure-functions/functions-how-to-use-azure-function-app-settings.md).</span><span class="sxs-lookup"><span data-stu-id="e6291-129">For more information, see  [How tooconfigure Azure Function app settings](../azure-functions/functions-how-to-use-azure-function-app-settings.md).</span></span> 

<span data-ttu-id="e6291-130">Například:</span><span class="sxs-lookup"><span data-stu-id="e6291-130">For example:</span></span>

![Nastavení](./media/media-services-azure-functions/media-services-azure-functions001.png)

<span data-ttu-id="e6291-132">Funkce Hello, definované v tomto článku předpokládá, že máte následující proměnné prostředí v nastavení aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="e6291-132">hello function, defined in this article, assumes you have hello following environment variables in your app settings:</span></span>

<span data-ttu-id="e6291-133">**AMSAccount** : *název účtu AMS* (např. testams)</span><span class="sxs-lookup"><span data-stu-id="e6291-133">**AMSAccount** : *AMS account name* (e.g. testams)</span></span>

<span data-ttu-id="e6291-134">**AMSKey** : *klíč účtu AMS* (například IHOySnH + XX3LGPfraE5fKPl0EnzvEPKkOPKCr59aiMM =)</span><span class="sxs-lookup"><span data-stu-id="e6291-134">**AMSKey** : *AMS account key* (e.g. IHOySnH+XX3LGPfraE5fKPl0EnzvEPKkOPKCr59aiMM=)</span></span>

<span data-ttu-id="e6291-135">**MediaServicesStorageAccountName** : *název účtu úložiště* (například testamsstorage)</span><span class="sxs-lookup"><span data-stu-id="e6291-135">**MediaServicesStorageAccountName** : *storage account name* (e.g., testamsstorage)</span></span>

<span data-ttu-id="e6291-136">**MediaServicesStorageAccountKey** : *klíč účtu úložiště* (například xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ nebo awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXXX ==)</span><span class="sxs-lookup"><span data-stu-id="e6291-136">**MediaServicesStorageAccountKey** : *storage account key* (e.g., xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXXX==)</span></span>

<span data-ttu-id="e6291-137">**StorageConnection** : *připojení úložiště* (například DefaultEndpointsProtocol = https; AccountName = testamsstorage; AccountKey = xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXX ==)</span><span class="sxs-lookup"><span data-stu-id="e6291-137">**StorageConnection** : *storage connection* (e.g., DefaultEndpointsProtocol=https;AccountName=testamsstorage;AccountKey=xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXX==)</span></span>

## <a name="create-a-function"></a><span data-ttu-id="e6291-138">Vytvoření funkce</span><span class="sxs-lookup"><span data-stu-id="e6291-138">Create a function</span></span>

<span data-ttu-id="e6291-139">Po nasazení aplikace funkce najdete ji mezi **App Services** Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="e6291-139">Once your function app is deployed, you can find it among **App Services** Azure Functions.</span></span>

1. <span data-ttu-id="e6291-140">Vyberte svou aplikaci funkce a klikněte na tlačítko **novou funkci**.</span><span class="sxs-lookup"><span data-stu-id="e6291-140">Select your function app and click **New Function**.</span></span>
2. <span data-ttu-id="e6291-141">Zvolte hello **C#** jazyk a **zpracování dat** scénář.</span><span class="sxs-lookup"><span data-stu-id="e6291-141">Choose hello **C#** language and **Data Processing** scenario.</span></span>
3. <span data-ttu-id="e6291-142">Zvolte **BlobTrigger** šablony.</span><span class="sxs-lookup"><span data-stu-id="e6291-142">Choose **BlobTrigger** template.</span></span> <span data-ttu-id="e6291-143">Tato funkce se spustí pokaždé, když objekt blob nahraje do hello **vstupní** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e6291-143">This function will be triggered whenever a blob is uploaded into hello **input** container.</span></span> <span data-ttu-id="e6291-144">Hello **vstupní** název je zadán v hello **cesta**, v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="e6291-144">hello **input** name is specified in hello **Path**, in hello next step.</span></span>

    ![Soubory](./media/media-services-azure-functions/media-services-azure-functions004.png)

4. <span data-ttu-id="e6291-146">Jakmile vyberete **BlobTrigger**, některé další ovládací prvky se zobrazí na stránce hello.</span><span class="sxs-lookup"><span data-stu-id="e6291-146">Once you select **BlobTrigger**, some more controls will appear on hello page.</span></span>

    ![Soubory](./media/media-services-azure-functions/media-services-azure-functions005.png)

4. <span data-ttu-id="e6291-148">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e6291-148">Click **Create**.</span></span> 


## <a name="files"></a><span data-ttu-id="e6291-149">Soubory</span><span class="sxs-lookup"><span data-stu-id="e6291-149">Files</span></span>

<span data-ttu-id="e6291-150">Funkce Azure je přidružen soubory kódu a další soubory, které jsou popsané v této části.</span><span class="sxs-lookup"><span data-stu-id="e6291-150">Your Azure function is associated with code files and other files that are described in this section.</span></span> <span data-ttu-id="e6291-151">Ve výchozím nastavení, je přidružen funkci **function.json** a **run.csx** soubory (C#).</span><span class="sxs-lookup"><span data-stu-id="e6291-151">By default, a function is associated with **function.json** and **run.csx** (C#) files.</span></span> <span data-ttu-id="e6291-152">Budete potřebovat tooadd **project.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="e6291-152">You will need tooadd a **project.json** file.</span></span> <span data-ttu-id="e6291-153">Hello zbytek této části ukazuje hello definice pro tyto soubory.</span><span class="sxs-lookup"><span data-stu-id="e6291-153">hello rest of this section shows hello definitions for these files.</span></span>

![Soubory](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a><span data-ttu-id="e6291-155">Function.JSON</span><span class="sxs-lookup"><span data-stu-id="e6291-155">function.json</span></span>

<span data-ttu-id="e6291-156">soubor function.json Hello definuje hello vazby funkcí a dalších nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e6291-156">hello function.json file defines hello function bindings and other configuration settings.</span></span> <span data-ttu-id="e6291-157">modul runtime Hello používá tento soubor toodetermine hello události toomonitor a jak toopass dat do a vrátit data z funkce provádění.</span><span class="sxs-lookup"><span data-stu-id="e6291-157">hello runtime uses this file toodetermine hello events toomonitor and how toopass data into and return data from function execution.</span></span> <span data-ttu-id="e6291-158">Další informace najdete v tématu [Azure functions vazby HTTP a webhooku](../azure-functions/functions-reference.md#function-code).</span><span class="sxs-lookup"><span data-stu-id="e6291-158">For more information, see [Azure functions HTTP and webhook bindings](../azure-functions/functions-reference.md#function-code).</span></span>

>[!NOTE]
><span data-ttu-id="e6291-159">Sada hello **zakázáno** vlastnost příliš**true** tooprevent hello funkce z spouštěna.</span><span class="sxs-lookup"><span data-stu-id="e6291-159">Set hello **disabled** property too**true** tooprevent hello function from being executed.</span></span> 


<span data-ttu-id="e6291-160">Tady je příklad **function.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="e6291-160">Here is an example of **function.json** file.</span></span>

    {
    "bindings": [
      {
        "name": "myBlob",
        "type": "blobTrigger",
        "direction": "in",
        "path": "input/{fileName}.mp4",
        "connection": "StorageConnection"
      }
    ],
    "disabled": false
    }

### <a name="projectjson"></a><span data-ttu-id="e6291-161">Project.JSON</span><span class="sxs-lookup"><span data-stu-id="e6291-161">project.json</span></span>

<span data-ttu-id="e6291-162">soubor project.json Hello obsahuje závislosti.</span><span class="sxs-lookup"><span data-stu-id="e6291-162">hello project.json file contains dependencies.</span></span> <span data-ttu-id="e6291-163">Tady je příklad **project.json** soubor, který zahrnuje hello vyžaduje rozhraní .NET Azure Media Services balíčky z Nuget.</span><span class="sxs-lookup"><span data-stu-id="e6291-163">Here is an example of **project.json** file that includes hello required .NET Azure Media Services packages from Nuget.</span></span> <span data-ttu-id="e6291-164">Všimněte si, že čísla verzí hello změní s nejnovějšími aktualizacemi toohello balíčky, tak ověřte, zda text hello nejnovější verze.</span><span class="sxs-lookup"><span data-stu-id="e6291-164">Note that hello version numbers will change with latest updates toohello packages, so you should confirm hello most recent versions.</span></span> 

    {
      "frameworks": {
        "net46":{
          "dependencies": {
        "windowsazure.mediaservices": "3.8.0.5",
        "windowsazure.mediaservices.extensions": "3.8.0.3"
          }
        }
       }
    }
    
### <a name="runcsx"></a><span data-ttu-id="e6291-165">Run.csx</span><span class="sxs-lookup"><span data-stu-id="e6291-165">run.csx</span></span>

<span data-ttu-id="e6291-166">Toto je hello C# – kód pro funkce.</span><span class="sxs-lookup"><span data-stu-id="e6291-166">This is hello C# code for your function.</span></span>  <span data-ttu-id="e6291-167">Funkce Hello definovaná níže monitorování kontejner účet úložiště s názvem **vstupní** (který je co byl zadaný v cestě hello) pro nové soubory MP4.</span><span class="sxs-lookup"><span data-stu-id="e6291-167">hello function defined below monitors a storage account container named **input** (that is what was specified in hello path) for new MP4 files.</span></span> <span data-ttu-id="e6291-168">Jakmile soubor do kontejneru úložiště hello vyřazen, aktivační události objektu blob hello provede hello funkce.</span><span class="sxs-lookup"><span data-stu-id="e6291-168">Once a file is dropped into hello storage container, hello blob trigger will execute hello function.</span></span>
    
<span data-ttu-id="e6291-169">Příklad Hello definované v této části ukazuje</span><span class="sxs-lookup"><span data-stu-id="e6291-169">hello example defined in this section demonstrates</span></span> 

1. <span data-ttu-id="e6291-170">jak tooingest prostředek ve službě Media Services účet (zkopírováním do objektu blob do assetu AMS) a</span><span class="sxs-lookup"><span data-stu-id="e6291-170">how tooingest an asset into a Media Services account (by coping a blob into an AMS asset) and</span></span> 
2. <span data-ttu-id="e6291-171">jak toosubmit kódování úlohy, která používá Media Encoder Standard na "adaptivní datové proudy" přednastavení.</span><span class="sxs-lookup"><span data-stu-id="e6291-171">how toosubmit an encoding job that uses Media Encoder Standard's "Adaptive Streaming" preset .</span></span>

<span data-ttu-id="e6291-172">Ve scénáři reálného života hello budete pravděpodobně má tootrack průběh úlohy a pak publikujte zakódovanému assetu.</span><span class="sxs-lookup"><span data-stu-id="e6291-172">In hello real life scenario, you most likely want tootrack job progress and then publish your encoded asset.</span></span> <span data-ttu-id="e6291-173">Další informace najdete v tématu [použití Azure Webhooky toomonitor Media Services úlohy oznámení](media-services-dotnet-check-job-progress-with-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="e6291-173">For more information, see [Use Azure WebHooks toomonitor Media Services job notifications](media-services-dotnet-check-job-progress-with-webhooks.md).</span></span> <span data-ttu-id="e6291-174">Další příklady najdete v tématu [funkce Azure Media Services](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span><span class="sxs-lookup"><span data-stu-id="e6291-174">For more examples, see [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span></span>  

<span data-ttu-id="e6291-175">Po dokončení definování funkce klikněte na tlačítko **uložte a spusťte**.</span><span class="sxs-lookup"><span data-stu-id="e6291-175">Once you are done defining your function click **Save and Run**.</span></span>

    #r "Microsoft.WindowsAzure.Storage"
    #r "Newtonsoft.Json"
    #r "System.Web"

    using System;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Net;
    using System.Threading;
    using System.Threading.Tasks;
    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage.Auth;

    private static readonly string _mediaServicesAccountName = Environment.GetEnvironmentVariable("AMSAccount");
    private static readonly string _mediaServicesAccountKey = Environment.GetEnvironmentVariable("AMSKey");

    static string _storageAccountName = Environment.GetEnvironmentVariable("MediaServicesStorageAccountName");
    static string _storageAccountKey = Environment.GetEnvironmentVariable("MediaServicesStorageAccountKey");

    private static CloudStorageAccount _destinationStorageAccount = null;

    // Field for service context.
    private static CloudMediaContext _context = null;
    private static MediaServicesCredentials _cachedCredentials = null;

    public static void Run(CloudBlockBlob myBlob, string fileName, TraceWriter log)
    {
        // NOTE that hello variables {fileName} here come from hello path setting in function.json
        // and are passed into hello  Run method signature above. We can use this toomake decisions on what type of file
        // was dropped into hello input container for hello function. 

        // No need toodo any Retry strategy in this function, By default, hello SDK calls a function up too5 times for a 
        // given blob. If hello fifth try fails, hello SDK adds a message tooa queue named webjobs-blobtrigger-poison.

        log.Info($"C# Blob trigger function processed: {fileName}.mp4");
        log.Info($"Using Azure Media Services account : {_mediaServicesAccountName}");


        try
        {
        // Create and cache hello Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(
                _mediaServicesAccountName,
                _mediaServicesAccountKey);

        // Used hello chached credentials toocreate CloudMediaContext.
        _context = new CloudMediaContext(_cachedCredentials);

        // Step 1:  Copy hello Blob into a new Input Asset for hello Job
        // ***NOTE: Ideally we would have a method tooingest a Blob directly here somehow. 
        // using code from this sample - https://azure.microsoft.com/en-us/documentation/articles/media-services-copying-existing-blob/

        StorageCredentials mediaServicesStorageCredentials =
            new StorageCredentials(_storageAccountName, _storageAccountKey);

        IAsset newAsset = CreateAssetFromBlob(myBlob, fileName, log).GetAwaiter().GetResult();

        // Step 2: Create an Encoding Job

        // Declare a new encoding job with hello Standard encoder
        IJob job = _context.Jobs.Create("Azure Function - MES Job");

        // Get a media processor reference, and pass tooit hello name of hello 
        // processor toouse for hello specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Create a task with hello encoding details, using a custom preset
        ITask task = job.Tasks.AddNew("Encode with Adaptive Streaming",
            processor,
            "Adaptive Streaming",
            TaskOptions.None); 

        // Specify hello input asset toobe encoded.
        task.InputAssets.Add(newAsset);

        // Add an output asset toocontain hello results of hello job. 
        // This output is specified as AssetCreationOptions.None, which 
        // means hello output asset is not encrypted. 
        task.OutputAssets.AddNew(fileName, AssetCreationOptions.None);

        job.Submit();
        log.Info("Job Submitted");

        }
        catch (Exception ex)
        {
        log.Error("ERROR: failed.");
        log.Info($"StackTrace : {ex.StackTrace}");
        throw ex;
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


    public static async Task<IAsset> CreateAssetFromBlob(CloudBlockBlob blob, string assetName, TraceWriter log){
        IAsset newAsset = null;

        try{
            Task<IAsset> copyAssetTask = CreateAssetFromBlobAsync(blob, assetName, log);
            newAsset = await copyAssetTask;
            log.Info($"Asset Copied : {newAsset.Id}");
        }
        catch(Exception ex){
            log.Info("Copy Failed");
            log.Info($"ERROR : {ex.Message}");
            throw ex;
        }

        return newAsset;
    }

    /// <summary>
    /// Creates a new asset and copies blobs from hello specifed storage account.
    /// </summary>
    /// <param name="blob">hello specified blob.</param>
    /// <returns>hello new asset.</returns>
    public static async Task<IAsset> CreateAssetFromBlobAsync(CloudBlockBlob blob, string assetName, TraceWriter log)
    {
         //Get a reference toohello storage account that is associated with hello Media Services account. 
        StorageCredentials mediaServicesStorageCredentials =
        new StorageCredentials(_storageAccountName, _storageAccountKey);
        _destinationStorageAccount = new CloudStorageAccount(mediaServicesStorageCredentials, false);

        // Create a new asset. 
        var asset = _context.Assets.Create(blob.Name, AssetCreationOptions.None);
        log.Info($"Created new asset {asset.Name}");

        IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy",
        TimeSpan.FromHours(4), AccessPermissions.Write);
        ILocator destinationLocator = _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);
        CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();

        // Get hello destination asset container reference
        string destinationContainerName = (new Uri(destinationLocator.Path)).Segments[1];
        CloudBlobContainer assetContainer = destBlobStorage.GetContainerReference(destinationContainerName);

        try{
        assetContainer.CreateIfNotExists();
        }
        catch (Exception ex)
        {
        log.Error ("ERROR:" + ex.Message);
        }

        log.Info("Created asset.");

        // Get hold of hello destination blob
        CloudBlockBlob destinationBlob = assetContainer.GetBlockBlobReference(blob.Name);

        // Copy Blob
        try
        {
        using (var stream = await blob.OpenReadAsync()) 
        {            
            await destinationBlob.UploadFromStreamAsync(stream);          
        }

        log.Info("Copy Complete.");

        var assetFile = asset.AssetFiles.Create(blob.Name);
        assetFile.ContentFileSize = blob.Properties.Length;
        assetFile.IsPrimary = true;
        assetFile.Update();
        asset.Update();
        }
        catch (Exception ex)
        {
        log.Error(ex.Message);
        log.Info (ex.StackTrace);
        log.Info ("Copy Failed.");
        throw;
        }

        destinationLocator.Delete();
        writePolicy.Delete();

        return asset;
    }
##<a name="test-your-function"></a><span data-ttu-id="e6291-176">Testování funkce</span><span class="sxs-lookup"><span data-stu-id="e6291-176">Test your function</span></span>

<span data-ttu-id="e6291-177">tootest funkce, je třeba soubor MP4 do hello tooupload **vstupní** kontejneru hello účtu úložiště, který jste zadali v hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="e6291-177">tootest your function, you need tooupload an MP4 file into hello **input** container of hello storage account that you specified in hello connection string.</span></span>  

## <a name="next-step"></a><span data-ttu-id="e6291-178">Další krok</span><span class="sxs-lookup"><span data-stu-id="e6291-178">Next step</span></span>

<span data-ttu-id="e6291-179">V tomto okamžiku jsou připravené toostart vývoj aplikace s Media Services.</span><span class="sxs-lookup"><span data-stu-id="e6291-179">At this point, you are ready toostart developing a Media Services application.</span></span> 
 
<span data-ttu-id="e6291-180">Další podrobnosti a kompletní ukázky nebo řešení pomocí Azure Functions a aplikacích logiky s pracovními postupy vytváření vlastní obsahu toocreate Azure Media Services najdete v tématu hello [Media Services .NET funkce Integraiton ukázce na Githubu](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)</span><span class="sxs-lookup"><span data-stu-id="e6291-180">For more details and complete samples/solutions of using Azure Functions and Logic Apps with Azure Media Services toocreate custom content creation workflows, see hello [Media Services .NET Functions Integraiton Sample on GitHub](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)</span></span>

<span data-ttu-id="e6291-181">Další informace naleznete v [použití Azure Webhooky toomonitor Media Services úlohy oznámení pomocí rozhraní .NET](media-services-dotnet-check-job-progress-with-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="e6291-181">Also, see [Use Azure WebHooks toomonitor Media Services job notifications with .NET](media-services-dotnet-check-job-progress-with-webhooks.md).</span></span> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="e6291-182">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="e6291-182">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="e6291-183">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="e6291-183">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

