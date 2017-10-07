---
title: aaaDevelop pro Azure File storage s Javou | Microsoft Docs
description: "Zjistěte, jak toodevelop Java aplikace a služby, které používají Azure File storage toostore souborová data."
services: storage
documentationcenter: java
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 3bfbfa7f-d378-4fb4-8df3-e0b6fcea5b27
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 05/27/2017
ms.author: robinsh
ms.openlocfilehash: b50703815daf2c829e7e9a9a4196c31a2b8727e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-java"></a><span data-ttu-id="23102-103">Vývoj pro Azure File storage s Javou</span><span class="sxs-lookup"><span data-stu-id="23102-103">Develop for Azure File storage with Java</span></span>
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="23102-104">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="23102-104">About this tutorial</span></span>
<span data-ttu-id="23102-105">V tomto kurzu se ukazují hello základy používání Java toodevelop aplikacím nebo službám, které používají Azure File storage toostore souborová data.</span><span class="sxs-lookup"><span data-stu-id="23102-105">This tutorial will demonstrate hello basics of using Java toodevelop applications or services that use Azure File storage toostore file data.</span></span> <span data-ttu-id="23102-106">V tomto kurzu bude vytvoření jednoduché konzolové aplikace a zobrazit jak tooperform základní operace s Java a Azure File storage:</span><span class="sxs-lookup"><span data-stu-id="23102-106">In this tutorial, we will create a simple console application and show how tooperform basic actions with Java and Azure File storage:</span></span>

* <span data-ttu-id="23102-107">Vytvářet a odstraňovat sdílené složky Azure File</span><span class="sxs-lookup"><span data-stu-id="23102-107">Create and delete Azure File shares</span></span>
* <span data-ttu-id="23102-108">Vytvářet a odstraňovat adresáře</span><span class="sxs-lookup"><span data-stu-id="23102-108">Create and delete directories</span></span>
* <span data-ttu-id="23102-109">Vytvoření výčtu souborů a adresářů v Azure File sdílet</span><span class="sxs-lookup"><span data-stu-id="23102-109">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="23102-110">Odesílání, stahování a odstranění souboru</span><span class="sxs-lookup"><span data-stu-id="23102-110">Upload, download, and delete a file</span></span>

> [!Note]  
> <span data-ttu-id="23102-111">Protože Azure File storage můžete získat přístup přes protokol SMB, je možné toowrite jednoduché aplikace, které přístup hello Azure souborové složce přes standardní třídy Java vstupně-výstupních operací hello.</span><span class="sxs-lookup"><span data-stu-id="23102-111">Because Azure File storage may be accessed over SMB, it is possible toowrite simple applications that access hello Azure File share using hello standard Java I/O classes.</span></span> <span data-ttu-id="23102-112">Tento článek popisuje, jak toowrite aplikace, které používají hello Java SDK úložiště Azure, kterou používá hello [REST API služby Azure File storage](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure úložiště File.</span><span class="sxs-lookup"><span data-stu-id="23102-112">This article will describe how toowrite applications that use hello Azure Storage Java SDK, which uses hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span></span>

## <a name="create-a-java-application"></a><span data-ttu-id="23102-113">Vytvoření aplikace Java</span><span class="sxs-lookup"><span data-stu-id="23102-113">Create a Java application</span></span>
<span data-ttu-id="23102-114">Ukázky hello toobuild, budete potřebovat hello Java Development Kit (JDK) a [hello [Azure SDK úložiště pro jazyk Java]].</span><span class="sxs-lookup"><span data-stu-id="23102-114">toobuild hello samples, you will need hello Java Development Kit (JDK) and hello [Azure Storage SDK for Java][].</span></span> <span data-ttu-id="23102-115">Musí také jste vytvořili účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="23102-115">You should also have created an Azure storage account.</span></span>

## <a name="setup-your-application-toouse-azure-file-storage"></a><span data-ttu-id="23102-116">Instalační program toouse vaše aplikace Azure File storage</span><span class="sxs-lookup"><span data-stu-id="23102-116">Setup your application toouse Azure File storage</span></span>
<span data-ttu-id="23102-117">toouse hello úložiště Azure rozhraní API, přidejte následující příkaz toohello horní části souboru Java hello, kde chcete tooaccess hello úložiště služby z hello.</span><span class="sxs-lookup"><span data-stu-id="23102-117">toouse hello Azure storage APIs, add hello following statement toohello top of hello Java file where you intend tooaccess hello storage service from.</span></span>

```java
// Include hello following imports toouse blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="23102-118">Instalační program připojovací řetězec úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="23102-118">Setup an Azure storage connection string</span></span>
<span data-ttu-id="23102-119">toouse Azure File storage, budete potřebovat účet úložiště Azure tooyour tooconnect.</span><span class="sxs-lookup"><span data-stu-id="23102-119">toouse Azure File storage, you need tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="23102-120">Hello prvním krokem by tooconfigure připojovací řetězec, který použijeme účet úložiště tooyour tooconnect.</span><span class="sxs-lookup"><span data-stu-id="23102-120">hello first step would be tooconfigure a connection string which we'll use tooconnect tooyour storage account.</span></span> <span data-ttu-id="23102-121">Umožňuje definovat statické proměnné toodo který.</span><span class="sxs-lookup"><span data-stu-id="23102-121">Let's define a static variable toodo that.</span></span>

```java
// Configure hello connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> <span data-ttu-id="23102-122">Nahraďte your_storage_account_name a your_storage_account_key hello skutečnými hodnotami pro váš účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="23102-122">Replace your_storage_account_name and your_storage_account_key with hello actual values for your storage account.</span></span>
> 
> 

## <a name="connecting-tooan-azure-storage-account"></a><span data-ttu-id="23102-123">Připojení tooan účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="23102-123">Connecting tooan Azure storage account</span></span>
<span data-ttu-id="23102-124">účet úložiště tooyour tooconnect, budete potřebovat toouse hello **CloudStorageAccount** objekt, předávání připojovací řetězec tooits **analyzovat** metoda.</span><span class="sxs-lookup"><span data-stu-id="23102-124">tooconnect tooyour storage account, you need toouse hello **CloudStorageAccount** object, passing a connection string tooits **parse** method.</span></span>

```java
// Use hello CloudStorageAccount object tooconnect tooyour storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle hello exception
}
```

<span data-ttu-id="23102-125">**CloudStorageAccount.parse** vyvolává InvalidKeyException, budete potřebovat tooput ho uvnitř try/catch blokovat.</span><span class="sxs-lookup"><span data-stu-id="23102-125">**CloudStorageAccount.parse** throws an InvalidKeyException so you'll need tooput it inside a try/catch block.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="23102-126">Vytvoření Azure sdílené složky</span><span class="sxs-lookup"><span data-stu-id="23102-126">Create an Azure File share</span></span>
<span data-ttu-id="23102-127">Všechny soubory a adresáře v úložišti Azure File jsou umístěny v kontejneru názvem **sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="23102-127">All files and directories in Azure File storage reside in a container called a **Share**.</span></span> <span data-ttu-id="23102-128">Váš účet úložiště může mít tolik sdílených složek jako umožňuje kapacitě vašeho účtu.</span><span class="sxs-lookup"><span data-stu-id="23102-128">Your storage account can have as much shares as your account capacity allows.</span></span> <span data-ttu-id="23102-129">tooobtain přístup tooa sdílenou složku a její obsah, je nutné toouse klienta úložiště Azure File.</span><span class="sxs-lookup"><span data-stu-id="23102-129">tooobtain access tooa share and its contents, you need toouse a Azure File storage client.</span></span>

```java
// Create hello Azure File storage client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

<span data-ttu-id="23102-130">Pomocí klienta aplikace hello Azure File storage můžete získat, sdílenou složku tooa odkaz.</span><span class="sxs-lookup"><span data-stu-id="23102-130">Using hello Azure File storage client, you can then obtain a reference tooa share.</span></span>

```java
// Get a reference toohello file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

<span data-ttu-id="23102-131">Vytvořte sdílenou složku hello tooactually, použijte hello **createIfNotExists** metodu objektu CloudFileShare hello.</span><span class="sxs-lookup"><span data-stu-id="23102-131">tooactually create hello share, use hello **createIfNotExists** method of hello CloudFileShare object.</span></span>

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

<span data-ttu-id="23102-132">V tomto okamžiku **sdílet** obsahuje odkaz na tooa sdílenou složku s názvem **sampleshare**.</span><span class="sxs-lookup"><span data-stu-id="23102-132">At this point, **share** holds a reference tooa share named **sampleshare**.</span></span>

## <a name="delete-an-azure-file-share"></a><span data-ttu-id="23102-133">Odstranit sdílenou složku Azure</span><span class="sxs-lookup"><span data-stu-id="23102-133">Delete an Azure File share</span></span>
<span data-ttu-id="23102-134">Odstranění sdílené složky se provádí volání hello **deleteIfExists** metoda CloudFileShare objektu.</span><span class="sxs-lookup"><span data-stu-id="23102-134">Deleting a share is done by calling hello **deleteIfExists** method on a CloudFileShare object.</span></span> <span data-ttu-id="23102-135">Tady je ukázkový kód, který nemá.</span><span class="sxs-lookup"><span data-stu-id="23102-135">Here's sample code that does that.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello file client.
   CloudFileClient fileClient = storageAccount.createCloudFileClient();

   // Get a reference toohello file share
   CloudFileShare share = fileClient.getShareReference("sampleshare");

   if (share.deleteIfExists()) {
       System.out.println("sampleshare deleted");
   }
} catch (Exception e) {
    e.printStackTrace();
}
```

## <a name="create-a-directory"></a><span data-ttu-id="23102-136">Vytvoření adresáře</span><span class="sxs-lookup"><span data-stu-id="23102-136">Create a directory</span></span>
<span data-ttu-id="23102-137">Úložiště můžete navíc uspořádat umístěním souborů v podadresářích místo nutnosti všechny z nich v kořenovém adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="23102-137">You can also organize storage by putting files inside sub-directories instead of having all of them in hello root directory.</span></span> <span data-ttu-id="23102-138">Úložiště Azure File umožňuje toocreate jako mnoho adresářů jako váš účet se povolit.</span><span class="sxs-lookup"><span data-stu-id="23102-138">Azure File storage allows you toocreate as much directories as your account will allow.</span></span> <span data-ttu-id="23102-139">Následující kód Hello vytvoří adresář s názvem **sampledir** pod kořenovým adresářem hello.</span><span class="sxs-lookup"><span data-stu-id="23102-139">hello code below will create a sub-directory named **sampledir** under hello root directory.</span></span>

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference toohello sampledir directory
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

if (sampleDir.createIfNotExists()) {
    System.out.println("sampledir created");
} else {
    System.out.println("sampledir already exists");
}
```

## <a name="delete-a-directory"></a><span data-ttu-id="23102-140">Odstranění adresáře</span><span class="sxs-lookup"><span data-stu-id="23102-140">Delete a directory</span></span>
<span data-ttu-id="23102-141">Odstranění adresáře je docela jednoduché úlohy, i když je potřeba poznamenat, že nelze odstranit adresář, který ještě obsahuje soubory nebo ostatních adresářů.</span><span class="sxs-lookup"><span data-stu-id="23102-141">Deleting a directory is a fairly simple task, although it should be noted that you cannot delete a directory that still contains files or other directories.</span></span>

```java
// Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference toohello directory you want toodelete
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

// Delete hello directory
if ( containerDir.deleteIfExists() ) {
    System.out.println("Directory deleted");
}
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="23102-142">Vytvoření výčtu souborů a adresářů v Azure File sdílet</span><span class="sxs-lookup"><span data-stu-id="23102-142">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="23102-143">Získání seznamu souborů a adresářů v rámci sdílené složky se snadno provádí voláním **listFilesAndDirectories** na CloudFileDirectory odkaz.</span><span class="sxs-lookup"><span data-stu-id="23102-143">Obtaining a list of files and directories within a share is easily done by calling **listFilesAndDirectories** on a CloudFileDirectory reference.</span></span> <span data-ttu-id="23102-144">Hello metoda vrátí seznam hodnot ListFileItem objekty, které můžete iterovat na.</span><span class="sxs-lookup"><span data-stu-id="23102-144">hello method returns a list of ListFileItem objects which you can iterate on.</span></span> <span data-ttu-id="23102-145">Jako příklad hello následující kód zobrazí seznam souborů a adresářů v kořenovém adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="23102-145">As an example, hello following code will list files and directories inside hello root directory.</span></span>

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="upload-a-file"></a><span data-ttu-id="23102-146">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="23102-146">Upload a file</span></span>
<span data-ttu-id="23102-147">Soubor s Azure, který obsahuje sdílenou složku v hello velmi alespoň, kořenový adresář, kde mohou být uloženy soubory.</span><span class="sxs-lookup"><span data-stu-id="23102-147">An Azure File share contains at hello very least, a root directory where files can reside.</span></span> <span data-ttu-id="23102-148">V této části se dozvíte, jak tooupload soubor z místního úložiště do hello kořenový adresář sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="23102-148">In this section, you'll learn how tooupload a file from local storage onto hello root directory of a share.</span></span>

<span data-ttu-id="23102-149">Hello prvním krokem při nahrání souboru je tooobtain adresář toohello odkaz kde by měl být umístěn.</span><span class="sxs-lookup"><span data-stu-id="23102-149">hello first step in uploading a file is tooobtain a reference toohello directory where it should reside.</span></span> <span data-ttu-id="23102-150">To se provádí volání hello **getRootDirectoryReference** metodu objektu hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="23102-150">You do this by calling hello **getRootDirectoryReference** method of hello share object.</span></span>

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

<span data-ttu-id="23102-151">Teď, když máte odkaz toohello kořenovým adresářem sdílené složky hello, můžete nahrát soubor do pomocí hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="23102-151">Now that you have a reference toohello root directory of hello share, you can upload a file onto it using hello following code.</span></span>

```java
        // Define hello path tooa local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="download-a-file"></a><span data-ttu-id="23102-152">Stažení souboru</span><span class="sxs-lookup"><span data-stu-id="23102-152">Download a file</span></span>
<span data-ttu-id="23102-153">Jedním z hello další časté operace, které je potřeba provést před Azure File storage je toodownload soubory.</span><span class="sxs-lookup"><span data-stu-id="23102-153">One of hello more frequent operations you will perform against Azure File storage is toodownload files.</span></span> <span data-ttu-id="23102-154">V následujícím příkladu hello kód hello stáhne SampleFile.txt a zobrazí její obsah.</span><span class="sxs-lookup"><span data-stu-id="23102-154">In hello following example, hello code downloads SampleFile.txt and displays its contents.</span></span>

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference toohello directory that contains hello file
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

//Get a reference toohello file you want toodownload
CloudFile file = sampleDir.getFileReference("SampleFile.txt");

//Write hello contents of hello file toohello console.
System.out.println(file.downloadText());
```

## <a name="delete-a-file"></a><span data-ttu-id="23102-155">Odstranění souboru</span><span class="sxs-lookup"><span data-stu-id="23102-155">Delete a file</span></span>
<span data-ttu-id="23102-156">Další běžné operace úložiště Azure File je odstranění souborů.</span><span class="sxs-lookup"><span data-stu-id="23102-156">Another common Azure File storage operation is file deletion.</span></span> <span data-ttu-id="23102-157">Hello následující kód odstraní soubor s názvem SampleFile.txt uložený v adresáři s názvem **sampledir**.</span><span class="sxs-lookup"><span data-stu-id="23102-157">hello following code deletes a file named SampleFile.txt stored inside a directory named **sampledir**.</span></span>

```java
// Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference toohello directory where hello file toobe deleted is in
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

String filename = "SampleFile.txt"
CloudFile file;

file = containerDir.getFileReference(filename)
if ( file.deleteIfExists() ) {
    System.out.println(filename + " was deleted");
}
```

## <a name="next-steps"></a><span data-ttu-id="23102-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="23102-158">Next steps</span></span>
<span data-ttu-id="23102-159">Pokud vás zajímají další informace o dalších úložiště Azure rozhraní API toolearn, použijte tyto odkazy.</span><span class="sxs-lookup"><span data-stu-id="23102-159">If you would like toolearn more about other Azure storage APIs, follow these links.</span></span>

* [<span data-ttu-id="23102-160">Středisko pro vývojáře Java</span><span class="sxs-lookup"><span data-stu-id="23102-160">Java Developer Center</span></span>](http://azure.microsoft.com/develop/java/)
* [<span data-ttu-id="23102-161">Úložiště Azure SDK pro jazyk Java</span><span class="sxs-lookup"><span data-stu-id="23102-161">Azure Storage SDK for Java</span></span>](https://github.com/azure/azure-storage-java)
* [<span data-ttu-id="23102-162">Úložiště Azure SDK pro Android</span><span class="sxs-lookup"><span data-stu-id="23102-162">Azure Storage SDK for Android</span></span>](https://github.com/azure/azure-storage-android)
* [<span data-ttu-id="23102-163">Odkaz na sadu SDK klienta úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="23102-163">Azure Storage Client SDK Reference</span></span>](http://dl.windowsazure.com/storage/javadoc/)
* [<span data-ttu-id="23102-164">REST API služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="23102-164">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="23102-165">Blog týmu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="23102-165">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* [<span data-ttu-id="23102-166">Přenos dat pomocí příkazového řádku Azcopy hello</span><span class="sxs-lookup"><span data-stu-id="23102-166">Transfer data with hello AzCopy Command-Line Utility</span></span>](storage-use-azcopy.md)
