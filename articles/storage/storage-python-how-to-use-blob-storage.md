---
title: "aaaHow toouse Azure Blob storage (úložiště objektů) z Python | Microsoft Docs"
description: "Ukládání nestrukturovaných dat v cloudu hello s Azure Blob storage (úložiště objektů)."
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
ms.openlocfilehash: 6efc61aa89e6d2544b7a18c80ce3546640f90462
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-blob-storage-from-python"></a><span data-ttu-id="1d736-103">Jak toouse Azure Blob storage z Pythonu</span><span class="sxs-lookup"><span data-stu-id="1d736-103">How toouse Azure Blob storage from Python</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="1d736-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1d736-104">Overview</span></span>
<span data-ttu-id="1d736-105">Azure Blob storage je služba, která ukládá Nestrukturovaná data v cloudu hello jako objekty nebo objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="1d736-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="1d736-106">Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d736-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="1d736-107">Úložiště objektů blob je také odkazované tooas objektu úložiště.</span><span class="sxs-lookup"><span data-stu-id="1d736-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="1d736-108">Tento článek vám ukáže, jak tooperform běžné scénáře s využitím úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="1d736-108">This article will show you how tooperform common scenarios using Blob storage.</span></span> <span data-ttu-id="1d736-109">Hello ukázky jsou napsané v Pythonu a používají hello [Microsoft Azure SDK úložiště pro jazyk Python].</span><span class="sxs-lookup"><span data-stu-id="1d736-109">hello samples are written in Python and use hello [Microsoft Azure Storage SDK for Python].</span></span> <span data-ttu-id="1d736-110">pokryté scénáře Hello zahrnují odesílání, výpis, stahování a odstraňování objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="1d736-110">hello scenarios covered include uploading, listing, downloading, and deleting blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a><span data-ttu-id="1d736-111">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="1d736-111">Create a container</span></span>
<span data-ttu-id="1d736-112">V závislosti na typu hello objektu blob chcete toouse, vytvoření **BlockBlobService**, **AppendBlobService**, nebo **PageBlobService** objektu.</span><span class="sxs-lookup"><span data-stu-id="1d736-112">Based on hello type of blob you would like toouse, create a **BlockBlobService**, **AppendBlobService**, or **PageBlobService** object.</span></span> <span data-ttu-id="1d736-113">Hello následující kód používá **BlockBlobService** objektu.</span><span class="sxs-lookup"><span data-stu-id="1d736-113">hello following code uses a **BlockBlobService** object.</span></span> <span data-ttu-id="1d736-114">Přidejte následující hello v horní hello jakéhokoliv Python souboru, ve kterém chcete tooprogrammatically přístupu Azure úložiště objektů Blob bloku.</span><span class="sxs-lookup"><span data-stu-id="1d736-114">Add hello following near hello top of any Python file in which you wish tooprogrammatically access Azure Block Blob Storage.</span></span>

```python
from azure.storage.blob import BlockBlobService
```

<span data-ttu-id="1d736-115">Hello následující kód vytvoří **BlockBlobService** objekt, který používá hello klíč účtu úložiště účet a název.</span><span class="sxs-lookup"><span data-stu-id="1d736-115">hello following code creates a **BlockBlobService** object using hello storage account name and account key.</span></span>  <span data-ttu-id="1d736-116">Nahraďte název účtu a klíč 'stránku Můj účet' a 'mykey.</span><span class="sxs-lookup"><span data-stu-id="1d736-116">Replace 'myaccount' and 'mykey' with your account name and key.</span></span>

```python
block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')
```

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="1d736-117">V hello následující ukázka kódu, můžete použít **BlockBlobService** objekt toocreate hello kontejneru Pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="1d736-117">In hello following code example, you can use a **BlockBlobService** object toocreate hello container if it doesn't exist.</span></span>

```python
block_blob_service.create_container('mycontainer')
```

<span data-ttu-id="1d736-118">Ve výchozím nastavení, je hello nový kontejner privátní, takže musíte zadat přístupový klíč k úložišti (stejně jako dříve) toodownload objekty BLOB z tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="1d736-118">By default, hello new container is private, so you must specify your storage access key (as you did earlier) toodownload blobs from this container.</span></span> <span data-ttu-id="1d736-119">Pokud chcete toomake hello objektů BLOB v dostupné tooeveryone hello kontejneru, můžete vytvořit kontejner hello a předat úroveň hello veřejný přístup pomocí hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="1d736-119">If you want toomake hello blobs within hello container available tooeveryone, you can create hello container and pass hello public access level using hello following code.</span></span>

```python
from azure.storage.blob import PublicAccess
block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)
```

<span data-ttu-id="1d736-120">Alternativně můžete upravit kontejner po vytvoření pomocí hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="1d736-120">Alternatively, you can modify a container after you have created it using hello following code.</span></span>

```python
block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)
```

<span data-ttu-id="1d736-121">Po této změně kdokoli na hello Internetu může vidět objekty BLOB ve veřejném kontejneru, ale pouze můžete upravit nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="1d736-121">After this change, anyone on hello Internet can see blobs in a public container, but only you can modify or delete them.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="1d736-122">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="1d736-122">Upload a blob into a container</span></span>
<span data-ttu-id="1d736-123">toocreate objekt blob bloku a nahrávání dat, použijte hello **vytvořit\_blob\_z\_cesta**, **vytvořit\_blob\_z\_datového proudu**, **vytvořit\_blob\_z\_bajtů** nebo **vytvořit\_blob\_z\_text** metody.</span><span class="sxs-lookup"><span data-stu-id="1d736-123">toocreate a block blob and upload data, use hello **create\_blob\_from\_path**, **create\_blob\_from\_stream**, **create\_blob\_from\_bytes** or **create\_blob\_from\_text** methods.</span></span> <span data-ttu-id="1d736-124">Jsou nejdůležitější metody, které provádějí hello nezbytné rozdělování když hello velikost dat hello překročí 64 MB.</span><span class="sxs-lookup"><span data-stu-id="1d736-124">They are high-level methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="1d736-125">**vytvořit\_objektů blob\_z\_cesta** nahrávání hello obsah souboru z hello zadané cesty, a **vytvořit\_objektů blob\_z\_datového proudu** nahrávání hello obsah z již otevřeného souboru/stream.</span><span class="sxs-lookup"><span data-stu-id="1d736-125">**create\_blob\_from\_path** uploads hello contents of a file from hello specified path, and **create\_blob\_from\_stream** uploads hello contents from an already opened file/stream.</span></span> <span data-ttu-id="1d736-126">**vytvořit\_blob\_z\_bajtů** odešle pole bajtů, a **vytvořit\_blob\_z\_text** odešle hello zadaný textové hodnoty pomocí hello zadat kódování (výchozí nastavení tooUTF-8).</span><span class="sxs-lookup"><span data-stu-id="1d736-126">**create\_blob\_from\_bytes** uploads an array of bytes, and **create\_blob\_from\_text** uploads hello specified text value using hello specified encoding (defaults tooUTF-8).</span></span>

<span data-ttu-id="1d736-127">Hello následujícím příkladu se uloží obsah hello hello **sunset.png** souboru do hello **můj_objekt_blob** objektů blob.</span><span class="sxs-lookup"><span data-stu-id="1d736-127">hello following example uploads hello contents of hello **sunset.png** file into hello **myblob** blob.</span></span>

```python
from azure.storage.blob import ContentSettings
block_blob_service.create_blob_from_path(
    'mycontainer',
    'myblockblob',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png')
            )
```

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="1d736-128">Seznam hello objekty BLOB v kontejneru</span><span class="sxs-lookup"><span data-stu-id="1d736-128">List hello blobs in a container</span></span>
<span data-ttu-id="1d736-129">toolist hello objekty BLOB v kontejneru, použijte hello **seznamu\_objekty BLOB** metoda.</span><span class="sxs-lookup"><span data-stu-id="1d736-129">toolist hello blobs in a container, use hello **list\_blobs** method.</span></span> <span data-ttu-id="1d736-130">Tato metoda vrátí generátor.</span><span class="sxs-lookup"><span data-stu-id="1d736-130">This method returns a generator.</span></span> <span data-ttu-id="1d736-131">Hello následující kód výstupy hello **název** z jednotlivých objektů blob v kontejneru toohello konzole.</span><span class="sxs-lookup"><span data-stu-id="1d736-131">hello following code outputs hello **name** of each blob in a container toohello console.</span></span>

```python
generator = block_blob_service.list_blobs('mycontainer')
for blob in generator:
    print(blob.name)
```

## <a name="download-blobs"></a><span data-ttu-id="1d736-132">Stáhnout objekty blob</span><span class="sxs-lookup"><span data-stu-id="1d736-132">Download blobs</span></span>
<span data-ttu-id="1d736-133">toodownload data z objektu blob, použijte **získat\_blob\_k\_cesta**, **získat\_objektů blob\_k\_datového proudu**, **získat\_blob\_k\_bajtů**, nebo **získat\_blob\_k\_text**.</span><span class="sxs-lookup"><span data-stu-id="1d736-133">toodownload data from a blob, use **get\_blob\_to\_path**, **get\_blob\_to\_stream**, **get\_blob\_to\_bytes**, or **get\_blob\_to\_text**.</span></span> <span data-ttu-id="1d736-134">Jsou nejdůležitější metody, které provádějí hello nezbytné rozdělování když hello velikost dat hello překročí 64 MB.</span><span class="sxs-lookup"><span data-stu-id="1d736-134">They are high-level methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="1d736-135">Hello následující příklad ukazuje použití **získat\_blob\_k\_cesta** toodownload hello obsah hello **můj_objekt_blob** objektů blob a uložte ho toohello **na více systémů sunset.png** souboru.</span><span class="sxs-lookup"><span data-stu-id="1d736-135">hello following example demonstrates using **get\_blob\_to\_path** toodownload hello contents of hello **myblob** blob and store it toohello **out-sunset.png** file.</span></span>

```python
block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')
```

## <a name="delete-a-blob"></a><span data-ttu-id="1d736-136">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="1d736-136">Delete a blob</span></span>
<span data-ttu-id="1d736-137">Nakonec toodelete objekt blob, zavolejte **delete_blob**.</span><span class="sxs-lookup"><span data-stu-id="1d736-137">Finally, toodelete a blob, call **delete_blob**.</span></span>

```python
block_blob_service.delete_blob('mycontainer', 'myblockblob')
```

## <a name="writing-tooan-append-blob"></a><span data-ttu-id="1d736-138">Zápis tooan připojit objektů blob</span><span class="sxs-lookup"><span data-stu-id="1d736-138">Writing tooan append blob</span></span>
<span data-ttu-id="1d736-139">Doplňovací objekt blob je optimalizován pro operace připojení, například protokolování.</span><span class="sxs-lookup"><span data-stu-id="1d736-139">An append blob is optimized for append operations, such as logging.</span></span> <span data-ttu-id="1d736-140">Podobně jako objekt blob bloku doplňovací objekt blob se skládá z bloků, ale když přidáte nový objekt blob bloku připojení tooan, je vždy připojením toohello konec objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="1d736-140">Like a block blob, an append blob is comprised of blocks, but when you add a new block tooan append blob, it is always appended toohello end of hello blob.</span></span> <span data-ttu-id="1d736-141">Existující blok v doplňovacím objektu blob se nedá aktualizovat ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="1d736-141">You cannot update or delete an existing block in an append blob.</span></span> <span data-ttu-id="1d736-142">ID Hello bloku pro doplňovací objekt blob nejsou vystavená, protože jsou pro objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="1d736-142">hello block IDs for an append blob are not exposed as they are for a block blob.</span></span>

<span data-ttu-id="1d736-143">Každý blok v doplňovacím objektu blob může mít různou velikost, až tooa nesmí být delší než 4 MB volného místa, a doplňovací objekt blob může obsahovat maximálně 50 000 bloků.</span><span class="sxs-lookup"><span data-stu-id="1d736-143">Each block in an append blob can be a different size, up tooa maximum of 4 MB, and an append blob can include a maximum of 50,000 blocks.</span></span> <span data-ttu-id="1d736-144">Hello maximální velikost doplňovacího objektu BLOB je proto něco větší než 195 GB (4 MB × 50 000 bloků).</span><span class="sxs-lookup"><span data-stu-id="1d736-144">hello maximum size of an append blob is therefore slightly more than 195 GB (4 MB X 50,000 blocks).</span></span>

<span data-ttu-id="1d736-145">Následující příklad Hello vytvoří nový doplňovací objekt blob a připojí některá data tooit simulaci jednoduché operace protokolování.</span><span class="sxs-lookup"><span data-stu-id="1d736-145">hello example below creates a new append blob and appends some data tooit, simulating a simple logging operation.</span></span>

```python
from azure.storage.blob import AppendBlobService
append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

# hello same containers can hold all types of blobs
append_blob_service.create_container('mycontainer')

# Append blobs must be created before they are appended to
append_blob_service.create_blob('mycontainer', 'myappendblob')
append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')
```

## <a name="next-steps"></a><span data-ttu-id="1d736-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d736-146">Next steps</span></span>
<span data-ttu-id="1d736-147">Teď, když jste se naučili základy používání Blob storage hello, postupujte podle těchto odkazů toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="1d736-147">Now that you've learned hello basics of Blob storage, follow these links toolearn more.</span></span>

* [<span data-ttu-id="1d736-148">Středisko pro vývojáře programující v Pythonu</span><span class="sxs-lookup"><span data-stu-id="1d736-148">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/)
* [<span data-ttu-id="1d736-149">REST API služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="1d736-149">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="1d736-150">[Blog týmu Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="1d736-150">[Azure Storage Team Blog]</span></span>
* <span data-ttu-id="1d736-151">[Microsoft Azure SDK úložiště pro jazyk Python]</span><span class="sxs-lookup"><span data-stu-id="1d736-151">[Microsoft Azure Storage SDK for Python]</span></span>

[Blog týmu Azure Storage]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure SDK úložiště pro jazyk Python]: https://github.com/Azure/azure-storage-python
