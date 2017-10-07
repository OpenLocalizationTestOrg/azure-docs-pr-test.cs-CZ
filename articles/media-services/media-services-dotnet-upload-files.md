---
title: "aaaUpload souborů do účtu Media Services pomocí rozhraní .NET | Microsoft Docs"
description: "Zjistěte, jak tooget média obsahu ve službě Media Services pomocí vytvoření a odeslání prostředky."
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
ms.openlocfilehash: 11c8a359b09efe04b54490fd48ac0cd7c366f8b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-net"></a><span data-ttu-id="0652a-103">Nahrání souborů do účtu Media Services pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="0652a-103">Upload files into a Media Services account using .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0652a-104">.NET</span><span class="sxs-lookup"><span data-stu-id="0652a-104">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="0652a-105">REST</span><span class="sxs-lookup"><span data-stu-id="0652a-105">REST</span></span>](media-services-rest-upload-files.md)
> * [<span data-ttu-id="0652a-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0652a-106">Portal</span></span>](media-services-portal-upload-files.md)
> 
> 

<span data-ttu-id="0652a-107">Ve službě Media Services můžete digitální soubory nahrát (nebo ingestovat) do prostředku.</span><span class="sxs-lookup"><span data-stu-id="0652a-107">In Media Services, you upload (or ingest) your digital files into an asset.</span></span> <span data-ttu-id="0652a-108">Hello **Asset** entita může obsahovat video, zvuk, obrázky, kolekci miniatur, text sleduje a titulků soubory (a hello metadata o těchto souborech.)  Jakmile hello soubory jsou odeslány, váš obsah bezpečně uložen v hello cloudu pro další zpracování a streamování.</span><span class="sxs-lookup"><span data-stu-id="0652a-108">hello **Asset** entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.)  Once hello files are uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span>

<span data-ttu-id="0652a-109">soubory Hello v hello prostředku se nazývají **soubory prostředku**.</span><span class="sxs-lookup"><span data-stu-id="0652a-109">hello files in hello asset are called **Asset Files**.</span></span> <span data-ttu-id="0652a-110">Hello **AssetFile** instance a hello samotný mediální soubor jsou dva odlišné objekty.</span><span class="sxs-lookup"><span data-stu-id="0652a-110">hello **AssetFile** instance and hello actual media file are two distinct objects.</span></span> <span data-ttu-id="0652a-111">Hello AssetFile instance obsahuje metadata o hello soubor média, zatímco soubor média hello obsahuje hello samotný mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="0652a-111">hello AssetFile instance contains metadata about hello media file, while hello media file contains hello actual media content.</span></span>

> [!NOTE]
> <span data-ttu-id="0652a-112">použít Hello následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="0652a-112">hello following considerations apply:</span></span>
> 
> * <span data-ttu-id="0652a-113">Služba Media Services použije hello hodnotu hello IAssetFile.Name vlastnost při sestavování adresy URL pro hello streamování obsahu (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu není povoleno kódování v procentech.</span><span class="sxs-lookup"><span data-stu-id="0652a-113">Media Services uses hello value of hello IAssetFile.Name property when building URLs for hello streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="0652a-114">Hello hodnotu hello **název** vlastnost nemůže mít žádné z následujících hello [procent kódování vyhrazené znaky](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ".</span><span class="sxs-lookup"><span data-stu-id="0652a-114">hello value of hello **Name** property cannot have any of hello following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="0652a-115">Navíc může existovat pouze jedna '.' pro příponu názvu souboru hello.</span><span class="sxs-lookup"><span data-stu-id="0652a-115">Also, there can only be one '.' for hello file name extension.</span></span>
> * <span data-ttu-id="0652a-116">Délka Hello hello názvu by neměla být větší než 260 znaků.</span><span class="sxs-lookup"><span data-stu-id="0652a-116">hello length of hello name should not be greater than 260 characters.</span></span>
> * <span data-ttu-id="0652a-117">Existuje limit toohello maximální velikost souboru pro zpracování ve službě Media Services podporována.</span><span class="sxs-lookup"><span data-stu-id="0652a-117">There is a limit toohello maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="0652a-118">Najdete v tématu [to](media-services-quotas-and-limitations.md) téma podrobné informace o omezení velikosti souborů hello.</span><span class="sxs-lookup"><span data-stu-id="0652a-118">Please see [this](media-services-quotas-and-limitations.md) topic for details about hello file size limitation.</span></span>
> * <span data-ttu-id="0652a-119">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="0652a-119">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="0652a-120">Měli byste použít hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění, například zásady pro lokátory, které jsou určený tooremain zavedené po dlouhou dobu (bez odeslání zásady).</span><span class="sxs-lookup"><span data-stu-id="0652a-120">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="0652a-121">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="0652a-121">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>
> 

<span data-ttu-id="0652a-122">Při vytváření prostředků, můžete zadat následující možnosti šifrování hello.</span><span class="sxs-lookup"><span data-stu-id="0652a-122">When you create assets, you can specify hello following encryption options.</span></span> 

* <span data-ttu-id="0652a-123">**Žádné** – nepoužívá se žádné šifrování.</span><span class="sxs-lookup"><span data-stu-id="0652a-123">**None** - No encryption is used.</span></span> <span data-ttu-id="0652a-124">Toto je výchozí hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="0652a-124">This is hello default value.</span></span> <span data-ttu-id="0652a-125">Všimněte si, že při použití této možnosti není váš obsah chráněný během přenosu ani umístěná v úložišti.</span><span class="sxs-lookup"><span data-stu-id="0652a-125">Note that when using this option your content is not protected in transit or at rest in storage.</span></span>
  <span data-ttu-id="0652a-126">Tuto možnost použijte, pokud máte v plánu toodeliver MP4 pomocí progresivního stahování.</span><span class="sxs-lookup"><span data-stu-id="0652a-126">If you plan toodeliver an MP4 using progressive download, use this option.</span></span> 
* <span data-ttu-id="0652a-127">**CommonEncryption** – tuto možnost použijte, pokud nahráváte obsah, který byl zašifrován a chráněný běžným šifrováním nebo DRM s technologií PlayReady (například Smooth Streaming chráněná pomocí DRM s technologií PlayReady).</span><span class="sxs-lookup"><span data-stu-id="0652a-127">**CommonEncryption** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="0652a-128">**EnvelopeEncrypted** – tuto možnost použijte, pokud odesíláte HLS se šifrováním pomocí standardu AES.</span><span class="sxs-lookup"><span data-stu-id="0652a-128">**EnvelopeEncrypted** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="0652a-129">Všimněte si, že hello soubory musí být kódovaný a zašifrované pomocí Správce transformací.</span><span class="sxs-lookup"><span data-stu-id="0652a-129">Note that hello files must have been encoded and encrypted by Transform Manager.</span></span>
* <span data-ttu-id="0652a-130">**StorageEncrypted** – šifruje vaše nešifrovaného obsahu pomocí 256bitového šifrování AES 256 a odešle ho tooAzure úložiště, kde je uložený v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="0652a-130">**StorageEncrypted** - Encrypts your clear content locally using AES-256 bit encryption and then uploads it tooAzure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="0652a-131">Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování a umístit do předchozí tooencoding systému souborů EFS a volitelně znovu zašifrovat předchozí toouploading zpět v podobě nového výstupního prostředku.</span><span class="sxs-lookup"><span data-stu-id="0652a-131">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior tooencoding, and optionally re-encrypted prior toouploading back as a new output asset.</span></span> <span data-ttu-id="0652a-132">Hello případem primárního použití šifrování úložiště je, pokud chcete toosecure rest souborů vysoké kvality vstupními médii pomocí silného šifrování na disku.</span><span class="sxs-lookup"><span data-stu-id="0652a-132">hello primary use case for Storage Encryption is when you want toosecure your high quality input media files with strong encryption at rest on disk.</span></span>
  
    <span data-ttu-id="0652a-133">Služba Media Services poskytuje šifrování úložiště na disku pro vaše prostředky, ne přes přenosu jako správce digitální práv (DRM).</span><span class="sxs-lookup"><span data-stu-id="0652a-133">Media Services provides on-disk storage encryption for your assets, not over-the-wire like Digital Rights Manager (DRM).</span></span>
  
    <span data-ttu-id="0652a-134">Pokud váš asset používá šifrování úložiště, musíte nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="0652a-134">If your asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="0652a-135">Další informace najdete v části [konfigurace zásad doručení assetu](media-services-dotnet-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="0652a-135">For more information see [Configuring asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md).</span></span>

<span data-ttu-id="0652a-136">Pokud zadáte pro váš asset toobe šifrován **CommonEncrypted** možnost, nebo **EnvelopeEncypted** možnost, budete potřebovat tooassociate asset s **ContentKey**.</span><span class="sxs-lookup"><span data-stu-id="0652a-136">If you specify for your asset toobe encrypted with a **CommonEncrypted** option, or an **EnvelopeEncypted** option, you will need tooassociate your asset with a **ContentKey**.</span></span> <span data-ttu-id="0652a-137">Další informace najdete v tématu [jak toocreate ContentKey](media-services-dotnet-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="0652a-137">For more information, see [How toocreate a ContentKey](media-services-dotnet-create-contentkey.md).</span></span> 

<span data-ttu-id="0652a-138">Pokud zadáte pro váš asset toobe šifrován **StorageEncrypted** možnost, hello sady Media Services SDK pro .NET vytvoří **StorateEncrypted** **ContentKey** pro vaše Asset.</span><span class="sxs-lookup"><span data-stu-id="0652a-138">If you specify for your asset toobe encrypted with a **StorageEncrypted** option, hello Media Services SDK for .NET will create a **StorateEncrypted** **ContentKey** for your asset.</span></span>

<span data-ttu-id="0652a-139">Toto téma ukazuje, jak toouse Media Services .NET SDK, jakož i sady Media Services .NET SDK rozšíření tooupload soubory do asset Media Services.</span><span class="sxs-lookup"><span data-stu-id="0652a-139">This topic shows how toouse Media Services .NET SDK as well as Media Services .NET SDK extensions tooupload files into a Media Services asset.</span></span>

## <a name="upload-a-single-file-with-media-services-net-sdk"></a><span data-ttu-id="0652a-140">Nahrát jeden soubor pomocí sady Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="0652a-140">Upload a single file with Media Services .NET SDK</span></span>
<span data-ttu-id="0652a-141">Hello následující ukázkový kód používá .NET SDK tooupload jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="0652a-141">hello sample code below uses .NET SDK tooupload a single file.</span></span> <span data-ttu-id="0652a-142">Hello AccessPolicy a Lokátor vytvořen a zničen funkcí nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="0652a-142">hello AccessPolicy and Locator are created and destroyed by hello Upload function.</span></span> 


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


## <a name="upload-multiple-files-with-media-services-net-sdk"></a><span data-ttu-id="0652a-143">Uložení více souborů pomocí sady Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="0652a-143">Upload multiple files with Media Services .NET SDK</span></span>
<span data-ttu-id="0652a-144">Následující kód ukazuje, jak Hello toocreate prostředek a uložení více souborů.</span><span class="sxs-lookup"><span data-stu-id="0652a-144">hello following code shows how toocreate an asset and upload multiple files.</span></span>

<span data-ttu-id="0652a-145">Kód Hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="0652a-145">hello code does hello following:</span></span>

* <span data-ttu-id="0652a-146">Vytvoří prázdný majetku pomocí metody CreateEmptyAsset hello definované v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="0652a-146">Creates an empty asset using hello CreateEmptyAsset method defined in hello previous step.</span></span>
* <span data-ttu-id="0652a-147">Vytvoří **AccessPolicy** instanci, která definuje hello oprávnění a dobu trvání asset toohello přístup.</span><span class="sxs-lookup"><span data-stu-id="0652a-147">Creates an **AccessPolicy** instance that defines hello permissions and duration of access toohello asset.</span></span>
* <span data-ttu-id="0652a-148">Vytvoří **Lokátor** instanci, která poskytuje přístup toohello asset.</span><span class="sxs-lookup"><span data-stu-id="0652a-148">Creates a **Locator** instance that provides access toohello asset.</span></span>
* <span data-ttu-id="0652a-149">Vytvoří **BlobTransferClient** instance.</span><span class="sxs-lookup"><span data-stu-id="0652a-149">Creates a **BlobTransferClient** instance.</span></span> <span data-ttu-id="0652a-150">Tento typ reprezentuje klienta, který funguje na hello objektů BLOB Azure.</span><span class="sxs-lookup"><span data-stu-id="0652a-150">This type represents a client that operates on hello Azure blobs.</span></span> <span data-ttu-id="0652a-151">V tomto příkladu používáme průběhu odesílání hello klienta toomonitor hello.</span><span class="sxs-lookup"><span data-stu-id="0652a-151">In this example we use hello client toomonitor hello upload progress.</span></span> 
* <span data-ttu-id="0652a-152">Vytvoří výčet prostřednictvím určeného adresáře hello soubory a vytvoří **AssetFile** instance pro každý soubor.</span><span class="sxs-lookup"><span data-stu-id="0652a-152">Enumerates through files in hello specified directory and creates an **AssetFile** instance for each file.</span></span>
* <span data-ttu-id="0652a-153">Nahrávání hello soubory ve službě Media Services pomocí hello **UploadAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="0652a-153">Uploads hello files into Media Services using hello **UploadAsync** method.</span></span> 

> [!NOTE]
> <span data-ttu-id="0652a-154">Použijte tooensure hello UploadAsync metoda, která hello volání neblokují a hello soubory jsou odeslány paralelně.</span><span class="sxs-lookup"><span data-stu-id="0652a-154">Use hello UploadAsync method tooensure that hello calls are not blocking and hello files are uploaded in parallel.</span></span>
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

                // It is recommended toovalidate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading hello files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }

    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as hello upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



<span data-ttu-id="0652a-155">Při nahrávání velký počet prostředků, zvažte následující hello.</span><span class="sxs-lookup"><span data-stu-id="0652a-155">When uploading a large number of assets, consider hello following.</span></span>

* <span data-ttu-id="0652a-156">Vytvořte novou **CloudMediaContext** objektu na vlákno.</span><span class="sxs-lookup"><span data-stu-id="0652a-156">Create a new **CloudMediaContext** object per thread.</span></span> <span data-ttu-id="0652a-157">Hello **CloudMediaContext** třída není bezpečná pro přístup z více vláken.</span><span class="sxs-lookup"><span data-stu-id="0652a-157">hello **CloudMediaContext** class is not thread safe.</span></span>
* <span data-ttu-id="0652a-158">Zvýšit NumberOfConcurrentTransfers z hello výchozí hodnotu 2 vyšší hodnota tooa, jako je 5.</span><span class="sxs-lookup"><span data-stu-id="0652a-158">Increase NumberOfConcurrentTransfers from hello default value of 2 tooa higher value like 5.</span></span> <span data-ttu-id="0652a-159">Nastavení této vlastnosti ovlivní všechny instance **CloudMediaContext**.</span><span class="sxs-lookup"><span data-stu-id="0652a-159">Setting this property affects all instances of **CloudMediaContext**.</span></span> 
* <span data-ttu-id="0652a-160">Zachovat ParallelTransferThreadCount v hello výchozí hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="0652a-160">Keep ParallelTransferThreadCount at hello default value of 10.</span></span>

## <span data-ttu-id="0652a-161"><a id="ingest_in_bulk"></a>Příjem prostředky hromadně pomocí sady Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="0652a-161"><a id="ingest_in_bulk"></a>Ingesting Assets in Bulk using Media Services .NET SDK</span></span>
<span data-ttu-id="0652a-162">Nahrávání souborů velké prostředek může být kritický bod během vytváření asset.</span><span class="sxs-lookup"><span data-stu-id="0652a-162">Uploading large asset files can be a bottleneck during asset creation.</span></span> <span data-ttu-id="0652a-163">Příjem prostředky v hromadné nebo "Hromadné příjem", zahrnuje oddělení asset vytvoření z procesu nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="0652a-163">Ingesting Assets in Bulk or “Bulk Ingesting”, involves decoupling asset creation from hello upload process.</span></span> <span data-ttu-id="0652a-164">toouse hromadné ingesting přístup, vytvořte manifestu (IngestManifest), který popisuje hello asset a jeho přidružené soubory.</span><span class="sxs-lookup"><span data-stu-id="0652a-164">toouse a bulk ingesting approach, create a manifest (IngestManifest) that describes hello asset and its associated files.</span></span> <span data-ttu-id="0652a-165">Pak použijte hello nahrávání metodu výběru tooupload hello přidružené soubory toohello manifest pro kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="0652a-165">Then use hello upload method of your choice tooupload hello associated files toohello manifest’s blob container.</span></span> <span data-ttu-id="0652a-166">Microsoft Azure Media Services sleduje kontejneru objektů blob hello přidruženého k manifestu hello.</span><span class="sxs-lookup"><span data-stu-id="0652a-166">Microsoft Azure Media Services watches hello blob container associated with hello manifest.</span></span> <span data-ttu-id="0652a-167">Kontejner objektů blob nahrané toohello po soubor Microsoft Azure Media Services dokončí vytváření asset hello na základě konfigurace hello hello majetku v manifestu hello (IngestManifestAsset).</span><span class="sxs-lookup"><span data-stu-id="0652a-167">Once a file is uploaded toohello blob container, Microsoft Azure Media Services completes hello asset creation based on hello configuration of hello asset in hello manifest (IngestManifestAsset).</span></span>

<span data-ttu-id="0652a-168">toocreate nové IngestManifest volat metodu Create hello vystavené hello IngestManifests kolekce na hello CloudMediaContext.</span><span class="sxs-lookup"><span data-stu-id="0652a-168">toocreate a new IngestManifest call hello Create method exposed by hello IngestManifests collection on hello CloudMediaContext.</span></span> <span data-ttu-id="0652a-169">Tato metoda vytvoří nový IngestManifest hello manifestu názvem, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="0652a-169">This method will create a new IngestManifest with hello manifest name you provide.</span></span>

    IIngestManifest manifest = context.IngestManifests.Create(name);

<span data-ttu-id="0652a-170">Vytvořte hello prostředky, které budou přidruženy k hromadné hello IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="0652a-170">Create hello assets that will be associated with hello bulk IngestManifest.</span></span> <span data-ttu-id="0652a-171">Konfigurujte možnosti šifrování hello potřeby na hello asset pro příjem hromadně.</span><span class="sxs-lookup"><span data-stu-id="0652a-171">Configure hello desired encryption options on hello asset for bulk ingesting.</span></span>

    // Create hello assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

<span data-ttu-id="0652a-172">IngestManifestAsset přidruží hromadné IngestManifest pro příjem hromadné prostředek.</span><span class="sxs-lookup"><span data-stu-id="0652a-172">An IngestManifestAsset associates an Asset with a bulk IngestManifest for bulk ingesting.</span></span> <span data-ttu-id="0652a-173">Také přidruží hello AssetFiles, které budou použity k vytvoření každého prostředku.</span><span class="sxs-lookup"><span data-stu-id="0652a-173">It also associates hello AssetFiles that will make up each Asset.</span></span> <span data-ttu-id="0652a-174">toocreate IngestManifestAsset, použijte metodu Create hello v kontextu server hello.</span><span class="sxs-lookup"><span data-stu-id="0652a-174">toocreate an IngestManifestAsset, use hello Create method on hello server context.</span></span>

<span data-ttu-id="0652a-175">Hello následující příklad ukazuje přidání dva nové IngestManifestAssets, které spojují hello dva prostředky vytvořili hromadné toohello ingestování manifestu.</span><span class="sxs-lookup"><span data-stu-id="0652a-175">hello following example demonstrates adding two new IngestManifestAssets that associate hello two assets previously created toohello bulk ingest manifest.</span></span> <span data-ttu-id="0652a-176">Každý IngestManifestAsset také přidruží sadu souborů, které budou odeslány, pro každý prostředek během hromadné příjem.</span><span class="sxs-lookup"><span data-stu-id="0652a-176">Each IngestManifestAsset also associates a set of files that will be uploaded for each asset during bulk ingesting.</span></span>  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });

<span data-ttu-id="0652a-177">Můžete použít libovolná aplikace klienta vysokorychlostní schopná odesílání hello asset soubory toohello kontejner úložiště objektů blob URI poskytované hello **IIngestManifest.BlobStorageUriForUpload** vlastnost hello IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="0652a-177">You can use any high speed client application capable of uploading hello asset files toohello blob storage container URI provided by hello **IIngestManifest.BlobStorageUriForUpload** property of hello IngestManifest.</span></span> <span data-ttu-id="0652a-178">Je jedna služba nahrávání významné vysokorychlostní [Aspera na vyžádání pro aplikaci Azure](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6).</span><span class="sxs-lookup"><span data-stu-id="0652a-178">One notable high speed upload service is [Aspera On Demand for Azure Application](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6).</span></span> <span data-ttu-id="0652a-179">Je také možné zapsat kód tooupload hello prostředky soubory jak ukazuje následující příklad kódu hello.</span><span class="sxs-lookup"><span data-stu-id="0652a-179">You can also write code tooupload hello assets files as shown in hello following code example.</span></span>

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

<span data-ttu-id="0652a-180">Hello kód pro nahrávání souborů hello asset pro ukázku hello použitým v tomto tématu je uveden v hello následující ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="0652a-180">hello code for uploading hello asset files for hello sample used in this topic is shown in hello following code example.</span></span>

    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);


<span data-ttu-id="0652a-181">Můžete určit hello průběh hello hromadné příjem pro všechny prostředky přidružené **IngestManifest** pomocí cyklického dotazování hello statistiky vlastnost hello **IngestManifest**.</span><span class="sxs-lookup"><span data-stu-id="0652a-181">You can determine hello progress of hello bulk ingesting for all assets associated with an **IngestManifest** by polling hello Statistics property of hello **IngestManifest**.</span></span> <span data-ttu-id="0652a-182">V pořadí informace o průběhu tooupdate, je třeba použít novou **CloudMediaContext** pokaždé, když dotazovat vlastnost statistiky hello.</span><span class="sxs-lookup"><span data-stu-id="0652a-182">In order tooupdate progress information, you must use a new **CloudMediaContext** each time you poll hello Statistics property.</span></span>

<span data-ttu-id="0652a-183">Hello následující příklad ukazuje, dotazování IngestManifest podle jeho **Id**.</span><span class="sxs-lookup"><span data-stu-id="0652a-183">hello following example demonstrates polling an IngestManifest by its **Id**.</span></span>

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



## <a name="upload-files-using-net-sdk-extensions"></a><span data-ttu-id="0652a-184">Nahrát soubory pomocí rozšíření sady SDK pro .NET</span><span class="sxs-lookup"><span data-stu-id="0652a-184">Upload files using .NET SDK Extensions</span></span>
<span data-ttu-id="0652a-185">Následující příklad Hello ukazuje, jak tooupload jednu souboru pomocí rozšíření sady SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="0652a-185">hello example below shows how tooupload a single file using .NET SDK Extensions.</span></span> <span data-ttu-id="0652a-186">V takovém případě hello **CreateFromFile** metoda se používá, ale je k dispozici také asynchronní verzi hello (**CreateFromFileAsync**).</span><span class="sxs-lookup"><span data-stu-id="0652a-186">In this case hello **CreateFromFile** method is used, but hello asynchronous version is also available (**CreateFromFileAsync**).</span></span> <span data-ttu-id="0652a-187">Hello **CreateFromFile** metoda slouží k určení názvu souboru text hello, možnost šifrování a zpětného volání v pořadí tooreport hello nahrát průběh hello souboru.</span><span class="sxs-lookup"><span data-stu-id="0652a-187">hello **CreateFromFile** method lets you specify hello file name, encryption option, and a callback in order tooreport hello upload progress of hello file.</span></span>

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

<span data-ttu-id="0652a-188">Hello následující příklad volá funkci UploadFile a určuje šifrování úložiště jako možnost vytvoření asset hello.</span><span class="sxs-lookup"><span data-stu-id="0652a-188">hello following example calls UploadFile function and specifies storage encryption as hello asset creation option.</span></span>  

    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);

## <a name="next-steps"></a><span data-ttu-id="0652a-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0652a-189">Next steps</span></span>

<span data-ttu-id="0652a-190">Nyní můžete kódovat nahrané assety.</span><span class="sxs-lookup"><span data-stu-id="0652a-190">You can now encode your uploaded assets.</span></span> <span data-ttu-id="0652a-191">Další informace najdete v tématu [Kódování assetů](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="0652a-191">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="0652a-192">Můžete také použít Azure Functions tootrigger úlohu kódování na základě souboru přicházejících do kontejneru hello nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="0652a-192">You can also use Azure Functions tootrigger an encoding job based on a file arriving in hello configured container.</span></span> <span data-ttu-id="0652a-193">Další informace najdete v [této ukázce](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="0652a-193">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="0652a-194">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="0652a-194">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0652a-195">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="0652a-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="0652a-196">Další krok</span><span class="sxs-lookup"><span data-stu-id="0652a-196">Next step</span></span>
<span data-ttu-id="0652a-197">Teď, když jste nahráli prostředek tooMedia služby, přejděte toohello [jak tooGet procesor médií] [ How tooGet a Media Processor] tématu.</span><span class="sxs-lookup"><span data-stu-id="0652a-197">Now that you have uploaded an asset tooMedia Services, go toohello [How tooGet a Media Processor][How tooGet a Media Processor] topic.</span></span>

[How tooGet a Media Processor]: media-services-get-media-processor.md

