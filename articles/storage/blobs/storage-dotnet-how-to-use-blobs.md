---
title: "aaaGet začít s Azure Blob storage (úložiště objektů) pomocí rozhraní .NET | Microsoft Docs"
description: "Ukládání nestrukturovaných dat v cloudu hello s Azure Blob storage (úložiště objektů)."
services: storage
documentationcenter: .net
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: d18a8fc8-97cb-4d37-a408-a6f8107ea8b3
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/27/2017
ms.author: marsma
ms.openlocfilehash: 3df0cf14b69d85cdc2f62cc3c8b901be102fa026
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-using-net"></a>Začínáme s úložištěm Azure Blob pomocí rozhraní .NET

[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

Azure Blob storage je služba, která ukládá Nestrukturovaná data v cloudu hello jako objekty nebo objekty BLOB. Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Úložiště objektů blob je také odkazované tooas objektu úložiště.

### <a name="about-this-tutorial"></a>O tomto kurzu
Tento kurz ukazuje, jak kód toowrite .NET pro některé běžné scénáře s využitím úložiště objektů Blob v Azure. Mezi zahrnuté scénáře patří odesílání, výpis, stahování a odstraňování objektů blob.

**Požadavky:**

* [Microsoft Visual Studio](https://www.visualstudio.com/)
* [Klientská knihovna Azure Storage pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Azure Configuration Manager for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [Účet úložiště Azure](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Další ukázky
Další příklady použití Blob Storage najdete v článku [Začínáme s Azure Blob Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/). Stáhněte hello ukázkovou aplikaci a potom ho spusťte nebo procházet kód hello na Githubu.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a>Přidání direktiv using
Přidejte následující hello **pomocí** direktivy toohello začátek hello `Program.cs` souboru:

```csharp
using Microsoft.WindowsAzure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types
```

### <a name="parse-hello-connection-string"></a>Analyzovat hello připojovací řetězec
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-blob-service-client"></a>Vytvoření klienta služby objektů Blob hello
Hello **CloudBlobClient** třída umožňuje tooretrieve kontejnery a objekty BLOB uložené v úložišti objektů Blob. Tady je jedním ze způsobů toocreate hello služby klienta:

```csharp
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```
Teď je připraven toowrite kód, který načítá a zapisuje data tooBlob úložiště.

## <a name="create-a-container"></a>Vytvoření kontejneru
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Tento příklad ukazuje, jak toocreate kontejner, pokud ještě neexistuje:

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference tooa container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create hello container if it doesn't already exist.
container.CreateIfNotExists();
```

Ve výchozím nastavení je hello nový kontejner privátní, což znamená, že úložiště objektů BLOB přístup klíče toodownload z tohoto kontejneru musíte zadat. Pokud chcete soubory hello toomake v rámci dostupné tooeveryone hello kontejneru, můžete nastavit kontejner toobe hello veřejné pomocí hello následující kód:

```csharp
container.SetPermissions(
    new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });
```

Kdokoli na hello Internetu může vidět objekty BLOB ve veřejném kontejneru. Můžete ale upravit nebo odstranit pouze v případě, že máte přístupový klíč hello odpovídající účtu nebo sdílený přístupový podpis.

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektu blob do kontejneru
Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky.  Ve většině případů je objekt blob bloku hello doporučená toouse typu.

tooupload objekt blob bloku souboru tooa získejte odkaz na kontejner a použít ho tooget odkaz na objekt blob bloku. Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud dat tooit pomocí volání hello **UploadFromStream** metoda. Tato operace vytvoří hello blob, pokud nebyla dříve neexistuje, nebo ho přepíše, pokud neexistuje.

Následující příklad ukazuje, jak Hello tooupload objekt blob do kontejneru a předpokládá, že hello kontejner byl již vytvořen.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "myblob".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

// Create or overwrite hello "myblob" blob with contents from a local file.
using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
{
    blockBlob.UploadFromStream(fileStream);
}
```

## <a name="list-hello-blobs-in-a-container"></a>Seznam hello objekty BLOB v kontejneru
toolist hello objekty BLOB v kontejneru, nejdřív získejte odkaz na kontejner. Pak můžete použít hello kontejneru **ListBlobs** metoda tooretrieve hello objekty BLOB a/nebo obsažené adresáře. tooaccess hello bohatou sadu vlastností a metod vrácené **IListBlobItem**, musíte vysílat tooa **CloudBlockBlob**, **CloudPageBlob**, nebo  **CloudBlobDirectory** objektu. Pokud typ hello neznámý, můžete použít typ kontroly toodetermine které toocast jeho. Hello následující kód ukazuje, jak tooretrieve a výstup hello URI pro každou položku v hello _fotografie_ kontejneru:

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("photos");

// Loop over items within hello container and output hello length and URI.
foreach (IListBlobItem item in container.ListBlobs(null, false))
{
    if (item.GetType() == typeof(CloudBlockBlob))
    {
        CloudBlockBlob blob = (CloudBlockBlob)item;

        Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

    }
    else if (item.GetType() == typeof(CloudPageBlob))
    {
        CloudPageBlob pageBlob = (CloudPageBlob)item;

        Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

    }
    else if (item.GetType() == typeof(CloudBlobDirectory))
    {
        CloudBlobDirectory directory = (CloudBlobDirectory)item;

        Console.WriteLine("Directory: {0}", directory.Uri);
    }
}
```

Zahrnutím informací o cestě do názvu objektu blob můžete vytvořit virtuální adresářovou strukturu, kterou můžete organizovat a procházet podle potřeby jako u tradičních systémů souborů. struktura adresářů Hello je virtuální pouze – hello jenom prostředky, které jsou k dispozici v úložišti objektů Blob jsou kontejnery a objekty BLOB. Však nabízí hello Klientská knihovna pro úložiště **CloudBlobDirectory** objektu toorefer tooa virtuální adresář a zjednodušit proces hello práce s objekty BLOB, které jsou tímto způsobem uspořádány.

Zvažte například následující sadu objektů BLOB bloku v kontejneru nazvaném hello *fotografie*:

```
photo1.jpg
2010/architecture/description.txt
2010/architecture/photo3.jpg
2010/architecture/photo4.jpg
2011/architecture/photo5.jpg
2011/architecture/photo6.jpg
2011/architecture/description.txt
2011/photo7.jpg
```

Při volání **ListBlobs** na hello *fotografie* kontejneru (jako hello předchozím fragmentu kódu), se vrátí hierarchický výpis. Obsahuje oba **CloudBlobDirectory** a **CloudBlockBlob** objekty, která reprezentuje hello adresáře a objekty BLOB v kontejneru hello. Hello výsledný výstup vypadá takto:

```
Directory: https://<accountname>.blob.core.windows.net/photos/2010/
Directory: https://<accountname>.blob.core.windows.net/photos/2011/
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

Volitelně můžete nastavit hello **UseFlatBlobListing** parametr hello **ListBlobs** metodu **true**. V takovém případě se každý objekt blob v kontejneru hello vrátí jako **CloudBlockBlob** objektu. Hello volání příliš**ListBlobs** tooreturn plochým výpisem vypadá takto:

```csharp
// Loop over items within hello container and output hello length and URI.
foreach (IListBlobItem item in container.ListBlobs(null, true))
{
    ...
}
```

a výsledky hello vypadat takto:

```
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

## <a name="download-blobs"></a>Stáhnout objekty blob
toodownload objekty BLOB, nejdřív načtěte odkaz na objekt blob a pak zavolají hello **DownloadToStream** metoda. Hello následující příklad používá hello **DownloadToStream** metoda tootransfer hello blob obsah tooa objektu stream, potom můžete zachovat tooa místního souboru.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "photo1.jpg".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

// Save blob contents tooa file.
using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
{
    blockBlob.DownloadToStream(fileStream);
}
```

Můžete taky hello **DownloadToStream** metoda toodownload hello obsah objektu blob jako textový řetězec.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "myblob.txt"
CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

string text;
using (var memoryStream = new MemoryStream())
{
    blockBlob2.DownloadToStream(memoryStream);
    text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
}
```

## <a name="delete-blobs"></a>Odstranění objektů blob
toodelete objekt blob, nejdřív získejte odkaz na objekt blob a potom zavolejte **odstranit** metoda na něm.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "myblob.txt".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

// Delete hello blob.
blockBlob.Delete();
```

## <a name="list-blobs-in-pages-asynchronously"></a>Asynchronní zobrazení seznamu objektů blob na stránkách
Pokud provádíte výpis velkého počtu objektů BLOB nebo chcete toocontrol hello počet výsledků, které vrátíte v rámci jedné operace výpisu, můžete vytvořit seznam objektů blob na stránkách s výsledky. Tento příklad ukazuje, jak tooreturn výsledky na stránkách asynchronně, takže čekání tooreturn velké sady výsledků neblokovalo provádění.

Tento příklad ukazuje výpis plochého objektu blob ale můžete také provést hierarchický výpis podle nastavení hello _useFlatBlobListing_ parametr hello **ListBlobsSegmentedAsync** too_false_ metoda.

Protože hello metoda ukázky volá asynchronní metodu, musí být uvedena s hello _asynchronní_ – klíčové slovo a musí vrátit **úloh** objektu. await – klíčové slovo zadané pro hello Hello **ListBlobsSegmentedAsync** pozastaví spuštění metody ukázky hello až do dokončení úlohy vytváření seznamu hello.

```csharp
async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
{
    //List blobs toohello console window, with paging.
    Console.WriteLine("List blobs in pages:");

    int i = 0;
    BlobContinuationToken continuationToken = null;
    BlobResultSegment resultSegment = null;

    //Call ListBlobsSegmentedAsync and enumerate hello result segment returned, while hello continuation token is non-null.
    //When hello continuation token is null, hello last page has been returned and execution can exit hello loop.
    do
    {
        //This overload allows control of hello page size. You can return all remaining results by passing null for hello maxResults parameter,
        //or by calling a different overload.
        resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
        if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
        foreach (var blobItem in resultSegment.Results)
        {
            Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
        }
        Console.WriteLine();

        //Get hello continuation token.
        continuationToken = resultSegment.ContinuationToken;
    }
    while (continuationToken != null);
}
```

## <a name="writing-tooan-append-blob"></a>Zápis tooan připojit objektů blob
Doplňovací objekt blob je optimalizován pro operace připojení, například protokolování. Podobně jako objekt blob bloku doplňovací objekt blob se skládá z bloků, ale když přidáte nový objekt blob bloku připojení tooan, je vždy připojením toohello konec objektu blob hello. Existující blok v doplňovacím objektu blob se nedá aktualizovat ani odstranit. ID Hello bloku pro doplňovací objekt blob nejsou vystavená, protože jsou pro objekt blob bloku.

Každý blok v doplňovacím objektu blob může mít různou velikost, až tooa nesmí být delší než 4 MB volného místa, a doplňovací objekt blob může obsahovat maximálně 50 000 bloků. Hello maximální velikost doplňovacího objektu BLOB je proto něco větší než 195 GB (4 MB × 50 000 bloků).

Následující příklad Hello vytvoří nový doplňovací objekt blob a připojí některá data tooit simulaci jednoduché operace protokolování.

```csharp
//Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

//Create service client for credentialed access toohello Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference tooa container.
CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

//Create hello container if it does not already exist.
container.CreateIfNotExists();

//Get a reference tooan append blob.
CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

//Create hello append blob. Note that if hello blob already exists, hello CreateOrReplace() method will overwrite it.
//You can check whether hello blob exists tooavoid overwriting it by using CloudAppendBlob.Exists().
appendBlob.CreateOrReplace();

int numBlocks = 10;

//Generate an array of random bytes.
Random rnd = new Random();
byte[] bytes = new byte[numBlocks];
rnd.NextBytes(bytes);

//Simulate a logging operation by writing text data and byte data toohello end of hello append blob.
for (int i = 0; i < numBlocks; i++)
{
    appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
        DateTime.UtcNow, bytes[i], Environment.NewLine));
}

//Read hello append blob toohello console window.
Console.WriteLine(appendBlob.DownloadText());
```

V tématu [objekty BLOB bloku pochopení, objekty BLOB stránky a doplňovacích objektů blob](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) Další informace o hello rozdíly mezi hello tři typy objektů BLOB.

## <a name="managing-security-for-blobs"></a>Správa zabezpečení pro objekty blob
Ve výchozím nastavení Azure Storage zajišťuje ochranu dat omezením vlastníka účtu toohello přístupu, která je vlastníkem hello přístupových klíčů k účtu. Pokud potřebujete tooshare data objektů blob v účtu úložiště, je důležité toodo Ano, aniž by to ohrozilo zabezpečení hello klíče pro přístup k účtu. Navíc můžete šifrovat tooensure data objektů blob, který je zabezpečený přenos přes přenosu hello a ve službě Azure Storage.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-tooblob-data"></a>Řízení přístupu tooblob dat
Ve výchozím nastavení hello data objektů blob v účtu úložiště je dostupný pouze vlastník účtu toostorage. Ve výchozím nastavení ověřování požadavků na úložiště objektů Blob vyžaduje přístupový klíč účtu hello. Můžete ale taky toomake určitých objektů blob data k dispozici tooother uživatele. Máte dvě možnosti:

* **Anonymní přístup:** můžete nastavit kontejner nebo jeho objekty blob na veřejně dostupné pro anonymní přístup. V tématu [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](storage-manage-access-to-resources.md) Další informace.
* **Sdílené přístupové podpisy:** klientům můžete zajistit sdílený přístupový podpis (SAS), který poskytuje Delegovaný přístup tooa prostředků ve vašem účtu úložiště s oprávněními, které zadáte a v intervalu, který zadáte. Další informace najdete v tématu [Použití sdílených přístupových podpisů (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="encrypting-blob-data"></a>Šifrování dat objektů blob
Úložiště Azure podporuje šifrování dat objektů blob na hello klientovi i na serveru hello:

* **Šifrování na straně klienta:** hello Klientská knihovna pro úložiště pro .NET podporuje šifrování dat v rámci klientské aplikace před nahráním tooAzure úložiště a dešifrování dat při stahování toohello klienta. Hello knihovna také podporuje integraci s Azure Key Vault pro správu klíčů účtu úložiště. Další informace viz [Šifrování na straně klienta s .NET pro úložiště Microsoft Azure](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). Viz také [Kurz: Šifrování a dešifrování objektů blob v úložišti Microsoft Azure pomocí služby Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md).
* **Šifrování na straně serveru**: Úložiště Azure nyní podporuje šifrování na straně serveru. Viz [Šifrování služby Azure Storage Service pro Neaktivní uložená data (Náhled)](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy používání Blob storage hello, postupujte podle těchto odkazů toolearn Další.

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Explorer
* [Microsoft Azure Storage Explorer (MASE)](../../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.

### <a name="blob-storage-samples"></a>Ukázky Blob Storage
* [Začínáme s úložištěm Azure Blob Storage pomocí rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>Odkazy Blob storage
* [Klientská knihovna pro úložiště pro .NET – referenční informace](https://msdn.microsoft.com/library/azure/mt347887.aspx)
* [REST API – referenční informace](/rest/api/storageservices/azure-storage-services-rest-api-reference)

### <a name="conceptual-guides"></a>Koncepční vodítka
* [Přenos dat pomocí hello příkazového řádku azcopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Začínáme se službou File Storage for .NET](../files/storage-dotnet-how-to-use-files.md)
* [Jak toouse Azure blob storage s hello WebJobs SDK](../../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
