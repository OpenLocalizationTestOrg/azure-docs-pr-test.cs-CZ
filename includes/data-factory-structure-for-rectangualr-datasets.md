## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Zadání definice struktury obdélníková datové sady
Hello struktura část v datových sadách hello JSON se **volitelné** část obdélníková tabulky (s řádky a sloupce) a obsahuje kolekci sloupců pro tabulku hello. Struktura části hello budete používat pro buď typ informacemi pro převody typů nebo provádění mapování sloupce. Hello následující části popisují tyto funkce podrobně. 

Každý sloupec obsahuje hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| jméno |Název sloupce hello. |Ano |
| type |Datový typ sloupce hello. Najdete v části převody typu níže pro další podrobnosti týkající se kdy by měl můžete určit informace o typu |Ne |
| Jazyková verze |.NET na základě používají, pokud je zadaný typ a je typ formátu .NET Datetime nebo Datetimeoffset toobe jazykovou verzi. Výchozí hodnota je "en-us". |Ne |
| Formát |Formátu řetězce toobe používají, pokud je zadaný typ a je typ formátu .NET Datetime nebo Datetimeoffset. |Ne |

Hello následující příklad ukazuje hello struktura část JSON pro tabulku, která má tři sloupce ID uživatele, název a lastlogindate.

```json
"structure": 
[
    { "name": "userid"},
    { "name": "name"},
    { "name": "lastlogindate"}
],
```

Použijte následující pokyny pro případ hello tooinclude "struktura" informace a jaké tooinclude v hello **struktura** části.

* **Pro strukturovaná data zdroje** , ukládání dat schéma a typ informací společně s samotná (zdrojů jako tabulky Azure SQL Server, Oracle, atd.), musíte zadat část "struktury" hello pouze v případě, že chcete mapování sloupců proveďte specifické data hello zdrojové sloupce, které jsou toospecific sloupců v jímka a jejich názvy hello není stejná (viz podrobnosti v následující části mapování sloupců). 
  
    Jak je uvedeno nahoře, je v části "struktura" volitelné informace o typu hello. Strukturované zdroje informací o typu je již k dispozici jako součást definice datové sady v úložišti dat hello, takže můžete nesmí obsahovat informace o typu obsahují část "struktura" hello.
* **Pro schéma pro zdroje dat pro čtení (konkrétně objektů blob v Azure)** můžete toostore dat bez ukládání žádné schéma nebo typ informace s daty hello. Pro tyto typy zdrojů dat by měla obsahovat "struktura" hello následující 2 případech:
  * Chcete toodo mapování sloupců.
  * Pokud hello datová sada zdroji v aktivitě kopírování, můžete zadat informace o typu "struktury" a objektu pro vytváření dat bude používat tento typ informace pro převod toonative typy pro hello sink. V tématu [přesunout tooand dat z Azure Blob](../articles/data-factory/data-factory-azure-blob-connector.md) Další informace najdete v článku.

### <a name="supported-net-based-types"></a>Podporováno. Na základě NET typy
Data factory podporuje hello následující .NET kompatibilní se specifikací CLS na základě hodnoty typu pro poskytování informací o typu "struktury" pro schéma na zdrojů dat čtení objektů blob v Azure.

* Int16
* Int32 
* Int64
* Jeden
* Double
* Decimal
* Byte]
* BOOL
* Řetězec 
* Identifikátor GUID
* Data a času
* Datový typ DateTimeOffset
* Časový interval 

Pro hodnotu Datetime a Datetimeoffset můžete volitelně specifikovat "culture" a "format" řetězec toofacilitate analýzy vaše vlastní řetězce data a času. Viz ukázka pro převod typů níže.

