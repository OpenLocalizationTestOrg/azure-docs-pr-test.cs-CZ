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
# <a name="azure-database-for-mysql-use-java-tooconnect-and-query-data"></a>Azure databáze pro databázi MySQL: použití Java tooconnect a dotazování dat
Tento rychlý start předvádí jak tooconnect tooan Azure databáze pro databázi MySQL pomocí aplikace Java. Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello. Hello postup v tomto článku předpokládá, že jste obeznámeni s vývojem pomocí Java, a že jste novou tooworking s Azure Database pro databázi MySQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

Budete také muset:
- Stáhnout ovladač JDBC hello [MySQL konektor/J](https://dev.mysql.com/downloads/connector/j/)
- Zahrňte soubor jar JDBC hello (například mysql konektor java-5.1.42-bin.jar) do cesty pro třídy vaší aplikace. Pokud s tím máte problémy, podívejte se na specifika cest ke třídám do dokumentace k vašemu prostředí, jako například [Apache Tomcat](https://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html) nebo [Java SE](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html).
- Ujistěte se, že je databáze Azure pro zabezpečení připojení MySQL nakonfigurovaná s branou firewall hello otevřít a upravit nastavení SSL pro vaše aplikace tooconnect úspěšně.

## <a name="get-connection-information"></a>Získání informací o připojení
Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL. Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. V levém podokně hello, klikněte na **všechny prostředky**a poté vyhledejte hello serveru, které jste vytvořili (například **myserver4demo**).
3. Klikněte na název serveru hello.
4. Vyberte hello serveru **vlastnosti** stránky. Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.
 ![Název serveru Azure Database for MySQL](./media/connect-java/1_server-properties-name-login.png)
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.

## <a name="connect-create-table-and-insert-data"></a>Připojení, vytvoření tabulky a vložení dat
Použití hello následující kód tooconnect a zatížení hello dat pomocí funkce hello **vložit** příkaz jazyka SQL. Hello [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metoda je použité tooconnect tooMySQL. Metody [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) a execute() jsou použité toodrop a vytvořit tabulku hello. objekt prepareStatement Hello je příkazy insert hello toobuild použité, s setString() a setInt() hodnotami parametrů toobind hello. Metoda executeUpdate() spustí příkaz hello pro každou sadu hodnot parametrů tooinsert hello. 

Nahraďte hello hodnoty, které jste zadali při vytváření vlastní server a databáze hello hostitele, databáze, uživatele a heslo parametry.

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

## <a name="read-data"></a>Čtení dat
Použití hello následující kód tooread hello dat pomocí **vyberte** příkaz jazyka SQL. Hello [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metoda je použité tooconnect tooMySQL. Hello metody [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) a executeQuery() jsou použité tooconnect a spusťte příkaz select hello. výsledky Hello se zpracovávají pomocí [třídy ResultSet](https://docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html) objektu. 

Nahraďte hello hodnoty, které jste zadali při vytváření vlastní server a databáze hello hostitele, databáze, uživatele a heslo parametry.

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

## <a name="update-data"></a>Aktualizace dat
Použití hello následující kód toochange hello dat pomocí **aktualizace** příkaz jazyka SQL. Hello [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metoda je použité tooconnect tooMySQL. Hello metody [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) a executeUpdate() jsou použité tooprepare a spusťte příkaz update hello. 

Nahraďte hello hodnoty, které jste zadali při vytváření vlastní server a databáze hello hostitele, databáze, uživatele a heslo parametry.

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

## <a name="delete-data"></a>Odstranění dat
Použití hello následující kód tooremove dat pomocí **odstranit** příkaz jazyka SQL. Hello [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metoda je použité tooconnect tooMySQL.  Hello metody [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) a executeUpdate() jsou použité tooprepare a spusťte příkaz update hello. 

Nahraďte hello hodnoty, které jste zadali při vytváření vlastní server a databáze hello hostitele, databáze, uživatele a heslo parametry.

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

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší tooAzure databáze MySQL databáze pro databázi MySQL pomocí výpisu a obnovení](concepts-migrate-dump-restore.md)
