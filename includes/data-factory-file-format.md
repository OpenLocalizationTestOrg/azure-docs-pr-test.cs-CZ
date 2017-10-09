## <a name="specifying-formats"></a>Zadávání formátů
Azure Data Factory podporuje následující typy formátu hello:

* [Formát Text](#specifying-textformat)
* [Formát JSON](#specifying-jsonformat)
* [Formát Avro](#specifying-avroformat)
* [Formát ORC](#specifying-orcformat)
* [Formát Parquet](#specifying-parquetformat)

### <a name="specifying-textformat"></a>Zadávání formátu TextFormat
Chcete-li tooparse hello textové soubory nebo zapisovat hello data v textovém formátu, nastavte hello `format` `type` vlastnost příliš**TextFormat**. Můžete také zadat následující hello **volitelné** vlastnosti v hello `format` části. V tématu [TextFormat příklad](#textformat-example) část o tooconfigure.

| Vlastnost | Popis | Povolené hodnoty | Požaduje se |
| --- | --- | --- | --- |
| columnDelimiter |v souboru použit znak Hello tooseparate sloupce. Můžete zvážit toouse výjimečných netisknutelné char, která již není pravděpodobně existuje ve vašich datech: například zadejte "\u0001" představující spustit z záhlaví (SOH). |Je povolený jenom jeden znak. Hello **výchozí** hodnota je **čárkou (,)**. <br/><br/>toouse znak Unicode odkazovat příliš[znaky znakové sady Unicode](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget hello odpovídající kód pro ni. |Ne |
| rowDelimiter |znak Hello používá tooseparate řádků v souboru. |Je povolený jenom jeden znak. Hello **výchozí** hodnota je všechny následující hodnoty na čtení hello: **["\r\n", "\r", "\n"]** a **"\r\n"** v zápisu. |Ne |
| escapeChar |speciální znak Hello používá tooescape oddělovač sloupec v hello obsah vstupní soubor. <br/><br/>Pro tabulku nejde zadat escapeChar a quoteChar současně. |Je povolený jenom jeden znak. Žádná výchozí hodnota. <br/><br/>Příklad: Pokud máte čárkou (', ') jako oddělovač sloupec hello ale chcete toohave hello čárku v textu hello (Příklad: "Hello, world"), můžete definovat jako hello řídicí znak "$" a použijte řetězec "Hello$, world" v hello zdroji. |Ne |
| quoteChar |tooquote řetězcovou hodnotu použít znak Hello. oddělovače sloupců a řádků Hello uvnitř znaky uvozovek za sebou hello by považovány za součást hello řetězcovou hodnotu. Tato vlastnost je použít tooboth vstupní a výstupní datové sady.<br/><br/>Pro tabulku nejde zadat escapeChar a quoteChar současně. |Je povolený jenom jeden znak. Žádná výchozí hodnota. <br/><br/>Například, pokud máte čárkou (', ') jako oddělovač sloupec hello ale chcete toohave čárku v textu hello (Příklad: < Hello, world >), můžete definovat "(dvojitých uvozovek) jako hello znak uvozovky a použít hello řetězec"Hello, world"v hello zdroji. |Ne |
| nullValue |Jeden nebo více znaků použít toorepresent hodnotu null. |Jeden nebo několik znaků. Hello **výchozí** hodnoty jsou **"\N" a "NULL"** na čtení a **"\N"** v zápisu. |Ne |
| encodingName |Zadejte název kódování hello. |Platný název kódování. Další informace najdete v tématu [Vlastnost Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx). Příklad: windows-1250 nebo shift_jis. Hello **výchozí** hodnota je **UTF-8**. |Ne |
| firstRowAsHeader |Určuje, zda tooconsider hello první řádek jako záhlaví. U vstupní datové sady Data Factory načítá první řádek jako záhlaví. U výstupní datové sady Data Factory zapisuje první řádek jako záhlaví. <br/><br/>Vzorové scénáře najdete v tématu [Scénáře použití `firstRowAsHeader` a `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount). |True<br/>**False (výchozí)** |Ne |
| skipLineCount |Označuje hello počet řádků tooskip při čtení dat ze vstupní soubory. Pokud jsou zadané skipLineCount i firstRowAsHeader, hello řádky jsou přeskočeny první a pak je informace hlavičky hello číst ze vstupního souboru hello. <br/><br/>Vzorové scénáře najdete v tématu [Scénáře použití `firstRowAsHeader` a `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount). |Integer |Ne |
| treatEmptyAsNull |Určuje, zda hodnota tootreat hodnotu null nebo prázdný řetězec jako hodnotu null při čtení dat ze vstupního souboru. |**True (výchozí)**<br/>False |Ne |

#### <a name="textformat-example"></a>Příklad typu TextFormat
Hello následující příklad ukazuje některé vlastnosti hello formát pro TextFormat.

```json
"typeProperties":
{
    "folderPath": "mycontainer/myfolder",
    "fileName": "myblobname",
    "format":
    {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";",
        "quoteChar": "\"",
        "NullValue": "NaN",
        "firstRowAsHeader": true,
        "skipLineCount": 0,
        "treatEmptyAsNull": true
    }
},
```

toouse `escapeChar` místo `quoteChar`, nahraďte hello řádku s `quoteChar` s hello escapeChar následující:

```json
"escapeChar": "$",
```

#### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Scénáře použití firstRowAsHeader a skipLineCount
* Kopírování z textového souboru tooa zdroj nepocházející ze souborů a chcete tooadd řádek záhlaví, který obsahuje metadata schématu hello (například: schématu SQL). Zadejte `firstRowAsHeader` jako true v hello výstupní datovou sadu pro tento scénář.
* Kopírování z textového souboru obsahujícího jímka nepocházející ze souborů tooa řádek záhlaví a chcete toodrop, který řádek. Zadejte `firstRowAsHeader` jako true v hello vstupní datové sady.
* Kopírování z textového souboru a mají tooskip pár řádků od začátku hello, které neobsahují žádné informace dat nebo záhlaví. Zadejte `skipLineCount` tooindicate hello počet řádků toobe přeskočena. Pokud hello zbytek hello soubor obsahuje řádek záhlaví, můžete také zadat `firstRowAsHeader`. Pokud oba `skipLineCount` a `firstRowAsHeader` jsou zadané, hello řádky jsou přeskočeny první a pak je informace hlavičky hello číst ze vstupního souboru hello

### <a name="specifying-jsonformat"></a>Zadávání formátu JsonFormat
příliš**importu a exportu soubory JSON jako-je do/z Azure Cosmos DB**, najdete v části [dokumentů JSON importu a exportu](../articles/data-factory/data-factory-azure-documentdb-connector.md#importexport-json-documents) část v konektoru Azure Cosmos DB hello s podrobnostmi.

Pokud chcete soubory JSON hello tooparse nebo zapsat hello data ve formátu JSON, nastavte hello `format` `type` vlastnost příliš**JsonFormat**. Můžete také zadat následující hello **volitelné** vlastnosti v hello `format` části. V tématu [JsonFormat příklad](#jsonformat-example) část o tooconfigure.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| filePattern |Uveďte hello vzor dat uložených do každého souboru JSON. Povolené hodnoty jsou **setOfObjects** a **arrayOfObjects**. Hello **výchozí** hodnota je **setOfObjects**. Podrobné informace o těchto vzorech najdete v tématu [Vzory souborů JSON](#json-file-patterns). |Ne |
| jsonNodeReference | Pokud chcete tooiterate a extrahovat data z objektů hello uvnitř pole pole s hello stejný vzor, zadejte cestu JSON hello tohoto pole. Tato vlastnost se podporuje jenom pro kopírování dat ze souborů JSON. | Ne |
| jsonPathDefinition | Zadejte výraz cesty hello JSON pro každé mapování sloupců s názvem přizpůsobené sloupce (začněte s malá). Tato vlastnost se podporuje jenom pro kopírování dat ze souborů JSON. Můžete extrahovat data z objektu nebo pole. <br/><br/> Pro pole v části kořenový objekt začněte kořenové $; pro pole uvnitř hello pole, kterou si zvolí `jsonNodeReference` vlastnost, začátek od hello pole elementu. V tématu [JsonFormat příklad](#jsonformat-example) část o tooconfigure. | Ne |
| encodingName |Zadejte název kódování hello. Hello seznam platných názvů kódování, najdete v tématu: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) vlastnost. Příklad: windows-1250 nebo shift_jis. Hello **výchozí** hodnota je: **UTF-8**. |Ne |
| nestingSeparator |Znak, který je použité tooseparate vnořených úrovní. Hello výchozí hodnota je '. " (tečka). |Ne |

#### <a name="json-file-patterns"></a>Vzory souborů JSON

Aktivita kopírování může analyzovat tyto vzory souborů JSON:

- **Typ I: setOfObjects**

    Každý soubor obsahuje jeden objekt nebo několik objektů, které jsou zřetězené nebo oddělené řádkem. Když je pro výstupní datovou sadu vybraná tato možnost, aktivita kopírování vytvoří jeden soubor JSON, ve kterém je každý objekt na jednom řádku (oddělení řádkem).

    * **Příklad JSON s jedním objektem**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * **Příklad JSON s oddělením řádky**

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * **Příklad JSON se zřetězením**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- **Typ II: arrayOfObjects**

    Každý soubor obsahuje pole objektů.

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

#### <a name="jsonformat-example"></a>Příklad typu JsonFormat

**Případ 1: Kopírování dat ze souborů JSON**

Najdete níže dva typy vzorků, které se při kopírování dat z soubory JSON a hello toonote obecné body:

**Ukázka 1: Extrakce dat z objektu a pole**

V této ukázce očekáváte, že jeden kořenový objekt JSON mapuje toosingle záznam v tabulkovém výsledek. Pokud máte soubor JSON s hello následující obsah:  

```json
{
    "id": "ed0e4960-d9c5-11e6-85dc-d7996816aad3",
    "context": {
        "device": {
            "type": "PC"
        },
        "custom": {
            "dimensions": [
                {
                    "TargetResourceType": "Microsoft.Compute/virtualMachines"
                },
                {
                    "ResourceManagmentProcessRunId": "827f8aaa-ab72-437c-ba48-d8917a7336a3"
                },
                {
                    "OccurrenceTime": "1/13/2017 11:24:37 AM"
                }
            ]
        }
    }
}
```
a chcete ji do tabulky Azure SQL v následujících hello formátu podle extrahování dat z objekty a pole toocopy:

| id | deviceType | targetResourceType | resourceManagmentProcessRunId | occurrenceTime |
| --- | --- | --- | --- | --- |
| ed0e4960-d9c5-11e6-85dc-d7996816aad3 | PC | Microsoft.Compute/virtualMachines | 827f8aaa-ab72-437c-ba48-d8917a7336a3 | 1/13/2017 11:24:37 AM |

vstupní datové sady Hello s **JsonFormat** typ je definován následujícím způsobem: (částečné definice se pouze relevantní částmi hello). A konkrétně:

- `structure`oddíl definuje názvy sloupců hello přizpůsobit a datový typ odpovídající hello při převodu tootabular data. Tato část se **volitelné** Pokud potřebujete toodo mapování sloupců. Další informace najdete v tématu věnovaném [určení definice struktury pro obdélníkové datové sady](#specifying-structure-definition-for-rectangular-datasets).
- `jsonPathDefinition`Určuje cestu hello JSON pro každý sloupec, která určuje, kde tooextract hello data z. toocopy data z pole, můžete použít **pole [x] .property** tooextract hodnotu hello zadána vlastnost z objektu x hello, nebo můžete použít  **pole [*] .property** toofind Hodnota Hello z libovolného objektu, který obsahuje tato vlastnost.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "deviceType",
            "type": "String"
        },
        {
            "name": "targetResourceType",
            "type": "String"
        },
        {
            "name": "resourceManagmentProcessRunId",
            "type": "String"
        },
        {
            "name": "occurrenceTime",
            "type": "DateTime"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonPathDefinition": {"id": "$.id", "deviceType": "$.context.device.type", "targetResourceType": "$.context.custom.dimensions[0].TargetResourceType", "resourceManagmentProcessRunId": "$.context.custom.dimensions[1].ResourceManagmentProcessRunId", "occurrenceTime": " $.context.custom.dimensions[2].OccurrenceTime"}      
        }
    }
}
```

**Příklad 2: mezi použít více objektů se stejný vzor z pole hello**

V této ukázce předpokládáte tootransform jeden kořenový objekt JSON do více záznamů v tabulkovém výsledek. Pokud máte soubor JSON s hello následující obsah:  

```json
{
    "ordernumber": "01",
    "orderdate": "20170122",
    "orderlines": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "sanmateo": "No 1" } ]
}
```
a chcete ji do tabulky Azure SQL v následujících hello naformátovat pomocí sloučení hello data uvnitř pole hello toocopy a křížové spojení s informacemi kořenové běžné hello:

| ordernumber | orderdate | order_pd | order_price | city |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | [{"sanmateo":"No 1"}] |
| 01 | 20170122 | P2 | 13 | [{"sanmateo":"No 1"}] |
| 01 | 20170122 | P3 | 231 | [{"sanmateo":"No 1"}] |

vstupní datové sady Hello s **JsonFormat** typ je definován následujícím způsobem: (částečné definice se pouze relevantní částmi hello). A konkrétně:

- `structure`oddíl definuje názvy sloupců hello přizpůsobit a datový typ odpovídající hello při převodu tootabular data. Tato část se **volitelné** Pokud potřebujete toodo mapování sloupců. Další informace najdete v tématu věnovaném [určení definice struktury pro obdélníkové datové sady](#specifying-structure-definition-for-rectangular-datasets).
- `jsonNodeReference`označuje tooiterate a extrahování dat z objektů hello se stejný vzor pod hello **pole** orderlines.
- `jsonPathDefinition`Určuje cestu hello JSON pro každý sloupec, která určuje, kde tooextract hello data z. V tomto příkladu "ordernumber", "orderdate" a "Město, jsou ve kořenový objekt s počínaje"$.", zatímco"order_pd"a"order_price"jsou definovány s cestou odvozen z elementu pole hello bez"$". cesta JSON.

```json
"properties": {
    "structure": [
        {
            "name": "ordernumber",
            "type": "String"
        },
        {
            "name": "orderdate",
            "type": "String"
        },
        {
            "name": "order_pd",
            "type": "String"
        },
        {
            "name": "order_price",
            "type": "Int64"
        },
        {
            "name": "city",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonNodeReference": "$.orderlines",
            "jsonPathDefinition": {"ordernumber": "$.ordernumber", "orderdate": "$.orderdate", "order_pd": "prod", "order_price": "price", "city": " $.city"}         
        }
    }
}
```

**Všimněte si hello následující body:**

* Pokud hello `structure` a `jsonPathDefinition` nejsou definovány v datové sadě služby Data Factory hello, hello aktivity kopírování zjistí hello schématu z první objekt hello a vyrovnání hello celý objekt.
* Pokud hello vstupu JSON má pole, převede hello aktivity kopírování ve výchozím nastavení hello celé pole hodnotu na řetězec. Můžete zvolit tooextract dat pomocí `jsonNodeReference` nebo `jsonPathDefinition`, nebo přeskočit zadáním není v `jsonPathDefinition`.
* Pokud existují duplicitní názvy v hello stejné úrovni, hello aktivity kopírování vybere hello naposledy.
* V názvech vlastností se rozlišují velká a malá písmena. Vlastnosti se stejným názvem, ale různým použitím velkých a malých písmen, se budou považovat za různé.

**Případ 2: Zápis dat tooJSON souboru**

Pokud máte v SQL Database tuto tabulku:

| id | order_date | order_price | order_by |
| --- | --- | --- | --- |
| 1 | 20170119 | 2000 | David |
| 2 | 20170120 | 3500 | Patrick |
| 3 | 20170121 | 4000 | Jason |

a pro každý záznam očekáváte, že toowrite objekt tooa JSON v následující formát:
```json
{
    "id": "1",
    "order": {
        "date": "20170119",
        "price": 2000,
        "customer": "David"
    }
}
```

Hello výstupní datovou sadu s **JsonFormat** typ je definován následujícím způsobem: (částečné definice se pouze relevantní částmi hello). Přesněji řečeno `structure` oddíl definuje názvy vlastností hello přizpůsobit v cílovém souboru `nestingSeparator` (výchozí hodnota je ".") bude použité tooidentify hello vnoření vrstvu ze hello název. Tato část se **volitelné** Pokud název vlastnosti hello toochange porovnávání s název zdrojového sloupce, nebo se některé vlastnosti hello vnořit.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "order.date",
            "type": "String"
        },
        {
            "name": "order.price",
            "type": "Int64"
        },
        {
            "name": "order.customer",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat"
        }
    }
}
```

### <a name="specifying-avroformat"></a>Zadávání formátu AvroFormat
Chcete-li tooparse hello Avro soubory nebo zapisovat hello data ve formátu Avro, nastavte hello `format` `type` vlastnost příliš**AvroFormat**. Není nutné toospecify všechny vlastnosti v části formátu hello v rámci typeProperties oddílu hello. Příklad:

```json
"format":
{
    "type": "AvroFormat",
}
```

Formát Avro toouse v tabulce Hive, naleznete v části příliš[kurz Apache Hive](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

Všimněte si hello následující body:  

* [Komplexní datové typy](http://avro.apache.org/docs/current/spec.html#schema_complex) se nepodporují (záznamy, výčty, pole, mapy, sjednocení a pevné typy).

### <a name="specifying-orcformat"></a>Zadávání formátu OrcFormat
Chcete-li tooparse hello ORC soubory nebo zapisovat hello data ve formátu ORC, nastavte hello `format` `type` vlastnost příliš**OrcFormat**. Není nutné toospecify všechny vlastnosti v části formátu hello v rámci typeProperties oddílu hello. Příklad:

```json
"format":
{
    "type": "OrcFormat"
}
```

> [!IMPORTANT]
> Pokud nejsou kopírování souborů ORC **jako-je** mezi místní a cloudové úložiště dat, musíte na počítači brány tooinstall hello prostředí JRE 8 (prostředí Java Runtime). 64bitová verze brány vyžaduje 64bitové prostředí JRE a 32bitová verze brány vyžaduje 32bitové prostředí JRE. Obě verze najdete [tady](http://go.microsoft.com/fwlink/?LinkId=808605). Vyberte příslušné hello.
>
>

Všimněte si hello následující body:

* Komplexní datové typy se nepodporují (STRUCT, MAP, LIST, UNION).
* Soubory ORC mají tři [možnosti komprese](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB a SNAPPY. Data Factory podporuje čtení dat ze souborů ORC v libovolném z těchto komprimovaných formátů. Používá kompresi hello kodeků je v datech hello tooread metadata hello. Ale při zápisu souboru ORC tooan, Data Factory zvolí ZLIB, což je výchozí hodnotou hello ORC. V současné době není žádná možnost toooverride toto chování.

### <a name="specifying-parquetformat"></a>Zadávání formátu ParquetFormat
Chcete-li tooparse hello Parquet soubory nebo zapisovat hello data ve formátu Parquet, nastavte hello `format` `type` vlastnost příliš**ParquetFormat**. Není nutné toospecify všechny vlastnosti v části formátu hello v rámci typeProperties oddílu hello. Příklad:

```json
"format":
{
    "type": "ParquetFormat"
}
```
> [!IMPORTANT]
> Pokud nejsou kopírování souborů Parquet **jako-je** mezi místní a cloudové úložiště dat, musíte na počítači brány tooinstall hello prostředí JRE 8 (prostředí Java Runtime). 64bitová verze brány vyžaduje 64bitové prostředí JRE a 32bitová verze brány vyžaduje 32bitové prostředí JRE. Obě verze najdete [tady](http://go.microsoft.com/fwlink/?LinkId=808605). Vyberte příslušné hello.
>
>

Všimněte si hello následující body:

* Komplexní datové typy se nepodporují (MAP, LIST).
* Soubor parquet má následující možnosti týkající se komprese hello: NONE, SNAPPY, GZIP a LZO. Data Factory podporuje čtení dat ze souborů ORC v libovolném z těchto komprimovaných formátů. Kompresní kodek hello používá v hello metadata tooread hello data. Ale při zápisu souboru Parquet tooa, Data Factory zvolí SNAPPY, což je výchozí hello Parquet formát. V současné době není žádná možnost toooverride toto chování.
