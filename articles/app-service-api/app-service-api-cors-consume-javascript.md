---
title: "aaaCORS podporují ve službě App Service | Microsoft Docs"
description: "Zjistěte, jak podporují toouse CORS v Azure Azure App Service."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 4f980a97-b9f5-4d1d-87ab-82b60bb96e1c
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/27/2016
ms.author: alkarche
ms.openlocfilehash: c229378b75840bc0f7b2eefc3df3031233f9b494
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-api-app-from-javascript-using-cors"></a><span data-ttu-id="008e6-103">Využití aplikace API z JavaScriptu pomocí CORS</span><span class="sxs-lookup"><span data-stu-id="008e6-103">Consume an API app from JavaScript using CORS</span></span>
<span data-ttu-id="008e6-104">Služby App Service má integrovanou podporu pro [mezi sdílení prostředků zdroji (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), což umožňuje JavaScript klienti toomake napříč doménami volá tooAPIs, které jsou hostované v aplikacích API.</span><span class="sxs-lookup"><span data-stu-id="008e6-104">App Service offers built-in support for [Cross Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), which enables JavaScript clients toomake cross-domain calls tooAPIs that are hosted in API apps.</span></span> <span data-ttu-id="008e6-105">Služby App Service umožňuje nakonfigurovat tooyour CORS přístup k rozhraní API bez psaní kódu v rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="008e6-105">App Service lets you configure CORS access tooyour API without writing any code in your API.</span></span>

<span data-ttu-id="008e6-106">Tento článek obsahuje dvě části:</span><span class="sxs-lookup"><span data-stu-id="008e6-106">This article contains two sections:</span></span>

* <span data-ttu-id="008e6-107">Hello [jak tooconfigure CORS](#corsconfig) část vysvětluje v obecné postupy tooconfigure CORS pro libovolnou aplikaci API, webovou aplikaci nebo mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="008e6-107">hello [How tooconfigure CORS](#corsconfig) section explains in general how tooconfigure CORS for any API app, web app, or mobile app.</span></span> <span data-ttu-id="008e6-108">Vztahuje stejnou měrou tooall rozhraní, které jsou podporovány službou App Service, včetně .NET, Node.js a Java.</span><span class="sxs-lookup"><span data-stu-id="008e6-108">It applies equally tooall frameworks that are supported by App Service, including .NET, Node.js, and Java.</span></span> 
* <span data-ttu-id="008e6-109">Počínaje hello [pokračování úvodních kurzů .NET hello](#tutorialstart) části článku hello je kurz, který ukazuje CORS podpory nebyla [hello první aplikace API kurzu Začínáme ](app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="008e6-109">Starting with hello [Continuing hello .NET getting-started tutorials](#tutorialstart) section, hello article is a tutorial that demonstrates CORS support by building on what you did in [hello first API Apps getting started tutorial](app-service-api-dotnet-get-started.md).</span></span> 

## <span data-ttu-id="008e6-110"><a id="corsconfig"></a>Jak tooconfigure CORS v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="008e6-110"><a id="corsconfig"></a> How tooconfigure CORS in Azure App Service</span></span>
<span data-ttu-id="008e6-111">CORS můžete nakonfigurovat v hello portál Azure nebo pomocí [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) nástroje.</span><span class="sxs-lookup"><span data-stu-id="008e6-111">You can configure CORS in hello Azure portal or by using [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) tools.</span></span>

#### <a name="configure-cors-in-hello-azure-portal"></a><span data-ttu-id="008e6-112">Konfigurace CORS v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="008e6-112">Configure CORS in hello Azure portal</span></span>
1. <span data-ttu-id="008e6-113">Přejděte v prohlížeči toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="008e6-113">In a browser go toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="008e6-114">Klikněte na tlačítko **App Services**a potom klikněte na název hello vaší aplikace API.</span><span class="sxs-lookup"><span data-stu-id="008e6-114">Click **App Services**, and then click hello name of your API app.</span></span>
   
    ![Výběr aplikace API na portálu](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. <span data-ttu-id="008e6-116">V hello **nastavení** okno, které se otevře napravo toohello od hello **aplikace API** okno, najít hello **rozhraní API** části a potom klikněte na **CORS**.</span><span class="sxs-lookup"><span data-stu-id="008e6-116">In hello **Settings** blade that opens toohello right of hello **API app** blade, find hello **API** section, and then click **CORS**.</span></span>
   
   ![Výběr CORS v okně Nastavení](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. <span data-ttu-id="008e6-118">Hello textového pole zadejte hello nebo adresy URL, které chcete tooallow JavaScript volání toocome z.</span><span class="sxs-lookup"><span data-stu-id="008e6-118">In hello text box enter hello URL or URLs that you want tooallow JavaScript calls toocome from.</span></span>

    <span data-ttu-id="008e6-119">Například pokud jste nasadili JavaScript aplikace tooa webovou aplikaci s názvem todolistangular, zadejte "https://todolistangular.azurewebsites.net".</span><span class="sxs-lookup"><span data-stu-id="008e6-119">For example, if you deployed your JavaScript application tooa web app named todolistangular, enter "https://todolistangular.azurewebsites.net".</span></span> <span data-ttu-id="008e6-120">Jako alternativu můžete zadat toospecify hvězdičky (*), jsou podmínky přijaty všechny zdrojové domény.</span><span class="sxs-lookup"><span data-stu-id="008e6-120">As an alternative, you can enter an asterisk (*) toospecify that all origin domains are accepted.</span></span>


1. <span data-ttu-id="008e6-121">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="008e6-121">Click **Save**.</span></span>
   
   ![Kliknutí na Uložit](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   <span data-ttu-id="008e6-123">Po kliknutí na tlačítko **Uložit**, bude aplikace hello rozhraní API přijímat JavaScript volání z hello zadané adresy URL.</span><span class="sxs-lookup"><span data-stu-id="008e6-123">After you click **Save**, hello API app will accept JavaScript calls from hello specified URLs.</span></span>

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a><span data-ttu-id="008e6-124">Konfigurace CORS pomocí nástrojů, které nabízí Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="008e6-124">Configure CORS by using Azure Resource Manager tools</span></span>
<span data-ttu-id="008e6-125">CORS můžete také nakonfigurovat pro aplikaci API pomocí [šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) v nástrojích příkazového řádku, jako [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) a hello [rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="008e6-125">You can also configure CORS for an API app by using [Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) in command line tools such as [Azure PowerShell](/powershell/azureps-cmdlets-docs) and hello [Azure CLI](../cli-install-nodejs.md).</span></span> 

<span data-ttu-id="008e6-126">Příklad šablony Azure Resource Manager, která nastaví vlastnost CORS hello otevřete hello [souboru azuredeploy.json v úložišti hello pro ukázkovou aplikaci tohoto kurzu](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="008e6-126">For an example of an Azure Resource Manager template that sets hello CORS property, open hello [azuredeploy.json file in hello repository for this tutorial's sample application](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json).</span></span> <span data-ttu-id="008e6-127">Najděte oddíl hello hello šablony, která vypadá jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="008e6-127">Find hello section of hello template that looks like hello following example:</span></span>

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <span data-ttu-id="008e6-128"><a id="tutorialstart"></a>Pokračováním úvodní kurz .NET hello</span><span class="sxs-lookup"><span data-stu-id="008e6-128"><a id="tutorialstart"></a> Continuing hello .NET getting-started tutorial</span></span>
<span data-ttu-id="008e6-129">Pokud postupujete podle hello Node.js nebo Java Začínáme series pro aplikace API, musíte dokončené hello získávání sérii.</span><span class="sxs-lookup"><span data-stu-id="008e6-129">If you are following hello Node.js or Java getting-started series for API apps, you have completed hello getting started series.</span></span> <span data-ttu-id="008e6-130">Přeskočit toohello [další kroky](#next-steps) části toofind návrhy dalších kurzů k API Apps.</span><span class="sxs-lookup"><span data-stu-id="008e6-130">Skip toohello [Next steps](#next-steps) section toofind suggestions for further learning about API Apps.</span></span>

<span data-ttu-id="008e6-131">Hello zbývající část tohoto článku je pokračování hello .NET Začínáme series a předpokládá, že jste úspěšně dokončili [první kurz hello](app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="008e6-131">hello remainder of this article is a continuation of hello .NET getting-started series and assumes that you successfully completed [hello first tutorial](app-service-api-dotnet-get-started.md).</span></span>

## <a name="deploy-hello-todolistangular-project-tooa-new-web-app"></a><span data-ttu-id="008e6-132">Nasazení hello ToDoListAngular projektu tooa nové webové aplikace</span><span class="sxs-lookup"><span data-stu-id="008e6-132">Deploy hello ToDoListAngular project tooa new web app</span></span>
<span data-ttu-id="008e6-133">V [první kurz hello](app-service-api-dotnet-get-started.md), jste vytvořili aplikaci API střední vrstvy a aplikaci API datové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="008e6-133">In [hello first tutorial](app-service-api-dotnet-get-started.md), you created a middle tier API app and a data tier API app.</span></span> <span data-ttu-id="008e6-134">V tomto kurzu vytvoříte webovou aplikaci jednostránkové aplikace (SPA) aplikaci API střední vrstvy hello volání.</span><span class="sxs-lookup"><span data-stu-id="008e6-134">In this tutorial you create a single-page application (SPA) web app that calls hello middle tier API app.</span></span> <span data-ttu-id="008e6-135">Pro hello SPA toowork máte tooenable CORS na aplikaci API střední vrstvy hello.</span><span class="sxs-lookup"><span data-stu-id="008e6-135">For hello SPA toowork you have tooenable CORS on hello middle tier API app.</span></span> 

<span data-ttu-id="008e6-136">V hello [ukázkové aplikaci ToDoList](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list), je hello projekt ToDoListAngular jednoduchý klient AngularJS, který volá projekt webového rozhraní API ToDoListAPI střední vrstvy hello.</span><span class="sxs-lookup"><span data-stu-id="008e6-136">In hello [ToDoList sample application](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list), hello ToDoListAngular project is a simple AngularJS client that calls hello middle tier ToDoListAPI Web API project.</span></span> <span data-ttu-id="008e6-137">kód jazyka JavaScript v hello Hello *app/scripts/todoListSvc.js* souboru zavolá hello API pomocí zprostředkovatele protokolu HTTP AngularJS hello.</span><span class="sxs-lookup"><span data-stu-id="008e6-137">hello JavaScript code in hello *app/scripts/todoListSvc.js* file calls hello API by using hello AngularJS HTTP provider.</span></span> 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 

            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-hello-todolistangular-project"></a><span data-ttu-id="008e6-138">Vytvoření nové webové aplikace pro projekt ToDoListAngular hello</span><span class="sxs-lookup"><span data-stu-id="008e6-138">Create a new web app for hello ToDoListAngular project</span></span>
<span data-ttu-id="008e6-139">Hello postup toocreate novou webovou aplikaci služby App Service a nasazení projektu tooit je podobné toowhat jste viděli pro [vytvoření a nasazení aplikace API hello prvního kurzu této série](app-service-api-dotnet-get-started.md#createapiapp).</span><span class="sxs-lookup"><span data-stu-id="008e6-139">hello procedure toocreate a new App Service web app and deploy a project tooit is similar toowhat you saw for [creating and deploying an API app in hello first tutorial in this series](app-service-api-dotnet-get-started.md#createapiapp).</span></span> <span data-ttu-id="008e6-140">Hello pouze rozdíl je tento typ aplikace hello je **webové aplikace** místo **aplikace API**.</span><span class="sxs-lookup"><span data-stu-id="008e6-140">hello only difference is that hello app type is **Web App** instead of **API App**.</span></span>  <span data-ttu-id="008e6-141">Snímky obrazovky hello dialogů najdete v tématu</span><span class="sxs-lookup"><span data-stu-id="008e6-141">For screen shots of hello dialogs, see</span></span> 

1. <span data-ttu-id="008e6-142">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ToDoListAngular hello a pak klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="008e6-142">In **Solution Explorer**, right-click hello ToDoListAngular project, and then click **Publish**.</span></span>
2. <span data-ttu-id="008e6-143">V hello **profil** kartě hello **Publikovat Web** průvodce, klikněte na tlačítko **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="008e6-143">In hello **Profile** tab of hello **Publish Web** wizard, click **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="008e6-144">V hello **služby App Service** dialogové okno, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="008e6-144">In hello **App Service** dialog box, click **New**.</span></span>
4. <span data-ttu-id="008e6-145">V hello **hostitelský** kartě hello **vytvořit službu App Service** dialogovém okně zadejte **název webové aplikace** který bude v hello *azurewebsites.net* domény.</span><span class="sxs-lookup"><span data-stu-id="008e6-145">In hello **Hosting** tab of hello **Create App Service** dialog box, enter a **Web App Name** that is unique in hello *azurewebsites.net* domain.</span></span> 
5. <span data-ttu-id="008e6-146">Zvolte hello Azure **předplatné** chcete toowork s.</span><span class="sxs-lookup"><span data-stu-id="008e6-146">Choose hello Azure **Subscription** you want toowork with.</span></span>
6. <span data-ttu-id="008e6-147">V hello **skupiny prostředků** rozevíracího seznamu vyberte hello stejnou skupinu prostředků, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="008e6-147">In hello **Resource Group** drop-down list, choose hello same resource group you created earlier.</span></span>
7. <span data-ttu-id="008e6-148">V hello **plán služby App Service** rozevíracího seznamu vyberte hello stejný plán, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="008e6-148">In hello **App Service Plan** drop-down list, choose hello same plan you created earlier.</span></span> 
8. <span data-ttu-id="008e6-149">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="008e6-149">Click **Create**.</span></span>
   
    <span data-ttu-id="008e6-150">Visual Studio vytvoří hello webovou aplikaci, vytvoří pro ni profil publikování a zobrazí hello **připojení** krok hello **Publikovat Web** průvodce.</span><span class="sxs-lookup"><span data-stu-id="008e6-150">Visual Studio creates hello web app, creates a publish profile for it, and displays hello **Connection** step of hello **Publish Web** wizard.</span></span>
   
    <span data-ttu-id="008e6-151">Na tlačítko **Publikovat** ještě neklikejte.</span><span class="sxs-lookup"><span data-stu-id="008e6-151">Don't click **Publish** yet.</span></span> <span data-ttu-id="008e6-152">V následující části hello můžete nakonfigurovat hello nové webové aplikace toocall hello API střední vrstvy aplikaci, která běží ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="008e6-152">In hello following section, you configure hello new web app toocall hello middle tier API app that is running in App Service.</span></span> 

### <a name="set-hello-middle-tier-url-in-web-app-settings"></a><span data-ttu-id="008e6-153">Nastavení adresy URL střední vrstvy hello v nastavení webové aplikace</span><span class="sxs-lookup"><span data-stu-id="008e6-153">Set hello middle tier URL in web app settings</span></span>
1. <span data-ttu-id="008e6-154">Přejděte toohello [portál Azure](https://portal.azure.com/)a potom přejděte toohello **webové aplikace** okně hello webové aplikace, které jste vytvořili projekt TodoListAngular (front-endu) toohost hello.</span><span class="sxs-lookup"><span data-stu-id="008e6-154">Go toohello [Azure portal](https://portal.azure.com/), and then navigate toohello **Web App** blade for hello web app that you created toohost hello TodoListAngular (front end) project.</span></span>
2. <span data-ttu-id="008e6-155">Klikněte na **Nastavení > Nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="008e6-155">Click **Settings > Application Settings**.</span></span>
3. <span data-ttu-id="008e6-156">V hello **nastavení aplikace** přidejte hello následující klíč a hodnotu:</span><span class="sxs-lookup"><span data-stu-id="008e6-156">In hello **App settings** section, add hello following key and value:</span></span>
   
   | <span data-ttu-id="008e6-157">Klíč</span><span class="sxs-lookup"><span data-stu-id="008e6-157">Key</span></span> | <span data-ttu-id="008e6-158">Hodnota</span><span class="sxs-lookup"><span data-stu-id="008e6-158">Value</span></span> | <span data-ttu-id="008e6-159">Příklad</span><span class="sxs-lookup"><span data-stu-id="008e6-159">Example</span></span> |
   | --- | --- | --- |
   | <span data-ttu-id="008e6-160">toDoListAPIURL</span><span class="sxs-lookup"><span data-stu-id="008e6-160">toDoListAPIURL</span></span> |<span data-ttu-id="008e6-161">https://{název vaší aplikace API střední vrstvy}.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="008e6-161">https://{your middle tier API app name}.azurewebsites.net</span></span> |<span data-ttu-id="008e6-162">https://todolistapi0121.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="008e6-162">https://todolistapi0121.azurewebsites.net</span></span> |
4. <span data-ttu-id="008e6-163">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="008e6-163">Click **Save**.</span></span>
   
    <span data-ttu-id="008e6-164">Při spuštění hello kódu v Azure, tato hodnota přepíše adresu URL hello místního hostitele, který je v hello *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="008e6-164">When hello code runs in Azure, this value overrides hello localhost URL that is in hello *Web.config* file.</span></span> 
   
    <span data-ttu-id="008e6-165">Hello kód, který získá hodnotu nastavení hello je v *index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="008e6-165">hello code that gets hello setting value is in *index.cshtml*:</span></span>
   
        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>
   
    <span data-ttu-id="008e6-166">Hello kód v *todoListSvc.js* nastavení hello používá:</span><span class="sxs-lookup"><span data-stu-id="008e6-166">hello code in *todoListSvc.js* uses hello setting:</span></span>
   
        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-hello-todolistangular-web-project-toohello-new-web-app"></a><span data-ttu-id="008e6-167">Nasazení hello webového projektu toohello nové webové aplikace ToDoListAngular</span><span class="sxs-lookup"><span data-stu-id="008e6-167">Deploy hello ToDoListAngular web project toohello new web app</span></span>
* <span data-ttu-id="008e6-168">V sadě Visual Studio v hello **připojení** krok hello **Publikovat Web** průvodce, klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="008e6-168">In Visual Studio, in hello **Connection** step of hello **Publish Web** wizard, click **Publish**.</span></span>
  
   <span data-ttu-id="008e6-169">Visual Studio nasadí hello projektu toohello nové webové aplikace ToDoListAngular a otevře adresu URL prohlížeče toohello hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="008e6-169">Visual Studio deploys hello ToDoListAngular project toohello new web app and opens a browser toohello URL of hello web app.</span></span> 

### <a name="test-hello-application-without-cors-enabled"></a><span data-ttu-id="008e6-170">Testovací aplikace hello bez povolení CORS</span><span class="sxs-lookup"><span data-stu-id="008e6-170">Test hello application without CORS enabled</span></span>
1. <span data-ttu-id="008e6-171">Ve vývojářských nástrojích prohlížeče otevřete okno konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="008e6-171">In your browser Developer Tools, open hello Console window.</span></span>
2. <span data-ttu-id="008e6-172">V okně prohlížeče hello, která zobrazuje hello uživatelské rozhraní AngularJS, klikněte na tlačítko hello **tooDo seznamu** odkaz.</span><span class="sxs-lookup"><span data-stu-id="008e6-172">In hello browser window that displays hello AngularJS UI, click hello **tooDo List** link.</span></span>
   
    <span data-ttu-id="008e6-173">Hello kódu jazyka JavaScript se pokusí toocall hello aplikaci API střední vrstvy, ale hello volání selže, protože hello front-end běží v jiné doméně než hello zpět end.</span><span class="sxs-lookup"><span data-stu-id="008e6-173">hello JavaScript code tries toocall hello middle tier API app, but hello call fails because hello front end is running in a different domain than hello back end.</span></span> <span data-ttu-id="008e6-174">okno konzoly vývojářských nástrojů prohlížeče Hello ukazuje cross-origin chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="008e6-174">hello browser's Developer Tools Console window shows a cross-origin error message.</span></span>
   
    ![Chybová zpráva nepůvodního zdroje](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-hello-middle-tier-api-app"></a><span data-ttu-id="008e6-176">Konfigurace CORS pro aplikaci API střední vrstvy hello</span><span class="sxs-lookup"><span data-stu-id="008e6-176">Configure CORS for hello middle tier API app</span></span>
<span data-ttu-id="008e6-177">V této části nakonfigurujete nastavení CORS hello v Azure pro aplikaci ToDoListAPI API střední vrstvy hello.</span><span class="sxs-lookup"><span data-stu-id="008e6-177">In this section, you configure hello CORS setting in Azure for hello middle tier ToDoListAPI API app.</span></span> <span data-ttu-id="008e6-178">Toto nastavení povolí hello střední vrstvy volání rozhraní API aplikace tooreceive JavaScript z webové aplikace hello, kterou jste vytvořili pro projekt ToDoListAngular hello.</span><span class="sxs-lookup"><span data-stu-id="008e6-178">This setting will allow hello middle tier API app tooreceive JavaScript calls from hello web app that you created for hello ToDoListAngular project.</span></span>

1. <span data-ttu-id="008e6-179">Přejděte v prohlížeči, toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="008e6-179">In a browser, go toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="008e6-180">Klikněte na tlačítko **App Services**a potom klikněte na hello rozhraní API ToDoListAPI (střední úroveň) aplikace.</span><span class="sxs-lookup"><span data-stu-id="008e6-180">Click **App Services**, and then click hello ToDoListAPI (middle tier) API app.</span></span>
   
    ![Výběr aplikace API na portálu](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. <span data-ttu-id="008e6-182">V hello **nastavení** okno, které se otevře napravo toohello od hello **aplikace API** okno, najít hello **rozhraní API** části a potom klikněte na **CORS**.</span><span class="sxs-lookup"><span data-stu-id="008e6-182">In hello **Settings** blade that opens toohello right of hello **API app** blade, find hello **API** section, and then click **CORS**.</span></span>
   
   ![Výběr CORS na portálu](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. <span data-ttu-id="008e6-184">Hello textového pole zadejte adresu URL hello pro webové aplikace ToDoListAngular (front-endu) hello.</span><span class="sxs-lookup"><span data-stu-id="008e6-184">In hello text box, enter hello URL for hello ToDoListAngular (front end) web app.</span></span> <span data-ttu-id="008e6-185">Například pokud jste nasadili hello projektu tooa webové aplikace ToDoListAngular s názvem todolistangular0121, povolte volání z adresy URL hello `https://todolistangular0121.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="008e6-185">For example, if you deployed hello ToDoListAngular project tooa web app named todolistangular0121, allow calls from hello URL `https://todolistangular0121.azurewebsites.net`.</span></span>
   
   <span data-ttu-id="008e6-186">Jako alternativu můžete zadat toospecify hvězdičky (*), jsou podmínky přijaty všechny zdrojové domény.</span><span class="sxs-lookup"><span data-stu-id="008e6-186">As an alternative, you can enter an asterisk (*) toospecify that all origin domains are accepted.</span></span>
5. <span data-ttu-id="008e6-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="008e6-187">Click **Save**.</span></span>
   
   ![Kliknutí na Uložit](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   <span data-ttu-id="008e6-189">Po kliknutí na tlačítko **Uložit**, bude aplikace hello rozhraní API přijímat JavaScript volání z hello zadaná adresa URL.</span><span class="sxs-lookup"><span data-stu-id="008e6-189">After you click **Save**, hello API app will accept JavaScript calls from hello specified URL.</span></span> <span data-ttu-id="008e6-190">Na tomto snímku obrazovky aplikace hello ToDoListAPI0223 API přijímat volání klienta JavaScript z webové aplikace ToDoListAngular hello.</span><span class="sxs-lookup"><span data-stu-id="008e6-190">In this screen shot, hello ToDoListAPI0223 API app will accept JavaScript client calls from hello ToDoListAngular web app.</span></span>

### <a name="test-hello-application-with-cors-enabled"></a><span data-ttu-id="008e6-191">Testovací aplikace hello s povolením CORS</span><span class="sxs-lookup"><span data-stu-id="008e6-191">Test hello application with CORS enabled</span></span>
* <span data-ttu-id="008e6-192">Otevřete prohlížeč toohello adresy URL HTTPS webové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="008e6-192">Open a browser toohello HTTPS URL of hello web app.</span></span> 
  
    <span data-ttu-id="008e6-193">Tato aplikace hello čas umožňuje zobrazit, přidat, upravit a odstraňovat položky seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="008e6-193">This time hello application lets you view, add, edit, and delete to-do items.</span></span> 
  
    ![Stránka seznamu tooDo ukázkové aplikace](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a><span data-ttu-id="008e6-195">CORS služby App Service versus CORS webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="008e6-195">App Service CORS versus Web API CORS</span></span>
<span data-ttu-id="008e6-196">V projektu webového rozhraní API můžete nainstalovat hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) toospecify balíček NuGet v kódu kterých domén bude rozhraní API přijímat volání JavaScriptu.</span><span class="sxs-lookup"><span data-stu-id="008e6-196">In a Web API project, you can install hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet package toospecify in code which domains your API will accept JavaScript calls from.</span></span>

<span data-ttu-id="008e6-197">Podpora CORS webového rozhraní API je flexibilnější než podpora CORS služby App Service.</span><span class="sxs-lookup"><span data-stu-id="008e6-197">Web API CORS support is more flexible than App Service CORS support.</span></span> <span data-ttu-id="008e6-198">Můžete například určit různé povolené zdroje pro různé metody akcí, zatímco u CORS služby App Service zadáváte jednu sadu povolených zdrojů pro všechny metody aplikace API.</span><span class="sxs-lookup"><span data-stu-id="008e6-198">For example, in code you can specify different accepted origins for different action methods, while for App Service CORS you specify one set of accepted origins for all of an API app's methods.</span></span>

> [!NOTE]
> <span data-ttu-id="008e6-199">Nepokoušejte toouse CORS webového rozhraní API a CORS služby App Service v jedné aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="008e6-199">Don't try toouse both Web API CORS and App Service CORS in one API app.</span></span> <span data-ttu-id="008e6-200">CORS služby App Service bude mít přednost a CORS webového rozhraní API nebude mít žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="008e6-200">App Service CORS will take precedence and Web API CORS will have no effect.</span></span> <span data-ttu-id="008e6-201">Například pokud povolíte jednu zdrojovou doménu ve službě App Service a povolit všechny zdrojové domény v kódu webového rozhraní API, vaše aplikace Azure API bude pouze přijímat volání z hello domény, který jste zadali v Azure.</span><span class="sxs-lookup"><span data-stu-id="008e6-201">For example, if you enable one origin domain in App Service, and enable all origin domains in your Web API code, your Azure API app will only accept calls from hello domain you specified in Azure.</span></span>
> 
> 

### <a name="how-tooenable-cors-in-web-api-code"></a><span data-ttu-id="008e6-202">Jak tooenable CORS v kódu webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="008e6-202">How tooenable CORS in Web API code</span></span>
<span data-ttu-id="008e6-203">Hello následující kroky shrnují proces hello povolení podpory CORS webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="008e6-203">hello following steps summarize hello process for enabling Web API CORS support.</span></span> <span data-ttu-id="008e6-204">Další informace najdete v tématu [Povolení žádostí napříč zdroji ve webovém rozhraní API 2 ASP.NET](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).</span><span class="sxs-lookup"><span data-stu-id="008e6-204">For more information, see [Enabling Cross-Origin Requests in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).</span></span>

1. <span data-ttu-id="008e6-205">V projektu webového rozhraní API, nainstalovat hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="008e6-205">In a Web API project, install hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet package.</span></span>
2. <span data-ttu-id="008e6-206">Zahrnout `config.EnableCors()` řádek kódu hello **zaregistrovat** metoda hello **WebApiConfig** třídy, jako hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="008e6-206">Include a `config.EnableCors()` line of code in hello **Register** method of hello **WebApiConfig** class, as in hello following example.</span></span> 
   
        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
   
                // hello following line enables you toocontrol CORS by using Web API code
                config.EnableCors();
   
                // Web API routes
                config.MapHttpAttributeRoutes();
   
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }
3. <span data-ttu-id="008e6-207">Do kontroleru webového rozhraní API, přidejte `using` příkaz pro hello `System.Web.Http.Cors` obor názvů a přidejte hello `EnableCors` atribut toohello třídu nebo tooindividual metody akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="008e6-207">In your Web API controller, add a `using` statement for hello `System.Web.Http.Cors` namespace, and add hello `EnableCors` attribute toohello controller class or tooindividual action methods.</span></span> <span data-ttu-id="008e6-208">V následujícím příkladu hello platí podpora CORS toohello celý kontroler.</span><span class="sxs-lookup"><span data-stu-id="008e6-208">In hello following example, CORS support applies toohello entire controller.</span></span>
   
        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController

## <a name="using-azure-api-management-with-api-apps"></a><span data-ttu-id="008e6-209">Použití služby API Management Azure pro aplikace API</span><span class="sxs-lookup"><span data-stu-id="008e6-209">Using Azure API Management with API Apps</span></span>
<span data-ttu-id="008e6-210">Pokud používáte Azure API Management s aplikací API, nakonfigurujte CORS ve službě API Management místo v aplikaci API hello.</span><span class="sxs-lookup"><span data-stu-id="008e6-210">If you use Azure API Management with an API app, configure CORS in API Management instead of in hello API app.</span></span> <span data-ttu-id="008e6-211">Další informace najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="008e6-211">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="008e6-212">Přehled služby Azure API Management (video: CORS začíná ve 12:10)</span><span class="sxs-lookup"><span data-stu-id="008e6-212">Azure API Management Overview (video: CORS starts at 12:10)</span></span>](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [<span data-ttu-id="008e6-213">Mezidoménové zásady služby API Management</span><span class="sxs-lookup"><span data-stu-id="008e6-213">API Management cross domain policies</span></span>](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)

## <a name="troubleshooting"></a><span data-ttu-id="008e6-214">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="008e6-214">Troubleshooting</span></span>
<span data-ttu-id="008e6-215">V případě, že při procházení tohoto kurzu narazíte na problém, můžete ho zkusit vyřešit pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="008e6-215">In case you run into a problem while going through this tutorial, here are some troubleshooting ideas.</span></span>

* <span data-ttu-id="008e6-216">Ujistěte se, že používáte nejnovější verzi hello hello [Azure SDK pro .NET pro Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).</span><span class="sxs-lookup"><span data-stu-id="008e6-216">Make sure that you're using hello latest version of hello [Azure SDK for .NET for Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).</span></span>
* <span data-ttu-id="008e6-217">Ujistěte se, zda jste zadali `https` v hello nastavení CORS a ujistěte se, že používáte `https` toorun hello front-endové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="008e6-217">Make sure that you entered `https` in hello CORS setting, and make sure that you're using `https` toorun hello front-end web app.</span></span>
* <span data-ttu-id="008e6-218">Ujistěte se, zda jste zadali nastavení CORS hello hello aplikaci API střední vrstvy, není v hello front-endové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="008e6-218">Make sure that you entered hello CORS setting in hello middle tier API app, not in hello front-end web app.</span></span>
* <span data-ttu-id="008e6-219">Pokud konfigurujete CORS v kódu aplikace a služby Azure App Service, Všimněte si, že přepíše nastavení CORS v App Service hello, vaše změny v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="008e6-219">If you're configuring CORS in both application code and Azure App Service, note that hello App Service CORS setting will override whatever you're doing in application code.</span></span> 

<span data-ttu-id="008e6-220">toolearn Další informace o funkcích Visual Studio, které usnadňují řešení potíží, najdete v části [aplikacemi řešení potíží s Azure App Service v sadě Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="008e6-220">toolearn more about Visual Studio features that simplify troubleshooting, see [Troubleshooting Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="008e6-221">Další kroky</span><span class="sxs-lookup"><span data-stu-id="008e6-221">Next steps</span></span>
<span data-ttu-id="008e6-222">V tomto článku jste viděli, jak tooenable CORS služby App Service podporuje, aby kód JavaScript klienta můžete volat rozhraní API v jiné doméně.</span><span class="sxs-lookup"><span data-stu-id="008e6-222">In this article, you saw how tooenable App Service CORS support so that client JavaScript code can call an API in a different domain.</span></span> <span data-ttu-id="008e6-223">Další informace o aplikacích API, přečtěte si hello toolearn [tooauthentication Úvod ve službě App Service](../app-service/app-service-authentication-overview.md), a potom přejděte toohello [ověřování uživatelů pro aplikace API](app-service-api-dotnet-user-principal-auth.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="008e6-223">toolearn more about API apps, read hello [introduction tooauthentication in App Service](../app-service/app-service-authentication-overview.md), and then go toohello [user authentication for API apps](app-service-api-dotnet-user-principal-auth.md) tutorial.</span></span>

