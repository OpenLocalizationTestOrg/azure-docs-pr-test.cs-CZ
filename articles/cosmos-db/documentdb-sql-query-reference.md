---
<span data-ttu-id="27ba2-101">Title: aaa "rozhraní API služby Azure Cosmos databáze DocumentDB: syntaxe SQL | Microsoft Docs"Popis: referenční dokumentace pro hello dotazovacího jazyka SQL rozhraní API Azure Cosmos databáze DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="27ba2-101">title: aaa"Azure Cosmos DB DocumentDB API: SQL syntax | Microsoft Docs" description: Reference documentation for hello Azure Cosmos DB DocumentDB API SQL query language.</span></span>
<span data-ttu-id="27ba2-102">služby: cosmos-db Autor: mimig1 správce: jhubbard editor: mimig documentationcenter: "</span><span class="sxs-lookup"><span data-stu-id="27ba2-102">services: cosmos-db author: mimig1 manager: jhubbard editor: mimig documentationcenter: ''</span></span>

<span data-ttu-id="27ba2-103">MS.AssetID: ms.service: ms.workload cosmos-db: datové služby ms.tgt_pltfrm: na ms.devlang: na ms.topic: referenční ms.date: 13/06/2017 ms.author: mimig</span><span class="sxs-lookup"><span data-stu-id="27ba2-103">ms.assetid: ms.service: cosmos-db ms.workload: data-services ms.tgt_pltfrm: na ms.devlang: na ms.topic: reference ms.date: 06/13/2017 ms.author: mimig</span></span>

---

# <a name="azure-cosmos-db-documentdb-api-sql-syntax-reference"></a><span data-ttu-id="27ba2-104">Azure DocumentDB Cosmos DB rozhraní API: Reference syntaxe SQL</span><span class="sxs-lookup"><span data-stu-id="27ba2-104">Azure Cosmos DB DocumentDB API: SQL syntax reference</span></span>

<span data-ttu-id="27ba2-105">Hello rozhraní API služby Azure Cosmos databáze DocumentDB podporuje dotazování dokumentů pomocí známých SQL (Structured Query Language) jako gramatika přes hierarchické dokumenty JSON bez nutnosti explicitního schématu nebo vytváření sekundárních indexů.</span><span class="sxs-lookup"><span data-stu-id="27ba2-105">hello Azure Cosmos DB DocumentDB API supports querying documents using a familiar SQL (Structured Query Language) like grammar over hierarchical JSON documents without requiring explicit schema or creation of secondary indexes.</span></span> <span data-ttu-id="27ba2-106">Toto téma obsahuje referenční dokumentaci k nástroji pro hello dotazovací jazyk DocumentDB SQL rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="27ba2-106">This topic provides reference documentation for hello DocumentDB API SQL query language.</span></span>

<span data-ttu-id="27ba2-107">Návod hello dotazovací jazyk DocumentDB SQL rozhraní API najdete v tématu [dotazy SQL pro rozhraní API služby Azure Cosmos databáze DocumentDB](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="27ba2-107">For a walkthrough of hello DocumentDB API SQL query language, see [SQL queries for Azure Cosmos DB DocumentDB API](documentdb-sql-query.md).</span></span>  
  
<span data-ttu-id="27ba2-108">Také doporučujeme toovisit hello [Query Playground](http://www.documentdb.com/sql/demo) kde mohou zkuste Azure Cosmos DB a spouštění dotazů SQL na naší datové sadě.</span><span class="sxs-lookup"><span data-stu-id="27ba2-108">We also invite you toovisit hello [Query Playground](http://www.documentdb.com/sql/demo) where you can try Azure Cosmos DB and run SQL queries against our dataset.</span></span>  
  
## <a name="select-query"></a><span data-ttu-id="27ba2-109">Vyberte možnost dotazu</span><span class="sxs-lookup"><span data-stu-id="27ba2-109">SELECT query</span></span>  
<span data-ttu-id="27ba2-110">Načte z databáze hello dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="27ba2-110">Retrieves JSON documents from hello database.</span></span> <span data-ttu-id="27ba2-111">Podporuje vyhodnocení výrazu, projekce, filtrování a připojí.</span><span class="sxs-lookup"><span data-stu-id="27ba2-111">Supports expression evaluation, projections, filtering and joins.</span></span>  <span data-ttu-id="27ba2-112">Hello s konvencemi použitými pro popisující příkazů SELECT hello jsou v tabulce v hello části konvence syntaxe.</span><span class="sxs-lookup"><span data-stu-id="27ba2-112">hello conventions used for describing hello SELECT statements are tabulated in hello Syntax conventions section.</span></span>  
  
<span data-ttu-id="27ba2-113">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-113">**Syntax**</span></span>  
  
```
<select_query> ::=  
SELECT <select_specification>   
    [ FROM <from_specification>]   
    [ WHERE <filter_condition> ]  
    [ ORDER BY <sort_specification> ]  
```  
  
 <span data-ttu-id="27ba2-114">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="27ba2-114">**Remarks**</span></span>  
  
 <span data-ttu-id="27ba2-115">Viz následující části Podrobnosti na každý klauzule:</span><span class="sxs-lookup"><span data-stu-id="27ba2-115">See following sections for details on each clause:</span></span>  
  
-   [<span data-ttu-id="27ba2-116">Klauzule SELECT</span><span class="sxs-lookup"><span data-stu-id="27ba2-116">SELECT clause</span></span>](#bk_select_query)  
  
-   [<span data-ttu-id="27ba2-117">FROM – klauzule</span><span class="sxs-lookup"><span data-stu-id="27ba2-117">FROM clause</span></span>](#bk_from_clause)  
  
-   [<span data-ttu-id="27ba2-118">Klauzule WHERE</span><span class="sxs-lookup"><span data-stu-id="27ba2-118">WHERE clause</span></span>](#bk_where_clause)  
  
-   [<span data-ttu-id="27ba2-119">Klauzuli ORDER by</span><span class="sxs-lookup"><span data-stu-id="27ba2-119">ORDER BY clause</span></span>](#bk_orderby_clause)  
  
<span data-ttu-id="27ba2-120">klauzule Hello v příkazu SELECT hello musejí být seřazeny, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="27ba2-120">hello clauses in hello SELECT statement must be ordered as shown above.</span></span> <span data-ttu-id="27ba2-121">Kterákoli z klauzule volitelné hello lze vynechat.</span><span class="sxs-lookup"><span data-stu-id="27ba2-121">Any one of hello optional clauses can be omitted.</span></span> <span data-ttu-id="27ba2-122">Ale pokud volitelné klauzule používají, musí být ve správném pořadí hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-122">But when optional clauses are used, they must appear in hello right order.</span></span>  
  
<span data-ttu-id="27ba2-123">**Logické pořadí zpracování příkazu SELECT hello**</span><span class="sxs-lookup"><span data-stu-id="27ba2-123">**Logical Processing Order of hello SELECT statement**</span></span>  
  
<span data-ttu-id="27ba2-124">Hello pořadí, ve kterém jsou zpracovány klauzule je:</span><span class="sxs-lookup"><span data-stu-id="27ba2-124">hello order in which clauses are processed is:</span></span>  

1.  [<span data-ttu-id="27ba2-125">FROM – klauzule</span><span class="sxs-lookup"><span data-stu-id="27ba2-125">FROM clause</span></span>](#bk_from_clause)  
2.  [<span data-ttu-id="27ba2-126">Klauzule WHERE</span><span class="sxs-lookup"><span data-stu-id="27ba2-126">WHERE clause</span></span>](#bk_where_clause)  
3.  [<span data-ttu-id="27ba2-127">Klauzuli ORDER by</span><span class="sxs-lookup"><span data-stu-id="27ba2-127">ORDER BY clause</span></span>](#bk_orderby_clause)  
4.  [<span data-ttu-id="27ba2-128">Klauzule SELECT</span><span class="sxs-lookup"><span data-stu-id="27ba2-128">SELECT clause</span></span>](#bk_select_query)  

<span data-ttu-id="27ba2-129">Všimněte si, že se to neliší od hello pořadí, ve kterém se zobrazí v syntaxi hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-129">Note that this is different from hello order in which they appear in hello syntax.</span></span> <span data-ttu-id="27ba2-130">řazení Hello je tak, aby všechny nové symboly zavedených v klauzuli zpracovaná viditelné a zda lze použít v klauzulích zpracovat později.</span><span class="sxs-lookup"><span data-stu-id="27ba2-130">hello ordering is such that all new symbols introduced by a processed clause are visible and can be used in clauses processed later.</span></span> <span data-ttu-id="27ba2-131">Pro instanci, jsou dostupné aliasy deklarované v klauzuli FROM tam, kde a klauzule FROM.</span><span class="sxs-lookup"><span data-stu-id="27ba2-131">For instance, aliases declared in a FROM clause are accessible in WHERE and SELECT clauses.</span></span>  

<span data-ttu-id="27ba2-132">**Prázdné znaky a komentáře**</span><span class="sxs-lookup"><span data-stu-id="27ba2-132">**Whitespace characters and comments**</span></span>  

<span data-ttu-id="27ba2-133">Všechny znaky prázdné znaky, které nejsou součástí řetězec v uvozovkách nebo identifikátor v uvozovkách nejsou součástí hello jazyk gramatika a jsou ignorovány během analýzy.</span><span class="sxs-lookup"><span data-stu-id="27ba2-133">All white space characters which are not part of a quoted string or quoted identifier are not part of hello language grammar and are ignored during parsing.</span></span>  

<span data-ttu-id="27ba2-134">Hello dotazovací jazyk podporuje komentáře styl T-SQL jako</span><span class="sxs-lookup"><span data-stu-id="27ba2-134">hello query language supports T-SQL style comments like</span></span>  

-   <span data-ttu-id="27ba2-135">Příkaz jazyka SQL`-- comment text [newline]`</span><span class="sxs-lookup"><span data-stu-id="27ba2-135">SQL Statement `-- comment text [newline]`</span></span>  

<span data-ttu-id="27ba2-136">Při prázdné znaky a komentáře v není k dispozici žádné násobek hello gramatika, musí být použité tooseparate tokeny.</span><span class="sxs-lookup"><span data-stu-id="27ba2-136">While whitespace characters and comments do not have any significance in hello grammar, they must be used tooseparate tokens.</span></span> <span data-ttu-id="27ba2-137">Pro instanci: `-1e5` je jeden číslo tokenu, chvíli`: – 1 e5` token znaménkem minus následuje identifikátor e5 a číslo 1.</span><span class="sxs-lookup"><span data-stu-id="27ba2-137">For instance: `-1e5` is a single number token, while`: – 1 e5` is a minus token followed by number 1 and identifier e5.</span></span>  

##  <span data-ttu-id="27ba2-138"><a name="bk_select_query"></a>Klauzule SELECT</span><span class="sxs-lookup"><span data-stu-id="27ba2-138"><a name="bk_select_query"></a> SELECT clause</span></span>  
<span data-ttu-id="27ba2-139">klauzule Hello v příkazu SELECT hello musejí být seřazeny, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="27ba2-139">hello clauses in hello SELECT statement must be ordered as shown above.</span></span> <span data-ttu-id="27ba2-140">Kterákoli z klauzule volitelné hello lze vynechat.</span><span class="sxs-lookup"><span data-stu-id="27ba2-140">Any one of hello optional clauses can be omitted.</span></span> <span data-ttu-id="27ba2-141">Ale pokud volitelné klauzule používají, musí být ve správném pořadí hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-141">But when optional clauses are used, they must appear in hello right order.</span></span>  

<span data-ttu-id="27ba2-142">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-142">**Syntax**</span></span>  
```  
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | <object_property_list>   
      | VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
 <span data-ttu-id="27ba2-143">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-143">**Arguments**</span></span>  
  
 `<select_specification>`  
  
 <span data-ttu-id="27ba2-144">Nastavit vlastnosti nebo hodnota toobe vybraná pro výsledek hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-144">Properties or value toobe selected for hello result set.</span></span>  
  
 `'*'`  
  
<span data-ttu-id="27ba2-145">Určuje, že mají být načtena hodnota hello beze změn.</span><span class="sxs-lookup"><span data-stu-id="27ba2-145">Specifies that hello value should be retrieved without making any changes.</span></span> <span data-ttu-id="27ba2-146">Konkrétně Pokud hello zpracovat hodnota objektu, budou všechny vlastnosti načteny.</span><span class="sxs-lookup"><span data-stu-id="27ba2-146">Specifically if hello processed value is an object, all properties will be retrieved.</span></span>  
  
 `<object_property_list>`  
  
<span data-ttu-id="27ba2-147">Určuje seznam hello toobe vlastnosti načteny.</span><span class="sxs-lookup"><span data-stu-id="27ba2-147">Specifies hello list of properties toobe retrieved.</span></span> <span data-ttu-id="27ba2-148">Každý vrácená hodnota bude objekt s byly zadány vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-148">Each returned value will be an object with hello properties specified.</span></span>  
  
`VALUE`  
  
<span data-ttu-id="27ba2-149">Určuje, že mají být načtena hodnota JSON hello místo hello kompletní objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="27ba2-149">Specifies that hello JSON value should be retrieved instead of hello complete JSON object.</span></span> <span data-ttu-id="27ba2-150">To, na rozdíl od `<property_list>` nezalamuje hello projektovat hodnoty v objektu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-150">This, unlike `<property_list>` does not wrap hello projected value in an object.</span></span>  
  
`<scalar_expression>`  
  
<span data-ttu-id="27ba2-151">Výraz představující hodnotu toobe hello počítaný.</span><span class="sxs-lookup"><span data-stu-id="27ba2-151">Expression representing hello value toobe computed.</span></span> <span data-ttu-id="27ba2-152">V tématu [skalární výrazy](#bk_scalar_expressions) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="27ba2-152">See [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
<span data-ttu-id="27ba2-153">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="27ba2-153">**Remarks**</span></span>  
  
<span data-ttu-id="27ba2-154">Hello `SELECT *` syntaxe je platný, pokud klauzule FROM deklaruje právě jednu alias.</span><span class="sxs-lookup"><span data-stu-id="27ba2-154">hello `SELECT *` syntax is only valid if FROM clause has declared exactly one alias.</span></span> <span data-ttu-id="27ba2-155">`SELECT *`poskytuje identitu projekce, které mohou být užitečné, pokud není nutná žádná projekce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-155">`SELECT *` provides an identity projection, which can be useful if no projection is needed.</span></span> <span data-ttu-id="27ba2-156">Vyberte * platí, pouze pokud je zadána klauzule FROM a zavedl pouze jeden vstupní zdroj.</span><span class="sxs-lookup"><span data-stu-id="27ba2-156">SELECT * is only valid if FROM clause is specified and introduced only a single input source.</span></span>  
  
<span data-ttu-id="27ba2-157">Všimněte si, že `SELECT <select_list>` a `SELECT *` jsou "syntaktické sugar" a může být případně vyjádřený pomocí jednoduchého příkazů SELECT, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="27ba2-157">Note that `SELECT <select_list>` and `SELECT *` are "syntactic sugar" and can be alternatively expressed by using simple SELECT statements as shown below.</span></span>  
  
1.  `SELECT * FROM ... AS from_alias ...`  
  
     <span data-ttu-id="27ba2-158">je ekvivalentní:</span><span class="sxs-lookup"><span data-stu-id="27ba2-158">is equivalent to:</span></span>  
  
     `SELECT from_alias FROM ... AS from_alias ...`  
  
2.  `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
     <span data-ttu-id="27ba2-159">je ekvivalentní:</span><span class="sxs-lookup"><span data-stu-id="27ba2-159">is equivalent to:</span></span>  
  
     `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
<span data-ttu-id="27ba2-160">**Viz také**</span><span class="sxs-lookup"><span data-stu-id="27ba2-160">**See Also**</span></span>  
  
[<span data-ttu-id="27ba2-161">Skalární výrazy</span><span class="sxs-lookup"><span data-stu-id="27ba2-161">Scalar expressions</span></span>](#bk_scalar_expressions)  
[<span data-ttu-id="27ba2-162">Klauzule SELECT</span><span class="sxs-lookup"><span data-stu-id="27ba2-162">SELECT clause</span></span>](#bk_select_query)  
  
##  <span data-ttu-id="27ba2-163"><a name="bk_from_clause"></a>FROM – klauzule</span><span class="sxs-lookup"><span data-stu-id="27ba2-163"><a name="bk_from_clause"></a> FROM clause</span></span>  
<span data-ttu-id="27ba2-164">Určuje zdroj hello nebo připojený k zdroje.</span><span class="sxs-lookup"><span data-stu-id="27ba2-164">Specifies hello source or joined sources.</span></span> <span data-ttu-id="27ba2-165">klauzule FROM Hello je volitelný.</span><span class="sxs-lookup"><span data-stu-id="27ba2-165">hello FROM clause is optional.</span></span> <span data-ttu-id="27ba2-166">Pokud není zadaný, další klauzule bude proveden stále jako, pokud klauzule FROM zadaný jednotlivý dokument.</span><span class="sxs-lookup"><span data-stu-id="27ba2-166">If not specified, other clauses will still be executed as if FROM clause provided a single document.</span></span>  
  
<span data-ttu-id="27ba2-167">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-167">**Syntax**</span></span>  
  
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
  
<span data-ttu-id="27ba2-168">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-168">**Arguments**</span></span>  
  
`<from_source>`  
  
<span data-ttu-id="27ba2-169">Určuje zdroj dat s nebo bez alias.</span><span class="sxs-lookup"><span data-stu-id="27ba2-169">Specifies a data source, with or without an alias.</span></span> <span data-ttu-id="27ba2-170">Pokud není zadán alias, bude odvodit z hello `<collection_expression>` pomocí následujících pravidel:</span><span class="sxs-lookup"><span data-stu-id="27ba2-170">If alias is not specified, it will be inferred from hello `<collection_expression>` using following rules:</span></span>  
  
-   <span data-ttu-id="27ba2-171">Pokud je výraz hello název_kolekce, bude Název_kolekce použít jako alias.</span><span class="sxs-lookup"><span data-stu-id="27ba2-171">If hello expression is a collection_name, then collection_name will be used as an alias.</span></span>  
  
-   <span data-ttu-id="27ba2-172">Pokud je výraz hello `<collection_expression>`, pak %{Property_Name/ pak %{Property_Name/ se použije jako alias.</span><span class="sxs-lookup"><span data-stu-id="27ba2-172">If hello expression is `<collection_expression>`, then property_name, then property_name will be used as an alias.</span></span> <span data-ttu-id="27ba2-173">Pokud je výraz hello název_kolekce, bude Název_kolekce použít jako alias.</span><span class="sxs-lookup"><span data-stu-id="27ba2-173">If hello expression is a collection_name, then collection_name will be used as an alias.</span></span>  
  
<span data-ttu-id="27ba2-174">STEJNĚ JAKO`input_alias`</span><span class="sxs-lookup"><span data-stu-id="27ba2-174">AS `input_alias`</span></span>  
  
<span data-ttu-id="27ba2-175">Určuje, že hello `input_alias` je sada hodnot vrácených hello základní kolekce výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-175">Specifies that hello `input_alias` is a set of values returned by hello underlying collection expression.</span></span>  
 
<span data-ttu-id="27ba2-176">`input_alias`V</span><span class="sxs-lookup"><span data-stu-id="27ba2-176">`input_alias` IN</span></span>  
  
<span data-ttu-id="27ba2-177">Určuje, že hello `input_alias` by měl představovat hello sadu hodnot získat iterování přes všechny elementy pole každé pole vrácené hello základní kolekce výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-177">Specifies that hello `input_alias` should represent hello set of values obtained by iterating over all array elements of each array returned by hello underlying collection expression.</span></span> <span data-ttu-id="27ba2-178">Libovolná hodnota vrácený základní kolekce výraz, který není pole se ignoruje.</span><span class="sxs-lookup"><span data-stu-id="27ba2-178">Any value returned by underlying collection expression that is not an array is ignored.</span></span>  
  
`<collection_expression>`  
  
<span data-ttu-id="27ba2-179">Určuje, že hello kolekce výraz toobe použité tooretrieve hello dokumenty.</span><span class="sxs-lookup"><span data-stu-id="27ba2-179">Specifies hello collection expression toobe used tooretrieve hello documents.</span></span>  
  
`ROOT`  
  
<span data-ttu-id="27ba2-180">Určuje, že tento dokument mají být načtena z hello výchozím aktuálně připojené kolekce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-180">Specifies that document should be retrieved from hello default, currently connected collection.</span></span>  
  
`collection_name`  
  
<span data-ttu-id="27ba2-181">Určuje, že tento dokument mají být načtena z hello zadané kolekce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-181">Specifies that document should be retrieved from hello provided collection.</span></span> <span data-ttu-id="27ba2-182">Název Hello hello kolekce musí odpovídat názvu hello hello kolekce aktuálně připojené k.</span><span class="sxs-lookup"><span data-stu-id="27ba2-182">hello name of hello collection must match hello name of hello collection currently connected to.</span></span>  
  
`input_alias`  
  
<span data-ttu-id="27ba2-183">Určuje, že dokumentu mají být načtena z hello jiný zdroj definované hello zadaný alias.</span><span class="sxs-lookup"><span data-stu-id="27ba2-183">Specifies that document should be retrieved from hello other source defined by hello provided alias.</span></span>  
  
`<collection_expression> '.' property_`  
  
<span data-ttu-id="27ba2-184">Určuje v tomto dokumentu mají být načtena díky přístupu k hello `property_name` element pole vlastnosti nebo array_index pro všechny dokumenty načíst zadaný výraz kolekce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-184">Specifies that document should be retrieved by accessing hello `property_name` property or array_index array element for all documents retrieved by specified collection expression.</span></span>  
  
`<collection_expression> '[' "property_name" | array_index ']'`  
  
<span data-ttu-id="27ba2-185">Určuje v tomto dokumentu mají být načtena díky přístupu k hello `property_name` element pole vlastnosti nebo array_index pro všechny dokumenty načíst zadaný výraz kolekce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-185">Specifies that document should be retrieved by accessing hello `property_name` property or array_index array element for all documents retrieved by specified collection expression.</span></span>  
  
<span data-ttu-id="27ba2-186">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="27ba2-186">**Remarks**</span></span>  
  
<span data-ttu-id="27ba2-187">Všechny aliasy zadáno nebo odvození v hello `<from_source>(`s) musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="27ba2-187">All aliases provided or inferred in hello `<from_source>(`s) must be unique.</span></span> <span data-ttu-id="27ba2-188">Hello syntaxe `<collection_expression>.`%{Property_Name/ je hello stejný jako `<collection_expression>' ['"property_name"']'`.</span><span class="sxs-lookup"><span data-stu-id="27ba2-188">hello Syntax `<collection_expression>.`property_name is hello same as `<collection_expression>' ['"property_name"']'`.</span></span> <span data-ttu-id="27ba2-189">Druhé syntaxe hello však může být použita, pokud název vlastnosti obsahuje jiný identifikátor – znaky.</span><span class="sxs-lookup"><span data-stu-id="27ba2-189">However, hello latter syntax can be used if a property name contains a non-identifier characters.</span></span>  
  
<span data-ttu-id="27ba2-190">**Chybí vlastnosti, chybějící pole prvky, není definovaná hodnoty zpracování**</span><span class="sxs-lookup"><span data-stu-id="27ba2-190">**Missing properties, missing array elements, undefined values handling**</span></span>  
  
<span data-ttu-id="27ba2-191">Pokud výraz kolekce přístup k vlastnosti nebo elementy pole a že hodnota neexistuje, bude se tato hodnota ignorována a není další zpracování.</span><span class="sxs-lookup"><span data-stu-id="27ba2-191">If a collection expression accesses properties or array elements and that value does not exist, that value will be ignored and not processed further.</span></span>  
  
<span data-ttu-id="27ba2-192">**Kolekce výraz kontextu rozsahu**</span><span class="sxs-lookup"><span data-stu-id="27ba2-192">**Collection expression context scoping**</span></span>  
  
<span data-ttu-id="27ba2-193">Výraz kolekce může být celou kolekci nebo obor dokumentu:</span><span class="sxs-lookup"><span data-stu-id="27ba2-193">A collection expression may be collection-scoped or document-scoped:</span></span>  
  
-   <span data-ttu-id="27ba2-194">Výraz je celou kolekci, pokud hello podkladovém zdroji hello kolekce výrazu je buď KOŘENOVÉ nebo `collection_name`.</span><span class="sxs-lookup"><span data-stu-id="27ba2-194">An expression is collection-scoped, if hello underlying source of hello collection expression is either ROOT or `collection_name`.</span></span> <span data-ttu-id="27ba2-195">Takové výraz představuje sadu dokumenty načíst přímo z kolekce hello a není závislá na zpracování hello výrazů jiné kolekce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-195">Such an expression represents a set of documents retrieved from hello collection directly, and is not dependent on hello processing of other collection expressions.</span></span>  
  
-   <span data-ttu-id="27ba2-196">Výraz je obor dokument, pokud hello podkladovém zdroji hello kolekce výrazu `input_alias` zavedená dříve v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-196">An expression is document-scoped, if hello underlying source of hello collection expression is `input_alias` introduced earlier in hello query.</span></span> <span data-ttu-id="27ba2-197">Takové výraz představuje sadu získala při vyhodnocování výrazu kolekce hello v oboru hello každý dokument patřící toohello sady přidružené hello alias kolekce dokumentů.</span><span class="sxs-lookup"><span data-stu-id="27ba2-197">Such an expression represents a set of documents obtained by evaluating hello collection expression in hello scope of each document belonging toohello set associated with hello aliased collection.</span></span>  <span data-ttu-id="27ba2-198">Výsledná sada Hello bude sjednocení sad získala při vyhodnocování výrazu hello kolekce pro jednotlivé dokumenty hello v hello základní sady.</span><span class="sxs-lookup"><span data-stu-id="27ba2-198">hello resulting set will be a union of sets obtained by evaluating hello collection expression for each of hello documents in hello underlying set.</span></span>  
  
<span data-ttu-id="27ba2-199">**Spojení**</span><span class="sxs-lookup"><span data-stu-id="27ba2-199">**Joins**</span></span>  
  
<span data-ttu-id="27ba2-200">V aktuální verzi hello podporuje Azure Cosmos DB vnitřní spojení.</span><span class="sxs-lookup"><span data-stu-id="27ba2-200">In hello current release, Azure Cosmos DB supports inner joins.</span></span> <span data-ttu-id="27ba2-201">Připojení k další možnosti jsou chystaný.</span><span class="sxs-lookup"><span data-stu-id="27ba2-201">Additional join capabilities are forthcoming.</span></span>

<span data-ttu-id="27ba2-202">Vnitřní spojení výsledek v dokončení smíšený produkt hello nastaví účastní spojení hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-202">Inner joins result in a complete cross product of hello sets participating in hello join.</span></span> <span data-ttu-id="27ba2-203">výsledek Hello spojení N-způsob, jak je sada N element řazené kolekce členů, kde každé hodnoty v řazené kolekce členů hello je spojena s aliasy hello nastavení účasti v hello spojení a je přístupný pomocí odkazující na tento alias v jiných klauzulích.</span><span class="sxs-lookup"><span data-stu-id="27ba2-203">hello result of an N-way join is a set of N-element tuples, where each value in hello tuple is associated with hello aliased set participating in hello join and can be accessed by referencing that alias in other clauses.</span></span>  
  
<span data-ttu-id="27ba2-204">vyhodnocení Hello hello spojení, závisí na hello kontextu oboru hello účasti sady:</span><span class="sxs-lookup"><span data-stu-id="27ba2-204">hello evaluation of hello join depends on hello context scope of hello participating sets:</span></span>  
  
-  <span data-ttu-id="27ba2-205">Spojení mezi sadu kolekce A a celou kolekci nastavení B, výsledky v produktu mezi všechny elementy ve sady A a B.</span><span class="sxs-lookup"><span data-stu-id="27ba2-205">A join between collection-set A and collection-scoped set B, results in a cross product of all elements in sets A and B.</span></span>
  
-   <span data-ttu-id="27ba2-206">Spojení mezi sada A a sada obor dokumentu B, výsledkem spojení všech sad získat vyhodnocením dokumentu obor sady B pro každý dokument z nastavit A.</span><span class="sxs-lookup"><span data-stu-id="27ba2-206">A join between set A and document-scoped set B, results in a union of all sets obtained by evaluating document-scoped set B for each document from set A.</span></span>  
  
 <span data-ttu-id="27ba2-207">V aktuální verzi hello maximálně jeden výraz celou kolekci podporuje procesor dotazů hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-207">In hello current release, a maximum of one collection-scoped expression is supported by hello query processor.</span></span>  
  
<span data-ttu-id="27ba2-208">**Příklady spojení:**</span><span class="sxs-lookup"><span data-stu-id="27ba2-208">**Examples of joins:**</span></span>  
  
<span data-ttu-id="27ba2-209">Podívejme se na následující klauzule FROM hello:`<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`</span><span class="sxs-lookup"><span data-stu-id="27ba2-209">Let's look at hello following FROM clause: `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`</span></span>  
  
 <span data-ttu-id="27ba2-210">Umožní každý zdroj definovat `input_alias1, input_alias2, …, input_aliasN`.</span><span class="sxs-lookup"><span data-stu-id="27ba2-210">Let each source define `input_alias1, input_alias2, …, input_aliasN`.</span></span> <span data-ttu-id="27ba2-211">Tato klauzule FROM vrací sadu N-řazené kolekce členů (řazené kolekce členů s hodnotami N).</span><span class="sxs-lookup"><span data-stu-id="27ba2-211">This FROM clause returns a set of N-tuples (tuple with N values).</span></span> <span data-ttu-id="27ba2-212">Každá řazená kolekce členů má vyprodukované všechny aliasy kolekce iterování přes jejich příslušné sady hodnot.</span><span class="sxs-lookup"><span data-stu-id="27ba2-212">Each tuple has values produced by iterating all collection aliases over their respective sets.</span></span>  
  
<span data-ttu-id="27ba2-213">*Připojit ke Příklad 1, s 2 zdrojů:*</span><span class="sxs-lookup"><span data-stu-id="27ba2-213">*JOIN example 1, with 2 sources:*</span></span>  
  
- <span data-ttu-id="27ba2-214">Umožní `<from_source1>` celou kolekci být a představují sadu {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="27ba2-214">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="27ba2-215">Umožní `<from_source2>` být obor dokumentu, odkazující na input_alias1 a představují sady:</span><span class="sxs-lookup"><span data-stu-id="27ba2-215">Let `<from_source2>` be document-scoped referencing input_alias1 and represent sets:</span></span>  
  
    <span data-ttu-id="27ba2-216">{1, 2} pro`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="27ba2-216">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="27ba2-217">{3} pro`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="27ba2-217">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="27ba2-218">{4, 5} pro`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="27ba2-218">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="27ba2-219">Hello klauzule FROM `<from_source1> JOIN <from_source2>` bude mít za následek hello následující řazených kolekcí členů:</span><span class="sxs-lookup"><span data-stu-id="27ba2-219">hello FROM clause `<from_source1> JOIN <from_source2>` will result in hello following tuples:</span></span>  
  
    <span data-ttu-id="27ba2-220">(`input_alias1, input_alias2`):</span><span class="sxs-lookup"><span data-stu-id="27ba2-220">(`input_alias1, input_alias2`):</span></span>  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
<span data-ttu-id="27ba2-221">*Příklad 2, s 3 zdroje připojení:*</span><span class="sxs-lookup"><span data-stu-id="27ba2-221">*JOIN example 2, with 3 sources:*</span></span>  
  
- <span data-ttu-id="27ba2-222">Umožní `<from_source1>` celou kolekci být a představují sadu {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="27ba2-222">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="27ba2-223">Umožní `<from_source2>` být obor dokumentu odkazující na `input_alias1` a představují sady:</span><span class="sxs-lookup"><span data-stu-id="27ba2-223">Let `<from_source2>` be document-scoped referencing `input_alias1` and represent sets:</span></span>  
  
    <span data-ttu-id="27ba2-224">{1, 2} pro`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="27ba2-224">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="27ba2-225">{3} pro`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="27ba2-225">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="27ba2-226">{4, 5} pro`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="27ba2-226">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="27ba2-227">Umožní `<from_source3>` být obor dokumentu odkazující na `input_alias2` a představují sady:</span><span class="sxs-lookup"><span data-stu-id="27ba2-227">Let `<from_source3>` be document-scoped referencing `input_alias2` and represent sets:</span></span>  
  
    <span data-ttu-id="27ba2-228">{100, 200} pro`input_alias2 = 1,`</span><span class="sxs-lookup"><span data-stu-id="27ba2-228">{100, 200} for `input_alias2 = 1,`</span></span>  
  
    <span data-ttu-id="27ba2-229">{300} pro`input_alias2 = 3,`</span><span class="sxs-lookup"><span data-stu-id="27ba2-229">{300} for `input_alias2 = 3,`</span></span>  
  
- <span data-ttu-id="27ba2-230">Hello klauzule FROM `<from_source1> JOIN <from_source2> JOIN <from_source3>` bude mít za následek hello následující řazených kolekcí členů:</span><span class="sxs-lookup"><span data-stu-id="27ba2-230">hello FROM clause `<from_source1> JOIN <from_source2> JOIN <from_source3>` will result in hello following tuples:</span></span>  
  
    <span data-ttu-id="27ba2-231">(input_alias1, input_alias2, input_alias3):</span><span class="sxs-lookup"><span data-stu-id="27ba2-231">(input_alias1, input_alias2, input_alias3):</span></span>  
  
    <span data-ttu-id="27ba2-232">(A, 1, 100), (A, 1, 200), (B, 3, 300)</span><span class="sxs-lookup"><span data-stu-id="27ba2-232">(A, 1, 100), (A, 1, 200), (B, 3, 300)</span></span>  
  
> [!NOTE]
> <span data-ttu-id="27ba2-233">Nedostatečná řazené kolekce členů pro jiné hodnoty `input_alias1`, `input_alias2`, pro které hello `<from_source3>` nevrátil žádné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="27ba2-233">Lack of tuples for other values of `input_alias1`, `input_alias2`, for which hello `<from_source3>` did not return any values.</span></span>  
  
<span data-ttu-id="27ba2-234">*Připojit ke příklad 3, s 3 zdrojů:*</span><span class="sxs-lookup"><span data-stu-id="27ba2-234">*JOIN example 3, with 3 sources:*</span></span>  
  
- <span data-ttu-id="27ba2-235">Umožní < from_source1 > být celou kolekci a představují sadu {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="27ba2-235">Let <from_source1> be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="27ba2-236">Umožní `<from_source1>` celou kolekci být a představují sadu {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="27ba2-236">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="27ba2-237">Umožní < from_source2 > být obor dokumentu odkazující input_alias1 a představují sady:</span><span class="sxs-lookup"><span data-stu-id="27ba2-237">Let <from_source2> be document-scoped referencing input_alias1 and represent sets:</span></span>  
  
    <span data-ttu-id="27ba2-238">{1, 2} pro`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="27ba2-238">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="27ba2-239">{3} pro`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="27ba2-239">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="27ba2-240">{4, 5} pro`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="27ba2-240">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="27ba2-241">Umožní `<from_source3>` příliš vymezovat`input_alias1` a představují sady:</span><span class="sxs-lookup"><span data-stu-id="27ba2-241">Let `<from_source3>` be scoped too`input_alias1` and represent sets:</span></span>  
  
    <span data-ttu-id="27ba2-242">{100, 200} pro`input_alias2 = A,`</span><span class="sxs-lookup"><span data-stu-id="27ba2-242">{100, 200} for `input_alias2 = A,`</span></span>  
  
    <span data-ttu-id="27ba2-243">{300} pro`input_alias2 = C,`</span><span class="sxs-lookup"><span data-stu-id="27ba2-243">{300} for `input_alias2 = C,`</span></span>  
  
- <span data-ttu-id="27ba2-244">Hello klauzule FROM `<from_source1> JOIN <from_source2> JOIN <from_source3>` bude mít za následek hello následující řazených kolekcí členů:</span><span class="sxs-lookup"><span data-stu-id="27ba2-244">hello FROM clause `<from_source1> JOIN <from_source2> JOIN <from_source3>` will result in hello following tuples:</span></span>  
  
    <span data-ttu-id="27ba2-245">(`input_alias1, input_alias2, input_alias3`):</span><span class="sxs-lookup"><span data-stu-id="27ba2-245">(`input_alias1, input_alias2, input_alias3`):</span></span>  
  
    <span data-ttu-id="27ba2-246">(A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200), (C, 4, 300) (C, 5, 300)</span><span class="sxs-lookup"><span data-stu-id="27ba2-246">(A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200),  (C, 4, 300) ,  (C, 5, 300)</span></span>  
  
> [!NOTE]
> <span data-ttu-id="27ba2-247">To bylo způsobeno v smíšený produkt mezi `<from_source2>` a `<from_source3>` vzhledem k tomu, že jsou obě vymezená toohello stejné `<from_source1>`.</span><span class="sxs-lookup"><span data-stu-id="27ba2-247">This resulted in cross product between `<from_source2>` and `<from_source3>` because both are scoped toohello same `<from_source1>`.</span></span>  <span data-ttu-id="27ba2-248">To bylo způsobeno 4 (2 x 2) řazených kolekcí členů má hodnotu, 0 řazené kolekce členů s hodnota B (1 × 0) a 2 (2 × 1) řazených kolekcí členů mají hodnotu C.</span><span class="sxs-lookup"><span data-stu-id="27ba2-248">This resulted in 4 (2x2) tuples having value A, 0 tuples having value B (1x0) and 2 (2x1) tuples having value C.</span></span>  
  
<span data-ttu-id="27ba2-249">**Viz také**</span><span class="sxs-lookup"><span data-stu-id="27ba2-249">**See also**</span></span>  
  
 [<span data-ttu-id="27ba2-250">Klauzule SELECT</span><span class="sxs-lookup"><span data-stu-id="27ba2-250">SELECT clause</span></span>](#bk_select_query)  
  
##  <span data-ttu-id="27ba2-251"><a name="bk_where_clause"></a>Klauzule WHERE</span><span class="sxs-lookup"><span data-stu-id="27ba2-251"><a name="bk_where_clause"></a> WHERE clause</span></span>  
 <span data-ttu-id="27ba2-252">Určuje podmínku hello vyhledávání pro hello dokumenty vrácené dotazem hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-252">Specifies hello search condition for hello documents returned by hello query.</span></span>  
  
 <span data-ttu-id="27ba2-253">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-253">**Syntax**</span></span>  
  
```  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
 <span data-ttu-id="27ba2-254">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-254">**Arguments**</span></span>  
  
-   `<filter_condition>`  
  
     <span data-ttu-id="27ba2-255">Určuje toobe hello podmínky splněny hello dokumenty toobe vrátila.</span><span class="sxs-lookup"><span data-stu-id="27ba2-255">Specifies hello condition toobe met for hello documents toobe returned.</span></span>  
  
-   `<scalar_expression>`  
  
     <span data-ttu-id="27ba2-256">Výraz představující hodnotu toobe hello počítaný.</span><span class="sxs-lookup"><span data-stu-id="27ba2-256">Expression representing hello value toobe computed.</span></span> <span data-ttu-id="27ba2-257">V tématu hello [skalární výrazy](#bk_scalar_expressions) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="27ba2-257">See hello [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
 <span data-ttu-id="27ba2-258">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="27ba2-258">**Remarks**</span></span>  
  
 <span data-ttu-id="27ba2-259">Aby hello vrátil dokumentu toobe výraz zadaný jako podmínku filtrování se musí vyhodnotit tootrue.</span><span class="sxs-lookup"><span data-stu-id="27ba2-259">In order for hello document toobe returned an expression specified as filter condition must evaluate tootrue.</span></span> <span data-ttu-id="27ba2-260">Pouze logickou hodnotu true budou splňovat podmínky hello, jakoukoli jinou hodnotu: nedefinované, null, hodnotu false, číslo, pole nebo objekt nebudou splňovat podmínky hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-260">Only Boolean value true will satisfy hello condition, any other value: undefined, null, false, Number, Array or Object will not satisfy hello condition.</span></span>  
  
##  <span data-ttu-id="27ba2-261"><a name="bk_orderby_clause"></a>Klauzuli ORDER by</span><span class="sxs-lookup"><span data-stu-id="27ba2-261"><a name="bk_orderby_clause"></a> ORDER BY clause</span></span>  
 <span data-ttu-id="27ba2-262">Určuje hello řazení pořadí výsledků vrácených dotazem hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-262">Specifies hello sorting order for results returned by hello query.</span></span>  
  
 <span data-ttu-id="27ba2-263">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-263">**Syntax**</span></span>  
  
```  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= <scalar_expression> [ASC | DESC]  
  
```  
  
 <span data-ttu-id="27ba2-264">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-264">**Arguments**</span></span>  
  
-   `<sort_specification>`  
  
     <span data-ttu-id="27ba2-265">Určuje vlastnosti nebo výraz, ve kterém sada výsledků dotazu hello toosort.</span><span class="sxs-lookup"><span data-stu-id="27ba2-265">Specifies a property or expression on which toosort hello query result set.</span></span> <span data-ttu-id="27ba2-266">Sloupec pro řazení lze zadat jako název nebo sloupci alias.</span><span class="sxs-lookup"><span data-stu-id="27ba2-266">A sort column can be specified as a name or column alias.</span></span>  
  
     <span data-ttu-id="27ba2-267">Je možné zadat více řazení sloupců.</span><span class="sxs-lookup"><span data-stu-id="27ba2-267">Multiple sort columns can be specified.</span></span> <span data-ttu-id="27ba2-268">Názvy sloupců musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="27ba2-268">Column names must be unique.</span></span> <span data-ttu-id="27ba2-269">pořadí Hello hello řazení sloupců v klauzuli pořadí podle hello definuje hello organizace hello Seřazená sada výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-269">hello sequence of hello sort columns in hello ORDER BY clause defines hello organization of hello sorted result set.</span></span> <span data-ttu-id="27ba2-270">To znamená sadu výsledků hello řazena podle hello první vlastnost a potom tento seřazený seznam je seřazen podle hello druhou vlastností a tak dále.</span><span class="sxs-lookup"><span data-stu-id="27ba2-270">That is, hello result set is sorted by hello first property and then that ordered list is sorted by hello second property, and so on.</span></span>  
  
     <span data-ttu-id="27ba2-271">názvy sloupců Hello odkazovat v klauzuli pořadí podle hello musí odpovídat tooeither sloupec v hello vyberte sloupec seznamu nebo tooa definovaná v tabulce, zadaný v klauzuli FROM hello bez jakékoli nejednoznačnosti.</span><span class="sxs-lookup"><span data-stu-id="27ba2-271">hello column names referenced in hello ORDER BY clause must correspond tooeither a column in hello select list or tooa column defined in a table specified in hello FROM clause without any ambiguities.</span></span>  
  
-   `<sort_expression>`  
  
     <span data-ttu-id="27ba2-272">Určuje vlastnosti jediné nebo výrazu v sadě výsledků dotazu které toosort hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-272">Specifies a single property or expression on which toosort hello query result set.</span></span>  
  
-   `<scalar_expression>`  
  
     <span data-ttu-id="27ba2-273">V tématu hello [skalární výrazy](#bk_scalar_expressions) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="27ba2-273">See hello [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
-   `ASC | DESC`  
  
     <span data-ttu-id="27ba2-274">Určuje, že hello hodnoty zadané sloupce hello by měly být seřazeny ve vzestupném nebo sestupném pořadí.</span><span class="sxs-lookup"><span data-stu-id="27ba2-274">Specifies that hello values in hello specified column should be sorted in ascending or descending order.</span></span> <span data-ttu-id="27ba2-275">ASC seřadí od hello nejnižší hodnotu toohighest hodnoty.</span><span class="sxs-lookup"><span data-stu-id="27ba2-275">ASC sorts from hello lowest value toohighest value.</span></span> <span data-ttu-id="27ba2-276">DESC seřadí z hodnoty toolowest nejvyšší hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-276">DESC sorts from highest value toolowest value.</span></span> <span data-ttu-id="27ba2-277">ASC je hello výchozí pořadí řazení.</span><span class="sxs-lookup"><span data-stu-id="27ba2-277">ASC is hello default sort order.</span></span> <span data-ttu-id="27ba2-278">Hodnoty Null jsou považovány za hello nejnižší možné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="27ba2-278">Null values are treated as hello lowest possible values.</span></span>  
  
 <span data-ttu-id="27ba2-279">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="27ba2-279">**Remarks**</span></span>  
  
 <span data-ttu-id="27ba2-280">I když gramatiky dotazů hello podporuje více pořadí podle vlastností, hello Azure Cosmos DB dotazu runtime podporuje řazení pouze s jedinou vlastností a pouze s názvy vlastností, tj., není pro počítané vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="27ba2-280">While hello query grammar supports multiple order by properties, hello Azure Cosmos DB query runtime supports sorting only against a single property, and only against property names, i.e., not against computed properties.</span></span> <span data-ttu-id="27ba2-281">Řazení také vyžaduje, že hello indexování zásad zahrnuje indexem rozsahu pro vlastnost hello a hello zadaný typ s maximální přesnost hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-281">Sorting also requires that hello indexing policy includes a range index for hello property and hello specified type, with hello maximum precision.</span></span> <span data-ttu-id="27ba2-282">Najdete v dokumentaci k zásadám podrobnosti indexování toohello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-282">Refer toohello indexing policy documentation for more details.</span></span>  
  
##  <span data-ttu-id="27ba2-283"><a name="bk_scalar_expressions"></a>Skalární výrazy</span><span class="sxs-lookup"><span data-stu-id="27ba2-283"><a name="bk_scalar_expressions"></a> Scalar expressions</span></span>  
 <span data-ttu-id="27ba2-284">Skalární výraz, který je kombinací symbolů a operátory, které se dají posoudit tooobtain jednu hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-284">A scalar expression is a combination of symbols and operators that can be evaluated tooobtain a single value.</span></span> <span data-ttu-id="27ba2-285">Jednoduché výrazy může být konstanty, odkazy na vlastnost, odkazuje na element pole, odkazy na alias nebo volání funkce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-285">Simple expressions can be constants, property references, array element references, alias references, or function calls.</span></span> <span data-ttu-id="27ba2-286">Jednoduché výrazy lze spojovat do složité výrazy pomocí operátorů.</span><span class="sxs-lookup"><span data-stu-id="27ba2-286">Simple expressions can be combined into complex expressions using operators.</span></span>  
  
 <span data-ttu-id="27ba2-287">Podrobnosti na hodnotách, které skalární výraz, který může mít najdete v tématu [konstanty](#bk_constants) části.</span><span class="sxs-lookup"><span data-stu-id="27ba2-287">For details on values which scalar expression may have, see [Constants](#bk_constants) section.</span></span>  
  
 <span data-ttu-id="27ba2-288">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-288">**Syntax**</span></span>  
  
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
  
 <span data-ttu-id="27ba2-289">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-289">**Arguments**</span></span>  
  
-   `<constant>`  
  
     <span data-ttu-id="27ba2-290">Představuje konstantní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-290">Represents a constant value.</span></span> <span data-ttu-id="27ba2-291">V tématu [konstanty](#bk_constants) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="27ba2-291">See [Constants](#bk_constants) section for details.</span></span>  
  
-   `input_alias`  
  
     <span data-ttu-id="27ba2-292">Reprezentuje hodnotu definované hello `input_alias` byla zavedená v hello `FROM` klauzule.</span><span class="sxs-lookup"><span data-stu-id="27ba2-292">Represents a value defined by hello `input_alias` introduced in hello `FROM` clause.</span></span>  
    <span data-ttu-id="27ba2-293">Tato hodnota je zaručeno toonot být **nedefinované** –**nedefinované** hodnoty ve vstupu hello se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="27ba2-293">This value is guaranteed toonot be **undefined** –**undefined** values in hello input are skipped.</span></span>  
  
-   `<scalar_expression>.property_name`  
  
     <span data-ttu-id="27ba2-294">Reprezentuje hodnotu hello vlastnosti objektu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-294">Represents a value of hello property of an object.</span></span> <span data-ttu-id="27ba2-295">Pokud hello vlastnost neexistuje nebo vlastnost odkazuje na hodnotu, která není objekt a potom hello výraz vyhodnotí příliš**nedefinované** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-295">If hello property does not exist or property is referenced on a value which is not an object, then hello expression evaluates too**undefined** value.</span></span>  
  
-   `<scalar_expression>'['"property_name"|array_index']'`  
  
     <span data-ttu-id="27ba2-296">Reprezentuje hodnotu hello vlastnost s názvem `property_name` nebo element pole s indexem `array_index` objekt nebo pole.</span><span class="sxs-lookup"><span data-stu-id="27ba2-296">Represents a value of hello property with name `property_name` or array element with index `array_index` of an object/array.</span></span> <span data-ttu-id="27ba2-297">Pokud vlastnost nebo pole indexu je hello neexistuje nebo hello vlastnost nebo pole indexu je odkazováno na hodnotu, která není objekt nebo pole, vyhodnotí výraz hello tooundefined hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-297">If hello property/array index does not exist or hello property/array index is referenced on a value which is not an object/array, then hello expression evaluates tooundefined value.</span></span>  
  
-   `unary_operator <scalar_expression>`  
  
     <span data-ttu-id="27ba2-298">Představuje operátor, který je použité tooa jedna hodnota.</span><span class="sxs-lookup"><span data-stu-id="27ba2-298">Represents an operator that is applied tooa single value.</span></span> <span data-ttu-id="27ba2-299">V tématu [operátory](#bk_operators) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="27ba2-299">See [Operators](#bk_operators) section for details.</span></span>  
  
-   `<scalar_expression> binary_operator <scalar_expression>`  
  
     <span data-ttu-id="27ba2-300">Představuje operátor, který je použité tootwo hodnoty.</span><span class="sxs-lookup"><span data-stu-id="27ba2-300">Represents an operator that is applied tootwo values.</span></span> <span data-ttu-id="27ba2-301">V tématu [operátory](#bk_operators) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="27ba2-301">See [Operators](#bk_operators) section for details.</span></span>  
  
-   `<scalar_function_expression>`  
  
     <span data-ttu-id="27ba2-302">Reprezentuje hodnotu definované výsledku volání funkce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-302">Represents a value defined by a result of a function call.</span></span>  
  
-   `udf_scalar_function`  
  
     <span data-ttu-id="27ba2-303">Jméno uživatele, hello definované skalární funkce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-303">Name of hello user defined scalar function.</span></span>  
  
-   `builtin_scalar_function`  
  
     <span data-ttu-id="27ba2-304">Název předdefinované skalární funkce hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-304">Name of hello built-in scalar function.</span></span>  
  
-   `<create_object_expression>`  
  
     <span data-ttu-id="27ba2-305">Představuje hodnotu získat tak, že vytvoříte nový objekt v zadaných vlastností a jejich hodnoty.</span><span class="sxs-lookup"><span data-stu-id="27ba2-305">Represents a value obtained by creating a new object with specified properties and their values.</span></span>  
  
-   `<create_array_expression>`  
  
     <span data-ttu-id="27ba2-306">Reprezentuje hodnotu získala při vytváření nové pole se zadanými hodnotami, jako elementy</span><span class="sxs-lookup"><span data-stu-id="27ba2-306">Represents a value obtained by creating a new array with specified values as elements</span></span>  
  
-   `parameter_name`  
  
     <span data-ttu-id="27ba2-307">Reprezentuje hodnotu hello zadaný název parametru.</span><span class="sxs-lookup"><span data-stu-id="27ba2-307">Represents a value of hello specified parameter name.</span></span> <span data-ttu-id="27ba2-308">Názvy parametrů musí mít jeden @ jako první znak hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-308">Parameter names must have a single @ as hello first character.</span></span>  
  
 <span data-ttu-id="27ba2-309">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="27ba2-309">**Remarks**</span></span>  
  
 <span data-ttu-id="27ba2-310">Při volání předdefinované nebo uživatel definované skalární funkce musí být definovány všechny argumenty.</span><span class="sxs-lookup"><span data-stu-id="27ba2-310">When calling a built-in or user defined scalar function all arguments must be defined.</span></span> <span data-ttu-id="27ba2-311">Pokud je některý z argumentů hello nedefinované, nebude volána funkce hello a hello výsledkem bude definován.</span><span class="sxs-lookup"><span data-stu-id="27ba2-311">If any of hello arguments is undefined, hello function will not be called and hello result will be undefined.</span></span>  
  
 <span data-ttu-id="27ba2-312">Při vytvoření objektu, bude přeskočen a nejsou zahrnuty v hello vytvořený objekt jakákoli vlastnost, která je přiřazena nedefinovanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-312">When creating an object, any property that is assigned undefined value will be skipped and not included in hello created object.</span></span>  
  
 <span data-ttu-id="27ba2-313">Při vytváření pole, libovolná hodnota elementu, který je přiřazen **nedefinované** hodnota bude přeskočen a nejsou zahrnuty v objektu vytvořeny hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-313">When creating an array, any element value that is assigned **undefined** value will be skipped and not included in hello created object.</span></span> <span data-ttu-id="27ba2-314">To způsobí, že hello vedle definovaný element tootake jeho místo tak nebude tohoto pole hello vytvořili přeskočili indexy.</span><span class="sxs-lookup"><span data-stu-id="27ba2-314">This will cause hello next defined element tootake its place in such a way that hello created array will not have skipped indexes.</span></span>  
  
##  <span data-ttu-id="27ba2-315"><a name="bk_operators"></a>Operátory</span><span class="sxs-lookup"><span data-stu-id="27ba2-315"><a name="bk_operators"></a> Operators</span></span>  
 <span data-ttu-id="27ba2-316">Tato část popisuje operátory hello podporována.</span><span class="sxs-lookup"><span data-stu-id="27ba2-316">This section describes hello supported operators.</span></span> <span data-ttu-id="27ba2-317">Každý operátor může být přiřazené tooexactly jednu kategorii.</span><span class="sxs-lookup"><span data-stu-id="27ba2-317">Each operator can be assigned tooexactly one category.</span></span>  
  
 <span data-ttu-id="27ba2-318">V tématu **operátor kategorie** tabulce podrobnosti ohledně zpracování **nedefinované** hodnoty, požadavky na typ vstupní hodnoty a zpracování hodnot s není odpovídající typy.</span><span class="sxs-lookup"><span data-stu-id="27ba2-318">See **Operator categories** table below, for details regarding handling of **undefined** values, type requirements for input values and handling of values with not matching types.</span></span>  
  
 <span data-ttu-id="27ba2-319">**Operátor kategorie:**</span><span class="sxs-lookup"><span data-stu-id="27ba2-319">**Operator categories:**</span></span>  
  
|<span data-ttu-id="27ba2-320">**Kategorie**</span><span class="sxs-lookup"><span data-stu-id="27ba2-320">**Category**</span></span>|<span data-ttu-id="27ba2-321">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="27ba2-321">**Details**</span></span>|  
|-|-|  
|<span data-ttu-id="27ba2-322">**aritmetické operace**</span><span class="sxs-lookup"><span data-stu-id="27ba2-322">**arithmetic**</span></span>|<span data-ttu-id="27ba2-323">Operátor očekává input(s) toobe čísel.</span><span class="sxs-lookup"><span data-stu-id="27ba2-323">Operator expects input(s) toobe Number(s).</span></span> <span data-ttu-id="27ba2-324">Výstup je také číslo.</span><span class="sxs-lookup"><span data-stu-id="27ba2-324">Output is also a Number.</span></span> <span data-ttu-id="27ba2-325">Pokud je některý z hello vstupů **nedefinované** nebo typ než číslo potom hello výsledek **nedefinované**.</span><span class="sxs-lookup"><span data-stu-id="27ba2-325">If any of hello inputs is **undefined** or type other than Number then hello result is **undefined**.</span></span>|  
|<span data-ttu-id="27ba2-326">**bitové operace**</span><span class="sxs-lookup"><span data-stu-id="27ba2-326">**bitwise**</span></span>|<span data-ttu-id="27ba2-327">Operátor očekává input(s) toobe 32bitové číslo se znaménkem čísel.</span><span class="sxs-lookup"><span data-stu-id="27ba2-327">Operator expects input(s) toobe 32-bit signed integer Number(s).</span></span> <span data-ttu-id="27ba2-328">Výstup je také 32bitové číslo se znaménkem číslo.</span><span class="sxs-lookup"><span data-stu-id="27ba2-328">Output is also 32-bit signed integer Number.</span></span><br /><br /> <span data-ttu-id="27ba2-329">Libovolná hodnota není celé číslo se zaokrouhlí.</span><span class="sxs-lookup"><span data-stu-id="27ba2-329">Any non-integer value will be rounded.</span></span> <span data-ttu-id="27ba2-330">Kladné celé číslo se zaokrouhlí směrem dolů, záporné hodnoty zaokrouhlený nahoru.</span><span class="sxs-lookup"><span data-stu-id="27ba2-330">Positive value will be rounded down, negative values rounded up.</span></span><br /><br /> <span data-ttu-id="27ba2-331">Jakoukoli hodnotu, která je mimo rozsah 32bitové celé číslo hello budou převedeny provedením poslední 32 bity jeho dva pro zápis doplňku.</span><span class="sxs-lookup"><span data-stu-id="27ba2-331">Any value that is outside of hello 32-bit integer range will be converted, by taking last 32-bits of its two's complement notation.</span></span><br /><br /> <span data-ttu-id="27ba2-332">Pokud je některý z hello vstupů **nedefinované** nebo jiného typu než číslo, pak je výsledek hello **nedefinované**.</span><span class="sxs-lookup"><span data-stu-id="27ba2-332">If any of hello inputs is **undefined** or type other than Number, then hello result is **undefined**.</span></span><br /><br /> <span data-ttu-id="27ba2-333">**Poznámka:** hello výše chování je kompatibilní s chováním bitový operátor jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="27ba2-333">**Note:** hello above behavior is compatible with JavaScript bitwise operator behavior.</span></span>|  
|<span data-ttu-id="27ba2-334">**logické**</span><span class="sxs-lookup"><span data-stu-id="27ba2-334">**logical**</span></span>|<span data-ttu-id="27ba2-335">Operátor očekává input(s) toobe Boolean(s).</span><span class="sxs-lookup"><span data-stu-id="27ba2-335">Operator expects input(s) toobe Boolean(s).</span></span> <span data-ttu-id="27ba2-336">Výstup je také logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-336">Output is also a Boolean.</span></span><br /><span data-ttu-id="27ba2-337">Pokud je některý z hello vstupů **nedefinované** nebo jiného typu než logickou hodnotu, bude výsledkem hello **nedefinované**.</span><span class="sxs-lookup"><span data-stu-id="27ba2-337">If any of hello inputs is **undefined** or type other than Boolean, then hello result will be **undefined**.</span></span>|  
|<span data-ttu-id="27ba2-338">**porovnání**</span><span class="sxs-lookup"><span data-stu-id="27ba2-338">**comparison**</span></span>|<span data-ttu-id="27ba2-339">Operátor očekává input(s) toohave hello stejný typ a nesmí být definován.</span><span class="sxs-lookup"><span data-stu-id="27ba2-339">Operator expects input(s) toohave hello same type and not be undefined.</span></span> <span data-ttu-id="27ba2-340">Výstup je logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="27ba2-340">Output is a Boolean.</span></span><br /><br /> <span data-ttu-id="27ba2-341">Pokud je některý z hello vstupů **nedefinované** nebo hello vstupy mají různé typy a pak je výsledek hello **nedefinované**.</span><span class="sxs-lookup"><span data-stu-id="27ba2-341">If any of hello inputs is **undefined** or hello inputs have different types, then hello result is **undefined**.</span></span><br /><br /> <span data-ttu-id="27ba2-342">V tématu **řazení hodnot pro porovnání** tabulky pro hodnotu řazení podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="27ba2-342">See **Ordering of values for comparison** table for value ordering details.</span></span>|  
|<span data-ttu-id="27ba2-343">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="27ba2-343">**string**</span></span>|<span data-ttu-id="27ba2-344">Operátor očekává input(s) toobe řetězec nebo řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-344">Operator expects input(s) toobe String(s).</span></span> <span data-ttu-id="27ba2-345">Výstup je také řetězec.</span><span class="sxs-lookup"><span data-stu-id="27ba2-345">Output is also a String.</span></span><br /><span data-ttu-id="27ba2-346">Pokud je některý z hello vstupů **nedefinované** nebo typ jiný než řetězec pak hello výsledek je **nedefinované**.</span><span class="sxs-lookup"><span data-stu-id="27ba2-346">If any of hello inputs is **undefined** or type other than String then hello result is **undefined**.</span></span>|  
  
 <span data-ttu-id="27ba2-347">**Unární operátory:**</span><span class="sxs-lookup"><span data-stu-id="27ba2-347">**Unary operators:**</span></span>  
  
|<span data-ttu-id="27ba2-348">**Název**</span><span class="sxs-lookup"><span data-stu-id="27ba2-348">**Name**</span></span>|<span data-ttu-id="27ba2-349">**Operátor**</span><span class="sxs-lookup"><span data-stu-id="27ba2-349">**Operator**</span></span>|<span data-ttu-id="27ba2-350">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="27ba2-350">**Details**</span></span>|  
|-|-|-|  
|<span data-ttu-id="27ba2-351">**aritmetické operace**</span><span class="sxs-lookup"><span data-stu-id="27ba2-351">**arithmetic**</span></span>|+<br /><br /> -|<span data-ttu-id="27ba2-352">Vrátí hello číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-352">Returns hello number value.</span></span><br /><br /> <span data-ttu-id="27ba2-353">Bitovou negaci.</span><span class="sxs-lookup"><span data-stu-id="27ba2-353">Bitwise negation.</span></span> <span data-ttu-id="27ba2-354">Vrátí Negované číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-354">Returns negated number value.</span></span>|  
|<span data-ttu-id="27ba2-355">**bitové operace**</span><span class="sxs-lookup"><span data-stu-id="27ba2-355">**bitwise**</span></span>|~|<span data-ttu-id="27ba2-356">Ty, které se doplňku.</span><span class="sxs-lookup"><span data-stu-id="27ba2-356">Ones' complement.</span></span> <span data-ttu-id="27ba2-357">Vrátí zbytek číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-357">Returns a complement of a number value.</span></span>|  
|<span data-ttu-id="27ba2-358">**Logické**</span><span class="sxs-lookup"><span data-stu-id="27ba2-358">**Logical**</span></span>|<span data-ttu-id="27ba2-359">**NENÍ**</span><span class="sxs-lookup"><span data-stu-id="27ba2-359">**NOT**</span></span>|<span data-ttu-id="27ba2-360">Negace.</span><span class="sxs-lookup"><span data-stu-id="27ba2-360">Negation.</span></span> <span data-ttu-id="27ba2-361">Vrátí Negované logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="27ba2-361">Returns negated Boolean value.</span></span>|  
  
 <span data-ttu-id="27ba2-362">**Binární operátory:**</span><span class="sxs-lookup"><span data-stu-id="27ba2-362">**Binary operators:**</span></span>  
  
|<span data-ttu-id="27ba2-363">**Název**</span><span class="sxs-lookup"><span data-stu-id="27ba2-363">**Name**</span></span>|<span data-ttu-id="27ba2-364">**Operátor**</span><span class="sxs-lookup"><span data-stu-id="27ba2-364">**Operator**</span></span>|<span data-ttu-id="27ba2-365">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="27ba2-365">**Details**</span></span>|  
|-|-|-|  
|<span data-ttu-id="27ba2-366">**aritmetické operace**</span><span class="sxs-lookup"><span data-stu-id="27ba2-366">**arithmetic**</span></span>|+<br /><br /> -<br /><br /> *<br /><br /> /<br /><br /> %|<span data-ttu-id="27ba2-367">Přidání.</span><span class="sxs-lookup"><span data-stu-id="27ba2-367">Addition.</span></span><br /><br /> <span data-ttu-id="27ba2-368">Odčítání.</span><span class="sxs-lookup"><span data-stu-id="27ba2-368">Subtraction.</span></span><br /><br /> <span data-ttu-id="27ba2-369">Násobení.</span><span class="sxs-lookup"><span data-stu-id="27ba2-369">Multiplication.</span></span><br /><br /> <span data-ttu-id="27ba2-370">Dělení.</span><span class="sxs-lookup"><span data-stu-id="27ba2-370">Division.</span></span><br /><br /> <span data-ttu-id="27ba2-371">Modulační.</span><span class="sxs-lookup"><span data-stu-id="27ba2-371">Modulation.</span></span>|  
|<span data-ttu-id="27ba2-372">**bitové operace**</span><span class="sxs-lookup"><span data-stu-id="27ba2-372">**bitwise**</span></span>|<span data-ttu-id="27ba2-373">&#124;</span><span class="sxs-lookup"><span data-stu-id="27ba2-373">&#124;</span></span><br /><br /> &<br /><br /> ^<br /><br /> <<<br /><br /> >><br /><br /> >>>|<span data-ttu-id="27ba2-374">Bitový operátor OR.</span><span class="sxs-lookup"><span data-stu-id="27ba2-374">Bitwise OR.</span></span><br /><br /> <span data-ttu-id="27ba2-375">Bitové operace AND.</span><span class="sxs-lookup"><span data-stu-id="27ba2-375">Bitwise AND.</span></span><br /><br /> <span data-ttu-id="27ba2-376">Bitové operace XOR.</span><span class="sxs-lookup"><span data-stu-id="27ba2-376">Bitwise XOR.</span></span><br /><br /> <span data-ttu-id="27ba2-377">Operátor posunu vlevo.</span><span class="sxs-lookup"><span data-stu-id="27ba2-377">Left Shift.</span></span><br /><br /> <span data-ttu-id="27ba2-378">Posunutí doprava.</span><span class="sxs-lookup"><span data-stu-id="27ba2-378">Right Shift.</span></span><br /><br /> <span data-ttu-id="27ba2-379">Posunutí doprava výplně nula.</span><span class="sxs-lookup"><span data-stu-id="27ba2-379">Zero-fill Right Shift.</span></span>|  
|<span data-ttu-id="27ba2-380">**logické**</span><span class="sxs-lookup"><span data-stu-id="27ba2-380">**logical**</span></span>|<span data-ttu-id="27ba2-381">**A**</span><span class="sxs-lookup"><span data-stu-id="27ba2-381">**AND**</span></span><br /><br /> <span data-ttu-id="27ba2-382">**OR**</span><span class="sxs-lookup"><span data-stu-id="27ba2-382">**OR**</span></span>|<span data-ttu-id="27ba2-383">Logické spojení.</span><span class="sxs-lookup"><span data-stu-id="27ba2-383">Logical conjunction.</span></span> <span data-ttu-id="27ba2-384">Vrátí **true** Pokud jsou oba argumenty **true**, vrátí **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="27ba2-384">Returns **true** if both arguments are **true**, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="27ba2-385">Logické spojení.</span><span class="sxs-lookup"><span data-stu-id="27ba2-385">Logical conjunction.</span></span> <span data-ttu-id="27ba2-386">Vrátí **true** Pokud jsou oba argumenty **true**, vrátí **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="27ba2-386">Returns **true** if both arguments are **true**, returns **false** otherwise.</span></span>|  
|<span data-ttu-id="27ba2-387">**porovnání**</span><span class="sxs-lookup"><span data-stu-id="27ba2-387">**comparison**</span></span>|**=**<br /><br /> <span data-ttu-id="27ba2-388">**!=, <>**</span><span class="sxs-lookup"><span data-stu-id="27ba2-388">**!=, <>**</span></span><br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> <span data-ttu-id="27ba2-389">**??**</span><span class="sxs-lookup"><span data-stu-id="27ba2-389">**??**</span></span>|<span data-ttu-id="27ba2-390">Rovná se.</span><span class="sxs-lookup"><span data-stu-id="27ba2-390">Equals.</span></span> <span data-ttu-id="27ba2-391">Vrátí **true** Pokud argumenty jsou stejné, vrátí **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="27ba2-391">Returns **true** if arguments are equal, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="27ba2-392">Není rovno.</span><span class="sxs-lookup"><span data-stu-id="27ba2-392">Not equal to.</span></span> <span data-ttu-id="27ba2-393">Vrátí **true** Pokud argumenty nejsou stejné, vrátí **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="27ba2-393">Returns **true** if arguments are not equal, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="27ba2-394">Větší než.</span><span class="sxs-lookup"><span data-stu-id="27ba2-394">Greater Than.</span></span> <span data-ttu-id="27ba2-395">Vrátí **true** Pokud první argument je větší než druhá text hello, vrátí **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="27ba2-395">Returns **true** if first argument is greater than hello second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="27ba2-396">Větší než nebo rovna hodnotě.</span><span class="sxs-lookup"><span data-stu-id="27ba2-396">Greater Than or Equal To.</span></span> <span data-ttu-id="27ba2-397">Vrátí **true** Pokud první argument je větší než nebo roven toohello druhá, vrátí **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="27ba2-397">Returns **true** if first argument is greater than or equal toohello second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="27ba2-398">Menší než.</span><span class="sxs-lookup"><span data-stu-id="27ba2-398">Less Than.</span></span> <span data-ttu-id="27ba2-399">Vrátí **true** Pokud první argument je menší než druhá text hello, vrátí **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="27ba2-399">Returns **true** if first argument is less than hello second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="27ba2-400">Menší než nebo rovno.</span><span class="sxs-lookup"><span data-stu-id="27ba2-400">Less Than or Equal To.</span></span> <span data-ttu-id="27ba2-401">Vrátí **true** Pokud první argument je menší než nebo rovna toohello druhé jeden návratový **false** jinak.</span><span class="sxs-lookup"><span data-stu-id="27ba2-401">Returns **true** if first argument is less than or equal toohello second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="27ba2-402">Sloučení.</span><span class="sxs-lookup"><span data-stu-id="27ba2-402">Coalesce.</span></span> <span data-ttu-id="27ba2-403">Vrátí hello druhý argument, pokud je hello argument **nedefinované** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-403">Returns hello second argument if hello first argument is an **undefined** value.</span></span>|  
|<span data-ttu-id="27ba2-404">**Řetězec**</span><span class="sxs-lookup"><span data-stu-id="27ba2-404">**String**</span></span>|<span data-ttu-id="27ba2-405">**&#124;&#124;**</span><span class="sxs-lookup"><span data-stu-id="27ba2-405">**&#124;&#124;**</span></span>|<span data-ttu-id="27ba2-406">Zřetězení.</span><span class="sxs-lookup"><span data-stu-id="27ba2-406">Concatenation.</span></span> <span data-ttu-id="27ba2-407">Vrátí zřetězení oba argumenty.</span><span class="sxs-lookup"><span data-stu-id="27ba2-407">Returns a concatenation of both arguments.</span></span>|  
  
 <span data-ttu-id="27ba2-408">**Ternární operátory:**</span><span class="sxs-lookup"><span data-stu-id="27ba2-408">**Ternary operators:**</span></span>  
  
|<span data-ttu-id="27ba2-409">Ternární operátor</span><span class="sxs-lookup"><span data-stu-id="27ba2-409">Ternary operator</span></span>|<span data-ttu-id="27ba2-410">?</span><span class="sxs-lookup"><span data-stu-id="27ba2-410">?</span></span>|<span data-ttu-id="27ba2-411">Vrátí hello druhý argument, pokud je první argument hello výsledkem příliš**true**; jinak vrátí třetí argument hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-411">Returns hello second argument if hello first argument evaluates too**true**; return hello third argument otherwise.</span></span>|  
|-|-|-|  
  
 <span data-ttu-id="27ba2-412">**Řazení hodnot pro porovnání**</span><span class="sxs-lookup"><span data-stu-id="27ba2-412">**Ordering of values for comparison**</span></span>  
  
|<span data-ttu-id="27ba2-413">**Typ**</span><span class="sxs-lookup"><span data-stu-id="27ba2-413">**Type**</span></span>|<span data-ttu-id="27ba2-414">**Pořadí hodnoty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-414">**Values order**</span></span>|  
|-|-|  
|<span data-ttu-id="27ba2-415">**Nedefinovaná**</span><span class="sxs-lookup"><span data-stu-id="27ba2-415">**Undefined**</span></span>|<span data-ttu-id="27ba2-416">Není porovnatelný.</span><span class="sxs-lookup"><span data-stu-id="27ba2-416">Not comparable.</span></span>|  
|<span data-ttu-id="27ba2-417">**Hodnotu Null**</span><span class="sxs-lookup"><span data-stu-id="27ba2-417">**Null**</span></span>|<span data-ttu-id="27ba2-418">Jedna hodnota: **hodnotu null.**</span><span class="sxs-lookup"><span data-stu-id="27ba2-418">Single value: **null**</span></span>|  
|<span data-ttu-id="27ba2-419">**Číslo**</span><span class="sxs-lookup"><span data-stu-id="27ba2-419">**Number**</span></span>|<span data-ttu-id="27ba2-420">Fyzická reálné číslo.</span><span class="sxs-lookup"><span data-stu-id="27ba2-420">Natural real number.</span></span><br /><br /> <span data-ttu-id="27ba2-421">Zápornou hodnotu Infinity je menší než ostatní číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-421">Negative Infinity value is smaller than any other Number value.</span></span><br /><br /> <span data-ttu-id="27ba2-422">Je větší než jakoukoli jinou hodnotu, číslo kladné nekonečnou hodnotu. **NaN** hodnota není porovnatelný.</span><span class="sxs-lookup"><span data-stu-id="27ba2-422">Positive Infinity value is larger than any other Number value.**NaN** value is not comparable.</span></span> <span data-ttu-id="27ba2-423">Porovnávání s **NaN** bude mít za následek **nedefinované** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-423">Comparing with **NaN** will result in **undefined** value.</span></span>|  
|<span data-ttu-id="27ba2-424">**Řetězec**</span><span class="sxs-lookup"><span data-stu-id="27ba2-424">**String**</span></span>|<span data-ttu-id="27ba2-425">Lexicographical pořadí.</span><span class="sxs-lookup"><span data-stu-id="27ba2-425">Lexicographical order.</span></span>|  
|<span data-ttu-id="27ba2-426">**Pole**</span><span class="sxs-lookup"><span data-stu-id="27ba2-426">**Array**</span></span>|<span data-ttu-id="27ba2-427">Žádné řazení, ale přiměřenou.</span><span class="sxs-lookup"><span data-stu-id="27ba2-427">No ordering, but equitable.</span></span>|  
|<span data-ttu-id="27ba2-428">**Objekt**</span><span class="sxs-lookup"><span data-stu-id="27ba2-428">**Object**</span></span>|<span data-ttu-id="27ba2-429">Žádné řazení, ale přiměřenou.</span><span class="sxs-lookup"><span data-stu-id="27ba2-429">No ordering, but equitable.</span></span>|  
  
 <span data-ttu-id="27ba2-430">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="27ba2-430">**Remarks**</span></span>  
  
 <span data-ttu-id="27ba2-431">V Azure DB Cosmos hello typy hodnot často není známý, dokud se ve skutečnosti načítají z databáze hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-431">In Azure Cosmos DB, hello types of values are often not known until they are actually retrieved from hello database.</span></span> <span data-ttu-id="27ba2-432">Většina hello operátorů v pořadí toosupport efektivní provádění dotazů má požadavky striktní typu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-432">In order toosupport efficient execution of queries, most of hello operators have strict type requirements.</span></span> <span data-ttu-id="27ba2-433">Operátory samy o sobě taky neprovádět implicitní převody.</span><span class="sxs-lookup"><span data-stu-id="27ba2-433">Also operators by themselves do not perform implicit conversions.</span></span>  
  
 <span data-ttu-id="27ba2-434">To znamená, že jako dotaz: vybrat * z ROOT r kde r.Age = 21 vrátí jenom dokumenty s vlastností číslo rovno toohello stáří 21.</span><span class="sxs-lookup"><span data-stu-id="27ba2-434">This means that a query like: SELECT * FROM ROOT r WHERE r.Age = 21 will only return documents with property Age equal toohello number 21.</span></span> <span data-ttu-id="27ba2-435">Dokumenty s vlastnost stáří rovna toohello řetězec "21" nebo hello řetězec "0021" nebude odpovídat jako hello výraz "21" = 21 vyhodnotí tooundefined.</span><span class="sxs-lookup"><span data-stu-id="27ba2-435">Documents with property Age equal toohello string "21" or hello string "0021" will not match, as hello expression "21" = 21 evaluates tooundefined.</span></span> <span data-ttu-id="27ba2-436">To umožňuje lepší využití indexy, protože vyhledávání hello konkrétní hodnoty (tj. počet 21) je rychlejší než vyhledávání nekonečný počet potenciální odpovídá (tj. počet 21 nebo řetězce "21", "021", "21.0"...).</span><span class="sxs-lookup"><span data-stu-id="27ba2-436">This allows for a better use of indexes, because hello lookup of a specific value (i.e. number 21) is faster than search for indefinite number of potential matches (i.e. number 21 or strings "21", "021", "21.0" …).</span></span> <span data-ttu-id="27ba2-437">To se liší od způsobu JavaScript vyhodnotí operátory na hodnoty různých typů.</span><span class="sxs-lookup"><span data-stu-id="27ba2-437">This is different from how JavaScript evaluates operators on values of different types.</span></span>  
  
 <span data-ttu-id="27ba2-438">**Porovnání rovnosti pole a objekty a**</span><span class="sxs-lookup"><span data-stu-id="27ba2-438">**Arrays and objects equality and comparison**</span></span>  
  
 <span data-ttu-id="27ba2-439">Porovnání hodnot pole nebo objekt pomocí operátorů rozsah (>, > =, <, < =) bude mít za následek není definována jako není pořadí na objekt nebo pole hodnot.</span><span class="sxs-lookup"><span data-stu-id="27ba2-439">Comparing of Array or Object values using range operators (>, >=, <, <=) will result in undefined as there is not order defined on Object or Array values.</span></span> <span data-ttu-id="27ba2-440">Ale pomocí operátory rovnosti/nerovnosti (=,! =, <>) je podporována a hodnoty budou porovnány strukturálně.</span><span class="sxs-lookup"><span data-stu-id="27ba2-440">However using equality/inequality operators (=, !=, <>) is supported and values will be compared structurally.</span></span>  
  
 <span data-ttu-id="27ba2-441">Pole jsou stejné, pokud obě pole mají stejný počet elementů a prvky v odpovídajícím pozic jsou také stejné.</span><span class="sxs-lookup"><span data-stu-id="27ba2-441">Arrays are equal if both arrays have same number of elements and elements at matching positions are also equal.</span></span> <span data-ttu-id="27ba2-442">Pokud porovnávání žádném páru elementů výsledkem nedefinované, hello výsledku porovnání pole není definováno.</span><span class="sxs-lookup"><span data-stu-id="27ba2-442">If comparing any pair of elements results in undefined, hello result of array comparison is undefined.</span></span>  
  
 <span data-ttu-id="27ba2-443">Objekty jsou stejné, pokud mají oba objekty stejné vlastnosti definované a jsou také stejné hodnoty odpovídající vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="27ba2-443">Objects are equal if both objects have same properties defined, and if values of matching properties are also equal.</span></span> <span data-ttu-id="27ba2-444">Pokud porovnávání všechny dvojice hodnot vlastností výsledkem nedefinované, hello výsledku porovnání objekt není definován.</span><span class="sxs-lookup"><span data-stu-id="27ba2-444">If comparing any pair of property values results in undefined, hello result of object comparison is undefined.</span></span>  
  
##  <span data-ttu-id="27ba2-445"><a name="bk_constants"></a>Konstanty</span><span class="sxs-lookup"><span data-stu-id="27ba2-445"><a name="bk_constants"></a> Constants</span></span>  
 <span data-ttu-id="27ba2-446">Konstanta, také známé jako literál nebo skalární hodnota, je symbol, který představuje konkrétní datové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="27ba2-446">A constant, also known as a literal or a scalar value, is a symbol that represents a specific data value.</span></span> <span data-ttu-id="27ba2-447">Formát Hello konstanta závisí na typu dat hello hello hodnoty, který představuje.</span><span class="sxs-lookup"><span data-stu-id="27ba2-447">hello format of a constant depends on hello data type of hello value it represents.</span></span>  
  
 <span data-ttu-id="27ba2-448">**Podporované typy skalární data:**</span><span class="sxs-lookup"><span data-stu-id="27ba2-448">**Supported scalar data types:**</span></span>  
  
|<span data-ttu-id="27ba2-449">**Typ**</span><span class="sxs-lookup"><span data-stu-id="27ba2-449">**Type**</span></span>|<span data-ttu-id="27ba2-450">**Pořadí hodnoty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-450">**Values order**</span></span>|  
|-|-|  
|<span data-ttu-id="27ba2-451">**Nedefinovaná**</span><span class="sxs-lookup"><span data-stu-id="27ba2-451">**Undefined**</span></span>|<span data-ttu-id="27ba2-452">Jedna hodnota: **Nedefinovaná**</span><span class="sxs-lookup"><span data-stu-id="27ba2-452">Single value: **undefined**</span></span>|  
|<span data-ttu-id="27ba2-453">**Hodnotu Null**</span><span class="sxs-lookup"><span data-stu-id="27ba2-453">**Null**</span></span>|<span data-ttu-id="27ba2-454">Jedna hodnota: **hodnotu null.**</span><span class="sxs-lookup"><span data-stu-id="27ba2-454">Single value: **null**</span></span>|  
|<span data-ttu-id="27ba2-455">**Logická hodnota**</span><span class="sxs-lookup"><span data-stu-id="27ba2-455">**Boolean**</span></span>|<span data-ttu-id="27ba2-456">Hodnoty: **false**, **true**.</span><span class="sxs-lookup"><span data-stu-id="27ba2-456">Values: **false**, **true**.</span></span>|  
|<span data-ttu-id="27ba2-457">**Číslo**</span><span class="sxs-lookup"><span data-stu-id="27ba2-457">**Number**</span></span>|<span data-ttu-id="27ba2-458">Dvojitá přesnost s plovoucí desetinnou čárkou číslo, IEEE 754 standard.</span><span class="sxs-lookup"><span data-stu-id="27ba2-458">A double-precision floating-point number, IEEE 754 standard.</span></span>|  
|<span data-ttu-id="27ba2-459">**Řetězec**</span><span class="sxs-lookup"><span data-stu-id="27ba2-459">**String**</span></span>|<span data-ttu-id="27ba2-460">Pořadí nula nebo více znaků Unicode.</span><span class="sxs-lookup"><span data-stu-id="27ba2-460">A sequence of zero or more Unicode characters.</span></span> <span data-ttu-id="27ba2-461">Řetězce musí být uzavřena do jednoduchých nebo dvojitých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="27ba2-461">Strings must be enclosed in single or double quotes.</span></span>|  
|<span data-ttu-id="27ba2-462">**Pole**</span><span class="sxs-lookup"><span data-stu-id="27ba2-462">**Array**</span></span>|<span data-ttu-id="27ba2-463">Pořadí počtu nula či více elementů.</span><span class="sxs-lookup"><span data-stu-id="27ba2-463">A sequence of zero or more elements.</span></span> <span data-ttu-id="27ba2-464">Každý element může být hodnota všechny skalární datového typu, s výjimkou Undefined.</span><span class="sxs-lookup"><span data-stu-id="27ba2-464">Each element can be a value of any scalar data type, except Undefined.</span></span>|  
|<span data-ttu-id="27ba2-465">**Objekt**</span><span class="sxs-lookup"><span data-stu-id="27ba2-465">**Object**</span></span>|<span data-ttu-id="27ba2-466">Neseřazený sadu nula nebo více dvojic název hodnota.</span><span class="sxs-lookup"><span data-stu-id="27ba2-466">An unordered set of zero or more name/value pairs.</span></span> <span data-ttu-id="27ba2-467">Název je řetězec znaků Unicode, hodnota může být jakékoli skalární datového typu, s výjimkou **Undefined**.</span><span class="sxs-lookup"><span data-stu-id="27ba2-467">Name is a Unicode string, value can be of any scalar data type, except **Undefined**.</span></span>|  
  
 <span data-ttu-id="27ba2-468">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-468">**Syntax**</span></span>  
  
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
  
 <span data-ttu-id="27ba2-469">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-469">**Arguments**</span></span>  
  
1.  `<undefined_constant>; undefined`  
  
     <span data-ttu-id="27ba2-470">Představuje není definovaná hodnota typu Undefined.</span><span class="sxs-lookup"><span data-stu-id="27ba2-470">Represents undefined value of type Undefined.</span></span>  
  
2.  `<null_constant>; null`  
  
     <span data-ttu-id="27ba2-471">Představuje **null** hodnotu typu **Null**.</span><span class="sxs-lookup"><span data-stu-id="27ba2-471">Represents **null** value of type **Null**.</span></span>  
  
3.  `<boolean_constant>`  
  
     <span data-ttu-id="27ba2-472">Představuje konstanta typu Boolean.</span><span class="sxs-lookup"><span data-stu-id="27ba2-472">Represents constant of type Boolean.</span></span>  
  
4.  `false`  
  
     <span data-ttu-id="27ba2-473">Představuje **false** hodnota typu Boolean.</span><span class="sxs-lookup"><span data-stu-id="27ba2-473">Represents **false** value of type Boolean.</span></span>  
  
5.  `true`  
  
     <span data-ttu-id="27ba2-474">Představuje **true** hodnota typu Boolean.</span><span class="sxs-lookup"><span data-stu-id="27ba2-474">Represents **true** value of type Boolean.</span></span>  
  
6.  `<number_constant>`  
  
     <span data-ttu-id="27ba2-475">Představuje konstantu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-475">Represents a constant.</span></span>  
  
7.  `decimal_literal`  
  
     <span data-ttu-id="27ba2-476">Decimal literály jsou reprezentované pomocí zápisu s tečkou nebo exponenciální notace čísla.</span><span class="sxs-lookup"><span data-stu-id="27ba2-476">Decimal literals are numbers represented using either decimal notation, or scientific notation.</span></span>  
  
8.  `hexadecimal_literal`  
  
     <span data-ttu-id="27ba2-477">Šestnáctkové literály jsou reprezentované pomocí předpony '0 x, za nímž následuje jeden nebo více šestnáctkové číslice čísla.</span><span class="sxs-lookup"><span data-stu-id="27ba2-477">Hexadecimal literals are numbers represented using prefix '0x' followed by one or more hexadecimal digits.</span></span>  
  
9. `<string_constant>`  
  
     <span data-ttu-id="27ba2-478">Představuje konstantu typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="27ba2-478">Represents a constant of type String.</span></span>  
  
10. `string _literal`  
  
     <span data-ttu-id="27ba2-479">Textové literály jsou reprezentována posloupnost nula nebo více znaků Unicode nebo řídicí sekvence řetězců v kódu Unicode.</span><span class="sxs-lookup"><span data-stu-id="27ba2-479">String literals are Unicode strings represented by a sequence of zero or more Unicode characters or escape sequences.</span></span> <span data-ttu-id="27ba2-480">Textové literály jsou uzavřená do jednoduchých uvozovek (apostrof: ') nebo dvojité uvozovky (uvozovky: ").</span><span class="sxs-lookup"><span data-stu-id="27ba2-480">String literals are enclosed in single quotes (apostrophe: ' ) or double quotes (quotation mark: ").</span></span>  
  
 <span data-ttu-id="27ba2-481">Jsou povoleny následující řídicí sekvence:</span><span class="sxs-lookup"><span data-stu-id="27ba2-481">Following escape sequences are allowed:</span></span>  
  
|<span data-ttu-id="27ba2-482">**Řídicí sekvence**</span><span class="sxs-lookup"><span data-stu-id="27ba2-482">**Escape sequence**</span></span>|<span data-ttu-id="27ba2-483">**Popis**</span><span class="sxs-lookup"><span data-stu-id="27ba2-483">**Description**</span></span>|<span data-ttu-id="27ba2-484">**Znak Unicode**</span><span class="sxs-lookup"><span data-stu-id="27ba2-484">**Unicode character**</span></span>|  
|-|-|-|  
|<span data-ttu-id="27ba2-485">\\'</span><span class="sxs-lookup"><span data-stu-id="27ba2-485">\\'</span></span>|<span data-ttu-id="27ba2-486">apostrof (')</span><span class="sxs-lookup"><span data-stu-id="27ba2-486">apostrophe (')</span></span>|<span data-ttu-id="27ba2-487">U + 0027</span><span class="sxs-lookup"><span data-stu-id="27ba2-487">U+0027</span></span>|  
|<span data-ttu-id="27ba2-488">\\"</span><span class="sxs-lookup"><span data-stu-id="27ba2-488">\\"</span></span>|<span data-ttu-id="27ba2-489">dvojité uvozovky (")</span><span class="sxs-lookup"><span data-stu-id="27ba2-489">quotation mark (")</span></span>|<span data-ttu-id="27ba2-490">U + 0022</span><span class="sxs-lookup"><span data-stu-id="27ba2-490">U+0022</span></span>|  
|\\\|<span data-ttu-id="27ba2-491">obrácené lomítko (\\)</span><span class="sxs-lookup"><span data-stu-id="27ba2-491">reverse solidus (\\)</span></span>|<span data-ttu-id="27ba2-492">U + 005C</span><span class="sxs-lookup"><span data-stu-id="27ba2-492">U+005C</span></span>|  
|\\/|<span data-ttu-id="27ba2-493">lomítko (/)</span><span class="sxs-lookup"><span data-stu-id="27ba2-493">solidus (/)</span></span>|<span data-ttu-id="27ba2-494">U + 002F</span><span class="sxs-lookup"><span data-stu-id="27ba2-494">U+002F</span></span>|  
|<span data-ttu-id="27ba2-495">\b</span><span class="sxs-lookup"><span data-stu-id="27ba2-495">\b</span></span>|<span data-ttu-id="27ba2-496">BACKSPACE</span><span class="sxs-lookup"><span data-stu-id="27ba2-496">backspace</span></span>|<span data-ttu-id="27ba2-497">U + 0008</span><span class="sxs-lookup"><span data-stu-id="27ba2-497">U+0008</span></span>|  
|<span data-ttu-id="27ba2-498">\f</span><span class="sxs-lookup"><span data-stu-id="27ba2-498">\f</span></span>|<span data-ttu-id="27ba2-499">řídicí znak</span><span class="sxs-lookup"><span data-stu-id="27ba2-499">form feed</span></span>|<span data-ttu-id="27ba2-500">U + 000C</span><span class="sxs-lookup"><span data-stu-id="27ba2-500">U+000C</span></span>|  
|\n|<span data-ttu-id="27ba2-501">Přechod na nový řádek</span><span class="sxs-lookup"><span data-stu-id="27ba2-501">line feed</span></span>|<span data-ttu-id="27ba2-502">U + 000A</span><span class="sxs-lookup"><span data-stu-id="27ba2-502">U+000A</span></span>|  
|<span data-ttu-id="27ba2-503">\r</span><span class="sxs-lookup"><span data-stu-id="27ba2-503">\r</span></span>|<span data-ttu-id="27ba2-504">Návrat na začátek</span><span class="sxs-lookup"><span data-stu-id="27ba2-504">carriage return</span></span>|<span data-ttu-id="27ba2-505">U + 000D</span><span class="sxs-lookup"><span data-stu-id="27ba2-505">U+000D</span></span>|  
|<span data-ttu-id="27ba2-506">\t</span><span class="sxs-lookup"><span data-stu-id="27ba2-506">\t</span></span>|<span data-ttu-id="27ba2-507">Karta</span><span class="sxs-lookup"><span data-stu-id="27ba2-507">tab</span></span>|<span data-ttu-id="27ba2-508">U + 0009</span><span class="sxs-lookup"><span data-stu-id="27ba2-508">U+0009</span></span>|  
|<span data-ttu-id="27ba2-509">\uXXXX</span><span class="sxs-lookup"><span data-stu-id="27ba2-509">\uXXXX</span></span>|<span data-ttu-id="27ba2-510">Znak Unicode definované 4 hexadecimální číslice.</span><span class="sxs-lookup"><span data-stu-id="27ba2-510">A Unicode character defined by 4 hexadecimal digits.</span></span>|<span data-ttu-id="27ba2-511">U + XXXX</span><span class="sxs-lookup"><span data-stu-id="27ba2-511">U+XXXX</span></span>|  
  
##  <span data-ttu-id="27ba2-512"><a name="bk_query_perf_guidelines"></a>Pravidla výkonu dotazu</span><span class="sxs-lookup"><span data-stu-id="27ba2-512"><a name="bk_query_perf_guidelines"></a> Query performance guidelines</span></span>  
 <span data-ttu-id="27ba2-513">Aby dotaz toobe, efektivně provést pro velké kolekce měla by používat filtry, které je možné dodávat prostřednictvím jednoho nebo více indexů.</span><span class="sxs-lookup"><span data-stu-id="27ba2-513">In order for a query toobe executed efficiently for a large collection, it should use filters which can be served through one or more indexes.</span></span>  
  
 <span data-ttu-id="27ba2-514">Následující filtry Hello se bude zvažovat indexu vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="27ba2-514">hello following filters will be considered for index lookup:</span></span>  
  
-   <span data-ttu-id="27ba2-515">Operátor rovnosti (=) pomocí výrazu cesty dokumentu a konstanta.</span><span class="sxs-lookup"><span data-stu-id="27ba2-515">Use equality operator ( = ) with a document path expression and a constant.</span></span>  
  
-   <span data-ttu-id="27ba2-516">Rozsah operátory (<, \<=, >, > =) s výraz cesty dokumentu a číselné konstanty.</span><span class="sxs-lookup"><span data-stu-id="27ba2-516">Use range operators (<, \<=, >, >=) with a document path expression and number constants.</span></span>  
  
-   <span data-ttu-id="27ba2-517">Výraz cesty dokumentu zastupuje všechny výraz, který identifikuje konstantní cestu v dokumentech hello z kolekce hello odkazovaná databáze.</span><span class="sxs-lookup"><span data-stu-id="27ba2-517">Document path expression stands for any expression which identifies a constant path in hello documents from hello referenced database collection.</span></span>  
  
 <span data-ttu-id="27ba2-518">**Výraz cesty dokumentu**</span><span class="sxs-lookup"><span data-stu-id="27ba2-518">**Document path expression**</span></span>  
  
 <span data-ttu-id="27ba2-519">Výrazy cesty dokumentu jsou výrazy, cesta vlastnost nebo pole posuzovatelů indexer přes dokumentu pocházejících z databáze kolekce dokumentů.</span><span class="sxs-lookup"><span data-stu-id="27ba2-519">Document path expressions are expressions that a path of property or array indexer assessors over a document coming from database collection documents.</span></span> <span data-ttu-id="27ba2-520">Tato cesta může být použité tooidentify hello umístění hodnot odkazuje ve filtru přímo v rámci hello dokumenty v kolekci hello databáze.</span><span class="sxs-lookup"><span data-stu-id="27ba2-520">This path can be used tooidentify hello location of values referenced in a filter directly within hello documents in hello database collection.</span></span>  
  
 <span data-ttu-id="27ba2-521">Pro výrazu toobe považována za výraz cesty dokumentu by měl:</span><span class="sxs-lookup"><span data-stu-id="27ba2-521">For an expression toobe considered a document path expression, it should:</span></span>  
  
1.  <span data-ttu-id="27ba2-522">Referenční dokumentace hello kolekce root přímo.</span><span class="sxs-lookup"><span data-stu-id="27ba2-522">Reference hello collection root directly.</span></span>  
  
2.  <span data-ttu-id="27ba2-523">Odkaz na vlastnost nebo konstanta indexer pole některé výrazu cesta dokumentu</span><span class="sxs-lookup"><span data-stu-id="27ba2-523">Reference property or constant array indexer of some document path expression</span></span>  
  
3.  <span data-ttu-id="27ba2-524">Referenční alias, který představuje výraz cesty některé dokumentu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-524">Reference an alias, which represents some document path expression.</span></span>  
  
     <span data-ttu-id="27ba2-525">**Syntaxe konvence**</span><span class="sxs-lookup"><span data-stu-id="27ba2-525">**Syntax conventions**</span></span>  
  
     <span data-ttu-id="27ba2-526">Hello následující tabulka popisuje syntaxi toodescribe hello konvence použité v referenci hello dotazovací jazyk DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="27ba2-526">hello following table describes hello conventions used toodescribe syntax in hello DocumentDB API Query Language reference.</span></span>  
  
    |<span data-ttu-id="27ba2-527">**Konvence**</span><span class="sxs-lookup"><span data-stu-id="27ba2-527">**Convention**</span></span>|<span data-ttu-id="27ba2-528">**Použít pro**</span><span class="sxs-lookup"><span data-stu-id="27ba2-528">**Used for**</span></span>|  
    |-|-|    
    |<span data-ttu-id="27ba2-529">VELKÁ PÍSMENA</span><span class="sxs-lookup"><span data-stu-id="27ba2-529">UPPERCASE</span></span>|<span data-ttu-id="27ba2-530">Klíčová slova velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="27ba2-530">Case-insensitive keywords.</span></span>|  
    |<span data-ttu-id="27ba2-531">malá písmena</span><span class="sxs-lookup"><span data-stu-id="27ba2-531">lowercase</span></span>|<span data-ttu-id="27ba2-532">Klíčová slova malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="27ba2-532">Case-sensitive keywords.</span></span>|  
    |<span data-ttu-id="27ba2-533">\<nonterminal ></span><span class="sxs-lookup"><span data-stu-id="27ba2-533">\<nonterminal></span></span>|<span data-ttu-id="27ba2-534">Nonterminal, definované samostatně.</span><span class="sxs-lookup"><span data-stu-id="27ba2-534">Nonterminal, defined separately.</span></span>|  
    |<span data-ttu-id="27ba2-535">\<nonterminal >:: =</span><span class="sxs-lookup"><span data-stu-id="27ba2-535">\<nonterminal> ::=</span></span>|<span data-ttu-id="27ba2-536">Syntaxe definice hello nonterminal.</span><span class="sxs-lookup"><span data-stu-id="27ba2-536">Syntax definition of hello nonterminal.</span></span>|  
    |<span data-ttu-id="27ba2-537">other_terminal</span><span class="sxs-lookup"><span data-stu-id="27ba2-537">other_terminal</span></span>|<span data-ttu-id="27ba2-538">Terminálové (token), podrobně popsané v slova.</span><span class="sxs-lookup"><span data-stu-id="27ba2-538">Terminal (token), described in detail in words.</span></span>|  
    |<span data-ttu-id="27ba2-539">Identifikátor</span><span class="sxs-lookup"><span data-stu-id="27ba2-539">identifier</span></span>|<span data-ttu-id="27ba2-540">Identifikátor.</span><span class="sxs-lookup"><span data-stu-id="27ba2-540">Identifier.</span></span> <span data-ttu-id="27ba2-541">Umožňuje následující pouze znaky: a – z A-Z 0-9 _First znak nemůže být číslice.</span><span class="sxs-lookup"><span data-stu-id="27ba2-541">Allows following characters only: a-z A-Z 0-9 _First character cannot be a digit.</span></span>|  
    |<span data-ttu-id="27ba2-542">"řetězec"</span><span class="sxs-lookup"><span data-stu-id="27ba2-542">"string"</span></span>|<span data-ttu-id="27ba2-543">Řetězec v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="27ba2-543">Quoted string.</span></span> <span data-ttu-id="27ba2-544">Umožňuje libovolný platný řetězec.</span><span class="sxs-lookup"><span data-stu-id="27ba2-544">Allows any valid string.</span></span> <span data-ttu-id="27ba2-545">Viz popis string_literal.</span><span class="sxs-lookup"><span data-stu-id="27ba2-545">See description of a string_literal.</span></span>|  
    |<span data-ttu-id="27ba2-546">'symbol.</span><span class="sxs-lookup"><span data-stu-id="27ba2-546">'symbol'</span></span>|<span data-ttu-id="27ba2-547">Literál symbol, který je součástí hello syntaxe.</span><span class="sxs-lookup"><span data-stu-id="27ba2-547">Literal symbol which is part of hello syntax.</span></span>|  
    |<span data-ttu-id="27ba2-548">&#124; (svislé čáry)</span><span class="sxs-lookup"><span data-stu-id="27ba2-548">&#124; (vertical bar)</span></span>|<span data-ttu-id="27ba2-549">Alternativy pro položky syntaxe.</span><span class="sxs-lookup"><span data-stu-id="27ba2-549">Alternatives for syntax items.</span></span> <span data-ttu-id="27ba2-550">Můžete použít pouze jednu z položky hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-550">You can use only one of hello items specified.</span></span>|  
    |<span data-ttu-id="27ba2-551">[] /(brackets)</span><span class="sxs-lookup"><span data-stu-id="27ba2-551">[ ] /(brackets)</span></span>|<span data-ttu-id="27ba2-552">Závorky uzavřete jeden nebo více nepovinných položek.</span><span class="sxs-lookup"><span data-stu-id="27ba2-552">Brackets enclose one or more optional items.</span></span>|  
    |<span data-ttu-id="27ba2-553">[,.. .n]</span><span class="sxs-lookup"><span data-stu-id="27ba2-553">[ ,...n ]</span></span>|<span data-ttu-id="27ba2-554">Označuje, že hello předcházející položky může být opakovaný n stanovený počet.</span><span class="sxs-lookup"><span data-stu-id="27ba2-554">Indicates hello preceding item can be repeated n number of times.</span></span> <span data-ttu-id="27ba2-555">Hello výskytů jsou oddělené čárkami.</span><span class="sxs-lookup"><span data-stu-id="27ba2-555">hello occurrences are separated by commas.</span></span>|  
    |<span data-ttu-id="27ba2-556">[.. .n]</span><span class="sxs-lookup"><span data-stu-id="27ba2-556">[ ...n ]</span></span>|<span data-ttu-id="27ba2-557">Označuje, že hello předcházející položky může být opakovaný n stanovený počet.</span><span class="sxs-lookup"><span data-stu-id="27ba2-557">Indicates hello preceding item can be repeated n number of times.</span></span> <span data-ttu-id="27ba2-558">Hello výskytů se oddělují mezerami.</span><span class="sxs-lookup"><span data-stu-id="27ba2-558">hello occurrences are separated by blanks.</span></span>|  
  
##  <span data-ttu-id="27ba2-559"><a name="bk_built_in_functions"></a>Integrované funkce</span><span class="sxs-lookup"><span data-stu-id="27ba2-559"><a name="bk_built_in_functions"></a> Built-in functions</span></span>  
 <span data-ttu-id="27ba2-560">Azure Cosmos DB poskytuje mnoho předdefinovaných funkcí SQL.</span><span class="sxs-lookup"><span data-stu-id="27ba2-560">Azure Cosmos DB provides many built-in SQL functions.</span></span> <span data-ttu-id="27ba2-561">kategorie Hello integrované funkce jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="27ba2-561">hello categories of built-in functions are listed below.</span></span>  
  
|<span data-ttu-id="27ba2-562">Funkce</span><span class="sxs-lookup"><span data-stu-id="27ba2-562">Function</span></span>|<span data-ttu-id="27ba2-563">Popis</span><span class="sxs-lookup"><span data-stu-id="27ba2-563">Description</span></span>|  
|--------------|-----------------|  
|[<span data-ttu-id="27ba2-564">Matematické funkce</span><span class="sxs-lookup"><span data-stu-id="27ba2-564">Mathematical functions</span></span>](#bk_mathematical_functions)|<span data-ttu-id="27ba2-565">Hello matematické funkce každé provedení výpočtů, obvykle podle vstupní hodnoty, které jsou k dispozici jako argumenty a vrátí číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-565">hello mathematical functions each perform a calculation, usually based on input values that are provided as arguments, and return a numeric value.</span></span>|  
|[<span data-ttu-id="27ba2-566">Typ kontroly funkce</span><span class="sxs-lookup"><span data-stu-id="27ba2-566">Type checking functions</span></span>](#bk_type_checking_functions)|<span data-ttu-id="27ba2-567">Kontrola, zda Funkce Hello typu Povolit toocheck hello typ výrazu v rámci dotazů SQL.</span><span class="sxs-lookup"><span data-stu-id="27ba2-567">hello type checking functions allow you toocheck hello type of an expression within SQL queries.</span></span>|  
|[<span data-ttu-id="27ba2-568">Funkce řetězců</span><span class="sxs-lookup"><span data-stu-id="27ba2-568">String functions</span></span>](#bk_string_functions)|<span data-ttu-id="27ba2-569">Funkce řetězce Hello provést operaci s vstupní hodnotu řetězce a vrátí řetězec, číselnou nebo logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="27ba2-569">hello string functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span>|  
|[<span data-ttu-id="27ba2-570">Funkce pole</span><span class="sxs-lookup"><span data-stu-id="27ba2-570">Array functions</span></span>](#bk_array_functions)|<span data-ttu-id="27ba2-571">Funkce pole Hello provést operaci na hodnotu vstupní pole a vrátí číselnou, logická hodnota nebo pole hodnota.</span><span class="sxs-lookup"><span data-stu-id="27ba2-571">hello array functions perform an operation on an array input value and return numeric, Boolean or array value.</span></span>|  
|[<span data-ttu-id="27ba2-572">Prostorové funkce</span><span class="sxs-lookup"><span data-stu-id="27ba2-572">Spatial functions</span></span>](#bk_spatial_functions)|<span data-ttu-id="27ba2-573">Funkce prostorových Hello provést operaci s vstupní hodnotu prostorový objekt a vrátit hodnotu číselná nebo logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="27ba2-573">hello spatial functions perform an operation on an spatial object input value and return a numeric or Boolean value.</span></span>|  
  
###  <span data-ttu-id="27ba2-574"><a name="bk_mathematical_functions"></a>Matematické funkce</span><span class="sxs-lookup"><span data-stu-id="27ba2-574"><a name="bk_mathematical_functions"></a> Mathematical functions</span></span>  
 <span data-ttu-id="27ba2-575">Hello následující funkce provedení výpočtů, obvykle podle vstupní hodnoty, které jsou k dispozici jako argumenty a vrátí číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-575">hello following functions each perform a calculation, usually based on input values that are provided as arguments, and return a numeric value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="27ba2-576">ABS</span><span class="sxs-lookup"><span data-stu-id="27ba2-576">ABS</span></span>](#bk_abs)|[<span data-ttu-id="27ba2-577">ACOS</span><span class="sxs-lookup"><span data-stu-id="27ba2-577">ACOS</span></span>](#bk_acos)|[<span data-ttu-id="27ba2-578">ASIN</span><span class="sxs-lookup"><span data-stu-id="27ba2-578">ASIN</span></span>](#bk_asin)|  
|[<span data-ttu-id="27ba2-579">ATAN</span><span class="sxs-lookup"><span data-stu-id="27ba2-579">ATAN</span></span>](#bk_atan)|[<span data-ttu-id="27ba2-580">ATN2</span><span class="sxs-lookup"><span data-stu-id="27ba2-580">ATN2</span></span>](#bk_atn2)|[<span data-ttu-id="27ba2-581">MEZNÍ HODNOTY</span><span class="sxs-lookup"><span data-stu-id="27ba2-581">CEILING</span></span>](#bk_ceiling)|  
|[<span data-ttu-id="27ba2-582">COS</span><span class="sxs-lookup"><span data-stu-id="27ba2-582">COS</span></span>](#bk_cos)|[<span data-ttu-id="27ba2-583">COP</span><span class="sxs-lookup"><span data-stu-id="27ba2-583">COT</span></span>](#bk_cot)|[<span data-ttu-id="27ba2-584">STUPŇŮ</span><span class="sxs-lookup"><span data-stu-id="27ba2-584">DEGREES</span></span>](#bk_degrees)|  
|[<span data-ttu-id="27ba2-585">EXP</span><span class="sxs-lookup"><span data-stu-id="27ba2-585">EXP</span></span>](#bk_exp)|[<span data-ttu-id="27ba2-586">FLOOR</span><span class="sxs-lookup"><span data-stu-id="27ba2-586">FLOOR</span></span>](#bk_floor)|[<span data-ttu-id="27ba2-587">PROTOKOLU</span><span class="sxs-lookup"><span data-stu-id="27ba2-587">LOG</span></span>](#bk_log)|  
|[<span data-ttu-id="27ba2-588">LOG10</span><span class="sxs-lookup"><span data-stu-id="27ba2-588">LOG10</span></span>](#bk_log10)|[<span data-ttu-id="27ba2-589">PLATFORMY</span><span class="sxs-lookup"><span data-stu-id="27ba2-589">PI</span></span>](#bk_pi)|[<span data-ttu-id="27ba2-590">NAPÁJENÍ</span><span class="sxs-lookup"><span data-stu-id="27ba2-590">POWER</span></span>](#bk_power)|  
|[<span data-ttu-id="27ba2-591">RADIÁNECH</span><span class="sxs-lookup"><span data-stu-id="27ba2-591">RADIANS</span></span>](#bk_radians)|[<span data-ttu-id="27ba2-592">ZAOKROUHLIT</span><span class="sxs-lookup"><span data-stu-id="27ba2-592">ROUND</span></span>](#bk_round)|[<span data-ttu-id="27ba2-593">SIN</span><span class="sxs-lookup"><span data-stu-id="27ba2-593">SIN</span></span>](#bk_sin)|  
|[<span data-ttu-id="27ba2-594">SQRT</span><span class="sxs-lookup"><span data-stu-id="27ba2-594">SQRT</span></span>](#bk_sqrt)|[<span data-ttu-id="27ba2-595">HRANATÉ</span><span class="sxs-lookup"><span data-stu-id="27ba2-595">SQUARE</span></span>](#bk_square)|[<span data-ttu-id="27ba2-596">PŘIHLÁŠENÍ</span><span class="sxs-lookup"><span data-stu-id="27ba2-596">SIGN</span></span>](#bk_sign)|  
|[<span data-ttu-id="27ba2-597">TAN</span><span class="sxs-lookup"><span data-stu-id="27ba2-597">TAN</span></span>](#bk_tan)|[<span data-ttu-id="27ba2-598">TRUNC</span><span class="sxs-lookup"><span data-stu-id="27ba2-598">TRUNC</span></span>](#bk_trunc)||  
  
####  <span data-ttu-id="27ba2-599"><a name="bk_abs"></a>ABS</span><span class="sxs-lookup"><span data-stu-id="27ba2-599"><a name="bk_abs"></a> ABS</span></span>  
 <span data-ttu-id="27ba2-600">Vrátí hello absolutní (kladné) hello zadána hodnota číselného výrazu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-600">Returns hello absolute (positive) value of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-601">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-601">**Syntax**</span></span>  
  
```  
ABS (<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-602">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-602">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-603">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-603">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-604">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-604">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-605">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-605">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-606">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-606">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-607">Hello následující příklad ukazuje výsledky hello pomocí funkce hello ABS na tři odlišné počty.</span><span class="sxs-lookup"><span data-stu-id="27ba2-607">hello following example shows hello results of using hello ABS function on three different numbers.</span></span>  
  
```  
SELECT ABS(-1), ABS(0), ABS(1)  
```  
  
 <span data-ttu-id="27ba2-608">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-608">Here is hello result set.</span></span>  
  
```  
[{$1: 1, $2: 0, $3: 1}]  
```  
  
####  <span data-ttu-id="27ba2-609"><a name="bk_acos"></a>ACOS</span><span class="sxs-lookup"><span data-stu-id="27ba2-609"><a name="bk_acos"></a> ACOS</span></span>  
 <span data-ttu-id="27ba2-610">Vrátí hello úhel v radiánech, jehož kosinus je hello zadaný číselného výrazu; Zkratka Arkus.</span><span class="sxs-lookup"><span data-stu-id="27ba2-610">Returns hello angle, in radians, whose cosine is hello specified numeric expression; also called arccosine.</span></span>  
  
 <span data-ttu-id="27ba2-611">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-611">**Syntax**</span></span>  
  
```  
ACOS(<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-612">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-612">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-613">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-613">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-614">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-614">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-615">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-615">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-616">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-616">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-617">Hello následující příklad vrátí hello ACOS-1.</span><span class="sxs-lookup"><span data-stu-id="27ba2-617">hello following example returns hello ACOS of -1.</span></span>  
  
```  
SELECT ACOS(-1)  
```  
  
 <span data-ttu-id="27ba2-618">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-618">Here is hello result set.</span></span>  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <span data-ttu-id="27ba2-619"><a name="bk_asin"></a>ASIN</span><span class="sxs-lookup"><span data-stu-id="27ba2-619"><a name="bk_asin"></a> ASIN</span></span>  
 <span data-ttu-id="27ba2-620">Vrátí hello úhlu, v radiánech, jehož sinus je hello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-620">Returns hello angle, in radians, whose sine is hello specified numeric expression.</span></span> <span data-ttu-id="27ba2-621">To je také označován Arkus sinus.</span><span class="sxs-lookup"><span data-stu-id="27ba2-621">This is also called arcsine.</span></span>  
  
 <span data-ttu-id="27ba2-622">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-622">**Syntax**</span></span>  
  
```  
ASIN(<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-623">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-623">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-624">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-624">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-625">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-625">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-626">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-626">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-627">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-627">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-628">Hello následující příklad vrátí hello ASIN-1.</span><span class="sxs-lookup"><span data-stu-id="27ba2-628">hello following example returns hello ASIN of -1.</span></span>  
  
```  
SELECT ASIN(-1)  
```  
  
 <span data-ttu-id="27ba2-629">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-629">Here is hello result set.</span></span>  
  
```  
[{"$1": -1.5707963267948966}]  
```  
  
####  <span data-ttu-id="27ba2-630"><a name="bk_atan"></a>ATAN</span><span class="sxs-lookup"><span data-stu-id="27ba2-630"><a name="bk_atan"></a> ATAN</span></span>  
 <span data-ttu-id="27ba2-631">Vrátí hello úhlu, v radiánech, jehož tangens je hello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-631">Returns hello angle, in radians, whose tangent is hello specified numeric expression.</span></span> <span data-ttu-id="27ba2-632">To je také označován Arkus.</span><span class="sxs-lookup"><span data-stu-id="27ba2-632">This is also called arctangent.</span></span>  
  
 <span data-ttu-id="27ba2-633">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-633">**Syntax**</span></span>  
  
```  
ATAN(<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-634">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-634">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-635">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-635">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-636">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-636">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-637">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-637">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-638">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-638">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-639">Následující příklad, vrátí hello ATAN hello Hello zadané hodnotě.</span><span class="sxs-lookup"><span data-stu-id="27ba2-639">hello following example returns hello ATAN of hello specified value.</span></span>  
  
```  
SELECT ATAN(-45.01)  
```  
  
 <span data-ttu-id="27ba2-640">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-640">Here is hello result set.</span></span>  
  
```  
[{"$1": -1.5485826962062663}]  
```  
  
####  <span data-ttu-id="27ba2-641"><a name="bk_atn2"></a>ATN2</span><span class="sxs-lookup"><span data-stu-id="27ba2-641"><a name="bk_atn2"></a> ATN2</span></span>  
 <span data-ttu-id="27ba2-642">Vrací hlavní hodnotu hello hello tangens oblouk y / x, vyjádřené v radiánech.</span><span class="sxs-lookup"><span data-stu-id="27ba2-642">Returns hello principal value of hello arc tangent of y/x, expressed in radians.</span></span>  
  
 <span data-ttu-id="27ba2-643">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-643">**Syntax**</span></span>  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-644">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-644">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-645">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-645">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-646">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-646">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-647">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-647">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-648">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-648">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-649">Hello následující příklad vypočítá hello ATN2 pro zadaný hello x a y součásti.</span><span class="sxs-lookup"><span data-stu-id="27ba2-649">hello following example calculates hello ATN2 for hello specified x and y components.</span></span>  
  
```  
SELECT ATN2(35.175643, 129.44)  
```  
  
 <span data-ttu-id="27ba2-650">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-650">Here is hello result set.</span></span>  
  
```  
[{"$1": 1.3054517947300646}]  
```  
  
####  <span data-ttu-id="27ba2-651"><a name="bk_ceiling"></a>MEZNÍ HODNOTY</span><span class="sxs-lookup"><span data-stu-id="27ba2-651"><a name="bk_ceiling"></a> CEILING</span></span>  
 <span data-ttu-id="27ba2-652">Vrátí hello nejmenší celé číslo větší než nebo rovno, hello zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-652">Returns hello smallest integer value greater than, or equal to, hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-653">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-653">**Syntax**</span></span>  
  
```  
CEILING (<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-654">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-654">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-655">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-655">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-656">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-656">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-657">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-657">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-658">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-658">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-659">Hello následující příklad ukazuje číselné kladné a záporné, a nulové hodnoty s hello funkce mezní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="27ba2-659">hello following example shows positive numeric, negative, and zero values with hello CEILING function.</span></span>  
  
```  
SELECT CEILING(123.45), CEILING(-123.45), CEILING(0.0)  
```  
  
 <span data-ttu-id="27ba2-660">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-660">Here is hello result set.</span></span>  
  
```  
[{$1: 124, $2: -123, $3: 0}]  
```  
  
####  <span data-ttu-id="27ba2-661"><a name="bk_cos"></a>COS</span><span class="sxs-lookup"><span data-stu-id="27ba2-661"><a name="bk_cos"></a> COS</span></span>  
 <span data-ttu-id="27ba2-662">Vrátí hello trigonometrické kosinus hello úhlu, uvedeného v radiánech, v hello zadaný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-662">Returns hello trigonometric cosine of hello specified angle, in radians, in hello specified expression.</span></span>  
  
 <span data-ttu-id="27ba2-663">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-663">**Syntax**</span></span>  
  
```  
COS(<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-664">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-664">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-665">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-665">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-666">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-666">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-667">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-667">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-668">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-668">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-669">Následující ukázka Hello vypočítá hello COS z hello zadaný úhlu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-669">hello following example calculates hello COS of hello specified angle.</span></span>  
  
```  
SELECT COS(14.78)  
```  
  
 <span data-ttu-id="27ba2-670">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-670">Here is hello result set.</span></span>  
  
```  
[{"$1": -0.59946542619465426}]  
```  
  
####  <span data-ttu-id="27ba2-671"><a name="bk_cot"></a>COP</span><span class="sxs-lookup"><span data-stu-id="27ba2-671"><a name="bk_cot"></a> COT</span></span>  
 <span data-ttu-id="27ba2-672">Vrátí hello trigonometrické kotangens hello úhlu, uvedeného v radiánech, v hello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-672">Returns hello trigonometric cotangent of hello specified angle, in radians, in hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-673">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-673">**Syntax**</span></span>  
  
```  
COT(<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-674">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-674">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-675">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-675">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-676">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-676">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-677">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-677">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-678">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-678">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-679">Následující ukázka Hello vypočítá hello COP hello určeného úhlu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-679">hello following example calculates hello COT of hello specified angle.</span></span>  
  
```  
SELECT COT(124.1332)  
```  
  
 <span data-ttu-id="27ba2-680">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-680">Here is hello result set.</span></span>  
  
```  
[{"$1": -0.040311998371148884}]  
```  
  
####  <span data-ttu-id="27ba2-681"><a name="bk_degrees"></a>STUPŇŮ</span><span class="sxs-lookup"><span data-stu-id="27ba2-681"><a name="bk_degrees"></a> DEGREES</span></span>  
 <span data-ttu-id="27ba2-682">Vrátí hello odpovídající úhel ve stupních pro úhlu uvedeného v radiánech.</span><span class="sxs-lookup"><span data-stu-id="27ba2-682">Returns hello corresponding angle in degrees for an angle specified in radians.</span></span>  
  
 <span data-ttu-id="27ba2-683">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-683">**Syntax**</span></span>  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-684">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-684">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-685">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-685">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-686">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-686">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-687">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-687">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-688">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-688">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-689">Hello následující příklad vrátí hello počet stupňů v úhel radiánech PÍ/2.</span><span class="sxs-lookup"><span data-stu-id="27ba2-689">hello following example returns hello number of degrees in an angle of PI/2 radians.</span></span>  
  
```  
SELECT DEGREES(PI()/2)  
```  
  
 <span data-ttu-id="27ba2-690">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-690">Here is hello result set.</span></span>  
  
```  
[{"$1": 90}]  
```  
  
####  <span data-ttu-id="27ba2-691"><a name="bk_floor"></a>FLOOR</span><span class="sxs-lookup"><span data-stu-id="27ba2-691"><a name="bk_floor"></a> FLOOR</span></span>  
 <span data-ttu-id="27ba2-692">Vrátí hello největší celé číslo menší než nebo rovna toohello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-692">Returns hello largest integer less than or equal toohello specified numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-693">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-693">**Syntax**</span></span>  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-694">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-694">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-695">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-695">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-696">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-696">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-697">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-697">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-698">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-698">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-699">Hello následující příklad ukazuje číselné kladné a záporné, a nulové hodnoty s hello FLOOR – funkce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-699">hello following example shows positive numeric, negative, and zero values with hello FLOOR function.</span></span>  
  
```  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR(0.0)  
```  
  
 <span data-ttu-id="27ba2-700">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-700">Here is hello result set.</span></span>  
  
```  
[{$1: 123, $2: -124, $3: 0}]  
```  
  
####  <span data-ttu-id="27ba2-701"><a name="bk_exp"></a>EXP</span><span class="sxs-lookup"><span data-stu-id="27ba2-701"><a name="bk_exp"></a> EXP</span></span>  
 <span data-ttu-id="27ba2-702">Vrátí hodnotu exponenciálního hello Dobrý den zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-702">Returns hello exponential value of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-703">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-703">**Syntax**</span></span>  
  
```  
EXP (<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-704">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-704">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-705">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-705">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-706">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-706">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-707">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-707">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-708">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="27ba2-708">**Remarks**</span></span>  
  
 <span data-ttu-id="27ba2-709">Konstanta Hello **e** (2.718281...), je hello základní přirozeného logaritmu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-709">hello constant **e** (2.718281…), is hello base of natural logarithms.</span></span>  
  
 <span data-ttu-id="27ba2-710">Hello exponent čísla je hello konstanta **e** vyvolá toohello power hello čísla.</span><span class="sxs-lookup"><span data-stu-id="27ba2-710">hello exponent of a number is hello constant **e** raised toohello power of hello number.</span></span> <span data-ttu-id="27ba2-711">Například EXP(1.0) = e ^ 1.0 = 2.71828182845905 a EXP(10) = e ^ 10 = 22026.4657948067.</span><span class="sxs-lookup"><span data-stu-id="27ba2-711">For example EXP(1.0) = e^1.0 = 2.71828182845905 and EXP(10) = e^10 = 22026.4657948067.</span></span>  
  
 <span data-ttu-id="27ba2-712">Hello exponent hello přirozený logaritmus čísla je číslo hello, samotné: EXP (protokol (ne)) = n.</span><span class="sxs-lookup"><span data-stu-id="27ba2-712">hello exponential of hello natural logarithm of a number is hello number itself: EXP (LOG (n)) = n.</span></span> <span data-ttu-id="27ba2-713">A hello přirozený logaritmus čísla exponenciální hello je hello číslo samotné: protokolu (EXP (ne)) = n.</span><span class="sxs-lookup"><span data-stu-id="27ba2-713">And hello natural logarithm of hello exponential of a number is hello number itself: LOG (EXP (n)) = n.</span></span>  
  
 <span data-ttu-id="27ba2-714">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-714">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-715">Hello následující příklad deklaruje proměnné a vrátí hodnotu exponenciálního hello hello zadanou proměnnou (10).</span><span class="sxs-lookup"><span data-stu-id="27ba2-715">hello following example declares a variable and returns hello exponential value of hello specified variable (10).</span></span>  
  
```  
SELECT EXP(10)  
```  
  
 <span data-ttu-id="27ba2-716">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-716">Here is hello result set.</span></span>  
  
```  
[{$1: 22026.465794806718}]  
```  
  
 <span data-ttu-id="27ba2-717">Hello následující příklad vrátí hodnotu exponenciálního hello hello přirozený logaritmus 20 a hello přirozený logaritmus hello exponenciální 20.</span><span class="sxs-lookup"><span data-stu-id="27ba2-717">hello following example returns hello exponential value of hello natural logarithm of 20 and hello natural logarithm of hello exponential of 20.</span></span> <span data-ttu-id="27ba2-718">Protože tyto funkce jsou inverzní funkce vzájemně, hello návratovou hodnotu s zaokrouhlení hodnoty pro číslo s plovoucí desetinnou matematické v obou případech je 20.</span><span class="sxs-lookup"><span data-stu-id="27ba2-718">Because these functions are inverse functions of one another, hello return value with rounding for floating point math in both cases is 20.</span></span>  
  
```  
SELECT EXP(LOG(20)), LOG(EXP(20))  
```  
  
 <span data-ttu-id="27ba2-719">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-719">Here is hello result set.</span></span>  
  
```  
[{$1: 19.999999999999996, $2: 20}]  
```  
  
####  <span data-ttu-id="27ba2-720"><a name="bk_log"></a>PROTOKOLU</span><span class="sxs-lookup"><span data-stu-id="27ba2-720"><a name="bk_log"></a> LOG</span></span>  
 <span data-ttu-id="27ba2-721">Vrátí hello přirozený logaritmus hello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-721">Returns hello natural logarithm of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-722">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-722">**Syntax**</span></span>  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 <span data-ttu-id="27ba2-723">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-723">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-724">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-724">Is a numeric expression.</span></span>  
  
-   `base`  
  
     <span data-ttu-id="27ba2-725">Nepovinný argument číselné, který nastaví hello základ pro hello logaritmus</span><span class="sxs-lookup"><span data-stu-id="27ba2-725">Optional numeric argument that sets hello base for hello logarithm.</span></span>  
  
 <span data-ttu-id="27ba2-726">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-726">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-727">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-727">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-728">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="27ba2-728">**Remarks**</span></span>  
  
 <span data-ttu-id="27ba2-729">Ve výchozím nastavení LOG() vrátí přirozený logaritmus hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-729">By default, LOG() returns hello natural logarithm.</span></span> <span data-ttu-id="27ba2-730">Základní hello hello logaritmus tooanother hodnoty můžete změnit pomocí volitelný parametr základní hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-730">You can change hello base of hello logarithm tooanother value by using hello optional base parameter.</span></span>  
  
 <span data-ttu-id="27ba2-731">Hello přirozený logaritmus je toohello logaritmus hello základní **e**, kde **e** je přibližně stejné too2.718281828 nenormální konstantní.</span><span class="sxs-lookup"><span data-stu-id="27ba2-731">hello natural logarithm is hello logarithm toohello base **e**, where **e** is an irrational constant approximately equal too2.718281828.</span></span>  
  
 <span data-ttu-id="27ba2-732">Hello přirozený logaritmus čísla exponenciální hello je číslo hello, samotné: protokolu (EXP (ne)) = n.</span><span class="sxs-lookup"><span data-stu-id="27ba2-732">hello natural logarithm of hello exponential of a number is hello number itself: LOG( EXP( n ) ) = n.</span></span> <span data-ttu-id="27ba2-733">A hello exponent hello přirozený logaritmus čísla je číslo hello, samotné: EXP (protokol (ne)) = n.</span><span class="sxs-lookup"><span data-stu-id="27ba2-733">And hello exponential of hello natural logarithm of a number is hello number itself: EXP( LOG( n ) ) = n.</span></span>  
  
 <span data-ttu-id="27ba2-734">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-734">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-735">Hello následující příklad deklaruje proměnné a vrátí hodnotu logaritmus hello hello zadanou proměnnou (10).</span><span class="sxs-lookup"><span data-stu-id="27ba2-735">hello following example declares a variable and returns hello logarithm value of hello specified variable (10).</span></span>  
  
```  
SELECT LOG(10)  
```  
  
 <span data-ttu-id="27ba2-736">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-736">Here is hello result set.</span></span>  
  
```  
[{$1: 2.3025850929940459}]  
```  
  
 <span data-ttu-id="27ba2-737">Hello následující příklad vypočítá hello protokolu pro hello exponent čísla.</span><span class="sxs-lookup"><span data-stu-id="27ba2-737">hello following example calculates hello LOG for hello exponent of a number.</span></span>  
  
```  
SELECT EXP(LOG(10))  
```  
  
 <span data-ttu-id="27ba2-738">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-738">Here is hello result set.</span></span>  
  
```  
[{$1: 10.000000000000002}]  
```  
  
####  <span data-ttu-id="27ba2-739"><a name="bk_log10"></a>LOG10</span><span class="sxs-lookup"><span data-stu-id="27ba2-739"><a name="bk_log10"></a> LOG10</span></span>  
 <span data-ttu-id="27ba2-740">Vrátí logaritmus základu 10 hello hello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-740">Returns hello base-10 logarithm of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-741">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-741">**Syntax**</span></span>  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-742">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-742">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-743">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-743">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-744">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-744">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-745">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-745">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-746">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="27ba2-746">**Remarks**</span></span>  
  
 <span data-ttu-id="27ba2-747">Hello LOG10 a funkce POWER jsou nepřímo související tooone jiné.</span><span class="sxs-lookup"><span data-stu-id="27ba2-747">hello LOG10 and POWER functions are inversely related tooone another.</span></span> <span data-ttu-id="27ba2-748">Například 10 ^ LOG10(n) = n.</span><span class="sxs-lookup"><span data-stu-id="27ba2-748">For example, 10 ^ LOG10(n) = n.</span></span>  
  
 <span data-ttu-id="27ba2-749">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-749">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-750">Hello následující příklad deklaruje proměnné a vrátí hodnotu LOG10 hello hello zadanou proměnnou (100).</span><span class="sxs-lookup"><span data-stu-id="27ba2-750">hello following example declares a variable and returns hello LOG10 value of hello specified variable (100).</span></span>  
  
```  
SELECT LOG10(100)  
```  
  
 <span data-ttu-id="27ba2-751">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-751">Here is hello result set.</span></span>  
  
```  
[{$1: 2}]  
```  
  
####  <span data-ttu-id="27ba2-752"><a name="bk_pi"></a>PLATFORMY</span><span class="sxs-lookup"><span data-stu-id="27ba2-752"><a name="bk_pi"></a> PI</span></span>  
 <span data-ttu-id="27ba2-753">Vrátí hello konstantní hodnotu čísla PÍ.</span><span class="sxs-lookup"><span data-stu-id="27ba2-753">Returns hello constant value of PI.</span></span>  
  
 <span data-ttu-id="27ba2-754">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-754">**Syntax**</span></span>  
  
```  
PI ()  
```  
  
 <span data-ttu-id="27ba2-755">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-755">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-756">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-756">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-757">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-757">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-758">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-758">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-759">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-759">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-760">Hello následující příklad vrátí hello hodnotu čísla pí.</span><span class="sxs-lookup"><span data-stu-id="27ba2-760">hello following example returns hello value of PI.</span></span>  
  
```  
SELECT PI()  
```  
  
 <span data-ttu-id="27ba2-761">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-761">Here is hello result set.</span></span>  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <span data-ttu-id="27ba2-762"><a name="bk_power"></a>NAPÁJENÍ</span><span class="sxs-lookup"><span data-stu-id="27ba2-762"><a name="bk_power"></a> POWER</span></span>  
 <span data-ttu-id="27ba2-763">Vrátí hello hodnotu hello zadaný výraz toohello zadaný napájení.</span><span class="sxs-lookup"><span data-stu-id="27ba2-763">Returns hello value of hello specified expression toohello specified power.</span></span>  
  
 <span data-ttu-id="27ba2-764">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-764">**Syntax**</span></span>  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 <span data-ttu-id="27ba2-765">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-765">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-766">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-766">Is a numeric expression.</span></span>  
  
-   `y`  
  
     <span data-ttu-id="27ba2-767">Je hello power toowhich tooraise `numeric_expression`.</span><span class="sxs-lookup"><span data-stu-id="27ba2-767">Is hello power toowhich tooraise `numeric_expression`.</span></span>  
  
 <span data-ttu-id="27ba2-768">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-768">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-769">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-769">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-770">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-770">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-771">Hello následující příklad ukazuje, vyvolání číslo power toohello 3 (hello datové krychle hello čísla).</span><span class="sxs-lookup"><span data-stu-id="27ba2-771">hello following example demonstrates raising a number toohello power of 3 (hello cube of hello number).</span></span>  
  
```  
SELECT POWER(2, 3), POWER(2.5, 3)  
```  
  
 <span data-ttu-id="27ba2-772">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-772">Here is hello result set.</span></span>  
  
```  
[{$1: 8, $2: 15.625}]  
```  
  
####  <span data-ttu-id="27ba2-773"><a name="bk_radians"></a>RADIÁNECH</span><span class="sxs-lookup"><span data-stu-id="27ba2-773"><a name="bk_radians"></a> RADIANS</span></span>  
 <span data-ttu-id="27ba2-774">Vrátí radiánech při zadání číselného výrazu, ve stupních, se.</span><span class="sxs-lookup"><span data-stu-id="27ba2-774">Returns radians when a numeric expression, in degrees, is entered.</span></span>  
  
 <span data-ttu-id="27ba2-775">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-775">**Syntax**</span></span>  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-776">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-776">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-777">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-777">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-778">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-778">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-779">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-779">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-780">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-780">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-781">Hello následující příklad vezme jako vstupní údaje několik úhly a vrátí jejich příslušné hodnoty radián.</span><span class="sxs-lookup"><span data-stu-id="27ba2-781">hello following example takes a few angles as input and returns their corresponding radian values.</span></span>  
  
```  
SELECT RADIANS(-45.01), RADIANS(-181.01), RADIANS(0), RADIANS(0.1472738), RADIANS(197.1099392)  
```  
  
 <span data-ttu-id="27ba2-782">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-782">Here is hello result set.</span></span>  
  
```  
[{  
       "$1": -0.7855726963226477,  
       "$2": -3.1592204790349356,  
       "$3": 0,  
       "$4": 0.0025704127119236249,  
       "$5": 3.4402174274458375  
   }]  
```  
  
####  <span data-ttu-id="27ba2-783"><a name="bk_round"></a>ZAOKROUHLIT</span><span class="sxs-lookup"><span data-stu-id="27ba2-783"><a name="bk_round"></a> ROUND</span></span>  
 <span data-ttu-id="27ba2-784">Vrátí číselnou hodnotu, zaokrouhlené toohello nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="27ba2-784">Returns a numeric value, rounded toohello closest integer value.</span></span>  
  
 <span data-ttu-id="27ba2-785">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-785">**Syntax**</span></span>  
  
```  
ROUND(<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-786">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-786">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-787">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-787">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-788">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-788">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-789">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-789">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-790">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-790">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-791">Hello následující příklad zaokrouhlí hello následující kladné a záporné čísla toohello nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="27ba2-791">hello following example rounds hello following positive and negative numbers toohello nearest integer.</span></span>  
  
```  
SELECT ROUND(2.4), ROUND(2.6), ROUND(2.5), ROUND(-2.4), ROUND(-2.6)  
```  
  
 <span data-ttu-id="27ba2-792">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-792">Here is hello result set.</span></span>  
  
```  
[{$1: 2, $2: 3, $3: 3, $4: -2, $5: -3}]  
```  
  
####  <span data-ttu-id="27ba2-793"><a name="bk_sign"></a>PŘIHLÁŠENÍ</span><span class="sxs-lookup"><span data-stu-id="27ba2-793"><a name="bk_sign"></a> SIGN</span></span>  
 <span data-ttu-id="27ba2-794">Vrátí hello kladnou (+ 1), nula (0), nebo záporné znaménko (-1) hello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-794">Returns hello positive (+1), zero (0), or negative (-1) sign of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-795">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-795">**Syntax**</span></span>  
  
```  
SIGN(<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-796">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-796">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-797">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-797">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-798">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-798">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-799">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-799">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-800">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-800">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-801">Hello následující příklad vrátí hello PŘIHLAŠOVACÍ hodnot čísel z too2-2.</span><span class="sxs-lookup"><span data-stu-id="27ba2-801">hello following example returns hello SIGN values of numbers from -2 too2.</span></span>  
  
```  
SELECT SIGN(-2), SIGN(-1), SIGN(0), SIGN(1), SIGN(2)  
```  
  
 <span data-ttu-id="27ba2-802">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-802">Here is hello result set.</span></span>  
  
```  
[{$1: -1, $2: -1, $3: 0, $4: 1, $5: 1}]  
```  
  
####  <span data-ttu-id="27ba2-803"><a name="bk_sin"></a>SIN</span><span class="sxs-lookup"><span data-stu-id="27ba2-803"><a name="bk_sin"></a> SIN</span></span>  
 <span data-ttu-id="27ba2-804">Vrátí hello trigonometrické sinus hello úhlu, uvedeného v radiánech, v hello zadaný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-804">Returns hello trigonometric sine of hello specified angle, in radians, in hello specified expression.</span></span>  
  
 <span data-ttu-id="27ba2-805">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-805">**Syntax**</span></span>  
  
```  
SIN(<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-806">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-806">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-807">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-807">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-808">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-808">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-809">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-809">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-810">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-810">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-811">Následující ukázka Hello vypočítá hello SIN hello určeného úhlu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-811">hello following example calculates hello SIN of hello specified angle.</span></span>  
  
```  
SELECT SIN(45.175643)  
```  
  
 <span data-ttu-id="27ba2-812">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-812">Here is hello result set.</span></span>  
  
```  
[{"$1": 0.929607286611012}]  
```  
  
####  <span data-ttu-id="27ba2-813"><a name="bk_sqrt"></a>SQRT</span><span class="sxs-lookup"><span data-stu-id="27ba2-813"><a name="bk_sqrt"></a> SQRT</span></span>  
 <span data-ttu-id="27ba2-814">Vrátí hello druhou odmocninu čísla hello zadat číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-814">Returns hello square root of hello specified numeric value.</span></span>  
  
 <span data-ttu-id="27ba2-815">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-815">**Syntax**</span></span>  
  
```  
SQRT(<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-816">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-816">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-817">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-817">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-818">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-818">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-819">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-819">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-820">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-820">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-821">Hello následující příklad vrátí hello druhé odmocniny čísel 1 – 3.</span><span class="sxs-lookup"><span data-stu-id="27ba2-821">hello following example returns hello square roots of numbers 1-3.</span></span>  
  
```  
SELECT SQRT(1), SQRT(2.0), SQRT(3)  
```  
  
 <span data-ttu-id="27ba2-822">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-822">Here is hello result set.</span></span>  
  
```  
[{$1: 1, $2: 1.4142135623730952, $3: 1.7320508075688772}]  
```  
  
####  <span data-ttu-id="27ba2-823"><a name="bk_square"></a>HRANATÉ</span><span class="sxs-lookup"><span data-stu-id="27ba2-823"><a name="bk_square"></a> SQUARE</span></span>  
 <span data-ttu-id="27ba2-824">Vrátí hello odmocnina z hello zadat číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-824">Returns hello square of hello specified numeric value.</span></span>  
  
 <span data-ttu-id="27ba2-825">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-825">**Syntax**</span></span>  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-826">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-826">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-827">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-827">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-828">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-828">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-829">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-829">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-830">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-830">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-831">Hello následující příklad vrátí hello čtverce čísel 1 – 3.</span><span class="sxs-lookup"><span data-stu-id="27ba2-831">hello following example returns hello squares of numbers 1-3.</span></span>  
  
```  
SELECT SQUARE(1), SQUARE(2.0), SQUARE(3)  
```  
  
 <span data-ttu-id="27ba2-832">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-832">Here is hello result set.</span></span>  
  
```  
[{$1: 1, $2: 4, $3: 9}]  
```  
  
####  <span data-ttu-id="27ba2-833"><a name="bk_tan"></a>TAN</span><span class="sxs-lookup"><span data-stu-id="27ba2-833"><a name="bk_tan"></a> TAN</span></span>  
 <span data-ttu-id="27ba2-834">Vrátí tangens hello hello úhlu, uvedeného v radiánech, v hello zadaný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-834">Returns hello tangent of hello specified angle, in radians, in hello specified expression.</span></span>  
  
 <span data-ttu-id="27ba2-835">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-835">**Syntax**</span></span>  
  
```  
TAN (<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-836">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-836">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-837">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-837">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-838">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-838">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-839">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-839">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-840">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-840">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-841">Hello následující příklad vypočítá hello tangens () PÍ / 2.</span><span class="sxs-lookup"><span data-stu-id="27ba2-841">hello following example calculates hello tangent of PI()/2.</span></span>  
  
```  
SELECT TAN(PI()/2);  
```  
  
 <span data-ttu-id="27ba2-842">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-842">Here is hello result set.</span></span>  
  
```  
[{"$1": 16331239353195370 }]  
```  
  
####  <span data-ttu-id="27ba2-843"><a name="bk_trunc"></a>TRUNC</span><span class="sxs-lookup"><span data-stu-id="27ba2-843"><a name="bk_trunc"></a> TRUNC</span></span>  
 <span data-ttu-id="27ba2-844">Vrátí číselnou hodnotu, zkrácený toohello nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="27ba2-844">Returns a numeric value, truncated toohello closest integer value.</span></span>  
  
 <span data-ttu-id="27ba2-845">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-845">**Syntax**</span></span>  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 <span data-ttu-id="27ba2-846">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-846">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="27ba2-847">Je číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-847">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-848">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-848">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-849">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-849">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-850">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-850">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-851">Následující ukázka Hello zkrátí hello následující kladné a záporné čísla toohello nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="27ba2-851">hello following example truncates hello following positive and negative numbers toohello nearest integer value.</span></span>  
  
```  
SELECT TRUNC(2.4), TRUNC(2.6), TRUNC(2.5), TRUNC(-2.4), TRUNC(-2.6)  
```  
  
 <span data-ttu-id="27ba2-852">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-852">Here is hello result set.</span></span>  
  
```  
[{$1: 2, $2: 2, $3: 2, $4: -2, $5: -2}]  
```  
  
###  <span data-ttu-id="27ba2-853"><a name="bk_type_checking_functions"></a>Typ kontroly funkce</span><span class="sxs-lookup"><span data-stu-id="27ba2-853"><a name="bk_type_checking_functions"></a> Type checking functions</span></span>  
 <span data-ttu-id="27ba2-854">Hello následující funkce podporují typ kontroly proti vstupní hodnoty, a každou vrátit logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-854">hello following functions support type checking against input values, and each return a Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="27ba2-855">IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="27ba2-855">IS_ARRAY</span></span>](#bk_is_array)|[<span data-ttu-id="27ba2-856">IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="27ba2-856">IS_BOOL</span></span>](#bk_is_bool)|[<span data-ttu-id="27ba2-857">IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="27ba2-857">IS_DEFINED</span></span>](#bk_is_defined)|  
|[<span data-ttu-id="27ba2-858">IS_NULL</span><span class="sxs-lookup"><span data-stu-id="27ba2-858">IS_NULL</span></span>](#bk_is_null)|[<span data-ttu-id="27ba2-859">IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="27ba2-859">IS_NUMBER</span></span>](#bk_is_number)|[<span data-ttu-id="27ba2-860">IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="27ba2-860">IS_OBJECT</span></span>](#bk_is_object)|  
|[<span data-ttu-id="27ba2-861">IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="27ba2-861">IS_PRIMITIVE</span></span>](#bk_is_primitive)|[<span data-ttu-id="27ba2-862">IS_STRING</span><span class="sxs-lookup"><span data-stu-id="27ba2-862">IS_STRING</span></span>](#bk_is_string)||  
  
####  <span data-ttu-id="27ba2-863"><a name="bk_is_array"></a>IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="27ba2-863"><a name="bk_is_array"></a> IS_ARRAY</span></span>  
 <span data-ttu-id="27ba2-864">Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je pole.</span><span class="sxs-lookup"><span data-stu-id="27ba2-864">Returns a Boolean value indicating if hello type of hello specified expression is an array.</span></span>  
  
 <span data-ttu-id="27ba2-865">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-865">**Syntax**</span></span>  
  
```  
IS_ARRAY(<expression>)  
```  
  
 <span data-ttu-id="27ba2-866">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-866">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="27ba2-867">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-867">Is any valid expression.</span></span>  
  
 <span data-ttu-id="27ba2-868">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-868">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-869">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-869">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="27ba2-870">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-870">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-871">Hello následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí hello IS_ARRAY funkce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-871">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_ARRAY function.</span></span>  
  
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
  
 <span data-ttu-id="27ba2-872">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-872">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: false, $6: true}]  
```  
  
####  <span data-ttu-id="27ba2-873"><a name="bk_is_bool"></a>IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="27ba2-873"><a name="bk_is_bool"></a> IS_BOOL</span></span>  
 <span data-ttu-id="27ba2-874">Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="27ba2-874">Returns a Boolean value indicating if hello type of hello specified expression is a Boolean.</span></span>  
  
 <span data-ttu-id="27ba2-875">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-875">**Syntax**</span></span>  
  
```  
IS_BOOL(<expression>)  
```  
  
 <span data-ttu-id="27ba2-876">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-876">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="27ba2-877">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-877">Is any valid expression.</span></span>  
  
 <span data-ttu-id="27ba2-878">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-878">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-879">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-879">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="27ba2-880">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-880">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-881">Hello následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí hello IS_BOOL funkce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-881">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_BOOL function.</span></span>  
  
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
  
 <span data-ttu-id="27ba2-882">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-882">Here is hello result set.</span></span>  
  
```  
[{$1: true, $2: false, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="27ba2-883"><a name="bk_is_defined"></a>IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="27ba2-883"><a name="bk_is_defined"></a> IS_DEFINED</span></span>  
 <span data-ttu-id="27ba2-884">Vrátí logickou hodnotu udávající, pokud byla vlastnost hello přiřazena hodnota.</span><span class="sxs-lookup"><span data-stu-id="27ba2-884">Returns a Boolean indicating if hello property has been assigned a value.</span></span>  
  
 <span data-ttu-id="27ba2-885">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-885">**Syntax**</span></span>  
  
```  
IS_DEFINED(<expression>)  
```  
  
 <span data-ttu-id="27ba2-886">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-886">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="27ba2-887">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-887">Is any valid expression.</span></span>  
  
 <span data-ttu-id="27ba2-888">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-888">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-889">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-889">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="27ba2-890">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-890">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-891">Následující příklad zkontroluje přítomnost hello vlastnosti v rámci hello Hello zadaný dokument JSON.</span><span class="sxs-lookup"><span data-stu-id="27ba2-891">hello following example checks for hello presence of a property within hello specified JSON document.</span></span> <span data-ttu-id="27ba2-892">Hello nejprve vrací hodnotu true, protože "a" je k dispozici, ale hello druhý vrací hodnotu false vzhledem k tomu, že chybí "b".</span><span class="sxs-lookup"><span data-stu-id="27ba2-892">hello first returns true since "a" is present, but hello second returns false since "b" is absent.</span></span>  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a), IS_DEFINED({ "a" : 5 }.b)  
```  
  
 <span data-ttu-id="27ba2-893">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-893">Here is hello result set.</span></span>  
  
```  
[{  
       "$1": true,    
       "$2": false   
   }]  
```  
  
####  <span data-ttu-id="27ba2-894"><a name="bk_is_null"></a>IS_NULL</span><span class="sxs-lookup"><span data-stu-id="27ba2-894"><a name="bk_is_null"></a> IS_NULL</span></span>  
 <span data-ttu-id="27ba2-895">Vrátí logickou hodnotu udávající, pokud zadaný typ hello hello výraz má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="27ba2-895">Returns a Boolean value indicating if hello type of hello specified expression is null.</span></span>  
  
 <span data-ttu-id="27ba2-896">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-896">**Syntax**</span></span>  
  
```  
IS_NULL(<expression>)  
```  
  
 <span data-ttu-id="27ba2-897">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-897">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="27ba2-898">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-898">Is any valid expression.</span></span>  
  
 <span data-ttu-id="27ba2-899">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-899">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-900">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-900">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="27ba2-901">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-901">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-902">Hello následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí hello IS_NULL funkce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-902">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_NULL function.</span></span>  
  
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
  
 <span data-ttu-id="27ba2-903">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-903">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: true, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="27ba2-904"><a name="bk_is_number"></a>IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="27ba2-904"><a name="bk_is_number"></a> IS_NUMBER</span></span>  
 <span data-ttu-id="27ba2-905">Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je číslo.</span><span class="sxs-lookup"><span data-stu-id="27ba2-905">Returns a Boolean value indicating if hello type of hello specified expression is a number.</span></span>  
  
 <span data-ttu-id="27ba2-906">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-906">**Syntax**</span></span>  
  
```  
IS_NUMBER(<expression>)  
```  
  
 <span data-ttu-id="27ba2-907">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-907">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="27ba2-908">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-908">Is any valid expression.</span></span>  
  
 <span data-ttu-id="27ba2-909">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-909">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-910">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-910">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="27ba2-911">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-911">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-912">Hello následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí hello IS_NULL funkce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-912">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_NULL function.</span></span>  
  
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
  
 <span data-ttu-id="27ba2-913">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-913">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: true, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="27ba2-914"><a name="bk_is_object"></a>IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="27ba2-914"><a name="bk_is_object"></a> IS_OBJECT</span></span>  
 <span data-ttu-id="27ba2-915">Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="27ba2-915">Returns a Boolean value indicating if hello type of hello specified expression is a JSON object.</span></span>  
  
 <span data-ttu-id="27ba2-916">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-916">**Syntax**</span></span>  
  
```  
IS_OBJECT(<expression>)  
```  
  
 <span data-ttu-id="27ba2-917">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-917">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="27ba2-918">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-918">Is any valid expression.</span></span>  
  
 <span data-ttu-id="27ba2-919">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-919">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-920">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-920">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="27ba2-921">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-921">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-922">Hello následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí hello IS_OBJECT funkce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-922">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_OBJECT function.</span></span>  
  
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
  
 <span data-ttu-id="27ba2-923">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-923">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: true, $6: false}]  
```  
  
####  <span data-ttu-id="27ba2-924"><a name="bk_is_primitive"></a>IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="27ba2-924"><a name="bk_is_primitive"></a> IS_PRIMITIVE</span></span>  
 <span data-ttu-id="27ba2-925">Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je primitivní (řetězec, logická hodnota, číselná nebo má hodnotu null).</span><span class="sxs-lookup"><span data-stu-id="27ba2-925">Returns a Boolean value indicating if hello type of hello specified expression is a primitive (string, Boolean, numeric or null).</span></span>  
  
 <span data-ttu-id="27ba2-926">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-926">**Syntax**</span></span>  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 <span data-ttu-id="27ba2-927">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-927">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="27ba2-928">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-928">Is any valid expression.</span></span>  
  
 <span data-ttu-id="27ba2-929">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-929">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-930">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-930">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="27ba2-931">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-931">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-932">Hello následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí hello IS_PRIMITIVE funkce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-932">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_PRIMITIVE function.</span></span>  
  
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
  
 <span data-ttu-id="27ba2-933">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-933">Here is hello result set.</span></span>  
  
```  
[{"$1": true, "$2": true, "$3": true, "$4": true, "$5": false, "$6": false, "$7": false}]  
```  
  
####  <span data-ttu-id="27ba2-934"><a name="bk_is_string"></a>IS_STRING</span><span class="sxs-lookup"><span data-stu-id="27ba2-934"><a name="bk_is_string"></a> IS_STRING</span></span>  
 <span data-ttu-id="27ba2-935">Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz obsahuje řetězec.</span><span class="sxs-lookup"><span data-stu-id="27ba2-935">Returns a Boolean value indicating if hello type of hello specified expression is a string.</span></span>  
  
 <span data-ttu-id="27ba2-936">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-936">**Syntax**</span></span>  
  
```  
IS_STRING(<expression>)  
```  
  
 <span data-ttu-id="27ba2-937">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-937">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="27ba2-938">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-938">Is any valid expression.</span></span>  
  
 <span data-ttu-id="27ba2-939">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-939">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-940">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-940">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="27ba2-941">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-941">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-942">Hello následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí hello IS_STRING funkce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-942">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_STRING function.</span></span>  
  
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
  
 <span data-ttu-id="27ba2-943">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-943">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: true, $4: false, $5: false, $6: false}]  
```  
  
###  <span data-ttu-id="27ba2-944"><a name="bk_string_functions"></a>Řetězcové funkce</span><span class="sxs-lookup"><span data-stu-id="27ba2-944"><a name="bk_string_functions"></a> String functions</span></span>  
 <span data-ttu-id="27ba2-945">Hello následující skalární funkce provést operaci s vstupní hodnotu řetězce a vrátí řetězec, číselnou nebo logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="27ba2-945">hello following scalar functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="27ba2-946">CONCAT</span><span class="sxs-lookup"><span data-stu-id="27ba2-946">CONCAT</span></span>](#bk_concat)|[<span data-ttu-id="27ba2-947">OBSAHUJE</span><span class="sxs-lookup"><span data-stu-id="27ba2-947">CONTAINS</span></span>](#bk_contains)|[<span data-ttu-id="27ba2-948">ENDSWITH</span><span class="sxs-lookup"><span data-stu-id="27ba2-948">ENDSWITH</span></span>](#bk_endswith)|  
|[<span data-ttu-id="27ba2-949">INDEX_OF</span><span class="sxs-lookup"><span data-stu-id="27ba2-949">INDEX_OF</span></span>](#bk_index_of)|[<span data-ttu-id="27ba2-950">VLEVO</span><span class="sxs-lookup"><span data-stu-id="27ba2-950">LEFT</span></span>](#bk_left)|[<span data-ttu-id="27ba2-951">DÉLKA</span><span class="sxs-lookup"><span data-stu-id="27ba2-951">LENGTH</span></span>](#bk_length)|  
|[<span data-ttu-id="27ba2-952">NIŽŠÍ</span><span class="sxs-lookup"><span data-stu-id="27ba2-952">LOWER</span></span>](#bk_lower)|[<span data-ttu-id="27ba2-953">LTRIM</span><span class="sxs-lookup"><span data-stu-id="27ba2-953">LTRIM</span></span>](#bk_ltrim)|[<span data-ttu-id="27ba2-954">NAHRADIT</span><span class="sxs-lookup"><span data-stu-id="27ba2-954">REPLACE</span></span>](#bk_replace)|  
|[<span data-ttu-id="27ba2-955">REPLIKACE</span><span class="sxs-lookup"><span data-stu-id="27ba2-955">REPLICATE</span></span>](#bk_replicate)|[<span data-ttu-id="27ba2-956">REVERSE</span><span class="sxs-lookup"><span data-stu-id="27ba2-956">REVERSE</span></span>](#bk_reverse)|[<span data-ttu-id="27ba2-957">VPRAVO</span><span class="sxs-lookup"><span data-stu-id="27ba2-957">RIGHT</span></span>](#bk_right)|  
|[<span data-ttu-id="27ba2-958">RTRIM</span><span class="sxs-lookup"><span data-stu-id="27ba2-958">RTRIM</span></span>](#bk_rtrim)|[<span data-ttu-id="27ba2-959">STARTSWITH</span><span class="sxs-lookup"><span data-stu-id="27ba2-959">STARTSWITH</span></span>](#bk_startswith)|[<span data-ttu-id="27ba2-960">DÍLČÍ ŘETĚZEC</span><span class="sxs-lookup"><span data-stu-id="27ba2-960">SUBSTRING</span></span>](#bk_substring)|  
|[<span data-ttu-id="27ba2-961">HORNÍ</span><span class="sxs-lookup"><span data-stu-id="27ba2-961">UPPER</span></span>](#bk_upper)|||  
  
####  <span data-ttu-id="27ba2-962"><a name="bk_concat"></a>CONCAT</span><span class="sxs-lookup"><span data-stu-id="27ba2-962"><a name="bk_concat"></a> CONCAT</span></span>  
 <span data-ttu-id="27ba2-963">Vrátí řetězec, který je výsledkem hello zřetězení dvou nebo více řetězcové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="27ba2-963">Returns a string that is hello result of concatenating two or more string values.</span></span>  
  
 <span data-ttu-id="27ba2-964">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-964">**Syntax**</span></span>  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 <span data-ttu-id="27ba2-965">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-965">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-966">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-966">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="27ba2-967">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-967">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-968">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-968">Returns a string expression.</span></span>  
  
 <span data-ttu-id="27ba2-969">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-969">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-970">Následující příklad vrátí řetězec hello zřetězených hello Hello zadané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="27ba2-970">hello following example returns hello concatenated string of hello specified values.</span></span>  
  
```  
SELECT CONCAT("abc", "def")  
```  
  
 <span data-ttu-id="27ba2-971">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-971">Here is hello result set.</span></span>  
  
```  
[{"$1": "abcdef"}  
```  
  
####  <span data-ttu-id="27ba2-972"><a name="bk_contains"></a>OBSAHUJE</span><span class="sxs-lookup"><span data-stu-id="27ba2-972"><a name="bk_contains"></a> CONTAINS</span></span>  
 <span data-ttu-id="27ba2-973">Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu obsahuje hello druhý.</span><span class="sxs-lookup"><span data-stu-id="27ba2-973">Returns a Boolean indicating whether hello first string expression contains hello second.</span></span>  
  
 <span data-ttu-id="27ba2-974">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-974">**Syntax**</span></span>  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="27ba2-975">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-975">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-976">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-976">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="27ba2-977">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-977">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-978">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-978">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="27ba2-979">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-979">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-980">Hello následující příklad zkontroluje Pokud "abc" "ab" a "d" obsahuje.</span><span class="sxs-lookup"><span data-stu-id="27ba2-980">hello following example checks if "abc" contains "ab" and contains "d".</span></span>  
  
```  
SELECT CONTAINS("abc", "ab"), CONTAINS("abc", "d")  
```  
  
 <span data-ttu-id="27ba2-981">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-981">Here is hello result set.</span></span>  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <span data-ttu-id="27ba2-982"><a name="bk_endswith"></a>ENDSWITH</span><span class="sxs-lookup"><span data-stu-id="27ba2-982"><a name="bk_endswith"></a> ENDSWITH</span></span>  
 <span data-ttu-id="27ba2-983">Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu končí hello druhý.</span><span class="sxs-lookup"><span data-stu-id="27ba2-983">Returns a Boolean indicating whether hello first string expression ends with hello second.</span></span>  
  
 <span data-ttu-id="27ba2-984">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-984">**Syntax**</span></span>  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="27ba2-985">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-985">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-986">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-986">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="27ba2-987">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-987">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-988">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-988">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="27ba2-989">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-989">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-990">Hello následující příklad vrátí hello "abc" končí textem "b" a "bc".</span><span class="sxs-lookup"><span data-stu-id="27ba2-990">hello following example returns hello "abc" ends with "b" and "bc".</span></span>  
  
```  
SELECT ENDSWITH("abc", "b"), ENDSWITH("abc", "bc")  
```  
  
 <span data-ttu-id="27ba2-991">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-991">Here is hello result set.</span></span>  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <span data-ttu-id="27ba2-992"><a name="bk_index_of"></a>INDEX_OF</span><span class="sxs-lookup"><span data-stu-id="27ba2-992"><a name="bk_index_of"></a> INDEX_OF</span></span>  
 <span data-ttu-id="27ba2-993">Vrátí počáteční pozici prvního výskytu hello hello druhý řetězcového výrazu v rámci zadaného řetězcového výrazu první hello nebo -1, pokud není nalezen řetězec hello hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-993">Returns hello starting position of hello first occurrence of hello second string expression within hello first specified string expression, or -1 if hello string is not found.</span></span>  
  
 <span data-ttu-id="27ba2-994">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-994">**Syntax**</span></span>  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="27ba2-995">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-995">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-996">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-996">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="27ba2-997">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-997">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-998">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-998">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-999">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-999">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1000">Hello následující příklad vrátí index hello různé podřetězců uvnitř "abc".</span><span class="sxs-lookup"><span data-stu-id="27ba2-1000">hello following example returns hello index of various substrings inside "abc".</span></span>  
  
```  
SELECT INDEX_OF("abc", "ab"), INDEX_OF("abc", "b"), INDEX_OF("abc", "c")  
```  
  
 <span data-ttu-id="27ba2-1001">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1001">Here is hello result set.</span></span>  
  
```  
[{"$1": 0, "$2": 1, "$3": -1}]  
```  
  
####  <span data-ttu-id="27ba2-1002"><a name="bk_left"></a>VLEVO</span><span class="sxs-lookup"><span data-stu-id="27ba2-1002"><a name="bk_left"></a> LEFT</span></span>  
 <span data-ttu-id="27ba2-1003">Vrátí hello levé části řetězce s hello zadaný počet znaků.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1003">Returns hello left part of a string with hello specified number of characters.</span></span>  
  
 <span data-ttu-id="27ba2-1004">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1004">**Syntax**</span></span>  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="27ba2-1005">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1005">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-1006">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1006">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="27ba2-1007">Je libovolný platný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1007">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-1008">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1008">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1009">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1009">Returns a string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1010">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1010">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1011">Hello následující příklad vrátí hello zbývající část "abc" pro různé hodnoty pro délku.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1011">hello following example returns hello left part of "abc" for various length values.</span></span>  
  
```  
SELECT LEFT("abc", 1), LEFT("abc", 2)  
```  
  
 <span data-ttu-id="27ba2-1012">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1012">Here is hello result set.</span></span>  
  
```  
[{"$1": "ab", "$2": "ab"}]  
```  
  
####  <span data-ttu-id="27ba2-1013"><a name="bk_length"></a>DÉLKA</span><span class="sxs-lookup"><span data-stu-id="27ba2-1013"><a name="bk_length"></a> LENGTH</span></span>  
 <span data-ttu-id="27ba2-1014">Vrátí hello počet znaků, který hello zadán řetězcovým výrazem.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1014">Returns hello number of characters of hello specified string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1015">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1015">**Syntax**</span></span>  
  
```  
LENGTH(<str_expr>)  
```  
  
 <span data-ttu-id="27ba2-1016">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1016">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-1017">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1017">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1018">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1018">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1019">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1019">Returns a string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1020">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1020">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1021">Hello následující příklad vrátí hello délku řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1021">hello following example returns hello length of a string.</span></span>  
  
```  
SELECT LENGTH("abc")  
```  
  
 <span data-ttu-id="27ba2-1022">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1022">Here is hello result set.</span></span>  
  
```  
[{"$1": 3}]  
```  
  
####  <span data-ttu-id="27ba2-1023"><a name="bk_lower"></a>NIŽŠÍ</span><span class="sxs-lookup"><span data-stu-id="27ba2-1023"><a name="bk_lower"></a> LOWER</span></span>  
 <span data-ttu-id="27ba2-1024">Vrací výraz řetězce po převodu dat toolowercase velké písmeno.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1024">Returns a string expression after converting uppercase character data toolowercase.</span></span>  
  
 <span data-ttu-id="27ba2-1025">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1025">**Syntax**</span></span>  
  
```  
LOWER(<str_expr>)  
```  
  
 <span data-ttu-id="27ba2-1026">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1026">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-1027">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1027">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1028">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1028">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1029">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1029">Returns a string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1030">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1030">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1031">Následující příklad ukazuje, jak Hello toouse nižší v dotazu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1031">hello following example shows how toouse LOWER in a query.</span></span>  
  
```  
SELECT LOWER("Abc")  
```  
  
 <span data-ttu-id="27ba2-1032">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1032">Here is hello result set.</span></span>  
  
```  
[{"$1": "abc"}]  
  
```  
  
####  <span data-ttu-id="27ba2-1033"><a name="bk_ltrim"></a>LTRIM</span><span class="sxs-lookup"><span data-stu-id="27ba2-1033"><a name="bk_ltrim"></a> LTRIM</span></span>  
 <span data-ttu-id="27ba2-1034">Vrací výraz řetězce po ho odebere úvodní mezery.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1034">Returns a string expression after it removes leading blanks.</span></span>  
  
 <span data-ttu-id="27ba2-1035">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1035">**Syntax**</span></span>  
  
```  
LTRIM(<str_expr>)  
```  
  
 <span data-ttu-id="27ba2-1036">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1036">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-1037">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1037">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1038">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1038">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1039">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1039">Returns a string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1040">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1040">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1041">Následující příklad ukazuje, jak Hello toouse LTRIM uvnitř dotazu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1041">hello following example shows how toouse LTRIM inside a query.</span></span>  
  
```  
SELECT LTRIM("  abc"), LTRIM("abc"), LTRIM("abc   ")  
```  
  
 <span data-ttu-id="27ba2-1042">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1042">Here is hello result set.</span></span>  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc   "}]  
```  
  
####  <span data-ttu-id="27ba2-1043"><a name="bk_replace"></a>NAHRADIT</span><span class="sxs-lookup"><span data-stu-id="27ba2-1043"><a name="bk_replace"></a> REPLACE</span></span>  
 <span data-ttu-id="27ba2-1044">Nahradí všechny výskyty zadaná řetězcová hodnota s jinou hodnotou řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1044">Replaces all occurrences of a specified string value with another string value.</span></span>  
  
 <span data-ttu-id="27ba2-1045">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1045">**Syntax**</span></span>  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="27ba2-1046">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1046">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-1047">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1047">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1048">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1048">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1049">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1049">Returns a string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1050">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1050">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1051">Hello následující příklad ukazuje, jak nahradit toouse v dotazu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1051">hello following example shows how toouse REPLACE in a query.</span></span>  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk")  
```  
  
 <span data-ttu-id="27ba2-1052">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1052">Here is hello result set.</span></span>  
  
```  
[{"$1": "This is a desk"}]  
```  
  
####  <span data-ttu-id="27ba2-1053"><a name="bk_replicate"></a>REPLIKACE</span><span class="sxs-lookup"><span data-stu-id="27ba2-1053"><a name="bk_replicate"></a> REPLICATE</span></span>  
 <span data-ttu-id="27ba2-1054">Opakuje hodnotu řetězce zadaného počtu opakování.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1054">Repeats a string value a specified number of times.</span></span>  
  
 <span data-ttu-id="27ba2-1055">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1055">**Syntax**</span></span>  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="27ba2-1056">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1056">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-1057">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1057">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="27ba2-1058">Je libovolný platný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1058">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-1059">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1059">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1060">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1060">Returns a string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1061">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1061">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1062">Hello následující příklad ukazuje, jak REPLIKOVAT toouse v dotazu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1062">hello following example shows how toouse REPLICATE in a query.</span></span>  
  
```  
SELECT REPLICATE("a", 3)  
```  
  
 <span data-ttu-id="27ba2-1063">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1063">Here is hello result set.</span></span>  
  
```  
[{"$1": "aaa"}]  
```  
  
####  <span data-ttu-id="27ba2-1064"><a name="bk_reverse"></a>REVERSE</span><span class="sxs-lookup"><span data-stu-id="27ba2-1064"><a name="bk_reverse"></a> REVERSE</span></span>  
 <span data-ttu-id="27ba2-1065">Vrátí hello zpětné pořadí řetězcovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1065">Returns hello reverse order of a string value.</span></span>  
  
 <span data-ttu-id="27ba2-1066">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1066">**Syntax**</span></span>  
  
```  
REVERSE(<str_expr>)  
```  
  
 <span data-ttu-id="27ba2-1067">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1067">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-1068">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1068">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1069">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1069">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1070">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1070">Returns a string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1071">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1071">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1072">Hello následující příklad ukazuje, jak toouse REVERSE v dotazu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1072">hello following example shows how toouse REVERSE in a query.</span></span>  
  
```  
SELECT REVERSE("Abc")  
```  
  
 <span data-ttu-id="27ba2-1073">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1073">Here is hello result set.</span></span>  
  
```  
[{"$1": "cbA"}]  
```  
  
####  <span data-ttu-id="27ba2-1074"><a name="bk_right"></a>VPRAVO</span><span class="sxs-lookup"><span data-stu-id="27ba2-1074"><a name="bk_right"></a> RIGHT</span></span>  
 <span data-ttu-id="27ba2-1075">Vrátí hello právo součástí řetězec s hello zadaný počet znaků.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1075">Returns hello right part of a string with hello specified number of characters.</span></span>  
  
 <span data-ttu-id="27ba2-1076">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1076">**Syntax**</span></span>  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="27ba2-1077">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1077">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-1078">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1078">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="27ba2-1079">Je libovolný platný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1079">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-1080">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1080">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1081">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1081">Returns a string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1082">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1082">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1083">Hello následující příklad vrátí hello pravou část "abc" pro různé hodnoty pro délku.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1083">hello following example returns hello right part of "abc" for various length values.</span></span>  
  
```  
SELECT RIGHT("abc", 1), RIGHT("abc", 2)  
```  
  
 <span data-ttu-id="27ba2-1084">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1084">Here is hello result set.</span></span>  
  
```  
[{"$1": "c", "$2": "bc"}]  
```  
  
####  <span data-ttu-id="27ba2-1085"><a name="bk_rtrim"></a>RTRIM</span><span class="sxs-lookup"><span data-stu-id="27ba2-1085"><a name="bk_rtrim"></a> RTRIM</span></span>  
 <span data-ttu-id="27ba2-1086">Vrací výraz řetězce po odebere koncové mezery.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1086">Returns a string expression after it removes trailing blanks.</span></span>  
  
 <span data-ttu-id="27ba2-1087">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1087">**Syntax**</span></span>  
  
```  
RTRIM(<str_expr>)  
```  
  
 <span data-ttu-id="27ba2-1088">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1088">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-1089">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1089">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1090">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1090">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1091">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1091">Returns a string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1092">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1092">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1093">Následující příklad ukazuje, jak Hello toouse RTRIM uvnitř dotazu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1093">hello following example shows how toouse RTRIM inside a query.</span></span>  
  
```  
SELECT RTRIM("  abc"), RTRIM("abc"), RTRIM("abc   ")  
```  
  
 <span data-ttu-id="27ba2-1094">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1094">Here is hello result set.</span></span>  
  
```  
[{"$1": "   abc", "$2": "abc", "$3": "abc"}]  
```  
  
####  <span data-ttu-id="27ba2-1095"><a name="bk_startswith"></a>STARTSWITH</span><span class="sxs-lookup"><span data-stu-id="27ba2-1095"><a name="bk_startswith"></a> STARTSWITH</span></span>  
 <span data-ttu-id="27ba2-1096">Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu začíná hello druhý.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1096">Returns a Boolean indicating whether hello first string expression starts with hello second.</span></span>  
  
 <span data-ttu-id="27ba2-1097">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1097">**Syntax**</span></span>  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="27ba2-1098">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1098">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-1099">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1099">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1100">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1100">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1101">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1101">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="27ba2-1102">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1102">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1103">Hello následující příklad ověří, zda je hello řetězec "abc" začíná "b" a "a".</span><span class="sxs-lookup"><span data-stu-id="27ba2-1103">hello following example checks if hello string "abc" begins with "b" and "a".</span></span>  
  
```  
SELECT STARTSWITH("abc", "b"), STARTSWITH("abc", "a")  
```  
  
 <span data-ttu-id="27ba2-1104">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1104">Here is hello result set.</span></span>  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <span data-ttu-id="27ba2-1105"><a name="bk_substring"></a>DÍLČÍ ŘETĚZEC</span><span class="sxs-lookup"><span data-stu-id="27ba2-1105"><a name="bk_substring"></a> SUBSTRING</span></span>  
 <span data-ttu-id="27ba2-1106">Vrátí část výrazu řetězce začínající hello zadaný znak pozice s nulovým základem a pokračuje v toohello Zadaná délka nebo toohello konce řetězce hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1106">Returns part of a string expression starting at hello specified character zero-based position and continues toohello specified length, or toohello end of hello string.</span></span>  
  
 <span data-ttu-id="27ba2-1107">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1107">**Syntax**</span></span>  
  
```  
SUBSTRING(<str_expr>, <num_expr> [, <num_expr>])  
```  
  
 <span data-ttu-id="27ba2-1108">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1108">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-1109">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1109">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="27ba2-1110">Je libovolný platný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1110">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-1111">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1111">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1112">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1112">Returns a string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1113">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1113">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1114">Hello následující příklad vrátí hello podřetězcem "abc" spouštění v 1 a o délce 1 znak.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1114">hello following example returns hello substring of "abc" starting at 1 and for a length of 1 character.</span></span>  
  
```  
SELECT SUBSTRING("abc", 1, 1)  
```  
  
 <span data-ttu-id="27ba2-1115">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1115">Here is hello result set.</span></span>  
  
```  
[{"$1": "b"}]  
```  
  
####  <span data-ttu-id="27ba2-1116"><a name="bk_upper"></a>HORNÍ</span><span class="sxs-lookup"><span data-stu-id="27ba2-1116"><a name="bk_upper"></a> UPPER</span></span>  
 <span data-ttu-id="27ba2-1117">Vrací výraz řetězce po převodu dat toouppercase malé písmeno.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1117">Returns a string expression after converting lowercase character data toouppercase.</span></span>  
  
 <span data-ttu-id="27ba2-1118">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1118">**Syntax**</span></span>  
  
```  
UPPER(<str_expr>)  
```  
  
 <span data-ttu-id="27ba2-1119">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1119">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="27ba2-1120">Je jakýkoli platný řetězec výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1120">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1121">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1121">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1122">Vrací výraz řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1122">Returns a string expression.</span></span>  
  
 <span data-ttu-id="27ba2-1123">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1123">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1124">Následující příklad ukazuje, jak Hello toouse horní v dotazu</span><span class="sxs-lookup"><span data-stu-id="27ba2-1124">hello following example shows how toouse UPPER in a query</span></span>  
  
```  
SELECT UPPER("Abc")  
```  
  
 <span data-ttu-id="27ba2-1125">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1125">Here is hello result set.</span></span>  
  
```  
[{"$1": "ABC"}]  
```  
  
###  <span data-ttu-id="27ba2-1126"><a name="bk_array_functions"></a>Funkce pole</span><span class="sxs-lookup"><span data-stu-id="27ba2-1126"><a name="bk_array_functions"></a> Array functions</span></span>  
 <span data-ttu-id="27ba2-1127">provedení operace hodnota vstupní pole a vrátí číselnou, logická hodnota nebo pole hodnota Hello následující skalární funkce</span><span class="sxs-lookup"><span data-stu-id="27ba2-1127">hello following scalar functions perform an operation on an array input value and return numeric, Boolean or array value</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="27ba2-1128">ARRAY_CONCAT</span><span class="sxs-lookup"><span data-stu-id="27ba2-1128">ARRAY_CONCAT</span></span>](#bk_array_concat)|[<span data-ttu-id="27ba2-1129">ARRAY_CONTAINS</span><span class="sxs-lookup"><span data-stu-id="27ba2-1129">ARRAY_CONTAINS</span></span>](#bk_array_contains)|[<span data-ttu-id="27ba2-1130">ARRAY_LENGTH</span><span class="sxs-lookup"><span data-stu-id="27ba2-1130">ARRAY_LENGTH</span></span>](#bk_array_length)|  
|[<span data-ttu-id="27ba2-1131">ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="27ba2-1131">ARRAY_SLICE</span></span>](#bk_array_slice)|||  
  
####  <span data-ttu-id="27ba2-1132"><a name="bk_array_concat"></a>ARRAY_CONCAT</span><span class="sxs-lookup"><span data-stu-id="27ba2-1132"><a name="bk_array_concat"></a> ARRAY_CONCAT</span></span>  
 <span data-ttu-id="27ba2-1133">Vrátí pole, které je výsledkem hello zřetězení dvě nebo více hodnot pole.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1133">Returns an array that is hello result of concatenating two or more array values.</span></span>  
  
 <span data-ttu-id="27ba2-1134">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1134">**Syntax**</span></span>  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 <span data-ttu-id="27ba2-1135">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1135">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="27ba2-1136">Je jakýkoli platný pole výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1136">Is any valid array expression.</span></span>  
  
 <span data-ttu-id="27ba2-1137">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1137">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1138">Vrací výraz pole.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1138">Returns an array expression.</span></span>  
  
 <span data-ttu-id="27ba2-1139">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1139">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1140">Hello následující příklad, jak maticových tooconcatenate dva.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1140">hello following example how tooconcatenate two arrays.</span></span>  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"])  
```  
  
 <span data-ttu-id="27ba2-1141">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1141">Here is hello result set.</span></span>  
  
```  
[{"$1": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <span data-ttu-id="27ba2-1142"><a name="bk_array_contains"></a>ARRAY_CONTAINS</span><span class="sxs-lookup"><span data-stu-id="27ba2-1142"><a name="bk_array_contains"></a> ARRAY_CONTAINS</span></span>  
 <span data-ttu-id="27ba2-1143">Vrátí logickou hodnotu udávající, zda text hello pole obsahuje hello zadané hodnotě.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1143">Returns a Boolean indicating whether hello array contains hello specified value.</span></span>  
  
 <span data-ttu-id="27ba2-1144">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1144">**Syntax**</span></span>  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr>)  
```  
  
 <span data-ttu-id="27ba2-1145">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1145">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="27ba2-1146">Je jakýkoli platný pole výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1146">Is any valid array expression.</span></span>  
  
-   `expr`  
  
     <span data-ttu-id="27ba2-1147">Je jakýkoli platný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1147">Is any valid expression.</span></span>  
  
 <span data-ttu-id="27ba2-1148">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1148">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1149">Vrátí logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1149">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="27ba2-1150">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1150">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1151">Následující příklad, jak Hello toocheck pro členství v poli pomocí ARRAY_CONTAINS.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1151">hello following example how toocheck for membership in an array using ARRAY_CONTAINS.</span></span>  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples"),  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes")  
```  
  
 <span data-ttu-id="27ba2-1152">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1152">Here is hello result set.</span></span>  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <span data-ttu-id="27ba2-1153"><a name="bk_array_length"></a>ARRAY_LENGTH</span><span class="sxs-lookup"><span data-stu-id="27ba2-1153"><a name="bk_array_length"></a> ARRAY_LENGTH</span></span>  
 <span data-ttu-id="27ba2-1154">Vrátí číslo hello elementů hello zadaný výraz pole.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1154">Returns hello number of elements of hello specified array expression.</span></span>  
  
 <span data-ttu-id="27ba2-1155">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1155">**Syntax**</span></span>  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 <span data-ttu-id="27ba2-1156">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1156">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="27ba2-1157">Je jakýkoli platný pole výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1157">Is any valid array expression.</span></span>  
  
 <span data-ttu-id="27ba2-1158">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1158">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1159">Vrátí číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1159">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-1160">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1160">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1161">Hello následující příklad, jak tooget hello délka pole pomocí ARRAY_LENGTH.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1161">hello following example how tooget hello length of an array using ARRAY_LENGTH.</span></span>  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"])  
```  
  
 <span data-ttu-id="27ba2-1162">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1162">Here is hello result set.</span></span>  
  
```  
[{"$1": 3}]  
```  
  
####  <span data-ttu-id="27ba2-1163"><a name="bk_array_slice"></a>ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="27ba2-1163"><a name="bk_array_slice"></a> ARRAY_SLICE</span></span>  
 <span data-ttu-id="27ba2-1164">Vrátí část výraz pole.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1164">Returns part of an array expression.</span></span>
  
 <span data-ttu-id="27ba2-1165">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1165">**Syntax**</span></span>  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 <span data-ttu-id="27ba2-1166">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1166">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="27ba2-1167">Je jakýkoli platný pole výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1167">Is any valid array expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="27ba2-1168">Je libovolný platný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1168">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="27ba2-1169">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1169">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1170">Vrátí logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1170">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="27ba2-1171">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1171">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1172">Následující příklad, jak Hello tooget součástí pomocí ARRAY_SLICE pole.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1172">hello following example how tooget a part of an array using ARRAY_SLICE.</span></span>  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1),  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1)  
```  
  
 <span data-ttu-id="27ba2-1173">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1173">Here is hello result set.</span></span>  
  
```  
[{  
           "$1": ["strawberries", "bananas"],   
           "$2": ["strawberries"]  
       }]  
```  
  
###  <span data-ttu-id="27ba2-1174"><a name="bk_spatial_functions"></a>Prostorové funkce</span><span class="sxs-lookup"><span data-stu-id="27ba2-1174"><a name="bk_spatial_functions"></a> Spatial functions</span></span>  
 <span data-ttu-id="27ba2-1175">Hello následující skalární funkce provádění operací na prostorový objekt vstupní hodnotu a vrátí číslo nebo logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1175">hello following scalar functions perform an operation on an spatial object input value and return a numeric or Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="27ba2-1176">ST_DISTANCE</span><span class="sxs-lookup"><span data-stu-id="27ba2-1176">ST_DISTANCE</span></span>](#bk_st_distance)|[<span data-ttu-id="27ba2-1177">ST_WITHIN</span><span class="sxs-lookup"><span data-stu-id="27ba2-1177">ST_WITHIN</span></span>](#bk_st_within)|[<span data-ttu-id="27ba2-1178">ST_INTERSECTS</span><span class="sxs-lookup"><span data-stu-id="27ba2-1178">ST_INTERSECTS</span></span>](#bk_st_intersects)|[<span data-ttu-id="27ba2-1179">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="27ba2-1179">ST_ISVALID</span></span>](#bk_st_isvalid)|  
|[<span data-ttu-id="27ba2-1180">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="27ba2-1180">ST_ISVALIDDETAILED</span></span>](#bk_st_isvaliddetailed)|||  
  
####  <span data-ttu-id="27ba2-1181"><a name="bk_st_distance"></a>ST_DISTANCE</span><span class="sxs-lookup"><span data-stu-id="27ba2-1181"><a name="bk_st_distance"></a> ST_DISTANCE</span></span>  
 <span data-ttu-id="27ba2-1182">Vrátí hello vzdálenost mezi hello dva GeoJSON bodu, mnohoúhelníku nebo LineString výrazů.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1182">Returns hello distance between hello two GeoJSON Point, Polygon, or LineString expressions.</span></span>  
  
 <span data-ttu-id="27ba2-1183">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1183">**Syntax**</span></span>  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="27ba2-1184">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1184">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="27ba2-1185">Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1185">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="27ba2-1186">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1186">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1187">Vrátí číselný výraz obsahující hello vzdálenost.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1187">Returns a numeric expression containing hello distance.</span></span> <span data-ttu-id="27ba2-1188">Vyjadřuje se v měřidla pro hello výchozí odkaz na systém.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1188">This is expressed in meters for hello default reference system.</span></span>  
  
 <span data-ttu-id="27ba2-1189">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1189">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1190">Následující příklad ukazuje, jak Hello tooreturn všechny rodiny dokumenty, které jsou v rámci 30 km hello zadané umístění pomocí hello ST_DISTANCE integrovaná funkce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1190">hello following example shows how tooreturn all family documents that are within 30 km of hello specified location using hello ST_DISTANCE built-in function.</span></span> <span data-ttu-id="27ba2-1191">.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1191">.</span></span>  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 <span data-ttu-id="27ba2-1192">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1192">Here is hello result set.</span></span>  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <span data-ttu-id="27ba2-1193"><a name="bk_st_within"></a>ST_WITHIN</span><span class="sxs-lookup"><span data-stu-id="27ba2-1193"><a name="bk_st_within"></a> ST_WITHIN</span></span>  
 <span data-ttu-id="27ba2-1194">Vrátí výrazu logické hodnoty, která určuje, zda hello objekt GeoJSON (bod, mnohoúhelníku nebo LineString) zadaný v prvním argumentu hello je v rámci hello GeoJSON (bod, mnohoúhelníku nebo LineString) v druhý argument hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1194">Returns a Boolean expression indicating whether hello GeoJSON object (Point, Polygon, or LineString) specified in hello first argument is within hello GeoJSON (Point, Polygon, or LineString) in hello second argument.</span></span>  
  
 <span data-ttu-id="27ba2-1195">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1195">**Syntax**</span></span>  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="27ba2-1196">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1196">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="27ba2-1197">Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1197">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
 
-   `spatial_expr`  
  
     <span data-ttu-id="27ba2-1198">Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1198">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="27ba2-1199">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1199">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1200">Vrátí logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1200">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="27ba2-1201">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1201">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1202">Hello následující příklad ukazuje, jak toofind rodiny všechny dokumenty v rámci pomocí ST_WITHIN mnohoúhelníku.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1202">hello following example shows how toofind all family documents within a polygon using ST_WITHIN.</span></span>  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 <span data-ttu-id="27ba2-1203">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1203">Here is hello result set.</span></span>  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <span data-ttu-id="27ba2-1204"><a name="bk_st_intersects"></a>ST_INTERSECTS</span><span class="sxs-lookup"><span data-stu-id="27ba2-1204"><a name="bk_st_intersects"></a> ST_INTERSECTS</span></span>  
 <span data-ttu-id="27ba2-1205">Vrátí hodnotu označující, zda text hello objekt GeoJSON (bod, mnohoúhelníku nebo LineString) zadaný v prvním argumentu hello protíná hello GeoJSON (bod, mnohoúhelníku nebo LineString) v hello druhý argument logický výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1205">Returns a Boolean expression indicating whether hello GeoJSON object (Point, Polygon, or LineString) specified in hello first argument intersects hello GeoJSON (Point, Polygon, or LineString) in hello second argument.</span></span>  
  
 <span data-ttu-id="27ba2-1206">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1206">**Syntax**</span></span>  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="27ba2-1207">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1207">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="27ba2-1208">Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1208">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
 
-   `spatial_expr`  
  
     <span data-ttu-id="27ba2-1209">Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1209">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="27ba2-1210">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1210">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1211">Vrátí logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1211">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="27ba2-1212">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1212">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1213">Následující příklad ukazuje, jak Hello toofind hello všechny oblasti, které protíná s danou mnohoúhelníku.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1213">hello following example shows how toofind all areas that intersects with hello given polygon.</span></span>  
  
```  
SELECT a.id   
FROM Areas a   
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 <span data-ttu-id="27ba2-1214">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1214">Here is hello result set.</span></span>  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <span data-ttu-id="27ba2-1215"><a name="bk_st_isvalid"></a>ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="27ba2-1215"><a name="bk_st_isvalid"></a> ST_ISVALID</span></span>  
 <span data-ttu-id="27ba2-1216">Vrátí logickou hodnotu udávající, zda zadaný hello GeoJSON bodu, mnohoúhelníku nebo LineString výraz není platný.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1216">Returns a Boolean value indicating whether hello specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span>  
  
 <span data-ttu-id="27ba2-1217">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1217">**Syntax**</span></span>  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 <span data-ttu-id="27ba2-1218">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1218">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="27ba2-1219">Je platný výraz GeoJSON bodu, mnohoúhelníku nebo LineString.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1219">Is any valid GeoJSON Point, Polygon, or LineString expression.</span></span>  
  
 <span data-ttu-id="27ba2-1220">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1220">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1221">Vrátí logický výraz.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1221">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="27ba2-1222">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1222">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1223">Následující příklad ukazuje, jak Hello toocheck, pokud je bod platný pomocí ST_VALID.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1223">hello following example shows how toocheck if a point is valid using ST_VALID.</span></span>  
  
 <span data-ttu-id="27ba2-1224">Například tohoto bodu má hodnotu zeměpisnou šířku, která není v platném rozsahu hodnot [-90, 90] hello, takže hello dotaz vrátí hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1224">For example, this point has a latitude value that's not in hello valid range of values [-90, 90], so hello query returns false.</span></span>  
  
 <span data-ttu-id="27ba2-1225">Pro mnohoúhelníky hello GeoJSON specifikace vyžaduje, aby hello poslední souřadnic pár poskytuje hello stejné jako první, toocreate hello uzavřený obrazec.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1225">For polygons, hello GeoJSON specification requires that hello last coordinate pair provided should be hello same as hello first, toocreate a closed shape.</span></span> <span data-ttu-id="27ba2-1226">Je třeba zadat body v rámci mnohoúhelníku v pořadí proti směru hodinových ručiček.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1226">Points within a polygon must be specified in counter-clockwise order.</span></span> <span data-ttu-id="27ba2-1227">Mnohoúhelníku zadaný v po směru hodinových ručiček pořadí představuje hello inverzní oblasti hello v něm.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1227">A polygon specified in clockwise order represents hello inverse of hello region within it.</span></span>  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })  
```  
  
 <span data-ttu-id="27ba2-1228">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1228">Here is hello result set.</span></span>  
  
```  
[{ "$1": false }]  
```  
  
####  <span data-ttu-id="27ba2-1229"><a name="bk_st_isvaliddetailed"></a>ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="27ba2-1229"><a name="bk_st_isvaliddetailed"></a> ST_ISVALIDDETAILED</span></span>  
 <span data-ttu-id="27ba2-1230">Vrátí hodnotu JSON obsahující logickou hodnotu, pokud hello Zadaný bod GeoJSON, mnohoúhelníku nebo LineString výraz je platná a pokud je neplatná, kromě hello důvod jako hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1230">Returns a JSON value containing a Boolean value if hello specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally hello reason as a string value.</span></span>  
  
 <span data-ttu-id="27ba2-1231">**Syntaxe**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1231">**Syntax**</span></span>  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 <span data-ttu-id="27ba2-1232">**Argumenty**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1232">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="27ba2-1233">Je jakýkoli platný výraz bodu nebo mnohoúhelníku GeoJSON.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1233">Is any valid GeoJSON point or polygon expression.</span></span>  
  
 <span data-ttu-id="27ba2-1234">**Návratové typy**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1234">**Return Types**</span></span>  
  
 <span data-ttu-id="27ba2-1235">Vrátí hodnotu JSON obsahující logickou hodnotu, pokud hello určena GeoJSON bodu nebo mnohoúhelníku výrazu je platný a pokud je neplatná, kromě hello důvod jako hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1235">Returns a JSON value containing a Boolean value if hello specified GeoJSON point or polygon expression is valid, and if invalid, additionally hello reason as a string value.</span></span>  
  
 <span data-ttu-id="27ba2-1236">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="27ba2-1236">**Examples**</span></span>  
  
 <span data-ttu-id="27ba2-1237">Následující příklad, jak Hello toocheck platnosti (s podrobnostmi) pomocí ST_ISVALIDDETAILED.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1237">hello following example how toocheck validity (with details) using ST_ISVALIDDETAILED.</span></span>  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
})  
```  
  
 <span data-ttu-id="27ba2-1238">Zde je sada výsledků dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="27ba2-1238">Here is hello result set.</span></span>  
  
```  
[{  
  "$1": {   
    "valid": false,   
    "reason": "hello Polygon input is not valid because hello start and end points of hello ring number 1 are not hello same. Each ring of a polygon must have hello same start and end points."   
  }  
}]  
```  
  
## <a name="next-steps"></a><span data-ttu-id="27ba2-1239">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27ba2-1239">Next steps</span></span>  
 <span data-ttu-id="27ba2-1240">[Syntaxe jazyka SQL a dotaz SQL pro Azure Cosmos DB](documentdb-sql-query.md) </span><span class="sxs-lookup"><span data-stu-id="27ba2-1240">[SQL syntax and SQL query for Azure Cosmos DB](documentdb-sql-query.md) </span></span>  
 [<span data-ttu-id="27ba2-1241">Dokumentace k Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="27ba2-1241">Azure Cosmos DB documentation</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/)  
  
  
