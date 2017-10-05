---
title: "Začínáme s objektem blob storage a Visual Studio připojené služby (cloudové služby) | Microsoft Docs"
description: "Jak začít používat úložiště objektů Blob v Azure v projektu cloudové služby v sadě Visual Studio po připojení k účtu úložiště pomocí sady Visual Studio připojené služby"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 1144a958-f75a-4466-bb21-320b7ae8f304
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: e154c81ef3765a3c006b3c27a979be881f14d0ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a><span data-ttu-id="8d517-103">Začínáme s Azure Blob Storage a Visual Studio připojené služby (projekty cloudových služeb)</span><span class="sxs-lookup"><span data-stu-id="8d517-103">Get started with Azure Blob Storage and Visual Studio connected services (cloud services projects)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="8d517-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="8d517-104">Overview</span></span>
<span data-ttu-id="8d517-105">Tento článek popisuje, jak začít pracovat s Azure Blob Storage po vytvoření nebo odkazuje účet úložiště Azure pomocí sady Visual Studio **přidat připojení služby** dialogové okno v sadě Visual Studio cloudové služby projektu.</span><span class="sxs-lookup"><span data-stu-id="8d517-105">This article describes how to get started with Azure Blob Storage after you created or referenced an Azure Storage account by using the Visual Studio **Add Connected Services** dialog in a Visual Studio cloud services project.</span></span> <span data-ttu-id="8d517-106">Ukážeme vám přístupu a vytvoření kontejnerů objektů blob a jak provádět běžné úkoly, jako odesílání, výpis a stahování objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="8d517-106">We'll show you how to access and create blob containers, and how to perform common tasks like uploading, listing, and downloading blobs.</span></span> <span data-ttu-id="8d517-107">Ukázky jsou napsané v jazyce C\# a použít [Microsoft Azure Storage Client Library pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d517-107">The samples are written in C\# and use the [Microsoft Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="8d517-108">Azure Blob Storage je služba pro ukládání velkého objemu nestrukturovaných dat, která je přístupná z kdekoliv na světě prostřednictvím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8d517-108">Azure Blob Storage is a service for storing large amounts of unstructured data that can be accessed from anywhere in the world via HTTP or HTTPS.</span></span> <span data-ttu-id="8d517-109">Jediného objektu blob může mít libovolnou velikost.</span><span class="sxs-lookup"><span data-stu-id="8d517-109">A single blob can be any size.</span></span> <span data-ttu-id="8d517-110">Objekty BLOB může být třeba bitové kopie, soubory audia a videa, nezpracovaná data a soubory dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8d517-110">Blobs can be things like images, audio and video files, raw data, and document files.</span></span>

<span data-ttu-id="8d517-111">Stejně jako soubory live ve složkách, za provozu v kontejnerech objektů BLOB storage.</span><span class="sxs-lookup"><span data-stu-id="8d517-111">Just as files live in folders, storage blobs live in containers.</span></span> <span data-ttu-id="8d517-112">Po vytvoření úložiště můžete vytvořit jeden nebo více kontejnerů v úložišti.</span><span class="sxs-lookup"><span data-stu-id="8d517-112">After you have created a storage, you create one or more containers in the storage.</span></span> <span data-ttu-id="8d517-113">Například v úložiště, který se nazývá "Výstřižků", vytvoříte kontejnerů v úložišti názvem "Image" k ukládání obrázků a jiné názvem "zvuk" k uložení zvukových souborů.</span><span class="sxs-lookup"><span data-stu-id="8d517-113">For example, in a storage called "Scrapbook," you can create containers in the storage called "images" to store pictures and another called "audio" to store audio files.</span></span> <span data-ttu-id="8d517-114">Po vytvoření kontejnerů, můžete odeslat soubory jednotlivých objektů blob k nim.</span><span class="sxs-lookup"><span data-stu-id="8d517-114">After you create the containers, you can upload individual blob files to them.</span></span>

* <span data-ttu-id="8d517-115">Další informace o programu manipulace s objekty BLOB najdete v tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="8d517-115">For more information on programmatically manipulating blobs, see [Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md).</span></span>
* <span data-ttu-id="8d517-116">Obecné informace o službě Azure Storage najdete v tématu [úložiště dokumentace](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="8d517-116">For general information about Azure Storage, see [Storage documentation](https://azure.microsoft.com/documentation/services/storage/).</span></span>
* <span data-ttu-id="8d517-117">Obecné informace o Azure Cloud Services najdete v tématu [cloudové služby dokumentaci](https://azure.microsoft.com/documentation/services/cloud-services/).</span><span class="sxs-lookup"><span data-stu-id="8d517-117">For general information about Azure Cloud Services, see [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/).</span></span>
* <span data-ttu-id="8d517-118">Další informace o programování aplikace ASP.NET najdete v tématu [ASP.NET](http://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="8d517-118">For more information about programming ASP.NET applications, see [ASP.NET](http://www.asp.net).</span></span>

## <a name="access-blob-containers-in-code"></a><span data-ttu-id="8d517-119">Kontejnery objektů blob přístup v kódu</span><span class="sxs-lookup"><span data-stu-id="8d517-119">Access blob containers in code</span></span>
<span data-ttu-id="8d517-120">K programovému přístupu ke objektů BLOB v projekty cloudových služeb, musíte přidat následující položky, pokud nejsou již existuje.</span><span class="sxs-lookup"><span data-stu-id="8d517-120">To programmatically access blobs in cloud service projects, you need to add the following items, if they're not already present.</span></span>

1. <span data-ttu-id="8d517-121">Přidejte následující deklarace oborů názvů kódu do horní části souboru žádné C# ve které chcete k programovému přístupu ke službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="8d517-121">Add the following code namespace declarations to the top of any C# file in which you wish to programmatically access Azure Storage.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="8d517-122">Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="8d517-122">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="8d517-123">Použít následující kód k získání připojovací řetězec úložiště a informace o účtu úložiště z konfigurace služby Azure.</span><span class="sxs-lookup"><span data-stu-id="8d517-123">Use the following code to get the your storage connection string and storage account information from the Azure service configuration.</span></span>
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));
3. <span data-ttu-id="8d517-124">Získání **CloudBlobClient** objekt, který má odkazovat na existující kontejner ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="8d517-124">Get a **CloudBlobClient** object to reference an existing container in your storage account.</span></span>
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
4. <span data-ttu-id="8d517-125">Získání **CloudBlobContainer** objekt, který má odkazovat na konkrétní objekt blob kontejneru.</span><span class="sxs-lookup"><span data-stu-id="8d517-125">Get a **CloudBlobContainer** object to reference a specific blob container.</span></span>
   
        // Get a reference to a container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [!NOTE]
> <span data-ttu-id="8d517-126">Použijte všechny uvedeném v předchozím postupu před uvedeném v následujících částech kódu kódu.</span><span class="sxs-lookup"><span data-stu-id="8d517-126">Use all of the code shown in the previous procedure in front of the code shown in the following sections.</span></span>
> 
> 

## <a name="create-a-container-in-code"></a><span data-ttu-id="8d517-127">Vytvořit kontejner v kódu</span><span class="sxs-lookup"><span data-stu-id="8d517-127">Create a container in code</span></span>
> [!NOTE]
> <span data-ttu-id="8d517-128">Některé rozhraní API, které provádět volání se do služby Azure Storage v ASP.NET jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="8d517-128">Some APIs that perform calls out to Azure Storage in ASP.NET are asynchronous.</span></span> <span data-ttu-id="8d517-129">V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="8d517-129">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="8d517-130">Kód v následujícím příkladu se předpokládá, že používáte asynchronní programování metody.</span><span class="sxs-lookup"><span data-stu-id="8d517-130">The code in the following example assumes that you are using async programming methods.</span></span>
> 
> 

<span data-ttu-id="8d517-131">Pokud chcete vytvořit kontejner ve vašem účtu úložiště, musíte udělat je přidáte volání **CreateIfNotExistsAsync** jako v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="8d517-131">To create a container in your storage account, all you need to do is add a call to **CreateIfNotExistsAsync** as in the following code:</span></span>

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


<span data-ttu-id="8d517-132">Soubory v kontejneru zpřístupnit všem uživatelům, můžete nastavit kontejner, aby se veřejné pomocí následující kód.</span><span class="sxs-lookup"><span data-stu-id="8d517-132">To make the files within the container available to everyone, you can set the container to be public by using the following code.</span></span>

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


<span data-ttu-id="8d517-133">Kdokoli na Internetu může vidět objekty BLOB ve veřejném kontejneru, ale můžete upravit nebo odstranit pouze v případě, že máte příslušný přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="8d517-133">Anyone on the Internet can see blobs in a public container, but you can modify or delete them only if you have the appropriate access key.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="8d517-134">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="8d517-134">Upload a blob into a container</span></span>
<span data-ttu-id="8d517-135">Úložiště Azure podporuje objekty BLOB bloku a objekty BLOB stránky.</span><span class="sxs-lookup"><span data-stu-id="8d517-135">Azure Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="8d517-136">Ve většině případů se jako vhodný typ k použití doporučuje objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="8d517-136">In the majority of cases, block blob is the recommended type to use.</span></span>

<span data-ttu-id="8d517-137">Když chcete nahrát soubor do objektu blob bloku, získejte odkaz na kontejner a použijte ho k získání odkazu objektu blob bloku.</span><span class="sxs-lookup"><span data-stu-id="8d517-137">To upload a file to a block blob, get a container reference and use it to get a block blob reference.</span></span> <span data-ttu-id="8d517-138">Jakmile získáte odkaz na objekt blob, můžete k němu nahrát jakýkoli proud dat voláním metody **UploadFromStream**.</span><span class="sxs-lookup"><span data-stu-id="8d517-138">Once you have a blob reference, you can upload any stream of data to it by calling the **UploadFromStream** method.</span></span> <span data-ttu-id="8d517-139">Tahle operace vytvoří objekt blob, pokud ještě dříve neexistoval, nebo ho přepíše, pokud už existoval.</span><span class="sxs-lookup"><span data-stu-id="8d517-139">This operation creates the blob if it didn't previously exist, or overwrites it if it does exist.</span></span> <span data-ttu-id="8d517-140">Následující příklad ukazuje, jak nahrát objekt blob do kontejneru, zároveň předpokládá, že kontejner byl již vytvořen.</span><span class="sxs-lookup"><span data-stu-id="8d517-140">The following example shows how to upload a blob into a container and assumes that the container was already created.</span></span>

    // Retrieve a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="8d517-141">Zobrazí seznam objektů blob v kontejneru</span><span class="sxs-lookup"><span data-stu-id="8d517-141">List the blobs in a container</span></span>
<span data-ttu-id="8d517-142">Pokud chcete mít seznam objektů blob v kontejneru, nejdřív získejte odkaz na kontejner.</span><span class="sxs-lookup"><span data-stu-id="8d517-142">To list the blobs in a container, first get a container reference.</span></span> <span data-ttu-id="8d517-143">Pak můžete použít metodu kontejneru **ListBlobs** a načíst objekty blob a/nebo obsažené adresáře.</span><span class="sxs-lookup"><span data-stu-id="8d517-143">You can then use the container's **ListBlobs** method to retrieve the blobs and/or directories within it.</span></span> <span data-ttu-id="8d517-144">Pro přístup k bohaté sadě vlastností a metod vrácené **IListBlobItem**, musíte vysílat na **CloudBlockBlob**, **CloudPageBlob**, nebo  **CloudBlobDirectory** objektu.</span><span class="sxs-lookup"><span data-stu-id="8d517-144">To access the rich set of properties and methods for a  returned **IListBlobItem**, you must cast it to a **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="8d517-145">Pokud je typ neznámý, můžete použít kontrolu typu a zjistit, na který typ vysílat.</span><span class="sxs-lookup"><span data-stu-id="8d517-145">If the type is unknown, you can use a type check to determine which to cast it to.</span></span> <span data-ttu-id="8d517-146">Následující kód ukazuje, jak načíst a na výstupu zobrazit identifikátor URI pro každou položku v kontejneru **photos**:</span><span class="sxs-lookup"><span data-stu-id="8d517-146">The following code demonstrates how to retrieve and output the URI of each item in the **photos** container:</span></span>

    // Loop over items within the container and output the length and URI.
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

<span data-ttu-id="8d517-147">Jak je znázorněno v předchozí ukázce kódu, služby objektů blob obsahuje koncepci adresáře v rámci kontejnery také.</span><span class="sxs-lookup"><span data-stu-id="8d517-147">As shown in the previous code sample, the blob service has the concept of directories within containers, as well.</span></span> <span data-ttu-id="8d517-148">Toto je tak, aby můžete uspořádat, objektů BLOB do více stromové struktury.</span><span class="sxs-lookup"><span data-stu-id="8d517-148">This is so that you can organize your blobs in a more folder-like structure.</span></span> <span data-ttu-id="8d517-149">Můžete zvolit například následující sadu objektů blob bloku v kontejneru s názvem **photos**:</span><span class="sxs-lookup"><span data-stu-id="8d517-149">For example, consider the following set of block blobs in a container named **photos**:</span></span>

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

<span data-ttu-id="8d517-150">Při volání **ListBlobs** na kontejneru (jako v předchozím příkladu), vrátí kolekce obsahuje **CloudBlobDirectory** a **CloudBlockBlob** objekty představují adresáře a objekty BLOB obsažené na nejvyšší úrovni.</span><span class="sxs-lookup"><span data-stu-id="8d517-150">When you call **ListBlobs** on the container (as in the previous sample), the collection returned contains **CloudBlobDirectory** and **CloudBlockBlob** objects representing the directories and blobs contained at the top level.</span></span> <span data-ttu-id="8d517-151">Zde je výsledný výstup:</span><span class="sxs-lookup"><span data-stu-id="8d517-151">Here is the resulting output:</span></span>

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


<span data-ttu-id="8d517-152">Volitelně můžete nastavit parametr **UseFlatBlobListing** metody **ListBlobs** na hodnotu **pravda**.</span><span class="sxs-lookup"><span data-stu-id="8d517-152">Optionally, you can set the **UseFlatBlobListing** parameter of of the **ListBlobs** method to **true**.</span></span> <span data-ttu-id="8d517-153">Výsledkem je každý objekt blob se vrátí jako **CloudBlockBlob**, bez ohledu na to adresáře.</span><span class="sxs-lookup"><span data-stu-id="8d517-153">This results in every blob being returned as a **CloudBlockBlob**, regardless of directory.</span></span> <span data-ttu-id="8d517-154">Tady je volání **ListBlobs**:</span><span class="sxs-lookup"><span data-stu-id="8d517-154">Here is the call to **ListBlobs**:</span></span>

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

<span data-ttu-id="8d517-155">a tady jsou výsledky:</span><span class="sxs-lookup"><span data-stu-id="8d517-155">and here are the results:</span></span>

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

<span data-ttu-id="8d517-156">Další informace najdete v tématu [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d517-156">For more information, see [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).</span></span>

## <a name="download-blobs"></a><span data-ttu-id="8d517-157">Stáhnout objekty blob</span><span class="sxs-lookup"><span data-stu-id="8d517-157">Download blobs</span></span>
<span data-ttu-id="8d517-158">Když chcete stáhnout objekty blob, nejdřív načtěte odkaz objektu blob a potom spusťte volání metody **DownloadToStream**.</span><span class="sxs-lookup"><span data-stu-id="8d517-158">To download blobs, first retrieve a blob reference and then call the **DownloadToStream** method.</span></span> <span data-ttu-id="8d517-159">Následující příklad používá metodu **DownloadToStream** k přenosu obsahu objektu blob na objekt proudu, který potom můžete zachovat trvale v místním souboru.</span><span class="sxs-lookup"><span data-stu-id="8d517-159">The following example uses the **DownloadToStream** method to transfer the blob contents to a stream object that you can then persist to a local file.</span></span>

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

<span data-ttu-id="8d517-160">Můžete také použít metodu **DownloadToStream** a stáhnout obsah objektu blob jako textový řetězec.</span><span class="sxs-lookup"><span data-stu-id="8d517-160">You can also use the **DownloadToStream** method to download the contents of a blob as a text string.</span></span>

    // Get a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a><span data-ttu-id="8d517-161">Odstranění objektů blob</span><span class="sxs-lookup"><span data-stu-id="8d517-161">Delete blobs</span></span>
<span data-ttu-id="8d517-162">Chcete-li odstranit objekt blob, nejdřív získejte odkaz na objekt blob a potom volejte **odstranit** metoda.</span><span class="sxs-lookup"><span data-stu-id="8d517-162">To delete a blob, first get a blob reference and then call the **Delete** method.</span></span>

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a><span data-ttu-id="8d517-163">Asynchronní zobrazení seznamu objektů blob na stránkách</span><span class="sxs-lookup"><span data-stu-id="8d517-163">List blobs in pages asynchronously</span></span>
<span data-ttu-id="8d517-164">Když provádíte výpis velkého počtu objektů blob nebo chcete mít přehled o počtu výsledků, které vrátíte v rámci jedné operace výpisu, můžete vytvořit seznam objektů blob na stránkách s výsledky.</span><span class="sxs-lookup"><span data-stu-id="8d517-164">If you are listing a large number of blobs, or you want to control the number of results you return in one listing operation, you can list blobs in pages of results.</span></span> <span data-ttu-id="8d517-165">Tento příklad ukazuje, jak vracet výsledky na stránkách asynchronně tak, aby čekání na vrácení velké sady výsledků neblokovalo provádění.</span><span class="sxs-lookup"><span data-stu-id="8d517-165">This example shows how to return results in pages asynchronously, so that execution is not blocked while waiting to return a large set of results.</span></span>

<span data-ttu-id="8d517-166">Tento příklad ukazuje plochý výpis objektu blob, můžete ale také provést hierarchický výpis nastavením parametru **useFlatBlobListing** metody **ListBlobsSegmentedAsync** na **false**.</span><span class="sxs-lookup"><span data-stu-id="8d517-166">This example shows a flat blob listing, but you can also perform a hierarchical listing, by setting the **useFlatBlobListing** parameter of the **ListBlobsSegmentedAsync** method to **false**.</span></span>

<span data-ttu-id="8d517-167">Vzhledem k tomu, že metoda ukázky volá asynchronní metodu, musí být uvedena klíčovým slovem **async** a musí vrátit objekt **Úloha**.</span><span class="sxs-lookup"><span data-stu-id="8d517-167">Because the sample method calls an asynchronous method, it must be prefaced with the **async** keyword, and it must return a **Task** object.</span></span> <span data-ttu-id="8d517-168">Klíčové slovo await, určené pro metodu **ListBlobsSegmentedAsync** pozastaví spuštění metody ukázky až do dokončení úlohy vytváření seznamu.</span><span class="sxs-lookup"><span data-stu-id="8d517-168">The await keyword specified for the **ListBlobsSegmentedAsync** method suspends execution of the sample method until the listing task completes.</span></span>

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        // When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            // This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            // or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="next-steps"></a><span data-ttu-id="8d517-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8d517-169">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

