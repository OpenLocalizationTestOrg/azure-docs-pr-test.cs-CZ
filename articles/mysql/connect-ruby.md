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
# <a name="azure-database-for-mysql-use-ruby-tooconnect-and-query-data"></a><span data-ttu-id="d72c8-103">Azure databáze pro databázi MySQL: použití Ruby tooconnect a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="d72c8-103">Azure Database for MySQL: Use Ruby tooconnect and query data</span></span>
<span data-ttu-id="d72c8-104">Tento rychlý start předvádí jak tooconnect tooan Azure databáze pro používání MySQL [Ruby](https://www.ruby-lang.org) aplikace a hello [mysql2](https://rubygems.org/gems/mysql2) gem z platformy Windows, Mac a Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="d72c8-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using a [Ruby](https://www.ruby-lang.org) application and hello [mysql2](https://rubygems.org/gems/mysql2) gem from Windows, Ubuntu Linux, and Mac platforms.</span></span> <span data-ttu-id="d72c8-105">Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="d72c8-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="d72c8-106">Tento článek předpokládá, že jste obeznámeni s vývojem pomocí Ruby, ale, že se nový tooworking s Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="d72c8-106">This article assumes you are familiar with development using Ruby, but that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d72c8-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d72c8-107">Prerequisites</span></span>
<span data-ttu-id="d72c8-108">Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:</span><span class="sxs-lookup"><span data-stu-id="d72c8-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="d72c8-109">Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d72c8-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="d72c8-110">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d72c8-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-ruby"></a><span data-ttu-id="d72c8-111">Instalace Ruby</span><span class="sxs-lookup"><span data-stu-id="d72c8-111">Install Ruby</span></span>
<span data-ttu-id="d72c8-112">Instalace Ruby, Gem a knihovna MySQL2 hello na vlastním počítači.</span><span class="sxs-lookup"><span data-stu-id="d72c8-112">Install Ruby, Gem, and hello MySQL2 library on your own machine.</span></span> 

### <a name="windows"></a><span data-ttu-id="d72c8-113">Windows</span><span class="sxs-lookup"><span data-stu-id="d72c8-113">Windows</span></span>
1. <span data-ttu-id="d72c8-114">Stáhnout a nainstalovat verzi hello 2.3 [Ruby](http://rubyinstaller.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="d72c8-114">Download and Install hello 2.3 version of [Ruby](http://rubyinstaller.org/downloads/).</span></span>
2. <span data-ttu-id="d72c8-115">Spuštění nového příkazového řádku (cmd) z nabídky Start hello.</span><span class="sxs-lookup"><span data-stu-id="d72c8-115">Launch a new command prompt (cmd) from hello Start menu.</span></span>
3. <span data-ttu-id="d72c8-116">Změnit adresář, do hello Ruby adresář pro verzi 2.3.</span><span class="sxs-lookup"><span data-stu-id="d72c8-116">Change directory into hello Ruby directory for version 2.3.</span></span> `cd c:\Ruby23-x64\bin`
4. <span data-ttu-id="d72c8-117">Test hello Ruby instalaci spuštěním příkazu hello `ruby -v` nainstalovaná verze toosee hello.</span><span class="sxs-lookup"><span data-stu-id="d72c8-117">Test hello Ruby installation by running hello command `ruby -v` toosee hello version installed.</span></span>
5. <span data-ttu-id="d72c8-118">Otestovat hello Gem instalaci spuštěním příkazu hello `gem -v` nainstalovaná verze toosee hello.</span><span class="sxs-lookup"><span data-stu-id="d72c8-118">Test hello Gem installation by running hello command `gem -v` toosee hello version installed.</span></span>
6. <span data-ttu-id="d72c8-119">Sestavení modulu hello Mysql2 pro Ruby pomocí Gem spuštěním příkazu hello `gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="d72c8-119">Build hello Mysql2 module for Ruby using Gem by running hello command `gem install mysql2`.</span></span>

### <a name="macos"></a><span data-ttu-id="d72c8-120">MacOS</span><span class="sxs-lookup"><span data-stu-id="d72c8-120">MacOS</span></span>
1. <span data-ttu-id="d72c8-121">Instalace pomocí Homebrew spuštěním příkazu hello Ruby `brew install ruby`.</span><span class="sxs-lookup"><span data-stu-id="d72c8-121">Install Ruby using Homebrew by running hello command `brew install ruby`.</span></span> <span data-ttu-id="d72c8-122">Další možnosti instalace najdete v tématu hello Ruby [instalace dokumentace](https://www.ruby-lang.org/en/documentation/installation/#homebrew).</span><span class="sxs-lookup"><span data-stu-id="d72c8-122">For more installation options, see hello Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/#homebrew).</span></span>
2. <span data-ttu-id="d72c8-123">Test hello Ruby instalaci spuštěním příkazu hello `ruby -v` nainstalovaná verze toosee hello.</span><span class="sxs-lookup"><span data-stu-id="d72c8-123">Test hello Ruby installation by running hello command `ruby -v` toosee hello version installed.</span></span>
3. <span data-ttu-id="d72c8-124">Otestovat hello Gem instalaci spuštěním příkazu hello `gem -v` nainstalovaná verze toosee hello.</span><span class="sxs-lookup"><span data-stu-id="d72c8-124">Test hello Gem installation by running hello command `gem -v` toosee hello version installed.</span></span>
4. <span data-ttu-id="d72c8-125">Sestavení modulu hello Mysql2 pro Ruby pomocí Gem spuštěním příkazu hello `gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="d72c8-125">Build hello Mysql2 module for Ruby using Gem by running hello command `gem install mysql2`.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="d72c8-126">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="d72c8-126">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="d72c8-127">Nainstalujte Ruby spuštěním příkazu hello `sudo apt-get install ruby-full`.</span><span class="sxs-lookup"><span data-stu-id="d72c8-127">Install Ruby by running hello command `sudo apt-get install ruby-full`.</span></span> <span data-ttu-id="d72c8-128">Další možnosti instalace najdete v tématu hello Ruby [instalace dokumentace](https://www.ruby-lang.org/en/documentation/installation/).</span><span class="sxs-lookup"><span data-stu-id="d72c8-128">For more installation options, see hello Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/).</span></span>
2. <span data-ttu-id="d72c8-129">Test hello Ruby instalaci spuštěním příkazu hello `ruby -v` nainstalovaná verze toosee hello.</span><span class="sxs-lookup"><span data-stu-id="d72c8-129">Test hello Ruby installation by running hello command `ruby -v` toosee hello version installed.</span></span>
3. <span data-ttu-id="d72c8-130">Nainstalujte nejnovější aktualizace hello pro Gem spuštěním příkazu hello `sudo gem update --system`.</span><span class="sxs-lookup"><span data-stu-id="d72c8-130">Install hello latest updates for Gem by running hello command `sudo gem update --system`.</span></span>
4. <span data-ttu-id="d72c8-131">Otestovat hello Gem instalaci spuštěním příkazu hello `gem -v` nainstalovaná verze toosee hello.</span><span class="sxs-lookup"><span data-stu-id="d72c8-131">Test hello Gem installation by running hello command `gem -v` toosee hello version installed.</span></span>
5. <span data-ttu-id="d72c8-132">Nainstalujte hello RSZ, zkontrolujte a další nástroje sestavení spuštěním příkazu hello `sudo apt-get install build-essential`.</span><span class="sxs-lookup"><span data-stu-id="d72c8-132">Install hello gcc, make, and other build tools by running hello command `sudo apt-get install build-essential`.</span></span>
6. <span data-ttu-id="d72c8-133">Nainstalujte knihovny vývojáře klienta MySQL hello spuštěním příkazu hello `sudo apt-get install libmysqlclient-dev`.</span><span class="sxs-lookup"><span data-stu-id="d72c8-133">Install hello MySQL client developer libraries by running hello command `sudo apt-get install libmysqlclient-dev`.</span></span>
7. <span data-ttu-id="d72c8-134">Sestavení modulu hello mysql2 pro Ruby pomocí Gem spuštěním příkazu hello `sudo gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="d72c8-134">Build hello mysql2 module for Ruby using Gem by running hello command `sudo gem install mysql2`.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="d72c8-135">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="d72c8-135">Get connection information</span></span>
<span data-ttu-id="d72c8-136">Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="d72c8-136">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="d72c8-137">Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="d72c8-137">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="d72c8-138">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d72c8-138">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d72c8-139">Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru můžete mít pokrčené, jako například **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="d72c8-139">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have creased, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="d72c8-140">Klikněte na název serveru hello **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="d72c8-140">Click hello server name **myserver4demo**.</span></span>
4. <span data-ttu-id="d72c8-141">Vyberte hello serveru **vlastnosti** stránky.</span><span class="sxs-lookup"><span data-stu-id="d72c8-141">Select hello server's **Properties** page.</span></span> <span data-ttu-id="d72c8-142">Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.</span><span class="sxs-lookup"><span data-stu-id="d72c8-142">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="d72c8-143">![Azure Database for MySQL – přihlašovací jméno správce serveru](./media/connect-ruby/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="d72c8-143">![Azure Database for MySQL - Server Admin Login](./media/connect-ruby/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="d72c8-144">Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="d72c8-144">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="run-ruby-code"></a><span data-ttu-id="d72c8-145">Spuštění kódu Ruby</span><span class="sxs-lookup"><span data-stu-id="d72c8-145">Run Ruby code</span></span> 
1. <span data-ttu-id="d72c8-146">Vložte hello Ruby kód z hello oddílech do textových souborů a uložit hello soubory do složky projektu s .rb souboru rozšíření, jako je například `C:\rubymysql\createtable.rb` nebo `/home/username/rubymysql/createtable.rb`.</span><span class="sxs-lookup"><span data-stu-id="d72c8-146">Paste hello Ruby code from hello sections below into text files, and save hello files into a project folder with file extension .rb, such as `C:\rubymysql\createtable.rb` or `/home/username/rubymysql/createtable.rb`.</span></span>
2. <span data-ttu-id="d72c8-147">Kód hello toorun, spusťte příkazový řádek hello nebo bash prostředí.</span><span class="sxs-lookup"><span data-stu-id="d72c8-147">toorun hello code, launch hello command prompt or bash shell.</span></span> <span data-ttu-id="d72c8-148">Změňte adresář na složku vašeho projektu pomocí příkazu `cd rubymysql`.</span><span class="sxs-lookup"><span data-stu-id="d72c8-148">Change directory into your project folder `cd rubymysql`</span></span>
3. <span data-ttu-id="d72c8-149">Zadejte příkaz hello ruby a potom podle názvu souboru text hello, jako například `ruby createtable.rb` toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d72c8-149">Then type hello ruby command followed by hello file name, such as `ruby createtable.rb` toorun hello application.</span></span>
4. <span data-ttu-id="d72c8-150">Na hello operačního systému Windows Pokud aplikace hello ruby není ve vašem prostředí PATH může být nutné toouse hello úplná cesta toolaunch hello uzel aplikace, jako například`"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`</span><span class="sxs-lookup"><span data-stu-id="d72c8-150">On hello Windows OS, if hello ruby application is not in your path environment variable, you may need toouse hello full path toolaunch hello node application, such as `"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="d72c8-151">Připojení a vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="d72c8-151">Connect and create a table</span></span>
<span data-ttu-id="d72c8-152">Použití hello následující kód tooconnect a vytvořte tabulku pomocí **CREATE TABLE** příkaz jazyka SQL, za nímž následuje **INSERT INTO** SQL příkazy tooadd řádků do tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="d72c8-152">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements tooadd rows into hello table.</span></span>

<span data-ttu-id="d72c8-153">Kód Hello používá [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) třídy .new() metoda tooconnect tooAzure databáze pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="d72c8-153">hello code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method tooconnect tooAzure Database for MySQL.</span></span> <span data-ttu-id="d72c8-154">Potom zavolá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) příkazů DROP, CREATE TABLE a INSERT INTO toorun hello několikrát.</span><span class="sxs-lookup"><span data-stu-id="d72c8-154">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) several times toorun hello DROP, CREATE TABLE, and INSERT INTO commands.</span></span> <span data-ttu-id="d72c8-155">Potom zavolá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello připojení předtím, než se ukončuje.</span><span class="sxs-lookup"><span data-stu-id="d72c8-155">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="d72c8-156">Nahraďte hello `host`, `database`, `username`, a `password` řetězce s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="d72c8-156">Replace hello `host`, `database`, `username`, and `password` strings with your own values.</span></span> 
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

## <a name="read-data"></a><span data-ttu-id="d72c8-157">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="d72c8-157">Read data</span></span>
<span data-ttu-id="d72c8-158">Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="d72c8-158">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="d72c8-159">Kód Hello používá [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) třídy .new() metoda tooconnect tooAzure databáze pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="d72c8-159">hello code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method tooconnect tooAzure Database for MySQL.</span></span> <span data-ttu-id="d72c8-160">Potom zavolá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello vyberte příkazy.</span><span class="sxs-lookup"><span data-stu-id="d72c8-160">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello SELECT commands.</span></span> <span data-ttu-id="d72c8-161">Potom zavolá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello připojení předtím, než se ukončuje.</span><span class="sxs-lookup"><span data-stu-id="d72c8-161">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="d72c8-162">Nahraďte hello `host`, `database`, `username`, a `password` řetězce s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="d72c8-162">Replace hello `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

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

## <a name="update-data"></a><span data-ttu-id="d72c8-163">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="d72c8-163">Update data</span></span>
<span data-ttu-id="d72c8-164">Použití hello následující kód tooconnect a aktualizovat data pomocí hello **aktualizace** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="d72c8-164">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="d72c8-165">Kód Hello používá [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) třídy .new() metoda tooconnect tooAzure databáze pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="d72c8-165">hello code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method tooconnect tooAzure Database for MySQL.</span></span> <span data-ttu-id="d72c8-166">Potom zavolá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello aktualizovat příkazy.</span><span class="sxs-lookup"><span data-stu-id="d72c8-166">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello UPDATE commands.</span></span> <span data-ttu-id="d72c8-167">Potom zavolá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello připojení předtím, než se ukončuje.</span><span class="sxs-lookup"><span data-stu-id="d72c8-167">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="d72c8-168">Nahraďte hello `host`, `database`, `username`, a `password` řetězce s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="d72c8-168">Replace hello `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

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


## <a name="delete-data"></a><span data-ttu-id="d72c8-169">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="d72c8-169">Delete data</span></span>
<span data-ttu-id="d72c8-170">Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="d72c8-170">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="d72c8-171">Kód Hello používá [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) třídy .new() metoda tooconnect tooAzure databáze pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="d72c8-171">hello code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method tooconnect tooAzure Database for MySQL.</span></span> <span data-ttu-id="d72c8-172">Potom zavolá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello odstranění příkazy.</span><span class="sxs-lookup"><span data-stu-id="d72c8-172">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello DELETE commands.</span></span> <span data-ttu-id="d72c8-173">Potom zavolá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello připojení předtím, než se ukončuje.</span><span class="sxs-lookup"><span data-stu-id="d72c8-173">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="d72c8-174">Nahraďte hello `host`, `database`, `username`, a `password` řetězce s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="d72c8-174">Replace hello `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="d72c8-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d72c8-175">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="d72c8-176">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="d72c8-176">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
