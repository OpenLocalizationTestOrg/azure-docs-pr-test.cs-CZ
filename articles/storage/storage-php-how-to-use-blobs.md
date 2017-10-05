---
title: "Jak používat úložiště objektů blob (úložiště objektů) z PHP | Microsoft Docs"
description: "Ukládejte nestrukturovaná data v cloudu pomocí Azure Blob Storage (úložiště objektů)."
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
ms.openlocfilehash: 2c356d7faafa8ef4591087b5b1f949b9374732be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-blob-storage-from-php"></a><span data-ttu-id="2d4cf-103">Používání úložiště blob z PHP</span><span class="sxs-lookup"><span data-stu-id="2d4cf-103">How to use blob storage from PHP</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="2d4cf-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="2d4cf-104">Overview</span></span>
<span data-ttu-id="2d4cf-105">Úložiště objektů blob v Azure je služba, která ukládá nestrukturovaná data v cloudu jako objekty nebo objekty blob.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-105">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="2d4cf-106">Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="2d4cf-107">Blob storage se také nazývá úložiště objektů.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="2d4cf-108">Tento průvodce vám ukáže, jak provádět běžné scénáře s využitím služby Azure blob.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-108">This guide shows you how to perform common scenarios using the Azure blob service.</span></span> <span data-ttu-id="2d4cf-109">Ukázky jsou napsané v PHP a použití [Azure SDK pro jazyk PHP][download].</span><span class="sxs-lookup"><span data-stu-id="2d4cf-109">The samples are written in PHP and use the [Azure SDK for PHP][download].</span></span> <span data-ttu-id="2d4cf-110">Pokryté scénáře zahrnují **odesílání**, **výpis**, **stahování**, a **odstraňování** objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-110">The scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="2d4cf-111">Další informace o objekty BLOB, najdete v článku [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-111">For more information on blobs, see the [Next steps](#next-steps) section.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="2d4cf-112">Vytvoření aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="2d4cf-112">Create a PHP application</span></span>
<span data-ttu-id="2d4cf-113">Jediný požadavek pro vytvoření aplikace PHP, který přistupuje k službě Azure blob je odkazování třídy v sadě Azure SDK pro jazyk PHP z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-113">The only requirement for creating a PHP application that accesses the Azure blob service is the referencing of classes in the Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="2d4cf-114">Všechny nástroje pro vývoj slouží k vytvoření aplikace, včetně Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-114">You can use any development tools to create your application, including Notepad.</span></span>

<span data-ttu-id="2d4cf-115">V této příručce používat funkce služby, které lze volat v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-115">In this guide, you use service features, which can be called within a PHP application locally or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-the-azure-client-libraries"></a><span data-ttu-id="2d4cf-116">Získat knihoven klienta Azure</span><span class="sxs-lookup"><span data-stu-id="2d4cf-116">Get the Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a><span data-ttu-id="2d4cf-117">Konfigurace aplikace pro přístup ke službě blob</span><span class="sxs-lookup"><span data-stu-id="2d4cf-117">Configure your application to access the blob service</span></span>
<span data-ttu-id="2d4cf-118">Pomocí rozhraní API služby objektů blob v Azure, budete muset:</span><span class="sxs-lookup"><span data-stu-id="2d4cf-118">To use the Azure blob service APIs, you need to:</span></span>

1. <span data-ttu-id="2d4cf-119">Reference souboru pomocí automatického zavaděče [require_once] příkaz, a</span><span class="sxs-lookup"><span data-stu-id="2d4cf-119">Reference the autoloader file using the [require_once] statement, and</span></span>
2. <span data-ttu-id="2d4cf-120">Referenční všechny třídy, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-120">Reference any classes you might use.</span></span>

<span data-ttu-id="2d4cf-121">Následující příklad ukazuje, jak se zahrnuje automatického zavaděče souboru a odkaz **ServicesBuilder** třídy.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-121">The following example shows how to include the autoloader file and reference the **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="2d4cf-122">V příkladech v tomto článku předpokládá, že jste nainstalovali PHP klientské knihovny pro Azure prostřednictvím autora.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-122">The examples in this article assume you have installed the PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="2d4cf-123">Pokud jste nainstalovali v knihovnách ručně, je třeba odkazovat `WindowsAzure.php` automatického zavaděče souboru.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-123">If you installed the libraries manually, you need to reference the `WindowsAzure.php` autoloader file.</span></span>
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="2d4cf-124">V následujících příkladech `require_once` příkaz zobrazí vždy, ale jenom ty třídy potřebné pro tento příklad provést odkazují.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-124">In the examples below, the `require_once` statement will be shown always, but only the classes necessary for the example to execute are referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="2d4cf-125">Nastavit připojení k úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="2d4cf-125">Set up an Azure storage connection</span></span>
<span data-ttu-id="2d4cf-126">K vytvoření instance klienta služby Azure blob, Nejdřív musíte mít platný připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-126">To instantiate an Azure blob service client, you must first have a valid connection string.</span></span> <span data-ttu-id="2d4cf-127">Formát pro připojovací řetězec služby objektů blob je:</span><span class="sxs-lookup"><span data-stu-id="2d4cf-127">The format for the blob service connection string is:</span></span>

<span data-ttu-id="2d4cf-128">Pro přístup k službě za provozu:</span><span class="sxs-lookup"><span data-stu-id="2d4cf-128">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="2d4cf-129">Pro přístup k emulátoru úložiště:</span><span class="sxs-lookup"><span data-stu-id="2d4cf-129">For accessing the storage emulator:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="2d4cf-130">Pokud chcete vytvořit libovolného klienta služby Azure, budete muset použít **ServicesBuilder** třídy.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-130">To create any Azure service client, you need to use the **ServicesBuilder** class.</span></span> <span data-ttu-id="2d4cf-131">Můžete:</span><span class="sxs-lookup"><span data-stu-id="2d4cf-131">You can:</span></span>

* <span data-ttu-id="2d4cf-132">Připojovací řetězec přímo jí předat nebo</span><span class="sxs-lookup"><span data-stu-id="2d4cf-132">Pass the connection string directly to it or</span></span>
* <span data-ttu-id="2d4cf-133">Použití **CloudConfigurationManager (CCM)** zkontrolujte několik externích zdrojů pro připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="2d4cf-133">Use the **CloudConfigurationManager (CCM)** to check multiple external sources for the connection string:</span></span>
  * <span data-ttu-id="2d4cf-134">Ve výchozím nastavení je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-134">By default, it comes with support for one external source - environmental variables.</span></span>
  * <span data-ttu-id="2d4cf-135">Můžete přidat nové zdroje tím, že rozšíří **ConnectionStringSource** třídy.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-135">You can add new sources by extending the **ConnectionStringSource** class.</span></span>

<span data-ttu-id="2d4cf-136">Příklady podle zde uvedeného se předají připojovací řetězec, který přímo.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-136">For the examples outlined here, the connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);
```

## <a name="create-a-container"></a><span data-ttu-id="2d4cf-137">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="2d4cf-137">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="2d4cf-138">A **BlobRestProxy** objektu umožňuje vytvářet kontejner objektů blob s **createContainer** metoda.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-138">A **BlobRestProxy** object lets you create a blob container with the **createContainer** method.</span></span> <span data-ttu-id="2d4cf-139">Při vytváření kontejneru, můžete nastavit možnosti v kontejneru, ale to tak není povinný.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-139">When creating a container, you can set options on the container, but doing so is not required.</span></span> <span data-ttu-id="2d4cf-140">(Následující příklad ukazuje, jak nastavit kontejner seznam řízení přístupu (ACL) a metadata kontejneru.)</span><span class="sxs-lookup"><span data-stu-id="2d4cf-140">(The example below shows how to set the container access control list (ACL) and container metadata.)</span></span>

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
// proxys can enumerate blobs within the container via anonymous
// request, but cannot enumerate containers within the storage account.
//
// BLOBS_ONLY:
// Specifies public read access for blobs. Blob data within this
// container can be read via anonymous request, but container data is not
// available. proxys cannot enumerate blobs within the container via
// anonymous request.
// If this value is not specified in the request, container data is
// private to the account owner.
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

<span data-ttu-id="2d4cf-141">Volání metody **setPublicAccess (PublicAccessType::CONTAINER\_a\_objektů BLOB)** zpřístupní data kontejnerů a objektů blob prostřednictvím anonymních požadavků.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-141">Calling **setPublicAccess(PublicAccessType::CONTAINER\_AND\_BLOBS)** makes the container and blob data accessible via anonymous requests.</span></span> <span data-ttu-id="2d4cf-142">Volání metody **setPublicAccess(PublicAccessType::BLOBS_ONLY)** díky pouze blob dat, které jsou přístupné prostřednictvím anonymních požadavků.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-142">Calling **setPublicAccess(PublicAccessType::BLOBS_ONLY)** makes only blob data accessible via anonymous requests.</span></span> <span data-ttu-id="2d4cf-143">Další informace o kontejneru seznamy řízení přístupu najdete v tématu [sadu kontejneru seznamu řízení přístupu (REST API)][container-acl].</span><span class="sxs-lookup"><span data-stu-id="2d4cf-143">For more information about container ACLs, see [Set container ACL (REST API)][container-acl].</span></span>

<span data-ttu-id="2d4cf-144">Další informace o chybových kódech služby objektů Blob najdete v tématu [kódy chyb služby objektů Blob][error-codes].</span><span class="sxs-lookup"><span data-stu-id="2d4cf-144">For more information about Blob service error codes, see [Blob Service Error Codes][error-codes].</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="2d4cf-145">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="2d4cf-145">Upload a blob into a container</span></span>
<span data-ttu-id="2d4cf-146">Pokud chcete nahrát soubor jako objekt blob, použijte **BlobRestProxy -> createBlockBlob** metoda.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-146">To upload a file as a blob, use the **BlobRestProxy->createBlockBlob** method.</span></span> <span data-ttu-id="2d4cf-147">Tato operace vytvoří objekt blob, pokud neexistuje, nebo ho přepíše, pokud k tomu.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-147">This operation creates the blob if it doesn't exist, or overwrites it if it does.</span></span> <span data-ttu-id="2d4cf-148">Následující příklad kódu předpokládá, že kontejner byl vytvořen a používá [fopen –] [ fopen] k otevření souboru jako datový proud.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-148">The code example below assumes that the container has already been created and uses [fopen][fopen] to open the file as a stream.</span></span>

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

<span data-ttu-id="2d4cf-149">Všimněte si, že v předchozím příkladu odešle objekt blob jako datový proud.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-149">Note that the previous sample uploads a blob as a stream.</span></span> <span data-ttu-id="2d4cf-150">Ale objekt blob je možné také uložit jako řetězec, pomocí, například [soubor\_získat\_obsah] [ file_get_contents] funkce.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-150">However, a blob can also be uploaded as a string using, for example, the [file\_get\_contents][file_get_contents] function.</span></span> <span data-ttu-id="2d4cf-151">Chcete-li to provést, pomocí v předchozím příkladu, změňte `$content = fopen("c:\myfile.txt", "r");` k `$content = file_get_contents("c:\myfile.txt");`.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-151">To do this using the previous sample, change `$content = fopen("c:\myfile.txt", "r");` to `$content = file_get_contents("c:\myfile.txt");`.</span></span>

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="2d4cf-152">Zobrazí seznam objektů blob v kontejneru</span><span class="sxs-lookup"><span data-stu-id="2d4cf-152">List the blobs in a container</span></span>
<span data-ttu-id="2d4cf-153">K zobrazení seznamu objektů BLOB v kontejneru, použijte **BlobRestProxy -> listBlobs** metoda s **foreach** smyčky do cyklus prostřednictvím výsledek.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-153">To list the blobs in a container, use the **BlobRestProxy->listBlobs** method with a **foreach** loop to loop through the result.</span></span> <span data-ttu-id="2d4cf-154">Následující kód zobrazí název jednotlivých objektů blob jako výstup v kontejneru a jeho identifikátoru URI do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-154">The following code displays the name of each blob as output in a container and displays its URI to the browser.</span></span>

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

## <a name="download-a-blob"></a><span data-ttu-id="2d4cf-155">Stažení objektu blob</span><span class="sxs-lookup"><span data-stu-id="2d4cf-155">Download a blob</span></span>
<span data-ttu-id="2d4cf-156">Chcete-li stáhnout objekt blob, volejte **BlobRestProxy -> getblob –** metoda, pak volání **getContentStream** metodu výsledná **GetBlobResult** objektu.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-156">To download a blob, call the **BlobRestProxy->getBlob** method, then call the **getContentStream** method on the resulting **GetBlobResult** object.</span></span>

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

<span data-ttu-id="2d4cf-157">Všimněte si, že v předchozím příkladu získá objekt blob jako prostředek datového proudu (výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="2d4cf-157">Note that the example above gets a blob as a stream resource (the default behavior).</span></span> <span data-ttu-id="2d4cf-158">Můžete však použít [datového proudu\_získat\_obsah] [ stream-get-contents] funkce vráceném datovém proudu převést na řetězec.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-158">However, you can use the [stream\_get\_contents][stream-get-contents] function to convert the returned stream to a string.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="2d4cf-159">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="2d4cf-159">Delete a blob</span></span>
<span data-ttu-id="2d4cf-160">Chcete-li odstranit objekt blob, předejte název kontejneru a název objektu blob na **BlobRestProxy -> deleteBlob**.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-160">To delete a blob, pass the container name and blob name to **BlobRestProxy->deleteBlob**.</span></span>

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

## <a name="delete-a-blob-container"></a><span data-ttu-id="2d4cf-161">Odstranit kontejner objektů blob</span><span class="sxs-lookup"><span data-stu-id="2d4cf-161">Delete a blob container</span></span>
<span data-ttu-id="2d4cf-162">Nakonec pokud chcete odstranit kontejner objektů blob, předat název kontejneru, který má **BlobRestProxy -> deleteContainer**.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-162">Finally, to delete a blob container, pass the container name to **BlobRestProxy->deleteContainer**.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="2d4cf-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2d4cf-163">Next steps</span></span>
<span data-ttu-id="2d4cf-164">Teď, když jste se naučili základy služby Azure blob, postupujte podle následujících odkazech na další informace o složitějších úlohách úložiště.</span><span class="sxs-lookup"><span data-stu-id="2d4cf-164">Now that you've learned the basics of the Azure blob service, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="2d4cf-165">Přejděte [blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)</span><span class="sxs-lookup"><span data-stu-id="2d4cf-165">Visit the [Azure Storage team blog](http://blogs.msdn.com/b/windowsazurestorage/)</span></span>
* <span data-ttu-id="2d4cf-166">Najdete v článku [příklad objekt blob bloku PHP](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).</span><span class="sxs-lookup"><span data-stu-id="2d4cf-166">See the [PHP block blob example](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).</span></span>
* <span data-ttu-id="2d4cf-167">Najdete v článku [příklad objekt blob stránky PHP](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).</span><span class="sxs-lookup"><span data-stu-id="2d4cf-167">See the [PHP page blob example](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).</span></span>
* [<span data-ttu-id="2d4cf-168">Přenos dat pomocí nástroje příkazového řádku AzCopy</span><span class="sxs-lookup"><span data-stu-id="2d4cf-168">Transfer data with the AzCopy Command-Line Utility</span></span>](storage-use-azcopy.md)

<span data-ttu-id="2d4cf-169">Další informace naleznete také [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="2d4cf-169">For more information, see also the [PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
<span data-ttu-id="2d4cf-170">[require_once]: http://php.net/require_once</span><span class="sxs-lookup"><span data-stu-id="2d4cf-170">[require_once]: http://php.net/require_once</span></span>
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
