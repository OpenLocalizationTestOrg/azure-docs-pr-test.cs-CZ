---
title: "úložiště objektů blob toouse (úložiště objektů) aaaHow z PHP | Microsoft Docs"
description: "Ukládání nestrukturovaných dat v cloudu hello s Azure Blob storage (úložiště objektů)."
documentationcenter: php
services: storage
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 1af56b59-b3f0-4b46-8441-aab463ae088e
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 2e77415519b38007652e3ea372da531b3a97c5d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-php"></a>Jak toouse objektu blob úložiště z PHP
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Přehled
Azure Blob storage je služba, která ukládá Nestrukturovaná data v cloudu hello jako objekty nebo objekty BLOB. Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Úložiště objektů blob je také odkazované tooas objektu úložiště.

Tento průvodce vám ukáže, jak služba objektů blob tooperform běžné scénáře s využitím hello Azure. Hello ukázky jsou napsané v jazyce PHP a používají hello [Azure SDK pro jazyk PHP][download]. Hello pokryté scénáře zahrnují **odesílání**, **výpis**, **stahování**, a **odstraňování** objekty BLOB. Další informace o objekty BLOB najdete v tématu hello [další kroky](#next-steps) části.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Vytvoření aplikace PHP
Hello jen požadavek pro vytvoření aplikace PHP, který přistupuje k službě Azure blob hello je hello odkazování tříd v hello Azure SDK pro jazyk PHP z vašeho kódu. Můžete použít všechny toocreate nástroje pro vývoj aplikace, včetně Poznámkový blok.

V této příručce používat funkce služby, které lze volat v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.

## <a name="get-hello-azure-client-libraries"></a>Získání knihovny klienta Azure hello
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-hello-blob-service"></a>Konfigurace služby objektů blob hello tooaccess vaší aplikace
toouse hello objektů blob v Azure API služby, musíte:

1. Referenční dokumentace hello automatického zavaděče soubor pomocí hello [require_once] příkaz, a
2. Referenční všechny třídy, které můžete použít.

Hello následující příklad ukazuje, jak tooinclude hello automatického zavaděče souboru a odkaz hello **ServicesBuilder** třídy.

> [!NOTE]
> Hello příklady v tomto článku předpokládá, že jste nainstalovali hello PHP klientské knihovny pro Azure prostřednictvím autora. Pokud jste nainstalovali hello knihovny ručně, je třeba tooreference hello `WindowsAzure.php` automatického zavaděče souboru.
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

V následujících příkladech hello, hello `require_once` příkaz zobrazí vždy, ale pouze hello třídy potřebné pro tooexecute příklad hello odkazují.

## <a name="set-up-an-azure-storage-connection"></a>Nastavit připojení k úložišti Azure
tooinstantiate klientem služby objektů blob v Azure, že máte platný připojovací řetězec. Formát Hello připojovací řetězec služby objektů blob hello je:

Pro přístup k službě za provozu:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Pro přístup k emulátoru úložiště hello:

```php
UseDevelopmentStorage=true
```

toocreate libovolného klienta služby Azure, budete potřebovat toouse hello **ServicesBuilder** třídy. Můžete:

* Předat hello připojovací řetězec přímo tooit nebo
* Použití hello **CloudConfigurationManager (CCM)** toocheck více externí zdroje pro hello připojovací řetězec:
  * Ve výchozím nastavení je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí.
  * Můžete přidat nové zdroje rozšířením hello **ConnectionStringSource** třídy.

Příklady hello podle zde uvedeného se předají hello připojovací řetězec, který přímo.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);
```

## <a name="create-a-container"></a>Vytvoření kontejneru
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

A **BlobRestProxy** objektu umožňuje vytvářet kontejner objektů blob s hello **createContainer** metoda. Při vytváření kontejneru, můžete nastavit možnosti hello kontejneru, ale to tak není povinný. (následující hello příklad ukazuje, jak přistupovat k tooset hello kontejneru ovládacího prvku seznam (ACL) a metadata kontejneru.)

```php
require_once 'vendor\autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


// OPTIONAL: Set public access policy and metadata.
// Create container options object.
$createContainerOptions = new CreateContainerOptions();

// Set public access policy. Possible values are
// PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
// CONTAINER_AND_BLOBS:
// Specifies full public read access for container and blob data.
// proxys can enumerate blobs within hello container via anonymous
// request, but cannot enumerate containers within hello storage account.
//
// BLOBS_ONLY:
// Specifies public read access for blobs. Blob data within this
// container can be read via anonymous request, but container data is not
// available. proxys cannot enumerate blobs within hello container via
// anonymous request.
// If this value is not specified in hello request, container data is
// private toohello account owner.
$createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

// Set container metadata.
$createContainerOptions->addMetaData("key1", "value1");
$createContainerOptions->addMetaData("key2", "value2");

try    {
    // Create container.
    $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Volání metody **setPublicAccess (PublicAccessType::CONTAINER\_a\_objektů BLOB)** díky hello kontejnerů a objektů blob dat, které jsou přístupné prostřednictvím anonymních požadavků. Volání metody **setPublicAccess(PublicAccessType::BLOBS_ONLY)** díky pouze blob dat, které jsou přístupné prostřednictvím anonymních požadavků. Další informace o kontejneru seznamy řízení přístupu najdete v tématu [sadu kontejneru seznamu řízení přístupu (REST API)][container-acl].

Další informace o chybových kódech služby objektů Blob najdete v tématu [kódy chyb služby objektů Blob][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektu blob do kontejneru
tooupload soubor jako objekt blob, použijte hello **BlobRestProxy -> createBlockBlob** metoda. Tato operace vytvoří objekt blob hello, pokud neexistuje, nebo ho přepíše, pokud k tomu. Hello následující příklad kódu předpokládá, kontejneru hello již byly vytvořeny a používá [fopen –] [ fopen] tooopen hello soubor jako datový proud.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


$content = fopen("c:\myfile.txt", "r");
$blob_name = "myblob";

try    {
    //Upload blob
    $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Všimněte si, že hello předchozí ukázce odešle objekt blob jako datový proud. Také však nahrát objekt blob jako řetězec, například pomocí hello [soubor\_získat\_obsah] [ file_get_contents] funkce. toodo tento pomocí předchozího vzorku hello, změnit `$content = fopen("c:\myfile.txt", "r");` příliš`$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-hello-blobs-in-a-container"></a>Seznam hello objekty BLOB v kontejneru
toolist hello objekty BLOB v kontejneru, použijte hello **BlobRestProxy -> listBlobs** metoda s **foreach** cykly tooloop prostřednictvím hello výsledků. Hello následující kód zobrazí název hello každý objekt blob jako výstup v kontejneru a jeho prohlížeče toohello identifikátor URI.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


try    {
    // List blobs.
    $blob_list = $blobRestProxy->listBlobs("mycontainer");
    $blobs = $blob_list->getBlobs();

    foreach($blobs as $blob)
    {
        echo $blob->getName().": ".$blob->getUrl()."<br />";
    }
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="download-a-blob"></a>Stažení objektu blob
toodownload objekt blob volání hello **BlobRestProxy -> getblob –** metoda pak volání hello **getContentStream** metodu hello výsledná **GetBlobResult** objektu.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


try    {
    // Get blob.
    $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
    fpassthru($blob->getContentStream());
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Poznámka: výše uvedené hello například získá objekt blob jako prostředek datového proudu (hello výchozí chování). Můžete však použít hello [datového proudu\_získat\_obsah] [ stream-get-contents] funkce tooconvert hello vrátil řetězec tooa datového proudu.

## <a name="delete-a-blob"></a>Odstranění objektu blob
toodelete objekt blob předat hello název kontejneru a název objektu blob příliš**BlobRestProxy -> deleteBlob**.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


try    {
    // Delete blob.
    $blobRestProxy->deleteBlob("mycontainer", "myblob");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-a-blob-container"></a>Odstranit kontejner objektů blob
Nakonec toodelete kontejner objektů blob předat název kontejneru hello příliš**BlobRestProxy -> deleteContainer**.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

try    {
    // Delete container.
    $blobRestProxy->deleteContainer("mycontainer");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello hello služby objektů blob v Azure, použijte tyto odkazy toolearn o složitějších úlohách úložiště.

* Navštivte hello [blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)
* V tématu hello [příklad objekt blob bloku PHP](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
* V tématu hello [příklad objekt blob stránky PHP](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
* [Přenos dat pomocí příkazového řádku Azcopy hello](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

Další informace najdete v tématu taky hello [středisku pro vývojáře PHP](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
