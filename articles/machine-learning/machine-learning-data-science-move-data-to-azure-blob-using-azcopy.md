---
title: "aaaMove tooand dat z Azure Blob Storage pomocí AzCopy | Microsoft Docs"
description: "Přesun dat tooand z Azure Blob Storage pomocí AzCopy"
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
ms.openlocfilehash: b381ba004ac16879b6c633950d03d13ad82d5b72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-blob-storage-using-azcopy"></a><span data-ttu-id="9df17-103">Přesun dat tooand z Azure Blob Storage pomocí AzCopy</span><span class="sxs-lookup"><span data-stu-id="9df17-103">Move data tooand from Azure Blob Storage using AzCopy</span></span>
<span data-ttu-id="9df17-104">AzCopy je nástroj příkazového řádku pro odesílání, stahování a tooand kopírování dat z objektu blob, soubory a úložiště tabulek Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9df17-104">AzCopy is a command-line utility designed for uploading, downloading, and copying data tooand from Microsoft Azure blob, file, and table storage.</span></span>

<span data-ttu-id="9df17-105">Pokyny k instalaci AzCopy a další informace o používání s hello platformy Azure najdete v tématu [Začínáme s hello příkazového řádku Azcopy](../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="9df17-105">For instructions on installing AzCopy and additional information on using it with hello Azure platform, see [Getting Started with hello AzCopy Command-Line Utility](../storage/common/storage-use-azcopy.md).</span></span>

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> <span data-ttu-id="9df17-106">Pokud používáte virtuální počítač, který byl nastaven pomocí skriptů hello poskytované [datové vědy virtuálních počítačů v Azure](machine-learning-data-science-virtual-machines.md), pak AzCopy je již nainstalován na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9df17-106">If you are using VM that was set up with hello scripts provided by [Data Science Virtual machines in Azure](machine-learning-data-science-virtual-machines.md), then AzCopy is already installed on hello VM.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="9df17-107">Úložiště objektů blob tooAzure dokončení úvod, najdete v části příliš[základy objektu Blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md) a příliš[služby objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="9df17-107">For a complete introduction tooAzure blob storage, refer too[Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and too[Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="9df17-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9df17-108">Prerequisites</span></span>
<span data-ttu-id="9df17-109">Tento dokument předpokládá, že máte předplatné Azure, účet úložiště a hello odpovídající klíč úložiště pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="9df17-109">This document assumes that you have an Azure subscription, a storage account and hello corresponding storage key for that account.</span></span> <span data-ttu-id="9df17-110">Před nahrávání nebo stahování dat, musíte znát klíč účet a název účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="9df17-110">Before uploading/downloading data, you must know your Azure storage account name and account key.</span></span>

* <span data-ttu-id="9df17-111">tooset si předplatné Azure, najdete v části [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9df17-111">tooset up an Azure subscription, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="9df17-112">Pokyny pro vytvoření účtu úložiště a získávání účet a klíčové informace naleznete v tématu [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="9df17-112">For instructions on creating a storage account and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

## <a name="run-azcopy-commands"></a><span data-ttu-id="9df17-113">Příkazy spouštějte AzCopy</span><span class="sxs-lookup"><span data-stu-id="9df17-113">Run AzCopy commands</span></span>
<span data-ttu-id="9df17-114">Příkazy nástroje AzCopy toorun, otevřete okno příkazového řádku a přejděte toohello instalační adresář AzCopy ve vašem počítači, kde se nachází hello AzCopy.exe spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="9df17-114">toorun AzCopy commands, open a command window and navigate toohello AzCopy installation directory on your computer, where hello AzCopy.exe executable is located.</span></span> 

<span data-ttu-id="9df17-115">Hello základní syntaxe pro příkazy AzCopy je:</span><span class="sxs-lookup"><span data-stu-id="9df17-115">hello basic syntax for AzCopy commands is:</span></span>

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> <span data-ttu-id="9df17-116">Můžete přidat hello AzCopy tooyour systému cesta k umístění instalace a pak spusťte příkazy hello z libovolného adresáře.</span><span class="sxs-lookup"><span data-stu-id="9df17-116">You can add hello AzCopy installation location tooyour system path and then run hello commands from any directory.</span></span> <span data-ttu-id="9df17-117">Ve výchozím nastavení, AzCopy je nainstalován příliš*% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy* nebo *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.</span><span class="sxs-lookup"><span data-stu-id="9df17-117">By default, AzCopy is installed too*%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy* or *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.</span></span>
> 
> 

## <a name="upload-files-tooan-azure-blob"></a><span data-ttu-id="9df17-118">Nahrání souborů tooan objektů blob v Azure</span><span class="sxs-lookup"><span data-stu-id="9df17-118">Upload files tooan Azure blob</span></span>
<span data-ttu-id="9df17-119">tooupload soubor, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="9df17-119">tooupload a file, use hello following command:</span></span>

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a><span data-ttu-id="9df17-120">Stáhnout soubory z objektu blob Azure</span><span class="sxs-lookup"><span data-stu-id="9df17-120">Download files from an Azure blob</span></span>
<span data-ttu-id="9df17-121">toodownload soubor z Azure blob, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9df17-121">toodownload a file from an Azure blob, use hello following command:</span></span>

    # Downloading blobs toolocal file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a><span data-ttu-id="9df17-122">Přenos mezi Azure kontejnery objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="9df17-122">Transfer blobs between Azure containers</span></span>
<span data-ttu-id="9df17-123">objekty BLOB tootransfer mezi Azure kontejnery, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="9df17-123">tootransfer blobs between Azure containers, use hello following command:</span></span>

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: hello sub directory in hello container
    <your_local_directory>: directory of local file system where files toobe uploaded from or hello directory of local file system files toobe downloaded to
    <file_pattern>: pattern of file names toobe transferred. hello standard wildcards are supported


## <a name="tips-for-using-azcopy"></a><span data-ttu-id="9df17-124">Tipy pro použití nástroje AzCopy</span><span class="sxs-lookup"><span data-stu-id="9df17-124">Tips for using AzCopy</span></span>
> [!TIP]
> 1. <span data-ttu-id="9df17-125">Když **odesílání** soubory, */S* nahrávání souborů rekurzivně.</span><span class="sxs-lookup"><span data-stu-id="9df17-125">When **uploading** files, */S* uploads files recursively.</span></span> <span data-ttu-id="9df17-126">Tento parametr nejsou uloženy soubory nacházejí v podadresářích adresáře.</span><span class="sxs-lookup"><span data-stu-id="9df17-126">Without this parameter, files in subdirectories are not uploaded.</span></span>  
> 2. <span data-ttu-id="9df17-127">Když **stahování** souboru */S* hledání hello kontejneru rekurzivně dokud všechny soubory v hello zadaný adresář a jejích podadresářů nebo všechny soubory, které odpovídají hello zadanému vzoru v zadané hello adresář a jejích podadresářů, se stáhnou.</span><span class="sxs-lookup"><span data-stu-id="9df17-127">When **downloading** file, */S* searches hello container recursively until all files in hello specified directory and its subdirectories, or all files that match hello specified pattern in hello given directory and its subdirectories, are downloaded.</span></span>  
> 3. <span data-ttu-id="9df17-128">Nelze zadat **konkrétní objekt blob souboru** toodownload pomocí hello */Source* parametr.</span><span class="sxs-lookup"><span data-stu-id="9df17-128">You cannot specify a **specific blob file** toodownload using hello */Source* parameter.</span></span> <span data-ttu-id="9df17-129">toodownload konkrétní soubor, zadejte hello objektu blob souboru název toodownload pomocí hello */vzor* parametr.</span><span class="sxs-lookup"><span data-stu-id="9df17-129">toodownload a specific file, specify hello blob file name toodownload using hello */Pattern* parameter.</span></span> <span data-ttu-id="9df17-130">**/S** parametr může být použité toohave AzCopy podívejte se rekurzivní vzor název souboru.</span><span class="sxs-lookup"><span data-stu-id="9df17-130">**/S** parameter can be used toohave AzCopy look for a file name pattern recursively.</span></span> <span data-ttu-id="9df17-131">Bez parametru hello vzor AzCopy stáhne všechny soubory v tomto adresáři.</span><span class="sxs-lookup"><span data-stu-id="9df17-131">Without hello pattern parameter, AzCopy downloads all files in that directory.</span></span>
> 
> 

