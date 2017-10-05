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
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a><span data-ttu-id="5586d-103">Vytvoření webové aplikace v PHP-MySQL ve službě Azure App Service a nasazení přes FTP</span><span class="sxs-lookup"><span data-stu-id="5586d-103">Create a PHP-MySQL web app in Azure App Service and deploy using FTP</span></span>
<span data-ttu-id="5586d-104">Tento kurz ukazuje, jak vytvořit webovou aplikaci PHP MySQL a jak se dá nasadit pomocí protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="5586d-104">This tutorial shows you how to create a PHP-MySQL web app and how to deploy it using FTP.</span></span> <span data-ttu-id="5586d-105">Tento kurz předpokládá, že máte [PHP][install-php], [MySQL][install-mysql], webového serveru a klienta v počítači nainstalována.</span><span class="sxs-lookup"><span data-stu-id="5586d-105">This tutorial assumes you have [PHP][install-php], [MySQL][install-mysql], a web server, and an FTP client installed on your computer.</span></span> <span data-ttu-id="5586d-106">Pokyny v tomto kurzu platí pro všechny operační systémy, včetně systému Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="5586d-106">The instructions in this tutorial can be followed on any operating system, including Windows, Mac, and  Linux.</span></span> <span data-ttu-id="5586d-107">Po dokončení tohoto průvodce, budete mít webovou aplikaci PHP nebo MySQL běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="5586d-107">Upon completing this guide, you will have a PHP/MySQL web app running in Azure.</span></span>

<span data-ttu-id="5586d-108">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="5586d-108">You will learn:</span></span>

* <span data-ttu-id="5586d-109">Jak vytvořit webovou aplikaci a databázi MySQL pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5586d-109">How to create a web app and a MySQL database using the Azure Portal.</span></span> <span data-ttu-id="5586d-110">Vzhledem k PHP je ve výchozím nastavení povolena ve službě Web Apps, nic speciální je vyžadovaná pro spouštění vašeho kódu PHP.</span><span class="sxs-lookup"><span data-stu-id="5586d-110">Because PHP is enabled in Web Apps by default, nothing special is required to run your PHP code.</span></span>
* <span data-ttu-id="5586d-111">Postup publikování aplikace do Azure pomocí protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="5586d-111">How to publish your application to Azure using FTP.</span></span>

<span data-ttu-id="5586d-112">Podle tohoto kurzu vytvoříte registrace jednoduché webové aplikace v jazyce PHP.</span><span class="sxs-lookup"><span data-stu-id="5586d-112">By following this tutorial, you will build a simple registration web app in PHP.</span></span> <span data-ttu-id="5586d-113">Aplikace bude hostován ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5586d-113">The application will be hosted in a Web App.</span></span> <span data-ttu-id="5586d-114">Zde je snímek obrazovky dokončené aplikace:</span><span class="sxs-lookup"><span data-stu-id="5586d-114">A screenshot of the completed application is below:</span></span>

![Webové stránky Azure PHP][running-app]

> [!NOTE]
> <span data-ttu-id="5586d-116">Pokud chcete začít používat Azure App Service před registrací k účtu, přejděte na [vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="5586d-116">If you want to get started with Azure App Service before signing up for an account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="5586d-117">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="5586d-117">No credit cards required, no commitments.</span></span> 
> 
> 

## <a name="create-a-web-app-and-set-up-ftp-publishing"></a><span data-ttu-id="5586d-118">Vytvoření webové aplikace a nastavení publikování FTP</span><span class="sxs-lookup"><span data-stu-id="5586d-118">Create a web app and set up FTP publishing</span></span>
<span data-ttu-id="5586d-119">Postupujte podle těchto kroků můžete vytvořit webovou aplikaci a databáze MySQL:</span><span class="sxs-lookup"><span data-stu-id="5586d-119">Follow these steps to create a web app and a MySQL database:</span></span>

1. <span data-ttu-id="5586d-120">Přihlášení k [portál Azure][management-portal].</span><span class="sxs-lookup"><span data-stu-id="5586d-120">Login to the [Azure Portal][management-portal].</span></span>
2. <span data-ttu-id="5586d-121">Klikněte **+ nový** ikona nahoře vlevo na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5586d-121">Click the **+ New** icon on the top left of the Azure Portal.</span></span>
   
    ![Vytvořit nový web Azure][new-website]
3. <span data-ttu-id="5586d-123">V typu vyhledávání **Web app + MySQL** a klikněte na **Web app + MySQL**.</span><span class="sxs-lookup"><span data-stu-id="5586d-123">In the search type **Web app + MySQL** and click on **Web app + MySQL**.</span></span>
   
    ![Vlastní vytvořit novou webovou stránku][custom-create]
4. <span data-ttu-id="5586d-125">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5586d-125">Click **Create**.</span></span> <span data-ttu-id="5586d-126">Zadejte název jedinečné app service, platný název pro skupinu prostředků a nový plán služby.</span><span class="sxs-lookup"><span data-stu-id="5586d-126">Enter a unique app service name, a valid name for the resource group and a new service plan.</span></span>
   
    ![Název skupiny prostředků sady][resource-group]
5. <span data-ttu-id="5586d-128">Zadejte hodnoty pro novou databázi, včetně, kterým Odsouhlasíte právní podmínky.</span><span class="sxs-lookup"><span data-stu-id="5586d-128">Enter values for your new database, including agreeing to the legal terms.</span></span>
   
    ![Vytvořit novou databázi MySQL][new-mysql-db]
6. <span data-ttu-id="5586d-130">Po vytvoření webové aplikace, zobrazí se nové okno aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="5586d-130">When the web app has been created, you will see the new app service blade.</span></span>
7. <span data-ttu-id="5586d-131">Klikněte na **nastavení** > **přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="5586d-131">Click on **Settings** > **Deployment credentials**.</span></span> 
   
    ![Nastavení přihlašovacích údajů nasazení][set-deployment-credentials]
8. <span data-ttu-id="5586d-133">Pokud chcete povolit publikování FTP, zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="5586d-133">To enable FTP publishing, you must provide a user name and password.</span></span> <span data-ttu-id="5586d-134">Uložení přihlašovacích údajů a poznamenejte si uživatelské jméno a heslo, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="5586d-134">Save the credentials and make a note of the user name and password you create.</span></span>
   
    ![Vytvořit přihlašovací údaje pro publikování][portal-ftp-username-password]

## <a name="build-and-test-your-app-locally"></a><span data-ttu-id="5586d-136">Vytvoření a testování vaší aplikace místně</span><span class="sxs-lookup"><span data-stu-id="5586d-136">Build and test your app locally</span></span>
<span data-ttu-id="5586d-137">Registrace aplikace je jednoduchou aplikaci PHP, která umožňuje zaregistrovat pro událost tím, že poskytuje vaše jméno a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="5586d-137">The Registration application is a simple PHP application that allows you to register for an event by providing your name and email address.</span></span> <span data-ttu-id="5586d-138">Zobrazí se informace o předchozích rejstříkem v tabulce.</span><span class="sxs-lookup"><span data-stu-id="5586d-138">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="5586d-139">Registrační informace jsou uloženy v databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="5586d-139">Registration information is stored in a MySQL database.</span></span> <span data-ttu-id="5586d-140">Aplikace se skládá z dva soubory:</span><span class="sxs-lookup"><span data-stu-id="5586d-140">The app consists of two files:</span></span>

* <span data-ttu-id="5586d-141">**index.php**: Zobrazí formulář pro registraci a tabulku obsahující informace osob žádajících o registraci.</span><span class="sxs-lookup"><span data-stu-id="5586d-141">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>
* <span data-ttu-id="5586d-142">**CreateTable.php**: vytvoří tabulku MySQL pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5586d-142">**createtable.php**: Creates the MySQL table for the application.</span></span> <span data-ttu-id="5586d-143">Tento soubor se dají použít jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="5586d-143">This file will only be used once.</span></span>

<span data-ttu-id="5586d-144">Sestavení a spuštění aplikace místně, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="5586d-144">To build and run the app locally, follow the steps below.</span></span> <span data-ttu-id="5586d-145">Všimněte si, že tento postup předpokládá, máte PHP, MySQL a webový server nastavit na místním počítači a pokud jste povolili [PDO rozšíření pro databázi MySQL][pdo-mysql].</span><span class="sxs-lookup"><span data-stu-id="5586d-145">Note that these steps assume you have PHP, MySQL, and a web server set up on your local machine, and that you have enabled the [PDO extension for MySQL][pdo-mysql].</span></span>

1. <span data-ttu-id="5586d-146">Vytvoření databáze MySQL názvem `registration`.</span><span class="sxs-lookup"><span data-stu-id="5586d-146">Create a MySQL database called `registration`.</span></span> <span data-ttu-id="5586d-147">Můžete to udělat z příkazového řádku MySQL pomocí tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="5586d-147">You can do this from the MySQL command prompt with this command:</span></span>
   
        mysql> create database registration;
2. <span data-ttu-id="5586d-148">V kořenovém adresáři váš webový server, vytvořte složku s názvem `registration` a vytvořit dva soubory v ní – jednu s názvem `createtable.php` a jednu s názvem `index.php`.</span><span class="sxs-lookup"><span data-stu-id="5586d-148">In your web server's root directory, create a folder called `registration` and create two files in it - one called `createtable.php` and one called `index.php`.</span></span>
3. <span data-ttu-id="5586d-149">Otevřete `createtable.php` soubor v textovém editoru nebo IDE a přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="5586d-149">Open the `createtable.php` file in a text editor or IDE and add the code below.</span></span> <span data-ttu-id="5586d-150">Tento kód se použije k vytvoření `registration_tbl` tabulky v `registration` databáze.</span><span class="sxs-lookup"><span data-stu-id="5586d-150">This code will be used to create the `registration_tbl` table in the `registration` database.</span></span>
   
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
   > <span data-ttu-id="5586d-151">Budete muset aktualizovat hodnoty <code>$user</code> a <code>$pwd</code> s vaší místní MySQL uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="5586d-151">You will need to update the values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
   > 
   > 
4. <span data-ttu-id="5586d-152">Otevřete webový prohlížeč a přejděte do [http://localhost/registration/createtable.php][localhost-createtable].</span><span class="sxs-lookup"><span data-stu-id="5586d-152">Open a web browser and browse to [http://localhost/registration/createtable.php][localhost-createtable].</span></span> <span data-ttu-id="5586d-153">Tím se vytvoří `registration_tbl` tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="5586d-153">This will create the `registration_tbl` table in the database.</span></span>
5. <span data-ttu-id="5586d-154">Otevřete **index.php** soubor v textovém editoru nebo IDE a přidat základní HTML a CSS kódu stránky (v následujících krocích se přidají kód PHP.).</span><span class="sxs-lookup"><span data-stu-id="5586d-154">Open the **index.php** file in a text editor or IDE and add the basic HTML and CSS code for the page (the PHP code will be added in later steps).</span></span>
   
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
6. <span data-ttu-id="5586d-155">V rámci značky PHP přidejte kód PHP pro připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="5586d-155">Within the PHP tags, add PHP code for connecting to the database.</span></span>
   
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
   > <span data-ttu-id="5586d-156">Znovu, budete muset aktualizovat hodnoty <code>$user</code> a <code>$pwd</code> s vaší místní MySQL uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="5586d-156">Again, you will need to update the values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
   > 
   > 
7. <span data-ttu-id="5586d-157">Následující kód připojení databáze přidáte kód pro vložení registrační informace do databáze.</span><span class="sxs-lookup"><span data-stu-id="5586d-157">Following the database connection code, add code for inserting registration information into the database.</span></span>
   
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
8. <span data-ttu-id="5586d-158">Nakonec následující kód výše, přidejte kód pro načítání dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="5586d-158">Finally, following the code above, add code for retrieving data from the database.</span></span>
   
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

<span data-ttu-id="5586d-159">Nyní můžete procházet k [http://localhost/registration/index.php] [ localhost-index] k testování aplikace.</span><span class="sxs-lookup"><span data-stu-id="5586d-159">You can now browse to [http://localhost/registration/index.php][localhost-index] to test the app.</span></span>

## <a name="get-mysql-and-ftp-connection-information"></a><span data-ttu-id="5586d-160">Získat informace o připojení databáze MySQL a FTP</span><span class="sxs-lookup"><span data-stu-id="5586d-160">Get MySQL and FTP connection information</span></span>
<span data-ttu-id="5586d-161">Pro připojení k databázi MySQL, která běží ve službě Web Apps, vaše bude potřebovat informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="5586d-161">To connect to the MySQL database that is running in Web Apps, your will need the connection information.</span></span> <span data-ttu-id="5586d-162">Chcete-li získat informace o připojení databáze MySQL, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5586d-162">To get MySQL connection information, follow these steps:</span></span>

1. <span data-ttu-id="5586d-163">Ze služby app service okně webové aplikace klikněte na odkaz skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="5586d-163">From the app service web app blade click on the resource group link:</span></span>
   
    ![Vyberte skupinu prostředků][select-resourcegroup]
2. <span data-ttu-id="5586d-165">Od vaší skupiny prostředků klikněte na databázi:</span><span class="sxs-lookup"><span data-stu-id="5586d-165">From your resource group, click the database:</span></span>
   
    ![Vyberte databázi][select-database]
3. <span data-ttu-id="5586d-167">Z databáze souhrn, vyberte **nastavení** > **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="5586d-167">From the database summary, select **Settings** > **Properties**.</span></span>
   
    ![Výběr vlastností][select-properties]
4. <span data-ttu-id="5586d-169">Poznamenejte si hodnoty pro `Database`, `Host`, `User Id`, a `Password`.</span><span class="sxs-lookup"><span data-stu-id="5586d-169">Make note of the values for `Database`, `Host`, `User Id`, and `Password`.</span></span>
   
    ![Poznámka: vlastnosti][note-properties]
5. <span data-ttu-id="5586d-171">Z vaší webové aplikace, klikněte **stažení profilu publikování** odkaz v pravém dolním rohu stránky:</span><span class="sxs-lookup"><span data-stu-id="5586d-171">From your web app, click the **Download publish profile** link at the bottom right corner of the page:</span></span>
   
    ![Stažení profilu publikování][download-publish-profile]
6. <span data-ttu-id="5586d-173">Otevřete `.publishsettings` souboru v editoru XML.</span><span class="sxs-lookup"><span data-stu-id="5586d-173">Open the `.publishsettings` file in an XML editor.</span></span> 
7. <span data-ttu-id="5586d-174">Najít `<publishProfile >` element s `publishMethod="FTP"` který vypadá podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="5586d-174">Find the `<publishProfile >` element with `publishMethod="FTP"` that looks similar to this:</span></span>
   
        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>

<span data-ttu-id="5586d-175">Poznamenejte si `publishUrl`, `userName`, a `userPWD` atributy.</span><span class="sxs-lookup"><span data-stu-id="5586d-175">Make note of the `publishUrl`, `userName`, and `userPWD` attributes.</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="5586d-176">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="5586d-176">Publish your app</span></span>
<span data-ttu-id="5586d-177">Po testování vaší aplikace místně, můžete ji publikujete do webové aplikace pomocí protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="5586d-177">After you have tested your app locally, you can publish it to your web app using FTP.</span></span> <span data-ttu-id="5586d-178">Však musíte nejprve aktualizovat informace o připojení databáze v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5586d-178">However, you first need to update the database connection information in the application.</span></span> <span data-ttu-id="5586d-179">Pomocí informací o připojení databáze jste dříve získali (v **získat MySQL a FTP informace o připojení** část), aktualizujte následující informace v **i** `createdatabase.php` a `index.php` soubory s příslušnými hodnotami:</span><span class="sxs-lookup"><span data-stu-id="5586d-179">Using the database connection information you obtained earlier (in the **Get MySQL and FTP connection information** section), update the following information in **both** the `createdatabase.php` and `index.php` files with the appropriate values:</span></span>

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

<span data-ttu-id="5586d-180">Nyní jste připraveni k publikování aplikace pomocí protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="5586d-180">Now you are ready to publish your app using FTP.</span></span>

1. <span data-ttu-id="5586d-181">Otevřete váš klient FTP podle volby.</span><span class="sxs-lookup"><span data-stu-id="5586d-181">Open your FTP client of choice.</span></span>
2. <span data-ttu-id="5586d-182">Zadejte *část názvu hostitele* z `publishUrl` atribut uvedených výše do vašeho klienta FTP.</span><span class="sxs-lookup"><span data-stu-id="5586d-182">Enter the *host name portion* from the `publishUrl` attribute you noted above into your FTP client.</span></span>
3. <span data-ttu-id="5586d-183">Zadejte `userName` a `userPWD` do vašeho klienta FTP beze změny atributů uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="5586d-183">Enter the `userName` and `userPWD` attributes you noted above unchanged into your FTP client.</span></span>
4. <span data-ttu-id="5586d-184">Navažte připojení.</span><span class="sxs-lookup"><span data-stu-id="5586d-184">Establish a connection.</span></span>

<span data-ttu-id="5586d-185">Po připojení bude moci nahrávání a stahování souborů podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5586d-185">After you have connected you will be able to upload and download files as needed.</span></span> <span data-ttu-id="5586d-186">Ujistěte se, že jsou nahrávání souborů do kořenového adresáře, který je `/site/wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="5586d-186">Be sure that you are uploading files to the root directory, which is `/site/wwwroot`.</span></span>

<span data-ttu-id="5586d-187">Po nahrání obě `index.php` a `createtable.php`, přejděte do **http://[site name].azurewebsites.net/createtable.php** k vytvoření databáze MySQL tabulky pro aplikaci, pak přejděte do **http://[site name]. azurewebsites.NET/index.php** začít používat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5586d-187">After uploading both `index.php` and `createtable.php`, browse to **http://[site name].azurewebsites.net/createtable.php** to create the MySQL table for the application, then browse to **http://[site name].azurewebsites.net/index.php** to begin using the application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5586d-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5586d-188">Next steps</span></span>
<span data-ttu-id="5586d-189">Další informace najdete v tématu [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="5586d-189">For more information, see the [PHP Developer Center](/develop/php/).</span></span>

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

