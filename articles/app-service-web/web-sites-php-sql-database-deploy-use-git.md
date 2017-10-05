---
title: "Vytvoření webové aplikace PHP-SQL a nasazení do Azure App Service pomocí Git"
description: "Kurz ukazuje, jak vytvořit webové aplikace PHP, která ukládá data v databázi SQL Azure a použít nasazení Git do služby Azure App Service."
services: app-service\web, sql-database
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 6b090bf6-31d8-4b74-81eb-050ef95929ca
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 0baa3eced3824fec0907ca937c594f127a2bdf8b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-php-sql-web-app-and-deploy-to-azure-app-service-using-git"></a>Vytvoření webové aplikace PHP-SQL a nasazení do Azure App Service pomocí Git
V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci PHP v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) která se připojuje k databázi SQL Azure a jak se dá nasadit pomocí Git. Tento kurz předpokládá, že máte [PHP][install-php], [SQL Server Express][install-SQLExpress], [Drivers společnosti Microsoft pro systém SQL Server pro jazyk PHP ](http://www.microsoft.com/download/en/details.aspx?id=20098), a [Git] [ install-git] v počítači nainstalována. Po dokončení tohoto průvodce, budete mít webovou aplikaci PHP SQL běžící v Azure.

> [!NOTE]
> Můžete nainstalovat a nakonfigurovat PHP, SQL Server Express a Drivers společnosti Microsoft pro systém SQL Server pro PHP pomocí [instalačního programu webové platformy Microsoft](http://www.microsoft.com/web/downloads/platform.aspx).
> 
> 

Co se dozvíte:

* Postup vytvoření webové aplikace Azure a SQL Database pomocí [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715). Protože PHP je ve výchozím nastavení povolená v App Service Web Apps, žádnou zvláštní akci je potřeba spustit kódu PHP.
* Postup publikování a znovu publikovat svoji aplikaci do Azure pomocí Git.

V následujícím kurzu, bude vytvoření jednoduché registrace webové aplikace v jazyce PHP. Aplikace se bude hostovat na webu Azure. Zde je snímek obrazovky dokončené aplikace:

![Webové stránky Azure PHP](./media/web-sites-php-sql-database-deploy-use-git/running_app_3.png)

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="create-an-azure-web-app-and-set-up-git-publishing"></a>Vytvoření webové aplikace Azure a nastavení publikování Git
Postupujte podle těchto kroků můžete vytvořit webové aplikace Azure a SQL Database:

1. Přihlaste se k [Azure Portal](https://portal.azure.com/).
2. Otevřít v Azure Marketplace kliknutím **nový** ikonu na horní pravé řídicí panel, klikněte na **Vybrat vše** vedle Marketplace a výběrem **Web + mobilní**.
3. Na webu Marketplace, vyberte **Web + mobilní**.
4. Klikněte **Web app + SQL** ikonu.
5. Po přečtení popis Web app + SQL aplikace, vyberte **vytvořit**.
6. Klikněte na každou část (**skupiny prostředků**, **webové aplikace**, **databáze**, a **předplatné**) a zadejte nebo vyberte hodnoty pro povinná pole :
   
   * Zadejte název adresy URL podle svého výběru    
   * Konfigurovat přihlašovací údaje serveru databáze
   * Vyberte oblast, která je nejblíže k vám
     
     ![Konfigurace aplikace](./media/web-sites-php-sql-database-deploy-use-git/configure-db-settings.png)
7. Po dokončení definování webové aplikace, klikněte na tlačítko **vytvořit**.
   
    Po vytvoření webové aplikace **oznámení** tlačítko bude flash zelená **úspěch** a otevřete okno skupiny prostředků k zobrazení webové aplikace a databáze SQL ve skupině.
8. Klikněte na ikonu webové aplikace v okně skupiny prostředků, otevřete okno webové aplikace.
   
    ![webové aplikace skupiny prostředků](./media/web-sites-php-sql-database-deploy-use-git/resource-group-blade.png)
9. V **nastavení** klikněte na tlačítko **průběžné nasazování** > **konfigurovat požadované nastavení**. Vyberte **místní úložiště Git** a klikněte na tlačítko **OK**.
   
    ![kde je vašeho zdrojového kódu](./media/web-sites-php-sql-database-deploy-use-git/setup-local-git.png)
   
    Pokud jste nenastavili úložiště Git před, je nutné zadat uživatelské jméno a heslo. Chcete-li to provést, klikněte na tlačítko **nastavení** > **přihlašovací údaje nasazení** v okně webové aplikace.
   
    ![](./media/web-sites-php-sql-database-deploy-use-git/deployment-credentials.png)
10. V **nastavení** klikněte na **vlastnosti** zobrazíte vzdálené adresy URL pro Git budete muset použít k nasazení své aplikace PHP později.

## <a name="get-sql-database-connection-information"></a>Získat informace o připojení k databázi SQL
Pro připojení k instanci databáze SQL, která je propojený s vaší webové aplikace, vaše bude potřebovat informace o připojení, který jste zadali při vytvoření databáze. Chcete-li získat informace o připojení databáze SQL, postupujte takto:

1. Zpět v okně skupiny prostředků klikněte na ikonu databáze SQL.
2. V okně databáze SQL, klikněte na tlačítko **nastavení** > **vlastnosti**, pak klikněte na tlačítko **zobrazit databázové připojovací řetězce**. 
   
    ![Zobrazit vlastnosti databáze](./media/web-sites-php-sql-database-deploy-use-git/view-database-properties.png)
3. Z **PHP** části výsledné dialogového okna, poznamenejte si hodnoty pro `Server`, `SQL Database`, a `User Name`. Tyto hodnoty budou pozdější použití při publikování webové aplikace PHP do služby Azure App Service.

## <a name="build-and-test-your-application-locally"></a>Místní sestavení a testování aplikace
Registrace aplikace je jednoduchou aplikaci PHP, která umožňuje zaregistrovat pro událost tím, že poskytuje vaše jméno a e-mailovou adresu. Zobrazí se informace o předchozích rejstříkem v tabulce. Registrační informace jsou uloženy v instanci databáze SQL. Aplikace se skládá ze dvou souborech (zkopírujte a vložte kód je k dispozici níže):

* **index.php**: Zobrazí formulář pro registraci a tabulku obsahující informace osob žádajících o registraci.
* **CreateTable.php**: vytvoří v tabulce databáze SQL pro aplikaci. Tento soubor se dají použít jenom jednou.

Spusťte aplikaci místně, postupujte podle následujících kroků. Všimněte si, že tento postup předpokládá, máte PHP a SQL Server Express na místním počítači a pokud jste povolili [PDO rozšíření pro SQL Server][pdo-sqlsrv].

1. Vytvořit databázi systému SQL Server volá `registration`. Můžete to provést z `sqlcmd` příkazového řádku pomocí těchto příkazů:
   
        >sqlcmd -S localhost\sqlexpress -U <local user name> -P <local password>
        1> create database registration
        2> GO    
2. V kořenovém adresáři aplikace, vytvořte dva soubory v ní – jednu s názvem `createtable.php` a jednu s názvem `index.php`.
3. Otevřete `createtable.php` soubor v textovém editoru nebo IDE a přidejte následující kód. Tento kód se použije k vytvoření `registration_tbl` tabulky v `registration` databáze.
   
        <?php
        // DB connection info
        $host = "localhost\sqlexpress";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "sqlsrv:Server= $host ; Database = $db ", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
            id INT NOT NULL IDENTITY(1,1) 
            PRIMARY KEY(id),
            name VARCHAR(30),
            email VARCHAR(30),
            date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>
   
    Všimněte si, že budete muset aktualizovat hodnoty <code>$user</code> a <code>$pwd</code> s vaší místní systém SQL Server uživatelské jméno a heslo.
4. V terminálu v kořenovém adresáři aplikace zadejte následující příkaz:
   
        php -S localhost:8000
5. Otevřete webový prohlížeč a přejděte do **http://localhost:8000/createtable.php**. Tím se vytvoří `registration_tbl` tabulky v databázi.
6. Otevřete **index.php** soubor v textovém editoru nebo IDE a přidat základní HTML a CSS kódu stránky (v následujících krocích se přidají kód PHP.).
   
        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php
   
        ?>
        </body>
        </html>
7. V rámci značky PHP přidejte kód PHP pro připojení k databázi.
   
        // DB connection info
        $host = "localhost\sqlexpress";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "sqlsrv:Server= $host ; Database = $db ", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }
   
    Znovu, budete muset aktualizovat hodnoty <code>$user</code> a <code>$pwd</code> s vaší místní MySQL uživatelské jméno a heslo.
8. Následující kód připojení databáze přidáte kód pro vložení registrační informace do databáze.
   
        if(!empty($_POST)) {
        try {
            $name = $_POST['name'];
            $email = $_POST['email'];
            $date = date("Y-m-d");
            // Insert data
            $sql_insert = "INSERT INTO registration_tbl (name, email, date) 
                           VALUES (?,?,?)";
            $stmt = $conn->prepare($sql_insert);
            $stmt->bindValue(1, $name);
            $stmt->bindValue(2, $email);
            $stmt->bindValue(3, $date);
            $stmt->execute();
        }
        catch(Exception $e) {
            die(var_dump($e));
        }
        echo "<h3>Your're registered!</h3>";
        }
9. Nakonec následující kód výše, přidejte kód pro načítání dat z databáze.
   
        $sql_select = "SELECT * FROM registration_tbl";
        $stmt = $conn->query($sql_select);
        $registrants = $stmt->fetchAll(); 
        if(count($registrants) > 0) {
            echo "<h2>People who are registered:</h2>";
            echo "<table>";
            echo "<tr><th>Name</th>";
            echo "<th>Email</th>";
            echo "<th>Date</th></tr>";
            foreach($registrants as $registrant) {
                echo "<tr><td>".$registrant['name']."</td>";
                echo "<td>".$registrant['email']."</td>";
                echo "<td>".$registrant['date']."</td></tr>";
            }
             echo "</table>";
        } else {
            echo "<h3>No one is currently registered.</h3>";
        }

Nyní můžete procházet k **http://localhost:8000/index.php** k testování aplikace.

## <a name="publish-your-application"></a>Publikování aplikace
Po testování vaší aplikace místně, můžete ho publikovat App Service Web Apps pomocí Git. Však musíte nejprve aktualizovat informace o připojení databáze v aplikaci. Pomocí informací o připojení databáze jste dříve získali (v **informace o připojení k databázi SQL získat** část), aktualizujte následující informace v **i** `createdatabase.php` a `index.php` soubory s příslušnými hodnotami:

    // DB connection info
    $host = "tcp:<value of Server>";
    $user = "<value of User Name>";
    $pwd = "<your password>";
    $db = "<value of SQL Database>";

> [!NOTE]
> V <code>$host</code>, musí být předponou hodnotu serveru <code>tcp:</code>.
> 
> 

Nyní jste připraveni k nastavení publikování Git a publikování aplikace.

> [!NOTE]
> Jedná se o tytéž kroky uvedené na konci **vytvoření webové aplikace Azure a nastavení publikování Git** část výše.
> 
> 

1. Otevřete Git Bash (nebo terminál, pokud Git je ve vaší `PATH`), změňte adresáře na kořenový adresář aplikace ( **registrace** directory), a spusťte následující příkazy:
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    Zobrazí se výzva k zadání hesla, které jste vytvořili dříve.
2. Přejděte do **http://[web aplikace name].azurewebsites.net/createtable.php** k vytvoření tabulky databáze SQL pro aplikaci.
3. Přejděte do **http://[web aplikace name].azurewebsites.net/index.php** začít používat aplikaci.

Po publikování aplikace, můžete začít, provedení změn a publikovat je pomocí Git. 

## <a name="publish-changes-to-your-application"></a>Publikování změn do aplikace
Při publikování změn do aplikace, postupujte takto:

1. Měnit svou aplikaci lokálně.
2. Otevřete Git Bash (nebo terminál, it Git je ve vaší `PATH`), změňte adresáře na kořenový adresář aplikace a spusťte následující příkazy:
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    Zobrazí se výzva k zadání hesla, které jste vytvořili dříve.
3. Přejděte do **http://[web aplikace name].azurewebsites.net/index.php** vaše změny zobrazily.

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[pdo-sqlsrv]: http://php.net/pdo_sqlsrv

