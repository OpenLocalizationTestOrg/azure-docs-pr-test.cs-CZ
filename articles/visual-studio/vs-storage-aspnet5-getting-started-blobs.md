---
title: "aaaGet začít s úložiště objektů blob a Visual Studio připojených služeb (ASP.NET Core) | Microsoft Docs"
description: "Po vytvoření účtu úložiště pomocí sady Visual Studio pomocí úložiště objektů Blob v Azure v projektu Visual Studio ASP.NET Core způsob spuštění tooget připojení služby"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 8eedf331896b21658c7b30a68a4391d8d60cd729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a><span data-ttu-id="53b47-103">Začínáme s Azure Blob storage a Visual Studio připojené služby (ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="53b47-103">Get started with Azure Blob storage and Visual Studio connected services (ASP.NET Core)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="53b47-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="53b47-104">Overview</span></span>
<span data-ttu-id="53b47-105">Tento článek popisuje, jak tooget spuštění pomocí úložiště objektů Blob v Azure v sadě Visual Studio, po vytvoření a odkazuje pomocí dialogu Visual Studio přidat připojení služby hello účet úložiště Azure v projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="53b47-105">This article describes how tooget started using Azure Blob storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using hello Visual Studio Add Connected Services dialog.</span></span>

<span data-ttu-id="53b47-106">Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, který lze přistupovat z libovolné místo v hello, world pomocí protokolů HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="53b47-106">Azure Blob storage is a service for storing large amounts of unstructured data that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="53b47-107">Jediného objektu blob může mít libovolnou velikost.</span><span class="sxs-lookup"><span data-stu-id="53b47-107">A single blob can be any size.</span></span> <span data-ttu-id="53b47-108">Objekty BLOB může být třeba bitové kopie, soubory audia a videa, nezpracovaná data a soubory dokumentu.</span><span class="sxs-lookup"><span data-stu-id="53b47-108">Blobs can be things like images, audio and video files, raw data, and document files.</span></span> <span data-ttu-id="53b47-109">Tento článek popisuje, jak tooget spustí pomocí úložiště objektů blob, po vytvoření účtu úložiště Azure pomocí sady Visual Studio hello **přidat připojení služby** dialogové okno v projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="53b47-109">This article describes how tooget started with blob storage after you create an Azure storage account by using hello Visual Studio **Add Connected Services** dialog in an ASP.NET Core project.</span></span>

<span data-ttu-id="53b47-110">Stejně jako soubory live ve složkách, za provozu v kontejnerech objektů BLOB storage.</span><span class="sxs-lookup"><span data-stu-id="53b47-110">Just as files live in folders, storage blobs live in containers.</span></span> <span data-ttu-id="53b47-111">Po vytvoření úložiště vytvoříte jeden nebo více kontejnerů v úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="53b47-111">After you have created a storage, you create one or more containers in hello storage.</span></span> <span data-ttu-id="53b47-112">Například v úložiště, který se nazývá "Výstřižků", vytvoříte kontejnerů v úložišti hello názvem "Image" toostore obrázky a jiné názvem "zvuk" toostore zvukových souborů.</span><span class="sxs-lookup"><span data-stu-id="53b47-112">For example, in a storage called "Scrapbook," you can create containers in hello storage called "images" toostore pictures and another called "audio" toostore audio files.</span></span> <span data-ttu-id="53b47-113">Po vytvoření kontejnerů hello můžete nahrát toothem soubory jednotlivých objektů blob.</span><span class="sxs-lookup"><span data-stu-id="53b47-113">After you create hello containers, you can upload individual blob files toothem.</span></span> <span data-ttu-id="53b47-114">V tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) Další informace o programu manipulace s objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="53b47-114">See [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) for more information on programmatically manipulating blobs.</span></span>

## <a name="access-blob-containers-in-code"></a><span data-ttu-id="53b47-115">Kontejnery objektů blob přístup v kódu</span><span class="sxs-lookup"><span data-stu-id="53b47-115">Access blob containers in code</span></span>
<span data-ttu-id="53b47-116">tooprogrammatically přístup k objektům BLOB projektů ASP.NET Core, musíte tooadd hello následující položky, pokud nejsou již existuje.</span><span class="sxs-lookup"><span data-stu-id="53b47-116">tooprogrammatically access blobs in ASP.NET Core projects, you need tooadd hello following items, if they're not already present.</span></span>

1. <span data-ttu-id="53b47-117">Přidejte následující kód obor názvů deklarace toohello horní části žádné C# soubor, ve kterém chcete tooprogrammatically přístup úložiště Azure hello.</span><span class="sxs-lookup"><span data-stu-id="53b47-117">Add hello following code namespace declarations toohello top of any C# file in which you want tooprogrammatically access Azure storage.</span></span>
   
        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;
2. <span data-ttu-id="53b47-118">Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="53b47-118">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="53b47-119">Použijte následující kód tooget hello připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure hello.</span><span class="sxs-lookup"><span data-stu-id="53b47-119">Use hello following code tooget your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
            "<storage-account-name>",
            "<access-key>"), true);
   
    <span data-ttu-id="53b47-120">**Poznámka:** používat všechny hello výše kódu před hello kódu v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="53b47-120">**NOTE:** Use all of hello above code in front of hello code in hello following sections.</span></span>
3. <span data-ttu-id="53b47-121">Použití **CloudBlobClient** objektu tooget **CloudBlobContainer** odkaz tooan existující kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="53b47-121">Use a **CloudBlobClient** object tooget a **CloudBlobContainer** reference tooan existing container in your storage account.</span></span>
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
   
        // Get a reference tooa container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

## <a name="create-a-container-in-code"></a><span data-ttu-id="53b47-122">Vytvořit kontejner v kódu</span><span class="sxs-lookup"><span data-stu-id="53b47-122">Create a container in code</span></span>
<span data-ttu-id="53b47-123">Můžete taky hello **CloudBlobClient** toocreate kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="53b47-123">You can also use hello **CloudBlobClient** toocreate a container in your storage account.</span></span> <span data-ttu-id="53b47-124">Stačí toodo tooadd volání je příliš**CreateIfNotExistsAsync** jako hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="53b47-124">All you need toodo is tooadd a call too**CreateIfNotExistsAsync** as in hello following code:</span></span>

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference tooa container named "my-new-container."
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


<span data-ttu-id="53b47-125">**Poznámka:** hello rozhraní API, která provádět volání tooAzure úložiště v ASP.NET Core jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="53b47-125">**NOTE:** hello APIs that perform calls tooAzure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="53b47-126">V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="53b47-126">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="53b47-127">Následující kód Hello předpokládá, že asynchronní programování metody jsou používány.</span><span class="sxs-lookup"><span data-stu-id="53b47-127">hello code below assumes async programming methods are being used.</span></span>

<span data-ttu-id="53b47-128">toomake hello soubory v rámci hello kontejneru dostupné tooeveryone, můžete nastavit kontejner toobe hello veřejné pomocí hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="53b47-128">toomake hello files within hello container available tooeveryone, you can set hello container toobe public by using hello following code.</span></span>

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="53b47-129">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="53b47-129">Upload a blob into a container</span></span>
<span data-ttu-id="53b47-130">tooupload souboru blob do kontejneru, získejte odkaz na kontejner a použít ho tooget odkaz na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="53b47-130">tooupload a blob file into a container, get a container reference and use it tooget a blob reference.</span></span> <span data-ttu-id="53b47-131">Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud dat tooit pomocí volání hello **UploadFromStreamAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="53b47-131">After you have a blob reference, you can upload any stream of data tooit by calling hello **UploadFromStreamAsync** method.</span></span> <span data-ttu-id="53b47-132">Tato operace vytvoří objekt blob hello, pokud ještě není, nebo ho přepíše, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="53b47-132">This operation creates hello blob if it's not already there, or overwrites it if it does exist.</span></span> <span data-ttu-id="53b47-133">Následující příklad ukazuje, jak Hello tooupload objekt blob do kontejneru a předpokládá, že hello kontejner byl již vytvořen.</span><span class="sxs-lookup"><span data-stu-id="53b47-133">hello following example shows how tooupload a blob into a container and assumes that hello container was already created.</span></span>

    // Get a reference tooa blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite hello "myblob" blob with hello contents of a local file
    // named "myfile".
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="53b47-134">Seznam hello objekty BLOB v kontejneru</span><span class="sxs-lookup"><span data-stu-id="53b47-134">List hello blobs in a container</span></span>
<span data-ttu-id="53b47-135">toolist hello objekty BLOB v kontejneru, nejdřív získejte odkaz na kontejner.</span><span class="sxs-lookup"><span data-stu-id="53b47-135">toolist hello blobs in a container, first get a container reference.</span></span> <span data-ttu-id="53b47-136">Potom můžete volat hello kontejneru **ListBlobsSegmentedAsync** metoda tooretrieve hello objekty BLOB a/nebo obsažené adresáře.</span><span class="sxs-lookup"><span data-stu-id="53b47-136">You can then call hello container's **ListBlobsSegmentedAsync** method tooretrieve hello blobs and/or directories within it.</span></span> <span data-ttu-id="53b47-137">tooaccess hello bohatou sadu vlastností a metod vrácené **IListBlobItem**, musíte vysílat tooa **CloudBlockBlob**, **CloudPageBlob**, nebo  **CloudBlobDirectory** objektu.</span><span class="sxs-lookup"><span data-stu-id="53b47-137">tooaccess hello rich set of properties and methods for a returned **IListBlobItem**, you must cast it tooa **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="53b47-138">Pokud neznáte typ blob hello, můžete použít typ kontroly toodetermine které toocast jeho.</span><span class="sxs-lookup"><span data-stu-id="53b47-138">If you don't know hello blob type, you can use a type check toodetermine which toocast it to.</span></span> <span data-ttu-id="53b47-139">Hello následující kód ukazuje, jak tooretrieve a výstup hello URI pro každou položku v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="53b47-139">hello following code demonstrates how tooretrieve and output hello URI of each item in a container.</span></span>

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
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
    } while (token != null);

<span data-ttu-id="53b47-140">Existují i další způsoby toolist hello obsah kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="53b47-140">There are others ways toolist hello contents of a blob container.</span></span> <span data-ttu-id="53b47-141">V tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) Další informace.</span><span class="sxs-lookup"><span data-stu-id="53b47-141">See [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) for more information.</span></span>

## <a name="download-a-blob"></a><span data-ttu-id="53b47-142">Stažení objektu blob</span><span class="sxs-lookup"><span data-stu-id="53b47-142">Download a blob</span></span>
<span data-ttu-id="53b47-143">toodownload objekt blob, nejdřív získejte odkaz objektu blob toohello a pak zavolají hello **DownloadToStreamAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="53b47-143">toodownload a blob, first get a reference toohello blob, and then call hello **DownloadToStreamAsync** method.</span></span> <span data-ttu-id="53b47-144">Hello následující příklad používá hello **DownloadToStreamAsync** metoda tootransfer hello blob obsah tooa datového proudu objekt, který pak můžete uložit jako místního souboru.</span><span class="sxs-lookup"><span data-stu-id="53b47-144">hello following example uses hello **DownloadToStreamAsync** method tootransfer hello blob contents tooa stream object that you can then save as a local file.</span></span>

    // Get a reference tooa blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save hello blob contents tooa file named "myfile".
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

<span data-ttu-id="53b47-145">Existují jiné způsoby objekty BLOB toosave jako soubory.</span><span class="sxs-lookup"><span data-stu-id="53b47-145">There are other ways toosave blobs as files.</span></span> <span data-ttu-id="53b47-146">V tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs) Další informace.</span><span class="sxs-lookup"><span data-stu-id="53b47-146">See [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs) for more information.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="53b47-147">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="53b47-147">Delete a blob</span></span>
<span data-ttu-id="53b47-148">toodelete objekt blob, nejdřív získejte odkaz objektu blob toohello a pak zavolají hello **DeleteAsync** metoda na něm.</span><span class="sxs-lookup"><span data-stu-id="53b47-148">toodelete a blob, first get a reference toohello blob, and then call hello **DeleteAsync** method on it.</span></span>

    // Get a reference tooa blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete hello blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a><span data-ttu-id="53b47-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="53b47-149">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

