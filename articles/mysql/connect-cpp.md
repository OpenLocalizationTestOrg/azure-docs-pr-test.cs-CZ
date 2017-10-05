---
title: "Připojení k Azure Database for MySQL z jazyka C++ | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete vzorový kód jazyka C++, který můžete použít k připojení a dotazování dat ze služby Azure Database for MySQL."
services: mysql
author: seanli1988
ms.author: seal
manager: janders
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: C++
ms.topic: hero-article
ms.date: 08/03/2017
ms.openlocfilehash: 63388b83b913d95136140fa4c56af0dbebbdad81
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-database-for-mysql-use-connectorc-to-connect-and-query-data"></a><span data-ttu-id="2b670-103">Azure Database for MySQL: Připojení a dotazování dat pomocí konektoru Connector/C++</span><span class="sxs-lookup"><span data-stu-id="2b670-103">Azure Database for MySQL: Use Connector/C++ to connect and query data</span></span>
<span data-ttu-id="2b670-104">Tento rychlý start ukazuje, jak se připojit ke službě Azure Database for MySQL pomocí aplikace v jazyce C++.</span><span class="sxs-lookup"><span data-stu-id="2b670-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using a C++ application.</span></span> <span data-ttu-id="2b670-105">Ukazuje, jak pomocí příkazů jazyka SQL dotazovat, vkládat, aktualizovat a odstraňovat data v databázi.</span><span class="sxs-lookup"><span data-stu-id="2b670-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="2b670-106">V krocích v tomto článku se předpokládá, že máte zkušenosti s vývojem pomocí jazyka C++ a teprve začínáte pracovat se službou Azure Database for MySQL.</span><span class="sxs-lookup"><span data-stu-id="2b670-106">The steps in this article assume that you are familiar with developing using C++, and that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2b670-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2b670-107">Prerequisites</span></span>
<span data-ttu-id="2b670-108">Tento rychlý start jako výchozí bod využívá prostředky vytvořené v některém z těchto průvodců:</span><span class="sxs-lookup"><span data-stu-id="2b670-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="2b670-109">Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2b670-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="2b670-110">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2b670-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="2b670-111">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="2b670-111">You also need to:</span></span>
- <span data-ttu-id="2b670-112">Nainstalovat [.NET Framework](https://www.microsoft.com/net/download)</span><span class="sxs-lookup"><span data-stu-id="2b670-112">Install [.NET Framework](https://www.microsoft.com/net/download)</span></span>
- <span data-ttu-id="2b670-113">Nainstalovat [Visual Studio](https://www.visualstudio.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="2b670-113">Install [Visual Studio](https://www.visualstudio.com/downloads/)</span></span>
- <span data-ttu-id="2b670-114">Nainstalovat [MySQL Connector/C++](https://dev.mysql.com/downloads/connector/cpp/)</span><span class="sxs-lookup"><span data-stu-id="2b670-114">Install [MySQL Connector/C++](https://dev.mysql.com/downloads/connector/cpp/)</span></span> 
- <span data-ttu-id="2b670-115">Nainstalovat [Boost](http://www.boost.org/)</span><span class="sxs-lookup"><span data-stu-id="2b670-115">Install [Boost](http://www.boost.org/)</span></span>

## <a name="install-visual-studio-and-net"></a><span data-ttu-id="2b670-116">Instalace sady Visual Studio a .NET</span><span class="sxs-lookup"><span data-stu-id="2b670-116">Install Visual Studio and .NET</span></span>
<span data-ttu-id="2b670-117">Kroky v této části předpokládají, že máte zkušenosti s vývojem pomocí rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="2b670-117">The steps in this section assume that you are familiar with developing using .NET.</span></span>

### <a name="windows"></a><span data-ttu-id="2b670-118">**Windows**</span><span class="sxs-lookup"><span data-stu-id="2b670-118">**Windows**</span></span>
1. <span data-ttu-id="2b670-119">Nainstalujte sadu Visual Studio 2017 Community, což je plně vybavené, rozšiřitelné a bezplatné integrované vývojové prostředí (IDE) pro vytváření moderních aplikací pro Android, iOS a Windows, stejně jako webových a databázových aplikací a cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="2b670-119">Install Visual Studio 2017 Community, which is a full featured, extensible, free IDE for creating modern applications for Android, iOS, Windows, as well as web & database applications and cloud services.</span></span> <span data-ttu-id="2b670-120">Můžete nainstalovat buď úplné rozhraní .NET Framework, nebo jenom jádro .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2b670-120">You can install either the full .NET Framework or just .NET Core.</span></span> <span data-ttu-id="2b670-121">Fragmenty kódu v rychlém startu pracují s oběma.</span><span class="sxs-lookup"><span data-stu-id="2b670-121">The code snippets in the Quickstart work with either.</span></span> <span data-ttu-id="2b670-122">Pokud již máte v počítači nainstalovanou sadu Visual Studio, přeskočte následující dva kroky.</span><span class="sxs-lookup"><span data-stu-id="2b670-122">If you already have Visual Studio installed on your machine, skip the next two steps.</span></span>
   - <span data-ttu-id="2b670-123">Stáhněte si [instalační program sady Visual Studio 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).</span><span class="sxs-lookup"><span data-stu-id="2b670-123">Download the [Visual Studio 2017 installer](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).</span></span> 
   - <span data-ttu-id="2b670-124">Spusťte instalační program a podle zobrazených pokynů instalaci dokončete.</span><span class="sxs-lookup"><span data-stu-id="2b670-124">Run the installer and follow the installation prompts to complete the installation.</span></span>

### <a name="configure-visual-studio"></a><span data-ttu-id="2b670-125">**Konfigurace sady Visual Studio**</span><span class="sxs-lookup"><span data-stu-id="2b670-125">**Configure Visual Studio**</span></span>
1. <span data-ttu-id="2b670-126">V sadě Visual Studio v části Vlastnosti projektu > Vlastnosti konfigurace > C/C++ > Linker > Obecné > Další adresáře knihoven přidejte adresář lib\opt (tj.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\lib\opt) konektoru C++.</span><span class="sxs-lookup"><span data-stu-id="2b670-126">From Visual Studio, project property > configuration properties > C/C++ > linker > general > additional library directories, add the lib\opt directory (i.e.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\lib\opt) of the c++ connector.</span></span>
2. <span data-ttu-id="2b670-127">V sadě Visual Studio v části Vlastnosti projektu > Vlastnosti konfigurace > C/C++ > Obecné > Další adresáře k zahrnutí:</span><span class="sxs-lookup"><span data-stu-id="2b670-127">From Visual Studio, project property > configuration properties > C/C++ > general > additional include directories</span></span>
   - <span data-ttu-id="2b670-128">Přidejte adresář include/ konektoru C++ (tj.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\include\)</span><span class="sxs-lookup"><span data-stu-id="2b670-128">Add include/ directory of c++ connector (i.e.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\include\)</span></span>
   - <span data-ttu-id="2b670-129">Přidejte kořenový adresář knihovny Boost (tj.: C:\boost_1_64_0\)</span><span class="sxs-lookup"><span data-stu-id="2b670-129">Add Boost library's root directory (i.e.: C:\boost_1_64_0\)</span></span>
3. <span data-ttu-id="2b670-130">V sadě Visual Studio v části Vlastnosti projektu > Vlastnosti konfigurace > C/C++ > Linker > Vstup > Další závislosti přidejte do textového pole mysqlcppconn.lib.</span><span class="sxs-lookup"><span data-stu-id="2b670-130">From Visual Studio, project property > configuration properties > C/C++ > linker > Input > Additional Dependencies, add mysqlcppconn.lib into the text field</span></span>
4. <span data-ttu-id="2b670-131">Zkopírujte soubor mysqlcppconn.dll ze složky knihovny konektoru C++ z kroku 3 do stejného adresáře jako spustitelný soubor aplikace nebo ho přidejte do proměnné prostředí, aby ho vaše aplikace mohla najít.</span><span class="sxs-lookup"><span data-stu-id="2b670-131">Either copy mysqlcppconn.dll from the c++ connector library folder in step 3 to the same directory as the application executable or add it to the environment variable so your application can find it.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="2b670-132">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="2b670-132">Get connection information</span></span>
<span data-ttu-id="2b670-133">Získejte informace o připojení potřebné pro připojení ke službě Azure Database for MySQL.</span><span class="sxs-lookup"><span data-stu-id="2b670-133">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="2b670-134">Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="2b670-134">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="2b670-135">Přihlaste se k [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2b670-135">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="2b670-136">V nabídce vlevo na webu Azure Portal klikněte na **Všechny prostředky** a vyhledejte vytvořený server, například **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="2b670-136">From the left-hand menu in Azure portal, click **All resources** and search for the server you have created, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="2b670-137">Klikněte na název serveru.</span><span class="sxs-lookup"><span data-stu-id="2b670-137">Click the server name.</span></span>
4. <span data-ttu-id="2b670-138">Vyberte stránku **Vlastnosti** serveru.</span><span class="sxs-lookup"><span data-stu-id="2b670-138">Select the server's **Properties** page.</span></span> <span data-ttu-id="2b670-139">Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="2b670-139">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="2b670-140">![Název serveru Azure Database for MySQL](./media/connect-cpp/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="2b670-140">![Azure Database for MySQL server name](./media/connect-cpp/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="2b670-141">Pokud zapomenete přihlašovací údaje pro váš server, přejděte na stránku **Přehled**, kde můžete zobrazit přihlašovací jméno správce serveru a v případě potřeby obnovit heslo.</span><span class="sxs-lookup"><span data-stu-id="2b670-141">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="2b670-142">Připojení, vytvoření tabulky a vložení dat</span><span class="sxs-lookup"><span data-stu-id="2b670-142">Connect, create table, and insert data</span></span>
<span data-ttu-id="2b670-143">Použijte následující kód k připojení a načtení dat pomocí příkazů **CREATE TABLE** a **INSERT INTO** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="2b670-143">Use the following code to connect and load the data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span> <span data-ttu-id="2b670-144">Tento kód pro navázání připojení k MySQL využívá třídu sql::Driver s metodou connect().</span><span class="sxs-lookup"><span data-stu-id="2b670-144">The code uses sql::Driver class with the connect() method to establish a connection to MySQL.</span></span> <span data-ttu-id="2b670-145">Potom kód použije metody createStatement() a execute() pro spuštění příkazů databáze.</span><span class="sxs-lookup"><span data-stu-id="2b670-145">Then the code uses method createStatement() and execute() to run the database commands.</span></span> 

<span data-ttu-id="2b670-146">Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="2b670-146">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```c++
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::Statement *stmt;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }

    stmt = con>createStatement();
    stmt>execute("DROP TABLE IF EXISTS inventory");
    cout << "Finished dropping table (if existed)" << endl;
    stmt>execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
    cout << "Finished creating table" << endl;
    delete stmt;

    pstmt = con>prepareStatement("INSERT INTO inventory(name, quantity) VALUES(?,?)");
    pstmt>setString(1, "banana");
    pstmt>setInt(2, 150);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "orange");
    pstmt>setInt(2, 154);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "apple");
    pstmt>setInt(2, 100);
    pstmt>execute();
    cout << "One row inserted." << endl;
    
    delete pstmt;   
    delete con;
    system("pause");
    return 0;

```

## <a name="read-data"></a><span data-ttu-id="2b670-147">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="2b670-147">Read data</span></span>

<span data-ttu-id="2b670-148">Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **SELECT** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="2b670-148">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="2b670-149">Tento kód pro navázání připojení k MySQL využívá třídu sql::Driver s metodou connect().</span><span class="sxs-lookup"><span data-stu-id="2b670-149">The code uses sql::Driver class with the connect() method to establish a connection to MySQL.</span></span> <span data-ttu-id="2b670-150">Potom kód použije metody prepareStatement() a executeQuery() pro spuštění příkazů select.</span><span class="sxs-lookup"><span data-stu-id="2b670-150">Then the code uses method prepareStatement() and executeQuery() to run the select commands.</span></span> <span data-ttu-id="2b670-151">Nakonec kód použije metodu next() k přechodu na záznamy ve výsledcích.</span><span class="sxs-lookup"><span data-stu-id="2b670-151">Finally the code uses next() to advance to the records in the results.</span></span> <span data-ttu-id="2b670-152">Potom kód použije metody getInt() a getString() k parsování hodnot v záznamu.</span><span class="sxs-lookup"><span data-stu-id="2b670-152">Then the code uses getInt() and getString() to parse the values in the record.</span></span>

<span data-ttu-id="2b670-153">Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="2b670-153">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

//  select  
    pstmt = con>prepareStatement("SELECT * FROM inventory;");
    result = pstmt>executeQuery();  
    
    while (result>next())
        printf("Reading from table=(%d, %s, %d)\n", result>getInt(1), result>getString(2).c_str(), result>getInt(3));   
    
    delete result;
    delete pstmt;   
    delete con;
    system("pause");
    return 0;
}
```

## <a name="update-data"></a><span data-ttu-id="2b670-154">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="2b670-154">Update data</span></span>
<span data-ttu-id="2b670-155">Použijte následující kód k připojení a čtení dat pomocí příkazu **UPDATE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="2b670-155">Use the following code to connect and read the data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="2b670-156">Tento kód pro navázání připojení k MySQL využívá třídu sql::Driver s metodou connect().</span><span class="sxs-lookup"><span data-stu-id="2b670-156">The code uses sql::Driver class with the connect() method to establish a connection to MySQL.</span></span> <span data-ttu-id="2b670-157">Potom kód použije metody prepareStatement() a executeQuery() pro spuštění příkazů update.</span><span class="sxs-lookup"><span data-stu-id="2b670-157">Then the code uses method prepareStatement() and executeQuery() to run the update commands.</span></span> 

<span data-ttu-id="2b670-158">Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="2b670-158">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

    //update
    pstmt = con>prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?");
    pstmt>setInt(1, 200);
    pstmt>setString(2, "banana");
    pstmt>executeQuery();
    printf("Row updated\n");
    
    delete con;
    delete pstmt;
    system("pause");
    return 0;
}
```


## <a name="delete-data"></a><span data-ttu-id="2b670-159">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="2b670-159">Delete data</span></span>
<span data-ttu-id="2b670-160">Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **DELETE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="2b670-160">Use the following code to connect and read the data using a **DELETE** SQL statement.</span></span> <span data-ttu-id="2b670-161">Tento kód pro navázání připojení k MySQL využívá třídu sql::Driver s metodou connect().</span><span class="sxs-lookup"><span data-stu-id="2b670-161">The code uses sql::Driver class with the connect() method to establish a connection to MySQL.</span></span> <span data-ttu-id="2b670-162">Potom kód použije metody prepareStatement() a executeQuery() pro spuštění příkazů delete.</span><span class="sxs-lookup"><span data-stu-id="2b670-162">Then the code uses method prepareStatement() and executeQuery() to run the delete commands.</span></span>

<span data-ttu-id="2b670-163">Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="2b670-163">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }
        
    //delete
    pstmt = con>prepareStatement("DELETE FROM inventory WHERE name = ?");
    pstmt>setString(1, "orange");
    result = pstmt>executeQuery();
    printf("Row deleted\n");    
    
    delete pstmt;
    delete con;
    delete result;
    system("pause");
    return 0;
}
```

## <a name="next-steps"></a><span data-ttu-id="2b670-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2b670-164">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="2b670-165">Migrace databáze MySQL do služby Azure Database for MySQL pomocí výpisu a obnovení.</span><span class="sxs-lookup"><span data-stu-id="2b670-165">Migrate your MySQL database to Azure Database for MySQL using dump and restore</span></span>](concepts-migrate-dump-restore.md)
