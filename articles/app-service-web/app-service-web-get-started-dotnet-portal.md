---
title: "Nasazení webové aplikace Umbraco na webu Azure Portal v pěti minutách | Dokumentace Microsoftu"
description: "Nasazením ukázkové aplikace v ASP.NET zjistíte, jak snadné je spustit webové aplikace ve službě App Service. Výsledky si můžete okamžitě prohlédnout."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: cephalin
ms.openlocfilehash: 9e3e2130a66cdfe5f06eb3b366e53028c44e7e6a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-an-umbraco-web-app-in-the-azure-portal-in-five-minutes"></a><span data-ttu-id="80345-104">Nasazení webové aplikace Umbraco na webu Azure Portal v pěti minutách</span><span class="sxs-lookup"><span data-stu-id="80345-104">Deploy an Umbraco web app in the Azure portal in five minutes</span></span>

<span data-ttu-id="80345-105">Tento kurz vám pomůže nasadit webovou aplikaci [Umbraco](https://our.umbraco.org/) do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md) během několika minut.</span><span class="sxs-lookup"><span data-stu-id="80345-105">This tutorial helps you deploy n [Umbraco](https://our.umbraco.org/) web app to [Azure App Service](../app-service/app-service-value-prop-what-is.md) in minutes.</span></span>

![Aplikace Umbraco](./media/app-service-web-get-started-dotnet-portal/defaultpage.png)

## <a name="prerequisites"></a><span data-ttu-id="80345-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="80345-107">Prerequisites</span></span>
<span data-ttu-id="80345-108">Potřebujete účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="80345-108">You need a Microsoft Azure account.</span></span> <span data-ttu-id="80345-109">Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="80345-109">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="80345-110">[App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="80345-110">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="80345-111">Můžete si vytvořit úvodní aplikaci a celou hodinu si s ní hrát, bez platebních karet a bez závazků.</span><span class="sxs-lookup"><span data-stu-id="80345-111">Create a starter app and play with it for up to an hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="deploy-the-aspnet-app"></a><span data-ttu-id="80345-112">Nasazení aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="80345-112">Deploy the ASP.NET app</span></span>
1. <span data-ttu-id="80345-113">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="80345-113">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="80345-114">Otevřete [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS).</span><span class="sxs-lookup"><span data-stu-id="80345-114">Open [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS).</span></span>

    <span data-ttu-id="80345-115">Prostřednictvím tohoto odkazu se okamžitě nakonfiguruje nová aplikace Umbraco na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="80345-115">This link is a shortcut to immediately configure a new Umbraco app in the Azure portal.</span></span>

3. <span data-ttu-id="80345-116">Do pole **Název aplikace** zadejte název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="80345-116">In **App name**, type a web app name.</span></span> <span data-ttu-id="80345-117">Pokud je zadaný název v rámci domény `azurewebsites.net` jedinečný, zobrazí se v tomto poli zelené zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="80345-117">You will see a green checkmark in the box if the name is unique in the `azurewebsites.net` domain.</span></span>
   
5. <span data-ttu-id="80345-118">Ve skupině **Skupina prostředků** klikněte na **Vytvořit novou**. Vytvoří se nová [skupina prostředků](../azure-resource-manager/resource-group-overview.md). Pojmenujte ji.</span><span class="sxs-lookup"><span data-stu-id="80345-118">In **Resource Group**, click **Create new** to create a new [resource group](../azure-resource-manager/resource-group-overview.md), then give it a name.</span></span>

7. <span data-ttu-id="80345-119">Klikněte na **Plán nebo umístění služby App Service** > **Vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="80345-119">Click **App Service plan/Location** > **Create New**.</span></span> <span data-ttu-id="80345-120">[Plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) nakonfigurujte podle následujícího postupu:</span><span class="sxs-lookup"><span data-stu-id="80345-120">Configure the [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) as shown:</span></span>

    - <span data-ttu-id="80345-121">Do pole **Plán služby App Service** zadejte požadovaný název.</span><span class="sxs-lookup"><span data-stu-id="80345-121">In **App Service plan**, type the desired name.</span></span>
    - <span data-ttu-id="80345-122">V poli **Umístění** zvolte umístění pro hostování vašeho plánu.</span><span class="sxs-lookup"><span data-stu-id="80345-122">In **Location**, choose a location to host your plan.</span></span>
    - <span data-ttu-id="80345-123">Klikněte na **Cenová úroveň** a potom vyberte **F1 Free** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="80345-123">Click **Pricing tier**, then select **F1 Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="80345-124">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="80345-124">Click **OK**.</span></span>

    <span data-ttu-id="80345-125">Vaše konfigurace CMS Umbraco by teď měla vypadat podobně jako na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="80345-125">Your Umbraco CMS configuration should now look like the following screenshot:</span></span>

    ![Konfigurace probíhá – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-in-progress.png)

12. <span data-ttu-id="80345-127">Klikněte na **SQL Database** > **Vytvořit novou databázi**.</span><span class="sxs-lookup"><span data-stu-id="80345-127">Click **SQL Database** > **Create a new database**.</span></span> <span data-ttu-id="80345-128">SQL Database nakonfigurujte podle následujícího postupu:</span><span class="sxs-lookup"><span data-stu-id="80345-128">Configure the SQL Database as shown:</span></span>

    - <span data-ttu-id="80345-129">Do pole **Název** zadejte název, například **myDB**.</span><span class="sxs-lookup"><span data-stu-id="80345-129">In **Name**, type a name, such as **myDB**.</span></span>
    - <span data-ttu-id="80345-130">Klikněte na **Cenová úroveň** a potom vyberte **F1 Free** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="80345-130">Click **Pricing tier**, then select **F Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="80345-131">Klikněte na **Cílový server** > **Vytvořit nový server**.</span><span class="sxs-lookup"><span data-stu-id="80345-131">Click **Target server** > **Create a new server**.</span></span> <span data-ttu-id="80345-132">Databázový server nakonfigurujte podle následujícího postupu:</span><span class="sxs-lookup"><span data-stu-id="80345-132">Configure the database server as shown:</span></span>

        - <span data-ttu-id="80345-133">Do pole **Název serveru** zadejte název serveru.</span><span class="sxs-lookup"><span data-stu-id="80345-133">In **Server name**, type a server name.</span></span> <span data-ttu-id="80345-134">Pokud je zadaný název v rámci domény `.database.windows.net` jedinečný, zobrazí se v tomto poli zelené zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="80345-134">You will see a green checkmark in the box if the name is unique in the `.database.windows.net` domain.</span></span>
        - <span data-ttu-id="80345-135">Do pole **Přihlášení správce serveru** zadejte požadované uživatelské jméno správce.</span><span class="sxs-lookup"><span data-stu-id="80345-135">In **Server admin login**, type the desired admininistrator username.</span></span>
        - <span data-ttu-id="80345-136">Do polí **Heslo** a **Potvrzení hesla** zadejte požadované heslo.</span><span class="sxs-lookup"><span data-stu-id="80345-136">In **Password** and **Confirm password**, type the desired password.</span></span>
        - <span data-ttu-id="80345-137">V poli Umístění vyberte stejné umístění, které používáte pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="80345-137">In Location, select the same location you use for the web app.</span></span>
        - <span data-ttu-id="80345-138">Ujistěte se, že je vybraná možnost **Povolit službám Azure přístup k serveru**.</span><span class="sxs-lookup"><span data-stu-id="80345-138">Make sure **Allow azure services to access server** is selected.</span></span>
        - <span data-ttu-id="80345-139">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="80345-139">Click **Select**.</span></span>
    
    - <span data-ttu-id="80345-140">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="80345-140">Click **Select**.</span></span>

13. <span data-ttu-id="80345-141">Klikněte na **Nastavení webové aplikace**, zadejte uživatelské jméno a heslo databáze a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="80345-141">Click **Web app settings**, specify the database username and password, and click **OK**.</span></span>

    <span data-ttu-id="80345-142">Vaše konfigurace CMS Umbraco by teď měla vypadat podobně jako na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="80345-142">Your Umbraco CMS configuration should now look like the following screenshot:</span></span>

    ![Konfigurace dokončena – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-complete.png)

14. <span data-ttu-id="80345-144">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="80345-144">Click **Create**.</span></span>
    
    <span data-ttu-id="80345-145">Azure teď na základě vaší konfigurace vytvoří aplikaci Umbraco.</span><span class="sxs-lookup"><span data-stu-id="80345-145">Azure now creates your Umbraco app based on your configuration.</span></span> <span data-ttu-id="80345-146">Mělo by se zobrazit oznámení **Nasazení začalo...**.</span><span class="sxs-lookup"><span data-stu-id="80345-146">You should see a **Deployment started...** notification.</span></span>

    ![Nasazení bylo úspěšné – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-umrbaco-web-app"></a><span data-ttu-id="80345-148">Spuštění a správa webové aplikace Umbraco</span><span class="sxs-lookup"><span data-stu-id="80345-148">Launch and manage your Umrbaco web app</span></span>

<span data-ttu-id="80345-149">Jakmile Azure dokončí nasazení aplikace, uvidíte další oznámení.</span><span class="sxs-lookup"><span data-stu-id="80345-149">When Azure completes app deployment you see another notification.</span></span>

![Nasazení bylo úspěšné – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-succeeded.png)

1. <span data-ttu-id="80345-151">Klikněte na toto oznámení.</span><span class="sxs-lookup"><span data-stu-id="80345-151">Click the notification.</span></span> <span data-ttu-id="80345-152">Pokud ho zmeškáte, můžete ho kdykoli zobrazit kliknutím na zvonek upozornění (![Oznámení níže – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span><span class="sxs-lookup"><span data-stu-id="80345-152">If you missed it, you can always access it by clicking the notification bell (![Notification bellow - first Umbraco in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span></span>

    <span data-ttu-id="80345-153">Teď byste měli vidět [okno](../azure-resource-manager/resource-group-portal.md#manage-resources) pro správu vaší webové aplikace (*okno*: stránka na portálu, která se otevírá vodorovně).</span><span class="sxs-lookup"><span data-stu-id="80345-153">You should now see your web app's management [blade](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blade*: a portal page that opens horizontally).</span></span>

3. <span data-ttu-id="80345-154">V horní části stránky Přehled klikněte na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="80345-154">In the top of the Overview page, click **Browse**.</span></span>
   
    ![Procházet – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/browse.png)

    <span data-ttu-id="80345-156">Teď vidíte **úvodní** stránku Umbraca.</span><span class="sxs-lookup"><span data-stu-id="80345-156">Now you see the Umbraco **Welcome** page.</span></span> <span data-ttu-id="80345-157">Nakonfigurujte instalaci Umbraca a pohrajte si s ní!</span><span class="sxs-lookup"><span data-stu-id="80345-157">Configure the Umbraco installation and start playing with it!</span></span>

    ![Konfigurace Umbraca – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/umbraco-config.png)
    
## <a name="next-steps"></a><span data-ttu-id="80345-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="80345-159">Next steps</span></span>
* <span data-ttu-id="80345-160">[Nasazení webové aplikace ASP.NET do služby Azure App Service pomocí sady Visual Studio](app-service-web-get-started-dotnet.md) – informace o vytvoření nové webové aplikace Azure v sadě Visual Studio pomocí některé z obsažených šablon</span><span class="sxs-lookup"><span data-stu-id="80345-160">[Deploy an ASP.NET web app to Azure App Service, using Visual Studio](app-service-web-get-started-dotnet.md) - Learn how to create a new Azure web app from Visual Studio, using any one of the included application templates.</span></span>
* <span data-ttu-id="80345-161">[Nasazení kódu do služby Azure App Service](web-sites-deploy.md) – informace o nasazení z FTP nebo úložišť správy zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="80345-161">[Deploy your code to Azure App Service](web-sites-deploy.md)- Learn how to deploy from FTP or from source control repositories.</span></span>
* <span data-ttu-id="80345-162">[Přidání funkce do první webové aplikace](app-service-web-get-started-2.md) – pozvednutí aplikace Azure na další úroveň</span><span class="sxs-lookup"><span data-stu-id="80345-162">[Add functionality to your first web app](app-service-web-get-started-2.md) - Take your Azure app to the next level.</span></span> <span data-ttu-id="80345-163">Ověřte svoje uživatele.</span><span class="sxs-lookup"><span data-stu-id="80345-163">Authenticate your users.</span></span> <span data-ttu-id="80345-164">Škálujte ji na základě poptávky.</span><span class="sxs-lookup"><span data-stu-id="80345-164">Scale it based on demand.</span></span> <span data-ttu-id="80345-165">Nastavte některá upozornění týkající se výkonu.</span><span class="sxs-lookup"><span data-stu-id="80345-165">Set up some performance alerts.</span></span> <span data-ttu-id="80345-166">To vše pomocí několika kliknutí.</span><span class="sxs-lookup"><span data-stu-id="80345-166">All with a few clicks.</span></span>
