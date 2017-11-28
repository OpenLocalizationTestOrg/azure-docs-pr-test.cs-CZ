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
# <a name="how-toouse-blob-storage-from-php"></a><span data-ttu-id="30db9-103">Jak toouse objektu blob úložiště z PHP</span><span class="sxs-lookup"><span data-stu-id="30db9-103">How toouse blob storage from PHP</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="30db9-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="30db9-104">Overview</span></span>
<span data-ttu-id="30db9-105">Azure Blob storage je služba, která ukládá Nestrukturovaná data v cloudu hello jako objekty nebo objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="30db9-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="30db9-106">Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="30db9-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="30db9-107">Úložiště objektů blob je také odkazované tooas objektu úložiště.</span><span class="sxs-lookup"><span data-stu-id="30db9-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="30db9-108">Tento průvodce vám ukáže, jak služba objektů blob tooperform běžné scénáře s využitím hello Azure.</span><span class="sxs-lookup"><span data-stu-id="30db9-108">This guide shows you how tooperform common scenarios using hello Azure blob service.</span></span> <span data-ttu-id="30db9-109">Hello ukázky jsou napsané v jazyce PHP a používají hello [Azure SDK pro jazyk PHP][download].</span><span class="sxs-lookup"><span data-stu-id="30db9-109">hello samples are written in PHP and use hello [Azure SDK for PHP][download].</span></span> <span data-ttu-id="30db9-110">Hello pokryté scénáře zahrnují **odesílání**, **výpis**, **stahování**, a **odstraňování** objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="30db9-110">hello scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="30db9-111">Další informace o objekty BLOB najdete v tématu hello [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="30db9-111">For more information on blobs, see hello [Next steps](#next-steps) section.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="30db9-112">Vytvoření aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="30db9-112">Create a PHP application</span></span>
<span data-ttu-id="30db9-113">Hello jen požadavek pro vytvoření aplikace PHP, který přistupuje k službě Azure blob hello je hello odkazování tříd v hello Azure SDK pro jazyk PHP z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="30db9-113">hello only requirement for creating a PHP application that accesses hello Azure blob service is hello referencing of classes in hello Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="30db9-114">Můžete použít všechny toocreate nástroje pro vývoj aplikace, včetně Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="30db9-114">You can use any development tools toocreate your application, including Notepad.</span></span>

<span data-ttu-id="30db9-115">V této příručce používat funkce služby, které lze volat v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.</span><span class="sxs-lookup"><span data-stu-id="30db9-115">In this guide, you use service features, which can be called within a PHP application locally or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="30db9-116">Získání knihovny klienta Azure hello</span><span class="sxs-lookup"><span data-stu-id="30db9-116">Get hello Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-hello-blob-service"></a><span data-ttu-id="30db9-117">Konfigurace služby objektů blob hello tooaccess vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="30db9-117">Configure your application tooaccess hello blob service</span></span>
<span data-ttu-id="30db9-118">toouse hello objektů blob v Azure API služby, musíte:</span><span class="sxs-lookup"><span data-stu-id="30db9-118">toouse hello Azure blob service APIs, you need to:</span></span>

1. <span data-ttu-id="30db9-119">Referenční dokumentace hello automatického zavaděče soubor pomocí hello [require_once] příkaz, a</span><span class="sxs-lookup"><span data-stu-id="30db9-119">Reference hello autoloader file using hello [require_once] statement, and</span></span>
2. <span data-ttu-id="30db9-120">Referenční všechny třídy, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="30db9-120">Reference any classes you might use.</span></span>

<span data-ttu-id="30db9-121">Hello následující příklad ukazuje, jak tooinclude hello automatického zavaděče souboru a odkaz hello **ServicesBuilder** třídy.</span><span class="sxs-lookup"><span data-stu-id="30db9-121">hello following example shows how tooinclude hello autoloader file and reference hello **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="30db9-122">Hello příklady v tomto článku předpokládá, že jste nainstalovali hello PHP klientské knihovny pro Azure prostřednictvím autora.</span><span class="sxs-lookup"><span data-stu-id="30db9-122">hello examples in this article assume you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="30db9-123">Pokud jste nainstalovali hello knihovny ručně, je třeba tooreference hello `WindowsAzure.php` automatického zavaděče souboru.</span><span class="sxs-lookup"><span data-stu-id="30db9-123">If you installed hello libraries manually, you need tooreference hello `WindowsAzure.php` autoloader file.</span></span>
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="30db9-124">V následujících příkladech hello, hello `require_once` příkaz zobrazí vždy, ale pouze hello třídy potřebné pro tooexecute příklad hello odkazují.</span><span class="sxs-lookup"><span data-stu-id="30db9-124">In hello examples below, hello `require_once` statement will be shown always, but only hello classes necessary for hello example tooexecute are referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="30db9-125">Nastavit připojení k úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="30db9-125">Set up an Azure storage connection</span></span>
<span data-ttu-id="30db9-126">tooinstantiate klientem služby objektů blob v Azure, že máte platný připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="30db9-126">tooinstantiate an Azure blob service client, you must first have a valid connection string.</span></span> <span data-ttu-id="30db9-127">Formát Hello připojovací řetězec služby objektů blob hello je:</span><span class="sxs-lookup"><span data-stu-id="30db9-127">hello format for hello blob service connection string is:</span></span>

<span data-ttu-id="30db9-128">Pro přístup k službě za provozu:</span><span class="sxs-lookup"><span data-stu-id="30db9-128">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="30db9-129">Pro přístup k emulátoru úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="30db9-129">For accessing hello storage emulator:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="30db9-130">toocreate libovolného klienta služby Azure, budete potřebovat toouse hello **ServicesBuilder** třídy.</span><span class="sxs-lookup"><span data-stu-id="30db9-130">toocreate any Azure service client, you need toouse hello **ServicesBuilder** class.</span></span> <span data-ttu-id="30db9-131">Můžete:</span><span class="sxs-lookup"><span data-stu-id="30db9-131">You can:</span></span>

* <span data-ttu-id="30db9-132">Předat hello připojovací řetězec přímo tooit nebo</span><span class="sxs-lookup"><span data-stu-id="30db9-132">Pass hello connection string directly tooit or</span></span>
* <span data-ttu-id="30db9-133">Použití hello **CloudConfigurationManager (CCM)** toocheck více externí zdroje pro hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="30db9-133">Use hello **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="30db9-134">Ve výchozím nastavení je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="30db9-134">By default, it comes with support for one external source - environmental variables.</span></span>
  * <span data-ttu-id="30db9-135">Můžete přidat nové zdroje rozšířením hello **ConnectionStringSource** třídy.</span><span class="sxs-lookup"><span data-stu-id="30db9-135">You can add new sources by extending hello **ConnectionStringSource** class.</span></span>

<span data-ttu-id="30db9-136">Příklady hello podle zde uvedeného se předají hello připojovací řetězec, který přímo.</span><span class="sxs-lookup"><span data-stu-id="30db9-136">For hello examples outlined here, hello connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);
```

## <a name="create-a-container"></a><span data-ttu-id="30db9-137">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="30db9-137">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="30db9-138">A **BlobRestProxy** objektu umožňuje vytvářet kontejner objektů blob s hello **createContainer** metoda.</span><span class="sxs-lookup"><span data-stu-id="30db9-138">A **BlobRestProxy** object lets you create a blob container with hello **createContainer** method.</span></span> <span data-ttu-id="30db9-139">Při vytváření kontejneru, můžete nastavit možnosti hello kontejneru, ale to tak není povinný.</span><span class="sxs-lookup"><span data-stu-id="30db9-139">When creating a container, you can set options on hello container, but doing so is not required.</span></span> <span data-ttu-id="30db9-140">(následující hello příklad ukazuje, jak přistupovat k tooset hello kontejneru ovládacího prvku seznam (ACL) a metadata kontejneru.)</span><span class="sxs-lookup"><span data-stu-id="30db9-140">(hello example below shows how tooset hello container access control list (ACL) and container metadata.)</span></span>

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

<span data-ttu-id="30db9-141">Volání metody **setPublicAccess (PublicAccessType::CONTAINER\_a\_objektů BLOB)** díky hello kontejnerů a objektů blob dat, které jsou přístupné prostřednictvím anonymních požadavků.</span><span class="sxs-lookup"><span data-stu-id="30db9-141">Calling **setPublicAccess(PublicAccessType::CONTAINER\_AND\_BLOBS)** makes hello container and blob data accessible via anonymous requests.</span></span> <span data-ttu-id="30db9-142">Volání metody **setPublicAccess(PublicAccessType::BLOBS_ONLY)** díky pouze blob dat, které jsou přístupné prostřednictvím anonymních požadavků.</span><span class="sxs-lookup"><span data-stu-id="30db9-142">Calling **setPublicAccess(PublicAccessType::BLOBS_ONLY)** makes only blob data accessible via anonymous requests.</span></span> <span data-ttu-id="30db9-143">Další informace o kontejneru seznamy řízení přístupu najdete v tématu [sadu kontejneru seznamu řízení přístupu (REST API)][container-acl].</span><span class="sxs-lookup"><span data-stu-id="30db9-143">For more information about container ACLs, see [Set container ACL (REST API)][container-acl].</span></span>

<span data-ttu-id="30db9-144">Další informace o chybových kódech služby objektů Blob najdete v tématu [kódy chyb služby objektů Blob][error-codes].</span><span class="sxs-lookup"><span data-stu-id="30db9-144">For more information about Blob service error codes, see [Blob Service Error Codes][error-codes].</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="30db9-145">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="30db9-145">Upload a blob into a container</span></span>
<span data-ttu-id="30db9-146">tooupload soubor jako objekt blob, použijte hello **BlobRestProxy -> createBlockBlob** metoda.</span><span class="sxs-lookup"><span data-stu-id="30db9-146">tooupload a file as a blob, use hello **BlobRestProxy->createBlockBlob** method.</span></span> <span data-ttu-id="30db9-147">Tato operace vytvoří objekt blob hello, pokud neexistuje, nebo ho přepíše, pokud k tomu.</span><span class="sxs-lookup"><span data-stu-id="30db9-147">This operation creates hello blob if it doesn't exist, or overwrites it if it does.</span></span> <span data-ttu-id="30db9-148">Hello následující příklad kódu předpokládá, kontejneru hello již byly vytvořeny a používá [fopen –] [ fopen] tooopen hello soubor jako datový proud.</span><span class="sxs-lookup"><span data-stu-id="30db9-148">hello code example below assumes that hello container has already been created and uses [fopen][fopen] tooopen hello file as a stream.</span></span>

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

<span data-ttu-id="30db9-149">Všimněte si, že hello předchozí ukázce odešle objekt blob jako datový proud.</span><span class="sxs-lookup"><span data-stu-id="30db9-149">Note that hello previous sample uploads a blob as a stream.</span></span> <span data-ttu-id="30db9-150">Také však nahrát objekt blob jako řetězec, například pomocí hello [soubor\_získat\_obsah] [ file_get_contents] funkce.</span><span class="sxs-lookup"><span data-stu-id="30db9-150">However, a blob can also be uploaded as a string using, for example, hello [file\_get\_contents][file_get_contents] function.</span></span> <span data-ttu-id="30db9-151">toodo tento pomocí předchozího vzorku hello, změnit `$content = fopen("c:\myfile.txt", "r");` příliš`$content = file_get_contents("c:\myfile.txt");`.</span><span class="sxs-lookup"><span data-stu-id="30db9-151">toodo this using hello previous sample, change `$content = fopen("c:\myfile.txt", "r");` too`$content = file_get_contents("c:\myfile.txt");`.</span></span>

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="30db9-152">Seznam hello objekty BLOB v kontejneru</span><span class="sxs-lookup"><span data-stu-id="30db9-152">List hello blobs in a container</span></span>
<span data-ttu-id="30db9-153">toolist hello objekty BLOB v kontejneru, použijte hello **BlobRestProxy -> listBlobs** metoda s **foreach** cykly tooloop prostřednictvím hello výsledků.</span><span class="sxs-lookup"><span data-stu-id="30db9-153">toolist hello blobs in a container, use hello **BlobRestProxy->listBlobs** method with a **foreach** loop tooloop through hello result.</span></span> <span data-ttu-id="30db9-154">Hello následující kód zobrazí název hello každý objekt blob jako výstup v kontejneru a jeho prohlížeče toohello identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="30db9-154">hello following code displays hello name of each blob as output in a container and displays its URI toohello browser.</span></span>

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

## <a name="download-a-blob"></a><span data-ttu-id="30db9-155">Stažení objektu blob</span><span class="sxs-lookup"><span data-stu-id="30db9-155">Download a blob</span></span>
<span data-ttu-id="30db9-156">toodownload objekt blob volání hello **BlobRestProxy -> getblob –** metoda pak volání hello **getContentStream** metodu hello výsledná **GetBlobResult** objektu.</span><span class="sxs-lookup"><span data-stu-id="30db9-156">toodownload a blob, call hello **BlobRestProxy->getBlob** method, then call hello **getContentStream** method on hello resulting **GetBlobResult** object.</span></span>

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

<span data-ttu-id="30db9-157">Poznámka: výše uvedené hello například získá objekt blob jako prostředek datového proudu (hello výchozí chování).</span><span class="sxs-lookup"><span data-stu-id="30db9-157">Note that hello example above gets a blob as a stream resource (hello default behavior).</span></span> <span data-ttu-id="30db9-158">Můžete však použít hello [datového proudu\_získat\_obsah] [ stream-get-contents] funkce tooconvert hello vrátil řetězec tooa datového proudu.</span><span class="sxs-lookup"><span data-stu-id="30db9-158">However, you can use hello [stream\_get\_contents][stream-get-contents] function tooconvert hello returned stream tooa string.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="30db9-159">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="30db9-159">Delete a blob</span></span>
<span data-ttu-id="30db9-160">toodelete objekt blob předat hello název kontejneru a název objektu blob příliš**BlobRestProxy -> deleteBlob**.</span><span class="sxs-lookup"><span data-stu-id="30db9-160">toodelete a blob, pass hello container name and blob name too**BlobRestProxy->deleteBlob**.</span></span>

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

## <a name="delete-a-blob-container"></a><span data-ttu-id="30db9-161">Odstranit kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="30db9-161">Delete a blob container</span></span>
<span data-ttu-id="30db9-162">Nakonec toodelete kontejner objektů blob předat název kontejneru hello příliš**BlobRestProxy -> deleteContainer**.</span><span class="sxs-lookup"><span data-stu-id="30db9-162">Finally, toodelete a blob container, pass hello container name too**BlobRestProxy->deleteContainer**.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="30db9-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="30db9-163">Next steps</span></span>
<span data-ttu-id="30db9-164">Teď, když jste se naučili základy hello hello služby objektů blob v Azure, použijte tyto odkazy toolearn o složitějších úlohách úložiště.</span><span class="sxs-lookup"><span data-stu-id="30db9-164">Now that you've learned hello basics of hello Azure blob service, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="30db9-165">Navštivte hello [blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)</span><span class="sxs-lookup"><span data-stu-id="30db9-165">Visit hello [Azure Storage team blog](http://blogs.msdn.com/b/windowsazurestorage/)</span></span>
* <span data-ttu-id="30db9-166">V tématu hello [příklad objekt blob bloku PHP](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).</span><span class="sxs-lookup"><span data-stu-id="30db9-166">See hello [PHP block blob example](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).</span></span>
* <span data-ttu-id="30db9-167">V tématu hello [příklad objekt blob stránky PHP](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).</span><span class="sxs-lookup"><span data-stu-id="30db9-167">See hello [PHP page blob example](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).</span></span>
* [<span data-ttu-id="30db9-168">Přenos dat pomocí příkazového řádku Azcopy hello</span><span class="sxs-lookup"><span data-stu-id="30db9-168">Transfer data with hello AzCopy Command-Line Utility</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

<span data-ttu-id="30db9-169">Další informace najdete v tématu taky hello [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="30db9-169">For more information, see also hello [PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
