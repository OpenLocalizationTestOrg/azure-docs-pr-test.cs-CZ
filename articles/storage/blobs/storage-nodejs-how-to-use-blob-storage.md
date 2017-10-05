---
title: "Používání úložiště Blob z Node.js | Microsoft Docs"
description: "Ukládejte nestrukturovaná data v cloudu pomocí Azure Blob Storage (úložiště objektů)."
services: storage
documentationcenter: nodejs
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 8b0df222-1ca8-4967-8248-6d6d720947b8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: e83ad647f6b7c70f34ef0c69b5bf322da5b6d60d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-blob-storage-from-nodejs"></a><span data-ttu-id="764da-103">Používání úložiště Blob z Node.js</span><span class="sxs-lookup"><span data-stu-id="764da-103">How to use Blob storage from Node.js</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a><span data-ttu-id="764da-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="764da-104">Overview</span></span>
<span data-ttu-id="764da-105">Tento článek ukazuje, jak provádět běžné scénáře s využitím úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="764da-105">This article shows you how to perform common scenarios using Blob storage.</span></span> <span data-ttu-id="764da-106">Ukázky jsou zapsány pomocí rozhraní API Node.js.</span><span class="sxs-lookup"><span data-stu-id="764da-106">The samples are written via the Node.js API.</span></span> <span data-ttu-id="764da-107">Pokryté scénáře zahrnují nahrát, seznam, stažení a odstranění objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="764da-107">The scenarios covered include how to upload, list, download, and delete blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="764da-108">Vytvoření aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="764da-108">Create a Node.js application</span></span>
<span data-ttu-id="764da-109">Pokyny o tom, jak vytvořit aplikaci Node.js najdete v tématu [vytvořit webové aplikace Node.js ve službě Azure App Service], [sestavení a nasazení aplikace Node.js ve službě Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) – pomocí prostředí Windows PowerShell nebo [sestavení a nasazení webové aplikace Node.js do Azure pomocí Web Matrix](https://www.microsoft.com/web/webmatrix/).</span><span class="sxs-lookup"><span data-stu-id="764da-109">For instructions on how to create a Node.js application, see [Create a Node.js web app in Azure App Service], [Build and deploy a Node.js application to an Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) -- using Windows PowerShell, or [Build and deploy a Node.js web app to Azure using Web Matrix](https://www.microsoft.com/web/webmatrix/).</span></span>

## <a name="configure-your-application-to-access-storage"></a><span data-ttu-id="764da-110">Konfigurace aplikace pro přístup k úložišti</span><span class="sxs-lookup"><span data-stu-id="764da-110">Configure your application to access storage</span></span>
<span data-ttu-id="764da-111">Pokud chcete používat úložiště Azure, musíte sady SDK úložiště Azure pro platformu Node.js, která obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště.</span><span class="sxs-lookup"><span data-stu-id="764da-111">To use Azure storage, you need the Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with the storage REST services.</span></span>

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a><span data-ttu-id="764da-112">Získat balíček pomocí uzlu balíček správce (NPM)</span><span class="sxs-lookup"><span data-stu-id="764da-112">Use Node Package Manager (NPM) to obtain the package</span></span>
1. <span data-ttu-id="764da-113">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix), přejděte na složku, kde jste vytvořili ukázkové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="764da-113">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix), to navigate to the folder where you created your sample application.</span></span>
2. <span data-ttu-id="764da-114">Typ **npm nainstalujte azure-storage** v příkazovém okně.</span><span class="sxs-lookup"><span data-stu-id="764da-114">Type **npm install azure-storage** in the command window.</span></span> <span data-ttu-id="764da-115">Výstup z tohoto příkazu je podobná následující příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="764da-115">Output from the command is similar to the following code example.</span></span>

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
3. <span data-ttu-id="764da-116">Můžete ručně spustit **ls** příkazu ověřte, že **uzlu\_moduly** složka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="764da-116">You can manually run the **ls** command to verify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="764da-117">V této složce najít **azure-storage** balíček, který obsahuje knihovny, které potřebujete získat přístup k úložišti.</span><span class="sxs-lookup"><span data-stu-id="764da-117">Inside that folder, find the **azure-storage** package, which contains the libraries that you need to access storage.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="764da-118">Import balíčku</span><span class="sxs-lookup"><span data-stu-id="764da-118">Import the package</span></span>
<span data-ttu-id="764da-119">Pomocí programu Poznámkový blok nebo jiném textovém editoru, přidejte následující do horní části **server.js** souboru aplikace, které máte v úmyslu používat úložiště:</span><span class="sxs-lookup"><span data-stu-id="764da-119">Using Notepad or another text editor, add the following to the top of the **server.js** file of the application where you intend to use storage:</span></span>

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="764da-120">Nastavit připojení k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="764da-120">Set up an Azure Storage connection</span></span>
<span data-ttu-id="764da-121">Modul Azure, bude číst proměnné prostředí `AZURE_STORAGE_ACCOUNT` a `AZURE_STORAGE_ACCESS_KEY`, nebo `AZURE_STORAGE_CONNECTION_STRING`, informace požadované pro připojení k účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="764da-121">The Azure module will read the environment variables `AZURE_STORAGE_ACCOUNT` and `AZURE_STORAGE_ACCESS_KEY`, or `AZURE_STORAGE_CONNECTION_STRING`, for information required to connect to your Azure storage account.</span></span> <span data-ttu-id="764da-122">Pokud nejsou nastavené těchto proměnných prostředí, musíte zadat informace o účtu při volání metody **createBlobService**.</span><span class="sxs-lookup"><span data-stu-id="764da-122">If these environment variables are not set, you must specify the account information when calling **createBlobService**.</span></span>

<span data-ttu-id="764da-123">Příklad nastavení proměnných prostředí [portál Azure](https://portal.azure.com) webové aplikace Azure, najdete v části [webové aplikace Node.js pomocí služby Azure Table](../../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span><span class="sxs-lookup"><span data-stu-id="764da-123">For an example of setting the environment variables in the [Azure portal](https://portal.azure.com) for an Azure web app, see [Node.js web app using the Azure Table Service](../../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span></span>

## <a name="create-a-container"></a><span data-ttu-id="764da-124">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="764da-124">Create a container</span></span>
<span data-ttu-id="764da-125">**BlobService** objekt vám umožňuje spolupracovat s kontejnery a objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="764da-125">The **BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="764da-126">Následující kód vytvoří **BlobService** objektu.</span><span class="sxs-lookup"><span data-stu-id="764da-126">The following code creates a **BlobService** object.</span></span> <span data-ttu-id="764da-127">Přidejte následující v horní části **server.js**:</span><span class="sxs-lookup"><span data-stu-id="764da-127">Add the following near the top of **server.js**:</span></span>

```nodejs
var blobSvc = azure.createBlobService();
```

> [!NOTE]
> <span data-ttu-id="764da-128">Objekt blob může anonymně přístup pomocí **createBlobServiceAnonymous** a poskytnutím adresa hostitele.</span><span class="sxs-lookup"><span data-stu-id="764da-128">You can access a blob anonymously by using **createBlobServiceAnonymous** and providing the host address.</span></span> <span data-ttu-id="764da-129">Například použít `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.</span><span class="sxs-lookup"><span data-stu-id="764da-129">For example, use `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.</span></span>
>
>

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="764da-130">Chcete-li vytvořit nový kontejner, použijte **createContainerIfNotExists**.</span><span class="sxs-lookup"><span data-stu-id="764da-130">To create a new container, use **createContainerIfNotExists**.</span></span> <span data-ttu-id="764da-131">Následující příklad kódu vytvoří nový kontejner s názvem 'můj_kontejner':</span><span class="sxs-lookup"><span data-stu-id="764da-131">The following code example creates a new container named 'mycontainer':</span></span>

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
    if(!error){
      // Container exists and is private
    }
});
```

<span data-ttu-id="764da-132">Pokud je nově vytvořený kontejner, `result.created` hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="764da-132">If the container is newly created, `result.created` is true.</span></span> <span data-ttu-id="764da-133">Pokud již existuje kontejner, `result.created` je false.</span><span class="sxs-lookup"><span data-stu-id="764da-133">If the container already exists, `result.created` is false.</span></span> <span data-ttu-id="764da-134">`response`obsahuje informace o operaci, včetně informací o ETag pro příslušný kontejner.</span><span class="sxs-lookup"><span data-stu-id="764da-134">`response` contains information about the operation, including the ETag information for the container.</span></span>

### <a name="container-security"></a><span data-ttu-id="764da-135">Kontejner zabezpečení</span><span class="sxs-lookup"><span data-stu-id="764da-135">Container security</span></span>
<span data-ttu-id="764da-136">Ve výchozím nastavení nové kontejnery jsou soukromá a nemohou být získat anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="764da-136">By default, new containers are private and cannot be accessed anonymously.</span></span> <span data-ttu-id="764da-137">Chcete-li zveřejnit kontejner, aby ho může anonymně přístup, můžete nastavit kontejneru přístup na úrovni **blob** nebo **kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="764da-137">To make the container public so that you can access it anonymously, you can set the container's access level to **blob** or **container**.</span></span>

* <span data-ttu-id="764da-138">**objekt BLOB** -umožňuje anonymní přístup pro čtení obsahu objektu blob a metadat v tomto kontejneru, ale není metadata kontejneru například výpis všech objektů BLOB do kontejneru</span><span class="sxs-lookup"><span data-stu-id="764da-138">**blob** - allows anonymous read access to blob content and metadata within this container, but not to container metadata such as listing all blobs within a container</span></span>
* <span data-ttu-id="764da-139">**kontejner** -umožňuje anonymní přístup pro čtení k obsah objektu blob a metadat, jakož i metadata kontejneru</span><span class="sxs-lookup"><span data-stu-id="764da-139">**container** - allows anonymous read access to blob content and metadata as well as container metadata</span></span>

<span data-ttu-id="764da-140">Následující příklad kódu ukazuje nastavení přístupu na úrovni **blob**:</span><span class="sxs-lookup"><span data-stu-id="764da-140">The following code example demonstrates setting the access level to **blob**:</span></span>

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
    if(!error){
      // Container exists and allows
      // anonymous read access to blob
      // content and metadata within this container
    }
});
```

<span data-ttu-id="764da-141">Alternativně můžete upravit úroveň přístupu tohoto kontejneru pomocí **setContainerAcl** k určení úrovně přístupu.</span><span class="sxs-lookup"><span data-stu-id="764da-141">Alternatively, you can modify the access level of a container by using **setContainerAcl** to specify the access level.</span></span> <span data-ttu-id="764da-142">Následující příklad kódu změní úroveň přístupu do kontejneru:</span><span class="sxs-lookup"><span data-stu-id="764da-142">The following code example changes the access level to container:</span></span>

```nodejs
blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
  if(!error){
    // Container access level set to 'container'
  }
});
```

<span data-ttu-id="764da-143">Výsledek obsahuje informace o operaci, včetně aktuální **značka ETag** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="764da-143">The result contains information about the operation, including the current **ETag** for the container.</span></span>

### <a name="filters"></a><span data-ttu-id="764da-144">Filtry</span><span class="sxs-lookup"><span data-stu-id="764da-144">Filters</span></span>
<span data-ttu-id="764da-145">Můžete provést volitelný filtrování operací u operace provedené pomocí **BlobService**.</span><span class="sxs-lookup"><span data-stu-id="764da-145">You can apply optional filtering operations to operations performed using **BlobService**.</span></span> <span data-ttu-id="764da-146">Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu s podpisem:</span><span class="sxs-lookup"><span data-stu-id="764da-146">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with the signature:</span></span>

```nodejs
function handle (requestOptions, next)
```

<span data-ttu-id="764da-147">Až to předzpracování na žádost o možnostech, metoda potřebuje volat tlačítko Další"předání zpětné volání podpisem následující:</span><span class="sxs-lookup"><span data-stu-id="764da-147">After doing its preprocessing on the request options, the method needs to call "next", passing a callback with the following signature:</span></span>

```nodejs
function (returnObject, finalCallback, next)
```

<span data-ttu-id="764da-148">V této zpětného volání a po zpracování returnObject (odpovědi požadavek na server) zpětné volání je potřeba buď vyvolat další, pokud existuje pokračovat ve zpracovávání ostatní filtry nebo jednoduše vyvolat finalCallback na konec volání služby.</span><span class="sxs-lookup"><span data-stu-id="764da-148">In this callback, and after processing the returnObject (the response from the request to the server), the callback needs to either invoke next if it exists to continue processing other filters or simply invoke finalCallback to end the service invocation.</span></span>

<span data-ttu-id="764da-149">Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí sady Azure SDK pro Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="764da-149">Two filters that implement retry logic are included with the Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="764da-150">Vytvoří následující **BlobService** objekt, který používá **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="764da-150">The following creates a **BlobService** object that uses the **ExponentialRetryPolicyFilter**:</span></span>

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var blobSvc = azure.createBlobService().withFilter(retryOperations);
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="764da-151">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="764da-151">Upload a blob into a container</span></span>
<span data-ttu-id="764da-152">Existují tři typy objektů blob: objekty BLOB bloků, objekty BLOB stránky a doplňovacích objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="764da-152">There are three types of blobs: block blobs, page blobs and append blobs.</span></span> <span data-ttu-id="764da-153">Objekty BLOB bloku povolit, že vám umožní nahrát efektivně velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="764da-153">Block blobs allow you to more efficiently upload large data.</span></span> <span data-ttu-id="764da-154">Append – objekty BLOB jsou optimalizované pro doplňovací operace.</span><span class="sxs-lookup"><span data-stu-id="764da-154">Append blobs are optimized for append operations.</span></span> <span data-ttu-id="764da-155">Objekty BLOB stránky jsou optimalizované pro operace čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="764da-155">Page blobs are optimized for read/write operations.</span></span> <span data-ttu-id="764da-156">Další informace najdete v tématu [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="764da-156">For more information, see [Understanding Block Blobs, Append Blobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

### <a name="block-blobs"></a><span data-ttu-id="764da-157">Objekty blob bloku</span><span class="sxs-lookup"><span data-stu-id="764da-157">Block blobs</span></span>
<span data-ttu-id="764da-158">Odeslání dat do objekt blob bloku, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="764da-158">To upload data to a block blob, use the following:</span></span>

* <span data-ttu-id="764da-159">**createBlockBlobFromLocalFile** – vytvoří nový objekt blob bloku a odesílá obsah souboru</span><span class="sxs-lookup"><span data-stu-id="764da-159">**createBlockBlobFromLocalFile** - creates a new block blob and uploads the contents of a file</span></span>
* <span data-ttu-id="764da-160">**createBlockBlobFromStream** – vytvoří nový objekt blob bloku a odesílá obsah datového proudu</span><span class="sxs-lookup"><span data-stu-id="764da-160">**createBlockBlobFromStream** - creates a new block blob and uploads the contents of a stream</span></span>
* <span data-ttu-id="764da-161">**createBlockBlobFromText** – vytvoří nový objekt blob bloku a odesílá obsah řetězce</span><span class="sxs-lookup"><span data-stu-id="764da-161">**createBlockBlobFromText** - creates a new block blob and uploads the contents of a string</span></span>
* <span data-ttu-id="764da-162">**createWriteStreamToBlockBlob** -poskytuje datový proud zápisu do objektu blob bloku</span><span class="sxs-lookup"><span data-stu-id="764da-162">**createWriteStreamToBlockBlob** - provides a write stream to a block blob</span></span>

<span data-ttu-id="764da-163">Následující příklad kódu odešle obsah **test.txt** soubor do **můj_objekt_blob**.</span><span class="sxs-lookup"><span data-stu-id="764da-163">The following code example uploads the contents of the **test.txt** file into **myblob**.</span></span>

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="764da-164">`result` Vrácený tyto metody obsahuje informace o operaci, jako jsou například **značka ETag** objektu blob.</span><span class="sxs-lookup"><span data-stu-id="764da-164">The `result` returned by these methods contains information on the operation, such as the **ETag** of the blob.</span></span>

### <a name="append-blobs"></a><span data-ttu-id="764da-165">Doplňovací objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="764da-165">Append blobs</span></span>
<span data-ttu-id="764da-166">Odeslání dat do nový doplňovací objekt blob, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="764da-166">To upload data to a new append blob, use the following:</span></span>

* <span data-ttu-id="764da-167">**createAppendBlobFromLocalFile** – vytvoří nový doplňovací objekt blob a odesílá obsah souboru</span><span class="sxs-lookup"><span data-stu-id="764da-167">**createAppendBlobFromLocalFile** - creates a new append blob and uploads the contents of a file</span></span>
* <span data-ttu-id="764da-168">**createAppendBlobFromStream** – vytvoří nový doplňovací objekt blob a odesílá obsah datového proudu</span><span class="sxs-lookup"><span data-stu-id="764da-168">**createAppendBlobFromStream** - creates a new append blob and uploads the contents of a stream</span></span>
* <span data-ttu-id="764da-169">**createAppendBlobFromText** – vytvoří nový doplňovací objekt blob a odesílá obsah řetězce</span><span class="sxs-lookup"><span data-stu-id="764da-169">**createAppendBlobFromText** - creates a new append blob and uploads the contents of a string</span></span>
* <span data-ttu-id="764da-170">**createWriteStreamToNewAppendBlob** – vytvoří nový doplňovací objekt blob a pak poskytuje datového proudu pro zápis do</span><span class="sxs-lookup"><span data-stu-id="764da-170">**createWriteStreamToNewAppendBlob** - creates a new append blob and then provides a stream to write to it</span></span>

<span data-ttu-id="764da-171">Následující příklad kódu odešle obsah **test.txt** soubor do **myappendblob**.</span><span class="sxs-lookup"><span data-stu-id="764da-171">The following code example uploads the contents of the **test.txt** file into **myappendblob**.</span></span>

```nodejs
blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="764da-172">Připojit k existující doplňovací objekt blob bloku, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="764da-172">To append a block to an existing append blob, use the following:</span></span>

* <span data-ttu-id="764da-173">**appendFromLocalFile** -ke stávající doplňovací objekt blob připojit obsah souboru</span><span class="sxs-lookup"><span data-stu-id="764da-173">**appendFromLocalFile** - append the contents of a file to an existing append blob</span></span>
* <span data-ttu-id="764da-174">**appendFromStream** -ke stávající doplňovací objekt blob připojit obsah datového proudu</span><span class="sxs-lookup"><span data-stu-id="764da-174">**appendFromStream** - append the contents of a stream to an existing append blob</span></span>
* <span data-ttu-id="764da-175">**appendFromText** -ke stávající doplňovací objekt blob připojit obsah řetězce</span><span class="sxs-lookup"><span data-stu-id="764da-175">**appendFromText** - append the contents of a string to an existing append blob</span></span>
* <span data-ttu-id="764da-176">**appendBlockFromStream** -ke stávající doplňovací objekt blob připojit obsah datového proudu</span><span class="sxs-lookup"><span data-stu-id="764da-176">**appendBlockFromStream** - append the contents of a stream to an existing append blob</span></span>
* <span data-ttu-id="764da-177">**appendBlockFromText** -ke stávající doplňovací objekt blob připojit obsah řetězce</span><span class="sxs-lookup"><span data-stu-id="764da-177">**appendBlockFromText** - append the contents of a string to an existing append blob</span></span>

> [!NOTE]
> <span data-ttu-id="764da-178">appendFromXXX rozhraní API budou dělat některé selhání rychle, aby se zabránilo zbytečným serveru volání ověření klienta.</span><span class="sxs-lookup"><span data-stu-id="764da-178">appendFromXXX APIs will do some client-side validation to fail fast to avoid unnecessary server calls.</span></span> <span data-ttu-id="764da-179">appendBlockFromXXX nebude.</span><span class="sxs-lookup"><span data-stu-id="764da-179">appendBlockFromXXX won't.</span></span>
>
>

<span data-ttu-id="764da-180">Následující příklad kódu odešle obsah **test.txt** soubor do **myappendblob**.</span><span class="sxs-lookup"><span data-stu-id="764da-180">The following code example uploads the contents of the **test.txt** file into **myappendblob**.</span></span>

```nodejs
blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
  if(!error){
    // text appended
  }
});
```

### <a name="page-blobs"></a><span data-ttu-id="764da-181">Objekty blob stránky</span><span class="sxs-lookup"><span data-stu-id="764da-181">Page blobs</span></span>
<span data-ttu-id="764da-182">Nahrát data do objektů blob stránky, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="764da-182">To upload data to a page blob, use the following:</span></span>

* <span data-ttu-id="764da-183">**createPageBlob** -vytvoří nový objekt blob stránky konkrétní délky</span><span class="sxs-lookup"><span data-stu-id="764da-183">**createPageBlob** - creates a new page blob of a specific length</span></span>
* <span data-ttu-id="764da-184">**createPageBlobFromLocalFile** – vytvoří nový objekt blob stránky a odesílá obsah souboru</span><span class="sxs-lookup"><span data-stu-id="764da-184">**createPageBlobFromLocalFile** - creates a new page blob and uploads the contents of a file</span></span>
* <span data-ttu-id="764da-185">**createPageBlobFromStream** – vytvoří nový objekt blob stránky a odesílá obsah datového proudu</span><span class="sxs-lookup"><span data-stu-id="764da-185">**createPageBlobFromStream** - creates a new page blob and uploads the contents of a stream</span></span>
* <span data-ttu-id="764da-186">**createWriteStreamToExistingPageBlob** -poskytuje zápisu datového proudu pro existující objekt blob stránky</span><span class="sxs-lookup"><span data-stu-id="764da-186">**createWriteStreamToExistingPageBlob** - provides a write stream to an existing page blob</span></span>
* <span data-ttu-id="764da-187">**createWriteStreamToNewPageBlob** – vytvoří nový objekt blob stránky a pak poskytuje datového proudu pro zápis do</span><span class="sxs-lookup"><span data-stu-id="764da-187">**createWriteStreamToNewPageBlob** - creates a new page blob and then provides a stream to write to it</span></span>

<span data-ttu-id="764da-188">Následující příklad kódu odešle obsah **test.txt** soubor do **mypageblob**.</span><span class="sxs-lookup"><span data-stu-id="764da-188">The following code example uploads the contents of the **test.txt** file into **mypageblob**.</span></span>

```nodejs
blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

> [!NOTE]
> <span data-ttu-id="764da-189">Objekty BLOB stránky obsahovat 512 bajtů stránky.</span><span class="sxs-lookup"><span data-stu-id="764da-189">Page blobs consist of 512-byte 'pages'.</span></span> <span data-ttu-id="764da-190">Obdržíte chybu při odesílání dat s velikostí, která není násobkem 512.</span><span class="sxs-lookup"><span data-stu-id="764da-190">You will receive an error when uploading data with a size that is not a multiple of 512.</span></span>
>
>

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="764da-191">Zobrazí seznam objektů blob v kontejneru</span><span class="sxs-lookup"><span data-stu-id="764da-191">List the blobs in a container</span></span>
<span data-ttu-id="764da-192">K zobrazení seznamu objektů BLOB v kontejneru, použijte **listBlobsSegmented** metoda.</span><span class="sxs-lookup"><span data-stu-id="764da-192">To list the blobs in a container, use the **listBlobsSegmented** method.</span></span> <span data-ttu-id="764da-193">Pokud chcete vrátit objektů BLOB s konkrétní předponou, použijte **listBlobsSegmentedWithPrefix**.</span><span class="sxs-lookup"><span data-stu-id="764da-193">If you'd like to return blobs with a specific prefix, use **listBlobsSegmentedWithPrefix**.</span></span>

```nodejs
blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
  if(!error){
      // result.entries contains the entries
      // If not all blobs were returned, result.continuationToken has the continuation token.
  }
});
```

<span data-ttu-id="764da-194">`result` Obsahuje `entries` kolekce, která je pole objektů, které popisují jednotlivých objektů blob.</span><span class="sxs-lookup"><span data-stu-id="764da-194">The `result` contains an `entries` collection, which is an array of objects that describe each blob.</span></span> <span data-ttu-id="764da-195">Pokud nemůže být vrácen všech objektů BLOB, `result` také poskytuje `continuationToken`, které můžete použít jako druhý parametr pro načtení další záznamy.</span><span class="sxs-lookup"><span data-stu-id="764da-195">If all blobs cannot be returned, the `result` also provides a `continuationToken`, which you may use as the second parameter to retrieve additional entries.</span></span>

## <a name="download-blobs"></a><span data-ttu-id="764da-196">Stáhnout objekty blob</span><span class="sxs-lookup"><span data-stu-id="764da-196">Download blobs</span></span>
<span data-ttu-id="764da-197">Chcete-li data z objektu blob, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="764da-197">To download data from a blob, use the following:</span></span>

* <span data-ttu-id="764da-198">**getBlobToLocalFile** -zapíše obsahu objektu blob do souboru</span><span class="sxs-lookup"><span data-stu-id="764da-198">**getBlobToLocalFile** - writes the blob contents to file</span></span>
* <span data-ttu-id="764da-199">**getBlobToStream** -zapíše do datového proudu obsahu objektu blob</span><span class="sxs-lookup"><span data-stu-id="764da-199">**getBlobToStream** - writes the blob contents to a stream</span></span>
* <span data-ttu-id="764da-200">**getBlobToText** -zapíše obsahu objektu blob na řetězec</span><span class="sxs-lookup"><span data-stu-id="764da-200">**getBlobToText** - writes the blob contents to a string</span></span>
* <span data-ttu-id="764da-201">**createReadStream** -poskytuje datový proud čtení z objektu blob</span><span class="sxs-lookup"><span data-stu-id="764da-201">**createReadStream** - provides a stream to read from the blob</span></span>

<span data-ttu-id="764da-202">Následující příklad kódu ukazuje, jak pomocí **getBlobToStream** stáhnout obsah **můj_objekt_blob** objektů blob a uložte ho do **výstup.txt** souboru pomocí datového proudu:</span><span class="sxs-lookup"><span data-stu-id="764da-202">The following code example demonstrates using **getBlobToStream** to download the contents of the **myblob** blob and store it to the **output.txt** file by using a stream:</span></span>

```nodejs
var fs = require('fs');
blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
  if(!error){
    // blob retrieved
  }
});
```

<span data-ttu-id="764da-203">`result` Obsahuje informace o objektu blob, včetně **značka ETag** informace.</span><span class="sxs-lookup"><span data-stu-id="764da-203">The `result` contains information about the blob, including **ETag** information.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="764da-204">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="764da-204">Delete a blob</span></span>
<span data-ttu-id="764da-205">Nakonec se odstranit objekt blob, zavolejte **deleteBlob**.</span><span class="sxs-lookup"><span data-stu-id="764da-205">Finally, to delete a blob, call **deleteBlob**.</span></span> <span data-ttu-id="764da-206">Následující příklad kódu odstraní objektů blob s názvem **můj_objekt_blob**.</span><span class="sxs-lookup"><span data-stu-id="764da-206">The following code example deletes the blob named **myblob**.</span></span>

```nodejs
blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
  if(!error){
    // Blob has been deleted
  }
});
```

## <a name="concurrent-access"></a><span data-ttu-id="764da-207">Souběžný přístup</span><span class="sxs-lookup"><span data-stu-id="764da-207">Concurrent access</span></span>
<span data-ttu-id="764da-208">Chcete-li podporovat souběžný přístup do objektu blob z více klientů nebo více instancí procesu, můžete použít **značky etag binárním rozsáhlým** nebo **zapůjčení**.</span><span class="sxs-lookup"><span data-stu-id="764da-208">To support concurrent access to a blob from multiple clients or multiple process instances, you can use **ETags** or **leases**.</span></span>

* <span data-ttu-id="764da-209">**Značka Etag** -poskytuje způsob, jak zjistit, že objektu blob nebo kontejneru byl změněn jiným procesem</span><span class="sxs-lookup"><span data-stu-id="764da-209">**Etag** - provides a way to detect that the blob or container has been modified by another process</span></span>
* <span data-ttu-id="764da-210">**Zapůjčení** -poskytuje způsob, jak získat exkluzivní, obnovitelné, zápisu nebo odstranit přístupu do objektu blob pro v časovém intervalu</span><span class="sxs-lookup"><span data-stu-id="764da-210">**Lease** - provides a way to obtain exclusive, renewable, write or delete access to a blob for a period of time</span></span>

### <a name="etag"></a><span data-ttu-id="764da-211">Značka ETag</span><span class="sxs-lookup"><span data-stu-id="764da-211">ETag</span></span>
<span data-ttu-id="764da-212">Značky etag použití binárním rozsáhlým Pokud je potřeba povolit více klientů nebo instance k zápisu do bloku, objektů Blob nebo stránky Blob současně.</span><span class="sxs-lookup"><span data-stu-id="764da-212">Use ETags if you need to allow multiple clients or instances to write to the block Blob or page Blob simultaneously.</span></span> <span data-ttu-id="764da-213">Značky ETag umožňuje určit, pokud kontejner nebo objekt blob byla změněna od začátku čtení nebo vytvořili, která umožňuje zabránit přepsání změny potvrzeny jiný klienta nebo proces.</span><span class="sxs-lookup"><span data-stu-id="764da-213">The ETag allows you to determine if the container or blob was modified since you initially read or created it, which allows you to avoid overwriting changes committed by another client or process.</span></span>

<span data-ttu-id="764da-214">Můžete nastavit podmínky, značka ETag pomocí volitelného `options.accessConditions` parametr.</span><span class="sxs-lookup"><span data-stu-id="764da-214">You can set ETag conditions by using the optional `options.accessConditions` parameter.</span></span> <span data-ttu-id="764da-215">Následující kód například pouze nahrávání **test.txt** soubor, pokud objekt blob již existuje a má hodnotu ETag obsahoval podle `etagToMatch`.</span><span class="sxs-lookup"><span data-stu-id="764da-215">The following code example only uploads the **test.txt** file if the blob already exists and has the ETag value contained by `etagToMatch`.</span></span>

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
    if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="764da-216">Při použití značky etag binárním rozsáhlým, je obecný vzor:</span><span class="sxs-lookup"><span data-stu-id="764da-216">When you're using ETags, the general pattern is:</span></span>

1. <span data-ttu-id="764da-217">Značky ETag získáte v důsledku create, seznamu nebo operaci get.</span><span class="sxs-lookup"><span data-stu-id="764da-217">Obtain the ETag as the result of a create, list, or get operation.</span></span>
2. <span data-ttu-id="764da-218">Provedení akce, kontrola, hodnota ETag nebyl změněn.</span><span class="sxs-lookup"><span data-stu-id="764da-218">Perform an action, checking that the ETag value has not been modified.</span></span>

<span data-ttu-id="764da-219">Pokud byla hodnota změněna, znamená to, že jiný klienta nebo instance upraveny objektů blob nebo kontejneru vzhledem k tomu, že jste získali hodnota značky ETag.</span><span class="sxs-lookup"><span data-stu-id="764da-219">If the value was modified, this indicates that another client or instance modified the blob or container since you obtained the ETag value.</span></span>

### <a name="lease"></a><span data-ttu-id="764da-220">Zapůjčení</span><span class="sxs-lookup"><span data-stu-id="764da-220">Lease</span></span>
<span data-ttu-id="764da-221">Nové zapůjčení můžete získat pomocí **acquireLease** metodu s uvedením objektů blob nebo kontejner, kterou chcete získat zapůjčení na.</span><span class="sxs-lookup"><span data-stu-id="764da-221">You can acquire a new lease by using the **acquireLease** method, specifying the blob or container that you wish to obtain a lease on.</span></span> <span data-ttu-id="764da-222">Například následující kód získá zapůjčení na **můj_objekt_blob**.</span><span class="sxs-lookup"><span data-stu-id="764da-222">For example, the following code acquires a lease on **myblob**.</span></span>

```nodejs
blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
  if(!error) {
    console.log('leaseId: ' + result.id);
  }
});
```

<span data-ttu-id="764da-223">Dalších operacích na **můj_objekt_blob** musíte zadat `options.leaseId` parametr.</span><span class="sxs-lookup"><span data-stu-id="764da-223">Subsequent operations on **myblob** must provide the `options.leaseId` parameter.</span></span> <span data-ttu-id="764da-224">Zapůjčení ID se vrátí jako `result.id` z **acquireLease**.</span><span class="sxs-lookup"><span data-stu-id="764da-224">The lease ID is returned as `result.id` from **acquireLease**.</span></span>

> [!NOTE]
> <span data-ttu-id="764da-225">Ve výchozím nastavení je neomezenou dobu trvání zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="764da-225">By default, the lease duration is infinite.</span></span> <span data-ttu-id="764da-226">Můžete určit jiný nekonečné dobu trvání (mezi 15 a 60 sekund) pomocí `options.leaseDuration` parametr.</span><span class="sxs-lookup"><span data-stu-id="764da-226">You can specify a non-infinite duration (between 15 and 60 seconds) by providing the `options.leaseDuration` parameter.</span></span>
>
>

<span data-ttu-id="764da-227">Chcete-li odebrat zapůjčení, použijte **releaseLease**.</span><span class="sxs-lookup"><span data-stu-id="764da-227">To remove a lease, use **releaseLease**.</span></span> <span data-ttu-id="764da-228">Chcete-li rozdělit zapůjčení, ale jiným uživatelům zabránit v získání nového zapůjčení, dokud nevyprší platnost původního, použijte **breakLease**.</span><span class="sxs-lookup"><span data-stu-id="764da-228">To break a lease, but prevent others from obtaining a new lease until the original duration has expired, use **breakLease**.</span></span>

## <a name="work-with-shared-access-signatures"></a><span data-ttu-id="764da-229">Práce s podpisy sdíleného přístupu</span><span class="sxs-lookup"><span data-stu-id="764da-229">Work with shared access signatures</span></span>
<span data-ttu-id="764da-230">Sdílené přístupové podpisy (SAS) jsou zabezpečené způsob, jak poskytnout podrobné přístup k objektům BLOB a kontejnerů, bez zadání názvu účtu úložiště nebo klíče.</span><span class="sxs-lookup"><span data-stu-id="764da-230">Shared access signatures (SAS) are a secure way to provide granular access to blobs and containers without providing your storage account name or keys.</span></span> <span data-ttu-id="764da-231">Sdílené přístupové podpisy jsou často používá k poskytování omezený přístup k datům, například povolení mobilních aplikací pro přístup k objektům BLOB.</span><span class="sxs-lookup"><span data-stu-id="764da-231">Shared access signatures are often used to provide limited access to your data, such as allowing a mobile app to access blobs.</span></span>

> [!NOTE]
> <span data-ttu-id="764da-232">Zatímco můžete také povolit anonymní přístup k objektům BLOB, sdílené přístupové podpisy umožňují zadejte více řízený přístup, protože musíte vygenerovat SAS.</span><span class="sxs-lookup"><span data-stu-id="764da-232">While you can also allow anonymous access to blobs, shared access signatures allow you to provide more controlled access, as you must generate the SAS.</span></span>
>
>

<span data-ttu-id="764da-233">Sdílené přístupové podpisy pomocí generuje důvěryhodné aplikace, například cloudové služby **generateSharedAccessSignature** z **BlobService**a poskytuje ji nedůvěryhodné nebo částečně důvěryhodné aplikaci, například mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="764da-233">A trusted application such as a cloud-based service generates shared access signatures using the **generateSharedAccessSignature** of the **BlobService**, and provides it to an untrusted or semi-trusted application such as a mobile app.</span></span> <span data-ttu-id="764da-234">Sdílené přístupové podpisy se generují pomocí zásad, která popisuje spuštění a koncovým datem, během které sdílené přístupové podpisy jsou platné, a také udělená úroveň přístupu držitele podpisy sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="764da-234">Shared access signatures are generated using a policy, which describes the start and end dates during which the shared access signatures are valid, as well as the access level granted to the shared access signatures holder.</span></span>

<span data-ttu-id="764da-235">Následující příklad kódu vytvoří nová zásada sdíleného přístupu, která umožňuje držitele podpisy sdíleného přístupu k provedení operací čtení na **můj_objekt_blob** objektů blob a jeho platnost vyprší 100 minut po čase jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="764da-235">The following code example generates a new shared access policy that allows the shared access signatures holder to perform read operations on the **myblob** blob, and expires 100 minutes after the time it is created.</span></span>

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
};

var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
var host = blobSvc.host;
```

<span data-ttu-id="764da-236">Všimněte si, že informace o hostiteli je třeba zadat také, jako je povinný, pokud držitele podpisy sdíleného přístupu pokusí o přístup k kontejneru.</span><span class="sxs-lookup"><span data-stu-id="764da-236">Note that the host information must be provided also, as it is required when the shared access signatures holder attempts to access the container.</span></span>

<span data-ttu-id="764da-237">Klientská aplikace pak používá sdílené přístupové podpisy s **BlobServiceWithSAS** k provedení operace u objektu blob.</span><span class="sxs-lookup"><span data-stu-id="764da-237">The client application then uses shared access signatures with **BlobServiceWithSAS** to perform operations against the blob.</span></span> <span data-ttu-id="764da-238">Následující získá informace **můj_objekt_blob**.</span><span class="sxs-lookup"><span data-stu-id="764da-238">The following gets information about **myblob**.</span></span>

```nodejs
var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
  if(!error) {
    // retrieved info
  }
});
```

<span data-ttu-id="764da-239">Vzhledem k tomu, že sdílené přístupové podpisy byly vygenerovány s přístupem jen pro čtení, pokud je k pokusu změnit objekt blob, bude vrácena chyba.</span><span class="sxs-lookup"><span data-stu-id="764da-239">Since the shared access signatures were generated with read-only access, if an attempt is made to modify the blob, an error will be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="764da-240">Seznamy řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="764da-240">Access control lists</span></span>
<span data-ttu-id="764da-241">Seznam řízení přístupu (ACL) můžete také nastavit zásady přístupu pro SAS.</span><span class="sxs-lookup"><span data-stu-id="764da-241">You can also use an access control list (ACL) to set the access policy for SAS.</span></span> <span data-ttu-id="764da-242">To je užitečné, pokud chcete povolit více klientům přístup k kontejner, ale poskytnutí zásad, jiný přístup pro každého klienta.</span><span class="sxs-lookup"><span data-stu-id="764da-242">This is useful if you wish to allow multiple clients to access a container but provide different access policies for each client.</span></span>

<span data-ttu-id="764da-243">Seznam ACL je implementovaná pomocí pole zásady přístupu s ID spojené s každou zásadu.</span><span class="sxs-lookup"><span data-stu-id="764da-243">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="764da-244">Následující příklad kódu definuje dvě zásady, jeden pro "uživatel1" a jeden pro, uživatel2":</span><span class="sxs-lookup"><span data-stu-id="764da-244">The following code example defines two policies, one for 'user1' and one for 'user2':</span></span>

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

<span data-ttu-id="764da-245">Následující příklad kódu získá aktuální seznam ACL pro **můj_kontejner**a potom se přidají nové zásady pomocí **setBlobAcl**.</span><span class="sxs-lookup"><span data-stu-id="764da-245">The following code example gets the current ACL for **mycontainer**, and then adds the new policies using **setBlobAcl**.</span></span> <span data-ttu-id="764da-246">Tento přístup umožňuje:</span><span class="sxs-lookup"><span data-stu-id="764da-246">This approach allows:</span></span>

```nodejs
var extend = require('extend');
blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

<span data-ttu-id="764da-247">Jakmile je nastavená seznamu ACL, potom můžete vytvořit sdílené přístupové podpisy, na základě ID pro zásadu.</span><span class="sxs-lookup"><span data-stu-id="764da-247">Once the ACL is set, you can then create shared access signatures based on the ID for a policy.</span></span> <span data-ttu-id="764da-248">Následující příklad kódu vytvoří nové sdílené přístupové podpisy pro, uživatel2":</span><span class="sxs-lookup"><span data-stu-id="764da-248">The following code example creates new shared access signatures for 'user2':</span></span>

```nodejs
blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="764da-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="764da-249">Next steps</span></span>
<span data-ttu-id="764da-250">Další informace najdete v následujících zdrojích informací.</span><span class="sxs-lookup"><span data-stu-id="764da-250">For more information, see the following resources.</span></span>

* <span data-ttu-id="764da-251">[Úložiště azure SDK pro uzel referenční dokumentace rozhraní API] [Úložiště azure SDK pro uzel referenční dokumentace rozhraní API]</span><span class="sxs-lookup"><span data-stu-id="764da-251">[Azure Storage SDK for Node API Reference][Azure Storage SDK for Node API Reference]</span></span>
* <span data-ttu-id="764da-252">[Blog týmu azure Storage] [Blog týmu azure Storage]</span><span class="sxs-lookup"><span data-stu-id="764da-252">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>
* <span data-ttu-id="764da-253">[Azure SDK úložiště pro uzel] [ Azure Storage SDK for Node] úložišti na Githubu</span><span class="sxs-lookup"><span data-stu-id="764da-253">[Azure Storage SDK for Node][Azure Storage SDK for Node] repository on GitHub</span></span>
* [<span data-ttu-id="764da-254">Středisku pro vývojáře Node.js</span><span class="sxs-lookup"><span data-stu-id="764da-254">Node.js Developer Center</span></span>](https://azure.microsoft.com/develop/nodejs/)
* [<span data-ttu-id="764da-255">Přenos dat pomocí nástroje příkazového řádku AzCopy</span><span class="sxs-lookup"><span data-stu-id="764da-255">Transfer data with the AzCopy command-line utility</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

<span data-ttu-id="764da-256">[Webové aplikace Node.js pomocí služby Azure Table](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)  </span><span class="sxs-lookup"><span data-stu-id="764da-256">[Node.js web app using the Azure Table Service](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)  </span></span>  
<span data-ttu-id="764da-257">[Sestavení a nasazení webové aplikace Node.js do Azure pomocí Web Matrix]: https://www.microsoft.com/web/webmatrix/</span><span class="sxs-lookup"><span data-stu-id="764da-257">[Build and deploy a Node.js web app to Azure using Web Matrix]: https://www.microsoft.com/web/webmatrix/</span></span>  
<span data-ttu-id="764da-258">[Pomocí rozhraní REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx [portál Azure]: https://portal.azure.com [sestavení a nasazení aplikace Node.js ve službě Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) [Blog týmu Azure Storage]: http:// blogs.msdn.com/b/windowsazurestorage/ [úložiště Azure SDK pro uzel referenční dokumentace rozhraní API]: http://dl.windowsazure.com/nodestoragedocs/index.html</span><span class="sxs-lookup"><span data-stu-id="764da-258">[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx [Azure portal]: https://portal.azure.com [Build and deploy a Node.js application to an Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/ [Azure Storage SDK for Node API Reference]: http://dl.windowsazure.com/nodestoragedocs/index.html</span></span>
