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
# <a name="move-data-tooand-from-azure-blob-storage-using-azcopy"></a>Přesun dat tooand z Azure Blob Storage pomocí AzCopy
AzCopy je nástroj příkazového řádku pro odesílání, stahování a tooand kopírování dat z objektu blob, soubory a úložiště tabulek Microsoft Azure.

Pokyny k instalaci AzCopy a další informace o používání s hello platformy Azure najdete v tématu [Začínáme s hello příkazového řádku Azcopy](../storage/common/storage-use-azcopy.md).

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Pokud používáte virtuální počítač, který byl nastaven pomocí skriptů hello poskytované [datové vědy virtuálních počítačů v Azure](machine-learning-data-science-virtual-machines.md), pak AzCopy je již nainstalován na hello virtuálních počítačů.
> 
> [!NOTE]
> Úložiště objektů blob tooAzure dokončení úvod, najdete v části příliš[základy objektu Blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md) a příliš[služby objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Požadavky
Tento dokument předpokládá, že máte předplatné Azure, účet úložiště a hello odpovídající klíč úložiště pro tento účet. Před nahrávání nebo stahování dat, musíte znát klíč účet a název účtu úložiště Azure.

* tooset si předplatné Azure, najdete v části [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).
* Pokyny pro vytvoření účtu úložiště a získávání účet a klíčové informace naleznete v tématu [účty Azure storage](../storage/common/storage-create-storage-account.md).

## <a name="run-azcopy-commands"></a>Příkazy spouštějte AzCopy
Příkazy nástroje AzCopy toorun, otevřete okno příkazového řádku a přejděte toohello instalační adresář AzCopy ve vašem počítači, kde se nachází hello AzCopy.exe spustitelný soubor. 

Hello základní syntaxe pro příkazy AzCopy je:

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> Můžete přidat hello AzCopy tooyour systému cesta k umístění instalace a pak spusťte příkazy hello z libovolného adresáře. Ve výchozím nastavení, AzCopy je nainstalován příliš*% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy* nebo *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.
> 
> 

## <a name="upload-files-tooan-azure-blob"></a>Nahrání souborů tooan objektů blob v Azure
tooupload soubor, použijte následující příkaz hello:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Stáhnout soubory z objektu blob Azure
toodownload soubor z Azure blob, hello použijte následující příkaz:

    # Downloading blobs toolocal file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Přenos mezi Azure kontejnery objektů BLOB
objekty BLOB tootransfer mezi Azure kontejnery, použijte následující příkaz hello:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: hello sub directory in hello container
    <your_local_directory>: directory of local file system where files toobe uploaded from or hello directory of local file system files toobe downloaded to
    <file_pattern>: pattern of file names toobe transferred. hello standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Tipy pro použití nástroje AzCopy
> [!TIP]
> 1. Když **odesílání** soubory, */S* nahrávání souborů rekurzivně. Tento parametr nejsou uloženy soubory nacházejí v podadresářích adresáře.  
> 2. Když **stahování** souboru */S* hledání hello kontejneru rekurzivně dokud všechny soubory v hello zadaný adresář a jejích podadresářů nebo všechny soubory, které odpovídají hello zadanému vzoru v zadané hello adresář a jejích podadresářů, se stáhnou.  
> 3. Nelze zadat **konkrétní objekt blob souboru** toodownload pomocí hello */Source* parametr. toodownload konkrétní soubor, zadejte hello objektu blob souboru název toodownload pomocí hello */vzor* parametr. **/S** parametr může být použité toohave AzCopy podívejte se rekurzivní vzor název souboru. Bez parametru hello vzor AzCopy stáhne všechny soubory v tomto adresáři.
> 
> 

