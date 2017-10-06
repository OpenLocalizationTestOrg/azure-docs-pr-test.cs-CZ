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
# <a name="how-toouse-blob-storage-from-c"></a><span data-ttu-id="1bb8c-103">Jak toouse úložiště objektů Blob z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="1bb8c-103">How toouse Blob Storage from C++</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="1bb8c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1bb8c-104">Overview</span></span>
<span data-ttu-id="1bb8c-105">Azure Blob storage je služba, která ukládá Nestrukturovaná data v cloudu hello jako objekty nebo objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="1bb8c-106">Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="1bb8c-107">Úložiště objektů blob je také odkazované tooas objektu úložiště.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="1bb8c-108">Tato příručka popisuje, jak hello tooperform běžné scénáře s využitím služby Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-108">This guide will demonstrate how tooperform common scenarios using hello Azure Blob storage service.</span></span> <span data-ttu-id="1bb8c-109">Hello ukázky jsou napsané v jazyce C++ a používají hello [Klientská knihovna pro úložiště Azure pro jazyk C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="1bb8c-109">hello samples are written in C++ and use hello [Azure Storage Client Library for C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span></span> <span data-ttu-id="1bb8c-110">Hello pokryté scénáře zahrnují **odesílání**, **výpis**, **stahování**, a **odstraňování** objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-110">hello scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span>  

> [!NOTE]
> <span data-ttu-id="1bb8c-111">Tato příručka cílí hello Klientská knihovna pro úložiště Azure pro jazyk C++ verze 1.0.0 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-111">This guide targets hello Azure Storage Client Library for C++ version 1.0.0 and above.</span></span> <span data-ttu-id="1bb8c-112">Hello doporučená verze je klientská knihovna pro úložiště 2.2.0, která je k dispozici prostřednictvím [NuGet](http://www.nuget.org/packages/wastorage) nebo [Githubu](https://github.com/Azure/azure-storage-cpp).</span><span class="sxs-lookup"><span data-stu-id="1bb8c-112">hello recommended version is Storage Client Library 2.2.0, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](https://github.com/Azure/azure-storage-cpp).</span></span>
> 
> 

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a><span data-ttu-id="1bb8c-113">Vytvoření aplikace C++</span><span class="sxs-lookup"><span data-stu-id="1bb8c-113">Create a C++ application</span></span>
<span data-ttu-id="1bb8c-114">V této příručce bude používat funkce úložiště, které lze spustit v rámci aplikace C++.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-114">In this guide, you will use storage features which can be run within a C++ application.</span></span>  

<span data-ttu-id="1bb8c-115">toodo tedy budete potřebovat tooinstall hello Klientská knihovna pro úložiště Azure pro jazyk C++ a vytvoření účtu úložiště Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-115">toodo so, you will need tooinstall hello Azure Storage Client Library for C++ and create an Azure storage account in your Azure subscription.</span></span>   

<span data-ttu-id="1bb8c-116">tooinstall hello Klientská knihovna pro úložiště Azure pro jazyk C++, můžete použít následující metody hello:</span><span class="sxs-lookup"><span data-stu-id="1bb8c-116">tooinstall hello Azure Storage Client Library for C++, you can use hello following methods:</span></span>

* <span data-ttu-id="1bb8c-117">**Linux:** postupujte podle pokynů hello uvedenému v hello [Klientská knihovna pro úložiště Azure pro C++ – soubor README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-117">**Linux:** Follow hello instructions given in hello [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>  
* <span data-ttu-id="1bb8c-118">**Windows:** v sadě Visual Studio, klikněte na tlačítko **nástroje > Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-118">**Windows:** In Visual Studio, click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="1bb8c-119">Typ hello následující příkaz do hello [Konzola správce balíčků NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-119">Type hello following command into hello [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>  
  
     <span data-ttu-id="1bb8c-120">Install-Package wastorage</span><span class="sxs-lookup"><span data-stu-id="1bb8c-120">Install-Package wastorage</span></span>

## <a name="configure-your-application-tooaccess-blob-storage"></a><span data-ttu-id="1bb8c-121">Konfigurace vaší aplikace tooaccess úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="1bb8c-121">Configure your application tooaccess Blob Storage</span></span>
<span data-ttu-id="1bb8c-122">Přidáte následující hello obsahovat toohello příkazy na začátek souboru C++ hello místo toouse objektů BLOB tooaccess rozhraní API hello Azure storage:</span><span class="sxs-lookup"><span data-stu-id="1bb8c-122">Add hello following include statements toohello top of hello C++ file where you want toouse hello Azure storage APIs tooaccess blobs:</span></span>  

```cpp
#include <was/storage_account.h>
#include <was/blob.h>
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="1bb8c-123">Instalační program připojovací řetězec úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="1bb8c-123">Setup an Azure storage connection string</span></span>
<span data-ttu-id="1bb8c-124">Klienta Azure storage používá úložiště připojovací řetězec toostore koncových bodů a přihlašovací údaje pro přístup ke službám dat správy.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-124">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="1bb8c-125">Když spustíte v aplikaci klienta, je nutné zadat připojovací řetězec úložiště hello v hello formátu, pomocí hello název účtu a hello úložiště přístupový klíč k úložišti pro účet úložiště hello uvedené v hello [portálu Azure](https://portal.azure.com)pro hello *AccountName* a *AccountKey* hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-125">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello storage access key for hello storage account listed in hello [Azure Portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="1bb8c-126">Informace o účtech úložiště a přístupové klávesy, najdete v části [o účtech úložiště Azure](storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="1bb8c-126">For information on storage accounts and access keys, see [About Azure Storage Accounts](storage-create-storage-account.md).</span></span> <span data-ttu-id="1bb8c-127">Tento příklad ukazuje, jak můžou deklarovat statické pole toohold hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="1bb8c-127">This example shows how you can declare a static field toohold hello connection string:</span></span>  

```cpp
// Define hello connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

<span data-ttu-id="1bb8c-128">tootest aplikace v místním počítači s Windows, můžete použít hello Microsoft Azure [emulátor úložiště](storage-use-emulator.md) který se instaluje s hello [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="1bb8c-128">tootest your application in your local Windows computer, you can use hello Microsoft Azure [storage emulator](storage-use-emulator.md) that is installed with hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="1bb8c-129">emulátor úložiště Hello je nástroj, který simuluje hello objektů Blob, Queue a Table služby v Azure k dispozici na místním vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-129">hello storage emulator is a utility that simulates hello Blob, Queue, and Table services available in Azure on your local development machine.</span></span> <span data-ttu-id="1bb8c-130">Hello následující příklad ukazuje, jak lze deklarovat statické pole toohold hello připojovací řetězec tooyour emulátor místního úložiště:</span><span class="sxs-lookup"><span data-stu-id="1bb8c-130">hello following example shows how you can declare a static field toohold hello connection string tooyour local storage emulator:</span></span>

```cpp
// Define hello connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

<span data-ttu-id="1bb8c-131">emulátor úložiště Azure toostart hello, vyberte hello **spustit** tlačítko nebo klikněte na tlačítko hello **Windows** klíč.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-131">toostart hello Azure storage emulator, Select hello **Start** button or press hello **Windows** key.</span></span> <span data-ttu-id="1bb8c-132">Začněte psát **emulátoru úložiště Azure**a vyberte **emulátor úložiště Microsoft Azure** hello seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-132">Begin typing **Azure Storage Emulator**, and select **Microsoft Azure Storage Emulator** from hello list of applications.</span></span>  

<span data-ttu-id="1bb8c-133">Hello následující ukázky předpokládejme použít jednu z těchto dvou metod tooget hello úložiště připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-133">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>  

## <a name="retrieve-your-connection-string"></a><span data-ttu-id="1bb8c-134">Načtení připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="1bb8c-134">Retrieve your connection string</span></span>
<span data-ttu-id="1bb8c-135">Můžete použít hello **cloud_storage_account** třídy toorepresent informací o vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-135">You can use hello **cloud_storage_account** class toorepresent your Storage Account information.</span></span> <span data-ttu-id="1bb8c-136">tooretrieve účet úložiště informací z připojovacího řetězce úložiště hello, můžete použít hello **analyzovat** metoda.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-136">tooretrieve your storage account information from hello storage connection string, you can use hello **parse** method.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

<span data-ttu-id="1bb8c-137">V dalším kroku získat odkaz na tooa **cloud_blob_client** třídy jako umožňuje tooretrieve objekty, které představují kontejnery a objekty BLOB uložené v rámci hello služby úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-137">Next, get a reference tooa **cloud_blob_client** class as it allows you tooretrieve objects that represent containers and blobs stored within hello Blob Storage Service.</span></span> <span data-ttu-id="1bb8c-138">Hello následující kód vytvoří **cloud_blob_client** objekt, který používá objekt účtu úložiště hello nemůžeme načíst výše:</span><span class="sxs-lookup"><span data-stu-id="1bb8c-138">hello following code creates a **cloud_blob_client** object using hello storage account object we retrieved above:</span></span>  

```cpp
// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  
```

## <a name="how-to-create-a-container"></a><span data-ttu-id="1bb8c-139">Postupy: vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="1bb8c-139">How to: Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="1bb8c-140">Tento příklad ukazuje, jak toocreate kontejner, pokud ještě neexistuje:</span><span class="sxs-lookup"><span data-stu-id="1bb8c-140">This example shows how toocreate a container if it does not already exist:</span></span>  

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

<span data-ttu-id="1bb8c-141">Ve výchozím nastavení je hello nový kontejner privátní a je nutné zadat úložiště objektů BLOB přístup klíče toodownload z tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-141">By default, hello new container is private and you must specify your storage access key toodownload blobs from this container.</span></span> <span data-ttu-id="1bb8c-142">Pokud chcete v rámci hello kontejneru dostupné tooeveryone toomake hello soubory (objektů BLOB), můžete nastavit kontejner toobe hello veřejné pomocí hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="1bb8c-142">If you want toomake hello files (blobs) within hello container available tooeveryone, you can set hello container toobe public using hello following code:</span></span>  

```cpp
// Make hello blob container publicly accessible.
azure::storage::blob_container_permissions permissions;
permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
container.upload_permissions(permissions);  
```

<span data-ttu-id="1bb8c-143">Kdokoli na hello Internetu může vidět objekty BLOB ve veřejném kontejneru, ale můžete upravit nebo odstranit pouze v případě, že máte příslušný přístupový klíč hello.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-143">Anyone on hello Internet can see blobs in a public container, but you can modify or delete them only if you have hello appropriate access key.</span></span>  

## <a name="how-to-upload-a-blob-into-a-container"></a><span data-ttu-id="1bb8c-144">Postupy: nahrát objekt blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="1bb8c-144">How to: Upload a blob into a container</span></span>
<span data-ttu-id="1bb8c-145">Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-145">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="1bb8c-146">Ve většině případů hello je objekt blob bloku hello doporučená toouse typu.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-146">In hello majority of cases, block blob is hello recommended type toouse.</span></span>  

<span data-ttu-id="1bb8c-147">tooupload objekt blob bloku souboru tooa získejte odkaz na kontejner a použít ho tooget odkaz na objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-147">tooupload a file tooa block blob, get a container reference and use it tooget a block blob reference.</span></span> <span data-ttu-id="1bb8c-148">Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud dat tooit pomocí volání hello **upload_from_stream** metoda.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-148">Once you have a blob reference, you can upload any stream of data tooit by calling hello **upload_from_stream** method.</span></span> <span data-ttu-id="1bb8c-149">Tato operace vytvoří objekt blob hello, pokud nebyla dříve neexistuje, nebo ho přepíše, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-149">This operation will create hello blob if it didn't previously exist, or overwrite it if it does exist.</span></span> <span data-ttu-id="1bb8c-150">Následující příklad ukazuje, jak Hello tooupload objekt blob do kontejneru a předpokládá, že hello kontejner byl již vytvořen.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-150">hello following example shows how tooupload a blob into a container and assumes that hello container was already created.</span></span>  

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

<span data-ttu-id="1bb8c-151">Alternativně můžete použít hello **upload_from_file** metoda tooupload tooa soubor – objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-151">Alternatively, you can use hello **upload_from_file** method tooupload a file tooa block blob.</span></span>

## <a name="how-to-list-hello-blobs-in-a-container"></a><span data-ttu-id="1bb8c-152">Postupy: seznam hello objektů BLOB v kontejneru</span><span class="sxs-lookup"><span data-stu-id="1bb8c-152">How to: List hello blobs in a container</span></span>
<span data-ttu-id="1bb8c-153">toolist hello objekty BLOB v kontejneru, nejdřív získejte odkaz na kontejner.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-153">toolist hello blobs in a container, first get a container reference.</span></span> <span data-ttu-id="1bb8c-154">Pak můžete použít hello kontejneru **list_blobs** metoda tooretrieve hello objekty BLOB a/nebo obsažené adresáře.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-154">You can then use hello container's **list_blobs** method tooretrieve hello blobs and/or directories within it.</span></span> <span data-ttu-id="1bb8c-155">tooaccess hello bohatou sadu vlastností a metod vrácené **list_blob_item**, musí volat hello **list_blob_item.as_blob** metoda tooget **cloud_blob** objektu nebo hello **list_blob.as_directory** metoda tooget cloud_blob_directory objektu.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-155">tooaccess hello rich set of properties and methods for a returned **list_blob_item**, you must call hello **list_blob_item.as_blob** method tooget a  **cloud_blob** object, or hello **list_blob.as_directory** method tooget a  cloud_blob_directory object.</span></span> <span data-ttu-id="1bb8c-156">Hello následující kód ukazuje, jak tooretrieve a výstup hello URI pro každou položku v hello **Moje ukázkový kontejner** kontejneru:</span><span class="sxs-lookup"><span data-stu-id="1bb8c-156">hello following code demonstrates how tooretrieve and output hello URI of each item in hello **my-sample-container** container:</span></span>

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

<span data-ttu-id="1bb8c-157">Další podrobnosti týkající se výpisu operací najdete v tématu [seznamu prostředků úložiště Azure v jazyce C++](storage-c-plus-plus-enumeration.md).</span><span class="sxs-lookup"><span data-stu-id="1bb8c-157">For more details on listing operations, see [List Azure Storage Resources in C++](storage-c-plus-plus-enumeration.md).</span></span>

## <a name="how-to-download-blobs"></a><span data-ttu-id="1bb8c-158">Postupy: stáhnout objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="1bb8c-158">How to: Download blobs</span></span>
<span data-ttu-id="1bb8c-159">toodownload objekty BLOB, nejdřív načtěte odkaz na objekt blob a pak zavolají hello **download_to_stream** metoda.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-159">toodownload blobs, first retrieve a blob reference and then call hello **download_to_stream** method.</span></span> <span data-ttu-id="1bb8c-160">Hello následující příklad používá hello **download_to_stream** metoda tootransfer hello blob obsah tooa objektu stream, potom můžete zachovat tooa místního souboru.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-160">hello following example uses hello **download_to_stream** method tootransfer hello blob contents tooa stream object that you can then persist tooa local file.</span></span>  

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

<span data-ttu-id="1bb8c-161">Alternativně můžete použít hello **download_to_file** metoda toodownload hello obsah souboru tooa objektů blob.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-161">Alternatively, you can use hello **download_to_file** method toodownload hello contents of a blob tooa file.</span></span>
<span data-ttu-id="1bb8c-162">Kromě toho můžete také použít hello **download_text** metoda toodownload hello obsah objektu blob jako textový řetězec.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-162">In addition, you can also use hello **download_text** method toodownload hello contents of a blob as a text string.</span></span>  

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

## <a name="how-to-delete-blobs"></a><span data-ttu-id="1bb8c-163">Postupy: odstranění objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="1bb8c-163">How to: Delete blobs</span></span>
<span data-ttu-id="1bb8c-164">toodelete objekt blob, nejdřív získejte odkaz na objekt blob a pak zavolají hello **delete_blob** metoda na něm.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-164">toodelete a blob, first get a blob reference and then call hello **delete_blob** method on it.</span></span>  

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

## <a name="next-steps"></a><span data-ttu-id="1bb8c-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1bb8c-165">Next steps</span></span>
<span data-ttu-id="1bb8c-166">Teď, když jste se naučili základy používání blob storage hello, postupujte podle těchto odkazů toolearn Další informace o Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="1bb8c-166">Now that you've learned hello basics of blob storage, follow these links toolearn more about Azure Storage.</span></span>  

* [<span data-ttu-id="1bb8c-167">Jak toouse Queue Storage z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="1bb8c-167">How toouse Queue Storage from C++</span></span>](storage-c-plus-plus-how-to-use-queues.md)
* [<span data-ttu-id="1bb8c-168">Jak toouse úložiště Table z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="1bb8c-168">How toouse Table Storage from C++</span></span>](storage-c-plus-plus-how-to-use-tables.md)
* [<span data-ttu-id="1bb8c-169">Seznam prostředků úložiště Azure v jazyce C++</span><span class="sxs-lookup"><span data-stu-id="1bb8c-169">List Azure Storage Resources in C++</span></span>](storage-c-plus-plus-enumeration.md)
* [<span data-ttu-id="1bb8c-170">Klientská knihovna pro úložiště pro C++ – referenční informace</span><span class="sxs-lookup"><span data-stu-id="1bb8c-170">Storage Client Library for C++ Reference</span></span>](http://azure.github.io/azure-storage-cpp)
* [<span data-ttu-id="1bb8c-171">Dokumentace k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="1bb8c-171">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="1bb8c-172">Přenos dat pomocí hello příkazového řádku azcopy</span><span class="sxs-lookup"><span data-stu-id="1bb8c-172">Transfer data with hello AzCopy command-line utility</span></span>](storage-use-azcopy.md)

