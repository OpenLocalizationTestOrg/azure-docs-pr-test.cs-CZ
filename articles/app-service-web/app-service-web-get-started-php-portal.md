---
title: "Nasazení aplikace WordPress na webu Azure Portal v pěti minutách | Dokumentace Microsoftu"
description: "Nasazením aplikace WordPress zjistíte, jak snadné je spustit webové aplikace ve službě App Service. Výsledky si můžete okamžitě prohlédnout."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: cephalin
ms.openlocfilehash: ef3be9823384bd8dc978c86f5cfde00d1117c4a2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-a-wordpress-app-in-the-azure-portal-in-five-minutes"></a><span data-ttu-id="08633-104">Nasazení aplikace WordPress na webu Azure Portal v pěti minutách</span><span class="sxs-lookup"><span data-stu-id="08633-104">Deploy a WordPress app in the Azure portal in five minutes</span></span>

<span data-ttu-id="08633-105">V tomto kurzu se dozvíte, jak nasadit první aplikaci [WordPress](https://wordpress.org/) do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md) během několika minut.</span><span class="sxs-lookup"><span data-stu-id="08633-105">This tutorial shows you how to deploy your first [WordPress](https://wordpress.org/) web app to [Azure App Service](../app-service/app-service-value-prop-what-is.md) in minutes.</span></span>

![Web WordPress](./media/app-service-web-get-started-php-portal/wpdashboard.png)

## <a name="prerequisites"></a><span data-ttu-id="08633-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="08633-107">Prerequisites</span></span>
<span data-ttu-id="08633-108">Potřebujete účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="08633-108">You need a Microsoft Azure account.</span></span> <span data-ttu-id="08633-109">Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="08633-109">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="08633-110">[App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="08633-110">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="08633-111">Můžete si vytvořit úvodní aplikaci a celou hodinu si s ní hrát, bez platebních karet a bez závazků.</span><span class="sxs-lookup"><span data-stu-id="08633-111">Create a starter app and play with it for up to an hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="deploy-the-wordpress-app"></a><span data-ttu-id="08633-112">Nasazení aplikace WordPress</span><span class="sxs-lookup"><span data-stu-id="08633-112">Deploy the WordPress app</span></span>
1. <span data-ttu-id="08633-113">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="08633-113">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="08633-114">Otevřete [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).</span><span class="sxs-lookup"><span data-stu-id="08633-114">Open [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).</span></span>

    <span data-ttu-id="08633-115">Prostřednictvím tohoto odkazu se okamžitě nakonfiguruje nová aplikace WordPress na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="08633-115">This link is a shortcut to immediately configure a new WordPress app in the Azure portal.</span></span>

3. <span data-ttu-id="08633-116">Do pole **Název aplikace** zadejte název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="08633-116">In **App name**, type a web app name.</span></span> <span data-ttu-id="08633-117">Pokud je zadaný název v rámci domény `azurewebsites.net` jedinečný, zobrazí se v tomto poli zelené zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="08633-117">You will see a green checkmark in the box if the name is unique in the `azurewebsites.net` domain.</span></span>
   
5. <span data-ttu-id="08633-118">Ve skupině **Skupina prostředků** klikněte na **Vytvořit novou**. Vytvoří se nová [skupina prostředků](../azure-resource-manager/resource-group-overview.md). Pojmenujte ji.</span><span class="sxs-lookup"><span data-stu-id="08633-118">In **Resource Group**, click **Create new** to create a new [resource group](../azure-resource-manager/resource-group-overview.md), then give it a name.</span></span>

6. <span data-ttu-id="08633-119">V části **Poskytovatel databáze** vyberte **CleaDB**.</span><span class="sxs-lookup"><span data-stu-id="08633-119">In **Database Provider**, select **CleaDB**.</span></span>

7. <span data-ttu-id="08633-120">Klikněte na **Plán nebo umístění služby App Service** > **Vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="08633-120">Click **App Service plan/Location** > **Create New**.</span></span> <span data-ttu-id="08633-121">[Plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) nakonfigurujte podle následujícího postupu:</span><span class="sxs-lookup"><span data-stu-id="08633-121">Configure the [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) as shown:</span></span>

    - <span data-ttu-id="08633-122">Do pole **Plán služby App Service** zadejte požadovaný název.</span><span class="sxs-lookup"><span data-stu-id="08633-122">In **App Service plan**, type the desired name.</span></span>
    - <span data-ttu-id="08633-123">V poli **Umístění** zvolte umístění pro hostování vašeho plánu.</span><span class="sxs-lookup"><span data-stu-id="08633-123">In **Location**, choose a location to host your plan.</span></span>
    - <span data-ttu-id="08633-124">Klikněte na **Cenová úroveň** a potom vyberte **F1 Free** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="08633-124">Click **Pricing tier**, then select **F1 Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="08633-125">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="08633-125">Click **OK**.</span></span>

8. <span data-ttu-id="08633-126">Klikněte na **Databáze** > **Vytvořit novou**.</span><span class="sxs-lookup"><span data-stu-id="08633-126">Click **Database** > **Create New**.</span></span> <span data-ttu-id="08633-127">SQL Database nakonfigurujte podle následujícího postupu:</span><span class="sxs-lookup"><span data-stu-id="08633-127">Configure the SQL Database as shown:</span></span>

    - <span data-ttu-id="08633-128">Do pole **Název databáze** zadejte požadovaný název.</span><span class="sxs-lookup"><span data-stu-id="08633-128">In **Database Name**, type a database name.</span></span> 
    - <span data-ttu-id="08633-129">V poli **Umístění** zvolte stejné umístění jako pro plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="08633-129">In **Location**, choose the same location as the App Service plan.</span></span>
    - <span data-ttu-id="08633-130">Klikněte na **Cenová úroveň** a potom vyberte **Mercury** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="08633-130">Click **Pricing tier**, then select **Mercury** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="08633-131">Klikněte na **Právní podmínky** a klikněte na **Koupit**.</span><span class="sxs-lookup"><span data-stu-id="08633-131">Click **Legal Terms** and click **Purchase**.</span></span>
    - <span data-ttu-id="08633-132">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="08633-132">Click **OK**.</span></span>

9. <span data-ttu-id="08633-133">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="08633-133">Click **Create**.</span></span>

    <span data-ttu-id="08633-134">Azure teď na základě vaší konfigurace vytvoří aplikaci WordPress.</span><span class="sxs-lookup"><span data-stu-id="08633-134">Azure now creates your WordPress app based on your configuration.</span></span> <span data-ttu-id="08633-135">Mělo by se zobrazit oznámení **Nasazení začalo...**.</span><span class="sxs-lookup"><span data-stu-id="08633-135">You should see a **Deployment started...** notification.</span></span>

    ![Nasazení začalo – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-wordpress-web-app"></a><span data-ttu-id="08633-137">Spuštění a správa webové aplikace WordPress</span><span class="sxs-lookup"><span data-stu-id="08633-137">Launch and manage your WordPress web app</span></span>

<span data-ttu-id="08633-138">Jakmile Azure dokončí nasazení aplikace, uvidíte další oznámení.</span><span class="sxs-lookup"><span data-stu-id="08633-138">When Azure completes app deployment you see another notification.</span></span>

![Nasazení bylo úspěšné – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/deployment-succeeded.png)

1. <span data-ttu-id="08633-140">Klikněte na toto oznámení.</span><span class="sxs-lookup"><span data-stu-id="08633-140">Click the notification.</span></span> <span data-ttu-id="08633-141">Pokud ho zmeškáte, můžete ho kdykoli zobrazit kliknutím na zvonek upozornění (![Oznámení níže – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span><span class="sxs-lookup"><span data-stu-id="08633-141">If you missed it, you can always access it by clicking the notification bell (![Notification bellow - first WordPress in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span></span>

    <span data-ttu-id="08633-142">Teď byste měli vidět [okno](../azure-resource-manager/resource-group-portal.md#manage-resources) pro správu vaší webové aplikace (*okno*: stránka na portálu, která se otevírá vodorovně).</span><span class="sxs-lookup"><span data-stu-id="08633-142">You should now see your web app's management [blade](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blade*: a portal page that opens horizontally).</span></span>

3. <span data-ttu-id="08633-143">V horní části stránky Přehled klikněte na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="08633-143">In the top of the Overview page, click **Browse**.</span></span>
   
    ![Procházet – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/browse.png)

    <span data-ttu-id="08633-145">Teď vidíte **úvodní** stránku WordPressu.</span><span class="sxs-lookup"><span data-stu-id="08633-145">Now you see the WordPress **Welcome** page.</span></span> <span data-ttu-id="08633-146">Nakonfigurujte instalaci WordPressu a pohrajte si s ní!</span><span class="sxs-lookup"><span data-stu-id="08633-146">Configure the WordPress installation and start playing with it!</span></span>

    ![Konfigurace WordPressu – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/wordpress-config.png)
    
## <a name="next-steps"></a><span data-ttu-id="08633-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08633-148">Next steps</span></span>
* <span data-ttu-id="08633-149">[Vytvořte, nakonfigurujte a nasaďte webovou aplikaci Laravel do Azure](app-service-web-php-get-started.md). Naučte se základní dovednosti pro spuštění libovolné webové aplikace v PHP v prostředí Azure:</span><span class="sxs-lookup"><span data-stu-id="08633-149">[Create, configure, and deploy a Laravel web app to Azure](app-service-web-php-get-started.md) - Learn the basic skills you need to run any PHP web app in Azure, such as:</span></span>

    * <span data-ttu-id="08633-150">Vytvoření a konfigurace aplikací v Azure z prostředí PowerShell/Bash</span><span class="sxs-lookup"><span data-stu-id="08633-150">Create and configure apps in Azure from PowerShell/Bash.</span></span>
    * <span data-ttu-id="08633-151">Nastavení verze PHP</span><span class="sxs-lookup"><span data-stu-id="08633-151">Set PHP version.</span></span>
    * <span data-ttu-id="08633-152">Použití spouštěcího souboru, který není v kořenovém adresáři aplikace</span><span class="sxs-lookup"><span data-stu-id="08633-152">Use a start file that is not in the root application directory.</span></span>
    * <span data-ttu-id="08633-153">Povolení automatizace nástroje Composer</span><span class="sxs-lookup"><span data-stu-id="08633-153">Enable Composer automation.</span></span>
    * <span data-ttu-id="08633-154">Přístup k proměnným pro konkrétní prostředí</span><span class="sxs-lookup"><span data-stu-id="08633-154">Access environment-specific variables.</span></span>
    * <span data-ttu-id="08633-155">Odstraňování běžných chyb</span><span class="sxs-lookup"><span data-stu-id="08633-155">Troubleshoot common errors.</span></span>

* <span data-ttu-id="08633-156">[Nasazení kódu do služby Azure App Service](web-sites-deploy.md) – informace o nasazení z FTP nebo úložišť správy zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="08633-156">[Deploy your code to Azure App Service](web-sites-deploy.md)- Learn how to deploy from FTP or from source control repositories.</span></span>
* <span data-ttu-id="08633-157">[Přidání funkce do první webové aplikace](app-service-web-get-started-2.md) – pozvednutí aplikace Azure na další úroveň</span><span class="sxs-lookup"><span data-stu-id="08633-157">[Add functionality to your first web app](app-service-web-get-started-2.md) - Take your Azure app to the next level.</span></span> <span data-ttu-id="08633-158">Ověřte svoje uživatele.</span><span class="sxs-lookup"><span data-stu-id="08633-158">Authenticate your users.</span></span> <span data-ttu-id="08633-159">Škálujte ji na základě poptávky.</span><span class="sxs-lookup"><span data-stu-id="08633-159">Scale it based on demand.</span></span> <span data-ttu-id="08633-160">Nastavte některá upozornění týkající se výkonu.</span><span class="sxs-lookup"><span data-stu-id="08633-160">Set up some performance alerts.</span></span> <span data-ttu-id="08633-161">To vše pomocí několika kliknutí.</span><span class="sxs-lookup"><span data-stu-id="08633-161">All with a few clicks.</span></span>
