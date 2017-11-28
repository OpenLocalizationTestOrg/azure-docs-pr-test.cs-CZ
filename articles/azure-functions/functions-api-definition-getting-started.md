---
title: "aaaGetting Začínáme s metadaty OpenAPI v Azure Functions | Microsoft Docs"
description: "Začínáme s podporou OpenAPI v Azure Functions"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/23/2017
ms.author: alkarche
ms.openlocfilehash: fee3464c9a0a11b6d3891ccd0e9c5343d6bedcad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-openapi-20-swagger-metadata-for-a-function-app-preview"></a><span data-ttu-id="2f86f-103">Vytváření OpenAPI 2.0 (Swagger) Metadata pro funkce aplikace (Preview)</span><span class="sxs-lookup"><span data-stu-id="2f86f-103">Creating OpenAPI 2.0 (Swagger) Metadata for a Function App (Preview)</span></span>

<span data-ttu-id="2f86f-104">Tento dokument vás provede procesem hello krok za krokem vytvoření definici OpenAPI pro jednoduché rozhraní API hostované na Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="2f86f-104">This document guides you through hello step by step process of creating an OpenAPI Definition for a simple API hosted on Azure Functions.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

### <a name="what-is-openapi-swagger"></a><span data-ttu-id="2f86f-105">Co je OpenAPI (Swagger)?</span><span class="sxs-lookup"><span data-stu-id="2f86f-105">What is OpenAPI (Swagger)?</span></span>
<span data-ttu-id="2f86f-106">[Swagger Metadata](http://swagger.io/) je soubor, který definuje hello funkce operační režimy rozhraní API a umožňuje funkce hostování REST API toobe spotřebovávají širokou škálu na ostatní software.</span><span class="sxs-lookup"><span data-stu-id="2f86f-106">[Swagger Metadata](http://swagger.io/) is a file that defines hello functionality and operating modes of your API, and allows a function hosting a REST API toobe consumed by a wide variety of other software.</span></span> <span data-ttu-id="2f86f-107">Microsoft nabídky jako PowerApps a [aplikace API](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), a také 3. stran vývojářských nástrojů jako [Postman](https://www.getpostman.com/docs/importing_swagger) a [mnoho další balíčky](http://swagger.io/tools/) všechny vaše rozhraní API toobe spotřebované s povolit Definici swaggeru.</span><span class="sxs-lookup"><span data-stu-id="2f86f-107">Microsoft offerings like PowerApps and [API Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), as well as 3rd party developer tooling like [Postman](https://www.getpostman.com/docs/importing_swagger) and [many more packages](http://swagger.io/tools/) all allow your API toobe consumed with a Swagger definition.</span></span>

## <span data-ttu-id="2f86f-108"><a name="prepare-function"></a>Vytvoření funkce pomocí jednoduché rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2f86f-108"><a name="prepare-function"></a>Creating a Function with a simple API</span></span>
  <span data-ttu-id="2f86f-109">toocreate definici OpenAPI, potřebujeme nejprve toocreate funkce s jednoduché rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2f86f-109">toocreate an OpenAPI definition, we first need toocreate a Function with a simple API.</span></span> <span data-ttu-id="2f86f-110">Pokud již máte rozhraní API hostované na funkce aplikace, můžete přeskočit přímých toohello další části</span><span class="sxs-lookup"><span data-stu-id="2f86f-110">If you already have an API hosted on a Function App, you can skip straight toohello next section</span></span>
1. <span data-ttu-id="2f86f-111">Vytvořte novou aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="2f86f-111">Create a new Function App.</span></span>
    1. <span data-ttu-id="2f86f-112">[Portál Azure](https://portal.azure.com)  >  `+ New` > vyhledejte "Aplikace funkce"</span><span class="sxs-lookup"><span data-stu-id="2f86f-112">[Azure portal](https://portal.azure.com) > `+ New` > Search for "Function App"</span></span>
1. <span data-ttu-id="2f86f-113">Vytvořte novou funkci Aktivace protokolu HTTP v nové funkce aplikace</span><span class="sxs-lookup"><span data-stu-id="2f86f-113">Create a new HTTP trigger function inside your new Function App</span></span>
    1. <span data-ttu-id="2f86f-114">Funkce je již nastavena kód definování velmi jednoduché rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="2f86f-114">Your function is pre-populated with code defining a very simple REST API.</span></span>
    1. <span data-ttu-id="2f86f-115">Libovolný řetězec úspěšnou toohello funkce jako parametr dotazu nebo v textu hello se vrátí jako "Hello {vstup}"</span><span class="sxs-lookup"><span data-stu-id="2f86f-115">Any string passed toohello Function as a query parameter or in hello body is returned as "Hello {input}"</span></span>
1. <span data-ttu-id="2f86f-116">Přejděte toohello `Integrate` kartě novou funkci triggeru protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="2f86f-116">Go toohello `Integrate` tab of your new HTTP Trigger function</span></span>
    1. <span data-ttu-id="2f86f-117">Přepnutí `Allowed HTTP methods` příliš`Selected methods`</span><span class="sxs-lookup"><span data-stu-id="2f86f-117">Toggle `Allowed HTTP methods` too`Selected methods`</span></span>
    1. <span data-ttu-id="2f86f-118">V `Selected HTTP methods` zrušte všechny operace s výjimkou POST.</span><span class="sxs-lookup"><span data-stu-id="2f86f-118">In `Selected HTTP methods` uncheck every verb except POST.</span></span>
    <span data-ttu-id="2f86f-119">![Metody vybrané HTTP](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)</span><span class="sxs-lookup"><span data-stu-id="2f86f-119">![Selected HTTP Methods](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)</span></span>
    1. <span data-ttu-id="2f86f-120">Tento krok zjednoduší definice rozhraní API později.</span><span class="sxs-lookup"><span data-stu-id="2f86f-120">This step will simplify your API definition later on.</span></span>

## <span data-ttu-id="2f86f-121"><a name="enable"></a>Povolení podpory definice rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2f86f-121"><a name="enable"></a>Enabling API Definition Support</span></span>
1. <span data-ttu-id="2f86f-122">Přejděte příliš`your function name` > `Platform Features` > `API Definition`
![karta definice.](./media/functions-api-definition-getting-started/definitiontab.png)</span><span class="sxs-lookup"><span data-stu-id="2f86f-122">Navigate too`your function name` > `Platform Features` > `API Definition`
![Definition Tab](./media/functions-api-definition-getting-started/definitiontab.png)</span></span>
1. <span data-ttu-id="2f86f-123">Nastavit `API Definition Source` příliš`Function (preview)`</span><span class="sxs-lookup"><span data-stu-id="2f86f-123">Set `API Definition Source` too`Function (preview)`</span></span>
    1. <span data-ttu-id="2f86f-124">Tento krok povoluje sada možností OpenAPI pro funkce aplikace, včetně koncový bod toohost soubor OpenAPI z domény aplikaci funkce, jako vložené kopie hello [OpenAPI Editor](http://editor.swagger.io)a generátor definice rychlý start.</span><span class="sxs-lookup"><span data-stu-id="2f86f-124">This step enables a suite of OpenAPI options for your Function App, including an endpoint toohost an OpenAPI file from your Function App's domain, an inline copy of hello [OpenAPI Editor](http://editor.swagger.io), and a quickstart definition generator.</span></span>
<span data-ttu-id="2f86f-125">![Povoleno definice](./media/functions-api-definition-getting-started/enabledefinition.png)</span><span class="sxs-lookup"><span data-stu-id="2f86f-125">![Enabled Definition](./media/functions-api-definition-getting-started/enabledefinition.png)</span></span>

## <span data-ttu-id="2f86f-126"><a name="create-definition"></a>Vytvoření vaší definice rozhraní API ze šablony</span><span class="sxs-lookup"><span data-stu-id="2f86f-126"><a name="create-definition"></a>Creating your API Definition from a template</span></span>
1. <span data-ttu-id="2f86f-127">Klikněte na `Generate API Definition template`.</span><span class="sxs-lookup"><span data-stu-id="2f86f-127">Click `Generate API Definition template`</span></span>
    1. <span data-ttu-id="2f86f-128">Tento krok kontroluje funkce aplikace pro funkce triggeru protokolu HTTP a použije hello údaje v functions.json toogenerate dokument OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="2f86f-128">This step scans your Function App for HTTP Trigger functions and uses hello info in functions.json toogenerate an OpenAPI document.</span></span>
1. <span data-ttu-id="2f86f-129">Přidejte objekt operaci příliš`paths: /api/yourfunctionroute post:`</span><span class="sxs-lookup"><span data-stu-id="2f86f-129">Add an operation object too`paths: /api/yourfunctionroute post:`</span></span>
    1. <span data-ttu-id="2f86f-130">Hello rychlý start OpenAPI dokument je přehled úplné OpenAPI dokumentů. To vyžaduje další metadata toobe úplné definice OpenAPI, například operace objekty a odpovědi šablony.</span><span class="sxs-lookup"><span data-stu-id="2f86f-130">hello quickstart OpenAPI document is an outline of a full OpenAPI doc. It requires more metadata toobe a full OpenAPI definition, such as operation objects and response templates.</span></span>
    1. <span data-ttu-id="2f86f-131">vytváří nebo využívá část, objekt parametr a objekt odpovědi je vyplnění Hello ukázka operaci objekt níže.</span><span class="sxs-lookup"><span data-stu-id="2f86f-131">hello sample operation object below has a filled out produces/consumes section, a parameter object, and a response object.</span></span>
    
    ```yaml
      post:
        operationId: /api/yourfunctionroute/post
        consumes: [application/json]
        produces: [application/json]
        parameters:
          - name: name
            in: body
            description: Your name
            x-ms-summary: Your name
            required: true
            schema:
              type: object
              properties:
                name:
                  type: string
        description: >-
          Replace with Operation Object
          #http://swagger.io/specification/#operationObject
        responses:
          '200':
            description: A Greeting
            x-ms-summary: Your name
            schema:
              type: string
        security:
          - apikeyQuery: []
    ```
    
    > [!NOTE]
    >  <span data-ttu-id="2f86f-132">x-ms souhrn poskytuje zobrazovaný název Logic Apps, toku a PowerApps.</span><span class="sxs-lookup"><span data-stu-id="2f86f-132">x-ms-summary provides a display name in Logic Apps, Flow, and PowerApps.</span></span>
    >
    > <span data-ttu-id="2f86f-133">Podívejte se na [přizpůsobit vaší definici Swaggeru pro PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="2f86f-133">Check out [customize your Swagger definition for PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) toolearn more.</span></span>

1. <span data-ttu-id="2f86f-134">Klikněte na tlačítko `save` toosave změny ![přidání definice šablony](./media/functions-api-definition-getting-started/addingtemplate.png)</span><span class="sxs-lookup"><span data-stu-id="2f86f-134">Click `save` toosave your changes ![Adding Template Definition](./media/functions-api-definition-getting-started/addingtemplate.png)</span></span>

## <span data-ttu-id="2f86f-135"><a name="use-definition"></a>Pomocí vaší definice rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2f86f-135"><a name="use-definition"></a>Using Your API Definition</span></span>
1. <span data-ttu-id="2f86f-136">Kopie vašeho `API definition URL` a vložte jej do nového tooview karta prohlížeče nezpracovaná OpenAPI dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2f86f-136">Copy your `API definition URL` and paste it into a new browser tab tooview your raw OpenAPI document.</span></span>
1. <span data-ttu-id="2f86f-137">Můžete importovat vaše OpenAPI tooany číslem nástrojů pro testování a integrace s použitím tuto adresu URL.</span><span class="sxs-lookup"><span data-stu-id="2f86f-137">You can import your OpenAPI document tooany number of tools for testing and integration using that URL.</span></span>
    1. <span data-ttu-id="2f86f-138">Mnoho prostředků Azure jsou možné tooautomatically import dokumentu OpenAPI pomocí hello URL definice rozhraní API, který je uložený v nastavení aplikace funkce.</span><span class="sxs-lookup"><span data-stu-id="2f86f-138">Many Azure resources are able tooautomatically import your OpenAPI doc using hello API Definition URL that is saved in your Function application settings.</span></span> <span data-ttu-id="2f86f-139">Jako součást hello `Function` zdroj definice rozhraní API, budeme aktualizovat tuto adresu url pro vás.</span><span class="sxs-lookup"><span data-stu-id="2f86f-139">As a part of hello `Function` API definition source, we update that url for you.</span></span>


## <span data-ttu-id="2f86f-140"><a name="test-definition"></a>Pomocí tootest hello uživatelské rozhraní Swagger, vaše definice rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2f86f-140"><a name="test-definition"></a>Using hello Swagger UI tootest your API definition</span></span>
<span data-ttu-id="2f86f-141">Testujete nástrojů integrovaných v zobrazení uživatelského rozhraní toohello editoru definice rozhraní API hello vestavěného okna.</span><span class="sxs-lookup"><span data-stu-id="2f86f-141">There are testing tools built in toohello UI view of hello imbedded API definition editor.</span></span> <span data-ttu-id="2f86f-142">Přidejte svůj klíč rozhraní API a pak použijte hello `Try this operation` tlačítko každá z metod.</span><span class="sxs-lookup"><span data-stu-id="2f86f-142">Add your API key, and then use hello `Try this operation` button under each method.</span></span> <span data-ttu-id="2f86f-143">Nástroj pro Hello bude používat své žádosti hello tooformat definice rozhraní API a úspěšné odpovědi bude znamenat, že vaše definice správné.</span><span class="sxs-lookup"><span data-stu-id="2f86f-143">hello tool will use your API Definition tooformat hello requests and successful responses will indicate that your definition is correct.</span></span>

### <a name="steps"></a><span data-ttu-id="2f86f-144">pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="2f86f-144">Steps:</span></span>

1. <span data-ttu-id="2f86f-145">Zkopírujte klíč rozhraní API – funkce</span><span class="sxs-lookup"><span data-stu-id="2f86f-145">Copy your function API key</span></span>
    1. <span data-ttu-id="2f86f-146">klíč Hello rozhraní API lze nalézt v aktivační události funkce protokolu HTTP v části `function name` > `Keys` > `Function Keys` 
   ![funkční klávesa](./media/functions-api-definition-getting-started/functionkey.png)</span><span class="sxs-lookup"><span data-stu-id="2f86f-146">hello API key can be found in your HTTP Trigger Function under `function name` > `Keys` > `Function Keys`
![Function key](./media/functions-api-definition-getting-started/functionkey.png)</span></span>
1. <span data-ttu-id="2f86f-147">Přejděte toohello `API Definition` stránky.</span><span class="sxs-lookup"><span data-stu-id="2f86f-147">Navigate toohello `API Definition` page.</span></span>
    1. <span data-ttu-id="2f86f-148">Klikněte na tlačítko `Authenticate` a přidejte objekt zabezpečení klíčů toohello vaše rozhraní API funkce v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="2f86f-148">Click `Authenticate` and add your Function API key toohello security object at hello top.</span></span>
  <span data-ttu-id="2f86f-149">![Klíč OpenAPI](./media/functions-api-definition-getting-started/definitionTest auth.png)</span><span class="sxs-lookup"><span data-stu-id="2f86f-149">![OpenAPI key](./media/functions-api-definition-getting-started/definitionTest auth.png)</span></span>
1. <span data-ttu-id="2f86f-150">Vyberte`/api/yourfunctionroute` > `POST`</span><span class="sxs-lookup"><span data-stu-id="2f86f-150">select `/api/yourfunctionroute` > `POST`</span></span>
1. <span data-ttu-id="2f86f-151">Klikněte na tlačítko `Try it out` a zadejte název tootest</span><span class="sxs-lookup"><span data-stu-id="2f86f-151">Click `Try it out` and enter a name tootest</span></span>
1. <span data-ttu-id="2f86f-152">Měli byste vidět úspěšný výsledek pod `Pretty` 
 ![definice rozhraní API testu](./media/functions-api-definition-getting-started/definitionTest.png)</span><span class="sxs-lookup"><span data-stu-id="2f86f-152">You should see a SUCCESS result under `Pretty`
![API Definition test](./media/functions-api-definition-getting-started/definitionTest.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f86f-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2f86f-153">Next steps</span></span>
* [<span data-ttu-id="2f86f-154">Definice OpenAPI v přehled funkcí</span><span class="sxs-lookup"><span data-stu-id="2f86f-154">OpenAPI Definition in Functions Overview</span></span>](functions-api-definition.md)
  * <span data-ttu-id="2f86f-155">Přečtěte si dokumentaci úplné hello Další informace o podporovaných OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="2f86f-155">Read hello full documentation for more info on OpenAPI support.</span></span>
* [<span data-ttu-id="2f86f-156">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="2f86f-156">Azure Functions developer reference</span></span>](functions-reference.md)  
  * <span data-ttu-id="2f86f-157">Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.</span><span class="sxs-lookup"><span data-stu-id="2f86f-157">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="2f86f-158">Úložiště GitHub – Azure Functions</span><span class="sxs-lookup"><span data-stu-id="2f86f-158">Azure Functions GitHub repository</span></span>](https://github.com/Azure/Azure-Functions/)
  * <span data-ttu-id="2f86f-159">Podívejte se na Githubu funkce toogive hello nám zpětnou vazbu na hello rozhraní API definice podporu preview!</span><span class="sxs-lookup"><span data-stu-id="2f86f-159">Check out hello Functions GitHub toogive us feedback on hello API definition support preview!</span></span> <span data-ttu-id="2f86f-160">Ujistěte se, problém Githubu pro všechno, co byste chtěli toosee aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="2f86f-160">Make a GitHub issue for anything you'd like toosee updated.</span></span>
