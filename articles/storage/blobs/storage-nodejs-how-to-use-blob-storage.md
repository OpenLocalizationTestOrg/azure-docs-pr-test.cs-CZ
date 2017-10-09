---
title: "aaaHow toouse úložiště objektů Blob z Node.js | Microsoft Docs"
description: "Ukládání nestrukturovaných dat v cloudu hello s Azure Blob storage (úložiště objektů)."
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
ms.openlocfilehash: 572e7fc9f7b19ff01720a7cadd495c809ed49fb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-nodejs"></a><span data-ttu-id="c36f8-103">Jak toouse úložiště objektů Blob z Node.js</span><span class="sxs-lookup"><span data-stu-id="c36f8-103">How toouse Blob storage from Node.js</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a><span data-ttu-id="c36f8-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="c36f8-104">Overview</span></span>
<span data-ttu-id="c36f8-105">Tento článek ukazuje, jak tooperform běžné scénáře s využitím úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="c36f8-105">This article shows you how tooperform common scenarios using Blob storage.</span></span> <span data-ttu-id="c36f8-106">Ukázky Hello jsou zapsány pomocí rozhraní API Node.js hello.</span><span class="sxs-lookup"><span data-stu-id="c36f8-106">hello samples are written via hello Node.js API.</span></span> <span data-ttu-id="c36f8-107">pokryté scénáře Hello zahrnují jak tooupload, seznam, stahování a odstraňování objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="c36f8-107">hello scenarios covered include how tooupload, list, download, and delete blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="c36f8-108">Vytvoření aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="c36f8-108">Create a Node.js application</span></span>
<span data-ttu-id="c36f8-109">Návod, jak toocreate aplikace Node.js, v tématu [vytvoření webové aplikace Node.js ve službě Azure App Service], [sestavení a nasazení tooan aplikace Node.js Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) – pomocí prostředí Windows PowerShell nebo [sestavení a nasazení tooAzure webové aplikace Node.js, pomocí Web Matrix](https://www.microsoft.com/web/webmatrix/).</span><span class="sxs-lookup"><span data-stu-id="c36f8-109">For instructions on how toocreate a Node.js application, see [Create a Node.js web app in Azure App Service], [Build and deploy a Node.js application tooan Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) -- using Windows PowerShell, or [Build and deploy a Node.js web app tooAzure using Web Matrix](https://www.microsoft.com/web/webmatrix/).</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="c36f8-110">Konfigurace úložiště tooaccess vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="c36f8-110">Configure your application tooaccess storage</span></span>
<span data-ttu-id="c36f8-111">toouse úložiště Azure, musíte hello sada SDK úložiště Azure pro platformu Node.js, která obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="c36f8-111">toouse Azure storage, you need hello Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="c36f8-112">Použijte uzel balíček správce (NPM) tooobtain hello balíček</span><span class="sxs-lookup"><span data-stu-id="c36f8-112">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="c36f8-113">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix), toonavigate toohello složku, kde můžete vytvořit ukázky aplikace.</span><span class="sxs-lookup"><span data-stu-id="c36f8-113">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix), toonavigate toohello folder where you created your sample application.</span></span>
2. <span data-ttu-id="c36f8-114">Typ **npm nainstalujte azure-storage** v příkazovém okně hello.</span><span class="sxs-lookup"><span data-stu-id="c36f8-114">Type **npm install azure-storage** in hello command window.</span></span> <span data-ttu-id="c36f8-115">Výstup z příkazu hello je podobné toohello následující ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="c36f8-115">Output from hello command is similar toohello following code example.</span></span>

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
3. <span data-ttu-id="c36f8-116">Můžete ručně spustit hello **ls** tooverify příkaz, **uzlu\_moduly** složka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="c36f8-116">You can manually run hello **ls** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="c36f8-117">V této složce najít hello **azure-storage** balíček, který obsahuje hello knihovny, je nutné, aby tooaccess úložiště.</span><span class="sxs-lookup"><span data-stu-id="c36f8-117">Inside that folder, find hello **azure-storage** package, which contains hello libraries that you need tooaccess storage.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="c36f8-118">Importovat balíček hello</span><span class="sxs-lookup"><span data-stu-id="c36f8-118">Import hello package</span></span>
<span data-ttu-id="c36f8-119">Pomocí programu Poznámkový blok nebo jiném textovém editoru, přidejte následující toohello horní části hello hello **server.js** souboru hello aplikace, kde chcete toouse úložiště:</span><span class="sxs-lookup"><span data-stu-id="c36f8-119">Using Notepad or another text editor, add hello following toohello top of hello **server.js** file of hello application where you intend toouse storage:</span></span>

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="c36f8-120">Nastavit připojení k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c36f8-120">Set up an Azure Storage connection</span></span>
<span data-ttu-id="c36f8-121">Hello modulu Azure, bude číst proměnné prostředí hello `AZURE_STORAGE_ACCOUNT` a `AZURE_STORAGE_ACCESS_KEY`, nebo `AZURE_STORAGE_CONNECTION_STRING`, informace o požadovaných účtu úložiště Azure tooyour tooconnect.</span><span class="sxs-lookup"><span data-stu-id="c36f8-121">hello Azure module will read hello environment variables `AZURE_STORAGE_ACCOUNT` and `AZURE_STORAGE_ACCESS_KEY`, or `AZURE_STORAGE_CONNECTION_STRING`, for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="c36f8-122">Pokud nejsou nastavené těchto proměnných prostředí, musíte zadat informace o účtu hello při volání metody **createBlobService**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-122">If these environment variables are not set, you must specify hello account information when calling **createBlobService**.</span></span>

<span data-ttu-id="c36f8-123">Příklad nastavení proměnných prostředí hello v hello [portál Azure](https://portal.azure.com) webové aplikace Azure, najdete v části [Node.js webové aplikace pomocí služby Azure Table hello](../../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span><span class="sxs-lookup"><span data-stu-id="c36f8-123">For an example of setting hello environment variables in hello [Azure portal](https://portal.azure.com) for an Azure web app, see [Node.js web app using hello Azure Table Service](../../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span></span>

## <a name="create-a-container"></a><span data-ttu-id="c36f8-124">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="c36f8-124">Create a container</span></span>
<span data-ttu-id="c36f8-125">Hello **BlobService** objekt vám umožňuje spolupracovat s kontejnery a objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="c36f8-125">hello **BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="c36f8-126">Hello následující kód vytvoří **BlobService** objektu.</span><span class="sxs-lookup"><span data-stu-id="c36f8-126">hello following code creates a **BlobService** object.</span></span> <span data-ttu-id="c36f8-127">Přidejte následující hello v hello horní části **server.js**:</span><span class="sxs-lookup"><span data-stu-id="c36f8-127">Add hello following near hello top of **server.js**:</span></span>

```nodejs
var blobSvc = azure.createBlobService();
```

> [!NOTE]
> <span data-ttu-id="c36f8-128">Objekt blob může anonymně přístup pomocí **createBlobServiceAnonymous** a poskytnutím adresa hostitele hello.</span><span class="sxs-lookup"><span data-stu-id="c36f8-128">You can access a blob anonymously by using **createBlobServiceAnonymous** and providing hello host address.</span></span> <span data-ttu-id="c36f8-129">Například použít `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.</span><span class="sxs-lookup"><span data-stu-id="c36f8-129">For example, use `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.</span></span>
>
>

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="c36f8-130">toocreate nový kontejner, použijte **createContainerIfNotExists**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-130">toocreate a new container, use **createContainerIfNotExists**.</span></span> <span data-ttu-id="c36f8-131">Hello následující příklad kódu vytvoří nový kontejner s názvem 'můj_kontejner':</span><span class="sxs-lookup"><span data-stu-id="c36f8-131">hello following code example creates a new container named 'mycontainer':</span></span>

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
    if(!error){
      // Container exists and is private
    }
});
```

<span data-ttu-id="c36f8-132">Pokud je nově vytvořený kontejner hello, `result.created` hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="c36f8-132">If hello container is newly created, `result.created` is true.</span></span> <span data-ttu-id="c36f8-133">Pokud hello kontejneru již existuje, `result.created` je false.</span><span class="sxs-lookup"><span data-stu-id="c36f8-133">If hello container already exists, `result.created` is false.</span></span> <span data-ttu-id="c36f8-134">`response`obsahuje informace o operaci hello, včetně informací o ETag hello hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c36f8-134">`response` contains information about hello operation, including hello ETag information for hello container.</span></span>

### <a name="container-security"></a><span data-ttu-id="c36f8-135">Kontejner zabezpečení</span><span class="sxs-lookup"><span data-stu-id="c36f8-135">Container security</span></span>
<span data-ttu-id="c36f8-136">Ve výchozím nastavení nové kontejnery jsou soukromá a nemohou být získat anonymní přístup.</span><span class="sxs-lookup"><span data-stu-id="c36f8-136">By default, new containers are private and cannot be accessed anonymously.</span></span> <span data-ttu-id="c36f8-137">kontejner hello toomake veřejné, tak, aby ho může anonymně přístup, můžete nastavit úroveň přístupu kontejneru hello také**blob** nebo **kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-137">toomake hello container public so that you can access it anonymously, you can set hello container's access level too**blob** or **container**.</span></span>

* <span data-ttu-id="c36f8-138">**objekt BLOB** -umožňuje anonymní přístup pro čtení tooblob obsahu a metadat v tomto kontejneru, ale není toocontainer metadata například výpis všech objektů BLOB do kontejneru</span><span class="sxs-lookup"><span data-stu-id="c36f8-138">**blob** - allows anonymous read access tooblob content and metadata within this container, but not toocontainer metadata such as listing all blobs within a container</span></span>
* <span data-ttu-id="c36f8-139">**kontejner** -umožňuje anonymní přístup pro čtení tooblob obsahu a metadat, jakož i metadata kontejneru</span><span class="sxs-lookup"><span data-stu-id="c36f8-139">**container** - allows anonymous read access tooblob content and metadata as well as container metadata</span></span>

<span data-ttu-id="c36f8-140">Hello následující příklad kódu ukazuje nastavení úrovně přístupu hello příliš**blob**:</span><span class="sxs-lookup"><span data-stu-id="c36f8-140">hello following code example demonstrates setting hello access level too**blob**:</span></span>

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
    if(!error){
      // Container exists and allows
      // anonymous read access tooblob
      // content and metadata within this container
    }
});
```

<span data-ttu-id="c36f8-141">Alternativně můžete upravit úroveň přístupu hello kontejneru pomocí **setContainerAcl** úroveň přístupu toospecify hello.</span><span class="sxs-lookup"><span data-stu-id="c36f8-141">Alternatively, you can modify hello access level of a container by using **setContainerAcl** toospecify hello access level.</span></span> <span data-ttu-id="c36f8-142">Hello následující kód například změny hello přístup úrovně toocontainer:</span><span class="sxs-lookup"><span data-stu-id="c36f8-142">hello following code example changes hello access level toocontainer:</span></span>

```nodejs
blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
  if(!error){
    // Container access level set too'container'
  }
});
```

<span data-ttu-id="c36f8-143">Hello výsledek obsahuje informace o operaci hello, včetně aktuální hello **značka ETag** pro kontejner hello.</span><span class="sxs-lookup"><span data-stu-id="c36f8-143">hello result contains information about hello operation, including hello current **ETag** for hello container.</span></span>

### <a name="filters"></a><span data-ttu-id="c36f8-144">Filtry</span><span class="sxs-lookup"><span data-stu-id="c36f8-144">Filters</span></span>
<span data-ttu-id="c36f8-145">Můžete provést volitelný filtrování operací toooperations provést pomocí **BlobService**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-145">You can apply optional filtering operations toooperations performed using **BlobService**.</span></span> <span data-ttu-id="c36f8-146">Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu podpisem hello:</span><span class="sxs-lookup"><span data-stu-id="c36f8-146">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```nodejs
function handle (requestOptions, next)
```

<span data-ttu-id="c36f8-147">Až to předzpracování na možnosti hello požadavku, hello musí metoda toocall tlačítko Další"předání zpětné volání s hello následující podpis:</span><span class="sxs-lookup"><span data-stu-id="c36f8-147">After doing its preprocessing on hello request options, hello method needs toocall "next", passing a callback with hello following signature:</span></span>

```nodejs
function (returnObject, finalCallback, next)
```

<span data-ttu-id="c36f8-148">V této zpětného volání a po zpracování hello returnObject (hello odpověď ze serveru toohello požadavek hello), zpětné volání hello musí tooeither vyvolat další, pokud existuje toocontinue zpracování ostatní filtry nebo jednoduše vyvolat finalCallback tooend hello služby volání.</span><span class="sxs-lookup"><span data-stu-id="c36f8-148">In this callback, and after processing hello returnObject (hello response from hello request toohello server), hello callback needs tooeither invoke next if it exists toocontinue processing other filters or simply invoke finalCallback tooend hello service invocation.</span></span>

<span data-ttu-id="c36f8-149">Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí hello Azure SDK pro Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-149">Two filters that implement retry logic are included with hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="c36f8-150">vytvoří následující Hello **BlobService** objekt, který používá hello **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="c36f8-150">hello following creates a **BlobService** object that uses hello **ExponentialRetryPolicyFilter**:</span></span>

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var blobSvc = azure.createBlobService().withFilter(retryOperations);
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="c36f8-151">Nahrání objektu blob do kontejneru</span><span class="sxs-lookup"><span data-stu-id="c36f8-151">Upload a blob into a container</span></span>
<span data-ttu-id="c36f8-152">Existují tři typy objektů blob: objekty BLOB bloků, objekty BLOB stránky a doplňovacích objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="c36f8-152">There are three types of blobs: block blobs, page blobs and append blobs.</span></span> <span data-ttu-id="c36f8-153">Objekty BLOB bloku umožňují toomore efektivně odeslání velkého množství dat.</span><span class="sxs-lookup"><span data-stu-id="c36f8-153">Block blobs allow you toomore efficiently upload large data.</span></span> <span data-ttu-id="c36f8-154">Append – objekty BLOB jsou optimalizované pro doplňovací operace.</span><span class="sxs-lookup"><span data-stu-id="c36f8-154">Append blobs are optimized for append operations.</span></span> <span data-ttu-id="c36f8-155">Objekty BLOB stránky jsou optimalizované pro operace čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="c36f8-155">Page blobs are optimized for read/write operations.</span></span> <span data-ttu-id="c36f8-156">Další informace najdete v tématu [Principy objekty BLOB bloku a doplňovacích objektů BLOB, objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="c36f8-156">For more information, see [Understanding Block Blobs, Append Blobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

### <a name="block-blobs"></a><span data-ttu-id="c36f8-157">Objekty blob bloku</span><span class="sxs-lookup"><span data-stu-id="c36f8-157">Block blobs</span></span>
<span data-ttu-id="c36f8-158">tooupload data tooa objekt blob bloku, použijte hello následující:</span><span class="sxs-lookup"><span data-stu-id="c36f8-158">tooupload data tooa block blob, use hello following:</span></span>

* <span data-ttu-id="c36f8-159">**createBlockBlobFromLocalFile** – vytvoří nový objekt blob bloku a odesílá hello obsah souboru</span><span class="sxs-lookup"><span data-stu-id="c36f8-159">**createBlockBlobFromLocalFile** - creates a new block blob and uploads hello contents of a file</span></span>
* <span data-ttu-id="c36f8-160">**createBlockBlobFromStream** – vytvoří nový objekt blob bloku a odesílá hello obsah datového proudu</span><span class="sxs-lookup"><span data-stu-id="c36f8-160">**createBlockBlobFromStream** - creates a new block blob and uploads hello contents of a stream</span></span>
* <span data-ttu-id="c36f8-161">**createBlockBlobFromText** – vytvoří nový objekt blob bloku a odesílá hello obsah řetězce</span><span class="sxs-lookup"><span data-stu-id="c36f8-161">**createBlockBlobFromText** - creates a new block blob and uploads hello contents of a string</span></span>
* <span data-ttu-id="c36f8-162">**createWriteStreamToBlockBlob** – obsahuje objekt blob bloku tooa zápisu datového proudu</span><span class="sxs-lookup"><span data-stu-id="c36f8-162">**createWriteStreamToBlockBlob** - provides a write stream tooa block blob</span></span>

<span data-ttu-id="c36f8-163">Hello následující příklad kódu odešle hello obsah hello **test.txt** soubor do **můj_objekt_blob**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-163">hello following code example uploads hello contents of hello **test.txt** file into **myblob**.</span></span>

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="c36f8-164">Hello `result` vrácený tyto metody obsahuje informace o operaci hello, jako je například hello **značka ETag** objektu hello blob.</span><span class="sxs-lookup"><span data-stu-id="c36f8-164">hello `result` returned by these methods contains information on hello operation, such as hello **ETag** of hello blob.</span></span>

### <a name="append-blobs"></a><span data-ttu-id="c36f8-165">Doplňovací objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="c36f8-165">Append blobs</span></span>
<span data-ttu-id="c36f8-166">tooupload data tooa nové připojení objektů blob, použijte hello:</span><span class="sxs-lookup"><span data-stu-id="c36f8-166">tooupload data tooa new append blob, use hello following:</span></span>

* <span data-ttu-id="c36f8-167">**createAppendBlobFromLocalFile** – vytvoří nový doplňovací objekt blob a odesílá hello obsah souboru</span><span class="sxs-lookup"><span data-stu-id="c36f8-167">**createAppendBlobFromLocalFile** - creates a new append blob and uploads hello contents of a file</span></span>
* <span data-ttu-id="c36f8-168">**createAppendBlobFromStream** – vytvoří nový doplňovací objekt blob a odesílá hello obsah datového proudu</span><span class="sxs-lookup"><span data-stu-id="c36f8-168">**createAppendBlobFromStream** - creates a new append blob and uploads hello contents of a stream</span></span>
* <span data-ttu-id="c36f8-169">**createAppendBlobFromText** – vytvoří nový doplňovací objekt blob a odesílá hello obsah řetězce</span><span class="sxs-lookup"><span data-stu-id="c36f8-169">**createAppendBlobFromText** - creates a new append blob and uploads hello contents of a string</span></span>
* <span data-ttu-id="c36f8-170">**createWriteStreamToNewAppendBlob** – vytvoří nový doplňovací objekt blob a pak poskytuje tooit toowrite datového proudu</span><span class="sxs-lookup"><span data-stu-id="c36f8-170">**createWriteStreamToNewAppendBlob** - creates a new append blob and then provides a stream toowrite tooit</span></span>

<span data-ttu-id="c36f8-171">Hello následující příklad kódu odešle hello obsah hello **test.txt** soubor do **myappendblob**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-171">hello following code example uploads hello contents of hello **test.txt** file into **myappendblob**.</span></span>

```nodejs
blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="c36f8-172">tooappend tooan bloku existující připojení objektů blob, použijte hello následující:</span><span class="sxs-lookup"><span data-stu-id="c36f8-172">tooappend a block tooan existing append blob, use hello following:</span></span>

* <span data-ttu-id="c36f8-173">**appendFromLocalFile** -připojit hello obsah ze souboru tooan existující připojení objektů blob</span><span class="sxs-lookup"><span data-stu-id="c36f8-173">**appendFromLocalFile** - append hello contents of a file tooan existing append blob</span></span>
* <span data-ttu-id="c36f8-174">**appendFromStream** -připojit hello obsah z datového proudu tooan existující připojení objektů blob</span><span class="sxs-lookup"><span data-stu-id="c36f8-174">**appendFromStream** - append hello contents of a stream tooan existing append blob</span></span>
* <span data-ttu-id="c36f8-175">**appendFromText** -připojit hello obsah z řetězce tooan existující připojení objektů blob</span><span class="sxs-lookup"><span data-stu-id="c36f8-175">**appendFromText** - append hello contents of a string tooan existing append blob</span></span>
* <span data-ttu-id="c36f8-176">**appendBlockFromStream** -připojit hello obsah z datového proudu tooan existující připojení objektů blob</span><span class="sxs-lookup"><span data-stu-id="c36f8-176">**appendBlockFromStream** - append hello contents of a stream tooan existing append blob</span></span>
* <span data-ttu-id="c36f8-177">**appendBlockFromText** -připojit hello obsah z řetězce tooan existující připojení objektů blob</span><span class="sxs-lookup"><span data-stu-id="c36f8-177">**appendBlockFromText** - append hello contents of a string tooan existing append blob</span></span>

> [!NOTE]
> <span data-ttu-id="c36f8-178">appendFromXXX rozhraní API provede volání nepotřebné serveru rychlé tooavoid toofail některé ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="c36f8-178">appendFromXXX APIs will do some client-side validation toofail fast tooavoid unnecessary server calls.</span></span> <span data-ttu-id="c36f8-179">appendBlockFromXXX nebude.</span><span class="sxs-lookup"><span data-stu-id="c36f8-179">appendBlockFromXXX won't.</span></span>
>
>

<span data-ttu-id="c36f8-180">Hello následující příklad kódu odešle hello obsah hello **test.txt** soubor do **myappendblob**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-180">hello following code example uploads hello contents of hello **test.txt** file into **myappendblob**.</span></span>

```nodejs
blobSvc.appendFromText('mycontainer', 'myappendblob', 'text toobe appended', function(error, result, response){
  if(!error){
    // text appended
  }
});
```

### <a name="page-blobs"></a><span data-ttu-id="c36f8-181">Objekty blob stránky</span><span class="sxs-lookup"><span data-stu-id="c36f8-181">Page blobs</span></span>
<span data-ttu-id="c36f8-182">tooupload data tooa objekt blob stránky, použijte hello následující:</span><span class="sxs-lookup"><span data-stu-id="c36f8-182">tooupload data tooa page blob, use hello following:</span></span>

* <span data-ttu-id="c36f8-183">**createPageBlob** -vytvoří nový objekt blob stránky konkrétní délky</span><span class="sxs-lookup"><span data-stu-id="c36f8-183">**createPageBlob** - creates a new page blob of a specific length</span></span>
* <span data-ttu-id="c36f8-184">**createPageBlobFromLocalFile** – vytvoří nový objekt blob stránky a odesílá hello obsah souboru</span><span class="sxs-lookup"><span data-stu-id="c36f8-184">**createPageBlobFromLocalFile** - creates a new page blob and uploads hello contents of a file</span></span>
* <span data-ttu-id="c36f8-185">**createPageBlobFromStream** – vytvoří nový objekt blob stránky a odesílá hello obsah datového proudu</span><span class="sxs-lookup"><span data-stu-id="c36f8-185">**createPageBlobFromStream** - creates a new page blob and uploads hello contents of a stream</span></span>
* <span data-ttu-id="c36f8-186">**createWriteStreamToExistingPageBlob** -poskytuje zápisu datového proudu tooan existující objekt blob stránky</span><span class="sxs-lookup"><span data-stu-id="c36f8-186">**createWriteStreamToExistingPageBlob** - provides a write stream tooan existing page blob</span></span>
* <span data-ttu-id="c36f8-187">**createWriteStreamToNewPageBlob** – vytvoří nový objekt blob stránky a pak poskytuje tooit toowrite datového proudu</span><span class="sxs-lookup"><span data-stu-id="c36f8-187">**createWriteStreamToNewPageBlob** - creates a new page blob and then provides a stream toowrite tooit</span></span>

<span data-ttu-id="c36f8-188">Hello následující příklad kódu odešle hello obsah hello **test.txt** soubor do **mypageblob**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-188">hello following code example uploads hello contents of hello **test.txt** file into **mypageblob**.</span></span>

```nodejs
blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

> [!NOTE]
> <span data-ttu-id="c36f8-189">Objekty BLOB stránky obsahovat 512 bajtů stránky.</span><span class="sxs-lookup"><span data-stu-id="c36f8-189">Page blobs consist of 512-byte 'pages'.</span></span> <span data-ttu-id="c36f8-190">Obdržíte chybu při odesílání dat s velikostí, která není násobkem 512.</span><span class="sxs-lookup"><span data-stu-id="c36f8-190">You will receive an error when uploading data with a size that is not a multiple of 512.</span></span>
>
>

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="c36f8-191">Seznam hello objekty BLOB v kontejneru</span><span class="sxs-lookup"><span data-stu-id="c36f8-191">List hello blobs in a container</span></span>
<span data-ttu-id="c36f8-192">toolist hello objekty BLOB v kontejneru, použijte hello **listBlobsSegmented** metoda.</span><span class="sxs-lookup"><span data-stu-id="c36f8-192">toolist hello blobs in a container, use hello **listBlobsSegmented** method.</span></span> <span data-ttu-id="c36f8-193">Pokud chcete objekty BLOB tooreturn s konkrétní předponou, použijte **listBlobsSegmentedWithPrefix**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-193">If you'd like tooreturn blobs with a specific prefix, use **listBlobsSegmentedWithPrefix**.</span></span>

```nodejs
blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
  if(!error){
      // result.entries contains hello entries
      // If not all blobs were returned, result.continuationToken has hello continuation token.
  }
});
```

<span data-ttu-id="c36f8-194">Hello `result` obsahuje `entries` kolekce, která je pole objektů, které popisují jednotlivých objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c36f8-194">hello `result` contains an `entries` collection, which is an array of objects that describe each blob.</span></span> <span data-ttu-id="c36f8-195">Pokud nemůže být vrácen všech objektů BLOB, hello `result` také poskytuje `continuationToken`, které můžete použít jako hello druhý parametr tooretrieve další záznamy.</span><span class="sxs-lookup"><span data-stu-id="c36f8-195">If all blobs cannot be returned, hello `result` also provides a `continuationToken`, which you may use as hello second parameter tooretrieve additional entries.</span></span>

## <a name="download-blobs"></a><span data-ttu-id="c36f8-196">Stáhnout objekty blob</span><span class="sxs-lookup"><span data-stu-id="c36f8-196">Download blobs</span></span>
<span data-ttu-id="c36f8-197">toodownload data z objektu blob, použijte následující hello:</span><span class="sxs-lookup"><span data-stu-id="c36f8-197">toodownload data from a blob, use hello following:</span></span>

* <span data-ttu-id="c36f8-198">**getBlobToLocalFile** -zapíše toofile obsah objektu blob hello</span><span class="sxs-lookup"><span data-stu-id="c36f8-198">**getBlobToLocalFile** - writes hello blob contents toofile</span></span>
* <span data-ttu-id="c36f8-199">**getBlobToStream** -zapíše hello datového proudu tooa obsah objektu blob</span><span class="sxs-lookup"><span data-stu-id="c36f8-199">**getBlobToStream** - writes hello blob contents tooa stream</span></span>
* <span data-ttu-id="c36f8-200">**getBlobToText** -zapíše obsahu objektu blob hello tooa řetězec</span><span class="sxs-lookup"><span data-stu-id="c36f8-200">**getBlobToText** - writes hello blob contents tooa string</span></span>
* <span data-ttu-id="c36f8-201">**createReadStream** -poskytuje tooread datového proudu z objektu blob hello</span><span class="sxs-lookup"><span data-stu-id="c36f8-201">**createReadStream** - provides a stream tooread from hello blob</span></span>

<span data-ttu-id="c36f8-202">Hello následující příklad kódu ukazuje, jak pomocí **getBlobToStream** toodownload hello obsah hello **můj_objekt_blob** objektů blob a uložte ho toohello **výstup.txt** souboru pomocí datový proud:</span><span class="sxs-lookup"><span data-stu-id="c36f8-202">hello following code example demonstrates using **getBlobToStream** toodownload hello contents of hello **myblob** blob and store it toohello **output.txt** file by using a stream:</span></span>

```nodejs
var fs = require('fs');
blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
  if(!error){
    // blob retrieved
  }
});
```

<span data-ttu-id="c36f8-203">Hello `result` obsahuje informace o objektu blob hello, včetně **značka ETag** informace.</span><span class="sxs-lookup"><span data-stu-id="c36f8-203">hello `result` contains information about hello blob, including **ETag** information.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="c36f8-204">Odstranění objektu blob</span><span class="sxs-lookup"><span data-stu-id="c36f8-204">Delete a blob</span></span>
<span data-ttu-id="c36f8-205">Nakonec toodelete objekt blob, zavolejte **deleteBlob**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-205">Finally, toodelete a blob, call **deleteBlob**.</span></span> <span data-ttu-id="c36f8-206">Následující ukázka kódu hello odstranění objektů blob s názvem Hello **můj_objekt_blob**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-206">hello following code example deletes hello blob named **myblob**.</span></span>

```nodejs
blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
  if(!error){
    // Blob has been deleted
  }
});
```

## <a name="concurrent-access"></a><span data-ttu-id="c36f8-207">Souběžný přístup</span><span class="sxs-lookup"><span data-stu-id="c36f8-207">Concurrent access</span></span>
<span data-ttu-id="c36f8-208">blob tooa toosupport souběžný přístup z více klientů nebo více instancí procesu, můžete použít **značky etag binárním rozsáhlým** nebo **zapůjčení**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-208">toosupport concurrent access tooa blob from multiple clients or multiple process instances, you can use **ETags** or **leases**.</span></span>

* <span data-ttu-id="c36f8-209">**Značka Etag** -poskytuje toodetect způsobem, který hello objektů blob nebo kontejner, byla změněna jiným procesem</span><span class="sxs-lookup"><span data-stu-id="c36f8-209">**Etag** - provides a way toodetect that hello blob or container has been modified by another process</span></span>
* <span data-ttu-id="c36f8-210">**Zapůjčení** – poskytuje způsob tooobtain výhradní, obnovitelné, zápis nebo odstranit objekt blob tooa přístup pro v časovém intervalu</span><span class="sxs-lookup"><span data-stu-id="c36f8-210">**Lease** - provides a way tooobtain exclusive, renewable, write or delete access tooa blob for a period of time</span></span>

### <a name="etag"></a><span data-ttu-id="c36f8-211">Značka ETag</span><span class="sxs-lookup"><span data-stu-id="c36f8-211">ETag</span></span>
<span data-ttu-id="c36f8-212">Značky etag binárním rozsáhlým použijte, pokud potřebujete tooallow více klientů nebo instancí toowrite toohello blok objektů Blob nebo stránky objektu Blob současně.</span><span class="sxs-lookup"><span data-stu-id="c36f8-212">Use ETags if you need tooallow multiple clients or instances toowrite toohello block Blob or page Blob simultaneously.</span></span> <span data-ttu-id="c36f8-213">Hello ETag vám umožní toodetermine Pokud hello kontejner nebo objekt blob byla změněna od začátku čtení nebo vytvořili, což vám umožní tooavoid přepsal změny potvrzeny jiný klienta nebo proces.</span><span class="sxs-lookup"><span data-stu-id="c36f8-213">hello ETag allows you toodetermine if hello container or blob was modified since you initially read or created it, which allows you tooavoid overwriting changes committed by another client or process.</span></span>

<span data-ttu-id="c36f8-214">Značka ETag podmínky lze nastavit pomocí hello volitelné `options.accessConditions` parametr.</span><span class="sxs-lookup"><span data-stu-id="c36f8-214">You can set ETag conditions by using hello optional `options.accessConditions` parameter.</span></span> <span data-ttu-id="c36f8-215">Hello následující příklad kódu pouze odešle hello **test.txt** soubor, pokud objekt blob hello již existuje a má hodnotu ETag hello obsahoval podle `etagToMatch`.</span><span class="sxs-lookup"><span data-stu-id="c36f8-215">hello following code example only uploads hello **test.txt** file if hello blob already exists and has hello ETag value contained by `etagToMatch`.</span></span>

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
    if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="c36f8-216">Při použití značky etag binárním rozsáhlým, je obecný vzor hello:</span><span class="sxs-lookup"><span data-stu-id="c36f8-216">When you're using ETags, hello general pattern is:</span></span>

1. <span data-ttu-id="c36f8-217">Získáte hello ETag jako výsledek hello create, seznamu nebo operaci get.</span><span class="sxs-lookup"><span data-stu-id="c36f8-217">Obtain hello ETag as hello result of a create, list, or get operation.</span></span>
2. <span data-ttu-id="c36f8-218">Provedení akce, kontrola této hello hodnota ETag nebyl změněn.</span><span class="sxs-lookup"><span data-stu-id="c36f8-218">Perform an action, checking that hello ETag value has not been modified.</span></span>

<span data-ttu-id="c36f8-219">Pokud byla změněna hello hodnotu, znamená to, že jiný klienta nebo instance upraveny hello blob nebo kontejneru vzhledem k tomu, že jste získali hodnota ETag hello.</span><span class="sxs-lookup"><span data-stu-id="c36f8-219">If hello value was modified, this indicates that another client or instance modified hello blob or container since you obtained hello ETag value.</span></span>

### <a name="lease"></a><span data-ttu-id="c36f8-220">Zapůjčení</span><span class="sxs-lookup"><span data-stu-id="c36f8-220">Lease</span></span>
<span data-ttu-id="c36f8-221">Nové zapůjčení můžete získat pomocí hello **acquireLease** metoda, zadání hello blob nebo kontejner, u nějž chcete zapůjčení pro tooobtain na.</span><span class="sxs-lookup"><span data-stu-id="c36f8-221">You can acquire a new lease by using hello **acquireLease** method, specifying hello blob or container that you wish tooobtain a lease on.</span></span> <span data-ttu-id="c36f8-222">Například následující kód hello získá zapůjčení na **můj_objekt_blob**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-222">For example, hello following code acquires a lease on **myblob**.</span></span>

```nodejs
blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
  if(!error) {
    console.log('leaseId: ' + result.id);
  }
});
```

<span data-ttu-id="c36f8-223">Dalších operacích na **můj_objekt_blob** musíte zadat hello `options.leaseId` parametr.</span><span class="sxs-lookup"><span data-stu-id="c36f8-223">Subsequent operations on **myblob** must provide hello `options.leaseId` parameter.</span></span> <span data-ttu-id="c36f8-224">Hello zapůjčení ID se vrátí jako `result.id` z **acquireLease**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-224">hello lease ID is returned as `result.id` from **acquireLease**.</span></span>

> [!NOTE]
> <span data-ttu-id="c36f8-225">Ve výchozím nastavení je neomezenou dobu trvání zápůjčky hello.</span><span class="sxs-lookup"><span data-stu-id="c36f8-225">By default, hello lease duration is infinite.</span></span> <span data-ttu-id="c36f8-226">Můžete zadat dobu trvání bez nekonečné (mezi 15 a 60 sekund) tím, že poskytuje hello `options.leaseDuration` parametr.</span><span class="sxs-lookup"><span data-stu-id="c36f8-226">You can specify a non-infinite duration (between 15 and 60 seconds) by providing hello `options.leaseDuration` parameter.</span></span>
>
>

<span data-ttu-id="c36f8-227">použít tooremove zapůjčení, **releaseLease**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-227">tooremove a lease, use **releaseLease**.</span></span> <span data-ttu-id="c36f8-228">toobreak zapůjčení, ale zabránit jiným získat nové zapůjčení, dokud nevyprší doba trvání původní text hello, použijte **breakLease**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-228">toobreak a lease, but prevent others from obtaining a new lease until hello original duration has expired, use **breakLease**.</span></span>

## <a name="work-with-shared-access-signatures"></a><span data-ttu-id="c36f8-229">Práce s podpisy sdíleného přístupu</span><span class="sxs-lookup"><span data-stu-id="c36f8-229">Work with shared access signatures</span></span>
<span data-ttu-id="c36f8-230">Sdílené přístupové podpisy (SAS) jsou bezpečný tooprovide granulární přístup tooblobs a kontejnery bez zadání názvu účtu úložiště nebo klíče.</span><span class="sxs-lookup"><span data-stu-id="c36f8-230">Shared access signatures (SAS) are a secure way tooprovide granular access tooblobs and containers without providing your storage account name or keys.</span></span> <span data-ttu-id="c36f8-231">Sdílené přístupové podpisy jsou často používané tooprovide omezený přístup tooyour data, například povolení mobilní aplikace tooaccess objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="c36f8-231">Shared access signatures are often used tooprovide limited access tooyour data, such as allowing a mobile app tooaccess blobs.</span></span>

> [!NOTE]
> <span data-ttu-id="c36f8-232">Při můžete povolit také tooblobs anonymní přístup, sdílené přístupové podpisy umožní tooprovide více řízený přístup, protože musíte vygenerovat hello SAS.</span><span class="sxs-lookup"><span data-stu-id="c36f8-232">While you can also allow anonymous access tooblobs, shared access signatures allow you tooprovide more controlled access, as you must generate hello SAS.</span></span>
>
>

<span data-ttu-id="c36f8-233">Důvěryhodné aplikace, jako je Cloudová služba generuje sdílené přístupové podpisy pomocí hello **generateSharedAccessSignature** z hello **BlobService**a poskytuje ji tooan nedůvěryhodná nebo částečně důvěryhodné aplikace například mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="c36f8-233">A trusted application such as a cloud-based service generates shared access signatures using hello **generateSharedAccessSignature** of hello **BlobService**, and provides it tooan untrusted or semi-trusted application such as a mobile app.</span></span> <span data-ttu-id="c36f8-234">Sdílený přístupový podpisy se generují pomocí zásad, která popisuje hello počáteční a koncové datum, během které hello sdílené přístupové podpisy jsou platné, jakož i hello přístup úrovně oprávnění vlastníka pro podpisy sdíleného přístupu toohello.</span><span class="sxs-lookup"><span data-stu-id="c36f8-234">Shared access signatures are generated using a policy, which describes hello start and end dates during which hello shared access signatures are valid, as well as hello access level granted toohello shared access signatures holder.</span></span>

<span data-ttu-id="c36f8-235">Hello následující příklad kódu vytvoří nová zásada sdíleného přístupu, která umožňuje čtení hello sdíleného přístupu podpisy držitel tooperform na hello **můj_objekt_blob** objektů blob a jeho platnost vyprší 100 minut po času hello je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="c36f8-235">hello following code example generates a new shared access policy that allows hello shared access signatures holder tooperform read operations on hello **myblob** blob, and expires 100 minutes after hello time it is created.</span></span>

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

<span data-ttu-id="c36f8-236">Všimněte si, že informace o hostiteli hello je nutno zadat také, jako je povinný při hello sdílené přístupové podpisy držitel pokusí tooaccess hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c36f8-236">Note that hello host information must be provided also, as it is required when hello shared access signatures holder attempts tooaccess hello container.</span></span>

<span data-ttu-id="c36f8-237">Hello klientská aplikace pak používá sdílené přístupové podpisy s **BlobServiceWithSAS** tooperform operace u objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="c36f8-237">hello client application then uses shared access signatures with **BlobServiceWithSAS** tooperform operations against hello blob.</span></span> <span data-ttu-id="c36f8-238">Hello následující získá informace **můj_objekt_blob**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-238">hello following gets information about **myblob**.</span></span>

```nodejs
var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
  if(!error) {
    // retrieved info
  }
});
```

<span data-ttu-id="c36f8-239">Vzhledem k tomu, že hello sdílené přístupové podpisy byly vygenerovány s přístupem jen pro čtení, pokud je proveden pokus o toomodify hello blob, bude vrácena chyba.</span><span class="sxs-lookup"><span data-stu-id="c36f8-239">Since hello shared access signatures were generated with read-only access, if an attempt is made toomodify hello blob, an error will be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="c36f8-240">Seznamy řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="c36f8-240">Access control lists</span></span>
<span data-ttu-id="c36f8-241">Můžete taky zásad řízení přístupu (ACL) seznamu tooset hello přístup pro SAS.</span><span class="sxs-lookup"><span data-stu-id="c36f8-241">You can also use an access control list (ACL) tooset hello access policy for SAS.</span></span> <span data-ttu-id="c36f8-242">To je užitečné, pokud chcete tooallow více klientů tooaccess kontejner, ale poskytnutí zásad, jiný přístup pro každého klienta.</span><span class="sxs-lookup"><span data-stu-id="c36f8-242">This is useful if you wish tooallow multiple clients tooaccess a container but provide different access policies for each client.</span></span>

<span data-ttu-id="c36f8-243">Seznam ACL je implementovaná pomocí pole zásady přístupu s ID spojené s každou zásadu.</span><span class="sxs-lookup"><span data-stu-id="c36f8-243">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="c36f8-244">Následující ukázka kódu Hello definuje dvě zásady, jeden pro "uživatel1" a jeden pro, uživatel2":</span><span class="sxs-lookup"><span data-stu-id="c36f8-244">hello following code example defines two policies, one for 'user1' and one for 'user2':</span></span>

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

<span data-ttu-id="c36f8-245">Následující kód například získá Hello hello aktuální seznam ACL pro **můj_kontejner**a potom se přidají nové zásady hello pomocí **setBlobAcl**.</span><span class="sxs-lookup"><span data-stu-id="c36f8-245">hello following code example gets hello current ACL for **mycontainer**, and then adds hello new policies using **setBlobAcl**.</span></span> <span data-ttu-id="c36f8-246">Tento přístup umožňuje:</span><span class="sxs-lookup"><span data-stu-id="c36f8-246">This approach allows:</span></span>

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

<span data-ttu-id="c36f8-247">Jednou hello seznamu ACL je nastavena, potom můžete vytvořit sdílené přístupové podpisy, na základě hello ID zásad.</span><span class="sxs-lookup"><span data-stu-id="c36f8-247">Once hello ACL is set, you can then create shared access signatures based on hello ID for a policy.</span></span> <span data-ttu-id="c36f8-248">Následující ukázka kódu Hello vytvoří nové sdílené přístupové podpisy pro, uživatel2":</span><span class="sxs-lookup"><span data-stu-id="c36f8-248">hello following code example creates new shared access signatures for 'user2':</span></span>

```nodejs
blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="c36f8-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c36f8-249">Next steps</span></span>
<span data-ttu-id="c36f8-250">Další informace najdete v tématu hello následující prostředky.</span><span class="sxs-lookup"><span data-stu-id="c36f8-250">For more information, see hello following resources.</span></span>

* <span data-ttu-id="c36f8-251">[Úložiště azure SDK pro uzel referenční dokumentace rozhraní API] [Úložiště azure SDK pro uzel referenční dokumentace rozhraní API]</span><span class="sxs-lookup"><span data-stu-id="c36f8-251">[Azure Storage SDK for Node API Reference][Azure Storage SDK for Node API Reference]</span></span>
* <span data-ttu-id="c36f8-252">[Blog týmu azure Storage] [Blog týmu azure Storage]</span><span class="sxs-lookup"><span data-stu-id="c36f8-252">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>
* <span data-ttu-id="c36f8-253">[Azure SDK úložiště pro uzel] [ Azure Storage SDK for Node] úložišti na Githubu</span><span class="sxs-lookup"><span data-stu-id="c36f8-253">[Azure Storage SDK for Node][Azure Storage SDK for Node] repository on GitHub</span></span>
* [<span data-ttu-id="c36f8-254">Středisko pro vývojáře Node.js</span><span class="sxs-lookup"><span data-stu-id="c36f8-254">Node.js Developer Center</span></span>](https://azure.microsoft.com/develop/nodejs/)
* [<span data-ttu-id="c36f8-255">Přenos dat pomocí hello příkazového řádku azcopy</span><span class="sxs-lookup"><span data-stu-id="c36f8-255">Transfer data with hello AzCopy command-line utility</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

<span data-ttu-id="c36f8-256">[Webové aplikace Node.js pomocí hello služby Azure Table](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)  </span><span class="sxs-lookup"><span data-stu-id="c36f8-256">[Node.js web app using hello Azure Table Service](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)  </span></span>  
<span data-ttu-id="c36f8-257">[Sestavení a nasazení tooAzure webové aplikace Node.js, pomocí Web Matrix]: https://www.microsoft.com/web/webmatrix/</span><span class="sxs-lookup"><span data-stu-id="c36f8-257">[Build and deploy a Node.js web app tooAzure using Web Matrix]: https://www.microsoft.com/web/webmatrix/</span></span>  
<span data-ttu-id="c36f8-258">[REST API pomocí hello]: http://msdn.microsoft.com/library/azure/hh264518.aspx [portál Azure]: https://portal.azure.com [sestavení a nasazení tooan aplikace Node.js Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) [Blog týmu Azure Storage]: http:// blogs.msdn.com/b/windowsazurestorage/ [úložiště Azure SDK pro uzel referenční dokumentace rozhraní API]: http://dl.windowsazure.com/nodestoragedocs/index.html</span><span class="sxs-lookup"><span data-stu-id="c36f8-258">[Using hello REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx [Azure portal]: https://portal.azure.com [Build and deploy a Node.js application tooan Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/ [Azure Storage SDK for Node API Reference]: http://dl.windowsazure.com/nodestoragedocs/index.html</span></span>
