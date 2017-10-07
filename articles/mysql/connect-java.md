---
title: "Připojit tooAzure databáze pro databázi MySQL pomocí Java | Microsoft Docs"
description: "Tento rychlý start poskytuje ukázka kódu Java můžete použít tooconnect a zadávat dotazy na data z databáze Azure pro databázi MySQL."
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
ms.openlocfilehash: d584b5491d29700b36fae26800c59d93ceb3e265
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-java-tooconnect-and-query-data"></a><span data-ttu-id="63cab-103">Azure databáze pro databázi MySQL: použití Java tooconnect a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="63cab-103">Azure Database for MySQL: Use Java tooconnect and query data</span></span>
<span data-ttu-id="63cab-104">Tento rychlý start předvádí jak tooconnect tooan Azure databáze pro databázi MySQL pomocí aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="63cab-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using a Java application.</span></span> <span data-ttu-id="63cab-105">Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="63cab-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="63cab-106">Hello postup v tomto článku předpokládá, že jste obeznámeni s vývojem pomocí Java, a že jste novou tooworking s Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="63cab-106">hello steps in this article assume that you are familiar with developing using Java, and that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63cab-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="63cab-107">Prerequisites</span></span>
<span data-ttu-id="63cab-108">Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:</span><span class="sxs-lookup"><span data-stu-id="63cab-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="63cab-109">Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="63cab-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="63cab-110">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="63cab-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="63cab-111">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="63cab-111">You also need to:</span></span>
- <span data-ttu-id="63cab-112">Stáhnout ovladač JDBC hello [MySQL konektor/J](https://dev.mysql.com/downloads/connector/j/)</span><span class="sxs-lookup"><span data-stu-id="63cab-112">Download hello JDBC driver [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/)</span></span>
- <span data-ttu-id="63cab-113">Zahrňte soubor jar JDBC hello (například mysql konektor java-5.1.42-bin.jar) do cesty pro třídy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="63cab-113">Include hello JDBC jar file (for example mysql-connector-java-5.1.42-bin.jar) into your application classpath.</span></span> <span data-ttu-id="63cab-114">Pokud s tím máte problémy, podívejte se na specifika cest ke třídám do dokumentace k vašemu prostředí, jako například [Apache Tomcat](https://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html) nebo [Java SE](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html).</span><span class="sxs-lookup"><span data-stu-id="63cab-114">If you have trouble with this, please consult your environment's documentation for class path specifics, such as [Apache Tomcat](https://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html) or [Java SE](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html)</span></span>
- <span data-ttu-id="63cab-115">Ujistěte se, že je databáze Azure pro zabezpečení připojení MySQL nakonfigurovaná s branou firewall hello otevřít a upravit nastavení SSL pro vaše aplikace tooconnect úspěšně.</span><span class="sxs-lookup"><span data-stu-id="63cab-115">Ensure your Azure Database for MySQL connection security is configured with hello firewall opened and SSL settings adjusted for your application tooconnect successfully.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="63cab-116">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="63cab-116">Get connection information</span></span>
<span data-ttu-id="63cab-117">Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="63cab-117">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="63cab-118">Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="63cab-118">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="63cab-119">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="63cab-119">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="63cab-120">V levém podokně hello, klikněte na **všechny prostředky**a poté vyhledejte hello serveru, které jste vytvořili (například **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="63cab-120">In hello left pane, click **All resources**, and then search for hello server you have created (for example, **myserver4demo**).</span></span>
3. <span data-ttu-id="63cab-121">Klikněte na název serveru hello.</span><span class="sxs-lookup"><span data-stu-id="63cab-121">Click hello server name.</span></span>
4. <span data-ttu-id="63cab-122">Vyberte hello serveru **vlastnosti** stránky.</span><span class="sxs-lookup"><span data-stu-id="63cab-122">Select hello server's **Properties** page.</span></span> <span data-ttu-id="63cab-123">Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.</span><span class="sxs-lookup"><span data-stu-id="63cab-123">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="63cab-124">![Název serveru Azure Database for MySQL](./media/connect-java/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="63cab-124">![Azure Database for MySQL server name](./media/connect-java/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="63cab-125">Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="63cab-125">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="63cab-126">Připojení, vytvoření tabulky a vložení dat</span><span class="sxs-lookup"><span data-stu-id="63cab-126">Connect, create table, and insert data</span></span>
<span data-ttu-id="63cab-127">Použití hello následující kód tooconnect a zatížení hello dat pomocí funkce hello **vložit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="63cab-127">Use hello following code tooconnect and load hello data using hello function with an **INSERT** SQL statement.</span></span> <span data-ttu-id="63cab-128">Hello [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metoda je použité tooconnect tooMySQL.</span><span class="sxs-lookup"><span data-stu-id="63cab-128">hello [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used tooconnect tooMySQL.</span></span> <span data-ttu-id="63cab-129">Metody [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) a execute() jsou použité toodrop a vytvořit tabulku hello.</span><span class="sxs-lookup"><span data-stu-id="63cab-129">Methods [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) and execute() are used toodrop and create hello table.</span></span> <span data-ttu-id="63cab-130">objekt prepareStatement Hello je příkazy insert hello toobuild použité, s setString() a setInt() hodnotami parametrů toobind hello.</span><span class="sxs-lookup"><span data-stu-id="63cab-130">hello prepareStatement object is used toobuild hello insert commands, with setString() and setInt() toobind hello parameter values.</span></span> <span data-ttu-id="63cab-131">Metoda executeUpdate() spustí příkaz hello pro každou sadu hodnot parametrů tooinsert hello.</span><span class="sxs-lookup"><span data-stu-id="63cab-131">Method executeUpdate() runs hello command for each set of parameters tooinsert hello values.</span></span> 

<span data-ttu-id="63cab-132">Nahraďte hello hodnoty, které jste zadali při vytváření vlastní server a databáze hello hostitele, databáze, uživatele a heslo parametry.</span><span class="sxs-lookup"><span data-stu-id="63cab-132">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

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

        // check that hello driver is installed
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

## <a name="read-data"></a><span data-ttu-id="63cab-133">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="63cab-133">Read data</span></span>
<span data-ttu-id="63cab-134">Použití hello následující kód tooread hello dat pomocí **vyberte** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="63cab-134">Use hello following code tooread hello data with a **SELECT** SQL statement.</span></span> <span data-ttu-id="63cab-135">Hello [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metoda je použité tooconnect tooMySQL.</span><span class="sxs-lookup"><span data-stu-id="63cab-135">hello [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used tooconnect tooMySQL.</span></span> <span data-ttu-id="63cab-136">Hello metody [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) a executeQuery() jsou použité tooconnect a spusťte příkaz select hello.</span><span class="sxs-lookup"><span data-stu-id="63cab-136">hello methods [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) and executeQuery() are used tooconnect and run hello select statement.</span></span> <span data-ttu-id="63cab-137">výsledky Hello se zpracovávají pomocí [třídy ResultSet](https://docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html) objektu.</span><span class="sxs-lookup"><span data-stu-id="63cab-137">hello results are processed using a [ResultSet](https://docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html) object.</span></span> 

<span data-ttu-id="63cab-138">Nahraďte hello hodnoty, které jste zadali při vytváření vlastní server a databáze hello hostitele, databáze, uživatele a heslo parametry.</span><span class="sxs-lookup"><span data-stu-id="63cab-138">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

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

        // check that hello driver is installed
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
            throw new SQLException("Failed toocreate connection toodatabase", e);
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
                throw new SQLException("Encountered an error when executing given sql statement", e);
            }       
        }
        else {
            System.out.println("Failed toocreate connection toodatabase."); 
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="update-data"></a><span data-ttu-id="63cab-139">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="63cab-139">Update data</span></span>
<span data-ttu-id="63cab-140">Použití hello následující kód toochange hello dat pomocí **aktualizace** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="63cab-140">Use hello following code toochange hello data with an **UPDATE** SQL statement.</span></span> <span data-ttu-id="63cab-141">Hello [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metoda je použité tooconnect tooMySQL.</span><span class="sxs-lookup"><span data-stu-id="63cab-141">hello [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used tooconnect tooMySQL.</span></span> <span data-ttu-id="63cab-142">Hello metody [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) a executeUpdate() jsou použité tooprepare a spusťte příkaz update hello.</span><span class="sxs-lookup"><span data-stu-id="63cab-142">hello methods [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) and executeUpdate() are used tooprepare and run hello update statement.</span></span> 

<span data-ttu-id="63cab-143">Nahraďte hello hodnoty, které jste zadali při vytváření vlastní server a databáze hello hostitele, databáze, uživatele a heslo parametry.</span><span class="sxs-lookup"><span data-stu-id="63cab-143">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

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

        // check that hello driver is installed
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
            
            // set up hello connection properties
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

## <a name="delete-data"></a><span data-ttu-id="63cab-144">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="63cab-144">Delete data</span></span>
<span data-ttu-id="63cab-145">Použití hello následující kód tooremove dat pomocí **odstranit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="63cab-145">Use hello following code tooremove data with a **DELETE** SQL statement.</span></span> <span data-ttu-id="63cab-146">Hello [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metoda je použité tooconnect tooMySQL.</span><span class="sxs-lookup"><span data-stu-id="63cab-146">hello [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used tooconnect tooMySQL.</span></span>  <span data-ttu-id="63cab-147">Hello metody [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) a executeUpdate() jsou použité tooprepare a spusťte příkaz update hello.</span><span class="sxs-lookup"><span data-stu-id="63cab-147">hello methods [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) and executeUpdate() are used tooprepare and run hello update statement.</span></span> 

<span data-ttu-id="63cab-148">Nahraďte hello hodnoty, které jste zadali při vytváření vlastní server a databáze hello hostitele, databáze, uživatele a heslo parametry.</span><span class="sxs-lookup"><span data-stu-id="63cab-148">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

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
        
        // check that hello driver is installed
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
            
            // set up hello connection properties
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
            throw new SQLException("Failed toocreate connection toodatabase", e);
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

## <a name="next-steps"></a><span data-ttu-id="63cab-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="63cab-149">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="63cab-150">Migrace vaší tooAzure databáze MySQL databáze pro databázi MySQL pomocí výpisu a obnovení</span><span class="sxs-lookup"><span data-stu-id="63cab-150">Migrate your MySQL database tooAzure Database for MySQL using dump and restore</span></span>](concepts-migrate-dump-restore.md)
