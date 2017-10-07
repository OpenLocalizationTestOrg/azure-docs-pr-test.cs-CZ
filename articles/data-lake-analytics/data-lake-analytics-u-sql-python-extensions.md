---
title: "aaaExtend U-SQL skriptů s Pythonem v Azure Data Lake Analytics | Microsoft Docs"
description: "Zjistěte, jak kód toorun Python na skriptů U-SQL"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/20/2017
ms.author: saveenr
ms.openlocfilehash: f051f56f67522d4f2b8e6e54fd21a5c95ce3ba92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-python"></a>Kurz: Začínáme s jazykem U-SQL s Pythonem rozšíření

Rozšíření Python pro U-SQL umožňují vývojářům tooperform massively paralelní provádění kódu jazyka Python. Hello následující příklad znázorňuje hello základní kroky:

* Použití hello `REFERENCE ASSEMBLY` příkaz tooenable Python rozšíření pro hello skript U-SQL
* Pomocí hello `REDUCE` operace toopartition hello vstupní data pro klíč
* Hello Python rozšíření pro U-SQL zahrnují předdefinované reduktorem (`Extension.Python.Reducer`) spouští se kód Python v každé reduktorem toohello vrchol přiřazen
* Hello skript U-SQL obsahuje kód hello vložených Python, který má funkci s názvem `usqlml_main` který přijímá pandas DataFrame jako vstup a vrátí pandas DataFrame jako výstup.

--

    REFERENCE ASSEMBLY [ExtPython];

    DECLARE @myScript = @"
    def get_mentions(tweet):
        return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )

    def usqlml_main(df):
        del df['time']
        del df['author']
        df['mentions'] = df.tweet.apply(get_mentions)
        del df['tweet']
        return df
    ";

    @t  = 
        SELECT * FROM 
           (VALUES
               ("D1","T1","A1","@foo Hello World @bar"),
               ("D2","T2","A2","@baz Hello World @beer")
           ) AS 
               D( date, time, author, tweet );

    @m  =
        REDUCE @t ON date
        PRODUCE date string, mentions string
        USING new Extension.Python.Reducer(pyScript:@myScript);

    OUTPUT @m
        too"/tweetmentions.csv"
        USING Outputters.Csv();

## <a name="how-python-integrates-with-u-sql"></a>Jak Python integruje s jazykem U-SQL

### <a name="datatypes"></a>Datové typy

* Řetězec a číselné sloupce z U-SQL se převedou jako-mezi Pandas a jazykem U-SQL
* Hodnoty Null U-SQL jsou převedené tooand z Pandas `NA` hodnoty

### <a name="schemas"></a>Schémata

* Index vektorů v Pandas nejsou podporovány v U-SQL. Všechny snímky vstupní data v hello Python funkce mít vždy 64-bit číselné index od 0 do hello počet řádků minus 1. 
* Datové sady U-SQL nemůže mít duplicitní názvy sloupců
* Názvy sloupců datových sad U-SQL, které nejsou typu řetězec. 

### <a name="python-versions"></a>Verze jazyka Python
Je podporován pouze Python 3.5.1 (zkompilovaném pro Windows). 

### <a name="standard-python-modules"></a>Standardní moduly jazyka Python
Všechny moduly standardní Python hello jsou zahrnuty.

### <a name="additional-python-modules"></a>Další moduly jazyka Python
Kromě hello standardní knihovny jazyka Python, mají několik běžně používané python knihovny:

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a>Zprávy o výjimkách
V současné době výjimku v kódu jazyka Python objeví jako obecný vrchol selhání. V budoucích hello hello úlohy U-SQL chybové zprávy se zobrazí zpráva o výjimce hello Python.

### <a name="input-and-output-size-limitations"></a>Vstup a výstup omezení velikosti
Všechny vrcholy má omezené množství paměti přidělené tooit. V současné době je tento limit pro AU 6 GB. Protože hello vstupní a výstupní DataFrames musí existovat v paměti hello kód Python, celková velikost hello hello vstupní a výstupní nesmí překročit 6 GB.

## <a name="see-also"></a>Viz také
* [Přehled služby Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Pro úlohy Azure Data Lake Analytics pomocí U-SQL okno funkce](data-lake-analytics-use-window-functions.md)

