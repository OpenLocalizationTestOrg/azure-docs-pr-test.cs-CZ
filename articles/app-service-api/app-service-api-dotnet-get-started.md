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
# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a><span data-ttu-id="30d43-103">Začínáme s aplikacemi API, technologií ASP.NET a Swaggerem v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="30d43-103">Get started with API Apps, ASP.NET, and Swagger in Azure App Service</span></span>
[!INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

<span data-ttu-id="30d43-104">Toto je hello, nejprve v řadě kurzy, které ukazují, jak toouse funkce Azure aplikace služby, které jsou užitečné pro vývoj a hostování rozhraní RESTful API.</span><span class="sxs-lookup"><span data-stu-id="30d43-104">This is hello first in a series of tutorials that show how toouse features of Azure App Service that are helpful for developing and hosting RESTful APIs.</span></span>  <span data-ttu-id="30d43-105">Tento kurz se zaměřuje na podporu metadat rozhraní API ve formátu Swagger.</span><span class="sxs-lookup"><span data-stu-id="30d43-105">This tutorial covers support for API metadata in Swagger format.</span></span>

<span data-ttu-id="30d43-106">Naučíte se:</span><span class="sxs-lookup"><span data-stu-id="30d43-106">You'll learn:</span></span>

* <span data-ttu-id="30d43-107">Jak toocreate a nasazení [aplikace API](app-service-api-apps-why-best-platform.md) v Azure App Service pomocí nástrojů integrovaných v sadě Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="30d43-107">How toocreate and deploy [API apps](app-service-api-apps-why-best-platform.md) in Azure App Service by using tools built into Visual Studio 2015.</span></span>
* <span data-ttu-id="30d43-108">Jak balíčku Swashbuckle NuGet zjišťování tooautomate rozhraní API pomocí hello toodynamically generování metadat Swagger rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="30d43-108">How tooautomate API discovery by using hello Swashbuckle NuGet package toodynamically generate Swagger API metadata.</span></span>
* <span data-ttu-id="30d43-109">Jak tooautomatically metadat rozhraní API Swaggeru toouse generovat kód klienta pro aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="30d43-109">How toouse Swagger API metadata tooautomatically generate client code for an API app.</span></span>

## <a name="sample-application-overview"></a><span data-ttu-id="30d43-110">Přehled ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="30d43-110">Sample application overview</span></span>
<span data-ttu-id="30d43-111">V tomto návodu budeme pracovat s jednoduchou ukázkovou aplikací seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="30d43-111">In this tutorial, you work with a simple to-do list sample application.</span></span> <span data-ttu-id="30d43-112">Hello aplikace má front-end jednostránkové aplikace (SPA), rozhraní ASP.NET Web API střední vrstvy a rozhraní ASP.NET Web API datové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="30d43-112">hello application has a single-page application (SPA) front end, an ASP.NET Web API middle tier, and an ASP.NET Web API data tier.</span></span>

![Diagram ukázkové aplikace API](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

<span data-ttu-id="30d43-114">Zde je snímek obrazovky hello [AngularJS](https://angularjs.org/) front-endu.</span><span class="sxs-lookup"><span data-stu-id="30d43-114">Here's a screen shot of hello [AngularJS](https://angularjs.org/) front end.</span></span>

![Seznam toodo aplikací ukázkové aplikace API](./media/app-service-api-dotnet-get-started/todospa.png)

<span data-ttu-id="30d43-116">Hello řešení sady Visual Studio skládá ze tří projektů:</span><span class="sxs-lookup"><span data-stu-id="30d43-116">hello Visual Studio solution includes three projects:</span></span>

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* <span data-ttu-id="30d43-117">**ToDoListAngular** -hello front-end: JEDNOSTRÁNKOVÁ aplikace AngularJS, která volá střední vrstvy hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-117">**ToDoListAngular** - hello front end: an AngularJS SPA that calls hello middle tier.</span></span>
* <span data-ttu-id="30d43-118">**ToDoListAPI** -hello střední vrstva: projekt webového rozhraní API ASP.NET, který volá datovou vrstvu hello tooperform operace CRUD s položkami seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="30d43-118">**ToDoListAPI** - hello middle tier: an ASP.NET Web API project that calls hello data tier tooperform CRUD operations on to-do items.</span></span>
* <span data-ttu-id="30d43-119">**ToDoListDataAPI** -hello Datová vrstva: projekt webového rozhraní API ASP.NET, který provádí operace CRUD s položkami seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="30d43-119">**ToDoListDataAPI** - hello data tier:  an ASP.NET Web API project that performs CRUD operations on to-do items.</span></span>

<span data-ttu-id="30d43-120">Hello třívrstvá architektura je jednou z mnoha architektur, které můžete implementovat pomocí aplikací API a tady je použita pouze pro účely ukázky.</span><span class="sxs-lookup"><span data-stu-id="30d43-120">hello three-tier architecture is one of many architectures that you can implement by using API Apps and is used here only for demonstration purposes.</span></span> <span data-ttu-id="30d43-121">Hello kód v jednotlivých vrstvách je jednoduché, funkce API Apps možné toodemonstrate; například hello Datová vrstva používá serveru paměť, místo databáze jako svůj mechanismus trvalosti.</span><span class="sxs-lookup"><span data-stu-id="30d43-121">hello code in each tier is as simple as possible toodemonstrate API Apps features; for example, hello data tier uses server memory rather than a database as its persistence mechanism.</span></span>

<span data-ttu-id="30d43-122">Po dokončení tohoto kurzu, budete mít dva projekty webového rozhraní API hello nahoru a spuštění v cloudu hello v App Service API apps.</span><span class="sxs-lookup"><span data-stu-id="30d43-122">On completing this tutorial, you'll have hello two Web API projects up and running in hello cloud in App Service API apps.</span></span>

<span data-ttu-id="30d43-123">Hello další kurz v řadě hello nasadí hello SPA front-endu toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="30d43-123">hello next tutorial in hello series deploys hello SPA front end toohello cloud.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30d43-124">Požadavky</span><span class="sxs-lookup"><span data-stu-id="30d43-124">Prerequisites</span></span>
* <span data-ttu-id="30d43-125">Rozhraní ASP.NET Web API – hello kurz předpokládá základní znalosti o tom toowork s technologií ASP.NET [webovém rozhraní API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="30d43-125">ASP.NET Web API - hello tutorial instructions assume you have a basic knowledge of how toowork with ASP.NET [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) in Visual Studio.</span></span>
* <span data-ttu-id="30d43-126">Účet Azure – můžete si [zdarma otevřít účet Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) nebo [aktivovat výhody pro předplatitele nástroje Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="30d43-126">Azure account - You can [Open an Azure account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>
  
    <span data-ttu-id="30d43-127">Pokud chcete, aby tooget začít s Azure App Service, ještě než si zaregistrujete účet Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/).</span><span class="sxs-lookup"><span data-stu-id="30d43-127">If you want tooget started with Azure App Service before you sign up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/).</span></span> <span data-ttu-id="30d43-128">Tam si můžete v App Service hned vytvořit krátkodobou úvodní aplikaci – **nepožaduje se žádná platební karta**, ani to s sebou nenese žádné závazky.</span><span class="sxs-lookup"><span data-stu-id="30d43-128">There, you can immediately create a short-lived starter app in App Service — **no credit card required**, and no commitments.</span></span>
* <span data-ttu-id="30d43-129">Visual Studio 2015 se hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) -hello SDK nainstaluje Visual Studio 2015 automaticky, pokud ještě nemáte ho.</span><span class="sxs-lookup"><span data-stu-id="30d43-129">Visual Studio 2015 with hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) - hello SDK installs Visual Studio 2015 automatically if you don't already have it.</span></span>
  
  * <span data-ttu-id="30d43-130">V sadě Visual Studio klikněte na Nápověda -> O aplikaci Microsoft Visual Studio a ujistěte se, že máte Azure App Service Tools ve verzi 2.9.1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="30d43-130">In Visual Studio, click Help -> About Microsoft Visual Studio and ensure that you have "Azure App Service Tools v2.9.1" or higher installed.</span></span>
    
    ![Verze Azure App Tools](./media/app-service-api-dotnet-get-started/apiversion.png)
    
    > [!NOTE]
    > <span data-ttu-id="30d43-132">V závislosti na tom, kolik závislostí sady SDK hello už máte na počítači může instalaci hello sady SDK trvat dlouhou dobu, od několika minut tooa půl hodiny nebo i déle.</span><span class="sxs-lookup"><span data-stu-id="30d43-132">Depending on how many of hello SDK dependencies you already have on your machine, installing hello SDK could take a long time, from several minutes tooa half hour or more.</span></span>
    > 
    > 

## <a name="download-hello-sample-application"></a><span data-ttu-id="30d43-133">Stáhnout ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="30d43-133">Download hello sample application</span></span>
1. <span data-ttu-id="30d43-134">Stáhnout hello [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) úložiště.</span><span class="sxs-lookup"><span data-stu-id="30d43-134">Download hello [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) repository.</span></span>
   
    <span data-ttu-id="30d43-135">Můžete kliknout na hello **stáhnout ZIP** tlačítko nebo klonování hello úložiště na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="30d43-135">You can click hello **Download ZIP** button or clone hello repository on your local machine.</span></span>
2. <span data-ttu-id="30d43-136">Otevřete řešení seznamu úkolů hello v sadě Visual Studio 2015 nebo 2013.</span><span class="sxs-lookup"><span data-stu-id="30d43-136">Open hello ToDoList solution in Visual Studio 2015 or 2013.</span></span>
   
   1. <span data-ttu-id="30d43-137">Je nutné tootrust každé řešení.</span><span class="sxs-lookup"><span data-stu-id="30d43-137">You will need tootrust each solution.</span></span>
         <span data-ttu-id="30d43-138">![Upozornění zabezpečení](./media/app-service-api-dotnet-get-started/securitywarning.png)</span><span class="sxs-lookup"><span data-stu-id="30d43-138">![Security Warning](./media/app-service-api-dotnet-get-started/securitywarning.png)</span></span>
3. <span data-ttu-id="30d43-139">Vytvoření balíčků NuGet hello řešení (CTRL + SHIFT + B) toorestore hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-139">Build hello solution (CTRL + SHIFT + B)  toorestore hello NuGet packages.</span></span>
   
    <span data-ttu-id="30d43-140">Pokud chcete aplikace hello toosee v operaci před nasazením, můžete ji spustit místně.</span><span class="sxs-lookup"><span data-stu-id="30d43-140">If you want toosee hello application in operation before you deploy it, you can run it locally.</span></span> <span data-ttu-id="30d43-141">Ujistěte se, že je ToDoListDataAPI spouštěný projekt a spusťte hello řešení.</span><span class="sxs-lookup"><span data-stu-id="30d43-141">Make sure that ToDoListDataAPI is your startup project and run hello solution.</span></span> <span data-ttu-id="30d43-142">Chyba protokolu HTTP 403 toosee byste měli očekávat v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="30d43-142">You should expect toosee a HTTP 403 error in your browser.</span></span>

## <a name="use-swagger-api-metadata-and-ui"></a><span data-ttu-id="30d43-143">Používání uživatelského rozhraní a metadat rozhraní API Swaggeru</span><span class="sxs-lookup"><span data-stu-id="30d43-143">Use Swagger API metadata and UI</span></span>
<span data-ttu-id="30d43-144">Podpora metadat rozhraní API [Swaggeru](http://swagger.io/) 2.0 je integrována v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="30d43-144">Support for [Swagger](http://swagger.io/) 2.0 API metadata is built into Azure App Service.</span></span> <span data-ttu-id="30d43-145">Každá aplikace API můžete zadat adresu URL koncového bodu, který vrátí metadata pro hello rozhraní API ve formátu JSON pro Swagger.</span><span class="sxs-lookup"><span data-stu-id="30d43-145">Each API app can specify a URL endpoint that returns metadata for hello API in Swagger JSON format.</span></span> <span data-ttu-id="30d43-146">Hello metadata vrácená z tohoto koncového bodu může být použit toogenerate kód klienta.</span><span class="sxs-lookup"><span data-stu-id="30d43-146">hello metadata returned from that endpoint can be used toogenerate client code.</span></span>

<span data-ttu-id="30d43-147">Projekt webového rozhraní API ASP.NET může dynamicky generovat Swagger metadata pomocí hello [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="30d43-147">An ASP.NET Web API project can dynamically generate Swagger metadata by using hello [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet package.</span></span> <span data-ttu-id="30d43-148">Hello balíčku Swashbuckle NuGet již je nainstalována v projektech ToDoListDataAPI a ToDoListAPI hello, které jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="30d43-148">hello Swashbuckle NuGet package is already installed in hello ToDoListDataAPI and ToDoListAPI projects that you downloaded.</span></span>

<span data-ttu-id="30d43-149">V této části kurzu hello se podíváte na hello generované metadata Swagger 2.0 a vyzkoušíte si testovací uživatelské rozhraní, která je založená na hello Swagger metadata.</span><span class="sxs-lookup"><span data-stu-id="30d43-149">In this section of hello tutorial, you look at hello generated Swagger 2.0 metadata, and then you try out a test UI that is based on hello Swagger metadata.</span></span>

1. <span data-ttu-id="30d43-150">Nastavte projekt ToDoListDataAPI hello (**není** hello projekt ToDoListAPI) jako spouštěný projekt hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-150">Set hello ToDoListDataAPI project (**not** hello ToDoListAPI project) as hello startup project.</span></span>
   
    ![Nastavení ToDoDataAPI jako spouštěného projektu](./media/app-service-api-dotnet-get-started/startupproject.png)
2. <span data-ttu-id="30d43-152">Stisknutím klávesy F5 nebo klikněte na tlačítko **ladění > Spustit ladění** toorun hello projekt v režimu ladění.</span><span class="sxs-lookup"><span data-stu-id="30d43-152">Press F5 or click **Debug > Start Debugging** toorun hello project in debug mode.</span></span>
   
    <span data-ttu-id="30d43-153">Otevře se prohlížeč Hello a zobrazuje chybová stránka HTTP 403 hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-153">hello browser opens and shows hello HTTP 403 error page.</span></span>
3. <span data-ttu-id="30d43-154">V panelu Adresa v prohlížeči, přidejte `swagger/docs/v1` toohello konec hello řádku a stiskněte ENTER.</span><span class="sxs-lookup"><span data-stu-id="30d43-154">In your browser address bar, add `swagger/docs/v1` toohello end of hello line, and then press Return.</span></span> <span data-ttu-id="30d43-155">(adresa URL je hello `http://localhost:45914/swagger/docs/v1`.)</span><span class="sxs-lookup"><span data-stu-id="30d43-155">(hello URL is `http://localhost:45914/swagger/docs/v1`.)</span></span>
   
    <span data-ttu-id="30d43-156">Toto je výchozí adresa URL hello používá Swashbuckle tooreturn JSON pro Swagger 2.0 metadata pro hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="30d43-156">This is hello default URL used by Swashbuckle tooreturn Swagger 2.0 JSON metadata for hello API.</span></span>
   
    <span data-ttu-id="30d43-157">Pokud používáte Internet Explorer, hello v prohlížeči výzva toodownload *v1.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="30d43-157">If you're using Internet Explorer, hello browser prompts you toodownload a *v1.json* file.</span></span>
   
    ![Stažení metadat JSON do IE](./media/app-service-api-dotnet-get-started/iev1json.png)
   
    <span data-ttu-id="30d43-159">Pokud používáte Chrome, Firefox nebo Microsoft Edge, zobrazí prohlížeč hello hello JSON v okně prohlížeče hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-159">If you're using Chrome, Firefox, or Edge, hello browser displays hello JSON in hello browser window.</span></span> <span data-ttu-id="30d43-160">Různé prohlížeče zpracovávají data JSON různě a okno prohlížeče může vypadat třeba hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-160">Different browsers handle JSON differently, and your browser window may not look exactly like hello example.</span></span>
   
    ![Metadata JSON v Chromu](./media/app-service-api-dotnet-get-started/chromev1json.png)
   
    <span data-ttu-id="30d43-162">Hello následující příklad ukazuje první část hello hello metadata Swagger pro rozhraní API, hello s hello Definice hello získáte metoda.</span><span class="sxs-lookup"><span data-stu-id="30d43-162">hello following sample shows hello first section of hello Swagger metadata for hello API, with hello definition for hello Get method.</span></span> <span data-ttu-id="30d43-163">Tato metadata je, jaké jednotky hello uživatelské rozhraní Swagger, které použijete v hello následující kroky a můžete ji použít v další části kurzu tooautomatically hello generování kódu klienta.</span><span class="sxs-lookup"><span data-stu-id="30d43-163">This metadata is what drives hello Swagger UI that you use in hello following steps, and you use it in a later section of hello tutorial tooautomatically generate client code.</span></span>
   
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
4. <span data-ttu-id="30d43-164">Zavřete prohlížeč hello a zastavte ladění v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="30d43-164">Close hello browser and stop Visual Studio debugging.</span></span>
5. <span data-ttu-id="30d43-165">V hello ToDoListDataAPI projektu v **Průzkumníku řešení**, otevřete hello *App_Start\SwaggerConfig.cs* soubor a poté přejděte dolů tooline 174 a zrušte komentář u následující kód hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-165">In hello ToDoListDataAPI project in **Solution Explorer**, open hello *App_Start\SwaggerConfig.cs* file, then scroll down tooline 174 and uncomment hello following code.</span></span>
   
        /*
            })
        .EnableSwaggerUi(c =>
            {
        */
   
    <span data-ttu-id="30d43-166">Hello *SwaggerConfig.cs* soubor se vytvoří při instalaci balíčku Swashbuckle hello v projektu.</span><span class="sxs-lookup"><span data-stu-id="30d43-166">hello *SwaggerConfig.cs* file is created when you install hello Swashbuckle package in a project.</span></span> <span data-ttu-id="30d43-167">soubor Hello poskytuje několik způsobů tooconfigure Swashbuckle.</span><span class="sxs-lookup"><span data-stu-id="30d43-167">hello file provides a number of ways tooconfigure Swashbuckle.</span></span>
   
    <span data-ttu-id="30d43-168">Kód Hello jste odkomentovali hello umožňuje uživatelského rozhraní Swagger, které budete používat v hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="30d43-168">hello code you've uncommented enables hello Swagger UI that you use in hello following steps.</span></span> <span data-ttu-id="30d43-169">Když vytvoříte projekt webového rozhraní API pomocí šablony projektu aplikace hello rozhraní API, tento kód je označeno jako komentář ve výchozím nastavení jako bezpečnostní opatření.</span><span class="sxs-lookup"><span data-stu-id="30d43-169">When you create a Web API project by using hello API app project template, this code is commented out by default as a security measure.</span></span>
6. <span data-ttu-id="30d43-170">Znovu spusťte projekt hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-170">Run hello project again.</span></span>
7. <span data-ttu-id="30d43-171">V panelu Adresa v prohlížeči, přidejte `swagger` toohello konec hello řádku a stiskněte ENTER.</span><span class="sxs-lookup"><span data-stu-id="30d43-171">In your browser address bar, add `swagger` toohello end of hello line, and then press Return.</span></span> <span data-ttu-id="30d43-172">(adresa URL je hello `http://localhost:45914/swagger`.)</span><span class="sxs-lookup"><span data-stu-id="30d43-172">(hello URL is `http://localhost:45914/swagger`.)</span></span>
8. <span data-ttu-id="30d43-173">Jakmile se zobrazí stránka uživatelského rozhraní Swagger hello, klikněte na tlačítko **ToDoList** toosee hello metody dostupné.</span><span class="sxs-lookup"><span data-stu-id="30d43-173">When hello Swagger UI page appears, click **ToDoList** toosee hello methods available.</span></span>
   
    ![Dostupné metody uživatelského rozhraní Swagger](./media/app-service-api-dotnet-get-started/methods.png)
9. <span data-ttu-id="30d43-175">Klikněte na tlačítko hello nejprve **získat** tlačítko v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-175">Click hello first **Get** button in hello list.</span></span>
10. <span data-ttu-id="30d43-176">V hello **parametry** části, zadejte hvězdičku hello hodnotu hello `owner` parametr a pak klikněte na tlačítko **vyzkoušení**.</span><span class="sxs-lookup"><span data-stu-id="30d43-176">In hello **Parameters** section, enter an asterisk as hello value of hello `owner` parameter, and then click **Try it out**.</span></span>
    
    <span data-ttu-id="30d43-177">Když v dalších kurzech přidáte ověřování, hello střední vrstva poskytne hello skutečného uživatelského ID toohello datové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="30d43-177">When you add authentication in later tutorials, hello middle tier will provide hello actual user ID toohello data tier.</span></span> <span data-ttu-id="30d43-178">Teď všechny úlohy má hvězdičky ID vlastníka hello aplikace běží bez povolení ověřování.</span><span class="sxs-lookup"><span data-stu-id="30d43-178">For now, all tasks will have asterisk as their owner ID while hello application runs without authentication enabled.</span></span>
    
    ![Vyzkoušení uživatelského rozhraní Swagger](./media/app-service-api-dotnet-get-started/gettryitout1.png)
    
    <span data-ttu-id="30d43-180">Hello uživatelské rozhraní Swagger zavolá hello metodu Get seznamu úkolů a zobrazí kód odpovědi hello a výsledky JSON.</span><span class="sxs-lookup"><span data-stu-id="30d43-180">hello Swagger UI calls hello ToDoList Get method and displays hello response code and JSON results.</span></span>
    
    ![Vyzkoušení uživatelského rozhraní Swagger – výsledky](./media/app-service-api-dotnet-get-started/gettryitout.png)
11. <span data-ttu-id="30d43-182">Klikněte na tlačítko **Post**a potom klikněte na pole hello v části **schéma modelu**.</span><span class="sxs-lookup"><span data-stu-id="30d43-182">Click **Post**, and then click hello box under **Model Schema**.</span></span>
    
    <span data-ttu-id="30d43-183">Kliknutím na tlačítko hello model schématu prefills hello vstupní pole kde můžete určit hello hodnota parametru pro metodu Post hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-183">Clicking hello model schema prefills hello input box where you can specify hello parameter value for hello Post method.</span></span> <span data-ttu-id="30d43-184">(Pokud to nebude fungovat v aplikaci Internet Explorer, použijte jiný prohlížeč, nebo zadejte hodnotu parametru hello ručně v dalším kroku hello.)</span><span class="sxs-lookup"><span data-stu-id="30d43-184">(If this doesn't work in Internet Explorer, use a different browser or enter hello parameter value manually in hello next step.)</span></span>  
    
    ![Vyzkoušení uživatelského rozhraní Swagger – metoda Post](./media/app-service-api-dotnet-get-started/post.png)
12. <span data-ttu-id="30d43-186">Změna hello JSON v hello `todo` parametr vstupní pole tak, aby vypadal jako následující příklad hello, nebo nahraďte váš vlastní text popisu:</span><span class="sxs-lookup"><span data-stu-id="30d43-186">Change hello JSON in hello `todo` parameter input box so that it looks like hello following example, or substitute your own description text:</span></span>
    
        {
          "ID": 2,
          "Description": "buy hello dog a toy",
          "Owner": "*"
        }
13. <span data-ttu-id="30d43-187">Klikněte na **Try it out** (Vyzkoušet).</span><span class="sxs-lookup"><span data-stu-id="30d43-187">Click **Try it out**.</span></span>
    
    <span data-ttu-id="30d43-188">Hello rozhraní API seznamu úkolů vrátí kód odpovědi HTTP 204, který označuje úspěch.</span><span class="sxs-lookup"><span data-stu-id="30d43-188">hello ToDoList API returns an HTTP 204 response code that indicates success.</span></span>
14. <span data-ttu-id="30d43-189">Klikněte na tlačítko hello nejprve **získat** tlačítko a klikněte v této části hello stránku hello **vyzkoušení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="30d43-189">Click hello first **Get** button, and then in that section of hello page click hello **Try it out** button.</span></span>
    
    <span data-ttu-id="30d43-190">Hello odpověď metody Get teď obsahuje novou položku toodo hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-190">hello Get method response now includes hello new toodo item.</span></span>
15. <span data-ttu-id="30d43-191">Volitelné: Pokuste se také hello Put, Delete a získat ID metody.</span><span class="sxs-lookup"><span data-stu-id="30d43-191">Optional: Try also hello Put, Delete, and Get by ID methods.</span></span>
16. <span data-ttu-id="30d43-192">Zavřete prohlížeč hello a zastavte ladění v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="30d43-192">Close hello browser and stop Visual Studio debugging.</span></span>

<span data-ttu-id="30d43-193">Swashbuckle funguje s libovolným projektem webového rozhraní API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="30d43-193">Swashbuckle works with any ASP.NET Web API project.</span></span> <span data-ttu-id="30d43-194">Pokud chcete tooadd Swagger metadata generování tooan existující projekt, stačí nainstalujte balíček Swashbuckle hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-194">If you want tooadd Swagger metadata generation tooan existing project, just install hello Swashbuckle package.</span></span>

> [!NOTE]
> <span data-ttu-id="30d43-195">Metadata Swagger mají pro každou operaci s rozhraním API jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="30d43-195">Swagger metadata includes a unique ID for each API operation.</span></span> <span data-ttu-id="30d43-196">Ve výchozím nastavení může Swashbuckle generovat duplicitní ID operací Swagger pro metody kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="30d43-196">By default, Swashbuckle may generate duplicate Swagger operation IDs for your Web API controller methods.</span></span> <span data-ttu-id="30d43-197">K tomu dojde, pokud kontroler přetížil metody HTTP, například `Get()` nebo `Get(id)`.</span><span class="sxs-lookup"><span data-stu-id="30d43-197">This happens if your controller has overloaded HTTP methods, such as `Get()` and `Get(id)`.</span></span> <span data-ttu-id="30d43-198">Informace o tom, jak toohandle přetížení najdete v tématu [definice rozhraní API generovaných ve Swashbuckle přizpůsobit](app-service-api-dotnet-swashbuckle-customize.md).</span><span class="sxs-lookup"><span data-stu-id="30d43-198">For information about how toohandle overloads, see [Customize Swashbuckle-generated API definitions](app-service-api-dotnet-swashbuckle-customize.md).</span></span> <span data-ttu-id="30d43-199">Pokud vytvoříte projekt webového rozhraní API v sadě Visual Studio pomocí šablony hello aplikace Azure API, kód, který generuje jedinečné identifikátory operací se automaticky přidá toohello *SwaggerConfig.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="30d43-199">If you create a Web API project in Visual Studio by using hello Azure API App template, code that generates unique operation IDs is automatically added toohello *SwaggerConfig.cs* file.</span></span>  
> 
> 

## <span data-ttu-id="30d43-200"><a id="createapiapp"></a>Vytvoření aplikace API v Azure a nasadit tooit kódu</span><span class="sxs-lookup"><span data-stu-id="30d43-200"><a id="createapiapp"></a> Create an API app in Azure and deploy code tooit</span></span>
<span data-ttu-id="30d43-201">V této části použijte nástroje Azure, které jsou integrovány do hello Visual Studio **Publikovat Web** aplikaci průvodce toocreate nové rozhraní API v Azure.</span><span class="sxs-lookup"><span data-stu-id="30d43-201">In this section, you use Azure tools that are integrated into hello Visual Studio **Publish Web** wizard toocreate a new API app in Azure.</span></span> <span data-ttu-id="30d43-202">Potom nasaďte hello ToDoListDataAPI projektu toohello nové aplikace API a volání rozhraní API hello spuštěním hello uživatelské rozhraní Swagger.</span><span class="sxs-lookup"><span data-stu-id="30d43-202">Then you deploy hello ToDoListDataAPI project toohello new API app and call hello API by running hello Swagger UI.</span></span>

1. <span data-ttu-id="30d43-203">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ToDoListDataAPI hello a pak klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="30d43-203">In **Solution Explorer**, right-click hello ToDoListDataAPI project, and then click **Publish**.</span></span>
   
    ![Kliknutí na Publikovat v nástroji Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu.png)
2. <span data-ttu-id="30d43-205">V hello **profil** krok hello **Publikovat Web** průvodce, klikněte na tlačítko **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="30d43-205">In hello **Profile** step of hello **Publish Web** wizard, click **Microsoft Azure App Service**.</span></span>
   
   ![Kliknutí na Azure App Service v průvodci Publikovat web](./media/app-service-api-dotnet-get-started/selectappservice.png)
3. <span data-ttu-id="30d43-207">Přihlášení tooyour účet Azure, pokud jste tak již neučinili, nebo aktualizujte přihlašovací údaje, pokud jste v platnost vypršela.</span><span class="sxs-lookup"><span data-stu-id="30d43-207">Sign in tooyour Azure account if you have not already done so, or refresh your credentials if they're expired.</span></span>
4. <span data-ttu-id="30d43-208">V hello dialogové okno služby App Service, zvolte hello Azure **předplatné** toouse a pak klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="30d43-208">In hello App Service dialog box, choose hello Azure **Subscription** you want toouse, and then click **New**.</span></span>
   
    ![Kliknutí na Nová v dialogovém okně App Service](./media/app-service-api-dotnet-get-started/clicknew.png)
   
    <span data-ttu-id="30d43-210">Hello **hostitelský** kartě hello **vytvořit službu App Service** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="30d43-210">hello **Hosting** tab of hello **Create App Service** dialog box appears.</span></span>
   
    <span data-ttu-id="30d43-211">Vzhledem k tomu, že nasazujete projekt webového rozhraní API, který je nainstalován Swashbuckle, předpokládá Visual Studio, které chcete toocreate aplikace API.</span><span class="sxs-lookup"><span data-stu-id="30d43-211">Because you're deploying a Web API project that has Swashbuckle installed, Visual Studio assumes that you want toocreate an API App.</span></span> <span data-ttu-id="30d43-212">To vyplývá z hello **název aplikace API** title a podle hello fakt, že hello **změnit typ** rozevíracího seznamu je nastaven příliš**aplikace API**.</span><span class="sxs-lookup"><span data-stu-id="30d43-212">This is indicated by hello **API App Name** title and by hello fact that hello **Change Type** drop-down list is set too**API App**.</span></span>
   
    ![Typ aplikace v dialogovém okně App Service](./media/app-service-api-dotnet-get-started/apptype.png)
5. <span data-ttu-id="30d43-214">Zadejte **název aplikace API** který bude v hello *azurewebsites.net* domény.</span><span class="sxs-lookup"><span data-stu-id="30d43-214">Enter an **API App Name** that is unique in hello *azurewebsites.net* domain.</span></span> <span data-ttu-id="30d43-215">Můžete přijmout výchozí název hello, který Visual Studio navrhne.</span><span class="sxs-lookup"><span data-stu-id="30d43-215">You can accept hello default name that Visual Studio proposes.</span></span>
   
    <span data-ttu-id="30d43-216">Pokud zadáte název, který již použil někdo jiný, zobrazí se toohello vpravo červený vykřičník.</span><span class="sxs-lookup"><span data-stu-id="30d43-216">If you enter a name that someone else has already used, you see a red exclamation mark toohello right.</span></span>
   
    <span data-ttu-id="30d43-217">Hello hello rozhraní API aplikace bude mít adresu URL `{API app name}.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="30d43-217">hello URL of hello API app will be `{API app name}.azurewebsites.net`.</span></span>
6. <span data-ttu-id="30d43-218">V hello **skupiny prostředků** rozevíracího seznamu, klikněte na tlačítko **nový**a pak zadejte "ToDoListGroup" nebo jiný název, který preferujete.</span><span class="sxs-lookup"><span data-stu-id="30d43-218">In hello **Resource Group** drop-down, click **New**, and then enter "ToDoListGroup" or another name if you prefer.</span></span>
   
    <span data-ttu-id="30d43-219">Skupina prostředků je kolekce prostředků Azure, jako jsou aplikace API, databáze, virtuální počítače apod.</span><span class="sxs-lookup"><span data-stu-id="30d43-219">A resource group is a collection of Azure resources such as API apps, databases, VMs, and so forth.</span></span>    <span data-ttu-id="30d43-220">V tomto kurzu je nejlepší toocreate novou skupinu prostředků vzhledem k tomu, který umožňuje snadno toodelete v jednom kroku všechny prostředky Azure, které vytvoříte pro kurz hello hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-220">For this tutorial, it's best toocreate a new resource group because that makes it easy toodelete in one step all hello Azure resources that you create for hello tutorial.</span></span>
   
    <span data-ttu-id="30d43-221">V tomto poli můžete vybrat existující [skupinu prostředků](../azure-resource-manager/resource-group-overview.md). Můžete taky vytvořit novou tak, že zadáte název, který zatím nemá žádná existující skupina prostředků ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="30d43-221">This box lets you select an existing [resource group](../azure-resource-manager/resource-group-overview.md) or create a new one by typing in a name that is different from any existing resource group in your subscription.</span></span>
7. <span data-ttu-id="30d43-222">Klikněte na tlačítko hello **nový** tlačítko Další toohello **plán služby App Service** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="30d43-222">Click hello **New** button next toohello **App Service Plan** drop-down.</span></span>
   
    <span data-ttu-id="30d43-223">Hello snímek obrazovky ukazuje ukázkové hodnoty pro **název aplikace API**, **předplatné**, a **skupiny prostředků** – vaše hodnoty budou lišit.</span><span class="sxs-lookup"><span data-stu-id="30d43-223">hello screen shot shows sample values for **API App Name**, **Subscription**, and **Resource Group** -- your values will be different.</span></span>
   
    ![Dialogové okno Vytvořit službu App Service](./media/app-service-api-dotnet-get-started/createas.png)
   
    <span data-ttu-id="30d43-225">V hello postupu vytvoříte plán služby App Service pro novou skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-225">In hello following steps you create an App Service plan for hello new resource group.</span></span> <span data-ttu-id="30d43-226">Plán služby App Service určuje výpočetní prostředky hello, které vaše aplikace API poběží na.</span><span class="sxs-lookup"><span data-stu-id="30d43-226">An App Service plan specifies hello compute resources that your API app runs on.</span></span> <span data-ttu-id="30d43-227">Například pokud zvolíte hello úroveň free, vaše aplikace API poběží na sdílených virtuálních počítačích, zatímco u některých placených úrovní běží na vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="30d43-227">For example, if you choose hello free tier, your API app runs on shared VMs, while for some paid tiers it runs on dedicated VMs.</span></span> <span data-ttu-id="30d43-228">Informace o plánech služby App Service naleznete v části [Přehled plánů služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="30d43-228">For information about App Service plans, see [App Service plans overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
8. <span data-ttu-id="30d43-229">V hello **nakonfigurovat plán služby App Service** dialogové okno, zadejte "ToDoListPlan" nebo jiný název, který preferujete.</span><span class="sxs-lookup"><span data-stu-id="30d43-229">In hello **Configure App Service Plan** dialog, enter "ToDoListPlan" or another name if you prefer.</span></span>
9. <span data-ttu-id="30d43-230">V hello **umístění** rozevíracího seznamu vyberte hello umístění, které je nejblíže tooyou.</span><span class="sxs-lookup"><span data-stu-id="30d43-230">In hello **Location** drop-down list, choose hello location that is closest tooyou.</span></span>
   
    <span data-ttu-id="30d43-231">Toto nastavení určuje, ve kterém datacentru Azure vaše aplikace poběží.</span><span class="sxs-lookup"><span data-stu-id="30d43-231">This setting specifies which Azure datacenter your app will run in.</span></span> <span data-ttu-id="30d43-232">Vyberte umístění zavřete tooyou toominimize [latence](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).</span><span class="sxs-lookup"><span data-stu-id="30d43-232">Choose a location close tooyou toominimize [latency](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).</span></span>
10. <span data-ttu-id="30d43-233">V hello **velikost** rozevíracího seznamu, klikněte na tlačítko **volné**.</span><span class="sxs-lookup"><span data-stu-id="30d43-233">In hello **Size** drop-down, click **Free**.</span></span>
    
    <span data-ttu-id="30d43-234">V tomto kurzu zajistí hello volné cenová úroveň dostatečný výkon.</span><span class="sxs-lookup"><span data-stu-id="30d43-234">For this tutorial, hello free pricing tier will provide sufficient performance.</span></span>
11. <span data-ttu-id="30d43-235">V hello **nakonfigurovat plán služby App Service** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="30d43-235">In hello **Configure App Service Plan** dialog, click **OK**.</span></span>
    
    ![Kliknutí na OK v dialogovém okně Nakonfigurovat plán služby App Service](./media/app-service-api-dotnet-get-started/configasp.png)
12. <span data-ttu-id="30d43-237">V hello **vytvořit službu App Service** dialogové okno, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="30d43-237">In hello **Create App Service** dialog box, click **Create**.</span></span>
    
    ![Kliknutí na Vytvořit v dialogovém okně Vytvořit službu App Service](./media/app-service-api-dotnet-get-started/clickcreate.png)
    
    <span data-ttu-id="30d43-239">Visual Studio vytvoří aplikaci API hello a profil publikování, který má všechna hello požadované nastavení pro aplikace hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="30d43-239">Visual Studio creates hello API app and a publish profile that has all of hello required settings for hello API app.</span></span> <span data-ttu-id="30d43-240">Potom otevře hello **Publikovat Web** průvodce, který budete používat toodeploy hello projektu.</span><span class="sxs-lookup"><span data-stu-id="30d43-240">Then it opens hello **Publish Web** wizard, which you'll use toodeploy hello project.</span></span>
    
    <span data-ttu-id="30d43-241">Hello **Publikovat Web** otevře se Průvodce na hello **připojení** (viz následující obrázek).</span><span class="sxs-lookup"><span data-stu-id="30d43-241">hello **Publish Web** wizard opens on hello **Connection** tab (shown below).</span></span>
    
    <span data-ttu-id="30d43-242">Na hello **připojení** kartě hello **Server** a **název lokality** aplikace API bodu tooyour nastavení.</span><span class="sxs-lookup"><span data-stu-id="30d43-242">On hello **Connection** tab, hello **Server** and **Site name** settings point tooyour API app.</span></span> <span data-ttu-id="30d43-243">Hello **uživatelské jméno** a **heslo** jsou přihlašovací údaje nasazení, které pro vás vytvoří Azure.</span><span class="sxs-lookup"><span data-stu-id="30d43-243">hello **User name** and **Password** are deployment credentials that Azure creates for you.</span></span> <span data-ttu-id="30d43-244">Po nasazení otevře Visual Studio prohlížeče toohello **cílová adresa URL** (který je jediným účelem hello **cílová adresa URL**).</span><span class="sxs-lookup"><span data-stu-id="30d43-244">After deployment, Visual Studio opens a browser toohello **Destination URL** (that's hello only purpose for **Destination URL**).</span></span>  
13. <span data-ttu-id="30d43-245">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="30d43-245">Click **Next**.</span></span>
    
    ![Kliknutí na Další na kartě Připojení průvodce Publikovat web](./media/app-service-api-dotnet-get-started/connnext.png)
    
    <span data-ttu-id="30d43-247">Další karta Hello je hello **nastavení** (viz následující obrázek).</span><span class="sxs-lookup"><span data-stu-id="30d43-247">hello next tab is hello **Settings** tab (shown below).</span></span> <span data-ttu-id="30d43-248">Zde můžete změnit hello sestavení konfigurace karta toodeploy sestavení pro ladění [vzdálené ladění](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).</span><span class="sxs-lookup"><span data-stu-id="30d43-248">Here you can change hello build configuration tab toodeploy a debug build for [remote debugging](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).</span></span> <span data-ttu-id="30d43-249">Hello kartě je také několik **možností publikování souboru**:</span><span class="sxs-lookup"><span data-stu-id="30d43-249">hello tab also offers several **File Publish Options**:</span></span>
    
    * <span data-ttu-id="30d43-250">Odebrat další soubory v cíli</span><span class="sxs-lookup"><span data-stu-id="30d43-250">Remove additional files at destination</span></span>
    * <span data-ttu-id="30d43-251">Předkompilovat během publikování</span><span class="sxs-lookup"><span data-stu-id="30d43-251">Precompile during publishing</span></span>
    * <span data-ttu-id="30d43-252">Vyloučit soubory ze složky App_Data hello</span><span class="sxs-lookup"><span data-stu-id="30d43-252">Exclude files from hello App_Data folder</span></span>
    
    <span data-ttu-id="30d43-253">V tomto kurzu není potřeba vybírat žádnou z nich.</span><span class="sxs-lookup"><span data-stu-id="30d43-253">For this tutorial you don't need any of these.</span></span> <span data-ttu-id="30d43-254">Podrobné vysvětlení, co tyto možnosti představují, najdete v části [Postupy: Nasazení webového projektu pomocí publikování jedním kliknutím v nástroji Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).</span><span class="sxs-lookup"><span data-stu-id="30d43-254">For detailed explanations of what they do, see [How to: Deploy a Web Project Using One-Click Publish in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).</span></span>
14. <span data-ttu-id="30d43-255">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="30d43-255">Click **Next**.</span></span>
    
    ![Kliknutí na Další na kartě Nastavení průvodce Publikovat web](./media/app-service-api-dotnet-get-started/settingsnext.png)
    
    <span data-ttu-id="30d43-257">Dále je hello **Preview** kartu (viz následující obrázek), která umožňuje toosee příležitost, které soubory budou zkopírovány z projektu aplikace toohello rozhraní API toobe.</span><span class="sxs-lookup"><span data-stu-id="30d43-257">Next is hello **Preview** tab (shown below), which gives you an opportunity toosee what files are going toobe copied from your project toohello API app.</span></span> <span data-ttu-id="30d43-258">Pokud nasazujete aplikace na projektu tooan rozhraní API už nasazená tooearlier, zkopírují se pouze změněné soubory.</span><span class="sxs-lookup"><span data-stu-id="30d43-258">When you're deploying a project tooan API app that you already deployed tooearlier, only changed files are copied.</span></span> <span data-ttu-id="30d43-259">Pokud chcete toosee seznam kopírovaného, můžete kliknout na hello **spustit Náhled** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="30d43-259">If you want toosee a list of what will be copied, you can click hello **Start Preview** button.</span></span>
15. <span data-ttu-id="30d43-260">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="30d43-260">Click **Publish**.</span></span>
    
    ![Kliknutí na Publikovat na kartě Náhled v průvodci Publikovat web](./media/app-service-api-dotnet-get-started/clickpublish.png)
    
    <span data-ttu-id="30d43-262">Visual Studio nasadí hello ToDoListDataAPI projektu toohello nové aplikace API.</span><span class="sxs-lookup"><span data-stu-id="30d43-262">Visual Studio deploys hello ToDoListDataAPI project toohello new API app.</span></span> <span data-ttu-id="30d43-263">Hello **výstup** zaprotokoluje úspěšné nasazení a v adrese URL prohlížeče otevřené okno toohello hello rozhraní API aplikace se zobrazí stránka s "byla úspěšně vytvořena.".</span><span class="sxs-lookup"><span data-stu-id="30d43-263">hello **Output** window logs successful deployment, and a "successfully created" page appears in a browser window opened toohello URL of hello API app.</span></span>
    
    ![Okno výstupu – úspěšné nasazení](./media/app-service-api-dotnet-get-started/deploymentoutput.png)
    
    ![Stránka úspěšného vytvoření nové aplikace API](./media/app-service-api-dotnet-get-started/appcreated.png)
16. <span data-ttu-id="30d43-266">Přidejte "swagger" toohello adresu URL v adresním řádku prohlížeče hello a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="30d43-266">Add "swagger" toohello URL in hello browser's address bar, and then press Enter.</span></span> <span data-ttu-id="30d43-267">(adresa URL je hello `http://{apiappname}.azurewebsites.net/swagger`.)</span><span class="sxs-lookup"><span data-stu-id="30d43-267">(hello URL is `http://{apiappname}.azurewebsites.net/swagger`.)</span></span>
    
    <span data-ttu-id="30d43-268">Hello prohlížeč zobrazí stejné uživatelské rozhraní Swagger, které jste předtím viděli, ale teď běží v cloudu hello hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-268">hello browser displays hello same Swagger UI that you saw earlier, but now it's running in hello cloud.</span></span> <span data-ttu-id="30d43-269">Vyzkoušejte hello metodu Get a zjistíte, že jste zpět toohello 2 výchozích položek.</span><span class="sxs-lookup"><span data-stu-id="30d43-269">Try out hello Get method, and you see that you're back toohello default 2 to-do items.</span></span> <span data-ttu-id="30d43-270">Hello změny, které jste provedli dříve byly uloženy v paměti v místním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-270">hello changes you made earlier were saved in memory in hello local machine.</span></span>
17. <span data-ttu-id="30d43-271">Otevřete hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="30d43-271">Open hello [Azure portal](https://portal.azure.com/).</span></span>
    
    <span data-ttu-id="30d43-272">Hello portál Azure je webové rozhraní pro správu prostředků Azure, jako je například aplikace API.</span><span class="sxs-lookup"><span data-stu-id="30d43-272">hello Azure portal is a web interface for managing Azure resources such as API apps.</span></span>
18. <span data-ttu-id="30d43-273">Klikněte na **Další služby > App Services**.</span><span class="sxs-lookup"><span data-stu-id="30d43-273">Click **More Services > App Services**.</span></span>
    
    ![Procházení služeb App Services](./media/app-service-api-dotnet-get-started/browseas.png)
19. <span data-ttu-id="30d43-275">V hello **App Services** okno, vyhledejte a vyberte novou aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="30d43-275">In hello **App Services** blade, find and click your new API app.</span></span> <span data-ttu-id="30d43-276">(V hello portálu Azure, se nazývají windows, které se otevřou vpravo toohello *okna*.)</span><span class="sxs-lookup"><span data-stu-id="30d43-276">(In hello Azure portal, windows that open toohello right are called *blades*.)</span></span>
    
    ![Okno App Services](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)
    
    <span data-ttu-id="30d43-278">Otevřou se dvě okna.</span><span class="sxs-lookup"><span data-stu-id="30d43-278">Two blades open.</span></span> <span data-ttu-id="30d43-279">V jednom okně je přehled aplikace API hello a ve druhém dlouhý seznam nastavení, které můžete zobrazit a změnit.</span><span class="sxs-lookup"><span data-stu-id="30d43-279">One blade has an overview of hello API app, and one has a long list of settings that you can view and change.</span></span>
20. <span data-ttu-id="30d43-280">V hello **nastavení** okno, najít hello **rozhraní API** části a klikněte na tlačítko **definice rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="30d43-280">In hello **Settings** blade, find hello **API** section and click **API Definition**.</span></span>
    
    ![Definice rozhraní API v okně Nastavení](./media/app-service-api-dotnet-get-started/apidefinsettings.png)
    
    <span data-ttu-id="30d43-282">Hello **definice rozhraní API** okno umožňuje určit hello URL, která vrací metadata Swagger 2.0 ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="30d43-282">hello **API Definition** blade lets you specify hello URL that returns Swagger 2.0 metadata in JSON format.</span></span> <span data-ttu-id="30d43-283">Když Visual Studio vytvoří aplikaci hello rozhraní API, nastaví výchozí hodnota adresy URL toohello hello rozhraní API definice pro metadata generovaná ve Swashbuckle, jste viděli dříve, což je aplikace hello rozhraní API základní adresa URL plus `/swagger/docs/v1`.</span><span class="sxs-lookup"><span data-stu-id="30d43-283">When Visual Studio creates hello API app, it sets hello API definition URL toohello default value for Swashbuckle-generated metadata that you saw earlier, which is hello API app's base URL plus `/swagger/docs/v1`.</span></span>
    
    ![Adresa URL definice rozhraní API](./media/app-service-api-dotnet-get-started/apidefurl.png)
    
    <span data-ttu-id="30d43-285">Když vyberete kódu toogenerate klienta aplikace API pro něj, načte Visual Studio hello metadata z této adresy URL.</span><span class="sxs-lookup"><span data-stu-id="30d43-285">When you select an API app toogenerate client code for it, Visual Studio retrieves hello metadata from this URL.</span></span>

## <span data-ttu-id="30d43-286"><a id="codegen"></a>Generování kódu klienta pro hello datové vrstvy</span><span class="sxs-lookup"><span data-stu-id="30d43-286"><a id="codegen"></a> Generate client code for hello data tier</span></span>
<span data-ttu-id="30d43-287">Jednou z výhod hello integraci Swagger do aplikace Azure API je automatické generování kódu.</span><span class="sxs-lookup"><span data-stu-id="30d43-287">One of hello advantages of integrating Swagger into Azure API apps is automatic code generation.</span></span> <span data-ttu-id="30d43-288">Vygenerované třídy klienta umožňují snadnější toowrite kód, který volá aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="30d43-288">Generated client classes make it easier toowrite code that calls an API app.</span></span>

<span data-ttu-id="30d43-289">Projekt ToDoListAPI Hello již hello vygenerovat kód klienta, ale v hello následující kroky vám ho odstraníme a znovu vygenerovat toosee jak toodo hello generování kódu.</span><span class="sxs-lookup"><span data-stu-id="30d43-289">hello ToDoListAPI project already has hello generated client code, but in hello following steps you'll delete it and regenerate it toosee how toodo hello code generation.</span></span>

1. <span data-ttu-id="30d43-290">V sadě Visual Studio **Průzkumníku řešení**, v hello ToDoListAPI projektu, odstraňovat hello *ToDoListDataAPI* složky.</span><span class="sxs-lookup"><span data-stu-id="30d43-290">In Visual Studio **Solution Explorer**, in hello ToDoListAPI project, delete hello *ToDoListDataAPI* folder.</span></span> <span data-ttu-id="30d43-291">**Upozornění: Odstraňte jenom hello složku, ne projekt ToDoListDataAPI hello.**</span><span class="sxs-lookup"><span data-stu-id="30d43-291">**Caution: Delete only hello folder, not hello ToDoListDataAPI project.**</span></span>
   
    ![Odstranění vygenerovaného kódu klienta](./media/app-service-api-dotnet-get-started/deletecodegen.png)
   
    <span data-ttu-id="30d43-293">Tato složka byla vytvořena pomocí hello kódu generování proces, který jste o toogo prostřednictvím.</span><span class="sxs-lookup"><span data-stu-id="30d43-293">This folder was created by using hello code generation process that you're about toogo through.</span></span>
2. <span data-ttu-id="30d43-294">Klikněte pravým tlačítkem na projekt ToDoListAPI hello a pak klikněte na **Přidat > klient REST API**.</span><span class="sxs-lookup"><span data-stu-id="30d43-294">Right-click hello ToDoListAPI project, and then click **Add > REST API Client**.</span></span>
   
    ![Přidání klienta REST API v nástroji Visual Studio](./media/app-service-api-dotnet-get-started/codegenmenu.png)
3. <span data-ttu-id="30d43-296">V hello **přidat klienta REST API** dialogové okno, klikněte na tlačítko **adresa URL Swaggeru**a potom klikněte na **vybrat prostředek Azure**.</span><span class="sxs-lookup"><span data-stu-id="30d43-296">In hello **Add REST API Client** dialog box, click **Swagger URL**, and then click **Select Azure Asset**.</span></span>
   
    ![Výběr prostředku Azure](./media/app-service-api-dotnet-get-started/codegenbrowse.png)
4. <span data-ttu-id="30d43-298">V hello **služby App Service** dialogové okno, rozbalte skupinu prostředků hello používáte v tomto kurzu vyberte svou aplikaci API a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="30d43-298">In hello **App Service** dialog box, expand hello resource group you're using for this tutorial and select your API app, and then click **OK**.</span></span>
   
    ![Výběr aplikace API pro generování kódu](./media/app-service-api-dotnet-get-started/codegenselect.png)
   
    <span data-ttu-id="30d43-300">Všimněte si, že při návratu toohello **přidat klienta REST API** dialogové okno, hello textového pole doplnila hello rozhraní API definice hodnota adresy URL, kterou jste viděli dříve portálu hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-300">Notice that when you return toohello **Add REST API Client** dialog, hello text box has been filled in with hello API definition URL value that you saw earlier in hello portal.</span></span>
   
    ![Adresa URL definice rozhraní API](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)
   
   > [!TIP]
   > <span data-ttu-id="30d43-302">Alternativní způsob tooget metadat pro generování kódu je adresa URL hello tooenter přímo místo dialogové okno Procházet hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-302">An alternative way tooget metadata for code generation is tooenter hello URL directly instead of going through hello browse dialog.</span></span> <span data-ttu-id="30d43-303">Pokud chcete kód klienta toogenerate před nasazením tooAzure, můžete spustit projekt webového rozhraní API hello místně, přejít toohello URL, která poskytuje soubor dat JSON pro Swagger hello, uložte soubor hello, a použijte hello **vyberte existující soubor metadat Swagger**možnost.</span><span class="sxs-lookup"><span data-stu-id="30d43-303">Or if you want toogenerate client code before deploying tooAzure, you could run hello Web API project locally, go toohello URL that provides hello Swagger JSON file, save hello file, and use hello **Select an existing Swagger metadata file** option.</span></span>
   > 
   > 
5. <span data-ttu-id="30d43-304">V hello **přidat klienta REST API** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="30d43-304">In hello **Add REST API Client** dialog box, click **OK**.</span></span>
   
    <span data-ttu-id="30d43-305">Visual Studio vytvoří složku pojmenovanou aplikace hello rozhraní API a vygeneruje třídy klienta.</span><span class="sxs-lookup"><span data-stu-id="30d43-305">Visual Studio creates a folder named after hello API app and generates client classes.</span></span>
   
    ![Soubory kódu pro generovaného klienta](./media/app-service-api-dotnet-get-started/codegenfiles.png)
6. <span data-ttu-id="30d43-307">Otevřete v projektu ToDoListAPI hello *Controllers\ToDoListController.cs* toosee hello kódu na řádku 40, který volá rozhraní API hello pomocí hello generované klienta.</span><span class="sxs-lookup"><span data-stu-id="30d43-307">In hello ToDoListAPI project, open *Controllers\ToDoListController.cs* toosee hello code at line 40  that calls hello API by using hello generated client.</span></span>
   
    <span data-ttu-id="30d43-308">Hello následující fragment kódu ukazuje, jak hello kód vytvoří objekt hello klienta a volání hello metodu Get.</span><span class="sxs-lookup"><span data-stu-id="30d43-308">hello following snippet shows how hello code instantiates hello client object and calls hello Get method.</span></span>
   
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
   
    <span data-ttu-id="30d43-309">Hello parametr konstruktoru získá adresu URL koncového bodu hello z hello `toDoListDataAPIURL` nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="30d43-309">hello constructor parameter gets hello endpoint URL from  hello `toDoListDataAPIURL` app setting.</span></span> <span data-ttu-id="30d43-310">V souboru Web.config hello že hodnota je sada toohello místní služby IIS Express URL hello API projekt tak, aby aplikace hello můžete spustit místně.</span><span class="sxs-lookup"><span data-stu-id="30d43-310">In hello Web.config file, that value is set toohello local IIS Express URL of hello API project so that you can run hello application locally.</span></span> <span data-ttu-id="30d43-311">Pokud vynecháte parametr konstruktoru hello, je výchozí koncový bod hello hello adresu URL, který jste vygenerovali hello kód z.</span><span class="sxs-lookup"><span data-stu-id="30d43-311">If you omit hello constructor parameter, hello default endpoint is hello URL that you generated hello code from.</span></span>
7. <span data-ttu-id="30d43-312">Třída klienta se vygeneruje s jiným názvem podle názvu vaší aplikace API; změnit kód hello v *Controllers\ToDoListController.cs* tak, aby hello název typu odpovídal co bylo vygenerováno v projektu.</span><span class="sxs-lookup"><span data-stu-id="30d43-312">Your client class will be generated with a different name based on your API app name; change hello code in *Controllers\ToDoListController.cs* so that hello type name matches what was generated in your project.</span></span> <span data-ttu-id="30d43-313">Pokud jste například aplikaci API nazvali ToDoListDataAPI071316, pak byste tento kód:</span><span class="sxs-lookup"><span data-stu-id="30d43-313">For example, if you named your API App ToDoListDataAPI071316, you would change this code:</span></span>
   
        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

<span data-ttu-id="30d43-314">toothis:</span><span class="sxs-lookup"><span data-stu-id="30d43-314">toothis:</span></span>

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-toohost-hello-middle-tier"></a><span data-ttu-id="30d43-315">Vytvoření rozhraní API aplikace toohost hello střední vrstva</span><span class="sxs-lookup"><span data-stu-id="30d43-315">Create an API app toohost hello middle tier</span></span>
<span data-ttu-id="30d43-316">Dříve jste [aplikaci API datové vrstvy hello vytvoření a nasazení kódu tooit](#createapiapp).</span><span class="sxs-lookup"><span data-stu-id="30d43-316">Earlier you [created hello data tier API app and deployed code tooit](#createapiapp).</span></span>  <span data-ttu-id="30d43-317">Nyní budete postupovat podle hello stejný postup pro aplikaci API střední vrstvy hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-317">Now you follow hello same procedure for hello middle tier API app.</span></span>

1. <span data-ttu-id="30d43-318">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello střední vrstvy ToDoListAPI projektu (ne hello datovou vrstvu ToDoListDataAPI) a potom klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="30d43-318">In **Solution Explorer**, right-click hello middle tier ToDoListAPI  project (not hello data tier ToDoListDataAPI), and then click **Publish**.</span></span>
   
    ![Kliknutí na Publikovat v nástroji Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu2.png)
2. <span data-ttu-id="30d43-320">V hello **profil** kartě hello **Publikovat Web** průvodce, klikněte na tlačítko **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="30d43-320">In hello **Profile** tab of hello **Publish Web** wizard, click **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="30d43-321">V hello **služby App Service** dialogové okno, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="30d43-321">In hello **App Service** dialog box, click **New**.</span></span>
4. <span data-ttu-id="30d43-322">V hello **hostitelský** kartě hello **vytvořit službu App Service** dialogové okno pole, přijměte výchozí hello **název aplikace API** nebo zadejte název, který je v hello jedinečný  *azurewebsites.NET* domény.</span><span class="sxs-lookup"><span data-stu-id="30d43-322">In hello **Hosting** tab of hello **Create App Service** dialog box, accept hello default **API App Name** or enter a name that is unique in hello *azurewebsites.net* domain.</span></span>
5. <span data-ttu-id="30d43-323">Zvolte hello Azure **předplatné** používáte.</span><span class="sxs-lookup"><span data-stu-id="30d43-323">Choose hello Azure **Subscription** you have been using.</span></span>
6. <span data-ttu-id="30d43-324">V hello **skupiny prostředků** rozevíracího seznamu, vyberte hello stejnou skupinu prostředků, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="30d43-324">In hello **Resource Group** drop-down, choose hello same resource group you created earlier.</span></span>
7. <span data-ttu-id="30d43-325">V hello **plán služby App Service** rozevíracího seznamu, vyberte hello stejný plán, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="30d43-325">In hello **App Service Plan** drop-down, choose hello same plan you created earlier.</span></span> <span data-ttu-id="30d43-326">Bude použita výchozí hodnota toothat.</span><span class="sxs-lookup"><span data-stu-id="30d43-326">It will default toothat value.</span></span>
8. <span data-ttu-id="30d43-327">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="30d43-327">Click **Create**.</span></span>
   
    <span data-ttu-id="30d43-328">Visual Studio vytvoří aplikaci hello rozhraní API, vytvoří pro ni profil publikování a zobrazí hello **připojení** krok hello **Publikovat Web** průvodce.</span><span class="sxs-lookup"><span data-stu-id="30d43-328">Visual Studio creates hello API app, creates a publish profile for it, and displays hello **Connection** step of hello **Publish Web** wizard.</span></span>
9. <span data-ttu-id="30d43-329">V hello **připojení** krok hello **Publikovat Web** průvodce, klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="30d43-329">In hello **Connection** step of hello **Publish Web** wizard, click **Publish**.</span></span>
   
   <span data-ttu-id="30d43-330">Visual Studio nasadí hello ToDoListAPI projektu toohello nové aplikace API a otevře prohlížeč toohello URL aplikace API hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-330">Visual Studio deploys hello ToDoListAPI project toohello new API app and opens a browser toohello URL of hello API app.</span></span> <span data-ttu-id="30d43-331">Zobrazí se stránka Hello "úspěšně vytvořen".</span><span class="sxs-lookup"><span data-stu-id="30d43-331">hello "successfully created" page appears.</span></span>

## <a name="configure-hello-middle-tier-toocall-hello-data-tier"></a><span data-ttu-id="30d43-332">Konfigurace hello střední vrstvy toocall hello datové vrstvy</span><span class="sxs-lookup"><span data-stu-id="30d43-332">Configure hello middle tier toocall hello data tier</span></span>
<span data-ttu-id="30d43-333">Pokud jste nyní volá aplikaci API střední vrstvy hello, pokusí se toocall hello datovou vrstvu pomocí hello adresu URL místního hostitele, která je stále v souboru Web.config hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-333">If you called hello middle tier API app now, it would try toocall hello data tier using hello localhost URL that is still in hello Web.config file.</span></span> <span data-ttu-id="30d43-334">V této části zadejte hello adresu URL aplikace rozhraní API datové vrstvy do nastavení prostředí v aplikaci API střední vrstvy hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-334">In this section you enter hello data tier API app URL into an environment setting in hello middle tier API app.</span></span> <span data-ttu-id="30d43-335">Když hello kódu v aplikaci API střední vrstvy hello načte nastavení adresy URL aplikace hello data vrstvy, přepíše nastavení prostředí hello, co je v souboru Web.config hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-335">When hello code in hello middle tier API app retrieves hello data tier URL setting, hello environment setting will override what's in hello Web.config file.</span></span>

1. <span data-ttu-id="30d43-336">Přejděte toohello [portál Azure](https://portal.azure.com/)a potom přejděte toohello **aplikace API** hello rozhraní API aplikace, které jste vytvořili projekt TodoListAPI (střední úroveň) toohost hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-336">Go toohello [Azure portal](https://portal.azure.com/), and then navigate toohello **API App** blade for hello API app that you created toohost hello TodoListAPI (middle tier) project.</span></span>
2. <span data-ttu-id="30d43-337">V hello aplikace API **nastavení** okně klikněte na tlačítko **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="30d43-337">In hello API App's **Settings** blade, click **Application settings**.</span></span>
3. <span data-ttu-id="30d43-338">V hello aplikace API **nastavení aplikace** okně projděte dolů toohello **nastavení aplikace** a přidejte hello následující klíč a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="30d43-338">In hello API App's **Application Settings** blade, scroll down toohello **App settings** section and add hello following key and value.</span></span> <span data-ttu-id="30d43-339">Hodnota Hello bude hello URL hello prvního rozhraní API aplikace, které jste publikovali v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="30d43-339">hello value will be hello URL of hello first API App you published in this tutorial.</span></span>
   
   | <span data-ttu-id="30d43-340">**Klíč**</span><span class="sxs-lookup"><span data-stu-id="30d43-340">**Key**</span></span> | <span data-ttu-id="30d43-341">toDoListDataAPIURL</span><span class="sxs-lookup"><span data-stu-id="30d43-341">toDoListDataAPIURL</span></span> |
   | --- | --- |
   | <span data-ttu-id="30d43-342">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="30d43-342">**Value**</span></span> |<span data-ttu-id="30d43-343">https://{název vaší aplikace API datové vrstvy}.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="30d43-343">https://{your data tier API app name}.azurewebsites.net</span></span> |
   | <span data-ttu-id="30d43-344">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="30d43-344">**Example**</span></span> |<span data-ttu-id="30d43-345">https://todolistdataapi.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="30d43-345">https://todolistdataapi.azurewebsites.net</span></span> |
4. <span data-ttu-id="30d43-346">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="30d43-346">Click **Save**.</span></span>
   
    ![Kliknutí na Uložit v Nastavení aplikace](./media/app-service-api-dotnet-get-started/asinportal.png)
   
    <span data-ttu-id="30d43-348">Při spuštění hello kódu v Azure, tato hodnota přepíše adresu URL hello místního hostitele, který je v souboru Web.config hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-348">When hello code runs in Azure, this value will now override hello localhost URL that is in hello Web.config file.</span></span>

## <a name="test"></a><span data-ttu-id="30d43-349">Test</span><span class="sxs-lookup"><span data-stu-id="30d43-349">Test</span></span>
1. <span data-ttu-id="30d43-350">V okně prohlížeče procházet toohello URL hello novou aplikaci API střední vrstvy, který jste právě vytvořili pro projekt ToDoListAPI.</span><span class="sxs-lookup"><span data-stu-id="30d43-350">In a browser window, browse toohello URL of hello new middle tier API app that you just created for ToDoListAPI.</span></span> <span data-ttu-id="30d43-351">Lze tak učinit klepnutím hello adresu URL v hlavním okně aplikace hello rozhraní API portálu hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-351">You can get there by clicking hello URL in hello API app's main blade in hello portal.</span></span>
2. <span data-ttu-id="30d43-352">Přidejte "swagger" toohello adresu URL v adresním řádku prohlížeče hello a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="30d43-352">Add "swagger" toohello URL in hello browser's address bar, and then press Enter.</span></span> <span data-ttu-id="30d43-353">(adresa URL je hello `http://{apiappname}.azurewebsites.net/swagger`.)</span><span class="sxs-lookup"><span data-stu-id="30d43-353">(hello URL is `http://{apiappname}.azurewebsites.net/swagger`.)</span></span>
   
    <span data-ttu-id="30d43-354">Hello prohlížeče zobrazí hello stejné uživatelské rozhraní Swagger, které jste předtím viděli pro ToDoListDataAPI, ale nyní `owner` není povinné pole pro operaci Get hello, protože aplikaci API střední vrstvy hello odesílá aplikaci API datové vrstvy tuto hodnotu toohello za vás.</span><span class="sxs-lookup"><span data-stu-id="30d43-354">hello browser displays hello same Swagger UI that you saw earlier for ToDoListDataAPI, but now `owner` is not a required field for hello Get operation, because hello middle tier API app is sending that value toohello data tier API app for you.</span></span> <span data-ttu-id="30d43-355">(Pokud hello kurzech ověřování, odešle hello střední vrstva skutečné uživatelské ID pro hello `owner` parametr; teď ho je pevně kódováno hvězdičky.)</span><span class="sxs-lookup"><span data-stu-id="30d43-355">(When you do hello authentication tutorials, hello middle tier will send actual user IDs for hello `owner` parameter; for now it is hard-coding an asterisk.)</span></span>
3. <span data-ttu-id="30d43-356">Vyzkoušejte metodu Get hello a hello toovalidate jiné metody, která hello aplikaci API střední vrstvy úspěšně volá aplikaci API datové vrstvy hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-356">Try out hello Get method and hello other methods toovalidate that hello middle tier API app is successfully calling hello data tier API app.</span></span>
   
    ![Metoda Get uživatelského rozhraní Swagger](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a><span data-ttu-id="30d43-358">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="30d43-358">Troubleshooting</span></span>
<span data-ttu-id="30d43-359">V případě, že při procházení tohoto kurzu narazíte na problém, můžete ho zkusit vyřešit pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="30d43-359">In case you run into a problem as you go through this tutorial here are some troubleshooting ideas:</span></span>

* <span data-ttu-id="30d43-360">Ujistěte se, že používáte nejnovější verzi hello hello [Azure SDK for .NET](http://go.microsoft.com/fwlink/?linkid=518003).</span><span class="sxs-lookup"><span data-stu-id="30d43-360">Make sure that you're using hello latest version of hello [Azure SDK for .NET](http://go.microsoft.com/fwlink/?linkid=518003).</span></span>
* <span data-ttu-id="30d43-361">Dva názvy projektů hello jsou podobné (ToDoListAPI, ToDoListDataAPI).</span><span class="sxs-lookup"><span data-stu-id="30d43-361">Two of hello project names are similar (ToDoListAPI, ToDoListDataAPI).</span></span> <span data-ttu-id="30d43-362">Pokud věcí nevypadají, jak je popsáno v hello pokyny, když pracujete s projektem, ujistěte se, že máte otevřený správný projekt hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-362">If things don't look as described in hello instructions when you are working with a project, make sure you have opened hello correct project.</span></span>
* <span data-ttu-id="30d43-363">Pokud jste v podnikové síti a pokoušíte toodeploy tooAzure služby App Service přes bránu firewall, ujistěte se, že jsou pro nasazení webu otevřené porty 443 a 8172.</span><span class="sxs-lookup"><span data-stu-id="30d43-363">If you're on a corporate network and are trying toodeploy tooAzure App Service through a firewall, make sure that ports 443 and 8172 are open for Web Deploy.</span></span> <span data-ttu-id="30d43-364">Pokud tyto porty nedají otevřít, můžete použít jiné metody nasazení.</span><span class="sxs-lookup"><span data-stu-id="30d43-364">If you can't open those ports, you can use other deployment methods.</span></span>  <span data-ttu-id="30d43-365">V tématu [nasazení vaší aplikace tooAzure služby App Service](../app-service-web/web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="30d43-365">See [Deploy your app tooAzure App Service](../app-service-web/web-sites-deploy.md).</span></span>
* <span data-ttu-id="30d43-366">Chyby "názvy tras musí být jedinečné" – může dojít, tyto Pokud neúmyslně nasadíte aplikaci tooan rozhraní API hello nesprávný projekt a později nasadíte správný jeden tooit hello.</span><span class="sxs-lookup"><span data-stu-id="30d43-366">"Route names must be unique" errors -- you could get these if you accidentally deploy hello wrong project tooan API app and then later deploy hello correct one tooit.</span></span> <span data-ttu-id="30d43-367">toocorrect této, znovu ho zaveďte hello správný projekt toohello rozhraní API aplikace a na hello **nastavení** kartě hello **Publikovat Web** průvodce vyberte **odebrat další soubory v cílovém umístění**.</span><span class="sxs-lookup"><span data-stu-id="30d43-367">toocorrect this, redeploy hello correct project toohello API app, and on hello **Settings** tab of hello **Publish Web** wizard select **Remove additional files at destination**.</span></span>

<span data-ttu-id="30d43-368">Až budete mít vaše aplikace API technologie ASP.NET spuštěná ve službě Azure App Service, můžete toolearn Další informace o sadě Visual Studio funkce, které usnadňují řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="30d43-368">After you have your ASP.NET API app running in Azure App Service, you may want toolearn more about Visual Studio features that simplify troubleshooting.</span></span> <span data-ttu-id="30d43-369">Informace o protokolování, vzdáleném ladění apod. najdete v tématu [Řešení potíží s aplikacemi Azure App Service v nástroji Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="30d43-369">For information about logging, remote debugging, and more, see  [Troubleshooting Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="30d43-370">Další kroky</span><span class="sxs-lookup"><span data-stu-id="30d43-370">Next steps</span></span>
<span data-ttu-id="30d43-371">Už víte, jak generovat kód klienta pro aplikace API toodeploy existující webové projekty tooAPI aplikace API a využívat aplikace API z klientů .NET.</span><span class="sxs-lookup"><span data-stu-id="30d43-371">You've seen how toodeploy existing Web API projects tooAPI apps, generate client code for API apps, and consume API apps from .NET clients.</span></span> <span data-ttu-id="30d43-372">Hello další kurzu této série se dozvíte, jak příliš[používat aplikace CORS tooconsume rozhraní API z klientů JavaScript](app-service-api-cors-consume-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="30d43-372">hello next tutorial in this series shows how too[use CORS tooconsume API apps from JavaScript clients](app-service-api-cors-consume-javascript.md).</span></span>

<span data-ttu-id="30d43-373">Další informace o generování kódu klienta najdete v tématu hello [Azure/AutoRest](https://github.com/azure/autorest) na webu GitHub.com. Pomoc s pomocí klienta hello generuje problémy, otevřete [potíže v úložišti AutoRest hello](https://github.com/azure/autorest/issues).</span><span class="sxs-lookup"><span data-stu-id="30d43-373">For more information about client code generation, see hello [Azure/AutoRest](https://github.com/azure/autorest) repository on GitHub.com. For help with problems using hello generated client, open an [issue in hello AutoRest repository](https://github.com/azure/autorest/issues).</span></span>

<span data-ttu-id="30d43-374">Pokud chcete toocreate nový projekt aplikace API od začátku, použijte hello **aplikace API Azure** šablony.</span><span class="sxs-lookup"><span data-stu-id="30d43-374">If you want toocreate new API app projects from scratch, use hello **Azure API App** template.</span></span>

![Šablona aplikace API v nástroji Visual Studio](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

<span data-ttu-id="30d43-376">Hello **aplikace API Azure** šablona projektu je ekvivalentní toochoosing hello **prázdný** ASP.NET 4.5.2 šablona, kliknutím na tlačítko podpory webového rozhraní API tooadd hello zaškrtávací políčko a instalaci hello balíčku Swashbuckle NuGet.</span><span class="sxs-lookup"><span data-stu-id="30d43-376">hello **Azure API App** project template is equivalent toochoosing hello **Empty** ASP.NET 4.5.2 template, clicking hello check box tooadd Web API support, and installing hello Swashbuckle NuGet package.</span></span> <span data-ttu-id="30d43-377">Kromě toho přidá šablona hello některé Swashbuckle konfigurace kód tooprevent hello vytvoření duplicitních ID operací Swagger.</span><span class="sxs-lookup"><span data-stu-id="30d43-377">In addition, hello template adds some Swashbuckle configuration code designed tooprevent hello creation of duplicate Swagger operation IDs.</span></span> <span data-ttu-id="30d43-378">Po vytvoření projektu aplikace API, abyste ji mohli nasadit aplikace hello tooan rozhraní API stejným způsobem, jakým jste viděli v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="30d43-378">Once you've created an API App project, you can deploy it tooan API app hello same way you saw in this tutorial.</span></span>

