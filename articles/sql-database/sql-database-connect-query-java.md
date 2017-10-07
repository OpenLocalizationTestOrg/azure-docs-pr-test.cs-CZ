---
title: aaaUse Java tooquery Azure SQL Database | Microsoft Docs
description: "Toto téma ukazuje, jak toouse Java toocreate program, který se připojuje tooan Azure SQL Database a dotazování pomocí příkazů Transact-SQL."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: andrela
ms.openlocfilehash: f014edbe38ca0e7b6e43f4eb4d2e53d3561bf3e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-java-tooquery-an-azure-sql-database"></a><span data-ttu-id="46efa-103">Použít Java tooquery Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="46efa-103">Use Java tooquery an Azure SQL database</span></span>

<span data-ttu-id="46efa-104">Tento rychlý start předvádí, jak toouse [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) tooconnect tooan Azure SQL databáze a pak používat data tooquery příkazy jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="46efa-104">This quick start demonstrates how toouse [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) tooconnect tooan Azure SQL database and then use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="46efa-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="46efa-105">Prerequisites</span></span>

<span data-ttu-id="46efa-106">toocomplete tento rychlý úvodní kurz, ujistěte se, že máte hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="46efa-106">toocomplete this quick start tutorial, make sure you have hello following prerequisites:</span></span>

- <span data-ttu-id="46efa-107">Databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="46efa-107">An Azure SQL database.</span></span> <span data-ttu-id="46efa-108">Tento rychlý start používá hello prostředky vytvořené v jednom z těchto rychlé spuštění:</span><span class="sxs-lookup"><span data-stu-id="46efa-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="46efa-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="46efa-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="46efa-110">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="46efa-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="46efa-111">Vytvoření databáze – PowerShell</span><span class="sxs-lookup"><span data-stu-id="46efa-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="46efa-112">A [pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro hello veřejnou IP adresu počítače hello použijete pro tento kurz rychlý start.</span><span class="sxs-lookup"><span data-stu-id="46efa-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>

- <span data-ttu-id="46efa-113">Máte nainstalovanou Javu a související software pro váš operační systém.</span><span class="sxs-lookup"><span data-stu-id="46efa-113">You have installed Java and related software for your operating system.</span></span>

    - <span data-ttu-id="46efa-114">**MacOS:** Nainstalujte Homebrew a Javu a potom nainstalujte Maven.</span><span class="sxs-lookup"><span data-stu-id="46efa-114">**MacOS**: Install Homebrew and Java, and then install Maven.</span></span> <span data-ttu-id="46efa-115">Viz [kroky 1.2 a 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).</span><span class="sxs-lookup"><span data-stu-id="46efa-115">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).</span></span>
    - <span data-ttu-id="46efa-116">**Ubuntu**: Nainstalujte hello Java Development Kit a nainstalujte Maven.</span><span class="sxs-lookup"><span data-stu-id="46efa-116">**Ubuntu**:  Install hello Java Development Kit, and install Maven.</span></span> <span data-ttu-id="46efa-117">Viz [kroky 1.2, 1.3 a 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="46efa-117">See [Step 1.2, 1.3, and 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).</span></span>
    - <span data-ttu-id="46efa-118">**Windows**: instalace hello Java Development Kit a Maven.</span><span class="sxs-lookup"><span data-stu-id="46efa-118">**Windows**: Install hello Java Development Kit, and Maven.</span></span> <span data-ttu-id="46efa-119">Viz [kroky 1.2 a 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).</span><span class="sxs-lookup"><span data-stu-id="46efa-119">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).</span></span>    

## <a name="sql-server-connection-information"></a><span data-ttu-id="46efa-120">Informace o připojení k SQL serveru</span><span class="sxs-lookup"><span data-stu-id="46efa-120">SQL server connection information</span></span>

<span data-ttu-id="46efa-121">Získáte hello připojení informace potřebné tooconnect toohello Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="46efa-121">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="46efa-122">Budete potřebovat hello serveru plně kvalifikovaný název, název databáze a přihlašovacích údajů v dalším postupu hello.</span><span class="sxs-lookup"><span data-stu-id="46efa-122">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="46efa-123">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="46efa-123">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="46efa-124">Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="46efa-124">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="46efa-125">Na hello **přehled** pro vaši databázi si prohlédněte hello serveru plně kvalifikovaný název, jak ukazuje následující obrázek hello: můžete podržet přes toobring název serveru hello až hello **klikněte na tlačítko toocopy** možnost.</span><span class="sxs-lookup"><span data-stu-id="46efa-125">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image: You can hover over hello server name toobring up hello **Click toocopy** option.</span></span>  

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="46efa-127">Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello databáze SQL serveru stránky tooview hello serveru správce název.</span><span class="sxs-lookup"><span data-stu-id="46efa-127">If you forget your server login information, navigate toohello SQL Database server page tooview hello server admin name.</span></span>  <span data-ttu-id="46efa-128">V případě potřeby resetovat heslo hello.</span><span class="sxs-lookup"><span data-stu-id="46efa-128">If necessary, reset hello password.</span></span>     

## <a name="create-maven-project-and-dependencies"></a><span data-ttu-id="46efa-129">**Vytvoření projektu a závislostí v Mavenu**</span><span class="sxs-lookup"><span data-stu-id="46efa-129">**Create Maven project and dependencies**</span></span>
1. <span data-ttu-id="46efa-130">Hello terminálu, vytvořte nový projekt Maven s názvem **sqltest**.</span><span class="sxs-lookup"><span data-stu-id="46efa-130">From hello terminal, create a new Maven project called **sqltest**.</span></span> 

   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```

2. <span data-ttu-id="46efa-131">Po zobrazení výzvy zadejte **Y**.</span><span class="sxs-lookup"><span data-stu-id="46efa-131">Enter **Y** when prompted.</span></span>
3. <span data-ttu-id="46efa-132">Změnit adresář příliš**sqltest** a otevřete ***pom.xml*** s svém oblíbeném textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="46efa-132">Change directory too**sqltest** and open ***pom.xml*** with your favorite text editor.</span></span>  <span data-ttu-id="46efa-133">Přidat hello **ovladač JDBC Microsoft pro systém SQL Server** závislosti projektu tooyour s využitím hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="46efa-133">Add hello **Microsoft JDBC Driver for SQL Server** tooyour project's dependencies using hello following code:</span></span>

   ```xml
   <dependency>
       <groupId>com.microsoft.sqlserver</groupId>
       <artifactId>mssql-jdbc</artifactId>
       <version>6.2.1.jre8</version>
   </dependency>
   ```

4. <span data-ttu-id="46efa-134">Také v ***pom.xml***, přidejte následující vlastnosti tooyour projektu hello.</span><span class="sxs-lookup"><span data-stu-id="46efa-134">Also in ***pom.xml***, add hello following properties tooyour project.</span></span>  <span data-ttu-id="46efa-135">Pokud nemáte oddíl vlastností, můžete ho přidat po hello závislosti.</span><span class="sxs-lookup"><span data-stu-id="46efa-135">If you don't have a properties section, you can add it after hello dependencies.</span></span>

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

5. <span data-ttu-id="46efa-136">Soubor ***pom.xml*** uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="46efa-136">Save and close ***pom.xml***.</span></span>

## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="46efa-137">Vložení kódu tooquery SQL database</span><span class="sxs-lookup"><span data-stu-id="46efa-137">Insert code tooquery SQL database</span></span>

1. <span data-ttu-id="46efa-138">V projektu v Mavenu už byste měli mít soubor ***App.java*** umístěný v: ..\sqltest\src\main\java\com\sqlsamples\App.java</span><span class="sxs-lookup"><span data-stu-id="46efa-138">You should already have a file called ***App.java*** in your Maven project located at:  ..\sqltest\src\main\java\com\sqlsamples\App.java</span></span>

2. <span data-ttu-id="46efa-139">Otevřete soubor hello a nahraďte jeho obsah s hello následující kód a přidat hello odpovídající hodnoty pro server, databáze, uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="46efa-139">Open hello file and replace its contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.DriverManager;

   public class App {

    public static void main(String[] args) {
    
        // Connect toodatabase
           String hostName = "your_server.database.windows.net";
           String dbName = "your_database";
           String user = "your_username";
           String password = "your_password";
           String url = String.format("jdbc:sqlserver://%s:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
           Connection connection = null;

           try {
                   connection = DriverManager.getConnection(url);
                   String schema = connection.getSchema();
                   System.out.println("Successful connection - Schema: " + schema);

                   System.out.println("Query data example:");
                   System.out.println("=========================================");

                   // Create and execute a SELECT SQL statement.
                   String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName " 
                       + "FROM [SalesLT].[ProductCategory] pc "  
                       + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";
                
                   try (Statement statement = connection.createStatement();
                       ResultSet resultSet = statement.executeQuery(selectSql)) {

                           // Print results from select statement
                           System.out.println("Top 20 categories:");
                           while (resultSet.next())
                           {
                               System.out.println(resultSet.getString(1) + " "
                                   + resultSet.getString(2));
                           }
                    connection.close();
                   }                   
           }
           catch (Exception e) {
                   e.printStackTrace();
           }
       }
   }
   ```

## <a name="run-hello-code"></a><span data-ttu-id="46efa-140">Spuštění kódu hello</span><span class="sxs-lookup"><span data-stu-id="46efa-140">Run hello code</span></span>

1. <span data-ttu-id="46efa-141">Hello příkazového řádku spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="46efa-141">At hello command prompt, run hello following commands:</span></span>

   ```bash
   mvn package
   mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

2. <span data-ttu-id="46efa-142">Ověřte, že horních řádků 20 hello se vrátí a pak zavřete okno aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="46efa-142">Verify that hello top 20 rows are returned and then close hello application window.</span></span>


## <a name="next-steps"></a><span data-ttu-id="46efa-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="46efa-143">Next steps</span></span>
- [<span data-ttu-id="46efa-144">Návrh první databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="46efa-144">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="46efa-145">Ovladač Microsoft JDBC pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="46efa-145">Microsoft JDBC Driver for SQL Server</span></span>](https://github.com/microsoft/mssql-jdbc)
- [<span data-ttu-id="46efa-146">Hlášení problémů/kladení dotazů</span><span class="sxs-lookup"><span data-stu-id="46efa-146">Report issues/ask questions</span></span>](https://github.com/microsoft/mssql-jdbc/issues)

