---
title: "návod řešení Azure IoT Suite factory aaaConnected | Microsoft Docs"
description: "Popis hello řešení Azure IoT předkonfigurované připojen objekt pro vytváření a jeho architektura."
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
ms.openlocfilehash: 7fd55c51351659401349cfde91a20fce1045b4f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connected-factory-preconfigured-solution-walkthrough"></a><span data-ttu-id="1b50a-103">Průvodce předkonfigurovaným řešením propojené továrny</span><span class="sxs-lookup"><span data-stu-id="1b50a-103">Connected factory preconfigured solution walkthrough</span></span>

<span data-ttu-id="1b50a-104">Hello IoT Suite připojen objekt pro vytváření [předkonfigurované řešení] [ lnk-preconfigured-solutions] je implementací řešení průmyslových začátku do konce který:</span><span class="sxs-lookup"><span data-stu-id="1b50a-104">hello IoT Suite connected factory [preconfigured solution][lnk-preconfigured-solutions] is an implementation of an end-to-end industrial solution that:</span></span>

* <span data-ttu-id="1b50a-105">Připojí tooboth simulated průmyslových zařízení se systémem OPC UA servery v řádky výrobní simulované objekt pro vytváření a skutečné zařízení OPC UA serveru.</span><span class="sxs-lookup"><span data-stu-id="1b50a-105">Connects tooboth simulated industrial devices running OPC UA servers in simulated factory production lines, and real OPC UA server devices.</span></span> <span data-ttu-id="1b50a-106">Další informace o OPC UA najdete v tématu hello [připojené nejčastější dotazy týkající se vytváření](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="1b50a-106">For more information about OPC UA, see hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>
* <span data-ttu-id="1b50a-107">Ukazuje klíčové ukazatele výkonu a celkovou efektivitu těchto zařízení a výrobních linek.</span><span class="sxs-lookup"><span data-stu-id="1b50a-107">Shows operational KPIs and OEE of those devices and production lines.</span></span>
* <span data-ttu-id="1b50a-108">Ukazuje, jak k aplikaci založené na cloudu může být použité toointeract systémech OPC UA server.</span><span class="sxs-lookup"><span data-stu-id="1b50a-108">Demonstrates how a cloud-based application could be used toointeract with OPC UA server systems.</span></span>
* <span data-ttu-id="1b50a-109">Umožňuje vám tooconnect si vlastní zařízení OPC UA serveru.</span><span class="sxs-lookup"><span data-stu-id="1b50a-109">Enables you tooconnect your own OPC UA server devices.</span></span>
* <span data-ttu-id="1b50a-110">Umožňuje toobrowse a upravit hello OPC UA dat serveru.</span><span class="sxs-lookup"><span data-stu-id="1b50a-110">Enables you toobrowse and modify hello OPC UA server data.</span></span>
* <span data-ttu-id="1b50a-111">Vlastní zobrazení dat hello z vašich serverů OPC UA se integruje s hello tooprovide služby Azure časové řady přehledy (TSI).</span><span class="sxs-lookup"><span data-stu-id="1b50a-111">Integrates with hello Azure Time Series Insights (TSI) service tooprovide customized views of hello data from your OPC UA servers.</span></span>

<span data-ttu-id="1b50a-112">Hello řešení můžete použít jako výchozí bod pro vlastní implementaci a [přizpůsobit] [ lnk-customize] ho toomeet specifických podnikových požadavků.</span><span class="sxs-lookup"><span data-stu-id="1b50a-112">You can use hello solution as a starting point for your own implementation and [customize][lnk-customize] it toomeet your own specific business requirements.</span></span>

<span data-ttu-id="1b50a-113">Tento článek vás provede procesem některé klíčové prvky hello hello připojen objekt pro vytváření řešení tooenable toounderstand jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="1b50a-113">This article walks you through some of hello key elements of hello connected factory solution tooenable you toounderstand how it works.</span></span> <span data-ttu-id="1b50a-114">Díky tomu budete moct:</span><span class="sxs-lookup"><span data-stu-id="1b50a-114">This knowledge helps you to:</span></span>

* <span data-ttu-id="1b50a-115">Řešení problémů v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="1b50a-115">Troubleshoot issues in hello solution.</span></span>
* <span data-ttu-id="1b50a-116">Plánování jak toocustomize toohello řešení toomeet vaše konkrétní požadavky.</span><span class="sxs-lookup"><span data-stu-id="1b50a-116">Plan how toocustomize toohello solution toomeet your own specific requirements.</span></span>
* <span data-ttu-id="1b50a-117">Navrhněte vlastní řešení IoT, které používá služby Azure.</span><span class="sxs-lookup"><span data-stu-id="1b50a-117">Design your own IoT solution that uses Azure services.</span></span>

<span data-ttu-id="1b50a-118">Další informace najdete v tématu hello [připojené nejčastější dotazy týkající se vytváření](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="1b50a-118">For more information, see hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>

## <a name="logical-architecture"></a><span data-ttu-id="1b50a-119">Logická architektura</span><span class="sxs-lookup"><span data-stu-id="1b50a-119">Logical architecture</span></span>

<span data-ttu-id="1b50a-120">Hello následující diagram popisuje logické součásti hello hello předkonfigurované řešení:</span><span class="sxs-lookup"><span data-stu-id="1b50a-120">hello following diagram outlines hello logical components of hello preconfigured solution:</span></span>

![Logická architektura propojené továrny][connected-factory-logical]

## <a name="communication-patterns"></a><span data-ttu-id="1b50a-122">Vzory komunikace</span><span class="sxs-lookup"><span data-stu-id="1b50a-122">Communication patterns</span></span>

<span data-ttu-id="1b50a-123">řešení Hello používá hello [OPC UA Pub nebo Sub specifikace](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) toosend OPC UA telemetrická data tooIoT rozbočovače ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="1b50a-123">hello solution uses hello [OPC UA Pub/Sub specification](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) toosend OPC UA telemetry data tooIoT Hub in JSON format.</span></span> <span data-ttu-id="1b50a-124">řešení Hello používá hello [OPC vydavatele](https://github.com/Azure/iot-edge-opc-publisher) IoT Edge modul pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="1b50a-124">hello solution uses hello [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) IoT Edge module for this purpose.</span></span>

<span data-ttu-id="1b50a-125">řešení Hello má také klientem OPC UA integrována do webové aplikace, které může vytvořit připojení s místními OPC UA servery.</span><span class="sxs-lookup"><span data-stu-id="1b50a-125">hello solution also has an OPC UA client integrated into a web application that can establish connections with on-premises OPC UA servers.</span></span> <span data-ttu-id="1b50a-126">Klient Hello používá [reverznímu proxy serveru](https://wikipedia.org/wiki/Reverse_proxy) a přijímá nápovědy ze služby IoT Hub toomake hello připojení bez nutnosti otevřít porty v bráně firewall místní hello.</span><span class="sxs-lookup"><span data-stu-id="1b50a-126">hello client uses a [reverse-proxy](https://wikipedia.org/wiki/Reverse_proxy) and receives help from IoT Hub toomake hello connection without requiring open ports in hello on-premises firewall.</span></span> <span data-ttu-id="1b50a-127">Tento vzor komunikace se nazývá [komunikace s asistencí služby](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/).</span><span class="sxs-lookup"><span data-stu-id="1b50a-127">This communication pattern is called [service-assisted communication](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/).</span></span> <span data-ttu-id="1b50a-128">řešení Hello používá hello [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT Edge modul pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="1b50a-128">hello solution uses hello [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT Edge module for this purpose.</span></span>


## <a name="simulation"></a><span data-ttu-id="1b50a-129">Simulace</span><span class="sxs-lookup"><span data-stu-id="1b50a-129">Simulation</span></span>

<span data-ttu-id="1b50a-130">Hello simulované stanic a provádění hello simulated výroby, které systémy (MES) tvoří tovární.</span><span class="sxs-lookup"><span data-stu-id="1b50a-130">hello simulated stations and hello simulated manufacturing execution systems (MES) make up a factory production line.</span></span> <span data-ttu-id="1b50a-131">Hello simulované zařízení a hello OPC vydavatele modulu jsou založené na hello [OPC UA .NET Standard] [ lnk-OPC-UA-NET-Standard] publikováno hello OPC Foundation.</span><span class="sxs-lookup"><span data-stu-id="1b50a-131">hello simulated devices and hello OPC Publisher Module are based on hello [OPC UA .NET Standard][lnk-OPC-UA-NET-Standard] published by hello OPC Foundation.</span></span>

<span data-ttu-id="1b50a-132">Hello OPC Proxy a OPC vydavatele jsou implementované jako moduly založené na [Azure IoT Edge][lnk-Azure-IoT-Gateway].</span><span class="sxs-lookup"><span data-stu-id="1b50a-132">hello OPC Proxy and OPC Publisher are implemented as modules based on [Azure IoT Edge][lnk-Azure-IoT-Gateway].</span></span> <span data-ttu-id="1b50a-133">Každá simulovaná výrobní linka má připojenu vyhrazenou bránu.</span><span class="sxs-lookup"><span data-stu-id="1b50a-133">Each simulated production line has a designated gateway attached.</span></span>

<span data-ttu-id="1b50a-134">Všechny simulované komponenty jsou spuštěné v kontejnerech Dockeru hostovaných na virtuálních počítačích Azure s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="1b50a-134">All simulation components run in Docker containers  hosted in an Azure Linux VM.</span></span> <span data-ttu-id="1b50a-135">simulace Hello je nakonfigurované toorun osm simulované produkční řádků ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="1b50a-135">hello simulation is configured toorun eight simulated production lines by default.</span></span>

## <a name="simulated-production-line"></a><span data-ttu-id="1b50a-136">Simulovaná výrobní linka</span><span class="sxs-lookup"><span data-stu-id="1b50a-136">Simulated production line</span></span>

<span data-ttu-id="1b50a-137">Výrobní linka vyrábí součásti.</span><span class="sxs-lookup"><span data-stu-id="1b50a-137">A production line manufactures parts.</span></span> <span data-ttu-id="1b50a-138">Skládá se z různých stanic: montážní stanice, testovací stanice a balicí stanice.</span><span class="sxs-lookup"><span data-stu-id="1b50a-138">It is composed of different stations: an assembly station, a test station, and a packaging station.</span></span>

<span data-ttu-id="1b50a-139">simulace Hello spustí a aktualizuje hello dat, který je zveřejněný prostřednictvím hello OPC UA uzlů.</span><span class="sxs-lookup"><span data-stu-id="1b50a-139">hello simulation runs and updates hello data that is exposed through hello OPC UA nodes.</span></span> <span data-ttu-id="1b50a-140">Všechny stanice produkční Simulovaná jsou řízená hello MES prostřednictvím OPC UA.</span><span class="sxs-lookup"><span data-stu-id="1b50a-140">All simulated production line stations are orchestrated by hello MES through OPC UA.</span></span>

## <a name="simulated-manufacturing-execution-system"></a><span data-ttu-id="1b50a-141">Simulovaný systém řízení výroby</span><span class="sxs-lookup"><span data-stu-id="1b50a-141">Simulated manufacturing execution system</span></span>

<span data-ttu-id="1b50a-142">Hello MES monitoruje každé stanici v řádku produkční hello prostřednictvím změny stavu stanice toodetect OPC UA.</span><span class="sxs-lookup"><span data-stu-id="1b50a-142">hello MES monitors each station in hello production line through OPC UA toodetect station status changes.</span></span> <span data-ttu-id="1b50a-143">Volá metody toocontrol hello stanice OPC UA a předává produktu z jedné stanici toohello vedle jeho dokončení.</span><span class="sxs-lookup"><span data-stu-id="1b50a-143">It calls OPC UA methods toocontrol hello stations and passes a product from one station toohello next until it is complete.</span></span>

## <a name="gateway-opc-publisher-module"></a><span data-ttu-id="1b50a-144">Modul vydavatele brány OPC</span><span class="sxs-lookup"><span data-stu-id="1b50a-144">Gateway OPC publisher module</span></span>

<span data-ttu-id="1b50a-145">OPC vydavatele modul připojí toohello stanice OPC UA servery a přihlásí se k odběru toohello OPC uzly toobe publikovaná.</span><span class="sxs-lookup"><span data-stu-id="1b50a-145">OPC Publisher Module connects toohello station OPC UA servers and subscribes toohello OPC nodes toobe published.</span></span> <span data-ttu-id="1b50a-146">modul Hello převede data uzlu hello do formátu JSON, zašifruje a odešle ji jako zprávy OPC UA Pub nebo Sub tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="1b50a-146">hello module converts hello node data into JSON format, encrypts it, and sends it tooIoT Hub as OPC UA Pub/Sub messages.</span></span>

<span data-ttu-id="1b50a-147">modul OPC vydavatele Hello pouze vyžaduje k portu odchozí https (443) a mohou pracovat s stávající infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="1b50a-147">hello OPC Publisher module only requires an outbound https port (443) and can work with existing enterprise infrastructure.</span></span>

## <a name="gateway-opc-proxy-module"></a><span data-ttu-id="1b50a-148">Modul proxy serveru brány OPC</span><span class="sxs-lookup"><span data-stu-id="1b50a-148">Gateway OPC proxy module</span></span>

<span data-ttu-id="1b50a-149">Hello brány OPC UA Proxy modulu tunelových propojení binární OPC UA příkazy a ovládání zpráv a vyžaduje pouze k portu odchozí https (443).</span><span class="sxs-lookup"><span data-stu-id="1b50a-149">hello Gateway OPC UA Proxy Module tunnels binary OPC UA command and control messages and only requires an outbound https port (443).</span></span> <span data-ttu-id="1b50a-150">Může fungovat s existující podnikovou infrastrukturou, včetně webových proxy serverů.</span><span class="sxs-lookup"><span data-stu-id="1b50a-150">It can work with existing enterprise infrastructure, including Web Proxies.</span></span>

<span data-ttu-id="1b50a-151">Používá zařízení IoT Hub metody tootransfer packetized TCP/IP data na aplikační vrstvu hello a tím zajišťuje důvěryhodnosti koncový bod, šifrování dat a integritu pomocí protokolu SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="1b50a-151">It uses IoT Hub Device methods tootransfer packetized TCP/IP data at hello application layer and thus ensures endpoint trust, data encryption, and integrity using SSL/TLS.</span></span>

<span data-ttu-id="1b50a-152">Hello OPC UA binární protokol přes předávací službu přes proxy server hello samotné používá uživatelský Agent ověřování a šifrování.</span><span class="sxs-lookup"><span data-stu-id="1b50a-152">hello OPC UA binary protocol relayed through hello proxy itself uses UA authentication and encryption.</span></span>

## <a name="azure-time-series-insights"></a><span data-ttu-id="1b50a-153">Azure Time Series Insights</span><span class="sxs-lookup"><span data-stu-id="1b50a-153">Azure Time Series Insights</span></span>

<span data-ttu-id="1b50a-154">Hello brány OPC vydavatele modulu odběratel tooOPC uživatelský Agent serveru uzly toodetect změnu v hodnotě hello datových hodnot.</span><span class="sxs-lookup"><span data-stu-id="1b50a-154">hello Gateway OPC Publisher Module subscribes tooOPC UA server nodes toodetect change in hello data values.</span></span> <span data-ttu-id="1b50a-155">Pokud je zjištěna změna dat v jednom z uzlů hello, tento modul pak odešle zprávy tooAzure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1b50a-155">If a data change is detected in one of hello nodes, this module then sends messages tooAzure IoT Hub.</span></span>

<span data-ttu-id="1b50a-156">Služba IoT Hub zajišťuje tooAzure zdroje událostí TSI.</span><span class="sxs-lookup"><span data-stu-id="1b50a-156">IoT Hub provides an event source tooAzure TSI.</span></span> <span data-ttu-id="1b50a-157">TSI ukládá data po dobu 30 dnů podle časová razítka připojených toohello zprávy.</span><span class="sxs-lookup"><span data-stu-id="1b50a-157">TSI stores data for 30 days based on timestamps attached toohello messages.</span></span> <span data-ttu-id="1b50a-158">Mezi tato data patří:</span><span class="sxs-lookup"><span data-stu-id="1b50a-158">This data includes:</span></span>

* <span data-ttu-id="1b50a-159">Identifikátor OPC UA ApplicationUri</span><span class="sxs-lookup"><span data-stu-id="1b50a-159">OPC UA ApplicationUri</span></span>
* <span data-ttu-id="1b50a-160">Identifikátor OPC UA NodeId</span><span class="sxs-lookup"><span data-stu-id="1b50a-160">OPC UA NodeId</span></span>
* <span data-ttu-id="1b50a-161">Hodnota uzlu hello</span><span class="sxs-lookup"><span data-stu-id="1b50a-161">Value of hello node</span></span>
* <span data-ttu-id="1b50a-162">Časové razítko zdroje</span><span class="sxs-lookup"><span data-stu-id="1b50a-162">Source timestamp</span></span>
* <span data-ttu-id="1b50a-163">Název OPC UA DisplayName</span><span class="sxs-lookup"><span data-stu-id="1b50a-163">OPC UA DisplayName</span></span>

<span data-ttu-id="1b50a-164">V současné době TSI neumožňuje zákazníci toocustomize jak dlouho si přejí tookeep hello data.</span><span class="sxs-lookup"><span data-stu-id="1b50a-164">Currently, TSI does not allow customers toocustomize how long they wish tookeep hello data for.</span></span>

<span data-ttu-id="1b50a-165">Dotazy služby TSI na data uzlů se provádí pomocí SearchSpan (Time.From a Time.To) a agregace podle identifikátoru OPC UA ApplicationUri nebo OPC UA NodeId nebo názvu OPC UA DisplayName.</span><span class="sxs-lookup"><span data-stu-id="1b50a-165">TSI queries against node data using a SearchSpan (Time.From, Time.To) and aggregates by OPC UA ApplicationUri or OPC UA NodeId or OPC UA DisplayName.</span></span>

<span data-ttu-id="1b50a-166">agregovaná data hello tooretrieve pro měřidla hello OEE a klíčových ukazatelů výkonu a grafy řady hello, data podle počtu událostí, Sum, Avg, Min a Max.</span><span class="sxs-lookup"><span data-stu-id="1b50a-166">tooretrieve hello data for hello OEE and KPI gauges, and hello time series charts, data is aggregated by count of events, Sum, Avg, Min, and Max.</span></span>

<span data-ttu-id="1b50a-167">Hello časové řady jsou vytvořeny pomocí jiným procesem.</span><span class="sxs-lookup"><span data-stu-id="1b50a-167">hello time series are built using a different process.</span></span> <span data-ttu-id="1b50a-168">OEE a klíčových ukazatelů výkonu se počítá z stanice základní data a probublala do topologie hello (řádků výroby, objekty Factory, enterprise) v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="1b50a-168">OEE and KPIs are calculated from station base data and bubbled up for hello topology (production lines, factories, enterprise) in hello application.</span></span>

<span data-ttu-id="1b50a-169">Kromě toho časové řady pro topologii s OEE a klíčového ukazatele výkonu se počítá v aplikaci hello vždy, když je připraven na zobrazené časové rozpětí.</span><span class="sxs-lookup"><span data-stu-id="1b50a-169">Additionally, time series for OEE and KPI topology is calculated in hello app, whenever a displayed timespan is ready.</span></span> <span data-ttu-id="1b50a-170">Hello den zobrazení je třeba aktualizovat každou hodinu úplné.</span><span class="sxs-lookup"><span data-stu-id="1b50a-170">For example, hello day view is updated every full hour.</span></span>

<span data-ttu-id="1b50a-171">Hello časové řady zobrazení data uzlu pochází přímo z TSI pomocí agregace pro časový interval.</span><span class="sxs-lookup"><span data-stu-id="1b50a-171">hello time series view of node data comes directly from TSI using an aggregation for timespan.</span></span>

## <a name="iot-hub"></a><span data-ttu-id="1b50a-172">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="1b50a-172">IoT Hub</span></span>
<span data-ttu-id="1b50a-173">Hello [služby IoT hub] [ lnk-IoT Hub] přijímá data odeslaná z hello OPC vydavatele modulu do cloudu hello a je k dispozici toohello Azure TSI služby.</span><span class="sxs-lookup"><span data-stu-id="1b50a-173">hello [IoT hub][lnk-IoT Hub] receives data sent from hello OPC Publisher Module into hello cloud and makes it available toohello Azure TSI service.</span></span> 

<span data-ttu-id="1b50a-174">Hello IoT Hub v řešení hello také:</span><span class="sxs-lookup"><span data-stu-id="1b50a-174">hello IoT Hub in hello solution also:</span></span>
- <span data-ttu-id="1b50a-175">Udržuje registr identity, který ukládá hello ID pro všechny OPC vydavatele modulu a všechny moduly OPC Proxy.</span><span class="sxs-lookup"><span data-stu-id="1b50a-175">Maintains an identity registry that stores hello IDs for all OPC Publisher Module and all OPC Proxy Modules.</span></span>
- <span data-ttu-id="1b50a-176">Slouží jako kanál přenosu pro obousměrnou komunikaci hello OPC modul proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="1b50a-176">Is used as transport channel for bidirectional communication of hello OPC Proxy Module.</span></span>

## <a name="azure-storage"></a><span data-ttu-id="1b50a-177">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="1b50a-177">Azure Storage</span></span>
<span data-ttu-id="1b50a-178">řešení Hello používá úložiště objektů blob v Azure jako úložiště disku pro data nasazení virtuálních počítačů a toostore hello.</span><span class="sxs-lookup"><span data-stu-id="1b50a-178">hello solution uses Azure blob storage as disk storage for hello VM and toostore deployment data.</span></span>

## <a name="web-app"></a><span data-ttu-id="1b50a-179">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="1b50a-179">Web app</span></span>
<span data-ttu-id="1b50a-180">webové aplikace Hello nasadit jako součást hello předkonfigurované řešení se skládá ze klientem integrované OPC UA, výstrahy zpracování a vizualizace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1b50a-180">hello web app deployed as part of hello preconfigured solution comprises of an integrated OPC UA client, alerts processing and telemetry visualization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b50a-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1b50a-181">Next steps</span></span>

<span data-ttu-id="1b50a-182">Budete-li pokračovat, Začínáme se službou IoT Suite načtením hello následující články:</span><span class="sxs-lookup"><span data-stu-id="1b50a-182">You can continue getting started with IoT Suite by reading hello following articles:</span></span>

* <span data-ttu-id="1b50a-183">[Oprávnění na webu azureiotsuite.com hello][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="1b50a-183">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>
* [<span data-ttu-id="1b50a-184">Nasazení brány v systému Windows nebo Linux hello připojen objekt pro vytváření předkonfigurovaného řešení</span><span class="sxs-lookup"><span data-stu-id="1b50a-184">Deploy a gateway on Windows or Linux for hello connected factory preconfigured solution</span></span>](iot-suite-connected-factory-gateway-deployment.md)

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-suite-permissions.md
