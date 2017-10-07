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
ms.openlocfilehash: a905d318abdaa7538ec3f6b53b5186b965b8b86e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-java"></a>Jak toouse úložiště Blob z Javy
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a>Přehled
Azure Blob storage je služba, která ukládá Nestrukturovaná data v cloudu hello jako objekty nebo objekty BLOB. Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Úložiště objektů blob je také odkazované tooas objektu úložiště.

Tento článek vám ukáže, jak hello tooperform běžné scénáře s využitím Microsoft Azure Blob storage. Hello ukázky jsou napsané v jazyce Java a používají hello [sada SDK úložiště Azure pro jazyk Java][Azure Storage SDK for Java]. Hello pokryté scénáře zahrnují **odesílání**, **výpis**, **stahování**, a **odstraňování** objekty BLOB. Další informace o objekty BLOB najdete v tématu hello [další kroky](#Next-Steps) části.

> [!NOTE]
> Sada SDK je k dispozici pro vývojáře, kteří na zařízení se systémem Android používají Azure Storage. Další informace najdete v tématu hello [sada SDK úložiště Azure pro Android][Azure Storage SDK for Android].
>
>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Vytvoření aplikace Java
V tomto článku budete používat funkce úložiště, které lze spustit v rámci aplikace Java místně nebo v kódu běžící v rámci webovou roli nebo role pracovního procesu v Azure.

toodo tedy budete potřebovat tooinstall hello Java Development Kit (JDK) a vytvoření účtu úložiště Azure ve vašem předplatném Azure. Jakmile provedete, budete potřebovat tooverify, který váš vývojový systém splňuje minimální požadavky hello a závislosti, které jsou uvedeny v hello [sada SDK úložiště Azure pro jazyk Java] [ Azure Storage SDK for Java] úložišti na Githubu. Pokud váš systém splňuje tyto požadavky, můžete postupovat podle pokynů hello ke stažení a instalaci hello knihovny úložiště Azure pro jazyk Java systému z tohoto úložiště. Po dokončení těchto úloh, bude možné toocreate aplikaci Java, která používá hello příklady v tomto článku.

## <a name="configure-your-application-tooaccess-blob-storage"></a>Konfigurace vaší aplikace tooaccess úložiště objektů Blob
Přidejte hello následující import příkazy toohello horní části souboru Java hello místo, kam chcete objekty BLOB tooaccess toouse hello rozhraní API úložiště Azure.

```java
// Include hello following imports toouse blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Nastavit připojovací řetězec službě Azure Storage
Klienta Azure Storage používá úložiště připojovací řetězec toostore koncových bodů a přihlašovací údaje pro přístup ke službám dat správy. Když spustíte v aplikaci klienta, musíte zadat připojovací řetězec úložiště hello v hello formátu, pomocí hello názvu účtu úložiště a hello primární přístupový klíč pro účet úložiště hello uvedené v hello [portál Azure](https://portal.azure.com)pro hello *AccountName* a *AccountKey* hodnoty. Hello následující příklad ukazuje, jak můžou deklarovat statické pole toohold hello připojovací řetězec.

```java
// Define hello connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

Ve spuštěné aplikace v rámci role ve službě Microsoft Azure, můžete tento řetězec uložené v konfiguračním souboru služby hello, *souboru ServiceConfiguration.cscfg*a je přístupný pomocí volání toohello  **RoleEnvironment.getConfigurationSettings** metoda. Hello následující příklad načte hello připojovacího řetězce z **nastavení** element s názvem *StorageConnectionString* v konfiguračním souboru služby hello.

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

Hello následující ukázky předpokládejme použít jednu z těchto dvou metod tooget hello úložiště připojovací řetězec.

## <a name="create-a-container"></a>Vytvoření kontejneru
A **CloudBlobClient** objektu umožňuje získat odkaz na objekty pro kontejnery a objekty BLOB. Hello následující kód vytvoří **CloudBlobClient** objektu.

> [!NOTE]
> Existují další způsoby toocreate **CloudStorageAccount** objekty; Další informace najdete v tématu **CloudStorageAccount** v hello [odkaz SDK klienta úložiště Azure].
>
>

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Použití hello **CloudBlobClient** objektu tooget odkaz na kontejner toohello chcete toouse. Můžete vytvořit kontejner hello, pokud neexistuje s hello **createIfNotExists** metodu, která v opačném případě vrátí hello existující kontejner. Ve výchozím nastavení, je hello nový kontejner privátní, takže musíte zadat přístupový klíč k úložišti (stejně jako dříve) toodownload objekty BLOB z tohoto kontejneru.

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

### <a name="optional-configure-a-container-for-public-access"></a>Volitelné: Konfigurace kontejner pro veřejný přístup
Kontejneru oprávnění jsou ve výchozím nastavení nakonfigurované pro soukromý přístup, ale můžete snadno nakonfigurovat kontejneru oprávnění tooallow veřejné, jen pro čtení přístup pro všechny uživatele na hello Internet:

```java
// Create a permissions object.
BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

// Include public access in hello permissions object.
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

// Set hello permissions on hello container.
container.uploadPermissions(containerPermissions);
```

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektu blob do kontejneru
tooupload soubor tooa objekt blob získejte odkaz na kontejner a použít ho tooget odkaz na objekt blob. Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud voláním nahrávání na odkaz na objekt blob hello. Tato operace vytvoří objekt blob hello, pokud není neexistuje nebo ho přepíše, pokud ji nemá. Následující ukázka kódu Hello to znázorňuje a předpokládá, že tento kontejner hello již byly vytvořeny.

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

## <a name="list-hello-blobs-in-a-container"></a>Seznam hello objekty BLOB v kontejneru
toolist hello objekty BLOB v kontejneru, nejdřív získejte odkaz na kontejner, stejně jako tooupload objekt blob. Můžete použít hello kontejneru **listBlobs** metoda s **pro** smyčky. Hello následující kód výstupy hello Uri jednotlivých objektů blob v kontejneru toohello konzole.

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

Všimněte si, že můžete pojmenovat objekty BLOB, informace o cestě jejich názvy. Tím se vytvoří struktura virtuálního adresáře, kterou můžete organizovat a měnit podle potřeby jako u tradičních systémů souborů. Všimněte si, že hello adresářovou strukturu je pouze virtuální – hello pouze prostředky k dispozici v úložišti objektů Blob jsou kontejnery a objekty BLOB. Však nabízí hello klientské knihovny **CloudBlobDirectory** objektu toorefer tooa virtuální adresář a zjednodušit proces hello práce s objekty BLOB, které jsou tímto způsobem uspořádány.

Například můžete mít kontejner s názvem "fotografie", ve kterých může odeslat objekty BLOB s názvem "rootphoto1", "2010 nebo photo1", "2010 nebo photo2" a "2011/photo1". To by vytvořit hello virtuální adresáře "2010" a "2011" hello "fotografie" kontejneru. Při volání **listBlobs** na kontejneru "fotografie" hello, bude obsahovat hello kolekce vrátil **CloudBlobDirectory** a **CloudBlob** objekty, které představují hello adresáře a objekty BLOB obsažené na nejvyšší úrovni hello. V takovém případě by byla vrácena adresáře "2010" a "2011", jakož i photo "rootphoto1". Můžete použít hello **instanceof** operátor toodistinguish tyto objekty.

Volitelně můžete předat parametry toohello **listBlobs** metoda s hello **useFlatBlobListing** tootrue sada parametrů. Výsledkem bude každý objekt blob se vrátí, a to bez ohledu na adresář. Další informace najdete v tématu **CloudBlobContainer.listBlobs** v hello [odkaz SDK klienta úložiště Azure].

## <a name="download-a-blob"></a>Stažení objektu blob
objekty BLOB toodownload, postupujte podle hello stejné kroky jako jste to udělali nahrát objekt blob v pořadí tooget odkaz na objekt blob. V hello odesílání příklad volá nahrávání u objektu blob hello. V následující ukázka hello, volat stažení tootransfer hello blob obsah tooa objektu stream, jako **FileOutputStream** používané toopersist hello blob tooa místního souboru.

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

## <a name="delete-a-blob"></a>Odstranění objektu blob
toodelete objekt blob, získání objektu blob odkazovat a volání **deleteIfExists**.

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

## <a name="delete-a-blob-container"></a>Odstranit kontejner objektů blob
Nakonec toodelete kontejneru objektů blob, získání objektu blob odkaz na kontejner a volání **deleteIfExists**.

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

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy používání Blob storage hello, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště.

* [Úložiště Azure SDK pro jazyk Java][Azure Storage SDK for Java]
* [Referenční informace sady SDK úložiště Azure klienta][odkaz SDK klienta úložiště Azure]
* [REST API pro Azure Storage][Azure Storage REST API]
* [Blog týmu Azure Storage][Azure Storage Team Blog]

Další informace najdete v tématu taky [Azure pro vývojáře v jazyce Java](/java/azure).

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[odkaz SDK klienta úložiště Azure]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
