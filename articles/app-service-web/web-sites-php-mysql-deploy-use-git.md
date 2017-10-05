---
title: "Vytvoření webové aplikace v PHP-MySQL ve službě Azure App Service a nasazení přes Git"
description: "Kurz ukazuje, jak vytvořit webovou aplikaci PHP, která ukládá data v MySQL a používat Git nasazení do Azure."
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
ms.openlocfilehash: aa845eb474dbd42ae2c31880690d4ced059eb448
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>Vytvoření webové aplikace v PHP-MySQL ve službě Azure App Service a nasazení přes Git
Tento kurz ukazuje, jak k vytvoření webové aplikace PHP MySQL a jak ho nasadit do [služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) pomocí Git. Budete používat [PHP][install-php], nástroje příkazového řádku MySQL (součástí [MySQL][install-mysql]), a [Git] [ install-git] v počítači nainstalována. Pokyny v tomto kurzu platí pro všechny operační systémy, včetně systému Windows, Mac a Linux. Po dokončení tohoto průvodce, budete mít webovou aplikaci PHP nebo MySQL běžící v Azure.

Co se dozvíte:

* Jak vytvořit webovou aplikaci a pomocí databáze MySQL [portálu Azure][management-portal]. Protože je v PHP [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) ve výchozím nastavení, nic speciální se vyžaduje ke spuštění kódu PHP.
* Postup publikování a znovu publikovat svoji aplikaci do Azure pomocí Git.
* Postup povolení rozšíření autora autora automatizovat úlohy v každé `git push`.

Podle tohoto kurzu vytvoříte registrace jednoduché webové aplikace v jazyce PHP. Aplikace hostované ve webové aplikace. Zde je snímek obrazovky dokončené aplikace:

![Azure webu PHP][running-app]

## <a name="set-up-the-development-environment"></a>Nastavení vývojového prostředí
Tento kurz předpokládá, že máte [PHP][install-php], nástroje příkazového řádku MySQL (součástí [MySQL][install-mysql]), a [Git] [ install-git] v počítači nainstalována.

<a id="create-web-site-and-set-up-git"></a>

## <a name="create-a-web-app-and-set-up-git-publishing"></a>Vytvoření webové aplikace a nastavení publikování Git
Postupujte podle těchto kroků můžete vytvořit webovou aplikaci a databáze MySQL:

1. Přihlášení k [portál Azure][management-portal].
2. Klikněte **nový** ikonu.
3. Klikněte na tlačítko **najdete v článku všechny** vedle **Marketplace**. 
4. Klikněte na tlačítko **Web + mobilní**, pak **Web app + MySQL**. Poté klikněte na možnost **Vytvořit**.
5. Zadejte platný název skupiny prostředků.
   
    ![Název skupiny prostředků sady][resource-group]
6. Zadejte hodnoty pro novou webovou aplikaci.
   
    ![Vytvoření webové aplikace][new-web-app]
7. Zadejte hodnoty pro novou databázi, včetně, kterým Odsouhlasíte právní podmínky.
   
    ![Vytvořit novou databázi MySQL][new-mysql-db]
8. Po vytvoření webové aplikace, zobrazí se nové okno webové aplikace.
9. V **nastavení** klikněte na **průběžné nasazování**, potom klikněte na *konfigurovat požadované nastavení*.
   
    ![Nastavit publikování Git][setup-publishing]
10. Vyberte **místní úložiště Git** zdroje.
    
     ![Nastavení úložiště Git][setup-repository]
11. Povolit publikování Git, je nutné zadat uživatelské jméno a heslo. Poznamenejte si uživatelské jméno a heslo, které vytvoříte. (Pokud jste nastavili úložiště Git před, bude přeskočen tento krok.)
    
     ![Vytvořit přihlašovací údaje pro publikování][credentials]

## <a name="get-remote-mysql-connection-information"></a>Získat informace o připojení k vzdálené MySQL
Pro připojení k databázi MySQL, která běží ve službě Web Apps, vaše bude potřebovat informace o připojení. Chcete-li získat informace o připojení databáze MySQL, postupujte takto:

1. Od vaší skupiny prostředků klikněte na databázi:
   
    ![Vyberte databázi][select-database]
2. Z databáze **nastavení**, vyberte **vlastnosti**.
   
    ![Výběr vlastností][select-properties]
3. Poznamenejte si hodnoty pro `Database`, `Host`, `User Id`, a `Password`.
   
    ![Poznámka: vlastnosti][note-properties]

## <a name="build-and-test-your-app-locally"></a>Vytvoření a testování vaší aplikace místně
Teď, když jste vytvořili webovou aplikaci, můžete vyvíjet aplikaci místně a pak ho nasadit po testování.

Registrace aplikace je jednoduchou aplikaci PHP, která umožňuje zaregistrovat pro událost tím, že poskytuje vaše jméno a e-mailovou adresu. Zobrazí se informace o předchozích rejstříkem v tabulce. Registrační informace jsou uloženy v databázi MySQL. Aplikace se skládá z jednoho souboru (zkopírujte a vložte kód je k dispozici níže):

* **index.php**: Zobrazí formulář pro registraci a tabulku obsahující informace osob žádajících o registraci.

Sestavení a spuštění aplikace místně, postupujte podle následujících kroků. Všimněte si, že tento postup předpokládá, máte PHP a databáze MySQL nástroj příkazového řádku (součást MySQL) nastavit na místním počítači a pokud jste povolili [PDO rozšíření pro databázi MySQL][pdo-mysql].

1. Připojit ke vzdálenému serveru MySQL, pomocí hodnoty pro `Data Source`, `User Id`, `Password`, a `Database` , který jste získali dříve:
   
        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]
2. MySQL příkazového řádku se zobrazí:
   
        mysql>
3. Vložte následující `CREATE TABLE` příkazu vytvořte `registration_tbl` tabulky v databázi:
   
        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);
4. V kořenové složce místní aplikace vytvořte **index.php** souboru.
5. Otevřete **index.php** soubor v textovém editoru nebo IDE a přidejte následující kód a provést potřebné změny, které jsou označené jako `//TODO:` komentáře.

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
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
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

1. V terminálu přejděte do složky aplikace a zadejte následující příkaz:
   
       php -S localhost:8000

Nyní můžete procházet k **http://localhost: 8000 /** k testování aplikace.

## <a name="publish-your-app"></a>Publikování aplikace
Po testování vaší aplikace místně, můžete ji publikovat pomocí Git webové aplikace. Bude inicializovat místní úložiště Git a publikování aplikace.

> [!NOTE]
> Toto jsou stejné kroky uvedené na portálu Azure na konci vytvořit webovou aplikaci a sadu až publikování Git část výše.
> 
> 

1. (Volitelné)  Pokud jste zapomněli, nebo k jejich chybnému umístění URL vzdálené repostitory Git, přejděte do vlastností webové aplikace na portálu Azure.
2. Otevřete Git Bash (nebo terminál, pokud Git je ve vaší `PATH`), změňte adresáře na kořenový adresář aplikace a spusťte následující příkazy:
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    Zobrazí se výzva k zadání hesla, které jste vytvořili dříve.
   
    ![Počáteční nabízené Azure prostřednictvím Git][git-initial-push]
3. Přejděte do **http://[site name].azurewebsites.net/index.php** začít používat aplikace (tyto informace se uloží na řídicím panelu účet):
   
    ![Azure webu PHP][running-app]

Po publikování aplikace, můžete začít, provedení změn a publikovat je pomocí Git.

## <a name="publish-changes-to-your-app"></a>Publikování změn do vaší aplikace
Při publikování změn do vaší aplikace, postupujte takto:

1. Proveďte změny aplikace místně.
2. Otevřete Git Bash (nebo terminál, it Git je ve vaší `PATH`), změňte adresáře na kořenový adresář aplikace a spusťte následující příkazy:
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    Zobrazí se výzva k zadání hesla, které jste vytvořili dříve.
   
    ![Vkládání změn lokalit k Azure přes Git][git-change-push]
3. Přejděte do **http://[site name].azurewebsites.net/index.php** vaší aplikace a všechny změny provedené:
   
    ![Azure webu PHP][running-app]

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

<a name="composer"></a>

## <a name="enable-composer-automation-with-the-composer-extension"></a>Povolit automatickou autora s příponou autora
Ve výchozím nastavení proces nasazení git ve službě App Service nic se neděje s composer.json, pokud máte ve vašem projektu PHP. Můžete povolit composer.json zpracování během `git push` povolením rozšíření autora.

1. Ve vašem PHP webové okně aplikace v [portál Azure][management-portal], klikněte na tlačítko **nástroje** > **rozšíření**.
   
    ![Nastavení rozšíření autora][composer-extension-settings]
2. Klikněte na tlačítko **přidat**, pak klikněte na tlačítko **autora**.
   
    ![Přidání rozšíření autora][composer-extension-add]
3. Klikněte na tlačítko **OK** přijměte právní podmínky. Klikněte na tlačítko **OK** přidat rozšíření.
   
    **Nainstalovaná rozšíření** okno se nyní zobrazí rozšíření autora.  
    ![Zobrazení rozšíření autora][composer-extension-view]
4. Teď, provádět `git add`, `git commit`, a `git push` jako v předchozí části. Nyní se zobrazí, autora je instalování závislostí, které jsou definované v composer.json.
   
    ![Úspěch rozšíření autora][composer-extension-success]

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu [středisku pro vývojáře PHP](/develop/php/).

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
