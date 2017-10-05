---
title: "Připojení k Azure Database for MySQL pomocí Javy | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete vzorový kód Java, který můžete použít k připojení a dotazování dat z databáze Azure Database for MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.devlang: java
ms.date: 06/20/2017
ms.openlocfilehash: 6ffcf3b38a3d868dfa10ea2e2a9d097441387d4f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-database-for-mysql-use-java-to-connect-and-query-data"></a><span data-ttu-id="c84cb-103">Azure Database for MySQL: Připojení a dotazování dat pomocí Javy</span><span class="sxs-lookup"><span data-stu-id="c84cb-103">Azure Database for MySQL: Use Java to connect and query data</span></span>
<span data-ttu-id="c84cb-104">Tento rychlý start ukazuje, jak se připojit ke službě Azure Database for MySQL pomocí aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="c84cb-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using a Java application.</span></span> <span data-ttu-id="c84cb-105">Ukazuje, jak pomocí příkazů jazyka SQL dotazovat, vkládat, aktualizovat a odstraňovat data v databázi.</span><span class="sxs-lookup"><span data-stu-id="c84cb-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="c84cb-106">V krocích v tomto článku se předpokládá, že máte zkušenosti s vývojem pomocí Javy a teprve začínáte pracovat se službou Azure Database for MySQL.</span><span class="sxs-lookup"><span data-stu-id="c84cb-106">The steps in this article assume that you are familiar with developing using Java, and that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c84cb-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c84cb-107">Prerequisites</span></span>
<span data-ttu-id="c84cb-108">Tento rychlý start jako výchozí bod využívá prostředky vytvořené v některém z těchto průvodců:</span><span class="sxs-lookup"><span data-stu-id="c84cb-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="c84cb-109">Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c84cb-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="c84cb-110">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c84cb-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="c84cb-111">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="c84cb-111">You also need to:</span></span>
- <span data-ttu-id="c84cb-112">Stáhnout ovladač JDBC [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/)</span><span class="sxs-lookup"><span data-stu-id="c84cb-112">Download the JDBC driver [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/)</span></span>
- <span data-ttu-id="c84cb-113">Zahrnout soubor .jar s JDBC (například mysql-connector-java-5.1.42-bin.jar) do cesty k třídě vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c84cb-113">Include the JDBC jar file (for example mysql-connector-java-5.1.42-bin.jar) into your application classpath.</span></span> <span data-ttu-id="c84cb-114">Pokud s tím máte problémy, podívejte se na specifika cest ke třídám do dokumentace k vašemu prostředí, jako například [Apache Tomcat](https://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html) nebo [Java SE](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html).</span><span class="sxs-lookup"><span data-stu-id="c84cb-114">If you have trouble with this, please consult your environment's documentation for class path specifics, such as [Apache Tomcat](https://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html) or [Java SE](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html)</span></span>
- <span data-ttu-id="c84cb-115">Zajistit, že je zabezpečení připojení Azure Database for MySQL nakonfigurováno s otevřenou bránou firewall a nastavením SSL upraveným pro úspěšné připojení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c84cb-115">Ensure your Azure Database for MySQL connection security is configured with the firewall opened and SSL settings adjusted for your application to connect successfully.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="c84cb-116">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="c84cb-116">Get connection information</span></span>
<span data-ttu-id="c84cb-117">Získejte informace o připojení potřebné pro připojení ke službě Azure Database for MySQL.</span><span class="sxs-lookup"><span data-stu-id="c84cb-117">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="c84cb-118">Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="c84cb-118">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="c84cb-119">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c84cb-119">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="c84cb-120">V levém podokně klikněte na **Všechny prostředky** a potom vyhledejte server, který jste vytvořili (například **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="c84cb-120">In the left pane, click **All resources**, and then search for the server you have created (for example, **myserver4demo**).</span></span>
3. <span data-ttu-id="c84cb-121">Klikněte na název serveru.</span><span class="sxs-lookup"><span data-stu-id="c84cb-121">Click the server name.</span></span>
4. <span data-ttu-id="c84cb-122">Vyberte stránku **Vlastnosti** serveru.</span><span class="sxs-lookup"><span data-stu-id="c84cb-122">Select the server's **Properties** page.</span></span> <span data-ttu-id="c84cb-123">Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="c84cb-123">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="c84cb-124">![Název serveru Azure Database for MySQL](./media/connect-java/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="c84cb-124">![Azure Database for MySQL server name](./media/connect-java/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="c84cb-125">Pokud zapomenete přihlašovací údaje pro váš server, přejděte na stránku **Přehled**, kde můžete zobrazit přihlašovací jméno správce serveru a v případě potřeby obnovit heslo.</span><span class="sxs-lookup"><span data-stu-id="c84cb-125">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="c84cb-126">Připojení, vytvoření tabulky a vložení dat</span><span class="sxs-lookup"><span data-stu-id="c84cb-126">Connect, create table, and insert data</span></span>
<span data-ttu-id="c84cb-127">Pomocí následujícího kódu se připojte a načtěte data s využitím funkce s příkazem **INSERT** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="c84cb-127">Use the following code to connect and load the data using the function with an **INSERT** SQL statement.</span></span> <span data-ttu-id="c84cb-128">Metoda [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) slouží k připojení k MySQL.</span><span class="sxs-lookup"><span data-stu-id="c84cb-128">The [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used to connect to MySQL.</span></span> <span data-ttu-id="c84cb-129">Metody [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) a execute() slouží k odstranění a vytvoření tabulky.</span><span class="sxs-lookup"><span data-stu-id="c84cb-129">Methods [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) and execute() are used to drop and create the table.</span></span> <span data-ttu-id="c84cb-130">Objekt prepareStatement slouží k sestavení příkazů INSERT a metody setString() a setInt() k vázání hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="c84cb-130">The prepareStatement object is used to build the insert commands, with setString() and setInt() to bind the parameter values.</span></span> <span data-ttu-id="c84cb-131">Metoda executeUpdate() vkládá hodnoty spuštěním příkazu pro každou sadu parametrů.</span><span class="sxs-lookup"><span data-stu-id="c84cb-131">Method executeUpdate() runs the command for each set of parameters to insert the values.</span></span> 

<span data-ttu-id="c84cb-132">Nahraďte parametry host (hostitel), database (databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="c84cb-132">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class CreateTableInsertRows {

    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables. 
        String host = "myserver4demo.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@myserver4demo";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("com.mysql.jdbc.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MySQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MySQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mysql://%s/%s", host, database);

            // Set connection properties.
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");

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

## <a name="read-data"></a><span data-ttu-id="c84cb-133">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="c84cb-133">Read data</span></span>
<span data-ttu-id="c84cb-134">Pomocí následujícího kódu načtěte data s využitím příkazu **SELECT** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="c84cb-134">Use the following code to read the data with a **SELECT** SQL statement.</span></span> <span data-ttu-id="c84cb-135">Metoda [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) slouží k připojení k MySQL.</span><span class="sxs-lookup"><span data-stu-id="c84cb-135">The [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used to connect to MySQL.</span></span> <span data-ttu-id="c84cb-136">Metody [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) a executeQuery() slouží k připojení a spuštění příkazu SELECT.</span><span class="sxs-lookup"><span data-stu-id="c84cb-136">The methods [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) and executeQuery() are used to connect and run the select statement.</span></span> <span data-ttu-id="c84cb-137">Výsledky se zpracovávají pomocí objektu [ResultSet](https://docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html).</span><span class="sxs-lookup"><span data-stu-id="c84cb-137">The results are processed using a [ResultSet](https://docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html) object.</span></span> 

<span data-ttu-id="c84cb-138">Nahraďte parametry host (hostitel), database (databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="c84cb-138">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class ReadTable {

    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables.
        String host = "myserver4demo.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@myserver4demo";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("com.mysql.jdbc.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MySQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MySQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mysql://%s/%s", host, database);

            // Set connection properties.
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");
            
            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database", e);
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
                throw new SQLException("Encountered an error when executing given sql statement", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database."); 
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="update-data"></a><span data-ttu-id="c84cb-139">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="c84cb-139">Update data</span></span>
<span data-ttu-id="c84cb-140">Pomocí následujícího kódu změňte data s využitím příkazu **UPDATE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="c84cb-140">Use the following code to change the data with an **UPDATE** SQL statement.</span></span> <span data-ttu-id="c84cb-141">Metoda [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) slouží k připojení k MySQL.</span><span class="sxs-lookup"><span data-stu-id="c84cb-141">The [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used to connect to MySQL.</span></span> <span data-ttu-id="c84cb-142">Metody [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) a executeUpdate() slouží k příprav a spuštění příkazu UPDATE.</span><span class="sxs-lookup"><span data-stu-id="c84cb-142">The methods [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) and executeUpdate() are used to prepare and run the update statement.</span></span> 

<span data-ttu-id="c84cb-143">Nahraďte parametry host (hostitel), database (databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="c84cb-143">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class UpdateTable {
    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables. 
        String host = "myserver4demo.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@myserver4demo";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("com.mysql.jdbc.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MySQL JDBC driver NOT detected in library path.", e);
        }
        System.out.println("MySQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mysql://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");

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

## <a name="delete-data"></a><span data-ttu-id="c84cb-144">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="c84cb-144">Delete data</span></span>
<span data-ttu-id="c84cb-145">Pomocí následujícího kódu odstraňte data s využitím příkazu **DELETE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="c84cb-145">Use the following code to remove data with a **DELETE** SQL statement.</span></span> <span data-ttu-id="c84cb-146">Metoda [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) slouží k připojení k MySQL.</span><span class="sxs-lookup"><span data-stu-id="c84cb-146">The [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used to connect to MySQL.</span></span>  <span data-ttu-id="c84cb-147">Metody [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) a executeUpdate() slouží k příprav a spuštění příkazu UPDATE.</span><span class="sxs-lookup"><span data-stu-id="c84cb-147">The methods [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) and executeUpdate() are used to prepare and run the update statement.</span></span> 

<span data-ttu-id="c84cb-148">Nahraďte parametry host (hostitel), database (databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="c84cb-148">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class DeleteTable {
    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables.
        String host = "myserver4demo.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@myserver4demo";
        String password = "<server_admin_password>";
        
        // check that the driver is installed
        try
        {
            Class.forName("com.mysql.jdbc.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MySQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MySQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mysql://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");
            
            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database", e);
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

## <a name="next-steps"></a><span data-ttu-id="c84cb-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c84cb-149">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="c84cb-150">Migrace databáze MySQL do služby Azure Database for MySQL pomocí výpisu a obnovení.</span><span class="sxs-lookup"><span data-stu-id="c84cb-150">Migrate your MySQL database to Azure Database for MySQL using dump and restore</span></span>](concepts-migrate-dump-restore.md)
