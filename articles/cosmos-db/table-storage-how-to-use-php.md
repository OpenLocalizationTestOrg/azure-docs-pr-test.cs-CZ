---
title: "aaaHow toouse úložiště table z PHP | Microsoft Docs"
description: "Zjistěte, jak toouse hello služby Table z PHP toocreate a odstranění tabulky, vložení, odstranění a tabulku hello dotazů."
services: cosmos-db
documentationcenter: php
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 1e57f371-6208-4753-b2a0-05db4aede8e3
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 5b7c92221069d1c2a6ca951c06ae8eea8bb8478c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-php"></a><span data-ttu-id="d7584-103">Jak toouse tabulky úložiště z PHP</span><span class="sxs-lookup"><span data-stu-id="d7584-103">How toouse table storage from PHP</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="d7584-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="d7584-104">Overview</span></span>
<span data-ttu-id="d7584-105">Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Table.</span><span class="sxs-lookup"><span data-stu-id="d7584-105">This guide shows you how tooperform common scenarios using hello Azure Table service.</span></span> <span data-ttu-id="d7584-106">Hello ukázky jsou napsané v jazyce PHP a používají hello [Azure SDK pro jazyk PHP][download].</span><span class="sxs-lookup"><span data-stu-id="d7584-106">hello samples are written in PHP and use hello [Azure SDK for PHP][download].</span></span> <span data-ttu-id="d7584-107">Hello pokryté scénáře zahrnují **vytváření a odstraňování tabulek a vkládání, odstraňování a dotazování entity v tabulce**.</span><span class="sxs-lookup"><span data-stu-id="d7584-107">hello scenarios covered include **creating and deleting a table, and inserting, deleting, and querying entities in a table**.</span></span> <span data-ttu-id="d7584-108">Další informace o hello služby Azure Table, najdete v části hello [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="d7584-108">For more information on hello Azure Table service, see hello [Next steps](#next-steps) section.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="d7584-109">Vytvoření aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="d7584-109">Create a PHP application</span></span>
<span data-ttu-id="d7584-110">Hello jen požadavek pro vytvoření aplikace PHP, který přistupuje k službě Azure Table hello je hello odkazování tříd v hello Azure SDK pro jazyk PHP z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="d7584-110">hello only requirement for creating a PHP application that accesses hello Azure Table service is hello referencing of classes in hello Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="d7584-111">Můžete použít všechny toocreate nástroje pro vývoj aplikace, včetně Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="d7584-111">You can use any development tools toocreate your application, including Notepad.</span></span>

<span data-ttu-id="d7584-112">V této příručce používat funkce služby tabulky, které lze volat z v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.</span><span class="sxs-lookup"><span data-stu-id="d7584-112">In this guide, you use Table service features which can be called from within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="d7584-113">Získání knihovny klienta Azure hello</span><span class="sxs-lookup"><span data-stu-id="d7584-113">Get hello Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-hello-table-service"></a><span data-ttu-id="d7584-114">Konfigurace služby Table hello tooaccess vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="d7584-114">Configure your application tooaccess hello Table service</span></span>
<span data-ttu-id="d7584-115">toouse hello Azure Table API služby, musíte:</span><span class="sxs-lookup"><span data-stu-id="d7584-115">toouse hello Azure Table service APIs, you need to:</span></span>

1. <span data-ttu-id="d7584-116">Referenční dokumentace hello automatického zavaděče soubor pomocí hello [require_once] [ require_once] příkaz, a</span><span class="sxs-lookup"><span data-stu-id="d7584-116">Reference hello autoloader file using hello [require_once][require_once] statement, and</span></span>
2. <span data-ttu-id="d7584-117">Referenční všechny třídy, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="d7584-117">Reference any classes you might use.</span></span>

<span data-ttu-id="d7584-118">Hello následující příklad ukazuje, jak tooinclude hello automatického zavaděče souboru a odkaz hello **ServicesBuilder** třídy.</span><span class="sxs-lookup"><span data-stu-id="d7584-118">hello following example shows how tooinclude hello autoloader file and reference hello **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="d7584-119">Hello příklady v tomto článku předpokládá, že jste nainstalovali hello PHP klientské knihovny pro Azure prostřednictvím autora.</span><span class="sxs-lookup"><span data-stu-id="d7584-119">hello examples in this article assume you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="d7584-120">Pokud jste nainstalovali hello knihovny ručně, je třeba tooreference hello <code>WindowsAzure.php</code> automatického zavaděče souboru.</span><span class="sxs-lookup"><span data-stu-id="d7584-120">If you installed hello libraries manually, you need tooreference hello <code>WindowsAzure.php</code> autoloader file.</span></span>
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="d7584-121">V následujících příkladech hello, hello `require_once` údajů se vždy zobrazí, ale pouze hello třídy potřebné pro tooexecute příklad hello odkazují.</span><span class="sxs-lookup"><span data-stu-id="d7584-121">In hello examples below, hello `require_once` statement is always shown, but only hello classes necessary for hello example tooexecute are referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="d7584-122">Nastavit připojení k úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="d7584-122">Set up an Azure storage connection</span></span>
<span data-ttu-id="d7584-123">tooinstantiate klientem služby Azure Table, že máte platný připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="d7584-123">tooinstantiate an Azure Table service client, you must first have a valid connection string.</span></span> <span data-ttu-id="d7584-124">Formát Hello hello připojovací řetězec služby tabulky je:</span><span class="sxs-lookup"><span data-stu-id="d7584-124">hello format for hello Table service connection string is:</span></span>

<span data-ttu-id="d7584-125">Pro přístup k službě za provozu:</span><span class="sxs-lookup"><span data-stu-id="d7584-125">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="d7584-126">Pro přístup k hello emulátor úložiště:</span><span class="sxs-lookup"><span data-stu-id="d7584-126">For accessing hello emulator storage:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="d7584-127">toocreate libovolného klienta služby Azure, budete potřebovat toouse hello **ServicesBuilder** třídy.</span><span class="sxs-lookup"><span data-stu-id="d7584-127">toocreate any Azure service client, you need toouse hello **ServicesBuilder** class.</span></span> <span data-ttu-id="d7584-128">Můžete:</span><span class="sxs-lookup"><span data-stu-id="d7584-128">You can:</span></span>

* <span data-ttu-id="d7584-129">Předat hello připojovací řetězec přímo tooit nebo</span><span class="sxs-lookup"><span data-stu-id="d7584-129">pass hello connection string directly tooit or</span></span>
* <span data-ttu-id="d7584-130">Použití hello **CloudConfigurationManager (CCM)** toocheck více externí zdroje pro hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="d7584-130">use hello **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="d7584-131">ve výchozím nastavení je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="d7584-131">by default, it comes with support for one external source - environmental variables</span></span>
  * <span data-ttu-id="d7584-132">můžete přidat nové zdroje rozšířením hello **ConnectionStringSource** – třída</span><span class="sxs-lookup"><span data-stu-id="d7584-132">you can add new sources by extending hello **ConnectionStringSource** class</span></span>

<span data-ttu-id="d7584-133">Příklady hello podle zde uvedeného se předají hello připojovací řetězec, který přímo.</span><span class="sxs-lookup"><span data-stu-id="d7584-133">For hello examples outlined here, hello connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);
```

## <a name="create-a-table"></a><span data-ttu-id="d7584-134">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="d7584-134">Create a table</span></span>
<span data-ttu-id="d7584-135">A **TableRestProxy** objektu umožňuje vytvořit tabulku s hello **createTable** metoda.</span><span class="sxs-lookup"><span data-stu-id="d7584-135">A **TableRestProxy** object lets you create a table with hello **createTable** method.</span></span> <span data-ttu-id="d7584-136">Při vytváření tabulky, můžete nastavit časový limit služby hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="d7584-136">When creating a table, you can set hello Table service timeout.</span></span> <span data-ttu-id="d7584-137">(Další informace o časový limit služby Table hello najdete v tématu [nastavení časových limitů pro operace služby s tabulkou][table-service-timeouts].)</span><span class="sxs-lookup"><span data-stu-id="d7584-137">(For more information about hello Table service timeout, see [Setting Timeouts for Table Service Operations][table-service-timeouts].)</span></span>

```php
require_once 'vendor\autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Create table.
    $tableRestProxy->createTable("mytable");
}
catch(ServiceException $e){
    $code = $e->getCode();
    $error_message = $e->getMessage();
    // Handle exception based on error codes and messages.
    // Error codes and messages can be found here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
}
```

<span data-ttu-id="d7584-138">Informace o omezeních na názvy tabulek najdete v tématu [hello Principy datového modelu služby Table][table-data-model].</span><span class="sxs-lookup"><span data-stu-id="d7584-138">For information about restrictions on table names, see [Understanding hello Table Service Data Model][table-data-model].</span></span>

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="d7584-139">Přidat tooa tabulka entity</span><span class="sxs-lookup"><span data-stu-id="d7584-139">Add an entity tooa table</span></span>
<span data-ttu-id="d7584-140">tooadd tooa tabulka entity, vytvořte novou **Entity** objektu a předejte ji příliš**TableRestProxy -> insertEntity**.</span><span class="sxs-lookup"><span data-stu-id="d7584-140">tooadd an entity tooa table, create a new **Entity** object and pass it too**TableRestProxy->insertEntity**.</span></span> <span data-ttu-id="d7584-141">Všimněte si, že při vytváření entity, musíte zadat `PartitionKey` a `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="d7584-141">Note that when you create an entity, you must specify a `PartitionKey` and `RowKey`.</span></span> <span data-ttu-id="d7584-142">Tyto jsou hello jedinečné identifikátory pro entitu a jsou hodnoty, které může být dotazován mnohem rychleji než ostatní vlastnosti entity.</span><span class="sxs-lookup"><span data-stu-id="d7584-142">These are hello unique identifiers for an entity and are values that can be queried much faster than other entity properties.</span></span> <span data-ttu-id="d7584-143">systém Hello používá `PartitionKey` tooautomatically distribuovat entity tabulky hello přes mnoho uzly úložiště.</span><span class="sxs-lookup"><span data-stu-id="d7584-143">hello system uses `PartitionKey` tooautomatically distribute hello table's entities over many storage nodes.</span></span> <span data-ttu-id="d7584-144">Entity s hello stejné `PartitionKey` jsou uložené na hello stejného uzlu.</span><span class="sxs-lookup"><span data-stu-id="d7584-144">Entities with hello same `PartitionKey` are stored on hello same node.</span></span> <span data-ttu-id="d7584-145">(Operace u více entit, které jsou uložené na hello stejného uzlu provádět lepší, než na entity ukládat v rámci různých uzlech.) Hello `RowKey` je hello jedinečné ID entity v rámci oddílu.</span><span class="sxs-lookup"><span data-stu-id="d7584-145">(Operations on multiple entities stored on hello same node perform better than on entities stored across different nodes.) hello `RowKey` is hello unique ID of an entity within a partition.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$entity = new Entity();
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");
$entity->addProperty("Description", null, "Take out hello trash.");
$entity->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity->addProperty("Location", EdmType::STRING, "Home");

try{
    $tableRestProxy->insertEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
}
```

<span data-ttu-id="d7584-146">Informace o vlastnosti tabulky a typy najdete v tématu [hello Principy datového modelu služby Table][table-data-model].</span><span class="sxs-lookup"><span data-stu-id="d7584-146">For information about Table properties and types, see [Understanding hello Table Service Data Model][table-data-model].</span></span>

<span data-ttu-id="d7584-147">Hello **TableRestProxy** třída nabízí dvě alternativní metody pro vložení entity: **insertOrMergeEntity** a **insertOrReplaceEntity**.</span><span class="sxs-lookup"><span data-stu-id="d7584-147">hello **TableRestProxy** class offers two alternative methods for inserting entities: **insertOrMergeEntity** and **insertOrReplaceEntity**.</span></span> <span data-ttu-id="d7584-148">toouse těchto metod, vytvořte novou **Entity** a předáváme jako parametr metody tooeither.</span><span class="sxs-lookup"><span data-stu-id="d7584-148">toouse these methods, create a new **Entity** and pass it as a parameter tooeither method.</span></span> <span data-ttu-id="d7584-149">Každá metoda budou vkládat hello entity, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="d7584-149">Each method will insert hello entity if it does not exist.</span></span> <span data-ttu-id="d7584-150">Pokud hello entity už existuje, **insertOrMergeEntity** aktualizace hodnot vlastností, pokud hello vlastnosti již existují a přidá nové vlastnosti Pokud neexistují, při **insertOrReplaceEntity** úplně nahradí stávající entity.</span><span class="sxs-lookup"><span data-stu-id="d7584-150">If hello entity already exists, **insertOrMergeEntity** updates property values if hello properties already exist and adds new properties if they do not exist, while **insertOrReplaceEntity** completely replaces an existing entity.</span></span> <span data-ttu-id="d7584-151">Následující příklad ukazuje, jak Hello toouse **insertOrMergeEntity**.</span><span class="sxs-lookup"><span data-stu-id="d7584-151">hello following example shows how toouse **insertOrMergeEntity**.</span></span> <span data-ttu-id="d7584-152">Pokud hello entita s `PartitionKey` "tasksSeattle" a `RowKey` "1" již neexistuje, bude možné vložit.</span><span class="sxs-lookup"><span data-stu-id="d7584-152">If hello entity with `PartitionKey` "tasksSeattle" and `RowKey` "1" does not already exist, it will be inserted.</span></span> <span data-ttu-id="d7584-153">Ale pokud dříve vložení (jak je uvedeno v předchozím příkladu hello), hello `DueDate` vlastnost budou aktualizovány a hello `Status` přidá vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d7584-153">However, if it has previously been inserted (as shown in hello example above), hello `DueDate` property will be updated, and hello `Status` property will be added.</span></span> <span data-ttu-id="d7584-154">Hello `Description` a `Location` jsou aktualizovány vlastnosti, ale s hodnotami, které efektivně je ponechte beze změny.</span><span class="sxs-lookup"><span data-stu-id="d7584-154">hello `Description` and `Location` properties are also updated, but with values that effectively leave them unchanged.</span></span> <span data-ttu-id="d7584-155">Pokud tyto dvě vlastnosti pozdější nebyly přidány jako v příkladu hello, ale byly na cílovou entitu hello, jejich existující hodnoty by zůstanou beze změny.</span><span class="sxs-lookup"><span data-stu-id="d7584-155">If these latter two properties were not added as shown in hello example, but existed on hello target entity, their existing values would remain unchanged.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

//Create new entity.
$entity = new Entity();

// PartitionKey and RowKey are required.
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");

// If entity exists, existing properties are updated with new values and
// new properties are added. Missing properties are unchanged.
$entity->addProperty("Description", null, "Take out hello trash.");
$entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified hello DueDate field.
$entity->addProperty("Location", EdmType::STRING, "Home");
$entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

try    {
    // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
    // would simply replace hello entity with PartitionKey "tasksSeattle" and RowKey "1".
    $tableRestProxy->insertOrMergeEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="retrieve-a-single-entity"></a><span data-ttu-id="d7584-156">Načtení jedné entity</span><span class="sxs-lookup"><span data-stu-id="d7584-156">Retrieve a single entity</span></span>
<span data-ttu-id="d7584-157">Hello **TableRestProxy -> getEntity** metoda vám umožní tooretrieve jednu entitu pomocí dotazů na jeho `PartitionKey` a `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="d7584-157">hello **TableRestProxy->getEntity** method allows you tooretrieve a single entity by querying for its `PartitionKey` and `RowKey`.</span></span> <span data-ttu-id="d7584-158">V příkladu hello níže hello klíč oddílu `tasksSeattle` a klíč řádku `1` se předávají toohello **getEntity** metoda.</span><span class="sxs-lookup"><span data-stu-id="d7584-158">In hello example below, hello partition key `tasksSeattle` and row key `1` are passed toohello **getEntity** method.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entity = $result->getEntity();

echo $entity->getPartitionKey().":".$entity->getRowKey();
```

## <a name="retrieve-all-entities-in-a-partition"></a><span data-ttu-id="d7584-159">Načtení všech entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="d7584-159">Retrieve all entities in a partition</span></span>
<span data-ttu-id="d7584-160">Dotazy na entity se vytvářejí pomocí filtrů (Další informace najdete v tématu [dotazování tabulky a entity][filters]).</span><span class="sxs-lookup"><span data-stu-id="d7584-160">Entity queries are constructed using filters (for more information, see [Querying Tables and Entities][filters]).</span></span> <span data-ttu-id="d7584-161">tooretrieve všechny entity v oddílu, použijte filtr hello "PartitionKey eq *název_oddílu*".</span><span class="sxs-lookup"><span data-stu-id="d7584-161">tooretrieve all entities in partition, use hello filter "PartitionKey eq *partition_name*".</span></span> <span data-ttu-id="d7584-162">Následující příklad ukazuje, jak Hello tooretrieve všechny entity v hello `tasksSeattle` oddílu předáním filtru toohello **queryEntities** metoda.</span><span class="sxs-lookup"><span data-stu-id="d7584-162">hello following example shows how tooretrieve all entities in hello `tasksSeattle` partition by passing a filter toohello **queryEntities** method.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$filter = "PartitionKey eq 'tasksSeattle'";

try    {
    $result = $tableRestProxy->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a><span data-ttu-id="d7584-163">Načtení podmnožinu entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="d7584-163">Retrieve a subset of entities in a partition</span></span>
<span data-ttu-id="d7584-164">Hello stejným způsobem používaným v předchozím příkladu hello lze použít tooretrieve žádné podmnožinu entit v oddílu.</span><span class="sxs-lookup"><span data-stu-id="d7584-164">hello same pattern used in hello previous example can be used tooretrieve any subset of entities in a partition.</span></span> <span data-ttu-id="d7584-165">Určuje podmnožinu Hello entit načtete hello filtru můžete použít (Další informace najdete v tématu [dotazování tabulky a entity][filters]) .hello následující příklad ukazuje, jak toouse tooretrieve filtru všechny entity s konkrétním `Location` a `DueDate` nižší než zadané datum.</span><span class="sxs-lookup"><span data-stu-id="d7584-165">hello subset of entities you retrieve are determined by hello filter you use (for more information, see [Querying Tables and Entities][filters]).hello following example shows how toouse a filter tooretrieve all entities with a specific `Location` and a `DueDate` less than a specified date.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

try    {
    $result = $tableRestProxy->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entity-properties"></a><span data-ttu-id="d7584-166">Načtení podmnožinu vlastností entity</span><span class="sxs-lookup"><span data-stu-id="d7584-166">Retrieve a subset of entity properties</span></span>
<span data-ttu-id="d7584-167">Dotaz můžete načíst podmnožinu vlastností entity.</span><span class="sxs-lookup"><span data-stu-id="d7584-167">A query can retrieve a subset of entity properties.</span></span> <span data-ttu-id="d7584-168">Tento postup volá *projekce*, omezuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entity.</span><span class="sxs-lookup"><span data-stu-id="d7584-168">This technique, called *projection*, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="d7584-169">načíst vlastnost toobe toospecify, předat název hello hello vlastnost toohello **dotazu -> addSelectField** metoda.</span><span class="sxs-lookup"><span data-stu-id="d7584-169">toospecify a property toobe retrieved, pass hello name of hello property toohello **Query->addSelectField** method.</span></span> <span data-ttu-id="d7584-170">Tato metoda několikrát tooadd můžete volat další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d7584-170">You can call this method multiple times tooadd more properties.</span></span> <span data-ttu-id="d7584-171">Po provedení **TableRestProxy -> queryEntities**, hello vrátil entity budou mít pouze hello vybrané vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d7584-171">After executing **TableRestProxy->queryEntities**, hello returned entities will only have hello selected properties.</span></span> <span data-ttu-id="d7584-172">(V Pokud chcete tooreturn podmnožinu entity tabulky, použijte filtr jak je znázorněno v dotazech hello výše.)</span><span class="sxs-lookup"><span data-stu-id="d7584-172">(If you want tooreturn a subset of Table entities, use a filter as shown in hello queries above.)</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$options = new QueryEntitiesOptions();
$options->addSelectField("Description");

try    {
    $result = $tableRestProxy->queryEntities("mytable", $options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

// All entities in hello table are returned, regardless of whether
// they have hello Description field.
// toolimit hello results returned, use a filter.
$entities = $result->getEntities();

foreach($entities as $entity){
    $description = $entity->getProperty("Description")->getValue();
    echo $description."<br />";
}
```

## <a name="update-an-entity"></a><span data-ttu-id="d7584-173">Aktualizace entity</span><span class="sxs-lookup"><span data-stu-id="d7584-173">Update an entity</span></span>
<span data-ttu-id="d7584-174">Stávající entitu můžete aktualizovat pomocí hello **Entity -> setProperty** a **Entity -> AddProperty –** v hello entitu a pak volání metody **TableRestProxy -> updateEntity **.</span><span class="sxs-lookup"><span data-stu-id="d7584-174">An existing entity can be updated by using hello **Entity->setProperty** and **Entity->addProperty** methods on hello entity, and then calling **TableRestProxy->updateEntity**.</span></span> <span data-ttu-id="d7584-175">Hello následující příklad načte entitu, upraví jednu vlastnost, odebere jinou vlastnost a přidá novou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d7584-175">hello following example retrieves an entity, modifies one property, removes another property, and adds a new property.</span></span> <span data-ttu-id="d7584-176">Všimněte si, můžete odebrat vlastnost tak, že nastavení její hodnoty příliš**null**.</span><span class="sxs-lookup"><span data-stu-id="d7584-176">Note that you can remove a property by setting its value too**null**.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

$entity = $result->getEntity();

$entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

$entity->setPropertyValue("Location", null); //Removed Location.

$entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

try    {
    $tableRestProxy->updateEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-an-entity"></a><span data-ttu-id="d7584-177">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="d7584-177">Delete an entity</span></span>
<span data-ttu-id="d7584-178">toodelete entity, předat hello název tabulky a hello entity `PartitionKey` a `RowKey` toohello **TableRestProxy -> deleteEntity** metoda.</span><span class="sxs-lookup"><span data-stu-id="d7584-178">toodelete an entity, pass hello table name, and hello entity's `PartitionKey` and `RowKey` toohello **TableRestProxy->deleteEntity** method.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Delete entity.
    $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

<span data-ttu-id="d7584-179">Všimněte si můžete nastavit pro kontrolách souběžnosti hello Etag pro entity toobe odstranit pomocí hello **DeleteEntityOptions -> setEtag** metoda a předávání hello **DeleteEntityOptions** objektu příliš**deleteEntity** jako čtvrtý parametr.</span><span class="sxs-lookup"><span data-stu-id="d7584-179">Note that for concurrency checks, you can set hello Etag for an entity toobe deleted by using hello **DeleteEntityOptions->setEtag** method and passing hello **DeleteEntityOptions** object too**deleteEntity** as a fourth parameter.</span></span>

## <a name="batch-table-operations"></a><span data-ttu-id="d7584-180">Operace s tabulkou batch</span><span class="sxs-lookup"><span data-stu-id="d7584-180">Batch table operations</span></span>
<span data-ttu-id="d7584-181">Hello **TableRestProxy -> batch** metoda vám umožní tooexecute více operací v jedné žádosti.</span><span class="sxs-lookup"><span data-stu-id="d7584-181">hello **TableRestProxy->batch** method allows you tooexecute multiple operations in a single request.</span></span> <span data-ttu-id="d7584-182">Hello vzor zde zahrnuje přidávání operací příliš**BatchRequest** objekt a následné předání hello **BatchRequest** objektu toohello **TableRestProxy -> batch** metoda.</span><span class="sxs-lookup"><span data-stu-id="d7584-182">hello pattern here involves adding operations too**BatchRequest** object and then passing hello **BatchRequest** object toohello **TableRestProxy->batch** method.</span></span> <span data-ttu-id="d7584-183">tooadd operaci tooa **BatchRequest** objekt, můžete některé z hello následující metody několikrát volat:</span><span class="sxs-lookup"><span data-stu-id="d7584-183">tooadd an operation tooa **BatchRequest** object, you can call any of hello following methods multiple times:</span></span>

* <span data-ttu-id="d7584-184">**addInsertEntity** (přidá insertEntity operace)</span><span class="sxs-lookup"><span data-stu-id="d7584-184">**addInsertEntity** (adds an insertEntity operation)</span></span>
* <span data-ttu-id="d7584-185">**addUpdateEntity** (přidá updateEntity operace)</span><span class="sxs-lookup"><span data-stu-id="d7584-185">**addUpdateEntity** (adds an updateEntity operation)</span></span>
* <span data-ttu-id="d7584-186">**addMergeEntity** (přidá mergeEntity operace)</span><span class="sxs-lookup"><span data-stu-id="d7584-186">**addMergeEntity** (adds a mergeEntity operation)</span></span>
* <span data-ttu-id="d7584-187">**addInsertOrReplaceEntity** (přidá insertOrReplaceEntity operace)</span><span class="sxs-lookup"><span data-stu-id="d7584-187">**addInsertOrReplaceEntity** (adds an insertOrReplaceEntity operation)</span></span>
* <span data-ttu-id="d7584-188">**addInsertOrMergeEntity** (přidá insertOrMergeEntity operace)</span><span class="sxs-lookup"><span data-stu-id="d7584-188">**addInsertOrMergeEntity** (adds an insertOrMergeEntity operation)</span></span>
* <span data-ttu-id="d7584-189">**addDeleteEntity** (přidá deleteEntity operace)</span><span class="sxs-lookup"><span data-stu-id="d7584-189">**addDeleteEntity** (adds a deleteEntity operation)</span></span>

<span data-ttu-id="d7584-190">Následující příklad ukazuje, jak Hello tooexecute **insertEntity** a **deleteEntity** operace v jedné žádosti:</span><span class="sxs-lookup"><span data-stu-id="d7584-190">hello following example shows how tooexecute **insertEntity** and **deleteEntity** operations in a single request:</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;
use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

// Create list of batch operation.
$operations = new BatchOperations();

$entity1 = new Entity();
$entity1->setPartitionKey("tasksSeattle");
$entity1->setRowKey("2");
$entity1->addProperty("Description", null, "Clean roof gutters.");
$entity1->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity1->addProperty("Location", EdmType::STRING, "Home");

// Add operation toolist of batch operations.
$operations->addInsertEntity("mytable", $entity1);

// Add operation toolist of batch operations.
$operations->addDeleteEntity("mytable", "tasksSeattle", "1");

try    {
    $tableRestProxy->batch($operations);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

<span data-ttu-id="d7584-191">Další informace o dávkování operace s tabulkou najdete v tématu [provádění transakcí skupiny Entity][entity-group-transactions].</span><span class="sxs-lookup"><span data-stu-id="d7584-191">For more information about batching Table operations, see [Performing Entity Group Transactions][entity-group-transactions].</span></span>

## <a name="delete-a-table"></a><span data-ttu-id="d7584-192">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="d7584-192">Delete a table</span></span>
<span data-ttu-id="d7584-193">Nakonec toodelete tabulce předat toohello název tabulky hello **TableRestProxy -> deleteTable** metoda.</span><span class="sxs-lookup"><span data-stu-id="d7584-193">Finally, toodelete a table, pass hello table name toohello **TableRestProxy->deleteTable** method.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Delete table.
    $tableRestProxy->deleteTable("mytable");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a><span data-ttu-id="d7584-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d7584-194">Next steps</span></span>
<span data-ttu-id="d7584-195">Teď, když jste se naučili základy hello hello služby Azure Table, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště.</span><span class="sxs-lookup"><span data-stu-id="d7584-195">Now that you've learned hello basics of hello Azure Table service, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="d7584-196">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="d7584-196">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

* <span data-ttu-id="d7584-197">[Středisko pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="d7584-197">[PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
