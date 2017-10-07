---
title: "VS Code: Připojení a dotazování dat ve službě Azure SQL Database | Dokumentace Microsoftu"
description: "Zjistěte, jak tooconnect tooSQL databáze v Azure pomocí sady Visual Studio Code. Potom spusťte příkazy jazyka Transact-SQL (T-SQL) tooquery a upravit data."
metacanonical: 
keywords: "připojit databáze toosql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 676bd799-a571-4bb8-848b-fb1720007866
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/20/2017
ms.author: carlrab
ms.openlocfilehash: ed8bdbfc3271b463a21cde5ff6b5f05fd0ff51d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-use-visual-studio-code-tooconnect-and-query-data"></a>Azure SQL Database: Použití Visual Studio Code tooconnect a dotazování dat

[Visual Studio Code](https://code.visualstudio.com/docs) je editor grafické kódu pro Linux, systému macOS, a Windows, které podporuje rozšíření, včetně hello [mssql rozšíření](https://aka.ms/mssql-marketplace) k dotazování systému Microsoft SQL Server, Azure SQL Database a SQL Data Warehouse. Tento rychlý start předvádí, jak toouse Visual Studio Code tooconnect tooan Azure SQL database a použití jazyka Transact-SQL příkazy tooquery, vložit, aktualizovat a odstranit data v databázi hello.

## <a name="prerequisites"></a>Požadavky

Tento rychlý start používá jako jeho výchozí prostředky hello bodu vytvořené v jednom z těchto rychlé spuštění:

- [Vytvoření databáze – portál](sql-database-get-started-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
- [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md)

Než začnete, ujistěte se, máte nainstalovanou nejnovější verzi hello [Visual Studio Code](https://code.visualstudio.com/Download) a načíst hello [mssql rozšíření](https://aka.ms/mssql-marketplace). Instalační pokyny pro rozšíření mssql hello najdete v tématu [instalaci VS Code](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code) a zobrazit [mssql pro Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql). 

## <a name="configure-vs-code"></a>Konfigurace VS Code 

### <a name="mac-os"></a>**Mac OS**
U systému macOS je nutné tooinstall OpenSSL, který je předpokladu pro DotNet základní tohoto rozšíření mssql používá. Otevřete terminálu a zadejte následující příkazy tooinstall hello **brew** a **OpenSSL**. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

Není potřeba žádná zvláštní konfigurace.

### <a name="windows"></a>**Windows**

Není potřeba žádná zvláštní konfigurace.

## <a name="sql-server-connection-information"></a>Informace o připojení k SQL serveru

Získáte hello připojení informace potřebné tooconnect toohello Azure SQL database. Budete potřebovat hello serveru plně kvalifikovaný název, název databáze a přihlašovacích údajů v dalším postupu hello.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky. 
3. Na hello **přehled** stránky pro vaši databázi hello zkontrolujte plně kvalifikovaný název serveru, jak ukazuje následující obrázek hello. Můžete podržet přes toobring název serveru hello až hello **klikněte na tlačítko toocopy** možnost.

   ![informace o připojení](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Pokud jste zapomněli hello přihlašovací informace pro váš server databáze SQL Azure, přejděte toohello databáze SQL serveru stránky tooview hello serveru správce název a, v případě potřeby obnovit heslo hello. 

## <a name="set-language-mode-toosql"></a>TooSQL režimu jazykové sady

Sada hello jazyk režim je nastaven příliš**SQL** ve Visual Studio Code tooenable mssql příkazy a IntelliSense T-SQL.

1. Otevřete nové okno nástroje Visual Studio Code. 

2. Klikněte na tlačítko **prostý Text** v hello pravém dolním rohu hello stavový řádek.
3. V hello **vyberte jazyk režimu** rozevírací nabídky, které se otevře, typ **SQL**a potom stiskněte klávesu **ENTER** tooset hello jazyk režimu tooSQL. 

   ![Režim jazyka SQL](./media/sql-database-connect-query-vscode/vscode-language-mode.png)

## <a name="connect-tooyour-database"></a>Připojit databáze tooyour

Pomocí Visual Studio Code tooestablish serveru Azure SQL Database tooyour připojení.

> [!IMPORTANT]
> Než budete pokračovat, ujistěte se, že máte připravené informace o vašem serveru, databázi a přihlašovacích údajích. Až začnete zadat informace o profilu hello připojení,-li změnit váš výběr z Visual Studio Code, budete mít toorestart vytváření profilu připojení hello.
>

1. V produktu VS Code, stiskněte klávesu **CTRL + SHIFT + P** (nebo **F1**) tooopen hello palety příkaz.

2. Zadejte **sqlcon** a stiskněte klávesu **ENTER**.

3. Stiskněte klávesu **ENTER** tooselect **vytvořit profil připojení**. Tím se vytvoří profil připojení pro vaši instanci SQL Serveru.

4. Postupujte podle vlastnosti připojení hello výzvy toospecify hello pro nový profil připojení hello. Po zadání jednotlivých hodnot, stiskněte klávesu **ENTER** toocontinue. 

   | Nastavení       | Navrhovaná hodnota | Popis |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Název serveru | název plně kvalifikovaný server Hello | Hello název by měl být přibližně takto: **mynewserver20170313.database.windows.net**. |
   | **Název databáze** | mySampleDatabase | Název Hello tooconnect toowhich hello databáze. |
   | **Ověřování** | Přihlášení k SQL serveru| Ověřování systému SQL je typ hello pouze ověřování, který jsme nakonfigurovali v tomto kurzu. |
   | **Uživatelské jméno** | účet správce serveru Hello | Toto je hello účet, který jste zadali při vytváření hello server. |
   | **Heslo (Přihlášení SQL)** | Hello heslo pro váš účet správce serveru | Toto je hello heslo, které jste zadali při vytváření hello server. |
   | **Uložit heslo?** | Ano nebo Ne | Pokud nechcete, aby heslo hello tooenter pokaždé, když, vyberte možnost Ano. |
   | **Zadejte název pro tento profil.** | Název profilu, jako například **mySampleDatabase** | Pokud uložíte název profilu, zrychlíte připojování k dalším přihlašovacím profilům. | 

5. Stiskněte klávesu hello **ESC** klíče tooclose hello informační zpráva informující o tom, že hello profil je vytvořen a připojené.

6. Ověření připojení ve stavovém hello.

   ![Stav připojení](./media/sql-database-connect-query-vscode/vscode-connection-status.png)

## <a name="query-data"></a>Dotazování dat

Použití hello následující kód tooquery produktů hello prvních 20 počítačů podle kategorie pomocí hello [vyberte](https://msdn.microsoft.com/library/ms189499.aspx) příkazu Transact-SQL.

1. V hello **Editor** okno, zadejte následující dotaz v okně dotazu prázdný hello hello:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

2. Stiskněte klávesu **CTRL + SHIFT + E** tooretrieve data z tabulek produktu a ProductCategory hello.

    ![Dotaz](./media/sql-database-connect-query-vscode/query.png)

## <a name="insert-data"></a>Vložení dat

Použití hello následující kód tooinsert nového produktu do tabulky SalesLT.Product hello pomocí hello [vložit](https://msdn.microsoft.com/library/ms174335.aspx) příkazu Transact-SQL.

1. V hello **Editor** okně hello předchozí dotaz odstranit a zadejte hello následující dotaz:

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

2. Stiskněte klávesu **CTRL + SHIFT + E** tooinsert nový řádek v tabulce produktu hello.

## <a name="update-data"></a>Aktualizace dat

Použití hello následující kód tooupdate hello nového produktu, zda jste dříve přidali pomocí hello [aktualizace](https://msdn.microsoft.com/library/ms177523.aspx) příkazu Transact-SQL.

1.  V hello **Editor** okně hello předchozí dotaz odstranit a zadejte hello následující dotaz:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Stiskněte klávesu **CTRL + SHIFT + E** tooupdate hello zadaný řádek v tabulce produktu hello.

## <a name="delete-data"></a>Odstranění dat

Použití hello následující kód toodelete hello nového produktu, zda jste dříve přidali pomocí hello [odstranit](https://msdn.microsoft.com/library/ms189835.aspx) příkazu Transact-SQL.

1. V hello **Editor** okně hello předchozí dotaz odstranit a zadejte hello následující dotaz:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Stiskněte klávesu **CTRL + SHIFT + E** toodelete hello zadaný řádek v tabulce produktu hello.

## <a name="next-steps"></a>Další kroky

- tooconnect a dotazu pomocí SQL Server Management Studio, najdete v části [připojit a zadávat dotazy pomocí SSMS](sql-database-connect-query-ssms.md).
- Článek z časopisu MSDN o použití editoru Visual Studio Code najdete v blogovém příspěvku [Vytvoření databáze IDE s rozšířením MSSQL](https://msdn.microsoft.com/magazine/mt809115).
