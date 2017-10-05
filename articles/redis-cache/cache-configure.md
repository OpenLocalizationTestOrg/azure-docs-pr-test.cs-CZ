---
title: Postup konfigurace Azure Redis Cache | Microsoft Docs
description: "Rady pro pochopení výchozí konfiguraci Redis pro Azure Redis Cache a informace o konfiguraci vaší instance služby Azure Redis Cache"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: d0bf2e1f-6a26-4e62-85ba-d82b35fc5aa6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 08/22/2017
ms.author: sdanie
ms.openlocfilehash: 0274e58eb2e83202d4dbc58da0c67d0fdde22ede
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-azure-redis-cache"></a><span data-ttu-id="66c3a-103">Postup konfigurace Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="66c3a-103">How to configure Azure Redis Cache</span></span>
<span data-ttu-id="66c3a-104">Toto téma popisuje, jak zkontrolovat a aktualizovat konfiguraci pro vaše instance služby Azure Redis Cache a obsahuje výchozí konfiguraci serveru Redis pro instance služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="66c3a-104">This topic describes how to review and update the configuration for your Azure Redis Cache instances, and covers the default Redis server configuration for Azure Redis Cache instances.</span></span>

> [!NOTE]
> <span data-ttu-id="66c3a-105">Další informace o konfiguraci a použití prémiových funkcí mezipaměti najdete v tématu [postup konfigurace trvalosti](cache-how-to-premium-persistence.md), [postup konfigurace clusterů](cache-how-to-premium-clustering.md), a [postup konfigurace podpory služby Virtual Network ](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-105">For more information on configuring and using premium cache features, see [How to configure persistence](cache-how-to-premium-persistence.md), [How to configure clustering](cache-how-to-premium-clustering.md), and [How to configure Virtual Network support](cache-how-to-premium-vnet.md).</span></span>
> 
> 

## <a name="configure-redis-cache-settings"></a><span data-ttu-id="66c3a-106">Konfigurace nastavení mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-106">Configure Redis cache settings</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

<span data-ttu-id="66c3a-107">Nastavení Azure Redis Cache jsou zobrazit a konfigurovat na **Redis Cache** okno pomocí **prostředků nabídky**.</span><span class="sxs-lookup"><span data-stu-id="66c3a-107">Azure Redis Cache settings are viewed and configured on the **Redis Cache** blade using the **Resource Menu**.</span></span>

![Nastavení mezipaměti redis](./media/cache-configure/redis-cache-settings.png)

<span data-ttu-id="66c3a-109">Můžete zobrazit a nakonfigurovat následující nastavení pomocí **prostředků nabídky**.</span><span class="sxs-lookup"><span data-stu-id="66c3a-109">You can view and configure the following settings using the **Resource Menu**.</span></span>

* [<span data-ttu-id="66c3a-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="66c3a-110">Overview</span></span>](#overview)
* [<span data-ttu-id="66c3a-111">Protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="66c3a-111">Activity log</span></span>](#activity-log)
* [<span data-ttu-id="66c3a-112">Řízení přístupu (IAM)</span><span class="sxs-lookup"><span data-stu-id="66c3a-112">Access control (IAM)</span></span>](#access-control-iam)
* [<span data-ttu-id="66c3a-113">Značky</span><span class="sxs-lookup"><span data-stu-id="66c3a-113">Tags</span></span>](#tags)
* [<span data-ttu-id="66c3a-114">Diagnóza a řešení problémů</span><span class="sxs-lookup"><span data-stu-id="66c3a-114">Diagnose and solve problems</span></span>](#diagnose-and-solve-problems)
* [<span data-ttu-id="66c3a-115">Nastavení</span><span class="sxs-lookup"><span data-stu-id="66c3a-115">Settings</span></span>](#settings)
    * [<span data-ttu-id="66c3a-116">Přístupové klávesy</span><span class="sxs-lookup"><span data-stu-id="66c3a-116">Access keys</span></span>](#access-keys)
    * [<span data-ttu-id="66c3a-117">Upřesnit nastavení</span><span class="sxs-lookup"><span data-stu-id="66c3a-117">Advanced settings</span></span>](#advanced-settings)
    * [<span data-ttu-id="66c3a-118">Advisor mezipaměti redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-118">Redis Cache Advisor</span></span>](#redis-cache-advisor)
    * [<span data-ttu-id="66c3a-119">Škálování</span><span class="sxs-lookup"><span data-stu-id="66c3a-119">Scale</span></span>](#scale)
    * [<span data-ttu-id="66c3a-120">Velikost clusteru redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-120">Redis cluster size</span></span>](#cluster-size)
    * [<span data-ttu-id="66c3a-121">Trvalosti dat redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-121">Redis data persistence</span></span>](#redis-data-persistence)
    * [<span data-ttu-id="66c3a-122">Plán aktualizací</span><span class="sxs-lookup"><span data-stu-id="66c3a-122">Schedule updates</span></span>](#schedule-updates)
    * [<span data-ttu-id="66c3a-123">Geografická replikace</span><span class="sxs-lookup"><span data-stu-id="66c3a-123">Geo-replication</span></span>](#geo-replication)
    * [<span data-ttu-id="66c3a-124">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="66c3a-124">Virtual Network</span></span>](#virtual-network)
    * [<span data-ttu-id="66c3a-125">Brána firewall</span><span class="sxs-lookup"><span data-stu-id="66c3a-125">Firewall</span></span>](#firewall)
    * [<span data-ttu-id="66c3a-126">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="66c3a-126">Properties</span></span>](#properties)
    * [<span data-ttu-id="66c3a-127">Zámky.</span><span class="sxs-lookup"><span data-stu-id="66c3a-127">Locks</span></span>](#locks)
    * [<span data-ttu-id="66c3a-128">Skriptu pro automatizaci</span><span class="sxs-lookup"><span data-stu-id="66c3a-128">Automation script</span></span>](#automation-script)
* [<span data-ttu-id="66c3a-129">Správa</span><span class="sxs-lookup"><span data-stu-id="66c3a-129">Administration</span></span>](#administration)
    * [<span data-ttu-id="66c3a-130">Umožňuje importovat data</span><span class="sxs-lookup"><span data-stu-id="66c3a-130">Import data</span></span>](#importexport)
    * [<span data-ttu-id="66c3a-131">Export dat</span><span class="sxs-lookup"><span data-stu-id="66c3a-131">Export data</span></span>](#importexport)
    * [<span data-ttu-id="66c3a-132">Restartování</span><span class="sxs-lookup"><span data-stu-id="66c3a-132">Reboot</span></span>](#reboot)
* [<span data-ttu-id="66c3a-133">Monitorování</span><span class="sxs-lookup"><span data-stu-id="66c3a-133">Monitoring</span></span>](#monitoring)
    * [<span data-ttu-id="66c3a-134">Metriky pro redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-134">Redis metrics</span></span>](#redis-metrics)
    * [<span data-ttu-id="66c3a-135">Pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="66c3a-135">Alert rules</span></span>](#alert-rules)
    * [<span data-ttu-id="66c3a-136">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="66c3a-136">Diagnostics</span></span>](#diagnostics)
* [<span data-ttu-id="66c3a-137">Podporovat & řešení potíží s nastavení</span><span class="sxs-lookup"><span data-stu-id="66c3a-137">Support & troubleshooting settings</span></span>](#support-amp-troubleshooting-settings)
    * [<span data-ttu-id="66c3a-138">Stav prostředků</span><span class="sxs-lookup"><span data-stu-id="66c3a-138">Resource health</span></span>](#resource-health)
    * [<span data-ttu-id="66c3a-139">Nová žádost o podporu</span><span class="sxs-lookup"><span data-stu-id="66c3a-139">New support request</span></span>](#new-support-request)


## <a name="overview"></a><span data-ttu-id="66c3a-140">Přehled</span><span class="sxs-lookup"><span data-stu-id="66c3a-140">Overview</span></span>

<span data-ttu-id="66c3a-141">**Přehled** poskytuje k základní informace o mezipaměti, například název, porty, cenová úroveň a metriky vybrané mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-141">**Overview** provides you with basic information about your cache, such as name, ports, pricing tier, and selected cache metrics.</span></span>

### <a name="activity-log"></a><span data-ttu-id="66c3a-142">Protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="66c3a-142">Activity log</span></span>

<span data-ttu-id="66c3a-143">Klikněte na tlačítko **protokol aktivit** zobrazíte akcích provedených v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-143">Click **Activity log** to view actions performed on your cache.</span></span> <span data-ttu-id="66c3a-144">Můžete také pomocí filtrování rozbalte toto zobrazení zahrnout další prostředky.</span><span class="sxs-lookup"><span data-stu-id="66c3a-144">You can also use filtering to expand this view to include other resources.</span></span> <span data-ttu-id="66c3a-145">Další informace o práci s protokoly auditu najdete v tématu [auditovat operace s Resource Managerem](../azure-resource-manager/resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-145">For more information on working with audit logs, see [Audit operations with Resource Manager](../azure-resource-manager/resource-group-audit.md).</span></span> <span data-ttu-id="66c3a-146">Další informace o sledování událostí Azure Redis Cache najdete v tématu [operace a výstrahy](cache-how-to-monitor.md#operations-and-alerts).</span><span class="sxs-lookup"><span data-stu-id="66c3a-146">For more information on monitoring Azure Redis Cache events, see [Operations and alerts](cache-how-to-monitor.md#operations-and-alerts).</span></span>

### <a name="access-control-iam"></a><span data-ttu-id="66c3a-147">Řízení přístupu (IAM)</span><span class="sxs-lookup"><span data-stu-id="66c3a-147">Access control (IAM)</span></span>

<span data-ttu-id="66c3a-148">**Přístup k ovládacímu prvku (IAM)** části poskytuje podporu pro řízení přístupu na základě role (RBAC) na portálu Azure, které pomůžou organizacím, které splňují požadavky na správu jejich přístup, jednoduše a přesně.</span><span class="sxs-lookup"><span data-stu-id="66c3a-148">The **Access control (IAM)** section provides support for role-based access control (RBAC) in the Azure portal to help organizations meet their access management requirements simply and precisely.</span></span> <span data-ttu-id="66c3a-149">Další informace najdete v tématu [řízení přístupu na základě rolí na portálu Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-149">For more information, see [Role-based access control in the Azure portal](../active-directory/role-based-access-control-configure.md).</span></span>

### <a name="tags"></a><span data-ttu-id="66c3a-150">Značky</span><span class="sxs-lookup"><span data-stu-id="66c3a-150">Tags</span></span>

<span data-ttu-id="66c3a-151">**Značky** část umožňuje uspořádání prostředků.</span><span class="sxs-lookup"><span data-stu-id="66c3a-151">The **Tags** section helps you organize your resources.</span></span> <span data-ttu-id="66c3a-152">Další informace najdete v tématu [použití značek k uspořádání prostředků Azure](../azure-resource-manager/resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-152">For more information, see [Using tags to organize your Azure resources](../azure-resource-manager/resource-group-using-tags.md).</span></span>


### <a name="diagnose-and-solve-problems"></a><span data-ttu-id="66c3a-153">Diagnostikovat a řešit problémy</span><span class="sxs-lookup"><span data-stu-id="66c3a-153">Diagnose and solve problems</span></span>

<span data-ttu-id="66c3a-154">Klikněte na tlačítko **Diagnostikujte a řešení problémů** poskytované s běžné problémy a strategie pro jejich řešení.</span><span class="sxs-lookup"><span data-stu-id="66c3a-154">Click **Diagnose and solve problems** to be provided with common issues and strategies for resolving them.</span></span>



## <a name="settings"></a><span data-ttu-id="66c3a-155">Nastavení</span><span class="sxs-lookup"><span data-stu-id="66c3a-155">Settings</span></span>
<span data-ttu-id="66c3a-156">**Nastavení** část vám pomůže přístup a nakonfigurujte následující nastavení pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="66c3a-156">The **Settings** section allows you to access and configure the following settings for your cache.</span></span>

* [<span data-ttu-id="66c3a-157">Přístupové klávesy</span><span class="sxs-lookup"><span data-stu-id="66c3a-157">Access keys</span></span>](#access-keys)
* [<span data-ttu-id="66c3a-158">Upřesnit nastavení</span><span class="sxs-lookup"><span data-stu-id="66c3a-158">Advanced settings</span></span>](#advanced-settings)
* [<span data-ttu-id="66c3a-159">Advisor mezipaměti redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-159">Redis Cache Advisor</span></span>](#redis-cache-advisor)
* [<span data-ttu-id="66c3a-160">Škálování</span><span class="sxs-lookup"><span data-stu-id="66c3a-160">Scale</span></span>](#scale)
* [<span data-ttu-id="66c3a-161">Velikost clusteru redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-161">Redis cluster size</span></span>](#cluster-size)
* [<span data-ttu-id="66c3a-162">Trvalosti dat redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-162">Redis data persistence</span></span>](#redis-data-persistence)
* [<span data-ttu-id="66c3a-163">Plán aktualizací</span><span class="sxs-lookup"><span data-stu-id="66c3a-163">Schedule updates</span></span>](#schedule-updates)
* [<span data-ttu-id="66c3a-164">Geografická replikace</span><span class="sxs-lookup"><span data-stu-id="66c3a-164">Geo-replication</span></span>](#geo-replication)
* [<span data-ttu-id="66c3a-165">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="66c3a-165">Virtual Network</span></span>](#virtual-network)
* [<span data-ttu-id="66c3a-166">Brána firewall</span><span class="sxs-lookup"><span data-stu-id="66c3a-166">Firewall</span></span>](#firewall)
* [<span data-ttu-id="66c3a-167">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="66c3a-167">Properties</span></span>](#properties)
* [<span data-ttu-id="66c3a-168">Zámky.</span><span class="sxs-lookup"><span data-stu-id="66c3a-168">Locks</span></span>](#locks)
* [<span data-ttu-id="66c3a-169">Skriptu pro automatizaci</span><span class="sxs-lookup"><span data-stu-id="66c3a-169">Automation script</span></span>](#automation-script)



### <a name="access-keys"></a><span data-ttu-id="66c3a-170">Přístupové klíče</span><span class="sxs-lookup"><span data-stu-id="66c3a-170">Access keys</span></span>
<span data-ttu-id="66c3a-171">Klikněte na tlačítko **přístupové klíče** k zobrazení nebo opětovné vygenerování přístupových klíčů pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="66c3a-171">Click **Access keys** to view or regenerate the access keys for your cache.</span></span> <span data-ttu-id="66c3a-172">Tyto klíče používají připojení klientů k mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-172">These keys are used by the clients connecting to your cache.</span></span>

![Přístupové klíče mezipaměti redis](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a><span data-ttu-id="66c3a-174">Upřesnit nastavení</span><span class="sxs-lookup"><span data-stu-id="66c3a-174">Advanced settings</span></span>
<span data-ttu-id="66c3a-175">Následující nastavení se konfigurují na **upřesňující nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="66c3a-175">The following settings are configured on the **Advanced settings** blade.</span></span>

* [<span data-ttu-id="66c3a-176">Přístupové porty</span><span class="sxs-lookup"><span data-stu-id="66c3a-176">Access Ports</span></span>](#access-ports)
* [<span data-ttu-id="66c3a-177">Zásady paměti</span><span class="sxs-lookup"><span data-stu-id="66c3a-177">Memory policies</span></span>](#memory-policies)
* [<span data-ttu-id="66c3a-178">Oznámení keyspace (rozšířené nastavení)</span><span class="sxs-lookup"><span data-stu-id="66c3a-178">Keyspace notifications (advanced settings)</span></span>](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a><span data-ttu-id="66c3a-179">Přístupové porty</span><span class="sxs-lookup"><span data-stu-id="66c3a-179">Access Ports</span></span>
<span data-ttu-id="66c3a-180">Přístup bez SSL je ve výchozím nastavení pro nové mezipaměti zakázaný.</span><span class="sxs-lookup"><span data-stu-id="66c3a-180">By default, non-SSL access is disabled for new caches.</span></span> <span data-ttu-id="66c3a-181">Chcete-li povolit port bez SSL, klikněte na tlačítko **ne** pro **povolit přístup jen přes SSL** na **upřesňující nastavení** okna a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="66c3a-181">To enable the non-SSL port, click **No** for **Allow access only via SSL** on the **Advanced settings** blade and then click **Save**.</span></span>

![Přístupové porty mezipaměti redis](./media/cache-configure/redis-cache-access-ports.png)

<a name="maxmemory-policy-and-maxmemory-reserved"></a>
#### <a name="memory-policies"></a><span data-ttu-id="66c3a-183">Zásady paměti</span><span class="sxs-lookup"><span data-stu-id="66c3a-183">Memory policies</span></span>
<span data-ttu-id="66c3a-184">**Maxmemory zásad**, **vyhrazené maxmemory**, a **vyhrazené maxfragmentationmemory** nastavení na **upřesňující nastavení** okno nakonfigurovat zásady paměť pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="66c3a-184">The **Maxmemory policy**, **maxmemory-reserved**, and **maxfragmentationmemory-reserved** settings on the **Advanced settings** blade configure the memory policies for the cache.</span></span>

![Zásady Maxmemory mezipaměti redis](./media/cache-configure/redis-cache-maxmemory-policy.png)

<span data-ttu-id="66c3a-186">**Zásady Maxmemory** nakonfiguruje zásady vyřazení mezipaměti a umožňuje zvolit z následujících zásad vyřazení:</span><span class="sxs-lookup"><span data-stu-id="66c3a-186">**Maxmemory policy** configures the eviction policy for the cache and allows you to choose from the following eviction policies:</span></span>

* <span data-ttu-id="66c3a-187">`volatile-lru`-Toto je výchozí.</span><span class="sxs-lookup"><span data-stu-id="66c3a-187">`volatile-lru` - this is the default.</span></span>
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

<span data-ttu-id="66c3a-188">Další informace o `maxmemory` zásady, najdete v části [zásady vyřazení](http://redis.io/topics/lru-cache#eviction-policies).</span><span class="sxs-lookup"><span data-stu-id="66c3a-188">For more information about `maxmemory` policies, see [Eviction policies](http://redis.io/topics/lru-cache#eviction-policies).</span></span>

<span data-ttu-id="66c3a-189">**Maxmemory vyhrazené** nastavení konfiguruje množství paměti v MB, která je vyhrazena pro operace bez mezipaměti, jako je například replikace během převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="66c3a-189">The **maxmemory-reserved** setting configures the amount of memory in MB that is reserved for non-cache operations such as replication during failover.</span></span> <span data-ttu-id="66c3a-190">Nastavení této hodnoty můžete mít více konzistentního prostředí serveru Redis, pokud se liší podle zatížení.</span><span class="sxs-lookup"><span data-stu-id="66c3a-190">Setting this value allows you to have a more consistent Redis server experience when your load varies.</span></span> <span data-ttu-id="66c3a-191">Tato hodnota je třeba nastavit vyšší pro úlohy, které jsou zapsat náročné.</span><span class="sxs-lookup"><span data-stu-id="66c3a-191">This value should be set higher for workloads that are write heavy.</span></span> <span data-ttu-id="66c3a-192">Když paměti je vyhrazena pro takové operace, není k dispozici pro úložiště dat uložených v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-192">When memory is reserved for such operations, it is unavailable for storage of cached data.</span></span>

<span data-ttu-id="66c3a-193">**Maxfragmentationmemory vyhrazené** nastavení konfiguruje množství paměti v MB, která je vyhrazena pro přizpůsobení pro fragmentace paměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-193">The **maxfragmentationmemory-reserved** setting configures the amount of memory in MB that is reserved to accommodate for memory fragmentation.</span></span> <span data-ttu-id="66c3a-194">Nastavení této hodnoty můžete mít více konzistentní serveru Redis dojít, pokud je mezipaměť plná nebo blízko úplné a fragmentaci poměr je také vysoká.</span><span class="sxs-lookup"><span data-stu-id="66c3a-194">Setting this value allows you to have a more consistent Redis server experience when the cache is full or close to full and the fragmentation ratio is also high.</span></span> <span data-ttu-id="66c3a-195">Když paměti je vyhrazena pro takové operace, není k dispozici pro úložiště dat uložených v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-195">When memory is reserved for such operations, it is unavailable for storage of cached data.</span></span>

<span data-ttu-id="66c3a-196">Jednou z věcí vzít v úvahu při výběru novou hodnotu rezervace paměti (**vyhrazené maxmemory** nebo **vyhrazené maxfragmentationmemory**) je, jak tato změna může ovlivnit mezipaměti, která je již spuštěn s velké objemy dat v něm.</span><span class="sxs-lookup"><span data-stu-id="66c3a-196">One thing to consider when choosing a new memory reservation value (**maxmemory-reserved** or **maxfragmentationmemory-reserved**) is how this change might affect a cache that is already running with large amounts of data in it.</span></span> <span data-ttu-id="66c3a-197">Například pokud jste 53 GB mezipaměti s 49 GB dat, pak změňte hodnotu rezervace na 8 GB, bude vyřadit maximální dostupná paměť systému dolů 45 GB.</span><span class="sxs-lookup"><span data-stu-id="66c3a-197">For instance, if you have a 53 GB cache with 49 GB of data, then change the reservation value to 8 GB, this will drop the max available memory for the system down to 45 GB.</span></span> <span data-ttu-id="66c3a-198">Pokud buď vaše aktuální `used_memory` , vaše `used_memory_rss` hodnoty jsou vyšší, než nový limit 45 GB a pak systému bude mít vyřazení dat dokud `used_memory` a `used_memory_rss` jsou pod 45 GB.</span><span class="sxs-lookup"><span data-stu-id="66c3a-198">If either your current `used_memory` or your `used_memory_rss` values are higher than the new limit of 45 GB, then the system will have to evict data until both `used_memory` and `used_memory_rss` are below 45 GB.</span></span> <span data-ttu-id="66c3a-199">Vyřazení může zvýšit fragmentace zatížení a paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="66c3a-199">Eviction can increase server load and memory fragmentation.</span></span> <span data-ttu-id="66c3a-200">Další informace o metrikách mezipaměti jako `used_memory` a `used_memory_rss`, najdete v části [k dostupným metrikám a vytváření sestav intervaly](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).</span><span class="sxs-lookup"><span data-stu-id="66c3a-200">For more information on cache metrics such as `used_memory` and `used_memory_rss`, see [Available metrics and reporting intervals](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="66c3a-201">**Vyhrazené maxmemory** a **maxfragmentationmemory vyhrazené** nastavení jsou dostupná jenom pro Standard a Premium ukládá do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-201">The **maxmemory-reserved** and **maxfragmentationmemory-reserved** settings are only available for Standard and Premium caches.</span></span>
> 
> 

#### <a name="keyspace-notifications-advanced-settings"></a><span data-ttu-id="66c3a-202">Oznámení keyspace (rozšířené nastavení)</span><span class="sxs-lookup"><span data-stu-id="66c3a-202">Keyspace notifications (advanced settings)</span></span>
<span data-ttu-id="66c3a-203">Redis keyspace oznámení se konfigurují na **upřesňující nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="66c3a-203">Redis keyspace notifications are configured on the **Advanced settings** blade.</span></span> <span data-ttu-id="66c3a-204">Oznámení keyspace umožňují klientům přijímat upozornění, když dojde k určitým událostem.</span><span class="sxs-lookup"><span data-stu-id="66c3a-204">Keyspace notifications allow clients to receive notifications when certain events occur.</span></span>

![Upřesňující nastavení mezipaměti redis](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> <span data-ttu-id="66c3a-206">Oznámení keyspace a **oznámení události keyspace** nastavení jsou dostupná jenom pro Cache úrovní Standard a Premium.</span><span class="sxs-lookup"><span data-stu-id="66c3a-206">Keyspace notifications and the **notify-keyspace-events** setting are only available for Standard and Premium caches.</span></span>
> 
> 

<span data-ttu-id="66c3a-207">Další informace najdete v tématu [Redis oznámení Keyspace](http://redis.io/topics/notifications).</span><span class="sxs-lookup"><span data-stu-id="66c3a-207">For more information, see [Redis Keyspace Notifications](http://redis.io/topics/notifications).</span></span> <span data-ttu-id="66c3a-208">Ukázkový kód, najdete v článku [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) v soubor [Hello, world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) ukázka.</span><span class="sxs-lookup"><span data-stu-id="66c3a-208">For sample code, see the [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) file in the [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) sample.</span></span>


<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a><span data-ttu-id="66c3a-209">Advisor mezipaměti redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-209">Redis Cache Advisor</span></span>
<span data-ttu-id="66c3a-210">**Redis Cache Advisor** zobrazuje doporučení pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="66c3a-210">The **Redis Cache Advisor** blade displays recommendations for your cache.</span></span> <span data-ttu-id="66c3a-211">Během normálních operací zobrazí se žádná doporučení.</span><span class="sxs-lookup"><span data-stu-id="66c3a-211">During normal operations, no recommendations are displayed.</span></span> 

![Doporučení](./media/cache-configure/redis-cache-no-recommendations.png)

<span data-ttu-id="66c3a-213">Pokud jakékoliv podmínky, ke kterým došlo během operace svojí mezipaměti, jako je například velké množství paměti, šířky pásma sítě nebo zatížení serveru, upozornění se zobrazí na **Redis Cache** okno.</span><span class="sxs-lookup"><span data-stu-id="66c3a-213">If any conditions occur during the operations of your cache such as high memory usage, network bandwidth, or server load, an alert is displayed on the **Redis Cache** blade.</span></span>

![Doporučení](./media/cache-configure/redis-cache-recommendations-alert.png)

<span data-ttu-id="66c3a-215">Další informace naleznete na **doporučení** okno.</span><span class="sxs-lookup"><span data-stu-id="66c3a-215">Further information can be found on the **Recommendations** blade.</span></span>

![Doporučení](./media/cache-configure/redis-cache-recommendations.png)

<span data-ttu-id="66c3a-217">Tyto metriky můžete sledovat na [tabulek sledování](cache-how-to-monitor.md#monitoring-charts) a [využití grafy](cache-how-to-monitor.md#usage-charts) části **Redis Cache** okno.</span><span class="sxs-lookup"><span data-stu-id="66c3a-217">You can monitor these metrics on the [Monitoring charts](cache-how-to-monitor.md#monitoring-charts) and [Usage charts](cache-how-to-monitor.md#usage-charts) sections of the **Redis Cache** blade.</span></span>

<span data-ttu-id="66c3a-218">Každá cenová úroveň má jiné limity pro připojení klientů, paměti a šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="66c3a-218">Each pricing tier has different limits for client connections, memory, and bandwidth.</span></span> <span data-ttu-id="66c3a-219">Pokud vaše mezipaměť blíží maximální kapacity pro tyto metriky po dobu delší dobu, vytvoří se doporučení.</span><span class="sxs-lookup"><span data-stu-id="66c3a-219">If your cache approaches maximum capacity for these metrics over a sustained period of time, a recommendation is created.</span></span> <span data-ttu-id="66c3a-220">Další informace o metriky a omezení zkontrolovány uživatelem **doporučení** nástroj, najdete v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="66c3a-220">For more information about the metrics and limits reviewed by the **Recommendations** tool, see the following table:</span></span>

| <span data-ttu-id="66c3a-221">Metrika mezipaměti redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-221">Redis Cache metric</span></span> | <span data-ttu-id="66c3a-222">Další informace</span><span class="sxs-lookup"><span data-stu-id="66c3a-222">More information</span></span> |
| --- | --- |
| <span data-ttu-id="66c3a-223">Využití šířky pásma sítě</span><span class="sxs-lookup"><span data-stu-id="66c3a-223">Network bandwidth usage</span></span> |[<span data-ttu-id="66c3a-224">Výkon mezipaměti - dostupnou šířku pásma</span><span class="sxs-lookup"><span data-stu-id="66c3a-224">Cache performance - available bandwidth</span></span>](cache-faq.md#cache-performance) |
| <span data-ttu-id="66c3a-225">Připojení klienti</span><span class="sxs-lookup"><span data-stu-id="66c3a-225">Connected clients</span></span> |[<span data-ttu-id="66c3a-226">Výchozí konfigurace serveru Redis - maxclients</span><span class="sxs-lookup"><span data-stu-id="66c3a-226">Default Redis server configuration - maxclients</span></span>](#maxclients) |
| <span data-ttu-id="66c3a-227">Zatížení serveru</span><span class="sxs-lookup"><span data-stu-id="66c3a-227">Server load</span></span> |[<span data-ttu-id="66c3a-228">Použití grafů – zatížení serveru Redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-228">Usage charts - Redis Server Load</span></span>](cache-how-to-monitor.md#usage-charts) |
| <span data-ttu-id="66c3a-229">Využití paměti</span><span class="sxs-lookup"><span data-stu-id="66c3a-229">Memory usage</span></span> |[<span data-ttu-id="66c3a-230">Výkon mezipaměti - velikost</span><span class="sxs-lookup"><span data-stu-id="66c3a-230">Cache performance - size</span></span>](cache-faq.md#cache-performance) |

<span data-ttu-id="66c3a-231">Chcete-li upgradovat mezipaměti, klikněte na tlačítko **upgradovat nyní** změnit cenovou úroveň a [škálování](#scale) svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-231">To upgrade your cache, click **Upgrade now** to change the pricing tier and [scale](#scale) your cache.</span></span> <span data-ttu-id="66c3a-232">Další informace o výběru cenovou úroveň, najdete v části [jaké mezipaměť Redis nabídky a velikosti použít?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)</span><span class="sxs-lookup"><span data-stu-id="66c3a-232">For more information on choosing a pricing tier, see [What Redis Cache offering and size should I use?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)</span></span>


### <a name="scale"></a><span data-ttu-id="66c3a-233">Měřítko</span><span class="sxs-lookup"><span data-stu-id="66c3a-233">Scale</span></span>
<span data-ttu-id="66c3a-234">Klikněte na tlačítko **škálování** chcete zobrazit nebo změnit jeho cenovou úroveň mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-234">Click **Scale** to view or change the pricing tier for your cache.</span></span> <span data-ttu-id="66c3a-235">Další informace o škálování najdete v tématu [postup škálování Azure Redis Cache](cache-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-235">For more information on scaling, see [How to Scale Azure Redis Cache](cache-how-to-scale.md).</span></span>

![Cenová úroveň mezipaměti redis](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a><span data-ttu-id="66c3a-237">Velikost clusteru redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-237">Redis Cluster Size</span></span>
<span data-ttu-id="66c3a-238">Klikněte na tlačítko **velikost clusteru Redis (PREVIEW)** Chcete-li změnit velikost clusteru pro spuštěný premium mezipaměti s povoleným clusteringem.</span><span class="sxs-lookup"><span data-stu-id="66c3a-238">Click **(PREVIEW) Redis Cluster Size** to change the cluster size for a running premium cache with clustering enabled.</span></span>

> [!NOTE]
> <span data-ttu-id="66c3a-239">Všimněte si, že při obecné dostupnosti bylo vydáno Azure Redis Cache na úrovni Premium, velikost clusteru Redis funkce je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="66c3a-239">Note that while the Azure Redis Cache Premium tier has been released to General Availability, the Redis Cluster Size feature is currently in preview.</span></span>
> 
> 

![Velikost clusteru redis](./media/cache-configure/redis-cache-redis-cluster-size.png)

<span data-ttu-id="66c3a-241">Chcete-li změnit velikost clusteru, posuvníkem nebo zadejte číslo mezi 1 a 10 v **počet horizontálních** textového pole a klikněte na tlačítko **OK** uložit.</span><span class="sxs-lookup"><span data-stu-id="66c3a-241">To change the cluster size, use the slider or type a number between 1 and 10 in the **Shard count** text box and click **OK** to save.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="66c3a-242">Redis clustering je dostupná jenom pro prémiových mezipamětí.</span><span class="sxs-lookup"><span data-stu-id="66c3a-242">Redis clustering is only available for Premium caches.</span></span> <span data-ttu-id="66c3a-243">Další informace najdete v článku [Postup konfigurace clusterů pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-clustering.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-243">For more information, see [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md).</span></span>
> 
> 


### <a name="redis-data-persistence"></a><span data-ttu-id="66c3a-244">Trvalosti dat redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-244">Redis data persistence</span></span>
<span data-ttu-id="66c3a-245">Klikněte na tlačítko **trvalosti dat Redis** povolit, zakázat nebo konfigurace trvalosti dat pro mezipaměť premium.</span><span class="sxs-lookup"><span data-stu-id="66c3a-245">Click **Redis data persistence** to enable, disable, or configure data persistence for your premium cache.</span></span> <span data-ttu-id="66c3a-246">Azure Redis Cache nabízí buď pomocí trvalosti Redis [RDB trvalost](cache-how-to-premium-persistence.md#configure-rdb-persistence) nebo [AOF trvalost](cache-how-to-premium-persistence.md#configure-aof-persistence).</span><span class="sxs-lookup"><span data-stu-id="66c3a-246">Azure Redis Cache offers Redis persistence using either [RDB persistence](cache-how-to-premium-persistence.md#configure-rdb-persistence) or [AOF persistence](cache-how-to-premium-persistence.md#configure-aof-persistence).</span></span>

<span data-ttu-id="66c3a-247">Další informace najdete v tématu [postup konfigurace trvalosti pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-persistence.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-247">For more information, see [How to configure persistence for a Premium Azure Redis Cache](cache-how-to-premium-persistence.md).</span></span>


> [!IMPORTANT]
> <span data-ttu-id="66c3a-248">Trvalosti dat redis je dostupná jenom pro prémiových mezipamětí.</span><span class="sxs-lookup"><span data-stu-id="66c3a-248">Redis data persistence is only available for Premium caches.</span></span> 
> 
> 

### <a name="schedule-updates"></a><span data-ttu-id="66c3a-249">Aktualizace plánu</span><span class="sxs-lookup"><span data-stu-id="66c3a-249">Schedule updates</span></span>
<span data-ttu-id="66c3a-250">**Naplánovat aktualizace** okno umožňuje určit časové období údržby pro aktualizace serveru Redis ke svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-250">The **Schedule updates** blade allows you to designate a maintenance window for Redis server updates for your cache.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="66c3a-251">Pro správu a údržbu platí jenom pro Redis serveru aktualizací a tak, aby všechny Azure aktualizace nebo aktualizace operačního systému virtuálních počítačů, které jsou hostiteli mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-251">The maintenance window applies only to Redis server updates, and not to any Azure updates or updates to the operating system of the VMs that host the cache.</span></span>
> 
> 

![Aktualizace plánu](./media/cache-configure/redis-schedule-updates.png)

<span data-ttu-id="66c3a-253">Zadejte časové období údržby, zkontrolujte požadované dny a zadejte hodina spouštění údržby okna pro každý den a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="66c3a-253">To specify a maintenance window, check the desired days and specify the maintenance window start hour for each day, and click **OK**.</span></span> <span data-ttu-id="66c3a-254">Všimněte si, že časového období údržby se ve standardu UTC.</span><span class="sxs-lookup"><span data-stu-id="66c3a-254">Note that the maintenance window time is in UTC.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="66c3a-255">**Naplánovat aktualizace** funkce je dostupná jenom pro prémiových mezipamětí vrstvy.</span><span class="sxs-lookup"><span data-stu-id="66c3a-255">The **Schedule updates** functionality is only available for Premium tier caches.</span></span> <span data-ttu-id="66c3a-256">Další informace a pokyny najdete v tématu [správy Azure Redis Cache - naplánovat aktualizace](cache-administration.md#schedule-updates).</span><span class="sxs-lookup"><span data-stu-id="66c3a-256">For more information and instructions, see [Azure Redis Cache administration - Schedule updates](cache-administration.md#schedule-updates).</span></span>
> 
> 

### <a name="geo-replication"></a><span data-ttu-id="66c3a-257">Geografická replikace</span><span class="sxs-lookup"><span data-stu-id="66c3a-257">Geo-replication</span></span>

<span data-ttu-id="66c3a-258">**Geografická replikace** okno poskytuje mechanismus pro propojení dvě instance vrstvy Azure Redis Cache Premium.</span><span class="sxs-lookup"><span data-stu-id="66c3a-258">The **Geo-replication** blade provides a mechanism for linking two Premium tier Azure Redis Cache instances.</span></span> <span data-ttu-id="66c3a-259">Jeden mezipaměti je určený jako primární propojené mezipaměti a druhý jako sekundární propojené mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-259">One cache is designated as the primary linked cache, and the other as the secondary linked cache.</span></span> <span data-ttu-id="66c3a-260">Sekundární propojené mezipaměti se stane, jen pro čtení a data zapsat do mezipaměti. primární se replikují do sekundární propojené mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-260">The secondary linked cache becomes read-only, and data written to the primary cache is replicated to the secondary linked cache.</span></span> <span data-ttu-id="66c3a-261">Tato funkce slouží k replikaci mezipaměti mezi oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="66c3a-261">This functionality can be used to replicate a cache across Azure regions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="66c3a-262">**Geografická replikace** je dostupná jenom u prémiových mezipamětí vrstvy.</span><span class="sxs-lookup"><span data-stu-id="66c3a-262">**Geo-replication** is only available for Premium tier caches.</span></span> <span data-ttu-id="66c3a-263">Další informace a pokyny najdete v tématu [konfiguraci geografická replikace pro Azure Redis Cache](cache-how-to-geo-replication.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-263">For more information and instructions, see [How to configure Geo-replication for Azure Redis Cache](cache-how-to-geo-replication.md).</span></span>
> 
> 

### <a name="virtual-network"></a><span data-ttu-id="66c3a-264">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="66c3a-264">Virtual Network</span></span>
<span data-ttu-id="66c3a-265">**Virtuální sítě** část vám pomůže nakonfigurovat nastavení virtuální sítě pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="66c3a-265">The **Virtual Network** section allows you to configure the virtual network settings for your cache.</span></span> <span data-ttu-id="66c3a-266">Informace týkající se vytváření cache ve verzi premium se virtuální síť podporovat a aktualizuje jeho nastavení, najdete v tématu [konfiguraci virtuální sítě podporu pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-266">For information on creating a premium cache with VNET support and updating its settings, see [How to configure Virtual Network Support for a Premium Azure Redis Cache](cache-how-to-premium-vnet.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="66c3a-267">Nastavení virtuální sítě jsou dostupné pouze pro prémiových mezipamětí, které byly nakonfigurovány s podporou virtuální sítě během vytváření mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-267">Virtual network settings are only available for premium caches that were configured with VNET support during cache creation.</span></span> 
> 
> 

### <a name="firewall"></a><span data-ttu-id="66c3a-268">Brána firewall</span><span class="sxs-lookup"><span data-stu-id="66c3a-268">Firewall</span></span>

<span data-ttu-id="66c3a-269">Klikněte na tlačítko **brány Firewall** k zobrazení a konfigurace pravidel brány firewall pro vaše mezipaměť Azure Redis Cache Premium.</span><span class="sxs-lookup"><span data-stu-id="66c3a-269">Click **Firewall** to view and configure firewall rules for your Premium Azure Redis Cache.</span></span>

![Brána firewall](./media/cache-configure/redis-firewall-rules.png)

<span data-ttu-id="66c3a-271">Pravidla brány firewall můžete zadat s počáteční a koncové rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="66c3a-271">You can specify firewall rules with a start and end IP address range.</span></span> <span data-ttu-id="66c3a-272">Pokud jsou nakonfigurovaná pravidla brány firewall, připojení klienta pouze z zadaných rozsazích IP adres může připojit k mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-272">When firewall rules are configured, only client connections from the specified IP address ranges can connect to the cache.</span></span> <span data-ttu-id="66c3a-273">Při uložení pravidlo brány firewall není chvíli trvat, než pravidlo je platná.</span><span class="sxs-lookup"><span data-stu-id="66c3a-273">When a firewall rule is saved there is a short delay before the rule is effective.</span></span> <span data-ttu-id="66c3a-274">Toto opoždění je obvykle méně než jedna minuta.</span><span class="sxs-lookup"><span data-stu-id="66c3a-274">This delay is typically less than one minute.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="66c3a-275">Připojení z Azure Redis Cache monitorování systémů jsou vždy povoleny, i když jsou nakonfigurovaná pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="66c3a-275">Connections from Azure Redis Cache monitoring systems are always permitted, even if firewall rules are configured.</span></span>
> 
> <span data-ttu-id="66c3a-276">Pravidla brány firewall jsou dostupná jenom pro prémiových mezipamětí vrstvy.</span><span class="sxs-lookup"><span data-stu-id="66c3a-276">Firewall rules are only available for Premium tier caches.</span></span>
> 
> 

### <a name="properties"></a><span data-ttu-id="66c3a-277">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="66c3a-277">Properties</span></span>
<span data-ttu-id="66c3a-278">Klikněte na tlačítko **vlastnosti** zobrazíte informace o mezipaměti, včetně koncový bod mezipaměti a portů.</span><span class="sxs-lookup"><span data-stu-id="66c3a-278">Click **Properties** to view information about your cache, including the cache endpoint and ports.</span></span>

![Vlastnosti mezipaměti redis](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a><span data-ttu-id="66c3a-280">Zámky</span><span class="sxs-lookup"><span data-stu-id="66c3a-280">Locks</span></span>
<span data-ttu-id="66c3a-281">**Zamkne** části umožňuje zamknout předplatné, skupinu prostředků nebo prostředek zabránit ostatním uživatelům ve vaší organizaci neúmyslnému odstranění nebo úprava důležitých prostředků.</span><span class="sxs-lookup"><span data-stu-id="66c3a-281">The **Locks** section allows you to lock a subscription, resource group, or resource to prevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="66c3a-282">Další informace najdete v tématu [Zamknutí prostředků pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-282">For more information, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

### <a name="automation-script"></a><span data-ttu-id="66c3a-283">Skriptu pro automatizaci</span><span class="sxs-lookup"><span data-stu-id="66c3a-283">Automation script</span></span>

<span data-ttu-id="66c3a-284">Klikněte na tlačítko **skriptu pro automatizaci** sestavení a exportovat šablonu vaše nasazené prostředky pro budoucí nasazení.</span><span class="sxs-lookup"><span data-stu-id="66c3a-284">Click **Automation script** to build and export a template of your deployed resources for future deployments.</span></span> <span data-ttu-id="66c3a-285">Další informace o práci se šablonami najdete v tématu [nasazení prostředků pomocí šablony Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-285">For more information about working with templates, see [Deploy resources with Azure Resource Manager templates](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

## <a name="administration-settings"></a><span data-ttu-id="66c3a-286">Nastavení správy</span><span class="sxs-lookup"><span data-stu-id="66c3a-286">Administration settings</span></span>
<span data-ttu-id="66c3a-287">Nastavení v **správy** části umožňují provádět následující úlohy správy pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="66c3a-287">The settings in the **Administration** section allow you to perform the following administrative tasks for your cache.</span></span> 

![Správa](./media/cache-configure/redis-cache-administration.png)

* [<span data-ttu-id="66c3a-289">Umožňuje importovat data</span><span class="sxs-lookup"><span data-stu-id="66c3a-289">Import data</span></span>](#importexport)
* [<span data-ttu-id="66c3a-290">Export dat</span><span class="sxs-lookup"><span data-stu-id="66c3a-290">Export data</span></span>](#importexport)
* [<span data-ttu-id="66c3a-291">Restartování</span><span class="sxs-lookup"><span data-stu-id="66c3a-291">Reboot</span></span>](#reboot)


### <a name="importexport"></a><span data-ttu-id="66c3a-292">Import/export</span><span class="sxs-lookup"><span data-stu-id="66c3a-292">Import/Export</span></span>
<span data-ttu-id="66c3a-293">Import a Export je operace Azure Redis Cache dat správy, který umožňuje importovat a exportovat data v mezipaměti import a Export snímku databáze Redis Cache (RDB) z mezipaměti premium do objektů blob stránky v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="66c3a-293">Import/Export is an Azure Redis Cache data management operation, which allows you to import and export data in the cache by importing and exporting a Redis Cache Database (RDB) snapshot from a premium cache to a page blob in an Azure Storage Account.</span></span> <span data-ttu-id="66c3a-294">Import a Export umožňuje migraci mezi různými instancemi Azure Redis Cache nebo naplnění mezipaměti s daty před použitím.</span><span class="sxs-lookup"><span data-stu-id="66c3a-294">Import/Export enables you to migrate between different Azure Redis Cache instances or populate the cache with data before use.</span></span>

<span data-ttu-id="66c3a-295">Import umožňuje přinést si Redis kompatibilní RDB soubory z jakéhokoli Redis serveru se systémem v libovolném cloudu nebo prostředí, včetně Redis systémem Linux, Windows nebo kteréhokoli poskytovatele cloudových služeb jako Amazon Web Services a dalších.</span><span class="sxs-lookup"><span data-stu-id="66c3a-295">Import can be used to bring Redis compatible RDB files from any Redis server running in any cloud or environment, including Redis running on Linux, Windows, or any cloud provider such as Amazon Web Services and others.</span></span> <span data-ttu-id="66c3a-296">Import dat je snadný způsob, jak vytvořit mezipaměti s předem vyplněná data.</span><span class="sxs-lookup"><span data-stu-id="66c3a-296">Importing data is an easy way to create a cache with pre-populated data.</span></span> <span data-ttu-id="66c3a-297">Během procesu importu Azure Redis Cache načte RDB soubory z úložiště Azure do paměti a vloží klíčů do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-297">During the import process, Azure Redis Cache loads the RDB files from Azure storage into memory, and then inserts the keys into the cache.</span></span>

<span data-ttu-id="66c3a-298">Export umožňuje exportovat data uložená ve službě Azure Redis Cache k Redis kompatibilní soubory RDB.</span><span class="sxs-lookup"><span data-stu-id="66c3a-298">Export allows you to export the data stored in Azure Redis Cache to Redis compatible RDB files.</span></span> <span data-ttu-id="66c3a-299">Tato funkce slouží pro přesun dat z jedné instance Azure Redis Cache do jiné nebo jinému serveru Redis.</span><span class="sxs-lookup"><span data-stu-id="66c3a-299">You can use this feature to move data from one Azure Redis Cache instance to another or to another Redis server.</span></span> <span data-ttu-id="66c3a-300">Během procesu exportu ve virtuálním počítači, který je hostitelem instance serveru Azure Redis Cache je vytvořit dočasný soubor a soubor je odeslán k účtu úložiště určený.</span><span class="sxs-lookup"><span data-stu-id="66c3a-300">During the export process, a temporary file is created on the VM that hosts the Azure Redis Cache server instance, and the file is uploaded to the designated storage account.</span></span> <span data-ttu-id="66c3a-301">Po dokončení operace exportu se stavem úspěch nebo selhání dočasný soubor bude odstraněn.</span><span class="sxs-lookup"><span data-stu-id="66c3a-301">When the export operation completes with either a status of success or failure, the temporary file is deleted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="66c3a-302">Import a Export je dostupná jenom pro prémiových mezipamětí vrstvy.</span><span class="sxs-lookup"><span data-stu-id="66c3a-302">Import/Export is only available for Premium tier caches.</span></span> <span data-ttu-id="66c3a-303">Další informace a pokyny najdete v tématu [importu a exportu dat ve službě Azure Redis Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-303">For more information and instructions, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

### <a name="reboot"></a><span data-ttu-id="66c3a-304">Restartování</span><span class="sxs-lookup"><span data-stu-id="66c3a-304">Reboot</span></span>
<span data-ttu-id="66c3a-305">**Restartovat** okno umožňuje restartovat uzly mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-305">The **Reboot** blade allows you to reboot the nodes of your cache.</span></span> <span data-ttu-id="66c3a-306">Tato možnost restartování umožňuje otestovat aplikaci pro odolnost proti chybám, pokud dojde k selhání uzlu mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-306">This reboot capability enables you to test your application for resiliency if there is a failure of a cache node.</span></span>

![Restartování](./media/cache-configure/redis-cache-reboot.png)

<span data-ttu-id="66c3a-308">Pokud máte s povoleným clusteringem cache ve verzi premium, můžete vybrat které horizontálních oddílů mezipaměti restartovat.</span><span class="sxs-lookup"><span data-stu-id="66c3a-308">If you have a premium cache with clustering enabled, you can select which shards of the cache to reboot.</span></span>

![Restartování](./media/cache-configure/redis-cache-reboot-cluster.png)

<span data-ttu-id="66c3a-310">Restartovat jeden nebo více uzlů svojí mezipaměti, vyberte požadovaný uzel a klikněte na tlačítko **restartovat**.</span><span class="sxs-lookup"><span data-stu-id="66c3a-310">To reboot one or more nodes of your cache, select the desired nodes and click **Reboot**.</span></span> <span data-ttu-id="66c3a-311">Pokud máte cache ve verzi premium s povoleným clusteringem, vyberte shard(s) restartovat a pak klikněte na tlačítko **restartovat**.</span><span class="sxs-lookup"><span data-stu-id="66c3a-311">If you have a premium cache with clustering enabled, select the shard(s) to reboot and then click **Reboot**.</span></span> <span data-ttu-id="66c3a-312">Po několika minutách restartování vybrané uzly a jsou zpět do režimu online několik minut později.</span><span class="sxs-lookup"><span data-stu-id="66c3a-312">After a few minutes, the selected node(s) reboot, and are back online a few minutes later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="66c3a-313">Restart je nyní k dispozici pro všechny cenové úrovně.</span><span class="sxs-lookup"><span data-stu-id="66c3a-313">Reboot is now available for all pricing tiers.</span></span> <span data-ttu-id="66c3a-314">Další informace a pokyny najdete v tématu [správy Azure Redis Cache - restartování](cache-administration.md#reboot).</span><span class="sxs-lookup"><span data-stu-id="66c3a-314">For more information and instructions, see [Azure Redis Cache administration - Reboot](cache-administration.md#reboot).</span></span>
> 
> 


## <a name="monitoring"></a><span data-ttu-id="66c3a-315">Monitorování</span><span class="sxs-lookup"><span data-stu-id="66c3a-315">Monitoring</span></span>

<span data-ttu-id="66c3a-316">**Monitorování** část vám pomůže nakonfigurovat monitorování pro mezipaměť Redis Cache a diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="66c3a-316">The **Monitoring** section allows you to configure diagnostics and monitoring for your Redis Cache.</span></span> <span data-ttu-id="66c3a-317">Další informace o Azure Redis Cache monitorování a Diagnostika, najdete v části [jak monitorovat Azure Redis Cache](cache-how-to-monitor.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-317">For more information on Azure Redis Cache monitoring and diagnostics, see [How to monitor Azure Redis Cache](cache-how-to-monitor.md).</span></span>

![Diagnostika](./media/cache-configure/redis-cache-diagnostics.png)

* [<span data-ttu-id="66c3a-319">Metriky pro redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-319">Redis metrics</span></span>](#redis-metrics)
* [<span data-ttu-id="66c3a-320">Pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="66c3a-320">Alert rules</span></span>](#alert-rules)
* [<span data-ttu-id="66c3a-321">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="66c3a-321">Diagnostics</span></span>](#diagnostics)

### <a name="redis-metrics"></a><span data-ttu-id="66c3a-322">Metriky pro redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-322">Redis metrics</span></span>
<span data-ttu-id="66c3a-323">Klikněte na tlačítko **Redis metriky** k [metriky zobrazit](cache-how-to-monitor.md#view-cache-metrics) ke svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-323">Click **Redis metrics** to [view metrics](cache-how-to-monitor.md#view-cache-metrics) for your cache.</span></span>

### <a name="alert-rules"></a><span data-ttu-id="66c3a-324">Pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="66c3a-324">Alert rules</span></span>

<span data-ttu-id="66c3a-325">Klikněte na tlačítko **výstrah pravidla** můžete konfigurovat upozornění na základě metriky pro Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="66c3a-325">Click **Alert rules** to configure alerts based on Redis Cache metrics.</span></span> <span data-ttu-id="66c3a-326">Další informace najdete v tématu [výstrahy](cache-how-to-monitor.md#alerts).</span><span class="sxs-lookup"><span data-stu-id="66c3a-326">For more information, see [Alerts](cache-how-to-monitor.md#alerts).</span></span>

### <a name="diagnostics"></a><span data-ttu-id="66c3a-327">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="66c3a-327">Diagnostics</span></span>

<span data-ttu-id="66c3a-328">Ve výchozím nastavení, mezipaměti metriky v Azure monitorování jsou [uchovávají po dobu 30 dnů](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) a odstraní.</span><span class="sxs-lookup"><span data-stu-id="66c3a-328">By default, cache metrics in Azure Monitor are [stored for 30 days](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) and then deleted.</span></span> <span data-ttu-id="66c3a-329">Chcete-li zachovat metriky vaší mezipaměti po dobu delší než 30 dní, klikněte na tlačítko **diagnostiky** k [konfigurace účtu úložiště](cache-how-to-monitor.md#export-cache-metrics) používá k ukládání diagnostiku mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-329">To persist your cache metrics for longer than 30 days, click **Diagnostics** to [configure the storage account](cache-how-to-monitor.md#export-cache-metrics) used to store cache diagnostics.</span></span>

>[!NOTE]
><span data-ttu-id="66c3a-330">Kromě archivace vaše mezipaměť metriky pro úložiště, můžete také [stream je do centra událostí nebo poslat k analýze protokolů](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).</span><span class="sxs-lookup"><span data-stu-id="66c3a-330">In addition to archiving your cache metrics to storage, you can also [stream them to an Event hub or send them to Log Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).</span></span>
>
>

## <a name="support--troubleshooting-settings"></a><span data-ttu-id="66c3a-331">Podporovat & řešení potíží s nastavení</span><span class="sxs-lookup"><span data-stu-id="66c3a-331">Support & troubleshooting settings</span></span>
<span data-ttu-id="66c3a-332">Nastavení v **podpory a řešení potíží s** části poskytují možnosti pro řešení problémů s mezipamětí.</span><span class="sxs-lookup"><span data-stu-id="66c3a-332">The settings in the **Support + troubleshooting** section provide you with options for resolving issues with your cache.</span></span>

![Podpora + řešení potíží](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [<span data-ttu-id="66c3a-334">Stav prostředků</span><span class="sxs-lookup"><span data-stu-id="66c3a-334">Resource health</span></span>](#resource-health)
* [<span data-ttu-id="66c3a-335">Nová žádost o podporu</span><span class="sxs-lookup"><span data-stu-id="66c3a-335">New support request</span></span>](#new-support-request)

### <a name="resource-health"></a><span data-ttu-id="66c3a-336">Stav prostředků</span><span class="sxs-lookup"><span data-stu-id="66c3a-336">Resource health</span></span>
<span data-ttu-id="66c3a-337">**Stav prostředku** sleduje prostředku a zjistíte, zda je spuštěn podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="66c3a-337">**Resource health** watches your resource and tells you if it's running as expected.</span></span> <span data-ttu-id="66c3a-338">Další informace o službě stavu prostředků Azure najdete v tématu [přehled stavu prostředků Azure](../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-338">For more information about the Azure Resource health service, see [Azure Resource health overview](../resource-health/resource-health-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="66c3a-339">Stav prostředku je momentálně nelze vytvořit sestavu stavu instance služby Azure Redis Cache hostované ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-339">Resource health is currently unable to report on the health of Azure Redis Cache instances hosted in a virtual network.</span></span> <span data-ttu-id="66c3a-340">Další informace najdete v tématu [všechny funkce mezipaměti fungují při hostování mezipaměti ve virtuální síti?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)</span><span class="sxs-lookup"><span data-stu-id="66c3a-340">For more information, see [Do all cache features work when hosting a cache in a VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)</span></span>
> 
> 

### <a name="new-support-request"></a><span data-ttu-id="66c3a-341">Nová žádost o podporu</span><span class="sxs-lookup"><span data-stu-id="66c3a-341">New support request</span></span>
<span data-ttu-id="66c3a-342">Klikněte na tlačítko **nová žádost o podporu** otevřete žádost o podporu pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="66c3a-342">Click **New support request** to open a support request for your cache.</span></span>





## <a name="default-redis-server-configuration"></a><span data-ttu-id="66c3a-343">Výchozí konfigurace serveru Redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-343">Default Redis server configuration</span></span>
<span data-ttu-id="66c3a-344">Nové instance služby Azure Redis Cache jsou nakonfigurovány s následující výchozí hodnoty konfigurace Redis.</span><span class="sxs-lookup"><span data-stu-id="66c3a-344">New Azure Redis Cache instances are configured with the following default Redis configuration values.</span></span>

> [!NOTE]
> <span data-ttu-id="66c3a-345">Nastavení v této části nelze změnit pomocí `StackExchange.Redis.IServer.ConfigSet` metoda.</span><span class="sxs-lookup"><span data-stu-id="66c3a-345">The settings in this section cannot be changed using the `StackExchange.Redis.IServer.ConfigSet` method.</span></span> <span data-ttu-id="66c3a-346">Pokud tato metoda je volána s příkazy v této části, je vyvolána výjimka podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="66c3a-346">If this method is called with one of the commands in this section, an exception similar to the following is thrown:</span></span>  
> 
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
> 
> <span data-ttu-id="66c3a-347">Všechny hodnoty, které se dají konfigurovat jako **max paměti policy**, se dají konfigurovat prostřednictvím portálu Azure nebo nástroje příkazového řádku pro správu například Azure CLI nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="66c3a-347">Any values that are configurable, such as **max-memory-policy**, are configurable through the Azure portal or command-line management tools such as Azure CLI or PowerShell.</span></span>
> 
> 

| <span data-ttu-id="66c3a-348">Nastavení</span><span class="sxs-lookup"><span data-stu-id="66c3a-348">Setting</span></span> | <span data-ttu-id="66c3a-349">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="66c3a-349">Default value</span></span> | <span data-ttu-id="66c3a-350">Popis</span><span class="sxs-lookup"><span data-stu-id="66c3a-350">Description</span></span> |
| --- | --- | --- |
| `databases` |<span data-ttu-id="66c3a-351">16</span><span class="sxs-lookup"><span data-stu-id="66c3a-351">16</span></span> |<span data-ttu-id="66c3a-352">Výchozí počet databází je 16, ale můžete nakonfigurovat různé počty podle cenové úrovně. <sup>1</sup> výchozí databáze je databáze 0, můžete vybrat jinou na základě jednotlivých připojení pomocí `connection.GetDatabase(dbid)` kde `dbid` je číslo v rozsahu od `0` a `databases - 1`.</span><span class="sxs-lookup"><span data-stu-id="66c3a-352">The default number of databases is 16 but you can configure a different number based on the pricing tier.<sup>1</sup> The default database is DB 0, you can select a different one on a per-connection basis using `connection.GetDatabase(dbid)` where `dbid` is a number between `0` and `databases - 1`.</span></span> |
| `maxclients` |<span data-ttu-id="66c3a-353">Závisí na cenovou úroveň<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="66c3a-353">Depends on the pricing tier<sup>2</sup></span></span> |<span data-ttu-id="66c3a-354">Toto je maximální počet připojených klientů, které jsou povoleny ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="66c3a-354">This is the maximum number of connected clients allowed at the same time.</span></span> <span data-ttu-id="66c3a-355">Po dosažení limitu Redis zavře všechna nová připojení, a vrátí chybu bylo dosaženo maximální počet klientů.</span><span class="sxs-lookup"><span data-stu-id="66c3a-355">Once the limit is reached Redis closes all the new connections, returning a 'max number of clients reached' error.</span></span> |
| `maxmemory-policy` |`volatile-lru` |<span data-ttu-id="66c3a-356">Maxmemory zásady je nastavení pro jak Redis vybere co při odebrání `maxmemory` je dosaženo (velikost mezipaměti nabídky, jste vybrali při vytvoření mezipaměti).</span><span class="sxs-lookup"><span data-stu-id="66c3a-356">Maxmemory policy is the setting for how Redis selects what to remove when `maxmemory` (the size of the cache offering you selected when you created the cache) is reached.</span></span> <span data-ttu-id="66c3a-357">S Azure Redis Cache ve výchozím nastavení je `volatile-lru`, které odebere klíče s vypršení platnosti nastavit pomocí algoritmu hodnoty nejdelšího Nepoužití.</span><span class="sxs-lookup"><span data-stu-id="66c3a-357">With Azure Redis Cache the default setting is `volatile-lru`, which removes the keys with an expiration set using an LRU algorithm.</span></span> <span data-ttu-id="66c3a-358">Toto nastavení můžete nakonfigurovat na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="66c3a-358">This setting can be configured in the Azure portal.</span></span> <span data-ttu-id="66c3a-359">Další informace najdete v tématu [paměti zásady](#memory-policies).</span><span class="sxs-lookup"><span data-stu-id="66c3a-359">For more information, see [Memory policies](#memory-policies).</span></span> |
| `maxmemory-samples` |<span data-ttu-id="66c3a-360">3</span><span class="sxs-lookup"><span data-stu-id="66c3a-360">3</span></span> |<span data-ttu-id="66c3a-361">Uložit paměti, jsou přibližně algoritmy místo přesné algoritmy LRU a minimální hodnota TTL algoritmy.</span><span class="sxs-lookup"><span data-stu-id="66c3a-361">To save memory, LRU and minimal TTL algorithms are approximated algorithms instead of precise algorithms.</span></span> <span data-ttu-id="66c3a-362">Ve výchozím nastavení Redis tři klíče kontroly a vyskladnění ten, který byl použit méně nedávno.</span><span class="sxs-lookup"><span data-stu-id="66c3a-362">By default Redis checks three keys and picks the one that was used less recently.</span></span> |
| `lua-time-limit` |<span data-ttu-id="66c3a-363">5,000</span><span class="sxs-lookup"><span data-stu-id="66c3a-363">5,000</span></span> |<span data-ttu-id="66c3a-364">Maximální doba provádění skriptu Lua v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="66c3a-364">Max execution time of a Lua script in milliseconds.</span></span> <span data-ttu-id="66c3a-365">Pokud je dosaženo maximální dobu spuštění, zaprotokoluje Redis, je stále v provádění po maximální povolenou dobu skript a spustí odpoví na dotazy s chybou.</span><span class="sxs-lookup"><span data-stu-id="66c3a-365">If the maximum execution time is reached, Redis logs that a script is still in execution after the maximum allowed time, and starts to reply to queries with an error.</span></span> |
| `lua-event-limit` |<span data-ttu-id="66c3a-366">500</span><span class="sxs-lookup"><span data-stu-id="66c3a-366">500</span></span> |<span data-ttu-id="66c3a-367">Maximální velikost fronty událostí skriptu.</span><span class="sxs-lookup"><span data-stu-id="66c3a-367">Max size of script event queue.</span></span> |
| <span data-ttu-id="66c3a-368">`client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub`</span><span class="sxs-lookup"><span data-stu-id="66c3a-368">`client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub`</span></span> |<span data-ttu-id="66c3a-369">0 0 032mb 8mb 60</span><span class="sxs-lookup"><span data-stu-id="66c3a-369">0 0 032mb 8mb 60</span></span> |<span data-ttu-id="66c3a-370">Omezení vyrovnávací paměti výstupní klienta lze vynutit odpojení klienti, kteří nejsou čtení dat ze serveru dostatečně rychle z nějakého důvodu (obvyklým důvodem je, že klient Pub nebo Sub nemůže využívat zprávy rychle, jak můžete vytvořit vydavatele, je).</span><span class="sxs-lookup"><span data-stu-id="66c3a-370">The client output buffer limits can be used to force disconnection of clients that are not reading data from the server fast enough for some reason (a common reason is that a Pub/Sub client can't consume messages as fast as the publisher can produce them).</span></span> <span data-ttu-id="66c3a-371">Další informace najdete v tématu [http://redis.io/topics/clients](http://redis.io/topics/clients).</span><span class="sxs-lookup"><span data-stu-id="66c3a-371">For more information, see [http://redis.io/topics/clients](http://redis.io/topics/clients).</span></span> |

<span data-ttu-id="66c3a-372"><a name="databases"></a>
<sup>1</sup>tento limit pro `databases` se liší pro každý Azure Redis Cache cenová úroveň a můžete nastavit při vytváření mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-372"><a name="databases"></a>
<sup>1</sup>The limit for `databases` is different for each Azure Redis Cache pricing tier and can be set at cache creation.</span></span> <span data-ttu-id="66c3a-373">Pokud žádné `databases` nastavení zadat během vytváření mezipaměti, výchozí hodnota je 16.</span><span class="sxs-lookup"><span data-stu-id="66c3a-373">If no `databases` setting is specified during cache creation, the default is 16.</span></span>

* <span data-ttu-id="66c3a-374">Základní a standardní mezipaměti</span><span class="sxs-lookup"><span data-stu-id="66c3a-374">Basic and Standard caches</span></span>
  * <span data-ttu-id="66c3a-375">C0 (250 MB) mezipaměti - až 16 databáze</span><span class="sxs-lookup"><span data-stu-id="66c3a-375">C0 (250 MB) cache - up to 16 databases</span></span>
  * <span data-ttu-id="66c3a-376">C1 mezipaměti (1 GB) - až 16 databáze</span><span class="sxs-lookup"><span data-stu-id="66c3a-376">C1 (1 GB) cache - up to 16 databases</span></span>
  * <span data-ttu-id="66c3a-377">C2 mezipaměti (2,5 GB) - až 16 databáze</span><span class="sxs-lookup"><span data-stu-id="66c3a-377">C2 (2.5 GB) cache - up to 16 databases</span></span>
  * <span data-ttu-id="66c3a-378">C3 (6 GB) mezipaměti - až 16 databáze</span><span class="sxs-lookup"><span data-stu-id="66c3a-378">C3 (6 GB) cache - up to 16 databases</span></span>
  * <span data-ttu-id="66c3a-379">C4 (13 GB) mezipaměti - až 32 databáze</span><span class="sxs-lookup"><span data-stu-id="66c3a-379">C4 (13 GB) cache - up to 32 databases</span></span>
  * <span data-ttu-id="66c3a-380">C5 (26 GB) mezipaměti - až 48 databáze</span><span class="sxs-lookup"><span data-stu-id="66c3a-380">C5 (26 GB) cache - up to 48 databases</span></span>
  * <span data-ttu-id="66c3a-381">C6 (53 GB) mezipaměti - až 64 databáze</span><span class="sxs-lookup"><span data-stu-id="66c3a-381">C6 (53 GB) cache - up to 64 databases</span></span>
* <span data-ttu-id="66c3a-382">Ukládá do mezipaměti Premium</span><span class="sxs-lookup"><span data-stu-id="66c3a-382">Premium caches</span></span>
  * <span data-ttu-id="66c3a-383">P1 (6 GB - 60 GB) - až 16 databáze</span><span class="sxs-lookup"><span data-stu-id="66c3a-383">P1 (6 GB - 60 GB) - up to 16 databases</span></span>
  * <span data-ttu-id="66c3a-384">P2 (13 GB - 130 GB) – až 32 databáze</span><span class="sxs-lookup"><span data-stu-id="66c3a-384">P2 (13 GB - 130 GB) - up to 32 databases</span></span>
  * <span data-ttu-id="66c3a-385">P3 (26 GB - 260 GB) – až 48 databáze</span><span class="sxs-lookup"><span data-stu-id="66c3a-385">P3 (26 GB - 260 GB) - up to 48 databases</span></span>
  * <span data-ttu-id="66c3a-386">P4 (53 GB - 530 GB) – až 64 databáze</span><span class="sxs-lookup"><span data-stu-id="66c3a-386">P4 (53 GB - 530 GB) - up to 64 databases</span></span>
  * <span data-ttu-id="66c3a-387">Všechny prémiových mezipamětí s clusterem Redis povoleny – Redis cluster podporuje jenom používání databáze 0 proto `databases` omezení pro všechny premium mezipaměť Redis clusteru povolena efektivně 1 a [vyberte](http://redis.io/commands/select) není povolen příkaz.</span><span class="sxs-lookup"><span data-stu-id="66c3a-387">All premium caches with Redis cluster enabled - Redis cluster only supports use of database 0 so the `databases` limit for any premium cache with Redis cluster enabled is effectively 1 and the [Select](http://redis.io/commands/select) command is not allowed.</span></span> <span data-ttu-id="66c3a-388">Další informace najdete v tématu [muset provést jakékoli změny Moje aplikace klienta použít clustering?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)</span><span class="sxs-lookup"><span data-stu-id="66c3a-388">For more information, see [Do I need to make any changes to my client application to use clustering?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)</span></span>

<span data-ttu-id="66c3a-389">Další informace o databáze najdete v tématu [co jsou databáze Redis?](cache-faq.md#what-are-redis-databases)</span><span class="sxs-lookup"><span data-stu-id="66c3a-389">For more information about databases, see [What are Redis databases?](cache-faq.md#what-are-redis-databases)</span></span>

> [!NOTE]
> <span data-ttu-id="66c3a-390">`databases` Nastavení může být nakonfigurovaný jenom během vytváření mezipaměti a pouze pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiných klientů pro správu.</span><span class="sxs-lookup"><span data-stu-id="66c3a-390">The `databases` setting can be configured only during cache creation and only using PowerShell, CLI, or other management clients.</span></span> <span data-ttu-id="66c3a-391">Příklad konfigurace `databases` během vytváření mezipaměti pomocí prostředí PowerShell, najdete v části [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).</span><span class="sxs-lookup"><span data-stu-id="66c3a-391">For an example of configuring `databases` during cache creation using PowerShell, see [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).</span></span>
> 
> 

<span data-ttu-id="66c3a-392"><a name="maxclients"></a>
<sup>2</sup> `maxclients` se liší pro každý Azure Redis Cache cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="66c3a-392"><a name="maxclients"></a>
<sup>2</sup>`maxclients` is different for each Azure Redis Cache pricing tier.</span></span>

* <span data-ttu-id="66c3a-393">Základní a standardní mezipaměti</span><span class="sxs-lookup"><span data-stu-id="66c3a-393">Basic and Standard caches</span></span>
  * <span data-ttu-id="66c3a-394">C0 (250 MB) mezipaměti - až 256 připojení</span><span class="sxs-lookup"><span data-stu-id="66c3a-394">C0 (250 MB) cache - up to 256 connections</span></span>
  * <span data-ttu-id="66c3a-395">C1 mezipaměti (1 GB) – až 1 000 připojení</span><span class="sxs-lookup"><span data-stu-id="66c3a-395">C1 (1 GB) cache - up to 1,000 connections</span></span>
  * <span data-ttu-id="66c3a-396">C2 mezipaměti (2,5 GB) – až 2 000 připojení</span><span class="sxs-lookup"><span data-stu-id="66c3a-396">C2 (2.5 GB) cache - up to 2,000 connections</span></span>
  * <span data-ttu-id="66c3a-397">C3 (6 GB) mezipaměti - až 5 000 připojení</span><span class="sxs-lookup"><span data-stu-id="66c3a-397">C3 (6 GB) cache - up to 5,000 connections</span></span>
  * <span data-ttu-id="66c3a-398">C4 (13 GB) mezipaměti - až 10 000 připojení</span><span class="sxs-lookup"><span data-stu-id="66c3a-398">C4 (13 GB) cache - up to 10,000 connections</span></span>
  * <span data-ttu-id="66c3a-399">C5 (26 GB) mezipaměti - až 15 000 připojení</span><span class="sxs-lookup"><span data-stu-id="66c3a-399">C5 (26 GB) cache - up to 15,000 connections</span></span>
  * <span data-ttu-id="66c3a-400">C6 (53 GB) mezipaměti - až 20 000 připojení</span><span class="sxs-lookup"><span data-stu-id="66c3a-400">C6 (53 GB) cache - up to 20,000 connections</span></span>
* <span data-ttu-id="66c3a-401">Ukládá do mezipaměti Premium</span><span class="sxs-lookup"><span data-stu-id="66c3a-401">Premium caches</span></span>
  * <span data-ttu-id="66c3a-402">P1 (6 GB - 60 GB) – až 7500 připojení</span><span class="sxs-lookup"><span data-stu-id="66c3a-402">P1 (6 GB - 60 GB) - up to 7,500 connections</span></span>
  * <span data-ttu-id="66c3a-403">P2 (13 GB - 130 GB) – až 15 000 připojení</span><span class="sxs-lookup"><span data-stu-id="66c3a-403">P2 (13 GB - 130 GB) - up to 15,000 connections</span></span>
  * <span data-ttu-id="66c3a-404">P3 (26 GB - 260 GB) – až 30 000 připojení</span><span class="sxs-lookup"><span data-stu-id="66c3a-404">P3 (26 GB - 260 GB) - up to 30,000 connections</span></span>
  * <span data-ttu-id="66c3a-405">P4 (53 GB - 530 GB) – až 40 000 připojení</span><span class="sxs-lookup"><span data-stu-id="66c3a-405">P4 (53 GB - 530 GB) - up to 40,000 connections</span></span>

> [!NOTE]
> <span data-ttu-id="66c3a-406">Při každém velikost mezipaměti umožňuje *až* určitý počet připojení, každé připojení k Redis má režie spojená s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="66c3a-406">While each size of cache allows *up to* a certain number of connections, each connection to Redis has overhead associated with it.</span></span> <span data-ttu-id="66c3a-407">Příkladem takových režie může být využití procesoru a paměti v důsledku šifrování pomocí protokolu TLS/SSL.</span><span class="sxs-lookup"><span data-stu-id="66c3a-407">An example of such overhead would be CPU and memory usage as a result of TLS/SSL encryption.</span></span> <span data-ttu-id="66c3a-408">Připojení maximální limit pro velikost daného mezipaměti předpokládá lehkým načíst mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-408">The maximum connection limit for a given cache size assumes a lightly loaded cache.</span></span> <span data-ttu-id="66c3a-409">Pokud načíst z režijní náklady na připojení *plus* zatížení z klientské operace překročí kapacitu pro systém, mezipaměti můžete mít problémy kapacitu, i pokud nebyl překročen limit připojení pro aktuální velikost mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-409">If load from connection overhead *plus* load from client operations exceeds capacity for the system, the cache can experience capacity issues even if you have not exceeded the connection limit for the current cache size.</span></span>
> 
> 



## <a name="redis-commands-not-supported-in-azure-redis-cache"></a><span data-ttu-id="66c3a-410">Redis příkazy nejsou podporované ve službě Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="66c3a-410">Redis commands not supported in Azure Redis Cache</span></span>
> [!IMPORTANT]
> <span data-ttu-id="66c3a-411">Vzhledem k tomu, že konfigurace a Správa instance služby Azure Redis Cache spravuje Microsoft, jsou zakázány následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="66c3a-411">Because configuration and management of Azure Redis Cache instances is managed by Microsoft, the following commands are disabled.</span></span> <span data-ttu-id="66c3a-412">Pokud se pokusíte je vyvolat, zobrazí chybová zpráva podobná `"(error) ERR unknown command"`.</span><span class="sxs-lookup"><span data-stu-id="66c3a-412">If you try to invoke them, you receive an error message similar to `"(error) ERR unknown command"`.</span></span>
> 
> * <span data-ttu-id="66c3a-413">BGREWRITEAOF</span><span class="sxs-lookup"><span data-stu-id="66c3a-413">BGREWRITEAOF</span></span>
> * <span data-ttu-id="66c3a-414">BGSAVE</span><span class="sxs-lookup"><span data-stu-id="66c3a-414">BGSAVE</span></span>
> * <span data-ttu-id="66c3a-415">KONFIGURACE</span><span class="sxs-lookup"><span data-stu-id="66c3a-415">CONFIG</span></span>
> * <span data-ttu-id="66c3a-416">LADĚNÍ</span><span class="sxs-lookup"><span data-stu-id="66c3a-416">DEBUG</span></span>
> * <span data-ttu-id="66c3a-417">MIGRACE</span><span class="sxs-lookup"><span data-stu-id="66c3a-417">MIGRATE</span></span>
> * <span data-ttu-id="66c3a-418">ULOŽIT</span><span class="sxs-lookup"><span data-stu-id="66c3a-418">SAVE</span></span>
> * <span data-ttu-id="66c3a-419">VYPNUTÍ</span><span class="sxs-lookup"><span data-stu-id="66c3a-419">SHUTDOWN</span></span>
> * <span data-ttu-id="66c3a-420">SLAVEOF</span><span class="sxs-lookup"><span data-stu-id="66c3a-420">SLAVEOF</span></span>
> * <span data-ttu-id="66c3a-421">CLUSTER - clusteru zápisu, které jsou příkazy, ale jen pro čtení clusteru příkazy jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="66c3a-421">CLUSTER - Cluster write commands are disabled, but read-only Cluster commands are permitted.</span></span>
> 
> 

<span data-ttu-id="66c3a-422">Další informace o příkazech Redis najdete v tématu [http://redis.io/commands](http://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="66c3a-422">For more information about Redis commands, see [http://redis.io/commands](http://redis.io/commands).</span></span>

## <a name="redis-console"></a><span data-ttu-id="66c3a-423">Konzola redis</span><span class="sxs-lookup"><span data-stu-id="66c3a-423">Redis console</span></span>
<span data-ttu-id="66c3a-424">Bezpečně vydávat příkazy vaší instancí Azure Redis Cache pomocí **konzola Redis**, která je k dispozici na webu Azure portal pro všechny úrovně mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-424">You can securely issue commands to your Azure Redis Cache instances using the **Redis Console**, which is available in the Azure portal for all cache tiers.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="66c3a-425">Konzola Redis nefunguje s [VNET](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-425">The Redis Console does not work with [VNET](cache-how-to-premium-vnet.md).</span></span> <span data-ttu-id="66c3a-426">Pokud vaše mezipaměť je součástí virtuální sítě, jenom pro klienty ve virtuální síti přístup do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-426">When your cache is part of a VNET, only clients in the VNET can access the cache.</span></span> <span data-ttu-id="66c3a-427">Protože se spouští konzola Redis v prohlížeči místní, což je mimo síť VNET, se nemůže připojit ke své mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-427">Because Redis Console runs in your local browser, which is outside the VNET, it can't connect to your cache.</span></span>
> - <span data-ttu-id="66c3a-428">Ne všechny příkazy Redis jsou podporovány ve službě Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="66c3a-428">Not all Redis commands are supported in Azure Redis Cache.</span></span> <span data-ttu-id="66c3a-429">Seznam Redis příkazy, které jsou pro Azure Redis Cache zakázán, najdete v předchozí [Redis příkazy nejsou podporované ve službě Azure Redis Cache](#redis-commands-not-supported-in-azure-redis-cache) části.</span><span class="sxs-lookup"><span data-stu-id="66c3a-429">For a list of Redis commands that are disabled for Azure Redis Cache, see the previous [Redis commands not supported in Azure Redis Cache](#redis-commands-not-supported-in-azure-redis-cache) section.</span></span> <span data-ttu-id="66c3a-430">Další informace o příkazech Redis najdete v tématu [http://redis.io/commands](http://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="66c3a-430">For more information about Redis commands, see [http://redis.io/commands](http://redis.io/commands).</span></span>
> 
> 

<span data-ttu-id="66c3a-431">Přístup ke konzole Redis, klikněte na tlačítko **konzoly** z **Redis Cache** okno.</span><span class="sxs-lookup"><span data-stu-id="66c3a-431">To access the Redis Console, click **Console** from the **Redis Cache** blade.</span></span>

![Konzola redis](./media/cache-configure/redis-console-menu.png)

<span data-ttu-id="66c3a-433">K vydání příkazů proti instance mezipaměti, jednoduše zadejte požadovaný příkaz do konzoly.</span><span class="sxs-lookup"><span data-stu-id="66c3a-433">To issue commands against your cache instance, simply type the desired command into the console.</span></span>

![Konzola redis](./media/cache-configure/redis-console.png)


### <a name="using-the-redis-console-with-a-premium-clustered-cache"></a><span data-ttu-id="66c3a-435">Pomocí konzole Redis cache ve verzi premium clusteru</span><span class="sxs-lookup"><span data-stu-id="66c3a-435">Using the Redis Console with a premium clustered cache</span></span>

<span data-ttu-id="66c3a-436">Když mezipaměť Redis konzoly pomocí prémiový clusteru, můžete vydávat příkazy jeden horizontálního oddílu mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="66c3a-436">When using the Redis Console with a premium clustered cache, you can issue commands to a single shard of the cache.</span></span> <span data-ttu-id="66c3a-437">Příkaz pro konkrétní horizontálního oddílu, nejprve připojí k požadované horizontálního oddílu kliknutím na výběr horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="66c3a-437">To issue a command to a specific shard, first connect to the desired shard by clicking it on the shard picker.</span></span>

![Konzola redis](./media/cache-configure/redis-console-premium-cluster.png)

<span data-ttu-id="66c3a-439">Pokud budete chtít získat přístup ke klíči, který je uložen v různých horizontálního oddílu než připojené horizontálního oddílu, zobrazí chybová zpráva, která je podobná následující zpráva.</span><span class="sxs-lookup"><span data-stu-id="66c3a-439">If you attempt to access a key that is stored in a different shard than the connected shard, you receive an error message similar to the following message.</span></span>

```
shard1>get myKey
(error) MOVED 866 13.90.202.154:13000 (shard 0)
```

<span data-ttu-id="66c3a-440">V předchozím příkladu je ID horizontálního oddílu 1 vybrané horizontálního oddílu, ale `myKey` se nachází v horizontálního oddílu 0, jak `(shard 0)` část chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="66c3a-440">In the previous example, shard 1 is the selected shard, but `myKey` is located in shard 0, as indicated by the `(shard 0)` portion of the error message.</span></span> <span data-ttu-id="66c3a-441">V tomto příkladu se pro přístup k `myKey`, vyberte horizontálního oddílu 0 pomocí nástroje pro výběr horizontálního oddílu a potom vydat příkaz požadované.</span><span class="sxs-lookup"><span data-stu-id="66c3a-441">In this example, to access `myKey`, select shard 0 using the shard picker, and then issue the desired command.</span></span>


## <a name="move-your-cache-to-a-new-subscription"></a><span data-ttu-id="66c3a-442">Vaše mezipaměť přesunout do nového předplatného</span><span class="sxs-lookup"><span data-stu-id="66c3a-442">Move your cache to a new subscription</span></span>
<span data-ttu-id="66c3a-443">Mezipaměti můžete přesunout do nového předplatného kliknutím **přesunout**.</span><span class="sxs-lookup"><span data-stu-id="66c3a-443">You can move your cache to a new subscription by clicking **Move**.</span></span>

![Přesunout mezipaměti Redis](./media/cache-configure/redis-cache-move.png)

<span data-ttu-id="66c3a-445">Informace o přesun prostředků z jedné skupiny prostředků do jiné a z jedno předplatné do druhého, najdete v části [přesunutím prostředků do nové skupiny prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="66c3a-445">For information on moving resources from one resource group to another, and from one subscription to another, see [Move resources to new resource group or subscription](../azure-resource-manager/resource-group-move-resources.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="66c3a-446">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66c3a-446">Next steps</span></span>
* <span data-ttu-id="66c3a-447">Další informace o práci s příkazy Redis najdete v tématu [jak můžete spouštět příkazy Redis?](cache-faq.md#how-can-i-run-redis-commands)</span><span class="sxs-lookup"><span data-stu-id="66c3a-447">For more information on working with Redis commands, see [How can I run Redis commands?](cache-faq.md#how-can-i-run-redis-commands)</span></span>

