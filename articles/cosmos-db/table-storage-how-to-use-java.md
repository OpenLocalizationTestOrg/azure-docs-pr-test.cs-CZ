---
title: "aaaHow toouse úložiště Table z Javy | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu hello pomocí Azure Table storage, úložiště dat typu NoSQL."
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
ms.openlocfilehash: 20d03e867219cc254da8dad37cf3cf61bca65671
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-java"></a><span data-ttu-id="99f04-103">Jak toouse úložiště Table z Javy</span><span class="sxs-lookup"><span data-stu-id="99f04-103">How toouse Table storage from Java</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="99f04-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="99f04-104">Overview</span></span>
<span data-ttu-id="99f04-105">Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="99f04-105">This guide will show you how tooperform common scenarios using hello Azure Table storage service.</span></span> <span data-ttu-id="99f04-106">Hello ukázky jsou napsané v jazyce Java a používají hello [sada SDK úložiště Azure pro jazyk Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="99f04-106">hello samples are written in Java and use hello [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="99f04-107">Hello pokryté scénáře zahrnují **vytváření**, **výpis**, a **odstraňování** tabulky, a také **vkládání**,  **dotazování**, **úprava**, a **odstraňování** entity v tabulce.</span><span class="sxs-lookup"><span data-stu-id="99f04-107">hello scenarios covered include **creating**, **listing**, and **deleting** tables, as well as **inserting**, **querying**, **modifying**, and **deleting** entities in a table.</span></span> <span data-ttu-id="99f04-108">Další informace o tabulkách najdete v tématu hello [další kroky](#Next-Steps) části.</span><span class="sxs-lookup"><span data-stu-id="99f04-108">For more information on tables, see hello [Next steps](#Next-Steps) section.</span></span>

<span data-ttu-id="99f04-109">Poznámka: Sada SDK je k dispozici pro vývojáře, kteří na zařízení se systémem Android používají Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="99f04-109">Note: An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="99f04-110">Další informace najdete v tématu hello [sada SDK úložiště Azure pro Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="99f04-110">For more information, see hello [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="99f04-111">Vytvoření aplikace Java</span><span class="sxs-lookup"><span data-stu-id="99f04-111">Create a Java application</span></span>
<span data-ttu-id="99f04-112">V této příručce bude používat funkce úložiště, které lze spustit v rámci aplikace Java místně nebo v kódu běžící v rámci webovou roli nebo role pracovního procesu v Azure.</span><span class="sxs-lookup"><span data-stu-id="99f04-112">In this guide, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="99f04-113">toodo tedy budete potřebovat tooinstall hello Java Development Kit (JDK) a vytvořit účet úložiště Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="99f04-113">toodo so, you will need tooinstall hello Java Development Kit (JDK) and create an Azure storage account in your Azure subscription.</span></span> <span data-ttu-id="99f04-114">Jakmile provedete, budete potřebovat tooverify, který váš vývojový systém splňuje minimální požadavky hello a závislosti, které jsou uvedeny v hello [sada SDK úložiště Azure pro jazyk Java] [ Azure Storage SDK for Java] úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="99f04-114">Once you have done so, you will need tooverify that your development system meets hello minimum requirements and dependencies which are listed in hello [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="99f04-115">Pokud váš systém splňuje tyto požadavky, můžete postupovat podle pokynů hello ke stažení a instalaci hello knihovny úložiště Azure pro jazyk Java systému z tohoto úložiště.</span><span class="sxs-lookup"><span data-stu-id="99f04-115">If your system meets those requirements, you can follow hello instructions for downloading and installing hello Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="99f04-116">Po dokončení těchto úloh, bude možné toocreate aplikaci Java, která používá hello příklady v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="99f04-116">Once you have completed those tasks, you will be able toocreate a Java application which uses hello examples in this article.</span></span>

## <a name="configure-your-application-tooaccess-table-storage"></a><span data-ttu-id="99f04-117">Konfigurace úložiště table tooaccess vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="99f04-117">Configure your application tooaccess table storage</span></span>
<span data-ttu-id="99f04-118">Přidejte následující import příkazy toohello horní části souboru Java hello místo toouse Microsoft Azure storage rozhraní API tooaccess tabulky hello:</span><span class="sxs-lookup"><span data-stu-id="99f04-118">Add hello following import statements toohello top of hello Java file where you want toouse Microsoft Azure storage APIs tooaccess tables:</span></span>

```java
// Include hello following imports toouse table APIs
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.table.*;
import com.microsoft.azure.storage.table.TableQuery.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="99f04-119">Nastavit připojovací řetězec úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="99f04-119">Set up an Azure storage connection string</span></span>
<span data-ttu-id="99f04-120">Klienta Azure storage používá úložiště připojovací řetězec toostore koncových bodů a přihlašovací údaje pro přístup ke službám dat správy.</span><span class="sxs-lookup"><span data-stu-id="99f04-120">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="99f04-121">Když spustíte v aplikaci klienta, musíte zadat připojovací řetězec úložiště hello v hello formátu, pomocí hello názvu účtu úložiště a hello primární přístupový klíč pro účet úložiště hello uvedené v hello [portál Azure](https://portal.azure.com)pro hello *AccountName* a *AccountKey* hodnoty.</span><span class="sxs-lookup"><span data-stu-id="99f04-121">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello Primary access key for hello storage account listed in hello [Azure portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="99f04-122">Tento příklad ukazuje, jak můžou deklarovat statické pole toohold hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="99f04-122">This example shows how you can declare a static field toohold hello connection string:</span></span>

```java
// Define hello connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="99f04-123">Ve spuštěné aplikace v rámci role ve službě Microsoft Azure, můžete tento řetězec uložené v konfiguračním souboru služby hello, *souboru ServiceConfiguration.cscfg*a je přístupný pomocí volání toohello  **RoleEnvironment.getConfigurationSettings** metoda.</span><span class="sxs-lookup"><span data-stu-id="99f04-123">In an application running within a role in Microsoft Azure, this string can be stored in hello service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call toohello **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="99f04-124">Tady je příklad získávání hello připojovacího řetězce z **nastavení** element s názvem *StorageConnectionString* v konfiguračním souboru služby hello:</span><span class="sxs-lookup"><span data-stu-id="99f04-124">Here's an example of getting hello connection string from a **Setting** element named *StorageConnectionString* in hello service configuration file:</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="99f04-125">Hello následující ukázky předpokládejme použít jednu z těchto dvou metod tooget hello úložiště připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="99f04-125">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>

## <a name="how-to-create-a-table"></a><span data-ttu-id="99f04-126">Postupy: vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="99f04-126">How to: Create a table</span></span>
<span data-ttu-id="99f04-127">A **CloudTableClient** objektu umožňuje získat odkaz na objekty pro tabulky a entity.</span><span class="sxs-lookup"><span data-stu-id="99f04-127">A **CloudTableClient** object lets you get reference objects for tables and entities.</span></span> <span data-ttu-id="99f04-128">Hello následující kód vytvoří **CloudTableClient** objektu a používá je toocreate nový **CloudTable** objekt, který reprezentuje tabulku s názvem "uživatelé".</span><span class="sxs-lookup"><span data-stu-id="99f04-128">hello following code creates a **CloudTableClient** object and uses it toocreate a new **CloudTable** object which represents a table named "people".</span></span> <span data-ttu-id="99f04-129">(Poznámka: existují další způsoby toocreate **CloudStorageAccount** objekty; Další informace najdete v tématu **CloudStorageAccount** v hello [Azure úložiště klienta SDK Reference].)</span><span class="sxs-lookup"><span data-stu-id="99f04-129">(Note: There are additional ways toocreate **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in hello [Azure Storage Client SDK Reference].)</span></span>

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

## <a name="how-to-list-hello-tables"></a><span data-ttu-id="99f04-130">Postupy: seznam tabulek hello</span><span class="sxs-lookup"><span data-stu-id="99f04-130">How to: List hello tables</span></span>
<span data-ttu-id="99f04-131">tooget seznam tabulek, volání hello **CloudTableClient.listTables()** metoda tooretrieve iterable seznam názvů tabulek.</span><span class="sxs-lookup"><span data-stu-id="99f04-131">tooget a list of tables, call hello **CloudTableClient.listTables()** method tooretrieve an iterable list of table names.</span></span>

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

## <a name="how-to-add-an-entity-tooa-table"></a><span data-ttu-id="99f04-132">Postupy: Přidání tooa tabulka entity</span><span class="sxs-lookup"><span data-stu-id="99f04-132">How to: Add an entity tooa table</span></span>
<span data-ttu-id="99f04-133">Entity se mapují tooJava objektů s použitím vlastní třída implementace **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="99f04-133">Entities map tooJava objects using a custom class implementing **TableEntity**.</span></span> <span data-ttu-id="99f04-134">Pro usnadnění práce hello **TableServiceEntity** třída implementuje **TableEntity** a používá reflexe toomap vlastnosti s názvem toogetter a setter metody pro hello vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="99f04-134">For convenience, hello **TableServiceEntity** class implements **TableEntity** and uses reflection toomap properties toogetter and setter methods named for hello properties.</span></span> <span data-ttu-id="99f04-135">tooadd tooa tabulka entity, nejdřív vytvořte třídu, která definuje hello vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="99f04-135">tooadd an entity tooa table, first create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="99f04-136">Hello následující kód definuje třídu entity, která používá jméno hello zákazníka jako klíč řádku hello a příjmení jako klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="99f04-136">hello following code defines an entity class which uses hello customer's first name as hello row key, and last name as hello partition key.</span></span> <span data-ttu-id="99f04-137">Společně oddílu a klíč řádku jednoznačně hello entity v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="99f04-137">Together, an entity's partition and row key uniquely identify hello entity in hello table.</span></span> <span data-ttu-id="99f04-138">Entity se stejným klíčem oddílu můžete zadat dotaz rychleji než ty, které se různé klíče oddílů hello.</span><span class="sxs-lookup"><span data-stu-id="99f04-138">Entities with hello same partition key can be queried faster than those with different partition keys.</span></span>

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

<span data-ttu-id="99f04-139">Operace s tabulkou zahrnující entity vyžadují **TableOperation** objektu.</span><span class="sxs-lookup"><span data-stu-id="99f04-139">Table operations involving entities require a **TableOperation** object.</span></span> <span data-ttu-id="99f04-140">Tento objekt definuje toobe hello operaci provést u entity, které lze spustit s **CloudTable** objektu.</span><span class="sxs-lookup"><span data-stu-id="99f04-140">This object defines hello operation toobe performed on an entity, which can be executed with a **CloudTable** object.</span></span> <span data-ttu-id="99f04-141">Hello následující kód vytvoří novou instanci třídy hello **CustomerEntity** se některé toobe dat zákazníka uložené.</span><span class="sxs-lookup"><span data-stu-id="99f04-141">hello following code creates a new instance of hello **CustomerEntity** class with some customer data toobe stored.</span></span> <span data-ttu-id="99f04-142">Hello kód zavolá metodu Další **TableOperation.insertOrReplace** toocreate **TableOperation** objektu tooinsert entity do tabulky a partnerů hello nové **CustomerEntity**s ním.</span><span class="sxs-lookup"><span data-stu-id="99f04-142">hello code next calls **TableOperation.insertOrReplace** toocreate a **TableOperation** object tooinsert an entity into a table, and associates hello new **CustomerEntity** with it.</span></span> <span data-ttu-id="99f04-143">Nakonec hello kód volá hello **provést** metodu hello **CloudTable** objekt, zadáte hello "lidé" tabulky a hello nové **TableOperation**, které pak odešle žádosti o toohello úložiště služby tooinsert hello novou entitu zákazníka do tabulky "lidé" hello, nebo nahrazení hello entity, pokud již existuje.</span><span class="sxs-lookup"><span data-stu-id="99f04-143">Finally, hello code calls hello **execute** method on hello **CloudTable** object, specifying hello "people" table and hello new **TableOperation**, which then sends a request toohello storage service tooinsert hello new customer entity into hello "people" table, or replace hello entity if it already exists.</span></span>

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

## <a name="how-to-insert-a-batch-of-entities"></a><span data-ttu-id="99f04-144">Postupy: vložení dávky entit</span><span class="sxs-lookup"><span data-stu-id="99f04-144">How to: Insert a batch of entities</span></span>
<span data-ttu-id="99f04-145">Můžete vložit dávku entit toohello tabulka služby v rámci jedné operace zápisu.</span><span class="sxs-lookup"><span data-stu-id="99f04-145">You can insert a batch of entities toohello table service in one write operation.</span></span> <span data-ttu-id="99f04-146">Hello následující kód vytvoří **TableBatchOperation** objekt a potom přidá tři vložit tooit operace.</span><span class="sxs-lookup"><span data-stu-id="99f04-146">hello following code creates a **TableBatchOperation** object, then adds three insert operations tooit.</span></span> <span data-ttu-id="99f04-147">Každé operace insert přidá se při vytvoření nového objektu entity, nastavení jeho hodnoty a potom volání hello **vložit** metodu hello **TableBatchOperation** objekt entity hello tooassociate s novou Vložte operaci.</span><span class="sxs-lookup"><span data-stu-id="99f04-147">Each insert operation is added by creating a new entity object, setting its values, and then calling hello **insert** method on hello **TableBatchOperation** object tooassociate hello entity with a new insert operation.</span></span> <span data-ttu-id="99f04-148">Potom hello kód zavolá metodu **provést** na hello **CloudTable** objekt, zadáte hello "lidé" tabulky a hello **TableBatchOperation** objekt, který odesílá hello batch tabulky operace toohello úložiště služby v jedné žádosti.</span><span class="sxs-lookup"><span data-stu-id="99f04-148">Then hello code calls **execute** on hello **CloudTable** object, specifying hello "people" table and hello **TableBatchOperation** object, which sends hello batch of table operations toohello storage service in a single request.</span></span>

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

<span data-ttu-id="99f04-149">Některé věci toonote ohledně dávkových operací:</span><span class="sxs-lookup"><span data-stu-id="99f04-149">Some things toonote on batch operations:</span></span>

* <span data-ttu-id="99f04-150">Můžete provést až too100 vložení, odstranění, sloučení, nahraďte, insert nebo merge a vložit nebo nahrazovat operace v libovolné kombinace v jedné dávce.</span><span class="sxs-lookup"><span data-stu-id="99f04-150">You can perform up too100 insert, delete, merge, replace, insert or merge, and insert or replace operations in any combination in a single batch.</span></span>
* <span data-ttu-id="99f04-151">Dávková operace může mít operaci načtení, pokud je hello jediná operace v dávce hello.</span><span class="sxs-lookup"><span data-stu-id="99f04-151">A batch operation can have a retrieve operation, if it is hello only operation in hello batch.</span></span>
* <span data-ttu-id="99f04-152">Všechny entity v jedné dávkové operaci musí mít hello stejným klíčem oddílu.</span><span class="sxs-lookup"><span data-stu-id="99f04-152">All entities in a single batch operation must have hello same partition key.</span></span>
* <span data-ttu-id="99f04-153">Dávkové operace je omezená tooa 4MB datovou částí.</span><span class="sxs-lookup"><span data-stu-id="99f04-153">A batch operation is limited tooa 4MB data payload.</span></span>

## <a name="how-to-retrieve-all-entities-in-a-partition"></a><span data-ttu-id="99f04-154">Postupy: načtení všech entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="99f04-154">How to: Retrieve all entities in a partition</span></span>
<span data-ttu-id="99f04-155">tooquery tabulku pro entity v oddílu, můžete použít **TableQuery**.</span><span class="sxs-lookup"><span data-stu-id="99f04-155">tooquery a table for entities in a partition, you can use a **TableQuery**.</span></span> <span data-ttu-id="99f04-156">Volání **TableQuery.from** toocreate dotazu pro konkrétní tabulku, která vrátí hodnotu typu určeného výsledku.</span><span class="sxs-lookup"><span data-stu-id="99f04-156">Call **TableQuery.from** toocreate a query on a particular table that returns a specified result type.</span></span> <span data-ttu-id="99f04-157">Hello následujícího kódu určuje filtr pro entity, kde je: Váša' klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="99f04-157">hello following code specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="99f04-158">**TableQuery.generateFilterCondition** je metoda helper toocreate filtry pro dotazy.</span><span class="sxs-lookup"><span data-stu-id="99f04-158">**TableQuery.generateFilterCondition** is a helper method toocreate filters for queries.</span></span> <span data-ttu-id="99f04-159">Volání **kde** na odkaz hello vrácený hello **TableQuery.from** metoda tooapply hello filtru toohello dotazu.</span><span class="sxs-lookup"><span data-stu-id="99f04-159">Call **where** on hello reference returned by hello **TableQuery.from** method tooapply hello filter toohello query.</span></span> <span data-ttu-id="99f04-160">Když hello se spustí dotaz pomocí volání příliš**provést** na hello **CloudTable** objektu se vrátí **Iterator** s hello **CustomerEntity**způsobit zadaný typ.</span><span class="sxs-lookup"><span data-stu-id="99f04-160">When hello query is executed with a call too**execute** on hello **CloudTable** object, it returns an **Iterator** with hello **CustomerEntity** result type specified.</span></span> <span data-ttu-id="99f04-161">Pak můžete použít hello **Iterator** , vrátí se v pro každou smyčku tooconsume hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="99f04-161">You can then use hello **Iterator** returned in a for each loop tooconsume hello results.</span></span> <span data-ttu-id="99f04-162">Tento kód zobrazí pole každé entity v konzole toohello výsledky dotazu hello hello.</span><span class="sxs-lookup"><span data-stu-id="99f04-162">This code prints hello fields of each entity in hello query results toohello console.</span></span>

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

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a><span data-ttu-id="99f04-163">Postupy: načtení rozsahu entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="99f04-163">How to: Retrieve a range of entities in a partition</span></span>
<span data-ttu-id="99f04-164">Pokud nechcete, aby tooquery všechny hello entit v oddílu, můžete zadat rozsah pomocí relačních operátorů ve filtru.</span><span class="sxs-lookup"><span data-stu-id="99f04-164">If you don't want tooquery all hello entities in a partition, you can specify a range by using comparison operators in a filter.</span></span> <span data-ttu-id="99f04-165">Následující kód kombinuje dva filtry tooget všechny entity v oddílu "Smith" kde klíč řádku hello (jméno) začíná písmenem až too'E Hello' v hello abecedy.</span><span class="sxs-lookup"><span data-stu-id="99f04-165">hello following code combines two filters tooget all entities in partition "Smith" where hello row key (first name) starts with a letter up too'E' in hello alphabet.</span></span> <span data-ttu-id="99f04-166">Potom zobrazí výsledky dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="99f04-166">Then it prints hello query results.</span></span> <span data-ttu-id="99f04-167">Pokud používáte hello entity added toohello tabulky v dávce hello vložení této příručce, pouze dvě entity, jsou vráceny tentokrát (Ben a Karel Smith); Jeff Smith není zahrnutý.</span><span class="sxs-lookup"><span data-stu-id="99f04-167">If you use hello entities added toohello table in hello batch insert section of this guide, only two entities are returned this time (Ben and Denise Smith); Jeff Smith is not included.</span></span>

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

## <a name="how-to-retrieve-a-single-entity"></a><span data-ttu-id="99f04-168">Postupy: načtení jedné entity</span><span class="sxs-lookup"><span data-stu-id="99f04-168">How to: Retrieve a single entity</span></span>
<span data-ttu-id="99f04-169">Můžete napsat dotaz tooretrieve jedné konkrétní entity.</span><span class="sxs-lookup"><span data-stu-id="99f04-169">You can write a query tooretrieve a single, specific entity.</span></span> <span data-ttu-id="99f04-170">Hello následující kód volání **TableOperation.retrieve** s oddílu klíč a řádek parametrů klíče toospecify hello zákazníka "Jeff Smith", místo vytvoření **TableQuery** a pomocí filtrů toodo hello samé.</span><span class="sxs-lookup"><span data-stu-id="99f04-170">hello following code calls **TableOperation.retrieve** with partition key and row key parameters toospecify hello customer "Jeff Smith", instead of creating a **TableQuery** and using filters toodo hello same thing.</span></span> <span data-ttu-id="99f04-171">Po provedení hello načíst operace vrátí pouze jednu entitu, nikoli kolekce.</span><span class="sxs-lookup"><span data-stu-id="99f04-171">When executed, hello retrieve operation returns just one entity, rather than a collection.</span></span> <span data-ttu-id="99f04-172">Hello **getResultAsType** metoda vrhá hello výsledný typ toohello hello přiřazení cíl, **CustomerEntity** objektu.</span><span class="sxs-lookup"><span data-stu-id="99f04-172">hello **getResultAsType** method casts hello result toohello type of hello assignment target, a **CustomerEntity** object.</span></span> <span data-ttu-id="99f04-173">Pokud tento typ není kompatibilní s typem hello pro hello dotaz byl zadán, bude vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="99f04-173">If this type is not compatible with hello type specified for hello query, an exception will be thrown.</span></span> <span data-ttu-id="99f04-174">Pokud žádná entita má přesný klíč oddílu a řádku shodovat, je vrácena hodnota null.</span><span class="sxs-lookup"><span data-stu-id="99f04-174">A null value is returned if no entity has an exact partition and row key match.</span></span> <span data-ttu-id="99f04-175">Určení klíče oddílu a řádku v dotazu je hello nejrychlejší způsob, jak tooretrieve jednu entitu ze služby Table hello.</span><span class="sxs-lookup"><span data-stu-id="99f04-175">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello Table service.</span></span>

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

## <a name="how-to-modify-an-entity"></a><span data-ttu-id="99f04-176">Postupy: Úprava entity</span><span class="sxs-lookup"><span data-stu-id="99f04-176">How to: Modify an entity</span></span>
<span data-ttu-id="99f04-177">toomodify entity, načtěte ji ze služby table hello, proveďte objekt entity toohello změny a uložte změny hello služby table back toohello s operaci nahrazení nebo merge.</span><span class="sxs-lookup"><span data-stu-id="99f04-177">toomodify an entity, retrieve it from hello table service, make changes toohello entity object, and save hello changes back toohello table service with a replace or merge operation.</span></span> <span data-ttu-id="99f04-178">Hello následující kód změní telefonní číslo stávajícího zákazníka.</span><span class="sxs-lookup"><span data-stu-id="99f04-178">hello following code changes an existing customer's phone number.</span></span> <span data-ttu-id="99f04-179">Místo volání **TableOperation.insert** jako jsme provedli tooinsert, tento kód zavolá **TableOperation.replace**.</span><span class="sxs-lookup"><span data-stu-id="99f04-179">Instead of calling **TableOperation.insert** like we did tooinsert, this code calls **TableOperation.replace**.</span></span> <span data-ttu-id="99f04-180">Hello **CloudTable.execute** metoda volá služby table hello a hello entity je nahrazena, pokud jiná aplikace změnil v čase hello vzhledem k tomu, že jejím načtení této aplikace.</span><span class="sxs-lookup"><span data-stu-id="99f04-180">hello **CloudTable.execute** method calls hello table service, and hello entity is replaced, unless another application changed it in hello time since this application retrieved it.</span></span> <span data-ttu-id="99f04-181">Pokud k tomu dojde, je vyvolána výjimka, a hello entit musí být načteny, upravit a uložit znovu.</span><span class="sxs-lookup"><span data-stu-id="99f04-181">When that happens, an exception is thrown, and hello entity must be retrieved, modified, and saved again.</span></span> <span data-ttu-id="99f04-182">Tento vzor opakování optimistickou metodu souběžného je běžné v systém distribuovaného úložiště.</span><span class="sxs-lookup"><span data-stu-id="99f04-182">This optimistic concurrency retry pattern is common in a distributed storage system.</span></span>

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

## <a name="how-to-query-a-subset-of-entity-properties"></a><span data-ttu-id="99f04-183">Postupy: dotaz na podmnožinu vlastností entity</span><span class="sxs-lookup"><span data-stu-id="99f04-183">How to: Query a subset of entity properties</span></span>
<span data-ttu-id="99f04-184">Tabulku tooa dotazu můžete načíst jenom několik z entity.</span><span class="sxs-lookup"><span data-stu-id="99f04-184">A query tooa table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="99f04-185">Tato technika, které se říká projekce, snižuje šířku pásma a může zlepšit výkon dotazů, zejména u velkých entit.</span><span class="sxs-lookup"><span data-stu-id="99f04-185">This technique, called projection, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="99f04-186">Hello dotaz v hello následující kód používá hello **vyberte** metoda tooreturn pouze hello e-mailové adresy entit v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="99f04-186">hello query in hello following code uses hello **select** method tooreturn only hello email addresses of entities in hello table.</span></span> <span data-ttu-id="99f04-187">Hello výsledky jsou promítnout do kolekce **řetězec** pomoci hello **EntityResolver**, který hello převod typů na hello entity, kterou vrátil hello server.</span><span class="sxs-lookup"><span data-stu-id="99f04-187">hello results are projected into a collection of **String** with hello help of an **EntityResolver**, which does hello type conversion on hello entities returned from hello server.</span></span> <span data-ttu-id="99f04-188">Další informace o projekce v [tabulky Azure: Představení funkcí Upsert a projekce dotazu][Azure Tables: Introducing Upsert and Query Projection].</span><span class="sxs-lookup"><span data-stu-id="99f04-188">You can learn more about projection in [Azure Tables: Introducing Upsert and Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span></span> <span data-ttu-id="99f04-189">Všimněte si, že projekci nepodporuje emulátor místního úložiště hello, proto tento kód spustí pouze v případě, že se pomocí účtu ve službě table hello.</span><span class="sxs-lookup"><span data-stu-id="99f04-189">Note that projection is not supported on hello local storage emulator, so this code runs only when using an account on hello table service.</span></span>

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

## <a name="how-to-insert-or-replace-an-entity"></a><span data-ttu-id="99f04-190">Postupy: vložení nebo nahrazení entity</span><span class="sxs-lookup"><span data-stu-id="99f04-190">How to: Insert or Replace an entity</span></span>
<span data-ttu-id="99f04-191">Často se má tooadd tooa tabulka entity aniž by věděly, pokud již existuje v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="99f04-191">Often you want tooadd an entity tooa table without knowing if it already exists in hello table.</span></span> <span data-ttu-id="99f04-192">Operace vložení nebo nahrazení umožňuje toomake jedné žádosti, která budou vkládat hello entity, pokud ji neexistuje nebo nahradit stávající jeden, pokud k tomu hello.</span><span class="sxs-lookup"><span data-stu-id="99f04-192">An insert-or-replace operation allows you toomake a single request which will insert hello entity if it does not exist or replace hello existing one if it does.</span></span> <span data-ttu-id="99f04-193">Sestavování na předchozí příklady, hello následující kód vložení nebo nahrazení hello entity pro "Walter tuleňů grónských".</span><span class="sxs-lookup"><span data-stu-id="99f04-193">Building on prior examples, hello following code inserts or replaces hello entity for "Walter Harp".</span></span> <span data-ttu-id="99f04-194">Po vytvoření nové entity, tento kód zavolá hello **TableOperation.insertOrReplace** metoda.</span><span class="sxs-lookup"><span data-stu-id="99f04-194">After creating a new entity, this code calls hello **TableOperation.insertOrReplace** method.</span></span> <span data-ttu-id="99f04-195">Tento kód pak zavolá **provést** na hello **CloudTable** objektu s hello tabulky a hello vložení nebo nahrazení operace tabulka jako parametry hello.</span><span class="sxs-lookup"><span data-stu-id="99f04-195">This code then calls **execute** on hello **CloudTable** object with hello table and hello insert or replace table operation as hello parameters.</span></span> <span data-ttu-id="99f04-196">tooupdate pouze část entity, hello **TableOperation.insertOrMerge** metodu je možné použít místo.</span><span class="sxs-lookup"><span data-stu-id="99f04-196">tooupdate only part of an entity, hello **TableOperation.insertOrMerge** method can be used instead.</span></span> <span data-ttu-id="99f04-197">Všimněte si, že, tento kód spustí pouze v případě, že se pomocí účtu ve službě table hello nepodporuje emulátor místního úložiště hello, že vložení nebo nahrazení.</span><span class="sxs-lookup"><span data-stu-id="99f04-197">Note that insert-or-replace is not supported on hello local storage emulator, so this code runs only when using an account on hello table service.</span></span> <span data-ttu-id="99f04-198">Můžete další informace o vložení nebo nahrazení a vložení nebo sloučení v tomto [tabulky Azure: Představení funkcí Upsert a projekce dotazu][Azure Tables: Introducing Upsert and Query Projection].</span><span class="sxs-lookup"><span data-stu-id="99f04-198">You can learn more about insert-or-replace and insert-or-merge in this [Azure Tables: Introducing Upsert and Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span></span>

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

## <a name="how-to-delete-an-entity"></a><span data-ttu-id="99f04-199">Postupy: Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="99f04-199">How to: Delete an entity</span></span>
<span data-ttu-id="99f04-200">Entitu můžete snadno odstranit po jejím načtení.</span><span class="sxs-lookup"><span data-stu-id="99f04-200">You can easily delete an entity after you have retrieved it.</span></span> <span data-ttu-id="99f04-201">Jakmile je načíst hello entity, volání **TableOperation.delete** s hello entity toodelete.</span><span class="sxs-lookup"><span data-stu-id="99f04-201">Once hello entity is retrieved, call **TableOperation.delete** with hello entity toodelete.</span></span> <span data-ttu-id="99f04-202">Potom zavolejte **provést** na hello **CloudTable** objektu.</span><span class="sxs-lookup"><span data-stu-id="99f04-202">Then call **execute** on hello **CloudTable** object.</span></span> <span data-ttu-id="99f04-203">Hello následující kód načte a odstraní entitu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="99f04-203">hello following code retrieves and deletes a customer entity.</span></span>

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

## <a name="how-to-delete-a-table"></a><span data-ttu-id="99f04-204">Postupy: odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="99f04-204">How to: Delete a table</span></span>
<span data-ttu-id="99f04-205">Nakonec hello následující kód odstraní tabulku z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="99f04-205">Finally, hello following code deletes a table from a storage account.</span></span> <span data-ttu-id="99f04-206">Tabulka, která byla odstraněna budou k dispozici toobe znovu vytvoří dobu následující hello odstranění, obvykle menší než 40 sekund.</span><span class="sxs-lookup"><span data-stu-id="99f04-206">A table which has been deleted will be unavailable toobe recreated for a period of time following hello deletion, usually less than forty seconds.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="99f04-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="99f04-207">Next steps</span></span>

* <span data-ttu-id="99f04-208">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="99f04-208">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="99f04-209">[Úložiště Azure SDK pro jazyk Java][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="99f04-209">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="99f04-210">[Referenční informace sady SDK úložiště Azure klienta][Azure úložiště klienta SDK Reference]</span><span class="sxs-lookup"><span data-stu-id="99f04-210">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="99f04-211">[REST API pro Azure Storage][Azure Storage REST API]</span><span class="sxs-lookup"><span data-stu-id="99f04-211">[Azure Storage REST API][Azure Storage REST API]</span></span>
* <span data-ttu-id="99f04-212">[Blog týmu Azure Storage][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="99f04-212">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

<span data-ttu-id="99f04-213">Další informace najdete na webu [Azure pro vývojáře v Javě](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="99f04-213">For more information, visit [Azure for Java developers](/java/azure).</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure úložiště klienta SDK Reference]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Tables: Introducing Upsert and Query Projection]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
