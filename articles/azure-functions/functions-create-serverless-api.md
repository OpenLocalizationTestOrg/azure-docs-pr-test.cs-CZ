---
title: "Vytvoření bez serveru API pomocí Azure Functions | Microsoft Docs"
description: "Postup vytvoření bez serveru API pomocí Azure Functions"
services: functions
author: mattchenderson
manager: erikre
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: tutorial
ms.date: 05/04/2017
ms.author: mahender
ms.custom: mvc
ms.openlocfilehash: 28056a385b058f7daeca2253ccb304116b49eba0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-serverless-api-using-azure-functions"></a><span data-ttu-id="d5a7c-103">Vytvoření bez serveru API pomocí Azure Functions</span><span class="sxs-lookup"><span data-stu-id="d5a7c-103">Create a serverless API using Azure Functions</span></span>

<span data-ttu-id="d5a7c-104">V tomto kurzu se dozvíte, jak Azure Functions umožňuje vytvářet škálovatelné rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-104">In this tutorial you will learn how Azure Functions allows you to build highly-scalable APIs.</span></span> <span data-ttu-id="d5a7c-105">Azure Functions se dodává s kolekce integrované HTTP triggerů a vazeb, které usnadňují vytváření koncový bod v různých jazycích, včetně Node.JS, C# a další.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-105">Azure Functions comes with a collection of built-in HTTP triggers and bindings which make it easy to author an endpoint in a variety of languages, including Node.JS, C#, and more.</span></span> <span data-ttu-id="d5a7c-106">V tomto kurzu budete přizpůsobovat aktivační procedury HTTP pro zpracování určitých akcí v návrhu rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-106">In this tutorial, you will customize an HTTP trigger to handle specific actions in your API design.</span></span> <span data-ttu-id="d5a7c-107">Také připravíte pro rostoucí vaše rozhraní API pomocí integrace s funkcí proxy Azure a nastavením imitované rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-107">You will also prepare for growing your API by integrating it with Azure Functions Proxies and setting up mock APIs.</span></span> <span data-ttu-id="d5a7c-108">Všechny tyto dosahuje nad výpočetním prostředí bez serveru funkce, takže nemusíte si dělat starosti o škálování prostředky – stačí se můžete soustředit na logice rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-108">All of this is accomplished on top of the Functions serverless compute environment, so you don't have to worry about scaling resources - you can just focus on your API logic.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5a7c-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d5a7c-109">Prerequisites</span></span> 

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

<span data-ttu-id="d5a7c-110">Výsledný funkce se použije pro zbytek tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-110">The resulting function will be used for the rest of this tutorial.</span></span>

### <a name="sign-in-to-azure"></a><span data-ttu-id="d5a7c-111">Přihlášení k Azure</span><span class="sxs-lookup"><span data-stu-id="d5a7c-111">Sign in to Azure</span></span>

<span data-ttu-id="d5a7c-112">Otevřete portál Azure.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-112">Open the Azure portal.</span></span> <span data-ttu-id="d5a7c-113">Chcete-li to provést, přihlaste se k [https://portal.azure.com](https://portal.azure.com) s vaším účtem Azure.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-113">To do this, sign in to [https://portal.azure.com](https://portal.azure.com) with your Azure account.</span></span>

## <a name="customize-your-http-function"></a><span data-ttu-id="d5a7c-114">Přizpůsobení funkce protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="d5a7c-114">Customize your HTTP function</span></span>

<span data-ttu-id="d5a7c-115">Ve výchozím nastavení je funkce aktivované protokolem HTTP nakonfigurovaná tak, aby přijímal libovolné metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-115">By default, your HTTP-triggered function is configured to accept any HTTP method.</span></span> <span data-ttu-id="d5a7c-116">Je také výchozí adresy URL ve formátu `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-116">There is also a default URL of the form `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`.</span></span> <span data-ttu-id="d5a7c-117">Pokud jste postupovali podle rychlé spuštění, pak `<funcname>` pravděpodobně vypadá podobně jako "HttpTriggerJS1".</span><span class="sxs-lookup"><span data-stu-id="d5a7c-117">If you followed the quickstart, then `<funcname>` probably looks something like "HttpTriggerJS1".</span></span> <span data-ttu-id="d5a7c-118">V této části upravíte funkce odpovídal pouze na požadavky GET proti `/api/hello` směrovat místo.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-118">In this section, you will modify the function to respond only to GET requests against `/api/hello` route instead.</span></span> 

<span data-ttu-id="d5a7c-119">Přejděte do funkce na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-119">Navigate to your function in the Azure portal.</span></span> <span data-ttu-id="d5a7c-120">Vyberte **integrací** v levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-120">Select **Integrate** in the left navigation.</span></span>

![Přizpůsobení funkce protokolu HTTP](./media/functions-create-serverless-api/customizing-http.png)

<span data-ttu-id="d5a7c-122">Použijte nastavení aktivační událost HTTP jako zadaný v tabulce.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-122">Use the HTTP trigger settings as specified in the table.</span></span>

| <span data-ttu-id="d5a7c-123">Pole</span><span class="sxs-lookup"><span data-stu-id="d5a7c-123">Field</span></span> | <span data-ttu-id="d5a7c-124">Ukázková hodnota</span><span class="sxs-lookup"><span data-stu-id="d5a7c-124">Sample value</span></span> | <span data-ttu-id="d5a7c-125">Popis</span><span class="sxs-lookup"><span data-stu-id="d5a7c-125">Description</span></span> |
|---|---|---|
| <span data-ttu-id="d5a7c-126">Povolených metod HTTP</span><span class="sxs-lookup"><span data-stu-id="d5a7c-126">Allowed HTTP methods</span></span> | <span data-ttu-id="d5a7c-127">Vybrané metody</span><span class="sxs-lookup"><span data-stu-id="d5a7c-127">Selected methods</span></span> | <span data-ttu-id="d5a7c-128">Určuje, jaké metody HTTP lze volat tuto funkci</span><span class="sxs-lookup"><span data-stu-id="d5a7c-128">Determines what HTTP methods may be used to invoke this function</span></span> |
| <span data-ttu-id="d5a7c-129">Vybrané metody HTTP</span><span class="sxs-lookup"><span data-stu-id="d5a7c-129">Selected HTTP methods</span></span> | <span data-ttu-id="d5a7c-130">GET</span><span class="sxs-lookup"><span data-stu-id="d5a7c-130">GET</span></span> | <span data-ttu-id="d5a7c-131">Umožňuje pouze vybrané HTTP metod, které bude použito ke volat tuto funkci</span><span class="sxs-lookup"><span data-stu-id="d5a7c-131">Allows only selected HTTP methods to be used to invoke this function</span></span> |
| <span data-ttu-id="d5a7c-132">Šablona trasy</span><span class="sxs-lookup"><span data-stu-id="d5a7c-132">Route template</span></span> | <span data-ttu-id="d5a7c-133">řetězec Ahoj</span><span class="sxs-lookup"><span data-stu-id="d5a7c-133">/hello</span></span> | <span data-ttu-id="d5a7c-134">Určuje, jaké trasy je použito k volání této funkce</span><span class="sxs-lookup"><span data-stu-id="d5a7c-134">Determines what route is used to invoke this function</span></span> |

<span data-ttu-id="d5a7c-135">Všimněte si, že nejsou zahrnuty `/api` základní cesta předpony v šabloně trasy, jak to se provádí globální nastavení.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-135">Note that you did not include the `/api` base path prefix in the route template, as this is handled by a global setting.</span></span>

<span data-ttu-id="d5a7c-136">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-136">Click **Save**.</span></span>

<span data-ttu-id="d5a7c-137">Další informace o přizpůsobení funkce protokolu HTTP v [vazby HTTP funkce Azure a webhooku](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).</span><span class="sxs-lookup"><span data-stu-id="d5a7c-137">You can learn more about customizing HTTP functions in [Azure Functions HTTP and webhook bindings](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).</span></span>

### <a name="test-your-api"></a><span data-ttu-id="d5a7c-138">Test rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d5a7c-138">Test your API</span></span>

<span data-ttu-id="d5a7c-139">V dalším kroku funkci nevidíte práce s novou plochy rozhraní API otestujte.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-139">Next, test your function to see it working with the new API surface.</span></span>

<span data-ttu-id="d5a7c-140">Přejděte zpět na stránku vývoj kliknutím na název funkce je v levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-140">Navigate back to the development page by clicking on the function's name in the left navigation.</span></span>

<span data-ttu-id="d5a7c-141">Klikněte na tlačítko **získat adresu URL funkce** a zkopírujte adresu URL.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-141">Click **Get function URL** and copy the URL.</span></span> <span data-ttu-id="d5a7c-142">Měli byste vidět, že používá `/api/hello` nyní trasy.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-142">You should see that it uses the `/api/hello` route now.</span></span>

<span data-ttu-id="d5a7c-143">Zkopírujte adresu URL do novou kartu prohlížeče nebo vaše upřednostňované klienta REST.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-143">Copy the URL into a new browser tab or your preferred REST client.</span></span> <span data-ttu-id="d5a7c-144">V prohlížečích se ve výchozím nastavení používá GET.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-144">Browsers will use GET by default.</span></span>

<span data-ttu-id="d5a7c-145">Spuštění funkce a potvrďte, že funguje.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-145">Run the function and confirm that it is working.</span></span> <span data-ttu-id="d5a7c-146">Musíte zadat parametr "název" jako řetězec dotazu tak, aby pokryl kód rychlý start.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-146">You may need to provide the "name" parameter as a query string to satisfy the quickstart code.</span></span>

<span data-ttu-id="d5a7c-147">Můžete také zkusit volání koncový bod s jinou metodu HTTP, potvrďte, že funkce není spuštěn.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-147">You can also try calling the endpoint with another HTTP method to confirm that the function is not executed.</span></span> <span data-ttu-id="d5a7c-148">V takovém případě budete muset použít klienta REST, například cURL, Postman nebo Fiddler.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-148">For this, you will need to use a REST client, such as cURL, Postman, or Fiddler.</span></span>

## <a name="proxies-overview"></a><span data-ttu-id="d5a7c-149">Přehled proxy</span><span class="sxs-lookup"><span data-stu-id="d5a7c-149">Proxies overview</span></span>

<span data-ttu-id="d5a7c-150">V další části bude surface vaše rozhraní API prostřednictvím proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-150">In the next section, you will surface your API through a proxy.</span></span> <span data-ttu-id="d5a7c-151">Azure proxy funkce je funkce preview, která umožňuje předat požadavky na jiné prostředky.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-151">Azure Functions Proxies is a preview feature that allows you to forward requests to other resources.</span></span> <span data-ttu-id="d5a7c-152">Můžete definovat koncový bod HTTP stejně jako s triggeru protokolu HTTP, ale místo psaní kódu pro spuštění, kdy se nazývá tohoto koncového bodu, zadejte adresu URL vzdálené implementaci.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-152">You define an HTTP endpoint just like with HTTP trigger, but instead of writing code to execute when that endpoint is called, you provide a URL to a remote implementation.</span></span> <span data-ttu-id="d5a7c-153">Umožňuje vytvořit více zdrojů rozhraní API do jednoho plochy rozhraní API, který je snadno klientům využívat.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-153">This allows you to compose multiple API sources into a single API surface which is easy for clients to consume.</span></span> <span data-ttu-id="d5a7c-154">To je zvlášť užitečné, pokud chcete vytvořit rozhraní API jako mikroslužeb.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-154">This is particularly useful if you wish to build your API as microservices.</span></span>

<span data-ttu-id="d5a7c-155">Proxy server může ukazovat na jakémukoli prostředku, HTTP, jako například:</span><span class="sxs-lookup"><span data-stu-id="d5a7c-155">A proxy can point to any HTTP resource, such as:</span></span>
- <span data-ttu-id="d5a7c-156">Funkce Azure</span><span class="sxs-lookup"><span data-stu-id="d5a7c-156">Azure Functions</span></span> 
- <span data-ttu-id="d5a7c-157">API apps v [služby Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)</span><span class="sxs-lookup"><span data-stu-id="d5a7c-157">API apps in [Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)</span></span>
- <span data-ttu-id="d5a7c-158">Kontejnery docker v [služby App Service v systému Linux](https://docs.microsoft.com/azure/app-service/app-service-linux-readme)</span><span class="sxs-lookup"><span data-stu-id="d5a7c-158">Docker containers in [App Service on Linux](https://docs.microsoft.com/azure/app-service/app-service-linux-readme)</span></span>
- <span data-ttu-id="d5a7c-159">Další hostovaným rozhraním API</span><span class="sxs-lookup"><span data-stu-id="d5a7c-159">Any other hosted API</span></span>

<span data-ttu-id="d5a7c-160">Další informace o proxy najdete v tématu [práce s funkcí proxy Azure (preview)].</span><span class="sxs-lookup"><span data-stu-id="d5a7c-160">To learn more about proxies, see [Working with Azure Functions Proxies (preview)].</span></span>

## <a name="create-your-first-proxy"></a><span data-ttu-id="d5a7c-161">Vytvoření vašeho prvního proxy</span><span class="sxs-lookup"><span data-stu-id="d5a7c-161">Create your first proxy</span></span>

<span data-ttu-id="d5a7c-162">V této části vytvoříte nový proxy, který slouží jako front-end pro celkový rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-162">In this section, you will create a new proxy which serves as a frontend to your overall API.</span></span> 

### <a name="setting-up-the-frontend-environment"></a><span data-ttu-id="d5a7c-163">Nastavení prostředí front-endu</span><span class="sxs-lookup"><span data-stu-id="d5a7c-163">Setting up the frontend environment</span></span>

<span data-ttu-id="d5a7c-164">Opakováním kroků [vytvořit aplikaci function app](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) k vytvoření nové aplikace funkce, ve kterém vytvoříte proxy.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-164">Repeat the steps to [Create a function app](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) to create a new function app in which you will create your proxy.</span></span> <span data-ttu-id="d5a7c-165">Toto nové aplikace bude sloužit jako front-endu pro naše API a funkce aplikace, kterou jste dříve upravovali bude sloužit jako back-end.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-165">This new app will serve as the frontend for our API, and the function app you were previously editing will serve as a backend.</span></span>

<span data-ttu-id="d5a7c-166">Přejděte do nové aplikace funkce front-endu na portálu.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-166">Navigate to your new frontend function app in the portal.</span></span>

<span data-ttu-id="d5a7c-167">Vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-167">Select **Settings**.</span></span> <span data-ttu-id="d5a7c-168">Potom přepněte **povolit funkce proxy Azure (preview)** na "On".</span><span class="sxs-lookup"><span data-stu-id="d5a7c-168">Then toggle **Enable Azure Functions Proxies (preview)** to "On".</span></span>

<span data-ttu-id="d5a7c-169">Vyberte **nastavení platformy** a zvolte **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-169">Select **Platform Settings** and choose **Application Settings**.</span></span>

<span data-ttu-id="d5a7c-170">Přejděte dolů k položce **nastavení aplikace** a vytvořit nové nastavení s klíčem "HELLO_HOST".</span><span class="sxs-lookup"><span data-stu-id="d5a7c-170">Scroll down to **App settings** and create a new setting with key "HELLO_HOST".</span></span> <span data-ttu-id="d5a7c-171">Jeho hodnotu nastavte hostitele back-end funkce aplikace, jako například `<YourApp>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-171">Set its value to the host of your backend function app, such as `<YourApp>.azurewebsites.net`.</span></span> <span data-ttu-id="d5a7c-172">Toto je část adresy URL, který jste zkopírovali dříve při testování funkce protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-172">This is part of the URL that you copied earlier when testing your HTTP function.</span></span> <span data-ttu-id="d5a7c-173">Toto nastavení v konfiguraci později budete odkazovat.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-173">You'll reference this setting in the configuration later.</span></span>

> [!NOTE] 
> <span data-ttu-id="d5a7c-174">Nastavení aplikace – se doporučují pro konfigurace hostitele, aby se zabránilo závislost pevně prostředí pro proxy server.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-174">App settings are recommended for the host configuration to prevent a hard-coded environment dependency for the proxy.</span></span> <span data-ttu-id="d5a7c-175">Pomocí nastavení aplikace znamená, že můžete přesunout konfiguraci proxy serveru mezi prostředími a nastavení prostředí aplikací se použijí.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-175">Using app settings means that you can move the proxy configuration between environments, and the environment-specific app settings will be applied.</span></span>

<span data-ttu-id="d5a7c-176">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-176">Click **Save**.</span></span>

### <a name="creating-a-proxy-on-the-frontend"></a><span data-ttu-id="d5a7c-177">Vytváření proxy server na front-endu</span><span class="sxs-lookup"><span data-stu-id="d5a7c-177">Creating a proxy on the frontend</span></span>

<span data-ttu-id="d5a7c-178">Přejděte zpět do aplikace front-endu funkce na portálu.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-178">Navigate back to your frontend function app in the portal.</span></span>

<span data-ttu-id="d5a7c-179">V levém navigačním panelu, klikněte na symbol plus '+' vedle "Proxy (preview)".</span><span class="sxs-lookup"><span data-stu-id="d5a7c-179">In the left-hand navigation, click the plus sign '+' next to "Proxies (preview)".</span></span>

![Vytváření proxy server](./media/functions-create-serverless-api/creating-proxy.png)

<span data-ttu-id="d5a7c-181">Použijte nastavení proxy serveru specifikovaný v tabulce.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-181">Use proxy settings as specified in the table.</span></span>

| <span data-ttu-id="d5a7c-182">Pole</span><span class="sxs-lookup"><span data-stu-id="d5a7c-182">Field</span></span> | <span data-ttu-id="d5a7c-183">Ukázková hodnota</span><span class="sxs-lookup"><span data-stu-id="d5a7c-183">Sample value</span></span> | <span data-ttu-id="d5a7c-184">Popis</span><span class="sxs-lookup"><span data-stu-id="d5a7c-184">Description</span></span> |
|---|---|---|
| <span data-ttu-id="d5a7c-185">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="d5a7c-185">Name</span></span> | <span data-ttu-id="d5a7c-186">HelloProxy</span><span class="sxs-lookup"><span data-stu-id="d5a7c-186">HelloProxy</span></span> | <span data-ttu-id="d5a7c-187">Popisný název, který se používá pouze pro správu</span><span class="sxs-lookup"><span data-stu-id="d5a7c-187">A friendly name used only for management</span></span> |
| <span data-ttu-id="d5a7c-188">Šablona trasy</span><span class="sxs-lookup"><span data-stu-id="d5a7c-188">Route template</span></span> | <span data-ttu-id="d5a7c-189">/ api/hello</span><span class="sxs-lookup"><span data-stu-id="d5a7c-189">/api/hello</span></span> | <span data-ttu-id="d5a7c-190">Určuje, jaké trasy se použije k vyvolání tento proxy server</span><span class="sxs-lookup"><span data-stu-id="d5a7c-190">Determines what route is used to invoke this proxy</span></span> |
| <span data-ttu-id="d5a7c-191">Adresa URL back-end</span><span class="sxs-lookup"><span data-stu-id="d5a7c-191">Backend URL</span></span> | <span data-ttu-id="d5a7c-192">https://%HELLO_HOST%/API/Hello</span><span class="sxs-lookup"><span data-stu-id="d5a7c-192">https://%HELLO_HOST%/api/hello</span></span> | <span data-ttu-id="d5a7c-193">Určuje koncový bod, ke kterému by měla být žádost směrovány přes proxy server</span><span class="sxs-lookup"><span data-stu-id="d5a7c-193">Specifies the endpoint to which the request should be proxied</span></span> |

<span data-ttu-id="d5a7c-194">Všimněte si, že proxy neposkytuje `/api` základní cesta předponu a tento musí být součástí šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-194">Note that Proxies does not provide the `/api` base path prefix, and this must be included in the route template.</span></span>

<span data-ttu-id="d5a7c-195">`%HELLO_HOST%` Syntaxe bude odkazovat nastavení aplikace, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-195">The `%HELLO_HOST%` syntax will reference the app setting you created earlier.</span></span> <span data-ttu-id="d5a7c-196">Přeložit adresy URL bude odkazovat na původní funkce.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-196">The resolved URL will point to your original function.</span></span>

<span data-ttu-id="d5a7c-197">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-197">Click **Create**.</span></span>

<span data-ttu-id="d5a7c-198">Váš nový proxy server můžete vyzkoušet tak, že adresa URL proxy serveru a jeho otestováním v prohlížeči nebo pomocí vašeho oblíbeného klienta HTTP.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-198">You can try out your new proxy by copying the Proxy URL and testing it in the browser or with your favorite HTTP client.</span></span>

## <a name="create-a-mock-api"></a><span data-ttu-id="d5a7c-199">Vytvoření imitované rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d5a7c-199">Create a mock API</span></span>

<span data-ttu-id="d5a7c-200">V dalším kroku použijete proxy serveru k vytvoření imitované rozhraní API pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-200">Next, you will use a proxy to create a mock API for your solution.</span></span> <span data-ttu-id="d5a7c-201">To umožňuje vývoje klienta pokroku, bez nutnosti back-end plně implementováno.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-201">This allows client development to progress, without needing the backend fully implemented.</span></span> <span data-ttu-id="d5a7c-202">Později v vývoj může vytvořit novou aplikaci funkce, která podporuje tuto logiku a přesměrování proxy k němu.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-202">Later in development, you could create a new function app which supports this logic and redirect your proxy to it.</span></span>

<span data-ttu-id="d5a7c-203">Pokud chcete vytvořit toto imitované rozhraní API, vytvoříme nový proxy, toto současně pomocí [Editor služby aplikace](https://github.com/projectkudu/kudu/wiki/App-Service-Editor).</span><span class="sxs-lookup"><span data-stu-id="d5a7c-203">To create this mock API, we will create a new proxy, this time using the [App Service Editor](https://github.com/projectkudu/kudu/wiki/App-Service-Editor).</span></span> <span data-ttu-id="d5a7c-204">Chcete-li začít, přejděte na funkce aplikace v portálu.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-204">To get started, navigate to your function app in the portal.</span></span> <span data-ttu-id="d5a7c-205">Vyberte **funkce** a najít **App Service Editor**.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-205">Select **Platform features** and find **App Service Editor**.</span></span> <span data-ttu-id="d5a7c-206">To kliknutím otevřete Editor služby aplikace na nové kartě.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-206">Clicking this will open the App Service Editor in a new tab.</span></span>

<span data-ttu-id="d5a7c-207">Vyberte `proxies.json` v levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-207">Select `proxies.json` in the left navigation.</span></span> <span data-ttu-id="d5a7c-208">Toto je soubor, který ukládá konfiguraci pro všechny vaše servery proxy.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-208">This is the file which stores the configuration for all of your proxies.</span></span> <span data-ttu-id="d5a7c-209">Pokud použijete jednu z [funkce metody nasazení](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), toto je soubor bude uchovávat ve správě zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-209">If you use one of the [Functions deployment methods](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), this is the file you will maintain in source control.</span></span> <span data-ttu-id="d5a7c-210">Další informace o tento soubor naleznete v tématu [proxy Upřesnit konfiguraci](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).</span><span class="sxs-lookup"><span data-stu-id="d5a7c-210">To learn more about this file, see [Proxies advanced configuration](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).</span></span>

<span data-ttu-id="d5a7c-211">Pokud jste jste postupovali podle dosavadní, vaše proxies.json by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="d5a7c-211">If you've followed along so far, your proxies.json should look like the following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        }
    }
}
```

<span data-ttu-id="d5a7c-212">Dále přidáte imitované rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-212">Next you'll add your mock API.</span></span> <span data-ttu-id="d5a7c-213">Nahraďte souboru proxies.json s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="d5a7c-213">Replace your proxies.json file with the following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        },
        "GetUserByName" : {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/users/{username}"
            },
            "responseOverrides": {
                "response.statusCode": "200",
                "response.headers.Content-Type" : "application/json",
                "response.body": {
                    "name": "{username}",
                    "description": "Awesome developer and master of serverless APIs",
                    "skills": [
                        "Serverless",
                        "APIs",
                        "Azure",
                        "Cloud"
                    ]
                }
            }
        }
    }
}
```

<span data-ttu-id="d5a7c-214">Tento postup přidá nový proxy, "GetUserByName", bez backendUri vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-214">This adds a new proxy, "GetUserByName", without the backendUri property.</span></span> <span data-ttu-id="d5a7c-215">Namísto volání jiný prostředek, upraví odpověď výchozí proxy pomocí přepsání odpovědi.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-215">Instead of calling another resource, it modifies the default response from Proxies using a response override.</span></span> <span data-ttu-id="d5a7c-216">Přepsání požadavku a odpovědi mohou sloužit také ve spojení s adresou URL back-end.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-216">Request and response overrides can also be used in conjunction with a backend URL.</span></span> <span data-ttu-id="d5a7c-217">To je zvlášť užitečné, když připojení přes proxy server na starší systém, kde budete muset upravit hlavičky, dotaz parametry atd. Další informace o požadavku a odpovědi přepsání najdete v tématu [úprava požadavky a odpovědi v proxy](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).</span><span class="sxs-lookup"><span data-stu-id="d5a7c-217">This is particularly useful when proxying to a legacy system, where you might need to modify headers, query parameters, etc. To learn more about request and response overrides, see [Modifying requests and responses in Proxies](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).</span></span>

<span data-ttu-id="d5a7c-218">Testovat imitované rozhraní API voláním `/api/users/{username}` koncového bodu pomocí prohlížeče nebo vaše oblíbené klienta REST.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-218">Test your mock API by calling the `/api/users/{username}` endpoint using a browser or your favorite REST client.</span></span> <span data-ttu-id="d5a7c-219">Nezapomeňte nahradit _{username}_ s řetězcovou hodnotu představující uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-219">Be sure to replace _{username}_ with a string value representing a username.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5a7c-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5a7c-220">Next steps</span></span>

<span data-ttu-id="d5a7c-221">V tomto kurzu jste zjistili, jak vytvářet a přizpůsobit rozhraní API na Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-221">In this tutorial, you learned how to build and customize an API on Azure Functions.</span></span> <span data-ttu-id="d5a7c-222">Také jste zjistili, jak probouzet několik rozhraní API, včetně mocks, společně jako jednotné rozhraní API prostor.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-222">You also learned how to bring multiple APIs, including mocks, together as a unified API surface.</span></span> <span data-ttu-id="d5a7c-223">Tyto postupy můžete použít k sestavení se rozhraní API jakékoli složitosti při spuštění v modelu bez serveru výpočetní poskytované Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="d5a7c-223">You can use these techniques to build out APIs of any complexity, all while running on the serverless compute model provided by Azure Functions.</span></span>

<span data-ttu-id="d5a7c-224">Následující odkazy mohou být užitečné, když budete vyvíjet další rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="d5a7c-224">The following references may be helpful as you develop your API further:</span></span>

- [<span data-ttu-id="d5a7c-225">Azure funkce protokolu HTTP a webhooku vazby</span><span class="sxs-lookup"><span data-stu-id="d5a7c-225">Azure Functions HTTP and webhook bindings</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook)
- <span data-ttu-id="d5a7c-226">[práce s funkcí proxy Azure (preview)]</span><span class="sxs-lookup"><span data-stu-id="d5a7c-226">[Working with Azure Functions Proxies (preview)]</span></span>
- [<span data-ttu-id="d5a7c-227">Dokumentace rozhraní API funkce Azure (preview)</span><span class="sxs-lookup"><span data-stu-id="d5a7c-227">Documenting an Azure Functions API (preview)</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-api-definition-getting-started)


[Create your first function]: https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function
[práce s funkcí proxy Azure (preview)]: https://docs.microsoft.com/azure/azure-functions/functions-proxies
