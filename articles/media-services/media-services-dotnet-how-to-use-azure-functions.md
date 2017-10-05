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
#<a name="develop-azure-functions-with-media-services"></a>Vývoj řešení Azure Functions pomocí služby Media Services

Toto téma ukazuje, jak začít s vytvářením Azure Functions, která pomocí služby Media Services. Funkce Azure, které jsou definované v tomto tématu monitoruje kontejner účet úložiště s názvem **vstupní** pro nové soubory MP4. Jakmile do kontejneru úložiště, bude vynechána soubor spustí aktivační události objektu blob funkce.

Pokud chcete prozkoumat a nasadit existující funkce Azure, který pomocí Azure Media Services, podívejte se na [funkce Azure Media Services](https://github.com/Azure-Samples/media-services-dotnet-functions-integration). Toto úložiště obsahuje příklady, které používají služby Media Services zobrazíte pracovní postupy související s příjem obsahu přímo z úložiště objektů blob, kódování a zápis obsahu zpět do úložiště objektů blob. Zahrnuje také příklady, jak monitorovat úlohy oznámení prostřednictvím Webhooky a fronty Azure. Také můžete vyvíjet funkcí podle příklady v [funkce Azure Media Services](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) úložiště. Chcete-li nasadit funkce, stiskněte **nasadit do Azure** tlačítko.

## <a name="prerequisites"></a>Požadavky

- Je nutné, abyste před vytvořením první funkce měli aktivní účet Azure. Pokud ještě nemáte účet Azure, [můžete použít bezplatné účty](https://azure.microsoft.com/free/).
- Pokud chcete vytvořit Azure Functions, který provádět akce na vašem účtu Azure Media Services (AMS) nebo poslouchat událostí odeslaných službou Media Services, měli byste vytvořit účet AMS, jak je popsáno [zde](media-services-portal-create-account.md).
- Pochopení [jak používat Azure functions](../azure-functions/functions-overview.md). Projděte si také téma:
    - [Řešení Azure functions vazby HTTP a webhooku](../azure-functions/functions-triggers-bindings.md)
    - [Postup konfigurace nastavení aplikace Azure – funkce](../azure-functions/functions-how-to-use-azure-function-app-settings.md)
    
## <a name="considerations"></a>Požadavky

-  Azure Functions spuštěna v rámci plánu spotřeby mít omezení vypršení časového limitu 5 minut.

## <a name="create-a-function-app"></a>Vytvoření Function App

1. Přejděte na web [Azure Portal](http://portal.azure.com) a přihlaste se pomocí účtu Azure.
2. Vytvoření aplikace pro funkce, jak je popsáno [zde](../azure-functions/functions-create-function-app-portal.md).

>[!NOTE]
> Účet úložiště, který určíte v **StorageConnection** proměnná prostředí (viz další krok) by měla být ve stejné oblasti jako vaše aplikace.

## <a name="configure-function-app-settings"></a>Konfigurovat nastavení aplikace – funkce

Při vývoji funkce Media Services, je užitečný pro přidání proměnné prostředí, které se používají v rámci funkcí. Chcete-li nakonfigurovat nastavení aplikace, klikněte na odkaz nakonfigurovat nastavení aplikace. Další informace najdete v tématu [postup konfigurace nastavení aplikace funkce Azure](../azure-functions/functions-how-to-use-azure-function-app-settings.md). 

Například:

![Nastavení](./media/media-services-azure-functions/media-services-azure-functions001.png)

Funkci definované v tomto článku předpokládá, že máte následující proměnné prostředí v nastavení aplikace:

**AMSAccount** : *název účtu AMS* (např. testams)

**AMSKey** : *klíč účtu AMS* (například IHOySnH + XX3LGPfraE5fKPl0EnzvEPKkOPKCr59aiMM =)

**MediaServicesStorageAccountName** : *název účtu úložiště* (například testamsstorage)

**MediaServicesStorageAccountKey** : *klíč účtu úložiště* (například xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ nebo awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXXX ==)

**StorageConnection** : *připojení úložiště* (například DefaultEndpointsProtocol = https; AccountName = testamsstorage; AccountKey = xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXX ==)

## <a name="create-a-function"></a>Vytvoření funkce

Po nasazení aplikace funkce najdete ji mezi **App Services** Azure Functions.

1. Vyberte svou aplikaci funkce a klikněte na tlačítko **novou funkci**.
2. Vyberte **C#** jazyk a **zpracování dat** scénář.
3. Zvolte **BlobTrigger** šablony. Tato funkce se spustí pokaždé, když se nahraje do objektu blob **vstupní** kontejneru. **Vstupní** název je zadán v **cesta**, v dalším kroku.

    ![Soubory](./media/media-services-azure-functions/media-services-azure-functions004.png)

4. Jakmile vyberete **BlobTrigger**, některé další ovládací prvky se zobrazí na stránce.

    ![Soubory](./media/media-services-azure-functions/media-services-azure-functions005.png)

4. Klikněte na možnost **Vytvořit**. 


## <a name="files"></a>Soubory

Funkce Azure je přidružen soubory kódu a další soubory, které jsou popsané v této části. Ve výchozím nastavení, je přidružen funkci **function.json** a **run.csx** soubory (C#). Budete muset přidat **project.json** souboru. Zbývající část tohoto oddílu obsahuje definice pro tyto soubory.

![Soubory](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a>Function.JSON

Soubor function.json definuje vazby funkcí a dalších nastavení konfigurace. Modul runtime používá tento soubor k určení události k monitorování a jak předat data do a ze spuštění funkce vrátit data. Další informace najdete v tématu [Azure functions vazby HTTP a webhooku](../azure-functions/functions-reference.md#function-code).

>[!NOTE]
>Nastavit **zakázáno** vlastnost **true** zabránit funkci spouštěna. 


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

Soubor project.json obsahuje závislosti. Tady je příklad **project.json** soubor, který zahrnuje požadované balíčky .NET Azure Media Services z Nuget. Všimněte si, že číslo verze se změní s nejnovějšími aktualizacemi pro balíčky, tak ověřte, zda nejnovější verze. 

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

Toto je kód C# pro funkce.  Funkce definovaná níže monitorování kontejner účet úložiště s názvem **vstupní** (který je co byl zadaný v cestě) pro nové soubory MP4. Jakmile do kontejneru úložiště, bude vynechána soubor spustí aktivační události objektu blob funkce.
    
Příklad definované v této části ukazuje 

1. Postup ingestování prostředek do účtu Media Services (zkopírováním do objektu blob do assetu AMS) a 
2. jak odeslat kódování úlohu, která využívá Media Encoder Standard na "Adaptivní datové proudy" přednastavení.

Ve scénáři reálného života, budete pravděpodobně chtít sledovat průběh úlohy a pak publikujte zakódovanému assetu. Další informace najdete v tématu [Webhooky Azure použijte ke sledování služby Media Services úlohy oznámení](media-services-dotnet-check-job-progress-with-webhooks.md). Další příklady najdete v tématu [funkce Azure Media Services](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).  

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
##<a name="test-your-function"></a>Testování funkce

Chcete-li funkci otestovat, je potřeba nahrát soubor MP4 do **vstupní** kontejneru účtu úložiště, který jste zadali v připojovacím řetězci.  

## <a name="next-step"></a>Další krok

V tomto okamžiku jste připravení začít s vývojem aplikací Media Services. 
 
Další podrobnosti a kompletní ukázky nebo řešení pomocí Azure Functions a Logic Apps pomocí Azure Media Services k vytváření pracovních postupů vlastní vytváření obsahu najdete v tématu [Media Services .NET funkce Integraiton ukázce na Githubu](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)

Další informace naleznete v [Webhooky Azure použijte ke sledování úloh oznámení Media Services pomocí rozhraní .NET](media-services-dotnet-check-job-progress-with-webhooks.md). 

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

