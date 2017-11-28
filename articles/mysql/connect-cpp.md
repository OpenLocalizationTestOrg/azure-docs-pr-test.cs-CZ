---
title: "Připojit tooAzure databáze pro databázi MySQL z jazyka C++ | Microsoft Docs"
description: "Tento rychlý start poskytuje ukázka kódu C++ pomocí tooconnect a zadávat dotazy na data z databáze Azure pro databázi MySQL."
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
ms.openlocfilehash: d027597bf02b1eacab9b8808957399f6e54e63cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-connectorc-tooconnect-and-query-data"></a><span data-ttu-id="94a40-103">Azure databáze pro databázi MySQL: tooconnect a dotazování dat použití konektoru/C++</span><span class="sxs-lookup"><span data-stu-id="94a40-103">Azure Database for MySQL: Use Connector/C++ tooconnect and query data</span></span>
<span data-ttu-id="94a40-104">Tento rychlý start předvádí jak tooconnect tooan Azure databáze pro databázi MySQL pomocí aplikace C++.</span><span class="sxs-lookup"><span data-stu-id="94a40-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using a C++ application.</span></span> <span data-ttu-id="94a40-105">Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="94a40-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="94a40-106">Hello postup v tomto článku předpokládá, že jste obeznámeni s vývojem pomocí C++, a že jste novou tooworking s Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="94a40-106">hello steps in this article assume that you are familiar with developing using C++, and that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="94a40-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="94a40-107">Prerequisites</span></span>
<span data-ttu-id="94a40-108">Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:</span><span class="sxs-lookup"><span data-stu-id="94a40-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="94a40-109">Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="94a40-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="94a40-110">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="94a40-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="94a40-111">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="94a40-111">You also need to:</span></span>
- <span data-ttu-id="94a40-112">Nainstalovat [.NET Framework](https://www.microsoft.com/net/download)</span><span class="sxs-lookup"><span data-stu-id="94a40-112">Install [.NET Framework](https://www.microsoft.com/net/download)</span></span>
- <span data-ttu-id="94a40-113">Nainstalovat [Visual Studio](https://www.visualstudio.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="94a40-113">Install [Visual Studio](https://www.visualstudio.com/downloads/)</span></span>
- <span data-ttu-id="94a40-114">Nainstalovat [MySQL Connector/C++](https://dev.mysql.com/downloads/connector/cpp/)</span><span class="sxs-lookup"><span data-stu-id="94a40-114">Install [MySQL Connector/C++](https://dev.mysql.com/downloads/connector/cpp/)</span></span> 
- <span data-ttu-id="94a40-115">Nainstalovat [Boost](http://www.boost.org/)</span><span class="sxs-lookup"><span data-stu-id="94a40-115">Install [Boost](http://www.boost.org/)</span></span>

## <a name="install-visual-studio-and-net"></a><span data-ttu-id="94a40-116">Instalace sady Visual Studio a .NET</span><span class="sxs-lookup"><span data-stu-id="94a40-116">Install Visual Studio and .NET</span></span>
<span data-ttu-id="94a40-117">Hello kroky v této části předpokládají, že jste obeznámeni s vývojem pomocí rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="94a40-117">hello steps in this section assume that you are familiar with developing using .NET.</span></span>

### <a name="windows"></a><span data-ttu-id="94a40-118">**Windows**</span><span class="sxs-lookup"><span data-stu-id="94a40-118">**Windows**</span></span>
1. <span data-ttu-id="94a40-119">Nainstalujte sadu Visual Studio 2017 Community, což je plně vybavené, rozšiřitelné a bezplatné integrované vývojové prostředí (IDE) pro vytváření moderních aplikací pro Android, iOS a Windows, stejně jako webových a databázových aplikací a cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="94a40-119">Install Visual Studio 2017 Community, which is a full featured, extensible, free IDE for creating modern applications for Android, iOS, Windows, as well as web & database applications and cloud services.</span></span> <span data-ttu-id="94a40-120">Můžete nainstalovat buď hello úplné rozhraní .NET Framework, nebo jenom .NET Core.</span><span class="sxs-lookup"><span data-stu-id="94a40-120">You can install either hello full .NET Framework or just .NET Core.</span></span> <span data-ttu-id="94a40-121">fragmenty kódu Hello v hello rychlý start fungovat se.</span><span class="sxs-lookup"><span data-stu-id="94a40-121">hello code snippets in hello Quickstart work with either.</span></span> <span data-ttu-id="94a40-122">Pokud již máte nainstalovanou na počítači pro sadu Visual Studio, přeskočte následující dva kroky hello.</span><span class="sxs-lookup"><span data-stu-id="94a40-122">If you already have Visual Studio installed on your machine, skip hello next two steps.</span></span>
   - <span data-ttu-id="94a40-123">Stáhnout hello [instalační program Visual Studio 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).</span><span class="sxs-lookup"><span data-stu-id="94a40-123">Download hello [Visual Studio 2017 installer](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).</span></span> 
   - <span data-ttu-id="94a40-124">Spusťte instalační program hello a postupujte podle hello instalace výzvy toocomplete hello instalace.</span><span class="sxs-lookup"><span data-stu-id="94a40-124">Run hello installer and follow hello installation prompts toocomplete hello installation.</span></span>

### <a name="configure-visual-studio"></a><span data-ttu-id="94a40-125">**Konfigurace sady Visual Studio**</span><span class="sxs-lookup"><span data-stu-id="94a40-125">**Configure Visual Studio**</span></span>
1. <span data-ttu-id="94a40-126">Ze sady Visual Studio projektu vlastnost > Vlastnosti konfigurace > C/C++ > linkeru > Obecné > adresáře další knihovny, přidejte adresář lib\opt hello (tj.: C:\Program Files (x86) \MySQL\MySQL konektor C++ 1.1.9\lib\opt) z hello c ++ konektor.</span><span class="sxs-lookup"><span data-stu-id="94a40-126">From Visual Studio, project property > configuration properties > C/C++ > linker > general > additional library directories, add hello lib\opt directory (i.e.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\lib\opt) of hello c++ connector.</span></span>
2. <span data-ttu-id="94a40-127">V sadě Visual Studio v části Vlastnosti projektu > Vlastnosti konfigurace > C/C++ > Obecné > Další adresáře k zahrnutí:</span><span class="sxs-lookup"><span data-stu-id="94a40-127">From Visual Studio, project property > configuration properties > C/C++ > general > additional include directories</span></span>
   - <span data-ttu-id="94a40-128">Přidejte adresář include/ konektoru C++ (tj.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\include\)</span><span class="sxs-lookup"><span data-stu-id="94a40-128">Add include/ directory of c++ connector (i.e.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\include\)</span></span>
   - <span data-ttu-id="94a40-129">Přidejte kořenový adresář knihovny Boost (tj.: C:\boost_1_64_0\)</span><span class="sxs-lookup"><span data-stu-id="94a40-129">Add Boost library's root directory (i.e.: C:\boost_1_64_0\)</span></span>
3. <span data-ttu-id="94a40-130">Ze sady Visual Studio projektu vlastnost > Vlastnosti konfigurace > C/C++ > linkeru > vstup > Další závislosti, do pole text hello, přidejte mysqlcppconn.lib</span><span class="sxs-lookup"><span data-stu-id="94a40-130">From Visual Studio, project property > configuration properties > C/C++ > linker > Input > Additional Dependencies, add mysqlcppconn.lib into hello text field</span></span>
4. <span data-ttu-id="94a40-131">Buď mysqlcppconn.dll kopírování z hello c ++ konektor složku knihovny v kroku 3 toohello stejném adresáři jako spustitelný soubor aplikace hello nebo ji můžete přidat proměnnou prostředí toohello tak vaše aplikace k dispozici.</span><span class="sxs-lookup"><span data-stu-id="94a40-131">Either copy mysqlcppconn.dll from hello c++ connector library folder in step 3 toohello same directory as hello application executable or add it toohello environment variable so your application can find it.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="94a40-132">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="94a40-132">Get connection information</span></span>
<span data-ttu-id="94a40-133">Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="94a40-133">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="94a40-134">Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="94a40-134">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="94a40-135">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="94a40-135">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="94a40-136">Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru, které jste vytvořili, například **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="94a40-136">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="94a40-137">Klikněte na název serveru hello.</span><span class="sxs-lookup"><span data-stu-id="94a40-137">Click hello server name.</span></span>
4. <span data-ttu-id="94a40-138">Vyberte hello serveru **vlastnosti** stránky.</span><span class="sxs-lookup"><span data-stu-id="94a40-138">Select hello server's **Properties** page.</span></span> <span data-ttu-id="94a40-139">Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.</span><span class="sxs-lookup"><span data-stu-id="94a40-139">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="94a40-140">![Název serveru Azure Database for MySQL](./media/connect-cpp/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="94a40-140">![Azure Database for MySQL server name](./media/connect-cpp/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="94a40-141">Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="94a40-141">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="94a40-142">Připojení, vytvoření tabulky a vložení dat</span><span class="sxs-lookup"><span data-stu-id="94a40-142">Connect, create table, and insert data</span></span>
<span data-ttu-id="94a40-143">Použití hello následující kód tooconnect a načtení dat pomocí hello **CREATE TABLE** a **INSERT INTO** příkazů SQL.</span><span class="sxs-lookup"><span data-stu-id="94a40-143">Use hello following code tooconnect and load hello data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span> <span data-ttu-id="94a40-144">Kód Hello používá třídu sql::Driver s hello connect() metoda tooestablish tooMySQL připojení.</span><span class="sxs-lookup"><span data-stu-id="94a40-144">hello code uses sql::Driver class with hello connect() method tooestablish a connection tooMySQL.</span></span> <span data-ttu-id="94a40-145">Potom kód hello používá metoda createStatement() a execute() toorun hello databáze příkazy.</span><span class="sxs-lookup"><span data-stu-id="94a40-145">Then hello code uses method createStatement() and execute() toorun hello database commands.</span></span> 

<span data-ttu-id="94a40-146">Nahraďte hello hostitele, DBName, uživatele a heslo parametry hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.</span><span class="sxs-lookup"><span data-stu-id="94a40-146">Replace hello Host, DBName, User, and Password parameters with hello values that you specified when you created hello server and database.</span></span> 

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
        //for demonstration only. never save password in hello code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect toodatabase. Error message: " << e.what() << endl;
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

## <a name="read-data"></a><span data-ttu-id="94a40-147">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="94a40-147">Read data</span></span>

<span data-ttu-id="94a40-148">Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="94a40-148">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="94a40-149">Kód Hello používá třídu sql::Driver s hello connect() metoda tooestablish tooMySQL připojení.</span><span class="sxs-lookup"><span data-stu-id="94a40-149">hello code uses sql::Driver class with hello connect() method tooestablish a connection tooMySQL.</span></span> <span data-ttu-id="94a40-150">Potom kód hello používá metoda prepareStatement() a executeQuery() toorun hello vyberte příkazy.</span><span class="sxs-lookup"><span data-stu-id="94a40-150">Then hello code uses method prepareStatement() and executeQuery() toorun hello select commands.</span></span> <span data-ttu-id="94a40-151">Nakonec hello kód používá next() tooadvance toohello záznamy ve výsledcích hello.</span><span class="sxs-lookup"><span data-stu-id="94a40-151">Finally hello code uses next() tooadvance toohello records in hello results.</span></span> <span data-ttu-id="94a40-152">Potom kód hello používá getInt() a funkci getString() tooparse hello hodnoty v záznamu hello.</span><span class="sxs-lookup"><span data-stu-id="94a40-152">Then hello code uses getInt() and getString() tooparse hello values in hello record.</span></span>

<span data-ttu-id="94a40-153">Nahraďte hello hostitele, DBName, uživatele a heslo parametry hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.</span><span class="sxs-lookup"><span data-stu-id="94a40-153">Replace hello Host, DBName, User, and Password parameters with hello values that you specified when you created hello server and database.</span></span> 

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
        //for demonstration only. never save password in hello code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect toodatabase. Error message: " << e.what() << endl;
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

## <a name="update-data"></a><span data-ttu-id="94a40-154">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="94a40-154">Update data</span></span>
<span data-ttu-id="94a40-155">Použití hello následující kód tooconnect a čtení dat pomocí hello **aktualizace** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="94a40-155">Use hello following code tooconnect and read hello data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="94a40-156">Kód Hello používá třídu sql::Driver s hello connect() metoda tooestablish tooMySQL připojení.</span><span class="sxs-lookup"><span data-stu-id="94a40-156">hello code uses sql::Driver class with hello connect() method tooestablish a connection tooMySQL.</span></span> <span data-ttu-id="94a40-157">Potom kód hello používá metoda prepareStatement() a executeQuery() toorun hello příkazy aktualizace.</span><span class="sxs-lookup"><span data-stu-id="94a40-157">Then hello code uses method prepareStatement() and executeQuery() toorun hello update commands.</span></span> 

<span data-ttu-id="94a40-158">Nahraďte hello hostitele, DBName, uživatele a heslo parametry hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.</span><span class="sxs-lookup"><span data-stu-id="94a40-158">Replace hello Host, DBName, User, and Password parameters with hello values that you specified when you created hello server and database.</span></span> 

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
        //for demonstration only. never save password in hello code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect toodatabase. Error message: " << e.what() << endl;
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


## <a name="delete-data"></a><span data-ttu-id="94a40-159">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="94a40-159">Delete data</span></span>
<span data-ttu-id="94a40-160">Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="94a40-160">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> <span data-ttu-id="94a40-161">Kód Hello používá třídu sql::Driver s hello connect() metoda tooestablish tooMySQL připojení.</span><span class="sxs-lookup"><span data-stu-id="94a40-161">hello code uses sql::Driver class with hello connect() method tooestablish a connection tooMySQL.</span></span> <span data-ttu-id="94a40-162">Potom kód hello používá metoda prepareStatement() executeQuery() toorun hello příkazy a delete.</span><span class="sxs-lookup"><span data-stu-id="94a40-162">Then hello code uses method prepareStatement() and executeQuery() toorun hello delete commands.</span></span>

<span data-ttu-id="94a40-163">Nahraďte hello hostitele, DBName, uživatele a heslo parametry hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.</span><span class="sxs-lookup"><span data-stu-id="94a40-163">Replace hello Host, DBName, User, and Password parameters with hello values that you specified when you created hello server and database.</span></span> 

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
        //for demonstration only. never save password in hello code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect toodatabase. Error message: " << e.what() << endl;
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

## <a name="next-steps"></a><span data-ttu-id="94a40-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="94a40-164">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="94a40-165">Migrace vaší tooAzure databáze MySQL databáze pro databázi MySQL pomocí výpisu a obnovení</span><span class="sxs-lookup"><span data-stu-id="94a40-165">Migrate your MySQL database tooAzure Database for MySQL using dump and restore</span></span>](concepts-migrate-dump-restore.md)
