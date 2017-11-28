---
title: aaaMonitor aplikace v Azure App Service | Microsoft Docs
description: "Zjistěte, jak hello toomonitor aplikace v Azure App Service pomocí portálu Azure."
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: d273da4e-07de-48e0-b99d-4020d84a425e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: byvinyal
ms.openlocfilehash: 80d5a466102a894a49d04ae35aa54cc1d05a58df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-monitor-apps-in-azure-app-service"></a><span data-ttu-id="b849f-103">Postupy: monitorování aplikací v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b849f-103">How to: Monitor Apps in Azure App Service</span></span>
<span data-ttu-id="b849f-104">[Služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) poskytuje integrované monitorování funkce v hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b849f-104">[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) provides built in monitoring functionality in hello [Azure Portal](https://portal.azure.com).</span></span>
<span data-ttu-id="b849f-105">To zahrnuje možnost tooreview hello **kvóty** a **metriky** pro aplikaci, jakož i hello plán služby App Service, nastavení **výstrahy** a i **škálování** automaticky v závislosti na tyto metriky.</span><span class="sxs-lookup"><span data-stu-id="b849f-105">This includes hello ability tooreview **quotas** and **metrics** for an app as well as hello App Service plan, setting up **alerts** and even **scaling** automatically based on these metrics.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a><span data-ttu-id="b849f-106">Principy kvóty a metriky</span><span class="sxs-lookup"><span data-stu-id="b849f-106">Understanding Quotas and Metrics</span></span>
### <a name="quotas"></a><span data-ttu-id="b849f-107">Kvóty</span><span class="sxs-lookup"><span data-stu-id="b849f-107">Quotas</span></span>
<span data-ttu-id="b849f-108">Aplikace hostované ve službě App Service jsou subjektu toocertain *omezení* s prostředky, které mohou používat.</span><span class="sxs-lookup"><span data-stu-id="b849f-108">Applications hosted in App Service are subject toocertain *limits* on the resources they can use.</span></span> <span data-ttu-id="b849f-109">Hello omezení jsou definovány hello **plán služby App Service** spojená s aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="b849f-109">hello limits are defined by hello **App Service plan** associated with hello app.</span></span>

<span data-ttu-id="b849f-110">Pokud aplikace hello je hostována v **volné** nebo **sdílené** plánu, pak jsou definovány hello omezení pro aplikace hello může používat prostředky hello **kvóty**.</span><span class="sxs-lookup"><span data-stu-id="b849f-110">If hello application is hosted in a **Free** or **Shared** plan, then hello limits on hello resources hello app can use are defined by **Quotas**.</span></span>

<span data-ttu-id="b849f-111">Pokud aplikace hello je hostována v **základní**, **standardní** nebo **Premium** plánu, pak hello omezení mohou používat prostředky hello se nastavují hello **velikost** (Malé, střední, velký) a **instance počet** (1, 2, 3,...) z hello **plán služby App Service**.</span><span class="sxs-lookup"><span data-stu-id="b849f-111">If hello application is hosted in a **Basic**, **Standard** or **Premium** plan, then hello limits on hello resources they can use are set by hello **size** (Small, Medium, Large) and **instance count** (1, 2, 3, ...) of hello **App Service plan**.</span></span>

<span data-ttu-id="b849f-112">**Kvóty** pro **volné** nebo **sdílené** aplikace jsou:</span><span class="sxs-lookup"><span data-stu-id="b849f-112">**Quotas** for **Free** or **Shared** apps are:</span></span>

* <span data-ttu-id="b849f-113">**CPU(Short)**</span><span class="sxs-lookup"><span data-stu-id="b849f-113">**CPU(Short)**</span></span>
  * <span data-ttu-id="b849f-114">Nároky na výkon procesoru, které jsou povoleny pro tuto aplikaci v intervalu 5 minut.</span><span class="sxs-lookup"><span data-stu-id="b849f-114">Amount of CPU allowed for this application in a 5-minute interval.</span></span> <span data-ttu-id="b849f-115">Tato kvóta znovu nastaví každých 5 minut.</span><span class="sxs-lookup"><span data-stu-id="b849f-115">This quota re-sets every 5 minutes.</span></span>
* <span data-ttu-id="b849f-116">**CPU(Day)**</span><span class="sxs-lookup"><span data-stu-id="b849f-116">**CPU(Day)**</span></span>
  * <span data-ttu-id="b849f-117">Celkový počet nároky na výkon procesoru, které jsou povoleny pro tuto aplikaci za den.</span><span class="sxs-lookup"><span data-stu-id="b849f-117">Total amount of CPU allowed for this application in a day.</span></span> <span data-ttu-id="b849f-118">Tato kvóta znovu nastaví každých 24 hodin půlnoci času UTC.</span><span class="sxs-lookup"><span data-stu-id="b849f-118">This quota re-sets every 24 hours at midnight UTC.</span></span>
* <span data-ttu-id="b849f-119">**Paměť**</span><span class="sxs-lookup"><span data-stu-id="b849f-119">**Memory**</span></span>
  * <span data-ttu-id="b849f-120">Celková velikost paměti pro tuto aplikaci povolena.</span><span class="sxs-lookup"><span data-stu-id="b849f-120">Total amount of memory allowed for this application.</span></span>
* <span data-ttu-id="b849f-121">**Šířka pásma**</span><span class="sxs-lookup"><span data-stu-id="b849f-121">**Bandwidth**</span></span>
  * <span data-ttu-id="b849f-122">Celková velikost odchozí šířky pásma povolená pro tuto aplikaci za den.</span><span class="sxs-lookup"><span data-stu-id="b849f-122">Total amount of outgoing bandwidth allowed for this application in a day.</span></span>
    <span data-ttu-id="b849f-123">Tato kvóta znovu nastaví každých 24 hodin půlnoci času UTC.</span><span class="sxs-lookup"><span data-stu-id="b849f-123">This quota re-sets every 24 hours at midnight UTC.</span></span>
* <span data-ttu-id="b849f-124">**Systém souborů**</span><span class="sxs-lookup"><span data-stu-id="b849f-124">**Filesystem**</span></span>
  * <span data-ttu-id="b849f-125">Celková velikost úložiště, které jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="b849f-125">Total amount of storage allowed.</span></span>

<span data-ttu-id="b849f-126">Hello jenom kvóta použít tooapps hostované na **základní**, **standardní** a **Premium** plány je **Filesystem**.</span><span class="sxs-lookup"><span data-stu-id="b849f-126">hello only quota applicable tooapps hosted on **Basic**, **Standard** and **Premium** plans is **Filesystem**.</span></span>

<span data-ttu-id="b849f-127">Další informace o hello konkrétní kvót, omezení a funkce, které jsou k dispozici pro různé identifikátory SKU služby aplikace hello naleznete zde: [omezení předplatné služby Azure](../azure-subscription-service-limits.md#app-service-limits)</span><span class="sxs-lookup"><span data-stu-id="b849f-127">More information about hello specific quotas, limits and features available to hello different App Service SKUs can be found here: [Azure Subscription Service Limits](../azure-subscription-service-limits.md#app-service-limits)</span></span>

#### <a name="quota-enforcement"></a><span data-ttu-id="b849f-128">Vynucení kvót</span><span class="sxs-lookup"><span data-stu-id="b849f-128">Quota Enforcement</span></span>
<span data-ttu-id="b849f-129">Pokud aplikace v jeho využití překročí hello **procesoru (krátký)**, **procesoru (den)**, nebo **šířky pásma** kvóty pak hello aplikace bude zastaven, dokud nebude znovu nastaví kvóty hello.</span><span class="sxs-lookup"><span data-stu-id="b849f-129">If an application in its usage exceeds hello **CPU (short)**, **CPU (Day)**, or **bandwidth** quota then hello application will be stopped until hello quota re-sets.</span></span> <span data-ttu-id="b849f-130">Během této doby bude výsledkem všechny příchozí požadavky **HTTP 403**.</span><span class="sxs-lookup"><span data-stu-id="b849f-130">During this time, all incoming requests will result in an **HTTP 403**.</span></span>
![][http403]

<span data-ttu-id="b849f-131">Pokud aplikace hello **paměti** dojde k překročení kvóty a pak aplikace hello se nebude znovu spuštěna.</span><span class="sxs-lookup"><span data-stu-id="b849f-131">If hello application **memory** quota is exceeded, then hello application will be re-started.</span></span>

<span data-ttu-id="b849f-132">Pokud hello **Filesystem** dojde k překročení kvóty, pak všechny zápisu se nezdaří, včetně zápis toologs.</span><span class="sxs-lookup"><span data-stu-id="b849f-132">If hello **Filesystem** quota is exceeded, then any write operation will fail, this includes writing toologs.</span></span>

<span data-ttu-id="b849f-133">Kvóty můžete zvýšit nebo odebrat z vaší aplikace, že si upgradujete plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="b849f-133">Quotas can be increased or removed from your app by upgrading your App Service plan.</span></span>

### <a name="metrics"></a><span data-ttu-id="b849f-134">Metriky</span><span class="sxs-lookup"><span data-stu-id="b849f-134">Metrics</span></span>
<span data-ttu-id="b849f-135">**Metriky** poskytují informace o aplikaci hello nebo chování plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="b849f-135">**Metrics** provide information about hello app, or App Service plan's behavior.</span></span>

<span data-ttu-id="b849f-136">Pro **aplikace**, jsou dostupné metriky hello:</span><span class="sxs-lookup"><span data-stu-id="b849f-136">For an **Application**, hello available metrics are:</span></span>

* <span data-ttu-id="b849f-137">**Průměrná doba odezvy**</span><span class="sxs-lookup"><span data-stu-id="b849f-137">**Average Response Time**</span></span>
  * <span data-ttu-id="b849f-138">Hello Průměrná doba hello aplikace tooserve požadavky v ms.</span><span class="sxs-lookup"><span data-stu-id="b849f-138">hello average time taken for hello app tooserve requests in ms.</span></span>
* <span data-ttu-id="b849f-139">**Průměrná paměti pracovní sady**</span><span class="sxs-lookup"><span data-stu-id="b849f-139">**Average memory working set**</span></span>
  * <span data-ttu-id="b849f-140">Hello Průměrná velikost paměti v MIB používané aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b849f-140">hello average amount of memory in MiBs used by hello app.</span></span>
* <span data-ttu-id="b849f-141">**Čas procesoru**</span><span class="sxs-lookup"><span data-stu-id="b849f-141">**CPU Time**</span></span>
  * <span data-ttu-id="b849f-142">Hello nároky na výkon procesoru v sekundách spotřebovávají aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b849f-142">hello amount of CPU in seconds consumed by hello app.</span></span> <span data-ttu-id="b849f-143">Další informace o této metriky najdete: [procento vs procesoru čas procesoru](#cpu-time-vs-cpu-percentage)</span><span class="sxs-lookup"><span data-stu-id="b849f-143">For more information about this metric see: [CPU time vs CPU percentage](#cpu-time-vs-cpu-percentage)</span></span>
* <span data-ttu-id="b849f-144">**Data v**</span><span class="sxs-lookup"><span data-stu-id="b849f-144">**Data In**</span></span>
  * <span data-ttu-id="b849f-145">Hello množství příchozích šířky pásma spotřebovávají aplikace hello v MIB.</span><span class="sxs-lookup"><span data-stu-id="b849f-145">hello amount of incoming bandwidth consumed by hello app in MiBs.</span></span>
* <span data-ttu-id="b849f-146">**Data**</span><span class="sxs-lookup"><span data-stu-id="b849f-146">**Data Out**</span></span>
  * <span data-ttu-id="b849f-147">Hello množství odchozí šířky pásma spotřebovávají aplikace hello v MIB.</span><span class="sxs-lookup"><span data-stu-id="b849f-147">hello amount of outgoing bandwidth consumed by hello app in MiBs.</span></span>
* <span data-ttu-id="b849f-148">**Http 2xx**</span><span class="sxs-lookup"><span data-stu-id="b849f-148">**Http 2xx**</span></span>
  * <span data-ttu-id="b849f-149">Počet požadavků, které jsou výsledkem stavový kód protokolu http > = 200 ale < 300.</span><span class="sxs-lookup"><span data-stu-id="b849f-149">Count of requests resulting in a http status code >= 200 but < 300.</span></span>
* <span data-ttu-id="b849f-150">**Http 3xx**</span><span class="sxs-lookup"><span data-stu-id="b849f-150">**Http 3xx**</span></span>
  * <span data-ttu-id="b849f-151">Počet požadavků, které jsou výsledkem stavový kód protokolu http > = 300 ale < 400.</span><span class="sxs-lookup"><span data-stu-id="b849f-151">Count of requests resulting in a http status code >= 300 but < 400.</span></span>
* <span data-ttu-id="b849f-152">**HTTP 401**</span><span class="sxs-lookup"><span data-stu-id="b849f-152">**Http 401**</span></span>
  * <span data-ttu-id="b849f-153">Počet požadavků, které jsou výsledkem stavový kód HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="b849f-153">Count of requests resulting in HTTP 401 status code.</span></span>
* <span data-ttu-id="b849f-154">**HTTP 403**</span><span class="sxs-lookup"><span data-stu-id="b849f-154">**Http 403**</span></span>
  * <span data-ttu-id="b849f-155">Počet požadavků, které jsou výsledkem stavový kód HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="b849f-155">Count of requests resulting in HTTP 403 status code.</span></span>
* <span data-ttu-id="b849f-156">**HTTP 404**</span><span class="sxs-lookup"><span data-stu-id="b849f-156">**Http 404**</span></span>
  * <span data-ttu-id="b849f-157">Počet požadavků, které jsou výsledkem stavový kód HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="b849f-157">Count of requests resulting in HTTP 404 status code.</span></span>
* <span data-ttu-id="b849f-158">**HTTP 406**</span><span class="sxs-lookup"><span data-stu-id="b849f-158">**Http 406**</span></span>
  * <span data-ttu-id="b849f-159">Počet požadavků, které jsou výsledkem stavový kód HTTP 406.</span><span class="sxs-lookup"><span data-stu-id="b849f-159">Count of requests resulting in HTTP 406 status code.</span></span>
* <span data-ttu-id="b849f-160">**Http 4xx**</span><span class="sxs-lookup"><span data-stu-id="b849f-160">**Http 4xx**</span></span>
  * <span data-ttu-id="b849f-161">Počet požadavků, které jsou výsledkem stavový kód protokolu http > = 400 ale < 500.</span><span class="sxs-lookup"><span data-stu-id="b849f-161">Count of requests resulting in a http status code >= 400 but < 500.</span></span>
* <span data-ttu-id="b849f-162">**Chyby protokolu HTTP serveru**</span><span class="sxs-lookup"><span data-stu-id="b849f-162">**Http Server Errors**</span></span>
  * <span data-ttu-id="b849f-163">Počet požadavků, které jsou výsledkem stavový kód protokolu http > = 500, ale < 600.</span><span class="sxs-lookup"><span data-stu-id="b849f-163">Count of requests resulting in a http status code >= 500 but < 600.</span></span>
* <span data-ttu-id="b849f-164">**Paměť pracovní sady**</span><span class="sxs-lookup"><span data-stu-id="b849f-164">**Memory working set**</span></span>
  * <span data-ttu-id="b849f-165">Aktuální velikost paměti, které aplikace hello v MIB.</span><span class="sxs-lookup"><span data-stu-id="b849f-165">Current amount of memory used by hello app in MiBs.</span></span>
* <span data-ttu-id="b849f-166">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="b849f-166">**Requests**</span></span>
  * <span data-ttu-id="b849f-167">Celkový počet požadavků bez ohledu na jejich výsledné stavový kód HTTP.</span><span class="sxs-lookup"><span data-stu-id="b849f-167">Total number of requests regardless of their resulting HTTP status code.</span></span>

<span data-ttu-id="b849f-168">Pro **plán služby App Service**, jsou dostupné metriky hello:</span><span class="sxs-lookup"><span data-stu-id="b849f-168">For an **App Service plan**, hello available metrics are:</span></span>

> [!NOTE]
> <span data-ttu-id="b849f-169">Metriky plánu služby App Service jsou k dispozici pro plány v pouze **základní**, **standardní** a **Premium** SKU.</span><span class="sxs-lookup"><span data-stu-id="b849f-169">App Service plan metrics are only available for plans in **Basic**, **Standard** and **Premium** SKU.</span></span>
> 
> 

* <span data-ttu-id="b849f-170">**Procento využití procesoru**</span><span class="sxs-lookup"><span data-stu-id="b849f-170">**CPU Percentage**</span></span>
  * <span data-ttu-id="b849f-171">CPU průměrné Hello použít napříč všemi instancemi plánu hello.</span><span class="sxs-lookup"><span data-stu-id="b849f-171">hello average CPU used across all instances of hello plan.</span></span>
* <span data-ttu-id="b849f-172">**Procento paměti**</span><span class="sxs-lookup"><span data-stu-id="b849f-172">**Memory Percentage**</span></span>
  * <span data-ttu-id="b849f-173">Průměrná paměti Hello používá napříč všemi instancemi plánu hello.</span><span class="sxs-lookup"><span data-stu-id="b849f-173">hello average memory used across all instances of hello plan.</span></span>
* <span data-ttu-id="b849f-174">**Data v**</span><span class="sxs-lookup"><span data-stu-id="b849f-174">**Data In**</span></span>
  * <span data-ttu-id="b849f-175">Hello průměrná příchozí šířky pásma použít napříč všemi instancemi plánu hello.</span><span class="sxs-lookup"><span data-stu-id="b849f-175">hello average incoming bandwidth used across all instances of hello plan.</span></span>
* <span data-ttu-id="b849f-176">**Data**</span><span class="sxs-lookup"><span data-stu-id="b849f-176">**Data Out**</span></span>
  * <span data-ttu-id="b849f-177">průměr Hello odchozí šířky pásma použít napříč všemi instancemi plánu hello.</span><span class="sxs-lookup"><span data-stu-id="b849f-177">hello average outgoing bandwidth used across all instances of hello plan.</span></span>
* <span data-ttu-id="b849f-178">**Délka fronty disku**</span><span class="sxs-lookup"><span data-stu-id="b849f-178">**Disk Queue Length**</span></span>
  * <span data-ttu-id="b849f-179">Průměrný počet k Hello číst a zapisovat požadavků, které byly zařazeny do fronty pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="b849f-179">hello average number of both read and write requests that were queued on storage.</span></span> <span data-ttu-id="b849f-180">Délka fronty vysoké disku je údajem o aplikaci, která může být zpomalením z důvodu tooexcessive diskové vstupně-výstupních operací.</span><span class="sxs-lookup"><span data-stu-id="b849f-180">A high disk queue length is an indication of an application that might be slowing down due tooexcessive disk I/O.</span></span>
* <span data-ttu-id="b849f-181">**Délka fronty http**</span><span class="sxs-lookup"><span data-stu-id="b849f-181">**Http Queue Length**</span></span>
  * <span data-ttu-id="b849f-182">Hello průměrný počet požadavků HTTP, které měl toosit na hello fronty před splnění.</span><span class="sxs-lookup"><span data-stu-id="b849f-182">hello average number of HTTP requests that had toosit on hello queue before being fulfilled.</span></span> <span data-ttu-id="b849f-183">Vysoká nebo roste délka fronty HTTP je příznakem plánu v případě velkého zatížení.</span><span class="sxs-lookup"><span data-stu-id="b849f-183">A high or increasing HTTP Queue length is a symptom of a plan under heavy load.</span></span>

### <a name="cpu-time-vs-cpu-percentage"></a><span data-ttu-id="b849f-184">Procentuální hodnota vs procesoru času procesoru</span><span class="sxs-lookup"><span data-stu-id="b849f-184">CPU time vs CPU percentage</span></span>
<!-- toodo: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

<span data-ttu-id="b849f-185">Existují 2 metriky, které odpovídají využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="b849f-185">There are 2 metrics that reflect CPU usage.</span></span> <span data-ttu-id="b849f-186">**Čas procesoru** a **procento využití procesoru**</span><span class="sxs-lookup"><span data-stu-id="b849f-186">**CPU time** and **CPU percentage**</span></span>

<span data-ttu-id="b849f-187">**Čas procesoru** je užitečné pro aplikace hostované v **volné** nebo **sdílené** plány, protože jeden z jejich kvóty je definována v minutách procesoru používané aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b849f-187">**CPU Time** is useful for apps hosted in **Free** or **Shared** plans since one of their quotas is defined in CPU minutes used by hello app.</span></span>

<span data-ttu-id="b849f-188">**Procento využití procesoru** na hello druhé straně je užitečné, pro aplikace hostované v **základní**, **standardní** a **premium** plány vzhledem k tomu může být škálovat na více systémů a tato metrika je dobrá indikace toho Dobrý den celkové využití v rámci všech instancí.</span><span class="sxs-lookup"><span data-stu-id="b849f-188">**CPU percentage** on hello other hand is useful for apps hosted in **basic**, **standard** and **premium** plans since they can be scaled out and this metric is a good indication of hello overall usage across all instances.</span></span>

## <a name="metrics-granularity-and-retention-policy"></a><span data-ttu-id="b849f-189">Metriky Členitosti a zásady uchovávání informací</span><span class="sxs-lookup"><span data-stu-id="b849f-189">Metrics Granularity and Retention Policy</span></span>
<span data-ttu-id="b849f-190">Metriky pro plán služby aplikace a aplikace jsou protokolovány a agregovat služba hello s hello členitostí a zásady uchovávání informací:</span><span class="sxs-lookup"><span data-stu-id="b849f-190">Metrics for an application and app service plan are logged and aggregated by hello service with hello following granularities and retention policies:</span></span>

* <span data-ttu-id="b849f-191">**Minutu** členitosti metriky se zachovají pro **48 hodin**</span><span class="sxs-lookup"><span data-stu-id="b849f-191">**Minute** granularity metrics are retained for **48 hours**</span></span>
* <span data-ttu-id="b849f-192">**Hodina** členitosti metriky se zachovají pro **30 dnů**</span><span class="sxs-lookup"><span data-stu-id="b849f-192">**Hour** granularity metrics are retained for **30 days**</span></span>
* <span data-ttu-id="b849f-193">**Den** členitosti metriky se zachovají pro **90 dnů**</span><span class="sxs-lookup"><span data-stu-id="b849f-193">**Day** granularity metrics are retained for **90 days**</span></span>

## <a name="monitoring-quotas-and-metrics-in-hello-azure-portal"></a><span data-ttu-id="b849f-194">Monitorování kvóty a metriky v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b849f-194">Monitoring Quotas and Metrics in hello Azure Portal.</span></span>
<span data-ttu-id="b849f-195">Můžete zkontrolovat stav hello hello různých **kvóty** a **metriky** ovlivnění aplikace v hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b849f-195">You can review hello status of hello different **quotas** and **metrics** affecting an application in hello [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="b849f-196">![][quotas]
**Kvóty** naleznete v části Nastavení >**kvóty**.</span><span class="sxs-lookup"><span data-stu-id="b849f-196">![][quotas]
**Quotas** can be found under Settings>**Quotas**.</span></span> <span data-ttu-id="b849f-197">Hello UX umožňuje zkontrolovat: název kvóty hello (1), (2) jeho interval resetování, (3) jeho aktuální limit a (4) aktuální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b849f-197">hello UX allows you to review: (1) hello quotas name, (2) its reset interval, (3) its current limit and (4) current value.</span></span>

<span data-ttu-id="b849f-198">![][metrics]
**Metriky** může být přístup přímo z okna prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="b849f-198">![][metrics]
**Metrics** can be access directly from hello resource blade.</span></span> <span data-ttu-id="b849f-199">Můžete také upravit graf hello podle: (1) **klikněte na tlačítko** na a vyberte (2) **upravit graf**.</span><span class="sxs-lookup"><span data-stu-id="b849f-199">You can also customize hello chart by: (1) **click** on it, and select (2) **edit chart**.</span></span>
<span data-ttu-id="b849f-200">Odsud můžete změnit hello (3) **čas rozsah**, (4) **typ grafu**a (5) **metriky** toodisplay.</span><span class="sxs-lookup"><span data-stu-id="b849f-200">From here you can change hello (3) **time range**, (4) **chart type**, and (5) **metrics** toodisplay.</span></span>  

<span data-ttu-id="b849f-201">Další informace o metrikách zde: [monitorovat metriky služby](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="b849f-201">You can learn more about metrics here: [Monitor service metrics](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span></span>

## <a name="alerts-and-autoscale"></a><span data-ttu-id="b849f-202">Výstrahy a škálování</span><span class="sxs-lookup"><span data-stu-id="b849f-202">Alerts and Autoscale</span></span>
<span data-ttu-id="b849f-203">Metriky pro plán aplikace nebo služby App Service může být připojili tooalerts, toolearn více informací o to, najdete v části [přijímat oznámení výstrah](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)</span><span class="sxs-lookup"><span data-stu-id="b849f-203">Metrics for an App or App Service plan can be hooked up tooalerts, toolearn more about this, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)</span></span>

<span data-ttu-id="b849f-204">Aplikace služby App Service hostované v basic, standard nebo premium plány služby App Service podporují **škálování**.</span><span class="sxs-lookup"><span data-stu-id="b849f-204">App Service apps hosted in basic, standard or premium App Service Plans support **autoscale**.</span></span> <span data-ttu-id="b849f-205">To vám umožní tooconfigure pravidla, která monitorování metriky plánu služby App Service a můžete zvýšit nebo snížit počet instancí hello poskytuje další zdroje informací, podle potřeby nebo ukládání peníze přečerpání zřídit po hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b849f-205">This allows you tooconfigure rules that monitor the App Service plan metrics and can increase or decrease hello instance count providing additional resources as needed, or saving money when hello application is over-provision.</span></span> <span data-ttu-id="b849f-206">Další informace o automatické škálování zde: [jak tooScale](../monitoring-and-diagnostics/insights-how-to-scale.md) a zde [osvědčené postupy pro automatické škálování Azure monitorování](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)</span><span class="sxs-lookup"><span data-stu-id="b849f-206">You can learn more about auto scale here: [How tooScale](../monitoring-and-diagnostics/insights-how-to-scale.md) and here [Best practices for Azure Monitor autoscaling](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)</span></span>

> [!NOTE]
> <span data-ttu-id="b849f-207">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="b849f-207">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="b849f-208">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="b849f-208">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="b849f-209">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="b849f-209">What's changed</span></span>
* <span data-ttu-id="b849f-210">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="b849f-210">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
