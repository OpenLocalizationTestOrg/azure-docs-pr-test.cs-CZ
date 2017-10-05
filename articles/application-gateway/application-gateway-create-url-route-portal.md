---
title: "Vytvoření pravidla na základě cesta - Azure Application Gateway - portálu Azure | Microsoft Docs"
description: "Naučte se vytvářet pravidla na základě cesty pro aplikační bránu pomocí portálu"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 87bd93bc-e1a6-45db-a226-555948f1feb7
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: gwallace
ms.openlocfilehash: c184e94a04cfbdedcae70ed154aeb7dd134d1baf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a><span data-ttu-id="c3fd1-103">Vytvoření pravidla na základě cesty pro aplikační bránu pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="c3fd1-103">Create a Path-based rule for an application gateway by using the portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c3fd1-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c3fd1-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="c3fd1-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="c3fd1-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="c3fd1-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c3fd1-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="c3fd1-107">Na základě cestu směrování adres URL umožňuje přidružit tras na základě cesty adresy URL protokolu Http žádosti.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-107">URL Path-based routing enables you to associate routes based on the URL path of Http request.</span></span> <span data-ttu-id="c3fd1-108">Se ověří, zda je trasu k back-end fondu pro adresu URL uvedené v bráně aplikace nakonfigurovaná a odešle síťový provoz do definované fond back-end.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-108">It checks if there is a route to a back-end pool configured for the URL listed in the Application Gateway and sends the network traffic to the defined back-end pool.</span></span> <span data-ttu-id="c3fd1-109">Běžně používá pro směrování podle adresy URL je načíst vyrovnávat požadavky pro různé typy obsahu, na jiný server back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-109">A common use for URL-based routing is to load balance requests for different content types to different back-end server pools.</span></span>

<span data-ttu-id="c3fd1-110">Na základě adresy URL směrování zavádí nový typ pravidla aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-110">URL-based routing introduces a new rule type to application gateway.</span></span> <span data-ttu-id="c3fd1-111">Application gateway poskytuje dva typy pravidel: pravidla základní a na základě cesty.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-111">Application gateway has two rule types: basic and path-based rules.</span></span> <span data-ttu-id="c3fd1-112">Typ základní pravidlo poskytuje kruhového dotazování služby pro back endové fondy při pravidla na základě cesty kromě distribučních kruhové dotazování, také vzorek cesty adresy URL žádosti bere v úvahu při výběru příslušné back-endový fond.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-112">The basic rule type, provides round-robin service for the back-end pools while path-based rules in addition to round robin distribution, also takes path pattern of the request URL into account while choosing the appropriate backend pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="c3fd1-113">Scénář</span><span class="sxs-lookup"><span data-stu-id="c3fd1-113">Scenario</span></span>

<span data-ttu-id="c3fd1-114">V následujícím scénáři projde vytvoření pravidla na základě cesty v existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-114">The following scenario goes through creating a Path-based rule in an existing application gateway.</span></span>
<span data-ttu-id="c3fd1-115">Tento scénář předpokládá, že jste již provedli postup [vytvoření služby Application Gateway](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c3fd1-115">The scenario assumes that you have already followed the steps to [Create an Application Gateway](application-gateway-create-gateway-portal.md).</span></span>

![Adresa URL trasy][scenario]

## <span data-ttu-id="c3fd1-117"><a name="createrule"></a>Vytvoření pravidla na základě cesty</span><span class="sxs-lookup"><span data-stu-id="c3fd1-117"><a name="createrule"></a>Create the Path-based rule</span></span>

<span data-ttu-id="c3fd1-118">Pravidla na základě cesty vyžaduje vlastní naslouchací proces, předtím, než při vytváření pravidla ověřte máte k dispozici naslouchací proces používat.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-118">A Path-based rule requires its own listener, before creating the rule be sure to verify you have an available listener to use.</span></span>

### <a name="step-1"></a><span data-ttu-id="c3fd1-119">Krok 1</span><span class="sxs-lookup"><span data-stu-id="c3fd1-119">Step 1</span></span>

<span data-ttu-id="c3fd1-120">Přejděte na [portál Azure](http://portal.azure.com) a vyberte existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-120">Navigate to the [Azure portal](http://portal.azure.com) and select an existing application gateway.</span></span> <span data-ttu-id="c3fd1-121">Klikněte na tlačítko **pravidla**</span><span class="sxs-lookup"><span data-stu-id="c3fd1-121">Click **Rules**</span></span>

![Přehled služby Application Gateway][1]

### <a name="step-2"></a><span data-ttu-id="c3fd1-123">Krok 2</span><span class="sxs-lookup"><span data-stu-id="c3fd1-123">Step 2</span></span>

<span data-ttu-id="c3fd1-124">Klikněte na tlačítko **na základě cesty** tlačítko Přidat nové pravidlo na základě cesty.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-124">Click **Path-based** button to add a new Path-based rule.</span></span>

### <a name="step-3"></a><span data-ttu-id="c3fd1-125">Krok 3</span><span class="sxs-lookup"><span data-stu-id="c3fd1-125">Step 3</span></span>

<span data-ttu-id="c3fd1-126">**Přidat na základě cesty pravidlo** okno obsahuje dvě části.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-126">The **Add path-based rule** blade has two sections.</span></span> <span data-ttu-id="c3fd1-127">V první části je, kde jste definovali naslouchací proces, název pravidla a nastavení výchozí cesty.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-127">The first section is where you defined the listener, the name of the rule and the default path settings.</span></span> <span data-ttu-id="c3fd1-128">Výchozí cesta nastavení jsou pro trasy, která nespadají pod vlastní trasy na základě cesty.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-128">The default path settings are for routes that do not fall under the custom path-based route.</span></span> <span data-ttu-id="c3fd1-129">Druhá část **přidat na základě cesty pravidlo** je okno, kde můžete definovat pravidla na základě cesty sami.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-129">The second section of the **Add path-based rule** blade is where you define the path-based rules themselves.</span></span>

<span data-ttu-id="c3fd1-130">**Základní nastavení**</span><span class="sxs-lookup"><span data-stu-id="c3fd1-130">**Basic Settings**</span></span>

* <span data-ttu-id="c3fd1-131">**Název** – tato hodnota je popisný název pravidla, která je přístupná na portálu.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-131">**Name** - This value is a friendly name to the rule that is accessible in the portal.</span></span>
* <span data-ttu-id="c3fd1-132">**Naslouchací proces** – tato hodnota je naslouchací proces, který se používá pro pravidlo.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-132">**Listener** - This value is the listener that is used for the rule.</span></span>
* <span data-ttu-id="c3fd1-133">**Výchozí fond back-end** – toto nastavení je nastavení, která definuje back-end, který se má použít pro výchozí pravidlo</span><span class="sxs-lookup"><span data-stu-id="c3fd1-133">**Default backend pool** - This setting is the setting that defines the back-end to be used for the default rule</span></span>
* <span data-ttu-id="c3fd1-134">**Výchozí nastavení HTTP** – toto nastavení je nastavení, které definuje nastavení protokolu HTTP, která má být použit pro výchozí pravidlo.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-134">**Default HTTP settings** - This setting is the setting that defines the HTTP settings to be used for the default rule.</span></span>

<span data-ttu-id="c3fd1-135">**Pravidla na základě cesty**</span><span class="sxs-lookup"><span data-stu-id="c3fd1-135">**Path-based rules**</span></span>

* <span data-ttu-id="c3fd1-136">**Název** – tato hodnota je popisný název, který pravidlem cesty.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-136">**Name** - This value is a friendly name to path-based rule.</span></span>
* <span data-ttu-id="c3fd1-137">**Cesty** – toto nastavení definuje cestu toto pravidlo vyhledá při předávání provozu</span><span class="sxs-lookup"><span data-stu-id="c3fd1-137">**Paths** - This setting defines the path the rule looks for when forwarding traffic</span></span>
* <span data-ttu-id="c3fd1-138">**Fond back-end** – toto nastavení je nastavení, která definuje back-end, který se má použít pro pravidlo</span><span class="sxs-lookup"><span data-stu-id="c3fd1-138">**Backend Pool** - This setting is the setting that defines the back-end to be used for the rule</span></span>
* <span data-ttu-id="c3fd1-139">**Nastavení HTTP** – toto nastavení je nastavení, které definuje nastavení protokolu HTTP, která má být použit pro pravidlo.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-139">**HTTP setting** - This setting is the setting that defines the HTTP settings to be used for the rule.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c3fd1-140">Cesty: Seznam vzorů cesta tak, aby odpovídaly.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-140">Paths: The list of path patterns to match.</span></span> <span data-ttu-id="c3fd1-141">Každý musí začínat znakem / a bude jediným místem "\*" je povolena je na konci.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-141">Each must start with / and the only place a "\*" is allowed is at the end.</span></span> <span data-ttu-id="c3fd1-142">Platnými hodnotami jsou /xyz, /xyz* nebo/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-142">Valid examples are /xyz, /xyz* or /xyz/*.</span></span>  

![Přidat pravidlo na základě cesty okno s informacemi o doplnit][2]

<span data-ttu-id="c3fd1-144">Přidávání pravidla na základě cestu k existující aplikační brány je jednoduchý proces prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-144">Adding a path-based rule to an existing application gateway is an easy process through the portal.</span></span> <span data-ttu-id="c3fd1-145">Po vytvoření pravidla na základě cesty, se dá upravit pro přidání dalších pravidlech.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-145">After a path-based rule has been created, it can be edited to add additional rules.</span></span> 

![Přidání další pravidla na základě cesty][3]

<span data-ttu-id="c3fd1-147">Tento krok nakonfiguruje trasu na základě cesty.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-147">This step configures a path-based route.</span></span> <span data-ttu-id="c3fd1-148">Je důležité pochopit, které požadavky nejsou přepsaná, protože mají různé požadavky aplikační brána zkontroluje žádosti a basic na vzor adresy url odešle požadavek na příslušné back-end.</span><span class="sxs-lookup"><span data-stu-id="c3fd1-148">It is important to understand that requests are not rewritten, as requests come in application gateway inspects the request and basic on the url pattern sends the request to the appropriate back-end.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c3fd1-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c3fd1-149">Next steps</span></span>

<span data-ttu-id="c3fd1-150">Další postup konfigurace snižování zátěže protokolu SSL s Azure Application Gateway najdete v tématu [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-portal.md)</span><span class="sxs-lookup"><span data-stu-id="c3fd1-150">To learn how to configure SSL Offloading with Azure Application Gateway, see [Configure SSL Offload](application-gateway-ssl-portal.md)</span></span>

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png
