---
title: aaaCreate aplikace .NET pro Service Fabric | Microsoft Docs
description: "Zjistěte, jak toocreate aplikace pomocí ASP.NET Core, která je front-end a spolehlivé služby stavová back-end a nasazení clusteru tooa aplikace hello."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: ryanwi, mikhegn
ms.openlocfilehash: bab331b9f8616c50a2794b6c048aace15579c8b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a><span data-ttu-id="70e05-103">Vytvoření a nasazení aplikace pomocí služby front-endové webové rozhraní API ASP.NET Core a stavové služby back-end</span><span class="sxs-lookup"><span data-stu-id="70e05-103">Create and deploy an application with an ASP.NET Core Web API front-end service and a stateful back-end service</span></span>
<span data-ttu-id="70e05-104">V tomto kurzu je součástí, jednu z řady.</span><span class="sxs-lookup"><span data-stu-id="70e05-104">This tutorial is part one of a series.</span></span>  <span data-ttu-id="70e05-105">Se dozvíte, jak toocreate aplikace Azure Service Fabric pomocí webového rozhraní API ASP.NET Core přední ukončení a stavové služby back-end toostore vaše data.</span><span class="sxs-lookup"><span data-stu-id="70e05-105">You will learn how toocreate an Azure Service Fabric application with an ASP.NET Core Web API front end and a stateful back-end service toostore your data.</span></span> <span data-ttu-id="70e05-106">Jakmile budete hotovi, máte hlasovací aplikaci s ASP.NET Core web, který je front-end, který uloží výsledků hlasování ve stavové služby back-end v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="70e05-106">When you're finished, you have a voting application with an ASP.NET Core web front-end that saves voting results in a stateful back-end service in hello cluster.</span></span> <span data-ttu-id="70e05-107">Pokud nechcete, aby toomanually vytvořit hello hlasovací aplikaci, můžete [stáhnout hello zdrojového kódu](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) hello byla dokončena aplikace a přeskočit příliš[provede hello hlasování ukázkovou aplikaci](#walkthrough_anchor).</span><span class="sxs-lookup"><span data-stu-id="70e05-107">If you don't want toomanually create hello voting application, you can [download hello source code](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) for hello completed application and skip ahead too[Walk through hello voting sample application](#walkthrough_anchor).</span></span>

![Diagram aplikace](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

<span data-ttu-id="70e05-109">V rámci jednu z řady hello, zjistíte, jak:</span><span class="sxs-lookup"><span data-stu-id="70e05-109">In part one of hello series, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="70e05-110">Vytvoření služby webového rozhraní API ASP.NET Core jako stavové spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="70e05-110">Create an ASP.NET Core Web API service as a stateful reliable service</span></span>
> * <span data-ttu-id="70e05-111">Vytvoření služby webové aplikace ASP.NET Core jako bezstavové webové služby</span><span class="sxs-lookup"><span data-stu-id="70e05-111">Create an ASP.NET Core Web Application service as a stateless web service</span></span>
> * <span data-ttu-id="70e05-112">Pomocí toocommunicate reverzní proxy server hello hello stavové služby</span><span class="sxs-lookup"><span data-stu-id="70e05-112">Use hello reverse proxy toocommunicate with hello stateful service</span></span>

<span data-ttu-id="70e05-113">V této série kurzu zjistíte, jak:</span><span class="sxs-lookup"><span data-stu-id="70e05-113">In this tutorial series you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="70e05-114">Sestavení aplikace .NET Service Fabric</span><span class="sxs-lookup"><span data-stu-id="70e05-114">Build a .NET Service Fabric application</span></span>
> * [<span data-ttu-id="70e05-115">Nasazení vzdáleného clusteru tooa hello aplikace</span><span class="sxs-lookup"><span data-stu-id="70e05-115">Deploy hello application tooa remote cluster</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [<span data-ttu-id="70e05-116">Konfigurace CI/CD pomocí Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="70e05-116">Configure CI/CD using Visual Studio Team Services</span></span>](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a><span data-ttu-id="70e05-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="70e05-117">Prerequisites</span></span>
<span data-ttu-id="70e05-118">Před zahájením tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="70e05-118">Before you begin this tutorial:</span></span>
- <span data-ttu-id="70e05-119">Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="70e05-119">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="70e05-120">[Nainstalovat Visual Studio 2017](https://www.visualstudio.com/) a nainstalujte hello **Azure development** a **ASP.NET a webové vývoj** úlohy.</span><span class="sxs-lookup"><span data-stu-id="70e05-120">[Install Visual Studio 2017](https://www.visualstudio.com/) and install hello **Azure development** and **ASP.NET and web development** workloads.</span></span>
- [<span data-ttu-id="70e05-121">Nainstalujte hello Service Fabric SDK</span><span class="sxs-lookup"><span data-stu-id="70e05-121">Install hello Service Fabric SDK</span></span>](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a><span data-ttu-id="70e05-122">Vytvoření služby ASP.NET Web API jako spolehlivě.</span><span class="sxs-lookup"><span data-stu-id="70e05-122">Create an ASP.NET Web API service as a reliable service</span></span>
<span data-ttu-id="70e05-123">Nejprve vytvořte hello webový front-end Dobrý den, hlasování aplikace pomocí ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70e05-123">First, create hello web front-end of hello voting application using ASP.NET Core.</span></span> <span data-ttu-id="70e05-124">ASP.NET Core je rozhraní vývoj webové lightweight, a platformy, můžete použít toocreate moderní webového uživatelského rozhraní a webovým rozhraním API.</span><span class="sxs-lookup"><span data-stu-id="70e05-124">ASP.NET Core is a lightweight, cross-platform web development framework that you can use toocreate modern web UI and web APIs.</span></span> <span data-ttu-id="70e05-125">tooget dokončení znalosti o tom, jak ASP.NET Core integruje se službou Service Fabric, důrazně doporučujeme čtení prostřednictvím hello [ASP.NET Core v Service Fabric spolehlivé služby](service-fabric-reliable-services-communication-aspnetcore.md) článku.</span><span class="sxs-lookup"><span data-stu-id="70e05-125">tooget a complete understanding of how ASP.NET Core integrates with Service Fabric, we strongly recommend reading through hello [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) article.</span></span> <span data-ttu-id="70e05-126">Teď můžete postupovat podle tohoto kurzu tooget rychle začít.</span><span class="sxs-lookup"><span data-stu-id="70e05-126">For now, you can follow this tutorial tooget started quickly.</span></span> <span data-ttu-id="70e05-127">toolearn Další informace o ASP.NET Core, najdete v části hello [ASP.NET základní dokumentace](https://docs.microsoft.com/aspnet/core/).</span><span class="sxs-lookup"><span data-stu-id="70e05-127">toolearn more about ASP.NET Core, see hello [ASP.NET Core Documentation](https://docs.microsoft.com/aspnet/core/).</span></span>

> [!NOTE]
> <span data-ttu-id="70e05-128">V tomto kurzu vychází z hello [ASP.NET Core nástrojů pro Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="70e05-128">This tutorial is based on hello [ASP.NET Core tools for Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc).</span></span> <span data-ttu-id="70e05-129">Hello .NET Core nástrojů pro Visual Studio 2015 se již nebude aktualizováno.</span><span class="sxs-lookup"><span data-stu-id="70e05-129">hello .NET Core tools for Visual Studio 2015 are no longer being updated.</span></span>

1. <span data-ttu-id="70e05-130">Spusťte sadu Visual Studio jako **správce**.</span><span class="sxs-lookup"><span data-stu-id="70e05-130">Launch Visual Studio as an **administrator**.</span></span>

2. <span data-ttu-id="70e05-131">Vytvoření projektu s **soubor**->**nový**->**projektu**</span><span class="sxs-lookup"><span data-stu-id="70e05-131">Create a project with **File**->**New**->**Project**</span></span>

3. <span data-ttu-id="70e05-132">V hello **nový projekt** dialogovém okně, vyberte **Cloud > aplikace Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="70e05-132">In hello **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

4. <span data-ttu-id="70e05-133">Název aplikace hello **Voting** a stiskněte klávesu **OK**.</span><span class="sxs-lookup"><span data-stu-id="70e05-133">Name hello application **Voting** and press **OK**.</span></span>

   ![Dialogové okno Nový projekt ve Visual Studiu](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. <span data-ttu-id="70e05-135">Na hello **nové služby Fabric** vyberte **bezstavové ASP.NET Core**a název vaší služby **VotingWeb**.</span><span class="sxs-lookup"><span data-stu-id="70e05-135">On hello **New Service Fabric Service** page, choose **Stateless ASP.NET Core**, and name your service **VotingWeb**.</span></span>
   
   ![Výběr webové služby ASP.NET v hello dialogové okno Nová služba](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. <span data-ttu-id="70e05-137">Další stránku Hello poskytuje sadu ASP.NET Core šablony projektů.</span><span class="sxs-lookup"><span data-stu-id="70e05-137">hello next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="70e05-138">V tomto kurzu zvolte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="70e05-138">For this tutorial, choose **Web Application**.</span></span> 
   
   ![Zvolte typ projektu ASP.NET](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   <span data-ttu-id="70e05-140">Visual Studio vytvoří aplikaci a projekt služby a zobrazí je v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="70e05-140">Visual Studio creates an application and a service project and displays them in Solution Explorer.</span></span>

   ![Průzkumník řešení po vytvoření aplikace ASP.NET základní webového rozhraní API služby]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-toohello-votingweb-service"></a><span data-ttu-id="70e05-142">Přidání AngularJS toohello VotingWeb služby</span><span class="sxs-lookup"><span data-stu-id="70e05-142">Add AngularJS toohello VotingWeb service</span></span>
<span data-ttu-id="70e05-143">Přidat [AngularJS](http://angularjs.org/) tooyour služby pomocí integrovaných hello [Bower podporu](/aspnet/core/client-side/bower).</span><span class="sxs-lookup"><span data-stu-id="70e05-143">Add [AngularJS](http://angularjs.org/) tooyour service using hello built-in [Bower support](/aspnet/core/client-side/bower).</span></span> <span data-ttu-id="70e05-144">Otevřete *bower.json* a přidejte položky pro úhlová a úhlová bootstrap a pak změny uložte.</span><span class="sxs-lookup"><span data-stu-id="70e05-144">Open *bower.json* and add entries for angular and angular-bootstrap, then save your changes.</span></span>

```json
{
  "name": "asp.net",
  "private": true,
  "dependencies": {
    "bootstrap": "3.3.7",
    "jquery": "2.2.0",
    "jquery-validation": "1.14.0",
    "jquery-validation-unobtrusive": "3.2.6",
    "angular": "v1.6.5",
    "angular-bootstrap": "v1.1.0"
  }
}
```
<span data-ttu-id="70e05-145">Při ukládání hello *bower.json* soubor, úhlová je nainstalován ve vašem projektu *wwwroot/lib* složky.</span><span class="sxs-lookup"><span data-stu-id="70e05-145">Upon saving hello *bower.json* file, Angular is installed in your project's *wwwroot/lib* folder.</span></span> <span data-ttu-id="70e05-146">Kromě toho je uveden v rámci hello *závislosti/Bower* složky.</span><span class="sxs-lookup"><span data-stu-id="70e05-146">Additionally, it is listed within hello *Dependencies/Bower* folder.</span></span>

### <a name="update-hello-sitejs-file"></a><span data-ttu-id="70e05-147">Aktualizujte soubor site.js hello</span><span class="sxs-lookup"><span data-stu-id="70e05-147">Update hello site.js file</span></span>
<span data-ttu-id="70e05-148">Otevřete hello *wwwroot/js/site.js* souboru.</span><span class="sxs-lookup"><span data-stu-id="70e05-148">Open hello *wwwroot/js/site.js* file.</span></span>  <span data-ttu-id="70e05-149">Nahraďte jeho obsah hello JavaScript používané hello domovské zobrazení:</span><span class="sxs-lookup"><span data-stu-id="70e05-149">Replace its contents with hello JavaScript used by hello Home views:</span></span>

```javascript
var app = angular.module('VotingApp', ['ui.bootstrap']);
app.run(function () { });

app.controller('VotingAppController', ['$rootScope', '$scope', '$http', '$timeout', function ($rootScope, $scope, $http, $timeout) {

    $scope.refresh = function () {
        $http.get('api/Votes?c=' + new Date().getTime())
            .then(function (data, status) {
                $scope.votes = data;
            }, function (data, status) {
                $scope.votes = undefined;
            });
    };

    $scope.remove = function (item) {
        $http.delete('api/Votes/' + item)
            .then(function (data, status) {
                $scope.refresh();
            })
    };

    $scope.add = function (item) {
        var fd = new FormData();
        fd.append('item', item);
        $http.put('api/Votes/' + item, fd, {
            transformRequest: angular.identity,
            headers: { 'Content-Type': undefined }
        })
            .then(function (data, status) {
                $scope.refresh();
                $scope.item = undefined;
            })
    };
}]);
```

### <a name="update-hello-indexcshtml-file"></a><span data-ttu-id="70e05-150">Soubor Index.cshtml hello aktualizace</span><span class="sxs-lookup"><span data-stu-id="70e05-150">Update hello Index.cshtml file</span></span>
<span data-ttu-id="70e05-151">Otevřete hello *Views/Home/Index.cshtml* soubor, hello zobrazení konkrétní domovskou toohello řadič.</span><span class="sxs-lookup"><span data-stu-id="70e05-151">Open hello *Views/Home/Index.cshtml* file, hello view specific toohello Home controller.</span></span>  <span data-ttu-id="70e05-152">Nahraďte jeho obsah hello následující a potom uložte změny.</span><span class="sxs-lookup"><span data-stu-id="70e05-152">Replace its contents with hello following, then save your changes.</span></span>

```html
@{
    ViewData["Title"] = "Service Fabric Voting Sample";
}

<div ng-controller="VotingAppController" ng-init="refresh()">
    <div class="container-fluid">
        <div class="row">
            <div class="col-xs-8 col-xs-offset-2 text-center">
                <h2>Service Fabric Voting Sample</h2>
            </div>
        </div>

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <form class="col-xs-12 center-block">
                    <div class="col-xs-6 form-group">
                        <input id="txtAdd" type="text" class="form-control" placeholder="Add voting option" ng-model="item" />
                    </div>
                    <button id="btnAdd" class="btn btn-default" ng-click="add(item)">
                        <span class="glyphicon glyphicon-plus" aria-hidden="true"></span>
                        Add
                    </button>
                </form>
            </div>
        </div>

        <hr />

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <div class="row">
                    <div class="col-xs-4">
                        Click toovote
                    </div>
                </div>
                <div class="row top-buffer" ng-repeat="vote in votes.data">
                    <div class="col-xs-8">
                        <button class="btn btn-success text-left btn-block" ng-click="add(vote.key)">
                            <span class="pull-left">
                                {{vote.key}}
                            </span>
                            <span class="badge pull-right">
                                {{vote.value}} Votes
                            </span>
                        </button>
                    </div>
                    <div class="col-xs-4">
                        <button class="btn btn-danger pull-right btn-block" ng-click="remove(vote.key)">
                            <span class="glyphicon glyphicon-remove" aria-hidden="true"></span>
                            Remove
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

### <a name="update-hello-layoutcshtml-file"></a><span data-ttu-id="70e05-153">Soubor _Layout.cshtml hello aktualizace</span><span class="sxs-lookup"><span data-stu-id="70e05-153">Update hello _Layout.cshtml file</span></span>
<span data-ttu-id="70e05-154">Otevřete hello *Views/Shared/_Layout.cshtml* soubor, hello výchozí rozložení pro aplikaci ASP.NET hello.</span><span class="sxs-lookup"><span data-stu-id="70e05-154">Open hello *Views/Shared/_Layout.cshtml* file, hello default layout for hello ASP.NET app.</span></span>  <span data-ttu-id="70e05-155">Nahraďte jeho obsah hello následující a potom uložte změny.</span><span class="sxs-lookup"><span data-stu-id="70e05-155">Replace its contents with hello following, then save your changes.</span></span>

```html
<!DOCTYPE html>
<html ng-app="VotingApp" xmlns:ng="http://angularjs.org">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"]</title>

    <link href="~/lib/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet" />
    <link href="~/css/site.css" rel="stylesheet" />

</head>
<body>
    <div class="container body-content">
        @RenderBody()
    </div>

    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/lib/angular/angular.js"></script>
    <script src="~/lib/angular-bootstrap/ui-bootstrap-tpls.js"></script>
    <script src="~/js/site.js"></script>

    @RenderSection("Scripts", required: false)
</body>
</html>
```

### <a name="update-hello-votingwebcs-file"></a><span data-ttu-id="70e05-156">Aktualizujte soubor VotingWeb.cs hello</span><span class="sxs-lookup"><span data-stu-id="70e05-156">Update hello VotingWeb.cs file</span></span>
<span data-ttu-id="70e05-157">Otevřete hello *VotingWeb.cs* souboru, který vytvoří hello ASP.NET Core tomuto webovému hostiteli uvnitř hello bezstavové služby pomocí hello WebListener webový server.</span><span class="sxs-lookup"><span data-stu-id="70e05-157">Open hello *VotingWeb.cs* file, which creates hello ASP.NET Core WebHost inside hello stateless service using hello WebListener web server.</span></span>  <span data-ttu-id="70e05-158">Přidat hello `using System.Net.Http;` direktivy toohello horní části souboru hello.</span><span class="sxs-lookup"><span data-stu-id="70e05-158">Add hello `using System.Net.Http;` directive toohello top of hello file.</span></span>  <span data-ttu-id="70e05-159">Nahraďte hello `CreateServiceInstanceListeners()` fungovat s hello následující a potom uložte změny.</span><span class="sxs-lookup"><span data-stu-id="70e05-159">Replace hello `CreateServiceInstanceListeners()` function with hello following, then save your changes.</span></span>

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
            {
                ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting WebListener on {url}");

                return new WebHostBuilder().UseWebListener()
                            .ConfigureServices(
                                services => services
                                    .AddSingleton<StatelessServiceContext>(serviceContext)
                                    .AddSingleton<HttpClient>())
                            .UseContentRoot(Directory.GetCurrentDirectory())
                            .UseStartup<Startup>()
                            .UseApplicationInsights()
                            .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                            .UseUrls(url)
                            .Build();
            }))
    };
}
```

### <a name="add-hello-votescontrollercs-file"></a><span data-ttu-id="70e05-160">Přidejte soubor VotesController.cs hello</span><span class="sxs-lookup"><span data-stu-id="70e05-160">Add hello VotesController.cs file</span></span>
<span data-ttu-id="70e05-161">Přidáte řadič, který definuje hlasujících akce.</span><span class="sxs-lookup"><span data-stu-id="70e05-161">Add a controller which defines voting actions.</span></span> <span data-ttu-id="70e05-162">Klikněte pravým tlačítkem na hello **řadiče** složku, pak vyberte **Přidat -> Nový položky -> třída**.</span><span class="sxs-lookup"><span data-stu-id="70e05-162">Right-click on hello **Controllers** folder, then select **Add->New item->Class**.</span></span>  <span data-ttu-id="70e05-163">Název souboru hello "VotesController.cs" a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="70e05-163">Name hello file "VotesController.cs" and click **Add**.</span></span>  <span data-ttu-id="70e05-164">Nahraďte obsah souboru hello hello následující a potom uložte změny.</span><span class="sxs-lookup"><span data-stu-id="70e05-164">Replace hello file contents with hello following, then save your changes.</span></span>  <span data-ttu-id="70e05-165">Dále v [soubor VotesController.cs hello aktualizace](#updatevotecontroller_anchor), tento soubor bude upravené tooread a zápisu hlasujících dat ze hello back endové službě.</span><span class="sxs-lookup"><span data-stu-id="70e05-165">Later, in [Update hello VotesController.cs file](#updatevotecontroller_anchor), this file will be modified tooread and write voting data from hello back-end service.</span></span>  <span data-ttu-id="70e05-166">Prozatím se vrátí hello řadiče zobrazení toohello dat statický řetězec.</span><span class="sxs-lookup"><span data-stu-id="70e05-166">For now, hello controller returns static string data toohello view.</span></span>

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using Newtonsoft.Json;
using System.Text;
using System.Net.Http;
using System.Net.Http.Headers;

namespace VotingWeb.Controllers
{
    [Produces("application/json")]
    [Route("api/Votes")]
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            List<KeyValuePair<string, int>> votes= new List<KeyValuePair<string, int>>();
            votes.Add(new KeyValuePair<string, int>("Pizza", 3));
            votes.Add(new KeyValuePair<string, int>("Ice cream", 4));

            return Json(votes);
        }
     }
}
```



### <a name="deploy-and-run-hello-application-locally"></a><span data-ttu-id="70e05-167">Nasazení a spuštění aplikace hello místně</span><span class="sxs-lookup"><span data-stu-id="70e05-167">Deploy and run hello application locally</span></span>
<span data-ttu-id="70e05-168">Teď můžete pokračovat a spustit aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="70e05-168">You can now go ahead and run hello application.</span></span> <span data-ttu-id="70e05-169">V sadě Visual Studio, stiskněte klávesu `F5` aplikace hello toodeploy pro ladění.</span><span class="sxs-lookup"><span data-stu-id="70e05-169">In Visual Studio, press `F5` toodeploy hello application for debugging.</span></span> <span data-ttu-id="70e05-170">`F5`selže, pokud nebyla dříve otevřete Visual Studio jako **správce**.</span><span class="sxs-lookup"><span data-stu-id="70e05-170">`F5` fails if you didn't previously open Visual Studio as **administrator**.</span></span>

> [!NOTE]
> <span data-ttu-id="70e05-171">Hello poprvé spustíte a nasadit hello aplikaci místně, Visual Studio vytvoří místní cluster pro ladění.</span><span class="sxs-lookup"><span data-stu-id="70e05-171">hello first time you run and deploy hello application locally, Visual Studio creates a local cluster for debugging.</span></span>  <span data-ttu-id="70e05-172">Vytvoření clusteru může trvat delší dobu.</span><span class="sxs-lookup"><span data-stu-id="70e05-172">Cluster creation may take some time.</span></span> <span data-ttu-id="70e05-173">Stav vytváření clusteru Hello se zobrazí v okně výstupu sady Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="70e05-173">hello cluster creation status is displayed in hello Visual Studio output window.</span></span>

<span data-ttu-id="70e05-174">Webové aplikace v tomto okamžiku by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="70e05-174">At this point, your web app should look like this:</span></span>

![ASP.NET Core front-endu](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

<span data-ttu-id="70e05-176">ladění aplikace hello toostop vraťte tooVisual Studio a stiskněte klávesu **Shift + F5**.</span><span class="sxs-lookup"><span data-stu-id="70e05-176">toostop debugging hello application, go back tooVisual Studio and press **Shift+F5**.</span></span>

## <a name="add-a-stateful-back-end-service-tooyour-application"></a><span data-ttu-id="70e05-177">Přidání aplikace tooyour stavové služby back-end</span><span class="sxs-lookup"><span data-stu-id="70e05-177">Add a stateful back-end service tooyour application</span></span>
<span data-ttu-id="70e05-178">Teď, když máme rozhraní ASP.NET Web API služby spuštěné v naší aplikaci, přejdeme dopředu a přidejte toostore stavové služby spolehlivé některá data v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70e05-178">Now that we have an ASP.NET Web API service running in our application, let's go ahead and add a stateful reliable service toostore some data in our application.</span></span>

<span data-ttu-id="70e05-179">Service Fabric vám umožní tooconsistently a spolehlivě ukládat vaše právo data uvnitř vaší služby pomocí spolehlivé kolekce.</span><span class="sxs-lookup"><span data-stu-id="70e05-179">Service Fabric allows you tooconsistently and reliably store your data right inside your service by using reliable collections.</span></span> <span data-ttu-id="70e05-180">Spolehlivé kolekce jsou sadu vysoce dostupné a spolehlivého kolekce tříd, které jsou známé tooanyone, který byl použit C# kolekce.</span><span class="sxs-lookup"><span data-stu-id="70e05-180">Reliable collections are a set of highly available and reliable collection classes that are familiar tooanyone who has used C# collections.</span></span>

<span data-ttu-id="70e05-181">V tomto kurzu vytvoříte službu, která ukládá hodnotu čítače v kolekci spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="70e05-181">In this tutorial, you create a service which stores a counter value in a reliable collection.</span></span>

1. <span data-ttu-id="70e05-182">V Průzkumníku řešení klikněte pravým tlačítkem na **služby** uvnitř hello projekt aplikace a zvolte **Přidat > Nový Service Fabric Service**.</span><span class="sxs-lookup"><span data-stu-id="70e05-182">In Solution Explorer, right-click **Services** within hello application project and choose **Add > New Service Fabric Service**.</span></span>
   
    ![Přidání nové služby tooan existující aplikace](./media/service-fabric-tutorial-create-dotnet-app/vs-add-new-service.png)

2. <span data-ttu-id="70e05-184">V hello **nové služby Fabric** dialogovém okně, vyberte **Stateful ASP.NET Core**a název služby hello **VotingData** a stiskněte klávesu **OK**.</span><span class="sxs-lookup"><span data-stu-id="70e05-184">In hello **New Service Fabric Service** dialog, choose **Stateful ASP.NET Core**, and name hello service **VotingData** and press **OK**.</span></span>

    ![Dialogové okno Nová služba ve Visual Studiu](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    <span data-ttu-id="70e05-186">Po vytvoření svůj projekt služby, máte dvě služby ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70e05-186">Once your service project is created, you have two services in your application.</span></span> <span data-ttu-id="70e05-187">Při dalším toobuild vaší aplikace, můžete přidat další služby v hello stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="70e05-187">As you continue toobuild your application, you can add more services in hello same way.</span></span> <span data-ttu-id="70e05-188">Každý může být nezávisle verzí a upgradovaný.</span><span class="sxs-lookup"><span data-stu-id="70e05-188">Each can be independently versioned and upgraded.</span></span>

3. <span data-ttu-id="70e05-189">Další stránku Hello poskytuje sadu ASP.NET Core šablony projektů.</span><span class="sxs-lookup"><span data-stu-id="70e05-189">hello next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="70e05-190">V tomto kurzu zvolte **webového rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="70e05-190">For this tutorial, choose **Web API**.</span></span>

    ![Zvolte typ projektu ASP.NET](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    <span data-ttu-id="70e05-192">Visual Studio vytvoří projekt služby a zobrazí je v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="70e05-192">Visual Studio creates a service project and displays them in Solution Explorer.</span></span>

    ![Průzkumník řešení](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-hello-votedatacontrollercs-file"></a><span data-ttu-id="70e05-194">Přidejte soubor VoteDataController.cs hello</span><span class="sxs-lookup"><span data-stu-id="70e05-194">Add hello VoteDataController.cs file</span></span>

<span data-ttu-id="70e05-195">V hello **VotingData** projektu klikněte pravým tlačítkem na hello **řadiče** složku, pak vyberte **Přidat -> Nový položky -> třída**.</span><span class="sxs-lookup"><span data-stu-id="70e05-195">In hello **VotingData** project right-click on hello **Controllers** folder, then select **Add->New item->Class**.</span></span> <span data-ttu-id="70e05-196">Název souboru hello "VoteDataController.cs" a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="70e05-196">Name hello file "VoteDataController.cs" and click **Add**.</span></span> <span data-ttu-id="70e05-197">Nahraďte obsah souboru hello hello následující a potom uložte změny.</span><span class="sxs-lookup"><span data-stu-id="70e05-197">Replace hello file contents with hello following, then save your changes.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.ServiceFabric.Data;
using System.Threading;
using Microsoft.ServiceFabric.Data.Collections;

namespace VotingData.Controllers
{
    [Route("api/[controller]")]
    public class VoteDataController : Controller
    {
        private readonly IReliableStateManager stateManager;

        public VoteDataController(IReliableStateManager stateManager)
        {
            this.stateManager = stateManager;
        }

        // GET api/VoteData
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            var ct = new CancellationToken();

            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                var list = await votesDictionary.CreateEnumerableAsync(tx);

                var enumerator = list.GetAsyncEnumerator();

                var result = new List<KeyValuePair<string, int>>();

                while (await enumerator.MoveNextAsync(ct))
                {
                    result.Add(enumerator.Current);
                }

                return Json(result);
            }
        }

        // PUT api/VoteData/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                await votesDictionary.AddOrUpdateAsync(tx, name, 1, (key, oldvalue) => oldvalue + 1);
                await tx.CommitAsync();
            }

            return new OkResult();
        }

        // DELETE api/VoteData/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                if (await votesDictionary.ContainsKeyAsync(tx, name))
                {
                    await votesDictionary.TryRemoveAsync(tx, name);
                    await tx.CommitAsync();
                    return new OkResult();
                }
                else
                {
                    return new NotFoundResult();
                }
            }
        }
    }
}
```


## <a name="connect-hello-services"></a><span data-ttu-id="70e05-198">Připojení služby hello</span><span class="sxs-lookup"><span data-stu-id="70e05-198">Connect hello services</span></span>
<span data-ttu-id="70e05-199">V tomto kroku další jsme připojí hello dvě služby a zkontrolujte hello front-endové webové aplikace získání a nastavení, hlasování informace z hello back endové službě.</span><span class="sxs-lookup"><span data-stu-id="70e05-199">In this next step, we will connect hello two services and make hello front-end Web application get and set voting information from hello back-end service.</span></span>

<span data-ttu-id="70e05-200">Service Fabric nabízí flexibilitu v tom, jak komunikovat se službami reliable services.</span><span class="sxs-lookup"><span data-stu-id="70e05-200">Service Fabric provides complete flexibility in how you communicate with reliable services.</span></span> <span data-ttu-id="70e05-201">V rámci jedné aplikace můžete mít služby, které jsou přístupné přes TCP.</span><span class="sxs-lookup"><span data-stu-id="70e05-201">Within a single application, you might have services that are accessible via TCP.</span></span> <span data-ttu-id="70e05-202">Dalším službám, které může být přístupné přes rozhraní HTTP REST API a stále dalších služeb může být přístupné přes webové sokety.</span><span class="sxs-lookup"><span data-stu-id="70e05-202">Other services that might be accessible via an HTTP REST API and still other services could be accessible via web sockets.</span></span> <span data-ttu-id="70e05-203">Pro informace o dostupných možnostech hello a hello kompromisy související se situací, viz [komunikaci se službou](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="70e05-203">For background on hello options available and hello tradeoffs involved, see [Communicating with services](service-fabric-connect-and-communicate-with-services.md).</span></span>

<span data-ttu-id="70e05-204">V tomto kurzu používáme [webového rozhraní API ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md).</span><span class="sxs-lookup"><span data-stu-id="70e05-204">In this tutorial, we are using [ASP.NET Core Web API](service-fabric-reliable-services-communication-aspnetcore.md).</span></span>

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-hello-votescontrollercs-file"></a><span data-ttu-id="70e05-205">Aktualizujte soubor VotesController.cs hello</span><span class="sxs-lookup"><span data-stu-id="70e05-205">Update hello VotesController.cs file</span></span>
<span data-ttu-id="70e05-206">V hello **VotingWeb** projekt, otevřete hello *Controllers/VotesController.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="70e05-206">In hello **VotingWeb** project, open hello *Controllers/VotesController.cs* file.</span></span>  <span data-ttu-id="70e05-207">Nahraďte hello `VotesController` třídy definice obsah s hello následující a potom uložte změny.</span><span class="sxs-lookup"><span data-stu-id="70e05-207">Replace hello `VotesController` class definition contents with hello following, then save your changes.</span></span>

```csharp
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;
        string serviceProxyUrl = "http://localhost:19081/Voting/VotingData/api/VoteData";
        string partitionKind = "Int64Range";
        string partitionKey = "0";

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            IEnumerable<KeyValuePair<string, int>> votes;

            HttpResponseMessage response = await this.httpClient.GetAsync($"{serviceProxyUrl}?PartitionKind={partitionKind}&PartitionKey={partitionKey}");

            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int)response.StatusCode);
            }

            votes = JsonConvert.DeserializeObject<List<KeyValuePair<string, int>>>(await response.Content.ReadAsStringAsync());

            return Json(votes);
        }

        // PUT: api/Votes/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            string payload = $"{{ 'name' : '{name}' }}";
            StringContent putContent = new StringContent(payload, Encoding.UTF8, "application/json");
            putContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");

            string proxyUrl = $"{serviceProxyUrl}/{name}?PartitionKind={partitionKind}&PartitionKey={partitionKey}";

            HttpResponseMessage response = await this.httpClient.PutAsync(proxyUrl, putContent);

            return new ContentResult()
            {
                StatusCode = (int)response.StatusCode,
                Content = await response.Content.ReadAsStringAsync()
            };
        }

        // DELETE: api/Votes/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            HttpResponseMessage response = await this.httpClient.DeleteAsync($"{serviceProxyUrl}/{name}?PartitionKind={partitionKind}&PartitionKey={partitionKey}");

            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int)response.StatusCode);
            }

            return new OkResult();

        }
    }
```
<a id="walkthrough" name="walkthrough_anchor"></a>

## <a name="walk-through-hello-voting-sample-application"></a><span data-ttu-id="70e05-208">Provede hello hlasování ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="70e05-208">Walk through hello voting sample application</span></span>
<span data-ttu-id="70e05-209">Hello hlasování aplikace se skládá ze dvou služeb:</span><span class="sxs-lookup"><span data-stu-id="70e05-209">hello voting application consists of two services:</span></span>
- <span data-ttu-id="70e05-210">Webová služba front-endu (VotingWeb) – ASP.NET Core webových front-endové služby, která obsluhuje hello webové stránky a zpřístupňuje rozhraní API toocommunicate s hello back-end službu.</span><span class="sxs-lookup"><span data-stu-id="70e05-210">Web front-end service (VotingWeb)- An ASP.NET Core web front-end service, which serves hello web page and exposes web APIs toocommunicate with hello backend service.</span></span>
- <span data-ttu-id="70e05-211">Služba back endu (VotingData)-ASP.NET Core webová služba, která zpřístupňuje rozhraní API toostore hello hlas výsledkem slovník spolehlivé trvalé na disku.</span><span class="sxs-lookup"><span data-stu-id="70e05-211">Back-end service (VotingData)- An ASP.NET Core web service, which exposes an API toostore hello vote results in a reliable dictionary persisted on disk.</span></span>

![Diagram aplikace](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

<span data-ttu-id="70e05-213">Když hlasovat v hello aplikace hello následující dojít k události:</span><span class="sxs-lookup"><span data-stu-id="70e05-213">When you vote in hello application hello following events occur:</span></span>
1. <span data-ttu-id="70e05-214">JavaScript odešle hello hlas požadavek toohello webové rozhraní API v hello webové front-endové služby jako požadavek HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="70e05-214">A JavaScript sends hello vote request toohello web API in hello web front-end service as an HTTP PUT request.</span></span>

2. <span data-ttu-id="70e05-215">front-end webové Hello používá proxy toolocate a předávat k službě back-end toohello požadavek HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="70e05-215">hello web front-end service uses a proxy toolocate and forward an HTTP PUT request toohello back-end service.</span></span>

3. <span data-ttu-id="70e05-216">Hello back endové službě trvá hello příchozího požadavku, a ukládá hello aktualizovat výsledek v spolehlivé slovník, který získá replikované toomultiple uzlů v clusteru hello a trvalé na disku.</span><span class="sxs-lookup"><span data-stu-id="70e05-216">hello back-end service takes hello incoming request, and stores hello updated result in a reliable dictionary, which gets replicated toomultiple nodes within hello cluster and persisted on disk.</span></span> <span data-ttu-id="70e05-217">Všechny aplikace hello data se ukládají v clusteru hello, takže se žádná databáze.</span><span class="sxs-lookup"><span data-stu-id="70e05-217">All hello application's data is stored in hello cluster, so no database is needed.</span></span>

## <a name="debug-in-visual-studio"></a><span data-ttu-id="70e05-218">Ladění v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="70e05-218">Debug in Visual Studio</span></span>
<span data-ttu-id="70e05-219">Při ladění aplikace v sadě Visual Studio, kterou používáte místní cluster Service Fabric vývoj.</span><span class="sxs-lookup"><span data-stu-id="70e05-219">When debugging application in Visual Studio, you are using a local Service Fabric development cluster.</span></span> <span data-ttu-id="70e05-220">Máte možnost tooadjust hello váš ladění tooyour scénář prostředí.</span><span class="sxs-lookup"><span data-stu-id="70e05-220">You have hello option tooadjust your debugging experience tooyour scenario.</span></span> <span data-ttu-id="70e05-221">V této aplikaci uložíme data v naší službě back-end pomocí slovník spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="70e05-221">In this application, we store data in our back-end service, using a reliable dictionary.</span></span> <span data-ttu-id="70e05-222">Visual Studio odebere aplikaci hello za výchozí při zastavení ladicího programu hello.</span><span class="sxs-lookup"><span data-stu-id="70e05-222">Visual Studio removes hello application per default when you stop hello debugger.</span></span> <span data-ttu-id="70e05-223">Odebrání aplikace hello způsobí hello data hello back-end služby tooalso odeberou.</span><span class="sxs-lookup"><span data-stu-id="70e05-223">Removing hello application causes hello data in hello back-end service tooalso be removed.</span></span> <span data-ttu-id="70e05-224">toopersist hello data mezi relace ladění, můžete změnit hello **režim ladění aplikací** jako vlastnost na hello **Voting** projektu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="70e05-224">toopersist hello data between debugging sessions, you can change hello **Application Debug Mode** as a property on hello **Voting** project in Visual Studio.</span></span>

<span data-ttu-id="70e05-225">toolook na co se stane, že v kódu hello dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="70e05-225">toolook at what happens in hello code, complete hello following steps:</span></span>
1. <span data-ttu-id="70e05-226">Otevřete hello **VotesController.cs** souboru a nastavit zarážky v hello webové rozhraní API **Put** – metoda (řádku 47) – můžete vyhledat hello soubor v Průzkumníku řešení v sadě Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="70e05-226">Open hello **VotesController.cs** file and set a breakpoint in hello web API's **Put** method (line 47) - You can search for hello file in hello Solution Explorer in Visual Studio.</span></span>

2. <span data-ttu-id="70e05-227">Otevřete hello **VoteDataController.cs** souboru a nastavit zarážky v tomto rozhraní web API **Put** – metoda (řádku 50).</span><span class="sxs-lookup"><span data-stu-id="70e05-227">Open hello **VoteDataController.cs** file and set a breakpoint in this web API's **Put** method (line 50).</span></span>

3. <span data-ttu-id="70e05-228">Vrátit toohello prohlížeče a klikněte na hlasování možnost nebo přidat novou možnost hlasování.</span><span class="sxs-lookup"><span data-stu-id="70e05-228">Go back toohello browser and click a voting option or add a new voting option.</span></span> <span data-ttu-id="70e05-229">Kliknutí na první zarážky hello do kontroleru hello webové front endové rozhraní api.</span><span class="sxs-lookup"><span data-stu-id="70e05-229">You hit hello first breakpoint in hello web front-end's api controller.</span></span>
    
    1. <span data-ttu-id="70e05-230">Toto je, kde hello JavaScript v prohlížeči hello odešle řadič požadavek toohello webové rozhraní API v hello front-endové služby.</span><span class="sxs-lookup"><span data-stu-id="70e05-230">This is where hello JavaScript in hello browser sends a request toohello web API controller in hello front-end service.</span></span>
    
    ![Přidejte Front-End služby hlas](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. <span data-ttu-id="70e05-232">Nejprve jsme vytvořit adresu URL toohello hello ReverseProxy pro naši službu back-end **(1)**.</span><span class="sxs-lookup"><span data-stu-id="70e05-232">First we construct hello URL toohello ReverseProxy for our back-end service **(1)**.</span></span>
    3. <span data-ttu-id="70e05-233">Potom pošleme hello požadavek HTTP PUT toohello ReverseProxy **(2)**.</span><span class="sxs-lookup"><span data-stu-id="70e05-233">Then we send hello HTTP PUT Request toohello ReverseProxy **(2)**.</span></span>
    4. <span data-ttu-id="70e05-234">Nakonec hello vrátíme hello odpověď z klienta toohello back-end služby hello **(3)**.</span><span class="sxs-lookup"><span data-stu-id="70e05-234">Finally hello we return hello response from hello back-end service toohello client **(3)**.</span></span>

4. <span data-ttu-id="70e05-235">Stiskněte klávesu **F5** toocontinue</span><span class="sxs-lookup"><span data-stu-id="70e05-235">Press **F5** toocontinue</span></span>
    1. <span data-ttu-id="70e05-236">Nyní jste na hello zarážce v hello back endové službě.</span><span class="sxs-lookup"><span data-stu-id="70e05-236">You are now at hello break point in hello back-end service.</span></span>
    
    ![Přidat hlas Back-End službu](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. <span data-ttu-id="70e05-238">V prvním řádku hello hello metoda **(1)** používáme hello `StateManager` tooget nebo přidejte spolehlivé slovník nazývaný `counts`.</span><span class="sxs-lookup"><span data-stu-id="70e05-238">In hello first line in hello method **(1)** we are using hello `StateManager` tooget or add a reliable dictionary called `counts`.</span></span>
    3. <span data-ttu-id="70e05-239">Všechny interakce s hodnotami ve slovníku spolehlivé vyžadují transakci, této konfigurace pomocí příkazu **(2)** vytvoří této transakce.</span><span class="sxs-lookup"><span data-stu-id="70e05-239">All interactions with values in a reliable dictionary require a transaction, this using statement **(2)** creates that transaction.</span></span>
    4. <span data-ttu-id="70e05-240">V transakci hello jsme potom aktualizujte hodnotu hello hello relevantní klíče pro hello volíte možnost a potvrzení hello operaci **(3)**.</span><span class="sxs-lookup"><span data-stu-id="70e05-240">In hello transaction, we then update hello value of hello relevant key for hello voting option and commits hello operation **(3)**.</span></span> <span data-ttu-id="70e05-241">Po potvrzení hello metoda vrátí, hello dat ve slovníku hello k aktualizaci a replikované tooother uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="70e05-241">Once hello commit method returns, hello data is updated in hello dictionary and replicated tooother nodes in hello cluster.</span></span> <span data-ttu-id="70e05-242">Hello data jsou nyní bezpečně uložena v clusteru hello a hello back-end služby při selhání tooother uzly, i nadále s hello data k dispozici.</span><span class="sxs-lookup"><span data-stu-id="70e05-242">hello data is now safely stored in hello cluster, and hello back-end service can fail over tooother nodes, still having hello data available.</span></span>
5. <span data-ttu-id="70e05-243">Stiskněte klávesu **F5** toocontinue</span><span class="sxs-lookup"><span data-stu-id="70e05-243">Press **F5** toocontinue</span></span>

<span data-ttu-id="70e05-244">hello toostop ladicí relace, stiskněte klávesu **Shift + F5**.</span><span class="sxs-lookup"><span data-stu-id="70e05-244">toostop hello debugging session, press **Shift+F5**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="70e05-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70e05-245">Next steps</span></span>
<span data-ttu-id="70e05-246">V této části kurzu hello jste zjistili, jak:</span><span class="sxs-lookup"><span data-stu-id="70e05-246">In this part of hello tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="70e05-247">Vytvoření služby webového rozhraní API ASP.NET Core jako stavové spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="70e05-247">Create an ASP.NET Core Web API service as a stateful reliable service</span></span>
> * <span data-ttu-id="70e05-248">Vytvoření služby webové aplikace ASP.NET Core jako bezstavové webové služby</span><span class="sxs-lookup"><span data-stu-id="70e05-248">Create an ASP.NET Core Web Application service as a stateless web service</span></span>
> * <span data-ttu-id="70e05-249">Pomocí toocommunicate reverzní proxy server hello hello stavové služby</span><span class="sxs-lookup"><span data-stu-id="70e05-249">Use hello reverse proxy toocommunicate with hello stateful service</span></span>

<span data-ttu-id="70e05-250">ADVANCE toohello další kurz:</span><span class="sxs-lookup"><span data-stu-id="70e05-250">Advance toohello next tutorial:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="70e05-251">Nasazení aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="70e05-251">Deploy hello application tooAzure</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)
