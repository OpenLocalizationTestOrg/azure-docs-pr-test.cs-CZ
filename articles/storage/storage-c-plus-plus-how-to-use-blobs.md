---
title: "Jak používat úložiště objektů blob (úložiště objektů) z jazyka C++ | Microsoft Docs"
description: "Ukládejte nestrukturovaná data v cloudu pomocí Azure Blob Storage (úložiště objektů)."
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
ms.openlocfilehash: 3f28fbee4e267ab6962e2f73af5af6461cc16448
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-blob-storage-from-c"></a><span data-ttu-id="761cf-103">Používání úložiště Blob z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="761cf-103">How to use Blob Storage from C++</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="761cf-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="761cf-104">Overview</span></span>
<span data-ttu-id="761cf-105">Úložiště objektů blob v Azure je služba, která ukládá nestrukturovaná data v cloudu jako objekty nebo objekty blob.</span><span class="sxs-lookup"><span data-stu-id="761cf-105">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="761cf-106">Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="761cf-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="761cf-107">Blob storage se také nazývá úložiště objektů.</span><span class="sxs-lookup"><span data-stu-id="761cf-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="761cf-108">Tato příručka popisuje, jak provádět běžné scénáře s využitím služby Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="761cf-108">This guide will demonstrate how to perform common scenarios using the Azure Blob storage service.</span></span> <span data-ttu-id="761cf-109">Ukázky jsou napsané v C++ a použít [Klientská knihovna pro úložiště Azure pro jazyk C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="761cf-109">The samples are written in C++ and use the [Azure Storage Client Library for C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span></span> <span data-ttu-id="761cf-110">Pokryté scénáře zahrnují **odesílání**, **výpis**, **stahování**, a **odstraňování** objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="761cf-110">The scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span>  

> [!NOTE]
> <span data-ttu-id="761cf-111">Tato příručka se zaměřuje Klientská knihovna pro úložiště Azure pro jazyk C++ verze 1.0.0 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="761cf-111">This guide targets the Azure Storage Client Library for C++ version 1.0.0 and above.</span></span> <span data-ttu-id="761cf-112">Doporučená verze je klientská knihovna pro úložiště 2.2.0, která je k dispozici prostřednictvím [NuGet](http://www.nuget.org/packages/wastorage) nebo [Githubu](https://github.com/Azure/azure-storage-cpp).</span><span class="sxs-lookup"><span data-stu-id="761cf-112">The recommended version is Storage Client Library 2.2.0, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](https://github.com/Azure/azure-storage-cpp).</span></span>
> 
> 

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a><span data-ttu-id="761cf-113">Vytvoření aplikace C++</span><span class="sxs-lookup"><span data-stu-id="761cf-113">Create a C++ application</span></span>
<span data-ttu-id="761cf-114">V této příručce bude používat funkce úložiště, které lze spustit v rámci aplikace C++.</span><span class="sxs-lookup"><span data-stu-id="761cf-114">In this guide, you will use storage features which can be run within a C++ application.</span></span>  

<span data-ttu-id="761cf-115">V takovém případě budete muset nainstalovat Klientská knihovna pro úložiště Azure pro jazyk C++ a vytvoření účtu úložiště Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="761cf-115">To do so, you will need to install the Azure Storage Client Library for C++ and create an Azure storage account in your Azure subscription.</span></span>   

<span data-ttu-id="761cf-116">Pokud chcete nainstalovat Klientská knihovna pro úložiště Azure pro jazyk C++, můžete použít následující metody:</span><span class="sxs-lookup"><span data-stu-id="761cf-116">To install the Azure Storage Client Library for C++, you can use the following methods:</span></span>

* <span data-ttu-id="761cf-117">**Linux:** postupujte podle pokynů uvedených v [Klientská knihovna pro úložiště Azure pro C++ – soubor README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="761cf-117">**Linux:** Follow the instructions given in the [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>  
* <span data-ttu-id="761cf-118">**Windows:** v sadě Visual Studio, klikněte na tlačítko **nástroje > Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="761cf-118">**Windows:** In Visual Studio, click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="761cf-119">Pomocí následujícího příkazu do [Konzola správce balíčků NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="761cf-119">Type the following command into the [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>  
  
     <span data-ttu-id="761cf-120">Install-Package wastorage</span><span class="sxs-lookup"><span data-stu-id="761cf-120">Install-Package wastorage</span></span>

## <a name="configure-your-application-to-access-blob-storage"></a><span data-ttu-id="761cf-121">Konfigurace aplikace pro přístup k úložišti objektů Blob</span><span class="sxs-lookup"><span data-stu-id="761cf-121">Configure your application to access Blob Storage</span></span>
<span data-ttu-id="761cf-122">Přidejte následující příkazy na začátek souboru C++, ve které chcete používat pro přístup k objektům BLOB úložiště Azure rozhraní API obsahovat:</span><span class="sxs-lookup"><span data-stu-id="761cf-122">Add the following include statements to the top of the C++ file where you want to use the Azure storage APIs to access blobs:</span></span>  

```cpp
#include <was/storage_account.h>
#include <was/blob.h>
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="761cf-123">Instalační program připojovací řetězec úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="761cf-123">Setup an Azure storage connection string</span></span>
<span data-ttu-id="761cf-124">Klienta Azure storage používá připojovací řetězec úložiště k ukládání koncových bodů a pověření pro přístup ke službám dat správy.</span><span class="sxs-lookup"><span data-stu-id="761cf-124">An Azure storage client uses a storage connection string to store endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="761cf-125">Když spustíte v aplikaci klienta, je nutné zadat připojovací řetězec úložiště v následujícím formátu, pomocí názvu účtu úložiště a přístupový klíč úložiště pro účet úložiště uvedené v [portálu Azure](https://portal.azure.com) pro *AccountName* a *AccountKey* hodnoty.</span><span class="sxs-lookup"><span data-stu-id="761cf-125">When running in a client application, you must provide the storage connection string in the following format, using the name of your storage account and the storage access key for the storage account listed in the [Azure Portal](https://portal.azure.com) for the *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="761cf-126">Informace o účtech úložiště a přístupové klávesy, najdete v části [o účtech úložiště Azure](storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="761cf-126">For information on storage accounts and access keys, see [About Azure Storage Accounts](storage-create-storage-account.md).</span></span> <span data-ttu-id="761cf-127">Tento příklad ukazuje, jak můžou deklarovat statické pole pro uložení připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="761cf-127">This example shows how you can declare a static field to hold the connection string:</span></span>  

```cpp
// Define the connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

<span data-ttu-id="761cf-128">K testování aplikace v místním počítači s Windows, můžete použít Microsoft Azure [emulátor úložiště](storage-use-emulator.md) který se instaluje s [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="761cf-128">To test your application in your local Windows computer, you can use the Microsoft Azure [storage emulator](storage-use-emulator.md) that is installed with the [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="761cf-129">Emulátor úložiště je nástroj, který simuluje služby objektů Blob, fronty a tabulky, které jsou v Azure k dispozici na místním vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="761cf-129">The storage emulator is a utility that simulates the Blob, Queue, and Table services available in Azure on your local development machine.</span></span> <span data-ttu-id="761cf-130">Následující příklad ukazuje, jak můžou deklarovat statické pole pro uložení připojovací řetězec k vaší emulátor místního úložiště:</span><span class="sxs-lookup"><span data-stu-id="761cf-130">The following example shows how you can declare a static field to hold the connection string to your local storage emulator:</span></span>

```cpp
// Define the connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

<span data-ttu-id="761cf-131">Spusťte emulátor úložiště Azure, vyberte **spustit** tlačítko nebo klikněte na tlačítko **Windows** klíč.</span><span class="sxs-lookup"><span data-stu-id="761cf-131">To start the Azure storage emulator, Select the **Start** button or press the **Windows** key.</span></span> <span data-ttu-id="761cf-132">Začněte psát **emulátoru úložiště Azure**a vyberte **emulátor úložiště Microsoft Azure** ze seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="761cf-132">Begin typing **Azure Storage Emulator**, and select **Microsoft Azure Storage Emulator** from the list of applications.</span></span>  

<span data-ttu-id="761cf-133">Následující ukázky předpokládejme, že používáte jednu z těchto dvou metod k získání připojovacího řetězce úložiště.</span><span class="sxs-lookup"><span data-stu-id="761cf-133">The following samples assume that you have used one of these two methods to get the storage connection string.</span></span>  

## <a name="retrieve-your-connection-string"></a><span data-ttu-id="761cf-134">Načtení připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="761cf-134">Retrieve your connection string</span></span>
<span data-ttu-id="761cf-135">Můžete použít **cloud_storage_account** třída představující informací o vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="761cf-135">You can use the **cloud_storage_account** class to represent your Storage Account information.</span></span> <span data-ttu-id="761cf-136">Chcete-li načíst informace o účtu úložiště z připojovacího řetězce úložiště, můžete použít **analyzovat** metoda.</span><span class="sxs-lookup"><span data-stu-id="761cf-136">To retrieve your storage account information from the storage connection string, you can use the **parse** method.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

<span data-ttu-id="761cf-137">V dalším kroku získat odkaz na **cloud_blob_client** třídy jako umožňuje načtení objektů, které představují kontejnery a objekty BLOB uložené v rámci služby úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="761cf-137">Next, get a reference to a **cloud_blob_client** class as it allows you to retrieve objects that represent containers and blobs stored within the Blob Storage Service.</span></span> <span data-ttu-id="761cf-138">Následující kód vytvoří **cloud_blob_client** pomocí objekt účtu úložiště jsme načíst výše:</span><span class="sxs-lookup"><span data-stu-id="761cf-138">The following code creates a **cloud_blob_client** object using the storage account object we retrieved above:</span></span>  

```cpp
// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  
```

## <a name="how-to-create-a-container"></a><span data-ttu-id="761cf-139">Postupy: vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="761cf-139">How to: Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="761cf-140">Tento příklad ukazuje, jak vytvořit kontejner, pokud ještě neexistuje:</span><span class="sxs-lookup"><span data-stu-id="761cf-140">This example shows how to create a container if it does not already exist:</span></span>  

```cpp
try
{
    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Create the container if it doesn't already exist.
    container.create_if_not_exists();
}
catch (const std::exception& e)
{
    std::wcout << U("Error: ") << e.what() << std::endl;
}  
```

<span data-ttu-id="761cf-141">Ve výchozím nastavení je nový kontejner privátní a je nutné zadat přístupový klíč k úložišti ke stažení z tohoto kontejneru objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="761cf-141">By default, the new container is private and you must specify your storage access key to download blobs from this container.</span></span> <span data-ttu-id="761cf-142">Pokud chcete, aby soubory (objektů BLOB) v kontejneru dostupné pro všechny uživatele, můžete nastavit kontejner, aby se veřejné, pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="761cf-142">If you want to make the files (blobs) within the container available to everyone, you can set the container to be public using the following code:</span></span>  

```cpp
// Make the blob container publicly accessible.
azure::storage::blob_container_permissions permissions;
permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
container.upload_permissions(permissions);  
```

<span data-ttu-id="761cf-143">Kdokoli na Internetu může vidět objekty BLOB ve veřejném kontejneru, ale můžete upravit nebo odstranit pouze v případě, že máte příslušný přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="761cf-143">Anyone on the Internet can see blobs in a public container, but you can modify or delete them only if you have the appropriate access key.</span></span>  

## <a name="how-to-upload-a-blob-into-a-container"></a><span data-ttu-id="761cf-144">Postupy: nahrát objekt blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="761cf-144">How to: Upload a blob into a container</span></span>
<span data-ttu-id="761cf-145">Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky.</span><span class="sxs-lookup"><span data-stu-id="761cf-145">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="761cf-146">Ve většině případů se jako vhodný typ k použití doporučuje objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="761cf-146">In the majority of cases, block blob is the recommended type to use.</span></span>  

<span data-ttu-id="761cf-147">Když chcete nahrát soubor do objektu blob bloku, získejte odkaz na kontejner a použijte ho k získání odkazu objektu blob bloku.</span><span class="sxs-lookup"><span data-stu-id="761cf-147">To upload a file to a block blob, get a container reference and use it to get a block blob reference.</span></span> <span data-ttu-id="761cf-148">Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud dat k němu voláním **upload_from_stream** metoda.</span><span class="sxs-lookup"><span data-stu-id="761cf-148">Once you have a blob reference, you can upload any stream of data to it by calling the **upload_from_stream** method.</span></span> <span data-ttu-id="761cf-149">Tahle operace vytvoří objekt blob, pokud už dříve neexistoval, nebo ho přepíše, pokud už existoval.</span><span class="sxs-lookup"><span data-stu-id="761cf-149">This operation will create the blob if it didn't previously exist, or overwrite it if it does exist.</span></span> <span data-ttu-id="761cf-150">Následující příklad ukazuje, jak nahrát objekt blob do kontejneru, zároveň předpokládá, že kontejner byl již vytvořen.</span><span class="sxs-lookup"><span data-stu-id="761cf-150">The following example shows how to upload a blob into a container and assumes that the container was already created.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Create or overwrite the "my-blob-1" blob with contents from a local file.
concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
blockBlob.upload_from_stream(input_stream);
input_stream.close().wait();

// Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
// Retrieve a reference to a blob named "my-blob-2".
azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
blob2.upload_text(U("more text"));

// Retrieve a reference to a blob named "my-blob-3".
azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
blob3.upload_text(U("other text"));  
```

<span data-ttu-id="761cf-151">Alternativně můžete použít **upload_from_file** metoda chcete nahrát soubor do objektu blob bloku.</span><span class="sxs-lookup"><span data-stu-id="761cf-151">Alternatively, you can use the **upload_from_file** method to upload a file to a block blob.</span></span>

## <a name="how-to-list-the-blobs-in-a-container"></a><span data-ttu-id="761cf-152">Postupy: seznam objektů BLOB v kontejneru</span><span class="sxs-lookup"><span data-stu-id="761cf-152">How to: List the blobs in a container</span></span>
<span data-ttu-id="761cf-153">Pokud chcete mít seznam objektů blob v kontejneru, nejdřív získejte odkaz na kontejner.</span><span class="sxs-lookup"><span data-stu-id="761cf-153">To list the blobs in a container, first get a container reference.</span></span> <span data-ttu-id="761cf-154">Pak můžete použít kontejneru **list_blobs** metoda pro načtení objektů BLOB a/nebo obsažené adresáře.</span><span class="sxs-lookup"><span data-stu-id="761cf-154">You can then use the container's **list_blobs** method to retrieve the blobs and/or directories within it.</span></span> <span data-ttu-id="761cf-155">Pro přístup k bohaté sadě vlastností a metod vrácené **list_blob_item**, musí volat **list_blob_item.as_blob** metodu za účelem získání **cloud_blob** objekt, nebo **list_blob.as_directory** metoda, jak získat objekt cloud_blob_directory.</span><span class="sxs-lookup"><span data-stu-id="761cf-155">To access the rich set of properties and methods for a returned **list_blob_item**, you must call the **list_blob_item.as_blob** method to get a  **cloud_blob** object, or the **list_blob.as_directory** method to get a  cloud_blob_directory object.</span></span> <span data-ttu-id="761cf-156">Následující kód ukazuje, jak načíst a získat výstup URI pro každou položku v **Moje ukázkový kontejner** kontejneru:</span><span class="sxs-lookup"><span data-stu-id="761cf-156">The following code demonstrates how to retrieve and output the URI of each item in the **my-sample-container** container:</span></span>

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
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

<span data-ttu-id="761cf-157">Další podrobnosti týkající se výpisu operací najdete v tématu [seznamu prostředků úložiště Azure v jazyce C++](storage-c-plus-plus-enumeration.md).</span><span class="sxs-lookup"><span data-stu-id="761cf-157">For more details on listing operations, see [List Azure Storage Resources in C++](storage-c-plus-plus-enumeration.md).</span></span>

## <a name="how-to-download-blobs"></a><span data-ttu-id="761cf-158">Postupy: stáhnout objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="761cf-158">How to: Download blobs</span></span>
<span data-ttu-id="761cf-159">Pokud chcete stáhnout objekty BLOB, nejdřív načtěte odkaz na objekt blob a potom zavolejte **download_to_stream** metoda.</span><span class="sxs-lookup"><span data-stu-id="761cf-159">To download blobs, first retrieve a blob reference and then call the **download_to_stream** method.</span></span> <span data-ttu-id="761cf-160">Následující příklad používá **download_to_stream** způsob přenosu obsahu objektu blob na objekt proudu, který potom můžete zachovat do místního souboru.</span><span class="sxs-lookup"><span data-stu-id="761cf-160">The following example uses the **download_to_stream** method to transfer the blob contents to a stream object that you can then persist to a local file.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Save blob contents to a file.
concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
concurrency::streams::ostream output_stream(buffer);
blockBlob.download_to_stream(output_stream);

std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
std::vector<unsigned char>& data = buffer.collection();

outfile.write((char *)&data[0], buffer.size());
outfile.close();  
```

<span data-ttu-id="761cf-161">Alternativně můžete použít **download_to_file** metoda pro stažení obsahu objektu blob do souboru.</span><span class="sxs-lookup"><span data-stu-id="761cf-161">Alternatively, you can use the **download_to_file** method to download the contents of a blob to a file.</span></span>
<span data-ttu-id="761cf-162">Kromě toho můžete také použít **download_text** metoda pro stažení obsahu objektu blob jako textový řetězec.</span><span class="sxs-lookup"><span data-stu-id="761cf-162">In addition, you can also use the **download_text** method to download the contents of a blob as a text string.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-2".
azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

// Download the contents of a blog as a text string.
utility::string_t text = text_blob.download_text();
```

## <a name="how-to-delete-blobs"></a><span data-ttu-id="761cf-163">Postupy: odstranění objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="761cf-163">How to: Delete blobs</span></span>
<span data-ttu-id="761cf-164">Chcete-li odstranit objekt blob, nejdřív získejte odkaz na objekt blob a potom volejte **delete_blob** metoda na něm.</span><span class="sxs-lookup"><span data-stu-id="761cf-164">To delete a blob, first get a blob reference and then call the **delete_blob** method on it.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Delete the blob.
blockBlob.delete_blob();
```

## <a name="next-steps"></a><span data-ttu-id="761cf-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="761cf-165">Next steps</span></span>
<span data-ttu-id="761cf-166">Teď, když jste se naučili základy používání blob storage, postupujte podle následujících odkazech na další informace o službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="761cf-166">Now that you've learned the basics of blob storage, follow these links to learn more about Azure Storage.</span></span>  

* [<span data-ttu-id="761cf-167">Postup používání úložiště Queue z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="761cf-167">How to use Queue Storage from C++</span></span>](storage-c-plus-plus-how-to-use-queues.md)
* [<span data-ttu-id="761cf-168">Postup používání úložiště Table z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="761cf-168">How to use Table Storage from C++</span></span>](storage-c-plus-plus-how-to-use-tables.md)
* [<span data-ttu-id="761cf-169">Seznam prostředků úložiště Azure v jazyce C++</span><span class="sxs-lookup"><span data-stu-id="761cf-169">List Azure Storage Resources in C++</span></span>](storage-c-plus-plus-enumeration.md)
* [<span data-ttu-id="761cf-170">Klientská knihovna pro úložiště pro C++ – referenční informace</span><span class="sxs-lookup"><span data-stu-id="761cf-170">Storage Client Library for C++ Reference</span></span>](http://azure.github.io/azure-storage-cpp)
* [<span data-ttu-id="761cf-171">Dokumentace k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="761cf-171">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="761cf-172">Přenos dat pomocí nástroje příkazového řádku AzCopy</span><span class="sxs-lookup"><span data-stu-id="761cf-172">Transfer data with the AzCopy command-line utility</span></span>](storage-use-azcopy.md)

