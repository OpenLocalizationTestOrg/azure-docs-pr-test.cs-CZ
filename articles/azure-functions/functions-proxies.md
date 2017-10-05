---
title: "Práce s proxy Azure Functions | Microsoft Docs"
description: "Přehled o tom, jak používat Azure funkce proxy"
services: functions
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/11/2017
ms.author: mahender
ms.openlocfilehash: 102e54627a8fee721d3ed85e86a8009e706bb5b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="work-with-azure-functions-proxies-preview"></a><span data-ttu-id="beb69-103">Práce s funkcí proxy Azure (preview)</span><span class="sxs-lookup"><span data-stu-id="beb69-103">Work with Azure Functions Proxies (preview)</span></span>

> [!NOTE] 
> <span data-ttu-id="beb69-104">Azure proxy funkce je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="beb69-104">Azure Functions Proxies is currently in preview.</span></span> <span data-ttu-id="beb69-105">Volné je během ve verzi preview, ale standardní funkce fakturace spuštěních proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="beb69-105">It is free while in preview, but standard Functions billing applies to proxy executions.</span></span> <span data-ttu-id="beb69-106">Další informace najdete v tématu [ceny Azure Functions](https://azure.microsoft.com/pricing/details/functions/).</span><span class="sxs-lookup"><span data-stu-id="beb69-106">For more information, see [Azure Functions pricing](https://azure.microsoft.com/pricing/details/functions/).</span></span>

<span data-ttu-id="beb69-107">Tento článek vysvětluje postup konfigurace a práce s Azure funkce proxy.</span><span class="sxs-lookup"><span data-stu-id="beb69-107">This article explains how to configure and work with Azure Functions Proxies.</span></span> <span data-ttu-id="beb69-108">Pomocí této funkce můžete určovat koncové body ve vaší aplikaci funkce, které jsou implementované pomocí jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="beb69-108">With this feature, you can specify endpoints on your function app that are implemented by another resource.</span></span> <span data-ttu-id="beb69-109">Tyto servery proxy pro přerušení velké rozhraní API do více aplikací – funkce (jako architektury mikroslužby), můžete použít při stále poskytovat jeden prostor rozhraní API pro klienty.</span><span class="sxs-lookup"><span data-stu-id="beb69-109">You can use these proxies to break a large API into multiple function apps (as in a microservice architecture), while still presenting a single API surface for clients.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]


## <span data-ttu-id="beb69-110"><a name="enable"></a>Povolit proxy Azure Functions</span><span class="sxs-lookup"><span data-stu-id="beb69-110"><a name="enable"></a>Enable Azure Functions Proxies</span></span>

<span data-ttu-id="beb69-111">Proxy nejsou povolené ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="beb69-111">Proxies are not enabled by default.</span></span> <span data-ttu-id="beb69-112">Tato funkce je zakázaná, ale spustit, můžete vytvořit proxy.</span><span class="sxs-lookup"><span data-stu-id="beb69-112">You can create proxies while the feature is disabled, but they will not execute.</span></span> <span data-ttu-id="beb69-113">Pokud chcete povolit proxy, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="beb69-113">To enable proxies, do the following:</span></span>

1. <span data-ttu-id="beb69-114">Otevřete [portál Azure]a poté přejděte k vaší aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="beb69-114">Open the [Azure portal], and then go to your function app.</span></span>
2. <span data-ttu-id="beb69-115">Vyberte **funkce nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="beb69-115">Select **Function app settings**.</span></span>
3. <span data-ttu-id="beb69-116">Přepínač **povolit funkce proxy Azure (preview)** k **na**.</span><span class="sxs-lookup"><span data-stu-id="beb69-116">Switch **Enable Azure Functions Proxies (preview)** to **On**.</span></span>

<span data-ttu-id="beb69-117">Můžete se taky vrátit zde o aktualizaci runtime proxy serveru je k dispozici nové funkce.</span><span class="sxs-lookup"><span data-stu-id="beb69-117">You can also return here to update the proxy runtime as new features become available.</span></span>


## <span data-ttu-id="beb69-118"><a name="create"></a>Vytvořit proxy</span><span class="sxs-lookup"><span data-stu-id="beb69-118"><a name="create"></a>Create a proxy</span></span>

<span data-ttu-id="beb69-119">V této části se dozvíte, jak vytvořit proxy na portálu funkce.</span><span class="sxs-lookup"><span data-stu-id="beb69-119">This section shows you how to create a proxy in the Functions portal.</span></span>

1. <span data-ttu-id="beb69-120">Otevřete [portál Azure]a poté přejděte k vaší aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="beb69-120">Open the [Azure portal], and then go to your function app.</span></span>
2. <span data-ttu-id="beb69-121">V levém podokně vyberte **nového proxy**.</span><span class="sxs-lookup"><span data-stu-id="beb69-121">In the left pane, select **New proxy**.</span></span>
3. <span data-ttu-id="beb69-122">Zadejte název pro proxy.</span><span class="sxs-lookup"><span data-stu-id="beb69-122">Provide a name for your proxy.</span></span>
4. <span data-ttu-id="beb69-123">Konfigurace koncového bodu, který je vystaven na tuto aplikaci funkce zadáním **šablonu trasy** a **metody HTTP**.</span><span class="sxs-lookup"><span data-stu-id="beb69-123">Configure the endpoint that's exposed on this function app by specifying the **route template** and **HTTP methods**.</span></span> <span data-ttu-id="beb69-124">Tyto parametry se chovat podle pravidel pro [aktivace protokolu HTTP].</span><span class="sxs-lookup"><span data-stu-id="beb69-124">These parameters behave according to the rules for [HTTP triggers].</span></span>
5. <span data-ttu-id="beb69-125">Nastavte **URL back-end** na jiný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="beb69-125">Set the **backend URL** to another endpoint.</span></span> <span data-ttu-id="beb69-126">Tento koncový bod může být funkce v jiné aplikaci funkce, nebo to může být ostatní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="beb69-126">This endpoint could be a function in another function app, or it could be any other API.</span></span> <span data-ttu-id="beb69-127">Hodnota nemusí být statické, a může obsahovat odkazy na [nastavení aplikace] a [parametry z původního požadavku klienta].</span><span class="sxs-lookup"><span data-stu-id="beb69-127">The value does not need to be static, and it can reference [application settings] and [parameters from the original client request].</span></span>
6. <span data-ttu-id="beb69-128">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="beb69-128">Click **Create**.</span></span>

<span data-ttu-id="beb69-129">Proxy nyní existuje jako nový koncový bod v aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="beb69-129">Your proxy now exists as a new endpoint on your function app.</span></span> <span data-ttu-id="beb69-130">Z hlediska klienta je ekvivalentní HttpTrigger v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="beb69-130">From a client perspective, it is equivalent to an HttpTrigger in Azure Functions.</span></span> <span data-ttu-id="beb69-131">Váš nový proxy server můžete vyzkoušet tak, že adresa URL proxy serveru a testování pomocí vašeho oblíbeného klienta HTTP.</span><span class="sxs-lookup"><span data-stu-id="beb69-131">You can try out your new proxy by copying the Proxy URL and testing it with your favorite HTTP client.</span></span>

## <span data-ttu-id="beb69-132"><a name="modify-requests-responses"></a>Upravit požadavky a odpovědi</span><span class="sxs-lookup"><span data-stu-id="beb69-132"><a name="modify-requests-responses"></a>Modify requests and responses</span></span>

<span data-ttu-id="beb69-133">S funkcí proxy Azure můžete upravit žádostí a odpovědí z back-end.</span><span class="sxs-lookup"><span data-stu-id="beb69-133">With Azure Functions Proxies, you can modify requests to and responses from the back end.</span></span> <span data-ttu-id="beb69-134">Tyto transformace můžete používat proměnné, jak jsou definovány v [použijte proměnné].</span><span class="sxs-lookup"><span data-stu-id="beb69-134">These transformations can use variables as defined in [Use variables].</span></span>

### <span data-ttu-id="beb69-135"><a name="modify-backend-request"></a>Upravit požadavek back-end</span><span class="sxs-lookup"><span data-stu-id="beb69-135"><a name="modify-backend-request"></a>Modify the back-end request</span></span>

<span data-ttu-id="beb69-136">Ve výchozím nastavení je požadavek back-end inicializován jako kopii původního požadavku.</span><span class="sxs-lookup"><span data-stu-id="beb69-136">By default, the back-end request is initialized as a copy of the original request.</span></span> <span data-ttu-id="beb69-137">Kromě nastavení adresy URL back-end, můžete provést změny metoda HTTP, hlaviček a parametrů řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="beb69-137">In addition to setting the back-end URL, you can make changes to the HTTP method, headers, and query string parameters.</span></span> <span data-ttu-id="beb69-138">Změny hodnot, můžete odkazovat [nastavení aplikace] a [parametry z původního požadavku klienta].</span><span class="sxs-lookup"><span data-stu-id="beb69-138">The modified values can reference [application settings] and [parameters from the original client request].</span></span>

<span data-ttu-id="beb69-139">V současné době není k dispozici žádné portálu prostředí pro úpravy požadavky back-end.</span><span class="sxs-lookup"><span data-stu-id="beb69-139">Currently, there is no portal experience for modifying back-end requests.</span></span> <span data-ttu-id="beb69-140">Zjistěte, jak použít tuto možnost z proxies.json, najdete v tématu [definovat objekt requestOverrides].</span><span class="sxs-lookup"><span data-stu-id="beb69-140">To learn how to apply this capability from proxies.json, see [Define a requestOverrides object].</span></span>

### <span data-ttu-id="beb69-141"><a name="modify-response"></a>Upravit odpověď</span><span class="sxs-lookup"><span data-stu-id="beb69-141"><a name="modify-response"></a>Modify the response</span></span>

<span data-ttu-id="beb69-142">Ve výchozím nastavení je odpověď klienta jako kopii odpovědi na back-end inicializován.</span><span class="sxs-lookup"><span data-stu-id="beb69-142">By default, the client response is initialized as a copy of the back-end response.</span></span> <span data-ttu-id="beb69-143">Můžete změnit stavový kód, frázi důvodu, hlavičky a text z odpovědi.</span><span class="sxs-lookup"><span data-stu-id="beb69-143">You can make changes to the response's status code, reason phrase, headers, and body.</span></span> <span data-ttu-id="beb69-144">Změny hodnot, můžete odkazovat [nastavení aplikace], [parametry z původního požadavku klienta], a [parametry z back-end odpovědi].</span><span class="sxs-lookup"><span data-stu-id="beb69-144">The modified values can reference [application settings], [parameters from the original client request], and [parameters from the back-end response].</span></span>

<span data-ttu-id="beb69-145">V současné době není k dispozici žádné portálu prostředí pro úpravy odpovědi.</span><span class="sxs-lookup"><span data-stu-id="beb69-145">Currently, there is no portal experience for modifying responses.</span></span> <span data-ttu-id="beb69-146">Zjistěte, jak použít tuto možnost z proxies.json, najdete v tématu [definovat objekt responseOverrides].</span><span class="sxs-lookup"><span data-stu-id="beb69-146">To learn how to apply this capability from proxies.json, see [Define a responseOverrides object].</span></span>

## <span data-ttu-id="beb69-147"><a name="using-variables"></a>Použití proměnných</span><span class="sxs-lookup"><span data-stu-id="beb69-147"><a name="using-variables"></a>Use variables</span></span>

<span data-ttu-id="beb69-148">Konfigurace proxy serveru nemusí být statická.</span><span class="sxs-lookup"><span data-stu-id="beb69-148">The configuration for a proxy does not need to be static.</span></span> <span data-ttu-id="beb69-149">Podmínka vyhodnocena ho na použití proměnné z původní žádost, back-end odpovědi nebo nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="beb69-149">You can condition it to use variables from the original request, the back-end response, or application settings.</span></span>

### <span data-ttu-id="beb69-150"><a name="request-parameters"></a>Parametry žádosti referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="beb69-150"><a name="request-parameters"></a>Reference request parameters</span></span>

<span data-ttu-id="beb69-151">Parametry žádosti můžete použít jako vstupy pro vlastnost adresa URL back-end nebo jako součást úprava požadavky a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="beb69-151">You can use request parameters as inputs to the back-end URL property or as part of modifying requests and responses.</span></span> <span data-ttu-id="beb69-152">Některé parametry mohou být vázány z šablony trasy, která je určená v konfiguraci proxy serveru základní a jiné mohou pocházet z vlastnosti příchozího požadavku.</span><span class="sxs-lookup"><span data-stu-id="beb69-152">Some parameters can be bound from the route template that's specified in the base proxy configuration, and others can come from properties of the incoming request.</span></span>

#### <a name="route-template-parameters"></a><span data-ttu-id="beb69-153">Parametry šablony trasy</span><span class="sxs-lookup"><span data-stu-id="beb69-153">Route template parameters</span></span>
<span data-ttu-id="beb69-154">Parametry, které se používají v šabloně trasy jsou k dispozici bude odkazovat podle názvu.</span><span class="sxs-lookup"><span data-stu-id="beb69-154">Parameters that are used in the route template are available to be referenced by name.</span></span> <span data-ttu-id="beb69-155">Názvy parametrů jsou uzavřené do složených závorek ({}).</span><span class="sxs-lookup"><span data-stu-id="beb69-155">The parameter names are enclosed in braces ({}).</span></span>

<span data-ttu-id="beb69-156">Pokud proxy server má šablonu trasy, jako například `/pets/{petId}`, adresa URL back-end může obsahovat hodnotu `{petId}`jako v `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`.</span><span class="sxs-lookup"><span data-stu-id="beb69-156">For example, if a proxy has a route template, such as `/pets/{petId}`, the back-end URL can include the value of `{petId}`, as in `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`.</span></span> <span data-ttu-id="beb69-157">Pokud šablonu trasy ukončí v zástupný znak, jako například `/api/{*restOfPath}`, hodnota `{restOfPath}` je řetězcová reprezentace zbývající segmenty cesty z příchozího požadavku.</span><span class="sxs-lookup"><span data-stu-id="beb69-157">If the route template terminates in a wildcard, such as `/api/{*restOfPath}`, the value `{restOfPath}` is a string representation of the remaining path segments from the incoming request.</span></span>

#### <a name="additional-request-parameters"></a><span data-ttu-id="beb69-158">Parametry další žádosti</span><span class="sxs-lookup"><span data-stu-id="beb69-158">Additional request parameters</span></span>
<span data-ttu-id="beb69-159">Kromě parametry šablony trasy lze použít následující hodnoty v hodnotách konfigurace:</span><span class="sxs-lookup"><span data-stu-id="beb69-159">In addition to the route template parameters, the following values can be used in config values:</span></span>

* <span data-ttu-id="beb69-160">**{request.method}** : Metoda protokolu HTTP, který se používá u původního požadavku.</span><span class="sxs-lookup"><span data-stu-id="beb69-160">**{request.method}**: The HTTP method that's used on the original request.</span></span>
* <span data-ttu-id="beb69-161">**{request.headers. \<HeaderName\>}**: hlavičku, který může číst z původního požadavku.</span><span class="sxs-lookup"><span data-stu-id="beb69-161">**{request.headers.\<HeaderName\>}**: A header that can be read from the original request.</span></span> <span data-ttu-id="beb69-162">Nahraďte  *\<HeaderName\>*  s názvem záhlaví, který chcete číst.</span><span class="sxs-lookup"><span data-stu-id="beb69-162">Replace *\<HeaderName\>* with the name of the header that you want to read.</span></span> <span data-ttu-id="beb69-163">Pokud není k dispozici hlavičky v požadavku, bude hodnota prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="beb69-163">If the header is not included on the request, the value will be the empty string.</span></span>
* <span data-ttu-id="beb69-164">**{request.querystring. \<ParameterName\>}**: parametr řetězce dotazu, který může číst z původního požadavku.</span><span class="sxs-lookup"><span data-stu-id="beb69-164">**{request.querystring.\<ParameterName\>}**: A query string parameter that can be read from the original request.</span></span> <span data-ttu-id="beb69-165">Nahraďte  *\<ParameterName\>*  s názvem parametr, který chcete číst.</span><span class="sxs-lookup"><span data-stu-id="beb69-165">Replace *\<ParameterName\>* with the name of the parameter that you want to read.</span></span> <span data-ttu-id="beb69-166">Pokud parametr není zahrnut v žádosti, bude hodnota prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="beb69-166">If the parameter is not included on the request, the value will be the empty string.</span></span>

### <span data-ttu-id="beb69-167"><a name="response-parameters"></a>Odkaz na back-end odpovědi parametry</span><span class="sxs-lookup"><span data-stu-id="beb69-167"><a name="response-parameters"></a>Reference back-end response parameters</span></span>

<span data-ttu-id="beb69-168">Odpověď parametry můžete použít jako součást úprava odpověď klientovi.</span><span class="sxs-lookup"><span data-stu-id="beb69-168">Response parameters can be used as part of modifying the response to the client.</span></span> <span data-ttu-id="beb69-169">Následující hodnoty lze použít v hodnotách konfigurace:</span><span class="sxs-lookup"><span data-stu-id="beb69-169">The following values can be used in config values:</span></span>

* <span data-ttu-id="beb69-170">**{backend.response.statusCode}** : Stavový kód protokolu HTTP, která je vrácena v odpovědi back-end.</span><span class="sxs-lookup"><span data-stu-id="beb69-170">**{backend.response.statusCode}**: The HTTP status code that's returned on the back-end response.</span></span>
* <span data-ttu-id="beb69-171">**{backend.response.statusReason}** : Frázi důvodu protokolu HTTP, která je vrácena v odpovědi back-end.</span><span class="sxs-lookup"><span data-stu-id="beb69-171">**{backend.response.statusReason}**: The HTTP reason phrase that's returned on the back-end response.</span></span>
* <span data-ttu-id="beb69-172">**{backend.response.headers. \<HeaderName\>}**: hlavičku, který může číst z back-end odpovědi.</span><span class="sxs-lookup"><span data-stu-id="beb69-172">**{backend.response.headers.\<HeaderName\>}**: A header that can be read from the back-end response.</span></span> <span data-ttu-id="beb69-173">Nahraďte  *\<HeaderName\>*  s názvem hlavičky, která si chcete přečíst.</span><span class="sxs-lookup"><span data-stu-id="beb69-173">Replace *\<HeaderName\>* with the name of the header you want to read.</span></span> <span data-ttu-id="beb69-174">Pokud není k dispozici hlavičky v požadavku, bude hodnota prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="beb69-174">If the header is not included on the request, the value will be the empty string.</span></span>

### <span data-ttu-id="beb69-175"><a name="use-appsettings"></a>Odkaz nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="beb69-175"><a name="use-appsettings"></a>Reference application settings</span></span>

<span data-ttu-id="beb69-176">Můžete taky odkazovat [nastavení aplikace, které jsou definovány pro funkce aplikace](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) tím, že název nastavení znaky procenta (%).</span><span class="sxs-lookup"><span data-stu-id="beb69-176">You can also reference [application settings defined for the function app](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) by surrounding the setting name with percent signs (%).</span></span>

<span data-ttu-id="beb69-177">Například adresu URL back-end z *https://%ORDER_PROCESSING_HOST%/api/orders* by měla mít "% ORDER_PROCESSING_HOST %" nahrazeno hodnotou ORDER_PROCESSING_HOST nastavení.</span><span class="sxs-lookup"><span data-stu-id="beb69-177">For example, a back-end URL of *https://%ORDER_PROCESSING_HOST%/api/orders* would have "%ORDER_PROCESSING_HOST%" replaced with the value of the ORDER_PROCESSING_HOST setting.</span></span>

> [!TIP] 
> <span data-ttu-id="beb69-178">Použít nastavení aplikace pro hostitele back-end, když máte více nasazení nebo testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="beb69-178">Use application settings for back-end hosts when you have multiple deployments or test environments.</span></span> <span data-ttu-id="beb69-179">Tímto způsobem můžete zajistit, že jste vždy rozhovoru s právo back-end pro prostředí.</span><span class="sxs-lookup"><span data-stu-id="beb69-179">That way, you can make sure that you are always talking to the right back end for that environment.</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="beb69-180">Pokročilá konfigurace</span><span class="sxs-lookup"><span data-stu-id="beb69-180">Advanced configuration</span></span>

<span data-ttu-id="beb69-181">Proxy servery, které nakonfigurujete jsou uloženy v souboru proxies.json, který se nachází v kořenovém adresáři aplikace funkce.</span><span class="sxs-lookup"><span data-stu-id="beb69-181">The proxies that you configure are stored in a proxies.json file, which is located in the root of a function app directory.</span></span> <span data-ttu-id="beb69-182">Můžete ručně upravit tento soubor a nasadit jako součást aplikace, když použijete některou z [metody nasazení](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) této funkce podporuje.</span><span class="sxs-lookup"><span data-stu-id="beb69-182">You can manually edit this file and deploy it as part of your app when you use any of the [deployment methods](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) that Functions supports.</span></span> <span data-ttu-id="beb69-183">Tato funkce musí být [povoleno](#enable) pro soubor, který má být zpracována.</span><span class="sxs-lookup"><span data-stu-id="beb69-183">The feature must be [enabled](#enable) for the file to be processed.</span></span> 

> [!TIP] 
> <span data-ttu-id="beb69-184">Pokud jste nenastavili jednu z metod nasazení, můžete také pracovat se soubor proxies.json na portálu.</span><span class="sxs-lookup"><span data-stu-id="beb69-184">If you have not set up one of the deployment methods, you can also work with the proxies.json file in the portal.</span></span> <span data-ttu-id="beb69-185">Přejděte do funkce aplikace, vyberte **funkce**a potom vyberte **App Service Editor**.</span><span class="sxs-lookup"><span data-stu-id="beb69-185">Go to your function app, select **Platform features**, and then select **App Service Editor**.</span></span> <span data-ttu-id="beb69-186">Díky tomu můžete zobrazit celý soubor strukturu funkce aplikace a proveďte změny.</span><span class="sxs-lookup"><span data-stu-id="beb69-186">By doing so, you can view the entire file structure of your function app and then make changes.</span></span>

<span data-ttu-id="beb69-187">Proxies.JSON je definován objekt proxy, který se skládá z pojmenované proxy serverů a jejich definice.</span><span class="sxs-lookup"><span data-stu-id="beb69-187">Proxies.json is defined by a proxies object, which is composed of named proxies and their definitions.</span></span> <span data-ttu-id="beb69-188">Případně, pokud vaše editor podporuje, můžete odkazovat [schématu JSON](http://json.schemastore.org/proxies) pro doplňování kódu.</span><span class="sxs-lookup"><span data-stu-id="beb69-188">Optionally, if your editor supports it, you can reference a [JSON schema](http://json.schemastore.org/proxies) for code completion.</span></span> <span data-ttu-id="beb69-189">Příklad souboru může vypadat následovně:</span><span class="sxs-lookup"><span data-stu-id="beb69-189">An example file might look like the following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>"
        }
    }
}
```

<span data-ttu-id="beb69-190">Každý proxy má popisný název, například *proxy1* v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="beb69-190">Each proxy has a friendly name, such as *proxy1* in the preceding example.</span></span> <span data-ttu-id="beb69-191">Objekt odpovídající definice proxy serveru je definováno následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="beb69-191">The corresponding proxy definition object is defined by the following properties:</span></span>

* <span data-ttu-id="beb69-192">**matchCondition**: požadované – objekt definování požadavků, které aktivace provádění tento proxy server.</span><span class="sxs-lookup"><span data-stu-id="beb69-192">**matchCondition**: Required--an object defining the requests that trigger the execution of this proxy.</span></span> <span data-ttu-id="beb69-193">Obsahuje dvě vlastnosti, které jsou sdíleny s [aktivace protokolu HTTP]:</span><span class="sxs-lookup"><span data-stu-id="beb69-193">It contains two properties that are shared with [HTTP triggers]:</span></span>
    * <span data-ttu-id="beb69-194">_metody_: metody HTTP, které proxy serveru reaguje na pole.</span><span class="sxs-lookup"><span data-stu-id="beb69-194">_methods_: An array of the HTTP methods that the proxy responds to.</span></span> <span data-ttu-id="beb69-195">Pokud není zadaný, proxy server odpoví na všechny metody HTTP na trasy.</span><span class="sxs-lookup"><span data-stu-id="beb69-195">If it is not specified, the proxy responds to all HTTP methods on the route.</span></span>
    * <span data-ttu-id="beb69-196">_trasy_: požadované – definuje šablonu trasy řízení, které adresy URL žádosti proxy odpoví.</span><span class="sxs-lookup"><span data-stu-id="beb69-196">_route_: Required--defines the route template, controlling which request URLs your proxy responds to.</span></span> <span data-ttu-id="beb69-197">Na rozdíl od v aktivační události protokolu HTTP, není žádná výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="beb69-197">Unlike in HTTP triggers, there is no default value.</span></span>
* <span data-ttu-id="beb69-198">**backendUri**: adresa URL back-end prostředku, na kterou by měla být žádost směrovány přes proxy server.</span><span class="sxs-lookup"><span data-stu-id="beb69-198">**backendUri**: The URL of the back-end resource to which the request should be proxied.</span></span> <span data-ttu-id="beb69-199">Tato hodnota může odkazovat nastavení aplikace a parametry z původního požadavku klienta.</span><span class="sxs-lookup"><span data-stu-id="beb69-199">This value can reference application settings and parameters from the original client request.</span></span> <span data-ttu-id="beb69-200">Pokud tuto vlastnost nezadáte, odpoví Azure Functions HTTP 200 OK.</span><span class="sxs-lookup"><span data-stu-id="beb69-200">If this property is not included, Azure Functions responds with an HTTP 200 OK.</span></span>
* <span data-ttu-id="beb69-201">**requestOverrides**: objekt, který definuje transformace na požadavek back-end.</span><span class="sxs-lookup"><span data-stu-id="beb69-201">**requestOverrides**: An object that defines transformations to the back-end request.</span></span> <span data-ttu-id="beb69-202">V tématu [definovat objekt requestOverrides].</span><span class="sxs-lookup"><span data-stu-id="beb69-202">See [Define a requestOverrides object].</span></span>
* <span data-ttu-id="beb69-203">**responseOverrides**: objekt, který definuje transformace na odpověď klienta.</span><span class="sxs-lookup"><span data-stu-id="beb69-203">**responseOverrides**: An object that defines transformations to the client response.</span></span> <span data-ttu-id="beb69-204">V tématu [definovat objekt responseOverrides].</span><span class="sxs-lookup"><span data-stu-id="beb69-204">See [Define a responseOverrides object].</span></span>

> [!NOTE] 
> <span data-ttu-id="beb69-205">Vlastnost trasy proxy funkce Azure nectí vlastnost routePrefix konfigurace hostitele funkce.</span><span class="sxs-lookup"><span data-stu-id="beb69-205">The route property Azure Functions Proxies does not honor the routePrefix property of the Functions host configuration.</span></span> <span data-ttu-id="beb69-206">Pokud chcete zahrnout předpona například /api, musí být zahrnut ve vlastnosti trasy.</span><span class="sxs-lookup"><span data-stu-id="beb69-206">If you want to include a prefix such as /api, it must be included in the route property.</span></span>

### <span data-ttu-id="beb69-207"><a name="requestOverrides"></a>Definování objekt requestOverrides</span><span class="sxs-lookup"><span data-stu-id="beb69-207"><a name="requestOverrides"></a>Define a requestOverrides object</span></span>

<span data-ttu-id="beb69-208">Objekt requestOverrides definuje změny provedené na žádost o při volání prostředků back-end.</span><span class="sxs-lookup"><span data-stu-id="beb69-208">The requestOverrides object defines changes made to the request when the back-end resource is called.</span></span> <span data-ttu-id="beb69-209">Objekt je definováno následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="beb69-209">The object is defined by the following properties:</span></span>

* <span data-ttu-id="beb69-210">**backend.Request.Method**: Metoda protokolu HTTP, který se používá k volání back-end.</span><span class="sxs-lookup"><span data-stu-id="beb69-210">**backend.request.method**: The HTTP method that's used to call the back end.</span></span>
* <span data-ttu-id="beb69-211">**backend.Request.QueryString. \<ParameterName\>**: parametr řetězce dotazu, který je možné nastavit pro volání back-end.</span><span class="sxs-lookup"><span data-stu-id="beb69-211">**backend.request.querystring.\<ParameterName\>**: A query string parameter that can be set for the call to the back end.</span></span> <span data-ttu-id="beb69-212">Nahraďte  *\<ParameterName\>*  s názvem parametr, který chcete nastavit.</span><span class="sxs-lookup"><span data-stu-id="beb69-212">Replace *\<ParameterName\>* with the name of the parameter that you want to set.</span></span> <span data-ttu-id="beb69-213">Pokud je zadán prázdný řetězec, není parametr součástí požadavek back-end.</span><span class="sxs-lookup"><span data-stu-id="beb69-213">If the empty string is provided, the parameter is not included on the back-end request.</span></span>
* <span data-ttu-id="beb69-214">**backend.Request.Headers. \<HeaderName\>**: hlavičku, která se dá nastavit pro volání back-end.</span><span class="sxs-lookup"><span data-stu-id="beb69-214">**backend.request.headers.\<HeaderName\>**: A header that can be set for the call to the back end.</span></span> <span data-ttu-id="beb69-215">Nahraďte  *\<HeaderName\>*  s názvem záhlaví, který chcete nastavit.</span><span class="sxs-lookup"><span data-stu-id="beb69-215">Replace *\<HeaderName\>* with the name of the header that you want to set.</span></span> <span data-ttu-id="beb69-216">Pokud zadáte prázdný řetězec, není k dispozici hlavičky v požadavku back-end.</span><span class="sxs-lookup"><span data-stu-id="beb69-216">If you provide the empty string, the header is not included on the back-end request.</span></span>

<span data-ttu-id="beb69-217">Hodnoty můžete odkazovat nastavení aplikace a parametry z původního požadavku klienta.</span><span class="sxs-lookup"><span data-stu-id="beb69-217">Values can reference application settings and parameters from the original client request.</span></span>

<span data-ttu-id="beb69-218">Příklad konfigurace může vypadat následovně:</span><span class="sxs-lookup"><span data-stu-id="beb69-218">An example configuration might look like the following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>",
            "requestOverrides": {
                "backend.request.headers.Accept": "application/xml",
                "backend.request.headers.x-functions-key": "%ANOTHERAPP_API_KEY%"
            }
        }
    }
}
```

### <span data-ttu-id="beb69-219"><a name="responseOverrides"></a>Definování objekt responseOverrides</span><span class="sxs-lookup"><span data-stu-id="beb69-219"><a name="responseOverrides"></a>Define a responseOverrides object</span></span>

<span data-ttu-id="beb69-220">Objekt requestOverrides definuje změny provedené v odpovědi, který je předán zpět do klienta.</span><span class="sxs-lookup"><span data-stu-id="beb69-220">The requestOverrides object defines changes that are made to the response that's passed back to the client.</span></span> <span data-ttu-id="beb69-221">Objekt je definováno následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="beb69-221">The object is defined by the following properties:</span></span>

* <span data-ttu-id="beb69-222">**response.statusCode**: stavový kód protokolu HTTP má být vrácen do klienta.</span><span class="sxs-lookup"><span data-stu-id="beb69-222">**response.statusCode**: The HTTP status code to be returned to the client.</span></span>
* <span data-ttu-id="beb69-223">**response.statusReason**: frázi důvodu protokolu HTTP má být vrácen do klienta.</span><span class="sxs-lookup"><span data-stu-id="beb69-223">**response.statusReason**: The HTTP reason phrase to be returned to the client.</span></span>
* <span data-ttu-id="beb69-224">**Response.body**: řetězcovou reprezentaci těla má být vrácen do klienta.</span><span class="sxs-lookup"><span data-stu-id="beb69-224">**response.body**: The string representation of the body to be returned to the client.</span></span>
* <span data-ttu-id="beb69-225">**Response.Headers. \<HeaderName\>**: hlavičku, která se dá nastavit pro odpověď klientovi.</span><span class="sxs-lookup"><span data-stu-id="beb69-225">**response.headers.\<HeaderName\>**: A header that can be set for the response to the client.</span></span> <span data-ttu-id="beb69-226">Nahraďte  *\<HeaderName\>*  s názvem záhlaví, který chcete nastavit.</span><span class="sxs-lookup"><span data-stu-id="beb69-226">Replace *\<HeaderName\>* with the name of the header that you want to set.</span></span> <span data-ttu-id="beb69-227">Pokud zadáte prázdný řetězec, záhlaví není zahrnutý v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="beb69-227">If you provide the empty string, the header is not included on the response.</span></span>

<span data-ttu-id="beb69-228">Hodnoty můžete odkazovat nastavení aplikace, parametry z původního požadavku klienta a parametry z back-end odpovědi.</span><span class="sxs-lookup"><span data-stu-id="beb69-228">Values can reference application settings, parameters from the original client request, and parameters from the back-end response.</span></span>

<span data-ttu-id="beb69-229">Příklad konfigurace může vypadat následovně:</span><span class="sxs-lookup"><span data-stu-id="beb69-229">An example configuration might look like the following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "responseOverrides": {
                "response.body": "Hello, {test}",
                "response.headers.Content-Type": "text/plain"
            }
        }
    }
}
```
> [!NOTE] 
> <span data-ttu-id="beb69-230">V tomto příkladu text se nastavuje přímo, takže ne `backendUri` vlastnost je vyžadována.</span><span class="sxs-lookup"><span data-stu-id="beb69-230">In this example, the body is being set directly, so no `backendUri` property is needed.</span></span> <span data-ttu-id="beb69-231">Příklad ukazuje, jak je možné použít Azure funkce proxy pro mocking rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="beb69-231">The example shows how you might use Azure Functions Proxies for mocking APIs.</span></span>

<span data-ttu-id="beb69-232">[portál Azure]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="beb69-232">[Azure portal]: https://portal.azure.com</span></span>
<span data-ttu-id="beb69-233">[aktivace protokolu HTTP]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger</span><span class="sxs-lookup"><span data-stu-id="beb69-233">[HTTP triggers]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger</span></span>
[Modify the back-end request]: #modify-backend-request
[Modify the response]: #modify-response
<span data-ttu-id="beb69-234">[definovat objekt requestOverrides]: #requestOverrides</span><span class="sxs-lookup"><span data-stu-id="beb69-234">[Define a requestOverrides object]: #requestOverrides</span></span>
<span data-ttu-id="beb69-235">[definovat objekt responseOverrides]: #responseOverrides</span><span class="sxs-lookup"><span data-stu-id="beb69-235">[Define a responseOverrides object]: #responseOverrides</span></span>
<span data-ttu-id="beb69-236">[nastavení aplikace]: #use-appsettings</span><span class="sxs-lookup"><span data-stu-id="beb69-236">[application settings]: #use-appsettings</span></span>
<span data-ttu-id="beb69-237">[použijte proměnné]: #using-variables</span><span class="sxs-lookup"><span data-stu-id="beb69-237">[Use variables]: #using-variables</span></span>
<span data-ttu-id="beb69-238">[parametry z původního požadavku klienta]: #request-parameters</span><span class="sxs-lookup"><span data-stu-id="beb69-238">[parameters from the original client request]: #request-parameters</span></span>
<span data-ttu-id="beb69-239">[parametry z back-end odpovědi]: #response-parameters</span><span class="sxs-lookup"><span data-stu-id="beb69-239">[parameters from the back-end response]: #response-parameters</span></span>
