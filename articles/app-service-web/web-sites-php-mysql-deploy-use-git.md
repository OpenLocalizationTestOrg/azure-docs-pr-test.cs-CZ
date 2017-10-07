---
title: "aaaCreate PHP-MySQL, webová aplikace ve službě Azure App Service a nasazení pomocí Git"
description: "Kurz, který ukazuje, jak webová aplikace, která ukládá data v MySQL toocreate PHP a použít tooAzure nasazení Git."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
tags: mysql
ms.assetid: 7454475f-e275-4bc7-9f09-1ef07382e5da
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 9c22946777598cc973cd9dfc8d2a258bd08cc39a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>Vytvoření webové aplikace v PHP-MySQL ve službě Azure App Service a nasazení přes Git
Tento kurz ukazuje, jak toocreate PHP-MySQL, webová aplikace a jak toodeploy je příliš[služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) pomocí Git. Budete používat [PHP][install-php], hello MySQL nástroj příkazového řádku (součástí [MySQL][install-mysql]), a [Git] [ install-git] v počítači nainstalována. Hello pokyny v tomto kurzu platí pro všechny operační systémy, včetně systému Windows, Mac a Linux. Po dokončení tohoto průvodce, budete mít webovou aplikaci PHP nebo MySQL běžící v Azure.

Co se dozvíte:

* Jak se toocreate webovou aplikaci a MySQL databáze, pomocí hello [portálu Azure][management-portal]. Protože je v PHP [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) ve výchozím nastavení, nic speciální je požadovaná toorun kódu PHP.
* Jak toopublish a znovu publikovat tooAzure vaší aplikace pomocí Git.
* Jak tooenable hello autora rozšíření tooautomate autora úloh v každé `git push`.

Podle tohoto kurzu vytvoříte registrace jednoduché webové aplikace v jazyce PHP. Hello aplikace hostované ve webové aplikace. Zde je snímek obrazovky aplikace hello dokončena:

![Azure webu PHP][running-app]

## <a name="set-up-hello-development-environment"></a>Nastavit hello vývojového prostředí
Tento kurz předpokládá, že máte [PHP][install-php], hello MySQL nástroj příkazového řádku (součástí [MySQL][install-mysql]), a [Git] [ install-git] v počítači nainstalována.

<a id="create-web-site-and-set-up-git"></a>

## <a name="create-a-web-app-and-set-up-git-publishing"></a>Vytvoření webové aplikace a nastavení publikování Git
Webové aplikace a databáze MySQL, postupujte podle těchto kroků toocreate:

1. Přihlášení toohello [portálu Azure][management-portal].
2. Klikněte na tlačítko hello **nový** ikonu.
3. Klikněte na tlačítko **najdete v článku všechny** další příliš**Marketplace**. 
4. Klikněte na tlačítko **Web + mobilní**, pak **Web app + MySQL**. Poté klikněte na možnost **Vytvořit**.
5. Zadejte platný název skupiny prostředků.
   
    ![Název skupiny prostředků sady][resource-group]
6. Zadejte hodnoty pro novou webovou aplikaci.
   
    ![Vytvoření webové aplikace][new-web-app]
7. Zadejte hodnoty pro novou databázi, včetně, kterým Odsouhlasíte toohello právní podmínky.
   
    ![Vytvořit novou databázi MySQL][new-mysql-db]
8. Po vytvoření hello webové aplikace, zobrazí se okno hello nové webové aplikace.
9. V **nastavení** klikněte na **průběžné nasazování**, potom klikněte na *konfigurovat požadované nastavení*.
   
    ![Nastavit publikování Git][setup-publishing]
10. Vyberte **místní úložiště Git** zdroje hello.
    
     ![Nastavení úložiště Git][setup-repository]
11. tooenable publikování Git, je nutné zadat uživatelské jméno a heslo. Poznamenejte si hello uživatelské jméno a heslo, které vytvoříte. (Pokud jste nastavili úložiště Git před, bude přeskočen tento krok.)
    
     ![Vytvořit přihlašovací údaje pro publikování][credentials]

## <a name="get-remote-mysql-connection-information"></a>Získat informace o připojení k vzdálené MySQL
databáze MySQL toohello tooconnect, které běží ve službě Web Apps, vaše bude potřebovat hello informace o připojení. tooget informace o připojení databáze MySQL, postupujte takto:

1. Od vaší skupiny prostředků klikněte na databázi hello:
   
    ![Vyberte databázi][select-database]
2. Z databáze hello **nastavení**, vyberte **vlastnosti**.
   
    ![Výběr vlastností][select-properties]
3. Poznamenejte si hodnoty hello `Database`, `Host`, `User Id`, a `Password`.
   
    ![Poznámka: vlastnosti][note-properties]

## <a name="build-and-test-your-app-locally"></a>Vytvoření a testování vaší aplikace místně
Teď, když jste vytvořili webovou aplikaci, můžete vyvíjet aplikaci místně a pak ho nasadit po testování.

Registrace aplikace Hello je jednoduchou aplikaci PHP, která vám umožní tooregister pro událost tím, že poskytuje vaše jméno a e-mailovou adresu. Zobrazí se informace o předchozích rejstříkem v tabulce. Registrační informace jsou uloženy v databázi MySQL. Hello aplikace se skládá z jednoho souboru (zkopírujte a vložte kód je k dispozici níže):

* **index.php**: Zobrazí formulář pro registraci a tabulku obsahující informace osob žádajících o registraci.

toobuild a spuštění hello aplikaci místně, postupujte podle následujících kroků hello. Všimněte si, že tento postup předpokládá, máte hello PHP a databáze MySQL nástroj příkazového řádku (součást MySQL) nastavit na místním počítači, a že je povoleno hello [PDO rozšíření pro databázi MySQL][pdo-mysql].

1. Připojit toohello vzdálené MySQL serveru, použijte hodnotu hello `Data Source`, `User Id`, `Password`, a `Database` , který jste získali dříve:
   
        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]
2. Zobrazí se Hello MySQL příkazového řádku:
   
        mysql>
3. Vložte následující hello `CREATE TABLE` příkaz toocreate hello `registration_tbl` tabulky v databázi:
   
        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);
4. V kořenové složce místní aplikace hello vytvořte **index.php** souboru.
5. Otevřete hello **index.php** soubor v textovém editoru nebo IDE a přidejte hello následující kód a dokončení hello potřebné změny označené jako `//TODO:` komentáře.

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
            // DB connection info
            //TODO: Update hello values for $host, $user, $pwd, and $db
            //using hello values you retrieved earlier from hello Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect toodatabase.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
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
            // Retrieve data
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
        ?>
        </body>
        </html>

1. V terminálu přejděte tooyour aplikace složku a typ hello následující příkaz:
   
       php -S localhost:8000

Nyní můžete procházet příliš**http://localhost: 8000 /** tootest hello aplikace.

## <a name="publish-your-app"></a>Publikování aplikace
Po testování vaší aplikace místně, můžete ho publikovat tooWeb aplikací pomocí Git. Bude inicializovat místní úložiště Git a publikování aplikace hello.

> [!NOTE]
> Tyto jsou hello stejné kroky uvedené v portálu Azure na konci hello hello vytvořit webovou aplikaci a sadu hello až publikování Git část výše.
> 
> 

1. (Volitelné)  Pokud jste zapomněli, nebo k jejich chybnému umístění URL vzdálené repostitory Git, přejděte toohello webové aplikace vlastnosti hello portálu Azure.
2. Otevřete Git Bash (nebo terminál, pokud Git je ve vaší `PATH`), změňte adresáře toohello kořenový adresář aplikace a spusťte následující příkazy hello:
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    Jste vyzváni k hello heslo, které jste vytvořili dříve.
   
    ![Počáteční tooAzure nabízené prostřednictvím Git][git-initial-push]
3. Procházet příliš**http://[site name].azurewebsites.net/index.php** toobegin pomocí aplikace hello (tyto informace se uloží na řídicím panelu účet):
   
    ![Azure webu PHP][running-app]

Po publikování aplikace, můžete začít vytvářet tooit změny a použít Git toopublish je.

## <a name="publish-changes-tooyour-app"></a>Publikování aplikace tooyour změny
toopublish změny tooyour aplikace, postupujte takto:

1. Zkontrolujte změny tooyour aplikace místně.
2. Otevřete Git Bash (nebo terminál, it Git je ve vaší `PATH`), změňte adresáře toohello kořenový adresář aplikace a spusťte následující příkazy hello:
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    Jste vyzváni k hello heslo, které jste vytvořili dříve.
   
    ![Vkládání tooAzure změny webu prostřednictvím Git][git-change-push]
3. Procházet příliš**http://[site name].azurewebsites.net/index.php** toosee vaší aplikace a veškeré změny provedené:
   
    ![Azure webu PHP][running-app]

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

<a name="composer"></a>

## <a name="enable-composer-automation-with-hello-composer-extension"></a>Povolit automatickou autora s hello rozšíření autora
Ve výchozím nastavení proces nasazení git hello ve službě App Service nic se neděje s composer.json, pokud máte ve vašem projektu PHP. Můžete povolit composer.json zpracování během `git push` povolením rozšíření autora hello.

1. Ve vašem PHP webové okně aplikace v hello [portál Azure][management-portal], klikněte na tlačítko **nástroje** > **rozšíření**.
   
    ![Nastavení rozšíření autora][composer-extension-settings]
2. Klikněte na tlačítko **přidat**, pak klikněte na tlačítko **autora**.
   
    ![Přidání rozšíření autora][composer-extension-add]
3. Klikněte na tlačítko **OK** tooaccept právní podmínky. Klikněte na tlačítko **OK** znovu tooadd hello rozšíření.
   
    Hello **nainstalovaná rozšíření** okno se nyní zobrazí rozšíření autora hello.  
    ![Zobrazení rozšíření autora][composer-extension-view]
4. Teď, provádět `git add`, `git commit`, a `git push` jako v předchozím oddílu hello. Nyní se zobrazí, autora je instalování závislostí, které jsou definované v composer.json.
   
    ![Úspěch rozšíření autora][composer-extension-success]

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello [středisku pro vývojáře PHP](/develop/php/).

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
