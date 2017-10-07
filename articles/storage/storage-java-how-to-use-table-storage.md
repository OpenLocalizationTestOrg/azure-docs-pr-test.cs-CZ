---
title: "aaaHow toouse úložiště Table z Javy | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu hello pomocí Azure Table storage, úložiště dat typu NoSQL."
services: storage
documentationcenter: java
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 45145189-e67f-4ca6-b15d-43af7bfd3f97
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: f72cac3fc10cf0aef74780b84c515d93d715d787
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-java"></a>Jak toouse úložiště Table z Javy
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Table storage. Hello ukázky jsou napsané v jazyce Java a používají hello [sada SDK úložiště Azure pro jazyk Java][Azure Storage SDK for Java]. Hello pokryté scénáře zahrnují **vytváření**, **výpis**, a **odstraňování** tabulky, a také **vkládání**,  **dotazování**, **úprava**, a **odstraňování** entity v tabulce. Další informace o tabulkách najdete v tématu hello [další kroky](#Next-Steps) části.

Poznámka: Sada SDK je k dispozici pro vývojáře, kteří na zařízení se systémem Android používají Azure Storage. Další informace najdete v tématu hello [sada SDK úložiště Azure pro Android][Azure Storage SDK for Android].

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Vytvoření aplikace Java
V této příručce bude používat funkce úložiště, které lze spustit v rámci aplikace Java místně nebo v kódu běžící v rámci webovou roli nebo role pracovního procesu v Azure.

toodo tedy budete potřebovat tooinstall hello Java Development Kit (JDK) a vytvořit účet úložiště Azure ve vašem předplatném Azure. Jakmile provedete, budete potřebovat tooverify, který váš vývojový systém splňuje minimální požadavky hello a závislosti, které jsou uvedeny v hello [sada SDK úložiště Azure pro jazyk Java] [ Azure Storage SDK for Java] úložišti na Githubu. Pokud váš systém splňuje tyto požadavky, můžete postupovat podle pokynů hello ke stažení a instalaci hello knihovny úložiště Azure pro jazyk Java systému z tohoto úložiště. Po dokončení těchto úloh, bude možné toocreate aplikaci Java, která používá hello příklady v tomto článku.

## <a name="configure-your-application-tooaccess-table-storage"></a>Konfigurace úložiště table tooaccess vaší aplikace
Přidejte následující import příkazy toohello horní části souboru Java hello místo toouse Microsoft Azure storage rozhraní API tooaccess tabulky hello:

```java
// Include hello following imports toouse table APIs
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.table.*;
import com.microsoft.azure.storage.table.TableQuery.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Nastavit připojovací řetězec úložiště Azure
Klienta Azure storage používá úložiště připojovací řetězec toostore koncových bodů a přihlašovací údaje pro přístup ke službám dat správy. Když spustíte v aplikaci klienta, musíte zadat připojovací řetězec úložiště hello v hello formátu, pomocí hello názvu účtu úložiště a hello primární přístupový klíč pro účet úložiště hello uvedené v hello [portál Azure](https://portal.azure.com)pro hello *AccountName* a *AccountKey* hodnoty. Tento příklad ukazuje, jak můžou deklarovat statické pole toohold hello připojovací řetězec:

```java
// Define hello connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

Ve spuštěné aplikace v rámci role ve službě Microsoft Azure, můžete tento řetězec uložené v konfiguračním souboru služby hello, *souboru ServiceConfiguration.cscfg*a je přístupný pomocí volání toohello  **RoleEnvironment.getConfigurationSettings** metoda. Tady je příklad získávání hello připojovacího řetězce z **nastavení** element s názvem *StorageConnectionString* v konfiguračním souboru služby hello:

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

Hello následující ukázky předpokládejme použít jednu z těchto dvou metod tooget hello úložiště připojovací řetězec.

## <a name="how-to-create-a-table"></a>Postupy: vytvoření tabulky
A **CloudTableClient** objektu umožňuje získat odkaz na objekty pro tabulky a entity. Hello následující kód vytvoří **CloudTableClient** objektu a používá je toocreate nový **CloudTable** objekt, který reprezentuje tabulku s názvem "uživatelé". (Poznámka: existují další způsoby toocreate **CloudStorageAccount** objekty; Další informace najdete v tématu **CloudStorageAccount** v hello [Azure úložiště klienta SDK Reference].)

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create hello table if it doesn't exist.
    String tableName = "people";
    CloudTable cloudTable = tableClient.getTableReference(tableName);
    cloudTable.createIfNotExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-hello-tables"></a>Postupy: seznam tabulek hello
tooget seznam tabulek, volání hello **CloudTableClient.listTables()** metoda tooretrieve iterable seznam názvů tabulek.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Loop through hello collection of table names.
    for (String table : tableClient.listTables())
    {
        // Output each table name.
        System.out.println(table);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-an-entity-tooa-table"></a>Postupy: Přidání tooa tabulka entity
Entity se mapují tooJava objektů s použitím vlastní třída implementace **TableEntity**. Pro usnadnění práce hello **TableServiceEntity** třída implementuje **TableEntity** a používá reflexe toomap vlastnosti s názvem toogetter a setter metody pro hello vlastnosti. tooadd tooa tabulka entity, nejdřív vytvořte třídu, která definuje hello vlastnosti vaší entity. Hello následující kód definuje třídu entity, která používá jméno hello zákazníka jako klíč řádku hello a příjmení jako klíč oddílu hello. Společně oddílu a klíč řádku jednoznačně hello entity v tabulce hello. Entity se stejným klíčem oddílu můžete zadat dotaz rychleji než ty, které se různé klíče oddílů hello.

```java
public class CustomerEntity extends TableServiceEntity {
    public CustomerEntity(String lastName, String firstName) {
        this.partitionKey = lastName;
        this.rowKey = firstName;
    }

    public CustomerEntity() { }

    String email;
    String phoneNumber;

    public String getEmail() {
        return this.email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPhoneNumber() {
        return this.phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }
}
```

Operace s tabulkou zahrnující entity vyžadují **TableOperation** objektu. Tento objekt definuje toobe hello operaci provést u entity, které lze spustit s **CloudTable** objektu. Hello následující kód vytvoří novou instanci třídy hello **CustomerEntity** se některé toobe dat zákazníka uložené. Hello kód zavolá metodu Další **TableOperation.insertOrReplace** toocreate **TableOperation** objektu tooinsert entity do tabulky a partnerů hello nové **CustomerEntity**s ním. Nakonec hello kód volá hello **provést** metodu hello **CloudTable** objekt, zadáte hello "lidé" tabulky a hello nové **TableOperation**, které pak odešle žádosti o toohello úložiště služby tooinsert hello novou entitu zákazníka do tabulky "lidé" hello, nebo nahrazení hello entity, pokud již existuje.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.setEmail("Walter@contoso.com");
    customer1.setPhoneNumber("425-555-0101");

    // Create an operation tooadd hello new customer toohello people table.
    TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

    // Submit hello operation toohello table service.
    cloudTable.execute(insertCustomer1);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-insert-a-batch-of-entities"></a>Postupy: vložení dávky entit
Můžete vložit dávku entit toohello tabulka služby v rámci jedné operace zápisu. Hello následující kód vytvoří **TableBatchOperation** objekt a potom přidá tři vložit tooit operace. Každé operace insert přidá se při vytvoření nového objektu entity, nastavení jeho hodnoty a potom volání hello **vložit** metodu hello **TableBatchOperation** objekt entity hello tooassociate s novou Vložte operaci. Potom hello kód zavolá metodu **provést** na hello **CloudTable** objekt, zadáte hello "lidé" tabulky a hello **TableBatchOperation** objekt, který odesílá hello batch tabulky operace toohello úložiště služby v jedné žádosti.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Define a batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a customer entity tooadd toohello table.
    CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
    customer.setEmail("Jeff@contoso.com");
    customer.setPhoneNumber("425-555-0104");
    batchOperation.insertOrReplace(customer);

    // Create another customer entity tooadd toohello table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.setEmail("Ben@contoso.com");
    customer2.setPhoneNumber("425-555-0102");
    batchOperation.insertOrReplace(customer2);

    // Create a third customer entity tooadd toohello table.
    CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
    customer3.setEmail("Denise@contoso.com");
    customer3.setPhoneNumber("425-555-0103");
    batchOperation.insertOrReplace(customer3);

    // Execute hello batch of operations on hello "people" table.
    cloudTable.execute(batchOperation);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

Některé věci toonote ohledně dávkových operací:

* Můžete provést až too100 vložení, odstranění, sloučení, nahraďte, insert nebo merge a vložit nebo nahrazovat operace v libovolné kombinace v jedné dávce.
* Dávková operace může mít operaci načtení, pokud je hello jediná operace v dávce hello.
* Všechny entity v jedné dávkové operaci musí mít hello stejným klíčem oddílu.
* Dávkové operace je omezená tooa 4MB datovou částí.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>Postupy: načtení všech entit v oddílu
tooquery tabulku pro entity v oddílu, můžete použít **TableQuery**. Volání **TableQuery.from** toocreate dotazu pro konkrétní tabulku, která vrátí hodnotu typu určeného výsledku. Hello následujícího kódu určuje filtr pro entity, kde je: Váša' klíč oddílu hello. **TableQuery.generateFilterCondition** je metoda helper toocreate filtry pro dotazy. Volání **kde** na odkaz hello vrácený hello **TableQuery.from** metoda tooapply hello filtru toohello dotazu. Když hello se spustí dotaz pomocí volání příliš**provést** na hello **CloudTable** objektu se vrátí **Iterator** s hello **CustomerEntity**způsobit zadaný typ. Pak můžete použít hello **Iterator** , vrátí se v pro každou smyčku tooconsume hello výsledky. Tento kód zobrazí pole každé entity v konzole toohello výsledky dotazu hello hello.

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where hello partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Specify a partition query, using "Smith" as hello partition key filter.
    TableQuery<CustomerEntity> partitionQuery =
        TableQuery.from(CustomerEntity.class)
        .where(partitionFilter);

    // Loop through hello results, displaying information about hello entity.
    for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>Postupy: načtení rozsahu entit v oddílu
Pokud nechcete, aby tooquery všechny hello entit v oddílu, můžete zadat rozsah pomocí relačních operátorů ve filtru. Následující kód kombinuje dva filtry tooget všechny entity v oddílu "Smith" kde klíč řádku hello (jméno) začíná písmenem až too'E Hello' v hello abecedy. Potom zobrazí výsledky dotazu hello. Pokud používáte hello entity added toohello tabulky v dávce hello vložení této příručce, pouze dvě entity, jsou vráceny tentokrát (Ben a Karel Smith); Jeff Smith není zahrnutý.

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where hello partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Create a filter condition where hello row key is less than hello letter "E".
    String rowFilter = TableQuery.generateFilterCondition(
        ROW_KEY,
        QueryComparisons.LESS_THAN,
        "E");

    // Combine hello two conditions into a filter expression.
    String combinedFilter = TableQuery.combineFilters(partitionFilter,
        Operators.AND, rowFilter);

    // Specify a range query, using "Smith" as hello partition key,
    // with hello row key being up toohello letter "E".
    TableQuery<CustomerEntity> rangeQuery =
        TableQuery.from(CustomerEntity.class)
        .where(combinedFilter);

    // Loop through hello results, displaying information about hello entity
    for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-retrieve-a-single-entity"></a>Postupy: načtení jedné entity
Můžete napsat dotaz tooretrieve jedné konkrétní entity. Hello následující kód volání **TableOperation.retrieve** s oddílu klíč a řádek parametrů klíče toospecify hello zákazníka "Jeff Smith", místo vytvoření **TableQuery** a pomocí filtrů toodo hello samé. Po provedení hello načíst operace vrátí pouze jednu entitu, nikoli kolekce. Hello **getResultAsType** metoda vrhá hello výsledný typ toohello hello přiřazení cíl, **CustomerEntity** objektu. Pokud tento typ není kompatibilní s typem hello pro hello dotaz byl zadán, bude vyvolána výjimka. Pokud žádná entita má přesný klíč oddílu a řádku shodovat, je vrácena hodnota null. Určení klíče oddílu a řádku v dotazu je hello nejrychlejší způsob, jak tooretrieve jednu entitu ze služby Table hello.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve hello entity with partition key of "Smith" and row key of "Jeff"
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit hello operation toohello table service and get hello specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Output hello entity.
    if (specificEntity != null)
    {
        System.out.println(specificEntity.getPartitionKey() +
            " " + specificEntity.getRowKey() +
            "\t" + specificEntity.getEmail() +
            "\t" + specificEntity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-modify-an-entity"></a>Postupy: Úprava entity
toomodify entity, načtěte ji ze služby table hello, proveďte objekt entity toohello změny a uložte změny hello služby table back toohello s operaci nahrazení nebo merge. Hello následující kód změní telefonní číslo stávajícího zákazníka. Místo volání **TableOperation.insert** jako jsme provedli tooinsert, tento kód zavolá **TableOperation.replace**. Hello **CloudTable.execute** metoda volá služby table hello a hello entity je nahrazena, pokud jiná aplikace změnil v čase hello vzhledem k tomu, že jejím načtení této aplikace. Pokud k tomu dojde, je vyvolána výjimka, a hello entit musí být načteny, upravit a uložit znovu. Tento vzor opakování optimistickou metodu souběžného je běžné v systém distribuovaného úložiště.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve hello entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit hello operation toohello table service and get hello specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Specify a new phone number.
    specificEntity.setPhoneNumber("425-555-0105");

    // Create an operation tooreplace hello entity.
    TableOperation replaceEntity = TableOperation.replace(specificEntity);

    // Submit hello operation toohello table service.
    cloudTable.execute(replaceEntity);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-query-a-subset-of-entity-properties"></a>Postupy: dotaz na podmnožinu vlastností entity
Tabulku tooa dotazu můžete načíst jenom několik z entity. Tato technika, které se říká projekce, snižuje šířku pásma a může zlepšit výkon dotazů, zejména u velkých entit. Hello dotaz v hello následující kód používá hello **vyberte** metoda tooreturn pouze hello e-mailové adresy entit v tabulce hello. Hello výsledky jsou promítnout do kolekce **řetězec** pomoci hello **EntityResolver**, který hello převod typů na hello entity, kterou vrátil hello server. Další informace o projekce v [tabulky Azure: Představení funkcí Upsert a projekce dotazu][Azure Tables: Introducing Upsert and Query Projection]. Všimněte si, že projekci nepodporuje emulátor místního úložiště hello, proto tento kód spustí pouze v případě, že se pomocí účtu ve službě table hello.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Define a projection query that retrieves only hello Email property
    TableQuery<CustomerEntity> projectionQuery =
        TableQuery.from(CustomerEntity.class)
        .select(new String[] {"Email"});

    // Define a Entity resolver tooproject hello entity toohello Email value.
    EntityResolver<String> emailResolver = new EntityResolver<String>() {
        @Override
        public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
            return properties.get("Email").getValueAsString();
        }
    };

    // Loop through hello results, displaying hello Email values.
    for (String projectedString :
        cloudTable.execute(projectionQuery, emailResolver)) {
            System.out.println(projectedString);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-insert-or-replace-an-entity"></a>Postupy: vložení nebo nahrazení entity
Často se má tooadd tooa tabulka entity aniž by věděly, pokud již existuje v tabulce hello. Operace vložení nebo nahrazení umožňuje toomake jedné žádosti, která budou vkládat hello entity, pokud ji neexistuje nebo nahradit stávající jeden, pokud k tomu hello. Sestavování na předchozí příklady, hello následující kód vložení nebo nahrazení hello entity pro "Walter tuleňů grónských". Po vytvoření nové entity, tento kód zavolá hello **TableOperation.insertOrReplace** metoda. Tento kód pak zavolá **provést** na hello **CloudTable** objektu s hello tabulky a hello vložení nebo nahrazení operace tabulka jako parametry hello. tooupdate pouze část entity, hello **TableOperation.insertOrMerge** metodu je možné použít místo. Všimněte si, že, tento kód spustí pouze v případě, že se pomocí účtu ve službě table hello nepodporuje emulátor místního úložiště hello, že vložení nebo nahrazení. Můžete další informace o vložení nebo nahrazení a vložení nebo sloučení v tomto [tabulky Azure: Představení funkcí Upsert a projekce dotazu][Azure Tables: Introducing Upsert and Query Projection].

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
    customer5.setEmail("Walter@contoso.com");
    customer5.setPhoneNumber("425-555-0106");

    // Create an operation tooadd hello new customer toohello people table.
    TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

    // Submit hello operation toohello table service.
    cloudTable.execute(insertCustomer5);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-an-entity"></a>Postupy: Odstranění entity
Entitu můžete snadno odstranit po jejím načtení. Jakmile je načíst hello entity, volání **TableOperation.delete** s hello entity toodelete. Potom zavolejte **provést** na hello **CloudTable** objektu. Hello následující kód načte a odstraní entitu zákazníka.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create an operation tooretrieve hello entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Retrieve hello entity with partition key of "Smith" and row key of "Jeff".
    CustomerEntity entitySmithJeff =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Create an operation toodelete hello entity.
    TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

    // Submit hello delete operation toohello table service.
    cloudTable.execute(deleteSmithJeff);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-table"></a>Postupy: odstranění tabulky
Nakonec hello následující kód odstraní tabulku z účtu úložiště. Tabulka, která byla odstraněna budou k dispozici toobe znovu vytvoří dobu následující hello odstranění, obvykle menší než 40 sekund.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Delete hello table and all its data if it exists.
    CloudTable cloudTable = tableClient.getTableReference("people");
    cloudTable.deleteIfExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```
[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="next-steps"></a>Další kroky

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.
* [Úložiště Azure SDK pro jazyk Java][Azure Storage SDK for Java]
* [Referenční informace sady SDK úložiště Azure klienta][Azure úložiště klienta SDK Reference]
* [REST API pro Azure Storage][Azure Storage REST API]
* [Blog týmu Azure Storage][Azure Storage Team Blog]

Další informace najdete v tématu taky hello [středisko pro vývojáře Java](/develop/java/).

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure úložiště klienta SDK Reference]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Tables: Introducing Upsert and Query Projection]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
