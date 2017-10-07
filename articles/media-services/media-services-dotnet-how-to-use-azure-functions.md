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
#<a name="develop-azure-functions-with-media-services"></a>Vývoj řešení Azure Functions pomocí služby Media Services

Toto téma ukazuje, jak tooget začít s vytvářením Azure Functions, která pomocí služby Media Services. Hello Azure funkci definovanou v tomto tématu monitoruje kontejner účet úložiště s názvem **vstupní** pro nové soubory MP4. Jakmile soubor do kontejneru úložiště hello vyřazen, aktivační události objektu blob hello provede hello funkce.

Pokud chcete tooexplore a nasadit existující funkce Azure, který pomocí Azure Media Services, podívejte se na [funkce Azure Media Services](https://github.com/Azure-Samples/media-services-dotnet-functions-integration). Toto úložiště obsahuje příklady, které používají služby Media Services tooshow pracovní postupy související tooingesting obsahu přímo z úložiště objektů blob, kódování a zápis obsahu zpět tooblob úložiště. Také obsahuje příklady jak toomonitor úlohy oznámení prostřednictvím Webhooky a fronty Azure. Také můžete vyvíjet funkcí podle hello příklady v hello [funkce Azure Media Services](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) úložiště. Funkce hello toodeploy, stiskněte klávesu hello **nasazení tooAzure** tlačítko.

## <a name="prerequisites"></a>Požadavky

- Před vytvořením první funkce, je nutné toohave aktivní účet Azure. Pokud ještě nemáte účet Azure, [můžete použít bezplatné účty](https://azure.microsoft.com/free/).
- Pokud chcete toocreate Azure Functions, který provedení akce v účtu Azure Media Services (AMS) nebo naslouchání tooevents odeslaných službou Media Services, měli byste vytvořit účet AMS, jak je popsáno [zde](media-services-portal-create-account.md).
- Pochopení [jak toouse Azure funkce](../azure-functions/functions-overview.md). Projděte si také téma:
    - [Řešení Azure functions vazby HTTP a webhooku](../azure-functions/functions-triggers-bindings.md)
    - [Jak nastavení aplikace tooconfigure funkce Azure](../azure-functions/functions-how-to-use-azure-function-app-settings.md)
    
## <a name="considerations"></a>Požadavky

-  Azure Functions spuštěna v rámci plánu spotřeby hello mít omezení vypršení časového limitu 5 minut.

## <a name="create-a-function-app"></a>Vytvoření Function App

1. Přejděte toohello [portál Azure](http://portal.azure.com) a přihlaste se pomocí účtu Azure.
2. Vytvoření aplikace pro funkce, jak je popsáno [zde](../azure-functions/functions-create-function-app-portal.md).

>[!NOTE]
> Účet úložiště, který určíte v hello **StorageConnection** proměnnou prostředí (viz další krok hello) by měl být ve hello stejné oblasti jako vaše aplikace.

## <a name="configure-function-app-settings"></a>Konfigurovat nastavení aplikace – funkce

Při vývoji funkce Media Services, je užitečný tooadd proměnné prostředí, které se používají v rámci funkcí. tooconfigure nastavení aplikace, klikněte na odkaz hello nakonfigurovat nastavení aplikace. Další informace najdete v tématu [jak nastavení aplikace funkce Azure tooconfigure](../azure-functions/functions-how-to-use-azure-function-app-settings.md). 

Například:

![Nastavení](./media/media-services-azure-functions/media-services-azure-functions001.png)

Funkce Hello, definované v tomto článku předpokládá, že máte následující proměnné prostředí v nastavení aplikace hello:

**AMSAccount** : *název účtu AMS* (např. testams)

**AMSKey** : *klíč účtu AMS* (například IHOySnH + XX3LGPfraE5fKPl0EnzvEPKkOPKCr59aiMM =)

**MediaServicesStorageAccountName** : *název účtu úložiště* (například testamsstorage)

**MediaServicesStorageAccountKey** : *klíč účtu úložiště* (například xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ nebo awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXXX ==)

**StorageConnection** : *připojení úložiště* (například DefaultEndpointsProtocol = https; AccountName = testamsstorage; AccountKey = xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXX ==)

## <a name="create-a-function"></a>Vytvoření funkce

Po nasazení aplikace funkce najdete ji mezi **App Services** Azure Functions.

1. Vyberte svou aplikaci funkce a klikněte na tlačítko **novou funkci**.
2. Zvolte hello **C#** jazyk a **zpracování dat** scénář.
3. Zvolte **BlobTrigger** šablony. Tato funkce se spustí pokaždé, když objekt blob nahraje do hello **vstupní** kontejneru. Hello **vstupní** název je zadán v hello **cesta**, v dalším kroku hello.

    ![Soubory](./media/media-services-azure-functions/media-services-azure-functions004.png)

4. Jakmile vyberete **BlobTrigger**, některé další ovládací prvky se zobrazí na stránce hello.

    ![Soubory](./media/media-services-azure-functions/media-services-azure-functions005.png)

4. Klikněte na možnost **Vytvořit**. 


## <a name="files"></a>Soubory

Funkce Azure je přidružen soubory kódu a další soubory, které jsou popsané v této části. Ve výchozím nastavení, je přidružen funkci **function.json** a **run.csx** soubory (C#). Budete potřebovat tooadd **project.json** souboru. Hello zbytek této části ukazuje hello definice pro tyto soubory.

![Soubory](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a>Function.JSON

soubor function.json Hello definuje hello vazby funkcí a dalších nastavení konfigurace. modul runtime Hello používá tento soubor toodetermine hello události toomonitor a jak toopass dat do a vrátit data z funkce provádění. Další informace najdete v tématu [Azure functions vazby HTTP a webhooku](../azure-functions/functions-reference.md#function-code).

>[!NOTE]
>Sada hello **zakázáno** vlastnost příliš**true** tooprevent hello funkce z spouštěna. 


Tady je příklad **function.json** souboru.

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

### <a name="projectjson"></a>Project.JSON

soubor project.json Hello obsahuje závislosti. Tady je příklad **project.json** soubor, který zahrnuje hello vyžaduje rozhraní .NET Azure Media Services balíčky z Nuget. Všimněte si, že čísla verzí hello změní s nejnovějšími aktualizacemi toohello balíčky, tak ověřte, zda text hello nejnovější verze. 

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
    
### <a name="runcsx"></a>Run.csx

Toto je hello C# – kód pro funkce.  Funkce Hello definovaná níže monitorování kontejner účet úložiště s názvem **vstupní** (který je co byl zadaný v cestě hello) pro nové soubory MP4. Jakmile soubor do kontejneru úložiště hello vyřazen, aktivační události objektu blob hello provede hello funkce.
    
Příklad Hello definované v této části ukazuje 

1. jak tooingest prostředek ve službě Media Services účet (zkopírováním do objektu blob do assetu AMS) a 
2. jak toosubmit kódování úlohy, která používá Media Encoder Standard na "adaptivní datové proudy" přednastavení.

Ve scénáři reálného života hello budete pravděpodobně má tootrack průběh úlohy a pak publikujte zakódovanému assetu. Další informace najdete v tématu [použití Azure Webhooky toomonitor Media Services úlohy oznámení](media-services-dotnet-check-job-progress-with-webhooks.md). Další příklady najdete v tématu [funkce Azure Media Services](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).  

Po dokončení definování funkce klikněte na tlačítko **uložte a spusťte**.

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
##<a name="test-your-function"></a>Testování funkce

tootest funkce, je třeba soubor MP4 do hello tooupload **vstupní** kontejneru hello účtu úložiště, který jste zadali v hello připojovací řetězec.  

## <a name="next-step"></a>Další krok

V tomto okamžiku jsou připravené toostart vývoj aplikace s Media Services. 
 
Další podrobnosti a kompletní ukázky nebo řešení pomocí Azure Functions a aplikacích logiky s pracovními postupy vytváření vlastní obsahu toocreate Azure Media Services najdete v tématu hello [Media Services .NET funkce Integraiton ukázce na Githubu](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)

Další informace naleznete v [použití Azure Webhooky toomonitor Media Services úlohy oznámení pomocí rozhraní .NET](media-services-dotnet-check-job-progress-with-webhooks.md). 

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

