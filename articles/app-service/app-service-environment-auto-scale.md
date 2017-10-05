---
title: "Automatické škálování a služby App Service Environment v1"
description: "Automatické škálování a služby App Service Environment"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: c23af2d8-d370-4b1f-9b3e-8782321ddccb
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: f32affd285f3918feb0e893543f2a28f678b7b10
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="autoscaling-and-app-service-environment-v1"></a><span data-ttu-id="bf2d3-103">Automatické škálování a služby App Service Environment v1</span><span class="sxs-lookup"><span data-stu-id="bf2d3-103">Autoscaling and App Service Environment v1</span></span>

> [!NOTE]
> <span data-ttu-id="bf2d3-104">Tento článek je o v1 App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-104">This article is about the App Service Environment v1.</span></span>  <span data-ttu-id="bf2d3-105">Existuje novější verze App Service Environment, který je jednodušší použít a běží na výkonnější infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-105">There is a newer version of the App Service Environment that is easier  to use and runs on more powerful infrastructure.</span></span> <span data-ttu-id="bf2d3-106">Další informace o nové verzi spuštění s [Úvod do služby App Service Environment](../app-service/app-service-environment/intro.md).</span><span class="sxs-lookup"><span data-stu-id="bf2d3-106">To learn more about the new version start with the [Introduction to the App  Service Environment](../app-service/app-service-environment/intro.md).</span></span>
> 

<span data-ttu-id="bf2d3-107">Podpora prostředí Azure App Service *automatické škálování*.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-107">Azure App Service environments support *autoscaling*.</span></span> <span data-ttu-id="bf2d3-108">Můžete fondy škálování jednotlivých pracovních procesů na základě metriky nebo plán.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-108">You can autoscale individual worker pools based on metrics or schedule.</span></span>

![Možnosti škálování fondu pracovních procesů.][intro]

<span data-ttu-id="bf2d3-110">Automatické škálování optimalizuje vaše využití prostředků pomocí automatické zvětšování a zmenšování služby App Service environment přizpůsobit rozpočet nebo načíst profil.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-110">Autoscaling optimizes your resource utilization by automatically growing and shrinking an App Service environment to fit your budget and or load profile.</span></span>

## <a name="configure-worker-pool-autoscale"></a><span data-ttu-id="bf2d3-111">Konfigurace pracovního procesu škálování fondu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-111">Configure worker pool autoscale</span></span>
<span data-ttu-id="bf2d3-112">Může přistupovat k funkcím škálování z **nastavení** ve fondu pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-112">You can access the autoscale functionality from the **Settings** tab of the worker pool.</span></span>

![Karta nastavení fondu pracovních procesů.][settings-scale]

<span data-ttu-id="bf2d3-114">Z tohoto místa rozhraní by měl být poměrně obeznámeni, protože je stejné prostředí, které vidíte, kdy škálovat plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-114">From there, the interface should be fairly familiar since it is the same experience that you see when you scale an App Service plan.</span></span> 

![Nastavení ruční škálování.][scale-manual]

<span data-ttu-id="bf2d3-116">Můžete také nakonfigurovat profilem škálování.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-116">You can also configure an autoscale profile.</span></span>

![Nastavení automatického škálování.][scale-profile]

<span data-ttu-id="bf2d3-118">Profilů automatického škálování jsou užitečné můžete nastavit omezení pro vaše škálování.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-118">Autoscale profiles are useful to set limits on your scale.</span></span> <span data-ttu-id="bf2d3-119">Tímto způsobem může mít konzistentní výkon prostředí nastavením hodnotu dolní mez rozsahu (1) a předvídatelný výdaji cap nastavením horní mez (2).</span><span class="sxs-lookup"><span data-stu-id="bf2d3-119">This way, you can have a consistent performance experience by setting a lower bound scale value (1) and a predictable spend cap by setting an upper bound (2).</span></span>

![Nastavení škálování v profilu.][scale-profile2]

<span data-ttu-id="bf2d3-121">Po definování profilu, můžete přidat pravidla automatického škálování se škálovat nahoru nebo dolů počet instancí ve fondu pracovních procesů v rámci hranice definované profilem.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-121">After you define a profile, you can add autoscale rules to scale up or down the number of instances in the worker pool within the bounds defined by the profile.</span></span> <span data-ttu-id="bf2d3-122">Pravidla automatického škálování jsou založené na metriky.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-122">Autoscale rules are based on metrics.</span></span>

![Pravidlo škálování.][scale-rule]

 <span data-ttu-id="bf2d3-124">Všechny fondu pracovních procesů nebo front-end metriky lze použít k definování pravidel škálování.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-124">Any worker pool or front-end metrics can be used to define autoscale rules.</span></span> <span data-ttu-id="bf2d3-125">Tyto metriky jsou stejné metriky můžete sledovat v okně grafy zdrojů nebo nastavit výstrahy pro.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-125">These metrics are the same metrics you can monitor in the resource blade graphs or set alerts for.</span></span>

## <a name="autoscale-example"></a><span data-ttu-id="bf2d3-126">Příklad škálování</span><span class="sxs-lookup"><span data-stu-id="bf2d3-126">Autoscale example</span></span>
<span data-ttu-id="bf2d3-127">Škálování služby App Service environment lze ukázat nejlépe ve scénáři s návodem.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-127">Autoscale of an App Service environment can best be illustrated by walking through a scenario.</span></span>

<span data-ttu-id="bf2d3-128">Tento článek vysvětluje všechny nezbytné informace při nastavení automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-128">This article explains all the necessary considerations when you set up autoscale.</span></span> <span data-ttu-id="bf2d3-129">Článek vás provede procesem interakce, které se vyskytnou se, když je zvážit automatické škálování služby App Service Environment, které jsou hostované v App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-129">The article walks you through the interactions that come into play when you factor in autoscaling App Service environments that are hosted in App Service Environment.</span></span>

### <a name="scenario-introduction"></a><span data-ttu-id="bf2d3-130">Scénář Úvod</span><span class="sxs-lookup"><span data-stu-id="bf2d3-130">Scenario introduction</span></span>
<span data-ttu-id="bf2d3-131">František se správce systému pro organizace, která byla migrována část zátěží, které spravuje do služby App Service environment.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-131">Frank is a sysadmin for an enterprise who has migrated a portion of the workloads that he manages to an App Service environment.</span></span>

<span data-ttu-id="bf2d3-132">Prostředí služby App Service je ruční škálování nakonfigurovány takto:</span><span class="sxs-lookup"><span data-stu-id="bf2d3-132">The App Service environment is configured to manual scale as follows:</span></span>

* <span data-ttu-id="bf2d3-133">**Front-zakončení:** 3</span><span class="sxs-lookup"><span data-stu-id="bf2d3-133">**Front ends:** 3</span></span>
* <span data-ttu-id="bf2d3-134">**Fond pracovních procesů 1**: 10</span><span class="sxs-lookup"><span data-stu-id="bf2d3-134">**Worker pool 1**: 10</span></span>
* <span data-ttu-id="bf2d3-135">**Fond pracovních procesů 2**: 5</span><span class="sxs-lookup"><span data-stu-id="bf2d3-135">**Worker pool 2**: 5</span></span>
* <span data-ttu-id="bf2d3-136">**Fond pracovních procesů 3**: 5</span><span class="sxs-lookup"><span data-stu-id="bf2d3-136">**Worker pool 3**: 5</span></span>

<span data-ttu-id="bf2d3-137">Fond pracovních procesů 1 se používá pro úlohy v produkčním prostředí při fondu pracovních procesů 2 a fondu pracovních procesů 3 se používají pro zajištění kvality (QA) a vývoj úlohy.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-137">Worker pool 1 is used for production workloads, while worker pool 2 and worker pool 3 are used for quality assurance (QA) and development workloads.</span></span>

<span data-ttu-id="bf2d3-138">Plány služby App Service pro kontrolu kvality a vývojářů je nakonfigurován pro ruční škálování.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-138">The App Service plans for QA and dev are configured to manual scale.</span></span> <span data-ttu-id="bf2d3-139">Provozní plán služby App Service je nastavena na automatické škálování jak nakládat s rozdíly v zatížení a provozu.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-139">The production App Service plan is set to autoscale to deal with variations in load and traffic.</span></span>

<span data-ttu-id="bf2d3-140">Jan je velmi dobře známé s aplikací.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-140">Frank is very familiar with the application.</span></span> <span data-ttu-id="bf2d3-141">Ví, že špičky pro zatížení jsou mezi 9:00:00 a 18:00:00, protože to je – obchodní (LOB) aplikace, která zaměstnanci používají době, kdy jsou v kanceláři v.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-141">He knows that the peak hours for load are between 9:00 AM and 6:00 PM because this is a line-of-business (LOB) application that employees use while they are in the office.</span></span> <span data-ttu-id="bf2d3-142">Využití zahodí po, když se uživatelé provádějí pro daný den.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-142">Usage drops after that when users are done for that day.</span></span> <span data-ttu-id="bf2d3-143">Mimo dobu ve špičce přetrvává některé zatížení vzhledem k tomu, že uživatelé mají přístup ke aplikaci vzdáleně pomocí jejich mobilní zařízení nebo domácích počítačů.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-143">Outside peak hours, there is still some load because users can access the app remotely by using their mobile devices or home PCs.</span></span> <span data-ttu-id="bf2d3-144">Provozní plán služby App Service je již nakonfigurována na automatické škálování podle využití procesoru na základě následujících pravidel:</span><span class="sxs-lookup"><span data-stu-id="bf2d3-144">The production App Service plan is already configured to autoscale based on CPU usage with the following rules:</span></span>

![Konkrétní nastavení pro obchodní aplikace.][asp-scale]

| <span data-ttu-id="bf2d3-146">**Škálování profil – dny v týdnu – plán služby App Service**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-146">**Autoscale profile – Weekdays – App Service plan**</span></span> | <span data-ttu-id="bf2d3-147">**Škálování profil – víkendů – plán služby App Service**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-147">**Autoscale profile – Weekends – App Service plan**</span></span> |
| --- | --- |
| <span data-ttu-id="bf2d3-148">**Název:** profil den v týdnu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-148">**Name:** Weekday profile</span></span> |<span data-ttu-id="bf2d3-149">**Název:** víkendu profilu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-149">**Name:** Weekend profile</span></span> |
| <span data-ttu-id="bf2d3-150">**Škálování podle:** pravidla plánu a výkonu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-150">**Scale by:** Schedule and performance rules</span></span> |<span data-ttu-id="bf2d3-151">**Škálování podle:** pravidla plánu a výkonu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-151">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="bf2d3-152">**Profil:** dny v týdnu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-152">**Profile:** Weekdays</span></span> |<span data-ttu-id="bf2d3-153">**Profil:** víkendu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-153">**Profile:** Weekend</span></span> |
| <span data-ttu-id="bf2d3-154">**Typ:** opakování</span><span class="sxs-lookup"><span data-stu-id="bf2d3-154">**Type:** Recurrence</span></span> |<span data-ttu-id="bf2d3-155">**Typ:** opakování</span><span class="sxs-lookup"><span data-stu-id="bf2d3-155">**Type:** Recurrence</span></span> |
| <span data-ttu-id="bf2d3-156">**Cílový rozsah:** 5 až 20 instancí</span><span class="sxs-lookup"><span data-stu-id="bf2d3-156">**Target range:** 5 to 20 instances</span></span> |<span data-ttu-id="bf2d3-157">**Cílový rozsah:** 3 až 10 instancí</span><span class="sxs-lookup"><span data-stu-id="bf2d3-157">**Target range:** 3 to 10 instances</span></span> |
| <span data-ttu-id="bf2d3-158">**Počet dnů:** pondělí, úterý, středu, čtvrtek a pátek</span><span class="sxs-lookup"><span data-stu-id="bf2d3-158">**Days:** Monday, Tuesday, Wednesday, Thursday, Friday</span></span> |<span data-ttu-id="bf2d3-159">**Počet dnů:** sobota, neděle</span><span class="sxs-lookup"><span data-stu-id="bf2d3-159">**Days:** Saturday, Sunday</span></span> |
| <span data-ttu-id="bf2d3-160">**Čas spuštění:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="bf2d3-160">**Start time:** 9:00 AM</span></span> |<span data-ttu-id="bf2d3-161">**Čas spuštění:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="bf2d3-161">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="bf2d3-162">**Časové pásmo:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="bf2d3-162">**Time zone:** UTC-08</span></span> |<span data-ttu-id="bf2d3-163">**Časové pásmo:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="bf2d3-163">**Time zone:** UTC-08</span></span> |
|  | |
| <span data-ttu-id="bf2d3-164">**Pravidlo automatického škálování (vertikálně navýšit kapacitu)**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-164">**Autoscale rule (Scale Up)**</span></span> |<span data-ttu-id="bf2d3-165">**Pravidlo automatického škálování (vertikálně navýšit kapacitu)**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-165">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="bf2d3-166">**Prostředek:** produkční (služby App Service Environment)</span><span class="sxs-lookup"><span data-stu-id="bf2d3-166">**Resource:** Production (App Service Environment)</span></span> |<span data-ttu-id="bf2d3-167">**Prostředek:** produkční (služby App Service Environment)</span><span class="sxs-lookup"><span data-stu-id="bf2d3-167">**Resource:** Production (App Service Environment)</span></span> |
| <span data-ttu-id="bf2d3-168">**Metrika:** % využití procesoru</span><span class="sxs-lookup"><span data-stu-id="bf2d3-168">**Metric:** CPU %</span></span> |<span data-ttu-id="bf2d3-169">**Metrika:** % využití procesoru</span><span class="sxs-lookup"><span data-stu-id="bf2d3-169">**Metric:** CPU %</span></span> |
| <span data-ttu-id="bf2d3-170">**Operace:** větší než 60 %</span><span class="sxs-lookup"><span data-stu-id="bf2d3-170">**Operation:** Greater than 60%</span></span> |<span data-ttu-id="bf2d3-171">**Operace:** větší než 80 %</span><span class="sxs-lookup"><span data-stu-id="bf2d3-171">**Operation:** Greater than 80%</span></span> |
| <span data-ttu-id="bf2d3-172">**Doba trvání:** 5 minut</span><span class="sxs-lookup"><span data-stu-id="bf2d3-172">**Duration:** 5 Minutes</span></span> |<span data-ttu-id="bf2d3-173">**Doba trvání:** 10 minut</span><span class="sxs-lookup"><span data-stu-id="bf2d3-173">**Duration:** 10 Minutes</span></span> |
| <span data-ttu-id="bf2d3-174">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="bf2d3-174">**Time aggregation:** Average</span></span> |<span data-ttu-id="bf2d3-175">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="bf2d3-175">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="bf2d3-176">**Akce:** zvýšit počet 2</span><span class="sxs-lookup"><span data-stu-id="bf2d3-176">**Action:** Increase count by 2</span></span> |<span data-ttu-id="bf2d3-177">**Akce:** zvýšit počet 1</span><span class="sxs-lookup"><span data-stu-id="bf2d3-177">**Action:** Increase count by 1</span></span> |
| <span data-ttu-id="bf2d3-178">**Nástrojů dolů (minuty):** 15</span><span class="sxs-lookup"><span data-stu-id="bf2d3-178">**Cool down (minutes):** 15</span></span> |<span data-ttu-id="bf2d3-179">**Nástrojů dolů (minuty):** 20</span><span class="sxs-lookup"><span data-stu-id="bf2d3-179">**Cool down (minutes):** 20</span></span> |
|  | |
| <span data-ttu-id="bf2d3-180">**Pravidlo automatického škálování (škálování dolů)**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-180">**Autoscale rule (Scale Down)**</span></span> |<span data-ttu-id="bf2d3-181">**Pravidlo automatického škálování (škálování dolů)**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-181">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="bf2d3-182">**Prostředek:** produkční (služby App Service Environment)</span><span class="sxs-lookup"><span data-stu-id="bf2d3-182">**Resource:** Production (App Service Environment)</span></span> |<span data-ttu-id="bf2d3-183">**Prostředek:** produkční (služby App Service Environment)</span><span class="sxs-lookup"><span data-stu-id="bf2d3-183">**Resource:** Production (App Service Environment)</span></span> |
| <span data-ttu-id="bf2d3-184">**Metrika:** % využití procesoru</span><span class="sxs-lookup"><span data-stu-id="bf2d3-184">**Metric:** CPU %</span></span> |<span data-ttu-id="bf2d3-185">**Metrika:** % využití procesoru</span><span class="sxs-lookup"><span data-stu-id="bf2d3-185">**Metric:** CPU %</span></span> |
| <span data-ttu-id="bf2d3-186">**Operace:** méně než 30 %</span><span class="sxs-lookup"><span data-stu-id="bf2d3-186">**Operation:** Less than 30%</span></span> |<span data-ttu-id="bf2d3-187">**Operace:** méně než 20 %</span><span class="sxs-lookup"><span data-stu-id="bf2d3-187">**Operation:** Less than 20%</span></span> |
| <span data-ttu-id="bf2d3-188">**Doba trvání:** 10 minut</span><span class="sxs-lookup"><span data-stu-id="bf2d3-188">**Duration:** 10 minutes</span></span> |<span data-ttu-id="bf2d3-189">**Doba trvání:** 15 minut</span><span class="sxs-lookup"><span data-stu-id="bf2d3-189">**Duration:** 15 minutes</span></span> |
| <span data-ttu-id="bf2d3-190">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="bf2d3-190">**Time aggregation:** Average</span></span> |<span data-ttu-id="bf2d3-191">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="bf2d3-191">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="bf2d3-192">**Akce:** snižte počet 1</span><span class="sxs-lookup"><span data-stu-id="bf2d3-192">**Action:** Decrease count by 1</span></span> |<span data-ttu-id="bf2d3-193">**Akce:** snižte počet 1</span><span class="sxs-lookup"><span data-stu-id="bf2d3-193">**Action:** Decrease count by 1</span></span> |
| <span data-ttu-id="bf2d3-194">**Nástrojů dolů (minuty):** 20</span><span class="sxs-lookup"><span data-stu-id="bf2d3-194">**Cool down (minutes):** 20</span></span> |<span data-ttu-id="bf2d3-195">**Nástrojů dolů (minuty):** 10</span><span class="sxs-lookup"><span data-stu-id="bf2d3-195">**Cool down (minutes):** 10</span></span> |

### <a name="app-service-plan-inflation-rate"></a><span data-ttu-id="bf2d3-196">Míry inflace plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="bf2d3-196">App Service plan inflation rate</span></span>
<span data-ttu-id="bf2d3-197">Plány služby App Service, které jsou nakonfigurované na automatické škálování to provést s maximální rychlostí za hodinu.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-197">App Service plans that are configured to autoscale do so at a maximum rate per hour.</span></span> <span data-ttu-id="bf2d3-198">Tento kurz, lze vypočítat na základě hodnot zadaných pro pravidlo škálování.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-198">This rate can be calculated based on the values provided on the autoscale rule.</span></span>

<span data-ttu-id="bf2d3-199">Princip fungování a způsob výpočtu *míry inflace plán služby App Service* je důležité pro škálování prostředí služby App Service, protože nejsou okamžitou škálování změny fondu pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-199">Understanding and calculating the *App Service plan inflation rate* is important for App Service environment autoscale because scale changes to a worker pool are not instantaneous.</span></span>

<span data-ttu-id="bf2d3-200">Míry inflace plán služby App Service se vypočítává takto:</span><span class="sxs-lookup"><span data-stu-id="bf2d3-200">The App Service plan inflation rate is calculated as follows:</span></span>

![Výpočet míry inflace plán služby App Service.][ASP-Inflation]

<span data-ttu-id="bf2d3-202">Podle škálování – pravidlo vertikálně navýšit kapacitu pro profil den v týdnu provozních plán služby App Service:</span><span class="sxs-lookup"><span data-stu-id="bf2d3-202">Based on the Autoscale – Scale Up rule for the Weekday profile of the production App Service plan:</span></span>

![Míra inflace plán služby App Service pro dny v týdnu podle škálování – pravidlo vertikálně navýšit kapacitu.][Equation1]

<span data-ttu-id="bf2d3-204">V případě škálování – pravidlo vertikálně navýšit kapacitu pro profil víkendu provozních plán služby App Service, by vzorec odkazující na:</span><span class="sxs-lookup"><span data-stu-id="bf2d3-204">In the case of the Autoscale – Scale Up rule for the Weekend profile of the production App Service plan, the formula would resolve to:</span></span>

![Míry inflace plán služby App Service pro víkendů podle škálování – pravidlo vertikálně navýšit kapacitu.][Equation2]

<span data-ttu-id="bf2d3-206">Tato hodnota může také vypočítá operacím vertikální snížení kapacity.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-206">This value can also be calculated for scale-down operations.</span></span>

<span data-ttu-id="bf2d3-207">Podle škálování – pravidlo škálování dolů profilu den v týdnu provozních plán služby App Service, to vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="bf2d3-207">Based on the Autoscale – Scale Down rule for the Weekday profile of the production App Service plan, this would look as follows:</span></span>

![Míra inflace plán služby App Service pro dny v týdnu podle škálování – pravidlo škálování dolů.][Equation3]

<span data-ttu-id="bf2d3-209">V případě škálování – pravidlo škálování dolů profilu víkendu provozních plán služby App Service, by vzorec odkazující na:</span><span class="sxs-lookup"><span data-stu-id="bf2d3-209">In the case of the Autoscale – Scale Down rule for the Weekend profile of the production App Service plan, the formula would resolve to:</span></span>  

![Rychlost inflace plán služby App Service pro víkendů podle škálování – pravidlo škálování dolů.][Equation4]

<span data-ttu-id="bf2d3-211">Provozní plán služby App Service můžou růst v maximálně osm instancí za hodinu v týdnu a čtyři instance za hodinu během víkendu.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-211">The production App Service plan can grow at a maximum rate of eight instances/hour during the week and four instances/hour during the weekend.</span></span> <span data-ttu-id="bf2d3-212">Můžete ho verzi instance s maximální rychlostí čtyři instancí za hodinu v týdnu a šesti instancí za hodinu během víkendu.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-212">It can release instances at a maximum rate of four instances/hour during the week and six instances/hour during weekends.</span></span>

<span data-ttu-id="bf2d3-213">Pokud víc plány služby App Service se hostovaným ve fondu pracovních procesů, je nutné vypočítat *celkovou rychlost inflace* jako součet inflace rychlost pro všechny plány služby App Service, které se hostování v tomto fondu pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-213">If multiple App Service plans are being hosted in a worker pool, you have to calculate the *total inflation rate* as the sum of the inflation rate for all the App Service plans that are being hosting in that worker pool.</span></span>

![Celková rychlost inflace výpočet pro víc plány služby App Service hostované ve fondu pracovních procesů.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a><span data-ttu-id="bf2d3-215">Použijte k definování pravidel škálování fondu pracovní míry inflace plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="bf2d3-215">Use the App Service plan inflation rate to define worker pool autoscale rules</span></span>
<span data-ttu-id="bf2d3-216">Pracovní fondu tohoto hostitele plány služby App Service, které jsou nakonfigurované pro škálování nutné přidělit vyrovnávací paměť kapacity.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-216">Worker pools that host App Service plans that are configured to autoscale need to be allocated a buffer of capacity.</span></span> <span data-ttu-id="bf2d3-217">Vyrovnávací paměť umožňuje operace škálování, které mají zvýšit nebo snížit plán služby App Service, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-217">The buffer allows for the autoscale operations to grow and shrink the App Service plan as needed.</span></span> <span data-ttu-id="bf2d3-218">Minimální velikost vyrovnávací paměti by počítané celková aplikace služby plánování inflace rychlost.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-218">The minimum buffer would be the calculated Total App Service Plan Inflation Rate.</span></span>

<span data-ttu-id="bf2d3-219">Protože operací škálování služby App Service environment trvat delší dobu použít, všechny změny by měl účet pro vyžádání další změny, které může nastat, když probíhá operace škálování.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-219">Because App Service environment scale operations take some time to apply, any change should account for further demand changes that could happen while a scale operation is in progress.</span></span> <span data-ttu-id="bf2d3-220">Abychom vyhověli tato čekací doba, doporučujeme použít počítaný celková aplikace služby plánování inflace rychlost jako minimální počet instancí, které jsou přidány pro každou operaci škálování.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-220">To accommodate this latency, we recommend that you use the calculated Total App Service Plan Inflation Rate as the minimum number of instances that are added for each autoscale operation.</span></span>

<span data-ttu-id="bf2d3-221">Tyto informace můžete definovat František následující škálování profil a pravidla:</span><span class="sxs-lookup"><span data-stu-id="bf2d3-221">With this information, Frank can define the following autoscale profile and rules:</span></span>

![Profil pravidel škálování například LOB.][Worker-Pool-Scale]

| <span data-ttu-id="bf2d3-223">**Škálování profil – dny v týdnu**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-223">**Autoscale profile – Weekdays**</span></span> | <span data-ttu-id="bf2d3-224">**Škálování profil – víkendů**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-224">**Autoscale profile – Weekends**</span></span> |
| --- | --- |
| <span data-ttu-id="bf2d3-225">**Název:** profil den v týdnu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-225">**Name:** Weekday profile</span></span> |<span data-ttu-id="bf2d3-226">**Název:** víkendu profilu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-226">**Name:** Weekend profile</span></span> |
| <span data-ttu-id="bf2d3-227">**Škálování podle:** pravidla plánu a výkonu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-227">**Scale by:** Schedule and performance rules</span></span> |<span data-ttu-id="bf2d3-228">**Škálování podle:** pravidla plánu a výkonu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-228">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="bf2d3-229">**Profil:** dny v týdnu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-229">**Profile:** Weekdays</span></span> |<span data-ttu-id="bf2d3-230">**Profil:** víkendu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-230">**Profile:** Weekend</span></span> |
| <span data-ttu-id="bf2d3-231">**Typ:** opakování</span><span class="sxs-lookup"><span data-stu-id="bf2d3-231">**Type:** Recurrence</span></span> |<span data-ttu-id="bf2d3-232">**Typ:** opakování</span><span class="sxs-lookup"><span data-stu-id="bf2d3-232">**Type:** Recurrence</span></span> |
| <span data-ttu-id="bf2d3-233">**Cílový rozsah:** 13 až 25 instancí</span><span class="sxs-lookup"><span data-stu-id="bf2d3-233">**Target range:** 13 to 25 instances</span></span> |<span data-ttu-id="bf2d3-234">**Cílový rozsah:** 6 až 15 instancí</span><span class="sxs-lookup"><span data-stu-id="bf2d3-234">**Target range:** 6 to 15 instances</span></span> |
| <span data-ttu-id="bf2d3-235">**Počet dnů:** pondělí, úterý, středu, čtvrtek a pátek</span><span class="sxs-lookup"><span data-stu-id="bf2d3-235">**Days:** Monday, Tuesday, Wednesday, Thursday, Friday</span></span> |<span data-ttu-id="bf2d3-236">**Počet dnů:** sobota, neděle</span><span class="sxs-lookup"><span data-stu-id="bf2d3-236">**Days:** Saturday, Sunday</span></span> |
| <span data-ttu-id="bf2d3-237">**Čas spuštění:** 7:00:00</span><span class="sxs-lookup"><span data-stu-id="bf2d3-237">**Start time:** 7:00 AM</span></span> |<span data-ttu-id="bf2d3-238">**Čas spuštění:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="bf2d3-238">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="bf2d3-239">**Časové pásmo:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="bf2d3-239">**Time zone:** UTC-08</span></span> |<span data-ttu-id="bf2d3-240">**Časové pásmo:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="bf2d3-240">**Time zone:** UTC-08</span></span> |
|  | |
| <span data-ttu-id="bf2d3-241">**Pravidlo automatického škálování (vertikálně navýšit kapacitu)**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-241">**Autoscale rule (Scale Up)**</span></span> |<span data-ttu-id="bf2d3-242">**Pravidlo automatického škálování (vertikálně navýšit kapacitu)**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-242">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="bf2d3-243">**Prostředek:** pracovní fond 1</span><span class="sxs-lookup"><span data-stu-id="bf2d3-243">**Resource:** Worker pool 1</span></span> |<span data-ttu-id="bf2d3-244">**Prostředek:** pracovní fond 1</span><span class="sxs-lookup"><span data-stu-id="bf2d3-244">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="bf2d3-245">**Metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="bf2d3-245">**Metric:** WorkersAvailable</span></span> |<span data-ttu-id="bf2d3-246">**Metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="bf2d3-246">**Metric:** WorkersAvailable</span></span> |
| <span data-ttu-id="bf2d3-247">**Operace:** menší než 8</span><span class="sxs-lookup"><span data-stu-id="bf2d3-247">**Operation:** Less than 8</span></span> |<span data-ttu-id="bf2d3-248">**Operace:** menší než 3</span><span class="sxs-lookup"><span data-stu-id="bf2d3-248">**Operation:** Less than 3</span></span> |
| <span data-ttu-id="bf2d3-249">**Doba trvání:** 20 minut</span><span class="sxs-lookup"><span data-stu-id="bf2d3-249">**Duration:** 20 minutes</span></span> |<span data-ttu-id="bf2d3-250">**Doba trvání:** 30 minut</span><span class="sxs-lookup"><span data-stu-id="bf2d3-250">**Duration:** 30 minutes</span></span> |
| <span data-ttu-id="bf2d3-251">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="bf2d3-251">**Time aggregation:** Average</span></span> |<span data-ttu-id="bf2d3-252">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="bf2d3-252">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="bf2d3-253">**Akce:** zvýšit počet 8</span><span class="sxs-lookup"><span data-stu-id="bf2d3-253">**Action:** Increase count by 8</span></span> |<span data-ttu-id="bf2d3-254">**Akce:** zvýšení počtu 3</span><span class="sxs-lookup"><span data-stu-id="bf2d3-254">**Action:** Increase count by 3</span></span> |
| <span data-ttu-id="bf2d3-255">**Nástrojů dolů (minuty):** 180</span><span class="sxs-lookup"><span data-stu-id="bf2d3-255">**Cool down (minutes):** 180</span></span> |<span data-ttu-id="bf2d3-256">**Nástrojů dolů (minuty):** 180</span><span class="sxs-lookup"><span data-stu-id="bf2d3-256">**Cool down (minutes):** 180</span></span> |
|  | |
| <span data-ttu-id="bf2d3-257">**Pravidlo automatického škálování (škálování dolů)**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-257">**Autoscale rule (Scale Down)**</span></span> |<span data-ttu-id="bf2d3-258">**Pravidlo automatického škálování (škálování dolů)**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-258">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="bf2d3-259">**Prostředek:** pracovní fond 1</span><span class="sxs-lookup"><span data-stu-id="bf2d3-259">**Resource:** Worker pool 1</span></span> |<span data-ttu-id="bf2d3-260">**Prostředek:** pracovní fond 1</span><span class="sxs-lookup"><span data-stu-id="bf2d3-260">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="bf2d3-261">**Metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="bf2d3-261">**Metric:** WorkersAvailable</span></span> |<span data-ttu-id="bf2d3-262">**Metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="bf2d3-262">**Metric:** WorkersAvailable</span></span> |
| <span data-ttu-id="bf2d3-263">**Operace:** větší než 8</span><span class="sxs-lookup"><span data-stu-id="bf2d3-263">**Operation:** Greater than 8</span></span> |<span data-ttu-id="bf2d3-264">**Operace:** větší než 3</span><span class="sxs-lookup"><span data-stu-id="bf2d3-264">**Operation:** Greater than 3</span></span> |
| <span data-ttu-id="bf2d3-265">**Doba trvání:** 20 minut</span><span class="sxs-lookup"><span data-stu-id="bf2d3-265">**Duration:** 20 minutes</span></span> |<span data-ttu-id="bf2d3-266">**Doba trvání:** 15 minut</span><span class="sxs-lookup"><span data-stu-id="bf2d3-266">**Duration:** 15 minutes</span></span> |
| <span data-ttu-id="bf2d3-267">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="bf2d3-267">**Time aggregation:** Average</span></span> |<span data-ttu-id="bf2d3-268">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="bf2d3-268">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="bf2d3-269">**Akce:** snižte počet 2</span><span class="sxs-lookup"><span data-stu-id="bf2d3-269">**Action:** Decrease count by 2</span></span> |<span data-ttu-id="bf2d3-270">**Akce:** snížit počet 3</span><span class="sxs-lookup"><span data-stu-id="bf2d3-270">**Action:** Decrease count by 3</span></span> |
| <span data-ttu-id="bf2d3-271">**Nástrojů dolů (minuty):** 120.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-271">**Cool down (minutes):** 120</span></span> |<span data-ttu-id="bf2d3-272">**Nástrojů dolů (minuty):** 120.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-272">**Cool down (minutes):** 120</span></span> |

<span data-ttu-id="bf2d3-273">Minimální instance definované v profilu pro plán služby App Service + vyrovnávací paměti je vypočítána cílový rozsah definovaný v profilu.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-273">The Target range defined in the profile is calculated by the minimum instances defined in the profile for the App Service plan + buffer.</span></span>

<span data-ttu-id="bf2d3-274">Maximální rozsah by součet všechny rozsahy maximální pro všechny plány služby App Service hostované ve fondu pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-274">The Maximum range would be the sum of all the maximum ranges for all App Service plans hosted in the worker pool.</span></span>

<span data-ttu-id="bf2d3-275">Zvyšte počet pro rozšiřování škálování využívajících pravidla měli nastavit na alespoň 1 X míry inflace plán služby App škálování.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-275">The Increase count for the scale up rules should be set to at least 1X the App Service Plan Inflation Rate for scale up.</span></span>

<span data-ttu-id="bf2d3-276">Snižte počet lze upravit na jinou hodnotu mezi 1/2 X nebo 1 X míry inflace plán služby App škálování směrem dolů.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-276">Decrease count can be adjusted to something between 1/2X or 1X the App Service Plan Inflation Rate for scale down.</span></span>

### <a name="autoscale-for-front-end-pool"></a><span data-ttu-id="bf2d3-277">Škálování front-endu fondu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-277">Autoscale for front-end pool</span></span>
<span data-ttu-id="bf2d3-278">Pravidla pro automatické škálování front-endu jsou jednodušší než pro fondy pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-278">Rules for front-end autoscale are simpler than for worker pools.</span></span> <span data-ttu-id="bf2d3-279">Především se stává měli byste</span><span class="sxs-lookup"><span data-stu-id="bf2d3-279">Primarily, you should</span></span>  
<span data-ttu-id="bf2d3-280">Ujistěte se, že doba trvání měření a časovače cooldown zvažte, že nejsou okamžitou operací škálování na plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-280">make sure that duration of the measurement and the cooldown timers consider that scale operations on an App Service plan are not instantaneous.</span></span>

<span data-ttu-id="bf2d3-281">V tomto scénáři František ví, že míra chyb zvyšuje po dosažení front-end 80 % využití CPU a nastaví pravidlo škálování zvýšit instance následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="bf2d3-281">For this scenario, Frank knows that the error rate increases after front ends reach 80% CPU utilization and sets the autoscale rule to increase instances as follows:</span></span>

![Nastavení automatického škálování front-endu fondu.][Front-End-Scale]

| <span data-ttu-id="bf2d3-283">**Škálování profil – přední končí**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-283">**Autoscale profile – Front ends**</span></span> |
| --- |
| <span data-ttu-id="bf2d3-284">**Název:** škálování – přední končí</span><span class="sxs-lookup"><span data-stu-id="bf2d3-284">**Name:** Autoscale – Front ends</span></span> |
| <span data-ttu-id="bf2d3-285">**Škálování podle:** pravidla plánu a výkonu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-285">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="bf2d3-286">**Profil:** každý den</span><span class="sxs-lookup"><span data-stu-id="bf2d3-286">**Profile:** Everyday</span></span> |
| <span data-ttu-id="bf2d3-287">**Typ:** opakování</span><span class="sxs-lookup"><span data-stu-id="bf2d3-287">**Type:** Recurrence</span></span> |
| <span data-ttu-id="bf2d3-288">**Cílový rozsah:** 3 až 10 instancí</span><span class="sxs-lookup"><span data-stu-id="bf2d3-288">**Target range:** 3 to 10 instances</span></span> |
| <span data-ttu-id="bf2d3-289">**Počet dnů:** každý den</span><span class="sxs-lookup"><span data-stu-id="bf2d3-289">**Days:** Everyday</span></span> |
| <span data-ttu-id="bf2d3-290">**Čas spuštění:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="bf2d3-290">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="bf2d3-291">**Časové pásmo:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="bf2d3-291">**Time zone:** UTC-08</span></span> |
|  |
| <span data-ttu-id="bf2d3-292">**Pravidlo automatického škálování (vertikálně navýšit kapacitu)**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-292">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="bf2d3-293">**Prostředek:** Front-end fondu</span><span class="sxs-lookup"><span data-stu-id="bf2d3-293">**Resource:** Front-end pool</span></span> |
| <span data-ttu-id="bf2d3-294">**Metrika:** % využití procesoru</span><span class="sxs-lookup"><span data-stu-id="bf2d3-294">**Metric:** CPU %</span></span> |
| <span data-ttu-id="bf2d3-295">**Operace:** větší než 60 %</span><span class="sxs-lookup"><span data-stu-id="bf2d3-295">**Operation:** Greater than 60%</span></span> |
| <span data-ttu-id="bf2d3-296">**Doba trvání:** 20 minut</span><span class="sxs-lookup"><span data-stu-id="bf2d3-296">**Duration:** 20 minutes</span></span> |
| <span data-ttu-id="bf2d3-297">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="bf2d3-297">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="bf2d3-298">**Akce:** zvýšení počtu 3</span><span class="sxs-lookup"><span data-stu-id="bf2d3-298">**Action:** Increase count by 3</span></span> |
| <span data-ttu-id="bf2d3-299">**Nástrojů dolů (minuty):** 120.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-299">**Cool down (minutes):** 120</span></span> |
|  |
| <span data-ttu-id="bf2d3-300">**Pravidlo automatického škálování (škálování dolů)**</span><span class="sxs-lookup"><span data-stu-id="bf2d3-300">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="bf2d3-301">**Prostředek:** pracovní fond 1</span><span class="sxs-lookup"><span data-stu-id="bf2d3-301">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="bf2d3-302">**Metrika:** % využití procesoru</span><span class="sxs-lookup"><span data-stu-id="bf2d3-302">**Metric:** CPU %</span></span> |
| <span data-ttu-id="bf2d3-303">**Operace:** méně než 30 %</span><span class="sxs-lookup"><span data-stu-id="bf2d3-303">**Operation:** Less than 30%</span></span> |
| <span data-ttu-id="bf2d3-304">**Doba trvání:** 20 minut</span><span class="sxs-lookup"><span data-stu-id="bf2d3-304">**Duration:** 20 Minutes</span></span> |
| <span data-ttu-id="bf2d3-305">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="bf2d3-305">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="bf2d3-306">**Akce:** snížit počet 3</span><span class="sxs-lookup"><span data-stu-id="bf2d3-306">**Action:** Decrease count by 3</span></span> |
| <span data-ttu-id="bf2d3-307">**Nástrojů dolů (minuty):** 120.</span><span class="sxs-lookup"><span data-stu-id="bf2d3-307">**Cool down (minutes):** 120</span></span> |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
