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
# <a name="create-a-php-sql-web-app-and-deploy-to-azure-app-service-using-git"></a><span data-ttu-id="93e42-103">Vytvoření webové aplikace PHP-SQL a nasazení do Azure App Service pomocí Git</span><span class="sxs-lookup"><span data-stu-id="93e42-103">Create a PHP-SQL web app and deploy to Azure App Service using Git</span></span>
<span data-ttu-id="93e42-104">V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci PHP v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) která se připojuje k databázi SQL Azure a jak se dá nasadit pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="93e42-104">This tutorial shows you how to create a PHP web app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) that connects to Azure SQL Database and how to deploy it using Git.</span></span> <span data-ttu-id="93e42-105">Tento kurz předpokládá, že máte [PHP][install-php], [SQL Server Express][install-SQLExpress], [Drivers společnosti Microsoft pro systém SQL Server pro jazyk PHP ](http://www.microsoft.com/download/en/details.aspx?id=20098), a [Git] [ install-git] v počítači nainstalována.</span><span class="sxs-lookup"><span data-stu-id="93e42-105">This tutorial assumes you have [PHP][install-php], [SQL Server Express][install-SQLExpress], the [Microsoft Drivers for SQL Server for PHP](http://www.microsoft.com/download/en/details.aspx?id=20098), and [Git][install-git] installed on your computer.</span></span> <span data-ttu-id="93e42-106">Po dokončení tohoto průvodce, budete mít webovou aplikaci PHP SQL běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="93e42-106">Upon completing this guide, you will have a PHP-SQL web app running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="93e42-107">Můžete nainstalovat a nakonfigurovat PHP, SQL Server Express a Drivers společnosti Microsoft pro systém SQL Server pro PHP pomocí [instalačního programu webové platformy Microsoft](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="93e42-107">You can install and configure PHP, SQL Server Express, and the Microsoft Drivers for SQL Server for PHP using the [Microsoft Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> 
> 

<span data-ttu-id="93e42-108">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="93e42-108">You will learn:</span></span>

* <span data-ttu-id="93e42-109">Postup vytvoření webové aplikace Azure a SQL Database pomocí [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715).</span><span class="sxs-lookup"><span data-stu-id="93e42-109">How to create an Azure web app and a SQL Database using the [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span></span> <span data-ttu-id="93e42-110">Protože PHP je ve výchozím nastavení povolená v App Service Web Apps, žádnou zvláštní akci je potřeba spustit kódu PHP.</span><span class="sxs-lookup"><span data-stu-id="93e42-110">Because PHP is enabled in App Service Web Apps by default, nothing special is required to run your PHP code.</span></span>
* <span data-ttu-id="93e42-111">Postup publikování a znovu publikovat svoji aplikaci do Azure pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="93e42-111">How to publish and re-publish your application to Azure using Git.</span></span>

<span data-ttu-id="93e42-112">V následujícím kurzu, bude vytvoření jednoduché registrace webové aplikace v jazyce PHP.</span><span class="sxs-lookup"><span data-stu-id="93e42-112">By following this tutorial, you will build a simple registration web application in PHP.</span></span> <span data-ttu-id="93e42-113">Aplikace se bude hostovat na webu Azure.</span><span class="sxs-lookup"><span data-stu-id="93e42-113">The application will be hosted in an Azure Website.</span></span> <span data-ttu-id="93e42-114">Zde je snímek obrazovky dokončené aplikace:</span><span class="sxs-lookup"><span data-stu-id="93e42-114">A screenshot of the completed application is below:</span></span>

![Webové stránky Azure PHP](./media/web-sites-php-sql-database-deploy-use-git/running_app_3.png)

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="93e42-116">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="93e42-116">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="93e42-117">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="93e42-117">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="create-an-azure-web-app-and-set-up-git-publishing"></a><span data-ttu-id="93e42-118">Vytvoření webové aplikace Azure a nastavení publikování Git</span><span class="sxs-lookup"><span data-stu-id="93e42-118">Create an Azure web app and set up Git publishing</span></span>
<span data-ttu-id="93e42-119">Postupujte podle těchto kroků můžete vytvořit webové aplikace Azure a SQL Database:</span><span class="sxs-lookup"><span data-stu-id="93e42-119">Follow these steps to create an Azure web app and a SQL Database:</span></span>

1. <span data-ttu-id="93e42-120">Přihlaste se k [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="93e42-120">Log in to the [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="93e42-121">Otevřít v Azure Marketplace kliknutím **nový** ikonu na horní pravé řídicí panel, klikněte na **Vybrat vše** vedle Marketplace a výběrem **Web + mobilní**.</span><span class="sxs-lookup"><span data-stu-id="93e42-121">Open the Azure Marketplace by clicking the **New** icon on the top left of the dashboard, click on **Select All** next to Marketplace and selecting **Web + Mobile**.</span></span>
3. <span data-ttu-id="93e42-122">Na webu Marketplace, vyberte **Web + mobilní**.</span><span class="sxs-lookup"><span data-stu-id="93e42-122">In the Marketplace, select **Web + Mobile**.</span></span>
4. <span data-ttu-id="93e42-123">Klikněte **Web app + SQL** ikonu.</span><span class="sxs-lookup"><span data-stu-id="93e42-123">Click the **Web app + SQL** icon.</span></span>
5. <span data-ttu-id="93e42-124">Po přečtení popis Web app + SQL aplikace, vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="93e42-124">After reading the description of the Web app + SQL app, select **Create**.</span></span>
6. <span data-ttu-id="93e42-125">Klikněte na každou část (**skupiny prostředků**, **webové aplikace**, **databáze**, a **předplatné**) a zadejte nebo vyberte hodnoty pro povinná pole :</span><span class="sxs-lookup"><span data-stu-id="93e42-125">Click on each part (**Resource Group**, **Web App**, **Database**, and **Subscription**) and enter or select values for the required fields:</span></span>
   
   * <span data-ttu-id="93e42-126">Zadejte název adresy URL podle svého výběru</span><span class="sxs-lookup"><span data-stu-id="93e42-126">Enter a URL name of your choice</span></span>    
   * <span data-ttu-id="93e42-127">Konfigurovat přihlašovací údaje serveru databáze</span><span class="sxs-lookup"><span data-stu-id="93e42-127">Configure database server credentials</span></span>
   * <span data-ttu-id="93e42-128">Vyberte oblast, která je nejblíže k vám</span><span class="sxs-lookup"><span data-stu-id="93e42-128">Select the region closest to you</span></span>
     
     ![Konfigurace aplikace](./media/web-sites-php-sql-database-deploy-use-git/configure-db-settings.png)
7. <span data-ttu-id="93e42-130">Po dokončení definování webové aplikace, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="93e42-130">When finished defining the web app, click **Create**.</span></span>
   
    <span data-ttu-id="93e42-131">Po vytvoření webové aplikace **oznámení** tlačítko bude flash zelená **úspěch** a otevřete okno skupiny prostředků k zobrazení webové aplikace a databáze SQL ve skupině.</span><span class="sxs-lookup"><span data-stu-id="93e42-131">When the web app has been created, the **Notifications** button will flash a green **SUCCESS** and the resource group blade open to show both the web app and the SQL database in the group.</span></span>
8. <span data-ttu-id="93e42-132">Klikněte na ikonu webové aplikace v okně skupiny prostředků, otevřete okno webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="93e42-132">Click the web app's icon in the resource group blade to open the web app's blade.</span></span>
   
    ![webové aplikace skupiny prostředků](./media/web-sites-php-sql-database-deploy-use-git/resource-group-blade.png)
9. <span data-ttu-id="93e42-134">V **nastavení** klikněte na tlačítko **průběžné nasazování** > **konfigurovat požadované nastavení**.</span><span class="sxs-lookup"><span data-stu-id="93e42-134">In **Settings** click **Continuous deployment** > **Configure required settings**.</span></span> <span data-ttu-id="93e42-135">Vyberte **místní úložiště Git** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="93e42-135">Select **Local Git Repository** and click **OK**.</span></span>
   
    ![kde je vašeho zdrojového kódu](./media/web-sites-php-sql-database-deploy-use-git/setup-local-git.png)
   
    <span data-ttu-id="93e42-137">Pokud jste nenastavili úložiště Git před, je nutné zadat uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="93e42-137">If you have not set up a Git repository before, you must provide a user name and password.</span></span> <span data-ttu-id="93e42-138">Chcete-li to provést, klikněte na tlačítko **nastavení** > **přihlašovací údaje nasazení** v okně webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="93e42-138">To do this, click **Settings** > **Deployment credentials** in the web app's blade.</span></span>
   
    ![](./media/web-sites-php-sql-database-deploy-use-git/deployment-credentials.png)
10. <span data-ttu-id="93e42-139">V **nastavení** klikněte na **vlastnosti** zobrazíte vzdálené adresy URL pro Git budete muset použít k nasazení své aplikace PHP později.</span><span class="sxs-lookup"><span data-stu-id="93e42-139">In **Settings** click on **Properties** to see the Git remote URL you need to use to deploy your PHP app later.</span></span>

## <a name="get-sql-database-connection-information"></a><span data-ttu-id="93e42-140">Získat informace o připojení k databázi SQL</span><span class="sxs-lookup"><span data-stu-id="93e42-140">Get SQL Database connection information</span></span>
<span data-ttu-id="93e42-141">Pro připojení k instanci databáze SQL, která je propojený s vaší webové aplikace, vaše bude potřebovat informace o připojení, který jste zadali při vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="93e42-141">To connect to the SQL Database instance that is linked to your web app, your will need the connection information, which you specified when you created the database.</span></span> <span data-ttu-id="93e42-142">Chcete-li získat informace o připojení databáze SQL, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="93e42-142">To get the SQL Database connection information, follow these steps:</span></span>

1. <span data-ttu-id="93e42-143">Zpět v okně skupiny prostředků klikněte na ikonu databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="93e42-143">Back in the resource group's blade, click the SQL database's icon.</span></span>
2. <span data-ttu-id="93e42-144">V okně databáze SQL, klikněte na tlačítko **nastavení** > **vlastnosti**, pak klikněte na tlačítko **zobrazit databázové připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="93e42-144">In the SQL database's blade, click **Settings** > **Properties**, then click **Show database connection strings**.</span></span> 
   
    ![Zobrazit vlastnosti databáze](./media/web-sites-php-sql-database-deploy-use-git/view-database-properties.png)
3. <span data-ttu-id="93e42-146">Z **PHP** části výsledné dialogového okna, poznamenejte si hodnoty pro `Server`, `SQL Database`, a `User Name`.</span><span class="sxs-lookup"><span data-stu-id="93e42-146">From the **PHP** section of the resulting dialog, make note of the values for `Server`, `SQL Database`, and `User Name`.</span></span> <span data-ttu-id="93e42-147">Tyto hodnoty budou pozdější použití při publikování webové aplikace PHP do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="93e42-147">You will use these values later when publishing your PHP web app to Azure App Service.</span></span>

## <a name="build-and-test-your-application-locally"></a><span data-ttu-id="93e42-148">Místní sestavení a testování aplikace</span><span class="sxs-lookup"><span data-stu-id="93e42-148">Build and test your application locally</span></span>
<span data-ttu-id="93e42-149">Registrace aplikace je jednoduchou aplikaci PHP, která umožňuje zaregistrovat pro událost tím, že poskytuje vaše jméno a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="93e42-149">The Registration application is a simple PHP application that allows you to register for an event by providing your name and email address.</span></span> <span data-ttu-id="93e42-150">Zobrazí se informace o předchozích rejstříkem v tabulce.</span><span class="sxs-lookup"><span data-stu-id="93e42-150">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="93e42-151">Registrační informace jsou uloženy v instanci databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="93e42-151">Registration information is stored in a SQL Database instance.</span></span> <span data-ttu-id="93e42-152">Aplikace se skládá ze dvou souborech (zkopírujte a vložte kód je k dispozici níže):</span><span class="sxs-lookup"><span data-stu-id="93e42-152">The application consists of two files (copy/paste code available below):</span></span>

* <span data-ttu-id="93e42-153">**index.php**: Zobrazí formulář pro registraci a tabulku obsahující informace osob žádajících o registraci.</span><span class="sxs-lookup"><span data-stu-id="93e42-153">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>
* <span data-ttu-id="93e42-154">**CreateTable.php**: vytvoří v tabulce databáze SQL pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="93e42-154">**createtable.php**: Creates the SQL Database table for the application.</span></span> <span data-ttu-id="93e42-155">Tento soubor se dají použít jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="93e42-155">This file will only be used once.</span></span>

<span data-ttu-id="93e42-156">Spusťte aplikaci místně, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="93e42-156">To run the application locally, follow the steps below.</span></span> <span data-ttu-id="93e42-157">Všimněte si, že tento postup předpokládá, máte PHP a SQL Server Express na místním počítači a pokud jste povolili [PDO rozšíření pro SQL Server][pdo-sqlsrv].</span><span class="sxs-lookup"><span data-stu-id="93e42-157">Note that these steps assume you have PHP and SQL Server Express set up on your local machine, and that you have enabled the [PDO extension for SQL Server][pdo-sqlsrv].</span></span>

1. <span data-ttu-id="93e42-158">Vytvořit databázi systému SQL Server volá `registration`.</span><span class="sxs-lookup"><span data-stu-id="93e42-158">Create a SQL Server database called `registration`.</span></span> <span data-ttu-id="93e42-159">Můžete to provést z `sqlcmd` příkazového řádku pomocí těchto příkazů:</span><span class="sxs-lookup"><span data-stu-id="93e42-159">You can do this from the `sqlcmd` command prompt with these commands:</span></span>
   
        >sqlcmd -S localhost\sqlexpress -U <local user name> -P <local password>
        1> create database registration
        2> GO    
2. <span data-ttu-id="93e42-160">V kořenovém adresáři aplikace, vytvořte dva soubory v ní – jednu s názvem `createtable.php` a jednu s názvem `index.php`.</span><span class="sxs-lookup"><span data-stu-id="93e42-160">In your application root directory, create two files in it - one called `createtable.php` and one called `index.php`.</span></span>
3. <span data-ttu-id="93e42-161">Otevřete `createtable.php` soubor v textovém editoru nebo IDE a přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="93e42-161">Open the `createtable.php` file in a text editor or IDE and add the code below.</span></span> <span data-ttu-id="93e42-162">Tento kód se použije k vytvoření `registration_tbl` tabulky v `registration` databáze.</span><span class="sxs-lookup"><span data-stu-id="93e42-162">This code will be used to create the `registration_tbl` table in the `registration` database.</span></span>
   
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
   
    <span data-ttu-id="93e42-163">Všimněte si, že budete muset aktualizovat hodnoty <code>$user</code> a <code>$pwd</code> s vaší místní systém SQL Server uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="93e42-163">Note that you will need to update the values for <code>$user</code> and <code>$pwd</code> with your local SQL Server user name and password.</span></span>
4. <span data-ttu-id="93e42-164">V terminálu v kořenovém adresáři aplikace zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="93e42-164">In a terminal at the root directory of the application type the following command:</span></span>
   
        php -S localhost:8000
5. <span data-ttu-id="93e42-165">Otevřete webový prohlížeč a přejděte do **http://localhost:8000/createtable.php**.</span><span class="sxs-lookup"><span data-stu-id="93e42-165">Open a web browser and browse to **http://localhost:8000/createtable.php**.</span></span> <span data-ttu-id="93e42-166">Tím se vytvoří `registration_tbl` tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="93e42-166">This will create the `registration_tbl` table in the database.</span></span>
6. <span data-ttu-id="93e42-167">Otevřete **index.php** soubor v textovém editoru nebo IDE a přidat základní HTML a CSS kódu stránky (v následujících krocích se přidají kód PHP.).</span><span class="sxs-lookup"><span data-stu-id="93e42-167">Open the **index.php** file in a text editor or IDE and add the basic HTML and CSS code for the page (the PHP code will be added in later steps).</span></span>
   
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
7. <span data-ttu-id="93e42-168">V rámci značky PHP přidejte kód PHP pro připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="93e42-168">Within the PHP tags, add PHP code for connecting to the database.</span></span>
   
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
   
    <span data-ttu-id="93e42-169">Znovu, budete muset aktualizovat hodnoty <code>$user</code> a <code>$pwd</code> s vaší místní MySQL uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="93e42-169">Again, you will need to update the values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
8. <span data-ttu-id="93e42-170">Následující kód připojení databáze přidáte kód pro vložení registrační informace do databáze.</span><span class="sxs-lookup"><span data-stu-id="93e42-170">Following the database connection code, add code for inserting registration information into the database.</span></span>
   
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
9. <span data-ttu-id="93e42-171">Nakonec následující kód výše, přidejte kód pro načítání dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="93e42-171">Finally, following the code above, add code for retrieving data from the database.</span></span>
   
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

<span data-ttu-id="93e42-172">Nyní můžete procházet k **http://localhost:8000/index.php** k testování aplikace.</span><span class="sxs-lookup"><span data-stu-id="93e42-172">You can now browse to **http://localhost:8000/index.php** to test the application.</span></span>

## <a name="publish-your-application"></a><span data-ttu-id="93e42-173">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="93e42-173">Publish your application</span></span>
<span data-ttu-id="93e42-174">Po testování vaší aplikace místně, můžete ho publikovat App Service Web Apps pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="93e42-174">After you have tested your application locally, you can publish it to App Service Web Apps using Git.</span></span> <span data-ttu-id="93e42-175">Však musíte nejprve aktualizovat informace o připojení databáze v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="93e42-175">However, you first need to update the database connection information in the application.</span></span> <span data-ttu-id="93e42-176">Pomocí informací o připojení databáze jste dříve získali (v **informace o připojení k databázi SQL získat** část), aktualizujte následující informace v **i** `createdatabase.php` a `index.php` soubory s příslušnými hodnotami:</span><span class="sxs-lookup"><span data-stu-id="93e42-176">Using the database connection information you obtained earlier (in the **Get SQL Database connection information** section), update the following information in **both** the `createdatabase.php` and `index.php` files with the appropriate values:</span></span>

    // DB connection info
    $host = "tcp:<value of Server>";
    $user = "<value of User Name>";
    $pwd = "<your password>";
    $db = "<value of SQL Database>";

> [!NOTE]
> <span data-ttu-id="93e42-177">V <code>$host</code>, musí být předponou hodnotu serveru <code>tcp:</code>.</span><span class="sxs-lookup"><span data-stu-id="93e42-177">In the <code>$host</code>, the value of Server must be prepended with <code>tcp:</code>.</span></span>
> 
> 

<span data-ttu-id="93e42-178">Nyní jste připraveni k nastavení publikování Git a publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="93e42-178">Now, you are ready to set up Git publishing and publish the application.</span></span>

> [!NOTE]
> <span data-ttu-id="93e42-179">Jedná se o tytéž kroky uvedené na konci **vytvoření webové aplikace Azure a nastavení publikování Git** část výše.</span><span class="sxs-lookup"><span data-stu-id="93e42-179">These are the same steps noted at the end of the **Create an Azure web app and set up Git publishing** section above.</span></span>
> 
> 

1. <span data-ttu-id="93e42-180">Otevřete Git Bash (nebo terminál, pokud Git je ve vaší `PATH`), změňte adresáře na kořenový adresář aplikace ( **registrace** directory), a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="93e42-180">Open GitBash (or a terminal, if Git is in your `PATH`), change directories to the root directory of your application (the **registration** directory), and run the following commands:</span></span>
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    <span data-ttu-id="93e42-181">Zobrazí se výzva k zadání hesla, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="93e42-181">You will be prompted for the password you created earlier.</span></span>
2. <span data-ttu-id="93e42-182">Přejděte do **http://[web aplikace name].azurewebsites.net/createtable.php** k vytvoření tabulky databáze SQL pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="93e42-182">Browse to **http://[web app name].azurewebsites.net/createtable.php** to create the SQL database table for the application.</span></span>
3. <span data-ttu-id="93e42-183">Přejděte do **http://[web aplikace name].azurewebsites.net/index.php** začít používat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="93e42-183">Browse to **http://[web app name].azurewebsites.net/index.php** to begin using the application.</span></span>

<span data-ttu-id="93e42-184">Po publikování aplikace, můžete začít, provedení změn a publikovat je pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="93e42-184">After you have published your application, you can begin making changes to it and use Git to publish them.</span></span> 

## <a name="publish-changes-to-your-application"></a><span data-ttu-id="93e42-185">Publikování změn do aplikace</span><span class="sxs-lookup"><span data-stu-id="93e42-185">Publish changes to your application</span></span>
<span data-ttu-id="93e42-186">Při publikování změn do aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="93e42-186">To publish changes to application, follow these steps:</span></span>

1. <span data-ttu-id="93e42-187">Měnit svou aplikaci lokálně.</span><span class="sxs-lookup"><span data-stu-id="93e42-187">Make changes to your application locally.</span></span>
2. <span data-ttu-id="93e42-188">Otevřete Git Bash (nebo terminál, it Git je ve vaší `PATH`), změňte adresáře na kořenový adresář aplikace a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="93e42-188">Open GitBash (or a terminal, it Git is in your `PATH`), change directories to the root directory of your application, and run the following commands:</span></span>
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    <span data-ttu-id="93e42-189">Zobrazí se výzva k zadání hesla, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="93e42-189">You will be prompted for the password you created earlier.</span></span>
3. <span data-ttu-id="93e42-190">Přejděte do **http://[web aplikace name].azurewebsites.net/index.php** vaše změny zobrazily.</span><span class="sxs-lookup"><span data-stu-id="93e42-190">Browse to **http://[web app name].azurewebsites.net/index.php** to see your changes.</span></span>

## <a name="whats-changed"></a><span data-ttu-id="93e42-191">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="93e42-191">What's changed</span></span>
* <span data-ttu-id="93e42-192">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="93e42-192">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[pdo-sqlsrv]: http://php.net/pdo_sqlsrv

