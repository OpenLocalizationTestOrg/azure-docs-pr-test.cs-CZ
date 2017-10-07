---
title: "Přehled vytváření připojení aaaAzure IoT Suite | Microsoft Docs"
description: "Popis hello Azure IoT Suite připojen objekt pro vytváření předkonfigurovaného řešení."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: 929b5ed41ef7d82c9b4480d02aa3f0db38931919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-connected-factory-preconfigured-solution"></a><span data-ttu-id="036d3-103">Začínáme s hello připojen objekt pro vytváření předkonfigurovaného řešení</span><span class="sxs-lookup"><span data-stu-id="036d3-103">Get started with hello connected factory preconfigured solution</span></span>

<span data-ttu-id="036d3-104">Azure IoT Suite [předkonfigurovaných řešení] [ lnk-preconfigured-solutions] kombinovat více Azure IoT služby toodeliver začátku do konce řešení, implementující běžné obchodní scénáře IoT.</span><span class="sxs-lookup"><span data-stu-id="036d3-104">Azure IoT Suite [preconfigured solutions][lnk-preconfigured-solutions] combine multiple Azure IoT services toodeliver end-to-end solutions that implement common IoT business scenarios.</span></span> <span data-ttu-id="036d3-105">Hello *připojené factory* předkonfigurované řešení připojí tooand monitorování průmyslových zařízení.</span><span class="sxs-lookup"><span data-stu-id="036d3-105">hello *connected factory* preconfigured solution connects tooand monitors your industrial devices.</span></span> <span data-ttu-id="036d3-106">Můžete použít hello řešení tooanalyze hello datový proud z vašeho zařízení a toodrive provozní produktivitu a ziskovost.</span><span class="sxs-lookup"><span data-stu-id="036d3-106">You can use hello solution tooanalyze hello stream of data from your devices and toodrive operational productivity and profitability.</span></span>

<span data-ttu-id="036d3-107">Tento kurz ukazuje, jak tooprovision hello připojen objekt pro vytváření předkonfigurovaného řešení.</span><span class="sxs-lookup"><span data-stu-id="036d3-107">This tutorial shows you how tooprovision hello connected factory preconfigured solution.</span></span> <span data-ttu-id="036d3-108">Je také vás provede procesem hello základní funkce hello předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="036d3-108">It also walks you through hello basic features of hello preconfigured solution.</span></span> <span data-ttu-id="036d3-109">Mnoho z těchto funkcí můžete přistupovat z řešení hello *řídicí panel* které nasazuje v rámci hello předkonfigurované řešení:</span><span class="sxs-lookup"><span data-stu-id="036d3-109">You can access many of these features from hello solution *dashboard* that deploys as part of hello preconfigured solution:</span></span>

![Řídicí panel předkonfigurovaného řešení propojené továrny][img-cf-home]

<span data-ttu-id="036d3-111">toocomplete tohoto kurzu potřebujete aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="036d3-111">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="036d3-112">Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební.</span><span class="sxs-lookup"><span data-stu-id="036d3-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="036d3-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="036d3-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
> 
> 

## <a name="provision-hello-solution"></a><span data-ttu-id="036d3-114">Zřízení hello řešení</span><span class="sxs-lookup"><span data-stu-id="036d3-114">Provision hello solution</span></span>

1. <span data-ttu-id="036d3-115">Přihlaste se pomocí svých přihlašovacích údajů účtu Azure tooazureiotsuite.com a klikněte na tlačítko "**+**" toocreate řešení.</span><span class="sxs-lookup"><span data-stu-id="036d3-115">Log on tooazureiotsuite.com using your Azure account credentials, and click "**+**" toocreate a solution.</span></span>
2. <span data-ttu-id="036d3-116">Klikněte na tlačítko **vyberte** na hello **připojené factory** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="036d3-116">Click **Select** on hello **Connected factory** tile.</span></span>
3. <span data-ttu-id="036d3-117">Zadejte **Název řešení** pro předkonfigurované řešení připojené továrny.</span><span class="sxs-lookup"><span data-stu-id="036d3-117">Enter a **Solution name** for your connected factory preconfigured solution.</span></span>
4. <span data-ttu-id="036d3-118">Vyberte hello **předplatné** a **oblast** chcete toouse tooprovision hello řešení.</span><span class="sxs-lookup"><span data-stu-id="036d3-118">Select hello **Subscription** and **Region** you want toouse tooprovision hello solution.</span></span>
5. <span data-ttu-id="036d3-119">Klikněte na tlačítko **vytvořit řešení** toobegin hello procesu zřizování.</span><span class="sxs-lookup"><span data-stu-id="036d3-119">Click **Create Solution** toobegin hello provisioning process.</span></span> <span data-ttu-id="036d3-120">Tento proces obvykle trvá několik minut toorun.</span><span class="sxs-lookup"><span data-stu-id="036d3-120">This process typically takes several minutes toorun.</span></span>

### <a name="while-you-wait-for-hello-provisioning-process-toocomplete"></a><span data-ttu-id="036d3-121">A počkat, hello zřizování toocomplete procesu</span><span class="sxs-lookup"><span data-stu-id="036d3-121">While you wait for hello provisioning process toocomplete</span></span>

1. <span data-ttu-id="036d3-122">Klikněte na dlaždici s řešením hello **zřizování** stavu.</span><span class="sxs-lookup"><span data-stu-id="036d3-122">Click hello tile for your solution with **Provisioning** status.</span></span>
2. <span data-ttu-id="036d3-123">Všimněte si hello **stavy zřizování** při nasazování služby Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="036d3-123">Notice hello **Provisioning states** as Azure services are deployed in your Azure subscription.</span></span>
3. <span data-ttu-id="036d3-124">Jakmile bude zřizování dokončeno, hello změny stavu příliš**připraven**.</span><span class="sxs-lookup"><span data-stu-id="036d3-124">Once provisioning completes, hello status changes too**Ready**.</span></span>
4. <span data-ttu-id="036d3-125">Klikněte na tlačítko hello dlaždice toosee hello informace o řešení v pravém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-125">Click hello tile toosee hello details of your solution in hello right-hand pane.</span></span>

> [!NOTE]
> <span data-ttu-id="036d3-126">Pokud narazíte na problémy nasazení hello předkonfigurované řešení, přečtěte si [oprávnění na webu azureiotsuite.com hello] [ lnk-permissions] a hello [připojené nejčastější dotazy týkající se vytváření](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="036d3-126">If you encounter issues deploying hello preconfigured solution, review [Permissions on hello azureiotsuite.com site][lnk-permissions] and hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span> <span data-ttu-id="036d3-127">Pokud hello problémy přetrvávají, vytvořte lístek služby na hello [portál][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="036d3-127">If hello issues persist, create a service ticket on hello [portal][lnk-portal].</span></span>

<span data-ttu-id="036d3-128">Existují by uživatel očekával toosee podrobnosti, které nejsou uvedené pro vaše řešení?</span><span class="sxs-lookup"><span data-stu-id="036d3-128">Are there details you'd expect toosee that aren't listed for your solution?</span></span> <span data-ttu-id="036d3-129">Sdělte nám návrhy na funkce na webu [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span><span class="sxs-lookup"><span data-stu-id="036d3-129">Give us feature suggestions on [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="036d3-130">Přehled scénáře</span><span class="sxs-lookup"><span data-stu-id="036d3-130">Scenario overview</span></span>

<span data-ttu-id="036d3-131">Při nasazování hello připojen objekt pro vytváření předkonfigurovaného řešení, je naplněna prostředky, které umožňují toostep prostřednictvím průmyslových běžný scénář.</span><span class="sxs-lookup"><span data-stu-id="036d3-131">When you deploy hello connected factory preconfigured solution, it is prepopulated with resources that enable you toostep through a common industrial scenario.</span></span> <span data-ttu-id="036d3-132">V tomto scénáři připojené několik objektů pro vytváření sestav řešení toohello hello data hodnoty požadované toocompute celkovou efektivitu zařízení (OEE) a klíčových ukazatelů výkonu (KPI).</span><span class="sxs-lookup"><span data-stu-id="036d3-132">In this scenario, several factories connected toohello solution report hello data values required toocompute overall equipment efficiency (OEE) and key performance indicators (KPIs).</span></span> <span data-ttu-id="036d3-133">Hello následující části ukazují, jak na:</span><span class="sxs-lookup"><span data-stu-id="036d3-133">hello following sections show you how to:</span></span>

* <span data-ttu-id="036d3-134">Monitorovat továrnu, výrobní linky, celkovou efektivitu zařízení stanic a hodnoty klíčových ukazatelů výkonu</span><span class="sxs-lookup"><span data-stu-id="036d3-134">Monitor factory, production lines, station OEE, and KPI values</span></span>
* <span data-ttu-id="036d3-135">Analyzovat telemetrická data hello generované z těchto zařízení pomocí Azure časové řady statistiky</span><span class="sxs-lookup"><span data-stu-id="036d3-135">Analyze hello telemetry data generated from these devices using Azure Time Series Insights</span></span>
* <span data-ttu-id="036d3-136">Fungovat na problémy toofix výstrahy</span><span class="sxs-lookup"><span data-stu-id="036d3-136">Act on alerts toofix issues</span></span>

<span data-ttu-id="036d3-137">Klíčové funkce tento scénář je, že můžete provádět tyto akce vzdáleně z řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-137">A key feature of this scenario is that you can perform all these actions remotely from hello solution dashboard.</span></span> <span data-ttu-id="036d3-138">Není nutné zařízení toohello fyzický přístup.</span><span class="sxs-lookup"><span data-stu-id="036d3-138">You do not need physical access toohello devices.</span></span>

## <a name="view-hello-solution-dashboard"></a><span data-ttu-id="036d3-139">Zobrazení řídicí panel řešení hello</span><span class="sxs-lookup"><span data-stu-id="036d3-139">View hello solution dashboard</span></span>

<span data-ttu-id="036d3-140">řídicí panel řešení Hello umožňuje toomanage hello nasazené řešení.</span><span class="sxs-lookup"><span data-stu-id="036d3-140">hello solution dashboard enables you toomanage hello deployed solution.</span></span> <span data-ttu-id="036d3-141">Je to hierarchická reprezentace globální konfigurace továrny.</span><span class="sxs-lookup"><span data-stu-id="036d3-141">It is a hierarchical representation of a global factory configuration.</span></span> <span data-ttu-id="036d3-142">Můžete například zobrazit celkovou efektivitu zařízení a klíčové ukazatele výkonu nebo publikovat nové uzly pro výstrahy akcí a telemetrie.</span><span class="sxs-lookup"><span data-stu-id="036d3-142">For example, you can view OEE and KPIs, publish new nodes for telemetry and action alerts.</span></span>

1. <span data-ttu-id="036d3-143">Když je hello zřizování dokončeno a dlaždice hello pro předkonfigurované řešení označuje **připraven**, zvolte **spusťte** tooopen portálu připojené vytváření řešení na nové záložce.</span><span class="sxs-lookup"><span data-stu-id="036d3-143">When hello provisioning is complete and hello tile for your preconfigured solution indicates **Ready**, choose **Launch** tooopen your connected factory solution portal in a new tab.</span></span>

    ![Spusťte hello předkonfigurované řešení][img-launch-solution]

1. <span data-ttu-id="036d3-145">Ve výchozím nastavení, hello portálu řešení zobrazuje hello *řídicí panel*.</span><span class="sxs-lookup"><span data-stu-id="036d3-145">By default, hello solution portal shows hello *dashboard*.</span></span> <span data-ttu-id="036d3-146">oblasti tooother toonavigate hello portál, pomocí hello nabídky na levé straně stránky hello hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-146">toonavigate tooother areas of hello portal, use hello menu on hello left-hand side of hello page.</span></span>

    ![Řídicí panel předkonfigurovaného řešení propojené továrny][cf-img-menu]

<span data-ttu-id="036d3-148">řídicí panel Hello zobrazuje hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="036d3-148">hello dashboard displays hello following information:</span></span>

* <span data-ttu-id="036d3-149">A **objekt pro vytváření seznamu** panel, který ukazuje hello stav, umístění a aktuální konfiguraci produkční v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-149">A **Factory list** panel that shows hello status, location, and current production configuration in hello solution.</span></span> <span data-ttu-id="036d3-150">Při prvním spuštění hello řešení, existuje několik simulovaných zařízení.</span><span class="sxs-lookup"><span data-stu-id="036d3-150">When you first run hello solution, there are a number of simulated devices.</span></span> <span data-ttu-id="036d3-151">Hello výrobní simulace se skládá ze tří skutečné OPC UA servery na každý řádek produkční simulované úkoly, které sdílet data.</span><span class="sxs-lookup"><span data-stu-id="036d3-151">hello production line simulation is composed of three real OPC UA servers per production line that perform simulated tasks and share data.</span></span> <span data-ttu-id="036d3-152">Další informace o OPC UA najdete v tématu hello [připojené nejčastější dotazy týkající se vytváření](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="036d3-152">For more information about OPC UA, see hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>
* <span data-ttu-id="036d3-153">A **mapy** , zobrazí hello umístění každého zařízení připojené toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="036d3-153">A **map** that displays hello location of each device connected toohello solution.</span></span> <span data-ttu-id="036d3-154">řešení Hello hello rozhraní API map Bing tooplot informace můžete použít na mapě hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-154">hello solution can use hello Bing Maps API tooplot information on hello map.</span></span> <span data-ttu-id="036d3-155">Pokud je ve vašem předplatném povolené podnikové rozhraní API pro Mapy Bing, tato funkce se použije automaticky.</span><span class="sxs-lookup"><span data-stu-id="036d3-155">If your subscription is enabled for Bing Maps Enterprise API, then this feature is used automatically.</span></span> <span data-ttu-id="036d3-156">Pokud ne, najdete v části hello [– nejčastější dotazy] [ lnk-faq] toolearn jak toomake hello dynamické mapy.</span><span class="sxs-lookup"><span data-stu-id="036d3-156">If not, see hello [FAQ][lnk-faq] toolearn how toomake hello map dynamic.</span></span>
* <span data-ttu-id="036d3-157">Panel **Výstrahy**, na kterém se zobrazují výstrahy generované při překročení konkrétních mezních hodnot pro telemetrii nebo hodnoty celkové efektivity zařízení nebo klíčových ukazatelů výkonu.</span><span class="sxs-lookup"><span data-stu-id="036d3-157">An **Alerts** panel that displays alerts generated when a telemetry or OEE/KPI value exceeds a specific threshold.</span></span>
* <span data-ttu-id="036d3-158">**Celkovou efektivitu vybavení** panel, který ukazuje hello OEE hodnot pro celý podnik hello nebo hello výroby řádku/stanici můžete prohlížíte.</span><span class="sxs-lookup"><span data-stu-id="036d3-158">An **Overall Equipment Efficiency** panel that shows hello OEE values for hello whole enterprise, or hello factory/production line/station you are viewing.</span></span> <span data-ttu-id="036d3-159">Tato hodnota se shromažďují z hello stanice zobrazení toohello podnikové úrovně.</span><span class="sxs-lookup"><span data-stu-id="036d3-159">This value is aggregated from hello station view toohello enterprise level.</span></span> <span data-ttu-id="036d3-160">Hello OEE obrázku a jeho základní prvky mohou být další analyzovány.</span><span class="sxs-lookup"><span data-stu-id="036d3-160">hello OEE figure and its constituent elements can be further analyzed.</span></span>
* <span data-ttu-id="036d3-161">**Klíčové ukazatele výkonu** panely, které zobrazí počet hello jednotky, které vytváří a energie používané hello celý podnik nebo hello objekt pro vytváření nebo produkční řádku stanici můžete prohlížíte.</span><span class="sxs-lookup"><span data-stu-id="036d3-161">**Key Performance Indicators** panel that displays hello number of units produced and energy used by hello whole enterprise or hello factory/production line/station you are viewing.</span></span> <span data-ttu-id="036d3-162">Tyto hodnoty se shromažďují z úrovně stanice zobrazení toohello enterprise.</span><span class="sxs-lookup"><span data-stu-id="036d3-162">These values are aggregated from a station view toohello enterprise level.</span></span>

## <a name="view-factories"></a><span data-ttu-id="036d3-163">Zobrazení továren</span><span class="sxs-lookup"><span data-stu-id="036d3-163">View factories</span></span>

<span data-ttu-id="036d3-164">Hello *objekty Factory* panelu ukazuje hello zeměpisné umístění všech objektů Factory hello hello řešení, jejich stav a aktuální konfiguraci výroby.</span><span class="sxs-lookup"><span data-stu-id="036d3-164">hello *Factories* panel shows you hello geographical location of all hello factories in hello solution, their status, and current production configuration.</span></span> <span data-ttu-id="036d3-165">Ze seznamu hello umístění můžete přejít toohello úrovně v hierarchii řešení hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-165">From hello locations list, you can navigate toohello other levels in hello solution hierarchy.</span></span> <span data-ttu-id="036d3-166">hypertextové odkazy, které odkazují podrobnosti hello produkční řádků v tomto umístění nejsou Hello řádky v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-166">hello rows in hello list are hyperlinks that link details of hello production lines at that location.</span></span> <span data-ttu-id="036d3-167">Je pak možné toodrill do hello výrobní podrobnosti a dolů toohello stanice úrovně zobrazení.</span><span class="sxs-lookup"><span data-stu-id="036d3-167">It is then possible toodrill into hello production line details and down toohello station level view.</span></span> <span data-ttu-id="036d3-168">Můžete také použít toohello seznam filtrů.</span><span class="sxs-lookup"><span data-stu-id="036d3-168">You can also apply a filter toohello list.</span></span>

![Továrny v předkonfigurovaném řešení propojené továrny][cf-img-factories] 

1. <span data-ttu-id="036d3-170">Hello **objekt pro vytváření panelů** ukazuje hello objekt pro vytváření seznamu pro toto řešení.</span><span class="sxs-lookup"><span data-stu-id="036d3-170">hello **Factory panel** shows hello factory list for this solution.</span></span>

2. <span data-ttu-id="036d3-171">seznam objekt pro vytváření Hello původně obsahuje šest objektů Factory vytvořené hello procesu zřizování.</span><span class="sxs-lookup"><span data-stu-id="036d3-171">hello factory list initially shows six factories created by hello provisioning process.</span></span> <span data-ttu-id="036d3-172">Můžete přidat další Simulovaná a fyzické zařízení toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="036d3-172">You can add additional simulated and physical devices toohello solution.</span></span>

3. <span data-ttu-id="036d3-173">Podrobnosti o hello tooview objektu pro vytváření, klikněte na tlačítko hello řádek v objektu pro vytváření seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-173">tooview hello details of a factory, click hello row in hello factory list.</span></span>

4. <span data-ttu-id="036d3-174">Podrobnosti hello tooview produkční řádku, klikněte na řádek hello v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-174">tooview hello details of a production line, click hello row in hello list.</span></span>

5. <span data-ttu-id="036d3-175">tooview hello publikovaná OPC UA uzly stanice na řádku výrobní hello, klikněte na řádek hello v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-175">tooview hello published OPC UA nodes of a station on hello production line, click hello row in hello list.</span></span>

6. <span data-ttu-id="036d3-176">Podrobnosti tooview v konkrétním uzlu v hello stanice, klikněte na řádek hello v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-176">tooview details on a specific node in hello station, click hello row in hello list.</span></span> <span data-ttu-id="036d3-177">Tato akce spustí hello kontextu panel s vizualizacemi Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="036d3-177">This action launches hello context panel with Time Series Insights visualizations.</span></span> <span data-ttu-id="036d3-178">Klikněte na tlačítko tyto grafy toodo další analýzu v prostředí Průzkumníka hello Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="036d3-178">Click these graphs toodo further analysis in hello Time Series Insights explorer environment.</span></span>

## <a name="view-map"></a><span data-ttu-id="036d3-179">Zobrazení mapy</span><span class="sxs-lookup"><span data-stu-id="036d3-179">View map</span></span>

<span data-ttu-id="036d3-180">Pokud vaše předplatné má toohello přístup k rozhraní API map Bing, hello *objekty Factory* mapy ukazuje hello zeměpisné umístění a stav všech objektů Factory hello v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-180">If your subscription has access toohello Bing Maps API, hello *Factories* map shows you hello geographical location and status of all hello factories in hello solution.</span></span> <span data-ttu-id="036d3-181">toodrill do hello podrobnosti umístění, klikněte na tlačítko Zobrazit na mapě hello umístění hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-181">toodrill into hello location details, click hello locations displayed on hello map.</span></span>

![Mapa v předkonfigurovaném řešení propojené továrny][cf-img-map]

## <a name="view-alerts"></a><span data-ttu-id="036d3-183">Zobrazení výstrah</span><span class="sxs-lookup"><span data-stu-id="036d3-183">View alerts</span></span>

<span data-ttu-id="036d3-184">Hello **výstrahy** panelu ukazuje výstrahy generované z důvodu tooa hlášené hodnoty nebo počítané hodnoty OEE/klíčového ukazatele výkonu vyšší než její nakonfigurovanou prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="036d3-184">hello **Alert** panel shows you alerts generated due tooa reported value or a calculated OEE/KPI value exceeding its configured threshold.</span></span> <span data-ttu-id="036d3-185">Tento panel zobrazuje výstrahy na každé úrovni hello hierarchii, postupně od hello stanice úrovně zobrazení toohello globální zobrazení.</span><span class="sxs-lookup"><span data-stu-id="036d3-185">This panel displays alerts at each level of hello hierarchy, from hello station level view toohello global view.</span></span> <span data-ttu-id="036d3-186">Hello výstrahy obsahují popis hello výstraha, datum, čas, umístění a počet výskytů.</span><span class="sxs-lookup"><span data-stu-id="036d3-186">hello alerts contain a description of hello alert, date, time, location, and number of occurrences.</span></span> <span data-ttu-id="036d3-187">Můžete získat přehledy toohello data, která způsobila výstrahu hello pomocí hello datům přehledů časové řady.</span><span class="sxs-lookup"><span data-stu-id="036d3-187">You can gain insights in toohello data that caused hello alert using hello Time Series Insights data.</span></span> <span data-ttu-id="036d3-188">Hello čas řady statistiky, které data jsou vizualizována ve hello výstrahy, kde je to možné.</span><span class="sxs-lookup"><span data-stu-id="036d3-188">hello Time Series Insights data is visualized in hello alerts where applicable.</span></span> <span data-ttu-id="036d3-189">Pokud jste správce, můžete provést výchozích akcí na hello výstrahy jako:</span><span class="sxs-lookup"><span data-stu-id="036d3-189">If you are an Administrator, you can take default actions on hello alerts such as:</span></span>

* <span data-ttu-id="036d3-190">Výstraha zavřít hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-190">Close hello alert.</span></span>
* <span data-ttu-id="036d3-191">Potvrdit hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="036d3-191">Acknowledge hello alert.</span></span>

<span data-ttu-id="036d3-192">Volitelně můžete provést složitější akce.</span><span class="sxs-lookup"><span data-stu-id="036d3-192">Optionally, you can take more complex actions.</span></span> <span data-ttu-id="036d3-193">Například pro hello přetížení OPC UA uzlu hello sestavení, které může:</span><span class="sxs-lookup"><span data-stu-id="036d3-193">For example, for hello Pressure OPC UA Node of hello Assembly you could:</span></span>

* <span data-ttu-id="036d3-194">Zobrazit v novém okně prohlížeče webovou stránku s podpůrnými informacemi.</span><span class="sxs-lookup"><span data-stu-id="036d3-194">Show supporting information in a web page in a new browser window.</span></span>
* <span data-ttu-id="036d3-195">Zmírnění hello příčinu výstrahy hello voláním metody OPC UA hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="036d3-195">Mitigate hello cause of hello alert by calling an OPC UA method on hello device.</span></span>
* <span data-ttu-id="036d3-196">Potlačíte hello dostupnost hello výchozích akcí.</span><span class="sxs-lookup"><span data-stu-id="036d3-196">Suppress hello availability of hello default actions.</span></span>

    ![Výstrahy v předkonfigurovaném řešení propojené továrny][cf-img-alerts]

> [!NOTE]
> <span data-ttu-id="036d3-198">Tyto výstrahy jsou generovány podle pravidel, které jsou určené v konfiguračním souboru v hello předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="036d3-198">These alerts are generated by rules that are specified in a configuration file in hello preconfigured solution.</span></span> <span data-ttu-id="036d3-199">Tato pravidla mohou generovat výstrahy, když hello OEE nebo obrázky klíčového ukazatele výkonu nebo OPC UA uzlu hodnoty jsou překročení jejich nakonfigurovanou prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="036d3-199">These rules can generate alerts when hello OEE or KPI figures or OPC UA Node values are exceeding their configured threshold.</span></span>

1. <span data-ttu-id="036d3-200">Hello **výstrahy panely** ukazuje hello výstrahy generované v tomto řešení.</span><span class="sxs-lookup"><span data-stu-id="036d3-200">hello **Alerts panel** shows hello alerts generated in this solution.</span></span>

2. <span data-ttu-id="036d3-201">tooview hello podrobnosti výstrahy, klikněte na výstrahu hello panelu hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="036d3-201">tooview hello details of an alert, click hello alert in hello alerts panel.</span></span>

3. <span data-ttu-id="036d3-202">toofurther analýza dat hello výstrah, klikněte na graf hello v hello výstrahy panely tooopen hello časové řady Statistika explorer prostředí.</span><span class="sxs-lookup"><span data-stu-id="036d3-202">toofurther analyze hello alert data, click hello graph in hello alert panel tooopen hello Time Series Insights explorer environment.</span></span>

4. <span data-ttu-id="036d3-203">tooaddress hello výstrahy, jsou k dispozici výstrah panelu hello několik akce.</span><span class="sxs-lookup"><span data-stu-id="036d3-203">tooaddress hello alert, several actions are available in hello alert panel.</span></span> <span data-ttu-id="036d3-204">Vyberte příslušnou možnost hello pro vás a klikněte na hello provést akce příkazového tlačítka.</span><span class="sxs-lookup"><span data-stu-id="036d3-204">Choose hello appropriate option for you and click hello execute action command button.</span></span>

## <a name="view-overall-equipment-efficiency"></a><span data-ttu-id="036d3-205">Zobrazení celkové efektivity zařízení</span><span class="sxs-lookup"><span data-stu-id="036d3-205">View overall equipment efficiency</span></span>

<span data-ttu-id="036d3-206">Sazby OEE hello efektivitu hello výrobní proces pomocí klíče související s provozním provozních parametrů.</span><span class="sxs-lookup"><span data-stu-id="036d3-206">OEE rates hello efficiency of hello manufacturing process using a key production-related operational parameters.</span></span> <span data-ttu-id="036d3-207">OEE je oborový standard měr vypočtena vynásobením hello míra dostupnosti, výkonu rychlost a míra kvality: OEE = dostupnosti x výkonu x kvality.</span><span class="sxs-lookup"><span data-stu-id="036d3-207">OEE is an industry standard measure calculated by multiplying hello availability rate, performance rate, and quality rate: OEE = availability x performance x quality.</span></span>

![Celková efektivita zařízení v předkonfigurovaném řešení propojené továrny][cf-img-oee]

1. <span data-ttu-id="036d3-209">tooview OEE pro všechny úrovně v hierarchii hello přejděte toohello konkrétní zobrazení, které vyžadujete.</span><span class="sxs-lookup"><span data-stu-id="036d3-209">tooview OEE for any level in hello hierarchy, navigate toohello specific view you require.</span></span> <span data-ttu-id="036d3-210">Hello OEE pro toto zobrazení zobrazí panelu hello společně s každou hello elementů, které tvoří hello OEE procento.</span><span class="sxs-lookup"><span data-stu-id="036d3-210">hello OEE for that view displays in hello panel along with each of hello elements that make up hello OEE percentage.</span></span>

2. <span data-ttu-id="036d3-211">toofurther analyzovat hello OEE pro všechny úrovně v hierarchii dat hello, klikněte na hello OEE, dostupnosti, výkonu nebo procento kvality.</span><span class="sxs-lookup"><span data-stu-id="036d3-211">toofurther analyze hello OEE for any level in hello hierarchy data, click either hello OEE, availability, performance, or quality percentage.</span></span> <span data-ttu-id="036d3-212">Kontext panelu se zobrazí se statistickými časové řady napájený vizualizací, které zobrazuje data z hello poslední hodinu, posledních 24 hodin a posledních 7 dnů.</span><span class="sxs-lookup"><span data-stu-id="036d3-212">A context panel appears with Time Series Insights powered visualizations that shows data from hello last hour, last 24 hours, and last 7 days.</span></span>

    ![Vizualizace TSI v předkonfigurovaném řešení propojené továrny][cf-img-tsi-visualization]

3. <span data-ttu-id="036d3-214">toofurther analýza dat hello výstrah, klikněte na graf hello výstrah panelu hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-214">toofurther analyze hello alert data, click hello graph in hello alert panel.</span></span> <span data-ttu-id="036d3-215">Tato akce otevře prostředí Průzkumníka hello Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="036d3-215">This action opens hello Time Series Insights explorer environment.</span></span>

    ![Průzkumník TSI v předkonfigurovaném řešení propojené továrny][cf-img-tsi-explorer]

## <a name="view-key-performance-indicators"></a><span data-ttu-id="036d3-217">Zobrazení klíčových ukazatelů výkonu</span><span class="sxs-lookup"><span data-stu-id="036d3-217">View Key Performance Indicators</span></span>

<span data-ttu-id="036d3-218">Hello řešení poskytuje dvě klíčové ukazatele výkonu, *jednotky za hodinu* a *energie v kWh*.</span><span class="sxs-lookup"><span data-stu-id="036d3-218">hello solution provides two key performance indicators, *units per hour* and *energy used in kWh*.</span></span>

![Klíčové ukazatele výkonu v předkonfigurovaném řešení propojené továrny][cf-img-kpi]

1. <span data-ttu-id="036d3-220">jednotky tooview za hodinu nebo energie použít pro všechny úrovně v hierarchii hello přejděte toohello konkrétní zobrazení, které vyžadujete.</span><span class="sxs-lookup"><span data-stu-id="036d3-220">tooview units per hour or energy used for any level in hello hierarchy, navigate toohello specific view you require.</span></span> <span data-ttu-id="036d3-221">jednotky Hello za hodinu a energie použít zobrazení panelu hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-221">hello units per hour and energy used display in hello panel.</span></span>

2. <span data-ttu-id="036d3-222">jednotky tooanalyze za hodinu nebo energie použít pro všechny úrovně v hierarchii hello další, klikněte na tlačítko hello měřidla v hello **klíčové ukazatele výkonu** panelu.</span><span class="sxs-lookup"><span data-stu-id="036d3-222">tooanalyze units per hour or energy used for any level in hello hierarchy further, click hello gauge in hello **Key Performance Indicators** panel.</span></span> <span data-ttu-id="036d3-223">Kontext panelu se zobrazí s vizualizacemi čas řady Insights používá technologii umožňuje tooview data z hello poslední hodinu hello posledních 24 hodin a posledních 7 dnů.</span><span class="sxs-lookup"><span data-stu-id="036d3-223">A context panel appears with Time Series Insights powered visualizations enabling you tooview data from hello last hour, hello last 24 hours, and last 7 days.</span></span>

## <a name="scenario-review"></a><span data-ttu-id="036d3-224">Revize scénáře</span><span class="sxs-lookup"><span data-stu-id="036d3-224">Scenario review</span></span>

<span data-ttu-id="036d3-225">V tomto scénáři můžete sledovat svoje objekty Factory OEE a klíčových ukazatelů výkonu hodnoty, na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-225">In this scenario, you monitored your factories OEE and KPIs values, in hello dashboard.</span></span> <span data-ttu-id="036d3-226">Můžete pak použít časové řady Insights tooprovide Další informace o toohelp k podrobnostem další hello telemetrická data pro OEE a klíčových ukazatelů výkonu toohelp s detekce anomálií.</span><span class="sxs-lookup"><span data-stu-id="036d3-226">You then used Time Series Insights tooprovide more information toohelp drill further into hello telemetry data for OEE and KPIs toohelp with detecting anomalies.</span></span> <span data-ttu-id="036d3-227">Také použít hello výstrahy panely tooview problémy s vaší továrny a použít hello akce k dispozici tooyou tooresolve hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="036d3-227">You also used hello alert panel tooview issues with your factories and you used hello actions available tooyou tooresolve hello alert.</span></span>

## <a name="other-features"></a><span data-ttu-id="036d3-228">Další funkce</span><span class="sxs-lookup"><span data-stu-id="036d3-228">Other features</span></span>

<span data-ttu-id="036d3-229">Hello následující části popisují některé další funkce, které nejsou popsané v předchozím scénáři hello hello připojen objekt pro vytváření řešení.</span><span class="sxs-lookup"><span data-stu-id="036d3-229">hello following sections describe some additional features of hello connected factory solution that are not described in hello previous scenario.</span></span>

## <a name="apply-filters"></a><span data-ttu-id="036d3-230">Použití filtrů</span><span class="sxs-lookup"><span data-stu-id="036d3-230">Apply filters</span></span>

1. <span data-ttu-id="036d3-231">Klikněte na tlačítko hello **dvojitou** toodisplay seznam k dispozici tyto filtry na buď hello factory umístění panelu nebo hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="036d3-231">Click hello **chevron** toodisplay a list of available filters in either hello factory locations panel or hello alerts panel.</span></span>

2. <span data-ttu-id="036d3-232">Hello filtry panelu se zobrazí za vás.</span><span class="sxs-lookup"><span data-stu-id="036d3-232">hello filters panel is displayed for you.</span></span> 

    ![Filtry v předkonfigurovaném řešení propojené továrny][cf-img-alert-filter]

3. <span data-ttu-id="036d3-234">Zvolte hello filtr, který požadujete.</span><span class="sxs-lookup"><span data-stu-id="036d3-234">Choose hello filter that you require.</span></span> <span data-ttu-id="036d3-235">Je také možné tootype libovolný text do pole filtru hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-235">It is also possible tootype free text into hello filter fields.</span></span>

4. <span data-ttu-id="036d3-236">Filtr Hello se použije pro vás.</span><span class="sxs-lookup"><span data-stu-id="036d3-236">hello filter is then applied for you.</span></span> <span data-ttu-id="036d3-237">Stav filtru Hello se také zobrazí v řídicí panel hello prostřednictvím trychtýřového grafu, který se zobrazí v hello továrny a výstrahy tabulky.</span><span class="sxs-lookup"><span data-stu-id="036d3-237">hello filter state is also shown in hello dashboard via a funnel that displays in hello factories and alerts tables.</span></span>

    ![Filtry v předkonfigurovaném řešení propojené továrny][cf-img-alert-filter-funnel]

    > [!NOTE]
    > <span data-ttu-id="036d3-239">Aktivní filtr nemá vliv na hodnoty OEE a klíčového ukazatele výkonu hello zobrazí, jenom filtruje hello zobrazovat obsah.</span><span class="sxs-lookup"><span data-stu-id="036d3-239">An active filter does not affect hello displayed OEE and KPI values, it only filters hello list contents.</span></span>

5. <span data-ttu-id="036d3-240">tooclear filtr, klikněte na hello trychtýřového grafu a klikněte na tlačítko filtru panelu kontextu filtru hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-240">tooclear a filter, click hello funnel and click filter in hello filter context panel.</span></span> <span data-ttu-id="036d3-241">Hello text **všechny** se zobrazí v hello továrny a výstrahy tabulky.</span><span class="sxs-lookup"><span data-stu-id="036d3-241">hello text **All** is displayed in hello factories and alerts tables.</span></span>

## <a name="browse-an-opc-ua-server"></a><span data-ttu-id="036d3-242">Procházení serveru OPC UA</span><span class="sxs-lookup"><span data-stu-id="036d3-242">Browse an OPC UA server</span></span>

<span data-ttu-id="036d3-243">Když nasadíte hello předkonfigurované řešení, automaticky zřizovat simulované OPC UA servery, které můžete vyhledat pomocí prohlížeče řešení hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-243">When you deploy hello preconfigured solution, you automatically provision simulated OPC UA servers that you can browse via hello solution browser.</span></span> <span data-ttu-id="036d3-244">Tyto servery jsou *simulované servery OPC UA*.</span><span class="sxs-lookup"><span data-stu-id="036d3-244">These servers are *simulated OPC UA servers*.</span></span> <span data-ttu-id="036d3-245">Simulované servery usnadnění pro vás tooexperiment hello předkonfigurované řešení bez hello nutné toodeploy skutečné, fyzické servery.</span><span class="sxs-lookup"><span data-stu-id="036d3-245">Simulated servers make it easy for you tooexperiment with hello preconfigured solution without hello need toodeploy real, physical servers.</span></span> <span data-ttu-id="036d3-246">Pokud chcete tooconnect skutečná řešení toohello serveru OPC UA, najdete v části hello [připojit vaše OPC UA zařízení toohello připojené objekt pro vytváření předkonfigurovaného řešení] [ lnk-connect-cf] kurzu.</span><span class="sxs-lookup"><span data-stu-id="036d3-246">If you do want tooconnect a real OPC UA server toohello solution, see hello [Connect your OPC UA device toohello connected factory preconfigured solution][lnk-connect-cf] tutorial.</span></span>

1. <span data-ttu-id="036d3-247">Klikněte na tlačítko hello **factory ikonu** hello řídicí panel navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="036d3-247">Click hello **factory icon** in hello dashboard navigation bar.</span></span>

    ![Prohlížeč serveru v předkonfigurovaném řešení propojené továrny][cf-img-server-browser]

2. <span data-ttu-id="036d3-249">Zvolte jeden ze serverů hello hello předkonfigurované seznamu.</span><span class="sxs-lookup"><span data-stu-id="036d3-249">Choose one of hello servers from hello preconfigured list.</span></span> <span data-ttu-id="036d3-250">Tento seznam obsahuje hello servery, které jsou nasazeny pro vás v hello předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="036d3-250">This list shows hello servers that are deployed for you in hello preconfigured solution.</span></span>

    ![Výběr serveru v předkonfigurovaném řešení propojené továrny][cf-img-server-choice]

3. <span data-ttu-id="036d3-252">Klikněte na **Připojit**, zobrazí se dialogové okno zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="036d3-252">Click **Connect**, a security dialog displays.</span></span> <span data-ttu-id="036d3-253">Pro simulaci hello je bezpečné tooclick **pokračovat**</span><span class="sxs-lookup"><span data-stu-id="036d3-253">For hello simulation, it is safe tooclick **Proceed**</span></span>

4. <span data-ttu-id="036d3-254">tooexpand hello uzly ve stromu hello serveru, klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="036d3-254">tooexpand any of hello nodes in hello server tree, click it.</span></span> <span data-ttu-id="036d3-255">Vedle uzlů, které publikují telemetrii, je značka.</span><span class="sxs-lookup"><span data-stu-id="036d3-255">Nodes that are publishing telemetry have a tick mark beside them.</span></span>

    ![Stromová struktura serveru v předkonfigurovaném řešení propojené továrny][cf-img-server-tree]

5. <span data-ttu-id="036d3-257">Klikněte pravým tlačítkem myši položku tooread, zápis, publikovat nebo volání tento uzel.</span><span class="sxs-lookup"><span data-stu-id="036d3-257">Right-click an item tooread, write, publish, or call that node.</span></span> <span data-ttu-id="036d3-258">k dispozici tooyou Hello akce závisí na oprávnění a atributy hello hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="036d3-258">hello actions available tooyou depend on your permissions and hello attributes of hello node.</span></span> <span data-ttu-id="036d3-259">Hello přečíst možnost toodisplays panelu kontextu zobrazující hello hodnotu hello konkrétním uzlu.</span><span class="sxs-lookup"><span data-stu-id="036d3-259">hello read option toodisplays a context panel showing hello value of hello specific node.</span></span> <span data-ttu-id="036d3-260">Hello zápisu zobrazí možnost kontextu panel, kde můžete zadat novou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="036d3-260">hello write option displays a context panel where you can enter a new value.</span></span> <span data-ttu-id="036d3-261">možnost volání Hello zobrazuje uzel, kde můžete zadat parametry hello hello volání.</span><span class="sxs-lookup"><span data-stu-id="036d3-261">hello call option displays a node where you can enter hello parameters for hello call.</span></span>

## <a name="publish-a-node"></a><span data-ttu-id="036d3-262">Publikování uzlu</span><span class="sxs-lookup"><span data-stu-id="036d3-262">Publish a node</span></span>

<span data-ttu-id="036d3-263">Když přejdete *simulované serveru OPC UA*, můžete také zvolit toopublish nové uzly.</span><span class="sxs-lookup"><span data-stu-id="036d3-263">When you browse a *simulated OPC UA server*, you can also choose toopublish new nodes.</span></span> <span data-ttu-id="036d3-264">Můžete analyzovat hello telemetrie z tyto uzly v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-264">You can analyze hello telemetry from these nodes in hello solution.</span></span> <span data-ttu-id="036d3-265">Tyto *simulated OPC UA servery* zkontrolujte snadno tooexperiment hello předkonfigurované řešení bez nasazení skutečné, fyzické zařízení.</span><span class="sxs-lookup"><span data-stu-id="036d3-265">These *simulated OPC UA servers* make it easy tooexperiment with hello preconfigured solution without deploying real, physical devices.</span></span>

1. <span data-ttu-id="036d3-266">Procházejte tooa uzel ve stromu prohlížeče server OPC UA chcete toopublish hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-266">Browse tooa node in hello OPC UA server browser tree that you wish toopublish.</span></span>

2. <span data-ttu-id="036d3-267">Klikněte pravým tlačítkem na uzel hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-267">Right-click hello node.</span></span>

3. <span data-ttu-id="036d3-268">Zvolte **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="036d3-268">Choose **Publish**.</span></span>

    ![Publikování uzlu v propojené továrně][cf-img-publish-node]

4. <span data-ttu-id="036d3-270">Kontext panelu se zobrazí, která sděluje, že hello publikování proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="036d3-270">A context panel appears which tells you that hello publish has succeeded.</span></span> <span data-ttu-id="036d3-271">Hello uzel se objeví v zobrazení úrovně hello stanice s zaškrtnutí u ní.</span><span class="sxs-lookup"><span data-stu-id="036d3-271">hello node appears in hello station level view with a check mark beside it.</span></span>

    ![Úspěšné publikování v předkonfigurovaném řešení propojené továrny][cf-img-publish-success]

## <a name="command-and-control"></a><span data-ttu-id="036d3-273">Příkazy a ovládání</span><span class="sxs-lookup"><span data-stu-id="036d3-273">Command and control</span></span>

<span data-ttu-id="036d3-274">vytváření připojené Hello umožňuje příkazů a řídit zařízení odvětví přímo z cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-274">hello connected factory allows you command and control your industry devices directly from hello cloud.</span></span> <span data-ttu-id="036d3-275">Pomocí této funkce toorespond tooalerts generován zařízením hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-275">You can use this feature toorespond tooalerts generated by hello device.</span></span> <span data-ttu-id="036d3-276">Například může odeslat příkaz toohello zařízení z cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-276">For example, you could send a command toohello device from hello cloud.</span></span> <span data-ttu-id="036d3-277">Můžete najít hello dostupné příkazy v hello **StationCommands** uzel v hello OPC UA servery prohlížeče stromu.</span><span class="sxs-lookup"><span data-stu-id="036d3-277">You can find hello available commands in hello **StationCommands** node in hello OPC UA servers browser tree.</span></span> <span data-ttu-id="036d3-278">V tomto scénáři otevřete ventil verze tlak na stanici sestavení hello produkční řádku v Mnichově.</span><span class="sxs-lookup"><span data-stu-id="036d3-278">In this scenario, you open a pressure release valve on hello assembly station of a production line in Munich.</span></span> <span data-ttu-id="036d3-279">Příkazy a ovládání funkce hello toouse musí být v hello **správce** role pro hello předkonfigurované řešení nasazení.</span><span class="sxs-lookup"><span data-stu-id="036d3-279">toouse hello command and control functionality, you must be in hello **Administrator** role for hello preconfigured solution deployment.</span></span>

1. <span data-ttu-id="036d3-280">Procházet toohello **StationCommands** uzel v hello OPC UA server prohlížeče stromu.</span><span class="sxs-lookup"><span data-stu-id="036d3-280">Browse toohello **StationCommands** node in hello OPC UA server browser tree.</span></span>

2. <span data-ttu-id="036d3-281">Vyberte příkaz hello chcete použít.</span><span class="sxs-lookup"><span data-stu-id="036d3-281">Choose hello command that you wish use.</span></span>

3. <span data-ttu-id="036d3-282">Klikněte pravým tlačítkem na uzel hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-282">Right-click hello node.</span></span>

4. <span data-ttu-id="036d3-283">Zvolte **Volání**.</span><span class="sxs-lookup"><span data-stu-id="036d3-283">Choose **Call**.</span></span>

    ![Příkaz volání v předkonfigurovaném řešení propojené továrny][cf-img-call-command]

5. <span data-ttu-id="036d3-285">Kontext panelu se zobrazí informací, jakou metodu jste o toocall a všechny podrobnosti parametrů se vztahuje.</span><span class="sxs-lookup"><span data-stu-id="036d3-285">A context panel appears informing you which method you are about toocall and any parameter details is applicable.</span></span>

6. <span data-ttu-id="036d3-286">Zvolte **Volání**.</span><span class="sxs-lookup"><span data-stu-id="036d3-286">Choose **Call**.</span></span>

    ![Místní panel volání v předkonfigurovaném řešení propojené továrny][cf-img-call-context]

7. <span data-ttu-id="036d3-288">panel kontextu Hello je aktualizovaný tooinform že hello volání metody, které bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="036d3-288">hello context panel is updated tooinform you that hello method call succeeded.</span></span> <span data-ttu-id="036d3-289">Můžete ověřit hello volání úspěšné načtením hello hodnota hello přetížení uzlu, který aktualizován v důsledku volání hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-289">You can verify hello call succeeded by reading hello value of hello pressure node that updated as a result of hello call.</span></span>

    ![Úspěšné volání v předkonfigurovaném řešení propojené továrny][cf-img-call-success]


## <a name="behind-hello-scenes"></a><span data-ttu-id="036d3-291">Pozadí hello</span><span class="sxs-lookup"><span data-stu-id="036d3-291">Behind hello scenes</span></span>

<span data-ttu-id="036d3-292">Když nasadíte předkonfigurované řešení, proces nasazení hello vytvoří několik prostředků v hello předplatné Azure, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="036d3-292">When you deploy a preconfigured solution, hello deployment process creates multiple resources in hello Azure subscription you selected.</span></span> <span data-ttu-id="036d3-293">Tyto prostředky můžete zobrazit v hello Azure [portál][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="036d3-293">You can view these resources in hello Azure [portal][lnk-portal].</span></span> <span data-ttu-id="036d3-294">proces nasazení Hello vytvoří **skupiny prostředků** s názvem na základě názvu hello jste vybrali pro předkonfigurované řešení:</span><span class="sxs-lookup"><span data-stu-id="036d3-294">hello deployment process creates a **resource group** with a name based on hello name you choose for your preconfigured solution:</span></span>

![Předkonfigurované řešení v hello portálu Azure][img-cf-portal]

<span data-ttu-id="036d3-296">Hello nastavení každého prostředku můžete zobrazit výběrem v seznamu prostředků ve skupině prostředků hello hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-296">You can view hello settings of each resource by selecting it in hello list of resources in hello resource group.</span></span>

<span data-ttu-id="036d3-297">Můžete také zobrazit zdrojový kód hello hello předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="036d3-297">You can also view hello source code for hello preconfigured solution.</span></span> <span data-ttu-id="036d3-298">Hello připojené objekt pro vytváření předkonfigurovaného řešení zdrojový kód je v hello [azure-iot připojené factory] [ lnk-cfgithub] úložiště GitHub:</span><span class="sxs-lookup"><span data-stu-id="036d3-298">hello connected factory preconfigured solution source code is in hello [azure-iot-connected-factory][lnk-cfgithub] GitHub repository:</span></span>

<span data-ttu-id="036d3-299">Až skončíte, můžete odstranit hello předkonfigurované řešení ze svého předplatného Azure na hello [azureiotsuite.com] [ lnk-azureiotsuite] lokality.</span><span class="sxs-lookup"><span data-stu-id="036d3-299">When you are done, you can delete hello preconfigured solution from your Azure subscription on hello [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="036d3-300">Tento web můžete tooeasily odstranit všechny prostředky, které byly zřízeny při vytváření hello předkonfigurované řešení hello.</span><span class="sxs-lookup"><span data-stu-id="036d3-300">This site enables you tooeasily delete all hello resources that were provisioned when you created hello preconfigured solution.</span></span>

> [!NOTE]
> <span data-ttu-id="036d3-301">tooensure odstraňte všechny položky související s toohello předkonfigurované řešení, odstraňte jej na hello [azureiotsuite.com] [ lnk-azureiotsuite] lokality.</span><span class="sxs-lookup"><span data-stu-id="036d3-301">tooensure that you delete everything related toohello preconfigured solution, delete it on hello [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="036d3-302">Neodstraňujte skupinu prostředků hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="036d3-302">Do not delete hello resource group in hello portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="036d3-303">Další kroky</span><span class="sxs-lookup"><span data-stu-id="036d3-303">Next Steps</span></span>

<span data-ttu-id="036d3-304">Teď, když máte ukázku nasazenou pracovní předkonfigurované řešení, budete pokračovat, Začínáme se službou IoT Suite načtením hello následující články:</span><span class="sxs-lookup"><span data-stu-id="036d3-304">Now that you’ve deployed a working preconfigured solution, you can continue getting started with IoT Suite by reading hello following articles:</span></span>

* <span data-ttu-id="036d3-305">[Průvodce předkonfigurovaným řešením propojené továrny][lnk-rm-walkthrough]</span><span class="sxs-lookup"><span data-stu-id="036d3-305">[Connected factory preconfigured solution walkthrough][lnk-rm-walkthrough]</span></span>
* <span data-ttu-id="036d3-306">[Připojit vaše zařízení toohello připojené objekt pro vytváření předkonfigurovaného řešení][lnk-connect-cf]</span><span class="sxs-lookup"><span data-stu-id="036d3-306">[Connect your device toohello Connected factory preconfigured solution][lnk-connect-cf]</span></span>
* <span data-ttu-id="036d3-307">[Oprávnění na webu azureiotsuite.com hello][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="036d3-307">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>

[img-cf-home]:media/iot-suite-connected-factory-overview/cf-dashboard.png
[img-launch-solution]: media/iot-suite-connected-factory-overview/launch-cf.png
[cf-img-menu]: media/iot-suite-connected-factory-overview/cf-dashboard-menu.png
[cf-img-factories]:media/iot-suite-connected-factory-overview/cf-dashboard-factory.png
[cf-img-map]:media/iot-suite-connected-factory-overview/cf-dashboard-map.png
[cf-img-alerts]:media/iot-suite-connected-factory-overview/cf-dashboard-alerts.png
[cf-img-oee]:media/iot-suite-connected-factory-overview/cf-dashboard-oee.png
[cf-img-kpi]:media/iot-suite-connected-factory-overview/cf-dashboard-kpi.png
[cf-img-tsi-visualization]:media/iot-suite-connected-factory-overview/cf-dashboard-tsi.png
[cf-img-tsi-explorer]:media/iot-suite-connected-factory-overview/tsi-explorer.png
[cf-img-server-browser]: media/iot-suite-connected-factory-overview/cf-dashboard-browser.png
[cf-img-server-choice]: media/iot-suite-connected-factory-overview/cf-select-server.png
[cf-img-server-tree]:media/iot-suite-connected-factory-overview/cf-server-tree.png
[cf-img-publish-node]:media/iot-suite-connected-factory-overview/cf-publish-node1.png
[cf-img-publish-success]:media/iot-suite-connected-factory-overview/cf-publish-success.png
[cf-img-call-command]:media/iot-suite-connected-factory-overview/cf-command-and-control-call.png
[cf-img-call-context]:media/iot-suite-connected-factory-overview/cf-command-and-control-call-button.png
[cf-img-call-success]:media/iot-suite-connected-factory-overview/cf-command-and-control-succeed.png
[img-cf-portal]:media/iot-suite-connected-factory-overview/cf-resource-group.png
[cf-img-alert-filter]:media/iot-suite-connected-factory-overview/cf-filter.png
[cf-img-alert-filter-funnel]:media/iot-suite-connected-factory-overview/cf-filter-funnel.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-portal]: http://portal.azure.com/
[lnk-cfgithub]: https://github.com/Azure/azure-iot-connected-factory
[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md