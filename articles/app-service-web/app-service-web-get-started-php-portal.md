---
title: "aaaDeploy aplikace WordPress v hello portálu Azure v pěti minutách | Microsoft Docs"
description: "Zjistěte, jak je snadné toorun webové aplikace ve službě App Service pomocí nasazení aplikace WordPress. Výsledky si můžete okamžitě prohlédnout."
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
ms.openlocfilehash: 4cd26bbacf89657997847ded1284e472288ddebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-wordpress-app-in-hello-azure-portal-in-five-minutes"></a><span data-ttu-id="c6965-104">Nasazení aplikace WordPress v hello portálu Azure v pěti minutách</span><span class="sxs-lookup"><span data-stu-id="c6965-104">Deploy a WordPress app in hello Azure portal in five minutes</span></span>

<span data-ttu-id="c6965-105">Tento kurz ukazuje, jak toodeploy první [WordPress](https://wordpress.org/) webová aplikace příliš[Azure App Service](../app-service/app-service-value-prop-what-is.md) v minutách.</span><span class="sxs-lookup"><span data-stu-id="c6965-105">This tutorial shows you how toodeploy your first [WordPress](https://wordpress.org/) web app too[Azure App Service](../app-service/app-service-value-prop-what-is.md) in minutes.</span></span>

![Web WordPress](./media/app-service-web-get-started-php-portal/wpdashboard.png)

## <a name="prerequisites"></a><span data-ttu-id="c6965-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c6965-107">Prerequisites</span></span>
<span data-ttu-id="c6965-108">Potřebujete účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c6965-108">You need a Microsoft Azure account.</span></span> <span data-ttu-id="c6965-109">Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="c6965-109">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="c6965-110">[App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="c6965-110">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="c6965-111">Vytvoření aplikace starter- and -play se pro až hodinu tooan – nepožaduje se žádná platební karta a bez jakýchkoli závazků.</span><span class="sxs-lookup"><span data-stu-id="c6965-111">Create a starter app and play with it for up tooan hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="deploy-hello-wordpress-app"></a><span data-ttu-id="c6965-112">Nasazení aplikace WordPress hello</span><span class="sxs-lookup"><span data-stu-id="c6965-112">Deploy hello WordPress app</span></span>
1. <span data-ttu-id="c6965-113">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c6965-113">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="c6965-114">Otevřete [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).</span><span class="sxs-lookup"><span data-stu-id="c6965-114">Open [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).</span></span>

    <span data-ttu-id="c6965-115">Tento odkaz je zástupce tooimmediately nakonfigurujte novou aplikaci WordPress v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c6965-115">This link is a shortcut tooimmediately configure a new WordPress app in hello Azure portal.</span></span>

3. <span data-ttu-id="c6965-116">Do pole **Název aplikace** zadejte název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c6965-116">In **App name**, type a web app name.</span></span> <span data-ttu-id="c6965-117">Pokud je v hello jedinečný název hello uvidíte zeleného zaškrtnutí pole hello `azurewebsites.net` domény.</span><span class="sxs-lookup"><span data-stu-id="c6965-117">You will see a green checkmark in hello box if hello name is unique in hello `azurewebsites.net` domain.</span></span>
   
5. <span data-ttu-id="c6965-118">V **skupiny prostředků**, klikněte na tlačítko **vytvořit nový** toocreate a nové [skupiny prostředků](../azure-resource-manager/resource-group-overview.md), zadejte jeho název.</span><span class="sxs-lookup"><span data-stu-id="c6965-118">In **Resource Group**, click **Create new** toocreate a new [resource group](../azure-resource-manager/resource-group-overview.md), then give it a name.</span></span>

6. <span data-ttu-id="c6965-119">V části **Poskytovatel databáze** vyberte **CleaDB**.</span><span class="sxs-lookup"><span data-stu-id="c6965-119">In **Database Provider**, select **CleaDB**.</span></span>

7. <span data-ttu-id="c6965-120">Klikněte na **Plán nebo umístění služby App Service** > **Vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="c6965-120">Click **App Service plan/Location** > **Create New**.</span></span> <span data-ttu-id="c6965-121">Konfigurace hello [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) znázorněné:</span><span class="sxs-lookup"><span data-stu-id="c6965-121">Configure hello [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) as shown:</span></span>

    - <span data-ttu-id="c6965-122">V **plán služby App Service**, název požadovaného typu hello.</span><span class="sxs-lookup"><span data-stu-id="c6965-122">In **App Service plan**, type hello desired name.</span></span>
    - <span data-ttu-id="c6965-123">V **umístění**, zvolit si plán toohost umístění.</span><span class="sxs-lookup"><span data-stu-id="c6965-123">In **Location**, choose a location toohost your plan.</span></span>
    - <span data-ttu-id="c6965-124">Klikněte na **Cenová úroveň** a potom vyberte **F1 Free** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="c6965-124">Click **Pricing tier**, then select **F1 Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="c6965-125">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6965-125">Click **OK**.</span></span>

8. <span data-ttu-id="c6965-126">Klikněte na **Databáze** > **Vytvořit novou**.</span><span class="sxs-lookup"><span data-stu-id="c6965-126">Click **Database** > **Create New**.</span></span> <span data-ttu-id="c6965-127">Nakonfigurujte hello SQL Database, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="c6965-127">Configure hello SQL Database as shown:</span></span>

    - <span data-ttu-id="c6965-128">Do pole **Název databáze** zadejte požadovaný název.</span><span class="sxs-lookup"><span data-stu-id="c6965-128">In **Database Name**, type a database name.</span></span> 
    - <span data-ttu-id="c6965-129">V **umístění**, zvolte hello stejné umístění jako hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="c6965-129">In **Location**, choose hello same location as hello App Service plan.</span></span>
    - <span data-ttu-id="c6965-130">Klikněte na **Cenová úroveň** a potom vyberte **Mercury** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="c6965-130">Click **Pricing tier**, then select **Mercury** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="c6965-131">Klikněte na **Právní podmínky** a klikněte na **Koupit**.</span><span class="sxs-lookup"><span data-stu-id="c6965-131">Click **Legal Terms** and click **Purchase**.</span></span>
    - <span data-ttu-id="c6965-132">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6965-132">Click **OK**.</span></span>

9. <span data-ttu-id="c6965-133">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c6965-133">Click **Create**.</span></span>

    <span data-ttu-id="c6965-134">Azure teď na základě vaší konfigurace vytvoří aplikaci WordPress.</span><span class="sxs-lookup"><span data-stu-id="c6965-134">Azure now creates your WordPress app based on your configuration.</span></span> <span data-ttu-id="c6965-135">Mělo by se zobrazit oznámení **Nasazení začalo...**.</span><span class="sxs-lookup"><span data-stu-id="c6965-135">You should see a **Deployment started...** notification.</span></span>

    ![Nasazení začalo – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-wordpress-web-app"></a><span data-ttu-id="c6965-137">Spuštění a správa webové aplikace WordPress</span><span class="sxs-lookup"><span data-stu-id="c6965-137">Launch and manage your WordPress web app</span></span>

<span data-ttu-id="c6965-138">Jakmile Azure dokončí nasazení aplikace, uvidíte další oznámení.</span><span class="sxs-lookup"><span data-stu-id="c6965-138">When Azure completes app deployment you see another notification.</span></span>

![Nasazení bylo úspěšné – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/deployment-succeeded.png)

1. <span data-ttu-id="c6965-140">Klikněte na tlačítko hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="c6965-140">Click hello notification.</span></span> <span data-ttu-id="c6965-141">Pokud je provedena, je vždy k němu přístup klepnutím na zvonek oznámení hello (![mocí následujících oznámení - první WordPress v Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span><span class="sxs-lookup"><span data-stu-id="c6965-141">If you missed it, you can always access it by clicking hello notification bell (![Notification bellow - first WordPress in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span></span>

    <span data-ttu-id="c6965-142">Teď byste měli vidět [okno](../azure-resource-manager/resource-group-portal.md#manage-resources) pro správu vaší webové aplikace (*okno*: stránka na portálu, která se otevírá vodorovně).</span><span class="sxs-lookup"><span data-stu-id="c6965-142">You should now see your web app's management [blade](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blade*: a portal page that opens horizontally).</span></span>

3. <span data-ttu-id="c6965-143">V hello horní části stránky přehled hello, klikněte na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="c6965-143">In hello top of hello Overview page, click **Browse**.</span></span>
   
    ![Procházet – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/browse.png)

    <span data-ttu-id="c6965-145">Nyní uvidíte hello WordPress **úvodní** stránky.</span><span class="sxs-lookup"><span data-stu-id="c6965-145">Now you see hello WordPress **Welcome** page.</span></span> <span data-ttu-id="c6965-146">Instalaci WordPress hello nakonfigurovat a spustit přehrávání s ním!</span><span class="sxs-lookup"><span data-stu-id="c6965-146">Configure hello WordPress installation and start playing with it!</span></span>

    ![Konfigurace WordPressu – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/wordpress-config.png)
    
## <a name="next-steps"></a><span data-ttu-id="c6965-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6965-148">Next steps</span></span>
* <span data-ttu-id="c6965-149">[Vytváření, konfiguraci a nasazení Laravel webové aplikace tooAzure](app-service-web-php-get-started.md) -další základní znalosti hello potřebujete toorun žádné webové aplikace PHP v Azure, jako například:</span><span class="sxs-lookup"><span data-stu-id="c6965-149">[Create, configure, and deploy a Laravel web app tooAzure](app-service-web-php-get-started.md) - Learn hello basic skills you need toorun any PHP web app in Azure, such as:</span></span>

    * <span data-ttu-id="c6965-150">Vytvoření a konfigurace aplikací v Azure z prostředí PowerShell/Bash</span><span class="sxs-lookup"><span data-stu-id="c6965-150">Create and configure apps in Azure from PowerShell/Bash.</span></span>
    * <span data-ttu-id="c6965-151">Nastavení verze PHP</span><span class="sxs-lookup"><span data-stu-id="c6965-151">Set PHP version.</span></span>
    * <span data-ttu-id="c6965-152">Použijte soubor spuštění, který není v kořenovém adresáři aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c6965-152">Use a start file that is not in hello root application directory.</span></span>
    * <span data-ttu-id="c6965-153">Povolení automatizace nástroje Composer</span><span class="sxs-lookup"><span data-stu-id="c6965-153">Enable Composer automation.</span></span>
    * <span data-ttu-id="c6965-154">Přístup k proměnným pro konkrétní prostředí</span><span class="sxs-lookup"><span data-stu-id="c6965-154">Access environment-specific variables.</span></span>
    * <span data-ttu-id="c6965-155">Odstraňování běžných chyb</span><span class="sxs-lookup"><span data-stu-id="c6965-155">Troubleshoot common errors.</span></span>

* <span data-ttu-id="c6965-156">[Nasazení vaší tooAzure kódu služby App Service](web-sites-deploy.md)– zjistěte, jak řídit toodeploy ze serveru FTP nebo ze zdroje úložiště.</span><span class="sxs-lookup"><span data-stu-id="c6965-156">[Deploy your code tooAzure App Service](web-sites-deploy.md)- Learn how toodeploy from FTP or from source control repositories.</span></span>
* <span data-ttu-id="c6965-157">[Přidání funkce tooyour první webové aplikace](app-service-web-get-started-2.md) -trvat vaše aplikace Azure toohello další úrovně.</span><span class="sxs-lookup"><span data-stu-id="c6965-157">[Add functionality tooyour first web app](app-service-web-get-started-2.md) - Take your Azure app toohello next level.</span></span> <span data-ttu-id="c6965-158">Ověřte svoje uživatele.</span><span class="sxs-lookup"><span data-stu-id="c6965-158">Authenticate your users.</span></span> <span data-ttu-id="c6965-159">Škálujte ji na základě poptávky.</span><span class="sxs-lookup"><span data-stu-id="c6965-159">Scale it based on demand.</span></span> <span data-ttu-id="c6965-160">Nastavte některá upozornění týkající se výkonu.</span><span class="sxs-lookup"><span data-stu-id="c6965-160">Set up some performance alerts.</span></span> <span data-ttu-id="c6965-161">To vše pomocí několika kliknutí.</span><span class="sxs-lookup"><span data-stu-id="c6965-161">All with a few clicks.</span></span>
