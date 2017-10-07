---
title: "aaaHow toouse úložiště objektů Blob (úložiště objektů) z Ruby | Microsoft Docs"
description: "Ukládání nestrukturovaných dat v cloudu hello s Azure Blob storage (úložiště objektů)."
services: storage
documentationcenter: ruby
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: e2fe4c45-27b0-4d15-b3fb-e7eb574db717
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 776e7d788e69d4960f8dde0b783513f6b39b7a47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-ruby"></a>Jak toouse úložiště objektů Blob z Ruby
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Přehled
Azure Blob storage je služba, která ukládá Nestrukturovaná data v cloudu hello jako objekty nebo objekty BLOB. Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Úložiště objektů blob je také odkazované tooas objektu úložiště.

Tento průvodce se dozvíte, jak tooperform běžné scénáře s využitím úložiště objektů Blob. Ukázky Hello jsou zapsány pomocí hello Ruby rozhraní API. Hello pokryté scénáře zahrnují **odesílání, výpis, stahování,** a **odstraňování** objekty BLOB.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Vytvoření Ruby aplikace
Vytvořte aplikaci pro poznámky Ruby. Pokyny najdete v tématu [Ruby, na které webové aplikace ve virtuálním počítači Azure](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)

## <a name="configure-your-application-tooaccess-storage"></a>Konfigurace vaší aplikace tooaccess úložiště
toouse Azure Storage, potřebujete toodownload a použití hello Ruby balíček azure, který obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště hello.

### <a name="use-rubygems-tooobtain-hello-package"></a>Použijte RubyGems tooobtain hello balíček
1. Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).
2. Zadejte "gem instalace azure" hello příkazového okna tooinstall hello gem a závislosti.

### <a name="import-hello-package"></a>Importovat balíček hello
Pomocí svém oblíbeném textovém editoru, přidejte následující toohello horní části hello Ruby souboru, kde chcete úložiště toouse hello:

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a>Nastavit připojení k Azure Storage
modul Hello azure, bude číst proměnné prostředí hello **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_ACCESS_KEY** pro účet úložiště Azure tooyour tooconnect jsou požadovány informace. Pokud nejsou nastavené těchto proměnných prostředí, je nutné zadat informace o účtu hello před použitím **Azure::Blob::BlobService** s hello následující kód:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

tooobtain tyto hodnoty ze Správce prostředků úložiště nebo klasický účet v hello portálu Azure:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Přejděte toohello úložiště účet, že který má toouse.
3. V okně Nastavení hello na hello správné, klikněte na tlačítko **přístupové klíče**.
4. V okně klíče přístup hello který se zobrazí uvidíte hello přístupový klíč 1 a 2 přístupový klíč. Můžete použít kteroukoli z nich.
5. Klikněte na tlačítko hello kopírování ikonu toocopy hello klíče toohello schránky.

## <a name="create-a-container"></a>Vytvoření kontejneru
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Hello **Azure::Blob::BlobService** objekt vám umožňuje spolupracovat s kontejnery a objekty BLOB. toocreate kontejner, použijte hello **vytvořit\_container()** metoda.

Hello následující příklad kódu vytvoří kontejner nebo vytiskne hello chyby, pokud existuje.

```ruby
azure_blob_service = Azure::Blob::BlobService.new
begin
    container = azure_blob_service.create_container("test-container")
rescue
    puts $!
end
```

Pokud chcete soubory hello toomake v kontejneru hello veřejné, můžete nastavit oprávnění hello kontejneru.

Upravíte na hello <strong>vytvořit\_container()</strong> volání toopass hello **: veřejné\_přístup\_úroveň** možnost:

```ruby
container = azure_blob_service.create_container("test-container",
    :public_access_level => "<public access level>")
```

Platné hodnoty pro hello **: veřejné\_přístup\_úroveň** možnosti jsou:

* **objekt BLOB:** Určuje veřejný přístup pro čtení pro objekty BLOB. Data objektů BLOB v tomto kontejneru může číst přes anonymní žádost, ale kontejneru data nejsou k dispozici. Klienty nelze vytvořit výčet objektů BLOB v kontejneru hello přes anonymní žádost.
* **kontejner:** Určuje úplné veřejný přístup pro čtení pro data kontejnerů a objektů blob. Klienti, můžete vytvořit výčet objektů BLOB v kontejneru hello přes anonymní žádost, ale nemůže vytvořit výčet kontejnery v rámci účtu úložiště hello.

Alternativně můžete upravit úroveň veřejný přístup hello kontejner pomocí **nastavit\_kontejneru\_acl()** metoda toospecify hello veřejný přístup úroveň.

Následující ukázka kódu změny hello veřejný přístup úroveň příliš Hello**kontejneru**:

```ruby
azure_blob_service.set_container_acl('test-container', "container")
```

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektu blob do kontejneru
Objekt blob obsahu tooa tooupload, použijte hello **vytvořit\_bloku\_blob()** metoda toocreate hello objektů blob, použijte soubor nebo řetězec jako hello obsahu objektu hello blob.

Hello následující kód odešle hello soubor **test.png** jako nový blob s názvem "– Objekt blob image" v kontejneru hello.

```ruby
content = File.open("test.png", "rb") { |file| file.read }
blob = azure_blob_service.create_block_blob(container.name,
    "image-blob", content)
puts blob.name
```

## <a name="list-hello-blobs-in-a-container"></a>Seznam hello objekty BLOB v kontejneru
kontejnery hello toolist, používají **list_containers()** metoda.
použít toolist hello objektů BLOB v kontejneru, **seznamu\_blobs()** metoda.

To výstupy hello adresy URL všech objektů BLOB hello v všechny kontejnery hello hello účtu.

```ruby
containers = azure_blob_service.list_containers()
containers.each do |container|
    blobs = azure_blob_service.list_blobs(container.name)
    blobs.each do |blob|
    puts blob.name
    end
end
```

## <a name="download-blobs"></a>Stáhnout objekty blob
objekty BLOB toodownload, použijte hello **získat\_blob()** metoda tooretrieve hello obsah.

Hello následující příklad kódu ukazuje, jak pomocí **získat\_blob()** toodownload hello obsah "Objekt blob image" a zapisují tooa místního souboru.

```ruby
blob, content = azure_blob_service.get_blob(container.name,"image-blob")
File.open("download.png","wb") {|f| f.write(content)}
```

## <a name="delete-a-blob"></a>Odstranit objekt Blob
Nakonec toodelete objekt blob, použijte hello **odstranit\_blob()** metoda. Hello následující příklad kódu ukazuje, jak toodelete objekt blob.

```ruby
azure_blob_service.delete_blob(container.name, "image-blob")
```

## <a name="next-steps"></a>Další kroky
toolearn o složitějších úlohách úložiště, použijte tyto odkazy:

* [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)
* [Azure SDK pro Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) úložišti na Githubu
* [Přenos dat pomocí příkazového řádku Azcopy hello](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

