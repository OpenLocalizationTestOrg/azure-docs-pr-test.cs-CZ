---
title: "Vytvoření databáze MySQL v Azure a připojení k ní"
description: "Další informace o použití portálu Azure k vytvoření databáze MySQL a potom k němu připojit z webové aplikace PHP v Azure."
documentationcenter: php
services: app-service\web
author: cephalin
manager: erikre
editor: 
tags: mysql
ms.assetid: 55465a9a-7e65-4fd9-8a65-dd83ee41f3e5
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;cephalin
ms.openlocfilehash: 66f4b7a5f8eb3f6f125c9420b40caffca3d43dd6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-connect-to-a-mysql-database-in-azure"></a><span data-ttu-id="b9891-103">Vytvoření databáze MySQL v Azure a připojení k ní</span><span class="sxs-lookup"><span data-stu-id="b9891-103">Create and connect to a MySQL database in Azure</span></span>
<span data-ttu-id="b9891-104">V tomto kurzu se dozvíte, jak k vytvoření databáze MySQL v [portál Azure](https://portal.azure.com) (zprostředkovatel je [ClearDB](http://www.cleardb.com/)) a jak se připojit k němu z webové aplikace PHP spuštěné v [Azure App Service](app-service/app-service-value-prop-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="b9891-104">This tutorial shows you how to create a MySQL database in the [Azure portal](https://portal.azure.com) (provider is [ClearDB](http://www.cleardb.com/)) and how to connect to it from a PHP web app running in [Azure App Service](app-service/app-service-value-prop-what-is.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b9891-105">Můžete také vytvořit databázi MySQL jako součást [šablony aplikace Marketplace](app-service-web/app-service-web-create-web-app-from-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="b9891-105">You can also create a MySQL database as part of a [Marketplace app template](app-service-web/app-service-web-create-web-app-from-marketplace.md).</span></span>
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a><span data-ttu-id="b9891-106">Vytvoření databáze MySQL na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b9891-106">Create a MySQL database in Azure portal</span></span>
<span data-ttu-id="b9891-107">K vytvoření databáze MySQL na portálu Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b9891-107">To create a MySQL database in the Azure portal, do the following:</span></span>

1. <span data-ttu-id="b9891-108">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b9891-108">Log in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="b9891-109">V levé nabídce klikněte na tlačítko **nový** > **Data + úložiště** > **databáze MySQL**.</span><span class="sxs-lookup"><span data-stu-id="b9891-109">From the left menu, click **New** > **Data + Storage** > **MySQL Database**.</span></span>

    ![Vytvoření databáze MySQL v Azure – spuštění](./media/store-php-create-mysql-database/create-db-1-start.png)
3. <span data-ttu-id="b9891-111">V nová databáze MySQL [okno](azure-portal-overview.md), nakonfigurujte novou databázi MySQL takto (*okno*: stránky portálu, které se otevře vodorovně):</span><span class="sxs-lookup"><span data-stu-id="b9891-111">In the New MySQL Database [blade](azure-portal-overview.md), configure your new MySQL database as follows (*blade*: a portal page that opens horizontally):</span></span>

   * <span data-ttu-id="b9891-112">**Název databáze**: Zadejte jedinečné osobní jméno</span><span class="sxs-lookup"><span data-stu-id="b9891-112">**Database Name**: Type a uniquely identifiable name</span></span>
   * <span data-ttu-id="b9891-113">**Předplatné**: Zvolte předplatné použít</span><span class="sxs-lookup"><span data-stu-id="b9891-113">**Subscription**: Choose the subscription to use</span></span>
   * <span data-ttu-id="b9891-114">**Databáze typu**: vyberte **sdílené** pro nízkonákladové nebo volné vrstvy, nebo **Dedicated** získat vyhrazených prostředcích.</span><span class="sxs-lookup"><span data-stu-id="b9891-114">**Database Type**: Select **Shared** for low-cost or free tiers, or **Dedicated** to get dedicated resources.</span></span>
   * <span data-ttu-id="b9891-115">**Skupina prostředků**: přidejte databáze MySQL na stávající [skupiny prostředků](azure-resource-manager/resource-group-overview.md) nebo ji umístit do nové.</span><span class="sxs-lookup"><span data-stu-id="b9891-115">**Resource group**: Add the MySQL database to an existing [resource group](azure-resource-manager/resource-group-overview.md) or put it in a new one.</span></span> <span data-ttu-id="b9891-116">Prostředky ve stejné skupině se dají snadno spravovat společně.</span><span class="sxs-lookup"><span data-stu-id="b9891-116">Resources in the same group can be easily managed together.</span></span>
   * <span data-ttu-id="b9891-117">**Umístění**: Vyberte umístění blízko vás.</span><span class="sxs-lookup"><span data-stu-id="b9891-117">**Location**: Select a location close to you.</span></span> <span data-ttu-id="b9891-118">Při přidávání do existující skupiny prostředků, že je uzamčené umístění skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="b9891-118">When adding to an existing resource group, you're locked to the resource group's location.</span></span>
   * <span data-ttu-id="b9891-119">**Cenová úroveň**: klikněte na tlačítko **cenová úroveň**, zvolte cenovou možnost (**Mercury** vrstvy je zdarma) a potom klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="b9891-119">**Pricing Tier**: Click **Pricing Tier**, then select a pricing option (**Mercury** tier is free), and then click **Select**.</span></span>
   * <span data-ttu-id="b9891-120">**Právní podmínky**: klikněte na tlačítko **právní podmínky**, přečtěte si podrobnosti o nákupu a klikněte na tlačítko **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="b9891-120">**Legal Terms**: Click **Legal Terms**, review the purchase details, and click **Purchase**.</span></span>
   * <span data-ttu-id="b9891-121">**Připnout na řídicí panel**: vyberte, pokud chcete získat přístup přímo z řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="b9891-121">**Pin to dashboard**: Select if you want to access it directly from the dashboard.</span></span> <span data-ttu-id="b9891-122">To je obzvláště užitečné, pokud se nevyznáte v portálu navigační ještě.</span><span class="sxs-lookup"><span data-stu-id="b9891-122">This is especially helpful if you aren't familiar with portal navigation yet.</span></span>

     <span data-ttu-id="b9891-123">Na následujícím snímku obrazovky je právě příklad konfiguraci databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="b9891-123">The following screenshot is just an example of how you can configure your MySQL database.</span></span>  
     ![Vytvoření databáze MySQL v Azure – konfigurace](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. <span data-ttu-id="b9891-125">Po dokončení konfigurace, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b9891-125">When you're done configuring, click **Create**.</span></span>

    ![Vytvoření databáze MySQL v Azure – vytvoření](./media/store-php-create-mysql-database/create-db-3-create.png)

    <span data-ttu-id="b9891-127">Zobrazí se automaticky otevírané okno notification informací, víte, že se zahájilo nasazení.</span><span class="sxs-lookup"><span data-stu-id="b9891-127">You will see a pop-up notification letting you know that deployment has started.</span></span>

    ![Vytvoření databáze MySQL v Azure – v průběhu](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    <span data-ttu-id="b9891-129">Zobrazí se další automaticky otevírané okno po nasazení proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="b9891-129">You will get another pop-up once deployment has succeeded.</span></span> <span data-ttu-id="b9891-130">Na portálu také se automaticky otevře okna databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="b9891-130">The portal will also open your MySQL database blade automatically.</span></span>

<a name="connect"></a>

## <a name="connect-to-your-mysql-database"></a><span data-ttu-id="b9891-131">Připojení k vaší databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="b9891-131">Connect to your MySQL database</span></span>
<span data-ttu-id="b9891-132">Pokud chcete zobrazit informace o připojení pro novou databázi MySQL, stačí kliknout na **vlastnosti** v okně vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9891-132">To see the connection information for your new MySQL database, just click **Properties** in your web app's blade.</span></span>

![Vytvoření databáze MySQL v Azure – okno databáze MySQL](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

<span data-ttu-id="b9891-134">Teď můžete použít informace o tomto připojení v jakékoli webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b9891-134">You can now use that connection information in any web app.</span></span> <span data-ttu-id="b9891-135">Ukázka, která ukazuje, jak používat informace o připojení z jednoduchou aplikaci PHP je k dispozici [zde](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).</span><span class="sxs-lookup"><span data-stu-id="b9891-135">A sample that shows how to use the connection information from a simple PHP app is available [here](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).</span></span>

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a><span data-ttu-id="b9891-136">Připojení webové aplikace Laravel (z PHP get kurzu Začínáme)</span><span class="sxs-lookup"><span data-stu-id="b9891-136">Connect a Laravel web app (from the PHP get started tutorial)</span></span>
<span data-ttu-id="b9891-137">Předpokládejme, že právě jste dokončili kurz [vytvoření, konfigurace a nasazení webové aplikace PHP do Azure](app-service-web/app-service-web-php-get-started.md) a mít [Laravel](https://www.laravel.com/) webová aplikace spuštěná v Azure.</span><span class="sxs-lookup"><span data-stu-id="b9891-137">Suppose you just finished the tutorial [Create, configure, and deploy a PHP web app to Azure](app-service-web/app-service-web-php-get-started.md) and have a [Laravel](https://www.laravel.com/) web app running in Azure.</span></span> <span data-ttu-id="b9891-138">Možnosti databáze můžete snadno přidat do aplikace Laravel.</span><span class="sxs-lookup"><span data-stu-id="b9891-138">You can easily add database capabilities to your Laravel app.</span></span> <span data-ttu-id="b9891-139">Postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="b9891-139">Just follow the steps below:</span></span>

> [!NOTE]
> <span data-ttu-id="b9891-140">Následující postup předpokládá, že dokončení tohoto kurzu [vytvoření, konfigurace a nasazení webové aplikace PHP do Azure](app-service-web/app-service-web-php-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b9891-140">The following steps assume that you have finished the tutorial [Create, configure, and deploy a PHP web app to Azure](app-service-web/app-service-web-php-get-started.md).</span></span>
>
>

1. <span data-ttu-id="b9891-141">Konfigurace aplikace Laravel ve vašem místním vývojovém prostředí tak, aby odkazoval na databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="b9891-141">Configure the Laravel app in your local development environment to point to the MySQL database.</span></span> <span data-ttu-id="b9891-142">Chcete-li to provést, otevřete `.env` z vaší aplikace Laravel kořenový adresář a nastavit možnosti databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="b9891-142">To do this, open `.env` from your Laravel app's root directory and configure the MySQL database options.</span></span>

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

   > [!NOTE]
   > <span data-ttu-id="b9891-143">V **vlastnosti** okně název vaší databáze MySQL může nebo nemusí být ten ve zobrazeny **název databáze** pole.</span><span class="sxs-lookup"><span data-stu-id="b9891-143">In the **Properties** blade, the name of your MySQL database may or may not be the one shown in the **DATABASE NAME** field.</span></span> <span data-ttu-id="b9891-144">Je lepší změnami parametr databáze **PŘIPOJOVACÍ řetězec** pole.</span><span class="sxs-lookup"><span data-stu-id="b9891-144">It's better to check the Database parameter in the **CONNECTION STRING** field.</span></span>    
   >
   > ![Vytvoření databáze MySQL v Azure – v průběhu](./media/store-php-create-mysql-database/connect-db-1-database-name.png)
   >
   >
2. <span data-ttu-id="b9891-146">Nejrychlejší způsob, jak ověřte, zda máte přístup MySQL teď má používat [Laravel je výchozí ověřování generování uživatelského rozhraní](https://laravel.com/docs/5.2/authentication#authentication-quickstart).</span><span class="sxs-lookup"><span data-stu-id="b9891-146">The quickest way to verify that you have MySQL access now is to use [Laravel's default authentication scaffolding](https://laravel.com/docs/5.2/authentication#authentication-quickstart).</span></span>
   <span data-ttu-id="b9891-147">V terminál příkazového řádku spusťte následující příkazy z kořenového adresáře aplikace Laravel:</span><span class="sxs-lookup"><span data-stu-id="b9891-147">In the command-line terminal, run the following commands from your Laravel app's root directory:</span></span>

         php artisan migrate
         php artisan make:auth

    <span data-ttu-id="b9891-148">První příkaz vytvoří tabulky, v závislosti na předdefinované migrace v Azure `database/migrations` directory a v druhém příkazu scaffolds základní zobrazení a trasy pro registraci uživatelů a ověřování.</span><span class="sxs-lookup"><span data-stu-id="b9891-148">The first command creates the tables in Azure based on predefined migrations in the `database/migrations` directory, and the second  command scaffolds the basic views and routes for user registration and authentication.</span></span>
3. <span data-ttu-id="b9891-149">Vývojový server nyní spusťte:</span><span class="sxs-lookup"><span data-stu-id="b9891-149">Run the development server now:</span></span>

        php artisan serve
4. <span data-ttu-id="b9891-150">V prohlížeči přejděte na http://localhost: 8000 a registraci nového uživatele, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="b9891-150">In the browser, navigate to http://localhost:8000 and register a new user as shown:</span></span>

    ![Připojení k databázi MySQL v Azure – registrace uživatele](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    <span data-ttu-id="b9891-152">Po zobrazení výzvy uživatelského rozhraní dokončení registrace.</span><span class="sxs-lookup"><span data-stu-id="b9891-152">Follow the UI prompt complete the registration.</span></span> <span data-ttu-id="b9891-153">Po dokončení registrace budete přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="b9891-153">Once registration completes, you will be logged in.</span></span>

    ![Připojení k databázi MySQL v Azure – registrace uživatele](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    <span data-ttu-id="b9891-155">Vyvíjíte aplikace pro databázi MySQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="b9891-155">You are now developing your app against the MySQL database in Azure.</span></span>
5. <span data-ttu-id="b9891-156">Nyní, stačí k replikaci vaší `.env` nastavení vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="b9891-156">Now, you just need to replicate your `.env` settings to your Azure web app.</span></span> <span data-ttu-id="b9891-157">Spusťte následující příkazy rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="b9891-157">Run the following Azure CLI commands:</span></span>

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

6. <span data-ttu-id="b9891-158">V dalším kroku potvrďte a odešlete do Azure místní změny provedené dříve během spuštění `php artisan make:auth`.</span><span class="sxs-lookup"><span data-stu-id="b9891-158">Next, commit and push to Azure the local changes made earlier while running `php artisan make:auth`.</span></span>

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master
7. <span data-ttu-id="b9891-159">Přejděte do webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="b9891-159">Browse to the Azure web app.</span></span>

        azure site browse
8. <span data-ttu-id="b9891-160">Přihlaste se pomocí přihlašovacích údajů uživatele, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="b9891-160">Log in using the user credentials you created earlier.</span></span>

    ![Připojení k databázi MySQL v Azure – přejděte do webové aplikace Azure](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    <span data-ttu-id="b9891-162">Po přihlášení, měli byste vidět popisný po přihlašovací obrazovce.</span><span class="sxs-lookup"><span data-stu-id="b9891-162">After you log in, you should see the friendly post-login screen.</span></span>

    ![Připojení k databázi MySQL v Azure - přihlášení](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    <span data-ttu-id="b9891-164">Blahopřejeme, webové aplikace PHP v Azure je nyní přístup k datům z databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="b9891-164">Congratulations, your PHP web app in Azure is now accessing data from your MySQL database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9891-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b9891-165">Next steps</span></span>
<span data-ttu-id="b9891-166">Další informace najdete v tématu [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="b9891-166">For more information, see the [PHP Developer Center](/develop/php/).</span></span>
