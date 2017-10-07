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
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a><span data-ttu-id="d3bce-103">Vytvoření webové aplikace v PHP-MySQL ve službě Azure App Service a nasazení přes Git</span><span class="sxs-lookup"><span data-stu-id="d3bce-103">Create a PHP-MySQL web app in Azure App Service and deploy using Git</span></span>
<span data-ttu-id="d3bce-104">Tento kurz ukazuje, jak toocreate PHP-MySQL, webová aplikace a jak toodeploy je příliš[služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="d3bce-104">This tutorial shows you how toocreate a PHP-MySQL web app and how toodeploy it too[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) using Git.</span></span> <span data-ttu-id="d3bce-105">Budete používat [PHP][install-php], hello MySQL nástroj příkazového řádku (součástí [MySQL][install-mysql]), a [Git] [ install-git] v počítači nainstalována.</span><span class="sxs-lookup"><span data-stu-id="d3bce-105">You will use [PHP][install-php], hello MySQL Command-Line Tool (part of [MySQL][install-mysql]), and [Git][install-git] installed on your computer.</span></span> <span data-ttu-id="d3bce-106">Hello pokyny v tomto kurzu platí pro všechny operační systémy, včetně systému Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="d3bce-106">hello instructions in this tutorial can be followed on any operating system, including Windows, Mac, and  Linux.</span></span> <span data-ttu-id="d3bce-107">Po dokončení tohoto průvodce, budete mít webovou aplikaci PHP nebo MySQL běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="d3bce-107">Upon completing this guide, you will have a PHP/MySQL web app running in Azure.</span></span>

<span data-ttu-id="d3bce-108">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="d3bce-108">You will learn:</span></span>

* <span data-ttu-id="d3bce-109">Jak se toocreate webovou aplikaci a MySQL databáze, pomocí hello [portálu Azure][management-portal].</span><span class="sxs-lookup"><span data-stu-id="d3bce-109">How toocreate a web app and a MySQL database using hello [Azure Portal][management-portal].</span></span> <span data-ttu-id="d3bce-110">Protože je v PHP [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) ve výchozím nastavení, nic speciální je požadovaná toorun kódu PHP.</span><span class="sxs-lookup"><span data-stu-id="d3bce-110">Because PHP is enabled in [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) by default, nothing special is required toorun your PHP code.</span></span>
* <span data-ttu-id="d3bce-111">Jak toopublish a znovu publikovat tooAzure vaší aplikace pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="d3bce-111">How toopublish and re-publish your application tooAzure using Git.</span></span>
* <span data-ttu-id="d3bce-112">Jak tooenable hello autora rozšíření tooautomate autora úloh v každé `git push`.</span><span class="sxs-lookup"><span data-stu-id="d3bce-112">How tooenable hello Composer extension tooautomate Composer tasks at every `git push`.</span></span>

<span data-ttu-id="d3bce-113">Podle tohoto kurzu vytvoříte registrace jednoduché webové aplikace v jazyce PHP.</span><span class="sxs-lookup"><span data-stu-id="d3bce-113">By following this tutorial, you will build a simple registration web app in PHP.</span></span> <span data-ttu-id="d3bce-114">Hello aplikace hostované ve webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3bce-114">hello application will be hosted in Web Apps.</span></span> <span data-ttu-id="d3bce-115">Zde je snímek obrazovky aplikace hello dokončena:</span><span class="sxs-lookup"><span data-stu-id="d3bce-115">A screenshot of hello completed application is below:</span></span>

![Azure webu PHP][running-app]

## <a name="set-up-hello-development-environment"></a><span data-ttu-id="d3bce-117">Nastavit hello vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="d3bce-117">Set up hello development environment</span></span>
<span data-ttu-id="d3bce-118">Tento kurz předpokládá, že máte [PHP][install-php], hello MySQL nástroj příkazového řádku (součástí [MySQL][install-mysql]), a [Git] [ install-git] v počítači nainstalována.</span><span class="sxs-lookup"><span data-stu-id="d3bce-118">This tutorial assumes you have [PHP][install-php], hello MySQL Command-Line Tool (part of [MySQL][install-mysql]), and [Git][install-git] installed on your computer.</span></span>

<a id="create-web-site-and-set-up-git"></a>

## <a name="create-a-web-app-and-set-up-git-publishing"></a><span data-ttu-id="d3bce-119">Vytvoření webové aplikace a nastavení publikování Git</span><span class="sxs-lookup"><span data-stu-id="d3bce-119">Create a web app and set up Git publishing</span></span>
<span data-ttu-id="d3bce-120">Webové aplikace a databáze MySQL, postupujte podle těchto kroků toocreate:</span><span class="sxs-lookup"><span data-stu-id="d3bce-120">Follow these steps toocreate a web app and a MySQL database:</span></span>

1. <span data-ttu-id="d3bce-121">Přihlášení toohello [portálu Azure][management-portal].</span><span class="sxs-lookup"><span data-stu-id="d3bce-121">Login toohello [Azure Portal][management-portal].</span></span>
2. <span data-ttu-id="d3bce-122">Klikněte na tlačítko hello **nový** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d3bce-122">Click hello **New** icon.</span></span>
3. <span data-ttu-id="d3bce-123">Klikněte na tlačítko **najdete v článku všechny** další příliš**Marketplace**.</span><span class="sxs-lookup"><span data-stu-id="d3bce-123">Click **See All** next too**Marketplace**.</span></span> 
4. <span data-ttu-id="d3bce-124">Klikněte na tlačítko **Web + mobilní**, pak **Web app + MySQL**.</span><span class="sxs-lookup"><span data-stu-id="d3bce-124">Click **Web + Mobile**, then **Web app + MySQL**.</span></span> <span data-ttu-id="d3bce-125">Poté klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d3bce-125">Then, click **Create**.</span></span>
5. <span data-ttu-id="d3bce-126">Zadejte platný název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="d3bce-126">Enter a valid name for your resource group.</span></span>
   
    ![Název skupiny prostředků sady][resource-group]
6. <span data-ttu-id="d3bce-128">Zadejte hodnoty pro novou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d3bce-128">Enter values for your new web app.</span></span>
   
    ![Vytvoření webové aplikace][new-web-app]
7. <span data-ttu-id="d3bce-130">Zadejte hodnoty pro novou databázi, včetně, kterým Odsouhlasíte toohello právní podmínky.</span><span class="sxs-lookup"><span data-stu-id="d3bce-130">Enter values for your new database, including agreeing toohello legal terms.</span></span>
   
    ![Vytvořit novou databázi MySQL][new-mysql-db]
8. <span data-ttu-id="d3bce-132">Po vytvoření hello webové aplikace, zobrazí se okno hello nové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3bce-132">When hello web app has been created, you will see hello new web app blade.</span></span>
9. <span data-ttu-id="d3bce-133">V **nastavení** klikněte na **průběžné nasazování**, potom klikněte na *konfigurovat požadované nastavení*.</span><span class="sxs-lookup"><span data-stu-id="d3bce-133">In **Settings** click on **Continuous Deployment**, then click on *Configure required settings*.</span></span>
   
    ![Nastavit publikování Git][setup-publishing]
10. <span data-ttu-id="d3bce-135">Vyberte **místní úložiště Git** zdroje hello.</span><span class="sxs-lookup"><span data-stu-id="d3bce-135">Select **Local Git Repository** for hello source.</span></span>
    
     ![Nastavení úložiště Git][setup-repository]
11. <span data-ttu-id="d3bce-137">tooenable publikování Git, je nutné zadat uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="d3bce-137">tooenable Git publishing, you must provide a user name and password.</span></span> <span data-ttu-id="d3bce-138">Poznamenejte si hello uživatelské jméno a heslo, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="d3bce-138">Make a note of hello user name and password you create.</span></span> <span data-ttu-id="d3bce-139">(Pokud jste nastavili úložiště Git před, bude přeskočen tento krok.)</span><span class="sxs-lookup"><span data-stu-id="d3bce-139">(If you have set up a Git repository before, this step will be skipped.)</span></span>
    
     ![Vytvořit přihlašovací údaje pro publikování][credentials]

## <a name="get-remote-mysql-connection-information"></a><span data-ttu-id="d3bce-141">Získat informace o připojení k vzdálené MySQL</span><span class="sxs-lookup"><span data-stu-id="d3bce-141">Get remote MySQL connection information</span></span>
<span data-ttu-id="d3bce-142">databáze MySQL toohello tooconnect, které běží ve službě Web Apps, vaše bude potřebovat hello informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="d3bce-142">tooconnect toohello MySQL database that is running in Web Apps, your will need hello connection information.</span></span> <span data-ttu-id="d3bce-143">tooget informace o připojení databáze MySQL, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="d3bce-143">tooget MySQL connection information, follow these steps:</span></span>

1. <span data-ttu-id="d3bce-144">Od vaší skupiny prostředků klikněte na databázi hello:</span><span class="sxs-lookup"><span data-stu-id="d3bce-144">From your resource group, click hello database:</span></span>
   
    ![Vyberte databázi][select-database]
2. <span data-ttu-id="d3bce-146">Z databáze hello **nastavení**, vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="d3bce-146">From hello database **Settings**, select **Properties**.</span></span>
   
    ![Výběr vlastností][select-properties]
3. <span data-ttu-id="d3bce-148">Poznamenejte si hodnoty hello `Database`, `Host`, `User Id`, a `Password`.</span><span class="sxs-lookup"><span data-stu-id="d3bce-148">Make note of hello values for `Database`, `Host`, `User Id`, and `Password`.</span></span>
   
    ![Poznámka: vlastnosti][note-properties]

## <a name="build-and-test-your-app-locally"></a><span data-ttu-id="d3bce-150">Vytvoření a testování vaší aplikace místně</span><span class="sxs-lookup"><span data-stu-id="d3bce-150">Build and test your app locally</span></span>
<span data-ttu-id="d3bce-151">Teď, když jste vytvořili webovou aplikaci, můžete vyvíjet aplikaci místně a pak ho nasadit po testování.</span><span class="sxs-lookup"><span data-stu-id="d3bce-151">Now that you have created a web app, you can develop your application locally, then deploy it after testing.</span></span>

<span data-ttu-id="d3bce-152">Registrace aplikace Hello je jednoduchou aplikaci PHP, která vám umožní tooregister pro událost tím, že poskytuje vaše jméno a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="d3bce-152">hello Registration application is a simple PHP application that allows you tooregister for an event by providing your name and email address.</span></span> <span data-ttu-id="d3bce-153">Zobrazí se informace o předchozích rejstříkem v tabulce.</span><span class="sxs-lookup"><span data-stu-id="d3bce-153">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="d3bce-154">Registrační informace jsou uloženy v databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="d3bce-154">Registration information is stored in a MySQL database.</span></span> <span data-ttu-id="d3bce-155">Hello aplikace se skládá z jednoho souboru (zkopírujte a vložte kód je k dispozici níže):</span><span class="sxs-lookup"><span data-stu-id="d3bce-155">hello application consists of one file (copy/paste code available below):</span></span>

* <span data-ttu-id="d3bce-156">**index.php**: Zobrazí formulář pro registraci a tabulku obsahující informace osob žádajících o registraci.</span><span class="sxs-lookup"><span data-stu-id="d3bce-156">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>

<span data-ttu-id="d3bce-157">toobuild a spuštění hello aplikaci místně, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="d3bce-157">toobuild and run hello application locally, follow hello steps below.</span></span> <span data-ttu-id="d3bce-158">Všimněte si, že tento postup předpokládá, máte hello PHP a databáze MySQL nástroj příkazového řádku (součást MySQL) nastavit na místním počítači, a že je povoleno hello [PDO rozšíření pro databázi MySQL][pdo-mysql].</span><span class="sxs-lookup"><span data-stu-id="d3bce-158">Note that these steps assume you have hello PHP and MySQL Command-Line Tool (part of MySQL) set up on your local machine, and that you have enabled hello [PDO extension for MySQL][pdo-mysql].</span></span>

1. <span data-ttu-id="d3bce-159">Připojit toohello vzdálené MySQL serveru, použijte hodnotu hello `Data Source`, `User Id`, `Password`, a `Database` , který jste získali dříve:</span><span class="sxs-lookup"><span data-stu-id="d3bce-159">Connect toohello remote MySQL server, using hello value for `Data Source`, `User Id`, `Password`, and `Database` that you retrieved earlier:</span></span>
   
        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]
2. <span data-ttu-id="d3bce-160">Zobrazí se Hello MySQL příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="d3bce-160">hello MySQL command prompt will appear:</span></span>
   
        mysql>
3. <span data-ttu-id="d3bce-161">Vložte následující hello `CREATE TABLE` příkaz toocreate hello `registration_tbl` tabulky v databázi:</span><span class="sxs-lookup"><span data-stu-id="d3bce-161">Paste in hello following `CREATE TABLE` command toocreate hello `registration_tbl` table in your database:</span></span>
   
        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);
4. <span data-ttu-id="d3bce-162">V kořenové složce místní aplikace hello vytvořte **index.php** souboru.</span><span class="sxs-lookup"><span data-stu-id="d3bce-162">In hello root of your local application folder create **index.php** file.</span></span>
5. <span data-ttu-id="d3bce-163">Otevřete hello **index.php** soubor v textovém editoru nebo IDE a přidejte hello následující kód a dokončení hello potřebné změny označené jako `//TODO:` komentáře.</span><span class="sxs-lookup"><span data-stu-id="d3bce-163">Open hello **index.php** file in a text editor or IDE and add hello following code, and complete hello necessary changes marked with `//TODO:` comments.</span></span>

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

1. <span data-ttu-id="d3bce-164">V terminálu přejděte tooyour aplikace složku a typ hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d3bce-164">In a terminal go tooyour application folder and type hello following command:</span></span>
   
       php -S localhost:8000

<span data-ttu-id="d3bce-165">Nyní můžete procházet příliš**http://localhost: 8000 /** tootest hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3bce-165">You can now browse too**http://localhost:8000/** tootest hello application.</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="d3bce-166">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="d3bce-166">Publish your app</span></span>
<span data-ttu-id="d3bce-167">Po testování vaší aplikace místně, můžete ho publikovat tooWeb aplikací pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="d3bce-167">After you have tested your app locally, you can publish it tooWeb Apps using Git.</span></span> <span data-ttu-id="d3bce-168">Bude inicializovat místní úložiště Git a publikování aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="d3bce-168">You will initialize your local Git repository and publish hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="d3bce-169">Tyto jsou hello stejné kroky uvedené v portálu Azure na konci hello hello vytvořit webovou aplikaci a sadu hello až publikování Git část výše.</span><span class="sxs-lookup"><span data-stu-id="d3bce-169">These are hello same steps shown in hello Azure Portal at hello end of hello Create a web app and Set up Git Publishing section above.</span></span>
> 
> 

1. <span data-ttu-id="d3bce-170">(Volitelné)  Pokud jste zapomněli, nebo k jejich chybnému umístění URL vzdálené repostitory Git, přejděte toohello webové aplikace vlastnosti hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d3bce-170">(Optional)  If you've forgotten or misplaced your Git remote repostitory URL, navigate toohello web app properties on hello Azure Portal.</span></span>
2. <span data-ttu-id="d3bce-171">Otevřete Git Bash (nebo terminál, pokud Git je ve vaší `PATH`), změňte adresáře toohello kořenový adresář aplikace a spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="d3bce-171">Open GitBash (or a terminal, if Git is in your `PATH`), change directories toohello root directory of your application, and run hello following commands:</span></span>
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    <span data-ttu-id="d3bce-172">Jste vyzváni k hello heslo, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="d3bce-172">You will be prompted for hello password you created earlier.</span></span>
   
    ![Počáteční tooAzure nabízené prostřednictvím Git][git-initial-push]
3. <span data-ttu-id="d3bce-174">Procházet příliš**http://[site name].azurewebsites.net/index.php** toobegin pomocí aplikace hello (tyto informace se uloží na řídicím panelu účet):</span><span class="sxs-lookup"><span data-stu-id="d3bce-174">Browse too**http://[site name].azurewebsites.net/index.php** toobegin using hello application (this information will be stored on your account dashboard):</span></span>
   
    ![Azure webu PHP][running-app]

<span data-ttu-id="d3bce-176">Po publikování aplikace, můžete začít vytvářet tooit změny a použít Git toopublish je.</span><span class="sxs-lookup"><span data-stu-id="d3bce-176">After you have published your app, you can begin making changes tooit and use Git toopublish them.</span></span>

## <a name="publish-changes-tooyour-app"></a><span data-ttu-id="d3bce-177">Publikování aplikace tooyour změny</span><span class="sxs-lookup"><span data-stu-id="d3bce-177">Publish changes tooyour app</span></span>
<span data-ttu-id="d3bce-178">toopublish změny tooyour aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="d3bce-178">toopublish changes tooyour app, follow these steps:</span></span>

1. <span data-ttu-id="d3bce-179">Zkontrolujte změny tooyour aplikace místně.</span><span class="sxs-lookup"><span data-stu-id="d3bce-179">Make changes tooyour app locally.</span></span>
2. <span data-ttu-id="d3bce-180">Otevřete Git Bash (nebo terminál, it Git je ve vaší `PATH`), změňte adresáře toohello kořenový adresář aplikace a spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="d3bce-180">Open GitBash (or a terminal, it Git is in your `PATH`), change directories toohello root directory of your app, and run hello following commands:</span></span>
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    <span data-ttu-id="d3bce-181">Jste vyzváni k hello heslo, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="d3bce-181">You will be prompted for hello password you created earlier.</span></span>
   
    ![Vkládání tooAzure změny webu prostřednictvím Git][git-change-push]
3. <span data-ttu-id="d3bce-183">Procházet příliš**http://[site name].azurewebsites.net/index.php** toosee vaší aplikace a veškeré změny provedené:</span><span class="sxs-lookup"><span data-stu-id="d3bce-183">Browse too**http://[site name].azurewebsites.net/index.php** toosee your app and any changes you may have made:</span></span>
   
    ![Azure webu PHP][running-app]

> [!NOTE]
> <span data-ttu-id="d3bce-185">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="d3bce-185">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="d3bce-186">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="d3bce-186">No credit cards required; no commitments.</span></span>
> 
> 

<a name="composer"></a>

## <a name="enable-composer-automation-with-hello-composer-extension"></a><span data-ttu-id="d3bce-187">Povolit automatickou autora s hello rozšíření autora</span><span class="sxs-lookup"><span data-stu-id="d3bce-187">Enable Composer automation with hello Composer extension</span></span>
<span data-ttu-id="d3bce-188">Ve výchozím nastavení proces nasazení git hello ve službě App Service nic se neděje s composer.json, pokud máte ve vašem projektu PHP.</span><span class="sxs-lookup"><span data-stu-id="d3bce-188">By default, hello git deployment process in App Service doesn't do anything with composer.json, if you have one in your PHP project.</span></span> <span data-ttu-id="d3bce-189">Můžete povolit composer.json zpracování během `git push` povolením rozšíření autora hello.</span><span class="sxs-lookup"><span data-stu-id="d3bce-189">You can enable composer.json processing during `git push` by enabling hello Composer extension.</span></span>

1. <span data-ttu-id="d3bce-190">Ve vašem PHP webové okně aplikace v hello [portál Azure][management-portal], klikněte na tlačítko **nástroje** > **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="d3bce-190">In your PHP web app's blade in hello [Azure portal][management-portal], click **Tools** > **Extensions**.</span></span>
   
    ![Nastavení rozšíření autora][composer-extension-settings]
2. <span data-ttu-id="d3bce-192">Klikněte na tlačítko **přidat**, pak klikněte na tlačítko **autora**.</span><span class="sxs-lookup"><span data-stu-id="d3bce-192">Click **Add**, then click **Composer**.</span></span>
   
    ![Přidání rozšíření autora][composer-extension-add]
3. <span data-ttu-id="d3bce-194">Klikněte na tlačítko **OK** tooaccept právní podmínky.</span><span class="sxs-lookup"><span data-stu-id="d3bce-194">Click **OK** tooaccept legal terms.</span></span> <span data-ttu-id="d3bce-195">Klikněte na tlačítko **OK** znovu tooadd hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d3bce-195">Click **OK** again tooadd hello extension.</span></span>
   
    <span data-ttu-id="d3bce-196">Hello **nainstalovaná rozšíření** okno se nyní zobrazí rozšíření autora hello.</span><span class="sxs-lookup"><span data-stu-id="d3bce-196">hello **Installed extensions** blade will now show hello Composer extension.</span></span>  
    <span data-ttu-id="d3bce-197">![Zobrazení rozšíření autora][composer-extension-view]</span><span class="sxs-lookup"><span data-stu-id="d3bce-197">![Composer Extension View][composer-extension-view]</span></span>
4. <span data-ttu-id="d3bce-198">Teď, provádět `git add`, `git commit`, a `git push` jako v předchozím oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="d3bce-198">Now, perform `git add`, `git commit`, and `git push` like in hello previous section.</span></span> <span data-ttu-id="d3bce-199">Nyní se zobrazí, autora je instalování závislostí, které jsou definované v composer.json.</span><span class="sxs-lookup"><span data-stu-id="d3bce-199">You'll now see that Composer is installing dependencies defined in composer.json.</span></span>
   
    ![Úspěch rozšíření autora][composer-extension-success]

## <a name="next-steps"></a><span data-ttu-id="d3bce-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d3bce-201">Next steps</span></span>
<span data-ttu-id="d3bce-202">Další informace najdete v tématu hello [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="d3bce-202">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

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
