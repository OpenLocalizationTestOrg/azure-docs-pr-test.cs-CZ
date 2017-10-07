---
title: "aaaDeploy webové aplikace Umbraco v hello portálu Azure v pěti minutách | Microsoft Docs"
description: "Zjistěte, jak je snadné toorun webové aplikace ve službě App Service pomocí nasazení ukázkové aplikace ASP.NET. Výsledky si můžete okamžitě prohlédnout."
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
ms.openlocfilehash: 6da45f99b3043a3684e3d99c14e6443d597b5212
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-umbraco-web-app-in-hello-azure-portal-in-five-minutes"></a><span data-ttu-id="c60f0-104">Nasazení webové aplikace Umbraco v hello portálu Azure v pěti minutách</span><span class="sxs-lookup"><span data-stu-id="c60f0-104">Deploy an Umbraco web app in hello Azure portal in five minutes</span></span>

<span data-ttu-id="c60f0-105">Tento kurz vám pomůže nasadit n [Umbraco](https://our.umbraco.org/) webová aplikace příliš[Azure App Service](../app-service/app-service-value-prop-what-is.md) v minutách.</span><span class="sxs-lookup"><span data-stu-id="c60f0-105">This tutorial helps you deploy n [Umbraco](https://our.umbraco.org/) web app too[Azure App Service](../app-service/app-service-value-prop-what-is.md) in minutes.</span></span>

![Aplikace Umbraco](./media/app-service-web-get-started-dotnet-portal/defaultpage.png)

## <a name="prerequisites"></a><span data-ttu-id="c60f0-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c60f0-107">Prerequisites</span></span>
<span data-ttu-id="c60f0-108">Potřebujete účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c60f0-108">You need a Microsoft Azure account.</span></span> <span data-ttu-id="c60f0-109">Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="c60f0-109">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="c60f0-110">[App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="c60f0-110">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="c60f0-111">Vytvoření aplikace starter- and -play se pro až hodinu tooan – nepožaduje se žádná platební karta a bez jakýchkoli závazků.</span><span class="sxs-lookup"><span data-stu-id="c60f0-111">Create a starter app and play with it for up tooan hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="deploy-hello-aspnet-app"></a><span data-ttu-id="c60f0-112">Nasaďte aplikaci ASP.NET hello</span><span class="sxs-lookup"><span data-stu-id="c60f0-112">Deploy hello ASP.NET app</span></span>
1. <span data-ttu-id="c60f0-113">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c60f0-113">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="c60f0-114">Otevřete [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS).</span><span class="sxs-lookup"><span data-stu-id="c60f0-114">Open [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS).</span></span>

    <span data-ttu-id="c60f0-115">Tento odkaz je zástupce tooimmediately nakonfigurujte novou aplikaci Umbraco v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c60f0-115">This link is a shortcut tooimmediately configure a new Umbraco app in hello Azure portal.</span></span>

3. <span data-ttu-id="c60f0-116">Do pole **Název aplikace** zadejte název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c60f0-116">In **App name**, type a web app name.</span></span> <span data-ttu-id="c60f0-117">Pokud je v hello jedinečný název hello uvidíte zeleného zaškrtnutí pole hello `azurewebsites.net` domény.</span><span class="sxs-lookup"><span data-stu-id="c60f0-117">You will see a green checkmark in hello box if hello name is unique in hello `azurewebsites.net` domain.</span></span>
   
5. <span data-ttu-id="c60f0-118">V **skupiny prostředků**, klikněte na tlačítko **vytvořit nový** toocreate a nové [skupiny prostředků](../azure-resource-manager/resource-group-overview.md), zadejte jeho název.</span><span class="sxs-lookup"><span data-stu-id="c60f0-118">In **Resource Group**, click **Create new** toocreate a new [resource group](../azure-resource-manager/resource-group-overview.md), then give it a name.</span></span>

7. <span data-ttu-id="c60f0-119">Klikněte na **Plán nebo umístění služby App Service** > **Vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="c60f0-119">Click **App Service plan/Location** > **Create New**.</span></span> <span data-ttu-id="c60f0-120">Konfigurace hello [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) znázorněné:</span><span class="sxs-lookup"><span data-stu-id="c60f0-120">Configure hello [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) as shown:</span></span>

    - <span data-ttu-id="c60f0-121">V **plán služby App Service**, název požadovaného typu hello.</span><span class="sxs-lookup"><span data-stu-id="c60f0-121">In **App Service plan**, type hello desired name.</span></span>
    - <span data-ttu-id="c60f0-122">V **umístění**, zvolit si plán toohost umístění.</span><span class="sxs-lookup"><span data-stu-id="c60f0-122">In **Location**, choose a location toohost your plan.</span></span>
    - <span data-ttu-id="c60f0-123">Klikněte na **Cenová úroveň** a potom vyberte **F1 Free** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="c60f0-123">Click **Pricing tier**, then select **F1 Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="c60f0-124">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c60f0-124">Click **OK**.</span></span>

    <span data-ttu-id="c60f0-125">Konfiguraci Umbraco CMS by teď měl vypadat jako hello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="c60f0-125">Your Umbraco CMS configuration should now look like hello following screenshot:</span></span>

    ![Konfigurace probíhá – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-in-progress.png)

12. <span data-ttu-id="c60f0-127">Klikněte na **SQL Database** > **Vytvořit novou databázi**.</span><span class="sxs-lookup"><span data-stu-id="c60f0-127">Click **SQL Database** > **Create a new database**.</span></span> <span data-ttu-id="c60f0-128">Nakonfigurujte hello SQL Database, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="c60f0-128">Configure hello SQL Database as shown:</span></span>

    - <span data-ttu-id="c60f0-129">Do pole **Název** zadejte název, například **myDB**.</span><span class="sxs-lookup"><span data-stu-id="c60f0-129">In **Name**, type a name, such as **myDB**.</span></span>
    - <span data-ttu-id="c60f0-130">Klikněte na **Cenová úroveň** a potom vyberte **F1 Free** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="c60f0-130">Click **Pricing tier**, then select **F Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="c60f0-131">Klikněte na **Cílový server** > **Vytvořit nový server**.</span><span class="sxs-lookup"><span data-stu-id="c60f0-131">Click **Target server** > **Create a new server**.</span></span> <span data-ttu-id="c60f0-132">Hello databázový server nakonfigurujte, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="c60f0-132">Configure hello database server as shown:</span></span>

        - <span data-ttu-id="c60f0-133">Do pole **Název serveru** zadejte název serveru.</span><span class="sxs-lookup"><span data-stu-id="c60f0-133">In **Server name**, type a server name.</span></span> <span data-ttu-id="c60f0-134">Pokud je v hello jedinečný název hello uvidíte zeleného zaškrtnutí pole hello `.database.windows.net` domény.</span><span class="sxs-lookup"><span data-stu-id="c60f0-134">You will see a green checkmark in hello box if hello name is unique in hello `.database.windows.net` domain.</span></span>
        - <span data-ttu-id="c60f0-135">V **přihlašovací jméno správce serveru**, typ hello potřeby uživatelské jméno správce.</span><span class="sxs-lookup"><span data-stu-id="c60f0-135">In **Server admin login**, type hello desired admininistrator username.</span></span>
        - <span data-ttu-id="c60f0-136">V **heslo** a **potvrzení hesla**, zadejte hello požadované heslo.</span><span class="sxs-lookup"><span data-stu-id="c60f0-136">In **Password** and **Confirm password**, type hello desired password.</span></span>
        - <span data-ttu-id="c60f0-137">V umístění vyberte hello stejné umístění, které používáte pro hello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c60f0-137">In Location, select hello same location you use for hello web app.</span></span>
        - <span data-ttu-id="c60f0-138">Zajistěte, aby **povolit službám azure tooaccess serveru** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="c60f0-138">Make sure **Allow azure services tooaccess server** is selected.</span></span>
        - <span data-ttu-id="c60f0-139">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="c60f0-139">Click **Select**.</span></span>
    
    - <span data-ttu-id="c60f0-140">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="c60f0-140">Click **Select**.</span></span>

13. <span data-ttu-id="c60f0-141">Klikněte na tlačítko **webové nastavení aplikace**, zadejte hello databáze uživatelské jméno a heslo a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c60f0-141">Click **Web app settings**, specify hello database username and password, and click **OK**.</span></span>

    <span data-ttu-id="c60f0-142">Konfiguraci Umbraco CMS by teď měl vypadat jako hello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="c60f0-142">Your Umbraco CMS configuration should now look like hello following screenshot:</span></span>

    ![Konfigurace dokončena – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-complete.png)

14. <span data-ttu-id="c60f0-144">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c60f0-144">Click **Create**.</span></span>
    
    <span data-ttu-id="c60f0-145">Azure teď na základě vaší konfigurace vytvoří aplikaci Umbraco.</span><span class="sxs-lookup"><span data-stu-id="c60f0-145">Azure now creates your Umbraco app based on your configuration.</span></span> <span data-ttu-id="c60f0-146">Mělo by se zobrazit oznámení **Nasazení začalo...**.</span><span class="sxs-lookup"><span data-stu-id="c60f0-146">You should see a **Deployment started...** notification.</span></span>

    ![Nasazení bylo úspěšné – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-umrbaco-web-app"></a><span data-ttu-id="c60f0-148">Spuštění a správa webové aplikace Umbraco</span><span class="sxs-lookup"><span data-stu-id="c60f0-148">Launch and manage your Umrbaco web app</span></span>

<span data-ttu-id="c60f0-149">Jakmile Azure dokončí nasazení aplikace, uvidíte další oznámení.</span><span class="sxs-lookup"><span data-stu-id="c60f0-149">When Azure completes app deployment you see another notification.</span></span>

![Nasazení bylo úspěšné – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-succeeded.png)

1. <span data-ttu-id="c60f0-151">Klikněte na tlačítko hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="c60f0-151">Click hello notification.</span></span> <span data-ttu-id="c60f0-152">Pokud je provedena, je vždy k němu přístup klepnutím na zvonek oznámení hello (![mocí následujících oznámení - první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span><span class="sxs-lookup"><span data-stu-id="c60f0-152">If you missed it, you can always access it by clicking hello notification bell (![Notification bellow - first Umbraco in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span></span>

    <span data-ttu-id="c60f0-153">Teď byste měli vidět [okno](../azure-resource-manager/resource-group-portal.md#manage-resources) pro správu vaší webové aplikace (*okno*: stránka na portálu, která se otevírá vodorovně).</span><span class="sxs-lookup"><span data-stu-id="c60f0-153">You should now see your web app's management [blade](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blade*: a portal page that opens horizontally).</span></span>

3. <span data-ttu-id="c60f0-154">V hello horní části stránky přehled hello, klikněte na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="c60f0-154">In hello top of hello Overview page, click **Browse**.</span></span>
   
    ![Procházet – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/browse.png)

    <span data-ttu-id="c60f0-156">Nyní uvidíte hello Umbraco **úvodní** stránky.</span><span class="sxs-lookup"><span data-stu-id="c60f0-156">Now you see hello Umbraco **Welcome** page.</span></span> <span data-ttu-id="c60f0-157">Instalaci Umbraco hello nakonfigurovat a spustit přehrávání s ním!</span><span class="sxs-lookup"><span data-stu-id="c60f0-157">Configure hello Umbraco installation and start playing with it!</span></span>

    ![Konfigurace Umbraca – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/umbraco-config.png)
    
## <a name="next-steps"></a><span data-ttu-id="c60f0-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c60f0-159">Next steps</span></span>
* <span data-ttu-id="c60f0-160">[Nasazení aplikace tooAzure web ASP.NET služby App Service pomocí sady Visual Studio](app-service-web-get-started-dotnet.md) – zjistěte, jak toocreate nové webové aplikace Azure ze sady Visual Studio, pomocí některého z hello zahrnuté šablon aplikací.</span><span class="sxs-lookup"><span data-stu-id="c60f0-160">[Deploy an ASP.NET web app tooAzure App Service, using Visual Studio](app-service-web-get-started-dotnet.md) - Learn how toocreate a new Azure web app from Visual Studio, using any one of hello included application templates.</span></span>
* <span data-ttu-id="c60f0-161">[Nasazení vaší tooAzure kódu služby App Service](web-sites-deploy.md)– zjistěte, jak řídit toodeploy ze serveru FTP nebo ze zdroje úložiště.</span><span class="sxs-lookup"><span data-stu-id="c60f0-161">[Deploy your code tooAzure App Service](web-sites-deploy.md)- Learn how toodeploy from FTP or from source control repositories.</span></span>
* <span data-ttu-id="c60f0-162">[Přidání funkce tooyour první webové aplikace](app-service-web-get-started-2.md) -trvat vaše aplikace Azure toohello další úrovně.</span><span class="sxs-lookup"><span data-stu-id="c60f0-162">[Add functionality tooyour first web app](app-service-web-get-started-2.md) - Take your Azure app toohello next level.</span></span> <span data-ttu-id="c60f0-163">Ověřte svoje uživatele.</span><span class="sxs-lookup"><span data-stu-id="c60f0-163">Authenticate your users.</span></span> <span data-ttu-id="c60f0-164">Škálujte ji na základě poptávky.</span><span class="sxs-lookup"><span data-stu-id="c60f0-164">Scale it based on demand.</span></span> <span data-ttu-id="c60f0-165">Nastavte některá upozornění týkající se výkonu.</span><span class="sxs-lookup"><span data-stu-id="c60f0-165">Set up some performance alerts.</span></span> <span data-ttu-id="c60f0-166">To vše pomocí několika kliknutí.</span><span class="sxs-lookup"><span data-stu-id="c60f0-166">All with a few clicks.</span></span>
