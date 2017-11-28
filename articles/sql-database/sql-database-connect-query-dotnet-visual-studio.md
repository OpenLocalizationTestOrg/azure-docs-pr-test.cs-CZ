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
# <a name="use-net-c-with-visual-studio-tooconnect-and-query-an-azure-sql-database"></a><span data-ttu-id="410e0-103">Pomocí sady Visual Studio tooconnect .NET (C#) a dotaz na databázi Azure SQL</span><span class="sxs-lookup"><span data-stu-id="410e0-103">Use .NET (C#) with Visual Studio tooconnect and query an Azure SQL database</span></span>

<span data-ttu-id="410e0-104">Tento úvodní kurz ukazuje, jak toouse hello [rozhraní .NET framework](https://www.microsoft.com/net/) toocreate C# programu sady Visual Studio tooconnect tooan Azure SQL Database a používat data tooquery příkazy jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="410e0-104">This quick start tutorial demonstrates how toouse hello [.NET framework](https://www.microsoft.com/net/) toocreate a C# program with Visual Studio tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="410e0-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="410e0-105">Prerequisites</span></span>

<span data-ttu-id="410e0-106">toocomplete tento rychlý úvodní kurz, ujistěte se, že máte hello následující:</span><span class="sxs-lookup"><span data-stu-id="410e0-106">toocomplete this quick start tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="410e0-107">Databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="410e0-107">An Azure SQL database.</span></span> <span data-ttu-id="410e0-108">Tento rychlý start používá hello prostředky vytvořené v jednom z těchto rychlé spuštění:</span><span class="sxs-lookup"><span data-stu-id="410e0-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="410e0-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="410e0-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="410e0-110">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="410e0-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="410e0-111">Vytvoření databáze – PowerShell</span><span class="sxs-lookup"><span data-stu-id="410e0-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="410e0-112">A [pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro hello veřejnou IP adresu počítače hello použijete pro tento kurz rychlý start.</span><span class="sxs-lookup"><span data-stu-id="410e0-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="410e0-113">Instalaci sady [Visual Studio Community 2017, Visual Studio Professional 2017 nebo Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="410e0-113">An installation of [Visual Studio Community 2017, Visual Studio Professional 2017, or Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="410e0-114">Informace o připojení k SQL serveru</span><span class="sxs-lookup"><span data-stu-id="410e0-114">SQL server connection information</span></span>

<span data-ttu-id="410e0-115">Získáte hello připojení informace potřebné tooconnect toohello Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="410e0-115">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="410e0-116">Budete potřebovat hello serveru plně kvalifikovaný název, název databáze a přihlašovacích údajů v dalším postupu hello.</span><span class="sxs-lookup"><span data-stu-id="410e0-116">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="410e0-117">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="410e0-117">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="410e0-118">Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="410e0-118">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="410e0-119">Na hello **přehled** stránky pro vaši databázi hello zkontrolujte plně kvalifikovaný název serveru, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="410e0-119">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="410e0-120">Můžete podržet přes toobring název serveru hello až hello **klikněte na tlačítko toocopy** možnost.</span><span class="sxs-lookup"><span data-stu-id="410e0-120">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span> 

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="410e0-122">Pokud zapomenete přihlašovací údaje serveru Azure SQL Database, přejděte toohello databáze SQL serveru stránky tooview hello serveru správce název.</span><span class="sxs-lookup"><span data-stu-id="410e0-122">If you forget your Azure SQL Database server login information, navigate toohello SQL Database server page tooview hello server admin name.</span></span> <span data-ttu-id="410e0-123">V případě potřeby můžete resetovat heslo hello.</span><span class="sxs-lookup"><span data-stu-id="410e0-123">You can reset hello password if necessary.</span></span>

5. <span data-ttu-id="410e0-124">Klikněte na tlačítko **Zobrazit databázové připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="410e0-124">Click **Show database connection strings**.</span></span>

6. <span data-ttu-id="410e0-125">Zkontrolujte hello dokončení **ADO.NET** připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="410e0-125">Review hello complete **ADO.NET** connection string.</span></span>

    ![Připojovací řetězec pro ADO.NET](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> <span data-ttu-id="410e0-127">Pravidlo brány firewall musí mít zavedené hello veřejných IP adres hello počítače, na kterém provádíte tento kurz.</span><span class="sxs-lookup"><span data-stu-id="410e0-127">You must have a firewall rule in place for hello public IP address of hello computer on which you perform this tutorial.</span></span> <span data-ttu-id="410e0-128">Pokud jsou v jiném počítači nebo mít jinou veřejnou IP adresu, vytvořte [pravidlo brány firewall na úrovni serveru pomocí portálu Azure hello](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="410e0-128">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using hello Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 
>
  
## <a name="create-a-new-visual-studio-project"></a><span data-ttu-id="410e0-129">Vytvoření nového projektu v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="410e0-129">Create a new Visual Studio project</span></span>

1. <span data-ttu-id="410e0-130">V sadě Visual Studio vyberte **Soubor**, **Nový**, **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="410e0-130">In Visual Studio, choose **File**, **New**, **Project**.</span></span> 
2. <span data-ttu-id="410e0-131">V hello **nový projekt** dialogové okno a rozbalte **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="410e0-131">In hello **New Project** dialog, and expand **Visual C#**.</span></span>
3. <span data-ttu-id="410e0-132">Vyberte **konzolovou aplikaci** a zadejte *sqltest* pro název projektu hello.</span><span class="sxs-lookup"><span data-stu-id="410e0-132">Select **Console App** and enter *sqltest* for hello project name.</span></span>
4. <span data-ttu-id="410e0-133">Klikněte na tlačítko **OK** toocreate a otevřete hello nový projekt v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="410e0-133">Click **OK** toocreate and open hello new project in Visual Studio</span></span>
4. <span data-ttu-id="410e0-134">V Průzkumníku řešení klikněte pravým tlačítkem na **sqltest** a klikněte na **Správa balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="410e0-134">In Solution Explorer, right-click **sqltest** and click **Manage NuGet Packages**.</span></span> 
5. <span data-ttu-id="410e0-135">Na hello **Procházet**, vyhledejte ```System.Data.SqlClient``` a, kdy najít, vyberte ji.</span><span class="sxs-lookup"><span data-stu-id="410e0-135">On hello **Browse**, search for ```System.Data.SqlClient``` and, when found, select it.</span></span>
6. <span data-ttu-id="410e0-136">V hello **System.Data.SqlClient** klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="410e0-136">In hello **System.Data.SqlClient** page, click **Install**.</span></span>
7. <span data-ttu-id="410e0-137">Po dokončení instalace hello hello změny a pak klikněte na **OK** tooclose hello **Preview** okno.</span><span class="sxs-lookup"><span data-stu-id="410e0-137">When hello install completes, review hello changes and then click **OK** tooclose hello **Preview** window.</span></span> 
8. <span data-ttu-id="410e0-138">Pokud se zobrazí okno **Souhlas s podmínkami licence**, klikněte na **Souhlasím**.</span><span class="sxs-lookup"><span data-stu-id="410e0-138">If a **License Acceptance** window appears, click **I Accept**.</span></span>

## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="410e0-139">Vložení kódu tooquery SQL database</span><span class="sxs-lookup"><span data-stu-id="410e0-139">Insert code tooquery SQL database</span></span>
1. <span data-ttu-id="410e0-140">Přepínač příliš (nebo otevřít v případě potřeby) **Program.cs**</span><span class="sxs-lookup"><span data-stu-id="410e0-140">Switch too(or open if necessary) **Program.cs**</span></span>

2. <span data-ttu-id="410e0-141">Nahraďte obsah hello **Program.cs** s hello následující kód a přidat hello odpovídající hodnoty pro server, databáze, uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="410e0-141">Replace hello contents of **Program.cs** with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

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

## <a name="run-hello-code"></a><span data-ttu-id="410e0-142">Spuštění kódu hello</span><span class="sxs-lookup"><span data-stu-id="410e0-142">Run hello code</span></span>

1. <span data-ttu-id="410e0-143">Stiskněte klávesu **F5** toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="410e0-143">Press **F5** toorun hello application.</span></span>
2. <span data-ttu-id="410e0-144">Ověřte, že horních řádků 20 hello se vrátí a pak zavřete okno aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="410e0-144">Verify that hello top 20 rows are returned and then close hello application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="410e0-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="410e0-145">Next steps</span></span>

- <span data-ttu-id="410e0-146">Zjistěte, jak příliš[připojení a dotazování Azure SQL database pomocí .NET core](sql-database-connect-query-dotnet-core.md) v systému Windows nebo Linux/systému macOS.</span><span class="sxs-lookup"><span data-stu-id="410e0-146">Learn how too[connect and query an Azure SQL database using .NET core](sql-database-connect-query-dotnet-core.md) on Windows/Linux/macOS.</span></span>  
- <span data-ttu-id="410e0-147">Další informace o [Začínáme s .NET Core v systému Windows nebo Linux/macOS hello příkazového řádku](/dotnet/core/tutorials/using-with-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="410e0-147">Learn about [Getting started with .NET Core on Windows/Linux/macOS using hello command line](/dotnet/core/tutorials/using-with-xplat-cli).</span></span>
- <span data-ttu-id="410e0-148">Zjistěte, jak příliš[navrhnout první databáze Azure SQL pomocí aplikace SSMS](sql-database-design-first-database.md) nebo [navrhnout první databáze Azure SQL pomocí rozhraní .NET](sql-database-design-first-database-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="410e0-148">Learn how too[Design your first Azure SQL database using SSMS](sql-database-design-first-database.md) or [Design your first Azure SQL database using .NET](sql-database-design-first-database-csharp.md).</span></span>
- <span data-ttu-id="410e0-149">Další informace o .NET najdete v [dokumentaci rozhraní .NET](https://docs.microsoft.com/dotnet/).</span><span class="sxs-lookup"><span data-stu-id="410e0-149">For more information about .NET, see [.NET documentation](https://docs.microsoft.com/dotnet/).</span></span>
