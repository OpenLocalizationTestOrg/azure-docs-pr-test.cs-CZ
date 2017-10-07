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
ms.openlocfilehash: be71a946604da8af0130f101f2eb6135c5e08abd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-java"></a>Vývoj pro Azure File storage s Javou
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="about-this-tutorial"></a>O tomto kurzu
V tomto kurzu se ukazují hello základy používání Java toodevelop aplikacím nebo službám, které používají Azure File storage toostore souborová data. V tomto kurzu bude vytvoření jednoduché konzolové aplikace a zobrazit jak tooperform základní operace s Java a Azure File storage:

* Vytvářet a odstraňovat sdílené složky Azure File
* Vytvářet a odstraňovat adresáře
* Vytvoření výčtu souborů a adresářů v Azure File sdílet
* Odesílání, stahování a odstranění souboru

> [!Note]  
> Protože Azure File storage můžete získat přístup přes protokol SMB, je možné toowrite jednoduché aplikace, které přístup hello Azure souborové složce přes standardní třídy Java vstupně-výstupních operací hello. Tento článek popisuje, jak toowrite aplikace, které používají hello Java SDK úložiště Azure, kterou používá hello [REST API služby Azure File storage](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure úložiště File.

## <a name="create-a-java-application"></a>Vytvoření aplikace Java
Ukázky hello toobuild, budete potřebovat hello Java Development Kit (JDK) a [hello [Azure SDK úložiště pro jazyk Java]]. Musí také jste vytvořili účet úložiště Azure.

## <a name="setup-your-application-toouse-azure-file-storage"></a>Instalační program toouse vaše aplikace Azure File storage
toouse hello úložiště Azure rozhraní API, přidejte následující příkaz toohello horní části souboru Java hello, kde chcete tooaccess hello úložiště služby z hello.

```java
// Include hello following imports toouse blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="setup-an-azure-storage-connection-string"></a>Instalační program připojovací řetězec úložiště Azure
toouse Azure File storage, budete potřebovat účet úložiště Azure tooyour tooconnect. Hello prvním krokem by tooconfigure připojovací řetězec, který použijeme účet úložiště tooyour tooconnect. Umožňuje definovat statické proměnné toodo který.

```java
// Configure hello connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> Nahraďte your_storage_account_name a your_storage_account_key hello skutečnými hodnotami pro váš účet úložiště.
> 
> 

## <a name="connecting-tooan-azure-storage-account"></a>Připojení tooan účtu úložiště Azure
účet úložiště tooyour tooconnect, budete potřebovat toouse hello **CloudStorageAccount** objekt, předávání připojovací řetězec tooits **analyzovat** metoda.

```java
// Use hello CloudStorageAccount object tooconnect tooyour storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle hello exception
}
```

**CloudStorageAccount.parse** vyvolává InvalidKeyException, budete potřebovat tooput ho uvnitř try/catch blokovat.

## <a name="create-an-azure-file-share"></a>Vytvoření Azure sdílené složky
Všechny soubory a adresáře v úložišti Azure File jsou umístěny v kontejneru názvem **sdílené složky**. Váš účet úložiště může mít tolik sdílených složek jako umožňuje kapacitě vašeho účtu. tooobtain přístup tooa sdílenou složku a její obsah, je nutné toouse klienta úložiště Azure File.

```java
// Create hello Azure File storage client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

Pomocí klienta aplikace hello Azure File storage můžete získat, sdílenou složku tooa odkaz.

```java
// Get a reference toohello file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

Vytvořte sdílenou složku hello tooactually, použijte hello **createIfNotExists** metodu objektu CloudFileShare hello.

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

V tomto okamžiku **sdílet** obsahuje odkaz na tooa sdílenou složku s názvem **sampleshare**.

## <a name="delete-an-azure-file-share"></a>Odstranit sdílenou složku Azure
Odstranění sdílené složky se provádí volání hello **deleteIfExists** metoda CloudFileShare objektu. Tady je ukázkový kód, který nemá.

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

## <a name="create-a-directory"></a>Vytvoření adresáře
Úložiště můžete navíc uspořádat umístěním souborů v podadresářích místo nutnosti všechny z nich v kořenovém adresáři hello. Úložiště Azure File umožňuje toocreate jako mnoho adresářů jako váš účet se povolit. Následující kód Hello vytvoří adresář s názvem **sampledir** pod kořenovým adresářem hello.

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

## <a name="delete-a-directory"></a>Odstranění adresáře
Odstranění adresáře je docela jednoduché úlohy, i když je potřeba poznamenat, že nelze odstranit adresář, který ještě obsahuje soubory nebo ostatních adresářů.

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

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Vytvoření výčtu souborů a adresářů v Azure File sdílet
Získání seznamu souborů a adresářů v rámci sdílené složky se snadno provádí voláním **listFilesAndDirectories** na CloudFileDirectory odkaz. Hello metoda vrátí seznam hodnot ListFileItem objekty, které můžete iterovat na. Jako příklad hello následující kód zobrazí seznam souborů a adresářů v kořenovém adresáři hello.

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="upload-a-file"></a>Nahrání souboru
Soubor s Azure, který obsahuje sdílenou složku v hello velmi alespoň, kořenový adresář, kde mohou být uloženy soubory. V této části se dozvíte, jak tooupload soubor z místního úložiště do hello kořenový adresář sdílené složky.

Hello prvním krokem při nahrání souboru je tooobtain adresář toohello odkaz kde by měl být umístěn. To se provádí volání hello **getRootDirectoryReference** metodu objektu hello sdílené složky.

```java
//Get a reference toohello root directory for hello share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

Teď, když máte odkaz toohello kořenovým adresářem sdílené složky hello, můžete nahrát soubor do pomocí hello následující kód.

```java
        // Define hello path tooa local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="download-a-file"></a>Stažení souboru
Jedním z hello další časté operace, které je potřeba provést před Azure File storage je toodownload soubory. V následujícím příkladu hello kód hello stáhne SampleFile.txt a zobrazí její obsah.

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

## <a name="delete-a-file"></a>Odstranění souboru
Další běžné operace úložiště Azure File je odstranění souborů. Hello následující kód odstraní soubor s názvem SampleFile.txt uložený v adresáři s názvem **sampledir**.

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

## <a name="next-steps"></a>Další kroky
Pokud vás zajímají další informace o dalších úložiště Azure rozhraní API toolearn, použijte tyto odkazy.

* [Azure pro vývojáře v jazyce Java](/java/azure)nebo)
* [Úložiště Azure SDK pro jazyk Java](https://github.com/azure/azure-storage-java)
* [Úložiště Azure SDK pro Android](https://github.com/azure/azure-storage-android)
* [Odkaz na sadu SDK klienta úložiště Azure](http://dl.windowsazure.com/storage/javadoc/)
* [REST API služby Azure Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)
* [Přenos dat pomocí příkazového řádku Azcopy hello](../common/storage-use-azcopy.md* [Troubleshooting Azure File storage problems - Windows](storage-troubleshoot-windows-file-connection-problems.md)
)