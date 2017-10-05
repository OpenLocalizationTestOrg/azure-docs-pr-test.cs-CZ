---
title: "Vytvoření služby application gateway pomocí pravidel směrování adres URL - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tato stránka obsahuje pokyny k vytvoření, konfiguraci služby Azure application gateway pomocí pravidel směrování adres URL"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: 958049830d6753ec26635f18f8f8b2fabdec0733
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-using-path-based-routing-with-azure-cli-20"></a><span data-ttu-id="bcd06-103">Vytvoření služby application gateway pomocí na základě cestu směrování s Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bcd06-103">Create an application gateway using Path-based routing with Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="bcd06-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bcd06-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="bcd06-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="bcd06-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="bcd06-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bcd06-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="bcd06-107">Na základě cestu směrování adres URL umožňuje přidružit tras na základě cesty adresy URL požadavku Http.</span><span class="sxs-lookup"><span data-stu-id="bcd06-107">URL Path-based routing enables you to associate routes based on the URL path of an Http request.</span></span> <span data-ttu-id="bcd06-108">Se ověří, zda je trasu k back-end fondu pro adresu URL v službu Application Gateway nakonfigurovaná a odešle síťový provoz do definované fond back-end.</span><span class="sxs-lookup"><span data-stu-id="bcd06-108">It checks if there is a route to a back-end pool configured for the URL presented in the Application Gateway and sends the network traffic to the defined back-end pool.</span></span> <span data-ttu-id="bcd06-109">Běžně používá pro směrování podle adresy URL je načíst vyrovnávat požadavky pro různé typy obsahu, na jiný server back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="bcd06-109">A common use for URL-based routing is to load balance requests for different content types to different back-end server pools.</span></span>

<span data-ttu-id="bcd06-110">Na základě adresy URL směrování zavádí nový typ pravidla aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="bcd06-110">URL-based routing introduces a new rule type to application gateway.</span></span> <span data-ttu-id="bcd06-111">Application gateway poskytuje dva typy pravidel: základní a PathBasedRouting.</span><span class="sxs-lookup"><span data-stu-id="bcd06-111">Application gateway has two rule types: basic and PathBasedRouting.</span></span> <span data-ttu-id="bcd06-112">Typ základní pravidlo poskytuje kruhového dotazování služby pro back endové fondy při PathBasedRouting kromě distribučních kruhové dotazování, také vzorek cesty adresy URL žádosti bere v úvahu při výběru fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="bcd06-112">Basic rule type provides round-robin service for the back-end pools while PathBasedRouting in addition to round robin distribution, also takes path pattern of the request URL into account while choosing the back-end pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="bcd06-113">Scénář</span><span class="sxs-lookup"><span data-stu-id="bcd06-113">Scenario</span></span>

<span data-ttu-id="bcd06-114">V následujícím příkladu se Aplikační brána obsluhuje přenosy pro doménu contoso.com s dvěma fondy back-end serverů: výchozí fond serverů a fond bitové kopie serveru.</span><span class="sxs-lookup"><span data-stu-id="bcd06-114">In the following example, Application Gateway is serving traffic for contoso.com with two back-end server pools: a default server pool and an image server pool.</span></span>

<span data-ttu-id="bcd06-115">Požadavky pro http://contoso.com/image * jsou směrovány do fondu serverů bitové kopie (imagesBackendPool), pokud vzorek cesty neodpovídá, je vybrán výchozí fond serverů (appGatewayBackendPool).</span><span class="sxs-lookup"><span data-stu-id="bcd06-115">Requests for http://contoso.com/image* are routed to image server pool (imagesBackendPool), if the path pattern does not match, a default server pool (appGatewayBackendPool) is selected.</span></span>

![Adresa URL trasy](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="log-in-to-azure"></a><span data-ttu-id="bcd06-117">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="bcd06-117">Log in to Azure</span></span>

<span data-ttu-id="bcd06-118">Otevřete **Microsoft Azure příkazového řádku**a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="bcd06-118">Open the **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli
az login -u "username"
```

> [!NOTE]
> <span data-ttu-id="bcd06-119">Můžete také použít `az login` bez přepínač pro přihlášení zařízení, která vyžaduje zadání kódu v aka.ms/devicelogin.</span><span class="sxs-lookup"><span data-stu-id="bcd06-119">You can also use `az login` without the switch for device login that requires entering a code at aka.ms/devicelogin.</span></span>

<span data-ttu-id="bcd06-120">Jakmile zadáte v předchozím příkladu, je k dispozici kód.</span><span class="sxs-lookup"><span data-stu-id="bcd06-120">Once you type the preceding example, a code is provided.</span></span> <span data-ttu-id="bcd06-121">Přejděte do https://aka.ms/devicelogin ve prohlížeči a pokračujte v procesu přihlášení.</span><span class="sxs-lookup"><span data-stu-id="bcd06-121">Navigate to https://aka.ms/devicelogin in a browser to continue the login process.</span></span>

![cmd zobrazující zařízení přihlášení][1]

<span data-ttu-id="bcd06-123">V prohlížeči zadejte kód, který jste dostali.</span><span class="sxs-lookup"><span data-stu-id="bcd06-123">In the browser, enter the code you received.</span></span> <span data-ttu-id="bcd06-124">Budete přesměrováni na stránku přihlášení.</span><span class="sxs-lookup"><span data-stu-id="bcd06-124">You are redirected to a sign-in page.</span></span>

![prohlížeče k zadání kódu][2]

<span data-ttu-id="bcd06-126">Jakmile byl zadán kód jste přihlášeni, zavřete prohlížeč pokračovat na tento scénář.</span><span class="sxs-lookup"><span data-stu-id="bcd06-126">Once the code has been entered you are signed in, close the browser to continue on with the scenario.</span></span>

![Úspěšné přihlášení][3]

## <a name="add-a-path-based-rule-to-an-existing-application-gateway"></a><span data-ttu-id="bcd06-128">Přidat pravidlo založené na cestu k existující aplikační brány</span><span class="sxs-lookup"><span data-stu-id="bcd06-128">Add a path-based rule to an existing application gateway</span></span>

<span data-ttu-id="bcd06-129">Vytvoření služby application gateway s definované pravidlo cesty</span><span class="sxs-lookup"><span data-stu-id="bcd06-129">Create an application gateway with a path rule defined</span></span>

### <a name="create-a-new-back-end-pool"></a><span data-ttu-id="bcd06-130">Vytvoření nového fondu back-end</span><span class="sxs-lookup"><span data-stu-id="bcd06-130">Create a new back-end pool</span></span>

<span data-ttu-id="bcd06-131">Konfigurace nastavení brány aplikace **imagesBackendPool** pro síťový provoz s vyrovnáváním zatížení ve fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="bcd06-131">Configure application gateway setting **imagesBackendPool** for the load-balanced network traffic in the back-end pool.</span></span> <span data-ttu-id="bcd06-132">V tomto příkladu nakonfigurujete nastavení jiný fond back-end pro nový fond back-end.</span><span class="sxs-lookup"><span data-stu-id="bcd06-132">In this example, you configure different back-end pool settings for the new back-end pool.</span></span> <span data-ttu-id="bcd06-133">Každý fond back-end může mít vlastní nastavení fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="bcd06-133">Each back-end pool can have its own back-end pool setting.</span></span>  <span data-ttu-id="bcd06-134">Nastavení back-endu HTTP jsou využívána pravidly pro směrování provozu do správných členů fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="bcd06-134">Backend HTTP settings are used by rules to route traffic to the correct backend pool members.</span></span> <span data-ttu-id="bcd06-135">Určuje protokol a port, který se používá při odesílání provozu do členy fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="bcd06-135">This determines the protocol and port that is used when sending traffic to the backend pool members.</span></span> <span data-ttu-id="bcd06-136">Podle nastavení HTTP back-endu se určují i relace založené na souborech cookie.</span><span class="sxs-lookup"><span data-stu-id="bcd06-136">Cookie-based sessions are also determined by the backend HTTP settings.</span></span>  <span data-ttu-id="bcd06-137">Pokud je tato funkce povolena, spřažení relace založené na souborech cookie odesílá provoz do stejného back-endu jako předchozí požadavky pro jednotlivé pakety.</span><span class="sxs-lookup"><span data-stu-id="bcd06-137">If enabled, cookie-based session affinity sends traffic to the same backend as previous requests for each packet.</span></span>

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port"></a><span data-ttu-id="bcd06-138">Vytvořit nový port front-end</span><span class="sxs-lookup"><span data-stu-id="bcd06-138">Create a new front-end port</span></span>

<span data-ttu-id="bcd06-139">Nakonfigurujte front-end port pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="bcd06-139">Configure the front-end port for an application gateway.</span></span> <span data-ttu-id="bcd06-140">Objekt konfigurace portu front-end je používán naslouchacím procesem k definování portu, na kterém služba Application Gateway v naslouchacím procesu naslouchá provozu.</span><span class="sxs-lookup"><span data-stu-id="bcd06-140">The front-end port configuration object is used by a listener to define what port the Application Gateway listens for traffic on the listener.</span></span>

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a><span data-ttu-id="bcd06-141">Vytvořte nový</span><span class="sxs-lookup"><span data-stu-id="bcd06-141">Create a new listener</span></span>

<span data-ttu-id="bcd06-142">Nakonfigurujte naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="bcd06-142">Configure the listener.</span></span> <span data-ttu-id="bcd06-143">V tomto kroku je nakonfigurován naslouchací proces pro veřejnou IP adresu a port používaný pro příjem příchozího síťového provozu.</span><span class="sxs-lookup"><span data-stu-id="bcd06-143">This step configures the listener for the public IP address and port used to receive incoming network traffic.</span></span> <span data-ttu-id="bcd06-144">Následující příklad trvá dříve nakonfigurované konfiguraci front-end IP adresy, konfigurace front-end port a protokol (http nebo https) a nakonfiguruje naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="bcd06-144">The following example takes the previously configured front-end IP configuration,  front-end port configuration, and a protocol (http or https) and configures the listener.</span></span> <span data-ttu-id="bcd06-145">V tomto příkladu naslouchá naslouchací proces HTTP přenosy na portu 82 na veřejnou IP adresu, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="bcd06-145">In this example, the listener listens to HTTP traffic on port 82 on the public IP address that was created earlier.</span></span>

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-the-url-path-map"></a><span data-ttu-id="bcd06-146">Vytvořit mapování cesty adresy Url</span><span class="sxs-lookup"><span data-stu-id="bcd06-146">Create the Url path map</span></span>

<span data-ttu-id="bcd06-147">Konfigurovat pravidla cesty adresy URL pro back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="bcd06-147">Configure URL rule paths for the back-end pools.</span></span> <span data-ttu-id="bcd06-148">Tento krok nakonfiguruje relativní cestu používá aplikační bránu můžete definovat mapování mezi cestu adresy URL a je mu přiřazená kterému fondu back-end pro zpracování příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="bcd06-148">This step configures the relative path used by application gateway to define the mapping between URL path and which back-end pool is assigned to handle the incoming traffic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bcd06-149">Každá cesta musí začínat znakem / a bude jediným místem "\*" je povolena, je na konci.</span><span class="sxs-lookup"><span data-stu-id="bcd06-149">Each path must start with / and the only place a "\*" is allowed, is at the end.</span></span> <span data-ttu-id="bcd06-150">Platnými hodnotami jsou /xyz, /xyz* nebo/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="bcd06-150">Valid examples are /xyz, /xyz* or /xyz/*.</span></span> <span data-ttu-id="bcd06-151">Řetězec dodáni do objekt přiřazení vzorce cesta nezahrnuje jakýkoli text po první "?" nebo "#" a tyto znaky nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="bcd06-151">The string fed to the path matcher does not include any text after the first "?" or "#", and those characters are not allowed.</span></span> 

<span data-ttu-id="bcd06-152">Následující příklad vytvoří jedno pravidlo pro "/ Image / *" cestu směrování provozu na back-end "imagesBackendPool."</span><span class="sxs-lookup"><span data-stu-id="bcd06-152">The following example creates one rule for "/images/*" path routing traffic to back-end "imagesBackendPool."</span></span> <span data-ttu-id="bcd06-153">Toto pravidlo zajišťuje, že přenosy dat pro každou sadu adresy URL se směruje na back-end.</span><span class="sxs-lookup"><span data-stu-id="bcd06-153">This rule ensures that traffic for each set of urls is routed to the backend.</span></span> <span data-ttu-id="bcd06-154">Například http://adatum.com/images/figure1.jpg přejde na "imagesBackendPool."</span><span class="sxs-lookup"><span data-stu-id="bcd06-154">For example, http://adatum.com/images/figure1.jpg goes to "imagesBackendPool."</span></span> <span data-ttu-id="bcd06-155">Pokud cesta neodpovídá žádné z pravidel předem definovaná cesta, nakonfiguruje konfiguraci pravidla cesty mapy taky výchozího fondu adres back-end.</span><span class="sxs-lookup"><span data-stu-id="bcd06-155">If the path doesn't match any of the pre-defined path rules, the rule path map configuration also configures a default back-end address pool.</span></span> <span data-ttu-id="bcd06-156">Například http://adatum.com/shoppingcart/test.html přejde na pool1, jako je definován jako výchozí fond pro neodpovídající provoz.</span><span class="sxs-lookup"><span data-stu-id="bcd06-156">For example, http://adatum.com/shoppingcart/test.html goes to pool1 as it is defined as the default pool for unmatched traffic.</span></span>

```azurecli-interactive
az network application-gateway url-path-map create \
--gateway-name AdatumAppGateway \
--name imagespathmap \
--paths /images/* \
--resource-group myresourcegroup2 \
--address-pool imagesBackendPool \
--default-address-pool appGatewayBackendPool \
--default-http-settings appGatewayBackendHttpSettings \
--http-settings appGatewayBackendHttpSettings \
--rule-name images
```

## <a name="next-steps"></a><span data-ttu-id="bcd06-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bcd06-157">Next steps</span></span>

<span data-ttu-id="bcd06-158">Pokud chcete další informace o přesměrování zpracování Secure Sockets Layer (SSL), najdete v části [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl-cli.md).</span><span class="sxs-lookup"><span data-stu-id="bcd06-158">If you want to learn about Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl-cli.md).</span></span>


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
