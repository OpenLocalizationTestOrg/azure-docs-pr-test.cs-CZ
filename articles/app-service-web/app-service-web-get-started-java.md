---
title: "aaaCreate první webové aplikace Java v Azure"
description: "Zjistěte, jak toorun webové aplikace ve službě App Service pomocí nasazení základní aplikace v jazyce Java."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 6/7/2017
ms.author: cephalin;robmcm
ms.custom: mvc
ms.openlocfilehash: 81315c07b5aa84cbec50a17b2cb3914927b19c00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-java-web-app-in-azure"></a><span data-ttu-id="8a23e-103">Vytvoření první webové Java aplikace ve službě Azure</span><span class="sxs-lookup"><span data-stu-id="8a23e-103">Create your first Java web app in Azure</span></span>

<span data-ttu-id="8a23e-104">Hello [webové aplikace](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) funkce [Azure App Service](../app-service/app-service-value-prop-what-is.md) nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby.</span><span class="sxs-lookup"><span data-stu-id="8a23e-104">hello [Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) feature of [Azure App Service](../app-service/app-service-value-prop-what-is.md) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="8a23e-105">Tento rychlý start ukazuje, jak toodeploy Java webové aplikace tooApp služby pomocí hello [IDE Eclipse pro vývojáře v jazyce Java EE](http://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="8a23e-105">This quickstart shows how toodeploy a Java web app tooApp Service by using hello [Eclipse IDE for Java EE Developers](http://www.eclipse.org/).</span></span>

![Hello Azure!](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="prerequisites"></a><span data-ttu-id="8a23e-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8a23e-108">Prerequisites</span></span>

<span data-ttu-id="8a23e-109">toocomplete tento rychlý start, nainstalovat:</span><span class="sxs-lookup"><span data-stu-id="8a23e-109">toocomplete this quickstart, install:</span></span>

* <span data-ttu-id="8a23e-110">Hello volné [IDE Eclipse pro vývojáře v jazyce Java EE](http://www.eclipse.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="8a23e-110">hello free [Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/).</span></span> <span data-ttu-id="8a23e-111">Tento kurz Rychlý start používá Eclipse Neon.</span><span class="sxs-lookup"><span data-stu-id="8a23e-111">This quickstart uses Eclipse Neon.</span></span>
* <span data-ttu-id="8a23e-112">Hello [nástrojů Azure pro Eclipse](/azure/azure-toolkit-for-eclipse-installation).</span><span class="sxs-lookup"><span data-stu-id="8a23e-112">hello [Azure Toolkit for Eclipse](/azure/azure-toolkit-for-eclipse-installation).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-dynamic-web-project-in-eclipse"></a><span data-ttu-id="8a23e-113">Vytvoření dynamického webového projektu v Eclipse</span><span class="sxs-lookup"><span data-stu-id="8a23e-113">Create a dynamic web project in Eclipse</span></span>

<span data-ttu-id="8a23e-114">V prostředí Eclipse vyberte **File** (Soubor) > **New** (Nový) > **Dynamic Web Project** (Dynamický webový projekt).</span><span class="sxs-lookup"><span data-stu-id="8a23e-114">In Eclipse, select **File** > **New** > **Dynamic Web Project**.</span></span>

<span data-ttu-id="8a23e-115">V hello **nové Dynamic Web Project** dialogové okno, název projektu hello **MyFirstJavaOnAzureWebApp**a vyberte **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="8a23e-115">In hello **New Dynamic Web Project** dialog box, name hello project **MyFirstJavaOnAzureWebApp**, and select **Finish**.</span></span>
   
![Dialogové okno New Dynamic Web Project (Nový dynamický webový projekt)](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### <a name="add-a-jsp-page"></a><span data-ttu-id="8a23e-117">Přidání stránky JSP</span><span class="sxs-lookup"><span data-stu-id="8a23e-117">Add a JSP page</span></span>

<span data-ttu-id="8a23e-118">Pokud se nezobrazí Project Explorer (Prohlížeč projektu), obnovte zobrazení.</span><span class="sxs-lookup"><span data-stu-id="8a23e-118">If Project Explorer is not displayed, restore it.</span></span>

![Pracovní prostor Java EE pro Eclipse](./media/app-service-web-get-started-java/pe.png)

<span data-ttu-id="8a23e-120">V prohlížeči projektu rozbalte hello **MyFirstJavaOnAzureWebApp** projektu.</span><span class="sxs-lookup"><span data-stu-id="8a23e-120">In Project Explorer, expand hello **MyFirstJavaOnAzureWebApp** project.</span></span>
<span data-ttu-id="8a23e-121">Klikněte pravým tlačítkem na **WebContent** (Webový obsah) a potom vyberte **New** (Nový) > **JSP File** (Soubor JSP).</span><span class="sxs-lookup"><span data-stu-id="8a23e-121">Right-click **WebContent**, and then select **New** > **JSP File**.</span></span>

![Nabídka pro nový soubor JSP v prohlížeči Project Explorer](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

<span data-ttu-id="8a23e-123">V hello **nový soubor JSP** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="8a23e-123">In hello **New JSP File** dialog box:</span></span>

* <span data-ttu-id="8a23e-124">Název souboru hello **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="8a23e-124">Name hello file **index.jsp**.</span></span>
* <span data-ttu-id="8a23e-125">Vyberte **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="8a23e-125">Select **Finish**.</span></span>

  ![Dialogové okno New JSP File (Nový soubor JSP)](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

<span data-ttu-id="8a23e-127">V souboru index.jsp hello nahraďte hello `<body></body>` element s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="8a23e-127">In hello index.jsp file, replace hello `<body></body>` element with hello following markup:</span></span>

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

<span data-ttu-id="8a23e-128">Uložte změny hello.</span><span class="sxs-lookup"><span data-stu-id="8a23e-128">Save hello changes.</span></span>

## <a name="publish-hello-web-app-tooazure"></a><span data-ttu-id="8a23e-129">Publikování hello webové aplikace tooAzure</span><span class="sxs-lookup"><span data-stu-id="8a23e-129">Publish hello web app tooAzure</span></span>

<span data-ttu-id="8a23e-130">V prohlížeči projektu klikněte pravým tlačítkem na projekt hello a pak vyberte **Azure** > **publikovat jako webové aplikace Azure**.</span><span class="sxs-lookup"><span data-stu-id="8a23e-130">In Project Explorer, right-click hello project, and then select **Azure** > **Publish as Azure Web App**.</span></span>

![Místní nabídka Publish as Azure Web App (Publikovat jako webovou aplikaci Azure)](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

<span data-ttu-id="8a23e-132">V hello **přihlásit k Azure** dialogové okno, udržování hello **interaktivní** a pak vyberte možnost **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="8a23e-132">In hello **Azure Sign In** dialog box, keep hello **Interactive** option, and then select **Sign in**.</span></span>

<span data-ttu-id="8a23e-133">Podle pokynů hello přihlášení.</span><span class="sxs-lookup"><span data-stu-id="8a23e-133">Follow hello sign-in instructions.</span></span>

### <a name="deploy-web-app-dialog-box"></a><span data-ttu-id="8a23e-134">Dialogové okno Deploy Web App (Nasazení webové aplikace)</span><span class="sxs-lookup"><span data-stu-id="8a23e-134">Deploy Web App dialog box</span></span>

<span data-ttu-id="8a23e-135">Po přihlášení tooyour účet Azure hello **nasadit webovou aplikaci** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8a23e-135">After you have signed in tooyour Azure account, hello **Deploy Web App** dialog box appears.</span></span>

<span data-ttu-id="8a23e-136">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8a23e-136">Select **Create**.</span></span>

![Dialogové okno Deploy Web App (Nasazení webové aplikace)](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### <a name="create-app-service-dialog-box"></a><span data-ttu-id="8a23e-138">Dialogové okno Create App Service (Vytvoření služby App Service)</span><span class="sxs-lookup"><span data-stu-id="8a23e-138">Create App Service dialog box</span></span>

<span data-ttu-id="8a23e-139">Hello **vytvořit službu App Service** dialogové okno s výchozími hodnotami.</span><span class="sxs-lookup"><span data-stu-id="8a23e-139">hello **Create App Service** dialog box appears with default values.</span></span> <span data-ttu-id="8a23e-140">Hello číslo **170602185241** ukazuje hello následující bitové kopie se ve vašem dialogovém liší.</span><span class="sxs-lookup"><span data-stu-id="8a23e-140">hello number **170602185241** shown in hello following image is different in your dialog box.</span></span>

![Dialogové okno Create App Service (Vytvoření služby App Service)](./media/app-service-web-get-started-java/cas1.png)

<span data-ttu-id="8a23e-142">V hello **vytvořit službu App Service** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="8a23e-142">In hello **Create App Service** dialog box:</span></span>

* <span data-ttu-id="8a23e-143">Zachovejte hello vygeneruje název hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a23e-143">Keep hello generated name for hello web app.</span></span> <span data-ttu-id="8a23e-144">Tento název musí být v rámci služby Azure jedinečný.</span><span class="sxs-lookup"><span data-stu-id="8a23e-144">This name must be unique across Azure.</span></span> <span data-ttu-id="8a23e-145">Název Hello je součástí hello adresu URL pro webovou aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="8a23e-145">hello name is part of hello URL address for hello web app.</span></span> <span data-ttu-id="8a23e-146">Například: Pokud je název webové aplikace hello **MyJavaWebApp**, adresa URL je hello *myjavawebapp.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="8a23e-146">For example: if hello web app name is **MyJavaWebApp**, hello URL is *myjavawebapp.azurewebsites.net*.</span></span>
* <span data-ttu-id="8a23e-147">Zachovat hello výchozí webový kontejner.</span><span class="sxs-lookup"><span data-stu-id="8a23e-147">Keep hello default web container.</span></span>
* <span data-ttu-id="8a23e-148">Vyberte předplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="8a23e-148">Select an Azure subscription.</span></span>
* <span data-ttu-id="8a23e-149">Na hello **plán služby App service** karty:</span><span class="sxs-lookup"><span data-stu-id="8a23e-149">On hello **App service plan** tab:</span></span>

  * <span data-ttu-id="8a23e-150">**Vytvořit nový**: zachovat hello výchozí, což je název hello hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="8a23e-150">**Create new**: Keep hello default, which is hello name of hello App Service plan.</span></span>
  * <span data-ttu-id="8a23e-151">**Location** (Umístění): Vyberte **West Europe** (Západní Evropa) nebo umístění ve vaší blízkosti.</span><span class="sxs-lookup"><span data-stu-id="8a23e-151">**Location**: Select **West Europe** or a location near you.</span></span>
  * <span data-ttu-id="8a23e-152">**Cenová úroveň**: Vyberte hello bezplatnou možností.</span><span class="sxs-lookup"><span data-stu-id="8a23e-152">**Pricing tier**: Select hello free option.</span></span> <span data-ttu-id="8a23e-153">Informace o funkcích najdete na stránce [App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="8a23e-153">For features, see [App Service pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>

   ![Dialogové okno Create App Service (Vytvoření služby App Service)](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### <a name="resource-group-tab"></a><span data-ttu-id="8a23e-155">Karta Resource group (Skupina prostředků)</span><span class="sxs-lookup"><span data-stu-id="8a23e-155">Resource group tab</span></span>

<span data-ttu-id="8a23e-156">Vyberte hello **skupiny prostředků** kartě. Zachovat hello výchozí generovaný hodnotu pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="8a23e-156">Select hello **Resource group** tab. Keep hello default generated value for hello resource group.</span></span>

![Karta Resource group (Skupina prostředků)](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

<span data-ttu-id="8a23e-158">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8a23e-158">Select **Create**.</span></span>

<!--
### hello JDK tab

Select hello **JDK** tab. Keep hello default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

<span data-ttu-id="8a23e-159">Hello nástrojů Azure vytvoří hello webovou aplikaci a zobrazí dialogové okno průběhu.</span><span class="sxs-lookup"><span data-stu-id="8a23e-159">hello Azure Toolkit creates hello web app and displays a progress dialog box.</span></span>

![Dialogové okno Create App Service Progress (Průběh vytváření plánu služby App Service)](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### <a name="deploy-web-app-dialog-box"></a><span data-ttu-id="8a23e-161">Dialogové okno Deploy Web App (Nasazení webové aplikace)</span><span class="sxs-lookup"><span data-stu-id="8a23e-161">Deploy Web App dialog box</span></span>

<span data-ttu-id="8a23e-162">V hello **nasadit webovou aplikaci** dialogové okno, vyberte **nasazení tooroot**.</span><span class="sxs-lookup"><span data-stu-id="8a23e-162">In hello **Deploy Web App** dialog box, select **Deploy tooroot**.</span></span> <span data-ttu-id="8a23e-163">Pokud máte aplikační službu v *wingtiptoys.azurewebsites.net* a nenasazujte toohello kořenová hello webovou aplikaci s názvem **MyFirstJavaOnAzureWebApp** nasazuje příliš *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="8a23e-163">If you have an app service at *wingtiptoys.azurewebsites.net* and you do not deploy toohello root, hello web app named **MyFirstJavaOnAzureWebApp** is deployed too*wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.</span></span>

![Dialogové okno Deploy Web App (Nasazení webové aplikace)](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

<span data-ttu-id="8a23e-165">Hello dialogové okno pole ukazuje hello Azure, JDK a výběr webového kontejneru.</span><span class="sxs-lookup"><span data-stu-id="8a23e-165">hello dialog box shows hello Azure, JDK, and web container selections.</span></span>

<span data-ttu-id="8a23e-166">Vyberte **nasadit** toopublish hello webové aplikace tooAzure.</span><span class="sxs-lookup"><span data-stu-id="8a23e-166">Select **Deploy** toopublish hello web app tooAzure.</span></span>

<span data-ttu-id="8a23e-167">Po dokončení publikování hello vyberte hello **publikováno** odkaz v hello **protokol činnosti Azure** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8a23e-167">When hello publishing finishes, select hello **Published** link in hello **Azure Activity Log** dialog box.</span></span>

![Dialogové okno Protokol aktivit v Azure](./media/app-service-web-get-started-java/aal.png)

<span data-ttu-id="8a23e-169">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="8a23e-169">Congratulations!</span></span> <span data-ttu-id="8a23e-170">Úspěšně jste nasadili tooAzure vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a23e-170">You have successfully deployed your web app tooAzure.</span></span> 

![Hello Azure!](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="update-hello-web-app"></a><span data-ttu-id="8a23e-173">Aktualizace hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="8a23e-173">Update hello web app</span></span>

<span data-ttu-id="8a23e-174">Změnit hello ukázka JSP kód tooa jiná zpráva.</span><span class="sxs-lookup"><span data-stu-id="8a23e-174">Change hello sample JSP code tooa different message.</span></span>

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

<span data-ttu-id="8a23e-175">Uložte změny hello.</span><span class="sxs-lookup"><span data-stu-id="8a23e-175">Save hello changes.</span></span>

<span data-ttu-id="8a23e-176">V prohlížeči projektu klikněte pravým tlačítkem na projekt hello a pak vyberte **Azure** > **publikovat jako webové aplikace Azure**.</span><span class="sxs-lookup"><span data-stu-id="8a23e-176">In Project Explorer, right-click hello project, and then select **Azure** > **Publish as Azure Web App**.</span></span>

<span data-ttu-id="8a23e-177">Hello **nasadit webovou aplikaci** se zobrazí dialogové okno a ukazuje hello služby app service, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="8a23e-177">hello **Deploy Web App** dialog box appears and shows hello app service that you previously created.</span></span> 

> [!NOTE]
> <span data-ttu-id="8a23e-178">Vyberte **nasazení tooroot** pokaždé, když publikujete.</span><span class="sxs-lookup"><span data-stu-id="8a23e-178">Select **Deploy tooroot** each time you publish.</span></span>
>

<span data-ttu-id="8a23e-179">Vyberte hello webovou aplikaci a vyberte **nasadit**, který publikuje hello změny.</span><span class="sxs-lookup"><span data-stu-id="8a23e-179">Select hello web app and select **Deploy**, which publishes hello changes.</span></span>

<span data-ttu-id="8a23e-180">Když hello **publikování** odkaz se zobrazí, vyberte ho toobrowse toohello webové aplikace a podívejte se změny hello.</span><span class="sxs-lookup"><span data-stu-id="8a23e-180">When hello **Publishing** link appears, select it toobrowse toohello web app and see hello changes.</span></span>

## <a name="manage-hello-web-app"></a><span data-ttu-id="8a23e-181">Správa webové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="8a23e-181">Manage hello web app</span></span>

<span data-ttu-id="8a23e-182">Přejděte toohello <a href="https://portal.azure.com" target="_blank">portál Azure</a> toosee hello webovou aplikaci, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="8a23e-182">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toosee hello web app that you created.</span></span>

<span data-ttu-id="8a23e-183">Hello levé nabídce vyberte **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="8a23e-183">From hello left menu, select **Resource Groups**.</span></span>

![Skupiny tooresource portálu](media/app-service-web-get-started-java/rg.png)

<span data-ttu-id="8a23e-185">Vyberte skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="8a23e-185">Select hello resource group.</span></span> <span data-ttu-id="8a23e-186">Hello stránka zobrazuje hello prostředky, které jste vytvořili v tento rychlý start.</span><span class="sxs-lookup"><span data-stu-id="8a23e-186">hello page shows hello resources that you created in this quickstart.</span></span>

![Skupina prostředků myResourceGroup](media/app-service-web-get-started-java/rg2.png)

<span data-ttu-id="8a23e-188">Vyberte hello webové aplikace (**webapp 170602193915** v hello předcházející bitové kopie).</span><span class="sxs-lookup"><span data-stu-id="8a23e-188">Select hello web app (**webapp-170602193915** in hello preceding image).</span></span>

<span data-ttu-id="8a23e-189">Hello **přehled** se zobrazí stránka.</span><span class="sxs-lookup"><span data-stu-id="8a23e-189">hello **Overview** page appears.</span></span> <span data-ttu-id="8a23e-190">Tato stránka poskytuje zobrazení jak je to aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="8a23e-190">This page gives you a view of how hello app is doing.</span></span> <span data-ttu-id="8a23e-191">Tady můžete provádět základní úkoly správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="8a23e-191">Here, you can  perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="8a23e-192">Hello karty na levé straně stránky hello hello zobrazit hello různé konfigurace, které lze otevřít.</span><span class="sxs-lookup"><span data-stu-id="8a23e-192">hello tabs on hello left side of hello page show hello different configurations that you can open.</span></span> 

![Stránka služby App Service na webu Azure Portal](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a><span data-ttu-id="8a23e-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a23e-194">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8a23e-195">Mapování vlastní domény</span><span class="sxs-lookup"><span data-stu-id="8a23e-195">Map custom domain</span></span>](app-service-web-tutorial-custom-domain.md)
