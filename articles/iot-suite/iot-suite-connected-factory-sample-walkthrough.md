---
title: "Průvodce řešením propojené továrny v sadě Azure IoT Suite | Dokumentace Microsoftu"
description: "Popis předkonfigurovaného řešení Azure IoT pro propojenou továrnu a jeho architektura."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: 517e908a744734139ed0aeee314a4f3b9eda86cc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connected-factory-preconfigured-solution-walkthrough"></a><span data-ttu-id="47c1d-103">Průvodce předkonfigurovaným řešením propojené továrny</span><span class="sxs-lookup"><span data-stu-id="47c1d-103">Connected factory preconfigured solution walkthrough</span></span>

<span data-ttu-id="47c1d-104">[Předkonfigurované řešení][lnk-preconfigured-solutions] sady IoT Suite pro propojenou továrnu je implementace komplexního průmyslového řešení, které:</span><span class="sxs-lookup"><span data-stu-id="47c1d-104">The IoT Suite connected factory [preconfigured solution][lnk-preconfigured-solutions] is an implementation of an end-to-end industrial solution that:</span></span>

* <span data-ttu-id="47c1d-105">Se připojuje k simulovaným průmyslovým zařízením se servery OPC UA na simulovaných výrobních linkách i ke skutečným zařízením serveru OPC UA.</span><span class="sxs-lookup"><span data-stu-id="47c1d-105">Connects to both simulated industrial devices running OPC UA servers in simulated factory production lines, and real OPC UA server devices.</span></span> <span data-ttu-id="47c1d-106">Další informace o OPC UA najdete v tématu [Propojená továrna – Nejčastější dotazy](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="47c1d-106">For more information about OPC UA, see the [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>
* <span data-ttu-id="47c1d-107">Ukazuje klíčové ukazatele výkonu a celkovou efektivitu těchto zařízení a výrobních linek.</span><span class="sxs-lookup"><span data-stu-id="47c1d-107">Shows operational KPIs and OEE of those devices and production lines.</span></span>
* <span data-ttu-id="47c1d-108">Ukazuje možnosti použití cloudové aplikace k interakci se serverovými systémy OPC UA.</span><span class="sxs-lookup"><span data-stu-id="47c1d-108">Demonstrates how a cloud-based application could be used to interact with OPC UA server systems.</span></span>
* <span data-ttu-id="47c1d-109">Umožňuje připojení vlastních zařízení serveru OPC UA.</span><span class="sxs-lookup"><span data-stu-id="47c1d-109">Enables you to connect your own OPC UA server devices.</span></span>
* <span data-ttu-id="47c1d-110">Umožňuje procházení a úpravy dat na serveru OPC UA.</span><span class="sxs-lookup"><span data-stu-id="47c1d-110">Enables you to browse and modify the OPC UA server data.</span></span>
* <span data-ttu-id="47c1d-111">Se integruje se službou Azure Time Series Insights (TSI), aby mohlo poskytovat přizpůsobená zobrazení dat ze serverů OPC UA.</span><span class="sxs-lookup"><span data-stu-id="47c1d-111">Integrates with the Azure Time Series Insights (TSI) service to provide customized views of the data from your OPC UA servers.</span></span>

<span data-ttu-id="47c1d-112">Můžete ho použít jako výchozí bod pro vlastní implementaci a [přizpůsobit][lnk-customize] si ho tak, aby splňovalo vaše konkrétní obchodní požadavky.</span><span class="sxs-lookup"><span data-stu-id="47c1d-112">You can use the solution as a starting point for your own implementation and [customize][lnk-customize] it to meet your own specific business requirements.</span></span>

<span data-ttu-id="47c1d-113">Tento článek vás provede některými z klíčových prvků řešení propojené továrny, aby vám pomohl pochopit, jak toto řešení funguje.</span><span class="sxs-lookup"><span data-stu-id="47c1d-113">This article walks you through some of the key elements of the connected factory solution to enable you to understand how it works.</span></span> <span data-ttu-id="47c1d-114">Díky tomu budete moct:</span><span class="sxs-lookup"><span data-stu-id="47c1d-114">This knowledge helps you to:</span></span>

* <span data-ttu-id="47c1d-115">Odstraňovat potíže v řešení.</span><span class="sxs-lookup"><span data-stu-id="47c1d-115">Troubleshoot issues in the solution.</span></span>
* <span data-ttu-id="47c1d-116">Naplánujte, jak řešení přizpůsobit podle konkrétních požadavků.</span><span class="sxs-lookup"><span data-stu-id="47c1d-116">Plan how to customize to the solution to meet your own specific requirements.</span></span>
* <span data-ttu-id="47c1d-117">Navrhněte vlastní řešení IoT, které používá služby Azure.</span><span class="sxs-lookup"><span data-stu-id="47c1d-117">Design your own IoT solution that uses Azure services.</span></span>

<span data-ttu-id="47c1d-118">Další informace najdete v tématu [Propojená továrna – Nejčastější dotazy](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="47c1d-118">For more information, see the [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>

## <a name="logical-architecture"></a><span data-ttu-id="47c1d-119">Logická architektura</span><span class="sxs-lookup"><span data-stu-id="47c1d-119">Logical architecture</span></span>

<span data-ttu-id="47c1d-120">Následující diagram popisuje logické součásti tohoto předkonfigurovaného řešení:</span><span class="sxs-lookup"><span data-stu-id="47c1d-120">The following diagram outlines the logical components of the preconfigured solution:</span></span>

![Logická architektura propojené továrny][connected-factory-logical]

## <a name="communication-patterns"></a><span data-ttu-id="47c1d-122">Vzory komunikace</span><span class="sxs-lookup"><span data-stu-id="47c1d-122">Communication patterns</span></span>

<span data-ttu-id="47c1d-123">Toto řešení k odesílání telemetrických dat OPC UA do služby IoT Hub ve formátu JSON používá [specifikaci publikování a odběru OPC UA](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/).</span><span class="sxs-lookup"><span data-stu-id="47c1d-123">The solution uses the [OPC UA Pub/Sub specification](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) to send OPC UA telemetry data to IoT Hub in JSON format.</span></span> <span data-ttu-id="47c1d-124">Řešení pro tento účel používá modul [vydavatele OPC](https://github.com/Azure/iot-edge-opc-publisher) pro IoT Edge.</span><span class="sxs-lookup"><span data-stu-id="47c1d-124">The solution uses the [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) IoT Edge module for this purpose.</span></span>

<span data-ttu-id="47c1d-125">Řešení má také ve webové aplikaci integrovaného klienta OPC UA, který navazuje připojení k místním serverům OPC UA.</span><span class="sxs-lookup"><span data-stu-id="47c1d-125">The solution also has an OPC UA client integrated into a web application that can establish connections with on-premises OPC UA servers.</span></span> <span data-ttu-id="47c1d-126">Klient používá [reverzní proxy](https://wikipedia.org/wiki/Reverse_proxy) a s pomocí služby IoT Hub vytváří připojení, aniž by vyžadoval otevřené porty na místní bráně firewall.</span><span class="sxs-lookup"><span data-stu-id="47c1d-126">The client uses a [reverse-proxy](https://wikipedia.org/wiki/Reverse_proxy) and receives help from IoT Hub to make the connection without requiring open ports in the on-premises firewall.</span></span> <span data-ttu-id="47c1d-127">Tento vzor komunikace se nazývá [komunikace s asistencí služby](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/).</span><span class="sxs-lookup"><span data-stu-id="47c1d-127">This communication pattern is called [service-assisted communication](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/).</span></span> <span data-ttu-id="47c1d-128">Řešení pro tento účel používá modul [proxy serveru OPC](https://github.com/Azure/iot-edge-opc-proxy/) pro IoT Edge.</span><span class="sxs-lookup"><span data-stu-id="47c1d-128">The solution uses the [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT Edge module for this purpose.</span></span>


## <a name="simulation"></a><span data-ttu-id="47c1d-129">Simulace</span><span class="sxs-lookup"><span data-stu-id="47c1d-129">Simulation</span></span>

<span data-ttu-id="47c1d-130">Simulované stanice a systémy řízení výroby tvoří výrobní linku továrny.</span><span class="sxs-lookup"><span data-stu-id="47c1d-130">The simulated stations and the simulated manufacturing execution systems (MES) make up a factory production line.</span></span> <span data-ttu-id="47c1d-131">Simulovaná zařízení a modul vydavatele OPC jsou založeny na [standardu OPC UA .NET][lnk-OPC-UA-NET-Standard] vydaném nadací OPC Foundation.</span><span class="sxs-lookup"><span data-stu-id="47c1d-131">The simulated devices and the OPC Publisher Module are based on the [OPC UA .NET Standard][lnk-OPC-UA-NET-Standard] published by the OPC Foundation.</span></span>

<span data-ttu-id="47c1d-132">Proxy server OPC a vydavatel OPC jsou implementovány jako moduly založené na [Azure IoT Edge][lnk-Azure-IoT-Gateway].</span><span class="sxs-lookup"><span data-stu-id="47c1d-132">The OPC Proxy and OPC Publisher are implemented as modules based on [Azure IoT Edge][lnk-Azure-IoT-Gateway].</span></span> <span data-ttu-id="47c1d-133">Každá simulovaná výrobní linka má připojenu vyhrazenou bránu.</span><span class="sxs-lookup"><span data-stu-id="47c1d-133">Each simulated production line has a designated gateway attached.</span></span>

<span data-ttu-id="47c1d-134">Všechny simulované komponenty jsou spuštěné v kontejnerech Dockeru hostovaných na virtuálních počítačích Azure s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="47c1d-134">All simulation components run in Docker containers  hosted in an Azure Linux VM.</span></span> <span data-ttu-id="47c1d-135">Simulace je ve výchozím nastavení nakonfigurovaná tak, aby bylo spuštěno 8 simulovaných výrobních linek.</span><span class="sxs-lookup"><span data-stu-id="47c1d-135">The simulation is configured to run eight simulated production lines by default.</span></span>

## <a name="simulated-production-line"></a><span data-ttu-id="47c1d-136">Simulovaná výrobní linka</span><span class="sxs-lookup"><span data-stu-id="47c1d-136">Simulated production line</span></span>

<span data-ttu-id="47c1d-137">Výrobní linka vyrábí součásti.</span><span class="sxs-lookup"><span data-stu-id="47c1d-137">A production line manufactures parts.</span></span> <span data-ttu-id="47c1d-138">Skládá se z různých stanic: montážní stanice, testovací stanice a balicí stanice.</span><span class="sxs-lookup"><span data-stu-id="47c1d-138">It is composed of different stations: an assembly station, a test station, and a packaging station.</span></span>

<span data-ttu-id="47c1d-139">Simulace zpracovává a aktualizuje data vystavená prostřednictvím uzlů OPC UA.</span><span class="sxs-lookup"><span data-stu-id="47c1d-139">The simulation runs and updates the data that is exposed through the OPC UA nodes.</span></span> <span data-ttu-id="47c1d-140">Všechny stanice simulované výrobní linky jsou orchestrované systémem řízení výroby (MES) prostřednictvím OPC UA.</span><span class="sxs-lookup"><span data-stu-id="47c1d-140">All simulated production line stations are orchestrated by the MES through OPC UA.</span></span>

## <a name="simulated-manufacturing-execution-system"></a><span data-ttu-id="47c1d-141">Simulovaný systém řízení výroby</span><span class="sxs-lookup"><span data-stu-id="47c1d-141">Simulated manufacturing execution system</span></span>

<span data-ttu-id="47c1d-142">Systém řízení výroby monitoruje všechny stanice na výrobní lince prostřednictvím OPC UA a zjišťuje tak změny stavu stanic.</span><span class="sxs-lookup"><span data-stu-id="47c1d-142">The MES monitors each station in the production line through OPC UA to detect station status changes.</span></span> <span data-ttu-id="47c1d-143">Voláním metod OPC UA řídí stanice a předává produkt z jedné stanice do další, dokud se proces nedokončí.</span><span class="sxs-lookup"><span data-stu-id="47c1d-143">It calls OPC UA methods to control the stations and passes a product from one station to the next until it is complete.</span></span>

## <a name="gateway-opc-publisher-module"></a><span data-ttu-id="47c1d-144">Modul vydavatele brány OPC</span><span class="sxs-lookup"><span data-stu-id="47c1d-144">Gateway OPC publisher module</span></span>

<span data-ttu-id="47c1d-145">Modul vydavatele OPC se připojuje ke staničním serverům OPC UA a přihlašuje se k odběru uzlů OPC, které se zřídí.</span><span class="sxs-lookup"><span data-stu-id="47c1d-145">OPC Publisher Module connects to the station OPC UA servers and subscribes to the OPC nodes to be published.</span></span> <span data-ttu-id="47c1d-146">Modul převádí data z uzlu do formátu JSON, šifruje je a odesílá je do služby IoT Hub jako zprávy publikování a odběru OPC UA.</span><span class="sxs-lookup"><span data-stu-id="47c1d-146">The module converts the node data into JSON format, encrypts it, and sends it to IoT Hub as OPC UA Pub/Sub messages.</span></span>

<span data-ttu-id="47c1d-147">Modul vydavatele OPC vyžaduje pouze výchozí port HTTPS (443) a může fungovat se stávající podnikovou infrastrukturou.</span><span class="sxs-lookup"><span data-stu-id="47c1d-147">The OPC Publisher module only requires an outbound https port (443) and can work with existing enterprise infrastructure.</span></span>

## <a name="gateway-opc-proxy-module"></a><span data-ttu-id="47c1d-148">Modul proxy serveru brány OPC</span><span class="sxs-lookup"><span data-stu-id="47c1d-148">Gateway OPC proxy module</span></span>

<span data-ttu-id="47c1d-149">Modul proxy serveru brány OPC UA tuneluje binární příkazy OPC UA a řídicí zprávy a vyžaduje pouze výchozí port HTTPS (443).</span><span class="sxs-lookup"><span data-stu-id="47c1d-149">The Gateway OPC UA Proxy Module tunnels binary OPC UA command and control messages and only requires an outbound https port (443).</span></span> <span data-ttu-id="47c1d-150">Může fungovat s existující podnikovou infrastrukturou, včetně webových proxy serverů.</span><span class="sxs-lookup"><span data-stu-id="47c1d-150">It can work with existing enterprise infrastructure, including Web Proxies.</span></span>

<span data-ttu-id="47c1d-151">Pomocí metod zařízení ve službě IoT Hub přenáší do balíčků zabalená data protokolu TCP/IP na úrovni aplikace a tak zajišťuje zabezpečení koncových bodů, šifrování dat a integritu pomocí protokolu SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="47c1d-151">It uses IoT Hub Device methods to transfer packetized TCP/IP data at the application layer and thus ensures endpoint trust, data encryption, and integrity using SSL/TLS.</span></span>

<span data-ttu-id="47c1d-152">Samotný binární protokol OPC UA předávaný přes proxy server používá ověřování a šifrování pomocí UA.</span><span class="sxs-lookup"><span data-stu-id="47c1d-152">The OPC UA binary protocol relayed through the proxy itself uses UA authentication and encryption.</span></span>

## <a name="azure-time-series-insights"></a><span data-ttu-id="47c1d-153">Azure Time Series Insights</span><span class="sxs-lookup"><span data-stu-id="47c1d-153">Azure Time Series Insights</span></span>

<span data-ttu-id="47c1d-154">Modul vydavatele brány OPC se přihlašuje k odběru uzlů serveru OPC UA, aby mohl zjišťovat změny hodnot dat.</span><span class="sxs-lookup"><span data-stu-id="47c1d-154">The Gateway OPC Publisher Module subscribes to OPC UA server nodes to detect change in the data values.</span></span> <span data-ttu-id="47c1d-155">Pokud se v některém z uzlů zjistí změna dat, modul odešle zprávy do služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="47c1d-155">If a data change is detected in one of the nodes, this module then sends messages to Azure IoT Hub.</span></span>

<span data-ttu-id="47c1d-156">Služba IoT Hub poskytuje zdroj událostí pro službu Azure TSI.</span><span class="sxs-lookup"><span data-stu-id="47c1d-156">IoT Hub provides an event source to Azure TSI.</span></span> <span data-ttu-id="47c1d-157">TSI data ukládá po dobu 30 dní na základě časových razítek připojených ke zprávám.</span><span class="sxs-lookup"><span data-stu-id="47c1d-157">TSI stores data for 30 days based on timestamps attached to the messages.</span></span> <span data-ttu-id="47c1d-158">Mezi tato data patří:</span><span class="sxs-lookup"><span data-stu-id="47c1d-158">This data includes:</span></span>

* <span data-ttu-id="47c1d-159">Identifikátor OPC UA ApplicationUri</span><span class="sxs-lookup"><span data-stu-id="47c1d-159">OPC UA ApplicationUri</span></span>
* <span data-ttu-id="47c1d-160">Identifikátor OPC UA NodeId</span><span class="sxs-lookup"><span data-stu-id="47c1d-160">OPC UA NodeId</span></span>
* <span data-ttu-id="47c1d-161">Hodnota uzlu</span><span class="sxs-lookup"><span data-stu-id="47c1d-161">Value of the node</span></span>
* <span data-ttu-id="47c1d-162">Časové razítko zdroje</span><span class="sxs-lookup"><span data-stu-id="47c1d-162">Source timestamp</span></span>
* <span data-ttu-id="47c1d-163">Název OPC UA DisplayName</span><span class="sxs-lookup"><span data-stu-id="47c1d-163">OPC UA DisplayName</span></span>

<span data-ttu-id="47c1d-164">TSI aktuálně neumožňuje změnu doby, po kterou se data mají uchovat.</span><span class="sxs-lookup"><span data-stu-id="47c1d-164">Currently, TSI does not allow customers to customize how long they wish to keep the data for.</span></span>

<span data-ttu-id="47c1d-165">Dotazy služby TSI na data uzlů se provádí pomocí SearchSpan (Time.From a Time.To) a agregace podle identifikátoru OPC UA ApplicationUri nebo OPC UA NodeId nebo názvu OPC UA DisplayName.</span><span class="sxs-lookup"><span data-stu-id="47c1d-165">TSI queries against node data using a SearchSpan (Time.From, Time.To) and aggregates by OPC UA ApplicationUri or OPC UA NodeId or OPC UA DisplayName.</span></span>

<span data-ttu-id="47c1d-166">Při načítání dat pro měřidla celkové efektivity zařízení, klíčových ukazatelů výkonu a grafů časových řad se data agregují podle počtu událostí, celkového součtu, průměrného součtu, minima a maxima.</span><span class="sxs-lookup"><span data-stu-id="47c1d-166">To retrieve the data for the OEE and KPI gauges, and the time series charts, data is aggregated by count of events, Sum, Avg, Min, and Max.</span></span>

<span data-ttu-id="47c1d-167">Časové řady se sestavují jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="47c1d-167">The time series are built using a different process.</span></span> <span data-ttu-id="47c1d-168">Celková efektivita zařízení a klíčové ukazatele výkonu se počítají ze základních dat stanic a pronikají do topologie (výrobní linky, továrny, podnik) aplikace.</span><span class="sxs-lookup"><span data-stu-id="47c1d-168">OEE and KPIs are calculated from station base data and bubbled up for the topology (production lines, factories, enterprise) in the application.</span></span>

<span data-ttu-id="47c1d-169">Kromě toho se časové řady pro celkovou efektivitu zařízení a klíčové ukazatele výkonu počítají v aplikaci kdykoli, kdy je připravený zobrazený časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="47c1d-169">Additionally, time series for OEE and KPI topology is calculated in the app, whenever a displayed timespan is ready.</span></span> <span data-ttu-id="47c1d-170">Například zobrazení dne se aktualizuje každou celou hodinu.</span><span class="sxs-lookup"><span data-stu-id="47c1d-170">For example, the day view is updated every full hour.</span></span>

<span data-ttu-id="47c1d-171">Zobrazení časových řad dat uzlu přichází přímo z TSI a používá agregaci časového rozsahu.</span><span class="sxs-lookup"><span data-stu-id="47c1d-171">The time series view of node data comes directly from TSI using an aggregation for timespan.</span></span>

## <a name="iot-hub"></a><span data-ttu-id="47c1d-172">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="47c1d-172">IoT Hub</span></span>
<span data-ttu-id="47c1d-173">Služba [IoT Hub][lnk-IoT Hub] přijímá data odesílaná z modulu vydavatele OPC do cloudu a zpřístupňuje je službě Azure TSI.</span><span class="sxs-lookup"><span data-stu-id="47c1d-173">The [IoT hub][lnk-IoT Hub] receives data sent from the OPC Publisher Module into the cloud and makes it available to the Azure TSI service.</span></span> 

<span data-ttu-id="47c1d-174">Služba IoT Hub v tomto řešení také:</span><span class="sxs-lookup"><span data-stu-id="47c1d-174">The IoT Hub in the solution also:</span></span>
- <span data-ttu-id="47c1d-175">Udržuje registr identit, ve kterém jsou uložena ID všech modulů vydavatele OPC a všech modulů proxy serveru OPC.</span><span class="sxs-lookup"><span data-stu-id="47c1d-175">Maintains an identity registry that stores the IDs for all OPC Publisher Module and all OPC Proxy Modules.</span></span>
- <span data-ttu-id="47c1d-176">Pro obousměrnou komunikaci modulu proxy serveru OPC používá přenosový kanál.</span><span class="sxs-lookup"><span data-stu-id="47c1d-176">Is used as transport channel for bidirectional communication of the OPC Proxy Module.</span></span>

## <a name="azure-storage"></a><span data-ttu-id="47c1d-177">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="47c1d-177">Azure Storage</span></span>
<span data-ttu-id="47c1d-178">Řešení používá jako diskové úložiště pro virtuální počítač a k ukládání dat nasazení službu Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="47c1d-178">The solution uses Azure blob storage as disk storage for the VM and to store deployment data.</span></span>

## <a name="web-app"></a><span data-ttu-id="47c1d-179">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="47c1d-179">Web app</span></span>
<span data-ttu-id="47c1d-180">Webová aplikace nasazená jako součást předkonfigurovaného řešení se skládá z integrovaného klienta OPC UA, zpracování upozornění a vizualizace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="47c1d-180">The web app deployed as part of the preconfigured solution comprises of an integrated OPC UA client, alerts processing and telemetry visualization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="47c1d-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="47c1d-181">Next steps</span></span>

<span data-ttu-id="47c1d-182">Další informace o sadě IoT Suite najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="47c1d-182">You can continue getting started with IoT Suite by reading the following articles:</span></span>

* <span data-ttu-id="47c1d-183">[Oprávnění na webu azureiotsuite.com][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="47c1d-183">[Permissions on the azureiotsuite.com site][lnk-permissions]</span></span>
* [<span data-ttu-id="47c1d-184">Nasazení brány pro předkonfigurované řešení propojené továrny ve Windows nebo v Linuxu</span><span class="sxs-lookup"><span data-stu-id="47c1d-184">Deploy a gateway on Windows or Linux for the connected factory preconfigured solution</span></span>](iot-suite-connected-factory-gateway-deployment.md)

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-suite-permissions.md
