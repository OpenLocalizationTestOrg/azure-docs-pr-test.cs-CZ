---
title: "Jak používat úložiště objektů Blob v Azure (úložiště objektů) z Javy | Microsoft Docs"
description: "Ukládejte nestrukturovaná data v cloudu pomocí Azure Blob Storage (úložiště objektů)."
services: storage
documentationcenter: java
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 2e223b38-92de-4c2f-9254-346374545d32
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: b8a4eca600b458802a7a23851bb80ea4da2664ef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-blob-storage-from-java"></a><span data-ttu-id="dfc76-103">Používání úložiště Blob z Javy</span><span class="sxs-lookup"><span data-stu-id="dfc76-103">How to use Blob storage from Java</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a><span data-ttu-id="dfc76-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="dfc76-104">Overview</span></span>
<span data-ttu-id="dfc76-105">Úložiště objektů blob v Azure je služba, která ukládá nestrukturovaná data v cloudu jako objekty nebo objekty blob.</span><span class="sxs-lookup"><span data-stu-id="dfc76-105">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="dfc76-106">Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="dfc76-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="dfc76-107">Blob storage se také nazývá úložiště objektů.</span><span class="sxs-lookup"><span data-stu-id="dfc76-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="dfc76-108">Tento článek vám ukáže, jak provádět běžné scénáře s využitím Microsoft Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="dfc76-108">This article will show you how to perform common scenarios using the Microsoft Azure Blob storage.</span></span> <span data-ttu-id="dfc76-109">Ukázky jsou napsané v jazyce Java a použít [sada SDK úložiště Azure pro jazyk Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="dfc76-109">The samples are written in Java and use the [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="dfc76-110">Pokryté scénáře zahrnují **odesílání**, **výpis**, **stahování**, a **odstraňování** objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="dfc76-110">The scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="dfc76-111">Další informace o objekty BLOB, najdete v článku [další kroky](#Next-Steps) části.</span><span class="sxs-lookup"><span data-stu-id="dfc76-111">For more information on blobs, see the [Next Steps](#Next-Steps) section.</span></span>

> [!NOTE]
> <span data-ttu-id="dfc76-112">Sada SDK je k dispozici pro vývojáře, kteří na zařízení se systémem Android používají Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="dfc76-112">An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="dfc76-113">Další informace najdete v tématu [sada SDK úložiště Azure pro Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="dfc76-113">For more information, see the [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>
>
>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="dfc76-114">Vytvoření aplikace Java</span><span class="sxs-lookup"><span data-stu-id="dfc76-114">Create a Java application</span></span>
<span data-ttu-id="dfc76-115">V tomto článku budete používat funkce úložiště, které lze spustit v rámci aplikace Java místně nebo v kódu běžící v rámci webovou roli nebo role pracovního procesu v Azure.</span><span class="sxs-lookup"><span data-stu-id="dfc76-115">In this article, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="dfc76-116">To pokud chcete udělat, musíte nainstalovat Java Development Kit (JDK) a vytvoření účtu úložiště Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="dfc76-116">To do so, you will need to install the Java Development Kit (JDK) and create an Azure Storage account in your Azure subscription.</span></span> <span data-ttu-id="dfc76-117">Jakmile provedete, budete muset ověřit, jestli váš vývojový systém splňuje minimální požadavky a závislosti, které jsou uvedeny v [sada SDK úložiště Azure pro jazyk Java] [ Azure Storage SDK for Java] úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="dfc76-117">Once you have done so, you will need to verify that your development system meets the minimum requirements and dependencies which are listed in the [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="dfc76-118">Pokud váš systém splňuje tyto požadavky, můžete podle pokynů ke stažení a instalaci knihovny úložiště Azure pro jazyk Java systému z tohoto úložiště.</span><span class="sxs-lookup"><span data-stu-id="dfc76-118">If your system meets those requirements, you can follow the instructions for downloading and installing the Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="dfc76-119">Po dokončení těchto úloh, bude moci vytvořit aplikaci Java, která používá v příkladech v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="dfc76-119">Once you have completed those tasks, you will be able to create a Java application which uses the examples in this article.</span></span>

## <a name="configure-your-application-to-access-blob-storage"></a><span data-ttu-id="dfc76-120">Konfigurace aplikace pro přístup k úložišti objektů Blob</span><span class="sxs-lookup"><span data-stu-id="dfc76-120">Configure your application to access Blob storage</span></span>
<span data-ttu-id="dfc76-121">Přidejte následující příkazy pro import do horní části souboru Java, ve které chcete používat rozhraní API úložiště Azure pro přístup k objektům BLOB.</span><span class="sxs-lookup"><span data-stu-id="dfc76-121">Add the following import statements to the top of the Java file where you want to use the Azure Storage APIs to access blobs.</span></span>

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="dfc76-122">Nastavit připojovací řetězec službě Azure Storage</span><span class="sxs-lookup"><span data-stu-id="dfc76-122">Set up an Azure Storage connection string</span></span>
<span data-ttu-id="dfc76-123">Klienta Azure Storage používá připojovací řetězec úložiště k ukládání koncových bodů a pověření pro přístup ke službám dat správy.</span><span class="sxs-lookup"><span data-stu-id="dfc76-123">An Azure Storage client uses a storage connection string to store endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="dfc76-124">Když spustíte v aplikaci klienta, je nutné zadat připojovací řetězec úložiště v následujícím formátu, pomocí názvu účtu úložiště a primární přístupový klíč pro účet úložiště uvedené v [portál Azure](https://portal.azure.com) pro *AccountName* a *AccountKey* hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dfc76-124">When running in a client application, you must provide the storage connection string in the following format, using the name of your storage account and the Primary access key for the storage account listed in the [Azure portal](https://portal.azure.com) for the *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="dfc76-125">Následující příklad ukazuje, jak můžou deklarovat statické pole pro uložení připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="dfc76-125">The following example shows how you can declare a static field to hold the connection string.</span></span>

```java
// Define the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="dfc76-126">Ve spuštěné aplikace v rámci role ve službě Microsoft Azure, můžete tento řetězec uložené v konfiguračním souboru služby *souboru ServiceConfiguration.cscfg*a je přístupný pomocí volání **RoleEnvironment.getConfigurationSettings** metoda.</span><span class="sxs-lookup"><span data-stu-id="dfc76-126">In an application running within a role in Microsoft Azure, this string can be stored in the service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call to the **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="dfc76-127">Následující příklad získá připojovací řetězec z **nastavení** element s názvem *StorageConnectionString* v konfiguračním souboru služby.</span><span class="sxs-lookup"><span data-stu-id="dfc76-127">The following example gets the connection string from a **Setting** element named *StorageConnectionString* in the service configuration file.</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="dfc76-128">Následující ukázky předpokládejme, že používáte jednu z těchto dvou metod k získání připojovacího řetězce úložiště.</span><span class="sxs-lookup"><span data-stu-id="dfc76-128">The following samples assume that you have used one of these two methods to get the storage connection string.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="dfc76-129">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="dfc76-129">Create a container</span></span>
<span data-ttu-id="dfc76-130">A **CloudBlobClient** objektu umožňuje získat odkaz na objekty pro kontejnery a objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="dfc76-130">A **CloudBlobClient** object lets you get reference objects for containers and blobs.</span></span> <span data-ttu-id="dfc76-131">Následující kód vytvoří **CloudBlobClient** objektu.</span><span class="sxs-lookup"><span data-stu-id="dfc76-131">The following code creates a **CloudBlobClient** object.</span></span>

> [!NOTE]
> <span data-ttu-id="dfc76-132">Chcete-li vytvořit další způsoby **CloudStorageAccount** objekty; Další informace najdete v tématu **CloudStorageAccount** v [odkaz SDK klienta úložiště Azure].</span><span class="sxs-lookup"><span data-stu-id="dfc76-132">There are additional ways to create **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in the [Azure Storage Client SDK Reference].</span></span>
>
>

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="dfc76-133">Použít **CloudBlobClient** objekt, který chcete získat odkaz na kontejner, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="dfc76-133">Use the **CloudBlobClient** object to get a reference to the container you want to use.</span></span> <span data-ttu-id="dfc76-134">Kontejner můžete vytvořit, pokud neexistuje s **createIfNotExists** metodu, která v opačném případě vrátí existující kontejner.</span><span class="sxs-lookup"><span data-stu-id="dfc76-134">You can create the container if it doesn't exist with the **createIfNotExists** method, which will otherwise return the existing container.</span></span> <span data-ttu-id="dfc76-135">Ve výchozím nastavení je nový kontejner privátní, proto přístupový klíč k úložišti musí určit (stejně jako dříve) ke stažení z tohoto kontejneru objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="dfc76-135">By default, the new container is private, so you must specify your storage access key (as you did earlier) to download blobs from this container.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Get a reference to a container.
    // The container name must be lower case
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Create the container if it does not exist.
    container.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

### <a name="optional-configure-a-container-for-public-access"></a><span data-ttu-id="dfc76-136">Volitelné: Konfigurace kontejner pro veřejný přístup</span><span class="sxs-lookup"><span data-stu-id="dfc76-136">Optional: Configure a container for public access</span></span>
<span data-ttu-id="dfc76-137">Kontejneru oprávnění jsou ve výchozím nastavení nakonfigurované pro soukromý přístup, ale můžete snadno konfigurovat kontejneru oprávnění pro povolení přístupu veřejné, jen pro čtení pro všechny uživatele v síti Internet:</span><span class="sxs-lookup"><span data-stu-id="dfc76-137">A container's permissions are configured for private access by default, but you can easily configure a container's permissions to allow public, read-only access for all users on the Internet:</span></span>

```java
// Create a permissions object.
BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

// Include public access in the permissions object.
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

// Set the permissions on the container.
container.uploadPermissions(containerPermissions);
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="dfc76-138">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="dfc76-138">Upload a blob into a container</span></span>
<span data-ttu-id="dfc76-139">Chcete-li nahrát soubor do objektu blob, získejte odkaz na kontejner a umožňuje vám získat odkaz na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="dfc76-139">To upload a file to a blob, get a container reference and use it to get a blob reference.</span></span> <span data-ttu-id="dfc76-140">Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud voláním nahrávání na odkaz na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="dfc76-140">Once you have a blob reference, you can upload any stream by calling upload on the blob reference.</span></span> <span data-ttu-id="dfc76-141">Tato operace vytvoří objekt blob, pokud není neexistuje, nebo ho přepíše, pokud ji nemá.</span><span class="sxs-lookup"><span data-stu-id="dfc76-141">This operation will create the blob if it doesn't exist, or overwrite it if it does.</span></span> <span data-ttu-id="dfc76-142">Následující příklad kódu ukazuje to a předpokládá, že kontejner byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="dfc76-142">The following code sample shows this, and assumes that the container has already been created.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Define the path to a local file.
    final String filePath = "C:\\myimages\\myimage.jpg";

    // Create or overwrite the "myimage.jpg" blob with contents from a local file.
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");
    File source = new File(filePath);
    blob.upload(new FileInputStream(source), source.length());
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="dfc76-143">Zobrazí seznam objektů blob v kontejneru</span><span class="sxs-lookup"><span data-stu-id="dfc76-143">List the blobs in a container</span></span>
<span data-ttu-id="dfc76-144">Pro zobrazení seznamu objektů BLOB v kontejneru, nejdřív získejte odkaz na kontejner stejně, jako jste nahrát objekt blob.</span><span class="sxs-lookup"><span data-stu-id="dfc76-144">To list the blobs in a container, first get a container reference like you did to upload a blob.</span></span> <span data-ttu-id="dfc76-145">Můžete použít metodu kontejneru **listBlobs** metoda s **pro** smyčky.</span><span class="sxs-lookup"><span data-stu-id="dfc76-145">You can use the container's **listBlobs** method with a **for** loop.</span></span> <span data-ttu-id="dfc76-146">Následující kód výstupy identifikátor Uri jednotlivých objektů blob v kontejneru ke konzole.</span><span class="sxs-lookup"><span data-stu-id="dfc76-146">The following code outputs the Uri of each blob in a container to the console.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop over blobs within the container and output the URI to each of them.
    for (ListBlobItem blobItem : container.listBlobs()) {
        System.out.println(blobItem.getUri());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="dfc76-147">Všimněte si, že můžete pojmenovat objekty BLOB, informace o cestě jejich názvy.</span><span class="sxs-lookup"><span data-stu-id="dfc76-147">Note that you can name blobs with path information in their names.</span></span> <span data-ttu-id="dfc76-148">Tím se vytvoří struktura virtuálního adresáře, kterou můžete organizovat a měnit podle potřeby jako u tradičních systémů souborů.</span><span class="sxs-lookup"><span data-stu-id="dfc76-148">This creates a virtual directory structure that you can organize and traverse as you would a traditional file system.</span></span> <span data-ttu-id="dfc76-149">Všimněte si, že struktura adresářů je jenom virtuální - jediné prostředky, které jsou dostupné v úložišti objektů Blob,  jsou kontejnery a objekty blob.</span><span class="sxs-lookup"><span data-stu-id="dfc76-149">Note that the directory structure is virtual only - the only resources available in Blob storage are containers and blobs.</span></span> <span data-ttu-id="dfc76-150">Však nabízí klientské knihovny **CloudBlobDirectory** objekt, který má odkazovat na virtuální adresář a zjednodušit tak práci s objekty BLOB, které jsou tímto způsobem uspořádány.</span><span class="sxs-lookup"><span data-stu-id="dfc76-150">However, the client library offers a **CloudBlobDirectory** object to refer to a virtual directory and simplify the process of working with blobs that are organized in this way.</span></span>

<span data-ttu-id="dfc76-151">Například můžete mít kontejner s názvem "fotografie", ve kterých může odeslat objekty BLOB s názvem "rootphoto1", "2010 nebo photo1", "2010 nebo photo2" a "2011/photo1".</span><span class="sxs-lookup"><span data-stu-id="dfc76-151">For example, you could have a container named "photos", in which you might upload blobs named "rootphoto1", "2010/photo1", "2010/photo2", and "2011/photo1".</span></span> <span data-ttu-id="dfc76-152">Tím se vytvoří virtuální adresáře "2010" a "2011" v "fotografie" kontejneru.</span><span class="sxs-lookup"><span data-stu-id="dfc76-152">This would create the virtual directories "2010" and "2011" within the "photos" container.</span></span> <span data-ttu-id="dfc76-153">Při volání **listBlobs** na kontejneru "fotografie" vrátí kolekce, která bude obsahovat **CloudBlobDirectory** a **CloudBlob** objekty, které představují adresáře a objekty BLOB obsažené na nejvyšší úrovni.</span><span class="sxs-lookup"><span data-stu-id="dfc76-153">When you call **listBlobs** on the "photos" container, the collection returned will contain **CloudBlobDirectory** and **CloudBlob** objects representing the directories and blobs contained at the top level.</span></span> <span data-ttu-id="dfc76-154">V takovém případě by byla vrácena adresáře "2010" a "2011", jakož i photo "rootphoto1".</span><span class="sxs-lookup"><span data-stu-id="dfc76-154">In this case, directories "2010" and "2011", as well as photo "rootphoto1" would be returned.</span></span> <span data-ttu-id="dfc76-155">Můžete použít **instanceof** operátor k rozlišení tyto objekty.</span><span class="sxs-lookup"><span data-stu-id="dfc76-155">You can use the **instanceof** operator to distinguish these objects.</span></span>

<span data-ttu-id="dfc76-156">Volitelně můžete předat parametry, které **listBlobs** metoda s **useFlatBlobListing** parametr nastaven na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="dfc76-156">Optionally, you can pass in parameters to the **listBlobs** method with the **useFlatBlobListing** parameter set to true.</span></span> <span data-ttu-id="dfc76-157">Výsledkem bude každý objekt blob se vrátí, a to bez ohledu na adresář.</span><span class="sxs-lookup"><span data-stu-id="dfc76-157">This will result in every blob being returned, regardless of directory.</span></span> <span data-ttu-id="dfc76-158">Další informace najdete v tématu **CloudBlobContainer.listBlobs** v [odkaz SDK klienta úložiště Azure].</span><span class="sxs-lookup"><span data-stu-id="dfc76-158">For more information, see **CloudBlobContainer.listBlobs** in the [Azure Storage Client SDK Reference].</span></span>

## <a name="download-a-blob"></a><span data-ttu-id="dfc76-159">Stažení objektu blob</span><span class="sxs-lookup"><span data-stu-id="dfc76-159">Download a blob</span></span>
<span data-ttu-id="dfc76-160">Chcete-li stáhnout objekty BLOB, postupujte stejným způsobem, jako jste to udělali nahrát objekt blob, pokud chcete získat odkaz na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="dfc76-160">To download blobs, follow the same steps as you did for uploading a blob in order to get a blob reference.</span></span> <span data-ttu-id="dfc76-161">V příkladu odesílání volá nahrávání na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="dfc76-161">In the uploading example, you called upload on the blob object.</span></span> <span data-ttu-id="dfc76-162">V následujícím příkladu volání stažení k přenosu obsahu objektu blob na objekt proudu, jako **FileOutputStream** , můžete použít k uchování objektu blob do místního souboru.</span><span class="sxs-lookup"><span data-stu-id="dfc76-162">In the following example, call download to transfer the blob contents to a stream object such as a **FileOutputStream** that you can use to persist the blob to a local file.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop through each blob item in the container.
    for (ListBlobItem blobItem : container.listBlobs()) {
        // If the item is a blob, not a virtual directory.
        if (blobItem instanceof CloudBlob) {
            // Download the item and save it to a file with the same name.
            CloudBlob blob = (CloudBlob) blobItem;
            blob.download(new FileOutputStream("C:\\mydownloads\\" + blob.getName()));
        }
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob"></a><span data-ttu-id="dfc76-163">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="dfc76-163">Delete a blob</span></span>
<span data-ttu-id="dfc76-164">Chcete-li odstranit objekt blob, získání odkaz na objekt blob a volání **deleteIfExists**.</span><span class="sxs-lookup"><span data-stu-id="dfc76-164">To delete a blob, get a blob reference, and call **deleteIfExists**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Retrieve reference to a blob named "myimage.jpg".
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");

    // Delete the blob.
    blob.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob-container"></a><span data-ttu-id="dfc76-165">Odstranit kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="dfc76-165">Delete a blob container</span></span>
<span data-ttu-id="dfc76-166">Nakonec pokud chcete odstranit kontejner objektů blob, získání objektu blob odkaz na kontejner a volání **deleteIfExists**.</span><span class="sxs-lookup"><span data-stu-id="dfc76-166">Finally, to delete a blob container, get a blob container reference, and call **deleteIfExists**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Delete the blob container.
    container.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a><span data-ttu-id="dfc76-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dfc76-167">Next steps</span></span>
<span data-ttu-id="dfc76-168">Teď, když jste se naučili základy používání Blob storage, postupujte podle následujících odkazech na další informace o složitějších úlohách úložiště.</span><span class="sxs-lookup"><span data-stu-id="dfc76-168">Now that you've learned the basics of Blob storage, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="dfc76-169">[Úložiště Azure SDK pro jazyk Java][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="dfc76-169">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="dfc76-170">[Referenční informace sady SDK úložiště Azure klienta][odkaz SDK klienta úložiště Azure]</span><span class="sxs-lookup"><span data-stu-id="dfc76-170">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="dfc76-171">[REST API pro Azure Storage][Azure Storage REST API]</span><span class="sxs-lookup"><span data-stu-id="dfc76-171">[Azure Storage REST API][Azure Storage REST API]</span></span>
* <span data-ttu-id="dfc76-172">[Blog týmu Azure Storage][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="dfc76-172">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

<span data-ttu-id="dfc76-173">Další informace naleznete také [středisko pro vývojáře Java](/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="dfc76-173">For more information, see also the [Java Developer Center](/develop/java/).</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
<span data-ttu-id="dfc76-174">[odkaz SDK klienta úložiště Azure]: http://dl.windowsazure.com/storage/javadoc/</span><span class="sxs-lookup"><span data-stu-id="dfc76-174">[Azure Storage Client SDK Reference]: http://dl.windowsazure.com/storage/javadoc/</span></span>
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
