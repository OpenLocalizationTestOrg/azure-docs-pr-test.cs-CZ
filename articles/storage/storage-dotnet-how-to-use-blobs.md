---
title: "aaaGet začít s Azure Blob storage (úložiště objektů) pomocí rozhraní .NET | Microsoft Docs"
description: "Ukládání nestrukturovaných dat v cloudu hello s Azure Blob storage (úložiště objektů)."
services: storage
documentationcenter: .net
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: d18a8fc8-97cb-4d37-a408-a6f8107ea8b3
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/27/2017
ms.author: marsma
ms.openlocfilehash: 9b675ac073e7e901fe1afe54f7bfea12eefbf3ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-using-net"></a><span data-ttu-id="3b592-103">Začínáme s úložištěm Azure Blob pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="3b592-103">Get started with Azure Blob storage using .NET</span></span>

[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

<span data-ttu-id="3b592-104">Azure Blob storage je služba, která ukládá Nestrukturovaná data v cloudu hello jako objekty nebo objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="3b592-104">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="3b592-105">Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b592-105">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="3b592-106">Úložiště objektů blob je také odkazované tooas objektu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3b592-106">Blob storage is also referred tooas object storage.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="3b592-107">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="3b592-107">About this tutorial</span></span>
<span data-ttu-id="3b592-108">Tento kurz ukazuje, jak kód toowrite .NET pro některé běžné scénáře s využitím úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="3b592-108">This tutorial shows how toowrite .NET code for some common scenarios using Azure Blob storage.</span></span> <span data-ttu-id="3b592-109">Mezi zahrnuté scénáře patří odesílání, výpis, stahování a odstraňování objektů blob.</span><span class="sxs-lookup"><span data-stu-id="3b592-109">Scenarios covered include uploading, listing, downloading, and deleting blobs.</span></span>

<span data-ttu-id="3b592-110">**Požadavky:**</span><span class="sxs-lookup"><span data-stu-id="3b592-110">**Prerequisites:**</span></span>

* [<span data-ttu-id="3b592-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3b592-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/)
* [<span data-ttu-id="3b592-112">Klientská knihovna Azure Storage pro .NET</span><span class="sxs-lookup"><span data-stu-id="3b592-112">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="3b592-113">Azure Configuration Manager for .NET</span><span class="sxs-lookup"><span data-stu-id="3b592-113">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* <span data-ttu-id="3b592-114">[Účet úložiště Azure](storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="3b592-114">An [Azure storage account](storage-create-storage-account.md#create-a-storage-account)</span></span>

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a><span data-ttu-id="3b592-115">Další ukázky</span><span class="sxs-lookup"><span data-stu-id="3b592-115">More samples</span></span>
<span data-ttu-id="3b592-116">Další příklady použití Blob Storage najdete v článku [Začínáme s Azure Blob Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="3b592-116">For additional examples using Blob storage, see [Getting Started with Azure Blob Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span></span> <span data-ttu-id="3b592-117">Stáhněte hello ukázkovou aplikaci a potom ho spusťte nebo procházet kód hello na Githubu.</span><span class="sxs-lookup"><span data-stu-id="3b592-117">You can download hello sample application and run it, or browse hello code on GitHub.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="3b592-118">Přidání direktiv using</span><span class="sxs-lookup"><span data-stu-id="3b592-118">Add using directives</span></span>
<span data-ttu-id="3b592-119">Přidejte následující hello **pomocí** direktivy toohello začátek hello `Program.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="3b592-119">Add hello following **using** directives toohello top of hello `Program.cs` file:</span></span>

```csharp
using Microsoft.WindowsAzure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types
```

### <a name="parse-hello-connection-string"></a><span data-ttu-id="3b592-120">Analyzovat hello připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="3b592-120">Parse hello connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-blob-service-client"></a><span data-ttu-id="3b592-121">Vytvoření klienta služby objektů Blob hello</span><span class="sxs-lookup"><span data-stu-id="3b592-121">Create hello Blob service client</span></span>
<span data-ttu-id="3b592-122">Hello **CloudBlobClient** třída umožňuje tooretrieve kontejnery a objekty BLOB uložené v úložišti objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="3b592-122">hello **CloudBlobClient** class enables you tooretrieve containers and blobs stored in Blob storage.</span></span> <span data-ttu-id="3b592-123">Tady je jedním ze způsobů toocreate hello služby klienta:</span><span class="sxs-lookup"><span data-stu-id="3b592-123">Here's one way toocreate hello service client:</span></span>

```csharp
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```
<span data-ttu-id="3b592-124">Teď je připraven toowrite kód, který načítá a zapisuje data tooBlob úložiště.</span><span class="sxs-lookup"><span data-stu-id="3b592-124">Now you are ready toowrite code that reads data from and writes data tooBlob storage.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="3b592-125">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="3b592-125">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="3b592-126">Tento příklad ukazuje, jak toocreate kontejner, pokud ještě neexistuje:</span><span class="sxs-lookup"><span data-stu-id="3b592-126">This example shows how toocreate a container if it does not already exist:</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference tooa container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create hello container if it doesn't already exist.
container.CreateIfNotExists();
```

<span data-ttu-id="3b592-127">Ve výchozím nastavení je hello nový kontejner privátní, což znamená, že úložiště objektů BLOB přístup klíče toodownload z tohoto kontejneru musíte zadat.</span><span class="sxs-lookup"><span data-stu-id="3b592-127">By default, hello new container is private, meaning that you must specify your storage access key toodownload blobs from this container.</span></span> <span data-ttu-id="3b592-128">Pokud chcete soubory hello toomake v rámci dostupné tooeveryone hello kontejneru, můžete nastavit kontejner toobe hello veřejné pomocí hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="3b592-128">If you want toomake hello files within hello container available tooeveryone, you can set hello container toobe public using hello following code:</span></span>

```csharp
container.SetPermissions(
    new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });
```

<span data-ttu-id="3b592-129">Kdokoli na hello Internetu může vidět objekty BLOB ve veřejném kontejneru.</span><span class="sxs-lookup"><span data-stu-id="3b592-129">Anyone on hello Internet can see blobs in a public container.</span></span> <span data-ttu-id="3b592-130">Můžete ale upravit nebo odstranit pouze v případě, že máte přístupový klíč hello odpovídající účtu nebo sdílený přístupový podpis.</span><span class="sxs-lookup"><span data-stu-id="3b592-130">However, you can modify or delete them only if you have hello appropriate account access key or a shared access signature.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="3b592-131">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="3b592-131">Upload a blob into a container</span></span>
<span data-ttu-id="3b592-132">Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky.</span><span class="sxs-lookup"><span data-stu-id="3b592-132">Azure Blob Storage supports block blobs and page blobs.</span></span>  <span data-ttu-id="3b592-133">Ve většině případů je objekt blob bloku hello doporučená toouse typu.</span><span class="sxs-lookup"><span data-stu-id="3b592-133">In most cases, block blob is hello recommended type toouse.</span></span>

<span data-ttu-id="3b592-134">tooupload objekt blob bloku souboru tooa získejte odkaz na kontejner a použít ho tooget odkaz na objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="3b592-134">tooupload a file tooa block blob, get a container reference and use it tooget a block blob reference.</span></span> <span data-ttu-id="3b592-135">Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud dat tooit pomocí volání hello **UploadFromStream** metoda.</span><span class="sxs-lookup"><span data-stu-id="3b592-135">Once you have a blob reference, you can upload any stream of data tooit by calling hello **UploadFromStream** method.</span></span> <span data-ttu-id="3b592-136">Tato operace vytvoří hello blob, pokud nebyla dříve neexistuje, nebo ho přepíše, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="3b592-136">This operation creates hello blob if it didn't previously exist, or overwrites it if it does exist.</span></span>

<span data-ttu-id="3b592-137">Následující příklad ukazuje, jak Hello tooupload objekt blob do kontejneru a předpokládá, že hello kontejner byl již vytvořen.</span><span class="sxs-lookup"><span data-stu-id="3b592-137">hello following example shows how tooupload a blob into a container and assumes that hello container was already created.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "myblob".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

// Create or overwrite hello "myblob" blob with contents from a local file.
using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
{
    blockBlob.UploadFromStream(fileStream);
}
```

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="3b592-138">Seznam hello objekty BLOB v kontejneru</span><span class="sxs-lookup"><span data-stu-id="3b592-138">List hello blobs in a container</span></span>
<span data-ttu-id="3b592-139">toolist hello objekty BLOB v kontejneru, nejdřív získejte odkaz na kontejner.</span><span class="sxs-lookup"><span data-stu-id="3b592-139">toolist hello blobs in a container, first get a container reference.</span></span> <span data-ttu-id="3b592-140">Pak můžete použít hello kontejneru **ListBlobs** metoda tooretrieve hello objekty BLOB a/nebo obsažené adresáře.</span><span class="sxs-lookup"><span data-stu-id="3b592-140">You can then use hello container's **ListBlobs** method tooretrieve hello blobs and/or directories within it.</span></span> <span data-ttu-id="3b592-141">tooaccess hello bohatou sadu vlastností a metod vrácené **IListBlobItem**, musíte vysílat tooa **CloudBlockBlob**, **CloudPageBlob**, nebo  **CloudBlobDirectory** objektu.</span><span class="sxs-lookup"><span data-stu-id="3b592-141">tooaccess hello  rich set of properties and methods for a returned **IListBlobItem**, you must cast it tooa **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="3b592-142">Pokud typ hello neznámý, můžete použít typ kontroly toodetermine které toocast jeho.</span><span class="sxs-lookup"><span data-stu-id="3b592-142">If hello type is unknown, you can use a type check toodetermine which toocast it to.</span></span> <span data-ttu-id="3b592-143">Hello následující kód ukazuje, jak tooretrieve a výstup hello URI pro každou položku v hello _fotografie_ kontejneru:</span><span class="sxs-lookup"><span data-stu-id="3b592-143">hello following code demonstrates how tooretrieve and output hello URI of each item in hello _photos_ container:</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("photos");

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
```

<span data-ttu-id="3b592-144">Zahrnutím informací o cestě do názvu objektu blob můžete vytvořit virtuální adresářovou strukturu, kterou můžete organizovat a procházet podle potřeby jako u tradičních systémů souborů.</span><span class="sxs-lookup"><span data-stu-id="3b592-144">By including path information in blob names, you can create a virtual directory structure you can organize and traverse as you would a traditional file system.</span></span> <span data-ttu-id="3b592-145">struktura adresářů Hello je virtuální pouze – hello jenom prostředky, které jsou k dispozici v úložišti objektů Blob jsou kontejnery a objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="3b592-145">hello directory structure is virtual only--hello only resources available in Blob storage are containers and blobs.</span></span> <span data-ttu-id="3b592-146">Však nabízí hello Klientská knihovna pro úložiště **CloudBlobDirectory** objektu toorefer tooa virtuální adresář a zjednodušit proces hello práce s objekty BLOB, které jsou tímto způsobem uspořádány.</span><span class="sxs-lookup"><span data-stu-id="3b592-146">However, hello storage client library offers a **CloudBlobDirectory** object toorefer tooa virtual directory and simplify hello process of working with blobs that are organized in this way.</span></span>

<span data-ttu-id="3b592-147">Zvažte například následující sadu objektů BLOB bloku v kontejneru nazvaném hello *fotografie*:</span><span class="sxs-lookup"><span data-stu-id="3b592-147">For example, consider hello following set of block blobs in a container named *photos*:</span></span>

```
photo1.jpg
2010/architecture/description.txt
2010/architecture/photo3.jpg
2010/architecture/photo4.jpg
2011/architecture/photo5.jpg
2011/architecture/photo6.jpg
2011/architecture/description.txt
2011/photo7.jpg
```

<span data-ttu-id="3b592-148">Při volání **ListBlobs** na hello *fotografie* kontejneru (jako hello předchozím fragmentu kódu), se vrátí hierarchický výpis.</span><span class="sxs-lookup"><span data-stu-id="3b592-148">When you call **ListBlobs** on hello *photos* container (as in hello preceding code snippet), a hierarchical listing is returned.</span></span> <span data-ttu-id="3b592-149">Obsahuje oba **CloudBlobDirectory** a **CloudBlockBlob** objekty, která reprezentuje hello adresáře a objekty BLOB v kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="3b592-149">It contains both **CloudBlobDirectory** and **CloudBlockBlob** objects, representing hello directories and blobs in hello container, respectively.</span></span> <span data-ttu-id="3b592-150">Hello výsledný výstup vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="3b592-150">hello resulting output looks like:</span></span>

```
Directory: https://<accountname>.blob.core.windows.net/photos/2010/
Directory: https://<accountname>.blob.core.windows.net/photos/2011/
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

<span data-ttu-id="3b592-151">Volitelně můžete nastavit hello **UseFlatBlobListing** parametr hello **ListBlobs** metodu **true**.</span><span class="sxs-lookup"><span data-stu-id="3b592-151">Optionally, you can set hello **UseFlatBlobListing** parameter of hello **ListBlobs** method to **true**.</span></span> <span data-ttu-id="3b592-152">V takovém případě se každý objekt blob v kontejneru hello vrátí jako **CloudBlockBlob** objektu.</span><span class="sxs-lookup"><span data-stu-id="3b592-152">In this case, every blob in hello container is returned as a **CloudBlockBlob** object.</span></span> <span data-ttu-id="3b592-153">Hello volání příliš**ListBlobs** tooreturn plochým výpisem vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="3b592-153">hello call too**ListBlobs** tooreturn a flat listing looks like this:</span></span>

```csharp
// Loop over items within hello container and output hello length and URI.
foreach (IListBlobItem item in container.ListBlobs(null, true))
{
    ...
}
```

<span data-ttu-id="3b592-154">a výsledky hello vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="3b592-154">and hello results look like this:</span></span>

```
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

## <a name="download-blobs"></a><span data-ttu-id="3b592-155">Stáhnout objekty blob</span><span class="sxs-lookup"><span data-stu-id="3b592-155">Download blobs</span></span>
<span data-ttu-id="3b592-156">toodownload objekty BLOB, nejdřív načtěte odkaz na objekt blob a pak zavolají hello **DownloadToStream** metoda.</span><span class="sxs-lookup"><span data-stu-id="3b592-156">toodownload blobs, first retrieve a blob reference and then call hello **DownloadToStream** method.</span></span> <span data-ttu-id="3b592-157">Hello následující příklad používá hello **DownloadToStream** metoda tootransfer hello blob obsah tooa objektu stream, potom můžete zachovat tooa místního souboru.</span><span class="sxs-lookup"><span data-stu-id="3b592-157">hello following example uses hello **DownloadToStream** method tootransfer hello blob contents tooa stream object that you can then persist tooa local file.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "photo1.jpg".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

// Save blob contents tooa file.
using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
{
    blockBlob.DownloadToStream(fileStream);
}
```

<span data-ttu-id="3b592-158">Můžete taky hello **DownloadToStream** metoda toodownload hello obsah objektu blob jako textový řetězec.</span><span class="sxs-lookup"><span data-stu-id="3b592-158">You can also use hello **DownloadToStream** method toodownload hello contents of a blob as a text string.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "myblob.txt"
CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

string text;
using (var memoryStream = new MemoryStream())
{
    blockBlob2.DownloadToStream(memoryStream);
    text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
}
```

## <a name="delete-blobs"></a><span data-ttu-id="3b592-159">Odstranění objektů blob</span><span class="sxs-lookup"><span data-stu-id="3b592-159">Delete blobs</span></span>
<span data-ttu-id="3b592-160">toodelete objekt blob, nejdřív získejte odkaz na objekt blob a potom zavolejte **odstranit** metoda na něm.</span><span class="sxs-lookup"><span data-stu-id="3b592-160">toodelete a blob, first get a blob reference and then call the **Delete** method on it.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "myblob.txt".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

// Delete hello blob.
blockBlob.Delete();
```

## <a name="list-blobs-in-pages-asynchronously"></a><span data-ttu-id="3b592-161">Asynchronní zobrazení seznamu objektů blob na stránkách</span><span class="sxs-lookup"><span data-stu-id="3b592-161">List blobs in pages asynchronously</span></span>
<span data-ttu-id="3b592-162">Pokud provádíte výpis velkého počtu objektů BLOB nebo chcete toocontrol hello počet výsledků, které vrátíte v rámci jedné operace výpisu, můžete vytvořit seznam objektů blob na stránkách s výsledky.</span><span class="sxs-lookup"><span data-stu-id="3b592-162">If you are listing a large number of blobs, or you want toocontrol hello number of results you return in one listing operation, you can list blobs in pages of results.</span></span> <span data-ttu-id="3b592-163">Tento příklad ukazuje, jak tooreturn výsledky na stránkách asynchronně, takže čekání tooreturn velké sady výsledků neblokovalo provádění.</span><span class="sxs-lookup"><span data-stu-id="3b592-163">This example shows how tooreturn results in pages asynchronously, so that execution is not blocked while waiting tooreturn a large set of results.</span></span>

<span data-ttu-id="3b592-164">Tento příklad ukazuje výpis plochého objektu blob ale můžete také provést hierarchický výpis podle nastavení hello _useFlatBlobListing_ parametr hello **ListBlobsSegmentedAsync** too_false_ metoda.</span><span class="sxs-lookup"><span data-stu-id="3b592-164">This example shows a flat blob listing, but you can also perform a hierarchical listing, by setting hello _useFlatBlobListing_ parameter of hello **ListBlobsSegmentedAsync** method too_false_.</span></span>

<span data-ttu-id="3b592-165">Protože hello metoda ukázky volá asynchronní metodu, musí být uvedena s hello _asynchronní_ – klíčové slovo a musí vrátit **úloh** objektu.</span><span class="sxs-lookup"><span data-stu-id="3b592-165">Because hello sample method calls an asynchronous method, it must be prefaced with hello _async_ keyword, and it must return a **Task** object.</span></span> <span data-ttu-id="3b592-166">await – klíčové slovo zadané pro hello Hello **ListBlobsSegmentedAsync** pozastaví spuštění metody ukázky hello až do dokončení úlohy vytváření seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="3b592-166">hello await keyword specified for hello **ListBlobsSegmentedAsync** method suspends execution of hello sample method until hello listing task completes.</span></span>

```csharp
async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
{
    //List blobs toohello console window, with paging.
    Console.WriteLine("List blobs in pages:");

    int i = 0;
    BlobContinuationToken continuationToken = null;
    BlobResultSegment resultSegment = null;

    //Call ListBlobsSegmentedAsync and enumerate hello result segment returned, while hello continuation token is non-null.
    //When hello continuation token is null, hello last page has been returned and execution can exit hello loop.
    do
    {
        //This overload allows control of hello page size. You can return all remaining results by passing null for hello maxResults parameter,
        //or by calling a different overload.
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
```

## <a name="writing-tooan-append-blob"></a><span data-ttu-id="3b592-167">Zápis tooan připojit objektů blob</span><span class="sxs-lookup"><span data-stu-id="3b592-167">Writing tooan append blob</span></span>
<span data-ttu-id="3b592-168">Doplňovací objekt blob je optimalizován pro operace připojení, například protokolování.</span><span class="sxs-lookup"><span data-stu-id="3b592-168">An append blob is optimized for append operations, such as logging.</span></span> <span data-ttu-id="3b592-169">Podobně jako objekt blob bloku doplňovací objekt blob se skládá z bloků, ale když přidáte nový objekt blob bloku připojení tooan, je vždy připojením toohello konec objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="3b592-169">Like a block blob, an append blob is comprised of blocks, but when you add a new block tooan append blob, it is always appended toohello end of hello blob.</span></span> <span data-ttu-id="3b592-170">Existující blok v doplňovacím objektu blob se nedá aktualizovat ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="3b592-170">You cannot update or delete an existing block in an append blob.</span></span> <span data-ttu-id="3b592-171">ID Hello bloku pro doplňovací objekt blob nejsou vystavená, protože jsou pro objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="3b592-171">hello block IDs for an append blob are not exposed as they are for a block blob.</span></span>

<span data-ttu-id="3b592-172">Každý blok v doplňovacím objektu blob může mít různou velikost, až tooa nesmí být delší než 4 MB volného místa, a doplňovací objekt blob může obsahovat maximálně 50 000 bloků.</span><span class="sxs-lookup"><span data-stu-id="3b592-172">Each block in an append blob can be a different size, up tooa maximum of 4 MB, and an append blob can include a maximum of 50,000 blocks.</span></span> <span data-ttu-id="3b592-173">Hello maximální velikost doplňovacího objektu BLOB je proto něco větší než 195 GB (4 MB × 50 000 bloků).</span><span class="sxs-lookup"><span data-stu-id="3b592-173">hello maximum size of an append blob is therefore slightly more than 195 GB (4 MB X 50,000 blocks).</span></span>

<span data-ttu-id="3b592-174">Následující příklad Hello vytvoří nový doplňovací objekt blob a připojí některá data tooit simulaci jednoduché operace protokolování.</span><span class="sxs-lookup"><span data-stu-id="3b592-174">hello example below creates a new append blob and appends some data tooit, simulating a simple logging operation.</span></span>

```csharp
//Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

//Create service client for credentialed access toohello Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference tooa container.
CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

//Create hello container if it does not already exist.
container.CreateIfNotExists();

//Get a reference tooan append blob.
CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

//Create hello append blob. Note that if hello blob already exists, hello CreateOrReplace() method will overwrite it.
//You can check whether hello blob exists tooavoid overwriting it by using CloudAppendBlob.Exists().
appendBlob.CreateOrReplace();

int numBlocks = 10;

//Generate an array of random bytes.
Random rnd = new Random();
byte[] bytes = new byte[numBlocks];
rnd.NextBytes(bytes);

//Simulate a logging operation by writing text data and byte data toohello end of hello append blob.
for (int i = 0; i < numBlocks; i++)
{
    appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
        DateTime.UtcNow, bytes[i], Environment.NewLine));
}

//Read hello append blob toohello console window.
Console.WriteLine(appendBlob.DownloadText());
```

<span data-ttu-id="3b592-175">V tématu [objekty BLOB bloku pochopení, objekty BLOB stránky a doplňovacích objektů blob](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) Další informace o hello rozdíly mezi hello tři typy objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="3b592-175">See [Understanding Block Blobs, Page Blobs, and Append Blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) for more information about hello differences between hello three types of blobs.</span></span>

## <a name="managing-security-for-blobs"></a><span data-ttu-id="3b592-176">Správa zabezpečení pro objekty blob</span><span class="sxs-lookup"><span data-stu-id="3b592-176">Managing security for blobs</span></span>
<span data-ttu-id="3b592-177">Ve výchozím nastavení Azure Storage zajišťuje ochranu dat omezením vlastníka účtu toohello přístupu, která je vlastníkem hello přístupových klíčů k účtu.</span><span class="sxs-lookup"><span data-stu-id="3b592-177">By default, Azure Storage keeps your data secure by limiting access toohello account owner, who is in possession of hello account access keys.</span></span> <span data-ttu-id="3b592-178">Pokud potřebujete tooshare data objektů blob v účtu úložiště, je důležité toodo Ano, aniž by to ohrozilo zabezpečení hello klíče pro přístup k účtu.</span><span class="sxs-lookup"><span data-stu-id="3b592-178">When you need tooshare blob data in your storage account, it is important toodo so without compromising hello security of your account access keys.</span></span> <span data-ttu-id="3b592-179">Navíc můžete šifrovat tooensure data objektů blob, který je zabezpečený přenos přes přenosu hello a ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3b592-179">Additionally, you can encrypt blob data tooensure that it is secure going over hello wire and in Azure Storage.</span></span>

[!INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-tooblob-data"></a><span data-ttu-id="3b592-180">Řízení přístupu tooblob dat</span><span class="sxs-lookup"><span data-stu-id="3b592-180">Controlling access tooblob data</span></span>
<span data-ttu-id="3b592-181">Ve výchozím nastavení hello data objektů blob v účtu úložiště je dostupný pouze vlastník účtu toostorage.</span><span class="sxs-lookup"><span data-stu-id="3b592-181">By default, hello blob data in your storage account is accessible only toostorage account owner.</span></span> <span data-ttu-id="3b592-182">Ve výchozím nastavení ověřování požadavků na úložiště objektů Blob vyžaduje přístupový klíč účtu hello.</span><span class="sxs-lookup"><span data-stu-id="3b592-182">Authenticating requests against Blob storage requires hello account access key by default.</span></span> <span data-ttu-id="3b592-183">Můžete ale taky toomake určitých objektů blob data k dispozici tooother uživatele.</span><span class="sxs-lookup"><span data-stu-id="3b592-183">However, you may wish toomake certain blob data available tooother users.</span></span> <span data-ttu-id="3b592-184">Máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="3b592-184">You have two options:</span></span>

* <span data-ttu-id="3b592-185">**Anonymní přístup:** můžete nastavit kontejner nebo jeho objekty blob na veřejně dostupné pro anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="3b592-185">**Anonymous access:** You can make a container or its blobs publicly available for anonymous access.</span></span> <span data-ttu-id="3b592-186">V tématu [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](storage-manage-access-to-resources.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="3b592-186">See [Manage anonymous read access toocontainers and blobs](storage-manage-access-to-resources.md) for more information.</span></span>
* <span data-ttu-id="3b592-187">**Sdílené přístupové podpisy:** klientům můžete zajistit sdílený přístupový podpis (SAS), který poskytuje Delegovaný přístup tooa prostředků ve vašem účtu úložiště s oprávněními, které zadáte a v intervalu, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="3b592-187">**Shared access signatures:** You can provide clients with a shared access signature (SAS), which provides delegated access tooa resource in your storage account, with permissions that you specify and over an interval that you specify.</span></span> <span data-ttu-id="3b592-188">Další informace najdete v tématu [Použití sdílených přístupových podpisů (SAS)](storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="3b592-188">See [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) for more information.</span></span>

### <a name="encrypting-blob-data"></a><span data-ttu-id="3b592-189">Šifrování dat objektů blob</span><span class="sxs-lookup"><span data-stu-id="3b592-189">Encrypting blob data</span></span>
<span data-ttu-id="3b592-190">Úložiště Azure podporuje šifrování dat objektů blob na hello klientovi i na serveru hello:</span><span class="sxs-lookup"><span data-stu-id="3b592-190">Azure Storage supports encrypting blob data both at hello client and on hello server:</span></span>

* <span data-ttu-id="3b592-191">**Šifrování na straně klienta:** hello Klientská knihovna pro úložiště pro .NET podporuje šifrování dat v rámci klientské aplikace před nahráním tooAzure úložiště a dešifrování dat při stahování toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="3b592-191">**Client-side encryption:** hello Storage Client Library for .NET supports encrypting data within client applications before uploading tooAzure Storage, and decrypting data while downloading toohello client.</span></span> <span data-ttu-id="3b592-192">Hello knihovna také podporuje integraci s Azure Key Vault pro správu klíčů účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3b592-192">hello library also supports integration with Azure Key Vault for storage account key management.</span></span> <span data-ttu-id="3b592-193">Další informace viz [Šifrování na straně klienta s .NET pro úložiště Microsoft Azure](storage-client-side-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="3b592-193">See [Client-Side Encryption with .NET for Microsoft Azure Storage](storage-client-side-encryption.md) for more information.</span></span> <span data-ttu-id="3b592-194">Viz také [Kurz: Šifrování a dešifrování objektů blob v úložišti Microsoft Azure pomocí služby Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="3b592-194">Also see [Tutorial: Encrypt and decrypt blobs in Microsoft Azure Storage using Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md).</span></span>
* <span data-ttu-id="3b592-195">**Šifrování na straně serveru**: Úložiště Azure nyní podporuje šifrování na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="3b592-195">**Server-side encryption**: Azure Storage now supports server-side encryption.</span></span> <span data-ttu-id="3b592-196">Viz [Šifrování služby Azure Storage Service pro Neaktivní uložená data (Náhled)](storage-service-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="3b592-196">See [Azure Storage Service Encryption for Data at Rest (Preview)](storage-service-encryption.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b592-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3b592-197">Next steps</span></span>
<span data-ttu-id="3b592-198">Teď, když jste se naučili základy používání Blob storage hello, postupujte podle těchto odkazů toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="3b592-198">Now that you've learned hello basics of Blob storage, follow these links toolearn more.</span></span>

### <a name="microsoft-azure-storage-explorer"></a><span data-ttu-id="3b592-199">Microsoft Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="3b592-199">Microsoft Azure Storage Explorer</span></span>
* <span data-ttu-id="3b592-200">[Microsoft Azure Storage Explorer (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="3b592-200">[Microsoft Azure Storage Explorer (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

### <a name="blob-storage-samples"></a><span data-ttu-id="3b592-201">Ukázky Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3b592-201">Blob storage samples</span></span>
* [<span data-ttu-id="3b592-202">Začínáme s úložištěm Azure Blob Storage pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="3b592-202">Getting Started with Azure Blob Storage in .NET</span></span>](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a><span data-ttu-id="3b592-203">Odkazy Blob storage</span><span class="sxs-lookup"><span data-stu-id="3b592-203">Blob storage reference</span></span>
* [<span data-ttu-id="3b592-204">Klientská knihovna pro úložiště pro .NET – referenční informace</span><span class="sxs-lookup"><span data-stu-id="3b592-204">Storage Client Library for .NET reference</span></span>](https://msdn.microsoft.com/library/azure/mt347887.aspx)
* [<span data-ttu-id="3b592-205">REST API – referenční informace</span><span class="sxs-lookup"><span data-stu-id="3b592-205">REST API reference</span></span>](/rest/api/storageservices/azure-storage-services-rest-api-reference)

### <a name="conceptual-guides"></a><span data-ttu-id="3b592-206">Koncepční vodítka</span><span class="sxs-lookup"><span data-stu-id="3b592-206">Conceptual guides</span></span>
* [<span data-ttu-id="3b592-207">Přenos dat pomocí hello příkazového řádku azcopy</span><span class="sxs-lookup"><span data-stu-id="3b592-207">Transfer data with hello AzCopy command-line utility</span></span>](storage-use-azcopy.md)
* [<span data-ttu-id="3b592-208">Začínáme se službou File Storage for .NET</span><span class="sxs-lookup"><span data-stu-id="3b592-208">Get started with File storage for .NET</span></span>](storage-dotnet-how-to-use-files.md)
* [<span data-ttu-id="3b592-209">Jak toouse Azure blob storage s hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="3b592-209">How toouse Azure blob storage with hello WebJobs SDK</span></span>](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
