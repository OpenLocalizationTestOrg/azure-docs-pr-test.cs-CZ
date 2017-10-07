---
title: "Připojit tooAzure databáze pro databázi MySQL pomocí přejděte | Microsoft Docs"
description: "Tento rychlý start poskytuje několik přejděte ukázky kódu, můžete použít tooconnect a zadávat dotazy na data z databáze Azure pro databázi MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: go
ms.topic: hero-article
ms.date: 07/18/2017
ms.openlocfilehash: e8067b807ee729e04850c5325f476806bcd54983
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-go-language-tooconnect-and-query-data"></a>Azure databáze pro databázi MySQL: přejít pomocí jazyka tooconnect a dotazování dat
Tento rychlý start předvádí jak tooconnect tooan databáze Azure pro používání MySQL kód napsaný v hello [přejděte](https://golang.org/) jazyka z systému macOS platformy Windows, Ubuntu Linux a Apple. Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello. Tento článek předpokládá, že jste obeznámeni s vývojem pomocí přejít, ale, že se nový tooworking s Azure Database pro databázi MySQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-go-and-mysql-connector"></a>Instalace jazyka Go a konektoru MySQL
Nainstalujte [přejděte](https://golang.org/doc/install) a hello [přejděte ovladače sql pro databázi MySQL](https://github.com/go-sql-driver/mysql#installation) na vlastním počítači. V závislosti na platformě postupujte podle kroků hello:

### <a name="windows"></a>Windows
1. [Stáhněte si](https://golang.org/dl/) a nainstalujte přejděte pro Microsoft Windows podle toohello [pokyny k instalaci](https://golang.org/doc/install).
2. Spusťte příkazový řádek hello z nabídky start hello.
3. Vytvořte složku pro váš projekt, například: `mkdir  %USERPROFILE%\go\src\mysqlgo`.
4. Změnit adresář, do složky hello projektu, například `cd %USERPROFILE%\go\src\mysqlgo`.
5. Nastavit proměnnou prostředí hello pro GOPATH toopoint toohello zdrojový kód adresář. `set GOPATH=%USERPROFILE%\go`.
6. Nainstalujte hello [přejděte ovladače sql pro databázi mysql](https://github.com/go-sql-driver/mysql#installation) spuštěním hello `go get github.com/go-sql-driver/mysql` příkaz.

   V souhrnu nainstalujte přejděte a potom spusťte tyto příkazy v příkazovém řádku hello:
   ```cmd
   mkdir  %USERPROFILE%\go\src\mysqlgo
   cd %USERPROFILE%\go\src\mysqlgo
   set GOPATH=%USERPROFILE%\go
   go get github.com/go-sql-driver/mysql
   ```

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Spusťte prostředí Bash hello. 
2. Nainstalujte jazyk Go spuštěním příkazu `sudo apt-get install golang-go`.
3. Ve svém domovském adresáři vytvořte složku pro projekt, například `mkdir -p ~/go/src/mysqlgo/`.
4. Změnit adresář, jako například do složky hello `cd ~/go/src/mysqlgo/`.
5. Sada hello GOPATH prostředí proměnné toopoint tooa platný zdrojový adresář, jako je vaše aktuální domovské adresáře přejít složku. V prostředí bash hello, spusťte `export GOPATH=~/go` tooadd hello přejděte directory jako hello GOPATH pro aktuální relaci prostředí hello.
6. Nainstalujte hello [přejděte ovladače sql pro databázi mysql](https://github.com/go-sql-driver/mysql#installation) spuštěním hello `go get github.com/go-sql-driver/mysql` příkaz.

   Stručně řečeno, spusťte tyto příkazy Bash:
   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

### <a name="apple-macos"></a>Apple macOS
1. Stáhněte a nainstalujte přejděte podle toohello [pokyny k instalaci](https://golang.org/doc/install) odpovídající vaši platformu. 
2. Spusťte prostředí Bash hello. 
3. Ve svém domovském adresáři vytvořte složku pro projekt, například `mkdir -p ~/go/src/mysqlgo/`.
4. Změnit adresář, jako například do složky hello `cd ~/go/src/mysqlgo/`.
5. Sada hello GOPATH prostředí proměnné toopoint tooa platný zdrojový adresář, jako je vaše aktuální domovské adresáře přejít složku. V prostředí bash hello, spusťte `export GOPATH=~/go` tooadd hello přejděte directory jako hello GOPATH pro aktuální relaci prostředí hello.
6. Nainstalujte hello [přejděte ovladače sql pro databázi mysql](https://github.com/go-sql-driver/mysql#installation) spuštěním hello `go get github.com/go-sql-driver/mysql` příkaz.

   Stručně řečeno, nainstalujte jazyk Go a potom spusťte tyto příkazy Bash:
   ```bash
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

## <a name="get-connection-information"></a>Získání informací o připojení
Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL. Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru můžete mít pokrčené, jako například **myserver4demo**.
3. Klikněte na název serveru hello **myserver4demo**.
4. Vyberte hello serveru **vlastnosti** stránky. Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.
 ![Azure Database for MySQL – přihlašovací jméno správce serveru](./media/connect-go/1_server-properties-name-login.png)
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.
   

## <a name="build-and-run-go-code"></a>Sestavení a spuštění kódu jazyka Go 
1. toowrite Golang kódu, můžete použít jednoduchý textový editor, například Poznámkový blok v systému Windows [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) nebo [Nano](https://www.nano-editor.org/) v Ubuntu nebo TextEdit v systému macOS. Pokud dáváte přednost bohatšímu integrovanému vývojovému prostředí (IDE), vyzkoušejte [Gogland](https://www.jetbrains.com/go/) od JetBrains, [Visual Studio Code](https://code.visualstudio.com/) od Microsoftu nebo [Atom](https://atom.io/).
2. Vložte hello kód přejít z hello oddílech do textových souborů a uložit do složky projektu s příponou \*.go, například Windows – cesta `%USERPROFILE%\go\src\mysqlgo\createtable.go` nebo Linux cestu `~/go/src/mysqlgo/createtable.go`.
3. Vyhledejte hello `HOST`, `DATABASE`, `USER`, a `PASSWORD` konstanty v hello kód a nahraďte hello ukázkové hodnoty vlastními hodnotami. 
4. Spusťte příkazový řádek hello nebo bash prostředí. Změňte adresář na složku vašeho projektu. Například ve Windows pomocí příkazu `cd %USERPROFILE%\go\src\mysqlgo\`. V Linuxu pomocí příkazu `cd ~/go/src/mysqlgo/`.  Některé editory hello IDE uvedená nabízí možnosti ladění a runtime bez nutnosti příkazů prostředí.
5. Spuštění kódu hello zadáním příkazu hello `go run createtable.go` toocompile hello aplikace a potom ho spusťte. 
6. Alternativně toobuild hello kód do nativní aplikace `go build createtable.go`, spusťte `createtable.exe` toorun hello aplikace.

## <a name="connect-create-table-and-insert-data"></a>Připojení, vytvoření tabulky a vložení dat
Použití hello následující kód serveru toohello tooconnect, vytvořit tabulku a načtení dat pomocí hello **vložit** příkaz jazyka SQL. 

Hello kód importuje tři balíčky: hello [balíček sql](https://golang.org/pkg/database/sql/), hello [ovladače přejděte sql pro databázi mysql](https://github.com/go-sql-driver/mysql#installation) jako ovladač toocommunicate s hello Azure Database pro databázi MySQL a hello [fmt balíček](https://golang.org/pkg/fmt/)pro tištěné vstup a výstup hello příkazového řádku.

Hello kód zavolá metodu [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure databáze MySQL a kontroly hello připojení pomocí metody [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). A [popisovač databáze](https://golang.org/pkg/database/sql/#DB) se používá napříč, která uchovává hello fondu připojení pro server databáze hello. Hello kód volání hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoda několikrát toorun několik příkazy DDL. Hello kód také používá hello [Prepare()](http://go-database-sql.org/prepared.html) a Exec() toorun připravené příkazy s odlišnými parametry tooinsert tři řádky. Pokaždé, když metoda vlastní checkError() je použité toocheck, pokud došlo k chybě a mít obavy tooexit.

Nahraďte hello `host`, `database`, `user`, a `password` konstanty vlastními hodnotami. 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase.")

    // Drop previous table of same name if one exists.
    _, err = db.Exec("DROP TABLE IF EXISTS inventory;")
    checkError(err)
    fmt.Println("Finished dropping table (if existed).")

    // Create table.
    _, err = db.Exec("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
    checkError(err)
    fmt.Println("Finished creating table.")

    // Insert some data into table.
    sqlStatement, err := db.Prepare("INSERT INTO inventory (name, quantity) VALUES (?, ?);")
    res, err := sqlStatement.Exec("banana", 150)
    checkError(err)
    rowCount, err := res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)

    res, err = sqlStatement.Exec("orange", 154)
    checkError(err)
    rowCount, err = res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)

    res, err = sqlStatement.Exec("apple", 100)
    checkError(err)
    rowCount, err = res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}

```

## <a name="read-data"></a>Čtení dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL. 

Hello kód importuje tři balíčky: hello [balíček sql](https://golang.org/pkg/database/sql/), hello [ovladače přejděte sql pro databázi mysql](https://github.com/go-sql-driver/mysql#installation) jako ovladač toocommunicate s hello Azure Database pro databázi MySQL a hello [fmt balíček](https://golang.org/pkg/fmt/)pro tištěné vstup a výstup hello příkazového řádku.

Hello kód zavolá metodu [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure databáze MySQL a kontroly hello připojení pomocí metody [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). A [popisovač databáze](https://golang.org/pkg/database/sql/#DB) se používá napříč, která uchovává hello fondu připojení pro server databáze hello. Hello kód volání hello [Query()](https://golang.org/pkg/database/sql/#DB.Query) metoda toorun hello vyberte příkaz. Pak spustí [Next()](https://golang.org/pkg/database/sql/#Rows.Next) tooiterate prostřednictvím sadu výsledků hello a [Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) tooparse hodnoty ve sloupcích hello, ukládání hello hodnotu do proměnné. Pokaždé, když metoda vlastní checkError() je použité toocheck, pokud došlo k chybě a mít obavy tooexit.

Nahraďte hello `host`, `database`, `user`, a `password` konstanty vlastními hodnotami. 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase.")

    // Variables for printing column data when scanned.
    var (
        id       int
        name     string
        quantity int
    )

    // Read some data from hello table.
    rows, err := db.Query("SELECT id, name, quantity from inventory;")
    checkError(err)
    defer rows.Close()
    fmt.Println("Reading data:")
    for rows.Next() {
        err := rows.Scan(&id, &name, &quantity)
        checkError(err)
        fmt.Printf("Data row = (%d, %s, %d)\n", id, name, quantity)
    }
    err = rows.Err()
    checkError(err)
    fmt.Println("Done.")
}
```

## <a name="update-data"></a>Aktualizace dat
Použití hello následující kód tooconnect a aktualizovat data pomocí hello **aktualizace** příkaz jazyka SQL. 

Hello kód importuje tři balíčky: hello [balíček sql](https://golang.org/pkg/database/sql/), hello [ovladače přejděte sql pro databázi mysql](https://github.com/go-sql-driver/mysql#installation) jako ovladač toocommunicate s hello Azure Database pro databázi MySQL a hello [fmt balíček](https://golang.org/pkg/fmt/)pro tištěné vstup a výstup hello příkazového řádku.

Hello kód zavolá metodu [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure databáze MySQL a kontroly hello připojení pomocí metody [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). A [popisovač databáze](https://golang.org/pkg/database/sql/#DB) se používá napříč, která uchovává hello fondu připojení pro server databáze hello. Hello kód volání hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) příkaz metoda toorun hello aktualizace. Pokaždé, když metoda vlastní checkError() je použité toocheck, pokud došlo k chybě a mít obavy tooexit.

Nahraďte hello `host`, `database`, `user`, a `password` konstanty vlastními hodnotami. 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase.")

    // Modify some data in table.
    rows, err := db.Exec("UPDATE inventory SET quantity = ? WHERE name = ?", 200, "banana")
    checkError(err)
    rowCount, err := rows.RowsAffected()
    fmt.Printf("Deleted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}
```

## <a name="delete-data"></a>Odstranění dat
Použití hello následující kód tooconnect a odebrat pomocí data **odstranit** příkaz jazyka SQL. 

Hello kód importuje tři balíčky: hello [balíček sql](https://golang.org/pkg/database/sql/), hello [ovladače přejděte sql pro databázi mysql](https://github.com/go-sql-driver/mysql#installation) jako ovladač toocommunicate s hello Azure Database pro databázi MySQL a hello [fmt balíček](https://golang.org/pkg/fmt/)pro tištěné vstup a výstup hello příkazového řádku.

Hello kód zavolá metodu [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure databáze MySQL a kontroly hello připojení pomocí metody [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). A [popisovač databáze](https://golang.org/pkg/database/sql/#DB) se používá napříč, která uchovává hello fondu připojení pro server databáze hello. Hello kód volání hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoda toorun hello odstranit příkaz. Pokaždé, když metoda vlastní checkError() je použité toocheck, pokud došlo k chybě a mít obavy tooexit.

Nahraďte hello `host`, `database`, `user`, a `password` konstanty vlastními hodnotami. 

```Go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase.")

    // Modify some data in table.
    rows, err := db.Exec("DELETE FROM inventory WHERE name = ?", "orange")
    checkError(err)
    rowCount, err := rows.RowsAffected()
    fmt.Printf("Deleted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}
```

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./concepts-migrate-import-export.md)
