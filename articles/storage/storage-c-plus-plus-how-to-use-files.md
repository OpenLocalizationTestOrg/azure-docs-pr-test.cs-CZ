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
# <a name="develop-for-azure-file-storage-with-c"></a><span data-ttu-id="060c1-103">Vývoj pro Azure File storage s C++</span><span class="sxs-lookup"><span data-stu-id="060c1-103">Develop for Azure File storage with C++</span></span>
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="060c1-104">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="060c1-104">About this tutorial</span></span>

<span data-ttu-id="060c1-105">V tomto kurzu se dozvíte jak tooperform základní operace v Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="060c1-105">In this tutorial, you'll learn how tooperform basic operations on Azure File storage.</span></span> <span data-ttu-id="060c1-106">Prostřednictvím napsané v jazyce C++ – ukázky, dozvíte, jak toocreate sdílí a adresáře, odesílání, seznamu a odstraňovat soubory.</span><span class="sxs-lookup"><span data-stu-id="060c1-106">Through samples written in C++, you'll learn how toocreate shares and directories, upload, list, and delete files.</span></span> <span data-ttu-id="060c1-107">Pokud se úložiště File nového tooAzure, bude užitečné pro porozumění hello ukázky projít hello koncepty v hello oddíly, které následují.</span><span class="sxs-lookup"><span data-stu-id="060c1-107">If you are new tooAzure File storage , going through hello concepts in hello sections that follow will be helpful in understanding hello samples.</span></span>


* <span data-ttu-id="060c1-108">Vytvářet a odstraňovat sdílené složky Azure File</span><span class="sxs-lookup"><span data-stu-id="060c1-108">Create and delete Azure File shares</span></span>
* <span data-ttu-id="060c1-109">Vytvářet a odstraňovat adresáře</span><span class="sxs-lookup"><span data-stu-id="060c1-109">Create and delete directories</span></span>
* <span data-ttu-id="060c1-110">Vytvoření výčtu souborů a adresářů v Azure File sdílet</span><span class="sxs-lookup"><span data-stu-id="060c1-110">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="060c1-111">Odesílání, stahování a odstranění souboru</span><span class="sxs-lookup"><span data-stu-id="060c1-111">Upload, download, and delete a file</span></span>
* <span data-ttu-id="060c1-112">Nastavit hello kvótu (maximální velikost) pro sdílení souborů Azure</span><span class="sxs-lookup"><span data-stu-id="060c1-112">Set hello quota (maximum size) for an Azure File share</span></span>
* <span data-ttu-id="060c1-113">Vytvořte sdílený přístupový podpis (SAS klíč) pro soubor, který používá definovaný ve sdílené složce hello zásady sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="060c1-113">Create a shared access signature (SAS key) for a file that uses a shared access policy defined on hello share.</span></span>

> [!Note]  
> <span data-ttu-id="060c1-114">Protože Azure File storage můžete získat přístup přes protokol SMB, je možné toowrite jednoduché aplikace, které přístup hello Azure sdílenou složku pomocí hello standardní C++ vstupně-výstupních operací třídy a funkce.</span><span class="sxs-lookup"><span data-stu-id="060c1-114">Because Azure File storage may be accessed over SMB, it is possible toowrite simple applications that access hello Azure File share using hello standard C++ I/O classes and functions.</span></span> <span data-ttu-id="060c1-115">Tento článek popisuje, jak toowrite aplikace, které používají hello C++ SDK úložiště Azure, který používá hello [REST API služby Azure File storage](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure úložiště File.</span><span class="sxs-lookup"><span data-stu-id="060c1-115">This article will describe how toowrite applications that use hello Azure Storage C++ SDK, which uses hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span></span>

## <a name="create-a-c-application"></a><span data-ttu-id="060c1-116">Vytvoření aplikace C++</span><span class="sxs-lookup"><span data-stu-id="060c1-116">Create a C++ application</span></span>
<span data-ttu-id="060c1-117">Ukázky hello toobuild, budete potřebovat tooinstall hello Klientská knihovna pro úložiště Azure 2.4.0 jazyka C++.</span><span class="sxs-lookup"><span data-stu-id="060c1-117">toobuild hello samples, you will need tooinstall hello Azure Storage Client Library 2.4.0 for C++.</span></span> <span data-ttu-id="060c1-118">Musí také jste vytvořili účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="060c1-118">You should also have created an Azure storage account.</span></span>

<span data-ttu-id="060c1-119">tooinstall hello klienta úložiště Azure 2.4.0 pro jazyk C++, můžete použít jednu z následujících metod hello:</span><span class="sxs-lookup"><span data-stu-id="060c1-119">tooinstall hello Azure Storage Client 2.4.0 for C++, you can use one of hello following methods:</span></span>

* <span data-ttu-id="060c1-120">**Linux:** postupujte podle pokynů hello uvedenému v hello [Klientská knihovna pro úložiště Azure pro C++ – soubor README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="060c1-120">**Linux:** Follow hello instructions given in hello [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>
* <span data-ttu-id="060c1-121">**Windows:** v sadě Visual Studio, klikněte na tlačítko **nástroje &gt; Správce balíčků NuGet &gt; Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="060c1-121">**Windows:** In Visual Studio, click **Tools &gt; NuGet Package Manager &gt; Package Manager Console**.</span></span> <span data-ttu-id="060c1-122">Typ hello následující příkaz do hello [Konzola správce balíčků NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="060c1-122">Type hello following command into hello [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>
  
```
Install-Package wastorage
```

## <a name="set-up-your-application-toouse-azure-file-storage"></a><span data-ttu-id="060c1-123">Nastavit toouse vaše aplikace Azure File storage</span><span class="sxs-lookup"><span data-stu-id="060c1-123">Set up your application toouse Azure File storage</span></span>
<span data-ttu-id="060c1-124">Přidáte následující hello obsahovat příkazy toohello začátek hello C++ zdrojový soubor, kam chcete toomanipulate Azure File storage:</span><span class="sxs-lookup"><span data-stu-id="060c1-124">Add hello following include statements toohello top of hello C++ source file where you want toomanipulate Azure File storage:</span></span>

```cpp
#include <was/storage_account.h>
#include <was/file.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="060c1-125">Nastavit připojovací řetězec úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="060c1-125">Set up an Azure storage connection string</span></span>
<span data-ttu-id="060c1-126">toouse úložiště File, budete potřebovat účet úložiště Azure tooyour tooconnect.</span><span class="sxs-lookup"><span data-stu-id="060c1-126">toouse File storage, you need tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="060c1-127">Hello prvním krokem by tooconfigure připojovací řetězec, který použijeme účet úložiště tooyour tooconnect.</span><span class="sxs-lookup"><span data-stu-id="060c1-127">hello first step would be tooconfigure a connection string, which we'll use tooconnect tooyour storage account.</span></span> <span data-ttu-id="060c1-128">Umožňuje definovat statické proměnné toodo který.</span><span class="sxs-lookup"><span data-stu-id="060c1-128">Let's define a static variable toodo that.</span></span>

```cpp
// Define hello connection-string with your values.
const utility::string_t 
storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="connecting-tooan-azure-storage-account"></a><span data-ttu-id="060c1-129">Připojení tooan účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="060c1-129">Connecting tooan Azure storage account</span></span>
<span data-ttu-id="060c1-130">Můžete použít hello **cloud_storage_account** třídy toorepresent informací o vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="060c1-130">You can use hello **cloud_storage_account** class toorepresent your Storage Account information.</span></span> <span data-ttu-id="060c1-131">tooretrieve účet úložiště informací z připojovacího řetězce úložiště hello, můžete použít hello **analyzovat** metoda.</span><span class="sxs-lookup"><span data-stu-id="060c1-131">tooretrieve your storage account information from hello storage connection string, you can use hello **parse** method.</span></span>

```cpp
// Retrieve storage account from connection string.    
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="create-an-azure-file-share"></a><span data-ttu-id="060c1-132">Vytvoření Azure sdílené složky</span><span class="sxs-lookup"><span data-stu-id="060c1-132">Create an Azure File share</span></span>
<span data-ttu-id="060c1-133">Všechny soubory a adresáře v úložišti Azure File jsou umístěny v kontejneru názvem **sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="060c1-133">All files and directories in Azure File storage reside in a container called a **Share**.</span></span> <span data-ttu-id="060c1-134">Váš účet úložiště může mít libovolný počet sdílených složek jako umožňuje kapacitě vašeho účtu.</span><span class="sxs-lookup"><span data-stu-id="060c1-134">Your storage account can have as many shares as your account capacity allows.</span></span> <span data-ttu-id="060c1-135">tooobtain přístup tooa sdílenou složku a její obsah, je nutné toouse klienta úložiště Azure File.</span><span class="sxs-lookup"><span data-stu-id="060c1-135">tooobtain access tooa share and its contents, you need toouse a Azure File storage client.</span></span>

```cpp
// Create hello Azure File storage client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();
```

<span data-ttu-id="060c1-136">Pomocí klienta aplikace hello Azure File storage můžete získat, sdílenou složku tooa odkaz.</span><span class="sxs-lookup"><span data-stu-id="060c1-136">Using hello Azure File storage client, you can then obtain a reference tooa share.</span></span>

```cpp
// Get a reference toohello file share
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
```

<span data-ttu-id="060c1-137">sdílené složky hello toocreate, použijte hello **create_if_not_exists** metoda hello **cloud_file_share** objektu.</span><span class="sxs-lookup"><span data-stu-id="060c1-137">toocreate hello share, use hello **create_if_not_exists** method of hello **cloud_file_share** object.</span></span>

```cpp
if (share.create_if_not_exists()) {    
    std::wcout << U("New share created") << std::endl;    
}
```

<span data-ttu-id="060c1-138">V tomto okamžiku **sdílet** obsahuje odkaz na tooa sdílenou složku s názvem **my ukázka share**.</span><span class="sxs-lookup"><span data-stu-id="060c1-138">At this point, **share** holds a reference tooa share named **my-sample-share**.</span></span>

## <a name="delete-an-azure-file-share"></a><span data-ttu-id="060c1-139">Odstranit sdílenou složku Azure</span><span class="sxs-lookup"><span data-stu-id="060c1-139">Delete an Azure File share</span></span>
<span data-ttu-id="060c1-140">Odstranění sdílené složky se provádí volání hello **delete_if_exists** metoda cloud_file_share objektu.</span><span class="sxs-lookup"><span data-stu-id="060c1-140">Deleting a share is done by calling hello **delete_if_exists** method on a cloud_file_share object.</span></span> <span data-ttu-id="060c1-141">Tady je ukázkový kód, který nemá.</span><span class="sxs-lookup"><span data-stu-id="060c1-141">Here's sample code that does that.</span></span>

```cpp
// Get a reference toohello share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// delete hello share if exists
share.delete_share_if_exists();
```

## <a name="create-a-directory"></a><span data-ttu-id="060c1-142">Vytvoření adresáře</span><span class="sxs-lookup"><span data-stu-id="060c1-142">Create a directory</span></span>
<span data-ttu-id="060c1-143">Úložiště můžete uspořádat umístěním soubory podadresáře místo nutnosti všechny z nich v kořenovém adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="060c1-143">You can organize storage by putting files inside subdirectories instead of having all of them in hello root directory.</span></span> <span data-ttu-id="060c1-144">Úložiště Azure File umožňuje toocreate jako víc adresářů jako váš účet se povolit.</span><span class="sxs-lookup"><span data-stu-id="060c1-144">Azure File storage allows you toocreate as many directories as your account will allow.</span></span> <span data-ttu-id="060c1-145">Hello následující kód vytvoří adresář s názvem **Moje ukázka directory** pod kořenovým adresářem hello, jakož i podadresáři s názvem **Moje ukázka podadresáři**.</span><span class="sxs-lookup"><span data-stu-id="060c1-145">hello code below will create a directory named **my-sample-directory** under hello root directory as well as a subdirectory named **my-sample-subdirectory**.</span></span>

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

## <a name="delete-a-directory"></a><span data-ttu-id="060c1-146">Odstranění adresáře</span><span class="sxs-lookup"><span data-stu-id="060c1-146">Delete a directory</span></span>
<span data-ttu-id="060c1-147">Při odstranění adresáře je jednoduchou úlohou, i když je potřeba poznamenat, že nelze odstranit adresář, který ještě obsahuje soubory nebo ostatních adresářů.</span><span class="sxs-lookup"><span data-stu-id="060c1-147">Deleting a directory is a simple task, although it should be noted that you cannot delete a directory that still contains files or other directories.</span></span>

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

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="060c1-148">Vytvoření výčtu souborů a adresářů v Azure File sdílet</span><span class="sxs-lookup"><span data-stu-id="060c1-148">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="060c1-149">Získání seznamu souborů a adresářů v rámci sdílené složky se snadno provádí voláním **list_files_and_directories** na **cloud_file_directory** odkaz.</span><span class="sxs-lookup"><span data-stu-id="060c1-149">Obtaining a list of files and directories within a share is easily done by calling **list_files_and_directories** on a **cloud_file_directory** reference.</span></span> <span data-ttu-id="060c1-150">tooaccess hello bohatou sadu vlastností a metod vrácené **list_file_and_directory_item**, musí volat hello **list_file_and_directory_item.as_file** metoda tooget **cloud_ soubor** objekt nebo hello **list_file_and_directory_item.as_directory** metoda tooget **cloud_file_directory** objektu.</span><span class="sxs-lookup"><span data-stu-id="060c1-150">tooaccess hello rich set of properties and methods for a returned **list_file_and_directory_item**, you must call hello **list_file_and_directory_item.as_file** method tooget a **cloud_file** object, or hello **list_file_and_directory_item.as_directory** method tooget a **cloud_file_directory** object.</span></span>

<span data-ttu-id="060c1-151">Hello následující kód ukazuje, jak tooretrieve a výstup hello URI pro každou položku v kořenovém adresáři hello hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="060c1-151">hello following code demonstrates how tooretrieve and output hello URI of each item in hello root directory of hello share.</span></span>

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

## <a name="upload-a-file"></a><span data-ttu-id="060c1-152">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="060c1-152">Upload a file</span></span>
<span data-ttu-id="060c1-153">V hello velmi alespoň, sdílenou složku Azure obsahuje kořenový adresář, kde mohou být uloženy soubory.</span><span class="sxs-lookup"><span data-stu-id="060c1-153">At hello very least, an Azure File share contains a root directory where files can reside.</span></span> <span data-ttu-id="060c1-154">V této části se dozvíte, jak tooupload soubor z místního úložiště do hello kořenový adresář sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="060c1-154">In this section, you'll learn how tooupload a file from local storage onto hello root directory of a share.</span></span>

<span data-ttu-id="060c1-155">Hello prvním krokem při nahrání souboru je tooobtain adresář toohello odkaz kde by měl být umístěn.</span><span class="sxs-lookup"><span data-stu-id="060c1-155">hello first step in uploading a file is tooobtain a reference toohello directory where it should reside.</span></span> <span data-ttu-id="060c1-156">To se provádí volání hello **get_root_directory_reference** metodu objektu hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="060c1-156">You do this by calling hello **get_root_directory_reference** method of hello share object.</span></span>

```cpp
//Get a reference toohello root directory for hello share.
azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();
```

<span data-ttu-id="060c1-157">Teď, když máte odkaz toohello kořenovým adresářem sdílené složky hello, můžete nahrát soubor do ho.</span><span class="sxs-lookup"><span data-stu-id="060c1-157">Now that you have a reference toohello root directory of hello share, you can upload a file onto it.</span></span> <span data-ttu-id="060c1-158">Tento příklad ukládání ze souboru, z textu a z datového proudu.</span><span class="sxs-lookup"><span data-stu-id="060c1-158">This example uploads from a file, from text, and from a stream.</span></span>

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

## <a name="download-a-file"></a><span data-ttu-id="060c1-159">Stažení souboru</span><span class="sxs-lookup"><span data-stu-id="060c1-159">Download a file</span></span>
<span data-ttu-id="060c1-160">soubory toodownload, nejdřív načtěte odkaz na soubor a pak zavolají hello **download_to_stream** metoda tootransfer hello souboru obsah tooa datového proudu objekt, který potom můžete zachovat trvale tooa místního souboru.</span><span class="sxs-lookup"><span data-stu-id="060c1-160">toodownload files, first retrieve a file reference and then call hello **download_to_stream** method tootransfer hello file contents tooa stream object, which you can then persist tooa local file.</span></span> <span data-ttu-id="060c1-161">Alternativně můžete použít hello **download_to_file** metoda toodownload hello obsah místního souboru tooa souboru.</span><span class="sxs-lookup"><span data-stu-id="060c1-161">Alternatively, you can use hello **download_to_file** method toodownload hello contents of a file tooa local file.</span></span> <span data-ttu-id="060c1-162">Můžete použít hello **download_text** metoda toodownload hello obsah souboru jako textový řetězec.</span><span class="sxs-lookup"><span data-stu-id="060c1-162">You can use hello **download_text** method toodownload hello contents of a file as a text string.</span></span>

<span data-ttu-id="060c1-163">Hello následující příklad používá hello **download_to_stream** a **download_text** metody toodemonstrate stahování hello soubory, které byly vytvořeny v předchozích částech.</span><span class="sxs-lookup"><span data-stu-id="060c1-163">hello following example uses hello **download_to_stream** and **download_text** methods toodemonstrate downloading hello files, which were created in previous sections.</span></span>

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

## <a name="delete-a-file"></a><span data-ttu-id="060c1-164">Odstranění souboru</span><span class="sxs-lookup"><span data-stu-id="060c1-164">Delete a file</span></span>
<span data-ttu-id="060c1-165">Další běžné operace úložiště Azure File je odstranění souborů.</span><span class="sxs-lookup"><span data-stu-id="060c1-165">Another common Azure File storage operation is file deletion.</span></span> <span data-ttu-id="060c1-166">Hello následující kód odstraní soubor s názvem Moje – ukázka – soubor-3 uložené pod kořenovým adresářem hello.</span><span class="sxs-lookup"><span data-stu-id="060c1-166">hello following code deletes a file named my-sample-file-3 stored under hello root directory.</span></span>

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

## <a name="set-hello-quota-maximum-size-for-an-azure-file-share"></a><span data-ttu-id="060c1-167">Nastavit hello kvótu (maximální velikost) pro sdílení souborů Azure</span><span class="sxs-lookup"><span data-stu-id="060c1-167">Set hello quota (maximum size) for an Azure File share</span></span>
<span data-ttu-id="060c1-168">Můžete nastavit kvótu hello (nebo maximální velikost) pro sdílenou složku, v gigabajtech.</span><span class="sxs-lookup"><span data-stu-id="060c1-168">You can set hello quota (or maximum size) for a file share, in gigabytes.</span></span> <span data-ttu-id="060c1-169">Můžete také zkontrolovat toosee množství dat, které jsou aktuálně uložené ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="060c1-169">You can also check toosee how much data is currently stored on hello share.</span></span>

<span data-ttu-id="060c1-170">Pomocí nastavení hello kvótu sdílené složky můžete omezit hello celková velikost souborů hello uložené ve sdílené složce hello.</span><span class="sxs-lookup"><span data-stu-id="060c1-170">By setting hello quota for a share, you can limit hello total size of hello files stored on hello share.</span></span> <span data-ttu-id="060c1-171">Pokud celková velikost souborů ve sdílené složce hello hello překročí kvótu hello nastavte ve sdílené složce hello a pak klienti budou nelze tooincrease hello velikost existující soubory nebo vytvořit nové soubory, pokud tyto soubory jsou prázdné.</span><span class="sxs-lookup"><span data-stu-id="060c1-171">If hello total size of files on hello share exceeds hello quota set on hello share, then clients will be unable tooincrease hello size of existing files or create new files, unless those files are empty.</span></span>

<span data-ttu-id="060c1-172">Následující příklad Hello ukazuje, jak toocheck hello aktuální využití sdílené složky a jak tooset hello kvótu pro sdílenou složku hello.</span><span class="sxs-lookup"><span data-stu-id="060c1-172">hello example below shows how toocheck hello current usage for a share and how tooset hello quota for hello share.</span></span>

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

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a><span data-ttu-id="060c1-173">Vygenerování sdíleného přístupového podpisu pro soubor nebo sdílenou složku</span><span class="sxs-lookup"><span data-stu-id="060c1-173">Generate a shared access signature for a file or file share</span></span>
<span data-ttu-id="060c1-174">Můžete vygenerovat sdílený přístupový podpis (SAS) pro sdílenou složku nebo pro konkrétní soubor.</span><span class="sxs-lookup"><span data-stu-id="060c1-174">You can generate a shared access signature (SAS) for a file share or for an individual file.</span></span> <span data-ttu-id="060c1-175">Můžete také vytvořit zásady sdíleného přístupu na toomanage sdílený přístup ke sdílení souborů signatur.</span><span class="sxs-lookup"><span data-stu-id="060c1-175">You can also create a shared access policy on a file share toomanage shared access signatures.</span></span> <span data-ttu-id="060c1-176">Vytvoření zásady sdíleného přístupu se doporučuje, protože nabízí způsob odvolání hello SAS, pokud by měl být dojde k ohrožení.</span><span class="sxs-lookup"><span data-stu-id="060c1-176">Creating a shared access policy is recommended, as it provides a means of revoking hello SAS if it should be compromised.</span></span>

<span data-ttu-id="060c1-177">Hello následující ukázka vytvoří zásada sdíleného přístupu ve sdílené složce a pak použije, že zásady omezení tooprovide hello pro SAS na souboru ve hello sdílet.</span><span class="sxs-lookup"><span data-stu-id="060c1-177">hello following example creates a shared access policy on a share, and then uses that policy tooprovide hello constraints for a SAS on a file in hello share.</span></span>

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
## <a name="next-steps"></a><span data-ttu-id="060c1-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="060c1-178">Next steps</span></span>
<span data-ttu-id="060c1-179">Další informace o Azure Storage, toolearn těchto materiálech:</span><span class="sxs-lookup"><span data-stu-id="060c1-179">toolearn more about Azure Storage, explore these resources:</span></span>

* [<span data-ttu-id="060c1-180">Klientská knihovna pro úložiště pro C++</span><span class="sxs-lookup"><span data-stu-id="060c1-180">Storage Client Library for C++</span></span>](https://github.com/Azure/azure-storage-cpp)
* <span data-ttu-id="060c1-181">[Úložiště azure služba vzorky souborů v jazyce C++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)</span><span class="sxs-lookup"><span data-stu-id="060c1-181">[Azure Storage File Service Samples in C++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)</span></span>
* [<span data-ttu-id="060c1-182">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="060c1-182">Azure Storage Explorer</span></span>](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
* [<span data-ttu-id="060c1-183">Dokumentace k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="060c1-183">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)