---
title: "Začínáme s předkonfigurovanými řešeními | Dokumentace Microsoftu"
description: "V tomto kurzu se dozvíte, jak nasadit předkonfigurované řešení Azure IoT Suite."
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
ms.openlocfilehash: 466825ab78a5ac9773d8beff69cca90ff9db6c01
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-the-preconfigured-solutions"></a><span data-ttu-id="cb5de-103">Začínáme s předkonfigurovanými řešeními</span><span class="sxs-lookup"><span data-stu-id="cb5de-103">Get started with the preconfigured solutions</span></span>

<span data-ttu-id="cb5de-104">[Předkonfigurovaná řešení][lnk-preconfigured-solutions] pro sadu Azure IoT Suite kombinují více služeb Azure IoT, aby mohla poskytovat komplexní řešení implementující běžné obchodní scénáře IoT.</span><span class="sxs-lookup"><span data-stu-id="cb5de-104">Azure IoT Suite [preconfigured solutions][lnk-preconfigured-solutions] combine multiple Azure IoT services to deliver end-to-end solutions that implement common IoT business scenarios.</span></span> <span data-ttu-id="cb5de-105">Předkonfigurované řešení *vzdálené monitorování* se připojuje k zařízením a monitoruje je.</span><span class="sxs-lookup"><span data-stu-id="cb5de-105">The *remote monitoring* preconfigured solution connects to and monitors your devices.</span></span> <span data-ttu-id="cb5de-106">Řešení můžete použít k analýze streamu dat ze všech zařízení a ke zlepšení obchodních výsledků díky tomu, že procesy mohou automaticky reagovat na tento stream dat.</span><span class="sxs-lookup"><span data-stu-id="cb5de-106">You can use the solution to analyze the stream of data from your devices and to improve business outcomes by making processes respond automatically to that stream of data.</span></span>

<span data-ttu-id="cb5de-107">V tomto kurzu se dozvíte, jak zřídit předkonfigurované řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="cb5de-107">This tutorial shows you how to provision the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="cb5de-108">Také se seznámíte se základními funkcemi předkonfigurovaného řešení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-108">It also walks you through the basic features of the preconfigured solution.</span></span> <span data-ttu-id="cb5de-109">Mnohé z těchto funkcí jsou přístupné z *řídicího panelu* řešení, který se nasazuje jako součást předkonfigurovaného řešení:</span><span class="sxs-lookup"><span data-stu-id="cb5de-109">You can access many of these features from the solution *dashboard* that deploys as part of the preconfigured solution:</span></span>

![Řídicí panel předkonfigurovaného řešení vzdáleného monitorování][img-dashboard]

<span data-ttu-id="cb5de-111">K dokončení tohoto kurzu potřebujete mít aktivní předplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="cb5de-111">To complete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="cb5de-112">Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební.</span><span class="sxs-lookup"><span data-stu-id="cb5de-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="cb5de-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="cb5de-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="scenario-overview"></a><span data-ttu-id="cb5de-114">Přehled scénáře</span><span class="sxs-lookup"><span data-stu-id="cb5de-114">Scenario overview</span></span>

<span data-ttu-id="cb5de-115">Když nasadíte předkonfigurované řešení vzdáleného monitorování, bude předem naplněné prostředky, které vám umožní projít běžným scénářem vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="cb5de-115">When you deploy the remote monitoring preconfigured solution, it is prepopulated with resources that enable you to step through a common remote monitoring scenario.</span></span> <span data-ttu-id="cb5de-116">V tomto scénáři několik zařízení připojených k řešení hlásí neočekávané teplotní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cb5de-116">In this scenario, several devices connected to the solution are reporting unexpected temperature values.</span></span> <span data-ttu-id="cb5de-117">V následujících částech se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="cb5de-117">The following sections show you how to:</span></span>

* <span data-ttu-id="cb5de-118">identifikovat zařízení, která odesílají neočekávané teplotní hodnoty,</span><span class="sxs-lookup"><span data-stu-id="cb5de-118">Identify the devices sending unexpected temperature values.</span></span>
* <span data-ttu-id="cb5de-119">nakonfigurovat tato zařízení, aby odesílala podrobnější telemetrii,</span><span class="sxs-lookup"><span data-stu-id="cb5de-119">Configure these devices to send more detailed telemetry.</span></span>
* <span data-ttu-id="cb5de-120">opravit problém pomocí aktualizace firmwaru na těchto zařízeních,</span><span class="sxs-lookup"><span data-stu-id="cb5de-120">Fix the problem by updating the firmware on these devices.</span></span>
* <span data-ttu-id="cb5de-121">ověřit, že jste touto akcí problém vyřešili.</span><span class="sxs-lookup"><span data-stu-id="cb5de-121">Verify that your action has resolved the issue.</span></span>

<span data-ttu-id="cb5de-122">Klíčovou vlastností tohoto scénáře je, že všechny tyto akce můžete provádět vzdáleně z řídicího panelu řešení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-122">A key feature of this scenario is that you can perform all these actions remotely from the solution dashboard.</span></span> <span data-ttu-id="cb5de-123">Nemusíte mít fyzický přístup k zařízením.</span><span class="sxs-lookup"><span data-stu-id="cb5de-123">You do not need physical access to the devices.</span></span>

## <a name="view-the-solution-dashboard"></a><span data-ttu-id="cb5de-124">Zobrazení řídicího panelu řešení</span><span class="sxs-lookup"><span data-stu-id="cb5de-124">View the solution dashboard</span></span>

<span data-ttu-id="cb5de-125">Přes řídicí panel řešení můžete spravovat nasazené řešení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-125">The solution dashboard enables you to manage the deployed solution.</span></span> <span data-ttu-id="cb5de-126">Můžete například zobrazit telemetrická data, přidat zařízení nebo konfigurovat pravidla.</span><span class="sxs-lookup"><span data-stu-id="cb5de-126">For example, you can view telemetry, add devices, and configure rules.</span></span>

1. <span data-ttu-id="cb5de-127">Až bude zřizování dokončeno a dlaždice předkonfigurovaného řešení bude hlásit **Připraveno**, zvolte **Spustit**. Na nové kartě se otevře portál předkonfigurovaného řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="cb5de-127">When the provisioning is complete and the tile for your preconfigured solution indicates **Ready**, choose **Launch** to open your remote monitoring solution portal in a new tab.</span></span>

    ![Spuštění předkonfigurovaného řešení][img-launch-solution]

1. <span data-ttu-id="cb5de-129">Ve výchozím nastavení se na portálu řešení zobrazuje *řídicí panel*.</span><span class="sxs-lookup"><span data-stu-id="cb5de-129">By default, the solution portal shows the *dashboard*.</span></span> <span data-ttu-id="cb5de-130">Do jiných oblastní portálu řešení můžete přecházet pomocí nabídky na levé straně stránky.</span><span class="sxs-lookup"><span data-stu-id="cb5de-130">You can navigate to other areas of the solution portal using the menu on the left-hand side of the page.</span></span>

    ![Řídicí panel předkonfigurovaného řešení vzdáleného monitorování][img-menu]

<span data-ttu-id="cb5de-132">Řídicí panel obsahuje tyto informace:</span><span class="sxs-lookup"><span data-stu-id="cb5de-132">The dashboard displays the following information:</span></span>

* <span data-ttu-id="cb5de-133">Mapu, která zobrazuje umístění každého zařízení, které je připojené k řešení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-133">A map that displays the location of each device connected to the solution.</span></span> <span data-ttu-id="cb5de-134">Když řešení poprvé spustíte, zahrnuje 25 simulovaných zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-134">When you first run the solution, there are 25 simulated devices.</span></span> <span data-ttu-id="cb5de-135">Simulovaná zařízení jsou implementována jako Azure WebJobs a řešení k vykreslování informací na mapě používá Bing Maps API.</span><span class="sxs-lookup"><span data-stu-id="cb5de-135">The simulated devices are implemented as Azure WebJobs, and the solution uses the Bing Maps API to plot information on the map.</span></span> <span data-ttu-id="cb5de-136">Pokud chcete mít mapu dynamickou, přečtěte si [Nejčastější dotazy][lnk-faq].</span><span class="sxs-lookup"><span data-stu-id="cb5de-136">See the [FAQ][lnk-faq] to learn how to make the map dynamic.</span></span>
* <span data-ttu-id="cb5de-137">Panel **Historie telemetrie**, na kterém se skoro v reálném čase zobrazuje telemetrie vlhkosti a teploty z vybraného zařízení a agregovaná data (například minimální, maximální a průměrná vlhkost).</span><span class="sxs-lookup"><span data-stu-id="cb5de-137">A **Telemetry History** panel that plots humidity and temperature telemetry from a selected device in near real time and displays aggregate data such as maximum, minimum, and average humidity.</span></span>
* <span data-ttu-id="cb5de-138">Panel **Historie alarmů**, který zobrazuje události, kdy v poslední době hodnota telemetrie překročila stanovenou mez.</span><span class="sxs-lookup"><span data-stu-id="cb5de-138">An **Alarm History** panel that shows recent alarm events when a telemetry value has exceeded a threshold.</span></span> <span data-ttu-id="cb5de-139">Kromě příkladů vytvořených předkonfigurovaným řešením můžete definovat i vlastní alarmy.</span><span class="sxs-lookup"><span data-stu-id="cb5de-139">You can define your own alarms in addition to the examples created by the preconfigured solution.</span></span>
* <span data-ttu-id="cb5de-140">Panel **Úlohy**, na kterém se zobrazují informace o plánovaných úlohách.</span><span class="sxs-lookup"><span data-stu-id="cb5de-140">A **Jobs** panel that displays information about scheduled jobs.</span></span> <span data-ttu-id="cb5de-141">Vlastní úlohy můžete plánovat na stránce **Úlohy správy**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-141">You can schedule your own jobs on **Management jobs** page.</span></span>

## <a name="view-alarms"></a><span data-ttu-id="cb5de-142">Zobrazení alarmů</span><span class="sxs-lookup"><span data-stu-id="cb5de-142">View alarms</span></span>

<span data-ttu-id="cb5de-143">Panel Historie alarmů ukazuje, že pět zařízení hlásí vyšší než očekávané hodnoty telemetrie.</span><span class="sxs-lookup"><span data-stu-id="cb5de-143">The alarm history panel shows you that five devices are reporting higher than expected telemetry values.</span></span>

![Historie alarmů na řídicím panelu řešení][img-alarms]

> [!NOTE]
> <span data-ttu-id="cb5de-145">Tyto alarmy generuje pravidlo, které je součástí předkonfigurovaného řešení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-145">These alarms are generated by a rule that is included in the preconfigured solution.</span></span> <span data-ttu-id="cb5de-146">Toto pravidlo generuje upozornění, když teplotní hodnota, kterou zařízení odešle, překročí 60.</span><span class="sxs-lookup"><span data-stu-id="cb5de-146">This rule generates an alert when the temperature value sent by a device exceeds 60.</span></span> <span data-ttu-id="cb5de-147">Pokud v nabídce vlevo zvolíte [Pravidla](#add-a-rule) nebo [Akce](#add-an-action), můžete definovat vlastní pravidla a akce.</span><span class="sxs-lookup"><span data-stu-id="cb5de-147">You can define your own rules and actions by choosing [Rules](#add-a-rule) and [Actions](#add-an-action) in the left-hand menu.</span></span>

## <a name="view-devices"></a><span data-ttu-id="cb5de-148">Zobrazení zařízení</span><span class="sxs-lookup"><span data-stu-id="cb5de-148">View devices</span></span>

<span data-ttu-id="cb5de-149">V seznamu *zařízení* jsou uvedena všechna zařízení, která jsou zaregistrována v řešení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-149">The *devices* list shows all the registered devices in the solution.</span></span> <span data-ttu-id="cb5de-150">V seznamu zařízení můžete zobrazit a upravovat metadata zařízení, přidávat a odebírat zařízení a vyvolávat v zařízeních metody.</span><span class="sxs-lookup"><span data-stu-id="cb5de-150">From the device list you can view and edit device metadata, add or remove devices, and invoke methods on devices.</span></span> <span data-ttu-id="cb5de-151">Seznam zařízení můžete filtrovat a řadit.</span><span class="sxs-lookup"><span data-stu-id="cb5de-151">You can filter and sort the list of devices in the device list.</span></span> <span data-ttu-id="cb5de-152">Můžete si také přizpůsobit, jaké sloupce se v seznamu zařízení zobrazí.</span><span class="sxs-lookup"><span data-stu-id="cb5de-152">You can also customize the columns shown in the device list.</span></span>

1. <span data-ttu-id="cb5de-153">Zvolte **Zařízení** a zobrazte seznam zařízení pro toto řešení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-153">Choose **Devices** to show the device list for this solution.</span></span>

   ![Zobrazení seznamu zařízení na portálu řešení][img-devicelist]

1. <span data-ttu-id="cb5de-155">V seznamu zařízení je zpočátku uvedeno 25 simulovaných zařízení, která byla vytvořena během procesu zřizování.</span><span class="sxs-lookup"><span data-stu-id="cb5de-155">The device list initially shows 25 simulated devices created by the provisioning process.</span></span> <span data-ttu-id="cb5de-156">Do řešení můžete přidat další simulovaná a fyzická zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-156">You can add additional simulated and physical devices to the solution.</span></span>

1. <span data-ttu-id="cb5de-157">Pokud chcete zobrazit podrobnosti o některém zařízení, vyberte ho v seznamu zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-157">To view the details of a device, choose a device in the device list.</span></span>

   ![Zobrazení podrobností o zařízení na portálu řešení][img-devicedetails]

<span data-ttu-id="cb5de-159">Panel **Podrobnosti o zařízení** se skládá ze šesti částí:</span><span class="sxs-lookup"><span data-stu-id="cb5de-159">The **Device Details** panel contains six sections:</span></span>

* <span data-ttu-id="cb5de-160">Kolekce odkazů, pomocí kterých si můžete přizpůsobit ikonu zařízení, zakázat zařízení, přidat pravidlo, vyvolat metodu nebo odeslat příkaz.</span><span class="sxs-lookup"><span data-stu-id="cb5de-160">A collection of links that enable you to customize the device icon, disable the device, add a rule, invoke a method, or send a command.</span></span> <span data-ttu-id="cb5de-161">Porovnání příkazů (zpráv typu zařízení-cloud) a metod (přímých metod) najdete v [doprovodných materiálech ke komunikaci typu cloud-zařízení][lnk-c2d-guidance].</span><span class="sxs-lookup"><span data-stu-id="cb5de-161">For a comparison of commands (device-to-cloud messages) and methods (direct methods), see [Cloud-to-device communications guidance][lnk-c2d-guidance].</span></span>
* <span data-ttu-id="cb5de-162">V části **Dvojče zařízení – Značky** můžete upravovat hodnoty značek pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-162">The **Device Twin - Tags** section enables you to edit tag values for the device.</span></span> <span data-ttu-id="cb5de-163">Hodnoty značek můžete zobrazit v seznamu zařízení a s jejich pomocí seznam zařízení filtrovat.</span><span class="sxs-lookup"><span data-stu-id="cb5de-163">You can display tag values in the device list and use tag values to filter the device list.</span></span>
* <span data-ttu-id="cb5de-164">V části **Dvojče zařízení – Požadované vlastnosti** můžete nastavit hodnoty vlastností, které se odešlou do zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-164">The **Device Twin - Desired Properties** section enables you to set property values to be sent to the device.</span></span>
* <span data-ttu-id="cb5de-165">V části **Dvojče zařízení – Ohlášené vlastnosti** se zobrazují hodnoty vlastností odeslané ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-165">The **Device Twin - Reported Properties** section shows property values sent from the device.</span></span>
* <span data-ttu-id="cb5de-166">V části **Vlastnosti zařízení** se zobrazují informace z registru identit, jako například ID zařízení a ověřovací klíče.</span><span class="sxs-lookup"><span data-stu-id="cb5de-166">The **Device Properties** section shows information from the identity registry such as the device id and authentication keys.</span></span>
* <span data-ttu-id="cb5de-167">V části **Poslední úlohy** se zobrazují informace o všech úlohách, které nedávno cílily na příslušné zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-167">The **Recent Jobs** section shows information about any jobs that have recently targeted this device.</span></span>

## <a name="filter-the-device-list"></a><span data-ttu-id="cb5de-168">Filtrování seznamu zařízení</span><span class="sxs-lookup"><span data-stu-id="cb5de-168">Filter the device list</span></span>

<span data-ttu-id="cb5de-169">Pomocí filtru můžete zobrazit jenom zařízení, která odesílají neočekávané teplotní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cb5de-169">You can use a filter to display only those devices that are sending unexpected temperature values.</span></span> <span data-ttu-id="cb5de-170">Předkonfigurované řešení vzdáleného monitorování obsahuje filtr **Poškozená zařízení**, který zobrazí zařízení, u nichž je střední hodnota teploty vyšší než 60.</span><span class="sxs-lookup"><span data-stu-id="cb5de-170">The remote monitoring preconfigured solution includes the **Unhealthy devices** filter to show devices with a mean temperature value greater than 60.</span></span> <span data-ttu-id="cb5de-171">Můžete si také [vytvořit vlastní filtry](#add-a-filter).</span><span class="sxs-lookup"><span data-stu-id="cb5de-171">You can also [create your own filters](#add-a-filter).</span></span>

1. <span data-ttu-id="cb5de-172">Zvolte **Otevřít uložený filtr** a zobrazí se seznam dostupných filtrů.</span><span class="sxs-lookup"><span data-stu-id="cb5de-172">Choose **Open saved filter** to display a list of available filters.</span></span> <span data-ttu-id="cb5de-173">Potom zvolením aplikujte filtr **Poškozená zařízení**:</span><span class="sxs-lookup"><span data-stu-id="cb5de-173">Then choose **Unhealthy devices** to apply the filter:</span></span>

    ![Zobrazení seznamu filtrů][img-unhealthy-filter]

1. <span data-ttu-id="cb5de-175">V seznamu zařízení teď budou uvedena pouze zařízení, u nichž je střední hodnota teploty vyšší než 60.</span><span class="sxs-lookup"><span data-stu-id="cb5de-175">The device list now shows only devices with a mean temperature value greater than 60.</span></span>

    ![Zobrazení filtrovaného seznamu zařízení, který ukazuje poškozená zařízení][img-filtered-unhealthy-list]

## <a name="update-desired-properties"></a><span data-ttu-id="cb5de-177">Aktualizace požadovaných vlastností</span><span class="sxs-lookup"><span data-stu-id="cb5de-177">Update desired properties</span></span>

<span data-ttu-id="cb5de-178">Nyní jste identifikovali sadu zařízení, která možná vyžadují opravu.</span><span class="sxs-lookup"><span data-stu-id="cb5de-178">You have now identified a set of devices that may need remediation.</span></span> <span data-ttu-id="cb5de-179">Rozhodnete se ale, že frekvence odesílání dat každých 15 sekund není pro jasnou diagnostiku problému dostatečná.</span><span class="sxs-lookup"><span data-stu-id="cb5de-179">However, you decide that the data frequency of 15 seconds is not sufficient for a clear diagnosis of the issue.</span></span> <span data-ttu-id="cb5de-180">Změníte frekvenci odesílání telemetrie na 5 sekund, což vám poskytne více datových bodů pro lepší diagnostiku problému.</span><span class="sxs-lookup"><span data-stu-id="cb5de-180">Changing the telemetry frequency to five seconds to provide you with more data points to better diagnose the issue.</span></span> <span data-ttu-id="cb5de-181">Tuto změnu konfigurace můžete z portálu řešení bez vyžádání doručit do vzdálených zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-181">You can push this configuration change to your remote devices from the solution portal.</span></span> <span data-ttu-id="cb5de-182">Můžete jednou provést změnu, vyhodnotit její dopad a na základě výsledků se potom rozhodnout.</span><span class="sxs-lookup"><span data-stu-id="cb5de-182">You can make the change once, evaluate the impact, and then act on the results.</span></span>

<span data-ttu-id="cb5de-183">Postupujte podle těchto kroků a spusťte úlohu, která změní požadovanou vlastnost **TelemetryInterval** pro příslušná zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-183">Follow these steps to run a job that changes the **TelemetryInterval** desired property for the affected devices.</span></span> <span data-ttu-id="cb5de-184">Zařízení po přijetí nové hodnoty vlastnosti **TelemetryInterval** změní vlastní konfiguraci a budou odesílat telemetrii každých pět sekund namísto každých 15 sekund:</span><span class="sxs-lookup"><span data-stu-id="cb5de-184">When the devices receive the new **TelemetryInterval** property value, they change their configuration to send telemetry every five seconds instead of every 15 seconds:</span></span>

1. <span data-ttu-id="cb5de-185">Když je v seznamu zařízení zobrazený seznam poškozených zařízení, zvolte **Plánovač úloh** a potom **Upravit dvojče zařízení**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-185">While you are showing the list of unhealthy devices in the device list, choose **Job Scheduler**, then **Edit Device Twin**.</span></span>

1. <span data-ttu-id="cb5de-186">Zavolejte úlohu **Změnit interval telemetrie**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-186">Call the job **Change telemetry interval**.</span></span>

1. <span data-ttu-id="cb5de-187">Změňte hodnotu **požadované vlastnosti** s názvem **desired.Config.TelemetryInterval** na pět sekund.</span><span class="sxs-lookup"><span data-stu-id="cb5de-187">Change the value of the **Desired Property** name **desired.Config.TelemetryInterval** to five seconds.</span></span>

1. <span data-ttu-id="cb5de-188">Zvolte **Naplánovat**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-188">Choose **Schedule**.</span></span>

    ![Změna vlastnosti TelemetryInterval na pět sekund][img-change-interval]

1. <span data-ttu-id="cb5de-190">Průběh úlohy můžete sledovat na stránce **Úlohy správy** na portálu.</span><span class="sxs-lookup"><span data-stu-id="cb5de-190">You can monitor the progress of the job on the **Management Jobs** page in the portal.</span></span>

> [!NOTE]
> <span data-ttu-id="cb5de-191">Pokud chcete změnit hodnotu požadované vlastnosti pro jednotlivá zařízení, místo spouštění úlohy použijte část **Požadované vlastnosti** na panelu **Podrobnosti o zařízení**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-191">If you want to change a desired property value for an individual device, use the **Desired Properties** section in the **Device Details** panel instead of running a job.</span></span>

<span data-ttu-id="cb5de-192">Tato úloha nastaví hodnotu požadované vlastnosti **TelemetryInterval** ve dvojčeti zařízení pro všechna zařízení vybraná pomocí filtru.</span><span class="sxs-lookup"><span data-stu-id="cb5de-192">This job sets the value of the **TelemetryInterval** desired property in the device twin for all the devices selected by the filter.</span></span> <span data-ttu-id="cb5de-193">Zařízení si tuto hodnotu načtou z dvojčete zařízení a aktualizují vlastní chování.</span><span class="sxs-lookup"><span data-stu-id="cb5de-193">The devices retrieve this value from the device twin and update their behavior.</span></span> <span data-ttu-id="cb5de-194">Když zařízení načte a zpracuje požadovanou vlastnost z dvojčete zařízení, nastaví odpovídající hodnotu ohlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="cb5de-194">When a device retrieves and processes a desired property from a device twin, it sets the corresponding reported value property.</span></span>

## <a name="invoke-methods"></a><span data-ttu-id="cb5de-195">Vyvolání metod</span><span class="sxs-lookup"><span data-stu-id="cb5de-195">Invoke methods</span></span>

<span data-ttu-id="cb5de-196">Zatímco je úloha spuštěná, v seznamu poškozených zařízení si všimnete, že všechna tato zařízení mají staré (nižší než 1.6) verze firmwaru.</span><span class="sxs-lookup"><span data-stu-id="cb5de-196">While the job runs, you notice in the list of unhealthy devices that all these devices have old (less than version 1.6) firmware versions.</span></span>

![Zobrazení ohlášených verzí firmwaru pro poškozená zařízení][img-old-firmware]

<span data-ttu-id="cb5de-198">Verze firmwaru může být hlavní příčinou neočekávaných teplotních hodnot, protože víte, že ostatní zařízení, která jsou v pořádku, byla nedávno aktualizována na verzi 2.0.</span><span class="sxs-lookup"><span data-stu-id="cb5de-198">This firmware version may be the root cause of the unexpected temperature values because you know that other healthy devices were recently updated to version 2.0.</span></span> <span data-ttu-id="cb5de-199">Pomocí integrovaného filtru **Zařízení se starou verzí firmwaru** můžete identifikovat všechna zařízení se starými verzemi firmwaru.</span><span class="sxs-lookup"><span data-stu-id="cb5de-199">You can use the built-in **Old firmware devices** filter to identify any devices with old firmware versions.</span></span> <span data-ttu-id="cb5de-200">Z portálu potom můžete vzdáleně aktualizovat všechna zařízení, která ještě používají staré verze firmwaru:</span><span class="sxs-lookup"><span data-stu-id="cb5de-200">From the portal, you can then remotely update all the devices still running old firmware versions:</span></span>

1. <span data-ttu-id="cb5de-201">Zvolte **Otevřít uložený filtr** a zobrazí se seznam dostupných filtrů.</span><span class="sxs-lookup"><span data-stu-id="cb5de-201">Choose **Open saved filter** to display a list of available filters.</span></span> <span data-ttu-id="cb5de-202">Potom zvolením aplikujte filtr **Zařízení se starou verzí firmwaru**:</span><span class="sxs-lookup"><span data-stu-id="cb5de-202">Then choose **Old firmware devices** to apply the filter:</span></span>

    ![Zobrazení seznamu filtrů][img-old-filter]

1. <span data-ttu-id="cb5de-204">V seznamu zařízení nyní budou uvedena pouze zařízení se starými verzemi firmwaru.</span><span class="sxs-lookup"><span data-stu-id="cb5de-204">The device list now shows only devices with old firmware versions.</span></span> <span data-ttu-id="cb5de-205">Tento seznam obsahuje pět zařízení, která identifikoval filtr **Poškozená zařízení**, a tři další zařízení:</span><span class="sxs-lookup"><span data-stu-id="cb5de-205">This list includes the five devices identified by the **Unhealthy devices** filter and three additional devices:</span></span>

    ![Zobrazení filtrovaného seznamu zařízení, který ukazuje stará zařízení][img-filtered-old-list]

1. <span data-ttu-id="cb5de-207">Zvolte **Plánovač úloh** a potom **Vyvolat metodu**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-207">Choose **Job Scheduler**, then **Invoke Method**.</span></span>

1. <span data-ttu-id="cb5de-208">Nastavte **Název úlohy** na **Aktualizace firmwaru na verzi 2.0**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-208">Set **Job Name** to **Firmware update to version 2.0**.</span></span>

1. <span data-ttu-id="cb5de-209">V poli **Metoda** zvolte **InitiateFirmwareUpdate**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-209">Choose **InitiateFirmwareUpdate** as the **Method**.</span></span>

1. <span data-ttu-id="cb5de-210">Nastavte parametr **FwPackageUri** na **https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-210">Set the **FwPackageUri** parameter to **https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.</span></span>

1. <span data-ttu-id="cb5de-211">Zvolte **Naplánovat**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-211">Choose **Schedule**.</span></span> <span data-ttu-id="cb5de-212">Ve výchozím nastavení se úloha spustí nyní.</span><span class="sxs-lookup"><span data-stu-id="cb5de-212">The default is for the job to run now.</span></span>

    ![Vytvoření úlohy pro aktualizaci firmwaru na vybraných zařízeních][img-method-update]

> [!NOTE]
> <span data-ttu-id="cb5de-214">Pokud chcete vyvolat metodu na jednotlivých zařízeních, místo spouštění úlohy na panelu **Podrobnosti o zařízení** zvolte **Metody**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-214">If you want to invoke a method on an individual device, choose **Methods** in the **Device Details** panel instead of running a job.</span></span>

<span data-ttu-id="cb5de-215">Tato úloha vyvolá přímou metodu **InitiateFirmwareUpdate** na všech zařízeních vybraných pomocí filtru.</span><span class="sxs-lookup"><span data-stu-id="cb5de-215">This job invokes the **InitiateFirmwareUpdate** direct method on all the devices selected by the filter.</span></span> <span data-ttu-id="cb5de-216">Zařízení okamžitě odešlou reakci do služby IoT Hub a následně asynchronně zahájí proces aktualizace firmwaru.</span><span class="sxs-lookup"><span data-stu-id="cb5de-216">Devices respond immediately to IoT Hub and then initiate the firmware update process asynchronously.</span></span> <span data-ttu-id="cb5de-217">Zařízení poskytují informace o stavu procesu aktualizace firmwaru prostřednictvím hodnot ohlášených vlastností, jak je znázorněno na následujících snímcích obrazovky.</span><span class="sxs-lookup"><span data-stu-id="cb5de-217">The devices provide status information about the firmware update process through reported property values, as shown in the following screenshots.</span></span> <span data-ttu-id="cb5de-218">Zvolte ikonu **Aktualizovat** a aktualizujte informace v seznamech zařízení a úloh:</span><span class="sxs-lookup"><span data-stu-id="cb5de-218">Choose the **Refresh** icon to update the information in the device and job lists:</span></span>

<span data-ttu-id="cb5de-219">![Seznam úloh ukazující spuštěnou úlohu aktualizace firmwaru][img-update-1]
![Seznam zařízení ukazující stav aktualizace firmwaru][img-update-2]
![Seznam úloh ukazující dokončenou úlohu aktualizace firmwaru][img-update-3]</span><span class="sxs-lookup"><span data-stu-id="cb5de-219">![Job list showing the firmware update list running][img-update-1]
![Device list showing firmware update status][img-update-2]
![Job list showing the firmware update list complete][img-update-3]</span></span>

> [!NOTE]
> <span data-ttu-id="cb5de-220">V produkčním prostředí můžete úlohy plánovat tak, aby se spouštěly během určeného časového období údržby.</span><span class="sxs-lookup"><span data-stu-id="cb5de-220">In a production environment, you can schedule jobs to run during a designated maintenance window.</span></span>

## <a name="scenario-review"></a><span data-ttu-id="cb5de-221">Revize scénáře</span><span class="sxs-lookup"><span data-stu-id="cb5de-221">Scenario review</span></span>

<span data-ttu-id="cb5de-222">V tomto scénáři jste identifikovali potenciální problém s některými vzdálenými zařízeními pomocí historie alarmů na řídicím panelu a filtru.</span><span class="sxs-lookup"><span data-stu-id="cb5de-222">In this scenario, you identified a potential issue with some of your remote devices using the alarm history on the dashboard and a filter.</span></span> <span data-ttu-id="cb5de-223">Pak jste pomocí filtru a úlohy vzdáleně nakonfigurovali zařízení, aby poskytovala více údajů a tak vám pomohla s diagnostikou problému.</span><span class="sxs-lookup"><span data-stu-id="cb5de-223">You then used the filter and a job to remotely configure the devices to provide more information to help diagnose the issue.</span></span> <span data-ttu-id="cb5de-224">Nakonec jste pomocí filtru a úlohy naplánovali údržbu příslušných zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-224">Finally, you used a filter and a job to schedule maintenance on the affected devices.</span></span> <span data-ttu-id="cb5de-225">Pokud se vrátíte na řídicí panel, můžete zkontrolovat, že ze zařízení ve vašem řešení již nepřichází žádné alarmy.</span><span class="sxs-lookup"><span data-stu-id="cb5de-225">If you return to the dashboard, you can check that there are no longer any alarms coming from devices in your solution.</span></span> <span data-ttu-id="cb5de-226">Pomocí filtru můžete ověřit, že na všech zařízeních ve vašem řešení je aktuální firmware, a že v řešení již nejsou žádná poškozená zařízení:</span><span class="sxs-lookup"><span data-stu-id="cb5de-226">You can use a filter to verify that the firmware is up-to-date on all the devices in your solution and that there are no more unhealthy devices:</span></span>

![Filtr ukazující, že všechna zařízení mají aktuální firmware][img-updated]

![Filtr ukazující, že všechna zařízení jsou v pořádku][img-healthy]

## <a name="other-features"></a><span data-ttu-id="cb5de-229">Další funkce</span><span class="sxs-lookup"><span data-stu-id="cb5de-229">Other features</span></span>

<span data-ttu-id="cb5de-230">Následující části popisují některé další funkce předkonfigurovaného řešení vzdáleného monitorování, které nebyly popsány v předchozím scénáři.</span><span class="sxs-lookup"><span data-stu-id="cb5de-230">The following sections describe some additional features of the remote monitoring preconfigured solution that are not described as part of the previous scenario.</span></span>

### <a name="customize-columns"></a><span data-ttu-id="cb5de-231">Přizpůsobení sloupců</span><span class="sxs-lookup"><span data-stu-id="cb5de-231">Customize columns</span></span>

<span data-ttu-id="cb5de-232">Informace, které se zobrazují v seznamu zařízení, si můžete přizpůsobit tak, že zvolíte **Editor sloupců**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-232">You can customize the information shown in the device list by choosing **Column editor**.</span></span> <span data-ttu-id="cb5de-233">Můžete přidat a odebrat sloupce, které zobrazují hodnoty ohlašovaných vlastností a značek.</span><span class="sxs-lookup"><span data-stu-id="cb5de-233">You can add and remove columns that display reported property and tag values.</span></span> <span data-ttu-id="cb5de-234">Můžete také změnit pořadí sloupců a přejmenovat je:</span><span class="sxs-lookup"><span data-stu-id="cb5de-234">You can also reorder and rename columns:</span></span>

   ![Ikona editoru sloupců v seznamu zařízení][img-columneditor]

### <a name="customize-the-device-icon"></a><span data-ttu-id="cb5de-236">Přizpůsobení ikony zařízení</span><span class="sxs-lookup"><span data-stu-id="cb5de-236">Customize the device icon</span></span>

<span data-ttu-id="cb5de-237">Ikonu zařízení, která se zobrazuje v seznamu zařízení, si můžete přizpůsobit na panelu **Podrobnosti o zařízení** podle následujícího postupu:</span><span class="sxs-lookup"><span data-stu-id="cb5de-237">You can customize the device icon displayed in the device list from the **Device Details** panel as follows:</span></span>

1. <span data-ttu-id="cb5de-238">Zvolením ikony tužky otevřete panel **Upravit obrázek** pro zařízení:</span><span class="sxs-lookup"><span data-stu-id="cb5de-238">Choose the pencil icon to open the **Edit image** panel for a device:</span></span>

   ![Otevřený editor obrázku zařízení][img-startimageedit]

1. <span data-ttu-id="cb5de-240">Nahrajte nový obrázek nebo použijte některý z existujících obrázků a potom zvolte **Uložit**:</span><span class="sxs-lookup"><span data-stu-id="cb5de-240">Either upload a new image or use one of the existing images and then choose **Save**:</span></span>

   ![Editor Upravit obrázek zařízení][img-imageedit]

1. <span data-ttu-id="cb5de-242">Obrázek, který jste vybrali, se teď zobrazí ve sloupci **Ikona** pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-242">The image you selected now displays in the **Icon** column for the device.</span></span>

> [!NOTE]
> <span data-ttu-id="cb5de-243">Obrázek se ukládá v úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="cb5de-243">The image is stored in blob storage.</span></span> <span data-ttu-id="cb5de-244">Značka ve dvojčeti zařízení obsahuje odkaz na tento obrázek v úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="cb5de-244">A tag in the device twin contains a link to the image in blob storage.</span></span>

### <a name="add-a-device"></a><span data-ttu-id="cb5de-245">Přidání zařízení</span><span class="sxs-lookup"><span data-stu-id="cb5de-245">Add a device</span></span>

<span data-ttu-id="cb5de-246">Při nasazení předkonfigurovaného řešení je automaticky zřízeno 25 ukázkových zařízení, která se zobrazí v seznamu zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-246">When you deploy the preconfigured solution, you automatically provision 25 sample devices that you can see in the device list.</span></span> <span data-ttu-id="cb5de-247">Tato zařízení jsou *simulovaná zařízení*, která běží ve webové úloze Azure.</span><span class="sxs-lookup"><span data-stu-id="cb5de-247">These devices are *simulated devices* running in an Azure WebJob.</span></span> <span data-ttu-id="cb5de-248">Simulované zařízení umožňují snadno experimentovat s předkonfigurovaným řešením, aniž by bylo nutné nasazovat skutečná fyzická zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-248">Simulated devices make it easy for you to experiment with the preconfigured solution without the need to deploy real, physical devices.</span></span> <span data-ttu-id="cb5de-249">Pokud chcete k řešení připojit skutečné zařízení, přečtěte si kurz [Připojení zařízení k předkonfigurovanému řešení vzdáleného monitorování][lnk-connect-rm].</span><span class="sxs-lookup"><span data-stu-id="cb5de-249">If you do want to connect a real device to the solution, see the [Connect your device to the remote monitoring preconfigured solution][lnk-connect-rm] tutorial.</span></span>

<span data-ttu-id="cb5de-250">Následující kroky ukazují, jak do řešení přidat simulované zařízení:</span><span class="sxs-lookup"><span data-stu-id="cb5de-250">The following steps show you how to add a simulated device to the solution:</span></span>

1. <span data-ttu-id="cb5de-251">Vraťte se zpět k seznamu zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-251">Navigate back to the device list.</span></span>

1. <span data-ttu-id="cb5de-252">Pokud chcete přidat zařízení, zvolte **+ Přidat zařízení** vlevo dole.</span><span class="sxs-lookup"><span data-stu-id="cb5de-252">To add a device, choose **+ Add A Device** in the bottom left corner.</span></span>

   ![Přidání zařízení do předkonfigurovaného řešení][img-adddevice]

1. <span data-ttu-id="cb5de-254">Na dlaždici **Simulované zařízení** zvolte **Přidat nové**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-254">Choose **Add New** on the **Simulated Device** tile.</span></span>

   ![Nastavení podrobností o zařízení na řídicím panelu][img-addnew]

   <span data-ttu-id="cb5de-256">Pokud se rozhodnete vytvořit**vlastní zařízení**, můžete kromě nového simulovaného zařízení vytvořit i zařízení fyzické.</span><span class="sxs-lookup"><span data-stu-id="cb5de-256">In addition to creating a new simulated device, you can also add a physical device if you choose to create a **Custom Device**.</span></span> <span data-ttu-id="cb5de-257">Další informace o připojení fyzických zařízení k řešení najdete v tématu [Připojení zařízení k předkonfigurovanému řešení vzdáleného monitorování sady IoT Suite][lnk-connect-rm].</span><span class="sxs-lookup"><span data-stu-id="cb5de-257">To learn more about connecting physical devices to the solution, see [Connect your device to the IoT Suite remote monitoring preconfigured solution][lnk-connect-rm].</span></span>

1. <span data-ttu-id="cb5de-258">Vyberte možnost **Ručně definovat vlastní ID zařízení** a zadejte jedinečný název zařízení, například **mydevice_01**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-258">Select **Let me define my own Device ID**, and enter a unique device ID name such as **mydevice_01**.</span></span>

1. <span data-ttu-id="cb5de-259">Zvolte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-259">Choose **Create**.</span></span>

   ![Uložení nového zařízení][img-definedevice]

1. <span data-ttu-id="cb5de-261">V kroku 3 **Přidání simulovaného zařízení** zvolte **Hotovo**. Vrátíte se do seznamu zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-261">In step 3 of **Add a simulated device**, choose **Done** to return to the device list.</span></span>

1. <span data-ttu-id="cb5de-262">Zařízení nyní v seznamu uvidíte jako **Spuštěné**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-262">You can view your device **Running** in the device list.</span></span>

    ![Zobrazení nového zařízení v seznamu zařízení][img-runningnew]

1. <span data-ttu-id="cb5de-264">Na řídicím panelu můžete také vidět simulovanou telemetrii z nového zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-264">You can also view the simulated telemetry from your new device on the dashboard:</span></span>

    ![Zobrazení telemetrie z nového zařízení][img-runningnew-2]

### <a name="disable-and-delete-a-device"></a><span data-ttu-id="cb5de-266">Zakázání a odstranění zařízení</span><span class="sxs-lookup"><span data-stu-id="cb5de-266">Disable and delete a device</span></span>

<span data-ttu-id="cb5de-267">Zařízení můžete zakázat, zakázané zařízení lze následně odebrat:</span><span class="sxs-lookup"><span data-stu-id="cb5de-267">You can disable a device, and after it is disabled you can remove it:</span></span>

![Zakázání a odebrání zařízení][img-disable]

### <a name="add-a-rule"></a><span data-ttu-id="cb5de-269">Přidání pravidla</span><span class="sxs-lookup"><span data-stu-id="cb5de-269">Add a rule</span></span>

<span data-ttu-id="cb5de-270">Pro zařízení, které jste právě přidali, ještě nejsou stanovena žádná pravidla.</span><span class="sxs-lookup"><span data-stu-id="cb5de-270">There are no rules for the new device you just added.</span></span> <span data-ttu-id="cb5de-271">V této sekci přidáte pravidlo, které spustí alarm, jakmile teplota hlášená novým zařízením přesáhne 47 stupňů.</span><span class="sxs-lookup"><span data-stu-id="cb5de-271">In this section, you add a rule that triggers an alarm when the temperature reported by the new device exceeds 47 degrees.</span></span> <span data-ttu-id="cb5de-272">Ještě než začnete, všimněte si, že historie telemetrie nového zařízení na řídicím panelu ukazuje, že teplota měřená tímto zařízením nikdy nepřesáhne 45 stupňů.</span><span class="sxs-lookup"><span data-stu-id="cb5de-272">Before you start, notice that the telemetry history for the new device on the dashboard shows the device temperature never exceeds 45 degrees.</span></span>

1. <span data-ttu-id="cb5de-273">Vraťte se zpět k seznamu zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-273">Navigate back to the device list.</span></span>

1. <span data-ttu-id="cb5de-274">Pokud chcete přidat pravidlo pro zařízení, vyberte nové zařízení na panelu **Seznam zařízení** a potom zvolte **Přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-274">To add a rule for the device, select your new device in the **Devices List**, and then choose **Add rule**.</span></span>

1. <span data-ttu-id="cb5de-275">Vytvořte pravidlo, které jako datové pole používá **teplotu**, a pokud tato veličina přesáhne 47 stupňů, použije jako výstup **AlarmTemp** (teplotní alarm):</span><span class="sxs-lookup"><span data-stu-id="cb5de-275">Create a rule that uses **Temperature** as the data field and uses **AlarmTemp** as the output when the temperature exceeds 47 degrees:</span></span>

    ![Přidání pravidla pro zařízení][img-adddevicerule]

1. <span data-ttu-id="cb5de-277">Pokud chcete uložit změny, zvolte **Uložit a zobrazit pravidla**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-277">To save your changes, choose **Save and View Rules**.</span></span>

1. <span data-ttu-id="cb5de-278">V podokně podrobností o novém zařízení zvolte **Příkazy**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-278">Choose **Commands** in the device details pane for the new device.</span></span>

    ![Přidání pravidla pro zařízení][img-adddevicerule2]

1. <span data-ttu-id="cb5de-280">V seznamu příkazů vyberte **ChangeSetPointTemp** (Změnit mezní teplotu) a příkaz **SetPointTemp** (Nastavit mezní teplotu) nastavte na 45.</span><span class="sxs-lookup"><span data-stu-id="cb5de-280">Select **ChangeSetPointTemp** from the command list and set **SetPointTemp** to 45.</span></span> <span data-ttu-id="cb5de-281">Potom zvolte **Odeslat příkaz**:</span><span class="sxs-lookup"><span data-stu-id="cb5de-281">Then choose **Send Command**:</span></span>

    ![Přidání pravidla pro zařízení][img-adddevicerule3]

1. <span data-ttu-id="cb5de-283">Vraťte se zpět na řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="cb5de-283">Navigate back to the dashboard.</span></span> <span data-ttu-id="cb5de-284">Po chvilce se v podokně **Historie alarmů** ukáže nový záznam, když teplota hlášená tímto novým zařízením přesáhne hranici 47 stupňů:</span><span class="sxs-lookup"><span data-stu-id="cb5de-284">After a short time, you will see a new entry in the **Alarm History** pane when the temperature reported by your new device exceeds the 47-degree threshold:</span></span>

    ![Přidání pravidla pro zařízení][img-adddevicerule4]

1. <span data-ttu-id="cb5de-286">Všechna pravidla můžete kontrolovat a upravovat na stránce **Pravidla** na řídicím panelu:</span><span class="sxs-lookup"><span data-stu-id="cb5de-286">You can review and edit all your rules on the **Rules** page of the dashboard:</span></span>

    ![Zobrazení pravidel zařízení][img-rules]

1. <span data-ttu-id="cb5de-288">Všechny akce, které se dají provést v reakci na některé pravidlo, můžete zkontrolovat a upravit na stránce **Akce** na řídicím panelu:</span><span class="sxs-lookup"><span data-stu-id="cb5de-288">You can review and edit all the actions that can be taken in response to a rule on the **Actions** page of the dashboard:</span></span>

    ![Zobrazení akcí zařízení][img-actions]

> [!NOTE]
> <span data-ttu-id="cb5de-290">Můžete definovat akce, které budou pomocí funkce [Logic Apps][lnk-logic-apps] odesílat zprávy e-mailem nebo jako SMS v reakci na pravidlo nebo v rámci integrace s obchodním systémem.</span><span class="sxs-lookup"><span data-stu-id="cb5de-290">It is possible to define actions that can send an email message or SMS in response to a rule or integrate with a line-of-business system through a [Logic App][lnk-logic-apps].</span></span> <span data-ttu-id="cb5de-291">Další informace najdete v tématu [Propojení funkce Logic Apps s předkonfigurovaným řešením vzdáleného monitorování sady Azure IoT Suite][lnk-logicapptutorial].</span><span class="sxs-lookup"><span data-stu-id="cb5de-291">For more information, see the [Connect Logic App to your Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logicapptutorial].</span></span>

### <a name="manage-filters"></a><span data-ttu-id="cb5de-292">Správa filtrů</span><span class="sxs-lookup"><span data-stu-id="cb5de-292">Manage filters</span></span>

<span data-ttu-id="cb5de-293">V seznamu zařízení můžete vytvořit, uložit a znovu načíst filtry, pomocí kterých zobrazíte přizpůsobený seznam zařízení připojených k vaší službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="cb5de-293">In the device list, you can create, save, and reload filters to display a customized list of devices connected to your hub.</span></span> <span data-ttu-id="cb5de-294">Vytvoření filtru:</span><span class="sxs-lookup"><span data-stu-id="cb5de-294">To create a filter:</span></span>

1. <span data-ttu-id="cb5de-295">Zvolte ikonu filtru nad seznamem zařízení:</span><span class="sxs-lookup"><span data-stu-id="cb5de-295">Choose the edit filter icon above the list of devices:</span></span>

    ![Otevření editoru filtru][img-editfiltericon]

1. <span data-ttu-id="cb5de-297">V **Editoru filtru** přidejte pole, operátory a hodnoty, podle kterých chcete filtrovat seznam zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-297">In the **Filter editor**, add the fields, operators, and values to filter the device list.</span></span> <span data-ttu-id="cb5de-298">Filtr můžete upřesnit přidáním více klauzulí.</span><span class="sxs-lookup"><span data-stu-id="cb5de-298">You can add multiple clauses to refine your filter.</span></span> <span data-ttu-id="cb5de-299">Zvolením **Filtrovat** použijte filtr:</span><span class="sxs-lookup"><span data-stu-id="cb5de-299">Choose **Filter** to apply the filter:</span></span>

    ![Vytvoření filtru][img-filtereditor]

1. <span data-ttu-id="cb5de-301">V tomto příkladu je seznam filtrovaný podle výrobce a čísla modelu:</span><span class="sxs-lookup"><span data-stu-id="cb5de-301">In this example, the list is filtered by manufacturer and model number:</span></span>

    ![Filtrovaný seznam][img-filterelist]

1. <span data-ttu-id="cb5de-303">Pokud chcete filtr uložit s vlastním názvem, zvolte ikonu **Uložit jako**:</span><span class="sxs-lookup"><span data-stu-id="cb5de-303">To save your filter with a custom name, choose the **Save as** icon:</span></span>

    ![Uložení filtru][img-savefilter]

1. <span data-ttu-id="cb5de-305">Pokud chcete znovu použít dříve uložený filtr, zvolte ikonu **Otevřít uložený filtr**:</span><span class="sxs-lookup"><span data-stu-id="cb5de-305">To reapply a filter you saved previously, choose the **Open saved filter** icon:</span></span>

    ![Otevření filtru][img-openfilter]

<span data-ttu-id="cb5de-307">Můžete vytvořit filtry na základě ID zařízení, stavu zařízení, požadovaných vlastností, ohlášených vlastností a značek.</span><span class="sxs-lookup"><span data-stu-id="cb5de-307">You can create filters based on device id, device state, desired properties, reported properties, and tags.</span></span> <span data-ttu-id="cb5de-308">Vlastní značky můžete k zařízení přidat v části **Značky** panelu **Podrobnosti o zařízení** nebo spuštěním úlohy, která aktualizuje značky na více zařízeních.</span><span class="sxs-lookup"><span data-stu-id="cb5de-308">You add your own custom tags to a device in the **Tags** section of the **Device Details** panel, or run a job to update tags on multiple devices.</span></span>

> [!NOTE]
> <span data-ttu-id="cb5de-309">V **Editoru filtru** můžete pomocí **Rozšířeného zobrazení** upravit přímo text dotazu.</span><span class="sxs-lookup"><span data-stu-id="cb5de-309">In the **Filter editor**, you can use the **Advanced view** to edit the query text directly.</span></span>

### <a name="commands"></a><span data-ttu-id="cb5de-310">Příkazy</span><span class="sxs-lookup"><span data-stu-id="cb5de-310">Commands</span></span>

<span data-ttu-id="cb5de-311">Na panelu **Podrobnosti o zařízení** můžete odesílat příkazy do zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-311">From the **Device Details** panel, you can send commands to the device.</span></span> <span data-ttu-id="cb5de-312">Jakmile zařízení poprvé spustíte, odešle do řešení informace o příkazech, které podporuje.</span><span class="sxs-lookup"><span data-stu-id="cb5de-312">When a device first starts, it sends information about the commands it supports to the solution.</span></span> <span data-ttu-id="cb5de-313">Diskuzi o rozdílech mezi příkazy a metodami najdete v tématu [Možnosti komunikace typu zařízení-cloud ve službě IoT Hub][lnk-c2d-guidance].</span><span class="sxs-lookup"><span data-stu-id="cb5de-313">For a discussion of the differences between commands and methods, see [Azure IoT Hub cloud-to-device options][lnk-c2d-guidance].</span></span>

1. <span data-ttu-id="cb5de-314">Na panelu **Podrobnosti o zařízení** pro vybrané zařízení zvolte **Příkazy**:</span><span class="sxs-lookup"><span data-stu-id="cb5de-314">Choose **Commands** in the **Device Details** panel for the selected device:</span></span>

   ![Příkazy zařízení na řídicím panelu][img-devicecommands]

1. <span data-ttu-id="cb5de-316">Ze seznamu příkazů vyberte **PingDevice** (Otestovat zařízení příkazem ping).</span><span class="sxs-lookup"><span data-stu-id="cb5de-316">Select **PingDevice** from the command list.</span></span>

1. <span data-ttu-id="cb5de-317">Zvolte **Odeslat příkaz**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-317">Choose **Send Command**.</span></span>

1. <span data-ttu-id="cb5de-318">Stav příkazu uvidíte v historii příkazů.</span><span class="sxs-lookup"><span data-stu-id="cb5de-318">You can see the status of the command in the command history.</span></span>

   ![Stav příkazu na řídicím panelu][img-pingcommand]

<span data-ttu-id="cb5de-320">Řešení sleduje stav každého příkazu, který odešle.</span><span class="sxs-lookup"><span data-stu-id="cb5de-320">The solution tracks the status of each command it sends.</span></span> <span data-ttu-id="cb5de-321">Zpočátku je výsledek uveden jako **Čekající**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-321">Initially the result is **Pending**.</span></span> <span data-ttu-id="cb5de-322">Když zařízení ohlásí, že příkaz provedlo, výsledek je nastaven na **Úspěch**.</span><span class="sxs-lookup"><span data-stu-id="cb5de-322">When the device reports that it has executed the command, the result is set to **Success**.</span></span>

## <a name="behind-the-scenes"></a><span data-ttu-id="cb5de-323">Informace pro pokročilé uživatele</span><span class="sxs-lookup"><span data-stu-id="cb5de-323">Behind the scenes</span></span>

<span data-ttu-id="cb5de-324">Když nasadíte předkonfigurované řešení, proces nasazení vytvoří ve vybraném předplatném Azure několik prostředků.</span><span class="sxs-lookup"><span data-stu-id="cb5de-324">When you deploy a preconfigured solution, the deployment process creates multiple resources in the Azure subscription you selected.</span></span> <span data-ttu-id="cb5de-325">Tyto prostředky můžete zobrazit na webu [Azure Portal][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="cb5de-325">You can view these resources in the Azure [portal][lnk-portal].</span></span> <span data-ttu-id="cb5de-326">Proces nasazení vytváří **skupinu prostředků**. Její název bude vycházet z názvu, který jste vybrali pro předkonfigurované řešení:</span><span class="sxs-lookup"><span data-stu-id="cb5de-326">The deployment process creates a **resource group** with a name based on the name you choose for your preconfigured solution:</span></span>

![Předkonfigurované řešení na portálu Azure Portal][img-portal]

<span data-ttu-id="cb5de-328">Nastavení každého prostředku se zobrazí, když jej vyberete v seznamu ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="cb5de-328">You can view the settings of each resource by selecting it in the list of resources in the resource group.</span></span>

<span data-ttu-id="cb5de-329">Můžete taky zobrazit zdrojový kód pro předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-329">You can also view the source code for the preconfigured solution.</span></span> <span data-ttu-id="cb5de-330">Zdrojový kód předkonfigurovaného řešení vzdáleného monitorování najdete v úložišti GitHub [azure-iot-remote-monitoring][lnk-rmgithub]:</span><span class="sxs-lookup"><span data-stu-id="cb5de-330">The remote monitoring preconfigured solution source code is in the [azure-iot-remote-monitoring][lnk-rmgithub] GitHub repository:</span></span>

* <span data-ttu-id="cb5de-331">Složka **DeviceAdministration** (Správa zařízení) obsahuje zdrojový kód pro řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="cb5de-331">The **DeviceAdministration** folder contains the source code for the dashboard.</span></span>
* <span data-ttu-id="cb5de-332">Složka **Simulator** (Simulátor) obsahuje zdrojový kód pro simulované zařízení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-332">The **Simulator** folder contains the source code for the simulated device.</span></span>
* <span data-ttu-id="cb5de-333">Složka **EventProcessor** (Procesor událostí) obsahuje zdrojový kód pro proces back-end, který zpracovává příchozí telemetrii.</span><span class="sxs-lookup"><span data-stu-id="cb5de-333">The **EventProcessor** folder contains the source code for the back-end process that handles the incoming telemetry.</span></span>

<span data-ttu-id="cb5de-334">Jakmile budete hotovi, můžete předkonfigurované řešení z vašeho předplatného Azure odstranit na webu [azureiotsuite.com][lnk-azureiotsuite].</span><span class="sxs-lookup"><span data-stu-id="cb5de-334">When you are done, you can delete the preconfigured solution from your Azure subscription on the [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="cb5de-335">Tento web umožňuje snadno odstranit všechny prostředky, které byly zřízeny při vytvoření předkonfigurovaného řešení.</span><span class="sxs-lookup"><span data-stu-id="cb5de-335">This site enables you to easily delete all the resources that were provisioned when you created the preconfigured solution.</span></span>

> [!NOTE]
> <span data-ttu-id="cb5de-336">Abyste zajistili, že jste odstranili opravdu všechno spojené s předkonfigurovaným řešením, odstraňte řešení na webu [azureiotsuite.com][lnk-azureiotsuite] – neodstraňujte jenom skupinu prostředků na portálu.</span><span class="sxs-lookup"><span data-stu-id="cb5de-336">To ensure that you delete everything related to the preconfigured solution, delete it on the [azureiotsuite.com][lnk-azureiotsuite] site and do not delete the resource group in the portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb5de-337">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb5de-337">Next Steps</span></span>

<span data-ttu-id="cb5de-338">Když jste teď nasadili fungující předkonfigurované řešení, můžete pokračovat v seznamování se sadou IoT Suite přečtením následujících článků:</span><span class="sxs-lookup"><span data-stu-id="cb5de-338">Now that you’ve deployed a working preconfigured solution, you can continue getting started with IoT Suite by reading the following articles:</span></span>

* <span data-ttu-id="cb5de-339">[Návod pro předkonfigurované řešení vzdáleného monitorování][lnk-rm-walkthrough]</span><span class="sxs-lookup"><span data-stu-id="cb5de-339">[Remote monitoring preconfigured solution walkthrough][lnk-rm-walkthrough]</span></span>
* <span data-ttu-id="cb5de-340">[Připojení zařízení k předkonfigurovanému řešení vzdáleného monitorování][lnk-connect-rm]</span><span class="sxs-lookup"><span data-stu-id="cb5de-340">[Connect your device to the remote monitoring preconfigured solution][lnk-connect-rm]</span></span>
* <span data-ttu-id="cb5de-341">[Oprávnění na webu azureiotsuite.com][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="cb5de-341">[Permissions on the azureiotsuite.com site][lnk-permissions]</span></span>

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