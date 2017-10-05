---
title: "Připojení k Azure Database for MySQL pomocí Ruby | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete několik vzorových kódů Ruby, které můžete použít k připojení a dotazování dat ze služby Azure Database for MySQL."
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
ms.openlocfilehash: e54f1dccbae060c52f48bfeb277c045b99a91715
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="azure-database-for-mysql-use-ruby-to-connect-and-query-data"></a><span data-ttu-id="479df-103">Azure Database for MySQL: Připojení a dotazování dat pomocí Ruby</span><span class="sxs-lookup"><span data-stu-id="479df-103">Azure Database for MySQL: Use Ruby to connect and query data</span></span>
<span data-ttu-id="479df-104">Tento rychlý start ukazuje, jak se připojit ke službě Azure Database for MySQL pomocí aplikace v [Ruby](https://www.ruby-lang.org) a gemu [mysql2](https://rubygems.org/gems/mysql2) z platforem Windows, Ubuntu Linux a Mac.</span><span class="sxs-lookup"><span data-stu-id="479df-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using a [Ruby](https://www.ruby-lang.org) application and the [mysql2](https://rubygems.org/gems/mysql2) gem from Windows, Ubuntu Linux, and Mac platforms.</span></span> <span data-ttu-id="479df-105">Ukazuje, jak pomocí příkazů jazyka SQL dotazovat, vkládat, aktualizovat a odstraňovat data v databázi.</span><span class="sxs-lookup"><span data-stu-id="479df-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="479df-106">V tomto článku se předpokládá, že máte zkušenosti s vývojem pomocí Ruby, ale teprve začínáte pracovat se službou Azure Database for MySQL.</span><span class="sxs-lookup"><span data-stu-id="479df-106">This article assumes you are familiar with development using Ruby, but that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="479df-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="479df-107">Prerequisites</span></span>
<span data-ttu-id="479df-108">Tento rychlý start jako výchozí bod využívá prostředky vytvořené v některém z těchto průvodců:</span><span class="sxs-lookup"><span data-stu-id="479df-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="479df-109">Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="479df-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="479df-110">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="479df-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-ruby"></a><span data-ttu-id="479df-111">Instalace Ruby</span><span class="sxs-lookup"><span data-stu-id="479df-111">Install Ruby</span></span>
<span data-ttu-id="479df-112">Nainstalujte na svém počítači Ruby, nástroj Gem a knihovnu MySQL2.</span><span class="sxs-lookup"><span data-stu-id="479df-112">Install Ruby, Gem, and the MySQL2 library on your own machine.</span></span> 

### <a name="windows"></a><span data-ttu-id="479df-113">Windows</span><span class="sxs-lookup"><span data-stu-id="479df-113">Windows</span></span>
1. <span data-ttu-id="479df-114">Stáhněte a nainstalujte [Ruby](http://rubyinstaller.org/downloads/) verze 2.3.</span><span class="sxs-lookup"><span data-stu-id="479df-114">Download and Install the 2.3 version of [Ruby](http://rubyinstaller.org/downloads/).</span></span>
2. <span data-ttu-id="479df-115">Z nabídky Start spusťte nový příkazový řádek (cmd).</span><span class="sxs-lookup"><span data-stu-id="479df-115">Launch a new command prompt (cmd) from the Start menu.</span></span>
3. <span data-ttu-id="479df-116">Změňte adresář na adresář Ruby verze 2.3.</span><span class="sxs-lookup"><span data-stu-id="479df-116">Change directory into the Ruby directory for version 2.3.</span></span> `cd c:\Ruby23-x64\bin`
4. <span data-ttu-id="479df-117">Otestujte instalaci Ruby spuštěním příkazu `ruby -v`, který zobrazí nainstalovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="479df-117">Test the Ruby installation by running the command `ruby -v` to see the version installed.</span></span>
5. <span data-ttu-id="479df-118">Otestujte instalaci nástroje Gem spuštěním příkazu `gem -v`, který zobrazí nainstalovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="479df-118">Test the Gem installation by running the command `gem -v` to see the version installed.</span></span>
6. <span data-ttu-id="479df-119">Pomocí nástroje Gem sestavte modul Mysql2 pro Ruby spuštěním příkazu `gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="479df-119">Build the Mysql2 module for Ruby using Gem by running the command `gem install mysql2`.</span></span>

### <a name="macos"></a><span data-ttu-id="479df-120">MacOS</span><span class="sxs-lookup"><span data-stu-id="479df-120">MacOS</span></span>
1. <span data-ttu-id="479df-121">Nainstalujte Ruby pomocí Homebrew spuštěním příkazu `brew install ruby`.</span><span class="sxs-lookup"><span data-stu-id="479df-121">Install Ruby using Homebrew by running the command `brew install ruby`.</span></span> <span data-ttu-id="479df-122">Další možnosti instalace najdete v [dokumentaci k instalaci](https://www.ruby-lang.org/en/documentation/installation/#homebrew) Ruby.</span><span class="sxs-lookup"><span data-stu-id="479df-122">For more installation options, see the Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/#homebrew).</span></span>
2. <span data-ttu-id="479df-123">Otestujte instalaci Ruby spuštěním příkazu `ruby -v`, který zobrazí nainstalovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="479df-123">Test the Ruby installation by running the command `ruby -v` to see the version installed.</span></span>
3. <span data-ttu-id="479df-124">Otestujte instalaci nástroje Gem spuštěním příkazu `gem -v`, který zobrazí nainstalovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="479df-124">Test the Gem installation by running the command `gem -v` to see the version installed.</span></span>
4. <span data-ttu-id="479df-125">Pomocí nástroje Gem sestavte modul Mysql2 pro Ruby spuštěním příkazu `gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="479df-125">Build the Mysql2 module for Ruby using Gem by running the command `gem install mysql2`.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="479df-126">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="479df-126">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="479df-127">Nainstalujte Ruby spuštěním příkazu `sudo apt-get install ruby-full`.</span><span class="sxs-lookup"><span data-stu-id="479df-127">Install Ruby by running the command `sudo apt-get install ruby-full`.</span></span> <span data-ttu-id="479df-128">Další možnosti instalace najdete v [dokumentaci k instalaci](https://www.ruby-lang.org/en/documentation/installation/) Ruby.</span><span class="sxs-lookup"><span data-stu-id="479df-128">For more installation options, see the Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/).</span></span>
2. <span data-ttu-id="479df-129">Otestujte instalaci Ruby spuštěním příkazu `ruby -v`, který zobrazí nainstalovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="479df-129">Test the Ruby installation by running the command `ruby -v` to see the version installed.</span></span>
3. <span data-ttu-id="479df-130">Nainstalujte nejnovější aktualizace pro nástroj Gem spuštěním příkazu `sudo gem update --system`.</span><span class="sxs-lookup"><span data-stu-id="479df-130">Install the latest updates for Gem by running the command `sudo gem update --system`.</span></span>
4. <span data-ttu-id="479df-131">Otestujte instalaci nástroje Gem spuštěním příkazu `gem -v`, který zobrazí nainstalovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="479df-131">Test the Gem installation by running the command `gem -v` to see the version installed.</span></span>
5. <span data-ttu-id="479df-132">Nainstalujte gcc, make a další nástroje sestavení spuštěním příkazu `sudo apt-get install build-essential`.</span><span class="sxs-lookup"><span data-stu-id="479df-132">Install the gcc, make, and other build tools by running the command `sudo apt-get install build-essential`.</span></span>
6. <span data-ttu-id="479df-133">Nainstalujte klientské knihovny MySQL pro vývojáře spuštěním příkazu `sudo apt-get install libmysqlclient-dev`.</span><span class="sxs-lookup"><span data-stu-id="479df-133">Install the MySQL client developer libraries by running the command `sudo apt-get install libmysqlclient-dev`.</span></span>
7. <span data-ttu-id="479df-134">Pomocí nástroje Gem sestavte modul mysql2 pro Ruby spuštěním příkazu `sudo gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="479df-134">Build the mysql2 module for Ruby using Gem by running the command `sudo gem install mysql2`.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="479df-135">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="479df-135">Get connection information</span></span>
<span data-ttu-id="479df-136">Získejte informace o připojení potřebné pro připojení ke službě Azure Database for MySQL.</span><span class="sxs-lookup"><span data-stu-id="479df-136">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="479df-137">Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="479df-137">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="479df-138">Přihlaste se k [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="479df-138">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="479df-139">V nabídce vlevo na webu Azure Portal klikněte na **Všechny prostředky** a vyhledejte vytvořený server, například **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="479df-139">From the left-hand menu in Azure portal, click **All resources** and search for the server you have creased, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="479df-140">Klikněte na název serveru **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="479df-140">Click the server name **myserver4demo**.</span></span>
4. <span data-ttu-id="479df-141">Vyberte stránku **Vlastnosti** serveru.</span><span class="sxs-lookup"><span data-stu-id="479df-141">Select the server's **Properties** page.</span></span> <span data-ttu-id="479df-142">Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="479df-142">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="479df-143">![Azure Database for MySQL – přihlašovací jméno správce serveru](./media/connect-ruby/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="479df-143">![Azure Database for MySQL - Server Admin Login](./media/connect-ruby/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="479df-144">Pokud zapomenete přihlašovací údaje k serveru, přejděte na stránku **Přehled**, kde můžete zobrazit přihlašovací jméno správce serveru a v případě potřeby resetovat heslo.</span><span class="sxs-lookup"><span data-stu-id="479df-144">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="run-ruby-code"></a><span data-ttu-id="479df-145">Spuštění kódu Ruby</span><span class="sxs-lookup"><span data-stu-id="479df-145">Run Ruby code</span></span> 
1. <span data-ttu-id="479df-146">Kód Ruby z dalších částí vložte do textových souborů a tyto soubory uložte do složky projektu s příponou .rb, například `C:\rubymysql\createtable.rb` nebo `/home/username/rubymysql/createtable.rb`.</span><span class="sxs-lookup"><span data-stu-id="479df-146">Paste the Ruby code from the sections below into text files, and save the files into a project folder with file extension .rb, such as `C:\rubymysql\createtable.rb` or `/home/username/rubymysql/createtable.rb`.</span></span>
2. <span data-ttu-id="479df-147">Pokud chcete kód spustit, spusťte příkazový řádek nebo prostředí Bash.</span><span class="sxs-lookup"><span data-stu-id="479df-147">To run the code, launch the command prompt or bash shell.</span></span> <span data-ttu-id="479df-148">Změňte adresář na složku vašeho projektu pomocí příkazu `cd rubymysql`.</span><span class="sxs-lookup"><span data-stu-id="479df-148">Change directory into your project folder `cd rubymysql`</span></span>
3. <span data-ttu-id="479df-149">Potom zadejte příkaz ruby následovaný názvem souboru, například `ruby createtable.rb`, a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="479df-149">Then type the ruby command followed by the file name, such as `ruby createtable.rb` to run the application.</span></span>
4. <span data-ttu-id="479df-150">Pokud v operačním systému Windows není aplikace v Ruby ve vaší proměnné prostředí PATH, možná bude nutné ke spuštění aplikace v Ruby použít úplnou cestu, například `"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`.</span><span class="sxs-lookup"><span data-stu-id="479df-150">On the Windows OS, if the ruby application is not in your path environment variable, you may need to use the full path to launch the node application, such as `"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="479df-151">Připojení a vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="479df-151">Connect and create a table</span></span>
<span data-ttu-id="479df-152">Pomocí následujícího kódu se připojte a vytvořte tabulku s využitím příkazu **CREATE TABLE** jazyka SQL, po kterém následují příkazy **INSERT INTO** jazyka SQL, které do tabulky přidají řádky.</span><span class="sxs-lookup"><span data-stu-id="479df-152">Use the following code to connect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements to add rows into the table.</span></span>

<span data-ttu-id="479df-153">Kód pro připojení ke službě Azure Database for MySQL používá metodu .new() třídy [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8).</span><span class="sxs-lookup"><span data-stu-id="479df-153">The code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method to connect to Azure Database for MySQL.</span></span> <span data-ttu-id="479df-154">Potom několikrát volá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) pro spuštění příkazů DROP, CREATE TABLE a INSERT INTO.</span><span class="sxs-lookup"><span data-stu-id="479df-154">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) several times to run the DROP, CREATE TABLE, and INSERT INTO commands.</span></span> <span data-ttu-id="479df-155">Před ukončením potom volá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) pro ukončení připojení.</span><span class="sxs-lookup"><span data-stu-id="479df-155">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) to close the connection before terminating.</span></span>

<span data-ttu-id="479df-156">Nahraďte řetězce `host`, `database`, `username` a `password` vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="479df-156">Replace the `host`, `database`, `username`, and `password` strings with your own values.</span></span> 
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
    puts 'Successfully created connection to database.'

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

## <a name="read-data"></a><span data-ttu-id="479df-157">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="479df-157">Read data</span></span>
<span data-ttu-id="479df-158">Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **SELECT** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="479df-158">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="479df-159">Kód pro připojení ke službě Azure Database for MySQL používá metodu .new() třídy [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8).</span><span class="sxs-lookup"><span data-stu-id="479df-159">The code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method to connect to Azure Database for MySQL.</span></span> <span data-ttu-id="479df-160">Potom volá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) pro spuštění příkazů SELECT.</span><span class="sxs-lookup"><span data-stu-id="479df-160">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) to run the SELECT commands.</span></span> <span data-ttu-id="479df-161">Před ukončením potom volá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) pro ukončení připojení.</span><span class="sxs-lookup"><span data-stu-id="479df-161">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) to close the connection before terminating.</span></span>

<span data-ttu-id="479df-162">Nahraďte řetězce `host`, `database`, `username` a `password` vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="479df-162">Replace the `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

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
    puts 'Successfully created connection to database.'

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

## <a name="update-data"></a><span data-ttu-id="479df-163">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="479df-163">Update data</span></span>
<span data-ttu-id="479df-164">Pomocí následujícího kódu se připojte a aktualizujte data s využitím příkazu **UPDATE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="479df-164">Use the following code to connect and update the data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="479df-165">Kód pro připojení ke službě Azure Database for MySQL používá metodu .new() třídy [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8).</span><span class="sxs-lookup"><span data-stu-id="479df-165">The code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method to connect to Azure Database for MySQL.</span></span> <span data-ttu-id="479df-166">Potom volá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) pro spuštění příkazů UPDATE.</span><span class="sxs-lookup"><span data-stu-id="479df-166">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) to run the UPDATE commands.</span></span> <span data-ttu-id="479df-167">Před ukončením potom volá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) pro ukončení připojení.</span><span class="sxs-lookup"><span data-stu-id="479df-167">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) to close the connection before terminating.</span></span>

<span data-ttu-id="479df-168">Nahraďte řetězce `host`, `database`, `username` a `password` vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="479df-168">Replace the `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

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
    puts 'Successfully created connection to database.'

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


## <a name="delete-data"></a><span data-ttu-id="479df-169">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="479df-169">Delete data</span></span>
<span data-ttu-id="479df-170">Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **DELETE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="479df-170">Use the following code to connect and read the data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="479df-171">Kód pro připojení ke službě Azure Database for MySQL používá metodu .new() třídy [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8).</span><span class="sxs-lookup"><span data-stu-id="479df-171">The code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method to connect to Azure Database for MySQL.</span></span> <span data-ttu-id="479df-172">Potom volá metodu [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) pro spuštění příkazů DELETE.</span><span class="sxs-lookup"><span data-stu-id="479df-172">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) to run the DELETE commands.</span></span> <span data-ttu-id="479df-173">Před ukončením potom volá metodu [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) pro ukončení připojení.</span><span class="sxs-lookup"><span data-stu-id="479df-173">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) to close the connection before terminating.</span></span>

<span data-ttu-id="479df-174">Nahraďte řetězce `host`, `database`, `username` a `password` vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="479df-174">Replace the `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

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
    puts 'Successfully created connection to database.'

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

## <a name="next-steps"></a><span data-ttu-id="479df-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="479df-175">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="479df-176">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="479df-176">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
