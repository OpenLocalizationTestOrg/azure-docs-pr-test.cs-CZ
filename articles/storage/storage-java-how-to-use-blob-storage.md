---
title: "aaaHow toouse úložiště objektů Blob v Azure (úložiště objektů) z Javy | Microsoft Docs"
description: "Ukládání nestrukturovaných dat v cloudu hello s Azure Blob storage (úložiště objektů)."
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
ms.openlocfilehash: 6dd6efdf688161c32b9d73a65fa7f3a98f2fad74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-java"></a><span data-ttu-id="f4b00-103">Jak toouse úložiště Blob z Javy</span><span class="sxs-lookup"><span data-stu-id="f4b00-103">How toouse Blob storage from Java</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a><span data-ttu-id="f4b00-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="f4b00-104">Overview</span></span>
<span data-ttu-id="f4b00-105">Azure Blob storage je služba, která ukládá Nestrukturovaná data v cloudu hello jako objekty nebo objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="f4b00-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="f4b00-106">Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4b00-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="f4b00-107">Úložiště objektů blob je také odkazované tooas objektu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f4b00-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="f4b00-108">Tento článek vám ukáže, jak hello tooperform běžné scénáře s využitím Microsoft Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="f4b00-108">This article will show you how tooperform common scenarios using hello Microsoft Azure Blob storage.</span></span> <span data-ttu-id="f4b00-109">Hello ukázky jsou napsané v jazyce Java a používají hello [sada SDK úložiště Azure pro jazyk Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="f4b00-109">hello samples are written in Java and use hello [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="f4b00-110">Hello pokryté scénáře zahrnují **odesílání**, **výpis**, **stahování**, a **odstraňování** objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="f4b00-110">hello scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="f4b00-111">Další informace o objekty BLOB najdete v tématu hello [další kroky](#Next-Steps) části.</span><span class="sxs-lookup"><span data-stu-id="f4b00-111">For more information on blobs, see hello [Next Steps](#Next-Steps) section.</span></span>

> [!NOTE]
> <span data-ttu-id="f4b00-112">Sada SDK je k dispozici pro vývojáře, kteří na zařízení se systémem Android používají Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="f4b00-112">An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="f4b00-113">Další informace najdete v tématu hello [sada SDK úložiště Azure pro Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="f4b00-113">For more information, see hello [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>
>
>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="f4b00-114">Vytvoření aplikace Java</span><span class="sxs-lookup"><span data-stu-id="f4b00-114">Create a Java application</span></span>
<span data-ttu-id="f4b00-115">V tomto článku budete používat funkce úložiště, které lze spustit v rámci aplikace Java místně nebo v kódu běžící v rámci webovou roli nebo role pracovního procesu v Azure.</span><span class="sxs-lookup"><span data-stu-id="f4b00-115">In this article, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="f4b00-116">toodo tedy budete potřebovat tooinstall hello Java Development Kit (JDK) a vytvoření účtu úložiště Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="f4b00-116">toodo so, you will need tooinstall hello Java Development Kit (JDK) and create an Azure Storage account in your Azure subscription.</span></span> <span data-ttu-id="f4b00-117">Jakmile provedete, budete potřebovat tooverify, který váš vývojový systém splňuje minimální požadavky hello a závislosti, které jsou uvedeny v hello [sada SDK úložiště Azure pro jazyk Java] [ Azure Storage SDK for Java] úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="f4b00-117">Once you have done so, you will need tooverify that your development system meets hello minimum requirements and dependencies which are listed in hello [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="f4b00-118">Pokud váš systém splňuje tyto požadavky, můžete postupovat podle pokynů hello ke stažení a instalaci hello knihovny úložiště Azure pro jazyk Java systému z tohoto úložiště.</span><span class="sxs-lookup"><span data-stu-id="f4b00-118">If your system meets those requirements, you can follow hello instructions for downloading and installing hello Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="f4b00-119">Po dokončení těchto úloh, bude možné toocreate aplikaci Java, která používá hello příklady v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="f4b00-119">Once you have completed those tasks, you will be able toocreate a Java application which uses hello examples in this article.</span></span>

## <a name="configure-your-application-tooaccess-blob-storage"></a><span data-ttu-id="f4b00-120">Konfigurace vaší aplikace tooaccess úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="f4b00-120">Configure your application tooaccess Blob storage</span></span>
<span data-ttu-id="f4b00-121">Přidejte hello následující import příkazy toohello horní části souboru Java hello místo, kam chcete objekty BLOB tooaccess toouse hello rozhraní API úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="f4b00-121">Add hello following import statements toohello top of hello Java file where you want toouse hello Azure Storage APIs tooaccess blobs.</span></span>

```java
// Include hello following imports toouse blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="f4b00-122">Nastavit připojovací řetězec službě Azure Storage</span><span class="sxs-lookup"><span data-stu-id="f4b00-122">Set up an Azure Storage connection string</span></span>
<span data-ttu-id="f4b00-123">Klienta Azure Storage používá úložiště připojovací řetězec toostore koncových bodů a přihlašovací údaje pro přístup ke službám dat správy.</span><span class="sxs-lookup"><span data-stu-id="f4b00-123">An Azure Storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="f4b00-124">Když spustíte v aplikaci klienta, musíte zadat připojovací řetězec úložiště hello v hello formátu, pomocí hello názvu účtu úložiště a hello primární přístupový klíč pro účet úložiště hello uvedené v hello [portál Azure](https://portal.azure.com)pro hello *AccountName* a *AccountKey* hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f4b00-124">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello Primary access key for hello storage account listed in hello [Azure portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="f4b00-125">Hello následující příklad ukazuje, jak můžou deklarovat statické pole toohold hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="f4b00-125">hello following example shows how you can declare a static field toohold hello connection string.</span></span>

```java
// Define hello connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="f4b00-126">Ve spuštěné aplikace v rámci role ve službě Microsoft Azure, můžete tento řetězec uložené v konfiguračním souboru služby hello, *souboru ServiceConfiguration.cscfg*a je přístupný pomocí volání toohello  **RoleEnvironment.getConfigurationSettings** metoda.</span><span class="sxs-lookup"><span data-stu-id="f4b00-126">In an application running within a role in Microsoft Azure, this string can be stored in hello service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call toohello **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="f4b00-127">Hello následující příklad načte hello připojovacího řetězce z **nastavení** element s názvem *StorageConnectionString* v konfiguračním souboru služby hello.</span><span class="sxs-lookup"><span data-stu-id="f4b00-127">hello following example gets hello connection string from a **Setting** element named *StorageConnectionString* in hello service configuration file.</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="f4b00-128">Hello následující ukázky předpokládejme použít jednu z těchto dvou metod tooget hello úložiště připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="f4b00-128">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="f4b00-129">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="f4b00-129">Create a container</span></span>
<span data-ttu-id="f4b00-130">A **CloudBlobClient** objektu umožňuje získat odkaz na objekty pro kontejnery a objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="f4b00-130">A **CloudBlobClient** object lets you get reference objects for containers and blobs.</span></span> <span data-ttu-id="f4b00-131">Hello následující kód vytvoří **CloudBlobClient** objektu.</span><span class="sxs-lookup"><span data-stu-id="f4b00-131">hello following code creates a **CloudBlobClient** object.</span></span>

> [!NOTE]
> <span data-ttu-id="f4b00-132">Existují další způsoby toocreate **CloudStorageAccount** objekty; Další informace najdete v tématu **CloudStorageAccount** v hello [odkaz SDK klienta úložiště Azure].</span><span class="sxs-lookup"><span data-stu-id="f4b00-132">There are additional ways toocreate **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in hello [Azure Storage Client SDK Reference].</span></span>
>
>

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="f4b00-133">Použití hello **CloudBlobClient** objektu tooget odkaz na kontejner toohello chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="f4b00-133">Use hello **CloudBlobClient** object tooget a reference toohello container you want toouse.</span></span> <span data-ttu-id="f4b00-134">Můžete vytvořit kontejner hello, pokud neexistuje s hello **createIfNotExists** metodu, která v opačném případě vrátí hello existující kontejner.</span><span class="sxs-lookup"><span data-stu-id="f4b00-134">You can create hello container if it doesn't exist with hello **createIfNotExists** method, which will otherwise return hello existing container.</span></span> <span data-ttu-id="f4b00-135">Ve výchozím nastavení, je hello nový kontejner privátní, takže musíte zadat přístupový klíč k úložišti (stejně jako dříve) toodownload objekty BLOB z tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="f4b00-135">By default, hello new container is private, so you must specify your storage access key (as you did earlier) toodownload blobs from this container.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Get a reference tooa container.
    // hello container name must be lower case
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Create hello container if it does not exist.
    container.createIfNotExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

### <a name="optional-configure-a-container-for-public-access"></a><span data-ttu-id="f4b00-136">Volitelné: Konfigurace kontejner pro veřejný přístup</span><span class="sxs-lookup"><span data-stu-id="f4b00-136">Optional: Configure a container for public access</span></span>
<span data-ttu-id="f4b00-137">Kontejneru oprávnění jsou ve výchozím nastavení nakonfigurované pro soukromý přístup, ale můžete snadno nakonfigurovat kontejneru oprávnění tooallow veřejné, jen pro čtení přístup pro všechny uživatele na hello Internet:</span><span class="sxs-lookup"><span data-stu-id="f4b00-137">A container's permissions are configured for private access by default, but you can easily configure a container's permissions tooallow public, read-only access for all users on hello Internet:</span></span>

```java
// Create a permissions object.
BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

// Include public access in hello permissions object.
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

// Set hello permissions on hello container.
container.uploadPermissions(containerPermissions);
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="f4b00-138">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="f4b00-138">Upload a blob into a container</span></span>
<span data-ttu-id="f4b00-139">tooupload soubor tooa objekt blob získejte odkaz na kontejner a použít ho tooget odkaz na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="f4b00-139">tooupload a file tooa blob, get a container reference and use it tooget a blob reference.</span></span> <span data-ttu-id="f4b00-140">Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud voláním nahrávání na odkaz na objekt blob hello.</span><span class="sxs-lookup"><span data-stu-id="f4b00-140">Once you have a blob reference, you can upload any stream by calling upload on hello blob reference.</span></span> <span data-ttu-id="f4b00-141">Tato operace vytvoří objekt blob hello, pokud není neexistuje nebo ho přepíše, pokud ji nemá.</span><span class="sxs-lookup"><span data-stu-id="f4b00-141">This operation will create hello blob if it doesn't exist, or overwrite it if it does.</span></span> <span data-ttu-id="f4b00-142">Následující ukázka kódu Hello to znázorňuje a předpokládá, že tento kontejner hello již byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="f4b00-142">hello following code sample shows this, and assumes that hello container has already been created.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Define hello path tooa local file.
    final String filePath = "C:\\myimages\\myimage.jpg";

    // Create or overwrite hello "myimage.jpg" blob with contents from a local file.
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");
    File source = new File(filePath);
    blob.upload(new FileInputStream(source), source.length());
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="f4b00-143">Seznam hello objekty BLOB v kontejneru</span><span class="sxs-lookup"><span data-stu-id="f4b00-143">List hello blobs in a container</span></span>
<span data-ttu-id="f4b00-144">toolist hello objekty BLOB v kontejneru, nejdřív získejte odkaz na kontejner, stejně jako tooupload objekt blob.</span><span class="sxs-lookup"><span data-stu-id="f4b00-144">toolist hello blobs in a container, first get a container reference like you did tooupload a blob.</span></span> <span data-ttu-id="f4b00-145">Můžete použít hello kontejneru **listBlobs** metoda s **pro** smyčky.</span><span class="sxs-lookup"><span data-stu-id="f4b00-145">You can use hello container's **listBlobs** method with a **for** loop.</span></span> <span data-ttu-id="f4b00-146">Hello následující kód výstupy hello Uri jednotlivých objektů blob v kontejneru toohello konzole.</span><span class="sxs-lookup"><span data-stu-id="f4b00-146">hello following code outputs hello Uri of each blob in a container toohello console.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop over blobs within hello container and output hello URI tooeach of them.
    for (ListBlobItem blobItem : container.listBlobs()) {
        System.out.println(blobItem.getUri());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="f4b00-147">Všimněte si, že můžete pojmenovat objekty BLOB, informace o cestě jejich názvy.</span><span class="sxs-lookup"><span data-stu-id="f4b00-147">Note that you can name blobs with path information in their names.</span></span> <span data-ttu-id="f4b00-148">Tím se vytvoří struktura virtuálního adresáře, kterou můžete organizovat a měnit podle potřeby jako u tradičních systémů souborů.</span><span class="sxs-lookup"><span data-stu-id="f4b00-148">This creates a virtual directory structure that you can organize and traverse as you would a traditional file system.</span></span> <span data-ttu-id="f4b00-149">Všimněte si, že hello adresářovou strukturu je pouze virtuální – hello pouze prostředky k dispozici v úložišti objektů Blob jsou kontejnery a objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="f4b00-149">Note that hello directory structure is virtual only - hello only resources available in Blob storage are containers and blobs.</span></span> <span data-ttu-id="f4b00-150">Však nabízí hello klientské knihovny **CloudBlobDirectory** objektu toorefer tooa virtuální adresář a zjednodušit proces hello práce s objekty BLOB, které jsou tímto způsobem uspořádány.</span><span class="sxs-lookup"><span data-stu-id="f4b00-150">However, hello client library offers a **CloudBlobDirectory** object toorefer tooa virtual directory and simplify hello process of working with blobs that are organized in this way.</span></span>

<span data-ttu-id="f4b00-151">Například můžete mít kontejner s názvem "fotografie", ve kterých může odeslat objekty BLOB s názvem "rootphoto1", "2010 nebo photo1", "2010 nebo photo2" a "2011/photo1".</span><span class="sxs-lookup"><span data-stu-id="f4b00-151">For example, you could have a container named "photos", in which you might upload blobs named "rootphoto1", "2010/photo1", "2010/photo2", and "2011/photo1".</span></span> <span data-ttu-id="f4b00-152">To by vytvořit hello virtuální adresáře "2010" a "2011" hello "fotografie" kontejneru.</span><span class="sxs-lookup"><span data-stu-id="f4b00-152">This would create hello virtual directories "2010" and "2011" within hello "photos" container.</span></span> <span data-ttu-id="f4b00-153">Při volání **listBlobs** na kontejneru "fotografie" hello, bude obsahovat hello kolekce vrátil **CloudBlobDirectory** a **CloudBlob** objekty, které představují hello adresáře a objekty BLOB obsažené na nejvyšší úrovni hello.</span><span class="sxs-lookup"><span data-stu-id="f4b00-153">When you call **listBlobs** on hello "photos" container, hello collection returned will contain **CloudBlobDirectory** and **CloudBlob** objects representing hello directories and blobs contained at hello top level.</span></span> <span data-ttu-id="f4b00-154">V takovém případě by byla vrácena adresáře "2010" a "2011", jakož i photo "rootphoto1".</span><span class="sxs-lookup"><span data-stu-id="f4b00-154">In this case, directories "2010" and "2011", as well as photo "rootphoto1" would be returned.</span></span> <span data-ttu-id="f4b00-155">Můžete použít hello **instanceof** operátor toodistinguish tyto objekty.</span><span class="sxs-lookup"><span data-stu-id="f4b00-155">You can use hello **instanceof** operator toodistinguish these objects.</span></span>

<span data-ttu-id="f4b00-156">Volitelně můžete předat parametry toohello **listBlobs** metoda s hello **useFlatBlobListing** tootrue sada parametrů.</span><span class="sxs-lookup"><span data-stu-id="f4b00-156">Optionally, you can pass in parameters toohello **listBlobs** method with hello **useFlatBlobListing** parameter set tootrue.</span></span> <span data-ttu-id="f4b00-157">Výsledkem bude každý objekt blob se vrátí, a to bez ohledu na adresář.</span><span class="sxs-lookup"><span data-stu-id="f4b00-157">This will result in every blob being returned, regardless of directory.</span></span> <span data-ttu-id="f4b00-158">Další informace najdete v tématu **CloudBlobContainer.listBlobs** v hello [odkaz SDK klienta úložiště Azure].</span><span class="sxs-lookup"><span data-stu-id="f4b00-158">For more information, see **CloudBlobContainer.listBlobs** in hello [Azure Storage Client SDK Reference].</span></span>

## <a name="download-a-blob"></a><span data-ttu-id="f4b00-159">Stažení objektu blob</span><span class="sxs-lookup"><span data-stu-id="f4b00-159">Download a blob</span></span>
<span data-ttu-id="f4b00-160">objekty BLOB toodownload, postupujte podle hello stejné kroky jako jste to udělali nahrát objekt blob v pořadí tooget odkaz na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="f4b00-160">toodownload blobs, follow hello same steps as you did for uploading a blob in order tooget a blob reference.</span></span> <span data-ttu-id="f4b00-161">V hello odesílání příklad volá nahrávání u objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="f4b00-161">In hello uploading example, you called upload on hello blob object.</span></span> <span data-ttu-id="f4b00-162">V následující ukázka hello, volat stažení tootransfer hello blob obsah tooa objektu stream, jako **FileOutputStream** používané toopersist hello blob tooa místního souboru.</span><span class="sxs-lookup"><span data-stu-id="f4b00-162">In hello following example, call download tootransfer hello blob contents tooa stream object such as a **FileOutputStream** that you can use toopersist hello blob tooa local file.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop through each blob item in hello container.
    for (ListBlobItem blobItem : container.listBlobs()) {
        // If hello item is a blob, not a virtual directory.
        if (blobItem instanceof CloudBlob) {
            // Download hello item and save it tooa file with hello same name.
            CloudBlob blob = (CloudBlob) blobItem;
            blob.download(new FileOutputStream("C:\\mydownloads\\" + blob.getName()));
        }
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob"></a><span data-ttu-id="f4b00-163">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="f4b00-163">Delete a blob</span></span>
<span data-ttu-id="f4b00-164">toodelete objekt blob, získání objektu blob odkazovat a volání **deleteIfExists**.</span><span class="sxs-lookup"><span data-stu-id="f4b00-164">toodelete a blob, get a blob reference, and call **deleteIfExists**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Retrieve reference tooa blob named "myimage.jpg".
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");

    // Delete hello blob.
    blob.deleteIfExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob-container"></a><span data-ttu-id="f4b00-165">Odstranit kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="f4b00-165">Delete a blob container</span></span>
<span data-ttu-id="f4b00-166">Nakonec toodelete kontejneru objektů blob, získání objektu blob odkaz na kontejner a volání **deleteIfExists**.</span><span class="sxs-lookup"><span data-stu-id="f4b00-166">Finally, toodelete a blob container, get a blob container reference, and call **deleteIfExists**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create hello blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference tooa previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Delete hello blob container.
    container.deleteIfExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a><span data-ttu-id="f4b00-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4b00-167">Next steps</span></span>
<span data-ttu-id="f4b00-168">Teď, když jste se naučili základy používání Blob storage hello, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště.</span><span class="sxs-lookup"><span data-stu-id="f4b00-168">Now that you've learned hello basics of Blob storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="f4b00-169">[Úložiště Azure SDK pro jazyk Java][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="f4b00-169">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="f4b00-170">[Referenční informace sady SDK úložiště Azure klienta][odkaz SDK klienta úložiště Azure]</span><span class="sxs-lookup"><span data-stu-id="f4b00-170">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="f4b00-171">[REST API pro Azure Storage][Azure Storage REST API]</span><span class="sxs-lookup"><span data-stu-id="f4b00-171">[Azure Storage REST API][Azure Storage REST API]</span></span>
* <span data-ttu-id="f4b00-172">[Blog týmu Azure Storage][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="f4b00-172">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

<span data-ttu-id="f4b00-173">Další informace najdete v tématu taky hello [středisko pro vývojáře Java](/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="f4b00-173">For more information, see also hello [Java Developer Center](/develop/java/).</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[odkaz SDK klienta úložiště Azure]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
