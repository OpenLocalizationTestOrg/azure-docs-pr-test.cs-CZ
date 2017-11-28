---
title: "aaaConnect tooAzure databázi PostgreSQL pomocí jazyka přejděte | Microsoft Docs"
description: "Tento rychlý start poskytuje přejděte programovací jazyk ukázku, můžete použít tooconnect a zadávat dotazy na data z databáze Azure pro PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: go
ms.topic: quickstart
ms.date: 06/29/2017
ms.openlocfilehash: aa3c93da03116b8fcb54557494dccfad558e5f1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-go-language-tooconnect-and-query-data"></a><span data-ttu-id="2daa9-103">Azure databázi PostgreSQL: přejít pomocí jazyka tooconnect a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="2daa9-103">Azure Database for PostgreSQL: Use Go language tooconnect and query data</span></span>
<span data-ttu-id="2daa9-104">Tento rychlý start předvádí jak tooconnect tooan databáze Azure pro používání PostgreSQL kód napsaný v hello [přejděte](https://golang.org/) jazyk (golang).</span><span class="sxs-lookup"><span data-stu-id="2daa9-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using code written in hello [Go](https://golang.org/) language (golang).</span></span> <span data-ttu-id="2daa9-105">Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="2daa9-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="2daa9-106">Tento článek předpokládá, že jste obeznámeni s vývojem pomocí přejít, ale, že se nový tooworking s Azure databáze PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="2daa9-106">This article assumes you are familiar with development using Go, but that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2daa9-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2daa9-107">Prerequisites</span></span>
<span data-ttu-id="2daa9-108">Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:</span><span class="sxs-lookup"><span data-stu-id="2daa9-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="2daa9-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="2daa9-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="2daa9-110">Vytvoření databáze – rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="2daa9-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

## <a name="install-go-and-pq-connector"></a><span data-ttu-id="2daa9-111">Instalace jazyka Go a konektoru pq</span><span class="sxs-lookup"><span data-stu-id="2daa9-111">Install Go and pq connector</span></span>
<span data-ttu-id="2daa9-112">Nainstalujte [přejděte](https://golang.org/doc/install) a hello [čistý přejděte Postgres ovladačů (pq)](https://github.com/lib/pq) na vlastním počítači.</span><span class="sxs-lookup"><span data-stu-id="2daa9-112">Install [Go](https://golang.org/doc/install) and hello [Pure Go Postgres driver (pq)](https://github.com/lib/pq) on your own machine.</span></span> <span data-ttu-id="2daa9-113">V závislosti na platformě postupujte podle kroků hello:</span><span class="sxs-lookup"><span data-stu-id="2daa9-113">Depending on your platform, follow hello steps:</span></span>

### <a name="windows"></a><span data-ttu-id="2daa9-114">Windows</span><span class="sxs-lookup"><span data-stu-id="2daa9-114">Windows</span></span>
1. <span data-ttu-id="2daa9-115">[Stáhněte si](https://golang.org/dl/) a nainstalujte přejděte pro Microsoft Windows podle toohello [pokyny k instalaci](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="2daa9-115">[Download](https://golang.org/dl/) and install Go for Microsoft Windows according toohello [installation instructions](https://golang.org/doc/install).</span></span>
2. <span data-ttu-id="2daa9-116">Spusťte příkazový řádek hello z nabídky start hello.</span><span class="sxs-lookup"><span data-stu-id="2daa9-116">Launch hello command prompt from hello start menu.</span></span>
3. <span data-ttu-id="2daa9-117">Vytvořte složku pro váš projekt, například:</span><span class="sxs-lookup"><span data-stu-id="2daa9-117">Make a folder for your project such.</span></span> <span data-ttu-id="2daa9-118">`mkdir  %USERPROFILE%\go\src\postgresqlgo`.</span><span class="sxs-lookup"><span data-stu-id="2daa9-118">`mkdir  %USERPROFILE%\go\src\postgresqlgo`.</span></span>
4. <span data-ttu-id="2daa9-119">Změnit adresář, do složky hello projektu, například `cd %USERPROFILE%\go\src\postgresqlgo`.</span><span class="sxs-lookup"><span data-stu-id="2daa9-119">Change directory into hello project folder, such as `cd %USERPROFILE%\go\src\postgresqlgo`.</span></span>
5. <span data-ttu-id="2daa9-120">Nastavit proměnnou prostředí hello pro GOPATH toopoint toohello zdrojový kód adresář.</span><span class="sxs-lookup"><span data-stu-id="2daa9-120">Set hello environment variable for GOPATH toopoint toohello source code directory.</span></span> <span data-ttu-id="2daa9-121">`set GOPATH=%USERPROFILE%\go`.</span><span class="sxs-lookup"><span data-stu-id="2daa9-121">`set GOPATH=%USERPROFILE%\go`.</span></span>
6. <span data-ttu-id="2daa9-122">Nainstalujte hello [čistý přejděte Postgres ovladačů (pq)](https://github.com/lib/pq) spuštěním hello `go get github.com/lib/pq` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2daa9-122">Install hello [Pure Go Postgres driver (pq)](https://github.com/lib/pq) by running hello `go get github.com/lib/pq` command.</span></span>

   <span data-ttu-id="2daa9-123">V souhrnu nainstalujte přejděte a potom spusťte tyto příkazy v příkazovém řádku hello:</span><span class="sxs-lookup"><span data-stu-id="2daa9-123">In summary, install Go, then run these commands in hello command prompt:</span></span>
   ```cmd
   mkdir  %USERPROFILE%\go\src\postgresqlgo
   cd %USERPROFILE%\go\src\postgresqlgo
   set GOPATH=%USERPROFILE%\go
   go get github.com/lib/pq
   ```

### <a name="linux-ubuntu"></a><span data-ttu-id="2daa9-124">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="2daa9-124">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="2daa9-125">Spusťte prostředí Bash hello.</span><span class="sxs-lookup"><span data-stu-id="2daa9-125">Launch hello Bash shell.</span></span> 
2. <span data-ttu-id="2daa9-126">Nainstalujte jazyk Go spuštěním příkazu `sudo apt-get install golang-go`.</span><span class="sxs-lookup"><span data-stu-id="2daa9-126">Install Go by running `sudo apt-get install golang-go`.</span></span>
3. <span data-ttu-id="2daa9-127">Ve svém domovském adresáři vytvořte složku pro projekt, například `mkdir -p ~/go/src/postgresqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="2daa9-127">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/postgresqlgo/`.</span></span>
4. <span data-ttu-id="2daa9-128">Změnit adresář, jako například do složky hello `cd ~/go/src/postgresqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="2daa9-128">Change directory into hello folder, such as `cd ~/go/src/postgresqlgo/`.</span></span>
5. <span data-ttu-id="2daa9-129">Sada hello GOPATH prostředí proměnné toopoint tooa platný zdrojový adresář, jako je vaše aktuální domovské adresáře přejít složku.</span><span class="sxs-lookup"><span data-stu-id="2daa9-129">Set hello GOPATH environment variable toopoint tooa valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="2daa9-130">V prostředí bash hello, spusťte `export GOPATH=~/go` tooadd hello přejděte directory jako hello GOPATH pro aktuální relaci prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="2daa9-130">At hello bash shell, run `export GOPATH=~/go` tooadd hello go directory as hello GOPATH for hello current shell session.</span></span>
6. <span data-ttu-id="2daa9-131">Nainstalujte hello [čistý přejděte Postgres ovladačů (pq)](https://github.com/lib/pq) spuštěním hello `go get github.com/lib/pq` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2daa9-131">Install hello [Pure Go Postgres driver (pq)](https://github.com/lib/pq) by running hello `go get github.com/lib/pq` command.</span></span>

   <span data-ttu-id="2daa9-132">Stručně řečeno, spusťte tyto příkazy Bash:</span><span class="sxs-lookup"><span data-stu-id="2daa9-132">In summary, run these bash commands:</span></span>
   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/postgresqlgo/
   cd ~/go/src/postgresqlgo/
   export GOPATH=~/go/
   go get github.com/lib/pq
   ```

### <a name="apple-macos"></a><span data-ttu-id="2daa9-133">Apple macOS</span><span class="sxs-lookup"><span data-stu-id="2daa9-133">Apple macOS</span></span>
1. <span data-ttu-id="2daa9-134">Stáhněte a nainstalujte přejděte podle toohello [pokyny k instalaci](https://golang.org/doc/install) odpovídající vaši platformu.</span><span class="sxs-lookup"><span data-stu-id="2daa9-134">Download and install Go according toohello [installation instructions](https://golang.org/doc/install)  matching your platform.</span></span> 
2. <span data-ttu-id="2daa9-135">Spusťte prostředí Bash hello.</span><span class="sxs-lookup"><span data-stu-id="2daa9-135">Launch hello Bash shell.</span></span> 
3. <span data-ttu-id="2daa9-136">Ve svém domovském adresáři vytvořte složku pro projekt, například `mkdir -p ~/go/src/postgresqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="2daa9-136">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/postgresqlgo/`.</span></span>
4. <span data-ttu-id="2daa9-137">Změnit adresář, jako například do složky hello `cd ~/go/src/postgresqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="2daa9-137">Change directory into hello folder, such as `cd ~/go/src/postgresqlgo/`.</span></span>
5. <span data-ttu-id="2daa9-138">Sada hello GOPATH prostředí proměnné toopoint tooa platný zdrojový adresář, jako je vaše aktuální domovské adresáře přejít složku.</span><span class="sxs-lookup"><span data-stu-id="2daa9-138">Set hello GOPATH environment variable toopoint tooa valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="2daa9-139">V prostředí bash hello, spusťte `export GOPATH=~/go` tooadd hello přejděte directory jako hello GOPATH pro aktuální relaci prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="2daa9-139">At hello bash shell, run `export GOPATH=~/go` tooadd hello go directory as hello GOPATH for hello current shell session.</span></span>
6. <span data-ttu-id="2daa9-140">Nainstalujte hello [čistý přejděte Postgres ovladačů (pq)](https://github.com/lib/pq) spuštěním hello `go get github.com/lib/pq` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2daa9-140">Install hello [Pure Go Postgres driver (pq)](https://github.com/lib/pq) by running hello `go get github.com/lib/pq` command.</span></span>

   <span data-ttu-id="2daa9-141">Stručně řečeno, nainstalujte jazyk Go a potom spusťte tyto příkazy Bash:</span><span class="sxs-lookup"><span data-stu-id="2daa9-141">In summary, install Go, then run these bash commands:</span></span>
   ```bash
   mkdir -p ~/go/src/postgresqlgo/
   cd ~/go/src/postgresqlgo/
   export GOPATH=~/go/
   go get github.com/lib/pq
   ```

## <a name="get-connection-information"></a><span data-ttu-id="2daa9-142">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="2daa9-142">Get connection information</span></span>
<span data-ttu-id="2daa9-143">Získáte hello připojení informace potřebné tooconnect toohello databáze Azure pro PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="2daa9-143">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="2daa9-144">Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="2daa9-144">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="2daa9-145">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2daa9-145">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="2daa9-146">Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru, které jste vytvořili, například **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="2daa9-146">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="2daa9-147">Klikněte na název serveru hello **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="2daa9-147">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="2daa9-148">Vyberte hello serveru **přehled** stránky.</span><span class="sxs-lookup"><span data-stu-id="2daa9-148">Select hello server's **Overview** page.</span></span> <span data-ttu-id="2daa9-149">Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.</span><span class="sxs-lookup"><span data-stu-id="2daa9-149">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="2daa9-150">![Azure Database for PostgreSQL – přihlášení správce serveru](./media/connect-go/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="2daa9-150">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-go/1-connection-string.png)</span></span>
5. <span data-ttu-id="2daa9-151">Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránky a zobrazení hello serveru správce přihlašovací jméno.</span><span class="sxs-lookup"><span data-stu-id="2daa9-151">If you forget your server login information, navigate toohello **Overview** page, and view hello Server admin login name.</span></span> <span data-ttu-id="2daa9-152">V případě potřeby resetovat heslo hello.</span><span class="sxs-lookup"><span data-stu-id="2daa9-152">If necessary, reset hello password.</span></span>

## <a name="build-and-run-go-code"></a><span data-ttu-id="2daa9-153">Sestavení a spuštění kódu jazyka Go</span><span class="sxs-lookup"><span data-stu-id="2daa9-153">Build and run Go code</span></span> 
1. <span data-ttu-id="2daa9-154">toowrite Golang kódu, můžete použít jednoduchý textový editor, například Poznámkový blok v systému Windows [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) nebo [Nano](https://www.nano-editor.org/) v Ubuntu nebo TextEdit v systému macOS.</span><span class="sxs-lookup"><span data-stu-id="2daa9-154">toowrite Golang code, you can use a simple text editor, such as Notepad in Microsoft Windows, [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) or [Nano](https://www.nano-editor.org/) in Ubuntu, or TextEdit in macOS.</span></span> <span data-ttu-id="2daa9-155">Pokud dáváte přednost bohatšímu integrovanému vývojovému prostředí (IDE), vyzkoušejte [Gogland](https://www.jetbrains.com/go/) od JetBrains, [Visual Studio Code](https://code.visualstudio.com/) od Microsoftu nebo [Atom](https://atom.io/).</span><span class="sxs-lookup"><span data-stu-id="2daa9-155">If you prefer a richer Interactive Development Environment (IDE) try [Gogland](https://www.jetbrains.com/go/) by Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) by Microsoft, or [Atom](https://atom.io/).</span></span>
2. <span data-ttu-id="2daa9-156">Vložte kód Golang hello z hello oddílech do textových souborů a uložit do složky projektu s příponou \*.go, například Windows – cesta `%USERPROFILE%\go\src\postgresqlgo\createtable.go` nebo Linux cestu `~/go/src/postgresqlgo/createtable.go`.</span><span class="sxs-lookup"><span data-stu-id="2daa9-156">Paste hello Golang code from hello sections below into text files, and save into your project folder with file extension \*.go, such as Windows path `%USERPROFILE%\go\src\postgresqlgo\createtable.go` or Linux path `~/go/src/postgresqlgo/createtable.go`.</span></span>
3. <span data-ttu-id="2daa9-157">Vyhledejte hello `HOST`, `DATABASE`, `USER`, a `PASSWORD` konstanty v hello kód a nahraďte hello ukázkové hodnoty vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="2daa9-157">Locate hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` constants in hello code, and replace hello example values with your own values.</span></span>  
4. <span data-ttu-id="2daa9-158">Spusťte příkazový řádek hello nebo bash prostředí.</span><span class="sxs-lookup"><span data-stu-id="2daa9-158">Launch hello command prompt or bash shell.</span></span> <span data-ttu-id="2daa9-159">Změňte adresář na složku vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="2daa9-159">Change directory into your project folder.</span></span> <span data-ttu-id="2daa9-160">Například ve Windows pomocí příkazu `cd %USERPROFILE%\go\src\postgresqlgo\`.</span><span class="sxs-lookup"><span data-stu-id="2daa9-160">For example, on Windows `cd %USERPROFILE%\go\src\postgresqlgo\`.</span></span> <span data-ttu-id="2daa9-161">V Linuxu pomocí příkazu `cd ~/go/src/postgresqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="2daa9-161">On Linux `cd ~/go/src/postgresqlgo/`.</span></span> <span data-ttu-id="2daa9-162">Některé hello prostředí IDE uvedená nabízí možnosti ladění a runtime bez nutnosti příkazů prostředí.</span><span class="sxs-lookup"><span data-stu-id="2daa9-162">Some of hello IDE environments mentioned offer debug and runtime capabilities without requiring shell commands.</span></span>
5. <span data-ttu-id="2daa9-163">Spuštění kódu hello zadáním příkazu hello `go run createtable.go` toocompile hello aplikace a potom ho spusťte.</span><span class="sxs-lookup"><span data-stu-id="2daa9-163">Run hello code by typing hello command `go run createtable.go` toocompile hello application and run it.</span></span> 
6. <span data-ttu-id="2daa9-164">Alternativně toobuild hello kód do nativní aplikace `go build createtable.go`, spusťte `createtable.exe` toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2daa9-164">Alternatively, toobuild hello code into a native application, `go build createtable.go`, then launch `createtable.exe` toorun hello application.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="2daa9-165">Připojení a vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="2daa9-165">Connect and create a table</span></span>
<span data-ttu-id="2daa9-166">Použití hello následující kód tooconnect a vytvořte tabulku pomocí **CREATE TABLE** příkaz jazyka SQL, za nímž následuje **INSERT INTO** SQL příkazy tooadd řádků do tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="2daa9-166">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements tooadd rows into hello table.</span></span>

<span data-ttu-id="2daa9-167">Hello kód importuje tři balíčky: hello [balíček sql](https://golang.org/pkg/database/sql/), hello [pq balíček](http://godoc.org/github.com/lib/pq) jako ovladač toocommunicate s hello Postgres serveru a hello [fmt balíček](https://golang.org/pkg/fmt/) pro vytisknout vstup a výstup hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="2daa9-167">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [pq package](http://godoc.org/github.com/lib/pq) as a driver toocommunicate with hello Postgres server, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="2daa9-168">Hello kód zavolá metodu [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure databáze PostgreSQL a kontroly hello připojení pomocí metody [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="2daa9-168">hello code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure Database for PostgreSQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="2daa9-169">A [popisovač databáze](https://golang.org/pkg/database/sql/#DB) se používá napříč, která uchovává hello fondu připojení pro server databáze hello.</span><span class="sxs-lookup"><span data-stu-id="2daa9-169">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="2daa9-170">Hello kód volání hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoda několikrát toorun několik příkazů SQL.</span><span class="sxs-lookup"><span data-stu-id="2daa9-170">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method several times toorun several SQL commands.</span></span> <span data-ttu-id="2daa9-171">Každý čas vlastní checkError() metoda toocheck, pokud došlo k chybě a nouzové tooexit, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="2daa9-171">Each time a custom checkError() method toocheck if an error occurred and panic tooexit if an error occurs.</span></span>

<span data-ttu-id="2daa9-172">Nahraďte hello `HOST`, `DATABASE`, `USER`, a `PASSWORD` parametry s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="2daa9-172">Replace hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/lib/pq"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    // Initialize connection string.
    var connectionString string = fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase")

    // Drop previous table of same name if one exists.
    _, err = db.Exec("DROP TABLE IF EXISTS inventory;")
    checkError(err)
    fmt.Println("Finished dropping table (if existed)")

    // Create table.
    _, err = db.Exec("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
    checkError(err)
    fmt.Println("Finished creating table")

    // Insert some data into table.
    sql_statement := "INSERT INTO inventory (name, quantity) VALUES ($1, $2);"
    _, err = db.Exec(sql_statement, "banana", 150)
    checkError(err)
    _, err = db.Exec(sql_statement, "orange", 154)
    checkError(err)
    _, err = db.Exec(sql_statement, "apple", 100)
    checkError(err)
    fmt.Println("Inserted 3 rows of data")
}
```

## <a name="read-data"></a><span data-ttu-id="2daa9-173">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="2daa9-173">Read data</span></span>
<span data-ttu-id="2daa9-174">Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="2daa9-174">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="2daa9-175">Hello kód importuje tři balíčky: hello [balíček sql](https://golang.org/pkg/database/sql/), hello [pq balíček](http://godoc.org/github.com/lib/pq) jako ovladač toocommunicate s hello Postgres serveru a hello [fmt balíček](https://golang.org/pkg/fmt/) pro vytisknout vstup a výstup hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="2daa9-175">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [pq package](http://godoc.org/github.com/lib/pq) as a driver toocommunicate with hello Postgres server, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="2daa9-176">Hello kód zavolá metodu [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure databáze PostgreSQL a kontroly hello připojení pomocí metody [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="2daa9-176">hello code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure Database for PostgreSQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="2daa9-177">A [popisovač databáze](https://golang.org/pkg/database/sql/#DB) se používá napříč, která uchovává hello fondu připojení pro server databáze hello.</span><span class="sxs-lookup"><span data-stu-id="2daa9-177">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="2daa9-178">spuštění dotazu vyberte Hello voláním metody [db. Query()](https://golang.org/pkg/database/sql/#DB.Query), a výsledné řádky hello je uložen v proměnné typu [řádky](https://golang.org/pkg/database/sql/#Rows).</span><span class="sxs-lookup"><span data-stu-id="2daa9-178">hello select query is run by calling method [db.Query()](https://golang.org/pkg/database/sql/#DB.Query), and hello resulting rows is kept in a variable of type [rows](https://golang.org/pkg/database/sql/#Rows).</span></span> <span data-ttu-id="2daa9-179">Hello kód čte data hodnoty ve sloupcích hello hello aktuálním řádku pomocí metody [řádků. Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) a smyčky přes hello řádky pomocí hello iterator [řádků. Next()](https://golang.org/pkg/database/sql/#Rows.Next) dokud neexistují žádné další řádky.</span><span class="sxs-lookup"><span data-stu-id="2daa9-179">hello code reads hello column data values in hello current row using method [rows.Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) and loops over hello rows using hello iterator [rows.Next()](https://golang.org/pkg/database/sql/#Rows.Next) until no more rows exist.</span></span> <span data-ttu-id="2daa9-180">Každý řádek hodnot sloupce jsou tištěné toohello konzoly se. Každý čas vlastní checkError() metoda toocheck, pokud došlo k chybě a nouzové tooexit, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="2daa9-180">Each row's column values are printed toohello console out. Each time a custom checkError() method toocheck if an error occurred and panic tooexit if an error occurs.</span></span>

<span data-ttu-id="2daa9-181">Nahraďte hello `HOST`, `DATABASE`, `USER`, a `PASSWORD` parametry s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="2daa9-181">Replace hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/lib/pq"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString string = fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase")

    // Read rows from table.
    var id int
    var name string
    var quantity int

    sql_statement := "SELECT * from inventory;"
    rows, err := db.Query(sql_statement)
    checkError(err)

    for rows.Next() {
        switch err := rows.Scan(&id, &name, &quantity); err {
        case sql.ErrNoRows:
            fmt.Println("No rows were returned")
        case nil:
            fmt.Printf("Data row = (%d, %s, %d)\n", id, name, quantity)
        default:
            checkError(err)
        }
    }
}
```

## <a name="update-data"></a><span data-ttu-id="2daa9-182">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="2daa9-182">Update data</span></span>
<span data-ttu-id="2daa9-183">Použití hello následující kód tooconnect a aktualizovat data pomocí hello **aktualizace** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="2daa9-183">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="2daa9-184">Hello kód importuje tři balíčky: hello [balíček sql](https://golang.org/pkg/database/sql/), hello [pq balíček](http://godoc.org/github.com/lib/pq) jako ovladač toocommunicate s hello Postgres serveru a hello [fmt balíček](https://golang.org/pkg/fmt/) pro vytisknout vstup a výstup hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="2daa9-184">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [pq package](http://godoc.org/github.com/lib/pq) as a driver toocommunicate with hello Postgres server, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="2daa9-185">Hello kód zavolá metodu [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure databáze PostgreSQL a kontroly hello připojení pomocí metody [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="2daa9-185">hello code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure Database for PostgreSQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="2daa9-186">A [popisovač databáze](https://golang.org/pkg/database/sql/#DB) se používá napříč, která uchovává hello fondu připojení pro server databáze hello.</span><span class="sxs-lookup"><span data-stu-id="2daa9-186">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="2daa9-187">Hello kód volání hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoda toorun hello příkaz jazyka SQL, který aktualizuje hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="2daa9-187">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method toorun hello SQL statement that updates hello table.</span></span> <span data-ttu-id="2daa9-188">Metoda toocheck vlastní checkError(), pokud došlo k chybě a pokud chybu nouzové tooexit dojde.</span><span class="sxs-lookup"><span data-stu-id="2daa9-188">A custom checkError() method toocheck if an error occurred and panic tooexit if an error occurs.</span></span>

<span data-ttu-id="2daa9-189">Nahraďte hello `HOST`, `DATABASE`, `USER`, a `PASSWORD` parametry s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="2daa9-189">Replace hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 
```go
package main

import (
  "database/sql"
  _ "github.com/lib/pq"
  "fmt"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    
    // Initialize connection string.
    var connectionString string = 
        fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase")

    // Modify some data in table.
    sql_statement := "UPDATE inventory SET quantity = $2 WHERE name = $1;"
    _, err = db.Exec(sql_statement, "banana", 200)
    checkError(err)
    fmt.Println("Updated 1 row of data")
}
```

## <a name="delete-data"></a><span data-ttu-id="2daa9-190">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="2daa9-190">Delete data</span></span>
<span data-ttu-id="2daa9-191">Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="2daa9-191">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="2daa9-192">Hello kód importuje tři balíčky: hello [balíček sql](https://golang.org/pkg/database/sql/), hello [pq balíček](http://godoc.org/github.com/lib/pq) jako ovladač toocommunicate s hello Postgres serveru a hello [fmt balíček](https://golang.org/pkg/fmt/) pro vytisknout vstup a výstup hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="2daa9-192">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [pq package](http://godoc.org/github.com/lib/pq) as a driver toocommunicate with hello Postgres server, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="2daa9-193">Hello kód zavolá metodu [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure databáze PostgreSQL a kontroly hello připojení pomocí metody [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="2daa9-193">hello code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure Database for PostgreSQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="2daa9-194">A [popisovač databáze](https://golang.org/pkg/database/sql/#DB) se používá napříč, která uchovává hello fondu připojení pro server databáze hello.</span><span class="sxs-lookup"><span data-stu-id="2daa9-194">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="2daa9-195">Hello kód volání hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoda toorun hello příkaz jazyka SQL, který aktualizuje hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="2daa9-195">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method toorun hello SQL statement that updates hello table.</span></span> <span data-ttu-id="2daa9-196">Metoda toocheck vlastní checkError(), pokud došlo k chybě a pokud chybu nouzové tooexit dojde.</span><span class="sxs-lookup"><span data-stu-id="2daa9-196">A custom checkError() method toocheck if an error occurred and panic tooexit if an error occurs.</span></span>

<span data-ttu-id="2daa9-197">Nahraďte hello `HOST`, `DATABASE`, `USER`, a `PASSWORD` parametry s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="2daa9-197">Replace hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 
```go
package main

import (
  "database/sql"
  _ "github.com/lib/pq"
  "fmt"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    
    // Initialize connection string.
    var connectionString string = 
        fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase")

    // Delete some data from table.
    sql_statement := "DELETE FROM inventory WHERE name = $1;"
    _, err = db.Exec(sql_statement, "orange")
    checkError(err)
    fmt.Println("Deleted 1 row of data")
}
```

## <a name="next-steps"></a><span data-ttu-id="2daa9-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2daa9-198">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="2daa9-199">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="2daa9-199">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
