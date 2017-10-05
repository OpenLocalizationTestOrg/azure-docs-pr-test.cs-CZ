---
title: "Vytvoření webové aplikace WordPress ve službě Azure App Service | Dokumentace Microsoftu"
description: "Naučte se vytvořit novou webovou aplikaci Azure pro blog WordPress pomocí webu Azure Portal."
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
ms.openlocfilehash: 460afdabed947fb4018a9ea8a7a5bc7dc5bc89c7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-wordpress-web-app-in-azure-app-service"></a><span data-ttu-id="36275-103">Vytvoření webové aplikace WordPress ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="36275-103">Create a WordPress web app in Azure App Service</span></span>
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

<span data-ttu-id="36275-104">V tomto kurzu se dozvíte, jak nasadit web blogu WordPress z Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="36275-104">This tutorial shows how to deploy a WordPress blog site from the Azure Marketplace.</span></span>

<span data-ttu-id="36275-105">Po dokončení kurzu budete mít vlastní funkční web blogu WordPress spuštěný v cloudu.</span><span class="sxs-lookup"><span data-stu-id="36275-105">When you're done with the tutorial you'll have your own WordPress blog site up and running in the cloud.</span></span>

![Web WordPress](./media/web-sites-php-web-site-gallery/wpdashboard.png)

<span data-ttu-id="36275-107">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="36275-107">You'll learn:</span></span>

* <span data-ttu-id="36275-108">Postup nalezení šablony aplikace v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="36275-108">How to find an application template in the Azure Marketplace.</span></span>
* <span data-ttu-id="36275-109">Postup vytvoření webové aplikace založené na šabloně ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="36275-109">How to create a web app in Azure App Service that is based on the template.</span></span>
* <span data-ttu-id="36275-110">Postup konfigurace nastavení služby Azure App Service pro novou webovou aplikaci a databázi.</span><span class="sxs-lookup"><span data-stu-id="36275-110">How to configure Azure App Service settings for the new web app and database.</span></span>

<span data-ttu-id="36275-111">Azure Marketplace nabízí širokou škálu oblíbených webových aplikací vyvinutých společností Microsoft, jinými společnostmi a iniciativami v oblasti softwaru Open Source.</span><span class="sxs-lookup"><span data-stu-id="36275-111">The Azure Marketplace makes available a wide range of popular web apps developed by Microsoft, third party companies, and open source software initiatives.</span></span> <span data-ttu-id="36275-112">Webové aplikace jsou postaveny na široké škále oblíbených rozhraní, jako je [PHP](/develop/nodejs/) v tomto příkladu aplikace WordPress, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/), [Python](/develop/python/) a další.</span><span class="sxs-lookup"><span data-stu-id="36275-112">The web apps are built on a wide range of popular frameworks, such as [PHP](/develop/nodejs/) in this WordPress example, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/), and [Python](/develop/python/), to name a few.</span></span> <span data-ttu-id="36275-113">K vytvoření webové aplikace v Azure Marketplace nepotřebujete žádný software, kromě prohlížeče, který používáte pro [web Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="36275-113">To create a web app from the Azure Marketplace the only software you need is the browser that you use for the [Azure Portal](https://portal.azure.com/).</span></span> 

<span data-ttu-id="36275-114">Web WordPress, který nasazujete v tomto kurzu, využívá jako databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="36275-114">The WordPress site that you deploy in this tutorial uses MySQL for the database.</span></span> <span data-ttu-id="36275-115">Chcete-li jako databázi raději použít SQL Database, informace naleznete v [Projektu Nami](http://projectnami.org/).</span><span class="sxs-lookup"><span data-stu-id="36275-115">If you wish to instead use SQL Database for the database, see [Project Nami](http://projectnami.org/).</span></span> <span data-ttu-id="36275-116">**Projekt Nami** je rovněž k dispozici prostřednictvím Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="36275-116">**Project Nami** is also available through the Marketplace.</span></span>

> [!NOTE]
> <span data-ttu-id="36275-117">K absolvování tohoto kurzu potřebujete účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="36275-117">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="36275-118">Pokud nemáte účet, můžete si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) nebo se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="36275-118">If you don't have an account, you can [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
> 
> <span data-ttu-id="36275-119">Chcete-li začít se službou Azure App Service dříve, než se zaregistrujete k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/).</span><span class="sxs-lookup"><span data-stu-id="36275-119">If you want to get started with Azure App Service before you sign up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/).</span></span> <span data-ttu-id="36275-120">Zde můžete ihned vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service – aniž by byla požadována platební karta a bez jakýchkoli závazků.</span><span class="sxs-lookup"><span data-stu-id="36275-120">There, you can immediately create a short-lived starter web app in App Service—no credit card required, and no commitments.</span></span>
> 
> 

## <a name="select-wordpress-and-configure-for-azure-app-service"></a><span data-ttu-id="36275-121">Výběr aplikace WordPress a konfigurace pro službu Azure App Service</span><span class="sxs-lookup"><span data-stu-id="36275-121">Select WordPress and configure for Azure App Service</span></span>
1. <span data-ttu-id="36275-122">Přihlaste se k [webu Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="36275-122">Log in to the [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="36275-123">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="36275-123">Click **New**.</span></span>
   
    ![Vytvořit nové][5]
3. <span data-ttu-id="36275-125">Vyhledejte **WordPress** a klikněte na možnost **WordPress**.</span><span class="sxs-lookup"><span data-stu-id="36275-125">Search for **WordPress**, and then click **WordPress**.</span></span> <span data-ttu-id="36275-126">Chcete-li namísto MySQL použít SQL Database, vyhledejte **Projekt Nami**.</span><span class="sxs-lookup"><span data-stu-id="36275-126">If you wish to use SQL Database instead of MySQL, search for **Project Nami**.</span></span>
   
    ![WordPress ze seznamu][7]
4. <span data-ttu-id="36275-128">Přečtěte si popis aplikace WordPress a klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="36275-128">After reading the description of the WordPress app, click **Create**.</span></span>
   
    ![Vytvořit](./media/web-sites-php-web-site-gallery/create.png)
5. <span data-ttu-id="36275-130">Zadejte název webové aplikace do pole **Webová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="36275-130">Enter a name for the web app in the **Web app** box.</span></span>
   
    <span data-ttu-id="36275-131">Tento název musí být v doméně azurewebsites.net jedinečný, protože webová aplikace bude mít adresu URL {název}.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="36275-131">This name must be unique in the azurewebsites.net domain because the URL of the web app will be {name}.azurewebsites.net.</span></span> <span data-ttu-id="36275-132">Není-li zadaný název jedinečný, v textovém poli se zobrazí červený vykřičník.</span><span class="sxs-lookup"><span data-stu-id="36275-132">If the name you enter isn't unique, a red exclamation mark appears in the text box.</span></span>
6. <span data-ttu-id="36275-133">Máte-li více předplatných, zvolte to, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="36275-133">If you have more than one subscription, choose the one you want to use.</span></span> 
7. <span data-ttu-id="36275-134">Vyberte **skupinu prostředků** nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="36275-134">Select a **Resource Group** or create a new one.</span></span>
   
    <span data-ttu-id="36275-135">Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="36275-135">For more information about resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
8. <span data-ttu-id="36275-136">Vyberte **umístění/plán služby App Service** nebo vytvořte nové.</span><span class="sxs-lookup"><span data-stu-id="36275-136">Select an **App Service plan/Location** or create a new one.</span></span>
   
    <span data-ttu-id="36275-137">Podrobnější informace o plánech služby App Service naleznete v tématu [Přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="36275-137">For more information about App Service plans, see [Azure App Service plans overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</span></span>    
9. <span data-ttu-id="36275-138">Klikněte na možnost **Databáze** a poté v okně **Nová databáze MySQL** zadejte požadované hodnoty pro konfiguraci databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="36275-138">Click **Database**, and then in the **New MySQL Database** blade provide the required values for configuring your MySQL database.</span></span>
   
    <span data-ttu-id="36275-139">a.</span><span class="sxs-lookup"><span data-stu-id="36275-139">a.</span></span> <span data-ttu-id="36275-140">Zadejte nový název nebo ponechte výchozí název.</span><span class="sxs-lookup"><span data-stu-id="36275-140">Enter a new name or leave the default name.</span></span>
   
    <span data-ttu-id="36275-141">b.</span><span class="sxs-lookup"><span data-stu-id="36275-141">b.</span></span> <span data-ttu-id="36275-142">Položku **Typ databáze** ponechte nastavenou na možnost **Shared**.</span><span class="sxs-lookup"><span data-stu-id="36275-142">Leave the **Database Type** set to **Shared**.</span></span>
   
    <span data-ttu-id="36275-143">c.</span><span class="sxs-lookup"><span data-stu-id="36275-143">c.</span></span> <span data-ttu-id="36275-144">Zvolte totéž umístění, které jste zvolili pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="36275-144">Choose the same location as the one you chose for the web app.</span></span>
   
    <span data-ttu-id="36275-145">d.</span><span class="sxs-lookup"><span data-stu-id="36275-145">d.</span></span> <span data-ttu-id="36275-146">Zvolte cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="36275-146">Choose a pricing tier.</span></span> <span data-ttu-id="36275-147">Mercury (bezplatná úroveň s minimálním počtem povolených připojení a místem na disku) je vhodná pro tento kurz.</span><span class="sxs-lookup"><span data-stu-id="36275-147">Mercury (free with minimal allowed connections and disk space) is fine for this tutorial.</span></span>
10. <span data-ttu-id="36275-148">V okně **Nová databáze MySQL** klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="36275-148">In the **New MySQL Database** blade, click **OK**.</span></span> 
11. <span data-ttu-id="36275-149">V okně **WordPress** přijměte právní podmínky a klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="36275-149">In the **WordPress** blade, accept the legal terms, and then click **Create**.</span></span> 
    
     ![Konfigurace webové aplikace](./media/web-sites-php-web-site-gallery/configure.png)
    
     <span data-ttu-id="36275-151">Služba Azure App Service vytvoří webovou aplikaci, což obvykle trvá méně než minutu.</span><span class="sxs-lookup"><span data-stu-id="36275-151">Azure App Service creates the web app, typically in less than a minute.</span></span> <span data-ttu-id="36275-152">Průběh můžete sledovat kliknutím na ikonu zvonu v horní části stránky portálu.</span><span class="sxs-lookup"><span data-stu-id="36275-152">You can watch the progress by clicking the bell icon at the top of the portal page.</span></span>
    
     ![Ukazatel průběhu](./media/web-sites-php-web-site-gallery/progress.png)

## <a name="launch-and-manage-your-wordpress-web-app"></a><span data-ttu-id="36275-154">Spuštění a správa webové aplikace WordPress</span><span class="sxs-lookup"><span data-stu-id="36275-154">Launch and manage your WordPress web app</span></span>
1. <span data-ttu-id="36275-155">Až budete s vytvářením webové aplikace hotovi, přejděte na webu Azure Portal do skupiny prostředků, v níž jste aplikaci vytvořili, a zde se zobrazí webová aplikace a databáze.</span><span class="sxs-lookup"><span data-stu-id="36275-155">When the web app creation is finished, navigate in the Azure Portal to the resource group in which you created the application, and you can see the web app and the database.</span></span>
   
    <span data-ttu-id="36275-156">Dodatečný prostředek s ikonou žárovky je služba [Application Insights](/services/application-insights/), která pro vaši webovou aplikaci zajišťuje služby monitorování.</span><span class="sxs-lookup"><span data-stu-id="36275-156">The extra resource with the light bulb icon is [Application Insights](/services/application-insights/), which provides monitoring services for your web app.</span></span>
2. <span data-ttu-id="36275-157">V okně **Skupina prostředků** klikněte na řádek webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="36275-157">In the **Resource group** blade, click the web app line.</span></span>
   
    ![Konfigurace webové aplikace](./media/web-sites-php-web-site-gallery/resourcegroup.png)
3. <span data-ttu-id="36275-159">V okně webové aplikace klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="36275-159">In the Web app blade, click **Browse**.</span></span>
   
    ![adresa URL webu][browse]
4. <span data-ttu-id="36275-161">Na **úvodní** stránce WordPress zadejte informace o konfiguraci vyžadované aplikací WordPress a klikněte na možnost **Instalovat WordPress**.</span><span class="sxs-lookup"><span data-stu-id="36275-161">In the WordPress **Welcome** page, enter the configuration information required by WordPress, and then click **Install WordPress**.</span></span>
   
    ![Konfigurace WordPress](./media/web-sites-php-web-site-gallery/wpconfigure.png)
5. <span data-ttu-id="36275-163">Přihlaste se pomocí přihlašovacích údajů, které jste vytvořili na **úvodní** stránce.</span><span class="sxs-lookup"><span data-stu-id="36275-163">Log in using the credentials you created on the **Welcome** page.</span></span>  
6. <span data-ttu-id="36275-164">Otevře se stránka řídicího panelu webu.</span><span class="sxs-lookup"><span data-stu-id="36275-164">Your site Dashboard page opens.</span></span>    
   
    ![Web WordPress](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## <a name="next-steps"></a><span data-ttu-id="36275-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36275-166">Next steps</span></span>
<span data-ttu-id="36275-167">Seznámili jste se s postupem vytvoření a nasazení webové aplikace PHP z galerie.</span><span class="sxs-lookup"><span data-stu-id="36275-167">You've seen how to create and deploy a PHP web app from the gallery.</span></span> <span data-ttu-id="36275-168">Další informace o použití PHP v Azure naleznete ve [Středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="36275-168">For more information about using PHP in Azure, see the [PHP Developer Center](/develop/php/).</span></span>

<span data-ttu-id="36275-169">Další informace týkající se práce s webovými aplikacemi App Service Web Apps naleznete prostřednictvím odkazů v levé části stránky (máte-li široké okno prohlížeče) nebo v horní části stránky (máte-li úzké okno prohlížeče).</span><span class="sxs-lookup"><span data-stu-id="36275-169">For more information about how to work with App Service Web Apps, see the links on the left side of the page (for wide browser windows) or at the top of the page (for narrow browser windows).</span></span> 

## <a name="whats-changed"></a><span data-ttu-id="36275-170">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="36275-170">What's changed</span></span>
* <span data-ttu-id="36275-171">Průvodce změnou z webů na službu App Service naleznete v tématu [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="36275-171">For a guide to the change from Websites to App Service, see [Azure App Service and its impact on existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[browse]: ./media/web-sites-php-web-site-gallery/browse-web.png
