---
title: "Začínáme s objektem blob storage a Visual Studio připojené služby (ASP.NET Core) | Microsoft Docs"
description: "Jak začít používat úložiště objektů Blob v Azure v projektu Visual Studio ASP.NET Core po vytvoření účtu úložiště pomocí sady Visual Studio připojené služby"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: e725015c8be7ecfa908f0ae75986b73f218fa3ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a><span data-ttu-id="19132-103">Začínáme s Azure Blob storage a Visual Studio připojené služby (ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="19132-103">Get started with Azure Blob storage and Visual Studio connected services (ASP.NET Core)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="19132-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="19132-104">Overview</span></span>
<span data-ttu-id="19132-105">Tento článek popisuje, jak začít používat úložiště objektů Blob v Azure v sadě Visual Studio, po vytvoření a účet úložiště Azure v projektu ASP.NET Core odkazuje v dialogovém okně Visual Studio přidat připojení služby.</span><span class="sxs-lookup"><span data-stu-id="19132-105">This article describes how to get started using Azure Blob storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using the Visual Studio Add Connected Services dialog.</span></span>

<span data-ttu-id="19132-106">Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, která je přístupná z kdekoliv na světě prostřednictvím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="19132-106">Azure Blob storage is a service for storing large amounts of unstructured data that can be accessed from anywhere in the world via HTTP or HTTPS.</span></span> <span data-ttu-id="19132-107">Jediného objektu blob může mít libovolnou velikost.</span><span class="sxs-lookup"><span data-stu-id="19132-107">A single blob can be any size.</span></span> <span data-ttu-id="19132-108">Objekty BLOB může být třeba bitové kopie, soubory audia a videa, nezpracovaná data a soubory dokumentu.</span><span class="sxs-lookup"><span data-stu-id="19132-108">Blobs can be things like images, audio and video files, raw data, and document files.</span></span> <span data-ttu-id="19132-109">Tento článek popisuje, jak začít pracovat s úložištěm blob po vytvoření účtu úložiště Azure pomocí sady Visual Studio **přidat připojení služby** dialogové okno v projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="19132-109">This article describes how to get started with blob storage after you create an Azure storage account by using the Visual Studio **Add Connected Services** dialog in an ASP.NET Core project.</span></span>

<span data-ttu-id="19132-110">Stejně jako soubory live ve složkách, za provozu v kontejnerech objektů BLOB storage.</span><span class="sxs-lookup"><span data-stu-id="19132-110">Just as files live in folders, storage blobs live in containers.</span></span> <span data-ttu-id="19132-111">Po vytvoření úložiště můžete vytvořit jeden nebo více kontejnerů v úložišti.</span><span class="sxs-lookup"><span data-stu-id="19132-111">After you have created a storage, you create one or more containers in the storage.</span></span> <span data-ttu-id="19132-112">Například v úložiště, který se nazývá "Výstřižků", vytvoříte kontejnerů v úložišti názvem "Image" k ukládání obrázků a jiné názvem "zvuk" k uložení zvukových souborů.</span><span class="sxs-lookup"><span data-stu-id="19132-112">For example, in a storage called "Scrapbook," you can create containers in the storage called "images" to store pictures and another called "audio" to store audio files.</span></span> <span data-ttu-id="19132-113">Po vytvoření kontejnerů, můžete odeslat soubory jednotlivých objektů blob k nim.</span><span class="sxs-lookup"><span data-stu-id="19132-113">After you create the containers, you can upload individual blob files to them.</span></span> <span data-ttu-id="19132-114">V tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md) Další informace o programu manipulace s objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="19132-114">See [Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) for more information on programmatically manipulating blobs.</span></span>

## <a name="access-blob-containers-in-code"></a><span data-ttu-id="19132-115">Kontejnery objektů blob přístup v kódu</span><span class="sxs-lookup"><span data-stu-id="19132-115">Access blob containers in code</span></span>
<span data-ttu-id="19132-116">K programovému přístupu ke objektů BLOB v projektů ASP.NET Core, musíte přidat následující položky, pokud nejsou již existuje.</span><span class="sxs-lookup"><span data-stu-id="19132-116">To programmatically access blobs in ASP.NET Core projects, you need to add the following items, if they're not already present.</span></span>

1. <span data-ttu-id="19132-117">Přidejte následující deklarace oborů názvů kódu do horní části souboru žádné C# ve kterém chcete získat přístup prostřednictvím kódu programu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="19132-117">Add the following code namespace declarations to the top of any C# file in which you want to programmatically access Azure storage.</span></span>
   
        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;
2. <span data-ttu-id="19132-118">Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="19132-118">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="19132-119">Použijte následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure.</span><span class="sxs-lookup"><span data-stu-id="19132-119">Use the following code to get your storage connection string and storage account information from the Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
            "<storage-account-name>",
            "<access-key>"), true);
   
    <span data-ttu-id="19132-120">**Poznámka:** používat všechny výše uvedené kódu před kód v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="19132-120">**NOTE:** Use all of the above code in front of the code in the following sections.</span></span>
3. <span data-ttu-id="19132-121">Použití **CloudBlobClient** objekt, který chcete získat **CloudBlobContainer** odkaz na existující kontejner ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="19132-121">Use a **CloudBlobClient** object to get a **CloudBlobContainer** reference to an existing container in your storage account.</span></span>
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
   
        // Get a reference to a container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

## <a name="create-a-container-in-code"></a><span data-ttu-id="19132-122">Vytvořit kontejner v kódu</span><span class="sxs-lookup"><span data-stu-id="19132-122">Create a container in code</span></span>
<span data-ttu-id="19132-123">Můžete také **CloudBlobClient** vytvořit kontejner ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="19132-123">You can also use the **CloudBlobClient** to create a container in your storage account.</span></span> <span data-ttu-id="19132-124">Vše je potřeba udělat, je přidáte volání **CreateIfNotExistsAsync** jako v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="19132-124">All you need to do is to add a call to **CreateIfNotExistsAsync** as in the following code:</span></span>

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named "my-new-container."
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


<span data-ttu-id="19132-125">**Poznámka:** rozhraní API, které provádět volání do úložiště Azure v ASP.NET Core jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="19132-125">**NOTE:** The APIs that perform calls to Azure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="19132-126">V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="19132-126">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="19132-127">Následující kód předpokládá, že asynchronní programování metody jsou používány.</span><span class="sxs-lookup"><span data-stu-id="19132-127">The code below assumes async programming methods are being used.</span></span>

<span data-ttu-id="19132-128">Soubory v kontejneru zpřístupnit všem uživatelům, můžete nastavit kontejner, aby se veřejné pomocí následující kód.</span><span class="sxs-lookup"><span data-stu-id="19132-128">To make the files within the container available to everyone, you can set the container to be public by using the following code.</span></span>

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="19132-129">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="19132-129">Upload a blob into a container</span></span>
<span data-ttu-id="19132-130">Nahrát soubor objektu blob do kontejneru, získejte odkaz na kontejner a umožňuje vám získat odkaz na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="19132-130">To upload a blob file into a container, get a container reference and use it to get a blob reference.</span></span> <span data-ttu-id="19132-131">Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud dat k němu voláním **UploadFromStreamAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="19132-131">After you have a blob reference, you can upload any stream of data to it by calling the **UploadFromStreamAsync** method.</span></span> <span data-ttu-id="19132-132">Tato operace vytvoří objekt blob, pokud ještě není, nebo ho přepíše, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="19132-132">This operation creates the blob if it's not already there, or overwrites it if it does exist.</span></span> <span data-ttu-id="19132-133">Následující příklad ukazuje, jak nahrát objekt blob do kontejneru, zároveň předpokládá, že kontejner byl již vytvořen.</span><span class="sxs-lookup"><span data-stu-id="19132-133">The following example shows how to upload a blob into a container and assumes that the container was already created.</span></span>

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named "myfile".
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="19132-134">Zobrazí seznam objektů blob v kontejneru</span><span class="sxs-lookup"><span data-stu-id="19132-134">List the blobs in a container</span></span>
<span data-ttu-id="19132-135">Pokud chcete mít seznam objektů blob v kontejneru, nejdřív získejte odkaz na kontejner.</span><span class="sxs-lookup"><span data-stu-id="19132-135">To list the blobs in a container, first get a container reference.</span></span> <span data-ttu-id="19132-136">Potom můžete volat kontejneru **ListBlobsSegmentedAsync** metoda pro načtení objektů BLOB a/nebo obsažené adresáře.</span><span class="sxs-lookup"><span data-stu-id="19132-136">You can then call the container's **ListBlobsSegmentedAsync** method to retrieve the blobs and/or directories within it.</span></span> <span data-ttu-id="19132-137">Pro přístup k bohaté sadě vlastností a metod vrácené položky **IListBlobItem** musíte vysílat na objekt **CloudBlockBlob**, **CloudPageBlob** nebo **CloudBlobDirectory**.</span><span class="sxs-lookup"><span data-stu-id="19132-137">To access the rich set of properties and methods for a returned **IListBlobItem**, you must cast it to a **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="19132-138">Pokud neznáte typ objektu blob, můžete použít kontrolu typu zjistíte, které vysílat.</span><span class="sxs-lookup"><span data-stu-id="19132-138">If you don't know the blob type, you can use a type check to determine which to cast it to.</span></span> <span data-ttu-id="19132-139">Následující kód ukazuje, jak načíst a získat výstup URI pro každou položku v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="19132-139">The following code demonstrates how to retrieve and output the URI of each item in a container.</span></span>

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

<span data-ttu-id="19132-140">Existují jiné způsoby vypíše obsah kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="19132-140">There are others ways to list the contents of a blob container.</span></span> <span data-ttu-id="19132-141">V tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) Další informace.</span><span class="sxs-lookup"><span data-stu-id="19132-141">See [Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) for more information.</span></span>

## <a name="download-a-blob"></a><span data-ttu-id="19132-142">Stažení objektu blob</span><span class="sxs-lookup"><span data-stu-id="19132-142">Download a blob</span></span>
<span data-ttu-id="19132-143">Stažení objekt blob, nejdřív získejte odkaz na objekt blob a potom zavolejte **DownloadToStreamAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="19132-143">To download a blob, first get a reference to the blob, and then call the **DownloadToStreamAsync** method.</span></span> <span data-ttu-id="19132-144">Následující příklad používá **DownloadToStreamAsync** způsob přenosu obsahu objektu blob na objekt proudu, který pak můžete uložit jako místního souboru.</span><span class="sxs-lookup"><span data-stu-id="19132-144">The following example uses the **DownloadToStreamAsync** method to transfer the blob contents to a stream object that you can then save as a local file.</span></span>

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named "myfile".
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

<span data-ttu-id="19132-145">Pro uložení objektů BLOB jako soubory i jinými způsoby.</span><span class="sxs-lookup"><span data-stu-id="19132-145">There are other ways to save blobs as files.</span></span> <span data-ttu-id="19132-146">V tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md#download-blobs) Další informace.</span><span class="sxs-lookup"><span data-stu-id="19132-146">See [Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md#download-blobs) for more information.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="19132-147">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="19132-147">Delete a blob</span></span>
<span data-ttu-id="19132-148">Pokud chcete odstranit objekt blob, nejdřív získejte odkaz na objekt blob a potom zavolejte **DeleteAsync** metoda na něm.</span><span class="sxs-lookup"><span data-stu-id="19132-148">To delete a blob, first get a reference to the blob, and then call the **DeleteAsync** method on it.</span></span>

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a><span data-ttu-id="19132-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="19132-149">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

