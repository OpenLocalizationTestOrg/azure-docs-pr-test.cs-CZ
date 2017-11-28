---
title: "Hostitelem více webů s Azure Application Gateway | Microsoft Docs"
description: "Tato stránka obsahuje pokyny ke konfiguraci služby Azure application gateway existující pro hostování několika webových aplikací na stejnou bránu pomocí portálu Azure."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 95f892f6-fa27-47ee-b980-7abf4f2c66a9
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 84bd62ae17b7f7ba4cd815ef1f9880679607ebce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a><span data-ttu-id="d7d42-103">Konfigurace existující aplikační brány pro hostování několika webových aplikací</span><span class="sxs-lookup"><span data-stu-id="d7d42-103">Configure an existing application gateway for hosting multiple web applications</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d7d42-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d7d42-104">Azure portal</span></span>](application-gateway-create-multisite-portal.md)
> * [<span data-ttu-id="d7d42-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="d7d42-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-multisite-azureresourcemanager-powershell.md)
> 
> 

<span data-ttu-id="d7d42-106">Hostování více lokalitu umožňuje nasadit více než jednu webovou aplikaci ve stejném application gateway.</span><span class="sxs-lookup"><span data-stu-id="d7d42-106">Multiple site hosting allows you to deploy more than one web application on the same application gateway.</span></span> <span data-ttu-id="d7d42-107">Přitom spoléhá na přítomnost Hlavička hostitele v příchozím požadavku HTTP, chcete-li zjistit, který naslouchací proces by přijímat přenosy.</span><span class="sxs-lookup"><span data-stu-id="d7d42-107">It relies on presence of host header in the incoming HTTP request, to determine which listener would receive traffic.</span></span> <span data-ttu-id="d7d42-108">Naslouchací proces pak přesměruje přenosy na příslušné back-end fondu podle konfigurace v definici pravidla brány.</span><span class="sxs-lookup"><span data-stu-id="d7d42-108">The listener then directs traffic to appropriate backend pool as configured in the rules definition of the gateway.</span></span> <span data-ttu-id="d7d42-109">V protokolu SSL povoleno webových aplikací Aplikační brána spoléhá na toto rozšíření indikace názvu serveru (SNI) vyberte správné naslouchací proces pro webový provoz.</span><span class="sxs-lookup"><span data-stu-id="d7d42-109">In SSL enabled web applications, application gateway relies on the Server Name Indication (SNI) extension to choose the correct listener for the web traffic.</span></span> <span data-ttu-id="d7d42-110">Běžně používá pro více hostování lokality je načíst vyrovnávat požadavky na jiných webových domén na jiný server back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="d7d42-110">A common use for multiple site hosting is to load balance requests for different web domains to different back-end server pools.</span></span> <span data-ttu-id="d7d42-111">Více subdomény stejné kořenové domény podobně také může být hostovaný na stejné aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="d7d42-111">Similarly multiple subdomains of the same root domain could also be hosted on the same application gateway.</span></span>

## <a name="scenario"></a><span data-ttu-id="d7d42-112">Scénář</span><span class="sxs-lookup"><span data-stu-id="d7d42-112">Scenario</span></span>

<span data-ttu-id="d7d42-113">V následujícím příkladu se Aplikační brána obsluhuje přenosy dat pro contoso.com a fabrikam.com s dvěma fondy back-end serverů: contoso fondu serverů a fond serverů fabrikam.</span><span class="sxs-lookup"><span data-stu-id="d7d42-113">In the following example, application gateway is serving traffic for contoso.com and fabrikam.com with two back-end server pools: contoso server pool and fabrikam server pool.</span></span> <span data-ttu-id="d7d42-114">Podobně jako instalační program může subdomény hostitele jako app.contoso.com a blog.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="d7d42-114">Similar setup could be used to host subdomains like app.contoso.com and blog.contoso.com.</span></span>

![scénář nasazení ve více lokalitách][multisite]

## <a name="before-you-begin"></a><span data-ttu-id="d7d42-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="d7d42-116">Before you begin</span></span>

<span data-ttu-id="d7d42-117">Tento scénář přidává podporu více lokalit existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="d7d42-117">This scenario adds multi-site support to an existing application gateway.</span></span> <span data-ttu-id="d7d42-118">Abyste mohli dokončit tento scénář, musí být k dispozici ke konfiguraci existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="d7d42-118">To complete this scenario, an existing application gateway needs to be available to configure.</span></span> <span data-ttu-id="d7d42-119">Navštivte [vytvoření služby application gateway pomocí portálu](application-gateway-create-gateway-portal.md) se dozvíte, jak vytvořit základní aplikační brána v portálu.</span><span class="sxs-lookup"><span data-stu-id="d7d42-119">Visit [Create an application gateway by using the portal](application-gateway-create-gateway-portal.md) to learn how to create a basic application gateway in the portal.</span></span>

<span data-ttu-id="d7d42-120">Tady jsou kroky potřebné k aktualizaci služby application gateway:</span><span class="sxs-lookup"><span data-stu-id="d7d42-120">The following are the steps needed to update the application gateway:</span></span>

1. <span data-ttu-id="d7d42-121">Vytvoření fondů back-end, které chcete použít pro každou lokalitu.</span><span class="sxs-lookup"><span data-stu-id="d7d42-121">Create back-end pools to use for each site.</span></span>
2. <span data-ttu-id="d7d42-122">Vytvořte naslouchací proces pro každou lokalitu Aplikační brána podporuje.</span><span class="sxs-lookup"><span data-stu-id="d7d42-122">Create a listener for each site application gateway supports.</span></span>
3. <span data-ttu-id="d7d42-123">Vytvoření pravidel pro mapování jednotlivých naslouchací proces s příslušnou back-end.</span><span class="sxs-lookup"><span data-stu-id="d7d42-123">Create rules to map each listener with the appropriate back-end.</span></span>

## <a name="requirements"></a><span data-ttu-id="d7d42-124">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d7d42-124">Requirements</span></span>

* <span data-ttu-id="d7d42-125">**Fond back-end serverů:** Seznam IP adres back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="d7d42-125">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="d7d42-126">Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami.</span><span class="sxs-lookup"><span data-stu-id="d7d42-126">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span> <span data-ttu-id="d7d42-127">Můžete také použít plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="d7d42-127">FQDN can also be used.</span></span>
* <span data-ttu-id="d7d42-128">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="d7d42-128">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="d7d42-129">Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.</span><span class="sxs-lookup"><span data-stu-id="d7d42-129">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="d7d42-130">**Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="d7d42-130">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="d7d42-131">Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.</span><span class="sxs-lookup"><span data-stu-id="d7d42-131">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="d7d42-132">**Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, u těchto hodnot se rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="d7d42-132">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span> <span data-ttu-id="d7d42-133">Pro aplikace s povolenou více servery brány název hostitele a indikátory SNI jsou také přidat.</span><span class="sxs-lookup"><span data-stu-id="d7d42-133">For multi-site enabled application gateways, host name and SNI indicators are also added.</span></span>
* <span data-ttu-id="d7d42-134">**Pravidlo:** pravidlo váže naslouchací proces fondu back-end serverů a definuje, kterému fondu back-end serverů provoz směrovat při volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="d7d42-134">**Rule:** The rule binds the listener, the back-end server pool, and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="d7d42-135">Pravidla se zpracovávají v pořadí, ve kterém jsou uvedeny a provoz se přesměruje přes první pravidlo, které odpovídá bez ohledu na specifické podobě.</span><span class="sxs-lookup"><span data-stu-id="d7d42-135">Rules are processed in the order they are listed, and traffic will be directed via the first rule that matches regardless of specificity.</span></span> <span data-ttu-id="d7d42-136">Například, pokud máte pravidlo pomocí základní naslouchací proces a pravidla pomocí více lokalit naslouchací proces obou na stejném portu, musí být uvedené pravidlo s několika lokalitami naslouchací proces před pravidlo základní naslouchací proces, aby pravidlo více lokalit a fungovat podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="d7d42-136">For example, if you have a rule using a basic listener and a rule using a multi-site listener both on the same port, the rule with the multi-site listener must be listed before the rule with the basic listener in order for the multi-site rule to function as expected.</span></span> 
* <span data-ttu-id="d7d42-137">**Certifikáty:** každý naslouchací proces vyžaduje jedinečný certifikát, v tomto příkladu jsou vytvořeny 2 naslouchací procesy pro více lokalit.</span><span class="sxs-lookup"><span data-stu-id="d7d42-137">**Certificates:** Each listener requires a unique certificate, in this example 2 listeners are created for multi-site.</span></span> <span data-ttu-id="d7d42-138">Dva certifikáty PFX a hesla je potřeba vytvořit.</span><span class="sxs-lookup"><span data-stu-id="d7d42-138">Two .pfx certificates and the passwords for them need to be created.</span></span>

## <a name="create-back-end-pools-for-each-site"></a><span data-ttu-id="d7d42-139">Vytvoření fondu back-end pro každou lokalitu</span><span class="sxs-lookup"><span data-stu-id="d7d42-139">Create back-end pools for each site</span></span>

<span data-ttu-id="d7d42-140">Back-end fondu pro každou lokalitu, že je potřeba brána podporuje aplikace, v takovém případě 2 se vytvářejí, jeden pro contoso11.com a jeden pro fabrikam11.com.</span><span class="sxs-lookup"><span data-stu-id="d7d42-140">A back-end pool for each site that application gateway supports is needed, in this case 2 are be created, one for contoso11.com and one for fabrikam11.com.</span></span>

### <a name="step-1"></a><span data-ttu-id="d7d42-141">Krok 1</span><span class="sxs-lookup"><span data-stu-id="d7d42-141">Step 1</span></span>

<span data-ttu-id="d7d42-142">Přejděte do existující aplikační brány na portálu Azure (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d7d42-142">Navigate to an existing application gateway in the Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="d7d42-143">Vyberte **back-endové fondy** a klikněte na tlačítko **přidat**</span><span class="sxs-lookup"><span data-stu-id="d7d42-143">Select **Backend pools** and click **Add**</span></span>

![Přidat back-endové fondy][7]

### <a name="step-2"></a><span data-ttu-id="d7d42-145">Krok 2</span><span class="sxs-lookup"><span data-stu-id="d7d42-145">Step 2</span></span>

<span data-ttu-id="d7d42-146">Zadejte informace pro fond back-end **pool1**, přidávání ip adresy nebo plně kvalifikované názvy domén pro back-end serverů a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="d7d42-146">Fill in the information for the back-end pool **pool1**, adding the ip addresses or FQDNs for the back-end servers and click **OK**</span></span>

![nastavení pool1 fondu back-end][8]

### <a name="step-3"></a><span data-ttu-id="d7d42-148">Krok 3</span><span class="sxs-lookup"><span data-stu-id="d7d42-148">Step 3</span></span>

<span data-ttu-id="d7d42-149">V okně back-endové fondy klikněte na **přidat** přidat další back-end fondu **pool2**, přidávání ip adresy nebo plně kvalifikované názvy DOMÉN pro back-end serverů a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="d7d42-149">On the backend-pools blade click **Add** to add an additional back-end pool **pool2**, adding the ip addresses or FQDNS for the back-end servers and click **OK**</span></span>

![nastavení pool2 fondu back-end][9]

## <a name="create-listeners-for-each-back-end"></a><span data-ttu-id="d7d42-151">Vytvořte naslouchací procesy pro každý back-end</span><span class="sxs-lookup"><span data-stu-id="d7d42-151">Create listeners for each back-end</span></span>

<span data-ttu-id="d7d42-152">Služba Application Gateway se při hostování více než jednoho webu na stejné veřejné IP adrese a portu spoléhá na hlavičky hostitele HTTP 1.1.</span><span class="sxs-lookup"><span data-stu-id="d7d42-152">Application Gateway relies on HTTP 1.1 host headers to host more than one website on the same public IP address and port.</span></span> <span data-ttu-id="d7d42-153">Základní naslouchací proces vytvořený na portálu tuto vlastnost neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="d7d42-153">The basic listener created in the portal does not contain this property.</span></span>

### <a name="step-1"></a><span data-ttu-id="d7d42-154">Krok 1</span><span class="sxs-lookup"><span data-stu-id="d7d42-154">Step 1</span></span>

<span data-ttu-id="d7d42-155">Klikněte na tlačítko **naslouchací procesy** na existující aplikační brány a klikněte na **Multi-Site** přidat první naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="d7d42-155">Click **Listeners** on the existing application gateway and click **Multi-site** to add the first listener.</span></span>

![okno Přehled – moduly naslouchání][1]

### <a name="step-2"></a><span data-ttu-id="d7d42-157">Krok 2</span><span class="sxs-lookup"><span data-stu-id="d7d42-157">Step 2</span></span>

<span data-ttu-id="d7d42-158">Zadejte informace pro naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="d7d42-158">Fill out the information for the listener.</span></span> <span data-ttu-id="d7d42-159">V tomto příkladu SSL je nakonfigurované ukončení, vytvořte nový port front-endu.</span><span class="sxs-lookup"><span data-stu-id="d7d42-159">In this example SSL termination is configured, create a new frontend port.</span></span> <span data-ttu-id="d7d42-160">Nahrajte certifikát .pfx, který chcete použít pro ukončení protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="d7d42-160">Upload the .pfx certificate to be used for SSL termination.</span></span> <span data-ttu-id="d7d42-161">Jediným rozdílem v tomto okně ve srovnání s okně Standardní základní naslouchací proces je název hostitele.</span><span class="sxs-lookup"><span data-stu-id="d7d42-161">The only difference on this blade compared to the standard basic listener blade is the hostname.</span></span>

![naslouchací proces vlastnosti okna][2]

### <a name="step-3"></a><span data-ttu-id="d7d42-163">Krok 3</span><span class="sxs-lookup"><span data-stu-id="d7d42-163">Step 3</span></span>

<span data-ttu-id="d7d42-164">Klikněte na tlačítko **Multi-Site** a vytvořit naslouchací proces jiný, jak je popsáno v předchozím kroku pro druhou lokalitu.</span><span class="sxs-lookup"><span data-stu-id="d7d42-164">Click **Multi-site** and create another listener as described in the previous step for the second site.</span></span> <span data-ttu-id="d7d42-165">Nezapomeňte použít jiný certifikát pro druhý naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="d7d42-165">Make sure to use a different certificate for the second listener.</span></span> <span data-ttu-id="d7d42-166">Jediným rozdílem v tomto okně ve srovnání s okně Standardní základní naslouchací proces je název hostitele.</span><span class="sxs-lookup"><span data-stu-id="d7d42-166">The only difference on this blade compared to the standard basic listener blade is the hostname.</span></span> <span data-ttu-id="d7d42-167">Zadejte informace pro naslouchací proces a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d7d42-167">Fill out the information for the listener and click **OK**.</span></span>

![naslouchací proces vlastnosti okna][3]

> [!NOTE]
> <span data-ttu-id="d7d42-169">Vytvoření naslouchacího procesu na portálu Azure pro službu application gateway je dlouhotrvající úlohy, může trvat nějakou dobu vytvořit dva naslouchací procesy v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="d7d42-169">Creation of listeners in the Azure portal for application gateway is a long running task, it may take some time to create the two listeners in this scenario.</span></span> <span data-ttu-id="d7d42-170">Po dokončení naslouchací procesy zobrazit na portálu, jak je vidět na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="d7d42-170">When complete the listeners show in the portal as seen in the following image:</span></span>

![Přehled naslouchací proces][4]

## <a name="create-rules-to-map-listeners-to-backend-pools"></a><span data-ttu-id="d7d42-172">Vytvoření pravidel pro mapování moduly pro naslouchání na back-endové fondy</span><span class="sxs-lookup"><span data-stu-id="d7d42-172">Create rules to map listeners to backend pools</span></span>

### <a name="step-1"></a><span data-ttu-id="d7d42-173">Krok 1</span><span class="sxs-lookup"><span data-stu-id="d7d42-173">Step 1</span></span>

<span data-ttu-id="d7d42-174">Přejděte do existující aplikační brány na portálu Azure (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d7d42-174">Navigate to an existing application gateway in the Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="d7d42-175">Vyberte **pravidla** a zvolte existující výchozí pravidlo **rule1 New** a klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="d7d42-175">Select **Rules** and choose the existing default rule **rule1** and click **Edit**.</span></span>

### <a name="step-2"></a><span data-ttu-id="d7d42-176">Krok 2</span><span class="sxs-lookup"><span data-stu-id="d7d42-176">Step 2</span></span>

<span data-ttu-id="d7d42-177">Vyplňte v okně pravidla, jak je vidět na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="d7d42-177">Fill out the rules blade as seen in the following image.</span></span> <span data-ttu-id="d7d42-178">Výběr prvního naslouchací proces a první fondu a kliknutím na **Uložit** při dokončení.</span><span class="sxs-lookup"><span data-stu-id="d7d42-178">Choosing the first listener and first pool and clicking **Save** when complete.</span></span>

![upravit existující pravidlo][6]

### <a name="step-3"></a><span data-ttu-id="d7d42-180">Krok 3</span><span class="sxs-lookup"><span data-stu-id="d7d42-180">Step 3</span></span>

<span data-ttu-id="d7d42-181">Klikněte na tlačítko **základní pravidlo** vytvoření druhého pravidla.</span><span class="sxs-lookup"><span data-stu-id="d7d42-181">Click **Basic rule** to create the second rule.</span></span> <span data-ttu-id="d7d42-182">Vyplňte formulář s druhý naslouchací proces a sekundu back-end fondu a klikněte na tlačítko **OK** uložit.</span><span class="sxs-lookup"><span data-stu-id="d7d42-182">Fill out the form with the second listener and second backend pool and click **OK** to save.</span></span>

![přidat základní pravidlo okno][10]

<span data-ttu-id="d7d42-184">Tento scénář se dokončí konfigurace aplikační brány s podporou více lokalit prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d7d42-184">This scenario completes configuring an existing application gateway with multi-site support through the Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7d42-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d7d42-185">Next steps</span></span>

<span data-ttu-id="d7d42-186">Zjistěte, jak chránit své weby s [Application Gateway - brány Firewall webových aplikací](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="d7d42-186">Learn how to protect your websites with [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)</span></span>

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png
