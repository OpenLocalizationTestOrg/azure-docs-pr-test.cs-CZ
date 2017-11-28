---
title: aaaHow toouse Table storage (C++) | Microsoft Docs
description: "Ukládejte si strukturovaná data v cloudu hello pomocí Azure Table storage, úložiště dat typu NoSQL."
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
ms.openlocfilehash: 8096fe531427ba4858f7f4cb7f664f941892d1c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-c"></a><span data-ttu-id="5458a-103">Jak toouse úložiště Table z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="5458a-103">How toouse Table storage from C++</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="5458a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="5458a-104">Overview</span></span>
<span data-ttu-id="5458a-105">Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="5458a-105">This guide will show you how tooperform common scenarios by using hello Azure Table storage service.</span></span> <span data-ttu-id="5458a-106">Hello ukázky jsou napsané v jazyce C++ a používají hello [Klientská knihovna pro úložiště Azure pro jazyk C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="5458a-106">hello samples are written in C++ and use hello [Azure Storage Client Library for C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span></span> <span data-ttu-id="5458a-107">Hello pokryté scénáře zahrnují **vytváření a odstraňování tabulek** a **práce s entity tabulky**.</span><span class="sxs-lookup"><span data-stu-id="5458a-107">hello scenarios covered include **creating and deleting a table** and **working with table entities**.</span></span>

> [!NOTE]
> <span data-ttu-id="5458a-108">Tato příručka cílí hello Klientská knihovna pro úložiště Azure pro jazyk C++ verze 1.0.0 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="5458a-108">This guide targets hello Azure Storage Client Library for C++ version 1.0.0 and above.</span></span> <span data-ttu-id="5458a-109">Hello doporučená verze je klientská knihovna pro úložiště 2.2.0, která je k dispozici prostřednictvím [NuGet](http://www.nuget.org/packages/wastorage) nebo [Githubu](https://github.com/Azure/azure-storage-cpp/).</span><span class="sxs-lookup"><span data-stu-id="5458a-109">hello recommended version is Storage Client Library 2.2.0, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](https://github.com/Azure/azure-storage-cpp/).</span></span>
> 
> 

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a><span data-ttu-id="5458a-110">Vytvoření aplikace C++</span><span class="sxs-lookup"><span data-stu-id="5458a-110">Create a C++ application</span></span>
<span data-ttu-id="5458a-111">V této příručce bude používat funkce úložiště, které lze spustit v rámci aplikace C++.</span><span class="sxs-lookup"><span data-stu-id="5458a-111">In this guide, you will use storage features that can be run within a C++ application.</span></span> <span data-ttu-id="5458a-112">toodo tedy budete potřebovat tooinstall hello Klientská knihovna pro úložiště Azure pro jazyk C++ a vytvoření účtu úložiště Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="5458a-112">toodo so, you will need tooinstall hello Azure Storage Client Library for C++ and create an Azure storage account in your Azure subscription.</span></span>  

<span data-ttu-id="5458a-113">tooinstall hello Klientská knihovna pro úložiště Azure pro jazyk C++, můžete použít následující metody hello:</span><span class="sxs-lookup"><span data-stu-id="5458a-113">tooinstall hello Azure Storage Client Library for C++, you can use hello following methods:</span></span>

* <span data-ttu-id="5458a-114">**Linux:** postupujte podle pokynů hello na hello [Klientská knihovna pro úložiště Azure pro C++ – soubor README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="5458a-114">**Linux:** Follow hello instructions given on hello [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>  
* <span data-ttu-id="5458a-115">**Windows:** v sadě Visual Studio, klikněte na tlačítko **nástroje > Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="5458a-115">**Windows:** In Visual Studio, click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="5458a-116">Typ hello následující příkaz do hello [Konzola správce balíčků NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="5458a-116">Type hello following command into hello [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press Enter.</span></span>  
  
     <span data-ttu-id="5458a-117">Install-Package wastorage</span><span class="sxs-lookup"><span data-stu-id="5458a-117">Install-Package wastorage</span></span>

## <a name="configure-your-application-tooaccess-table-storage"></a><span data-ttu-id="5458a-118">Konfigurace vaší aplikace tooaccess úložiště Table</span><span class="sxs-lookup"><span data-stu-id="5458a-118">Configure your application tooaccess Table storage</span></span>
<span data-ttu-id="5458a-119">Přidáte následující hello obsahovat toohello příkazy na začátek souboru C++ hello místo toouse hello úložiště Azure rozhraní API tooaccess tabulky:</span><span class="sxs-lookup"><span data-stu-id="5458a-119">Add hello following include statements toohello top of hello C++ file where you want toouse hello Azure storage APIs tooaccess tables:</span></span>  

```cpp
#include <was/storage_account.h>
#include <was/table.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="5458a-120">Nastavit připojovací řetězec úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="5458a-120">Set up an Azure storage connection string</span></span>
<span data-ttu-id="5458a-121">Klienta Azure storage používá úložiště připojovací řetězec toostore koncových bodů a přihlašovací údaje pro přístup ke službám dat správy.</span><span class="sxs-lookup"><span data-stu-id="5458a-121">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="5458a-122">Při spuštění klienta aplikace, je nutné zadat připojovací řetězec úložiště hello v hello formátu.</span><span class="sxs-lookup"><span data-stu-id="5458a-122">When running a client application, you must provide hello storage connection string in hello following format.</span></span> <span data-ttu-id="5458a-123">Použití hello název účtu a hello úložiště přístupový klíč k úložišti pro účet úložiště hello uvedené v hello [portálu Azure](https://portal.azure.com) pro hello *AccountName* a *AccountKey* hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5458a-123">Use hello name of your storage account and hello storage access key for hello storage account listed in hello [Azure Portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="5458a-124">Informace o účtech úložiště a přístupové klávesy, najdete v části [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="5458a-124">For information on storage accounts and access keys, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="5458a-125">Tento příklad ukazuje, jak můžou deklarovat statické pole toohold hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="5458a-125">This example shows how you can declare a static field toohold hello connection string:</span></span>  

```cpp
// Define hello connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

<span data-ttu-id="5458a-126">tootest aplikace ve vaší místní počítače se systémem Windows, můžete použít hello Azure [emulátor úložiště](../storage/common/storage-use-emulator.md) který se instaluje s hello [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="5458a-126">tootest your application in your local Windows-based computer, you can use hello Azure [storage emulator](../storage/common/storage-use-emulator.md) that is installed with hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="5458a-127">emulátor úložiště Hello je nástroj, který simuluje hello Azure Blob, Queue a Table služeb dostupných na počítači pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="5458a-127">hello storage emulator is a utility that simulates hello Azure Blob, Queue, and Table services available on your local development machine.</span></span> <span data-ttu-id="5458a-128">Hello následující příklad ukazuje, jak lze deklarovat statické pole toohold hello připojovací řetězec tooyour emulátor místního úložiště:</span><span class="sxs-lookup"><span data-stu-id="5458a-128">hello following example shows how you can declare a static field toohold hello connection string tooyour local storage emulator:</span></span>  

```cpp
// Define hello connection string with Azure storage emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

<span data-ttu-id="5458a-129">toostart hello emulátoru úložiště Azure, klikněte na tlačítko hello **spustit** tlačítko nebo stiskněte klávesu Windows hello.</span><span class="sxs-lookup"><span data-stu-id="5458a-129">toostart hello Azure storage emulator, click hello **Start** button or press hello Windows key.</span></span> <span data-ttu-id="5458a-130">Začněte psát **emulátoru úložiště Azure**a potom vyberte **emulátor úložiště Microsoft Azure** hello seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="5458a-130">Begin typing **Azure Storage Emulator**, and then select **Microsoft Azure Storage Emulator** from hello list of applications.</span></span>  

<span data-ttu-id="5458a-131">Hello následující ukázky předpokládejme použít jednu z těchto dvou metod tooget hello úložiště připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="5458a-131">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>  

## <a name="retrieve-your-connection-string"></a><span data-ttu-id="5458a-132">Načtení připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="5458a-132">Retrieve your connection string</span></span>
<span data-ttu-id="5458a-133">Můžete použít hello **cloud_storage_account** třídy toorepresent informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="5458a-133">You can use hello **cloud_storage_account** class toorepresent your storage account information.</span></span> <span data-ttu-id="5458a-134">tooretrieve účet úložiště informací z připojovacího řetězce úložiště hello, můžete použít metodu analýzy hello.</span><span class="sxs-lookup"><span data-stu-id="5458a-134">tooretrieve your storage account information from hello storage connection string, you can use hello parse method.</span></span>

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

<span data-ttu-id="5458a-135">V dalším kroku získat odkaz na tooa **cloud_table_client** třídy, protože umožňuje získat odkaz na objekty pro tabulky a entity uložené v rámci hello tabulka úložiště služby.</span><span class="sxs-lookup"><span data-stu-id="5458a-135">Next, get a reference tooa **cloud_table_client** class, as it lets you get reference objects for tables and entities stored within hello Table storage service.</span></span> <span data-ttu-id="5458a-136">Hello následující kód vytvoří **cloud_table_client** objekt pomocí objektu účtu úložiště hello nemůžeme načíst výše:</span><span class="sxs-lookup"><span data-stu-id="5458a-136">hello following code creates a **cloud_table_client** object by using hello storage account object we retrieved above:</span></span>  

```cpp
// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();
```

## <a name="create-a-table"></a><span data-ttu-id="5458a-137">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="5458a-137">Create a table</span></span>
<span data-ttu-id="5458a-138">A **cloud_table_client** objektu umožňuje získat odkaz na objekty pro tabulky a entity.</span><span class="sxs-lookup"><span data-stu-id="5458a-138">A **cloud_table_client** object lets you get reference objects for tables and entities.</span></span> <span data-ttu-id="5458a-139">Hello následující kód vytvoří **cloud_table_client** objektu a používá je toocreate novou tabulku.</span><span class="sxs-lookup"><span data-stu-id="5458a-139">hello following code creates a **cloud_table_client** object and uses it toocreate a new table.</span></span>

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Retrieve a reference tooa table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create hello table if it doesn't exist.
table.create_if_not_exists();  
```

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="5458a-140">Přidat tooa tabulka entity</span><span class="sxs-lookup"><span data-stu-id="5458a-140">Add an entity tooa table</span></span>
<span data-ttu-id="5458a-141">tooadd tooa tabulka entity, vytvořte novou **table_entity** objektu a předejte ji příliš**table_operation::insert_entity**.</span><span class="sxs-lookup"><span data-stu-id="5458a-141">tooadd an entity tooa table, create a new **table_entity** object and pass it too**table_operation::insert_entity**.</span></span> <span data-ttu-id="5458a-142">Hello následující kód používá jméno hello zákazníka jako klíč řádku hello a příjmení jako klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="5458a-142">hello following code uses hello customer's first name as hello row key and last name as hello partition key.</span></span> <span data-ttu-id="5458a-143">Společně oddílu a klíč řádku jednoznačně hello entity v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="5458a-143">Together, an entity's partition and row key uniquely identify hello entity in hello table.</span></span> <span data-ttu-id="5458a-144">Entity s hello, které se stejným klíčem oddílu můžete zadat dotaz rychleji než ty, které jiné oddílu klíče, ale používání různých klíčů oddílů umožňuje větší škálovatelnost paralelních operaci.</span><span class="sxs-lookup"><span data-stu-id="5458a-144">Entities with hello same partition key can be queried faster than those with different partition keys, but using diverse partition keys allows for greater parallel operation scalability.</span></span> <span data-ttu-id="5458a-145">Další informace najdete v tématu [Microsoft Azure storage výkon a škálovatelnost kontrolní seznam](../storage/common/storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="5458a-145">For more information, see [Microsoft Azure storage performance and scalability checklist](../storage/common/storage-performance-checklist.md).</span></span>

<span data-ttu-id="5458a-146">Hello následující kód vytvoří novou instanci třídy **table_entity** s některé toobe dat zákazníka uložené.</span><span class="sxs-lookup"><span data-stu-id="5458a-146">hello following code creates a new instance of **table_entity** with some customer data toobe stored.</span></span> <span data-ttu-id="5458a-147">Hello kód zavolá metodu Další **table_operation::insert_entity** toocreate **table_operation** objektu tooinsert entity do tabulky a partnerů hello novou entitu tabulky s ním.</span><span class="sxs-lookup"><span data-stu-id="5458a-147">hello code next calls **table_operation::insert_entity** toocreate a **table_operation** object tooinsert an entity into a table, and associates hello new table entity with it.</span></span> <span data-ttu-id="5458a-148">Nakonec hello kód zavolá metodu hello provést metodu hello **cloud_table** objektu.</span><span class="sxs-lookup"><span data-stu-id="5458a-148">Finally, hello code calls hello execute method on hello **cloud_table** object.</span></span> <span data-ttu-id="5458a-149">A hello nové **table_operation** odešle žádost toohello tabulky služby tooinsert hello nové zákazníka entity do tabulky "lidé" hello.</span><span class="sxs-lookup"><span data-stu-id="5458a-149">And hello new **table_operation** sends a request toohello Table service tooinsert hello new customer entity into hello "people" table.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Retrieve a reference tooa table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create hello table if it doesn't exist.
table.create_if_not_exists();

// Create a new customer entity.
azure::storage::table_entity customer1(U("Harp"), U("Walter"));

azure::storage::table_entity::properties_type& properties = customer1.properties();
properties.reserve(2);
properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

// Create hello table operation that inserts hello customer entity.
azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

// Execute hello insert operation.
azure::storage::table_result insert_result = table.execute(insert_operation);
```

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="5458a-150">Vložení dávky entit</span><span class="sxs-lookup"><span data-stu-id="5458a-150">Insert a batch of entities</span></span>
<span data-ttu-id="5458a-151">V rámci jedné operace zápisu můžete vložit dávku entit toohello služby Table.</span><span class="sxs-lookup"><span data-stu-id="5458a-151">You can insert a batch of entities toohello Table service in one write operation.</span></span> <span data-ttu-id="5458a-152">Hello následující kód vytvoří **table_batch_operation** objektu a potom se přidají tři vložit tooit operace.</span><span class="sxs-lookup"><span data-stu-id="5458a-152">hello following code creates a **table_batch_operation** object, and then adds three insert operations tooit.</span></span> <span data-ttu-id="5458a-153">Každé operace insert přidá se při vytvoření nového objektu entity, nastavení jeho hodnoty, a pak volání hello vložit metodu hello **table_batch_operation** objekt tooassociate hello entita s novou Vložit operaci.</span><span class="sxs-lookup"><span data-stu-id="5458a-153">Each insert operation is added by creating a new entity object, setting its values, and then calling hello insert method on hello **table_batch_operation** object tooassociate hello entity with a new insert operation.</span></span> <span data-ttu-id="5458a-154">Potom **cloud_table.execute** nazývá tooexecute hello operaci.</span><span class="sxs-lookup"><span data-stu-id="5458a-154">Then, **cloud_table.execute** is called tooexecute hello operation.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Define a batch operation.
azure::storage::table_batch_operation batch_operation;

// Create a customer entity and add it toohello table.
azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

azure::storage::table_entity::properties_type& properties1 = customer1.properties();
properties1.reserve(2);
properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

// Create another customer entity and add it toohello table.
azure::storage::table_entity customer2(U("Smith"), U("Ben"));

azure::storage::table_entity::properties_type& properties2 = customer2.properties();
properties2.reserve(2);
properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

// Create a third customer entity tooadd toohello table.
azure::storage::table_entity customer3(U("Smith"), U("Denise"));

azure::storage::table_entity::properties_type& properties3 = customer3.properties();
properties3.reserve(2);
properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

// Add customer entities toohello batch insert operation.
batch_operation.insert_or_replace_entity(customer1);
batch_operation.insert_or_replace_entity(customer2);
batch_operation.insert_or_replace_entity(customer3);

// Execute hello batch operation.
std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);
```

<span data-ttu-id="5458a-155">Některé věci toonote ohledně dávkových operací:</span><span class="sxs-lookup"><span data-stu-id="5458a-155">Some things toonote on batch operations:</span></span>  

* <span data-ttu-id="5458a-156">Můžete provést až too100 vložení, odstranění, sloučení, nahraďte, operace insert nebo sloučení a vložení nebo nahrazení v libovolné kombinace v jedné dávce.</span><span class="sxs-lookup"><span data-stu-id="5458a-156">You can perform up too100 insert, delete, merge, replace, insert-or-merge, and insert-or-replace operations in any combination in a single batch.</span></span>  
* <span data-ttu-id="5458a-157">Dávková operace může mít operaci načtení, pokud je hello jediná operace v dávce hello.</span><span class="sxs-lookup"><span data-stu-id="5458a-157">A batch operation can have a retrieve operation, if it is hello only operation in hello batch.</span></span>  
* <span data-ttu-id="5458a-158">Všechny entity v jedné dávkové operaci musí mít hello stejným klíčem oddílu.</span><span class="sxs-lookup"><span data-stu-id="5458a-158">All entities in a single batch operation must have hello same partition key.</span></span>  
* <span data-ttu-id="5458a-159">Dávkové operace je omezená tooa 4 MB datovou částí.</span><span class="sxs-lookup"><span data-stu-id="5458a-159">A batch operation is limited tooa 4-MB data payload.</span></span>  

## <a name="retrieve-all-entities-in-a-partition"></a><span data-ttu-id="5458a-160">Načtení všech entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="5458a-160">Retrieve all entities in a partition</span></span>
<span data-ttu-id="5458a-161">tooquery tabulku pro všechny entity v oddílu, použití **table_query** objektu.</span><span class="sxs-lookup"><span data-stu-id="5458a-161">tooquery a table for all entities in a partition, use a **table_query** object.</span></span> <span data-ttu-id="5458a-162">Hello následující příklad kódu určuje filtr pro entity, kde je: Váša' klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="5458a-162">hello following code example specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="5458a-163">Tento příklad zobrazí pole každé entity v konzole toohello výsledky dotazu hello hello.</span><span class="sxs-lookup"><span data-stu-id="5458a-163">This example prints hello fields of each entity in hello query results toohello console.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Construct hello query operation for all customer entities where PartitionKey="Smith".
azure::storage::table_query query;

query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

// Execute hello query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Print hello fields for each customer.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    const azure::storage::table_entity::properties_type& properties = it->properties();

    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
}  
```

<span data-ttu-id="5458a-164">Hello dotazu v tomto příkladu se zobrazí všechny hello entity, které odpovídají kritériím filtru hello.</span><span class="sxs-lookup"><span data-stu-id="5458a-164">hello query in this example brings all hello entities that match hello filter criteria.</span></span> <span data-ttu-id="5458a-165">Pokud máte velké tabulky a entity tabulky hello toodownload potřebujete často, doporučujeme místo toho ukládat data do objektů BLOB Azure storage.</span><span class="sxs-lookup"><span data-stu-id="5458a-165">If you have large tables and need toodownload hello table entities often, we recommend that you store your data in Azure storage blobs instead.</span></span>

## <a name="retrieve-a-range-of-entities-in-a-partition"></a><span data-ttu-id="5458a-166">Načtení rozsahu entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="5458a-166">Retrieve a range of entities in a partition</span></span>
<span data-ttu-id="5458a-167">Pokud nechcete, aby tooquery všechny hello entit v oddílu, můžete zadat rozsah kombinací hello filtru klíče oddílu s filtrem klíče řádku.</span><span class="sxs-lookup"><span data-stu-id="5458a-167">If you don't want tooquery all hello entities in a partition, you can specify a range by combining hello partition key filter with a row key filter.</span></span> <span data-ttu-id="5458a-168">Hello následující příklad kódu používá dva filtry tooget všech entit v oddílu "Smith, kde klíč řádku hello (jméno) začíná písmenem starší než"E"v hello abecedy a potom zobrazí výsledky dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="5458a-168">hello following code example uses two filters tooget all entities in partition 'Smith' where hello row key (first name) starts with a letter earlier than 'E' in hello alphabet and then prints hello query results.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create hello table query.
azure::storage::table_query query;

query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
    azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
    azure::storage::query_comparison_operator::equal, U("Smith")),
    azure::storage::query_logical_operator::op_and,
    azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

// Execute hello query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Loop through hello results, displaying information about hello entity.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    const azure::storage::table_entity::properties_type& properties = it->properties();

    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
}  
```

## <a name="retrieve-a-single-entity"></a><span data-ttu-id="5458a-169">Načtení jedné entity</span><span class="sxs-lookup"><span data-stu-id="5458a-169">Retrieve a single entity</span></span>
<span data-ttu-id="5458a-170">Můžete napsat dotaz tooretrieve jedné konkrétní entity.</span><span class="sxs-lookup"><span data-stu-id="5458a-170">You can write a query tooretrieve a single, specific entity.</span></span> <span data-ttu-id="5458a-171">Hello následující kód používá **table_operation::retrieve_entity** toospecify hello zákazníka 'Jeff Smith'.</span><span class="sxs-lookup"><span data-stu-id="5458a-171">hello following code uses **table_operation::retrieve_entity** toospecify hello customer 'Jeff Smith'.</span></span> <span data-ttu-id="5458a-172">Tato metoda vrátí místo kolekce pouze jednu entitu a hello vrácená hodnota v **table_result**.</span><span class="sxs-lookup"><span data-stu-id="5458a-172">This method returns just one entity, rather than a collection, and hello returned value is in **table_result**.</span></span> <span data-ttu-id="5458a-173">Určení klíče oddílu a řádku v dotazu je hello nejrychlejší způsob, jak tooretrieve jednu entitu ze služby Table hello.</span><span class="sxs-lookup"><span data-stu-id="5458a-173">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello Table service.</span></span>  

```cpp
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Retrieve hello entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Output hello entity.
azure::storage::table_entity entity = retrieve_result.entity();
const azure::storage::table_entity::properties_type& properties = entity.properties();

std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
    << U(", Property1: ") << properties.at(U("Email")).string_value()
    << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
```

## <a name="replace-an-entity"></a><span data-ttu-id="5458a-174">Nahrazení entity</span><span class="sxs-lookup"><span data-stu-id="5458a-174">Replace an entity</span></span>
<span data-ttu-id="5458a-175">tooreplace entity, načtěte ji ze služby Table hello, upravte objekt entity hello a potom uložte změny hello zpět toohello služby Table.</span><span class="sxs-lookup"><span data-stu-id="5458a-175">tooreplace an entity, retrieve it from hello Table service, modify hello entity object, and then save hello changes back toohello Table service.</span></span> <span data-ttu-id="5458a-176">Hello následující kód změní stávající zákazník telefonní číslo a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="5458a-176">hello following code changes an existing customer's phone number and email address.</span></span> <span data-ttu-id="5458a-177">Místo volání **table_operation::insert_entity**, tento kód používá **table_operation::replace_entity**.</span><span class="sxs-lookup"><span data-stu-id="5458a-177">Instead of calling **table_operation::insert_entity**, this code uses **table_operation::replace_entity**.</span></span> <span data-ttu-id="5458a-178">To způsobí, že toobe entity hello plně nahradí na serveru hello, pokud došlo ke změně hello entit na serveru hello vzhledem k tomu, že byla načtena, v takovém případě hello se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="5458a-178">This causes hello entity toobe fully replaced on hello server, unless hello entity on hello server has changed since it was retrieved, in which case hello operation will fail.</span></span> <span data-ttu-id="5458a-179">Tato chyba je tooprevent aplikace nechtěném přepsání změny provedené mezi hello načtení a aktualizací provedenou jinou součástí vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5458a-179">This failure is tooprevent your application from inadvertently overwriting a change made between hello retrieval and update by another component of your application.</span></span> <span data-ttu-id="5458a-180">Dobrý den správné zpracování této chyby je tooretrieve hello entita znovu, provedené změny (Pokud je stále platný) a pak provedete další **table_operation::replace_entity** operaci.</span><span class="sxs-lookup"><span data-stu-id="5458a-180">hello proper handling of this failure is tooretrieve hello entity again, make your changes (if still valid), and then perform another **table_operation::replace_entity** operation.</span></span> <span data-ttu-id="5458a-181">Hello další části se dozvíte, jak toooverride toto chování.</span><span class="sxs-lookup"><span data-stu-id="5458a-181">hello next section will show you how toooverride this behavior.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Replace an entity.
azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
properties_to_replace.reserve(2);

// Specify a new phone number.
properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

// Specify a new email address.
properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

// Create an operation tooreplace hello entity.
azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

// Submit hello operation toohello Table service.
azure::storage::table_result replace_result = table.execute(replace_operation);
```

## <a name="insert-or-replace-an-entity"></a><span data-ttu-id="5458a-182">Vložení nebo nahrazení entity</span><span class="sxs-lookup"><span data-stu-id="5458a-182">Insert-or-replace an entity</span></span>
<span data-ttu-id="5458a-183">**table_operation::replace_entity** operace se nezdaří, pokud hello entity se změnil od načtení ze serveru hello.</span><span class="sxs-lookup"><span data-stu-id="5458a-183">**table_operation::replace_entity** operations will fail if hello entity has been changed since it was retrieved from hello server.</span></span> <span data-ttu-id="5458a-184">Kromě toho musí načíst hello entity ze serveru hello první v pořadí pro **table_operation::replace_entity** toobe úspěšné.</span><span class="sxs-lookup"><span data-stu-id="5458a-184">Furthermore, you must retrieve hello entity from hello server first in order for **table_operation::replace_entity** toobe successful.</span></span> <span data-ttu-id="5458a-185">V některých případech ale nevíte, pokud hello entita existuje na serveru hello a hello aktuální hodnoty v ní uloženy jsou důležité – vaše aktualizace by je měla všechny přepsat.</span><span class="sxs-lookup"><span data-stu-id="5458a-185">Sometimes, however, you don't know if hello entity exists on hello server and hello current values stored in it are irrelevant—your update should overwrite them all.</span></span> <span data-ttu-id="5458a-186">tooaccomplish, byste použili **table_operation::insert_or_replace_entity** operaci.</span><span class="sxs-lookup"><span data-stu-id="5458a-186">tooaccomplish this, you would use a **table_operation::insert_or_replace_entity** operation.</span></span> <span data-ttu-id="5458a-187">Tato operace vloží entitu hello, pokud neexistuje, nebo ji nahradí, pokud ano, bez ohledu na to, kdy byla provedena poslední aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="5458a-187">This operation inserts hello entity if it doesn't exist, or replaces it if it does, regardless of when hello last update was made.</span></span> <span data-ttu-id="5458a-188">V následující ukázka kódu hello, entita zákazník hello Jeff Smith načtena, ale pak je uložena zpět toohello serveru prostřednictvím **table_operation::insert_or_replace_entity**.</span><span class="sxs-lookup"><span data-stu-id="5458a-188">In hello following code example, hello customer entity for Jeff Smith is still retrieved, but it is then saved back toohello server via **table_operation::insert_or_replace_entity**.</span></span> <span data-ttu-id="5458a-189">Jakékoli aktualizace provedené toohello entity mezi hello operace načtení a aktualizace budou přepsány.</span><span class="sxs-lookup"><span data-stu-id="5458a-189">Any updates made toohello entity between hello retrieval and update operation will be overwritten.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Insert-or-replace an entity.
azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

properties_to_insert_or_replace.reserve(2);

// Specify a phone number.
properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

// Specify an email address.
properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

// Create an operation tooinsert-or-replace hello entity.
azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

// Submit hello operation toohello Table service.
azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);
```

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="5458a-190">Dotaz na podmnožinu vlastností entity</span><span class="sxs-lookup"><span data-stu-id="5458a-190">Query a subset of entity properties</span></span>
<span data-ttu-id="5458a-191">Tabulku tooa dotazu můžete načíst jenom několik z entity.</span><span class="sxs-lookup"><span data-stu-id="5458a-191">A query tooa table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="5458a-192">Hello dotaz v hello následující kód používá hello **table_query::set_select_columns** metoda tooreturn pouze hello e-mailové adresy entit v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="5458a-192">hello query in hello following code uses hello **table_query::set_select_columns** method tooreturn only hello email addresses of entities in hello table.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Define hello query, and select only hello Email property.
azure::storage::table_query query;
std::vector<utility::string_t> columns;

columns.push_back(U("Email"));
query.set_select_columns(columns);

// Execute hello query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Display hello results.
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
> <span data-ttu-id="5458a-193">Dotazování několik vlastností z entity, je efektivnější operace než načítání všechny vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5458a-193">Querying a few properties from an entity is a more efficient operation than retrieving all properties.</span></span>
> 
> 

## <a name="delete-an-entity"></a><span data-ttu-id="5458a-194">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="5458a-194">Delete an entity</span></span>
<span data-ttu-id="5458a-195">Entitu můžete snadno odstranit po jejím načtení.</span><span class="sxs-lookup"><span data-stu-id="5458a-195">You can easily delete an entity after you have retrieved it.</span></span> <span data-ttu-id="5458a-196">Jakmile je načíst hello entity, volání **table_operation::delete_entity** s hello entity toodelete.</span><span class="sxs-lookup"><span data-stu-id="5458a-196">Once hello entity is retrieved, call **table_operation::delete_entity** with hello entity toodelete.</span></span> <span data-ttu-id="5458a-197">Potom zavolejte hello **cloud_table.execute** metoda.</span><span class="sxs-lookup"><span data-stu-id="5458a-197">Then call hello **cloud_table.execute** method.</span></span> <span data-ttu-id="5458a-198">Hello následující kód načte a odstraní entitu s klíčem oddílu pro "Smith" a klíč řádku z "Jan".</span><span class="sxs-lookup"><span data-stu-id="5458a-198">hello following code retrieves and deletes an entity with a partition key of "Smith" and a row key of "Jeff".</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create an operation tooretrieve hello entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Create an operation toodelete hello entity.
azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

// Submit hello delete operation toohello Table service.
azure::storage::table_result delete_result = table.execute(delete_operation);  
```

## <a name="delete-a-table"></a><span data-ttu-id="5458a-199">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="5458a-199">Delete a table</span></span>
<span data-ttu-id="5458a-200">Hello následující příklad kódu nakonec odstraní tabulku z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="5458a-200">Finally, hello following code example deletes a table from a storage account.</span></span> <span data-ttu-id="5458a-201">Tabulka, která byla odstraněna budou k dispozici toobe znovu vytvořit pro následující hello odstranění časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="5458a-201">A table that has been deleted will be unavailable toobe re-created for a period of time following hello deletion.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create an operation tooretrieve hello entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Create an operation toodelete hello entity.
azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

// Submit hello delete operation toohello Table service.
azure::storage::table_result delete_result = table.execute(delete_operation);
```

## <a name="next-steps"></a><span data-ttu-id="5458a-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5458a-202">Next steps</span></span>
<span data-ttu-id="5458a-203">Teď, když jste se naučili základy hello služby table storage, postupujte podle těchto odkazů toolearn Další informace o Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="5458a-203">Now that you've learned hello basics of table storage, follow these links toolearn more about Azure Storage:</span></span>  

* <span data-ttu-id="5458a-204">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="5458a-204">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* [<span data-ttu-id="5458a-205">Jak toouse úložiště objektů Blob z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="5458a-205">How toouse Blob storage from C++</span></span>](../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="5458a-206">Jak toouse úložiště Queue z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="5458a-206">How toouse Queue storage from C++</span></span>](../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
* [<span data-ttu-id="5458a-207">Seznam prostředků úložiště Azure v jazyce C++</span><span class="sxs-lookup"><span data-stu-id="5458a-207">List Azure Storage resources in C++</span></span>](../storage/common/storage-c-plus-plus-enumeration.md)
* [<span data-ttu-id="5458a-208">Klientská knihovna pro úložiště pro C++ – referenční informace</span><span class="sxs-lookup"><span data-stu-id="5458a-208">Storage Client Library for C++ reference</span></span>](http://azure.github.io/azure-storage-cpp)
* [<span data-ttu-id="5458a-209">Dokumentace k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="5458a-209">Azure Storage documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)
