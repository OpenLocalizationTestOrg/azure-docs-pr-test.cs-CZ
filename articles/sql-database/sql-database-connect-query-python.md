---
title: "Použití Pythonu k dotazování služby Azure SQL Database | Dokumentace Microsoftu"
description: "Toto téma vám ukáže, jak pomocí Pythonu vytvořit program, který se připojí ke službě Azure SQL Database a bude ji dotazovat s použitím příkazů jazyka Transact-SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 452ad236-7a15-4f19-8ea7-df528052a3ad
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: On Demand
ms.tgt_pltfrm: n
ms.devlang: python
ms.topic: quickstart
ms.date: 08/09/2017
ms.author: carlrab
ms.openlocfilehash: f7496c7b78449d5ceae861dc0daac607acdb1f84
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="use-python-to-query-an-azure-sql-database"></a>Použití Pythonu k dotazování databáze SQL Azure

 Tento rychlý start ukazuje použití [Pythonu](https://python.org) pro připojení k databázi SQL Azure a použití příkazů jazyka Transact-SQL k dotazování dat.

## <a name="prerequisites"></a>Požadavky

Abyste mohli absolvovat tento rychlý úvodní kurz, ujistěte se, že máte následující:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- [Pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro veřejnou IP adresu počítače, který používáte pro tento rychlý úvodní kurz.

- Máte nainstalovaný Python a související software pro váš operační systém:

    - **MacOS:** Nainstalujte Homebrew a Python, nainstalujte ovladač ODBC a nástroj SQLCMD a potom nainstalujte ovladač Python pro SQL Server. Viz [kroky 1.2, 1.3 a 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).
    - **Ubuntu:** Nainstalujte Python a další požadované balíčky a potom nainstalujte ovladač Python pro SQL Server. Viz [kroky 1.2, 1.3 a 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu/).
    - **Windows:** Nainstalujte nejnovější verzi Pythonu (proměnnou prostředí teď už máte nakonfigurovanou), nainstalujte ovladač ODBC a nástroj SQLCMD a potom nainstalujte ovladač Python pro SQL Server. Viz [kroky 1.2, 1.3 a 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/windows/). 

## <a name="sql-server-connection-information"></a>Informace o připojení k SQL serveru

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]
    
## <a name="insert-code-to-query-sql-database"></a>Vložení kódu pro dotazování databáze SQL 

1. V oblíbeném textovém editoru vytvořte nový soubor **sqltest.py**.  

2. Nahraďte jeho obsah následujícím kódem a přidejte odpovídající hodnoty pro váš server, databázi, uživatele a heslo.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

## <a name="run-the-code"></a>Spuštění kódu

1. V příkazovém řádku spusťte následující příkazy:

   ```Python
   python sqltest.py
   ```

2. Ověřte, že se vrátilo prvních 20 řádků, a potom zavřete okno aplikace.

## <a name="next-steps"></a>Další kroky

- [Návrh první databáze SQL Azure](sql-database-design-first-database.md)
- [Ovladače Microsoft Python pro SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Středisko pro vývojáře programující v Pythonu](https://azure.microsoft.com/develop/python/?v=17.23h)

