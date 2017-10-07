---
title: aaaDevelop pro Azure File storage s Pythonem | Microsoft Docs
description: "Zjistěte, jak toodevelop Python aplikace a služby, které používají Azure File storage toostore souborová data."
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
ms.openlocfilehash: 45623e6dbec6f140cedc4e58e56a93fb4af9054e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-python"></a>Vývoj pro Azure File storage s Pythonem
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a>O tomto kurzu
V tomto kurzu se ukazují hello základy používání Python toodevelop aplikacím nebo službám, které používají Azure File storage toostore souborová data. V tomto kurzu bude vytvoření jednoduché konzolové aplikace a zobrazit jak tooperform základní operace s Python a Azure File storage:

* Vytvoření sdílené složky Azure File
* Vytváření adresářů
* Vytvoření výčtu souborů a adresářů v Azure File sdílet
* Odesílání, stahování a odstranění souboru

> [!Note]  
> Protože Azure File storage můžete získat přístup přes protokol SMB, je možné toowrite jednoduché aplikace, které přístup hello Azure sdílenou složku pomocí hello standardní Python vstupně-výstupních operací třídy a funkce. Tento článek popisuje, jak toowrite aplikace, které používají hello Python SDK úložiště Azure, který používá hello [REST API služby Azure File storage](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure úložiště File.

### <a name="set-up-your-application-toouse-azure-file-storage"></a>Nastavit toouse vaše aplikace Azure File storage
Přidejte následující hello v hello horní části všech Python zdrojový soubor, ve kterém chcete tooprogrammatically přístupu Azure Storage.

```python
from azure.storage.file import FileService
```

### <a name="set-up-a-connection-tooazure-file-storage"></a>Nastavení připojení tooAzure úložiště File 
Hello `FileService` objekt vám umožňuje spolupracovat s sdílených složek, adresářů a souborů. Hello následující kód vytvoří `FileService` objekt, který používá hello klíč účtu úložiště účet a název. Nahraďte `<myaccount>` a `<mykey>` s názvem účtu a klíč.

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

### <a name="create-an-azure-file-share"></a>Vytvoření Azure sdílené složky
V hello následující ukázka kódu, můžete použít `FileService` objektu toocreate hello sdílené složky pokud neexistuje.

```python
file_service.create_share('myshare')
```

### <a name="create-a-directory"></a>Vytvoření adresáře
Úložiště můžete navíc uspořádat umístěním souborů v podadresářích místo nutnosti všechny z nich v kořenovém adresáři hello. Úložiště Azure File umožňuje toocreate jako víc adresářů jako váš účet se povolit. Následující kód Hello vytvoří adresář s názvem **sampledir** pod kořenovým adresářem hello.

```python
file_service.create_directory('myshare', 'sampledir')
```

### <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Vytvoření výčtu souborů a adresářů v Azure File sdílet
toolist hello souborů a adresářů ve sdílené složce, použít hello **seznamu\_adresáře\_a\_soubory** metoda. Tato metoda vrátí generátor. Hello následující kód výstupy hello **název** z jednotlivých souborů a adresářů v konzole toohello sdílené složky.

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

### <a name="upload-a-file"></a>Nahrání souboru 
Azure soubor, který obsahuje sdílenou složku v hello velmi alespoň, kořenový adresář, kde mohou být uloženy soubory. V této části se dozvíte, jak tooupload soubor z místního úložiště do hello kořenový adresář sdílené složky.

toocreate soubor a nahrávání dat, použijte hello `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` nebo `create_file_from_text` metody. Jsou nejdůležitější metody, které provádějí hello nezbytné rozdělování když hello velikost dat hello překročí 64 MB.

`create_file_from_path`nahrávání hello obsah souboru z hello zadané cesty, a `create_file_from_stream` nahrávání hello obsah z již otevřeného souboru/stream. `create_file_from_bytes`odešle pole bajtů, a `create_file_from_text` nahrávání hello zadán textové hodnoty pomocí hello kódování (výchozí nastavení tooUTF-8).

Hello následujícím příkladu se uloží obsah hello hello **sunset.png** souboru do hello **myfile** souboru.

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want toocreate this blob in hello root directory, so we specify None for hello directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

### <a name="download-a-file"></a>Stažení souboru
toodownload data ze souboru, použijte `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, nebo `get_file_to_text`. Jsou nejdůležitější metody, které provádějí hello nezbytné rozdělování když hello velikost dat hello překročí 64 MB.

Hello následující příklad ukazuje použití `get_file_to_path` toodownload hello obsah hello **myfile** souboru a uložte ho toohello **na více systémů sunset.png** souboru.

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

### <a name="delete-a-file"></a>Odstranění souboru
Nakonec toodelete soubor, zavolejte `delete_file`.

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili jak toomanipulate Azure File storage s Python, použijte tyto odkazy toolearn Další.

* [Středisko pro vývojáře programující v Pythonu](/develop/python/)
* [REST API služby Azure Storage](http://msdn.microsoft.com/library/azure/dd179355)
* [Úložiště Microsoft Azure SDK pro jazyk Python](https://github.com/Azure/azure-storage-python)