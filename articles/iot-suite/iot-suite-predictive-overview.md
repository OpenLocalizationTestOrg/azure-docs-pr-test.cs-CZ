---
title: "předkonfigurované řešení aaaPredictive údržby | Microsoft Docs"
description: "Popis prediktivní údržby Azure IoT Suite hello předkonfigurovaného řešení."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: b370b3d7-2ce5-4906-9818-3aeedd471ee3
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 2d09801467d33db6b7d6333fa071aea2bf573f20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="predictive-maintenance-preconfigured-solution-overview"></a><span data-ttu-id="4e4af-103">Přehled řešení předkonfigurované prediktivní údržby</span><span class="sxs-lookup"><span data-stu-id="4e4af-103">Predictive maintenance preconfigured solution overview</span></span>

<span data-ttu-id="4e4af-104">Hello *prediktivní údržby* [předkonfigurované řešení] [ lnk_preconfigured_solutions] je jedním z hello [Microsoft Azure IoT Suite] [ lnk_iot_suite] předkonfigurovaných řešení.</span><span class="sxs-lookup"><span data-stu-id="4e4af-104">hello *predictive maintenance* [preconfigured solution][lnk_preconfigured_solutions] is one of hello [Microsoft Azure IoT Suite][lnk_iot_suite] preconfigured solutions.</span></span> <span data-ttu-id="4e4af-105">Toto řešení integruje sběr telemetrických dat ze zařízení v reálném čase s prediktivním modelem vytvořeným pomocí služby [Azure Machine Learning][lnk-machine-learning].</span><span class="sxs-lookup"><span data-stu-id="4e4af-105">This solution integrates real-time device telemetry collection with a predictive model created using [Azure Machine Learning][lnk-machine-learning].</span></span>

<span data-ttu-id="4e4af-106">S Azure IoT Suite můžete rychle a snadno připojit tooand monitorování prostředky a analyzovat telemetrická data v reálném čase v řídicích panelů a vizualizací.</span><span class="sxs-lookup"><span data-stu-id="4e4af-106">With Azure IoT Suite, you can quickly and easily connect tooand monitor assets, and analyze telemetry in real time in dashboards and visualizations.</span></span> <span data-ttu-id="4e4af-107">V řešení prediktivní údržby hello hello řídicích panelů a vizualizací poskytují nové informace, které můžete zvýšit efektivitu a výnosy.</span><span class="sxs-lookup"><span data-stu-id="4e4af-107">In hello predictive maintenance solution, hello dashboards and visualizations provide you with new intelligence that can drive efficiencies and enhance revenue streams.</span></span>

## <a name="hello-scenario"></a><span data-ttu-id="4e4af-108">Hello scénář</span><span class="sxs-lookup"><span data-stu-id="4e4af-108">hello Scenario</span></span>

<span data-ttu-id="4e4af-109">Fabrikam je regionální letecká společnost, která se zaměřuje na pohodlí zákazníků za konkurenční ceny.</span><span class="sxs-lookup"><span data-stu-id="4e4af-109">Fabrikam is a regional airline that focuses on great customer experience at competitive prices.</span></span> <span data-ttu-id="4e4af-110">Jednou z příčin zpoždění letů jsou problémy s údržbou, protože údržba leteckých motorů je velmi náročná.</span><span class="sxs-lookup"><span data-stu-id="4e4af-110">One cause of flight delays is maintenance issues and aircraft engine maintenance is particularly challenging.</span></span> <span data-ttu-id="4e4af-111">Společnost Fabrikam musí vyhnout poruchám motorů během letu za každou cenu tak, aby ho své stroje pravidelně kontroluje a plány údržby podle plánu tooa.</span><span class="sxs-lookup"><span data-stu-id="4e4af-111">Fabrikam must avoid engine failure during flight at all costs, so it inspects its engines regularly and schedules maintenance according tooa plan.</span></span> <span data-ttu-id="4e4af-112">Letadla, moduly neopotřebovávají hello však stejné.</span><span class="sxs-lookup"><span data-stu-id="4e4af-112">However, aircraft engines don’t always wear hello same.</span></span> <span data-ttu-id="4e4af-113">Některou údržbu motorů není vždy nutné provádět.</span><span class="sxs-lookup"><span data-stu-id="4e4af-113">Some unnecessary maintenance is performed on engines.</span></span> <span data-ttu-id="4e4af-114">Naproti tomu se můžou vyskytnout problémy, kvůli kterým musí letadlo zůstat na zemi, dokud není opravené.</span><span class="sxs-lookup"><span data-stu-id="4e4af-114">More importantly, issues arise which can ground an aircraft until maintenance is performed.</span></span> <span data-ttu-id="4e4af-115">Pokud je letadlo v místě kde hello vhodní technici nebo náhradní díly nejsou k dispozici, k těmto problémům může být obzvláště drahé.</span><span class="sxs-lookup"><span data-stu-id="4e4af-115">If an aircraft is at a location where hello right technicians or spare parts are not available, these issues can be especially costly.</span></span>

<span data-ttu-id="4e4af-116">Hello motory letadel společnosti Fabrikam jsou vybaveny snímači, které monitorují stav motoru během letu.</span><span class="sxs-lookup"><span data-stu-id="4e4af-116">hello engines of Fabrikam’s aircraft are instrumented with sensors that monitor engine conditions during flight.</span></span> <span data-ttu-id="4e4af-117">Společnost Fabrikam používá hello prediktivní údržby řešení toocollect hello data shromážděná během letu hello.</span><span class="sxs-lookup"><span data-stu-id="4e4af-117">Fabrikam uses hello predictive maintenance solution toocollect hello sensor data collected during hello flight.</span></span> <span data-ttu-id="4e4af-118">Po mnoho let provozu a selháních motorů společnosti Fabrikam datových vědců model, způsob toopredict hello zbývající dobu životnosti (RUL) leteckého motoru.</span><span class="sxs-lookup"><span data-stu-id="4e4af-118">After accumulating years of engine operational and failure data, Fabrikam’s data scientists have modeled a way toopredict hello Remaining Useful Life (RUL) of an aircraft engine.</span></span> <span data-ttu-id="4e4af-119">Hello model používá korelace mezi údaji ze čtyř hello modul senzory a motoru a opotřebením motoru který vede tooeventual selhání.</span><span class="sxs-lookup"><span data-stu-id="4e4af-119">hello model uses a correlation between data from four of hello engine sensors and engine wear that leads tooeventual failure.</span></span> <span data-ttu-id="4e4af-120">Když společnost Fabrikam stále tooperform pravidelné kontroly tooensure zabezpečení, ho teď můžete použít hello modely toocompute hello RUL jednotlivých motorů po každém letu.</span><span class="sxs-lookup"><span data-stu-id="4e4af-120">While Fabrikam continues tooperform regular inspections tooensure safety, it can now use hello models toocompute hello RUL for each engine after every flight.</span></span> <span data-ttu-id="4e4af-121">Hello model používá hello shromažďování telemetrických údajů z hello motorů během letu hello.</span><span class="sxs-lookup"><span data-stu-id="4e4af-121">hello model uses hello telemetry collected from hello engines during hello flight.</span></span> <span data-ttu-id="4e4af-122">Společnost Fabrikam teď může předpovídat budoucí selhání a s předstihem plánovat údržbu a opravy.</span><span class="sxs-lookup"><span data-stu-id="4e4af-122">Fabrikam can now predict future points of failure and plan for maintenance and repair in advance.</span></span>

> [!NOTE]
> <span data-ttu-id="4e4af-123">Hello model řešení využívá data o opotřebení ze skutečných motorů.</span><span class="sxs-lookup"><span data-stu-id="4e4af-123">hello solution model uses actual engine wear data.</span></span>

<span data-ttu-id="4e4af-124">Podle predikci hello bodu čas potřebné údržby, může společnost Fabrikam optimalizovat náklady tooreduce její operace.</span><span class="sxs-lookup"><span data-stu-id="4e4af-124">By predicting hello point when maintenance is required, Fabrikam can optimize its operations tooreduce costs.</span></span>

<span data-ttu-id="4e4af-125">Koordinátoři údržby spolupracují s plánovači při:</span><span class="sxs-lookup"><span data-stu-id="4e4af-125">Maintenance coordinators work with schedulers to:</span></span>

- <span data-ttu-id="4e4af-126">Plán údržby toocoincide s letadel v konkrétních místech zastavení.</span><span class="sxs-lookup"><span data-stu-id="4e4af-126">Plan maintenance toocoincide with an aircraft stopping at a particular location.</span></span>
- <span data-ttu-id="4e4af-127">Zkontrolujte, zda dostatek času k dispozici pro hello letadla toobe mimo provoz aniž by to způsobilo harmonogramu.</span><span class="sxs-lookup"><span data-stu-id="4e4af-127">Ensure sufficient time is available for hello aircraft toobe out of service without causing schedule disruption.</span></span>
- <span data-ttu-id="4e4af-128">tooensure technici tooschedule že servis letadla bude probíhat efektivně bez prodlev.</span><span class="sxs-lookup"><span data-stu-id="4e4af-128">tooschedule technicians tooensure that aircraft are serviced efficiently without wait time.</span></span>

<span data-ttu-id="4e4af-129">Plán údržby dostávají také správci řízení zásob, kteří díky tomu mohou optimalizovat proces objednávání a skladové zásoby náhradních dílů.</span><span class="sxs-lookup"><span data-stu-id="4e4af-129">Inventory control managers receive maintenance plans, so they can optimize their ordering process and spare parts inventory.</span></span>

<span data-ttu-id="4e4af-130">Tyto aktivity povolte prostoje letadel společnosti Fabrikam toominimize a snížení provozních nákladů a zajistit bezpečnost cestujících i posádek hello.</span><span class="sxs-lookup"><span data-stu-id="4e4af-130">These activities enable Fabrikam toominimize aircraft ground time and reduce operating costs while ensuring hello safety of passengers and crew.</span></span>

<span data-ttu-id="4e4af-131">toounderstand jak [Azure IoT Suite] [ lnk_iot_suite] poskytuje zákazníkům hello potřebovat toorealize hello potenciál prediktivní údržby, zkontrolujte tato [infografice] [lnk_infographic].</span><span class="sxs-lookup"><span data-stu-id="4e4af-131">toounderstand how [Azure IoT Suite][lnk_iot_suite] provides hello capabilities customers need toorealize hello potential of predictive maintenance, review this [infographic][lnk_infographic].</span></span>

## <a name="how-hello-predictive-maintenance-solution-is-built"></a><span data-ttu-id="4e4af-132">Jak je sestaveno řešení prediktivní údržby hello</span><span class="sxs-lookup"><span data-stu-id="4e4af-132">How hello predictive maintenance solution is built</span></span>

<span data-ttu-id="4e4af-133">řešení Hello používá existující model Azure Machine Learning k dispozici jako šablona tooshow možnosti práce s telemetrickými údaji ze zařízení shromažďovaných prostřednictvím služeb IoT Suite.</span><span class="sxs-lookup"><span data-stu-id="4e4af-133">hello solution uses an existing Azure Machine Learning model available as a template tooshow these capabilities working from device telemetry collected through IoT Suite services.</span></span> <span data-ttu-id="4e4af-134">Společnost Microsoft vytvořila [regresní model] [ lnk_regression_model] leteckého motoru na základě dat o veřejně dostupné<sup>\[1\]</sup>a krok za krokem informace o tom, jak toouse hello modelu.</span><span class="sxs-lookup"><span data-stu-id="4e4af-134">Microsoft has built a [regression model][lnk_regression_model] of an aircraft engine based on publically available data<sup>\[1\]</sup>, and step-by-step guidance on how toouse hello model.</span></span>

<span data-ttu-id="4e4af-135">Hello řešení prediktivní údržby Azure IoT používá regresní model hello vytvořené z této šablony.</span><span class="sxs-lookup"><span data-stu-id="4e4af-135">hello Azure IoT predictive maintenance solution uses hello regression model created from this template.</span></span> <span data-ttu-id="4e4af-136">Hello modelu je nasadit do vašeho předplatného Azure a vystavené prostřednictvím automaticky generovaného rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4e4af-136">hello model is deployed into your Azure subscription and exposed through an automatically generated API.</span></span> <span data-ttu-id="4e4af-137">Hello řešení obsahuje podmnožinu hello testování dat, která představují 4 (z celkem 100) moduly a hello 4 (z celkem 21) senzor datových proudů.</span><span class="sxs-lookup"><span data-stu-id="4e4af-137">hello solution includes a subset of hello testing data representing 4 (of 100 total) engines and hello 4 (of 21 total) sensor data streams.</span></span> <span data-ttu-id="4e4af-138">Tato data jsou dostatečná tooprovide přesné výsledky z hello trained model.</span><span class="sxs-lookup"><span data-stu-id="4e4af-138">This data is sufficient tooprovide an accurate result from hello trained model.</span></span>

<span data-ttu-id="4e4af-139">*\[1\] A. Saxena and K. Goebel (2008). „Turbofan Engine Degradation Simulation Data Set“, datové úložiště NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*</span><span class="sxs-lookup"><span data-stu-id="4e4af-139">*\[1\] A. Saxena and K. Goebel (2008). "Turbofan Engine Degradation Simulation Data Set", NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*</span></span>

## <a name="get-started-with-predictive-maintenance"></a><span data-ttu-id="4e4af-140">Začínáme s prediktivní údržbou</span><span class="sxs-lookup"><span data-stu-id="4e4af-140">Get started with predictive maintenance</span></span>

<span data-ttu-id="4e4af-141">Tento kurz ukazuje, jak tooprovision hello řešení prediktivní údržby.</span><span class="sxs-lookup"><span data-stu-id="4e4af-141">This tutorial shows you how tooprovision hello predictive maintenance solution.</span></span> <span data-ttu-id="4e4af-142">Je také vás provede procesem hello základní funkce řešení prediktivní údržby hello.</span><span class="sxs-lookup"><span data-stu-id="4e4af-142">It also walks you through hello basic features of hello predictive maintenance solution.</span></span> <span data-ttu-id="4e4af-143">Mnoho z těchto funkcí můžete přistupovat prostřednictvím hello řídicí panel řešení, která nasazuje společně s hello předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="4e4af-143">You can access many of these features through hello solution dashboard that deploys along with hello preconfigured solution.</span></span>

<span data-ttu-id="4e4af-144">toocomplete tohoto kurzu potřebujete aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="4e4af-144">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="4e4af-145">Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební.</span><span class="sxs-lookup"><span data-stu-id="4e4af-145">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="4e4af-146">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="4e4af-146">For details, see [Azure Free Trial][lnk_free_trial].</span></span>

1. <span data-ttu-id="4e4af-147">Přihlaste se příliš[azureiotsuite.com] [ lnk-azureiotsuite] pomocí vaší Azure přihlašovací údaje účtu a klikněte na tlačítko  **+**  toocreate řešení.</span><span class="sxs-lookup"><span data-stu-id="4e4af-147">Log on too[azureiotsuite.com][lnk-azureiotsuite] using your Azure account credentials, and click **+** toocreate a solution.</span></span>
1. <span data-ttu-id="4e4af-148">Klikněte na tlačítko **vyberte** hello **prediktivní údržby** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="4e4af-148">Click **Select** hello **Predictive maintenance** tile.</span></span>
1. <span data-ttu-id="4e4af-149">Zadejte **Název řešení** pro předkonfigurované řešení prediktivní údržby.</span><span class="sxs-lookup"><span data-stu-id="4e4af-149">Enter a **Solution name** for your predictive maintenance preconfigured solution.</span></span>
1. <span data-ttu-id="4e4af-150">Vyberte hello **oblast** a **předplatné** chcete toouse tooprovision hello řešení.</span><span class="sxs-lookup"><span data-stu-id="4e4af-150">Select hello **Region** and **Subscription** you want toouse tooprovision hello solution.</span></span>
1. <span data-ttu-id="4e4af-151">Klikněte na tlačítko **vytvořit řešení** toobegin hello procesu zřizování.</span><span class="sxs-lookup"><span data-stu-id="4e4af-151">Click **Create Solution** toobegin hello provisioning process.</span></span> <span data-ttu-id="4e4af-152">Tento proces obvykle trvá několik minut toorun.</span><span class="sxs-lookup"><span data-stu-id="4e4af-152">This process typically takes several minutes toorun.</span></span>

### <a name="wait-for-hello-provisioning-process-toocomplete"></a><span data-ttu-id="4e4af-153">Počkejte hello zřizování toocomplete procesu</span><span class="sxs-lookup"><span data-stu-id="4e4af-153">Wait for hello provisioning process toocomplete</span></span>

1. <span data-ttu-id="4e4af-154">Klikněte na dlaždici s řešením hello **zřizování** stavu.</span><span class="sxs-lookup"><span data-stu-id="4e4af-154">Click hello tile for your solution with **Provisioning** status.</span></span>
1. <span data-ttu-id="4e4af-155">Všimněte si hello **stavy zřizování** při nasazování služby Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="4e4af-155">Notice hello **Provisioning states** as Azure services are deployed in your Azure subscription.</span></span>
1. <span data-ttu-id="4e4af-156">Jakmile bude zřizování dokončeno, hello změny stavu příliš**připraven**.</span><span class="sxs-lookup"><span data-stu-id="4e4af-156">Once provisioning completes, hello status changes too**Ready**.</span></span>
1. <span data-ttu-id="4e4af-157">Klikněte na tlačítko hello dlaždice toosee hello informace o řešení v pravém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="4e4af-157">Click hello tile toosee hello details of your solution in hello right-hand pane.</span></span> <span data-ttu-id="4e4af-158">V tomto podokně můžete spustit hello řešení řídicí panel a přístup hello pracovní prostor Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4e4af-158">From this pane, you can launch hello solution dashboard and access hello Machine Learning workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="4e4af-159">Pokud narazíte na problémy nasazení hello předkonfigurované řešení, přečtěte si [oprávnění na webu azureiotsuite.com hello] [ lnk-permissions] a hello [– nejčastější dotazy] [ lnk-faq].</span><span class="sxs-lookup"><span data-stu-id="4e4af-159">If you encounter issues deploying hello preconfigured solution, review [Permissions on hello azureiotsuite.com site][lnk-permissions] and hello [FAQ][lnk-faq].</span></span> <span data-ttu-id="4e4af-160">Pokud hello problémy přetrvávají, vytvořte lístek služby na hello [portál][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="4e4af-160">If hello issues persist, create a service ticket on hello [portal][lnk-portal].</span></span>

<span data-ttu-id="4e4af-161">Existují by uživatel očekával toosee podrobnosti, které nejsou uvedené pro vaše řešení?</span><span class="sxs-lookup"><span data-stu-id="4e4af-161">Are there details you'd expect toosee that aren't listed for your solution?</span></span> <span data-ttu-id="4e4af-162">Sdělte nám návrhy na funkce na webu [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span><span class="sxs-lookup"><span data-stu-id="4e4af-162">Give us feature suggestions on [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span></span>

## <a name="view-hello-solution"></a><span data-ttu-id="4e4af-163">Zobrazení hello řešení</span><span class="sxs-lookup"><span data-stu-id="4e4af-163">View hello solution</span></span>

<span data-ttu-id="4e4af-164">Tato část vás provede hello řešení uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="4e4af-164">This section guides you through hello solution UI.</span></span>

### <a name="predictive-maintenance-dashboard"></a><span data-ttu-id="4e4af-165">Řídicí panel prediktivní údržby</span><span class="sxs-lookup"><span data-stu-id="4e4af-165">Predictive Maintenance Dashboard</span></span>

<span data-ttu-id="4e4af-166">Tato stránka ve hello webové aplikaci používá ovládací prvky PowerBI v jazyce JavaScript (viz hello [úložiště vizuálních prvků PowerBI][lnk-powerbi]) toovisualize:</span><span class="sxs-lookup"><span data-stu-id="4e4af-166">This page in hello web application uses PowerBI JavaScript controls (see hello [PowerBI-visuals repository][lnk-powerbi]) toovisualize:</span></span>

* <span data-ttu-id="4e4af-167">Hello výstupní data z hello úlohy Stream Analytics v úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="4e4af-167">hello output data from hello Stream Analytics jobs in blob storage.</span></span>
* <span data-ttu-id="4e4af-168">Hello počet každý motor letadla RUL a cyklů.</span><span class="sxs-lookup"><span data-stu-id="4e4af-168">hello RUL and cycle count per aircraft engine.</span></span>

### <a name="observing-hello-behavior-of-hello-cloud-solution"></a><span data-ttu-id="4e4af-169">Sledování chování hello hello cloudového řešení</span><span class="sxs-lookup"><span data-stu-id="4e4af-169">Observing hello behavior of hello cloud solution</span></span>

<span data-ttu-id="4e4af-170">V hello portálu Azure, přejděte toohello skupinu prostředků s názvem řešení hello jste zvolili tooview zřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="4e4af-170">In hello Azure portal, navigate toohello resource group with hello solution name you chose tooview your provisioned resources.</span></span>

![][img-resource-group]

<span data-ttu-id="4e4af-171">Při zřizování hello předkonfigurovaného řešení obdržíte e-mail s pracovní prostor Machine Learning toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="4e4af-171">When you provision hello preconfigured solution, you receive an email with a link toohello Machine Learning workspace.</span></span> <span data-ttu-id="4e4af-172">Můžete také přejít pracovní prostor Machine Learning toohello z hello [azureiotsuite.com] [ lnk-azureiotsuite] stránky zřízeného řešení.</span><span class="sxs-lookup"><span data-stu-id="4e4af-172">You can also navigate toohello Machine Learning workspace from hello [azureiotsuite.com][lnk-azureiotsuite] page for your provisioned solution.</span></span> <span data-ttu-id="4e4af-173">Dlaždice je k dispozici na této stránce, když hello řešení v hello **připraven** stavu.</span><span class="sxs-lookup"><span data-stu-id="4e4af-173">A tile is available on this page when hello solution is in hello **Ready** state.</span></span>

![][img-machine-learning]

<span data-ttu-id="4e4af-174">Hello portálu řešení uvidíte, že tento ukázkový hello je opatřen čtyři Simulovaná zařízení toorepresent dvě letadla se dvěma motory pro každé letadlo, každý se čtyřmi snímači.</span><span class="sxs-lookup"><span data-stu-id="4e4af-174">In hello solution portal, you can see that hello sample is provisioned with four simulated devices toorepresent two aircraft with two engines per aircraft, each with four sensors.</span></span> <span data-ttu-id="4e4af-175">Při první návštěvě portálu řešení toohello, dojde k zastavení simulace hello.</span><span class="sxs-lookup"><span data-stu-id="4e4af-175">When you first navigate toohello solution portal, hello simulation is stopped.</span></span>

![][img-simulation-stopped]

<span data-ttu-id="4e4af-176">Klikněte na tlačítko **spustit simulaci** toobegin hello simulace.</span><span class="sxs-lookup"><span data-stu-id="4e4af-176">Click **Start simulation** toobegin hello simulation.</span></span> <span data-ttu-id="4e4af-177">Hello senzor historie, RUL, cykly a RUL historie naplnit hello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="4e4af-177">hello sensor history, RUL, Cycles, and RUL history populate hello dashboard.</span></span>

![][img-simulation-running]

<span data-ttu-id="4e4af-178">Když RUL je menší než 160 (libovolná hodnota, zvolená pro demonstrační účely), zobrazí se portál řešení hello toohello další symbol upozornění RUL zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4e4af-178">When RUL is less than 160 (an arbitrary threshold chosen for demonstration purposes), hello solution portal displays a warning symbol next toohello RUL display.</span></span> <span data-ttu-id="4e4af-179">portál řešení Hello také označuje hello leteckého motoru žlutě.</span><span class="sxs-lookup"><span data-stu-id="4e4af-179">hello solution portal also highlights hello aircraft engine in yellow.</span></span> <span data-ttu-id="4e4af-180">Všimněte si, jak hodnoty RUL hello mají obecné klesající trend, ale mívají toobounce nahoru a dolů.</span><span class="sxs-lookup"><span data-stu-id="4e4af-180">Notice how hello RUL values have a general downward trend overall, but tend toobounce up and down.</span></span> <span data-ttu-id="4e4af-181">Toto chování je výsledkem hello různých délek cyklu a přesnosti modelu hello.</span><span class="sxs-lookup"><span data-stu-id="4e4af-181">This behavior results from hello varying cycle lengths and hello model accuracy.</span></span>

![][img-simulation-warning]

<span data-ttu-id="4e4af-182">Hello úplné simulaci trvá asi 35 minut toocomplete 148 cyklů.</span><span class="sxs-lookup"><span data-stu-id="4e4af-182">hello full simulation takes around 35 minutes toocomplete 148 cycles.</span></span> <span data-ttu-id="4e4af-183">Hello 160 RUL dosáhla prahová hodnota pro hello poprvé v přibližně po 5 minutách a oba motory dosáhl hello prahovou hodnotu v přibližně po 8 minutách.</span><span class="sxs-lookup"><span data-stu-id="4e4af-183">hello 160 RUL threshold is met for hello first time at around 5 minutes and both engines hit hello threshold at around 8 minutes.</span></span>

<span data-ttu-id="4e4af-184">Hello simulace zpracuje úplnou datovou sadu hello údaji o 148 cyklech a vyrovná na konečné hodnoty RUL a cyklů.</span><span class="sxs-lookup"><span data-stu-id="4e4af-184">hello simulation runs through hello complete dataset for 148 cycles and settles on final RUL and cycle values.</span></span>

<span data-ttu-id="4e4af-185">Můžete zastavit hello simulace u libovolné bodu, ale kliknutím na tlačítko **Start simulace** opětovná přehrání hello simulace od začátku hello hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="4e4af-185">You can stop hello simulation at any point, but clicking **Start Simulation** replays hello simulation from hello start of hello dataset.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e4af-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e4af-186">Next steps</span></span>

<span data-ttu-id="4e4af-187">Další informace o tom, jak Azure IoT umožňuje scénáře prediktivní údržby, přečtěte si toolearn [získání hodnoty z hello Internet věcí][lnk_capture_value].</span><span class="sxs-lookup"><span data-stu-id="4e4af-187">toolearn more about how Azure IoT enables predictive maintenance scenarios, read [Capture value from hello Internet of Things][lnk_capture_value].</span></span>

<span data-ttu-id="4e4af-188">Trvat [návod] [ lnk-predictive-walkthrough] řešení prediktivní údržby hello.</span><span class="sxs-lookup"><span data-stu-id="4e4af-188">Take a [walkthrough][lnk-predictive-walkthrough] of hello predictive maintenance solution.</span></span>

<span data-ttu-id="4e4af-189">Můžete také prozkoumat některé hello další funkce a možnosti hello předkonfigurovaná řešení IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="4e4af-189">You can also explore some of hello other features and capabilities of hello IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="4e4af-190">[Nejčastější dotazy k sadě IoT Suite][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="4e4af-190">[Frequently asked questions for IoT Suite][lnk-faq]</span></span>
* <span data-ttu-id="4e4af-191">[Zabezpečení IoT z hello pozadí][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="4e4af-191">[IoT security from hello ground up][lnk-security-groundup]</span></span>

[img-resource-group]: media/iot-suite-predictive-overview/resource-group.png
[img-simulation-stopped]: media/iot-suite-predictive-overview/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-overview/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-overview/simulation-warning.png
[img-machine-learning]: media/iot-suite-predictive-overview/machine-learning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3

[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-permissions]: iot-suite-permissions.md
[lnk-portal]: http://portal.azure.com/
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/