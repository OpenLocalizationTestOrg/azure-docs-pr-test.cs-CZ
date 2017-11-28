---
title: "Vytvoření webové aplikace ASP.NET ve službě Azure | Dokumentace Microsoftu"
description: "Nasazením ukázkové webové aplikace ASP.NET se naučíte, jak spouštět webové aplikace ve službě Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/14/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 0f0035f6fef03ddcbb500b78f3445ced5b749808
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-aspnet-web-app-in-azure"></a><span data-ttu-id="c806c-103">Vytvoření webové aplikace ASP.NET v Azure</span><span class="sxs-lookup"><span data-stu-id="c806c-103">Create an ASP.NET web app in Azure</span></span>

<span data-ttu-id="c806c-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="c806c-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="c806c-105">V tomto kurzu Rychlý start se dozvíte, jak nasadit svoji první webovou aplikaci ASP.NET pomocí služby Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="c806c-105">This quickstart shows how to deploy your first ASP.NET web app to Azure Web Apps.</span></span> <span data-ttu-id="c806c-106">Po dokončení kurzu budete mít skupinu prostředků, která se bude skládat z plánu služby App Service a webové aplikace Azure s nasazenou webovou aplikací.</span><span class="sxs-lookup"><span data-stu-id="c806c-106">When you're finished, you'll have a resource group that consists of an App Service plan and an Azure web app with a deployed web application.</span></span>

<span data-ttu-id="c806c-107">Podívejte se na video s tímto rychlým startem v akci a potom sami proveďte příslušné kroky a publikujte svou první aplikaci .NET v Azure.</span><span class="sxs-lookup"><span data-stu-id="c806c-107">Watch the video to see this quickstart in action and then follow the steps yourself to publish your first .NET app on Azure.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-NET-Developers/Create-a-NET-app-in-Azure-Quickstart/player]

## <a name="prerequisites"></a><span data-ttu-id="c806c-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c806c-108">Prerequisites</span></span>

<span data-ttu-id="c806c-109">K provedení kroků v tomto kurzu je potřeba:</span><span class="sxs-lookup"><span data-stu-id="c806c-109">To complete this tutorial:</span></span>

* <span data-ttu-id="c806c-110">Nainstalovat [Visual Studio 2017](https://www.visualstudio.com/downloads/) s následujícími sadami funkcí:</span><span class="sxs-lookup"><span data-stu-id="c806c-110">Install [Visual Studio 2017](https://www.visualstudio.com/downloads/) with the following workloads:</span></span>
    - <span data-ttu-id="c806c-111">**Vývoj pro ASP.NET a web**</span><span class="sxs-lookup"><span data-stu-id="c806c-111">**ASP.NET and web development**</span></span>
    - <span data-ttu-id="c806c-112">**Azure – vývoj**</span><span class="sxs-lookup"><span data-stu-id="c806c-112">**Azure development**</span></span>

    ![Vývoj pro ASP.NET a Azure – vývoj (v části Web a cloud)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="c806c-114">Vytvoření webové aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c806c-114">Create an ASP.NET web app</span></span>

<span data-ttu-id="c806c-115">Ve Visual Studiu vytvořte projekt tak, že vyberete **Soubor > Nový > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="c806c-115">In Visual Studio, create a project by selecting **File > New > Project**.</span></span> 

<span data-ttu-id="c806c-116">V dialogovém okně **Nový projekt** vyberte **Visual C# > Web > Webová aplikace ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="c806c-116">In the **New Project** dialog, select **Visual C# > Web > ASP.NET Web Application (.NET Framework)**.</span></span>

<span data-ttu-id="c806c-117">Aplikaci pojmenujte _myFirstAzureWebApp_ a pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="c806c-117">Name the application _myFirstAzureWebApp_, and then select **OK**.</span></span>
   
![Dialogové okno Nový projekt](./media/app-service-web-get-started-dotnet/new-project.png)

<span data-ttu-id="c806c-119">Do Azure můžete nasadit jakýkoli typ webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c806c-119">You can deploy any type of ASP.NET web app to Azure.</span></span> <span data-ttu-id="c806c-120">V tomto kurzu Rychlý start vyberte šablonu **MVC** a ujistěte se, že u ověřování je nastavena možnost **Bez ověření**.</span><span class="sxs-lookup"><span data-stu-id="c806c-120">For this quickstart, select the **MVC** template, and make sure authentication is set to **No Authentication**.</span></span>
      
<span data-ttu-id="c806c-121">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="c806c-121">Select **OK**.</span></span>

![Dialogové okno Nový projekt ASP.NET](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

<span data-ttu-id="c806c-123">V nabídce vyberte **Ladit > Spustit bez ladění** a spusťte tak webovou aplikaci místně.</span><span class="sxs-lookup"><span data-stu-id="c806c-123">From the menu, select **Debug > Start without Debugging** to run the web app locally.</span></span>

![Místní spuštění aplikace](./media/app-service-web-get-started-dotnet/local-web-app.png)

## <a name="publish-to-azure"></a><span data-ttu-id="c806c-125">Publikování aplikací do Azure</span><span class="sxs-lookup"><span data-stu-id="c806c-125">Publish to Azure</span></span>

<span data-ttu-id="c806c-126">V **Průzkumníku řešení** klikněte pravým tlačítkem na projekt **myFirstAzureWebApp** a vyberte možnost **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="c806c-126">In the **Solution Explorer**, right-click the **myFirstAzureWebApp** project and select **Publish**.</span></span>

![Publikování z Průzkumníka řešení](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

<span data-ttu-id="c806c-128">Zkontrolujte, že je vybrána možnost **Microsoft Azure App Service** a vyberte **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="c806c-128">Make sure that **Microsoft Azure App Service** is selected and select **Publish**.</span></span>

![Publikování ze stránky přehledu projektu](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

<span data-ttu-id="c806c-130">Tím se otevře dialogové okno **Vytvořit plán Aplikační služby**, které vám pomůže vytvořit všechny prostředky služby Azure potřebné ke spuštění webové aplikace ASP.NET ve službě Azure.</span><span class="sxs-lookup"><span data-stu-id="c806c-130">This opens the **Create App Service** dialog, which helps you create all the necessary Azure resources to run the ASP.NET web app in Azure.</span></span>

## <a name="sign-in-to-azure"></a><span data-ttu-id="c806c-131">Přihlášení k Azure</span><span class="sxs-lookup"><span data-stu-id="c806c-131">Sign in to Azure</span></span>

<span data-ttu-id="c806c-132">V dialogovém okně **Vytvořit plán Aplikační služby** vyberte **Přidat účet** a přihlaste se ke svému předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="c806c-132">In the **Create App Service** dialog, select **Add an account**, and sign in to your Azure subscription.</span></span> <span data-ttu-id="c806c-133">Pokud už jste přihlášeni, vyberte z rozevíracího seznamu účet, který obsahuje požadované předplatné.</span><span class="sxs-lookup"><span data-stu-id="c806c-133">If you're already signed in, select the account containing the desired subscription from the dropdown.</span></span>

> [!NOTE]
> <span data-ttu-id="c806c-134">Pokud už jste přihlášení, nevybírejte zatím možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c806c-134">If you're already signed in, don't select **Create** yet.</span></span>
>
>
   
![Přihlášení k Azure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

## <a name="create-a-resource-group"></a><span data-ttu-id="c806c-136">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="c806c-136">Create a resource group</span></span>

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

<span data-ttu-id="c806c-137">Vedle pole **Skupina prostředků** vyberte **Nová**.</span><span class="sxs-lookup"><span data-stu-id="c806c-137">Next to **Resource Group**, select **New**.</span></span>

<span data-ttu-id="c806c-138">Skupinu prostředků pojmenujte **myResourceGroup** a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="c806c-138">Name the resource group **myResourceGroup** and select **OK**.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="c806c-139">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="c806c-139">Create an App Service plan</span></span>

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="c806c-140">Vedle pole **Plán služby App Service** vyberte **Nový**.</span><span class="sxs-lookup"><span data-stu-id="c806c-140">Next to **App Service Plan**, select **New**.</span></span> 

<span data-ttu-id="c806c-141">V dialogovém okně **Konfigurovat plán Aplikační služby** použijte nastavení podle tabulky následující po snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="c806c-141">In the **Configure App Service Plan** dialog, use the settings in the table following the screenshot.</span></span>

![Vytvoření plánu služby App Service](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

| <span data-ttu-id="c806c-143">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c806c-143">Setting</span></span> | <span data-ttu-id="c806c-144">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="c806c-144">Suggested Value</span></span> | <span data-ttu-id="c806c-145">Popis</span><span class="sxs-lookup"><span data-stu-id="c806c-145">Description</span></span> |
|-|-|-|
|<span data-ttu-id="c806c-146">Plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="c806c-146">App Service Plan</span></span>| <span data-ttu-id="c806c-147">myAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="c806c-147">myAppServicePlan</span></span> | <span data-ttu-id="c806c-148">Název plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="c806c-148">Name of the App Service plan.</span></span> |
| <span data-ttu-id="c806c-149">Umístění</span><span class="sxs-lookup"><span data-stu-id="c806c-149">Location</span></span> | <span data-ttu-id="c806c-150">Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="c806c-150">West Europe</span></span> | <span data-ttu-id="c806c-151">Datacentrum, které je hostitelem webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c806c-151">The datacenter where the web app is hosted.</span></span> |
| <span data-ttu-id="c806c-152">Velikost</span><span class="sxs-lookup"><span data-stu-id="c806c-152">Size</span></span> | <span data-ttu-id="c806c-153">Free</span><span class="sxs-lookup"><span data-stu-id="c806c-153">Free</span></span> | <span data-ttu-id="c806c-154">[Cenová úroveň](https://azure.microsoft.com/pricing/details/app-service/) určuje funkce hostování.</span><span class="sxs-lookup"><span data-stu-id="c806c-154">[Pricing tier](https://azure.microsoft.com/pricing/details/app-service/) determines hosting features.</span></span> |

<span data-ttu-id="c806c-155">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="c806c-155">Select **OK**.</span></span>

## <a name="create-and-publish-the-web-app"></a><span data-ttu-id="c806c-156">Vytvoření a publikování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c806c-156">Create and publish the web app</span></span>

<span data-ttu-id="c806c-157">V části **Název webové aplikace** zadejte jedinečný název aplikace (platné znaky jsou `a-z`, `0-9` a `-`) nebo přijměte automaticky vygenerovaný jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="c806c-157">In **Web App Name**, type a unique app name (valid characters are `a-z`, `0-9`, and `-`), or accept the automatically generated unique name.</span></span> <span data-ttu-id="c806c-158">Adresa URL webové aplikace je `http://<app_name>.azurewebsites.net`, kde `<app_name>` je název vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c806c-158">The URL of the web app is `http://<app_name>.azurewebsites.net`, where `<app_name>` is your web app name.</span></span>

<span data-ttu-id="c806c-159">Výběrem možnosti **Vytvořit** spustíte vytváření prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="c806c-159">Select **Create** to start creating the Azure resources.</span></span>

![Nastavení názvu webové aplikace](./media/app-service-web-get-started-dotnet/web-app-name.png)

<span data-ttu-id="c806c-161">Průvodce po dokončení publikuje webovou aplikaci ASP.NET do služby Azure a pak aplikaci spustí ve výchozím prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c806c-161">Once the wizard completes, it publishes the ASP.NET web app to Azure, and then launches the app in the default browser.</span></span>

![Publikovaná webová aplikace ASP.NET v Azure](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

<span data-ttu-id="c806c-163">Název webové aplikace zadaný v [kroku vytvoření a publikování](#create-and-publish-the-web-app) se použije jako předpona adresy URL ve formátu `http://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="c806c-163">The web app name specified in the [create and publish step](#create-and-publish-the-web-app) is used as the URL prefix in the format `http://<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="c806c-164">Blahopřejeme, vaše webová aplikace ASP.NET je veřejně spuštěná ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c806c-164">Congratulations, your ASP.NET web app is running live in Azure App Service.</span></span>

## <a name="update-the-app-and-redeploy"></a><span data-ttu-id="c806c-165">Aktualizace a opětovné nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="c806c-165">Update the app and redeploy</span></span>

<span data-ttu-id="c806c-166">Z **Průzkumníku řešení** otevřete _Views\Home\Index.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="c806c-166">From the **Solution Explorer**, open _Views\Home\Index.cshtml_.</span></span>

<span data-ttu-id="c806c-167">Najděte HTML značku `<div class="jumbotron">` poblíž začátku a nahraďte celý element následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="c806c-167">Find the `<div class="jumbotron">` HTML tag near the top, and replace the entire element with the following code:</span></span>

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

<span data-ttu-id="c806c-168">Opětovné nasazení do služby Azure provedete tak, že v **Průzkumníku řešení** kliknete pravým tlačítkem na projekt **myFirstAzureWebApp** a vyberete **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="c806c-168">To redeploy to Azure, right-click the **myFirstAzureWebApp** project in **Solution Explorer** and select **Publish**.</span></span>

<span data-ttu-id="c806c-169">Na stránce publikování vyberte **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="c806c-169">In the publish page, select **Publish**.</span></span>

<span data-ttu-id="c806c-170">Po dokončení publikování spustí Visual Studio prohlížeč na adrese URL webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c806c-170">When publishing completes, Visual Studio launches a browser to the URL of the web app.</span></span>

![Aktualizovaná webová aplikace ASP.NET v Azure](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="manage-the-azure-web-app"></a><span data-ttu-id="c806c-172">Správa webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="c806c-172">Manage the Azure web app</span></span>

<span data-ttu-id="c806c-173">Pokud chcete webovou aplikaci spravovat, přejděte na web <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.</span><span class="sxs-lookup"><span data-stu-id="c806c-173">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to manage the web app.</span></span>

<span data-ttu-id="c806c-174">V levé nabídce klikněte na **App Services** a potom vyberte název své webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="c806c-174">From the left menu, select **App Services**, and then select the name of your Azure web app.</span></span>

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-get-started-dotnet/access-portal.png)

<span data-ttu-id="c806c-176">Zobrazí se stránka s přehledem vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c806c-176">You see your web app's Overview page.</span></span> <span data-ttu-id="c806c-177">Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="c806c-177">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Okno App Service na webu Azure Portal](./media/app-service-web-get-started-dotnet/web-app-blade.png)

<span data-ttu-id="c806c-179">Levá nabídka obsahuje odkazy na různé stránky pro konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c806c-179">The left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="c806c-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c806c-180">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c806c-181">ASP.NET s databází SQL Database</span><span class="sxs-lookup"><span data-stu-id="c806c-181">ASP.NET with SQL Database</span></span>](app-service-web-tutorial-dotnet-sqldatabase.md)
