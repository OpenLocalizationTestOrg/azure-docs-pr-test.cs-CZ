---
title: .NET Core tooquery aaaUse Azure SQL Database | Microsoft Docs
description: "Toto téma ukazuje, jak toouse .NET základní toocreate program, který se připojuje tooan Azure SQL Database a dotazování pomocí příkazů Transact-SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 2d10c407f44f43b6baa3bf337cdd1173d9c9c35f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-net-core-c-tooquery-an-azure-sql-database"></a>Použít tooquery .NET Core (C#) Azure SQL database

Tento úvodní kurz ukazuje, jak toouse [.NET Core](https://www.microsoft.com/net/) v systému Windows nebo Linux/systému macOS toocreate C# program tooconnect tooan Azure SQL databáze a používat data tooquery příkazy jazyka Transact-SQL.

## <a name="prerequisites"></a>Požadavky

toocomplete tento rychlý úvodní kurz, ujistěte se, že máte hello následující:

- Databázi SQL Azure. Tento rychlý start používá hello prostředky vytvořené v jednom z těchto rychlé spuštění: 

   - [Vytvoření databáze – portál](sql-database-get-started-portal.md)
   - [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
   - [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md)

- A [pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro hello veřejnou IP adresu počítače hello použijete pro tento kurz rychlý start.
- Nainstalované [.NET Core pro váš operační systém](https://www.microsoft.com/net/core). 

## <a name="sql-server-connection-information"></a>Informace o připojení k SQL serveru

Získáte hello připojení informace potřebné tooconnect toohello Azure SQL database. Budete potřebovat hello serveru plně kvalifikovaný název, název databáze a přihlašovacích údajů v dalším postupu hello.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky. 
3. Na hello **přehled** stránky pro vaši databázi hello zkontrolujte plně kvalifikovaný název serveru, jak ukazuje následující obrázek hello. Můžete podržet přes toobring název serveru hello až hello **klikněte na tlačítko toocopy** možnost. 

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Pokud zapomenete přihlašovací údaje serveru Azure SQL Database, přejděte toohello databáze SQL serveru stránky tooview hello serveru správce název. V případě potřeby můžete resetovat heslo hello.

5. Klikněte na tlačítko **Zobrazit databázové připojovací řetězce**.

6. Zkontrolujte hello dokončení **ADO.NET** připojovací řetězec.

    ![Připojovací řetězec pro ADO.NET](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> Pravidlo brány firewall musí mít zavedené hello veřejných IP adres hello počítače, na kterém provádíte tento kurz. Pokud jsou v jiném počítači nebo mít jinou veřejnou IP adresu, vytvořte [pravidlo brány firewall na úrovni serveru pomocí portálu Azure hello](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 
>
  
## <a name="create-a-new-net-project"></a>Vytvoření nového projektu .NET

1. Otevřete příkazový řádek a vytvořte složku *sqltest*. Přejděte toohello složky můžete vytvořit a spustit hello následující příkaz:

    ```
    dotnet new console
    ```

2. Otevřete ***sqltest.csproj*** s svém oblíbeném textovém editoru a přidejte System.Data.SqlClient jako závislost pomocí hello následující kód:

    ```xml
    <ItemGroup>
        <PackageReference Include="System.Data.SqlClient" Version="4.3.0" />
    </ItemGroup>
    ```

## <a name="insert-code-tooquery-sql-database"></a>Vložení kódu tooquery SQL database

1. Ve vývojovém prostředí nebo oblíbeném textovém editoru otevřete soubor **Program.cs**.

2. Nahraďte obsah hello hello následující kód a přidat hello odpovídající hodnoty pro server, databáze, uživatele a heslo.

```csharp
using System;
using System.Data.SqlClient;
using System.Text;

namespace sqltest
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nQuery data example:");
                    Console.WriteLine("=========================================\n");
                    
                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                    sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                    sb.Append("JOIN [SalesLT].[Product] p ");
                    sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                    String sql = sb.ToString();

                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        using (SqlDataReader reader = command.ExecuteReader())
                        {
                            while (reader.Read())
                            {
                                Console.WriteLine("{0} {1}", reader.GetString(0), reader.GetString(1));
                            }
                        }
                    }                    
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
            Console.ReadLine();
        }
    }
}
```

## <a name="run-hello-code"></a>Spuštění kódu hello

1. Hello příkazového řádku spusťte následující příkazy hello:

   ```csharp
   dotnet restore
   dotnet run
   ```

2. Ověřte, že horních řádků 20 hello se vrátí a pak zavřete okno aplikace hello.


## <a name="next-steps"></a>Další kroky

- [Začínáme s .NET Core v systému Windows nebo Linux/macOS hello příkazového řádku](/dotnet/core/tutorials/using-with-xplat-cli).
- Zjistěte, jak příliš[připojení a dotazování Azure SQL database pomocí hello rozhraní .NET framework a Visual Studio](sql-database-connect-query-dotnet-visual-studio.md).  
- Zjistěte, jak příliš[navrhnout první databáze Azure SQL pomocí aplikace SSMS](sql-database-design-first-database.md) nebo [navrhnout první databáze Azure SQL pomocí rozhraní .NET](sql-database-design-first-database-csharp.md).
- Další informace o .NET najdete v [dokumentaci rozhraní .NET](https://docs.microsoft.com/dotnet/).
