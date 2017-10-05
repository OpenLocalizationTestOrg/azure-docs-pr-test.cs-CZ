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
ms.openlocfilehash: a1a37266908277b54e7b42d85b9b4873af77e622
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="develop-for-azure-file-storage-with-python"></a><span data-ttu-id="959ac-103">Vývoj pro Azure File storage s Pythonem</span><span class="sxs-lookup"><span data-stu-id="959ac-103">Develop for Azure File storage with Python</span></span>
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="959ac-104">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="959ac-104">About this tutorial</span></span>
<span data-ttu-id="959ac-105">V tomto kurzu se ukazují základy používání Python k vývoji aplikací nebo služeb, které používají Azure File storage k ukládání dat souborů.</span><span class="sxs-lookup"><span data-stu-id="959ac-105">This tutorial will demonstrate the basics of using Python to develop applications or services that use Azure File storage to store file data.</span></span> <span data-ttu-id="959ac-106">V tomto kurzu jsme vytvořit jednoduché konzolové aplikace a ukazují, jak provést základní operace s Python a Azure File storage:</span><span class="sxs-lookup"><span data-stu-id="959ac-106">In this tutorial, we will create a simple console application and show how to perform basic actions with Python and Azure File storage:</span></span>

* <span data-ttu-id="959ac-107">Vytvoření sdílené složky Azure File</span><span class="sxs-lookup"><span data-stu-id="959ac-107">Create Azure File shares</span></span>
* <span data-ttu-id="959ac-108">Vytváření adresářů</span><span class="sxs-lookup"><span data-stu-id="959ac-108">Create directories</span></span>
* <span data-ttu-id="959ac-109">Vytvoření výčtu souborů a adresářů v Azure File sdílet</span><span class="sxs-lookup"><span data-stu-id="959ac-109">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="959ac-110">Odesílání, stahování a odstranění souboru</span><span class="sxs-lookup"><span data-stu-id="959ac-110">Upload, download, and delete a file</span></span>

> [!Note]  
> <span data-ttu-id="959ac-111">Protože Azure File storage můžete získat přístup přes protokol SMB, je možné psát jednoduché aplikace, které přístup k Azure souborové složce přes standardní Python vstupně-výstupních operací třídy a funkce.</span><span class="sxs-lookup"><span data-stu-id="959ac-111">Because Azure File storage may be accessed over SMB, it is possible to write simple applications that access the Azure File share using the standard Python I/O classes and functions.</span></span> <span data-ttu-id="959ac-112">Tento článek popisuje, jak k psaní aplikací, které používají Azure Python SDK úložiště, který používá [REST API služby Azure File storage](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) ke komunikaci s úložištěm Azure File.</span><span class="sxs-lookup"><span data-stu-id="959ac-112">This article will describe how to write applications that use the Azure Storage Python SDK, which uses the [Azure File storage REST API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) to talk to Azure File storage.</span></span>

### <a name="set-up-your-application-to-use-azure-file-storage"></a><span data-ttu-id="959ac-113">Nastavení aplikace k používání Azure File storage</span><span class="sxs-lookup"><span data-stu-id="959ac-113">Set up your application to use Azure File storage</span></span>
<span data-ttu-id="959ac-114">Přidejte následující v horní části všech Python zdrojový soubor, ve kterém chcete k programovému přístupu ke službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="959ac-114">Add the following near the top of any Python source file in which you wish to programmatically access Azure Storage.</span></span>

```python
from azure.storage.file import FileService
```

### <a name="set-up-a-connection-to-azure-file-storage"></a><span data-ttu-id="959ac-115">Nastavit připojení k Azure File storage</span><span class="sxs-lookup"><span data-stu-id="959ac-115">Set up a connection to Azure File storage</span></span> 
<span data-ttu-id="959ac-116">`FileService` Objekt vám umožňuje spolupracovat s sdílených složek, adresářů a souborů.</span><span class="sxs-lookup"><span data-stu-id="959ac-116">The `FileService` object lets you work with shares, directories and files.</span></span> <span data-ttu-id="959ac-117">Následující kód vytvoří `FileService` pomocí klíč účet a název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="959ac-117">The following code creates a `FileService` object using the storage account name and account key.</span></span> <span data-ttu-id="959ac-118">Nahraďte `<myaccount>` a `<mykey>` s názvem účtu a klíč.</span><span class="sxs-lookup"><span data-stu-id="959ac-118">Replace `<myaccount>` and `<mykey>` with your account name and key.</span></span>

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

### <a name="create-an-azure-file-share"></a><span data-ttu-id="959ac-119">Vytvoření Azure sdílené složky</span><span class="sxs-lookup"><span data-stu-id="959ac-119">Create an Azure File share</span></span>
<span data-ttu-id="959ac-120">V následujícím příkladu kódu, můžete použít `FileService` objekt, který chcete vytvořit sdílenou složku, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="959ac-120">In the following code example, you can use a `FileService` object to create the share if it doesn't exist.</span></span>

```python
file_service.create_share('myshare')
```

### <a name="create-a-directory"></a><span data-ttu-id="959ac-121">Vytvoření adresáře</span><span class="sxs-lookup"><span data-stu-id="959ac-121">Create a directory</span></span>
<span data-ttu-id="959ac-122">Úložiště můžete navíc uspořádat umístěním souborů v podadresářích místo nutnosti všechny z nich v kořenovém adresáři.</span><span class="sxs-lookup"><span data-stu-id="959ac-122">You can also organize storage by putting files inside sub-directories instead of having all of them in the root directory.</span></span> <span data-ttu-id="959ac-123">Azure File storage můžete vytvořit tolik adresáře, které umožní váš účet.</span><span class="sxs-lookup"><span data-stu-id="959ac-123">Azure File storage allows you to create as many directories as your account will allow.</span></span> <span data-ttu-id="959ac-124">Následující kód vytvoří adresář s názvem **sampledir** pod kořenovým adresářem.</span><span class="sxs-lookup"><span data-stu-id="959ac-124">The code below will create a sub-directory named **sampledir** under the root directory.</span></span>

```python
file_service.create_directory('myshare', 'sampledir')
```

### <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="959ac-125">Vytvoření výčtu souborů a adresářů v Azure File sdílet</span><span class="sxs-lookup"><span data-stu-id="959ac-125">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="959ac-126">K zobrazení seznamu souborů a adresářů ve sdílené složce, použijte **seznamu\_adresáře\_a\_soubory** metoda.</span><span class="sxs-lookup"><span data-stu-id="959ac-126">To list the files and directories in a share, use the **list\_directories\_and\_files** method.</span></span> <span data-ttu-id="959ac-127">Tato metoda vrátí generátor.</span><span class="sxs-lookup"><span data-stu-id="959ac-127">This method returns a generator.</span></span> <span data-ttu-id="959ac-128">Následující kód výstupy **název** každého souboru a adresáře ve sdílené složce, do konzoly.</span><span class="sxs-lookup"><span data-stu-id="959ac-128">The following code outputs the **name** of each file and directory in a share to the console.</span></span>

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

### <a name="upload-a-file"></a><span data-ttu-id="959ac-129">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="959ac-129">Upload a file</span></span> 
<span data-ttu-id="959ac-130">Azure soubor, který obsahuje sdílenou složku v každém, kořenový adresář, kde mohou být uloženy soubory.</span><span class="sxs-lookup"><span data-stu-id="959ac-130">Azure File share contains at the very least, a root directory where files can reside.</span></span> <span data-ttu-id="959ac-131">V této části se dozvíte jak nahrát soubor z místního úložiště do kořenového adresáře sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="959ac-131">In this section, you'll learn how to upload a file from local storage onto the root directory of a share.</span></span>

<span data-ttu-id="959ac-132">Chcete-li vytvořit soubor a odesílat data, použijte `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` nebo `create_file_from_text` metody.</span><span class="sxs-lookup"><span data-stu-id="959ac-132">To create a file and upload data, use the `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` or `create_file_from_text` methods.</span></span> <span data-ttu-id="959ac-133">Jsou nejdůležitější metody, které provádějí potřebné rozdělování, když velikost dat přesáhne 64 MB.</span><span class="sxs-lookup"><span data-stu-id="959ac-133">They are high-level methods that perform the necessary chunking when the size of the data exceeds 64 MB.</span></span>

<span data-ttu-id="959ac-134">`create_file_from_path`Odešle obsah souboru ze zadané cesty a `create_file_from_stream` odešle obsah z již otevřeného souboru/stream.</span><span class="sxs-lookup"><span data-stu-id="959ac-134">`create_file_from_path` uploads the contents of a file from the specified path, and `create_file_from_stream` uploads the contents from an already opened file/stream.</span></span> <span data-ttu-id="959ac-135">`create_file_from_bytes`odešle pole bajtů, a `create_file_from_text` odešle zadanou textovou hodnotu pomocí zadaného kódování (výchozí hodnota je UTF-8).</span><span class="sxs-lookup"><span data-stu-id="959ac-135">`create_file_from_bytes` uploads an array of bytes, and `create_file_from_text` uploads the specified text value using the specified encoding (defaults to UTF-8).</span></span>

<span data-ttu-id="959ac-136">V následujícím příkladu se uloží obsah **sunset.png** soubor do **myfile** souboru.</span><span class="sxs-lookup"><span data-stu-id="959ac-136">The following example uploads the contents of the **sunset.png** file into the **myfile** file.</span></span>

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want to create this blob in the root directory, so we specify None for the directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

### <a name="download-a-file"></a><span data-ttu-id="959ac-137">Stažení souboru</span><span class="sxs-lookup"><span data-stu-id="959ac-137">Download a file</span></span>
<span data-ttu-id="959ac-138">Chcete-li stáhnout data ze souboru, použijte `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, nebo `get_file_to_text`.</span><span class="sxs-lookup"><span data-stu-id="959ac-138">To download data from a file, use `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, or `get_file_to_text`.</span></span> <span data-ttu-id="959ac-139">Jsou nejdůležitější metody, které provádějí potřebné rozdělování, když velikost dat přesáhne 64 MB.</span><span class="sxs-lookup"><span data-stu-id="959ac-139">They are high-level methods that perform the necessary chunking when the size of the data exceeds 64 MB.</span></span>

<span data-ttu-id="959ac-140">Následující příklad ukazuje, jak pomocí `get_file_to_path` stáhnout obsah **myfile** souboru a uložte ho do **na více systémů sunset.png** souboru.</span><span class="sxs-lookup"><span data-stu-id="959ac-140">The following example demonstrates using `get_file_to_path` to download the contents of the **myfile** file and store it to the **out-sunset.png** file.</span></span>

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

### <a name="delete-a-file"></a><span data-ttu-id="959ac-141">Odstranění souboru</span><span class="sxs-lookup"><span data-stu-id="959ac-141">Delete a file</span></span>
<span data-ttu-id="959ac-142">Nakonec odstranění souboru, zavolejte `delete_file`.</span><span class="sxs-lookup"><span data-stu-id="959ac-142">Finally, to delete a file, call `delete_file`.</span></span>

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a><span data-ttu-id="959ac-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="959ac-143">Next steps</span></span>
<span data-ttu-id="959ac-144">Teď, když jste se naučili jak pracovat s Azure File storage s Python, postupujte podle následujících odkazech na další informace.</span><span class="sxs-lookup"><span data-stu-id="959ac-144">Now that you've learned how to manipulate Azure File storage with Python, follow these links to learn more.</span></span>

* [<span data-ttu-id="959ac-145">Středisko pro vývojáře programující v Pythonu</span><span class="sxs-lookup"><span data-stu-id="959ac-145">Python Developer Center</span></span>](/develop/python/)
* [<span data-ttu-id="959ac-146">REST API služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="959ac-146">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* [<span data-ttu-id="959ac-147">Úložiště Microsoft Azure SDK pro jazyk Python</span><span class="sxs-lookup"><span data-stu-id="959ac-147">Microsoft Azure Storage SDK for Python</span></span>](https://github.com/Azure/azure-storage-python)