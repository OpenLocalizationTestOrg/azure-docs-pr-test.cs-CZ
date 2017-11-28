---
title: "aaaCreate a připojte tooa databáze MySQL v Azure"
description: "Zjistěte, jak toouse hello Azure portálu toocreate databázi MySQL a potom se připojte tooit z webové aplikace PHP v Azure."
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
ms.openlocfilehash: 3abc02f8887432625666afd847e9dc0c0a4db2e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-connect-tooa-mysql-database-in-azure"></a><span data-ttu-id="76b37-103">Vytvořit a připojit tooa databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="76b37-103">Create and connect tooa MySQL database in Azure</span></span>
<span data-ttu-id="76b37-104">Tento kurz ukazuje, jak databáze toocreate MySQL v hello [portál Azure](https://portal.azure.com) (zprostředkovatel je [ClearDB](http://www.cleardb.com/)) a jak tooconnect tooit z PHP webová aplikace spuštěna [Azure App Service](app-service/app-service-value-prop-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="76b37-104">This tutorial shows you how toocreate a MySQL database in hello [Azure portal](https://portal.azure.com) (provider is [ClearDB](http://www.cleardb.com/)) and how tooconnect tooit from a PHP web app running in [Azure App Service](app-service/app-service-value-prop-what-is.md).</span></span>

> [!NOTE]
> <span data-ttu-id="76b37-105">Můžete také vytvořit databázi MySQL jako součást [šablony aplikace Marketplace](app-service-web/app-service-web-create-web-app-from-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="76b37-105">You can also create a MySQL database as part of a [Marketplace app template](app-service-web/app-service-web-create-web-app-from-marketplace.md).</span></span>
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a><span data-ttu-id="76b37-106">Vytvoření databáze MySQL na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="76b37-106">Create a MySQL database in Azure portal</span></span>
<span data-ttu-id="76b37-107">toocreate databáze MySQL v hello portál Azure, hello následující:</span><span class="sxs-lookup"><span data-stu-id="76b37-107">toocreate a MySQL database in hello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="76b37-108">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="76b37-108">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="76b37-109">V levé nabídce hello, klikněte na **nový** > **Data + úložiště** > **databáze MySQL**.</span><span class="sxs-lookup"><span data-stu-id="76b37-109">From hello left menu, click **New** > **Data + Storage** > **MySQL Database**.</span></span>

    ![Vytvoření databáze MySQL v Azure – spuštění](./media/store-php-create-mysql-database/create-db-1-start.png)
3. <span data-ttu-id="76b37-111">V hello nová databáze MySQL [okno](azure-portal-overview.md), nakonfigurujte novou databázi MySQL takto (*okno*: stránky portálu, které se otevře vodorovně):</span><span class="sxs-lookup"><span data-stu-id="76b37-111">In hello New MySQL Database [blade](azure-portal-overview.md), configure your new MySQL database as follows (*blade*: a portal page that opens horizontally):</span></span>

   * <span data-ttu-id="76b37-112">**Název databáze**: Zadejte jedinečné osobní jméno</span><span class="sxs-lookup"><span data-stu-id="76b37-112">**Database Name**: Type a uniquely identifiable name</span></span>
   * <span data-ttu-id="76b37-113">**Předplatné**: Zvolte předplatné toouse hello</span><span class="sxs-lookup"><span data-stu-id="76b37-113">**Subscription**: Choose hello subscription toouse</span></span>
   * <span data-ttu-id="76b37-114">**Databáze typu**: vyberte **sdílené** pro nízkonákladové nebo volné vrstvy, nebo **Dedicated** tooget vyhrazené prostředky.</span><span class="sxs-lookup"><span data-stu-id="76b37-114">**Database Type**: Select **Shared** for low-cost or free tiers, or **Dedicated** tooget dedicated resources.</span></span>
   * <span data-ttu-id="76b37-115">**Skupina prostředků**: Přidat existující tooan databáze MySQL hello [skupiny prostředků](azure-resource-manager/resource-group-overview.md) nebo pro něj nový.</span><span class="sxs-lookup"><span data-stu-id="76b37-115">**Resource group**: Add hello MySQL database tooan existing [resource group](azure-resource-manager/resource-group-overview.md) or put it in a new one.</span></span> <span data-ttu-id="76b37-116">Prostředky ve stejné skupině se dají snadno spravovat společně hello.</span><span class="sxs-lookup"><span data-stu-id="76b37-116">Resources in hello same group can be easily managed together.</span></span>
   * <span data-ttu-id="76b37-117">**Umístění**: Vyberte tooyou zavřít umístění.</span><span class="sxs-lookup"><span data-stu-id="76b37-117">**Location**: Select a location close tooyou.</span></span> <span data-ttu-id="76b37-118">Při přidávání tooan existující skupinu prostředků, jste umístění skupiny prostředků uzamčeném toohello.</span><span class="sxs-lookup"><span data-stu-id="76b37-118">When adding tooan existing resource group, you're locked toohello resource group's location.</span></span>
   * <span data-ttu-id="76b37-119">**Cenová úroveň**: klikněte na tlačítko **cenová úroveň**, zvolte cenovou možnost (**Mercury** vrstvy je zdarma) a potom klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="76b37-119">**Pricing Tier**: Click **Pricing Tier**, then select a pricing option (**Mercury** tier is free), and then click **Select**.</span></span>
   * <span data-ttu-id="76b37-120">**Právní podmínky**: klikněte na tlačítko **právní podmínky**, přečtěte si podrobnosti o nákupu hello a klikněte na tlačítko **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="76b37-120">**Legal Terms**: Click **Legal Terms**, review hello purchase details, and click **Purchase**.</span></span>
   * <span data-ttu-id="76b37-121">**PIN kód toodashboard**: vyberte, pokud chcete, aby tooaccess přímo z řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="76b37-121">**Pin toodashboard**: Select if you want tooaccess it directly from hello dashboard.</span></span> <span data-ttu-id="76b37-122">To je obzvláště užitečné, pokud se nevyznáte v portálu navigační ještě.</span><span class="sxs-lookup"><span data-stu-id="76b37-122">This is especially helpful if you aren't familiar with portal navigation yet.</span></span>

     <span data-ttu-id="76b37-123">Následující snímek obrazovky Hello je právě příklad konfiguraci databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="76b37-123">hello following screenshot is just an example of how you can configure your MySQL database.</span></span>  
     ![Vytvoření databáze MySQL v Azure – konfigurace](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. <span data-ttu-id="76b37-125">Po dokončení konfigurace, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="76b37-125">When you're done configuring, click **Create**.</span></span>

    ![Vytvoření databáze MySQL v Azure – vytvoření](./media/store-php-create-mysql-database/create-db-3-create.png)

    <span data-ttu-id="76b37-127">Zobrazí se automaticky otevírané okno notification informací, víte, že se zahájilo nasazení.</span><span class="sxs-lookup"><span data-stu-id="76b37-127">You will see a pop-up notification letting you know that deployment has started.</span></span>

    ![Vytvoření databáze MySQL v Azure – v průběhu](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    <span data-ttu-id="76b37-129">Zobrazí se další automaticky otevírané okno po nasazení proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="76b37-129">You will get another pop-up once deployment has succeeded.</span></span> <span data-ttu-id="76b37-130">portál Hello také se automaticky otevře okna databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="76b37-130">hello portal will also open your MySQL database blade automatically.</span></span>

<a name="connect"></a>

## <a name="connect-tooyour-mysql-database"></a><span data-ttu-id="76b37-131">Připojit databáze MySQL tooyour</span><span class="sxs-lookup"><span data-stu-id="76b37-131">Connect tooyour MySQL database</span></span>
<span data-ttu-id="76b37-132">informace o připojení hello toosee pro novou databázi MySQL, stačí kliknout na **vlastnosti** v okně vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="76b37-132">toosee hello connection information for your new MySQL database, just click **Properties** in your web app's blade.</span></span>

![Vytvoření databáze MySQL v Azure – okno databáze MySQL](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

<span data-ttu-id="76b37-134">Teď můžete použít informace o tomto připojení v jakékoli webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="76b37-134">You can now use that connection information in any web app.</span></span> <span data-ttu-id="76b37-135">Ukázka, která ukazuje, jak je k dispozici informace o připojení hello toouse z jednoduchou aplikaci PHP [zde](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).</span><span class="sxs-lookup"><span data-stu-id="76b37-135">A sample that shows how toouse hello connection information from a simple PHP app is available [here](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).</span></span>

## <a name="connect-a-laravel-web-app-from-hello-php-get-started-tutorial"></a><span data-ttu-id="76b37-136">Připojení webové aplikace Laravel (z hello PHP get kurzu Začínáme)</span><span class="sxs-lookup"><span data-stu-id="76b37-136">Connect a Laravel web app (from hello PHP get started tutorial)</span></span>
<span data-ttu-id="76b37-137">Předpokládejme, právě dokončení hello kurzu [vytvořit, nakonfigurovat a nasadit tooAzure webové aplikace PHP](app-service-web/app-service-web-php-get-started.md) a mít [Laravel](https://www.laravel.com/) webová aplikace spuštěná v Azure.</span><span class="sxs-lookup"><span data-stu-id="76b37-137">Suppose you just finished hello tutorial [Create, configure, and deploy a PHP web app tooAzure](app-service-web/app-service-web-php-get-started.md) and have a [Laravel](https://www.laravel.com/) web app running in Azure.</span></span> <span data-ttu-id="76b37-138">Můžete snadno přidat databáze možnosti tooyour Laravel aplikace.</span><span class="sxs-lookup"><span data-stu-id="76b37-138">You can easily add database capabilities tooyour Laravel app.</span></span> <span data-ttu-id="76b37-139">Postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="76b37-139">Just follow hello steps below:</span></span>

> [!NOTE]
> <span data-ttu-id="76b37-140">Hello následující kroky předpokládají, že dokončení kurzu hello [vytvořit, nakonfigurovat a nasadit tooAzure webové aplikace PHP](app-service-web/app-service-web-php-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="76b37-140">hello following steps assume that you have finished hello tutorial [Create, configure, and deploy a PHP web app tooAzure](app-service-web/app-service-web-php-get-started.md).</span></span>
>
>

1. <span data-ttu-id="76b37-141">Konfigurace aplikace hello Laravel v databázi MySQL toohello toopoint místním vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="76b37-141">Configure hello Laravel app in your local development environment toopoint toohello MySQL database.</span></span> <span data-ttu-id="76b37-142">toodo tuto, otevřete `.env` z vaší aplikace Laravel kořenového adresáře a nastavit možnosti databáze MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="76b37-142">toodo this, open `.env` from your Laravel app's root directory and configure hello MySQL database options.</span></span>

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

   > [!NOTE]
   > <span data-ttu-id="76b37-143">V hello **vlastnosti** okně hello název vaší databáze MySQL může nebo nemusí být hello jeden zobrazeny v hello **název databáze** pole.</span><span class="sxs-lookup"><span data-stu-id="76b37-143">In hello **Properties** blade, hello name of your MySQL database may or may not be hello one shown in hello **DATABASE NAME** field.</span></span> <span data-ttu-id="76b37-144">Je lepší toocheck hello databáze parametr v hello **PŘIPOJOVACÍ řetězec** pole.</span><span class="sxs-lookup"><span data-stu-id="76b37-144">It's better toocheck hello Database parameter in hello **CONNECTION STRING** field.</span></span>    
   >
   > ![Vytvoření databáze MySQL v Azure – v průběhu](./media/store-php-create-mysql-database/connect-db-1-database-name.png)
   >
   >
2. <span data-ttu-id="76b37-146">Dobrý den, abyste měli přístup MySQL teď nejrychlejší způsob, jak tooverify je toouse [Laravel je výchozí ověřování generování uživatelského rozhraní](https://laravel.com/docs/5.2/authentication#authentication-quickstart).</span><span class="sxs-lookup"><span data-stu-id="76b37-146">hello quickest way tooverify that you have MySQL access now is toouse [Laravel's default authentication scaffolding](https://laravel.com/docs/5.2/authentication#authentication-quickstart).</span></span>
   <span data-ttu-id="76b37-147">V hello terminál příkazového řádku spusťte následující příkazy z kořenového adresáře aplikace Laravel hello:</span><span class="sxs-lookup"><span data-stu-id="76b37-147">In hello command-line terminal, run hello following commands from your Laravel app's root directory:</span></span>

         php artisan migrate
         php artisan make:auth

    <span data-ttu-id="76b37-148">První příkaz Hello vytváří hello tabulky v Azure podle předdefinovaných migrace v hello `database/migrations` adresář a druhý příkaz hello scaffold hello základní zobrazení a trasy pro registraci uživatelů a ověřování.</span><span class="sxs-lookup"><span data-stu-id="76b37-148">hello first command creates hello tables in Azure based on predefined migrations in hello `database/migrations` directory, and hello second  command scaffolds hello basic views and routes for user registration and authentication.</span></span>
3. <span data-ttu-id="76b37-149">Nyní spusťte hello vývojového serveru:</span><span class="sxs-lookup"><span data-stu-id="76b37-149">Run hello development server now:</span></span>

        php artisan serve
4. <span data-ttu-id="76b37-150">V prohlížeči hello přejděte toohttp://localhost:8000 a registraci nového uživatele, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="76b37-150">In hello browser, navigate toohttp://localhost:8000 and register a new user as shown:</span></span>

    ![Připojit databáze tooMySQL ve službě Azure - registraci uživatele](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    <span data-ttu-id="76b37-152">Postupujte podle hello uživatelského rozhraní výzva hello dokončení registrace.</span><span class="sxs-lookup"><span data-stu-id="76b37-152">Follow hello UI prompt complete hello registration.</span></span> <span data-ttu-id="76b37-153">Po dokončení registrace budete přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="76b37-153">Once registration completes, you will be logged in.</span></span>

    ![Připojit databáze tooMySQL ve službě Azure - registraci uživatele](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    <span data-ttu-id="76b37-155">Vyvíjíte aplikace hello databázi MySQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="76b37-155">You are now developing your app against hello MySQL database in Azure.</span></span>
5. <span data-ttu-id="76b37-156">Nyní, stačí tooreplicate vaše `.env` nastavení tooyour webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="76b37-156">Now, you just need tooreplicate your `.env` settings tooyour Azure web app.</span></span> <span data-ttu-id="76b37-157">Spusťte následující příkazy příkazového řádku Azure CLI hello:</span><span class="sxs-lookup"><span data-stu-id="76b37-157">Run hello following Azure CLI commands:</span></span>

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

6. <span data-ttu-id="76b37-158">V dalším kroku potvrďte a odešlete tooAzure hello místních změn provedených dříve během spuštění `php artisan make:auth`.</span><span class="sxs-lookup"><span data-stu-id="76b37-158">Next, commit and push tooAzure hello local changes made earlier while running `php artisan make:auth`.</span></span>

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master
7. <span data-ttu-id="76b37-159">Procházejte toohello webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="76b37-159">Browse toohello Azure web app.</span></span>

        azure site browse
8. <span data-ttu-id="76b37-160">Přihlaste se pomocí přihlašovacích údajů uživatele hello, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="76b37-160">Log in using hello user credentials you created earlier.</span></span>

    ![Připojit databáze tooMySQL ve službě Azure - procházet tooAzure webové aplikace](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    <span data-ttu-id="76b37-162">Po přihlášení, měli byste vidět hello popisný po přihlašovací obrazovku.</span><span class="sxs-lookup"><span data-stu-id="76b37-162">After you log in, you should see hello friendly post-login screen.</span></span>

    ![Připojit databáze tooMySQL ve službě Azure - přihlášení](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    <span data-ttu-id="76b37-164">Blahopřejeme, webové aplikace PHP v Azure je nyní přístup k datům z databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="76b37-164">Congratulations, your PHP web app in Azure is now accessing data from your MySQL database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="76b37-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="76b37-165">Next steps</span></span>
<span data-ttu-id="76b37-166">Další informace najdete v tématu hello [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="76b37-166">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>
