## <a name="repeatability-during-copy"></a>Opakovatelnosti během kopírování
Při kopírování dat tooAzure SQL nebo SQL Server z jiných dat ukládá jeden opakovatelnosti tookeep potřebám v paměti tooavoid nezamýšleným výstupy. 

Při kopírování dat tooAzure databázi SQL nebo SQL Server, bude aktivitou kopírování pomocí výchozí připojení hello datové sady toohello podřízený tabulka ve výchozím nastavení. Například při kopírování dat ze zdroje souboru CSV (data hodnot oddělených čárkami) obsahující dva zaznamenává tooAzure databázi SQL nebo SQL Server, to je co hello tabulka vypadá jako:

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Předpokládejme, že nalezlo chyby v zdrojový soubor a množství aktualizované hello zkumavky dolů z 2 too4 hello zdrojový soubor. Pokud znovu spustíte hello datový řez pro toto období, zjistíte, že dva nové záznamy připojí tooAzure databázi SQL nebo SQL Server. Hello níže předpokládá, že žádná z hello sloupců v tabulce hello nemá hello omezení primárního klíče.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

tooavoid, budete potřebovat toospecify UPSERT sémantiku s využitím mezi hello pod 2 mechanismy, které jsou uvedeny níže.

> [!NOTE]
> Řez opětovně lze spustit automaticky v Azure Data Factory podle určené zásady opakování hello.
> 
> 

### <a name="mechanism-1"></a>Mechanismus 1
Můžete využít **sqlWriterCleanupScript** toofirst vlastnost provedení čištění akce při spuštění řez. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

Hello skript pro vyčištění by byl proveden první během kopírování pro danou řez, který by odstranit hello data z hello tabulky SQL odpovídající toothat řez. Hello aktivity budou následně insert hello tabulky SQL hello data. 

Pokud hello řez je nyní spusťte znovu a potom můžete zjistí, že množství hello je aktualizovat, protože požadovaný.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Předpokládejme, že hello ploché podložku záznam se odebere z původní csv hello. Opětovným spuštěním řez hello by pak vytváří hello následující výsledek: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
Nic nového měl toobe Hotovo. Aktivita kopírování Hello spustili hello čištění toodelete hello odpovídající data skriptu pro tento řez. Pak jej číst hello vstup ze souboru csv hello (což je obsaženy pouze 1 záznam) a vložit do hello tabulky. 

### <a name="mechanism-2"></a>Mechanismus 2
> [!IMPORTANT]
> sliceIdentifierColumnName není v současné době podporovaný pro Azure SQL Data Warehouse. 

Jiný mechanismus tooachieve opakovatelnosti spočívá v použití vyhrazených sloupec (**sliceIdentifierColumnName**) v hello cílové tabulky. V tomto sloupci se použije pomocí Azure Data Factory tooensure hello zdrojové a cílové synchronní. Tento přístup funguje, když je flexibilitu při změně nebo definice schématu tabulky SQL cílové hello. 

V tomto sloupci se použije službou Azure Data Factory pro účely opakovatelnost a v procesu hello Azure Data Factory nebude mít žádné schéma změny toohello tabulky. Způsob toouse tohoto přístupu:

1. Definujte sloupce binárního typu (32) v cílovém hello tabulky SQL. Měla by existovat žádné omezení na tomto sloupci. V tomto příkladu budeme název v tomto sloupci jako 'ColumnForADFuseOnly'.
2. Ho použijte k aktivitě kopírování hello následujícím způsobem:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

Azure Data Factory bude naplnit tento sloupec podle jeho nutné tooensure hello zdrojové a cílové zůstane synchronizovaná. mimo tento kontext hello uživatele by neměl používat Hello hodnoty daného sloupce. 

Podobně jako toomechanism 1, aktivitě kopírování se automaticky první vyčištění hello dat pro hello zadané řez z cílového hello tabulky SQL a poté spusťte aktivitu kopírování hello normálně tooinsert hello data ze zdroje toodestination pro tento řez. 

