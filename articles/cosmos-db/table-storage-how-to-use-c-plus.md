---
title: "Postup používání úložiště Table (C++) | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu pomocí Azure Table Storage, úložiště dat typu NoSQL."
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jahogg
editor: tysonn
ms.assetid: f191f308-e4b2-4de9-85cb-551b82b1ea7c
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: mimig
ms.openlocfilehash: 8314292cdb9b7a3f464c60119ed10f6b06ed4d10
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-table-storage-from-c"></a><span data-ttu-id="99e87-103">Postup používání úložiště Table z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="99e87-103">How to use Table storage from C++</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="99e87-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="99e87-104">Overview</span></span>
<span data-ttu-id="99e87-105">Tento průvodce vám ukáže, jak provádět běžné scénáře pomocí služby Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="99e87-105">This guide will show you how to perform common scenarios by using the Azure Table storage service.</span></span> <span data-ttu-id="99e87-106">Ukázky jsou napsané v C++ a použít [Klientská knihovna pro úložiště Azure pro jazyk C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="99e87-106">The samples are written in C++ and use the [Azure Storage Client Library for C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span></span> <span data-ttu-id="99e87-107">Pokryté scénáře zahrnují **vytváření a odstraňování tabulek** a **práce s entity tabulky**.</span><span class="sxs-lookup"><span data-stu-id="99e87-107">The scenarios covered include **creating and deleting a table** and **working with table entities**.</span></span>

> [!NOTE]
> <span data-ttu-id="99e87-108">Tato příručka se zaměřuje Klientská knihovna pro úložiště Azure pro jazyk C++ verze 1.0.0 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="99e87-108">This guide targets the Azure Storage Client Library for C++ version 1.0.0 and above.</span></span> <span data-ttu-id="99e87-109">Doporučená verze je klientská knihovna pro úložiště 2.2.0, která je k dispozici prostřednictvím [NuGet](http://www.nuget.org/packages/wastorage) nebo [Githubu](https://github.com/Azure/azure-storage-cpp/).</span><span class="sxs-lookup"><span data-stu-id="99e87-109">The recommended version is Storage Client Library 2.2.0, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](https://github.com/Azure/azure-storage-cpp/).</span></span>
> 
> 

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a><span data-ttu-id="99e87-110">Vytvoření aplikace C++</span><span class="sxs-lookup"><span data-stu-id="99e87-110">Create a C++ application</span></span>
<span data-ttu-id="99e87-111">V této příručce bude používat funkce úložiště, které lze spustit v rámci aplikace C++.</span><span class="sxs-lookup"><span data-stu-id="99e87-111">In this guide, you will use storage features that can be run within a C++ application.</span></span> <span data-ttu-id="99e87-112">V takovém případě budete muset nainstalovat Klientská knihovna pro úložiště Azure pro jazyk C++ a vytvoření účtu úložiště Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="99e87-112">To do so, you will need to install the Azure Storage Client Library for C++ and create an Azure storage account in your Azure subscription.</span></span>  

<span data-ttu-id="99e87-113">Pokud chcete nainstalovat Klientská knihovna pro úložiště Azure pro jazyk C++, můžete použít následující metody:</span><span class="sxs-lookup"><span data-stu-id="99e87-113">To install the Azure Storage Client Library for C++, you can use the following methods:</span></span>

* <span data-ttu-id="99e87-114">**Linux:** postupujte podle pokynů na [Klientská knihovna pro úložiště Azure pro C++ – soubor README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="99e87-114">**Linux:** Follow the instructions given on the [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>  
* <span data-ttu-id="99e87-115">**Windows:** v sadě Visual Studio, klikněte na tlačítko **nástroje > Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="99e87-115">**Windows:** In Visual Studio, click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="99e87-116">Pomocí následujícího příkazu do [Konzola správce balíčků NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="99e87-116">Type the following command into the [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press Enter.</span></span>  
  
     <span data-ttu-id="99e87-117">Install-Package wastorage</span><span class="sxs-lookup"><span data-stu-id="99e87-117">Install-Package wastorage</span></span>

## <a name="configure-your-application-to-access-table-storage"></a><span data-ttu-id="99e87-118">Konfigurace aplikace k přístupu k Table storage</span><span class="sxs-lookup"><span data-stu-id="99e87-118">Configure your application to access Table storage</span></span>
<span data-ttu-id="99e87-119">Přidejte následující příkazy na začátek souboru C++, ve které chcete používat službu Azure storage rozhraní API pro přístup k tabulky obsahovat:</span><span class="sxs-lookup"><span data-stu-id="99e87-119">Add the following include statements to the top of the C++ file where you want to use the Azure storage APIs to access tables:</span></span>  

```cpp
#include <was/storage_account.h>
#include <was/table.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="99e87-120">Nastavit připojovací řetězec úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="99e87-120">Set up an Azure storage connection string</span></span>
<span data-ttu-id="99e87-121">Klienta Azure storage používá připojovací řetězec úložiště k ukládání koncových bodů a pověření pro přístup ke službám dat správy.</span><span class="sxs-lookup"><span data-stu-id="99e87-121">An Azure storage client uses a storage connection string to store endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="99e87-122">Při spuštění klienta aplikace, je nutné zadat připojovací řetězec úložiště v následujícím formátu.</span><span class="sxs-lookup"><span data-stu-id="99e87-122">When running a client application, you must provide the storage connection string in the following format.</span></span> <span data-ttu-id="99e87-123">Použít název účtu úložiště a přístupový klíč úložiště pro účet úložiště uvedené v [portálu Azure](https://portal.azure.com) pro *AccountName* a *AccountKey* hodnoty.</span><span class="sxs-lookup"><span data-stu-id="99e87-123">Use the name of your storage account and the storage access key for the storage account listed in the [Azure Portal](https://portal.azure.com) for the *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="99e87-124">Informace o účtech úložiště a přístupové klávesy, najdete v části [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="99e87-124">For information on storage accounts and access keys, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="99e87-125">Tento příklad ukazuje, jak můžou deklarovat statické pole pro uložení připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="99e87-125">This example shows how you can declare a static field to hold the connection string:</span></span>  

```cpp
// Define the connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

<span data-ttu-id="99e87-126">K testování aplikace v místním počítači a systému Windows, můžete použít Azure [emulátor úložiště](../storage/common/storage-use-emulator.md) který se instaluje s [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="99e87-126">To test your application in your local Windows-based computer, you can use the Azure [storage emulator](../storage/common/storage-use-emulator.md) that is installed with the [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="99e87-127">Emulátor úložiště je nástroj, který simuluje služby Azure Blob, fronty a tabulky, které jsou k dispozici na místním vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="99e87-127">The storage emulator is a utility that simulates the Azure Blob, Queue, and Table services available on your local development machine.</span></span> <span data-ttu-id="99e87-128">Následující příklad ukazuje, jak můžou deklarovat statické pole pro uložení připojovací řetězec k vaší emulátor místního úložiště:</span><span class="sxs-lookup"><span data-stu-id="99e87-128">The following example shows how you can declare a static field to hold the connection string to your local storage emulator:</span></span>  

```cpp
// Define the connection string with Azure storage emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

<span data-ttu-id="99e87-129">Chcete-li spustit emulátoru úložiště Azure, klikněte na tlačítko **spustit** tlačítko nebo stiskněte klávesu Windows.</span><span class="sxs-lookup"><span data-stu-id="99e87-129">To start the Azure storage emulator, click the **Start** button or press the Windows key.</span></span> <span data-ttu-id="99e87-130">Začněte psát **emulátoru úložiště Azure**a potom vyberte **emulátor úložiště Microsoft Azure** ze seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="99e87-130">Begin typing **Azure Storage Emulator**, and then select **Microsoft Azure Storage Emulator** from the list of applications.</span></span>  

<span data-ttu-id="99e87-131">Následující ukázky předpokládejme, že používáte jednu z těchto dvou metod k získání připojovacího řetězce úložiště.</span><span class="sxs-lookup"><span data-stu-id="99e87-131">The following samples assume that you have used one of these two methods to get the storage connection string.</span></span>  

## <a name="retrieve-your-connection-string"></a><span data-ttu-id="99e87-132">Načtení připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="99e87-132">Retrieve your connection string</span></span>
<span data-ttu-id="99e87-133">Můžete použít **cloud_storage_account** třída představující informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="99e87-133">You can use the **cloud_storage_account** class to represent your storage account information.</span></span> <span data-ttu-id="99e87-134">Pokud chcete načíst informace o účtu úložiště z připojovacího řetězce úložiště, můžete použít metodu analýzy.</span><span class="sxs-lookup"><span data-stu-id="99e87-134">To retrieve your storage account information from the storage connection string, you can use the parse method.</span></span>

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

<span data-ttu-id="99e87-135">V dalším kroku získat odkaz na **cloud_table_client** třídy, jako umožňuje získat odkaz na objekty pro tabulky a entity, které jsou uložené v rámci služby úložiště Table.</span><span class="sxs-lookup"><span data-stu-id="99e87-135">Next, get a reference to a **cloud_table_client** class, as it lets you get reference objects for tables and entities stored within the Table storage service.</span></span> <span data-ttu-id="99e87-136">Následující kód vytvoří **cloud_table_client** objekt pomocí objektu účtu úložiště jsme načíst výše:</span><span class="sxs-lookup"><span data-stu-id="99e87-136">The following code creates a **cloud_table_client** object by using the storage account object we retrieved above:</span></span>  

```cpp
// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();
```

## <a name="create-a-table"></a><span data-ttu-id="99e87-137">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="99e87-137">Create a table</span></span>
<span data-ttu-id="99e87-138">A **cloud_table_client** objektu umožňuje získat odkaz na objekty pro tabulky a entity.</span><span class="sxs-lookup"><span data-stu-id="99e87-138">A **cloud_table_client** object lets you get reference objects for tables and entities.</span></span> <span data-ttu-id="99e87-139">Následující kód vytvoří **cloud_table_client** objektu a pomocí něj vytvořit novou tabulku.</span><span class="sxs-lookup"><span data-stu-id="99e87-139">The following code creates a **cloud_table_client** object and uses it to create a new table.</span></span>

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Retrieve a reference to a table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create the table if it doesn't exist.
table.create_if_not_exists();  
```

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="99e87-140">Přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="99e87-140">Add an entity to a table</span></span>
<span data-ttu-id="99e87-141">Pokud chcete přidat entitu do tabulky, vytvořte novou **table_entity** objektu a předejte ji do **table_operation::insert_entity**.</span><span class="sxs-lookup"><span data-stu-id="99e87-141">To add an entity to a table, create a new **table_entity** object and pass it to **table_operation::insert_entity**.</span></span> <span data-ttu-id="99e87-142">Následující kód používá jméno zákazníka jako klíč řádku a jeho příjmení jako klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="99e87-142">The following code uses the customer's first name as the row key and last name as the partition key.</span></span> <span data-ttu-id="99e87-143">Společně pak klíč oddílu a řádku entity jednoznačně identifikují entitu v tabulce.</span><span class="sxs-lookup"><span data-stu-id="99e87-143">Together, an entity's partition and row key uniquely identify the entity in the table.</span></span> <span data-ttu-id="99e87-144">Entity se stejným klíčem oddílu můžete zadat dotaz rychleji než ty, které se různé klíče oddílů, ale používání různých klíčů oddílů umožňuje větší škálovatelnost paralelních operaci.</span><span class="sxs-lookup"><span data-stu-id="99e87-144">Entities with the same partition key can be queried faster than those with different partition keys, but using diverse partition keys allows for greater parallel operation scalability.</span></span> <span data-ttu-id="99e87-145">Další informace najdete v tématu [Microsoft Azure storage výkon a škálovatelnost kontrolní seznam](../storage/common/storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="99e87-145">For more information, see [Microsoft Azure storage performance and scalability checklist](../storage/common/storage-performance-checklist.md).</span></span>

<span data-ttu-id="99e87-146">Následující kód vytvoří novou instanci třídy **table_entity** s některé zákaznické údaje uložit.</span><span class="sxs-lookup"><span data-stu-id="99e87-146">The following code creates a new instance of **table_entity** with some customer data to be stored.</span></span> <span data-ttu-id="99e87-147">Další kód zavolá metodu **table_operation::insert_entity** k vytvoření **table_operation** objektu pro vložení entity do tabulky a přidruží novou entitu tabulky.</span><span class="sxs-lookup"><span data-stu-id="99e87-147">The code next calls **table_operation::insert_entity** to create a **table_operation** object to insert an entity into a table, and associates the new table entity with it.</span></span> <span data-ttu-id="99e87-148">Nakonec kód volá metodu execute na **cloud_table** objektu.</span><span class="sxs-lookup"><span data-stu-id="99e87-148">Finally, the code calls the execute method on the **cloud_table** object.</span></span> <span data-ttu-id="99e87-149">A nové **table_operation** odešle požadavek do služby Table pro vložení nové entity zákazníka do tabulky "lidé".</span><span class="sxs-lookup"><span data-stu-id="99e87-149">And the new **table_operation** sends a request to the Table service to insert the new customer entity into the "people" table.</span></span>  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Retrieve a reference to a table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create the table if it doesn't exist.
table.create_if_not_exists();

// Create a new customer entity.
azure::storage::table_entity customer1(U("Harp"), U("Walter"));

azure::storage::table_entity::properties_type& properties = customer1.properties();
properties.reserve(2);
properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

// Create the table operation that inserts the customer entity.
azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

// Execute the insert operation.
azure::storage::table_result insert_result = table.execute(insert_operation);
```

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="99e87-150">Vložení dávky entit</span><span class="sxs-lookup"><span data-stu-id="99e87-150">Insert a batch of entities</span></span>
<span data-ttu-id="99e87-151">V rámci jedné operace zápisu můžete vložit dávku entit do služby Table.</span><span class="sxs-lookup"><span data-stu-id="99e87-151">You can insert a batch of entities to the Table service in one write operation.</span></span> <span data-ttu-id="99e87-152">Následující kód vytvoří **table_batch_operation** objektu a potom se přidají tři operace k němu vložení.</span><span class="sxs-lookup"><span data-stu-id="99e87-152">The following code creates a **table_batch_operation** object, and then adds three insert operations to it.</span></span> <span data-ttu-id="99e87-153">Každé operace insert přidá se při vytvoření nového objektu entity, nastavení jeho hodnoty a potom volání metody insert u **table_batch_operation** objekt, který chcete přidružit nové operace insert entity.</span><span class="sxs-lookup"><span data-stu-id="99e87-153">Each insert operation is added by creating a new entity object, setting its values, and then calling the insert method on the **table_batch_operation** object to associate the entity with a new insert operation.</span></span> <span data-ttu-id="99e87-154">Potom **cloud_table.execute** je volána k provedení operace.</span><span class="sxs-lookup"><span data-stu-id="99e87-154">Then, **cloud_table.execute** is called to execute the operation.</span></span>  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Define a batch operation.
azure::storage::table_batch_operation batch_operation;

// Create a customer entity and add it to the table.
azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

azure::storage::table_entity::properties_type& properties1 = customer1.properties();
properties1.reserve(2);
properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

// Create another customer entity and add it to the table.
azure::storage::table_entity customer2(U("Smith"), U("Ben"));

azure::storage::table_entity::properties_type& properties2 = customer2.properties();
properties2.reserve(2);
properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

// Create a third customer entity to add to the table.
azure::storage::table_entity customer3(U("Smith"), U("Denise"));

azure::storage::table_entity::properties_type& properties3 = customer3.properties();
properties3.reserve(2);
properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

// Add customer entities to the batch insert operation.
batch_operation.insert_or_replace_entity(customer1);
batch_operation.insert_or_replace_entity(customer2);
batch_operation.insert_or_replace_entity(customer3);

// Execute the batch operation.
std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);
```

<span data-ttu-id="99e87-155">Některé věci poznamenat ohledně dávkových operací:</span><span class="sxs-lookup"><span data-stu-id="99e87-155">Some things to note on batch operations:</span></span>  

* <span data-ttu-id="99e87-156">Až 100 vložení, odstranění, sloučení, nahraďte, operace insert nebo sloučení a vložení nebo nahrazení můžete provádět v libovolné kombinace v jedné dávce.</span><span class="sxs-lookup"><span data-stu-id="99e87-156">You can perform up to 100 insert, delete, merge, replace, insert-or-merge, and insert-or-replace operations in any combination in a single batch.</span></span>  
* <span data-ttu-id="99e87-157">Dávková operace může mít operaci načtení, pokud je ale jediná operace v dávce.</span><span class="sxs-lookup"><span data-stu-id="99e87-157">A batch operation can have a retrieve operation, if it is the only operation in the batch.</span></span>  
* <span data-ttu-id="99e87-158">Všechny entity v jedné dávkové operaci musí mít stejný klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="99e87-158">All entities in a single batch operation must have the same partition key.</span></span>  
* <span data-ttu-id="99e87-159">Dávkové operace je omezena na 4 MB datovou částí.</span><span class="sxs-lookup"><span data-stu-id="99e87-159">A batch operation is limited to a 4-MB data payload.</span></span>  

## <a name="retrieve-all-entities-in-a-partition"></a><span data-ttu-id="99e87-160">Načtení všech entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="99e87-160">Retrieve all entities in a partition</span></span>
<span data-ttu-id="99e87-161">Dotaz na tabulku pro všechny entity v oddílu, použijte **table_query** objektu.</span><span class="sxs-lookup"><span data-stu-id="99e87-161">To query a table for all entities in a partition, use a **table_query** object.</span></span> <span data-ttu-id="99e87-162">Následující příklad kódu určuje filtr pro entity, kde Smith je klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="99e87-162">The following code example specifies a filter for entities where 'Smith' is the partition key.</span></span> <span data-ttu-id="99e87-163">Tento příklad zobrazí pole každé entity z výsledků dotazu z konzoly.</span><span class="sxs-lookup"><span data-stu-id="99e87-163">This example prints the fields of each entity in the query results to the console.</span></span>  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Construct the query operation for all customer entities where PartitionKey="Smith".
azure::storage::table_query query;

query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

// Execute the query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Print the fields for each customer.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    const azure::storage::table_entity::properties_type& properties = it->properties();

    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
}  
```

<span data-ttu-id="99e87-164">V tomto příkladu dotaz zobrazí všechny entity, které odpovídají kritériím filtru.</span><span class="sxs-lookup"><span data-stu-id="99e87-164">The query in this example brings all the entities that match the filter criteria.</span></span> <span data-ttu-id="99e87-165">Pokud máte velké tabulky a potřebovat stáhnout entity tabulky často, doporučujeme, aby data ukládáte do objektů BLOB Azure storage místo.</span><span class="sxs-lookup"><span data-stu-id="99e87-165">If you have large tables and need to download the table entities often, we recommend that you store your data in Azure storage blobs instead.</span></span>

## <a name="retrieve-a-range-of-entities-in-a-partition"></a><span data-ttu-id="99e87-166">Načtení rozsahu entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="99e87-166">Retrieve a range of entities in a partition</span></span>
<span data-ttu-id="99e87-167">Pokud nechcete, aby se zadával dotaz na všechny entity v oddílu, můžete zadat rozsah nakombinováním filtru klíče oddílu s filtrem klíče řádku.</span><span class="sxs-lookup"><span data-stu-id="99e87-167">If you don't want to query all the entities in a partition, you can specify a range by combining the partition key filter with a row key filter.</span></span> <span data-ttu-id="99e87-168">Následující příklad kódu používá dva filtry k získání všech entit v oddílu Smith, kde klíč řádku (jméno) začíná písmenem abecedy před písmenem E, a potom zobrazí výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="99e87-168">The following code example uses two filters to get all entities in partition 'Smith' where the row key (first name) starts with a letter earlier than 'E' in the alphabet and then prints the query results.</span></span>  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create the table query.
azure::storage::table_query query;

query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
    azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
    azure::storage::query_comparison_operator::equal, U("Smith")),
    azure::storage::query_logical_operator::op_and,
    azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

// Execute the query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Loop through the results, displaying information about the entity.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    const azure::storage::table_entity::properties_type& properties = it->properties();

    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
}  
```

## <a name="retrieve-a-single-entity"></a><span data-ttu-id="99e87-169">Načtení jedné entity</span><span class="sxs-lookup"><span data-stu-id="99e87-169">Retrieve a single entity</span></span>
<span data-ttu-id="99e87-170">Můžete napsat dotaz pro načtení jedné konkrétní entity.</span><span class="sxs-lookup"><span data-stu-id="99e87-170">You can write a query to retrieve a single, specific entity.</span></span> <span data-ttu-id="99e87-171">Následující kód používá **table_operation::retrieve_entity** k určení zákazníka 'Jeff Smith'.</span><span class="sxs-lookup"><span data-stu-id="99e87-171">The following code uses **table_operation::retrieve_entity** to specify the customer 'Jeff Smith'.</span></span> <span data-ttu-id="99e87-172">Tato metoda vrátí místo kolekce pouze jednu entitu a vrácená hodnota v **table_result**.</span><span class="sxs-lookup"><span data-stu-id="99e87-172">This method returns just one entity, rather than a collection, and the returned value is in **table_result**.</span></span> <span data-ttu-id="99e87-173">Určení jak klíčů oddílu, tak klíčů řádků v dotazu představuje nejrychlejší způsob, jak načíst jednu entitu ze služby Table.</span><span class="sxs-lookup"><span data-stu-id="99e87-173">Specifying both partition and row keys in a query is the fastest way to retrieve a single entity from the Table service.</span></span>  

```cpp
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Retrieve the entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Output the entity.
azure::storage::table_entity entity = retrieve_result.entity();
const azure::storage::table_entity::properties_type& properties = entity.properties();

std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
    << U(", Property1: ") << properties.at(U("Email")).string_value()
    << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
```

## <a name="replace-an-entity"></a><span data-ttu-id="99e87-174">Nahrazení entity</span><span class="sxs-lookup"><span data-stu-id="99e87-174">Replace an entity</span></span>
<span data-ttu-id="99e87-175">Jako náhrada za entitu, načtěte ji ze služby Table, upravte objekt entity a potom uložte změny zpět do služby Table.</span><span class="sxs-lookup"><span data-stu-id="99e87-175">To replace an entity, retrieve it from the Table service, modify the entity object, and then save the changes back to the Table service.</span></span> <span data-ttu-id="99e87-176">Následující kód změní stávající zákazník telefonní číslo a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="99e87-176">The following code changes an existing customer's phone number and email address.</span></span> <span data-ttu-id="99e87-177">Místo volání **table_operation::insert_entity**, tento kód používá **table_operation::replace_entity**.</span><span class="sxs-lookup"><span data-stu-id="99e87-177">Instead of calling **table_operation::insert_entity**, this code uses **table_operation::replace_entity**.</span></span> <span data-ttu-id="99e87-178">To způsobí, že entita se na serveru plně nahradí, pokud se entita na serveru od načtení nezměnila, protože v takovém případě se operace nezdaří.</span><span class="sxs-lookup"><span data-stu-id="99e87-178">This causes the entity to be fully replaced on the server, unless the entity on the server has changed since it was retrieved, in which case the operation will fail.</span></span> <span data-ttu-id="99e87-179">Toto selhání zabrání vaší aplikaci v nechtěném přepsání změny provedené mezi načtením a aktualizací provedenou jinou součástí vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="99e87-179">This failure is to prevent your application from inadvertently overwriting a change made between the retrieval and update by another component of your application.</span></span> <span data-ttu-id="99e87-180">Správné zpracování této chyby je, že entitu znovu načtete, provedené změny (Pokud je stále platný) a pak provedete další **table_operation::replace_entity** operaci.</span><span class="sxs-lookup"><span data-stu-id="99e87-180">The proper handling of this failure is to retrieve the entity again, make your changes (if still valid), and then perform another **table_operation::replace_entity** operation.</span></span> <span data-ttu-id="99e87-181">V další části si ukážeme, jak toto chování potlačit.</span><span class="sxs-lookup"><span data-stu-id="99e87-181">The next section will show you how to override this behavior.</span></span>  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Replace an entity.
azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
properties_to_replace.reserve(2);

// Specify a new phone number.
properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

// Specify a new email address.
properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

// Create an operation to replace the entity.
azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

// Submit the operation to the Table service.
azure::storage::table_result replace_result = table.execute(replace_operation);
```

## <a name="insert-or-replace-an-entity"></a><span data-ttu-id="99e87-182">Vložení nebo nahrazení entity</span><span class="sxs-lookup"><span data-stu-id="99e87-182">Insert-or-replace an entity</span></span>
<span data-ttu-id="99e87-183">**table_operation::replace_entity** operace se nezdaří, pokud byl změněn entita od načtení ze serveru.</span><span class="sxs-lookup"><span data-stu-id="99e87-183">**table_operation::replace_entity** operations will fail if the entity has been changed since it was retrieved from the server.</span></span> <span data-ttu-id="99e87-184">Kromě toho musí načtení entity ze serveru první v pořadí pro **table_operation::replace_entity** úspěšný.</span><span class="sxs-lookup"><span data-stu-id="99e87-184">Furthermore, you must retrieve the entity from the server first in order for **table_operation::replace_entity** to be successful.</span></span> <span data-ttu-id="99e87-185">V některých případech ale nevíte, pokud entita existuje na serveru a aktuální hodnoty v ní uloženy jsou důležité – vaše aktualizace by je měla všechny přepsat.</span><span class="sxs-lookup"><span data-stu-id="99e87-185">Sometimes, however, you don't know if the entity exists on the server and the current values stored in it are irrelevant—your update should overwrite them all.</span></span> <span data-ttu-id="99e87-186">Chcete-li dosáhnout, použijte **table_operation::insert_or_replace_entity** operaci.</span><span class="sxs-lookup"><span data-stu-id="99e87-186">To accomplish this, you would use a **table_operation::insert_or_replace_entity** operation.</span></span> <span data-ttu-id="99e87-187">Tato operace vloží entitu, pokud neexistuje, nebo ji nahradí, pokud existuje, a to bez ohledu na to, kdy byla provedena poslední aktualizace.</span><span class="sxs-lookup"><span data-stu-id="99e87-187">This operation inserts the entity if it doesn't exist, or replaces it if it does, regardless of when the last update was made.</span></span> <span data-ttu-id="99e87-188">V následujícím příkladu kódu je entita zákazník Jeff Smith stále načtena, ale pak je uložena zpět na server prostřednictvím **table_operation::insert_or_replace_entity**.</span><span class="sxs-lookup"><span data-stu-id="99e87-188">In the following code example, the customer entity for Jeff Smith is still retrieved, but it is then saved back to the server via **table_operation::insert_or_replace_entity**.</span></span> <span data-ttu-id="99e87-189">Jakékoli aktualizace provedené v entitě mezi načtením a operace aktualizace budou přepsány.</span><span class="sxs-lookup"><span data-stu-id="99e87-189">Any updates made to the entity between the retrieval and update operation will be overwritten.</span></span>  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Insert-or-replace an entity.
azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

properties_to_insert_or_replace.reserve(2);

// Specify a phone number.
properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

// Specify an email address.
properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

// Create an operation to insert-or-replace the entity.
azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

// Submit the operation to the Table service.
azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);
```

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="99e87-190">Dotaz na podmnožinu vlastností entity</span><span class="sxs-lookup"><span data-stu-id="99e87-190">Query a subset of entity properties</span></span>
<span data-ttu-id="99e87-191">Dotaz na tabulku můžete načíst jenom několik z entity.</span><span class="sxs-lookup"><span data-stu-id="99e87-191">A query to a table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="99e87-192">Dotaz v následující kód používá **table_query::set_select_columns** metoda vrátí pouze e-mailové adresy entit v tabulce.</span><span class="sxs-lookup"><span data-stu-id="99e87-192">The query in the following code uses the **table_query::set_select_columns** method to return only the email addresses of entities in the table.</span></span>  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Define the query, and select only the Email property.
azure::storage::table_query query;
std::vector<utility::string_t> columns;

columns.push_back(U("Email"));
query.set_select_columns(columns);

// Execute the query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Display the results.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

    const azure::storage::table_entity::properties_type& properties = it->properties();
    for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
    {
        std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
    }

    std::wcout << std::endl;
}
```

> [!NOTE]
> <span data-ttu-id="99e87-193">Dotazování několik vlastností z entity, je efektivnější operace než načítání všechny vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="99e87-193">Querying a few properties from an entity is a more efficient operation than retrieving all properties.</span></span>
> 
> 

## <a name="delete-an-entity"></a><span data-ttu-id="99e87-194">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="99e87-194">Delete an entity</span></span>
<span data-ttu-id="99e87-195">Entitu můžete snadno odstranit po jejím načtení.</span><span class="sxs-lookup"><span data-stu-id="99e87-195">You can easily delete an entity after you have retrieved it.</span></span> <span data-ttu-id="99e87-196">Jakmile se načíst entity, volání **table_operation::delete_entity** s entity, která má odstranit.</span><span class="sxs-lookup"><span data-stu-id="99e87-196">Once the entity is retrieved, call **table_operation::delete_entity** with the entity to delete.</span></span> <span data-ttu-id="99e87-197">Potom zavolejte **cloud_table.execute** metoda.</span><span class="sxs-lookup"><span data-stu-id="99e87-197">Then call the **cloud_table.execute** method.</span></span> <span data-ttu-id="99e87-198">Následující kód načte a odstraní entitu s klíčem oddílu pro "Smith" a klíč řádku z "Jan".</span><span class="sxs-lookup"><span data-stu-id="99e87-198">The following code retrieves and deletes an entity with a partition key of "Smith" and a row key of "Jeff".</span></span>  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Create an operation to delete the entity.
azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

// Submit the delete operation to the Table service.
azure::storage::table_result delete_result = table.execute(delete_operation);  
```

## <a name="delete-a-table"></a><span data-ttu-id="99e87-199">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="99e87-199">Delete a table</span></span>
<span data-ttu-id="99e87-200">Následující příklad kódu nakonec odstraní tabulku z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="99e87-200">Finally, the following code example deletes a table from a storage account.</span></span> <span data-ttu-id="99e87-201">Tabulku, která byla odstraněna, nebude možné po odstranění nějakou dobu znovu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="99e87-201">A table that has been deleted will be unavailable to be re-created for a period of time following the deletion.</span></span>  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Create an operation to delete the entity.
azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

// Submit the delete operation to the Table service.
azure::storage::table_result delete_result = table.execute(delete_operation);
```

## <a name="next-steps"></a><span data-ttu-id="99e87-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="99e87-202">Next steps</span></span>
<span data-ttu-id="99e87-203">Teď, když jste se naučili základy používání služby table storage, postupujte podle následujících odkazech na další informace o službě Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="99e87-203">Now that you've learned the basics of table storage, follow these links to learn more about Azure Storage:</span></span>  

* <span data-ttu-id="99e87-204">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.</span><span class="sxs-lookup"><span data-stu-id="99e87-204">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* [<span data-ttu-id="99e87-205">Používání úložiště Blob z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="99e87-205">How to use Blob storage from C++</span></span>](../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="99e87-206">Postup používání úložiště Queue z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="99e87-206">How to use Queue storage from C++</span></span>](../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
* [<span data-ttu-id="99e87-207">Seznam prostředků úložiště Azure v jazyce C++</span><span class="sxs-lookup"><span data-stu-id="99e87-207">List Azure Storage resources in C++</span></span>](../storage/common/storage-c-plus-plus-enumeration.md)
* [<span data-ttu-id="99e87-208">Klientská knihovna pro úložiště pro C++ – referenční informace</span><span class="sxs-lookup"><span data-stu-id="99e87-208">Storage Client Library for C++ reference</span></span>](http://azure.github.io/azure-storage-cpp)
* [<span data-ttu-id="99e87-209">Dokumentace k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="99e87-209">Azure Storage documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)
