---
title: "Připojení tooAzure databáze pro PostgreSQL pomocí Java | Microsoft Docs"
description: "Tento rychlý start poskytuje ukázka kódu Java můžete použít tooconnect a zadávat dotazy na data z databáze Azure pro PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: java
ms.topic: quickstart
ms.date: 06/23/2017
ms.openlocfilehash: 8f6e0a47a0d6dfebf29eb56c31ccccabd7c2b670
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-java-tooconnect-and-query-data"></a><span data-ttu-id="f03cf-103">Azure databázi PostgreSQL: použití Java tooconnect a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="f03cf-103">Azure Database for PostgreSQL: Use Java tooconnect and query data</span></span>
<span data-ttu-id="f03cf-104">Tento rychlý start předvádí jak tooconnect tooan Azure databázi PostgreSQL pomocí aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="f03cf-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using a Java application.</span></span> <span data-ttu-id="f03cf-105">Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="f03cf-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="f03cf-106">Hello postup v tomto článku předpokládá, že jste obeznámeni s vývojem pomocí Java, a že jste novou tooworking s Azure databáze PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="f03cf-106">hello steps in this article assume that you are familiar with developing using Java, and that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f03cf-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f03cf-107">Prerequisites</span></span>
<span data-ttu-id="f03cf-108">Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:</span><span class="sxs-lookup"><span data-stu-id="f03cf-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="f03cf-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="f03cf-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="f03cf-110">Vytvoření databáze – rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="f03cf-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="f03cf-111">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="f03cf-111">You also need to:</span></span>
- <span data-ttu-id="f03cf-112">Stáhnout hello [ovladač JDBC PostgreSQL](https://jdbc.postgresql.org/download.html) odpovídající verzi aplikace Java a hello Java Development Kit.</span><span class="sxs-lookup"><span data-stu-id="f03cf-112">Download hello [PostgreSQL JDBC Driver](https://jdbc.postgresql.org/download.html) matching your version of Java and hello Java Development Kit.</span></span>
- <span data-ttu-id="f03cf-113">Zahrňte hello soubor jar PostgreSQL JDBC (například postgresql-42.1.1.jar) do cesty pro třídy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f03cf-113">Include hello PostgreSQL JDBC jar file (for example postgresql-42.1.1.jar) in your application classpath.</span></span> <span data-ttu-id="f03cf-114">Další informace najdete v [podrobnostech cesty pro třídu](https://jdbc.postgresql.org/documentation/head/classpath.html).</span><span class="sxs-lookup"><span data-stu-id="f03cf-114">For more information, see [classpath details](https://jdbc.postgresql.org/documentation/head/classpath.html).</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="f03cf-115">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="f03cf-115">Get connection information</span></span>
<span data-ttu-id="f03cf-116">Získáte hello připojení informace potřebné tooconnect toohello databáze Azure pro PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="f03cf-116">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="f03cf-117">Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="f03cf-117">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="f03cf-118">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f03cf-118">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="f03cf-119">Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru, které jste vytvořili, například **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="f03cf-119">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="f03cf-120">Klikněte na název serveru hello **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="f03cf-120">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="f03cf-121">Vyberte hello serveru **přehled** stránky.</span><span class="sxs-lookup"><span data-stu-id="f03cf-121">Select hello server's **Overview** page.</span></span> <span data-ttu-id="f03cf-122">Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.</span><span class="sxs-lookup"><span data-stu-id="f03cf-122">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="f03cf-123">![Azure Database for PostgreSQL – přihlášení správce serveru](./media/connect-java/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="f03cf-123">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-java/1-connection-string.png)</span></span>
5. <span data-ttu-id="f03cf-124">Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="f03cf-124">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="f03cf-125">Připojení, vytvoření tabulky a vložení dat</span><span class="sxs-lookup"><span data-stu-id="f03cf-125">Connect, create table, and insert data</span></span>
<span data-ttu-id="f03cf-126">Použití hello následující kód tooconnect a zatížení hello dat pomocí funkce hello **vložit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="f03cf-126">Use hello following code tooconnect and load hello data using hello function with an **INSERT** SQL statement.</span></span> <span data-ttu-id="f03cf-127">Hello metody [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), a [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) jsou použité tooconnect, vyřaďte a vytvořte tabulku hello.</span><span class="sxs-lookup"><span data-stu-id="f03cf-127">hello methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) are used tooconnect, drop, and create hello table.</span></span> <span data-ttu-id="f03cf-128">Hello [prepareStatement](https://jdbc.postgresql.org/documentation/head/query.html) objekt je příkazy insert hello toobuild použité, s setString() a setInt() hodnotami parametrů toobind hello.</span><span class="sxs-lookup"><span data-stu-id="f03cf-128">hello [prepareStatement](https://jdbc.postgresql.org/documentation/head/query.html) object is used toobuild hello insert commands, with setString() and setInt() toobind hello parameter values.</span></span> <span data-ttu-id="f03cf-129">Metoda [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) hello spustí příkaz pro každou sadu parametrů.</span><span class="sxs-lookup"><span data-stu-id="f03cf-129">Method [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) runs hello command for each set of parameters.</span></span> 

<span data-ttu-id="f03cf-130">Nahraďte hello hodnoty, které jste zadali při vytváření vlastní server a databáze hello hostitele, databáze, uživatele a heslo parametry.</span><span class="sxs-lookup"><span data-stu-id="f03cf-130">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class CreateTableInsertRows {

    public static void main (String[] args)  throws Exception
    {

        // Initialize connection variables.
        String host = "mypgserver-20170401.postgres.database.azure.com";
        String database = "mypgsqldb";
        String user = "mylogin@mypgserver-20170401";
        String password = "<server_admin_password>";

        // check that hello driver is installed
        try
        {
            Class.forName("org.postgresql.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("PostgreSQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("PostgreSQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:postgresql://%s/%s", host, database);
            
            // set up hello connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed toocreate connection toodatabase.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection toodatabase.");
        
            // Perform some SQL queries over hello connection.
            try
            {
                // Drop previous table of same name if one exists.
                Statement statement = connection.createStatement();
                statement.execute("DROP TABLE IF EXISTS inventory;");
                System.out.println("Finished dropping table (if existed).");
    
                // Create table.
                statement.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
                System.out.println("Created table.");
    
                // Insert some data into table.
                int nRowsInserted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO inventory (name, quantity) VALUES (?, ?);");
                preparedStatement.setString(1, "banana");
                preparedStatement.setInt(2, 150);
                nRowsInserted += preparedStatement.executeUpdate();

                preparedStatement.setString(1, "orange");
                preparedStatement.setInt(2, 154);
                nRowsInserted += preparedStatement.executeUpdate();

                preparedStatement.setString(1, "apple");
                preparedStatement.setInt(2, 100);
                nRowsInserted += preparedStatement.executeUpdate();
                System.out.println(String.format("Inserted %d row(s) of data.", nRowsInserted));
    
                // NOTE No need toocommit all changes toodatabase, as auto-commit is enabled by default.
    
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed toocreate connection toodatabase.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="read-data"></a><span data-ttu-id="f03cf-131">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="f03cf-131">Read data</span></span>
<span data-ttu-id="f03cf-132">Použití hello následující kód tooread hello dat pomocí **vyberte** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="f03cf-132">Use hello following code tooread hello data with a **SELECT** SQL statement.</span></span> <span data-ttu-id="f03cf-133">Hello metody [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), a [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) jsou použité tooconnect, vytvořte a spusťte příkaz select hello.</span><span class="sxs-lookup"><span data-stu-id="f03cf-133">hello methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) are used tooconnect, create, and run hello select statement.</span></span> <span data-ttu-id="f03cf-134">výsledky Hello se zpracovávají pomocí [třídy ResultSet](https://www.postgresql.org/docs/7.4/static/jdbc-query.html) objektu.</span><span class="sxs-lookup"><span data-stu-id="f03cf-134">hello results are processed using a [ResultSet](https://www.postgresql.org/docs/7.4/static/jdbc-query.html) object.</span></span> 

<span data-ttu-id="f03cf-135">Nahraďte hello hodnoty, které jste zadali při vytváření vlastní server a databáze hello hostitele, databáze, uživatele a heslo parametry.</span><span class="sxs-lookup"><span data-stu-id="f03cf-135">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class ReadTable {

    public static void main (String[] args)  throws Exception
    {

        // Initialize connection variables.
        String host = "mypgserver-20170401.postgres.database.azure.com";
        String database = "mypgsqldb";
        String user = "mylogin@mypgserver-20170401";
        String password = "<server_admin_password>";

        // check that hello driver is installed
        try
        {
            Class.forName("org.postgresql.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("PostgreSQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("PostgreSQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:postgresql://%s/%s", host, database);
            
            // set up hello connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed toocreate connection toodatabase.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection toodatabase.");
        
            // Perform some SQL queries over hello connection.
            try
            {
    
                Statement statement = connection.createStatement();
                ResultSet results = statement.executeQuery("SELECT * from inventory;");
                while (results.next())
                {
                    String outputString = 
                        String.format(
                            "Data row = (%s, %s, %s)",
                            results.getString(1),
                            results.getString(2),
                            results.getString(3));
                    System.out.println(outputString);
                }
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed toocreate connection toodatabase.");
        }
        System.out.println("Execution finished.");
    }
}

```

## <a name="update-data"></a><span data-ttu-id="f03cf-136">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="f03cf-136">Update data</span></span>
<span data-ttu-id="f03cf-137">Použití hello následující kód toochange hello dat pomocí **aktualizace** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="f03cf-137">Use hello following code toochange hello data with an **UPDATE** SQL statement.</span></span> <span data-ttu-id="f03cf-138">Hello metody [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), a [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) jsou použité tooconnect, Příprava, a spusťte příkaz update hello.</span><span class="sxs-lookup"><span data-stu-id="f03cf-138">hello methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) are used tooconnect, prepare, and run hello update statement.</span></span> 

<span data-ttu-id="f03cf-139">Nahraďte hello hodnoty, které jste zadali při vytváření vlastní server a databáze hello hostitele, databáze, uživatele a heslo parametry.</span><span class="sxs-lookup"><span data-stu-id="f03cf-139">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class UpdateTable {
    public static void main (String[] args)  throws Exception
    {

        // Initialize connection variables.
        String host = "mypgserver-20170401.postgres.database.azure.com";
        String database = "mypgsqldb";
        String user = "mylogin@mypgserver-20170401";
        String password = "<server_admin_password>";

        // check that hello driver is installed
        try
        {
            Class.forName("org.postgresql.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("PostgreSQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("PostgreSQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:postgresql://%s/%s", host, database);
            
            // set up hello connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed toocreate connection toodatabase.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection toodatabase.");
        
            // Perform some SQL queries over hello connection.
            try
            {
                // Modify some data in table.
                int nRowsUpdated = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?;");
                preparedStatement.setInt(1, 200);
                preparedStatement.setString(2, "banana");
                nRowsUpdated += preparedStatement.executeUpdate();
                System.out.println(String.format("Updated %d row(s) of data.", nRowsUpdated));
    
                // NOTE No need toocommit all changes toodatabase, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed toocreate connection toodatabase.");
        }
        System.out.println("Execution finished.");
    }
}
```
## <a name="delete-data"></a><span data-ttu-id="f03cf-140">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="f03cf-140">Delete data</span></span>
<span data-ttu-id="f03cf-141">Použití hello následující kód tooremove dat pomocí **odstranit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="f03cf-141">Use hello following code tooremove data with a **DELETE** SQL statement.</span></span> <span data-ttu-id="f03cf-142">Hello metody [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), a [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) jsou použité tooconnect, Příprava, a spusťte příkaz delete hello.</span><span class="sxs-lookup"><span data-stu-id="f03cf-142">hello methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) are used tooconnect, prepare, and run hello delete statement.</span></span> 

<span data-ttu-id="f03cf-143">Nahraďte hello hodnoty, které jste zadali při vytváření vlastní server a databáze hello hostitele, databáze, uživatele a heslo parametry.</span><span class="sxs-lookup"><span data-stu-id="f03cf-143">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class DeleteTable {
    public static void main (String[] args)  throws Exception
    {

        // Initialize connection variables.
        String host = "mypgserver-20170401.postgres.database.azure.com";
        String database = "mypgsqldb";
        String user = "mylogin@mypgserver-20170401";
        String password = "<server_admin_password>";

        // check that hello driver is installed
        try
        {
            Class.forName("org.postgresql.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("PostgreSQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("PostgreSQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:postgresql://%s/%s", host, database);
            
            // set up hello connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed toocreate connection toodatabase.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection toodatabase.");
        
            // Perform some SQL queries over hello connection.
            try
            {
                // Delete some data from table.
                int nRowsDeleted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("DELETE FROM inventory WHERE name = ?;");
                preparedStatement.setString(1, "orange");
                nRowsDeleted += preparedStatement.executeUpdate();
                System.out.println(String.format("Deleted %d row(s) of data.", nRowsDeleted));
    
                // NOTE No need toocommit all changes toodatabase, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed toocreate connection toodatabase.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="f03cf-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f03cf-144">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="f03cf-145">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="f03cf-145">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
