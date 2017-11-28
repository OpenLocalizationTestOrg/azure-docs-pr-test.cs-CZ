---
title: "aaaGet začít s úložiště objektů blob a Visual Studio připojených služeb (cloudové služby) | Microsoft Docs"
description: "Jak tooget spustit po připojení tooa účet úložiště pomocí sady Visual Studio připojené služby používat úložiště objektů Blob v Azure v projektu cloudové služby v sadě Visual Studio"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1144a958-f75a-4466-bb21-320b7ae8f304
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 158197a9d49bc4f26841d689405529192c52f529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a><span data-ttu-id="44d8e-103">Začínáme s Azure Blob Storage a Visual Studio připojené služby (projekty cloudových služeb)</span><span class="sxs-lookup"><span data-stu-id="44d8e-103">Get started with Azure Blob Storage and Visual Studio connected services (cloud services projects)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="44d8e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="44d8e-104">Overview</span></span>
<span data-ttu-id="44d8e-105">Tento článek popisuje, jak tooget pracovat s Azure Blob Storage po vytvoření nebo odkazovaný účet úložiště Azure pomocí sady Visual Studio hello **přidat připojení služby** dialogové okno v sadě Visual Studio cloudové služby projektu.</span><span class="sxs-lookup"><span data-stu-id="44d8e-105">This article describes how tooget started with Azure Blob Storage after you created or referenced an Azure Storage account by using hello Visual Studio **Add Connected Services** dialog in a Visual Studio cloud services project.</span></span> <span data-ttu-id="44d8e-106">Ukážeme vám jak tooaccess a vytvoření kontejnerů objektů blob a jak tooperform běžné úkoly jako odesílání, výpis a stahování objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="44d8e-106">We'll show you how tooaccess and create blob containers, and how tooperform common tasks like uploading, listing, and downloading blobs.</span></span> <span data-ttu-id="44d8e-107">Hello ukázky jsou napsané v jazyce C\# a používat hello [Microsoft Azure Storage Client Library pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="44d8e-107">hello samples are written in C\# and use hello [Microsoft Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="44d8e-108">Azure Blob Storage je služba pro ukládání velkého objemu nestrukturovaných dat, který lze přistupovat z libovolné místo v hello, world pomocí protokolů HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="44d8e-108">Azure Blob Storage is a service for storing large amounts of unstructured data that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="44d8e-109">Jediného objektu blob může mít libovolnou velikost.</span><span class="sxs-lookup"><span data-stu-id="44d8e-109">A single blob can be any size.</span></span> <span data-ttu-id="44d8e-110">Objekty BLOB může být třeba bitové kopie, soubory audia a videa, nezpracovaná data a soubory dokumentu.</span><span class="sxs-lookup"><span data-stu-id="44d8e-110">Blobs can be things like images, audio and video files, raw data, and document files.</span></span>

<span data-ttu-id="44d8e-111">Stejně jako soubory live ve složkách, za provozu v kontejnerech objektů BLOB storage.</span><span class="sxs-lookup"><span data-stu-id="44d8e-111">Just as files live in folders, storage blobs live in containers.</span></span> <span data-ttu-id="44d8e-112">Po vytvoření úložiště vytvoříte jeden nebo více kontejnerů v úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="44d8e-112">After you have created a storage, you create one or more containers in hello storage.</span></span> <span data-ttu-id="44d8e-113">Například v úložiště, který se nazývá "Výstřižků", vytvoříte kontejnerů v úložišti hello názvem "Image" toostore obrázky a jiné názvem "zvuk" toostore zvukových souborů.</span><span class="sxs-lookup"><span data-stu-id="44d8e-113">For example, in a storage called "Scrapbook," you can create containers in hello storage called "images" toostore pictures and another called "audio" toostore audio files.</span></span> <span data-ttu-id="44d8e-114">Po vytvoření kontejnerů hello můžete nahrát toothem soubory jednotlivých objektů blob.</span><span class="sxs-lookup"><span data-stu-id="44d8e-114">After you create hello containers, you can upload individual blob files toothem.</span></span>

* <span data-ttu-id="44d8e-115">Další informace o programu manipulace s objekty BLOB najdete v tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="44d8e-115">For more information on programmatically manipulating blobs, see [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span>
* <span data-ttu-id="44d8e-116">Obecné informace o službě Azure Storage najdete v tématu [úložiště dokumentace](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="44d8e-116">For general information about Azure Storage, see [Storage documentation](https://azure.microsoft.com/documentation/services/storage/).</span></span>
* <span data-ttu-id="44d8e-117">Obecné informace o Azure Cloud Services najdete v tématu [cloudové služby dokumentaci](https://azure.microsoft.com/documentation/services/cloud-services/).</span><span class="sxs-lookup"><span data-stu-id="44d8e-117">For general information about Azure Cloud Services, see [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/).</span></span>
* <span data-ttu-id="44d8e-118">Další informace o programování aplikace ASP.NET najdete v tématu [ASP.NET](http://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="44d8e-118">For more information about programming ASP.NET applications, see [ASP.NET](http://www.asp.net).</span></span>

## <a name="access-blob-containers-in-code"></a><span data-ttu-id="44d8e-119">Kontejnery objektů blob přístup v kódu</span><span class="sxs-lookup"><span data-stu-id="44d8e-119">Access blob containers in code</span></span>
<span data-ttu-id="44d8e-120">tooprogrammatically přístup k objektům BLOB v projekty cloudových služeb, je nutné tooadd hello následující položky, pokud nejsou již existuje.</span><span class="sxs-lookup"><span data-stu-id="44d8e-120">tooprogrammatically access blobs in cloud service projects, you need tooadd hello following items, if they're not already present.</span></span>

1. <span data-ttu-id="44d8e-121">Přidejte následující kód obor názvů deklarace toohello horní části souboru žádné C# ve kterém chcete tooprogrammatically přístupu Azure Storage hello.</span><span class="sxs-lookup"><span data-stu-id="44d8e-121">Add hello following code namespace declarations toohello top of any C# file in which you wish tooprogrammatically access Azure Storage.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="44d8e-122">Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="44d8e-122">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="44d8e-123">Následující kód tooget hello použití hello připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure hello.</span><span class="sxs-lookup"><span data-stu-id="44d8e-123">Use hello following code tooget hello your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));
3. <span data-ttu-id="44d8e-124">Získání **CloudBlobClient** objektu tooreference existující kontejner ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="44d8e-124">Get a **CloudBlobClient** object tooreference an existing container in your storage account.</span></span>
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
4. <span data-ttu-id="44d8e-125">Získání **CloudBlobContainer** tooreference kontejner objektů blob konkrétní objekt.</span><span class="sxs-lookup"><span data-stu-id="44d8e-125">Get a **CloudBlobContainer** object tooreference a specific blob container.</span></span>
   
        // Get a reference tooa container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [!NOTE]
> <span data-ttu-id="44d8e-126">Použijte všechny hello kódu uvedené v předchozím postupu hello před uvedeném v následující části hello hello kódu.</span><span class="sxs-lookup"><span data-stu-id="44d8e-126">Use all of hello code shown in hello previous procedure in front of hello code shown in hello following sections.</span></span>
> 
> 

## <a name="create-a-container-in-code"></a><span data-ttu-id="44d8e-127">Vytvořit kontejner v kódu</span><span class="sxs-lookup"><span data-stu-id="44d8e-127">Create a container in code</span></span>
> [!NOTE]
> <span data-ttu-id="44d8e-128">Některé rozhraní API, které provádět volání out tooAzure úložiště v ASP.NET jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="44d8e-128">Some APIs that perform calls out tooAzure Storage in ASP.NET are asynchronous.</span></span> <span data-ttu-id="44d8e-129">V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="44d8e-129">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="44d8e-130">Hello kód v hello následující příklad předpokládá, že používáte asynchronní programování metody.</span><span class="sxs-lookup"><span data-stu-id="44d8e-130">hello code in hello following example assumes that you are using async programming methods.</span></span>
> 
> 

<span data-ttu-id="44d8e-131">toocreate kontejneru v účtu úložiště, stačí toodo je přidejte volání příliš**CreateIfNotExistsAsync** jako hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="44d8e-131">toocreate a container in your storage account, all you need toodo is add a call too**CreateIfNotExistsAsync** as in hello following code:</span></span>

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


<span data-ttu-id="44d8e-132">toomake hello soubory v rámci hello kontejneru dostupné tooeveryone, můžete nastavit kontejner toobe hello veřejné pomocí hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="44d8e-132">toomake hello files within hello container available tooeveryone, you can set hello container toobe public by using hello following code.</span></span>

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


<span data-ttu-id="44d8e-133">Kdokoli na hello Internetu může vidět objekty BLOB ve veřejném kontejneru, ale můžete upravit nebo odstranit pouze v případě, že máte příslušný přístupový klíč hello.</span><span class="sxs-lookup"><span data-stu-id="44d8e-133">Anyone on hello Internet can see blobs in a public container, but you can modify or delete them only if you have hello appropriate access key.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="44d8e-134">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="44d8e-134">Upload a blob into a container</span></span>
<span data-ttu-id="44d8e-135">Úložiště Azure podporuje objekty BLOB bloku a objekty BLOB stránky.</span><span class="sxs-lookup"><span data-stu-id="44d8e-135">Azure Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="44d8e-136">Ve většině případů hello je objekt blob bloku hello doporučená toouse typu.</span><span class="sxs-lookup"><span data-stu-id="44d8e-136">In hello majority of cases, block blob is hello recommended type toouse.</span></span>

<span data-ttu-id="44d8e-137">tooupload objekt blob bloku souboru tooa získejte odkaz na kontejner a použít ho tooget odkaz na objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="44d8e-137">tooupload a file tooa block blob, get a container reference and use it tooget a block blob reference.</span></span> <span data-ttu-id="44d8e-138">Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud dat tooit pomocí volání hello **UploadFromStream** metoda.</span><span class="sxs-lookup"><span data-stu-id="44d8e-138">Once you have a blob reference, you can upload any stream of data tooit by calling hello **UploadFromStream** method.</span></span> <span data-ttu-id="44d8e-139">Tato operace vytvoří hello blob, pokud nebyla dříve neexistuje, nebo ho přepíše, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="44d8e-139">This operation creates hello blob if it didn't previously exist, or overwrites it if it does exist.</span></span> <span data-ttu-id="44d8e-140">Následující příklad ukazuje, jak Hello tooupload objekt blob do kontejneru a předpokládá, že hello kontejner byl již vytvořen.</span><span class="sxs-lookup"><span data-stu-id="44d8e-140">hello following example shows how tooupload a blob into a container and assumes that hello container was already created.</span></span>

    // Retrieve a reference tooa blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite hello "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="44d8e-141">Seznam hello objekty BLOB v kontejneru</span><span class="sxs-lookup"><span data-stu-id="44d8e-141">List hello blobs in a container</span></span>
<span data-ttu-id="44d8e-142">toolist hello objekty BLOB v kontejneru, nejdřív získejte odkaz na kontejner.</span><span class="sxs-lookup"><span data-stu-id="44d8e-142">toolist hello blobs in a container, first get a container reference.</span></span> <span data-ttu-id="44d8e-143">Pak můžete použít hello kontejneru **ListBlobs** metoda tooretrieve hello objekty BLOB a/nebo obsažené adresáře.</span><span class="sxs-lookup"><span data-stu-id="44d8e-143">You can then use hello container's **ListBlobs** method tooretrieve hello blobs and/or directories within it.</span></span> <span data-ttu-id="44d8e-144">tooaccess hello bohatou sadu vlastností a metod vrácené **IListBlobItem**, musíte vysílat tooa **CloudBlockBlob**, **CloudPageBlob**, nebo  **CloudBlobDirectory** objektu.</span><span class="sxs-lookup"><span data-stu-id="44d8e-144">tooaccess hello rich set of properties and methods for a  returned **IListBlobItem**, you must cast it tooa **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="44d8e-145">Pokud typ hello neznámý, můžete použít typ kontroly toodetermine které toocast jeho.</span><span class="sxs-lookup"><span data-stu-id="44d8e-145">If hello type is unknown, you can use a type check toodetermine which toocast it to.</span></span> <span data-ttu-id="44d8e-146">Hello následující kód ukazuje, jak tooretrieve a výstup hello URI pro každou položku v hello **fotografie** kontejneru:</span><span class="sxs-lookup"><span data-stu-id="44d8e-146">hello following code demonstrates how tooretrieve and output hello URI of each item in hello **photos** container:</span></span>

    // Loop over items within hello container and output hello length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

<span data-ttu-id="44d8e-147">Jak ukazuje předchozí příklad hello, má služby objektů blob hello hello konceptu adresáře v rámci kontejnery také.</span><span class="sxs-lookup"><span data-stu-id="44d8e-147">As shown in hello previous code sample, hello blob service has hello concept of directories within containers, as well.</span></span> <span data-ttu-id="44d8e-148">Toto je tak, aby můžete uspořádat, objektů BLOB do více stromové struktury.</span><span class="sxs-lookup"><span data-stu-id="44d8e-148">This is so that you can organize your blobs in a more folder-like structure.</span></span> <span data-ttu-id="44d8e-149">Zvažte například následující sadu objektů BLOB bloku v kontejneru nazvaném hello **fotografie**:</span><span class="sxs-lookup"><span data-stu-id="44d8e-149">For example, consider hello following set of block blobs in a container named **photos**:</span></span>

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

<span data-ttu-id="44d8e-150">Při volání **ListBlobs** hello kontejneru (jako v předchozím příkladu hello), obsahuje kolekci hello vrátil **CloudBlobDirectory** a **CloudBlockBlob** objekty představující hello adresáře a objekty BLOB obsažené na nejvyšší úrovni hello.</span><span class="sxs-lookup"><span data-stu-id="44d8e-150">When you call **ListBlobs** on hello container (as in hello previous sample), hello collection returned contains **CloudBlobDirectory** and **CloudBlockBlob** objects representing hello directories and blobs contained at hello top level.</span></span> <span data-ttu-id="44d8e-151">Zde je výsledný výstup hello:</span><span class="sxs-lookup"><span data-stu-id="44d8e-151">Here is hello resulting output:</span></span>

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


<span data-ttu-id="44d8e-152">Volitelně můžete nastavit hello **UseFlatBlobListing** parametr z hello **ListBlobs** metodu **true**.</span><span class="sxs-lookup"><span data-stu-id="44d8e-152">Optionally, you can set hello **UseFlatBlobListing** parameter of of hello **ListBlobs** method to **true**.</span></span> <span data-ttu-id="44d8e-153">Výsledkem je každý objekt blob se vrátí jako **CloudBlockBlob**, bez ohledu na to adresáře.</span><span class="sxs-lookup"><span data-stu-id="44d8e-153">This results in every blob being returned as a **CloudBlockBlob**, regardless of directory.</span></span> <span data-ttu-id="44d8e-154">Zde je hello volání příliš**ListBlobs**:</span><span class="sxs-lookup"><span data-stu-id="44d8e-154">Here is hello call too**ListBlobs**:</span></span>

    // Loop over items within hello container and output hello length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

<span data-ttu-id="44d8e-155">a tady jsou výsledky hello:</span><span class="sxs-lookup"><span data-stu-id="44d8e-155">and here are hello results:</span></span>

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

<span data-ttu-id="44d8e-156">Další informace najdete v tématu [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).</span><span class="sxs-lookup"><span data-stu-id="44d8e-156">For more information, see [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).</span></span>

## <a name="download-blobs"></a><span data-ttu-id="44d8e-157">Stáhnout objekty blob</span><span class="sxs-lookup"><span data-stu-id="44d8e-157">Download blobs</span></span>
<span data-ttu-id="44d8e-158">toodownload objekty BLOB, nejdřív načtěte odkaz na objekt blob a pak zavolají hello **DownloadToStream** metoda.</span><span class="sxs-lookup"><span data-stu-id="44d8e-158">toodownload blobs, first retrieve a blob reference and then call hello **DownloadToStream** method.</span></span> <span data-ttu-id="44d8e-159">Hello následující příklad používá hello **DownloadToStream** metoda tootransfer hello blob obsah tooa objektu stream, potom můžete zachovat tooa místního souboru.</span><span class="sxs-lookup"><span data-stu-id="44d8e-159">hello following example uses hello **DownloadToStream** method tootransfer hello blob contents tooa stream object that you can then persist tooa local file.</span></span>

    // Get a reference tooa blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents tooa file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

<span data-ttu-id="44d8e-160">Můžete taky hello **DownloadToStream** metoda toodownload hello obsah objektu blob jako textový řetězec.</span><span class="sxs-lookup"><span data-stu-id="44d8e-160">You can also use hello **DownloadToStream** method toodownload hello contents of a blob as a text string.</span></span>

    // Get a reference tooa blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a><span data-ttu-id="44d8e-161">Odstranění objektů blob</span><span class="sxs-lookup"><span data-stu-id="44d8e-161">Delete blobs</span></span>
<span data-ttu-id="44d8e-162">toodelete objekt blob, nejdřív získejte odkaz na objekt blob a potom zavolejte **odstranit** metoda.</span><span class="sxs-lookup"><span data-stu-id="44d8e-162">toodelete a blob, first get a blob reference and then call the **Delete** method.</span></span>

    // Get a reference tooa blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete hello blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a><span data-ttu-id="44d8e-163">Asynchronní zobrazení seznamu objektů blob na stránkách</span><span class="sxs-lookup"><span data-stu-id="44d8e-163">List blobs in pages asynchronously</span></span>
<span data-ttu-id="44d8e-164">Pokud provádíte výpis velkého počtu objektů BLOB nebo chcete toocontrol hello počet výsledků, které vrátíte v rámci jedné operace výpisu, můžete vytvořit seznam objektů blob na stránkách s výsledky.</span><span class="sxs-lookup"><span data-stu-id="44d8e-164">If you are listing a large number of blobs, or you want toocontrol hello number of results you return in one listing operation, you can list blobs in pages of results.</span></span> <span data-ttu-id="44d8e-165">Tento příklad ukazuje, jak tooreturn výsledky na stránkách asynchronně, takže čekání tooreturn velké sady výsledků neblokovalo provádění.</span><span class="sxs-lookup"><span data-stu-id="44d8e-165">This example shows how tooreturn results in pages asynchronously, so that execution is not blocked while waiting tooreturn a large set of results.</span></span>

<span data-ttu-id="44d8e-166">Tento příklad ukazuje výpis plochého objektu blob ale můžete také provést hierarchický výpis podle nastavení hello **useFlatBlobListing** parametr hello **ListBlobsSegmentedAsync** metoda příliš **false**.</span><span class="sxs-lookup"><span data-stu-id="44d8e-166">This example shows a flat blob listing, but you can also perform a hierarchical listing, by setting hello **useFlatBlobListing** parameter of hello **ListBlobsSegmentedAsync** method too**false**.</span></span>

<span data-ttu-id="44d8e-167">Protože hello metoda ukázky volá asynchronní metodu, musí být uvedena s hello **asynchronní** – klíčové slovo a musí vrátit **úloh** objektu.</span><span class="sxs-lookup"><span data-stu-id="44d8e-167">Because hello sample method calls an asynchronous method, it must be prefaced with hello **async** keyword, and it must return a **Task** object.</span></span> <span data-ttu-id="44d8e-168">await – klíčové slovo zadané pro hello Hello **ListBlobsSegmentedAsync** pozastaví spuštění metody ukázky hello až do dokončení úlohy vytváření seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="44d8e-168">hello await keyword specified for hello **ListBlobsSegmentedAsync** method suspends execution of hello sample method until hello listing task completes.</span></span>

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs toohello console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate hello result segment returned, while hello continuation token is non-null.
        // When hello continuation token is null, hello last page has been returned and execution can exit hello loop.
        do
        {
            // This overload allows control of hello page size. You can return all remaining results by passing null for hello maxResults parameter,
            // or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get hello continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="next-steps"></a><span data-ttu-id="44d8e-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="44d8e-169">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

