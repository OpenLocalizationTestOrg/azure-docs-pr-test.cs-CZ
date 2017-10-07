---
title: "aaaLoad ukázková data do SQL Data Warehouse | Microsoft Docs"
description: "Načtení ukázkových dat do SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e338ecf8-cfee-419b-b7b6-98108d381c62
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 3459c42f3aae51c27fd35db7874faf99e1e577e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-sample-data-into-sql-data-warehouse"></a>Načtení ukázkových dat do SQL Data Warehouse
Postupujte podle těchto tooload jednoduchých kroků a ukázkové společnosti Adventure Works hello dotaz do databáze. Tyto skripty se nejprve pomocí sqlcmd toorun SQL, která bude vytvářet tabulky a zobrazení. Po vytvoření tabulky hello skripty bcp tooload data používat.  Pokud ještě nemáte sqlcmd a bcp nainstalována, použijte tyto odkazy příliš[nainstalovat bcp] [ install bcp] a příliš[nainstalovat sqlcmd][install sqlcmd].

## <a name="load-sample-data"></a>Načíst ukázková data
1. Stáhnout hello [společnosti Adventure Works ukázkové skripty pro SQL Data Warehouse] [ Adventure Works Sample Scripts for SQL Data Warehouse] soubor zip.
2. Extrahujte soubory hello z adresáře tooa zip staženého v místním počítači.
3. Upravte hello extrahovat soubor aw_create.bat a nastavte následující proměnné nalezen hello horní části souboru hello hello.  Být jisti tooleave žádné mezery mezi hello "=" a parametr hello.  Níže jsou příklady, jak může vypadat provedené úpravy.
   
    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```
4. Do příkazového řádku systému Windows spusťte aw_create.bat hello upravit.  Ujistěte se, že jste v hello adresáře, kam jste uložili vašeho upravenou verzi aw_create.bat.
   Tento skript bude...
   
   * Vyřaďte společnosti Adventure Works tabulky ani zobrazení, které již existují ve vaší databázi
   * Vytvoření hello společnosti Adventure Works tabulek a zobrazení
   * Načíst každá tabulka společnosti Adventure Works pomocí bcp
   * Ověření hello počty řádků pro každou tabulku společnosti Adventure Works
   * Shromáždit statistiku pro každý sloupec pro každou tabulku společnosti Adventure Works

## <a name="query-sample-data"></a>Dotaz na ukázková data
Jakmile jste načíst ukázková data do SQL Data Warehouse, můžete rychle spustit pár dotazů.  toorun dotazu, připojení databáze společnosti Adventure Works tooyour nově vytvořený v Azure SQL DW pomocí sady Visual Studio a rozšíření SSDT, jak je popsáno v hello [dotazu pomocí sady Visual Studio] [ query with Visual Studio] dokumentu.

Příklad použití jednoduchých vyberte příkaz tooget všechny údaje hello hello zaměstnanců:

```sql
SELECT * FROM DimEmployee;
```

Příklad komplexnější dotaz pomocí konstruktory, jako jsou toolook GROUP BY v hello celkovou velikost pro všechny prodeje na každý den:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Příklad výběru se toofilter klauzule WHERE se objednávky z do určitého data:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL Data Warehouse podporuje téměř všechny konstrukce T-SQL, které systém SQL Server podporuje.  Případné rozdíly jsou popsány v našem [migrace kódu] [ migrate code] dokumentaci.

## <a name="next-steps"></a>Další kroky
Teď, když využijete prvního tootry několik dotazů s ukázkovými daty, podívejte se jak příliš[vyvíjet][develop], [načíst][load], nebo [ migrace] [ migrate] tooSQL datového skladu.

<!--Image references-->

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[query with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[migrate code]: sql-data-warehouse-migrate-code.md
[install bcp]: sql-data-warehouse-load-with-bcp.md
[install sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works Sample Scripts for SQL Data Warehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
