---
title: aaaHow toouse Table storage (C++) | Microsoft Docs
description: "Ukládejte si strukturovaná data v cloudu hello pomocí Azure Table storage, úložiště dat typu NoSQL."
services: storage
documentationcenter: .net
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: f191f308-e4b2-4de9-85cb-551b82b1ea7c
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: seguler
ms.openlocfilehash: 8eee0031350ab6ff3f76fb288b2f896687aa17a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-c"></a>Jak toouse úložiště Table z jazyka C++
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Table storage. Hello ukázky jsou napsané v jazyce C++ a používají hello [Klientská knihovna pro úložiště Azure pro jazyk C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). Hello pokryté scénáře zahrnují **vytváření a odstraňování tabulek** a **práce s entity tabulky**.

> [!NOTE]
> Tato příručka cílí hello Klientská knihovna pro úložiště Azure pro jazyk C++ verze 1.0.0 a vyšší. Hello doporučená verze je klientská knihovna pro úložiště 2.2.0, která je k dispozici prostřednictvím [NuGet](http://www.nuget.org/packages/wastorage) nebo [Githubu](https://github.com/Azure/azure-storage-cpp/).
> 
> 

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Vytvoření aplikace C++
V této příručce bude používat funkce úložiště, které lze spustit v rámci aplikace C++. toodo tedy budete potřebovat tooinstall hello Klientská knihovna pro úložiště Azure pro jazyk C++ a vytvoření účtu úložiště Azure ve vašem předplatném Azure.  

tooinstall hello Klientská knihovna pro úložiště Azure pro jazyk C++, můžete použít následující metody hello:

* **Linux:** postupujte podle pokynů hello na hello [Klientská knihovna pro úložiště Azure pro C++ – soubor README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) stránky.  
* **Windows:** v sadě Visual Studio, klikněte na tlačítko **nástroje > Správce balíčků NuGet > Konzola správce balíčků**. Typ hello následující příkaz do hello [Konzola správce balíčků NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) a stiskněte klávesu Enter.  
  
     Install-Package wastorage

## <a name="configure-your-application-tooaccess-table-storage"></a>Konfigurace vaší aplikace tooaccess úložiště Table
Přidáte následující hello obsahovat toohello příkazy na začátek souboru C++ hello místo toouse hello úložiště Azure rozhraní API tooaccess tabulky:  

```cpp
#include <was/storage_account.h>
#include <was/table.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Nastavit připojovací řetězec úložiště Azure
Klienta Azure storage používá úložiště připojovací řetězec toostore koncových bodů a přihlašovací údaje pro přístup ke službám dat správy. Při spuštění klienta aplikace, je nutné zadat připojovací řetězec úložiště hello v hello formátu. Použití hello název účtu a hello úložiště přístupový klíč k úložišti pro účet úložiště hello uvedené v hello [portálu Azure](https://portal.azure.com) pro hello *AccountName* a *AccountKey* hodnoty. Informace o účtech úložiště a přístupové klávesy, najdete v části [účty Azure storage](storage-create-storage-account.md). Tento příklad ukazuje, jak můžou deklarovat statické pole toohold hello připojovací řetězec:  

```cpp
// Define hello connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

tootest aplikace ve vaší místní počítače se systémem Windows, můžete použít hello Azure [emulátor úložiště](storage-use-emulator.md) který se instaluje s hello [Azure SDK](https://azure.microsoft.com/downloads/). emulátor úložiště Hello je nástroj, který simuluje hello Azure Blob, Queue a Table služeb dostupných na počítači pro místní vývoj. Hello následující příklad ukazuje, jak lze deklarovat statické pole toohold hello připojovací řetězec tooyour emulátor místního úložiště:  

```cpp
// Define hello connection string with Azure storage emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

toostart hello emulátoru úložiště Azure, klikněte na tlačítko hello **spustit** tlačítko nebo stiskněte klávesu Windows hello. Začněte psát **emulátoru úložiště Azure**a potom vyberte **emulátor úložiště Microsoft Azure** hello seznamu aplikací.  

Hello následující ukázky předpokládejme použít jednu z těchto dvou metod tooget hello úložiště připojovací řetězec.  

## <a name="retrieve-your-connection-string"></a>Načtení připojovacího řetězce
Můžete použít hello **cloud_storage_account** třídy toorepresent informace o účtu úložiště. tooretrieve účet úložiště informací z připojovacího řetězce úložiště hello, můžete použít metodu analýzy hello.

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

V dalším kroku získat odkaz na tooa **cloud_table_client** třídy, protože umožňuje získat odkaz na objekty pro tabulky a entity uložené v rámci hello tabulka úložiště služby. Hello následující kód vytvoří **cloud_table_client** objekt pomocí objektu účtu úložiště hello nemůžeme načíst výše:  

```cpp
// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();
```

## <a name="create-a-table"></a>Vytvoření tabulky
A **cloud_table_client** objektu umožňuje získat odkaz na objekty pro tabulky a entity. Hello následující kód vytvoří **cloud_table_client** objektu a používá je toocreate novou tabulku.

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

## <a name="add-an-entity-tooa-table"></a>Přidat tooa tabulka entity
tooadd tooa tabulka entity, vytvořte novou **table_entity** objektu a předejte ji příliš**table_operation::insert_entity**. Hello následující kód používá jméno hello zákazníka jako klíč řádku hello a příjmení jako klíč oddílu hello. Společně oddílu a klíč řádku jednoznačně hello entity v tabulce hello. Entity s hello, které se stejným klíčem oddílu můžete zadat dotaz rychleji než ty, které jiné oddílu klíče, ale používání různých klíčů oddílů umožňuje větší škálovatelnost paralelních operaci. Další informace najdete v tématu [Microsoft Azure storage výkon a škálovatelnost kontrolní seznam](storage-performance-checklist.md).

Hello následující kód vytvoří novou instanci třídy **table_entity** s některé toobe dat zákazníka uložené. Hello kód zavolá metodu Další **table_operation::insert_entity** toocreate **table_operation** objektu tooinsert entity do tabulky a partnerů hello novou entitu tabulky s ním. Nakonec hello kód zavolá metodu hello provést metodu hello **cloud_table** objektu. A hello nové **table_operation** odešle žádost toohello tabulky služby tooinsert hello nové zákazníka entity do tabulky "lidé" hello.  

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

## <a name="insert-a-batch-of-entities"></a>Vložení dávky entit
V rámci jedné operace zápisu můžete vložit dávku entit toohello služby Table. Hello následující kód vytvoří **table_batch_operation** objektu a potom se přidají tři vložit tooit operace. Každé operace insert přidá se při vytvoření nového objektu entity, nastavení jeho hodnoty, a pak volání hello vložit metodu hello **table_batch_operation** objekt tooassociate hello entita s novou Vložit operaci. Potom **cloud_table.execute** nazývá tooexecute hello operaci.  

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

Některé věci toonote ohledně dávkových operací:  

* Můžete provést až too100 vložení, odstranění, sloučení, nahraďte, operace insert nebo sloučení a vložení nebo nahrazení v libovolné kombinace v jedné dávce.  
* Dávková operace může mít operaci načtení, pokud je hello jediná operace v dávce hello.  
* Všechny entity v jedné dávkové operaci musí mít hello stejným klíčem oddílu.  
* Dávkové operace je omezená tooa 4 MB datovou částí.  

## <a name="retrieve-all-entities-in-a-partition"></a>Načtení všech entit v oddílu
tooquery tabulku pro všechny entity v oddílu, použití **table_query** objektu. Hello následující příklad kódu určuje filtr pro entity, kde je: Váša' klíč oddílu hello. Tento příklad zobrazí pole každé entity v konzole toohello výsledky dotazu hello hello.  

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

Hello dotazu v tomto příkladu se zobrazí všechny hello entity, které odpovídají kritériím filtru hello. Pokud máte velké tabulky a entity tabulky hello toodownload potřebujete často, doporučujeme místo toho ukládat data do objektů BLOB Azure storage.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Načtení rozsahu entit v oddílu
Pokud nechcete, aby tooquery všechny hello entit v oddílu, můžete zadat rozsah kombinací hello filtru klíče oddílu s filtrem klíče řádku. Hello následující příklad kódu používá dva filtry tooget všech entit v oddílu "Smith, kde klíč řádku hello (jméno) začíná písmenem starší než"E"v hello abecedy a potom zobrazí výsledky dotazu hello.  

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

## <a name="retrieve-a-single-entity"></a>Načtení jedné entity
Můžete napsat dotaz tooretrieve jedné konkrétní entity. Hello následující kód používá **table_operation::retrieve_entity** toospecify hello zákazníka 'Jeff Smith'. Tato metoda vrátí místo kolekce pouze jednu entitu a hello vrácená hodnota v **table_result**. Určení klíče oddílu a řádku v dotazu je hello nejrychlejší způsob, jak tooretrieve jednu entitu ze služby Table hello.  

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

## <a name="replace-an-entity"></a>Nahrazení entity
tooreplace entity, načtěte ji ze služby Table hello, upravte objekt entity hello a potom uložte změny hello zpět toohello služby Table. Hello následující kód změní stávající zákazník telefonní číslo a e-mailovou adresu. Místo volání **table_operation::insert_entity**, tento kód používá **table_operation::replace_entity**. To způsobí, že toobe entity hello plně nahradí na serveru hello, pokud došlo ke změně hello entit na serveru hello vzhledem k tomu, že byla načtena, v takovém případě hello se nezdaří. Tato chyba je tooprevent aplikace nechtěném přepsání změny provedené mezi hello načtení a aktualizací provedenou jinou součástí vaší aplikace. Dobrý den správné zpracování této chyby je tooretrieve hello entita znovu, provedené změny (Pokud je stále platný) a pak provedete další **table_operation::replace_entity** operaci. Hello další části se dozvíte, jak toooverride toto chování.  

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

## <a name="insert-or-replace-an-entity"></a>Vložení nebo nahrazení entity
**table_operation::replace_entity** operace se nezdaří, pokud hello entity se změnil od načtení ze serveru hello. Kromě toho musí načíst hello entity ze serveru hello první v pořadí pro **table_operation::replace_entity** toobe úspěšné. V některých případech ale nevíte, pokud hello entita existuje na serveru hello a hello aktuální hodnoty v ní uloženy jsou důležité – vaše aktualizace by je měla všechny přepsat. tooaccomplish, byste použili **table_operation::insert_or_replace_entity** operaci. Tato operace vloží entitu hello, pokud neexistuje, nebo ji nahradí, pokud ano, bez ohledu na to, kdy byla provedena poslední aktualizace hello. V následující ukázka kódu hello, entita zákazník hello Jeff Smith načtena, ale pak je uložena zpět toohello serveru prostřednictvím **table_operation::insert_or_replace_entity**. Jakékoli aktualizace provedené toohello entity mezi hello operace načtení a aktualizace budou přepsány.  

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

## <a name="query-a-subset-of-entity-properties"></a>Dotaz na podmnožinu vlastností entity
Tabulku tooa dotazu můžete načíst jenom několik z entity. Hello dotaz v hello následující kód používá hello **table_query::set_select_columns** metoda tooreturn pouze hello e-mailové adresy entit v tabulce hello.  

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
> Dotazování několik vlastností z entity, je efektivnější operace než načítání všechny vlastnosti.
> 
> 

## <a name="delete-an-entity"></a>Odstranění entity
Entitu můžete snadno odstranit po jejím načtení. Jakmile je načíst hello entity, volání **table_operation::delete_entity** s hello entity toodelete. Potom zavolejte hello **cloud_table.execute** metoda. Hello následující kód načte a odstraní entitu s klíčem oddílu pro "Smith" a klíč řádku z "Jan".  

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

## <a name="delete-a-table"></a>Odstranění tabulky
Hello následující příklad kódu nakonec odstraní tabulku z účtu úložiště. Tabulka, která byla odstraněna budou k dispozici toobe znovu vytvořit pro následující hello odstranění časovém intervalu.  

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

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello služby table storage, postupujte podle těchto odkazů toolearn Další informace o Azure Storage:  

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.
* [Jak toouse úložiště objektů Blob z jazyka C++](storage-c-plus-plus-how-to-use-blobs.md)
* [Jak toouse úložiště Queue z jazyka C++](storage-c-plus-plus-how-to-use-queues.md)
* [Seznam prostředků úložiště Azure v jazyce C++](storage-c-plus-plus-enumeration.md)
* [Klientská knihovna pro úložiště pro C++ – referenční informace](http://azure.github.io/azure-storage-cpp)
* [Dokumentace k Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
