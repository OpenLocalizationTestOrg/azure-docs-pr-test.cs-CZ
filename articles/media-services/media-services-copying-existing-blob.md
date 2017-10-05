---
title: "Kopírování objektů blob z účtu úložiště do prostředek služby Azure Media Services | Microsoft Docs"
description: "Toto téma ukazuje, jak chcete zkopírovat existující objekt blob do služby Asset média. V příkladu rozšíření Azure Media Services .NET SDK."
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
ms.openlocfilehash: 2bc1f0114a096920d4a7c9cb57e44c9b3612bf86
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="copying-existing-blobs-into-a-media-services-asset"></a><span data-ttu-id="c75ac-104">Kopírování do služby Asset média existující objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="c75ac-104">Copying existing blobs into a Media Services Asset</span></span>
<span data-ttu-id="c75ac-105">Toto téma ukazuje, jak chcete zkopírovat objekty BLOB z účtu úložiště do nového prostředku Azure Media Services (AMS) pomocí [rozšíření Azure Media Services .NET SDK](https://github.com/Azure/azure-sdk-for-media-services-extensions/).</span><span class="sxs-lookup"><span data-stu-id="c75ac-105">This topic shows how to copy blobs from a storage account into a new Azure Media Services (AMS) asset using [Azure Media Services .NET SDK Extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions/).</span></span>

<span data-ttu-id="c75ac-106">Rozšiřující metody funguje s:</span><span class="sxs-lookup"><span data-stu-id="c75ac-106">The extension methods works with:</span></span>

- <span data-ttu-id="c75ac-107">Regulární prostředky.</span><span class="sxs-lookup"><span data-stu-id="c75ac-107">Regular assets.</span></span>
- <span data-ttu-id="c75ac-108">Živé archivu prostředky (FragBlob format).</span><span class="sxs-lookup"><span data-stu-id="c75ac-108">Live archive assets (FragBlob format).</span></span>
- <span data-ttu-id="c75ac-109">Zdrojové a cílové prostředky, které patří do různých účtech Media Services (i v rámci různých datových střediscích).</span><span class="sxs-lookup"><span data-stu-id="c75ac-109">Source and destination assets belonging to different Media Services accounts (even across different data centers).</span></span> <span data-ttu-id="c75ac-110">Však může být poplatky za tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="c75ac-110">However, there may be charges incurred by doing so.</span></span> <span data-ttu-id="c75ac-111">Další informace o cenách najdete v tématu [přenosů dat](https://azure.microsoft.com/pricing/#header-11).</span><span class="sxs-lookup"><span data-stu-id="c75ac-111">For more information about pricing, see [Data Transfers](https://azure.microsoft.com/pricing/#header-11).</span></span>

> [!NOTE]
> <span data-ttu-id="c75ac-112">Chcete-li změnit obsah kontejnery objektů blob, které byly vygenerovány Media Services bez použití rozhraní API služby Media byste neměli.</span><span class="sxs-lookup"><span data-stu-id="c75ac-112">You should not attempt to change the contents of blob containers that were generated by Media Services without using Media Service APIs.</span></span>
> 

<span data-ttu-id="c75ac-113">Toto téma vás seznámí dvě ukázky kódu:</span><span class="sxs-lookup"><span data-stu-id="c75ac-113">The topic shows two code samples:</span></span>

1. <span data-ttu-id="c75ac-114">Zkopírujte objekty BLOB z prostředek v jednom účtu AMS do nového prostředku na jiném účtu AMS.</span><span class="sxs-lookup"><span data-stu-id="c75ac-114">Copy blobs from an asset in one AMS account into a new asset in another AMS account.</span></span>
2. <span data-ttu-id="c75ac-115">Zkopírujte objekty BLOB z některé účtu úložiště do nového prostředku v účtu AMS.</span><span class="sxs-lookup"><span data-stu-id="c75ac-115">Copy blobs from some storage account into a new asset in an AMS account.</span></span>

## <a name="copy-blobs-between-two-ams-accounts"></a><span data-ttu-id="c75ac-116">Kopírování objektů BLOB mezi dva účty AMS</span><span class="sxs-lookup"><span data-stu-id="c75ac-116">Copy blobs between two AMS accounts</span></span>  

### <a name="prerequisites"></a><span data-ttu-id="c75ac-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c75ac-117">Prerequisites</span></span>

<span data-ttu-id="c75ac-118">Dva účty služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="c75ac-118">Two Media Services accounts.</span></span> <span data-ttu-id="c75ac-119">Další informace najdete v tématu [Vytvoření účtu Media Services](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="c75ac-119">See the topic [How to Create a Media Services Account](media-services-portal-create-account.md).</span></span>

### <a name="download-sample"></a><span data-ttu-id="c75ac-120">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="c75ac-120">Download sample</span></span>
<span data-ttu-id="c75ac-121">Můžete provést kroky v tomto článku nebo si můžete stáhnout vzorek, který obsahuje kód popsané v tomto článku [zde](https://azure.microsoft.com/documentation/samples/media-services-dotnet-copy-blob-into-asset/).</span><span class="sxs-lookup"><span data-stu-id="c75ac-121">You can follow the steps in this article or you can download a sample that contains the code described in this article from [here](https://azure.microsoft.com/documentation/samples/media-services-dotnet-copy-blob-into-asset/).</span></span>

### <a name="set-up-your-project"></a><span data-ttu-id="c75ac-122">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="c75ac-122">Set up your project</span></span>

1. <span data-ttu-id="c75ac-123">Nastavení vývojového prostředí, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="c75ac-123">Set up your development environment as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="c75ac-124">Přidejte další reference, které jsou požadovány pro tento projekt: System.Configuration.</span><span class="sxs-lookup"><span data-stu-id="c75ac-124">Add other references that are required for this project: System.Configuration.</span></span>
3. <span data-ttu-id="c75ac-125">Přidejte oddíl appSettings do souboru .config a aktualizujte hodnoty na základě svých účtů Media Services, cílový účet úložiště a ID zdroje asset.</span><span class="sxs-lookup"><span data-stu-id="c75ac-125">Add the appSettings section to the .config file and update the values based on your Media Services accounts, the destination storage account and the source asset ID.</span></span>  

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

### <a name="copy-blobs-from-an-asset-in-one-ams-account-into-an-asset-in-another-ams-account"></a><span data-ttu-id="c75ac-126">Kopírování objektů blob z prostředek v jednom účtu AMS do prostředku na jiném účtu AMS</span><span class="sxs-lookup"><span data-stu-id="c75ac-126">Copy blobs from an asset in one AMS account into an asset in another AMS account</span></span>

<span data-ttu-id="c75ac-127">Následující kód používá rozšíření **IAsset.Copy** metoda zkopíruje všechny soubory v prostředku zdrojového do cílového prostředku pomocí jedné rozšíření.</span><span class="sxs-lookup"><span data-stu-id="c75ac-127">The following code uses extension **IAsset.Copy** method to copy all files in the source asset into the destination asset using a single extension.</span></span>

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

            // Create the context for your source Media Services account.
            _sourceContext = new CloudMediaContext(new Uri(_sourceRESTAPIEndpoint), tokenProvider1);

            // Create the context for your destination Media Services account.
            _destContext = new CloudMediaContext(new Uri(_destRESTAPIEndpoint), tokenProvider2);

            // Get the credentials of the default Storage account bound to your destination Media Services account.
            StorageCredentials destinationStorageCredentials =
                new StorageCredentials(_destStorageAccountName, _destStorageAccountKey);

            // Get a reference to the source asset in the source context.
            IAsset sourceAsset = _sourceContext.Assets.Where(a => a.Id == _sourceAssetID).First();

            // Create an empty destination asset in the destination context.
            IAsset destinationAsset = _destContext.Assets.Create(sourceAsset.Name, AssetCreationOptions.None);

            // Copy the files in the source asset instance into the destination asset instance.
            sourceAsset.Copy(destinationAsset, destinationStorageCredentials);

            Console.WriteLine("Done");
        }
    }
}
```

## <a name="copy-blobs-from-a-storage-account-into-an-ams-account"></a><span data-ttu-id="c75ac-128">Kopírování objektů blob z účtu úložiště k účtu AMS</span><span class="sxs-lookup"><span data-stu-id="c75ac-128">Copy blobs from a storage account into an AMS account</span></span> 

### <a name="prerequisites"></a><span data-ttu-id="c75ac-129">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c75ac-129">Prerequisites</span></span>

- <span data-ttu-id="c75ac-130">Jeden účet úložiště, ze kterého chcete zkopírovat objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="c75ac-130">One Storage account from which you want to copy blobs.</span></span>
- <span data-ttu-id="c75ac-131">Jeden účet AMS, do kterého chcete zkopírovat objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="c75ac-131">One AMS account into which you want to copy blobs.</span></span>

### <a name="set-up-your-project"></a><span data-ttu-id="c75ac-132">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="c75ac-132">Set up your project</span></span>

1. <span data-ttu-id="c75ac-133">Nastavení vývojového prostředí, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="c75ac-133">Set up your development environment as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="c75ac-134">Přidejte další reference, které jsou požadovány pro tento projekt: System.Configuration.</span><span class="sxs-lookup"><span data-stu-id="c75ac-134">Add other references that are required for this project: System.Configuration.</span></span>
3. <span data-ttu-id="c75ac-135">Přidejte oddíl appSettings do souboru .config a aktualizujte hodnoty na základě vaší zdrojové úložiště a cílový účtů AMS.</span><span class="sxs-lookup"><span data-stu-id="c75ac-135">Add the appSettings section to the .config file and update the values based on your source storage and destination AMS accounts.</span></span>

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

### <a name="copy-blobs-from-some-storage-account-into-a-new-asset-in-a-ams-account"></a><span data-ttu-id="c75ac-136">Kopírování objektů blob z některé účtu úložiště do nového prostředku v účtu AMS</span><span class="sxs-lookup"><span data-stu-id="c75ac-136">Copy blobs from some storage account into a new asset in a AMS account</span></span>

<span data-ttu-id="c75ac-137">Následující kód zkopíruje objekty BLOB z účtu úložiště do asset Media Services.</span><span class="sxs-lookup"><span data-stu-id="c75ac-137">The following code copies blobs from a storage account into a Media Services asset.</span></span> 

>[!NOTE]
><span data-ttu-id="c75ac-138">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="c75ac-138">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="c75ac-139">Pokud vždy používáte stejné dny / přístupová oprávnění, například zásady pro lokátory, které mají zůstat na místě po dlouhou dobu (zásady bez odeslání), měli byste použít stejné ID zásad.</span><span class="sxs-lookup"><span data-stu-id="c75ac-139">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="c75ac-140">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="c75ac-140">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

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
        // Read values from the App.config file.
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

            // Create the context for your source Media Services account.
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

            // Get the asset container URI and Blob copy from mediaContainer to assetContainer. 
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

            // Set the primary asset file.
            // If, for example, we copied a set of Smooth Streaming files, 
            // set the .ism file to be the primary file. 
            // If we, for example, copied an .mp4, then the mp4 would be the primary file. 
            var ismAssetFile = asset.AssetFiles.ToList().
                Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).ToArray().FirstOrDefault();

            // The following code assigns the first .ism file as the primary file in the asset.
            // An asset should have one .ism file.  
            if (ismAssetFile != null)
            {
                ismAssetFile.IsPrimary = true;
                ismAssetFile.Update();
            }

            return asset;
        }

        /// <summary>
        /// Copies the specified blob into the specified container.
        /// </summary>
        /// <param name="sourceBlob">The source container.</param>
        /// <param name="destinationContainer">The destination container.</param>
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

                // Display the size of the source blob.
                Console.WriteLine(sourceBlob.Properties.Length);

                Console.WriteLine(string.Format("Copy blob '{0}' to '{1}'", sourceBlob.Uri, destinationBlob.Uri));
                destinationBlob.StartCopyFromBlob(new Uri(sourceBlob.Uri.AbsoluteUri + signature));

                while (true)
                {
                    // The StartCopyFromBlob is an async operation, 
                    // so we want to check if the copy operation is completed before proceeding. 
                    // To do that, we call FetchAttributes on the blob and check the CopyStatus. 
                    destinationBlob.FetchAttributes();
                    if (destinationBlob.CopyState.Status != CopyStatus.Pending)
                    {
                        break;
                    }
                    //It's still not completed. So wait for some time.
                    System.Threading.Thread.Sleep(1000);
                }

                // Display the size of the destination blob.
                Console.WriteLine(destinationBlob.Properties.Length);

            }
        }
    }
}
```
## <a name="next-steps"></a><span data-ttu-id="c75ac-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c75ac-141">Next steps</span></span>

<span data-ttu-id="c75ac-142">Nyní můžete kódovat nahrané assety.</span><span class="sxs-lookup"><span data-stu-id="c75ac-142">You can now encode your uploaded assets.</span></span> <span data-ttu-id="c75ac-143">Další informace najdete v tématu [Kódování assetů](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="c75ac-143">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="c75ac-144">Můžete také použít službu Azure Functions k aktivaci úlohy kódování při příchodu souboru do nakonfigurovaného kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c75ac-144">You can also use Azure Functions to trigger an encoding job based on a file arriving in the configured container.</span></span> <span data-ttu-id="c75ac-145">Další informace najdete v [této ukázce](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="c75ac-145">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="c75ac-146">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="c75ac-146">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c75ac-147">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="c75ac-147">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
