---
title: "Začínáme s metadaty OpenAPI v Azure Functions | Microsoft Docs"
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
ms.openlocfilehash: 9b861aacf31e17866293732dc2323f56014c1877
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="creating-openapi-20-swagger-metadata-for-a-function-app-preview"></a><span data-ttu-id="df03a-103">Vytváření OpenAPI 2.0 (Swagger) Metadata pro funkce aplikace (Preview)</span><span class="sxs-lookup"><span data-stu-id="df03a-103">Creating OpenAPI 2.0 (Swagger) Metadata for a Function App (Preview)</span></span>

<span data-ttu-id="df03a-104">Tento dokument vás provede krok za krokem procesu vytváření definici OpenAPI pro jednoduché rozhraní API hostované na Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="df03a-104">This document guides you through the step by step process of creating an OpenAPI Definition for a simple API hosted on Azure Functions.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

### <a name="what-is-openapi-swagger"></a><span data-ttu-id="df03a-105">Co je OpenAPI (Swagger)?</span><span class="sxs-lookup"><span data-stu-id="df03a-105">What is OpenAPI (Swagger)?</span></span>
<span data-ttu-id="df03a-106">[Swagger Metadata](http://swagger.io/) je soubor, který definuje provozní režimy rozhraní API a funkce a umožňuje funkce hostování rozhraní REST API pro širokou škálu na ostatní software.</span><span class="sxs-lookup"><span data-stu-id="df03a-106">[Swagger Metadata](http://swagger.io/) is a file that defines the functionality and operating modes of your API, and allows a function hosting a REST API to be consumed by a wide variety of other software.</span></span> <span data-ttu-id="df03a-107">Microsoft nabídky jako PowerApps a [aplikace API](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), a také 3. stran vývojářských nástrojů jako [Postman](https://www.getpostman.com/docs/importing_swagger) a [mnoho další balíčky](http://swagger.io/tools/) všechny vaše rozhraní API, který se má používat s povolit Definici swaggeru.</span><span class="sxs-lookup"><span data-stu-id="df03a-107">Microsoft offerings like PowerApps and [API Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), as well as 3rd party developer tooling like [Postman](https://www.getpostman.com/docs/importing_swagger) and [many more packages](http://swagger.io/tools/) all allow your API to be consumed with a Swagger definition.</span></span>

## <span data-ttu-id="df03a-108"><a name="prepare-function"></a>Vytvoření funkce pomocí jednoduché rozhraní API</span><span class="sxs-lookup"><span data-stu-id="df03a-108"><a name="prepare-function"></a>Creating a Function with a simple API</span></span>
  <span data-ttu-id="df03a-109">Vytvoření definice OpenAPI, musíme nejprve vytvořit funkci s jednoduché rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="df03a-109">To create an OpenAPI definition, we first need to create a Function with a simple API.</span></span> <span data-ttu-id="df03a-110">Pokud již máte rozhraní API hostované na funkce aplikace, můžete přeskočit přímo k další části</span><span class="sxs-lookup"><span data-stu-id="df03a-110">If you already have an API hosted on a Function App, you can skip straight to the next section</span></span>
1. <span data-ttu-id="df03a-111">Vytvořte novou aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="df03a-111">Create a new Function App.</span></span>
    1. <span data-ttu-id="df03a-112">[Portál Azure](https://portal.azure.com)  >  `+ New` > vyhledejte "Aplikace funkce"</span><span class="sxs-lookup"><span data-stu-id="df03a-112">[Azure portal](https://portal.azure.com) > `+ New` > Search for "Function App"</span></span>
1. <span data-ttu-id="df03a-113">Vytvořte novou funkci Aktivace protokolu HTTP v nové funkce aplikace</span><span class="sxs-lookup"><span data-stu-id="df03a-113">Create a new HTTP trigger function inside your new Function App</span></span>
    1. <span data-ttu-id="df03a-114">Funkce je již nastavena kód definování velmi jednoduché rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="df03a-114">Your function is pre-populated with code defining a very simple REST API.</span></span>
    1. <span data-ttu-id="df03a-115">Libovolný řetězec funkci byl předán jako parametr dotazu nebo v těle se vrátí jako "Hello {vstup}"</span><span class="sxs-lookup"><span data-stu-id="df03a-115">Any string passed to the Function as a query parameter or in the body is returned as "Hello {input}"</span></span>
1. <span data-ttu-id="df03a-116">Přejděte na `Integrate` kartě novou funkci triggeru protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="df03a-116">Go to the `Integrate` tab of your new HTTP Trigger function</span></span>
    1. <span data-ttu-id="df03a-117">Přepnutí `Allowed HTTP methods` na`Selected methods`</span><span class="sxs-lookup"><span data-stu-id="df03a-117">Toggle `Allowed HTTP methods` to `Selected methods`</span></span>
    1. <span data-ttu-id="df03a-118">V `Selected HTTP methods` zrušte všechny operace s výjimkou POST.</span><span class="sxs-lookup"><span data-stu-id="df03a-118">In `Selected HTTP methods` uncheck every verb except POST.</span></span>
    <span data-ttu-id="df03a-119">![Metody vybrané HTTP](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)</span><span class="sxs-lookup"><span data-stu-id="df03a-119">![Selected HTTP Methods](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)</span></span>
    1. <span data-ttu-id="df03a-120">Tento krok zjednoduší definice rozhraní API později.</span><span class="sxs-lookup"><span data-stu-id="df03a-120">This step will simplify your API definition later on.</span></span>

## <span data-ttu-id="df03a-121"><a name="enable"></a>Povolení podpory definice rozhraní API</span><span class="sxs-lookup"><span data-stu-id="df03a-121"><a name="enable"></a>Enabling API Definition Support</span></span>
1. <span data-ttu-id="df03a-122">Přejděte na `your function name`  >  `Platform Features`  >  `API Definition` 
 ![karta definice.](./media/functions-api-definition-getting-started/definitiontab.png)</span><span class="sxs-lookup"><span data-stu-id="df03a-122">Navigate to `your function name` > `Platform Features` > `API Definition`
![Definition Tab](./media/functions-api-definition-getting-started/definitiontab.png)</span></span>
1. <span data-ttu-id="df03a-123">Nastavit `API Definition Source` do`Function (preview)`</span><span class="sxs-lookup"><span data-stu-id="df03a-123">Set `API Definition Source` to `Function (preview)`</span></span>
    1. <span data-ttu-id="df03a-124">Tento krok povoluje sada možností OpenAPI pro funkce aplikace, včetně koncový bod pro hostování soubor OpenAPI z domény aplikaci funkce, vložené kopie [OpenAPI Editor](http://editor.swagger.io)a generátor definice rychlý start.</span><span class="sxs-lookup"><span data-stu-id="df03a-124">This step enables a suite of OpenAPI options for your Function App, including an endpoint to host an OpenAPI file from your Function App's domain, an inline copy of the [OpenAPI Editor](http://editor.swagger.io), and a quickstart definition generator.</span></span>
<span data-ttu-id="df03a-125">![Povoleno definice](./media/functions-api-definition-getting-started/enabledefinition.png)</span><span class="sxs-lookup"><span data-stu-id="df03a-125">![Enabled Definition](./media/functions-api-definition-getting-started/enabledefinition.png)</span></span>

## <span data-ttu-id="df03a-126"><a name="create-definition"></a>Vytvoření vaší definice rozhraní API ze šablony</span><span class="sxs-lookup"><span data-stu-id="df03a-126"><a name="create-definition"></a>Creating your API Definition from a template</span></span>
1. <span data-ttu-id="df03a-127">Klikněte na `Generate API Definition template`.</span><span class="sxs-lookup"><span data-stu-id="df03a-127">Click `Generate API Definition template`</span></span>
    1. <span data-ttu-id="df03a-128">Tento krok kontroluje funkce aplikace pro funkce triggeru protokolu HTTP a používá informace v functions.json k vygenerování dokument OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="df03a-128">This step scans your Function App for HTTP Trigger functions and uses the info in functions.json to generate an OpenAPI document.</span></span>
1. <span data-ttu-id="df03a-129">Přidat objekt operaci tak, aby`paths: /api/yourfunctionroute post:`</span><span class="sxs-lookup"><span data-stu-id="df03a-129">Add an operation object to `paths: /api/yourfunctionroute post:`</span></span>
    1. <span data-ttu-id="df03a-130">Dokument OpenAPI rychlý start je přehled úplné OpenAPI dokumentů. To vyžaduje další metadata, aby se úplná definice OpenAPI, jako jsou operace objekty a odpovědi šablony.</span><span class="sxs-lookup"><span data-stu-id="df03a-130">The quickstart OpenAPI document is an outline of a full OpenAPI doc. It requires more metadata to be a full OpenAPI definition, such as operation objects and response templates.</span></span>
    1. <span data-ttu-id="df03a-131">Objekt operace ukázka níže má vyplnění vytváří nebo využívá část, objekt parametr a objektu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="df03a-131">The sample operation object below has a filled out produces/consumes section, a parameter object, and a response object.</span></span>
    
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
    >  <span data-ttu-id="df03a-132">x-ms souhrn poskytuje zobrazovaný název Logic Apps, toku a PowerApps.</span><span class="sxs-lookup"><span data-stu-id="df03a-132">x-ms-summary provides a display name in Logic Apps, Flow, and PowerApps.</span></span>
    >
    > <span data-ttu-id="df03a-133">Podívejte se na [přizpůsobit vaší definici Swaggeru pro PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="df03a-133">Check out [customize your Swagger definition for PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) to learn more.</span></span>

1. <span data-ttu-id="df03a-134">Klikněte na tlačítko `save` uložte provedené změny ![přidání definice šablony](./media/functions-api-definition-getting-started/addingtemplate.png)</span><span class="sxs-lookup"><span data-stu-id="df03a-134">Click `save` to save your changes ![Adding Template Definition](./media/functions-api-definition-getting-started/addingtemplate.png)</span></span>

## <span data-ttu-id="df03a-135"><a name="use-definition"></a>Pomocí vaší definice rozhraní API</span><span class="sxs-lookup"><span data-stu-id="df03a-135"><a name="use-definition"></a>Using Your API Definition</span></span>
1. <span data-ttu-id="df03a-136">Kopie vašeho `API definition URL` a vložte jej do novou kartu prohlížeče na nezpracovaná OpenAPI dokument.</span><span class="sxs-lookup"><span data-stu-id="df03a-136">Copy your `API definition URL` and paste it into a new browser tab to view your raw OpenAPI document.</span></span>
1. <span data-ttu-id="df03a-137">Dokument OpenAPI můžete importovat do libovolný počet nástrojů pro testování a integrace s použitím tuto adresu URL.</span><span class="sxs-lookup"><span data-stu-id="df03a-137">You can import your OpenAPI document to any number of tools for testing and integration using that URL.</span></span>
    1. <span data-ttu-id="df03a-138">Mnoho prostředků Azure dokážou automaticky importovat OpenAPI dokumentu pomocí URL definice rozhraní API, který je uložený v nastavení aplikace funkce.</span><span class="sxs-lookup"><span data-stu-id="df03a-138">Many Azure resources are able to automatically import your OpenAPI doc using the API Definition URL that is saved in your Function application settings.</span></span> <span data-ttu-id="df03a-139">Jako součást `Function` zdroj definice rozhraní API, budeme aktualizovat tuto adresu url pro vás.</span><span class="sxs-lookup"><span data-stu-id="df03a-139">As a part of the `Function` API definition source, we update that url for you.</span></span>


## <span data-ttu-id="df03a-140"><a name="test-definition"></a>Testování vaší definice rozhraní API pomocí uživatelského rozhraní Swagger</span><span class="sxs-lookup"><span data-stu-id="df03a-140"><a name="test-definition"></a>Using the Swagger UI to test your API definition</span></span>
<span data-ttu-id="df03a-141">Testujete nástrojů integrovaných zobrazení uživatelského rozhraní editoru vestavěného okna definice rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="df03a-141">There are testing tools built in to the UI view of the imbedded API definition editor.</span></span> <span data-ttu-id="df03a-142">Přidejte svůj klíč rozhraní API a potom pomocí `Try this operation` tlačítko jednotlivých metod.</span><span class="sxs-lookup"><span data-stu-id="df03a-142">Add your API key, and then use the `Try this operation` button under each method.</span></span> <span data-ttu-id="df03a-143">Nástroj použije vaše definice rozhraní API pro formát žádosti a úspěšné odpovědi bude znamenat, že vaše definice správné.</span><span class="sxs-lookup"><span data-stu-id="df03a-143">The tool will use your API Definition to format the requests and successful responses will indicate that your definition is correct.</span></span>

### <a name="steps"></a><span data-ttu-id="df03a-144">pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="df03a-144">Steps:</span></span>

1. <span data-ttu-id="df03a-145">Zkopírujte klíč rozhraní API – funkce</span><span class="sxs-lookup"><span data-stu-id="df03a-145">Copy your function API key</span></span>
    1. <span data-ttu-id="df03a-146">Klíč rozhraní API najdete v aktivační události funkce protokolu HTTP v části `function name` > `Keys` > `Function Keys` 
   ![funkční klávesa](./media/functions-api-definition-getting-started/functionkey.png)</span><span class="sxs-lookup"><span data-stu-id="df03a-146">The API key can be found in your HTTP Trigger Function under `function name` > `Keys` > `Function Keys`
![Function key](./media/functions-api-definition-getting-started/functionkey.png)</span></span>
1. <span data-ttu-id="df03a-147">Přejděte do `API Definition` stránky.</span><span class="sxs-lookup"><span data-stu-id="df03a-147">Navigate to the `API Definition` page.</span></span>
    1. <span data-ttu-id="df03a-148">Klikněte na tlačítko `Authenticate` a přidejte klíč rozhraní API funkce k objektu zabezpečení v horní části.</span><span class="sxs-lookup"><span data-stu-id="df03a-148">Click `Authenticate` and add your Function API key to the security object at the top.</span></span>
  <span data-ttu-id="df03a-149">![Klíč OpenAPI](./media/functions-api-definition-getting-started/definitionTest auth.png)</span><span class="sxs-lookup"><span data-stu-id="df03a-149">![OpenAPI key](./media/functions-api-definition-getting-started/definitionTest auth.png)</span></span>
1. <span data-ttu-id="df03a-150">Vyberte`/api/yourfunctionroute` > `POST`</span><span class="sxs-lookup"><span data-stu-id="df03a-150">select `/api/yourfunctionroute` > `POST`</span></span>
1. <span data-ttu-id="df03a-151">Klikněte na tlačítko `Try it out` a zadejte název pro testování</span><span class="sxs-lookup"><span data-stu-id="df03a-151">Click `Try it out` and enter a name to test</span></span>
1. <span data-ttu-id="df03a-152">Měli byste vidět úspěšný výsledek pod `Pretty` 
 ![definice rozhraní API testu](./media/functions-api-definition-getting-started/definitionTest.png)</span><span class="sxs-lookup"><span data-stu-id="df03a-152">You should see a SUCCESS result under `Pretty`
![API Definition test](./media/functions-api-definition-getting-started/definitionTest.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="df03a-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="df03a-153">Next steps</span></span>
* [<span data-ttu-id="df03a-154">Definice OpenAPI v přehled funkcí</span><span class="sxs-lookup"><span data-stu-id="df03a-154">OpenAPI Definition in Functions Overview</span></span>](functions-api-definition.md)
  * <span data-ttu-id="df03a-155">Úplnou dokumentaci pro další informace o podporovaných OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="df03a-155">Read the full documentation for more info on OpenAPI support.</span></span>
* [<span data-ttu-id="df03a-156">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="df03a-156">Azure Functions developer reference</span></span>](functions-reference.md)  
  * <span data-ttu-id="df03a-157">Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.</span><span class="sxs-lookup"><span data-stu-id="df03a-157">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="df03a-158">Úložiště GitHub – Azure Functions</span><span class="sxs-lookup"><span data-stu-id="df03a-158">Azure Functions GitHub repository</span></span>](https://github.com/Azure/Azure-Functions/)
  * <span data-ttu-id="df03a-159">Podívejte se na Githubu funkce pro vaše názory na preview podporu definice rozhraní API!</span><span class="sxs-lookup"><span data-stu-id="df03a-159">Check out the Functions GitHub to give us feedback on the API definition support preview!</span></span> <span data-ttu-id="df03a-160">Ujistěte se, problém Githubu nic, co chcete zobrazit aktualizovaný.</span><span class="sxs-lookup"><span data-stu-id="df03a-160">Make a GitHub issue for anything you'd like to see updated.</span></span>
