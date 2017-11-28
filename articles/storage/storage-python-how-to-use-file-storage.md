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
# <a name="develop-for-azure-file-storage-with-python"></a><span data-ttu-id="a42d3-103">Vývoj pro Azure File storage s Pythonem</span><span class="sxs-lookup"><span data-stu-id="a42d3-103">Develop for Azure File storage with Python</span></span>
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="a42d3-104">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="a42d3-104">About this tutorial</span></span>
<span data-ttu-id="a42d3-105">V tomto kurzu se ukazují hello základy používání Python toodevelop aplikacím nebo službám, které používají Azure File storage toostore souborová data.</span><span class="sxs-lookup"><span data-stu-id="a42d3-105">This tutorial will demonstrate hello basics of using Python toodevelop applications or services that use Azure File storage toostore file data.</span></span> <span data-ttu-id="a42d3-106">V tomto kurzu bude vytvoření jednoduché konzolové aplikace a zobrazit jak tooperform základní operace s Python a Azure File storage:</span><span class="sxs-lookup"><span data-stu-id="a42d3-106">In this tutorial, we will create a simple console application and show how tooperform basic actions with Python and Azure File storage:</span></span>

* <span data-ttu-id="a42d3-107">Vytvoření sdílené složky Azure File</span><span class="sxs-lookup"><span data-stu-id="a42d3-107">Create Azure File shares</span></span>
* <span data-ttu-id="a42d3-108">Vytváření adresářů</span><span class="sxs-lookup"><span data-stu-id="a42d3-108">Create directories</span></span>
* <span data-ttu-id="a42d3-109">Vytvoření výčtu souborů a adresářů v Azure File sdílet</span><span class="sxs-lookup"><span data-stu-id="a42d3-109">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="a42d3-110">Odesílání, stahování a odstranění souboru</span><span class="sxs-lookup"><span data-stu-id="a42d3-110">Upload, download, and delete a file</span></span>

> [!Note]  
> <span data-ttu-id="a42d3-111">Protože Azure File storage můžete získat přístup přes protokol SMB, je možné toowrite jednoduché aplikace, které přístup hello Azure sdílenou složku pomocí hello standardní Python vstupně-výstupních operací třídy a funkce.</span><span class="sxs-lookup"><span data-stu-id="a42d3-111">Because Azure File storage may be accessed over SMB, it is possible toowrite simple applications that access hello Azure File share using hello standard Python I/O classes and functions.</span></span> <span data-ttu-id="a42d3-112">Tento článek popisuje, jak toowrite aplikace, které používají hello Python SDK úložiště Azure, který používá hello [REST API služby Azure File storage](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure úložiště File.</span><span class="sxs-lookup"><span data-stu-id="a42d3-112">This article will describe how toowrite applications that use hello Azure Storage Python SDK, which uses hello [Azure File storage REST API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span></span>

### <a name="set-up-your-application-toouse-azure-file-storage"></a><span data-ttu-id="a42d3-113">Nastavit toouse vaše aplikace Azure File storage</span><span class="sxs-lookup"><span data-stu-id="a42d3-113">Set up your application toouse Azure File storage</span></span>
<span data-ttu-id="a42d3-114">Přidejte následující hello v hello horní části všech Python zdrojový soubor, ve kterém chcete tooprogrammatically přístupu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a42d3-114">Add hello following near hello top of any Python source file in which you wish tooprogrammatically access Azure Storage.</span></span>

```python
from azure.storage.file import FileService
```

### <a name="set-up-a-connection-tooazure-file-storage"></a><span data-ttu-id="a42d3-115">Nastavení připojení tooAzure úložiště File</span><span class="sxs-lookup"><span data-stu-id="a42d3-115">Set up a connection tooAzure File storage</span></span> 
<span data-ttu-id="a42d3-116">Hello `FileService` objekt vám umožňuje spolupracovat s sdílených složek, adresářů a souborů.</span><span class="sxs-lookup"><span data-stu-id="a42d3-116">hello `FileService` object lets you work with shares, directories and files.</span></span> <span data-ttu-id="a42d3-117">Hello následující kód vytvoří `FileService` objekt, který používá hello klíč účtu úložiště účet a název.</span><span class="sxs-lookup"><span data-stu-id="a42d3-117">hello following code creates a `FileService` object using hello storage account name and account key.</span></span> <span data-ttu-id="a42d3-118">Nahraďte `<myaccount>` a `<mykey>` s názvem účtu a klíč.</span><span class="sxs-lookup"><span data-stu-id="a42d3-118">Replace `<myaccount>` and `<mykey>` with your account name and key.</span></span>

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

### <a name="create-an-azure-file-share"></a><span data-ttu-id="a42d3-119">Vytvoření Azure sdílené složky</span><span class="sxs-lookup"><span data-stu-id="a42d3-119">Create an Azure File share</span></span>
<span data-ttu-id="a42d3-120">V hello následující ukázka kódu, můžete použít `FileService` objektu toocreate hello sdílené složky pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="a42d3-120">In hello following code example, you can use a `FileService` object toocreate hello share if it doesn't exist.</span></span>

```python
file_service.create_share('myshare')
```

### <a name="create-a-directory"></a><span data-ttu-id="a42d3-121">Vytvoření adresáře</span><span class="sxs-lookup"><span data-stu-id="a42d3-121">Create a directory</span></span>
<span data-ttu-id="a42d3-122">Úložiště můžete navíc uspořádat umístěním souborů v podadresářích místo nutnosti všechny z nich v kořenovém adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="a42d3-122">You can also organize storage by putting files inside sub-directories instead of having all of them in hello root directory.</span></span> <span data-ttu-id="a42d3-123">Úložiště Azure File umožňuje toocreate jako víc adresářů jako váš účet se povolit.</span><span class="sxs-lookup"><span data-stu-id="a42d3-123">Azure File storage allows you toocreate as many directories as your account will allow.</span></span> <span data-ttu-id="a42d3-124">Následující kód Hello vytvoří adresář s názvem **sampledir** pod kořenovým adresářem hello.</span><span class="sxs-lookup"><span data-stu-id="a42d3-124">hello code below will create a sub-directory named **sampledir** under hello root directory.</span></span>

```python
file_service.create_directory('myshare', 'sampledir')
```

### <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="a42d3-125">Vytvoření výčtu souborů a adresářů v Azure File sdílet</span><span class="sxs-lookup"><span data-stu-id="a42d3-125">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="a42d3-126">toolist hello souborů a adresářů ve sdílené složce, použít hello **seznamu\_adresáře\_a\_soubory** metoda.</span><span class="sxs-lookup"><span data-stu-id="a42d3-126">toolist hello files and directories in a share, use hello **list\_directories\_and\_files** method.</span></span> <span data-ttu-id="a42d3-127">Tato metoda vrátí generátor.</span><span class="sxs-lookup"><span data-stu-id="a42d3-127">This method returns a generator.</span></span> <span data-ttu-id="a42d3-128">Hello následující kód výstupy hello **název** z jednotlivých souborů a adresářů v konzole toohello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="a42d3-128">hello following code outputs hello **name** of each file and directory in a share toohello console.</span></span>

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

### <a name="upload-a-file"></a><span data-ttu-id="a42d3-129">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="a42d3-129">Upload a file</span></span> 
<span data-ttu-id="a42d3-130">Azure soubor, který obsahuje sdílenou složku v hello velmi alespoň, kořenový adresář, kde mohou být uloženy soubory.</span><span class="sxs-lookup"><span data-stu-id="a42d3-130">Azure File share contains at hello very least, a root directory where files can reside.</span></span> <span data-ttu-id="a42d3-131">V této části se dozvíte, jak tooupload soubor z místního úložiště do hello kořenový adresář sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="a42d3-131">In this section, you'll learn how tooupload a file from local storage onto hello root directory of a share.</span></span>

<span data-ttu-id="a42d3-132">toocreate soubor a nahrávání dat, použijte hello `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` nebo `create_file_from_text` metody.</span><span class="sxs-lookup"><span data-stu-id="a42d3-132">toocreate a file and upload data, use hello `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` or `create_file_from_text` methods.</span></span> <span data-ttu-id="a42d3-133">Jsou nejdůležitější metody, které provádějí hello nezbytné rozdělování když hello velikost dat hello překročí 64 MB.</span><span class="sxs-lookup"><span data-stu-id="a42d3-133">They are high-level methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="a42d3-134">`create_file_from_path`nahrávání hello obsah souboru z hello zadané cesty, a `create_file_from_stream` nahrávání hello obsah z již otevřeného souboru/stream.</span><span class="sxs-lookup"><span data-stu-id="a42d3-134">`create_file_from_path` uploads hello contents of a file from hello specified path, and `create_file_from_stream` uploads hello contents from an already opened file/stream.</span></span> <span data-ttu-id="a42d3-135">`create_file_from_bytes`odešle pole bajtů, a `create_file_from_text` nahrávání hello zadán textové hodnoty pomocí hello kódování (výchozí nastavení tooUTF-8).</span><span class="sxs-lookup"><span data-stu-id="a42d3-135">`create_file_from_bytes` uploads an array of bytes, and `create_file_from_text` uploads hello specified text value using hello specified encoding (defaults tooUTF-8).</span></span>

<span data-ttu-id="a42d3-136">Hello následujícím příkladu se uloží obsah hello hello **sunset.png** souboru do hello **myfile** souboru.</span><span class="sxs-lookup"><span data-stu-id="a42d3-136">hello following example uploads hello contents of hello **sunset.png** file into hello **myfile** file.</span></span>

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want toocreate this blob in hello root directory, so we specify None for hello directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

### <a name="download-a-file"></a><span data-ttu-id="a42d3-137">Stažení souboru</span><span class="sxs-lookup"><span data-stu-id="a42d3-137">Download a file</span></span>
<span data-ttu-id="a42d3-138">toodownload data ze souboru, použijte `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, nebo `get_file_to_text`.</span><span class="sxs-lookup"><span data-stu-id="a42d3-138">toodownload data from a file, use `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, or `get_file_to_text`.</span></span> <span data-ttu-id="a42d3-139">Jsou nejdůležitější metody, které provádějí hello nezbytné rozdělování když hello velikost dat hello překročí 64 MB.</span><span class="sxs-lookup"><span data-stu-id="a42d3-139">They are high-level methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="a42d3-140">Hello následující příklad ukazuje použití `get_file_to_path` toodownload hello obsah hello **myfile** souboru a uložte ho toohello **na více systémů sunset.png** souboru.</span><span class="sxs-lookup"><span data-stu-id="a42d3-140">hello following example demonstrates using `get_file_to_path` toodownload hello contents of hello **myfile** file and store it toohello **out-sunset.png** file.</span></span>

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

### <a name="delete-a-file"></a><span data-ttu-id="a42d3-141">Odstranění souboru</span><span class="sxs-lookup"><span data-stu-id="a42d3-141">Delete a file</span></span>
<span data-ttu-id="a42d3-142">Nakonec toodelete soubor, zavolejte `delete_file`.</span><span class="sxs-lookup"><span data-stu-id="a42d3-142">Finally, toodelete a file, call `delete_file`.</span></span>

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a><span data-ttu-id="a42d3-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a42d3-143">Next steps</span></span>
<span data-ttu-id="a42d3-144">Teď, když jste se naučili jak toomanipulate Azure File storage s Python, použijte tyto odkazy toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="a42d3-144">Now that you've learned how toomanipulate Azure File storage with Python, follow these links toolearn more.</span></span>

* [<span data-ttu-id="a42d3-145">Středisko pro vývojáře programující v Pythonu</span><span class="sxs-lookup"><span data-stu-id="a42d3-145">Python Developer Center</span></span>](/develop/python/)
* [<span data-ttu-id="a42d3-146">REST API služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="a42d3-146">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* [<span data-ttu-id="a42d3-147">Úložiště Microsoft Azure SDK pro jazyk Python</span><span class="sxs-lookup"><span data-stu-id="a42d3-147">Microsoft Azure Storage SDK for Python</span></span>](https://github.com/Azure/azure-storage-python)