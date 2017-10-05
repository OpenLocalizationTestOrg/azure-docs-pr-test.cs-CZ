---
title: "Aplikace Azure Statistika Telemetrie datový Model - kontextu Telemetrie | Microsoft Docs"
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
ms.openlocfilehash: d6a0cad8bda6ca68aa691867e84f540c5ac9f6f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="telemetry-context-application-insights-data-model"></a><span data-ttu-id="1858a-103">Kontext telemetrie: Application Insights datový model</span><span class="sxs-lookup"><span data-stu-id="1858a-103">Telemetry context: Application Insights data model</span></span>

<span data-ttu-id="1858a-104">Každá položka telemetrie může mít pole silného typu kontextu.</span><span class="sxs-lookup"><span data-stu-id="1858a-104">Every telemetry item may have a strongly typed context fields.</span></span> <span data-ttu-id="1858a-105">Každé pole umožňuje určitého scénáře monitorování.</span><span class="sxs-lookup"><span data-stu-id="1858a-105">Every field enables a specific monitoring scenario.</span></span> <span data-ttu-id="1858a-106">Kolekce vlastních vlastností slouží k ukládání kontextové informace specifické pro aplikaci nebo vlastní.</span><span class="sxs-lookup"><span data-stu-id="1858a-106">Use the custom properties collection to store custom or application-specific contextual information.</span></span>


##<a name="application-version"></a><span data-ttu-id="1858a-107">Verze aplikace</span><span class="sxs-lookup"><span data-stu-id="1858a-107">Application version</span></span>

<span data-ttu-id="1858a-108">Informace v kontextu pole aplikace je vždy o aplikaci, která odesílá telemetrii.</span><span class="sxs-lookup"><span data-stu-id="1858a-108">Information in the application context fields is always about the application that is sending the telemetry.</span></span> <span data-ttu-id="1858a-109">Verze aplikace se používá k analýze trendů změny v chování aplikace a její korelace k nasazení.</span><span class="sxs-lookup"><span data-stu-id="1858a-109">Application version is used to analyze trend changes in the application behavior and its correlation to the deployments.</span></span>

<span data-ttu-id="1858a-110">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="1858a-110">Max length: 1024</span></span>


##<a name="client-ip-address"></a><span data-ttu-id="1858a-111">IP adresa klienta</span><span class="sxs-lookup"><span data-stu-id="1858a-111">Client IP address</span></span>

<span data-ttu-id="1858a-112">IP adresa klientského zařízení.</span><span class="sxs-lookup"><span data-stu-id="1858a-112">The IP address of the client device.</span></span> <span data-ttu-id="1858a-113">IPv4 a IPv6 jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="1858a-113">IPv4 and IPv6 are supported.</span></span> <span data-ttu-id="1858a-114">Při odesílání telemetrických dat ze služby umístění kontextu je o uživatele, který spustil operace ve službě.</span><span class="sxs-lookup"><span data-stu-id="1858a-114">When telemetry is sent from a service, the location context is about the user that initiated the operation in the service.</span></span> <span data-ttu-id="1858a-115">Application Insights extrahovat informace geografického umístění, z IP adresy klienta a pak ho zkrátit.</span><span class="sxs-lookup"><span data-stu-id="1858a-115">Application Insights extract the geo-location information from the client IP and then truncate it.</span></span> <span data-ttu-id="1858a-116">IP adresa klienta samostatně proto nelze použít jako osobní údaje koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="1858a-116">So client IP by itself cannot be used as end-user identifiable information.</span></span> 

<span data-ttu-id="1858a-117">Maximální délka: 46</span><span class="sxs-lookup"><span data-stu-id="1858a-117">Max length: 46</span></span>


##<a name="device-type"></a><span data-ttu-id="1858a-118">Typ zařízení</span><span class="sxs-lookup"><span data-stu-id="1858a-118">Device type</span></span>

<span data-ttu-id="1858a-119">Toto pole byl původně slouží k určení typu zařízení, kterou používá koncový uživatel aplikace.</span><span class="sxs-lookup"><span data-stu-id="1858a-119">Originally this field was used to indicate the type of the device the end user of the application is using.</span></span> <span data-ttu-id="1858a-120">Dnes používaná primárně k rozlišení telemetrie JavaScript s typem zařízení, prohlížeč, z telemetrických dat na straně serveru se zařízením zadejte "Počítač".</span><span class="sxs-lookup"><span data-stu-id="1858a-120">Today used primarily to distinguish JavaScript telemetry with the device type 'Browser' from server-side telemetry with the device type 'PC'.</span></span>

<span data-ttu-id="1858a-121">Maximální délka: 64</span><span class="sxs-lookup"><span data-stu-id="1858a-121">Max length: 64</span></span>


##<a name="operation-id"></a><span data-ttu-id="1858a-122">Id operace</span><span class="sxs-lookup"><span data-stu-id="1858a-122">Operation id</span></span>

<span data-ttu-id="1858a-123">Jedinečný identifikátor kořenové operace.</span><span class="sxs-lookup"><span data-stu-id="1858a-123">A unique identifier of the root operation.</span></span> <span data-ttu-id="1858a-124">Tento identifikátor skupiny telemetrie umožňuje napříč více součástí.</span><span class="sxs-lookup"><span data-stu-id="1858a-124">This identifier allows to group telemetry across multiple components.</span></span> <span data-ttu-id="1858a-125">V tématu [telemetrie korelace](application-insights-correlation.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1858a-125">See [telemetry correlation](application-insights-correlation.md) for details.</span></span> <span data-ttu-id="1858a-126">Id operace vytvoří žádost o nebo zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="1858a-126">The operation id is created by either a request or a page view.</span></span> <span data-ttu-id="1858a-127">Všechny další telemetrií nastaví toto pole na hodnotu obsahující zobrazení požadavku nebo stránky.</span><span class="sxs-lookup"><span data-stu-id="1858a-127">All other telemetry sets this field to the value for the containing request or page view.</span></span> 

<span data-ttu-id="1858a-128">Maximální délka: 128</span><span class="sxs-lookup"><span data-stu-id="1858a-128">Max length: 128</span></span>


##<a name="parent-operation-id"></a><span data-ttu-id="1858a-129">ID nadřazené operace</span><span class="sxs-lookup"><span data-stu-id="1858a-129">Parent operation ID</span></span>

<span data-ttu-id="1858a-130">Jedinečný identifikátor okamžitou nadřazené položce telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1858a-130">The unique identifier of the telemetry item's immediate parent.</span></span> <span data-ttu-id="1858a-131">V tématu [telemetrie korelace](application-insights-correlation.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1858a-131">See [telemetry correlation](application-insights-correlation.md) for details.</span></span>

<span data-ttu-id="1858a-132">Maximální délka: 128</span><span class="sxs-lookup"><span data-stu-id="1858a-132">Max length: 128</span></span>


##<a name="operation-name"></a><span data-ttu-id="1858a-133">Název operace</span><span class="sxs-lookup"><span data-stu-id="1858a-133">Operation name</span></span>

<span data-ttu-id="1858a-134">Název (skupina) operaci.</span><span class="sxs-lookup"><span data-stu-id="1858a-134">The name (group) of the operation.</span></span> <span data-ttu-id="1858a-135">Název operace vytvoří žádost o nebo zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="1858a-135">The operation name is created by either a request or a page view.</span></span> <span data-ttu-id="1858a-136">Všechny ostatní položky telemetrie nastavte pole na hodnotu obsahující zobrazení požadavku nebo stránky.</span><span class="sxs-lookup"><span data-stu-id="1858a-136">All other telemetry items set this field to the value for the containing request or page view.</span></span> <span data-ttu-id="1858a-137">Název operace se používá pro hledání všech telemetrie položek pro skupinu operací (například "GET domovské/indexem").</span><span class="sxs-lookup"><span data-stu-id="1858a-137">Operation name is used for finding all the telemetry items for a group of operations (for example 'GET Home/Index').</span></span> <span data-ttu-id="1858a-138">Tato vlastnost kontextu se používá k odpovědi na otázky typu "jaké jsou typické výjimky vydané na této stránce."</span><span class="sxs-lookup"><span data-stu-id="1858a-138">This context property is used to answer questions like "what are the typical exceptions thrown on this page."</span></span>

<span data-ttu-id="1858a-139">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="1858a-139">Max length: 1024</span></span>


##<a name="synthetic-source-of-the-operation"></a><span data-ttu-id="1858a-140">Syntetické zdroj operace</span><span class="sxs-lookup"><span data-stu-id="1858a-140">Synthetic source of the operation</span></span>

<span data-ttu-id="1858a-141">Název syntetické zdroje.</span><span class="sxs-lookup"><span data-stu-id="1858a-141">Name of synthetic source.</span></span> <span data-ttu-id="1858a-142">Nějaké telemetrie z aplikace může představovat syntetické provoz.</span><span class="sxs-lookup"><span data-stu-id="1858a-142">Some telemetry from the application may represent synthetic traffic.</span></span> <span data-ttu-id="1858a-143">To může být webový prohledávací modul indexování webu, testy dostupnosti webu nebo trasování z diagnostiky knihoven jako Application Insights SDK sám sebe.</span><span class="sxs-lookup"><span data-stu-id="1858a-143">It may be web crawler indexing the web site, site availability tests, or traces from diagnostic libraries like Application Insights SDK itself.</span></span>

<span data-ttu-id="1858a-144">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="1858a-144">Max length: 1024</span></span>


##<a name="session-id"></a><span data-ttu-id="1858a-145">Id relace</span><span class="sxs-lookup"><span data-stu-id="1858a-145">Session id</span></span>

<span data-ttu-id="1858a-146">ID relace - instanci interakce uživatele s aplikací.</span><span class="sxs-lookup"><span data-stu-id="1858a-146">Session ID - the instance of the user's interaction with the app.</span></span> <span data-ttu-id="1858a-147">Informace v polích kontextu relace je vždy o koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="1858a-147">Information in the session context fields is always about the end user.</span></span> <span data-ttu-id="1858a-148">Při odesílání telemetrických dat ze služby kontextu relace je o uživatele, který spustil operace ve službě.</span><span class="sxs-lookup"><span data-stu-id="1858a-148">When telemetry is sent from a service, the session context is about the user that initiated the operation in the service.</span></span>

<span data-ttu-id="1858a-149">Maximální délka: 64</span><span class="sxs-lookup"><span data-stu-id="1858a-149">Max length: 64</span></span>


##<a name="anonymous-user-id"></a><span data-ttu-id="1858a-150">Id anonymní uživatele</span><span class="sxs-lookup"><span data-stu-id="1858a-150">Anonymous user id</span></span>

<span data-ttu-id="1858a-151">Id anonymního uživatele.</span><span class="sxs-lookup"><span data-stu-id="1858a-151">Anonymous user id.</span></span> <span data-ttu-id="1858a-152">Představuje koncový uživatel aplikace.</span><span class="sxs-lookup"><span data-stu-id="1858a-152">Represents the end user of the application.</span></span> <span data-ttu-id="1858a-153">Při odesílání telemetrických dat ze služby je kontext uživatele o uživatele, který spustil operace ve službě.</span><span class="sxs-lookup"><span data-stu-id="1858a-153">When telemetry is sent from a service, the user context is about the user that initiated the operation in the service.</span></span>

<span data-ttu-id="1858a-154">[Vzorkování](app-insights-sampling.md) je jedním z techniky, chcete-li minimalizovat objem shromážděných telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1858a-154">[Sampling](app-insights-sampling.md) is one of the techniques to minimize the amount of collected telemetry.</span></span> <span data-ttu-id="1858a-155">Algoritmus vzorkování pokusí buď ukázka příchozí nebo odchozí korelační telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1858a-155">Sampling algorithm attempts to either sample in or out all the correlated telemetry.</span></span> <span data-ttu-id="1858a-156">Id anonymní uživatele se používá pro generování skóre vzorkování.</span><span class="sxs-lookup"><span data-stu-id="1858a-156">Anonymous user id is used for sampling score generation.</span></span> <span data-ttu-id="1858a-157">Takže id anonymní uživatele by mělo být dostatečně náhodná hodnota.</span><span class="sxs-lookup"><span data-stu-id="1858a-157">So anonymous user id should be a random enough value.</span></span> 

<span data-ttu-id="1858a-158">Pomocí id anonymního uživatele pro uložení uživatelské jméno je zneužití pole.</span><span class="sxs-lookup"><span data-stu-id="1858a-158">Using anonymous user id to store user name is a misuse of the field.</span></span> <span data-ttu-id="1858a-159">Pomocí id uživatele ověřený.</span><span class="sxs-lookup"><span data-stu-id="1858a-159">Use Authenticated user id.</span></span>

<span data-ttu-id="1858a-160">Maximální délka: 128</span><span class="sxs-lookup"><span data-stu-id="1858a-160">Max length: 128</span></span>


##<a name="authenticated-user-id"></a><span data-ttu-id="1858a-161">Id ověřeného uživatele</span><span class="sxs-lookup"><span data-stu-id="1858a-161">Authenticated user id</span></span>

<span data-ttu-id="1858a-162">Id ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="1858a-162">Authenticated user id.</span></span> <span data-ttu-id="1858a-163">Opak id anonymního uživatele, toto pole představuje uživatele s popisným názvem.</span><span class="sxs-lookup"><span data-stu-id="1858a-163">The opposite of anonymous user id, this field represents the user with a friendly name.</span></span> <span data-ttu-id="1858a-164">Od jeho PII informace nejsou shromažďovány ve výchozím nastavení většina SDK.</span><span class="sxs-lookup"><span data-stu-id="1858a-164">Since its PII information it is not collected by default by most SDK.</span></span>

<span data-ttu-id="1858a-165">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="1858a-165">Max length: 1024</span></span>


##<a name="account-id"></a><span data-ttu-id="1858a-166">Id účtu</span><span class="sxs-lookup"><span data-stu-id="1858a-166">Account id</span></span>

<span data-ttu-id="1858a-167">V aplikacích víceklientské Toto je účet ID nebo název, který funguje s uživateli.</span><span class="sxs-lookup"><span data-stu-id="1858a-167">In multi-tenant applications this is the account ID or name, which the user is acting with.</span></span> <span data-ttu-id="1858a-168">Příklady může být ID předplatného pro Azure portal nebo blog název platforma blogu.</span><span class="sxs-lookup"><span data-stu-id="1858a-168">Examples may be subscription ID for Azure portal or blog name blogging platform.</span></span>

<span data-ttu-id="1858a-169">Maximální délka: 1024</span><span class="sxs-lookup"><span data-stu-id="1858a-169">Max length: 1024</span></span>


##<a name="cloud-role"></a><span data-ttu-id="1858a-170">Role cloudu</span><span class="sxs-lookup"><span data-stu-id="1858a-170">Cloud role</span></span>

<span data-ttu-id="1858a-171">Název role aplikace je součástí.</span><span class="sxs-lookup"><span data-stu-id="1858a-171">Name of the role the application is a part of.</span></span> <span data-ttu-id="1858a-172">Mapuje přímo název role v azure.</span><span class="sxs-lookup"><span data-stu-id="1858a-172">Maps directly to the role name in azure.</span></span> <span data-ttu-id="1858a-173">Můžete také použít k rozlišení malých služeb, které jsou součástí jedné aplikace.</span><span class="sxs-lookup"><span data-stu-id="1858a-173">Can also be used to distinguish micro services, which are part of a single application.</span></span>

<span data-ttu-id="1858a-174">Maximální délka: 256</span><span class="sxs-lookup"><span data-stu-id="1858a-174">Max length: 256</span></span>


##<a name="cloud-role-instance"></a><span data-ttu-id="1858a-175">Instance role cloudu</span><span class="sxs-lookup"><span data-stu-id="1858a-175">Cloud role instance</span></span>

<span data-ttu-id="1858a-176">Název instance, kde je aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="1858a-176">Name of the instance where the application is running.</span></span> <span data-ttu-id="1858a-177">Název počítače pro místní, název instance pro Azure.</span><span class="sxs-lookup"><span data-stu-id="1858a-177">Computer name for on-premises, instance name for Azure.</span></span>

<span data-ttu-id="1858a-178">Maximální délka: 256</span><span class="sxs-lookup"><span data-stu-id="1858a-178">Max length: 256</span></span>


##<a name="internal-sdk-version"></a><span data-ttu-id="1858a-179">Interní: Verze sady SDK</span><span class="sxs-lookup"><span data-stu-id="1858a-179">Internal: SDK version</span></span>

<span data-ttu-id="1858a-180">Verze sady SDK.</span><span class="sxs-lookup"><span data-stu-id="1858a-180">SDK version.</span></span> <span data-ttu-id="1858a-181">V tématu https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification informace.</span><span class="sxs-lookup"><span data-stu-id="1858a-181">See https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification for information.</span></span>

<span data-ttu-id="1858a-182">Maximální délka: 64</span><span class="sxs-lookup"><span data-stu-id="1858a-182">Max length: 64</span></span>


##<a name="internal-node-name"></a><span data-ttu-id="1858a-183">Interní: Název uzlu</span><span class="sxs-lookup"><span data-stu-id="1858a-183">Internal: Node name</span></span>

<span data-ttu-id="1858a-184">Toto pole představuje název uzlu používá pro účely fakturace.</span><span class="sxs-lookup"><span data-stu-id="1858a-184">This field represents the node name used for billing purposes.</span></span> <span data-ttu-id="1858a-185">Použijte ho k přepsání standardní detekce uzlů.</span><span class="sxs-lookup"><span data-stu-id="1858a-185">Use it to override the standard detection of nodes.</span></span>

<span data-ttu-id="1858a-186">Maximální délka: 256</span><span class="sxs-lookup"><span data-stu-id="1858a-186">Max length: 256</span></span>


## <a name="next-steps"></a><span data-ttu-id="1858a-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1858a-187">Next steps</span></span>

- <span data-ttu-id="1858a-188">Zjistěte, jak [rozšířit a filtrovat telemetrie](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="1858a-188">Learn how to [extend and filter telemetry](app-insights-api-filtering-sampling.md).</span></span>
- <span data-ttu-id="1858a-189">V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.</span><span class="sxs-lookup"><span data-stu-id="1858a-189">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="1858a-190">Podívejte se na kolekci vlastností kontextu standardní [konfigurace](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).</span><span class="sxs-lookup"><span data-stu-id="1858a-190">Check out standard context properties collection [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).</span></span>
