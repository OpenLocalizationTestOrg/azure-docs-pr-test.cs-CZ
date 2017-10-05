---
title: "Používání úložiště table z PHP | Microsoft Docs"
description: "Další informace o použití služby Table z PHP vytvářet a odstraňovat tabulky, vložení, odstranit a dotaz tabulku."
services: storage
documentationcenter: php
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 1e57f371-6208-4753-b2a0-05db4aede8e3
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 15d3216ef5bb1d7ff312bd886837a3a7b0335afd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-table-storage-from-php"></a><span data-ttu-id="3845f-103">Používání úložiště table z PHP</span><span class="sxs-lookup"><span data-stu-id="3845f-103">How to use table storage from PHP</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="3845f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3845f-104">Overview</span></span>
<span data-ttu-id="3845f-105">Tento průvodce vám ukáže, jak provádět běžné scénáře s využitím služby Azure Table.</span><span class="sxs-lookup"><span data-stu-id="3845f-105">This guide shows you how to perform common scenarios using the Azure Table service.</span></span> <span data-ttu-id="3845f-106">Ukázky jsou napsané v PHP a použití [Azure SDK pro jazyk PHP][download].</span><span class="sxs-lookup"><span data-stu-id="3845f-106">The samples are written in PHP and use the [Azure SDK for PHP][download].</span></span> <span data-ttu-id="3845f-107">Pokryté scénáře zahrnují **vytváření a odstraňování tabulek a vkládání, odstraňování a dotazování entity v tabulce**.</span><span class="sxs-lookup"><span data-stu-id="3845f-107">The scenarios covered include **creating and deleting a table, and inserting, deleting, and querying entities in a table**.</span></span> <span data-ttu-id="3845f-108">Další informace o službě Azure Table, najdete v článku [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="3845f-108">For more information on the Azure Table service, see the [Next steps](#next-steps) section.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="3845f-109">Vytvoření aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="3845f-109">Create a PHP application</span></span>
<span data-ttu-id="3845f-110">Jediný požadavek pro vytvoření aplikace PHP, který přistupuje k službě Azure Table je odkazování třídy v sadě Azure SDK pro jazyk PHP z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="3845f-110">The only requirement for creating a PHP application that accesses the Azure Table service is the referencing of classes in the Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="3845f-111">Všechny nástroje pro vývoj slouží k vytvoření aplikace, včetně Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="3845f-111">You can use any development tools to create your application, including Notepad.</span></span>

<span data-ttu-id="3845f-112">V této příručce používat funkce služby tabulky, které lze volat z v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.</span><span class="sxs-lookup"><span data-stu-id="3845f-112">In this guide, you use Table service features which can be called from within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-the-azure-client-libraries"></a><span data-ttu-id="3845f-113">Získat knihoven klienta Azure</span><span class="sxs-lookup"><span data-stu-id="3845f-113">Get the Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a><span data-ttu-id="3845f-114">Konfigurace aplikace pro přístup ke službě tabulky</span><span class="sxs-lookup"><span data-stu-id="3845f-114">Configure your application to access the Table service</span></span>
<span data-ttu-id="3845f-115">Pokud chcete používat službu Azure Table rozhraní API, budete muset:</span><span class="sxs-lookup"><span data-stu-id="3845f-115">To use the Azure Table service APIs, you need to:</span></span>

1. <span data-ttu-id="3845f-116">Reference souboru pomocí automatického zavaděče [require_once] [ require_once] příkaz, a</span><span class="sxs-lookup"><span data-stu-id="3845f-116">Reference the autoloader file using the [require_once][require_once] statement, and</span></span>
2. <span data-ttu-id="3845f-117">Referenční všechny třídy, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="3845f-117">Reference any classes you might use.</span></span>

<span data-ttu-id="3845f-118">Následující příklad ukazuje, jak se zahrnuje automatického zavaděče souboru a odkaz **ServicesBuilder** třídy.</span><span class="sxs-lookup"><span data-stu-id="3845f-118">The following example shows how to include the autoloader file and reference the **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="3845f-119">V příkladech v tomto článku předpokládá, že jste nainstalovali PHP klientské knihovny pro Azure prostřednictvím autora.</span><span class="sxs-lookup"><span data-stu-id="3845f-119">The examples in this article assume you have installed the PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="3845f-120">Pokud jste nainstalovali v knihovnách ručně, je třeba odkazovat <code>WindowsAzure.php</code> automatického zavaděče souboru.</span><span class="sxs-lookup"><span data-stu-id="3845f-120">If you installed the libraries manually, you need to reference the <code>WindowsAzure.php</code> autoloader file.</span></span>
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="3845f-121">V následujících příkladech `require_once` údajů se vždy zobrazí, ale jenom ty třídy potřebné pro tento příklad provést odkazují.</span><span class="sxs-lookup"><span data-stu-id="3845f-121">In the examples below, the `require_once` statement is always shown, but only the classes necessary for the example to execute are referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="3845f-122">Nastavit připojení k úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="3845f-122">Set up an Azure storage connection</span></span>
<span data-ttu-id="3845f-123">K vytvoření instance klienta služby Azure Table, musíte nejprve mít platný připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="3845f-123">To instantiate an Azure Table service client, you must first have a valid connection string.</span></span> <span data-ttu-id="3845f-124">Formát pro připojovací řetězec služby tabulky je:</span><span class="sxs-lookup"><span data-stu-id="3845f-124">The format for the Table service connection string is:</span></span>

<span data-ttu-id="3845f-125">Pro přístup k službě za provozu:</span><span class="sxs-lookup"><span data-stu-id="3845f-125">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="3845f-126">Pro přístup k emulátoru úložiště:</span><span class="sxs-lookup"><span data-stu-id="3845f-126">For accessing the emulator storage:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="3845f-127">Pokud chcete vytvořit libovolného klienta služby Azure, budete muset použít **ServicesBuilder** třídy.</span><span class="sxs-lookup"><span data-stu-id="3845f-127">To create any Azure service client, you need to use the **ServicesBuilder** class.</span></span> <span data-ttu-id="3845f-128">Můžete:</span><span class="sxs-lookup"><span data-stu-id="3845f-128">You can:</span></span>

* <span data-ttu-id="3845f-129">Připojovací řetězec přímo jí předat nebo</span><span class="sxs-lookup"><span data-stu-id="3845f-129">pass the connection string directly to it or</span></span>
* <span data-ttu-id="3845f-130">Použití **CloudConfigurationManager (CCM)** zkontrolujte několik externích zdrojů pro připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="3845f-130">use the **CloudConfigurationManager (CCM)** to check multiple external sources for the connection string:</span></span>
  * <span data-ttu-id="3845f-131">ve výchozím nastavení je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="3845f-131">by default, it comes with support for one external source - environmental variables</span></span>
  * <span data-ttu-id="3845f-132">můžete přidat nové zdroje tím, že rozšíří **ConnectionStringSource** – třída</span><span class="sxs-lookup"><span data-stu-id="3845f-132">you can add new sources by extending the **ConnectionStringSource** class</span></span>

<span data-ttu-id="3845f-133">Příklady podle zde uvedeného se předají připojovací řetězec, který přímo.</span><span class="sxs-lookup"><span data-stu-id="3845f-133">For the examples outlined here, the connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);
```

## <a name="create-a-table"></a><span data-ttu-id="3845f-134">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="3845f-134">Create a table</span></span>
<span data-ttu-id="3845f-135">A **TableRestProxy** objektu umožňuje vytvořit tabulku s **createTable** metoda.</span><span class="sxs-lookup"><span data-stu-id="3845f-135">A **TableRestProxy** object lets you create a table with the **createTable** method.</span></span> <span data-ttu-id="3845f-136">Při vytváření tabulky, můžete nastavit časový limit služby Table.</span><span class="sxs-lookup"><span data-stu-id="3845f-136">When creating a table, you can set the Table service timeout.</span></span> <span data-ttu-id="3845f-137">(Další informace o tabulce časový limit služby najdete v tématu [nastavení časových limitů pro operace služby s tabulkou][table-service-timeouts].)</span><span class="sxs-lookup"><span data-stu-id="3845f-137">(For more information about the Table service timeout, see [Setting Timeouts for Table Service Operations][table-service-timeouts].)</span></span>

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

<span data-ttu-id="3845f-138">Informace o omezeních na názvy tabulek najdete v tématu [Principy datového modelu služby Table][table-data-model].</span><span class="sxs-lookup"><span data-stu-id="3845f-138">For information about restrictions on table names, see [Understanding the Table Service Data Model][table-data-model].</span></span>

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="3845f-139">Přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="3845f-139">Add an entity to a table</span></span>
<span data-ttu-id="3845f-140">Pokud chcete přidat entitu do tabulky, vytvořte novou **Entity** objektu a předejte ji do **TableRestProxy -> insertEntity**.</span><span class="sxs-lookup"><span data-stu-id="3845f-140">To add an entity to a table, create a new **Entity** object and pass it to **TableRestProxy->insertEntity**.</span></span> <span data-ttu-id="3845f-141">Všimněte si, že při vytváření entity, musíte zadat `PartitionKey` a `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="3845f-141">Note that when you create an entity, you must specify a `PartitionKey` and `RowKey`.</span></span> <span data-ttu-id="3845f-142">Tyto jsou jedinečné identifikátory pro entitu a jsou hodnoty, které může být dotazován mnohem rychleji než ostatní vlastnosti entity.</span><span class="sxs-lookup"><span data-stu-id="3845f-142">These are the unique identifiers for an entity and are values that can be queried much faster than other entity properties.</span></span> <span data-ttu-id="3845f-143">Používá systém `PartitionKey` automaticky distribuovat entity v tabulce přes mnoho uzly úložiště.</span><span class="sxs-lookup"><span data-stu-id="3845f-143">The system uses `PartitionKey` to automatically distribute the table's entities over many storage nodes.</span></span> <span data-ttu-id="3845f-144">Entity se stejným `PartitionKey` jsou uložené na stejném uzlu.</span><span class="sxs-lookup"><span data-stu-id="3845f-144">Entities with the same `PartitionKey` are stored on the same node.</span></span> <span data-ttu-id="3845f-145">(Provedení operace u více entit, které jsou uložené ve stejném uzlu lepší, než na entity ukládat v rámci různých uzlech.) `RowKey` Je jedinečný Identifikátor entity v rámci oddílu.</span><span class="sxs-lookup"><span data-stu-id="3845f-145">(Operations on multiple entities stored on the same node perform better than on entities stored across different nodes.) The `RowKey` is the unique ID of an entity within a partition.</span></span>

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
$entity->addProperty("Description", null, "Take out the trash.");
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

<span data-ttu-id="3845f-146">Informace o vlastnosti tabulky a typy najdete v tématu [Principy datového modelu služby Table][table-data-model].</span><span class="sxs-lookup"><span data-stu-id="3845f-146">For information about Table properties and types, see [Understanding the Table Service Data Model][table-data-model].</span></span>

<span data-ttu-id="3845f-147">**TableRestProxy** třída nabízí dvě alternativní metody pro vložení entity: **insertOrMergeEntity** a **insertOrReplaceEntity**.</span><span class="sxs-lookup"><span data-stu-id="3845f-147">The **TableRestProxy** class offers two alternative methods for inserting entities: **insertOrMergeEntity** and **insertOrReplaceEntity**.</span></span> <span data-ttu-id="3845f-148">Pokud chcete použít tyto metody, vytvořte novou **Entity** a předejte ji jako parametr metody.</span><span class="sxs-lookup"><span data-stu-id="3845f-148">To use these methods, create a new **Entity** and pass it as a parameter to either method.</span></span> <span data-ttu-id="3845f-149">Pokud neexistuje, každá metoda vloží entitu.</span><span class="sxs-lookup"><span data-stu-id="3845f-149">Each method will insert the entity if it does not exist.</span></span> <span data-ttu-id="3845f-150">Pokud entita již existuje, **insertOrMergeEntity** aktualizace hodnot vlastností, pokud již existují vlastnosti a přidá nové vlastnosti Pokud neexistují, při **insertOrReplaceEntity** úplně nahradí stávající entity.</span><span class="sxs-lookup"><span data-stu-id="3845f-150">If the entity already exists, **insertOrMergeEntity** updates property values if the properties already exist and adds new properties if they do not exist, while **insertOrReplaceEntity** completely replaces an existing entity.</span></span> <span data-ttu-id="3845f-151">Následující příklad ukazuje, jak používat **insertOrMergeEntity**.</span><span class="sxs-lookup"><span data-stu-id="3845f-151">The following example shows how to use **insertOrMergeEntity**.</span></span> <span data-ttu-id="3845f-152">Pokud entita s `PartitionKey` "tasksSeattle" a `RowKey` "1" již neexistuje, bude možné vložit.</span><span class="sxs-lookup"><span data-stu-id="3845f-152">If the entity with `PartitionKey` "tasksSeattle" and `RowKey` "1" does not already exist, it will be inserted.</span></span> <span data-ttu-id="3845f-153">Ale pokud dříve vložení (jak je uvedeno v předchozím příkladu), `DueDate` vlastnost bude aktualizovat a `Status` přidá vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3845f-153">However, if it has previously been inserted (as shown in the example above), the `DueDate` property will be updated, and the `Status` property will be added.</span></span> <span data-ttu-id="3845f-154">`Description` a `Location` jsou aktualizovány vlastnosti, ale s hodnotami, které efektivně je ponechte beze změny.</span><span class="sxs-lookup"><span data-stu-id="3845f-154">The `Description` and `Location` properties are also updated, but with values that effectively leave them unchanged.</span></span> <span data-ttu-id="3845f-155">Pokud tyto dvě vlastnosti pozdější nebyly přidány, jak je znázorněno v příkladu, ale byly na cílovou entitu, jejich existující hodnoty by zůstanou beze změny.</span><span class="sxs-lookup"><span data-stu-id="3845f-155">If these latter two properties were not added as shown in the example, but existed on the target entity, their existing values would remain unchanged.</span></span>

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
$entity->addProperty("Description", null, "Take out the trash.");
$entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
$entity->addProperty("Location", EdmType::STRING, "Home");
$entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

try    {
    // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
    // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
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

## <a name="retrieve-a-single-entity"></a><span data-ttu-id="3845f-156">Načtení jedné entity</span><span class="sxs-lookup"><span data-stu-id="3845f-156">Retrieve a single entity</span></span>
<span data-ttu-id="3845f-157">**TableRestProxy -> getEntity** metoda umožňuje načíst jednu entitu pomocí dotazů na jeho `PartitionKey` a `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="3845f-157">The **TableRestProxy->getEntity** method allows you to retrieve a single entity by querying for its `PartitionKey` and `RowKey`.</span></span> <span data-ttu-id="3845f-158">V příkladu níže klíč oddílu `tasksSeattle` a klíč řádku `1` jsou předávány **getEntity** metoda.</span><span class="sxs-lookup"><span data-stu-id="3845f-158">In the example below, the partition key `tasksSeattle` and row key `1` are passed to the **getEntity** method.</span></span>

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

## <a name="retrieve-all-entities-in-a-partition"></a><span data-ttu-id="3845f-159">Načtení všech entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="3845f-159">Retrieve all entities in a partition</span></span>
<span data-ttu-id="3845f-160">Dotazy na entity se vytvářejí pomocí filtrů (Další informace najdete v tématu [dotazování tabulky a entity][filters]).</span><span class="sxs-lookup"><span data-stu-id="3845f-160">Entity queries are constructed using filters (for more information, see [Querying Tables and Entities][filters]).</span></span> <span data-ttu-id="3845f-161">Pro načtení všech entit v oddílu, použijte filtr "PartitionKey eq *název_oddílu*".</span><span class="sxs-lookup"><span data-stu-id="3845f-161">To retrieve all entities in partition, use the filter "PartitionKey eq *partition_name*".</span></span> <span data-ttu-id="3845f-162">Následující příklad ukazuje, jak načíst všechny entity v `tasksSeattle` oddílu pomocí filtru k předání **queryEntities** metoda.</span><span class="sxs-lookup"><span data-stu-id="3845f-162">The following example shows how to retrieve all entities in the `tasksSeattle` partition by passing a filter to the **queryEntities** method.</span></span>

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

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a><span data-ttu-id="3845f-163">Načtení podmnožinu entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="3845f-163">Retrieve a subset of entities in a partition</span></span>
<span data-ttu-id="3845f-164">Stejným způsobem používaným v předchozím příkladu lze použít k načtení všech podmnožinu entit v oddílu.</span><span class="sxs-lookup"><span data-stu-id="3845f-164">The same pattern used in the previous example can be used to retrieve any subset of entities in a partition.</span></span> <span data-ttu-id="3845f-165">Určuje podmnožinu entity načtete filtru můžete použít (Další informace najdete v tématu [dotazování tabulky a entity][filters]). Následující příklad ukazuje, jak použít filtr k načtení všech entit s konkrétním `Location` a `DueDate` nižší než zadané datum.</span><span class="sxs-lookup"><span data-stu-id="3845f-165">The subset of entities you retrieve are determined by the filter you use (for more information, see [Querying Tables and Entities][filters]).The following example shows how to use a filter to retrieve all entities with a specific `Location` and a `DueDate` less than a specified date.</span></span>

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

## <a name="retrieve-a-subset-of-entity-properties"></a><span data-ttu-id="3845f-166">Načtení podmnožinu vlastností entity</span><span class="sxs-lookup"><span data-stu-id="3845f-166">Retrieve a subset of entity properties</span></span>
<span data-ttu-id="3845f-167">Dotaz můžete načíst podmnožinu vlastností entity.</span><span class="sxs-lookup"><span data-stu-id="3845f-167">A query can retrieve a subset of entity properties.</span></span> <span data-ttu-id="3845f-168">Tento postup volá *projekce*, omezuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entity.</span><span class="sxs-lookup"><span data-stu-id="3845f-168">This technique, called *projection*, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="3845f-169">Pokud chcete nastavit vlastnost, která má být načtena, předat název vlastnosti, která má **dotazu -> addSelectField** metoda.</span><span class="sxs-lookup"><span data-stu-id="3845f-169">To specify a property to be retrieved, pass the name of the property to the **Query->addSelectField** method.</span></span> <span data-ttu-id="3845f-170">Tuto metodu můžete volat více než jednou. Chcete-li přidat další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3845f-170">You can call this method multiple times to add more properties.</span></span> <span data-ttu-id="3845f-171">Po provedení **TableRestProxy -> queryEntities**, vrácené entity budou mít pouze vybraných vlastností.</span><span class="sxs-lookup"><span data-stu-id="3845f-171">After executing **TableRestProxy->queryEntities**, the returned entities will only have the selected properties.</span></span> <span data-ttu-id="3845f-172">(V Pokud budete chtít vrátit podmnožinu entity tabulky, použijte filtr jak je znázorněno v dotazech výše.)</span><span class="sxs-lookup"><span data-stu-id="3845f-172">(If you want to return a subset of Table entities, use a filter as shown in the queries above.)</span></span>

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

// All entities in the table are returned, regardless of whether
// they have the Description field.
// To limit the results returned, use a filter.
$entities = $result->getEntities();

foreach($entities as $entity){
    $description = $entity->getProperty("Description")->getValue();
    echo $description."<br />";
}
```

## <a name="update-an-entity"></a><span data-ttu-id="3845f-173">Aktualizace entity</span><span class="sxs-lookup"><span data-stu-id="3845f-173">Update an entity</span></span>
<span data-ttu-id="3845f-174">Stávající entitu můžete aktualizovat pomocí **Entity -> SetProperty –** a **Entity -> AddProperty –** metody pro entitu a pak volání **TableRestProxy -> updateEntity**.</span><span class="sxs-lookup"><span data-stu-id="3845f-174">An existing entity can be updated by using the **Entity->setProperty** and **Entity->addProperty** methods on the entity, and then calling **TableRestProxy->updateEntity**.</span></span> <span data-ttu-id="3845f-175">Následující příklad načte entitu, upraví jednu vlastnost, odebere jinou vlastnost a přidá novou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3845f-175">The following example retrieves an entity, modifies one property, removes another property, and adds a new property.</span></span> <span data-ttu-id="3845f-176">Všimněte si, že vlastnost můžete odebrat tak nastavení její hodnoty na **null**.</span><span class="sxs-lookup"><span data-stu-id="3845f-176">Note that you can remove a property by setting its value to **null**.</span></span>

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

## <a name="delete-an-entity"></a><span data-ttu-id="3845f-177">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="3845f-177">Delete an entity</span></span>
<span data-ttu-id="3845f-178">Pokud chcete odstranit entitu, předat název tabulky a entity `PartitionKey` a `RowKey` k **TableRestProxy -> deleteEntity** metoda.</span><span class="sxs-lookup"><span data-stu-id="3845f-178">To delete an entity, pass the table name, and the entity's `PartitionKey` and `RowKey` to the **TableRestProxy->deleteEntity** method.</span></span>

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

<span data-ttu-id="3845f-179">Všimněte si, že pro kontrolách souběžnosti, můžete nastavit značku Etag pro entitu k odstranění pomocí **DeleteEntityOptions -> setEtag** metoda a předávání **DeleteEntityOptions** do objektu  **deleteEntity** jako čtvrtý parametr.</span><span class="sxs-lookup"><span data-stu-id="3845f-179">Note that for concurrency checks, you can set the Etag for an entity to be deleted by using the **DeleteEntityOptions->setEtag** method and passing the **DeleteEntityOptions** object to **deleteEntity** as a fourth parameter.</span></span>

## <a name="batch-table-operations"></a><span data-ttu-id="3845f-180">Operace s tabulkou batch</span><span class="sxs-lookup"><span data-stu-id="3845f-180">Batch table operations</span></span>
<span data-ttu-id="3845f-181">**TableRestProxy -> batch** metoda umožňuje spuštění více operací v jedné žádosti.</span><span class="sxs-lookup"><span data-stu-id="3845f-181">The **TableRestProxy->batch** method allows you to execute multiple operations in a single request.</span></span> <span data-ttu-id="3845f-182">Vzor zde vyžaduje přidání operací do **BatchRequest** objekt a následné předání **BatchRequest** do objektu **TableRestProxy -> batch** metoda.</span><span class="sxs-lookup"><span data-stu-id="3845f-182">The pattern here involves adding operations to **BatchRequest** object and then passing the **BatchRequest** object to the **TableRestProxy->batch** method.</span></span> <span data-ttu-id="3845f-183">Chcete-li přidat operace **BatchRequest** objektu, žádný z následujících metod můžete volat vícekrát:</span><span class="sxs-lookup"><span data-stu-id="3845f-183">To add an operation to a **BatchRequest** object, you can call any of the following methods multiple times:</span></span>

* <span data-ttu-id="3845f-184">**addInsertEntity** (přidá insertEntity operace)</span><span class="sxs-lookup"><span data-stu-id="3845f-184">**addInsertEntity** (adds an insertEntity operation)</span></span>
* <span data-ttu-id="3845f-185">**addUpdateEntity** (přidá updateEntity operace)</span><span class="sxs-lookup"><span data-stu-id="3845f-185">**addUpdateEntity** (adds an updateEntity operation)</span></span>
* <span data-ttu-id="3845f-186">**addMergeEntity** (přidá mergeEntity operace)</span><span class="sxs-lookup"><span data-stu-id="3845f-186">**addMergeEntity** (adds a mergeEntity operation)</span></span>
* <span data-ttu-id="3845f-187">**addInsertOrReplaceEntity** (přidá insertOrReplaceEntity operace)</span><span class="sxs-lookup"><span data-stu-id="3845f-187">**addInsertOrReplaceEntity** (adds an insertOrReplaceEntity operation)</span></span>
* <span data-ttu-id="3845f-188">**addInsertOrMergeEntity** (přidá insertOrMergeEntity operace)</span><span class="sxs-lookup"><span data-stu-id="3845f-188">**addInsertOrMergeEntity** (adds an insertOrMergeEntity operation)</span></span>
* <span data-ttu-id="3845f-189">**addDeleteEntity** (přidá deleteEntity operace)</span><span class="sxs-lookup"><span data-stu-id="3845f-189">**addDeleteEntity** (adds a deleteEntity operation)</span></span>

<span data-ttu-id="3845f-190">Následující příklad ukazuje, jak provádět **insertEntity** a **deleteEntity** operace v jedné žádosti:</span><span class="sxs-lookup"><span data-stu-id="3845f-190">The following example shows how to execute **insertEntity** and **deleteEntity** operations in a single request:</span></span>

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

// Add operation to list of batch operations.
$operations->addInsertEntity("mytable", $entity1);

// Add operation to list of batch operations.
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

<span data-ttu-id="3845f-191">Další informace o dávkování operace s tabulkou najdete v tématu [provádění transakcí skupiny Entity][entity-group-transactions].</span><span class="sxs-lookup"><span data-stu-id="3845f-191">For more information about batching Table operations, see [Performing Entity Group Transactions][entity-group-transactions].</span></span>

## <a name="delete-a-table"></a><span data-ttu-id="3845f-192">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="3845f-192">Delete a table</span></span>
<span data-ttu-id="3845f-193">Nakonec pokud chcete odstranit tabulku, předat název tabulky k **TableRestProxy -> deleteTable** metoda.</span><span class="sxs-lookup"><span data-stu-id="3845f-193">Finally, to delete a table, pass the table name to the **TableRestProxy->deleteTable** method.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="3845f-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3845f-194">Next steps</span></span>
<span data-ttu-id="3845f-195">Teď, když jste se naučili základy služby Azure Table, postupujte podle následujících odkazech na další informace o složitějších úlohách úložiště.</span><span class="sxs-lookup"><span data-stu-id="3845f-195">Now that you've learned the basics of the Azure Table service, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="3845f-196">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.</span><span class="sxs-lookup"><span data-stu-id="3845f-196">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

* <span data-ttu-id="3845f-197">[Středisko pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="3845f-197">[PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
