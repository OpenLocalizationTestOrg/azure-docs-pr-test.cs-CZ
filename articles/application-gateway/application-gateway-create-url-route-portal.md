---
title: "aaaCreate cesta založená pravidlo - Azure Application Gateway - Azure Portal | Microsoft Docs"
description: "Zjistěte, jak hello toocreate pravidlo na základě cesty pro aplikační bránu pomocí portálu"
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
ms.openlocfilehash: 21cb52c426ca5f7dfedf07a96e87fbc85d243647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-hello-portal"></a><span data-ttu-id="c0807-103">Vytvoření pravidla na základě cesty pro aplikační bránu pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="c0807-103">Create a Path-based rule for an application gateway by using hello portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c0807-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c0807-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="c0807-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="c0807-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="c0807-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c0807-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="c0807-107">Na základě cestu směrování adres URL umožňuje vám trasy tooassociate na základě hello cesty adresy URL požadavku Http.</span><span class="sxs-lookup"><span data-stu-id="c0807-107">URL Path-based routing enables you tooassociate routes based on hello URL path of Http request.</span></span> <span data-ttu-id="c0807-108">Se kontroluje, jestli je fond back-end tooa trasy pro URL hello uvedené v hello Aplikační brána nakonfigurovaná a odešle hello síťový provoz toohello definovanými fond back-end.</span><span class="sxs-lookup"><span data-stu-id="c0807-108">It checks if there is a route tooa back-end pool configured for hello URL listed in hello Application Gateway and sends hello network traffic toohello defined back-end pool.</span></span> <span data-ttu-id="c0807-109">Běžně používá pro směrování podle adresy URL je tooload vyrovnávat požadavky pro různé typy obsahu toodifferent back-end serverů fondy.</span><span class="sxs-lookup"><span data-stu-id="c0807-109">A common use for URL-based routing is tooload balance requests for different content types toodifferent back-end server pools.</span></span>

<span data-ttu-id="c0807-110">Na základě adresy URL směrování zavádí novou bránu tooapplication typ pravidla.</span><span class="sxs-lookup"><span data-stu-id="c0807-110">URL-based routing introduces a new rule type tooapplication gateway.</span></span> <span data-ttu-id="c0807-111">Application gateway poskytuje dva typy pravidel: pravidla základní a na základě cesty.</span><span class="sxs-lookup"><span data-stu-id="c0807-111">Application gateway has two rule types: basic and path-based rules.</span></span> <span data-ttu-id="c0807-112">Dobrý den pravidel základní typ, poskytuje služby kruhového dotazování pro hello back-end fondu při pravidla založená na cestu kromě tooround každý s každým distribuční, také vzorek cesty adresy URL žádosti hello bere v úvahu při výběru hello odpovídající back-endový fond.</span><span class="sxs-lookup"><span data-stu-id="c0807-112">hello basic rule type, provides round-robin service for hello back-end pools while path-based rules in addition tooround robin distribution, also takes path pattern of hello request URL into account while choosing hello appropriate backend pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="c0807-113">Scénář</span><span class="sxs-lookup"><span data-stu-id="c0807-113">Scenario</span></span>

<span data-ttu-id="c0807-114">Hello následující scénář projde vytvoření pravidla na základě cesty v existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c0807-114">hello following scenario goes through creating a Path-based rule in an existing application gateway.</span></span>
<span data-ttu-id="c0807-115">Hello scénář předpokládá, že jste již provedli kroky hello příliš[vytvoření služby Application Gateway](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c0807-115">hello scenario assumes that you have already followed hello steps too[Create an Application Gateway](application-gateway-create-gateway-portal.md).</span></span>

![Adresa URL trasy][scenario]

## <span data-ttu-id="c0807-117"><a name="createrule"></a>Vytvoření pravidla na základě cesty hello</span><span class="sxs-lookup"><span data-stu-id="c0807-117"><a name="createrule"></a>Create hello Path-based rule</span></span>

<span data-ttu-id="c0807-118">Pravidla na základě cesty vyžaduje vlastní naslouchací proces, před vytvořením pravidel hello být jisti tooverify máte toouse dostupné naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="c0807-118">A Path-based rule requires its own listener, before creating hello rule be sure tooverify you have an available listener toouse.</span></span>

### <a name="step-1"></a><span data-ttu-id="c0807-119">Krok 1</span><span class="sxs-lookup"><span data-stu-id="c0807-119">Step 1</span></span>

<span data-ttu-id="c0807-120">Přejděte toohello [portál Azure](http://portal.azure.com) a vyberte existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c0807-120">Navigate toohello [Azure portal](http://portal.azure.com) and select an existing application gateway.</span></span> <span data-ttu-id="c0807-121">Klikněte na tlačítko **pravidla**</span><span class="sxs-lookup"><span data-stu-id="c0807-121">Click **Rules**</span></span>

![Přehled služby Application Gateway][1]

### <a name="step-2"></a><span data-ttu-id="c0807-123">Krok 2</span><span class="sxs-lookup"><span data-stu-id="c0807-123">Step 2</span></span>

<span data-ttu-id="c0807-124">Klikněte na tlačítko **na základě cesty** tooadd tlačítko Nové pravidlo na základě cesty.</span><span class="sxs-lookup"><span data-stu-id="c0807-124">Click **Path-based** button tooadd a new Path-based rule.</span></span>

### <a name="step-3"></a><span data-ttu-id="c0807-125">Krok 3</span><span class="sxs-lookup"><span data-stu-id="c0807-125">Step 3</span></span>

<span data-ttu-id="c0807-126">Hello **přidat na základě cesty pravidlo** okno obsahuje dvě části.</span><span class="sxs-lookup"><span data-stu-id="c0807-126">hello **Add path-based rule** blade has two sections.</span></span> <span data-ttu-id="c0807-127">první část Hello je, kde jste definovali hello naslouchací proces a název hello hello pravidla a nastavení výchozí cesty hello.</span><span class="sxs-lookup"><span data-stu-id="c0807-127">hello first section is where you defined hello listener, hello name of hello rule and hello default path settings.</span></span> <span data-ttu-id="c0807-128">Nastavení cesty výchozí Hello jsou pro trasy, která nespadají pod hello vlastní trasy na základě cesty.</span><span class="sxs-lookup"><span data-stu-id="c0807-128">hello default path settings are for routes that do not fall under hello custom path-based route.</span></span> <span data-ttu-id="c0807-129">Druhá část hello Hello **přidat pravidlo na základě cesty** okno je, kde můžete definovat hello cesta pravidla založená na sami.</span><span class="sxs-lookup"><span data-stu-id="c0807-129">hello second section of hello **Add path-based rule** blade is where you define hello path-based rules themselves.</span></span>

<span data-ttu-id="c0807-130">**Základní nastavení**</span><span class="sxs-lookup"><span data-stu-id="c0807-130">**Basic Settings**</span></span>

* <span data-ttu-id="c0807-131">**Název** – tato hodnota je popisný název toohello pravidlo, které je přístupné hello portálu.</span><span class="sxs-lookup"><span data-stu-id="c0807-131">**Name** - This value is a friendly name toohello rule that is accessible in hello portal.</span></span>
* <span data-ttu-id="c0807-132">**Naslouchací proces** – tato hodnota je hello naslouchací proces, který se používá pro pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="c0807-132">**Listener** - This value is hello listener that is used for hello rule.</span></span>
* <span data-ttu-id="c0807-133">**Výchozí fond back-end** – toto nastavení je hello nastavení, která definuje toobe back-end hello používá pro hello výchozí pravidlo</span><span class="sxs-lookup"><span data-stu-id="c0807-133">**Default backend pool** - This setting is hello setting that defines hello back-end toobe used for hello default rule</span></span>
* <span data-ttu-id="c0807-134">**Výchozí nastavení HTTP** – toto nastavení je hello nastavení, která definuje toobe nastavení HTTP hello používá pro hello výchozí pravidlo.</span><span class="sxs-lookup"><span data-stu-id="c0807-134">**Default HTTP settings** - This setting is hello setting that defines hello HTTP settings toobe used for hello default rule.</span></span>

<span data-ttu-id="c0807-135">**Pravidla na základě cesty**</span><span class="sxs-lookup"><span data-stu-id="c0807-135">**Path-based rules**</span></span>

* <span data-ttu-id="c0807-136">**Název** – tato hodnota je pravidlo na základě toopath popisný název.</span><span class="sxs-lookup"><span data-stu-id="c0807-136">**Name** - This value is a friendly name toopath-based rule.</span></span>
* <span data-ttu-id="c0807-137">**Cesty** – toto nastavení definuje hello cesta hello pravidlo hledá při předávání provozu</span><span class="sxs-lookup"><span data-stu-id="c0807-137">**Paths** - This setting defines hello path hello rule looks for when forwarding traffic</span></span>
* <span data-ttu-id="c0807-138">**Fond back-end** – toto nastavení je hello nastavení, která definuje toobe back-end hello používá pro pravidlo hello</span><span class="sxs-lookup"><span data-stu-id="c0807-138">**Backend Pool** - This setting is hello setting that defines hello back-end toobe used for hello rule</span></span>
* <span data-ttu-id="c0807-139">**Nastavení HTTP** – toto nastavení je hello nastavení, která definuje toobe nastavení HTTP hello používá pro pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="c0807-139">**HTTP setting** - This setting is hello setting that defines hello HTTP settings toobe used for hello rule.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c0807-140">Cesty: hello seznam vzorů toomatch cesta.</span><span class="sxs-lookup"><span data-stu-id="c0807-140">Paths: hello list of path patterns toomatch.</span></span> <span data-ttu-id="c0807-141">Každý musí začínat znakem / a jediným místem hello "\*" je povolena je na konci hello.</span><span class="sxs-lookup"><span data-stu-id="c0807-141">Each must start with / and hello only place a "\*" is allowed is at hello end.</span></span> <span data-ttu-id="c0807-142">Platnými hodnotami jsou /xyz, /xyz* nebo/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="c0807-142">Valid examples are /xyz, /xyz* or /xyz/*.</span></span>  

![Přidat pravidlo na základě cesty okno s informacemi o doplnit][2]

<span data-ttu-id="c0807-144">Přidání pravidla na základě cesty tooan existující application gateway je jednoduchý proces prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="c0807-144">Adding a path-based rule tooan existing application gateway is an easy process through hello portal.</span></span> <span data-ttu-id="c0807-145">Po vytvoření pravidla na základě cesty, může být upravený tooadd dalších pravidlech.</span><span class="sxs-lookup"><span data-stu-id="c0807-145">After a path-based rule has been created, it can be edited tooadd additional rules.</span></span> 

![Přidání další pravidla na základě cesty][3]

<span data-ttu-id="c0807-147">Tento krok nakonfiguruje trasu na základě cesty.</span><span class="sxs-lookup"><span data-stu-id="c0807-147">This step configures a path-based route.</span></span> <span data-ttu-id="c0807-148">Je důležité toounderstand, že požadavky nejsou přepsaná, protože mají různé požadavky aplikační brána zkontroluje hello požadavku a basic na hello url vzor zasílá hello požadavek toohello odpovídající back endu.</span><span class="sxs-lookup"><span data-stu-id="c0807-148">It is important toounderstand that requests are not rewritten, as requests come in application gateway inspects hello request and basic on hello url pattern sends hello request toohello appropriate back-end.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c0807-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c0807-149">Next steps</span></span>

<span data-ttu-id="c0807-150">jak tooconfigure s Azure Application Gateway, snižování zátěže protokolu SSL najdete v části toolearn [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-portal.md)</span><span class="sxs-lookup"><span data-stu-id="c0807-150">toolearn how tooconfigure SSL Offloading with Azure Application Gateway, see [Configure SSL Offload](application-gateway-ssl-portal.md)</span></span>

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png
