---
title: Metadata OpenAPI v Azure Functions | Microsoft Docs
description: "Přehled podpory OpenAPI v Azure Functions"
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
ms.openlocfilehash: e426e56bcac30c740e86d620dadf291fe31e3e10
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="openapi-20-metadata-support-in-azure-functions-preview"></a><span data-ttu-id="ecd11-103">Podpora metadat OpenAPI 2.0 v Azure Functions (preview)</span><span class="sxs-lookup"><span data-stu-id="ecd11-103">OpenAPI 2.0 metadata support in Azure Functions (preview)</span></span>
<span data-ttu-id="ecd11-104">OpenAPI 2.0 (dříve Swagger) podpora metadat v Azure Functions je funkce preview, která můžete použít k zápisu definici OpenAPI 2.0 v aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="ecd11-104">OpenAPI 2.0 (formerly Swagger) metadata support in Azure Functions is a preview feature that you can use to write an OpenAPI 2.0 definition inside a function app.</span></span> <span data-ttu-id="ecd11-105">Tento soubor pak můžete hostovat pomocí funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="ecd11-105">You can then host that file by using the function app.</span></span>

<span data-ttu-id="ecd11-106">[OpenAPI metadata](http://swagger.io/) umožňuje funkci, která je hostitelem rozhraní REST API pro širokou škálu na ostatní software.</span><span class="sxs-lookup"><span data-stu-id="ecd11-106">[OpenAPI metadata](http://swagger.io/) allows a function that's hosting a REST API to be consumed by a wide variety of other software.</span></span> <span data-ttu-id="ecd11-107">Tento software obsahuje Microsoft nabídky jako PowerApps a [funkce API Apps služby Azure App Service](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), jako jsou nástroje pro vývojáře třetích stran [Postman](https://www.getpostman.com/docs/importing_swagger), a [mnoho další balíčky](http://swagger.io/tools/).</span><span class="sxs-lookup"><span data-stu-id="ecd11-107">This software includes Microsoft offerings like PowerApps and the [API Apps feature of Azure App Service](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), third-party developer tools like [Postman](https://www.getpostman.com/docs/importing_swagger), and [many more packages](http://swagger.io/tools/).</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

>[!TIP]
><span data-ttu-id="ecd11-108">Doporučujeme začít s [kurz Začínáme](./functions-api-definition-getting-started.md) a vrátilo se do tohoto dokumentu další informace týkající se konkrétních funkcí.</span><span class="sxs-lookup"><span data-stu-id="ecd11-108">We recommend starting with the [getting started tutorial](./functions-api-definition-getting-started.md) and then returning to this document to learn more about specific features.</span></span>

## <span data-ttu-id="ecd11-109"><a name="enable"></a>Povolit podporu definice OpenAPI</span><span class="sxs-lookup"><span data-stu-id="ecd11-109"><a name="enable"></a>Enable OpenAPI definition support</span></span>
<span data-ttu-id="ecd11-110">Můžete nakonfigurovat všechna nastavení OpenAPI na **definice rozhraní API** stránky v aplikaci funkce **funkce**.</span><span class="sxs-lookup"><span data-stu-id="ecd11-110">You can configure all OpenAPI settings on the **API Definition** page in your function app's **Platform features**.</span></span>

<span data-ttu-id="ecd11-111">Chcete-li povolit generování definici hostované OpenAPI a definice rychlý start, nastavte **zdroj definice rozhraní API** k **– funkce (Preview)**.</span><span class="sxs-lookup"><span data-stu-id="ecd11-111">To enable the generation of a hosted OpenAPI definition and a quickstart definition, set **API definition source** to **Function (Preview)**.</span></span> <span data-ttu-id="ecd11-112">**Externí adresu URL** umožňuje funkce sloužící definici OpenAPI, která má hostovaný jinde.</span><span class="sxs-lookup"><span data-stu-id="ecd11-112">**External URL** allows your function to use an OpenAPI definition that's hosted elsewhere.</span></span>

## <span data-ttu-id="ecd11-113"><a name="generate-definition"></a>Generovat kostru Swagger z vaší funkce metadat</span><span class="sxs-lookup"><span data-stu-id="ecd11-113"><a name="generate-definition"></a>Generate a Swagger skeleton from your function's metadata</span></span>
<span data-ttu-id="ecd11-114">Šablonu můžete zahájit zápis svou první OpenAPI definici.</span><span class="sxs-lookup"><span data-stu-id="ecd11-114">A template can help you start writing your first OpenAPI definition.</span></span> <span data-ttu-id="ecd11-115">Funkce šablony definice vytvoří definici zhuštěných OpenAPI pomocí všechna metadata v souboru function.json pro jednotlivé funkce aktivace protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="ecd11-115">The definition template feature creates a sparse OpenAPI definition by using all the metadata in the function.json file for each of your HTTP trigger functions.</span></span> <span data-ttu-id="ecd11-116">Budete muset vyplňte další informace o rozhraní API z [OpenAPI specifikace](http://swagger.io/specification/), jako je například šablony žádostí a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="ecd11-116">You'll need to fill in more information about your API from the [OpenAPI specification](http://swagger.io/specification/), such as request and response templates.</span></span>

<span data-ttu-id="ecd11-117">Podrobné pokyny najdete v tématu [kurz Začínáme](./functions-api-definition-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="ecd11-117">For step-by-step instructions, see the [getting started tutorial](./functions-api-definition-getting-started.md).</span></span>

### <span data-ttu-id="ecd11-118"><a name="templates"></a>Dostupné šablony</span><span class="sxs-lookup"><span data-stu-id="ecd11-118"><a name="templates"></a>Available templates</span></span>

|<span data-ttu-id="ecd11-119">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ecd11-119">Name</span></span>| <span data-ttu-id="ecd11-120">Popis</span><span class="sxs-lookup"><span data-stu-id="ecd11-120">Description</span></span> |
|:-----|:-----|
|<span data-ttu-id="ecd11-121">Vygenerovaný definice</span><span class="sxs-lookup"><span data-stu-id="ecd11-121">Generated Definition</span></span>|<span data-ttu-id="ecd11-122">Definici OpenAPI s maximální velikostí informace, které lze odvodit z existující metadata funkce.</span><span class="sxs-lookup"><span data-stu-id="ecd11-122">An OpenAPI definition with the maximum amount of information that can be inferred from the function's existing metadata.</span></span>|

### <span data-ttu-id="ecd11-123"><a name="quickstart-details"></a>Zahrnuté metadata v definici generovaného</span><span class="sxs-lookup"><span data-stu-id="ecd11-123"><a name="quickstart-details"></a>Included metadata in the generated definition</span></span>

<span data-ttu-id="ecd11-124">Následující tabulka představuje nastavení portálu Azure a odpovídajících dat v function.json, jak je mapován na generovaného kostru Swagger.</span><span class="sxs-lookup"><span data-stu-id="ecd11-124">The following table represents the Azure portal settings and corresponding data in function.json as it is mapped to the generated Swagger skeleton.</span></span>

|<span data-ttu-id="ecd11-125">Swagger.JSON</span><span class="sxs-lookup"><span data-stu-id="ecd11-125">Swagger.json</span></span>|<span data-ttu-id="ecd11-126">Portál uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="ecd11-126">Portal UI</span></span>|<span data-ttu-id="ecd11-127">Function.JSON</span><span class="sxs-lookup"><span data-stu-id="ecd11-127">Function.json</span></span>|
|:----|:-----|:-----|
|[<span data-ttu-id="ecd11-128">Hostitele</span><span class="sxs-lookup"><span data-stu-id="ecd11-128">Host</span></span>](http://swagger.io/specification/#fixed-fields-15)|<span data-ttu-id="ecd11-129">**Funkce nastavení aplikace** > **nastavení služby App Service** > **přehled** > **adresy URL**</span><span class="sxs-lookup"><span data-stu-id="ecd11-129">**Function app settings** > **App Service settings** > **Overview** > **URL**</span></span>|<span data-ttu-id="ecd11-130">*Není k dispozici*</span><span class="sxs-lookup"><span data-stu-id="ecd11-130">*Not present*</span></span>
|[<span data-ttu-id="ecd11-131">Cesty</span><span class="sxs-lookup"><span data-stu-id="ecd11-131">Paths</span></span>](http://swagger.io/specification/#paths-object-29)|<span data-ttu-id="ecd11-132">**Integrovat** > **metody vybrané HTTP**</span><span class="sxs-lookup"><span data-stu-id="ecd11-132">**Integrate** > **Selected HTTP methods**</span></span>|<span data-ttu-id="ecd11-133">Vazby: trasy</span><span class="sxs-lookup"><span data-stu-id="ecd11-133">Bindings: Route</span></span>
|[<span data-ttu-id="ecd11-134">Položky cesty</span><span class="sxs-lookup"><span data-stu-id="ecd11-134">Path Item</span></span>](http://swagger.io/specification/#path-item-object-32)|<span data-ttu-id="ecd11-135">**Integrovat** > **šablonu trasy**</span><span class="sxs-lookup"><span data-stu-id="ecd11-135">**Integrate** > **Route template**</span></span>|<span data-ttu-id="ecd11-136">Vazby: metody</span><span class="sxs-lookup"><span data-stu-id="ecd11-136">Bindings: Methods</span></span>
|[<span data-ttu-id="ecd11-137">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="ecd11-137">Security</span></span>](http://swagger.io/specification/#security-scheme-object-112)|<span data-ttu-id="ecd11-138">**Klíče**</span><span class="sxs-lookup"><span data-stu-id="ecd11-138">**Keys**</span></span>|<span data-ttu-id="ecd11-139">*Není k dispozici*</span><span class="sxs-lookup"><span data-stu-id="ecd11-139">*Not present*</span></span>|
|<span data-ttu-id="ecd11-140">operationID *</span><span class="sxs-lookup"><span data-stu-id="ecd11-140">operationID*</span></span>|<span data-ttu-id="ecd11-141">**Trasy + povolené příkazy**</span><span class="sxs-lookup"><span data-stu-id="ecd11-141">**Route + Allowed verbs**</span></span>|<span data-ttu-id="ecd11-142">Trasy + povolených příkazů</span><span class="sxs-lookup"><span data-stu-id="ecd11-142">Route + Allowed Verbs</span></span>|

<span data-ttu-id="ecd11-143">\*ID operace se vyžaduje jenom pro integraci s PowerApps a toku.</span><span class="sxs-lookup"><span data-stu-id="ecd11-143">\*The operation ID is required only for integrating with PowerApps and Flow.</span></span>
> [!NOTE]
> <span data-ttu-id="ecd11-144">Rozšíření x-ms souhrn poskytuje zobrazovaný název Logic Apps, PowerApps a toku.</span><span class="sxs-lookup"><span data-stu-id="ecd11-144">The x-ms-summary extension provides a display name in Logic Apps, PowerApps, and Flow.</span></span>
>
> <span data-ttu-id="ecd11-145">Další informace najdete v tématu [přizpůsobit vaší definici Swaggeru pro PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/).</span><span class="sxs-lookup"><span data-stu-id="ecd11-145">To learn more, see [Customize your Swagger definition for PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/).</span></span>

## <span data-ttu-id="ecd11-146"><a name="CICD"></a>Použití CI/CD nastavení definice rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ecd11-146"><a name="CICD"></a>Use CI/CD to set an API definition</span></span>

 <span data-ttu-id="ecd11-147">Je nutné povolit definice rozhraní API hostování portálu před povolením zdrojového kódu k úpravě vaše definice rozhraní API od správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="ecd11-147">You must enable API definition hosting in the portal before you enable source control to modify your API definition from source control.</span></span> <span data-ttu-id="ecd11-148">Postupujte podle těchto pokynů:</span><span class="sxs-lookup"><span data-stu-id="ecd11-148">Follow these instructions:</span></span>

1. <span data-ttu-id="ecd11-149">Přejděte do **definice rozhraní API (preview)** v aplikaci nastavení funkce.</span><span class="sxs-lookup"><span data-stu-id="ecd11-149">Browse to **API Definition (preview)** in your function app settings.</span></span>
  1. <span data-ttu-id="ecd11-150">Nastavit **zdroj definice rozhraní API** k **funkce**.</span><span class="sxs-lookup"><span data-stu-id="ecd11-150">Set **API definition source** to **Function**.</span></span>
  1. <span data-ttu-id="ecd11-151">Klikněte na tlačítko **šablony definice rozhraní API generovat** a potom **Uložit** k vytvoření definice šablony úpravy později.</span><span class="sxs-lookup"><span data-stu-id="ecd11-151">Click **Generate API definition template** and then **Save** to create a template definition for modifying later.</span></span>
  1. <span data-ttu-id="ecd11-152">Poznámka: adresa URL definice rozhraní API a klíč.</span><span class="sxs-lookup"><span data-stu-id="ecd11-152">Note your API definition URL and key.</span></span>
1. <span data-ttu-id="ecd11-153">[Nastavit nepřetržité integrace/průběžné nasazování (CI/CD)](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment#continuous-deployment-requirements).</span><span class="sxs-lookup"><span data-stu-id="ecd11-153">[Set up continuous integration/continuous deployment (CI/CD)](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment#continuous-deployment-requirements).</span></span>
2. <span data-ttu-id="ecd11-154">Upravit swagger.json ve správě zdrojového kódu v \site\wwwroot\.azurefunctions\swagger\swagger.json.</span><span class="sxs-lookup"><span data-stu-id="ecd11-154">Modify swagger.json in source control at \site\wwwroot\.azurefunctions\swagger\swagger.json.</span></span>

<span data-ttu-id="ecd11-155">Nyní, jsou změny swagger.json ve svém úložišti hostované funkce aplikace na rozhraní API adresa URL definice a klíč, který jste si poznamenali v kroku 1.c.</span><span class="sxs-lookup"><span data-stu-id="ecd11-155">Now, changes to swagger.json in your repository are hosted by your function app at the API definition URL and key that you noted in step 1.c.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ecd11-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ecd11-156">Next steps</span></span>
* <span data-ttu-id="ecd11-157">[Kurz Začínáme](functions-api-definition-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="ecd11-157">[Getting started tutorial](functions-api-definition-getting-started.md).</span></span> <span data-ttu-id="ecd11-158">Zkuste naše návod zobrazíte definici OpenAPI v akci.</span><span class="sxs-lookup"><span data-stu-id="ecd11-158">Try our walkthrough to see an OpenAPI definition in action.</span></span>
* <span data-ttu-id="ecd11-159">[Azure úložiště GitHub funkce](https://github.com/Azure/Azure-Functions/).</span><span class="sxs-lookup"><span data-stu-id="ecd11-159">[Azure Functions GitHub repository](https://github.com/Azure/Azure-Functions/).</span></span> <span data-ttu-id="ecd11-160">Podívejte se na funkce úložiště pro vaše názory na preview podporu definice rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ecd11-160">Check out the Functions repository to give us feedback on the API definition support preview.</span></span> <span data-ttu-id="ecd11-161">Ujistěte se, problém Githubu nic, co chcete zobrazit aktualizovaný.</span><span class="sxs-lookup"><span data-stu-id="ecd11-161">Make a GitHub issue for anything you want to see updated.</span></span>
* <span data-ttu-id="ecd11-162">[Referenční informace pro vývojáře Azure Functions](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="ecd11-162">[Azure Functions developer reference](functions-reference.md).</span></span> <span data-ttu-id="ecd11-163">Další informace o kódování funkcí a definování triggerů a vazeb.</span><span class="sxs-lookup"><span data-stu-id="ecd11-163">Learn about coding functions and defining triggers and bindings.</span></span>
