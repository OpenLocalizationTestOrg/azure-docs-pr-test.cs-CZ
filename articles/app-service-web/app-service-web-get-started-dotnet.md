---
title: "aaaCreate technologie ASP.NET webové aplikace v Azure | Microsoft Docs"
description: "Zjistěte, jak toorun webové aplikace v Azure App Service nasazením hello výchozí ASP.NET webové aplikace."
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
ms.openlocfilehash: eec916b3c32b6c8b68083177938c5c822a9782b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-web-app-in-azure"></a><span data-ttu-id="c1f82-103">Vytvoření webové aplikace ASP.NET v Azure</span><span class="sxs-lookup"><span data-stu-id="c1f82-103">Create an ASP.NET web app in Azure</span></span>

<span data-ttu-id="c1f82-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="c1f82-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="c1f82-105">Tento rychlý start ukazuje, jak toodeploy vaše první ASP.NET webové aplikace tooAzure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1f82-105">This quickstart shows how toodeploy your first ASP.NET web app tooAzure Web Apps.</span></span> <span data-ttu-id="c1f82-106">Po dokončení kurzu budete mít skupinu prostředků, která se bude skládat z plánu služby App Service a webové aplikace Azure s nasazenou webovou aplikací.</span><span class="sxs-lookup"><span data-stu-id="c1f82-106">When you're finished, you'll have a resource group that consists of an App Service plan and an Azure web app with a deployed web application.</span></span>

<span data-ttu-id="c1f82-107">Podívejte se na hello video toosee tento rychlý start v akci a potom postupujte podle hello kroky sami toopublish vaší první aplikace .NET v Azure.</span><span class="sxs-lookup"><span data-stu-id="c1f82-107">Watch hello video toosee this quickstart in action and then follow hello steps yourself toopublish your first .NET app on Azure.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-NET-Developers/Create-a-NET-app-in-Azure-Quickstart/player]

## <a name="prerequisites"></a><span data-ttu-id="c1f82-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c1f82-108">Prerequisites</span></span>

<span data-ttu-id="c1f82-109">toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="c1f82-109">toocomplete this tutorial:</span></span>

* <span data-ttu-id="c1f82-110">Nainstalujte [Visual Studio 2017](https://www.visualstudio.com/downloads/) s hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="c1f82-110">Install [Visual Studio 2017](https://www.visualstudio.com/downloads/) with hello following workloads:</span></span>
    - <span data-ttu-id="c1f82-111">**Vývoj pro ASP.NET a web**</span><span class="sxs-lookup"><span data-stu-id="c1f82-111">**ASP.NET and web development**</span></span>
    - <span data-ttu-id="c1f82-112">**Azure – vývoj**</span><span class="sxs-lookup"><span data-stu-id="c1f82-112">**Azure development**</span></span>

    ![Vývoj pro ASP.NET a Azure – vývoj (v části Web a cloud)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="c1f82-114">Vytvoření webové aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c1f82-114">Create an ASP.NET web app</span></span>

<span data-ttu-id="c1f82-115">Ve Visual Studiu vytvořte projekt tak, že vyberete **Soubor > Nový > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="c1f82-115">In Visual Studio, create a project by selecting **File > New > Project**.</span></span> 

<span data-ttu-id="c1f82-116">V hello **nový projekt** dialogovém okně, vyberte **Visual C# > Web > webové aplikace ASP.NET (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="c1f82-116">In hello **New Project** dialog, select **Visual C# > Web > ASP.NET Web Application (.NET Framework)**.</span></span>

<span data-ttu-id="c1f82-117">Název aplikace hello _myFirstAzureWebApp_a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1f82-117">Name hello application _myFirstAzureWebApp_, and then select **OK**.</span></span>
   
![Dialogové okno Nový projekt](./media/app-service-web-get-started-dotnet/new-project.png)

<span data-ttu-id="c1f82-119">Můžete nasadit libovolného typu tooAzure aplikace ASP.NET web.</span><span class="sxs-lookup"><span data-stu-id="c1f82-119">You can deploy any type of ASP.NET web app tooAzure.</span></span> <span data-ttu-id="c1f82-120">Pro tento rychlý start, vyberte hello **MVC** šablony a zajistěte, aby ověřování je nastaven příliš**bez ověřování**.</span><span class="sxs-lookup"><span data-stu-id="c1f82-120">For this quickstart, select hello **MVC** template, and make sure authentication is set too**No Authentication**.</span></span>
      
<span data-ttu-id="c1f82-121">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1f82-121">Select **OK**.</span></span>

![Dialogové okno Nový projekt ASP.NET](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

<span data-ttu-id="c1f82-123">Hello nabídce vyberte **ladění > Spustit bez ladění** toorun hello webovou aplikaci místně.</span><span class="sxs-lookup"><span data-stu-id="c1f82-123">From hello menu, select **Debug > Start without Debugging** toorun hello web app locally.</span></span>

![Místní spuštění aplikace](./media/app-service-web-get-started-dotnet/local-web-app.png)

## <a name="publish-tooazure"></a><span data-ttu-id="c1f82-125">Publikování tooAzure</span><span class="sxs-lookup"><span data-stu-id="c1f82-125">Publish tooAzure</span></span>

<span data-ttu-id="c1f82-126">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **myFirstAzureWebApp** projektu a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="c1f82-126">In hello **Solution Explorer**, right-click hello **myFirstAzureWebApp** project and select **Publish**.</span></span>

![Publikování z Průzkumníka řešení](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

<span data-ttu-id="c1f82-128">Zkontrolujte, že je vybrána možnost **Microsoft Azure App Service** a vyberte **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="c1f82-128">Make sure that **Microsoft Azure App Service** is selected and select **Publish**.</span></span>

![Publikování ze stránky přehledu projektu](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

<span data-ttu-id="c1f82-130">Tím se otevře hello **vytvořit službu App Service** dialog, který vám pomůže vytvořit všechny hello potřebné prostředky Azure toorun hello webové aplikace ASP.NET v Azure.</span><span class="sxs-lookup"><span data-stu-id="c1f82-130">This opens hello **Create App Service** dialog, which helps you create all hello necessary Azure resources toorun hello ASP.NET web app in Azure.</span></span>

## <a name="sign-in-tooazure"></a><span data-ttu-id="c1f82-131">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="c1f82-131">Sign in tooAzure</span></span>

<span data-ttu-id="c1f82-132">V hello **vytvořit službu App Service** dialogovém okně, vyberte **přidat účet**a přihlaste se tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="c1f82-132">In hello **Create App Service** dialog, select **Add an account**, and sign in tooyour Azure subscription.</span></span> <span data-ttu-id="c1f82-133">Pokud už jste přihlášení, vyberte hello účet obsahující hello požadované předplatné z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="c1f82-133">If you're already signed in, select hello account containing hello desired subscription from hello dropdown.</span></span>

> [!NOTE]
> <span data-ttu-id="c1f82-134">Pokud už jste přihlášení, nevybírejte zatím možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c1f82-134">If you're already signed in, don't select **Create** yet.</span></span>
>
>
   
![Přihlaste se tooAzure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

## <a name="create-a-resource-group"></a><span data-ttu-id="c1f82-136">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="c1f82-136">Create a resource group</span></span>

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

<span data-ttu-id="c1f82-137">Další příliš**skupiny prostředků**, vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="c1f82-137">Next too**Resource Group**, select **New**.</span></span>

<span data-ttu-id="c1f82-138">Název skupiny prostředků hello **myResourceGroup** a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1f82-138">Name hello resource group **myResourceGroup** and select **OK**.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="c1f82-139">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="c1f82-139">Create an App Service plan</span></span>

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="c1f82-140">Další příliš**plán služby App Service**, vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="c1f82-140">Next too**App Service Plan**, select **New**.</span></span> 

<span data-ttu-id="c1f82-141">V hello **nakonfigurovat plán služby App Service** dialogové okno, použít nastavení hello v tabulce hello následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="c1f82-141">In hello **Configure App Service Plan** dialog, use hello settings in hello table following hello screenshot.</span></span>

![Vytvoření plánu služby App Service](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

| <span data-ttu-id="c1f82-143">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c1f82-143">Setting</span></span> | <span data-ttu-id="c1f82-144">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="c1f82-144">Suggested Value</span></span> | <span data-ttu-id="c1f82-145">Popis</span><span class="sxs-lookup"><span data-stu-id="c1f82-145">Description</span></span> |
|-|-|-|
|<span data-ttu-id="c1f82-146">Plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="c1f82-146">App Service Plan</span></span>| <span data-ttu-id="c1f82-147">myAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="c1f82-147">myAppServicePlan</span></span> | <span data-ttu-id="c1f82-148">Název hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="c1f82-148">Name of hello App Service plan.</span></span> |
| <span data-ttu-id="c1f82-149">Umístění</span><span class="sxs-lookup"><span data-stu-id="c1f82-149">Location</span></span> | <span data-ttu-id="c1f82-150">Západní Evropa</span><span class="sxs-lookup"><span data-stu-id="c1f82-150">West Europe</span></span> | <span data-ttu-id="c1f82-151">datacentrum Hello je hostitelem hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1f82-151">hello datacenter where hello web app is hosted.</span></span> |
| <span data-ttu-id="c1f82-152">Velikost</span><span class="sxs-lookup"><span data-stu-id="c1f82-152">Size</span></span> | <span data-ttu-id="c1f82-153">Free</span><span class="sxs-lookup"><span data-stu-id="c1f82-153">Free</span></span> | <span data-ttu-id="c1f82-154">[Cenová úroveň](https://azure.microsoft.com/pricing/details/app-service/) určuje funkce hostování.</span><span class="sxs-lookup"><span data-stu-id="c1f82-154">[Pricing tier](https://azure.microsoft.com/pricing/details/app-service/) determines hosting features.</span></span> |

<span data-ttu-id="c1f82-155">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1f82-155">Select **OK**.</span></span>

## <a name="create-and-publish-hello-web-app"></a><span data-ttu-id="c1f82-156">Vytvoření a publikování webové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="c1f82-156">Create and publish hello web app</span></span>

<span data-ttu-id="c1f82-157">V **název webové aplikace**, zadejte název jedinečné aplikace (platnými znaky jsou `a-z`, `0-9`, a `-`), nebo přijměte hello automaticky generuje jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="c1f82-157">In **Web App Name**, type a unique app name (valid characters are `a-z`, `0-9`, and `-`), or accept hello automatically generated unique name.</span></span> <span data-ttu-id="c1f82-158">Adresa URL Hello hello webové aplikace je `http://<app_name>.azurewebsites.net`, kde `<app_name>` je název vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1f82-158">hello URL of hello web app is `http://<app_name>.azurewebsites.net`, where `<app_name>` is your web app name.</span></span>

<span data-ttu-id="c1f82-159">Vyberte **vytvořit** toostart hello vytváření prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="c1f82-159">Select **Create** toostart creating hello Azure resources.</span></span>

![Nastavení názvu webové aplikace](./media/app-service-web-get-started-dotnet/web-app-name.png)

<span data-ttu-id="c1f82-161">Po dokončení Průvodce hello, tato možnost publikuje hello ASP.NET webové aplikace tooAzure a pak spustí hello aplikace v hello výchozí prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="c1f82-161">Once hello wizard completes, it publishes hello ASP.NET web app tooAzure, and then launches hello app in hello default browser.</span></span>

![Publikovaná webová aplikace ASP.NET v Azure](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

<span data-ttu-id="c1f82-163">Název webové aplikace Hello zadaný v hello [vytvoření a publikování krok](#create-and-publish-the-web-app) slouží jako hello předponu adresy URL ve formátu hello `http://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="c1f82-163">hello web app name specified in hello [create and publish step](#create-and-publish-the-web-app) is used as hello URL prefix in hello format `http://<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="c1f82-164">Blahopřejeme, vaše webová aplikace ASP.NET je veřejně spuštěná ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c1f82-164">Congratulations, your ASP.NET web app is running live in Azure App Service.</span></span>

## <a name="update-hello-app-and-redeploy"></a><span data-ttu-id="c1f82-165">Aktualizace aplikace hello a znovu ho zaveďte</span><span class="sxs-lookup"><span data-stu-id="c1f82-165">Update hello app and redeploy</span></span>

<span data-ttu-id="c1f82-166">Z hello **Průzkumníku řešení**, otevřete _Views\Home\Index.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="c1f82-166">From hello **Solution Explorer**, open _Views\Home\Index.cshtml_.</span></span>

<span data-ttu-id="c1f82-167">Najde hello `<div class="jumbotron">` HTML značky v horní hello a nahraďte celý elementu hello hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="c1f82-167">Find hello `<div class="jumbotron">` HTML tag near hello top, and replace hello entire element with hello following code:</span></span>

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how toodeploy a .NET app tooAzure App Service.</p>
</div>
```

<span data-ttu-id="c1f82-168">tooredeploy tooAzure, klikněte pravým tlačítkem na hello **myFirstAzureWebApp** projektu v **Průzkumníku řešení** a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="c1f82-168">tooredeploy tooAzure, right-click hello **myFirstAzureWebApp** project in **Solution Explorer** and select **Publish**.</span></span>

<span data-ttu-id="c1f82-169">V hello stránky publikování, vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="c1f82-169">In hello publish page, select **Publish**.</span></span>

<span data-ttu-id="c1f82-170">Po dokončení publikování aplikace Visual Studio spustí adresu URL prohlížeče toohello hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1f82-170">When publishing completes, Visual Studio launches a browser toohello URL of hello web app.</span></span>

![Aktualizovaná webová aplikace ASP.NET v Azure](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="manage-hello-azure-web-app"></a><span data-ttu-id="c1f82-172">Spravovat hello webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="c1f82-172">Manage hello Azure web app</span></span>

<span data-ttu-id="c1f82-173">Přejděte toohello <a href="https://portal.azure.com" target="_blank">portál Azure</a> toomanage hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1f82-173">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app.</span></span>

<span data-ttu-id="c1f82-174">Hello levé nabídce vyberte **App Services**a pak vyberte název hello Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1f82-174">From hello left menu, select **App Services**, and then select hello name of your Azure web app.</span></span>

![Portálu tooAzure webové aplikace](./media/app-service-web-get-started-dotnet/access-portal.png)

<span data-ttu-id="c1f82-176">Zobrazí se stránka s přehledem vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1f82-176">You see your web app's Overview page.</span></span> <span data-ttu-id="c1f82-177">Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="c1f82-177">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Okno App Service na webu Azure Portal](./media/app-service-web-get-started-dotnet/web-app-blade.png)

<span data-ttu-id="c1f82-179">levé nabídce Hello obsahuje různé stránky pro konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1f82-179">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="c1f82-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c1f82-180">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c1f82-181">ASP.NET s databází SQL Database</span><span class="sxs-lookup"><span data-stu-id="c1f82-181">ASP.NET with SQL Database</span></span>](app-service-web-tutorial-dotnet-sqldatabase.md)
