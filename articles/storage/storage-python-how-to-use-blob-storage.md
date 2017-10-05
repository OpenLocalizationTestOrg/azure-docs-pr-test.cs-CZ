---
title: "Jak používat Azure Blob storage (úložiště objektů) z Python | Microsoft Docs"
description: "Ukládejte nestrukturovaná data v cloudu pomocí Azure Blob Storage (úložiště objektů)."
services: storage
documentationcenter: python
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 0348e360-b24d-41fa-bb12-b8f18990d8bc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 2/24/2017
ms.author: marsma
ms.openlocfilehash: 968814db9496fd410162d482191592c8a56101f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-blob-storage-from-python"></a><span data-ttu-id="13721-103">Jak používat Azure Blob storage z Pythonu</span><span class="sxs-lookup"><span data-stu-id="13721-103">How to use Azure Blob storage from Python</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="13721-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="13721-104">Overview</span></span>
<span data-ttu-id="13721-105">Úložiště objektů blob v Azure je služba, která ukládá nestrukturovaná data v cloudu jako objekty nebo objekty blob.</span><span class="sxs-lookup"><span data-stu-id="13721-105">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="13721-106">Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="13721-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="13721-107">Blob storage se také nazývá úložiště objektů.</span><span class="sxs-lookup"><span data-stu-id="13721-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="13721-108">Tento článek vám ukáže, jak provádět běžné scénáře s využitím úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="13721-108">This article will show you how to perform common scenarios using Blob storage.</span></span> <span data-ttu-id="13721-109">Ukázky jsou napsané v Pythonu a použití [Microsoft Azure SDK úložiště pro jazyk Python].</span><span class="sxs-lookup"><span data-stu-id="13721-109">The samples are written in Python and use the [Microsoft Azure Storage SDK for Python].</span></span> <span data-ttu-id="13721-110">Pokryté scénáře zahrnují odesílání, výpis, stahování a odstraňování objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="13721-110">The scenarios covered include uploading, listing, downloading, and deleting blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a><span data-ttu-id="13721-111">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="13721-111">Create a container</span></span>
<span data-ttu-id="13721-112">Na základě typu objektu blob, které chcete použít, vytvořte **BlockBlobService**, **AppendBlobService**, nebo **PageBlobService** objektu.</span><span class="sxs-lookup"><span data-stu-id="13721-112">Based on the type of blob you would like to use, create a **BlockBlobService**, **AppendBlobService**, or **PageBlobService** object.</span></span> <span data-ttu-id="13721-113">Následující kód používá **BlockBlobService** objektu.</span><span class="sxs-lookup"><span data-stu-id="13721-113">The following code uses a **BlockBlobService** object.</span></span> <span data-ttu-id="13721-114">Přidejte následující v horní části všech soubor Python, ve kterém chcete prostřednictvím kódu programu přístup k úložišti objektů Blob bloku Azure.</span><span class="sxs-lookup"><span data-stu-id="13721-114">Add the following near the top of any Python file in which you wish to programmatically access Azure Block Blob Storage.</span></span>

```python
from azure.storage.blob import BlockBlobService
```

<span data-ttu-id="13721-115">Následující kód vytvoří **BlockBlobService** pomocí klíč účet a název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="13721-115">The following code creates a **BlockBlobService** object using the storage account name and account key.</span></span>  <span data-ttu-id="13721-116">Nahraďte název účtu a klíč 'stránku Můj účet' a 'mykey.</span><span class="sxs-lookup"><span data-stu-id="13721-116">Replace 'myaccount' and 'mykey' with your account name and key.</span></span>

```python
block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')
```

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="13721-117">V následujícím příkladu kódu, můžete použít **BlockBlobService** objekt, který chcete vytvořit kontejner, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="13721-117">In the following code example, you can use a **BlockBlobService** object to create the container if it doesn't exist.</span></span>

```python
block_blob_service.create_container('mycontainer')
```

<span data-ttu-id="13721-118">Ve výchozím nastavení je nový kontejner privátní, proto přístupový klíč k úložišti musí určit (stejně jako dříve) ke stažení z tohoto kontejneru objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="13721-118">By default, the new container is private, so you must specify your storage access key (as you did earlier) to download blobs from this container.</span></span> <span data-ttu-id="13721-119">Pokud chcete zpřístupnit objektů BLOB v kontejneru pro všechny uživatele, můžete vytvořit kontejner a předat úroveň veřejný přístup pomocí následujícího kódu.</span><span class="sxs-lookup"><span data-stu-id="13721-119">If you want to make the blobs within the container available to everyone, you can create the container and pass the public access level using the following code.</span></span>

```python
from azure.storage.blob import PublicAccess
block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)
```

<span data-ttu-id="13721-120">Alternativně můžete upravit kontejner po vytvoření ho pomocí následujícího kódu.</span><span class="sxs-lookup"><span data-stu-id="13721-120">Alternatively, you can modify a container after you have created it using the following code.</span></span>

```python
block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)
```

<span data-ttu-id="13721-121">Po této změně kdokoli na Internetu může vidět objekty BLOB ve veřejném kontejneru, ale pouze můžete upravit nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="13721-121">After this change, anyone on the Internet can see blobs in a public container, but only you can modify or delete them.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="13721-122">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="13721-122">Upload a blob into a container</span></span>
<span data-ttu-id="13721-123">Chcete-li vytvořit objekt blob bloku a odesílat data, použijte **vytvořit\_objektů blob\_z\_cesta**, **vytvořit\_objektů blob\_z\_datového proudu**, **vytvořit\_blob\_z\_bajtů** nebo **vytvořit\_blob\_z\_text** metody.</span><span class="sxs-lookup"><span data-stu-id="13721-123">To create a block blob and upload data, use the **create\_blob\_from\_path**, **create\_blob\_from\_stream**, **create\_blob\_from\_bytes** or **create\_blob\_from\_text** methods.</span></span> <span data-ttu-id="13721-124">Jsou nejdůležitější metody, které provádějí potřebné rozdělování, když velikost dat přesáhne 64 MB.</span><span class="sxs-lookup"><span data-stu-id="13721-124">They are high-level methods that perform the necessary chunking when the size of the data exceeds 64 MB.</span></span>

<span data-ttu-id="13721-125">**vytvořit\_objektů blob\_z\_cesta** odešle obsah souboru ze zadané cesty a **vytvořit\_blob\_z\_datového proudu**odešle obsah z již otevřeného souboru/stream.</span><span class="sxs-lookup"><span data-stu-id="13721-125">**create\_blob\_from\_path** uploads the contents of a file from the specified path, and **create\_blob\_from\_stream** uploads the contents from an already opened file/stream.</span></span> <span data-ttu-id="13721-126">**vytvořit\_blob\_z\_bajtů** odešle pole bajtů, a **vytvořit\_blob\_z\_text** odešle zadaný textové hodnoty pomocí zadaného kódování (výchozí hodnota je UTF-8).</span><span class="sxs-lookup"><span data-stu-id="13721-126">**create\_blob\_from\_bytes** uploads an array of bytes, and **create\_blob\_from\_text** uploads the specified text value using the specified encoding (defaults to UTF-8).</span></span>

<span data-ttu-id="13721-127">V následujícím příkladu se uloží obsah **sunset.png** soubor do **můj_objekt_blob** objektů blob.</span><span class="sxs-lookup"><span data-stu-id="13721-127">The following example uploads the contents of the **sunset.png** file into the **myblob** blob.</span></span>

```python
from azure.storage.blob import ContentSettings
block_blob_service.create_blob_from_path(
    'mycontainer',
    'myblockblob',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png')
            )
```

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="13721-128">Zobrazí seznam objektů blob v kontejneru</span><span class="sxs-lookup"><span data-stu-id="13721-128">List the blobs in a container</span></span>
<span data-ttu-id="13721-129">K zobrazení seznamu objektů BLOB v kontejneru, použijte **seznamu\_objekty BLOB** metoda.</span><span class="sxs-lookup"><span data-stu-id="13721-129">To list the blobs in a container, use the **list\_blobs** method.</span></span> <span data-ttu-id="13721-130">Tato metoda vrátí generátor.</span><span class="sxs-lookup"><span data-stu-id="13721-130">This method returns a generator.</span></span> <span data-ttu-id="13721-131">Následující kód výstupy **název** z jednotlivých objektů blob v kontejneru ke konzole.</span><span class="sxs-lookup"><span data-stu-id="13721-131">The following code outputs the **name** of each blob in a container to the console.</span></span>

```python
generator = block_blob_service.list_blobs('mycontainer')
for blob in generator:
    print(blob.name)
```

## <a name="download-blobs"></a><span data-ttu-id="13721-132">Stáhnout objekty blob</span><span class="sxs-lookup"><span data-stu-id="13721-132">Download blobs</span></span>
<span data-ttu-id="13721-133">Ke stahování dat z objektu blob, použijte **získat\_blob\_k\_cesta**, **získat\_blob\_k\_datového proudu**, **získat\_blob\_k\_bajtů**, nebo **získat\_blob\_k\_text**.</span><span class="sxs-lookup"><span data-stu-id="13721-133">To download data from a blob, use **get\_blob\_to\_path**, **get\_blob\_to\_stream**, **get\_blob\_to\_bytes**, or **get\_blob\_to\_text**.</span></span> <span data-ttu-id="13721-134">Jsou nejdůležitější metody, které provádějí potřebné rozdělování, když velikost dat přesáhne 64 MB.</span><span class="sxs-lookup"><span data-stu-id="13721-134">They are high-level methods that perform the necessary chunking when the size of the data exceeds 64 MB.</span></span>

<span data-ttu-id="13721-135">Následující příklad ukazuje, jak pomocí **získat\_blob\_k\_cesta** stáhnout obsah **můj_objekt_blob** objektů blob a uložte ho do  **na více systémů sunset.png** souboru.</span><span class="sxs-lookup"><span data-stu-id="13721-135">The following example demonstrates using **get\_blob\_to\_path** to download the contents of the **myblob** blob and store it to the **out-sunset.png** file.</span></span>

```python
block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')
```

## <a name="delete-a-blob"></a><span data-ttu-id="13721-136">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="13721-136">Delete a blob</span></span>
<span data-ttu-id="13721-137">Nakonec se odstranit objekt blob, zavolejte **delete_blob**.</span><span class="sxs-lookup"><span data-stu-id="13721-137">Finally, to delete a blob, call **delete_blob**.</span></span>

```python
block_blob_service.delete_blob('mycontainer', 'myblockblob')
```

## <a name="writing-to-an-append-blob"></a><span data-ttu-id="13721-138">Zápis do doplňovacího objektu blob</span><span class="sxs-lookup"><span data-stu-id="13721-138">Writing to an append blob</span></span>
<span data-ttu-id="13721-139">Doplňovací objekt blob je optimalizován pro operace připojení, například protokolování.</span><span class="sxs-lookup"><span data-stu-id="13721-139">An append blob is optimized for append operations, such as logging.</span></span> <span data-ttu-id="13721-140">Podobně jako objekt blob bloku se doplňovací objekt blob skládá z bloků, ale když chcete do doplňovacího objektu blob připojit nový blok, je připojen vždy na konec objektu blob.</span><span class="sxs-lookup"><span data-stu-id="13721-140">Like a block blob, an append blob is comprised of blocks, but when you add a new block to an append blob, it is always appended to the end of the blob.</span></span> <span data-ttu-id="13721-141">Existující blok v doplňovacím objektu blob se nedá aktualizovat ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="13721-141">You cannot update or delete an existing block in an append blob.</span></span> <span data-ttu-id="13721-142">ID bloku pro doplňovací objekt blob nejsou vystavená, protože jsou určená pro objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="13721-142">The block IDs for an append blob are not exposed as they are for a block blob.</span></span>

<span data-ttu-id="13721-143">Každý blok v doplňovacím objektu blob může mít různou velikost až do 4 MB, každý doplňovací objekt blob může obsahovat maximálně 50 000 bloků.</span><span class="sxs-lookup"><span data-stu-id="13721-143">Each block in an append blob can be a different size, up to a maximum of 4 MB, and an append blob can include a maximum of 50,000 blocks.</span></span> <span data-ttu-id="13721-144">Maximální velikost doplňovacího objektu blob je proto o něco větší než 195 GB (4 MB × 50 000 bloků).</span><span class="sxs-lookup"><span data-stu-id="13721-144">The maximum size of an append blob is therefore slightly more than 195 GB (4 MB X 50,000 blocks).</span></span>

<span data-ttu-id="13721-145">Následující příklad vytvoří nový doplňovací objekt blob a připojí některá data pro simulaci jednoduché operace protokolování.</span><span class="sxs-lookup"><span data-stu-id="13721-145">The example below creates a new append blob and appends some data to it, simulating a simple logging operation.</span></span>

```python
from azure.storage.blob import AppendBlobService
append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

# The same containers can hold all types of blobs
append_blob_service.create_container('mycontainer')

# Append blobs must be created before they are appended to
append_blob_service.create_blob('mycontainer', 'myappendblob')
append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')
```

## <a name="next-steps"></a><span data-ttu-id="13721-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="13721-146">Next steps</span></span>
<span data-ttu-id="13721-147">Teď, když jste se naučili základy používání Blob storage, podívejte se na následující odkazy a získejte další informace.</span><span class="sxs-lookup"><span data-stu-id="13721-147">Now that you've learned the basics of Blob storage, follow these links to learn more.</span></span>

* [<span data-ttu-id="13721-148">Středisko pro vývojáře programující v Pythonu</span><span class="sxs-lookup"><span data-stu-id="13721-148">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/)
* [<span data-ttu-id="13721-149">REST API služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="13721-149">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="13721-150">[Blog týmu Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="13721-150">[Azure Storage Team Blog]</span></span>
* <span data-ttu-id="13721-151">[Microsoft Azure SDK úložiště pro jazyk Python]</span><span class="sxs-lookup"><span data-stu-id="13721-151">[Microsoft Azure Storage SDK for Python]</span></span>

<span data-ttu-id="13721-152">[Blog týmu Azure Storage]: http://blogs.msdn.com/b/windowsazurestorage/</span><span class="sxs-lookup"><span data-stu-id="13721-152">[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/</span></span>
<span data-ttu-id="13721-153">[Microsoft Azure SDK úložiště pro jazyk Python]: https://github.com/Azure/azure-storage-python</span><span class="sxs-lookup"><span data-stu-id="13721-153">[Microsoft Azure Storage SDK for Python]: https://github.com/Azure/azure-storage-python</span></span>
