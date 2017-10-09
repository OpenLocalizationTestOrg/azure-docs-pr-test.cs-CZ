---
title: "aaaGet začít s předkonfigurovanými řešeními | Microsoft Docs"
description: "Postupujte podle tohoto kurzu toolearn jak toodeploy Azure IoT Suite předkonfigurované řešení."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 6ab38d1a-b564-469e-8a87-e597aa51d0f7
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: a7f46023d26b08de2e8ed48c34c5066a43e3fa38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-preconfigured-solutions"></a><span data-ttu-id="15253-103">Začínáme s hello předkonfigurované řešení</span><span class="sxs-lookup"><span data-stu-id="15253-103">Get started with hello preconfigured solutions</span></span>

<span data-ttu-id="15253-104">Azure IoT Suite [předkonfigurovaných řešení] [ lnk-preconfigured-solutions] kombinovat více Azure IoT služby toodeliver začátku do konce řešení, implementující běžné obchodní scénáře IoT.</span><span class="sxs-lookup"><span data-stu-id="15253-104">Azure IoT Suite [preconfigured solutions][lnk-preconfigured-solutions] combine multiple Azure IoT services toodeliver end-to-end solutions that implement common IoT business scenarios.</span></span> <span data-ttu-id="15253-105">Hello *vzdálené monitorování* předkonfigurované řešení připojí tooand monitorování zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-105">hello *remote monitoring* preconfigured solution connects tooand monitors your devices.</span></span> <span data-ttu-id="15253-106">Můžete vytvořit hello řešení tooanalyze hello datový proud ze zařízení a výstupy obchodní tooimprove tím, že procesy reagovat automaticky toothat datový proud.</span><span class="sxs-lookup"><span data-stu-id="15253-106">You can use hello solution tooanalyze hello stream of data from your devices and tooimprove business outcomes by making processes respond automatically toothat stream of data.</span></span>

<span data-ttu-id="15253-107">Tento kurz ukazuje, jak tooprovision hello předkonfigurované řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="15253-107">This tutorial shows you how tooprovision hello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="15253-108">Je také vás provede procesem hello základní funkce hello předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="15253-108">It also walks you through hello basic features of hello preconfigured solution.</span></span> <span data-ttu-id="15253-109">Mnoho z těchto funkcí můžete přistupovat z řešení hello *řídicí panel* které nasazuje v rámci hello předkonfigurované řešení:</span><span class="sxs-lookup"><span data-stu-id="15253-109">You can access many of these features from hello solution *dashboard* that deploys as part of hello preconfigured solution:</span></span>

![Řídicí panel předkonfigurovaného řešení vzdáleného monitorování][img-dashboard]

<span data-ttu-id="15253-111">toocomplete tohoto kurzu potřebujete aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="15253-111">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="15253-112">Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební.</span><span class="sxs-lookup"><span data-stu-id="15253-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="15253-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="15253-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="scenario-overview"></a><span data-ttu-id="15253-114">Přehled scénáře</span><span class="sxs-lookup"><span data-stu-id="15253-114">Scenario overview</span></span>

<span data-ttu-id="15253-115">Když nasadíte předkonfigurované řešení vzdáleného sledování hello, je naplněna prostředky, které umožňují toostep prostřednictvím běžný scénář vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="15253-115">When you deploy hello remote monitoring preconfigured solution, it is prepopulated with resources that enable you toostep through a common remote monitoring scenario.</span></span> <span data-ttu-id="15253-116">V tomto scénáři jsou několik zařízení připojených toohello řešení reporting neočekávané teploty hodnoty.</span><span class="sxs-lookup"><span data-stu-id="15253-116">In this scenario, several devices connected toohello solution are reporting unexpected temperature values.</span></span> <span data-ttu-id="15253-117">Hello následující části ukazují, jak na:</span><span class="sxs-lookup"><span data-stu-id="15253-117">hello following sections show you how to:</span></span>

* <span data-ttu-id="15253-118">Identifikujte zařízení hello odesílání neočekávané teploty hodnoty.</span><span class="sxs-lookup"><span data-stu-id="15253-118">Identify hello devices sending unexpected temperature values.</span></span>
* <span data-ttu-id="15253-119">Konfigurace těchto zařízení toosend podrobnější telemetrie.</span><span class="sxs-lookup"><span data-stu-id="15253-119">Configure these devices toosend more detailed telemetry.</span></span>
* <span data-ttu-id="15253-120">Hello problém vyřešte aktualizací firmwaru hello v těchto zařízeních.</span><span class="sxs-lookup"><span data-stu-id="15253-120">Fix hello problem by updating hello firmware on these devices.</span></span>
* <span data-ttu-id="15253-121">Ověřte, že vaše akce rozpoznal hello problém.</span><span class="sxs-lookup"><span data-stu-id="15253-121">Verify that your action has resolved hello issue.</span></span>

<span data-ttu-id="15253-122">Klíčové funkce tento scénář je, že můžete provádět tyto akce vzdáleně z řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="15253-122">A key feature of this scenario is that you can perform all these actions remotely from hello solution dashboard.</span></span> <span data-ttu-id="15253-123">Není nutné zařízení toohello fyzický přístup.</span><span class="sxs-lookup"><span data-stu-id="15253-123">You do not need physical access toohello devices.</span></span>

## <a name="view-hello-solution-dashboard"></a><span data-ttu-id="15253-124">Zobrazení řídicí panel řešení hello</span><span class="sxs-lookup"><span data-stu-id="15253-124">View hello solution dashboard</span></span>

<span data-ttu-id="15253-125">řídicí panel řešení Hello umožňuje toomanage hello nasazené řešení.</span><span class="sxs-lookup"><span data-stu-id="15253-125">hello solution dashboard enables you toomanage hello deployed solution.</span></span> <span data-ttu-id="15253-126">Můžete například zobrazit telemetrická data, přidat zařízení nebo konfigurovat pravidla.</span><span class="sxs-lookup"><span data-stu-id="15253-126">For example, you can view telemetry, add devices, and configure rules.</span></span>

1. <span data-ttu-id="15253-127">Když je hello zřizování dokončeno a dlaždice hello pro předkonfigurované řešení označuje **připravené**, zvolte **spusťte** tooopen vzdálené monitorování řešení portálu na nové kartě.</span><span class="sxs-lookup"><span data-stu-id="15253-127">When hello provisioning is complete and hello tile for your preconfigured solution indicates **Ready**, choose **Launch** tooopen your remote monitoring solution portal in a new tab.</span></span>

    ![Spusťte hello předkonfigurované řešení][img-launch-solution]

1. <span data-ttu-id="15253-129">Ve výchozím nastavení, hello portálu řešení zobrazuje hello *řídicí panel*.</span><span class="sxs-lookup"><span data-stu-id="15253-129">By default, hello solution portal shows hello *dashboard*.</span></span> <span data-ttu-id="15253-130">Můžete procházet tooother oblasti hello portál řešení pomocí hello nabídky na levé straně stránky hello hello.</span><span class="sxs-lookup"><span data-stu-id="15253-130">You can navigate tooother areas of hello solution portal using hello menu on hello left-hand side of hello page.</span></span>

    ![Řídicí panel předkonfigurovaného řešení vzdáleného monitorování][img-menu]

<span data-ttu-id="15253-132">řídicí panel Hello zobrazuje hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="15253-132">hello dashboard displays hello following information:</span></span>

* <span data-ttu-id="15253-133">Mapu, která zobrazuje umístění každého zařízení hello připojen toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="15253-133">A map that displays hello location of each device connected toohello solution.</span></span> <span data-ttu-id="15253-134">Při prvním spuštění hello řešení, nejsou 25 simulované zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-134">When you first run hello solution, there are 25 simulated devices.</span></span> <span data-ttu-id="15253-135">Hello Simulovaná zařízení jsou implementována jako Azure WebJobs a řešení hello používá hello rozhraní API map Bing tooplot informace na mapě hello.</span><span class="sxs-lookup"><span data-stu-id="15253-135">hello simulated devices are implemented as Azure WebJobs, and hello solution uses hello Bing Maps API tooplot information on hello map.</span></span> <span data-ttu-id="15253-136">V tématu hello [– nejčastější dotazy] [ lnk-faq] toolearn jak toomake hello dynamické mapy.</span><span class="sxs-lookup"><span data-stu-id="15253-136">See hello [FAQ][lnk-faq] toolearn how toomake hello map dynamic.</span></span>
* <span data-ttu-id="15253-137">Panel **Historie telemetrie**, na kterém se skoro v reálném čase zobrazuje telemetrie vlhkosti a teploty z vybraného zařízení a agregovaná data (například minimální, maximální a průměrná vlhkost).</span><span class="sxs-lookup"><span data-stu-id="15253-137">A **Telemetry History** panel that plots humidity and temperature telemetry from a selected device in near real time and displays aggregate data such as maximum, minimum, and average humidity.</span></span>
* <span data-ttu-id="15253-138">Panel **Historie alarmů**, který zobrazuje události, kdy v poslední době hodnota telemetrie překročila stanovenou mez.</span><span class="sxs-lookup"><span data-stu-id="15253-138">An **Alarm History** panel that shows recent alarm events when a telemetry value has exceeded a threshold.</span></span> <span data-ttu-id="15253-139">V příkladech toohello přidání vytvořené hello předkonfigurované řešení můžete definovat vlastní alarmy.</span><span class="sxs-lookup"><span data-stu-id="15253-139">You can define your own alarms in addition toohello examples created by hello preconfigured solution.</span></span>
* <span data-ttu-id="15253-140">Panel **Úlohy**, na kterém se zobrazují informace o plánovaných úlohách.</span><span class="sxs-lookup"><span data-stu-id="15253-140">A **Jobs** panel that displays information about scheduled jobs.</span></span> <span data-ttu-id="15253-141">Vlastní úlohy můžete plánovat na stránce **Úlohy správy**.</span><span class="sxs-lookup"><span data-stu-id="15253-141">You can schedule your own jobs on **Management jobs** page.</span></span>

## <a name="view-alarms"></a><span data-ttu-id="15253-142">Zobrazení alarmů</span><span class="sxs-lookup"><span data-stu-id="15253-142">View alarms</span></span>

<span data-ttu-id="15253-143">panel Historie alarmů Hello ukazuje, že pěti zařízení podávají zprávy vyšší než očekávaný telemetrie hodnoty.</span><span class="sxs-lookup"><span data-stu-id="15253-143">hello alarm history panel shows you that five devices are reporting higher than expected telemetry values.</span></span>

![Historie alarmů úkolů na řídicí panel řešení hello][img-alarms]

> [!NOTE]
> <span data-ttu-id="15253-145">Tyto výstrahy jsou generovány pravidlem, které je součástí hello předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="15253-145">These alarms are generated by a rule that is included in hello preconfigured solution.</span></span> <span data-ttu-id="15253-146">Toto pravidlo vygeneruje výstrahu, pokud hodnota teploty hello odeslaný zařízením překročí 60.</span><span class="sxs-lookup"><span data-stu-id="15253-146">This rule generates an alert when hello temperature value sent by a device exceeds 60.</span></span> <span data-ttu-id="15253-147">Můžete definovat vlastní pravidla a akce výběrem [pravidla](#add-a-rule) a [akce](#add-an-action) v levé nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="15253-147">You can define your own rules and actions by choosing [Rules](#add-a-rule) and [Actions](#add-an-action) in hello left-hand menu.</span></span>

## <a name="view-devices"></a><span data-ttu-id="15253-148">Zobrazení zařízení</span><span class="sxs-lookup"><span data-stu-id="15253-148">View devices</span></span>

<span data-ttu-id="15253-149">Hello *zařízení* seznamu jsou uvedeny všechny hello zaregistrovat zařízení v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="15253-149">hello *devices* list shows all hello registered devices in hello solution.</span></span> <span data-ttu-id="15253-150">Ze seznamu zařízení hello můžete zobrazit a upravit metadata zařízení přidejte nebo odeberte zařízení a volat metody na zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-150">From hello device list you can view and edit device metadata, add or remove devices, and invoke methods on devices.</span></span> <span data-ttu-id="15253-151">Můžete filtrovat a řadit hello seznam zařízení v seznamu zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="15253-151">You can filter and sort hello list of devices in hello device list.</span></span> <span data-ttu-id="15253-152">Můžete také upravit hello sloupce zobrazené v seznamu zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="15253-152">You can also customize hello columns shown in hello device list.</span></span>

1. <span data-ttu-id="15253-153">Zvolte **zařízení** tooshow hello seznam zařízení pro toto řešení.</span><span class="sxs-lookup"><span data-stu-id="15253-153">Choose **Devices** tooshow hello device list for this solution.</span></span>

   ![Zobrazení seznamu zařízení hello portálu řešení hello][img-devicelist]

1. <span data-ttu-id="15253-155">seznam zařízení Hello původně ukazuje 25 Simulovaná zařízení, které jsou vytvořené hello procesu zřizování.</span><span class="sxs-lookup"><span data-stu-id="15253-155">hello device list initially shows 25 simulated devices created by hello provisioning process.</span></span> <span data-ttu-id="15253-156">Můžete přidat další Simulovaná a fyzické zařízení toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="15253-156">You can add additional simulated and physical devices toohello solution.</span></span>

1. <span data-ttu-id="15253-157">v seznamu zařízení hello tooview hello podrobnosti o zařízení, vyberte zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-157">tooview hello details of a device, choose a device in hello device list.</span></span>

   ![Zobrazení podrobností o zařízeních hello portálu řešení hello][img-devicedetails]

<span data-ttu-id="15253-159">Hello **podrobnosti o zařízení** panel obsahuje šest části:</span><span class="sxs-lookup"><span data-stu-id="15253-159">hello **Device Details** panel contains six sections:</span></span>

* <span data-ttu-id="15253-160">Kolekce odkazů, které jste toocustomize hello ikonu zařízení povolit, zakázat hello zařízení, přidat pravidlo, volání metody nebo odeslat příkaz.</span><span class="sxs-lookup"><span data-stu-id="15253-160">A collection of links that enable you toocustomize hello device icon, disable hello device, add a rule, invoke a method, or send a command.</span></span> <span data-ttu-id="15253-161">Porovnání příkazů (zpráv typu zařízení-cloud) a metod (přímých metod) najdete v [doprovodných materiálech ke komunikaci typu cloud-zařízení][lnk-c2d-guidance].</span><span class="sxs-lookup"><span data-stu-id="15253-161">For a comparison of commands (device-to-cloud messages) and methods (direct methods), see [Cloud-to-device communications guidance][lnk-c2d-guidance].</span></span>
* <span data-ttu-id="15253-162">Hello **dvojče zařízení - značky** oddílu můžete hodnoty značky tooedit hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-162">hello **Device Twin - Tags** section enables you tooedit tag values for hello device.</span></span> <span data-ttu-id="15253-163">Můžete zobrazit v seznamu zařízení hello hodnoty značky a použít seznam zařízení hello toofilter hodnoty značky.</span><span class="sxs-lookup"><span data-stu-id="15253-163">You can display tag values in hello device list and use tag values toofilter hello device list.</span></span>
* <span data-ttu-id="15253-164">Hello **dvojče zařízení - potřeby vlastnosti** oddílu můžete tooset vlastnost hodnoty toobe odeslané toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-164">hello **Device Twin - Desired Properties** section enables you tooset property values toobe sent toohello device.</span></span>
* <span data-ttu-id="15253-165">Hello **dvojče zařízení - hlášené vlastnosti** část zobrazuje hodnoty vlastností, odeslané zařízením hello.</span><span class="sxs-lookup"><span data-stu-id="15253-165">hello **Device Twin - Reported Properties** section shows property values sent from hello device.</span></span>
* <span data-ttu-id="15253-166">Hello **vlastnosti zařízení** části se zobrazují informace z registru identit hello například hello zařízení id a ověřovací klíč.</span><span class="sxs-lookup"><span data-stu-id="15253-166">hello **Device Properties** section shows information from hello identity registry such as hello device id and authentication keys.</span></span>
* <span data-ttu-id="15253-167">Hello **posledních úloh** části zobrazují informace o všechny úlohy, které jste nedávno zaměřili toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-167">hello **Recent Jobs** section shows information about any jobs that have recently targeted this device.</span></span>

## <a name="filter-hello-device-list"></a><span data-ttu-id="15253-168">Filtrovat seznam zařízení hello</span><span class="sxs-lookup"><span data-stu-id="15253-168">Filter hello device list</span></span>

<span data-ttu-id="15253-169">Toodisplay filtru můžete použít pouze taková zařízení, která odesílají neočekávané teploty hodnoty.</span><span class="sxs-lookup"><span data-stu-id="15253-169">You can use a filter toodisplay only those devices that are sending unexpected temperature values.</span></span> <span data-ttu-id="15253-170">Hello předkonfigurovanému řešení vzdáleného monitorování zahrnuje hello **není v pořádku zařízení** filtrovat tooshow zařízení s hodnotou střední teploty větší než 60.</span><span class="sxs-lookup"><span data-stu-id="15253-170">hello remote monitoring preconfigured solution includes hello **Unhealthy devices** filter tooshow devices with a mean temperature value greater than 60.</span></span> <span data-ttu-id="15253-171">Můžete si také [vytvořit vlastní filtry](#add-a-filter).</span><span class="sxs-lookup"><span data-stu-id="15253-171">You can also [create your own filters](#add-a-filter).</span></span>

1. <span data-ttu-id="15253-172">Zvolte **otevřít uložené filtru** toodisplay seznam k dispozici tyto filtry.</span><span class="sxs-lookup"><span data-stu-id="15253-172">Choose **Open saved filter** toodisplay a list of available filters.</span></span> <span data-ttu-id="15253-173">Zvolte **není v pořádku zařízení** tooapply hello filtru:</span><span class="sxs-lookup"><span data-stu-id="15253-173">Then choose **Unhealthy devices** tooapply hello filter:</span></span>

    ![Zobrazení hello seznam filtrů][img-unhealthy-filter]

1. <span data-ttu-id="15253-175">seznam zařízení Hello nyní zobrazuje pouze zařízení s střední teploty hodnotu větší než 60.</span><span class="sxs-lookup"><span data-stu-id="15253-175">hello device list now shows only devices with a mean temperature value greater than 60.</span></span>

    ![Seznam Filtrované zařízení hello zobrazení zobrazující zařízení není v pořádku][img-filtered-unhealthy-list]

## <a name="update-desired-properties"></a><span data-ttu-id="15253-177">Aktualizace požadovaných vlastností</span><span class="sxs-lookup"><span data-stu-id="15253-177">Update desired properties</span></span>

<span data-ttu-id="15253-178">Nyní jste identifikovali sadu zařízení, která možná vyžadují opravu.</span><span class="sxs-lookup"><span data-stu-id="15253-178">You have now identified a set of devices that may need remediation.</span></span> <span data-ttu-id="15253-179">Však můžete rozhodnout, že data frekvenci hello 15 sekund není dostatečná pro zrušte diagnostiku problému hello.</span><span class="sxs-lookup"><span data-stu-id="15253-179">However, you decide that hello data frequency of 15 seconds is not sufficient for a clear diagnosis of hello issue.</span></span> <span data-ttu-id="15253-180">Změna hello telemetrie frekvence toofive sekund tooprovide vám další datové body toobetter diagnostikovat problém hello.</span><span class="sxs-lookup"><span data-stu-id="15253-180">Changing hello telemetry frequency toofive seconds tooprovide you with more data points toobetter diagnose hello issue.</span></span> <span data-ttu-id="15253-181">Vzdálená zařízení tuto konfiguraci změnu tooyour můžete nabízet z portálu řešení hello.</span><span class="sxs-lookup"><span data-stu-id="15253-181">You can push this configuration change tooyour remote devices from hello solution portal.</span></span> <span data-ttu-id="15253-182">Můžete provést změnu hello jednou, vyhodnotit vliv hello a potom na výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="15253-182">You can make hello change once, evaluate hello impact, and then act on hello results.</span></span>

<span data-ttu-id="15253-183">Postupujte podle těchto kroků toorun úlohu, která změní hello **TelemetryInterval** potřeby vlastnost hello vliv na zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-183">Follow these steps toorun a job that changes hello **TelemetryInterval** desired property for hello affected devices.</span></span> <span data-ttu-id="15253-184">Když hello zařízení obdrží hello nové **TelemetryInterval** hodnotu vlastnosti, se změní, jejich konfigurace toosend telemetrie každých pět sekund místo každých 15 sekund:</span><span class="sxs-lookup"><span data-stu-id="15253-184">When hello devices receive hello new **TelemetryInterval** property value, they change their configuration toosend telemetry every five seconds instead of every 15 seconds:</span></span>

1. <span data-ttu-id="15253-185">Při hello seznam zařízení, není v pořádku zobrazené v seznamu zařízení hello, zvolte **plánovače úloh**, pak **upravit dvojče zařízení**.</span><span class="sxs-lookup"><span data-stu-id="15253-185">While you are showing hello list of unhealthy devices in hello device list, choose **Job Scheduler**, then **Edit Device Twin**.</span></span>

1. <span data-ttu-id="15253-186">Volání hello úlohy **interval telemetrie změny**.</span><span class="sxs-lookup"><span data-stu-id="15253-186">Call hello job **Change telemetry interval**.</span></span>

1. <span data-ttu-id="15253-187">Změnit hodnotu hello hello **požadovanou vlastnost** název **požadované. Config.TelemetryInterval** toofive sekund.</span><span class="sxs-lookup"><span data-stu-id="15253-187">Change hello value of hello **Desired Property** name **desired.Config.TelemetryInterval** toofive seconds.</span></span>

1. <span data-ttu-id="15253-188">Zvolte **Naplánovat**.</span><span class="sxs-lookup"><span data-stu-id="15253-188">Choose **Schedule**.</span></span>

    ![Změnit hello TelemetryInterval vlastnost toofive sekund][img-change-interval]

1. <span data-ttu-id="15253-190">Můžete sledovat průběh hello hello úlohy na hello **úlohy správy** stránku hello portálu.</span><span class="sxs-lookup"><span data-stu-id="15253-190">You can monitor hello progress of hello job on hello **Management Jobs** page in hello portal.</span></span>

> [!NOTE]
> <span data-ttu-id="15253-191">Pokud chcete toochange hodnotu požadované vlastnosti pro jednotlivá zařízení, použijte hello **požadované vlastnosti** část v hello **podrobnosti o zařízení** panely namísto spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="15253-191">If you want toochange a desired property value for an individual device, use hello **Desired Properties** section in hello **Device Details** panel instead of running a job.</span></span>

<span data-ttu-id="15253-192">Tato úloha nastaví hodnotu hello hello **TelemetryInterval** potřeby vlastnost hello dvojče zařízení pro všechny hello filtrem hello vybraná zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-192">This job sets hello value of hello **TelemetryInterval** desired property in hello device twin for all hello devices selected by hello filter.</span></span> <span data-ttu-id="15253-193">zařízení Hello načetlo tuto hodnotu z dvojče zařízení hello a aktualizaci jejich chování.</span><span class="sxs-lookup"><span data-stu-id="15253-193">hello devices retrieve this value from hello device twin and update their behavior.</span></span> <span data-ttu-id="15253-194">Když zařízení načte a zpracuje požadovanou vlastnost z dvojče zařízení, nastaví hello odpovídající hlášené hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="15253-194">When a device retrieves and processes a desired property from a device twin, it sets hello corresponding reported value property.</span></span>

## <a name="invoke-methods"></a><span data-ttu-id="15253-195">Vyvolání metod</span><span class="sxs-lookup"><span data-stu-id="15253-195">Invoke methods</span></span>

<span data-ttu-id="15253-196">Při spuštění úlohy hello zjistíte v seznamu hello zařízení není v pořádku, že tato zařízení mají starý (méně než verze 1.6) firmwaru verze.</span><span class="sxs-lookup"><span data-stu-id="15253-196">While hello job runs, you notice in hello list of unhealthy devices that all these devices have old (less than version 1.6) firmware versions.</span></span>

![Zobrazení hello hlášené verzi firmwaru pro zařízení není v pořádku hello][img-old-firmware]

<span data-ttu-id="15253-198">Tato verze firmwaru může být hello hlavní příčinu hello neočekávané teploty hodnoty, protože víte, že další pořádku zařízení byly nedávno aktualizovaného tooversion 2.0.</span><span class="sxs-lookup"><span data-stu-id="15253-198">This firmware version may be hello root cause of hello unexpected temperature values because you know that other healthy devices were recently updated tooversion 2.0.</span></span> <span data-ttu-id="15253-199">Můžete použít předdefinované hello **staré firmwaru zařízení** filtrovat tooidentify žádná zařízení s staré verze firmwaru.</span><span class="sxs-lookup"><span data-stu-id="15253-199">You can use hello built-in **Old firmware devices** filter tooidentify any devices with old firmware versions.</span></span> <span data-ttu-id="15253-200">Z portálu hello může vzdáleně aktualizovat všechna zařízení hello stále spuštěna staré verze firmwaru:</span><span class="sxs-lookup"><span data-stu-id="15253-200">From hello portal, you can then remotely update all hello devices still running old firmware versions:</span></span>

1. <span data-ttu-id="15253-201">Zvolte **otevřít uložené filtru** toodisplay seznam k dispozici tyto filtry.</span><span class="sxs-lookup"><span data-stu-id="15253-201">Choose **Open saved filter** toodisplay a list of available filters.</span></span> <span data-ttu-id="15253-202">Zvolte **staré firmwaru zařízení** tooapply hello filtru:</span><span class="sxs-lookup"><span data-stu-id="15253-202">Then choose **Old firmware devices** tooapply hello filter:</span></span>

    ![Zobrazení hello seznam filtrů][img-old-filter]

1. <span data-ttu-id="15253-204">seznam zařízení Hello nyní zobrazuje pouze zařízení s staré verze firmwaru.</span><span class="sxs-lookup"><span data-stu-id="15253-204">hello device list now shows only devices with old firmware versions.</span></span> <span data-ttu-id="15253-205">Tento seznam obsahuje pět zařízení hello identifikovaný hello **není v pořádku zařízení** filtr a tři další zařízení:</span><span class="sxs-lookup"><span data-stu-id="15253-205">This list includes hello five devices identified by hello **Unhealthy devices** filter and three additional devices:</span></span>

    ![Seznam Filtrované zařízení hello zobrazení zobrazující staré zařízení][img-filtered-old-list]

1. <span data-ttu-id="15253-207">Zvolte **Plánovač úloh** a potom **Vyvolat metodu**.</span><span class="sxs-lookup"><span data-stu-id="15253-207">Choose **Job Scheduler**, then **Invoke Method**.</span></span>

1. <span data-ttu-id="15253-208">Nastavit **název úlohy** příliš**tooversion aktualizace firmwaru 2.0**.</span><span class="sxs-lookup"><span data-stu-id="15253-208">Set **Job Name** too**Firmware update tooversion 2.0**.</span></span>

1. <span data-ttu-id="15253-209">Zvolte **InitiateFirmwareUpdate** jako hello **metoda**.</span><span class="sxs-lookup"><span data-stu-id="15253-209">Choose **InitiateFirmwareUpdate** as hello **Method**.</span></span>

1. <span data-ttu-id="15253-210">Sada hello **FwPackageUri** parametr příliš**https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.</span><span class="sxs-lookup"><span data-stu-id="15253-210">Set hello **FwPackageUri** parameter too**https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.</span></span>

1. <span data-ttu-id="15253-211">Zvolte **Naplánovat**.</span><span class="sxs-lookup"><span data-stu-id="15253-211">Choose **Schedule**.</span></span> <span data-ttu-id="15253-212">Výchozí Hello je nyní pro úlohy toorun hello.</span><span class="sxs-lookup"><span data-stu-id="15253-212">hello default is for hello job toorun now.</span></span>

    ![Vytvoření úlohy tooupdate hello firmwaru hello vybrané zařízení][img-method-update]

> [!NOTE]
> <span data-ttu-id="15253-214">Pokud chcete, aby tooinvoke metoda na jednotlivá zařízení, zvolte **metody** v hello **podrobnosti o zařízení** panely namísto spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="15253-214">If you want tooinvoke a method on an individual device, choose **Methods** in hello **Device Details** panel instead of running a job.</span></span>

<span data-ttu-id="15253-215">Tato úloha vyvolá hello **InitiateFirmwareUpdate** přímá metoda na všech zařízeních hello vybraný filtr hello.</span><span class="sxs-lookup"><span data-stu-id="15253-215">This job invokes hello **InitiateFirmwareUpdate** direct method on all hello devices selected by hello filter.</span></span> <span data-ttu-id="15253-216">Zařízení reagovat okamžitě tooIoT rozbočovače a poté asynchronně zahájit proces aktualizace firmwaru hello.</span><span class="sxs-lookup"><span data-stu-id="15253-216">Devices respond immediately tooIoT Hub and then initiate hello firmware update process asynchronously.</span></span> <span data-ttu-id="15253-217">zařízení Hello poskytují informace o stavu procesu aktualizace firmwaru hello prostřednictvím hodnoty hlášené vlastností, jak je znázorněno v následujícím snímky obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="15253-217">hello devices provide status information about hello firmware update process through reported property values, as shown in hello following screenshots.</span></span> <span data-ttu-id="15253-218">Zvolte hello **aktualizovat** ikonu tooupdate hello informace v seznamech zařízení a úlohy hello:</span><span class="sxs-lookup"><span data-stu-id="15253-218">Choose hello **Refresh** icon tooupdate hello information in hello device and job lists:</span></span>

<span data-ttu-id="15253-219">![Seznam úloh zobrazující hello firmware aktualizace seznamu spuštěná][img-update-1]
![seznam zařízení zobrazuje stav aktualizace firmwaru][img-update-2]
![úlohy seznamu zobrazení hello firmware aktualizace seznamu dokončení][img-update-3]</span><span class="sxs-lookup"><span data-stu-id="15253-219">![Job list showing hello firmware update list running][img-update-1]
![Device list showing firmware update status][img-update-2]
![Job list showing hello firmware update list complete][img-update-3]</span></span>

> [!NOTE]
> <span data-ttu-id="15253-220">V produkčním prostředí můžete naplánovat úlohy toorun během časového období údržby určené.</span><span class="sxs-lookup"><span data-stu-id="15253-220">In a production environment, you can schedule jobs toorun during a designated maintenance window.</span></span>

## <a name="scenario-review"></a><span data-ttu-id="15253-221">Revize scénáře</span><span class="sxs-lookup"><span data-stu-id="15253-221">Scenario review</span></span>

<span data-ttu-id="15253-222">V tomto scénáři identifikovat potenciální problém s některými vzdáleného zařízení pomocí hello historie alarmů na řídicím panelu hello a filtr.</span><span class="sxs-lookup"><span data-stu-id="15253-222">In this scenario, you identified a potential issue with some of your remote devices using hello alarm history on hello dashboard and a filter.</span></span> <span data-ttu-id="15253-223">Můžete potom použít hello filtru a tooremotely úlohy konfigurace hello zařízení tooprovide Další informace o toohelp diagnostikovat problém hello.</span><span class="sxs-lookup"><span data-stu-id="15253-223">You then used hello filter and a job tooremotely configure hello devices tooprovide more information toohelp diagnose hello issue.</span></span> <span data-ttu-id="15253-224">Nakonec použít filtr a úlohy údržby tooschedule na zařízeních hello vliv.</span><span class="sxs-lookup"><span data-stu-id="15253-224">Finally, you used a filter and a job tooschedule maintenance on hello affected devices.</span></span> <span data-ttu-id="15253-225">Pokud vrátíte toohello řídicího panelu, můžete zkontrolovat, že již neexistují žádné výstrahy přicházející ze zařízení ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="15253-225">If you return toohello dashboard, you can check that there are no longer any alarms coming from devices in your solution.</span></span> <span data-ttu-id="15253-226">Můžete použít filtr tooverify, který hello firmware aktuální na všech zařízeních hello ve vašem řešení a jestli je k dispozici nejsou žádné další není v pořádku zařízení:</span><span class="sxs-lookup"><span data-stu-id="15253-226">You can use a filter tooverify that hello firmware is up-to-date on all hello devices in your solution and that there are no more unhealthy devices:</span></span>

![Filtr ukazující, že všechna zařízení mají aktuální firmware][img-updated]

![Filtr ukazující, že všechna zařízení jsou v pořádku][img-healthy]

## <a name="other-features"></a><span data-ttu-id="15253-229">Další funkce</span><span class="sxs-lookup"><span data-stu-id="15253-229">Other features</span></span>

<span data-ttu-id="15253-230">Hello následující části popisují některé další funkce hello předkonfigurovanému řešení vzdáleného monitorování, které nejsou popsané v rámci předchozí scénáře hello.</span><span class="sxs-lookup"><span data-stu-id="15253-230">hello following sections describe some additional features of hello remote monitoring preconfigured solution that are not described as part of hello previous scenario.</span></span>

### <a name="customize-columns"></a><span data-ttu-id="15253-231">Přizpůsobení sloupců</span><span class="sxs-lookup"><span data-stu-id="15253-231">Customize columns</span></span>

<span data-ttu-id="15253-232">Můžete přizpůsobit hello informace zobrazené v seznamu zařízení hello výběrem **editor sloupců**.</span><span class="sxs-lookup"><span data-stu-id="15253-232">You can customize hello information shown in hello device list by choosing **Column editor**.</span></span> <span data-ttu-id="15253-233">Můžete přidat a odebrat sloupce, které zobrazují hodnoty ohlašovaných vlastností a značek.</span><span class="sxs-lookup"><span data-stu-id="15253-233">You can add and remove columns that display reported property and tag values.</span></span> <span data-ttu-id="15253-234">Můžete také změnit pořadí sloupců a přejmenovat je:</span><span class="sxs-lookup"><span data-stu-id="15253-234">You can also reorder and rename columns:</span></span>

   ![Seznam zařízení sloupec editor uchování hello][img-columneditor]

### <a name="customize-hello-device-icon"></a><span data-ttu-id="15253-236">Přizpůsobení ikonu zařízení hello</span><span class="sxs-lookup"><span data-stu-id="15253-236">Customize hello device icon</span></span>

<span data-ttu-id="15253-237">Můžete přizpůsobit ikonu hello zařízení zobrazí v seznamu zařízení hello z hello **podrobnosti o zařízení** panelu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="15253-237">You can customize hello device icon displayed in hello device list from hello **Device Details** panel as follows:</span></span>

1. <span data-ttu-id="15253-238">Zvolte hello tooopen ikonu tužky hello **úpravy image** panel pro zařízení:</span><span class="sxs-lookup"><span data-stu-id="15253-238">Choose hello pencil icon tooopen hello **Edit image** panel for a device:</span></span>

   ![Otevřený editor obrázku zařízení][img-startimageedit]

1. <span data-ttu-id="15253-240">Nahrát novou bitovou kopii, použijte jednu z existujících imagí hello a potom zvolte **Uložit**:</span><span class="sxs-lookup"><span data-stu-id="15253-240">Either upload a new image or use one of hello existing images and then choose **Save**:</span></span>

   ![Editor Upravit obrázek zařízení][img-imageedit]

1. <span data-ttu-id="15253-242">Hello bitové kopie, které jste vybrali teď zobrazí v hello **ikonu** sloupec pro hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-242">hello image you selected now displays in hello **Icon** column for hello device.</span></span>

> [!NOTE]
> <span data-ttu-id="15253-243">Hello bitová kopie uložena v úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="15253-243">hello image is stored in blob storage.</span></span> <span data-ttu-id="15253-244">Značku dvojče zařízení hello obsahuje bitovou kopii odkaz toohello v úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="15253-244">A tag in hello device twin contains a link toohello image in blob storage.</span></span>

### <a name="add-a-device"></a><span data-ttu-id="15253-245">Přidání zařízení</span><span class="sxs-lookup"><span data-stu-id="15253-245">Add a device</span></span>

<span data-ttu-id="15253-246">Když nasadíte hello předkonfigurované řešení, automaticky zřizovat 25 ukázka zařízení, které se zobrazí v seznamu zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="15253-246">When you deploy hello preconfigured solution, you automatically provision 25 sample devices that you can see in hello device list.</span></span> <span data-ttu-id="15253-247">Tato zařízení jsou *simulovaná zařízení*, která běží ve webové úloze Azure.</span><span class="sxs-lookup"><span data-stu-id="15253-247">These devices are *simulated devices* running in an Azure WebJob.</span></span> <span data-ttu-id="15253-248">Simulovaná zařízení usnadnění pro vás tooexperiment hello předkonfigurované řešení bez hello nutné toodeploy skutečné, fyzické zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-248">Simulated devices make it easy for you tooexperiment with hello preconfigured solution without hello need toodeploy real, physical devices.</span></span> <span data-ttu-id="15253-249">Pokud chcete tooconnect řešení toohello skutečné zařízení, najdete v části hello [připojit vaše zařízení toohello předkonfigurovanému řešení vzdáleného monitorování] [ lnk-connect-rm] kurzu.</span><span class="sxs-lookup"><span data-stu-id="15253-249">If you do want tooconnect a real device toohello solution, see hello [Connect your device toohello remote monitoring preconfigured solution][lnk-connect-rm] tutorial.</span></span>

<span data-ttu-id="15253-250">Hello následující kroky ukazují, jak tooadd řešení toohello simulované zařízení:</span><span class="sxs-lookup"><span data-stu-id="15253-250">hello following steps show you how tooadd a simulated device toohello solution:</span></span>

1. <span data-ttu-id="15253-251">Přejděte zpět toohello seznam zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-251">Navigate back toohello device list.</span></span>

1. <span data-ttu-id="15253-252">tooadd zařízení, zvolte **+ přidat zařízení** v levém dolním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="15253-252">tooadd a device, choose **+ Add A Device** in hello bottom left corner.</span></span>

   ![Přidat zařízení toohello předkonfigurované řešení][img-adddevice]

1. <span data-ttu-id="15253-254">Zvolte **přidat nové** na hello **simulované zařízení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="15253-254">Choose **Add New** on hello **Simulated Device** tile.</span></span>

   ![Nastavení podrobností o zařízení na řídicím panelu][img-addnew]

   <span data-ttu-id="15253-256">V toocreating přidání nového simulovaného zařízení, můžete také přidat fyzického zařízení. Pokud se rozhodnete toocreate **vlastní zařízení**.</span><span class="sxs-lookup"><span data-stu-id="15253-256">In addition toocreating a new simulated device, you can also add a physical device if you choose toocreate a **Custom Device**.</span></span> <span data-ttu-id="15253-257">toolearn Další informace o propojení řešení toohello fyzických zařízení, najdete v části [připojit vaše zařízení toohello předkonfigurované řešení vzdáleného sledování IoT Suite][lnk-connect-rm].</span><span class="sxs-lookup"><span data-stu-id="15253-257">toolearn more about connecting physical devices toohello solution, see [Connect your device toohello IoT Suite remote monitoring preconfigured solution][lnk-connect-rm].</span></span>

1. <span data-ttu-id="15253-258">Vyberte možnost **Ručně definovat vlastní ID zařízení** a zadejte jedinečný název zařízení, například **mydevice_01**.</span><span class="sxs-lookup"><span data-stu-id="15253-258">Select **Let me define my own Device ID**, and enter a unique device ID name such as **mydevice_01**.</span></span>

1. <span data-ttu-id="15253-259">Zvolte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="15253-259">Choose **Create**.</span></span>

   ![Uložení nového zařízení][img-definedevice]

1. <span data-ttu-id="15253-261">V kroku 3 **přidání simulovaného zařízení**, zvolte **provádí** tooreturn toohello zařízení seznamu.</span><span class="sxs-lookup"><span data-stu-id="15253-261">In step 3 of **Add a simulated device**, choose **Done** tooreturn toohello device list.</span></span>

1. <span data-ttu-id="15253-262">Můžete zobrazit zařízení **systémem** v seznamu zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="15253-262">You can view your device **Running** in hello device list.</span></span>

    ![Zobrazení nového zařízení v seznamu zařízení][img-runningnew]

1. <span data-ttu-id="15253-264">Můžete také zobrazit telemetrii z nového zařízení na řídicím panelu hello simulated hello:</span><span class="sxs-lookup"><span data-stu-id="15253-264">You can also view hello simulated telemetry from your new device on hello dashboard:</span></span>

    ![Zobrazení telemetrie z nového zařízení][img-runningnew-2]

### <a name="disable-and-delete-a-device"></a><span data-ttu-id="15253-266">Zakázání a odstranění zařízení</span><span class="sxs-lookup"><span data-stu-id="15253-266">Disable and delete a device</span></span>

<span data-ttu-id="15253-267">Zařízení můžete zakázat, zakázané zařízení lze následně odebrat:</span><span class="sxs-lookup"><span data-stu-id="15253-267">You can disable a device, and after it is disabled you can remove it:</span></span>

![Zakázání a odebrání zařízení][img-disable]

### <a name="add-a-rule"></a><span data-ttu-id="15253-269">Přidání pravidla</span><span class="sxs-lookup"><span data-stu-id="15253-269">Add a rule</span></span>

<span data-ttu-id="15253-270">Neexistují žádná pravidla pro nové zařízení hello, že jste právě přidali.</span><span class="sxs-lookup"><span data-stu-id="15253-270">There are no rules for hello new device you just added.</span></span> <span data-ttu-id="15253-271">V této části přidáte pravidlo, které spustí alarm při hello teplota hlášená tímto hello nové, že zařízení přesáhne hodnotu 47 stupňů.</span><span class="sxs-lookup"><span data-stu-id="15253-271">In this section, you add a rule that triggers an alarm when hello temperature reported by hello new device exceeds 47 degrees.</span></span> <span data-ttu-id="15253-272">Než začnete, Všimněte si, že historie telemetrie hello hello nové zařízení na řídicím panelu hello zobrazuje hello zařízení teploty nikdy nepřesáhne 45 stupňů.</span><span class="sxs-lookup"><span data-stu-id="15253-272">Before you start, notice that hello telemetry history for hello new device on hello dashboard shows hello device temperature never exceeds 45 degrees.</span></span>

1. <span data-ttu-id="15253-273">Přejděte zpět toohello seznam zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-273">Navigate back toohello device list.</span></span>

1. <span data-ttu-id="15253-274">tooadd pravidlo pro hello zařízení, vyberte nové zařízení v hello **seznam zařízení**a potom zvolte **přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="15253-274">tooadd a rule for hello device, select your new device in hello **Devices List**, and then choose **Add rule**.</span></span>

1. <span data-ttu-id="15253-275">Vytvořte pravidlo, které používá **teploty** jako hello datové pole a používá **AlarmTemp** jako hello výstup, když teplota hello přesáhne hodnotu 47 stupňů:</span><span class="sxs-lookup"><span data-stu-id="15253-275">Create a rule that uses **Temperature** as hello data field and uses **AlarmTemp** as hello output when hello temperature exceeds 47 degrees:</span></span>

    ![Přidání pravidla pro zařízení][img-adddevicerule]

1. <span data-ttu-id="15253-277">Zvolte změny, toosave **uložit a zobrazit pravidla**.</span><span class="sxs-lookup"><span data-stu-id="15253-277">toosave your changes, choose **Save and View Rules**.</span></span>

1. <span data-ttu-id="15253-278">Zvolte **příkazy** v podokně podrobností hello zařízení pro nové zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="15253-278">Choose **Commands** in hello device details pane for hello new device.</span></span>

    ![Přidání pravidla pro zařízení][img-adddevicerule2]

1. <span data-ttu-id="15253-280">Vyberte **ChangeSetPointTemp** ze seznamu příkaz hello a sadu **SetPointTemp** too45.</span><span class="sxs-lookup"><span data-stu-id="15253-280">Select **ChangeSetPointTemp** from hello command list and set **SetPointTemp** too45.</span></span> <span data-ttu-id="15253-281">Potom zvolte **Odeslat příkaz**:</span><span class="sxs-lookup"><span data-stu-id="15253-281">Then choose **Send Command**:</span></span>

    ![Přidání pravidla pro zařízení][img-adddevicerule3]

1. <span data-ttu-id="15253-283">Přejděte zpět toohello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="15253-283">Navigate back toohello dashboard.</span></span> <span data-ttu-id="15253-284">Po krátkou dobu zobrazí jako nová položka v hello **historie alarmů** když hello teplota hlášená tímto novým zařízením překročí prahovou hodnotu 47 stupňů hello:</span><span class="sxs-lookup"><span data-stu-id="15253-284">After a short time, you will see a new entry in hello **Alarm History** pane when hello temperature reported by your new device exceeds hello 47-degree threshold:</span></span>

    ![Přidání pravidla pro zařízení][img-adddevicerule4]

1. <span data-ttu-id="15253-286">Můžete zkontrolovat a upravit všechna pravidla v hello **pravidla** hello řídicím panelu:</span><span class="sxs-lookup"><span data-stu-id="15253-286">You can review and edit all your rules on hello **Rules** page of hello dashboard:</span></span>

    ![Zobrazení pravidel zařízení][img-rules]

1. <span data-ttu-id="15253-288">Můžete zkontrolovat a upravit všechny hello akce, které můžete provést v pravidle tooa odpovědi na hello **akce** hello řídicím panelu:</span><span class="sxs-lookup"><span data-stu-id="15253-288">You can review and edit all hello actions that can be taken in response tooa rule on hello **Actions** page of hello dashboard:</span></span>

    ![Zobrazení akcí zařízení][img-actions]

> [!NOTE]
> <span data-ttu-id="15253-290">Je možné toodefine akce, které může odesílat e-mailovou zprávu nebo SMS v odpovědi tooa pravidla nebo integrovat-obchodní systému prostřednictvím [aplikace logiky][lnk-logic-apps].</span><span class="sxs-lookup"><span data-stu-id="15253-290">It is possible toodefine actions that can send an email message or SMS in response tooa rule or integrate with a line-of-business system through a [Logic App][lnk-logic-apps].</span></span> <span data-ttu-id="15253-291">Další informace najdete v tématu hello [tooyour připojit aplikace logiky Azure IoT Suite vzdálené monitorování předkonfigurované řešení][lnk-logicapptutorial].</span><span class="sxs-lookup"><span data-stu-id="15253-291">For more information, see hello [Connect Logic App tooyour Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logicapptutorial].</span></span>

### <a name="manage-filters"></a><span data-ttu-id="15253-292">Správa filtrů</span><span class="sxs-lookup"><span data-stu-id="15253-292">Manage filters</span></span>

<span data-ttu-id="15253-293">V seznamu hello zařízení můžete vytvořit, uložit a znovu načíst filtry toodisplay přizpůsobeného seznamu zařízení připojených tooyour rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="15253-293">In hello device list, you can create, save, and reload filters toodisplay a customized list of devices connected tooyour hub.</span></span> <span data-ttu-id="15253-294">toocreate filtru:</span><span class="sxs-lookup"><span data-stu-id="15253-294">toocreate a filter:</span></span>

1. <span data-ttu-id="15253-295">Vyberte ikonu pro úpravu filtru hello hello v seznamu zařízení:</span><span class="sxs-lookup"><span data-stu-id="15253-295">Choose hello edit filter icon above hello list of devices:</span></span>

    ![Otevřete hello filtru editoru][img-editfiltericon]

1. <span data-ttu-id="15253-297">V hello **filtru editor**, přidat hello pole, operátory a seznam hodnot toofilter hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-297">In hello **Filter editor**, add hello fields, operators, and values toofilter hello device list.</span></span> <span data-ttu-id="15253-298">Více klauzulí toorefine můžete přidat filtr.</span><span class="sxs-lookup"><span data-stu-id="15253-298">You can add multiple clauses toorefine your filter.</span></span> <span data-ttu-id="15253-299">Zvolte **filtru** tooapply hello filtru:</span><span class="sxs-lookup"><span data-stu-id="15253-299">Choose **Filter** tooapply hello filter:</span></span>

    ![Vytvoření filtru][img-filtereditor]

1. <span data-ttu-id="15253-301">V tomto příkladu je seznam hello filtrovaný podle výrobce a model číslo:</span><span class="sxs-lookup"><span data-stu-id="15253-301">In this example, hello list is filtered by manufacturer and model number:</span></span>

    ![Filtrovaný seznam][img-filterelist]

1. <span data-ttu-id="15253-303">toosave filtr vlastní název, vyberte hello **uložit jako** ikona:</span><span class="sxs-lookup"><span data-stu-id="15253-303">toosave your filter with a custom name, choose hello **Save as** icon:</span></span>

    ![Uložení filtru][img-savefilter]

1. <span data-ttu-id="15253-305">tooreapply filtr jste uložili dřív, zvolte hello **otevřít uložené filtru** ikona:</span><span class="sxs-lookup"><span data-stu-id="15253-305">tooreapply a filter you saved previously, choose hello **Open saved filter** icon:</span></span>

    ![Otevření filtru][img-openfilter]

<span data-ttu-id="15253-307">Můžete vytvořit filtry na základě ID zařízení, stavu zařízení, požadovaných vlastností, ohlášených vlastností a značek.</span><span class="sxs-lookup"><span data-stu-id="15253-307">You can create filters based on device id, device state, desired properties, reported properties, and tags.</span></span> <span data-ttu-id="15253-308">Přidejte si vlastní zařízení tooa vlastní značky v hello **značky** části hello **podrobnosti o zařízení** panelu nebo spustit úlohu tooupdate značky na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="15253-308">You add your own custom tags tooa device in hello **Tags** section of hello **Device Details** panel, or run a job tooupdate tags on multiple devices.</span></span>

> [!NOTE]
> <span data-ttu-id="15253-309">V hello **filtru editor**, můžete použít hello **rozšířené zobrazení** tooedit hello text dotazu přímo.</span><span class="sxs-lookup"><span data-stu-id="15253-309">In hello **Filter editor**, you can use hello **Advanced view** tooedit hello query text directly.</span></span>

### <a name="commands"></a><span data-ttu-id="15253-310">Příkazy</span><span class="sxs-lookup"><span data-stu-id="15253-310">Commands</span></span>

<span data-ttu-id="15253-311">Z hello **podrobnosti o zařízení** panelu, můžete odeslat příkazy toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="15253-311">From hello **Device Details** panel, you can send commands toohello device.</span></span> <span data-ttu-id="15253-312">Při prvním spuštění zařízení, odešle se, že informace o hello příkazy, že podporuje toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="15253-312">When a device first starts, it sends information about hello commands it supports toohello solution.</span></span> <span data-ttu-id="15253-313">Informace o hello rozdíly mezi příkazy a metody, najdete v části [možnosti cloud zařízení Azure IoT Hub][lnk-c2d-guidance].</span><span class="sxs-lookup"><span data-stu-id="15253-313">For a discussion of hello differences between commands and methods, see [Azure IoT Hub cloud-to-device options][lnk-c2d-guidance].</span></span>

1. <span data-ttu-id="15253-314">Zvolte **příkazy** v hello **podrobnosti o zařízení** panel pro vybrané zařízení hello:</span><span class="sxs-lookup"><span data-stu-id="15253-314">Choose **Commands** in hello **Device Details** panel for hello selected device:</span></span>

   ![Příkazy zařízení na řídicím panelu][img-devicecommands]

1. <span data-ttu-id="15253-316">Vyberte **PingDevice** hello příkaz seznamu.</span><span class="sxs-lookup"><span data-stu-id="15253-316">Select **PingDevice** from hello command list.</span></span>

1. <span data-ttu-id="15253-317">Zvolte **Odeslat příkaz**.</span><span class="sxs-lookup"><span data-stu-id="15253-317">Choose **Send Command**.</span></span>

1. <span data-ttu-id="15253-318">Zobrazí se stav hello hello příkazu v historii příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="15253-318">You can see hello status of hello command in hello command history.</span></span>

   ![Stav příkazu na řídicím panelu][img-pingcommand]

<span data-ttu-id="15253-320">Hello řešení sleduje stav každého příkazu, který odešle hello.</span><span class="sxs-lookup"><span data-stu-id="15253-320">hello solution tracks hello status of each command it sends.</span></span> <span data-ttu-id="15253-321">Zpočátku je výsledek hello **čekající**.</span><span class="sxs-lookup"><span data-stu-id="15253-321">Initially hello result is **Pending**.</span></span> <span data-ttu-id="15253-322">Když hello zařízení ohlásí, že se provedla hello příkaz, hello výsledek je nastaven příliš**úspěch**.</span><span class="sxs-lookup"><span data-stu-id="15253-322">When hello device reports that it has executed hello command, hello result is set too**Success**.</span></span>

## <a name="behind-hello-scenes"></a><span data-ttu-id="15253-323">Pozadí hello</span><span class="sxs-lookup"><span data-stu-id="15253-323">Behind hello scenes</span></span>

<span data-ttu-id="15253-324">Když nasadíte předkonfigurované řešení, proces nasazení hello vytvoří několik prostředků v hello předplatné Azure, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="15253-324">When you deploy a preconfigured solution, hello deployment process creates multiple resources in hello Azure subscription you selected.</span></span> <span data-ttu-id="15253-325">Tyto prostředky můžete zobrazit v hello Azure [portál][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="15253-325">You can view these resources in hello Azure [portal][lnk-portal].</span></span> <span data-ttu-id="15253-326">proces nasazení Hello vytvoří **skupiny prostředků** s názvem na základě názvu hello jste vybrali pro předkonfigurované řešení:</span><span class="sxs-lookup"><span data-stu-id="15253-326">hello deployment process creates a **resource group** with a name based on hello name you choose for your preconfigured solution:</span></span>

![Předkonfigurované řešení v hello portálu Azure][img-portal]

<span data-ttu-id="15253-328">Hello nastavení každého prostředku můžete zobrazit výběrem v seznamu prostředků ve skupině prostředků hello hello.</span><span class="sxs-lookup"><span data-stu-id="15253-328">You can view hello settings of each resource by selecting it in hello list of resources in hello resource group.</span></span>

<span data-ttu-id="15253-329">Můžete také zobrazit zdrojový kód hello hello předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="15253-329">You can also view hello source code for hello preconfigured solution.</span></span> <span data-ttu-id="15253-330">Hello vzdálené monitorování zdrojový kód předkonfigurovaného řešení je v hello [azure-iot-remote-monitoring] [ lnk-rmgithub] úložiště GitHub:</span><span class="sxs-lookup"><span data-stu-id="15253-330">hello remote monitoring preconfigured solution source code is in hello [azure-iot-remote-monitoring][lnk-rmgithub] GitHub repository:</span></span>

* <span data-ttu-id="15253-331">Hello **DeviceAdministration** složka obsahuje hello zdrojový kód pro řídicí panel hello.</span><span class="sxs-lookup"><span data-stu-id="15253-331">hello **DeviceAdministration** folder contains hello source code for hello dashboard.</span></span>
* <span data-ttu-id="15253-332">Hello **simulátoru** složka obsahuje hello zdrojový kód pro simulované zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="15253-332">hello **Simulator** folder contains hello source code for hello simulated device.</span></span>
* <span data-ttu-id="15253-333">Hello **EventProcessor** složka obsahuje zdrojový kód hello hello back endový proces, který zpracovává příchozí telemetrii hello.</span><span class="sxs-lookup"><span data-stu-id="15253-333">hello **EventProcessor** folder contains hello source code for hello back-end process that handles hello incoming telemetry.</span></span>

<span data-ttu-id="15253-334">Až skončíte, můžete odstranit hello předkonfigurované řešení ze svého předplatného Azure na hello [azureiotsuite.com] [ lnk-azureiotsuite] lokality.</span><span class="sxs-lookup"><span data-stu-id="15253-334">When you are done, you can delete hello preconfigured solution from your Azure subscription on hello [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="15253-335">Tento web můžete tooeasily odstranit všechny prostředky, které byly zřízeny při vytváření hello předkonfigurované řešení hello.</span><span class="sxs-lookup"><span data-stu-id="15253-335">This site enables you tooeasily delete all hello resources that were provisioned when you created hello preconfigured solution.</span></span>

> [!NOTE]
> <span data-ttu-id="15253-336">tooensure odstraňte všechny položky související s toohello předkonfigurované řešení, odstraňte jej na hello [azureiotsuite.com] [ lnk-azureiotsuite] lokality a neodstraňujte skupinu prostředků hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="15253-336">tooensure that you delete everything related toohello preconfigured solution, delete it on hello [azureiotsuite.com][lnk-azureiotsuite] site and do not delete hello resource group in hello portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="15253-337">Další kroky</span><span class="sxs-lookup"><span data-stu-id="15253-337">Next Steps</span></span>

<span data-ttu-id="15253-338">Teď, když máte ukázku nasazenou pracovní předkonfigurované řešení, budete pokračovat, Začínáme se službou IoT Suite načtením hello následující články:</span><span class="sxs-lookup"><span data-stu-id="15253-338">Now that you’ve deployed a working preconfigured solution, you can continue getting started with IoT Suite by reading hello following articles:</span></span>

* <span data-ttu-id="15253-339">[Návod pro předkonfigurované řešení vzdáleného monitorování][lnk-rm-walkthrough]</span><span class="sxs-lookup"><span data-stu-id="15253-339">[Remote monitoring preconfigured solution walkthrough][lnk-rm-walkthrough]</span></span>
* <span data-ttu-id="15253-340">[Připojit vaše zařízení toohello předkonfigurovanému řešení vzdáleného monitorování][lnk-connect-rm]</span><span class="sxs-lookup"><span data-stu-id="15253-340">[Connect your device toohello remote monitoring preconfigured solution][lnk-connect-rm]</span></span>
* <span data-ttu-id="15253-341">[Oprávnění na webu azureiotsuite.com hello][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="15253-341">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-menu]: media/iot-suite-getstarted-preconfigured-solutions/menu.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-alarms]: media/iot-suite-getstarted-preconfigured-solutions/alarms.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png
[img-columneditor]: media/iot-suite-getstarted-preconfigured-solutions/columneditor.png
[img-startimageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit1.png
[img-imageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit2.png
[img-editfiltericon]: media/iot-suite-getstarted-preconfigured-solutions/editfiltericon.png
[img-filtereditor]: media/iot-suite-getstarted-preconfigured-solutions/filtereditor.png
[img-filterelist]: media/iot-suite-getstarted-preconfigured-solutions/filteredlist.png
[img-savefilter]: media/iot-suite-getstarted-preconfigured-solutions/savefilter.png
[img-openfilter]:  media/iot-suite-getstarted-preconfigured-solutions/openfilter.png
[img-unhealthy-filter]: media/iot-suite-getstarted-preconfigured-solutions/unhealthyfilter.png
[img-filtered-unhealthy-list]: media/iot-suite-getstarted-preconfigured-solutions/unhealthylist.png
[img-change-interval]: media/iot-suite-getstarted-preconfigured-solutions/changeinterval.png
[img-old-firmware]: media/iot-suite-getstarted-preconfigured-solutions/noticeold.png
[img-old-filter]: media/iot-suite-getstarted-preconfigured-solutions/oldfilter.png
[img-filtered-old-list]: media/iot-suite-getstarted-preconfigured-solutions/oldlist.png
[img-method-update]: media/iot-suite-getstarted-preconfigured-solutions/methodupdate.png
[img-update-1]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate1.png
[img-update-2]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate2.png
[img-update-3]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate3.png
[img-updated]: media/iot-suite-getstarted-preconfigured-solutions/updated.png
[img-healthy]: media/iot-suite-getstarted-preconfigured-solutions/healthy.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-faq]: iot-suite-faq.md