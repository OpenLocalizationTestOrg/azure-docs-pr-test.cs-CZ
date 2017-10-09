---
Title: aaa "rozhraní API služby Azure Cosmos databáze DocumentDB: syntaxe SQL | Microsoft Docs"Popis: referenční dokumentace pro hello dotazovacího jazyka SQL rozhraní API Azure Cosmos databáze DocumentDB.
služby: cosmos-db Autor: mimig1 správce: jhubbard editor: mimig documentationcenter: "

MS.AssetID: ms.service: ms.workload cosmos-db: datové služby ms.tgt_pltfrm: na ms.devlang: na ms.topic: referenční ms.date: 13/06/2017 ms.author: mimig

---

# <a name="azure-cosmos-db-documentdb-api-sql-syntax-reference"></a>Azure DocumentDB Cosmos DB rozhraní API: Reference syntaxe SQL

Hello rozhraní API služby Azure Cosmos databáze DocumentDB podporuje dotazování dokumentů pomocí známých SQL (Structured Query Language) jako gramatika přes hierarchické dokumenty JSON bez nutnosti explicitního schématu nebo vytváření sekundárních indexů. Toto téma obsahuje referenční dokumentaci k nástroji pro hello dotazovací jazyk DocumentDB SQL rozhraní API.

Návod hello dotazovací jazyk DocumentDB SQL rozhraní API najdete v tématu [dotazy SQL pro rozhraní API služby Azure Cosmos databáze DocumentDB](documentdb-sql-query.md).  
  
Také doporučujeme toovisit hello [Query Playground](http://www.documentdb.com/sql/demo) kde mohou zkuste Azure Cosmos DB a spouštění dotazů SQL na naší datové sadě.  
  
## <a name="select-query"></a>Vyberte možnost dotazu  
Načte z databáze hello dokumentů JSON. Podporuje vyhodnocení výrazu, projekce, filtrování a připojí.  Hello s konvencemi použitými pro popisující příkazů SELECT hello jsou v tabulce v hello části konvence syntaxe.  
  
**Syntaxe**  
  
```
<select_query> ::=  
SELECT <select_specification>   
    [ FROM <from_specification>]   
    [ WHERE <filter_condition> ]  
    [ ORDER BY <sort_specification> ]  
```  
  
 **Poznámky**  
  
 Viz následující části Podrobnosti na každý klauzule:  
  
-   [Klauzule SELECT](#bk_select_query)  
  
-   [FROM – klauzule](#bk_from_clause)  
  
-   [Klauzule WHERE](#bk_where_clause)  
  
-   [Klauzuli ORDER by](#bk_orderby_clause)  
  
klauzule Hello v příkazu SELECT hello musejí být seřazeny, jak je uvedeno výše. Kterákoli z klauzule volitelné hello lze vynechat. Ale pokud volitelné klauzule používají, musí být ve správném pořadí hello.  
  
**Logické pořadí zpracování příkazu SELECT hello**  
  
Hello pořadí, ve kterém jsou zpracovány klauzule je:  

1.  [FROM – klauzule](#bk_from_clause)  
2.  [Klauzule WHERE](#bk_where_clause)  
3.  [Klauzuli ORDER by](#bk_orderby_clause)  
4.  [Klauzule SELECT](#bk_select_query)  

Všimněte si, že se to neliší od hello pořadí, ve kterém se zobrazí v syntaxi hello. řazení Hello je tak, aby všechny nové symboly zavedených v klauzuli zpracovaná viditelné a zda lze použít v klauzulích zpracovat později. Pro instanci, jsou dostupné aliasy deklarované v klauzuli FROM tam, kde a klauzule FROM.  

**Prázdné znaky a komentáře**  

Všechny znaky prázdné znaky, které nejsou součástí řetězec v uvozovkách nebo identifikátor v uvozovkách nejsou součástí hello jazyk gramatika a jsou ignorovány během analýzy.  

Hello dotazovací jazyk podporuje komentáře styl T-SQL jako  

-   Příkaz jazyka SQL`-- comment text [newline]`  

Při prázdné znaky a komentáře v není k dispozici žádné násobek hello gramatika, musí být použité tooseparate tokeny. Pro instanci: `-1e5` je jeden číslo tokenu, chvíli`: – 1 e5` token znaménkem minus následuje identifikátor e5 a číslo 1.  

##  <a name="bk_select_query"></a>Klauzule SELECT  
klauzule Hello v příkazu SELECT hello musejí být seřazeny, jak je uvedeno výše. Kterákoli z klauzule volitelné hello lze vynechat. Ale pokud volitelné klauzule používají, musí být ve správném pořadí hello.  

**Syntaxe**  
```  
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | <object_property_list>   
      | VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
 **Argumenty**  
  
 `<select_specification>`  
  
 Nastavit vlastnosti nebo hodnota toobe vybraná pro výsledek hello.  
  
 `'*'`  
  
Určuje, že mají být načtena hodnota hello beze změn. Konkrétně Pokud hello zpracovat hodnota objektu, budou všechny vlastnosti načteny.  
  
 `<object_property_list>`  
  
Určuje seznam hello toobe vlastnosti načteny. Každý vrácená hodnota bude objekt s byly zadány vlastnosti hello.  
  
`VALUE`  
  
Určuje, že mají být načtena hodnota JSON hello místo hello kompletní objekt JSON. To, na rozdíl od `<property_list>` nezalamuje hello projektovat hodnoty v objektu.  
  
`<scalar_expression>`  
  
Výraz představující hodnotu toobe hello počítaný. V tématu [skalární výrazy](#bk_scalar_expressions) podrobnosti.  
  
**Poznámky**  
  
Hello `SELECT *` syntaxe je platný, pokud klauzule FROM deklaruje právě jednu alias. `SELECT *`poskytuje identitu projekce, které mohou být užitečné, pokud není nutná žádná projekce. Vyberte * platí, pouze pokud je zadána klauzule FROM a zavedl pouze jeden vstupní zdroj.  
  
Všimněte si, že `SELECT <select_list>` a `SELECT *` jsou "syntaktické sugar" a může být případně vyjádřený pomocí jednoduchého příkazů SELECT, jak je uvedeno níže.  
  
1.  `SELECT * FROM ... AS from_alias ...`  
  
     je ekvivalentní:  
  
     `SELECT from_alias FROM ... AS from_alias ...`  
  
2.  `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
     je ekvivalentní:  
  
     `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
**Viz také**  
  
[Skalární výrazy](#bk_scalar_expressions)  
[Klauzule SELECT](#bk_select_query)  
  
##  <a name="bk_from_clause"></a>FROM – klauzule  
Určuje zdroj hello nebo připojený k zdroje. klauzule FROM Hello je volitelný. Pokud není zadaný, další klauzule bude proveden stále jako, pokud klauzule FROM zadaný jednotlivý dokument.  
  
**Syntaxe**  
  
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
  
**Argumenty**  
  
`<from_source>`  
  
Určuje zdroj dat s nebo bez alias. Pokud není zadán alias, bude odvodit z hello `<collection_expression>` pomocí následujících pravidel:  
  
-   Pokud je výraz hello název_kolekce, bude Název_kolekce použít jako alias.  
  
-   Pokud je výraz hello `<collection_expression>`, pak %{Property_Name/ pak %{Property_Name/ se použije jako alias. Pokud je výraz hello název_kolekce, bude Název_kolekce použít jako alias.  
  
STEJNĚ JAKO`input_alias`  
  
Určuje, že hello `input_alias` je sada hodnot vrácených hello základní kolekce výraz.  
 
`input_alias`V  
  
Určuje, že hello `input_alias` by měl představovat hello sadu hodnot získat iterování přes všechny elementy pole každé pole vrácené hello základní kolekce výraz. Libovolná hodnota vrácený základní kolekce výraz, který není pole se ignoruje.  
  
`<collection_expression>`  
  
Určuje, že hello kolekce výraz toobe použité tooretrieve hello dokumenty.  
  
`ROOT`  
  
Určuje, že tento dokument mají být načtena z hello výchozím aktuálně připojené kolekce.  
  
`collection_name`  
  
Určuje, že tento dokument mají být načtena z hello zadané kolekce. Název Hello hello kolekce musí odpovídat názvu hello hello kolekce aktuálně připojené k.  
  
`input_alias`  
  
Určuje, že dokumentu mají být načtena z hello jiný zdroj definované hello zadaný alias.  
  
`<collection_expression> '.' property_`  
  
Určuje v tomto dokumentu mají být načtena díky přístupu k hello `property_name` element pole vlastnosti nebo array_index pro všechny dokumenty načíst zadaný výraz kolekce.  
  
`<collection_expression> '[' "property_name" | array_index ']'`  
  
Určuje v tomto dokumentu mají být načtena díky přístupu k hello `property_name` element pole vlastnosti nebo array_index pro všechny dokumenty načíst zadaný výraz kolekce.  
  
**Poznámky**  
  
Všechny aliasy zadáno nebo odvození v hello `<from_source>(`s) musí být jedinečný. Hello syntaxe `<collection_expression>.`%{Property_Name/ je hello stejný jako `<collection_expression>' ['"property_name"']'`. Druhé syntaxe hello však může být použita, pokud název vlastnosti obsahuje jiný identifikátor – znaky.  
  
**Chybí vlastnosti, chybějící pole prvky, není definovaná hodnoty zpracování**  
  
Pokud výraz kolekce přístup k vlastnosti nebo elementy pole a že hodnota neexistuje, bude se tato hodnota ignorována a není další zpracování.  
  
**Kolekce výraz kontextu rozsahu**  
  
Výraz kolekce může být celou kolekci nebo obor dokumentu:  
  
-   Výraz je celou kolekci, pokud hello podkladovém zdroji hello kolekce výrazu je buď KOŘENOVÉ nebo `collection_name`. Takové výraz představuje sadu dokumenty načíst přímo z kolekce hello a není závislá na zpracování hello výrazů jiné kolekce.  
  
-   Výraz je obor dokument, pokud hello podkladovém zdroji hello kolekce výrazu `input_alias` zavedená dříve v dotazu hello. Takové výraz představuje sadu získala při vyhodnocování výrazu kolekce hello v oboru hello každý dokument patřící toohello sady přidružené hello alias kolekce dokumentů.  Výsledná sada Hello bude sjednocení sad získala při vyhodnocování výrazu hello kolekce pro jednotlivé dokumenty hello v hello základní sady.  
  
**Spojení**  
  
V aktuální verzi hello podporuje Azure Cosmos DB vnitřní spojení. Připojení k další možnosti jsou chystaný.

Vnitřní spojení výsledek v dokončení smíšený produkt hello nastaví účastní spojení hello. výsledek Hello spojení N-způsob, jak je sada N element řazené kolekce členů, kde každé hodnoty v řazené kolekce členů hello je spojena s aliasy hello nastavení účasti v hello spojení a je přístupný pomocí odkazující na tento alias v jiných klauzulích.  
  
vyhodnocení Hello hello spojení, závisí na hello kontextu oboru hello účasti sady:  
  
-  Spojení mezi sadu kolekce A a celou kolekci nastavení B, výsledky v produktu mezi všechny elementy ve sady A a B.
  
-   Spojení mezi sada A a sada obor dokumentu B, výsledkem spojení všech sad získat vyhodnocením dokumentu obor sady B pro každý dokument z nastavit A.  
  
 V aktuální verzi hello maximálně jeden výraz celou kolekci podporuje procesor dotazů hello.  
  
**Příklady spojení:**  
  
Podívejme se na následující klauzule FROM hello:`<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  
  
 Umožní každý zdroj definovat `input_alias1, input_alias2, …, input_aliasN`. Tato klauzule FROM vrací sadu N-řazené kolekce členů (řazené kolekce členů s hodnotami N). Každá řazená kolekce členů má vyprodukované všechny aliasy kolekce iterování přes jejich příslušné sady hodnot.  
  
*Připojit ke Příklad 1, s 2 zdrojů:*  
  
- Umožní `<from_source1>` celou kolekci být a představují sadu {A, B, C}.  
  
- Umožní `<from_source2>` být obor dokumentu, odkazující na input_alias1 a představují sady:  
  
    {1, 2} pro`input_alias1 = A,`  
  
    {3} pro`input_alias1 = B,`  
  
    {4, 5} pro`input_alias1 = C,`  
  
- Hello klauzule FROM `<from_source1> JOIN <from_source2>` bude mít za následek hello následující řazených kolekcí členů:  
  
    (`input_alias1, input_alias2`):  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
*Příklad 2, s 3 zdroje připojení:*  
  
- Umožní `<from_source1>` celou kolekci být a představují sadu {A, B, C}.  
  
- Umožní `<from_source2>` být obor dokumentu odkazující na `input_alias1` a představují sady:  
  
    {1, 2} pro`input_alias1 = A,`  
  
    {3} pro`input_alias1 = B,`  
  
    {4, 5} pro`input_alias1 = C,`  
  
- Umožní `<from_source3>` být obor dokumentu odkazující na `input_alias2` a představují sady:  
  
    {100, 200} pro`input_alias2 = 1,`  
  
    {300} pro`input_alias2 = 3,`  
  
- Hello klauzule FROM `<from_source1> JOIN <from_source2> JOIN <from_source3>` bude mít za následek hello následující řazených kolekcí členů:  
  
    (input_alias1, input_alias2, input_alias3):  
  
    (A, 1, 100), (A, 1, 200), (B, 3, 300)  
  
> [!NOTE]
> Nedostatečná řazené kolekce členů pro jiné hodnoty `input_alias1`, `input_alias2`, pro které hello `<from_source3>` nevrátil žádné hodnoty.  
  
*Připojit ke příklad 3, s 3 zdrojů:*  
  
- Umožní < from_source1 > být celou kolekci a představují sadu {A, B, C}.  
  
- Umožní `<from_source1>` celou kolekci být a představují sadu {A, B, C}.  
  
- Umožní < from_source2 > být obor dokumentu odkazující input_alias1 a představují sady:  
  
    {1, 2} pro`input_alias1 = A,`  
  
    {3} pro`input_alias1 = B,`  
  
    {4, 5} pro`input_alias1 = C,`  
  
- Umožní `<from_source3>` příliš vymezovat`input_alias1` a představují sady:  
  
    {100, 200} pro`input_alias2 = A,`  
  
    {300} pro`input_alias2 = C,`  
  
- Hello klauzule FROM `<from_source1> JOIN <from_source2> JOIN <from_source3>` bude mít za následek hello následující řazených kolekcí členů:  
  
    (`input_alias1, input_alias2, input_alias3`):  
  
    (A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200), (C, 4, 300) (C, 5, 300)  
  
> [!NOTE]
> To bylo způsobeno v smíšený produkt mezi `<from_source2>` a `<from_source3>` vzhledem k tomu, že jsou obě vymezená toohello stejné `<from_source1>`.  To bylo způsobeno 4 (2 x 2) řazených kolekcí členů má hodnotu, 0 řazené kolekce členů s hodnota B (1 × 0) a 2 (2 × 1) řazených kolekcí členů mají hodnotu C.  
  
**Viz také**  
  
 [Klauzule SELECT](#bk_select_query)  
  
##  <a name="bk_where_clause"></a>Klauzule WHERE  
 Určuje podmínku hello vyhledávání pro hello dokumenty vrácené dotazem hello.  
  
 **Syntaxe**  
  
```  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
 **Argumenty**  
  
-   `<filter_condition>`  
  
     Určuje toobe hello podmínky splněny hello dokumenty toobe vrátila.  
  
-   `<scalar_expression>`  
  
     Výraz představující hodnotu toobe hello počítaný. V tématu hello [skalární výrazy](#bk_scalar_expressions) podrobnosti.  
  
 **Poznámky**  
  
 Aby hello vrátil dokumentu toobe výraz zadaný jako podmínku filtrování se musí vyhodnotit tootrue. Pouze logickou hodnotu true budou splňovat podmínky hello, jakoukoli jinou hodnotu: nedefinované, null, hodnotu false, číslo, pole nebo objekt nebudou splňovat podmínky hello.  
  
##  <a name="bk_orderby_clause"></a>Klauzuli ORDER by  
 Určuje hello řazení pořadí výsledků vrácených dotazem hello.  
  
 **Syntaxe**  
  
```  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= <scalar_expression> [ASC | DESC]  
  
```  
  
 **Argumenty**  
  
-   `<sort_specification>`  
  
     Určuje vlastnosti nebo výraz, ve kterém sada výsledků dotazu hello toosort. Sloupec pro řazení lze zadat jako název nebo sloupci alias.  
  
     Je možné zadat více řazení sloupců. Názvy sloupců musí být jedinečný. pořadí Hello hello řazení sloupců v klauzuli pořadí podle hello definuje hello organizace hello Seřazená sada výsledků dotazu. To znamená sadu výsledků hello řazena podle hello první vlastnost a potom tento seřazený seznam je seřazen podle hello druhou vlastností a tak dále.  
  
     názvy sloupců Hello odkazovat v klauzuli pořadí podle hello musí odpovídat tooeither sloupec v hello vyberte sloupec seznamu nebo tooa definovaná v tabulce, zadaný v klauzuli FROM hello bez jakékoli nejednoznačnosti.  
  
-   `<sort_expression>`  
  
     Určuje vlastnosti jediné nebo výrazu v sadě výsledků dotazu které toosort hello.  
  
-   `<scalar_expression>`  
  
     V tématu hello [skalární výrazy](#bk_scalar_expressions) podrobnosti.  
  
-   `ASC | DESC`  
  
     Určuje, že hello hodnoty zadané sloupce hello by měly být seřazeny ve vzestupném nebo sestupném pořadí. ASC seřadí od hello nejnižší hodnotu toohighest hodnoty. DESC seřadí z hodnoty toolowest nejvyšší hodnotu. ASC je hello výchozí pořadí řazení. Hodnoty Null jsou považovány za hello nejnižší možné hodnoty.  
  
 **Poznámky**  
  
 I když gramatiky dotazů hello podporuje více pořadí podle vlastností, hello Azure Cosmos DB dotazu runtime podporuje řazení pouze s jedinou vlastností a pouze s názvy vlastností, tj., není pro počítané vlastnosti. Řazení také vyžaduje, že hello indexování zásad zahrnuje indexem rozsahu pro vlastnost hello a hello zadaný typ s maximální přesnost hello. Najdete v dokumentaci k zásadám podrobnosti indexování toohello.  
  
##  <a name="bk_scalar_expressions"></a>Skalární výrazy  
 Skalární výraz, který je kombinací symbolů a operátory, které se dají posoudit tooobtain jednu hodnotu. Jednoduché výrazy může být konstanty, odkazy na vlastnost, odkazuje na element pole, odkazy na alias nebo volání funkce. Jednoduché výrazy lze spojovat do složité výrazy pomocí operátorů.  
  
 Podrobnosti na hodnotách, které skalární výraz, který může mít najdete v tématu [konstanty](#bk_constants) části.  
  
 **Syntaxe**  
  
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
  
 **Argumenty**  
  
-   `<constant>`  
  
     Představuje konstantní hodnotu. V tématu [konstanty](#bk_constants) podrobnosti.  
  
-   `input_alias`  
  
     Reprezentuje hodnotu definované hello `input_alias` byla zavedená v hello `FROM` klauzule.  
    Tato hodnota je zaručeno toonot být **nedefinované** –**nedefinované** hodnoty ve vstupu hello se přeskočí.  
  
-   `<scalar_expression>.property_name`  
  
     Reprezentuje hodnotu hello vlastnosti objektu. Pokud hello vlastnost neexistuje nebo vlastnost odkazuje na hodnotu, která není objekt a potom hello výraz vyhodnotí příliš**nedefinované** hodnotu.  
  
-   `<scalar_expression>'['"property_name"|array_index']'`  
  
     Reprezentuje hodnotu hello vlastnost s názvem `property_name` nebo element pole s indexem `array_index` objekt nebo pole. Pokud vlastnost nebo pole indexu je hello neexistuje nebo hello vlastnost nebo pole indexu je odkazováno na hodnotu, která není objekt nebo pole, vyhodnotí výraz hello tooundefined hodnotu.  
  
-   `unary_operator <scalar_expression>`  
  
     Představuje operátor, který je použité tooa jedna hodnota. V tématu [operátory](#bk_operators) podrobnosti.  
  
-   `<scalar_expression> binary_operator <scalar_expression>`  
  
     Představuje operátor, který je použité tootwo hodnoty. V tématu [operátory](#bk_operators) podrobnosti.  
  
-   `<scalar_function_expression>`  
  
     Reprezentuje hodnotu definované výsledku volání funkce.  
  
-   `udf_scalar_function`  
  
     Jméno uživatele, hello definované skalární funkce.  
  
-   `builtin_scalar_function`  
  
     Název předdefinované skalární funkce hello.  
  
-   `<create_object_expression>`  
  
     Představuje hodnotu získat tak, že vytvoříte nový objekt v zadaných vlastností a jejich hodnoty.  
  
-   `<create_array_expression>`  
  
     Reprezentuje hodnotu získala při vytváření nové pole se zadanými hodnotami, jako elementy  
  
-   `parameter_name`  
  
     Reprezentuje hodnotu hello zadaný název parametru. Názvy parametrů musí mít jeden @ jako první znak hello.  
  
 **Poznámky**  
  
 Při volání předdefinované nebo uživatel definované skalární funkce musí být definovány všechny argumenty. Pokud je některý z argumentů hello nedefinované, nebude volána funkce hello a hello výsledkem bude definován.  
  
 Při vytvoření objektu, bude přeskočen a nejsou zahrnuty v hello vytvořený objekt jakákoli vlastnost, která je přiřazena nedefinovanou hodnotu.  
  
 Při vytváření pole, libovolná hodnota elementu, který je přiřazen **nedefinované** hodnota bude přeskočen a nejsou zahrnuty v objektu vytvořeny hello. To způsobí, že hello vedle definovaný element tootake jeho místo tak nebude tohoto pole hello vytvořili přeskočili indexy.  
  
##  <a name="bk_operators"></a>Operátory  
 Tato část popisuje operátory hello podporována. Každý operátor může být přiřazené tooexactly jednu kategorii.  
  
 V tématu **operátor kategorie** tabulce podrobnosti ohledně zpracování **nedefinované** hodnoty, požadavky na typ vstupní hodnoty a zpracování hodnot s není odpovídající typy.  
  
 **Operátor kategorie:**  
  
|**Kategorie**|**Podrobnosti**|  
|-|-|  
|**aritmetické operace**|Operátor očekává input(s) toobe čísel. Výstup je také číslo. Pokud je některý z hello vstupů **nedefinované** nebo typ než číslo potom hello výsledek **nedefinované**.|  
|**bitové operace**|Operátor očekává input(s) toobe 32bitové číslo se znaménkem čísel. Výstup je také 32bitové číslo se znaménkem číslo.<br /><br /> Libovolná hodnota není celé číslo se zaokrouhlí. Kladné celé číslo se zaokrouhlí směrem dolů, záporné hodnoty zaokrouhlený nahoru.<br /><br /> Jakoukoli hodnotu, která je mimo rozsah 32bitové celé číslo hello budou převedeny provedením poslední 32 bity jeho dva pro zápis doplňku.<br /><br /> Pokud je některý z hello vstupů **nedefinované** nebo jiného typu než číslo, pak je výsledek hello **nedefinované**.<br /><br /> **Poznámka:** hello výše chování je kompatibilní s chováním bitový operátor jazyka JavaScript.|  
|**logické**|Operátor očekává input(s) toobe Boolean(s). Výstup je také logickou hodnotu.<br />Pokud je některý z hello vstupů **nedefinované** nebo jiného typu než logickou hodnotu, bude výsledkem hello **nedefinované**.|  
|**porovnání**|Operátor očekává input(s) toohave hello stejný typ a nesmí být definován. Výstup je logická hodnota.<br /><br /> Pokud je některý z hello vstupů **nedefinované** nebo hello vstupy mají různé typy a pak je výsledek hello **nedefinované**.<br /><br /> V tématu **řazení hodnot pro porovnání** tabulky pro hodnotu řazení podrobnosti.|  
|**řetězec**|Operátor očekává input(s) toobe řetězec nebo řetězce. Výstup je také řetězec.<br />Pokud je některý z hello vstupů **nedefinované** nebo typ jiný než řetězec pak hello výsledek je **nedefinované**.|  
  
 **Unární operátory:**  
  
|**Název**|**Operátor**|**Podrobnosti**|  
|-|-|-|  
|**aritmetické operace**|+<br /><br /> -|Vrátí hello číselnou hodnotu.<br /><br /> Bitovou negaci. Vrátí Negované číselnou hodnotu.|  
|**bitové operace**|~|Ty, které se doplňku. Vrátí zbytek číselnou hodnotu.|  
|**Logické**|**NENÍ**|Negace. Vrátí Negované logická hodnota.|  
  
 **Binární operátory:**  
  
|**Název**|**Operátor**|**Podrobnosti**|  
|-|-|-|  
|**aritmetické operace**|+<br /><br /> -<br /><br /> *<br /><br /> /<br /><br /> %|Přidání.<br /><br /> Odčítání.<br /><br /> Násobení.<br /><br /> Dělení.<br /><br /> Modulační.|  
|**bitové operace**|&#124;<br /><br /> &<br /><br /> ^<br /><br /> <<<br /><br /> >><br /><br /> >>>|Bitový operátor OR.<br /><br /> Bitové operace AND.<br /><br /> Bitové operace XOR.<br /><br /> Operátor posunu vlevo.<br /><br /> Posunutí doprava.<br /><br /> Posunutí doprava výplně nula.|  
|**logické**|**A**<br /><br /> **OR**|Logické spojení. Vrátí **true** Pokud jsou oba argumenty **true**, vrátí **false** jinak.<br /><br /> Logické spojení. Vrátí **true** Pokud jsou oba argumenty **true**, vrátí **false** jinak.|  
|**porovnání**|**=**<br /><br /> **!=, <>**<br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> **??**|Rovná se. Vrátí **true** Pokud argumenty jsou stejné, vrátí **false** jinak.<br /><br /> Není rovno. Vrátí **true** Pokud argumenty nejsou stejné, vrátí **false** jinak.<br /><br /> Větší než. Vrátí **true** Pokud první argument je větší než druhá text hello, vrátí **false** jinak.<br /><br /> Větší než nebo rovna hodnotě. Vrátí **true** Pokud první argument je větší než nebo roven toohello druhá, vrátí **false** jinak.<br /><br /> Menší než. Vrátí **true** Pokud první argument je menší než druhá text hello, vrátí **false** jinak.<br /><br /> Menší než nebo rovno. Vrátí **true** Pokud první argument je menší než nebo rovna toohello druhé jeden návratový **false** jinak.<br /><br /> Sloučení. Vrátí hello druhý argument, pokud je hello argument **nedefinované** hodnotu.|  
|**Řetězec**|**&#124;&#124;**|Zřetězení. Vrátí zřetězení oba argumenty.|  
  
 **Ternární operátory:**  
  
|Ternární operátor|?|Vrátí hello druhý argument, pokud je první argument hello výsledkem příliš**true**; jinak vrátí třetí argument hello.|  
|-|-|-|  
  
 **Řazení hodnot pro porovnání**  
  
|**Typ**|**Pořadí hodnoty**|  
|-|-|  
|**Nedefinovaná**|Není porovnatelný.|  
|**Hodnotu Null**|Jedna hodnota: **hodnotu null.**|  
|**Číslo**|Fyzická reálné číslo.<br /><br /> Zápornou hodnotu Infinity je menší než ostatní číselnou hodnotu.<br /><br /> Je větší než jakoukoli jinou hodnotu, číslo kladné nekonečnou hodnotu. **NaN** hodnota není porovnatelný. Porovnávání s **NaN** bude mít za následek **nedefinované** hodnotu.|  
|**Řetězec**|Lexicographical pořadí.|  
|**Pole**|Žádné řazení, ale přiměřenou.|  
|**Objekt**|Žádné řazení, ale přiměřenou.|  
  
 **Poznámky**  
  
 V Azure DB Cosmos hello typy hodnot často není známý, dokud se ve skutečnosti načítají z databáze hello. Většina hello operátorů v pořadí toosupport efektivní provádění dotazů má požadavky striktní typu. Operátory samy o sobě taky neprovádět implicitní převody.  
  
 To znamená, že jako dotaz: vybrat * z ROOT r kde r.Age = 21 vrátí jenom dokumenty s vlastností číslo rovno toohello stáří 21. Dokumenty s vlastnost stáří rovna toohello řetězec "21" nebo hello řetězec "0021" nebude odpovídat jako hello výraz "21" = 21 vyhodnotí tooundefined. To umožňuje lepší využití indexy, protože vyhledávání hello konkrétní hodnoty (tj. počet 21) je rychlejší než vyhledávání nekonečný počet potenciální odpovídá (tj. počet 21 nebo řetězce "21", "021", "21.0"...). To se liší od způsobu JavaScript vyhodnotí operátory na hodnoty různých typů.  
  
 **Porovnání rovnosti pole a objekty a**  
  
 Porovnání hodnot pole nebo objekt pomocí operátorů rozsah (>, > =, <, < =) bude mít za následek není definována jako není pořadí na objekt nebo pole hodnot. Ale pomocí operátory rovnosti/nerovnosti (=,! =, <>) je podporována a hodnoty budou porovnány strukturálně.  
  
 Pole jsou stejné, pokud obě pole mají stejný počet elementů a prvky v odpovídajícím pozic jsou také stejné. Pokud porovnávání žádném páru elementů výsledkem nedefinované, hello výsledku porovnání pole není definováno.  
  
 Objekty jsou stejné, pokud mají oba objekty stejné vlastnosti definované a jsou také stejné hodnoty odpovídající vlastnosti. Pokud porovnávání všechny dvojice hodnot vlastností výsledkem nedefinované, hello výsledku porovnání objekt není definován.  
  
##  <a name="bk_constants"></a>Konstanty  
 Konstanta, také známé jako literál nebo skalární hodnota, je symbol, který představuje konkrétní datové hodnoty. Formát Hello konstanta závisí na typu dat hello hello hodnoty, který představuje.  
  
 **Podporované typy skalární data:**  
  
|**Typ**|**Pořadí hodnoty**|  
|-|-|  
|**Nedefinovaná**|Jedna hodnota: **Nedefinovaná**|  
|**Hodnotu Null**|Jedna hodnota: **hodnotu null.**|  
|**Logická hodnota**|Hodnoty: **false**, **true**.|  
|**Číslo**|Dvojitá přesnost s plovoucí desetinnou čárkou číslo, IEEE 754 standard.|  
|**Řetězec**|Pořadí nula nebo více znaků Unicode. Řetězce musí být uzavřena do jednoduchých nebo dvojitých uvozovek.|  
|**Pole**|Pořadí počtu nula či více elementů. Každý element může být hodnota všechny skalární datového typu, s výjimkou Undefined.|  
|**Objekt**|Neseřazený sadu nula nebo více dvojic název hodnota. Název je řetězec znaků Unicode, hodnota může být jakékoli skalární datového typu, s výjimkou **Undefined**.|  
  
 **Syntaxe**  
  
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
  
 **Argumenty**  
  
1.  `<undefined_constant>; undefined`  
  
     Představuje není definovaná hodnota typu Undefined.  
  
2.  `<null_constant>; null`  
  
     Představuje **null** hodnotu typu **Null**.  
  
3.  `<boolean_constant>`  
  
     Představuje konstanta typu Boolean.  
  
4.  `false`  
  
     Představuje **false** hodnota typu Boolean.  
  
5.  `true`  
  
     Představuje **true** hodnota typu Boolean.  
  
6.  `<number_constant>`  
  
     Představuje konstantu.  
  
7.  `decimal_literal`  
  
     Decimal literály jsou reprezentované pomocí zápisu s tečkou nebo exponenciální notace čísla.  
  
8.  `hexadecimal_literal`  
  
     Šestnáctkové literály jsou reprezentované pomocí předpony '0 x, za nímž následuje jeden nebo více šestnáctkové číslice čísla.  
  
9. `<string_constant>`  
  
     Představuje konstantu typu řetězec.  
  
10. `string _literal`  
  
     Textové literály jsou reprezentována posloupnost nula nebo více znaků Unicode nebo řídicí sekvence řetězců v kódu Unicode. Textové literály jsou uzavřená do jednoduchých uvozovek (apostrof: ') nebo dvojité uvozovky (uvozovky: ").  
  
 Jsou povoleny následující řídicí sekvence:  
  
|**Řídicí sekvence**|**Popis**|**Znak Unicode**|  
|-|-|-|  
|\\'|apostrof (')|U + 0027|  
|\\"|dvojité uvozovky (")|U + 0022|  
|\\\|obrácené lomítko (\\)|U + 005C|  
|\\/|lomítko (/)|U + 002F|  
|\b|BACKSPACE|U + 0008|  
|\f|řídicí znak|U + 000C|  
|\n|Přechod na nový řádek|U + 000A|  
|\r|Návrat na začátek|U + 000D|  
|\t|Karta|U + 0009|  
|\uXXXX|Znak Unicode definované 4 hexadecimální číslice.|U + XXXX|  
  
##  <a name="bk_query_perf_guidelines"></a>Pravidla výkonu dotazu  
 Aby dotaz toobe, efektivně provést pro velké kolekce měla by používat filtry, které je možné dodávat prostřednictvím jednoho nebo více indexů.  
  
 Následující filtry Hello se bude zvažovat indexu vyhledávání:  
  
-   Operátor rovnosti (=) pomocí výrazu cesty dokumentu a konstanta.  
  
-   Rozsah operátory (<, \<=, >, > =) s výraz cesty dokumentu a číselné konstanty.  
  
-   Výraz cesty dokumentu zastupuje všechny výraz, který identifikuje konstantní cestu v dokumentech hello z kolekce hello odkazovaná databáze.  
  
 **Výraz cesty dokumentu**  
  
 Výrazy cesty dokumentu jsou výrazy, cesta vlastnost nebo pole posuzovatelů indexer přes dokumentu pocházejících z databáze kolekce dokumentů. Tato cesta může být použité tooidentify hello umístění hodnot odkazuje ve filtru přímo v rámci hello dokumenty v kolekci hello databáze.  
  
 Pro výrazu toobe považována za výraz cesty dokumentu by měl:  
  
1.  Referenční dokumentace hello kolekce root přímo.  
  
2.  Odkaz na vlastnost nebo konstanta indexer pole některé výrazu cesta dokumentu  
  
3.  Referenční alias, který představuje výraz cesty některé dokumentu.  
  
     **Syntaxe konvence**  
  
     Hello následující tabulka popisuje syntaxi toodescribe hello konvence použité v referenci hello dotazovací jazyk DocumentDB rozhraní API.  
  
    |**Konvence**|**Použít pro**|  
    |-|-|    
    |VELKÁ PÍSMENA|Klíčová slova velká a malá písmena.|  
    |malá písmena|Klíčová slova malá a velká písmena.|  
    |\<nonterminal >|Nonterminal, definované samostatně.|  
    |\<nonterminal >:: =|Syntaxe definice hello nonterminal.|  
    |other_terminal|Terminálové (token), podrobně popsané v slova.|  
    |Identifikátor|Identifikátor. Umožňuje následující pouze znaky: a – z A-Z 0-9 _First znak nemůže být číslice.|  
    |"řetězec"|Řetězec v uvozovkách. Umožňuje libovolný platný řetězec. Viz popis string_literal.|  
    |'symbol.|Literál symbol, který je součástí hello syntaxe.|  
    |&#124; (svislé čáry)|Alternativy pro položky syntaxe. Můžete použít pouze jednu z položky hello.|  
    |[] /(brackets)|Závorky uzavřete jeden nebo více nepovinných položek.|  
    |[,.. .n]|Označuje, že hello předcházející položky může být opakovaný n stanovený počet. Hello výskytů jsou oddělené čárkami.|  
    |[.. .n]|Označuje, že hello předcházející položky může být opakovaný n stanovený počet. Hello výskytů se oddělují mezerami.|  
  
##  <a name="bk_built_in_functions"></a>Integrované funkce  
 Azure Cosmos DB poskytuje mnoho předdefinovaných funkcí SQL. kategorie Hello integrované funkce jsou uvedeny níže.  
  
|Funkce|Popis|  
|--------------|-----------------|  
|[Matematické funkce](#bk_mathematical_functions)|Hello matematické funkce každé provedení výpočtů, obvykle podle vstupní hodnoty, které jsou k dispozici jako argumenty a vrátí číselnou hodnotu.|  
|[Typ kontroly funkce](#bk_type_checking_functions)|Kontrola, zda Funkce Hello typu Povolit toocheck hello typ výrazu v rámci dotazů SQL.|  
|[Funkce řetězců](#bk_string_functions)|Funkce řetězce Hello provést operaci s vstupní hodnotu řetězce a vrátí řetězec, číselnou nebo logická hodnota.|  
|[Funkce pole](#bk_array_functions)|Funkce pole Hello provést operaci na hodnotu vstupní pole a vrátí číselnou, logická hodnota nebo pole hodnota.|  
|[Prostorové funkce](#bk_spatial_functions)|Funkce prostorových Hello provést operaci s vstupní hodnotu prostorový objekt a vrátit hodnotu číselná nebo logická hodnota.|  
  
###  <a name="bk_mathematical_functions"></a>Matematické funkce  
 Hello následující funkce provedení výpočtů, obvykle podle vstupní hodnoty, které jsou k dispozici jako argumenty a vrátí číselnou hodnotu.  
  
||||  
|-|-|-|  
|[ABS](#bk_abs)|[ACOS](#bk_acos)|[ASIN](#bk_asin)|  
|[ATAN](#bk_atan)|[ATN2](#bk_atn2)|[MEZNÍ HODNOTY](#bk_ceiling)|  
|[COS](#bk_cos)|[COP](#bk_cot)|[STUPŇŮ](#bk_degrees)|  
|[EXP](#bk_exp)|[FLOOR](#bk_floor)|[PROTOKOLU](#bk_log)|  
|[LOG10](#bk_log10)|[PLATFORMY](#bk_pi)|[NAPÁJENÍ](#bk_power)|  
|[RADIÁNECH](#bk_radians)|[ZAOKROUHLIT](#bk_round)|[SIN](#bk_sin)|  
|[SQRT](#bk_sqrt)|[HRANATÉ](#bk_square)|[PŘIHLÁŠENÍ](#bk_sign)|  
|[TAN](#bk_tan)|[TRUNC](#bk_trunc)||  
  
####  <a name="bk_abs"></a>ABS  
 Vrátí hello absolutní (kladné) hello zadána hodnota číselného výrazu.  
  
 **Syntaxe**  
  
```  
ABS (<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad ukazuje výsledky hello pomocí funkce hello ABS na tři odlišné počty.  
  
```  
SELECT ABS(-1), ABS(0), ABS(1)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: 1, $2: 0, $3: 1}]  
```  
  
####  <a name="bk_acos"></a>ACOS  
 Vrátí hello úhel v radiánech, jehož kosinus je hello zadaný číselného výrazu; Zkratka Arkus.  
  
 **Syntaxe**  
  
```  
ACOS(<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad vrátí hello ACOS-1.  
  
```  
SELECT ACOS(-1)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_asin"></a>ASIN  
 Vrátí hello úhlu, v radiánech, jehož sinus je hello zadán číselný výraz. To je také označován Arkus sinus.  
  
 **Syntaxe**  
  
```  
ASIN(<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad vrátí hello ASIN-1.  
  
```  
SELECT ASIN(-1)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": -1.5707963267948966}]  
```  
  
####  <a name="bk_atan"></a>ATAN  
 Vrátí hello úhlu, v radiánech, jehož tangens je hello zadán číselný výraz. To je také označován Arkus.  
  
 **Syntaxe**  
  
```  
ATAN(<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Následující příklad, vrátí hello ATAN hello Hello zadané hodnotě.  
  
```  
SELECT ATAN(-45.01)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": -1.5485826962062663}]  
```  
  
####  <a name="bk_atn2"></a>ATN2  
 Vrací hlavní hodnotu hello hello tangens oblouk y / x, vyjádřené v radiánech.  
  
 **Syntaxe**  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad vypočítá hello ATN2 pro zadaný hello x a y součásti.  
  
```  
SELECT ATN2(35.175643, 129.44)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": 1.3054517947300646}]  
```  
  
####  <a name="bk_ceiling"></a>MEZNÍ HODNOTY  
 Vrátí hello nejmenší celé číslo větší než nebo rovno, hello zadaný číselný výraz.  
  
 **Syntaxe**  
  
```  
CEILING (<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad ukazuje číselné kladné a záporné, a nulové hodnoty s hello funkce mezní hodnoty.  
  
```  
SELECT CEILING(123.45), CEILING(-123.45), CEILING(0.0)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: 124, $2: -123, $3: 0}]  
```  
  
####  <a name="bk_cos"></a>COS  
 Vrátí hello trigonometrické kosinus hello úhlu, uvedeného v radiánech, v hello zadaný výraz.  
  
 **Syntaxe**  
  
```  
COS(<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Následující ukázka Hello vypočítá hello COS z hello zadaný úhlu.  
  
```  
SELECT COS(14.78)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": -0.59946542619465426}]  
```  
  
####  <a name="bk_cot"></a>COP  
 Vrátí hello trigonometrické kotangens hello úhlu, uvedeného v radiánech, v hello zadán číselný výraz.  
  
 **Syntaxe**  
  
```  
COT(<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Následující ukázka Hello vypočítá hello COP hello určeného úhlu.  
  
```  
SELECT COT(124.1332)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": -0.040311998371148884}]  
```  
  
####  <a name="bk_degrees"></a>STUPŇŮ  
 Vrátí hello odpovídající úhel ve stupních pro úhlu uvedeného v radiánech.  
  
 **Syntaxe**  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad vrátí hello počet stupňů v úhel radiánech PÍ/2.  
  
```  
SELECT DEGREES(PI()/2)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": 90}]  
```  
  
####  <a name="bk_floor"></a>FLOOR  
 Vrátí hello největší celé číslo menší než nebo rovna toohello zadán číselný výraz.  
  
 **Syntaxe**  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad ukazuje číselné kladné a záporné, a nulové hodnoty s hello FLOOR – funkce.  
  
```  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR(0.0)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: 123, $2: -124, $3: 0}]  
```  
  
####  <a name="bk_exp"></a>EXP  
 Vrátí hodnotu exponenciálního hello Dobrý den zadán číselný výraz.  
  
 **Syntaxe**  
  
```  
EXP (<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Poznámky**  
  
 Konstanta Hello **e** (2.718281...), je hello základní přirozeného logaritmu.  
  
 Hello exponent čísla je hello konstanta **e** vyvolá toohello power hello čísla. Například EXP(1.0) = e ^ 1.0 = 2.71828182845905 a EXP(10) = e ^ 10 = 22026.4657948067.  
  
 Hello exponent hello přirozený logaritmus čísla je číslo hello, samotné: EXP (protokol (ne)) = n. A hello přirozený logaritmus čísla exponenciální hello je hello číslo samotné: protokolu (EXP (ne)) = n.  
  
 **Příklady**  
  
 Hello následující příklad deklaruje proměnné a vrátí hodnotu exponenciálního hello hello zadanou proměnnou (10).  
  
```  
SELECT EXP(10)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: 22026.465794806718}]  
```  
  
 Hello následující příklad vrátí hodnotu exponenciálního hello hello přirozený logaritmus 20 a hello přirozený logaritmus hello exponenciální 20. Protože tyto funkce jsou inverzní funkce vzájemně, hello návratovou hodnotu s zaokrouhlení hodnoty pro číslo s plovoucí desetinnou matematické v obou případech je 20.  
  
```  
SELECT EXP(LOG(20)), LOG(EXP(20))  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: 19.999999999999996, $2: 20}]  
```  
  
####  <a name="bk_log"></a>PROTOKOLU  
 Vrátí hello přirozený logaritmus hello zadán číselný výraz.  
  
 **Syntaxe**  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
-   `base`  
  
     Nepovinný argument číselné, který nastaví hello základ pro hello logaritmus  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Poznámky**  
  
 Ve výchozím nastavení LOG() vrátí přirozený logaritmus hello. Základní hello hello logaritmus tooanother hodnoty můžete změnit pomocí volitelný parametr základní hello.  
  
 Hello přirozený logaritmus je toohello logaritmus hello základní **e**, kde **e** je přibližně stejné too2.718281828 nenormální konstantní.  
  
 Hello přirozený logaritmus čísla exponenciální hello je číslo hello, samotné: protokolu (EXP (ne)) = n. A hello exponent hello přirozený logaritmus čísla je číslo hello, samotné: EXP (protokol (ne)) = n.  
  
 **Příklady**  
  
 Hello následující příklad deklaruje proměnné a vrátí hodnotu logaritmus hello hello zadanou proměnnou (10).  
  
```  
SELECT LOG(10)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: 2.3025850929940459}]  
```  
  
 Hello následující příklad vypočítá hello protokolu pro hello exponent čísla.  
  
```  
SELECT EXP(LOG(10))  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: 10.000000000000002}]  
```  
  
####  <a name="bk_log10"></a>LOG10  
 Vrátí logaritmus základu 10 hello hello zadán číselný výraz.  
  
 **Syntaxe**  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Poznámky**  
  
 Hello LOG10 a funkce POWER jsou nepřímo související tooone jiné. Například 10 ^ LOG10(n) = n.  
  
 **Příklady**  
  
 Hello následující příklad deklaruje proměnné a vrátí hodnotu LOG10 hello hello zadanou proměnnou (100).  
  
```  
SELECT LOG10(100)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: 2}]  
```  
  
####  <a name="bk_pi"></a>PLATFORMY  
 Vrátí hello konstantní hodnotu čísla PÍ.  
  
 **Syntaxe**  
  
```  
PI ()  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad vrátí hello hodnotu čísla pí.  
  
```  
SELECT PI()  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_power"></a>NAPÁJENÍ  
 Vrátí hello hodnotu hello zadaný výraz toohello zadaný napájení.  
  
 **Syntaxe**  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
-   `y`  
  
     Je hello power toowhich tooraise `numeric_expression`.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad ukazuje, vyvolání číslo power toohello 3 (hello datové krychle hello čísla).  
  
```  
SELECT POWER(2, 3), POWER(2.5, 3)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: 8, $2: 15.625}]  
```  
  
####  <a name="bk_radians"></a>RADIÁNECH  
 Vrátí radiánech při zadání číselného výrazu, ve stupních, se.  
  
 **Syntaxe**  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad vezme jako vstupní údaje několik úhly a vrátí jejich příslušné hodnoty radián.  
  
```  
SELECT RADIANS(-45.01), RADIANS(-181.01), RADIANS(0), RADIANS(0.1472738), RADIANS(197.1099392)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{  
       "$1": -0.7855726963226477,  
       "$2": -3.1592204790349356,  
       "$3": 0,  
       "$4": 0.0025704127119236249,  
       "$5": 3.4402174274458375  
   }]  
```  
  
####  <a name="bk_round"></a>ZAOKROUHLIT  
 Vrátí číselnou hodnotu, zaokrouhlené toohello nejbližší celé číslo.  
  
 **Syntaxe**  
  
```  
ROUND(<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad zaokrouhlí hello následující kladné a záporné čísla toohello nejbližší celé číslo.  
  
```  
SELECT ROUND(2.4), ROUND(2.6), ROUND(2.5), ROUND(-2.4), ROUND(-2.6)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: 2, $2: 3, $3: 3, $4: -2, $5: -3}]  
```  
  
####  <a name="bk_sign"></a>PŘIHLÁŠENÍ  
 Vrátí hello kladnou (+ 1), nula (0), nebo záporné znaménko (-1) hello zadán číselný výraz.  
  
 **Syntaxe**  
  
```  
SIGN(<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad vrátí hello PŘIHLAŠOVACÍ hodnot čísel z too2-2.  
  
```  
SELECT SIGN(-2), SIGN(-1), SIGN(0), SIGN(1), SIGN(2)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: -1, $2: -1, $3: 0, $4: 1, $5: 1}]  
```  
  
####  <a name="bk_sin"></a>SIN  
 Vrátí hello trigonometrické sinus hello úhlu, uvedeného v radiánech, v hello zadaný výraz.  
  
 **Syntaxe**  
  
```  
SIN(<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Následující ukázka Hello vypočítá hello SIN hello určeného úhlu.  
  
```  
SELECT SIN(45.175643)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": 0.929607286611012}]  
```  
  
####  <a name="bk_sqrt"></a>SQRT  
 Vrátí hello druhou odmocninu čísla hello zadat číselnou hodnotu.  
  
 **Syntaxe**  
  
```  
SQRT(<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad vrátí hello druhé odmocniny čísel 1 – 3.  
  
```  
SELECT SQRT(1), SQRT(2.0), SQRT(3)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: 1, $2: 1.4142135623730952, $3: 1.7320508075688772}]  
```  
  
####  <a name="bk_square"></a>HRANATÉ  
 Vrátí hello odmocnina z hello zadat číselnou hodnotu.  
  
 **Syntaxe**  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad vrátí hello čtverce čísel 1 – 3.  
  
```  
SELECT SQUARE(1), SQUARE(2.0), SQUARE(3)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: 1, $2: 4, $3: 9}]  
```  
  
####  <a name="bk_tan"></a>TAN  
 Vrátí tangens hello hello úhlu, uvedeného v radiánech, v hello zadaný výraz.  
  
 **Syntaxe**  
  
```  
TAN (<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad vypočítá hello tangens () PÍ / 2.  
  
```  
SELECT TAN(PI()/2);  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": 16331239353195370 }]  
```  
  
####  <a name="bk_trunc"></a>TRUNC  
 Vrátí číselnou hodnotu, zkrácený toohello nejbližší celé číslo.  
  
 **Syntaxe**  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 **Argumenty**  
  
-   `numeric_expression`  
  
     Je číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Následující ukázka Hello zkrátí hello následující kladné a záporné čísla toohello nejbližší celé číslo.  
  
```  
SELECT TRUNC(2.4), TRUNC(2.6), TRUNC(2.5), TRUNC(-2.4), TRUNC(-2.6)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: 2, $2: 2, $3: 2, $4: -2, $5: -2}]  
```  
  
###  <a name="bk_type_checking_functions"></a>Typ kontroly funkce  
 Hello následující funkce podporují typ kontroly proti vstupní hodnoty, a každou vrátit logickou hodnotu.  
  
||||  
|-|-|-|  
|[IS_ARRAY](#bk_is_array)|[IS_BOOL](#bk_is_bool)|[IS_DEFINED](#bk_is_defined)|  
|[IS_NULL](#bk_is_null)|[IS_NUMBER](#bk_is_number)|[IS_OBJECT](#bk_is_object)|  
|[IS_PRIMITIVE](#bk_is_primitive)|[IS_STRING](#bk_is_string)||  
  
####  <a name="bk_is_array"></a>IS_ARRAY  
 Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je pole.  
  
 **Syntaxe**  
  
```  
IS_ARRAY(<expression>)  
```  
  
 **Argumenty**  
  
-   `expression`  
  
     Je jakýkoli platný výraz.  
  
 **Návratové typy**  
  
 Vrátí logický výraz.  
  
 **Příklady**  
  
 Hello následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí hello IS_ARRAY funkce.  
  
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
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: false, $6: true}]  
```  
  
####  <a name="bk_is_bool"></a>IS_BOOL  
 Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je logická hodnota.  
  
 **Syntaxe**  
  
```  
IS_BOOL(<expression>)  
```  
  
 **Argumenty**  
  
-   `expression`  
  
     Je jakýkoli platný výraz.  
  
 **Návratové typy**  
  
 Vrátí logický výraz.  
  
 **Příklady**  
  
 Hello následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí hello IS_BOOL funkce.  
  
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
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: true, $2: false, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_defined"></a>IS_DEFINED  
 Vrátí logickou hodnotu udávající, pokud byla vlastnost hello přiřazena hodnota.  
  
 **Syntaxe**  
  
```  
IS_DEFINED(<expression>)  
```  
  
 **Argumenty**  
  
-   `expression`  
  
     Je jakýkoli platný výraz.  
  
 **Návratové typy**  
  
 Vrátí logický výraz.  
  
 **Příklady**  
  
 Následující příklad zkontroluje přítomnost hello vlastnosti v rámci hello Hello zadaný dokument JSON. Hello nejprve vrací hodnotu true, protože "a" je k dispozici, ale hello druhý vrací hodnotu false vzhledem k tomu, že chybí "b".  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a), IS_DEFINED({ "a" : 5 }.b)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{  
       "$1": true,    
       "$2": false   
   }]  
```  
  
####  <a name="bk_is_null"></a>IS_NULL  
 Vrátí logickou hodnotu udávající, pokud zadaný typ hello hello výraz má hodnotu null.  
  
 **Syntaxe**  
  
```  
IS_NULL(<expression>)  
```  
  
 **Argumenty**  
  
-   `expression`  
  
     Je jakýkoli platný výraz.  
  
 **Návratové typy**  
  
 Vrátí logický výraz.  
  
 **Příklady**  
  
 Hello následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí hello IS_NULL funkce.  
  
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
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: false, $2: false, $3: false, $4: true, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_number"></a>IS_NUMBER  
 Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je číslo.  
  
 **Syntaxe**  
  
```  
IS_NUMBER(<expression>)  
```  
  
 **Argumenty**  
  
-   `expression`  
  
     Je jakýkoli platný výraz.  
  
 **Návratové typy**  
  
 Vrátí logický výraz.  
  
 **Příklady**  
  
 Hello následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí hello IS_NULL funkce.  
  
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
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: false, $2: true, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_object"></a>IS_OBJECT  
 Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je objekt JSON.  
  
 **Syntaxe**  
  
```  
IS_OBJECT(<expression>)  
```  
  
 **Argumenty**  
  
-   `expression`  
  
     Je jakýkoli platný výraz.  
  
 **Návratové typy**  
  
 Vrátí logický výraz.  
  
 **Příklady**  
  
 Hello následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí hello IS_OBJECT funkce.  
  
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
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: true, $6: false}]  
```  
  
####  <a name="bk_is_primitive"></a>IS_PRIMITIVE  
 Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je primitivní (řetězec, logická hodnota, číselná nebo má hodnotu null).  
  
 **Syntaxe**  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 **Argumenty**  
  
-   `expression`  
  
     Je jakýkoli platný výraz.  
  
 **Návratové typy**  
  
 Vrátí logický výraz.  
  
 **Příklady**  
  
 Hello následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí hello IS_PRIMITIVE funkce.  
  
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
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": true, "$2": true, "$3": true, "$4": true, "$5": false, "$6": false, "$7": false}]  
```  
  
####  <a name="bk_is_string"></a>IS_STRING  
 Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz obsahuje řetězec.  
  
 **Syntaxe**  
  
```  
IS_STRING(<expression>)  
```  
  
 **Argumenty**  
  
-   `expression`  
  
     Je jakýkoli platný výraz.  
  
 **Návratové typy**  
  
 Vrátí logický výraz.  
  
 **Příklady**  
  
 Hello následující příklad ověří objekty JSON Boolean, číslo, řetězec hodnotu null, objekt, pole a nedefinované typy pomocí hello IS_STRING funkce.  
  
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
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{$1: false, $2: false, $3: true, $4: false, $5: false, $6: false}]  
```  
  
###  <a name="bk_string_functions"></a>Řetězcové funkce  
 Hello následující skalární funkce provést operaci s vstupní hodnotu řetězce a vrátí řetězec, číselnou nebo logická hodnota.  
  
||||  
|-|-|-|  
|[CONCAT](#bk_concat)|[OBSAHUJE](#bk_contains)|[ENDSWITH](#bk_endswith)|  
|[INDEX_OF](#bk_index_of)|[VLEVO](#bk_left)|[DÉLKA](#bk_length)|  
|[NIŽŠÍ](#bk_lower)|[LTRIM](#bk_ltrim)|[NAHRADIT](#bk_replace)|  
|[REPLIKACE](#bk_replicate)|[REVERSE](#bk_reverse)|[VPRAVO](#bk_right)|  
|[RTRIM](#bk_rtrim)|[STARTSWITH](#bk_startswith)|[DÍLČÍ ŘETĚZEC](#bk_substring)|  
|[HORNÍ](#bk_upper)|||  
  
####  <a name="bk_concat"></a>CONCAT  
 Vrátí řetězec, který je výsledkem hello zřetězení dvou nebo více řetězcové hodnoty.  
  
 **Syntaxe**  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
 **Návratové typy**  
  
 Vrací výraz řetězce.  
  
 **Příklady**  
  
 Následující příklad vrátí řetězec hello zřetězených hello Hello zadané hodnoty.  
  
```  
SELECT CONCAT("abc", "def")  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": "abcdef"}  
```  
  
####  <a name="bk_contains"></a>OBSAHUJE  
 Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu obsahuje hello druhý.  
  
 **Syntaxe**  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
 **Návratové typy**  
  
 Vrátí logický výraz.  
  
 **Příklady**  
  
 Hello následující příklad zkontroluje Pokud "abc" "ab" a "d" obsahuje.  
  
```  
SELECT CONTAINS("abc", "ab"), CONTAINS("abc", "d")  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <a name="bk_endswith"></a>ENDSWITH  
 Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu končí hello druhý.  
  
 **Syntaxe**  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
 **Návratové typy**  
  
 Vrátí logický výraz.  
  
 **Příklady**  
  
 Hello následující příklad vrátí hello "abc" končí textem "b" a "bc".  
  
```  
SELECT ENDSWITH("abc", "b"), ENDSWITH("abc", "bc")  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_index_of"></a>INDEX_OF  
 Vrátí počáteční pozici prvního výskytu hello hello druhý řetězcového výrazu v rámci zadaného řetězcového výrazu první hello nebo -1, pokud není nalezen řetězec hello hello.  
  
 **Syntaxe**  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad vrátí index hello různé podřetězců uvnitř "abc".  
  
```  
SELECT INDEX_OF("abc", "ab"), INDEX_OF("abc", "b"), INDEX_OF("abc", "c")  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": 0, "$2": 1, "$3": -1}]  
```  
  
####  <a name="bk_left"></a>VLEVO  
 Vrátí hello levé části řetězce s hello zadaný počet znaků.  
  
 **Syntaxe**  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
-   `num_expr`  
  
     Je libovolný platný číselný výraz.  
  
 **Návratové typy**  
  
 Vrací výraz řetězce.  
  
 **Příklady**  
  
 Hello následující příklad vrátí hello zbývající část "abc" pro různé hodnoty pro délku.  
  
```  
SELECT LEFT("abc", 1), LEFT("abc", 2)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": "ab", "$2": "ab"}]  
```  
  
####  <a name="bk_length"></a>DÉLKA  
 Vrátí hello počet znaků, který hello zadán řetězcovým výrazem.  
  
 **Syntaxe**  
  
```  
LENGTH(<str_expr>)  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
 **Návratové typy**  
  
 Vrací výraz řetězce.  
  
 **Příklady**  
  
 Hello následující příklad vrátí hello délku řetězce.  
  
```  
SELECT LENGTH("abc")  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_lower"></a>NIŽŠÍ  
 Vrací výraz řetězce po převodu dat toolowercase velké písmeno.  
  
 **Syntaxe**  
  
```  
LOWER(<str_expr>)  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
 **Návratové typy**  
  
 Vrací výraz řetězce.  
  
 **Příklady**  
  
 Následující příklad ukazuje, jak Hello toouse nižší v dotazu.  
  
```  
SELECT LOWER("Abc")  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": "abc"}]  
  
```  
  
####  <a name="bk_ltrim"></a>LTRIM  
 Vrací výraz řetězce po ho odebere úvodní mezery.  
  
 **Syntaxe**  
  
```  
LTRIM(<str_expr>)  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
 **Návratové typy**  
  
 Vrací výraz řetězce.  
  
 **Příklady**  
  
 Následující příklad ukazuje, jak Hello toouse LTRIM uvnitř dotazu.  
  
```  
SELECT LTRIM("  abc"), LTRIM("abc"), LTRIM("abc   ")  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc   "}]  
```  
  
####  <a name="bk_replace"></a>NAHRADIT  
 Nahradí všechny výskyty zadaná řetězcová hodnota s jinou hodnotou řetězce.  
  
 **Syntaxe**  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
 **Návratové typy**  
  
 Vrací výraz řetězce.  
  
 **Příklady**  
  
 Hello následující příklad ukazuje, jak nahradit toouse v dotazu.  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk")  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": "This is a desk"}]  
```  
  
####  <a name="bk_replicate"></a>REPLIKACE  
 Opakuje hodnotu řetězce zadaného počtu opakování.  
  
 **Syntaxe**  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
-   `num_expr`  
  
     Je libovolný platný číselný výraz.  
  
 **Návratové typy**  
  
 Vrací výraz řetězce.  
  
 **Příklady**  
  
 Hello následující příklad ukazuje, jak REPLIKOVAT toouse v dotazu.  
  
```  
SELECT REPLICATE("a", 3)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": "aaa"}]  
```  
  
####  <a name="bk_reverse"></a>REVERSE  
 Vrátí hello zpětné pořadí řetězcovou hodnotu.  
  
 **Syntaxe**  
  
```  
REVERSE(<str_expr>)  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
 **Návratové typy**  
  
 Vrací výraz řetězce.  
  
 **Příklady**  
  
 Hello následující příklad ukazuje, jak toouse REVERSE v dotazu.  
  
```  
SELECT REVERSE("Abc")  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": "cbA"}]  
```  
  
####  <a name="bk_right"></a>VPRAVO  
 Vrátí hello právo součástí řetězec s hello zadaný počet znaků.  
  
 **Syntaxe**  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
-   `num_expr`  
  
     Je libovolný platný číselný výraz.  
  
 **Návratové typy**  
  
 Vrací výraz řetězce.  
  
 **Příklady**  
  
 Hello následující příklad vrátí hello pravou část "abc" pro různé hodnoty pro délku.  
  
```  
SELECT RIGHT("abc", 1), RIGHT("abc", 2)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": "c", "$2": "bc"}]  
```  
  
####  <a name="bk_rtrim"></a>RTRIM  
 Vrací výraz řetězce po odebere koncové mezery.  
  
 **Syntaxe**  
  
```  
RTRIM(<str_expr>)  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
 **Návratové typy**  
  
 Vrací výraz řetězce.  
  
 **Příklady**  
  
 Následující příklad ukazuje, jak Hello toouse RTRIM uvnitř dotazu.  
  
```  
SELECT RTRIM("  abc"), RTRIM("abc"), RTRIM("abc   ")  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": "   abc", "$2": "abc", "$3": "abc"}]  
```  
  
####  <a name="bk_startswith"></a>STARTSWITH  
 Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu začíná hello druhý.  
  
 **Syntaxe**  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
 **Návratové typy**  
  
 Vrátí logický výraz.  
  
 **Příklady**  
  
 Hello následující příklad ověří, zda je hello řetězec "abc" začíná "b" a "a".  
  
```  
SELECT STARTSWITH("abc", "b"), STARTSWITH("abc", "a")  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_substring"></a>DÍLČÍ ŘETĚZEC  
 Vrátí část výrazu řetězce začínající hello zadaný znak pozice s nulovým základem a pokračuje v toohello Zadaná délka nebo toohello konce řetězce hello.  
  
 **Syntaxe**  
  
```  
SUBSTRING(<str_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
-   `num_expr`  
  
     Je libovolný platný číselný výraz.  
  
 **Návratové typy**  
  
 Vrací výraz řetězce.  
  
 **Příklady**  
  
 Hello následující příklad vrátí hello podřetězcem "abc" spouštění v 1 a o délce 1 znak.  
  
```  
SELECT SUBSTRING("abc", 1, 1)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": "b"}]  
```  
  
####  <a name="bk_upper"></a>HORNÍ  
 Vrací výraz řetězce po převodu dat toouppercase malé písmeno.  
  
 **Syntaxe**  
  
```  
UPPER(<str_expr>)  
```  
  
 **Argumenty**  
  
-   `str_expr`  
  
     Je jakýkoli platný řetězec výraz.  
  
 **Návratové typy**  
  
 Vrací výraz řetězce.  
  
 **Příklady**  
  
 Následující příklad ukazuje, jak Hello toouse horní v dotazu  
  
```  
SELECT UPPER("Abc")  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": "ABC"}]  
```  
  
###  <a name="bk_array_functions"></a>Funkce pole  
 provedení operace hodnota vstupní pole a vrátí číselnou, logická hodnota nebo pole hodnota Hello následující skalární funkce  
  
||||  
|-|-|-|  
|[ARRAY_CONCAT](#bk_array_concat)|[ARRAY_CONTAINS](#bk_array_contains)|[ARRAY_LENGTH](#bk_array_length)|  
|[ARRAY_SLICE](#bk_array_slice)|||  
  
####  <a name="bk_array_concat"></a>ARRAY_CONCAT  
 Vrátí pole, které je výsledkem hello zřetězení dvě nebo více hodnot pole.  
  
 **Syntaxe**  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 **Argumenty**  
  
-   `arr_expr`  
  
     Je jakýkoli platný pole výraz.  
  
 **Návratové typy**  
  
 Vrací výraz pole.  
  
 **Příklady**  
  
 Hello následující příklad, jak maticových tooconcatenate dva.  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"])  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <a name="bk_array_contains"></a>ARRAY_CONTAINS  
 Vrátí logickou hodnotu udávající, zda text hello pole obsahuje hello zadané hodnotě.  
  
 **Syntaxe**  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr>)  
```  
  
 **Argumenty**  
  
-   `arr_expr`  
  
     Je jakýkoli platný pole výraz.  
  
-   `expr`  
  
     Je jakýkoli platný výraz.  
  
 **Návratové typy**  
  
 Vrátí logickou hodnotu.  
  
 **Příklady**  
  
 Následující příklad, jak Hello toocheck pro členství v poli pomocí ARRAY_CONTAINS.  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples"),  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes")  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <a name="bk_array_length"></a>ARRAY_LENGTH  
 Vrátí číslo hello elementů hello zadaný výraz pole.  
  
 **Syntaxe**  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 **Argumenty**  
  
-   `arr_expr`  
  
     Je jakýkoli platný pole výraz.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz.  
  
 **Příklady**  
  
 Hello následující příklad, jak tooget hello délka pole pomocí ARRAY_LENGTH.  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"])  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_array_slice"></a>ARRAY_SLICE  
 Vrátí část výraz pole.
  
 **Syntaxe**  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argumenty**  
  
-   `arr_expr`  
  
     Je jakýkoli platný pole výraz.  
  
-   `num_expr`  
  
     Je libovolný platný číselný výraz.  
  
 **Návratové typy**  
  
 Vrátí logickou hodnotu.  
  
 **Příklady**  
  
 Následující příklad, jak Hello tooget součástí pomocí ARRAY_SLICE pole.  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1),  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1)  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{  
           "$1": ["strawberries", "bananas"],   
           "$2": ["strawberries"]  
       }]  
```  
  
###  <a name="bk_spatial_functions"></a>Prostorové funkce  
 Hello následující skalární funkce provádění operací na prostorový objekt vstupní hodnotu a vrátí číslo nebo logická hodnota.  
  
||||  
|-|-|-|  
|[ST_DISTANCE](#bk_st_distance)|[ST_WITHIN](#bk_st_within)|[ST_INTERSECTS](#bk_st_intersects)|[ST_ISVALID](#bk_st_isvalid)|  
|[ST_ISVALIDDETAILED](#bk_st_isvaliddetailed)|||  
  
####  <a name="bk_st_distance"></a>ST_DISTANCE  
 Vrátí hello vzdálenost mezi hello dva GeoJSON bodu, mnohoúhelníku nebo LineString výrazů.  
  
 **Syntaxe**  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumenty**  
  
-   `spatial_expr`  
  
     Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.  
  
 **Návratové typy**  
  
 Vrátí číselný výraz obsahující hello vzdálenost. Vyjadřuje se v měřidla pro hello výchozí odkaz na systém.  
  
 **Příklady**  
  
 Následující příklad ukazuje, jak Hello tooreturn všechny rodiny dokumenty, které jsou v rámci 30 km hello zadané umístění pomocí hello ST_DISTANCE integrovaná funkce. .  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <a name="bk_st_within"></a>ST_WITHIN  
 Vrátí výrazu logické hodnoty, která určuje, zda hello objekt GeoJSON (bod, mnohoúhelníku nebo LineString) zadaný v prvním argumentu hello je v rámci hello GeoJSON (bod, mnohoúhelníku nebo LineString) v druhý argument hello.  
  
 **Syntaxe**  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumenty**  
  
-   `spatial_expr`  
  
     Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.  
 
-   `spatial_expr`  
  
     Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.  
  
 **Návratové typy**  
  
 Vrátí logickou hodnotu.  
  
 **Příklady**  
  
 Hello následující příklad ukazuje, jak toofind rodiny všechny dokumenty v rámci pomocí ST_WITHIN mnohoúhelníku.  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <a name="bk_st_intersects"></a>ST_INTERSECTS  
 Vrátí hodnotu označující, zda text hello objekt GeoJSON (bod, mnohoúhelníku nebo LineString) zadaný v prvním argumentu hello protíná hello GeoJSON (bod, mnohoúhelníku nebo LineString) v hello druhý argument logický výraz.  
  
 **Syntaxe**  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumenty**  
  
-   `spatial_expr`  
  
     Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.  
 
-   `spatial_expr`  
  
     Je jakýkoli platný objekt výraz GeoJSON bodu, mnohoúhelníku nebo LineString.  
  
 **Návratové typy**  
  
 Vrátí logickou hodnotu.  
  
 **Příklady**  
  
 Následující příklad ukazuje, jak Hello toofind hello všechny oblasti, které protíná s danou mnohoúhelníku.  
  
```  
SELECT a.id   
FROM Areas a   
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <a name="bk_st_isvalid"></a>ST_ISVALID  
 Vrátí logickou hodnotu udávající, zda zadaný hello GeoJSON bodu, mnohoúhelníku nebo LineString výraz není platný.  
  
 **Syntaxe**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argumenty**  
  
-   `spatial_expr`  
  
     Je platný výraz GeoJSON bodu, mnohoúhelníku nebo LineString.  
  
 **Návratové typy**  
  
 Vrátí logický výraz.  
  
 **Příklady**  
  
 Následující příklad ukazuje, jak Hello toocheck, pokud je bod platný pomocí ST_VALID.  
  
 Například tohoto bodu má hodnotu zeměpisnou šířku, která není v platném rozsahu hodnot [-90, 90] hello, takže hello dotaz vrátí hodnotu false.  
  
 Pro mnohoúhelníky hello GeoJSON specifikace vyžaduje, aby hello poslední souřadnic pár poskytuje hello stejné jako první, toocreate hello uzavřený obrazec. Je třeba zadat body v rámci mnohoúhelníku v pořadí proti směru hodinových ručiček. Mnohoúhelníku zadaný v po směru hodinových ručiček pořadí představuje hello inverzní oblasti hello v něm.  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{ "$1": false }]  
```  
  
####  <a name="bk_st_isvaliddetailed"></a>ST_ISVALIDDETAILED  
 Vrátí hodnotu JSON obsahující logickou hodnotu, pokud hello Zadaný bod GeoJSON, mnohoúhelníku nebo LineString výraz je platná a pokud je neplatná, kromě hello důvod jako hodnotu řetězce.  
  
 **Syntaxe**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argumenty**  
  
-   `spatial_expr`  
  
     Je jakýkoli platný výraz bodu nebo mnohoúhelníku GeoJSON.  
  
 **Návratové typy**  
  
 Vrátí hodnotu JSON obsahující logickou hodnotu, pokud hello určena GeoJSON bodu nebo mnohoúhelníku výrazu je platný a pokud je neplatná, kromě hello důvod jako hodnotu řetězce.  
  
 **Příklady**  
  
 Následující příklad, jak Hello toocheck platnosti (s podrobnostmi) pomocí ST_ISVALIDDETAILED.  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
})  
```  
  
 Zde je sada výsledků dotazu hello.  
  
```  
[{  
  "$1": {   
    "valid": false,   
    "reason": "hello Polygon input is not valid because hello start and end points of hello ring number 1 are not hello same. Each ring of a polygon must have hello same start and end points."   
  }  
}]  
```  
  
## <a name="next-steps"></a>Další kroky  
 [Syntaxe jazyka SQL a dotaz SQL pro Azure Cosmos DB](documentdb-sql-query.md)   
 [Dokumentace k Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/)  
  
  
