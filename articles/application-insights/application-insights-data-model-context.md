---
title: "aaaAzure aplikace Insights Telemetrie datový Model - kontextu Telemetrie | Microsoft Docs"
description: "Application Insights telemetrie kontextu datový model"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/15/2017
ms.author: sergkanz
ms.openlocfilehash: 6cdd6240d1c448f883d104a871ee9d5f6b5af2ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="telemetry-context-application-insights-data-model"></a><span data-ttu-id="58680-103">Kontext telemetrie: Application Insights datový model</span><span class="sxs-lookup"><span data-stu-id="58680-103">Telemetry context: Application Insights data model</span></span>

<span data-ttu-id="58680-104">Každá položka telemetrie může mít pole silného typu kontextu.</span><span class="sxs-lookup"><span data-stu-id="58680-104">Every telemetry item may have a strongly typed context fields.</span></span> <span data-ttu-id="58680-105">Každé pole umožňuje určitého scénáře monitorování.</span><span class="sxs-lookup"><span data-stu-id="58680-105">Every field enables a specific monitoring scenario.</span></span> <span data-ttu-id="58680-106">Použijte hello vlastní vlastnosti kolekce toostore vlastní nebo specifické pro aplikaci kontextové informace.</span><span class="sxs-lookup"><span data-stu-id="58680-106">Use hello custom properties collection toostore custom or application-specific contextual information.</span></span>


##<a name="application-version"></a><span data-ttu-id="58680-107">Verze aplikace</span><span class="sxs-lookup"><span data-stu-id="58680-107">Application version</span></span>

<span data-ttu-id="58680-108">Informace v pole kontextu hello aplikace je vždy o hello aplikaci, která odesílá telemetrii hello.</span><span class="sxs-lookup"><span data-stu-id="58680-108">Information in hello application context fields is always about hello application that is sending hello telemetry.</span></span> <span data-ttu-id="58680-109">Verze aplikace je použité tooanalyze trend změny v chování aplikace hello a její nasazení toohello korelace.</span><span class="sxs-lookup"><span data-stu-id="58680-109">Application version is used tooanalyze trend changes in hello application behavior and its correlation toohello deployments.</span></span>

<span data-ttu-id="58680-110">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="58680-110">Max length: 1024</span></span>


##<a name="client-ip-address"></a><span data-ttu-id="58680-111">IP adresa klienta</span><span class="sxs-lookup"><span data-stu-id="58680-111">Client IP address</span></span>

<span data-ttu-id="58680-112">IP adresa Hello hello klientského zařízení.</span><span class="sxs-lookup"><span data-stu-id="58680-112">hello IP address of hello client device.</span></span> <span data-ttu-id="58680-113">IPv4 a IPv6 jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="58680-113">IPv4 and IPv6 are supported.</span></span> <span data-ttu-id="58680-114">Při odesílání telemetrických dat ze služby hello umístění kontext je o hello uživateli, který spustil hello operace ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="58680-114">When telemetry is sent from a service, hello location context is about hello user that initiated hello operation in hello service.</span></span> <span data-ttu-id="58680-115">Application Insights extrahovat informace hello geografického umístění, z IP adresy klienta hello a pak ho zkrátit.</span><span class="sxs-lookup"><span data-stu-id="58680-115">Application Insights extract hello geo-location information from hello client IP and then truncate it.</span></span> <span data-ttu-id="58680-116">IP adresa klienta samostatně proto nelze použít jako osobní údaje koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="58680-116">So client IP by itself cannot be used as end-user identifiable information.</span></span> 

<span data-ttu-id="58680-117">Maximální délka: 46</span><span class="sxs-lookup"><span data-stu-id="58680-117">Max length: 46</span></span>


##<a name="device-type"></a><span data-ttu-id="58680-118">Typ zařízení</span><span class="sxs-lookup"><span data-stu-id="58680-118">Device type</span></span>

<span data-ttu-id="58680-119">Původně byl použit v tomto poli tooindicate hello typ hello zařízení hello koncový uživatel aplikace hello používá.</span><span class="sxs-lookup"><span data-stu-id="58680-119">Originally this field was used tooindicate hello type of hello device hello end user of hello application is using.</span></span> <span data-ttu-id="58680-120">Dnes použít především telemetrie JavaScript toodistinguish s hello typ zařízení, prohlížeč, z telemetrických dat na straně serveru s typem zařízení hello "Počítač".</span><span class="sxs-lookup"><span data-stu-id="58680-120">Today used primarily toodistinguish JavaScript telemetry with hello device type 'Browser' from server-side telemetry with hello device type 'PC'.</span></span>

<span data-ttu-id="58680-121">Maximální délka: 64</span><span class="sxs-lookup"><span data-stu-id="58680-121">Max length: 64</span></span>


##<a name="operation-id"></a><span data-ttu-id="58680-122">Id operace</span><span class="sxs-lookup"><span data-stu-id="58680-122">Operation id</span></span>

<span data-ttu-id="58680-123">Jedinečný identifikátor hello kořenové operace.</span><span class="sxs-lookup"><span data-stu-id="58680-123">A unique identifier of hello root operation.</span></span> <span data-ttu-id="58680-124">Tento identifikátor umožňuje toogroup telemetrie napříč více součástí.</span><span class="sxs-lookup"><span data-stu-id="58680-124">This identifier allows toogroup telemetry across multiple components.</span></span> <span data-ttu-id="58680-125">V tématu [telemetrie korelace](application-insights-correlation.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="58680-125">See [telemetry correlation](application-insights-correlation.md) for details.</span></span> <span data-ttu-id="58680-126">id operace Hello vytvoří žádost o nebo zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="58680-126">hello operation id is created by either a request or a page view.</span></span> <span data-ttu-id="58680-127">Všechny další telemetrií nastaví hodnotu tohoto pole toohello pro hello obsahující zobrazení požadavku nebo stránky.</span><span class="sxs-lookup"><span data-stu-id="58680-127">All other telemetry sets this field toohello value for hello containing request or page view.</span></span> 

<span data-ttu-id="58680-128">Maximální délka: 128</span><span class="sxs-lookup"><span data-stu-id="58680-128">Max length: 128</span></span>


##<a name="parent-operation-id"></a><span data-ttu-id="58680-129">ID nadřazené operace</span><span class="sxs-lookup"><span data-stu-id="58680-129">Parent operation ID</span></span>

<span data-ttu-id="58680-130">Jedinečný identifikátor okamžitou nadřazené položce telemetrie hello Hello.</span><span class="sxs-lookup"><span data-stu-id="58680-130">hello unique identifier of hello telemetry item's immediate parent.</span></span> <span data-ttu-id="58680-131">V tématu [telemetrie korelace](application-insights-correlation.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="58680-131">See [telemetry correlation](application-insights-correlation.md) for details.</span></span>

<span data-ttu-id="58680-132">Maximální délka: 128</span><span class="sxs-lookup"><span data-stu-id="58680-132">Max length: 128</span></span>


##<a name="operation-name"></a><span data-ttu-id="58680-133">Název operace</span><span class="sxs-lookup"><span data-stu-id="58680-133">Operation name</span></span>

<span data-ttu-id="58680-134">Hello název operace hello (skupiny).</span><span class="sxs-lookup"><span data-stu-id="58680-134">hello name (group) of hello operation.</span></span> <span data-ttu-id="58680-135">Název operace Hello vytvoří žádost o nebo zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="58680-135">hello operation name is created by either a request or a page view.</span></span> <span data-ttu-id="58680-136">Všechny ostatní položky telemetrie nastavit hodnotu tohoto pole toohello pro hello obsahující zobrazení požadavku nebo stránky.</span><span class="sxs-lookup"><span data-stu-id="58680-136">All other telemetry items set this field toohello value for hello containing request or page view.</span></span> <span data-ttu-id="58680-137">Název operace se používá pro hledání všech hello telemetrie položek pro skupinu operací (například "GET domovské/indexem").</span><span class="sxs-lookup"><span data-stu-id="58680-137">Operation name is used for finding all hello telemetry items for a group of operations (for example 'GET Home/Index').</span></span> <span data-ttu-id="58680-138">Tato vlastnost kontextu je použité tooanswer otázky typu "jaké jsou typické výjimky hello vydané na této stránce."</span><span class="sxs-lookup"><span data-stu-id="58680-138">This context property is used tooanswer questions like "what are hello typical exceptions thrown on this page."</span></span>

<span data-ttu-id="58680-139">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="58680-139">Max length: 1024</span></span>


##<a name="synthetic-source-of-hello-operation"></a><span data-ttu-id="58680-140">Syntetické zdroj operace hello</span><span class="sxs-lookup"><span data-stu-id="58680-140">Synthetic source of hello operation</span></span>

<span data-ttu-id="58680-141">Název syntetické zdroje.</span><span class="sxs-lookup"><span data-stu-id="58680-141">Name of synthetic source.</span></span> <span data-ttu-id="58680-142">Nějaké telemetrie z aplikace hello může představovat syntetické provoz.</span><span class="sxs-lookup"><span data-stu-id="58680-142">Some telemetry from hello application may represent synthetic traffic.</span></span> <span data-ttu-id="58680-143">Může být web prohledávacího modulu indexování hello webu, testy dostupnosti webu nebo trasování z diagnostiky knihoven jako Application Insights SDK sám sebe.</span><span class="sxs-lookup"><span data-stu-id="58680-143">It may be web crawler indexing hello web site, site availability tests, or traces from diagnostic libraries like Application Insights SDK itself.</span></span>

<span data-ttu-id="58680-144">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="58680-144">Max length: 1024</span></span>


##<a name="session-id"></a><span data-ttu-id="58680-145">Id relace</span><span class="sxs-lookup"><span data-stu-id="58680-145">Session id</span></span>

<span data-ttu-id="58680-146">ID relace - hello instanci hello uživatelské interakce s aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="58680-146">Session ID - hello instance of hello user's interaction with hello app.</span></span> <span data-ttu-id="58680-147">Informace v polích kontextu relace hello je vždy o hello koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="58680-147">Information in hello session context fields is always about hello end user.</span></span> <span data-ttu-id="58680-148">Při odesílání telemetrických dat ze služby kontextu relace hello je o hello uživateli, který spustil hello operace ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="58680-148">When telemetry is sent from a service, hello session context is about hello user that initiated hello operation in hello service.</span></span>

<span data-ttu-id="58680-149">Maximální délka: 64</span><span class="sxs-lookup"><span data-stu-id="58680-149">Max length: 64</span></span>


##<a name="anonymous-user-id"></a><span data-ttu-id="58680-150">Id anonymní uživatele</span><span class="sxs-lookup"><span data-stu-id="58680-150">Anonymous user id</span></span>

<span data-ttu-id="58680-151">Id anonymního uživatele. Představuje hello koncový uživatel aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="58680-151">Anonymous user id. Represents hello end user of hello application.</span></span> <span data-ttu-id="58680-152">Při odesílání telemetrických dat ze služby, o hello uživateli, který spustil hello operace ve službě hello je hello uživatelský kontext.</span><span class="sxs-lookup"><span data-stu-id="58680-152">When telemetry is sent from a service, hello user context is about hello user that initiated hello operation in hello service.</span></span>

<span data-ttu-id="58680-153">[Vzorkování](app-insights-sampling.md) je jedním z hello techniky toominimize hello objem shromážděných telemetrie.</span><span class="sxs-lookup"><span data-stu-id="58680-153">[Sampling](app-insights-sampling.md) is one of hello techniques toominimize hello amount of collected telemetry.</span></span> <span data-ttu-id="58680-154">Vzorkování algoritmus pokusy o tooeither ukázka příchozí nebo odchozí všechny hello korelační telemetrie.</span><span class="sxs-lookup"><span data-stu-id="58680-154">Sampling algorithm attempts tooeither sample in or out all hello correlated telemetry.</span></span> <span data-ttu-id="58680-155">Id anonymní uživatele se používá pro generování skóre vzorkování.</span><span class="sxs-lookup"><span data-stu-id="58680-155">Anonymous user id is used for sampling score generation.</span></span> <span data-ttu-id="58680-156">Takže id anonymní uživatele by mělo být dostatečně náhodná hodnota.</span><span class="sxs-lookup"><span data-stu-id="58680-156">So anonymous user id should be a random enough value.</span></span> 

<span data-ttu-id="58680-157">Pomocí anonymního uživatele id toostore uživatelského jména je zneužití hello pole.</span><span class="sxs-lookup"><span data-stu-id="58680-157">Using anonymous user id toostore user name is a misuse of hello field.</span></span> <span data-ttu-id="58680-158">Pomocí id uživatele ověřený.</span><span class="sxs-lookup"><span data-stu-id="58680-158">Use Authenticated user id.</span></span>

<span data-ttu-id="58680-159">Maximální délka: 128</span><span class="sxs-lookup"><span data-stu-id="58680-159">Max length: 128</span></span>


##<a name="authenticated-user-id"></a><span data-ttu-id="58680-160">Id ověřeného uživatele</span><span class="sxs-lookup"><span data-stu-id="58680-160">Authenticated user id</span></span>

<span data-ttu-id="58680-161">Id ověřeného uživatele. Hello opačným id anonymního uživatele, toto pole představuje hello uživatele s popisným názvem.</span><span class="sxs-lookup"><span data-stu-id="58680-161">Authenticated user id. hello opposite of anonymous user id, this field represents hello user with a friendly name.</span></span> <span data-ttu-id="58680-162">Od jeho PII informace nejsou shromažďovány ve výchozím nastavení většina SDK.</span><span class="sxs-lookup"><span data-stu-id="58680-162">Since its PII information it is not collected by default by most SDK.</span></span>

<span data-ttu-id="58680-163">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="58680-163">Max length: 1024</span></span>


##<a name="account-id"></a><span data-ttu-id="58680-164">Id účtu</span><span class="sxs-lookup"><span data-stu-id="58680-164">Account id</span></span>

<span data-ttu-id="58680-165">V aplikacích víceklientské jde hello účet ID nebo názvem, který uživatel hello funguje s.</span><span class="sxs-lookup"><span data-stu-id="58680-165">In multi-tenant applications this is hello account ID or name, which hello user is acting with.</span></span> <span data-ttu-id="58680-166">Příklady může být ID předplatného pro Azure portal nebo blog název platforma blogu.</span><span class="sxs-lookup"><span data-stu-id="58680-166">Examples may be subscription ID for Azure portal or blog name blogging platform.</span></span>

<span data-ttu-id="58680-167">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="58680-167">Max length: 1024</span></span>


##<a name="cloud-role"></a><span data-ttu-id="58680-168">Role cloudu</span><span class="sxs-lookup"><span data-stu-id="58680-168">Cloud role</span></span>

<span data-ttu-id="58680-169">Název aplikace hello hello role je součástí.</span><span class="sxs-lookup"><span data-stu-id="58680-169">Name of hello role hello application is a part of.</span></span> <span data-ttu-id="58680-170">Mapuje přímo toohello název role v azure.</span><span class="sxs-lookup"><span data-stu-id="58680-170">Maps directly toohello role name in azure.</span></span> <span data-ttu-id="58680-171">Může být také použít toodistinguish malých služeb, které jsou součástí jedné aplikace.</span><span class="sxs-lookup"><span data-stu-id="58680-171">Can also be used toodistinguish micro services, which are part of a single application.</span></span>

<span data-ttu-id="58680-172">Maximální délka: 256</span><span class="sxs-lookup"><span data-stu-id="58680-172">Max length: 256</span></span>


##<a name="cloud-role-instance"></a><span data-ttu-id="58680-173">Instance role cloudu</span><span class="sxs-lookup"><span data-stu-id="58680-173">Cloud role instance</span></span>

<span data-ttu-id="58680-174">Název instance text hello, kde je spuštěna aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="58680-174">Name of hello instance where hello application is running.</span></span> <span data-ttu-id="58680-175">Název počítače pro místní, název instance pro Azure.</span><span class="sxs-lookup"><span data-stu-id="58680-175">Computer name for on-premises, instance name for Azure.</span></span>

<span data-ttu-id="58680-176">Maximální délka: 256</span><span class="sxs-lookup"><span data-stu-id="58680-176">Max length: 256</span></span>


##<a name="internal-sdk-version"></a><span data-ttu-id="58680-177">Interní: Verze sady SDK</span><span class="sxs-lookup"><span data-stu-id="58680-177">Internal: SDK version</span></span>

<span data-ttu-id="58680-178">Verze sady SDK.</span><span class="sxs-lookup"><span data-stu-id="58680-178">SDK version.</span></span> <span data-ttu-id="58680-179">V tématu https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification informace.</span><span class="sxs-lookup"><span data-stu-id="58680-179">See https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification for information.</span></span>

<span data-ttu-id="58680-180">Maximální délka: 64</span><span class="sxs-lookup"><span data-stu-id="58680-180">Max length: 64</span></span>


##<a name="internal-node-name"></a><span data-ttu-id="58680-181">Interní: Název uzlu</span><span class="sxs-lookup"><span data-stu-id="58680-181">Internal: Node name</span></span>

<span data-ttu-id="58680-182">Toto pole představuje název uzlu hello používá pro účely fakturace.</span><span class="sxs-lookup"><span data-stu-id="58680-182">This field represents hello node name used for billing purposes.</span></span> <span data-ttu-id="58680-183">Použijte toooverride hello standardní detekce uzlů.</span><span class="sxs-lookup"><span data-stu-id="58680-183">Use it toooverride hello standard detection of nodes.</span></span>

<span data-ttu-id="58680-184">Maximální délka: 256</span><span class="sxs-lookup"><span data-stu-id="58680-184">Max length: 256</span></span>


## <a name="next-steps"></a><span data-ttu-id="58680-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="58680-185">Next steps</span></span>

- <span data-ttu-id="58680-186">Zjistěte, jak příliš[rozšířit a filtrovat telemetrie](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="58680-186">Learn how too[extend and filter telemetry](app-insights-api-filtering-sampling.md).</span></span>
- <span data-ttu-id="58680-187">V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.</span><span class="sxs-lookup"><span data-stu-id="58680-187">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="58680-188">Podívejte se na kolekci vlastností kontextu standardní [konfigurace](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).</span><span class="sxs-lookup"><span data-stu-id="58680-188">Check out standard context properties collection [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).</span></span>
