---
title: "úložiště objektů blob toouse (úložiště objektů) aaaHow z jazyka C++ | Microsoft Docs"
description: "Ukládání nestrukturovaných dat v cloudu hello s Azure Blob storage (úložiště objektů)."
services: storage
documentationcenter: .net
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: 53844120-1c48-4e2f-8f77-5359ed0147a4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: 0d7e7436a109ef54fc07cc238c03cfc7cf2caac0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-c"></a>Jak toouse úložiště objektů Blob z jazyka C++
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Přehled
Azure Blob storage je služba, která ukládá Nestrukturovaná data v cloudu hello jako objekty nebo objekty BLOB. Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Úložiště objektů blob je také odkazované tooas objektu úložiště.

Tato příručka popisuje, jak hello tooperform běžné scénáře s využitím služby Azure Blob storage. Hello ukázky jsou napsané v jazyce C++ a používají hello [Klientská knihovna pro úložiště Azure pro jazyk C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Hello pokryté scénáře zahrnují **odesílání**, **výpis**, **stahování**, a **odstraňování** objekty BLOB.  

> [!NOTE]
> Tato příručka cílí hello Klientská knihovna pro úložiště Azure pro jazyk C++ verze 1.0.0 a vyšší. Hello doporučená verze je klientská knihovna pro úložiště 2.2.0, která je k dispozici prostřednictvím [NuGet](http://www.nuget.org/packages/wastorage) nebo [Githubu](https://github.com/Azure/azure-storage-cpp).
> 
> 

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Vytvoření aplikace C++
V této příručce bude používat funkce úložiště, které lze spustit v rámci aplikace C++.  

toodo tedy budete potřebovat tooinstall hello Klientská knihovna pro úložiště Azure pro jazyk C++ a vytvoření účtu úložiště Azure ve vašem předplatném Azure.   

tooinstall hello Klientská knihovna pro úložiště Azure pro jazyk C++, můžete použít následující metody hello:

* **Linux:** postupujte podle pokynů hello uvedenému v hello [Klientská knihovna pro úložiště Azure pro C++ – soubor README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) stránky.  
* **Windows:** v sadě Visual Studio, klikněte na tlačítko **nástroje > Správce balíčků NuGet > Konzola správce balíčků**. Typ hello následující příkaz do hello [Konzola správce balíčků NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) a stiskněte klávesu **ENTER**.  
  
     Install-Package wastorage

## <a name="configure-your-application-tooaccess-blob-storage"></a>Konfigurace vaší aplikace tooaccess úložiště objektů Blob
Přidáte následující hello obsahovat toohello příkazy na začátek souboru C++ hello místo toouse objektů BLOB tooaccess rozhraní API hello Azure storage:  

```cpp
#include <was/storage_account.h>
#include <was/blob.h>
```

## <a name="setup-an-azure-storage-connection-string"></a>Instalační program připojovací řetězec úložiště Azure
Klienta Azure storage používá úložiště připojovací řetězec toostore koncových bodů a přihlašovací údaje pro přístup ke službám dat správy. Když spustíte v aplikaci klienta, je nutné zadat připojovací řetězec úložiště hello v hello formátu, pomocí hello název účtu a hello úložiště přístupový klíč k úložišti pro účet úložiště hello uvedené v hello [portálu Azure](https://portal.azure.com)pro hello *AccountName* a *AccountKey* hodnoty. Informace o účtech úložiště a přístupové klávesy, najdete v části [o účtech úložiště Azure](storage-create-storage-account.md). Tento příklad ukazuje, jak můžou deklarovat statické pole toohold hello připojovací řetězec:  

```cpp
// Define hello connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

tootest aplikace v místním počítači s Windows, můžete použít hello Microsoft Azure [emulátor úložiště](storage-use-emulator.md) který se instaluje s hello [Azure SDK](https://azure.microsoft.com/downloads/). emulátor úložiště Hello je nástroj, který simuluje hello objektů Blob, Queue a Table služby v Azure k dispozici na místním vývojovém počítači. Hello následující příklad ukazuje, jak lze deklarovat statické pole toohold hello připojovací řetězec tooyour emulátor místního úložiště:

```cpp
// Define hello connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

emulátor úložiště Azure toostart hello, vyberte hello **spustit** tlačítko nebo klikněte na tlačítko hello **Windows** klíč. Začněte psát **emulátoru úložiště Azure**a vyberte **emulátor úložiště Microsoft Azure** hello seznamu aplikací.  

Hello následující ukázky předpokládejme použít jednu z těchto dvou metod tooget hello úložiště připojovací řetězec.  

## <a name="retrieve-your-connection-string"></a>Načtení připojovacího řetězce
Můžete použít hello **cloud_storage_account** třídy toorepresent informací o vašem účtu úložiště. tooretrieve účet úložiště informací z připojovacího řetězce úložiště hello, můžete použít hello **analyzovat** metoda.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

V dalším kroku získat odkaz na tooa **cloud_blob_client** třídy jako umožňuje tooretrieve objekty, které představují kontejnery a objekty BLOB uložené v rámci hello služby úložiště objektů Blob. Hello následující kód vytvoří **cloud_blob_client** objekt, který používá objekt účtu úložiště hello nemůžeme načíst výše:  

```cpp
// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  
```

## <a name="how-to-create-a-container"></a>Postupy: vytvoření kontejneru
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Tento příklad ukazuje, jak toocreate kontejner, pokud ještě neexistuje:  

```cpp
try
{
    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create hello blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference tooa container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Create hello container if it doesn't already exist.
    container.create_if_not_exists();
}
catch (const std::exception& e)
{
    std::wcout << U("Error: ") << e.what() << std::endl;
}  
```

Ve výchozím nastavení je hello nový kontejner privátní a je nutné zadat úložiště objektů BLOB přístup klíče toodownload z tohoto kontejneru. Pokud chcete v rámci hello kontejneru dostupné tooeveryone toomake hello soubory (objektů BLOB), můžete nastavit kontejner toobe hello veřejné pomocí hello následující kód:  

```cpp
// Make hello blob container publicly accessible.
azure::storage::blob_container_permissions permissions;
permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
container.upload_permissions(permissions);  
```

Kdokoli na hello Internetu může vidět objekty BLOB ve veřejném kontejneru, ale můžete upravit nebo odstranit pouze v případě, že máte příslušný přístupový klíč hello.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Postupy: nahrát objekt blob do kontejneru
Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky. Ve většině případů hello je objekt blob bloku hello doporučená toouse typu.  

tooupload objekt blob bloku souboru tooa získejte odkaz na kontejner a použít ho tooget odkaz na objekt blob bloku. Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud dat tooit pomocí volání hello **upload_from_stream** metoda. Tato operace vytvoří objekt blob hello, pokud nebyla dříve neexistuje, nebo ho přepíše, pokud existuje. Následující příklad ukazuje, jak Hello tooupload objekt blob do kontejneru a předpokládá, že hello kontejner byl již vytvořen.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference tooa blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Create or overwrite hello "my-blob-1" blob with contents from a local file.
concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
blockBlob.upload_from_stream(input_stream);
input_stream.close().wait();

// Create or overwrite hello "my-blob-2" and "my-blob-3" blobs with contents from text.
// Retrieve a reference tooa blob named "my-blob-2".
azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
blob2.upload_text(U("more text"));

// Retrieve a reference tooa blob named "my-blob-3".
azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
blob3.upload_text(U("other text"));  
```

Alternativně můžete použít hello **upload_from_file** metoda tooupload tooa soubor – objekt blob bloku.

## <a name="how-to-list-hello-blobs-in-a-container"></a>Postupy: seznam hello objektů BLOB v kontejneru
toolist hello objekty BLOB v kontejneru, nejdřív získejte odkaz na kontejner. Pak můžete použít hello kontejneru **list_blobs** metoda tooretrieve hello objekty BLOB a/nebo obsažené adresáře. tooaccess hello bohatou sadu vlastností a metod vrácené **list_blob_item**, musí volat hello **list_blob_item.as_blob** metoda tooget **cloud_blob** objektu nebo hello **list_blob.as_directory** metoda tooget cloud_blob_directory objektu. Hello následující kód ukazuje, jak tooretrieve a výstup hello URI pro každou položku v hello **Moje ukázkový kontejner** kontejneru:

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Output URI of each item.
azure::storage::list_blob_item_iterator end_of_results;
for (auto it = container.list_blobs(); it != end_of_results; ++it)
{
    if (it->is_blob())
    {
        std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
    }
    else
    {
        std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
    }
}
```

Další podrobnosti týkající se výpisu operací najdete v tématu [seznamu prostředků úložiště Azure v jazyce C++](storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Postupy: stáhnout objekty BLOB
toodownload objekty BLOB, nejdřív načtěte odkaz na objekt blob a pak zavolají hello **download_to_stream** metoda. Hello následující příklad používá hello **download_to_stream** metoda tootransfer hello blob obsah tooa objektu stream, potom můžete zachovat tooa místního souboru.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference tooa blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Save blob contents tooa file.
concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
concurrency::streams::ostream output_stream(buffer);
blockBlob.download_to_stream(output_stream);

std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
std::vector<unsigned char>& data = buffer.collection();

outfile.write((char *)&data[0], buffer.size());
outfile.close();  
```

Alternativně můžete použít hello **download_to_file** metoda toodownload hello obsah souboru tooa objektů blob.
Kromě toho můžete také použít hello **download_text** metoda toodownload hello obsah objektu blob jako textový řetězec.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference tooa blob named "my-blob-2".
azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

// Download hello contents of a blog as a text string.
utility::string_t text = text_blob.download_text();
```

## <a name="how-to-delete-blobs"></a>Postupy: odstranění objektů BLOB
toodelete objekt blob, nejdřív získejte odkaz na objekt blob a pak zavolají hello **delete_blob** metoda na něm.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference tooa blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Delete hello blob.
blockBlob.delete_blob();
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy používání blob storage hello, postupujte podle těchto odkazů toolearn Další informace o Azure Storage.  

* [Jak toouse Queue Storage z jazyka C++](storage-c-plus-plus-how-to-use-queues.md)
* [Jak toouse úložiště Table z jazyka C++](storage-c-plus-plus-how-to-use-tables.md)
* [Seznam prostředků úložiště Azure v jazyce C++](storage-c-plus-plus-enumeration.md)
* [Klientská knihovna pro úložiště pro C++ – referenční informace](http://azure.github.io/azure-storage-cpp)
* [Dokumentace k Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Přenos dat pomocí hello příkazového řádku azcopy](storage-use-azcopy.md)

