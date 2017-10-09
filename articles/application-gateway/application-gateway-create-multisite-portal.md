---
title: "aaaHost víc lokalit s Azure Application Gateway | Microsoft Docs"
description: "Tato stránka poskytuje pokyny tooconfigure existující bránu aplikací Azure pro hostování několika webových aplikací na hello stejnou bránu s hello portálu Azure."
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
ms.openlocfilehash: 2172aa2c80720f6f1ab7dd91745b44654bcaee00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a><span data-ttu-id="b976d-103">Konfigurace existující aplikační brány pro hostování několika webových aplikací</span><span class="sxs-lookup"><span data-stu-id="b976d-103">Configure an existing application gateway for hosting multiple web applications</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b976d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b976d-104">Azure portal</span></span>](application-gateway-create-multisite-portal.md)
> * [<span data-ttu-id="b976d-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="b976d-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-multisite-azureresourcemanager-powershell.md)
> 
> 

<span data-ttu-id="b976d-106">Hostování více lokality můžete toodeploy více než jednu webovou aplikaci na hello stejné aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b976d-106">Multiple site hosting allows you toodeploy more than one web application on hello same application gateway.</span></span> <span data-ttu-id="b976d-107">Přitom spoléhá na přítomnost Hlavička hostitele v hello příchozí požadavek HTTP, který naslouchací proces by přijímat přenosy toodetermine.</span><span class="sxs-lookup"><span data-stu-id="b976d-107">It relies on presence of host header in hello incoming HTTP request, toodetermine which listener would receive traffic.</span></span> <span data-ttu-id="b976d-108">naslouchací proces Hello potom směrovat přenosy tooappropriate back-end fondu podle konfigurace v definici pravidla hello hello brány.</span><span class="sxs-lookup"><span data-stu-id="b976d-108">hello listener then directs traffic tooappropriate backend pool as configured in hello rules definition of hello gateway.</span></span> <span data-ttu-id="b976d-109">V protokolu SSL povoleno webových aplikací Aplikační brána spoléhá na hello indikace názvu serveru (SNI) rozšíření toochoose hello správné naslouchací proces pro hello webový provoz.</span><span class="sxs-lookup"><span data-stu-id="b976d-109">In SSL enabled web applications, application gateway relies on hello Server Name Indication (SNI) extension toochoose hello correct listener for hello web traffic.</span></span> <span data-ttu-id="b976d-110">Běžně používá pro více hostování lokality je tooload vyrovnávat požadavky pro fondy jiných webových domén toodifferent back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="b976d-110">A common use for multiple site hosting is tooload balance requests for different web domains toodifferent back-end server pools.</span></span> <span data-ttu-id="b976d-111">Podobně jako více subdomény hello stejné kořenové domény může být také hostovaná v hello stejné aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b976d-111">Similarly multiple subdomains of hello same root domain could also be hosted on hello same application gateway.</span></span>

## <a name="scenario"></a><span data-ttu-id="b976d-112">Scénář</span><span class="sxs-lookup"><span data-stu-id="b976d-112">Scenario</span></span>

<span data-ttu-id="b976d-113">V následujícím příkladu hello, aplikační brána obsluhuje přenosy dat pro contoso.com a fabrikam.com s dvěma fondy back-end serverů: contoso fondu serverů a fond serverů fabrikam.</span><span class="sxs-lookup"><span data-stu-id="b976d-113">In hello following example, application gateway is serving traffic for contoso.com and fabrikam.com with two back-end server pools: contoso server pool and fabrikam server pool.</span></span> <span data-ttu-id="b976d-114">Podobně jako instalační program může být subdomény používané toohost jako app.contoso.com a blog.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="b976d-114">Similar setup could be used toohost subdomains like app.contoso.com and blog.contoso.com.</span></span>

![scénář nasazení ve více lokalitách][multisite]

## <a name="before-you-begin"></a><span data-ttu-id="b976d-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="b976d-116">Before you begin</span></span>

<span data-ttu-id="b976d-117">Tento scénář přidá podporu více lokalit tooan existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b976d-117">This scenario adds multi-site support tooan existing application gateway.</span></span> <span data-ttu-id="b976d-118">toocomplete tento scénář existující bránu aplikace potřebuje k dispozici tooconfigure toobe.</span><span class="sxs-lookup"><span data-stu-id="b976d-118">toocomplete this scenario, an existing application gateway needs toobe available tooconfigure.</span></span> <span data-ttu-id="b976d-119">Navštivte [vytvoření služby application gateway pomocí portálu hello](application-gateway-create-gateway-portal.md) toolearn jak toocreate základní aplikační brána hello portálu.</span><span class="sxs-lookup"><span data-stu-id="b976d-119">Visit [Create an application gateway by using hello portal](application-gateway-create-gateway-portal.md) toolearn how toocreate a basic application gateway in hello portal.</span></span>

<span data-ttu-id="b976d-120">Následující Hello se, že kroky hello tooupdate hello aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="b976d-120">hello following are hello steps needed tooupdate hello application gateway:</span></span>

1. <span data-ttu-id="b976d-121">Vytvořte toouse back endové fondy pro každou lokalitu.</span><span class="sxs-lookup"><span data-stu-id="b976d-121">Create back-end pools toouse for each site.</span></span>
2. <span data-ttu-id="b976d-122">Vytvořte naslouchací proces pro každou lokalitu Aplikační brána podporuje.</span><span class="sxs-lookup"><span data-stu-id="b976d-122">Create a listener for each site application gateway supports.</span></span>
3. <span data-ttu-id="b976d-123">Vytvoření pravidla toomap každý naslouchací proces s hello odpovídající back-end.</span><span class="sxs-lookup"><span data-stu-id="b976d-123">Create rules toomap each listener with hello appropriate back-end.</span></span>

## <a name="requirements"></a><span data-ttu-id="b976d-124">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b976d-124">Requirements</span></span>

* <span data-ttu-id="b976d-125">**Fond back-end serverů:** hello seznam IP adres hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="b976d-125">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="b976d-126">uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě nebo by měla být veřejné IP Adrese nebo VIP.</span><span class="sxs-lookup"><span data-stu-id="b976d-126">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span> <span data-ttu-id="b976d-127">Můžete také použít plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="b976d-127">FQDN can also be used.</span></span>
* <span data-ttu-id="b976d-128">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="b976d-128">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="b976d-129">Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.</span><span class="sxs-lookup"><span data-stu-id="b976d-129">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="b976d-130">**Front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b976d-130">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="b976d-131">Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="b976d-131">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="b976d-132">**Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="b976d-132">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span> <span data-ttu-id="b976d-133">Pro aplikace s povolenou více servery brány název hostitele a indikátory SNI jsou také přidat.</span><span class="sxs-lookup"><span data-stu-id="b976d-133">For multi-site enabled application gateways, host name and SNI indicators are also added.</span></span>
* <span data-ttu-id="b976d-134">**Pravidlo:** hello pravidlo váže naslouchací proces hello, hello fondu back-end serverů a definuje, jaký provoz hello fond back-end serverů by měla být směrovanou toowhen volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="b976d-134">**Rule:** hello rule binds hello listener, hello back-end server pool, and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="b976d-135">Pravidla se zpracovávají v pořadí hello, které jsou uvedeny a provoz se přesměruje prostřednictvím hello první pravidlo, které odpovídá bez ohledu na specifické podobě.</span><span class="sxs-lookup"><span data-stu-id="b976d-135">Rules are processed in hello order they are listed, and traffic will be directed via hello first rule that matches regardless of specificity.</span></span> <span data-ttu-id="b976d-136">Například pokud máte pravidlo pomocí základní naslouchací proces a pravidla pomocí více lokalit naslouchací proces i na stejný port, hello pravidlo s hello naslouchací proces hello více lokalit musí být uvedený před hello pravidlo základní naslouchací proces hello-li pravidlo více lokalit toofunction hello jako byl očekáván.</span><span class="sxs-lookup"><span data-stu-id="b976d-136">For example, if you have a rule using a basic listener and a rule using a multi-site listener both on hello same port, hello rule with hello multi-site listener must be listed before hello rule with hello basic listener in order for hello multi-site rule toofunction as expected.</span></span> 
* <span data-ttu-id="b976d-137">**Certifikáty:** každý naslouchací proces vyžaduje jedinečný certifikát, v tomto příkladu jsou vytvořeny 2 naslouchací procesy pro více lokalit.</span><span class="sxs-lookup"><span data-stu-id="b976d-137">**Certificates:** Each listener requires a unique certificate, in this example 2 listeners are created for multi-site.</span></span> <span data-ttu-id="b976d-138">Dva certifikáty PFX a hesla hello u nich třeba toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b976d-138">Two .pfx certificates and hello passwords for them need toobe created.</span></span>

## <a name="create-back-end-pools-for-each-site"></a><span data-ttu-id="b976d-139">Vytvoření fondu back-end pro každou lokalitu</span><span class="sxs-lookup"><span data-stu-id="b976d-139">Create back-end pools for each site</span></span>

<span data-ttu-id="b976d-140">Back-end fondu pro každou lokalitu, že je potřeba brána podporuje aplikace, v takovém případě 2 se vytvářejí, jeden pro contoso11.com a jeden pro fabrikam11.com.</span><span class="sxs-lookup"><span data-stu-id="b976d-140">A back-end pool for each site that application gateway supports is needed, in this case 2 are be created, one for contoso11.com and one for fabrikam11.com.</span></span>

### <a name="step-1"></a><span data-ttu-id="b976d-141">Krok 1</span><span class="sxs-lookup"><span data-stu-id="b976d-141">Step 1</span></span>

<span data-ttu-id="b976d-142">Přejděte tooan existující aplikační brána v hello portálu Azure (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b976d-142">Navigate tooan existing application gateway in hello Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="b976d-143">Vyberte **back-endové fondy** a klikněte na tlačítko **přidat**</span><span class="sxs-lookup"><span data-stu-id="b976d-143">Select **Backend pools** and click **Add**</span></span>

![Přidat back-endové fondy][7]

### <a name="step-2"></a><span data-ttu-id="b976d-145">Krok 2</span><span class="sxs-lookup"><span data-stu-id="b976d-145">Step 2</span></span>

<span data-ttu-id="b976d-146">Vyplnit hello informace pro fond back-end hello **pool1**, přidání hello ip adresy nebo plně kvalifikované názvy domén pro hello back-end serverů a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="b976d-146">Fill in hello information for hello back-end pool **pool1**, adding hello ip addresses or FQDNs for hello back-end servers and click **OK**</span></span>

![nastavení pool1 fondu back-end][8]

### <a name="step-3"></a><span data-ttu-id="b976d-148">Krok 3</span><span class="sxs-lookup"><span data-stu-id="b976d-148">Step 3</span></span>

<span data-ttu-id="b976d-149">V okně back-endové fondy hello klikněte na tlačítko **přidat** tooadd další back-end fondu **pool2**, přidání hello ip adresy nebo plně kvalifikované názvy DOMÉN pro hello back-end serverů a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="b976d-149">On hello backend-pools blade click **Add** tooadd an additional back-end pool **pool2**, adding hello ip addresses or FQDNS for hello back-end servers and click **OK**</span></span>

![nastavení pool2 fondu back-end][9]

## <a name="create-listeners-for-each-back-end"></a><span data-ttu-id="b976d-151">Vytvořte naslouchací procesy pro každý back-end</span><span class="sxs-lookup"><span data-stu-id="b976d-151">Create listeners for each back-end</span></span>

<span data-ttu-id="b976d-152">Aplikační brána spoléhá na HTTP 1.1 toohost hlavičky hostitele více než jeden web na hello stejnou veřejnou IP adresu a port.</span><span class="sxs-lookup"><span data-stu-id="b976d-152">Application Gateway relies on HTTP 1.1 host headers toohost more than one website on hello same public IP address and port.</span></span> <span data-ttu-id="b976d-153">Hello základní naslouchací proces vytvoření portálu hello tuto vlastnost neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="b976d-153">hello basic listener created in hello portal does not contain this property.</span></span>

### <a name="step-1"></a><span data-ttu-id="b976d-154">Krok 1</span><span class="sxs-lookup"><span data-stu-id="b976d-154">Step 1</span></span>

<span data-ttu-id="b976d-155">Klikněte na tlačítko **naslouchací procesy** hello existující aplikační brány a klikněte na tlačítko **Multi-Site** první naslouchací proces tooadd hello.</span><span class="sxs-lookup"><span data-stu-id="b976d-155">Click **Listeners** on hello existing application gateway and click **Multi-site** tooadd hello first listener.</span></span>

![okno Přehled – moduly naslouchání][1]

### <a name="step-2"></a><span data-ttu-id="b976d-157">Krok 2</span><span class="sxs-lookup"><span data-stu-id="b976d-157">Step 2</span></span>

<span data-ttu-id="b976d-158">Vyplňte hello informace pro naslouchací proces hello.</span><span class="sxs-lookup"><span data-stu-id="b976d-158">Fill out hello information for hello listener.</span></span> <span data-ttu-id="b976d-159">V tomto příkladu SSL je nakonfigurované ukončení, vytvořte nový port front-endu.</span><span class="sxs-lookup"><span data-stu-id="b976d-159">In this example SSL termination is configured, create a new frontend port.</span></span> <span data-ttu-id="b976d-160">Nahrajte toobe certifikátu .pfx hello používá pro ukončení protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="b976d-160">Upload hello .pfx certificate toobe used for SSL termination.</span></span> <span data-ttu-id="b976d-161">jediným rozdílem Hello v tomto okně okno porovnání toohello standardní základní naslouchací proces je název hostitele hello.</span><span class="sxs-lookup"><span data-stu-id="b976d-161">hello only difference on this blade compared toohello standard basic listener blade is hello hostname.</span></span>

![naslouchací proces vlastnosti okna][2]

### <a name="step-3"></a><span data-ttu-id="b976d-163">Krok 3</span><span class="sxs-lookup"><span data-stu-id="b976d-163">Step 3</span></span>

<span data-ttu-id="b976d-164">Klikněte na tlačítko **Multi-Site** a vytvořit naslouchací proces jiný, jak je popsáno v předchozím kroku hello pro druhou lokalitu hello.</span><span class="sxs-lookup"><span data-stu-id="b976d-164">Click **Multi-site** and create another listener as described in hello previous step for hello second site.</span></span> <span data-ttu-id="b976d-165">Ujistěte se, že toouse jiný certifikát pro druhý naslouchací proces hello.</span><span class="sxs-lookup"><span data-stu-id="b976d-165">Make sure toouse a different certificate for hello second listener.</span></span> <span data-ttu-id="b976d-166">jediným rozdílem Hello v tomto okně okno porovnání toohello standardní základní naslouchací proces je název hostitele hello.</span><span class="sxs-lookup"><span data-stu-id="b976d-166">hello only difference on this blade compared toohello standard basic listener blade is hello hostname.</span></span> <span data-ttu-id="b976d-167">Vyplnění hello informací pro naslouchací proces hello a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b976d-167">Fill out hello information for hello listener and click **OK**.</span></span>

![naslouchací proces vlastnosti okna][3]

> [!NOTE]
> <span data-ttu-id="b976d-169">Vytvoření naslouchacího procesu v hello portál Azure pro službu application gateway je dlouhotrvající úlohy, může trvat některé hello toocreate čas dva naslouchací procesy v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="b976d-169">Creation of listeners in hello Azure portal for application gateway is a long running task, it may take some time toocreate hello two listeners in this scenario.</span></span> <span data-ttu-id="b976d-170">Při dokončení hello naslouchací procesy zobrazit hello portálu, jak je vidět v hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="b976d-170">When complete hello listeners show in hello portal as seen in hello following image:</span></span>

![Přehled naslouchací proces][4]

## <a name="create-rules-toomap-listeners-toobackend-pools"></a><span data-ttu-id="b976d-172">Vytvoření pravidel fondy toobackend toomap – moduly naslouchání</span><span class="sxs-lookup"><span data-stu-id="b976d-172">Create rules toomap listeners toobackend pools</span></span>

### <a name="step-1"></a><span data-ttu-id="b976d-173">Krok 1</span><span class="sxs-lookup"><span data-stu-id="b976d-173">Step 1</span></span>

<span data-ttu-id="b976d-174">Přejděte tooan existující aplikační brána v hello portálu Azure (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b976d-174">Navigate tooan existing application gateway in hello Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="b976d-175">Vyberte **pravidla** a zvolte existující výchozí pravidlo hello **rule1 New** a klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="b976d-175">Select **Rules** and choose hello existing default rule **rule1** and click **Edit**.</span></span>

### <a name="step-2"></a><span data-ttu-id="b976d-176">Krok 2</span><span class="sxs-lookup"><span data-stu-id="b976d-176">Step 2</span></span>

<span data-ttu-id="b976d-177">Vyplňte okno hello pravidla, jak je vidět v hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="b976d-177">Fill out hello rules blade as seen in hello following image.</span></span> <span data-ttu-id="b976d-178">Výběr hello první naslouchací proces a první fondu a kliknutím na **Uložit** při dokončení.</span><span class="sxs-lookup"><span data-stu-id="b976d-178">Choosing hello first listener and first pool and clicking **Save** when complete.</span></span>

![upravit existující pravidlo][6]

### <a name="step-3"></a><span data-ttu-id="b976d-180">Krok 3</span><span class="sxs-lookup"><span data-stu-id="b976d-180">Step 3</span></span>

<span data-ttu-id="b976d-181">Klikněte na tlačítko **základní pravidlo** toocreate hello druhé pravidlo.</span><span class="sxs-lookup"><span data-stu-id="b976d-181">Click **Basic rule** toocreate hello second rule.</span></span> <span data-ttu-id="b976d-182">Vyplňte formulář hello hello druhý naslouchací proces a sekundu fond back-end a klikněte na tlačítko **OK** toosave.</span><span class="sxs-lookup"><span data-stu-id="b976d-182">Fill out hello form with hello second listener and second backend pool and click **OK** toosave.</span></span>

![přidat základní pravidlo okno][10]

<span data-ttu-id="b976d-184">Tento scénář se dokončí konfigurace aplikační brány s podporou více lokalit prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b976d-184">This scenario completes configuring an existing application gateway with multi-site support through hello Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b976d-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b976d-185">Next steps</span></span>

<span data-ttu-id="b976d-186">Zjistěte, jak tooprotect své weby s [Application Gateway - brány Firewall webových aplikací](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="b976d-186">Learn how tooprotect your websites with [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)</span></span>

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
