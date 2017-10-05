---
title: "Připojení k Azure Database for MySQL z jazyka C# | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete vzorový kód jazyka C# (.NET), který můžete použít k připojení a dotazování dat ze služby Azure Database for MySQL."
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
ms.openlocfilehash: f1488f6b4a240165c71c95f759af73d6b9fd7bfe
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-database-for-mysql-use-net-c-to-connect-and-query-data"></a><span data-ttu-id="21a38-103">Azure Database for MySQL: Připojení a dotazování dat pomocí .NET (jazyk C#)</span><span class="sxs-lookup"><span data-stu-id="21a38-103">Azure Database for MySQL: Use .NET (C#) to connect and query data</span></span>
<span data-ttu-id="21a38-104">Tento rychlý start ukazuje, jak se připojit ke službě Azure Database for MySQL pomocí aplikace v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="21a38-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using a C# application.</span></span> <span data-ttu-id="21a38-105">Ukazuje, jak pomocí příkazů jazyka SQL dotazovat, vkládat, aktualizovat a odstraňovat data v databázi.</span><span class="sxs-lookup"><span data-stu-id="21a38-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="21a38-106">V krocích v tomto článku se předpokládá, že máte zkušenosti s vývojem pomocí jazyka C# a teprve začínáte pracovat se službou Azure Database for MySQL.</span><span class="sxs-lookup"><span data-stu-id="21a38-106">The steps in this article assume that you are familiar with developing using C#, and that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21a38-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="21a38-107">Prerequisites</span></span>
<span data-ttu-id="21a38-108">Tento rychlý start jako výchozí bod využívá prostředky vytvořené v některém z těchto průvodců:</span><span class="sxs-lookup"><span data-stu-id="21a38-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="21a38-109">Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="21a38-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="21a38-110">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="21a38-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="21a38-111">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="21a38-111">You also need to:</span></span>
- <span data-ttu-id="21a38-112">Nainstalovat rozhraní [.NET](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="21a38-112">Install [.NET](https://www.microsoft.com/net/download).</span></span> <span data-ttu-id="21a38-113">Postupujte podle kroků v odkazovaném článku a nainstalujte .NET pro vaši platformu (Windows, Ubuntu Linux nebo macOS).</span><span class="sxs-lookup"><span data-stu-id="21a38-113">Follow the steps in the linked article to install .NET specifically for your platform (Windows, Ubuntu Linux, or macOS).</span></span> 
- <span data-ttu-id="21a38-114">Nainstalovat sadu [Visual Studio](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="21a38-114">Install [Visual Studio](https://www.visualstudio.com/downloads/).</span></span>
- <span data-ttu-id="21a38-115">Nainstalovat [ovladač ODBC pro MySQL](https://dev.mysql.com/downloads/connector/odbc/).</span><span class="sxs-lookup"><span data-stu-id="21a38-115">Install [ODBC Driver for MySQL](https://dev.mysql.com/downloads/connector/odbc/).</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="21a38-116">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="21a38-116">Get connection information</span></span>
<span data-ttu-id="21a38-117">Získejte informace o připojení potřebné pro připojení ke službě Azure Database for MySQL.</span><span class="sxs-lookup"><span data-stu-id="21a38-117">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="21a38-118">Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="21a38-118">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="21a38-119">Přihlaste se k [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="21a38-119">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="21a38-120">V nabídce vlevo na webu Azure Portal klikněte na **Všechny prostředky** a vyhledejte vytvořený server, například **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="21a38-120">From the left-hand menu in Azure portal, click **All resources** and search for the server you have created, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="21a38-121">Klikněte na název serveru.</span><span class="sxs-lookup"><span data-stu-id="21a38-121">Click the server name.</span></span>
4. <span data-ttu-id="21a38-122">Vyberte stránku **Vlastnosti** serveru.</span><span class="sxs-lookup"><span data-stu-id="21a38-122">Select the server's **Properties** page.</span></span> <span data-ttu-id="21a38-123">Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="21a38-123">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="21a38-124">![Název serveru Azure Database for MySQL](./media/connect-csharp/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="21a38-124">![Azure Database for MySQL server name](./media/connect-csharp/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="21a38-125">Pokud zapomenete přihlašovací údaje pro váš server, přejděte na stránku **Přehled**, kde můžete zobrazit přihlašovací jméno správce serveru a v případě potřeby obnovit heslo.</span><span class="sxs-lookup"><span data-stu-id="21a38-125">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="21a38-126">Připojení, vytvoření tabulky a vložení dat</span><span class="sxs-lookup"><span data-stu-id="21a38-126">Connect, create table, and insert data</span></span>
<span data-ttu-id="21a38-127">Použijte následující kód k připojení a načtení dat pomocí příkazů **CREATE TABLE** a **INSERT INTO** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="21a38-127">Use the following code to connect and load the data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span> <span data-ttu-id="21a38-128">Tento kód pro navázání připojení k MySQL využívá třídu ODBC s metodou [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="21a38-128">The code uses ODBC class with method [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) to establish a connection to MySQL.</span></span> <span data-ttu-id="21a38-129">Potom tento kód použije metodu [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), nastaví vlastnost CommandText a volá metodu [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) pro spuštění databázových příkazů.</span><span class="sxs-lookup"><span data-stu-id="21a38-129">Then the code uses method [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), sets the CommandText property, and calls method [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) to run the database commands.</span></span> 

<span data-ttu-id="21a38-130">Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="21a38-130">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

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

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }

    }
}

```

## <a name="read-data"></a><span data-ttu-id="21a38-131">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="21a38-131">Read data</span></span>

<span data-ttu-id="21a38-132">Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **SELECT** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="21a38-132">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="21a38-133">Tento kód pro navázání připojení k MySQL využívá třídu ODBC s metodou [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="21a38-133">The code uses ODBC class with method [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) to establish a connection to MySQL.</span></span> <span data-ttu-id="21a38-134">Potom tento kód použije metodu [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) a metodu [ExecuteReader()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executereader(v=vs.110).aspx) pro spuštění databázových příkazů.</span><span class="sxs-lookup"><span data-stu-id="21a38-134">Then the code uses method [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) and method [ExecuteReader()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executereader(v=vs.110).aspx) to run the database commands.</span></span> <span data-ttu-id="21a38-135">Dál tento kód použije [Read()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcdatareader.read(v=vs.110).aspx) k přechodu na záznamy ve výsledcích.</span><span class="sxs-lookup"><span data-stu-id="21a38-135">Next the code uses [Read()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcdatareader.read(v=vs.110).aspx) to advance to the records in the results.</span></span> <span data-ttu-id="21a38-136">Potom tento kód použije GetInt32 a GetString k parsování hodnot v záznamu.</span><span class="sxs-lookup"><span data-stu-id="21a38-136">Then the code uses GetInt32 and GetString to parse the values in the record.</span></span>

<span data-ttu-id="21a38-137">Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="21a38-137">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

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

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}


```

## <a name="update-data"></a><span data-ttu-id="21a38-138">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="21a38-138">Update data</span></span>
<span data-ttu-id="21a38-139">Použijte následující kód k připojení a čtení dat pomocí příkazu **UPDATE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="21a38-139">Use the following code to connect and read the data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="21a38-140">Tento kód pro navázání připojení k MySQL využívá třídu ODBC s metodou [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="21a38-140">The code uses ODBC class with method [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) to establish a connection to MySQL.</span></span> <span data-ttu-id="21a38-141">Potom tento kód použije metodu [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), nastaví vlastnost CommandText a volá metodu [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) pro spuštění databázových příkazů.</span><span class="sxs-lookup"><span data-stu-id="21a38-141">Then the code uses method [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), sets the CommandText property, and calls method [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) to run the database commands.</span></span>

<span data-ttu-id="21a38-142">Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="21a38-142">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

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

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}



```


## <a name="delete-data"></a><span data-ttu-id="21a38-143">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="21a38-143">Delete data</span></span>
<span data-ttu-id="21a38-144">Pomocí následujícího kódu se připojte a odstraňte data s využitím příkazu **DELETE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="21a38-144">Use the following code to connect and delete the data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="21a38-145">Tento kód pro navázání připojení k MySQL využívá třídu ODBC s metodou [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="21a38-145">The code uses ODBC class with method [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) to establish a connection to MySQL.</span></span> <span data-ttu-id="21a38-146">Potom tento kód použije metodu [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), nastaví vlastnost CommandText a volá metodu [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) pro spuštění databázových příkazů.</span><span class="sxs-lookup"><span data-stu-id="21a38-146">Then the code uses method [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx), sets the CommandText property, and calls method [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) to run the database commands.</span></span>

<span data-ttu-id="21a38-147">Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="21a38-147">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

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

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}

```

## <a name="next-steps"></a><span data-ttu-id="21a38-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="21a38-148">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="21a38-149">Migrace databáze MySQL do služby Azure Database for MySQL pomocí výpisu a obnovení.</span><span class="sxs-lookup"><span data-stu-id="21a38-149">Migrate your MySQL database to Azure Database for MySQL using dump and restore</span></span>](concepts-migrate-dump-restore.md)
