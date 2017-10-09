---
title: "aaaGet začít s úložiště objektů blob a Visual Studio připojených služeb (cloudové služby) | Microsoft Docs"
description: "Jak tooget spustit po připojení tooa účet úložiště pomocí sady Visual Studio připojené služby používat úložiště objektů Blob v Azure v projektu cloudové služby v sadě Visual Studio"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 1144a958-f75a-4466-bb21-320b7ae8f304
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 62fb7fcff0a90008859ebe23755f13ef0555e380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Začínáme s Azure Blob Storage a Visual Studio připojené služby (projekty cloudových služeb)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Přehled
Tento článek popisuje, jak tooget pracovat s Azure Blob Storage po vytvoření nebo odkazovaný účet úložiště Azure pomocí sady Visual Studio hello **přidat připojení služby** dialogové okno v sadě Visual Studio cloudové služby projektu. Ukážeme vám jak tooaccess a vytvoření kontejnerů objektů blob a jak tooperform běžné úkoly jako odesílání, výpis a stahování objekty BLOB. Hello ukázky jsou napsané v jazyce C\# a používat hello [Microsoft Azure Storage Client Library pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Azure Blob Storage je služba pro ukládání velkého objemu nestrukturovaných dat, který lze přistupovat z libovolné místo v hello, world pomocí protokolů HTTP nebo HTTPS. Jediného objektu blob může mít libovolnou velikost. Objekty BLOB může být třeba bitové kopie, soubory audia a videa, nezpracovaná data a soubory dokumentu.

Stejně jako soubory live ve složkách, za provozu v kontejnerech objektů BLOB storage. Po vytvoření úložiště vytvoříte jeden nebo více kontejnerů v úložišti hello. Například v úložiště, který se nazývá "Výstřižků", vytvoříte kontejnerů v úložišti hello názvem "Image" toostore obrázky a jiné názvem "zvuk" toostore zvukových souborů. Po vytvoření kontejnerů hello můžete nahrát toothem soubory jednotlivých objektů blob.

* Další informace o programu manipulace s objekty BLOB najdete v tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md).
* Obecné informace o službě Azure Storage najdete v tématu [úložiště dokumentace](https://azure.microsoft.com/documentation/services/storage/).
* Obecné informace o Azure Cloud Services najdete v tématu [cloudové služby dokumentaci](https://azure.microsoft.com/documentation/services/cloud-services/).
* Další informace o programování aplikace ASP.NET najdete v tématu [ASP.NET](http://www.asp.net).

## <a name="access-blob-containers-in-code"></a>Kontejnery objektů blob přístup v kódu
tooprogrammatically přístup k objektům BLOB v projekty cloudových služeb, je nutné tooadd hello následující položky, pokud nejsou již existuje.

1. Přidejte následující kód obor názvů deklarace toohello horní části souboru žádné C# ve kterém chcete tooprogrammatically přístupu Azure Storage hello.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Následující kód tooget hello použití hello připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure hello.
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));
3. Získání **CloudBlobClient** objektu tooreference existující kontejner ve vašem účtu úložiště.
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
4. Získání **CloudBlobContainer** tooreference kontejner objektů blob konkrétní objekt.
   
        // Get a reference tooa container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [!NOTE]
> Použijte všechny hello kódu uvedené v předchozím postupu hello před uvedeném v následující části hello hello kódu.
> 
> 

## <a name="create-a-container-in-code"></a>Vytvořit kontejner v kódu
> [!NOTE]
> Některé rozhraní API, které provádět volání out tooAzure úložiště v ASP.NET jsou asynchronní. V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace. Hello kód v hello následující příklad předpokládá, že používáte asynchronní programování metody.
> 
> 

toocreate kontejneru v účtu úložiště, stačí toodo je přidejte volání příliš**CreateIfNotExistsAsync** jako hello následující kód:

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


toomake hello soubory v rámci hello kontejneru dostupné tooeveryone, můžete nastavit kontejner toobe hello veřejné pomocí hello následující kód.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Kdokoli na hello Internetu může vidět objekty BLOB ve veřejném kontejneru, ale můžete upravit nebo odstranit pouze v případě, že máte příslušný přístupový klíč hello.

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektu blob do kontejneru
Úložiště Azure podporuje objekty BLOB bloku a objekty BLOB stránky. Ve většině případů hello je objekt blob bloku hello doporučená toouse typu.

tooupload objekt blob bloku souboru tooa získejte odkaz na kontejner a použít ho tooget odkaz na objekt blob bloku. Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud dat tooit pomocí volání hello **UploadFromStream** metoda. Tato operace vytvoří hello blob, pokud nebyla dříve neexistuje, nebo ho přepíše, pokud neexistuje. Následující příklad ukazuje, jak Hello tooupload objekt blob do kontejneru a předpokládá, že hello kontejner byl již vytvořen.

    // Retrieve a reference tooa blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite hello "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-hello-blobs-in-a-container"></a>Seznam hello objekty BLOB v kontejneru
toolist hello objekty BLOB v kontejneru, nejdřív získejte odkaz na kontejner. Pak můžete použít hello kontejneru **ListBlobs** metoda tooretrieve hello objekty BLOB a/nebo obsažené adresáře. tooaccess hello bohatou sadu vlastností a metod vrácené **IListBlobItem**, musíte vysílat tooa **CloudBlockBlob**, **CloudPageBlob**, nebo  **CloudBlobDirectory** objektu. Pokud typ hello neznámý, můžete použít typ kontroly toodetermine které toocast jeho. Hello následující kód ukazuje, jak tooretrieve a výstup hello URI pro každou položku v hello **fotografie** kontejneru:

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

Jak ukazuje předchozí příklad hello, má služby objektů blob hello hello konceptu adresáře v rámci kontejnery také. Toto je tak, aby můžete uspořádat, objektů BLOB do více stromové struktury. Zvažte například následující sadu objektů BLOB bloku v kontejneru nazvaném hello **fotografie**:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Při volání **ListBlobs** hello kontejneru (jako v předchozím příkladu hello), obsahuje kolekci hello vrátil **CloudBlobDirectory** a **CloudBlockBlob** objekty představující hello adresáře a objekty BLOB obsažené na nejvyšší úrovni hello. Zde je výsledný výstup hello:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Volitelně můžete nastavit hello **UseFlatBlobListing** parametr z hello **ListBlobs** metodu **true**. Výsledkem je každý objekt blob se vrátí jako **CloudBlockBlob**, bez ohledu na to adresáře. Zde je hello volání příliš**ListBlobs**:

    // Loop over items within hello container and output hello length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

a tady jsou výsledky hello:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

Další informace najdete v tématu [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).

## <a name="download-blobs"></a>Stáhnout objekty blob
toodownload objekty BLOB, nejdřív načtěte odkaz na objekt blob a pak zavolají hello **DownloadToStream** metoda. Hello následující příklad používá hello **DownloadToStream** metoda tootransfer hello blob obsah tooa objektu stream, potom můžete zachovat tooa místního souboru.

    // Get a reference tooa blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents tooa file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Můžete taky hello **DownloadToStream** metoda toodownload hello obsah objektu blob jako textový řetězec.

    // Get a reference tooa blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Odstranění objektů blob
toodelete objekt blob, nejdřív získejte odkaz na objekt blob a potom zavolejte **odstranit** metoda.

    // Get a reference tooa blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete hello blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Asynchronní zobrazení seznamu objektů blob na stránkách
Pokud provádíte výpis velkého počtu objektů BLOB nebo chcete toocontrol hello počet výsledků, které vrátíte v rámci jedné operace výpisu, můžete vytvořit seznam objektů blob na stránkách s výsledky. Tento příklad ukazuje, jak tooreturn výsledky na stránkách asynchronně, takže čekání tooreturn velké sady výsledků neblokovalo provádění.

Tento příklad ukazuje výpis plochého objektu blob ale můžete také provést hierarchický výpis podle nastavení hello **useFlatBlobListing** parametr hello **ListBlobsSegmentedAsync** metoda příliš **false**.

Protože hello metoda ukázky volá asynchronní metodu, musí být uvedena s hello **asynchronní** – klíčové slovo a musí vrátit **úloh** objektu. await – klíčové slovo zadané pro hello Hello **ListBlobsSegmentedAsync** pozastaví spuštění metody ukázky hello až do dokončení úlohy vytváření seznamu hello.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs toohello console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate hello result segment returned, while hello continuation token is non-null.
        // When hello continuation token is null, hello last page has been returned and execution can exit hello loop.
        do
        {
            // This overload allows control of hello page size. You can return all remaining results by passing null for hello maxResults parameter,
            // or by calling a different overload.
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

## <a name="next-steps"></a>Další kroky
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

