---
title: "pravidla pro aplikační bránu pomocí směrování adres URL - aaaCreate 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate, konfigurace služby Azure application gateway pomocí pravidel směrování adres URL"
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
ms.openlocfilehash: 335b52be258945e1172eb0252b732e0e6ecb2ef0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-using-path-based-routing-with-azure-cli-20"></a><span data-ttu-id="ebdc4-103">Vytvoření služby application gateway pomocí na základě cestu směrování s Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ebdc4-103">Create an application gateway using Path-based routing with Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ebdc4-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ebdc4-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="ebdc4-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="ebdc4-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="ebdc4-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ebdc4-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="ebdc4-107">Na základě cestu směrování adres URL umožňuje vám trasy tooassociate na základě hello cesty adresy URL požadavku Http.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-107">URL Path-based routing enables you tooassociate routes based on hello URL path of an Http request.</span></span> <span data-ttu-id="ebdc4-108">Se kontroluje, jestli je fond back-end tooa trasy pro URL hello uvedené v hello Aplikační brána nakonfigurovaná a odešle hello síťový provoz toohello definovanými fond back-end.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-108">It checks if there is a route tooa back-end pool configured for hello URL presented in hello Application Gateway and sends hello network traffic toohello defined back-end pool.</span></span> <span data-ttu-id="ebdc4-109">Běžně používá pro směrování podle adresy URL je tooload vyrovnávat požadavky pro různé typy obsahu toodifferent back-end serverů fondy.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-109">A common use for URL-based routing is tooload balance requests for different content types toodifferent back-end server pools.</span></span>

<span data-ttu-id="ebdc4-110">Na základě adresy URL směrování zavádí novou bránu tooapplication typ pravidla.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-110">URL-based routing introduces a new rule type tooapplication gateway.</span></span> <span data-ttu-id="ebdc4-111">Application gateway poskytuje dva typy pravidel: základní a PathBasedRouting.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-111">Application gateway has two rule types: basic and PathBasedRouting.</span></span> <span data-ttu-id="ebdc4-112">Typ základní pravidlo poskytuje kruhového dotazování služby pro hello back-end fondy při PathBasedRouting kromě tooround každý s každým distribuční, také vzorek cesty adresy URL žádosti hello bere v úvahu při výběru fondu back-end hello.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-112">Basic rule type provides round-robin service for hello back-end pools while PathBasedRouting in addition tooround robin distribution, also takes path pattern of hello request URL into account while choosing hello back-end pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="ebdc4-113">Scénář</span><span class="sxs-lookup"><span data-stu-id="ebdc4-113">Scenario</span></span>

<span data-ttu-id="ebdc4-114">V následujícím příkladu hello, Application Gateway obsluhuje přenosy pro doménu contoso.com s dvěma fondy back-end serverů: výchozí fond serverů a fond bitové kopie serveru.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-114">In hello following example, Application Gateway is serving traffic for contoso.com with two back-end server pools: a default server pool and an image server pool.</span></span>

<span data-ttu-id="ebdc4-115">Požadavky pro http://contoso.com/image * jsou směrovány fondu serverů tooimage (imagesBackendPool), pokud hello vzorek cesty neodpovídá, je vybraný výchozí fond serverů (appGatewayBackendPool).</span><span class="sxs-lookup"><span data-stu-id="ebdc4-115">Requests for http://contoso.com/image* are routed tooimage server pool (imagesBackendPool), if hello path pattern does not match, a default server pool (appGatewayBackendPool) is selected.</span></span>

![Adresa URL trasy](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="log-in-tooazure"></a><span data-ttu-id="ebdc4-117">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="ebdc4-117">Log in tooAzure</span></span>

<span data-ttu-id="ebdc4-118">Otevřete hello **Microsoft Azure příkazového řádku**a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-118">Open hello **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli
az login -u "username"
```

> [!NOTE]
> <span data-ttu-id="ebdc4-119">Můžete také použít `az login` bez hello přepínač pro přihlášení zařízení, která vyžaduje zadání kódu v aka.ms/devicelogin.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-119">You can also use `az login` without hello switch for device login that requires entering a code at aka.ms/devicelogin.</span></span>

<span data-ttu-id="ebdc4-120">Jakmile zadáte hello předchozím příkladu, je k dispozici kód.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-120">Once you type hello preceding example, a code is provided.</span></span> <span data-ttu-id="ebdc4-121">Přejděte toohttps://aka.ms/devicelogin v procesu přihlášení hello toocontinue prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-121">Navigate toohttps://aka.ms/devicelogin in a browser toocontinue hello login process.</span></span>

![cmd zobrazující zařízení přihlášení][1]

<span data-ttu-id="ebdc4-123">V prohlížeči hello zadejte kód hello, kterou jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-123">In hello browser, enter hello code you received.</span></span> <span data-ttu-id="ebdc4-124">Jste přihlašovací stránku přesměrovaného tooa.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-124">You are redirected tooa sign-in page.</span></span>

![Prohlížeč tooenter kódu][2]

<span data-ttu-id="ebdc4-126">Jakmile byl zadán kód hello jste přihlášeni, Zavřít hello prohlížeče toocontinue se scénářem hello.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-126">Once hello code has been entered you are signed in, close hello browser toocontinue on with hello scenario.</span></span>

![Úspěšné přihlášení][3]

## <a name="add-a-path-based-rule-tooan-existing-application-gateway"></a><span data-ttu-id="ebdc4-128">Přidat bránu existující aplikace na základě cesty pravidlo tooan</span><span class="sxs-lookup"><span data-stu-id="ebdc4-128">Add a path-based rule tooan existing application gateway</span></span>

<span data-ttu-id="ebdc4-129">Vytvoření služby application gateway s definované pravidlo cesty</span><span class="sxs-lookup"><span data-stu-id="ebdc4-129">Create an application gateway with a path rule defined</span></span>

### <a name="create-a-new-back-end-pool"></a><span data-ttu-id="ebdc4-130">Vytvoření nového fondu back-end</span><span class="sxs-lookup"><span data-stu-id="ebdc4-130">Create a new back-end pool</span></span>

<span data-ttu-id="ebdc4-131">Konfigurace nastavení brány aplikace **imagesBackendPool** hello Vyrovnávání zatížení síťových přenosů ve fondu back-end hello.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-131">Configure application gateway setting **imagesBackendPool** for hello load-balanced network traffic in hello back-end pool.</span></span> <span data-ttu-id="ebdc4-132">V tomto příkladu nakonfigurujete nastavení jiný fond back-end pro nový fond back-end hello.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-132">In this example, you configure different back-end pool settings for hello new back-end pool.</span></span> <span data-ttu-id="ebdc4-133">Každý fond back-end může mít vlastní nastavení fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-133">Each back-end pool can have its own back-end pool setting.</span></span>  <span data-ttu-id="ebdc4-134">Nastavení HTTP back-end jsou používány pravidla tooroute provoz toohello správné back-end členy fondu.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-134">Backend HTTP settings are used by rules tooroute traffic toohello correct backend pool members.</span></span> <span data-ttu-id="ebdc4-135">Určuje hello protokol a port, který se používá při odesílání provozu toohello členy fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-135">This determines hello protocol and port that is used when sending traffic toohello backend pool members.</span></span> <span data-ttu-id="ebdc4-136">Na základě souborů cookie relací jsou určeny také nastavení HTTP back-end hello.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-136">Cookie-based sessions are also determined by hello backend HTTP settings.</span></span>  <span data-ttu-id="ebdc4-137">Pokud je povoleno, spřažení relace na základě souborů cookie odešle provoz toohello stejnou back-end jako předchozí požadavky jednotlivých paketů.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-137">If enabled, cookie-based session affinity sends traffic toohello same backend as previous requests for each packet.</span></span>

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port"></a><span data-ttu-id="ebdc4-138">Vytvořit nový port front-end</span><span class="sxs-lookup"><span data-stu-id="ebdc4-138">Create a new front-end port</span></span>

<span data-ttu-id="ebdc4-139">Nakonfigurujte port front-end hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-139">Configure hello front-end port for an application gateway.</span></span> <span data-ttu-id="ebdc4-140">objekt konfigurace Hello front-end port je používán toodefine naslouchací proces port hello Application Gateway naslouchá provoz na hello naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-140">hello front-end port configuration object is used by a listener toodefine what port hello Application Gateway listens for traffic on hello listener.</span></span>

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a><span data-ttu-id="ebdc4-141">Vytvořte nový</span><span class="sxs-lookup"><span data-stu-id="ebdc4-141">Create a new listener</span></span>

<span data-ttu-id="ebdc4-142">Nakonfigurujte hello naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-142">Configure hello listener.</span></span> <span data-ttu-id="ebdc4-143">Tento krok nakonfiguruje hello naslouchací proces pro hello veřejnou IP adresu a port používá tooreceive příchozí síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-143">This step configures hello listener for hello public IP address and port used tooreceive incoming network traffic.</span></span> <span data-ttu-id="ebdc4-144">Následující ukázka Hello trvá hello dříve nakonfigurované konfiguraci front-end IP adresy, konfigurace front-end port a protokol (http nebo https) a nakonfiguruje hello naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-144">hello following example takes hello previously configured front-end IP configuration,  front-end port configuration, and a protocol (http or https) and configures hello listener.</span></span> <span data-ttu-id="ebdc4-145">V tomto příkladu naslouchá naslouchací proces hello tooHTTP přenosy na portu 82 na hello veřejnou IP adresu, která jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-145">In this example, hello listener listens tooHTTP traffic on port 82 on hello public IP address that was created earlier.</span></span>

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-hello-url-path-map"></a><span data-ttu-id="ebdc4-146">Vytvořit mapování cesty adresy Url hello</span><span class="sxs-lookup"><span data-stu-id="ebdc4-146">Create hello Url path map</span></span>

<span data-ttu-id="ebdc4-147">Konfigurovat pravidla cesty adresy URL pro hello back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-147">Configure URL rule paths for hello back-end pools.</span></span> <span data-ttu-id="ebdc4-148">Tento krok nakonfiguruje hello relativní cesta používaná systémem brány toodefine hello mapování mezi cestu adresy URL a kterému fondu back-end je přiřazen toohandle hello příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-148">This step configures hello relative path used by application gateway toodefine hello mapping between URL path and which back-end pool is assigned toohandle hello incoming traffic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ebdc4-149">Každá cesta musí začínat znakem / a jediným místem hello "\*" je povolena, je na konci hello.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-149">Each path must start with / and hello only place a "\*" is allowed, is at hello end.</span></span> <span data-ttu-id="ebdc4-150">Platnými hodnotami jsou /xyz, /xyz* nebo/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-150">Valid examples are /xyz, /xyz* or /xyz/*.</span></span> <span data-ttu-id="ebdc4-151">Hello řetězec dodáni toohello cesta objekt přiřazení vzorce nezahrnuje jakýkoli text po hello nejprve "?" nebo "#" a tyto znaky nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-151">hello string fed toohello path matcher does not include any text after hello first "?" or "#", and those characters are not allowed.</span></span> 

<span data-ttu-id="ebdc4-152">Hello následující příklad vytvoří jedno pravidlo pro "/ Image / *" cestu směrování provozu tooback-end "imagesBackendPool."</span><span class="sxs-lookup"><span data-stu-id="ebdc4-152">hello following example creates one rule for "/images/*" path routing traffic tooback-end "imagesBackendPool."</span></span> <span data-ttu-id="ebdc4-153">Toto pravidlo zajišťuje, že přenosy dat pro každou sadu adres URL směrované toohello back-end.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-153">This rule ensures that traffic for each set of urls is routed toohello backend.</span></span> <span data-ttu-id="ebdc4-154">Například http://adatum.com/images/figure1.jpg přejde příliš "imagesBackendPool."</span><span class="sxs-lookup"><span data-stu-id="ebdc4-154">For example, http://adatum.com/images/figure1.jpg goes too"imagesBackendPool."</span></span> <span data-ttu-id="ebdc4-155">Pokud cesta hello neodpovídá žádné z pravidel hello předem definovaná cesta, hello pravidla cesty mapy konfigurace nakonfiguruje taky výchozího fondu adres back-end.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-155">If hello path doesn't match any of hello pre-defined path rules, hello rule path map configuration also configures a default back-end address pool.</span></span> <span data-ttu-id="ebdc4-156">Například http://adatum.com/shoppingcart/test.html přejde toopool1, jako je definována jako hello výchozí fond pro neodpovídající provoz.</span><span class="sxs-lookup"><span data-stu-id="ebdc4-156">For example, http://adatum.com/shoppingcart/test.html goes toopool1 as it is defined as hello default pool for unmatched traffic.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="ebdc4-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ebdc4-157">Next steps</span></span>

<span data-ttu-id="ebdc4-158">Pokud chcete toolearn o přesměrování zpracování Secure Sockets Layer (SSL), najdete v části [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl-cli.md).</span><span class="sxs-lookup"><span data-stu-id="ebdc4-158">If you want toolearn about Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl-cli.md).</span></span>


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
