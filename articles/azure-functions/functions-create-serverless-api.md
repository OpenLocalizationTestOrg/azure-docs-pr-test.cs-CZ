---
title: "aaaCreate bez serveru API pomocí Azure Functions | Microsoft Docs"
description: "Jak toocreate bez serveru API pomocí Azure Functions"
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
ms.openlocfilehash: 877e3b229d5477fc5fec594ccd284fb55d7f3c07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-serverless-api-using-azure-functions"></a><span data-ttu-id="df790-103">Vytvoření bez serveru API pomocí Azure Functions</span><span class="sxs-lookup"><span data-stu-id="df790-103">Create a serverless API using Azure Functions</span></span>

<span data-ttu-id="df790-104">V tomto kurzu se dozvíte, jak Azure Functions umožňuje toobuild vysoce škálovatelné rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="df790-104">In this tutorial you will learn how Azure Functions allows you toobuild highly-scalable APIs.</span></span> <span data-ttu-id="df790-105">Azure Functions se dodává s kolekce integrované HTTP triggerů a vazeb, které umožňují snadno tooauthor koncový bod v různých jazycích, včetně Node.JS, C# a další.</span><span class="sxs-lookup"><span data-stu-id="df790-105">Azure Functions comes with a collection of built-in HTTP triggers and bindings which make it easy tooauthor an endpoint in a variety of languages, including Node.JS, C#, and more.</span></span> <span data-ttu-id="df790-106">V tomto kurzu budete přizpůsobovat toohandle aktivace protokolu HTTP pro konkrétní akce při návrhu rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="df790-106">In this tutorial, you will customize an HTTP trigger toohandle specific actions in your API design.</span></span> <span data-ttu-id="df790-107">Také připravíte pro rostoucí vaše rozhraní API pomocí integrace s funkcí proxy Azure a nastavením imitované rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="df790-107">You will also prepare for growing your API by integrating it with Azure Functions Proxies and setting up mock APIs.</span></span> <span data-ttu-id="df790-108">Všechny tyto dosahuje nad hello funkce bez serveru výpočetním prostředí, takže nemáte tooworry o škálování prostředky – stačí se můžete soustředit na logice rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="df790-108">All of this is accomplished on top of hello Functions serverless compute environment, so you don't have tooworry about scaling resources - you can just focus on your API logic.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df790-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="df790-109">Prerequisites</span></span> 

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

<span data-ttu-id="df790-110">Funkce výsledná Hello se použije pro hello zbytek tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="df790-110">hello resulting function will be used for hello rest of this tutorial.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="df790-111">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="df790-111">Sign in tooAzure</span></span>

<span data-ttu-id="df790-112">Otevřete hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="df790-112">Open hello Azure portal.</span></span> <span data-ttu-id="df790-113">toodo, přihlaste se příliš[https://portal.azure.com](https://portal.azure.com) s vaším účtem Azure.</span><span class="sxs-lookup"><span data-stu-id="df790-113">toodo this, sign in too[https://portal.azure.com](https://portal.azure.com) with your Azure account.</span></span>

## <a name="customize-your-http-function"></a><span data-ttu-id="df790-114">Přizpůsobení funkce protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="df790-114">Customize your HTTP function</span></span>

<span data-ttu-id="df790-115">Ve výchozím nastavení, je funkce aktivované protokolem HTTP tooaccept nakonfigurovaný libovolné metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="df790-115">By default, your HTTP-triggered function is configured tooaccept any HTTP method.</span></span> <span data-ttu-id="df790-116">Je také výchozí adresa URL hello formátu `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`.</span><span class="sxs-lookup"><span data-stu-id="df790-116">There is also a default URL of hello form `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`.</span></span> <span data-ttu-id="df790-117">Pokud jste postupovali podle hello rychlý start, pak `<funcname>` pravděpodobně vypadá podobně jako "HttpTriggerJS1".</span><span class="sxs-lookup"><span data-stu-id="df790-117">If you followed hello quickstart, then `<funcname>` probably looks something like "HttpTriggerJS1".</span></span> <span data-ttu-id="df790-118">V této části upravíte hello funkce toorespond pouze tooGET požadavky DHCP proti `/api/hello` směrovat místo.</span><span class="sxs-lookup"><span data-stu-id="df790-118">In this section, you will modify hello function toorespond only tooGET requests against `/api/hello` route instead.</span></span> 

<span data-ttu-id="df790-119">Přejděte tooyour funkce v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="df790-119">Navigate tooyour function in hello Azure portal.</span></span> <span data-ttu-id="df790-120">Vyberte **integrací** v levé navigační hello.</span><span class="sxs-lookup"><span data-stu-id="df790-120">Select **Integrate** in hello left navigation.</span></span>

![Přizpůsobení funkce protokolu HTTP](./media/functions-create-serverless-api/customizing-http.png)

<span data-ttu-id="df790-122">Použijte nastavení aktivační událost HTTP jako tabulka hello.</span><span class="sxs-lookup"><span data-stu-id="df790-122">Use the HTTP trigger settings as specified in hello table.</span></span>

| <span data-ttu-id="df790-123">Pole</span><span class="sxs-lookup"><span data-stu-id="df790-123">Field</span></span> | <span data-ttu-id="df790-124">Ukázková hodnota</span><span class="sxs-lookup"><span data-stu-id="df790-124">Sample value</span></span> | <span data-ttu-id="df790-125">Popis</span><span class="sxs-lookup"><span data-stu-id="df790-125">Description</span></span> |
|---|---|---|
| <span data-ttu-id="df790-126">Povolených metod HTTP</span><span class="sxs-lookup"><span data-stu-id="df790-126">Allowed HTTP methods</span></span> | <span data-ttu-id="df790-127">Vybrané metody</span><span class="sxs-lookup"><span data-stu-id="df790-127">Selected methods</span></span> | <span data-ttu-id="df790-128">Určuje, jaké metody HTTP, může být použité tooinvoke této funkce</span><span class="sxs-lookup"><span data-stu-id="df790-128">Determines what HTTP methods may be used tooinvoke this function</span></span> |
| <span data-ttu-id="df790-129">Vybrané metody HTTP</span><span class="sxs-lookup"><span data-stu-id="df790-129">Selected HTTP methods</span></span> | <span data-ttu-id="df790-130">GET</span><span class="sxs-lookup"><span data-stu-id="df790-130">GET</span></span> | <span data-ttu-id="df790-131">Umožňuje pouze vybrané toobe metody HTTP použili tooinvoke tuto funkci</span><span class="sxs-lookup"><span data-stu-id="df790-131">Allows only selected HTTP methods toobe used tooinvoke this function</span></span> |
| <span data-ttu-id="df790-132">Šablona trasy</span><span class="sxs-lookup"><span data-stu-id="df790-132">Route template</span></span> | <span data-ttu-id="df790-133">řetězec Ahoj</span><span class="sxs-lookup"><span data-stu-id="df790-133">/hello</span></span> | <span data-ttu-id="df790-134">Určuje, jaké trasy je použité tooinvoke této funkce</span><span class="sxs-lookup"><span data-stu-id="df790-134">Determines what route is used tooinvoke this function</span></span> |

<span data-ttu-id="df790-135">Všimněte si, že jste nezahrnuli hello `/api` základní cesta předponu hello šablonu trasy, jak to se provádí globální nastavení.</span><span class="sxs-lookup"><span data-stu-id="df790-135">Note that you did not include hello `/api` base path prefix in hello route template, as this is handled by a global setting.</span></span>

<span data-ttu-id="df790-136">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="df790-136">Click **Save**.</span></span>

<span data-ttu-id="df790-137">Další informace o přizpůsobení funkce protokolu HTTP v [vazby HTTP funkce Azure a webhooku](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).</span><span class="sxs-lookup"><span data-stu-id="df790-137">You can learn more about customizing HTTP functions in [Azure Functions HTTP and webhook bindings](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).</span></span>

### <a name="test-your-api"></a><span data-ttu-id="df790-138">Test rozhraní API</span><span class="sxs-lookup"><span data-stu-id="df790-138">Test your API</span></span>

<span data-ttu-id="df790-139">V dalším kroku otestujte váš funkce toosee práce s hello nový prostor pro rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="df790-139">Next, test your function toosee it working with hello new API surface.</span></span>

<span data-ttu-id="df790-140">Přejděte zpět toohello vývoj stránku kliknutím na název funkce hello v levé navigační hello.</span><span class="sxs-lookup"><span data-stu-id="df790-140">Navigate back toohello development page by clicking on hello function's name in hello left navigation.</span></span>

<span data-ttu-id="df790-141">Klikněte na tlačítko **získat adresu URL funkce** a zkopírujte adresu URL hello.</span><span class="sxs-lookup"><span data-stu-id="df790-141">Click **Get function URL** and copy hello URL.</span></span> <span data-ttu-id="df790-142">Měli byste vidět, že používá hello `/api/hello` nyní trasy.</span><span class="sxs-lookup"><span data-stu-id="df790-142">You should see that it uses hello `/api/hello` route now.</span></span>

<span data-ttu-id="df790-143">Zkopírujte adresu URL hello do novou kartu prohlížeče nebo vaše upřednostňované klienta REST.</span><span class="sxs-lookup"><span data-stu-id="df790-143">Copy hello URL into a new browser tab or your preferred REST client.</span></span> <span data-ttu-id="df790-144">V prohlížečích se ve výchozím nastavení používá GET.</span><span class="sxs-lookup"><span data-stu-id="df790-144">Browsers will use GET by default.</span></span>

<span data-ttu-id="df790-145">Spusťte hello funkce a zkontrolujte, že funguje.</span><span class="sxs-lookup"><span data-stu-id="df790-145">Run hello function and confirm that it is working.</span></span> <span data-ttu-id="df790-146">Může být nutné tooprovide hello "název" parametr jako kód rychlý start hello toosatisfy řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="df790-146">You may need tooprovide hello "name" parameter as a query string toosatisfy hello quickstart code.</span></span>

<span data-ttu-id="df790-147">Můžete také zkusit volání hello koncový bod s jinou tooconfirm metoda HTTP že hello funkce není spuštěn.</span><span class="sxs-lookup"><span data-stu-id="df790-147">You can also try calling hello endpoint with another HTTP method tooconfirm that hello function is not executed.</span></span> <span data-ttu-id="df790-148">V takovém případě budete potřebovat toouse klienta REST, například cURL, Postman nebo Fiddler.</span><span class="sxs-lookup"><span data-stu-id="df790-148">For this, you will need toouse a REST client, such as cURL, Postman, or Fiddler.</span></span>

## <a name="proxies-overview"></a><span data-ttu-id="df790-149">Přehled proxy</span><span class="sxs-lookup"><span data-stu-id="df790-149">Proxies overview</span></span>

<span data-ttu-id="df790-150">V další části hello bude surface vaše rozhraní API prostřednictvím proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="df790-150">In hello next section, you will surface your API through a proxy.</span></span> <span data-ttu-id="df790-151">Azure proxy funkce je funkce preview, která vám umožní tooforward požadavky tooother prostředky.</span><span class="sxs-lookup"><span data-stu-id="df790-151">Azure Functions Proxies is a preview feature that allows you tooforward requests tooother resources.</span></span> <span data-ttu-id="df790-152">Můžete definovat koncový bod protokolu HTTP, podobně jako pomocí triggeru protokolu HTTP, ale místo psaní kódu tooexecute při volání tohoto koncového bodu, zadáte vzdálené implementaci tooa adresy URL.</span><span class="sxs-lookup"><span data-stu-id="df790-152">You define an HTTP endpoint just like with HTTP trigger, but instead of writing code tooexecute when that endpoint is called, you provide a URL tooa remote implementation.</span></span> <span data-ttu-id="df790-153">To vám umožní toocompose zdroje několik rozhraní API do jednoho plochy rozhraní API, který je snadno tooconsume klientů.</span><span class="sxs-lookup"><span data-stu-id="df790-153">This allows you toocompose multiple API sources into a single API surface which is easy for clients tooconsume.</span></span> <span data-ttu-id="df790-154">To je zvlášť užitečné, pokud chcete toobuild vaše rozhraní API jako mikroslužeb.</span><span class="sxs-lookup"><span data-stu-id="df790-154">This is particularly useful if you wish toobuild your API as microservices.</span></span>

<span data-ttu-id="df790-155">Proxy server můžete bodu tooany HTTP prostředků, například:</span><span class="sxs-lookup"><span data-stu-id="df790-155">A proxy can point tooany HTTP resource, such as:</span></span>
- <span data-ttu-id="df790-156">Funkce Azure</span><span class="sxs-lookup"><span data-stu-id="df790-156">Azure Functions</span></span> 
- <span data-ttu-id="df790-157">API apps v [služby Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)</span><span class="sxs-lookup"><span data-stu-id="df790-157">API apps in [Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)</span></span>
- <span data-ttu-id="df790-158">Kontejnery docker v [služby App Service v systému Linux](https://docs.microsoft.com/azure/app-service/app-service-linux-readme)</span><span class="sxs-lookup"><span data-stu-id="df790-158">Docker containers in [App Service on Linux](https://docs.microsoft.com/azure/app-service/app-service-linux-readme)</span></span>
- <span data-ttu-id="df790-159">Další hostovaným rozhraním API</span><span class="sxs-lookup"><span data-stu-id="df790-159">Any other hosted API</span></span>

<span data-ttu-id="df790-160">toolearn Další informace o proxy, najdete v části [práce s funkcí proxy Azure (preview)].</span><span class="sxs-lookup"><span data-stu-id="df790-160">toolearn more about proxies, see [Working with Azure Functions Proxies (preview)].</span></span>

## <a name="create-your-first-proxy"></a><span data-ttu-id="df790-161">Vytvoření vašeho prvního proxy</span><span class="sxs-lookup"><span data-stu-id="df790-161">Create your first proxy</span></span>

<span data-ttu-id="df790-162">V této části vytvoříte nový proxy které slouží jako front-endu tooyour celkové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="df790-162">In this section, you will create a new proxy which serves as a frontend tooyour overall API.</span></span> 

### <a name="setting-up-hello-frontend-environment"></a><span data-ttu-id="df790-163">Nastavení prostředí front-endu hello</span><span class="sxs-lookup"><span data-stu-id="df790-163">Setting up hello frontend environment</span></span>

<span data-ttu-id="df790-164">Opakujte kroky hello příliš[vytvořit aplikaci function app](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) toocreate novou aplikaci funkce v tím se vytvoří proxy.</span><span class="sxs-lookup"><span data-stu-id="df790-164">Repeat hello steps too[Create a function app](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) toocreate a new function app in which you will create your proxy.</span></span> <span data-ttu-id="df790-165">Toto nové aplikace bude sloužit jako hello front-endu pro naše API a hello funkce aplikace, kterou jste dříve upravovali bude sloužit jako back-end.</span><span class="sxs-lookup"><span data-stu-id="df790-165">This new app will serve as hello frontend for our API, and hello function app you were previously editing will serve as a backend.</span></span>

<span data-ttu-id="df790-166">Přejděte tooyour nové front-endu funkce aplikace hello portálu.</span><span class="sxs-lookup"><span data-stu-id="df790-166">Navigate tooyour new frontend function app in hello portal.</span></span>

<span data-ttu-id="df790-167">Vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="df790-167">Select **Settings**.</span></span> <span data-ttu-id="df790-168">Potom přepněte **povolit funkce proxy Azure (preview)** příliš "na".</span><span class="sxs-lookup"><span data-stu-id="df790-168">Then toggle **Enable Azure Functions Proxies (preview)** too"On".</span></span>

<span data-ttu-id="df790-169">Vyberte **nastavení platformy** a zvolte **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="df790-169">Select **Platform Settings** and choose **Application Settings**.</span></span>

<span data-ttu-id="df790-170">Posuňte se dolů příliš**nastavení aplikace** a vytvořit nové nastavení s klíčem "HELLO_HOST".</span><span class="sxs-lookup"><span data-stu-id="df790-170">Scroll down too**App settings** and create a new setting with key "HELLO_HOST".</span></span> <span data-ttu-id="df790-171">Nastavte jeho hodnota toohello hostitel back-end funkce aplikace, jako je třeba `<YourApp>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="df790-171">Set its value toohello host of your backend function app, such as `<YourApp>.azurewebsites.net`.</span></span> <span data-ttu-id="df790-172">Toto je část adresy URL hello, který jste zkopírovali dříve při testování funkce protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="df790-172">This is part of hello URL that you copied earlier when testing your HTTP function.</span></span> <span data-ttu-id="df790-173">Toto nastavení v konfiguraci hello později budete odkazovat.</span><span class="sxs-lookup"><span data-stu-id="df790-173">You'll reference this setting in hello configuration later.</span></span>

> [!NOTE] 
> <span data-ttu-id="df790-174">Nastavení aplikace – se doporučují pro tooprevent konfigurace hostitele hello závislost pevně prostředí pro proxy server hello.</span><span class="sxs-lookup"><span data-stu-id="df790-174">App settings are recommended for hello host configuration tooprevent a hard-coded environment dependency for hello proxy.</span></span> <span data-ttu-id="df790-175">Pomocí nastavení aplikace znamená, že můžete přesunout hello konfiguraci proxy serveru mezi prostředími a nastavení prostředí aplikací hello se použijí.</span><span class="sxs-lookup"><span data-stu-id="df790-175">Using app settings means that you can move hello proxy configuration between environments, and hello environment-specific app settings will be applied.</span></span>

<span data-ttu-id="df790-176">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="df790-176">Click **Save**.</span></span>

### <a name="creating-a-proxy-on-hello-frontend"></a><span data-ttu-id="df790-177">Vytváření proxy server na front-endu hello</span><span class="sxs-lookup"><span data-stu-id="df790-177">Creating a proxy on hello frontend</span></span>

<span data-ttu-id="df790-178">Přejděte zpět tooyour front-endu funkce aplikace hello portálu.</span><span class="sxs-lookup"><span data-stu-id="df790-178">Navigate back tooyour frontend function app in hello portal.</span></span>

<span data-ttu-id="df790-179">V levém navigačním hello, klikněte na tlačítko hello znak plus další '+' příliš "Proxy (preview)".</span><span class="sxs-lookup"><span data-stu-id="df790-179">In hello left-hand navigation, click hello plus sign '+' next too"Proxies (preview)".</span></span>

![Vytváření proxy server](./media/functions-create-serverless-api/creating-proxy.png)

<span data-ttu-id="df790-181">Nastavení proxy serveru použijte jako tabulka hello.</span><span class="sxs-lookup"><span data-stu-id="df790-181">Use proxy settings as specified in hello table.</span></span>

| <span data-ttu-id="df790-182">Pole</span><span class="sxs-lookup"><span data-stu-id="df790-182">Field</span></span> | <span data-ttu-id="df790-183">Ukázková hodnota</span><span class="sxs-lookup"><span data-stu-id="df790-183">Sample value</span></span> | <span data-ttu-id="df790-184">Popis</span><span class="sxs-lookup"><span data-stu-id="df790-184">Description</span></span> |
|---|---|---|
| <span data-ttu-id="df790-185">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="df790-185">Name</span></span> | <span data-ttu-id="df790-186">HelloProxy</span><span class="sxs-lookup"><span data-stu-id="df790-186">HelloProxy</span></span> | <span data-ttu-id="df790-187">Popisný název, který se používá pouze pro správu</span><span class="sxs-lookup"><span data-stu-id="df790-187">A friendly name used only for management</span></span> |
| <span data-ttu-id="df790-188">Šablona trasy</span><span class="sxs-lookup"><span data-stu-id="df790-188">Route template</span></span> | <span data-ttu-id="df790-189">/ api/hello</span><span class="sxs-lookup"><span data-stu-id="df790-189">/api/hello</span></span> | <span data-ttu-id="df790-190">Určuje, jaké trasy je použité tooinvoke tento proxy server</span><span class="sxs-lookup"><span data-stu-id="df790-190">Determines what route is used tooinvoke this proxy</span></span> |
| <span data-ttu-id="df790-191">Adresa URL back-end</span><span class="sxs-lookup"><span data-stu-id="df790-191">Backend URL</span></span> | <span data-ttu-id="df790-192">https://%HELLO_HOST%/API/Hello</span><span class="sxs-lookup"><span data-stu-id="df790-192">https://%HELLO_HOST%/api/hello</span></span> | <span data-ttu-id="df790-193">Určuje, že požadavku hello toowhich hello koncového bodu musí být směrovány přes proxy server</span><span class="sxs-lookup"><span data-stu-id="df790-193">Specifies hello endpoint toowhich hello request should be proxied</span></span> |

<span data-ttu-id="df790-194">Všimněte si, že proxy nenabízí hello `/api` základní cesta předponu a tento musí být součástí hello šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="df790-194">Note that Proxies does not provide hello `/api` base path prefix, and this must be included in hello route template.</span></span>

<span data-ttu-id="df790-195">Hello `%HELLO_HOST%` syntaxe bude odkazovat nastavení aplikace hello jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="df790-195">hello `%HELLO_HOST%` syntax will reference hello app setting you created earlier.</span></span> <span data-ttu-id="df790-196">Hello rozpoznat, že adresa URL bude odkazovat tooyour původní funkce.</span><span class="sxs-lookup"><span data-stu-id="df790-196">hello resolved URL will point tooyour original function.</span></span>

<span data-ttu-id="df790-197">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="df790-197">Click **Create**.</span></span>

<span data-ttu-id="df790-198">Můžete vyzkoušet nový proxy zkopírováním hello adresa URL proxy serveru a jeho otestováním v prohlížeči hello nebo pomocí vašeho oblíbeného klienta HTTP.</span><span class="sxs-lookup"><span data-stu-id="df790-198">You can try out your new proxy by copying hello Proxy URL and testing it in hello browser or with your favorite HTTP client.</span></span>

## <a name="create-a-mock-api"></a><span data-ttu-id="df790-199">Vytvoření imitované rozhraní API</span><span class="sxs-lookup"><span data-stu-id="df790-199">Create a mock API</span></span>

<span data-ttu-id="df790-200">V dalším kroku použijete proxy toocreate imitované rozhraní API pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="df790-200">Next, you will use a proxy toocreate a mock API for your solution.</span></span> <span data-ttu-id="df790-201">To umožňuje vývoje klienta tooprogress, bez nutnosti plně implementované back-end hello.</span><span class="sxs-lookup"><span data-stu-id="df790-201">This allows client development tooprogress, without needing hello backend fully implemented.</span></span> <span data-ttu-id="df790-202">Později v vývoj může vytvořit novou aplikaci funkce, která podporuje tuto logiku a přesměrování tooit vašeho proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="df790-202">Later in development, you could create a new function app which supports this logic and redirect your proxy tooit.</span></span>

<span data-ttu-id="df790-203">toocreate to model rozhraní API, vytvoříme nový proxy, tentokrát pomocí hello [App Service Editor](https://github.com/projectkudu/kudu/wiki/App-Service-Editor).</span><span class="sxs-lookup"><span data-stu-id="df790-203">toocreate this mock API, we will create a new proxy, this time using hello [App Service Editor](https://github.com/projectkudu/kudu/wiki/App-Service-Editor).</span></span> <span data-ttu-id="df790-204">tooget začít, přejděte tooyour funkce aplikace hello portálu.</span><span class="sxs-lookup"><span data-stu-id="df790-204">tooget started, navigate tooyour function app in hello portal.</span></span> <span data-ttu-id="df790-205">Vyberte **funkce** a najít **App Service Editor**.</span><span class="sxs-lookup"><span data-stu-id="df790-205">Select **Platform features** and find **App Service Editor**.</span></span> <span data-ttu-id="df790-206">To kliknutím otevřete hello Editor služby aplikace na nové kartě.</span><span class="sxs-lookup"><span data-stu-id="df790-206">Clicking this will open hello App Service Editor in a new tab.</span></span>

<span data-ttu-id="df790-207">Vyberte `proxies.json` v levé navigační hello.</span><span class="sxs-lookup"><span data-stu-id="df790-207">Select `proxies.json` in hello left navigation.</span></span> <span data-ttu-id="df790-208">Toto je hello souboru, který ukládá hello konfiguraci pro všechny vaše servery proxy.</span><span class="sxs-lookup"><span data-stu-id="df790-208">This is hello file which stores hello configuration for all of your proxies.</span></span> <span data-ttu-id="df790-209">Pokud použijete jeden hello [funkce metody nasazení](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), toto je soubor hello bude uchovávat ve správě zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="df790-209">If you use one of hello [Functions deployment methods](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), this is hello file you will maintain in source control.</span></span> <span data-ttu-id="df790-210">najdete v části Další informace o tento soubor toolearn [proxy Upřesnit konfiguraci](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).</span><span class="sxs-lookup"><span data-stu-id="df790-210">toolearn more about this file, see [Proxies advanced configuration](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).</span></span>

<span data-ttu-id="df790-211">Pokud jste jste postupovali podle dosavadní, vaše proxies.json by měl vypadat jako následující hello:</span><span class="sxs-lookup"><span data-stu-id="df790-211">If you've followed along so far, your proxies.json should look like hello following:</span></span>

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

<span data-ttu-id="df790-212">Dále přidáte imitované rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="df790-212">Next you'll add your mock API.</span></span> <span data-ttu-id="df790-213">Váš soubor proxies.json nahraďte hello následující:</span><span class="sxs-lookup"><span data-stu-id="df790-213">Replace your proxies.json file with hello following:</span></span>

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

<span data-ttu-id="df790-214">Tento postup přidá nový proxy, "GetUserByName", bez hello backendUri vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="df790-214">This adds a new proxy, "GetUserByName", without hello backendUri property.</span></span> <span data-ttu-id="df790-215">Namísto volání jiný prostředek, upraví hello výchozí odpověď od proxy pomocí přepsání odpovědi.</span><span class="sxs-lookup"><span data-stu-id="df790-215">Instead of calling another resource, it modifies hello default response from Proxies using a response override.</span></span> <span data-ttu-id="df790-216">Přepsání požadavku a odpovědi mohou sloužit také ve spojení s adresou URL back-end.</span><span class="sxs-lookup"><span data-stu-id="df790-216">Request and response overrides can also be used in conjunction with a backend URL.</span></span> <span data-ttu-id="df790-217">To je zvlášť užitečné, když se starší verze systému tooa připojení přes proxy server, kde je nutné toomodify hlavičky, parametry dotazu, toolearn atd. Další informace o požadavku a odpovědi přepsání, [úprava požadavky a odpovědi v proxy](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).</span><span class="sxs-lookup"><span data-stu-id="df790-217">This is particularly useful when proxying tooa legacy system, where you might need toomodify headers, query parameters, etc. toolearn more about request and response overrides, see [Modifying requests and responses in Proxies](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).</span></span>

<span data-ttu-id="df790-218">Test imitované rozhraní API volání hello `/api/users/{username}` koncového bodu pomocí prohlížeče nebo vaše oblíbené klienta REST.</span><span class="sxs-lookup"><span data-stu-id="df790-218">Test your mock API by calling hello `/api/users/{username}` endpoint using a browser or your favorite REST client.</span></span> <span data-ttu-id="df790-219">Být jisti tooreplace _{username}_ s řetězcovou hodnotu představující uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="df790-219">Be sure tooreplace _{username}_ with a string value representing a username.</span></span>

## <a name="next-steps"></a><span data-ttu-id="df790-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="df790-220">Next steps</span></span>

<span data-ttu-id="df790-221">V tomto kurzu jste se naučili jak toobuild a přizpůsobení rozhraní API na Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="df790-221">In this tutorial, you learned how toobuild and customize an API on Azure Functions.</span></span> <span data-ttu-id="df790-222">Také jste zjistili, jak toobring několik rozhraní API, včetně mocks, společně jako jednotné rozhraní API prostor.</span><span class="sxs-lookup"><span data-stu-id="df790-222">You also learned how toobring multiple APIs, including mocks, together as a unified API surface.</span></span> <span data-ttu-id="df790-223">Můžete použít tyto techniky toobuild se rozhraní API jakékoli složitosti, všechny při spuštění v hello bez serveru výpočetní model poskytované Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="df790-223">You can use these techniques toobuild out APIs of any complexity, all while running on hello serverless compute model provided by Azure Functions.</span></span>

<span data-ttu-id="df790-224">Hello následující odkazy může být užitečné, když budete vyvíjet další rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="df790-224">hello following references may be helpful as you develop your API further:</span></span>

- [<span data-ttu-id="df790-225">Azure funkce protokolu HTTP a webhooku vazby</span><span class="sxs-lookup"><span data-stu-id="df790-225">Azure Functions HTTP and webhook bindings</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook)
- <span data-ttu-id="df790-226">[práce s funkcí proxy Azure (preview)]</span><span class="sxs-lookup"><span data-stu-id="df790-226">[Working with Azure Functions Proxies (preview)]</span></span>
- [<span data-ttu-id="df790-227">Dokumentace rozhraní API funkce Azure (preview)</span><span class="sxs-lookup"><span data-stu-id="df790-227">Documenting an Azure Functions API (preview)</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-api-definition-getting-started)


[Create your first function]: https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function
[práce s funkcí proxy Azure (preview)]: https://docs.microsoft.com/azure/azure-functions/functions-proxies
