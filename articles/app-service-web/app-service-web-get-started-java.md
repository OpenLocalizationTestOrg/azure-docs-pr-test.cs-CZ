---
title: "Vytvoření první webové Java aplikace ve službě Azure"
description: "Nasazením základní Java aplikace se naučíte, jak spouštět webové aplikace ve službě App Service."
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
ms.openlocfilehash: b91b9bde5eb8ea0d7e2196056b635fe54095e748
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-your-first-java-web-app-in-azure"></a><span data-ttu-id="ab269-103">Vytvoření první webové Java aplikace ve službě Azure</span><span class="sxs-lookup"><span data-stu-id="ab269-103">Create your first Java web app in Azure</span></span>

<span data-ttu-id="ab269-104">Funkce [Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) služby [Azure App Service](../app-service/app-service-value-prop-what-is.md) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="ab269-104">The [Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) feature of [Azure App Service](../app-service/app-service-value-prop-what-is.md) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="ab269-105">Tento kurz Rychlý start ukazuje, jak nasadit webovou Java aplikaci do služby App Service pomocí [integrovaného vývojového prostředí Eclipse pro vývojáře na platformě Java EE](http://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="ab269-105">This quickstart shows how to deploy a Java web app to App Service by using the [Eclipse IDE for Java EE Developers](http://www.eclipse.org/).</span></span>

![Hello Azure!](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="prerequisites"></a><span data-ttu-id="ab269-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ab269-108">Prerequisites</span></span>

<span data-ttu-id="ab269-109">K provedení kroků v tomto kurzu Rychlý start je potřeba nainstalovat:</span><span class="sxs-lookup"><span data-stu-id="ab269-109">To complete this quickstart, install:</span></span>

* <span data-ttu-id="ab269-110">Bezplatné [integrované vývojové prostředí Eclipse pro vývojáře na platformě Java EE](http://www.eclipse.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="ab269-110">The free [Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/).</span></span> <span data-ttu-id="ab269-111">Tento kurz Rychlý start používá Eclipse Neon.</span><span class="sxs-lookup"><span data-stu-id="ab269-111">This quickstart uses Eclipse Neon.</span></span>
* <span data-ttu-id="ab269-112">[Sadu nástrojů Azure pro Eclipse](/azure/azure-toolkit-for-eclipse-installation).</span><span class="sxs-lookup"><span data-stu-id="ab269-112">The [Azure Toolkit for Eclipse](/azure/azure-toolkit-for-eclipse-installation).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-dynamic-web-project-in-eclipse"></a><span data-ttu-id="ab269-113">Vytvoření dynamického webového projektu v Eclipse</span><span class="sxs-lookup"><span data-stu-id="ab269-113">Create a dynamic web project in Eclipse</span></span>

<span data-ttu-id="ab269-114">V prostředí Eclipse vyberte **File** (Soubor) > **New** (Nový) > **Dynamic Web Project** (Dynamický webový projekt).</span><span class="sxs-lookup"><span data-stu-id="ab269-114">In Eclipse, select **File** > **New** > **Dynamic Web Project**.</span></span>

<span data-ttu-id="ab269-115">V dialogovém okně **New Dynamic Web Project** (Nový dynamický webový projekt) pojmenujte projekt **MyFirstJavaOnAzureWebApp** a vyberte **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="ab269-115">In the **New Dynamic Web Project** dialog box, name the project **MyFirstJavaOnAzureWebApp**, and select **Finish**.</span></span>
   
![Dialogové okno New Dynamic Web Project (Nový dynamický webový projekt)](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### <a name="add-a-jsp-page"></a><span data-ttu-id="ab269-117">Přidání stránky JSP</span><span class="sxs-lookup"><span data-stu-id="ab269-117">Add a JSP page</span></span>

<span data-ttu-id="ab269-118">Pokud se nezobrazí Project Explorer (Prohlížeč projektu), obnovte zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ab269-118">If Project Explorer is not displayed, restore it.</span></span>

![Pracovní prostor Java EE pro Eclipse](./media/app-service-web-get-started-java/pe.png)

<span data-ttu-id="ab269-120">V prohlížeči Project Explorer rozbalte projekt **MyFirstJavaOnAzureWebApp**.</span><span class="sxs-lookup"><span data-stu-id="ab269-120">In Project Explorer, expand the **MyFirstJavaOnAzureWebApp** project.</span></span>
<span data-ttu-id="ab269-121">Klikněte pravým tlačítkem na **WebContent** (Webový obsah) a potom vyberte **New** (Nový) > **JSP File** (Soubor JSP).</span><span class="sxs-lookup"><span data-stu-id="ab269-121">Right-click **WebContent**, and then select **New** > **JSP File**.</span></span>

![Nabídka pro nový soubor JSP v prohlížeči Project Explorer](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

<span data-ttu-id="ab269-123">V dialogovém okně **New JSP File** (Nový soubor JSP):</span><span class="sxs-lookup"><span data-stu-id="ab269-123">In the **New JSP File** dialog box:</span></span>

* <span data-ttu-id="ab269-124">Soubor pojmenujte **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="ab269-124">Name the file **index.jsp**.</span></span>
* <span data-ttu-id="ab269-125">Vyberte **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="ab269-125">Select **Finish**.</span></span>

  ![Dialogové okno New JSP File (Nový soubor JSP)](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

<span data-ttu-id="ab269-127">V souboru index.jsp nahraďte element `<body></body>` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ab269-127">In the index.jsp file, replace the `<body></body>` element with the following markup:</span></span>

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

<span data-ttu-id="ab269-128">Uložte změny.</span><span class="sxs-lookup"><span data-stu-id="ab269-128">Save the changes.</span></span>

## <a name="publish-the-web-app-to-azure"></a><span data-ttu-id="ab269-129">Publikování webové aplikace do služby Azure</span><span class="sxs-lookup"><span data-stu-id="ab269-129">Publish the web app to Azure</span></span>

<span data-ttu-id="ab269-130">V prohlížeči Project Explorer klikněte pravým tlačítkem na projekt a potom vyberte **Azure** > **Publish as Azure Web App** (Publikovat jako webovou aplikaci Azure).</span><span class="sxs-lookup"><span data-stu-id="ab269-130">In Project Explorer, right-click the project, and then select **Azure** > **Publish as Azure Web App**.</span></span>

![Místní nabídka Publish as Azure Web App (Publikovat jako webovou aplikaci Azure)](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

<span data-ttu-id="ab269-132">V dialogovém okně **Azure Sign In** (Přihlášení k Azure) ponechejte možnost **Interactive** (Interaktivní) a pak vyberte **Sign in** (Přihlásit).</span><span class="sxs-lookup"><span data-stu-id="ab269-132">In the **Azure Sign In** dialog box, keep the **Interactive** option, and then select **Sign in**.</span></span>

<span data-ttu-id="ab269-133">Postupujte podle pokynů k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ab269-133">Follow the sign-in instructions.</span></span>

### <a name="deploy-web-app-dialog-box"></a><span data-ttu-id="ab269-134">Dialogové okno Deploy Web App (Nasazení webové aplikace)</span><span class="sxs-lookup"><span data-stu-id="ab269-134">Deploy Web App dialog box</span></span>

<span data-ttu-id="ab269-135">Jakmile budete přihlášeni ke svému účtu Azure, zobrazí se dialogové okno **Deploy Web App** (Nasazení webové aplikace).</span><span class="sxs-lookup"><span data-stu-id="ab269-135">After you have signed in to your Azure account, the **Deploy Web App** dialog box appears.</span></span>

<span data-ttu-id="ab269-136">Vyberte **Create** (Vytvořit).</span><span class="sxs-lookup"><span data-stu-id="ab269-136">Select **Create**.</span></span>

![Dialogové okno Deploy Web App (Nasazení webové aplikace)](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### <a name="create-app-service-dialog-box"></a><span data-ttu-id="ab269-138">Dialogové okno Create App Service (Vytvoření služby App Service)</span><span class="sxs-lookup"><span data-stu-id="ab269-138">Create App Service dialog box</span></span>

<span data-ttu-id="ab269-139">Dialogové okno **Create App Service** (Vytvoření služby App Service) se zobrazí s výchozími hodnotami.</span><span class="sxs-lookup"><span data-stu-id="ab269-139">The **Create App Service** dialog box appears with default values.</span></span> <span data-ttu-id="ab269-140">Číslo **170602185241** zobrazené na následujícím obrázku se bude ve vašem dialogovém okně lišit.</span><span class="sxs-lookup"><span data-stu-id="ab269-140">The number **170602185241** shown in the following image is different in your dialog box.</span></span>

![Dialogové okno Create App Service (Vytvoření služby App Service)](./media/app-service-web-get-started-java/cas1.png)

<span data-ttu-id="ab269-142">V dialogovém okně **Create App Service** (Vytvoření služby App Service):</span><span class="sxs-lookup"><span data-stu-id="ab269-142">In the **Create App Service** dialog box:</span></span>

* <span data-ttu-id="ab269-143">Ponechejte vygenerovaný název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ab269-143">Keep the generated name for the web app.</span></span> <span data-ttu-id="ab269-144">Tento název musí být v rámci služby Azure jedinečný.</span><span class="sxs-lookup"><span data-stu-id="ab269-144">This name must be unique across Azure.</span></span> <span data-ttu-id="ab269-145">Název je součástí adresy URL webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ab269-145">The name is part of the URL address for the web app.</span></span> <span data-ttu-id="ab269-146">Příklad: Pokud je název webové aplikace **MyJavaWebApp**, bude adresa URL *myjavawebapp.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="ab269-146">For example: if the web app name is **MyJavaWebApp**, the URL is *myjavawebapp.azurewebsites.net*.</span></span>
* <span data-ttu-id="ab269-147">Ponechejte výchozí webový kontejner.</span><span class="sxs-lookup"><span data-stu-id="ab269-147">Keep the default web container.</span></span>
* <span data-ttu-id="ab269-148">Vyberte předplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="ab269-148">Select an Azure subscription.</span></span>
* <span data-ttu-id="ab269-149">Na kartě **App service plan** (Plán služby App Service):</span><span class="sxs-lookup"><span data-stu-id="ab269-149">On the **App service plan** tab:</span></span>

  * <span data-ttu-id="ab269-150">**Create new** (Vytvořit nový): Ponechejte výchozí hodnotu, kterou je název plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="ab269-150">**Create new**: Keep the default, which is the name of the App Service plan.</span></span>
  * <span data-ttu-id="ab269-151">**Location** (Umístění): Vyberte **West Europe** (Západní Evropa) nebo umístění ve vaší blízkosti.</span><span class="sxs-lookup"><span data-stu-id="ab269-151">**Location**: Select **West Europe** or a location near you.</span></span>
  * <span data-ttu-id="ab269-152">**Pricing tier** (Cenová úroveň): Vyberte možnost Free.</span><span class="sxs-lookup"><span data-stu-id="ab269-152">**Pricing tier**: Select the free option.</span></span> <span data-ttu-id="ab269-153">Informace o funkcích najdete na stránce [App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="ab269-153">For features, see [App Service pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>

   ![Dialogové okno Create App Service (Vytvoření služby App Service)](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### <a name="resource-group-tab"></a><span data-ttu-id="ab269-155">Karta Resource group (Skupina prostředků)</span><span class="sxs-lookup"><span data-stu-id="ab269-155">Resource group tab</span></span>

<span data-ttu-id="ab269-156">Vyberte kartu **Resource group** (Skupina prostředků). Ponechte výchozí vygenerovanou hodnotu pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="ab269-156">Select the **Resource group** tab. Keep the default generated value for the resource group.</span></span>

![Karta Resource group (Skupina prostředků)](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

<span data-ttu-id="ab269-158">Vyberte **Create** (Vytvořit).</span><span class="sxs-lookup"><span data-stu-id="ab269-158">Select **Create**.</span></span>

<!--
### The JDK tab

Select the **JDK** tab. Keep the default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

<span data-ttu-id="ab269-159">Sada nástrojů Azure vytvoří webovou aplikaci a zobrazí dialogové okno s ukazatelem průběhu.</span><span class="sxs-lookup"><span data-stu-id="ab269-159">The Azure Toolkit creates the web app and displays a progress dialog box.</span></span>

![Dialogové okno Create App Service Progress (Průběh vytváření plánu služby App Service)](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### <a name="deploy-web-app-dialog-box"></a><span data-ttu-id="ab269-161">Dialogové okno Deploy Web App (Nasazení webové aplikace)</span><span class="sxs-lookup"><span data-stu-id="ab269-161">Deploy Web App dialog box</span></span>

<span data-ttu-id="ab269-162">V dialogovém okně **Deploy Web App** (Nasazení webové aplikace) vyberte **Deploy to root** (Nasadit do kořene).</span><span class="sxs-lookup"><span data-stu-id="ab269-162">In the **Deploy Web App** dialog box, select **Deploy to root**.</span></span> <span data-ttu-id="ab269-163">Pokud máte službu App Service na adrese *wingtiptoys.azurewebsites.net* a nezvolíte nasazení do kořene, nasadí se vaše webová aplikace **MyFirstJavaOnAzureWebApp** do *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="ab269-163">If you have an app service at *wingtiptoys.azurewebsites.net* and you do not deploy to the root, the web app named **MyFirstJavaOnAzureWebApp** is deployed to *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.</span></span>

![Dialogové okno Deploy Web App (Nasazení webové aplikace)](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

<span data-ttu-id="ab269-165">Dialogové okno zobrazuje vybrané hodnoty pro Azure, JDK a webový kontejner.</span><span class="sxs-lookup"><span data-stu-id="ab269-165">The dialog box shows the Azure, JDK, and web container selections.</span></span>

<span data-ttu-id="ab269-166">Výběrem možnosti **Deploy** (Nasadit) publikujte webovou aplikaci do služby Azure.</span><span class="sxs-lookup"><span data-stu-id="ab269-166">Select **Deploy** to publish the web app to Azure.</span></span>

<span data-ttu-id="ab269-167">Po dokončení publikování vyberte odkaz **Publikováno** v dialogovém okně **Protokol aktivit v Azure**.</span><span class="sxs-lookup"><span data-stu-id="ab269-167">When the publishing finishes, select the **Published** link in the **Azure Activity Log** dialog box.</span></span>

![Dialogové okno Protokol aktivit v Azure](./media/app-service-web-get-started-java/aal.png)

<span data-ttu-id="ab269-169">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="ab269-169">Congratulations!</span></span> <span data-ttu-id="ab269-170">Úspěšně jste nasadili svou webovou aplikaci do služby Azure.</span><span class="sxs-lookup"><span data-stu-id="ab269-170">You have successfully deployed your web app to Azure.</span></span> 

![Hello Azure!](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="update-the-web-app"></a><span data-ttu-id="ab269-173">Aktualizace webové aplikace</span><span class="sxs-lookup"><span data-stu-id="ab269-173">Update the web app</span></span>

<span data-ttu-id="ab269-174">Změňte zprávu v ukázkovém kódu JSP na jinou.</span><span class="sxs-lookup"><span data-stu-id="ab269-174">Change the sample JSP code to a different message.</span></span>

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

<span data-ttu-id="ab269-175">Uložte změny.</span><span class="sxs-lookup"><span data-stu-id="ab269-175">Save the changes.</span></span>

<span data-ttu-id="ab269-176">V prohlížeči Project Explorer klikněte pravým tlačítkem na projekt a potom vyberte **Azure** > **Publish as Azure Web App** (Publikovat jako webovou aplikaci Azure).</span><span class="sxs-lookup"><span data-stu-id="ab269-176">In Project Explorer, right-click the project, and then select **Azure** > **Publish as Azure Web App**.</span></span>

<span data-ttu-id="ab269-177">Zobrazí se dialogové okno **Deploy Web App** (Nasazení webové aplikace), ve kterém se zobrazí aplikační služba, kterou jste předtím vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ab269-177">The **Deploy Web App** dialog box appears and shows the app service that you previously created.</span></span> 

> [!NOTE]
> <span data-ttu-id="ab269-178">Při každém publikování vyberte **Deploy to root** (Nasadit do kořene).</span><span class="sxs-lookup"><span data-stu-id="ab269-178">Select **Deploy to root** each time you publish.</span></span>
>

<span data-ttu-id="ab269-179">Vyberte webovou aplikaci a pak vyberte **Deploy** (Nasadit). Tím změny publikujete.</span><span class="sxs-lookup"><span data-stu-id="ab269-179">Select the web app and select **Deploy**, which publishes the changes.</span></span>

<span data-ttu-id="ab269-180">Když se zobrazí odkaz **Publikováno**, vyberte ho. Tím přejdete do webové aplikace a budete moct vidět změny.</span><span class="sxs-lookup"><span data-stu-id="ab269-180">When the **Publishing** link appears, select it to browse to the web app and see the changes.</span></span>

## <a name="manage-the-web-app"></a><span data-ttu-id="ab269-181">Správa webové aplikace</span><span class="sxs-lookup"><span data-stu-id="ab269-181">Manage the web app</span></span>

<span data-ttu-id="ab269-182">Pokud chcete zobrazit webovou aplikaci, kterou jste vytvořili, přejděte na web <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.</span><span class="sxs-lookup"><span data-stu-id="ab269-182">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to see the web app that you created.</span></span>

<span data-ttu-id="ab269-183">V levé nabídce vyberte **Skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="ab269-183">From the left menu, select **Resource Groups**.</span></span>

![Navigace na skupiny prostředků na portálu](media/app-service-web-get-started-java/rg.png)

<span data-ttu-id="ab269-185">Vyberte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="ab269-185">Select the resource group.</span></span> <span data-ttu-id="ab269-186">Stránka zobrazuje prostředky, které jste vytvořili v tomto kurzu Rychlý start.</span><span class="sxs-lookup"><span data-stu-id="ab269-186">The page shows the resources that you created in this quickstart.</span></span>

![Skupina prostředků myResourceGroup](media/app-service-web-get-started-java/rg2.png)

<span data-ttu-id="ab269-188">Vyberte webovou aplikaci (**webapp-170602193915** na předchozím obrázku).</span><span class="sxs-lookup"><span data-stu-id="ab269-188">Select the web app (**webapp-170602193915** in the preceding image).</span></span>

<span data-ttu-id="ab269-189">Zobrazí se stránka **Přehled**.</span><span class="sxs-lookup"><span data-stu-id="ab269-189">The **Overview** page appears.</span></span> <span data-ttu-id="ab269-190">Tato stránka poskytuje přehled toho, jak si aplikace vede.</span><span class="sxs-lookup"><span data-stu-id="ab269-190">This page gives you a view of how the app is doing.</span></span> <span data-ttu-id="ab269-191">Tady můžete provádět základní úkoly správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="ab269-191">Here, you can  perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="ab269-192">Záložky na levé straně okna obsahují další možnosti konfigurace, které můžete otevřít.</span><span class="sxs-lookup"><span data-stu-id="ab269-192">The tabs on the left side of the page show the different configurations that you can open.</span></span> 

![Stránka služby App Service na webu Azure Portal](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a><span data-ttu-id="ab269-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ab269-194">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ab269-195">Mapování vlastní domény</span><span class="sxs-lookup"><span data-stu-id="ab269-195">Map custom domain</span></span>](app-service-web-tutorial-custom-domain.md)
