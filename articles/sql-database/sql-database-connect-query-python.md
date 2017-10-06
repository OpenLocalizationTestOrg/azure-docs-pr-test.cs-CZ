---
title: aaaUse Python tooquery Azure SQL Database | Microsoft Docs
description: "Toto téma ukazuje, jak toouse Python toocreate program, který se připojuje tooan Azure SQL Database a dotazování pomocí příkazů Transact-SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 452ad236-7a15-4f19-8ea7-df528052a3ad
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 08/08/2017
ms.author: carlrab
ms.openlocfilehash: 6c786e7a61f5ed3e7d2395da59acfeae008e90e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-python-tooquery-an-azure-sql-database"></a>Použít Python tooquery Azure SQL database

 Tento rychlý start předvádí, jak toouse [Python](https://python.org) tooconnect tooan Azure SQL databáze a používat data tooquery příkazy jazyka Transact-SQL.

## <a name="prerequisites"></a>Požadavky

toocomplete tento rychlý úvodní kurz, ujistěte se, že máte hello následující:

- Databázi SQL Azure. Tento rychlý start používá hello prostředky vytvořené v jednom z těchto rychlé spuštění: 

   - [Vytvoření databáze – portál](sql-database-get-started-portal.md)
   - [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
   - [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md)

- A [pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro hello veřejnou IP adresu počítače hello použijete pro tento kurz rychlý start.

- Máte nainstalovaný Python a související software pro váš operační systém.

    - **Systému MacOS**: nainstalovat Homebrew a Python, nainstalujte ovladač ODBC hello a SQLCMD a potom nainstalujte hello Python ovladač pro systém SQL Server. Viz [kroky 1.2, 1.3 a 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).
    - **Ubuntu**: nainstalovat Python a další požadované balíčky a pak hello instalaci ovladačů Python pro systém SQL Server. Viz [kroky 1.2, 1.3 a 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu/).
    - **Windows**: nainstalujte nejnovější verzi jazyka Python (proměnnou prostředí je nyní nakonfigurována pro vás) hello, nainstalujte ovladač ODBC hello a SQLCMD a pak nainstalujte hello Python ovladač pro systém SQL Server. Viz [kroky 1.2, 1.3 a 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/windows/). 

## <a name="sql-server-connection-information"></a>Informace o připojení k SQL serveru

Získáte hello připojení informace potřebné tooconnect toohello Azure SQL database. Budete potřebovat hello serveru plně kvalifikovaný název, název databáze a přihlašovacích údajů v dalším postupu hello.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky. 
3. Na hello **přehled** stránky pro vaši databázi hello zkontrolujte plně kvalifikovaný název serveru, jak ukazuje následující obrázek hello. Můžete podržet přes toobring název serveru hello až hello **klikněte na tlačítko toocopy** možnost.  

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Pokud jste zapomněli vaše přihlašovací údaje serveru, přejděte toohello databáze SQL serveru stránky tooview hello serveru správce název nebo, v případě potřeby obnovit heslo hello.     
    
## <a name="insert-code-tooquery-sql-database"></a>Vložení kódu tooquery SQL database 

1. V oblíbeném textovém editoru vytvořte nový soubor **sqltest.py**.  

2. Nahraďte obsah hello hello následující kód a přidat hello odpovídající hodnoty pro server, databáze, uživatele a heslo.

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

## <a name="run-hello-code"></a>Spuštění kódu hello

1. Hello příkazového řádku spusťte následující příkazy hello:

   ```Python
   python sqltest.py
   ```

2. Ověřte, že horních řádků 20 hello se vrátí a pak zavřete okno aplikace hello.

## <a name="next-steps"></a>Další kroky

- [Návrh první databáze SQL Azure](sql-database-design-first-database.md)
- [Ovladače Microsoft Python pro SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Středisko pro vývojáře programující v Pythonu](https://azure.microsoft.com/develop/python/?v=17.23h)

