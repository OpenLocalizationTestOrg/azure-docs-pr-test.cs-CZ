---
title: "Funkce aaaAzure SQL databáze JSON | Microsoft Docs"
description: "Databáze SQL Azure vám umožní tooparse, dotazů a formátování dat v notaci JavaScript Object Notation (JSON)."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: jhubbard
editor: 
ms.assetid: 55860105-2f5f-4b10-87a0-99faa32b5653
ms.service: sql-database
ms.custom: develop databases
ms.devlang: NA
ms.date: 11/15/2016
ms.author: jovanpop
ms.workload: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 30a31a1b01482ec276646b6fd6ca0c1f581168d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-json-features-in-azure-sql-database"></a><span data-ttu-id="c75af-103">Začínáme s funkcí JSON v Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="c75af-103">Getting started with JSON features in Azure SQL Database</span></span>
<span data-ttu-id="c75af-104">Azure SQL Database umožňuje analyzovat a zadávat dotazy na data v JavaScript Object Notation [(JSON)](http://www.json.org/) formátu a exportovat relačních dat jako JSON text.</span><span class="sxs-lookup"><span data-stu-id="c75af-104">Azure SQL Database lets you parse and query data represented in JavaScript Object Notation [(JSON)](http://www.json.org/) format, and export your relational data as JSON text.</span></span>

<span data-ttu-id="c75af-105">JSON je Oblíbené data formát používaný pro výměnu dat v moderní webové a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="c75af-105">JSON is a popular data format used for exchanging data in modern web and mobile applications.</span></span> <span data-ttu-id="c75af-106">JSON je také používá k ukládání částečně strukturovaných dat v souborech protokolů nebo ve databáze NoSQL, jako [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="c75af-106">JSON is also used for storing semi-structured data in log files or in NoSQL databases like [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/).</span></span> <span data-ttu-id="c75af-107">Mnoho webových služeb REST vráceny výsledky formátovat jako JSON text nebo přijímat data formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="c75af-107">Many REST web services return results formatted as JSON text or accept data formatted as JSON.</span></span> <span data-ttu-id="c75af-108">Většina Azure služeb, jako [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), a [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) mít koncové body REST, které vrátí nebo využívat JSON.</span><span class="sxs-lookup"><span data-stu-id="c75af-108">Most Azure services such as [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), and [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) have REST endpoints that return or consume JSON.</span></span>

<span data-ttu-id="c75af-109">Databáze SQL Azure vám umožní snadno pracovat s daty JSON a integrovat moderní služeb vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="c75af-109">Azure SQL Database lets you work with JSON data easily and integrate your database with modern services.</span></span>

## <a name="overview"></a><span data-ttu-id="c75af-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="c75af-110">Overview</span></span>
<span data-ttu-id="c75af-111">Azure SQL Database poskytuje následující funkce pro práci s daty JSON hello:</span><span class="sxs-lookup"><span data-stu-id="c75af-111">Azure SQL Database provides hello following functions for working with JSON data:</span></span>

![Funkce JSON](./media/sql-database-json-features/image_1.png)

<span data-ttu-id="c75af-113">Pokud máte JSON text, můžete extrahovat data z JSON nebo ověřte, že JSON je ve správném formátu pomocí integrované funkce hello [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), a [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx) .</span><span class="sxs-lookup"><span data-stu-id="c75af-113">If you have JSON text, you can extract data from JSON or verify that JSON is properly formatted by using hello built-in functions [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), and [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx).</span></span> <span data-ttu-id="c75af-114">Hello [příkazu JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) funkce vám umožní aktualizovat hodnotu uvnitř JSON text.</span><span class="sxs-lookup"><span data-stu-id="c75af-114">hello [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) function lets you update value inside JSON text.</span></span> <span data-ttu-id="c75af-115">Pro další pokročilé dotazy a analýzy, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) funkce můžete převést pole objektů JSON do sady řádků.</span><span class="sxs-lookup"><span data-stu-id="c75af-115">For more advanced querying and analysis, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) function can transform an array of JSON objects into a set of rows.</span></span> <span data-ttu-id="c75af-116">Jakýkoli dotaz SQL, mohou být provedeny u hello vrátila sadu výsledků.</span><span class="sxs-lookup"><span data-stu-id="c75af-116">Any SQL query can be executed on hello returned result set.</span></span> <span data-ttu-id="c75af-117">Nakonec je [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) klauzuli, která vám umožní zformátovat data uložená v tabulkách relační jako JSON text.</span><span class="sxs-lookup"><span data-stu-id="c75af-117">Finally, there is a [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) clause that lets you format data stored in your relational tables as JSON text.</span></span>

## <a name="formatting-relational-data-in-json-format"></a><span data-ttu-id="c75af-118">Formátování relační data ve formátu JSON</span><span class="sxs-lookup"><span data-stu-id="c75af-118">Formatting relational data in JSON format</span></span>
<span data-ttu-id="c75af-119">Pokud máte webová služba přijímá data z databáze hello vrstvy a poskytuje odpověď ve formátu JSON formátu, nebo rozhraní JavaScript na straně klienta nebo knihovny, které přijímá data formátu JSON, je databáze obsahu formátovat jako JSON přímo v příkazu jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="c75af-119">If you have a web service that takes data from hello database layer and provides a response in JSON format, or client-side JavaScript frameworks or libraries that accept data formatted as JSON, you can format your database content as JSON directly in a SQL query.</span></span> <span data-ttu-id="c75af-120">Už máte toowrite aplikační kód, který zformátuje výsledky z Azure SQL Database jako formát JSON, nebo zahrnout některé JSON serializace knihovny tooconvert tabulkový dotaz výsledky a potom serializovat objekty tooJSON formátu.</span><span class="sxs-lookup"><span data-stu-id="c75af-120">You no longer have toowrite application code that formats results from Azure SQL Database as JSON, or include some JSON serialization library tooconvert tabular query results and then serialize objects tooJSON format.</span></span> <span data-ttu-id="c75af-121">Místo toho můžete použít hello na výsledky dotazu SQL tooformat klauzule JSON jako JSON v databázi SQL Azure a použít ho přímo v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c75af-121">Instead, you can use hello FOR JSON clause tooformat SQL query results as JSON in Azure SQL Database and use it directly in your application.</span></span>

<span data-ttu-id="c75af-122">V následujícím příkladu hello jsou pomocí hello klauzule FOR JSON řádky z tabulky Sales.Customer hello formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="c75af-122">In hello following example, rows from hello Sales.Customer table are formatted as JSON by using hello FOR JSON clause:</span></span>

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

<span data-ttu-id="c75af-123">klauzule FOR JSON PATH Hello naformátuje hello výsledky dotazu hello jako JSON text.</span><span class="sxs-lookup"><span data-stu-id="c75af-123">hello FOR JSON PATH clause formats hello results of hello query as JSON text.</span></span> <span data-ttu-id="c75af-124">Názvy sloupců se používají jako klíče, hodnot v buňkách hello se sice generuje jako hodnoty JSON:</span><span class="sxs-lookup"><span data-stu-id="c75af-124">Column names are used as keys, while hello cell values are generated as JSON values:</span></span>

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

<span data-ttu-id="c75af-125">Sada výsledků dotazu Hello je naformátován jako pole JSON, kde každý řádek je formátován jako samostatný objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="c75af-125">hello result set is formatted as a JSON array where each row is formatted as a separate JSON object.</span></span>

<span data-ttu-id="c75af-126">CESTA Určuje hello výstupní formát vaší výsledku JSON lze přizpůsobit pomocí zápisu s tečkou v aliasy sloupců.</span><span class="sxs-lookup"><span data-stu-id="c75af-126">PATH indicates that you can customize hello output format of your JSON result by using dot notation in column aliases.</span></span> <span data-ttu-id="c75af-127">Následující dotaz Hello změní hello název hello "JménoZákazníka" klíč ve formátu JSON výstup hello a vloží číslo telefonu a faxu v objektu dílčí "Kontakt" hello:</span><span class="sxs-lookup"><span data-stu-id="c75af-127">hello following query changes hello name of hello "CustomerName" key in hello output JSON format, and puts phone and fax numbers in hello "Contact" sub-object:</span></span>

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

<span data-ttu-id="c75af-128">Hello výstup tohoto dotazu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c75af-128">hello output of this query looks like this:</span></span>

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

<span data-ttu-id="c75af-129">V tomto příkladu jsme vrátil jeden objekt JSON místo pole zadáním hello [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) možnost.</span><span class="sxs-lookup"><span data-stu-id="c75af-129">In this example we returned a single JSON object instead of an array by specifying hello [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) option.</span></span> <span data-ttu-id="c75af-130">Tuto možnost můžete použít, pokud víte, že jsou vrácení jednoho objektu. v důsledku dotazu.</span><span class="sxs-lookup"><span data-stu-id="c75af-130">You can use this option if you know that you are returning a single object as a result of query.</span></span>

<span data-ttu-id="c75af-131">Hello hlavní hodnota hello klauzule FOR JSON je, že umožňuje vrátit komplexní hierarchické data v databázi formátován jako vnořených objektů JSON nebo pole.</span><span class="sxs-lookup"><span data-stu-id="c75af-131">hello main value of hello FOR JSON clause is that it lets you return complex hierarchical data from your database formatted as nested JSON objects or arrays.</span></span> <span data-ttu-id="c75af-132">Následující příklad ukazuje způsob tooinclude řadí, které patří toohello zákazníka jako vnořená pole objednávek Hello:</span><span class="sxs-lookup"><span data-stu-id="c75af-132">hello following example shows how tooinclude Orders that belong toohello Customer as a nested array of Orders:</span></span>

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

<span data-ttu-id="c75af-133">Místo abyste odesílali samostatné dotazy tooget zákaznická data a pak toofetch seznam související objednávky, můžete získat všechna potřebná data hello s jediný dotaz, jak je znázorněno v následující ukázkový výstup hello:</span><span class="sxs-lookup"><span data-stu-id="c75af-133">Instead of sending separate queries tooget Customer data and then toofetch a list of related Orders, you can get all hello necessary data with a single query, as shown in hello following sample output:</span></span>

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a><span data-ttu-id="c75af-134">Práce s daty JSON</span><span class="sxs-lookup"><span data-stu-id="c75af-134">Working with JSON data</span></span>
<span data-ttu-id="c75af-135">Pokud nemáte výhradně strukturovaných dat, pokud máte komplexní dílčí objekty, pole nebo hierarchické data, nebo pokud datové struktury v průběhu času vyvíjejí, formátu JSON hello vám může pomoct toorepresent všechny rozšířené datové struktury.</span><span class="sxs-lookup"><span data-stu-id="c75af-135">If you don’t have strictly structured data, if you have complex sub-objects, arrays, or hierarchical data, or if your data structures evolve over time, hello JSON format can help you toorepresent any complex data structure.</span></span>

<span data-ttu-id="c75af-136">JSON je textový formát, který lze použít jako libovolný jiný typ řetězce ve službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="c75af-136">JSON is a textual format that can be used like any other string type in Azure SQL Database.</span></span> <span data-ttu-id="c75af-137">Můžete odeslat nebo jako standardní NVARCHAR úložiště dat JSON:</span><span class="sxs-lookup"><span data-stu-id="c75af-137">You can send or store JSON data as a standard NVARCHAR:</span></span>

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

<span data-ttu-id="c75af-138">Hello data JSON použité v tomto příkladu je reprezentována pomocí hello typu NVARCHAR(MAX).</span><span class="sxs-lookup"><span data-stu-id="c75af-138">hello JSON data used in this example is represented by using hello NVARCHAR(MAX) type.</span></span> <span data-ttu-id="c75af-139">JSON můžete vložit do této tabulky nebo zadaný jako argument hello uložené procedury pomocí standardní syntaxe Transact-SQL, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="c75af-139">JSON can be inserted into this table or provided as an argument of hello stored procedure using standard Transact-SQL syntax as shown in hello following example:</span></span>

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

<span data-ttu-id="c75af-140">Jazyk klienta nebo knihovny, která funguje s daty řetězec v databázi SQL Azure bude také pracovat s daty JSON.</span><span class="sxs-lookup"><span data-stu-id="c75af-140">Any client-side language or library that works with string data in Azure SQL Database will also work with JSON data.</span></span> <span data-ttu-id="c75af-141">JSON může být uložen v žádné tabulky, která podporuje typu NVARCHAR hello, například paměťově optimalizované tabulky nebo tabulky se systémovou správou verzí.</span><span class="sxs-lookup"><span data-stu-id="c75af-141">JSON can be stored in any table that supports hello NVARCHAR type, such as a Memory-optimized table or a System-versioned table.</span></span> <span data-ttu-id="c75af-142">JSON nezavádí žádné omezení v kódu na straně klienta hello nebo v databázové vrstvě hello.</span><span class="sxs-lookup"><span data-stu-id="c75af-142">JSON does not introduce any constraint either in hello client-side code or in hello database layer.</span></span>

## <a name="querying-json-data"></a><span data-ttu-id="c75af-143">Dotazování na JSON data</span><span class="sxs-lookup"><span data-stu-id="c75af-143">Querying JSON data</span></span>
<span data-ttu-id="c75af-144">Pokud máte data formátu JSON, ukládat do tabulek Azure SQL, funkce JSON umožňují používat tato data v jakékoli dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="c75af-144">If you have data formatted as JSON stored in Azure SQL tables, JSON functions let you use this data in any SQL query.</span></span>

<span data-ttu-id="c75af-145">Funkce JSON, které jsou k dispozici v Azure SQL database umožňují považovat datového naformátován jako formát JSON jako jiný typ dat SQL.</span><span class="sxs-lookup"><span data-stu-id="c75af-145">JSON functions that are available in Azure SQL database let you treat data formatted as JSON as any other SQL data type.</span></span> <span data-ttu-id="c75af-146">Můžete snadno rozbalte hodnoty z hello JSON text a použít JSON data v jakýkoli dotaz:</span><span class="sxs-lookup"><span data-stu-id="c75af-146">You can easily extract values from hello JSON text, and use JSON data in any query:</span></span>

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

<span data-ttu-id="c75af-147">Hello funkce JSON_VALUE extrahuje hodnotu z textu JSON uložené v hello datový sloupec.</span><span class="sxs-lookup"><span data-stu-id="c75af-147">hello JSON_VALUE function extracts a value from JSON text stored in hello Data column.</span></span> <span data-ttu-id="c75af-148">Tato funkce využívá jazyka JavaScript cesta tooreference hodnotu v JSON text tooextract.</span><span class="sxs-lookup"><span data-stu-id="c75af-148">This function uses a JavaScript-like path tooreference a value in JSON text tooextract.</span></span> <span data-ttu-id="c75af-149">Hodnota Hello extrahovat lze v libovolnou část dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="c75af-149">hello extracted value can be used in any part of SQL query.</span></span>

<span data-ttu-id="c75af-150">Hello JSON_QUERY funkce je podobné tooJSON_VALUE.</span><span class="sxs-lookup"><span data-stu-id="c75af-150">hello JSON_QUERY function is similar tooJSON_VALUE.</span></span> <span data-ttu-id="c75af-151">Na rozdíl od JSON_VALUE tato funkce extrahuje komplexní dílčí objekt, jako je například pole nebo objekty, které jsou umístěny v textu JSON.</span><span class="sxs-lookup"><span data-stu-id="c75af-151">Unlike JSON_VALUE, this function extracts complex sub-object such as arrays or objects that are placed in JSON text.</span></span>

<span data-ttu-id="c75af-152">Funkce příkazu JSON_MODIFY Hello můžete zadat cestu hello hello hodnoty v textu hello JSON, který by měly být aktualizovány a také novou hodnotu, který přepíše hello ten starý.</span><span class="sxs-lookup"><span data-stu-id="c75af-152">hello JSON_MODIFY function lets you specify hello path of hello value in hello JSON text that should be updated, as well as a new value that will overwrite hello old one.</span></span> <span data-ttu-id="c75af-153">Tímto způsobem můžete snadno aktualizovat v textu JSON bez reparsing celá struktura hello.</span><span class="sxs-lookup"><span data-stu-id="c75af-153">This way you can easily update JSON text without reparsing hello entire structure.</span></span>

<span data-ttu-id="c75af-154">Vzhledem k tomu, že JSON je uložený ve formátu standardního textu, neexistují žádné záruky, že hello hodnotami uloženými v textu sloupce jsou ve správném formátu.</span><span class="sxs-lookup"><span data-stu-id="c75af-154">Since JSON is stored in a standard text, there are no guarantees that hello values stored in text columns are properly formatted.</span></span> <span data-ttu-id="c75af-155">Můžete ověřit, že text uložený ve formátu JSON sloupce je ve správném formátu pomocí standardní omezení check Azure SQL Database a hello ISJSON funkce:</span><span class="sxs-lookup"><span data-stu-id="c75af-155">You can verify that text stored in JSON column is properly formatted by using standard Azure SQL Database check constraints and hello ISJSON function:</span></span>

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

<span data-ttu-id="c75af-156">Pokud je ve správném formátu vstupního textu hello formát JSON, hello ISJSON funkce vrátí hello hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="c75af-156">If hello input text is properly formatted JSON, hello ISJSON function returns hello value 1.</span></span> <span data-ttu-id="c75af-157">Na každém vložení nebo aktualizace sloupce JSON bude toto omezení ověřte novou textovou hodnotou není nesprávný formát JSON.</span><span class="sxs-lookup"><span data-stu-id="c75af-157">On every insert or update of JSON column, this constraint will verify that new text value is not malformed JSON.</span></span>

## <a name="transforming-json-into-tabular-format"></a><span data-ttu-id="c75af-158">Transformace v tabulkovém formátu JSON</span><span class="sxs-lookup"><span data-stu-id="c75af-158">Transforming JSON into tabular format</span></span>
<span data-ttu-id="c75af-159">Databáze SQL Azure také vám umožňuje transformovat kolekce JSON v tabulkovém formátu a zatížení nebo dotaz, data JSON.</span><span class="sxs-lookup"><span data-stu-id="c75af-159">Azure SQL Database also lets you transform JSON collections into tabular format and load or query JSON data.</span></span>

<span data-ttu-id="c75af-160">OPENJSON je funkce tabulky hodnota, která analyzuje JSON text, vyhledá pole objektů JSON, prochází hello prvky hello pole a vrátí jeden řádek v hello výsledných výstupů pro jednotlivé elementy pole hello.</span><span class="sxs-lookup"><span data-stu-id="c75af-160">OPENJSON is a table-value function that parses JSON text, locates an array of JSON objects, iterates through hello elements of hello array, and returns one row in hello output result for each element of hello array.</span></span>

![Tabulkové JSON](./media/sql-database-json-features/image_2.png)

<span data-ttu-id="c75af-162">V předchozím příkladu hello můžeme určit, kde toolocate hello pole JSON, který musí být otevřen (v hello $. Cesta objednávky), které sloupce má být vrácen jako výsledek a kde toofind hello JSON hodnoty, které bude vrácen jako buněk.</span><span class="sxs-lookup"><span data-stu-id="c75af-162">In hello example above, we can specify where toolocate hello JSON array that should be opened (in hello $.Orders path), what columns should be returned as result, and where toofind hello JSON values that will be returned as cells.</span></span>

<span data-ttu-id="c75af-163">Jsme můžete převést pole JSON v hello @orders proměnné do sady řádků, analyzovat tuto sadu výsledků nebo vložení řádků do tabulky Standardní:</span><span class="sxs-lookup"><span data-stu-id="c75af-163">We can transform a JSON array in hello @orders variable into a set of rows, analyze this result set, or insert rows into a standard table:</span></span>

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
<span data-ttu-id="c75af-164">kolekce Hello objednávek naformátovaný jako pole JSON a zadat jako parametr toohello uložené procedury je možné analyzovat a vložit do tabulky objednávky hello.</span><span class="sxs-lookup"><span data-stu-id="c75af-164">hello collection of orders formatted as a JSON array and provided as a parameter toohello stored procedure can be parsed and inserted into hello Orders table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c75af-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c75af-165">Next steps</span></span>
<span data-ttu-id="c75af-166">toolearn jak toointegrate JSON do vaší aplikace, podívejte se na tyto prostředky:</span><span class="sxs-lookup"><span data-stu-id="c75af-166">toolearn how toointegrate JSON into your application, check out these resources:</span></span>

* [<span data-ttu-id="c75af-167">Blog na TechNetu</span><span class="sxs-lookup"><span data-stu-id="c75af-167">TechNet Blog</span></span>](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
* [<span data-ttu-id="c75af-168">Dokumentace MSDN</span><span class="sxs-lookup"><span data-stu-id="c75af-168">MSDN documentation</span></span>](https://msdn.microsoft.com/library/dn921897.aspx)
* [<span data-ttu-id="c75af-169">Channel 9 video</span><span class="sxs-lookup"><span data-stu-id="c75af-169">Channel 9 video</span></span>](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

<span data-ttu-id="c75af-170">toolearn o různé scénáře pro integraci JSON do vaší aplikace, najdete v části hello ukázky v tomto [Channel 9 video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) nebo najít scénáře, který odpovídá váš případ použití v [JSON příspěvky](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).</span><span class="sxs-lookup"><span data-stu-id="c75af-170">toolearn about various scenarios for integrating JSON into your application, see hello demos in this [Channel 9 video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) or find a scenario that matches your use case in [JSON Blog posts](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).</span></span>

