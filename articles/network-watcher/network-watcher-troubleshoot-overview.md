---
title: "Úvod k řešení potíží s v sledovací proces sítě Azure prostředku | Microsoft Docs"
description: "Tato stránka obsahuje přehled možnosti pro odstraňování potíží prostředků sledovací proces sítě"
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
ms.openlocfilehash: 0d5091b682d1b25c47b224394bcc2c46366eeb2a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="introduction-to-resource-troubleshooting-in-azure-network-watcher"></a><span data-ttu-id="c2b95-103">Úvod k řešení potíží s v sledovací proces sítě Azure prostředku</span><span class="sxs-lookup"><span data-stu-id="c2b95-103">Introduction to resource troubleshooting in Azure Network Watcher</span></span>

<span data-ttu-id="c2b95-104">Brány virtuální sítě zajistěte připojení mezi místními prostředky a dalším virtuálním sítím v rámci Azure.</span><span class="sxs-lookup"><span data-stu-id="c2b95-104">Virtual Network Gateways provide connectivity between on-premises resources and other virtual networks within Azure.</span></span> <span data-ttu-id="c2b95-105">Monitorování tyto brány a jejich připojení je důležité používat k zajištění komunikace není přerušeno.</span><span class="sxs-lookup"><span data-stu-id="c2b95-105">Monitoring these gateways and their Connections is critical to ensuring communication is not broken.</span></span> <span data-ttu-id="c2b95-106">Sledovací proces sítě poskytuje možnost Poradce při potížích brány virtuální sítě a připojení.</span><span class="sxs-lookup"><span data-stu-id="c2b95-106">Network Watcher provides the capability to troubleshoot Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="c2b95-107">To je možné volat prostřednictvím portálu, prostředí PowerShell, rozhraní příkazového řádku nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="c2b95-107">This can be called through the portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="c2b95-108">Při volání, sledovací proces sítě diagnostikuje stavu brány virtuální sítě nebo připojení a vrátit správné výsledky.</span><span class="sxs-lookup"><span data-stu-id="c2b95-108">When called, Network Watcher diagnoses the health of the virtual network gateway or connection and return the appropriate results.</span></span> <span data-ttu-id="c2b95-109">Tento požadavek je dlouhotrvající transakci, budou vráceny výsledky, po dokončení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="c2b95-109">This request is a long running transaction, the results are returned once the diagnosis is complete.</span></span>

![portál][2]

## <a name="results"></a><span data-ttu-id="c2b95-111">Výsledky</span><span class="sxs-lookup"><span data-stu-id="c2b95-111">Results</span></span>

<span data-ttu-id="c2b95-112">Předběžné výsledky vrácené poskytují celkový přehled o stavu prostředku.</span><span class="sxs-lookup"><span data-stu-id="c2b95-112">The preliminary results returned give an overall picture of the health of the resource.</span></span> <span data-ttu-id="c2b95-113">Podrobnější informace lze zadat pro prostředky, jak je znázorněno v následující části:</span><span class="sxs-lookup"><span data-stu-id="c2b95-113">Deeper information can be provided for resources as shown in the following section:</span></span>

<span data-ttu-id="c2b95-114">V následujícím seznamu je hodnot vrácených s Poradce při potížích rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="c2b95-114">The following list is the values returned with the troubleshoot API:</span></span>

* <span data-ttu-id="c2b95-115">**startTime** – tato hodnota je spuštění volání rozhraní API Poradce při potížích.</span><span class="sxs-lookup"><span data-stu-id="c2b95-115">**startTime** - This value is the time the troubleshoot API call started.</span></span>
* <span data-ttu-id="c2b95-116">**čas ukončení** – tato hodnota je čas ukončení poradce při potížích.</span><span class="sxs-lookup"><span data-stu-id="c2b95-116">**endTime** - This value is the time when the troubleshooting ended.</span></span>
* <span data-ttu-id="c2b95-117">**kód** – tato hodnota není v pořádku, pokud dojde k selhání jedné diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="c2b95-117">**code** - This value is UnHealthy, if there is a single diagnosis failure.</span></span>
* <span data-ttu-id="c2b95-118">**výsledky** -výsledků je kolekce výsledků vrácených na připojení nebo brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c2b95-118">**results** - Results is a collection of results returned on the Connection or the virtual network gateway.</span></span>
    * <span data-ttu-id="c2b95-119">**ID** – tato hodnota je typem chyby.</span><span class="sxs-lookup"><span data-stu-id="c2b95-119">**id** - This value is the fault type.</span></span>
    * <span data-ttu-id="c2b95-120">**Souhrn** – tato hodnota je souhrn poruchy.</span><span class="sxs-lookup"><span data-stu-id="c2b95-120">**summary** - This value is a summary of the fault.</span></span>
    * <span data-ttu-id="c2b95-121">**Podrobné** -tuto hodnotu poskytuje podrobný popis chyby.</span><span class="sxs-lookup"><span data-stu-id="c2b95-121">**detailed** - This value provides a detailed description of the fault.</span></span>
    * <span data-ttu-id="c2b95-122">**recommendedActions** -tato vlastnost je kolekce doporučených akcí.</span><span class="sxs-lookup"><span data-stu-id="c2b95-122">**recommendedActions** - This property is a collection of recommended actions to take.</span></span>
      * <span data-ttu-id="c2b95-123">**actionText** – tato hodnota obsahuje text popisující, jakou akci chcete provést.</span><span class="sxs-lookup"><span data-stu-id="c2b95-123">**actionText** - This value contains the text describing what action to take.</span></span>
      * <span data-ttu-id="c2b95-124">**actionUri** -tuto hodnotu poskytuje identifikátor URI pro dokumentaci o tom, jak fungují.</span><span class="sxs-lookup"><span data-stu-id="c2b95-124">**actionUri** - This value provides the URI to documentation on how to act.</span></span>
      * <span data-ttu-id="c2b95-125">**actionUriText** – tato hodnota je krátký popis text akce.</span><span class="sxs-lookup"><span data-stu-id="c2b95-125">**actionUriText** - This value is a short description of the action text.</span></span>

<span data-ttu-id="c2b95-126">Následující tabulky popisují různé chyby typy (id pod výsledky v předchozím seznamu), které jsou k dispozici, a pokud selhání vytvoří protokoly.</span><span class="sxs-lookup"><span data-stu-id="c2b95-126">The following tables show the different fault types (id under results from the preceding list) that are available and if the fault creates logs.</span></span>

### <a name="gateway"></a><span data-ttu-id="c2b95-127">brána</span><span class="sxs-lookup"><span data-stu-id="c2b95-127">Gateway</span></span>

| <span data-ttu-id="c2b95-128">Typ chyby</span><span class="sxs-lookup"><span data-stu-id="c2b95-128">Fault Type</span></span> | <span data-ttu-id="c2b95-129">Důvod</span><span class="sxs-lookup"><span data-stu-id="c2b95-129">Reason</span></span> | <span data-ttu-id="c2b95-130">Protokol</span><span class="sxs-lookup"><span data-stu-id="c2b95-130">Log</span></span>|
|---|---|---|
| <span data-ttu-id="c2b95-131">NoFault</span><span class="sxs-lookup"><span data-stu-id="c2b95-131">NoFault</span></span> | <span data-ttu-id="c2b95-132">Když je zjištěna žádná chyba.</span><span class="sxs-lookup"><span data-stu-id="c2b95-132">When no error is detected.</span></span> |<span data-ttu-id="c2b95-133">Ano</span><span class="sxs-lookup"><span data-stu-id="c2b95-133">Yes</span></span>|
| <span data-ttu-id="c2b95-134">GatewayNotFound</span><span class="sxs-lookup"><span data-stu-id="c2b95-134">GatewayNotFound</span></span> | <span data-ttu-id="c2b95-135">Nelze najít, že není zřízený brány nebo brána.</span><span class="sxs-lookup"><span data-stu-id="c2b95-135">Cannot find Gateway or Gateway is not provisioned.</span></span> |<span data-ttu-id="c2b95-136">Ne</span><span class="sxs-lookup"><span data-stu-id="c2b95-136">No</span></span>|
| <span data-ttu-id="c2b95-137">PlannedMaintenance</span><span class="sxs-lookup"><span data-stu-id="c2b95-137">PlannedMaintenance</span></span> |  <span data-ttu-id="c2b95-138">Instance brány je v rámci údržby.</span><span class="sxs-lookup"><span data-stu-id="c2b95-138">Gateway instance is under maintenance.</span></span>  |<span data-ttu-id="c2b95-139">Ne</span><span class="sxs-lookup"><span data-stu-id="c2b95-139">No</span></span>|
| <span data-ttu-id="c2b95-140">UserDrivenUpdate</span><span class="sxs-lookup"><span data-stu-id="c2b95-140">UserDrivenUpdate</span></span> | <span data-ttu-id="c2b95-141">Pokud je aktualizace uživatele v průběhu.</span><span class="sxs-lookup"><span data-stu-id="c2b95-141">When a user update is in progress.</span></span> <span data-ttu-id="c2b95-142">To může být operace změny velikosti.</span><span class="sxs-lookup"><span data-stu-id="c2b95-142">This could be a resize operation.</span></span> | <span data-ttu-id="c2b95-143">Ne</span><span class="sxs-lookup"><span data-stu-id="c2b95-143">No</span></span> |
| <span data-ttu-id="c2b95-144">VipUnResponsive</span><span class="sxs-lookup"><span data-stu-id="c2b95-144">VipUnResponsive</span></span> | <span data-ttu-id="c2b95-145">Nelze kontaktovat primární instance brány.</span><span class="sxs-lookup"><span data-stu-id="c2b95-145">Cannot reach the primary instance of the Gateway.</span></span> <span data-ttu-id="c2b95-146">To se stane, když selže test stavu.</span><span class="sxs-lookup"><span data-stu-id="c2b95-146">This happens when the health probe fails.</span></span> | <span data-ttu-id="c2b95-147">Ne</span><span class="sxs-lookup"><span data-stu-id="c2b95-147">No</span></span> |
| <span data-ttu-id="c2b95-148">PlatformInActive</span><span class="sxs-lookup"><span data-stu-id="c2b95-148">PlatformInActive</span></span> | <span data-ttu-id="c2b95-149">Nastane problém s platformou.</span><span class="sxs-lookup"><span data-stu-id="c2b95-149">There is an issue with the platform.</span></span> | <span data-ttu-id="c2b95-150">Ne</span><span class="sxs-lookup"><span data-stu-id="c2b95-150">No</span></span>|
| <span data-ttu-id="c2b95-151">ServiceNotRunning</span><span class="sxs-lookup"><span data-stu-id="c2b95-151">ServiceNotRunning</span></span> | <span data-ttu-id="c2b95-152">Základní služba není spuštěna.</span><span class="sxs-lookup"><span data-stu-id="c2b95-152">The underlying service is not running.</span></span> | <span data-ttu-id="c2b95-153">Ne</span><span class="sxs-lookup"><span data-stu-id="c2b95-153">No</span></span>|
| <span data-ttu-id="c2b95-154">NoConnectionsFoundForGateway</span><span class="sxs-lookup"><span data-stu-id="c2b95-154">NoConnectionsFoundForGateway</span></span> | <span data-ttu-id="c2b95-155">Žádná připojení existuje v bráně.</span><span class="sxs-lookup"><span data-stu-id="c2b95-155">No Connections exists on the gateway.</span></span> <span data-ttu-id="c2b95-156">Toto je pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="c2b95-156">This is only a warning.</span></span>| <span data-ttu-id="c2b95-157">Ne</span><span class="sxs-lookup"><span data-stu-id="c2b95-157">No</span></span>|
| <span data-ttu-id="c2b95-158">ConnectionsNotConnected</span><span class="sxs-lookup"><span data-stu-id="c2b95-158">ConnectionsNotConnected</span></span> | <span data-ttu-id="c2b95-159">Připojení nejsou připojené.</span><span class="sxs-lookup"><span data-stu-id="c2b95-159">Connections are not connected.</span></span> <span data-ttu-id="c2b95-160">Toto je pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="c2b95-160">This is only a warning.</span></span>| <span data-ttu-id="c2b95-161">Ano</span><span class="sxs-lookup"><span data-stu-id="c2b95-161">Yes</span></span>|
| <span data-ttu-id="c2b95-162">GatewayCPUUsageExceeded</span><span class="sxs-lookup"><span data-stu-id="c2b95-162">GatewayCPUUsageExceeded</span></span> | <span data-ttu-id="c2b95-163">Aktuální využití procesoru brány je > 95 %.</span><span class="sxs-lookup"><span data-stu-id="c2b95-163">The current Gateway CPU usage is > 95%.</span></span> | <span data-ttu-id="c2b95-164">Ano</span><span class="sxs-lookup"><span data-stu-id="c2b95-164">Yes</span></span> |

### <a name="connection"></a><span data-ttu-id="c2b95-165">Připojení</span><span class="sxs-lookup"><span data-stu-id="c2b95-165">Connection</span></span>

| <span data-ttu-id="c2b95-166">Typ chyby</span><span class="sxs-lookup"><span data-stu-id="c2b95-166">Fault Type</span></span> | <span data-ttu-id="c2b95-167">Důvod</span><span class="sxs-lookup"><span data-stu-id="c2b95-167">Reason</span></span> | <span data-ttu-id="c2b95-168">Protokol</span><span class="sxs-lookup"><span data-stu-id="c2b95-168">Log</span></span>|
|---|---|---|
| <span data-ttu-id="c2b95-169">NoFault</span><span class="sxs-lookup"><span data-stu-id="c2b95-169">NoFault</span></span> | <span data-ttu-id="c2b95-170">Když je zjištěna žádná chyba.</span><span class="sxs-lookup"><span data-stu-id="c2b95-170">When no error is detected.</span></span> |<span data-ttu-id="c2b95-171">Ano</span><span class="sxs-lookup"><span data-stu-id="c2b95-171">Yes</span></span>|
| <span data-ttu-id="c2b95-172">GatewayNotFound</span><span class="sxs-lookup"><span data-stu-id="c2b95-172">GatewayNotFound</span></span> | <span data-ttu-id="c2b95-173">Nelze najít, že není zřízený brány nebo brána.</span><span class="sxs-lookup"><span data-stu-id="c2b95-173">Cannot find Gateway or Gateway is not provisioned.</span></span> |<span data-ttu-id="c2b95-174">Ne</span><span class="sxs-lookup"><span data-stu-id="c2b95-174">No</span></span>|
| <span data-ttu-id="c2b95-175">PlannedMaintenance</span><span class="sxs-lookup"><span data-stu-id="c2b95-175">PlannedMaintenance</span></span> | <span data-ttu-id="c2b95-176">Instance brány je v rámci údržby.</span><span class="sxs-lookup"><span data-stu-id="c2b95-176">Gateway instance is under maintenance.</span></span>  |<span data-ttu-id="c2b95-177">Ne</span><span class="sxs-lookup"><span data-stu-id="c2b95-177">No</span></span>|
| <span data-ttu-id="c2b95-178">UserDrivenUpdate</span><span class="sxs-lookup"><span data-stu-id="c2b95-178">UserDrivenUpdate</span></span> | <span data-ttu-id="c2b95-179">Pokud je aktualizace uživatele v průběhu.</span><span class="sxs-lookup"><span data-stu-id="c2b95-179">When a user update is in progress.</span></span> <span data-ttu-id="c2b95-180">To může být operace změny velikosti.</span><span class="sxs-lookup"><span data-stu-id="c2b95-180">This could be a resize operation.</span></span>  | <span data-ttu-id="c2b95-181">Ne</span><span class="sxs-lookup"><span data-stu-id="c2b95-181">No</span></span> |
| <span data-ttu-id="c2b95-182">VipUnResponsive</span><span class="sxs-lookup"><span data-stu-id="c2b95-182">VipUnResponsive</span></span> | <span data-ttu-id="c2b95-183">Nelze kontaktovat primární instance brány.</span><span class="sxs-lookup"><span data-stu-id="c2b95-183">Cannot reach the primary instance of the Gateway.</span></span> <span data-ttu-id="c2b95-184">Ho se stane, když selže test stavu.</span><span class="sxs-lookup"><span data-stu-id="c2b95-184">It happens when the health probe fails.</span></span> | <span data-ttu-id="c2b95-185">Ne</span><span class="sxs-lookup"><span data-stu-id="c2b95-185">No</span></span> |
| <span data-ttu-id="c2b95-186">ConnectionEntityNotFound</span><span class="sxs-lookup"><span data-stu-id="c2b95-186">ConnectionEntityNotFound</span></span> | <span data-ttu-id="c2b95-187">Konfigurace připojení nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="c2b95-187">Connection configuration is missing.</span></span> | <span data-ttu-id="c2b95-188">Ne</span><span class="sxs-lookup"><span data-stu-id="c2b95-188">No</span></span> |
| <span data-ttu-id="c2b95-189">ConnectionIsMarkedDisconnected</span><span class="sxs-lookup"><span data-stu-id="c2b95-189">ConnectionIsMarkedDisconnected</span></span> | <span data-ttu-id="c2b95-190">Připojení je označena jako "odpojené".</span><span class="sxs-lookup"><span data-stu-id="c2b95-190">The Connection is marked "disconnected".</span></span> |<span data-ttu-id="c2b95-191">Ne</span><span class="sxs-lookup"><span data-stu-id="c2b95-191">No</span></span>|
| <span data-ttu-id="c2b95-192">ConnectionNotConfiguredOnGateway</span><span class="sxs-lookup"><span data-stu-id="c2b95-192">ConnectionNotConfiguredOnGateway</span></span> | <span data-ttu-id="c2b95-193">Základní služby není k dispozici připojení nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="c2b95-193">The underlying service does not have the Connection configured.</span></span> | <span data-ttu-id="c2b95-194">Ano</span><span class="sxs-lookup"><span data-stu-id="c2b95-194">Yes</span></span> |
| <span data-ttu-id="c2b95-195">ConnectionMarkedStandy</span><span class="sxs-lookup"><span data-stu-id="c2b95-195">ConnectionMarkedStandy</span></span> | <span data-ttu-id="c2b95-196">Základní služby je označena jako pohotovostní režim.</span><span class="sxs-lookup"><span data-stu-id="c2b95-196">The underlying service is marked as standby.</span></span>| <span data-ttu-id="c2b95-197">Ano</span><span class="sxs-lookup"><span data-stu-id="c2b95-197">Yes</span></span>|
| <span data-ttu-id="c2b95-198">Authentication</span><span class="sxs-lookup"><span data-stu-id="c2b95-198">Authentication</span></span> | <span data-ttu-id="c2b95-199">Neshoda předsdílený klíč.</span><span class="sxs-lookup"><span data-stu-id="c2b95-199">Preshared Key mismatch.</span></span> | <span data-ttu-id="c2b95-200">Ano</span><span class="sxs-lookup"><span data-stu-id="c2b95-200">Yes</span></span>|
| <span data-ttu-id="c2b95-201">PeerReachability</span><span class="sxs-lookup"><span data-stu-id="c2b95-201">PeerReachability</span></span> | <span data-ttu-id="c2b95-202">Sdílené brána není dostupný.</span><span class="sxs-lookup"><span data-stu-id="c2b95-202">The peer gateway is not reachable.</span></span> | <span data-ttu-id="c2b95-203">Ano</span><span class="sxs-lookup"><span data-stu-id="c2b95-203">Yes</span></span>|
| <span data-ttu-id="c2b95-204">IkePolicyMismatch</span><span class="sxs-lookup"><span data-stu-id="c2b95-204">IkePolicyMismatch</span></span> | <span data-ttu-id="c2b95-205">Bránu sdílené má IKE zásady, které nejsou podporované službou Azure.</span><span class="sxs-lookup"><span data-stu-id="c2b95-205">The peer gateway has IKE policies that are not supported by Azure.</span></span> | <span data-ttu-id="c2b95-206">Ano</span><span class="sxs-lookup"><span data-stu-id="c2b95-206">Yes</span></span>|
| <span data-ttu-id="c2b95-207">Chyba WfpParse</span><span class="sxs-lookup"><span data-stu-id="c2b95-207">WfpParse Error</span></span> | <span data-ttu-id="c2b95-208">Došlo k chybě při analýze protokolů Ochrana souborů systému Windows.</span><span class="sxs-lookup"><span data-stu-id="c2b95-208">An error occurred parsing the WFP log.</span></span> |<span data-ttu-id="c2b95-209">Ano</span><span class="sxs-lookup"><span data-stu-id="c2b95-209">Yes</span></span>|

## <a name="supported-gateway-types"></a><span data-ttu-id="c2b95-210">Podporované typy brány</span><span class="sxs-lookup"><span data-stu-id="c2b95-210">Supported Gateway types</span></span>

<span data-ttu-id="c2b95-211">Následující seznam obsahuje podporu ukazuje připojení a bran, které jsou podporovány při řešení problémů sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="c2b95-211">The following list shows the support shows which gateways and connections are supported with Network Watcher troubleshooting.</span></span>
|  |  |
|---------|---------|
|<span data-ttu-id="c2b95-212">**Typy brány**</span><span class="sxs-lookup"><span data-stu-id="c2b95-212">**Gateway types**</span></span>   |         |
|<span data-ttu-id="c2b95-213">Síť VPN</span><span class="sxs-lookup"><span data-stu-id="c2b95-213">VPN</span></span>      | <span data-ttu-id="c2b95-214">Podporuje se</span><span class="sxs-lookup"><span data-stu-id="c2b95-214">Supported</span></span>        |
|<span data-ttu-id="c2b95-215">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="c2b95-215">ExpressRoute</span></span> | <span data-ttu-id="c2b95-216">Nepodporuje se</span><span class="sxs-lookup"><span data-stu-id="c2b95-216">Not Supported</span></span> |
|<span data-ttu-id="c2b95-217">Hypernet</span><span class="sxs-lookup"><span data-stu-id="c2b95-217">Hypernet</span></span> | <span data-ttu-id="c2b95-218">Nepodporuje se</span><span class="sxs-lookup"><span data-stu-id="c2b95-218">Not Supported</span></span>|
|<span data-ttu-id="c2b95-219">**Typy sítě VPN**</span><span class="sxs-lookup"><span data-stu-id="c2b95-219">**VPN types**</span></span> | |
|<span data-ttu-id="c2b95-220">Na základě trasy</span><span class="sxs-lookup"><span data-stu-id="c2b95-220">Route Based</span></span> | <span data-ttu-id="c2b95-221">Podporuje se</span><span class="sxs-lookup"><span data-stu-id="c2b95-221">Supported</span></span>|
|<span data-ttu-id="c2b95-222">Na základě zásad</span><span class="sxs-lookup"><span data-stu-id="c2b95-222">Policy Based</span></span> | <span data-ttu-id="c2b95-223">Nepodporuje se</span><span class="sxs-lookup"><span data-stu-id="c2b95-223">Not Supported</span></span>|
|<span data-ttu-id="c2b95-224">**Typy připojení**</span><span class="sxs-lookup"><span data-stu-id="c2b95-224">**Connection types**</span></span>||
|<span data-ttu-id="c2b95-225">Protokol IPSec</span><span class="sxs-lookup"><span data-stu-id="c2b95-225">IPSec</span></span>| <span data-ttu-id="c2b95-226">Podporuje se</span><span class="sxs-lookup"><span data-stu-id="c2b95-226">Supported</span></span>|
|<span data-ttu-id="c2b95-227">VNet2Vnet</span><span class="sxs-lookup"><span data-stu-id="c2b95-227">VNet2Vnet</span></span>| <span data-ttu-id="c2b95-228">Podporuje se</span><span class="sxs-lookup"><span data-stu-id="c2b95-228">Supported</span></span>|
|<span data-ttu-id="c2b95-229">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="c2b95-229">ExpressRoute</span></span>| <span data-ttu-id="c2b95-230">Nepodporuje se</span><span class="sxs-lookup"><span data-stu-id="c2b95-230">Not Supported</span></span>|
|<span data-ttu-id="c2b95-231">Hypernet</span><span class="sxs-lookup"><span data-stu-id="c2b95-231">Hypernet</span></span>| <span data-ttu-id="c2b95-232">Nepodporuje se</span><span class="sxs-lookup"><span data-stu-id="c2b95-232">Not Supported</span></span>|
|<span data-ttu-id="c2b95-233">VPNClient</span><span class="sxs-lookup"><span data-stu-id="c2b95-233">VPNClient</span></span>| <span data-ttu-id="c2b95-234">Nepodporuje se</span><span class="sxs-lookup"><span data-stu-id="c2b95-234">Not Supported</span></span>|

## <a name="log-files"></a><span data-ttu-id="c2b95-235">Soubory protokolu</span><span class="sxs-lookup"><span data-stu-id="c2b95-235">Log files</span></span>

<span data-ttu-id="c2b95-236">Soubory protokolů Poradce prostředků jsou uložené v účtu úložiště po dokončení řešení potíží s prostředků.</span><span class="sxs-lookup"><span data-stu-id="c2b95-236">The resource troubleshooting log files are stored in a storage account after resource troubleshooting is finished.</span></span> <span data-ttu-id="c2b95-237">Následující obrázek znázorňuje příklad obsah volání, jehož výsledkem chyba.</span><span class="sxs-lookup"><span data-stu-id="c2b95-237">The following image shows the example contents of a call that resulted in an error.</span></span>

![soubor ZIP][1]

> [!NOTE]
> <span data-ttu-id="c2b95-239">V některých případech pouze podmnožinu soubory protokolů se zapíše do úložiště.</span><span class="sxs-lookup"><span data-stu-id="c2b95-239">In some cases, only a subset of the logs files is written to storage.</span></span>

<span data-ttu-id="c2b95-240">Pokyny ke stahování souborů z účty azure storage, najdete v části [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="c2b95-240">For instructions on downloading files from azure storage accounts, refer to [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="c2b95-241">Jiný nástroj, který je možné je Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="c2b95-241">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="c2b95-242">Další informace o Storage Explorer naleznete zde na následující odkaz: [Storage Explorer](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="c2b95-242">More information about Storage Explorer can be found here at the following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

### <a name="connectionstatstxt"></a><span data-ttu-id="c2b95-243">ConnectionStats.txt</span><span class="sxs-lookup"><span data-stu-id="c2b95-243">ConnectionStats.txt</span></span>

<span data-ttu-id="c2b95-244">**ConnectionStats.txt** soubor obsahuje celkové statistiky připojení, včetně příchozí a Odchozí bajty, stav připojení a času, které bylo vytvořeno připojení.</span><span class="sxs-lookup"><span data-stu-id="c2b95-244">The **ConnectionStats.txt** file contains overall stats of the Connection, including ingress and egress bytes, Connection status, and the time the Connection was established.</span></span>

> [!NOTE]
> <span data-ttu-id="c2b95-245">Pokud vrátí volání rozhraní API pro odstraňování potíží v pořádku, je jediné, co, vrátí se v souboru zip **ConnectionStats.txt** souboru.</span><span class="sxs-lookup"><span data-stu-id="c2b95-245">If the call to the troubleshooting API returns healthy, the only thing returned in the zip file is a **ConnectionStats.txt** file.</span></span>

<span data-ttu-id="c2b95-246">Obsah tohoto souboru se podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c2b95-246">The contents of this file are similar to the following example:</span></span>

```
Connectivity State : Connected
Remote Tunnel Endpoint :
Ingress Bytes (since last connected) : 288 B
Egress Bytes (Since last connected) : 288 B
Connected Since : 2/1/2017 8:22:06 PM
```

### <a name="cpustatstxt"></a><span data-ttu-id="c2b95-247">CPUStats.txt</span><span class="sxs-lookup"><span data-stu-id="c2b95-247">CPUStats.txt</span></span>

<span data-ttu-id="c2b95-248">**CPUStats.txt** soubor obsahuje využití procesoru a paměti, které jsou k dispozici při testování.</span><span class="sxs-lookup"><span data-stu-id="c2b95-248">The **CPUStats.txt** file contains CPU usage and memory available at the time of testing.</span></span>  <span data-ttu-id="c2b95-249">Obsah tohoto souboru je stejný jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c2b95-249">The contents of this file is similar to the following example:</span></span>

```
Current CPU Usage : 0 % Current Memory Available : 641 MBs
```

### <a name="ikeerrorstxt"></a><span data-ttu-id="c2b95-250">IKEErrors.txt</span><span class="sxs-lookup"><span data-stu-id="c2b95-250">IKEErrors.txt</span></span>

<span data-ttu-id="c2b95-251">**IKEErrors.txt** soubor obsahuje chyby IKE, ke kterým během monitorování nebyly nalezeny.</span><span class="sxs-lookup"><span data-stu-id="c2b95-251">The **IKEErrors.txt** file contains any IKE errors that were found during monitoring.</span></span>

<span data-ttu-id="c2b95-252">Následující příklad ukazuje obsah souboru IKEErrors.txt.</span><span class="sxs-lookup"><span data-stu-id="c2b95-252">The following example shows the contents of an IKEErrors.txt file.</span></span> <span data-ttu-id="c2b95-253">Chyby se může lišit v závislosti na problém.</span><span class="sxs-lookup"><span data-stu-id="c2b95-253">Your errors may be different depending on the issue.</span></span>

```
Error: Authentication failed. Check shared key. Check crypto. Check lifetimes. 
     based on log : Peer failed with Windows error 13801(ERROR_IPSEC_IKE_AUTH_FAIL)
Error: On-prem device sent invalid payload. 
     based on log : IkeFindPayloadInPacket failed with Windows error 13843(ERROR_IPSEC_IKE_INVALID_PAYLOAD)
```

### <a name="scrubbed-wfpdiagtxt"></a><span data-ttu-id="c2b95-254">Očistí wfpdiag.txt</span><span class="sxs-lookup"><span data-stu-id="c2b95-254">Scrubbed-wfpdiag.txt</span></span>

<span data-ttu-id="c2b95-255">**Scrubbed wfpdiag.txt** protokolový soubor obsahuje protokol Ochrana souborů systému Windows.</span><span class="sxs-lookup"><span data-stu-id="c2b95-255">The **Scrubbed-wfpdiag.txt** log file contains the wfp log.</span></span> <span data-ttu-id="c2b95-256">Tento protokol obsahuje protokolování paketu vyřaďte a IKE/AuthIP selhání.</span><span class="sxs-lookup"><span data-stu-id="c2b95-256">This log contains logging of packet drop and IKE/AuthIP failures.</span></span>

<span data-ttu-id="c2b95-257">Následující příklad ukazuje obsah souboru Scrubbed wfpdiag.txt.</span><span class="sxs-lookup"><span data-stu-id="c2b95-257">The following example shows the contents of the Scrubbed-wfpdiag.txt file.</span></span> <span data-ttu-id="c2b95-258">V tomto příkladu nebyla sdílený klíč připojení správné, jak je vidět z 3. řádku dole.</span><span class="sxs-lookup"><span data-stu-id="c2b95-258">In this example, the shared key of a Connection was not correct as can be seen from the 3rd line from the bottom.</span></span> <span data-ttu-id="c2b95-259">V následujícím příkladu je právě fragment celý protokolu, jako protokol může být náročná v závislosti na problém.</span><span class="sxs-lookup"><span data-stu-id="c2b95-259">The following example is just a snippet of the entire log, as the log can be lengthy depending on the issue.</span></span>

```
...
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Deleted ICookie from the high priority thread pool list
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

### <a name="wfpdiagtxtsum"></a><span data-ttu-id="c2b95-260">wfpdiag.txt.Sum</span><span class="sxs-lookup"><span data-stu-id="c2b95-260">wfpdiag.txt.sum</span></span>

<span data-ttu-id="c2b95-261">**Wfpdiag.txt.sum** je soubor protokolu vyrovnávací paměti a zpracovaných událostí.</span><span class="sxs-lookup"><span data-stu-id="c2b95-261">The **wfpdiag.txt.sum** file is a log showing the buffers and events processed.</span></span>

<span data-ttu-id="c2b95-262">V následujícím příkladu je obsah souboru wfpdiag.txt.sum.</span><span class="sxs-lookup"><span data-stu-id="c2b95-262">The following example is the contents of a wfpdiag.txt.sum file.</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="c2b95-263">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c2b95-263">Next steps</span></span>

<span data-ttu-id="c2b95-264">Zjistěte, jak diagnostikovat brány sítě VPN a připojení přes portál navštivte stránky [brány řešení potíží – portál Azure](network-watcher-troubleshoot-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c2b95-264">Learn how to diagnose VPN Gateways and Connections through the portal by visiting [Gateway troubleshooting - Azure portal](network-watcher-troubleshoot-manage-portal.md).</span></span>
<!--Image references-->

[1]: ./media/network-watcher-troubleshoot-overview/GatewayTenantWorkerLogs.png
[2]: ./media/network-watcher-troubleshoot-overview/portal.png
