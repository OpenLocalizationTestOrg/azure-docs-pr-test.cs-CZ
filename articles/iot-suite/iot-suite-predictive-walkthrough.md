---
title: "Návod pro prediktivní údržbu | Dokumentace Microsoftu"
description: "Návod pro předkonfigurované řešení prediktivní údržby Azure IoT."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3c48a716-b805-4c99-8177-414cc4bec3de
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: a68a8fdc3976ade0d1036d5ed58c8b2eb6d32a5d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a><span data-ttu-id="d593a-103">Návod pro předkonfigurované řešení prediktivní údržby</span><span class="sxs-lookup"><span data-stu-id="d593a-103">Predictive maintenance preconfigured solution walkthrough</span></span>

<span data-ttu-id="d593a-104">Předkonfigurované řešení prediktivní údržby je uceleným řešením pro podnikový scénář, které se pokouší předvídat bod, ve kterém pravděpodobně nastane chyba.</span><span class="sxs-lookup"><span data-stu-id="d593a-104">The predictive maintenance preconfigured solution is an end-to-end solution for a business scenario that predicts the point at which a failure is likely to occur.</span></span> <span data-ttu-id="d593a-105">Toto předkonfigurované řešení můžete aktivně využívat pro různé činnosti, jako je třeba optimalizace údržby.</span><span class="sxs-lookup"><span data-stu-id="d593a-105">You can use this preconfigured solution proactively for activities such as optimizing maintenance.</span></span> <span data-ttu-id="d593a-106">Řešení kombinuje klíčové služby sady Azure IoT Suite, jako je služba IoT Hub, Stream Analytics a pracovní prostor [Azure Machine Learning][lnk-machine-learning].</span><span class="sxs-lookup"><span data-stu-id="d593a-106">The solution combines key Azure IoT Suite services, such as IoT Hub, Stream analytics, and an [Azure Machine Learning][lnk-machine-learning] workspace.</span></span> <span data-ttu-id="d593a-107">Tento pracovní prostor obsahuje model založený na veřejné ukázkové sadě dat, který předpovídá zbývající dobu životnosti (RUL) leteckého motoru.</span><span class="sxs-lookup"><span data-stu-id="d593a-107">This workspace contains a model, based on a public sample data set, to predict the Remaining Useful Life (RUL) of an aircraft engine.</span></span> <span data-ttu-id="d593a-108">Řešení nabízí úplnou implementaci daného obchodního scénáře IoT jako výchozího bodu pro plánování a implementaci řešení, které vyhovuje vašim konkrétním obchodním požadavkům.</span><span class="sxs-lookup"><span data-stu-id="d593a-108">The solution fully implements the IoT business scenario as a starting point for you to plan and implement a solution that meets your own specific business requirements.</span></span>

## <a name="logical-architecture"></a><span data-ttu-id="d593a-109">Logická architektura</span><span class="sxs-lookup"><span data-stu-id="d593a-109">Logical architecture</span></span>

<span data-ttu-id="d593a-110">Následující diagram popisuje logické součásti tohoto předkonfigurovaného řešení:</span><span class="sxs-lookup"><span data-stu-id="d593a-110">The following diagram outlines the logical components of the preconfigured solution:</span></span>

![][img-architecture]

<span data-ttu-id="d593a-111">Modré položky jsou služby Azure zřízené v oblasti, kam jste nasadili předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="d593a-111">The blue items are Azure services provisioned in the region where you deployed the preconfigured solution.</span></span> <span data-ttu-id="d593a-112">Seznam oblastí, do kterých můžete nasadit předkonfigurované řešení, najdete na [stránce zřizování][lnk-azureiotsuite].</span><span class="sxs-lookup"><span data-stu-id="d593a-112">The list of regions where you can deploy the preconfigured solution displays on the [provisioning page][lnk-azureiotsuite].</span></span>

<span data-ttu-id="d593a-113">Zelená položka je simulované zařízení, které představuje letecký motor.</span><span class="sxs-lookup"><span data-stu-id="d593a-113">The green item is a simulated device that represents an aircraft engine.</span></span> <span data-ttu-id="d593a-114">Další informace o těchto simulovaných zařízeních najdete v následující části.</span><span class="sxs-lookup"><span data-stu-id="d593a-114">You can learn more about these simulated devices in the following section.</span></span>

<span data-ttu-id="d593a-115">Šedé položky představují součásti, které implementují funkce *správy zařízení*.</span><span class="sxs-lookup"><span data-stu-id="d593a-115">The gray items represent components that implement *device management* capabilities.</span></span> <span data-ttu-id="d593a-116">Aktuální verze předkonfigurovaného řešení prediktivní údržby tyto prostředky neposkytuje.</span><span class="sxs-lookup"><span data-stu-id="d593a-116">The current release of the predictive maintenance preconfigured solution does not provision these resources.</span></span> <span data-ttu-id="d593a-117">Další informace o správě zařízení najdete v tématu [Předkonfigurované řešení pro vzdálené monitorování][lnk-remote-monitoring].</span><span class="sxs-lookup"><span data-stu-id="d593a-117">To learn more about device management, refer to the [remote monitoring pre-configured solution][lnk-remote-monitoring].</span></span>

## <a name="simulated-devices"></a><span data-ttu-id="d593a-118">Simulovaná zařízení</span><span class="sxs-lookup"><span data-stu-id="d593a-118">Simulated devices</span></span>

<span data-ttu-id="d593a-119">V předkonfigurovaných řešeních simulované zařízení představuje letecký motor.</span><span class="sxs-lookup"><span data-stu-id="d593a-119">In the preconfigured solution, a simulated device represents an aircraft engine.</span></span> <span data-ttu-id="d593a-120">Řešení obsahuje dva motory, které jsou součástí jednoho letadla.</span><span class="sxs-lookup"><span data-stu-id="d593a-120">The solution is provisioned with two engines that map to a single aircraft.</span></span> <span data-ttu-id="d593a-121">Každý motor vysílá čtyři typy telemetrických dat: ze snímačů Sensor 9, Sensor 11, Sensor 14 a Sensor 15, které poskytují data potřebná k tomu, aby mohl model Machine Learning vypočítat zbývající dobu životnosti pro tento motor.</span><span class="sxs-lookup"><span data-stu-id="d593a-121">Each engine emits four types of telemetry: Sensor 9, Sensor 11, Sensor 14, and Sensor 15 provide the data necessary for the Machine Learning model to calculate the RUL for the engine.</span></span> <span data-ttu-id="d593a-122">Každé simulované zařízení posílá do služby IoT Hub následující telemetrické zprávy:</span><span class="sxs-lookup"><span data-stu-id="d593a-122">Each simulated device sends the following telemetry messages to IoT Hub:</span></span>

<span data-ttu-id="d593a-123">*Počet cyklů*.</span><span class="sxs-lookup"><span data-stu-id="d593a-123">*Cycle count*.</span></span> <span data-ttu-id="d593a-124">Cyklus představuje dokončený let trvající 2 až 10 hodin.</span><span class="sxs-lookup"><span data-stu-id="d593a-124">A cycle represents a completed flight with a duration between two and ten hours.</span></span> <span data-ttu-id="d593a-125">Během letu se telemetrická data zaznamenávají každou půlhodinu.</span><span class="sxs-lookup"><span data-stu-id="d593a-125">During the flight, telemetry data is captured every half hour.</span></span>

<span data-ttu-id="d593a-126">*Telemetrická data*.</span><span class="sxs-lookup"><span data-stu-id="d593a-126">*Telemetry*.</span></span> <span data-ttu-id="d593a-127">V řešení jsou čtyři snímače, které představují vlastnosti motoru.</span><span class="sxs-lookup"><span data-stu-id="d593a-127">There are four sensors that represent engine attributes.</span></span> <span data-ttu-id="d593a-128">Snímače jsou obecně označeny jako Sensor 9, Sensor 11, Sensor 14 a Sensor 15.</span><span class="sxs-lookup"><span data-stu-id="d593a-128">The sensors are generically labeled Sensor 9, Sensor 11, Sensor 14, and Sensor 15.</span></span> <span data-ttu-id="d593a-129">Tyto čtyři snímače představují dostatek telemetrických dat pro získání užitečných výsledků z modelu zbývající doby životnosti.</span><span class="sxs-lookup"><span data-stu-id="d593a-129">These four sensors represent telemetry sufficient to obtain useful results from the RUL model.</span></span> <span data-ttu-id="d593a-130">Model použitý v předkonfigurovaném řešení je vytvořený z veřejné sady dat, obsahující data snímačů reálného motoru.</span><span class="sxs-lookup"><span data-stu-id="d593a-130">The model used in the preconfigured solution is created from a public data set that includes real engine sensor data.</span></span> <span data-ttu-id="d593a-131">Další informace o způsobu vytvoření modelu z původní sady dat najdete v tématu [Šablona prediktivní údržby v galerii Cortana Intelligence][lnk-cortana-analytics].</span><span class="sxs-lookup"><span data-stu-id="d593a-131">For more information on how the model was created from the original data set, see the [Cortana Intelligence Gallery Predictive Maintenance Template][lnk-cortana-analytics].</span></span>

<span data-ttu-id="d593a-132">Simulovaná zařízení mohou v řešení zpracovávat následující příkazy odeslané ze služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="d593a-132">The simulated devices can handle the following commands sent from the IoT hub in the solution:</span></span>

| <span data-ttu-id="d593a-133">Příkaz</span><span class="sxs-lookup"><span data-stu-id="d593a-133">Command</span></span> | <span data-ttu-id="d593a-134">Popis</span><span class="sxs-lookup"><span data-stu-id="d593a-134">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d593a-135">StartTelemetry</span><span class="sxs-lookup"><span data-stu-id="d593a-135">StartTelemetry</span></span> |<span data-ttu-id="d593a-136">Řídí stav simulace.</span><span class="sxs-lookup"><span data-stu-id="d593a-136">Controls the state of the simulation.</span></span><br/><span data-ttu-id="d593a-137">Spustí odesílání telemetrických dat ze zařízení</span><span class="sxs-lookup"><span data-stu-id="d593a-137">Starts the device sending telemetry</span></span> |
| <span data-ttu-id="d593a-138">StopTelemetry</span><span class="sxs-lookup"><span data-stu-id="d593a-138">StopTelemetry</span></span> |<span data-ttu-id="d593a-139">Řídí stav simulace.</span><span class="sxs-lookup"><span data-stu-id="d593a-139">Controls the state of the simulation.</span></span><br/><span data-ttu-id="d593a-140">Zastaví odesílání telemetrických dat ze zařízení</span><span class="sxs-lookup"><span data-stu-id="d593a-140">Stops the device sending telemetry</span></span> |

<span data-ttu-id="d593a-141">Služba IoT Hub zajišťuje potvrzení příkazu zařízení.</span><span class="sxs-lookup"><span data-stu-id="d593a-141">IoT Hub provides device command acknowledgment.</span></span>

## <a name="azure-stream-analytics-job"></a><span data-ttu-id="d593a-142">Úlohy služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d593a-142">Azure Stream Analytics job</span></span>

<span data-ttu-id="d593a-143">**Úloha: Telemetrie** funguje v příchozím datovém proudu telemetrických dat ze zařízení pomocí dvou příkazů:</span><span class="sxs-lookup"><span data-stu-id="d593a-143">**Job: Telemetry** operates on the incoming device telemetry stream using two statements:</span></span>

* <span data-ttu-id="d593a-144">První vybere veškerá telemetrická data ze zařízení a odešle je do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d593a-144">The first selects all telemetry from the devices and sends this data to blob storage.</span></span> <span data-ttu-id="d593a-145">Odtud se data vizualizují ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d593a-145">From here, it is visualized in the web app.</span></span>
* <span data-ttu-id="d593a-146">Druhý vypočítá průměrné hodnoty snímačů v rámci dvouminutového posuvného okna a odešle je prostřednictvím centra událostí do **procesoru událostí**.</span><span class="sxs-lookup"><span data-stu-id="d593a-146">The second computes average sensor values over a two-minute sliding window and sends this data through the Event hub to an **event processor**.</span></span>

## <a name="event-processor"></a><span data-ttu-id="d593a-147">Procesor událostí</span><span class="sxs-lookup"><span data-stu-id="d593a-147">Event processor</span></span>
<span data-ttu-id="d593a-148">**Hostitel procesoru událostí** běží ve webové úloze Azure.</span><span class="sxs-lookup"><span data-stu-id="d593a-148">The **event processor host** runs in an Azure Web Job.</span></span> <span data-ttu-id="d593a-149">**Procesor událostí** přebírá průměrné hodnoty snímačů za dokončený cyklus.</span><span class="sxs-lookup"><span data-stu-id="d593a-149">The **event processor** takes the average sensor values for a completed cycle.</span></span> <span data-ttu-id="d593a-150">Potom tyto hodnoty předá do rozhraní API, které zpřístupní trénovaný model pro výpočet zbývající doby životnosti motoru.</span><span class="sxs-lookup"><span data-stu-id="d593a-150">It then passes those values to an API that exposes trained model to calculate the RUL for an engine.</span></span> <span data-ttu-id="d593a-151">Rozhraní API je zveřejněné pracovním prostorem Machine Learning zřízeným jako součást řešení.</span><span class="sxs-lookup"><span data-stu-id="d593a-151">The API is exposed by a Machine Learning workspace that is provisioned as part of the solution.</span></span>

## <a name="machine-learning"></a><span data-ttu-id="d593a-152">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="d593a-152">Machine Learning</span></span>
<span data-ttu-id="d593a-153">Součást Machine Learning používá model odvozený z dat shromážděných z reálných leteckých motorů.</span><span class="sxs-lookup"><span data-stu-id="d593a-153">The Machine Learning component uses a model derived from data collected from real aircraft engines.</span></span> <span data-ttu-id="d593a-154">Do pracovního prostoru Machine Learning se můžete dostat z dlaždice na stránce zřízeného řešení na webu [azureiotsuite.com][lnk-azureiotsuite].</span><span class="sxs-lookup"><span data-stu-id="d593a-154">You can navigate to the Machine Learning workspace from the tile on the [azureiotsuite.com][lnk-azureiotsuite] page for your provisioned solution.</span></span> <span data-ttu-id="d593a-155">Tato dlaždice je k dispozici v případě, že je řešení ve stavu **Připraveno**.</span><span class="sxs-lookup"><span data-stu-id="d593a-155">The tile is available when the solution is in the **Ready** state.</span></span>


## <a name="next-steps"></a><span data-ttu-id="d593a-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d593a-156">Next steps</span></span>
<span data-ttu-id="d593a-157">Když jste se seznámili s klíčovými součástmi předkonfigurovaného řešení prediktivní údržby, můžete si jej přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="d593a-157">Now you've seen the key components of the predictive maintenance preconfigured solution, you may want to customize it.</span></span> <span data-ttu-id="d593a-158">Viz [Doprovodné materiály k přizpůsobení předkonfigurovaných řešení][lnk-customize].</span><span class="sxs-lookup"><span data-stu-id="d593a-158">See [Guidance on customizing preconfigured solutions][lnk-customize].</span></span>

<span data-ttu-id="d593a-159">Můžete si taky prostudovat některé další funkce a možnosti předkonfigurovaných řešení sady IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="d593a-159">You can also explore some of the other features and capabilities of the IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="d593a-160">[Nejčastější dotazy k sadě IoT Suite][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="d593a-160">[Frequently asked questions for IoT Suite][lnk-faq]</span></span>
* <span data-ttu-id="d593a-161">[Zabezpečení IoT od počátku][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="d593a-161">[IoT security from the ground up][lnk-security-groundup]</span></span>

[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png

[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/