---
title: "aaaCreate webové aplikace WordPress v Azure App Service | Microsoft Docs"
description: "Zjistěte, jak toocreate nový Azure webové aplikace pro blog WordPress pomocí portálu Azure hello."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 193ae094-0d7c-4749-a09b-ff4b1240149e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 3a95fcb6732c15a8200921ce474b6dde2298dec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-wordpress-web-app-in-azure-app-service"></a><span data-ttu-id="e63ce-103">Vytvoření webové aplikace WordPress ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e63ce-103">Create a WordPress web app in Azure App Service</span></span>
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

<span data-ttu-id="e63ce-104">Tento kurz ukazuje, jak lokality toodeploy blogu WordPress z Azure Marketplace hello.</span><span class="sxs-lookup"><span data-stu-id="e63ce-104">This tutorial shows how toodeploy a WordPress blog site from hello Azure Marketplace.</span></span>

<span data-ttu-id="e63ce-105">Když jste hotovi s hello kurzu budete mít vlastní funkční web blogu WordPress a spouštění v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="e63ce-105">When you're done with hello tutorial you'll have your own WordPress blog site up and running in hello cloud.</span></span>

![Web WordPress](./media/web-sites-php-web-site-gallery/wpdashboard.png)

<span data-ttu-id="e63ce-107">Naučíte se:</span><span class="sxs-lookup"><span data-stu-id="e63ce-107">You'll learn:</span></span>

* <span data-ttu-id="e63ce-108">Jak toofind šablony aplikace v hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="e63ce-108">How toofind an application template in hello Azure Marketplace.</span></span>
* <span data-ttu-id="e63ce-109">Jak toocreate webové aplikace v Azure aplikaci služby, který je založený na šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="e63ce-109">How toocreate a web app in Azure App Service that is based on hello template.</span></span>
* <span data-ttu-id="e63ce-110">Jak tooconfigure nastavení služby Azure App Service pro hello nové webové aplikace a databáze.</span><span class="sxs-lookup"><span data-stu-id="e63ce-110">How tooconfigure Azure App Service settings for hello new web app and database.</span></span>

<span data-ttu-id="e63ce-111">Hello Azure Marketplace zpřístupní širokou škálu oblíbených webových aplikací vyvinutých společností Microsoft, jinými společnostmi a iniciativami v oblasti softwaru s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="e63ce-111">hello Azure Marketplace makes available a wide range of popular web apps developed by Microsoft, third party companies, and open source software initiatives.</span></span> <span data-ttu-id="e63ce-112">Hello webové aplikace jsou postaveny na široké škále oblíbených rozhraní, jako například [PHP](/develop/nodejs/) v tomto příkladu aplikace WordPress, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/), a [Python](/develop/python/), tooname a několika.</span><span class="sxs-lookup"><span data-stu-id="e63ce-112">hello web apps are built on a wide range of popular frameworks, such as [PHP](/develop/nodejs/) in this WordPress example, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/), and [Python](/develop/python/), tooname a few.</span></span> <span data-ttu-id="e63ce-113">toocreate webové aplikace z Azure Marketplace hello pouze softwaru je nutné je hello prohlížeče, který používáte pro hello hello [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e63ce-113">toocreate a web app from hello Azure Marketplace hello only software you need is hello browser that you use for hello [Azure Portal](https://portal.azure.com/).</span></span> 

<span data-ttu-id="e63ce-114">Hello web WordPress, který nasazujete v tomto kurzu využívá hello databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="e63ce-114">hello WordPress site that you deploy in this tutorial uses MySQL for hello database.</span></span> <span data-ttu-id="e63ce-115">Pokud chcete použít tooinstead databázi SQL pro databázi hello, najdete v části [Project Nami](http://projectnami.org/).</span><span class="sxs-lookup"><span data-stu-id="e63ce-115">If you wish tooinstead use SQL Database for hello database, see [Project Nami](http://projectnami.org/).</span></span> <span data-ttu-id="e63ce-116">**Projekt Nami** je také k dispozici prostřednictvím hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="e63ce-116">**Project Nami** is also available through hello Marketplace.</span></span>

> [!NOTE]
> <span data-ttu-id="e63ce-117">toocomplete tohoto kurzu potřebujete účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="e63ce-117">toocomplete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="e63ce-118">Pokud nemáte účet, můžete si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) nebo se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="e63ce-118">If you don't have an account, you can [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
> 
> <span data-ttu-id="e63ce-119">Pokud chcete, aby tooget začít s Azure App Service, ještě než si zaregistrujete účet Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/).</span><span class="sxs-lookup"><span data-stu-id="e63ce-119">If you want tooget started with Azure App Service before you sign up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/).</span></span> <span data-ttu-id="e63ce-120">Zde můžete ihned vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service – aniž by byla požadována platební karta a bez jakýchkoli závazků.</span><span class="sxs-lookup"><span data-stu-id="e63ce-120">There, you can immediately create a short-lived starter web app in App Service—no credit card required, and no commitments.</span></span>
> 
> 

## <a name="select-wordpress-and-configure-for-azure-app-service"></a><span data-ttu-id="e63ce-121">Výběr aplikace WordPress a konfigurace pro službu Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e63ce-121">Select WordPress and configure for Azure App Service</span></span>
1. <span data-ttu-id="e63ce-122">Přihlaste se toohello [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e63ce-122">Log in toohello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e63ce-123">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="e63ce-123">Click **New**.</span></span>
   
    ![Vytvořit nové][5]
3. <span data-ttu-id="e63ce-125">Vyhledejte **WordPress** a klikněte na možnost **WordPress**.</span><span class="sxs-lookup"><span data-stu-id="e63ce-125">Search for **WordPress**, and then click **WordPress**.</span></span> <span data-ttu-id="e63ce-126">Pokud chcete toouse SQL Database namísto MySQL, vyhledejte **Project Nami**.</span><span class="sxs-lookup"><span data-stu-id="e63ce-126">If you wish toouse SQL Database instead of MySQL, search for **Project Nami**.</span></span>
   
    ![WordPress ze seznamu][7]
4. <span data-ttu-id="e63ce-128">Po přečtení hello popis aplikace WordPress hello, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e63ce-128">After reading hello description of hello WordPress app, click **Create**.</span></span>
   
    ![Vytvořit](./media/web-sites-php-web-site-gallery/create.png)
5. <span data-ttu-id="e63ce-130">Zadejte název webové aplikace hello v hello **webové aplikace** pole.</span><span class="sxs-lookup"><span data-stu-id="e63ce-130">Enter a name for hello web app in hello **Web app** box.</span></span>
   
    <span data-ttu-id="e63ce-131">Tento název musí být jedinečný v doméně azurewebsites.net hello, protože adresa URL hello hello webové aplikace bude {name}. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="e63ce-131">This name must be unique in hello azurewebsites.net domain because hello URL of hello web app will be {name}.azurewebsites.net.</span></span> <span data-ttu-id="e63ce-132">Pokud není jedinečný název hello, kterou zadáte, se zobrazí červený vykřičník hello textového pole.</span><span class="sxs-lookup"><span data-stu-id="e63ce-132">If hello name you enter isn't unique, a red exclamation mark appears in hello text box.</span></span>
6. <span data-ttu-id="e63ce-133">Pokud máte více než jedno předplatné, zvolte hello, kterou chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="e63ce-133">If you have more than one subscription, choose hello one you want toouse.</span></span> 
7. <span data-ttu-id="e63ce-134">Vyberte **skupinu prostředků** nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="e63ce-134">Select a **Resource Group** or create a new one.</span></span>
   
    <span data-ttu-id="e63ce-135">Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e63ce-135">For more information about resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
8. <span data-ttu-id="e63ce-136">Vyberte **umístění/plán služby App Service** nebo vytvořte nové.</span><span class="sxs-lookup"><span data-stu-id="e63ce-136">Select an **App Service plan/Location** or create a new one.</span></span>
   
    <span data-ttu-id="e63ce-137">Podrobnější informace o plánech služby App Service naleznete v tématu [Přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e63ce-137">For more information about App Service plans, see [Azure App Service plans overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</span></span>    
9. <span data-ttu-id="e63ce-138">Klikněte na tlačítko **databáze**a potom v hello **nová databáze MySQL** okno Zadejte hello požadované hodnoty pro konfiguraci databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="e63ce-138">Click **Database**, and then in hello **New MySQL Database** blade provide hello required values for configuring your MySQL database.</span></span>
   
    <span data-ttu-id="e63ce-139">a.</span><span class="sxs-lookup"><span data-stu-id="e63ce-139">a.</span></span> <span data-ttu-id="e63ce-140">Zadejte nový název nebo ponechte výchozí název hello.</span><span class="sxs-lookup"><span data-stu-id="e63ce-140">Enter a new name or leave hello default name.</span></span>
   
    <span data-ttu-id="e63ce-141">b.</span><span class="sxs-lookup"><span data-stu-id="e63ce-141">b.</span></span> <span data-ttu-id="e63ce-142">Nechte hello **typ databáze** nastavit příliš**sdílené**.</span><span class="sxs-lookup"><span data-stu-id="e63ce-142">Leave hello **Database Type** set too**Shared**.</span></span>
   
    <span data-ttu-id="e63ce-143">c.</span><span class="sxs-lookup"><span data-stu-id="e63ce-143">c.</span></span> <span data-ttu-id="e63ce-144">Zvolte hello, které stejné umístění jako hello ten, který jste zvolili pro webovou aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="e63ce-144">Choose hello same location as hello one you chose for hello web app.</span></span>
   
    <span data-ttu-id="e63ce-145">d.</span><span class="sxs-lookup"><span data-stu-id="e63ce-145">d.</span></span> <span data-ttu-id="e63ce-146">Zvolte cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="e63ce-146">Choose a pricing tier.</span></span> <span data-ttu-id="e63ce-147">Mercury (bezplatná úroveň s minimálním počtem povolených připojení a místem na disku) je vhodná pro tento kurz.</span><span class="sxs-lookup"><span data-stu-id="e63ce-147">Mercury (free with minimal allowed connections and disk space) is fine for this tutorial.</span></span>
10. <span data-ttu-id="e63ce-148">V hello **nová databáze MySQL** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e63ce-148">In hello **New MySQL Database** blade, click **OK**.</span></span> 
11. <span data-ttu-id="e63ce-149">V hello **WordPress** přijměte právní podmínky hello a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e63ce-149">In hello **WordPress** blade, accept hello legal terms, and then click **Create**.</span></span> 
    
     ![Konfigurace webové aplikace](./media/web-sites-php-web-site-gallery/configure.png)
    
     <span data-ttu-id="e63ce-151">Aplikační služba Azure vytvoří hello webové aplikace, obvykle v méně než minutu.</span><span class="sxs-lookup"><span data-stu-id="e63ce-151">Azure App Service creates hello web app, typically in less than a minute.</span></span> <span data-ttu-id="e63ce-152">Kliknutím na ikonu zvonku hello hello horní části stránky portálu hello můžete sledovat průběh hello.</span><span class="sxs-lookup"><span data-stu-id="e63ce-152">You can watch hello progress by clicking hello bell icon at hello top of hello portal page.</span></span>
    
     ![Ukazatel průběhu](./media/web-sites-php-web-site-gallery/progress.png)

## <a name="launch-and-manage-your-wordpress-web-app"></a><span data-ttu-id="e63ce-154">Spuštění a správa webové aplikace WordPress</span><span class="sxs-lookup"><span data-stu-id="e63ce-154">Launch and manage your WordPress web app</span></span>
1. <span data-ttu-id="e63ce-155">Po dokončení vytvoření webové aplikace hello přejděte v hello portálu Azure toohello skupině prostředků ve které jste vytvořili hello aplikace, a uvidíte hello webovou aplikaci a databázi hello.</span><span class="sxs-lookup"><span data-stu-id="e63ce-155">When hello web app creation is finished, navigate in hello Azure Portal toohello resource group in which you created hello application, and you can see hello web app and hello database.</span></span>
   
    <span data-ttu-id="e63ce-156">Hello dodatečný prostředek s ikonou žárovky hello je [Application Insights](/services/application-insights/), která zajišťuje služby monitorování pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e63ce-156">hello extra resource with hello light bulb icon is [Application Insights](/services/application-insights/), which provides monitoring services for your web app.</span></span>
2. <span data-ttu-id="e63ce-157">V hello **skupiny prostředků** okně klikněte na řádek webové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e63ce-157">In hello **Resource group** blade, click hello web app line.</span></span>
   
    ![Konfigurace webové aplikace](./media/web-sites-php-web-site-gallery/resourcegroup.png)
3. <span data-ttu-id="e63ce-159">V okně webové aplikace hello, klikněte na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="e63ce-159">In hello Web app blade, click **Browse**.</span></span>
   
    ![adresa URL webu][browse]
4. <span data-ttu-id="e63ce-161">V hello WordPress **úvodní** zadejte informace o konfiguraci hello vyžadované aplikací WordPress a pak klikněte na tlačítko **instalovat WordPress**.</span><span class="sxs-lookup"><span data-stu-id="e63ce-161">In hello WordPress **Welcome** page, enter hello configuration information required by WordPress, and then click **Install WordPress**.</span></span>
   
    ![Konfigurace WordPress](./media/web-sites-php-web-site-gallery/wpconfigure.png)
5. <span data-ttu-id="e63ce-163">Přihlaste se pomocí přihlašovacích údajů hello jste vytvořili na hello **úvodní** stránky.</span><span class="sxs-lookup"><span data-stu-id="e63ce-163">Log in using hello credentials you created on hello **Welcome** page.</span></span>  
6. <span data-ttu-id="e63ce-164">Otevře se stránka řídicího panelu webu.</span><span class="sxs-lookup"><span data-stu-id="e63ce-164">Your site Dashboard page opens.</span></span>    
   
    ![Web WordPress](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## <a name="next-steps"></a><span data-ttu-id="e63ce-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e63ce-166">Next steps</span></span>
<span data-ttu-id="e63ce-167">Už víte, jak toocreate a nasazení webové aplikace PHP z Galerie hello.</span><span class="sxs-lookup"><span data-stu-id="e63ce-167">You've seen how toocreate and deploy a PHP web app from hello gallery.</span></span> <span data-ttu-id="e63ce-168">Další informace o použití PHP v Azure najdete v tématu hello [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="e63ce-168">For more information about using PHP in Azure, see hello [PHP Developer Center](/develop/php/).</span></span>

<span data-ttu-id="e63ce-169">Další informace o tom, toowork s App Service Web Apps, najdete v části hello odkazy na levé straně stránky hello (pro široké okno prohlížeče) hello nebo hello horní části stránky hello (pro úzké okno prohlížeče).</span><span class="sxs-lookup"><span data-stu-id="e63ce-169">For more information about how toowork with App Service Web Apps, see hello links on hello left side of hello page (for wide browser windows) or at hello top of hello page (for narrow browser windows).</span></span> 

## <a name="whats-changed"></a><span data-ttu-id="e63ce-170">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="e63ce-170">What's changed</span></span>
* <span data-ttu-id="e63ce-171">Průvodce toohello změnu z tooApp weby služby, najdete v části [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="e63ce-171">For a guide toohello change from Websites tooApp Service, see [Azure App Service and its impact on existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[browse]: ./media/web-sites-php-web-site-gallery/browse-web.png
