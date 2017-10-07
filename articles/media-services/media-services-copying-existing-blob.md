---
title: "aaaCopying objekty BLOB z účtu úložiště do prostředku Azure Media Services | Microsoft Docs"
description: "Toto téma ukazuje, jak toocopy existující objekt blob do služby Asset média. Příklad Hello používá rozšíření Azure Media Services .NET SDK."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 6a63823f-f3c9-424c-91b8-566f70bec346
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 08/03/2017
ms.author: juliako
ms.openlocfilehash: 40660e5cbb3698fb2b0bdf414751e47d367794da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copying-existing-blobs-into-a-media-services-asset"></a>Kopírování do služby Asset média existující objekty BLOB
Toto téma ukazuje, jak toocopy objektů blob z účtu úložiště do nového prostředku Azure Media Services (AMS) pomocí [rozšíření Azure Media Services .NET SDK](https://github.com/Azure/azure-sdk-for-media-services-extensions/).

metody rozšíření Hello funguje s:

- Regulární prostředky.
- Živé archivu prostředky (FragBlob format).
- Zdrojové a cílové prostředky patřící toodifferent Media Services účty (i v rámci různých datových střediscích). Však může být poplatky za tímto způsobem. Další informace o cenách najdete v tématu [přenosů dat](https://azure.microsoft.com/pricing/#header-11).

> [!NOTE]
> Byste neměli toochange hello obsah kontejnery objektů blob, které byly vygenerovány Media Services bez použití rozhraní API služby Media.
> 

Hello téma ukazuje dva ukázky kódu:

1. Zkopírujte objekty BLOB z prostředek v jednom účtu AMS do nového prostředku na jiném účtu AMS.
2. Zkopírujte objekty BLOB z některé účtu úložiště do nového prostředku v účtu AMS.

## <a name="copy-blobs-between-two-ams-accounts"></a>Kopírování objektů BLOB mezi dva účty AMS  

### <a name="prerequisites"></a>Požadavky

Dva účty služby Media Services. V tématu hello [jak tooCreate účtu Media Services](media-services-portal-create-account.md).

### <a name="download-sample"></a>Stažení ukázky
Můžete provést hello kroky v tomto článku nebo si můžete stáhnout ukázku, která obsahuje kód hello popsané v tomto článku [zde](https://azure.microsoft.com/documentation/samples/media-services-dotnet-copy-blob-into-asset/).

### <a name="set-up-your-project"></a>Nastavení projektu

1. Nastavení vývojového prostředí, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md). 
2. Přidejte další reference, které jsou požadovány pro tento projekt: System.Configuration.
3. Přidání hello appSettings části toohello .config souboru a aktualizace hello hodnot na základě svých účtů Media Services, hello cílový účet úložiště a hello ID zdroje asset.  

```   
<appSettings>
    <add key="AMSSourceAADTenantDomain" value="AADTenantDomain"/>
    <add key="AMSSourceRESTAPIEndpoint" value="RESTAPIEndpoint"/>
    <add key="AMSDestAADTenantDomain" value="AADTenantDomain"/>
    <add key="AMSDestRESTAPIEndpoint" value="RESTAPIEndpoint"/>
    <add key="DestStorageAccountName" value="name"/>
    <add key="DestStorageAccountKey" value="key"/>
    <add key="SourceAssetID" value="nb:cid:UUID:assetID"/>
</appSettings>
```

### <a name="copy-blobs-from-an-asset-in-one-ams-account-into-an-asset-in-another-ams-account"></a>Kopírování objektů blob z prostředek v jednom účtu AMS do prostředku na jiném účtu AMS

Hello následující kód používá rozšíření **IAsset.Copy** metoda toocopy všechny soubory v hello zdroj asset do hello cílový prostředek pomocí jednoho rozšíření.

```
using System;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Linq;
using System.Configuration;
using Microsoft.WindowsAzure.Storage.Auth;

namespace CopyExistingBlobsIntoAsset
{
    class Program
    {
        static string _sourceAADTenantDomain = ConfigurationManager.AppSettings["AMSSourceAADTenantDomain"];
        static string _sourceRESTAPIEndpoint = ConfigurationManager.AppSettings["AMSSourceRESTAPIEndpoint"];
        static string _destAADTenantDomain = ConfigurationManager.AppSettings["AMSDestAADTenantDomain"];
        static string _destRESTAPIEndpoint = ConfigurationManager.AppSettings["AMSDestRESTAPIEndpoint"];
        static string _destStorageAccountName = ConfigurationManager.AppSettings["DestStorageAccountName"];
        static string _destStorageAccountKey = ConfigurationManager.AppSettings["DestStorageAccountKey"];
        static string _sourceAssetID = ConfigurationManager.AppSettings["SourceAssetID"];

        private static CloudMediaContext _sourceContext = null;
        private static CloudMediaContext _destContext = null;

        static void Main(string[] args)
        {
            var tokenCredentials1 = new AzureAdTokenCredentials(_sourceAADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider1 = new AzureAdTokenProvider(tokenCredentials1);
            var tokenCredentials2 = new AzureAdTokenCredentials(_destAADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider2 = new AzureAdTokenProvider(tokenCredentials2);

            // Create hello context for your source Media Services account.
            _sourceContext = new CloudMediaContext(new Uri(_sourceRESTAPIEndpoint), tokenProvider1);

            // Create hello context for your destination Media Services account.
            _destContext = new CloudMediaContext(new Uri(_destRESTAPIEndpoint), tokenProvider2);

            // Get hello credentials of hello default Storage account bound tooyour destination Media Services account.
            StorageCredentials destinationStorageCredentials =
                new StorageCredentials(_destStorageAccountName, _destStorageAccountKey);

            // Get a reference toohello source asset in hello source context.
            IAsset sourceAsset = _sourceContext.Assets.Where(a => a.Id == _sourceAssetID).First();

            // Create an empty destination asset in hello destination context.
            IAsset destinationAsset = _destContext.Assets.Create(sourceAsset.Name, AssetCreationOptions.None);

            // Copy hello files in hello source asset instance into hello destination asset instance.
            sourceAsset.Copy(destinationAsset, destinationStorageCredentials);

            Console.WriteLine("Done");
        }
    }
}
```

## <a name="copy-blobs-from-a-storage-account-into-an-ams-account"></a>Kopírování objektů blob z účtu úložiště k účtu AMS 

### <a name="prerequisites"></a>Požadavky

- Jeden účet úložiště, ze kterého mají být toocopy objekty BLOB.
- Jeden účet AMS, do kterého chcete toocopy objekty BLOB.

### <a name="set-up-your-project"></a>Nastavení projektu

1. Nastavení vývojového prostředí, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md). 
2. Přidejte další reference, které jsou požadovány pro tento projekt: System.Configuration.
3. Přidejte hello appSettings části souboru .config toohello a aktualizace hello hodnot na základě vašeho zdrojového úložiště a cílový AMS účty.

```
<appSettings>
  <add key="SourceStorageAccountName" value="name" />
  <add key="SourceStorageAccountKey" value="key" />
  <add key="AMSAADTenantDomain" value="tenant"/>
  <add key="AMSESTAPIEndpoint" value="endpoint"/>
  <add key="AMSStorageAccountName" value="name" />
  <add key="AMSStorageAccountKey" value="key" />
</appSettings>
```

### <a name="copy-blobs-from-some-storage-account-into-a-new-asset-in-a-ams-account"></a>Kopírování objektů blob z některé účtu úložiště do nového prostředku v účtu AMS

Hello následující kód kopie objekty BLOB z účtu úložiště do asset Media Services. 

>[!NOTE]
>Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy). Měli byste použít hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění, například zásady pro lokátory, které jsou určený tooremain zavedené po dlouhou dobu (bez odeslání zásady). Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.

```
using System;
using System.Configuration;
using System.Linq;
using Microsoft.WindowsAzure.MediaServices.Client;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
    
namespace CopyExistingBlobsIntoAsset
{
    class Program
    {
        // Read values from hello App.config file.
        private static readonly string _AMSAADTenantDomain =
            ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _AMSRESTAPIEndpoint =
            ConfigurationManager.AppSettings["AMSESTAPIEndpoint"];
        private static readonly string _AMSStorageAccountName =
            ConfigurationManager.AppSettings["AMSStorageAccountName"];
        private static readonly string _AMSStorageAccountKey =
            ConfigurationManager.AppSettings["AMSStorageAccountKey"];
        private static readonly string _sourceStorageAccountName =
            ConfigurationManager.AppSettings["SourceStorageAccountName"];
        private static readonly string _sourceStorageAccountKey =
            ConfigurationManager.AppSettings["SourceStorageAccountKey"];

        // Field for service context.
        private static CloudMediaContext _context = null;
        private static CloudStorageAccount _sourceStorageAccount = null;
        private static CloudStorageAccount _destinationStorageAccount = null;

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AMSAADTenantDomain, 
                AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            // Create hello context for your source Media Services account.
            _context = new CloudMediaContext(new Uri(_AMSRESTAPIEndpoint), tokenProvider);
            
            _sourceStorageAccount =
                new CloudStorageAccount(new StorageCredentials(_sourceStorageAccountName,
                    _sourceStorageAccountKey), true);

            _destinationStorageAccount =
                new CloudStorageAccount(new StorageCredentials(_AMSStorageAccountName,
                    _AMSStorageAccountKey), true);

            CloudBlobClient sourceCloudBlobClient =
                _sourceStorageAccount.CreateCloudBlobClient();
            CloudBlobContainer sourceContainer =
                sourceCloudBlobClient.GetContainerReference("NameOfBlobContainerYouWantToCopy");

            CreateAssetFromExistingBlobs(sourceContainer);

            Console.WriteLine("Done");
        }

        static public IAsset CreateAssetFromExistingBlobs(CloudBlobContainer sourceBlobContainer)
        {
            CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();

            // Create a new asset. 
            IAsset asset = _context.Assets.Create("NewAsset_" + Guid.NewGuid(), AssetCreationOptions.None);

            IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy",
                TimeSpan.FromHours(24), AccessPermissions.Write);

            ILocator destinationLocator =
                _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);

            // Get hello asset container URI and Blob copy from mediaContainer tooassetContainer. 
            CloudBlobContainer destAssetContainer =
                destBlobStorage.GetContainerReference((new Uri(destinationLocator.Path)).Segments[1]);

            if (destAssetContainer.CreateIfNotExists())
            {
                destAssetContainer.SetPermissions(new BlobContainerPermissions
                {
                    PublicAccess = BlobContainerPublicAccessType.Blob
                });
            }

            var blobList = sourceBlobContainer.ListBlobs();

            foreach (var sourceBlob in blobList)
            {
                var assetFile = asset.AssetFiles.Create((sourceBlob as ICloudBlob).Name);

                ICloudBlob destinationBlob = destAssetContainer.GetBlockBlobReference(assetFile.Name);

                CopyBlob(sourceBlob as ICloudBlob, destAssetContainer);

                assetFile.ContentFileSize = (sourceBlob as ICloudBlob).Properties.Length;
                assetFile.Update();
                Console.WriteLine("File {0} is of {1} size", assetFile.Name, assetFile.ContentFileSize);
            }

            asset.Update();

            destinationLocator.Delete();
            writePolicy.Delete();

            // Set hello primary asset file.
            // If, for example, we copied a set of Smooth Streaming files, 
            // set hello .ism file toobe hello primary file. 
            // If we, for example, copied an .mp4, then hello mp4 would be hello primary file. 
            var ismAssetFile = asset.AssetFiles.ToList().
                Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).ToArray().FirstOrDefault();

            // hello following code assigns hello first .ism file as hello primary file in hello asset.
            // An asset should have one .ism file.  
            if (ismAssetFile != null)
            {
                ismAssetFile.IsPrimary = true;
                ismAssetFile.Update();
            }

            return asset;
        }

        /// <summary>
        /// Copies hello specified blob into hello specified container.
        /// </summary>
        /// <param name="sourceBlob">hello source container.</param>
        /// <param name="destinationContainer">hello destination container.</param>
        static private void CopyBlob(ICloudBlob sourceBlob, CloudBlobContainer destinationContainer)
        {
            var signature = sourceBlob.GetSharedAccessSignature(new SharedAccessBlobPolicy
            {
                Permissions = SharedAccessBlobPermissions.Read,
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
            });

            ICloudBlob destinationBlob = destinationContainer.GetBlockBlobReference(sourceBlob.Name);

            if (destinationBlob.Exists())
            {
                Console.WriteLine(string.Format("Destination blob '{0}' already exists. Skipping.", destinationBlob.Uri));
            }
            else
            {

                // Display hello size of hello source blob.
                Console.WriteLine(sourceBlob.Properties.Length);

                Console.WriteLine(string.Format("Copy blob '{0}' too'{1}'", sourceBlob.Uri, destinationBlob.Uri));
                destinationBlob.StartCopyFromBlob(new Uri(sourceBlob.Uri.AbsoluteUri + signature));

                while (true)
                {
                    // hello StartCopyFromBlob is an async operation, 
                    // so we want toocheck if hello copy operation is completed before proceeding. 
                    // toodo that, we call FetchAttributes on hello blob and check hello CopyStatus. 
                    destinationBlob.FetchAttributes();
                    if (destinationBlob.CopyState.Status != CopyStatus.Pending)
                    {
                        break;
                    }
                    //It's still not completed. So wait for some time.
                    System.Threading.Thread.Sleep(1000);
                }

                // Display hello size of hello destination blob.
                Console.WriteLine(destinationBlob.Properties.Length);

            }
        }
    }
}
```
## <a name="next-steps"></a>Další kroky

Nyní můžete kódovat nahrané assety. Další informace najdete v tématu [Kódování assetů](media-services-portal-encode.md).

Můžete také použít Azure Functions tootrigger úlohu kódování na základě souboru přicházejících do kontejneru hello nakonfigurované. Další informace najdete v [této ukázce](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

