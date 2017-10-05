---
title: "Používání úložiště Azure Table z Node.js | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu pomocí Azure Table Storage, úložiště dat typu NoSQL."
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: fc2e33d2-c5da-4861-8503-53fdc25750de
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 539212c6abe7738c022d67245f8992516f0899ff
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-table-storage-from-nodejs"></a><span data-ttu-id="c9a46-103">Používání úložiště Azure Table z Node.js</span><span class="sxs-lookup"><span data-stu-id="c9a46-103">How to use Azure Table storage from Node.js</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="c9a46-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="c9a46-104">Overview</span></span>
<span data-ttu-id="c9a46-105">Toto téma ukazuje, jak provádět běžné scénáře s využitím služby Azure Table v aplikaci Node.js.</span><span class="sxs-lookup"><span data-stu-id="c9a46-105">This topic shows how to perform common scenarios using the Azure Table service in a Node.js application.</span></span>

<span data-ttu-id="c9a46-106">Příklady kódu v tomto tématu se předpokládá, že již máte aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="c9a46-106">The code examples in this topic assume you already have a Node.js application.</span></span> <span data-ttu-id="c9a46-107">Informace o tom, jak vytvořit aplikaci Node.js v Azure najdete v tématu některé z těchto témat:</span><span class="sxs-lookup"><span data-stu-id="c9a46-107">For information about how to create a Node.js application in Azure, see any of these topics:</span></span>

* [<span data-ttu-id="c9a46-108">Vytvoření webové aplikace Node.js ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c9a46-108">Create a Node.js web app in Azure App Service</span></span>](../app-service-web/app-service-web-get-started-nodejs.md)
* [<span data-ttu-id="c9a46-109">Sestavení a nasazení webové aplikace Node.js do Azure pomocí služby WebMatrix</span><span class="sxs-lookup"><span data-stu-id="c9a46-109">Build and deploy a Node.js web app to Azure using WebMatrix</span></span>](../app-service-web/web-sites-nodejs-use-webmatrix.md)
* <span data-ttu-id="c9a46-110">[Sestavení a nasazení aplikace Node.js ve službě Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (pomocí prostředí Windows PowerShell)</span><span class="sxs-lookup"><span data-stu-id="c9a46-110">[Build and deploy a Node.js application to an Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (using Windows PowerShell)</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="configure-your-application-to-access-azure-storage"></a><span data-ttu-id="c9a46-111">Konfigurace aplikace pro přístup k úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="c9a46-111">Configure your application to access Azure Storage</span></span>
<span data-ttu-id="c9a46-112">Používání Azure Storage, musíte sadu Azure SDK úložiště pro Node.js, která obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště.</span><span class="sxs-lookup"><span data-stu-id="c9a46-112">To use Azure Storage, you need the Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with the storage REST services.</span></span>

### <a name="use-node-package-manager-npm-to-install-the-package"></a><span data-ttu-id="c9a46-113">Uzel balíčku správce (NPM) použijte k instalaci balíčku</span><span class="sxs-lookup"><span data-stu-id="c9a46-113">Use Node Package Manager (NPM) to install the package</span></span>
1. <span data-ttu-id="c9a46-114">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix) a přejděte do složky, které jste vytvořili vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c9a46-114">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix), and navigate to the folder where you created your application.</span></span>
2. <span data-ttu-id="c9a46-115">Typ **npm nainstalujte azure-storage** v příkazovém okně.</span><span class="sxs-lookup"><span data-stu-id="c9a46-115">Type **npm install azure-storage** in the command window.</span></span> <span data-ttu-id="c9a46-116">Výstup z tohoto příkazu se podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="c9a46-116">Output from the command is similar to the following example.</span></span>

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
3. <span data-ttu-id="c9a46-117">Můžete ručně spustit **ls** příkazu ověřte, že **uzlu\_moduly** složka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="c9a46-117">You can manually run the **ls** command to verify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="c9a46-118">Uvnitř této složky najdete **azure-storage** balíček, který obsahuje knihovny, je třeba získat přístup k úložišti.</span><span class="sxs-lookup"><span data-stu-id="c9a46-118">Inside that folder you will find the **azure-storage** package, which contains the libraries you need to access storage.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="c9a46-119">Import balíčku</span><span class="sxs-lookup"><span data-stu-id="c9a46-119">Import the package</span></span>
<span data-ttu-id="c9a46-120">Přidejte následující kód do horní části **server.js** souborů ve vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="c9a46-120">Add the following code to the top of the **server.js** file in your application:</span></span>

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="c9a46-121">Nastavit připojení k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c9a46-121">Set up an Azure Storage connection</span></span>
<span data-ttu-id="c9a46-122">Modul azure, bude číst proměnné prostředí AZURE\_úložiště\_účet a AZURE\_úložiště\_přístup\_klíč nebo AZURE\_úložiště\_připojení\_řetězec informace požadované pro připojení k účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c9a46-122">The azure module will read the environment variables AZURE\_STORAGE\_ACCOUNT and AZURE\_STORAGE\_ACCESS\_KEY, or AZURE\_STORAGE\_CONNECTION\_STRING for information required to connect to your Azure storage account.</span></span> <span data-ttu-id="c9a46-123">Pokud nejsou nastavené těchto proměnných prostředí, musíte zadat informace o účtu při volání metody **TableService**.</span><span class="sxs-lookup"><span data-stu-id="c9a46-123">If these environment variables are not set, you must specify the account information when calling **TableService**.</span></span>

<span data-ttu-id="c9a46-124">Příklad nastavení proměnných prostředí [portál Azure](https://portal.azure.com) web Azure, najdete v části [webové aplikace Node.js pomocí služby Azure Table](../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span><span class="sxs-lookup"><span data-stu-id="c9a46-124">For an example of setting the environment variables in the [Azure portal](https://portal.azure.com) for an Azure Website, see [Node.js web app using the Azure Table Service](../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span></span>

## <a name="create-a-table"></a><span data-ttu-id="c9a46-125">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="c9a46-125">Create a table</span></span>
<span data-ttu-id="c9a46-126">Následující kód vytvoří **TableService** objektu a pomocí něj vytvořit novou tabulku.</span><span class="sxs-lookup"><span data-stu-id="c9a46-126">The following code creates a **TableService** object and uses it to create a new table.</span></span> <span data-ttu-id="c9a46-127">Přidejte následující v horní části **server.js**.</span><span class="sxs-lookup"><span data-stu-id="c9a46-127">Add the following near the top of **server.js**.</span></span>

```nodejs
var tableSvc = azure.createTableService();
```

<span data-ttu-id="c9a46-128">Volání **createTableIfNotExists** vytvoří novou tabulku se zadaným názvem, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c9a46-128">The call to **createTableIfNotExists** will create a new table with the specified name if it does not already exist.</span></span> <span data-ttu-id="c9a46-129">Následující příklad vytvoří novou tabulku s názvem "mytable", pokud ještě neexistuje:</span><span class="sxs-lookup"><span data-stu-id="c9a46-129">The following example creates a new table named 'mytable' if it does not already exist:</span></span>

```nodejs
tableSvc.createTableIfNotExists('mytable', function(error, result, response){
  if(!error){
    // Table exists or created
  }
});
```

<span data-ttu-id="c9a46-130">`result.created` Bude `true` Pokud se vytvoří nové tabulky, a `false` Pokud tabulka již existuje.</span><span class="sxs-lookup"><span data-stu-id="c9a46-130">The `result.created` will be `true` if a new table is created, and `false` if the table already exists.</span></span> <span data-ttu-id="c9a46-131">`response` Bude obsahovat informace o požadavku.</span><span class="sxs-lookup"><span data-stu-id="c9a46-131">The `response` will contain information about the request.</span></span>

### <a name="filters"></a><span data-ttu-id="c9a46-132">Filtry</span><span class="sxs-lookup"><span data-stu-id="c9a46-132">Filters</span></span>
<span data-ttu-id="c9a46-133">Volitelné filtrování operace lze použít pro operace provedené pomocí **TableService**.</span><span class="sxs-lookup"><span data-stu-id="c9a46-133">Optional filtering operations can be applied to operations performed using **TableService**.</span></span> <span data-ttu-id="c9a46-134">Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu s podpisem:</span><span class="sxs-lookup"><span data-stu-id="c9a46-134">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with the signature:</span></span>

```nodejs
function handle (requestOptions, next)
```

<span data-ttu-id="c9a46-135">Až to předzpracování na žádost o možnostech, metoda potřebuje volat tlačítko Další"předání zpětné volání podpisem následující:</span><span class="sxs-lookup"><span data-stu-id="c9a46-135">After doing its preprocessing on the request options, the method needs to call "next", passing a callback with the following signature:</span></span>

```nodejs
function (returnObject, finalCallback, next)
```

<span data-ttu-id="c9a46-136">V této zpětného volání a po zpracování returnObject (odpovědi požadavek na server) zpětné volání je potřeba buď vyvolat další, pokud existuje pokračovat ve zpracovávání ostatní filtry nebo jednoduše vyvolat finalCallback jinak k ukončení volání služby.</span><span class="sxs-lookup"><span data-stu-id="c9a46-136">In this callback, and after processing the returnObject (the response from the request to the server), the callback needs to either invoke next if it exists to continue processing other filters or simply invoke finalCallback otherwise to end the service invocation.</span></span>

<span data-ttu-id="c9a46-137">Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí sady Azure SDK pro Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="c9a46-137">Two filters that implement retry logic are included with the Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="c9a46-138">Vytvoří následující **TableService** objekt, který používá **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="c9a46-138">The following creates a **TableService** object that uses the **ExponentialRetryPolicyFilter**:</span></span>

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var tableSvc = azure.createTableService().withFilter(retryOperations);
```

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="c9a46-139">Přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="c9a46-139">Add an entity to a table</span></span>
<span data-ttu-id="c9a46-140">Chcete-li přidat entitu, nejprve vytvořte objekt, který definuje vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="c9a46-140">To add an entity, first create an object that defines your entity properties.</span></span> <span data-ttu-id="c9a46-141">Musí obsahovat všechny entity **PartitionKey** a **RowKey**, které jsou jedinečné identifikátory pro entitu.</span><span class="sxs-lookup"><span data-stu-id="c9a46-141">All entities must contain a **PartitionKey** and **RowKey**, which are unique identifiers for the entity.</span></span>

* <span data-ttu-id="c9a46-142">**PartitionKey** -určuje uložených entity v oddílu</span><span class="sxs-lookup"><span data-stu-id="c9a46-142">**PartitionKey** - determines the partition that the entity is stored in</span></span>
* <span data-ttu-id="c9a46-143">**RowKey** – jednoznačně identifikuje entity v oddílu</span><span class="sxs-lookup"><span data-stu-id="c9a46-143">**RowKey** - uniquely identifies the entity within the partition</span></span>

<span data-ttu-id="c9a46-144">Obě **PartitionKey** a **RowKey** musí být řetězcové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c9a46-144">Both **PartitionKey** and **RowKey** must be string values.</span></span> <span data-ttu-id="c9a46-145">Další informace najdete v tématu [Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="c9a46-145">For more information, see [Understanding the Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

<span data-ttu-id="c9a46-146">Následuje příklad definování entity.</span><span class="sxs-lookup"><span data-stu-id="c9a46-146">The following is an example of defining an entity.</span></span> <span data-ttu-id="c9a46-147">Všimněte si, že **dueDate** je definována jako typ **Edm.DateTime**.</span><span class="sxs-lookup"><span data-stu-id="c9a46-147">Note that **dueDate** is defined as a type of **Edm.DateTime**.</span></span> <span data-ttu-id="c9a46-148">Určení typu je volitelný a typy bude možné odvodit, pokud není zadán.</span><span class="sxs-lookup"><span data-stu-id="c9a46-148">Specifying the type is optional, and types will be inferred if not specified.</span></span>

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
};
```

> [!NOTE]
> <span data-ttu-id="c9a46-149">K dispozici je také **časové razítko** pole pro každý záznam, který je nastavena jako Azure po vložení nebo aktualizaci entity.</span><span class="sxs-lookup"><span data-stu-id="c9a46-149">There is also a **Timestamp** field for each record, which is set by Azure when an entity is inserted or updated.</span></span>
>
>

<span data-ttu-id="c9a46-150">Můžete také **entityGenerator** vytváření entit.</span><span class="sxs-lookup"><span data-stu-id="c9a46-150">You can also use the **entityGenerator** to create entities.</span></span> <span data-ttu-id="c9a46-151">Následující příklad vytvoří stejné entity úloh pomocí **entityGenerator**.</span><span class="sxs-lookup"><span data-stu-id="c9a46-151">The following example creates the same task entity using the **entityGenerator**.</span></span>

```nodejs
var entGen = azure.TableUtilities.entityGenerator;
var task = {
  PartitionKey: entGen.String('hometasks'),
  RowKey: entGen.String('1'),
  description: entGen.String('take out the trash'),
  dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
};
```

<span data-ttu-id="c9a46-152">Chcete-li do tabulky přidat entitu, předejte objekt entity, který má **insertEntity** metoda.</span><span class="sxs-lookup"><span data-stu-id="c9a46-152">To add an entity to your table, pass the entity object to the **insertEntity** method.</span></span>

```nodejs
tableSvc.insertEntity('mytable',task, function (error, result, response) {
  if(!error){
    // Entity inserted
  }
});
```

<span data-ttu-id="c9a46-153">Pokud byla operace úspěšná, `result` bude obsahovat [značka ETag](http://en.wikipedia.org/wiki/HTTP_ETag) vložené záznamu a `response` bude obsahovat informace o operaci.</span><span class="sxs-lookup"><span data-stu-id="c9a46-153">If the operation is successful, `result` will contain the [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) of the inserted record and `response` will contain information about the operation.</span></span>

<span data-ttu-id="c9a46-154">Příklad odpovědi:</span><span class="sxs-lookup"><span data-stu-id="c9a46-154">Example response:</span></span>

```nodejs
{ '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }
```

> [!NOTE]
> <span data-ttu-id="c9a46-155">Ve výchozím nastavení **insertEntity** nevrací vložené entity jako součást `response` informace.</span><span class="sxs-lookup"><span data-stu-id="c9a46-155">By default, **insertEntity** does not return the inserted entity as part of the `response` information.</span></span> <span data-ttu-id="c9a46-156">Pokud plánujete provádí jiné operace na tuto entitu, nebo chcete pro ukládání do mezipaměti informace, může být užitečné jej vraceny jako součást `result`.</span><span class="sxs-lookup"><span data-stu-id="c9a46-156">If you plan on performing other operations on this entity, or wish to cache the information, it can be useful to have it returned as part of the `result`.</span></span> <span data-ttu-id="c9a46-157">Můžete k tomu povolením **echoContent** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c9a46-157">You can do this by enabling **echoContent** as follows:</span></span>
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`
>
>

## <a name="update-an-entity"></a><span data-ttu-id="c9a46-158">Aktualizace entity</span><span class="sxs-lookup"><span data-stu-id="c9a46-158">Update an entity</span></span>
<span data-ttu-id="c9a46-159">Existuje několik metod, které jsou k dispozici pro aktualizace stávající entity:</span><span class="sxs-lookup"><span data-stu-id="c9a46-159">There are multiple methods available to update an existing entity:</span></span>

* <span data-ttu-id="c9a46-160">**replaceEntity** -aktualizace stávající entity podle jeho nahrazení</span><span class="sxs-lookup"><span data-stu-id="c9a46-160">**replaceEntity** - updates an existing entity by replacing it</span></span>
* <span data-ttu-id="c9a46-161">**mergeEntity** -aktualizace stávající entity sloučením nové hodnoty vlastností do stávající entity</span><span class="sxs-lookup"><span data-stu-id="c9a46-161">**mergeEntity** - updates an existing entity by merging new property values into the existing entity</span></span>
* <span data-ttu-id="c9a46-162">**insertOrReplaceEntity** -aktualizace stávající entity podle jeho nahrazení.</span><span class="sxs-lookup"><span data-stu-id="c9a46-162">**insertOrReplaceEntity** - updates an existing entity by replacing it.</span></span> <span data-ttu-id="c9a46-163">Pokud žádná entita existuje, bude možné vložit nový</span><span class="sxs-lookup"><span data-stu-id="c9a46-163">If no entity exists, a new one will be inserted</span></span>
* <span data-ttu-id="c9a46-164">**insertOrMergeEntity** -aktualizace stávající entity sloučením nové hodnoty vlastností do stávající.</span><span class="sxs-lookup"><span data-stu-id="c9a46-164">**insertOrMergeEntity** - updates an existing entity by merging new property values into the existing.</span></span> <span data-ttu-id="c9a46-165">Pokud žádná entita existuje, bude možné vložit nový</span><span class="sxs-lookup"><span data-stu-id="c9a46-165">If no entity exists, a new one will be inserted</span></span>

<span data-ttu-id="c9a46-166">Následující příklad ukazuje, aktualizuje entitu s využitím **replaceEntity**:</span><span class="sxs-lookup"><span data-stu-id="c9a46-166">The following example demonstrates updating an entity using **replaceEntity**:</span></span>

```nodejs
tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
  if(!error) {
    // Entity updated
  }
});
```

> [!NOTE]
> <span data-ttu-id="c9a46-167">Ve výchozím nastavení aktualizaci entity nekontroluje zobrazíte, když data aktualizované dříve byla změněna jiným procesem.</span><span class="sxs-lookup"><span data-stu-id="c9a46-167">By default, updating an entity does not check to see if the data being updated has previously been modified by another process.</span></span> <span data-ttu-id="c9a46-168">Podpora souběžných aktualizace:</span><span class="sxs-lookup"><span data-stu-id="c9a46-168">To support concurrent updates:</span></span>
>
> 1. <span data-ttu-id="c9a46-169">Získá značku ETag objekt je aktualizován.</span><span class="sxs-lookup"><span data-stu-id="c9a46-169">Get the ETag of the object being updated.</span></span> <span data-ttu-id="c9a46-170">K této chybě dochází v rámci `response` pro všechny operace související entity a mohou být načteny prostřednictvím `response['.metadata'].etag`.</span><span class="sxs-lookup"><span data-stu-id="c9a46-170">This is returned as part of the `response` for any entity-related operation and can be retrieved through `response['.metadata'].etag`.</span></span>
> 2. <span data-ttu-id="c9a46-171">Při provádění operace aktualizace na entitu, přidání značka ETag informace dříve načtené do nové entity.</span><span class="sxs-lookup"><span data-stu-id="c9a46-171">When performing an update operation on an entity, add the ETag information previously retrieved to the new entity.</span></span> <span data-ttu-id="c9a46-172">Například:</span><span class="sxs-lookup"><span data-stu-id="c9a46-172">For example:</span></span>
>
>       <span data-ttu-id="c9a46-173">entity2 [.metadata] .etag = currentEtag;</span><span class="sxs-lookup"><span data-stu-id="c9a46-173">entity2['.metadata'].etag = currentEtag;</span></span>
> 3. <span data-ttu-id="c9a46-174">Proveďte operaci aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c9a46-174">Perform the update operation.</span></span> <span data-ttu-id="c9a46-175">Pokud byla entita od načíst hodnotu značka ETag, jako je například jiná instance aplikace, změněna `error` bude vrácen s informacemi o tom, zda je aktualizace podmínka uvedená v žádosti nebyla splněná.</span><span class="sxs-lookup"><span data-stu-id="c9a46-175">If the entity has been modified since you retrieved the ETag value, such as another instance of your application, an `error` will be returned stating that the update condition specified in the request was not satisfied.</span></span>
>
>

<span data-ttu-id="c9a46-176">S **replaceEntity** a **mergeEntity**, pokud typ entity, která se právě aktualizuje neexistuje, pak operace aktualizace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="c9a46-176">With **replaceEntity** and **mergeEntity**, if the entity that is being updated doesn't exist, then the update operation will fail.</span></span> <span data-ttu-id="c9a46-177">Proto pokud chcete uložit entity bez ohledu na tom, zda již existuje, použijte **insertOrReplaceEntity** nebo **insertOrMergeEntity**.</span><span class="sxs-lookup"><span data-stu-id="c9a46-177">Therefore if you wish to store an entity regardless of whether it already exists, use **insertOrReplaceEntity** or **insertOrMergeEntity**.</span></span>

<span data-ttu-id="c9a46-178">`result` Operace úspěšná aktualizace bude obsahovat **Značka Etag** aktualizované entity.</span><span class="sxs-lookup"><span data-stu-id="c9a46-178">The `result` for successful update operations will contain the **Etag** of the updated entity.</span></span>

## <a name="work-with-groups-of-entities"></a><span data-ttu-id="c9a46-179">Práce se skupinami entit</span><span class="sxs-lookup"><span data-stu-id="c9a46-179">Work with groups of entities</span></span>
<span data-ttu-id="c9a46-180">Někdy má smysl odeslat více operací společně v dávce zajistit atomic zpracování serverem.</span><span class="sxs-lookup"><span data-stu-id="c9a46-180">Sometimes it makes sense to submit multiple operations together in a batch to ensure atomic processing by the server.</span></span> <span data-ttu-id="c9a46-181">Chcete-li provést tuto akci, použijte **TableBatch** třídy pro vytvoření dávky a pak použijte **executeBatch** metodu **TableService** provádět dávkové operace.</span><span class="sxs-lookup"><span data-stu-id="c9a46-181">To accomplish that, use the **TableBatch** class to create a batch, and then use the **executeBatch** method of **TableService** to perform the batched operations.</span></span>

 <span data-ttu-id="c9a46-182">Následující příklad ukazuje, odesílání dvě entity v dávce:</span><span class="sxs-lookup"><span data-stu-id="c9a46-182">The following example demonstrates submitting two entities in a batch:</span></span>

```nodejs
var task1 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'Take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20)}
};
var task2 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '2'},
  description: {'_':'Wash the dishes'},
  dueDate: {'_':new Date(2015, 6, 20)}
};

var batch = new azure.TableBatch();

batch.insertEntity(task1, {echoContent: true});
batch.insertEntity(task2, {echoContent: true});

tableSvc.executeBatch('mytable', batch, function (error, result, response) {
  if(!error) {
    // Batch completed
  }
});
```

<span data-ttu-id="c9a46-183">Pro úspěšné dávkových operací `result` bude obsahovat informace o každé operace v dávce.</span><span class="sxs-lookup"><span data-stu-id="c9a46-183">For successful batch operations, `result` will contain information for each operation in the batch.</span></span>

### <a name="work-with-batched-operations"></a><span data-ttu-id="c9a46-184">Práce s dávkové operace</span><span class="sxs-lookup"><span data-stu-id="c9a46-184">Work with batched operations</span></span>
<span data-ttu-id="c9a46-185">Přidat do dávky operace může být prověřovány zobrazením `operations` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c9a46-185">Operations added to a batch can be inspected by viewing the `operations` property.</span></span> <span data-ttu-id="c9a46-186">Můžete taky následující metody pro práci s operací:</span><span class="sxs-lookup"><span data-stu-id="c9a46-186">You can also use the following methods to work with operations:</span></span>

* <span data-ttu-id="c9a46-187">**Vymazat** -vymaže všechny operace z dávky</span><span class="sxs-lookup"><span data-stu-id="c9a46-187">**clear** - clears all operations from a batch</span></span>
* <span data-ttu-id="c9a46-188">**getOperations** -získá operace z dávky</span><span class="sxs-lookup"><span data-stu-id="c9a46-188">**getOperations** - gets an operation from the batch</span></span>
* <span data-ttu-id="c9a46-189">**hasOperations** -vrátí hodnotu true, pokud dávka obsahuje operace</span><span class="sxs-lookup"><span data-stu-id="c9a46-189">**hasOperations** - returns true if the batch contains operations</span></span>
* <span data-ttu-id="c9a46-190">**removeOperations** – odebere operace</span><span class="sxs-lookup"><span data-stu-id="c9a46-190">**removeOperations** - removes an operation</span></span>
* <span data-ttu-id="c9a46-191">**velikost** -vrátí počet operací v dávce</span><span class="sxs-lookup"><span data-stu-id="c9a46-191">**size** - returns the number of operations in the batch</span></span>

## <a name="retrieve-an-entity-by-key"></a><span data-ttu-id="c9a46-192">Načtení entity pomocí klíče</span><span class="sxs-lookup"><span data-stu-id="c9a46-192">Retrieve an entity by key</span></span>
<span data-ttu-id="c9a46-193">Vrátit konkrétní entitu na základě **PartitionKey** a **RowKey**, použijte **retrieveEntity** metoda.</span><span class="sxs-lookup"><span data-stu-id="c9a46-193">To return a specific entity based on the **PartitionKey** and **RowKey**, use the **retrieveEntity** method.</span></span>

```nodejs
tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
  if(!error){
    // result contains the entity
  }
});
```

<span data-ttu-id="c9a46-194">Po dokončení této operace `result` bude obsahovat entity.</span><span class="sxs-lookup"><span data-stu-id="c9a46-194">Once this operation is complete, `result` will contain the entity.</span></span>

## <a name="query-a-set-of-entities"></a><span data-ttu-id="c9a46-195">Dotaz na sadu entit</span><span class="sxs-lookup"><span data-stu-id="c9a46-195">Query a set of entities</span></span>
<span data-ttu-id="c9a46-196">Dotaz na tabulku, použijte **TableQuery** objekt vybudovat výrazu dotazu pomocí klauzule následující:</span><span class="sxs-lookup"><span data-stu-id="c9a46-196">To query a table, use the **TableQuery** object to build up a query expression using the following clauses:</span></span>

* <span data-ttu-id="c9a46-197">**Vyberte** -polí, která mají být vráceny z dotazu</span><span class="sxs-lookup"><span data-stu-id="c9a46-197">**select** - the fields to be returned from the query</span></span>
* <span data-ttu-id="c9a46-198">**kde** -where – klauzule</span><span class="sxs-lookup"><span data-stu-id="c9a46-198">**where** - the where clause</span></span>

  * <span data-ttu-id="c9a46-199">**a** – `and` kde podmínky</span><span class="sxs-lookup"><span data-stu-id="c9a46-199">**and** - an `and` where condition</span></span>
  * <span data-ttu-id="c9a46-200">**nebo** – `or` kde podmínky</span><span class="sxs-lookup"><span data-stu-id="c9a46-200">**or** - an `or` where condition</span></span>
* <span data-ttu-id="c9a46-201">**horní** -počet položek k načtení</span><span class="sxs-lookup"><span data-stu-id="c9a46-201">**top** - the number of items to fetch</span></span>

<span data-ttu-id="c9a46-202">Následující příklad vytvoří dotaz, který vrátí nejvyšší pět položek s PartitionKey 'hometasks'.</span><span class="sxs-lookup"><span data-stu-id="c9a46-202">The following example builds a query that will return the top five items with a PartitionKey of 'hometasks'.</span></span>

```nodejs
var query = new azure.TableQuery()
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

<span data-ttu-id="c9a46-203">Vzhledem k tomu **vyberte** se nepoužívá, bude vrácen všechna pole.</span><span class="sxs-lookup"><span data-stu-id="c9a46-203">Since **select** is not used, all fields will be returned.</span></span> <span data-ttu-id="c9a46-204">Pokud chcete provést dotaz na tabulku, použijte **queryEntities**.</span><span class="sxs-lookup"><span data-stu-id="c9a46-204">To perform the query against a table, use **queryEntities**.</span></span> <span data-ttu-id="c9a46-205">Následující příklad používá tento dotaz se vrátí entity ze "mytable".</span><span class="sxs-lookup"><span data-stu-id="c9a46-205">The following example uses this query to return entities from 'mytable'.</span></span>

```nodejs
tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
  if(!error) {
    // query was successful
  }
});
```

<span data-ttu-id="c9a46-206">V případě úspěšného `result.entries` bude obsahovat pole entit, které odpovídají dotazu.</span><span class="sxs-lookup"><span data-stu-id="c9a46-206">If successful, `result.entries` will contain an array of entities that match the query.</span></span> <span data-ttu-id="c9a46-207">Pokud dotaz se nepodařilo vrátit všechny entity `result.continuationToken` bude jinou hodnotu než*null* a mohou být použity jako třetí parametr funkce **queryEntities** k načtení více výsledků.</span><span class="sxs-lookup"><span data-stu-id="c9a46-207">If the query was unable to return all entities, `result.continuationToken` will be non-*null* and can be used as the third parameter of **queryEntities** to retrieve more results.</span></span> <span data-ttu-id="c9a46-208">Pro počáteční dotaz, použít *null* pro třetí parametr.</span><span class="sxs-lookup"><span data-stu-id="c9a46-208">For the initial query, use *null* for the third parameter.</span></span>

### <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="c9a46-209">Dotaz na podmnožinu vlastností entity</span><span class="sxs-lookup"><span data-stu-id="c9a46-209">Query a subset of entity properties</span></span>
<span data-ttu-id="c9a46-210">Dotaz na tabulku může načíst několika pole z entity.</span><span class="sxs-lookup"><span data-stu-id="c9a46-210">A query to a table can retrieve just a few fields from an entity.</span></span>
<span data-ttu-id="c9a46-211">To zmenšuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entity.</span><span class="sxs-lookup"><span data-stu-id="c9a46-211">This reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="c9a46-212">Použití **vyberte** klauzule a předejte názvy polí, která má být vrácen.</span><span class="sxs-lookup"><span data-stu-id="c9a46-212">Use the **select** clause and pass the names of the fields to be returned.</span></span> <span data-ttu-id="c9a46-213">Například následující dotaz vrátí jenom **popis** a **dueDate** pole.</span><span class="sxs-lookup"><span data-stu-id="c9a46-213">For example, the following query will return only the **description** and **dueDate** fields.</span></span>

```nodejs
var query = new azure.TableQuery()
  .select(['description', 'dueDate'])
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

## <a name="delete-an-entity"></a><span data-ttu-id="c9a46-214">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="c9a46-214">Delete an entity</span></span>
<span data-ttu-id="c9a46-215">Můžete odstranit pomocí jeho klíče oddílu a řádku entity.</span><span class="sxs-lookup"><span data-stu-id="c9a46-215">You can delete an entity using its partition and row keys.</span></span> <span data-ttu-id="c9a46-216">V tomto příkladu **task1** objekt obsahuje **RowKey** a **PartitionKey** hodnoty entity, která má být odstraněna.</span><span class="sxs-lookup"><span data-stu-id="c9a46-216">In this example, the **task1** object contains the **RowKey** and **PartitionKey** values of the entity to be deleted.</span></span> <span data-ttu-id="c9a46-217">Pak je objekt předaný **deleteEntity** metoda.</span><span class="sxs-lookup"><span data-stu-id="c9a46-217">Then the object is passed to the **deleteEntity** method.</span></span>

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'}
};

tableSvc.deleteEntity('mytable', task, function(error, response){
  if(!error) {
    // Entity deleted
  }
});
```

> [!NOTE]
> <span data-ttu-id="c9a46-218">Zvažte použití značky etag binárním rozsáhlým při odstraňování položek, zajistit, že položka nebyl změněn jiným procesem.</span><span class="sxs-lookup"><span data-stu-id="c9a46-218">Consider using ETags when deleting items, to ensure that the item hasn't been modified by another process.</span></span> <span data-ttu-id="c9a46-219">V tématu [aktualizovat entitu,](#update-an-entity) informace o použití značky etag binárním rozsáhlým.</span><span class="sxs-lookup"><span data-stu-id="c9a46-219">See [Update an entity](#update-an-entity) for information on using ETags.</span></span>
>
>

## <a name="delete-a-table"></a><span data-ttu-id="c9a46-220">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="c9a46-220">Delete a table</span></span>
<span data-ttu-id="c9a46-221">Následující kód odstraní tabulku z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c9a46-221">The following code deletes a table from a storage account.</span></span>

```nodejs
tableSvc.deleteTable('mytable', function(error, response){
    if(!error){
        // Table deleted
    }
});
```

<span data-ttu-id="c9a46-222">Pokud si nejste jisti, zda tabulka existuje, použijte **deleteTableIfExists**.</span><span class="sxs-lookup"><span data-stu-id="c9a46-222">If you are uncertain whether the table exists, use **deleteTableIfExists**.</span></span>

## <a name="use-continuation-tokens"></a><span data-ttu-id="c9a46-223">Použít pokračování tokeny</span><span class="sxs-lookup"><span data-stu-id="c9a46-223">Use continuation tokens</span></span>
<span data-ttu-id="c9a46-224">Když dotazujete tabulky pro velké objemy výsledky, vyhledejte pokračování tokeny.</span><span class="sxs-lookup"><span data-stu-id="c9a46-224">When you are querying tables for large amounts of results, look for continuation tokens.</span></span> <span data-ttu-id="c9a46-225">K dispozici pro svůj dotaz, který je nemusíte být vědomi toho, pokud není sestavení poznáte při token pokračování je k dispozici může být velké objemy dat.</span><span class="sxs-lookup"><span data-stu-id="c9a46-225">There may be large amounts of data available for your query that you might not realize if you do not build to recognize when a continuation token is present.</span></span>

<span data-ttu-id="c9a46-226">Výsledky objektu vrátil během dotazování sady entit `continuationToken` vlastnost, pokud takový token je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c9a46-226">The results object returned during querying entities sets a `continuationToken` property when such a token is present.</span></span> <span data-ttu-id="c9a46-227">Pak můžete toto při provádění dotazu nadále přesouvat mezi oddílů a tabulka entity.</span><span class="sxs-lookup"><span data-stu-id="c9a46-227">You can then use this when performing a query to continue to move across the partition and table entities.</span></span>

<span data-ttu-id="c9a46-228">Při dotazování, může být zadán parametr continuationToken mezi instance objektu dotazu a funkce zpětného volání:</span><span class="sxs-lookup"><span data-stu-id="c9a46-228">When querying, a continuationToken parameter may be provided between the query object instance and the callback function:</span></span>

```nodejs
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

<span data-ttu-id="c9a46-229">Je-li si prohlédnout `continuationToken` objekt, zjistíte, vlastnosti, jako `nextPartitionKey`, `nextRowKey` a `targetLocation`, které lze použít k iteraci v rámci všechny výsledky.</span><span class="sxs-lookup"><span data-stu-id="c9a46-229">If you inspect the `continuationToken` object, you will find properties such as `nextPartitionKey`, `nextRowKey` and `targetLocation`, which can be used to iterate through all the results.</span></span>

<span data-ttu-id="c9a46-230">Je také ukázku pokračování v úložišti Azure Storage Node.js na Githubu.</span><span class="sxs-lookup"><span data-stu-id="c9a46-230">There is also a continuation sample within the Azure Storage Node.js repo on GitHub.</span></span> <span data-ttu-id="c9a46-231">Vyhledejte `examples/samples/continuationsample.js`.</span><span class="sxs-lookup"><span data-stu-id="c9a46-231">Look for `examples/samples/continuationsample.js`.</span></span>

## <a name="work-with-shared-access-signatures"></a><span data-ttu-id="c9a46-232">Práce s podpisy sdíleného přístupu</span><span class="sxs-lookup"><span data-stu-id="c9a46-232">Work with shared access signatures</span></span>
<span data-ttu-id="c9a46-233">Sdílené přístupové podpisy (SAS) jsou zabezpečené způsob, jak poskytnout podrobné přístup k tabulkám bez zadání názvu účtu úložiště nebo klíče.</span><span class="sxs-lookup"><span data-stu-id="c9a46-233">Shared access signatures (SAS) are a secure way to provide granular access to tables without providing your storage account name or keys.</span></span> <span data-ttu-id="c9a46-234">SAS se často používá k poskytování omezený přístup k datům, například povolení mobilní aplikace vyhledejte záznamy.</span><span class="sxs-lookup"><span data-stu-id="c9a46-234">SAS are often used to provide limited access to your data, such as allowing a mobile app to query records.</span></span>

<span data-ttu-id="c9a46-235">Důvěryhodné aplikace, jako je Cloudová služba vygeneruje SAS pomocí **generateSharedAccessSignature** z **TableService**a poskytuje ji jako nedůvěryhodný nebo částečně důvěryhodné aplikace mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="c9a46-235">A trusted application such as a cloud-based service generates a SAS using the **generateSharedAccessSignature** of the **TableService**, and provides it to an untrusted or semi-trusted application such as a mobile app.</span></span> <span data-ttu-id="c9a46-236">SAS je generována pomocí zásad, která popisuje počátečním a koncovým datem, během které SAS je platný, a také úroveň přístupu k majiteli SAS.</span><span class="sxs-lookup"><span data-stu-id="c9a46-236">The SAS is generated using a policy, which describes the start and end dates during which the SAS is valid, as well as the access level granted to the SAS holder.</span></span>

<span data-ttu-id="c9a46-237">Následující příklad vytvoří nové zásady sdíleného přístupu, který vám umožní držitele SAS pro dotaz ('r') v tabulce a vyprší platnost 100 minut od okamžiku, kdy je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="c9a46-237">The following example generates a new shared access policy that will allow the SAS holder to query ('r') the table, and expires 100 minutes after the time it is created.</span></span>

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
};

var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
var host = tableSvc.host;
```

<span data-ttu-id="c9a46-238">Všimněte si, že informace o hostiteli je třeba zadat také, jako je povinný, když se držitele SAS pokusí o přístup k tabulce.</span><span class="sxs-lookup"><span data-stu-id="c9a46-238">Note that the host information must be provided also, as it is required when the SAS holder attempts to access the table.</span></span>

<span data-ttu-id="c9a46-239">Klientská aplikace pak používá SAS s **TableServiceWithSAS** k provádění operací s tabulkou.</span><span class="sxs-lookup"><span data-stu-id="c9a46-239">The client application then uses the SAS with **TableServiceWithSAS** to perform operations against the table.</span></span> <span data-ttu-id="c9a46-240">Následující příklad se připojí k tabulce a provádí dotazu.</span><span class="sxs-lookup"><span data-stu-id="c9a46-240">The following example connects to the table and performs a query.</span></span>

```nodejs
var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
var query = azure.TableQuery()
  .where('PartitionKey eq ?', 'hometasks');

sharedTableService.queryEntities(query, null, function(error, result, response) {
  if(!error) {
    // result contains the entities
  }
});
```

<span data-ttu-id="c9a46-241">Vzhledem k tomu, že SAS se vygeneroval s dotazu přístup jenom, pokud byly proveden pokus o vložení, aktualizaci nebo odstranění entity, bude vrácena chyba.</span><span class="sxs-lookup"><span data-stu-id="c9a46-241">Since the SAS was generated with only query access, if an attempt were made to insert, update, or delete entities, an error would be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="c9a46-242">Seznamy řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="c9a46-242">Access Control Lists</span></span>
<span data-ttu-id="c9a46-243">Seznam řízení přístupu (ACL) můžete také nastavit zásady přístupu pro SAS.</span><span class="sxs-lookup"><span data-stu-id="c9a46-243">You can also use an Access Control List (ACL) to set the access policy for a SAS.</span></span> <span data-ttu-id="c9a46-244">To je užitečné, pokud chcete povolit více klientům přístup k tabulce, ale poskytnutí zásad, jiný přístup pro každého klienta.</span><span class="sxs-lookup"><span data-stu-id="c9a46-244">This is useful if you wish to allow multiple clients to access the table, but provide different access policies for each client.</span></span>

<span data-ttu-id="c9a46-245">Seznam ACL je implementovaná pomocí pole zásady přístupu s ID spojené s každou zásadu.</span><span class="sxs-lookup"><span data-stu-id="c9a46-245">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="c9a46-246">V následujícím příkladu definuje dvě zásady, jeden pro "uživatel1" a jeden pro, uživatel2":</span><span class="sxs-lookup"><span data-stu-id="c9a46-246">The following example defines two policies, one for 'user1' and one for 'user2':</span></span>

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

<span data-ttu-id="c9a46-247">Následující příklad načte aktuální seznam ACL pro **hometasks** tabulky a potom se přidají nové zásady pomocí **setTableAcl**.</span><span class="sxs-lookup"><span data-stu-id="c9a46-247">The following example gets the current ACL for the **hometasks** table, and then adds the new policies using **setTableAcl**.</span></span> <span data-ttu-id="c9a46-248">Tento přístup umožňuje:</span><span class="sxs-lookup"><span data-stu-id="c9a46-248">This approach allows:</span></span>

```nodejs
var extend = require('extend');
tableSvc.getTableAcl('hometasks', function(error, result, response) {
if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

<span data-ttu-id="c9a46-249">Jakmile je nastavená seznamu ACL, potom můžete vytvořit SAS na základě ID pro zásadu.</span><span class="sxs-lookup"><span data-stu-id="c9a46-249">Once the ACL has been set, you can then create a SAS based on the ID for a policy.</span></span> <span data-ttu-id="c9a46-250">Následující příklad vytvoří nový SAS pro, uživatel2":</span><span class="sxs-lookup"><span data-stu-id="c9a46-250">The following example creates a new SAS for 'user2':</span></span>

```nodejs
tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="c9a46-251">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c9a46-251">Next steps</span></span>
<span data-ttu-id="c9a46-252">Další informace najdete v následujících zdrojích informací.</span><span class="sxs-lookup"><span data-stu-id="c9a46-252">For more information, see the following resources.</span></span>

* <span data-ttu-id="c9a46-253">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.</span><span class="sxs-lookup"><span data-stu-id="c9a46-253">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="c9a46-254">[Azure SDK úložiště pro uzel](https://github.com/Azure/azure-storage-node) úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="c9a46-254">[Azure Storage SDK for Node](https://github.com/Azure/azure-storage-node) repository on GitHub.</span></span>
* [<span data-ttu-id="c9a46-255">Středisku pro vývojáře Node.js</span><span class="sxs-lookup"><span data-stu-id="c9a46-255">Node.js Developer Center</span></span>](/develop/nodejs/)
* [<span data-ttu-id="c9a46-256">Vytvoření a nasazení aplikace Node.js ve službě Azure Web</span><span class="sxs-lookup"><span data-stu-id="c9a46-256">Create and deploy a Node.js application to an Azure website</span></span>](../app-service-web/app-service-web-get-started-nodejs.md)