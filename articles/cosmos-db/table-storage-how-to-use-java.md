---
title: "Používání úložiště Table z Javy | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu pomocí Azure Table Storage, úložiště dat typu NoSQL."
services: cosmos-db
documentationcenter: java
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 45145189-e67f-4ca6-b15d-43af7bfd3f97
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 7f92b1e14a514e9eda39f7ca94f63fc761dfdf41
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-table-storage-from-java"></a><span data-ttu-id="ab2a4-103">Používání úložiště Table z Javy</span><span class="sxs-lookup"><span data-stu-id="ab2a4-103">How to use Table storage from Java</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="ab2a4-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="ab2a4-104">Overview</span></span>
<span data-ttu-id="ab2a4-105">Tento průvodce vám ukáže, jak provádět běžné scénáře s využitím služby Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-105">This guide will show you how to perform common scenarios using the Azure Table storage service.</span></span> <span data-ttu-id="ab2a4-106">Ukázky jsou napsané v jazyce Java a použít [sada SDK úložiště Azure pro jazyk Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="ab2a4-106">The samples are written in Java and use the [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="ab2a4-107">Pokryté scénáře zahrnují **vytváření**, **výpis**, a **odstraňování** tabulky, a také **vkládání**, **dotazování**, **úprava**, a **odstraňování** entity v tabulce.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-107">The scenarios covered include **creating**, **listing**, and **deleting** tables, as well as **inserting**, **querying**, **modifying**, and **deleting** entities in a table.</span></span> <span data-ttu-id="ab2a4-108">Další informace o tabulkách najdete v tématu [další kroky](#Next-Steps) části.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-108">For more information on tables, see the [Next steps](#Next-Steps) section.</span></span>

<span data-ttu-id="ab2a4-109">Poznámka: Sada SDK je k dispozici pro vývojáře, kteří na zařízení se systémem Android používají Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-109">Note: An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="ab2a4-110">Další informace najdete v tématu [sada SDK úložiště Azure pro Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="ab2a4-110">For more information, see the [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="ab2a4-111">Vytvoření aplikace Java</span><span class="sxs-lookup"><span data-stu-id="ab2a4-111">Create a Java application</span></span>
<span data-ttu-id="ab2a4-112">V této příručce bude používat funkce úložiště, které lze spustit v rámci aplikace Java místně nebo v kódu běžící v rámci webovou roli nebo role pracovního procesu v Azure.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-112">In this guide, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="ab2a4-113">V takovém případě budete muset nainstalovat Java Development Kit (JDK) a vytvořit účet úložiště Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-113">To do so, you will need to install the Java Development Kit (JDK) and create an Azure storage account in your Azure subscription.</span></span> <span data-ttu-id="ab2a4-114">Jakmile provedete, budete muset ověřit, jestli váš vývojový systém splňuje minimální požadavky a závislosti, které jsou uvedeny v [sada SDK úložiště Azure pro jazyk Java] [ Azure Storage SDK for Java] úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-114">Once you have done so, you will need to verify that your development system meets the minimum requirements and dependencies which are listed in the [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="ab2a4-115">Pokud váš systém splňuje tyto požadavky, můžete podle pokynů ke stažení a instalaci knihovny úložiště Azure pro jazyk Java systému z tohoto úložiště.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-115">If your system meets those requirements, you can follow the instructions for downloading and installing the Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="ab2a4-116">Po dokončení těchto úloh, bude moci vytvořit aplikaci Java, která používá v příkladech v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-116">Once you have completed those tasks, you will be able to create a Java application which uses the examples in this article.</span></span>

## <a name="configure-your-application-to-access-table-storage"></a><span data-ttu-id="ab2a4-117">Konfigurace aplikace k přístupu k table storage</span><span class="sxs-lookup"><span data-stu-id="ab2a4-117">Configure your application to access table storage</span></span>
<span data-ttu-id="ab2a4-118">Přidejte následující příkazy pro import do horní části souboru Java, ve které chcete používat pro přístup k tabulky rozhraním API Microsoft Azure storage:</span><span class="sxs-lookup"><span data-stu-id="ab2a4-118">Add the following import statements to the top of the Java file where you want to use Microsoft Azure storage APIs to access tables:</span></span>

```java
// Include the following imports to use table APIs
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.table.*;
import com.microsoft.azure.storage.table.TableQuery.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="ab2a4-119">Nastavit připojovací řetězec úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="ab2a4-119">Set up an Azure storage connection string</span></span>
<span data-ttu-id="ab2a4-120">Klienta Azure storage používá připojovací řetězec úložiště k ukládání koncových bodů a pověření pro přístup ke službám dat správy.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-120">An Azure storage client uses a storage connection string to store endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="ab2a4-121">Když spustíte v aplikaci klienta, je nutné zadat připojovací řetězec úložiště v následujícím formátu, pomocí názvu účtu úložiště a primární přístupový klíč pro účet úložiště uvedené v [portál Azure](https://portal.azure.com) pro *AccountName* a *AccountKey* hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-121">When running in a client application, you must provide the storage connection string in the following format, using the name of your storage account and the Primary access key for the storage account listed in the [Azure portal](https://portal.azure.com) for the *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="ab2a4-122">Tento příklad ukazuje, jak můžou deklarovat statické pole pro uložení připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="ab2a4-122">This example shows how you can declare a static field to hold the connection string:</span></span>

```java
// Define the connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="ab2a4-123">Ve spuštěné aplikace v rámci role ve službě Microsoft Azure, můžete tento řetězec uložené v konfiguračním souboru služby *souboru ServiceConfiguration.cscfg*a je přístupný pomocí volání **RoleEnvironment.getConfigurationSettings** metoda.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-123">In an application running within a role in Microsoft Azure, this string can be stored in the service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call to the **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="ab2a4-124">Tady je příklad získávání připojovacího řetězce z **nastavení** element s názvem *StorageConnectionString* v konfiguračním souboru služby:</span><span class="sxs-lookup"><span data-stu-id="ab2a4-124">Here's an example of getting the connection string from a **Setting** element named *StorageConnectionString* in the service configuration file:</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="ab2a4-125">Následující ukázky předpokládejme, že používáte jednu z těchto dvou metod k získání připojovacího řetězce úložiště.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-125">The following samples assume that you have used one of these two methods to get the storage connection string.</span></span>

## <a name="how-to-create-a-table"></a><span data-ttu-id="ab2a4-126">Postupy: vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="ab2a4-126">How to: Create a table</span></span>
<span data-ttu-id="ab2a4-127">A **CloudTableClient** objektu umožňuje získat odkaz na objekty pro tabulky a entity.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-127">A **CloudTableClient** object lets you get reference objects for tables and entities.</span></span> <span data-ttu-id="ab2a4-128">Následující kód vytvoří **CloudTableClient** objektu a použije k vytvoření nového **CloudTable** objekt, který reprezentuje tabulku s názvem "uživatelé".</span><span class="sxs-lookup"><span data-stu-id="ab2a4-128">The following code creates a **CloudTableClient** object and uses it to create a new **CloudTable** object which represents a table named "people".</span></span> <span data-ttu-id="ab2a4-129">(Poznámka: Chcete-li vytvořit další způsoby **CloudStorageAccount** objekty; Další informace najdete v tématu **CloudStorageAccount** v [odkaz SDK klienta úložiště Azure].)</span><span class="sxs-lookup"><span data-stu-id="ab2a4-129">(Note: There are additional ways to create **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in the [Azure Storage Client SDK Reference].)</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create the table if it doesn't exist.
    String tableName = "people";
    CloudTable cloudTable = tableClient.getTableReference(tableName);
    cloudTable.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-the-tables"></a><span data-ttu-id="ab2a4-130">Postupy: seznam tabulek</span><span class="sxs-lookup"><span data-stu-id="ab2a4-130">How to: List the tables</span></span>
<span data-ttu-id="ab2a4-131">Chcete-li získat seznam tabulek, zavolejte **CloudTableClient.listTables()** metoda pro načtení iterable seznam názvů tabulek.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-131">To get a list of tables, call the **CloudTableClient.listTables()** method to retrieve an iterable list of table names.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Loop through the collection of table names.
    for (String table : tableClient.listTables())
    {
        // Output each table name.
        System.out.println(table);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-an-entity-to-a-table"></a><span data-ttu-id="ab2a4-132">Postupy: Přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="ab2a4-132">How to: Add an entity to a table</span></span>
<span data-ttu-id="ab2a4-133">Entity se mapují na objekty Java pomocí vlastní třídy implementace **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-133">Entities map to Java objects using a custom class implementing **TableEntity**.</span></span> <span data-ttu-id="ab2a4-134">Pro usnadnění práce **TableServiceEntity** třída implementuje **TableEntity** a pomocí reflexe vlastnosti metody getter a setter s názvem vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-134">For convenience, the **TableServiceEntity** class implements **TableEntity** and uses reflection to map properties to getter and setter methods named for the properties.</span></span> <span data-ttu-id="ab2a4-135">Chcete-li do tabulky přidat entitu, nejdřív vytvořte třídu, která definuje vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-135">To add an entity to a table, first create a class that defines the properties of your entity.</span></span> <span data-ttu-id="ab2a4-136">Následující kód definuje třídu entity, která používá jméno zákazníka jako klíč řádku a příjmení jako klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-136">The following code defines an entity class which uses the customer's first name as the row key, and last name as the partition key.</span></span> <span data-ttu-id="ab2a4-137">Společně pak klíč oddílu a řádku entity jednoznačně identifikují entitu v tabulce.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-137">Together, an entity's partition and row key uniquely identify the entity in the table.</span></span> <span data-ttu-id="ab2a4-138">Entity se stejným klíčem oddílu můžete položit dotaz rychleji než ty, které se různé klíče oddílů.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-138">Entities with the same partition key can be queried faster than those with different partition keys.</span></span>

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

<span data-ttu-id="ab2a4-139">Operace s tabulkou zahrnující entity vyžadují **TableOperation** objektu.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-139">Table operations involving entities require a **TableOperation** object.</span></span> <span data-ttu-id="ab2a4-140">Tento objekt definuje operace provést u entity, které lze spustit s **CloudTable** objektu.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-140">This object defines the operation to be performed on an entity, which can be executed with a **CloudTable** object.</span></span> <span data-ttu-id="ab2a4-141">Následující kód vytvoří novou instanci třídy **CustomerEntity** se některé zákaznické údaje uložit.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-141">The following code creates a new instance of the **CustomerEntity** class with some customer data to be stored.</span></span> <span data-ttu-id="ab2a4-142">Další kód zavolá metodu **TableOperation.insertOrReplace** k vytvoření **TableOperation** objektu pro vložení entity do tabulky a přidruží nový **CustomerEntity** s ním.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-142">The code next calls **TableOperation.insertOrReplace** to create a **TableOperation** object to insert an entity into a table, and associates the new **CustomerEntity** with it.</span></span> <span data-ttu-id="ab2a4-143">Nakonec kód volá **provést** metodu **CloudTable** objekt, zadání "lidé" tabulky a nové **TableOperation**, který pak odešle požadavek na službu úložiště vložení nové entity zákazníka do tabulky "lidé", nebo nahrazení entity, pokud již existuje.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-143">Finally, the code calls the **execute** method on the **CloudTable** object, specifying the "people" table and the new **TableOperation**, which then sends a request to the storage service to insert the new customer entity into the "people" table, or replace the entity if it already exists.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.setEmail("Walter@contoso.com");
    customer1.setPhoneNumber("425-555-0101");

    // Create an operation to add the new customer to the people table.
    TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

    // Submit the operation to the table service.
    cloudTable.execute(insertCustomer1);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-insert-a-batch-of-entities"></a><span data-ttu-id="ab2a4-144">Postupy: vložení dávky entit</span><span class="sxs-lookup"><span data-stu-id="ab2a4-144">How to: Insert a batch of entities</span></span>
<span data-ttu-id="ab2a4-145">V rámci jedné operace zápisu můžete vložit dávku entit do služby table.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-145">You can insert a batch of entities to the table service in one write operation.</span></span> <span data-ttu-id="ab2a4-146">Následující kód vytvoří **TableBatchOperation** objekt a potom přidá tři operace k němu vložení.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-146">The following code creates a **TableBatchOperation** object, then adds three insert operations to it.</span></span> <span data-ttu-id="ab2a4-147">Každé operace insert přidá se při vytvoření nového objektu entity, nastavení jeho hodnoty a potom volání **vložit** metodu **TableBatchOperation** objekt, který chcete přidružit nové operace insert entity.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-147">Each insert operation is added by creating a new entity object, setting its values, and then calling the **insert** method on the **TableBatchOperation** object to associate the entity with a new insert operation.</span></span> <span data-ttu-id="ab2a4-148">Potom kód zavolá metodu **provést** na **CloudTable** objekt, zadání "lidé" tabulky a **TableBatchOperation** objekt, který dávku operace s tabulkou se odešle do služby úložiště v jedné žádosti.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-148">Then the code calls **execute** on the **CloudTable** object, specifying the "people" table and the **TableBatchOperation** object, which sends the batch of table operations to the storage service in a single request.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Define a batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a customer entity to add to the table.
    CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
    customer.setEmail("Jeff@contoso.com");
    customer.setPhoneNumber("425-555-0104");
    batchOperation.insertOrReplace(customer);

    // Create another customer entity to add to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.setEmail("Ben@contoso.com");
    customer2.setPhoneNumber("425-555-0102");
    batchOperation.insertOrReplace(customer2);

    // Create a third customer entity to add to the table.
    CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
    customer3.setEmail("Denise@contoso.com");
    customer3.setPhoneNumber("425-555-0103");
    batchOperation.insertOrReplace(customer3);

    // Execute the batch of operations on the "people" table.
    cloudTable.execute(batchOperation);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="ab2a4-149">Některé věci poznamenat ohledně dávkových operací:</span><span class="sxs-lookup"><span data-stu-id="ab2a4-149">Some things to note on batch operations:</span></span>

* <span data-ttu-id="ab2a4-150">Můžete provést až 100 vložit, odstranit, sloučení, nahradit, vložit nebo sloučení a vložit nebo nahrazovat operace v libovolné kombinace v jedné dávce.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-150">You can perform up to 100 insert, delete, merge, replace, insert or merge, and insert or replace operations in any combination in a single batch.</span></span>
* <span data-ttu-id="ab2a4-151">Dávková operace může mít operaci načtení, pokud je ale jediná operace v dávce.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-151">A batch operation can have a retrieve operation, if it is the only operation in the batch.</span></span>
* <span data-ttu-id="ab2a4-152">Všechny entity v jedné dávkové operaci musí mít stejný klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-152">All entities in a single batch operation must have the same partition key.</span></span>
* <span data-ttu-id="ab2a4-153">Dávkové operace je omezený na datovou částí 4 MB volného místa.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-153">A batch operation is limited to a 4MB data payload.</span></span>

## <a name="how-to-retrieve-all-entities-in-a-partition"></a><span data-ttu-id="ab2a4-154">Postupy: načtení všech entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="ab2a4-154">How to: Retrieve all entities in a partition</span></span>
<span data-ttu-id="ab2a4-155">Dotaz na tabulku pro entity v oddílu, můžete použít **TableQuery**.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-155">To query a table for entities in a partition, you can use a **TableQuery**.</span></span> <span data-ttu-id="ab2a4-156">Volání **TableQuery.from** k vytvoření dotazu na konkrétní tabulku, která vrátí typ určeného výsledku.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-156">Call **TableQuery.from** to create a query on a particular table that returns a specified result type.</span></span> <span data-ttu-id="ab2a4-157">Následující kód určuje filtr pro entity, kde je: Váša' klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-157">The following code specifies a filter for entities where 'Smith' is the partition key.</span></span> <span data-ttu-id="ab2a4-158">**TableQuery.generateFilterCondition** je metoda helper pro vytvoření filtrů pro dotazy.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-158">**TableQuery.generateFilterCondition** is a helper method to create filters for queries.</span></span> <span data-ttu-id="ab2a4-159">Volání **kde** na odkaz na vrácený **TableQuery.from** metoda použít filtr dotazu.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-159">Call **where** on the reference returned by the **TableQuery.from** method to apply the filter to the query.</span></span> <span data-ttu-id="ab2a4-160">Provedení dotazu s výzvou k **provést** na **CloudTable** objektu se vrátí **Iterator** s **CustomerEntity** způsobit zadaný typ.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-160">When the query is executed with a call to **execute** on the **CloudTable** object, it returns an **Iterator** with the **CustomerEntity** result type specified.</span></span> <span data-ttu-id="ab2a4-161">Pak můžete použít **Iterator** , vrátí se v pro každou smyčku využívat výsledky.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-161">You can then use the **Iterator** returned in a for each loop to consume the results.</span></span> <span data-ttu-id="ab2a4-162">Tento kód zobrazí pole každé entity ve výsledcích dotazu do konzoly.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-162">This code prints the fields of each entity in the query results to the console.</span></span>

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

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where the partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Specify a partition query, using "Smith" as the partition key filter.
    TableQuery<CustomerEntity> partitionQuery =
        TableQuery.from(CustomerEntity.class)
        .where(partitionFilter);

    // Loop through the results, displaying information about the entity.
    for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a><span data-ttu-id="ab2a4-163">Postupy: načtení rozsahu entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="ab2a4-163">How to: Retrieve a range of entities in a partition</span></span>
<span data-ttu-id="ab2a4-164">Pokud nechcete, aby k dotazování všechny entity v oddílu, můžete zadat rozsah pomocí relačních operátorů ve filtru.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-164">If you don't want to query all the entities in a partition, you can specify a range by using comparison operators in a filter.</span></span> <span data-ttu-id="ab2a4-165">Následující kód kombinuje dva filtry k získání všech entit v oddílu "Smith", kde klíč řádku (jméno) začíná písmenem až "E" abecedy.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-165">The following code combines two filters to get all entities in partition "Smith" where the row key (first name) starts with a letter up to 'E' in the alphabet.</span></span> <span data-ttu-id="ab2a4-166">Potom zobrazí výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-166">Then it prints the query results.</span></span> <span data-ttu-id="ab2a4-167">Pokud používáte entity přidat do tabulky v dávce vložení této příručce, pouze dvě entity, jsou vráceny tentokrát (Ben a Karel Smith); Jeff Smith není zahrnutý.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-167">If you use the entities added to the table in the batch insert section of this guide, only two entities are returned this time (Ben and Denise Smith); Jeff Smith is not included.</span></span>

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

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where the partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Create a filter condition where the row key is less than the letter "E".
    String rowFilter = TableQuery.generateFilterCondition(
        ROW_KEY,
        QueryComparisons.LESS_THAN,
        "E");

    // Combine the two conditions into a filter expression.
    String combinedFilter = TableQuery.combineFilters(partitionFilter,
        Operators.AND, rowFilter);

    // Specify a range query, using "Smith" as the partition key,
    // with the row key being up to the letter "E".
    TableQuery<CustomerEntity> rangeQuery =
        TableQuery.from(CustomerEntity.class)
        .where(combinedFilter);

    // Loop through the results, displaying information about the entity
    for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-retrieve-a-single-entity"></a><span data-ttu-id="ab2a4-168">Postupy: načtení jedné entity</span><span class="sxs-lookup"><span data-stu-id="ab2a4-168">How to: Retrieve a single entity</span></span>
<span data-ttu-id="ab2a4-169">Můžete napsat dotaz pro načtení jedné konkrétní entity.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-169">You can write a query to retrieve a single, specific entity.</span></span> <span data-ttu-id="ab2a4-170">Následující kód volání **TableOperation.retrieve** s oddílem klíč a řádek klíče parametry k určení zákazníka "Jeff Smith", místo vytvoření **TableQuery** a stejnou věc udělat pomocí filtrů.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-170">The following code calls **TableOperation.retrieve** with partition key and row key parameters to specify the customer "Jeff Smith", instead of creating a **TableQuery** and using filters to do the same thing.</span></span> <span data-ttu-id="ab2a4-171">Po provedení operace načtení vrátí místo kolekce pouze jednu entitu.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-171">When executed, the retrieve operation returns just one entity, rather than a collection.</span></span> <span data-ttu-id="ab2a4-172">**GetResultAsType** metoda vrhá výsledek, který má typ přiřazení cíle, **CustomerEntity** objektu.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-172">The **getResultAsType** method casts the result to the type of the assignment target, a **CustomerEntity** object.</span></span> <span data-ttu-id="ab2a4-173">Pokud tento typ není kompatibilní s typem parametru dotazu, bude vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-173">If this type is not compatible with the type specified for the query, an exception will be thrown.</span></span> <span data-ttu-id="ab2a4-174">Pokud žádná entita má přesný klíč oddílu a řádku shodovat, je vrácena hodnota null.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-174">A null value is returned if no entity has an exact partition and row key match.</span></span> <span data-ttu-id="ab2a4-175">Určení jak klíčů oddílu, tak klíčů řádků v dotazu představuje nejrychlejší způsob, jak načíst jednu entitu ze služby Table.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-175">Specifying both partition and row keys in a query is the fastest way to retrieve a single entity from the Table service.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit the operation to the table service and get the specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Output the entity.
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
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-modify-an-entity"></a><span data-ttu-id="ab2a4-176">Postupy: Úprava entity</span><span class="sxs-lookup"><span data-stu-id="ab2a4-176">How to: Modify an entity</span></span>
<span data-ttu-id="ab2a4-177">Upravte entitu, načtěte ji ze služby table, změnit objekt entity a uložte změny zpět do služby table v operaci nahrazení nebo merge.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-177">To modify an entity, retrieve it from the table service, make changes to the entity object, and save the changes back to the table service with a replace or merge operation.</span></span> <span data-ttu-id="ab2a4-178">Následující kód změní telefonní číslo stávajícího zákazníka.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-178">The following code changes an existing customer's phone number.</span></span> <span data-ttu-id="ab2a4-179">Místo volání **TableOperation.insert** jako jsme provedli vložení, tento kód zavolá **TableOperation.replace**.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-179">Instead of calling **TableOperation.insert** like we did to insert, this code calls **TableOperation.replace**.</span></span> <span data-ttu-id="ab2a4-180">**CloudTable.execute** metoda volá služby table a nahradí entitu, pokud jiná aplikace změnil v čase vzhledem k tomu, že jejím načtení této aplikace.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-180">The **CloudTable.execute** method calls the table service, and the entity is replaced, unless another application changed it in the time since this application retrieved it.</span></span> <span data-ttu-id="ab2a4-181">Pokud k tomu dojde, je vyvolána výjimka, a entity musí být načteny, upravit a uložit znovu.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-181">When that happens, an exception is thrown, and the entity must be retrieved, modified, and saved again.</span></span> <span data-ttu-id="ab2a4-182">Tento vzor opakování optimistickou metodu souběžného je běžné v systém distribuovaného úložiště.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-182">This optimistic concurrency retry pattern is common in a distributed storage system.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit the operation to the table service and get the specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Specify a new phone number.
    specificEntity.setPhoneNumber("425-555-0105");

    // Create an operation to replace the entity.
    TableOperation replaceEntity = TableOperation.replace(specificEntity);

    // Submit the operation to the table service.
    cloudTable.execute(replaceEntity);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-query-a-subset-of-entity-properties"></a><span data-ttu-id="ab2a4-183">Postupy: dotaz na podmnožinu vlastností entity</span><span class="sxs-lookup"><span data-stu-id="ab2a4-183">How to: Query a subset of entity properties</span></span>
<span data-ttu-id="ab2a4-184">Dotaz na tabulku můžete načíst jenom několik z entity.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-184">A query to a table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="ab2a4-185">Tato technika, které se říká projekce, snižuje šířku pásma a může zlepšit výkon dotazů, zejména u velkých entit.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-185">This technique, called projection, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="ab2a4-186">Dotaz v následující kód používá **vyberte** metoda vrátí pouze e-mailové adresy entit v tabulce.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-186">The query in the following code uses the **select** method to return only the email addresses of entities in the table.</span></span> <span data-ttu-id="ab2a4-187">Výsledky jsou promítnout do kolekce **řetězec** za pomoci **EntityResolver**, na které se převod typů na entity vrácená serverem.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-187">The results are projected into a collection of **String** with the help of an **EntityResolver**, which does the type conversion on the entities returned from the server.</span></span> <span data-ttu-id="ab2a4-188">Další informace o projekce v [tabulky Azure: Představení funkcí Upsert a projekce dotazu][Azure Tables: Introducing Upsert and Query Projection].</span><span class="sxs-lookup"><span data-stu-id="ab2a4-188">You can learn more about projection in [Azure Tables: Introducing Upsert and Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span></span> <span data-ttu-id="ab2a4-189">Všimněte si, že projekci nepodporuje emulátor místního úložiště, takže tento kód spustí pouze v případě, že se pomocí účtu služby table.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-189">Note that projection is not supported on the local storage emulator, so this code runs only when using an account on the table service.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Define a projection query that retrieves only the Email property
    TableQuery<CustomerEntity> projectionQuery =
        TableQuery.from(CustomerEntity.class)
        .select(new String[] {"Email"});

    // Define a Entity resolver to project the entity to the Email value.
    EntityResolver<String> emailResolver = new EntityResolver<String>() {
        @Override
        public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
            return properties.get("Email").getValueAsString();
        }
    };

    // Loop through the results, displaying the Email values.
    for (String projectedString :
        cloudTable.execute(projectionQuery, emailResolver)) {
            System.out.println(projectedString);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-insert-or-replace-an-entity"></a><span data-ttu-id="ab2a4-190">Postupy: vložení nebo nahrazení entity</span><span class="sxs-lookup"><span data-stu-id="ab2a4-190">How to: Insert or Replace an entity</span></span>
<span data-ttu-id="ab2a4-191">Často budete chtít do tabulky přidat entitu, aniž by věděly, pokud již existuje v tabulce.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-191">Often you want to add an entity to a table without knowing if it already exists in the table.</span></span> <span data-ttu-id="ab2a4-192">Operace vložení nebo nahrazení můžete jedné žádosti, která budou vkládat entity, pokud ji neexistuje nebo pokud k tomu nahradí existující.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-192">An insert-or-replace operation allows you to make a single request which will insert the entity if it does not exist or replace the existing one if it does.</span></span> <span data-ttu-id="ab2a4-193">Sestavování na předchozí příklady, následující kód vložení nebo nahrazení entity pro "Walter tuleňů grónských".</span><span class="sxs-lookup"><span data-stu-id="ab2a4-193">Building on prior examples, the following code inserts or replaces the entity for "Walter Harp".</span></span> <span data-ttu-id="ab2a4-194">Po vytvoření nové entity, tento kód zavolá **TableOperation.insertOrReplace** metoda.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-194">After creating a new entity, this code calls the **TableOperation.insertOrReplace** method.</span></span> <span data-ttu-id="ab2a4-195">Tento kód pak zavolá **provést** na **CloudTable** objektu s tabulkou a úlohy insert nebo nahradit tabulku operace jako parametry.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-195">This code then calls **execute** on the **CloudTable** object with the table and the insert or replace table operation as the parameters.</span></span> <span data-ttu-id="ab2a4-196">Chcete-li aktualizovat jenom část entity, **TableOperation.insertOrMerge** metodu je možné použít místo.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-196">To update only part of an entity, the **TableOperation.insertOrMerge** method can be used instead.</span></span> <span data-ttu-id="ab2a4-197">Všimněte si, že vložení nebo nahrazení není podporována na emulátor místního úložiště, takže tento kód spustí pouze v případě, že se pomocí účtu služby table.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-197">Note that insert-or-replace is not supported on the local storage emulator, so this code runs only when using an account on the table service.</span></span> <span data-ttu-id="ab2a4-198">Můžete další informace o vložení nebo nahrazení a vložení nebo sloučení v tomto [tabulky Azure: Představení funkcí Upsert a projekce dotazu][Azure Tables: Introducing Upsert and Query Projection].</span><span class="sxs-lookup"><span data-stu-id="ab2a4-198">You can learn more about insert-or-replace and insert-or-merge in this [Azure Tables: Introducing Upsert and Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
    customer5.setEmail("Walter@contoso.com");
    customer5.setPhoneNumber("425-555-0106");

    // Create an operation to add the new customer to the people table.
    TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

    // Submit the operation to the table service.
    cloudTable.execute(insertCustomer5);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-an-entity"></a><span data-ttu-id="ab2a4-199">Postupy: Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="ab2a4-199">How to: Delete an entity</span></span>
<span data-ttu-id="ab2a4-200">Entitu můžete snadno odstranit po jejím načtení.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-200">You can easily delete an entity after you have retrieved it.</span></span> <span data-ttu-id="ab2a4-201">Jakmile se načíst entity, volání **TableOperation.delete** s entity, která má odstranit.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-201">Once the entity is retrieved, call **TableOperation.delete** with the entity to delete.</span></span> <span data-ttu-id="ab2a4-202">Potom zavolejte **provést** na **CloudTable** objektu.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-202">Then call **execute** on the **CloudTable** object.</span></span> <span data-ttu-id="ab2a4-203">Následující kód načte a odstraní entitu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-203">The following code retrieves and deletes a customer entity.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    CustomerEntity entitySmithJeff =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Create an operation to delete the entity.
    TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

    // Submit the delete operation to the table service.
    cloudTable.execute(deleteSmithJeff);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-table"></a><span data-ttu-id="ab2a4-204">Postupy: odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="ab2a4-204">How to: Delete a table</span></span>
<span data-ttu-id="ab2a4-205">Následující kód nakonec odstraní tabulku z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-205">Finally, the following code deletes a table from a storage account.</span></span> <span data-ttu-id="ab2a4-206">Tabulky, který byl odstraněn nebude znovu vytvořit pro následující odstranění, obvykle menší než 40 sekund časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-206">A table which has been deleted will be unavailable to be recreated for a period of time following the deletion, usually less than forty seconds.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Delete the table and all its data if it exists.
    CloudTable cloudTable = tableClient.getTableReference("people");
    cloudTable.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```
[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="next-steps"></a><span data-ttu-id="ab2a4-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ab2a4-207">Next steps</span></span>

* <span data-ttu-id="ab2a4-208">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.</span><span class="sxs-lookup"><span data-stu-id="ab2a4-208">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="ab2a4-209">[Úložiště Azure SDK pro jazyk Java][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="ab2a4-209">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="ab2a4-210">[Referenční informace sady SDK úložiště Azure klienta][odkaz SDK klienta úložiště Azure]</span><span class="sxs-lookup"><span data-stu-id="ab2a4-210">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="ab2a4-211">[REST API pro Azure Storage][Azure Storage REST API]</span><span class="sxs-lookup"><span data-stu-id="ab2a4-211">[Azure Storage REST API][Azure Storage REST API]</span></span>
* <span data-ttu-id="ab2a4-212">[Blog týmu Azure Storage][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="ab2a4-212">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

<span data-ttu-id="ab2a4-213">Další informace najdete na webu [Azure pro vývojáře v Javě](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="ab2a4-213">For more information, visit [Azure for Java developers](/java/azure).</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[odkaz SDK klienta úložiště Azure]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Tables: Introducing Upsert and Query Projection]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
