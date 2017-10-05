---
title: "Použití .NET Core k dotazování služby Azure SQL Database | Dokumentace Microsoftu"
description: "Toto téma vám ukáže, jak pomocí .NET Core vytvořit program, který se připojí ke službě Azure SQL Database a bude ji dotazovat s použitím příkazů jazyka Transact-SQL."
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
ms.openlocfilehash: 046322624d3b89bb983acee863534256fee94b60
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-net-core-c-to-query-an-azure-sql-database"></a><span data-ttu-id="4b919-103">Použití .NET Core (jazyk C#) k dotazování databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="4b919-103">Use .NET Core (C#) to query an Azure SQL database</span></span>

<span data-ttu-id="4b919-104">Tento rychlý úvodní kurz ukazuje použití [.NET Core](https://www.microsoft.com/net/) v systému Windows, Linux nebo macOS k vytvoření programu v jazyce C# pro připojení k databázi SQL Azure a použití příkazů jazyka Transact-SQL k dotazování dat.</span><span class="sxs-lookup"><span data-stu-id="4b919-104">This quick start tutorial demonstrates how to use [.NET Core](https://www.microsoft.com/net/) on Windows/Linux/macOS to create a C# program to connect to an Azure SQL database and use Transact-SQL statements to query data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4b919-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4b919-105">Prerequisites</span></span>

<span data-ttu-id="4b919-106">Abyste mohli absolvovat tento rychlý úvodní kurz, ujistěte se, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="4b919-106">To complete this quick start tutorial, make sure you have the following:</span></span>

- <span data-ttu-id="4b919-107">Databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="4b919-107">An Azure SQL database.</span></span> <span data-ttu-id="4b919-108">Tento rychlý start používá prostředky vytvořené v některém z těchto rychlých startů:</span><span class="sxs-lookup"><span data-stu-id="4b919-108">This quick start uses the resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="4b919-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="4b919-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="4b919-110">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4b919-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="4b919-111">Vytvoření databáze – PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b919-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="4b919-112">[Pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro veřejnou IP adresu počítače, který používáte pro tento rychlý úvodní kurz.</span><span class="sxs-lookup"><span data-stu-id="4b919-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for the public IP address of the computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="4b919-113">Nainstalované [.NET Core pro váš operační systém](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="4b919-113">You have installed [.NET Core for your operating system](https://www.microsoft.com/net/core).</span></span> 

## <a name="sql-server-connection-information"></a><span data-ttu-id="4b919-114">Informace o připojení k SQL serveru</span><span class="sxs-lookup"><span data-stu-id="4b919-114">SQL server connection information</span></span>

<span data-ttu-id="4b919-115">Získejte informace o připojení potřebné pro připojení k databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="4b919-115">Get the connection information needed to connect to the Azure SQL database.</span></span> <span data-ttu-id="4b919-116">V dalších postupech budete potřebovat plně kvalifikovaný název serveru, název databáze a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="4b919-116">You will need the fully qualified server name, database name, and login information in the next procedures.</span></span>

1. <span data-ttu-id="4b919-117">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4b919-117">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="4b919-118">V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="4b919-118">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="4b919-119">Na stránce **Přehled** pro vaši databázi si prohlédněte plně kvalifikovaný název serveru, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="4b919-119">On the **Overview** page for your database, review the fully qualified server name as shown in the following image.</span></span> <span data-ttu-id="4b919-120">Pokud na název serveru najedete myší, můžete vyvolat možnost **Kopírování kliknutím**.</span><span class="sxs-lookup"><span data-stu-id="4b919-120">You can hover over the server name to bring up the **Click to copy** option.</span></span> 

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="4b919-122">Pokud zapomenete přihlašovací informace pro váš server služby Azure SQL Database, přejděte na stránku serveru služby SQL Database, abyste zobrazili jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="4b919-122">If you forget your Azure SQL Database server login information, navigate to the SQL Database server page to view the server admin name.</span></span> <span data-ttu-id="4b919-123">V případě potřeby můžete obnovit heslo.</span><span class="sxs-lookup"><span data-stu-id="4b919-123">You can reset the password if necessary.</span></span>

5. <span data-ttu-id="4b919-124">Klikněte na tlačítko **Zobrazit databázové připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="4b919-124">Click **Show database connection strings**.</span></span>

6. <span data-ttu-id="4b919-125">Zkontrolujte úplný připojovací řetězec **ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="4b919-125">Review the complete **ADO.NET** connection string.</span></span>

    ![Připojovací řetězec pro ADO.NET](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> <span data-ttu-id="4b919-127">Musíte mít nastavené pravidlo brány firewall pro veřejnou IP adresu počítače, na kterém provádíte tento kurz.</span><span class="sxs-lookup"><span data-stu-id="4b919-127">You must have a firewall rule in place for the public IP address of the computer on which you perform this tutorial.</span></span> <span data-ttu-id="4b919-128">Pokud jste na jiném počítači nebo máte jinou veřejnou IP adresu, vytvořte [pravidlo brány firewall na úrovni serveru pomocí webu Azure Portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="4b919-128">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using the Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 
>
  
## <a name="create-a-new-net-project"></a><span data-ttu-id="4b919-129">Vytvoření nového projektu .NET</span><span class="sxs-lookup"><span data-stu-id="4b919-129">Create a new .NET project</span></span>

1. <span data-ttu-id="4b919-130">Otevřete příkazový řádek a vytvořte složku *sqltest*.</span><span class="sxs-lookup"><span data-stu-id="4b919-130">Open a command prompt and create a folder named *sqltest*.</span></span> <span data-ttu-id="4b919-131">Přejděte do složky, kterou jste vytvořili, a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4b919-131">Navigate to the folder you created and run the following command:</span></span>

    ```
    dotnet new console
    ```

2. <span data-ttu-id="4b919-132">Otevřete soubor ***sqltest.csproj*** v oblíbeném textovém editoru a pomocí následujícího kódu přidejte System.Data.SqlClient jako závislost:</span><span class="sxs-lookup"><span data-stu-id="4b919-132">Open ***sqltest.csproj*** with your favorite text editor and add System.Data.SqlClient as a dependency using the following code:</span></span>

    ```xml
    <ItemGroup>
        <PackageReference Include="System.Data.SqlClient" Version="4.3.0" />
    </ItemGroup>
    ```

## <a name="insert-code-to-query-sql-database"></a><span data-ttu-id="4b919-133">Vložení kódu pro dotazování databáze SQL</span><span class="sxs-lookup"><span data-stu-id="4b919-133">Insert code to query SQL database</span></span>

1. <span data-ttu-id="4b919-134">Ve vývojovém prostředí nebo oblíbeném textovém editoru otevřete soubor **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="4b919-134">In your development environment or favorite text editor open **Program.cs**</span></span>

2. <span data-ttu-id="4b919-135">Nahraďte jeho obsah následujícím kódem a přidejte odpovídající hodnoty pro váš server, databázi, uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="4b919-135">Replace the contents with the following code and add the appropriate values for your server, database, user, and password.</span></span>

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

## <a name="run-the-code"></a><span data-ttu-id="4b919-136">Spuštění kódu</span><span class="sxs-lookup"><span data-stu-id="4b919-136">Run the code</span></span>

1. <span data-ttu-id="4b919-137">V příkazovém řádku spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="4b919-137">At the command prompt, run the following commands:</span></span>

   ```csharp
   dotnet restore
   dotnet run
   ```

2. <span data-ttu-id="4b919-138">Ověřte, že se vrátilo prvních 20 řádků, a potom zavřete okno aplikace.</span><span class="sxs-lookup"><span data-stu-id="4b919-138">Verify that the top 20 rows are returned and then close the application window.</span></span>


## <a name="next-steps"></a><span data-ttu-id="4b919-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b919-139">Next steps</span></span>

- <span data-ttu-id="4b919-140">[Začínáme s .NET Core v systému Windows, Linux nebo macOS pomocí příkazového řádku](/dotnet/core/tutorials/using-with-xplat-cli)</span><span class="sxs-lookup"><span data-stu-id="4b919-140">[Getting started with .NET Core on Windows/Linux/macOS using the command line](/dotnet/core/tutorials/using-with-xplat-cli).</span></span>
- <span data-ttu-id="4b919-141">Informace o [připojení k databázi SQL Azure a jejím dotazování pomocí rozhraní .NET Framework a sady Visual Studio](sql-database-connect-query-dotnet-visual-studio.md)</span><span class="sxs-lookup"><span data-stu-id="4b919-141">Learn how to [connect and query an Azure SQL database using the .NET framework and Visual Studio](sql-database-connect-query-dotnet-visual-studio.md).</span></span>  
- <span data-ttu-id="4b919-142">Informace o [návrhu první databáze SQL Azure pomocí aplikace SSMS](sql-database-design-first-database.md) nebo [návrhu první databáze SQL Azure pomocí .NET](sql-database-design-first-database-csharp.md)</span><span class="sxs-lookup"><span data-stu-id="4b919-142">Learn how to [Design your first Azure SQL database using SSMS](sql-database-design-first-database.md) or [Design your first Azure SQL database using .NET](sql-database-design-first-database-csharp.md).</span></span>
- <span data-ttu-id="4b919-143">Další informace o .NET najdete v [dokumentaci rozhraní .NET](https://docs.microsoft.com/dotnet/).</span><span class="sxs-lookup"><span data-stu-id="4b919-143">For more information about .NET, see [.NET documentation](https://docs.microsoft.com/dotnet/).</span></span>
