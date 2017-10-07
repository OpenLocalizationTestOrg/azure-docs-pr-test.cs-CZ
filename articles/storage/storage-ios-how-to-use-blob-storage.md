---
title: aaaHow toouse Azure Blob storage z iOS | Microsoft Docs
description: "Ukládání nestrukturovaných dat v cloudu hello s Azure Blob storage (úložiště objektů)."
services: storage
documentationcenter: ios
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: df188021-86fc-4d31-a810-1b0e7bcd814b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: objective-c
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: 474c4263a4bfbd61bfa39e4fdb01ddd9c3829c77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-ios"></a>Jak toouse úložiště Blob z iOS
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Přehled
Tento článek vám ukáže, jak tooperform běžné scénáře s využitím Microsoft Azure Blob storage. Hello ukázky jsou napsané v jazyce Objective-C a používají hello [Klientská knihovna pro úložiště Azure pro iOS](https://github.com/Azure/azure-storage-ios). Hello pokryté scénáře zahrnují **odesílání**, **výpis**, **stahování**, a **odstraňování** objekty BLOB. Další informace o objekty BLOB najdete v tématu hello [další kroky](#next-steps) části. Můžete také stáhnout hello [ukázkovou aplikaci](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) najdete v části tooquickly hello využívání Azure Storage v aplikace pro iOS.

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-hello-azure-storage-ios-library-into-your-application"></a>Importovat knihovny iOS hello Azure Storage do vaší aplikace
Můžete importovat hello Azure Storage iOS knihovny do své aplikace buď pomocí hello [CocoaPod úložiště Azure](https://cocoapods.org/pods/AZSClient) nebo importem hello **Framework** souboru. CocoaPod je hello nedoporučuje způsobem, protože umožňuje jednodušší, ale import ze souboru framework hello je šetrnější pro existující projekt integrační hello knihovně.

toouse této knihovny, je nutné hello následující:
- iOS 8 +
- Xcode 7 +

## <a name="cocoapod"></a>CocoaPod
1. Pokud jste to ještě neudělali, [nainstalovat CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) ve vašem počítači otevřete okno terminálu a spuštěním hello následující příkaz
    
    ```shell   
    sudo gem install cocoapods
    ```

2. V dalším kroku v adresáři projektu hello (hello adresář obsahující souboru .xcodeproj), vytvořte nový soubor s názvem _Podfile_(bez přípony souboru). Přidejte následující too_Podfile_ hello a uložte.

    ```ruby
    platform :ios, '8.0'

    target 'TargetName' do
      pod 'AZSClient'
    end
    ```

3. V okně hello terminálu přejděte toohello adresáři projektu a spusťte hello následující příkaz

    ```shell    
    pod install
    ```

4. Pokud vaše .xcodeproj otevřít v Xcode, zavřete ji. V souboru projektu otevřete hello nově vytvořený adresář projektu, které budou mít hello .xcworkspace rozšíření. Toto je soubor hello, kterou budete pracujete z nyní na.

## <a name="framework"></a>Framework
Hello způsobem, který je toouse hello knihovně toobuild hello framework ručně:

1. První, stažení nebo klonování hello [úložišti azure-storage-ios](https://github.com/azure/azure-storage-ios).
2. Přejděte do *azure-storage-ios* -> *Lib* -> *Klientská knihovna pro úložiště Azure*a otevřete `AZSClient.xcodeproj` v Xcode.
3. V hello levého horního pro Xcode, změna hello active scheme z "Klientská knihovna pro úložiště Azure" příliš "Framework".
4. Sestavení projektu hello (⌘ + B). Tím se vytvoří `AZSClient.framework` soubor na pracovní ploše.

Potom můžete importovat soubor framework hello do vaší aplikace pomocí tohoto postupu hello následující:

1. Vytvoření nového projektu nebo otevře existující projekt v Xcode.
2. Přetažení hello `AZSClient.framework` do Xcode navigátoru projektů.
3. Vyberte *kopírovat položky v případě potřeby*a klikněte na *Dokončit*.
4. Klikněte na projekt v levém navigačním hello a klikněte na tlačítko hello *Obecné* karty v horní části hello hello projektu editoru.
5. V části hello *propojené architektury a knihovny* klikněte na tlačítko Přidat hello (+).
6. Vyhledejte v seznamu hello knihoven už poskytuje `libxml2.2.tbd` a přidejte ji tooyour projektu.

## <a name="import-hello-library"></a>Import hello knihovně 
```objc
// Include hello following import statement toouse blob APIs.
#import <AZSClient/AZSClient.h>
```

Pokud používáte Swift, bude nutné toocreate hlavičku přemostění a importovat < AZSClient/AZSClient.h > existuje:

1. Vytvořte soubor hlavičky `Bridging-Header.h`a přidejte hello výše příkaz importu.
2. Přejděte toohello *nastavení sestavení* kartě a vyhledejte *hlavičky přemostění jazyka Objective-C*.
3. Dvakrát klikněte na pole hello *hlavičky přemostění jazyka Objective-C* a přidejte soubor hlaviček tooyour hello cesta:`ProjectName/Bridging-Header.h`
4. Tooverify sestavení hello projektu (⌘ + B), který hello přemostění záhlaví byla zachyceny pomocí Xcode.
5. Začít používat hello knihovně přímo v žádném Swift souboru, není nutné pro příkazy pro import.

[!INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Asynchronní operace
> [!NOTE]
> Všechny metody, které provádět požadavek hello služby jsou asynchronní operace. V hello ukázky kódu najdete splnit tyto metody dokončení obslužná rutina. Kód dokončení hello obslužná rutina se spustí **po** hello požadavek je dokončen. Kód po dokončení hello obslužná rutina se spustí **při** probíhá požadavku hello.
> 
> 

## <a name="create-a-container"></a>Vytvoření kontejneru
Každý objekt blob v Azure Storage se musí nacházet v kontejneru. Hello následující příklad ukazuje, jak toocreate kontejner, nazývá *newcontainer*, ve vašem účtu úložiště, pokud ještě neexistuje. Když vyberete název vašeho kontejneru, být s vědomím hello názvy pravidel uvedených výše.

```objc
-(void)createContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

    // Create container in your Storage account if hello container doesn't already exist
    [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

Můžete potvrdit, že to funguje prohlížením hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) a ověřování, který *newcontainer* je v seznamu hello kontejnery pro váš účet úložiště.

## <a name="set-container-permissions"></a>Nastavte oprávnění kontejneru
Kontejneru oprávnění jsou nakonfigurovaná pro **privátní** přístup ve výchozím nastavení. Kontejnery však obsahují několik různých volbách pro kontejner přístupu:

* **Privátní**: kontejnerů a objektů blob dat může číst pouze vlastníka účtu hello.
* **Objekt BLOB**: data objektu Blob v tomto kontejneru mohou číst přes anonymní žádost, ale kontejneru data nejsou k dispozici. Klienty nelze vytvořit výčet objektů BLOB v kontejneru hello přes anonymní žádost.
* **Kontejner**: přes anonymní dotazy můžete číst data kontejnerů a objektů blob. Klienti, můžete vytvořit výčet objektů BLOB v kontejneru hello přes anonymní žádost, ale nemůže vytvořit výčet kontejnery v rámci účtu úložiště hello.

Hello následující příklad ukazuje, jak toocreate a kontejner s **kontejneru** přístupová oprávnění, které vám umožní přístup k veřejné, jen pro čtení pro všechny uživatele v hello Internetu:

```objc
-(void)createContainerWithPublicAccess{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create container in your Storage account if hello container doesn't already exist
    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektu blob do kontejneru
Jak je uvedeno v hello [koncepty služby objektů Blob](#blob-service-concepts) část, úložiště objektů Blob nabízí tři různé typy objektů blob: objekty BLOB bloků, doplňovací objekty BLOB a objekty BLOB stránky. Knihovna iOS Hello Azure Storage podporuje všechny tři typy objektů BLOB. Ve většině případů je objekt blob bloku hello doporučená toouse typu.

Hello následující příklad ukazuje, jak z NSString tooupload blok objektů blob. Pokud objekt blob se stejným názvem již existuje v tomto kontejneru hello hello obsah tohoto objektu blob budou přepsány.

```objc
-(void)uploadBlobToContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
        {
            if (error){
                NSLog(@"Error in creating container.");
            }
            else{
                // Create a local blob object
                AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                // Upload blob tooStorage
                [blockBlob uploadFromText:@"This text will be uploaded tooBlob Storage." completionHandler:^(NSError *error) {
                    if (error){
                        NSLog(@"Error in creating blob.");
                    }
                }];
            }
        }];
}
```

Můžete potvrdit, že to funguje prohlížením hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) a ověření tohoto kontejneru hello *containerpublic*, obsahuje hello blob *sampleblob*. V této ukázce jsme použili kontejner veřejné, tak můžete také ověřit, zda tato aplikace funguje podle přejdete objekty BLOB toohello identifikátor URI:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Kromě toho toouploading objekt blob bloku z NSString podobné metody existují NSData, NSInputStream nebo místního souboru.

## <a name="list-hello-blobs-in-a-container"></a>Seznam hello objekty BLOB v kontejneru
Hello následující příklad ukazuje, jak toolist všechny objekty BLOB v kontejneru. Při provádění této operace, mělo pamatovat hello následující parametry:     

* **continuationToken** -hello tokenu představuje pokračování, kde by se měl spustit hello výpis operaci. Pokud je k dispozici žádné token, zobrazí seznam objektů BLOB od začátku hello. Libovolný počet objektů BLOB může být uvedený od nuly až tooa nastavit maximální. I když tato metoda vrátí hodnotu nula výsledky, pokud `results.continuationToken` není nulovou hodnotu, může být další objekty BLOB ve službě hello nejsou uvedené.
* **Předpona** -můžete určit předpona toouse hello seznam objektů blob. Pouze objekty BLOB, které začínají s touto předponou budou zobrazeny.
* **useFlatBlobListing** – jak je uvedeno v hello [pojmenování a odkazování na kontejnery a objekty BLOB](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) části, i když hello služby objektů Blob je schéma ploché úložiště, můžete vytvořit virtuální hierarchie názvy objektů BLOB s cestou informace. Výpis bez dvojrozměrném však není aktuálně podporováno. Tato funkce bude brzy k dispozici. Prozatím se tato hodnota by měla být **Ano**.

* **blobListingDetails** -tooinclude které položky můžete zadat při výpisu objektů BLOB
  * _AZSBlobListingDetailsNone_: seznam pouze potvrdit objektů BLOB a nevrátí metadata objektu blob.
  * _AZSBlobListingDetailsSnapshots_: seznam potvrdit objektů BLOB a objektů blob snímky.
  * _AZSBlobListingDetailsMetadata_: načíst metadata objektu blob pro každý objekt blob, vrátí se v seznamu hello.
  * _AZSBlobListingDetailsUncommittedBlobs_: seznam objektů BLOB nepotvrzené a potvrzené.
  * _AZSBlobListingDetailsCopy_: zahrnují kopírování vlastnosti v seznamu hello.
  * _AZSBlobListingDetailsAll_: seznam dostupných potvrdit objekty BLOB, nepotvrzené objekty BLOB a snímky a vrátí všechny metadata a zkopírujte stav pro tyto objekty BLOB.
* **shluku** -hello maximální počet výsledků tooreturn pro tuto operaci. Použití -1 toonot nastavit omezení.
* **completionHandler** -hello blok kódu tooexecute s výsledky hello hello výpis operaci.

V tomto příkladu je pomocná metoda použité toorecursively volání hello seznamu objektů BLOB metoda pokaždé, když se vrátí token pokračování.

```objc
-(void)listBlobsInContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    //List all blobs in container
    [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
        if (error != nil){
            NSLog(@"Error in creating container.");
        }
    }];
}

//List blobs helper method
-(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
{
    [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
        if (error)
        {
            completionHandler(error);
        }
        else
        {
            for (int i = 0; i < results.blobs.count; i++) {
                NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
            }
            if (results.continuationToken)
            {
                [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
            }
            else
            {
                completionHandler(nil);
            }
        }
    }];
}
```

## <a name="download-a-blob"></a>Stažení objektu blob
Následující příklad ukazuje, jak Hello toodownload objekt blob tooa NSString.

```objc
-(void)downloadBlobToString{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create a local blob object
    AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

    // Download blob
    [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
        if (error) {
            NSLog(@"Error in downloading blob");
        }
        else{
            NSLog(@"%@",text);
        }
    }];
}
```

## <a name="delete-a-blob"></a>Odstranění objektu blob
Následující příklad ukazuje, jak Hello toodelete objekt blob.

```objc
-(void)deleteBlob{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create a local blob object
    AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

    // Delete blob
    [blockBlob deleteWithCompletionHandler:^(NSError *error) {
        if (error) {
            NSLog(@"Error in deleting blob.");
        }
    }];
}
```

## <a name="delete-a-blob-container"></a>Odstranit kontejner objektů blob
Následující příklad ukazuje, jak Hello toodelete kontejner.

```objc
-(void)deleteContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Delete container
    [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
        if(error){
            NSLog(@"Error in deleting container");
        }
    }];
}
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili jak toouse úložiště objektů Blob z iOS, použijte tyto odkazy toolearn více o hello knihovně iOS a hello služby úložiště.

* [Azure Klientská knihovna pro úložiště pro iOS](https://github.com/azure/azure-storage-ios)
* [Azure Storage iOS referenční dokumentaci k nástroji](http://azure.github.io/azure-storage-ios/)
* [REST API služby Azure Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage)

Pokud máte dotazy týkající se této knihovny, klidně volné toopost tooour [fóru MSDN Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) nebo [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Pokud máte návrhy na funkce pro Azure Storage, položit příliš[zpětnou vazbu úložiště Azure](https://feedback.azure.com/forums/217298-storage/).

