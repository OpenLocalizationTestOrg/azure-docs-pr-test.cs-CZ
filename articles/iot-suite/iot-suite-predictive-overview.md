---
title: "Předkonfigurované řešení prediktivní údržby | Dokumentace Microsoftu"
description: "Popis předkonfigurovaného řešení prediktivní údržby sady Azure IoT Suite."
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
ms.openlocfilehash: 8bad198488c4940a83eb32ec02122a91d47ca86c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="predictive-maintenance-preconfigured-solution-overview"></a><span data-ttu-id="5feaa-103">Přehled řešení předkonfigurované prediktivní údržby</span><span class="sxs-lookup"><span data-stu-id="5feaa-103">Predictive maintenance preconfigured solution overview</span></span>

<span data-ttu-id="5feaa-104">[Předkonfigurované řešení][lnk_preconfigured_solutions] *prediktivní údržby* je jedním z předkonfigurovaných řešení sady [Microsoft Azure IoT Suite][lnk_iot_suite].</span><span class="sxs-lookup"><span data-stu-id="5feaa-104">The *predictive maintenance* [preconfigured solution][lnk_preconfigured_solutions] is one of the [Microsoft Azure IoT Suite][lnk_iot_suite] preconfigured solutions.</span></span> <span data-ttu-id="5feaa-105">Toto řešení integruje sběr telemetrických dat ze zařízení v reálném čase s prediktivním modelem vytvořeným pomocí služby [Azure Machine Learning][lnk-machine-learning].</span><span class="sxs-lookup"><span data-stu-id="5feaa-105">This solution integrates real-time device telemetry collection with a predictive model created using [Azure Machine Learning][lnk-machine-learning].</span></span>

<span data-ttu-id="5feaa-106">Pomocí sady Azure IoT Suite se můžete rychle a snadno připojit k prostředkům, monitorovat je a v reálném čase analyzovat telemetrii na řídicích panelech a ve vizualizacích.</span><span class="sxs-lookup"><span data-stu-id="5feaa-106">With Azure IoT Suite, you can quickly and easily connect to and monitor assets, and analyze telemetry in real time in dashboards and visualizations.</span></span> <span data-ttu-id="5feaa-107">Řídicí panely a vizualizace v řešení prediktivní údržby poskytují nové informace, které vám můžou pomoci zvýšit efektivitu a výnosy.</span><span class="sxs-lookup"><span data-stu-id="5feaa-107">In the predictive maintenance solution, the dashboards and visualizations provide you with new intelligence that can drive efficiencies and enhance revenue streams.</span></span>

## <a name="the-scenario"></a><span data-ttu-id="5feaa-108">Scénář</span><span class="sxs-lookup"><span data-stu-id="5feaa-108">The Scenario</span></span>

<span data-ttu-id="5feaa-109">Fabrikam je regionální letecká společnost, která se zaměřuje na pohodlí zákazníků za konkurenční ceny.</span><span class="sxs-lookup"><span data-stu-id="5feaa-109">Fabrikam is a regional airline that focuses on great customer experience at competitive prices.</span></span> <span data-ttu-id="5feaa-110">Jednou z příčin zpoždění letů jsou problémy s údržbou, protože údržba leteckých motorů je velmi náročná.</span><span class="sxs-lookup"><span data-stu-id="5feaa-110">One cause of flight delays is maintenance issues and aircraft engine maintenance is particularly challenging.</span></span> <span data-ttu-id="5feaa-111">Společnost Fabrikam musí za každou cenu zabránit poruchám motorů během letu, a proto své motory pravidelně kontroluje a plánuje údržbu v souladu s plánem.</span><span class="sxs-lookup"><span data-stu-id="5feaa-111">Fabrikam must avoid engine failure during flight at all costs, so it inspects its engines regularly and schedules maintenance according to a plan.</span></span> <span data-ttu-id="5feaa-112">Letecké motory se ale neopotřebovávají všechny stejně.</span><span class="sxs-lookup"><span data-stu-id="5feaa-112">However, aircraft engines don’t always wear the same.</span></span> <span data-ttu-id="5feaa-113">Některou údržbu motorů není vždy nutné provádět.</span><span class="sxs-lookup"><span data-stu-id="5feaa-113">Some unnecessary maintenance is performed on engines.</span></span> <span data-ttu-id="5feaa-114">Naproti tomu se můžou vyskytnout problémy, kvůli kterým musí letadlo zůstat na zemi, dokud není opravené.</span><span class="sxs-lookup"><span data-stu-id="5feaa-114">More importantly, issues arise which can ground an aircraft until maintenance is performed.</span></span> <span data-ttu-id="5feaa-115">Pokud je letadlo v místě, kde nejsou k dispozici vhodní technici nebo náhradní díly, pak tyto problémy můžou být zvláště nákladné.</span><span class="sxs-lookup"><span data-stu-id="5feaa-115">If an aircraft is at a location where the right technicians or spare parts are not available, these issues can be especially costly.</span></span>

<span data-ttu-id="5feaa-116">Motory letadel společnosti Fabrikam jsou vybaveny snímači, které monitorují stav motoru během letu.</span><span class="sxs-lookup"><span data-stu-id="5feaa-116">The engines of Fabrikam’s aircraft are instrumented with sensors that monitor engine conditions during flight.</span></span> <span data-ttu-id="5feaa-117">Společnost Fabrikam pomocí řešení prediktivní údržby sbírá data ze snímačů shromážděná během letu.</span><span class="sxs-lookup"><span data-stu-id="5feaa-117">Fabrikam uses the predictive maintenance solution to collect the sensor data collected during the flight.</span></span> <span data-ttu-id="5feaa-118">Z údajů o provozu a selháních motorů nashromážděných za mnoho let analytici společnosti Fabrikam vytvořili model, který předpovídá zbývající dobu životnosti (RUL) leteckého motoru.</span><span class="sxs-lookup"><span data-stu-id="5feaa-118">After accumulating years of engine operational and failure data, Fabrikam’s data scientists have modeled a way to predict the Remaining Useful Life (RUL) of an aircraft engine.</span></span> <span data-ttu-id="5feaa-119">Tento model využívá korelaci mezi daty ze čtyř snímačů v motoru a opotřebením motoru, které vede k jeho případnému selhání.</span><span class="sxs-lookup"><span data-stu-id="5feaa-119">The model uses a correlation between data from four of the engine sensors and engine wear that leads to eventual failure.</span></span> <span data-ttu-id="5feaa-120">I když společnost Fabrikam stále provádí pravidelné kontroly k zajištění bezpečnosti, může pomocí modelu vypočítat zbývající životnost jednotlivých motorů po každém letu.</span><span class="sxs-lookup"><span data-stu-id="5feaa-120">While Fabrikam continues to perform regular inspections to ensure safety, it can now use the models to compute the RUL for each engine after every flight.</span></span> <span data-ttu-id="5feaa-121">Model využívá telemetrická data shromážděná z motorů během letu.</span><span class="sxs-lookup"><span data-stu-id="5feaa-121">The model uses the telemetry collected from the engines during the flight.</span></span> <span data-ttu-id="5feaa-122">Společnost Fabrikam teď může předpovídat budoucí selhání a s předstihem plánovat údržbu a opravy.</span><span class="sxs-lookup"><span data-stu-id="5feaa-122">Fabrikam can now predict future points of failure and plan for maintenance and repair in advance.</span></span>

> [!NOTE]
> <span data-ttu-id="5feaa-123">Model řešení využívá data o opotřebení ze skutečných motorů.</span><span class="sxs-lookup"><span data-stu-id="5feaa-123">The solution model uses actual engine wear data.</span></span>

<span data-ttu-id="5feaa-124">Díky tomu, že společnost Fabrikam dokáže předpovědět čas potřebné údržby, může optimalizovat svůj provoz a snižovat náklady.</span><span class="sxs-lookup"><span data-stu-id="5feaa-124">By predicting the point when maintenance is required, Fabrikam can optimize its operations to reduce costs.</span></span>

<span data-ttu-id="5feaa-125">Koordinátoři údržby spolupracují s plánovači při:</span><span class="sxs-lookup"><span data-stu-id="5feaa-125">Maintenance coordinators work with schedulers to:</span></span>

- <span data-ttu-id="5feaa-126">Plánování údržby tak, aby se časově shodovala se zastávkami letadel v konkrétních místech.</span><span class="sxs-lookup"><span data-stu-id="5feaa-126">Plan maintenance to coincide with an aircraft stopping at a particular location.</span></span>
- <span data-ttu-id="5feaa-127">Zajištění, aby servis letadel nezpůsoboval komplikace v časovém harmonogramu.</span><span class="sxs-lookup"><span data-stu-id="5feaa-127">Ensure sufficient time is available for the aircraft to be out of service without causing schedule disruption.</span></span>
- <span data-ttu-id="5feaa-128">Rozvržení služeb techniků pro zajištění, že servis letadel bude probíhat efektivně a bez prodlev.</span><span class="sxs-lookup"><span data-stu-id="5feaa-128">To schedule technicians to ensure that aircraft are serviced efficiently without wait time.</span></span>

<span data-ttu-id="5feaa-129">Plán údržby dostávají také správci řízení zásob, kteří díky tomu mohou optimalizovat proces objednávání a skladové zásoby náhradních dílů.</span><span class="sxs-lookup"><span data-stu-id="5feaa-129">Inventory control managers receive maintenance plans, so they can optimize their ordering process and spare parts inventory.</span></span>

<span data-ttu-id="5feaa-130">Tyto aktivity umožňují společnosti Fabrikam minimalizovat prostoje letadel a snižovat provozní náklady a současně zajistit bezpečnost cestujících i posádek.</span><span class="sxs-lookup"><span data-stu-id="5feaa-130">These activities enable Fabrikam to minimize aircraft ground time and reduce operating costs while ensuring the safety of passengers and crew.</span></span>

<span data-ttu-id="5feaa-131">Vysvětlení, jak sada [Azure IoT Suite][lnk_iot_suite] zákazníkům umožňuje využít potenciál prediktivní údržby, najdete v této [infografice][lnk_infographic].</span><span class="sxs-lookup"><span data-stu-id="5feaa-131">To understand how [Azure IoT Suite][lnk_iot_suite] provides the capabilities customers need to realize the potential of predictive maintenance, review this [infographic][lnk_infographic].</span></span>

## <a name="how-the-predictive-maintenance-solution-is-built"></a><span data-ttu-id="5feaa-132">Jak je sestaveno řešení prediktivní údržby</span><span class="sxs-lookup"><span data-stu-id="5feaa-132">How the predictive maintenance solution is built</span></span>

<span data-ttu-id="5feaa-133">Řešení využívá existující model Azure Machine Learning, který je k dispozici jako šablona a ukazuje možnosti práce s telemetrickými daty ze zařízení shromážděnými prostřednictvím služeb IoT Suite.</span><span class="sxs-lookup"><span data-stu-id="5feaa-133">The solution uses an existing Azure Machine Learning model available as a template to show these capabilities working from device telemetry collected through IoT Suite services.</span></span> <span data-ttu-id="5feaa-134">Společnost Microsoft vytvořila [regresní model][lnk_regression_model] leteckého motoru na základě veřejně dostupných dat<sup>\[1\]</sup> a podrobné pokyny k použití modelu.</span><span class="sxs-lookup"><span data-stu-id="5feaa-134">Microsoft has built a [regression model][lnk_regression_model] of an aircraft engine based on publically available data<sup>\[1\]</sup>, and step-by-step guidance on how to use the model.</span></span>

<span data-ttu-id="5feaa-135">Řešení prediktivní údržby Azure IoT používá regresní model vytvořený z této šablony.</span><span class="sxs-lookup"><span data-stu-id="5feaa-135">The Azure IoT predictive maintenance solution uses the regression model created from this template.</span></span> <span data-ttu-id="5feaa-136">Model je nasazený do vašeho předplatného Azure a vystavený prostřednictvím automaticky generovaného rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="5feaa-136">The model is deployed into your Azure subscription and exposed through an automatically generated API.</span></span> <span data-ttu-id="5feaa-137">Řešení obsahuje podmnožinu testovacích dat, která představují 4 motory (z celkem 100) a 4 datové proudy ze snímačů (z celkem 21).</span><span class="sxs-lookup"><span data-stu-id="5feaa-137">The solution includes a subset of the testing data representing 4 (of 100 total) engines and the 4 (of 21 total) sensor data streams.</span></span> <span data-ttu-id="5feaa-138">Tato data jsou dostatečná pro poskytování přesných výsledků z trénovaného modelu.</span><span class="sxs-lookup"><span data-stu-id="5feaa-138">This data is sufficient to provide an accurate result from the trained model.</span></span>

<span data-ttu-id="5feaa-139">*\[1\] A. Saxena and K. Goebel (2008). „Turbofan Engine Degradation Simulation Data Set“, datové úložiště NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*</span><span class="sxs-lookup"><span data-stu-id="5feaa-139">*\[1\] A. Saxena and K. Goebel (2008). "Turbofan Engine Degradation Simulation Data Set", NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*</span></span>

## <a name="get-started-with-predictive-maintenance"></a><span data-ttu-id="5feaa-140">Začínáme s prediktivní údržbou</span><span class="sxs-lookup"><span data-stu-id="5feaa-140">Get started with predictive maintenance</span></span>

<span data-ttu-id="5feaa-141">V tomto kurzu se dozvíte, jak zřídit řešení prediktivní údržby.</span><span class="sxs-lookup"><span data-stu-id="5feaa-141">This tutorial shows you how to provision the predictive maintenance solution.</span></span> <span data-ttu-id="5feaa-142">Také se seznámíte se základními funkcemi řešení prediktivní údržby.</span><span class="sxs-lookup"><span data-stu-id="5feaa-142">It also walks you through the basic features of the predictive maintenance solution.</span></span> <span data-ttu-id="5feaa-143">Mnohé z těchto funkcí jsou přístupné prostřednictvím řídicího panelu řešení, který se nasazuje spolu s předkonfigurovaným řešením.</span><span class="sxs-lookup"><span data-stu-id="5feaa-143">You can access many of these features through the solution dashboard that deploys along with the preconfigured solution.</span></span>

<span data-ttu-id="5feaa-144">K dokončení tohoto kurzu potřebujete mít aktivní předplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="5feaa-144">To complete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="5feaa-145">Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební.</span><span class="sxs-lookup"><span data-stu-id="5feaa-145">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="5feaa-146">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="5feaa-146">For details, see [Azure Free Trial][lnk_free_trial].</span></span>

1. <span data-ttu-id="5feaa-147">Pomocí svých přihlašovacích údajů k účtu Azure se přihlaste na webu [azureiotsuite.com][lnk-azureiotsuite] a kliknutím na tlačítko **+** vytvořte řešení.</span><span class="sxs-lookup"><span data-stu-id="5feaa-147">Log on to [azureiotsuite.com][lnk-azureiotsuite] using your Azure account credentials, and click **+** to create a solution.</span></span>
1. <span data-ttu-id="5feaa-148">Klikněte na **Vybrat** na dlaždici **Prediktivní údržba**.</span><span class="sxs-lookup"><span data-stu-id="5feaa-148">Click **Select** the **Predictive maintenance** tile.</span></span>
1. <span data-ttu-id="5feaa-149">Zadejte **Název řešení** pro předkonfigurované řešení prediktivní údržby.</span><span class="sxs-lookup"><span data-stu-id="5feaa-149">Enter a **Solution name** for your predictive maintenance preconfigured solution.</span></span>
1. <span data-ttu-id="5feaa-150">Vyberte **Oblast** a **Předplatné**, které chcete při zřizování řešení použít.</span><span class="sxs-lookup"><span data-stu-id="5feaa-150">Select the **Region** and **Subscription** you want to use to provision the solution.</span></span>
1. <span data-ttu-id="5feaa-151">Kliknutím na tlačítko **Vytvořit řešení** zahájíte proces zřizování.</span><span class="sxs-lookup"><span data-stu-id="5feaa-151">Click **Create Solution** to begin the provisioning process.</span></span> <span data-ttu-id="5feaa-152">Tento proces obvykle trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="5feaa-152">This process typically takes several minutes to run.</span></span>

### <a name="wait-for-the-provisioning-process-to-complete"></a><span data-ttu-id="5feaa-153">Počkejte, dokud proces zřizování neskončí.</span><span class="sxs-lookup"><span data-stu-id="5feaa-153">Wait for the provisioning process to complete</span></span>

1. <span data-ttu-id="5feaa-154">Klikněte na dlaždici s řešením, u kterého je uveden stav **Zřizování**.</span><span class="sxs-lookup"><span data-stu-id="5feaa-154">Click the tile for your solution with **Provisioning** status.</span></span>
1. <span data-ttu-id="5feaa-155">**Stavy zřizování** umožňují sledovat, jak se služby Azure nasazují na váš účet Azure.</span><span class="sxs-lookup"><span data-stu-id="5feaa-155">Notice the **Provisioning states** as Azure services are deployed in your Azure subscription.</span></span>
1. <span data-ttu-id="5feaa-156">Jakmile bude zřizování dokončeno, stav se změní na **Připraveno**.</span><span class="sxs-lookup"><span data-stu-id="5feaa-156">Once provisioning completes, the status changes to **Ready**.</span></span>
1. <span data-ttu-id="5feaa-157">Kliknutím na dlaždici zobrazíte v pravém podokně informace o řešení.</span><span class="sxs-lookup"><span data-stu-id="5feaa-157">Click the tile to see the details of your solution in the right-hand pane.</span></span> <span data-ttu-id="5feaa-158">Z tohoto podokna můžete spustit řídicí panel řešení a využívat přístup k pracovnímu prostoru Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="5feaa-158">From this pane, you can launch the solution dashboard and access the Machine Learning workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="5feaa-159">Pokud při nasazování předkonfigurovaného řešení narazíte na problémy, zkontrolujte [Oprávnění na webu azureiotsuite.com][lnk-permissions] a přečtěte si [Nejčastější dotazy][lnk-faq].</span><span class="sxs-lookup"><span data-stu-id="5feaa-159">If you encounter issues deploying the preconfigured solution, review [Permissions on the azureiotsuite.com site][lnk-permissions] and the [FAQ][lnk-faq].</span></span> <span data-ttu-id="5feaa-160">Pokud problémy přetrvávají, vytvořte na [portálu][lnk-portal] lístek služby.</span><span class="sxs-lookup"><span data-stu-id="5feaa-160">If the issues persist, create a service ticket on the [portal][lnk-portal].</span></span>

<span data-ttu-id="5feaa-161">Hledali jste informace, které se týkají vašeho řešení a nejsou zde uvedeny?</span><span class="sxs-lookup"><span data-stu-id="5feaa-161">Are there details you'd expect to see that aren't listed for your solution?</span></span> <span data-ttu-id="5feaa-162">Sdělte nám návrhy na funkce na webu [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span><span class="sxs-lookup"><span data-stu-id="5feaa-162">Give us feature suggestions on [User Voice](https://feedback.azure.com/forums/321918-azure-iot).</span></span>

## <a name="view-the-solution"></a><span data-ttu-id="5feaa-163">Zobrazení řešení</span><span class="sxs-lookup"><span data-stu-id="5feaa-163">View the solution</span></span>

<span data-ttu-id="5feaa-164">Tato část vás provede uživatelským rozhraním řešení.</span><span class="sxs-lookup"><span data-stu-id="5feaa-164">This section guides you through the solution UI.</span></span>

### <a name="predictive-maintenance-dashboard"></a><span data-ttu-id="5feaa-165">Řídicí panel prediktivní údržby</span><span class="sxs-lookup"><span data-stu-id="5feaa-165">Predictive Maintenance Dashboard</span></span>

<span data-ttu-id="5feaa-166">Tato stránka ve webové aplikaci používá ovládací prvky PowerBI v jazyce JavaScript (viz [Úložiště vizuálních prvků PowerBI][lnk-powerbi]) k vizualizaci:</span><span class="sxs-lookup"><span data-stu-id="5feaa-166">This page in the web application uses PowerBI JavaScript controls (see the [PowerBI-visuals repository][lnk-powerbi]) to visualize:</span></span>

* <span data-ttu-id="5feaa-167">výstupních dat úlohy služby Stream Analytics v úložišti objektů blob</span><span class="sxs-lookup"><span data-stu-id="5feaa-167">The output data from the Stream Analytics jobs in blob storage.</span></span>
* <span data-ttu-id="5feaa-168">zbývající doby životnosti (RUL) a počtu cyklů pro každý motor letadla</span><span class="sxs-lookup"><span data-stu-id="5feaa-168">The RUL and cycle count per aircraft engine.</span></span>

### <a name="observing-the-behavior-of-the-cloud-solution"></a><span data-ttu-id="5feaa-169">Sledování chování cloudového řešení</span><span class="sxs-lookup"><span data-stu-id="5feaa-169">Observing the behavior of the cloud solution</span></span>

<span data-ttu-id="5feaa-170">Na webu Azure Portal přejděte do skupiny prostředků s názvem řešení, které jste si vybrali k zobrazení zřízených prostředků.</span><span class="sxs-lookup"><span data-stu-id="5feaa-170">In the Azure portal, navigate to the resource group with the solution name you chose to view your provisioned resources.</span></span>

![][img-resource-group]

<span data-ttu-id="5feaa-171">Při zřizování předkonfigurovaného řešení obdržíte e-mail s odkazem na pracovní prostor Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="5feaa-171">When you provision the preconfigured solution, you receive an email with a link to the Machine Learning workspace.</span></span> <span data-ttu-id="5feaa-172">Do pracovního prostoru Machine Learning se můžete dostat také ze stránky zřízeného řešení na webu [azureiotsuite.com][lnk-azureiotsuite].</span><span class="sxs-lookup"><span data-stu-id="5feaa-172">You can also navigate to the Machine Learning workspace from the [azureiotsuite.com][lnk-azureiotsuite] page for your provisioned solution.</span></span> <span data-ttu-id="5feaa-173">Na této stránce je k dispozici dlaždice v případě, že je řešení ve stavu **Připraveno**.</span><span class="sxs-lookup"><span data-stu-id="5feaa-173">A tile is available on this page when the solution is in the **Ready** state.</span></span>

![][img-machine-learning]

<span data-ttu-id="5feaa-174">Na portálu řešení uvidíte, že ve vzorovém řešení jsou čtyři simulovaná zařízení, která představují dvě letadla se dvěma motory na každé letadlo, z nichž každý má čtyři snímače.</span><span class="sxs-lookup"><span data-stu-id="5feaa-174">In the solution portal, you can see that the sample is provisioned with four simulated devices to represent two aircraft with two engines per aircraft, each with four sensors.</span></span> <span data-ttu-id="5feaa-175">Při první návštěvě portálu řešení dojde k zastavení simulace.</span><span class="sxs-lookup"><span data-stu-id="5feaa-175">When you first navigate to the solution portal, the simulation is stopped.</span></span>

![][img-simulation-stopped]

<span data-ttu-id="5feaa-176">Kliknutím na **Spustit simulaci** zahajte simulaci.</span><span class="sxs-lookup"><span data-stu-id="5feaa-176">Click **Start simulation** to begin the simulation.</span></span> <span data-ttu-id="5feaa-177">Na řídicím panelu se zobrazí historie hodnot snímačů, zbývající doba životnosti (RUL), počet cyklů a historie hodnot RUL.</span><span class="sxs-lookup"><span data-stu-id="5feaa-177">The sensor history, RUL, Cycles, and RUL history populate the dashboard.</span></span>

![][img-simulation-running]

<span data-ttu-id="5feaa-178">Když je zbývající doba životnosti (RUL) nižší než 160 (libovolná prahová hodnota zvolená pro demonstrační účely), portál řešení zobrazí vedle hodnoty RUL symbol upozornění.</span><span class="sxs-lookup"><span data-stu-id="5feaa-178">When RUL is less than 160 (an arbitrary threshold chosen for demonstration purposes), the solution portal displays a warning symbol next to the RUL display.</span></span> <span data-ttu-id="5feaa-179">Portál řešení také zvýrazní daný motor letadla žlutou barvou.</span><span class="sxs-lookup"><span data-stu-id="5feaa-179">The solution portal also highlights the aircraft engine in yellow.</span></span> <span data-ttu-id="5feaa-180">Můžete si všimnout, že hodnoty zbývající doby životnosti (RUL) mají obecné klesající trend, ale kolísají nahoru a dolů.</span><span class="sxs-lookup"><span data-stu-id="5feaa-180">Notice how the RUL values have a general downward trend overall, but tend to bounce up and down.</span></span> <span data-ttu-id="5feaa-181">Toto chování vyplývá z různých délek cyklu a přesnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="5feaa-181">This behavior results from the varying cycle lengths and the model accuracy.</span></span>

![][img-simulation-warning]

<span data-ttu-id="5feaa-182">Při úplné simulaci trvá dokončení 148 cyklů asi 35 minut.</span><span class="sxs-lookup"><span data-stu-id="5feaa-182">The full simulation takes around 35 minutes to complete 148 cycles.</span></span> <span data-ttu-id="5feaa-183">Prahová hodnota 160 pro zbývající dobu životnosti (RUL) je poprvé dosažena přibližně po 5 minutách simulace a oba motory se na tuto hodnotu dostanou přibližně po 8 minutách.</span><span class="sxs-lookup"><span data-stu-id="5feaa-183">The 160 RUL threshold is met for the first time at around 5 minutes and both engines hit the threshold at around 8 minutes.</span></span>

<span data-ttu-id="5feaa-184">Simulace zpracuje úplnou datovou sadu s údaji o 148 cyklech a vytvoří konečnou hodnotu RUL a cyklů.</span><span class="sxs-lookup"><span data-stu-id="5feaa-184">The simulation runs through the complete dataset for 148 cycles and settles on final RUL and cycle values.</span></span>

<span data-ttu-id="5feaa-185">Simulaci lze zastavit v libovolný okamžik, ale kliknutím na tlačítko **Start simulace** spustíte simulaci znovu od začátku datové sady.</span><span class="sxs-lookup"><span data-stu-id="5feaa-185">You can stop the simulation at any point, but clicking **Start Simulation** replays the simulation from the start of the dataset.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5feaa-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5feaa-186">Next steps</span></span>

<span data-ttu-id="5feaa-187">Další informace o tom, jak Azure IoT podporuje scénáře prediktivní údržby, najdete v tématu [Získání hodnoty z Internetu věcí][lnk_capture_value].</span><span class="sxs-lookup"><span data-stu-id="5feaa-187">To learn more about how Azure IoT enables predictive maintenance scenarios, read [Capture value from the Internet of Things][lnk_capture_value].</span></span>

<span data-ttu-id="5feaa-188">[Podrobný návod][lnk-predictive-walkthrough] pro řešení prediktivní údržby.</span><span class="sxs-lookup"><span data-stu-id="5feaa-188">Take a [walkthrough][lnk-predictive-walkthrough] of the predictive maintenance solution.</span></span>

<span data-ttu-id="5feaa-189">Můžete si taky prostudovat některé další funkce a možnosti předkonfigurovaných řešení sady IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="5feaa-189">You can also explore some of the other features and capabilities of the IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="5feaa-190">[Nejčastější dotazy k sadě IoT Suite][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="5feaa-190">[Frequently asked questions for IoT Suite][lnk-faq]</span></span>
* <span data-ttu-id="5feaa-191">[Zabezpečení IoT od počátku][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="5feaa-191">[IoT security from the ground up][lnk-security-groundup]</span></span>

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