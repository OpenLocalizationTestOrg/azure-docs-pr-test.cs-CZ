---
title: aaaDevelop pro Azure File storage s jazykem C++ | Microsoft Docs
description: "Zjistěte, jak toodevelop C++ aplikací a služeb, které používají Azure File storage toostore souborová data."
services: storage
documentationcenter: .net
author: renashahmsft
manager: aungoo
editor: tysonn
ms.assetid: a1e8c99e-47a6-43a9-9541-c9262eb00b38
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2017
ms.author: renashahmsft
ms.openlocfilehash: 40c3aac94214a91121913e2ded315031aeed1c30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-c"></a>Vývoj pro Azure File storage s C++
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a>O tomto kurzu

V tomto kurzu se dozvíte jak tooperform základní operace v Azure File storage. Prostřednictvím napsané v jazyce C++ – ukázky, dozvíte, jak toocreate sdílí a adresáře, odesílání, seznamu a odstraňovat soubory. Pokud se úložiště File nového tooAzure, bude užitečné pro porozumění hello ukázky projít hello koncepty v hello oddíly, které následují.


* Vytvářet a odstraňovat sdílené složky Azure File
* Vytvářet a odstraňovat adresáře
* Vytvoření výčtu souborů a adresářů v Azure File sdílet
* Odesílání, stahování a odstranění souboru
* Nastavit hello kvótu (maximální velikost) pro sdílení souborů Azure
* Vytvořte sdílený přístupový podpis (SAS klíč) pro soubor, který používá definovaný ve sdílené složce hello zásady sdíleného přístupu.

> [!Note]  
> Protože Azure File storage můžete získat přístup přes protokol SMB, je možné toowrite jednoduché aplikace, které přístup hello Azure sdílenou složku pomocí hello standardní C++ vstupně-výstupních operací třídy a funkce. Tento článek popisuje, jak toowrite aplikace, které používají hello C++ SDK úložiště Azure, který používá hello [REST API služby Azure File storage](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure úložiště File.

## <a name="create-a-c-application"></a>Vytvoření aplikace C++
Ukázky hello toobuild, budete potřebovat tooinstall hello Klientská knihovna pro úložiště Azure 2.4.0 jazyka C++. Musí také jste vytvořili účet úložiště Azure.

tooinstall hello klienta úložiště Azure 2.4.0 pro jazyk C++, můžete použít jednu z následujících metod hello:

* **Linux:** postupujte podle pokynů hello uvedenému v hello [Klientská knihovna pro úložiště Azure pro C++ – soubor README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) stránky.
* **Windows:** v sadě Visual Studio, klikněte na tlačítko **nástroje &gt; Správce balíčků NuGet &gt; Konzola správce balíčků**. Typ hello následující příkaz do hello [Konzola správce balíčků NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) a stiskněte klávesu **ENTER**.
  
```
Install-Package wastorage
```

## <a name="set-up-your-application-toouse-azure-file-storage"></a>Nastavit toouse vaše aplikace Azure File storage
Přidáte následující hello obsahovat příkazy toohello začátek hello C++ zdrojový soubor, kam chcete toomanipulate Azure File storage:

```cpp
#include <was/storage_account.h>
#include <was/file.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Nastavit připojovací řetězec úložiště Azure
toouse úložiště File, budete potřebovat účet úložiště Azure tooyour tooconnect. Hello prvním krokem by tooconfigure připojovací řetězec, který použijeme účet úložiště tooyour tooconnect. Umožňuje definovat statické proměnné toodo který.

```cpp
// Define hello connection-string with your values.
const utility::string_t 
storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="connecting-tooan-azure-storage-account"></a>Připojení tooan účtu úložiště Azure
Můžete použít hello **cloud_storage_account** třídy toorepresent informací o vašem účtu úložiště. tooretrieve účet úložiště informací z připojovacího řetězce úložiště hello, můžete použít hello **analyzovat** metoda.

```cpp
// Retrieve storage account from connection string.    
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="create-an-azure-file-share"></a>Vytvoření Azure sdílené složky
Všechny soubory a adresáře v úložišti Azure File jsou umístěny v kontejneru názvem **sdílené složky**. Váš účet úložiště může mít libovolný počet sdílených složek jako umožňuje kapacitě vašeho účtu. tooobtain přístup tooa sdílenou složku a její obsah, je nutné toouse klienta úložiště Azure File.

```cpp
// Create hello Azure File storage client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();
```

Pomocí klienta aplikace hello Azure File storage můžete získat, sdílenou složku tooa odkaz.

```cpp
// Get a reference toohello file share
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
```

sdílené složky hello toocreate, použijte hello **create_if_not_exists** metoda hello **cloud_file_share** objektu.

```cpp
if (share.create_if_not_exists()) {    
    std::wcout << U("New share created") << std::endl;    
}
```

V tomto okamžiku **sdílet** obsahuje odkaz na tooa sdílenou složku s názvem **my ukázka share**.

## <a name="delete-an-azure-file-share"></a>Odstranit sdílenou složku Azure
Odstranění sdílené složky se provádí volání hello **delete_if_exists** metoda cloud_file_share objektu. Tady je ukázkový kód, který nemá.

```cpp
// Get a reference toohello share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// delete hello share if exists
share.delete_share_if_exists();
```

## <a name="create-a-directory"></a>Vytvoření adresáře
Úložiště můžete uspořádat umístěním soubory podadresáře místo nutnosti všechny z nich v kořenovém adresáři hello. Úložiště Azure File umožňuje toocreate jako víc adresářů jako váš účet se povolit. Hello následující kód vytvoří adresář s názvem **Moje ukázka directory** pod kořenovým adresářem hello, jakož i podadresáři s názvem **Moje ukázka podadresáři**.

```cpp
// Retrieve a reference tooa directory
azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Return value is true if hello share did not exist and was successfully created.
directory.create_if_not_exists();

// Create a subdirectory.
azure::storage::cloud_file_directory subdirectory = 
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
subdirectory.create_if_not_exists();
```

## <a name="delete-a-directory"></a>Odstranění adresáře
Při odstranění adresáře je jednoduchou úlohou, i když je potřeba poznamenat, že nelze odstranit adresář, který ještě obsahuje soubory nebo ostatních adresářů.

```cpp
// Get a reference toohello share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// Get a reference toohello directory.
azure::storage::cloud_file_directory directory = 
  share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Get a reference toohello subdirectory you want toodelete.
azure::storage::cloud_file_directory sub_directory =
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));

// Delete hello subdirectory and hello sample directory.
sub_directory.delete_directory_if_exists();

directory.delete_directory_if_exists();
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Vytvoření výčtu souborů a adresářů v Azure File sdílet
Získání seznamu souborů a adresářů v rámci sdílené složky se snadno provádí voláním **list_files_and_directories** na **cloud_file_directory** odkaz. tooaccess hello bohatou sadu vlastností a metod vrácené **list_file_and_directory_item**, musí volat hello **list_file_and_directory_item.as_file** metoda tooget **cloud_ soubor** objekt nebo hello **list_file_and_directory_item.as_directory** metoda tooget **cloud_file_directory** objektu.

Hello následující kód ukazuje, jak tooretrieve a výstup hello URI pro každou položku v kořenovém adresáři hello hello sdílené složky.

```cpp
//Get a reference toohello root directory for hello share.
azure::storage::cloud_file_directory root_dir = 
  share.get_root_directory_reference();

// Output URI of each item.
azure::storage::list_file_and_diretory_result_iterator end_of_results;

for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
{
    if(it->is_directory())
    {
        ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
    }
    else if (it->is_file())
    {
        ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
    }        
}
```

## <a name="upload-a-file"></a>Nahrání souboru
V hello velmi alespoň, sdílenou složku Azure obsahuje kořenový adresář, kde mohou být uloženy soubory. V této části se dozvíte, jak tooupload soubor z místního úložiště do hello kořenový adresář sdílené složky.

Hello prvním krokem při nahrání souboru je tooobtain adresář toohello odkaz kde by měl být umístěn. To se provádí volání hello **get_root_directory_reference** metodu objektu hello sdílené složky.

```cpp
//Get a reference toohello root directory for hello share.
azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();
```

Teď, když máte odkaz toohello kořenovým adresářem sdílené složky hello, můžete nahrát soubor do ho. Tento příklad ukládání ze souboru, z textu a z datového proudu.

```cpp
// Upload a file from a stream.
concurrency::streams::istream input_stream = 
  concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

azure::storage::cloud_file file1 = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
file1.upload_from_stream(input_stream);

// Upload some files from text.
azure::storage::cloud_file file2 = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
file2.upload_text(_XPLATSTR("more text"));

// Upload a file from a file.
azure::storage::cloud_file file4 = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
file4.upload_from_file(_XPLATSTR("DataFile.txt"));    
```

## <a name="download-a-file"></a>Stažení souboru
soubory toodownload, nejdřív načtěte odkaz na soubor a pak zavolají hello **download_to_stream** metoda tootransfer hello souboru obsah tooa datového proudu objekt, který potom můžete zachovat trvale tooa místního souboru. Alternativně můžete použít hello **download_to_file** metoda toodownload hello obsah místního souboru tooa souboru. Můžete použít hello **download_text** metoda toodownload hello obsah souboru jako textový řetězec.

Hello následující příklad používá hello **download_to_stream** a **download_text** metody toodemonstrate stahování hello soubory, které byly vytvořeny v předchozích částech.

```cpp
// Download as text
azure::storage::cloud_file text_file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
utility::string_t text = text_file.download_text();
ucout << "File Text: " << text << std::endl;

// Download as a stream.
azure::storage::cloud_file stream_file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
concurrency::streams::ostream output_stream(buffer);
stream_file.download_to_stream(output_stream);
std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
std::vector<unsigned char>& data = buffer.collection();
outfile.write((char *)&data[0], buffer.size());
outfile.close();
```

## <a name="delete-a-file"></a>Odstranění souboru
Další běžné operace úložiště Azure File je odstranění souborů. Hello následující kód odstraní soubor s názvem Moje – ukázka – soubor-3 uložené pod kořenovým adresářem hello.

```cpp
// Get a reference toohello root directory for hello share.    
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

azure::storage::cloud_file_directory root_dir = 
  share.get_root_directory_reference();

azure::storage::cloud_file file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

file.delete_file_if_exists();
```

## <a name="set-hello-quota-maximum-size-for-an-azure-file-share"></a>Nastavit hello kvótu (maximální velikost) pro sdílení souborů Azure
Můžete nastavit kvótu hello (nebo maximální velikost) pro sdílenou složku, v gigabajtech. Můžete také zkontrolovat toosee množství dat, které jsou aktuálně uložené ve složce hello.

Pomocí nastavení hello kvótu sdílené složky můžete omezit hello celková velikost souborů hello uložené ve sdílené složce hello. Pokud celková velikost souborů ve sdílené složce hello hello překročí kvótu hello nastavte ve sdílené složce hello a pak klienti budou nelze tooincrease hello velikost existující soubory nebo vytvořit nové soubory, pokud tyto soubory jsou prázdné.

Následující příklad Hello ukazuje, jak toocheck hello aktuální využití sdílené složky a jak tooset hello kvótu pro sdílenou složku hello.

```cpp
// Parse hello connection string for hello storage account.
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello file client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();

// Get a reference toohello share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
if (share.exists())
{
    std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();

    //This line sets hello quota too2560GB
    share.resize(2560);

    std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();

}
```

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Vygenerování sdíleného přístupového podpisu pro soubor nebo sdílenou složku
Můžete vygenerovat sdílený přístupový podpis (SAS) pro sdílenou složku nebo pro konkrétní soubor. Můžete také vytvořit zásady sdíleného přístupu na toomanage sdílený přístup ke sdílení souborů signatur. Vytvoření zásady sdíleného přístupu se doporučuje, protože nabízí způsob odvolání hello SAS, pokud by měl být dojde k ohrožení.

Hello následující ukázka vytvoří zásada sdíleného přístupu ve sdílené složce a pak použije, že zásady omezení tooprovide hello pro SAS na souboru ve hello sdílet.

```cpp
// Parse hello connection string for hello storage account.
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello file client and get a reference toohello share
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();

azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

if (share.exists())
{
    // Create and assign a policy
    utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

    azure::storage::file_shared_access_policy sharedPolicy = 
      azure::storage::file_shared_access_policy();

    //set permissions tooexpire in 90 minutes
    sharedPolicy.set_expiry(utility::datetime::utc_now() + 
       utility::datetime::from_minutes(90));

    //give read and write permissions
    sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

    //set permissions for hello share
    azure::storage::file_share_permissions permissions;    

    //retrieve hello current list of shared access policies
    azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;

    //add hello new shared policy
    policies.insert(std::make_pair(policy_name, sharedPolicy));

    //save hello updated policy list
    permissions.set_policies(policies);
    share.upload_permissions(permissions);

    //Retrieve hello root directory and file references
    azure::storage::cloud_file_directory root_dir = 
        share.get_root_directory_reference();
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

    // Generate a SAS for a file in hello share 
    //  and associate this access policy with it.        
    utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);

    // Create a new CloudFile object from hello SAS, and write some text toohello file.        
    azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
    utility::string_t text = _XPLATSTR("My sample content");        
    file_with_sas.upload_text(text);        

    //Download and print URL with SAS.
    utility::string_t downloaded_text = file_with_sas.download_text();        
    ucout << downloaded_text << std::endl;        
    ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;

}
```
## <a name="next-steps"></a>Další kroky
Další informace o Azure Storage, toolearn těchto materiálech:

* [Klientská knihovna pro úložiště pro C++](https://github.com/Azure/azure-storage-cpp)
* [Úložiště azure služba vzorky souborů v jazyce C++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)
* [Azure Storage Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
* [Dokumentace k Azure Storage](https://azure.microsoft.com/documentation/services/storage/)