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
ms.openlocfilehash: 8f9ca93e52b030384e28a739d2f1c6b610be094a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-blob-storage-from-python"></a>Jak toouse Azure Blob storage z Pythonu
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Přehled
Azure Blob storage je služba, která ukládá Nestrukturovaná data v cloudu hello jako objekty nebo objekty BLOB. Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Úložiště objektů blob je také odkazované tooas objektu úložiště.

Tento článek vám ukáže, jak tooperform běžné scénáře s využitím úložiště objektů Blob. Hello ukázky jsou napsané v Pythonu a používají hello [Microsoft Azure SDK úložiště pro jazyk Python]. pokryté scénáře Hello zahrnují odesílání, výpis, stahování a odstraňování objektů BLOB.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a>Vytvoření kontejneru
V závislosti na typu hello objektu blob chcete toouse, vytvoření **BlockBlobService**, **AppendBlobService**, nebo **PageBlobService** objektu. Hello následující kód používá **BlockBlobService** objektu. Přidejte následující hello v horní hello jakéhokoliv Python souboru, ve kterém chcete tooprogrammatically přístupu Azure úložiště objektů Blob bloku.

```python
from azure.storage.blob import BlockBlobService
```

Hello následující kód vytvoří **BlockBlobService** objekt, který používá hello klíč účtu úložiště účet a název.  Nahraďte název účtu a klíč 'stránku Můj účet' a 'mykey.

```python
block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')
```

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

V hello následující ukázka kódu, můžete použít **BlockBlobService** objekt toocreate hello kontejneru Pokud neexistuje.

```python
block_blob_service.create_container('mycontainer')
```

Ve výchozím nastavení, je hello nový kontejner privátní, takže musíte zadat přístupový klíč k úložišti (stejně jako dříve) toodownload objekty BLOB z tohoto kontejneru. Pokud chcete toomake hello objektů BLOB v dostupné tooeveryone hello kontejneru, můžete vytvořit kontejner hello a předat úroveň hello veřejný přístup pomocí hello následující kód.

```python
from azure.storage.blob import PublicAccess
block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)
```

Alternativně můžete upravit kontejner po vytvoření pomocí hello následující kód.

```python
block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)
```

Po této změně kdokoli na hello Internetu může vidět objekty BLOB ve veřejném kontejneru, ale pouze můžete upravit nebo odstranit.

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektu blob do kontejneru
toocreate objekt blob bloku a nahrávání dat, použijte hello **vytvořit\_blob\_z\_cesta**, **vytvořit\_blob\_z\_datového proudu**, **vytvořit\_blob\_z\_bajtů** nebo **vytvořit\_blob\_z\_text** metody. Jsou nejdůležitější metody, které provádějí hello nezbytné rozdělování když hello velikost dat hello překročí 64 MB.

**vytvořit\_objektů blob\_z\_cesta** nahrávání hello obsah souboru z hello zadané cesty, a **vytvořit\_objektů blob\_z\_datového proudu** nahrávání hello obsah z již otevřeného souboru/stream. **vytvořit\_blob\_z\_bajtů** odešle pole bajtů, a **vytvořit\_blob\_z\_text** odešle hello zadaný textové hodnoty pomocí hello zadat kódování (výchozí nastavení tooUTF-8).

Hello následujícím příkladu se uloží obsah hello hello **sunset.png** souboru do hello **můj_objekt_blob** objektů blob.

```python
from azure.storage.blob import ContentSettings
block_blob_service.create_blob_from_path(
    'mycontainer',
    'myblockblob',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png')
            )
```

## <a name="list-hello-blobs-in-a-container"></a>Seznam hello objekty BLOB v kontejneru
toolist hello objekty BLOB v kontejneru, použijte hello **seznamu\_objekty BLOB** metoda. Tato metoda vrátí generátor. Hello následující kód výstupy hello **název** z jednotlivých objektů blob v kontejneru toohello konzole.

```python
generator = block_blob_service.list_blobs('mycontainer')
for blob in generator:
    print(blob.name)
```

## <a name="download-blobs"></a>Stáhnout objekty blob
toodownload data z objektu blob, použijte **získat\_blob\_k\_cesta**, **získat\_objektů blob\_k\_datového proudu**, **získat\_blob\_k\_bajtů**, nebo **získat\_blob\_k\_text**. Jsou nejdůležitější metody, které provádějí hello nezbytné rozdělování když hello velikost dat hello překročí 64 MB.

Hello následující příklad ukazuje použití **získat\_blob\_k\_cesta** toodownload hello obsah hello **můj_objekt_blob** objektů blob a uložte ho toohello **na více systémů sunset.png** souboru.

```python
block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')
```

## <a name="delete-a-blob"></a>Odstranění objektu blob
Nakonec toodelete objekt blob, zavolejte **delete_blob**.

```python
block_blob_service.delete_blob('mycontainer', 'myblockblob')
```

## <a name="writing-tooan-append-blob"></a>Zápis tooan připojit objektů blob
Doplňovací objekt blob je optimalizován pro operace připojení, například protokolování. Podobně jako objekt blob bloku doplňovací objekt blob se skládá z bloků, ale když přidáte nový objekt blob bloku připojení tooan, je vždy připojením toohello konec objektu blob hello. Existující blok v doplňovacím objektu blob se nedá aktualizovat ani odstranit. ID Hello bloku pro doplňovací objekt blob nejsou vystavená, protože jsou pro objekt blob bloku.

Každý blok v doplňovacím objektu blob může mít různou velikost, až tooa nesmí být delší než 4 MB volného místa, a doplňovací objekt blob může obsahovat maximálně 50 000 bloků. Hello maximální velikost doplňovacího objektu BLOB je proto něco větší než 195 GB (4 MB × 50 000 bloků).

Následující příklad Hello vytvoří nový doplňovací objekt blob a připojí některá data tooit simulaci jednoduché operace protokolování.

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

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy používání Blob storage hello, postupujte podle těchto odkazů toolearn Další.

* [Středisko pro vývojáře programující v Pythonu](https://azure.microsoft.com/develop/python/)
* [REST API služby Azure Storage](http://msdn.microsoft.com/library/azure/dd179355)
* [Blog týmu Azure Storage]
* [Microsoft Azure SDK úložiště pro jazyk Python]

[Blog týmu Azure Storage]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure SDK úložiště pro jazyk Python]: https://github.com/Azure/azure-storage-python
