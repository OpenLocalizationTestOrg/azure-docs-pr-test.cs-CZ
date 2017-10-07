---
title: "aaaCreate PHP-SQL webová aplikace a nasazení tooAzure služby App Service pomocí Git"
description: "Kurz, který ukazuje, jak toocreate PHP webová aplikace, která ukládá data v databázi SQL Azure a použít tooAzure nasazení Git služby App Service."
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
ms.openlocfilehash: aaacb2fe0787bbcdafa72871912e8d08792be29d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-php-sql-web-app-and-deploy-tooazure-app-service-using-git"></a>Vytvoření webové aplikace PHP-SQL a nasazení tooAzure služby App Service pomocí Git
Tento kurz ukazuje, jak toocreate PHP webové aplikace v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) která se připojuje tooAzure databáze SQL a jak toodeploy pomocí Git. Tento kurz předpokládá, že máte [PHP][install-php], [SQL Server Express][install-SQLExpress], hello [Drivers společnosti Microsoft pro systém SQL Server pro jazyk PHP ](http://www.microsoft.com/download/en/details.aspx?id=20098), a [Git] [ install-git] v počítači nainstalována. Po dokončení tohoto průvodce, budete mít webovou aplikaci PHP SQL běžící v Azure.

> [!NOTE]
> Můžete nainstalovat a nakonfigurovat PHP, SQL Server Express a hello Drivers společnosti Microsoft pro systém SQL Server pro jazyk PHP pomocí hello [instalačního programu webové platformy Microsoft](http://www.microsoft.com/web/downloads/platform.aspx).
> 
> 

Co se dozvíte:

* Jak toocreate Azure webové aplikace a SQL Database pomocí hello [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715). Protože PHP je ve výchozím nastavení povolená v App Service Web Apps, nic speciální je požadovaná toorun kódu PHP.
* Jak toopublish a znovu publikovat tooAzure vaší aplikace pomocí Git.

V následujícím kurzu, bude vytvoření jednoduché registrace webové aplikace v jazyce PHP. aplikace Hello se bude hostovat na webu Azure. Zde je snímek obrazovky aplikace hello dokončena:

![Webové stránky Azure PHP](./media/web-sites-php-sql-database-deploy-use-git/running_app_3.png)

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="create-an-azure-web-app-and-set-up-git-publishing"></a>Vytvoření webové aplikace Azure a nastavení publikování Git
Postupujte podle těchto kroků toocreate webové aplikace Azure a SQL Database:

1. Přihlaste se toohello [portálu Azure](https://portal.azure.com/).
2. Otevřete hello Azure Marketplace kliknutím hello **nový** ikonu hello nahoře vlevo hello řídicí panel, klikněte na **Vybrat vše** další tooMarketplace a výběrem **Web + mobilní**.
3. V hello Marketplace, vyberte **Web + mobilní**.
4. Klikněte na tlačítko hello **Web app + SQL** ikonu.
5. Po přečtení hello popis hello Web app + SQL aplikace, vyberte **vytvořit**.
6. Klikněte na každou část (**skupiny prostředků**, **webové aplikace**, **databáze**, a **předplatné**) a zadejte nebo vyberte hodnoty pro požadované hello pole:
   
   * Zadejte název adresy URL podle svého výběru    
   * Konfigurovat přihlašovací údaje serveru databáze
   * Vyberte nejbližší tooyou oblast hello
     
     ![Konfigurace aplikace](./media/web-sites-php-sql-database-deploy-use-git/configure-db-settings.png)
7. Po dokončení definování hello webové aplikace, klikněte na tlačítko **vytvořit**.
   
    Po vytvoření webové aplikace hello hello **oznámení** tlačítko bude flash zelená **úspěch** a hello prostředků skupiny okno otevřít tooshow obou hello webové aplikace a hello databáze SQL ve skupině hello.
8. Klikněte na ikonu hello webové aplikace v okně hello prostředků skupiny okno tooopen hello webové aplikace.
   
    ![webové aplikace skupiny prostředků](./media/web-sites-php-sql-database-deploy-use-git/resource-group-blade.png)
9. V **nastavení** klikněte na tlačítko **průběžné nasazování** > **konfigurovat požadované nastavení**. Vyberte **místní úložiště Git** a klikněte na tlačítko **OK**.
   
    ![kde je vašeho zdrojového kódu](./media/web-sites-php-sql-database-deploy-use-git/setup-local-git.png)
   
    Pokud jste nenastavili úložiště Git před, je nutné zadat uživatelské jméno a heslo. toodo tento, klikněte na tlačítko **nastavení** > **přihlašovací údaje nasazení** v okně hello webové aplikace.
   
    ![](./media/web-sites-php-sql-database-deploy-use-git/deployment-credentials.png)
10. V **nastavení** klikněte na **vlastnosti** toosee hello vzdálené adresy URL pro Git budete potřebovat toouse toodeploy aplikaci PHP později.

## <a name="get-sql-database-connection-information"></a>Získat informace o připojení k databázi SQL
instance databáze SQL toohello tooconnect, který je propojený tooyour webové aplikace, vaše bude potřebovat hello informace o připojení, který jste zadali při vytváření databáze hello. hello tooget informace o připojení databáze SQL, postupujte takto:

1. Zpět v okně skupiny prostředků hello klikněte na ikonu hello SQL database.
2. V okně databáze SQL hello, klikněte na tlačítko **nastavení** > **vlastnosti**, pak klikněte na tlačítko **zobrazit databázové připojovací řetězce**. 
   
    ![Zobrazit vlastnosti databáze](./media/web-sites-php-sql-database-deploy-use-git/view-database-properties.png)
3. Z hello **PHP** části hello výsledné dialogového okna, poznamenejte si hodnoty hello `Server`, `SQL Database`, a `User Name`. Budete používat tyto hodnoty později při publikování vašeho PHP webové aplikace tooAzure služby App Service.

## <a name="build-and-test-your-application-locally"></a>Místní sestavení a testování aplikace
Registrace aplikace Hello je jednoduchou aplikaci PHP, která vám umožní tooregister pro událost tím, že poskytuje vaše jméno a e-mailovou adresu. Zobrazí se informace o předchozích rejstříkem v tabulce. Registrační informace jsou uloženy v instanci databáze SQL. Hello aplikace se skládá ze dvou souborů (zkopírujte a vložte kód je k dispozici níže):

* **index.php**: Zobrazí formulář pro registraci a tabulku obsahující informace osob žádajících o registraci.
* **CreateTable.php**: vytvoří hello tabulka databáze SQL pro aplikaci hello. Tento soubor se dají použít jenom jednou.

toorun hello aplikaci místně, postupujte podle následujících kroků hello. Všimněte si, že tento postup předpokládá, máte PHP a SQL Server Express nastavit na místním počítači, a že je povoleno hello [PDO rozšíření pro SQL Server][pdo-sqlsrv].

1. Vytvořit databázi systému SQL Server volá `registration`. Můžete to provést z hello `sqlcmd` příkazového řádku pomocí těchto příkazů:
   
        >sqlcmd -S localhost\sqlexpress -U <local user name> -P <local password>
        1> create database registration
        2> GO    
2. V kořenovém adresáři aplikace, vytvořte dva soubory v ní – jednu s názvem `createtable.php` a jednu s názvem `index.php`.
3. Otevřete hello `createtable.php` soubor v textovém editoru nebo IDE a přidejte následující kód hello. Tento kód bude hello použité toocreate `registration_tbl` tabulky v hello `registration` databáze.
   
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
   
    Všimněte si, že budete potřebovat tooupdate hello hodnoty pro <code>$user</code> a <code>$pwd</code> s vaší místní systém SQL Server uživatelské jméno a heslo.
4. V terminálu v kořenovém adresáři hello hello typu aplikace hello následující příkaz:
   
        php -S localhost:8000
5. Otevřete webový prohlížeč a vyhledejte příliš**http://localhost:8000/createtable.php**. Tím se vytvoří hello `registration_tbl` tabulky v databázi hello.
6. Otevřete hello **index.php** soubor v textovém editoru nebo IDE a přidejte hello kód základní HTML a CSS hello stránky (v následujících krocích se přidají hello kódu PHP.).
   
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
        <p>Fill in your name and email address, then click <strong>Submit</strong> tooregister.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php
   
        ?>
        </body>
        </html>
7. V rámci hello PHP značky přidejte kód PHP pro připojení toohello databáze.
   
        // DB connection info
        $host = "localhost\sqlexpress";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect toodatabase.
        try {
            $conn = new PDO( "sqlsrv:Server= $host ; Database = $db ", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }
   
    Znovu, budete potřebovat tooupdate hello hodnoty pro <code>$user</code> a <code>$pwd</code> s vaší místní MySQL uživatelské jméno a heslo.
8. Následující kód připojení databáze hello přidáte kód pro vložení registrační informace do databáze hello.
   
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
9. Nakonec následující kód hello výše, přidejte kód pro načítání dat z databáze hello.
   
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

Nyní můžete procházet příliš**http://localhost:8000/index.php** tootest hello aplikace.

## <a name="publish-your-application"></a>Publikování aplikace
Po testování vaší aplikace místně, můžete ho publikovat tooApp Service Web Apps pomocí Git. Nicméně je nutné nejprve tooupdate informací o připojení databáze hello v aplikaci hello. Pomocí informací připojení databáze hello jste dříve získali (v hello **informace o připojení k databázi SQL získat** část), aktualizace hello následující informace v **i** hello `createdatabase.php` a `index.php` soubory s hello příslušné hodnoty:

    // DB connection info
    $host = "tcp:<value of Server>";
    $user = "<value of User Name>";
    $pwd = "<your password>";
    $db = "<value of SQL Database>";

> [!NOTE]
> V hello <code>$host</code>, musí být předponou hello hodnotu serveru <code>tcp:</code>.
> 
> 

Nyní jsou připravené tooset až publikování Git a publikování aplikace hello.

> [!NOTE]
> Jedná se o stejný postup si poznamenali na konci hello hello hello **vytvoření webové aplikace Azure a nastavení publikování Git** část výše.
> 
> 

1. Otevřete Git Bash (nebo terminál, pokud je Git v vaše `PATH`), změňte adresáře toohello kořenový adresář aplikace (hello **registrace** directory), a spusťte hello následující příkazy:
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    Jste vyzváni k hello heslo, které jste vytvořili dříve.
2. Procházet příliš**http://[web aplikace name].azurewebsites.net/createtable.php** toocreate hello SQL databázové tabulky pro aplikace hello.
3. Procházet příliš**http://[web aplikace name].azurewebsites.net/index.php** toobegin pomocí aplikace hello.

Po publikování aplikace, můžete začít vytvářet tooit změny a použít Git toopublish je. 

## <a name="publish-changes-tooyour-application"></a>Publikování aplikace tooyour změny
toopublish změny tooapplication, postupujte takto:

1. Proveďte změny tooyour aplikace místně.
2. Otevřete Git Bash (nebo terminál, it Git je ve vaší `PATH`), změňte adresáře toohello kořenový adresář aplikace a spusťte následující příkazy hello:
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    Jste vyzváni k hello heslo, které jste vytvořili dříve.
3. Procházet příliš**http://[web aplikace name].azurewebsites.net/index.php** toosee změny.

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[pdo-sqlsrv]: http://php.net/pdo_sqlsrv

