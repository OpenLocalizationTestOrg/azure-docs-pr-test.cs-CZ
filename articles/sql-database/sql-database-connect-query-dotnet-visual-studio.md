---
title: aaaUse Visual Studio a .NET tooquery Azure SQL Database | Microsoft Docs
description: "Toto téma ukazuje, jak toouse Visual Studio toocreate program, který se připojuje tooan Azure SQL Database a dotazování pomocí příkazů Transact-SQL."
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
ms.openlocfilehash: 038cfb9c680217dfeea5a9996a0abed88cc80559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-net-c-with-visual-studio-tooconnect-and-query-an-azure-sql-database"></a>Pomocí sady Visual Studio tooconnect .NET (C#) a dotaz na databázi Azure SQL

Tento úvodní kurz ukazuje, jak toouse hello [rozhraní .NET framework](https://www.microsoft.com/net/) toocreate C# programu sady Visual Studio tooconnect tooan Azure SQL Database a používat data tooquery příkazy jazyka Transact-SQL.

## <a name="prerequisites"></a>Požadavky

toocomplete tento rychlý úvodní kurz, ujistěte se, že máte hello následující:

- Databázi SQL Azure. Tento rychlý start používá hello prostředky vytvořené v jednom z těchto rychlé spuštění: 

   - [Vytvoření databáze – portál](sql-database-get-started-portal.md)
   - [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
   - [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md)

- A [pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro hello veřejnou IP adresu počítače hello použijete pro tento kurz rychlý start.
- Instalaci sady [Visual Studio Community 2017, Visual Studio Professional 2017 nebo Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

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
  
## <a name="create-a-new-visual-studio-project"></a>Vytvoření nového projektu v sadě Visual Studio

1. V sadě Visual Studio vyberte **Soubor**, **Nový**, **Projekt**. 
2. V hello **nový projekt** dialogové okno a rozbalte **Visual C#**.
3. Vyberte **konzolovou aplikaci** a zadejte *sqltest* pro název projektu hello.
4. Klikněte na tlačítko **OK** toocreate a otevřete hello nový projekt v sadě Visual Studio
4. V Průzkumníku řešení klikněte pravým tlačítkem na **sqltest** a klikněte na **Správa balíčků NuGet**. 
5. Na hello **Procházet**, vyhledejte ```System.Data.SqlClient``` a, kdy najít, vyberte ji.
6. V hello **System.Data.SqlClient** klikněte na tlačítko **nainstalovat**.
7. Po dokončení instalace hello hello změny a pak klikněte na **OK** tooclose hello **Preview** okno. 
8. Pokud se zobrazí okno **Souhlas s podmínkami licence**, klikněte na **Souhlasím**.

## <a name="insert-code-tooquery-sql-database"></a>Vložení kódu tooquery SQL database
1. Přepínač příliš (nebo otevřít v případě potřeby) **Program.cs**

2. Nahraďte obsah hello **Program.cs** s hello následující kód a přidat hello odpovídající hodnoty pro server, databáze, uživatele a heslo.

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

1. Stiskněte klávesu **F5** toorun hello aplikace.
2. Ověřte, že horních řádků 20 hello se vrátí a pak zavřete okno aplikace hello.

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak příliš[připojení a dotazování Azure SQL database pomocí .NET core](sql-database-connect-query-dotnet-core.md) v systému Windows nebo Linux/systému macOS.  
- Další informace o [Začínáme s .NET Core v systému Windows nebo Linux/macOS hello příkazového řádku](/dotnet/core/tutorials/using-with-xplat-cli).
- Zjistěte, jak příliš[navrhnout první databáze Azure SQL pomocí aplikace SSMS](sql-database-design-first-database.md) nebo [navrhnout první databáze Azure SQL pomocí rozhraní .NET](sql-database-design-first-database-csharp.md).
- Další informace o .NET najdete v [dokumentaci rozhraní .NET](https://docs.microsoft.com/dotnet/).
