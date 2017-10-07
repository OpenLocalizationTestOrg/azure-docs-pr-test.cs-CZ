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
# <a name="move-data-tooand-from-azure-blob-storage-using-python"></a>Přesun dat tooand z Azure Blob Storage používá Python
Toto téma popisuje, jak toolist, odesílání a stáhnout objekty BLOB pomocí hello rozhraní API jazyka Python. Hello součástí sady Azure SDK API Python můžete:

* Vytvoření kontejneru
* Nahrání objektu blob do kontejneru
* Stáhnout objekty blob
* Seznam hello objekty BLOB v kontejneru
* Odstranění objektu blob

Další informace o používání hello Python API najdete v tématu [jak tooUse hello služby úložiště objektů Blob z Pythonu](../storage/blobs/storage-python-how-to-use-blob-storage.md).

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

## <a name="upload-data-tooblob"></a>Nahrání dat tooBlob
Přidejte následující fragment kódu v hello horní části kód Python, ve kterém chcete tooprogrammatically přístupu Azure Storage hello:

    from azure.storage.blob import BlobService

Hello **BlobService** objekt vám umožňuje spolupracovat s kontejnery a objekty BLOB. Hello následující kód vytvoří objekt BlobService pomocí hello klíč účtu úložiště účet a název. Nahraďte název účtu a klíč účtu skutečné účtu a klíč.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

Použijte následující metody tooupload data tooa blob hello:

1. uveďte\_bloku\_blob\_z\_cesta (odešle hello obsah souboru z cesty zadané hello)
2. uveďte\_block_blob\_z\_souboru (odešle hello obsah z již otevřeného souboru/proud)
3. uveďte\_bloku\_blob\_z\_bajtů (nahrávání na pole bajtů)
4. PUT\_bloku\_blob\_z\_text (odešle hello zadané textové hodnoty pomocí hello zadána, kódování)

Následující ukázkový kód Hello odešle kontejner tooa místního souboru:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Hello následující vzorový kód odesílá všechny soubory hello (s výjimkou adresáře) v místní adresář tooblob úložiště:

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


## <a name="download-data-from-blob"></a>Stahování dat z objektu Blob
Pomocí následujících metod toodownload dat z objektu blob hello:

1. získat\_blob\_k\_cesta
2. získat\_blob\_k\_souboru
3. získat\_blob\_k\_bajtů
4. získat\_blob\_k\_textu

Tyto metody, které provádějí hello nezbytné rozdělování když hello velikost dat hello překročí 64 MB.

Hello následující vzorový kód stáhne hello obsah objektu blob v kontejneru místního souboru tooa:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Hello následující vzorový kód stáhne všechny objekty BLOB kontejneru. Použije seznam\_objekty BLOB tooget hello seznam dostupných objektů BLOB v kontejneru hello a stáhne je tooa místního adresáře.

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
