---
title: "Vývoj pro Azure File storage s Pythonem | Microsoft Docs"
description: "Další informace jak vyvíjet aplikace Python a služby, které používají Azure File storage k ukládání dat souborů."
services: storage
documentationcenter: python
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 3dd14e8d3ea7d1e50f41633a7920a6d36becf789
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="develop-for-azure-file-storage-with-python"></a>Vývoj pro Azure File storage s Pythonem
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a>O tomto kurzu
V tomto kurzu se ukazují základy používání Python k vývoji aplikací nebo služeb, které používají Azure File storage k ukládání dat souborů. V tomto kurzu jsme vytvořit jednoduché konzolové aplikace a ukazují, jak provést základní operace s Python a Azure File storage:

* Vytvoření sdílené složky Azure File
* Vytváření adresářů
* Vytvoření výčtu souborů a adresářů v Azure File sdílet
* Odesílání, stahování a odstranění souboru

> [!Note]  
> Protože Azure File storage můžete získat přístup přes protokol SMB, je možné psát jednoduché aplikace, které přístup k Azure souborové složce přes standardní Python vstupně-výstupních operací třídy a funkce. Tento článek popisuje, jak k psaní aplikací, které používají Azure Python SDK úložiště, který používá [REST API služby Azure File storage](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) ke komunikaci s úložištěm Azure File.

### <a name="set-up-your-application-to-use-azure-file-storage"></a>Nastavení aplikace k používání Azure File storage
Přidejte následující v horní části všech Python zdrojový soubor, ve kterém chcete k programovému přístupu ke službě Azure Storage.

```python
from azure.storage.file import FileService
```

### <a name="set-up-a-connection-to-azure-file-storage"></a>Nastavit připojení k Azure File storage 
`FileService` Objekt vám umožňuje spolupracovat s sdílených složek, adresářů a souborů. Následující kód vytvoří `FileService` pomocí klíč účet a název účtu úložiště. Nahraďte `<myaccount>` a `<mykey>` s názvem účtu a klíč.

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

### <a name="create-an-azure-file-share"></a>Vytvoření Azure sdílené složky
V následujícím příkladu kódu, můžete použít `FileService` objekt, který chcete vytvořit sdílenou složku, pokud neexistuje.

```python
file_service.create_share('myshare')
```

### <a name="create-a-directory"></a>Vytvoření adresáře
Úložiště můžete navíc uspořádat umístěním souborů v podadresářích místo nutnosti všechny z nich v kořenovém adresáři. Azure File storage můžete vytvořit tolik adresáře, které umožní váš účet. Následující kód vytvoří adresář s názvem **sampledir** pod kořenovým adresářem.

```python
file_service.create_directory('myshare', 'sampledir')
```

### <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Vytvoření výčtu souborů a adresářů v Azure File sdílet
K zobrazení seznamu souborů a adresářů ve sdílené složce, použijte **seznamu\_adresáře\_a\_soubory** metoda. Tato metoda vrátí generátor. Následující kód výstupy **název** každého souboru a adresáře ve sdílené složce, do konzoly.

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

### <a name="upload-a-file"></a>Nahrání souboru 
Azure soubor, který obsahuje sdílenou složku v každém, kořenový adresář, kde mohou být uloženy soubory. V této části se dozvíte jak nahrát soubor z místního úložiště do kořenového adresáře sdílenou složku.

Chcete-li vytvořit soubor a odesílat data, použijte `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` nebo `create_file_from_text` metody. Jsou nejdůležitější metody, které provádějí potřebné rozdělování, když velikost dat přesáhne 64 MB.

`create_file_from_path`Odešle obsah souboru ze zadané cesty a `create_file_from_stream` odešle obsah z již otevřeného souboru/stream. `create_file_from_bytes`odešle pole bajtů, a `create_file_from_text` odešle zadanou textovou hodnotu pomocí zadaného kódování (výchozí hodnota je UTF-8).

V následujícím příkladu se uloží obsah **sunset.png** soubor do **myfile** souboru.

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want to create this blob in the root directory, so we specify None for the directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

### <a name="download-a-file"></a>Stažení souboru
Chcete-li stáhnout data ze souboru, použijte `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, nebo `get_file_to_text`. Jsou nejdůležitější metody, které provádějí potřebné rozdělování, když velikost dat přesáhne 64 MB.

Následující příklad ukazuje, jak pomocí `get_file_to_path` stáhnout obsah **myfile** souboru a uložte ho do **na více systémů sunset.png** souboru.

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

### <a name="delete-a-file"></a>Odstranění souboru
Nakonec odstranění souboru, zavolejte `delete_file`.

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili jak pracovat s Azure File storage s Python, postupujte podle následujících odkazech na další informace.

* [Středisko pro vývojáře programující v Pythonu](/develop/python/)
* [REST API služby Azure Storage](http://msdn.microsoft.com/library/azure/dd179355)
* [Úložiště Microsoft Azure SDK pro jazyk Python](https://github.com/Azure/azure-storage-python)