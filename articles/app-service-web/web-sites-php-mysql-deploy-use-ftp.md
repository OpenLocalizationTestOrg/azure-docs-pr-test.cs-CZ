---
title: "Vytvoření webové aplikace v PHP-MySQL ve službě Azure App Service a nasazení přes FTP"
description: "Kurz ukazuje, jak vytvořit webovou aplikaci PHP, která ukládá data v MySQL a používání FTP nasazení do Azure."
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
ms.openlocfilehash: d428dffc6b810a692be0ec39a5f9cca05f5439e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>Vytvoření webové aplikace v PHP-MySQL ve službě Azure App Service a nasazení přes FTP
Tento kurz ukazuje, jak vytvořit webovou aplikaci PHP MySQL a jak se dá nasadit pomocí protokolu FTP. Tento kurz předpokládá, že máte [PHP][install-php], [MySQL][install-mysql], webového serveru a klienta v počítači nainstalována. Pokyny v tomto kurzu platí pro všechny operační systémy, včetně systému Windows, Mac a Linux. Po dokončení tohoto průvodce, budete mít webovou aplikaci PHP nebo MySQL běžící v Azure.

Co se dozvíte:

* Jak vytvořit webovou aplikaci a databázi MySQL pomocí portálu Azure. Vzhledem k PHP je ve výchozím nastavení povolena ve službě Web Apps, nic speciální je vyžadovaná pro spouštění vašeho kódu PHP.
* Postup publikování aplikace do Azure pomocí protokolu FTP.

Podle tohoto kurzu vytvoříte registrace jednoduché webové aplikace v jazyce PHP. Aplikace bude hostován ve webové aplikaci. Zde je snímek obrazovky dokončené aplikace:

![Webové stránky Azure PHP][running-app]

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací k účtu, přejděte na [vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek. 
> 
> 

## <a name="create-a-web-app-and-set-up-ftp-publishing"></a>Vytvoření webové aplikace a nastavení publikování FTP
Postupujte podle těchto kroků můžete vytvořit webovou aplikaci a databáze MySQL:

1. Přihlášení k [portál Azure][management-portal].
2. Klikněte **+ nový** ikona nahoře vlevo na portálu Azure.
   
    ![Vytvořit nový web Azure][new-website]
3. V typu vyhledávání **Web app + MySQL** a klikněte na **Web app + MySQL**.
   
    ![Vlastní vytvořit novou webovou stránku][custom-create]
4. Klikněte na možnost **Vytvořit**. Zadejte název jedinečné app service, platný název pro skupinu prostředků a nový plán služby.
   
    ![Název skupiny prostředků sady][resource-group]
5. Zadejte hodnoty pro novou databázi, včetně, kterým Odsouhlasíte právní podmínky.
   
    ![Vytvořit novou databázi MySQL][new-mysql-db]
6. Po vytvoření webové aplikace, zobrazí se nové okno aplikace služby.
7. Klikněte na **nastavení** > **přihlašovací údaje nasazení**. 
   
    ![Nastavení přihlašovacích údajů nasazení][set-deployment-credentials]
8. Pokud chcete povolit publikování FTP, zadejte uživatelské jméno a heslo. Uložení přihlašovacích údajů a poznamenejte si uživatelské jméno a heslo, které vytvoříte.
   
    ![Vytvořit přihlašovací údaje pro publikování][portal-ftp-username-password]

## <a name="build-and-test-your-app-locally"></a>Vytvoření a testování vaší aplikace místně
Registrace aplikace je jednoduchou aplikaci PHP, která umožňuje zaregistrovat pro událost tím, že poskytuje vaše jméno a e-mailovou adresu. Zobrazí se informace o předchozích rejstříkem v tabulce. Registrační informace jsou uloženy v databázi MySQL. Aplikace se skládá z dva soubory:

* **index.php**: Zobrazí formulář pro registraci a tabulku obsahující informace osob žádajících o registraci.
* **CreateTable.php**: vytvoří tabulku MySQL pro aplikaci. Tento soubor se dají použít jenom jednou.

Sestavení a spuštění aplikace místně, postupujte podle následujících kroků. Všimněte si, že tento postup předpokládá, máte PHP, MySQL a webový server nastavit na místním počítači a pokud jste povolili [PDO rozšíření pro databázi MySQL][pdo-mysql].

1. Vytvoření databáze MySQL názvem `registration`. Můžete to udělat z příkazového řádku MySQL pomocí tohoto příkazu:
   
        mysql> create database registration;
2. V kořenovém adresáři váš webový server, vytvořte složku s názvem `registration` a vytvořit dva soubory v ní – jednu s názvem `createtable.php` a jednu s názvem `index.php`.
3. Otevřete `createtable.php` soubor v textovém editoru nebo IDE a přidejte následující kód. Tento kód se použije k vytvoření `registration_tbl` tabulky v `registration` databáze.
   
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
   > Budete muset aktualizovat hodnoty <code>$user</code> a <code>$pwd</code> s vaší místní MySQL uživatelské jméno a heslo.
   > 
   > 
4. Otevřete webový prohlížeč a přejděte do [http://localhost/registration/createtable.php][localhost-createtable]. Tím se vytvoří `registration_tbl` tabulky v databázi.
5. Otevřete **index.php** soubor v textovém editoru nebo IDE a přidat základní HTML a CSS kódu stránky (v následujících krocích se přidají kód PHP.).
   
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
6. V rámci značky PHP přidejte kód PHP pro připojení k databázi.
   
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }
   
   > [!NOTE]
   > Znovu, budete muset aktualizovat hodnoty <code>$user</code> a <code>$pwd</code> s vaší místní MySQL uživatelské jméno a heslo.
   > 
   > 
7. Následující kód připojení databáze přidáte kód pro vložení registrační informace do databáze.
   
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
8. Nakonec následující kód výše, přidejte kód pro načítání dat z databáze.
   
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

Nyní můžete procházet k [http://localhost/registration/index.php] [ localhost-index] k testování aplikace.

## <a name="get-mysql-and-ftp-connection-information"></a>Získat informace o připojení databáze MySQL a FTP
Pro připojení k databázi MySQL, která běží ve službě Web Apps, vaše bude potřebovat informace o připojení. Chcete-li získat informace o připojení databáze MySQL, postupujte takto:

1. Ze služby app service okně webové aplikace klikněte na odkaz skupiny prostředků:
   
    ![Vyberte skupinu prostředků][select-resourcegroup]
2. Od vaší skupiny prostředků klikněte na databázi:
   
    ![Vyberte databázi][select-database]
3. Z databáze souhrn, vyberte **nastavení** > **vlastnosti**.
   
    ![Výběr vlastností][select-properties]
4. Poznamenejte si hodnoty pro `Database`, `Host`, `User Id`, a `Password`.
   
    ![Poznámka: vlastnosti][note-properties]
5. Z vaší webové aplikace, klikněte **stažení profilu publikování** odkaz v pravém dolním rohu stránky:
   
    ![Stažení profilu publikování][download-publish-profile]
6. Otevřete `.publishsettings` souboru v editoru XML. 
7. Najít `<publishProfile >` element s `publishMethod="FTP"` který vypadá podobně jako tento:
   
        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>

Poznamenejte si `publishUrl`, `userName`, a `userPWD` atributy.

## <a name="publish-your-app"></a>Publikování aplikace
Po testování vaší aplikace místně, můžete ji publikujete do webové aplikace pomocí protokolu FTP. Však musíte nejprve aktualizovat informace o připojení databáze v aplikaci. Pomocí informací o připojení databáze jste dříve získali (v **získat MySQL a FTP informace o připojení** část), aktualizujte následující informace v **i** `createdatabase.php` a `index.php` soubory s příslušnými hodnotami:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

Nyní jste připraveni k publikování aplikace pomocí protokolu FTP.

1. Otevřete váš klient FTP podle volby.
2. Zadejte *část názvu hostitele* z `publishUrl` atribut uvedených výše do vašeho klienta FTP.
3. Zadejte `userName` a `userPWD` do vašeho klienta FTP beze změny atributů uvedených výše.
4. Navažte připojení.

Po připojení bude moci nahrávání a stahování souborů podle potřeby. Ujistěte se, že jsou nahrávání souborů do kořenového adresáře, který je `/site/wwwroot`.

Po nahrání obě `index.php` a `createtable.php`, přejděte do **http://[site name].azurewebsites.net/createtable.php** k vytvoření databáze MySQL tabulky pro aplikaci, pak přejděte do **http://[site name]. azurewebsites.NET/index.php** začít používat aplikaci.

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu [středisku pro vývojáře PHP](/develop/php/).

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

