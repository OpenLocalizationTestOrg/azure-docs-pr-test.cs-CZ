---
title: "kopírování aaaRepeatable v Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak tooavoid duplicitních to i v případě, že je řez, který kopíruje data spustit více než jednou."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 79a3fde2b700bf0a0e167479d6a86c5bee1bf7ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="repeatable-copy-in-azure-data-factory"></a>Opakovatelných kopie v Azure Data Factory

## <a name="repeatable-read-from-relational-sources"></a>Opakovatelných číst z relační zdrojů
Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy. V Azure Data Factory může řez znovu ručně. Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě. Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.  
 
> [!NOTE]
> Hello následující ukázky jsou pro Azure SQL, ale jsou příslušné tooany datové úložiště, které podporuje obdélníková datové sady. Můžete mít tooadjust hello **typ** zdroje a hello **dotazu** vlastnosti (například: dotazu místo sqlReaderQuery) pro hello data uložit.   

Obvykle při čtení z relační úložiště, budete chtít tooread pouze hello data odpovídající toothat řez. Způsob toodo tak bude pomocí hello WindowStart WindowEnd systémové proměnné a k dispozici v Azure Data Factory. Přečtěte si informace o proměnných hello a funkcí v datové továrně Azure v hello [Azure Data Factory – funkce a systémové proměnné](data-factory-functions-variables.md) článku. Příklad: 

```json
"source": {
    "type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
Tento dotaz načte data, která se nachází ve hello řez trvání rozsahu (WindowStart -> WindowEnd) z tabulky hello MyTable. Spusťte tento řez by také vždy ujistěte se, že hello je čtení stejná data. 

V jiných případech může přát tooread hello celou tabulku a může definovat hello sqlReaderQuery následujícím způsobem:

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```

## <a name="repeatable-write-toosqlsink"></a>TooSqlSink opakovatelných zápisu
Při kopírování dat příliš**Azure SQL nebo SQL Server** z jiných úložišť dat je třeba tookeep opakovatelnosti v paměti tooavoid nezamýšleným výstupy. 

Při kopírování dat tooAzure databázi SQL nebo SQL Server, aktivity kopírování hello připojí tabulky jímky toohello data ve výchozím nastavení. Vyslovte jsou kopírování dat z CSV (hodnoty oddělené čárkami) soubor obsahující dva záznamy toohello následující tabulka v databázi Azure SQL nebo SQL Server. Při spuštění se řez, jsou dva záznamy hello zkopírovaný toohello tabulky SQL. 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Předpokládejme, že byly nalezeny chyby ve zdrojovém souboru a aktualizovat hello objemu trubice dolů z 2 too4. Pokud hello datový řez pro toto období ručně, zjistíte, že dva nové záznamy připojí tooAzure databázi SQL nebo SQL Server. Tento příklad předpokládá, že žádný hello sloupců v tabulce hello nemá hello omezení primárního klíče.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

tooavoid toto chování je třeba toospecify UPSERT sémantiku pomocí jedné z následujících dvou mechanismů hello:

### <a name="mechanism-1-using-sqlwritercleanupscript"></a>Mechanismus 1: použití sqlWriterCleanupScript
Můžete použít hello **sqlWriterCleanupScript** tooclean vlastnost zálohovat data z tabulky jímky hello před vložením dat hello při spuštění řez. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

Při spuštění se řez, je spustit skript pro vyčištění hello první toodelete data, která odpovídá toohello řez z tabulky SQL hello. Aktivita kopírování Hello vkládá data do hello tabulky SQL. Pokud se znovu spustí hello řez, množství hello je aktualizováno podle potřeby.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Předpokládejme, že hello ploché podložku záznam se odebere z původní csv hello. Znovu spustit řez hello by pak vytváří hello následující výsledek: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```

Aktivita kopírování Hello spustili hello čištění toodelete hello odpovídající data skriptu pro tento řez. Pak jej číst hello vstup ze souboru csv hello (což je obsaženy jenom jeden záznam) a vložit do hello tabulky. 

### <a name="mechanism-2-using-sliceidentifiercolumnname"></a>Mechanismus 2: pomocí sliceIdentifierColumnName
> [!IMPORTANT]
> V současné době sliceIdentifierColumnName není podporována pro Azure SQL Data Warehouse. 

Hello druhý mechanismus tooachieve opakovatelnosti spočívá v použití vyhrazených sloupce (sliceIdentifierColumnName) v hello cílové tabulky. V tomto sloupci se použije pomocí Azure Data Factory tooensure hello zdrojové a cílové synchronní. Tento přístup funguje, když je flexibilitu při změně nebo definice schématu tabulky SQL cílové hello. 

V tomto sloupci se používá službou Azure Data Factory pro účely opakovatelnost a v procesu hello Azure Data Factory zajistěte, aby nebyly žádné schéma změny toohello tabulky. Způsob toouse tohoto přístupu:

1. Definování sloupec typu **binárního souboru (32)** v hello cílové tabulky SQL. Měla by existovat žádné omezení na tomto sloupci. Název v tomto sloupci jako AdfSliceIdentifier budeme v tomto příkladu.


    Zdrojová tabulka:

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL
    )
    ```

    Cílové tabulky: 

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL,
       [AdfSliceIdentifier] [binary](32) NULL
    )
    ```

2. Ho použijte k aktivitě kopírování hello následujícím způsobem:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "AdfSliceIdentifier"
    }
    ```

Azure Data Factory naplní tento sloupec podle jeho nutné tooensure hello zdrojové a cílové zůstane synchronizovaná. mimo tento kontext není vhodné používat Hello hodnoty daného sloupce. 

Podobně jako toomechanism 1, aktivity kopírování automaticky vyčistí hello dat pro danou řez z cílového hello tabulky SQL hello. Potom vloží data ze zdroje v toohello cílové tabulky. 

## <a name="next-steps"></a>Další kroky
Projděte si následující články konektoru, které pro dokončení JSON příklady hello: 

- [Azure SQL Database](data-factory-azure-sql-connector.md)
- [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)