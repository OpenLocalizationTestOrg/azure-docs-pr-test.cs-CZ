---
title: "Nahrání souborů do účtu Media Services pomocí rozhraní .NET | Microsoft Docs"
description: "Další informace o získání mediálního obsahu ve službě Media Services pomocí vytvoření a odeslání prostředky."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: c9c86380-9395-4db8-acea-507c52066f73
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2017
ms.author: juliako
ms.openlocfilehash: ec8c1da633374ba684f6a0a895c542ee76ef73b8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="upload-files-into-a-media-services-account-using-net"></a><span data-ttu-id="50a6e-103">Nahrání souborů do účtu Media Services pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="50a6e-103">Upload files into a Media Services account using .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="50a6e-104">.NET</span><span class="sxs-lookup"><span data-stu-id="50a6e-104">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="50a6e-105">REST</span><span class="sxs-lookup"><span data-stu-id="50a6e-105">REST</span></span>](media-services-rest-upload-files.md)
> * [<span data-ttu-id="50a6e-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="50a6e-106">Portal</span></span>](media-services-portal-upload-files.md)
> 
> 

<span data-ttu-id="50a6e-107">Ve službě Media Services můžete digitální soubory nahrát (nebo ingestovat) do prostředku.</span><span class="sxs-lookup"><span data-stu-id="50a6e-107">In Media Services, you upload (or ingest) your digital files into an asset.</span></span> <span data-ttu-id="50a6e-108">**Asset** entita může obsahovat video, zvuk, obrázky, kolekci miniatur, text sleduje a titulků soubory (a metadata o těchto souborech.)  Jakmile soubory odešlete, bude váš obsah bezpečně uložen v cloudu pro další zpracování a streamování.</span><span class="sxs-lookup"><span data-stu-id="50a6e-108">The **Asset** entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and the metadata about these files.)  Once the files are uploaded, your content is stored securely in the cloud for further processing and streaming.</span></span>

<span data-ttu-id="50a6e-109">Soubory v prostředku se nazývají **soubory prostředku**.</span><span class="sxs-lookup"><span data-stu-id="50a6e-109">The files in the asset are called **Asset Files**.</span></span> <span data-ttu-id="50a6e-110">**AssetFile** instance a samotný mediální soubor jsou dva odlišné objekty.</span><span class="sxs-lookup"><span data-stu-id="50a6e-110">The **AssetFile** instance and the actual media file are two distinct objects.</span></span> <span data-ttu-id="50a6e-111">AssetFile instance obsahuje metadata o souboru média, zatímco souboru média obsahuje samotný mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="50a6e-111">The AssetFile instance contains metadata about the media file, while the media file contains the actual media content.</span></span>

> [!NOTE]
> <span data-ttu-id="50a6e-112">Platí následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="50a6e-112">The following considerations apply:</span></span>
> 
> * <span data-ttu-id="50a6e-113">Služba Media Services použije hodnotu vlastnosti IAssetFile.Name při sestavování adresy URL pro streamování obsah (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu není povoleno kódování v procentech.</span><span class="sxs-lookup"><span data-stu-id="50a6e-113">Media Services uses the value of the IAssetFile.Name property when building URLs for the streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="50a6e-114">Hodnota **název** vlastnost nemůže mít žádné z následujících [procent kódování vyhrazené znaky](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ".</span><span class="sxs-lookup"><span data-stu-id="50a6e-114">The value of the **Name** property cannot have any of the following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="50a6e-115">Navíc může existovat pouze jedna '.' pro příponu názvu souboru.</span><span class="sxs-lookup"><span data-stu-id="50a6e-115">Also, there can only be one '.' for the file name extension.</span></span>
> * <span data-ttu-id="50a6e-116">Délka názvu nesmí být větší než 260 znaků.</span><span class="sxs-lookup"><span data-stu-id="50a6e-116">The length of the name should not be greater than 260 characters.</span></span>
> * <span data-ttu-id="50a6e-117">Maximální velikost souboru podporovaná při zpracování ve službě Media Services je omezená.</span><span class="sxs-lookup"><span data-stu-id="50a6e-117">There is a limit to the maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="50a6e-118">Podrobnosti o omezení velikosti souboru najdete [tady](media-services-quotas-and-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="50a6e-118">Please see [this](media-services-quotas-and-limitations.md) topic for details about the file size limitation.</span></span>
> * <span data-ttu-id="50a6e-119">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="50a6e-119">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="50a6e-120">Pokud vždy používáte stejné dny / přístupová oprávnění, například zásady pro lokátory, které mají zůstat na místě po dlouhou dobu (zásady bez odeslání), měli byste použít stejné ID zásad.</span><span class="sxs-lookup"><span data-stu-id="50a6e-120">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="50a6e-121">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="50a6e-121">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>
> 

<span data-ttu-id="50a6e-122">Při vytváření prostředků, můžete zadat následující možnosti šifrování.</span><span class="sxs-lookup"><span data-stu-id="50a6e-122">When you create assets, you can specify the following encryption options.</span></span> 

* <span data-ttu-id="50a6e-123">**Žádné** – nepoužívá se žádné šifrování.</span><span class="sxs-lookup"><span data-stu-id="50a6e-123">**None** - No encryption is used.</span></span> <span data-ttu-id="50a6e-124">Toto je výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="50a6e-124">This is the default value.</span></span> <span data-ttu-id="50a6e-125">Všimněte si, že při použití této možnosti není váš obsah chráněný během přenosu ani umístěná v úložišti.</span><span class="sxs-lookup"><span data-stu-id="50a6e-125">Note that when using this option your content is not protected in transit or at rest in storage.</span></span>
  <span data-ttu-id="50a6e-126">Pokud chcete pomocí progresivního stahování dodávat obsah ve formátu MP4, použijte tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="50a6e-126">If you plan to deliver an MP4 using progressive download, use this option.</span></span> 
* <span data-ttu-id="50a6e-127">**CommonEncryption** – tuto možnost použijte, pokud nahráváte obsah, který byl zašifrován a chráněný běžným šifrováním nebo DRM s technologií PlayReady (například Smooth Streaming chráněná pomocí DRM s technologií PlayReady).</span><span class="sxs-lookup"><span data-stu-id="50a6e-127">**CommonEncryption** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="50a6e-128">**EnvelopeEncrypted** – tuto možnost použijte, pokud odesíláte HLS se šifrováním pomocí standardu AES.</span><span class="sxs-lookup"><span data-stu-id="50a6e-128">**EnvelopeEncrypted** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="50a6e-129">Pamatujte, že soubory musí být zakódované a zašifrované pomocí správce transformací.</span><span class="sxs-lookup"><span data-stu-id="50a6e-129">Note that the files must have been encoded and encrypted by Transform Manager.</span></span>
* <span data-ttu-id="50a6e-130">**StorageEncrypted** – šifruje vaše nešifrovaného obsahu pomocí 256bitového šifrování AES 256 a odešle ji do Azure Storage kde bude uložený v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="50a6e-130">**StorageEncrypted** - Encrypts your clear content locally using AES-256 bit encryption and then uploads it to Azure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="50a6e-131">Prostředky chráněné pomocí šifrování úložiště jsou před kódováním automaticky bez šifrování umístěny do systému souborů EFS a volitelně se znovu zašifrují před jejich odesláním zpět v podobě nového výstupního prostředku.</span><span class="sxs-lookup"><span data-stu-id="50a6e-131">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior to encoding, and optionally re-encrypted prior to uploading back as a new output asset.</span></span> <span data-ttu-id="50a6e-132">Případem primárního použití šifrování úložiště je, když chcete zabezpečit vysoké kvality souborů vstupními médii pomocí silného šifrování v klidovém stavu na disku.</span><span class="sxs-lookup"><span data-stu-id="50a6e-132">The primary use case for Storage Encryption is when you want to secure your high quality input media files with strong encryption at rest on disk.</span></span>
  
    <span data-ttu-id="50a6e-133">Služba Media Services poskytuje šifrování úložiště na disku pro vaše prostředky, ne přes přenosu jako správce digitální práv (DRM).</span><span class="sxs-lookup"><span data-stu-id="50a6e-133">Media Services provides on-disk storage encryption for your assets, not over-the-wire like Digital Rights Manager (DRM).</span></span>
  
    <span data-ttu-id="50a6e-134">Pokud váš asset používá šifrování úložiště, musíte nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="50a6e-134">If your asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="50a6e-135">Další informace najdete v části [konfigurace zásad doručení assetu](media-services-dotnet-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="50a6e-135">For more information see [Configuring asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md).</span></span>

<span data-ttu-id="50a6e-136">Pokud zadáte pro váš asset mají být šifrována pomocí **CommonEncrypted** možnost, nebo **EnvelopeEncypted** možnost, budete muset přiřadit asset s **ContentKey**.</span><span class="sxs-lookup"><span data-stu-id="50a6e-136">If you specify for your asset to be encrypted with a **CommonEncrypted** option, or an **EnvelopeEncypted** option, you will need to associate your asset with a **ContentKey**.</span></span> <span data-ttu-id="50a6e-137">Další informace najdete v tématu [postup vytvoření ContentKey](media-services-dotnet-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="50a6e-137">For more information, see [How to create a ContentKey](media-services-dotnet-create-contentkey.md).</span></span> 

<span data-ttu-id="50a6e-138">Pokud zadáte pro váš asset mají být šifrována pomocí **StorageEncrypted** možnost, sady Media Services SDK pro .NET vytvoří **StorateEncrypted** **ContentKey** pro vaše Asset.</span><span class="sxs-lookup"><span data-stu-id="50a6e-138">If you specify for your asset to be encrypted with a **StorageEncrypted** option, the Media Services SDK for .NET will create a **StorateEncrypted** **ContentKey** for your asset.</span></span>

<span data-ttu-id="50a6e-139">Toto téma ukazuje, jak používat sadu Media Services .NET SDK, jakož i rozšíření sady Media Services .NET SDK k nahrání souborů do asset Media Services.</span><span class="sxs-lookup"><span data-stu-id="50a6e-139">This topic shows how to use Media Services .NET SDK as well as Media Services .NET SDK extensions to upload files into a Media Services asset.</span></span>

## <a name="upload-a-single-file-with-media-services-net-sdk"></a><span data-ttu-id="50a6e-140">Nahrát jeden soubor pomocí sady Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="50a6e-140">Upload a single file with Media Services .NET SDK</span></span>
<span data-ttu-id="50a6e-141">Následující ukázkový kód používá .NET SDK k nahrát jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="50a6e-141">The sample code below uses .NET SDK to upload a single file.</span></span> <span data-ttu-id="50a6e-142">AccessPolicy a Lokátor vytvořen a zničen odesílání funkce.</span><span class="sxs-lookup"><span data-stu-id="50a6e-142">The AccessPolicy and Locator are created and destroyed by the Upload function.</span></span> 


        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }


## <a name="upload-multiple-files-with-media-services-net-sdk"></a><span data-ttu-id="50a6e-143">Uložení více souborů pomocí sady Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="50a6e-143">Upload multiple files with Media Services .NET SDK</span></span>
<span data-ttu-id="50a6e-144">Následující kód ukazuje, jak vytvořit prostředek a odeslat více souborů.</span><span class="sxs-lookup"><span data-stu-id="50a6e-144">The following code shows how to create an asset and upload multiple files.</span></span>

<span data-ttu-id="50a6e-145">Kód provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="50a6e-145">The code does the following:</span></span>

* <span data-ttu-id="50a6e-146">Vytvoří prázdný majetku pomocí metody CreateEmptyAsset definované v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="50a6e-146">Creates an empty asset using the CreateEmptyAsset method defined in the previous step.</span></span>
* <span data-ttu-id="50a6e-147">Vytvoří **AccessPolicy** instanci, která definuje oprávnění a doba trvání přístupu pro daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="50a6e-147">Creates an **AccessPolicy** instance that defines the permissions and duration of access to the asset.</span></span>
* <span data-ttu-id="50a6e-148">Vytvoří **Lokátor** instance, který poskytuje přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="50a6e-148">Creates a **Locator** instance that provides access to the asset.</span></span>
* <span data-ttu-id="50a6e-149">Vytvoří **BlobTransferClient** instance.</span><span class="sxs-lookup"><span data-stu-id="50a6e-149">Creates a **BlobTransferClient** instance.</span></span> <span data-ttu-id="50a6e-150">Tento typ reprezentuje klienta, který funguje na Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="50a6e-150">This type represents a client that operates on the Azure blobs.</span></span> <span data-ttu-id="50a6e-151">V tomto příkladu používáme klienta pro monitorování průběhu nahrávání.</span><span class="sxs-lookup"><span data-stu-id="50a6e-151">In this example we use the client to monitor the upload progress.</span></span> 
* <span data-ttu-id="50a6e-152">Vytvoří výčet prostřednictvím souborů v adresáři zadaný a vytvoří **AssetFile** instance pro každý soubor.</span><span class="sxs-lookup"><span data-stu-id="50a6e-152">Enumerates through files in the specified directory and creates an **AssetFile** instance for each file.</span></span>
* <span data-ttu-id="50a6e-153">Nahrávání souborů do aplikace pomocí služby Media Services **UploadAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="50a6e-153">Uploads the files into Media Services using the **UploadAsync** method.</span></span> 

> [!NOTE]
> <span data-ttu-id="50a6e-154">Pomocí této metody UploadAsync zajistěte, aby volání neblokují a soubory odešlete paralelně.</span><span class="sxs-lookup"><span data-stu-id="50a6e-154">Use the UploadAsync method to ensure that the calls are not blocking and the files are uploaded in parallel.</span></span>
> 
> 

        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }

    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



<span data-ttu-id="50a6e-155">Při nahrávání velký počet prostředků, zvažte následující.</span><span class="sxs-lookup"><span data-stu-id="50a6e-155">When uploading a large number of assets, consider the following.</span></span>

* <span data-ttu-id="50a6e-156">Vytvořte novou **CloudMediaContext** objektu na vlákno.</span><span class="sxs-lookup"><span data-stu-id="50a6e-156">Create a new **CloudMediaContext** object per thread.</span></span> <span data-ttu-id="50a6e-157">**CloudMediaContext** třída není bezpečná pro přístup z více vláken.</span><span class="sxs-lookup"><span data-stu-id="50a6e-157">The **CloudMediaContext** class is not thread safe.</span></span>
* <span data-ttu-id="50a6e-158">Zvýšit NumberOfConcurrentTransfers z výchozí hodnotu 2 na vyšší hodnotu, jako je 5.</span><span class="sxs-lookup"><span data-stu-id="50a6e-158">Increase NumberOfConcurrentTransfers from the default value of 2 to a higher value like 5.</span></span> <span data-ttu-id="50a6e-159">Nastavení této vlastnosti ovlivní všechny instance **CloudMediaContext**.</span><span class="sxs-lookup"><span data-stu-id="50a6e-159">Setting this property affects all instances of **CloudMediaContext**.</span></span> 
* <span data-ttu-id="50a6e-160">Zachovat ParallelTransferThreadCount na výchozí hodnotu 10.</span><span class="sxs-lookup"><span data-stu-id="50a6e-160">Keep ParallelTransferThreadCount at the default value of 10.</span></span>

## <span data-ttu-id="50a6e-161"><a id="ingest_in_bulk"></a>Příjem prostředky hromadně pomocí sady Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="50a6e-161"><a id="ingest_in_bulk"></a>Ingesting Assets in Bulk using Media Services .NET SDK</span></span>
<span data-ttu-id="50a6e-162">Nahrávání souborů velké prostředek může být kritický bod během vytváření asset.</span><span class="sxs-lookup"><span data-stu-id="50a6e-162">Uploading large asset files can be a bottleneck during asset creation.</span></span> <span data-ttu-id="50a6e-163">Příjem prostředky v hromadné nebo "Hromadné příjem", zahrnuje vytvoření prostředku z procesu nahrávání oddělení.</span><span class="sxs-lookup"><span data-stu-id="50a6e-163">Ingesting Assets in Bulk or “Bulk Ingesting”, involves decoupling asset creation from the upload process.</span></span> <span data-ttu-id="50a6e-164">Pokud chcete používat hromadné příjem přístup, vytvořte manifestu (IngestManifest), který popisuje asset a jeho přidružené soubory.</span><span class="sxs-lookup"><span data-stu-id="50a6e-164">To use a bulk ingesting approach, create a manifest (IngestManifest) that describes the asset and its associated files.</span></span> <span data-ttu-id="50a6e-165">Potom použijte metodu nahrávání podle svého výběru k nahrání přidružené soubory do kontejneru objektů blob v manifestu.</span><span class="sxs-lookup"><span data-stu-id="50a6e-165">Then use the upload method of your choice to upload the associated files to the manifest’s blob container.</span></span> <span data-ttu-id="50a6e-166">Microsoft Azure Media Services sleduje kontejneru objektů blob přidružený manifest.</span><span class="sxs-lookup"><span data-stu-id="50a6e-166">Microsoft Azure Media Services watches the blob container associated with the manifest.</span></span> <span data-ttu-id="50a6e-167">Po odeslání souboru do kontejneru objektů blob Microsoft Azure Media Services dokončení vytvoření prostředku, na základě konfigurace prostředku manifestu (IngestManifestAsset).</span><span class="sxs-lookup"><span data-stu-id="50a6e-167">Once a file is uploaded to the blob container, Microsoft Azure Media Services completes the asset creation based on the configuration of the asset in the manifest (IngestManifestAsset).</span></span>

<span data-ttu-id="50a6e-168">Chcete-li vytvořit nové volání IngestManifest vystavené kolekci IngestManifests na CloudMediaContext metodu Create.</span><span class="sxs-lookup"><span data-stu-id="50a6e-168">To create a new IngestManifest call the Create method exposed by the IngestManifests collection on the CloudMediaContext.</span></span> <span data-ttu-id="50a6e-169">Tato metoda vytvoří nový IngestManifest manifestu názvem, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="50a6e-169">This method will create a new IngestManifest with the manifest name you provide.</span></span>

    IIngestManifest manifest = context.IngestManifests.Create(name);

<span data-ttu-id="50a6e-170">Vytvořte prostředky, které budou přidruženy k hromadné IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="50a6e-170">Create the assets that will be associated with the bulk IngestManifest.</span></span> <span data-ttu-id="50a6e-171">Konfigurujte možnosti požadované šifrování na asset pro příjem hromadně.</span><span class="sxs-lookup"><span data-stu-id="50a6e-171">Configure the desired encryption options on the asset for bulk ingesting.</span></span>

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

<span data-ttu-id="50a6e-172">IngestManifestAsset přidruží hromadné IngestManifest pro příjem hromadné prostředek.</span><span class="sxs-lookup"><span data-stu-id="50a6e-172">An IngestManifestAsset associates an Asset with a bulk IngestManifest for bulk ingesting.</span></span> <span data-ttu-id="50a6e-173">Také přidruží AssetFiles, které budou použity k vytvoření každého prostředku.</span><span class="sxs-lookup"><span data-stu-id="50a6e-173">It also associates the AssetFiles that will make up each Asset.</span></span> <span data-ttu-id="50a6e-174">Pokud chcete vytvořit IngestManifestAsset, použijte metodu Create na kontext serveru.</span><span class="sxs-lookup"><span data-stu-id="50a6e-174">To create an IngestManifestAsset, use the Create method on the server context.</span></span>

<span data-ttu-id="50a6e-175">Následující příklad ukazuje, přidávání dvě nové IngestManifestAssets, které spojují dva prostředky předtím vytvořili pro hromadným ingestování manifestu.</span><span class="sxs-lookup"><span data-stu-id="50a6e-175">The following example demonstrates adding two new IngestManifestAssets that associate the two assets previously created to the bulk ingest manifest.</span></span> <span data-ttu-id="50a6e-176">Každý IngestManifestAsset také přidruží sadu souborů, které budou odeslány, pro každý prostředek během hromadné příjem.</span><span class="sxs-lookup"><span data-stu-id="50a6e-176">Each IngestManifestAsset also associates a set of files that will be uploaded for each asset during bulk ingesting.</span></span>  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });

<span data-ttu-id="50a6e-177">Můžete použít libovolná aplikace klienta vysokorychlostní schopná nahrávání souborů asset ke kontejneru úložiště objektů blob URI poskytované **IIngestManifest.BlobStorageUriForUpload** vlastnost IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="50a6e-177">You can use any high speed client application capable of uploading the asset files to the blob storage container URI provided by the **IIngestManifest.BlobStorageUriForUpload** property of the IngestManifest.</span></span> <span data-ttu-id="50a6e-178">Je jedna služba nahrávání významné vysokorychlostní [Aspera na vyžádání pro aplikaci Azure](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6).</span><span class="sxs-lookup"><span data-stu-id="50a6e-178">One notable high speed upload service is [Aspera On Demand for Azure Application](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6).</span></span> <span data-ttu-id="50a6e-179">Můžete taky napsat kód k nahrání souborů prostředků, jak je znázorněno v následujícím příkladu kódu.</span><span class="sxs-lookup"><span data-stu-id="50a6e-179">You can also write code to upload the assets files as shown in the following code example.</span></span>

    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);

            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);

            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);

            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });

        copytask.Start();
    }

<span data-ttu-id="50a6e-180">V následujícím příkladu kódu se zobrazí kód pro nahrávání souborů asset pro ukázku použitým v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="50a6e-180">The code for uploading the asset files for the sample used in this topic is shown in the following code example.</span></span>

    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);


<span data-ttu-id="50a6e-181">Můžete určit průběh hromadné příjem pro všechny prostředky přidružené **IngestManifest** pomocí cyklického dotazování vlastnost statistiky **IngestManifest**.</span><span class="sxs-lookup"><span data-stu-id="50a6e-181">You can determine the progress of the bulk ingesting for all assets associated with an **IngestManifest** by polling the Statistics property of the **IngestManifest**.</span></span> <span data-ttu-id="50a6e-182">Chcete-li aktualizovat informace o průběhu, je nutné použít novou **CloudMediaContext** pokaždé, když dotazovat vlastnost statistiky.</span><span class="sxs-lookup"><span data-stu-id="50a6e-182">In order to update progress information, you must use a new **CloudMediaContext** each time you poll the Statistics property.</span></span>

<span data-ttu-id="50a6e-183">Následující příklad ukazuje, dotazování IngestManifest podle jeho **Id**.</span><span class="sxs-lookup"><span data-stu-id="50a6e-183">The following example demonstrates polling an IngestManifest by its **Id**.</span></span>

    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();

          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);

                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }

             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }



## <a name="upload-files-using-net-sdk-extensions"></a><span data-ttu-id="50a6e-184">Nahrát soubory pomocí rozšíření sady SDK pro .NET</span><span class="sxs-lookup"><span data-stu-id="50a6e-184">Upload files using .NET SDK Extensions</span></span>
<span data-ttu-id="50a6e-185">Následující příklad ukazuje, jak nahrát jeden soubor pomocí rozšíření sady SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="50a6e-185">The example below shows how to upload a single file using .NET SDK Extensions.</span></span> <span data-ttu-id="50a6e-186">V takovém případě **CreateFromFile** metoda se používá, ale o asynchronní verzi je také k dispozici (**CreateFromFileAsync**).</span><span class="sxs-lookup"><span data-stu-id="50a6e-186">In this case the **CreateFromFile** method is used, but the asynchronous version is also available (**CreateFromFileAsync**).</span></span> <span data-ttu-id="50a6e-187">**CreateFromFile** metoda můžete zadat název souboru, možnost šifrování a zpětného volání za účelem hlášení průběhu odesílání souboru.</span><span class="sxs-lookup"><span data-stu-id="50a6e-187">The **CreateFromFile** method lets you specify the file name, encryption option, and a callback in order to report the upload progress of the file.</span></span>

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }

<span data-ttu-id="50a6e-188">V následujícím příkladu volání funkce UploadFile a určuje šifrování úložiště jako možnost vytvoření prostředku.</span><span class="sxs-lookup"><span data-stu-id="50a6e-188">The following example calls UploadFile function and specifies storage encryption as the asset creation option.</span></span>  

    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);

## <a name="next-steps"></a><span data-ttu-id="50a6e-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="50a6e-189">Next steps</span></span>

<span data-ttu-id="50a6e-190">Nyní můžete kódovat nahrané assety.</span><span class="sxs-lookup"><span data-stu-id="50a6e-190">You can now encode your uploaded assets.</span></span> <span data-ttu-id="50a6e-191">Další informace najdete v tématu [Kódování assetů](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="50a6e-191">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="50a6e-192">Můžete také použít službu Azure Functions k aktivaci úlohy kódování při příchodu souboru do nakonfigurovaného kontejneru.</span><span class="sxs-lookup"><span data-stu-id="50a6e-192">You can also use Azure Functions to trigger an encoding job based on a file arriving in the configured container.</span></span> <span data-ttu-id="50a6e-193">Další informace najdete v [této ukázce](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="50a6e-193">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="50a6e-194">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="50a6e-194">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="50a6e-195">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="50a6e-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="50a6e-196">Další krok</span><span class="sxs-lookup"><span data-stu-id="50a6e-196">Next step</span></span>
<span data-ttu-id="50a6e-197">Teď, když jste nahráli prostředek ke službě Media Services, přejděte na [jak získat procesor médií] [ How to Get a Media Processor] tématu.</span><span class="sxs-lookup"><span data-stu-id="50a6e-197">Now that you have uploaded an asset to Media Services, go to the [How to Get a Media Processor][How to Get a Media Processor] topic.</span></span>

[How to Get a Media Processor]: media-services-get-media-processor.md

