---
title: "aaaHow toouse úložiště Azure Table z Node.js | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu hello pomocí Azure Table storage, úložiště dat typu NoSQL."
services: storage
documentationcenter: nodejs
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: fc2e33d2-c5da-4861-8503-53fdc25750de
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 990a71337b806d759d0277a7691712346db7b355
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-from-nodejs"></a><span data-ttu-id="acd32-103">Jak toouse úložiště Azure Table z Node.js</span><span class="sxs-lookup"><span data-stu-id="acd32-103">How toouse Azure Table storage from Node.js</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="acd32-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="acd32-104">Overview</span></span>
<span data-ttu-id="acd32-105">Toto téma ukazuje, jak služba tooperform běžné scénáře s využitím hello Azure Table v aplikaci Node.js.</span><span class="sxs-lookup"><span data-stu-id="acd32-105">This topic shows how tooperform common scenarios using hello Azure Table service in a Node.js application.</span></span>

<span data-ttu-id="acd32-106">Hello příklady kódu v tomto tématu se předpokládá, že již máte aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="acd32-106">hello code examples in this topic assume you already have a Node.js application.</span></span> <span data-ttu-id="acd32-107">Informace o toocreate aplikace Node.js v Azure, najdete v některé z těchto témat:</span><span class="sxs-lookup"><span data-stu-id="acd32-107">For information about how toocreate a Node.js application in Azure, see any of these topics:</span></span>

* [<span data-ttu-id="acd32-108">Vytvoření webové aplikace Node.js ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="acd32-108">Create a Node.js web app in Azure App Service</span></span>](../app-service-web/app-service-web-get-started-nodejs.md)
* [<span data-ttu-id="acd32-109">Vytváření a nasazování tooAzure Node.js webové aplikace pomocí služby WebMatrix</span><span class="sxs-lookup"><span data-stu-id="acd32-109">Build and deploy a Node.js web app tooAzure using WebMatrix</span></span>](../app-service-web/web-sites-nodejs-use-webmatrix.md)
* <span data-ttu-id="acd32-110">[Sestavení a nasazení tooan aplikace Node.js Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (pomocí prostředí Windows PowerShell)</span><span class="sxs-lookup"><span data-stu-id="acd32-110">[Build and deploy a Node.js application tooan Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (using Windows PowerShell)</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="configure-your-application-tooaccess-azure-storage"></a><span data-ttu-id="acd32-111">Konfigurace tooaccess vaše aplikace Azure Storage</span><span class="sxs-lookup"><span data-stu-id="acd32-111">Configure your application tooaccess Azure Storage</span></span>
<span data-ttu-id="acd32-112">toouse Azure Storage, musíte hello sada SDK úložiště Azure pro platformu Node.js, která obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="acd32-112">toouse Azure Storage, you need hello Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-node-package-manager-npm-tooinstall-hello-package"></a><span data-ttu-id="acd32-113">Použijte uzel balíček správce (NPM) tooinstall hello balíček</span><span class="sxs-lookup"><span data-stu-id="acd32-113">Use Node Package Manager (NPM) tooinstall hello package</span></span>
1. <span data-ttu-id="acd32-114">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix) a přejděte toohello složku, kde jste vytvořili vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="acd32-114">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix), and navigate toohello folder where you created your application.</span></span>
2. <span data-ttu-id="acd32-115">Typ **npm nainstalujte azure-storage** v příkazovém okně hello.</span><span class="sxs-lookup"><span data-stu-id="acd32-115">Type **npm install azure-storage** in hello command window.</span></span> <span data-ttu-id="acd32-116">Výstup z příkazu hello je podobné toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="acd32-116">Output from hello command is similar toohello following example.</span></span>

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
3. <span data-ttu-id="acd32-117">Můžete ručně spustit hello **ls** tooverify příkaz, **uzlu\_moduly** složka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="acd32-117">You can manually run hello **ls** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="acd32-118">V této složce budou najde hello **azure-storage** balíček, který obsahuje hello knihovny potřebujete tooaccess úložiště.</span><span class="sxs-lookup"><span data-stu-id="acd32-118">Inside that folder you will find hello **azure-storage** package, which contains hello libraries you need tooaccess storage.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="acd32-119">Importovat balíček hello</span><span class="sxs-lookup"><span data-stu-id="acd32-119">Import hello package</span></span>
<span data-ttu-id="acd32-120">Přidejte následující kód toohello začátek hello hello **server.js** souborů ve vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="acd32-120">Add hello following code toohello top of hello **server.js** file in your application:</span></span>

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="acd32-121">Nastavit připojení k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="acd32-121">Set up an Azure Storage connection</span></span>
<span data-ttu-id="acd32-122">modul Hello azure, bude číst hello proměnné prostředí AZURE\_úložiště\_účet a AZURE\_úložiště\_přístup\_klíč nebo AZURE\_úložiště\_připojení \_Řetězec pro požadované informace tooconnect tooyour účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="acd32-122">hello azure module will read hello environment variables AZURE\_STORAGE\_ACCOUNT and AZURE\_STORAGE\_ACCESS\_KEY, or AZURE\_STORAGE\_CONNECTION\_STRING for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="acd32-123">Pokud nejsou nastavené těchto proměnných prostředí, musíte zadat informace o účtu hello při volání metody **TableService**.</span><span class="sxs-lookup"><span data-stu-id="acd32-123">If these environment variables are not set, you must specify hello account information when calling **TableService**.</span></span>

<span data-ttu-id="acd32-124">Příklad nastavení proměnných prostředí hello v hello [portál Azure](https://portal.azure.com) web Azure, najdete v části [Node.js webové aplikace pomocí služby Azure Table hello](../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span><span class="sxs-lookup"><span data-stu-id="acd32-124">For an example of setting hello environment variables in hello [Azure portal](https://portal.azure.com) for an Azure Website, see [Node.js web app using hello Azure Table Service](../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span></span>

## <a name="create-a-table"></a><span data-ttu-id="acd32-125">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="acd32-125">Create a table</span></span>
<span data-ttu-id="acd32-126">Hello následující kód vytvoří **TableService** objektu a používá je toocreate novou tabulku.</span><span class="sxs-lookup"><span data-stu-id="acd32-126">hello following code creates a **TableService** object and uses it toocreate a new table.</span></span> <span data-ttu-id="acd32-127">Přidejte následující hello v hello horní části **server.js**.</span><span class="sxs-lookup"><span data-stu-id="acd32-127">Add hello following near hello top of **server.js**.</span></span>

```nodejs
var tableSvc = azure.createTableService();
```

<span data-ttu-id="acd32-128">Hello volání příliš**createTableIfNotExists** vytvoří novou tabulku se zadaným názvem hello, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="acd32-128">hello call too**createTableIfNotExists** will create a new table with hello specified name if it does not already exist.</span></span> <span data-ttu-id="acd32-129">Hello následující příklad vytvoří novou tabulku s názvem "mytable", pokud ještě neexistuje:</span><span class="sxs-lookup"><span data-stu-id="acd32-129">hello following example creates a new table named 'mytable' if it does not already exist:</span></span>

```nodejs
tableSvc.createTableIfNotExists('mytable', function(error, result, response){
  if(!error){
    // Table exists or created
  }
});
```

<span data-ttu-id="acd32-130">Hello `result.created` bude `true` Pokud se vytvoří nové tabulky, a `false` Pokud hello tabulka již existuje.</span><span class="sxs-lookup"><span data-stu-id="acd32-130">hello `result.created` will be `true` if a new table is created, and `false` if hello table already exists.</span></span> <span data-ttu-id="acd32-131">Hello `response` bude obsahovat informace o požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="acd32-131">hello `response` will contain information about hello request.</span></span>

### <a name="filters"></a><span data-ttu-id="acd32-132">Filtry</span><span class="sxs-lookup"><span data-stu-id="acd32-132">Filters</span></span>
<span data-ttu-id="acd32-133">Volitelné filtrování operace může být použité toooperations provádí pomocí **TableService**.</span><span class="sxs-lookup"><span data-stu-id="acd32-133">Optional filtering operations can be applied toooperations performed using **TableService**.</span></span> <span data-ttu-id="acd32-134">Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu podpisem hello:</span><span class="sxs-lookup"><span data-stu-id="acd32-134">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```nodejs
function handle (requestOptions, next)
```

<span data-ttu-id="acd32-135">Až to předzpracování na možnosti hello požadavku, hello musí metoda toocall tlačítko Další"předání zpětné volání s hello následující podpis:</span><span class="sxs-lookup"><span data-stu-id="acd32-135">After doing its preprocessing on hello request options, hello method needs toocall "next", passing a callback with hello following signature:</span></span>

```nodejs
function (returnObject, finalCallback, next)
```

<span data-ttu-id="acd32-136">V této zpětného volání a po zpracování hello returnObject (hello odpověď ze serveru toohello požadavek hello), zpětné volání hello musí tooeither vyvolat další, pokud existuje toocontinue zpracování ostatní filtry nebo jednoduše vyvolat finalCallback jinak tooend hello volání služby.</span><span class="sxs-lookup"><span data-stu-id="acd32-136">In this callback, and after processing hello returnObject (hello response from hello request toohello server), hello callback needs tooeither invoke next if it exists toocontinue processing other filters or simply invoke finalCallback otherwise tooend hello service invocation.</span></span>

<span data-ttu-id="acd32-137">Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí hello Azure SDK pro Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="acd32-137">Two filters that implement retry logic are included with hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="acd32-138">vytvoří následující Hello **TableService** objekt, který používá hello **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="acd32-138">hello following creates a **TableService** object that uses hello **ExponentialRetryPolicyFilter**:</span></span>

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var tableSvc = azure.createTableService().withFilter(retryOperations);
```

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="acd32-139">Přidat tooa tabulka entity</span><span class="sxs-lookup"><span data-stu-id="acd32-139">Add an entity tooa table</span></span>
<span data-ttu-id="acd32-140">tooadd entity, nejprve vytvořit objekt, který definuje vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="acd32-140">tooadd an entity, first create an object that defines your entity properties.</span></span> <span data-ttu-id="acd32-141">Musí obsahovat všechny entity **PartitionKey** a **RowKey**, které jsou jedinečné identifikátory pro entitu hello.</span><span class="sxs-lookup"><span data-stu-id="acd32-141">All entities must contain a **PartitionKey** and **RowKey**, which are unique identifiers for hello entity.</span></span>

* <span data-ttu-id="acd32-142">**PartitionKey** -určuje hello oddílu hello entity uložená v</span><span class="sxs-lookup"><span data-stu-id="acd32-142">**PartitionKey** - determines hello partition that hello entity is stored in</span></span>
* <span data-ttu-id="acd32-143">**RowKey** – jednoznačně identifikuje hello entity v rámci oddílu hello</span><span class="sxs-lookup"><span data-stu-id="acd32-143">**RowKey** - uniquely identifies hello entity within hello partition</span></span>

<span data-ttu-id="acd32-144">Obě **PartitionKey** a **RowKey** musí být řetězcové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="acd32-144">Both **PartitionKey** and **RowKey** must be string values.</span></span> <span data-ttu-id="acd32-145">Další informace najdete v tématu [hello Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="acd32-145">For more information, see [Understanding hello Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

<span data-ttu-id="acd32-146">Hello následuje příklad definování entity.</span><span class="sxs-lookup"><span data-stu-id="acd32-146">hello following is an example of defining an entity.</span></span> <span data-ttu-id="acd32-147">Všimněte si, že **dueDate** je definována jako typ **Edm.DateTime**.</span><span class="sxs-lookup"><span data-stu-id="acd32-147">Note that **dueDate** is defined as a type of **Edm.DateTime**.</span></span> <span data-ttu-id="acd32-148">Zadání typu hello je volitelný a typy bude možné odvodit, pokud není zadán.</span><span class="sxs-lookup"><span data-stu-id="acd32-148">Specifying hello type is optional, and types will be inferred if not specified.</span></span>

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'take out hello trash'},
  dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
};
```

> [!NOTE]
> <span data-ttu-id="acd32-149">K dispozici je také **časové razítko** pole pro každý záznam, který je nastavena jako Azure po vložení nebo aktualizaci entity.</span><span class="sxs-lookup"><span data-stu-id="acd32-149">There is also a **Timestamp** field for each record, which is set by Azure when an entity is inserted or updated.</span></span>
>
>

<span data-ttu-id="acd32-150">Můžete taky hello **entityGenerator** toocreate entity.</span><span class="sxs-lookup"><span data-stu-id="acd32-150">You can also use hello **entityGenerator** toocreate entities.</span></span> <span data-ttu-id="acd32-151">Hello následující příklad vytvoří hello stejné entity úloh pomocí hello **entityGenerator**.</span><span class="sxs-lookup"><span data-stu-id="acd32-151">hello following example creates hello same task entity using hello **entityGenerator**.</span></span>

```nodejs
var entGen = azure.TableUtilities.entityGenerator;
var task = {
  PartitionKey: entGen.String('hometasks'),
  RowKey: entGen.String('1'),
  description: entGen.String('take out hello trash'),
  dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
};
```

<span data-ttu-id="acd32-152">tooadd tabulku tooyour entity předat hello entity objektu toohello **insertEntity** metoda.</span><span class="sxs-lookup"><span data-stu-id="acd32-152">tooadd an entity tooyour table, pass hello entity object toohello **insertEntity** method.</span></span>

```nodejs
tableSvc.insertEntity('mytable',task, function (error, result, response) {
  if(!error){
    // Entity inserted
  }
});
```

<span data-ttu-id="acd32-153">Pokud hello operace úspěšná, `result` bude obsahovat hello [značka ETag](http://en.wikipedia.org/wiki/HTTP_ETag) z hello vložit záznam a `response` bude obsahovat informace o operaci hello.</span><span class="sxs-lookup"><span data-stu-id="acd32-153">If hello operation is successful, `result` will contain hello [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) of hello inserted record and `response` will contain information about hello operation.</span></span>

<span data-ttu-id="acd32-154">Příklad odpovědi:</span><span class="sxs-lookup"><span data-stu-id="acd32-154">Example response:</span></span>

```nodejs
{ '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }
```

> [!NOTE]
> <span data-ttu-id="acd32-155">Ve výchozím nastavení **insertEntity** nevrací entity hello vložit jako součást hello `response` informace.</span><span class="sxs-lookup"><span data-stu-id="acd32-155">By default, **insertEntity** does not return hello inserted entity as part of hello `response` information.</span></span> <span data-ttu-id="acd32-156">Pokud plánujete provádí jiné operace na tuto entitu, nebo chcete toocache hello informace, může být užitečné toohave vrátil jako součást hello `result`.</span><span class="sxs-lookup"><span data-stu-id="acd32-156">If you plan on performing other operations on this entity, or wish toocache hello information, it can be useful toohave it returned as part of hello `result`.</span></span> <span data-ttu-id="acd32-157">Můžete k tomu povolením **echoContent** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="acd32-157">You can do this by enabling **echoContent** as follows:</span></span>
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`
>
>

## <a name="update-an-entity"></a><span data-ttu-id="acd32-158">Aktualizace entity</span><span class="sxs-lookup"><span data-stu-id="acd32-158">Update an entity</span></span>
<span data-ttu-id="acd32-159">Existuje několik metod k dispozici tooupdate stávající entity:</span><span class="sxs-lookup"><span data-stu-id="acd32-159">There are multiple methods available tooupdate an existing entity:</span></span>

* <span data-ttu-id="acd32-160">**replaceEntity** -aktualizace stávající entity podle jeho nahrazení</span><span class="sxs-lookup"><span data-stu-id="acd32-160">**replaceEntity** - updates an existing entity by replacing it</span></span>
* <span data-ttu-id="acd32-161">**mergeEntity** -aktualizace stávající entity sloučením nové hodnoty vlastností do stávající entity hello</span><span class="sxs-lookup"><span data-stu-id="acd32-161">**mergeEntity** - updates an existing entity by merging new property values into hello existing entity</span></span>
* <span data-ttu-id="acd32-162">**insertOrReplaceEntity** -aktualizace stávající entity podle jeho nahrazení.</span><span class="sxs-lookup"><span data-stu-id="acd32-162">**insertOrReplaceEntity** - updates an existing entity by replacing it.</span></span> <span data-ttu-id="acd32-163">Pokud žádná entita existuje, bude možné vložit nový</span><span class="sxs-lookup"><span data-stu-id="acd32-163">If no entity exists, a new one will be inserted</span></span>
* <span data-ttu-id="acd32-164">**insertOrMergeEntity** -aktualizace stávající entity sloučením nové hodnoty vlastností do stávající hello.</span><span class="sxs-lookup"><span data-stu-id="acd32-164">**insertOrMergeEntity** - updates an existing entity by merging new property values into hello existing.</span></span> <span data-ttu-id="acd32-165">Pokud žádná entita existuje, bude možné vložit nový</span><span class="sxs-lookup"><span data-stu-id="acd32-165">If no entity exists, a new one will be inserted</span></span>

<span data-ttu-id="acd32-166">Hello následující příklad ukazuje, aktualizuje entitu s využitím **replaceEntity**:</span><span class="sxs-lookup"><span data-stu-id="acd32-166">hello following example demonstrates updating an entity using **replaceEntity**:</span></span>

```nodejs
tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
  if(!error) {
    // Entity updated
  }
});
```

> [!NOTE]
> <span data-ttu-id="acd32-167">Ve výchozím nastavení aktualizaci entity nekontroluje toosee Pokud aktualizovaných dat hello dříve bylo změněno jiným procesem.</span><span class="sxs-lookup"><span data-stu-id="acd32-167">By default, updating an entity does not check toosee if hello data being updated has previously been modified by another process.</span></span> <span data-ttu-id="acd32-168">Souběžná aktualizace toosupport:</span><span class="sxs-lookup"><span data-stu-id="acd32-168">toosupport concurrent updates:</span></span>
>
> 1. <span data-ttu-id="acd32-169">Získáte hello značka ETag objektu hello aktualizované.</span><span class="sxs-lookup"><span data-stu-id="acd32-169">Get hello ETag of hello object being updated.</span></span> <span data-ttu-id="acd32-170">K této chybě dochází v rámci hello `response` pro všechny operace související entity a mohou být načteny prostřednictvím `response['.metadata'].etag`.</span><span class="sxs-lookup"><span data-stu-id="acd32-170">This is returned as part of hello `response` for any entity-related operation and can be retrieved through `response['.metadata'].etag`.</span></span>
> 2. <span data-ttu-id="acd32-171">Při provádění operace aktualizace na entitu, přidejte načíst hello ETag informace dříve toohello novou entitu.</span><span class="sxs-lookup"><span data-stu-id="acd32-171">When performing an update operation on an entity, add hello ETag information previously retrieved toohello new entity.</span></span> <span data-ttu-id="acd32-172">Například:</span><span class="sxs-lookup"><span data-stu-id="acd32-172">For example:</span></span>
>
>       <span data-ttu-id="acd32-173">entity2 [.metadata] .etag = currentEtag;</span><span class="sxs-lookup"><span data-stu-id="acd32-173">entity2['.metadata'].etag = currentEtag;</span></span>
> 3. <span data-ttu-id="acd32-174">Proveďte operaci aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="acd32-174">Perform hello update operation.</span></span> <span data-ttu-id="acd32-175">Pokud byl upraven hello entity vzhledem k tomu, že jste získali hello hodnota ETag, jako je například jiná instance aplikace, `error` bude vrácen s oznámením, že nebyla splněna podmínka aktualizace hello určený v požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="acd32-175">If hello entity has been modified since you retrieved hello ETag value, such as another instance of your application, an `error` will be returned stating that hello update condition specified in hello request was not satisfied.</span></span>
>
>

<span data-ttu-id="acd32-176">S **replaceEntity** a **mergeEntity**, pokud neexistuje hello entity, který se právě aktualizuje, pak hello operace aktualizace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="acd32-176">With **replaceEntity** and **mergeEntity**, if hello entity that is being updated doesn't exist, then hello update operation will fail.</span></span> <span data-ttu-id="acd32-177">Proto pokud chcete toostore entity bez ohledu na to, jestli už existuje, použijte **insertOrReplaceEntity** nebo **insertOrMergeEntity**.</span><span class="sxs-lookup"><span data-stu-id="acd32-177">Therefore if you wish toostore an entity regardless of whether it already exists, use **insertOrReplaceEntity** or **insertOrMergeEntity**.</span></span>

<span data-ttu-id="acd32-178">Hello `result` operace úspěšná aktualizace bude obsahovat hello **Značka Etag** hello aktualizovat entity.</span><span class="sxs-lookup"><span data-stu-id="acd32-178">hello `result` for successful update operations will contain hello **Etag** of hello updated entity.</span></span>

## <a name="work-with-groups-of-entities"></a><span data-ttu-id="acd32-179">Práce se skupinami entit</span><span class="sxs-lookup"><span data-stu-id="acd32-179">Work with groups of entities</span></span>
<span data-ttu-id="acd32-180">Někdy má smysl toosubmit více operací společně v batch tooensure atomic zpracování serverem hello.</span><span class="sxs-lookup"><span data-stu-id="acd32-180">Sometimes it makes sense toosubmit multiple operations together in a batch tooensure atomic processing by hello server.</span></span> <span data-ttu-id="acd32-181">tooaccomplish, který použít hello **TableBatch** třídy toocreate dávky a pak použijte hello **executeBatch** metodu **TableService** tooperform hello zpracovat v dávce operace.</span><span class="sxs-lookup"><span data-stu-id="acd32-181">tooaccomplish that, use hello **TableBatch** class toocreate a batch, and then use hello **executeBatch** method of **TableService** tooperform hello batched operations.</span></span>

 <span data-ttu-id="acd32-182">Hello následující příklad ukazuje, odesílání dvě entity v dávce:</span><span class="sxs-lookup"><span data-stu-id="acd32-182">hello following example demonstrates submitting two entities in a batch:</span></span>

```nodejs
var task1 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'Take out hello trash'},
  dueDate: {'_':new Date(2015, 6, 20)}
};
var task2 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '2'},
  description: {'_':'Wash hello dishes'},
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

<span data-ttu-id="acd32-183">Pro úspěšné dávkových operací `result` bude obsahovat informace o jednotlivých operací ve službě hello batch.</span><span class="sxs-lookup"><span data-stu-id="acd32-183">For successful batch operations, `result` will contain information for each operation in hello batch.</span></span>

### <a name="work-with-batched-operations"></a><span data-ttu-id="acd32-184">Práce s dávkové operace</span><span class="sxs-lookup"><span data-stu-id="acd32-184">Work with batched operations</span></span>
<span data-ttu-id="acd32-185">Operace přidány tooa batch může být prověřovány zobrazením hello `operations` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="acd32-185">Operations added tooa batch can be inspected by viewing hello `operations` property.</span></span> <span data-ttu-id="acd32-186">Můžete také použít následující metody toowork s operacemi hello:</span><span class="sxs-lookup"><span data-stu-id="acd32-186">You can also use hello following methods toowork with operations:</span></span>

* <span data-ttu-id="acd32-187">**Vymazat** -vymaže všechny operace z dávky</span><span class="sxs-lookup"><span data-stu-id="acd32-187">**clear** - clears all operations from a batch</span></span>
* <span data-ttu-id="acd32-188">**getOperations** -získá operace z hello batch</span><span class="sxs-lookup"><span data-stu-id="acd32-188">**getOperations** - gets an operation from hello batch</span></span>
* <span data-ttu-id="acd32-189">**hasOperations** -vrátí hodnotu true, pokud hello batch obsahuje operace</span><span class="sxs-lookup"><span data-stu-id="acd32-189">**hasOperations** - returns true if hello batch contains operations</span></span>
* <span data-ttu-id="acd32-190">**removeOperations** – odebere operace</span><span class="sxs-lookup"><span data-stu-id="acd32-190">**removeOperations** - removes an operation</span></span>
* <span data-ttu-id="acd32-191">**velikost** – vrátí text hello počet operací v dávce hello</span><span class="sxs-lookup"><span data-stu-id="acd32-191">**size** - returns hello number of operations in hello batch</span></span>

## <a name="retrieve-an-entity-by-key"></a><span data-ttu-id="acd32-192">Načtení entity pomocí klíče</span><span class="sxs-lookup"><span data-stu-id="acd32-192">Retrieve an entity by key</span></span>
<span data-ttu-id="acd32-193">tooreturn konkrétní entitu podle hello **PartitionKey** a **RowKey**, použijte hello **retrieveEntity** metoda.</span><span class="sxs-lookup"><span data-stu-id="acd32-193">tooreturn a specific entity based on hello **PartitionKey** and **RowKey**, use hello **retrieveEntity** method.</span></span>

```nodejs
tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
  if(!error){
    // result contains hello entity
  }
});
```

<span data-ttu-id="acd32-194">Po dokončení této operace `result` bude obsahovat hello entity.</span><span class="sxs-lookup"><span data-stu-id="acd32-194">Once this operation is complete, `result` will contain hello entity.</span></span>

## <a name="query-a-set-of-entities"></a><span data-ttu-id="acd32-195">Dotaz na sadu entit</span><span class="sxs-lookup"><span data-stu-id="acd32-195">Query a set of entities</span></span>
<span data-ttu-id="acd32-196">tooquery tabulky, použijte hello **TableQuery** objektu toobuild do výrazu dotazu pomocí hello následující klauzulí:</span><span class="sxs-lookup"><span data-stu-id="acd32-196">tooquery a table, use hello **TableQuery** object toobuild up a query expression using hello following clauses:</span></span>

* <span data-ttu-id="acd32-197">**Vyberte** -hello pole toobe vrácená z dotazu hello</span><span class="sxs-lookup"><span data-stu-id="acd32-197">**select** - hello fields toobe returned from hello query</span></span>
* <span data-ttu-id="acd32-198">**kde** – hello kde – klauzule</span><span class="sxs-lookup"><span data-stu-id="acd32-198">**where** - hello where clause</span></span>

  * <span data-ttu-id="acd32-199">**a** – `and` kde podmínky</span><span class="sxs-lookup"><span data-stu-id="acd32-199">**and** - an `and` where condition</span></span>
  * <span data-ttu-id="acd32-200">**nebo** – `or` kde podmínky</span><span class="sxs-lookup"><span data-stu-id="acd32-200">**or** - an `or` where condition</span></span>
* <span data-ttu-id="acd32-201">**horní** -hello počet položek toofetch</span><span class="sxs-lookup"><span data-stu-id="acd32-201">**top** - hello number of items toofetch</span></span>

<span data-ttu-id="acd32-202">Hello následující příklad vytvoří dotaz, který vrátí hello nejvyšší pěti položek s PartitionKey 'hometasks'.</span><span class="sxs-lookup"><span data-stu-id="acd32-202">hello following example builds a query that will return hello top five items with a PartitionKey of 'hometasks'.</span></span>

```nodejs
var query = new azure.TableQuery()
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

<span data-ttu-id="acd32-203">Vzhledem k tomu **vyberte** se nepoužívá, bude vrácen všechna pole.</span><span class="sxs-lookup"><span data-stu-id="acd32-203">Since **select** is not used, all fields will be returned.</span></span> <span data-ttu-id="acd32-204">tooperform hello dotaz na tabulku, použijte **queryEntities**.</span><span class="sxs-lookup"><span data-stu-id="acd32-204">tooperform hello query against a table, use **queryEntities**.</span></span> <span data-ttu-id="acd32-205">Hello následující příklad používá tento query tooreturn entity z "mytable".</span><span class="sxs-lookup"><span data-stu-id="acd32-205">hello following example uses this query tooreturn entities from 'mytable'.</span></span>

```nodejs
tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
  if(!error) {
    // query was successful
  }
});
```

<span data-ttu-id="acd32-206">V případě úspěšného `result.entries` bude obsahovat pole entit, které odpovídají dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="acd32-206">If successful, `result.entries` will contain an array of entities that match hello query.</span></span> <span data-ttu-id="acd32-207">Pokud byl hello dotaz nelze tooreturn všechny entity, `result.continuationToken` bude jinou hodnotu než*null* , můžete použít jako hello třetí parametr **queryEntities** tooretrieve více výsledků.</span><span class="sxs-lookup"><span data-stu-id="acd32-207">If hello query was unable tooreturn all entities, `result.continuationToken` will be non-*null* and can be used as hello third parameter of **queryEntities** tooretrieve more results.</span></span> <span data-ttu-id="acd32-208">Hello počáteční dotaz, použít *null* pro třetí parametr hello.</span><span class="sxs-lookup"><span data-stu-id="acd32-208">For hello initial query, use *null* for hello third parameter.</span></span>

### <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="acd32-209">Dotaz na podmnožinu vlastností entity</span><span class="sxs-lookup"><span data-stu-id="acd32-209">Query a subset of entity properties</span></span>
<span data-ttu-id="acd32-210">Tabulku tooa dotaz můžete načíst několika pole z entity.</span><span class="sxs-lookup"><span data-stu-id="acd32-210">A query tooa table can retrieve just a few fields from an entity.</span></span>
<span data-ttu-id="acd32-211">To zmenšuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entity.</span><span class="sxs-lookup"><span data-stu-id="acd32-211">This reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="acd32-212">Použití hello **vyberte** vrátil klauzule a předejte jí hello názvy polí toobe hello.</span><span class="sxs-lookup"><span data-stu-id="acd32-212">Use hello **select** clause and pass hello names of hello fields toobe returned.</span></span> <span data-ttu-id="acd32-213">Například hello následující dotaz vrátí jenom hello **popis** a **dueDate** pole.</span><span class="sxs-lookup"><span data-stu-id="acd32-213">For example, hello following query will return only hello **description** and **dueDate** fields.</span></span>

```nodejs
var query = new azure.TableQuery()
  .select(['description', 'dueDate'])
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

## <a name="delete-an-entity"></a><span data-ttu-id="acd32-214">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="acd32-214">Delete an entity</span></span>
<span data-ttu-id="acd32-215">Můžete odstranit pomocí jeho klíče oddílu a řádku entity.</span><span class="sxs-lookup"><span data-stu-id="acd32-215">You can delete an entity using its partition and row keys.</span></span> <span data-ttu-id="acd32-216">V tomto příkladu hello **task1** objekt obsahuje hello **RowKey** a **PartitionKey** hodnoty toobe entity hello odstranit.</span><span class="sxs-lookup"><span data-stu-id="acd32-216">In this example, hello **task1** object contains hello **RowKey** and **PartitionKey** values of hello entity toobe deleted.</span></span> <span data-ttu-id="acd32-217">Pak je předán hello objekt toohello **deleteEntity** metoda.</span><span class="sxs-lookup"><span data-stu-id="acd32-217">Then hello object is passed toohello **deleteEntity** method.</span></span>

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
> <span data-ttu-id="acd32-218">Zvažte použití značky etag binárním rozsáhlým při odstraňování položek, tooensure, který hello položky nebyl změněn jiným procesem.</span><span class="sxs-lookup"><span data-stu-id="acd32-218">Consider using ETags when deleting items, tooensure that hello item hasn't been modified by another process.</span></span> <span data-ttu-id="acd32-219">V tématu [aktualizovat entitu,](#update-an-entity) informace o použití značky etag binárním rozsáhlým.</span><span class="sxs-lookup"><span data-stu-id="acd32-219">See [Update an entity](#update-an-entity) for information on using ETags.</span></span>
>
>

## <a name="delete-a-table"></a><span data-ttu-id="acd32-220">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="acd32-220">Delete a table</span></span>
<span data-ttu-id="acd32-221">Následující kód Hello odstraní tabulku z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="acd32-221">hello following code deletes a table from a storage account.</span></span>

```nodejs
tableSvc.deleteTable('mytable', function(error, response){
    if(!error){
        // Table deleted
    }
});
```

<span data-ttu-id="acd32-222">Pokud si nejste jisti, zda text hello tabulka existuje, použijte **deleteTableIfExists**.</span><span class="sxs-lookup"><span data-stu-id="acd32-222">If you are uncertain whether hello table exists, use **deleteTableIfExists**.</span></span>

## <a name="use-continuation-tokens"></a><span data-ttu-id="acd32-223">Použít pokračování tokeny</span><span class="sxs-lookup"><span data-stu-id="acd32-223">Use continuation tokens</span></span>
<span data-ttu-id="acd32-224">Když dotazujete tabulky pro velké objemy výsledky, vyhledejte pokračování tokeny.</span><span class="sxs-lookup"><span data-stu-id="acd32-224">When you are querying tables for large amounts of results, look for continuation tokens.</span></span> <span data-ttu-id="acd32-225">K dispozici pro dotaz, možná zjistíte, pokud jste nevytvářejte toorecognize při token pokračování je k dispozici může být velké objemy dat.</span><span class="sxs-lookup"><span data-stu-id="acd32-225">There may be large amounts of data available for your query that you might not realize if you do not build toorecognize when a continuation token is present.</span></span>

<span data-ttu-id="acd32-226">objekt Hello výsledky vrácené během dotazování sady entit `continuationToken` vlastnost, pokud takový token je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="acd32-226">hello results object returned during querying entities sets a `continuationToken` property when such a token is present.</span></span> <span data-ttu-id="acd32-227">Pak můžete toto při provádění dotaz toocontinue toomove napříč hello oddílu a tabulka entity.</span><span class="sxs-lookup"><span data-stu-id="acd32-227">You can then use this when performing a query toocontinue toomove across hello partition and table entities.</span></span>

<span data-ttu-id="acd32-228">Při dotazování, může být zadán parametr continuationToken mezi instance objektu hello dotazu a funkce zpětného volání hello:</span><span class="sxs-lookup"><span data-stu-id="acd32-228">When querying, a continuationToken parameter may be provided between hello query object instance and hello callback function:</span></span>

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

<span data-ttu-id="acd32-229">Je-li si prohlédnout hello `continuationToken` objekt, zjistíte, vlastnosti, jako `nextPartitionKey`, `nextRowKey` a `targetLocation`, což může být použité tooiterate prostřednictvím všechny výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="acd32-229">If you inspect hello `continuationToken` object, you will find properties such as `nextPartitionKey`, `nextRowKey` and `targetLocation`, which can be used tooiterate through all hello results.</span></span>

<span data-ttu-id="acd32-230">Je také ukázku pokračování v úložišti Azure Storage Node.js hello na Githubu.</span><span class="sxs-lookup"><span data-stu-id="acd32-230">There is also a continuation sample within hello Azure Storage Node.js repo on GitHub.</span></span> <span data-ttu-id="acd32-231">Vyhledejte `examples/samples/continuationsample.js`.</span><span class="sxs-lookup"><span data-stu-id="acd32-231">Look for `examples/samples/continuationsample.js`.</span></span>

## <a name="work-with-shared-access-signatures"></a><span data-ttu-id="acd32-232">Práce s podpisy sdíleného přístupu</span><span class="sxs-lookup"><span data-stu-id="acd32-232">Work with shared access signatures</span></span>
<span data-ttu-id="acd32-233">Sdílené přístupové podpisy (SAS) jsou bezpečný tooprovide granulární přístup tootables bez zadání názvu účtu úložiště nebo klíče.</span><span class="sxs-lookup"><span data-stu-id="acd32-233">Shared access signatures (SAS) are a secure way tooprovide granular access tootables without providing your storage account name or keys.</span></span> <span data-ttu-id="acd32-234">SAS jsou často používané tooprovide omezený přístup tooyour data, například povolení mobilní aplikace tooquery záznamy.</span><span class="sxs-lookup"><span data-stu-id="acd32-234">SAS are often used tooprovide limited access tooyour data, such as allowing a mobile app tooquery records.</span></span>

<span data-ttu-id="acd32-235">Důvěryhodné aplikace, jako je Cloudová služba vygeneruje SAS pomocí hello **generateSharedAccessSignature** z hello **TableService**a poskytuje ji tooan nedůvěryhodné nebo částečně důvěryhodné aplikace například mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="acd32-235">A trusted application such as a cloud-based service generates a SAS using hello **generateSharedAccessSignature** of hello **TableService**, and provides it tooan untrusted or semi-trusted application such as a mobile app.</span></span> <span data-ttu-id="acd32-236">Hello SAS je generována pomocí zásad, která popisuje hello počáteční a koncové datum během které hello SAS je platný, a také hello držitel SAS úrovně toohello udělí přístup.</span><span class="sxs-lookup"><span data-stu-id="acd32-236">hello SAS is generated using a policy, which describes hello start and end dates during which hello SAS is valid, as well as hello access level granted toohello SAS holder.</span></span>

<span data-ttu-id="acd32-237">Hello následující příklad vytvoří nové zásady sdíleného přístupu, který vám umožní hello SAS držitel tooquery (r) hello tabulky a vyprší platnost 100 minut po času hello, je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="acd32-237">hello following example generates a new shared access policy that will allow hello SAS holder tooquery ('r') hello table, and expires 100 minutes after hello time it is created.</span></span>

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

<span data-ttu-id="acd32-238">Všimněte si, že informace o hostiteli hello je nutno zadat také, jako je povinný při hello SAS držitel pokusí tooaccess hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="acd32-238">Note that hello host information must be provided also, as it is required when hello SAS holder attempts tooaccess hello table.</span></span>

<span data-ttu-id="acd32-239">Hello klientskou aplikaci, pak používá hello SAS s **TableServiceWithSAS** tooperform operace s tabulkou hello.</span><span class="sxs-lookup"><span data-stu-id="acd32-239">hello client application then uses hello SAS with **TableServiceWithSAS** tooperform operations against hello table.</span></span> <span data-ttu-id="acd32-240">Následující ukázka Hello připojí toohello tabulky a provádí dotazu.</span><span class="sxs-lookup"><span data-stu-id="acd32-240">hello following example connects toohello table and performs a query.</span></span>

```nodejs
var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
var query = azure.TableQuery()
  .where('PartitionKey eq ?', 'hometasks');

sharedTableService.queryEntities(query, null, function(error, result, response) {
  if(!error) {
    // result contains hello entities
  }
});
```

<span data-ttu-id="acd32-241">Vzhledem k tomu, že hello SAS se vygeneroval s dotazu přístup jenom, pokud byly proveden pokus o tooinsert, aktualizace nebo odstranění entity, bude vrácena chyba.</span><span class="sxs-lookup"><span data-stu-id="acd32-241">Since hello SAS was generated with only query access, if an attempt were made tooinsert, update, or delete entities, an error would be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="acd32-242">Seznamy řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="acd32-242">Access Control Lists</span></span>
<span data-ttu-id="acd32-243">Můžete taky zásady přístupu hello tooset seznamu řízení přístupu (ACL) pro SAS.</span><span class="sxs-lookup"><span data-stu-id="acd32-243">You can also use an Access Control List (ACL) tooset hello access policy for a SAS.</span></span> <span data-ttu-id="acd32-244">To je užitečné, pokud chcete tooallow více klientů tooaccess hello tabulky, ale poskytnutí zásad, jiný přístup pro každého klienta.</span><span class="sxs-lookup"><span data-stu-id="acd32-244">This is useful if you wish tooallow multiple clients tooaccess hello table, but provide different access policies for each client.</span></span>

<span data-ttu-id="acd32-245">Seznam ACL je implementovaná pomocí pole zásady přístupu s ID spojené s každou zásadu.</span><span class="sxs-lookup"><span data-stu-id="acd32-245">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="acd32-246">Následující ukázka Hello definuje dvě zásady, jeden pro "uživatel1" a jeden pro, uživatel2":</span><span class="sxs-lookup"><span data-stu-id="acd32-246">hello following example defines two policies, one for 'user1' and one for 'user2':</span></span>

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

<span data-ttu-id="acd32-247">Následující příklad získá Hello hello aktuální seznam ACL pro hello **hometasks** tabulky a potom se přidají nové zásady hello pomocí **setTableAcl**.</span><span class="sxs-lookup"><span data-stu-id="acd32-247">hello following example gets hello current ACL for hello **hometasks** table, and then adds hello new policies using **setTableAcl**.</span></span> <span data-ttu-id="acd32-248">Tento přístup umožňuje:</span><span class="sxs-lookup"><span data-stu-id="acd32-248">This approach allows:</span></span>

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

<span data-ttu-id="acd32-249">Jednou hello byla nastavena seznamu ACL, potom můžete vytvořit na základě hello ID pro zásadu SAS.</span><span class="sxs-lookup"><span data-stu-id="acd32-249">Once hello ACL has been set, you can then create a SAS based on hello ID for a policy.</span></span> <span data-ttu-id="acd32-250">Hello následující příklad vytvoří nový SAS pro, uživatel2":</span><span class="sxs-lookup"><span data-stu-id="acd32-250">hello following example creates a new SAS for 'user2':</span></span>

```nodejs
tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="acd32-251">Další kroky</span><span class="sxs-lookup"><span data-stu-id="acd32-251">Next steps</span></span>
<span data-ttu-id="acd32-252">Další informace najdete v tématu hello následující prostředky.</span><span class="sxs-lookup"><span data-stu-id="acd32-252">For more information, see hello following resources.</span></span>

* <span data-ttu-id="acd32-253">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="acd32-253">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="acd32-254">[Azure SDK úložiště pro uzel](https://github.com/Azure/azure-storage-node) úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="acd32-254">[Azure Storage SDK for Node](https://github.com/Azure/azure-storage-node) repository on GitHub.</span></span>
* [<span data-ttu-id="acd32-255">Středisko pro vývojáře Node.js</span><span class="sxs-lookup"><span data-stu-id="acd32-255">Node.js Developer Center</span></span>](/develop/nodejs/)
* [<span data-ttu-id="acd32-256">Vytvoření a nasazení tooan aplikace Node.js webu Azure</span><span class="sxs-lookup"><span data-stu-id="acd32-256">Create and deploy a Node.js application tooan Azure website</span></span>](../app-service-web/app-service-web-get-started-nodejs.md)
