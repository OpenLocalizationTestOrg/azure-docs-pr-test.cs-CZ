---
title: "Azure DocumentDB Cosmos DB rozhraní API: Syntaxe SQL | Microsoft Docs"
description: "Referenční dokumentace pro jazyk dotazu SQL rozhraní API Azure Cosmos databáze DocumentDB."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 06/13/2017
ms.author: mimig
ms.openlocfilehash: 63b2d20c74df4fd6173994ee1a727594ba8afba3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-documentdb-api-sql-syntax-reference"></a><span data-ttu-id="bc057-103">Azure DocumentDB Cosmos DB rozhraní API: Reference syntaxe SQL</span><span class="sxs-lookup"><span data-stu-id="bc057-103">Azure Cosmos DB DocumentDB API: SQL syntax reference</span></span>

<span data-ttu-id="bc057-104">Rozhraní API služby Azure DB Cosmos DocumentDB podporuje dotazování dokumentů pomocí známých SQL (Structured Query Language) jako gramatika přes hierarchické dokumenty JSON bez nutnosti explicitního schématu nebo vytváření sekundárních indexů.</span><span class="sxs-lookup"><span data-stu-id="bc057-104">The Azure Cosmos DB DocumentDB API supports querying documents using a familiar SQL (Structured Query Language) like grammar over hierarchical JSON documents without requiring explicit schema or creation of secondary indexes.</span></span> <span data-ttu-id="bc057-105">Toto téma obsahuje referenční dokumentaci k nástroji pro dotazovací jazyk DocumentDB SQL rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="bc057-105">This topic provides reference documentation for the DocumentDB API SQL query language.</span></span>

<span data-ttu-id="bc057-106">Návod dotazovací jazyk DocumentDB SQL rozhraní API najdete v tématu [dotazy SQL pro rozhraní API služby Azure Cosmos databáze DocumentDB](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="bc057-106">For a walkthrough of the DocumentDB API SQL query language, see [SQL queries for Azure Cosmos DB DocumentDB API](documentdb-sql-query.md).</span></span>  
  
<span data-ttu-id="bc057-107">Také doporučujeme přejděte [Query Playground](http://www.documentdb.com/sql/demo) kde mohou zkuste Azure Cosmos DB a spouštění dotazů SQL na naší datové sadě.</span><span class="sxs-lookup"><span data-stu-id="bc057-107">We also invite you to visit the [Query Playground](http://www.documentdb.com/sql/demo) where you can try Azure Cosmos DB and run SQL queries against our dataset.</span></span>  
  
## <a name="select-query"></a><span data-ttu-id="bc057-108">Vyberte možnost dotazu</span><span class="sxs-lookup"><span data-stu-id="bc057-108">SELECT query</span></span>  
<span data-ttu-id="bc057-109">Načte z databáze dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="bc057-109">Retrieves JSON documents from the database.</span></span> <span data-ttu-id="bc057-110">Podporuje vyhodnocení výrazu, projekce, filtrování a připojí.</span><span class="sxs-lookup"><span data-stu-id="bc057-110">Supports expression evaluation, projections, filtering and joins.</span></span>  <span data-ttu-id="bc057-111">V části syntaxe názvů jsou v tabulce konvencemi použitými pro popisující příkazů SELECT.</span><span class="sxs-lookup"><span data-stu-id="bc057-111">The conventions used for describing the SELECT statements are tabulated in the Syntax conventions section.</span></span>  
  
<span data-ttu-id="bc057-112">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-112">**Syntax**</span></span>  
  
```
<select_query> ::=  
SELECT <select_specification>   
    [ FROM <from_specification>]   
    [ WHERE <filter_condition> ]  
    [ ORDER BY <sort_specification> ]  
```  
  
 <span data-ttu-id="bc057-113">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="bc057-113">**Remarks**</span></span>  
  
 <span data-ttu-id="bc057-114">Viz následující části Podrobnosti na každý klauzule:</span><span class="sxs-lookup"><span data-stu-id="bc057-114">See following sections for details on each clause:</span></span>  
  
-   [<span data-ttu-id="bc057-115">Klauzule SELECT</span><span class="sxs-lookup"><span data-stu-id="bc057-115">SELECT clause</span></span>](#bk_select_query)  
  
-   [<span data-ttu-id="bc057-116">FROM – klauzule</span><span class="sxs-lookup"><span data-stu-id="bc057-116">FROM clause</span></span>](#bk_from_clause)  
  
-   [<span data-ttu-id="bc057-117">Klauzule WHERE</span><span class="sxs-lookup"><span data-stu-id="bc057-117">WHERE clause</span></span>](#bk_where_clause)  
  
-   [<span data-ttu-id="bc057-118">Klauzuli ORDER by</span><span class="sxs-lookup"><span data-stu-id="bc057-118">ORDER BY clause</span></span>](#bk_orderby_clause)  
  
<span data-ttu-id="bc057-119">Klauzule v příkazu SELECT musejí být seřazeny, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="bc057-119">The clauses in the SELECT statement must be ordered as shown above.</span></span> <span data-ttu-id="bc057-120">Kterákoli z volitelné klauzule lze vynechat.</span><span class="sxs-lookup"><span data-stu-id="bc057-120">Any one of the optional clauses can be omitted.</span></span> <span data-ttu-id="bc057-121">Ale pokud volitelné klauzule používají, musí být ve správném pořadí.</span><span class="sxs-lookup"><span data-stu-id="bc057-121">But when optional clauses are used, they must appear in the right order.</span></span>  
  
<span data-ttu-id="bc057-122">**Logické pořadí zpracování příkazu SELECT**</span><span class="sxs-lookup"><span data-stu-id="bc057-122">**Logical Processing Order of the SELECT statement**</span></span>  
  
<span data-ttu-id="bc057-123">Pořadí, ve kterém jsou zpracovány klauzule je:</span><span class="sxs-lookup"><span data-stu-id="bc057-123">The order in which clauses are processed is:</span></span>  

1.  [<span data-ttu-id="bc057-124">FROM – klauzule</span><span class="sxs-lookup"><span data-stu-id="bc057-124">FROM clause</span></span>](#bk_from_clause)  
2.  [<span data-ttu-id="bc057-125">Klauzule WHERE</span><span class="sxs-lookup"><span data-stu-id="bc057-125">WHERE clause</span></span>](#bk_where_clause)  
3.  [<span data-ttu-id="bc057-126">Klauzuli ORDER by</span><span class="sxs-lookup"><span data-stu-id="bc057-126">ORDER BY clause</span></span>](#bk_orderby_clause)  
4.  [<span data-ttu-id="bc057-127">Klauzule SELECT</span><span class="sxs-lookup"><span data-stu-id="bc057-127">SELECT clause</span></span>](#bk_select_query)  

<span data-ttu-id="bc057-128">Všimněte si, že se jedná o liší od pořadí, ve kterém se zobrazí v syntaxi.</span><span class="sxs-lookup"><span data-stu-id="bc057-128">Note that this is different from the order in which they appear in the syntax.</span></span> <span data-ttu-id="bc057-129">Pořadí je tak, aby všechny nové symboly zavedených v klauzuli zpracovaná viditelné a zda lze použít v klauzulích zpracovat později.</span><span class="sxs-lookup"><span data-stu-id="bc057-129">The ordering is such that all new symbols introduced by a processed clause are visible and can be used in clauses processed later.</span></span> <span data-ttu-id="bc057-130">Pro instanci, jsou dostupné aliasy deklarované v klauzuli FROM tam, kde a klauzule FROM.</span><span class="sxs-lookup"><span data-stu-id="bc057-130">For instance, aliases declared in a FROM clause are accessible in WHERE and SELECT clauses.</span></span>  

<span data-ttu-id="bc057-131">**Prázdné znaky a komentáře**</span><span class="sxs-lookup"><span data-stu-id="bc057-131">**Whitespace characters and comments**</span></span>  

<span data-ttu-id="bc057-132">Všechny znaky prázdné znaky, které nejsou součástí řetězec v uvozovkách nebo identifikátor v uvozovkách nejsou součástí gramatika jazyka a jsou ignorovány během analýzy.</span><span class="sxs-lookup"><span data-stu-id="bc057-132">All white space characters which are not part of a quoted string or quoted identifier are not part of the language grammar and are ignored during parsing.</span></span>  

<span data-ttu-id="bc057-133">Dotazovací jazyk podporuje komentáře styl T-SQL jako</span><span class="sxs-lookup"><span data-stu-id="bc057-133">The query language supports T-SQL style comments like</span></span>  

-   <span data-ttu-id="bc057-134">Příkaz jazyka SQL`-- comment text [newline]`</span><span class="sxs-lookup"><span data-stu-id="bc057-134">SQL Statement `-- comment text [newline]`</span></span>  

<span data-ttu-id="bc057-135">Prázdné znaky a komentáře v není k dispozici žádné násobek gramatiku, musí se používají k oddělení tokeny.</span><span class="sxs-lookup"><span data-stu-id="bc057-135">While whitespace characters and comments do not have any significance in the grammar, they must be used to separate tokens.</span></span> <span data-ttu-id="bc057-136">Pro instanci: `-1e5` je jeden číslo tokenu, chvíli`: – 1 e5` token znaménkem minus následuje identifikátor e5 a číslo 1.</span><span class="sxs-lookup"><span data-stu-id="bc057-136">For instance: `-1e5` is a single number token, while`: – 1 e5` is a minus token followed by number 1 and identifier e5.</span></span>  

##  <span data-ttu-id="bc057-137"><a name="bk_select_query"></a>Klauzule SELECT</span><span class="sxs-lookup"><span data-stu-id="bc057-137"><a name="bk_select_query"></a> SELECT clause</span></span>  
<span data-ttu-id="bc057-138">Klauzule v příkazu SELECT musejí být seřazeny, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="bc057-138">The clauses in the SELECT statement must be ordered as shown above.</span></span> <span data-ttu-id="bc057-139">Kterákoli z volitelné klauzule lze vynechat.</span><span class="sxs-lookup"><span data-stu-id="bc057-139">Any one of the optional clauses can be omitted.</span></span> <span data-ttu-id="bc057-140">Ale pokud volitelné klauzule používají, musí být ve správném pořadí.</span><span class="sxs-lookup"><span data-stu-id="bc057-140">But when optional clauses are used, they must appear in the right order.</span></span>  

<span data-ttu-id="bc057-141">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-141">**Syntax**</span></span>  
```  
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | <object_property_list>   
      | VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
 <span data-ttu-id="bc057-142">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-142">**Arguments**</span></span>  
  
 `<select_specification>`  
  
 <span data-ttu-id="bc057-143">Vlastnosti nebo hodnota, která má být vybrán pro sadu výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="bc057-143">Properties or value to be selected for the result set.</span></span>  
  
 `'*'`  
  
<span data-ttu-id="bc057-144">Určuje, zda mají být načtena hodnota beze změn.</span><span class="sxs-lookup"><span data-stu-id="bc057-144">Specifies that the value should be retrieved without making any changes.</span></span> <span data-ttu-id="bc057-145">Konkrétně Pokud zpracovaná hodnota objektu, budou všechny vlastnosti načteny.</span><span class="sxs-lookup"><span data-stu-id="bc057-145">Specifically if the processed value is an object, all properties will be retrieved.</span></span>  
  
 `<object_property_list>`  
  
<span data-ttu-id="bc057-146">Určuje seznam vlastností, které mají být načteny.</span><span class="sxs-lookup"><span data-stu-id="bc057-146">Specifies the list of properties to be retrieved.</span></span> <span data-ttu-id="bc057-147">Každý vrácená hodnota bude objekt s byly zadány vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bc057-147">Each returned value will be an object with the properties specified.</span></span>  
  
`VALUE`  
  
<span data-ttu-id="bc057-148">Určuje, zda mají být načtena hodnota JSON místo kompletní objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="bc057-148">Specifies that the JSON value should be retrieved instead of the complete JSON object.</span></span> <span data-ttu-id="bc057-149">To, na rozdíl od `<property_list>` nezalamuje očekávaná hodnota v objektu.</span><span class="sxs-lookup"><span data-stu-id="bc057-149">This, unlike `<property_list>` does not wrap the projected value in an object.</span></span>  
  
`<scalar_expression>`  
  
<span data-ttu-id="bc057-150">Výraz představující hodnotu počítaný.</span><span class="sxs-lookup"><span data-stu-id="bc057-150">Expression representing the value to be computed.</span></span> <span data-ttu-id="bc057-151">V tématu [skalární výrazy](#bk_scalar_expressions) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bc057-151">See [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
<span data-ttu-id="bc057-152">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="bc057-152">**Remarks**</span></span>  
  
<span data-ttu-id="bc057-153">`SELECT *` Syntaxe je platný, pokud klauzule FROM deklaruje právě jednu alias.</span><span class="sxs-lookup"><span data-stu-id="bc057-153">The `SELECT *` syntax is only valid if FROM clause has declared exactly one alias.</span></span> <span data-ttu-id="bc057-154">`SELECT *`poskytuje identitu projekce, které mohou být užitečné, pokud není nutná žádná projekce.</span><span class="sxs-lookup"><span data-stu-id="bc057-154">`SELECT *` provides an identity projection, which can be useful if no projection is needed.</span></span> <span data-ttu-id="bc057-155">Vyberte * platí, pouze pokud je zadána klauzule FROM a zavedl pouze jeden vstupní zdroj.</span><span class="sxs-lookup"><span data-stu-id="bc057-155">SELECT * is only valid if FROM clause is specified and introduced only a single input source.</span></span>  
  
<span data-ttu-id="bc057-156">Všimněte si, že `SELECT <select_list>` a `SELECT *` jsou "syntaktické sugar" a může být případně vyjádřený pomocí jednoduchého příkazů SELECT, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="bc057-156">Note that `SELECT <select_list>` and `SELECT *` are "syntactic sugar" and can be alternatively expressed by using simple SELECT statements as shown below.</span></span>  
  
1.  `SELECT * FROM ... AS from_alias ...`  
  
     <span data-ttu-id="bc057-157">je ekvivalentní:</span><span class="sxs-lookup"><span data-stu-id="bc057-157">is equivalent to:</span></span>  
  
     `SELECT from_alias FROM ... AS from_alias ...`  
  
2.  `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
     <span data-ttu-id="bc057-158">je ekvivalentní:</span><span class="sxs-lookup"><span data-stu-id="bc057-158">is equivalent to:</span></span>  
  
     `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
<span data-ttu-id="bc057-159">**Viz také**</span><span class="sxs-lookup"><span data-stu-id="bc057-159">**See Also**</span></span>  
  
[<span data-ttu-id="bc057-160">Skalární výrazy</span><span class="sxs-lookup"><span data-stu-id="bc057-160">Scalar expressions</span></span>](#bk_scalar_expressions)  
[<span data-ttu-id="bc057-161">Klauzule SELECT</span><span class="sxs-lookup"><span data-stu-id="bc057-161">SELECT clause</span></span>](#bk_select_query)  
  
##  <span data-ttu-id="bc057-162"><a name="bk_from_clause"></a>FROM – klauzule</span><span class="sxs-lookup"><span data-stu-id="bc057-162"><a name="bk_from_clause"></a> FROM clause</span></span>  
<span data-ttu-id="bc057-163">Určuje zdroj nebo připojený k zdroje.</span><span class="sxs-lookup"><span data-stu-id="bc057-163">Specifies the source or joined sources.</span></span> <span data-ttu-id="bc057-164">V klauzuli FROM je volitelný.</span><span class="sxs-lookup"><span data-stu-id="bc057-164">The FROM clause is optional.</span></span> <span data-ttu-id="bc057-165">Pokud není zadaný, další klauzule bude proveden stále jako, pokud klauzule FROM zadaný jednotlivý dokument.</span><span class="sxs-lookup"><span data-stu-id="bc057-165">If not specified, other clauses will still be executed as if FROM clause provided a single document.</span></span>  
  
<span data-ttu-id="bc057-166">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-166">**Syntax**</span></span>  
  
```  
FROM <from_specification>  
  
<from_specification> ::=   
        <from_source> {[ JOIN <from_source>][,...n]}  
  
<from_source> ::=   
          <collection_expression> [[AS] input_alias]  
        | input_alias IN <collection_expression>  
  
<collection_expression> ::=   
        ROOT   
     | collection_name  
     | input_alias  
     | <collection_expression> '.' property_name  
     | <collection_expression> '[' "property_name" | array_index ']'  
```  
  
<span data-ttu-id="bc057-167">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-167">**Arguments**</span></span>  
  
`<from_source>`  
  
<span data-ttu-id="bc057-168">Určuje zdroj dat s nebo bez alias.</span><span class="sxs-lookup"><span data-stu-id="bc057-168">Specifies a data source, with or without an alias.</span></span> <span data-ttu-id="bc057-169">Pokud není zadán alias, bude odvodit z `<collection_expression>` pomocí následujících pravidel:</span><span class="sxs-lookup"><span data-stu-id="bc057-169">If alias is not specified, it will be inferred from the `<collection_expression>` using following rules:</span></span>  
  
-   <span data-ttu-id="bc057-170">Pokud ve výrazu název_kolekce, bude Název_kolekce použít jako alias.</span><span class="sxs-lookup"><span data-stu-id="bc057-170">If the expression is a collection_name, then collection_name will be used as an alias.</span></span>  
  
-   <span data-ttu-id="bc057-171">Pokud je výraz `<collection_expression>`, pak %{Property_Name/ pak %{Property_Name/ se použije jako alias.</span><span class="sxs-lookup"><span data-stu-id="bc057-171">If the expression is `<collection_expression>`, then property_name, then property_name will be used as an alias.</span></span> <span data-ttu-id="bc057-172">Pokud ve výrazu název_kolekce, bude Název_kolekce použít jako alias.</span><span class="sxs-lookup"><span data-stu-id="bc057-172">If the expression is a collection_name, then collection_name will be used as an alias.</span></span>  
  
<span data-ttu-id="bc057-173">STEJNĚ JAKO`input_alias`</span><span class="sxs-lookup"><span data-stu-id="bc057-173">AS `input_alias`</span></span>  
  
<span data-ttu-id="bc057-174">Určuje, že `input_alias` je sada hodnot vrácených základní výraz kolekce.</span><span class="sxs-lookup"><span data-stu-id="bc057-174">Specifies that the `input_alias` is a set of values returned by the underlying collection expression.</span></span>  
 
<span data-ttu-id="bc057-175">`input_alias`V</span><span class="sxs-lookup"><span data-stu-id="bc057-175">`input_alias` IN</span></span>  
  
<span data-ttu-id="bc057-176">Určuje, že `input_alias` by měl představovat sadu hodnot získat iterování přes všechny elementy pole každé pole vrácené výrazem základní kolekce.</span><span class="sxs-lookup"><span data-stu-id="bc057-176">Specifies that the `input_alias` should represent the set of values obtained by iterating over all array elements of each array returned by the underlying collection expression.</span></span> <span data-ttu-id="bc057-177">Libovolná hodnota vrácený základní kolekce výraz, který není pole se ignoruje.</span><span class="sxs-lookup"><span data-stu-id="bc057-177">Any value returned by underlying collection expression that is not an array is ignored.</span></span>  
  
`<collection_expression>`  
  
<span data-ttu-id="bc057-178">Určuje výraz kolekce, který má použít k načtení dokumenty.</span><span class="sxs-lookup"><span data-stu-id="bc057-178">Specifies the collection expression to be used to retrieve the documents.</span></span>  
  
`ROOT`  
  
<span data-ttu-id="bc057-179">Určuje, že tento dokument se mají načíst výchozí kolekci aktuálně připojené.</span><span class="sxs-lookup"><span data-stu-id="bc057-179">Specifies that document should be retrieved from the default, currently connected collection.</span></span>  
  
`collection_name`  
  
<span data-ttu-id="bc057-180">Určuje, že tento dokument mají být načtena ze zadané kolekci.</span><span class="sxs-lookup"><span data-stu-id="bc057-180">Specifies that document should be retrieved from the provided collection.</span></span> <span data-ttu-id="bc057-181">Název kolekce musí odpovídat názvu aktuálně připojených do kolekce.</span><span class="sxs-lookup"><span data-stu-id="bc057-181">The name of the collection must match the name of the collection currently connected to.</span></span>  
  
`input_alias`  
  
<span data-ttu-id="bc057-182">Určuje, že tento dokument mají být načtena z jiného zdroje definované zadaný alias.</span><span class="sxs-lookup"><span data-stu-id="bc057-182">Specifies that document should be retrieved from the other source defined by the provided alias.</span></span>  
  
`<collection_expression> '.' property_`  
  
<span data-ttu-id="bc057-183">Určuje v tomto dokumentu mají být načtena přímým přístupem `property_name` element pole vlastnosti nebo array_index pro všechny dokumenty načíst zadaný výraz kolekce.</span><span class="sxs-lookup"><span data-stu-id="bc057-183">Specifies that document should be retrieved by accessing the `property_name` property or array_index array element for all documents retrieved by specified collection expression.</span></span>  
  
`<collection_expression> '[' "property_name" | array_index ']'`  
  
<span data-ttu-id="bc057-184">Určuje v tomto dokumentu mají být načtena přímým přístupem `property_name` element pole vlastnosti nebo array_index pro všechny dokumenty načíst zadaný výraz kolekce.</span><span class="sxs-lookup"><span data-stu-id="bc057-184">Specifies that document should be retrieved by accessing the `property_name` property or array_index array element for all documents retrieved by specified collection expression.</span></span>  
  
<span data-ttu-id="bc057-185">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="bc057-185">**Remarks**</span></span>  
  
<span data-ttu-id="bc057-186">Všechny aliasy zadáno nebo odvození v `<from_source>(`s) musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="bc057-186">All aliases provided or inferred in the `<from_source>(`s) must be unique.</span></span> <span data-ttu-id="bc057-187">Syntaxe `<collection_expression>.`%{Property_Name/ je stejný jako `<collection_expression>' ['"property_name"']'`.</span><span class="sxs-lookup"><span data-stu-id="bc057-187">The Syntax `<collection_expression>.`property_name is the same as `<collection_expression>' ['"property_name"']'`.</span></span> <span data-ttu-id="bc057-188">Druhé syntaxe však může být použita, pokud název vlastnosti obsahuje jiný identifikátor – znaky.</span><span class="sxs-lookup"><span data-stu-id="bc057-188">However, the latter syntax can be used if a property name contains a non-identifier characters.</span></span>  
  
<span data-ttu-id="bc057-189">**Chybí vlastnosti, chybějící pole prvky, není definovaná hodnoty zpracování**</span><span class="sxs-lookup"><span data-stu-id="bc057-189">**Missing properties, missing array elements, undefined values handling**</span></span>  
  
<span data-ttu-id="bc057-190">Pokud výraz kolekce přístup k vlastnosti nebo elementy pole a že hodnota neexistuje, bude se tato hodnota ignorována a není další zpracování.</span><span class="sxs-lookup"><span data-stu-id="bc057-190">If a collection expression accesses properties or array elements and that value does not exist, that value will be ignored and not processed further.</span></span>  
  
<span data-ttu-id="bc057-191">**Kolekce výraz kontextu rozsahu**</span><span class="sxs-lookup"><span data-stu-id="bc057-191">**Collection expression context scoping**</span></span>  
  
<span data-ttu-id="bc057-192">Výraz kolekce může být celou kolekci nebo obor dokumentu:</span><span class="sxs-lookup"><span data-stu-id="bc057-192">A collection expression may be collection-scoped or document-scoped:</span></span>  
  
-   <span data-ttu-id="bc057-193">Výraz je celou kolekci, pokud je základní zdroj výrazu kolekce buď KOŘENOVÉ nebo `collection_name`.</span><span class="sxs-lookup"><span data-stu-id="bc057-193">An expression is collection-scoped, if the underlying source of the collection expression is either ROOT or `collection_name`.</span></span> <span data-ttu-id="bc057-194">Takové výraz představuje sadu dokumenty načíst přímo z kolekce a není závislá na zpracování jiné kolekce výrazy.</span><span class="sxs-lookup"><span data-stu-id="bc057-194">Such an expression represents a set of documents retrieved from the collection directly, and is not dependent on the processing of other collection expressions.</span></span>  
  
-   <span data-ttu-id="bc057-195">Výraz je dokument s rozsahem, pokud je základní zdroj výrazu kolekce `input_alias` zavedená dříve v dotazu.</span><span class="sxs-lookup"><span data-stu-id="bc057-195">An expression is document-scoped, if the underlying source of the collection expression is `input_alias` introduced earlier in the query.</span></span> <span data-ttu-id="bc057-196">Takové výraz představuje sadu dokumenty získala při vyhodnocování výrazu kolekce v oboru jednotlivých dokumentů, které patří do sady přidružené ke kolekci alias.</span><span class="sxs-lookup"><span data-stu-id="bc057-196">Such an expression represents a set of documents obtained by evaluating the collection expression in the scope of each document belonging to the set associated with the aliased collection.</span></span>  <span data-ttu-id="bc057-197">Výsledná sada bude spojení získala při vyhodnocování výrazu kolekce pro jednotlivé dokumenty v sadě základní sady.</span><span class="sxs-lookup"><span data-stu-id="bc057-197">The resulting set will be a union of sets obtained by evaluating the collection expression for each of the documents in the underlying set.</span></span>  
  
<span data-ttu-id="bc057-198">**Spojení**</span><span class="sxs-lookup"><span data-stu-id="bc057-198">**Joins**</span></span>  
  
<span data-ttu-id="bc057-199">V aktuální verzi podporuje Azure Cosmos DB vnitřní spojení.</span><span class="sxs-lookup"><span data-stu-id="bc057-199">In the current release, Azure Cosmos DB supports inner joins.</span></span> <span data-ttu-id="bc057-200">Připojení k další možnosti jsou chystaný.</span><span class="sxs-lookup"><span data-stu-id="bc057-200">Additional join capabilities are forthcoming.</span></span>

<span data-ttu-id="bc057-201">Vnitřní spojení vést k dokončení smíšený produkt sad účastní spojení.</span><span class="sxs-lookup"><span data-stu-id="bc057-201">Inner joins result in a complete cross product of the sets participating in the join.</span></span> <span data-ttu-id="bc057-202">Výsledek spojení N-způsob, jak je sada N element řazené kolekce členů, kde každá hodnota v řazené kolekci členů je spojena s alias nastavit účasti ve spojení a je přístupný pomocí odkazující na tento alias v jiných klauzulích.</span><span class="sxs-lookup"><span data-stu-id="bc057-202">The result of an N-way join is a set of N-element tuples, where each value in the tuple is associated with the aliased set participating in the join and can be accessed by referencing that alias in other clauses.</span></span>  
  
<span data-ttu-id="bc057-203">Připojení k vyhodnocení, závisí na oboru kontextu zúčastněných sad:</span><span class="sxs-lookup"><span data-stu-id="bc057-203">The evaluation of the join depends on the context scope of the participating sets:</span></span>  
  
-  <span data-ttu-id="bc057-204">Spojení mezi sadu kolekce A a celou kolekci nastavení B, výsledky v produktu mezi všechny elementy ve sady A a B.</span><span class="sxs-lookup"><span data-stu-id="bc057-204">A join between collection-set A and collection-scoped set B, results in a cross product of all elements in sets A and B.</span></span>
  
-   <span data-ttu-id="bc057-205">Spojení mezi sada A a sada obor dokumentu B, výsledkem spojení všech sad získat vyhodnocením dokumentu obor sady B pro každý dokument z nastavit A.</span><span class="sxs-lookup"><span data-stu-id="bc057-205">A join between set A and document-scoped set B, results in a union of all sets obtained by evaluating document-scoped set B for each document from set A.</span></span>  
  
 <span data-ttu-id="bc057-206">V aktuální verzi maximálně jeden výraz celou kolekci podporuje procesor dotazů.</span><span class="sxs-lookup"><span data-stu-id="bc057-206">In the current release, a maximum of one collection-scoped expression is supported by the query processor.</span></span>  
  
<span data-ttu-id="bc057-207">**Příklady spojení:**</span><span class="sxs-lookup"><span data-stu-id="bc057-207">**Examples of joins:**</span></span>  
  
<span data-ttu-id="bc057-208">Podívejme se na následující klauzule FROM:`<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`</span><span class="sxs-lookup"><span data-stu-id="bc057-208">Let's look at the following FROM clause: `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`</span></span>  
  
 <span data-ttu-id="bc057-209">Umožní každý zdroj definovat `input_alias1, input_alias2, …, input_aliasN`.</span><span class="sxs-lookup"><span data-stu-id="bc057-209">Let each source define `input_alias1, input_alias2, …, input_aliasN`.</span></span> <span data-ttu-id="bc057-210">Tato klauzule FROM vrací sadu N-řazené kolekce členů (řazené kolekce členů s hodnotami N).</span><span class="sxs-lookup"><span data-stu-id="bc057-210">This FROM clause returns a set of N-tuples (tuple with N values).</span></span> <span data-ttu-id="bc057-211">Každá řazená kolekce členů má vyprodukované všechny aliasy kolekce iterování přes jejich příslušné sady hodnot.</span><span class="sxs-lookup"><span data-stu-id="bc057-211">Each tuple has values produced by iterating all collection aliases over their respective sets.</span></span>  
  
<span data-ttu-id="bc057-212">*Připojit ke Příklad 1, s 2 zdrojů:*</span><span class="sxs-lookup"><span data-stu-id="bc057-212">*JOIN example 1, with 2 sources:*</span></span>  
  
- <span data-ttu-id="bc057-213">Umožní `<from_source1>` celou kolekci být a představují sadu {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="bc057-213">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="bc057-214">Umožní `<from_source2>` být obor dokumentu, odkazující na input_alias1 a představují sady:</span><span class="sxs-lookup"><span data-stu-id="bc057-214">Let `<from_source2>` be document-scoped referencing input_alias1 and represent sets:</span></span>  
  
    <span data-ttu-id="bc057-215">{1, 2} pro`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="bc057-215">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="bc057-216">{3} pro`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="bc057-216">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="bc057-217">{4, 5} pro`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="bc057-217">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="bc057-218">V klauzuli FROM `<from_source1> JOIN <from_source2>` bude mít za následek následující řazené kolekce členů:</span><span class="sxs-lookup"><span data-stu-id="bc057-218">The FROM clause `<from_source1> JOIN <from_source2>` will result in the following tuples:</span></span>  
  
    <span data-ttu-id="bc057-219">(`input_alias1, input_alias2`):</span><span class="sxs-lookup"><span data-stu-id="bc057-219">(`input_alias1, input_alias2`):</span></span>  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
<span data-ttu-id="bc057-220">*Příklad 2, s 3 zdroje připojení:*</span><span class="sxs-lookup"><span data-stu-id="bc057-220">*JOIN example 2, with 3 sources:*</span></span>  
  
- <span data-ttu-id="bc057-221">Umožní `<from_source1>` celou kolekci být a představují sadu {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="bc057-221">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="bc057-222">Umožní `<from_source2>` být obor dokumentu odkazující na `input_alias1` a představují sady:</span><span class="sxs-lookup"><span data-stu-id="bc057-222">Let `<from_source2>` be document-scoped referencing `input_alias1` and represent sets:</span></span>  
  
    <span data-ttu-id="bc057-223">{1, 2} pro`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="bc057-223">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="bc057-224">{3} pro`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="bc057-224">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="bc057-225">{4, 5} pro`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="bc057-225">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="bc057-226">Umožní `<from_source3>` být obor dokumentu odkazující na `input_alias2` a představují sady:</span><span class="sxs-lookup"><span data-stu-id="bc057-226">Let `<from_source3>` be document-scoped referencing `input_alias2` and represent sets:</span></span>  
  
    <span data-ttu-id="bc057-227">{100, 200} pro`input_alias2 = 1,`</span><span class="sxs-lookup"><span data-stu-id="bc057-227">{100, 200} for `input_alias2 = 1,`</span></span>  
  
    <span data-ttu-id="bc057-228">{300} pro`input_alias2 = 3,`</span><span class="sxs-lookup"><span data-stu-id="bc057-228">{300} for `input_alias2 = 3,`</span></span>  
  
- <span data-ttu-id="bc057-229">V klauzuli FROM `<from_source1> JOIN <from_source2> JOIN <from_source3>` bude mít za následek následující řazené kolekce členů:</span><span class="sxs-lookup"><span data-stu-id="bc057-229">The FROM clause `<from_source1> JOIN <from_source2> JOIN <from_source3>` will result in the following tuples:</span></span>  
  
    <span data-ttu-id="bc057-230">(input_alias1, input_alias2, input_alias3):</span><span class="sxs-lookup"><span data-stu-id="bc057-230">(input_alias1, input_alias2, input_alias3):</span></span>  
  
    <span data-ttu-id="bc057-231">(A, 1, 100), (A, 1, 200), (B, 3, 300)</span><span class="sxs-lookup"><span data-stu-id="bc057-231">(A, 1, 100), (A, 1, 200), (B, 3, 300)</span></span>  
  
> [!NOTE]
> <span data-ttu-id="bc057-232">Nedostatečná řazené kolekce členů pro jiné hodnoty `input_alias1`, `input_alias2`, pro kterou `<from_source3>` nevrátil žádné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bc057-232">Lack of tuples for other values of `input_alias1`, `input_alias2`, for which the `<from_source3>` did not return any values.</span></span>  
  
<span data-ttu-id="bc057-233">*Připojit ke příklad 3, s 3 zdrojů:*</span><span class="sxs-lookup"><span data-stu-id="bc057-233">*JOIN example 3, with 3 sources:*</span></span>  
  
- <span data-ttu-id="bc057-234">Umožní < from_source1 > být celou kolekci a představují sadu {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="bc057-234">Let <from_source1> be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="bc057-235">Umožní `<from_source1>` celou kolekci být a představují sadu {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="bc057-235">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="bc057-236">Umožní < from_source2 > být obor dokumentu odkazující input_alias1 a představují sady:</span><span class="sxs-lookup"><span data-stu-id="bc057-236">Let <from_source2> be document-scoped referencing input_alias1 and represent sets:</span></span>  
  
    <span data-ttu-id="bc057-237">{1, 2} pro`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="bc057-237">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="bc057-238">{3} pro`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="bc057-238">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="bc057-239">{4, 5} pro`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="bc057-239">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="bc057-240">Umožní `<from_source3>` být omezená na `input_alias1` a představují sady:</span><span class="sxs-lookup"><span data-stu-id="bc057-240">Let `<from_source3>` be scoped to `input_alias1` and represent sets:</span></span>  
  
    <span data-ttu-id="bc057-241">{100, 200} pro`input_alias2 = A,`</span><span class="sxs-lookup"><span data-stu-id="bc057-241">{100, 200} for `input_alias2 = A,`</span></span>  
  
    <span data-ttu-id="bc057-242">{300} pro`input_alias2 = C,`</span><span class="sxs-lookup"><span data-stu-id="bc057-242">{300} for `input_alias2 = C,`</span></span>  
  
- <span data-ttu-id="bc057-243">V klauzuli FROM `<from_source1> JOIN <from_source2> JOIN <from_source3>` bude mít za následek následující řazené kolekce členů:</span><span class="sxs-lookup"><span data-stu-id="bc057-243">The FROM clause `<from_source1> JOIN <from_source2> JOIN <from_source3>` will result in the following tuples:</span></span>  
  
    <span data-ttu-id="bc057-244">(`input_alias1, input_alias2, input_alias3`):</span><span class="sxs-lookup"><span data-stu-id="bc057-244">(`input_alias1, input_alias2, input_alias3`):</span></span>  
  
    <span data-ttu-id="bc057-245">(A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200), (C, 4, 300) (C, 5, 300)</span><span class="sxs-lookup"><span data-stu-id="bc057-245">(A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200),  (C, 4, 300) ,  (C, 5, 300)</span></span>  
  
> [!NOTE]
> <span data-ttu-id="bc057-246">To bylo způsobeno v smíšený produkt mezi `<from_source2>` a `<from_source3>` vzhledem k tomu, jak jsou omezená na stejné `<from_source1>`.</span><span class="sxs-lookup"><span data-stu-id="bc057-246">This resulted in cross product between `<from_source2>` and `<from_source3>` because both are scoped to the same `<from_source1>`.</span></span>  <span data-ttu-id="bc057-247">To bylo způsobeno 4 (2 x 2) řazených kolekcí členů má hodnotu, 0 řazené kolekce členů s hodnota B (1 × 0) a 2 (2 × 1) řazených kolekcí členů mají hodnotu C.</span><span class="sxs-lookup"><span data-stu-id="bc057-247">This resulted in 4 (2x2) tuples having value A, 0 tuples having value B (1x0) and 2 (2x1) tuples having value C.</span></span>  
  
<span data-ttu-id="bc057-248">**Viz také**</span><span class="sxs-lookup"><span data-stu-id="bc057-248">**See also**</span></span>  
  
 [<span data-ttu-id="bc057-249">Klauzule SELECT</span><span class="sxs-lookup"><span data-stu-id="bc057-249">SELECT clause</span></span>](#bk_select_query)  
  
##  <span data-ttu-id="bc057-250"><a name="bk_where_clause"></a>Klauzule WHERE</span><span class="sxs-lookup"><span data-stu-id="bc057-250"><a name="bk_where_clause"></a> WHERE clause</span></span>  
 <span data-ttu-id="bc057-251">Určuje podmínku vyhledávání pro dokumenty vrácené dotazem.</span><span class="sxs-lookup"><span data-stu-id="bc057-251">Specifies the search condition for the documents returned by the query.</span></span>  
  
 <span data-ttu-id="bc057-252">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-252">**Syntax**</span></span>  
  
```  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
 <span data-ttu-id="bc057-253">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-253">**Arguments**</span></span>  
  
-   `<filter_condition>`  
  
     <span data-ttu-id="bc057-254">Určuje podmínku, která má být splněny pro dokumenty, které má být vrácen.</span><span class="sxs-lookup"><span data-stu-id="bc057-254">Specifies the condition to be met for the documents to be returned.</span></span>  
  
-   `<scalar_expression>`  
  
     <span data-ttu-id="bc057-255">Výraz představující hodnotu počítaný.</span><span class="sxs-lookup"><span data-stu-id="bc057-255">Expression representing the value to be computed.</span></span> <span data-ttu-id="bc057-256">Najdete v článku [skalární výrazy](#bk_scalar_expressions) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bc057-256">See the [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
 <span data-ttu-id="bc057-257">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="bc057-257">**Remarks**</span></span>  
  
 <span data-ttu-id="bc057-258">Aby dokumentu, který má být vrácen výraz zadaný jako filtr musí podmínka vyhodnocena jako true.</span><span class="sxs-lookup"><span data-stu-id="bc057-258">In order for the document to be returned an expression specified as filter condition must evaluate to true.</span></span> <span data-ttu-id="bc057-259">Pouze logickou hodnotu true budou splňovat podmínky, jakoukoli jinou hodnotu: nedefinované, null, hodnotu false, číslo, pole nebo objekt nebude splňují zadanou podmínku.</span><span class="sxs-lookup"><span data-stu-id="bc057-259">Only Boolean value true will satisfy the condition, any other value: undefined, null, false, Number, Array or Object will not satisfy the condition.</span></span>  
  
##  <span data-ttu-id="bc057-260"><a name="bk_orderby_clause"></a>Klauzuli ORDER by</span><span class="sxs-lookup"><span data-stu-id="bc057-260"><a name="bk_orderby_clause"></a> ORDER BY clause</span></span>  
 <span data-ttu-id="bc057-261">Určuje pořadí řazení výsledků vrácených dotazem.</span><span class="sxs-lookup"><span data-stu-id="bc057-261">Specifies the sorting order for results returned by the query.</span></span>  
  
 <span data-ttu-id="bc057-262">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-262">**Syntax**</span></span>  
  
```  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= <scalar_expression> [ASC | DESC]  
  
```  
  
 <span data-ttu-id="bc057-263">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-263">**Arguments**</span></span>  
  
-   `<sort_specification>`  
  
     <span data-ttu-id="bc057-264">Určuje vlastnosti nebo výraz, podle kterého řazení sady výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="bc057-264">Specifies a property or expression on which to sort the query result set.</span></span> <span data-ttu-id="bc057-265">Sloupec pro řazení lze zadat jako název nebo sloupci alias.</span><span class="sxs-lookup"><span data-stu-id="bc057-265">A sort column can be specified as a name or column alias.</span></span>  
  
     <span data-ttu-id="bc057-266">Je možné zadat více řazení sloupců.</span><span class="sxs-lookup"><span data-stu-id="bc057-266">Multiple sort columns can be specified.</span></span> <span data-ttu-id="bc057-267">Názvy sloupců musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="bc057-267">Column names must be unique.</span></span> <span data-ttu-id="bc057-268">Definuje pořadí řazení sloupců v klauzuli ORDER by organizace ze seřazené sady.</span><span class="sxs-lookup"><span data-stu-id="bc057-268">The sequence of the sort columns in the ORDER BY clause defines the organization of the sorted result set.</span></span> <span data-ttu-id="bc057-269">To znamená sadu výsledků dotazu je seřazen podle první vlastnost a potom tento seřazený seznam je seřazen podle druhý vlastností a tak dále.</span><span class="sxs-lookup"><span data-stu-id="bc057-269">That is, the result set is sorted by the first property and then that ordered list is sorted by the second property, and so on.</span></span>  
  
     <span data-ttu-id="bc057-270">Názvy sloupců, na který odkazuje klauzule ORDER BY musí odpovídat buď na sloupec v seznamu select nebo na sloupec definovaný v tabulce zadané v klauzuli FROM bez jakékoli nejednoznačnosti.</span><span class="sxs-lookup"><span data-stu-id="bc057-270">The column names referenced in the ORDER BY clause must correspond to either a column in the select list or to a column defined in a table specified in the FROM clause without any ambiguities.</span></span>  
  
-   `<sort_expression>`  
  
     <span data-ttu-id="bc057-271">Určuje vlastnosti jediné nebo výraz, podle kterého řazení sady výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="bc057-271">Specifies a single property or expression on which to sort the query result set.</span></span>  
  
-   `<scalar_expression>`  
  
     <span data-ttu-id="bc057-272">Najdete v článku [skalární výrazy](#bk_scalar_expressions) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bc057-272">See the [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
-   `ASC | DESC`  
  
     <span data-ttu-id="bc057-273">Určuje, že hodnoty ve vybraném sloupci by měly být seřazeny ve vzestupném nebo sestupném pořadí.</span><span class="sxs-lookup"><span data-stu-id="bc057-273">Specifies that the values in the specified column should be sorted in ascending or descending order.</span></span> <span data-ttu-id="bc057-274">ASC seřadí od nejnižší hodnoty po nejvyšší hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-274">ASC sorts from the lowest value to highest value.</span></span> <span data-ttu-id="bc057-275">DESC seřadí od nejvyšší hodnoty nejnižší hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-275">DESC sorts from highest value to lowest value.</span></span> <span data-ttu-id="bc057-276">ASC je výchozí pořadí řazení.</span><span class="sxs-lookup"><span data-stu-id="bc057-276">ASC is the default sort order.</span></span> <span data-ttu-id="bc057-277">Hodnoty Null jsou považovány za nejnižší možné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bc057-277">Null values are treated as the lowest possible values.</span></span>  
  
 <span data-ttu-id="bc057-278">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="bc057-278">**Remarks**</span></span>  
  
 <span data-ttu-id="bc057-279">I když gramatiky dotazů podporuje více pořadí podle vlastností, modulu runtime Azure Cosmos DB dotazu podporuje řazení pouze s jedinou vlastností a pouze s názvy vlastností, tj., není pro počítané vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bc057-279">While the query grammar supports multiple order by properties, the Azure Cosmos DB query runtime supports sorting only against a single property, and only against property names, i.e., not against computed properties.</span></span> <span data-ttu-id="bc057-280">Řazení také vyžaduje, zásady indexování zahrnuje indexem rozsahu pro vlastnost a zadaného typu, s maximální přesnost.</span><span class="sxs-lookup"><span data-stu-id="bc057-280">Sorting also requires that the indexing policy includes a range index for the property and the specified type, with the maximum precision.</span></span> <span data-ttu-id="bc057-281">Naleznete v dokumentaci indexování zásady další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bc057-281">Refer to the indexing policy documentation for more details.</span></span>  
  
##  <span data-ttu-id="bc057-282"><a name="bk_scalar_expressions"></a>Skalární výrazy</span><span class="sxs-lookup"><span data-stu-id="bc057-282"><a name="bk_scalar_expressions"></a> Scalar expressions</span></span>  
 <span data-ttu-id="bc057-283">Skalární výraz, který je kombinací symbolů a operátory, které lze vyhodnotit na získat jednu hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-283">A scalar expression is a combination of symbols and operators that can be evaluated to obtain a single value.</span></span> <span data-ttu-id="bc057-284">Jednoduché výrazy může být konstanty, odkazy na vlastnost, odkazuje na element pole, odkazy na alias nebo volání funkce.</span><span class="sxs-lookup"><span data-stu-id="bc057-284">Simple expressions can be constants, property references, array element references, alias references, or function calls.</span></span> <span data-ttu-id="bc057-285">Jednoduché výrazy lze spojovat do složité výrazy pomocí operátorů.</span><span class="sxs-lookup"><span data-stu-id="bc057-285">Simple expressions can be combined into complex expressions using operators.</span></span>  
  
 <span data-ttu-id="bc057-286">Podrobnosti na hodnotách, které skalární výraz, který může mít najdete v tématu [konstanty](#bk_constants) části.</span><span class="sxs-lookup"><span data-stu-id="bc057-286">For details on values which scalar expression may have, see [Constants](#bk_constants) section.</span></span>  
  
 <span data-ttu-id="bc057-287">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-287">**Syntax**</span></span>  
  
```  
<scalar_expression> ::=  
       <constant>   
     | input_alias   
     | parameter_name  
     | <scalar_expression>.property_name  
     | <scalar_expression>'['"property_name"|array_index']'  
     | unary_operator <scalar_expression>  
     | <scalar_expression> binary_operator <scalar_expression>    
     | <scalar_expression> ? <scalar_expression> : <scalar_expression>  
     | <scalar_function_expression>  
     | <create_object_expression>   
     | <create_array_expression>  
     | (<scalar_expression>)   
  
<scalar_function_expression> ::=  
        'udf.' Udf_scalar_function([<scalar_expression>][,…n])  
        | builtin_scalar_function([<scalar_expression>][,…n])  
  
<create_object_expression> ::=  
   '{' [{property_name | "property_name"} : <scalar_expression>][,…n] '}'  
  
<create_array_expression> ::=  
   '[' [<scalar_expression>][,…n] ']'  
  
```  
  
 <span data-ttu-id="bc057-288">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-288">**Arguments**</span></span>  
  
-   `<constant>`  
  
     <span data-ttu-id="bc057-289">Představuje konstantní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-289">Represents a constant value.</span></span> <span data-ttu-id="bc057-290">V tématu [konstanty](#bk_constants) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bc057-290">See [Constants](#bk_constants) section for details.</span></span>  
  
-   `input_alias`  
  
     <span data-ttu-id="bc057-291">Reprezentuje hodnotu definované `input_alias` počínaje `FROM` klauzule.</span><span class="sxs-lookup"><span data-stu-id="bc057-291">Represents a value defined by the `input_alias` introduced in the `FROM` clause.</span></span>  
    <span data-ttu-id="bc057-292">Tato hodnota představuje záruku není **nedefinované** –**nedefinované** hodnoty ve vstupu se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="bc057-292">This value is guaranteed to not be **undefined** –**undefined** values in the input are skipped.</span></span>  
  
-   `<scalar_expression>.property_name`  
  
     <span data-ttu-id="bc057-293">Reprezentuje hodnotu vlastnosti objektu.</span><span class="sxs-lookup"><span data-stu-id="bc057-293">Represents a value of the property of an object.</span></span> <span data-ttu-id="bc057-294">Pokud vlastnost neexistuje nebo je odkazováno vlastnost na hodnotu, která není objekt a potom vyhodnocen jako **nedefinované** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-294">If the property does not exist or property is referenced on a value which is not an object, then the expression evaluates to **undefined** value.</span></span>  
  
-   `<scalar_expression>'['"property_name"|array_index']'`  
  
     <span data-ttu-id="bc057-295">Reprezentuje hodnotu vlastnosti s názvem `property_name` nebo element pole s indexem `array_index` objekt nebo pole.</span><span class="sxs-lookup"><span data-stu-id="bc057-295">Represents a value of the property with name `property_name` or array element with index `array_index` of an object/array.</span></span> <span data-ttu-id="bc057-296">Pokud neexistuje index vlastnost nebo pole nebo vlastnost nebo pole indexu je odkazováno na hodnotu, která není objekt nebo pole, pak je vyhodnocen nedefinovanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-296">If the property/array index does not exist or the property/array index is referenced on a value which is not an object/array, then the expression evaluates to undefined value.</span></span>  
  
-   `unary_operator <scalar_expression>`  
  
     <span data-ttu-id="bc057-297">Představuje operátor, který se použije pro jednu hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-297">Represents an operator that is applied to a single value.</span></span> <span data-ttu-id="bc057-298">V tématu [operátory](#bk_operators) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bc057-298">See [Operators](#bk_operators) section for details.</span></span>  
  
-   `<scalar_expression> binary_operator <scalar_expression>`  
  
     <span data-ttu-id="bc057-299">Představuje operátor, který se použije pro dvě hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bc057-299">Represents an operator that is applied to two values.</span></span> <span data-ttu-id="bc057-300">V tématu [operátory](#bk_operators) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bc057-300">See [Operators](#bk_operators) section for details.</span></span>  
  
-   `<scalar_function_expression>`  
  
     <span data-ttu-id="bc057-301">Reprezentuje hodnotu definované výsledku volání funkce.</span><span class="sxs-lookup"><span data-stu-id="bc057-301">Represents a value defined by a result of a function call.</span></span>  
  
-   `udf_scalar_function`  
  
     <span data-ttu-id="bc057-302">Jméno uživatele definované skalární funkce.</span><span class="sxs-lookup"><span data-stu-id="bc057-302">Name of the user defined scalar function.</span></span>  
  
-   `builtin_scalar_function`  
  
     <span data-ttu-id="bc057-303">Název předdefinované skalární funkce.</span><span class="sxs-lookup"><span data-stu-id="bc057-303">Name of the built-in scalar function.</span></span>  
  
-   `<create_object_expression>`  
  
     <span data-ttu-id="bc057-304">Představuje hodnotu získat tak, že vytvoříte nový objekt v zadaných vlastností a jejich hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bc057-304">Represents a value obtained by creating a new object with specified properties and their values.</span></span>  
  
-   `<create_array_expression>`  
  
     <span data-ttu-id="bc057-305">Reprezentuje hodnotu získala při vytváření nové pole se zadanými hodnotami, jako elementy</span><span class="sxs-lookup"><span data-stu-id="bc057-305">Represents a value obtained by creating a new array with specified values as elements</span></span>  
  
-   `parameter_name`  
  
     <span data-ttu-id="bc057-306">Reprezentuje hodnotu zadaný název parametru.</span><span class="sxs-lookup"><span data-stu-id="bc057-306">Represents a value of the specified parameter name.</span></span> <span data-ttu-id="bc057-307">Názvy parametrů musí mít jeden @ jako první znak.</span><span class="sxs-lookup"><span data-stu-id="bc057-307">Parameter names must have a single @ as the first character.</span></span>  
  
 <span data-ttu-id="bc057-308">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="bc057-308">**Remarks**</span></span>  
  
 <span data-ttu-id="bc057-309">Při volání předdefinované nebo uživatel definované skalární funkce musí být definovány všechny argumenty.</span><span class="sxs-lookup"><span data-stu-id="bc057-309">When calling a built-in or user defined scalar function all arguments must be defined.</span></span> <span data-ttu-id="bc057-310">Pokud některý z argumentů není definován, nebude volána funkce a výsledkem bude definován.</span><span class="sxs-lookup"><span data-stu-id="bc057-310">If any of the arguments is undefined, the function will not be called and the result will be undefined.</span></span>  
  
 <span data-ttu-id="bc057-311">Při vytvoření objektu, bude přeskočen a nejsou zahrnuty v vytvořený objekt jakákoli vlastnost, která je přiřazena nedefinovanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-311">When creating an object, any property that is assigned undefined value will be skipped and not included in the created object.</span></span>  
  
 <span data-ttu-id="bc057-312">Při vytváření pole, libovolná hodnota elementu, který je přiřazen **nedefinované** hodnota bude přeskočen a nejsou zahrnuty v vytvořený objekt.</span><span class="sxs-lookup"><span data-stu-id="bc057-312">When creating an array, any element value that is assigned **undefined** value will be skipped and not included in the created object.</span></span> <span data-ttu-id="bc057-313">To způsobí, že další definované elementu, který chcete provést jeho tak, že vytvořený pole nebude přeskočena indexy.</span><span class="sxs-lookup"><span data-stu-id="bc057-313">This will cause the next defined element to take its place in such a way that the created array will not have skipped indexes.</span></span>  
  
##  <span data-ttu-id="bc057-314"><a name="bk_operators"></a>Operátory</span><span class="sxs-lookup"><span data-stu-id="bc057-314"><a name="bk_operators"></a> Operators</span></span>  
 <span data-ttu-id="bc057-315">Tato část popisuje podporované operátory.</span><span class="sxs-lookup"><span data-stu-id="bc057-315">This section describes the supported operators.</span></span> <span data-ttu-id="bc057-316">Každý operátor lze přiřadit k přesně jednu kategorii.</span><span class="sxs-lookup"><span data-stu-id="bc057-316">Each operator can be assigned to exactly one category.</span></span>  
  
 <span data-ttu-id="bc057-317">V tématu **operátor kategorie** tabulce podrobnosti ohledně zpracování **nedefinované** hodnoty, požadavky na typ vstupní hodnoty a zpracování hodnot s není odpovídající typy.</span><span class="sxs-lookup"><span data-stu-id="bc057-317">See **Operator categories** table below, for details regarding handling of **undefined** values, type requirements for input values and handling of values with not matching types.</span></span>  
  
 <span data-ttu-id="bc057-318">**Operátor kategorie:**</span><span class="sxs-lookup"><span data-stu-id="bc057-318">**Operator categories:**</span></span>  
  
|<span data-ttu-id="bc057-319">**Kategorie**</span><span class="sxs-lookup"><span data-stu-id="bc057-319">**Category**</span></span>|<span data-ttu-id="bc057-320">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="bc057-320">**Details**</span></span>|  
|-|-|  
|<span data-ttu-id="bc057-321">**aritmetické operace**</span><span class="sxs-lookup"><span data-stu-id="bc057-321">**arithmetic**</span></span>|<span data-ttu-id="bc057-322">Operátor očekává input(s) být čísel.</span><span class="sxs-lookup"><span data-stu-id="bc057-322">Operator expects input(s) to be Number(s).</span></span> <span data-ttu-id="bc057-323">Výstup je také číslo.</span><span class="sxs-lookup"><span data-stu-id="bc057-323">Output is also a Number.</span></span> <span data-ttu-id="bc057-324">Pokud je některý ze vstupních údajů **nedefinované** nebo typ než číslo potom výsledek **nedefinované**.</span><span class="sxs-lookup"><span data-stu-id="bc057-324">If any of the inputs is **undefined** or type other than Number then the result is **undefined**.</span></span>|  
|<span data-ttu-id="bc057-325">**bitové operace**</span><span class="sxs-lookup"><span data-stu-id="bc057-325">**bitwise**</span></span>|<span data-ttu-id="bc057-326">Operátor očekává input(s) být 32bitové číslo se znaménkem čísel.</span><span class="sxs-lookup"><span data-stu-id="bc057-326">Operator expects input(s) to be 32-bit signed integer Number(s).</span></span> <span data-ttu-id="bc057-327">Výstup je také 32bitové číslo se znaménkem číslo.</span><span class="sxs-lookup"><span data-stu-id="bc057-327">Output is also 32-bit signed integer Number.</span></span><br /><br /> <span data-ttu-id="bc057-328">Libovolná hodnota není celé číslo se zaokrouhlí.</span><span class="sxs-lookup"><span data-stu-id="bc057-328">Any non-integer value will be rounded.</span></span> <span data-ttu-id="bc057-329">Kladné celé číslo se zaokrouhlí směrem dolů, záporné hodnoty zaokrouhlený nahoru.</span><span class="sxs-lookup"><span data-stu-id="bc057-329">Positive value will be rounded down, negative values rounded up.</span></span><br /><br /> <span data-ttu-id="bc057-330">Provedením poslední 32 bity jeho dva pro zápis doplňku budou převedeny jakoukoli hodnotu, která je mimo rozsah 32bitové celé číslo.</span><span class="sxs-lookup"><span data-stu-id="bc057-330">Any value that is outside of the 32-bit integer range will be converted, by taking last 32-bits of its two's complement notation.</span></span><br /><br /> <span data-ttu-id="bc057-331">Pokud je některý ze vstupních údajů **nedefinované** nebo jiného typu než čísla, potom je **nedefinované**.</span><span class="sxs-lookup"><span data-stu-id="bc057-331">If any of the inputs is **undefined** or type other than Number, then the result is **undefined**.</span></span><br /><br /> <span data-ttu-id="bc057-332">**Poznámka:** výše uvedené chování je kompatibilní s chováním bitový operátor jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bc057-332">**Note:** The above behavior is compatible with JavaScript bitwise operator behavior.</span></span>|  
|<span data-ttu-id="bc057-333">**logické**</span><span class="sxs-lookup"><span data-stu-id="bc057-333">**logical**</span></span>|<span data-ttu-id="bc057-334">Operátor očekává input(s) být Boolean(s).</span><span class="sxs-lookup"><span data-stu-id="bc057-334">Operator expects input(s) to be Boolean(s).</span></span> <span data-ttu-id="bc057-335">Výstup je také logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-335">Output is also a Boolean.</span></span><br /><span data-ttu-id="bc057-336">Pokud je některý ze vstupních údajů **nedefinované** nebo jiného typu než logickou hodnotu, bude výsledkem **nedefinované**.</span><span class="sxs-lookup"><span data-stu-id="bc057-336">If any of the inputs is **undefined** or type other than Boolean, then the result will be **undefined**.</span></span>|  
|<span data-ttu-id="bc057-337">**porovnání**</span><span class="sxs-lookup"><span data-stu-id="bc057-337">**comparison**</span></span>|<span data-ttu-id="bc057-338">Operátor očekává input(s) stejného typu a nesmí být definován.</span><span class="sxs-lookup"><span data-stu-id="bc057-338">Operator expects input(s) to have the same type and not be undefined.</span></span> <span data-ttu-id="bc057-339">Výstup je logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="bc057-339">Output is a Boolean.</span></span><br /><br /> <span data-ttu-id="bc057-340">Pokud je některý ze vstupních údajů **nedefinované** nebo mají různé typy vstupních hodnot, potom je **nedefinované**.</span><span class="sxs-lookup"><span data-stu-id="bc057-340">If any of the inputs is **undefined** or the inputs have different types, then the result is **undefined**.</span></span><br /><br /> <span data-ttu-id="bc057-341">V tématu **řazení hodnot pro porovnání** tabulky pro hodnotu řazení podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bc057-341">See **Ordering of values for comparison** table for value ordering details.</span></span>|  
|<span data-ttu-id="bc057-342">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="bc057-342">**string**</span></span>|<span data-ttu-id="bc057-343">Operátor očekává input(s) jako řetězec nebo řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-343">Operator expects input(s) to be String(s).</span></span> <span data-ttu-id="bc057-344">Výstup je také řetězec.</span><span class="sxs-lookup"><span data-stu-id="bc057-344">Output is also a String.</span></span><br /><span data-ttu-id="bc057-345">Pokud je některý ze vstupních údajů **nedefinované** nebo jiného typu než řetězec potom je **nedefinované**.</span><span class="sxs-lookup"><span data-stu-id="bc057-345">If any of the inputs is **undefined** or type other than String then the result is **undefined**.</span></span>|  
  
 <span data-ttu-id="bc057-346">**Unární operátory:**</span><span class="sxs-lookup"><span data-stu-id="bc057-346">**Unary operators:**</span></span>  
  
|<span data-ttu-id="bc057-347">**Název**</span><span class="sxs-lookup"><span data-stu-id="bc057-347">**Name**</span></span>|<span data-ttu-id="bc057-348">**Operátor**</span><span class="sxs-lookup"><span data-stu-id="bc057-348">**Operator**</span></span>|<span data-ttu-id="bc057-349">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="bc057-349">**Details**</span></span>|  
|-|-|-|  
|<span data-ttu-id="bc057-350">**aritmetické operace**</span><span class="sxs-lookup"><span data-stu-id="bc057-350">**arithmetic**</span></span>|+<br /><br /> -|<span data-ttu-id="bc057-351">Vrátí číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-351">Returns the number value.</span></span><br /><br /> <span data-ttu-id="bc057-352">Bitovou negaci.</span><span class="sxs-lookup"><span data-stu-id="bc057-352">Bitwise negation.</span></span> <span data-ttu-id="bc057-353">Vrátí Negované číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-353">Returns negated number value.</span></span>|  
|<span data-ttu-id="bc057-354">**bitové operace**</span><span class="sxs-lookup"><span data-stu-id="bc057-354">**bitwise**</span></span>|~|<span data-ttu-id="bc057-355">Ty, které se doplňku.</span><span class="sxs-lookup"><span data-stu-id="bc057-355">Ones' complement.</span></span> <span data-ttu-id="bc057-356">Vrátí zbytek číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-356">Returns a complement of a number value.</span></span>|  
|<span data-ttu-id="bc057-357">**Logické**</span><span class="sxs-lookup"><span data-stu-id="bc057-357">**Logical**</span></span>|<span data-ttu-id="bc057-358">**NENÍ**</span><span class="sxs-lookup"><span data-stu-id="bc057-358">**NOT**</span></span>|<span data-ttu-id="bc057-359">Negace.</span><span class="sxs-lookup"><span data-stu-id="bc057-359">Negation.</span></span> <span data-ttu-id="bc057-360">Vrátí Negované logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="bc057-360">Returns negated Boolean value.</span></span>|  
  
 <span data-ttu-id="bc057-361">**Binární operátory:**</span><span class="sxs-lookup"><span data-stu-id="bc057-361">**Binary operators:**</span></span>  
  
|<span data-ttu-id="bc057-362">**Název**</span><span class="sxs-lookup"><span data-stu-id="bc057-362">**Name**</span></span>|<span data-ttu-id="bc057-363">**Operátor**</span><span class="sxs-lookup"><span data-stu-id="bc057-363">**Operator**</span></span>|<span data-ttu-id="bc057-364">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="bc057-364">**Details**</span></span>|  
|-|-|-|  
|<span data-ttu-id="bc057-365">**aritmetické operace**</span><span class="sxs-lookup"><span data-stu-id="bc057-365">**arithmetic**</span></span>|+<br /><br /> -<br /><br /> *<br /><br /> /<br /><br /> %|<span data-ttu-id="bc057-366">Přidání.</span><span class="sxs-lookup"><span data-stu-id="bc057-366">Addition.</span></span><br /><br /> <span data-ttu-id="bc057-367">Odčítání.</span><span class="sxs-lookup"><span data-stu-id="bc057-367">Subtraction.</span></span><br /><br /> <span data-ttu-id="bc057-368">Násobení.</span><span class="sxs-lookup"><span data-stu-id="bc057-368">Multiplication.</span></span><br /><br /> <span data-ttu-id="bc057-369">Dělení.</span><span class="sxs-lookup"><span data-stu-id="bc057-369">Division.</span></span><br /><br /> <span data-ttu-id="bc057-370">Modulační.</span><span class="sxs-lookup"><span data-stu-id="bc057-370">Modulation.</span></span>|  
|<span data-ttu-id="bc057-371">**bitové operace**</span><span class="sxs-lookup"><span data-stu-id="bc057-371">**bitwise**</span></span>|<span data-ttu-id="bc057-372">&#124;</span><span class="sxs-lookup"><span data-stu-id="bc057-372">&#124;</span></span><br /><br /> &<br /><br /> ^<br /><br /> <<<br /><br /> >><br /><br /> >>>|<span data-ttu-id="bc057-373">Bitový operátor OR.</span><span class="sxs-lookup"><span data-stu-id="bc057-373">Bitwise OR.</span></span><br /><br /> <span data-ttu-id="bc057-374">Bitové operace AND.</span><span class="sxs-lookup"><span data-stu-id="bc057-374">Bitwise AND.</span></span><br /><br /> <span data-ttu-id="bc057-375">Bitové operace XOR.</span><span class="sxs-lookup"><span data-stu-id="bc057-375">Bitwise XOR.</span></span><br /><br /> <span data-ttu-id="bc057-376">Operátor posunu vlevo.</span><span class="sxs-lookup"><span data-stu-id="bc057-376">Left Shift.</span></span><br /><br /> <span data-ttu-id="bc057-377">Posunutí doprava.</span><span class="sxs-lookup"><span data-stu-id="bc057-377">Right Shift.</span></span><br /><br /> <span data-ttu-id="bc057-378">Posunutí doprava výplně nula.</span><span class="sxs-lookup"><span data-stu-id="bc057-378">Zero-fill Right Shift.</span></span>|  
|<span data-ttu-id="bc057-379">**logické**</span><span class="sxs-lookup"><span data-stu-id="bc057-379">**logical**</span></span>|<span data-ttu-id="bc057-380">**A**</span><span class="sxs-lookup"><span data-stu-id="bc057-380">**AND**</span></span><br /><br /> <span data-ttu-id="bc057-381">**OR**</span><span class="sxs-lookup"><span data-stu-id="bc057-381">**OR**</span></span>|<span data-ttu-id="bc057-382">Logické spojení.</span><span class="sxs-lookup"><span data-stu-id="bc057-382">Logical conjunction.</span></span> <span data-ttu-id="bc057-383">Vrátí **true** Pokud jsou oba argumenty **true**, vrátí **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="bc057-383">Returns **true** if both arguments are **true**, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="bc057-384">Logické spojení.</span><span class="sxs-lookup"><span data-stu-id="bc057-384">Logical conjunction.</span></span> <span data-ttu-id="bc057-385">Vrátí **true** Pokud jsou oba argumenty **true**, vrátí **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="bc057-385">Returns **true** if both arguments are **true**, returns **false** otherwise.</span></span>|  
|<span data-ttu-id="bc057-386">**porovnání**</span><span class="sxs-lookup"><span data-stu-id="bc057-386">**comparison**</span></span>|**=**<br /><br /> <span data-ttu-id="bc057-387">**!=, <>**</span><span class="sxs-lookup"><span data-stu-id="bc057-387">**!=, <>**</span></span><br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> <span data-ttu-id="bc057-388">**??**</span><span class="sxs-lookup"><span data-stu-id="bc057-388">**??**</span></span>|<span data-ttu-id="bc057-389">Rovná se.</span><span class="sxs-lookup"><span data-stu-id="bc057-389">Equals.</span></span> <span data-ttu-id="bc057-390">Vrátí **true** Pokud argumenty jsou stejné, vrátí **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="bc057-390">Returns **true** if arguments are equal, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="bc057-391">Není rovno.</span><span class="sxs-lookup"><span data-stu-id="bc057-391">Not equal to.</span></span> <span data-ttu-id="bc057-392">Vrátí **true** Pokud argumenty nejsou stejné, vrátí **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="bc057-392">Returns **true** if arguments are not equal, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="bc057-393">Větší než.</span><span class="sxs-lookup"><span data-stu-id="bc057-393">Greater Than.</span></span> <span data-ttu-id="bc057-394">Vrátí **true** Pokud je větší než druhý argument, vrátí **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="bc057-394">Returns **true** if first argument is greater than the second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="bc057-395">Větší než nebo rovna hodnotě.</span><span class="sxs-lookup"><span data-stu-id="bc057-395">Greater Than or Equal To.</span></span> <span data-ttu-id="bc057-396">Vrátí **true** Pokud první argument je větší než nebo rovno druhý, vrátí **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="bc057-396">Returns **true** if first argument is greater than or equal to the second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="bc057-397">Menší než.</span><span class="sxs-lookup"><span data-stu-id="bc057-397">Less Than.</span></span> <span data-ttu-id="bc057-398">Vrátí **true** Pokud první argument je menší než druhý jeden návratový **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="bc057-398">Returns **true** if first argument is less than the second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="bc057-399">Menší než nebo rovno.</span><span class="sxs-lookup"><span data-stu-id="bc057-399">Less Than or Equal To.</span></span> <span data-ttu-id="bc057-400">Vrátí **true** Pokud první argument je menší než nebo rovna druhý, vrátí **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="bc057-400">Returns **true** if first argument is less than or equal to the second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="bc057-401">Sloučení.</span><span class="sxs-lookup"><span data-stu-id="bc057-401">Coalesce.</span></span> <span data-ttu-id="bc057-402">Vrátí druhý argument, pokud je první argument **nedefinované** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-402">Returns the second argument if the first argument is an **undefined** value.</span></span>|  
|<span data-ttu-id="bc057-403">**Řetězec**</span><span class="sxs-lookup"><span data-stu-id="bc057-403">**String**</span></span>|<span data-ttu-id="bc057-404">**&#124;&#124;**</span><span class="sxs-lookup"><span data-stu-id="bc057-404">**&#124;&#124;**</span></span>|<span data-ttu-id="bc057-405">Zřetězení.</span><span class="sxs-lookup"><span data-stu-id="bc057-405">Concatenation.</span></span> <span data-ttu-id="bc057-406">Vrátí zřetězení oba argumenty.</span><span class="sxs-lookup"><span data-stu-id="bc057-406">Returns a concatenation of both arguments.</span></span>|  
  
 <span data-ttu-id="bc057-407">**Ternární operátory:**</span><span class="sxs-lookup"><span data-stu-id="bc057-407">**Ternary operators:**</span></span>  
  
|<span data-ttu-id="bc057-408">Ternární operátor</span><span class="sxs-lookup"><span data-stu-id="bc057-408">Ternary operator</span></span>|<span data-ttu-id="bc057-409">?</span><span class="sxs-lookup"><span data-stu-id="bc057-409">?</span></span>|<span data-ttu-id="bc057-410">Vrátí druhý argument, pokud se vyhodnotí jako první argument **true**; jinak vrátí třetí argument.</span><span class="sxs-lookup"><span data-stu-id="bc057-410">Returns the second argument if the first argument evaluates to **true**; return the third argument otherwise.</span></span>|  
|-|-|-|  
  
 <span data-ttu-id="bc057-411">**Řazení hodnot pro porovnání**</span><span class="sxs-lookup"><span data-stu-id="bc057-411">**Ordering of values for comparison**</span></span>  
  
|<span data-ttu-id="bc057-412">**Typ**</span><span class="sxs-lookup"><span data-stu-id="bc057-412">**Type**</span></span>|<span data-ttu-id="bc057-413">**Pořadí hodnoty**</span><span class="sxs-lookup"><span data-stu-id="bc057-413">**Values order**</span></span>|  
|-|-|  
|<span data-ttu-id="bc057-414">**Nedefinovaná**</span><span class="sxs-lookup"><span data-stu-id="bc057-414">**Undefined**</span></span>|<span data-ttu-id="bc057-415">Není porovnatelný.</span><span class="sxs-lookup"><span data-stu-id="bc057-415">Not comparable.</span></span>|  
|<span data-ttu-id="bc057-416">**Hodnotu Null**</span><span class="sxs-lookup"><span data-stu-id="bc057-416">**Null**</span></span>|<span data-ttu-id="bc057-417">Jedna hodnota: **hodnotu null.**</span><span class="sxs-lookup"><span data-stu-id="bc057-417">Single value: **null**</span></span>|  
|<span data-ttu-id="bc057-418">**Číslo**</span><span class="sxs-lookup"><span data-stu-id="bc057-418">**Number**</span></span>|<span data-ttu-id="bc057-419">Fyzická reálné číslo.</span><span class="sxs-lookup"><span data-stu-id="bc057-419">Natural real number.</span></span><br /><br /> <span data-ttu-id="bc057-420">Zápornou hodnotu Infinity je menší než ostatní číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-420">Negative Infinity value is smaller than any other Number value.</span></span><br /><br /> <span data-ttu-id="bc057-421">Je větší než jakoukoli jinou hodnotu, číslo kladné nekonečnou hodnotu. **NaN** hodnota není porovnatelný.</span><span class="sxs-lookup"><span data-stu-id="bc057-421">Positive Infinity value is larger than any other Number value.**NaN** value is not comparable.</span></span> <span data-ttu-id="bc057-422">Porovnávání s **NaN** bude mít za následek **nedefinované** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-422">Comparing with **NaN** will result in **undefined** value.</span></span>|  
|<span data-ttu-id="bc057-423">**Řetězec**</span><span class="sxs-lookup"><span data-stu-id="bc057-423">**String**</span></span>|<span data-ttu-id="bc057-424">Lexicographical pořadí.</span><span class="sxs-lookup"><span data-stu-id="bc057-424">Lexicographical order.</span></span>|  
|<span data-ttu-id="bc057-425">**Pole**</span><span class="sxs-lookup"><span data-stu-id="bc057-425">**Array**</span></span>|<span data-ttu-id="bc057-426">Žádné řazení, ale přiměřenou.</span><span class="sxs-lookup"><span data-stu-id="bc057-426">No ordering, but equitable.</span></span>|  
|<span data-ttu-id="bc057-427">**Objekt**</span><span class="sxs-lookup"><span data-stu-id="bc057-427">**Object**</span></span>|<span data-ttu-id="bc057-428">Žádné řazení, ale přiměřenou.</span><span class="sxs-lookup"><span data-stu-id="bc057-428">No ordering, but equitable.</span></span>|  
  
 <span data-ttu-id="bc057-429">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="bc057-429">**Remarks**</span></span>  
  
 <span data-ttu-id="bc057-430">V Azure DB Cosmos typy hodnot, často není známý, dokud se ve skutečnosti načítají z databáze.</span><span class="sxs-lookup"><span data-stu-id="bc057-430">In Azure Cosmos DB, the types of values are often not known until they are actually retrieved from the database.</span></span> <span data-ttu-id="bc057-431">Za účelem podpory efektivní provádění dotazů, většina operátory má požadavky na typ strict.</span><span class="sxs-lookup"><span data-stu-id="bc057-431">In order to support efficient execution of queries, most of the operators have strict type requirements.</span></span> <span data-ttu-id="bc057-432">Operátory samy o sobě taky neprovádět implicitní převody.</span><span class="sxs-lookup"><span data-stu-id="bc057-432">Also operators by themselves do not perform implicit conversions.</span></span>  
  
 <span data-ttu-id="bc057-433">To znamená, že jako dotaz: vybrat * z ROOT r kde r.Age = 21 vrátí jenom dokumenty s vlastností stáří rovná počtu 21.</span><span class="sxs-lookup"><span data-stu-id="bc057-433">This means that a query like: SELECT * FROM ROOT r WHERE r.Age = 21 will only return documents with property Age equal to the number 21.</span></span> <span data-ttu-id="bc057-434">Dokumenty s vlastností stáří rovna řetězec "21" nebo "0021" řetězec nebude odpovídat jako výraz "21" = 21 vyhodnotí na nedefinovaný.</span><span class="sxs-lookup"><span data-stu-id="bc057-434">Documents with property Age equal to the string "21" or the string "0021" will not match, as the expression "21" = 21 evaluates to undefined.</span></span> <span data-ttu-id="bc057-435">To umožňuje lepší využití indexy, protože je vyhledávání určitou hodnotu (tj. počet 21) je rychlejší než vyhledávání nekonečný počet potenciální odpovídá (tj. počet 21 nebo řetězce "21", "021", "21.0"...).</span><span class="sxs-lookup"><span data-stu-id="bc057-435">This allows for a better use of indexes, because the lookup of a specific value (i.e. number 21) is faster than search for indefinite number of potential matches (i.e. number 21 or strings "21", "021", "21.0" …).</span></span> <span data-ttu-id="bc057-436">To se liší od způsobu JavaScript vyhodnotí operátory na hodnoty různých typů.</span><span class="sxs-lookup"><span data-stu-id="bc057-436">This is different from how JavaScript evaluates operators on values of different types.</span></span>  
  
 <span data-ttu-id="bc057-437">**Porovnání rovnosti pole a objekty a**</span><span class="sxs-lookup"><span data-stu-id="bc057-437">**Arrays and objects equality and comparison**</span></span>  
  
 <span data-ttu-id="bc057-438">Porovnání hodnot pole nebo objekt pomocí operátorů rozsah (>, > =, <, < =) bude mít za následek není definována jako není pořadí na objekt nebo pole hodnot.</span><span class="sxs-lookup"><span data-stu-id="bc057-438">Comparing of Array or Object values using range operators (>, >=, <, <=) will result in undefined as there is not order defined on Object or Array values.</span></span> <span data-ttu-id="bc057-439">Ale pomocí operátory rovnosti/nerovnosti (=,! =, <>) je podporována a hodnoty budou porovnány strukturálně.</span><span class="sxs-lookup"><span data-stu-id="bc057-439">However using equality/inequality operators (=, !=, <>) is supported and values will be compared structurally.</span></span>  
  
 <span data-ttu-id="bc057-440">Pole jsou stejné, pokud obě pole mají stejný počet elementů a prvky v odpovídajícím pozic jsou také stejné.</span><span class="sxs-lookup"><span data-stu-id="bc057-440">Arrays are equal if both arrays have same number of elements and elements at matching positions are also equal.</span></span> <span data-ttu-id="bc057-441">Pokud není definovaná porovnání výsledků elementy v žádném páru, výsledku porovnání pole není definováno.</span><span class="sxs-lookup"><span data-stu-id="bc057-441">If comparing any pair of elements results in undefined, the result of array comparison is undefined.</span></span>  
  
 <span data-ttu-id="bc057-442">Objekty jsou stejné, pokud mají oba objekty stejné vlastnosti definované a jsou také stejné hodnoty odpovídající vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bc057-442">Objects are equal if both objects have same properties defined, and if values of matching properties are also equal.</span></span> <span data-ttu-id="bc057-443">Pokud není definovaná porovnání výsledků hodnoty vlastností v žádném páru, výsledku porovnání objekt není definován.</span><span class="sxs-lookup"><span data-stu-id="bc057-443">If comparing any pair of property values results in undefined, the result of object comparison is undefined.</span></span>  
  
##  <span data-ttu-id="bc057-444"><a name="bk_constants"></a>Konstanty</span><span class="sxs-lookup"><span data-stu-id="bc057-444"><a name="bk_constants"></a> Constants</span></span>  
 <span data-ttu-id="bc057-445">Konstanta, také známé jako literál nebo skalární hodnota, je symbol, který představuje konkrétní datové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bc057-445">A constant, also known as a literal or a scalar value, is a symbol that represents a specific data value.</span></span> <span data-ttu-id="bc057-446">Formát konstanta závisí na datový typ hodnoty, který představuje.</span><span class="sxs-lookup"><span data-stu-id="bc057-446">The format of a constant depends on the data type of the value it represents.</span></span>  
  
 <span data-ttu-id="bc057-447">**Podporované typy skalární data:**</span><span class="sxs-lookup"><span data-stu-id="bc057-447">**Supported scalar data types:**</span></span>  
  
|<span data-ttu-id="bc057-448">**Typ**</span><span class="sxs-lookup"><span data-stu-id="bc057-448">**Type**</span></span>|<span data-ttu-id="bc057-449">**Pořadí hodnoty**</span><span class="sxs-lookup"><span data-stu-id="bc057-449">**Values order**</span></span>|  
|-|-|  
|<span data-ttu-id="bc057-450">**Nedefinovaná**</span><span class="sxs-lookup"><span data-stu-id="bc057-450">**Undefined**</span></span>|<span data-ttu-id="bc057-451">Jedna hodnota: **Nedefinovaná**</span><span class="sxs-lookup"><span data-stu-id="bc057-451">Single value: **undefined**</span></span>|  
|<span data-ttu-id="bc057-452">**Hodnotu Null**</span><span class="sxs-lookup"><span data-stu-id="bc057-452">**Null**</span></span>|<span data-ttu-id="bc057-453">Jedna hodnota: **hodnotu null.**</span><span class="sxs-lookup"><span data-stu-id="bc057-453">Single value: **null**</span></span>|  
|<span data-ttu-id="bc057-454">**Logická hodnota**</span><span class="sxs-lookup"><span data-stu-id="bc057-454">**Boolean**</span></span>|<span data-ttu-id="bc057-455">Hodnoty: **false**, **true**.</span><span class="sxs-lookup"><span data-stu-id="bc057-455">Values: **false**, **true**.</span></span>|  
|<span data-ttu-id="bc057-456">**Číslo**</span><span class="sxs-lookup"><span data-stu-id="bc057-456">**Number**</span></span>|<span data-ttu-id="bc057-457">Dvojitá přesnost s plovoucí desetinnou čárkou číslo, IEEE 754 standard.</span><span class="sxs-lookup"><span data-stu-id="bc057-457">A double-precision floating-point number, IEEE 754 standard.</span></span>|  
|<span data-ttu-id="bc057-458">**Řetězec**</span><span class="sxs-lookup"><span data-stu-id="bc057-458">**String**</span></span>|<span data-ttu-id="bc057-459">Pořadí nula nebo více znaků Unicode.</span><span class="sxs-lookup"><span data-stu-id="bc057-459">A sequence of zero or more Unicode characters.</span></span> <span data-ttu-id="bc057-460">Řetězce musí být uzavřena do jednoduchých nebo dvojitých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="bc057-460">Strings must be enclosed in single or double quotes.</span></span>|  
|<span data-ttu-id="bc057-461">**Pole**</span><span class="sxs-lookup"><span data-stu-id="bc057-461">**Array**</span></span>|<span data-ttu-id="bc057-462">Pořadí počtu nula či více elementů.</span><span class="sxs-lookup"><span data-stu-id="bc057-462">A sequence of zero or more elements.</span></span> <span data-ttu-id="bc057-463">Každý element může být hodnota všechny skalární datového typu, s výjimkou Undefined.</span><span class="sxs-lookup"><span data-stu-id="bc057-463">Each element can be a value of any scalar data type, except Undefined.</span></span>|  
|<span data-ttu-id="bc057-464">**Objekt**</span><span class="sxs-lookup"><span data-stu-id="bc057-464">**Object**</span></span>|<span data-ttu-id="bc057-465">Neseřazený sadu nula nebo více dvojic název hodnota.</span><span class="sxs-lookup"><span data-stu-id="bc057-465">An unordered set of zero or more name/value pairs.</span></span> <span data-ttu-id="bc057-466">Název je řetězec znaků Unicode, hodnota může být jakékoli skalární datového typu, s výjimkou **Undefined**.</span><span class="sxs-lookup"><span data-stu-id="bc057-466">Name is a Unicode string, value can be of any scalar data type, except **Undefined**.</span></span>|  
  
 <span data-ttu-id="bc057-467">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-467">**Syntax**</span></span>  
  
```  
<constant> ::=  
   <undefined_constant>  
     | <null_constant>   
     | <boolean_constant>   
     | <number_constant>   
     | <string_constant>   
     | <array_constant>   
     | <object_constant>   
  
<undefined_constant> ::= undefined  
  
<null_constant> ::= null  
  
<boolean_constant> ::= false | true  
  
<number_constant> ::= decimal_literal | hexadecimal_literal  
  
<string_constant> ::= string_literal  
  
<array_constant> ::=  
    '[' [<constant>][,...n] ']'  
  
<object_constant> ::=   
   '{' [{property_name | "property_name"} : <constant>][,...n] '}'  
  
```  
  
 <span data-ttu-id="bc057-468">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-468">**Arguments**</span></span>  
  
1.  `<undefined_constant>; undefined`  
  
     <span data-ttu-id="bc057-469">Představuje není definovaná hodnota typu Undefined.</span><span class="sxs-lookup"><span data-stu-id="bc057-469">Represents undefined value of type Undefined.</span></span>  
  
2.  `<null_constant>; null`  
  
     <span data-ttu-id="bc057-470">Představuje **null** hodnotu typu **Null**.</span><span class="sxs-lookup"><span data-stu-id="bc057-470">Represents **null** value of type **Null**.</span></span>  
  
3.  `<boolean_constant>`  
  
     <span data-ttu-id="bc057-471">Představuje konstanta typu Boolean.</span><span class="sxs-lookup"><span data-stu-id="bc057-471">Represents constant of type Boolean.</span></span>  
  
4.  `false`  
  
     <span data-ttu-id="bc057-472">Představuje **false** hodnota typu Boolean.</span><span class="sxs-lookup"><span data-stu-id="bc057-472">Represents **false** value of type Boolean.</span></span>  
  
5.  `true`  
  
     <span data-ttu-id="bc057-473">Představuje **true** hodnota typu Boolean.</span><span class="sxs-lookup"><span data-stu-id="bc057-473">Represents **true** value of type Boolean.</span></span>  
  
6.  `<number_constant>`  
  
     <span data-ttu-id="bc057-474">Představuje konstantu.</span><span class="sxs-lookup"><span data-stu-id="bc057-474">Represents a constant.</span></span>  
  
7.  `decimal_literal`  
  
     <span data-ttu-id="bc057-475">Decimal literály jsou reprezentované pomocí zápisu s tečkou nebo exponenciální notace čísla.</span><span class="sxs-lookup"><span data-stu-id="bc057-475">Decimal literals are numbers represented using either decimal notation, or scientific notation.</span></span>  
  
8.  `hexadecimal_literal`  
  
     <span data-ttu-id="bc057-476">Šestnáctkové literály jsou reprezentované pomocí předpony '0 x, za nímž následuje jeden nebo více šestnáctkové číslice čísla.</span><span class="sxs-lookup"><span data-stu-id="bc057-476">Hexadecimal literals are numbers represented using prefix '0x' followed by one or more hexadecimal digits.</span></span>  
  
9. `<string_constant>`  
  
     <span data-ttu-id="bc057-477">Představuje konstantu typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="bc057-477">Represents a constant of type String.</span></span>  
  
10. `string _literal`  
  
     <span data-ttu-id="bc057-478">Textové literály jsou reprezentována posloupnost nula nebo více znaků Unicode nebo řídicí sekvence řetězců v kódu Unicode.</span><span class="sxs-lookup"><span data-stu-id="bc057-478">String literals are Unicode strings represented by a sequence of zero or more Unicode characters or escape sequences.</span></span> <span data-ttu-id="bc057-479">Textové literály jsou uzavřená do jednoduchých uvozovek (apostrof: ') nebo dvojité uvozovky (uvozovky: ").</span><span class="sxs-lookup"><span data-stu-id="bc057-479">String literals are enclosed in single quotes (apostrophe: ' ) or double quotes (quotation mark: ").</span></span>  
  
 <span data-ttu-id="bc057-480">Jsou povoleny následující řídicí sekvence:</span><span class="sxs-lookup"><span data-stu-id="bc057-480">Following escape sequences are allowed:</span></span>  
  
|<span data-ttu-id="bc057-481">**Řídicí sekvence**</span><span class="sxs-lookup"><span data-stu-id="bc057-481">**Escape sequence**</span></span>|<span data-ttu-id="bc057-482">**Popis**</span><span class="sxs-lookup"><span data-stu-id="bc057-482">**Description**</span></span>|<span data-ttu-id="bc057-483">**Znak Unicode**</span><span class="sxs-lookup"><span data-stu-id="bc057-483">**Unicode character**</span></span>|  
|-|-|-|  
|<span data-ttu-id="bc057-484">\\'</span><span class="sxs-lookup"><span data-stu-id="bc057-484">\\'</span></span>|<span data-ttu-id="bc057-485">apostrof (')</span><span class="sxs-lookup"><span data-stu-id="bc057-485">apostrophe (')</span></span>|<span data-ttu-id="bc057-486">U + 0027</span><span class="sxs-lookup"><span data-stu-id="bc057-486">U+0027</span></span>|  
|<span data-ttu-id="bc057-487">\\"</span><span class="sxs-lookup"><span data-stu-id="bc057-487">\\"</span></span>|<span data-ttu-id="bc057-488">dvojité uvozovky (")</span><span class="sxs-lookup"><span data-stu-id="bc057-488">quotation mark (")</span></span>|<span data-ttu-id="bc057-489">U + 0022</span><span class="sxs-lookup"><span data-stu-id="bc057-489">U+0022</span></span>|  
|\\\|<span data-ttu-id="bc057-490">obrácené lomítko (\\)</span><span class="sxs-lookup"><span data-stu-id="bc057-490">reverse solidus (\\)</span></span>|<span data-ttu-id="bc057-491">U + 005C</span><span class="sxs-lookup"><span data-stu-id="bc057-491">U+005C</span></span>|  
|\\/|<span data-ttu-id="bc057-492">lomítko (/)</span><span class="sxs-lookup"><span data-stu-id="bc057-492">solidus (/)</span></span>|<span data-ttu-id="bc057-493">U + 002F</span><span class="sxs-lookup"><span data-stu-id="bc057-493">U+002F</span></span>|  
|<span data-ttu-id="bc057-494">\b</span><span class="sxs-lookup"><span data-stu-id="bc057-494">\b</span></span>|<span data-ttu-id="bc057-495">BACKSPACE</span><span class="sxs-lookup"><span data-stu-id="bc057-495">backspace</span></span>|<span data-ttu-id="bc057-496">U + 0008</span><span class="sxs-lookup"><span data-stu-id="bc057-496">U+0008</span></span>|  
|<span data-ttu-id="bc057-497">\f</span><span class="sxs-lookup"><span data-stu-id="bc057-497">\f</span></span>|<span data-ttu-id="bc057-498">řídicí znak</span><span class="sxs-lookup"><span data-stu-id="bc057-498">form feed</span></span>|<span data-ttu-id="bc057-499">U + 000C</span><span class="sxs-lookup"><span data-stu-id="bc057-499">U+000C</span></span>|  
|\n|<span data-ttu-id="bc057-500">Přechod na nový řádek</span><span class="sxs-lookup"><span data-stu-id="bc057-500">line feed</span></span>|<span data-ttu-id="bc057-501">U + 000A</span><span class="sxs-lookup"><span data-stu-id="bc057-501">U+000A</span></span>|  
|<span data-ttu-id="bc057-502">\r</span><span class="sxs-lookup"><span data-stu-id="bc057-502">\r</span></span>|<span data-ttu-id="bc057-503">Návrat na začátek</span><span class="sxs-lookup"><span data-stu-id="bc057-503">carriage return</span></span>|<span data-ttu-id="bc057-504">U + 000D</span><span class="sxs-lookup"><span data-stu-id="bc057-504">U+000D</span></span>|  
|<span data-ttu-id="bc057-505">\t</span><span class="sxs-lookup"><span data-stu-id="bc057-505">\t</span></span>|<span data-ttu-id="bc057-506">Karta</span><span class="sxs-lookup"><span data-stu-id="bc057-506">tab</span></span>|<span data-ttu-id="bc057-507">U + 0009</span><span class="sxs-lookup"><span data-stu-id="bc057-507">U+0009</span></span>|  
|<span data-ttu-id="bc057-508">\uXXXX</span><span class="sxs-lookup"><span data-stu-id="bc057-508">\uXXXX</span></span>|<span data-ttu-id="bc057-509">Znak Unicode definované 4 hexadecimální číslice.</span><span class="sxs-lookup"><span data-stu-id="bc057-509">A Unicode character defined by 4 hexadecimal digits.</span></span>|<span data-ttu-id="bc057-510">U + XXXX</span><span class="sxs-lookup"><span data-stu-id="bc057-510">U+XXXX</span></span>|  
  
##  <span data-ttu-id="bc057-511"><a name="bk_query_perf_guidelines"></a>Pravidla výkonu dotazu</span><span class="sxs-lookup"><span data-stu-id="bc057-511"><a name="bk_query_perf_guidelines"></a> Query performance guidelines</span></span>  
 <span data-ttu-id="bc057-512">Aby dotaz nelze provést efektivně pro velké kolekce měla by používat filtry, které je možné dodávat prostřednictvím jednoho nebo více indexů.</span><span class="sxs-lookup"><span data-stu-id="bc057-512">In order for a query to be executed efficiently for a large collection, it should use filters which can be served through one or more indexes.</span></span>  
  
 <span data-ttu-id="bc057-513">Následující filtry se bude zvažovat indexu vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="bc057-513">The following filters will be considered for index lookup:</span></span>  
  
-   <span data-ttu-id="bc057-514">Operátor rovnosti (=) pomocí výrazu cesty dokumentu a konstanta.</span><span class="sxs-lookup"><span data-stu-id="bc057-514">Use equality operator ( = ) with a document path expression and a constant.</span></span>  
  
-   <span data-ttu-id="bc057-515">Rozsah operátory (<, \<=, >, > =) s výraz cesty dokumentu a číselné konstanty.</span><span class="sxs-lookup"><span data-stu-id="bc057-515">Use range operators (<, \<=, >, >=) with a document path expression and number constants.</span></span>  
  
-   <span data-ttu-id="bc057-516">Výraz cesty dokumentu zastupuje všechny výraz, který identifikuje konstantní cestu v dokumentech z kolekce odkazovaná databáze.</span><span class="sxs-lookup"><span data-stu-id="bc057-516">Document path expression stands for any expression which identifies a constant path in the documents from the referenced database collection.</span></span>  
  
 <span data-ttu-id="bc057-517">**Výraz cesty dokumentu**</span><span class="sxs-lookup"><span data-stu-id="bc057-517">**Document path expression**</span></span>  
  
 <span data-ttu-id="bc057-518">Výrazy cesty dokumentu jsou výrazy, cesta vlastnost nebo pole posuzovatelů indexer přes dokumentu pocházejících z databáze kolekce dokumentů.</span><span class="sxs-lookup"><span data-stu-id="bc057-518">Document path expressions are expressions that a path of property or array indexer assessors over a document coming from database collection documents.</span></span> <span data-ttu-id="bc057-519">Tuto cestu můžete použít pro určení umístění hodnot odkazuje ve filtru přímo v rámci dokumentů v kolekci databáze.</span><span class="sxs-lookup"><span data-stu-id="bc057-519">This path can be used to identify the location of values referenced in a filter directly within the documents in the database collection.</span></span>  
  
 <span data-ttu-id="bc057-520">Pro výraz považovat za výraz cesty dokumentu by měl:</span><span class="sxs-lookup"><span data-stu-id="bc057-520">For an expression to be considered a document path expression, it should:</span></span>  
  
1.  <span data-ttu-id="bc057-521">Kořenové kolekce odkazujte přímo.</span><span class="sxs-lookup"><span data-stu-id="bc057-521">Reference the collection root directly.</span></span>  
  
2.  <span data-ttu-id="bc057-522">Odkaz na vlastnost nebo konstanta indexer pole některé výrazu cesta dokumentu</span><span class="sxs-lookup"><span data-stu-id="bc057-522">Reference property or constant array indexer of some document path expression</span></span>  
  
3.  <span data-ttu-id="bc057-523">Referenční alias, který představuje výraz cesty některé dokumentu.</span><span class="sxs-lookup"><span data-stu-id="bc057-523">Reference an alias, which represents some document path expression.</span></span>  
  
     <span data-ttu-id="bc057-524">**Syntaxe konvence**</span><span class="sxs-lookup"><span data-stu-id="bc057-524">**Syntax conventions**</span></span>  
  
     <span data-ttu-id="bc057-525">Následující tabulka popisuje konvence používají k popisu syntaxe v referenci dotazovací jazyk DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="bc057-525">The following table describes the conventions used to describe syntax in the DocumentDB API Query Language reference.</span></span>  
  
    |<span data-ttu-id="bc057-526">**Konvence**</span><span class="sxs-lookup"><span data-stu-id="bc057-526">**Convention**</span></span>|<span data-ttu-id="bc057-527">**Použít pro**</span><span class="sxs-lookup"><span data-stu-id="bc057-527">**Used for**</span></span>|  
    |-|-|    
    |<span data-ttu-id="bc057-528">VELKÁ PÍSMENA</span><span class="sxs-lookup"><span data-stu-id="bc057-528">UPPERCASE</span></span>|<span data-ttu-id="bc057-529">Klíčová slova velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="bc057-529">Case-insensitive keywords.</span></span>|  
    |<span data-ttu-id="bc057-530">malá písmena</span><span class="sxs-lookup"><span data-stu-id="bc057-530">lowercase</span></span>|<span data-ttu-id="bc057-531">Klíčová slova malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="bc057-531">Case-sensitive keywords.</span></span>|  
    |<span data-ttu-id="bc057-532">\<nonterminal ></span><span class="sxs-lookup"><span data-stu-id="bc057-532">\<nonterminal></span></span>|<span data-ttu-id="bc057-533">Nonterminal, definované samostatně.</span><span class="sxs-lookup"><span data-stu-id="bc057-533">Nonterminal, defined separately.</span></span>|  
    |<span data-ttu-id="bc057-534">\<nonterminal >:: =</span><span class="sxs-lookup"><span data-stu-id="bc057-534">\<nonterminal> ::=</span></span>|<span data-ttu-id="bc057-535">Syntaxe definice nonterminal.</span><span class="sxs-lookup"><span data-stu-id="bc057-535">Syntax definition of the nonterminal.</span></span>|  
    |<span data-ttu-id="bc057-536">other_terminal</span><span class="sxs-lookup"><span data-stu-id="bc057-536">other_terminal</span></span>|<span data-ttu-id="bc057-537">Terminálové (token), podrobně popsané v slova.</span><span class="sxs-lookup"><span data-stu-id="bc057-537">Terminal (token), described in detail in words.</span></span>|  
    |<span data-ttu-id="bc057-538">Identifikátor</span><span class="sxs-lookup"><span data-stu-id="bc057-538">identifier</span></span>|<span data-ttu-id="bc057-539">Identifikátor.</span><span class="sxs-lookup"><span data-stu-id="bc057-539">Identifier.</span></span> <span data-ttu-id="bc057-540">Umožňuje následující pouze znaky: a – z A-Z 0-9 _First znak nemůže být číslice.</span><span class="sxs-lookup"><span data-stu-id="bc057-540">Allows following characters only: a-z A-Z 0-9 _First character cannot be a digit.</span></span>|  
    |<span data-ttu-id="bc057-541">"řetězec"</span><span class="sxs-lookup"><span data-stu-id="bc057-541">"string"</span></span>|<span data-ttu-id="bc057-542">Řetězec v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="bc057-542">Quoted string.</span></span> <span data-ttu-id="bc057-543">Umožňuje libovolný platný řetězec.</span><span class="sxs-lookup"><span data-stu-id="bc057-543">Allows any valid string.</span></span> <span data-ttu-id="bc057-544">Viz popis string_literal.</span><span class="sxs-lookup"><span data-stu-id="bc057-544">See description of a string_literal.</span></span>|  
    |<span data-ttu-id="bc057-545">'symbol.</span><span class="sxs-lookup"><span data-stu-id="bc057-545">'symbol'</span></span>|<span data-ttu-id="bc057-546">Literál symbol, který je součástí syntaxe.</span><span class="sxs-lookup"><span data-stu-id="bc057-546">Literal symbol which is part of the syntax.</span></span>|  
    |<span data-ttu-id="bc057-547">&#124; (svislé čáry)</span><span class="sxs-lookup"><span data-stu-id="bc057-547">&#124; (vertical bar)</span></span>|<span data-ttu-id="bc057-548">Alternativy pro položky syntaxe.</span><span class="sxs-lookup"><span data-stu-id="bc057-548">Alternatives for syntax items.</span></span> <span data-ttu-id="bc057-549">Můžete vytvořit pouze jeden položky.</span><span class="sxs-lookup"><span data-stu-id="bc057-549">You can use only one of the items specified.</span></span>|  
    |<span data-ttu-id="bc057-550">[] /(brackets)</span><span class="sxs-lookup"><span data-stu-id="bc057-550">[ ] /(brackets)</span></span>|<span data-ttu-id="bc057-551">Závorky uzavřete jeden nebo více nepovinných položek.</span><span class="sxs-lookup"><span data-stu-id="bc057-551">Brackets enclose one or more optional items.</span></span>|  
    |<span data-ttu-id="bc057-552">[,.. .n]</span><span class="sxs-lookup"><span data-stu-id="bc057-552">[ ,...n ]</span></span>|<span data-ttu-id="bc057-553">Označuje, že předchozí položce může být opakovaný n stanovený počet.</span><span class="sxs-lookup"><span data-stu-id="bc057-553">Indicates the preceding item can be repeated n number of times.</span></span> <span data-ttu-id="bc057-554">Výskytů jsou oddělené čárkami.</span><span class="sxs-lookup"><span data-stu-id="bc057-554">The occurrences are separated by commas.</span></span>|  
    |<span data-ttu-id="bc057-555">[.. .n]</span><span class="sxs-lookup"><span data-stu-id="bc057-555">[ ...n ]</span></span>|<span data-ttu-id="bc057-556">Označuje, že předchozí položce může být opakovaný n stanovený počet.</span><span class="sxs-lookup"><span data-stu-id="bc057-556">Indicates the preceding item can be repeated n number of times.</span></span> <span data-ttu-id="bc057-557">Výskytů se oddělují mezerami.</span><span class="sxs-lookup"><span data-stu-id="bc057-557">The occurrences are separated by blanks.</span></span>|  
  
##  <span data-ttu-id="bc057-558"><a name="bk_built_in_functions"></a>Integrované funkce</span><span class="sxs-lookup"><span data-stu-id="bc057-558"><a name="bk_built_in_functions"></a> Built-in functions</span></span>  
 <span data-ttu-id="bc057-559">Azure Cosmos DB poskytuje mnoho předdefinovaných funkcí SQL.</span><span class="sxs-lookup"><span data-stu-id="bc057-559">Azure Cosmos DB provides many built-in SQL functions.</span></span> <span data-ttu-id="bc057-560">Kategorie integrované funkce jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="bc057-560">The categories of built-in functions are listed below.</span></span>  
  
|<span data-ttu-id="bc057-561">Funkce</span><span class="sxs-lookup"><span data-stu-id="bc057-561">Function</span></span>|<span data-ttu-id="bc057-562">Popis</span><span class="sxs-lookup"><span data-stu-id="bc057-562">Description</span></span>|  
|--------------|-----------------|  
|[<span data-ttu-id="bc057-563">Matematické funkce</span><span class="sxs-lookup"><span data-stu-id="bc057-563">Mathematical functions</span></span>](#bk_mathematical_functions)|<span data-ttu-id="bc057-564">Matematické funkce provedení výpočtů, obvykle podle vstupní hodnoty, které jsou k dispozici jako argumenty a vrátí číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-564">The mathematical functions each perform a calculation, usually based on input values that are provided as arguments, and return a numeric value.</span></span>|  
|[<span data-ttu-id="bc057-565">Typ kontroly funkce</span><span class="sxs-lookup"><span data-stu-id="bc057-565">Type checking functions</span></span>](#bk_type_checking_functions)|<span data-ttu-id="bc057-566">Kontrola, zda funkce typů umožňují zkontrolujte typ výrazu v rámci dotazů SQL.</span><span class="sxs-lookup"><span data-stu-id="bc057-566">The type checking functions allow you to check the type of an expression within SQL queries.</span></span>|  
|[<span data-ttu-id="bc057-567">Funkce řetězců</span><span class="sxs-lookup"><span data-stu-id="bc057-567">String functions</span></span>](#bk_string_functions)|<span data-ttu-id="bc057-568">Funkce řetězce provést operaci s vstupní hodnotu řetězce a vrátí řetězec, číselnou nebo logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="bc057-568">The string functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span>|  
|[<span data-ttu-id="bc057-569">Funkce pole</span><span class="sxs-lookup"><span data-stu-id="bc057-569">Array functions</span></span>](#bk_array_functions)|<span data-ttu-id="bc057-570">Funkce pole provést operaci na hodnotu vstupní pole a vrátí číselnou, logická hodnota nebo pole hodnota.</span><span class="sxs-lookup"><span data-stu-id="bc057-570">The array functions perform an operation on an array input value and return numeric, Boolean or array value.</span></span>|  
|[<span data-ttu-id="bc057-571">Prostorové funkce</span><span class="sxs-lookup"><span data-stu-id="bc057-571">Spatial functions</span></span>](#bk_spatial_functions)|<span data-ttu-id="bc057-572">Prostorové funkce provést operaci s vstupní hodnotu prostorový objekt a vrátit hodnotu číselná nebo logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="bc057-572">The spatial functions perform an operation on an spatial object input value and return a numeric or Boolean value.</span></span>|  
  
###  <span data-ttu-id="bc057-573"><a name="bk_mathematical_functions"></a>Matematické funkce</span><span class="sxs-lookup"><span data-stu-id="bc057-573"><a name="bk_mathematical_functions"></a> Mathematical functions</span></span>  
 <span data-ttu-id="bc057-574">Následující funkce provedení výpočtů, obvykle podle vstupní hodnoty, které jsou k dispozici jako argumenty a vrátí číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-574">The following functions each perform a calculation, usually based on input values that are provided as arguments, and return a numeric value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="bc057-575">ABS</span><span class="sxs-lookup"><span data-stu-id="bc057-575">ABS</span></span>](#bk_abs)|[<span data-ttu-id="bc057-576">ACOS</span><span class="sxs-lookup"><span data-stu-id="bc057-576">ACOS</span></span>](#bk_acos)|[<span data-ttu-id="bc057-577">ASIN</span><span class="sxs-lookup"><span data-stu-id="bc057-577">ASIN</span></span>](#bk_asin)|  
|[<span data-ttu-id="bc057-578">ATAN</span><span class="sxs-lookup"><span data-stu-id="bc057-578">ATAN</span></span>](#bk_atan)|[<span data-ttu-id="bc057-579">ATN2</span><span class="sxs-lookup"><span data-stu-id="bc057-579">ATN2</span></span>](#bk_atn2)|[<span data-ttu-id="bc057-580">MEZNÍ HODNOTY</span><span class="sxs-lookup"><span data-stu-id="bc057-580">CEILING</span></span>](#bk_ceiling)|  
|[<span data-ttu-id="bc057-581">COS</span><span class="sxs-lookup"><span data-stu-id="bc057-581">COS</span></span>](#bk_cos)|[<span data-ttu-id="bc057-582">COP</span><span class="sxs-lookup"><span data-stu-id="bc057-582">COT</span></span>](#bk_cot)|[<span data-ttu-id="bc057-583">STUPŇŮ</span><span class="sxs-lookup"><span data-stu-id="bc057-583">DEGREES</span></span>](#bk_degrees)|  
|[<span data-ttu-id="bc057-584">EXP</span><span class="sxs-lookup"><span data-stu-id="bc057-584">EXP</span></span>](#bk_exp)|[<span data-ttu-id="bc057-585">FLOOR</span><span class="sxs-lookup"><span data-stu-id="bc057-585">FLOOR</span></span>](#bk_floor)|[<span data-ttu-id="bc057-586">PROTOKOLU</span><span class="sxs-lookup"><span data-stu-id="bc057-586">LOG</span></span>](#bk_log)|  
|[<span data-ttu-id="bc057-587">LOG10</span><span class="sxs-lookup"><span data-stu-id="bc057-587">LOG10</span></span>](#bk_log10)|[<span data-ttu-id="bc057-588">PLATFORMY</span><span class="sxs-lookup"><span data-stu-id="bc057-588">PI</span></span>](#bk_pi)|[<span data-ttu-id="bc057-589">NAPÁJENÍ</span><span class="sxs-lookup"><span data-stu-id="bc057-589">POWER</span></span>](#bk_power)|  
|[<span data-ttu-id="bc057-590">RADIÁNECH</span><span class="sxs-lookup"><span data-stu-id="bc057-590">RADIANS</span></span>](#bk_radians)|[<span data-ttu-id="bc057-591">ZAOKROUHLIT</span><span class="sxs-lookup"><span data-stu-id="bc057-591">ROUND</span></span>](#bk_round)|[<span data-ttu-id="bc057-592">SIN</span><span class="sxs-lookup"><span data-stu-id="bc057-592">SIN</span></span>](#bk_sin)|  
|[<span data-ttu-id="bc057-593">SQRT</span><span class="sxs-lookup"><span data-stu-id="bc057-593">SQRT</span></span>](#bk_sqrt)|[<span data-ttu-id="bc057-594">HRANATÉ</span><span class="sxs-lookup"><span data-stu-id="bc057-594">SQUARE</span></span>](#bk_square)|[<span data-ttu-id="bc057-595">PŘIHLÁŠENÍ</span><span class="sxs-lookup"><span data-stu-id="bc057-595">SIGN</span></span>](#bk_sign)|  
|[<span data-ttu-id="bc057-596">TAN</span><span class="sxs-lookup"><span data-stu-id="bc057-596">TAN</span></span>](#bk_tan)|[<span data-ttu-id="bc057-597">TRUNC</span><span class="sxs-lookup"><span data-stu-id="bc057-597">TRUNC</span></span>](#bk_trunc)||  
  
####  <span data-ttu-id="bc057-598"><a name="bk_abs"></a>ABS</span><span class="sxs-lookup"><span data-stu-id="bc057-598"><a name="bk_abs"></a> ABS</span></span>  
 <span data-ttu-id="bc057-599">Vrátí absolutní hodnotu (kladné) zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-599">Returns the absolute (positive) value of the specified numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-600">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-600">**Syntax**</span></span>  
  
```  
ABS (<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-601">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-601">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-602">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-602">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-603">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-603">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-604">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-604">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-605">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-605">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-606">Následující příklad ukazuje výsledky pomocí funkce ABS na tři odlišné počty.</span><span class="sxs-lookup"><span data-stu-id="bc057-606">The following example shows the results of using the ABS function on three different numbers.</span></span>  
  
```  
SELECT ABS(-1), ABS(0), ABS(1)  
```  
  
 <span data-ttu-id="bc057-607">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-607">Here is the result set.</span></span>  
  
```  
[{$1: 1, $2: 0, $3: 1}]  
```  
  
####  <span data-ttu-id="bc057-608"><a name="bk_acos"></a>ACOS</span><span class="sxs-lookup"><span data-stu-id="bc057-608"><a name="bk_acos"></a> ACOS</span></span>  
 <span data-ttu-id="bc057-609">Vrací úhel, v radiánech, jehož kosinus je zadaný číselný výraz. Zkratka Arkus.</span><span class="sxs-lookup"><span data-stu-id="bc057-609">Returns the angle, in radians, whose cosine is the specified numeric expression; also called arccosine.</span></span>  
  
 <span data-ttu-id="bc057-610">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-610">**Syntax**</span></span>  
  
```  
ACOS(<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-611">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-611">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-612">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-612">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-613">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-613">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-614">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-614">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-615">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-615">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-616">Následující příklad vrátí ACOS-1.</span><span class="sxs-lookup"><span data-stu-id="bc057-616">The following example returns the ACOS of -1.</span></span>  
  
```  
SELECT ACOS(-1)  
```  
  
 <span data-ttu-id="bc057-617">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-617">Here is the result set.</span></span>  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <span data-ttu-id="bc057-618"><a name="bk_asin"></a>ASIN</span><span class="sxs-lookup"><span data-stu-id="bc057-618"><a name="bk_asin"></a> ASIN</span></span>  
 <span data-ttu-id="bc057-619">Vrací úhel, v radiánech, jehož sinus je zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-619">Returns the angle, in radians, whose sine is the specified numeric expression.</span></span> <span data-ttu-id="bc057-620">To je také označován Arkus sinus.</span><span class="sxs-lookup"><span data-stu-id="bc057-620">This is also called arcsine.</span></span>  
  
 <span data-ttu-id="bc057-621">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-621">**Syntax**</span></span>  
  
```  
ASIN(<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-622">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-622">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-623">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-623">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-624">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-624">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-625">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-625">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-626">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-626">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-627">Následující příklad vrátí ASIN-1.</span><span class="sxs-lookup"><span data-stu-id="bc057-627">The following example returns the ASIN of -1.</span></span>  
  
```  
SELECT ASIN(-1)  
```  
  
 <span data-ttu-id="bc057-628">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-628">Here is the result set.</span></span>  
  
```  
[{"$1": -1.5707963267948966}]  
```  
  
####  <span data-ttu-id="bc057-629"><a name="bk_atan"></a>ATAN</span><span class="sxs-lookup"><span data-stu-id="bc057-629"><a name="bk_atan"></a> ATAN</span></span>  
 <span data-ttu-id="bc057-630">Vrací úhel, v radiánech, jehož tangens je zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-630">Returns the angle, in radians, whose tangent is the specified numeric expression.</span></span> <span data-ttu-id="bc057-631">To je také označován Arkus.</span><span class="sxs-lookup"><span data-stu-id="bc057-631">This is also called arctangent.</span></span>  
  
 <span data-ttu-id="bc057-632">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-632">**Syntax**</span></span>  
  
```  
ATAN(<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-633">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-633">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-634">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-634">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-635">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-635">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-636">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-636">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-637">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-637">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-638">Následující příklad vrátí ATAN zadané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bc057-638">The following example returns the ATAN of the specified value.</span></span>  
  
```  
SELECT ATAN(-45.01)  
```  
  
 <span data-ttu-id="bc057-639">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-639">Here is the result set.</span></span>  
  
```  
[{"$1": -1.5485826962062663}]  
```  
  
####  <span data-ttu-id="bc057-640"><a name="bk_atn2"></a>ATN2</span><span class="sxs-lookup"><span data-stu-id="bc057-640"><a name="bk_atn2"></a> ATN2</span></span>  
 <span data-ttu-id="bc057-641">Vrací hlavní hodnotu tangens oblouk y / x, vyjádřené v radiánech.</span><span class="sxs-lookup"><span data-stu-id="bc057-641">Returns the principal value of the arc tangent of y/x, expressed in radians.</span></span>  
  
 <span data-ttu-id="bc057-642">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-642">**Syntax**</span></span>  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-643">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-643">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-644">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-644">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-645">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-645">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-646">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-646">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-647">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-647">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-648">Následující příklad vypočítá ATN2 pro zadaný x a y součásti.</span><span class="sxs-lookup"><span data-stu-id="bc057-648">The following example calculates the ATN2 for the specified x and y components.</span></span>  
  
```  
SELECT ATN2(35.175643, 129.44)  
```  
  
 <span data-ttu-id="bc057-649">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-649">Here is the result set.</span></span>  
  
```  
[{"$1": 1.3054517947300646}]  
```  
  
####  <span data-ttu-id="bc057-650"><a name="bk_ceiling"></a>MEZNÍ HODNOTY</span><span class="sxs-lookup"><span data-stu-id="bc057-650"><a name="bk_ceiling"></a> CEILING</span></span>  
 <span data-ttu-id="bc057-651">Vrátí nejmenší hodnotu, celé číslo větší než nebo rovna hodnotě zadané číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-651">Returns the smallest integer value greater than, or equal to, the specified numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-652">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-652">**Syntax**</span></span>  
  
```  
CEILING (<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-653">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-653">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-654">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-654">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-655">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-655">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-656">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-656">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-657">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-657">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-658">Následující příklad ukazuje kladné číselné, záporné a nulové hodnoty pomocí funkce mezní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bc057-658">The following example shows positive numeric, negative, and zero values with the CEILING function.</span></span>  
  
```  
SELECT CEILING(123.45), CEILING(-123.45), CEILING(0.0)  
```  
  
 <span data-ttu-id="bc057-659">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-659">Here is the result set.</span></span>  
  
```  
[{$1: 124, $2: -123, $3: 0}]  
```  
  
####  <span data-ttu-id="bc057-660"><a name="bk_cos"></a>COS</span><span class="sxs-lookup"><span data-stu-id="bc057-660"><a name="bk_cos"></a> COS</span></span>  
 <span data-ttu-id="bc057-661">Vrací trigonometrické kosinus určeného úhlu v radiánech v zadaným výrazem.</span><span class="sxs-lookup"><span data-stu-id="bc057-661">Returns the trigonometric cosine of the specified angle, in radians, in the specified expression.</span></span>  
  
 <span data-ttu-id="bc057-662">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-662">**Syntax**</span></span>  
  
```  
COS(<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-663">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-663">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-664">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-664">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-665">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-665">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-666">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-666">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-667">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-667">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-668">Následující příklad vypočítá COS určeného úhlu.</span><span class="sxs-lookup"><span data-stu-id="bc057-668">The following example calculates the COS of the specified angle.</span></span>  
  
```  
SELECT COS(14.78)  
```  
  
 <span data-ttu-id="bc057-669">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-669">Here is the result set.</span></span>  
  
```  
[{"$1": -0.59946542619465426}]  
```  
  
####  <span data-ttu-id="bc057-670"><a name="bk_cot"></a>COP</span><span class="sxs-lookup"><span data-stu-id="bc057-670"><a name="bk_cot"></a> COT</span></span>  
 <span data-ttu-id="bc057-671">Vrací trigonometrické kotangens zadaný úhel v radiánech v zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-671">Returns the trigonometric cotangent of the specified angle, in radians, in the specified numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-672">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-672">**Syntax**</span></span>  
  
```  
COT(<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-673">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-673">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-674">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-674">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-675">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-675">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-676">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-676">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-677">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-677">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-678">Následující příklad vypočítá COP z určeného úhlu.</span><span class="sxs-lookup"><span data-stu-id="bc057-678">The following example calculates the COT of the specified angle.</span></span>  
  
```  
SELECT COT(124.1332)  
```  
  
 <span data-ttu-id="bc057-679">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-679">Here is the result set.</span></span>  
  
```  
[{"$1": -0.040311998371148884}]  
```  
  
####  <span data-ttu-id="bc057-680"><a name="bk_degrees"></a>STUPŇŮ</span><span class="sxs-lookup"><span data-stu-id="bc057-680"><a name="bk_degrees"></a> DEGREES</span></span>  
 <span data-ttu-id="bc057-681">Vrací odpovídající úhel ve stupních pro úhlu uvedeného v radiánech.</span><span class="sxs-lookup"><span data-stu-id="bc057-681">Returns the corresponding angle in degrees for an angle specified in radians.</span></span>  
  
 <span data-ttu-id="bc057-682">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-682">**Syntax**</span></span>  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-683">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-683">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-684">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-684">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-685">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-685">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-686">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-686">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-687">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-687">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-688">Následující příklad vrátí počet stupňů v úhel radiánech PÍ/2.</span><span class="sxs-lookup"><span data-stu-id="bc057-688">The following example returns the number of degrees in an angle of PI/2 radians.</span></span>  
  
```  
SELECT DEGREES(PI()/2)  
```  
  
 <span data-ttu-id="bc057-689">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-689">Here is the result set.</span></span>  
  
```  
[{"$1": 90}]  
```  
  
####  <span data-ttu-id="bc057-690"><a name="bk_floor"></a>FLOOR</span><span class="sxs-lookup"><span data-stu-id="bc057-690"><a name="bk_floor"></a> FLOOR</span></span>  
 <span data-ttu-id="bc057-691">Vrátí největší celé číslo menší než nebo rovna zadané číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-691">Returns the largest integer less than or equal to the specified numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-692">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-692">**Syntax**</span></span>  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-693">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-693">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-694">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-694">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-695">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-695">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-696">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-696">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-697">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-697">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-698">Následující příklad ukazuje kladné číselné, záporné a nulové hodnoty pomocí funkce FLOOR.</span><span class="sxs-lookup"><span data-stu-id="bc057-698">The following example shows positive numeric, negative, and zero values with the FLOOR function.</span></span>  
  
```  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR(0.0)  
```  
  
 <span data-ttu-id="bc057-699">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-699">Here is the result set.</span></span>  
  
```  
[{$1: 123, $2: -124, $3: 0}]  
```  
  
####  <span data-ttu-id="bc057-700"><a name="bk_exp"></a>EXP</span><span class="sxs-lookup"><span data-stu-id="bc057-700"><a name="bk_exp"></a> EXP</span></span>  
 <span data-ttu-id="bc057-701">Vrátí hodnotu exponenciálního zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-701">Returns the exponential value of the specified numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-702">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-702">**Syntax**</span></span>  
  
```  
EXP (<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-703">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-703">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-704">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-704">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-705">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-705">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-706">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-706">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-707">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="bc057-707">**Remarks**</span></span>  
  
 <span data-ttu-id="bc057-708">Konstanta **e** (2.718281...), je základ přirozeného logaritmu.</span><span class="sxs-lookup"><span data-stu-id="bc057-708">The constant **e** (2.718281…), is the base of natural logarithms.</span></span>  
  
 <span data-ttu-id="bc057-709">Exponent číslo je konstanta **e** umocněné číslo.</span><span class="sxs-lookup"><span data-stu-id="bc057-709">The exponent of a number is the constant **e** raised to the power of the number.</span></span> <span data-ttu-id="bc057-710">Například EXP(1.0) = e ^ 1.0 = 2.71828182845905 a EXP(10) = e ^ 10 = 22026.4657948067.</span><span class="sxs-lookup"><span data-stu-id="bc057-710">For example EXP(1.0) = e^1.0 = 2.71828182845905 and EXP(10) = e^10 = 22026.4657948067.</span></span>  
  
 <span data-ttu-id="bc057-711">Exponent přirozený logaritmus čísla se číslo samotné: EXP (protokol (ne)) = n.</span><span class="sxs-lookup"><span data-stu-id="bc057-711">The exponential of the natural logarithm of a number is the number itself: EXP (LOG (n)) = n.</span></span> <span data-ttu-id="bc057-712">A přirozený logaritmus čísla exponenciální je číslo samotné: protokolu (EXP (ne)) = n.</span><span class="sxs-lookup"><span data-stu-id="bc057-712">And the natural logarithm of the exponential of a number is the number itself: LOG (EXP (n)) = n.</span></span>  
  
 <span data-ttu-id="bc057-713">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-713">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-714">Následující příklad deklaruje proměnné a vrátí hodnotu exponenciálního zadanou proměnnou (10).</span><span class="sxs-lookup"><span data-stu-id="bc057-714">The following example declares a variable and returns the exponential value of the specified variable (10).</span></span>  
  
```  
SELECT EXP(10)  
```  
  
 <span data-ttu-id="bc057-715">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-715">Here is the result set.</span></span>  
  
```  
[{$1: 22026.465794806718}]  
```  
  
 <span data-ttu-id="bc057-716">Následující příklad vrátí hodnotu exponenciálního natural logarithm 20 a přirozený logaritmus exponenciální 20.</span><span class="sxs-lookup"><span data-stu-id="bc057-716">The following example returns the exponential value of the natural logarithm of 20 and the natural logarithm of the exponential of 20.</span></span> <span data-ttu-id="bc057-717">Protože tyto funkce jsou inverzní funkce vzájemně, návratovou hodnotu s zaokrouhlení pro plovoucí matematické bodu v obou případech je 20.</span><span class="sxs-lookup"><span data-stu-id="bc057-717">Because these functions are inverse functions of one another, the return value with rounding for floating point math in both cases is 20.</span></span>  
  
```  
SELECT EXP(LOG(20)), LOG(EXP(20))  
```  
  
 <span data-ttu-id="bc057-718">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-718">Here is the result set.</span></span>  
  
```  
[{$1: 19.999999999999996, $2: 20}]  
```  
  
####  <span data-ttu-id="bc057-719"><a name="bk_log"></a>PROTOKOLU</span><span class="sxs-lookup"><span data-stu-id="bc057-719"><a name="bk_log"></a> LOG</span></span>  
 <span data-ttu-id="bc057-720">Vrátí přirozený logaritmus zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-720">Returns the natural logarithm of the specified numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-721">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-721">**Syntax**</span></span>  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 <span data-ttu-id="bc057-722">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-722">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-723">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-723">Is a numeric expression.</span></span>  
  
-   `base`  
  
     <span data-ttu-id="bc057-724">Nepovinný argument číselné, který nastaví základ pro logaritmus</span><span class="sxs-lookup"><span data-stu-id="bc057-724">Optional numeric argument that sets the base for the logarithm.</span></span>  
  
 <span data-ttu-id="bc057-725">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-725">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-726">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-726">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-727">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="bc057-727">**Remarks**</span></span>  
  
 <span data-ttu-id="bc057-728">Ve výchozím nastavení LOG() vrátí přirozený logaritmus.</span><span class="sxs-lookup"><span data-stu-id="bc057-728">By default, LOG() returns the natural logarithm.</span></span> <span data-ttu-id="bc057-729">Základ logaritmu. můžete změnit na jinou hodnotu pomocí základní volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="bc057-729">You can change the base of the logarithm to another value by using the optional base parameter.</span></span>  
  
 <span data-ttu-id="bc057-730">Přirozený logaritmus je logaritmus s jejím základem **e**, kde **e** je přibližně rovna 2.718281828 nenormální konstanta.</span><span class="sxs-lookup"><span data-stu-id="bc057-730">The natural logarithm is the logarithm to the base **e**, where **e** is an irrational constant approximately equal to 2.718281828.</span></span>  
  
 <span data-ttu-id="bc057-731">Přirozený logaritmus čísla exponenciální je číslo samotné: protokolu (EXP (ne)) = n.</span><span class="sxs-lookup"><span data-stu-id="bc057-731">The natural logarithm of the exponential of a number is the number itself: LOG( EXP( n ) ) = n.</span></span> <span data-ttu-id="bc057-732">A exponent přirozený logaritmus čísla je číslo samotné: EXP (protokol (ne)) = n.</span><span class="sxs-lookup"><span data-stu-id="bc057-732">And the exponential of the natural logarithm of a number is the number itself: EXP( LOG( n ) ) = n.</span></span>  
  
 <span data-ttu-id="bc057-733">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-733">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-734">Následující příklad deklaruje proměnné a Vrátí logaritmus hodnotu zadanou proměnnou (10).</span><span class="sxs-lookup"><span data-stu-id="bc057-734">The following example declares a variable and returns the logarithm value of the specified variable (10).</span></span>  
  
```  
SELECT LOG(10)  
```  
  
 <span data-ttu-id="bc057-735">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-735">Here is the result set.</span></span>  
  
```  
[{$1: 2.3025850929940459}]  
```  
  
 <span data-ttu-id="bc057-736">Následující příklad vypočítá protokol pro exponent číslo.</span><span class="sxs-lookup"><span data-stu-id="bc057-736">The following example calculates the LOG for the exponent of a number.</span></span>  
  
```  
SELECT EXP(LOG(10))  
```  
  
 <span data-ttu-id="bc057-737">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-737">Here is the result set.</span></span>  
  
```  
[{$1: 10.000000000000002}]  
```  
  
####  <span data-ttu-id="bc057-738"><a name="bk_log10"></a>LOG10</span><span class="sxs-lookup"><span data-stu-id="bc057-738"><a name="bk_log10"></a> LOG10</span></span>  
 <span data-ttu-id="bc057-739">Vrátí logaritmus o základu 10 zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-739">Returns the base-10 logarithm of the specified numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-740">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-740">**Syntax**</span></span>  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-741">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-741">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-742">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-742">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-743">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-743">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-744">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-744">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-745">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="bc057-745">**Remarks**</span></span>  
  
 <span data-ttu-id="bc057-746">Funkce LOG10 a POWER souvisejí nepřímo jednu na druhou.</span><span class="sxs-lookup"><span data-stu-id="bc057-746">The LOG10 and POWER functions are inversely related to one another.</span></span> <span data-ttu-id="bc057-747">Například 10 ^ LOG10(n) = n.</span><span class="sxs-lookup"><span data-stu-id="bc057-747">For example, 10 ^ LOG10(n) = n.</span></span>  
  
 <span data-ttu-id="bc057-748">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-748">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-749">Následující příklad deklaruje proměnné a vrátí hodnotu LOG10 zadanou proměnnou (100).</span><span class="sxs-lookup"><span data-stu-id="bc057-749">The following example declares a variable and returns the LOG10 value of the specified variable (100).</span></span>  
  
```  
SELECT LOG10(100)  
```  
  
 <span data-ttu-id="bc057-750">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-750">Here is the result set.</span></span>  
  
```  
[{$1: 2}]  
```  
  
####  <span data-ttu-id="bc057-751"><a name="bk_pi"></a>PLATFORMY</span><span class="sxs-lookup"><span data-stu-id="bc057-751"><a name="bk_pi"></a> PI</span></span>  
 <span data-ttu-id="bc057-752">Vrátí konstantní hodnotu čísla PÍ.</span><span class="sxs-lookup"><span data-stu-id="bc057-752">Returns the constant value of PI.</span></span>  
  
 <span data-ttu-id="bc057-753">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-753">**Syntax**</span></span>  
  
```  
PI ()  
```  
  
 <span data-ttu-id="bc057-754">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-754">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-755">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-755">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-756">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-756">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-757">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-757">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-758">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-758">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-759">Následující příklad vrátí hodnotu čísla PÍ.</span><span class="sxs-lookup"><span data-stu-id="bc057-759">The following example returns the value of PI.</span></span>  
  
```  
SELECT PI()  
```  
  
 <span data-ttu-id="bc057-760">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-760">Here is the result set.</span></span>  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <span data-ttu-id="bc057-761"><a name="bk_power"></a>NAPÁJENÍ</span><span class="sxs-lookup"><span data-stu-id="bc057-761"><a name="bk_power"></a> POWER</span></span>  
 <span data-ttu-id="bc057-762">Vrátí hodnotu zadaného výrazu na zadanou mocninu.</span><span class="sxs-lookup"><span data-stu-id="bc057-762">Returns the value of the specified expression to the specified power.</span></span>  
  
 <span data-ttu-id="bc057-763">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-763">**Syntax**</span></span>  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 <span data-ttu-id="bc057-764">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-764">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-765">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-765">Is a numeric expression.</span></span>  
  
-   `y`  
  
     <span data-ttu-id="bc057-766">Je napájení, do kterého se mají zvýšit `numeric_expression`.</span><span class="sxs-lookup"><span data-stu-id="bc057-766">Is the power to which to raise `numeric_expression`.</span></span>  
  
 <span data-ttu-id="bc057-767">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-767">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-768">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-768">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-769">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-769">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-770">Následující příklad ukazuje, vyvolání číslo exponentem 3 (datové krychle číslo).</span><span class="sxs-lookup"><span data-stu-id="bc057-770">The following example demonstrates raising a number to the power of 3 (the cube of the number).</span></span>  
  
```  
SELECT POWER(2, 3), POWER(2.5, 3)  
```  
  
 <span data-ttu-id="bc057-771">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-771">Here is the result set.</span></span>  
  
```  
[{$1: 8, $2: 15.625}]  
```  
  
####  <span data-ttu-id="bc057-772"><a name="bk_radians"></a>RADIÁNECH</span><span class="sxs-lookup"><span data-stu-id="bc057-772"><a name="bk_radians"></a> RADIANS</span></span>  
 <span data-ttu-id="bc057-773">Vrátí radiánech při zadání číselného výrazu, ve stupních, se.</span><span class="sxs-lookup"><span data-stu-id="bc057-773">Returns radians when a numeric expression, in degrees, is entered.</span></span>  
  
 <span data-ttu-id="bc057-774">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-774">**Syntax**</span></span>  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-775">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-775">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-776">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-776">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-777">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-777">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-778">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-778">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-779">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-779">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-780">Následující příklad vezme jako vstupní údaje několik úhly a vrátí jejich příslušné hodnoty radián.</span><span class="sxs-lookup"><span data-stu-id="bc057-780">The following example takes a few angles as input and returns their corresponding radian values.</span></span>  
  
```  
SELECT RADIANS(-45.01), RADIANS(-181.01), RADIANS(0), RADIANS(0.1472738), RADIANS(197.1099392)  
```  
  
 <span data-ttu-id="bc057-781">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-781">Here is the result set.</span></span>  
  
```  
[{  
       "$1": -0.7855726963226477,  
       "$2": -3.1592204790349356,  
       "$3": 0,  
       "$4": 0.0025704127119236249,  
       "$5": 3.4402174274458375  
   }]  
```  
  
####  <span data-ttu-id="bc057-782"><a name="bk_round"></a>ZAOKROUHLIT</span><span class="sxs-lookup"><span data-stu-id="bc057-782"><a name="bk_round"></a> ROUND</span></span>  
 <span data-ttu-id="bc057-783">Vrátí číselnou hodnotu, zaokrouhlí na nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="bc057-783">Returns a numeric value, rounded to the closest integer value.</span></span>  
  
 <span data-ttu-id="bc057-784">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-784">**Syntax**</span></span>  
  
```  
ROUND(<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-785">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-785">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-786">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-786">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-787">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-787">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-788">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-788">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-789">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-789">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-790">V následujícím příkladu se zaokrouhlí na následující kladné a záporné čísla na nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="bc057-790">The following example rounds the following positive and negative numbers to the nearest integer.</span></span>  
  
```  
SELECT ROUND(2.4), ROUND(2.6), ROUND(2.5), ROUND(-2.4), ROUND(-2.6)  
```  
  
 <span data-ttu-id="bc057-791">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-791">Here is the result set.</span></span>  
  
```  
[{$1: 2, $2: 3, $3: 3, $4: -2, $5: -3}]  
```  
  
####  <span data-ttu-id="bc057-792"><a name="bk_sign"></a>PŘIHLÁŠENÍ</span><span class="sxs-lookup"><span data-stu-id="bc057-792"><a name="bk_sign"></a> SIGN</span></span>  
 <span data-ttu-id="bc057-793">Vrátí kladnou (+ 1), nula (0) nebo záporné znaménko (-1) zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-793">Returns the positive (+1), zero (0), or negative (-1) sign of the specified numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-794">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-794">**Syntax**</span></span>  
  
```  
SIGN(<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-795">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-795">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-796">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-796">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-797">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-797">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-798">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-798">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-799">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-799">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-800">Následující příklad vrací hodnoty znaménko čísla z -2 2.</span><span class="sxs-lookup"><span data-stu-id="bc057-800">The following example returns the SIGN values of numbers from -2 to 2.</span></span>  
  
```  
SELECT SIGN(-2), SIGN(-1), SIGN(0), SIGN(1), SIGN(2)  
```  
  
 <span data-ttu-id="bc057-801">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-801">Here is the result set.</span></span>  
  
```  
[{$1: -1, $2: -1, $3: 0, $4: 1, $5: 1}]  
```  
  
####  <span data-ttu-id="bc057-802"><a name="bk_sin"></a>SIN</span><span class="sxs-lookup"><span data-stu-id="bc057-802"><a name="bk_sin"></a> SIN</span></span>  
 <span data-ttu-id="bc057-803">Vrací trigonometrické sinus určeného úhlu v radiánech v zadaným výrazem.</span><span class="sxs-lookup"><span data-stu-id="bc057-803">Returns the trigonometric sine of the specified angle, in radians, in the specified expression.</span></span>  
  
 <span data-ttu-id="bc057-804">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-804">**Syntax**</span></span>  
  
```  
SIN(<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-805">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-805">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-806">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-806">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-807">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-807">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-808">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-808">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-809">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-809">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-810">Následující příklad vypočítá SIN z určeného úhlu.</span><span class="sxs-lookup"><span data-stu-id="bc057-810">The following example calculates the SIN of the specified angle.</span></span>  
  
```  
SELECT SIN(45.175643)  
```  
  
 <span data-ttu-id="bc057-811">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-811">Here is the result set.</span></span>  
  
```  
[{"$1": 0.929607286611012}]  
```  
  
####  <span data-ttu-id="bc057-812"><a name="bk_sqrt"></a>SQRT</span><span class="sxs-lookup"><span data-stu-id="bc057-812"><a name="bk_sqrt"></a> SQRT</span></span>  
 <span data-ttu-id="bc057-813">Vrátí druhou odmocninu čísla zadaná číselná hodnota.</span><span class="sxs-lookup"><span data-stu-id="bc057-813">Returns the square root of the specified numeric value.</span></span>  
  
 <span data-ttu-id="bc057-814">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-814">**Syntax**</span></span>  
  
```  
SQRT(<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-815">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-815">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-816">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-816">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-817">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-817">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-818">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-818">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-819">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-819">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-820">Následující příklad vrátí druhé odmocniny čísel 1 – 3.</span><span class="sxs-lookup"><span data-stu-id="bc057-820">The following example returns the square roots of numbers 1-3.</span></span>  
  
```  
SELECT SQRT(1), SQRT(2.0), SQRT(3)  
```  
  
 <span data-ttu-id="bc057-821">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-821">Here is the result set.</span></span>  
  
```  
[{$1: 1, $2: 1.4142135623730952, $3: 1.7320508075688772}]  
```  
  
####  <span data-ttu-id="bc057-822"><a name="bk_square"></a>HRANATÉ</span><span class="sxs-lookup"><span data-stu-id="bc057-822"><a name="bk_square"></a> SQUARE</span></span>  
 <span data-ttu-id="bc057-823">Vrátí druhou mocninu Zadaná číselná hodnota.</span><span class="sxs-lookup"><span data-stu-id="bc057-823">Returns the square of the specified numeric value.</span></span>  
  
 <span data-ttu-id="bc057-824">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-824">**Syntax**</span></span>  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-825">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-825">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-826">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-826">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-827">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-827">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-828">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-828">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-829">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-829">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-830">Následující příklad vrátí čtverce čísel 1 – 3.</span><span class="sxs-lookup"><span data-stu-id="bc057-830">The following example returns the squares of numbers 1-3.</span></span>  
  
```  
SELECT SQUARE(1), SQUARE(2.0), SQUARE(3)  
```  
  
 <span data-ttu-id="bc057-831">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-831">Here is the result set.</span></span>  
  
```  
[{$1: 1, $2: 4, $3: 9}]  
```  
  
####  <span data-ttu-id="bc057-832"><a name="bk_tan"></a>TAN</span><span class="sxs-lookup"><span data-stu-id="bc057-832"><a name="bk_tan"></a> TAN</span></span>  
 <span data-ttu-id="bc057-833">Vrací tangens určeného úhlu v radiánech v zadaným výrazem.</span><span class="sxs-lookup"><span data-stu-id="bc057-833">Returns the tangent of the specified angle, in radians, in the specified expression.</span></span>  
  
 <span data-ttu-id="bc057-834">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-834">**Syntax**</span></span>  
  
```  
TAN (<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-835">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-835">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-836">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-836">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-837">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-837">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-838">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-838">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-839">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-839">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-840">Následující příklad vypočítá tangens PI () / 2.</span><span class="sxs-lookup"><span data-stu-id="bc057-840">The following example calculates the tangent of PI()/2.</span></span>  
  
```  
SELECT TAN(PI()/2);  
```  
  
 <span data-ttu-id="bc057-841">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-841">Here is the result set.</span></span>  
  
```  
[{"$1": 16331239353195370 }]  
```  
  
####  <span data-ttu-id="bc057-842"><a name="bk_trunc"></a>TRUNC</span><span class="sxs-lookup"><span data-stu-id="bc057-842"><a name="bk_trunc"></a> TRUNC</span></span>  
 <span data-ttu-id="bc057-843">Vrátí číselnou hodnotu, zkrácen na nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="bc057-843">Returns a numeric value, truncated to the closest integer value.</span></span>  
  
 <span data-ttu-id="bc057-844">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-844">**Syntax**</span></span>  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 <span data-ttu-id="bc057-845">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-845">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="bc057-846">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-846">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-847">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-847">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-848">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-848">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-849">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-849">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-850">Následující příklad zkrátí následující kladné a záporné čísla na nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="bc057-850">The following example truncates the following positive and negative numbers to the nearest integer value.</span></span>  
  
```  
SELECT TRUNC(2.4), TRUNC(2.6), TRUNC(2.5), TRUNC(-2.4), TRUNC(-2.6)  
```  
  
 <span data-ttu-id="bc057-851">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-851">Here is the result set.</span></span>  
  
```  
[{$1: 2, $2: 2, $3: 2, $4: -2, $5: -2}]  
```  
  
###  <span data-ttu-id="bc057-852"><a name="bk_type_checking_functions"></a>Typ kontroly funkce</span><span class="sxs-lookup"><span data-stu-id="bc057-852"><a name="bk_type_checking_functions"></a> Type checking functions</span></span>  
 <span data-ttu-id="bc057-853">Následující funkce podporují kontroly proti vstupní hodnoty typu a každý vrátit logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-853">The following functions support type checking against input values, and each return a Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="bc057-854">IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="bc057-854">IS_ARRAY</span></span>](#bk_is_array)|[<span data-ttu-id="bc057-855">IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="bc057-855">IS_BOOL</span></span>](#bk_is_bool)|[<span data-ttu-id="bc057-856">IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="bc057-856">IS_DEFINED</span></span>](#bk_is_defined)|  
|[<span data-ttu-id="bc057-857">IS_NULL</span><span class="sxs-lookup"><span data-stu-id="bc057-857">IS_NULL</span></span>](#bk_is_null)|[<span data-ttu-id="bc057-858">IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="bc057-858">IS_NUMBER</span></span>](#bk_is_number)|[<span data-ttu-id="bc057-859">IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="bc057-859">IS_OBJECT</span></span>](#bk_is_object)|  
|[<span data-ttu-id="bc057-860">IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="bc057-860">IS_PRIMITIVE</span></span>](#bk_is_primitive)|[<span data-ttu-id="bc057-861">IS_STRING</span><span class="sxs-lookup"><span data-stu-id="bc057-861">IS_STRING</span></span>](#bk_is_string)||  
  
####  <span data-ttu-id="bc057-862"><a name="bk_is_array"></a>IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="bc057-862"><a name="bk_is_array"></a> IS_ARRAY</span></span>  
 <span data-ttu-id="bc057-863">Vrátí logickou hodnotu udávající, pokud je typ zadaný výraz pole.</span><span class="sxs-lookup"><span data-stu-id="bc057-863">Returns a Boolean value indicating if the type of the specified expression is an array.</span></span>  
  
 <span data-ttu-id="bc057-864">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-864">**Syntax**</span></span>  
  
```  
IS_ARRAY(<expression>)  
```  
  
 <span data-ttu-id="bc057-865">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-865">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="bc057-866">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-866">Is any valid expression.</span></span>  
  
 <span data-ttu-id="bc057-867">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-867">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-868">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-868">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="bc057-869">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-869">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-870">Následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí funkce IS_ARRAY.</span><span class="sxs-lookup"><span data-stu-id="bc057-870">The following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using the IS_ARRAY function.</span></span>  
  
```  
SELECT   
 IS_ARRAY(true),   
 IS_ARRAY(1),  
 IS_ARRAY("value"),  
 IS_ARRAY(null),  
 IS_ARRAY({prop: "value"}),   
 IS_ARRAY([1, 2, 3]),  
 IS_ARRAY({prop: "value"}.prop2)  
```  
  
 <span data-ttu-id="bc057-871">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-871">Here is the result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: false, $6: true}]  
```  
  
####  <span data-ttu-id="bc057-872"><a name="bk_is_bool"></a>IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="bc057-872"><a name="bk_is_bool"></a> IS_BOOL</span></span>  
 <span data-ttu-id="bc057-873">Vrátí logickou hodnotu udávající, pokud typ zadaný výraz je logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="bc057-873">Returns a Boolean value indicating if the type of the specified expression is a Boolean.</span></span>  
  
 <span data-ttu-id="bc057-874">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-874">**Syntax**</span></span>  
  
```  
IS_BOOL(<expression>)  
```  
  
 <span data-ttu-id="bc057-875">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-875">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="bc057-876">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-876">Is any valid expression.</span></span>  
  
 <span data-ttu-id="bc057-877">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-877">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-878">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-878">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="bc057-879">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-879">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-880">Následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí funkce IS_BOOL.</span><span class="sxs-lookup"><span data-stu-id="bc057-880">The following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using the IS_BOOL function.</span></span>  
  
```  
SELECT   
    IS_BOOL(true),   
    IS_BOOL(1),  
    IS_BOOL("value"),   
    IS_BOOL(null),  
    IS_BOOL({prop: "value"}),   
    IS_BOOL([1, 2, 3]),  
    IS_BOOL({prop: "value"}.prop2)  
```  
  
 <span data-ttu-id="bc057-881">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-881">Here is the result set.</span></span>  
  
```  
[{$1: true, $2: false, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="bc057-882"><a name="bk_is_defined"></a>IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="bc057-882"><a name="bk_is_defined"></a> IS_DEFINED</span></span>  
 <span data-ttu-id="bc057-883">Vrátí logickou hodnotu udávající, pokud byla vlastnost přiřazenou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-883">Returns a Boolean indicating if the property has been assigned a value.</span></span>  
  
 <span data-ttu-id="bc057-884">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-884">**Syntax**</span></span>  
  
```  
IS_DEFINED(<expression>)  
```  
  
 <span data-ttu-id="bc057-885">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-885">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="bc057-886">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-886">Is any valid expression.</span></span>  
  
 <span data-ttu-id="bc057-887">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-887">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-888">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-888">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="bc057-889">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-889">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-890">Následující příklad zjišťuje přítomnost vlastnosti v rámci zadaný dokument JSON.</span><span class="sxs-lookup"><span data-stu-id="bc057-890">The following example checks for the presence of a property within the specified JSON document.</span></span> <span data-ttu-id="bc057-891">První vrací hodnotu true, protože "a" je k dispozici, ale druhý vrací hodnotu false, protože chybí "b".</span><span class="sxs-lookup"><span data-stu-id="bc057-891">The first returns true since "a" is present, but the second returns false since "b" is absent.</span></span>  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a), IS_DEFINED({ "a" : 5 }.b)  
```  
  
 <span data-ttu-id="bc057-892">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-892">Here is the result set.</span></span>  
  
```  
[{  
       "$1": true,    
       "$2": false   
   }]  
```  
  
####  <span data-ttu-id="bc057-893"><a name="bk_is_null"></a>IS_NULL</span><span class="sxs-lookup"><span data-stu-id="bc057-893"><a name="bk_is_null"></a> IS_NULL</span></span>  
 <span data-ttu-id="bc057-894">Vrátí logickou hodnotu udávající, pokud není typ zadaného výrazu hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="bc057-894">Returns a Boolean value indicating if the type of the specified expression is null.</span></span>  
  
 <span data-ttu-id="bc057-895">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-895">**Syntax**</span></span>  
  
```  
IS_NULL(<expression>)  
```  
  
 <span data-ttu-id="bc057-896">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-896">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="bc057-897">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-897">Is any valid expression.</span></span>  
  
 <span data-ttu-id="bc057-898">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-898">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-899">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-899">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="bc057-900">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-900">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-901">Následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí funkce IS_NULL.</span><span class="sxs-lookup"><span data-stu-id="bc057-901">The following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using the IS_NULL function.</span></span>  
  
```  
SELECT   
    IS_NULL(true),   
    IS_NULL(1),  
    IS_NULL("value"),   
    IS_NULL(null),  
    IS_NULL({prop: "value"}),   
    IS_NULL([1, 2, 3]),  
    IS_NULL({prop: "value"}.prop2)  
```  
  
 <span data-ttu-id="bc057-902">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-902">Here is the result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: true, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="bc057-903"><a name="bk_is_number"></a>IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="bc057-903"><a name="bk_is_number"></a> IS_NUMBER</span></span>  
 <span data-ttu-id="bc057-904">Vrátí logickou hodnotu udávající, pokud typ zadaný výraz je číslo.</span><span class="sxs-lookup"><span data-stu-id="bc057-904">Returns a Boolean value indicating if the type of the specified expression is a number.</span></span>  
  
 <span data-ttu-id="bc057-905">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-905">**Syntax**</span></span>  
  
```  
IS_NUMBER(<expression>)  
```  
  
 <span data-ttu-id="bc057-906">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-906">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="bc057-907">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-907">Is any valid expression.</span></span>  
  
 <span data-ttu-id="bc057-908">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-908">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-909">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-909">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="bc057-910">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-910">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-911">Následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí funkce IS_NULL.</span><span class="sxs-lookup"><span data-stu-id="bc057-911">The following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using the IS_NULL function.</span></span>  
  
```  
SELECT   
    IS_NUMBER(true),   
    IS_NUMBER(1),  
    IS_NUMBER("value"),   
    IS_NUMBER(null),  
    IS_NUMBER({prop: "value"}),   
    IS_NUMBER([1, 2, 3]),  
    IS_NUMBER({prop: "value"}.prop2)  
```  
  
 <span data-ttu-id="bc057-912">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-912">Here is the result set.</span></span>  
  
```  
[{$1: false, $2: true, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="bc057-913"><a name="bk_is_object"></a>IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="bc057-913"><a name="bk_is_object"></a> IS_OBJECT</span></span>  
 <span data-ttu-id="bc057-914">Vrátí logickou hodnotu udávající, pokud typ zadaný výraz je objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="bc057-914">Returns a Boolean value indicating if the type of the specified expression is a JSON object.</span></span>  
  
 <span data-ttu-id="bc057-915">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-915">**Syntax**</span></span>  
  
```  
IS_OBJECT(<expression>)  
```  
  
 <span data-ttu-id="bc057-916">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-916">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="bc057-917">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-917">Is any valid expression.</span></span>  
  
 <span data-ttu-id="bc057-918">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-918">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-919">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-919">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="bc057-920">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-920">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-921">Následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí funkce IS_OBJECT.</span><span class="sxs-lookup"><span data-stu-id="bc057-921">The following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using the IS_OBJECT function.</span></span>  
  
```  
SELECT   
    IS_OBJECT(true),   
    IS_OBJECT(1),  
    IS_OBJECT("value"),   
    IS_OBJECT(null),  
    IS_OBJECT({prop: "value"}),   
    IS_OBJECT([1, 2, 3]),  
    IS_OBJECT({prop: "value"}.prop2)  
```  
  
 <span data-ttu-id="bc057-922">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-922">Here is the result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: true, $6: false}]  
```  
  
####  <span data-ttu-id="bc057-923"><a name="bk_is_primitive"></a>IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="bc057-923"><a name="bk_is_primitive"></a> IS_PRIMITIVE</span></span>  
 <span data-ttu-id="bc057-924">Vrátí logickou hodnotu udávající, pokud není typ zadaného výrazu jednoduchého typu (řetězec, logická hodnota, číselná nebo má hodnotu null).</span><span class="sxs-lookup"><span data-stu-id="bc057-924">Returns a Boolean value indicating if the type of the specified expression is a primitive (string, Boolean, numeric or null).</span></span>  
  
 <span data-ttu-id="bc057-925">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-925">**Syntax**</span></span>  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 <span data-ttu-id="bc057-926">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-926">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="bc057-927">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-927">Is any valid expression.</span></span>  
  
 <span data-ttu-id="bc057-928">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-928">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-929">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-929">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="bc057-930">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-930">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-931">Následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí funkce IS_PRIMITIVE.</span><span class="sxs-lookup"><span data-stu-id="bc057-931">The following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using the IS_PRIMITIVE function.</span></span>  
  
```  
SELECT   
           IS_PRIMITIVE(true),   
           IS_PRIMITIVE(1),  
           IS_PRIMITIVE("value"),   
           IS_PRIMITIVE(null),  
           IS_PRIMITIVE({prop: "value"}),   
           IS_PRIMITIVE([1, 2, 3]),  
           IS_PRIMITIVE({prop: "value"}.prop2)  
```  
  
 <span data-ttu-id="bc057-932">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-932">Here is the result set.</span></span>  
  
```  
[{"$1": true, "$2": true, "$3": true, "$4": true, "$5": false, "$6": false, "$7": false}]  
```  
  
####  <span data-ttu-id="bc057-933"><a name="bk_is_string"></a>IS_STRING</span><span class="sxs-lookup"><span data-stu-id="bc057-933"><a name="bk_is_string"></a> IS_STRING</span></span>  
 <span data-ttu-id="bc057-934">Vrátí logickou hodnotu udávající, pokud typ zadaný výraz obsahuje řetězec.</span><span class="sxs-lookup"><span data-stu-id="bc057-934">Returns a Boolean value indicating if the type of the specified expression is a string.</span></span>  
  
 <span data-ttu-id="bc057-935">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-935">**Syntax**</span></span>  
  
```  
IS_STRING(<expression>)  
```  
  
 <span data-ttu-id="bc057-936">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-936">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="bc057-937">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-937">Is any valid expression.</span></span>  
  
 <span data-ttu-id="bc057-938">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-938">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-939">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-939">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="bc057-940">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-940">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-941">Následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí funkce IS_STRING.</span><span class="sxs-lookup"><span data-stu-id="bc057-941">The following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using the IS_STRING function.</span></span>  
  
```  
SELECT   
       IS_STRING(true),   
       IS_STRING(1),  
       IS_STRING("value"),   
       IS_STRING(null),  
       IS_STRING({prop: "value"}),   
       IS_STRING([1, 2, 3]),  
       IS_STRING({prop: "value"}.prop2)  
```  
  
 <span data-ttu-id="bc057-942">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-942">Here is the result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: true, $4: false, $5: false, $6: false}]  
```  
  
###  <span data-ttu-id="bc057-943"><a name="bk_string_functions"></a>Řetězcové funkce</span><span class="sxs-lookup"><span data-stu-id="bc057-943"><a name="bk_string_functions"></a> String functions</span></span>  
 <span data-ttu-id="bc057-944">Následující skalární funkce provést operaci s vstupní hodnotu řetězce a vrátí řetězec, číselnou nebo logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="bc057-944">The following scalar functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="bc057-945">CONCAT</span><span class="sxs-lookup"><span data-stu-id="bc057-945">CONCAT</span></span>](#bk_concat)|[<span data-ttu-id="bc057-946">OBSAHUJE</span><span class="sxs-lookup"><span data-stu-id="bc057-946">CONTAINS</span></span>](#bk_contains)|[<span data-ttu-id="bc057-947">ENDSWITH</span><span class="sxs-lookup"><span data-stu-id="bc057-947">ENDSWITH</span></span>](#bk_endswith)|  
|[<span data-ttu-id="bc057-948">INDEX_OF</span><span class="sxs-lookup"><span data-stu-id="bc057-948">INDEX_OF</span></span>](#bk_index_of)|[<span data-ttu-id="bc057-949">VLEVO</span><span class="sxs-lookup"><span data-stu-id="bc057-949">LEFT</span></span>](#bk_left)|[<span data-ttu-id="bc057-950">DÉLKA</span><span class="sxs-lookup"><span data-stu-id="bc057-950">LENGTH</span></span>](#bk_length)|  
|[<span data-ttu-id="bc057-951">NIŽŠÍ</span><span class="sxs-lookup"><span data-stu-id="bc057-951">LOWER</span></span>](#bk_lower)|[<span data-ttu-id="bc057-952">LTRIM</span><span class="sxs-lookup"><span data-stu-id="bc057-952">LTRIM</span></span>](#bk_ltrim)|[<span data-ttu-id="bc057-953">NAHRADIT</span><span class="sxs-lookup"><span data-stu-id="bc057-953">REPLACE</span></span>](#bk_replace)|  
|[<span data-ttu-id="bc057-954">REPLIKACE</span><span class="sxs-lookup"><span data-stu-id="bc057-954">REPLICATE</span></span>](#bk_replicate)|[<span data-ttu-id="bc057-955">REVERSE</span><span class="sxs-lookup"><span data-stu-id="bc057-955">REVERSE</span></span>](#bk_reverse)|[<span data-ttu-id="bc057-956">VPRAVO</span><span class="sxs-lookup"><span data-stu-id="bc057-956">RIGHT</span></span>](#bk_right)|  
|[<span data-ttu-id="bc057-957">RTRIM</span><span class="sxs-lookup"><span data-stu-id="bc057-957">RTRIM</span></span>](#bk_rtrim)|[<span data-ttu-id="bc057-958">STARTSWITH</span><span class="sxs-lookup"><span data-stu-id="bc057-958">STARTSWITH</span></span>](#bk_startswith)|[<span data-ttu-id="bc057-959">DÍLČÍ ŘETĚZEC</span><span class="sxs-lookup"><span data-stu-id="bc057-959">SUBSTRING</span></span>](#bk_substring)|  
|[<span data-ttu-id="bc057-960">HORNÍ</span><span class="sxs-lookup"><span data-stu-id="bc057-960">UPPER</span></span>](#bk_upper)|||  
  
####  <span data-ttu-id="bc057-961"><a name="bk_concat"></a>CONCAT</span><span class="sxs-lookup"><span data-stu-id="bc057-961"><a name="bk_concat"></a> CONCAT</span></span>  
 <span data-ttu-id="bc057-962">Vrátí řetězec, který je výsledkem zřetězení dvou nebo více řetězcové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bc057-962">Returns a string that is the result of concatenating two or more string values.</span></span>  
  
 <span data-ttu-id="bc057-963">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-963">**Syntax**</span></span>  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 <span data-ttu-id="bc057-964">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-964">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-965">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-965">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="bc057-966">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-966">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-967">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-967">Returns a string expression.</span></span>  
  
 <span data-ttu-id="bc057-968">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-968">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-969">Následující příklad vrací spojený řetězec ze zadaných hodnot.</span><span class="sxs-lookup"><span data-stu-id="bc057-969">The following example returns the concatenated string of the specified values.</span></span>  
  
```  
SELECT CONCAT("abc", "def")  
```  
  
 <span data-ttu-id="bc057-970">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-970">Here is the result set.</span></span>  
  
```  
[{"$1": "abcdef"}  
```  
  
####  <span data-ttu-id="bc057-971"><a name="bk_contains"></a>OBSAHUJE</span><span class="sxs-lookup"><span data-stu-id="bc057-971"><a name="bk_contains"></a> CONTAINS</span></span>  
 <span data-ttu-id="bc057-972">Vrátí logická hodnota, která určuje zda první řetězec výraz obsahuje druhý.</span><span class="sxs-lookup"><span data-stu-id="bc057-972">Returns a Boolean indicating whether the first string expression contains the second.</span></span>  
  
 <span data-ttu-id="bc057-973">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-973">**Syntax**</span></span>  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="bc057-974">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-974">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-975">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-975">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="bc057-976">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-976">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-977">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-977">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="bc057-978">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-978">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-979">Následující příklad zkontroluje Pokud "abc" "ab" a "d" obsahuje.</span><span class="sxs-lookup"><span data-stu-id="bc057-979">The following example checks if "abc" contains "ab" and contains "d".</span></span>  
  
```  
SELECT CONTAINS("abc", "ab"), CONTAINS("abc", "d")  
```  
  
 <span data-ttu-id="bc057-980">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-980">Here is the result set.</span></span>  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <span data-ttu-id="bc057-981"><a name="bk_endswith"></a>ENDSWITH</span><span class="sxs-lookup"><span data-stu-id="bc057-981"><a name="bk_endswith"></a> ENDSWITH</span></span>  
 <span data-ttu-id="bc057-982">Vrátí logická hodnota, která určuje zda první řetězec výraz končí druhý.</span><span class="sxs-lookup"><span data-stu-id="bc057-982">Returns a Boolean indicating whether the first string expression ends with the second.</span></span>  
  
 <span data-ttu-id="bc057-983">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-983">**Syntax**</span></span>  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="bc057-984">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-984">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-985">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-985">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="bc057-986">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-986">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-987">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-987">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="bc057-988">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-988">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-989">Následující příklad vrátí "abc" končí textem "b" a "bc".</span><span class="sxs-lookup"><span data-stu-id="bc057-989">The following example returns the "abc" ends with "b" and "bc".</span></span>  
  
```  
SELECT ENDSWITH("abc", "b"), ENDSWITH("abc", "bc")  
```  
  
 <span data-ttu-id="bc057-990">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-990">Here is the result set.</span></span>  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <span data-ttu-id="bc057-991"><a name="bk_index_of"></a>INDEX_OF</span><span class="sxs-lookup"><span data-stu-id="bc057-991"><a name="bk_index_of"></a> INDEX_OF</span></span>  
 <span data-ttu-id="bc057-992">Vrátí počáteční pozici prvního výskytu druhý řetězec výrazu v rámci první zadaného řetězcového výrazu nebo -1, pokud není nalezen řetězec.</span><span class="sxs-lookup"><span data-stu-id="bc057-992">Returns the starting position of the first occurrence of the second string expression within the first specified string expression, or -1 if the string is not found.</span></span>  
  
 <span data-ttu-id="bc057-993">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-993">**Syntax**</span></span>  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="bc057-994">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-994">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-995">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-995">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="bc057-996">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-996">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-997">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-997">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-998">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-998">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-999">Následující příklad vrátí index různé dílčích řetězců uvnitř "abc".</span><span class="sxs-lookup"><span data-stu-id="bc057-999">The following example returns the index of various substrings inside "abc".</span></span>  
  
```  
SELECT INDEX_OF("abc", "ab"), INDEX_OF("abc", "b"), INDEX_OF("abc", "c")  
```  
  
 <span data-ttu-id="bc057-1000">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1000">Here is the result set.</span></span>  
  
```  
[{"$1": 0, "$2": 1, "$3": -1}]  
```  
  
####  <span data-ttu-id="bc057-1001"><a name="bk_left"></a>VLEVO</span><span class="sxs-lookup"><span data-stu-id="bc057-1001"><a name="bk_left"></a> LEFT</span></span>  
 <span data-ttu-id="bc057-1002">Vrátí levé části řetězec s zadaný počet znaků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1002">Returns the left part of a string with the specified number of characters.</span></span>  
  
 <span data-ttu-id="bc057-1003">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1003">**Syntax**</span></span>  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="bc057-1004">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1004">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-1005">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1005">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="bc057-1006">Je libovolný platný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1006">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-1007">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1007">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1008">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1008">Returns a string expression.</span></span>  
  
 <span data-ttu-id="bc057-1009">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1009">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1010">Následující příklad vrátí levé části "abc" pro různé hodnoty pro délku.</span><span class="sxs-lookup"><span data-stu-id="bc057-1010">The following example returns the left part of "abc" for various length values.</span></span>  
  
```  
SELECT LEFT("abc", 1), LEFT("abc", 2)  
```  
  
 <span data-ttu-id="bc057-1011">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1011">Here is the result set.</span></span>  
  
```  
[{"$1": "ab", "$2": "ab"}]  
```  
  
####  <span data-ttu-id="bc057-1012"><a name="bk_length"></a>DÉLKA</span><span class="sxs-lookup"><span data-stu-id="bc057-1012"><a name="bk_length"></a> LENGTH</span></span>  
 <span data-ttu-id="bc057-1013">Vrátí počet znaků ze zadaného řetězcového výrazu.</span><span class="sxs-lookup"><span data-stu-id="bc057-1013">Returns the number of characters of the specified string expression.</span></span>  
  
 <span data-ttu-id="bc057-1014">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1014">**Syntax**</span></span>  
  
```  
LENGTH(<str_expr>)  
```  
  
 <span data-ttu-id="bc057-1015">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1015">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-1016">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1016">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="bc057-1017">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1017">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1018">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1018">Returns a string expression.</span></span>  
  
 <span data-ttu-id="bc057-1019">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1019">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1020">Následující příklad vrátí délku řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1020">The following example returns the length of a string.</span></span>  
  
```  
SELECT LENGTH("abc")  
```  
  
 <span data-ttu-id="bc057-1021">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1021">Here is the result set.</span></span>  
  
```  
[{"$1": 3}]  
```  
  
####  <span data-ttu-id="bc057-1022"><a name="bk_lower"></a>NIŽŠÍ</span><span class="sxs-lookup"><span data-stu-id="bc057-1022"><a name="bk_lower"></a> LOWER</span></span>  
 <span data-ttu-id="bc057-1023">Vrací výraz řetězce po převodu dat velké písmeno na malá písmena.</span><span class="sxs-lookup"><span data-stu-id="bc057-1023">Returns a string expression after converting uppercase character data to lowercase.</span></span>  
  
 <span data-ttu-id="bc057-1024">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1024">**Syntax**</span></span>  
  
```  
LOWER(<str_expr>)  
```  
  
 <span data-ttu-id="bc057-1025">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1025">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-1026">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1026">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="bc057-1027">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1027">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1028">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1028">Returns a string expression.</span></span>  
  
 <span data-ttu-id="bc057-1029">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1029">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1030">Následující příklad ukazuje, jak používat nižší v dotazu.</span><span class="sxs-lookup"><span data-stu-id="bc057-1030">The following example shows how to use LOWER in a query.</span></span>  
  
```  
SELECT LOWER("Abc")  
```  
  
 <span data-ttu-id="bc057-1031">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1031">Here is the result set.</span></span>  
  
```  
[{"$1": "abc"}]  
  
```  
  
####  <span data-ttu-id="bc057-1032"><a name="bk_ltrim"></a>LTRIM</span><span class="sxs-lookup"><span data-stu-id="bc057-1032"><a name="bk_ltrim"></a> LTRIM</span></span>  
 <span data-ttu-id="bc057-1033">Vrací výraz řetězce po ho odebere úvodní mezery.</span><span class="sxs-lookup"><span data-stu-id="bc057-1033">Returns a string expression after it removes leading blanks.</span></span>  
  
 <span data-ttu-id="bc057-1034">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1034">**Syntax**</span></span>  
  
```  
LTRIM(<str_expr>)  
```  
  
 <span data-ttu-id="bc057-1035">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1035">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-1036">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1036">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="bc057-1037">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1037">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1038">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1038">Returns a string expression.</span></span>  
  
 <span data-ttu-id="bc057-1039">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1039">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1040">Následující příklad ukazuje, jak používat LTRIM uvnitř dotazu.</span><span class="sxs-lookup"><span data-stu-id="bc057-1040">The following example shows how to use LTRIM inside a query.</span></span>  
  
```  
SELECT LTRIM("  abc"), LTRIM("abc"), LTRIM("abc   ")  
```  
  
 <span data-ttu-id="bc057-1041">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1041">Here is the result set.</span></span>  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc   "}]  
```  
  
####  <span data-ttu-id="bc057-1042"><a name="bk_replace"></a>NAHRADIT</span><span class="sxs-lookup"><span data-stu-id="bc057-1042"><a name="bk_replace"></a> REPLACE</span></span>  
 <span data-ttu-id="bc057-1043">Nahradí všechny výskyty zadaná řetězcová hodnota s jinou hodnotou řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1043">Replaces all occurrences of a specified string value with another string value.</span></span>  
  
 <span data-ttu-id="bc057-1044">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1044">**Syntax**</span></span>  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="bc057-1045">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1045">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-1046">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1046">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="bc057-1047">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1047">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1048">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1048">Returns a string expression.</span></span>  
  
 <span data-ttu-id="bc057-1049">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1049">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1050">Následující příklad ukazuje, jak se používá v dotazu.</span><span class="sxs-lookup"><span data-stu-id="bc057-1050">The following example shows how to use REPLACE in a query.</span></span>  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk")  
```  
  
 <span data-ttu-id="bc057-1051">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1051">Here is the result set.</span></span>  
  
```  
[{"$1": "This is a desk"}]  
```  
  
####  <span data-ttu-id="bc057-1052"><a name="bk_replicate"></a>REPLIKACE</span><span class="sxs-lookup"><span data-stu-id="bc057-1052"><a name="bk_replicate"></a> REPLICATE</span></span>  
 <span data-ttu-id="bc057-1053">Opakuje hodnotu řetězce zadaného počtu opakování.</span><span class="sxs-lookup"><span data-stu-id="bc057-1053">Repeats a string value a specified number of times.</span></span>  
  
 <span data-ttu-id="bc057-1054">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1054">**Syntax**</span></span>  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="bc057-1055">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1055">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-1056">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1056">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="bc057-1057">Je libovolný platný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1057">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-1058">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1058">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1059">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1059">Returns a string expression.</span></span>  
  
 <span data-ttu-id="bc057-1060">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1060">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1061">Následující příklad ukazuje, jak používat REPLIKACE v dotazu.</span><span class="sxs-lookup"><span data-stu-id="bc057-1061">The following example shows how to use REPLICATE in a query.</span></span>  
  
```  
SELECT REPLICATE("a", 3)  
```  
  
 <span data-ttu-id="bc057-1062">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1062">Here is the result set.</span></span>  
  
```  
[{"$1": "aaa"}]  
```  
  
####  <span data-ttu-id="bc057-1063"><a name="bk_reverse"></a>REVERSE</span><span class="sxs-lookup"><span data-stu-id="bc057-1063"><a name="bk_reverse"></a> REVERSE</span></span>  
 <span data-ttu-id="bc057-1064">Vrátí obráceném pořadí řetězcovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-1064">Returns the reverse order of a string value.</span></span>  
  
 <span data-ttu-id="bc057-1065">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1065">**Syntax**</span></span>  
  
```  
REVERSE(<str_expr>)  
```  
  
 <span data-ttu-id="bc057-1066">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1066">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-1067">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1067">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="bc057-1068">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1068">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1069">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1069">Returns a string expression.</span></span>  
  
 <span data-ttu-id="bc057-1070">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1070">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1071">Následující příklad ukazuje, jak používat zpětného v dotazu.</span><span class="sxs-lookup"><span data-stu-id="bc057-1071">The following example shows how to use REVERSE in a query.</span></span>  
  
```  
SELECT REVERSE("Abc")  
```  
  
 <span data-ttu-id="bc057-1072">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1072">Here is the result set.</span></span>  
  
```  
[{"$1": "cbA"}]  
```  
  
####  <span data-ttu-id="bc057-1073"><a name="bk_right"></a>VPRAVO</span><span class="sxs-lookup"><span data-stu-id="bc057-1073"><a name="bk_right"></a> RIGHT</span></span>  
 <span data-ttu-id="bc057-1074">Vrátí pravou část řetězec s zadaný počet znaků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1074">Returns the right part of a string with the specified number of characters.</span></span>  
  
 <span data-ttu-id="bc057-1075">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1075">**Syntax**</span></span>  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="bc057-1076">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1076">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-1077">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1077">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="bc057-1078">Je libovolný platný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1078">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-1079">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1079">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1080">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1080">Returns a string expression.</span></span>  
  
 <span data-ttu-id="bc057-1081">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1081">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1082">Následující příklad vrátí pravou část "abc" pro různé hodnoty pro délku.</span><span class="sxs-lookup"><span data-stu-id="bc057-1082">The following example returns the right part of "abc" for various length values.</span></span>  
  
```  
SELECT RIGHT("abc", 1), RIGHT("abc", 2)  
```  
  
 <span data-ttu-id="bc057-1083">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1083">Here is the result set.</span></span>  
  
```  
[{"$1": "c", "$2": "bc"}]  
```  
  
####  <span data-ttu-id="bc057-1084"><a name="bk_rtrim"></a>RTRIM</span><span class="sxs-lookup"><span data-stu-id="bc057-1084"><a name="bk_rtrim"></a> RTRIM</span></span>  
 <span data-ttu-id="bc057-1085">Vrací výraz řetězce po odebere koncové mezery.</span><span class="sxs-lookup"><span data-stu-id="bc057-1085">Returns a string expression after it removes trailing blanks.</span></span>  
  
 <span data-ttu-id="bc057-1086">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1086">**Syntax**</span></span>  
  
```  
RTRIM(<str_expr>)  
```  
  
 <span data-ttu-id="bc057-1087">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1087">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-1088">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1088">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="bc057-1089">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1089">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1090">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1090">Returns a string expression.</span></span>  
  
 <span data-ttu-id="bc057-1091">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1091">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1092">Následující příklad ukazuje, jak používat RTRIM uvnitř dotazu.</span><span class="sxs-lookup"><span data-stu-id="bc057-1092">The following example shows how to use RTRIM inside a query.</span></span>  
  
```  
SELECT RTRIM("  abc"), RTRIM("abc"), RTRIM("abc   ")  
```  
  
 <span data-ttu-id="bc057-1093">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1093">Here is the result set.</span></span>  
  
```  
[{"$1": "   abc", "$2": "abc", "$3": "abc"}]  
```  
  
####  <span data-ttu-id="bc057-1094"><a name="bk_startswith"></a>STARTSWITH</span><span class="sxs-lookup"><span data-stu-id="bc057-1094"><a name="bk_startswith"></a> STARTSWITH</span></span>  
 <span data-ttu-id="bc057-1095">Vrátí logická hodnota, která určuje zda první řetězec výraz začíná druhý.</span><span class="sxs-lookup"><span data-stu-id="bc057-1095">Returns a Boolean indicating whether the first string expression starts with the second.</span></span>  
  
 <span data-ttu-id="bc057-1096">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1096">**Syntax**</span></span>  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="bc057-1097">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1097">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-1098">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1098">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="bc057-1099">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1099">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1100">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1100">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="bc057-1101">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1101">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1102">Následující příklad zkontroluje, zda řetězec "abc" začíná "b" a "a".</span><span class="sxs-lookup"><span data-stu-id="bc057-1102">The following example checks if the string "abc" begins with "b" and "a".</span></span>  
  
```  
SELECT STARTSWITH("abc", "b"), STARTSWITH("abc", "a")  
```  
  
 <span data-ttu-id="bc057-1103">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1103">Here is the result set.</span></span>  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <span data-ttu-id="bc057-1104"><a name="bk_substring"></a>DÍLČÍ ŘETĚZEC</span><span class="sxs-lookup"><span data-stu-id="bc057-1104"><a name="bk_substring"></a> SUBSTRING</span></span>  
 <span data-ttu-id="bc057-1105">Vrátí část řetězcového výrazu od pozice s nulovým základem zadaný znak a pokračuje na určenou délku nebo na konci řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1105">Returns part of a string expression starting at the specified character zero-based position and continues to the specified length, or to the end of the string.</span></span>  
  
 <span data-ttu-id="bc057-1106">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1106">**Syntax**</span></span>  
  
```  
SUBSTRING(<str_expr>, <num_expr> [, <num_expr>])  
```  
  
 <span data-ttu-id="bc057-1107">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1107">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-1108">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1108">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="bc057-1109">Je libovolný platný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1109">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-1110">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1110">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1111">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1111">Returns a string expression.</span></span>  
  
 <span data-ttu-id="bc057-1112">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1112">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1113">Následující příklad vrátí dílčí řetězec "abc" spouštění v 1 a o délce 1 znak.</span><span class="sxs-lookup"><span data-stu-id="bc057-1113">The following example returns the substring of "abc" starting at 1 and for a length of 1 character.</span></span>  
  
```  
SELECT SUBSTRING("abc", 1, 1)  
```  
  
 <span data-ttu-id="bc057-1114">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1114">Here is the result set.</span></span>  
  
```  
[{"$1": "b"}]  
```  
  
####  <span data-ttu-id="bc057-1115"><a name="bk_upper"></a>HORNÍ</span><span class="sxs-lookup"><span data-stu-id="bc057-1115"><a name="bk_upper"></a> UPPER</span></span>  
 <span data-ttu-id="bc057-1116">Vrací výraz řetězce po převodu dat malé písmeno na velká písmena.</span><span class="sxs-lookup"><span data-stu-id="bc057-1116">Returns a string expression after converting lowercase character data to uppercase.</span></span>  
  
 <span data-ttu-id="bc057-1117">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1117">**Syntax**</span></span>  
  
```  
UPPER(<str_expr>)  
```  
  
 <span data-ttu-id="bc057-1118">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1118">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="bc057-1119">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1119">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="bc057-1120">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1120">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1121">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1121">Returns a string expression.</span></span>  
  
 <span data-ttu-id="bc057-1122">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1122">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1123">Následující příklad ukazuje, jak používat horní v dotazu</span><span class="sxs-lookup"><span data-stu-id="bc057-1123">The following example shows how to use UPPER in a query</span></span>  
  
```  
SELECT UPPER("Abc")  
```  
  
 <span data-ttu-id="bc057-1124">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1124">Here is the result set.</span></span>  
  
```  
[{"$1": "ABC"}]  
```  
  
###  <span data-ttu-id="bc057-1125"><a name="bk_array_functions"></a>Funkce pole</span><span class="sxs-lookup"><span data-stu-id="bc057-1125"><a name="bk_array_functions"></a> Array functions</span></span>  
 <span data-ttu-id="bc057-1126">Provedení operace hodnota vstupní pole a vrátí číselnou, logická hodnota nebo pole hodnota následující skalární funkce</span><span class="sxs-lookup"><span data-stu-id="bc057-1126">The following scalar functions perform an operation on an array input value and return numeric, Boolean or array value</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="bc057-1127">ARRAY_CONCAT</span><span class="sxs-lookup"><span data-stu-id="bc057-1127">ARRAY_CONCAT</span></span>](#bk_array_concat)|[<span data-ttu-id="bc057-1128">ARRAY_CONTAINS</span><span class="sxs-lookup"><span data-stu-id="bc057-1128">ARRAY_CONTAINS</span></span>](#bk_array_contains)|[<span data-ttu-id="bc057-1129">ARRAY_LENGTH</span><span class="sxs-lookup"><span data-stu-id="bc057-1129">ARRAY_LENGTH</span></span>](#bk_array_length)|  
|[<span data-ttu-id="bc057-1130">ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="bc057-1130">ARRAY_SLICE</span></span>](#bk_array_slice)|||  
  
####  <span data-ttu-id="bc057-1131"><a name="bk_array_concat"></a>ARRAY_CONCAT</span><span class="sxs-lookup"><span data-stu-id="bc057-1131"><a name="bk_array_concat"></a> ARRAY_CONCAT</span></span>  
 <span data-ttu-id="bc057-1132">Vrátí pole, které je výsledkem zřetězení dvě nebo více hodnot pole.</span><span class="sxs-lookup"><span data-stu-id="bc057-1132">Returns an array that is the result of concatenating two or more array values.</span></span>  
  
 <span data-ttu-id="bc057-1133">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1133">**Syntax**</span></span>  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 <span data-ttu-id="bc057-1134">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1134">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="bc057-1135">Je jakýkoli platný pole výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1135">Is any valid array expression.</span></span>  
  
 <span data-ttu-id="bc057-1136">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1136">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1137">Vrací výraz pole.</span><span class="sxs-lookup"><span data-stu-id="bc057-1137">Returns an array expression.</span></span>  
  
 <span data-ttu-id="bc057-1138">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1138">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1139">Následující příklad postup řetězení dvěma poli.</span><span class="sxs-lookup"><span data-stu-id="bc057-1139">The following example how to concatenate two arrays.</span></span>  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"])  
```  
  
 <span data-ttu-id="bc057-1140">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1140">Here is the result set.</span></span>  
  
```  
[{"$1": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <span data-ttu-id="bc057-1141"><a name="bk_array_contains"></a>ARRAY_CONTAINS</span><span class="sxs-lookup"><span data-stu-id="bc057-1141"><a name="bk_array_contains"></a> ARRAY_CONTAINS</span></span>  
 <span data-ttu-id="bc057-1142">Vrátí logickou hodnotu udávající, zda pole obsahuje zadanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-1142">Returns a Boolean indicating whether the array contains the specified value.</span></span>  
  
 <span data-ttu-id="bc057-1143">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1143">**Syntax**</span></span>  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr>)  
```  
  
 <span data-ttu-id="bc057-1144">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1144">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="bc057-1145">Je jakýkoli platný pole výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1145">Is any valid array expression.</span></span>  
  
-   `expr`  
  
     <span data-ttu-id="bc057-1146">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1146">Is any valid expression.</span></span>  
  
 <span data-ttu-id="bc057-1147">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1147">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1148">Vrátí logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-1148">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="bc057-1149">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1149">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1150">Následující příklad postup kontroly členství v poli pomocí ARRAY_CONTAINS.</span><span class="sxs-lookup"><span data-stu-id="bc057-1150">The following example how to check for membership in an array using ARRAY_CONTAINS.</span></span>  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples"),  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes")  
```  
  
 <span data-ttu-id="bc057-1151">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1151">Here is the result set.</span></span>  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <span data-ttu-id="bc057-1152"><a name="bk_array_length"></a>ARRAY_LENGTH</span><span class="sxs-lookup"><span data-stu-id="bc057-1152"><a name="bk_array_length"></a> ARRAY_LENGTH</span></span>  
 <span data-ttu-id="bc057-1153">Vrátí počet prvků výrazu zadané pole.</span><span class="sxs-lookup"><span data-stu-id="bc057-1153">Returns the number of elements of the specified array expression.</span></span>  
  
 <span data-ttu-id="bc057-1154">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1154">**Syntax**</span></span>  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 <span data-ttu-id="bc057-1155">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1155">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="bc057-1156">Je jakýkoli platný pole výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1156">Is any valid array expression.</span></span>  
  
 <span data-ttu-id="bc057-1157">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1157">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1158">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1158">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-1159">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1159">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1160">Následující příklad jak získat délka pole pomocí ARRAY_LENGTH.</span><span class="sxs-lookup"><span data-stu-id="bc057-1160">The following example how to get the length of an array using ARRAY_LENGTH.</span></span>  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"])  
```  
  
 <span data-ttu-id="bc057-1161">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1161">Here is the result set.</span></span>  
  
```  
[{"$1": 3}]  
```  
  
####  <span data-ttu-id="bc057-1162"><a name="bk_array_slice"></a>ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="bc057-1162"><a name="bk_array_slice"></a> ARRAY_SLICE</span></span>  
 <span data-ttu-id="bc057-1163">Vrátí část výraz pole.</span><span class="sxs-lookup"><span data-stu-id="bc057-1163">Returns part of an array expression.</span></span>
  
 <span data-ttu-id="bc057-1164">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1164">**Syntax**</span></span>  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 <span data-ttu-id="bc057-1165">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1165">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="bc057-1166">Je jakýkoli platný pole výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1166">Is any valid array expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="bc057-1167">Je libovolný platný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1167">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="bc057-1168">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1168">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1169">Vrátí logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-1169">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="bc057-1170">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1170">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1171">Následující příklad jak získat součástí pomocí ARRAY_SLICE pole.</span><span class="sxs-lookup"><span data-stu-id="bc057-1171">The following example how to get a part of an array using ARRAY_SLICE.</span></span>  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1),  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1)  
```  
  
 <span data-ttu-id="bc057-1172">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1172">Here is the result set.</span></span>  
  
```  
[{  
           "$1": ["strawberries", "bananas"],   
           "$2": ["strawberries"]  
       }]  
```  
  
###  <span data-ttu-id="bc057-1173"><a name="bk_spatial_functions"></a>Prostorové funkce</span><span class="sxs-lookup"><span data-stu-id="bc057-1173"><a name="bk_spatial_functions"></a> Spatial functions</span></span>  
 <span data-ttu-id="bc057-1174">Následující skalární funkce provádění operací na prostorový objekt vstupní hodnotu a vrátí číslo nebo logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="bc057-1174">The following scalar functions perform an operation on an spatial object input value and return a numeric or Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="bc057-1175">ST_DISTANCE</span><span class="sxs-lookup"><span data-stu-id="bc057-1175">ST_DISTANCE</span></span>](#bk_st_distance)|[<span data-ttu-id="bc057-1176">ST_WITHIN</span><span class="sxs-lookup"><span data-stu-id="bc057-1176">ST_WITHIN</span></span>](#bk_st_within)|[<span data-ttu-id="bc057-1177">ST_INTERSECTS</span><span class="sxs-lookup"><span data-stu-id="bc057-1177">ST_INTERSECTS</span></span>](#bk_st_intersects)|[<span data-ttu-id="bc057-1178">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="bc057-1178">ST_ISVALID</span></span>](#bk_st_isvalid)|  
|[<span data-ttu-id="bc057-1179">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="bc057-1179">ST_ISVALIDDETAILED</span></span>](#bk_st_isvaliddetailed)|||  
  
####  <span data-ttu-id="bc057-1180"><a name="bk_st_distance"></a>ST_DISTANCE</span><span class="sxs-lookup"><span data-stu-id="bc057-1180"><a name="bk_st_distance"></a> ST_DISTANCE</span></span>  
 <span data-ttu-id="bc057-1181">Vrací vzdálenost mezi dvěma GeoJSON bodu, mnohoúhelníku nebo LineString výrazy.</span><span class="sxs-lookup"><span data-stu-id="bc057-1181">Returns the distance between the two GeoJSON Point, Polygon, or LineString expressions.</span></span>  
  
 <span data-ttu-id="bc057-1182">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1182">**Syntax**</span></span>  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="bc057-1183">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1183">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="bc057-1184">Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.</span><span class="sxs-lookup"><span data-stu-id="bc057-1184">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="bc057-1185">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1185">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1186">Vrátí číselný výraz obsahující vzdálenost.</span><span class="sxs-lookup"><span data-stu-id="bc057-1186">Returns a numeric expression containing the distance.</span></span> <span data-ttu-id="bc057-1187">Vyjadřuje se v měřidla pro výchozí odkaz na systém.</span><span class="sxs-lookup"><span data-stu-id="bc057-1187">This is expressed in meters for the default reference system.</span></span>  
  
 <span data-ttu-id="bc057-1188">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1188">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1189">Následující příklad ukazuje, jak vrátit všechny rodiny dokumenty, které jsou v rámci 30 km v zadaném umístění pomocí předdefinované funkci ST_DISTANCE.</span><span class="sxs-lookup"><span data-stu-id="bc057-1189">The following example shows how to return all family documents that are within 30 km of the specified location using the ST_DISTANCE built-in function.</span></span> <span data-ttu-id="bc057-1190">.</span><span class="sxs-lookup"><span data-stu-id="bc057-1190">.</span></span>  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 <span data-ttu-id="bc057-1191">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1191">Here is the result set.</span></span>  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <span data-ttu-id="bc057-1192"><a name="bk_st_within"></a>ST_WITHIN</span><span class="sxs-lookup"><span data-stu-id="bc057-1192"><a name="bk_st_within"></a> ST_WITHIN</span></span>  
 <span data-ttu-id="bc057-1193">Vrátí hodnotu určující, zda je v prvním argumentu zadaného objektu GeoJSON (bod, mnohoúhelníku nebo LineString) v rámci GeoJSON (bod, mnohoúhelníku nebo LineString) v druhý argument logický výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1193">Returns a Boolean expression indicating whether the GeoJSON object (Point, Polygon, or LineString) specified in the first argument is within the GeoJSON (Point, Polygon, or LineString) in the second argument.</span></span>  
  
 <span data-ttu-id="bc057-1194">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1194">**Syntax**</span></span>  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="bc057-1195">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1195">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="bc057-1196">Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.</span><span class="sxs-lookup"><span data-stu-id="bc057-1196">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
 
-   `spatial_expr`  
  
     <span data-ttu-id="bc057-1197">Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.</span><span class="sxs-lookup"><span data-stu-id="bc057-1197">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="bc057-1198">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1198">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1199">Vrátí logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-1199">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="bc057-1200">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1200">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1201">Následující příklad ukazuje, jak najít všechny rodiny dokumenty pomocí ST_WITHIN mnohoúhelníku.</span><span class="sxs-lookup"><span data-stu-id="bc057-1201">The following example shows how to find all family documents within a polygon using ST_WITHIN.</span></span>  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 <span data-ttu-id="bc057-1202">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1202">Here is the result set.</span></span>  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <span data-ttu-id="bc057-1203"><a name="bk_st_intersects"></a>ST_INTERSECTS</span><span class="sxs-lookup"><span data-stu-id="bc057-1203"><a name="bk_st_intersects"></a> ST_INTERSECTS</span></span>  
 <span data-ttu-id="bc057-1204">Vrátí hodnotu označující, zda zadaného v prvním argumentu objektu GeoJSON (bod, mnohoúhelníku nebo LineString) protíná GeoJSON (bod, mnohoúhelníku nebo LineString) v druhý argument logický výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1204">Returns a Boolean expression indicating whether the GeoJSON object (Point, Polygon, or LineString) specified in the first argument intersects the GeoJSON (Point, Polygon, or LineString) in the second argument.</span></span>  
  
 <span data-ttu-id="bc057-1205">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1205">**Syntax**</span></span>  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="bc057-1206">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1206">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="bc057-1207">Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.</span><span class="sxs-lookup"><span data-stu-id="bc057-1207">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
 
-   `spatial_expr`  
  
     <span data-ttu-id="bc057-1208">Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.</span><span class="sxs-lookup"><span data-stu-id="bc057-1208">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="bc057-1209">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1209">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1210">Vrátí logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc057-1210">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="bc057-1211">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1211">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1212">Následující příklad ukazuje, jak najít všechny oblasti, která protíná s danou mnohoúhelníku.</span><span class="sxs-lookup"><span data-stu-id="bc057-1212">The following example shows how to find all areas that intersects with the given polygon.</span></span>  
  
```  
SELECT a.id   
FROM Areas a   
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 <span data-ttu-id="bc057-1213">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1213">Here is the result set.</span></span>  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <span data-ttu-id="bc057-1214"><a name="bk_st_isvalid"></a>ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="bc057-1214"><a name="bk_st_isvalid"></a> ST_ISVALID</span></span>  
 <span data-ttu-id="bc057-1215">Vrátí logickou hodnotu udávající, zda je zadaný výraz GeoJSON bodu, mnohoúhelníku nebo LineString platný.</span><span class="sxs-lookup"><span data-stu-id="bc057-1215">Returns a Boolean value indicating whether the specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span>  
  
 <span data-ttu-id="bc057-1216">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1216">**Syntax**</span></span>  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 <span data-ttu-id="bc057-1217">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1217">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="bc057-1218">Je platný výraz GeoJSON bodu, mnohoúhelníku nebo LineString.</span><span class="sxs-lookup"><span data-stu-id="bc057-1218">Is any valid GeoJSON Point, Polygon, or LineString expression.</span></span>  
  
 <span data-ttu-id="bc057-1219">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1219">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1220">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="bc057-1220">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="bc057-1221">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1221">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1222">Následující příklad ukazuje, jak zkontrolovat, jestli je platný, pomocí ST_VALID bod.</span><span class="sxs-lookup"><span data-stu-id="bc057-1222">The following example shows how to check if a point is valid using ST_VALID.</span></span>  
  
 <span data-ttu-id="bc057-1223">Například tohoto bodu má hodnotu zeměpisnou šířku, která není v platném rozsahu hodnot [-90, 90], takže dotaz vrátí hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="bc057-1223">For example, this point has a latitude value that's not in the valid range of values [-90, 90], so the query returns false.</span></span>  
  
 <span data-ttu-id="bc057-1224">Pro mnohoúhelníky specifikace GeoJSON vyžaduje, aby poslední souřadnic pár zadat stejný jako první, chcete-li vytvořit uzavřený obrazec.</span><span class="sxs-lookup"><span data-stu-id="bc057-1224">For polygons, the GeoJSON specification requires that the last coordinate pair provided should be the same as the first, to create a closed shape.</span></span> <span data-ttu-id="bc057-1225">Je třeba zadat body v rámci mnohoúhelníku v pořadí proti směru hodinových ručiček.</span><span class="sxs-lookup"><span data-stu-id="bc057-1225">Points within a polygon must be specified in counter-clockwise order.</span></span> <span data-ttu-id="bc057-1226">Mnohoúhelníku zadaný v po směru hodinových ručiček pořadí představuje inverzní oblasti v něm.</span><span class="sxs-lookup"><span data-stu-id="bc057-1226">A polygon specified in clockwise order represents the inverse of the region within it.</span></span>  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })  
```  
  
 <span data-ttu-id="bc057-1227">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1227">Here is the result set.</span></span>  
  
```  
[{ "$1": false }]  
```  
  
####  <span data-ttu-id="bc057-1228"><a name="bk_st_isvaliddetailed"></a>ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="bc057-1228"><a name="bk_st_isvaliddetailed"></a> ST_ISVALIDDETAILED</span></span>  
 <span data-ttu-id="bc057-1229">Vrátí hodnotu hodnotu JSON obsahující logickou hodnotu, pokud zadaný výraz GeoJSON bodu, mnohoúhelníku nebo LineString je platný a pokud neplatný, dále z důvodu jako hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1229">Returns a JSON value containing a Boolean value if the specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally the reason as a string value.</span></span>  
  
 <span data-ttu-id="bc057-1230">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="bc057-1230">**Syntax**</span></span>  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 <span data-ttu-id="bc057-1231">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="bc057-1231">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="bc057-1232">Je jakýkoli platný výraz bodu nebo mnohoúhelníku GeoJSON.</span><span class="sxs-lookup"><span data-stu-id="bc057-1232">Is any valid GeoJSON point or polygon expression.</span></span>  
  
 <span data-ttu-id="bc057-1233">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="bc057-1233">**Return Types**</span></span>  
  
 <span data-ttu-id="bc057-1234">Vrátí hodnotu hodnotu JSON obsahující logickou hodnotu, pokud zadaný výraz bodu nebo mnohoúhelníku GeoJSON je platný a pokud je neplatný, dále z důvodu jako hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="bc057-1234">Returns a JSON value containing a Boolean value if the specified GeoJSON point or polygon expression is valid, and if invalid, additionally the reason as a string value.</span></span>  
  
 <span data-ttu-id="bc057-1235">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="bc057-1235">**Examples**</span></span>  
  
 <span data-ttu-id="bc057-1236">Následující příklad postup kontroly platnosti (s podrobnostmi) pomocí ST_ISVALIDDETAILED.</span><span class="sxs-lookup"><span data-stu-id="bc057-1236">The following example how to check validity (with details) using ST_ISVALIDDETAILED.</span></span>  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
})  
```  
  
 <span data-ttu-id="bc057-1237">Zde je sada výsledků.</span><span class="sxs-lookup"><span data-stu-id="bc057-1237">Here is the result set.</span></span>  
  
```  
[{  
  "$1": {   
    "valid": false,   
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  
  
## <a name="next-steps"></a><span data-ttu-id="bc057-1238">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bc057-1238">Next steps</span></span>  
 <span data-ttu-id="bc057-1239">[Syntaxe jazyka SQL a dotaz SQL pro Azure Cosmos DB](documentdb-sql-query.md) </span><span class="sxs-lookup"><span data-stu-id="bc057-1239">[SQL syntax and SQL query for Azure Cosmos DB](documentdb-sql-query.md) </span></span>  
 [<span data-ttu-id="bc057-1240">Dokumentace k Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="bc057-1240">Azure Cosmos DB documentation</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/)  
  
  
