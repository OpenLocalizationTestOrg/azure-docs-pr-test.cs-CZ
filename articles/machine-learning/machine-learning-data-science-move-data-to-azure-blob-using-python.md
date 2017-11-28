---
title: "aaaMove tooand dat z Azure Blob Storage pomocí Python | Microsoft Docs"
description: "Přesun dat tooand z Azure Blob Storage používá Python"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 24276252-b3dd-4edf-9e5d-f6803f8ccccc
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: c2be9600e0d6cb05bcf4109a8d554db522704ecb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-blob-storage-using-python"></a><span data-ttu-id="0c169-103">Přesun dat tooand z Azure Blob Storage používá Python</span><span class="sxs-lookup"><span data-stu-id="0c169-103">Move data tooand from Azure Blob Storage using Python</span></span>
<span data-ttu-id="0c169-104">Toto téma popisuje, jak toolist, odesílání a stáhnout objekty BLOB pomocí hello rozhraní API jazyka Python.</span><span class="sxs-lookup"><span data-stu-id="0c169-104">This topic describes how toolist, upload, and download blobs using hello Python API.</span></span> <span data-ttu-id="0c169-105">Hello součástí sady Azure SDK API Python můžete:</span><span class="sxs-lookup"><span data-stu-id="0c169-105">With hello Python API provided in Azure SDK, you can:</span></span>

* <span data-ttu-id="0c169-106">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="0c169-106">Create a container</span></span>
* <span data-ttu-id="0c169-107">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="0c169-107">Upload a blob into a container</span></span>
* <span data-ttu-id="0c169-108">Stáhnout objekty blob</span><span class="sxs-lookup"><span data-stu-id="0c169-108">Download blobs</span></span>
* <span data-ttu-id="0c169-109">Seznam hello objekty BLOB v kontejneru</span><span class="sxs-lookup"><span data-stu-id="0c169-109">List hello blobs in a container</span></span>
* <span data-ttu-id="0c169-110">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="0c169-110">Delete a blob</span></span>

<span data-ttu-id="0c169-111">Další informace o používání hello Python API najdete v tématu [jak tooUse hello služby úložiště objektů Blob z Pythonu](../storage/blobs/storage-python-how-to-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="0c169-111">For more information about using hello Python API, see [How tooUse hello Blob Storage Service from Python](../storage/blobs/storage-python-how-to-use-blob-storage.md).</span></span>

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> <span data-ttu-id="0c169-112">Pokud používáte virtuální počítač, který byl nastaven pomocí skriptů hello poskytované [datové vědy virtuálních počítačů v Azure](machine-learning-data-science-virtual-machines.md), pak AzCopy je již nainstalován na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0c169-112">If you are using VM that was set up with hello scripts provided by [Data Science Virtual machines in Azure](machine-learning-data-science-virtual-machines.md), then AzCopy is already installed on hello VM.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="0c169-113">Úložiště objektů blob tooAzure dokončení úvod, najdete v části příliš[základy objektu Blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md) a příliš[služby objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="0c169-113">For a complete introduction tooAzure blob storage, refer too[Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and too[Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="0c169-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0c169-114">Prerequisites</span></span>
<span data-ttu-id="0c169-115">Tento dokument předpokládá, že máte předplatné Azure, účet úložiště a hello odpovídající klíč úložiště pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="0c169-115">This document assumes that you have an Azure subscription, a storage account, and hello corresponding storage key for that account.</span></span> <span data-ttu-id="0c169-116">Před nahrávání nebo stahování dat, musíte znát klíč účet a název účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="0c169-116">Before uploading/downloading data, you must know your Azure storage account name and account key.</span></span>

* <span data-ttu-id="0c169-117">tooset si předplatné Azure, najdete v části [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0c169-117">tooset up an Azure subscription, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="0c169-118">Pokyny pro vytvoření účtu úložiště a získávání účet a klíčové informace naleznete v tématu [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="0c169-118">For instructions on creating a storage account and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

## <a name="upload-data-tooblob"></a><span data-ttu-id="0c169-119">Nahrání dat tooBlob</span><span class="sxs-lookup"><span data-stu-id="0c169-119">Upload Data tooBlob</span></span>
<span data-ttu-id="0c169-120">Přidejte následující fragment kódu v hello horní části kód Python, ve kterém chcete tooprogrammatically přístupu Azure Storage hello:</span><span class="sxs-lookup"><span data-stu-id="0c169-120">Add hello following snippet near hello top of any Python code in which you wish tooprogrammatically access Azure Storage:</span></span>

    from azure.storage.blob import BlobService

<span data-ttu-id="0c169-121">Hello **BlobService** objekt vám umožňuje spolupracovat s kontejnery a objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="0c169-121">hello **BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="0c169-122">Hello následující kód vytvoří objekt BlobService pomocí hello klíč účtu úložiště účet a název.</span><span class="sxs-lookup"><span data-stu-id="0c169-122">hello following code creates a BlobService object using hello storage account name and account key.</span></span> <span data-ttu-id="0c169-123">Nahraďte název účtu a klíč účtu skutečné účtu a klíč.</span><span class="sxs-lookup"><span data-stu-id="0c169-123">Replace account name and account key with your real account and key.</span></span>

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

<span data-ttu-id="0c169-124">Použijte následující metody tooupload data tooa blob hello:</span><span class="sxs-lookup"><span data-stu-id="0c169-124">Use hello following methods tooupload data tooa blob:</span></span>

1. <span data-ttu-id="0c169-125">uveďte\_bloku\_blob\_z\_cesta (odešle hello obsah souboru z cesty zadané hello)</span><span class="sxs-lookup"><span data-stu-id="0c169-125">put\_block\_blob\_from\_path (uploads hello contents of a file from hello specified path)</span></span>
2. <span data-ttu-id="0c169-126">uveďte\_block_blob\_z\_souboru (odešle hello obsah z již otevřeného souboru/proud)</span><span class="sxs-lookup"><span data-stu-id="0c169-126">put\_block_blob\_from\_file (uploads hello contents from an already opened file/stream)</span></span>
3. <span data-ttu-id="0c169-127">uveďte\_bloku\_blob\_z\_bajtů (nahrávání na pole bajtů)</span><span class="sxs-lookup"><span data-stu-id="0c169-127">put\_block\_blob\_from\_bytes (uploads an array of bytes)</span></span>
4. <span data-ttu-id="0c169-128">PUT\_bloku\_blob\_z\_text (odešle hello zadané textové hodnoty pomocí hello zadána, kódování)</span><span class="sxs-lookup"><span data-stu-id="0c169-128">put\_block\_blob\_from\_text (uploads hello specified text value using hello specified encoding)</span></span>

<span data-ttu-id="0c169-129">Následující ukázkový kód Hello odešle kontejner tooa místního souboru:</span><span class="sxs-lookup"><span data-stu-id="0c169-129">hello following sample code uploads a local file tooa container:</span></span>

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

<span data-ttu-id="0c169-130">Hello následující vzorový kód odesílá všechny soubory hello (s výjimkou adresáře) v místní adresář tooblob úložiště:</span><span class="sxs-lookup"><span data-stu-id="0c169-130">hello following sample code uploads all hello files (excluding directories) in a local directory tooblob storage:</span></span>

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in hello LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading hello data %s"%blob_name


## <a name="download-data-from-blob"></a><span data-ttu-id="0c169-131">Stahování dat z objektu Blob</span><span class="sxs-lookup"><span data-stu-id="0c169-131">Download Data from Blob</span></span>
<span data-ttu-id="0c169-132">Pomocí následujících metod toodownload dat z objektu blob hello:</span><span class="sxs-lookup"><span data-stu-id="0c169-132">Use hello following methods toodownload data from a blob:</span></span>

1. <span data-ttu-id="0c169-133">získat\_blob\_k\_cesta</span><span class="sxs-lookup"><span data-stu-id="0c169-133">get\_blob\_to\_path</span></span>
2. <span data-ttu-id="0c169-134">získat\_blob\_k\_souboru</span><span class="sxs-lookup"><span data-stu-id="0c169-134">get\_blob\_to\_file</span></span>
3. <span data-ttu-id="0c169-135">získat\_blob\_k\_bajtů</span><span class="sxs-lookup"><span data-stu-id="0c169-135">get\_blob\_to\_bytes</span></span>
4. <span data-ttu-id="0c169-136">získat\_blob\_k\_textu</span><span class="sxs-lookup"><span data-stu-id="0c169-136">get\_blob\_to\_text</span></span>

<span data-ttu-id="0c169-137">Tyto metody, které provádějí hello nezbytné rozdělování když hello velikost dat hello překročí 64 MB.</span><span class="sxs-lookup"><span data-stu-id="0c169-137">These methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="0c169-138">Hello následující vzorový kód stáhne hello obsah objektu blob v kontejneru místního souboru tooa:</span><span class="sxs-lookup"><span data-stu-id="0c169-138">hello following sample code downloads hello contents of a blob in a container tooa local file:</span></span>

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

<span data-ttu-id="0c169-139">Hello následující vzorový kód stáhne všechny objekty BLOB kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0c169-139">hello following sample code downloads all blobs from a container.</span></span> <span data-ttu-id="0c169-140">Použije seznam\_objekty BLOB tooget hello seznam dostupných objektů BLOB v kontejneru hello a stáhne je tooa místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="0c169-140">It uses list\_blobs tooget hello list of available blobs in hello container and downloads them tooa local directory.</span></span>

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading hello data %s"%blob.name
