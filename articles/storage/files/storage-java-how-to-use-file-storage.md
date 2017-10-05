---
title: "Vývoj pro Azure File storage s Javou | Microsoft Docs"
description: "Další informace jak vyvíjet aplikace Java a služby, které používají Azure File storage k ukládání dat souborů."
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
ms.openlocfilehash: ce38944b9d5e663505c5808864ba61a5e2284f3b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="develop-for-azure-file-storage-with-java"></a><span data-ttu-id="aefd5-103">Vývoj pro Azure File storage s Javou</span><span class="sxs-lookup"><span data-stu-id="aefd5-103">Develop for Azure File storage with Java</span></span>
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="aefd5-104">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="aefd5-104">About this tutorial</span></span>
<span data-ttu-id="aefd5-105">Tento kurz popisuje základní informace o používání Javy k vývoji aplikací nebo služeb, které používají Azure File storage k ukládání dat souborů.</span><span class="sxs-lookup"><span data-stu-id="aefd5-105">This tutorial will demonstrate the basics of using Java to develop applications or services that use Azure File storage to store file data.</span></span> <span data-ttu-id="aefd5-106">V tomto kurzu jsme vytvořit jednoduché konzolové aplikace a ukazují, jak provést základní operace s Java a Azure File storage:</span><span class="sxs-lookup"><span data-stu-id="aefd5-106">In this tutorial, we will create a simple console application and show how to perform basic actions with Java and Azure File storage:</span></span>

* <span data-ttu-id="aefd5-107">Vytvářet a odstraňovat sdílené složky Azure File</span><span class="sxs-lookup"><span data-stu-id="aefd5-107">Create and delete Azure File shares</span></span>
* <span data-ttu-id="aefd5-108">Vytvářet a odstraňovat adresáře</span><span class="sxs-lookup"><span data-stu-id="aefd5-108">Create and delete directories</span></span>
* <span data-ttu-id="aefd5-109">Vytvoření výčtu souborů a adresářů v Azure File sdílet</span><span class="sxs-lookup"><span data-stu-id="aefd5-109">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="aefd5-110">Odesílání, stahování a odstranění souboru</span><span class="sxs-lookup"><span data-stu-id="aefd5-110">Upload, download, and delete a file</span></span>

> [!Note]  
> <span data-ttu-id="aefd5-111">Protože Azure File storage můžete získat přístup přes protokol SMB, je možné psát jednoduché aplikace, které přístup k Azure souborové složce přes standardní třídy Java vstupně-výstupních operací.</span><span class="sxs-lookup"><span data-stu-id="aefd5-111">Because Azure File storage may be accessed over SMB, it is possible to write simple applications that access the Azure File share using the standard Java I/O classes.</span></span> <span data-ttu-id="aefd5-112">Tento článek popisuje, jak k psaní aplikací, které používají Java SDK úložiště Azure, kterou používá [REST API služby Azure File storage](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) ke komunikaci s úložištěm Azure File.</span><span class="sxs-lookup"><span data-stu-id="aefd5-112">This article will describe how to write applications that use the Azure Storage Java SDK, which uses the [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) to talk to Azure File storage.</span></span>

## <a name="create-a-java-application"></a><span data-ttu-id="aefd5-113">Vytvoření aplikace Java</span><span class="sxs-lookup"><span data-stu-id="aefd5-113">Create a Java application</span></span>
<span data-ttu-id="aefd5-114">Pokud chcete vytvořit ukázky, budete potřebovat Java Development Kit (JDK) a [] [Azure SDK úložiště pro jazyk Java].</span><span class="sxs-lookup"><span data-stu-id="aefd5-114">To build the samples, you will need the Java Development Kit (JDK) and the [Azure Storage SDK for Java][].</span></span> <span data-ttu-id="aefd5-115">Musí také jste vytvořili účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="aefd5-115">You should also have created an Azure storage account.</span></span>

## <a name="setup-your-application-to-use-azure-file-storage"></a><span data-ttu-id="aefd5-116">Instalační program aplikace k používání Azure File storage</span><span class="sxs-lookup"><span data-stu-id="aefd5-116">Setup your application to use Azure File storage</span></span>
<span data-ttu-id="aefd5-117">Pokud chcete používat službu Azure storage rozhraní API, přidejte následující příkaz na začátek souboru Java, které máte v úmyslu přístup ke službě úložiště z.</span><span class="sxs-lookup"><span data-stu-id="aefd5-117">To use the Azure storage APIs, add the following statement to the top of the Java file where you intend to access the storage service from.</span></span>

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="aefd5-118">Instalační program připojovací řetězec úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="aefd5-118">Setup an Azure storage connection string</span></span>
<span data-ttu-id="aefd5-119">Používání Azure File storage, budete muset připojit k účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="aefd5-119">To use Azure File storage, you need to connect to your Azure storage account.</span></span> <span data-ttu-id="aefd5-120">Prvním krokem by mohla být nakonfigurovat připojovací řetězec, který jsme budete používat pro připojení k vašemu účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aefd5-120">The first step would be to configure a connection string which we'll use to connect to your storage account.</span></span> <span data-ttu-id="aefd5-121">Umožňuje definovat statické proměnné na to.</span><span class="sxs-lookup"><span data-stu-id="aefd5-121">Let's define a static variable to do that.</span></span>

```java
// Configure the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> <span data-ttu-id="aefd5-122">Your_storage_account_name a your_storage_account_key nahraďte skutečnými hodnotami pro váš účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="aefd5-122">Replace your_storage_account_name and your_storage_account_key with the actual values for your storage account.</span></span>
> 
> 

## <a name="connecting-to-an-azure-storage-account"></a><span data-ttu-id="aefd5-123">Připojení k účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="aefd5-123">Connecting to an Azure storage account</span></span>
<span data-ttu-id="aefd5-124">Pokud chcete připojit k účtu úložiště, budete muset použít **CloudStorageAccount** objekt, předávání připojovací řetězec k jeho **analyzovat** metoda.</span><span class="sxs-lookup"><span data-stu-id="aefd5-124">To connect to your storage account, you need to use the **CloudStorageAccount** object, passing a connection string to its **parse** method.</span></span>

```java
// Use the CloudStorageAccount object to connect to your storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle the exception
}
```

<span data-ttu-id="aefd5-125">**CloudStorageAccount.parse** vyvolá InvalidKeyException, budete muset uvést uvnitř bloku try/catch.</span><span class="sxs-lookup"><span data-stu-id="aefd5-125">**CloudStorageAccount.parse** throws an InvalidKeyException so you'll need to put it inside a try/catch block.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="aefd5-126">Vytvoření Azure sdílené složky</span><span class="sxs-lookup"><span data-stu-id="aefd5-126">Create an Azure File share</span></span>
<span data-ttu-id="aefd5-127">Všechny soubory a adresáře v úložišti Azure File jsou umístěny v kontejneru názvem **sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="aefd5-127">All files and directories in Azure File storage reside in a container called a **Share**.</span></span> <span data-ttu-id="aefd5-128">Váš účet úložiště může mít tolik sdílených složek jako umožňuje kapacitě vašeho účtu.</span><span class="sxs-lookup"><span data-stu-id="aefd5-128">Your storage account can have as much shares as your account capacity allows.</span></span> <span data-ttu-id="aefd5-129">Pokud chcete získat přístup ke sdílené složce a její obsah, musíte použít klienta úložiště Azure File.</span><span class="sxs-lookup"><span data-stu-id="aefd5-129">To obtain access to a share and its contents, you need to use a Azure File storage client.</span></span>

```java
// Create the Azure File storage client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

<span data-ttu-id="aefd5-130">Pomocí klienta aplikace Azure File storage můžete pak získat odkaz na sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="aefd5-130">Using the Azure File storage client, you can then obtain a reference to a share.</span></span>

```java
// Get a reference to the file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

<span data-ttu-id="aefd5-131">Chcete-li ve skutečnosti vytvořit sdílenou složku, použijte **createIfNotExists** metodu objektu CloudFileShare.</span><span class="sxs-lookup"><span data-stu-id="aefd5-131">To actually create the share, use the **createIfNotExists** method of the CloudFileShare object.</span></span>

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

<span data-ttu-id="aefd5-132">V tomto okamžiku **sdílet** obsahuje odkaz na sdílenou složku s názvem **sampleshare**.</span><span class="sxs-lookup"><span data-stu-id="aefd5-132">At this point, **share** holds a reference to a share named **sampleshare**.</span></span>

## <a name="delete-an-azure-file-share"></a><span data-ttu-id="aefd5-133">Odstranit sdílenou složku Azure</span><span class="sxs-lookup"><span data-stu-id="aefd5-133">Delete an Azure File share</span></span>
<span data-ttu-id="aefd5-134">Odstranění sdílené složky se provádí volání **deleteIfExists** metoda CloudFileShare objektu.</span><span class="sxs-lookup"><span data-stu-id="aefd5-134">Deleting a share is done by calling the **deleteIfExists** method on a CloudFileShare object.</span></span> <span data-ttu-id="aefd5-135">Tady je ukázkový kód, který nemá.</span><span class="sxs-lookup"><span data-stu-id="aefd5-135">Here's sample code that does that.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the file client.
   CloudFileClient fileClient = storageAccount.createCloudFileClient();

   // Get a reference to the file share
   CloudFileShare share = fileClient.getShareReference("sampleshare");

   if (share.deleteIfExists()) {
       System.out.println("sampleshare deleted");
   }
} catch (Exception e) {
    e.printStackTrace();
}
```

## <a name="create-a-directory"></a><span data-ttu-id="aefd5-136">Vytvoření adresáře</span><span class="sxs-lookup"><span data-stu-id="aefd5-136">Create a directory</span></span>
<span data-ttu-id="aefd5-137">Úložiště můžete navíc uspořádat umístěním souborů v podadresářích místo nutnosti všechny z nich v kořenovém adresáři.</span><span class="sxs-lookup"><span data-stu-id="aefd5-137">You can also organize storage by putting files inside sub-directories instead of having all of them in the root directory.</span></span> <span data-ttu-id="aefd5-138">Azure File storage můžete vytvořit tolik adresáře, které umožní váš účet.</span><span class="sxs-lookup"><span data-stu-id="aefd5-138">Azure File storage allows you to create as much directories as your account will allow.</span></span> <span data-ttu-id="aefd5-139">Následující kód vytvoří adresář s názvem **sampledir** pod kořenovým adresářem.</span><span class="sxs-lookup"><span data-stu-id="aefd5-139">The code below will create a sub-directory named **sampledir** under the root directory.</span></span>

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the sampledir directory
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

if (sampleDir.createIfNotExists()) {
    System.out.println("sampledir created");
} else {
    System.out.println("sampledir already exists");
}
```

## <a name="delete-a-directory"></a><span data-ttu-id="aefd5-140">Odstranění adresáře</span><span class="sxs-lookup"><span data-stu-id="aefd5-140">Delete a directory</span></span>
<span data-ttu-id="aefd5-141">Odstranění adresáře je docela jednoduché úlohy, i když je potřeba poznamenat, že nelze odstranit adresář, který ještě obsahuje soubory nebo ostatních adresářů.</span><span class="sxs-lookup"><span data-stu-id="aefd5-141">Deleting a directory is a fairly simple task, although it should be noted that you cannot delete a directory that still contains files or other directories.</span></span>

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory you want to delete
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

// Delete the directory
if ( containerDir.deleteIfExists() ) {
    System.out.println("Directory deleted");
}
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="aefd5-142">Vytvoření výčtu souborů a adresářů v Azure File sdílet</span><span class="sxs-lookup"><span data-stu-id="aefd5-142">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="aefd5-143">Získání seznamu souborů a adresářů v rámci sdílené složky se snadno provádí voláním **listFilesAndDirectories** na CloudFileDirectory odkaz.</span><span class="sxs-lookup"><span data-stu-id="aefd5-143">Obtaining a list of files and directories within a share is easily done by calling **listFilesAndDirectories** on a CloudFileDirectory reference.</span></span> <span data-ttu-id="aefd5-144">Metoda vrátí seznam ListFileItem objektů, které můžete iterovat na.</span><span class="sxs-lookup"><span data-stu-id="aefd5-144">The method returns a list of ListFileItem objects which you can iterate on.</span></span> <span data-ttu-id="aefd5-145">Například následující kód zobrazí seznam souborů a adresářů v kořenovém adresáři.</span><span class="sxs-lookup"><span data-stu-id="aefd5-145">As an example, the following code will list files and directories inside the root directory.</span></span>

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="upload-a-file"></a><span data-ttu-id="aefd5-146">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="aefd5-146">Upload a file</span></span>
<span data-ttu-id="aefd5-147">Soubor s Azure, které se sdílená složka obsahuje v každém, kořenový adresář, kde mohou být uloženy soubory.</span><span class="sxs-lookup"><span data-stu-id="aefd5-147">An Azure File share contains at the very least, a root directory where files can reside.</span></span> <span data-ttu-id="aefd5-148">V této části se dozvíte jak nahrát soubor z místního úložiště do kořenového adresáře sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="aefd5-148">In this section, you'll learn how to upload a file from local storage onto the root directory of a share.</span></span>

<span data-ttu-id="aefd5-149">Prvním krokem při nahrání souboru se má získat odkaz na adresář, kde by měl být umístěn.</span><span class="sxs-lookup"><span data-stu-id="aefd5-149">The first step in uploading a file is to obtain a reference to the directory where it should reside.</span></span> <span data-ttu-id="aefd5-150">To provedete pomocí volání **getRootDirectoryReference** metodu objektu sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="aefd5-150">You do this by calling the **getRootDirectoryReference** method of the share object.</span></span>

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

<span data-ttu-id="aefd5-151">Teď, když máte odkaz na kořenovém adresáři sdílené složky, můžete nahrát soubor do jej pomocí následujícího kódu.</span><span class="sxs-lookup"><span data-stu-id="aefd5-151">Now that you have a reference to the root directory of the share, you can upload a file onto it using the following code.</span></span>

```java
        // Define the path to a local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="download-a-file"></a><span data-ttu-id="aefd5-152">Stažení souboru</span><span class="sxs-lookup"><span data-stu-id="aefd5-152">Download a file</span></span>
<span data-ttu-id="aefd5-153">Jedním z častější operace, které je potřeba provést před Azure File storage je ke stažení souborů.</span><span class="sxs-lookup"><span data-stu-id="aefd5-153">One of the more frequent operations you will perform against Azure File storage is to download files.</span></span> <span data-ttu-id="aefd5-154">V následujícím příkladu kódu stáhne SampleFile.txt a zobrazí její obsah.</span><span class="sxs-lookup"><span data-stu-id="aefd5-154">In the following example, the code downloads SampleFile.txt and displays its contents.</span></span>

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the directory that contains the file
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

//Get a reference to the file you want to download
CloudFile file = sampleDir.getFileReference("SampleFile.txt");

//Write the contents of the file to the console.
System.out.println(file.downloadText());
```

## <a name="delete-a-file"></a><span data-ttu-id="aefd5-155">Odstranění souboru</span><span class="sxs-lookup"><span data-stu-id="aefd5-155">Delete a file</span></span>
<span data-ttu-id="aefd5-156">Další běžné operace úložiště Azure File je odstranění souborů.</span><span class="sxs-lookup"><span data-stu-id="aefd5-156">Another common Azure File storage operation is file deletion.</span></span> <span data-ttu-id="aefd5-157">Následující kód odstraní soubor s názvem SampleFile.txt uložený v adresáři s názvem **sampledir**.</span><span class="sxs-lookup"><span data-stu-id="aefd5-157">The following code deletes a file named SampleFile.txt stored inside a directory named **sampledir**.</span></span>

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory where the file to be deleted is in
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

String filename = "SampleFile.txt"
CloudFile file;

file = containerDir.getFileReference(filename)
if ( file.deleteIfExists() ) {
    System.out.println(filename + " was deleted");
}
```

## <a name="next-steps"></a><span data-ttu-id="aefd5-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aefd5-158">Next steps</span></span>
<span data-ttu-id="aefd5-159">Pokud vás zajímají další informace o dalších úložiště Azure API, použijte tyto odkazy.</span><span class="sxs-lookup"><span data-stu-id="aefd5-159">If you would like to learn more about other Azure storage APIs, follow these links.</span></span>

* <span data-ttu-id="aefd5-160">[Azure pro vývojáře v jazyce Java](/java/azure)nebo)</span><span class="sxs-lookup"><span data-stu-id="aefd5-160">[Azure for Java developers](/java/azure)/)</span></span>
* [<span data-ttu-id="aefd5-161">Úložiště Azure SDK pro jazyk Java</span><span class="sxs-lookup"><span data-stu-id="aefd5-161">Azure Storage SDK for Java</span></span>](https://github.com/azure/azure-storage-java)
* [<span data-ttu-id="aefd5-162">Úložiště Azure SDK pro Android</span><span class="sxs-lookup"><span data-stu-id="aefd5-162">Azure Storage SDK for Android</span></span>](https://github.com/azure/azure-storage-android)
* [<span data-ttu-id="aefd5-163">Odkaz na sadu SDK klienta úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="aefd5-163">Azure Storage Client SDK Reference</span></span>](http://dl.windowsazure.com/storage/javadoc/)
* [<span data-ttu-id="aefd5-164">REST API služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="aefd5-164">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="aefd5-165">Blog týmu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="aefd5-165">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* <span data-ttu-id="aefd5-166">[Přenos dat pomocí nástroje příkazového řádku AzCopy](../common/storage-use-azcopy.md* [Troubleshooting Azure File storage problems - Windows](storage-troubleshoot-windows-file-connection-problems.md)
)</span><span class="sxs-lookup"><span data-stu-id="aefd5-166">[Transfer data with the AzCopy Command-Line Utility](../common/storage-use-azcopy.md* [Troubleshooting Azure File storage problems - Windows](storage-troubleshoot-windows-file-connection-problems.md)
)</span></span>