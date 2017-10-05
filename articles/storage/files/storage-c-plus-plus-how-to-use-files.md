---
title: "Vývoj pro Azure File storage s jazykem C++ | Microsoft Docs"
description: "Další informace jak vyvíjet aplikace C++ a služby, které používají Azure File storage k ukládání dat souborů."
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
ms.openlocfilehash: 86c3714327074f5576e535f67a0a2a8e761ffb46
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="develop-for-azure-file-storage-with-c"></a><span data-ttu-id="b9fa7-103">Vývoj pro Azure File storage s C++</span><span class="sxs-lookup"><span data-stu-id="b9fa7-103">Develop for Azure File storage with C++</span></span>
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="b9fa7-104">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="b9fa7-104">About this tutorial</span></span>

<span data-ttu-id="b9fa7-105">V tomto kurzu budete zjistěte, jak provádět základní operace s Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-105">In this tutorial, you'll learn how to perform basic operations on Azure File storage.</span></span> <span data-ttu-id="b9fa7-106">Prostřednictvím napsané v jazyce C++ – ukázky budete zjistěte, jak vytvářet sdílené složky a adresáře, nahrát, seznamu a odstraňovat soubory.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-106">Through samples written in C++, you'll learn how to create shares and directories, upload, list, and delete files.</span></span> <span data-ttu-id="b9fa7-107">Pokud jste ještě Azure File storage, bude užitečné pro porozumění ukázky projít koncepty v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-107">If you are new to Azure File storage , going through the concepts in the sections that follow will be helpful in understanding the samples.</span></span>


* <span data-ttu-id="b9fa7-108">Vytvářet a odstraňovat sdílené složky Azure File</span><span class="sxs-lookup"><span data-stu-id="b9fa7-108">Create and delete Azure File shares</span></span>
* <span data-ttu-id="b9fa7-109">Vytvářet a odstraňovat adresáře</span><span class="sxs-lookup"><span data-stu-id="b9fa7-109">Create and delete directories</span></span>
* <span data-ttu-id="b9fa7-110">Vytvoření výčtu souborů a adresářů v Azure File sdílet</span><span class="sxs-lookup"><span data-stu-id="b9fa7-110">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="b9fa7-111">Odesílání, stahování a odstranění souboru</span><span class="sxs-lookup"><span data-stu-id="b9fa7-111">Upload, download, and delete a file</span></span>
* <span data-ttu-id="b9fa7-112">Nastavit kvótu (maximální velikost) pro sdílení souborů Azure</span><span class="sxs-lookup"><span data-stu-id="b9fa7-112">Set the quota (maximum size) for an Azure File share</span></span>
* <span data-ttu-id="b9fa7-113">Vytvořte sdílený přístupový podpis (klíč SAS) pro soubor, který používá zásady sdíleného přístupu definované ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-113">Create a shared access signature (SAS key) for a file that uses a shared access policy defined on the share.</span></span>

> [!Note]  
> <span data-ttu-id="b9fa7-114">Protože Azure File storage můžete získat přístup přes protokol SMB, je možné psát jednoduché aplikace, které přístup k Azure souborové složce přes standardní C++ vstupně-výstupních operací třídy a funkce.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-114">Because Azure File storage may be accessed over SMB, it is possible to write simple applications that access the Azure File share using the standard C++ I/O classes and functions.</span></span> <span data-ttu-id="b9fa7-115">Tento článek popisuje, jak k psaní aplikací, které používají Azure SDK C++ úložiště, který používá [REST API služby Azure File storage](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) ke komunikaci s úložištěm Azure File.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-115">This article will describe how to write applications that use the Azure Storage C++ SDK, which uses the [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) to talk to Azure File storage.</span></span>

## <a name="create-a-c-application"></a><span data-ttu-id="b9fa7-116">Vytvoření aplikace C++</span><span class="sxs-lookup"><span data-stu-id="b9fa7-116">Create a C++ application</span></span>
<span data-ttu-id="b9fa7-117">Pokud chcete vytvořit ukázky, musíte nainstalovat Klientská knihovna pro úložiště Azure 2.4.0 jazyka C++.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-117">To build the samples, you will need to install the Azure Storage Client Library 2.4.0 for C++.</span></span> <span data-ttu-id="b9fa7-118">Musí také jste vytvořili účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-118">You should also have created an Azure storage account.</span></span>

<span data-ttu-id="b9fa7-119">Chcete-li nainstalovat klienta úložiště Azure 2.4.0 jazyka C++, můžete použít jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="b9fa7-119">To install the Azure Storage Client 2.4.0 for C++, you can use one of the following methods:</span></span>

* <span data-ttu-id="b9fa7-120">**Linux:** postupujte podle pokynů uvedených v [Klientská knihovna pro úložiště Azure pro C++ – soubor README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-120">**Linux:** Follow the instructions given in the [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>
* <span data-ttu-id="b9fa7-121">**Windows:** v sadě Visual Studio, klikněte na tlačítko **nástroje &gt; Správce balíčků NuGet &gt; Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-121">**Windows:** In Visual Studio, click **Tools &gt; NuGet Package Manager &gt; Package Manager Console**.</span></span> <span data-ttu-id="b9fa7-122">Pomocí následujícího příkazu do [Konzola správce balíčků NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-122">Type the following command into the [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>
  
```
Install-Package wastorage
```

## <a name="set-up-your-application-to-use-azure-file-storage"></a><span data-ttu-id="b9fa7-123">Nastavení aplikace k používání Azure File storage</span><span class="sxs-lookup"><span data-stu-id="b9fa7-123">Set up your application to use Azure File storage</span></span>
<span data-ttu-id="b9fa7-124">Přidáte že následující příkazy do horní části C++ zdrojový soubor, kam chcete pracovat s Azure File storage patří:</span><span class="sxs-lookup"><span data-stu-id="b9fa7-124">Add the following include statements to the top of the C++ source file where you want to manipulate Azure File storage:</span></span>

```cpp
#include <was/storage_account.h>
#include <was/file.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="b9fa7-125">Nastavit připojovací řetězec úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="b9fa7-125">Set up an Azure storage connection string</span></span>
<span data-ttu-id="b9fa7-126">Používání úložiště File, budete muset připojit k účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-126">To use File storage, you need to connect to your Azure storage account.</span></span> <span data-ttu-id="b9fa7-127">Prvním krokem by mohla být nakonfigurovat připojovací řetězec, který jsme budete používat pro připojení k vašemu účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-127">The first step would be to configure a connection string, which we'll use to connect to your storage account.</span></span> <span data-ttu-id="b9fa7-128">Umožňuje definovat statické proměnné na to.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-128">Let's define a static variable to do that.</span></span>

```cpp
// Define the connection-string with your values.
const utility::string_t 
storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="connecting-to-an-azure-storage-account"></a><span data-ttu-id="b9fa7-129">Připojení k účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="b9fa7-129">Connecting to an Azure storage account</span></span>
<span data-ttu-id="b9fa7-130">Můžete použít **cloud_storage_account** třída představující informací o vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-130">You can use the **cloud_storage_account** class to represent your Storage Account information.</span></span> <span data-ttu-id="b9fa7-131">Chcete-li načíst informace o účtu úložiště z připojovacího řetězce úložiště, můžete použít **analyzovat** metoda.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-131">To retrieve your storage account information from the storage connection string, you can use the **parse** method.</span></span>

```cpp
// Retrieve storage account from connection string.    
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="create-an-azure-file-share"></a><span data-ttu-id="b9fa7-132">Vytvoření Azure sdílené složky</span><span class="sxs-lookup"><span data-stu-id="b9fa7-132">Create an Azure File share</span></span>
<span data-ttu-id="b9fa7-133">Všechny soubory a adresáře v úložišti Azure File jsou umístěny v kontejneru názvem **sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-133">All files and directories in Azure File storage reside in a container called a **Share**.</span></span> <span data-ttu-id="b9fa7-134">Váš účet úložiště může mít libovolný počet sdílených složek jako umožňuje kapacitě vašeho účtu.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-134">Your storage account can have as many shares as your account capacity allows.</span></span> <span data-ttu-id="b9fa7-135">Pokud chcete získat přístup ke sdílené složce a její obsah, musíte použít klienta úložiště Azure File.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-135">To obtain access to a share and its contents, you need to use a Azure File storage client.</span></span>

```cpp
// Create the Azure File storage client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();
```

<span data-ttu-id="b9fa7-136">Pomocí klienta aplikace Azure File storage můžete pak získat odkaz na sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-136">Using the Azure File storage client, you can then obtain a reference to a share.</span></span>

```cpp
// Get a reference to the file share
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
```

<span data-ttu-id="b9fa7-137">Chcete-li vytvořit sdílenou složku, použijte **create_if_not_exists** metodu **cloud_file_share** objektu.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-137">To create the share, use the **create_if_not_exists** method of the **cloud_file_share** object.</span></span>

```cpp
if (share.create_if_not_exists()) {    
    std::wcout << U("New share created") << std::endl;    
}
```

<span data-ttu-id="b9fa7-138">V tomto okamžiku **sdílet** obsahuje odkaz na sdílenou složku s názvem **my ukázka share**.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-138">At this point, **share** holds a reference to a share named **my-sample-share**.</span></span>

## <a name="delete-an-azure-file-share"></a><span data-ttu-id="b9fa7-139">Odstranit sdílenou složku Azure</span><span class="sxs-lookup"><span data-stu-id="b9fa7-139">Delete an Azure File share</span></span>
<span data-ttu-id="b9fa7-140">Odstranění sdílené složky se provádí volání **delete_if_exists** metoda cloud_file_share objektu.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-140">Deleting a share is done by calling the **delete_if_exists** method on a cloud_file_share object.</span></span> <span data-ttu-id="b9fa7-141">Tady je ukázkový kód, který nemá.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-141">Here's sample code that does that.</span></span>

```cpp
// Get a reference to the share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// delete the share if exists
share.delete_share_if_exists();
```

## <a name="create-a-directory"></a><span data-ttu-id="b9fa7-142">Vytvoření adresáře</span><span class="sxs-lookup"><span data-stu-id="b9fa7-142">Create a directory</span></span>
<span data-ttu-id="b9fa7-143">Úložiště můžete uspořádat umístěním soubory podadresáře místo nutnosti všechny z nich v kořenovém adresáři.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-143">You can organize storage by putting files inside subdirectories instead of having all of them in the root directory.</span></span> <span data-ttu-id="b9fa7-144">Azure File storage můžete vytvořit tolik adresáře, které umožní váš účet.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-144">Azure File storage allows you to create as many directories as your account will allow.</span></span> <span data-ttu-id="b9fa7-145">Následující kód vytvoří adresář s názvem **Moje ukázka directory** v kořenovém adresáři, jakož i podadresáři s názvem **Moje ukázka podadresáři**.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-145">The code below will create a directory named **my-sample-directory** under the root directory as well as a subdirectory named **my-sample-subdirectory**.</span></span>

```cpp
// Retrieve a reference to a directory
azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Return value is true if the share did not exist and was successfully created.
directory.create_if_not_exists();

// Create a subdirectory.
azure::storage::cloud_file_directory subdirectory = 
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
subdirectory.create_if_not_exists();
```

## <a name="delete-a-directory"></a><span data-ttu-id="b9fa7-146">Odstranění adresáře</span><span class="sxs-lookup"><span data-stu-id="b9fa7-146">Delete a directory</span></span>
<span data-ttu-id="b9fa7-147">Při odstranění adresáře je jednoduchou úlohou, i když je potřeba poznamenat, že nelze odstranit adresář, který ještě obsahuje soubory nebo ostatních adresářů.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-147">Deleting a directory is a simple task, although it should be noted that you cannot delete a directory that still contains files or other directories.</span></span>

```cpp
// Get a reference to the share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// Get a reference to the directory.
azure::storage::cloud_file_directory directory = 
  share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Get a reference to the subdirectory you want to delete.
azure::storage::cloud_file_directory sub_directory =
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));

// Delete the subdirectory and the sample directory.
sub_directory.delete_directory_if_exists();

directory.delete_directory_if_exists();
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="b9fa7-148">Vytvoření výčtu souborů a adresářů v Azure File sdílet</span><span class="sxs-lookup"><span data-stu-id="b9fa7-148">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="b9fa7-149">Získání seznamu souborů a adresářů v rámci sdílené složky se snadno provádí voláním **list_files_and_directories** na **cloud_file_directory** odkaz.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-149">Obtaining a list of files and directories within a share is easily done by calling **list_files_and_directories** on a **cloud_file_directory** reference.</span></span> <span data-ttu-id="b9fa7-150">Pro přístup k bohaté sadě vlastností a metod vrácené **list_file_and_directory_item**, musí volat **list_file_and_directory_item.as_file** metodu za účelem získání **cloud_file** objekt, nebo **list_file_and_directory_item.as_directory** metodu za účelem získání **cloud_file_directory** objektu.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-150">To access the rich set of properties and methods for a returned **list_file_and_directory_item**, you must call the **list_file_and_directory_item.as_file** method to get a **cloud_file** object, or the **list_file_and_directory_item.as_directory** method to get a **cloud_file_directory** object.</span></span>

<span data-ttu-id="b9fa7-151">Následující kód ukazuje, jak načíst a získat výstup URI pro každou položku v kořenovém adresáři sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-151">The following code demonstrates how to retrieve and output the URI of each item in the root directory of the share.</span></span>

```cpp
//Get a reference to the root directory for the share.
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

## <a name="upload-a-file"></a><span data-ttu-id="b9fa7-152">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="b9fa7-152">Upload a file</span></span>
<span data-ttu-id="b9fa7-153">V každém obsahuje sdílenou složku Azure File kořenový adresář, kde mohou být uloženy soubory.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-153">At the very least, an Azure File share contains a root directory where files can reside.</span></span> <span data-ttu-id="b9fa7-154">V této části se dozvíte jak nahrát soubor z místního úložiště do kořenového adresáře sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-154">In this section, you'll learn how to upload a file from local storage onto the root directory of a share.</span></span>

<span data-ttu-id="b9fa7-155">Prvním krokem při nahrání souboru se má získat odkaz na adresář, kde by měl být umístěn.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-155">The first step in uploading a file is to obtain a reference to the directory where it should reside.</span></span> <span data-ttu-id="b9fa7-156">To provedete pomocí volání **get_root_directory_reference** metodu objektu sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-156">You do this by calling the **get_root_directory_reference** method of the share object.</span></span>

```cpp
//Get a reference to the root directory for the share.
azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();
```

<span data-ttu-id="b9fa7-157">Teď, když máte odkaz na kořenovém adresáři sdílené složky, můžete nahrát soubor do ho.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-157">Now that you have a reference to the root directory of the share, you can upload a file onto it.</span></span> <span data-ttu-id="b9fa7-158">Tento příklad ukládání ze souboru, z textu a z datového proudu.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-158">This example uploads from a file, from text, and from a stream.</span></span>

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

## <a name="download-a-file"></a><span data-ttu-id="b9fa7-159">Stažení souboru</span><span class="sxs-lookup"><span data-stu-id="b9fa7-159">Download a file</span></span>
<span data-ttu-id="b9fa7-160">Ke stažení souborů, nejdřív načtěte odkaz na soubor a pak zavolají **download_to_stream** způsob přenosu obsahu souboru na objekt proudu, který potom můžete zachovat do místního souboru.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-160">To download files, first retrieve a file reference and then call the **download_to_stream** method to transfer the file contents to a stream object, which you can then persist to a local file.</span></span> <span data-ttu-id="b9fa7-161">Alternativně můžete použít **download_to_file** metoda pro stažení obsahu souboru do místního souboru.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-161">Alternatively, you can use the **download_to_file** method to download the contents of a file to a local file.</span></span> <span data-ttu-id="b9fa7-162">Můžete použít **download_text** metoda pro stažení obsahu soubor jako textový řetězec.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-162">You can use the **download_text** method to download the contents of a file as a text string.</span></span>

<span data-ttu-id="b9fa7-163">Následující příklad používá **download_to_stream** a **download_text** metody k předvedení stahování souborů, které byly vytvořeny v předchozích částech.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-163">The following example uses the **download_to_stream** and **download_text** methods to demonstrate downloading the files, which were created in previous sections.</span></span>

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

## <a name="delete-a-file"></a><span data-ttu-id="b9fa7-164">Odstranění souboru</span><span class="sxs-lookup"><span data-stu-id="b9fa7-164">Delete a file</span></span>
<span data-ttu-id="b9fa7-165">Další běžné operace úložiště Azure File je odstranění souborů.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-165">Another common Azure File storage operation is file deletion.</span></span> <span data-ttu-id="b9fa7-166">Následující kód odstraní soubor s názvem Moje – ukázka – soubor-3 uložená v kořenovém adresáři.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-166">The following code deletes a file named my-sample-file-3 stored under the root directory.</span></span>

```cpp
// Get a reference to the root directory for the share.    
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

azure::storage::cloud_file_directory root_dir = 
  share.get_root_directory_reference();

azure::storage::cloud_file file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

file.delete_file_if_exists();
```

## <a name="set-the-quota-maximum-size-for-an-azure-file-share"></a><span data-ttu-id="b9fa7-167">Nastavit kvótu (maximální velikost) pro sdílení souborů Azure</span><span class="sxs-lookup"><span data-stu-id="b9fa7-167">Set the quota (maximum size) for an Azure File share</span></span>
<span data-ttu-id="b9fa7-168">Můžete nastavit kvótu (nebo maximální velikost) pro sdílenou složku, v gigabajtech.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-168">You can set the quota (or maximum size) for a file share, in gigabytes.</span></span> <span data-ttu-id="b9fa7-169">Můžete se taky podívat, kolik data je aktuálně uloženo ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-169">You can also check to see how much data is currently stored on the share.</span></span>

<span data-ttu-id="b9fa7-170">Pokud nastavíte kvótu sdílené složky, můžete omezit celkovou velikost souborů uložených ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-170">By setting the quota for a share, you can limit the total size of the files stored on the share.</span></span> <span data-ttu-id="b9fa7-171">Pokud celková velikost souborů ve sdílené složce překročí kvótu nastavenou pro sdílenou složku, klienti nebudou moct zvyšovat velikost existujících souborů, s výjimkou situace, když je velikost souborů nulová.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-171">If the total size of files on the share exceeds the quota set on the share, then clients will be unable to increase the size of existing files or create new files, unless those files are empty.</span></span>

<span data-ttu-id="b9fa7-172">Dole uvedený příklad ukazuje, jak můžete zkontrolovat aktuální využití sdílené složky a jak nastavit kvótu pro sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-172">The example below shows how to check the current usage for a share and how to set the quota for the share.</span></span>

```cpp
// Parse the connection string for the storage account.
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the file client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();

// Get a reference to the share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
if (share.exists())
{
    std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();

    //This line sets the quota to 2560GB
    share.resize(2560);

    std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();

}
```

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a><span data-ttu-id="b9fa7-173">Vygenerování sdíleného přístupového podpisu pro soubor nebo sdílenou složku</span><span class="sxs-lookup"><span data-stu-id="b9fa7-173">Generate a shared access signature for a file or file share</span></span>
<span data-ttu-id="b9fa7-174">Můžete vygenerovat sdílený přístupový podpis (SAS) pro sdílenou složku nebo pro konkrétní soubor.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-174">You can generate a shared access signature (SAS) for a file share or for an individual file.</span></span> <span data-ttu-id="b9fa7-175">Můžete taky vytvořit sdílené zásady přístupu pro sdílenou složku ke správě sdílených přístupových podpisů.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-175">You can also create a shared access policy on a file share to manage shared access signatures.</span></span> <span data-ttu-id="b9fa7-176">Vytvoření sdílených zásad přístupu se doporučuje, protože se tím nabízí způsob odvolání SAS v případě narušení jeho integrity nebo důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-176">Creating a shared access policy is recommended, as it provides a means of revoking the SAS if it should be compromised.</span></span>

<span data-ttu-id="b9fa7-177">V následujícím příkladu se vytvoří sdílená zásada přístupu pro sdílenou složku a ta se pak použije k omezení pro SAS na souboru ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="b9fa7-177">The following example creates a shared access policy on a share, and then uses that policy to provide the constraints for a SAS on a file in the share.</span></span>

```cpp
// Parse the connection string for the storage account.
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the file client and get a reference to the share
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

    //set permissions to expire in 90 minutes
    sharedPolicy.set_expiry(utility::datetime::utc_now() + 
       utility::datetime::from_minutes(90));

    //give read and write permissions
    sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

    //set permissions for the share
    azure::storage::file_share_permissions permissions;    

    //retrieve the current list of shared access policies
    azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;

    //add the new shared policy
    policies.insert(std::make_pair(policy_name, sharedPolicy));

    //save the updated policy list
    permissions.set_policies(policies);
    share.upload_permissions(permissions);

    //Retrieve the root directory and file references
    azure::storage::cloud_file_directory root_dir = 
        share.get_root_directory_reference();
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

    // Generate a SAS for a file in the share 
    //  and associate this access policy with it.        
    utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);

    // Create a new CloudFile object from the SAS, and write some text to the file.        
    azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
    utility::string_t text = _XPLATSTR("My sample content");        
    file_with_sas.upload_text(text);        

    //Download and print URL with SAS.
    utility::string_t downloaded_text = file_with_sas.download_text();        
    ucout << downloaded_text << std::endl;        
    ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;

}
```
## <a name="next-steps"></a><span data-ttu-id="b9fa7-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b9fa7-178">Next steps</span></span>
<span data-ttu-id="b9fa7-179">Další informace o službě Azure Storage najdete v těchto zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="b9fa7-179">To learn more about Azure Storage, explore these resources:</span></span>

* [<span data-ttu-id="b9fa7-180">Klientská knihovna pro úložiště pro C++</span><span class="sxs-lookup"><span data-stu-id="b9fa7-180">Storage Client Library for C++</span></span>](https://github.com/Azure/azure-storage-cpp)
* <span data-ttu-id="b9fa7-181">[Úložiště azure služba vzorky souborů v jazyce C++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)</span><span class="sxs-lookup"><span data-stu-id="b9fa7-181">[Azure Storage File Service Samples in C++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)</span></span>
* [<span data-ttu-id="b9fa7-182">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="b9fa7-182">Azure Storage Explorer</span></span>](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
* [<span data-ttu-id="b9fa7-183">Dokumentace k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b9fa7-183">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)