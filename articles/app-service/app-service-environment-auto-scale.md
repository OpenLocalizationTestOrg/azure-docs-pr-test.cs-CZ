---
title: aaaAutoscaling a v1 App Service Environment
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
ms.openlocfilehash: 1a03cf494309e80596b64471d1a067b2f64a9fee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="autoscaling-and-app-service-environment-v1"></a><span data-ttu-id="cf651-103">Automatické škálování a služby App Service Environment v1</span><span class="sxs-lookup"><span data-stu-id="cf651-103">Autoscaling and App Service Environment v1</span></span>

> [!NOTE]
> <span data-ttu-id="cf651-104">Tento článek je o hello App Service Environment v1.</span><span class="sxs-lookup"><span data-stu-id="cf651-104">This article is about hello App Service Environment v1.</span></span>  <span data-ttu-id="cf651-105">Není k dispozici novější verze hello služby App Service Environment, je snazší toouse a běží na výkonnější infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="cf651-105">There is a newer version of hello App Service Environment that is easier  toouse and runs on more powerful infrastructure.</span></span> <span data-ttu-id="cf651-106">Další informace o nové verzi hello začínat hello toolearn [toohello Úvod App Service Environment](../app-service/app-service-environment/intro.md).</span><span class="sxs-lookup"><span data-stu-id="cf651-106">toolearn more about hello new version start with hello [Introduction toohello App  Service Environment](../app-service/app-service-environment/intro.md).</span></span>
> 

<span data-ttu-id="cf651-107">Podpora prostředí Azure App Service *automatické škálování*.</span><span class="sxs-lookup"><span data-stu-id="cf651-107">Azure App Service environments support *autoscaling*.</span></span> <span data-ttu-id="cf651-108">Můžete fondy škálování jednotlivých pracovních procesů na základě metriky nebo plán.</span><span class="sxs-lookup"><span data-stu-id="cf651-108">You can autoscale individual worker pools based on metrics or schedule.</span></span>

![Možnosti škálování fondu pracovních procesů.][intro]

<span data-ttu-id="cf651-110">Automatické škálování optimalizuje vaše využití prostředků pomocí automatické zvětšování a zmenšování toofit prostředí App Service váš profil a rozpočet nebo zatížení.</span><span class="sxs-lookup"><span data-stu-id="cf651-110">Autoscaling optimizes your resource utilization by automatically growing and shrinking an App Service environment toofit your budget and or load profile.</span></span>

## <a name="configure-worker-pool-autoscale"></a><span data-ttu-id="cf651-111">Konfigurace pracovního procesu škálování fondu</span><span class="sxs-lookup"><span data-stu-id="cf651-111">Configure worker pool autoscale</span></span>
<span data-ttu-id="cf651-112">Funkce škálování hello můžete přistupovat z hello **nastavení** kartě hello fondu pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="cf651-112">You can access hello autoscale functionality from hello **Settings** tab of hello worker pool.</span></span>

![Karta nastavení fondu pracovních procesů hello.][settings-scale]

<span data-ttu-id="cf651-114">Z tohoto místa hello rozhraní by měl být poměrně obeznámeni, protože je hello plánování stejné prostředí, které vidíte při škálování App Service.</span><span class="sxs-lookup"><span data-stu-id="cf651-114">From there, hello interface should be fairly familiar since it is hello same experience that you see when you scale an App Service plan.</span></span> 

![Nastavení ruční škálování.][scale-manual]

<span data-ttu-id="cf651-116">Můžete také nakonfigurovat profilem škálování.</span><span class="sxs-lookup"><span data-stu-id="cf651-116">You can also configure an autoscale profile.</span></span>

![Nastavení automatického škálování.][scale-profile]

<span data-ttu-id="cf651-118">Profilů automatického škálování jsou užitečné tooset omezení pro vaše škálování.</span><span class="sxs-lookup"><span data-stu-id="cf651-118">Autoscale profiles are useful tooset limits on your scale.</span></span> <span data-ttu-id="cf651-119">Tímto způsobem může mít konzistentní výkon prostředí nastavením hodnotu dolní mez rozsahu (1) a předvídatelný výdaji cap nastavením horní mez (2).</span><span class="sxs-lookup"><span data-stu-id="cf651-119">This way, you can have a consistent performance experience by setting a lower bound scale value (1) and a predictable spend cap by setting an upper bound (2).</span></span>

![Nastavení škálování v profilu.][scale-profile2]

<span data-ttu-id="cf651-121">Po definování profilu, můžete přidat tooscale pravidel škálování nahoru nebo dolů hello počet instancí ve fondu pracovních procesů hello v rámci hranice hello definované hello profilem.</span><span class="sxs-lookup"><span data-stu-id="cf651-121">After you define a profile, you can add autoscale rules tooscale up or down hello number of instances in hello worker pool within hello bounds defined by hello profile.</span></span> <span data-ttu-id="cf651-122">Pravidla automatického škálování jsou založené na metriky.</span><span class="sxs-lookup"><span data-stu-id="cf651-122">Autoscale rules are based on metrics.</span></span>

![Pravidlo škálování.][scale-rule]

 <span data-ttu-id="cf651-124">Všechny fondu pracovních procesů nebo front-end metriky můžou být použité toodefine škálování pravidla.</span><span class="sxs-lookup"><span data-stu-id="cf651-124">Any worker pool or front-end metrics can be used toodefine autoscale rules.</span></span> <span data-ttu-id="cf651-125">Tyto metriky jsou stejné metriky můžete sledovat v hello prostředků okno grafů nebo nastavit výstrahy pro hello.</span><span class="sxs-lookup"><span data-stu-id="cf651-125">These metrics are hello same metrics you can monitor in hello resource blade graphs or set alerts for.</span></span>

## <a name="autoscale-example"></a><span data-ttu-id="cf651-126">Příklad škálování</span><span class="sxs-lookup"><span data-stu-id="cf651-126">Autoscale example</span></span>
<span data-ttu-id="cf651-127">Škálování služby App Service environment lze ukázat nejlépe ve scénáři s návodem.</span><span class="sxs-lookup"><span data-stu-id="cf651-127">Autoscale of an App Service environment can best be illustrated by walking through a scenario.</span></span>

<span data-ttu-id="cf651-128">Tento článek vysvětluje všechny požadavky nutné hello při nastavení automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="cf651-128">This article explains all hello necessary considerations when you set up autoscale.</span></span> <span data-ttu-id="cf651-129">Hello článek vás provede procesem hello interakce, které začalo přehrát při zohlednit v automatické škálování služby App Service Environment, které jsou hostované v App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="cf651-129">hello article walks you through hello interactions that come into play when you factor in autoscaling App Service environments that are hosted in App Service Environment.</span></span>

### <a name="scenario-introduction"></a><span data-ttu-id="cf651-130">Scénář Úvod</span><span class="sxs-lookup"><span data-stu-id="cf651-130">Scenario introduction</span></span>
<span data-ttu-id="cf651-131">František se správce systému pro organizace, který migroval část hello úlohy, že spravuje tooan služby App Service environment.</span><span class="sxs-lookup"><span data-stu-id="cf651-131">Frank is a sysadmin for an enterprise who has migrated a portion of hello workloads that he manages tooan App Service environment.</span></span>

<span data-ttu-id="cf651-132">Hello App Service environment není nakonfigurováno toomanual škálování takto:</span><span class="sxs-lookup"><span data-stu-id="cf651-132">hello App Service environment is configured toomanual scale as follows:</span></span>

* <span data-ttu-id="cf651-133">**Front-zakončení:** 3</span><span class="sxs-lookup"><span data-stu-id="cf651-133">**Front ends:** 3</span></span>
* <span data-ttu-id="cf651-134">**Fond pracovních procesů 1**: 10</span><span class="sxs-lookup"><span data-stu-id="cf651-134">**Worker pool 1**: 10</span></span>
* <span data-ttu-id="cf651-135">**Fond pracovních procesů 2**: 5</span><span class="sxs-lookup"><span data-stu-id="cf651-135">**Worker pool 2**: 5</span></span>
* <span data-ttu-id="cf651-136">**Fond pracovních procesů 3**: 5</span><span class="sxs-lookup"><span data-stu-id="cf651-136">**Worker pool 3**: 5</span></span>

<span data-ttu-id="cf651-137">Fond pracovních procesů 1 se používá pro úlohy v produkčním prostředí při fondu pracovních procesů 2 a fondu pracovních procesů 3 se používají pro zajištění kvality (QA) a vývoj úlohy.</span><span class="sxs-lookup"><span data-stu-id="cf651-137">Worker pool 1 is used for production workloads, while worker pool 2 and worker pool 3 are used for quality assurance (QA) and development workloads.</span></span>

<span data-ttu-id="cf651-138">plánů pro kontrolu kvality a vývojářů jsou zprostředkovatele Hello služby App Service nakonfigurované toomanual škálování.</span><span class="sxs-lookup"><span data-stu-id="cf651-138">hello App Service plans for QA and dev are configured toomanual scale.</span></span> <span data-ttu-id="cf651-139">produkční Hello plán služby App Service se nastavuje tooautoscale toodeal s rozdíly v zatížení a provozu.</span><span class="sxs-lookup"><span data-stu-id="cf651-139">hello production App Service plan is set tooautoscale toodeal with variations in load and traffic.</span></span>

<span data-ttu-id="cf651-140">Jan je velmi dobře známé s aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="cf651-140">Frank is very familiar with hello application.</span></span> <span data-ttu-id="cf651-141">Ví, že hello hodiny špičky pro zatížení jsou mezi 9:00:00 a 18:00:00, protože to je – obchodní (LOB) aplikace, která zaměstnanci používají, když jsou ve hello office.</span><span class="sxs-lookup"><span data-stu-id="cf651-141">He knows that hello peak hours for load are between 9:00 AM and 6:00 PM because this is a line-of-business (LOB) application that employees use while they are in hello office.</span></span> <span data-ttu-id="cf651-142">Využití zahodí po, když se uživatelé provádějí pro daný den.</span><span class="sxs-lookup"><span data-stu-id="cf651-142">Usage drops after that when users are done for that day.</span></span> <span data-ttu-id="cf651-143">Mimo dobu ve špičce přetrvává některé zatížení vzhledem k tomu, že uživatelé mají přístup ke hello aplikaci vzdáleně pomocí jejich mobilní zařízení nebo domácích počítačů.</span><span class="sxs-lookup"><span data-stu-id="cf651-143">Outside peak hours, there is still some load because users can access hello app remotely by using their mobile devices or home PCs.</span></span> <span data-ttu-id="cf651-144">produkční Hello, plán služby App Service je již nakonfigurován tooautoscale podle využití procesoru s hello následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="cf651-144">hello production App Service plan is already configured tooautoscale based on CPU usage with hello following rules:</span></span>

![Konkrétní nastavení pro obchodní aplikace.][asp-scale]

| <span data-ttu-id="cf651-146">**Škálování profil – dny v týdnu – plán služby App Service**</span><span class="sxs-lookup"><span data-stu-id="cf651-146">**Autoscale profile – Weekdays – App Service plan**</span></span> | <span data-ttu-id="cf651-147">**Škálování profil – víkendů – plán služby App Service**</span><span class="sxs-lookup"><span data-stu-id="cf651-147">**Autoscale profile – Weekends – App Service plan**</span></span> |
| --- | --- |
| <span data-ttu-id="cf651-148">**Název:** profil den v týdnu</span><span class="sxs-lookup"><span data-stu-id="cf651-148">**Name:** Weekday profile</span></span> |<span data-ttu-id="cf651-149">**Název:** víkendu profilu</span><span class="sxs-lookup"><span data-stu-id="cf651-149">**Name:** Weekend profile</span></span> |
| <span data-ttu-id="cf651-150">**Škálování podle:** pravidla plánu a výkonu</span><span class="sxs-lookup"><span data-stu-id="cf651-150">**Scale by:** Schedule and performance rules</span></span> |<span data-ttu-id="cf651-151">**Škálování podle:** pravidla plánu a výkonu</span><span class="sxs-lookup"><span data-stu-id="cf651-151">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="cf651-152">**Profil:** dny v týdnu</span><span class="sxs-lookup"><span data-stu-id="cf651-152">**Profile:** Weekdays</span></span> |<span data-ttu-id="cf651-153">**Profil:** víkendu</span><span class="sxs-lookup"><span data-stu-id="cf651-153">**Profile:** Weekend</span></span> |
| <span data-ttu-id="cf651-154">**Typ:** opakování</span><span class="sxs-lookup"><span data-stu-id="cf651-154">**Type:** Recurrence</span></span> |<span data-ttu-id="cf651-155">**Typ:** opakování</span><span class="sxs-lookup"><span data-stu-id="cf651-155">**Type:** Recurrence</span></span> |
| <span data-ttu-id="cf651-156">**Cílový rozsah:** 5 too20 instancí</span><span class="sxs-lookup"><span data-stu-id="cf651-156">**Target range:** 5 too20 instances</span></span> |<span data-ttu-id="cf651-157">**Cílový rozsah:** 3 too10 instancí</span><span class="sxs-lookup"><span data-stu-id="cf651-157">**Target range:** 3 too10 instances</span></span> |
| <span data-ttu-id="cf651-158">**Počet dnů:** pondělí, úterý, středu, čtvrtek a pátek</span><span class="sxs-lookup"><span data-stu-id="cf651-158">**Days:** Monday, Tuesday, Wednesday, Thursday, Friday</span></span> |<span data-ttu-id="cf651-159">**Počet dnů:** sobota, neděle</span><span class="sxs-lookup"><span data-stu-id="cf651-159">**Days:** Saturday, Sunday</span></span> |
| <span data-ttu-id="cf651-160">**Čas spuštění:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="cf651-160">**Start time:** 9:00 AM</span></span> |<span data-ttu-id="cf651-161">**Čas spuštění:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="cf651-161">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="cf651-162">**Časové pásmo:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="cf651-162">**Time zone:** UTC-08</span></span> |<span data-ttu-id="cf651-163">**Časové pásmo:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="cf651-163">**Time zone:** UTC-08</span></span> |
|  | |
| <span data-ttu-id="cf651-164">**Pravidlo automatického škálování (vertikálně navýšit kapacitu)**</span><span class="sxs-lookup"><span data-stu-id="cf651-164">**Autoscale rule (Scale Up)**</span></span> |<span data-ttu-id="cf651-165">**Pravidlo automatického škálování (vertikálně navýšit kapacitu)**</span><span class="sxs-lookup"><span data-stu-id="cf651-165">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="cf651-166">**Prostředek:** produkční (služby App Service Environment)</span><span class="sxs-lookup"><span data-stu-id="cf651-166">**Resource:** Production (App Service Environment)</span></span> |<span data-ttu-id="cf651-167">**Prostředek:** produkční (služby App Service Environment)</span><span class="sxs-lookup"><span data-stu-id="cf651-167">**Resource:** Production (App Service Environment)</span></span> |
| <span data-ttu-id="cf651-168">**Metrika:** % využití procesoru</span><span class="sxs-lookup"><span data-stu-id="cf651-168">**Metric:** CPU %</span></span> |<span data-ttu-id="cf651-169">**Metrika:** % využití procesoru</span><span class="sxs-lookup"><span data-stu-id="cf651-169">**Metric:** CPU %</span></span> |
| <span data-ttu-id="cf651-170">**Operace:** větší než 60 %</span><span class="sxs-lookup"><span data-stu-id="cf651-170">**Operation:** Greater than 60%</span></span> |<span data-ttu-id="cf651-171">**Operace:** větší než 80 %</span><span class="sxs-lookup"><span data-stu-id="cf651-171">**Operation:** Greater than 80%</span></span> |
| <span data-ttu-id="cf651-172">**Doba trvání:** 5 minut</span><span class="sxs-lookup"><span data-stu-id="cf651-172">**Duration:** 5 Minutes</span></span> |<span data-ttu-id="cf651-173">**Doba trvání:** 10 minut</span><span class="sxs-lookup"><span data-stu-id="cf651-173">**Duration:** 10 Minutes</span></span> |
| <span data-ttu-id="cf651-174">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="cf651-174">**Time aggregation:** Average</span></span> |<span data-ttu-id="cf651-175">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="cf651-175">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="cf651-176">**Akce:** zvýšit počet 2</span><span class="sxs-lookup"><span data-stu-id="cf651-176">**Action:** Increase count by 2</span></span> |<span data-ttu-id="cf651-177">**Akce:** zvýšit počet 1</span><span class="sxs-lookup"><span data-stu-id="cf651-177">**Action:** Increase count by 1</span></span> |
| <span data-ttu-id="cf651-178">**Nástrojů dolů (minuty):** 15</span><span class="sxs-lookup"><span data-stu-id="cf651-178">**Cool down (minutes):** 15</span></span> |<span data-ttu-id="cf651-179">**Nástrojů dolů (minuty):** 20</span><span class="sxs-lookup"><span data-stu-id="cf651-179">**Cool down (minutes):** 20</span></span> |
|  | |
| <span data-ttu-id="cf651-180">**Pravidlo automatického škálování (škálování dolů)**</span><span class="sxs-lookup"><span data-stu-id="cf651-180">**Autoscale rule (Scale Down)**</span></span> |<span data-ttu-id="cf651-181">**Pravidlo automatického škálování (škálování dolů)**</span><span class="sxs-lookup"><span data-stu-id="cf651-181">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="cf651-182">**Prostředek:** produkční (služby App Service Environment)</span><span class="sxs-lookup"><span data-stu-id="cf651-182">**Resource:** Production (App Service Environment)</span></span> |<span data-ttu-id="cf651-183">**Prostředek:** produkční (služby App Service Environment)</span><span class="sxs-lookup"><span data-stu-id="cf651-183">**Resource:** Production (App Service Environment)</span></span> |
| <span data-ttu-id="cf651-184">**Metrika:** % využití procesoru</span><span class="sxs-lookup"><span data-stu-id="cf651-184">**Metric:** CPU %</span></span> |<span data-ttu-id="cf651-185">**Metrika:** % využití procesoru</span><span class="sxs-lookup"><span data-stu-id="cf651-185">**Metric:** CPU %</span></span> |
| <span data-ttu-id="cf651-186">**Operace:** méně než 30 %</span><span class="sxs-lookup"><span data-stu-id="cf651-186">**Operation:** Less than 30%</span></span> |<span data-ttu-id="cf651-187">**Operace:** méně než 20 %</span><span class="sxs-lookup"><span data-stu-id="cf651-187">**Operation:** Less than 20%</span></span> |
| <span data-ttu-id="cf651-188">**Doba trvání:** 10 minut</span><span class="sxs-lookup"><span data-stu-id="cf651-188">**Duration:** 10 minutes</span></span> |<span data-ttu-id="cf651-189">**Doba trvání:** 15 minut</span><span class="sxs-lookup"><span data-stu-id="cf651-189">**Duration:** 15 minutes</span></span> |
| <span data-ttu-id="cf651-190">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="cf651-190">**Time aggregation:** Average</span></span> |<span data-ttu-id="cf651-191">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="cf651-191">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="cf651-192">**Akce:** snižte počet 1</span><span class="sxs-lookup"><span data-stu-id="cf651-192">**Action:** Decrease count by 1</span></span> |<span data-ttu-id="cf651-193">**Akce:** snižte počet 1</span><span class="sxs-lookup"><span data-stu-id="cf651-193">**Action:** Decrease count by 1</span></span> |
| <span data-ttu-id="cf651-194">**Nástrojů dolů (minuty):** 20</span><span class="sxs-lookup"><span data-stu-id="cf651-194">**Cool down (minutes):** 20</span></span> |<span data-ttu-id="cf651-195">**Nástrojů dolů (minuty):** 10</span><span class="sxs-lookup"><span data-stu-id="cf651-195">**Cool down (minutes):** 10</span></span> |

### <a name="app-service-plan-inflation-rate"></a><span data-ttu-id="cf651-196">Míry inflace plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="cf651-196">App Service plan inflation rate</span></span>
<span data-ttu-id="cf651-197">Plány služby App Service, které jsou nakonfigurované tooautoscale učinit s maximální rychlostí za hodinu.</span><span class="sxs-lookup"><span data-stu-id="cf651-197">App Service plans that are configured tooautoscale do so at a maximum rate per hour.</span></span> <span data-ttu-id="cf651-198">Tento kurz můžete vypočítáváno na hello hodnoty zadané v pravidle automatického škálování hello.</span><span class="sxs-lookup"><span data-stu-id="cf651-198">This rate can be calculated based on hello values provided on hello autoscale rule.</span></span>

<span data-ttu-id="cf651-199">Princip fungování a způsob výpočtu hello *míry inflace plán služby App Service* je důležité pro škálování prostředí služby App Service, protože nejsou okamžitou fondu pracovních procesů tooa změny měřítka.</span><span class="sxs-lookup"><span data-stu-id="cf651-199">Understanding and calculating hello *App Service plan inflation rate* is important for App Service environment autoscale because scale changes tooa worker pool are not instantaneous.</span></span>

<span data-ttu-id="cf651-200">Hello míry inflace plán služby App Service se vypočítává takto:</span><span class="sxs-lookup"><span data-stu-id="cf651-200">hello App Service plan inflation rate is calculated as follows:</span></span>

![Výpočet míry inflace plán služby App Service.][ASP-Inflation]

<span data-ttu-id="cf651-202">Podle hello škálování – pravidlo vertikálně navýšit kapacitu pro profil den v týdnu hello hello provozních plán služby App Service:</span><span class="sxs-lookup"><span data-stu-id="cf651-202">Based on hello Autoscale – Scale Up rule for hello Weekday profile of hello production App Service plan:</span></span>

![Míra inflace plán služby App Service pro dny v týdnu podle škálování – pravidlo vertikálně navýšit kapacitu.][Equation1]

<span data-ttu-id="cf651-204">V případě hello hello škálování – pravidlo vertikálně navýšit kapacitu pro profil víkendu hello hello provozních plán služby App Service, by hello vzorec odkazující na:</span><span class="sxs-lookup"><span data-stu-id="cf651-204">In hello case of hello Autoscale – Scale Up rule for hello Weekend profile of hello production App Service plan, hello formula would resolve to:</span></span>

![Míry inflace plán služby App Service pro víkendů podle škálování – pravidlo vertikálně navýšit kapacitu.][Equation2]

<span data-ttu-id="cf651-206">Tato hodnota může také vypočítá operacím vertikální snížení kapacity.</span><span class="sxs-lookup"><span data-stu-id="cf651-206">This value can also be calculated for scale-down operations.</span></span>

<span data-ttu-id="cf651-207">Podle hello škálování – pravidlo škálování dolů profilu hello den v týdnu hello provozních plán služby App Service, to vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="cf651-207">Based on hello Autoscale – Scale Down rule for hello Weekday profile of hello production App Service plan, this would look as follows:</span></span>

![Míra inflace plán služby App Service pro dny v týdnu podle škálování – pravidlo škálování dolů.][Equation3]

<span data-ttu-id="cf651-209">V případě hello hello škálování – pravidlo škálování dolů profilu hello víkendu hello provozních plán služby App Service, by hello vzorec odkazující na:</span><span class="sxs-lookup"><span data-stu-id="cf651-209">In hello case of hello Autoscale – Scale Down rule for hello Weekend profile of hello production App Service plan, hello formula would resolve to:</span></span>  

![Rychlost inflace plán služby App Service pro víkendů podle škálování – pravidlo škálování dolů.][Equation4]

<span data-ttu-id="cf651-211">produkční Hello plán služby App Service můžou růst v maximálně osm instancí za hodinu během hello týden a čtyři instance za hodinu během víkendu hello.</span><span class="sxs-lookup"><span data-stu-id="cf651-211">hello production App Service plan can grow at a maximum rate of eight instances/hour during hello week and four instances/hour during hello weekend.</span></span> <span data-ttu-id="cf651-212">Můžete ho verzi instance s maximální rychlostí čtyři instancí za hodinu během hello týden a šesti instancí za hodinu během víkendu.</span><span class="sxs-lookup"><span data-stu-id="cf651-212">It can release instances at a maximum rate of four instances/hour during hello week and six instances/hour during weekends.</span></span>

<span data-ttu-id="cf651-213">Pokud víc plány služby App Service se hostovaným ve fondu pracovních procesů, máte toocalculate hello *celkovou rychlost inflace* jako součet hello hello inflace rychlost pro všechny hello plánů služby App Service, která bude hostování v tomto fondu pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="cf651-213">If multiple App Service plans are being hosted in a worker pool, you have toocalculate hello *total inflation rate* as hello sum of hello inflation rate for all hello App Service plans that are being hosting in that worker pool.</span></span>

![Celková rychlost inflace výpočet pro víc plány služby App Service hostované ve fondu pracovních procesů.][ASP-Total-Inflation]

### <a name="use-hello-app-service-plan-inflation-rate-toodefine-worker-pool-autoscale-rules"></a><span data-ttu-id="cf651-215">Použití hello služby App Service plánování míry inflace toodefine pracovní fond škálování pravidla</span><span class="sxs-lookup"><span data-stu-id="cf651-215">Use hello App Service plan inflation rate toodefine worker pool autoscale rules</span></span>
<span data-ttu-id="cf651-216">Pracovní fondu, které hostují plány služby App Service, které jsou nakonfigurované tooautoscale nutné přidělit vyrovnávací paměť kapacity.</span><span class="sxs-lookup"><span data-stu-id="cf651-216">Worker pools that host App Service plans that are configured tooautoscale need to be allocated a buffer of capacity.</span></span> <span data-ttu-id="cf651-217">vyrovnávací paměť Hello umožňuje toogrow operace škálování hello a podle potřeby zmenšit plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="cf651-217">hello buffer allows for hello autoscale operations toogrow and shrink the App Service plan as needed.</span></span> <span data-ttu-id="cf651-218">Minimální vyrovnávací paměti Hello můžou být hello vypočítat celkové aplikace služby plánování inflace Rate.</span><span class="sxs-lookup"><span data-stu-id="cf651-218">hello minimum buffer would be hello calculated Total App Service Plan Inflation Rate.</span></span>

<span data-ttu-id="cf651-219">Protože operací škálování služby App Service environment trvat některé tooapply čas, všechny změny by měl účet pro vyžádání další změny, které může nastat, když probíhá operace škálování.</span><span class="sxs-lookup"><span data-stu-id="cf651-219">Because App Service environment scale operations take some time tooapply, any change should account for further demand changes that could happen while a scale operation is in progress.</span></span> <span data-ttu-id="cf651-220">tooaccommodate tato čekací doba, doporučujeme vám použít hello počítá celkovou aplikace služby plánování inflace rychlost jako hello minimální počet instancí, které jsou přidány pro každou operaci škálování.</span><span class="sxs-lookup"><span data-stu-id="cf651-220">tooaccommodate this latency, we recommend that you use hello calculated Total App Service Plan Inflation Rate as hello minimum number of instances that are added for each autoscale operation.</span></span>

<span data-ttu-id="cf651-221">Pomocí těchto informací můžete definovat František hello profil škálování a pravidla:</span><span class="sxs-lookup"><span data-stu-id="cf651-221">With this information, Frank can define hello following autoscale profile and rules:</span></span>

![Profil pravidel škálování například LOB.][Worker-Pool-Scale]

| <span data-ttu-id="cf651-223">**Škálování profil – dny v týdnu**</span><span class="sxs-lookup"><span data-stu-id="cf651-223">**Autoscale profile – Weekdays**</span></span> | <span data-ttu-id="cf651-224">**Škálování profil – víkendů**</span><span class="sxs-lookup"><span data-stu-id="cf651-224">**Autoscale profile – Weekends**</span></span> |
| --- | --- |
| <span data-ttu-id="cf651-225">**Název:** profil den v týdnu</span><span class="sxs-lookup"><span data-stu-id="cf651-225">**Name:** Weekday profile</span></span> |<span data-ttu-id="cf651-226">**Název:** víkendu profilu</span><span class="sxs-lookup"><span data-stu-id="cf651-226">**Name:** Weekend profile</span></span> |
| <span data-ttu-id="cf651-227">**Škálování podle:** pravidla plánu a výkonu</span><span class="sxs-lookup"><span data-stu-id="cf651-227">**Scale by:** Schedule and performance rules</span></span> |<span data-ttu-id="cf651-228">**Škálování podle:** pravidla plánu a výkonu</span><span class="sxs-lookup"><span data-stu-id="cf651-228">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="cf651-229">**Profil:** dny v týdnu</span><span class="sxs-lookup"><span data-stu-id="cf651-229">**Profile:** Weekdays</span></span> |<span data-ttu-id="cf651-230">**Profil:** víkendu</span><span class="sxs-lookup"><span data-stu-id="cf651-230">**Profile:** Weekend</span></span> |
| <span data-ttu-id="cf651-231">**Typ:** opakování</span><span class="sxs-lookup"><span data-stu-id="cf651-231">**Type:** Recurrence</span></span> |<span data-ttu-id="cf651-232">**Typ:** opakování</span><span class="sxs-lookup"><span data-stu-id="cf651-232">**Type:** Recurrence</span></span> |
| <span data-ttu-id="cf651-233">**Cílový rozsah:** 13 too25 instancí</span><span class="sxs-lookup"><span data-stu-id="cf651-233">**Target range:** 13 too25 instances</span></span> |<span data-ttu-id="cf651-234">**Cílový rozsah:** 6 too15 instancí</span><span class="sxs-lookup"><span data-stu-id="cf651-234">**Target range:** 6 too15 instances</span></span> |
| <span data-ttu-id="cf651-235">**Počet dnů:** pondělí, úterý, středu, čtvrtek a pátek</span><span class="sxs-lookup"><span data-stu-id="cf651-235">**Days:** Monday, Tuesday, Wednesday, Thursday, Friday</span></span> |<span data-ttu-id="cf651-236">**Počet dnů:** sobota, neděle</span><span class="sxs-lookup"><span data-stu-id="cf651-236">**Days:** Saturday, Sunday</span></span> |
| <span data-ttu-id="cf651-237">**Čas spuštění:** 7:00:00</span><span class="sxs-lookup"><span data-stu-id="cf651-237">**Start time:** 7:00 AM</span></span> |<span data-ttu-id="cf651-238">**Čas spuštění:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="cf651-238">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="cf651-239">**Časové pásmo:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="cf651-239">**Time zone:** UTC-08</span></span> |<span data-ttu-id="cf651-240">**Časové pásmo:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="cf651-240">**Time zone:** UTC-08</span></span> |
|  | |
| <span data-ttu-id="cf651-241">**Pravidlo automatického škálování (vertikálně navýšit kapacitu)**</span><span class="sxs-lookup"><span data-stu-id="cf651-241">**Autoscale rule (Scale Up)**</span></span> |<span data-ttu-id="cf651-242">**Pravidlo automatického škálování (vertikálně navýšit kapacitu)**</span><span class="sxs-lookup"><span data-stu-id="cf651-242">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="cf651-243">**Prostředek:** pracovní fond 1</span><span class="sxs-lookup"><span data-stu-id="cf651-243">**Resource:** Worker pool 1</span></span> |<span data-ttu-id="cf651-244">**Prostředek:** pracovní fond 1</span><span class="sxs-lookup"><span data-stu-id="cf651-244">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="cf651-245">**Metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="cf651-245">**Metric:** WorkersAvailable</span></span> |<span data-ttu-id="cf651-246">**Metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="cf651-246">**Metric:** WorkersAvailable</span></span> |
| <span data-ttu-id="cf651-247">**Operace:** menší než 8</span><span class="sxs-lookup"><span data-stu-id="cf651-247">**Operation:** Less than 8</span></span> |<span data-ttu-id="cf651-248">**Operace:** menší než 3</span><span class="sxs-lookup"><span data-stu-id="cf651-248">**Operation:** Less than 3</span></span> |
| <span data-ttu-id="cf651-249">**Doba trvání:** 20 minut</span><span class="sxs-lookup"><span data-stu-id="cf651-249">**Duration:** 20 minutes</span></span> |<span data-ttu-id="cf651-250">**Doba trvání:** 30 minut</span><span class="sxs-lookup"><span data-stu-id="cf651-250">**Duration:** 30 minutes</span></span> |
| <span data-ttu-id="cf651-251">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="cf651-251">**Time aggregation:** Average</span></span> |<span data-ttu-id="cf651-252">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="cf651-252">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="cf651-253">**Akce:** zvýšit počet 8</span><span class="sxs-lookup"><span data-stu-id="cf651-253">**Action:** Increase count by 8</span></span> |<span data-ttu-id="cf651-254">**Akce:** zvýšení počtu 3</span><span class="sxs-lookup"><span data-stu-id="cf651-254">**Action:** Increase count by 3</span></span> |
| <span data-ttu-id="cf651-255">**Nástrojů dolů (minuty):** 180</span><span class="sxs-lookup"><span data-stu-id="cf651-255">**Cool down (minutes):** 180</span></span> |<span data-ttu-id="cf651-256">**Nástrojů dolů (minuty):** 180</span><span class="sxs-lookup"><span data-stu-id="cf651-256">**Cool down (minutes):** 180</span></span> |
|  | |
| <span data-ttu-id="cf651-257">**Pravidlo automatického škálování (škálování dolů)**</span><span class="sxs-lookup"><span data-stu-id="cf651-257">**Autoscale rule (Scale Down)**</span></span> |<span data-ttu-id="cf651-258">**Pravidlo automatického škálování (škálování dolů)**</span><span class="sxs-lookup"><span data-stu-id="cf651-258">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="cf651-259">**Prostředek:** pracovní fond 1</span><span class="sxs-lookup"><span data-stu-id="cf651-259">**Resource:** Worker pool 1</span></span> |<span data-ttu-id="cf651-260">**Prostředek:** pracovní fond 1</span><span class="sxs-lookup"><span data-stu-id="cf651-260">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="cf651-261">**Metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="cf651-261">**Metric:** WorkersAvailable</span></span> |<span data-ttu-id="cf651-262">**Metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="cf651-262">**Metric:** WorkersAvailable</span></span> |
| <span data-ttu-id="cf651-263">**Operace:** větší než 8</span><span class="sxs-lookup"><span data-stu-id="cf651-263">**Operation:** Greater than 8</span></span> |<span data-ttu-id="cf651-264">**Operace:** větší než 3</span><span class="sxs-lookup"><span data-stu-id="cf651-264">**Operation:** Greater than 3</span></span> |
| <span data-ttu-id="cf651-265">**Doba trvání:** 20 minut</span><span class="sxs-lookup"><span data-stu-id="cf651-265">**Duration:** 20 minutes</span></span> |<span data-ttu-id="cf651-266">**Doba trvání:** 15 minut</span><span class="sxs-lookup"><span data-stu-id="cf651-266">**Duration:** 15 minutes</span></span> |
| <span data-ttu-id="cf651-267">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="cf651-267">**Time aggregation:** Average</span></span> |<span data-ttu-id="cf651-268">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="cf651-268">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="cf651-269">**Akce:** snižte počet 2</span><span class="sxs-lookup"><span data-stu-id="cf651-269">**Action:** Decrease count by 2</span></span> |<span data-ttu-id="cf651-270">**Akce:** snížit počet 3</span><span class="sxs-lookup"><span data-stu-id="cf651-270">**Action:** Decrease count by 3</span></span> |
| <span data-ttu-id="cf651-271">**Nástrojů dolů (minuty):** 120.</span><span class="sxs-lookup"><span data-stu-id="cf651-271">**Cool down (minutes):** 120</span></span> |<span data-ttu-id="cf651-272">**Nástrojů dolů (minuty):** 120.</span><span class="sxs-lookup"><span data-stu-id="cf651-272">**Cool down (minutes):** 120</span></span> |

<span data-ttu-id="cf651-273">minimální instancí hello definovaná v profilu pro hello plán služby App Service + vyrovnávací paměti je vypočítána Hello cílový rozsah definovaný v profilu hello.</span><span class="sxs-lookup"><span data-stu-id="cf651-273">hello Target range defined in hello profile is calculated by hello minimum instances defined in the profile for hello App Service plan + buffer.</span></span>

<span data-ttu-id="cf651-274">Maximální rozsah Hello by všechny rozsahy maximální hello pro všechny plány služby App Service hostované ve fondu pracovních procesů hello hello součet.</span><span class="sxs-lookup"><span data-stu-id="cf651-274">hello Maximum range would be hello sum of all hello maximum ranges for all App Service plans hosted in hello worker pool.</span></span>

<span data-ttu-id="cf651-275">Hello zvýšení počtu u hello rozšiřování škálování využívajících pravidla musí být sada tooat minimálně 1 X míry inflace plán služby App škálování nahoru.</span><span class="sxs-lookup"><span data-stu-id="cf651-275">hello Increase count for hello scale up rules should be set tooat least 1X the App Service Plan Inflation Rate for scale up.</span></span>

<span data-ttu-id="cf651-276">Snižte počet může být upravenou toosomething mezi 1/2 X nebo 1 X hello míry inflace plán služby App pro škálování směrem dolů.</span><span class="sxs-lookup"><span data-stu-id="cf651-276">Decrease count can be adjusted toosomething between 1/2X or 1X hello App Service Plan Inflation Rate for scale down.</span></span>

### <a name="autoscale-for-front-end-pool"></a><span data-ttu-id="cf651-277">Škálování front-endu fondu</span><span class="sxs-lookup"><span data-stu-id="cf651-277">Autoscale for front-end pool</span></span>
<span data-ttu-id="cf651-278">Pravidla pro automatické škálování front-endu jsou jednodušší než pro fondy pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="cf651-278">Rules for front-end autoscale are simpler than for worker pools.</span></span> <span data-ttu-id="cf651-279">Především se stává měli byste</span><span class="sxs-lookup"><span data-stu-id="cf651-279">Primarily, you should</span></span>  
<span data-ttu-id="cf651-280">Ujistěte se, když trvání hello měření a hello cooldown časovače zvážit, že operace škálování na plán služby App Service nejsou okamžitou.</span><span class="sxs-lookup"><span data-stu-id="cf651-280">make sure that duration of hello measurement and hello cooldown timers consider that scale operations on an App Service plan are not instantaneous.</span></span>

<span data-ttu-id="cf651-281">Pro tento scénář František ví, že míra chyb hello zvyšuje po dosažení front-end 80 % využití CPU a nastaví hello škálování instance tooincrease pravidel následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cf651-281">For this scenario, Frank knows that hello error rate increases after front ends reach 80% CPU utilization and sets hello autoscale rule tooincrease instances as follows:</span></span>

![Nastavení automatického škálování front-endu fondu.][Front-End-Scale]

| <span data-ttu-id="cf651-283">**Škálování profil – přední končí**</span><span class="sxs-lookup"><span data-stu-id="cf651-283">**Autoscale profile – Front ends**</span></span> |
| --- |
| <span data-ttu-id="cf651-284">**Název:** škálování – přední končí</span><span class="sxs-lookup"><span data-stu-id="cf651-284">**Name:** Autoscale – Front ends</span></span> |
| <span data-ttu-id="cf651-285">**Škálování podle:** pravidla plánu a výkonu</span><span class="sxs-lookup"><span data-stu-id="cf651-285">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="cf651-286">**Profil:** každý den</span><span class="sxs-lookup"><span data-stu-id="cf651-286">**Profile:** Everyday</span></span> |
| <span data-ttu-id="cf651-287">**Typ:** opakování</span><span class="sxs-lookup"><span data-stu-id="cf651-287">**Type:** Recurrence</span></span> |
| <span data-ttu-id="cf651-288">**Cílový rozsah:** 3 too10 instancí</span><span class="sxs-lookup"><span data-stu-id="cf651-288">**Target range:** 3 too10 instances</span></span> |
| <span data-ttu-id="cf651-289">**Počet dnů:** každý den</span><span class="sxs-lookup"><span data-stu-id="cf651-289">**Days:** Everyday</span></span> |
| <span data-ttu-id="cf651-290">**Čas spuštění:** 9:00:00</span><span class="sxs-lookup"><span data-stu-id="cf651-290">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="cf651-291">**Časové pásmo:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="cf651-291">**Time zone:** UTC-08</span></span> |
|  |
| <span data-ttu-id="cf651-292">**Pravidlo automatického škálování (vertikálně navýšit kapacitu)**</span><span class="sxs-lookup"><span data-stu-id="cf651-292">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="cf651-293">**Prostředek:** Front-end fondu</span><span class="sxs-lookup"><span data-stu-id="cf651-293">**Resource:** Front-end pool</span></span> |
| <span data-ttu-id="cf651-294">**Metrika:** % využití procesoru</span><span class="sxs-lookup"><span data-stu-id="cf651-294">**Metric:** CPU %</span></span> |
| <span data-ttu-id="cf651-295">**Operace:** větší než 60 %</span><span class="sxs-lookup"><span data-stu-id="cf651-295">**Operation:** Greater than 60%</span></span> |
| <span data-ttu-id="cf651-296">**Doba trvání:** 20 minut</span><span class="sxs-lookup"><span data-stu-id="cf651-296">**Duration:** 20 minutes</span></span> |
| <span data-ttu-id="cf651-297">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="cf651-297">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="cf651-298">**Akce:** zvýšení počtu 3</span><span class="sxs-lookup"><span data-stu-id="cf651-298">**Action:** Increase count by 3</span></span> |
| <span data-ttu-id="cf651-299">**Nástrojů dolů (minuty):** 120.</span><span class="sxs-lookup"><span data-stu-id="cf651-299">**Cool down (minutes):** 120</span></span> |
|  |
| <span data-ttu-id="cf651-300">**Pravidlo automatického škálování (škálování dolů)**</span><span class="sxs-lookup"><span data-stu-id="cf651-300">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="cf651-301">**Prostředek:** pracovní fond 1</span><span class="sxs-lookup"><span data-stu-id="cf651-301">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="cf651-302">**Metrika:** % využití procesoru</span><span class="sxs-lookup"><span data-stu-id="cf651-302">**Metric:** CPU %</span></span> |
| <span data-ttu-id="cf651-303">**Operace:** méně než 30 %</span><span class="sxs-lookup"><span data-stu-id="cf651-303">**Operation:** Less than 30%</span></span> |
| <span data-ttu-id="cf651-304">**Doba trvání:** 20 minut</span><span class="sxs-lookup"><span data-stu-id="cf651-304">**Duration:** 20 Minutes</span></span> |
| <span data-ttu-id="cf651-305">**Čas agregace:** průměrná</span><span class="sxs-lookup"><span data-stu-id="cf651-305">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="cf651-306">**Akce:** snížit počet 3</span><span class="sxs-lookup"><span data-stu-id="cf651-306">**Action:** Decrease count by 3</span></span> |
| <span data-ttu-id="cf651-307">**Nástrojů dolů (minuty):** 120.</span><span class="sxs-lookup"><span data-stu-id="cf651-307">**Cool down (minutes):** 120</span></span> |

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
