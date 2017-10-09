---
title: "Připojit tooAzure databáze pro databázi MySQL pomocí Ruby | Microsoft Docs"
description: "Tento rychlý start poskytuje několik ukázky Ruby kódu můžete použít tooconnect a zadávat dotazy na data z databáze Azure pro databázi MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: ruby
ms.topic: hero-article
ms.date: 07/13/2017
ms.openlocfilehash: ff0880dcc24e96f467c9092bc663ce3dc4c2637a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-ruby-tooconnect-and-query-data"></a>Azure databáze pro databázi MySQL: použití Ruby tooconnect a dotazování dat
Tento rychlý start předvádí jak tooconnect tooan Azure databáze pro používání MySQL [Ruby](https://www.ruby-lang.org) aplikace a hello [mysql2](https://rubygems.org/gems/mysql2) gem z platformy Windows, Mac a Ubuntu Linux. Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello. Tento článek předpokládá, že jste obeznámeni s vývojem pomocí Ruby, ale, že se nový tooworking s Azure Database pro databázi MySQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-ruby"></a>Instalace Ruby
Instalace Ruby, Gem a knihovna MySQL2 hello na vlastním počítači. 

### <a name="windows"></a>Windows
1. Stáhnout a nainstalovat verzi hello 2.3 [Ruby](http://rubyinstaller.org/downloads/).
2. Spuštění nového příkazového řádku (cmd) z nabídky Start hello.
3. Změnit adresář, do hello Ruby adresář pro verzi 2.3. `cd c:\Ruby23-x64\bin`
4. Test hello Ruby instalaci spuštěním příkazu hello `ruby -v` nainstalovaná verze toosee hello.
5. Otestovat hello Gem instalaci spuštěním příkazu hello `gem -v` nainstalovaná verze toosee hello.
6. Sestavení modulu hello Mysql2 pro Ruby pomocí Gem spuštěním příkazu hello `gem install mysql2`.

### <a name="macos"></a>MacOS
1. Instalace pomocí Homebrew spuštěním příkazu hello Ruby `brew install ruby`. Další možnosti instalace najdete v tématu hello Ruby [instalace dokumentace](https://www.ruby-lang.org/en/documentation/installation/#homebrew).
2. Test hello Ruby instalaci spuštěním příkazu hello `ruby -v` nainstalovaná verze toosee hello.
3. Otestovat hello Gem instalaci spuštěním příkazu hello `gem -v` nainstalovaná verze toosee hello.
4. Sestavení modulu hello Mysql2 pro Ruby pomocí Gem spuštěním příkazu hello `gem install mysql2`.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Nainstalujte Ruby spuštěním příkazu hello `sudo apt-get install ruby-full`. Další možnosti instalace najdete v tématu hello Ruby [instalace dokumentace](https://www.ruby-lang.org/en/documentation/installation/).
2. Test hello Ruby instalaci spuštěním příkazu hello `ruby -v` nainstalovaná verze toosee hello.
3. Nainstalujte nejnovější aktualizace hello pro Gem spuštěním příkazu hello `sudo gem update --system`.
4. Otestovat hello Gem instalaci spuštěním příkazu hello `gem -v` nainstalovaná verze toosee hello.
5. Nainstalujte hello RSZ, zkontrolujte a další nástroje sestavení spuštěním příkazu hello `sudo apt-get install build-essential`.
6. Nainstalujte knihovny vývojáře klienta MySQL hello spuštěním příkazu hello `sudo apt-get install libmysqlclient-dev`.
7. Sestavení modulu hello mysql2 pro Ruby pomocí Gem spuštěním příkazu hello `sudo gem install mysql2`.

## <a name="get-connection-information"></a>Získání informací o připojení
Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL. Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru můžete mít pokrčené, jako například **myserver4demo**.
3. Klikněte na název serveru hello **myserver4demo**.
4. Vyberte hello serveru **vlastnosti** stránky. Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.
 ![Azure Database for MySQL – přihlašovací jméno správce serveru](./media/connect-ruby/1_server-properties-name-login.png)
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.

## <a name="run-ruby-code"></a>Spuštění kódu Ruby 
1. Vložte hello Ruby kód z hello oddílech do textových souborů a uložit hello soubory do složky projektu s .rb souboru rozšíření, jako je například `C:\rubymysql\createtable.rb` nebo `/home/username/rubymysql/createtable.rb`.
2. Kód hello toorun, spusťte příkazový řádek hello nebo bash prostředí. Změňte adresář na složku vašeho projektu pomocí příkazu `cd rubymysql`.
3. Zadejte příkaz hello ruby a potom podle názvu souboru text hello, jako například `ruby createtable.rb` toorun hello aplikace.
4. Na hello operačního systému Windows Pokud aplikace hello ruby není ve vašem prostředí PATH může být nutné toouse hello úplná cesta toolaunch hello uzel aplikace, jako například`"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`

## <a name="connect-and-create-a-table"></a>Připojení a vytvoření tabulky
Použití hello následující kód tooconnect a vytvořte tabulku pomocí **CREATE TABLE** příkaz jazyka SQL, za nímž následuje **INSERT INTO** SQL příkazy tooadd řádků do tabulky hello.

Kód Hello používá [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) třídy .new() metoda tooconnect tooAzure databáze pro databázi MySQL. Potom zavolá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) příkazů DROP, CREATE TABLE a INSERT INTO toorun hello několikrát. Potom zavolá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello připojení předtím, než se ukončuje.

Nahraďte hello `host`, `database`, `username`, a `password` řetězce s vlastními hodnotami. 
```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Drop previous table of same name if one exists
    client.query('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    client.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    client.query("INSERT INTO inventory VALUES(1, 'banana', 150)")
    client.query("INSERT INTO inventory VALUES(2, 'orange', 154)")
    client.query("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="read-data"></a>Čtení dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL. 

Kód Hello používá [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) třídy .new() metoda tooconnect tooAzure databáze pro databázi MySQL. Potom zavolá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello vyberte příkazy. Potom zavolá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello připojení předtím, než se ukončuje.

Nahraďte hello `host`, `database`, `username`, a `password` řetězce s vlastními hodnotami. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Read data
    resultSet = client.query('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end
    puts 'Read ' + resultSet.count.to_s + ' row(s).'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="update-data"></a>Aktualizace dat
Použití hello následující kód tooconnect a aktualizovat data pomocí hello **aktualizace** příkaz jazyka SQL.

Kód Hello používá [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) třídy .new() metoda tooconnect tooAzure databáze pro databázi MySQL. Potom zavolá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello aktualizovat příkazy. Potom zavolá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello připojení předtím, než se ukončuje.

Nahraďte hello `host`, `database`, `username`, a `password` řetězce s vlastními hodnotami. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Update data
   client.query('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
   puts 'Updated 1 row of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```


## <a name="delete-data"></a>Odstranění dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL. 

Kód Hello používá [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) třídy .new() metoda tooconnect tooAzure databáze pro databázi MySQL. Potom zavolá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello odstranění příkazy. Potom zavolá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello připojení předtím, než se ukončuje.

Nahraďte hello `host`, `database`, `username`, a `password` řetězce s vlastními hodnotami. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Delete data
    resultSet = client.query('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./concepts-migrate-import-export.md)
