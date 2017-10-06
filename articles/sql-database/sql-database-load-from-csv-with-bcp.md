---
title: soubor aaaLoad data ze souboru CSV do Azure SQL Database (bcp) | Microsoft Docs
description: "Pro malou velikost dat využívá bcp tooimport data do Azure SQL Database."
services: sql-database
documentationcenter: NA
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 875f9b8d-f1a1-4895-b717-f45570fb7f80
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 9350e459aa844223820fbbd849a830cf0354d4e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-csv-into-azure-sql-database-flat-files"></a>Načtení dat ze souboru CSV do Azure SQL Database (ploché soubory)
Můžete data tooimport nástroj příkazového řádku bcp hello ze souboru CSV do Azure SQL Database.

## <a name="before-you-begin"></a>Než začnete
### <a name="prerequisites"></a>Požadavky
toocomplete hello kroky v tomto článku, budete potřebovat:

* Logický server a databáze Azure SQL Database
* Hello bcp nainstalovaný nástroj příkazového řádku
* Hello sqlcmd nainstalovaný nástroj příkazového řádku

Hello nástroje bcp a sqlcmd si můžete stáhnout z hello [Microsoft Download Center][Microsoft Download Center].

### <a name="data-in-ascii-or-utf-16-format"></a>Data ve formátu ASCII nebo UTF-16
Pokud se tento kurz používáte svoje vlastní data, musí vaše data toouse hello ASCII nebo UTF-16 kódování, protože bcp nepodporuje kódování UTF-8. 

## <a name="1-create-a-destination-table"></a>1. Vytvoření cílové tabulky
Definujte tabulku v databázi SQL jako hello cílové tabulky. Hello sloupců v tabulce hello musí odpovídat toohello dat v jednotlivých řádcích vašeho datového souboru.

toocreate tabulku, otevřete příkazový řádek a pomocí sqlcmd.exe toorun hello následující příkaz:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
"
```


## <a name="2-create-a-source-data-file"></a>2. Vytvoření zdrojového datového souboru
Otevřete Poznámkový blok a zkopírujte hello následující řádky dat do nového textového souboru a potom uložte tento soubor tooyour místního dočasného adresáře C:\Temp\DimDate2.txt. Tato data jsou ve formátu ASCII.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

(Volitelné) tooexport svoje vlastní data z databáze systému SQL Server, otevřete příkazový řádek a spusťte následující příkaz hello. TableName, ServerName, DatabaseName, Username a Password nahraďte svými vlastními informacemi.

```bcp
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t , 
```

## <a name="3-load-hello-data"></a>3. Načtení dat hello
tooload hello data, otevřete příkazový řádek a spusťte následující příkaz, nahraďte hello hodnoty pro název serveru, název databáze, uživatelské jméno a heslo s informacemi o sobě hello.

```bcp
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ,
```

Použijte tento příkaz tooverify hello data načetla správně.

```bcp
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

výsledky Hello by měl vypadat takto:

| DateId | CalendarQuarter | FiscalQuarter |
| --- | --- | --- |
| 20150101 |1 |3 |
| 20150201 |1 |3 |
| 20150301 |1 |3 |
| 20150401 |2 |4 |
| 20150501 |2 |4 |
| 20150601 |2 |4 |
| 20150701 |3 |1 |
| 20150801 |3 |1 |
| 20150801 |3 |1 |
| 20151001 |4 |2 |
| 20151101 |4 |2 |
| 20151201 |4 |2 |

## <a name="next-steps"></a>Další kroky
toomigrate databázi systému SQL Server najdete v části [migrace databáze SQL serveru](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
