---
title: "Připojit tooAzure databáze pro databázi MySQL z jazyka C# | Microsoft Docs"
description: "Tento rychlý start poskytuje C# (.NET) ukázku kódu pomocí tooconnect a zadávat dotazy na data z databáze Azure pro databázi MySQL."
services: MySQL
author: seanli1988
ms.author: seal
manager: janders
editor: jasonwhowell
ms.service: MySQL-database
ms.custom: mvc
ms.devlang: csharp
ms.topic: hero-article
ms.date: 07/10/2017
ms.openlocfilehash: 0dca98186199a40ef9cc592b93c3b2e815260273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-net-c-tooconnect-and-query-data"></a>Azure databáze pro databázi MySQL: tooconnect a dotaz data pomocí .NET (C#)
Tento rychlý start předvádí jak tooconnect tooan Azure databáze pro databázi MySQL pomocí aplikace v jazyce C#. Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello. Hello postup v tomto článku předpokládá, že jste obeznámeni s vývojem pomocí jazyka C#, a že jste novou tooworking s Azure Database pro databázi MySQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

Budete také muset:
- Nainstalovat rozhraní [.NET](https://www.microsoft.com/net/download). Postupujte podle kroků hello v hello propojit článek tooinstall .NET speciálně pro vaši platformu (Windows, Ubuntu Linux nebo systému macOS). 
- Nainstalovat sadu [Visual Studio](https://www.visualstudio.com/downloads/).
- Nainstalovat [ovladač ODBC pro MySQL](https://dev.mysql.com/downloads/connector/odbc/).

## <a name="get-connection-information"></a>Získání informací o připojení
Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL. Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru, které jste vytvořili, například **myserver4demo**.
3. Klikněte na název serveru hello.
4. Vyberte hello serveru **vlastnosti** stránky. Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.
 ![Název serveru Azure Database for MySQL](./media/connect-csharp/1_server-properties-name-login.png)
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.

## <a name="connect-create-table-and-insert-data"></a>Připojení, vytvoření tabulky a vložení dat
Použití hello následující kód tooconnect a načtení dat pomocí hello **CREATE TABLE** a **INSERT INTO** příkazů SQL. Kód Hello používá třídu rozhraní ODBC se metoda [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) tooestablish tooMySQL připojení. Potom kód hello používá metoda [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), nastaví vlastnost CommandText hello a volá metodu [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) toorun hello databáze příkazy. 

Nahraďte hello hostitele, DBName, uživatele a heslo parametry hello hodnoty, kterou jste zadali při vytvoření hello server a databáze. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data;
using System.Data.Odbc;

namespace driver
{
    class MySQLCreate
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText = "DROP TABLE IF EXISTS inventory;";
            command.ExecuteNonQuery();
            Console.Out.WriteLine("Finished dropping table (if existed)");

            command.CommandText = "CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);";
            command.ExecuteNonQuery();
            Console.Out.WriteLine("Finished creating table");

            command.CommandText =
                String.Format(
                    @"INSERT INTO inventory (name, quantity) VALUES ({0}, {1});
                    INSERT INTO inventory (name, quantity) VALUES ({2}, {3});
                    INSERT INTO inventory (name, quantity) VALUES ({4}, {5});",
                    "\'banana\'", 150,
                    "\'orange\'", 154,
                    "\'apple\'", 100
                    );

            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows inserted={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN tooexit");
            Console.ReadLine();
        }

    }
}

```

## <a name="read-data"></a>Čtení dat

Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL. Kód Hello používá třídu rozhraní ODBC se metoda [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) tooestablish tooMySQL připojení. Potom kód hello používá metoda [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) a metoda [ExecuteReader())](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executereader(v=vs.110).aspx) toorun hello databáze příkazy. Další hello kód používá [Read()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcdatareader.read(v=vs.110).aspx) tooadvance toohello záznamy ve výsledcích hello. Potom kód hello používá hodnoty hello tooparse GetInt32 a GetString – v záznamu hello.

Nahraďte hello hostitele, DBName, uživatele a heslo parametry hello hodnoty, kterou jste zadali při vytvoření hello server a databáze. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data;
using System.Data.Odbc;


namespace driver
{
    class MySQLRead
    {

        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText = "SELECT * FROM inventory;";

            var reader = command.ExecuteReader();
            while (reader.Read())
            {
                Console.WriteLine(
                    string.Format(
                        "Reading from table=({0}, {1}, {2})",
                        reader.GetInt32(0).ToString(),
                        reader.GetString(1),
                        reader.GetInt32(2).ToString()
                        )
                    );
            }

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN tooexit");
            Console.ReadLine();
        }
    }
}


```

## <a name="update-data"></a>Aktualizace dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **aktualizace** příkaz jazyka SQL. Kód Hello používá třídu rozhraní ODBC se metoda [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) tooestablish tooMySQL připojení. Potom kód hello používá metoda [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), nastaví vlastnost CommandText hello a volá metodu [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) toorun hello databáze příkazy.

Nahraďte hello hostitele, DBName, uživatele a heslo parametry hello hodnoty, kterou jste zadali při vytvoření hello server a databáze. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Odbc;

namespace driver
{
    class MySQLUpdate
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText =
            String.Format("UPDATE inventory SET quantity = {0} WHERE name = {1};",
                200,
                "\'banana\'"
                );

            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows updated={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN tooexit");
            Console.ReadLine();
        }
    }
}



```


## <a name="delete-data"></a>Odstranění dat
Použití hello následující kód tooconnect a odstranit hello dat pomocí **odstranit** příkaz jazyka SQL. 

Kód Hello používá třídu rozhraní ODBC se metoda [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) tooestablish tooMySQL připojení. Potom kód hello používá metoda [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), nastaví vlastnost CommandText hello a volá metodu [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) toorun hello databáze příkazy.

Nahraďte hello hostitele, DBName, uživatele a heslo parametry hello hodnoty, kterou jste zadali při vytvoření hello server a databáze. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Odbc;

namespace driver
{
    class MySQLDelete
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText =
                String.Format("DELETE FROM inventory WHERE name = {0};",
                    "\'orange\'");
            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows deleted={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN tooexit");
            Console.ReadLine();
        }
    }
}

```

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší tooAzure databáze MySQL databáze pro databázi MySQL pomocí výpisu a obnovení](concepts-migrate-dump-restore.md)
