---
title: "aaaConnect tooAzure databázi PostgreSQL pomocí Ruby | Microsoft Docs"
description: "Tento rychlý start poskytuje ukázka Ruby kódu můžete použít tooconnect a zadávat dotazy na data z databáze Azure pro PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: ruby
ms.topic: quickstart
ms.date: 06/30/2017
ms.openlocfilehash: 7a0c8c92023452b40ca19d76fa659744f3e9a236
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-ruby-tooconnect-and-query-data"></a>Azure databázi PostgreSQL: použití Ruby tooconnect a dotazování dat
Tento rychlý start předvádí jak tooconnect tooan Azure databázi PostgreSQL pomocí [Ruby](https://www.ruby-lang.org) aplikace. Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello. Tento článek předpokládá, že jste obeznámeni s vývojem pomocí Ruby, ale, že se nový tooworking s Azure databáze PostgreSQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:
- [Vytvoření databáze – portál](quickstart-create-server-database-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku Azure](quickstart-create-server-database-azure-cli.md)

## <a name="install-ruby"></a>Instalace Ruby
Nainstalujte Ruby na vlastní počítač. 

### <a name="windows"></a>Windows
- Stažení a instalace hello nejnovější verzi [Ruby](http://rubyinstaller.org/downloads/).
- Na hello dokončit obrazovky hello Instalační služby MSI, zaškrtněte políčko hello, která říká "spustit ridk nainstalovat tooinstall MSYS2 a nástrojů pro vývoj." Pak klikněte na tlačítko **Dokončit** toolaunch hello další instalační služby systému.
- spustí instalační program RubyInstaller2 pro Windows Hello. Zadejte 2 tooinstall hello MSYS2 úložiště aktualizací. Po dokončení a vrátí toohello výzva k instalaci, zavřete příkazové okno hello.
- Spuštění nového příkazového řádku (cmd) z nabídky Start hello.
- Test hello Ruby instalace `ruby -v` nainstalovaná verze toosee hello.
- Testování instalace Gem hello `gem -v` nainstalovaná verze toosee hello.
- Sestavení modulu hello PostgreSQL pro Ruby pomocí Gem spuštěním příkazu hello `gem install pg`.

### <a name="macos"></a>MacOS
- Instalace pomocí Homebrew spuštěním příkazu hello Ruby `brew install ruby`. Další možnosti instalace najdete v tématu hello Ruby [dokumentaci k instalaci](https://www.ruby-lang.org/en/documentation/installation/#homebrew)
- Test hello Ruby instalace `ruby -v` nainstalovaná verze toosee hello.
- Testování instalace Gem hello `gem -v` nainstalovaná verze toosee hello.
- Sestavení modulu hello PostgreSQL pro Ruby pomocí Gem spuštěním příkazu hello `gem install pg`.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- Nainstalujte Ruby spuštěním příkazu hello `sudo apt-get install ruby-full`. Další možnosti instalace najdete v tématu hello Ruby [instalace dokumentace](https://www.ruby-lang.org/en/documentation/installation/).
- Test hello Ruby instalace `ruby -v` nainstalovaná verze toosee hello.
- Nainstalujte nejnovější aktualizace hello pro Gem spuštěním příkazu hello `sudo gem update --system`.
- Testování instalace Gem hello `gem -v` nainstalovaná verze toosee hello.
- Nainstalujte hello RSZ, zkontrolujte a další nástroje sestavení spuštěním příkazu hello `sudo apt-get install build-essential`.
- Nainstalujte hello PostgreSQL knihovny spuštěním příkazu hello `sudo apt-get install libpq-dev`.
- Sestavení modulu Ruby pg hello pomocí Gem spuštěním příkazu hello `sudo gem install pg`.

## <a name="run-ruby-code"></a>Spuštění kódu Ruby 
- Uložte hello kódu do textového souboru a uložte soubor hello do složky projektu s .rb souboru rozšíření, jako například `C:\rubypostgres\read.rb` nebo`/home/username/rubypostgres/read.rb`
- Kód hello toorun, spusťte příkazový řádek hello nebo bash prostředí. Změnit adresář, do složky projektu `cd rubypostgres`, zadejte příkaz hello `ruby read.rb` toorun hello aplikace.

## <a name="get-connection-information"></a>Získání informací o připojení
Získáte hello připojení informace potřebné tooconnect toohello databáze Azure pro PostgreSQL. Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru, které jste vytvořili, například **mypgserver 20170401**.
3. Klikněte na název serveru hello **mypgserver 20170401**.
4. Vyberte hello serveru **přehled** stránky. Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.
 ![Azure Database for PostgreSQL – přihlášení správce serveru](./media/connect-ruby/1-connection-string.png)
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránky tooview hello serveru správce přihlašovací jméno. V případě potřeby resetovat heslo hello.

## <a name="connect-and-create-a-table"></a>Připojení a vytvoření tabulky
Použití hello následující kód tooconnect a vytvořte tabulku pomocí **CREATE TABLE** příkaz jazyka SQL, za nímž následuje **INSERT INTO** SQL příkazy tooadd řádků do tabulky hello.

Kód Hello používá [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt s konstruktor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure databázi PostgreSQL. Potom zavolá metodu [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello rozevírací, vytvořit tabulku a VLOŽTE do příkazy. Hello kód kontroluje chyby pomocí hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) třídy. Potom zavolá metodu [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello připojení předtím, než se ukončuje.

Nahraďte hello `host`, `database`, `user`, a `password` řetězce s vlastními hodnotami. 
```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection toodatabase'

    # Drop previous table of same name if one exists
    connection.exec('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    connection.exec('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    connection.exec("INSERT INTO inventory VALUES(1, 'banana', 150)")
    connection.exec("INSERT INTO inventory VALUES(2, 'orange', 154)")
    connection.exec("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="read-data"></a>Čtení dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL. 

Kód Hello používá [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt s konstruktor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure databázi PostgreSQL. Potom zavolá metodu [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) vyberte příkaz hello toorun, udržování hello výsledky sady výsledků dotazu. Hello výsledek sadu kolekce je vstupní oproti použití hello `resultSet.each do` ve smyčce, udržování hello aktuální hodnoty řádků v hello `row` proměnné. Hello kód kontroluje chyby pomocí hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) třídy. Potom zavolá metodu [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello připojení předtím, než se ukončuje.

Nahraďte hello `host`, `database`, `user`, a `password` řetězce s vlastními hodnotami. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :database => dbname, :port => '5432', :password => password)
    puts 'Successfully created connection toodatabase.'

    resultSet = connection.exec('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="update-data"></a>Aktualizace dat
Použití hello následující kód tooconnect a aktualizovat data pomocí hello **aktualizace** příkaz jazyka SQL.

Kód Hello používá [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt s konstruktor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure databázi PostgreSQL. Potom zavolá metodu [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello příkaz aktualizace. Hello kód kontroluje chyby pomocí hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) třídy. Potom zavolá metodu [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello připojení předtím, než se ukončuje.

Nahraďte hello `host`, `database`, `user`, a `password` řetězce s vlastními hodnotami. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection toodatabase.'

    # Modify some data in table.
    connection.exec('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
    puts 'Updated 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```


## <a name="delete-data"></a>Odstranění dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL. 

Kód Hello používá [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt s konstruktor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure databázi PostgreSQL. Potom zavolá metodu [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello příkaz aktualizace. Hello kód kontroluje chyby pomocí hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) třídy. Potom zavolá metodu [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello připojení předtím, než se ukončuje.

Nahraďte hello `host`, `database`, `user`, a `password` řetězce s vlastními hodnotami. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection toodatabase.'

    # Modify some data in table.
    connection.exec('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./howto-migrate-using-export-and-import.md)
