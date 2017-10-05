---
title: "Začínáme s Azure Blob Storage (úložiště objektů) pomocí rozhraní .NET | Dokumentace Microsoftu"
description: "Ukládejte nestrukturovaná data v cloudu pomocí Azure Blob Storage (úložiště objektů)."
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
ms.openlocfilehash: 70c7d6a5e1b9aa9a13481893e0baa56538be097c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-blob-storage-using-net"></a><span data-ttu-id="ba1dd-103">Začínáme s úložištěm Azure Blob pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="ba1dd-103">Get started with Azure Blob storage using .NET</span></span>

[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

<span data-ttu-id="ba1dd-104">Úložiště objektů blob v Azure je služba, která ukládá nestrukturovaná data v cloudu jako objekty nebo objekty blob.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-104">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="ba1dd-105">Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-105">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="ba1dd-106">Blob storage se také nazývá úložiště objektů.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-106">Blob storage is also referred to as object storage.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="ba1dd-107">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="ba1dd-107">About this tutorial</span></span>
<span data-ttu-id="ba1dd-108">Tenhle kurz ukazuje, jak napsat kód .NET pro některé běžné scénáře s využitím Úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-108">This tutorial shows how to write .NET code for some common scenarios using Azure Blob storage.</span></span> <span data-ttu-id="ba1dd-109">Mezi zahrnuté scénáře patří odesílání, výpis, stahování a odstraňování objektů blob.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-109">Scenarios covered include uploading, listing, downloading, and deleting blobs.</span></span>

<span data-ttu-id="ba1dd-110">**Požadavky:**</span><span class="sxs-lookup"><span data-stu-id="ba1dd-110">**Prerequisites:**</span></span>

* [<span data-ttu-id="ba1dd-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ba1dd-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/)
* [<span data-ttu-id="ba1dd-112">Klientská knihovna Azure Storage pro .NET</span><span class="sxs-lookup"><span data-stu-id="ba1dd-112">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="ba1dd-113">Azure Configuration Manager for .NET</span><span class="sxs-lookup"><span data-stu-id="ba1dd-113">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* <span data-ttu-id="ba1dd-114">[Účet úložiště Azure](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="ba1dd-114">An [Azure storage account](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-storage-account)</span></span>

[!INCLUDE [storage-dotnet-client-library-version-include](../../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a><span data-ttu-id="ba1dd-115">Další ukázky</span><span class="sxs-lookup"><span data-stu-id="ba1dd-115">More samples</span></span>
<span data-ttu-id="ba1dd-116">Další příklady použití Blob Storage najdete v článku [Začínáme s Azure Blob Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="ba1dd-116">For additional examples using Blob storage, see [Getting Started with Azure Blob Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span></span> <span data-ttu-id="ba1dd-117">Můžete si stáhnout a spustit ukázkovou aplikaci nebo si prohlédnout kód na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-117">You can download the sample application and run it, or browse the code on GitHub.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="ba1dd-118">Přidání direktiv using</span><span class="sxs-lookup"><span data-stu-id="ba1dd-118">Add using directives</span></span>
<span data-ttu-id="ba1dd-119">Na začátek souboru `Program.cs` přidejte následující direktivy **using**:</span><span class="sxs-lookup"><span data-stu-id="ba1dd-119">Add the following **using** directives to the top of the `Program.cs` file:</span></span>

```csharp
using Microsoft.WindowsAzure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types
```

### <a name="parse-the-connection-string"></a><span data-ttu-id="ba1dd-120">Analýza připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="ba1dd-120">Parse the connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a><span data-ttu-id="ba1dd-121">Vytvoření klienta služby objektu blob</span><span class="sxs-lookup"><span data-stu-id="ba1dd-121">Create the Blob service client</span></span>
<span data-ttu-id="ba1dd-122">Třída **CloudBlobClient** umožňuje načíst kontejnery a objekty blob, které jsou uloženy v Blob storage.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-122">The **CloudBlobClient** class enables you to retrieve containers and blobs stored in Blob storage.</span></span> <span data-ttu-id="ba1dd-123">Tady je jeden ze způsobů, jak vytvořit klienta služby:</span><span class="sxs-lookup"><span data-stu-id="ba1dd-123">Here's one way to create the service client:</span></span>

```csharp
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```
<span data-ttu-id="ba1dd-124">Teď můžete napsat kód, který bude číst data z Blob storage a bude je tam také zapisovat.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-124">Now you are ready to write code that reads data from and writes data to Blob storage.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="ba1dd-125">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="ba1dd-125">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="ba1dd-126">Tento příklad ukazuje, jak vytvořit kontejner, pokud ještě neexistuje:</span><span class="sxs-lookup"><span data-stu-id="ba1dd-126">This example shows how to create a container if it does not already exist:</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create the container if it doesn't already exist.
container.CreateIfNotExists();
```

<span data-ttu-id="ba1dd-127">Ve výchozím nastavení je nový kontejner privátní, což znamená, že ke stažení objektů blob z tohoto kontejneru musíte zadat přístupový klíč úložiště.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-127">By default, the new container is private, meaning that you must specify your storage access key to download blobs from this container.</span></span> <span data-ttu-id="ba1dd-128">Když chcete, aby soubory v kontejneru byly k dispozici všem uživatelům, můžete jej pomocí následujícího kódu nastavit jako veřejný:</span><span class="sxs-lookup"><span data-stu-id="ba1dd-128">If you want to make the files within the container available to everyone, you can set the container to be public using the following code:</span></span>

```csharp
container.SetPermissions(
    new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });
```

<span data-ttu-id="ba1dd-129">Kdokoli na internetu může vidět objekty blob ve veřejném kontejneru.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-129">Anyone on the Internet can see blobs in a public container.</span></span> <span data-ttu-id="ba1dd-130">Upravit nebo odstranit je ale můžete jenom v případě, že máte příslušný přístupový klíč k účtu nebo sdílený přístupový podpis.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-130">However, you can modify or delete them only if you have the appropriate account access key or a shared access signature.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="ba1dd-131">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="ba1dd-131">Upload a blob into a container</span></span>
<span data-ttu-id="ba1dd-132">Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-132">Azure Blob Storage supports block blobs and page blobs.</span></span>  <span data-ttu-id="ba1dd-133">Ve většině případů se jako vhodný typ k použití doporučuje objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-133">In most cases, block blob is the recommended type to use.</span></span>

<span data-ttu-id="ba1dd-134">Když chcete nahrát soubor do objektu blob bloku, získejte odkaz na kontejner a použijte ho k získání odkazu objektu blob bloku.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-134">To upload a file to a block blob, get a container reference and use it to get a block blob reference.</span></span> <span data-ttu-id="ba1dd-135">Jakmile získáte odkaz na objekt blob, můžete k němu nahrát jakýkoli proud dat voláním metody **UploadFromStream**.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-135">Once you have a blob reference, you can upload any stream of data to it by calling the **UploadFromStream** method.</span></span> <span data-ttu-id="ba1dd-136">Tahle operace vytvoří objekt blob, pokud ještě dříve neexistoval, nebo ho přepíše, pokud už existoval.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-136">This operation creates the blob if it didn't previously exist, or overwrites it if it does exist.</span></span>

<span data-ttu-id="ba1dd-137">Následující příklad ukazuje, jak nahrát objekt blob do kontejneru, zároveň předpokládá, že kontejner byl již vytvořen.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-137">The following example shows how to upload a blob into a container and assumes that the container was already created.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference to a previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference to a blob named "myblob".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

// Create or overwrite the "myblob" blob with contents from a local file.
using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
{
    blockBlob.UploadFromStream(fileStream);
}
```

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="ba1dd-138">Zobrazí seznam objektů blob v kontejneru</span><span class="sxs-lookup"><span data-stu-id="ba1dd-138">List the blobs in a container</span></span>
<span data-ttu-id="ba1dd-139">Pokud chcete mít seznam objektů blob v kontejneru, nejdřív získejte odkaz na kontejner.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-139">To list the blobs in a container, first get a container reference.</span></span> <span data-ttu-id="ba1dd-140">Pak můžete použít metodu kontejneru **ListBlobs** a načíst objekty blob a/nebo obsažené adresáře.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-140">You can then use the container's **ListBlobs** method to retrieve the blobs and/or directories within it.</span></span> <span data-ttu-id="ba1dd-141">Pro přístup k bohaté sadě vlastností a metod vrácené položky **IListBlobItem** musíte vysílat na objekt **CloudBlockBlob**, **CloudPageBlob** nebo **CloudBlobDirectory**.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-141">To access the  rich set of properties and methods for a returned **IListBlobItem**, you must cast it to a **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="ba1dd-142">Pokud je typ neznámý, můžete použít kontrolu typu a zjistit, na který typ vysílat.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-142">If the type is unknown, you can use a type check to determine which to cast it to.</span></span> <span data-ttu-id="ba1dd-143">Následující kód ukazuje, jak načíst a na výstupu zobrazit identifikátor URI pro každou položku v kontejneru _photos_:</span><span class="sxs-lookup"><span data-stu-id="ba1dd-143">The following code demonstrates how to retrieve and output the URI of each item in the _photos_ container:</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference to a previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("photos");

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
```

<span data-ttu-id="ba1dd-144">Zahrnutím informací o cestě do názvu objektu blob můžete vytvořit virtuální adresářovou strukturu, kterou můžete organizovat a procházet podle potřeby jako u tradičních systémů souborů.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-144">By including path information in blob names, you can create a virtual directory structure you can organize and traverse as you would a traditional file system.</span></span> <span data-ttu-id="ba1dd-145">Adresářová struktura je jenom virtuální – jediné prostředky, které jsou dostupné v Blob Storage, jsou kontejnery a objekty blob.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-145">The directory structure is virtual only--the only resources available in Blob storage are containers and blobs.</span></span> <span data-ttu-id="ba1dd-146">Klientská knihovna pro úložiště nabízí objekt **CloudBlobDirectory**, který má odkazovat na virtuální adresář a zjednodušit tak práci s objekty blob, které jsou tímto způsobem uspořádány.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-146">However, the storage client library offers a **CloudBlobDirectory** object to refer to a virtual directory and simplify the process of working with blobs that are organized in this way.</span></span>

<span data-ttu-id="ba1dd-147">Můžete zvolit například následující sadu objektů blob bloku v kontejneru s názvem *photos*:</span><span class="sxs-lookup"><span data-stu-id="ba1dd-147">For example, consider the following set of block blobs in a container named *photos*:</span></span>

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

<span data-ttu-id="ba1dd-148">Při volání **ListBlobs** na kontejneru *photos* (viz předchozí fragment kódu) se vrátí hierarchický výpis.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-148">When you call **ListBlobs** on the *photos* container (as in the preceding code snippet), a hierarchical listing is returned.</span></span> <span data-ttu-id="ba1dd-149">Obsahuje oba objekty **CloudBlobDirectory** a **CloudBlockBlob**, které představují adresáře a objekty blob v kontejneru, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-149">It contains both **CloudBlobDirectory** and **CloudBlockBlob** objects, representing the directories and blobs in the container, respectively.</span></span> <span data-ttu-id="ba1dd-150">Výsledný výstup vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="ba1dd-150">The resulting output looks like:</span></span>

```
Directory: https://<accountname>.blob.core.windows.net/photos/2010/
Directory: https://<accountname>.blob.core.windows.net/photos/2011/
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

<span data-ttu-id="ba1dd-151">Volitelně můžete nastavit parametr **UseFlatBlobListing** metody **ListBlobs** na hodnotu **true**.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-151">Optionally, you can set the **UseFlatBlobListing** parameter of the **ListBlobs** method to **true**.</span></span> <span data-ttu-id="ba1dd-152">V takovém případě bude každý objekt blob v kontejneru vrácen jako objekt **CloudBlockBlob**.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-152">In this case, every blob in the container is returned as a **CloudBlockBlob** object.</span></span> <span data-ttu-id="ba1dd-153">Volání **ListBlobs** s vráceným plochým výpisem vypadá takhle:</span><span class="sxs-lookup"><span data-stu-id="ba1dd-153">The call to **ListBlobs** to return a flat listing looks like this:</span></span>

```csharp
// Loop over items within the container and output the length and URI.
foreach (IListBlobItem item in container.ListBlobs(null, true))
{
    ...
}
```

<span data-ttu-id="ba1dd-154">a výsledky vypadají takhle:</span><span class="sxs-lookup"><span data-stu-id="ba1dd-154">and the results look like this:</span></span>

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

## <a name="download-blobs"></a><span data-ttu-id="ba1dd-155">Stáhnout objekty blob</span><span class="sxs-lookup"><span data-stu-id="ba1dd-155">Download blobs</span></span>
<span data-ttu-id="ba1dd-156">Když chcete stáhnout objekty blob, nejdřív načtěte odkaz objektu blob a potom spusťte volání metody **DownloadToStream**.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-156">To download blobs, first retrieve a blob reference and then call the **DownloadToStream** method.</span></span> <span data-ttu-id="ba1dd-157">Následující příklad používá metodu **DownloadToStream** k přenosu obsahu objektu blob na objekt proudu, který potom můžete zachovat trvale v místním souboru.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-157">The following example uses the **DownloadToStream** method to transfer the blob contents to a stream object that you can then persist to a local file.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference to a previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference to a blob named "photo1.jpg".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

// Save blob contents to a file.
using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
{
    blockBlob.DownloadToStream(fileStream);
}
```

<span data-ttu-id="ba1dd-158">Můžete také použít metodu **DownloadToStream** a stáhnout obsah objektu blob jako textový řetězec.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-158">You can also use the **DownloadToStream** method to download the contents of a blob as a text string.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference to a previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference to a blob named "myblob.txt"
CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

string text;
using (var memoryStream = new MemoryStream())
{
    blockBlob2.DownloadToStream(memoryStream);
    text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
}
```

## <a name="delete-blobs"></a><span data-ttu-id="ba1dd-159">Odstranění objektů blob</span><span class="sxs-lookup"><span data-stu-id="ba1dd-159">Delete blobs</span></span>
<span data-ttu-id="ba1dd-160">Pokud chcete odstranit objekt blob, nejdřív získejte odkaz na objekt blob a potom na něm spusťte volání metody **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-160">To delete a blob, first get a blob reference and then call the **Delete** method on it.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference to a previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference to a blob named "myblob.txt".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

// Delete the blob.
blockBlob.Delete();
```

## <a name="list-blobs-in-pages-asynchronously"></a><span data-ttu-id="ba1dd-161">Asynchronní zobrazení seznamu objektů blob na stránkách</span><span class="sxs-lookup"><span data-stu-id="ba1dd-161">List blobs in pages asynchronously</span></span>
<span data-ttu-id="ba1dd-162">Když provádíte výpis velkého počtu objektů blob nebo chcete mít přehled o počtu výsledků, které vrátíte v rámci jedné operace výpisu, můžete vytvořit seznam objektů blob na stránkách s výsledky.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-162">If you are listing a large number of blobs, or you want to control the number of results you return in one listing operation, you can list blobs in pages of results.</span></span> <span data-ttu-id="ba1dd-163">Tento příklad ukazuje, jak vracet výsledky na stránkách asynchronně tak, aby čekání na vrácení velké sady výsledků neblokovalo provádění.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-163">This example shows how to return results in pages asynchronously, so that execution is not blocked while waiting to return a large set of results.</span></span>

<span data-ttu-id="ba1dd-164">Tento příklad ukazuje plochý výpis objektu blob, můžete ale také provést hierarchický výpis nastavením parametru _useFlatBlobListing_ metody **ListBlobsSegmentedAsync** na _false_.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-164">This example shows a flat blob listing, but you can also perform a hierarchical listing, by setting the _useFlatBlobListing_ parameter of the **ListBlobsSegmentedAsync** method to _false_.</span></span>

<span data-ttu-id="ba1dd-165">Vzhledem k tomu, že metoda ukázky volá asynchronní metodu, musí být uvedena klíčovým slovem _async_ a musí vrátit objekt **Úloha**.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-165">Because the sample method calls an asynchronous method, it must be prefaced with the _async_ keyword, and it must return a **Task** object.</span></span> <span data-ttu-id="ba1dd-166">Klíčové slovo await, určené pro metodu **ListBlobsSegmentedAsync** pozastaví spuštění metody ukázky až do dokončení úlohy vytváření seznamu.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-166">The await keyword specified for the **ListBlobsSegmentedAsync** method suspends execution of the sample method until the listing task completes.</span></span>

```csharp
async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
{
    //List blobs to the console window, with paging.
    Console.WriteLine("List blobs in pages:");

    int i = 0;
    BlobContinuationToken continuationToken = null;
    BlobResultSegment resultSegment = null;

    //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
    //When the continuation token is null, the last page has been returned and execution can exit the loop.
    do
    {
        //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
        //or by calling a different overload.
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
```

## <a name="writing-to-an-append-blob"></a><span data-ttu-id="ba1dd-167">Zápis do doplňovacího objektu blob</span><span class="sxs-lookup"><span data-stu-id="ba1dd-167">Writing to an append blob</span></span>
<span data-ttu-id="ba1dd-168">Doplňovací objekt blob je optimalizován pro operace připojení, například protokolování.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-168">An append blob is optimized for append operations, such as logging.</span></span> <span data-ttu-id="ba1dd-169">Podobně jako objekt blob bloku se doplňovací objekt blob skládá z bloků, ale když chcete do doplňovacího objektu blob připojit nový blok, je připojen vždy na konec objektu blob.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-169">Like a block blob, an append blob is comprised of blocks, but when you add a new block to an append blob, it is always appended to the end of the blob.</span></span> <span data-ttu-id="ba1dd-170">Existující blok v doplňovacím objektu blob se nedá aktualizovat ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-170">You cannot update or delete an existing block in an append blob.</span></span> <span data-ttu-id="ba1dd-171">ID bloku pro doplňovací objekt blob nejsou vystavená, protože jsou určená pro objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-171">The block IDs for an append blob are not exposed as they are for a block blob.</span></span>

<span data-ttu-id="ba1dd-172">Každý blok v doplňovacím objektu blob může mít různou velikost až do 4 MB, každý doplňovací objekt blob může obsahovat maximálně 50 000 bloků.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-172">Each block in an append blob can be a different size, up to a maximum of 4 MB, and an append blob can include a maximum of 50,000 blocks.</span></span> <span data-ttu-id="ba1dd-173">Maximální velikost doplňovacího objektu blob je proto o něco větší než 195 GB (4 MB × 50 000 bloků).</span><span class="sxs-lookup"><span data-stu-id="ba1dd-173">The maximum size of an append blob is therefore slightly more than 195 GB (4 MB X 50,000 blocks).</span></span>

<span data-ttu-id="ba1dd-174">Následující příklad vytvoří nový doplňovací objekt blob a připojí některá data pro simulaci jednoduché operace protokolování.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-174">The example below creates a new append blob and appends some data to it, simulating a simple logging operation.</span></span>

```csharp
//Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

//Create service client for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

//Create the container if it does not already exist.
container.CreateIfNotExists();

//Get a reference to an append blob.
CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

//Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
//You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
appendBlob.CreateOrReplace();

int numBlocks = 10;

//Generate an array of random bytes.
Random rnd = new Random();
byte[] bytes = new byte[numBlocks];
rnd.NextBytes(bytes);

//Simulate a logging operation by writing text data and byte data to the end of the append blob.
for (int i = 0; i < numBlocks; i++)
{
    appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
        DateTime.UtcNow, bytes[i], Environment.NewLine));
}

//Read the append blob to the console window.
Console.WriteLine(appendBlob.DownloadText());
```

<span data-ttu-id="ba1dd-175">Další informace o rozdílech mezi třemi typy objektů blob získáte v části [ Vysvětlení objektů blob bloku, objektů blob stránky a doplňovacích objektů blob](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).</span><span class="sxs-lookup"><span data-stu-id="ba1dd-175">See [Understanding Block Blobs, Page Blobs, and Append Blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) for more information about the differences between the three types of blobs.</span></span>

## <a name="managing-security-for-blobs"></a><span data-ttu-id="ba1dd-176">Správa zabezpečení pro objekty blob</span><span class="sxs-lookup"><span data-stu-id="ba1dd-176">Managing security for blobs</span></span>
<span data-ttu-id="ba1dd-177">Úložiště Azure ve výchozím nastavení zajišťuje ochranu dat omezením přístupu k majiteli účtu, který vlastní klíče pro přístup k účtu.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-177">By default, Azure Storage keeps your data secure by limiting access to the account owner, who is in possession of the account access keys.</span></span> <span data-ttu-id="ba1dd-178">Když budete chtít sdílet data objektu blob na svém účtu úložiště, je důležité zajistit, aby nedošlo k ohrožení zabezpečení vašich klíčů pro přístup k účtu.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-178">When you need to share blob data in your storage account, it is important to do so without compromising the security of your account access keys.</span></span> <span data-ttu-id="ba1dd-179">Navíc můžete šifrovat data objektů blob tak, aby byl zaručen jejich zabezpečený přenos přes síť nebo úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-179">Additionally, you can encrypt blob data to ensure that it is secure going over the wire and in Azure Storage.</span></span>

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a><span data-ttu-id="ba1dd-180">Kontrola přístupu k datům objektu blob</span><span class="sxs-lookup"><span data-stu-id="ba1dd-180">Controlling access to blob data</span></span>
<span data-ttu-id="ba1dd-181">Ve výchozím nastavení jsou data objektu blob ve vašem účtu úložiště dostupná pouze majiteli účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-181">By default, the blob data in your storage account is accessible only to storage account owner.</span></span> <span data-ttu-id="ba1dd-182">Ve výchozím nastavení vyžaduje ověřování požadavků na Blob storage přístupový klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-182">Authenticating requests against Blob storage requires the account access key by default.</span></span> <span data-ttu-id="ba1dd-183">Určitá data objektu blob ale můžete zpřístupnit ostatním uživatelům.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-183">However, you may wish to make certain blob data available to other users.</span></span> <span data-ttu-id="ba1dd-184">Máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="ba1dd-184">You have two options:</span></span>

* <span data-ttu-id="ba1dd-185">**Anonymní přístup:** můžete nastavit kontejner nebo jeho objekty blob na veřejně dostupné pro anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-185">**Anonymous access:** You can make a container or its blobs publicly available for anonymous access.</span></span> <span data-ttu-id="ba1dd-186">Další informace viz [Správa anonymního přístupu pro čtení ke kontejnerům a objektům blob](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="ba1dd-186">See [Manage anonymous read access to containers and blobs](storage-manage-access-to-resources.md) for more information.</span></span>
* <span data-ttu-id="ba1dd-187">**Sdílený přístupový podpis:** Klientům můžete zajistit sdílený přístupový podpis (SAS), který poskytuje delegovaný přístup k prostředku ve vašem účtu úložiště s oprávněními, která jste zadali a v intervalu, který určíte.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-187">**Shared access signatures:** You can provide clients with a shared access signature (SAS), which provides delegated access to a resource in your storage account, with permissions that you specify and over an interval that you specify.</span></span> <span data-ttu-id="ba1dd-188">Další informace najdete v tématu [Použití sdílených přístupových podpisů (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ba1dd-188">See [Using Shared Access Signatures (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) for more information.</span></span>

### <a name="encrypting-blob-data"></a><span data-ttu-id="ba1dd-189">Šifrování dat objektů blob</span><span class="sxs-lookup"><span data-stu-id="ba1dd-189">Encrypting blob data</span></span>
<span data-ttu-id="ba1dd-190">Úložiště Azure podporuje šifrování dat objektů blob na straně klienta i na serveru:</span><span class="sxs-lookup"><span data-stu-id="ba1dd-190">Azure Storage supports encrypting blob data both at the client and on the server:</span></span>

* <span data-ttu-id="ba1dd-191">**Šifrování na straně klienta:** Klientská knihovna pro úložiště pro .NET podporuje šifrování dat v rámci klientské aplikace před nahráním do úložiště Azure a dešifrování dat při stahování do klienta.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-191">**Client-side encryption:** The Storage Client Library for .NET supports encrypting data within client applications before uploading to Azure Storage, and decrypting data while downloading to the client.</span></span> <span data-ttu-id="ba1dd-192">Knihovna také podporuje integraci se službou Azure Key Vault pro správu klíčů účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-192">The library also supports integration with Azure Key Vault for storage account key management.</span></span> <span data-ttu-id="ba1dd-193">Další informace viz [Šifrování na straně klienta s .NET pro úložiště Microsoft Azure](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ba1dd-193">See [Client-Side Encryption with .NET for Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) for more information.</span></span> <span data-ttu-id="ba1dd-194">Viz také [Kurz: Šifrování a dešifrování objektů blob v úložišti Microsoft Azure pomocí služby Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="ba1dd-194">Also see [Tutorial: Encrypt and decrypt blobs in Microsoft Azure Storage using Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md).</span></span>
* <span data-ttu-id="ba1dd-195">**Šifrování na straně serveru**: Úložiště Azure nyní podporuje šifrování na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-195">**Server-side encryption**: Azure Storage now supports server-side encryption.</span></span> <span data-ttu-id="ba1dd-196">Viz [Šifrování služby Azure Storage Service pro Neaktivní uložená data (Náhled)](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ba1dd-196">See [Azure Storage Service Encryption for Data at Rest (Preview)](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba1dd-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ba1dd-197">Next steps</span></span>
<span data-ttu-id="ba1dd-198">Teď, když jste se naučili základy používání Blob storage, podívejte se na následující odkazy a získejte další informace.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-198">Now that you've learned the basics of Blob storage, follow these links to learn more.</span></span>

### <a name="microsoft-azure-storage-explorer"></a><span data-ttu-id="ba1dd-199">Microsoft Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="ba1dd-199">Microsoft Azure Storage Explorer</span></span>
* <span data-ttu-id="ba1dd-200">[Microsoft Azure Storage Explorer (MASE)](../../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.</span><span class="sxs-lookup"><span data-stu-id="ba1dd-200">[Microsoft Azure Storage Explorer (MASE)](../../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

### <a name="blob-storage-samples"></a><span data-ttu-id="ba1dd-201">Ukázky Blob Storage</span><span class="sxs-lookup"><span data-stu-id="ba1dd-201">Blob storage samples</span></span>
* [<span data-ttu-id="ba1dd-202">Začínáme s úložištěm Azure Blob Storage pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="ba1dd-202">Getting Started with Azure Blob Storage in .NET</span></span>](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a><span data-ttu-id="ba1dd-203">Odkazy Blob storage</span><span class="sxs-lookup"><span data-stu-id="ba1dd-203">Blob storage reference</span></span>
* [<span data-ttu-id="ba1dd-204">Klientská knihovna pro úložiště pro .NET – referenční informace</span><span class="sxs-lookup"><span data-stu-id="ba1dd-204">Storage Client Library for .NET reference</span></span>](https://msdn.microsoft.com/library/azure/mt347887.aspx)
* [<span data-ttu-id="ba1dd-205">REST API – referenční informace</span><span class="sxs-lookup"><span data-stu-id="ba1dd-205">REST API reference</span></span>](/rest/api/storageservices/azure-storage-services-rest-api-reference)

### <a name="conceptual-guides"></a><span data-ttu-id="ba1dd-206">Koncepční vodítka</span><span class="sxs-lookup"><span data-stu-id="ba1dd-206">Conceptual guides</span></span>
* [<span data-ttu-id="ba1dd-207">Přenos dat pomocí nástroje příkazového řádku AzCopy</span><span class="sxs-lookup"><span data-stu-id="ba1dd-207">Transfer data with the AzCopy command-line utility</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [<span data-ttu-id="ba1dd-208">Začínáme se službou File Storage for .NET</span><span class="sxs-lookup"><span data-stu-id="ba1dd-208">Get started with File storage for .NET</span></span>](../files/storage-dotnet-how-to-use-files.md)
* [<span data-ttu-id="ba1dd-209">Použití úložiště objektů blob v Azure pomocí WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="ba1dd-209">How to use Azure blob storage with the WebJobs SDK</span></span>](../../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
