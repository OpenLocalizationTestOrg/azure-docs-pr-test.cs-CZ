---
title: "aaaIntroduction tooAzure úložiště objektů Blob | Microsoft Docs"
description: "Úvod tooAzure úložiště objektů Blob"
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
ms.openlocfilehash: 3431f826ae51d42dbced084ee60f9ff70a8168d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooblob-storage"></a><span data-ttu-id="beb1d-103">Úvod tooBlob úložiště</span><span class="sxs-lookup"><span data-stu-id="beb1d-103">Introduction tooBlob storage</span></span>

<span data-ttu-id="beb1d-104">Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, jako je například textu nebo binárních dat, která je přístupná z kdekoli v hello, world pomocí protokolů HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="beb1d-104">Azure Blob storage is a service for storing large amounts of unstructured object data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="beb1d-105">Data tooexpose úložiště objektů Blob můžete použít veřejně toohello svět nebo data aplikací toostore soukromě.</span><span class="sxs-lookup"><span data-stu-id="beb1d-105">You can use Blob storage tooexpose data publicly toohello world, or toostore application data privately.</span></span>

<span data-ttu-id="beb1d-106">Mezi běžná použití služby Blob Storage patří:</span><span class="sxs-lookup"><span data-stu-id="beb1d-106">Common uses of Blob storage include:</span></span>

* <span data-ttu-id="beb1d-107">Slouží obrázků nebo dokumentů přímo tooa prohlížeče</span><span class="sxs-lookup"><span data-stu-id="beb1d-107">Serving images or documents directly tooa browser</span></span>
* <span data-ttu-id="beb1d-108">ukládání souborů pro distribuovaný přístup</span><span class="sxs-lookup"><span data-stu-id="beb1d-108">Storing files for distributed access</span></span>
* <span data-ttu-id="beb1d-109">streamování videa a zvuku</span><span class="sxs-lookup"><span data-stu-id="beb1d-109">Streaming video and audio</span></span>
* <span data-ttu-id="beb1d-110">ukládání dat pro zálohování a obnovování, zotavení po havárii a pro archivaci</span><span class="sxs-lookup"><span data-stu-id="beb1d-110">Storing data for backup and restore, disaster recovery, and archiving</span></span>
* <span data-ttu-id="beb1d-111">ukládání dat, které bude analyzovat místní nebo v Azure hostovaná služba</span><span class="sxs-lookup"><span data-stu-id="beb1d-111">Storing data for analysis by an on-premises or Azure-hosted service</span></span>

## <a name="blob-service-concepts"></a><span data-ttu-id="beb1d-112">Koncepty služby Blob service</span><span class="sxs-lookup"><span data-stu-id="beb1d-112">Blob service concepts</span></span>

<span data-ttu-id="beb1d-113">Hello služby objektů Blob obsahuje hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="beb1d-113">hello Blob service contains hello following components:</span></span>

![Architektura objektu blob](./media/storage-blobs-introduction/blob1.png)

* <span data-ttu-id="beb1d-115">**Účet úložiště:** všechny přístup tooAzure úložiště se provádí prostřednictvím účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="beb1d-115">**Storage Account:** All access tooAzure Storage is done through a storage account.</span></span> <span data-ttu-id="beb1d-116">Tento účet úložiště může být **účet úložiště pro obecné účely** nebo **účtem služby Blob storage** který se specializuje na ukládání objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="beb1d-116">This storage account can be a **General-purpose storage account** or a **Blob storage account** that is specialized for storing objects/blobs.</span></span> <span data-ttu-id="beb1d-117">Další informace najdete v tématu [Účty Azure Storage](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="beb1d-117">See [About Azure storage accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) for more information.</span></span>

* <span data-ttu-id="beb1d-118">**Kontejner:** Kontejner zajišťuje seskupení sady objektů blob.</span><span class="sxs-lookup"><span data-stu-id="beb1d-118">**Container:** A container provides a grouping of a set of blobs.</span></span> <span data-ttu-id="beb1d-119">Všechny objekty blob musí být v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="beb1d-119">All blobs must be in a container.</span></span> <span data-ttu-id="beb1d-120">Účet může obsahovat neomezený počet kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="beb1d-120">An account can contain an unlimited number of containers.</span></span> <span data-ttu-id="beb1d-121">Kontejner můžete pojmout neomezený počet objektů blob.</span><span class="sxs-lookup"><span data-stu-id="beb1d-121">A container can store an unlimited number of blobs.</span></span> <span data-ttu-id="beb1d-122">Všimněte si, že hello název kontejneru musí být malá písmena.</span><span class="sxs-lookup"><span data-stu-id="beb1d-122">Note that hello container name must be lowercase.</span></span>

* <span data-ttu-id="beb1d-123">**Objekt blob:** Soubor libovolného typu a velikosti.</span><span class="sxs-lookup"><span data-stu-id="beb1d-123">**Blob:** A file of any type and size.</span></span> <span data-ttu-id="beb1d-124">Úložiště Azure Storage nabízí tři typy objektů blob – objekty blob bloků, doplňovací objekty blob a objekty blob stránek.</span><span class="sxs-lookup"><span data-stu-id="beb1d-124">Azure Storage offers three types of blobs: block blobs, page blobs, and append blobs.</span></span>
  
    <span data-ttu-id="beb1d-125">*Objekty blob bloků* jsou ideální pro ukládání textových nebo binárních souborů, například dokumentů a multimediálních souborů.</span><span class="sxs-lookup"><span data-stu-id="beb1d-125">*Block blobs* are ideal for storing text or binary files, such as documents and media files.</span></span> <span data-ttu-id="beb1d-126">*Doplňovací objekty BLOB* jsou podobné objektům BLOB tooblock v tom, že je tvoří bloky, ale jsou optimalizované pro doplňovací operace, takže se hodí pro scénáře protokolování.</span><span class="sxs-lookup"><span data-stu-id="beb1d-126">*Append blobs* are similar tooblock blobs in that they are made up of blocks, but they are optimized for append operations, so they are useful for logging scenarios.</span></span> <span data-ttu-id="beb1d-127">Objekt blob jeden blok může obsahovat až too50 000 bloků až too100 MB, každý na celkovou velikost mírně větší, než 4.75 TB (100 MB × 50 000).</span><span class="sxs-lookup"><span data-stu-id="beb1d-127">A single block blob can contain up too50,000 blocks of up too100 MB each, for a total size of slightly more than 4.75 TB (100 MB X 50,000).</span></span> <span data-ttu-id="beb1d-128">Jeden doplňovací objekt blob může obsahovat až too50 000 bloků až too4 MB, každý na celkovou velikost něco větší než 195 GB (4 MB × 50 000).</span><span class="sxs-lookup"><span data-stu-id="beb1d-128">A single append blob can contain up too50,000 blocks of up too4 MB each, for a total size of slightly more than 195 GB (4 MB X 50,000).</span></span>
  
    <span data-ttu-id="beb1d-129">*Objekty BLOB stránek* může být až velikost too1 TB a jsou efektivnější pro časté čtení/zápisu operace.</span><span class="sxs-lookup"><span data-stu-id="beb1d-129">*Page blobs* can be up too1 TB in size, and are more efficient for frequent read/write operations.</span></span> <span data-ttu-id="beb1d-130">Virtuální počítače Azure používá objekty BLOB stránek jako disky s operačním systémem a daty.</span><span class="sxs-lookup"><span data-stu-id="beb1d-130">Azure Virtual Machines uses page blobs as OS and data disks.</span></span>
  
    <span data-ttu-id="beb1d-131">Podrobnosti o vytváření názvů kontejnerů a objektů blob najdete v článku [Názvy kontejnerů, objektů blob a metadat a odkazování na ně](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).</span><span class="sxs-lookup"><span data-stu-id="beb1d-131">For details about naming containers and blobs, see [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).</span></span>

## <a name="next-steps"></a><span data-ttu-id="beb1d-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="beb1d-132">Next steps</span></span>

* [<span data-ttu-id="beb1d-133">Vytvoření účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="beb1d-133">Create a storage account</span></span>](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [<span data-ttu-id="beb1d-134">Začínáme s úložištěm Blob pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="beb1d-134">Getting started with Blob storage using .NET</span></span>](storage-dotnet-how-to-use-blobs.md)