---
title: "Přesun dat do a z Azure Blob Storage pomocí AzCopy | Microsoft Docs"
description: "Přesun dat z a do služby Azure Blob Storage pomocí AzCopy"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: c309ceb2-0e83-4a07-b16d-c997dcd62d5c
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: a41ccdd5739a5b10cef201910abd639ae3126c02
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a><span data-ttu-id="e3535-103">Přesun dat do a z Azure Blob Storage pomocí AzCopy</span><span class="sxs-lookup"><span data-stu-id="e3535-103">Move data to and from Azure Blob Storage using AzCopy</span></span>
<span data-ttu-id="e3535-104">AzCopy je nástroj příkazového řádku pro odesílání, stahování a kopírování dat do a z objektu blob, soubory a úložiště tabulek Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="e3535-104">AzCopy is a command-line utility designed for uploading, downloading, and copying data to and from Microsoft Azure blob, file, and table storage.</span></span>

<span data-ttu-id="e3535-105">Pokyny k instalaci na platformu Azure pomocí nástroje AzCopy a další informace najdete v tématu [Začínáme pomocí příkazového řádku Azcopy](../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="e3535-105">For instructions on installing AzCopy and additional information on using it with the Azure platform, see [Getting Started with the AzCopy Command-Line Utility](../storage/common/storage-use-azcopy.md).</span></span>

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> <span data-ttu-id="e3535-106">Pokud používáte virtuální počítač, který byl nastaven pomocí skriptů poskytovaných [datové vědy virtuálních počítačů v Azure](machine-learning-data-science-virtual-machines.md), pak AzCopy je již nainstalován ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="e3535-106">If you are using VM that was set up with the scripts provided by [Data Science Virtual machines in Azure](machine-learning-data-science-virtual-machines.md), then AzCopy is already installed on the VM.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="e3535-107">Dokončení Úvod do Azure blob storage, najdete v části [základy objektu Blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md) a [služby objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="e3535-107">For a complete introduction to Azure blob storage, refer to [Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and to [Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="e3535-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e3535-108">Prerequisites</span></span>
<span data-ttu-id="e3535-109">Tento dokument předpokládá, že máte předplatné Azure, účet úložiště a odpovídající klíč úložiště pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="e3535-109">This document assumes that you have an Azure subscription, a storage account and the corresponding storage key for that account.</span></span> <span data-ttu-id="e3535-110">Před nahrávání nebo stahování dat, musíte znát klíč účet a název účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="e3535-110">Before uploading/downloading data, you must know your Azure storage account name and account key.</span></span>

* <span data-ttu-id="e3535-111">Předplatné Azure, naleznete v tématu [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e3535-111">To set up an Azure subscription, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="e3535-112">Pokyny pro vytvoření účtu úložiště a získávání účet a klíčové informace naleznete v tématu [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="e3535-112">For instructions on creating a storage account and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

## <a name="run-azcopy-commands"></a><span data-ttu-id="e3535-113">Příkazy spouštějte AzCopy</span><span class="sxs-lookup"><span data-stu-id="e3535-113">Run AzCopy commands</span></span>
<span data-ttu-id="e3535-114">Ke spuštění příkazů AzCopy, otevřete okno příkazového řádku a přejděte do instalačního adresáře nástroje AzCopy ve vašem počítači, kde se nachází AzCopy.exe spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="e3535-114">To run AzCopy commands, open a command window and navigate to the AzCopy installation directory on your computer, where the AzCopy.exe executable is located.</span></span> 

<span data-ttu-id="e3535-115">Základní syntaxe pro příkazy AzCopy je:</span><span class="sxs-lookup"><span data-stu-id="e3535-115">The basic syntax for AzCopy commands is:</span></span>

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> <span data-ttu-id="e3535-116">Můžete přidat umístění instalace AzCopy cestu v systému a pak spusťte příkazy z libovolného adresáře.</span><span class="sxs-lookup"><span data-stu-id="e3535-116">You can add the AzCopy installation location to your system path and then run the commands from any directory.</span></span> <span data-ttu-id="e3535-117">Ve výchozím nastavení, je nainstalován nástroj AzCopy k *% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy* nebo *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.</span><span class="sxs-lookup"><span data-stu-id="e3535-117">By default, AzCopy is installed to *%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy* or *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.</span></span>
> 
> 

## <a name="upload-files-to-an-azure-blob"></a><span data-ttu-id="e3535-118">Nahrání souborů do objektu blob Azure</span><span class="sxs-lookup"><span data-stu-id="e3535-118">Upload files to an Azure blob</span></span>
<span data-ttu-id="e3535-119">Když Pokud chcete nahrát soubor, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3535-119">To upload a file, use the following command:</span></span>

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a><span data-ttu-id="e3535-120">Stáhnout soubory z objektu blob Azure</span><span class="sxs-lookup"><span data-stu-id="e3535-120">Download files from an Azure blob</span></span>
<span data-ttu-id="e3535-121">Ke stažení souboru z objektu blob Azure, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3535-121">To download a file from an Azure blob, use the following command:</span></span>

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a><span data-ttu-id="e3535-122">Přenos mezi Azure kontejnery objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="e3535-122">Transfer blobs between Azure containers</span></span>
<span data-ttu-id="e3535-123">Objekty BLOB přenášet mezi Azure kontejnery, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3535-123">To transfer blobs between Azure containers, use the following command:</span></span>

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a><span data-ttu-id="e3535-124">Tipy pro použití nástroje AzCopy</span><span class="sxs-lookup"><span data-stu-id="e3535-124">Tips for using AzCopy</span></span>
> [!TIP]
> 1. <span data-ttu-id="e3535-125">Když **odesílání** soubory, */S* nahrávání souborů rekurzivně.</span><span class="sxs-lookup"><span data-stu-id="e3535-125">When **uploading** files, */S* uploads files recursively.</span></span> <span data-ttu-id="e3535-126">Tento parametr nejsou uloženy soubory nacházejí v podadresářích adresáře.</span><span class="sxs-lookup"><span data-stu-id="e3535-126">Without this parameter, files in subdirectories are not uploaded.</span></span>  
> 2. <span data-ttu-id="e3535-127">Když **stahování** souboru */S* Vyhledá rekurzivně kontejneru dokud všechny soubory v určeném adresáři a jejích podadresářů nebo všechny soubory, které odpovídají zadanému vzoru v daném adresáři a jejích podadresářů, se stáhnou.</span><span class="sxs-lookup"><span data-stu-id="e3535-127">When **downloading** file, */S* searches the container recursively until all files in the specified directory and its subdirectories, or all files that match the specified pattern in the given directory and its subdirectories, are downloaded.</span></span>  
> 3. <span data-ttu-id="e3535-128">Nelze zadat **konkrétní objekt blob souboru** ke stažení pomocí */Source* parametr.</span><span class="sxs-lookup"><span data-stu-id="e3535-128">You cannot specify a **specific blob file** to download using the */Source* parameter.</span></span> <span data-ttu-id="e3535-129">Pokud chcete stáhnout konkrétní soubor, zadejte název objektu blob souboru ke stažení pomocí */vzor* parametr.</span><span class="sxs-lookup"><span data-stu-id="e3535-129">To download a specific file, specify the blob file name to download using the */Pattern* parameter.</span></span> <span data-ttu-id="e3535-130">**/S** parametr můžete použít tak, aby měl AzCopy hledat rekurzivně vzor názvu souboru.</span><span class="sxs-lookup"><span data-stu-id="e3535-130">**/S** parameter can be used to have AzCopy look for a file name pattern recursively.</span></span> <span data-ttu-id="e3535-131">Bez parametru vzor AzCopy stáhne všechny soubory v tomto adresáři.</span><span class="sxs-lookup"><span data-stu-id="e3535-131">Without the pattern parameter, AzCopy downloads all files in that directory.</span></span>
> 
> 

