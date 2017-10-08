---
title: "aaaGet začít s úložiště objektů blob a Visual Studio připojených služeb (ASP.NET Core) | Microsoft Docs"
description: "Po vytvoření účtu úložiště pomocí sady Visual Studio pomocí úložiště objektů Blob v Azure v projektu Visual Studio ASP.NET Core způsob spuštění tooget připojení služby"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 8eedf331896b21658c7b30a68a4391d8d60cd729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Začínáme s Azure Blob storage a Visual Studio připojené služby (ASP.NET Core)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Přehled
Tento článek popisuje, jak tooget spuštění pomocí úložiště objektů Blob v Azure v sadě Visual Studio, po vytvoření a odkazuje pomocí dialogu Visual Studio přidat připojení služby hello účet úložiště Azure v projektu ASP.NET Core.

Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, který lze přistupovat z libovolné místo v hello, world pomocí protokolů HTTP nebo HTTPS. Jediného objektu blob může mít libovolnou velikost. Objekty BLOB může být třeba bitové kopie, soubory audia a videa, nezpracovaná data a soubory dokumentu. Tento článek popisuje, jak tooget spustí pomocí úložiště objektů blob, po vytvoření účtu úložiště Azure pomocí sady Visual Studio hello **přidat připojení služby** dialogové okno v projektu ASP.NET Core.

Stejně jako soubory live ve složkách, za provozu v kontejnerech objektů BLOB storage. Po vytvoření úložiště vytvoříte jeden nebo více kontejnerů v úložišti hello. Například v úložiště, který se nazývá "Výstřižků", vytvoříte kontejnerů v úložišti hello názvem "Image" toostore obrázky a jiné názvem "zvuk" toostore zvukových souborů. Po vytvoření kontejnerů hello můžete nahrát toothem soubory jednotlivých objektů blob. V tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) Další informace o programu manipulace s objekty BLOB.

## <a name="access-blob-containers-in-code"></a>Kontejnery objektů blob přístup v kódu
tooprogrammatically přístup k objektům BLOB projektů ASP.NET Core, musíte tooadd hello následující položky, pokud nejsou již existuje.

1. Přidejte následující kód obor názvů deklarace toohello horní části žádné C# soubor, ve kterém chcete tooprogrammatically přístup úložiště Azure hello.
   
        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;
2. Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použijte následující kód tooget hello připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure hello.
   
         CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
            "<storage-account-name>",
            "<access-key>"), true);
   
    **Poznámka:** používat všechny hello výše kódu před hello kódu v následující části hello.
3. Použití **CloudBlobClient** objektu tooget **CloudBlobContainer** odkaz tooan existující kontejneru v účtu úložiště.
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
   
        // Get a reference tooa container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

## <a name="create-a-container-in-code"></a>Vytvořit kontejner v kódu
Můžete taky hello **CloudBlobClient** toocreate kontejneru v účtu úložiště. Stačí toodo tooadd volání je příliš**CreateIfNotExistsAsync** jako hello následující kód:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference tooa container named "my-new-container."
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


**Poznámka:** hello rozhraní API, která provádět volání tooAzure úložiště v ASP.NET Core jsou asynchronní. V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace. Následující kód Hello předpokládá, že asynchronní programování metody jsou používány.

toomake hello soubory v rámci hello kontejneru dostupné tooeveryone, můžete nastavit kontejner toobe hello veřejné pomocí hello následující kód.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektu blob do kontejneru
tooupload souboru blob do kontejneru, získejte odkaz na kontejner a použít ho tooget odkaz na objekt blob. Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud dat tooit pomocí volání hello **UploadFromStreamAsync** metoda. Tato operace vytvoří objekt blob hello, pokud ještě není, nebo ho přepíše, pokud neexistuje. Následující příklad ukazuje, jak Hello tooupload objekt blob do kontejneru a předpokládá, že hello kontejner byl již vytvořen.

    // Get a reference tooa blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite hello "myblob" blob with hello contents of a local file
    // named "myfile".
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

## <a name="list-hello-blobs-in-a-container"></a>Seznam hello objekty BLOB v kontejneru
toolist hello objekty BLOB v kontejneru, nejdřív získejte odkaz na kontejner. Potom můžete volat hello kontejneru **ListBlobsSegmentedAsync** metoda tooretrieve hello objekty BLOB a/nebo obsažené adresáře. tooaccess hello bohatou sadu vlastností a metod vrácené **IListBlobItem**, musíte vysílat tooa **CloudBlockBlob**, **CloudPageBlob**, nebo  **CloudBlobDirectory** objektu. Pokud neznáte typ blob hello, můžete použít typ kontroly toodetermine které toocast jeho. Hello následující kód ukazuje, jak tooretrieve a výstup hello URI pro každou položku v kontejneru.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
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
    } while (token != null);

Existují i další způsoby toolist hello obsah kontejneru objektů blob. V tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) Další informace.

## <a name="download-a-blob"></a>Stažení objektu blob
toodownload objekt blob, nejdřív získejte odkaz objektu blob toohello a pak zavolají hello **DownloadToStreamAsync** metoda. Hello následující příklad používá hello **DownloadToStreamAsync** metoda tootransfer hello blob obsah tooa datového proudu objekt, který pak můžete uložit jako místního souboru.

    // Get a reference tooa blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save hello blob contents tooa file named "myfile".
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Existují jiné způsoby objekty BLOB toosave jako soubory. V tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs) Další informace.

## <a name="delete-a-blob"></a>Odstranění objektu blob
toodelete objekt blob, nejdřív získejte odkaz objektu blob toohello a pak zavolají hello **DeleteAsync** metoda na něm.

    // Get a reference tooa blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete hello blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Další kroky
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

