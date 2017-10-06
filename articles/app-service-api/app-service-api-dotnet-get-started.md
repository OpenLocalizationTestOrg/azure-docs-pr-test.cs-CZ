---
title: "aaaGet začít s API Apps a technologií ASP.NET ve službě App Service | Microsoft Docs"
description: "Zjistěte, jak nasadit toocreate a využívat aplikace API technologie ASP.NET ve službě Azure App Service pomocí sady Visual Studio 2015."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: ddc028b2-cde0-4567-a6ee-32cb264a830a
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: hero-article
ms.date: 09/20/2016
ms.author: alkarche
ms.openlocfilehash: d3e90f1585907d183b0435c6cafc5585bc1e29ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>Začínáme s aplikacemi API, technologií ASP.NET a Swaggerem v Azure App Service
[!INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Toto je hello, nejprve v řadě kurzy, které ukazují, jak toouse funkce Azure aplikace služby, které jsou užitečné pro vývoj a hostování rozhraní RESTful API.  Tento kurz se zaměřuje na podporu metadat rozhraní API ve formátu Swagger.

Naučíte se:

* Jak toocreate a nasazení [aplikace API](app-service-api-apps-why-best-platform.md) v Azure App Service pomocí nástrojů integrovaných v sadě Visual Studio 2015.
* Jak balíčku Swashbuckle NuGet zjišťování tooautomate rozhraní API pomocí hello toodynamically generování metadat Swagger rozhraní API.
* Jak tooautomatically metadat rozhraní API Swaggeru toouse generovat kód klienta pro aplikaci API.

## <a name="sample-application-overview"></a>Přehled ukázkové aplikace
V tomto návodu budeme pracovat s jednoduchou ukázkovou aplikací seznamu úkolů. Hello aplikace má front-end jednostránkové aplikace (SPA), rozhraní ASP.NET Web API střední vrstvy a rozhraní ASP.NET Web API datové vrstvy.

![Diagram ukázkové aplikace API](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Zde je snímek obrazovky hello [AngularJS](https://angularjs.org/) front-endu.

![Seznam toodo aplikací ukázkové aplikace API](./media/app-service-api-dotnet-get-started/todospa.png)

Hello řešení sady Visual Studio skládá ze tří projektů:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** -hello front-end: JEDNOSTRÁNKOVÁ aplikace AngularJS, která volá střední vrstvy hello.
* **ToDoListAPI** -hello střední vrstva: projekt webového rozhraní API ASP.NET, který volá datovou vrstvu hello tooperform operace CRUD s položkami seznamu úkolů.
* **ToDoListDataAPI** -hello Datová vrstva: projekt webového rozhraní API ASP.NET, který provádí operace CRUD s položkami seznamu úkolů.

Hello třívrstvá architektura je jednou z mnoha architektur, které můžete implementovat pomocí aplikací API a tady je použita pouze pro účely ukázky. Hello kód v jednotlivých vrstvách je jednoduché, funkce API Apps možné toodemonstrate; například hello Datová vrstva používá serveru paměť, místo databáze jako svůj mechanismus trvalosti.

Po dokončení tohoto kurzu, budete mít dva projekty webového rozhraní API hello nahoru a spuštění v cloudu hello v App Service API apps.

Hello další kurz v řadě hello nasadí hello SPA front-endu toohello cloudu.

## <a name="prerequisites"></a>Požadavky
* Rozhraní ASP.NET Web API – hello kurz předpokládá základní znalosti o tom toowork s technologií ASP.NET [webovém rozhraní API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) v sadě Visual Studio.
* Účet Azure – můžete si [zdarma otevřít účet Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) nebo [aktivovat výhody pro předplatitele nástroje Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
  
    Pokud chcete, aby tooget začít s Azure App Service, ještě než si zaregistrujete účet Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/). Tam si můžete v App Service hned vytvořit krátkodobou úvodní aplikaci – **nepožaduje se žádná platební karta**, ani to s sebou nenese žádné závazky.
* Visual Studio 2015 se hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) -hello SDK nainstaluje Visual Studio 2015 automaticky, pokud ještě nemáte ho.
  
  * V sadě Visual Studio klikněte na Nápověda -> O aplikaci Microsoft Visual Studio a ujistěte se, že máte Azure App Service Tools ve verzi 2.9.1 nebo vyšší.
    
    ![Verze Azure App Tools](./media/app-service-api-dotnet-get-started/apiversion.png)
    
    > [!NOTE]
    > V závislosti na tom, kolik závislostí sady SDK hello už máte na počítači může instalaci hello sady SDK trvat dlouhou dobu, od několika minut tooa půl hodiny nebo i déle.
    > 
    > 

## <a name="download-hello-sample-application"></a>Stáhnout ukázkové aplikace hello
1. Stáhnout hello [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) úložiště.
   
    Můžete kliknout na hello **stáhnout ZIP** tlačítko nebo klonování hello úložiště na místním počítači.
2. Otevřete řešení seznamu úkolů hello v sadě Visual Studio 2015 nebo 2013.
   
   1. Je nutné tootrust každé řešení.
         ![Upozornění zabezpečení](./media/app-service-api-dotnet-get-started/securitywarning.png)
3. Vytvoření balíčků NuGet hello řešení (CTRL + SHIFT + B) toorestore hello.
   
    Pokud chcete aplikace hello toosee v operaci před nasazením, můžete ji spustit místně. Ujistěte se, že je ToDoListDataAPI spouštěný projekt a spusťte hello řešení. Chyba protokolu HTTP 403 toosee byste měli očekávat v prohlížeči.

## <a name="use-swagger-api-metadata-and-ui"></a>Používání uživatelského rozhraní a metadat rozhraní API Swaggeru
Podpora metadat rozhraní API [Swaggeru](http://swagger.io/) 2.0 je integrována v Azure App Service. Každá aplikace API můžete zadat adresu URL koncového bodu, který vrátí metadata pro hello rozhraní API ve formátu JSON pro Swagger. Hello metadata vrácená z tohoto koncového bodu může být použit toogenerate kód klienta.

Projekt webového rozhraní API ASP.NET může dynamicky generovat Swagger metadata pomocí hello [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) balíček NuGet. Hello balíčku Swashbuckle NuGet již je nainstalována v projektech ToDoListDataAPI a ToDoListAPI hello, které jste stáhli.

V této části kurzu hello se podíváte na hello generované metadata Swagger 2.0 a vyzkoušíte si testovací uživatelské rozhraní, která je založená na hello Swagger metadata.

1. Nastavte projekt ToDoListDataAPI hello (**není** hello projekt ToDoListAPI) jako spouštěný projekt hello.
   
    ![Nastavení ToDoDataAPI jako spouštěného projektu](./media/app-service-api-dotnet-get-started/startupproject.png)
2. Stisknutím klávesy F5 nebo klikněte na tlačítko **ladění > Spustit ladění** toorun hello projekt v režimu ladění.
   
    Otevře se prohlížeč Hello a zobrazuje chybová stránka HTTP 403 hello.
3. V panelu Adresa v prohlížeči, přidejte `swagger/docs/v1` toohello konec hello řádku a stiskněte ENTER. (adresa URL je hello `http://localhost:45914/swagger/docs/v1`.)
   
    Toto je výchozí adresa URL hello používá Swashbuckle tooreturn JSON pro Swagger 2.0 metadata pro hello rozhraní API.
   
    Pokud používáte Internet Explorer, hello v prohlížeči výzva toodownload *v1.json* souboru.
   
    ![Stažení metadat JSON do IE](./media/app-service-api-dotnet-get-started/iev1json.png)
   
    Pokud používáte Chrome, Firefox nebo Microsoft Edge, zobrazí prohlížeč hello hello JSON v okně prohlížeče hello. Různé prohlížeče zpracovávají data JSON různě a okno prohlížeče může vypadat třeba hello.
   
    ![Metadata JSON v Chromu](./media/app-service-api-dotnet-get-started/chromev1json.png)
   
    Hello následující příklad ukazuje první část hello hello metadata Swagger pro rozhraní API, hello s hello Definice hello získáte metoda. Tato metadata je, jaké jednotky hello uživatelské rozhraní Swagger, které použijete v hello následující kroky a můžete ji použít v další části kurzu tooautomatically hello generování kódu klienta.
   
        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },
4. Zavřete prohlížeč hello a zastavte ladění v sadě Visual Studio.
5. V hello ToDoListDataAPI projektu v **Průzkumníku řešení**, otevřete hello *App_Start\SwaggerConfig.cs* soubor a poté přejděte dolů tooline 174 a zrušte komentář u následující kód hello.
   
        /*
            })
        .EnableSwaggerUi(c =>
            {
        */
   
    Hello *SwaggerConfig.cs* soubor se vytvoří při instalaci balíčku Swashbuckle hello v projektu. soubor Hello poskytuje několik způsobů tooconfigure Swashbuckle.
   
    Kód Hello jste odkomentovali hello umožňuje uživatelského rozhraní Swagger, které budete používat v hello následující kroky. Když vytvoříte projekt webového rozhraní API pomocí šablony projektu aplikace hello rozhraní API, tento kód je označeno jako komentář ve výchozím nastavení jako bezpečnostní opatření.
6. Znovu spusťte projekt hello.
7. V panelu Adresa v prohlížeči, přidejte `swagger` toohello konec hello řádku a stiskněte ENTER. (adresa URL je hello `http://localhost:45914/swagger`.)
8. Jakmile se zobrazí stránka uživatelského rozhraní Swagger hello, klikněte na tlačítko **ToDoList** toosee hello metody dostupné.
   
    ![Dostupné metody uživatelského rozhraní Swagger](./media/app-service-api-dotnet-get-started/methods.png)
9. Klikněte na tlačítko hello nejprve **získat** tlačítko v seznamu hello.
10. V hello **parametry** části, zadejte hvězdičku hello hodnotu hello `owner` parametr a pak klikněte na tlačítko **vyzkoušení**.
    
    Když v dalších kurzech přidáte ověřování, hello střední vrstva poskytne hello skutečného uživatelského ID toohello datové vrstvy. Teď všechny úlohy má hvězdičky ID vlastníka hello aplikace běží bez povolení ověřování.
    
    ![Vyzkoušení uživatelského rozhraní Swagger](./media/app-service-api-dotnet-get-started/gettryitout1.png)
    
    Hello uživatelské rozhraní Swagger zavolá hello metodu Get seznamu úkolů a zobrazí kód odpovědi hello a výsledky JSON.
    
    ![Vyzkoušení uživatelského rozhraní Swagger – výsledky](./media/app-service-api-dotnet-get-started/gettryitout.png)
11. Klikněte na tlačítko **Post**a potom klikněte na pole hello v části **schéma modelu**.
    
    Kliknutím na tlačítko hello model schématu prefills hello vstupní pole kde můžete určit hello hodnota parametru pro metodu Post hello. (Pokud to nebude fungovat v aplikaci Internet Explorer, použijte jiný prohlížeč, nebo zadejte hodnotu parametru hello ručně v dalším kroku hello.)  
    
    ![Vyzkoušení uživatelského rozhraní Swagger – metoda Post](./media/app-service-api-dotnet-get-started/post.png)
12. Změna hello JSON v hello `todo` parametr vstupní pole tak, aby vypadal jako následující příklad hello, nebo nahraďte váš vlastní text popisu:
    
        {
          "ID": 2,
          "Description": "buy hello dog a toy",
          "Owner": "*"
        }
13. Klikněte na **Try it out** (Vyzkoušet).
    
    Hello rozhraní API seznamu úkolů vrátí kód odpovědi HTTP 204, který označuje úspěch.
14. Klikněte na tlačítko hello nejprve **získat** tlačítko a klikněte v této části hello stránku hello **vyzkoušení** tlačítko.
    
    Hello odpověď metody Get teď obsahuje novou položku toodo hello.
15. Volitelné: Pokuste se také hello Put, Delete a získat ID metody.
16. Zavřete prohlížeč hello a zastavte ladění v sadě Visual Studio.

Swashbuckle funguje s libovolným projektem webového rozhraní API ASP.NET. Pokud chcete tooadd Swagger metadata generování tooan existující projekt, stačí nainstalujte balíček Swashbuckle hello.

> [!NOTE]
> Metadata Swagger mají pro každou operaci s rozhraním API jedinečný identifikátor. Ve výchozím nastavení může Swashbuckle generovat duplicitní ID operací Swagger pro metody kontroleru webového rozhraní API. K tomu dojde, pokud kontroler přetížil metody HTTP, například `Get()` nebo `Get(id)`. Informace o tom, jak toohandle přetížení najdete v tématu [definice rozhraní API generovaných ve Swashbuckle přizpůsobit](app-service-api-dotnet-swashbuckle-customize.md). Pokud vytvoříte projekt webového rozhraní API v sadě Visual Studio pomocí šablony hello aplikace Azure API, kód, který generuje jedinečné identifikátory operací se automaticky přidá toohello *SwaggerConfig.cs* souboru.  
> 
> 

## <a id="createapiapp"></a>Vytvoření aplikace API v Azure a nasadit tooit kódu
V této části použijte nástroje Azure, které jsou integrovány do hello Visual Studio **Publikovat Web** aplikaci průvodce toocreate nové rozhraní API v Azure. Potom nasaďte hello ToDoListDataAPI projektu toohello nové aplikace API a volání rozhraní API hello spuštěním hello uživatelské rozhraní Swagger.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ToDoListDataAPI hello a pak klikněte na tlačítko **publikovat**.
   
    ![Kliknutí na Publikovat v nástroji Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu.png)
2. V hello **profil** krok hello **Publikovat Web** průvodce, klikněte na tlačítko **Microsoft Azure App Service**.
   
   ![Kliknutí na Azure App Service v průvodci Publikovat web](./media/app-service-api-dotnet-get-started/selectappservice.png)
3. Přihlášení tooyour účet Azure, pokud jste tak již neučinili, nebo aktualizujte přihlašovací údaje, pokud jste v platnost vypršela.
4. V hello dialogové okno služby App Service, zvolte hello Azure **předplatné** toouse a pak klikněte na **nový**.
   
    ![Kliknutí na Nová v dialogovém okně App Service](./media/app-service-api-dotnet-get-started/clicknew.png)
   
    Hello **hostitelský** kartě hello **vytvořit službu App Service** zobrazí se dialogové okno.
   
    Vzhledem k tomu, že nasazujete projekt webového rozhraní API, který je nainstalován Swashbuckle, předpokládá Visual Studio, které chcete toocreate aplikace API. To vyplývá z hello **název aplikace API** title a podle hello fakt, že hello **změnit typ** rozevíracího seznamu je nastaven příliš**aplikace API**.
   
    ![Typ aplikace v dialogovém okně App Service](./media/app-service-api-dotnet-get-started/apptype.png)
5. Zadejte **název aplikace API** který bude v hello *azurewebsites.net* domény. Můžete přijmout výchozí název hello, který Visual Studio navrhne.
   
    Pokud zadáte název, který již použil někdo jiný, zobrazí se toohello vpravo červený vykřičník.
   
    Hello hello rozhraní API aplikace bude mít adresu URL `{API app name}.azurewebsites.net`.
6. V hello **skupiny prostředků** rozevíracího seznamu, klikněte na tlačítko **nový**a pak zadejte "ToDoListGroup" nebo jiný název, který preferujete.
   
    Skupina prostředků je kolekce prostředků Azure, jako jsou aplikace API, databáze, virtuální počítače apod.    V tomto kurzu je nejlepší toocreate novou skupinu prostředků vzhledem k tomu, který umožňuje snadno toodelete v jednom kroku všechny prostředky Azure, které vytvoříte pro kurz hello hello.
   
    V tomto poli můžete vybrat existující [skupinu prostředků](../azure-resource-manager/resource-group-overview.md). Můžete taky vytvořit novou tak, že zadáte název, který zatím nemá žádná existující skupina prostředků ve vašem předplatném.
7. Klikněte na tlačítko hello **nový** tlačítko Další toohello **plán služby App Service** rozevíracího seznamu.
   
    Hello snímek obrazovky ukazuje ukázkové hodnoty pro **název aplikace API**, **předplatné**, a **skupiny prostředků** – vaše hodnoty budou lišit.
   
    ![Dialogové okno Vytvořit službu App Service](./media/app-service-api-dotnet-get-started/createas.png)
   
    V hello postupu vytvoříte plán služby App Service pro novou skupinu prostředků hello. Plán služby App Service určuje výpočetní prostředky hello, které vaše aplikace API poběží na. Například pokud zvolíte hello úroveň free, vaše aplikace API poběží na sdílených virtuálních počítačích, zatímco u některých placených úrovní běží na vyhrazených virtuálních počítačích. Informace o plánech služby App Service naleznete v části [Přehled plánů služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
8. V hello **nakonfigurovat plán služby App Service** dialogové okno, zadejte "ToDoListPlan" nebo jiný název, který preferujete.
9. V hello **umístění** rozevíracího seznamu vyberte hello umístění, které je nejblíže tooyou.
   
    Toto nastavení určuje, ve kterém datacentru Azure vaše aplikace poběží. Vyberte umístění zavřete tooyou toominimize [latence](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).
10. V hello **velikost** rozevíracího seznamu, klikněte na tlačítko **volné**.
    
    V tomto kurzu zajistí hello volné cenová úroveň dostatečný výkon.
11. V hello **nakonfigurovat plán služby App Service** dialogové okno, klikněte na tlačítko **OK**.
    
    ![Kliknutí na OK v dialogovém okně Nakonfigurovat plán služby App Service](./media/app-service-api-dotnet-get-started/configasp.png)
12. V hello **vytvořit službu App Service** dialogové okno, klikněte na tlačítko **vytvořit**.
    
    ![Kliknutí na Vytvořit v dialogovém okně Vytvořit službu App Service](./media/app-service-api-dotnet-get-started/clickcreate.png)
    
    Visual Studio vytvoří aplikaci API hello a profil publikování, který má všechna hello požadované nastavení pro aplikace hello rozhraní API. Potom otevře hello **Publikovat Web** průvodce, který budete používat toodeploy hello projektu.
    
    Hello **Publikovat Web** otevře se Průvodce na hello **připojení** (viz následující obrázek).
    
    Na hello **připojení** kartě hello **Server** a **název lokality** aplikace API bodu tooyour nastavení. Hello **uživatelské jméno** a **heslo** jsou přihlašovací údaje nasazení, které pro vás vytvoří Azure. Po nasazení otevře Visual Studio prohlížeče toohello **cílová adresa URL** (který je jediným účelem hello **cílová adresa URL**).  
13. Klikněte na **Další**.
    
    ![Kliknutí na Další na kartě Připojení průvodce Publikovat web](./media/app-service-api-dotnet-get-started/connnext.png)
    
    Další karta Hello je hello **nastavení** (viz následující obrázek). Zde můžete změnit hello sestavení konfigurace karta toodeploy sestavení pro ladění [vzdálené ladění](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug). Hello kartě je také několik **možností publikování souboru**:
    
    * Odebrat další soubory v cíli
    * Předkompilovat během publikování
    * Vyloučit soubory ze složky App_Data hello
    
    V tomto kurzu není potřeba vybírat žádnou z nich. Podrobné vysvětlení, co tyto možnosti představují, najdete v části [Postupy: Nasazení webového projektu pomocí publikování jedním kliknutím v nástroji Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).
14. Klikněte na **Další**.
    
    ![Kliknutí na Další na kartě Nastavení průvodce Publikovat web](./media/app-service-api-dotnet-get-started/settingsnext.png)
    
    Dále je hello **Preview** kartu (viz následující obrázek), která umožňuje toosee příležitost, které soubory budou zkopírovány z projektu aplikace toohello rozhraní API toobe. Pokud nasazujete aplikace na projektu tooan rozhraní API už nasazená tooearlier, zkopírují se pouze změněné soubory. Pokud chcete toosee seznam kopírovaného, můžete kliknout na hello **spustit Náhled** tlačítko.
15. Klikněte na **Publikovat**.
    
    ![Kliknutí na Publikovat na kartě Náhled v průvodci Publikovat web](./media/app-service-api-dotnet-get-started/clickpublish.png)
    
    Visual Studio nasadí hello ToDoListDataAPI projektu toohello nové aplikace API. Hello **výstup** zaprotokoluje úspěšné nasazení a v adrese URL prohlížeče otevřené okno toohello hello rozhraní API aplikace se zobrazí stránka s "byla úspěšně vytvořena.".
    
    ![Okno výstupu – úspěšné nasazení](./media/app-service-api-dotnet-get-started/deploymentoutput.png)
    
    ![Stránka úspěšného vytvoření nové aplikace API](./media/app-service-api-dotnet-get-started/appcreated.png)
16. Přidejte "swagger" toohello adresu URL v adresním řádku prohlížeče hello a stiskněte klávesu Enter. (adresa URL je hello `http://{apiappname}.azurewebsites.net/swagger`.)
    
    Hello prohlížeč zobrazí stejné uživatelské rozhraní Swagger, které jste předtím viděli, ale teď běží v cloudu hello hello. Vyzkoušejte hello metodu Get a zjistíte, že jste zpět toohello 2 výchozích položek. Hello změny, které jste provedli dříve byly uloženy v paměti v místním počítači hello.
17. Otevřete hello [portál Azure](https://portal.azure.com/).
    
    Hello portál Azure je webové rozhraní pro správu prostředků Azure, jako je například aplikace API.
18. Klikněte na **Další služby > App Services**.
    
    ![Procházení služeb App Services](./media/app-service-api-dotnet-get-started/browseas.png)
19. V hello **App Services** okno, vyhledejte a vyberte novou aplikaci API. (V hello portálu Azure, se nazývají windows, které se otevřou vpravo toohello *okna*.)
    
    ![Okno App Services](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)
    
    Otevřou se dvě okna. V jednom okně je přehled aplikace API hello a ve druhém dlouhý seznam nastavení, které můžete zobrazit a změnit.
20. V hello **nastavení** okno, najít hello **rozhraní API** části a klikněte na tlačítko **definice rozhraní API**.
    
    ![Definice rozhraní API v okně Nastavení](./media/app-service-api-dotnet-get-started/apidefinsettings.png)
    
    Hello **definice rozhraní API** okno umožňuje určit hello URL, která vrací metadata Swagger 2.0 ve formátu JSON. Když Visual Studio vytvoří aplikaci hello rozhraní API, nastaví výchozí hodnota adresy URL toohello hello rozhraní API definice pro metadata generovaná ve Swashbuckle, jste viděli dříve, což je aplikace hello rozhraní API základní adresa URL plus `/swagger/docs/v1`.
    
    ![Adresa URL definice rozhraní API](./media/app-service-api-dotnet-get-started/apidefurl.png)
    
    Když vyberete kódu toogenerate klienta aplikace API pro něj, načte Visual Studio hello metadata z této adresy URL.

## <a id="codegen"></a>Generování kódu klienta pro hello datové vrstvy
Jednou z výhod hello integraci Swagger do aplikace Azure API je automatické generování kódu. Vygenerované třídy klienta umožňují snadnější toowrite kód, který volá aplikaci API.

Projekt ToDoListAPI Hello již hello vygenerovat kód klienta, ale v hello následující kroky vám ho odstraníme a znovu vygenerovat toosee jak toodo hello generování kódu.

1. V sadě Visual Studio **Průzkumníku řešení**, v hello ToDoListAPI projektu, odstraňovat hello *ToDoListDataAPI* složky. **Upozornění: Odstraňte jenom hello složku, ne projekt ToDoListDataAPI hello.**
   
    ![Odstranění vygenerovaného kódu klienta](./media/app-service-api-dotnet-get-started/deletecodegen.png)
   
    Tato složka byla vytvořena pomocí hello kódu generování proces, který jste o toogo prostřednictvím.
2. Klikněte pravým tlačítkem na projekt ToDoListAPI hello a pak klikněte na **Přidat > klient REST API**.
   
    ![Přidání klienta REST API v nástroji Visual Studio](./media/app-service-api-dotnet-get-started/codegenmenu.png)
3. V hello **přidat klienta REST API** dialogové okno, klikněte na tlačítko **adresa URL Swaggeru**a potom klikněte na **vybrat prostředek Azure**.
   
    ![Výběr prostředku Azure](./media/app-service-api-dotnet-get-started/codegenbrowse.png)
4. V hello **služby App Service** dialogové okno, rozbalte skupinu prostředků hello používáte v tomto kurzu vyberte svou aplikaci API a pak klikněte na tlačítko **OK**.
   
    ![Výběr aplikace API pro generování kódu](./media/app-service-api-dotnet-get-started/codegenselect.png)
   
    Všimněte si, že při návratu toohello **přidat klienta REST API** dialogové okno, hello textového pole doplnila hello rozhraní API definice hodnota adresy URL, kterou jste viděli dříve portálu hello.
   
    ![Adresa URL definice rozhraní API](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)
   
   > [!TIP]
   > Alternativní způsob tooget metadat pro generování kódu je adresa URL hello tooenter přímo místo dialogové okno Procházet hello. Pokud chcete kód klienta toogenerate před nasazením tooAzure, můžete spustit projekt webového rozhraní API hello místně, přejít toohello URL, která poskytuje soubor dat JSON pro Swagger hello, uložte soubor hello, a použijte hello **vyberte existující soubor metadat Swagger**možnost.
   > 
   > 
5. V hello **přidat klienta REST API** dialogové okno, klikněte na tlačítko **OK**.
   
    Visual Studio vytvoří složku pojmenovanou aplikace hello rozhraní API a vygeneruje třídy klienta.
   
    ![Soubory kódu pro generovaného klienta](./media/app-service-api-dotnet-get-started/codegenfiles.png)
6. Otevřete v projektu ToDoListAPI hello *Controllers\ToDoListController.cs* toosee hello kódu na řádku 40, který volá rozhraní API hello pomocí hello generované klienta.
   
    Hello následující fragment kódu ukazuje, jak hello kód vytvoří objekt hello klienta a volání hello metodu Get.
   
        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }
   
        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }
   
    Hello parametr konstruktoru získá adresu URL koncového bodu hello z hello `toDoListDataAPIURL` nastavení aplikace. V souboru Web.config hello že hodnota je sada toohello místní služby IIS Express URL hello API projekt tak, aby aplikace hello můžete spustit místně. Pokud vynecháte parametr konstruktoru hello, je výchozí koncový bod hello hello adresu URL, který jste vygenerovali hello kód z.
7. Třída klienta se vygeneruje s jiným názvem podle názvu vaší aplikace API; změnit kód hello v *Controllers\ToDoListController.cs* tak, aby hello název typu odpovídal co bylo vygenerováno v projektu. Pokud jste například aplikaci API nazvali ToDoListDataAPI071316, pak byste tento kód:
   
        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

toothis:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-toohost-hello-middle-tier"></a>Vytvoření rozhraní API aplikace toohost hello střední vrstva
Dříve jste [aplikaci API datové vrstvy hello vytvoření a nasazení kódu tooit](#createapiapp).  Nyní budete postupovat podle hello stejný postup pro aplikaci API střední vrstvy hello.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello střední vrstvy ToDoListAPI projektu (ne hello datovou vrstvu ToDoListDataAPI) a potom klikněte na **publikovat**.
   
    ![Kliknutí na Publikovat v nástroji Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu2.png)
2. V hello **profil** kartě hello **Publikovat Web** průvodce, klikněte na tlačítko **Microsoft Azure App Service**.
3. V hello **služby App Service** dialogové okno, klikněte na tlačítko **nový**.
4. V hello **hostitelský** kartě hello **vytvořit službu App Service** dialogové okno pole, přijměte výchozí hello **název aplikace API** nebo zadejte název, který je v hello jedinečný  *azurewebsites.NET* domény.
5. Zvolte hello Azure **předplatné** používáte.
6. V hello **skupiny prostředků** rozevíracího seznamu, vyberte hello stejnou skupinu prostředků, které jste vytvořili dříve.
7. V hello **plán služby App Service** rozevíracího seznamu, vyberte hello stejný plán, který jste vytvořili dříve. Bude použita výchozí hodnota toothat.
8. Klikněte na možnost **Vytvořit**.
   
    Visual Studio vytvoří aplikaci hello rozhraní API, vytvoří pro ni profil publikování a zobrazí hello **připojení** krok hello **Publikovat Web** průvodce.
9. V hello **připojení** krok hello **Publikovat Web** průvodce, klikněte na tlačítko **publikovat**.
   
   Visual Studio nasadí hello ToDoListAPI projektu toohello nové aplikace API a otevře prohlížeč toohello URL aplikace API hello. Zobrazí se stránka Hello "úspěšně vytvořen".

## <a name="configure-hello-middle-tier-toocall-hello-data-tier"></a>Konfigurace hello střední vrstvy toocall hello datové vrstvy
Pokud jste nyní volá aplikaci API střední vrstvy hello, pokusí se toocall hello datovou vrstvu pomocí hello adresu URL místního hostitele, která je stále v souboru Web.config hello. V této části zadejte hello adresu URL aplikace rozhraní API datové vrstvy do nastavení prostředí v aplikaci API střední vrstvy hello. Když hello kódu v aplikaci API střední vrstvy hello načte nastavení adresy URL aplikace hello data vrstvy, přepíše nastavení prostředí hello, co je v souboru Web.config hello.

1. Přejděte toohello [portál Azure](https://portal.azure.com/)a potom přejděte toohello **aplikace API** hello rozhraní API aplikace, které jste vytvořili projekt TodoListAPI (střední úroveň) toohost hello.
2. V hello aplikace API **nastavení** okně klikněte na tlačítko **nastavení aplikace**.
3. V hello aplikace API **nastavení aplikace** okně projděte dolů toohello **nastavení aplikace** a přidejte hello následující klíč a hodnotu. Hodnota Hello bude hello URL hello prvního rozhraní API aplikace, které jste publikovali v tomto kurzu.
   
   | **Klíč** | toDoListDataAPIURL |
   | --- | --- |
   | **Hodnota** |https://{název vaší aplikace API datové vrstvy}.azurewebsites.net |
   | **Příklad** |https://todolistdataapi.azurewebsites.net |
4. Klikněte na **Uložit**.
   
    ![Kliknutí na Uložit v Nastavení aplikace](./media/app-service-api-dotnet-get-started/asinportal.png)
   
    Při spuštění hello kódu v Azure, tato hodnota přepíše adresu URL hello místního hostitele, který je v souboru Web.config hello.

## <a name="test"></a>Test
1. V okně prohlížeče procházet toohello URL hello novou aplikaci API střední vrstvy, který jste právě vytvořili pro projekt ToDoListAPI. Lze tak učinit klepnutím hello adresu URL v hlavním okně aplikace hello rozhraní API portálu hello.
2. Přidejte "swagger" toohello adresu URL v adresním řádku prohlížeče hello a stiskněte klávesu Enter. (adresa URL je hello `http://{apiappname}.azurewebsites.net/swagger`.)
   
    Hello prohlížeče zobrazí hello stejné uživatelské rozhraní Swagger, které jste předtím viděli pro ToDoListDataAPI, ale nyní `owner` není povinné pole pro operaci Get hello, protože aplikaci API střední vrstvy hello odesílá aplikaci API datové vrstvy tuto hodnotu toohello za vás. (Pokud hello kurzech ověřování, odešle hello střední vrstva skutečné uživatelské ID pro hello `owner` parametr; teď ho je pevně kódováno hvězdičky.)
3. Vyzkoušejte metodu Get hello a hello toovalidate jiné metody, která hello aplikaci API střední vrstvy úspěšně volá aplikaci API datové vrstvy hello.
   
    ![Metoda Get uživatelského rozhraní Swagger](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>Řešení potíží
V případě, že při procházení tohoto kurzu narazíte na problém, můžete ho zkusit vyřešit pomocí následujících kroků:

* Ujistěte se, že používáte nejnovější verzi hello hello [Azure SDK for .NET](http://go.microsoft.com/fwlink/?linkid=518003).
* Dva názvy projektů hello jsou podobné (ToDoListAPI, ToDoListDataAPI). Pokud věcí nevypadají, jak je popsáno v hello pokyny, když pracujete s projektem, ujistěte se, že máte otevřený správný projekt hello.
* Pokud jste v podnikové síti a pokoušíte toodeploy tooAzure služby App Service přes bránu firewall, ujistěte se, že jsou pro nasazení webu otevřené porty 443 a 8172. Pokud tyto porty nedají otevřít, můžete použít jiné metody nasazení.  V tématu [nasazení vaší aplikace tooAzure služby App Service](../app-service-web/web-sites-deploy.md).
* Chyby "názvy tras musí být jedinečné" – může dojít, tyto Pokud neúmyslně nasadíte aplikaci tooan rozhraní API hello nesprávný projekt a později nasadíte správný jeden tooit hello. toocorrect této, znovu ho zaveďte hello správný projekt toohello rozhraní API aplikace a na hello **nastavení** kartě hello **Publikovat Web** průvodce vyberte **odebrat další soubory v cílovém umístění**.

Až budete mít vaše aplikace API technologie ASP.NET spuštěná ve službě Azure App Service, můžete toolearn Další informace o sadě Visual Studio funkce, které usnadňují řešení potíží. Informace o protokolování, vzdáleném ladění apod. najdete v tématu [Řešení potíží s aplikacemi Azure App Service v nástroji Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Další kroky
Už víte, jak generovat kód klienta pro aplikace API toodeploy existující webové projekty tooAPI aplikace API a využívat aplikace API z klientů .NET. Hello další kurzu této série se dozvíte, jak příliš[používat aplikace CORS tooconsume rozhraní API z klientů JavaScript](app-service-api-cors-consume-javascript.md).

Další informace o generování kódu klienta najdete v tématu hello [Azure/AutoRest](https://github.com/azure/autorest) na webu GitHub.com. Pomoc s pomocí klienta hello generuje problémy, otevřete [potíže v úložišti AutoRest hello](https://github.com/azure/autorest/issues).

Pokud chcete toocreate nový projekt aplikace API od začátku, použijte hello **aplikace API Azure** šablony.

![Šablona aplikace API v nástroji Visual Studio](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

Hello **aplikace API Azure** šablona projektu je ekvivalentní toochoosing hello **prázdný** ASP.NET 4.5.2 šablona, kliknutím na tlačítko podpory webového rozhraní API tooadd hello zaškrtávací políčko a instalaci hello balíčku Swashbuckle NuGet. Kromě toho přidá šablona hello některé Swashbuckle konfigurace kód tooprevent hello vytvoření duplicitních ID operací Swagger. Po vytvoření projektu aplikace API, abyste ji mohli nasadit aplikace hello tooan rozhraní API stejným způsobem, jakým jste viděli v tomto kurzu.

