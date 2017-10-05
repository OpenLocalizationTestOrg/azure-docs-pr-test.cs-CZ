---
title: "Úvod do Azure Blob storage | Microsoft Docs"
description: "Úvod do Azure Blob storage"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: robinsh
ms.openlocfilehash: 051f1b37eab254d4ab4f806166ac8d0b8cab944d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="introduction-to-blob-storage"></a><span data-ttu-id="978e1-103">Úvod do úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="978e1-103">Introduction to Blob storage</span></span>

<span data-ttu-id="978e1-104">Azure Blob Storage je služba, která slouží k ukládání velkých objemů nestrukturovaných dat objektů, například textu nebo binárních dat, která jsou přístupná odkudkoli na světě prostřednictvím protokolů HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="978e1-104">Azure Blob storage is a service for storing large amounts of unstructured object data, such as text or binary data, that can be accessed from anywhere in the world via HTTP or HTTPS.</span></span> <span data-ttu-id="978e1-105">Službu Blob Storage můžete používat ke zveřejňování dat pro celý svět, nebo k soukromému ukládání dat aplikací.</span><span class="sxs-lookup"><span data-stu-id="978e1-105">You can use Blob storage to expose data publicly to the world, or to store application data privately.</span></span>

<span data-ttu-id="978e1-106">Mezi běžná použití služby Blob Storage patří:</span><span class="sxs-lookup"><span data-stu-id="978e1-106">Common uses of Blob storage include:</span></span>

* <span data-ttu-id="978e1-107">poskytování obrázků nebo dokumentů přímo do prohlížeče</span><span class="sxs-lookup"><span data-stu-id="978e1-107">Serving images or documents directly to a browser</span></span>
* <span data-ttu-id="978e1-108">ukládání souborů pro distribuovaný přístup</span><span class="sxs-lookup"><span data-stu-id="978e1-108">Storing files for distributed access</span></span>
* <span data-ttu-id="978e1-109">streamování videa a zvuku</span><span class="sxs-lookup"><span data-stu-id="978e1-109">Streaming video and audio</span></span>
* <span data-ttu-id="978e1-110">ukládání dat pro zálohování a obnovování, zotavení po havárii a pro archivaci</span><span class="sxs-lookup"><span data-stu-id="978e1-110">Storing data for backup and restore, disaster recovery, and archiving</span></span>
* <span data-ttu-id="978e1-111">ukládání dat, které bude analyzovat místní nebo v Azure hostovaná služba</span><span class="sxs-lookup"><span data-stu-id="978e1-111">Storing data for analysis by an on-premises or Azure-hosted service</span></span>

## <a name="blob-service-concepts"></a><span data-ttu-id="978e1-112">Koncepty služby Blob service</span><span class="sxs-lookup"><span data-stu-id="978e1-112">Blob service concepts</span></span>

<span data-ttu-id="978e1-113">Služba Blob service obsahuje následující součásti:</span><span class="sxs-lookup"><span data-stu-id="978e1-113">The Blob service contains the following components:</span></span>

![Architektura objektu blob](./media/storage-blobs-introduction/blob1.png)

* <span data-ttu-id="978e1-115">**Účet úložiště:** Veškerý přístup k úložišti Azure se provádí prostřednictvím účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="978e1-115">**Storage Account:** All access to Azure Storage is done through a storage account.</span></span> <span data-ttu-id="978e1-116">Tento účet úložiště může být **účet úložiště pro obecné účely** nebo **účtem služby Blob storage** který se specializuje na ukládání objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="978e1-116">This storage account can be a **General-purpose storage account** or a **Blob storage account** that is specialized for storing objects/blobs.</span></span> <span data-ttu-id="978e1-117">Další informace najdete v tématu [Účty Azure Storage](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="978e1-117">See [About Azure storage accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) for more information.</span></span>

* <span data-ttu-id="978e1-118">**Kontejner:** Kontejner zajišťuje seskupení sady objektů blob.</span><span class="sxs-lookup"><span data-stu-id="978e1-118">**Container:** A container provides a grouping of a set of blobs.</span></span> <span data-ttu-id="978e1-119">Všechny objekty blob musí být v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="978e1-119">All blobs must be in a container.</span></span> <span data-ttu-id="978e1-120">Účet může obsahovat neomezený počet kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="978e1-120">An account can contain an unlimited number of containers.</span></span> <span data-ttu-id="978e1-121">Kontejner můžete pojmout neomezený počet objektů blob.</span><span class="sxs-lookup"><span data-stu-id="978e1-121">A container can store an unlimited number of blobs.</span></span> <span data-ttu-id="978e1-122">Všimněte si, že název kontejneru musí být psaný malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="978e1-122">Note that the container name must be lowercase.</span></span>

* <span data-ttu-id="978e1-123">**Objekt blob:** Soubor libovolného typu a velikosti.</span><span class="sxs-lookup"><span data-stu-id="978e1-123">**Blob:** A file of any type and size.</span></span> <span data-ttu-id="978e1-124">Úložiště Azure Storage nabízí tři typy objektů blob – objekty blob bloků, doplňovací objekty blob a objekty blob stránek.</span><span class="sxs-lookup"><span data-stu-id="978e1-124">Azure Storage offers three types of blobs: block blobs, page blobs, and append blobs.</span></span>
  
    <span data-ttu-id="978e1-125">*Objekty blob bloků* jsou ideální pro ukládání textových nebo binárních souborů, například dokumentů a multimediálních souborů.</span><span class="sxs-lookup"><span data-stu-id="978e1-125">*Block blobs* are ideal for storing text or binary files, such as documents and media files.</span></span> <span data-ttu-id="978e1-126">*Doplňovací objekty blob* jsou podobné objektům blob bloku v tom, že je tvoří bloky, ale jsou optimalizované pro doplňovací operace, takže se hodí pro scénáře protokolování.</span><span class="sxs-lookup"><span data-stu-id="978e1-126">*Append blobs* are similar to block blobs in that they are made up of blocks, but they are optimized for append operations, so they are useful for logging scenarios.</span></span> <span data-ttu-id="978e1-127">Jeden objekt blob bloku může obsahovat až 50 000 bloků, každý o velikosti až 100 MB, v celkové velikosti o něco větší než 4,75 TB (100 MB × 50 000).</span><span class="sxs-lookup"><span data-stu-id="978e1-127">A single block blob can contain up to 50,000 blocks of up to 100 MB each, for a total size of slightly more than 4.75 TB (100 MB X 50,000).</span></span> <span data-ttu-id="978e1-128">Jeden doplňovací objekt blob může obsahovat až 50 000 bloků, každý o velikosti až 4 MB, v celkové velikosti o něco větší než 195 GB (4 MB × 50 000).</span><span class="sxs-lookup"><span data-stu-id="978e1-128">A single append blob can contain up to 50,000 blocks of up to 4 MB each, for a total size of slightly more than 195 GB (4 MB X 50,000).</span></span>
  
    <span data-ttu-id="978e1-129">*Objekty blob stránek* můžou mít velikost až 1 TB a jsou efektivnější pro časté operace čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="978e1-129">*Page blobs* can be up to 1 TB in size, and are more efficient for frequent read/write operations.</span></span> <span data-ttu-id="978e1-130">Virtuální počítače Azure používá objekty BLOB stránek jako disky s operačním systémem a daty.</span><span class="sxs-lookup"><span data-stu-id="978e1-130">Azure Virtual Machines uses page blobs as OS and data disks.</span></span>
  
    <span data-ttu-id="978e1-131">Podrobnosti o vytváření názvů kontejnerů a objektů blob najdete v článku [Názvy kontejnerů, objektů blob a metadat a odkazování na ně](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).</span><span class="sxs-lookup"><span data-stu-id="978e1-131">For details about naming containers and blobs, see [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).</span></span>

## <a name="next-steps"></a><span data-ttu-id="978e1-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="978e1-132">Next steps</span></span>

* [<span data-ttu-id="978e1-133">Vytvoření účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="978e1-133">Create a storage account</span></span>](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [<span data-ttu-id="978e1-134">Začínáme s úložištěm Blob pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="978e1-134">Getting started with Blob storage using .NET</span></span>](storage-dotnet-how-to-use-blobs.md)