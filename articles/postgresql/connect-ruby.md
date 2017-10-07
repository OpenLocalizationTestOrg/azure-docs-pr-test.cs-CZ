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
# <a name="azure-database-for-postgresql-use-ruby-tooconnect-and-query-data"></a><span data-ttu-id="bc8e5-103">Azure databázi PostgreSQL: použití Ruby tooconnect a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="bc8e5-103">Azure Database for PostgreSQL: Use Ruby tooconnect and query data</span></span>
<span data-ttu-id="bc8e5-104">Tento rychlý start předvádí jak tooconnect tooan Azure databázi PostgreSQL pomocí [Ruby](https://www.ruby-lang.org) aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using a [Ruby](https://www.ruby-lang.org) application.</span></span> <span data-ttu-id="bc8e5-105">Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="bc8e5-106">Tento článek předpokládá, že jste obeznámeni s vývojem pomocí Ruby, ale, že se nový tooworking s Azure databáze PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-106">This article assumes you are familiar with development using Ruby, but that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc8e5-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bc8e5-107">Prerequisites</span></span>
<span data-ttu-id="bc8e5-108">Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:</span><span class="sxs-lookup"><span data-stu-id="bc8e5-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="bc8e5-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="bc8e5-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="bc8e5-110">Vytvoření databáze – rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="bc8e5-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

## <a name="install-ruby"></a><span data-ttu-id="bc8e5-111">Instalace Ruby</span><span class="sxs-lookup"><span data-stu-id="bc8e5-111">Install Ruby</span></span>
<span data-ttu-id="bc8e5-112">Nainstalujte Ruby na vlastní počítač.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-112">Install Ruby on your own machine.</span></span> 

### <a name="windows"></a><span data-ttu-id="bc8e5-113">Windows</span><span class="sxs-lookup"><span data-stu-id="bc8e5-113">Windows</span></span>
- <span data-ttu-id="bc8e5-114">Stažení a instalace hello nejnovější verzi [Ruby](http://rubyinstaller.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="bc8e5-114">Download and Install hello latest version of [Ruby](http://rubyinstaller.org/downloads/).</span></span>
- <span data-ttu-id="bc8e5-115">Na hello dokončit obrazovky hello Instalační služby MSI, zaškrtněte políčko hello, která říká "spustit ridk nainstalovat tooinstall MSYS2 a nástrojů pro vývoj."</span><span class="sxs-lookup"><span data-stu-id="bc8e5-115">On hello finish screen of hello MSI installer, check hello box that says "Run 'ridk install' tooinstall MSYS2 and development toolchain."</span></span> <span data-ttu-id="bc8e5-116">Pak klikněte na tlačítko **Dokončit** toolaunch hello další instalační služby systému.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-116">Then click **Finish** toolaunch hello next installer.</span></span>
- <span data-ttu-id="bc8e5-117">spustí instalační program RubyInstaller2 pro Windows Hello.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-117">hello RubyInstaller2 for Windows installer launches.</span></span> <span data-ttu-id="bc8e5-118">Zadejte 2 tooinstall hello MSYS2 úložiště aktualizací.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-118">Type 2 tooinstall hello MSYS2 repository update.</span></span> <span data-ttu-id="bc8e5-119">Po dokončení a vrátí toohello výzva k instalaci, zavřete příkazové okno hello.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-119">After it finishes and returns toohello installation prompt, close hello command window.</span></span>
- <span data-ttu-id="bc8e5-120">Spuštění nového příkazového řádku (cmd) z nabídky Start hello.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-120">Launch a new command prompt (cmd) from hello Start menu.</span></span>
- <span data-ttu-id="bc8e5-121">Test hello Ruby instalace `ruby -v` nainstalovaná verze toosee hello.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-121">Test hello Ruby installation `ruby -v` toosee hello version installed.</span></span>
- <span data-ttu-id="bc8e5-122">Testování instalace Gem hello `gem -v` nainstalovaná verze toosee hello.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-122">Test hello Gem installation `gem -v` toosee hello version installed.</span></span>
- <span data-ttu-id="bc8e5-123">Sestavení modulu hello PostgreSQL pro Ruby pomocí Gem spuštěním příkazu hello `gem install pg`.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-123">Build hello PostgreSQL module for Ruby using Gem by running hello command `gem install pg`.</span></span>

### <a name="macos"></a><span data-ttu-id="bc8e5-124">MacOS</span><span class="sxs-lookup"><span data-stu-id="bc8e5-124">MacOS</span></span>
- <span data-ttu-id="bc8e5-125">Instalace pomocí Homebrew spuštěním příkazu hello Ruby `brew install ruby`.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-125">Install Ruby using Homebrew by running hello command `brew install ruby`.</span></span> <span data-ttu-id="bc8e5-126">Další možnosti instalace najdete v tématu hello Ruby [dokumentaci k instalaci](https://www.ruby-lang.org/en/documentation/installation/#homebrew)</span><span class="sxs-lookup"><span data-stu-id="bc8e5-126">For more installation options, see hello Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/#homebrew)</span></span>
- <span data-ttu-id="bc8e5-127">Test hello Ruby instalace `ruby -v` nainstalovaná verze toosee hello.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-127">Test hello Ruby installation `ruby -v` toosee hello version installed.</span></span>
- <span data-ttu-id="bc8e5-128">Testování instalace Gem hello `gem -v` nainstalovaná verze toosee hello.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-128">Test hello Gem installation `gem -v` toosee hello version installed.</span></span>
- <span data-ttu-id="bc8e5-129">Sestavení modulu hello PostgreSQL pro Ruby pomocí Gem spuštěním příkazu hello `gem install pg`.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-129">Build hello PostgreSQL module for Ruby using Gem by running hello command `gem install pg`.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="bc8e5-130">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="bc8e5-130">Linux (Ubuntu)</span></span>
- <span data-ttu-id="bc8e5-131">Nainstalujte Ruby spuštěním příkazu hello `sudo apt-get install ruby-full`.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-131">Install Ruby by running hello command `sudo apt-get install ruby-full`.</span></span> <span data-ttu-id="bc8e5-132">Další možnosti instalace najdete v tématu hello Ruby [instalace dokumentace](https://www.ruby-lang.org/en/documentation/installation/).</span><span class="sxs-lookup"><span data-stu-id="bc8e5-132">For more installation options, see hello Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/).</span></span>
- <span data-ttu-id="bc8e5-133">Test hello Ruby instalace `ruby -v` nainstalovaná verze toosee hello.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-133">Test hello Ruby installation `ruby -v` toosee hello version installed.</span></span>
- <span data-ttu-id="bc8e5-134">Nainstalujte nejnovější aktualizace hello pro Gem spuštěním příkazu hello `sudo gem update --system`.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-134">Install hello latest updates for Gem by running hello command `sudo gem update --system`.</span></span>
- <span data-ttu-id="bc8e5-135">Testování instalace Gem hello `gem -v` nainstalovaná verze toosee hello.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-135">Test hello Gem installation `gem -v` toosee hello version installed.</span></span>
- <span data-ttu-id="bc8e5-136">Nainstalujte hello RSZ, zkontrolujte a další nástroje sestavení spuštěním příkazu hello `sudo apt-get install build-essential`.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-136">Install hello gcc, make, and other build tools by running hello command `sudo apt-get install build-essential`.</span></span>
- <span data-ttu-id="bc8e5-137">Nainstalujte hello PostgreSQL knihovny spuštěním příkazu hello `sudo apt-get install libpq-dev`.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-137">Install hello PostgreSQL libraries by running hello command `sudo apt-get install libpq-dev`.</span></span>
- <span data-ttu-id="bc8e5-138">Sestavení modulu Ruby pg hello pomocí Gem spuštěním příkazu hello `sudo gem install pg`.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-138">Build hello Ruby pg module using Gem by running hello command `sudo gem install pg`.</span></span>

## <a name="run-ruby-code"></a><span data-ttu-id="bc8e5-139">Spuštění kódu Ruby</span><span class="sxs-lookup"><span data-stu-id="bc8e5-139">Run Ruby code</span></span> 
- <span data-ttu-id="bc8e5-140">Uložte hello kódu do textového souboru a uložte soubor hello do složky projektu s .rb souboru rozšíření, jako například `C:\rubypostgres\read.rb` nebo`/home/username/rubypostgres/read.rb`</span><span class="sxs-lookup"><span data-stu-id="bc8e5-140">Save hello code into a text file, and save hello file into a project folder with file extension .rb, such as `C:\rubypostgres\read.rb` or `/home/username/rubypostgres/read.rb`</span></span>
- <span data-ttu-id="bc8e5-141">Kód hello toorun, spusťte příkazový řádek hello nebo bash prostředí.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-141">toorun hello code, launch hello command prompt or bash shell.</span></span> <span data-ttu-id="bc8e5-142">Změnit adresář, do složky projektu `cd rubypostgres`, zadejte příkaz hello `ruby read.rb` toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-142">Change directory into your project folder `cd rubypostgres`, then type hello command `ruby read.rb` toorun hello application.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="bc8e5-143">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="bc8e5-143">Get connection information</span></span>
<span data-ttu-id="bc8e5-144">Získáte hello připojení informace potřebné tooconnect toohello databáze Azure pro PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-144">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="bc8e5-145">Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-145">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="bc8e5-146">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="bc8e5-146">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="bc8e5-147">Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru, které jste vytvořili, například **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-147">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="bc8e5-148">Klikněte na název serveru hello **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-148">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="bc8e5-149">Vyberte hello serveru **přehled** stránky.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-149">Select hello server's **Overview** page.</span></span> <span data-ttu-id="bc8e5-150">Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-150">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="bc8e5-151">![Azure Database for PostgreSQL – přihlášení správce serveru](./media/connect-ruby/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="bc8e5-151">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-ruby/1-connection-string.png)</span></span>
5. <span data-ttu-id="bc8e5-152">Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránky tooview hello serveru správce přihlašovací jméno.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-152">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name.</span></span> <span data-ttu-id="bc8e5-153">V případě potřeby resetovat heslo hello.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-153">If necessary, reset hello password.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="bc8e5-154">Připojení a vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="bc8e5-154">Connect and create a table</span></span>
<span data-ttu-id="bc8e5-155">Použití hello následující kód tooconnect a vytvořte tabulku pomocí **CREATE TABLE** příkaz jazyka SQL, za nímž následuje **INSERT INTO** SQL příkazy tooadd řádků do tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-155">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements tooadd rows into hello table.</span></span>

<span data-ttu-id="bc8e5-156">Kód Hello používá [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt s konstruktor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-156">hello code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="bc8e5-157">Potom zavolá metodu [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello rozevírací, vytvořit tabulku a VLOŽTE do příkazy.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-157">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello DROP, CREATE TABLE, and INSERT INTO commands.</span></span> <span data-ttu-id="bc8e5-158">Hello kód kontroluje chyby pomocí hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) třídy.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-158">hello code checks for errors using hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="bc8e5-159">Potom zavolá metodu [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello připojení předtím, než se ukončuje.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-159">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="bc8e5-160">Nahraďte hello `host`, `database`, `user`, a `password` řetězce s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-160">Replace hello `host`, `database`, `user`, and `password` strings with your own values.</span></span> 
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

## <a name="read-data"></a><span data-ttu-id="bc8e5-161">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="bc8e5-161">Read data</span></span>
<span data-ttu-id="bc8e5-162">Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-162">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="bc8e5-163">Kód Hello používá [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt s konstruktor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-163">hello code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="bc8e5-164">Potom zavolá metodu [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) vyberte příkaz hello toorun, udržování hello výsledky sady výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-164">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello SELECT command, keeping hello results in a result set.</span></span> <span data-ttu-id="bc8e5-165">Hello výsledek sadu kolekce je vstupní oproti použití hello `resultSet.each do` ve smyčce, udržování hello aktuální hodnoty řádků v hello `row` proměnné.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-165">hello result set collection is iterated over using hello `resultSet.each do` loop, keeping hello current row values in hello `row` variable.</span></span> <span data-ttu-id="bc8e5-166">Hello kód kontroluje chyby pomocí hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) třídy.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-166">hello code checks for errors using hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="bc8e5-167">Potom zavolá metodu [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello připojení předtím, než se ukončuje.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-167">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="bc8e5-168">Nahraďte hello `host`, `database`, `user`, a `password` řetězce s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-168">Replace hello `host`, `database`, `user`, and `password` strings with your own values.</span></span> 

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

## <a name="update-data"></a><span data-ttu-id="bc8e5-169">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="bc8e5-169">Update data</span></span>
<span data-ttu-id="bc8e5-170">Použití hello následující kód tooconnect a aktualizovat data pomocí hello **aktualizace** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-170">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="bc8e5-171">Kód Hello používá [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt s konstruktor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-171">hello code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="bc8e5-172">Potom zavolá metodu [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello příkaz aktualizace.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-172">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello UPDATE command.</span></span> <span data-ttu-id="bc8e5-173">Hello kód kontroluje chyby pomocí hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) třídy.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-173">hello code checks for errors using hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="bc8e5-174">Potom zavolá metodu [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello připojení předtím, než se ukončuje.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-174">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="bc8e5-175">Nahraďte hello `host`, `database`, `user`, a `password` řetězce s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-175">Replace hello `host`, `database`, `user`, and `password` strings with your own values.</span></span> 

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


## <a name="delete-data"></a><span data-ttu-id="bc8e5-176">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="bc8e5-176">Delete data</span></span>
<span data-ttu-id="bc8e5-177">Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-177">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="bc8e5-178">Kód Hello používá [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt s konstruktor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-178">hello code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="bc8e5-179">Potom zavolá metodu [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello příkaz aktualizace.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-179">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello UPDATE command.</span></span> <span data-ttu-id="bc8e5-180">Hello kód kontroluje chyby pomocí hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) třídy.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-180">hello code checks for errors using hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="bc8e5-181">Potom zavolá metodu [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello připojení předtím, než se ukončuje.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-181">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="bc8e5-182">Nahraďte hello `host`, `database`, `user`, a `password` řetězce s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="bc8e5-182">Replace hello `host`, `database`, `user`, and `password` strings with your own values.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="bc8e5-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bc8e5-183">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="bc8e5-184">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="bc8e5-184">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
