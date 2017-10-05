---
title: "Vytvořit aplikaci .NET pro Service Fabric | Microsoft Docs"
description: "Zjistěte, jak vytvořit aplikaci s front-endové ASP.NET Core a spolehlivá služba stavová back-end a nasazení aplikací do clusteru."
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
ms.openlocfilehash: ef50adf3af19bce494c3256308b443c8eaccdcea
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a><span data-ttu-id="9b8e8-103">Vytvoření a nasazení aplikace pomocí služby front-endové webové rozhraní API ASP.NET Core a stavové služby back-end</span><span class="sxs-lookup"><span data-stu-id="9b8e8-103">Create and deploy an application with an ASP.NET Core Web API front-end service and a stateful back-end service</span></span>
<span data-ttu-id="9b8e8-104">V tomto kurzu je součástí, jednu z řady.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-104">This tutorial is part one of a series.</span></span>  <span data-ttu-id="9b8e8-105">Se dozvíte, jak vytvořit aplikaci Azure Service Fabric pomocí webového rozhraní API ASP.NET Core front-end a stavové služby back-end ukládat data.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-105">You will learn how to create an Azure Service Fabric application with an ASP.NET Core Web API front end and a stateful back-end service to store your data.</span></span> <span data-ttu-id="9b8e8-106">Jakmile budete hotovi, máte hlasovací aplikaci s ASP.NET Core web, který je front-end, který uloží výsledků hlasování ve stavové služby back-end v clusteru.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-106">When you're finished, you have a voting application with an ASP.NET Core web front-end that saves voting results in a stateful back-end service in the cluster.</span></span> <span data-ttu-id="9b8e8-107">Pokud nechcete, aby ručně vytvořit hlasovací aplikaci, můžete [stáhnout zdrojový kód](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) pro hotová aplikace a přeskočit na [provede hlasujících ukázkovou aplikaci](#walkthrough_anchor).</span><span class="sxs-lookup"><span data-stu-id="9b8e8-107">If you don't want to manually create the voting application, you can [download the source code](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) for the completed application and skip ahead to [Walk through the voting sample application](#walkthrough_anchor).</span></span>

![Diagram aplikace](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

<span data-ttu-id="9b8e8-109">V rámci jedna řada, zjistíte, jak:</span><span class="sxs-lookup"><span data-stu-id="9b8e8-109">In part one of the series, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9b8e8-110">Vytvoření služby webového rozhraní API ASP.NET Core jako stavové spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="9b8e8-110">Create an ASP.NET Core Web API service as a stateful reliable service</span></span>
> * <span data-ttu-id="9b8e8-111">Vytvoření služby webové aplikace ASP.NET Core jako bezstavové webové služby</span><span class="sxs-lookup"><span data-stu-id="9b8e8-111">Create an ASP.NET Core Web Application service as a stateless web service</span></span>
> * <span data-ttu-id="9b8e8-112">Použít reverzní proxy server ke komunikaci s stavové služby</span><span class="sxs-lookup"><span data-stu-id="9b8e8-112">Use the reverse proxy to communicate with the stateful service</span></span>

<span data-ttu-id="9b8e8-113">V této série kurzu zjistíte, jak:</span><span class="sxs-lookup"><span data-stu-id="9b8e8-113">In this tutorial series you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="9b8e8-114">Sestavení aplikace .NET Service Fabric</span><span class="sxs-lookup"><span data-stu-id="9b8e8-114">Build a .NET Service Fabric application</span></span>
> * [<span data-ttu-id="9b8e8-115">Nasazení aplikace na vzdálený cluster</span><span class="sxs-lookup"><span data-stu-id="9b8e8-115">Deploy the application to a remote cluster</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [<span data-ttu-id="9b8e8-116">Konfigurace CI/CD pomocí Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="9b8e8-116">Configure CI/CD using Visual Studio Team Services</span></span>](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a><span data-ttu-id="9b8e8-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9b8e8-117">Prerequisites</span></span>
<span data-ttu-id="9b8e8-118">Před zahájením tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="9b8e8-118">Before you begin this tutorial:</span></span>
- <span data-ttu-id="9b8e8-119">Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="9b8e8-119">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="9b8e8-120">[Nainstalovat Visual Studio 2017](https://www.visualstudio.com/) a nainstalujte **Azure development** a **ASP.NET a webové vývoj** úlohy.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-120">[Install Visual Studio 2017](https://www.visualstudio.com/) and install the **Azure development** and **ASP.NET and web development** workloads.</span></span>
- [<span data-ttu-id="9b8e8-121">Instalace Service Fabric SDK</span><span class="sxs-lookup"><span data-stu-id="9b8e8-121">Install the Service Fabric SDK</span></span>](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a><span data-ttu-id="9b8e8-122">Vytvoření služby ASP.NET Web API jako spolehlivě.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-122">Create an ASP.NET Web API service as a reliable service</span></span>
<span data-ttu-id="9b8e8-123">Nejprve vytvořte webu front-end hlasovací aplikace pomocí ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-123">First, create the web front-end of the voting application using ASP.NET Core.</span></span> <span data-ttu-id="9b8e8-124">ASP.NET Core je odlehčený, napříč platformami webové rozhraní vývoj, které vám pomůže vytvořit moderní webové uživatelské rozhraní a webovým rozhraním API.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-124">ASP.NET Core is a lightweight, cross-platform web development framework that you can use to create modern web UI and web APIs.</span></span> <span data-ttu-id="9b8e8-125">Pokud chcete získat úplný znalosti o tom, jak ASP.NET Core integruje se službou Service Fabric, důrazně doporučujeme čtení [ASP.NET Core v Service Fabric spolehlivé služby](service-fabric-reliable-services-communication-aspnetcore.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-125">To get a complete understanding of how ASP.NET Core integrates with Service Fabric, we strongly recommend reading through the [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) article.</span></span> <span data-ttu-id="9b8e8-126">Teď může v tomto kurzu vám rychle začít.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-126">For now, you can follow this tutorial to get started quickly.</span></span> <span data-ttu-id="9b8e8-127">Další informace o ASP.NET Core, najdete v článku [ASP.NET základní dokumentace](https://docs.microsoft.com/aspnet/core/).</span><span class="sxs-lookup"><span data-stu-id="9b8e8-127">To learn more about ASP.NET Core, see the [ASP.NET Core Documentation](https://docs.microsoft.com/aspnet/core/).</span></span>

> [!NOTE]
> <span data-ttu-id="9b8e8-128">V tomto kurzu vychází z [ASP.NET Core nástrojů pro Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="9b8e8-128">This tutorial is based on the [ASP.NET Core tools for Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc).</span></span> <span data-ttu-id="9b8e8-129">.NET Core nástrojů pro Visual Studio 2015 se již nebude aktualizováno.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-129">The .NET Core tools for Visual Studio 2015 are no longer being updated.</span></span>

1. <span data-ttu-id="9b8e8-130">Spusťte sadu Visual Studio jako **správce**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-130">Launch Visual Studio as an **administrator**.</span></span>

2. <span data-ttu-id="9b8e8-131">Vytvoření projektu s **soubor**->**nový**->**projektu**</span><span class="sxs-lookup"><span data-stu-id="9b8e8-131">Create a project with **File**->**New**->**Project**</span></span>

3. <span data-ttu-id="9b8e8-132">V dialogovém okně **Nový projekt** zvolte **Cloud > Aplikace Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-132">In the **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

4. <span data-ttu-id="9b8e8-133">Název aplikace **Voting** a stiskněte klávesu **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-133">Name the application **Voting** and press **OK**.</span></span>

   ![Dialogové okno Nový projekt ve Visual Studiu](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. <span data-ttu-id="9b8e8-135">Na **nové služby Fabric** vyberte **bezstavové ASP.NET Core**a název vaší služby **VotingWeb**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-135">On the **New Service Fabric Service** page, choose **Stateless ASP.NET Core**, and name your service **VotingWeb**.</span></span>
   
   ![Výběr v dialogovém okně Nový služby webovou službu ASP.NET.](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. <span data-ttu-id="9b8e8-137">Další stránka obsahuje sadu ASP.NET Core šablony projektů.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-137">The next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="9b8e8-138">V tomto kurzu zvolte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-138">For this tutorial, choose **Web Application**.</span></span> 
   
   ![Zvolte typ projektu ASP.NET](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   <span data-ttu-id="9b8e8-140">Visual Studio vytvoří aplikaci a projekt služby a zobrazí je v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-140">Visual Studio creates an application and a service project and displays them in Solution Explorer.</span></span>

   ![Průzkumník řešení po vytvoření aplikace ASP.NET základní webového rozhraní API služby]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-to-the-votingweb-service"></a><span data-ttu-id="9b8e8-142">Přidat ke službě VotingWeb AngularJS</span><span class="sxs-lookup"><span data-stu-id="9b8e8-142">Add AngularJS to the VotingWeb service</span></span>
<span data-ttu-id="9b8e8-143">Přidat [AngularJS](http://angularjs.org/) pro vaši službu pomocí integrovaných [Bower podporu](/aspnet/core/client-side/bower).</span><span class="sxs-lookup"><span data-stu-id="9b8e8-143">Add [AngularJS](http://angularjs.org/) to your service using the built-in [Bower support](/aspnet/core/client-side/bower).</span></span> <span data-ttu-id="9b8e8-144">Otevřete *bower.json* a přidejte položky pro úhlová a úhlová bootstrap a pak změny uložte.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-144">Open *bower.json* and add entries for angular and angular-bootstrap, then save your changes.</span></span>

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
<span data-ttu-id="9b8e8-145">Při ukládání *bower.json* soubor, úhlová je nainstalován ve vašem projektu *wwwroot/lib* složky.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-145">Upon saving the *bower.json* file, Angular is installed in your project's *wwwroot/lib* folder.</span></span> <span data-ttu-id="9b8e8-146">Kromě toho je uveden v rámci *závislosti/Bower* složky.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-146">Additionally, it is listed within the *Dependencies/Bower* folder.</span></span>

### <a name="update-the-sitejs-file"></a><span data-ttu-id="9b8e8-147">Aktualizovat soubor site.js</span><span class="sxs-lookup"><span data-stu-id="9b8e8-147">Update the site.js file</span></span>
<span data-ttu-id="9b8e8-148">Otevřete *wwwroot/js/site.js* souboru.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-148">Open the *wwwroot/js/site.js* file.</span></span>  <span data-ttu-id="9b8e8-149">Nahraďte jeho obsah JavaScript používané domovské zobrazení:</span><span class="sxs-lookup"><span data-stu-id="9b8e8-149">Replace its contents with the JavaScript used by the Home views:</span></span>

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

### <a name="update-the-indexcshtml-file"></a><span data-ttu-id="9b8e8-150">Aktualizovat soubor Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="9b8e8-150">Update the Index.cshtml file</span></span>
<span data-ttu-id="9b8e8-151">Otevřete *Views/Home/Index.cshtml* souboru konkrétní domovskou řadiče zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-151">Open the *Views/Home/Index.cshtml* file, the view specific to the Home controller.</span></span>  <span data-ttu-id="9b8e8-152">Nahraďte jeho obsah následujícím, pak uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-152">Replace its contents with the following, then save your changes.</span></span>

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
                        Click to vote
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

### <a name="update-the-layoutcshtml-file"></a><span data-ttu-id="9b8e8-153">Aktualizovat soubor _Layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="9b8e8-153">Update the _Layout.cshtml file</span></span>
<span data-ttu-id="9b8e8-154">Otevřete *Views/Shared/_Layout.cshtml* souboru, výchozí rozložení pro aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-154">Open the *Views/Shared/_Layout.cshtml* file, the default layout for the ASP.NET app.</span></span>  <span data-ttu-id="9b8e8-155">Nahraďte jeho obsah následujícím, pak uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-155">Replace its contents with the following, then save your changes.</span></span>

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

### <a name="update-the-votingwebcs-file"></a><span data-ttu-id="9b8e8-156">Aktualizovat soubor VotingWeb.cs</span><span class="sxs-lookup"><span data-stu-id="9b8e8-156">Update the VotingWeb.cs file</span></span>
<span data-ttu-id="9b8e8-157">Otevřete *VotingWeb.cs* souboru, který vytvoří webového hostitele jádro ASP.NET uvnitř bezstavové služby pomocí WebListener webový server.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-157">Open the *VotingWeb.cs* file, which creates the ASP.NET Core WebHost inside the stateless service using the WebListener web server.</span></span>  <span data-ttu-id="9b8e8-158">Přidat `using System.Net.Http;` direktivy do horní části souboru.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-158">Add the `using System.Net.Http;` directive to the top of the file.</span></span>  <span data-ttu-id="9b8e8-159">Nahraďte `CreateServiceInstanceListeners()` fungovat s následující a potom uložte změny.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-159">Replace the `CreateServiceInstanceListeners()` function with the following, then save your changes.</span></span>

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

### <a name="add-the-votescontrollercs-file"></a><span data-ttu-id="9b8e8-160">Přidejte soubor VotesController.cs</span><span class="sxs-lookup"><span data-stu-id="9b8e8-160">Add the VotesController.cs file</span></span>
<span data-ttu-id="9b8e8-161">Přidáte řadič, který definuje hlasujících akce.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-161">Add a controller which defines voting actions.</span></span> <span data-ttu-id="9b8e8-162">Klikněte pravým tlačítkem na **řadiče** složku, pak vyberte **Přidat -> Nový položky -> třída**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-162">Right-click on the **Controllers** folder, then select **Add->New item->Class**.</span></span>  <span data-ttu-id="9b8e8-163">Název souboru "VotesController.cs" a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-163">Name the file "VotesController.cs" and click **Add**.</span></span>  <span data-ttu-id="9b8e8-164">Nahraďte obsah souboru následující příkaz, potom uložte změny.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-164">Replace the file contents with the following, then save your changes.</span></span>  <span data-ttu-id="9b8e8-165">Dále v [aktualizovat soubor VotesController.cs](#updatevotecontroller_anchor), tento soubor se změní pro čtení a zápis hlasujících data z back endové službě.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-165">Later, in [Update the VotesController.cs file](#updatevotecontroller_anchor), this file will be modified to read and write voting data from the back-end service.</span></span>  <span data-ttu-id="9b8e8-166">Prozatím se vrátí řadičem statický řetězec data k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-166">For now, the controller returns static string data to the view.</span></span>

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



### <a name="deploy-and-run-the-application-locally"></a><span data-ttu-id="9b8e8-167">Nasazení a spuštění aplikace místně</span><span class="sxs-lookup"><span data-stu-id="9b8e8-167">Deploy and run the application locally</span></span>
<span data-ttu-id="9b8e8-168">Teď můžete pokračovat a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-168">You can now go ahead and run the application.</span></span> <span data-ttu-id="9b8e8-169">Stisknutím klávesy `F5` v sadě Visual Studio aplikaci nasaďte pro účely ladění.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-169">In Visual Studio, press `F5` to deploy the application for debugging.</span></span> <span data-ttu-id="9b8e8-170">`F5`selže, pokud nebyla dříve otevřete Visual Studio jako **správce**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-170">`F5` fails if you didn't previously open Visual Studio as **administrator**.</span></span>

> [!NOTE]
> <span data-ttu-id="9b8e8-171">Při prvním místním spuštění a nasazení aplikace sada Visual Studio vytvoří místní cluster pro účely ladění.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-171">The first time you run and deploy the application locally, Visual Studio creates a local cluster for debugging.</span></span>  <span data-ttu-id="9b8e8-172">Vytvoření clusteru může trvat delší dobu.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-172">Cluster creation may take some time.</span></span> <span data-ttu-id="9b8e8-173">Stav vytváření clusteru se zobrazí v okně výstupu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-173">The cluster creation status is displayed in the Visual Studio output window.</span></span>

<span data-ttu-id="9b8e8-174">Webové aplikace v tomto okamžiku by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="9b8e8-174">At this point, your web app should look like this:</span></span>

![ASP.NET Core front-endu](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

<span data-ttu-id="9b8e8-176">Aby se ukončilo ladění aplikace, přejděte zpět do sady Visual Studio a stiskněte klávesu **Shift + F5**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-176">To stop debugging the application, go back to Visual Studio and press **Shift+F5**.</span></span>

## <a name="add-a-stateful-back-end-service-to-your-application"></a><span data-ttu-id="9b8e8-177">Přidání stavová služba back endu do aplikace</span><span class="sxs-lookup"><span data-stu-id="9b8e8-177">Add a stateful back-end service to your application</span></span>
<span data-ttu-id="9b8e8-178">Teď, když máme rozhraní ASP.NET Web API služby spuštěné v naší aplikaci, přejdeme dopředu a přidejte stavové spolehlivé služby k uložení některá data v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-178">Now that we have an ASP.NET Web API service running in our application, let's go ahead and add a stateful reliable service to store some data in our application.</span></span>

<span data-ttu-id="9b8e8-179">Service Fabric můžete konzistentně a spolehlivě ukládat vaše právo data uvnitř vaší služby pomocí spolehlivé kolekce.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-179">Service Fabric allows you to consistently and reliably store your data right inside your service by using reliable collections.</span></span> <span data-ttu-id="9b8e8-180">Spolehlivé kolekce jsou sadu třídy vysoce dostupné a spolehlivého kolekce, které jsou pro každý, kdo má používá kolekce C#.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-180">Reliable collections are a set of highly available and reliable collection classes that are familiar to anyone who has used C# collections.</span></span>

<span data-ttu-id="9b8e8-181">V tomto kurzu vytvoříte službu, která ukládá hodnotu čítače v kolekci spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-181">In this tutorial, you create a service which stores a counter value in a reliable collection.</span></span>

1. <span data-ttu-id="9b8e8-182">V Průzkumníku řešení klikněte pravým tlačítkem na **služby** v aplikaci projektu a zvolte **Přidat > Nový Service Fabric Service**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-182">In Solution Explorer, right-click **Services** within the application project and choose **Add > New Service Fabric Service**.</span></span>
   
    ![Přidání nové služby do existující aplikace](./media/service-fabric-tutorial-create-dotnet-app/vs-add-new-service.png)

2. <span data-ttu-id="9b8e8-184">V **nové služby Fabric** dialogovém okně, vyberte **Stateful ASP.NET Core**a název služby **VotingData** a stiskněte klávesu **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-184">In the **New Service Fabric Service** dialog, choose **Stateful ASP.NET Core**, and name the service **VotingData** and press **OK**.</span></span>

    ![Dialogové okno Nová služba ve Visual Studiu](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    <span data-ttu-id="9b8e8-186">Po vytvoření svůj projekt služby, máte dvě služby ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-186">Once your service project is created, you have two services in your application.</span></span> <span data-ttu-id="9b8e8-187">Při dalším sestavit aplikaci, můžete přidat další služby stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-187">As you continue to build your application, you can add more services in the same way.</span></span> <span data-ttu-id="9b8e8-188">Každý může být nezávisle verzí a upgradovaný.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-188">Each can be independently versioned and upgraded.</span></span>

3. <span data-ttu-id="9b8e8-189">Další stránka obsahuje sadu ASP.NET Core šablony projektů.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-189">The next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="9b8e8-190">V tomto kurzu zvolte **webového rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-190">For this tutorial, choose **Web API**.</span></span>

    ![Zvolte typ projektu ASP.NET](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    <span data-ttu-id="9b8e8-192">Visual Studio vytvoří projekt služby a zobrazí je v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-192">Visual Studio creates a service project and displays them in Solution Explorer.</span></span>

    ![Průzkumník řešení](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-the-votedatacontrollercs-file"></a><span data-ttu-id="9b8e8-194">Přidejte soubor VoteDataController.cs</span><span class="sxs-lookup"><span data-stu-id="9b8e8-194">Add the VoteDataController.cs file</span></span>

<span data-ttu-id="9b8e8-195">V **VotingData** projektu klikněte pravým tlačítkem na **řadiče** složku, pak vyberte **Přidat -> Nový položky -> – třída**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-195">In the **VotingData** project right-click on the **Controllers** folder, then select **Add->New item->Class**.</span></span> <span data-ttu-id="9b8e8-196">Název souboru "VoteDataController.cs" a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-196">Name the file "VoteDataController.cs" and click **Add**.</span></span> <span data-ttu-id="9b8e8-197">Nahraďte obsah souboru následující příkaz, potom uložte změny.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-197">Replace the file contents with the following, then save your changes.</span></span>

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


## <a name="connect-the-services"></a><span data-ttu-id="9b8e8-198">Připojení služby</span><span class="sxs-lookup"><span data-stu-id="9b8e8-198">Connect the services</span></span>
<span data-ttu-id="9b8e8-199">V tomto kroku další jsme připojení dvě služby a zkontrolujte front-end webové aplikace, získání a nastavení, hlasování informace z back endové službě.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-199">In this next step, we will connect the two services and make the front-end Web application get and set voting information from the back-end service.</span></span>

<span data-ttu-id="9b8e8-200">Service Fabric nabízí flexibilitu v tom, jak komunikovat se službami reliable services.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-200">Service Fabric provides complete flexibility in how you communicate with reliable services.</span></span> <span data-ttu-id="9b8e8-201">V rámci jedné aplikace můžete mít služby, které jsou přístupné přes TCP.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-201">Within a single application, you might have services that are accessible via TCP.</span></span> <span data-ttu-id="9b8e8-202">Dalším službám, které může být přístupné přes rozhraní HTTP REST API a stále dalších služeb může být přístupné přes webové sokety.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-202">Other services that might be accessible via an HTTP REST API and still other services could be accessible via web sockets.</span></span> <span data-ttu-id="9b8e8-203">Pro informace o dostupných možnostech a kompromisy související se situací, viz [komunikaci se službou](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="9b8e8-203">For background on the options available and the tradeoffs involved, see [Communicating with services](service-fabric-connect-and-communicate-with-services.md).</span></span>

<span data-ttu-id="9b8e8-204">V tomto kurzu používáme [webového rozhraní API ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md).</span><span class="sxs-lookup"><span data-stu-id="9b8e8-204">In this tutorial, we are using [ASP.NET Core Web API](service-fabric-reliable-services-communication-aspnetcore.md).</span></span>

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-the-votescontrollercs-file"></a><span data-ttu-id="9b8e8-205">Aktualizovat soubor VotesController.cs</span><span class="sxs-lookup"><span data-stu-id="9b8e8-205">Update the VotesController.cs file</span></span>
<span data-ttu-id="9b8e8-206">V **VotingWeb** projekt, otevřete *Controllers/VotesController.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-206">In the **VotingWeb** project, open the *Controllers/VotesController.cs* file.</span></span>  <span data-ttu-id="9b8e8-207">Nahraďte `VotesController` třídy definice obsah následujícím kódem a pak změny uložte.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-207">Replace the `VotesController` class definition contents with the following, then save your changes.</span></span>

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

## <a name="walk-through-the-voting-sample-application"></a><span data-ttu-id="9b8e8-208">Provede hlasujících ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="9b8e8-208">Walk through the voting sample application</span></span>
<span data-ttu-id="9b8e8-209">Hlasovací aplikaci se skládá ze dvou služeb:</span><span class="sxs-lookup"><span data-stu-id="9b8e8-209">The voting application consists of two services:</span></span>
- <span data-ttu-id="9b8e8-210">Webová služba front-endu (VotingWeb) – ASP.NET Core webových front-endové služby, která obsluhuje webové stránky a zpřístupňuje rozhraní API pro komunikaci s back-end službu.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-210">Web front-end service (VotingWeb)- An ASP.NET Core web front-end service, which serves the web page and exposes web APIs to communicate with the backend service.</span></span>
- <span data-ttu-id="9b8e8-211">Služba back endu (VotingData)-ASP.NET Core webová služba, která zpřístupňuje rozhraní API můžete ukládat výsledky hlas ve slovníku spolehlivé trvalé na disku.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-211">Back-end service (VotingData)- An ASP.NET Core web service, which exposes an API to store the vote results in a reliable dictionary persisted on disk.</span></span>

![Diagram aplikace](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

<span data-ttu-id="9b8e8-213">Když hlasovat v aplikaci dojít k následujícím událostem:</span><span class="sxs-lookup"><span data-stu-id="9b8e8-213">When you vote in the application the following events occur:</span></span>
1. <span data-ttu-id="9b8e8-214">JavaScript odešle žádost hlas webovému rozhraní API ve front-endové webové službě jako požadavek HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-214">A JavaScript sends the vote request to the web API in the web front-end service as an HTTP PUT request.</span></span>

2. <span data-ttu-id="9b8e8-215">Webovou službu front-endu používá proxy server pro vyhledání a předávat požadavek HTTP PUT ve službě back-end.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-215">The web front-end service uses a proxy to locate and forward an HTTP PUT request to the back-end service.</span></span>

3. <span data-ttu-id="9b8e8-216">Back endové službě přijímá příchozí požadavky a ukládá aktualizované výsledek v spolehlivé slovník, který získá replikují do několika uzly v clusteru a trvalé na disku.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-216">The back-end service takes the incoming request, and stores the updated result in a reliable dictionary, which gets replicated to multiple nodes within the cluster and persisted on disk.</span></span> <span data-ttu-id="9b8e8-217">Všechny aplikační data se ukládají v clusteru, takže se žádná databáze.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-217">All the application's data is stored in the cluster, so no database is needed.</span></span>

## <a name="debug-in-visual-studio"></a><span data-ttu-id="9b8e8-218">Ladění v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b8e8-218">Debug in Visual Studio</span></span>
<span data-ttu-id="9b8e8-219">Při ladění aplikace v sadě Visual Studio, kterou používáte místní cluster Service Fabric vývoj.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-219">When debugging application in Visual Studio, you are using a local Service Fabric development cluster.</span></span> <span data-ttu-id="9b8e8-220">Máte možnost upravit prostředí ladění pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-220">You have the option to adjust your debugging experience to your scenario.</span></span> <span data-ttu-id="9b8e8-221">V této aplikaci uložíme data v naší službě back-end pomocí slovník spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-221">In this application, we store data in our back-end service, using a reliable dictionary.</span></span> <span data-ttu-id="9b8e8-222">Visual Studio odebere aplikaci za výchozí při zastavení ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-222">Visual Studio removes the application per default when you stop the debugger.</span></span> <span data-ttu-id="9b8e8-223">Odebrání aplikace způsobí, že data ve službě back-end taky odeberou.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-223">Removing the application causes the data in the back-end service to also be removed.</span></span> <span data-ttu-id="9b8e8-224">Chcete-li zachovat data mezi relace ladění, můžete změnit **režim ladění aplikací** jako vlastnost na **Voting** projektu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-224">To persist the data between debugging sessions, you can change the **Application Debug Mode** as a property on the **Voting** project in Visual Studio.</span></span>

<span data-ttu-id="9b8e8-225">Podívat se na co se stane, že v kódu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9b8e8-225">To look at what happens in the code, complete the following steps:</span></span>
1. <span data-ttu-id="9b8e8-226">Otevřete **VotesController.cs** souboru a nastavit zarážky ve webové rozhraní API **Put** – metoda (řádku 47) – můžete vyhledat soubor v Průzkumníku řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-226">Open the **VotesController.cs** file and set a breakpoint in the web API's **Put** method (line 47) - You can search for the file in the Solution Explorer in Visual Studio.</span></span>

2. <span data-ttu-id="9b8e8-227">Otevřete **VoteDataController.cs** souboru a nastavit zarážky v tomto rozhraní web API **Put** – metoda (řádku 50).</span><span class="sxs-lookup"><span data-stu-id="9b8e8-227">Open the **VoteDataController.cs** file and set a breakpoint in this web API's **Put** method (line 50).</span></span>

3. <span data-ttu-id="9b8e8-228">Přejděte zpět do prohlížeče a klikněte na hlasování možnost nebo přidat novou možnost hlasování.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-228">Go back to the browser and click a voting option or add a new voting option.</span></span> <span data-ttu-id="9b8e8-229">Kliknutí na první zarážky do kontroleru webového přední konci na rozhraní api.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-229">You hit the first breakpoint in the web front-end's api controller.</span></span>
    
    1. <span data-ttu-id="9b8e8-230">Toto je, kde JavaScript v prohlížeči odešle požadavek na kontroler API web ve službě front-endu.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-230">This is where the JavaScript in the browser sends a request to the web API controller in the front-end service.</span></span>
    
    ![Přidejte Front-End služby hlas](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. <span data-ttu-id="9b8e8-232">Nejprve jsme vytvořit adresu URL ReverseProxy pro naši službu back-end **(1)**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-232">First we construct the URL to the ReverseProxy for our back-end service **(1)**.</span></span>
    3. <span data-ttu-id="9b8e8-233">Potom jsme poslat PUT požadavek HTTP ReverseProxy **(2)**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-233">Then we send the HTTP PUT Request to the ReverseProxy **(2)**.</span></span>
    4. <span data-ttu-id="9b8e8-234">Nakonec vrátíme odpověď z back-end službu do klienta **(3)**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-234">Finally the we return the response from the back-end service to the client **(3)**.</span></span>

4. <span data-ttu-id="9b8e8-235">Stiskněte klávesu **F5** pokračovat</span><span class="sxs-lookup"><span data-stu-id="9b8e8-235">Press **F5** to continue</span></span>
    1. <span data-ttu-id="9b8e8-236">Nyní jste na zarážce ve službě back-end.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-236">You are now at the break point in the back-end service.</span></span>
    
    ![Přidat hlas Back-End službu](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. <span data-ttu-id="9b8e8-238">Na prvním řádku v metodě **(1)** používáme `StateManager` nebo přidat spolehlivé slovník nazývaný `counts`.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-238">In the first line in the method **(1)** we are using the `StateManager` to get or add a reliable dictionary called `counts`.</span></span>
    3. <span data-ttu-id="9b8e8-239">Všechny interakce s hodnotami ve slovníku spolehlivé vyžadují transakci, této konfigurace pomocí příkazu **(2)** vytvoří této transakce.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-239">All interactions with values in a reliable dictionary require a transaction, this using statement **(2)** creates that transaction.</span></span>
    4. <span data-ttu-id="9b8e8-240">V transakci, jsme potom aktualizujte hodnotu relevantní klíče pro možnost hlasování a provede operaci **(3)**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-240">In the transaction, we then update the value of the relevant key for the voting option and commits the operation **(3)**.</span></span> <span data-ttu-id="9b8e8-241">Po potvrzení metoda vrátí, data aktualizovat ve slovníku a replikovat do jiných uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-241">Once the commit method returns, the data is updated in the dictionary and replicated to other nodes in the cluster.</span></span> <span data-ttu-id="9b8e8-242">Data jsou teď bezpečně uložená v clusteru, a může převzít back-end službu do dalších uzlů, i nadále s dostupná data.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-242">The data is now safely stored in the cluster, and the back-end service can fail over to other nodes, still having the data available.</span></span>
5. <span data-ttu-id="9b8e8-243">Stiskněte klávesu **F5** pokračovat</span><span class="sxs-lookup"><span data-stu-id="9b8e8-243">Press **F5** to continue</span></span>

<span data-ttu-id="9b8e8-244">Chcete-li ukončit relaci ladění, stiskněte **Shift + F5**.</span><span class="sxs-lookup"><span data-stu-id="9b8e8-244">To stop the debugging session, press **Shift+F5**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="9b8e8-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9b8e8-245">Next steps</span></span>
<span data-ttu-id="9b8e8-246">V této části kurzu jste se dozvěděli, jak:</span><span class="sxs-lookup"><span data-stu-id="9b8e8-246">In this part of the tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9b8e8-247">Vytvoření služby webového rozhraní API ASP.NET Core jako stavové spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="9b8e8-247">Create an ASP.NET Core Web API service as a stateful reliable service</span></span>
> * <span data-ttu-id="9b8e8-248">Vytvoření služby webové aplikace ASP.NET Core jako bezstavové webové služby</span><span class="sxs-lookup"><span data-stu-id="9b8e8-248">Create an ASP.NET Core Web Application service as a stateless web service</span></span>
> * <span data-ttu-id="9b8e8-249">Použít reverzní proxy server ke komunikaci s stavové služby</span><span class="sxs-lookup"><span data-stu-id="9b8e8-249">Use the reverse proxy to communicate with the stateful service</span></span>

<span data-ttu-id="9b8e8-250">Přechodu na další kurz:</span><span class="sxs-lookup"><span data-stu-id="9b8e8-250">Advance to the next tutorial:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="9b8e8-251">Nasaďte aplikaci do Azure</span><span class="sxs-lookup"><span data-stu-id="9b8e8-251">Deploy the application to Azure</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)