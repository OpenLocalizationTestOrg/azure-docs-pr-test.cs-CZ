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
# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a>Vytvoření a nasazení aplikace pomocí služby front-endové webové rozhraní API ASP.NET Core a stavové služby back-end
V tomto kurzu je součástí, jednu z řady.  Se dozvíte, jak toocreate aplikace Azure Service Fabric pomocí webového rozhraní API ASP.NET Core přední ukončení a stavové služby back-end toostore vaše data. Jakmile budete hotovi, máte hlasovací aplikaci s ASP.NET Core web, který je front-end, který uloží výsledků hlasování ve stavové služby back-end v clusteru hello. Pokud nechcete, aby toomanually vytvořit hello hlasovací aplikaci, můžete [stáhnout hello zdrojového kódu](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) hello byla dokončena aplikace a přeskočit příliš[provede hello hlasování ukázkovou aplikaci](#walkthrough_anchor).

![Diagram aplikace](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

V rámci jednu z řady hello, zjistíte, jak:

> [!div class="checklist"]
> * Vytvoření služby webového rozhraní API ASP.NET Core jako stavové spolehlivé služby
> * Vytvoření služby webové aplikace ASP.NET Core jako bezstavové webové služby
> * Pomocí toocommunicate reverzní proxy server hello hello stavové služby

V této série kurzu zjistíte, jak:
> [!div class="checklist"]
> * Sestavení aplikace .NET Service Fabric
> * [Nasazení vzdáleného clusteru tooa hello aplikace](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Konfigurace CI/CD pomocí Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Nainstalovat Visual Studio 2017](https://www.visualstudio.com/) a nainstalujte hello **Azure development** a **ASP.NET a webové vývoj** úlohy.
- [Nainstalujte hello Service Fabric SDK](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a>Vytvoření služby ASP.NET Web API jako spolehlivě.
Nejprve vytvořte hello webový front-end Dobrý den, hlasování aplikace pomocí ASP.NET Core. ASP.NET Core je rozhraní vývoj webové lightweight, a platformy, můžete použít toocreate moderní webového uživatelského rozhraní a webovým rozhraním API. tooget dokončení znalosti o tom, jak ASP.NET Core integruje se službou Service Fabric, důrazně doporučujeme čtení prostřednictvím hello [ASP.NET Core v Service Fabric spolehlivé služby](service-fabric-reliable-services-communication-aspnetcore.md) článku. Teď můžete postupovat podle tohoto kurzu tooget rychle začít. toolearn Další informace o ASP.NET Core, najdete v části hello [ASP.NET základní dokumentace](https://docs.microsoft.com/aspnet/core/).

> [!NOTE]
> V tomto kurzu vychází z hello [ASP.NET Core nástrojů pro Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc). Hello .NET Core nástrojů pro Visual Studio 2015 se již nebude aktualizováno.

1. Spusťte sadu Visual Studio jako **správce**.

2. Vytvoření projektu s **soubor**->**nový**->**projektu**

3. V hello **nový projekt** dialogovém okně, vyberte **Cloud > aplikace Service Fabric**.

4. Název aplikace hello **Voting** a stiskněte klávesu **OK**.

   ![Dialogové okno Nový projekt ve Visual Studiu](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. Na hello **nové služby Fabric** vyberte **bezstavové ASP.NET Core**a název vaší služby **VotingWeb**.
   
   ![Výběr webové služby ASP.NET v hello dialogové okno Nová služba](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. Další stránku Hello poskytuje sadu ASP.NET Core šablony projektů. V tomto kurzu zvolte **webové aplikace**. 
   
   ![Zvolte typ projektu ASP.NET](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   Visual Studio vytvoří aplikaci a projekt služby a zobrazí je v Průzkumníku řešení.

   ![Průzkumník řešení po vytvoření aplikace ASP.NET základní webového rozhraní API služby]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-toohello-votingweb-service"></a>Přidání AngularJS toohello VotingWeb služby
Přidat [AngularJS](http://angularjs.org/) tooyour služby pomocí integrovaných hello [Bower podporu](/aspnet/core/client-side/bower). Otevřete *bower.json* a přidejte položky pro úhlová a úhlová bootstrap a pak změny uložte.

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
Při ukládání hello *bower.json* soubor, úhlová je nainstalován ve vašem projektu *wwwroot/lib* složky. Kromě toho je uveden v rámci hello *závislosti/Bower* složky.

### <a name="update-hello-sitejs-file"></a>Aktualizujte soubor site.js hello
Otevřete hello *wwwroot/js/site.js* souboru.  Nahraďte jeho obsah hello JavaScript používané hello domovské zobrazení:

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

### <a name="update-hello-indexcshtml-file"></a>Soubor Index.cshtml hello aktualizace
Otevřete hello *Views/Home/Index.cshtml* soubor, hello zobrazení konkrétní domovskou toohello řadič.  Nahraďte jeho obsah hello následující a potom uložte změny.

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

### <a name="update-hello-layoutcshtml-file"></a>Soubor _Layout.cshtml hello aktualizace
Otevřete hello *Views/Shared/_Layout.cshtml* soubor, hello výchozí rozložení pro aplikaci ASP.NET hello.  Nahraďte jeho obsah hello následující a potom uložte změny.

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

### <a name="update-hello-votingwebcs-file"></a>Aktualizujte soubor VotingWeb.cs hello
Otevřete hello *VotingWeb.cs* souboru, který vytvoří hello ASP.NET Core tomuto webovému hostiteli uvnitř hello bezstavové služby pomocí hello WebListener webový server.  Přidat hello `using System.Net.Http;` direktivy toohello horní části souboru hello.  Nahraďte hello `CreateServiceInstanceListeners()` fungovat s hello následující a potom uložte změny.

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

### <a name="add-hello-votescontrollercs-file"></a>Přidejte soubor VotesController.cs hello
Přidáte řadič, který definuje hlasujících akce. Klikněte pravým tlačítkem na hello **řadiče** složku, pak vyberte **Přidat -> Nový položky -> třída**.  Název souboru hello "VotesController.cs" a klikněte na tlačítko **přidat**.  Nahraďte obsah souboru hello hello následující a potom uložte změny.  Dále v [soubor VotesController.cs hello aktualizace](#updatevotecontroller_anchor), tento soubor bude upravené tooread a zápisu hlasujících dat ze hello back endové službě.  Prozatím se vrátí hello řadiče zobrazení toohello dat statický řetězec.

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



### <a name="deploy-and-run-hello-application-locally"></a>Nasazení a spuštění aplikace hello místně
Teď můžete pokračovat a spustit aplikaci hello. V sadě Visual Studio, stiskněte klávesu `F5` aplikace hello toodeploy pro ladění. `F5`selže, pokud nebyla dříve otevřete Visual Studio jako **správce**.

> [!NOTE]
> Hello poprvé spustíte a nasadit hello aplikaci místně, Visual Studio vytvoří místní cluster pro ladění.  Vytvoření clusteru může trvat delší dobu. Stav vytváření clusteru Hello se zobrazí v okně výstupu sady Visual Studio hello.

Webové aplikace v tomto okamžiku by měl vypadat přibližně takto:

![ASP.NET Core front-endu](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

ladění aplikace hello toostop vraťte tooVisual Studio a stiskněte klávesu **Shift + F5**.

## <a name="add-a-stateful-back-end-service-tooyour-application"></a>Přidání aplikace tooyour stavové služby back-end
Teď, když máme rozhraní ASP.NET Web API služby spuštěné v naší aplikaci, přejdeme dopředu a přidejte toostore stavové služby spolehlivé některá data v naší aplikaci.

Service Fabric vám umožní tooconsistently a spolehlivě ukládat vaše právo data uvnitř vaší služby pomocí spolehlivé kolekce. Spolehlivé kolekce jsou sadu vysoce dostupné a spolehlivého kolekce tříd, které jsou známé tooanyone, který byl použit C# kolekce.

V tomto kurzu vytvoříte službu, která ukládá hodnotu čítače v kolekci spolehlivé.

1. V Průzkumníku řešení klikněte pravým tlačítkem na **služby** uvnitř hello projekt aplikace a zvolte **Přidat > Nový Service Fabric Service**.
   
    ![Přidání nové služby tooan existující aplikace](./media/service-fabric-tutorial-create-dotnet-app/vs-add-new-service.png)

2. V hello **nové služby Fabric** dialogovém okně, vyberte **Stateful ASP.NET Core**a název služby hello **VotingData** a stiskněte klávesu **OK**.

    ![Dialogové okno Nová služba ve Visual Studiu](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    Po vytvoření svůj projekt služby, máte dvě služby ve vaší aplikaci. Při dalším toobuild vaší aplikace, můžete přidat další služby v hello stejným způsobem. Každý může být nezávisle verzí a upgradovaný.

3. Další stránku Hello poskytuje sadu ASP.NET Core šablony projektů. V tomto kurzu zvolte **webového rozhraní API**.

    ![Zvolte typ projektu ASP.NET](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    Visual Studio vytvoří projekt služby a zobrazí je v Průzkumníku řešení.

    ![Průzkumník řešení](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-hello-votedatacontrollercs-file"></a>Přidejte soubor VoteDataController.cs hello

V hello **VotingData** projektu klikněte pravým tlačítkem na hello **řadiče** složku, pak vyberte **Přidat -> Nový položky -> třída**. Název souboru hello "VoteDataController.cs" a klikněte na tlačítko **přidat**. Nahraďte obsah souboru hello hello následující a potom uložte změny.

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


## <a name="connect-hello-services"></a>Připojení služby hello
V tomto kroku další jsme připojí hello dvě služby a zkontrolujte hello front-endové webové aplikace získání a nastavení, hlasování informace z hello back endové službě.

Service Fabric nabízí flexibilitu v tom, jak komunikovat se službami reliable services. V rámci jedné aplikace můžete mít služby, které jsou přístupné přes TCP. Dalším službám, které může být přístupné přes rozhraní HTTP REST API a stále dalších služeb může být přístupné přes webové sokety. Pro informace o dostupných možnostech hello a hello kompromisy související se situací, viz [komunikaci se službou](service-fabric-connect-and-communicate-with-services.md).

V tomto kurzu používáme [webového rozhraní API ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md).

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-hello-votescontrollercs-file"></a>Aktualizujte soubor VotesController.cs hello
V hello **VotingWeb** projekt, otevřete hello *Controllers/VotesController.cs* souboru.  Nahraďte hello `VotesController` třídy definice obsah s hello následující a potom uložte změny.

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

## <a name="walk-through-hello-voting-sample-application"></a>Provede hello hlasování ukázkové aplikace
Hello hlasování aplikace se skládá ze dvou služeb:
- Webová služba front-endu (VotingWeb) – ASP.NET Core webových front-endové služby, která obsluhuje hello webové stránky a zpřístupňuje rozhraní API toocommunicate s hello back-end službu.
- Služba back endu (VotingData)-ASP.NET Core webová služba, která zpřístupňuje rozhraní API toostore hello hlas výsledkem slovník spolehlivé trvalé na disku.

![Diagram aplikace](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

Když hlasovat v hello aplikace hello následující dojít k události:
1. JavaScript odešle hello hlas požadavek toohello webové rozhraní API v hello webové front-endové služby jako požadavek HTTP PUT.

2. front-end webové Hello používá proxy toolocate a předávat k službě back-end toohello požadavek HTTP PUT.

3. Hello back endové službě trvá hello příchozího požadavku, a ukládá hello aktualizovat výsledek v spolehlivé slovník, který získá replikované toomultiple uzlů v clusteru hello a trvalé na disku. Všechny aplikace hello data se ukládají v clusteru hello, takže se žádná databáze.

## <a name="debug-in-visual-studio"></a>Ladění v sadě Visual Studio
Při ladění aplikace v sadě Visual Studio, kterou používáte místní cluster Service Fabric vývoj. Máte možnost tooadjust hello váš ladění tooyour scénář prostředí. V této aplikaci uložíme data v naší službě back-end pomocí slovník spolehlivé. Visual Studio odebere aplikaci hello za výchozí při zastavení ladicího programu hello. Odebrání aplikace hello způsobí hello data hello back-end služby tooalso odeberou. toopersist hello data mezi relace ladění, můžete změnit hello **režim ladění aplikací** jako vlastnost na hello **Voting** projektu v sadě Visual Studio.

toolook na co se stane, že v kódu hello dokončení hello následující kroky:
1. Otevřete hello **VotesController.cs** souboru a nastavit zarážky v hello webové rozhraní API **Put** – metoda (řádku 47) – můžete vyhledat hello soubor v Průzkumníku řešení v sadě Visual Studio hello.

2. Otevřete hello **VoteDataController.cs** souboru a nastavit zarážky v tomto rozhraní web API **Put** – metoda (řádku 50).

3. Vrátit toohello prohlížeče a klikněte na hlasování možnost nebo přidat novou možnost hlasování. Kliknutí na první zarážky hello do kontroleru hello webové front endové rozhraní api.
    
    1. Toto je, kde hello JavaScript v prohlížeči hello odešle řadič požadavek toohello webové rozhraní API v hello front-endové služby.
    
    ![Přidejte Front-End služby hlas](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. Nejprve jsme vytvořit adresu URL toohello hello ReverseProxy pro naši službu back-end **(1)**.
    3. Potom pošleme hello požadavek HTTP PUT toohello ReverseProxy **(2)**.
    4. Nakonec hello vrátíme hello odpověď z klienta toohello back-end služby hello **(3)**.

4. Stiskněte klávesu **F5** toocontinue
    1. Nyní jste na hello zarážce v hello back endové službě.
    
    ![Přidat hlas Back-End službu](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. V prvním řádku hello hello metoda **(1)** používáme hello `StateManager` tooget nebo přidejte spolehlivé slovník nazývaný `counts`.
    3. Všechny interakce s hodnotami ve slovníku spolehlivé vyžadují transakci, této konfigurace pomocí příkazu **(2)** vytvoří této transakce.
    4. V transakci hello jsme potom aktualizujte hodnotu hello hello relevantní klíče pro hello volíte možnost a potvrzení hello operaci **(3)**. Po potvrzení hello metoda vrátí, hello dat ve slovníku hello k aktualizaci a replikované tooother uzly v clusteru hello. Hello data jsou nyní bezpečně uložena v clusteru hello a hello back-end služby při selhání tooother uzly, i nadále s hello data k dispozici.
5. Stiskněte klávesu **F5** toocontinue

hello toostop ladicí relace, stiskněte klávesu **Shift + F5**.


## <a name="next-steps"></a>Další kroky
V této části kurzu hello jste zjistili, jak:

> [!div class="checklist"]
> * Vytvoření služby webového rozhraní API ASP.NET Core jako stavové spolehlivé služby
> * Vytvoření služby webové aplikace ASP.NET Core jako bezstavové webové služby
> * Pomocí toocommunicate reverzní proxy server hello hello stavové služby

ADVANCE toohello další kurz:
> [!div class="nextstepaction"]
> [Nasazení aplikace tooAzure hello](service-fabric-tutorial-deploy-app-to-party-cluster.md)
