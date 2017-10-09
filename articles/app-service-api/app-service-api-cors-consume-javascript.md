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
# <a name="consume-an-api-app-from-javascript-using-cors"></a>Využití aplikace API z JavaScriptu pomocí CORS
Služby App Service má integrovanou podporu pro [mezi sdílení prostředků zdroji (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), což umožňuje JavaScript klienti toomake napříč doménami volá tooAPIs, které jsou hostované v aplikacích API. Služby App Service umožňuje nakonfigurovat tooyour CORS přístup k rozhraní API bez psaní kódu v rozhraní API.

Tento článek obsahuje dvě části:

* Hello [jak tooconfigure CORS](#corsconfig) část vysvětluje v obecné postupy tooconfigure CORS pro libovolnou aplikaci API, webovou aplikaci nebo mobilní aplikace. Vztahuje stejnou měrou tooall rozhraní, které jsou podporovány službou App Service, včetně .NET, Node.js a Java. 
* Počínaje hello [pokračování úvodních kurzů .NET hello](#tutorialstart) části článku hello je kurz, který ukazuje CORS podpory nebyla [hello první aplikace API kurzu Začínáme ](app-service-api-dotnet-get-started.md). 

## <a id="corsconfig"></a>Jak tooconfigure CORS v Azure App Service
CORS můžete nakonfigurovat v hello portál Azure nebo pomocí [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) nástroje.

#### <a name="configure-cors-in-hello-azure-portal"></a>Konfigurace CORS v hello portálu Azure
1. Přejděte v prohlížeči toohello [portál Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **App Services**a potom klikněte na název hello vaší aplikace API.
   
    ![Výběr aplikace API na portálu](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. V hello **nastavení** okno, které se otevře napravo toohello od hello **aplikace API** okno, najít hello **rozhraní API** části a potom klikněte na **CORS**.
   
   ![Výběr CORS v okně Nastavení](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. Hello textového pole zadejte hello nebo adresy URL, které chcete tooallow JavaScript volání toocome z.

    Například pokud jste nasadili JavaScript aplikace tooa webovou aplikaci s názvem todolistangular, zadejte "https://todolistangular.azurewebsites.net". Jako alternativu můžete zadat toospecify hvězdičky (*), jsou podmínky přijaty všechny zdrojové domény.


1. Klikněte na **Uložit**.
   
   ![Kliknutí na Uložit](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   Po kliknutí na tlačítko **Uložit**, bude aplikace hello rozhraní API přijímat JavaScript volání z hello zadané adresy URL.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>Konfigurace CORS pomocí nástrojů, které nabízí Azure Resource Manager
CORS můžete také nakonfigurovat pro aplikaci API pomocí [šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) v nástrojích příkazového řádku, jako [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) a hello [rozhraní příkazového řádku Azure](../cli-install-nodejs.md). 

Příklad šablony Azure Resource Manager, která nastaví vlastnost CORS hello otevřete hello [souboru azuredeploy.json v úložišti hello pro ukázkovou aplikaci tohoto kurzu](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Najděte oddíl hello hello šablony, která vypadá jako hello následující ukázka:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a id="tutorialstart"></a>Pokračováním úvodní kurz .NET hello
Pokud postupujete podle hello Node.js nebo Java Začínáme series pro aplikace API, musíte dokončené hello získávání sérii. Přeskočit toohello [další kroky](#next-steps) části toofind návrhy dalších kurzů k API Apps.

Hello zbývající část tohoto článku je pokračování hello .NET Začínáme series a předpokládá, že jste úspěšně dokončili [první kurz hello](app-service-api-dotnet-get-started.md).

## <a name="deploy-hello-todolistangular-project-tooa-new-web-app"></a>Nasazení hello ToDoListAngular projektu tooa nové webové aplikace
V [první kurz hello](app-service-api-dotnet-get-started.md), jste vytvořili aplikaci API střední vrstvy a aplikaci API datové vrstvy. V tomto kurzu vytvoříte webovou aplikaci jednostránkové aplikace (SPA) aplikaci API střední vrstvy hello volání. Pro hello SPA toowork máte tooenable CORS na aplikaci API střední vrstvy hello. 

V hello [ukázkové aplikaci ToDoList](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list), je hello projekt ToDoListAngular jednoduchý klient AngularJS, který volá projekt webového rozhraní API ToDoListAPI střední vrstvy hello. kód jazyka JavaScript v hello Hello *app/scripts/todoListSvc.js* souboru zavolá hello API pomocí zprostředkovatele protokolu HTTP AngularJS hello. 

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

### <a name="create-a-new-web-app-for-hello-todolistangular-project"></a>Vytvoření nové webové aplikace pro projekt ToDoListAngular hello
Hello postup toocreate novou webovou aplikaci služby App Service a nasazení projektu tooit je podobné toowhat jste viděli pro [vytvoření a nasazení aplikace API hello prvního kurzu této série](app-service-api-dotnet-get-started.md#createapiapp). Hello pouze rozdíl je tento typ aplikace hello je **webové aplikace** místo **aplikace API**.  Snímky obrazovky hello dialogů najdete v tématu 

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ToDoListAngular hello a pak klikněte na tlačítko **publikovat**.
2. V hello **profil** kartě hello **Publikovat Web** průvodce, klikněte na tlačítko **Microsoft Azure App Service**.
3. V hello **služby App Service** dialogové okno, klikněte na tlačítko **nový**.
4. V hello **hostitelský** kartě hello **vytvořit službu App Service** dialogovém okně zadejte **název webové aplikace** který bude v hello *azurewebsites.net* domény. 
5. Zvolte hello Azure **předplatné** chcete toowork s.
6. V hello **skupiny prostředků** rozevíracího seznamu vyberte hello stejnou skupinu prostředků, které jste vytvořili dříve.
7. V hello **plán služby App Service** rozevíracího seznamu vyberte hello stejný plán, který jste vytvořili dříve. 
8. Klikněte na možnost **Vytvořit**.
   
    Visual Studio vytvoří hello webovou aplikaci, vytvoří pro ni profil publikování a zobrazí hello **připojení** krok hello **Publikovat Web** průvodce.
   
    Na tlačítko **Publikovat** ještě neklikejte. V následující části hello můžete nakonfigurovat hello nové webové aplikace toocall hello API střední vrstvy aplikaci, která běží ve službě App Service. 

### <a name="set-hello-middle-tier-url-in-web-app-settings"></a>Nastavení adresy URL střední vrstvy hello v nastavení webové aplikace
1. Přejděte toohello [portál Azure](https://portal.azure.com/)a potom přejděte toohello **webové aplikace** okně hello webové aplikace, které jste vytvořili projekt TodoListAngular (front-endu) toohost hello.
2. Klikněte na **Nastavení > Nastavení aplikace**.
3. V hello **nastavení aplikace** přidejte hello následující klíč a hodnotu:
   
   | Klíč | Hodnota | Příklad |
   | --- | --- | --- |
   | toDoListAPIURL |https://{název vaší aplikace API střední vrstvy}.azurewebsites.net |https://todolistapi0121.azurewebsites.net |
4. Klikněte na **Uložit**.
   
    Při spuštění hello kódu v Azure, tato hodnota přepíše adresu URL hello místního hostitele, který je v hello *Web.config* souboru. 
   
    Hello kód, který získá hodnotu nastavení hello je v *index.cshtml*:
   
        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>
   
    Hello kód v *todoListSvc.js* nastavení hello používá:
   
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

### <a name="deploy-hello-todolistangular-web-project-toohello-new-web-app"></a>Nasazení hello webového projektu toohello nové webové aplikace ToDoListAngular
* V sadě Visual Studio v hello **připojení** krok hello **Publikovat Web** průvodce, klikněte na tlačítko **publikovat**.
  
   Visual Studio nasadí hello projektu toohello nové webové aplikace ToDoListAngular a otevře adresu URL prohlížeče toohello hello webové aplikace. 

### <a name="test-hello-application-without-cors-enabled"></a>Testovací aplikace hello bez povolení CORS
1. Ve vývojářských nástrojích prohlížeče otevřete okno konzoly hello.
2. V okně prohlížeče hello, která zobrazuje hello uživatelské rozhraní AngularJS, klikněte na tlačítko hello **tooDo seznamu** odkaz.
   
    Hello kódu jazyka JavaScript se pokusí toocall hello aplikaci API střední vrstvy, ale hello volání selže, protože hello front-end běží v jiné doméně než hello zpět end. okno konzoly vývojářských nástrojů prohlížeče Hello ukazuje cross-origin chybová zpráva.
   
    ![Chybová zpráva nepůvodního zdroje](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-hello-middle-tier-api-app"></a>Konfigurace CORS pro aplikaci API střední vrstvy hello
V této části nakonfigurujete nastavení CORS hello v Azure pro aplikaci ToDoListAPI API střední vrstvy hello. Toto nastavení povolí hello střední vrstvy volání rozhraní API aplikace tooreceive JavaScript z webové aplikace hello, kterou jste vytvořili pro projekt ToDoListAngular hello.

1. Přejděte v prohlížeči, toohello [portál Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **App Services**a potom klikněte na hello rozhraní API ToDoListAPI (střední úroveň) aplikace.
   
    ![Výběr aplikace API na portálu](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. V hello **nastavení** okno, které se otevře napravo toohello od hello **aplikace API** okno, najít hello **rozhraní API** části a potom klikněte na **CORS**.
   
   ![Výběr CORS na portálu](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. Hello textového pole zadejte adresu URL hello pro webové aplikace ToDoListAngular (front-endu) hello. Například pokud jste nasadili hello projektu tooa webové aplikace ToDoListAngular s názvem todolistangular0121, povolte volání z adresy URL hello `https://todolistangular0121.azurewebsites.net`.
   
   Jako alternativu můžete zadat toospecify hvězdičky (*), jsou podmínky přijaty všechny zdrojové domény.
5. Klikněte na **Uložit**.
   
   ![Kliknutí na Uložit](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   Po kliknutí na tlačítko **Uložit**, bude aplikace hello rozhraní API přijímat JavaScript volání z hello zadaná adresa URL. Na tomto snímku obrazovky aplikace hello ToDoListAPI0223 API přijímat volání klienta JavaScript z webové aplikace ToDoListAngular hello.

### <a name="test-hello-application-with-cors-enabled"></a>Testovací aplikace hello s povolením CORS
* Otevřete prohlížeč toohello adresy URL HTTPS webové aplikace hello. 
  
    Tato aplikace hello čas umožňuje zobrazit, přidat, upravit a odstraňovat položky seznamu úkolů. 
  
    ![Stránka seznamu tooDo ukázkové aplikace](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>CORS služby App Service versus CORS webového rozhraní API
V projektu webového rozhraní API můžete nainstalovat hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) toospecify balíček NuGet v kódu kterých domén bude rozhraní API přijímat volání JavaScriptu.

Podpora CORS webového rozhraní API je flexibilnější než podpora CORS služby App Service. Můžete například určit různé povolené zdroje pro různé metody akcí, zatímco u CORS služby App Service zadáváte jednu sadu povolených zdrojů pro všechny metody aplikace API.

> [!NOTE]
> Nepokoušejte toouse CORS webového rozhraní API a CORS služby App Service v jedné aplikaci API. CORS služby App Service bude mít přednost a CORS webového rozhraní API nebude mít žádný vliv. Například pokud povolíte jednu zdrojovou doménu ve službě App Service a povolit všechny zdrojové domény v kódu webového rozhraní API, vaše aplikace Azure API bude pouze přijímat volání z hello domény, který jste zadali v Azure.
> 
> 

### <a name="how-tooenable-cors-in-web-api-code"></a>Jak tooenable CORS v kódu webového rozhraní API
Hello následující kroky shrnují proces hello povolení podpory CORS webového rozhraní API. Další informace najdete v tématu [Povolení žádostí napříč zdroji ve webovém rozhraní API 2 ASP.NET](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

1. V projektu webového rozhraní API, nainstalovat hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) balíček NuGet.
2. Zahrnout `config.EnableCors()` řádek kódu hello **zaregistrovat** metoda hello **WebApiConfig** třídy, jako hello následující ukázka. 
   
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
3. Do kontroleru webového rozhraní API, přidejte `using` příkaz pro hello `System.Web.Http.Cors` obor názvů a přidejte hello `EnableCors` atribut toohello třídu nebo tooindividual metody akce kontroleru. V následujícím příkladu hello platí podpora CORS toohello celý kontroler.
   
        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController

## <a name="using-azure-api-management-with-api-apps"></a>Použití služby API Management Azure pro aplikace API
Pokud používáte Azure API Management s aplikací API, nakonfigurujte CORS ve službě API Management místo v aplikaci API hello. Další informace najdete v tématu hello následující prostředky:

* [Přehled služby Azure API Management (video: CORS začíná ve 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [Mezidoménové zásady služby API Management](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)

## <a name="troubleshooting"></a>Řešení potíží
V případě, že při procházení tohoto kurzu narazíte na problém, můžete ho zkusit vyřešit pomocí následujících kroků.

* Ujistěte se, že používáte nejnovější verzi hello hello [Azure SDK pro .NET pro Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).
* Ujistěte se, zda jste zadali `https` v hello nastavení CORS a ujistěte se, že používáte `https` toorun hello front-endové webové aplikace.
* Ujistěte se, zda jste zadali nastavení CORS hello hello aplikaci API střední vrstvy, není v hello front-endové webové aplikace.
* Pokud konfigurujete CORS v kódu aplikace a služby Azure App Service, Všimněte si, že přepíše nastavení CORS v App Service hello, vaše změny v kódu aplikace. 

toolearn Další informace o funkcích Visual Studio, které usnadňují řešení potíží, najdete v části [aplikacemi řešení potíží s Azure App Service v sadě Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Další kroky
V tomto článku jste viděli, jak tooenable CORS služby App Service podporuje, aby kód JavaScript klienta můžete volat rozhraní API v jiné doméně. Další informace o aplikacích API, přečtěte si hello toolearn [tooauthentication Úvod ve službě App Service](../app-service/app-service-authentication-overview.md), a potom přejděte toohello [ověřování uživatelů pro aplikace API](app-service-api-dotnet-user-principal-auth.md) kurzu.

