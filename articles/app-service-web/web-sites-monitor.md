---
title: "Monitorování aplikací v Azure App Service | Microsoft Docs"
description: "Informace o monitorování aplikací v Azure App Service pomocí portálu Azure."
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
ms.openlocfilehash: 25d3776920d683fffedcd8ac6ed0e84dfe875974
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-monitor-apps-in-azure-app-service"></a><span data-ttu-id="e2b3f-103">Postupy: monitorování aplikací v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e2b3f-103">How to: Monitor Apps in Azure App Service</span></span>
<span data-ttu-id="e2b3f-104">[Služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) poskytuje integrované monitorování funkce v [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e2b3f-104">[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) provides built in monitoring functionality in the [Azure Portal](https://portal.azure.com).</span></span>
<span data-ttu-id="e2b3f-105">To zahrnuje možnost zkontrolovat **kvóty** a **metriky** pro aplikace, jakož i plán služby App Service, nastavení **výstrahy** a i **škálování**automaticky v závislosti na tyto metriky.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-105">This includes the ability to review **quotas** and **metrics** for an app as well as the App Service plan, setting up **alerts** and even **scaling** automatically based on these metrics.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a><span data-ttu-id="e2b3f-106">Principy kvóty a metriky</span><span class="sxs-lookup"><span data-stu-id="e2b3f-106">Understanding Quotas and Metrics</span></span>
### <a name="quotas"></a><span data-ttu-id="e2b3f-107">Kvóty</span><span class="sxs-lookup"><span data-stu-id="e2b3f-107">Quotas</span></span>
<span data-ttu-id="e2b3f-108">Aplikace hostované ve službě App Service se vztahují určité *omezení* s prostředky, které mohou používat.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-108">Applications hosted in App Service are subject to certain *limits* on the resources they can use.</span></span> <span data-ttu-id="e2b3f-109">Omezení jsou definovány **plán služby App Service** přidružené aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-109">The limits are defined by the **App Service plan** associated with the app.</span></span>

<span data-ttu-id="e2b3f-110">Pokud je aplikace hostovaná v **volné** nebo **sdílené** plánu, a omezení pro aplikace může používat prostředky jsou definovány **kvóty**.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-110">If the application is hosted in a **Free** or **Shared** plan, then the limits on the resources the app can use are defined by **Quotas**.</span></span>

<span data-ttu-id="e2b3f-111">Pokud je aplikace hostovaná v **základní**, **standardní** nebo **Premium** plánu, omezení pro mohou používat prostředky jsou nastavena **velikost**(Malé, střední, velký) a **instance počet** (1, 2, 3,...) z **plán služby App Service**.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-111">If the application is hosted in a **Basic**, **Standard** or **Premium** plan, then the limits on the resources they can use are set by the **size** (Small, Medium, Large) and **instance count** (1, 2, 3, ...) of the **App Service plan**.</span></span>

<span data-ttu-id="e2b3f-112">**Kvóty** pro **volné** nebo **sdílené** aplikace jsou:</span><span class="sxs-lookup"><span data-stu-id="e2b3f-112">**Quotas** for **Free** or **Shared** apps are:</span></span>

* <span data-ttu-id="e2b3f-113">**CPU(Short)**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-113">**CPU(Short)**</span></span>
  * <span data-ttu-id="e2b3f-114">Nároky na výkon procesoru, které jsou povoleny pro tuto aplikaci v intervalu 5 minut.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-114">Amount of CPU allowed for this application in a 5-minute interval.</span></span> <span data-ttu-id="e2b3f-115">Tato kvóta znovu nastaví každých 5 minut.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-115">This quota re-sets every 5 minutes.</span></span>
* <span data-ttu-id="e2b3f-116">**CPU(Day)**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-116">**CPU(Day)**</span></span>
  * <span data-ttu-id="e2b3f-117">Celkový počet nároky na výkon procesoru, které jsou povoleny pro tuto aplikaci za den.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-117">Total amount of CPU allowed for this application in a day.</span></span> <span data-ttu-id="e2b3f-118">Tato kvóta znovu nastaví každých 24 hodin půlnoci času UTC.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-118">This quota re-sets every 24 hours at midnight UTC.</span></span>
* <span data-ttu-id="e2b3f-119">**Paměť**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-119">**Memory**</span></span>
  * <span data-ttu-id="e2b3f-120">Celková velikost paměti pro tuto aplikaci povolena.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-120">Total amount of memory allowed for this application.</span></span>
* <span data-ttu-id="e2b3f-121">**Šířka pásma**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-121">**Bandwidth**</span></span>
  * <span data-ttu-id="e2b3f-122">Celková velikost odchozí šířky pásma povolená pro tuto aplikaci za den.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-122">Total amount of outgoing bandwidth allowed for this application in a day.</span></span>
    <span data-ttu-id="e2b3f-123">Tato kvóta znovu nastaví každých 24 hodin půlnoci času UTC.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-123">This quota re-sets every 24 hours at midnight UTC.</span></span>
* <span data-ttu-id="e2b3f-124">**Systém souborů**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-124">**Filesystem**</span></span>
  * <span data-ttu-id="e2b3f-125">Celková velikost úložiště, které jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-125">Total amount of storage allowed.</span></span>

<span data-ttu-id="e2b3f-126">Pouze kvótu pro aplikace hostované na **základní**, **standardní** a **Premium** plány je **Filesystem**.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-126">The only quota applicable to apps hosted on **Basic**, **Standard** and **Premium** plans is **Filesystem**.</span></span>

<span data-ttu-id="e2b3f-127">Další informace o konkrétní kvót, omezení a funkce, které jsou k dispozici pro jiné služby SKU aplikace naleznete zde: [omezení předplatné služby Azure](../azure-subscription-service-limits.md#app-service-limits)</span><span class="sxs-lookup"><span data-stu-id="e2b3f-127">More information about the specific quotas, limits and features available to the different App Service SKUs can be found here: [Azure Subscription Service Limits](../azure-subscription-service-limits.md#app-service-limits)</span></span>

#### <a name="quota-enforcement"></a><span data-ttu-id="e2b3f-128">Vynucení kvót</span><span class="sxs-lookup"><span data-stu-id="e2b3f-128">Quota Enforcement</span></span>
<span data-ttu-id="e2b3f-129">Pokud aplikace v jeho využití překročí **procesoru (krátký)**, **procesoru (den)**, nebo **šířky pásma** kvóty a aplikace bude zastaven, dokud nebude znovu nastaví kvótu.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-129">If an application in its usage exceeds the **CPU (short)**, **CPU (Day)**, or **bandwidth** quota then the application will be stopped until the quota re-sets.</span></span> <span data-ttu-id="e2b3f-130">Během této doby bude výsledkem všechny příchozí požadavky **HTTP 403**.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-130">During this time, all incoming requests will result in an **HTTP 403**.</span></span>
![][http403]

<span data-ttu-id="e2b3f-131">Pokud aplikace **paměti** dojde k překročení kvóty a aplikace se nebude znovu spuštěna.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-131">If the application **memory** quota is exceeded, then the application will be re-started.</span></span>

<span data-ttu-id="e2b3f-132">Pokud **Filesystem** dojde k překročení kvóty, pak všechny zápisu se nezdaří, jedná se o zápis do protokolů.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-132">If the **Filesystem** quota is exceeded, then any write operation will fail, this includes writing to logs.</span></span>

<span data-ttu-id="e2b3f-133">Kvóty můžete zvýšit nebo odebrat z vaší aplikace, že si upgradujete plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-133">Quotas can be increased or removed from your app by upgrading your App Service plan.</span></span>

### <a name="metrics"></a><span data-ttu-id="e2b3f-134">Metriky</span><span class="sxs-lookup"><span data-stu-id="e2b3f-134">Metrics</span></span>
<span data-ttu-id="e2b3f-135">**Metriky** poskytují informace o aplikaci nebo chování plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-135">**Metrics** provide information about the app, or App Service plan's behavior.</span></span>

<span data-ttu-id="e2b3f-136">Pro **aplikace**, jsou dostupné metriky:</span><span class="sxs-lookup"><span data-stu-id="e2b3f-136">For an **Application**, the available metrics are:</span></span>

* <span data-ttu-id="e2b3f-137">**Průměrná doba odezvy**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-137">**Average Response Time**</span></span>
  * <span data-ttu-id="e2b3f-138">Průměrná doba pro aplikaci obsluhovat požadavky v ms.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-138">The average time taken for the app to serve requests in ms.</span></span>
* <span data-ttu-id="e2b3f-139">**Průměrná paměti pracovní sady**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-139">**Average memory working set**</span></span>
  * <span data-ttu-id="e2b3f-140">Průměrná velikost paměti v MIB používané aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-140">The average amount of memory in MiBs used by the app.</span></span>
* <span data-ttu-id="e2b3f-141">**Čas procesoru**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-141">**CPU Time**</span></span>
  * <span data-ttu-id="e2b3f-142">Množství procesoru v sekundách spotřebovávají aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-142">The amount of CPU in seconds consumed by the app.</span></span> <span data-ttu-id="e2b3f-143">Další informace o této metriky najdete: [procento vs procesoru čas procesoru](#cpu-time-vs-cpu-percentage)</span><span class="sxs-lookup"><span data-stu-id="e2b3f-143">For more information about this metric see: [CPU time vs CPU percentage](#cpu-time-vs-cpu-percentage)</span></span>
* <span data-ttu-id="e2b3f-144">**Data v**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-144">**Data In**</span></span>
  * <span data-ttu-id="e2b3f-145">Množství příchozích šířky pásma spotřebovávají aplikace v MIB.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-145">The amount of incoming bandwidth consumed by the app in MiBs.</span></span>
* <span data-ttu-id="e2b3f-146">**Data**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-146">**Data Out**</span></span>
  * <span data-ttu-id="e2b3f-147">Množství odchozí šířky pásma spotřebovávají aplikace v MIB.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-147">The amount of outgoing bandwidth consumed by the app in MiBs.</span></span>
* <span data-ttu-id="e2b3f-148">**Http 2xx**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-148">**Http 2xx**</span></span>
  * <span data-ttu-id="e2b3f-149">Počet požadavků, které jsou výsledkem stavový kód protokolu http > = 200 ale < 300.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-149">Count of requests resulting in a http status code >= 200 but < 300.</span></span>
* <span data-ttu-id="e2b3f-150">**Http 3xx**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-150">**Http 3xx**</span></span>
  * <span data-ttu-id="e2b3f-151">Počet požadavků, které jsou výsledkem stavový kód protokolu http > = 300 ale < 400.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-151">Count of requests resulting in a http status code >= 300 but < 400.</span></span>
* <span data-ttu-id="e2b3f-152">**HTTP 401**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-152">**Http 401**</span></span>
  * <span data-ttu-id="e2b3f-153">Počet požadavků, které jsou výsledkem stavový kód HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-153">Count of requests resulting in HTTP 401 status code.</span></span>
* <span data-ttu-id="e2b3f-154">**HTTP 403**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-154">**Http 403**</span></span>
  * <span data-ttu-id="e2b3f-155">Počet požadavků, které jsou výsledkem stavový kód HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-155">Count of requests resulting in HTTP 403 status code.</span></span>
* <span data-ttu-id="e2b3f-156">**HTTP 404**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-156">**Http 404**</span></span>
  * <span data-ttu-id="e2b3f-157">Počet požadavků, které jsou výsledkem stavový kód HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-157">Count of requests resulting in HTTP 404 status code.</span></span>
* <span data-ttu-id="e2b3f-158">**HTTP 406**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-158">**Http 406**</span></span>
  * <span data-ttu-id="e2b3f-159">Počet požadavků, které jsou výsledkem stavový kód HTTP 406.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-159">Count of requests resulting in HTTP 406 status code.</span></span>
* <span data-ttu-id="e2b3f-160">**Http 4xx**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-160">**Http 4xx**</span></span>
  * <span data-ttu-id="e2b3f-161">Počet požadavků, které jsou výsledkem stavový kód protokolu http > = 400 ale < 500.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-161">Count of requests resulting in a http status code >= 400 but < 500.</span></span>
* <span data-ttu-id="e2b3f-162">**Chyby protokolu HTTP serveru**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-162">**Http Server Errors**</span></span>
  * <span data-ttu-id="e2b3f-163">Počet požadavků, které jsou výsledkem stavový kód protokolu http > = 500, ale < 600.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-163">Count of requests resulting in a http status code >= 500 but < 600.</span></span>
* <span data-ttu-id="e2b3f-164">**Paměť pracovní sady**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-164">**Memory working set**</span></span>
  * <span data-ttu-id="e2b3f-165">Aktuální velikost paměti, které aplikace v MIB.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-165">Current amount of memory used by the app in MiBs.</span></span>
* <span data-ttu-id="e2b3f-166">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-166">**Requests**</span></span>
  * <span data-ttu-id="e2b3f-167">Celkový počet požadavků bez ohledu na jejich výsledné stavový kód HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-167">Total number of requests regardless of their resulting HTTP status code.</span></span>

<span data-ttu-id="e2b3f-168">Pro **plán služby App Service**, jsou dostupné metriky:</span><span class="sxs-lookup"><span data-stu-id="e2b3f-168">For an **App Service plan**, the available metrics are:</span></span>

> [!NOTE]
> <span data-ttu-id="e2b3f-169">Metriky plánu služby App Service jsou k dispozici pro plány v pouze **základní**, **standardní** a **Premium** SKU.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-169">App Service plan metrics are only available for plans in **Basic**, **Standard** and **Premium** SKU.</span></span>
> 
> 

* <span data-ttu-id="e2b3f-170">**Procento využití procesoru**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-170">**CPU Percentage**</span></span>
  * <span data-ttu-id="e2b3f-171">Průměrná procesoru použít napříč všemi instancemi plánu.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-171">The average CPU used across all instances of the plan.</span></span>
* <span data-ttu-id="e2b3f-172">**Procento paměti**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-172">**Memory Percentage**</span></span>
  * <span data-ttu-id="e2b3f-173">Průměrná paměť použitá napříč všemi instancemi plánu.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-173">The average memory used across all instances of the plan.</span></span>
* <span data-ttu-id="e2b3f-174">**Data v**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-174">**Data In**</span></span>
  * <span data-ttu-id="e2b3f-175">Průměrná příchozí šířku pásma používanou ve všech instancích plánu.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-175">The average incoming bandwidth used across all instances of the plan.</span></span>
* <span data-ttu-id="e2b3f-176">**Data**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-176">**Data Out**</span></span>
  * <span data-ttu-id="e2b3f-177">Průměr odchozí šířky pásma použít napříč všemi instancemi plánu.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-177">The average outgoing bandwidth used across all instances of the plan.</span></span>
* <span data-ttu-id="e2b3f-178">**Délka fronty disku**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-178">**Disk Queue Length**</span></span>
  * <span data-ttu-id="e2b3f-179">Průměrný počet čtení i zápis požadavků, které byly zařazeny do fronty na úložiště.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-179">The average number of both read and write requests that were queued on storage.</span></span> <span data-ttu-id="e2b3f-180">Délka fronty vysoké disku je údajem o aplikaci, která může být zpomalením z důvodu nadměrného diskových operací.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-180">A high disk queue length is an indication of an application that might be slowing down due to excessive disk I/O.</span></span>
* <span data-ttu-id="e2b3f-181">**Délka fronty http**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-181">**Http Queue Length**</span></span>
  * <span data-ttu-id="e2b3f-182">Průměrný počet požadavků HTTP, které musely nacházejí na fronty před splnění.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-182">The average number of HTTP requests that had to sit on the queue before being fulfilled.</span></span> <span data-ttu-id="e2b3f-183">Vysoká nebo roste délka fronty HTTP je příznakem plánu v případě velkého zatížení.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-183">A high or increasing HTTP Queue length is a symptom of a plan under heavy load.</span></span>

### <a name="cpu-time-vs-cpu-percentage"></a><span data-ttu-id="e2b3f-184">Procentuální hodnota vs procesoru času procesoru</span><span class="sxs-lookup"><span data-stu-id="e2b3f-184">CPU time vs CPU percentage</span></span>
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

<span data-ttu-id="e2b3f-185">Existují 2 metriky, které odpovídají využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-185">There are 2 metrics that reflect CPU usage.</span></span> <span data-ttu-id="e2b3f-186">**Čas procesoru** a **procento využití procesoru**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-186">**CPU time** and **CPU percentage**</span></span>

<span data-ttu-id="e2b3f-187">**Čas procesoru** je užitečné pro aplikace hostované v **volné** nebo **sdílené** plány, protože jeden z jejich kvóty je definována v minutách procesoru používané aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-187">**CPU Time** is useful for apps hosted in **Free** or **Shared** plans since one of their quotas is defined in CPU minutes used by the app.</span></span>

<span data-ttu-id="e2b3f-188">**Procento využití procesoru** na druhé straně je užitečné pro aplikace hostované v **základní**, **standardní** a **premium** plány vzhledem k tomu může být škálovat na více systémů a tato metrika je Dobrá indikace toho celkové využití v rámci všech instancí.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-188">**CPU percentage** on the other hand is useful for apps hosted in **basic**, **standard** and **premium** plans since they can be scaled out and this metric is a good indication of the overall usage across all instances.</span></span>

## <a name="metrics-granularity-and-retention-policy"></a><span data-ttu-id="e2b3f-189">Metriky Členitosti a zásady uchovávání informací</span><span class="sxs-lookup"><span data-stu-id="e2b3f-189">Metrics Granularity and Retention Policy</span></span>
<span data-ttu-id="e2b3f-190">Metriky pro aplikace a plán služby app service se protokolují a agregovat služba s následující členitostí a zásady uchovávání informací:</span><span class="sxs-lookup"><span data-stu-id="e2b3f-190">Metrics for an application and app service plan are logged and aggregated by the service with the following granularities and retention policies:</span></span>

* <span data-ttu-id="e2b3f-191">**Minutu** členitosti metriky se zachovají pro **48 hodin**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-191">**Minute** granularity metrics are retained for **48 hours**</span></span>
* <span data-ttu-id="e2b3f-192">**Hodina** členitosti metriky se zachovají pro **30 dnů**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-192">**Hour** granularity metrics are retained for **30 days**</span></span>
* <span data-ttu-id="e2b3f-193">**Den** členitosti metriky se zachovají pro **90 dnů**</span><span class="sxs-lookup"><span data-stu-id="e2b3f-193">**Day** granularity metrics are retained for **90 days**</span></span>

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a><span data-ttu-id="e2b3f-194">Monitorování, kvóty a metriky na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-194">Monitoring Quotas and Metrics in the Azure Portal.</span></span>
<span data-ttu-id="e2b3f-195">Můžete zkontrolovat stav různých **kvóty** a **metriky** ovlivnění aplikace [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e2b3f-195">You can review the status of the different **quotas** and **metrics** affecting an application in the [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="e2b3f-196">![][quotas]
**Kvóty** naleznete v části Nastavení >**kvóty**.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-196">![][quotas]
**Quotas** can be found under Settings>**Quotas**.</span></span> <span data-ttu-id="e2b3f-197">Uživatelského umožňuje zkontrolovat: (1) název kvóty, (2) jeho interval resetování, (3) jeho aktuální limit a (4) aktuální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-197">The UX allows you to review: (1) the quotas name, (2) its reset interval, (3) its current limit and (4) current value.</span></span>

<span data-ttu-id="e2b3f-198">![][metrics]
**Metriky** může být přístup přímo z okna prostředků.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-198">![][metrics]
**Metrics** can be access directly from the resource blade.</span></span> <span data-ttu-id="e2b3f-199">Můžete také upravit graf podle: (1) **klikněte na tlačítko** na a vyberte (2) **upravit graf**.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-199">You can also customize the chart by: (1) **click** on it, and select (2) **edit chart**.</span></span>
<span data-ttu-id="e2b3f-200">Odsud můžete změnit (3) **čas rozsah**, (4) **typ grafu**a (5) **metriky** k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-200">From here you can change the (3) **time range**, (4) **chart type**, and (5) **metrics** to display.</span></span>  

<span data-ttu-id="e2b3f-201">Další informace o metrikách zde: [monitorovat metriky služby](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="e2b3f-201">You can learn more about metrics here: [Monitor service metrics](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span></span>

## <a name="alerts-and-autoscale"></a><span data-ttu-id="e2b3f-202">Výstrahy a škálování</span><span class="sxs-lookup"><span data-stu-id="e2b3f-202">Alerts and Autoscale</span></span>
<span data-ttu-id="e2b3f-203">Metriky plán aplikace nebo služby App Service může být připojených k výstrahy, další informace o tom, najdete v části [přijímat oznámení výstrah](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)</span><span class="sxs-lookup"><span data-stu-id="e2b3f-203">Metrics for an App or App Service plan can be hooked up to alerts, to learn more about this, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)</span></span>

<span data-ttu-id="e2b3f-204">Aplikace služby App Service hostované v basic, standard nebo premium plány služby App Service podporují **škálování**.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-204">App Service apps hosted in basic, standard or premium App Service Plans support **autoscale**.</span></span> <span data-ttu-id="e2b3f-205">To umožňuje můžete nakonfigurovat pravidla monitorování metriky plánu služby App Service a můžete zvýšit nebo snížit počet instancí, že poskytuje další zdroje informací, podle potřeby nebo ukládání peníze, když je aplikace přečerpání zřizování.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-205">This allows you to configure rules that monitor the App Service plan metrics and can increase or decrease the instance count providing additional resources as needed, or saving money when the application is over-provision.</span></span> <span data-ttu-id="e2b3f-206">Další informace o automatické škálování zde: [postup škálování](../monitoring-and-diagnostics/insights-how-to-scale.md) a zde [osvědčené postupy pro automatické škálování Azure monitorování](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)</span><span class="sxs-lookup"><span data-stu-id="e2b3f-206">You can learn more about auto scale here: [How to Scale](../monitoring-and-diagnostics/insights-how-to-scale.md) and here [Best practices for Azure Monitor autoscaling](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)</span></span>

> [!NOTE]
> <span data-ttu-id="e2b3f-207">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-207">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="e2b3f-208">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="e2b3f-208">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="e2b3f-209">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="e2b3f-209">What's changed</span></span>
* <span data-ttu-id="e2b3f-210">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="e2b3f-210">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
