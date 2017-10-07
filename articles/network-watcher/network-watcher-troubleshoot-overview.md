---
title: "řešení potíží s v sledovací proces sítě Azure tooresource aaaIntroduction | Microsoft Docs"
description: "Tato stránka obsahuje přehled řešení problémů s možností prostředků hello sledovací proces sítě"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: c1145cd6-d1cf-4770-b1cc-eaf0464cc315
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: ccbe4c1c2364473aba06e709460d67c773cf25ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooresource-troubleshooting-in-azure-network-watcher"></a><span data-ttu-id="3049e-103">Řešení potíží s v sledovací proces sítě Azure tooresource Úvod</span><span class="sxs-lookup"><span data-stu-id="3049e-103">Introduction tooresource troubleshooting in Azure Network Watcher</span></span>

<span data-ttu-id="3049e-104">Brány virtuální sítě zajistěte připojení mezi místními prostředky a dalším virtuálním sítím v rámci Azure.</span><span class="sxs-lookup"><span data-stu-id="3049e-104">Virtual Network Gateways provide connectivity between on-premises resources and other virtual networks within Azure.</span></span> <span data-ttu-id="3049e-105">Monitorování tyto brány a jejich připojení je velmi důležité tooensuring komunikace není poškozený.</span><span class="sxs-lookup"><span data-stu-id="3049e-105">Monitoring these gateways and their Connections is critical tooensuring communication is not broken.</span></span> <span data-ttu-id="3049e-106">Sledovací proces sítě poskytuje schopnost tootroubleshoot hello brány virtuální sítě a připojení.</span><span class="sxs-lookup"><span data-stu-id="3049e-106">Network Watcher provides hello capability tootroubleshoot Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="3049e-107">To je možné volat prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="3049e-107">This can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="3049e-108">Při volání, sledovací proces sítě diagnostikuje hello stavu brány virtuální sítě hello nebo připojení a příslušné výsledky vrácené hello.</span><span class="sxs-lookup"><span data-stu-id="3049e-108">When called, Network Watcher diagnoses hello health of hello virtual network gateway or connection and return hello appropriate results.</span></span> <span data-ttu-id="3049e-109">Tento požadavek je dlouhotrvající transakci, hello výsledky, se vrátí po dokončení diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="3049e-109">This request is a long running transaction, hello results are returned once hello diagnosis is complete.</span></span>

![portál][2]

## <a name="results"></a><span data-ttu-id="3049e-111">Výsledky</span><span class="sxs-lookup"><span data-stu-id="3049e-111">Results</span></span>

<span data-ttu-id="3049e-112">Hello předběžné výsledky vrácené poskytují celkový přehled o stavu hello hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="3049e-112">hello preliminary results returned give an overall picture of hello health of hello resource.</span></span> <span data-ttu-id="3049e-113">Podrobnější informace lze zadat pro prostředky, jak je znázorněno v následující části hello:</span><span class="sxs-lookup"><span data-stu-id="3049e-113">Deeper information can be provided for resources as shown in hello following section:</span></span>

<span data-ttu-id="3049e-114">Hello následujícím seznamu je hello hodnot vrácených s hello řešení rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="3049e-114">hello following list is hello values returned with hello troubleshoot API:</span></span>

* <span data-ttu-id="3049e-115">**startTime** – tato hodnota je čas hello hello řešení volání rozhraní API spustit.</span><span class="sxs-lookup"><span data-stu-id="3049e-115">**startTime** - This value is hello time hello troubleshoot API call started.</span></span>
* <span data-ttu-id="3049e-116">**čas ukončení** – tato hodnota je hello čas ukončení řešení potíží s hello.</span><span class="sxs-lookup"><span data-stu-id="3049e-116">**endTime** - This value is hello time when hello troubleshooting ended.</span></span>
* <span data-ttu-id="3049e-117">**kód** – tato hodnota není v pořádku, pokud dojde k selhání jedné diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="3049e-117">**code** - This value is UnHealthy, if there is a single diagnosis failure.</span></span>
* <span data-ttu-id="3049e-118">**výsledky** -výsledků je kolekce výsledků vrácených na hello připojení nebo hello brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3049e-118">**results** - Results is a collection of results returned on hello Connection or hello virtual network gateway.</span></span>
    * <span data-ttu-id="3049e-119">**ID** – tato hodnota je typem chyby hello.</span><span class="sxs-lookup"><span data-stu-id="3049e-119">**id** - This value is hello fault type.</span></span>
    * <span data-ttu-id="3049e-120">**Souhrn** – tato hodnota je uveden seznam hello selhání.</span><span class="sxs-lookup"><span data-stu-id="3049e-120">**summary** - This value is a summary of hello fault.</span></span>
    * <span data-ttu-id="3049e-121">**Podrobné** -tuto hodnotu poskytuje podrobný popis chyby hello.</span><span class="sxs-lookup"><span data-stu-id="3049e-121">**detailed** - This value provides a detailed description of hello fault.</span></span>
    * <span data-ttu-id="3049e-122">**recommendedActions** -tato vlastnost je kolekce tootake doporučené akce.</span><span class="sxs-lookup"><span data-stu-id="3049e-122">**recommendedActions** - This property is a collection of recommended actions tootake.</span></span>
      * <span data-ttu-id="3049e-123">**actionText** – tato hodnota obsahuje text hello popisující, co tootake akce.</span><span class="sxs-lookup"><span data-stu-id="3049e-123">**actionText** - This value contains hello text describing what action tootake.</span></span>
      * <span data-ttu-id="3049e-124">**actionUri** -tuto hodnotu poskytuje hello URI toodocumentation tooact.</span><span class="sxs-lookup"><span data-stu-id="3049e-124">**actionUri** - This value provides hello URI toodocumentation on how tooact.</span></span>
      * <span data-ttu-id="3049e-125">**actionUriText** – tato hodnota je krátký popis text hello akce.</span><span class="sxs-lookup"><span data-stu-id="3049e-125">**actionUriText** - This value is a short description of hello action text.</span></span>

<span data-ttu-id="3049e-126">Následující tabulky zobrazit hello různé chyby typy (id pod výsledky z hello předcházející seznamu), které jsou k dispozici Hello a pokud hello selhání vytvoří protokoly.</span><span class="sxs-lookup"><span data-stu-id="3049e-126">hello following tables show hello different fault types (id under results from hello preceding list) that are available and if hello fault creates logs.</span></span>

### <a name="gateway"></a><span data-ttu-id="3049e-127">brána</span><span class="sxs-lookup"><span data-stu-id="3049e-127">Gateway</span></span>

| <span data-ttu-id="3049e-128">Typ chyby</span><span class="sxs-lookup"><span data-stu-id="3049e-128">Fault Type</span></span> | <span data-ttu-id="3049e-129">Důvod</span><span class="sxs-lookup"><span data-stu-id="3049e-129">Reason</span></span> | <span data-ttu-id="3049e-130">Protokol</span><span class="sxs-lookup"><span data-stu-id="3049e-130">Log</span></span>|
|---|---|---|
| <span data-ttu-id="3049e-131">NoFault</span><span class="sxs-lookup"><span data-stu-id="3049e-131">NoFault</span></span> | <span data-ttu-id="3049e-132">Když je zjištěna žádná chyba.</span><span class="sxs-lookup"><span data-stu-id="3049e-132">When no error is detected.</span></span> |<span data-ttu-id="3049e-133">Ano</span><span class="sxs-lookup"><span data-stu-id="3049e-133">Yes</span></span>|
| <span data-ttu-id="3049e-134">GatewayNotFound</span><span class="sxs-lookup"><span data-stu-id="3049e-134">GatewayNotFound</span></span> | <span data-ttu-id="3049e-135">Nelze najít, že není zřízený brány nebo brána.</span><span class="sxs-lookup"><span data-stu-id="3049e-135">Cannot find Gateway or Gateway is not provisioned.</span></span> |<span data-ttu-id="3049e-136">Ne</span><span class="sxs-lookup"><span data-stu-id="3049e-136">No</span></span>|
| <span data-ttu-id="3049e-137">PlannedMaintenance</span><span class="sxs-lookup"><span data-stu-id="3049e-137">PlannedMaintenance</span></span> |  <span data-ttu-id="3049e-138">Instance brány je v rámci údržby.</span><span class="sxs-lookup"><span data-stu-id="3049e-138">Gateway instance is under maintenance.</span></span>  |<span data-ttu-id="3049e-139">Ne</span><span class="sxs-lookup"><span data-stu-id="3049e-139">No</span></span>|
| <span data-ttu-id="3049e-140">UserDrivenUpdate</span><span class="sxs-lookup"><span data-stu-id="3049e-140">UserDrivenUpdate</span></span> | <span data-ttu-id="3049e-141">Pokud je aktualizace uživatele v průběhu.</span><span class="sxs-lookup"><span data-stu-id="3049e-141">When a user update is in progress.</span></span> <span data-ttu-id="3049e-142">To může být operace změny velikosti.</span><span class="sxs-lookup"><span data-stu-id="3049e-142">This could be a resize operation.</span></span> | <span data-ttu-id="3049e-143">Ne</span><span class="sxs-lookup"><span data-stu-id="3049e-143">No</span></span> |
| <span data-ttu-id="3049e-144">VipUnResponsive</span><span class="sxs-lookup"><span data-stu-id="3049e-144">VipUnResponsive</span></span> | <span data-ttu-id="3049e-145">Nelze kontaktovat hello primární instance hello brány.</span><span class="sxs-lookup"><span data-stu-id="3049e-145">Cannot reach hello primary instance of hello Gateway.</span></span> <span data-ttu-id="3049e-146">To se stane, když selže test stavu hello.</span><span class="sxs-lookup"><span data-stu-id="3049e-146">This happens when hello health probe fails.</span></span> | <span data-ttu-id="3049e-147">Ne</span><span class="sxs-lookup"><span data-stu-id="3049e-147">No</span></span> |
| <span data-ttu-id="3049e-148">PlatformInActive</span><span class="sxs-lookup"><span data-stu-id="3049e-148">PlatformInActive</span></span> | <span data-ttu-id="3049e-149">Nastane problém s platformou hello.</span><span class="sxs-lookup"><span data-stu-id="3049e-149">There is an issue with hello platform.</span></span> | <span data-ttu-id="3049e-150">Ne</span><span class="sxs-lookup"><span data-stu-id="3049e-150">No</span></span>|
| <span data-ttu-id="3049e-151">ServiceNotRunning</span><span class="sxs-lookup"><span data-stu-id="3049e-151">ServiceNotRunning</span></span> | <span data-ttu-id="3049e-152">Hello základní služba není spuštěna.</span><span class="sxs-lookup"><span data-stu-id="3049e-152">hello underlying service is not running.</span></span> | <span data-ttu-id="3049e-153">Ne</span><span class="sxs-lookup"><span data-stu-id="3049e-153">No</span></span>|
| <span data-ttu-id="3049e-154">NoConnectionsFoundForGateway</span><span class="sxs-lookup"><span data-stu-id="3049e-154">NoConnectionsFoundForGateway</span></span> | <span data-ttu-id="3049e-155">Žádná připojení existuje v bráně hello.</span><span class="sxs-lookup"><span data-stu-id="3049e-155">No Connections exists on hello gateway.</span></span> <span data-ttu-id="3049e-156">Toto je pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="3049e-156">This is only a warning.</span></span>| <span data-ttu-id="3049e-157">Ne</span><span class="sxs-lookup"><span data-stu-id="3049e-157">No</span></span>|
| <span data-ttu-id="3049e-158">ConnectionsNotConnected</span><span class="sxs-lookup"><span data-stu-id="3049e-158">ConnectionsNotConnected</span></span> | <span data-ttu-id="3049e-159">Připojení nejsou připojené.</span><span class="sxs-lookup"><span data-stu-id="3049e-159">Connections are not connected.</span></span> <span data-ttu-id="3049e-160">Toto je pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="3049e-160">This is only a warning.</span></span>| <span data-ttu-id="3049e-161">Ano</span><span class="sxs-lookup"><span data-stu-id="3049e-161">Yes</span></span>|
| <span data-ttu-id="3049e-162">GatewayCPUUsageExceeded</span><span class="sxs-lookup"><span data-stu-id="3049e-162">GatewayCPUUsageExceeded</span></span> | <span data-ttu-id="3049e-163">Aktuální využití procesoru brány Hello je > 95 %.</span><span class="sxs-lookup"><span data-stu-id="3049e-163">hello current Gateway CPU usage is > 95%.</span></span> | <span data-ttu-id="3049e-164">Ano</span><span class="sxs-lookup"><span data-stu-id="3049e-164">Yes</span></span> |

### <a name="connection"></a><span data-ttu-id="3049e-165">Připojení</span><span class="sxs-lookup"><span data-stu-id="3049e-165">Connection</span></span>

| <span data-ttu-id="3049e-166">Typ chyby</span><span class="sxs-lookup"><span data-stu-id="3049e-166">Fault Type</span></span> | <span data-ttu-id="3049e-167">Důvod</span><span class="sxs-lookup"><span data-stu-id="3049e-167">Reason</span></span> | <span data-ttu-id="3049e-168">Protokol</span><span class="sxs-lookup"><span data-stu-id="3049e-168">Log</span></span>|
|---|---|---|
| <span data-ttu-id="3049e-169">NoFault</span><span class="sxs-lookup"><span data-stu-id="3049e-169">NoFault</span></span> | <span data-ttu-id="3049e-170">Když je zjištěna žádná chyba.</span><span class="sxs-lookup"><span data-stu-id="3049e-170">When no error is detected.</span></span> |<span data-ttu-id="3049e-171">Ano</span><span class="sxs-lookup"><span data-stu-id="3049e-171">Yes</span></span>|
| <span data-ttu-id="3049e-172">GatewayNotFound</span><span class="sxs-lookup"><span data-stu-id="3049e-172">GatewayNotFound</span></span> | <span data-ttu-id="3049e-173">Nelze najít, že není zřízený brány nebo brána.</span><span class="sxs-lookup"><span data-stu-id="3049e-173">Cannot find Gateway or Gateway is not provisioned.</span></span> |<span data-ttu-id="3049e-174">Ne</span><span class="sxs-lookup"><span data-stu-id="3049e-174">No</span></span>|
| <span data-ttu-id="3049e-175">PlannedMaintenance</span><span class="sxs-lookup"><span data-stu-id="3049e-175">PlannedMaintenance</span></span> | <span data-ttu-id="3049e-176">Instance brány je v rámci údržby.</span><span class="sxs-lookup"><span data-stu-id="3049e-176">Gateway instance is under maintenance.</span></span>  |<span data-ttu-id="3049e-177">Ne</span><span class="sxs-lookup"><span data-stu-id="3049e-177">No</span></span>|
| <span data-ttu-id="3049e-178">UserDrivenUpdate</span><span class="sxs-lookup"><span data-stu-id="3049e-178">UserDrivenUpdate</span></span> | <span data-ttu-id="3049e-179">Pokud je aktualizace uživatele v průběhu.</span><span class="sxs-lookup"><span data-stu-id="3049e-179">When a user update is in progress.</span></span> <span data-ttu-id="3049e-180">To může být operace změny velikosti.</span><span class="sxs-lookup"><span data-stu-id="3049e-180">This could be a resize operation.</span></span>  | <span data-ttu-id="3049e-181">Ne</span><span class="sxs-lookup"><span data-stu-id="3049e-181">No</span></span> |
| <span data-ttu-id="3049e-182">VipUnResponsive</span><span class="sxs-lookup"><span data-stu-id="3049e-182">VipUnResponsive</span></span> | <span data-ttu-id="3049e-183">Nelze kontaktovat hello primární instance hello brány.</span><span class="sxs-lookup"><span data-stu-id="3049e-183">Cannot reach hello primary instance of hello Gateway.</span></span> <span data-ttu-id="3049e-184">Ho se stane, když selže test stavu hello.</span><span class="sxs-lookup"><span data-stu-id="3049e-184">It happens when hello health probe fails.</span></span> | <span data-ttu-id="3049e-185">Ne</span><span class="sxs-lookup"><span data-stu-id="3049e-185">No</span></span> |
| <span data-ttu-id="3049e-186">ConnectionEntityNotFound</span><span class="sxs-lookup"><span data-stu-id="3049e-186">ConnectionEntityNotFound</span></span> | <span data-ttu-id="3049e-187">Konfigurace připojení nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="3049e-187">Connection configuration is missing.</span></span> | <span data-ttu-id="3049e-188">Ne</span><span class="sxs-lookup"><span data-stu-id="3049e-188">No</span></span> |
| <span data-ttu-id="3049e-189">ConnectionIsMarkedDisconnected</span><span class="sxs-lookup"><span data-stu-id="3049e-189">ConnectionIsMarkedDisconnected</span></span> | <span data-ttu-id="3049e-190">Hello připojení je označena jako "odpojené".</span><span class="sxs-lookup"><span data-stu-id="3049e-190">hello Connection is marked "disconnected".</span></span> |<span data-ttu-id="3049e-191">Ne</span><span class="sxs-lookup"><span data-stu-id="3049e-191">No</span></span>|
| <span data-ttu-id="3049e-192">ConnectionNotConfiguredOnGateway</span><span class="sxs-lookup"><span data-stu-id="3049e-192">ConnectionNotConfiguredOnGateway</span></span> | <span data-ttu-id="3049e-193">základní služby Hello nemá hello nakonfigurováno připojení.</span><span class="sxs-lookup"><span data-stu-id="3049e-193">hello underlying service does not have hello Connection configured.</span></span> | <span data-ttu-id="3049e-194">Ano</span><span class="sxs-lookup"><span data-stu-id="3049e-194">Yes</span></span> |
| <span data-ttu-id="3049e-195">ConnectionMarkedStandy</span><span class="sxs-lookup"><span data-stu-id="3049e-195">ConnectionMarkedStandy</span></span> | <span data-ttu-id="3049e-196">podkladová služba Hello je označena jako pohotovostní režim.</span><span class="sxs-lookup"><span data-stu-id="3049e-196">hello underlying service is marked as standby.</span></span>| <span data-ttu-id="3049e-197">Ano</span><span class="sxs-lookup"><span data-stu-id="3049e-197">Yes</span></span>|
| <span data-ttu-id="3049e-198">Authentication</span><span class="sxs-lookup"><span data-stu-id="3049e-198">Authentication</span></span> | <span data-ttu-id="3049e-199">Neshoda předsdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="3049e-199">Preshared Key mismatch.</span></span> | <span data-ttu-id="3049e-200">Ano</span><span class="sxs-lookup"><span data-stu-id="3049e-200">Yes</span></span>|
| <span data-ttu-id="3049e-201">PeerReachability</span><span class="sxs-lookup"><span data-stu-id="3049e-201">PeerReachability</span></span> | <span data-ttu-id="3049e-202">Hello sdílené brány není dostupný.</span><span class="sxs-lookup"><span data-stu-id="3049e-202">hello peer gateway is not reachable.</span></span> | <span data-ttu-id="3049e-203">Ano</span><span class="sxs-lookup"><span data-stu-id="3049e-203">Yes</span></span>|
| <span data-ttu-id="3049e-204">IkePolicyMismatch</span><span class="sxs-lookup"><span data-stu-id="3049e-204">IkePolicyMismatch</span></span> | <span data-ttu-id="3049e-205">Hello sdílené brány má IKE zásady, které nejsou podporované službou Azure.</span><span class="sxs-lookup"><span data-stu-id="3049e-205">hello peer gateway has IKE policies that are not supported by Azure.</span></span> | <span data-ttu-id="3049e-206">Ano</span><span class="sxs-lookup"><span data-stu-id="3049e-206">Yes</span></span>|
| <span data-ttu-id="3049e-207">Chyba WfpParse</span><span class="sxs-lookup"><span data-stu-id="3049e-207">WfpParse Error</span></span> | <span data-ttu-id="3049e-208">Došlo k chybě analýzy protokolů Ochrana souborů systému Windows hello.</span><span class="sxs-lookup"><span data-stu-id="3049e-208">An error occurred parsing hello WFP log.</span></span> |<span data-ttu-id="3049e-209">Ano</span><span class="sxs-lookup"><span data-stu-id="3049e-209">Yes</span></span>|

## <a name="supported-gateway-types"></a><span data-ttu-id="3049e-210">Podporované typy brány</span><span class="sxs-lookup"><span data-stu-id="3049e-210">Supported Gateway types</span></span>

<span data-ttu-id="3049e-211">Hello následující seznam obsahuje podporu hello ukazuje připojení a bran, které jsou podporovány při řešení problémů sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="3049e-211">hello following list shows hello support shows which gateways and connections are supported with Network Watcher troubleshooting.</span></span>
|  |  |
|---------|---------|
|<span data-ttu-id="3049e-212">**Typy brány**</span><span class="sxs-lookup"><span data-stu-id="3049e-212">**Gateway types**</span></span>   |         |
|<span data-ttu-id="3049e-213">Síť VPN</span><span class="sxs-lookup"><span data-stu-id="3049e-213">VPN</span></span>      | <span data-ttu-id="3049e-214">Podporuje se</span><span class="sxs-lookup"><span data-stu-id="3049e-214">Supported</span></span>        |
|<span data-ttu-id="3049e-215">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="3049e-215">ExpressRoute</span></span> | <span data-ttu-id="3049e-216">Nepodporuje se</span><span class="sxs-lookup"><span data-stu-id="3049e-216">Not Supported</span></span> |
|<span data-ttu-id="3049e-217">Hypernet</span><span class="sxs-lookup"><span data-stu-id="3049e-217">Hypernet</span></span> | <span data-ttu-id="3049e-218">Nepodporuje se</span><span class="sxs-lookup"><span data-stu-id="3049e-218">Not Supported</span></span>|
|<span data-ttu-id="3049e-219">**Typy sítě VPN**</span><span class="sxs-lookup"><span data-stu-id="3049e-219">**VPN types**</span></span> | |
|<span data-ttu-id="3049e-220">Na základě trasy</span><span class="sxs-lookup"><span data-stu-id="3049e-220">Route Based</span></span> | <span data-ttu-id="3049e-221">Podporuje se</span><span class="sxs-lookup"><span data-stu-id="3049e-221">Supported</span></span>|
|<span data-ttu-id="3049e-222">Na základě zásad</span><span class="sxs-lookup"><span data-stu-id="3049e-222">Policy Based</span></span> | <span data-ttu-id="3049e-223">Nepodporuje se</span><span class="sxs-lookup"><span data-stu-id="3049e-223">Not Supported</span></span>|
|<span data-ttu-id="3049e-224">**Typy připojení**</span><span class="sxs-lookup"><span data-stu-id="3049e-224">**Connection types**</span></span>||
|<span data-ttu-id="3049e-225">Protokol IPSec</span><span class="sxs-lookup"><span data-stu-id="3049e-225">IPSec</span></span>| <span data-ttu-id="3049e-226">Podporuje se</span><span class="sxs-lookup"><span data-stu-id="3049e-226">Supported</span></span>|
|<span data-ttu-id="3049e-227">VNet2Vnet</span><span class="sxs-lookup"><span data-stu-id="3049e-227">VNet2Vnet</span></span>| <span data-ttu-id="3049e-228">Podporuje se</span><span class="sxs-lookup"><span data-stu-id="3049e-228">Supported</span></span>|
|<span data-ttu-id="3049e-229">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="3049e-229">ExpressRoute</span></span>| <span data-ttu-id="3049e-230">Nepodporuje se</span><span class="sxs-lookup"><span data-stu-id="3049e-230">Not Supported</span></span>|
|<span data-ttu-id="3049e-231">Hypernet</span><span class="sxs-lookup"><span data-stu-id="3049e-231">Hypernet</span></span>| <span data-ttu-id="3049e-232">Nepodporuje se</span><span class="sxs-lookup"><span data-stu-id="3049e-232">Not Supported</span></span>|
|<span data-ttu-id="3049e-233">VPNClient</span><span class="sxs-lookup"><span data-stu-id="3049e-233">VPNClient</span></span>| <span data-ttu-id="3049e-234">Nepodporuje se</span><span class="sxs-lookup"><span data-stu-id="3049e-234">Not Supported</span></span>|

## <a name="log-files"></a><span data-ttu-id="3049e-235">Soubory protokolu</span><span class="sxs-lookup"><span data-stu-id="3049e-235">Log files</span></span>

<span data-ttu-id="3049e-236">soubory protokolů Poradce prostředků Hello jsou uložené v účtu úložiště po dokončení řešení potíží s prostředků.</span><span class="sxs-lookup"><span data-stu-id="3049e-236">hello resource troubleshooting log files are stored in a storage account after resource troubleshooting is finished.</span></span> <span data-ttu-id="3049e-237">Hello následující obrázek znázorňuje příklad obsah hello volání, jehož výsledkem chyba.</span><span class="sxs-lookup"><span data-stu-id="3049e-237">hello following image shows hello example contents of a call that resulted in an error.</span></span>

![soubor ZIP][1]

> [!NOTE]
> <span data-ttu-id="3049e-239">V některých případech je zapsán pouze podmnožinu souborů protokolů hello toostorage.</span><span class="sxs-lookup"><span data-stu-id="3049e-239">In some cases, only a subset of hello logs files is written toostorage.</span></span>

<span data-ttu-id="3049e-240">Pokyny ke stahování souborů z účty azure storage, najdete v části příliš[Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="3049e-240">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="3049e-241">Jiný nástroj, který je možné je Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="3049e-241">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="3049e-242">Další informace o Storage Explorer naleznete zde na hello následující odkaz: [Storage Explorer](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="3049e-242">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

### <a name="connectionstatstxt"></a><span data-ttu-id="3049e-243">ConnectionStats.txt</span><span class="sxs-lookup"><span data-stu-id="3049e-243">ConnectionStats.txt</span></span>

<span data-ttu-id="3049e-244">Hello **ConnectionStats.txt** soubor obsahuje celkové statistiky hello připojení, včetně příchozí a Odchozí bajty, stav připojení a hello čas hello bylo vytvořeno připojení.</span><span class="sxs-lookup"><span data-stu-id="3049e-244">hello **ConnectionStats.txt** file contains overall stats of hello Connection, including ingress and egress bytes, Connection status, and hello time hello Connection was established.</span></span>

> [!NOTE]
> <span data-ttu-id="3049e-245">Pokud toohello volání hello řešení potíží s rozhraní API vrátí v pořádku, je hello pouze věcí, vrátí se v souboru zip hello **ConnectionStats.txt** souboru.</span><span class="sxs-lookup"><span data-stu-id="3049e-245">If hello call toohello troubleshooting API returns healthy, hello only thing returned in hello zip file is a **ConnectionStats.txt** file.</span></span>

<span data-ttu-id="3049e-246">Hello obsah tohoto souboru jsou podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="3049e-246">hello contents of this file are similar toohello following example:</span></span>

```
Connectivity State : Connected
Remote Tunnel Endpoint :
Ingress Bytes (since last connected) : 288 B
Egress Bytes (Since last connected) : 288 B
Connected Since : 2/1/2017 8:22:06 PM
```

### <a name="cpustatstxt"></a><span data-ttu-id="3049e-247">CPUStats.txt</span><span class="sxs-lookup"><span data-stu-id="3049e-247">CPUStats.txt</span></span>

<span data-ttu-id="3049e-248">Hello **CPUStats.txt** soubor obsahuje využití procesoru a paměti, které jsou k dispozici v době hello testování.</span><span class="sxs-lookup"><span data-stu-id="3049e-248">hello **CPUStats.txt** file contains CPU usage and memory available at hello time of testing.</span></span>  <span data-ttu-id="3049e-249">Hello obsah tohoto souboru je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="3049e-249">hello contents of this file is similar toohello following example:</span></span>

```
Current CPU Usage : 0 % Current Memory Available : 641 MBs
```

### <a name="ikeerrorstxt"></a><span data-ttu-id="3049e-250">IKEErrors.txt</span><span class="sxs-lookup"><span data-stu-id="3049e-250">IKEErrors.txt</span></span>

<span data-ttu-id="3049e-251">Hello **IKEErrors.txt** soubor obsahuje chyby IKE, ke kterým během monitorování nebyly nalezeny.</span><span class="sxs-lookup"><span data-stu-id="3049e-251">hello **IKEErrors.txt** file contains any IKE errors that were found during monitoring.</span></span>

<span data-ttu-id="3049e-252">Hello následující příklad ukazuje hello obsahu souboru IKEErrors.txt.</span><span class="sxs-lookup"><span data-stu-id="3049e-252">hello following example shows hello contents of an IKEErrors.txt file.</span></span> <span data-ttu-id="3049e-253">Vaše chyby se může lišit v závislosti na hello problém.</span><span class="sxs-lookup"><span data-stu-id="3049e-253">Your errors may be different depending on hello issue.</span></span>

```
Error: Authentication failed. Check shared key. Check crypto. Check lifetimes. 
     based on log : Peer failed with Windows error 13801(ERROR_IPSEC_IKE_AUTH_FAIL)
Error: On-prem device sent invalid payload. 
     based on log : IkeFindPayloadInPacket failed with Windows error 13843(ERROR_IPSEC_IKE_INVALID_PAYLOAD)
```

### <a name="scrubbed-wfpdiagtxt"></a><span data-ttu-id="3049e-254">Očistí wfpdiag.txt</span><span class="sxs-lookup"><span data-stu-id="3049e-254">Scrubbed-wfpdiag.txt</span></span>

<span data-ttu-id="3049e-255">Hello **Scrubbed wfpdiag.txt** protokolový soubor obsahuje hello wfp protokolu.</span><span class="sxs-lookup"><span data-stu-id="3049e-255">hello **Scrubbed-wfpdiag.txt** log file contains hello wfp log.</span></span> <span data-ttu-id="3049e-256">Tento protokol obsahuje protokolování paketu vyřaďte a IKE/AuthIP selhání.</span><span class="sxs-lookup"><span data-stu-id="3049e-256">This log contains logging of packet drop and IKE/AuthIP failures.</span></span>

<span data-ttu-id="3049e-257">Hello následující příklad ukazuje hello obsah souboru Scrubbed wfpdiag.txt hello.</span><span class="sxs-lookup"><span data-stu-id="3049e-257">hello following example shows hello contents of hello Scrubbed-wfpdiag.txt file.</span></span> <span data-ttu-id="3049e-258">V tomto příkladu nebyla hello sdílený klíč připojení správné, jak je vidět z hello 3. řádku zdola hello.</span><span class="sxs-lookup"><span data-stu-id="3049e-258">In this example, hello shared key of a Connection was not correct as can be seen from hello 3rd line from hello bottom.</span></span> <span data-ttu-id="3049e-259">Následující ukázka Hello je právě fragment hello celý protokolu, jako hello protokolu může být náročná v závislosti na hello problém.</span><span class="sxs-lookup"><span data-stu-id="3049e-259">hello following example is just a snippet of hello entire log, as hello log can be lengthy depending on hello issue.</span></span>

```
...
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Deleted ICookie from hello high priority thread pool list
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|IKE diagnostic event:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Event Header:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Timestamp: 1601-01-01T00:00:00.000Z
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Flags: 0x00000106
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Local address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Remote address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IP version field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP version: IPv4
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP protocol: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local address: 13.78.238.92
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote address: 52.161.24.36
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Application ID:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  User SID: <invalid>
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Failure type: IKE/Authip Main Mode Failure
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Type specific info:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure error code:0x000035e9
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IKE authentication credentials are unacceptable
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure point: Remote
...
```

### <a name="wfpdiagtxtsum"></a><span data-ttu-id="3049e-260">wfpdiag.txt.Sum</span><span class="sxs-lookup"><span data-stu-id="3049e-260">wfpdiag.txt.sum</span></span>

<span data-ttu-id="3049e-261">Hello **wfpdiag.txt.sum** je soubor protokolu hello vyrovnávací paměti a zpracovaných událostí.</span><span class="sxs-lookup"><span data-stu-id="3049e-261">hello **wfpdiag.txt.sum** file is a log showing hello buffers and events processed.</span></span>

<span data-ttu-id="3049e-262">Hello následující příklad je hello obsah souboru wfpdiag.txt.sum.</span><span class="sxs-lookup"><span data-stu-id="3049e-262">hello following example is hello contents of a wfpdiag.txt.sum file.</span></span>
```
Files Processed:
    C:\Resources\directory\924336c47dd045d5a246c349b8ae57f2.GatewayTenantWorker.DiagnosticsStorage\2017-02-02T17-34-23\wfpdiag.etl
Total Buffers Processed 8
Total Events  Processed 2169
Total Events  Lost      0
Total Format  Errors    0
Total Formats Unknown   486
Elapsed Time            330 sec
+-----------------------------------------------------------------------------------+
|EventCount    EventName            EventType   TMF                                 |
+-----------------------------------------------------------------------------------+
|        36    ikeext               ike_addr_utils_c844  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        12    ikeext               ike_addr_utils_c857  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        96    ikeext               ike_addr_utils_c832  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|         6    ikeext               ike_bfe_callbacks_c133  1dc2d67f-8381-6303-e314-6c1452eeb529|
|         6    ikeext               ike_bfe_callbacks_c61  1dc2d67f-8381-6303-e314-6c1452eeb529|
|        12    ikeext               ike_sa_management_c5698  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c8447  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c494  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c642  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c3162  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c3307  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
```

## <a name="next-steps"></a><span data-ttu-id="3049e-263">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3049e-263">Next steps</span></span>

<span data-ttu-id="3049e-264">Zjistěte, jak toodiagnose brány sítě VPN a připojení přes hello navštivte stránky portálu [brány řešení potíží – portál Azure](network-watcher-troubleshoot-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3049e-264">Learn how toodiagnose VPN Gateways and Connections through hello portal by visiting [Gateway troubleshooting - Azure portal](network-watcher-troubleshoot-manage-portal.md).</span></span>
<!--Image references-->

[1]: ./media/network-watcher-troubleshoot-overview/GatewayTenantWorkerLogs.png
[2]: ./media/network-watcher-troubleshoot-overview/portal.png
