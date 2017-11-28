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
# <a name="azure-database-for-mysql-use-go-language-tooconnect-and-query-data"></a><span data-ttu-id="d36e4-103">Azure databáze pro databázi MySQL: přejít pomocí jazyka tooconnect a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="d36e4-103">Azure Database for MySQL: Use Go language tooconnect and query data</span></span>
<span data-ttu-id="d36e4-104">Tento rychlý start předvádí jak tooconnect tooan databáze Azure pro používání MySQL kód napsaný v hello [přejděte](https://golang.org/) jazyka z systému macOS platformy Windows, Ubuntu Linux a Apple.</span><span class="sxs-lookup"><span data-stu-id="d36e4-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using code written in hello [Go](https://golang.org/) language from Windows, Ubuntu Linux, and Apple macOS platforms.</span></span> <span data-ttu-id="d36e4-105">Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="d36e4-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="d36e4-106">Tento článek předpokládá, že jste obeznámeni s vývojem pomocí přejít, ale, že se nový tooworking s Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="d36e4-106">This article assumes you are familiar with development using Go, but that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d36e4-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d36e4-107">Prerequisites</span></span>
<span data-ttu-id="d36e4-108">Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:</span><span class="sxs-lookup"><span data-stu-id="d36e4-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="d36e4-109">Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d36e4-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="d36e4-110">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d36e4-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-go-and-mysql-connector"></a><span data-ttu-id="d36e4-111">Instalace jazyka Go a konektoru MySQL</span><span class="sxs-lookup"><span data-stu-id="d36e4-111">Install Go and MySQL connector</span></span>
<span data-ttu-id="d36e4-112">Nainstalujte [přejděte](https://golang.org/doc/install) a hello [přejděte ovladače sql pro databázi MySQL](https://github.com/go-sql-driver/mysql#installation) na vlastním počítači.</span><span class="sxs-lookup"><span data-stu-id="d36e4-112">Install [Go](https://golang.org/doc/install) and hello [go-sql-driver for MySQL](https://github.com/go-sql-driver/mysql#installation) on your own machine.</span></span> <span data-ttu-id="d36e4-113">V závislosti na platformě postupujte podle kroků hello:</span><span class="sxs-lookup"><span data-stu-id="d36e4-113">Depending on your platform, follow hello steps:</span></span>

### <a name="windows"></a><span data-ttu-id="d36e4-114">Windows</span><span class="sxs-lookup"><span data-stu-id="d36e4-114">Windows</span></span>
1. <span data-ttu-id="d36e4-115">[Stáhněte si](https://golang.org/dl/) a nainstalujte přejděte pro Microsoft Windows podle toohello [pokyny k instalaci](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="d36e4-115">[Download](https://golang.org/dl/) and install Go for Microsoft Windows according toohello [installation instructions](https://golang.org/doc/install).</span></span>
2. <span data-ttu-id="d36e4-116">Spusťte příkazový řádek hello z nabídky start hello.</span><span class="sxs-lookup"><span data-stu-id="d36e4-116">Launch hello command prompt from hello start menu.</span></span>
3. <span data-ttu-id="d36e4-117">Vytvořte složku pro váš projekt, například:</span><span class="sxs-lookup"><span data-stu-id="d36e4-117">Make a folder for your project such.</span></span> <span data-ttu-id="d36e4-118">`mkdir  %USERPROFILE%\go\src\mysqlgo`.</span><span class="sxs-lookup"><span data-stu-id="d36e4-118">`mkdir  %USERPROFILE%\go\src\mysqlgo`.</span></span>
4. <span data-ttu-id="d36e4-119">Změnit adresář, do složky hello projektu, například `cd %USERPROFILE%\go\src\mysqlgo`.</span><span class="sxs-lookup"><span data-stu-id="d36e4-119">Change directory into hello project folder, such as `cd %USERPROFILE%\go\src\mysqlgo`.</span></span>
5. <span data-ttu-id="d36e4-120">Nastavit proměnnou prostředí hello pro GOPATH toopoint toohello zdrojový kód adresář.</span><span class="sxs-lookup"><span data-stu-id="d36e4-120">Set hello environment variable for GOPATH toopoint toohello source code directory.</span></span> <span data-ttu-id="d36e4-121">`set GOPATH=%USERPROFILE%\go`.</span><span class="sxs-lookup"><span data-stu-id="d36e4-121">`set GOPATH=%USERPROFILE%\go`.</span></span>
6. <span data-ttu-id="d36e4-122">Nainstalujte hello [přejděte ovladače sql pro databázi mysql](https://github.com/go-sql-driver/mysql#installation) spuštěním hello `go get github.com/go-sql-driver/mysql` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d36e4-122">Install hello [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) by running hello `go get github.com/go-sql-driver/mysql` command.</span></span>

   <span data-ttu-id="d36e4-123">V souhrnu nainstalujte přejděte a potom spusťte tyto příkazy v příkazovém řádku hello:</span><span class="sxs-lookup"><span data-stu-id="d36e4-123">In summary, install Go, then run these commands in hello command prompt:</span></span>
   ```cmd
   mkdir  %USERPROFILE%\go\src\mysqlgo
   cd %USERPROFILE%\go\src\mysqlgo
   set GOPATH=%USERPROFILE%\go
   go get github.com/go-sql-driver/mysql
   ```

### <a name="linux-ubuntu"></a><span data-ttu-id="d36e4-124">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="d36e4-124">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="d36e4-125">Spusťte prostředí Bash hello.</span><span class="sxs-lookup"><span data-stu-id="d36e4-125">Launch hello Bash shell.</span></span> 
2. <span data-ttu-id="d36e4-126">Nainstalujte jazyk Go spuštěním příkazu `sudo apt-get install golang-go`.</span><span class="sxs-lookup"><span data-stu-id="d36e4-126">Install Go by running `sudo apt-get install golang-go`.</span></span>
3. <span data-ttu-id="d36e4-127">Ve svém domovském adresáři vytvořte složku pro projekt, například `mkdir -p ~/go/src/mysqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="d36e4-127">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/mysqlgo/`.</span></span>
4. <span data-ttu-id="d36e4-128">Změnit adresář, jako například do složky hello `cd ~/go/src/mysqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="d36e4-128">Change directory into hello folder, such as `cd ~/go/src/mysqlgo/`.</span></span>
5. <span data-ttu-id="d36e4-129">Sada hello GOPATH prostředí proměnné toopoint tooa platný zdrojový adresář, jako je vaše aktuální domovské adresáře přejít složku.</span><span class="sxs-lookup"><span data-stu-id="d36e4-129">Set hello GOPATH environment variable toopoint tooa valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="d36e4-130">V prostředí bash hello, spusťte `export GOPATH=~/go` tooadd hello přejděte directory jako hello GOPATH pro aktuální relaci prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="d36e4-130">At hello bash shell, run `export GOPATH=~/go` tooadd hello go directory as hello GOPATH for hello current shell session.</span></span>
6. <span data-ttu-id="d36e4-131">Nainstalujte hello [přejděte ovladače sql pro databázi mysql](https://github.com/go-sql-driver/mysql#installation) spuštěním hello `go get github.com/go-sql-driver/mysql` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d36e4-131">Install hello [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) by running hello `go get github.com/go-sql-driver/mysql` command.</span></span>

   <span data-ttu-id="d36e4-132">Stručně řečeno, spusťte tyto příkazy Bash:</span><span class="sxs-lookup"><span data-stu-id="d36e4-132">In summary, run these bash commands:</span></span>
   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

### <a name="apple-macos"></a><span data-ttu-id="d36e4-133">Apple macOS</span><span class="sxs-lookup"><span data-stu-id="d36e4-133">Apple macOS</span></span>
1. <span data-ttu-id="d36e4-134">Stáhněte a nainstalujte přejděte podle toohello [pokyny k instalaci](https://golang.org/doc/install) odpovídající vaši platformu.</span><span class="sxs-lookup"><span data-stu-id="d36e4-134">Download and install Go according toohello [installation instructions](https://golang.org/doc/install)  matching your platform.</span></span> 
2. <span data-ttu-id="d36e4-135">Spusťte prostředí Bash hello.</span><span class="sxs-lookup"><span data-stu-id="d36e4-135">Launch hello Bash shell.</span></span> 
3. <span data-ttu-id="d36e4-136">Ve svém domovském adresáři vytvořte složku pro projekt, například `mkdir -p ~/go/src/mysqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="d36e4-136">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/mysqlgo/`.</span></span>
4. <span data-ttu-id="d36e4-137">Změnit adresář, jako například do složky hello `cd ~/go/src/mysqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="d36e4-137">Change directory into hello folder, such as `cd ~/go/src/mysqlgo/`.</span></span>
5. <span data-ttu-id="d36e4-138">Sada hello GOPATH prostředí proměnné toopoint tooa platný zdrojový adresář, jako je vaše aktuální domovské adresáře přejít složku.</span><span class="sxs-lookup"><span data-stu-id="d36e4-138">Set hello GOPATH environment variable toopoint tooa valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="d36e4-139">V prostředí bash hello, spusťte `export GOPATH=~/go` tooadd hello přejděte directory jako hello GOPATH pro aktuální relaci prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="d36e4-139">At hello bash shell, run `export GOPATH=~/go` tooadd hello go directory as hello GOPATH for hello current shell session.</span></span>
6. <span data-ttu-id="d36e4-140">Nainstalujte hello [přejděte ovladače sql pro databázi mysql](https://github.com/go-sql-driver/mysql#installation) spuštěním hello `go get github.com/go-sql-driver/mysql` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d36e4-140">Install hello [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) by running hello `go get github.com/go-sql-driver/mysql` command.</span></span>

   <span data-ttu-id="d36e4-141">Stručně řečeno, nainstalujte jazyk Go a potom spusťte tyto příkazy Bash:</span><span class="sxs-lookup"><span data-stu-id="d36e4-141">In summary, install Go, then run these bash commands:</span></span>
   ```bash
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

## <a name="get-connection-information"></a><span data-ttu-id="d36e4-142">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="d36e4-142">Get connection information</span></span>
<span data-ttu-id="d36e4-143">Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="d36e4-143">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="d36e4-144">Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="d36e4-144">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="d36e4-145">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d36e4-145">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d36e4-146">Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru můžete mít pokrčené, jako například **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="d36e4-146">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have creased, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="d36e4-147">Klikněte na název serveru hello **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="d36e4-147">Click hello server name **myserver4demo**.</span></span>
4. <span data-ttu-id="d36e4-148">Vyberte hello serveru **vlastnosti** stránky.</span><span class="sxs-lookup"><span data-stu-id="d36e4-148">Select hello server's **Properties** page.</span></span> <span data-ttu-id="d36e4-149">Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.</span><span class="sxs-lookup"><span data-stu-id="d36e4-149">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="d36e4-150">![Azure Database for MySQL – přihlašovací jméno správce serveru](./media/connect-go/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="d36e4-150">![Azure Database for MySQL - Server Admin Login](./media/connect-go/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="d36e4-151">Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="d36e4-151">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>
   

## <a name="build-and-run-go-code"></a><span data-ttu-id="d36e4-152">Sestavení a spuštění kódu jazyka Go</span><span class="sxs-lookup"><span data-stu-id="d36e4-152">Build and run Go code</span></span> 
1. <span data-ttu-id="d36e4-153">toowrite Golang kódu, můžete použít jednoduchý textový editor, například Poznámkový blok v systému Windows [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) nebo [Nano](https://www.nano-editor.org/) v Ubuntu nebo TextEdit v systému macOS.</span><span class="sxs-lookup"><span data-stu-id="d36e4-153">toowrite Golang code, you can use a simple text editor, such as Notepad in Microsoft Windows, [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) or [Nano](https://www.nano-editor.org/) in Ubuntu, or TextEdit in macOS.</span></span> <span data-ttu-id="d36e4-154">Pokud dáváte přednost bohatšímu integrovanému vývojovému prostředí (IDE), vyzkoušejte [Gogland](https://www.jetbrains.com/go/) od JetBrains, [Visual Studio Code](https://code.visualstudio.com/) od Microsoftu nebo [Atom](https://atom.io/).</span><span class="sxs-lookup"><span data-stu-id="d36e4-154">If you prefer a richer Interactive Development Environment (IDE) try [Gogland](https://www.jetbrains.com/go/) by Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) by Microsoft, or [Atom](https://atom.io/).</span></span>
2. <span data-ttu-id="d36e4-155">Vložte hello kód přejít z hello oddílech do textových souborů a uložit do složky projektu s příponou \*.go, například Windows – cesta `%USERPROFILE%\go\src\mysqlgo\createtable.go` nebo Linux cestu `~/go/src/mysqlgo/createtable.go`.</span><span class="sxs-lookup"><span data-stu-id="d36e4-155">Paste hello Go code from hello sections below into text files, and save into your project folder with file extension \*.go, such as Windows path `%USERPROFILE%\go\src\mysqlgo\createtable.go` or Linux path `~/go/src/mysqlgo/createtable.go`.</span></span>
3. <span data-ttu-id="d36e4-156">Vyhledejte hello `HOST`, `DATABASE`, `USER`, a `PASSWORD` konstanty v hello kód a nahraďte hello ukázkové hodnoty vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="d36e4-156">Locate hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` constants in hello code, and replace hello example values with your own values.</span></span> 
4. <span data-ttu-id="d36e4-157">Spusťte příkazový řádek hello nebo bash prostředí.</span><span class="sxs-lookup"><span data-stu-id="d36e4-157">Launch hello command prompt or bash shell.</span></span> <span data-ttu-id="d36e4-158">Změňte adresář na složku vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="d36e4-158">Change directory into your project folder.</span></span> <span data-ttu-id="d36e4-159">Například ve Windows pomocí příkazu `cd %USERPROFILE%\go\src\mysqlgo\`.</span><span class="sxs-lookup"><span data-stu-id="d36e4-159">For example, on Windows `cd %USERPROFILE%\go\src\mysqlgo\`.</span></span> <span data-ttu-id="d36e4-160">V Linuxu pomocí příkazu `cd ~/go/src/mysqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="d36e4-160">On Linux `cd ~/go/src/mysqlgo/`.</span></span>  <span data-ttu-id="d36e4-161">Některé editory hello IDE uvedená nabízí možnosti ladění a runtime bez nutnosti příkazů prostředí.</span><span class="sxs-lookup"><span data-stu-id="d36e4-161">Some of hello IDE editors mentioned offer debug and runtime capabilities without requiring shell commands.</span></span>
5. <span data-ttu-id="d36e4-162">Spuštění kódu hello zadáním příkazu hello `go run createtable.go` toocompile hello aplikace a potom ho spusťte.</span><span class="sxs-lookup"><span data-stu-id="d36e4-162">Run hello code by typing hello command `go run createtable.go` toocompile hello application and run it.</span></span> 
6. <span data-ttu-id="d36e4-163">Alternativně toobuild hello kód do nativní aplikace `go build createtable.go`, spusťte `createtable.exe` toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d36e4-163">Alternatively, toobuild hello code into a native application, `go build createtable.go`, then launch `createtable.exe` toorun hello application.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="d36e4-164">Připojení, vytvoření tabulky a vložení dat</span><span class="sxs-lookup"><span data-stu-id="d36e4-164">Connect, create table, and insert data</span></span>
<span data-ttu-id="d36e4-165">Použití hello následující kód serveru toohello tooconnect, vytvořit tabulku a načtení dat pomocí hello **vložit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="d36e4-165">Use hello following code tooconnect toohello server, create a table, and load hello data using an **INSERT** SQL statement.</span></span> 

<span data-ttu-id="d36e4-166">Hello kód importuje tři balíčky: hello [balíček sql](https://golang.org/pkg/database/sql/), hello [ovladače přejděte sql pro databázi mysql](https://github.com/go-sql-driver/mysql#installation) jako ovladač toocommunicate s hello Azure Database pro databázi MySQL a hello [fmt balíček](https://golang.org/pkg/fmt/)pro tištěné vstup a výstup hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d36e4-166">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver toocommunicate with hello Azure Database for MySQL, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="d36e4-167">Hello kód zavolá metodu [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure databáze MySQL a kontroly hello připojení pomocí metody [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="d36e4-167">hello code calls method [sql.Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure Database for MySQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="d36e4-168">A [popisovač databáze](https://golang.org/pkg/database/sql/#DB) se používá napříč, která uchovává hello fondu připojení pro server databáze hello.</span><span class="sxs-lookup"><span data-stu-id="d36e4-168">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="d36e4-169">Hello kód volání hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoda několikrát toorun několik příkazy DDL.</span><span class="sxs-lookup"><span data-stu-id="d36e4-169">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method several times toorun several DDL commands.</span></span> <span data-ttu-id="d36e4-170">Hello kód také používá hello [Prepare()](http://go-database-sql.org/prepared.html) a Exec() toorun připravené příkazy s odlišnými parametry tooinsert tři řádky.</span><span class="sxs-lookup"><span data-stu-id="d36e4-170">hello code also uses hello [Prepare()](http://go-database-sql.org/prepared.html) and Exec() toorun prepared statements with different parameters tooinsert three rows.</span></span> <span data-ttu-id="d36e4-171">Pokaždé, když metoda vlastní checkError() je použité toocheck, pokud došlo k chybě a mít obavy tooexit.</span><span class="sxs-lookup"><span data-stu-id="d36e4-171">Each time a custom checkError() method is used toocheck if an error occurred and panic tooexit.</span></span>

<span data-ttu-id="d36e4-172">Nahraďte hello `host`, `database`, `user`, a `password` konstanty vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="d36e4-172">Replace hello `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

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

## <a name="read-data"></a><span data-ttu-id="d36e4-173">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="d36e4-173">Read data</span></span>
<span data-ttu-id="d36e4-174">Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="d36e4-174">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="d36e4-175">Hello kód importuje tři balíčky: hello [balíček sql](https://golang.org/pkg/database/sql/), hello [ovladače přejděte sql pro databázi mysql](https://github.com/go-sql-driver/mysql#installation) jako ovladač toocommunicate s hello Azure Database pro databázi MySQL a hello [fmt balíček](https://golang.org/pkg/fmt/)pro tištěné vstup a výstup hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d36e4-175">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver toocommunicate with hello Azure Database for MySQL, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="d36e4-176">Hello kód zavolá metodu [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure databáze MySQL a kontroly hello připojení pomocí metody [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="d36e4-176">hello code calls method [sql.Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure Database for MySQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="d36e4-177">A [popisovač databáze](https://golang.org/pkg/database/sql/#DB) se používá napříč, která uchovává hello fondu připojení pro server databáze hello.</span><span class="sxs-lookup"><span data-stu-id="d36e4-177">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="d36e4-178">Hello kód volání hello [Query()](https://golang.org/pkg/database/sql/#DB.Query) metoda toorun hello vyberte příkaz.</span><span class="sxs-lookup"><span data-stu-id="d36e4-178">hello code calls hello [Query()](https://golang.org/pkg/database/sql/#DB.Query) method toorun hello select command.</span></span> <span data-ttu-id="d36e4-179">Pak spustí [Next()](https://golang.org/pkg/database/sql/#Rows.Next) tooiterate prostřednictvím sadu výsledků hello a [Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) tooparse hodnoty ve sloupcích hello, ukládání hello hodnotu do proměnné.</span><span class="sxs-lookup"><span data-stu-id="d36e4-179">Then it runs [Next()](https://golang.org/pkg/database/sql/#Rows.Next) tooiterate through hello result set and [Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) tooparse hello column values, saving hello value into variables.</span></span> <span data-ttu-id="d36e4-180">Pokaždé, když metoda vlastní checkError() je použité toocheck, pokud došlo k chybě a mít obavy tooexit.</span><span class="sxs-lookup"><span data-stu-id="d36e4-180">Each time a custom checkError() method is used toocheck if an error occurred and panic tooexit.</span></span>

<span data-ttu-id="d36e4-181">Nahraďte hello `host`, `database`, `user`, a `password` konstanty vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="d36e4-181">Replace hello `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

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

## <a name="update-data"></a><span data-ttu-id="d36e4-182">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="d36e4-182">Update data</span></span>
<span data-ttu-id="d36e4-183">Použití hello následující kód tooconnect a aktualizovat data pomocí hello **aktualizace** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="d36e4-183">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span> 

<span data-ttu-id="d36e4-184">Hello kód importuje tři balíčky: hello [balíček sql](https://golang.org/pkg/database/sql/), hello [ovladače přejděte sql pro databázi mysql](https://github.com/go-sql-driver/mysql#installation) jako ovladač toocommunicate s hello Azure Database pro databázi MySQL a hello [fmt balíček](https://golang.org/pkg/fmt/)pro tištěné vstup a výstup hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d36e4-184">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver toocommunicate with hello Azure Database for MySQL, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="d36e4-185">Hello kód zavolá metodu [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure databáze MySQL a kontroly hello připojení pomocí metody [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="d36e4-185">hello code calls method [sql.Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure Database for MySQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="d36e4-186">A [popisovač databáze](https://golang.org/pkg/database/sql/#DB) se používá napříč, která uchovává hello fondu připojení pro server databáze hello.</span><span class="sxs-lookup"><span data-stu-id="d36e4-186">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="d36e4-187">Hello kód volání hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) příkaz metoda toorun hello aktualizace.</span><span class="sxs-lookup"><span data-stu-id="d36e4-187">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method toorun hello update command.</span></span> <span data-ttu-id="d36e4-188">Pokaždé, když metoda vlastní checkError() je použité toocheck, pokud došlo k chybě a mít obavy tooexit.</span><span class="sxs-lookup"><span data-stu-id="d36e4-188">Each time a custom checkError() method is used toocheck if an error occurred and panic tooexit.</span></span>

<span data-ttu-id="d36e4-189">Nahraďte hello `host`, `database`, `user`, a `password` konstanty vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="d36e4-189">Replace hello `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

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

## <a name="delete-data"></a><span data-ttu-id="d36e4-190">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="d36e4-190">Delete data</span></span>
<span data-ttu-id="d36e4-191">Použití hello následující kód tooconnect a odebrat pomocí data **odstranit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="d36e4-191">Use hello following code tooconnect and remove data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="d36e4-192">Hello kód importuje tři balíčky: hello [balíček sql](https://golang.org/pkg/database/sql/), hello [ovladače přejděte sql pro databázi mysql](https://github.com/go-sql-driver/mysql#installation) jako ovladač toocommunicate s hello Azure Database pro databázi MySQL a hello [fmt balíček](https://golang.org/pkg/fmt/)pro tištěné vstup a výstup hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d36e4-192">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver toocommunicate with hello Azure Database for MySQL, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="d36e4-193">Hello kód zavolá metodu [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure databáze MySQL a kontroly hello připojení pomocí metody [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="d36e4-193">hello code calls method [sql.Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure Database for MySQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="d36e4-194">A [popisovač databáze](https://golang.org/pkg/database/sql/#DB) se používá napříč, která uchovává hello fondu připojení pro server databáze hello.</span><span class="sxs-lookup"><span data-stu-id="d36e4-194">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="d36e4-195">Hello kód volání hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoda toorun hello odstranit příkaz.</span><span class="sxs-lookup"><span data-stu-id="d36e4-195">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method toorun hello delete command.</span></span> <span data-ttu-id="d36e4-196">Pokaždé, když metoda vlastní checkError() je použité toocheck, pokud došlo k chybě a mít obavy tooexit.</span><span class="sxs-lookup"><span data-stu-id="d36e4-196">Each time a custom checkError() method is used toocheck if an error occurred and panic tooexit.</span></span>

<span data-ttu-id="d36e4-197">Nahraďte hello `host`, `database`, `user`, a `password` konstanty vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="d36e4-197">Replace hello `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="d36e4-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d36e4-198">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="d36e4-199">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="d36e4-199">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
