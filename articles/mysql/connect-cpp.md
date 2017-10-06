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
# <a name="azure-database-for-mysql-use-connectorc-tooconnect-and-query-data"></a>Azure databáze pro databázi MySQL: tooconnect a dotazování dat použití konektoru/C++
Tento rychlý start předvádí jak tooconnect tooan Azure databáze pro databázi MySQL pomocí aplikace C++. Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello. Hello postup v tomto článku předpokládá, že jste obeznámeni s vývojem pomocí C++, a že jste novou tooworking s Azure Database pro databázi MySQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

Budete také muset:
- Nainstalovat [.NET Framework](https://www.microsoft.com/net/download)
- Nainstalovat [Visual Studio](https://www.visualstudio.com/downloads/)
- Nainstalovat [MySQL Connector/C++](https://dev.mysql.com/downloads/connector/cpp/) 
- Nainstalovat [Boost](http://www.boost.org/)

## <a name="install-visual-studio-and-net"></a>Instalace sady Visual Studio a .NET
Hello kroky v této části předpokládají, že jste obeznámeni s vývojem pomocí rozhraní .NET.

### <a name="windows"></a>**Windows**
1. Nainstalujte sadu Visual Studio 2017 Community, což je plně vybavené, rozšiřitelné a bezplatné integrované vývojové prostředí (IDE) pro vytváření moderních aplikací pro Android, iOS a Windows, stejně jako webových a databázových aplikací a cloudových služeb. Můžete nainstalovat buď hello úplné rozhraní .NET Framework, nebo jenom .NET Core. fragmenty kódu Hello v hello rychlý start fungovat se. Pokud již máte nainstalovanou na počítači pro sadu Visual Studio, přeskočte následující dva kroky hello.
   - Stáhnout hello [instalační program Visual Studio 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15). 
   - Spusťte instalační program hello a postupujte podle hello instalace výzvy toocomplete hello instalace.

### <a name="configure-visual-studio"></a>**Konfigurace sady Visual Studio**
1. Ze sady Visual Studio projektu vlastnost > Vlastnosti konfigurace > C/C++ > linkeru > Obecné > adresáře další knihovny, přidejte adresář lib\opt hello (tj.: C:\Program Files (x86) \MySQL\MySQL konektor C++ 1.1.9\lib\opt) z hello c ++ konektor.
2. V sadě Visual Studio v části Vlastnosti projektu > Vlastnosti konfigurace > C/C++ > Obecné > Další adresáře k zahrnutí:
   - Přidejte adresář include/ konektoru C++ (tj.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\include\)
   - Přidejte kořenový adresář knihovny Boost (tj.: C:\boost_1_64_0\)
3. Ze sady Visual Studio projektu vlastnost > Vlastnosti konfigurace > C/C++ > linkeru > vstup > Další závislosti, do pole text hello, přidejte mysqlcppconn.lib
4. Buď mysqlcppconn.dll kopírování z hello c ++ konektor složku knihovny v kroku 3 toohello stejném adresáři jako spustitelný soubor aplikace hello nebo ji můžete přidat proměnnou prostředí toohello tak vaše aplikace k dispozici.

## <a name="get-connection-information"></a>Získání informací o připojení
Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL. Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru, které jste vytvořili, například **myserver4demo**.
3. Klikněte na název serveru hello.
4. Vyberte hello serveru **vlastnosti** stránky. Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.
 ![Název serveru Azure Database for MySQL](./media/connect-cpp/1_server-properties-name-login.png)
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.

## <a name="connect-create-table-and-insert-data"></a>Připojení, vytvoření tabulky a vložení dat
Použití hello následující kód tooconnect a načtení dat pomocí hello **CREATE TABLE** a **INSERT INTO** příkazů SQL. Kód Hello používá třídu sql::Driver s hello connect() metoda tooestablish tooMySQL připojení. Potom kód hello používá metoda createStatement() a execute() toorun hello databáze příkazy. 

Nahraďte hello hostitele, DBName, uživatele a heslo parametry hello hodnoty, kterou jste zadali při vytvoření hello server a databáze. 

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

## <a name="read-data"></a>Čtení dat

Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL. Kód Hello používá třídu sql::Driver s hello connect() metoda tooestablish tooMySQL připojení. Potom kód hello používá metoda prepareStatement() a executeQuery() toorun hello vyberte příkazy. Nakonec hello kód používá next() tooadvance toohello záznamy ve výsledcích hello. Potom kód hello používá getInt() a funkci getString() tooparse hello hodnoty v záznamu hello.

Nahraďte hello hostitele, DBName, uživatele a heslo parametry hello hodnoty, kterou jste zadali při vytvoření hello server a databáze. 

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

## <a name="update-data"></a>Aktualizace dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **aktualizace** příkaz jazyka SQL. Kód Hello používá třídu sql::Driver s hello connect() metoda tooestablish tooMySQL připojení. Potom kód hello používá metoda prepareStatement() a executeQuery() toorun hello příkazy aktualizace. 

Nahraďte hello hostitele, DBName, uživatele a heslo parametry hello hodnoty, kterou jste zadali při vytvoření hello server a databáze. 

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


## <a name="delete-data"></a>Odstranění dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL. Kód Hello používá třídu sql::Driver s hello connect() metoda tooestablish tooMySQL připojení. Potom kód hello používá metoda prepareStatement() executeQuery() toorun hello příkazy a delete.

Nahraďte hello hostitele, DBName, uživatele a heslo parametry hello hodnoty, kterou jste zadali při vytvoření hello server a databáze. 

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

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší tooAzure databáze MySQL databáze pro databázi MySQL pomocí výpisu a obnovení](concepts-migrate-dump-restore.md)
