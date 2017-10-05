---
title: "Použití Javy k dotazování služby Azure SQL Database | Dokumentace Microsoftu"
description: "Toto téma vám ukáže, jak pomocí Javy vytvořit program, který se připojí ke službě Azure SQL Database a bude ji dotazovat s použitím příkazů jazyka Transact-SQL."
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
ms.openlocfilehash: 103b0755ab89a13297cfdc9ec72416664da8c1e9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-java-to-query-an-azure-sql-database"></a><span data-ttu-id="db7db-103">Použití Javy k dotazování databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="db7db-103">Use Java to query an Azure SQL database</span></span>

<span data-ttu-id="db7db-104">Tento rychlý start ukazuje použití [Javy](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) pro připojení k databázi SQL Azure a následné použití příkazů jazyka Transact-SQL k dotazování dat.</span><span class="sxs-lookup"><span data-stu-id="db7db-104">This quick start demonstrates how to use [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) to connect to an Azure SQL database and then use Transact-SQL statements to query data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db7db-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="db7db-105">Prerequisites</span></span>

<span data-ttu-id="db7db-106">Abyste mohli absolvovat tento rychlý úvodní kurz, ujistěte se, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="db7db-106">To complete this quick start tutorial, make sure you have the following prerequisites:</span></span>

- <span data-ttu-id="db7db-107">Databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="db7db-107">An Azure SQL database.</span></span> <span data-ttu-id="db7db-108">Tento rychlý start používá prostředky vytvořené v některém z těchto rychlých startů:</span><span class="sxs-lookup"><span data-stu-id="db7db-108">This quick start uses the resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="db7db-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="db7db-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="db7db-110">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="db7db-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="db7db-111">Vytvoření databáze – PowerShell</span><span class="sxs-lookup"><span data-stu-id="db7db-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="db7db-112">[Pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro veřejnou IP adresu počítače, který používáte pro tento rychlý úvodní kurz.</span><span class="sxs-lookup"><span data-stu-id="db7db-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for the public IP address of the computer you use for this quick start tutorial.</span></span>

- <span data-ttu-id="db7db-113">Máte nainstalovanou Javu a související software pro váš operační systém.</span><span class="sxs-lookup"><span data-stu-id="db7db-113">You have installed Java and related software for your operating system.</span></span>

    - <span data-ttu-id="db7db-114">**MacOS:** Nainstalujte Homebrew a Javu a potom nainstalujte Maven.</span><span class="sxs-lookup"><span data-stu-id="db7db-114">**MacOS**: Install Homebrew and Java, and then install Maven.</span></span> <span data-ttu-id="db7db-115">Viz [kroky 1.2 a 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).</span><span class="sxs-lookup"><span data-stu-id="db7db-115">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).</span></span>
    - <span data-ttu-id="db7db-116">**Ubuntu:** Nainstalujte Java Development Kit a Maven.</span><span class="sxs-lookup"><span data-stu-id="db7db-116">**Ubuntu**:  Install the Java Development Kit, and install Maven.</span></span> <span data-ttu-id="db7db-117">Viz [kroky 1.2, 1.3 a 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="db7db-117">See [Step 1.2, 1.3, and 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).</span></span>
    - <span data-ttu-id="db7db-118">**Windows:** Nainstalujte Java Development Kit a Maven.</span><span class="sxs-lookup"><span data-stu-id="db7db-118">**Windows**: Install the Java Development Kit, and Maven.</span></span> <span data-ttu-id="db7db-119">Viz [kroky 1.2 a 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).</span><span class="sxs-lookup"><span data-stu-id="db7db-119">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).</span></span>    

## <a name="sql-server-connection-information"></a><span data-ttu-id="db7db-120">Informace o připojení k SQL serveru</span><span class="sxs-lookup"><span data-stu-id="db7db-120">SQL server connection information</span></span>

<span data-ttu-id="db7db-121">Získejte informace o připojení potřebné pro připojení k databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="db7db-121">Get the connection information needed to connect to the Azure SQL database.</span></span> <span data-ttu-id="db7db-122">V dalších postupech budete potřebovat plně kvalifikovaný název serveru, název databáze a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="db7db-122">You will need the fully qualified server name, database name, and login information in the next procedures.</span></span>

1. <span data-ttu-id="db7db-123">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="db7db-123">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="db7db-124">V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="db7db-124">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="db7db-125">Na stránce **Přehled** pro vaši databázi si prohlédněte plně kvalifikovaný název serveru, jak je znázorněno na následujícím obrázku: Pokud na název serveru najedete myší, můžete vyvolat možnost **Kopírovat kliknutím**.</span><span class="sxs-lookup"><span data-stu-id="db7db-125">On the **Overview** page for your database, review the fully qualified server name as shown in the following image: You can hover over the server name to bring up the **Click to copy** option.</span></span>  

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="db7db-127">Pokud zapomenete přihlašovací informace pro váš server, přejděte na stránku serveru služby SQL Database, abyste zobrazili jméno správce serveru.</span><span class="sxs-lookup"><span data-stu-id="db7db-127">If you forget your server login information, navigate to the SQL Database server page to view the server admin name.</span></span>  <span data-ttu-id="db7db-128">V případě potřeby obnovte heslo.</span><span class="sxs-lookup"><span data-stu-id="db7db-128">If necessary, reset the password.</span></span>     

## <a name="create-maven-project-and-dependencies"></a><span data-ttu-id="db7db-129">**Vytvoření projektu a závislostí v Mavenu**</span><span class="sxs-lookup"><span data-stu-id="db7db-129">**Create Maven project and dependencies**</span></span>
1. <span data-ttu-id="db7db-130">Na terminálu vytvořte nový projekt v Mavenu s názvem **sqltest**.</span><span class="sxs-lookup"><span data-stu-id="db7db-130">From the terminal, create a new Maven project called **sqltest**.</span></span> 

   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```

2. <span data-ttu-id="db7db-131">Po zobrazení výzvy zadejte **Y**.</span><span class="sxs-lookup"><span data-stu-id="db7db-131">Enter **Y** when prompted.</span></span>
3. <span data-ttu-id="db7db-132">Změňte adresář na **sqltest** a v oblíbeném textovém editoru otevřete soubor ***pom.xml***.</span><span class="sxs-lookup"><span data-stu-id="db7db-132">Change directory to **sqltest** and open ***pom.xml*** with your favorite text editor.</span></span>  <span data-ttu-id="db7db-133">Pomocí následujícího kódu přidejte k závislostem projektu **Ovladač Microsoft JDBC pro SQL Server**:</span><span class="sxs-lookup"><span data-stu-id="db7db-133">Add the **Microsoft JDBC Driver for SQL Server** to your project's dependencies using the following code:</span></span>

   ```xml
   <dependency>
       <groupId>com.microsoft.sqlserver</groupId>
       <artifactId>mssql-jdbc</artifactId>
       <version>6.2.1.jre8</version>
   </dependency>
   ```

4. <span data-ttu-id="db7db-134">Také do souboru ***pom.xml*** přidejte k projektu následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="db7db-134">Also in ***pom.xml***, add the following properties to your project.</span></span>  <span data-ttu-id="db7db-135">Pokud nemáte část s vlastnostmi, můžete ji přidat po závislostech.</span><span class="sxs-lookup"><span data-stu-id="db7db-135">If you don't have a properties section, you can add it after the dependencies.</span></span>

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

5. <span data-ttu-id="db7db-136">Soubor ***pom.xml*** uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="db7db-136">Save and close ***pom.xml***.</span></span>

## <a name="insert-code-to-query-sql-database"></a><span data-ttu-id="db7db-137">Vložení kódu pro dotazování databáze SQL</span><span class="sxs-lookup"><span data-stu-id="db7db-137">Insert code to query SQL database</span></span>

1. <span data-ttu-id="db7db-138">V projektu v Mavenu už byste měli mít soubor ***App.java*** umístěný v: ..\sqltest\src\main\java\com\sqlsamples\App.java</span><span class="sxs-lookup"><span data-stu-id="db7db-138">You should already have a file called ***App.java*** in your Maven project located at:  ..\sqltest\src\main\java\com\sqlsamples\App.java</span></span>

2. <span data-ttu-id="db7db-139">Otevřete tento soubor, jeho obsah nahraďte následujícím kódem a přidejte odpovídající hodnoty pro váš server, databázi, uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="db7db-139">Open the file and replace its contents with the following code and add the appropriate values for your server, database, user, and password.</span></span>

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.DriverManager;

   public class App {

    public static void main(String[] args) {
    
        // Connect to database
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

## <a name="run-the-code"></a><span data-ttu-id="db7db-140">Spuštění kódu</span><span class="sxs-lookup"><span data-stu-id="db7db-140">Run the code</span></span>

1. <span data-ttu-id="db7db-141">V příkazovém řádku spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="db7db-141">At the command prompt, run the following commands:</span></span>

   ```bash
   mvn package
   mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

2. <span data-ttu-id="db7db-142">Ověřte, že se vrátilo prvních 20 řádků, a potom zavřete okno aplikace.</span><span class="sxs-lookup"><span data-stu-id="db7db-142">Verify that the top 20 rows are returned and then close the application window.</span></span>


## <a name="next-steps"></a><span data-ttu-id="db7db-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db7db-143">Next steps</span></span>
- [<span data-ttu-id="db7db-144">Návrh první databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="db7db-144">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="db7db-145">Ovladač Microsoft JDBC pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="db7db-145">Microsoft JDBC Driver for SQL Server</span></span>](https://github.com/microsoft/mssql-jdbc)
- [<span data-ttu-id="db7db-146">Hlášení problémů/kladení dotazů</span><span class="sxs-lookup"><span data-stu-id="db7db-146">Report issues/ask questions</span></span>](https://github.com/microsoft/mssql-jdbc/issues)

