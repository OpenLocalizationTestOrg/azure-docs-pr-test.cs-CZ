---
title: "Připojení k Azure Database for PostgreSQL pomocí jazyka Java| Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete vzorek kódu Java, který můžete použít k připojení a dotazování dat ze služby Azure Database for PostgreSQL."
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
ms.openlocfilehash: 730a3f464b4437c260d09abc026a186a0e26293c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-database-for-postgresql-use-java-to-connect-and-query-data"></a><span data-ttu-id="a05ef-103">Azure Database for PostgreSQL: Použití jazyka Java k připojení a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="a05ef-103">Azure Database for PostgreSQL: Use Java to connect and query data</span></span>
<span data-ttu-id="a05ef-104">Tento rychlý start ukazuje, jak se připojit ke službě Azure Database for PostgreSQL pomocí aplikace jazyka Java.</span><span class="sxs-lookup"><span data-stu-id="a05ef-104">This quickstart demonstrates how to connect to an Azure Database for PostgreSQL using a Java application.</span></span> <span data-ttu-id="a05ef-105">Ukazuje, jak pomocí příkazů jazyka SQL dotazovat, vkládat, aktualizovat a odstraňovat data v databázi.</span><span class="sxs-lookup"><span data-stu-id="a05ef-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="a05ef-106">Kroky v tomto článku předpokládají, že máte zkušenosti s vývojem pomocí jazyka Java a teprve začínáte pracovat se službou Azure Database for PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="a05ef-106">The steps in this article assume that you are familiar with developing using Java, and that you are new to working with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a05ef-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a05ef-107">Prerequisites</span></span>
<span data-ttu-id="a05ef-108">Tento rychlý start využívá jako výchozí bod prostředky vytvořené v některém z těchto průvodců:</span><span class="sxs-lookup"><span data-stu-id="a05ef-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="a05ef-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="a05ef-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="a05ef-110">Vytvoření databáze – rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="a05ef-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="a05ef-111">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="a05ef-111">You also need to:</span></span>
- <span data-ttu-id="a05ef-112">Stáhnout [ovladač JDBC pro PostgreSQL](https://jdbc.postgresql.org/download.html) odpovídající verzi jazyka Java a sadě Java Development Kit</span><span class="sxs-lookup"><span data-stu-id="a05ef-112">Download the [PostgreSQL JDBC Driver](https://jdbc.postgresql.org/download.html) matching your version of Java and the Java Development Kit.</span></span>
- <span data-ttu-id="a05ef-113">Zahrnout soubor .jar JDBC pro PostgreSQL (například postgresql-42.1.1.jar) do cesty pro třídu aplikace</span><span class="sxs-lookup"><span data-stu-id="a05ef-113">Include the PostgreSQL JDBC jar file (for example postgresql-42.1.1.jar) in your application classpath.</span></span> <span data-ttu-id="a05ef-114">Další informace najdete v [podrobnostech cesty pro třídu](https://jdbc.postgresql.org/documentation/head/classpath.html).</span><span class="sxs-lookup"><span data-stu-id="a05ef-114">For more information, see [classpath details](https://jdbc.postgresql.org/documentation/head/classpath.html).</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="a05ef-115">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="a05ef-115">Get connection information</span></span>
<span data-ttu-id="a05ef-116">Získejte informace o připojení potřebné pro připojení ke službě Azure Database for PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="a05ef-116">Get the connection information needed to connect to the Azure Database for PostgreSQL.</span></span> <span data-ttu-id="a05ef-117">Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="a05ef-117">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="a05ef-118">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a05ef-118">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="a05ef-119">V nabídce vlevo na webu Azure Portal klikněte na **Všechny prostředky** a vyhledejte vytvořený server, například **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="a05ef-119">From the left-hand menu in Azure portal, click **All resources** and search for the server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="a05ef-120">Klikněte na název serveru **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="a05ef-120">Click the server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="a05ef-121">Vyberte stránku **Přehled** serveru.</span><span class="sxs-lookup"><span data-stu-id="a05ef-121">Select the server's **Overview** page.</span></span> <span data-ttu-id="a05ef-122">Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="a05ef-122">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="a05ef-123">![Azure Database for PostgreSQL – přihlašovací jméno správce serveru](./media/connect-java/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="a05ef-123">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-java/1-connection-string.png)</span></span>
5. <span data-ttu-id="a05ef-124">Pokud zapomenete přihlašovací údaje k serveru, přejděte na stránku **Přehled**, kde můžete zobrazit přihlašovací jméno správce serveru a v případě potřeby resetovat heslo.</span><span class="sxs-lookup"><span data-stu-id="a05ef-124">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="a05ef-125">Připojení, vytvoření tabulky a vložení dat</span><span class="sxs-lookup"><span data-stu-id="a05ef-125">Connect, create table, and insert data</span></span>
<span data-ttu-id="a05ef-126">Pomocí následujícího kódu se připojte a načtěte data s využitím funkce s příkazem **INSERT** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="a05ef-126">Use the following code to connect and load the data using the function with an **INSERT** SQL statement.</span></span> <span data-ttu-id="a05ef-127">Metody [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html) a [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) slouží k připojení, odpojení a vytvoření tabulky.</span><span class="sxs-lookup"><span data-stu-id="a05ef-127">The methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) are used to connect, drop, and create the table.</span></span> <span data-ttu-id="a05ef-128">Objekt [prepareStatement](https://jdbc.postgresql.org/documentation/head/query.html) slouží k sestavení příkazů INSERT a metody setString() a setInt() k vázání hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="a05ef-128">The [prepareStatement](https://jdbc.postgresql.org/documentation/head/query.html) object is used to build the insert commands, with setString() and setInt() to bind the parameter values.</span></span> <span data-ttu-id="a05ef-129">Metoda [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) spouští příkaz pro každou sadu parametrů.</span><span class="sxs-lookup"><span data-stu-id="a05ef-129">Method [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) runs the command for each set of parameters.</span></span> 

<span data-ttu-id="a05ef-130">Nahraďte parametry host (hostitel), database (databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="a05ef-130">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

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

        // check that the driver is installed
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
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
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
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
    
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="read-data"></a><span data-ttu-id="a05ef-131">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="a05ef-131">Read data</span></span>
<span data-ttu-id="a05ef-132">Pomocí následujícího kódu načtěte data s využitím příkazu **SELECT** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="a05ef-132">Use the following code to read the data with a **SELECT** SQL statement.</span></span> <span data-ttu-id="a05ef-133">Metody [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html) a [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) slouží k připojení, vytvoření a spuštění příkazu SELECT.</span><span class="sxs-lookup"><span data-stu-id="a05ef-133">The methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) are used to connect, create, and run the select statement.</span></span> <span data-ttu-id="a05ef-134">Výsledky se zpracovávají pomocí objektu [ResultSet](https://www.postgresql.org/docs/7.4/static/jdbc-query.html).</span><span class="sxs-lookup"><span data-stu-id="a05ef-134">The results are processed using a [ResultSet](https://www.postgresql.org/docs/7.4/static/jdbc-query.html) object.</span></span> 

<span data-ttu-id="a05ef-135">Nahraďte parametry host (hostitel), database (databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="a05ef-135">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

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

        // check that the driver is installed
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
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
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
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}

```

## <a name="update-data"></a><span data-ttu-id="a05ef-136">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="a05ef-136">Update data</span></span>
<span data-ttu-id="a05ef-137">Pomocí následujícího kódu změňte data s využitím příkazu **UPDATE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="a05ef-137">Use the following code to change the data with an **UPDATE** SQL statement.</span></span> <span data-ttu-id="a05ef-138">Metody [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html) a [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) slouží k připojení, přípravě a spuštění příkazu UPDATE.</span><span class="sxs-lookup"><span data-stu-id="a05ef-138">The methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) are used to connect, prepare, and run the update statement.</span></span> 

<span data-ttu-id="a05ef-139">Nahraďte parametry host (hostitel), database (databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="a05ef-139">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

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

        // check that the driver is installed
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
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Modify some data in table.
                int nRowsUpdated = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?;");
                preparedStatement.setInt(1, 200);
                preparedStatement.setString(2, "banana");
                nRowsUpdated += preparedStatement.executeUpdate();
                System.out.println(String.format("Updated %d row(s) of data.", nRowsUpdated));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```
## <a name="delete-data"></a><span data-ttu-id="a05ef-140">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="a05ef-140">Delete data</span></span>
<span data-ttu-id="a05ef-141">Pomocí následujícího kódu odstraňte data s využitím příkazu **DELETE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="a05ef-141">Use the following code to remove data with a **DELETE** SQL statement.</span></span> <span data-ttu-id="a05ef-142">Metody [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html) a [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) slouží k připojení, přípravě a spuštění příkazu DELETE.</span><span class="sxs-lookup"><span data-stu-id="a05ef-142">The methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) are used to connect, prepare, and run the delete statement.</span></span> 

<span data-ttu-id="a05ef-143">Nahraďte parametry host (hostitel), database (databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="a05ef-143">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

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

        // check that the driver is installed
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
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Delete some data from table.
                int nRowsDeleted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("DELETE FROM inventory WHERE name = ?;");
                preparedStatement.setString(1, "orange");
                nRowsDeleted += preparedStatement.executeUpdate();
                System.out.println(String.format("Deleted %d row(s) of data.", nRowsDeleted));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="a05ef-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a05ef-144">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="a05ef-145">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="a05ef-145">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
