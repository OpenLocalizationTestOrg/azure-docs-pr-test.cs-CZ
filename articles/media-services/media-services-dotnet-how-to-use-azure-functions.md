---
title: "Vývoj řešení Azure Functions pomocí služby Media Services"
description: "Toto téma ukazuje, jak začít vyvíjet Azure Functions pomocí služby Media Services pomocí portálu Azure."
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
ms.openlocfilehash: 35d539855572fef6c00de614a4e57738a8abd075
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
#<a name="develop-azure-functions-with-media-services"></a><span data-ttu-id="83d8a-103">Vývoj řešení Azure Functions pomocí služby Media Services</span><span class="sxs-lookup"><span data-stu-id="83d8a-103">Develop Azure Functions with Media Services</span></span>

<span data-ttu-id="83d8a-104">Toto téma ukazuje, jak začít s vytvářením Azure Functions, která pomocí služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="83d8a-104">This topic shows you how to get started with creating Azure Functions that use Media Services.</span></span> <span data-ttu-id="83d8a-105">Funkce Azure, které jsou definované v tomto tématu monitoruje kontejner účet úložiště s názvem **vstupní** pro nové soubory MP4.</span><span class="sxs-lookup"><span data-stu-id="83d8a-105">The Azure Function defined in this topic monitors a storage account container named **input** for new MP4 files.</span></span> <span data-ttu-id="83d8a-106">Jakmile do kontejneru úložiště, bude vynechána soubor spustí aktivační události objektu blob funkce.</span><span class="sxs-lookup"><span data-stu-id="83d8a-106">Once a file is dropped into the storage container, the blob trigger will execute the function.</span></span>

<span data-ttu-id="83d8a-107">Pokud chcete prozkoumat a nasadit existující funkce Azure, který pomocí Azure Media Services, podívejte se na [funkce Azure Media Services](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span><span class="sxs-lookup"><span data-stu-id="83d8a-107">If you want to explore and deploy existing Azure Functions that use Azure Media Services, check out [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span></span> <span data-ttu-id="83d8a-108">Toto úložiště obsahuje příklady, které používají služby Media Services zobrazíte pracovní postupy související s příjem obsahu přímo z úložiště objektů blob, kódování a zápis obsahu zpět do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="83d8a-108">This repository contains examples that use Media Services to show workflows related to ingesting content directly from blob storage, encoding, and writing content back to blob storage.</span></span> <span data-ttu-id="83d8a-109">Zahrnuje také příklady, jak monitorovat úlohy oznámení prostřednictvím Webhooky a fronty Azure.</span><span class="sxs-lookup"><span data-stu-id="83d8a-109">It also includes examples of how to monitor job notifications via WebHooks and Azure Queues.</span></span> <span data-ttu-id="83d8a-110">Také můžete vyvíjet funkcí podle příklady v [funkce Azure Media Services](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) úložiště.</span><span class="sxs-lookup"><span data-stu-id="83d8a-110">You can also develop your Functions based on the examples in the [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) repository.</span></span> <span data-ttu-id="83d8a-111">Chcete-li nasadit funkce, stiskněte **nasadit do Azure** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="83d8a-111">To deploy the functions, press the **Deploy to Azure** button.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83d8a-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="83d8a-112">Prerequisites</span></span>

- <span data-ttu-id="83d8a-113">Je nutné, abyste před vytvořením první funkce měli aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="83d8a-113">Before you can create your first function, you need to have an active Azure account.</span></span> <span data-ttu-id="83d8a-114">Pokud ještě nemáte účet Azure, [můžete použít bezplatné účty](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="83d8a-114">If you don't already have an Azure account, [free accounts are available](https://azure.microsoft.com/free/).</span></span>
- <span data-ttu-id="83d8a-115">Pokud chcete vytvořit Azure Functions, který provádět akce na vašem účtu Azure Media Services (AMS) nebo poslouchat událostí odeslaných službou Media Services, měli byste vytvořit účet AMS, jak je popsáno [zde](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="83d8a-115">If you are going to create Azure Functions that perform actions on your Azure Media Services (AMS) account or listen to events sent by Media Services, you should create an AMS account, as described [here](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="83d8a-116">Pochopení [jak používat Azure functions](../azure-functions/functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="83d8a-116">Understanding of [how to use Azure functions](../azure-functions/functions-overview.md).</span></span> <span data-ttu-id="83d8a-117">Projděte si také téma:</span><span class="sxs-lookup"><span data-stu-id="83d8a-117">Also, review:</span></span>
    - [<span data-ttu-id="83d8a-118">Řešení Azure functions vazby HTTP a webhooku</span><span class="sxs-lookup"><span data-stu-id="83d8a-118">Azure functions HTTP and webhook bindings</span></span>](../azure-functions/functions-triggers-bindings.md)
    - [<span data-ttu-id="83d8a-119">Postup konfigurace nastavení aplikace Azure – funkce</span><span class="sxs-lookup"><span data-stu-id="83d8a-119">How to configure Azure Function app settings</span></span>](../azure-functions/functions-how-to-use-azure-function-app-settings.md)
    
## <a name="considerations"></a><span data-ttu-id="83d8a-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="83d8a-120">Considerations</span></span>

-  <span data-ttu-id="83d8a-121">Azure Functions spuštěna v rámci plánu spotřeby mít omezení vypršení časového limitu 5 minut.</span><span class="sxs-lookup"><span data-stu-id="83d8a-121">Azure Functions running under the Consumption plan have 5 minutes timeout limit.</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="83d8a-122">Vytvoření Function App</span><span class="sxs-lookup"><span data-stu-id="83d8a-122">Create a function app</span></span>

1. <span data-ttu-id="83d8a-123">Přejděte na web [Azure Portal](http://portal.azure.com) a přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="83d8a-123">Go to the [Azure portal](http://portal.azure.com) and sign-in with your Azure account.</span></span>
2. <span data-ttu-id="83d8a-124">Vytvoření aplikace pro funkce, jak je popsáno [zde](../azure-functions/functions-create-function-app-portal.md).</span><span class="sxs-lookup"><span data-stu-id="83d8a-124">Create a function app as described [here](../azure-functions/functions-create-function-app-portal.md).</span></span>

>[!NOTE]
> <span data-ttu-id="83d8a-125">Účet úložiště, který určíte v **StorageConnection** proměnná prostředí (viz další krok) by měla být ve stejné oblasti jako vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="83d8a-125">A storage account that you specify in the **StorageConnection** environment variable (see the next step) should be in the same region as your app.</span></span>

## <a name="configure-function-app-settings"></a><span data-ttu-id="83d8a-126">Konfigurovat nastavení aplikace – funkce</span><span class="sxs-lookup"><span data-stu-id="83d8a-126">Configure function app settings</span></span>

<span data-ttu-id="83d8a-127">Při vývoji funkce Media Services, je užitečný pro přidání proměnné prostředí, které se používají v rámci funkcí.</span><span class="sxs-lookup"><span data-stu-id="83d8a-127">When developing Media Services functions, it is handy to add environment variables that will be used throughout your functions.</span></span> <span data-ttu-id="83d8a-128">Chcete-li nakonfigurovat nastavení aplikace, klikněte na odkaz nakonfigurovat nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="83d8a-128">To configure app settings, click the Configure App Settings link.</span></span> <span data-ttu-id="83d8a-129">Další informace najdete v tématu [postup konfigurace nastavení aplikace funkce Azure](../azure-functions/functions-how-to-use-azure-function-app-settings.md).</span><span class="sxs-lookup"><span data-stu-id="83d8a-129">For more information, see  [How to configure Azure Function app settings](../azure-functions/functions-how-to-use-azure-function-app-settings.md).</span></span> 

<span data-ttu-id="83d8a-130">Například:</span><span class="sxs-lookup"><span data-stu-id="83d8a-130">For example:</span></span>

![Nastavení](./media/media-services-azure-functions/media-services-azure-functions001.png)

<span data-ttu-id="83d8a-132">Funkci definované v tomto článku předpokládá, že máte následující proměnné prostředí v nastavení aplikace:</span><span class="sxs-lookup"><span data-stu-id="83d8a-132">The function, defined in this article, assumes you have the following environment variables in your app settings:</span></span>

<span data-ttu-id="83d8a-133">**AMSAccount** : *název účtu AMS* (např. testams)</span><span class="sxs-lookup"><span data-stu-id="83d8a-133">**AMSAccount** : *AMS account name* (e.g. testams)</span></span>

<span data-ttu-id="83d8a-134">**AMSKey** : *klíč účtu AMS* (například IHOySnH + XX3LGPfraE5fKPl0EnzvEPKkOPKCr59aiMM =)</span><span class="sxs-lookup"><span data-stu-id="83d8a-134">**AMSKey** : *AMS account key* (e.g. IHOySnH+XX3LGPfraE5fKPl0EnzvEPKkOPKCr59aiMM=)</span></span>

<span data-ttu-id="83d8a-135">**MediaServicesStorageAccountName** : *název účtu úložiště* (například testamsstorage)</span><span class="sxs-lookup"><span data-stu-id="83d8a-135">**MediaServicesStorageAccountName** : *storage account name* (e.g., testamsstorage)</span></span>

<span data-ttu-id="83d8a-136">**MediaServicesStorageAccountKey** : *klíč účtu úložiště* (například xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ nebo awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXXX ==)</span><span class="sxs-lookup"><span data-stu-id="83d8a-136">**MediaServicesStorageAccountKey** : *storage account key* (e.g., xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXXX==)</span></span>

<span data-ttu-id="83d8a-137">**StorageConnection** : *připojení úložiště* (například DefaultEndpointsProtocol = https; AccountName = testamsstorage; AccountKey = xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXX ==)</span><span class="sxs-lookup"><span data-stu-id="83d8a-137">**StorageConnection** : *storage connection* (e.g., DefaultEndpointsProtocol=https;AccountName=testamsstorage;AccountKey=xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXX==)</span></span>

## <a name="create-a-function"></a><span data-ttu-id="83d8a-138">Vytvoření funkce</span><span class="sxs-lookup"><span data-stu-id="83d8a-138">Create a function</span></span>

<span data-ttu-id="83d8a-139">Po nasazení aplikace funkce najdete ji mezi **App Services** Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="83d8a-139">Once your function app is deployed, you can find it among **App Services** Azure Functions.</span></span>

1. <span data-ttu-id="83d8a-140">Vyberte svou aplikaci funkce a klikněte na tlačítko **novou funkci**.</span><span class="sxs-lookup"><span data-stu-id="83d8a-140">Select your function app and click **New Function**.</span></span>
2. <span data-ttu-id="83d8a-141">Vyberte **C#** jazyk a **zpracování dat** scénář.</span><span class="sxs-lookup"><span data-stu-id="83d8a-141">Choose the **C#** language and **Data Processing** scenario.</span></span>
3. <span data-ttu-id="83d8a-142">Zvolte **BlobTrigger** šablony.</span><span class="sxs-lookup"><span data-stu-id="83d8a-142">Choose **BlobTrigger** template.</span></span> <span data-ttu-id="83d8a-143">Tato funkce se spustí pokaždé, když se nahraje do objektu blob **vstupní** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="83d8a-143">This function will be triggered whenever a blob is uploaded into the **input** container.</span></span> <span data-ttu-id="83d8a-144">**Vstupní** název je zadán v **cesta**, v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="83d8a-144">The **input** name is specified in the **Path**, in the next step.</span></span>

    ![Soubory](./media/media-services-azure-functions/media-services-azure-functions004.png)

4. <span data-ttu-id="83d8a-146">Jakmile vyberete **BlobTrigger**, některé další ovládací prvky se zobrazí na stránce.</span><span class="sxs-lookup"><span data-stu-id="83d8a-146">Once you select **BlobTrigger**, some more controls will appear on the page.</span></span>

    ![Soubory](./media/media-services-azure-functions/media-services-azure-functions005.png)

4. <span data-ttu-id="83d8a-148">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="83d8a-148">Click **Create**.</span></span> 


## <a name="files"></a><span data-ttu-id="83d8a-149">Soubory</span><span class="sxs-lookup"><span data-stu-id="83d8a-149">Files</span></span>

<span data-ttu-id="83d8a-150">Funkce Azure je přidružen soubory kódu a další soubory, které jsou popsané v této části.</span><span class="sxs-lookup"><span data-stu-id="83d8a-150">Your Azure function is associated with code files and other files that are described in this section.</span></span> <span data-ttu-id="83d8a-151">Ve výchozím nastavení, je přidružen funkci **function.json** a **run.csx** soubory (C#).</span><span class="sxs-lookup"><span data-stu-id="83d8a-151">By default, a function is associated with **function.json** and **run.csx** (C#) files.</span></span> <span data-ttu-id="83d8a-152">Budete muset přidat **project.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="83d8a-152">You will need to add a **project.json** file.</span></span> <span data-ttu-id="83d8a-153">Zbývající část tohoto oddílu obsahuje definice pro tyto soubory.</span><span class="sxs-lookup"><span data-stu-id="83d8a-153">The rest of this section shows the definitions for these files.</span></span>

![Soubory](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a><span data-ttu-id="83d8a-155">Function.JSON</span><span class="sxs-lookup"><span data-stu-id="83d8a-155">function.json</span></span>

<span data-ttu-id="83d8a-156">Soubor function.json definuje vazby funkcí a dalších nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="83d8a-156">The function.json file defines the function bindings and other configuration settings.</span></span> <span data-ttu-id="83d8a-157">Modul runtime používá tento soubor k určení události k monitorování a jak předat data do a ze spuštění funkce vrátit data.</span><span class="sxs-lookup"><span data-stu-id="83d8a-157">The runtime uses this file to determine the events to monitor and how to pass data into and return data from function execution.</span></span> <span data-ttu-id="83d8a-158">Další informace najdete v tématu [Azure functions vazby HTTP a webhooku](../azure-functions/functions-reference.md#function-code).</span><span class="sxs-lookup"><span data-stu-id="83d8a-158">For more information, see [Azure functions HTTP and webhook bindings](../azure-functions/functions-reference.md#function-code).</span></span>

>[!NOTE]
><span data-ttu-id="83d8a-159">Nastavit **zakázáno** vlastnost **true** zabránit funkci spouštěna.</span><span class="sxs-lookup"><span data-stu-id="83d8a-159">Set the **disabled** property to **true** to prevent the function from being executed.</span></span> 


<span data-ttu-id="83d8a-160">Tady je příklad **function.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="83d8a-160">Here is an example of **function.json** file.</span></span>

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

### <a name="projectjson"></a><span data-ttu-id="83d8a-161">Project.JSON</span><span class="sxs-lookup"><span data-stu-id="83d8a-161">project.json</span></span>

<span data-ttu-id="83d8a-162">Soubor project.json obsahuje závislosti.</span><span class="sxs-lookup"><span data-stu-id="83d8a-162">The project.json file contains dependencies.</span></span> <span data-ttu-id="83d8a-163">Tady je příklad **project.json** soubor, který zahrnuje požadované balíčky .NET Azure Media Services z Nuget.</span><span class="sxs-lookup"><span data-stu-id="83d8a-163">Here is an example of **project.json** file that includes the required .NET Azure Media Services packages from Nuget.</span></span> <span data-ttu-id="83d8a-164">Všimněte si, že číslo verze se změní s nejnovějšími aktualizacemi pro balíčky, tak ověřte, zda nejnovější verze.</span><span class="sxs-lookup"><span data-stu-id="83d8a-164">Note that the version numbers will change with latest updates to the packages, so you should confirm the most recent versions.</span></span> 

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
    
### <a name="runcsx"></a><span data-ttu-id="83d8a-165">Run.csx</span><span class="sxs-lookup"><span data-stu-id="83d8a-165">run.csx</span></span>

<span data-ttu-id="83d8a-166">Toto je kód C# pro funkce.</span><span class="sxs-lookup"><span data-stu-id="83d8a-166">This is the C# code for your function.</span></span>  <span data-ttu-id="83d8a-167">Funkce definovaná níže monitorování kontejner účet úložiště s názvem **vstupní** (který je co byl zadaný v cestě) pro nové soubory MP4.</span><span class="sxs-lookup"><span data-stu-id="83d8a-167">The function defined below monitors a storage account container named **input** (that is what was specified in the path) for new MP4 files.</span></span> <span data-ttu-id="83d8a-168">Jakmile do kontejneru úložiště, bude vynechána soubor spustí aktivační události objektu blob funkce.</span><span class="sxs-lookup"><span data-stu-id="83d8a-168">Once a file is dropped into the storage container, the blob trigger will execute the function.</span></span>
    
<span data-ttu-id="83d8a-169">Příklad definované v této části ukazuje</span><span class="sxs-lookup"><span data-stu-id="83d8a-169">The example defined in this section demonstrates</span></span> 

1. <span data-ttu-id="83d8a-170">Postup ingestování prostředek do účtu Media Services (zkopírováním do objektu blob do assetu AMS) a</span><span class="sxs-lookup"><span data-stu-id="83d8a-170">how to ingest an asset into a Media Services account (by coping a blob into an AMS asset) and</span></span> 
2. <span data-ttu-id="83d8a-171">jak odeslat kódování úlohu, která využívá Media Encoder Standard na "Adaptivní datové proudy" přednastavení.</span><span class="sxs-lookup"><span data-stu-id="83d8a-171">how to submit an encoding job that uses Media Encoder Standard's "Adaptive Streaming" preset .</span></span>

<span data-ttu-id="83d8a-172">Ve scénáři reálného života, budete pravděpodobně chtít sledovat průběh úlohy a pak publikujte zakódovanému assetu.</span><span class="sxs-lookup"><span data-stu-id="83d8a-172">In the real life scenario, you most likely want to track job progress and then publish your encoded asset.</span></span> <span data-ttu-id="83d8a-173">Další informace najdete v tématu [Webhooky Azure použijte ke sledování služby Media Services úlohy oznámení](media-services-dotnet-check-job-progress-with-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="83d8a-173">For more information, see [Use Azure WebHooks to monitor Media Services job notifications](media-services-dotnet-check-job-progress-with-webhooks.md).</span></span> <span data-ttu-id="83d8a-174">Další příklady najdete v tématu [funkce Azure Media Services](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span><span class="sxs-lookup"><span data-stu-id="83d8a-174">For more examples, see [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span></span>  

<span data-ttu-id="83d8a-175">Po dokončení definování funkce klikněte na tlačítko **uložte a spusťte**.</span><span class="sxs-lookup"><span data-stu-id="83d8a-175">Once you are done defining your function click **Save and Run**.</span></span>

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
        // NOTE that the variables {fileName} here come from the path setting in function.json
        // and are passed into the  Run method signature above. We can use this to make decisions on what type of file
        // was dropped into the input container for the function. 

        // No need to do any Retry strategy in this function, By default, the SDK calls a function up to 5 times for a 
        // given blob. If the fifth try fails, the SDK adds a message to a queue named webjobs-blobtrigger-poison.

        log.Info($"C# Blob trigger function processed: {fileName}.mp4");
        log.Info($"Using Azure Media Services account : {_mediaServicesAccountName}");


        try
        {
        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(
                _mediaServicesAccountName,
                _mediaServicesAccountKey);

        // Used the chached credentials to create CloudMediaContext.
        _context = new CloudMediaContext(_cachedCredentials);

        // Step 1:  Copy the Blob into a new Input Asset for the Job
        // ***NOTE: Ideally we would have a method to ingest a Blob directly here somehow. 
        // using code from this sample - https://azure.microsoft.com/en-us/documentation/articles/media-services-copying-existing-blob/

        StorageCredentials mediaServicesStorageCredentials =
            new StorageCredentials(_storageAccountName, _storageAccountKey);

        IAsset newAsset = CreateAssetFromBlob(myBlob, fileName, log).GetAwaiter().GetResult();

        // Step 2: Create an Encoding Job

        // Declare a new encoding job with the Standard encoder
        IJob job = _context.Jobs.Create("Azure Function - MES Job");

        // Get a media processor reference, and pass to it the name of the 
        // processor to use for the specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Create a task with the encoding details, using a custom preset
        ITask task = job.Tasks.AddNew("Encode with Adaptive Streaming",
            processor,
            "Adaptive Streaming",
            TaskOptions.None); 

        // Specify the input asset to be encoded.
        task.InputAssets.Add(newAsset);

        // Add an output asset to contain the results of the job. 
        // This output is specified as AssetCreationOptions.None, which 
        // means the output asset is not encrypted. 
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
    /// Creates a new asset and copies blobs from the specifed storage account.
    /// </summary>
    /// <param name="blob">The specified blob.</param>
    /// <returns>The new asset.</returns>
    public static async Task<IAsset> CreateAssetFromBlobAsync(CloudBlockBlob blob, string assetName, TraceWriter log)
    {
         //Get a reference to the storage account that is associated with the Media Services account. 
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

        // Get the destination asset container reference
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

        // Get hold of the destination blob
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
##<a name="test-your-function"></a><span data-ttu-id="83d8a-176">Testování funkce</span><span class="sxs-lookup"><span data-stu-id="83d8a-176">Test your function</span></span>

<span data-ttu-id="83d8a-177">Chcete-li funkci otestovat, je potřeba nahrát soubor MP4 do **vstupní** kontejneru účtu úložiště, který jste zadali v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="83d8a-177">To test your function, you need to upload an MP4 file into the **input** container of the storage account that you specified in the connection string.</span></span>  

## <a name="next-step"></a><span data-ttu-id="83d8a-178">Další krok</span><span class="sxs-lookup"><span data-stu-id="83d8a-178">Next step</span></span>

<span data-ttu-id="83d8a-179">V tomto okamžiku jste připravení začít s vývojem aplikací Media Services.</span><span class="sxs-lookup"><span data-stu-id="83d8a-179">At this point, you are ready to start developing a Media Services application.</span></span> 
 
<span data-ttu-id="83d8a-180">Další podrobnosti a kompletní ukázky nebo řešení pomocí Azure Functions a Logic Apps pomocí Azure Media Services k vytváření pracovních postupů vlastní vytváření obsahu najdete v tématu [Media Services .NET funkce Integraiton ukázce na Githubu](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)</span><span class="sxs-lookup"><span data-stu-id="83d8a-180">For more details and complete samples/solutions of using Azure Functions and Logic Apps with Azure Media Services to create custom content creation workflows, see the [Media Services .NET Functions Integraiton Sample on GitHub](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)</span></span>

<span data-ttu-id="83d8a-181">Další informace naleznete v [Webhooky Azure použijte ke sledování úloh oznámení Media Services pomocí rozhraní .NET](media-services-dotnet-check-job-progress-with-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="83d8a-181">Also, see [Use Azure WebHooks to monitor Media Services job notifications with .NET](media-services-dotnet-check-job-progress-with-webhooks.md).</span></span> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="83d8a-182">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="83d8a-182">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="83d8a-183">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="83d8a-183">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

