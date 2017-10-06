---
title: "aaaHow toouse úložiště table z PHP | Microsoft Docs"
description: "Zjistěte, jak toouse hello služby Table z PHP toocreate a odstranění tabulky, vložení, odstranění a tabulku hello dotazů."
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
ms.openlocfilehash: 1e1036118e208280b4c205da7d7eea61e79359c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-php"></a>Jak toouse tabulky úložiště z PHP
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Table. Hello ukázky jsou napsané v jazyce PHP a používají hello [Azure SDK pro jazyk PHP][download]. Hello pokryté scénáře zahrnují **vytváření a odstraňování tabulek a vkládání, odstraňování a dotazování entity v tabulce**. Další informace o hello služby Azure Table, najdete v části hello [další kroky](#next-steps) části.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Vytvoření aplikace PHP
Hello jen požadavek pro vytvoření aplikace PHP, který přistupuje k službě Azure Table hello je hello odkazování tříd v hello Azure SDK pro jazyk PHP z vašeho kódu. Můžete použít všechny toocreate nástroje pro vývoj aplikace, včetně Poznámkový blok.

V této příručce používat funkce služby tabulky, které lze volat z v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.

## <a name="get-hello-azure-client-libraries"></a>Získání knihovny klienta Azure hello
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-hello-table-service"></a>Konfigurace služby Table hello tooaccess vaší aplikace
toouse hello Azure Table API služby, musíte:

1. Referenční dokumentace hello automatického zavaděče soubor pomocí hello [require_once] [ require_once] příkaz, a
2. Referenční všechny třídy, které můžete použít.

Hello následující příklad ukazuje, jak tooinclude hello automatického zavaděče souboru a odkaz hello **ServicesBuilder** třídy.

> [!NOTE]
> Hello příklady v tomto článku předpokládá, že jste nainstalovali hello PHP klientské knihovny pro Azure prostřednictvím autora. Pokud jste nainstalovali hello knihovny ručně, je třeba tooreference hello <code>WindowsAzure.php</code> automatického zavaděče souboru.
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

V následujících příkladech hello, hello `require_once` údajů se vždy zobrazí, ale pouze hello třídy potřebné pro tooexecute příklad hello odkazují.

## <a name="set-up-an-azure-storage-connection"></a>Nastavit připojení k úložišti Azure
tooinstantiate klientem služby Azure Table, že máte platný připojovací řetězec. Formát Hello hello připojovací řetězec služby tabulky je:

Pro přístup k službě za provozu:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Pro přístup k hello emulátor úložiště:

```php
UseDevelopmentStorage=true
```

toocreate libovolného klienta služby Azure, budete potřebovat toouse hello **ServicesBuilder** třídy. Můžete:

* Předat hello připojovací řetězec přímo tooit nebo
* Použití hello **CloudConfigurationManager (CCM)** toocheck více externí zdroje pro hello připojovací řetězec:
  * ve výchozím nastavení je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí
  * můžete přidat nové zdroje rozšířením hello **ConnectionStringSource** – třída

Příklady hello podle zde uvedeného se předají hello připojovací řetězec, který přímo.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);
```

## <a name="create-a-table"></a>Vytvoření tabulky
A **TableRestProxy** objektu umožňuje vytvořit tabulku s hello **createTable** metoda. Při vytváření tabulky, můžete nastavit časový limit služby hello tabulky. (Další informace o časový limit služby Table hello najdete v tématu [nastavení časových limitů pro operace služby s tabulkou][table-service-timeouts].)

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

Informace o omezeních na názvy tabulek najdete v tématu [hello Principy datového modelu služby Table][table-data-model].

## <a name="add-an-entity-tooa-table"></a>Přidat tooa tabulka entity
tooadd tooa tabulka entity, vytvořte novou **Entity** objektu a předejte ji příliš**TableRestProxy -> insertEntity**. Všimněte si, že při vytváření entity, musíte zadat `PartitionKey` a `RowKey`. Tyto jsou hello jedinečné identifikátory pro entitu a jsou hodnoty, které může být dotazován mnohem rychleji než ostatní vlastnosti entity. systém Hello používá `PartitionKey` tooautomatically distribuovat entity tabulky hello přes mnoho uzly úložiště. Entity s hello stejné `PartitionKey` jsou uložené na hello stejného uzlu. (Operace u více entit, které jsou uložené na hello stejného uzlu provádět lepší, než na entity ukládat v rámci různých uzlech.) Hello `RowKey` je hello jedinečné ID entity v rámci oddílu.

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

Informace o vlastnosti tabulky a typy najdete v tématu [hello Principy datového modelu služby Table][table-data-model].

Hello **TableRestProxy** třída nabízí dvě alternativní metody pro vložení entity: **insertOrMergeEntity** a **insertOrReplaceEntity**. toouse těchto metod, vytvořte novou **Entity** a předáváme jako parametr metody tooeither. Každá metoda budou vkládat hello entity, pokud neexistuje. Pokud hello entity už existuje, **insertOrMergeEntity** aktualizace hodnot vlastností, pokud hello vlastnosti již existují a přidá nové vlastnosti Pokud neexistují, při **insertOrReplaceEntity** úplně nahradí stávající entity. Následující příklad ukazuje, jak Hello toouse **insertOrMergeEntity**. Pokud hello entita s `PartitionKey` "tasksSeattle" a `RowKey` "1" již neexistuje, bude možné vložit. Ale pokud dříve vložení (jak je uvedeno v předchozím příkladu hello), hello `DueDate` vlastnost budou aktualizovány a hello `Status` přidá vlastnost. Hello `Description` a `Location` jsou aktualizovány vlastnosti, ale s hodnotami, které efektivně je ponechte beze změny. Pokud tyto dvě vlastnosti pozdější nebyly přidány jako v příkladu hello, ale byly na cílovou entitu hello, jejich existující hodnoty by zůstanou beze změny.

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

## <a name="retrieve-a-single-entity"></a>Načtení jedné entity
Hello **TableRestProxy -> getEntity** metoda vám umožní tooretrieve jednu entitu pomocí dotazů na jeho `PartitionKey` a `RowKey`. V příkladu hello níže hello klíč oddílu `tasksSeattle` a klíč řádku `1` se předávají toohello **getEntity** metoda.

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

## <a name="retrieve-all-entities-in-a-partition"></a>Načtení všech entit v oddílu
Dotazy na entity se vytvářejí pomocí filtrů (Další informace najdete v tématu [dotazování tabulky a entity][filters]). tooretrieve všechny entity v oddílu, použijte filtr hello "PartitionKey eq *název_oddílu*". Následující příklad ukazuje, jak Hello tooretrieve všechny entity v hello `tasksSeattle` oddílu předáním filtru toohello **queryEntities** metoda.

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

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Načtení podmnožinu entit v oddílu
Hello stejným způsobem používaným v předchozím příkladu hello lze použít tooretrieve žádné podmnožinu entit v oddílu. Určuje podmnožinu Hello entit načtete hello filtru můžete použít (Další informace najdete v tématu [dotazování tabulky a entity][filters]) .hello následující příklad ukazuje, jak toouse tooretrieve filtru všechny entity s konkrétním `Location` a `DueDate` nižší než zadané datum.

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

## <a name="retrieve-a-subset-of-entity-properties"></a>Načtení podmnožinu vlastností entity
Dotaz můžete načíst podmnožinu vlastností entity. Tento postup volá *projekce*, omezuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entity. načíst vlastnost toobe toospecify, předat název hello hello vlastnost toohello **dotazu -> addSelectField** metoda. Tato metoda několikrát tooadd můžete volat další vlastnosti. Po provedení **TableRestProxy -> queryEntities**, hello vrátil entity budou mít pouze hello vybrané vlastnosti. (V Pokud chcete tooreturn podmnožinu entity tabulky, použijte filtr jak je znázorněno v dotazech hello výše.)

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

## <a name="update-an-entity"></a>Aktualizace entity
Stávající entitu můžete aktualizovat pomocí hello **Entity -> setProperty** a **Entity -> AddProperty –** v hello entitu a pak volání metody **TableRestProxy -> updateEntity** . Hello následující příklad načte entitu, upraví jednu vlastnost, odebere jinou vlastnost a přidá novou vlastnost. Všimněte si, můžete odebrat vlastnost tak, že nastavení její hodnoty příliš**null**.

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

## <a name="delete-an-entity"></a>Odstranění entity
toodelete entity, předat hello název tabulky a hello entity `PartitionKey` a `RowKey` toohello **TableRestProxy -> deleteEntity** metoda.

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

Všimněte si můžete nastavit pro kontrolách souběžnosti hello Etag pro entity toobe odstranit pomocí hello **DeleteEntityOptions -> setEtag** metoda a předávání hello **DeleteEntityOptions** objektu příliš**deleteEntity** jako čtvrtý parametr.

## <a name="batch-table-operations"></a>Operace s tabulkou batch
Hello **TableRestProxy -> batch** metoda vám umožní tooexecute více operací v jedné žádosti. Hello vzor zde zahrnuje přidávání operací příliš**BatchRequest** objekt a následné předání hello **BatchRequest** objektu toohello **TableRestProxy -> batch** metoda. tooadd operaci tooa **BatchRequest** objekt, můžete některé z hello následující metody několikrát volat:

* **addInsertEntity** (přidá insertEntity operace)
* **addUpdateEntity** (přidá updateEntity operace)
* **addMergeEntity** (přidá mergeEntity operace)
* **addInsertOrReplaceEntity** (přidá insertOrReplaceEntity operace)
* **addInsertOrMergeEntity** (přidá insertOrMergeEntity operace)
* **addDeleteEntity** (přidá deleteEntity operace)

Následující příklad ukazuje, jak Hello tooexecute **insertEntity** a **deleteEntity** operace v jedné žádosti:

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

Další informace o dávkování operace s tabulkou najdete v tématu [provádění transakcí skupiny Entity][entity-group-transactions].

## <a name="delete-a-table"></a>Odstranění tabulky
Nakonec toodelete tabulce předat toohello název tabulky hello **TableRestProxy -> deleteTable** metoda.

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

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello hello služby Azure Table, postupujte podle těchto odkazů toolearn o složitějších úlohách úložiště.

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.

* [Středisko pro vývojáře PHP](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
