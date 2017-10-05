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
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a><span data-ttu-id="15aeb-103">Vytvoření webové aplikace v PHP-MySQL ve službě Azure App Service a nasazení přes Git</span><span class="sxs-lookup"><span data-stu-id="15aeb-103">Create a PHP-MySQL web app in Azure App Service and deploy using Git</span></span>
<span data-ttu-id="15aeb-104">Tento kurz ukazuje, jak k vytvoření webové aplikace PHP MySQL a jak ho nasadit do [služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="15aeb-104">This tutorial shows you how to create a PHP-MySQL web app and how to deploy it to [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) using Git.</span></span> <span data-ttu-id="15aeb-105">Budete používat [PHP][install-php], nástroje příkazového řádku MySQL (součástí [MySQL][install-mysql]), a [Git] [ install-git] v počítači nainstalována.</span><span class="sxs-lookup"><span data-stu-id="15aeb-105">You will use [PHP][install-php], the MySQL Command-Line Tool (part of [MySQL][install-mysql]), and [Git][install-git] installed on your computer.</span></span> <span data-ttu-id="15aeb-106">Pokyny v tomto kurzu platí pro všechny operační systémy, včetně systému Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="15aeb-106">The instructions in this tutorial can be followed on any operating system, including Windows, Mac, and  Linux.</span></span> <span data-ttu-id="15aeb-107">Po dokončení tohoto průvodce, budete mít webovou aplikaci PHP nebo MySQL běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="15aeb-107">Upon completing this guide, you will have a PHP/MySQL web app running in Azure.</span></span>

<span data-ttu-id="15aeb-108">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="15aeb-108">You will learn:</span></span>

* <span data-ttu-id="15aeb-109">Jak vytvořit webovou aplikaci a pomocí databáze MySQL [portálu Azure][management-portal].</span><span class="sxs-lookup"><span data-stu-id="15aeb-109">How to create a web app and a MySQL database using the [Azure Portal][management-portal].</span></span> <span data-ttu-id="15aeb-110">Protože je v PHP [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) ve výchozím nastavení, nic speciální se vyžaduje ke spuštění kódu PHP.</span><span class="sxs-lookup"><span data-stu-id="15aeb-110">Because PHP is enabled in [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) by default, nothing special is required to run your PHP code.</span></span>
* <span data-ttu-id="15aeb-111">Postup publikování a znovu publikovat svoji aplikaci do Azure pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="15aeb-111">How to publish and re-publish your application to Azure using Git.</span></span>
* <span data-ttu-id="15aeb-112">Postup povolení rozšíření autora autora automatizovat úlohy v každé `git push`.</span><span class="sxs-lookup"><span data-stu-id="15aeb-112">How to enable the Composer extension to automate Composer tasks at every `git push`.</span></span>

<span data-ttu-id="15aeb-113">Podle tohoto kurzu vytvoříte registrace jednoduché webové aplikace v jazyce PHP.</span><span class="sxs-lookup"><span data-stu-id="15aeb-113">By following this tutorial, you will build a simple registration web app in PHP.</span></span> <span data-ttu-id="15aeb-114">Aplikace hostované ve webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="15aeb-114">The application will be hosted in Web Apps.</span></span> <span data-ttu-id="15aeb-115">Zde je snímek obrazovky dokončené aplikace:</span><span class="sxs-lookup"><span data-stu-id="15aeb-115">A screenshot of the completed application is below:</span></span>

![Azure webu PHP][running-app]

## <a name="set-up-the-development-environment"></a><span data-ttu-id="15aeb-117">Nastavení vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="15aeb-117">Set up the development environment</span></span>
<span data-ttu-id="15aeb-118">Tento kurz předpokládá, že máte [PHP][install-php], nástroje příkazového řádku MySQL (součástí [MySQL][install-mysql]), a [Git] [ install-git] v počítači nainstalována.</span><span class="sxs-lookup"><span data-stu-id="15aeb-118">This tutorial assumes you have [PHP][install-php], the MySQL Command-Line Tool (part of [MySQL][install-mysql]), and [Git][install-git] installed on your computer.</span></span>

<a id="create-web-site-and-set-up-git"></a>

## <a name="create-a-web-app-and-set-up-git-publishing"></a><span data-ttu-id="15aeb-119">Vytvoření webové aplikace a nastavení publikování Git</span><span class="sxs-lookup"><span data-stu-id="15aeb-119">Create a web app and set up Git publishing</span></span>
<span data-ttu-id="15aeb-120">Postupujte podle těchto kroků můžete vytvořit webovou aplikaci a databáze MySQL:</span><span class="sxs-lookup"><span data-stu-id="15aeb-120">Follow these steps to create a web app and a MySQL database:</span></span>

1. <span data-ttu-id="15aeb-121">Přihlášení k [portál Azure][management-portal].</span><span class="sxs-lookup"><span data-stu-id="15aeb-121">Login to the [Azure Portal][management-portal].</span></span>
2. <span data-ttu-id="15aeb-122">Klikněte **nový** ikonu.</span><span class="sxs-lookup"><span data-stu-id="15aeb-122">Click the **New** icon.</span></span>
3. <span data-ttu-id="15aeb-123">Klikněte na tlačítko **najdete v článku všechny** vedle **Marketplace**.</span><span class="sxs-lookup"><span data-stu-id="15aeb-123">Click **See All** next to **Marketplace**.</span></span> 
4. <span data-ttu-id="15aeb-124">Klikněte na tlačítko **Web + mobilní**, pak **Web app + MySQL**.</span><span class="sxs-lookup"><span data-stu-id="15aeb-124">Click **Web + Mobile**, then **Web app + MySQL**.</span></span> <span data-ttu-id="15aeb-125">Poté klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="15aeb-125">Then, click **Create**.</span></span>
5. <span data-ttu-id="15aeb-126">Zadejte platný název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="15aeb-126">Enter a valid name for your resource group.</span></span>
   
    ![Název skupiny prostředků sady][resource-group]
6. <span data-ttu-id="15aeb-128">Zadejte hodnoty pro novou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="15aeb-128">Enter values for your new web app.</span></span>
   
    ![Vytvoření webové aplikace][new-web-app]
7. <span data-ttu-id="15aeb-130">Zadejte hodnoty pro novou databázi, včetně, kterým Odsouhlasíte právní podmínky.</span><span class="sxs-lookup"><span data-stu-id="15aeb-130">Enter values for your new database, including agreeing to the legal terms.</span></span>
   
    ![Vytvořit novou databázi MySQL][new-mysql-db]
8. <span data-ttu-id="15aeb-132">Po vytvoření webové aplikace, zobrazí se nové okno webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="15aeb-132">When the web app has been created, you will see the new web app blade.</span></span>
9. <span data-ttu-id="15aeb-133">V **nastavení** klikněte na **průběžné nasazování**, potom klikněte na *konfigurovat požadované nastavení*.</span><span class="sxs-lookup"><span data-stu-id="15aeb-133">In **Settings** click on **Continuous Deployment**, then click on *Configure required settings*.</span></span>
   
    ![Nastavit publikování Git][setup-publishing]
10. <span data-ttu-id="15aeb-135">Vyberte **místní úložiště Git** zdroje.</span><span class="sxs-lookup"><span data-stu-id="15aeb-135">Select **Local Git Repository** for the source.</span></span>
    
     ![Nastavení úložiště Git][setup-repository]
11. <span data-ttu-id="15aeb-137">Povolit publikování Git, je nutné zadat uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="15aeb-137">To enable Git publishing, you must provide a user name and password.</span></span> <span data-ttu-id="15aeb-138">Poznamenejte si uživatelské jméno a heslo, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="15aeb-138">Make a note of the user name and password you create.</span></span> <span data-ttu-id="15aeb-139">(Pokud jste nastavili úložiště Git před, bude přeskočen tento krok.)</span><span class="sxs-lookup"><span data-stu-id="15aeb-139">(If you have set up a Git repository before, this step will be skipped.)</span></span>
    
     ![Vytvořit přihlašovací údaje pro publikování][credentials]

## <a name="get-remote-mysql-connection-information"></a><span data-ttu-id="15aeb-141">Získat informace o připojení k vzdálené MySQL</span><span class="sxs-lookup"><span data-stu-id="15aeb-141">Get remote MySQL connection information</span></span>
<span data-ttu-id="15aeb-142">Pro připojení k databázi MySQL, která běží ve službě Web Apps, vaše bude potřebovat informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="15aeb-142">To connect to the MySQL database that is running in Web Apps, your will need the connection information.</span></span> <span data-ttu-id="15aeb-143">Chcete-li získat informace o připojení databáze MySQL, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="15aeb-143">To get MySQL connection information, follow these steps:</span></span>

1. <span data-ttu-id="15aeb-144">Od vaší skupiny prostředků klikněte na databázi:</span><span class="sxs-lookup"><span data-stu-id="15aeb-144">From your resource group, click the database:</span></span>
   
    ![Vyberte databázi][select-database]
2. <span data-ttu-id="15aeb-146">Z databáze **nastavení**, vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="15aeb-146">From the database **Settings**, select **Properties**.</span></span>
   
    ![Výběr vlastností][select-properties]
3. <span data-ttu-id="15aeb-148">Poznamenejte si hodnoty pro `Database`, `Host`, `User Id`, a `Password`.</span><span class="sxs-lookup"><span data-stu-id="15aeb-148">Make note of the values for `Database`, `Host`, `User Id`, and `Password`.</span></span>
   
    ![Poznámka: vlastnosti][note-properties]

## <a name="build-and-test-your-app-locally"></a><span data-ttu-id="15aeb-150">Vytvoření a testování vaší aplikace místně</span><span class="sxs-lookup"><span data-stu-id="15aeb-150">Build and test your app locally</span></span>
<span data-ttu-id="15aeb-151">Teď, když jste vytvořili webovou aplikaci, můžete vyvíjet aplikaci místně a pak ho nasadit po testování.</span><span class="sxs-lookup"><span data-stu-id="15aeb-151">Now that you have created a web app, you can develop your application locally, then deploy it after testing.</span></span>

<span data-ttu-id="15aeb-152">Registrace aplikace je jednoduchou aplikaci PHP, která umožňuje zaregistrovat pro událost tím, že poskytuje vaše jméno a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="15aeb-152">The Registration application is a simple PHP application that allows you to register for an event by providing your name and email address.</span></span> <span data-ttu-id="15aeb-153">Zobrazí se informace o předchozích rejstříkem v tabulce.</span><span class="sxs-lookup"><span data-stu-id="15aeb-153">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="15aeb-154">Registrační informace jsou uloženy v databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="15aeb-154">Registration information is stored in a MySQL database.</span></span> <span data-ttu-id="15aeb-155">Aplikace se skládá z jednoho souboru (zkopírujte a vložte kód je k dispozici níže):</span><span class="sxs-lookup"><span data-stu-id="15aeb-155">The application consists of one file (copy/paste code available below):</span></span>

* <span data-ttu-id="15aeb-156">**index.php**: Zobrazí formulář pro registraci a tabulku obsahující informace osob žádajících o registraci.</span><span class="sxs-lookup"><span data-stu-id="15aeb-156">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>

<span data-ttu-id="15aeb-157">Sestavení a spuštění aplikace místně, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="15aeb-157">To build and run the application locally, follow the steps below.</span></span> <span data-ttu-id="15aeb-158">Všimněte si, že tento postup předpokládá, máte PHP a databáze MySQL nástroj příkazového řádku (součást MySQL) nastavit na místním počítači a pokud jste povolili [PDO rozšíření pro databázi MySQL][pdo-mysql].</span><span class="sxs-lookup"><span data-stu-id="15aeb-158">Note that these steps assume you have the PHP and MySQL Command-Line Tool (part of MySQL) set up on your local machine, and that you have enabled the [PDO extension for MySQL][pdo-mysql].</span></span>

1. <span data-ttu-id="15aeb-159">Připojit ke vzdálenému serveru MySQL, pomocí hodnoty pro `Data Source`, `User Id`, `Password`, a `Database` , který jste získali dříve:</span><span class="sxs-lookup"><span data-stu-id="15aeb-159">Connect to the remote MySQL server, using the value for `Data Source`, `User Id`, `Password`, and `Database` that you retrieved earlier:</span></span>
   
        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]
2. <span data-ttu-id="15aeb-160">MySQL příkazového řádku se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="15aeb-160">The MySQL command prompt will appear:</span></span>
   
        mysql>
3. <span data-ttu-id="15aeb-161">Vložte následující `CREATE TABLE` příkazu vytvořte `registration_tbl` tabulky v databázi:</span><span class="sxs-lookup"><span data-stu-id="15aeb-161">Paste in the following `CREATE TABLE` command to create the `registration_tbl` table in your database:</span></span>
   
        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);
4. <span data-ttu-id="15aeb-162">V kořenové složce místní aplikace vytvořte **index.php** souboru.</span><span class="sxs-lookup"><span data-stu-id="15aeb-162">In the root of your local application folder create **index.php** file.</span></span>
5. <span data-ttu-id="15aeb-163">Otevřete **index.php** soubor v textovém editoru nebo IDE a přidejte následující kód a provést potřebné změny, které jsou označené jako `//TODO:` komentáře.</span><span class="sxs-lookup"><span data-stu-id="15aeb-163">Open the **index.php** file in a text editor or IDE and add the following code, and complete the necessary changes marked with `//TODO:` comments.</span></span>

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

1. <span data-ttu-id="15aeb-164">V terminálu přejděte do složky aplikace a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="15aeb-164">In a terminal go to your application folder and type the following command:</span></span>
   
       php -S localhost:8000

<span data-ttu-id="15aeb-165">Nyní můžete procházet k **http://localhost: 8000 /** k testování aplikace.</span><span class="sxs-lookup"><span data-stu-id="15aeb-165">You can now browse to **http://localhost:8000/** to test the application.</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="15aeb-166">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="15aeb-166">Publish your app</span></span>
<span data-ttu-id="15aeb-167">Po testování vaší aplikace místně, můžete ji publikovat pomocí Git webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="15aeb-167">After you have tested your app locally, you can publish it to Web Apps using Git.</span></span> <span data-ttu-id="15aeb-168">Bude inicializovat místní úložiště Git a publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="15aeb-168">You will initialize your local Git repository and publish the application.</span></span>

> [!NOTE]
> <span data-ttu-id="15aeb-169">Toto jsou stejné kroky uvedené na portálu Azure na konci vytvořit webovou aplikaci a sadu až publikování Git část výše.</span><span class="sxs-lookup"><span data-stu-id="15aeb-169">These are the same steps shown in the Azure Portal at the end of the Create a web app and Set up Git Publishing section above.</span></span>
> 
> 

1. <span data-ttu-id="15aeb-170">(Volitelné)  Pokud jste zapomněli, nebo k jejich chybnému umístění URL vzdálené repostitory Git, přejděte do vlastností webové aplikace na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="15aeb-170">(Optional)  If you've forgotten or misplaced your Git remote repostitory URL, navigate to the web app properties on the Azure Portal.</span></span>
2. <span data-ttu-id="15aeb-171">Otevřete Git Bash (nebo terminál, pokud Git je ve vaší `PATH`), změňte adresáře na kořenový adresář aplikace a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="15aeb-171">Open GitBash (or a terminal, if Git is in your `PATH`), change directories to the root directory of your application, and run the following commands:</span></span>
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    <span data-ttu-id="15aeb-172">Zobrazí se výzva k zadání hesla, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="15aeb-172">You will be prompted for the password you created earlier.</span></span>
   
    ![Počáteční nabízené Azure prostřednictvím Git][git-initial-push]
3. <span data-ttu-id="15aeb-174">Přejděte do **http://[site name].azurewebsites.net/index.php** začít používat aplikace (tyto informace se uloží na řídicím panelu účet):</span><span class="sxs-lookup"><span data-stu-id="15aeb-174">Browse to **http://[site name].azurewebsites.net/index.php** to begin using the application (this information will be stored on your account dashboard):</span></span>
   
    ![Azure webu PHP][running-app]

<span data-ttu-id="15aeb-176">Po publikování aplikace, můžete začít, provedení změn a publikovat je pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="15aeb-176">After you have published your app, you can begin making changes to it and use Git to publish them.</span></span>

## <a name="publish-changes-to-your-app"></a><span data-ttu-id="15aeb-177">Publikování změn do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="15aeb-177">Publish changes to your app</span></span>
<span data-ttu-id="15aeb-178">Při publikování změn do vaší aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="15aeb-178">To publish changes to your app, follow these steps:</span></span>

1. <span data-ttu-id="15aeb-179">Proveďte změny aplikace místně.</span><span class="sxs-lookup"><span data-stu-id="15aeb-179">Make changes to your app locally.</span></span>
2. <span data-ttu-id="15aeb-180">Otevřete Git Bash (nebo terminál, it Git je ve vaší `PATH`), změňte adresáře na kořenový adresář aplikace a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="15aeb-180">Open GitBash (or a terminal, it Git is in your `PATH`), change directories to the root directory of your app, and run the following commands:</span></span>
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    <span data-ttu-id="15aeb-181">Zobrazí se výzva k zadání hesla, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="15aeb-181">You will be prompted for the password you created earlier.</span></span>
   
    ![Vkládání změn lokalit k Azure přes Git][git-change-push]
3. <span data-ttu-id="15aeb-183">Přejděte do **http://[site name].azurewebsites.net/index.php** vaší aplikace a všechny změny provedené:</span><span class="sxs-lookup"><span data-stu-id="15aeb-183">Browse to **http://[site name].azurewebsites.net/index.php** to see your app and any changes you may have made:</span></span>
   
    ![Azure webu PHP][running-app]

> [!NOTE]
> <span data-ttu-id="15aeb-185">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="15aeb-185">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="15aeb-186">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="15aeb-186">No credit cards required; no commitments.</span></span>
> 
> 

<a name="composer"></a>

## <a name="enable-composer-automation-with-the-composer-extension"></a><span data-ttu-id="15aeb-187">Povolit automatickou autora s příponou autora</span><span class="sxs-lookup"><span data-stu-id="15aeb-187">Enable Composer automation with the Composer extension</span></span>
<span data-ttu-id="15aeb-188">Ve výchozím nastavení proces nasazení git ve službě App Service nic se neděje s composer.json, pokud máte ve vašem projektu PHP.</span><span class="sxs-lookup"><span data-stu-id="15aeb-188">By default, the git deployment process in App Service doesn't do anything with composer.json, if you have one in your PHP project.</span></span> <span data-ttu-id="15aeb-189">Můžete povolit composer.json zpracování během `git push` povolením rozšíření autora.</span><span class="sxs-lookup"><span data-stu-id="15aeb-189">You can enable composer.json processing during `git push` by enabling the Composer extension.</span></span>

1. <span data-ttu-id="15aeb-190">Ve vašem PHP webové okně aplikace v [portál Azure][management-portal], klikněte na tlačítko **nástroje** > **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="15aeb-190">In your PHP web app's blade in the [Azure portal][management-portal], click **Tools** > **Extensions**.</span></span>
   
    ![Nastavení rozšíření autora][composer-extension-settings]
2. <span data-ttu-id="15aeb-192">Klikněte na tlačítko **přidat**, pak klikněte na tlačítko **autora**.</span><span class="sxs-lookup"><span data-stu-id="15aeb-192">Click **Add**, then click **Composer**.</span></span>
   
    ![Přidání rozšíření autora][composer-extension-add]
3. <span data-ttu-id="15aeb-194">Klikněte na tlačítko **OK** přijměte právní podmínky.</span><span class="sxs-lookup"><span data-stu-id="15aeb-194">Click **OK** to accept legal terms.</span></span> <span data-ttu-id="15aeb-195">Klikněte na tlačítko **OK** přidat rozšíření.</span><span class="sxs-lookup"><span data-stu-id="15aeb-195">Click **OK** again to add the extension.</span></span>
   
    <span data-ttu-id="15aeb-196">**Nainstalovaná rozšíření** okno se nyní zobrazí rozšíření autora.</span><span class="sxs-lookup"><span data-stu-id="15aeb-196">The **Installed extensions** blade will now show the Composer extension.</span></span>  
    <span data-ttu-id="15aeb-197">![Zobrazení rozšíření autora][composer-extension-view]</span><span class="sxs-lookup"><span data-stu-id="15aeb-197">![Composer Extension View][composer-extension-view]</span></span>
4. <span data-ttu-id="15aeb-198">Teď, provádět `git add`, `git commit`, a `git push` jako v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="15aeb-198">Now, perform `git add`, `git commit`, and `git push` like in the previous section.</span></span> <span data-ttu-id="15aeb-199">Nyní se zobrazí, autora je instalování závislostí, které jsou definované v composer.json.</span><span class="sxs-lookup"><span data-stu-id="15aeb-199">You'll now see that Composer is installing dependencies defined in composer.json.</span></span>
   
    ![Úspěch rozšíření autora][composer-extension-success]

## <a name="next-steps"></a><span data-ttu-id="15aeb-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="15aeb-201">Next steps</span></span>
<span data-ttu-id="15aeb-202">Další informace najdete v tématu [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="15aeb-202">For more information, see the [PHP Developer Center](/develop/php/).</span></span>

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
