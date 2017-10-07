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
# <a name="create-a-php-sql-web-app-and-deploy-tooazure-app-service-using-git"></a><span data-ttu-id="486c3-103">Vytvoření webové aplikace PHP-SQL a nasazení tooAzure služby App Service pomocí Git</span><span class="sxs-lookup"><span data-stu-id="486c3-103">Create a PHP-SQL web app and deploy tooAzure App Service using Git</span></span>
<span data-ttu-id="486c3-104">Tento kurz ukazuje, jak toocreate PHP webové aplikace v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) která se připojuje tooAzure databáze SQL a jak toodeploy pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="486c3-104">This tutorial shows you how toocreate a PHP web app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) that connects tooAzure SQL Database and how toodeploy it using Git.</span></span> <span data-ttu-id="486c3-105">Tento kurz předpokládá, že máte [PHP][install-php], [SQL Server Express][install-SQLExpress], hello [Drivers společnosti Microsoft pro systém SQL Server pro jazyk PHP ](http://www.microsoft.com/download/en/details.aspx?id=20098), a [Git] [ install-git] v počítači nainstalována.</span><span class="sxs-lookup"><span data-stu-id="486c3-105">This tutorial assumes you have [PHP][install-php], [SQL Server Express][install-SQLExpress], hello [Microsoft Drivers for SQL Server for PHP](http://www.microsoft.com/download/en/details.aspx?id=20098), and [Git][install-git] installed on your computer.</span></span> <span data-ttu-id="486c3-106">Po dokončení tohoto průvodce, budete mít webovou aplikaci PHP SQL běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="486c3-106">Upon completing this guide, you will have a PHP-SQL web app running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="486c3-107">Můžete nainstalovat a nakonfigurovat PHP, SQL Server Express a hello Drivers společnosti Microsoft pro systém SQL Server pro jazyk PHP pomocí hello [instalačního programu webové platformy Microsoft](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="486c3-107">You can install and configure PHP, SQL Server Express, and hello Microsoft Drivers for SQL Server for PHP using hello [Microsoft Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> 
> 

<span data-ttu-id="486c3-108">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="486c3-108">You will learn:</span></span>

* <span data-ttu-id="486c3-109">Jak toocreate Azure webové aplikace a SQL Database pomocí hello [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715).</span><span class="sxs-lookup"><span data-stu-id="486c3-109">How toocreate an Azure web app and a SQL Database using hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span></span> <span data-ttu-id="486c3-110">Protože PHP je ve výchozím nastavení povolená v App Service Web Apps, nic speciální je požadovaná toorun kódu PHP.</span><span class="sxs-lookup"><span data-stu-id="486c3-110">Because PHP is enabled in App Service Web Apps by default, nothing special is required toorun your PHP code.</span></span>
* <span data-ttu-id="486c3-111">Jak toopublish a znovu publikovat tooAzure vaší aplikace pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="486c3-111">How toopublish and re-publish your application tooAzure using Git.</span></span>

<span data-ttu-id="486c3-112">V následujícím kurzu, bude vytvoření jednoduché registrace webové aplikace v jazyce PHP.</span><span class="sxs-lookup"><span data-stu-id="486c3-112">By following this tutorial, you will build a simple registration web application in PHP.</span></span> <span data-ttu-id="486c3-113">aplikace Hello se bude hostovat na webu Azure.</span><span class="sxs-lookup"><span data-stu-id="486c3-113">hello application will be hosted in an Azure Website.</span></span> <span data-ttu-id="486c3-114">Zde je snímek obrazovky aplikace hello dokončena:</span><span class="sxs-lookup"><span data-stu-id="486c3-114">A screenshot of hello completed application is below:</span></span>

![Webové stránky Azure PHP](./media/web-sites-php-sql-database-deploy-use-git/running_app_3.png)

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="486c3-116">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="486c3-116">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="486c3-117">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="486c3-117">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="create-an-azure-web-app-and-set-up-git-publishing"></a><span data-ttu-id="486c3-118">Vytvoření webové aplikace Azure a nastavení publikování Git</span><span class="sxs-lookup"><span data-stu-id="486c3-118">Create an Azure web app and set up Git publishing</span></span>
<span data-ttu-id="486c3-119">Postupujte podle těchto kroků toocreate webové aplikace Azure a SQL Database:</span><span class="sxs-lookup"><span data-stu-id="486c3-119">Follow these steps toocreate an Azure web app and a SQL Database:</span></span>

1. <span data-ttu-id="486c3-120">Přihlaste se toohello [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="486c3-120">Log in toohello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="486c3-121">Otevřete hello Azure Marketplace kliknutím hello **nový** ikonu hello nahoře vlevo hello řídicí panel, klikněte na **Vybrat vše** další tooMarketplace a výběrem **Web + mobilní**.</span><span class="sxs-lookup"><span data-stu-id="486c3-121">Open hello Azure Marketplace by clicking hello **New** icon on hello top left of hello dashboard, click on **Select All** next tooMarketplace and selecting **Web + Mobile**.</span></span>
3. <span data-ttu-id="486c3-122">V hello Marketplace, vyberte **Web + mobilní**.</span><span class="sxs-lookup"><span data-stu-id="486c3-122">In hello Marketplace, select **Web + Mobile**.</span></span>
4. <span data-ttu-id="486c3-123">Klikněte na tlačítko hello **Web app + SQL** ikonu.</span><span class="sxs-lookup"><span data-stu-id="486c3-123">Click hello **Web app + SQL** icon.</span></span>
5. <span data-ttu-id="486c3-124">Po přečtení hello popis hello Web app + SQL aplikace, vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="486c3-124">After reading hello description of hello Web app + SQL app, select **Create**.</span></span>
6. <span data-ttu-id="486c3-125">Klikněte na každou část (**skupiny prostředků**, **webové aplikace**, **databáze**, a **předplatné**) a zadejte nebo vyberte hodnoty pro požadované hello pole:</span><span class="sxs-lookup"><span data-stu-id="486c3-125">Click on each part (**Resource Group**, **Web App**, **Database**, and **Subscription**) and enter or select values for hello required fields:</span></span>
   
   * <span data-ttu-id="486c3-126">Zadejte název adresy URL podle svého výběru</span><span class="sxs-lookup"><span data-stu-id="486c3-126">Enter a URL name of your choice</span></span>    
   * <span data-ttu-id="486c3-127">Konfigurovat přihlašovací údaje serveru databáze</span><span class="sxs-lookup"><span data-stu-id="486c3-127">Configure database server credentials</span></span>
   * <span data-ttu-id="486c3-128">Vyberte nejbližší tooyou oblast hello</span><span class="sxs-lookup"><span data-stu-id="486c3-128">Select hello region closest tooyou</span></span>
     
     ![Konfigurace aplikace](./media/web-sites-php-sql-database-deploy-use-git/configure-db-settings.png)
7. <span data-ttu-id="486c3-130">Po dokončení definování hello webové aplikace, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="486c3-130">When finished defining hello web app, click **Create**.</span></span>
   
    <span data-ttu-id="486c3-131">Po vytvoření webové aplikace hello hello **oznámení** tlačítko bude flash zelená **úspěch** a hello prostředků skupiny okno otevřít tooshow obou hello webové aplikace a hello databáze SQL ve skupině hello.</span><span class="sxs-lookup"><span data-stu-id="486c3-131">When hello web app has been created, hello **Notifications** button will flash a green **SUCCESS** and hello resource group blade open tooshow both hello web app and hello SQL database in hello group.</span></span>
8. <span data-ttu-id="486c3-132">Klikněte na ikonu hello webové aplikace v okně hello prostředků skupiny okno tooopen hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="486c3-132">Click hello web app's icon in hello resource group blade tooopen hello web app's blade.</span></span>
   
    ![webové aplikace skupiny prostředků](./media/web-sites-php-sql-database-deploy-use-git/resource-group-blade.png)
9. <span data-ttu-id="486c3-134">V **nastavení** klikněte na tlačítko **průběžné nasazování** > **konfigurovat požadované nastavení**.</span><span class="sxs-lookup"><span data-stu-id="486c3-134">In **Settings** click **Continuous deployment** > **Configure required settings**.</span></span> <span data-ttu-id="486c3-135">Vyberte **místní úložiště Git** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="486c3-135">Select **Local Git Repository** and click **OK**.</span></span>
   
    ![kde je vašeho zdrojového kódu](./media/web-sites-php-sql-database-deploy-use-git/setup-local-git.png)
   
    <span data-ttu-id="486c3-137">Pokud jste nenastavili úložiště Git před, je nutné zadat uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="486c3-137">If you have not set up a Git repository before, you must provide a user name and password.</span></span> <span data-ttu-id="486c3-138">toodo tento, klikněte na tlačítko **nastavení** > **přihlašovací údaje nasazení** v okně hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="486c3-138">toodo this, click **Settings** > **Deployment credentials** in hello web app's blade.</span></span>
   
    ![](./media/web-sites-php-sql-database-deploy-use-git/deployment-credentials.png)
10. <span data-ttu-id="486c3-139">V **nastavení** klikněte na **vlastnosti** toosee hello vzdálené adresy URL pro Git budete potřebovat toouse toodeploy aplikaci PHP později.</span><span class="sxs-lookup"><span data-stu-id="486c3-139">In **Settings** click on **Properties** toosee hello Git remote URL you need toouse toodeploy your PHP app later.</span></span>

## <a name="get-sql-database-connection-information"></a><span data-ttu-id="486c3-140">Získat informace o připojení k databázi SQL</span><span class="sxs-lookup"><span data-stu-id="486c3-140">Get SQL Database connection information</span></span>
<span data-ttu-id="486c3-141">instance databáze SQL toohello tooconnect, který je propojený tooyour webové aplikace, vaše bude potřebovat hello informace o připojení, který jste zadali při vytváření databáze hello.</span><span class="sxs-lookup"><span data-stu-id="486c3-141">tooconnect toohello SQL Database instance that is linked tooyour web app, your will need hello connection information, which you specified when you created hello database.</span></span> <span data-ttu-id="486c3-142">hello tooget informace o připojení databáze SQL, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="486c3-142">tooget hello SQL Database connection information, follow these steps:</span></span>

1. <span data-ttu-id="486c3-143">Zpět v okně skupiny prostředků hello klikněte na ikonu hello SQL database.</span><span class="sxs-lookup"><span data-stu-id="486c3-143">Back in hello resource group's blade, click hello SQL database's icon.</span></span>
2. <span data-ttu-id="486c3-144">V okně databáze SQL hello, klikněte na tlačítko **nastavení** > **vlastnosti**, pak klikněte na tlačítko **zobrazit databázové připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="486c3-144">In hello SQL database's blade, click **Settings** > **Properties**, then click **Show database connection strings**.</span></span> 
   
    ![Zobrazit vlastnosti databáze](./media/web-sites-php-sql-database-deploy-use-git/view-database-properties.png)
3. <span data-ttu-id="486c3-146">Z hello **PHP** části hello výsledné dialogového okna, poznamenejte si hodnoty hello `Server`, `SQL Database`, a `User Name`.</span><span class="sxs-lookup"><span data-stu-id="486c3-146">From hello **PHP** section of hello resulting dialog, make note of hello values for `Server`, `SQL Database`, and `User Name`.</span></span> <span data-ttu-id="486c3-147">Budete používat tyto hodnoty později při publikování vašeho PHP webové aplikace tooAzure služby App Service.</span><span class="sxs-lookup"><span data-stu-id="486c3-147">You will use these values later when publishing your PHP web app tooAzure App Service.</span></span>

## <a name="build-and-test-your-application-locally"></a><span data-ttu-id="486c3-148">Místní sestavení a testování aplikace</span><span class="sxs-lookup"><span data-stu-id="486c3-148">Build and test your application locally</span></span>
<span data-ttu-id="486c3-149">Registrace aplikace Hello je jednoduchou aplikaci PHP, která vám umožní tooregister pro událost tím, že poskytuje vaše jméno a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="486c3-149">hello Registration application is a simple PHP application that allows you tooregister for an event by providing your name and email address.</span></span> <span data-ttu-id="486c3-150">Zobrazí se informace o předchozích rejstříkem v tabulce.</span><span class="sxs-lookup"><span data-stu-id="486c3-150">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="486c3-151">Registrační informace jsou uloženy v instanci databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="486c3-151">Registration information is stored in a SQL Database instance.</span></span> <span data-ttu-id="486c3-152">Hello aplikace se skládá ze dvou souborů (zkopírujte a vložte kód je k dispozici níže):</span><span class="sxs-lookup"><span data-stu-id="486c3-152">hello application consists of two files (copy/paste code available below):</span></span>

* <span data-ttu-id="486c3-153">**index.php**: Zobrazí formulář pro registraci a tabulku obsahující informace osob žádajících o registraci.</span><span class="sxs-lookup"><span data-stu-id="486c3-153">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>
* <span data-ttu-id="486c3-154">**CreateTable.php**: vytvoří hello tabulka databáze SQL pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="486c3-154">**createtable.php**: Creates hello SQL Database table for hello application.</span></span> <span data-ttu-id="486c3-155">Tento soubor se dají použít jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="486c3-155">This file will only be used once.</span></span>

<span data-ttu-id="486c3-156">toorun hello aplikaci místně, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="486c3-156">toorun hello application locally, follow hello steps below.</span></span> <span data-ttu-id="486c3-157">Všimněte si, že tento postup předpokládá, máte PHP a SQL Server Express nastavit na místním počítači, a že je povoleno hello [PDO rozšíření pro SQL Server][pdo-sqlsrv].</span><span class="sxs-lookup"><span data-stu-id="486c3-157">Note that these steps assume you have PHP and SQL Server Express set up on your local machine, and that you have enabled hello [PDO extension for SQL Server][pdo-sqlsrv].</span></span>

1. <span data-ttu-id="486c3-158">Vytvořit databázi systému SQL Server volá `registration`.</span><span class="sxs-lookup"><span data-stu-id="486c3-158">Create a SQL Server database called `registration`.</span></span> <span data-ttu-id="486c3-159">Můžete to provést z hello `sqlcmd` příkazového řádku pomocí těchto příkazů:</span><span class="sxs-lookup"><span data-stu-id="486c3-159">You can do this from hello `sqlcmd` command prompt with these commands:</span></span>
   
        >sqlcmd -S localhost\sqlexpress -U <local user name> -P <local password>
        1> create database registration
        2> GO    
2. <span data-ttu-id="486c3-160">V kořenovém adresáři aplikace, vytvořte dva soubory v ní – jednu s názvem `createtable.php` a jednu s názvem `index.php`.</span><span class="sxs-lookup"><span data-stu-id="486c3-160">In your application root directory, create two files in it - one called `createtable.php` and one called `index.php`.</span></span>
3. <span data-ttu-id="486c3-161">Otevřete hello `createtable.php` soubor v textovém editoru nebo IDE a přidejte následující kód hello.</span><span class="sxs-lookup"><span data-stu-id="486c3-161">Open hello `createtable.php` file in a text editor or IDE and add hello code below.</span></span> <span data-ttu-id="486c3-162">Tento kód bude hello použité toocreate `registration_tbl` tabulky v hello `registration` databáze.</span><span class="sxs-lookup"><span data-stu-id="486c3-162">This code will be used toocreate hello `registration_tbl` table in hello `registration` database.</span></span>
   
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
   
    <span data-ttu-id="486c3-163">Všimněte si, že budete potřebovat tooupdate hello hodnoty pro <code>$user</code> a <code>$pwd</code> s vaší místní systém SQL Server uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="486c3-163">Note that you will need tooupdate hello values for <code>$user</code> and <code>$pwd</code> with your local SQL Server user name and password.</span></span>
4. <span data-ttu-id="486c3-164">V terminálu v kořenovém adresáři hello hello typu aplikace hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="486c3-164">In a terminal at hello root directory of hello application type hello following command:</span></span>
   
        php -S localhost:8000
5. <span data-ttu-id="486c3-165">Otevřete webový prohlížeč a vyhledejte příliš**http://localhost:8000/createtable.php**.</span><span class="sxs-lookup"><span data-stu-id="486c3-165">Open a web browser and browse too**http://localhost:8000/createtable.php**.</span></span> <span data-ttu-id="486c3-166">Tím se vytvoří hello `registration_tbl` tabulky v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="486c3-166">This will create hello `registration_tbl` table in hello database.</span></span>
6. <span data-ttu-id="486c3-167">Otevřete hello **index.php** soubor v textovém editoru nebo IDE a přidejte hello kód základní HTML a CSS hello stránky (v následujících krocích se přidají hello kódu PHP.).</span><span class="sxs-lookup"><span data-stu-id="486c3-167">Open hello **index.php** file in a text editor or IDE and add hello basic HTML and CSS code for hello page (hello PHP code will be added in later steps).</span></span>
   
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
7. <span data-ttu-id="486c3-168">V rámci hello PHP značky přidejte kód PHP pro připojení toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="486c3-168">Within hello PHP tags, add PHP code for connecting toohello database.</span></span>
   
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
   
    <span data-ttu-id="486c3-169">Znovu, budete potřebovat tooupdate hello hodnoty pro <code>$user</code> a <code>$pwd</code> s vaší místní MySQL uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="486c3-169">Again, you will need tooupdate hello values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
8. <span data-ttu-id="486c3-170">Následující kód připojení databáze hello přidáte kód pro vložení registrační informace do databáze hello.</span><span class="sxs-lookup"><span data-stu-id="486c3-170">Following hello database connection code, add code for inserting registration information into hello database.</span></span>
   
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
9. <span data-ttu-id="486c3-171">Nakonec následující kód hello výše, přidejte kód pro načítání dat z databáze hello.</span><span class="sxs-lookup"><span data-stu-id="486c3-171">Finally, following hello code above, add code for retrieving data from hello database.</span></span>
   
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

<span data-ttu-id="486c3-172">Nyní můžete procházet příliš**http://localhost:8000/index.php** tootest hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="486c3-172">You can now browse too**http://localhost:8000/index.php** tootest hello application.</span></span>

## <a name="publish-your-application"></a><span data-ttu-id="486c3-173">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="486c3-173">Publish your application</span></span>
<span data-ttu-id="486c3-174">Po testování vaší aplikace místně, můžete ho publikovat tooApp Service Web Apps pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="486c3-174">After you have tested your application locally, you can publish it tooApp Service Web Apps using Git.</span></span> <span data-ttu-id="486c3-175">Nicméně je nutné nejprve tooupdate informací o připojení databáze hello v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="486c3-175">However, you first need tooupdate hello database connection information in hello application.</span></span> <span data-ttu-id="486c3-176">Pomocí informací připojení databáze hello jste dříve získali (v hello **informace o připojení k databázi SQL získat** část), aktualizace hello následující informace v **i** hello `createdatabase.php` a `index.php` soubory s hello příslušné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="486c3-176">Using hello database connection information you obtained earlier (in hello **Get SQL Database connection information** section), update hello following information in **both** hello `createdatabase.php` and `index.php` files with hello appropriate values:</span></span>

    // DB connection info
    $host = "tcp:<value of Server>";
    $user = "<value of User Name>";
    $pwd = "<your password>";
    $db = "<value of SQL Database>";

> [!NOTE]
> <span data-ttu-id="486c3-177">V hello <code>$host</code>, musí být předponou hello hodnotu serveru <code>tcp:</code>.</span><span class="sxs-lookup"><span data-stu-id="486c3-177">In hello <code>$host</code>, hello value of Server must be prepended with <code>tcp:</code>.</span></span>
> 
> 

<span data-ttu-id="486c3-178">Nyní jsou připravené tooset až publikování Git a publikování aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="486c3-178">Now, you are ready tooset up Git publishing and publish hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="486c3-179">Jedná se o stejný postup si poznamenali na konci hello hello hello **vytvoření webové aplikace Azure a nastavení publikování Git** část výše.</span><span class="sxs-lookup"><span data-stu-id="486c3-179">These are hello same steps noted at hello end of hello **Create an Azure web app and set up Git publishing** section above.</span></span>
> 
> 

1. <span data-ttu-id="486c3-180">Otevřete Git Bash (nebo terminál, pokud je Git v vaše `PATH`), změňte adresáře toohello kořenový adresář aplikace (hello **registrace** directory), a spusťte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="486c3-180">Open GitBash (or a terminal, if Git is in your `PATH`), change directories toohello root directory of your application (hello **registration** directory), and run hello following commands:</span></span>
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    <span data-ttu-id="486c3-181">Jste vyzváni k hello heslo, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="486c3-181">You will be prompted for hello password you created earlier.</span></span>
2. <span data-ttu-id="486c3-182">Procházet příliš**http://[web aplikace name].azurewebsites.net/createtable.php** toocreate hello SQL databázové tabulky pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="486c3-182">Browse too**http://[web app name].azurewebsites.net/createtable.php** toocreate hello SQL database table for hello application.</span></span>
3. <span data-ttu-id="486c3-183">Procházet příliš**http://[web aplikace name].azurewebsites.net/index.php** toobegin pomocí aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="486c3-183">Browse too**http://[web app name].azurewebsites.net/index.php** toobegin using hello application.</span></span>

<span data-ttu-id="486c3-184">Po publikování aplikace, můžete začít vytvářet tooit změny a použít Git toopublish je.</span><span class="sxs-lookup"><span data-stu-id="486c3-184">After you have published your application, you can begin making changes tooit and use Git toopublish them.</span></span> 

## <a name="publish-changes-tooyour-application"></a><span data-ttu-id="486c3-185">Publikování aplikace tooyour změny</span><span class="sxs-lookup"><span data-stu-id="486c3-185">Publish changes tooyour application</span></span>
<span data-ttu-id="486c3-186">toopublish změny tooapplication, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="486c3-186">toopublish changes tooapplication, follow these steps:</span></span>

1. <span data-ttu-id="486c3-187">Proveďte změny tooyour aplikace místně.</span><span class="sxs-lookup"><span data-stu-id="486c3-187">Make changes tooyour application locally.</span></span>
2. <span data-ttu-id="486c3-188">Otevřete Git Bash (nebo terminál, it Git je ve vaší `PATH`), změňte adresáře toohello kořenový adresář aplikace a spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="486c3-188">Open GitBash (or a terminal, it Git is in your `PATH`), change directories toohello root directory of your application, and run hello following commands:</span></span>
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    <span data-ttu-id="486c3-189">Jste vyzváni k hello heslo, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="486c3-189">You will be prompted for hello password you created earlier.</span></span>
3. <span data-ttu-id="486c3-190">Procházet příliš**http://[web aplikace name].azurewebsites.net/index.php** toosee změny.</span><span class="sxs-lookup"><span data-stu-id="486c3-190">Browse too**http://[web app name].azurewebsites.net/index.php** toosee your changes.</span></span>

## <a name="whats-changed"></a><span data-ttu-id="486c3-191">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="486c3-191">What's changed</span></span>
* <span data-ttu-id="486c3-192">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="486c3-192">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[pdo-sqlsrv]: http://php.net/pdo_sqlsrv

