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
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a><span data-ttu-id="8be25-103">Vytvoření webové aplikace v PHP-MySQL ve službě Azure App Service a nasazení přes FTP</span><span class="sxs-lookup"><span data-stu-id="8be25-103">Create a PHP-MySQL web app in Azure App Service and deploy using FTP</span></span>
<span data-ttu-id="8be25-104">Tento kurz ukazuje, jak toocreate PHP-MySQL, webová aplikace a jak toodeploy pomocí služby FTP.</span><span class="sxs-lookup"><span data-stu-id="8be25-104">This tutorial shows you how toocreate a PHP-MySQL web app and how toodeploy it using FTP.</span></span> <span data-ttu-id="8be25-105">Tento kurz předpokládá, že máte [PHP][install-php], [MySQL][install-mysql], webového serveru a klienta v počítači nainstalována.</span><span class="sxs-lookup"><span data-stu-id="8be25-105">This tutorial assumes you have [PHP][install-php], [MySQL][install-mysql], a web server, and an FTP client installed on your computer.</span></span> <span data-ttu-id="8be25-106">Hello pokyny v tomto kurzu platí pro všechny operační systémy, včetně systému Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="8be25-106">hello instructions in this tutorial can be followed on any operating system, including Windows, Mac, and  Linux.</span></span> <span data-ttu-id="8be25-107">Po dokončení tohoto průvodce, budete mít webovou aplikaci PHP nebo MySQL běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="8be25-107">Upon completing this guide, you will have a PHP/MySQL web app running in Azure.</span></span>

<span data-ttu-id="8be25-108">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="8be25-108">You will learn:</span></span>

* <span data-ttu-id="8be25-109">Jak hello toocreate webovou aplikaci a MySQL databáze pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8be25-109">How toocreate a web app and a MySQL database using hello Azure Portal.</span></span> <span data-ttu-id="8be25-110">Vzhledem k tomu, že jazyk PHP je ve výchozím nastavení povolena ve službě Web Apps, nic speciální je požadovaná toorun kódu PHP.</span><span class="sxs-lookup"><span data-stu-id="8be25-110">Because PHP is enabled in Web Apps by default, nothing special is required toorun your PHP code.</span></span>
* <span data-ttu-id="8be25-111">Jak toopublish tooAzure aplikace pomocí serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="8be25-111">How toopublish your application tooAzure using FTP.</span></span>

<span data-ttu-id="8be25-112">Podle tohoto kurzu vytvoříte registrace jednoduché webové aplikace v jazyce PHP.</span><span class="sxs-lookup"><span data-stu-id="8be25-112">By following this tutorial, you will build a simple registration web app in PHP.</span></span> <span data-ttu-id="8be25-113">aplikace Hello bude hostován ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8be25-113">hello application will be hosted in a Web App.</span></span> <span data-ttu-id="8be25-114">Zde je snímek obrazovky aplikace hello dokončena:</span><span class="sxs-lookup"><span data-stu-id="8be25-114">A screenshot of hello completed application is below:</span></span>

![Webové stránky Azure PHP][running-app]

> [!NOTE]
> <span data-ttu-id="8be25-116">Pokud chcete začít s Azure App Service před registrací účtu tooget, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="8be25-116">If you want tooget started with Azure App Service before signing up for an account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="8be25-117">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="8be25-117">No credit cards required, no commitments.</span></span> 
> 
> 

## <a name="create-a-web-app-and-set-up-ftp-publishing"></a><span data-ttu-id="8be25-118">Vytvoření webové aplikace a nastavení publikování FTP</span><span class="sxs-lookup"><span data-stu-id="8be25-118">Create a web app and set up FTP publishing</span></span>
<span data-ttu-id="8be25-119">Webové aplikace a databáze MySQL, postupujte podle těchto kroků toocreate:</span><span class="sxs-lookup"><span data-stu-id="8be25-119">Follow these steps toocreate a web app and a MySQL database:</span></span>

1. <span data-ttu-id="8be25-120">Přihlášení toohello [portálu Azure][management-portal].</span><span class="sxs-lookup"><span data-stu-id="8be25-120">Login toohello [Azure Portal][management-portal].</span></span>
2. <span data-ttu-id="8be25-121">Klikněte na tlačítko hello **+ nový** ikonu na hello horní pravé hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8be25-121">Click hello **+ New** icon on hello top left of hello Azure Portal.</span></span>
   
    ![Vytvořit nový web Azure][new-website]
3. <span data-ttu-id="8be25-123">V typu vyhledávání hello **Web app + MySQL** a klikněte na **Web app + MySQL**.</span><span class="sxs-lookup"><span data-stu-id="8be25-123">In hello search type **Web app + MySQL** and click on **Web app + MySQL**.</span></span>
   
    ![Vlastní vytvořit novou webovou stránku][custom-create]
4. <span data-ttu-id="8be25-125">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8be25-125">Click **Create**.</span></span> <span data-ttu-id="8be25-126">Zadejte název jedinečné app service, platný název pro skupinu prostředků hello a nový plán služby.</span><span class="sxs-lookup"><span data-stu-id="8be25-126">Enter a unique app service name, a valid name for hello resource group and a new service plan.</span></span>
   
    ![Název skupiny prostředků sady][resource-group]
5. <span data-ttu-id="8be25-128">Zadejte hodnoty pro novou databázi, včetně, kterým Odsouhlasíte toohello právní podmínky.</span><span class="sxs-lookup"><span data-stu-id="8be25-128">Enter values for your new database, including agreeing toohello legal terms.</span></span>
   
    ![Vytvořit novou databázi MySQL][new-mysql-db]
6. <span data-ttu-id="8be25-130">Po vytvoření webové aplikace hello, zobrazí se okno hello nové aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="8be25-130">When hello web app has been created, you will see hello new app service blade.</span></span>
7. <span data-ttu-id="8be25-131">Klikněte na **nastavení** > **přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="8be25-131">Click on **Settings** > **Deployment credentials**.</span></span> 
   
    ![Nastavení přihlašovacích údajů nasazení][set-deployment-credentials]
8. <span data-ttu-id="8be25-133">tooenable publikování FTP, je nutné zadat uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="8be25-133">tooenable FTP publishing, you must provide a user name and password.</span></span> <span data-ttu-id="8be25-134">Uložit pověření hello a poznamenejte si hello uživatelské jméno a heslo, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="8be25-134">Save hello credentials and make a note of hello user name and password you create.</span></span>
   
    ![Vytvořit přihlašovací údaje pro publikování][portal-ftp-username-password]

## <a name="build-and-test-your-app-locally"></a><span data-ttu-id="8be25-136">Vytvoření a testování vaší aplikace místně</span><span class="sxs-lookup"><span data-stu-id="8be25-136">Build and test your app locally</span></span>
<span data-ttu-id="8be25-137">Registrace aplikace Hello je jednoduchou aplikaci PHP, která vám umožní tooregister pro událost tím, že poskytuje vaše jméno a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="8be25-137">hello Registration application is a simple PHP application that allows you tooregister for an event by providing your name and email address.</span></span> <span data-ttu-id="8be25-138">Zobrazí se informace o předchozích rejstříkem v tabulce.</span><span class="sxs-lookup"><span data-stu-id="8be25-138">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="8be25-139">Registrační informace jsou uloženy v databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="8be25-139">Registration information is stored in a MySQL database.</span></span> <span data-ttu-id="8be25-140">Hello aplikace se skládá z dva soubory:</span><span class="sxs-lookup"><span data-stu-id="8be25-140">hello app consists of two files:</span></span>

* <span data-ttu-id="8be25-141">**index.php**: Zobrazí formulář pro registraci a tabulku obsahující informace osob žádajících o registraci.</span><span class="sxs-lookup"><span data-stu-id="8be25-141">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>
* <span data-ttu-id="8be25-142">**CreateTable.php**: vytvoří tabulku hello MySQL pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="8be25-142">**createtable.php**: Creates hello MySQL table for hello application.</span></span> <span data-ttu-id="8be25-143">Tento soubor se dají použít jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="8be25-143">This file will only be used once.</span></span>

<span data-ttu-id="8be25-144">toobuild a spuštění hello aplikaci místně, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="8be25-144">toobuild and run hello app locally, follow hello steps below.</span></span> <span data-ttu-id="8be25-145">Všimněte si, že tento postup předpokládá, že máte PHP, MySQL a webový server nastavit na místním počítači, a že je povoleno hello [PDO rozšíření pro databázi MySQL][pdo-mysql].</span><span class="sxs-lookup"><span data-stu-id="8be25-145">Note that these steps assume you have PHP, MySQL, and a web server set up on your local machine, and that you have enabled hello [PDO extension for MySQL][pdo-mysql].</span></span>

1. <span data-ttu-id="8be25-146">Vytvoření databáze MySQL názvem `registration`.</span><span class="sxs-lookup"><span data-stu-id="8be25-146">Create a MySQL database called `registration`.</span></span> <span data-ttu-id="8be25-147">Můžete to udělat z hello MySQL příkazového řádku pomocí tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="8be25-147">You can do this from hello MySQL command prompt with this command:</span></span>
   
        mysql> create database registration;
2. <span data-ttu-id="8be25-148">V kořenovém adresáři váš webový server, vytvořte složku s názvem `registration` a vytvořit dva soubory v ní – jednu s názvem `createtable.php` a jednu s názvem `index.php`.</span><span class="sxs-lookup"><span data-stu-id="8be25-148">In your web server's root directory, create a folder called `registration` and create two files in it - one called `createtable.php` and one called `index.php`.</span></span>
3. <span data-ttu-id="8be25-149">Otevřete hello `createtable.php` soubor v textovém editoru nebo IDE a přidejte následující kód hello.</span><span class="sxs-lookup"><span data-stu-id="8be25-149">Open hello `createtable.php` file in a text editor or IDE and add hello code below.</span></span> <span data-ttu-id="8be25-150">Tento kód bude hello použité toocreate `registration_tbl` tabulky v hello `registration` databáze.</span><span class="sxs-lookup"><span data-stu-id="8be25-150">This code will be used toocreate hello `registration_tbl` table in hello `registration` database.</span></span>
   
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
   > <span data-ttu-id="8be25-151">Budete potřebovat tooupdate hello hodnoty pro <code>$user</code> a <code>$pwd</code> s vaší místní MySQL uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="8be25-151">You will need tooupdate hello values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
   > 
   > 
4. <span data-ttu-id="8be25-152">Otevřete webový prohlížeč a vyhledejte příliš[http://localhost/registration/createtable.php][localhost-createtable].</span><span class="sxs-lookup"><span data-stu-id="8be25-152">Open a web browser and browse too[http://localhost/registration/createtable.php][localhost-createtable].</span></span> <span data-ttu-id="8be25-153">Tím se vytvoří hello `registration_tbl` tabulky v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="8be25-153">This will create hello `registration_tbl` table in hello database.</span></span>
5. <span data-ttu-id="8be25-154">Otevřete hello **index.php** soubor v textovém editoru nebo IDE a přidejte hello kód základní HTML a CSS hello stránky (v následujících krocích se přidají hello kódu PHP.).</span><span class="sxs-lookup"><span data-stu-id="8be25-154">Open hello **index.php** file in a text editor or IDE and add hello basic HTML and CSS code for hello page (hello PHP code will be added in later steps).</span></span>
   
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
6. <span data-ttu-id="8be25-155">V rámci hello PHP značky přidejte kód PHP pro připojení toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="8be25-155">Within hello PHP tags, add PHP code for connecting toohello database.</span></span>
   
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
   > <span data-ttu-id="8be25-156">Znovu, budete potřebovat tooupdate hello hodnoty pro <code>$user</code> a <code>$pwd</code> s vaší místní MySQL uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="8be25-156">Again, you will need tooupdate hello values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
   > 
   > 
7. <span data-ttu-id="8be25-157">Následující kód připojení databáze hello přidáte kód pro vložení registrační informace do databáze hello.</span><span class="sxs-lookup"><span data-stu-id="8be25-157">Following hello database connection code, add code for inserting registration information into hello database.</span></span>
   
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
8. <span data-ttu-id="8be25-158">Nakonec následující kód hello výše, přidejte kód pro načítání dat z databáze hello.</span><span class="sxs-lookup"><span data-stu-id="8be25-158">Finally, following hello code above, add code for retrieving data from hello database.</span></span>
   
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

<span data-ttu-id="8be25-159">Nyní můžete procházet příliš[http://localhost/registration/index.php] [ localhost-index] tootest hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8be25-159">You can now browse too[http://localhost/registration/index.php][localhost-index] tootest hello app.</span></span>

## <a name="get-mysql-and-ftp-connection-information"></a><span data-ttu-id="8be25-160">Získat informace o připojení databáze MySQL a FTP</span><span class="sxs-lookup"><span data-stu-id="8be25-160">Get MySQL and FTP connection information</span></span>
<span data-ttu-id="8be25-161">databáze MySQL toohello tooconnect, které běží ve službě Web Apps, vaše bude potřebovat hello informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="8be25-161">tooconnect toohello MySQL database that is running in Web Apps, your will need hello connection information.</span></span> <span data-ttu-id="8be25-162">tooget informace o připojení databáze MySQL, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="8be25-162">tooget MySQL connection information, follow these steps:</span></span>

1. <span data-ttu-id="8be25-163">Ze služby aplikace hello okně webové aplikace klikněte na odkaz skupinu prostředků hello:</span><span class="sxs-lookup"><span data-stu-id="8be25-163">From hello app service web app blade click on hello resource group link:</span></span>
   
    ![Vyberte skupinu prostředků][select-resourcegroup]
2. <span data-ttu-id="8be25-165">Od vaší skupiny prostředků klikněte na databázi hello:</span><span class="sxs-lookup"><span data-stu-id="8be25-165">From your resource group, click hello database:</span></span>
   
    ![Vyberte databázi][select-database]
3. <span data-ttu-id="8be25-167">Z databáze hello souhrn, vyberte **nastavení** > **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="8be25-167">From hello database summary, select **Settings** > **Properties**.</span></span>
   
    ![Výběr vlastností][select-properties]
4. <span data-ttu-id="8be25-169">Poznamenejte si hodnoty hello `Database`, `Host`, `User Id`, a `Password`.</span><span class="sxs-lookup"><span data-stu-id="8be25-169">Make note of hello values for `Database`, `Host`, `User Id`, and `Password`.</span></span>
   
    ![Poznámka: vlastnosti][note-properties]
5. <span data-ttu-id="8be25-171">Z vaší webové aplikace, klikněte na tlačítko hello **stažení profilu publikování** odkaz na hello pravém dolním rohu stránky hello:</span><span class="sxs-lookup"><span data-stu-id="8be25-171">From your web app, click hello **Download publish profile** link at hello bottom right corner of hello page:</span></span>
   
    ![Stažení profilu publikování][download-publish-profile]
6. <span data-ttu-id="8be25-173">Otevřete hello `.publishsettings` souboru v editoru XML.</span><span class="sxs-lookup"><span data-stu-id="8be25-173">Open hello `.publishsettings` file in an XML editor.</span></span> 
7. <span data-ttu-id="8be25-174">Najde hello `<publishProfile >` element s `publishMethod="FTP"` toto vypadá podobně jako toothis:</span><span class="sxs-lookup"><span data-stu-id="8be25-174">Find hello `<publishProfile >` element with `publishMethod="FTP"` that looks similar toothis:</span></span>
   
        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>

<span data-ttu-id="8be25-175">Poznamenejte si hello `publishUrl`, `userName`, a `userPWD` atributy.</span><span class="sxs-lookup"><span data-stu-id="8be25-175">Make note of hello `publishUrl`, `userName`, and `userPWD` attributes.</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="8be25-176">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="8be25-176">Publish your app</span></span>
<span data-ttu-id="8be25-177">Po testování vaší aplikace místně, můžete ho publikovat tooyour webové aplikace pomocí protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="8be25-177">After you have tested your app locally, you can publish it tooyour web app using FTP.</span></span> <span data-ttu-id="8be25-178">Nicméně je nutné nejprve tooupdate informací o připojení databáze hello v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="8be25-178">However, you first need tooupdate hello database connection information in hello application.</span></span> <span data-ttu-id="8be25-179">Pomocí informací připojení databáze hello jste dříve získali (v hello **získat MySQL a FTP informace o připojení** část), aktualizace hello následující informace v **i** hello `createdatabase.php` a `index.php` soubory s hello příslušné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="8be25-179">Using hello database connection information you obtained earlier (in hello **Get MySQL and FTP connection information** section), update hello following information in **both** hello `createdatabase.php` and `index.php` files with hello appropriate values:</span></span>

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

<span data-ttu-id="8be25-180">Nyní jste připraveni toopublish vaší aplikace pomocí protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="8be25-180">Now you are ready toopublish your app using FTP.</span></span>

1. <span data-ttu-id="8be25-181">Otevřete váš klient FTP podle volby.</span><span class="sxs-lookup"><span data-stu-id="8be25-181">Open your FTP client of choice.</span></span>
2. <span data-ttu-id="8be25-182">Zadejte hello *část názvu hostitele* z hello `publishUrl` atribut uvedených výše do vašeho klienta FTP.</span><span class="sxs-lookup"><span data-stu-id="8be25-182">Enter hello *host name portion* from hello `publishUrl` attribute you noted above into your FTP client.</span></span>
3. <span data-ttu-id="8be25-183">Zadejte hello `userName` a `userPWD` do vašeho klienta FTP beze změny atributů uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="8be25-183">Enter hello `userName` and `userPWD` attributes you noted above unchanged into your FTP client.</span></span>
4. <span data-ttu-id="8be25-184">Navažte připojení.</span><span class="sxs-lookup"><span data-stu-id="8be25-184">Establish a connection.</span></span>

<span data-ttu-id="8be25-185">Po připojení je bude možné tooupload a v případě potřeby stáhnout soubory.</span><span class="sxs-lookup"><span data-stu-id="8be25-185">After you have connected you will be able tooupload and download files as needed.</span></span> <span data-ttu-id="8be25-186">Ujistěte se, že nahráváte soubory toohello kořenový adresář, který je `/site/wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="8be25-186">Be sure that you are uploading files toohello root directory, which is `/site/wwwroot`.</span></span>

<span data-ttu-id="8be25-187">Po nahrání obě `index.php` a `createtable.php`, procházet příliš**http://[site name].azurewebsites.net/createtable.php** toocreate hello MySQL tabulky pro hello aplikaci a potom vyhledejte příliš**http://[site název ].azurewebsites.net/index.php** toobegin pomocí aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="8be25-187">After uploading both `index.php` and `createtable.php`, browse too**http://[site name].azurewebsites.net/createtable.php** toocreate hello MySQL table for hello application, then browse too**http://[site name].azurewebsites.net/index.php** toobegin using hello application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8be25-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8be25-188">Next steps</span></span>
<span data-ttu-id="8be25-189">Další informace najdete v tématu hello [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="8be25-189">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

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

