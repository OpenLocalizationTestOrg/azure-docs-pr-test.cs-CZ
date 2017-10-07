---
title: aaaWork s proxy Azure Functions | Microsoft Docs
description: "Základní informace o toouse funkce proxy Azure"
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
ms.openlocfilehash: 4d94c89e8f8f2d2c771b01bae142bf9a4f3b7f2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-azure-functions-proxies-preview"></a><span data-ttu-id="8cea2-103">Práce s funkcí proxy Azure (preview)</span><span class="sxs-lookup"><span data-stu-id="8cea2-103">Work with Azure Functions Proxies (preview)</span></span>

> [!NOTE] 
> <span data-ttu-id="8cea2-104">Azure proxy funkce je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="8cea2-104">Azure Functions Proxies is currently in preview.</span></span> <span data-ttu-id="8cea2-105">Volné je během ve verzi preview, ale standardní funkce fakturace tooproxy spuštěních.</span><span class="sxs-lookup"><span data-stu-id="8cea2-105">It is free while in preview, but standard Functions billing applies tooproxy executions.</span></span> <span data-ttu-id="8cea2-106">Další informace najdete v tématu [ceny Azure Functions](https://azure.microsoft.com/pricing/details/functions/).</span><span class="sxs-lookup"><span data-stu-id="8cea2-106">For more information, see [Azure Functions pricing](https://azure.microsoft.com/pricing/details/functions/).</span></span>

<span data-ttu-id="8cea2-107">Tento článek vysvětluje, jak tooconfigure a práce s Azure funkce proxy.</span><span class="sxs-lookup"><span data-stu-id="8cea2-107">This article explains how tooconfigure and work with Azure Functions Proxies.</span></span> <span data-ttu-id="8cea2-108">Pomocí této funkce můžete určovat koncové body ve vaší aplikaci funkce, které jsou implementované pomocí jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="8cea2-108">With this feature, you can specify endpoints on your function app that are implemented by another resource.</span></span> <span data-ttu-id="8cea2-109">Tyto servery proxy toobreak velké rozhraní API do více aplikací – funkce (jako architektury mikroslužby), můžete použít při stále poskytovat jeden prostor rozhraní API pro klienty.</span><span class="sxs-lookup"><span data-stu-id="8cea2-109">You can use these proxies toobreak a large API into multiple function apps (as in a microservice architecture), while still presenting a single API surface for clients.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]


## <span data-ttu-id="8cea2-110"><a name="enable"></a>Povolit proxy Azure Functions</span><span class="sxs-lookup"><span data-stu-id="8cea2-110"><a name="enable"></a>Enable Azure Functions Proxies</span></span>

<span data-ttu-id="8cea2-111">Proxy nejsou povolené ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="8cea2-111">Proxies are not enabled by default.</span></span> <span data-ttu-id="8cea2-112">Můžete vytvořit proxy hello funkce je zakázaná, ale spustit.</span><span class="sxs-lookup"><span data-stu-id="8cea2-112">You can create proxies while hello feature is disabled, but they will not execute.</span></span> <span data-ttu-id="8cea2-113">proxy tooenable hello následující:</span><span class="sxs-lookup"><span data-stu-id="8cea2-113">tooenable proxies, do hello following:</span></span>

1. <span data-ttu-id="8cea2-114">Otevřete hello [portál Azure], a potom přejděte tooyour funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="8cea2-114">Open hello [Azure portal], and then go tooyour function app.</span></span>
2. <span data-ttu-id="8cea2-115">Vyberte **funkce nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8cea2-115">Select **Function app settings**.</span></span>
3. <span data-ttu-id="8cea2-116">Přepínač **povolit funkce proxy Azure (preview)** příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="8cea2-116">Switch **Enable Azure Functions Proxies (preview)** too**On**.</span></span>

<span data-ttu-id="8cea2-117">Můžete se taky vrátit zde tooupdate hello proxy runtime nové funkce dostupná.</span><span class="sxs-lookup"><span data-stu-id="8cea2-117">You can also return here tooupdate hello proxy runtime as new features become available.</span></span>


## <span data-ttu-id="8cea2-118"><a name="create"></a>Vytvořit proxy</span><span class="sxs-lookup"><span data-stu-id="8cea2-118"><a name="create"></a>Create a proxy</span></span>

<span data-ttu-id="8cea2-119">Tato část uvádí, jak toocreate a proxy serveru v hello Functions na portálu.</span><span class="sxs-lookup"><span data-stu-id="8cea2-119">This section shows you how toocreate a proxy in hello Functions portal.</span></span>

1. <span data-ttu-id="8cea2-120">Otevřete hello [portál Azure], a potom přejděte tooyour funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="8cea2-120">Open hello [Azure portal], and then go tooyour function app.</span></span>
2. <span data-ttu-id="8cea2-121">V levém podokně hello vyberte **nového proxy**.</span><span class="sxs-lookup"><span data-stu-id="8cea2-121">In hello left pane, select **New proxy**.</span></span>
3. <span data-ttu-id="8cea2-122">Zadejte název pro proxy.</span><span class="sxs-lookup"><span data-stu-id="8cea2-122">Provide a name for your proxy.</span></span>
4. <span data-ttu-id="8cea2-123">Konfigurace hello koncový bod, který je vystaven na tuto aplikaci funkce zadáním hello **šablonu trasy** a **metody HTTP**.</span><span class="sxs-lookup"><span data-stu-id="8cea2-123">Configure hello endpoint that's exposed on this function app by specifying hello **route template** and **HTTP methods**.</span></span> <span data-ttu-id="8cea2-124">Tyto parametry chovat podle pravidla toohello pro [aktivace protokolu HTTP].</span><span class="sxs-lookup"><span data-stu-id="8cea2-124">These parameters behave according toohello rules for [HTTP triggers].</span></span>
5. <span data-ttu-id="8cea2-125">Sada hello **URL back-end** tooanother koncový bod.</span><span class="sxs-lookup"><span data-stu-id="8cea2-125">Set hello **backend URL** tooanother endpoint.</span></span> <span data-ttu-id="8cea2-126">Tento koncový bod může být funkce v jiné aplikaci funkce, nebo to může být ostatní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8cea2-126">This endpoint could be a function in another function app, or it could be any other API.</span></span> <span data-ttu-id="8cea2-127">Hello hodnotu, nebude potřebovat toobe statické, a může obsahovat odkazy na [nastavení aplikace] a [parametry z původního požadavku klienta hello].</span><span class="sxs-lookup"><span data-stu-id="8cea2-127">hello value does not need toobe static, and it can reference [application settings] and [parameters from hello original client request].</span></span>
6. <span data-ttu-id="8cea2-128">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8cea2-128">Click **Create**.</span></span>

<span data-ttu-id="8cea2-129">Proxy nyní existuje jako nový koncový bod v aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="8cea2-129">Your proxy now exists as a new endpoint on your function app.</span></span> <span data-ttu-id="8cea2-130">Z hlediska klienta je ekvivalentní tooan HttpTrigger v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="8cea2-130">From a client perspective, it is equivalent tooan HttpTrigger in Azure Functions.</span></span> <span data-ttu-id="8cea2-131">Váš nový proxy server můžete vyzkoušet tak, že hello adresa URL proxy serveru a testování pomocí vašeho oblíbeného klienta HTTP.</span><span class="sxs-lookup"><span data-stu-id="8cea2-131">You can try out your new proxy by copying hello Proxy URL and testing it with your favorite HTTP client.</span></span>

## <span data-ttu-id="8cea2-132"><a name="modify-requests-responses"></a>Upravit požadavky a odpovědi</span><span class="sxs-lookup"><span data-stu-id="8cea2-132"><a name="modify-requests-responses"></a>Modify requests and responses</span></span>

<span data-ttu-id="8cea2-133">S funkcí proxy Azure můžete upravit požadavky tooand odpovědí z back-end hello.</span><span class="sxs-lookup"><span data-stu-id="8cea2-133">With Azure Functions Proxies, you can modify requests tooand responses from hello back end.</span></span> <span data-ttu-id="8cea2-134">Tyto transformace můžete používat proměnné, jak jsou definovány v [použijte proměnné].</span><span class="sxs-lookup"><span data-stu-id="8cea2-134">These transformations can use variables as defined in [Use variables].</span></span>

### <span data-ttu-id="8cea2-135"><a name="modify-backend-request"></a>Upravit požadavek back-end hello</span><span class="sxs-lookup"><span data-stu-id="8cea2-135"><a name="modify-backend-request"></a>Modify hello back-end request</span></span>

<span data-ttu-id="8cea2-136">Ve výchozím nastavení je požadavek back-end hello inicializován jako kopii hello původní žádost.</span><span class="sxs-lookup"><span data-stu-id="8cea2-136">By default, hello back-end request is initialized as a copy of hello original request.</span></span> <span data-ttu-id="8cea2-137">Kromě toho toosetting hello back-end adresy URL, můžete provést změny metoda HTTP toohello, hlaviček a parametrů řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="8cea2-137">In addition toosetting hello back-end URL, you can make changes toohello HTTP method, headers, and query string parameters.</span></span> <span data-ttu-id="8cea2-138">Hello změny hodnot můžete odkazovat [nastavení aplikace] a [parametry z původního požadavku klienta hello].</span><span class="sxs-lookup"><span data-stu-id="8cea2-138">hello modified values can reference [application settings] and [parameters from hello original client request].</span></span>

<span data-ttu-id="8cea2-139">V současné době není k dispozici žádné portálu prostředí pro úpravy požadavky back-end.</span><span class="sxs-lookup"><span data-stu-id="8cea2-139">Currently, there is no portal experience for modifying back-end requests.</span></span> <span data-ttu-id="8cea2-140">toolearn jak tooapply tato funkce z proxies.json, najdete v části [definovat objekt requestOverrides].</span><span class="sxs-lookup"><span data-stu-id="8cea2-140">toolearn how tooapply this capability from proxies.json, see [Define a requestOverrides object].</span></span>

### <span data-ttu-id="8cea2-141"><a name="modify-response"></a>Upravit hello odpovědi</span><span class="sxs-lookup"><span data-stu-id="8cea2-141"><a name="modify-response"></a>Modify hello response</span></span>

<span data-ttu-id="8cea2-142">Ve výchozím nastavení je odpověď klienta hello inicializováno jako kopii hello back-end odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8cea2-142">By default, hello client response is initialized as a copy of hello back-end response.</span></span> <span data-ttu-id="8cea2-143">Můžete nastavit stavový kód odpovědi toohello změny, frázi důvodu, hlavičky a text.</span><span class="sxs-lookup"><span data-stu-id="8cea2-143">You can make changes toohello response's status code, reason phrase, headers, and body.</span></span> <span data-ttu-id="8cea2-144">Hello změny hodnot můžete odkazovat [nastavení aplikace], [parametry z původního požadavku klienta hello], a [parametry z back-end odpovědi hello].</span><span class="sxs-lookup"><span data-stu-id="8cea2-144">hello modified values can reference [application settings], [parameters from hello original client request], and [parameters from hello back-end response].</span></span>

<span data-ttu-id="8cea2-145">V současné době není k dispozici žádné portálu prostředí pro úpravy odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8cea2-145">Currently, there is no portal experience for modifying responses.</span></span> <span data-ttu-id="8cea2-146">toolearn jak tooapply tato funkce z proxies.json, najdete v části [definovat objekt responseOverrides].</span><span class="sxs-lookup"><span data-stu-id="8cea2-146">toolearn how tooapply this capability from proxies.json, see [Define a responseOverrides object].</span></span>

## <span data-ttu-id="8cea2-147"><a name="using-variables"></a>Použití proměnných</span><span class="sxs-lookup"><span data-stu-id="8cea2-147"><a name="using-variables"></a>Use variables</span></span>

<span data-ttu-id="8cea2-148">Hello konfiguraci proxy serveru nemusí toobe statické.</span><span class="sxs-lookup"><span data-stu-id="8cea2-148">hello configuration for a proxy does not need toobe static.</span></span> <span data-ttu-id="8cea2-149">Můžete ho podmínky toouse proměnné z původní žádost hello, hello back-end odpovědi nebo nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="8cea2-149">You can condition it toouse variables from hello original request, hello back-end response, or application settings.</span></span>

### <span data-ttu-id="8cea2-150"><a name="request-parameters"></a>Parametry žádosti referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="8cea2-150"><a name="request-parameters"></a>Reference request parameters</span></span>

<span data-ttu-id="8cea2-151">Parametry žádosti můžete použít jako vstup vlastnost adresa URL back-end toohello nebo jako součást úprava požadavky a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8cea2-151">You can use request parameters as inputs toohello back-end URL property or as part of modifying requests and responses.</span></span> <span data-ttu-id="8cea2-152">Některé parametry mohou být vázány z šablony hello trasy, která je určená v konfiguraci základní proxy hello a ostatní mohou pocházet z vlastnosti hello příchozího požadavku.</span><span class="sxs-lookup"><span data-stu-id="8cea2-152">Some parameters can be bound from hello route template that's specified in hello base proxy configuration, and others can come from properties of hello incoming request.</span></span>

#### <a name="route-template-parameters"></a><span data-ttu-id="8cea2-153">Parametry šablony trasy</span><span class="sxs-lookup"><span data-stu-id="8cea2-153">Route template parameters</span></span>
<span data-ttu-id="8cea2-154">Parametry, které se používají v šabloně trasy hello jsou k dispozici toobe odkazuje jeho názvem.</span><span class="sxs-lookup"><span data-stu-id="8cea2-154">Parameters that are used in hello route template are available toobe referenced by name.</span></span> <span data-ttu-id="8cea2-155">názvy parametrů Hello jsou uzavřené do složených závorek ({}).</span><span class="sxs-lookup"><span data-stu-id="8cea2-155">hello parameter names are enclosed in braces ({}).</span></span>

<span data-ttu-id="8cea2-156">Pokud proxy server má šablonu trasy, jako například `/pets/{petId}`, adresa URL back-end hello může obsahovat hodnotu hello `{petId}`jako v `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`.</span><span class="sxs-lookup"><span data-stu-id="8cea2-156">For example, if a proxy has a route template, such as `/pets/{petId}`, hello back-end URL can include hello value of `{petId}`, as in `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`.</span></span> <span data-ttu-id="8cea2-157">Pokud šablonu trasy hello ukončí v zástupný znak, jako například `/api/{*restOfPath}`, hello hodnota `{restOfPath}` je řetězcová reprezentace hello zbývající segmenty cesty z příchozího požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="8cea2-157">If hello route template terminates in a wildcard, such as `/api/{*restOfPath}`, hello value `{restOfPath}` is a string representation of hello remaining path segments from hello incoming request.</span></span>

#### <a name="additional-request-parameters"></a><span data-ttu-id="8cea2-158">Parametry další žádosti</span><span class="sxs-lookup"><span data-stu-id="8cea2-158">Additional request parameters</span></span>
<span data-ttu-id="8cea2-159">Kromě toho toohello směrovat parametry šablony, hello následující hodnoty mohou být používány hodnoty konfigurace:</span><span class="sxs-lookup"><span data-stu-id="8cea2-159">In addition toohello route template parameters, hello following values can be used in config values:</span></span>

* <span data-ttu-id="8cea2-160">**{request.method}** : hello metoda HTTP, který se používá na původní žádost hello.</span><span class="sxs-lookup"><span data-stu-id="8cea2-160">**{request.method}**: hello HTTP method that's used on hello original request.</span></span>
* <span data-ttu-id="8cea2-161">**{request.headers. \<HeaderName\>}**: hlavičku, který může číst z původního požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="8cea2-161">**{request.headers.\<HeaderName\>}**: A header that can be read from hello original request.</span></span> <span data-ttu-id="8cea2-162">Nahraďte  *\<HeaderName\>*  s názvem hello hello hlavičky, které chcete tooread.</span><span class="sxs-lookup"><span data-stu-id="8cea2-162">Replace *\<HeaderName\>* with hello name of hello header that you want tooread.</span></span> <span data-ttu-id="8cea2-163">Pokud hlavička hello není k dispozici v požadavku hello, bude hodnota hello hello prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="8cea2-163">If hello header is not included on hello request, hello value will be hello empty string.</span></span>
* <span data-ttu-id="8cea2-164">**{request.querystring. \<ParameterName\>}**: parametr řetězce dotazu, který může číst z původního požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="8cea2-164">**{request.querystring.\<ParameterName\>}**: A query string parameter that can be read from hello original request.</span></span> <span data-ttu-id="8cea2-165">Nahraďte  *\<ParameterName\>*  s názvem hello hello parametru, které chcete tooread.</span><span class="sxs-lookup"><span data-stu-id="8cea2-165">Replace *\<ParameterName\>* with hello name of hello parameter that you want tooread.</span></span> <span data-ttu-id="8cea2-166">Pokud parametr hello není k dispozici v požadavku hello, bude hodnota hello hello prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="8cea2-166">If hello parameter is not included on hello request, hello value will be hello empty string.</span></span>

### <span data-ttu-id="8cea2-167"><a name="response-parameters"></a>Odkaz na back-end odpovědi parametry</span><span class="sxs-lookup"><span data-stu-id="8cea2-167"><a name="response-parameters"></a>Reference back-end response parameters</span></span>

<span data-ttu-id="8cea2-168">Odpověď parametry můžete použít jako součást úprava hello odpovědi toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="8cea2-168">Response parameters can be used as part of modifying hello response toohello client.</span></span> <span data-ttu-id="8cea2-169">Hello následující hodnoty lze použít v hodnotách konfigurace:</span><span class="sxs-lookup"><span data-stu-id="8cea2-169">hello following values can be used in config values:</span></span>

* <span data-ttu-id="8cea2-170">**{backend.response.statusCode}** : hello stavový kód protokolu HTTP, která je vrácena v odpovědi hello back-end.</span><span class="sxs-lookup"><span data-stu-id="8cea2-170">**{backend.response.statusCode}**: hello HTTP status code that's returned on hello back-end response.</span></span>
* <span data-ttu-id="8cea2-171">**{backend.response.statusReason}** : frázi důvodu hello HTTP, která je vrácena v odpovědi hello back-end.</span><span class="sxs-lookup"><span data-stu-id="8cea2-171">**{backend.response.statusReason}**: hello HTTP reason phrase that's returned on hello back-end response.</span></span>
* <span data-ttu-id="8cea2-172">**{backend.response.headers. \<HeaderName\>}**: hlavičku, který může číst z odpovědi hello back-end.</span><span class="sxs-lookup"><span data-stu-id="8cea2-172">**{backend.response.headers.\<HeaderName\>}**: A header that can be read from hello back-end response.</span></span> <span data-ttu-id="8cea2-173">Nahraďte  *\<HeaderName\>*  s názvem hello hello hlavičky chcete tooread.</span><span class="sxs-lookup"><span data-stu-id="8cea2-173">Replace *\<HeaderName\>* with hello name of hello header you want tooread.</span></span> <span data-ttu-id="8cea2-174">Pokud hlavička hello není k dispozici v požadavku hello, bude hodnota hello hello prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="8cea2-174">If hello header is not included on hello request, hello value will be hello empty string.</span></span>

### <span data-ttu-id="8cea2-175"><a name="use-appsettings"></a>Odkaz nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="8cea2-175"><a name="use-appsettings"></a>Reference application settings</span></span>

<span data-ttu-id="8cea2-176">Můžete taky odkazovat [nastavení aplikace, které jsou definovány pro funkce aplikace hello](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) tím, že název nastavení hello znaky procenta (%).</span><span class="sxs-lookup"><span data-stu-id="8cea2-176">You can also reference [application settings defined for hello function app](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) by surrounding hello setting name with percent signs (%).</span></span>

<span data-ttu-id="8cea2-177">Například adresu URL back-end z *https://%ORDER_PROCESSING_HOST%/api/orders* by měla mít "% ORDER_PROCESSING_HOST %" nahrazen hodnotou hello hello ORDER_PROCESSING_HOST nastavení.</span><span class="sxs-lookup"><span data-stu-id="8cea2-177">For example, a back-end URL of *https://%ORDER_PROCESSING_HOST%/api/orders* would have "%ORDER_PROCESSING_HOST%" replaced with hello value of hello ORDER_PROCESSING_HOST setting.</span></span>

> [!TIP] 
> <span data-ttu-id="8cea2-178">Použít nastavení aplikace pro hostitele back-end, když máte více nasazení nebo testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="8cea2-178">Use application settings for back-end hosts when you have multiple deployments or test environments.</span></span> <span data-ttu-id="8cea2-179">Tímto způsobem, abyste měli jistotu, že vždy mluvení toohello přímo zpět ukončení pro prostředí.</span><span class="sxs-lookup"><span data-stu-id="8cea2-179">That way, you can make sure that you are always talking toohello right back end for that environment.</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="8cea2-180">Pokročilá konfigurace</span><span class="sxs-lookup"><span data-stu-id="8cea2-180">Advanced configuration</span></span>

<span data-ttu-id="8cea2-181">Hello proxy servery, které nakonfigurujete jsou uloženy v souboru proxies.json, který se nachází v hello kořenového adresáře aplikace funkce.</span><span class="sxs-lookup"><span data-stu-id="8cea2-181">hello proxies that you configure are stored in a proxies.json file, which is located in hello root of a function app directory.</span></span> <span data-ttu-id="8cea2-182">Můžete ručně upravit tento soubor a nasadit jako součást aplikace, když použijete některou z hello [metody nasazení](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) této funkce podporuje.</span><span class="sxs-lookup"><span data-stu-id="8cea2-182">You can manually edit this file and deploy it as part of your app when you use any of hello [deployment methods](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) that Functions supports.</span></span> <span data-ttu-id="8cea2-183">Hello funkce musí být [povoleno](#enable) pro toobe souboru hello zpracovat.</span><span class="sxs-lookup"><span data-stu-id="8cea2-183">hello feature must be [enabled](#enable) for hello file toobe processed.</span></span> 

> [!TIP] 
> <span data-ttu-id="8cea2-184">Pokud jste nenastavili jednu z metod nasazení hello, můžete také pracovat se soubor proxies.json hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="8cea2-184">If you have not set up one of hello deployment methods, you can also work with hello proxies.json file in hello portal.</span></span> <span data-ttu-id="8cea2-185">Přejděte tooyour funkce aplikace, vyberte **funkce**a potom vyberte **App Service Editor**.</span><span class="sxs-lookup"><span data-stu-id="8cea2-185">Go tooyour function app, select **Platform features**, and then select **App Service Editor**.</span></span> <span data-ttu-id="8cea2-186">Díky tomu můžete zobrazit celý soubor struktura hello funkce aplikace a proveďte změny.</span><span class="sxs-lookup"><span data-stu-id="8cea2-186">By doing so, you can view hello entire file structure of your function app and then make changes.</span></span>

<span data-ttu-id="8cea2-187">Proxies.JSON je definován objekt proxy, který se skládá z pojmenované proxy serverů a jejich definice.</span><span class="sxs-lookup"><span data-stu-id="8cea2-187">Proxies.json is defined by a proxies object, which is composed of named proxies and their definitions.</span></span> <span data-ttu-id="8cea2-188">Případně, pokud vaše editor podporuje, můžete odkazovat [schématu JSON](http://json.schemastore.org/proxies) pro doplňování kódu.</span><span class="sxs-lookup"><span data-stu-id="8cea2-188">Optionally, if your editor supports it, you can reference a [JSON schema](http://json.schemastore.org/proxies) for code completion.</span></span> <span data-ttu-id="8cea2-189">Příklad souboru může vypadat hello následující:</span><span class="sxs-lookup"><span data-stu-id="8cea2-189">An example file might look like hello following:</span></span>

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

<span data-ttu-id="8cea2-190">Každý proxy má popisný název, například *proxy1* v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="8cea2-190">Each proxy has a friendly name, such as *proxy1* in hello preceding example.</span></span> <span data-ttu-id="8cea2-191">Hello odpovídající proxy definici objektu je definována hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="8cea2-191">hello corresponding proxy definition object is defined by hello following properties:</span></span>

* <span data-ttu-id="8cea2-192">**matchCondition**: požadované – objekt definování hello požadavků, které aktivovat provedení hello tento proxy server.</span><span class="sxs-lookup"><span data-stu-id="8cea2-192">**matchCondition**: Required--an object defining hello requests that trigger hello execution of this proxy.</span></span> <span data-ttu-id="8cea2-193">Obsahuje dvě vlastnosti, které jsou sdíleny s [aktivace protokolu HTTP]:</span><span class="sxs-lookup"><span data-stu-id="8cea2-193">It contains two properties that are shared with [HTTP triggers]:</span></span>
    * <span data-ttu-id="8cea2-194">_metody_: pole hello HTTP metody, které hello proxy serveru reaguje na.</span><span class="sxs-lookup"><span data-stu-id="8cea2-194">_methods_: An array of hello HTTP methods that hello proxy responds to.</span></span> <span data-ttu-id="8cea2-195">Pokud není zadaný, odpoví hello proxy tooall metody HTTP na hello trasy.</span><span class="sxs-lookup"><span data-stu-id="8cea2-195">If it is not specified, hello proxy responds tooall HTTP methods on hello route.</span></span>
    * <span data-ttu-id="8cea2-196">_trasy_: požadované – definuje šablonu trasy hello, řízení, které adresy URL žádosti proxy odpoví.</span><span class="sxs-lookup"><span data-stu-id="8cea2-196">_route_: Required--defines hello route template, controlling which request URLs your proxy responds to.</span></span> <span data-ttu-id="8cea2-197">Na rozdíl od v aktivační události protokolu HTTP, není žádná výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="8cea2-197">Unlike in HTTP triggers, there is no default value.</span></span>
* <span data-ttu-id="8cea2-198">**backendUri**: adresa URL hello hello back-end prostředků toowhich hello žádosti by měla být směrovány přes proxy server.</span><span class="sxs-lookup"><span data-stu-id="8cea2-198">**backendUri**: hello URL of hello back-end resource toowhich hello request should be proxied.</span></span> <span data-ttu-id="8cea2-199">Tato hodnota může odkazovat nastavení aplikace a parametry z původní požadavek klienta hello.</span><span class="sxs-lookup"><span data-stu-id="8cea2-199">This value can reference application settings and parameters from hello original client request.</span></span> <span data-ttu-id="8cea2-200">Pokud tuto vlastnost nezadáte, odpoví Azure Functions HTTP 200 OK.</span><span class="sxs-lookup"><span data-stu-id="8cea2-200">If this property is not included, Azure Functions responds with an HTTP 200 OK.</span></span>
* <span data-ttu-id="8cea2-201">**requestOverrides**: objekt, který definuje požadavek back-end toohello transformace.</span><span class="sxs-lookup"><span data-stu-id="8cea2-201">**requestOverrides**: An object that defines transformations toohello back-end request.</span></span> <span data-ttu-id="8cea2-202">V tématu [definovat objekt requestOverrides].</span><span class="sxs-lookup"><span data-stu-id="8cea2-202">See [Define a requestOverrides object].</span></span>
* <span data-ttu-id="8cea2-203">**responseOverrides**: objekt, který definuje odpověď klienta toohello transformace.</span><span class="sxs-lookup"><span data-stu-id="8cea2-203">**responseOverrides**: An object that defines transformations toohello client response.</span></span> <span data-ttu-id="8cea2-204">V tématu [definovat objekt responseOverrides].</span><span class="sxs-lookup"><span data-stu-id="8cea2-204">See [Define a responseOverrides object].</span></span>

> [!NOTE] 
> <span data-ttu-id="8cea2-205">Vlastnost Hello trasy proxy funkce Azure nectí hello routePrefix vlastnost konfigurace hostitele funkce hello.</span><span class="sxs-lookup"><span data-stu-id="8cea2-205">hello route property Azure Functions Proxies does not honor hello routePrefix property of hello Functions host configuration.</span></span> <span data-ttu-id="8cea2-206">Pokud chcete tooinclude předpona například /api, musí být zahrnut ve vlastnosti hello trasy.</span><span class="sxs-lookup"><span data-stu-id="8cea2-206">If you want tooinclude a prefix such as /api, it must be included in hello route property.</span></span>

### <span data-ttu-id="8cea2-207"><a name="requestOverrides"></a>Definování objekt requestOverrides</span><span class="sxs-lookup"><span data-stu-id="8cea2-207"><a name="requestOverrides"></a>Define a requestOverrides object</span></span>

<span data-ttu-id="8cea2-208">objekt requestOverrides Hello definuje změny provedené toohello žádost, když hello back-end zdroj nebude volán.</span><span class="sxs-lookup"><span data-stu-id="8cea2-208">hello requestOverrides object defines changes made toohello request when hello back-end resource is called.</span></span> <span data-ttu-id="8cea2-209">Hello objektu je definována hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="8cea2-209">hello object is defined by hello following properties:</span></span>

* <span data-ttu-id="8cea2-210">**backend.Request.Method**: hello metoda HTTP, který byl použit toocall hello back-end.</span><span class="sxs-lookup"><span data-stu-id="8cea2-210">**backend.request.method**: hello HTTP method that's used toocall hello back end.</span></span>
* <span data-ttu-id="8cea2-211">**backend.Request.QueryString. \<ParameterName\>**: parametr řetězce dotazu, který se dá nastavit pro hello volání toohello back-end.</span><span class="sxs-lookup"><span data-stu-id="8cea2-211">**backend.request.querystring.\<ParameterName\>**: A query string parameter that can be set for hello call toohello back end.</span></span> <span data-ttu-id="8cea2-212">Nahraďte  *\<ParameterName\>*  s názvem hello hello parametru, které chcete tooset.</span><span class="sxs-lookup"><span data-stu-id="8cea2-212">Replace *\<ParameterName\>* with hello name of hello parameter that you want tooset.</span></span> <span data-ttu-id="8cea2-213">Pokud je zadaný hello prázdný řetězec, není parametr hello součástí hello požadavek back-end.</span><span class="sxs-lookup"><span data-stu-id="8cea2-213">If hello empty string is provided, hello parameter is not included on hello back-end request.</span></span>
* <span data-ttu-id="8cea2-214">**backend.Request.Headers. \<HeaderName\>**: hlavičku, která se dá nastavit pro hello volání toohello back-end.</span><span class="sxs-lookup"><span data-stu-id="8cea2-214">**backend.request.headers.\<HeaderName\>**: A header that can be set for hello call toohello back end.</span></span> <span data-ttu-id="8cea2-215">Nahraďte  *\<HeaderName\>*  s názvem hello hello hlavičky, které chcete tooset.</span><span class="sxs-lookup"><span data-stu-id="8cea2-215">Replace *\<HeaderName\>* with hello name of hello header that you want tooset.</span></span> <span data-ttu-id="8cea2-216">Pokud zadáte hello prázdný řetězec, není k dispozici hello hlavičky v požadavku hello back-end.</span><span class="sxs-lookup"><span data-stu-id="8cea2-216">If you provide hello empty string, hello header is not included on hello back-end request.</span></span>

<span data-ttu-id="8cea2-217">Hodnoty, můžete odkazovat z původního požadavku klienta hello parametry a nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="8cea2-217">Values can reference application settings and parameters from hello original client request.</span></span>

<span data-ttu-id="8cea2-218">Příklad konfigurace může vypadat hello následující:</span><span class="sxs-lookup"><span data-stu-id="8cea2-218">An example configuration might look like hello following:</span></span>

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

### <span data-ttu-id="8cea2-219"><a name="responseOverrides"></a>Definování objekt responseOverrides</span><span class="sxs-lookup"><span data-stu-id="8cea2-219"><a name="responseOverrides"></a>Define a responseOverrides object</span></span>

<span data-ttu-id="8cea2-220">objekt requestOverrides Hello definuje změny, které jsou vytvářeny toohello odpověď, který je předán zpět toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="8cea2-220">hello requestOverrides object defines changes that are made toohello response that's passed back toohello client.</span></span> <span data-ttu-id="8cea2-221">Hello objektu je definována hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="8cea2-221">hello object is defined by hello following properties:</span></span>

* <span data-ttu-id="8cea2-222">**response.statusCode**: toobe kód stavu hello HTTP vrátil toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="8cea2-222">**response.statusCode**: hello HTTP status code toobe returned toohello client.</span></span>
* <span data-ttu-id="8cea2-223">**response.statusReason**: toobe frázi důvodu hello HTTP vrácená toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="8cea2-223">**response.statusReason**: hello HTTP reason phrase toobe returned toohello client.</span></span>
* <span data-ttu-id="8cea2-224">**Response.body**: hello řetězcovou reprezentaci toobe textu hello vrátil toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="8cea2-224">**response.body**: hello string representation of hello body toobe returned toohello client.</span></span>
* <span data-ttu-id="8cea2-225">**Response.Headers. \<HeaderName\>**: hlavičku, která se dá nastavit pro hello odpovědi toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="8cea2-225">**response.headers.\<HeaderName\>**: A header that can be set for hello response toohello client.</span></span> <span data-ttu-id="8cea2-226">Nahraďte  *\<HeaderName\>*  s názvem hello hello hlavičky, které chcete tooset.</span><span class="sxs-lookup"><span data-stu-id="8cea2-226">Replace *\<HeaderName\>* with hello name of hello header that you want tooset.</span></span> <span data-ttu-id="8cea2-227">Pokud zadáte hello prázdný řetězec, není součástí hello hlavičky odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="8cea2-227">If you provide hello empty string, hello header is not included on hello response.</span></span>

<span data-ttu-id="8cea2-228">Hodnoty můžete odkazovat aplikace nastavení, parametry z původního požadavku klienta hello a parametry z odpovědi hello back-end.</span><span class="sxs-lookup"><span data-stu-id="8cea2-228">Values can reference application settings, parameters from hello original client request, and parameters from hello back-end response.</span></span>

<span data-ttu-id="8cea2-229">Příklad konfigurace může vypadat hello následující:</span><span class="sxs-lookup"><span data-stu-id="8cea2-229">An example configuration might look like hello following:</span></span>

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
> <span data-ttu-id="8cea2-230">V tomto příkladu textu hello se nastavuje přímo, takže ne `backendUri` vlastnost je vyžadována.</span><span class="sxs-lookup"><span data-stu-id="8cea2-230">In this example, hello body is being set directly, so no `backendUri` property is needed.</span></span> <span data-ttu-id="8cea2-231">Hello příklad ukazuje, jak je možné použít Azure funkce proxy pro mocking rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8cea2-231">hello example shows how you might use Azure Functions Proxies for mocking APIs.</span></span>

[portál Azure]: https://portal.azure.com
[aktivace protokolu HTTP]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger
[Modify hello back-end request]: #modify-backend-request
[Modify hello response]: #modify-response
[definovat objekt requestOverrides]: #requestOverrides
[definovat objekt responseOverrides]: #responseOverrides
[nastavení aplikace]: #use-appsettings
[použijte proměnné]: #using-variables
[parametry z původního požadavku klienta hello]: #request-parameters
[parametry z back-end odpovědi hello]: #response-parameters
