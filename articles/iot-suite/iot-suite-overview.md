---
title: "Přehled Azure IoT Suite aaaMicrosoft | Microsoft Docs"
description: "Přehled o tom, jak Azure IoT Suite nabízí internet věcí toocollect předkonfigurovaných řešení, analyzovat a ukládání dat, vizualizacím a integraci s jinými systémy."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2d38d08a-4133-4e5c-8b28-f93cadb5df05
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 385025c5ec0d37c74689a928bc09e85b33439634
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-iot-suite"></a><span data-ttu-id="39020-103">Přehled sady Azure IoT Suite</span><span class="sxs-lookup"><span data-stu-id="39020-103">Overview of Azure IoT Suite</span></span>

<span data-ttu-id="39020-104">Hello Azure pro internet věcí (IoT) services nabízí celou řadu funkcí.</span><span class="sxs-lookup"><span data-stu-id="39020-104">hello Azure internet of things (IoT) services offer a broad range of capabilities.</span></span> <span data-ttu-id="39020-105">Tyto služby na úrovni řešení pro velké firmy umožňují:</span><span class="sxs-lookup"><span data-stu-id="39020-105">These enterprise grade services enable you to:</span></span>

* <span data-ttu-id="39020-106">sběr dat ze zařízení</span><span class="sxs-lookup"><span data-stu-id="39020-106">Collect data from devices</span></span>
* <span data-ttu-id="39020-107">analýzu datových proudů za provozu</span><span class="sxs-lookup"><span data-stu-id="39020-107">Analyze data streams in-motion</span></span>
* <span data-ttu-id="39020-108">ukládání objemných datových sad a dotazy na ně</span><span class="sxs-lookup"><span data-stu-id="39020-108">Store and query large data sets</span></span>
* <span data-ttu-id="39020-109">vizualizaci historických dat i dat v reálném čase</span><span class="sxs-lookup"><span data-stu-id="39020-109">Visualize both real-time and historical data</span></span>
* <span data-ttu-id="39020-110">integraci se systémy administrativní podpory (back-office)</span><span class="sxs-lookup"><span data-stu-id="39020-110">Integrate with back-office systems</span></span>
* <span data-ttu-id="39020-111">správu zařízení</span><span class="sxs-lookup"><span data-stu-id="39020-111">Manage your devices</span></span>

<span data-ttu-id="39020-112">toodeliver tyto funkce Azure IoT Suite balíčky současně více služeb Azure s vlastními rozšířeními v podobě *předkonfigurovaných řešení*.</span><span class="sxs-lookup"><span data-stu-id="39020-112">toodeliver these capabilities, Azure IoT Suite packages together multiple Azure services with custom extensions as *preconfigured solutions*.</span></span> <span data-ttu-id="39020-113">Tato předkonfigurovaná řešení jsou základní implementací běžných vzorů řešení IoT, které pomáhají tooreduce hello čas trvat toodeliver řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="39020-113">These preconfigured solutions are base implementations of common IoT solution patterns that help tooreduce hello time you take toodeliver your IoT solutions.</span></span> <span data-ttu-id="39020-114">Pomocí hello [IoT software development Kit][lnk-sdks], můžete přizpůsobit a rozšířit těchto řešení toomeet vlastní požadavky.</span><span class="sxs-lookup"><span data-stu-id="39020-114">Using hello [IoT software development kits][lnk-sdks], you can customize and extend these solutions toomeet your own requirements.</span></span> <span data-ttu-id="39020-115">Tato řešení můžete využít i jako příklady či šablony při vývoji nových řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="39020-115">You can also use these solutions as examples or templates when you are developing new IoT solutions.</span></span>

<span data-ttu-id="39020-116">Hello následující video obsahuje úvod tooAzure IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="39020-116">hello following video provides an introduction tooAzure IoT Suite:</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON309/player]
> 
> 

## <a name="azure-iot-services-in-azure-iot-suite"></a><span data-ttu-id="39020-117">Služby Azure IoT v Azure IoT Suite</span><span class="sxs-lookup"><span data-stu-id="39020-117">Azure IoT services in Azure IoT Suite</span></span>
<span data-ttu-id="39020-118">Hello předkonfigurované řešení obvykle využívají hello následující služby:</span><span class="sxs-lookup"><span data-stu-id="39020-118">hello preconfigured solutions typically use hello following services:</span></span>

* <span data-ttu-id="39020-119">Základní tooAzure IoT Suite je hello [Azure IoT Hub] [ lnk-iot-hub] služby.</span><span class="sxs-lookup"><span data-stu-id="39020-119">Core tooAzure IoT Suite is hello [Azure IoT Hub][lnk-iot-hub] service.</span></span> <span data-ttu-id="39020-120">Tato služba poskytuje hello zařízení cloud a funkce pro zasílání zpráv typu cloud zařízení a jednání jako hello brány toohello cloudu a hello jiných klíčovým službám IoT Suite.</span><span class="sxs-lookup"><span data-stu-id="39020-120">This service provides hello device-to-cloud and cloud-to-device messaging capabilities and acts as hello gateway toohello cloud and hello other key IoT Suite services.</span></span> <span data-ttu-id="39020-121">Hello služba vám umožní tooreceive zpráv ze zařízení v měřítku a odesílat příkazy tooyour zařízení.</span><span class="sxs-lookup"><span data-stu-id="39020-121">hello service enables you tooreceive messages from your devices at scale, and send commands tooyour devices.</span></span> <span data-ttu-id="39020-122">Hello služba vám také umožní příliš[spravovat vaše zařízení][lnk-device-management].</span><span class="sxs-lookup"><span data-stu-id="39020-122">hello service also enables you too[manage your devices][lnk-device-management].</span></span> <span data-ttu-id="39020-123">Můžete například nakonfigurovat, restartovat nebo obnovit tovární nastavení na jeden nebo více zařízení připojených toohello rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="39020-123">For example, you can configure, reboot, or perform a factory reset on one or more devices connected toohello hub.</span></span>
* <span data-ttu-id="39020-124">Služba [Azure Stream Analytics][lnk-asa] umožňuje analyzovat data za provozu.</span><span class="sxs-lookup"><span data-stu-id="39020-124">[Azure Stream Analytics][lnk-asa] provides in-motion data analysis.</span></span> <span data-ttu-id="39020-125">IoT Suite využívá tuto službu tooprocess příchozí telemetrii, vytváření agregací a zjišťování událostí.</span><span class="sxs-lookup"><span data-stu-id="39020-125">IoT Suite uses this service tooprocess incoming telemetry, perform aggregation, and detect events.</span></span> <span data-ttu-id="39020-126">Hello předkonfigurovaná řešení dále s využitím stream analytics tooprocess informační zprávy, které obsahují data, jako je například metadata nebo odezvy ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="39020-126">hello preconfigured solutions also use stream analytics tooprocess informational messages that contain data such as metadata or command responses from devices.</span></span> <span data-ttu-id="39020-127">Hello řešení pomocí služby Stream Analytics tooprocess hello zpráv ze zařízení a přenosu těchto zpráv tooother služby.</span><span class="sxs-lookup"><span data-stu-id="39020-127">hello solutions use Stream Analytics tooprocess hello messages from your devices and deliver those messages tooother services.</span></span>
* <span data-ttu-id="39020-128">[Úložiště Azure] [ lnk-azure-storage] a [Azure Cosmos DB] [ lnk-document-db] poskytují možnosti úložiště dat hello.</span><span class="sxs-lookup"><span data-stu-id="39020-128">[Azure Storage][lnk-azure-storage] and [Azure Cosmos DB][lnk-document-db] provide hello data storage capabilities.</span></span> <span data-ttu-id="39020-129">Hello předkonfigurovaná řešení využívají telemetrie toostore úložiště objektů blob a toomake je k dispozici pro analýzu.</span><span class="sxs-lookup"><span data-stu-id="39020-129">hello preconfigured solutions use blob storage toostore telemetry and toomake it available for analysis.</span></span> <span data-ttu-id="39020-130">Hello řešení pomocí metadat zařízení toostore Cosmos DB a povolit možnosti správy zařízení hello hello řešení.</span><span class="sxs-lookup"><span data-stu-id="39020-130">hello solutions use Cosmos DB toostore device metadata and enable hello device management capabilities of hello solutions.</span></span>
* <span data-ttu-id="39020-131">[Azure Web Apps] [ lnk-web-apps] a [Microsoft Power BI] [ lnk-power-bi] poskytovat funkce pro vizualizaci dat hello.</span><span class="sxs-lookup"><span data-stu-id="39020-131">[Azure Web Apps][lnk-web-apps] and [Microsoft Power BI][lnk-power-bi] provide hello data visualization capabilities.</span></span> <span data-ttu-id="39020-132">Hello flexibilitu Power BI vám umožní tooquickly sestavení vlastní interaktivní řídicí panely, které používají data IoT Suite.</span><span class="sxs-lookup"><span data-stu-id="39020-132">hello flexibility of Power BI enables you tooquickly build your own interactive dashboards that use IoT Suite data.</span></span>

<span data-ttu-id="39020-133">Přehled hello architektura typického řešení IoT najdete v tématu [Microsoft Azure a Internet věcí (IoT) hello][iot-suite-what-is-azure-iot].</span><span class="sxs-lookup"><span data-stu-id="39020-133">For an overview of hello architecture of a typical IoT solution, see [Microsoft Azure and hello Internet of Things (IoT)][iot-suite-what-is-azure-iot].</span></span>

## <a name="preconfigured-solutions"></a><span data-ttu-id="39020-134">Předkonfigurovaná řešení</span><span class="sxs-lookup"><span data-stu-id="39020-134">Preconfigured solutions</span></span>

<span data-ttu-id="39020-135">Sada IoT Suite zahrnuje předkonfigurovaná řešení tohoto povolení tooquickly můžete začít pracovat s a tooexplore hello běžným scénářům IoT, jako například:</span><span class="sxs-lookup"><span data-stu-id="39020-135">IoT Suite includes preconfigured solutions that enable you tooquickly get started with and tooexplore hello common IoT scenarios, such as:</span></span>

* <span data-ttu-id="39020-136">Vzdálené monitorování</span><span class="sxs-lookup"><span data-stu-id="39020-136">Remote monitoring</span></span>
* <span data-ttu-id="39020-137">Prediktivní údržba</span><span class="sxs-lookup"><span data-stu-id="39020-137">Predictive maintenance</span></span>
* <span data-ttu-id="39020-138">Propojená továrna</span><span class="sxs-lookup"><span data-stu-id="39020-138">Connected factory</span></span>

<span data-ttu-id="39020-139">Můžete nasadit tyto řešení tooyour předplatného Azure a pak spusťte scénář IoT dokončení, začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="39020-139">You can deploy these solutions tooyour Azure subscription and then run a complete, end-to-end IoT scenario.</span></span>

## <a name="next-steps"></a><span data-ttu-id="39020-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="39020-140">Next steps</span></span>

<span data-ttu-id="39020-141">Teď, když máte přehled co IoT Suite dělat a jaké jsou jeho hlavní součásti, můžete další informace o hello předkonfigurované řešení IoT Suite.</span><span class="sxs-lookup"><span data-stu-id="39020-141">Now that you have an overview of what IoT Suite can do and what are its main components, you can learn more about hello preconfigured solutions in IoT Suite.</span></span> <span data-ttu-id="39020-142">Další informace najdete v tématu [co jsou hello Azure IoT předkonfigurovaných řešení?][lnk-what-are-preconfig]</span><span class="sxs-lookup"><span data-stu-id="39020-142">For more information, see [What are hello Azure IoT preconfigured solutions?][lnk-what-are-preconfig]</span></span>

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
[lnk-device-management]: ../iot-hub/iot-hub-device-management-overview.md
