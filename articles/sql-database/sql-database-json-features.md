---
title: "Funkce Azure SQL databáze JSON | Microsoft Docs"
description: "Databáze SQL Azure umožňuje analýzu, dotazů a formátování dat v notaci JavaScript Object Notation (JSON)."
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
ms.openlocfilehash: 883e661107dd838f5c381cdef2c7f891b9a9389c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-json-features-in-azure-sql-database"></a><span data-ttu-id="d61f2-103">Začínáme s funkcí JSON v Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="d61f2-103">Getting started with JSON features in Azure SQL Database</span></span>
<span data-ttu-id="d61f2-104">Azure SQL Database umožňuje analyzovat a zadávat dotazy na data v JavaScript Object Notation [(JSON)](http://www.json.org/) formátu a exportovat relačních dat jako JSON text.</span><span class="sxs-lookup"><span data-stu-id="d61f2-104">Azure SQL Database lets you parse and query data represented in JavaScript Object Notation [(JSON)](http://www.json.org/) format, and export your relational data as JSON text.</span></span>

<span data-ttu-id="d61f2-105">JSON je Oblíbené data formát používaný pro výměnu dat v moderní webové a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d61f2-105">JSON is a popular data format used for exchanging data in modern web and mobile applications.</span></span> <span data-ttu-id="d61f2-106">JSON je také používá k ukládání částečně strukturovaných dat v souborech protokolů nebo ve databáze NoSQL, jako [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="d61f2-106">JSON is also used for storing semi-structured data in log files or in NoSQL databases like [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/).</span></span> <span data-ttu-id="d61f2-107">Mnoho webových služeb REST vráceny výsledky formátovat jako JSON text nebo přijímat data formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="d61f2-107">Many REST web services return results formatted as JSON text or accept data formatted as JSON.</span></span> <span data-ttu-id="d61f2-108">Většina Azure služeb, jako [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), a [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) mít koncové body REST, které vrátí nebo využívat JSON.</span><span class="sxs-lookup"><span data-stu-id="d61f2-108">Most Azure services such as [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), and [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) have REST endpoints that return or consume JSON.</span></span>

<span data-ttu-id="d61f2-109">Databáze SQL Azure vám umožní snadno pracovat s daty JSON a integrovat moderní služeb vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="d61f2-109">Azure SQL Database lets you work with JSON data easily and integrate your database with modern services.</span></span>

## <a name="overview"></a><span data-ttu-id="d61f2-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="d61f2-110">Overview</span></span>
<span data-ttu-id="d61f2-111">Azure SQL Database poskytuje následující funkce pro práci s daty JSON:</span><span class="sxs-lookup"><span data-stu-id="d61f2-111">Azure SQL Database provides the following functions for working with JSON data:</span></span>

![Funkce JSON](./media/sql-database-json-features/image_1.png)

<span data-ttu-id="d61f2-113">Pokud máte JSON text, můžete extrahovat data z JSON nebo ověřte, že JSON je ve správném formátu pomocí integrované funkce [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), a [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx).</span><span class="sxs-lookup"><span data-stu-id="d61f2-113">If you have JSON text, you can extract data from JSON or verify that JSON is properly formatted by using the built-in functions [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), and [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx).</span></span> <span data-ttu-id="d61f2-114">[Příkazu JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) funkce vám umožní aktualizovat hodnotu uvnitř JSON text.</span><span class="sxs-lookup"><span data-stu-id="d61f2-114">The [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) function lets you update value inside JSON text.</span></span> <span data-ttu-id="d61f2-115">Pro další pokročilé dotazy a analýzy, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) funkce můžete převést pole objektů JSON do sady řádků.</span><span class="sxs-lookup"><span data-stu-id="d61f2-115">For more advanced querying and analysis, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) function can transform an array of JSON objects into a set of rows.</span></span> <span data-ttu-id="d61f2-116">Jakýkoli dotaz SQL, mohou být provedeny u vrácené výsledné sady.</span><span class="sxs-lookup"><span data-stu-id="d61f2-116">Any SQL query can be executed on the returned result set.</span></span> <span data-ttu-id="d61f2-117">Nakonec je [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) klauzuli, která vám umožní zformátovat data uložená v tabulkách relační jako JSON text.</span><span class="sxs-lookup"><span data-stu-id="d61f2-117">Finally, there is a [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) clause that lets you format data stored in your relational tables as JSON text.</span></span>

## <a name="formatting-relational-data-in-json-format"></a><span data-ttu-id="d61f2-118">Formátování relační data ve formátu JSON</span><span class="sxs-lookup"><span data-stu-id="d61f2-118">Formatting relational data in JSON format</span></span>
<span data-ttu-id="d61f2-119">Pokud máte webová služba přijímá data z databáze vrstvy a poskytuje odpověď ve formátu JSON formátu, nebo rozhraní JavaScript na straně klienta nebo knihovny, které přijímá data formátu JSON, je databáze obsahu formátovat jako JSON přímo v příkazu jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="d61f2-119">If you have a web service that takes data from the database layer and provides a response in JSON format, or client-side JavaScript frameworks or libraries that accept data formatted as JSON, you can format your database content as JSON directly in a SQL query.</span></span> <span data-ttu-id="d61f2-120">Už máte k zápisu kódu aplikace, který zformátuje výsledky z Azure SQL Database jako JSON, nebo zahrnout některé knihovny serializace JSON a převést výsledky tabulkový dotaz a pak serializovat objekty do formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="d61f2-120">You no longer have to write application code that formats results from Azure SQL Database as JSON, or include some JSON serialization library to convert tabular query results and then serialize objects to JSON format.</span></span> <span data-ttu-id="d61f2-121">Místo toho můžete v klauzuli FOR JSON výsledky dotazu SQL formátovat jako JSON v Azure SQL Database a jeho použití přímo v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d61f2-121">Instead, you can use the FOR JSON clause to format SQL query results as JSON in Azure SQL Database and use it directly in your application.</span></span>

<span data-ttu-id="d61f2-122">V následujícím příkladu jsou pomocí klauzule FOR JSON řádky z tabulky Sales.Customer formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="d61f2-122">In the following example, rows from the Sales.Customer table are formatted as JSON by using the FOR JSON clause:</span></span>

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

<span data-ttu-id="d61f2-123">V klauzuli FOR JSON PATH naformátuje výsledky dotazu jako JSON text.</span><span class="sxs-lookup"><span data-stu-id="d61f2-123">The FOR JSON PATH clause formats the results of the query as JSON text.</span></span> <span data-ttu-id="d61f2-124">Názvy sloupců se používají jako klíče, hodnoty buněk se sice generuje jako hodnoty JSON:</span><span class="sxs-lookup"><span data-stu-id="d61f2-124">Column names are used as keys, while the cell values are generated as JSON values:</span></span>

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

<span data-ttu-id="d61f2-125">Sada výsledků je naformátován jako pole JSON, kde každý řádek je formátován jako samostatný objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="d61f2-125">The result set is formatted as a JSON array where each row is formatted as a separate JSON object.</span></span>

<span data-ttu-id="d61f2-126">CESTA označuje, že výstupní formát vaší výsledku JSON můžete přizpůsobit pomocí zápisu s tečkou v aliasy sloupců.</span><span class="sxs-lookup"><span data-stu-id="d61f2-126">PATH indicates that you can customize the output format of your JSON result by using dot notation in column aliases.</span></span> <span data-ttu-id="d61f2-127">Následující dotaz změní název "JménoZákazníka" klíč ve formátu JSON výstupu a vloží číslo telefonu a faxu v objektu dílčí "Kontakt":</span><span class="sxs-lookup"><span data-stu-id="d61f2-127">The following query changes the name of the "CustomerName" key in the output JSON format, and puts phone and fax numbers in the "Contact" sub-object:</span></span>

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

<span data-ttu-id="d61f2-128">Výstup tohoto dotazu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="d61f2-128">The output of this query looks like this:</span></span>

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

<span data-ttu-id="d61f2-129">V tomto příkladu jsme vrátil jeden objekt JSON místo pole tak, že zadáte [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) možnost.</span><span class="sxs-lookup"><span data-stu-id="d61f2-129">In this example we returned a single JSON object instead of an array by specifying the [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) option.</span></span> <span data-ttu-id="d61f2-130">Tuto možnost můžete použít, pokud víte, že jsou vrácení jednoho objektu. v důsledku dotazu.</span><span class="sxs-lookup"><span data-stu-id="d61f2-130">You can use this option if you know that you are returning a single object as a result of query.</span></span>

<span data-ttu-id="d61f2-131">Hlavní hodnota klauzule FOR JSON je, že umožňuje vrátit komplexní hierarchické data v databázi formátován jako vnořených objektů JSON nebo pole.</span><span class="sxs-lookup"><span data-stu-id="d61f2-131">The main value of the FOR JSON clause is that it lets you return complex hierarchical data from your database formatted as nested JSON objects or arrays.</span></span> <span data-ttu-id="d61f2-132">Následující příklad ukazuje, jak zahrnout objednávky, které patří do zákazníka jako vnořená pole objednávek:</span><span class="sxs-lookup"><span data-stu-id="d61f2-132">The following example shows how to include Orders that belong to the Customer as a nested array of Orders:</span></span>

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

<span data-ttu-id="d61f2-133">Místo abyste odesílali oddělené dotazy k získání dat zákazníků a pak se načíst seznam související objednávky, můžete získat všechna potřebná data pomocí jednoho dotazu, jak je znázorněno v následující ukázkový výstup:</span><span class="sxs-lookup"><span data-stu-id="d61f2-133">Instead of sending separate queries to get Customer data and then to fetch a list of related Orders, you can get all the necessary data with a single query, as shown in the following sample output:</span></span>

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

## <a name="working-with-json-data"></a><span data-ttu-id="d61f2-134">Práce s daty JSON</span><span class="sxs-lookup"><span data-stu-id="d61f2-134">Working with JSON data</span></span>
<span data-ttu-id="d61f2-135">Pokud nemáte výhradně strukturovaných dat, pokud máte komplexní dílčí objekty, pole nebo hierarchické data nebo datové struktury v průběhu času vyvíjejí, formát JSON můžete k reprezentaci všech komplexní datová struktura.</span><span class="sxs-lookup"><span data-stu-id="d61f2-135">If you don’t have strictly structured data, if you have complex sub-objects, arrays, or hierarchical data, or if your data structures evolve over time, the JSON format can help you to represent any complex data structure.</span></span>

<span data-ttu-id="d61f2-136">JSON je textový formát, který lze použít jako libovolný jiný typ řetězce ve službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="d61f2-136">JSON is a textual format that can be used like any other string type in Azure SQL Database.</span></span> <span data-ttu-id="d61f2-137">Můžete odeslat nebo jako standardní NVARCHAR úložiště dat JSON:</span><span class="sxs-lookup"><span data-stu-id="d61f2-137">You can send or store JSON data as a standard NVARCHAR:</span></span>

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

<span data-ttu-id="d61f2-138">Data JSON použité v tomto příkladu je reprezentována pomocí typu NVARCHAR(MAX).</span><span class="sxs-lookup"><span data-stu-id="d61f2-138">The JSON data used in this example is represented by using the NVARCHAR(MAX) type.</span></span> <span data-ttu-id="d61f2-139">JSON můžete vložit do této tabulky nebo zadaný jako argument uloženou proceduru pomocí standardní syntaxe Transact-SQL, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="d61f2-139">JSON can be inserted into this table or provided as an argument of the stored procedure using standard Transact-SQL syntax as shown in the following example:</span></span>

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

<span data-ttu-id="d61f2-140">Jazyk klienta nebo knihovny, která funguje s daty řetězec v databázi SQL Azure bude také pracovat s daty JSON.</span><span class="sxs-lookup"><span data-stu-id="d61f2-140">Any client-side language or library that works with string data in Azure SQL Database will also work with JSON data.</span></span> <span data-ttu-id="d61f2-141">JSON může být uložen v žádné tabulky, která podporuje typu NVARCHAR, jako je například paměťově optimalizované tabulky nebo tabulky se systémovou správou verzí.</span><span class="sxs-lookup"><span data-stu-id="d61f2-141">JSON can be stored in any table that supports the NVARCHAR type, such as a Memory-optimized table or a System-versioned table.</span></span> <span data-ttu-id="d61f2-142">JSON nezavádí žádné omezení v kódu na straně klienta nebo v databázové vrstvě.</span><span class="sxs-lookup"><span data-stu-id="d61f2-142">JSON does not introduce any constraint either in the client-side code or in the database layer.</span></span>

## <a name="querying-json-data"></a><span data-ttu-id="d61f2-143">Dotazování na JSON data</span><span class="sxs-lookup"><span data-stu-id="d61f2-143">Querying JSON data</span></span>
<span data-ttu-id="d61f2-144">Pokud máte data formátu JSON, ukládat do tabulek Azure SQL, funkce JSON umožňují používat tato data v jakékoli dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="d61f2-144">If you have data formatted as JSON stored in Azure SQL tables, JSON functions let you use this data in any SQL query.</span></span>

<span data-ttu-id="d61f2-145">Funkce JSON, které jsou k dispozici v Azure SQL database umožňují považovat datového naformátován jako formát JSON jako jiný typ dat SQL.</span><span class="sxs-lookup"><span data-stu-id="d61f2-145">JSON functions that are available in Azure SQL database let you treat data formatted as JSON as any other SQL data type.</span></span> <span data-ttu-id="d61f2-146">Můžete snadno rozbalte hodnot z textu JSON a použít JSON data v jakýkoli dotaz:</span><span class="sxs-lookup"><span data-stu-id="d61f2-146">You can easily extract values from the JSON text, and use JSON data in any query:</span></span>

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

<span data-ttu-id="d61f2-147">Funkce JSON_VALUE extrahuje hodnotu z textu JSON uložené ve sloupci Data.</span><span class="sxs-lookup"><span data-stu-id="d61f2-147">The JSON_VALUE function extracts a value from JSON text stored in the Data column.</span></span> <span data-ttu-id="d61f2-148">Tato funkce používá cestu jazyka JavaScript tak, aby odkazovaly na hodnotu v textu JSON k extrakci.</span><span class="sxs-lookup"><span data-stu-id="d61f2-148">This function uses a JavaScript-like path to reference a value in JSON text to extract.</span></span> <span data-ttu-id="d61f2-149">K extrahované hodnotě lze v libovolnou část dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="d61f2-149">The extracted value can be used in any part of SQL query.</span></span>

<span data-ttu-id="d61f2-150">Funkce JSON_QUERY je podobná JSON_VALUE.</span><span class="sxs-lookup"><span data-stu-id="d61f2-150">The JSON_QUERY function is similar to JSON_VALUE.</span></span> <span data-ttu-id="d61f2-151">Na rozdíl od JSON_VALUE tato funkce extrahuje komplexní dílčí objekt, jako je například pole nebo objekty, které jsou umístěny v textu JSON.</span><span class="sxs-lookup"><span data-stu-id="d61f2-151">Unlike JSON_VALUE, this function extracts complex sub-object such as arrays or objects that are placed in JSON text.</span></span>

<span data-ttu-id="d61f2-152">Funkce příkazu JSON_MODIFY umožňuje vám zadat cestu k hodnotě text JSON, který by měly být aktualizovány a také novou hodnotu, která přepíší starý.</span><span class="sxs-lookup"><span data-stu-id="d61f2-152">The JSON_MODIFY function lets you specify the path of the value in the JSON text that should be updated, as well as a new value that will overwrite the old one.</span></span> <span data-ttu-id="d61f2-153">Tímto způsobem můžete snadno aktualizovat v textu JSON bez reparsing celá struktura.</span><span class="sxs-lookup"><span data-stu-id="d61f2-153">This way you can easily update JSON text without reparsing the entire structure.</span></span>

<span data-ttu-id="d61f2-154">Vzhledem k tomu, že JSON je uložený ve formátu standardního textu, neexistují žádné záruky, že hodnotami uloženými v textu sloupce jsou ve správném formátu.</span><span class="sxs-lookup"><span data-stu-id="d61f2-154">Since JSON is stored in a standard text, there are no guarantees that the values stored in text columns are properly formatted.</span></span> <span data-ttu-id="d61f2-155">Můžete ověřit, že text uložený ve formátu JSON sloupce je ve správném formátu pomocí standardní omezení check Azure SQL Database a ISJSON funkce:</span><span class="sxs-lookup"><span data-stu-id="d61f2-155">You can verify that text stored in JSON column is properly formatted by using standard Azure SQL Database check constraints and the ISJSON function:</span></span>

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

<span data-ttu-id="d61f2-156">Pokud je ve správném formátu vstupního textu JSON, ISJSON funkce vrátí hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="d61f2-156">If the input text is properly formatted JSON, the ISJSON function returns the value 1.</span></span> <span data-ttu-id="d61f2-157">Na každém vložení nebo aktualizace sloupce JSON bude toto omezení ověřte novou textovou hodnotou není nesprávný formát JSON.</span><span class="sxs-lookup"><span data-stu-id="d61f2-157">On every insert or update of JSON column, this constraint will verify that new text value is not malformed JSON.</span></span>

## <a name="transforming-json-into-tabular-format"></a><span data-ttu-id="d61f2-158">Transformace v tabulkovém formátu JSON</span><span class="sxs-lookup"><span data-stu-id="d61f2-158">Transforming JSON into tabular format</span></span>
<span data-ttu-id="d61f2-159">Databáze SQL Azure také vám umožňuje transformovat kolekce JSON v tabulkovém formátu a zatížení nebo dotaz, data JSON.</span><span class="sxs-lookup"><span data-stu-id="d61f2-159">Azure SQL Database also lets you transform JSON collections into tabular format and load or query JSON data.</span></span>

<span data-ttu-id="d61f2-160">OPENJSON je funkce tabulky hodnota, která analyzuje JSON text, vyhledá pole objektů JSON, iteruje v rámci prvků pole a vrátí jeden řádek ve výsledném výstupu pro jednotlivé elementy pole.</span><span class="sxs-lookup"><span data-stu-id="d61f2-160">OPENJSON is a table-value function that parses JSON text, locates an array of JSON objects, iterates through the elements of the array, and returns one row in the output result for each element of the array.</span></span>

![Tabulkové JSON](./media/sql-database-json-features/image_2.png)

<span data-ttu-id="d61f2-162">V předchozím příkladu jsme můžete určit, kde najít pole JSON, který musí být otevřen (v $. Cesta objednávky), které sloupce má být vrácen jako výsledek a kde najít JSON hodnoty, které bude vrácen jako buněk.</span><span class="sxs-lookup"><span data-stu-id="d61f2-162">In the example above, we can specify where to locate the JSON array that should be opened (in the $.Orders path), what columns should be returned as result, and where to find the JSON values that will be returned as cells.</span></span>

<span data-ttu-id="d61f2-163">Pole JSON v jsme můžete převést @orders proměnné do sady řádků, analyzovat tuto sadu výsledků nebo vložení řádků do tabulky Standardní:</span><span class="sxs-lookup"><span data-stu-id="d61f2-163">We can transform a JSON array in the @orders variable into a set of rows, analyze this result set, or insert rows into a standard table:</span></span>

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
<span data-ttu-id="d61f2-164">Kolekce objednávek naformátovaný jako pole JSON a zadat jako parametr uložené procedury je možné analyzovat a vložit do tabulky objednávky.</span><span class="sxs-lookup"><span data-stu-id="d61f2-164">The collection of orders formatted as a JSON array and provided as a parameter to the stored procedure can be parsed and inserted into the Orders table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d61f2-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d61f2-165">Next steps</span></span>
<span data-ttu-id="d61f2-166">Informace o tom, jak integrovat JSON do vaší aplikace, podívejte se na tyto prostředky:</span><span class="sxs-lookup"><span data-stu-id="d61f2-166">To learn how to integrate JSON into your application, check out these resources:</span></span>

* [<span data-ttu-id="d61f2-167">Blog na TechNetu</span><span class="sxs-lookup"><span data-stu-id="d61f2-167">TechNet Blog</span></span>](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
* [<span data-ttu-id="d61f2-168">Dokumentace MSDN</span><span class="sxs-lookup"><span data-stu-id="d61f2-168">MSDN documentation</span></span>](https://msdn.microsoft.com/library/dn921897.aspx)
* [<span data-ttu-id="d61f2-169">Channel 9 video</span><span class="sxs-lookup"><span data-stu-id="d61f2-169">Channel 9 video</span></span>](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

<span data-ttu-id="d61f2-170">Další informace o různé scénáře pro integraci JSON do své aplikace naleznete v tématu ukázky v tomto [Channel 9 video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) nebo najít scénáře, který odpovídá váš případ použití v [JSON příspěvky](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).</span><span class="sxs-lookup"><span data-stu-id="d61f2-170">To learn about various scenarios for integrating JSON into your application, see the demos in this [Channel 9 video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) or find a scenario that matches your use case in [JSON Blog posts](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).</span></span>

