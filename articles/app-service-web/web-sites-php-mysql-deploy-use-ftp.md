---
title: "aaaCreate PHP-MySQL, webová aplikace ve službě Azure App Service a nasazení pomocí protokolu FTP"
description: "Kurz, který ukazuje, jak toocreate PHP webová aplikace, která ukládá data v MySQL a používání tooAzure nasazení FTP."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 6d9d1de5-5868-48fd-8bad-decb4979cd65
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 4d3b56a8ac63d0eba0dc0aec1b62e6d12f601bf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>Vytvoření webové aplikace v PHP-MySQL ve službě Azure App Service a nasazení přes FTP
Tento kurz ukazuje, jak toocreate PHP-MySQL, webová aplikace a jak toodeploy pomocí služby FTP. Tento kurz předpokládá, že máte [PHP][install-php], [MySQL][install-mysql], webového serveru a klienta v počítači nainstalována. Hello pokyny v tomto kurzu platí pro všechny operační systémy, včetně systému Windows, Mac a Linux. Po dokončení tohoto průvodce, budete mít webovou aplikaci PHP nebo MySQL běžící v Azure.

Co se dozvíte:

* Jak hello toocreate webovou aplikaci a MySQL databáze pomocí portálu Azure. Vzhledem k tomu, že jazyk PHP je ve výchozím nastavení povolena ve službě Web Apps, nic speciální je požadovaná toorun kódu PHP.
* Jak toopublish tooAzure aplikace pomocí serveru FTP.

Podle tohoto kurzu vytvoříte registrace jednoduché webové aplikace v jazyce PHP. aplikace Hello bude hostován ve webové aplikaci. Zde je snímek obrazovky aplikace hello dokončena:

![Webové stránky Azure PHP][running-app]

> [!NOTE]
> Pokud chcete začít s Azure App Service před registrací účtu tooget, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek. 
> 
> 

## <a name="create-a-web-app-and-set-up-ftp-publishing"></a>Vytvoření webové aplikace a nastavení publikování FTP
Webové aplikace a databáze MySQL, postupujte podle těchto kroků toocreate:

1. Přihlášení toohello [portálu Azure][management-portal].
2. Klikněte na tlačítko hello **+ nový** ikonu na hello horní pravé hello portálu Azure.
   
    ![Vytvořit nový web Azure][new-website]
3. V typu vyhledávání hello **Web app + MySQL** a klikněte na **Web app + MySQL**.
   
    ![Vlastní vytvořit novou webovou stránku][custom-create]
4. Klikněte na možnost **Vytvořit**. Zadejte název jedinečné app service, platný název pro skupinu prostředků hello a nový plán služby.
   
    ![Název skupiny prostředků sady][resource-group]
5. Zadejte hodnoty pro novou databázi, včetně, kterým Odsouhlasíte toohello právní podmínky.
   
    ![Vytvořit novou databázi MySQL][new-mysql-db]
6. Po vytvoření webové aplikace hello, zobrazí se okno hello nové aplikace služby.
7. Klikněte na **nastavení** > **přihlašovací údaje nasazení**. 
   
    ![Nastavení přihlašovacích údajů nasazení][set-deployment-credentials]
8. tooenable publikování FTP, je nutné zadat uživatelské jméno a heslo. Uložit pověření hello a poznamenejte si hello uživatelské jméno a heslo, které vytvoříte.
   
    ![Vytvořit přihlašovací údaje pro publikování][portal-ftp-username-password]

## <a name="build-and-test-your-app-locally"></a>Vytvoření a testování vaší aplikace místně
Registrace aplikace Hello je jednoduchou aplikaci PHP, která vám umožní tooregister pro událost tím, že poskytuje vaše jméno a e-mailovou adresu. Zobrazí se informace o předchozích rejstříkem v tabulce. Registrační informace jsou uloženy v databázi MySQL. Hello aplikace se skládá z dva soubory:

* **index.php**: Zobrazí formulář pro registraci a tabulku obsahující informace osob žádajících o registraci.
* **CreateTable.php**: vytvoří tabulku hello MySQL pro aplikace hello. Tento soubor se dají použít jenom jednou.

toobuild a spuštění hello aplikaci místně, postupujte podle následujících kroků hello. Všimněte si, že tento postup předpokládá, že máte PHP, MySQL a webový server nastavit na místním počítači, a že je povoleno hello [PDO rozšíření pro databázi MySQL][pdo-mysql].

1. Vytvoření databáze MySQL názvem `registration`. Můžete to udělat z hello MySQL příkazového řádku pomocí tohoto příkazu:
   
        mysql> create database registration;
2. V kořenovém adresáři váš webový server, vytvořte složku s názvem `registration` a vytvořit dva soubory v ní – jednu s názvem `createtable.php` a jednu s názvem `index.php`.
3. Otevřete hello `createtable.php` soubor v textovém editoru nebo IDE a přidejte následující kód hello. Tento kód bude hello použité toocreate `registration_tbl` tabulky v hello `registration` databáze.
   
        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
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
   
   > [!NOTE]
   > Budete potřebovat tooupdate hello hodnoty pro <code>$user</code> a <code>$pwd</code> s vaší místní MySQL uživatelské jméno a heslo.
   > 
   > 
4. Otevřete webový prohlížeč a vyhledejte příliš[http://localhost/registration/createtable.php][localhost-createtable]. Tím se vytvoří hello `registration_tbl` tabulky v databázi hello.
5. Otevřete hello **index.php** soubor v textovém editoru nebo IDE a přidejte hello kód základní HTML a CSS hello stránky (v následujících krocích se přidají hello kódu PHP.).
   
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
6. V rámci hello PHP značky přidejte kód PHP pro připojení toohello databáze.
   
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect toodatabase.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }
   
   > [!NOTE]
   > Znovu, budete potřebovat tooupdate hello hodnoty pro <code>$user</code> a <code>$pwd</code> s vaší místní MySQL uživatelské jméno a heslo.
   > 
   > 
7. Následující kód připojení databáze hello přidáte kód pro vložení registrační informace do databáze hello.
   
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
8. Nakonec následující kód hello výše, přidejte kód pro načítání dat z databáze hello.
   
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

Nyní můžete procházet příliš[http://localhost/registration/index.php] [ localhost-index] tootest hello aplikace.

## <a name="get-mysql-and-ftp-connection-information"></a>Získat informace o připojení databáze MySQL a FTP
databáze MySQL toohello tooconnect, které běží ve službě Web Apps, vaše bude potřebovat hello informace o připojení. tooget informace o připojení databáze MySQL, postupujte takto:

1. Ze služby aplikace hello okně webové aplikace klikněte na odkaz skupinu prostředků hello:
   
    ![Vyberte skupinu prostředků][select-resourcegroup]
2. Od vaší skupiny prostředků klikněte na databázi hello:
   
    ![Vyberte databázi][select-database]
3. Z databáze hello souhrn, vyberte **nastavení** > **vlastnosti**.
   
    ![Výběr vlastností][select-properties]
4. Poznamenejte si hodnoty hello `Database`, `Host`, `User Id`, a `Password`.
   
    ![Poznámka: vlastnosti][note-properties]
5. Z vaší webové aplikace, klikněte na tlačítko hello **stažení profilu publikování** odkaz na hello pravém dolním rohu stránky hello:
   
    ![Stažení profilu publikování][download-publish-profile]
6. Otevřete hello `.publishsettings` souboru v editoru XML. 
7. Najde hello `<publishProfile >` element s `publishMethod="FTP"` toto vypadá podobně jako toothis:
   
        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>

Poznamenejte si hello `publishUrl`, `userName`, a `userPWD` atributy.

## <a name="publish-your-app"></a>Publikování aplikace
Po testování vaší aplikace místně, můžete ho publikovat tooyour webové aplikace pomocí protokolu FTP. Nicméně je nutné nejprve tooupdate informací o připojení databáze hello v aplikaci hello. Pomocí informací připojení databáze hello jste dříve získali (v hello **získat MySQL a FTP informace o připojení** část), aktualizace hello následující informace v **i** hello `createdatabase.php` a `index.php` soubory s hello příslušné hodnoty:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

Nyní jste připraveni toopublish vaší aplikace pomocí protokolu FTP.

1. Otevřete váš klient FTP podle volby.
2. Zadejte hello *část názvu hostitele* z hello `publishUrl` atribut uvedených výše do vašeho klienta FTP.
3. Zadejte hello `userName` a `userPWD` do vašeho klienta FTP beze změny atributů uvedených výše.
4. Navažte připojení.

Po připojení je bude možné tooupload a v případě potřeby stáhnout soubory. Ujistěte se, že nahráváte soubory toohello kořenový adresář, který je `/site/wwwroot`.

Po nahrání obě `index.php` a `createtable.php`, procházet příliš**http://[site name].azurewebsites.net/createtable.php** toocreate hello MySQL tabulky pro hello aplikaci a potom vyhledejte příliš**http://[site název ].azurewebsites.net/index.php** toobegin pomocí aplikace hello.

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello [středisku pro vývojáře PHP](/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[install-mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[localhost-createtable]: http://localhost/tasklist/createtable.php
[localhost-index]: http://localhost/tasklist/index.php
[running-app]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[website-details]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[set-deployment-credentials]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[portal-ftp-username-password]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[select-resourcegroup]: ./media/web-sites-php-mysql-deploy-use-ftp/select_resourcegroup.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[connection-string-info]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[management-portal]: https://portal.azure.com
[download-publish-profile]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png

