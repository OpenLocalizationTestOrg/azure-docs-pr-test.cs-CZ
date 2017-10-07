---
title: aaaHow tooconfigure Azure Redis Cache | Microsoft Docs
description: "Rady pro pochopení hello výchozí konfiguraci Redis pro Azure Redis Cache a zjistěte, jak tooconfigure vaší služby Azure Redis instancích mezipaměti"
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
ms.openlocfilehash: 46bffb74cdf40e0e0a99c3a83dbe06d6fe1ea65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-azure-redis-cache"></a><span data-ttu-id="18ecd-103">Jak tooconfigure Azure mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-103">How tooconfigure Azure Redis Cache</span></span>
<span data-ttu-id="18ecd-104">Toto téma popisuje, jak tooreview a aktualizace hello konfigurace pro vaše instance služby Azure Redis Cache a konfigurace serveru zahrnuje hello výchozí Redis pro instance služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="18ecd-104">This topic describes how tooreview and update hello configuration for your Azure Redis Cache instances, and covers hello default Redis server configuration for Azure Redis Cache instances.</span></span>

> [!NOTE]
> <span data-ttu-id="18ecd-105">Další informace o konfiguraci a použití prémiových funkcí mezipaměti najdete v tématu [jak trvalost tooconfigure](cache-how-to-premium-persistence.md), [jak tooconfigure clustering](cache-how-to-premium-clustering.md), a [jak podporují tooconfigure virtuální sítě ](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-105">For more information on configuring and using premium cache features, see [How tooconfigure persistence](cache-how-to-premium-persistence.md), [How tooconfigure clustering](cache-how-to-premium-clustering.md), and [How tooconfigure Virtual Network support](cache-how-to-premium-vnet.md).</span></span>
> 
> 

## <a name="configure-redis-cache-settings"></a><span data-ttu-id="18ecd-106">Konfigurace nastavení mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-106">Configure Redis cache settings</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

<span data-ttu-id="18ecd-107">Nastavení Azure Redis Cache jsou zobrazit a konfigurovat na hello **Redis Cache** okno pomocí hello **prostředků nabídky**.</span><span class="sxs-lookup"><span data-stu-id="18ecd-107">Azure Redis Cache settings are viewed and configured on hello **Redis Cache** blade using hello **Resource Menu**.</span></span>

![Nastavení mezipaměti redis](./media/cache-configure/redis-cache-settings.png)

<span data-ttu-id="18ecd-109">Můžete zobrazit a konfigurovat následující nastavení pomocí hello hello **prostředků nabídky**.</span><span class="sxs-lookup"><span data-stu-id="18ecd-109">You can view and configure hello following settings using hello **Resource Menu**.</span></span>

* [<span data-ttu-id="18ecd-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="18ecd-110">Overview</span></span>](#overview)
* [<span data-ttu-id="18ecd-111">Protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="18ecd-111">Activity log</span></span>](#activity-log)
* [<span data-ttu-id="18ecd-112">Řízení přístupu (IAM)</span><span class="sxs-lookup"><span data-stu-id="18ecd-112">Access control (IAM)</span></span>](#access-control-iam)
* [<span data-ttu-id="18ecd-113">Značky</span><span class="sxs-lookup"><span data-stu-id="18ecd-113">Tags</span></span>](#tags)
* [<span data-ttu-id="18ecd-114">Diagnóza a řešení problémů</span><span class="sxs-lookup"><span data-stu-id="18ecd-114">Diagnose and solve problems</span></span>](#diagnose-and-solve-problems)
* [<span data-ttu-id="18ecd-115">Nastavení</span><span class="sxs-lookup"><span data-stu-id="18ecd-115">Settings</span></span>](#settings)
    * [<span data-ttu-id="18ecd-116">Přístupové klávesy</span><span class="sxs-lookup"><span data-stu-id="18ecd-116">Access keys</span></span>](#access-keys)
    * [<span data-ttu-id="18ecd-117">Upřesnit nastavení</span><span class="sxs-lookup"><span data-stu-id="18ecd-117">Advanced settings</span></span>](#advanced-settings)
    * [<span data-ttu-id="18ecd-118">Advisor mezipaměti redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-118">Redis Cache Advisor</span></span>](#redis-cache-advisor)
    * [<span data-ttu-id="18ecd-119">Škálování</span><span class="sxs-lookup"><span data-stu-id="18ecd-119">Scale</span></span>](#scale)
    * [<span data-ttu-id="18ecd-120">Velikost clusteru redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-120">Redis cluster size</span></span>](#cluster-size)
    * [<span data-ttu-id="18ecd-121">Trvalosti dat redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-121">Redis data persistence</span></span>](#redis-data-persistence)
    * [<span data-ttu-id="18ecd-122">Plán aktualizací</span><span class="sxs-lookup"><span data-stu-id="18ecd-122">Schedule updates</span></span>](#schedule-updates)
    * [<span data-ttu-id="18ecd-123">Geografická replikace</span><span class="sxs-lookup"><span data-stu-id="18ecd-123">Geo-replication</span></span>](#geo-replication)
    * [<span data-ttu-id="18ecd-124">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="18ecd-124">Virtual Network</span></span>](#virtual-network)
    * [<span data-ttu-id="18ecd-125">Brána firewall</span><span class="sxs-lookup"><span data-stu-id="18ecd-125">Firewall</span></span>](#firewall)
    * [<span data-ttu-id="18ecd-126">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="18ecd-126">Properties</span></span>](#properties)
    * [<span data-ttu-id="18ecd-127">Zámky.</span><span class="sxs-lookup"><span data-stu-id="18ecd-127">Locks</span></span>](#locks)
    * [<span data-ttu-id="18ecd-128">Skriptu pro automatizaci</span><span class="sxs-lookup"><span data-stu-id="18ecd-128">Automation script</span></span>](#automation-script)
* [<span data-ttu-id="18ecd-129">Správa</span><span class="sxs-lookup"><span data-stu-id="18ecd-129">Administration</span></span>](#administration)
    * [<span data-ttu-id="18ecd-130">Umožňuje importovat data</span><span class="sxs-lookup"><span data-stu-id="18ecd-130">Import data</span></span>](#importexport)
    * [<span data-ttu-id="18ecd-131">Export dat</span><span class="sxs-lookup"><span data-stu-id="18ecd-131">Export data</span></span>](#importexport)
    * [<span data-ttu-id="18ecd-132">Restartování</span><span class="sxs-lookup"><span data-stu-id="18ecd-132">Reboot</span></span>](#reboot)
* [<span data-ttu-id="18ecd-133">Monitorování</span><span class="sxs-lookup"><span data-stu-id="18ecd-133">Monitoring</span></span>](#monitoring)
    * [<span data-ttu-id="18ecd-134">Metriky pro redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-134">Redis metrics</span></span>](#redis-metrics)
    * [<span data-ttu-id="18ecd-135">Pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="18ecd-135">Alert rules</span></span>](#alert-rules)
    * [<span data-ttu-id="18ecd-136">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="18ecd-136">Diagnostics</span></span>](#diagnostics)
* [<span data-ttu-id="18ecd-137">Podporovat & řešení potíží s nastavení</span><span class="sxs-lookup"><span data-stu-id="18ecd-137">Support & troubleshooting settings</span></span>](#support-amp-troubleshooting-settings)
    * [<span data-ttu-id="18ecd-138">Stav prostředků</span><span class="sxs-lookup"><span data-stu-id="18ecd-138">Resource health</span></span>](#resource-health)
    * [<span data-ttu-id="18ecd-139">Nová žádost o podporu</span><span class="sxs-lookup"><span data-stu-id="18ecd-139">New support request</span></span>](#new-support-request)


## <a name="overview"></a><span data-ttu-id="18ecd-140">Přehled</span><span class="sxs-lookup"><span data-stu-id="18ecd-140">Overview</span></span>

<span data-ttu-id="18ecd-141">**Přehled** poskytuje k základní informace o mezipaměti, například název, porty, cenová úroveň a metriky vybrané mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-141">**Overview** provides you with basic information about your cache, such as name, ports, pricing tier, and selected cache metrics.</span></span>

### <a name="activity-log"></a><span data-ttu-id="18ecd-142">Protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="18ecd-142">Activity log</span></span>

<span data-ttu-id="18ecd-143">Klikněte na tlačítko **protokol aktivit** tooview akce provádět v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-143">Click **Activity log** tooview actions performed on your cache.</span></span> <span data-ttu-id="18ecd-144">Můžete také použít filtrování tooexpand tato zobrazení tooinclude jiné prostředky.</span><span class="sxs-lookup"><span data-stu-id="18ecd-144">You can also use filtering tooexpand this view tooinclude other resources.</span></span> <span data-ttu-id="18ecd-145">Další informace o práci s protokoly auditu najdete v tématu [auditovat operace s Resource Managerem](../azure-resource-manager/resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-145">For more information on working with audit logs, see [Audit operations with Resource Manager](../azure-resource-manager/resource-group-audit.md).</span></span> <span data-ttu-id="18ecd-146">Další informace o sledování událostí Azure Redis Cache najdete v tématu [operace a výstrahy](cache-how-to-monitor.md#operations-and-alerts).</span><span class="sxs-lookup"><span data-stu-id="18ecd-146">For more information on monitoring Azure Redis Cache events, see [Operations and alerts](cache-how-to-monitor.md#operations-and-alerts).</span></span>

### <a name="access-control-iam"></a><span data-ttu-id="18ecd-147">Řízení přístupu (IAM)</span><span class="sxs-lookup"><span data-stu-id="18ecd-147">Access control (IAM)</span></span>

<span data-ttu-id="18ecd-148">Hello **přístup k ovládacímu prvku (IAM)** části poskytuje podporu pro řízení přístupu na základě role (RBAC) v hello Azure portálu toohelp organizace splňují požadavky na správu jejich přístup jednoduše a přesně.</span><span class="sxs-lookup"><span data-stu-id="18ecd-148">hello **Access control (IAM)** section provides support for role-based access control (RBAC) in hello Azure portal toohelp organizations meet their access management requirements simply and precisely.</span></span> <span data-ttu-id="18ecd-149">Další informace najdete v tématu [řízení přístupu na základě Role v portálu Azure hello](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-149">For more information, see [Role-based access control in hello Azure portal](../active-directory/role-based-access-control-configure.md).</span></span>

### <a name="tags"></a><span data-ttu-id="18ecd-150">Značky</span><span class="sxs-lookup"><span data-stu-id="18ecd-150">Tags</span></span>

<span data-ttu-id="18ecd-151">Hello **značky** část umožňuje uspořádání prostředků.</span><span class="sxs-lookup"><span data-stu-id="18ecd-151">hello **Tags** section helps you organize your resources.</span></span> <span data-ttu-id="18ecd-152">Další informace najdete v tématu [pomocí značky tooorganize vašich prostředků Azure](../azure-resource-manager/resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-152">For more information, see [Using tags tooorganize your Azure resources](../azure-resource-manager/resource-group-using-tags.md).</span></span>


### <a name="diagnose-and-solve-problems"></a><span data-ttu-id="18ecd-153">Diagnostikovat a řešit problémy</span><span class="sxs-lookup"><span data-stu-id="18ecd-153">Diagnose and solve problems</span></span>

<span data-ttu-id="18ecd-154">Klikněte na tlačítko **Diagnostikujte a řešení problémů** toobe součástí běžné problémy a strategie pro jejich řešení.</span><span class="sxs-lookup"><span data-stu-id="18ecd-154">Click **Diagnose and solve problems** toobe provided with common issues and strategies for resolving them.</span></span>



## <a name="settings"></a><span data-ttu-id="18ecd-155">Nastavení</span><span class="sxs-lookup"><span data-stu-id="18ecd-155">Settings</span></span>
<span data-ttu-id="18ecd-156">Hello **nastavení** části vám umožní tooaccess a nakonfigurujte následující nastavení pro mezipaměť hello.</span><span class="sxs-lookup"><span data-stu-id="18ecd-156">hello **Settings** section allows you tooaccess and configure hello following settings for your cache.</span></span>

* [<span data-ttu-id="18ecd-157">Přístupové klávesy</span><span class="sxs-lookup"><span data-stu-id="18ecd-157">Access keys</span></span>](#access-keys)
* [<span data-ttu-id="18ecd-158">Upřesnit nastavení</span><span class="sxs-lookup"><span data-stu-id="18ecd-158">Advanced settings</span></span>](#advanced-settings)
* [<span data-ttu-id="18ecd-159">Advisor mezipaměti redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-159">Redis Cache Advisor</span></span>](#redis-cache-advisor)
* [<span data-ttu-id="18ecd-160">Škálování</span><span class="sxs-lookup"><span data-stu-id="18ecd-160">Scale</span></span>](#scale)
* [<span data-ttu-id="18ecd-161">Velikost clusteru redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-161">Redis cluster size</span></span>](#cluster-size)
* [<span data-ttu-id="18ecd-162">Trvalosti dat redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-162">Redis data persistence</span></span>](#redis-data-persistence)
* [<span data-ttu-id="18ecd-163">Plán aktualizací</span><span class="sxs-lookup"><span data-stu-id="18ecd-163">Schedule updates</span></span>](#schedule-updates)
* [<span data-ttu-id="18ecd-164">Geografická replikace</span><span class="sxs-lookup"><span data-stu-id="18ecd-164">Geo-replication</span></span>](#geo-replication)
* [<span data-ttu-id="18ecd-165">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="18ecd-165">Virtual Network</span></span>](#virtual-network)
* [<span data-ttu-id="18ecd-166">Brána firewall</span><span class="sxs-lookup"><span data-stu-id="18ecd-166">Firewall</span></span>](#firewall)
* [<span data-ttu-id="18ecd-167">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="18ecd-167">Properties</span></span>](#properties)
* [<span data-ttu-id="18ecd-168">Zámky.</span><span class="sxs-lookup"><span data-stu-id="18ecd-168">Locks</span></span>](#locks)
* [<span data-ttu-id="18ecd-169">Skriptu pro automatizaci</span><span class="sxs-lookup"><span data-stu-id="18ecd-169">Automation script</span></span>](#automation-script)



### <a name="access-keys"></a><span data-ttu-id="18ecd-170">Přístupové klíče</span><span class="sxs-lookup"><span data-stu-id="18ecd-170">Access keys</span></span>
<span data-ttu-id="18ecd-171">Klikněte na tlačítko **přístupové klíče** tooview nebo znovu vygenerovat hello přístupových klíčů pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="18ecd-171">Click **Access keys** tooview or regenerate hello access keys for your cache.</span></span> <span data-ttu-id="18ecd-172">Tyto klíče jsou používány hello klienti připojení tooyour mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-172">These keys are used by hello clients connecting tooyour cache.</span></span>

![Přístupové klíče mezipaměti redis](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a><span data-ttu-id="18ecd-174">Upřesnit nastavení</span><span class="sxs-lookup"><span data-stu-id="18ecd-174">Advanced settings</span></span>
<span data-ttu-id="18ecd-175">Hello se konfiguruje následující nastavení na hello **upřesňující nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="18ecd-175">hello following settings are configured on hello **Advanced settings** blade.</span></span>

* [<span data-ttu-id="18ecd-176">Přístupové porty</span><span class="sxs-lookup"><span data-stu-id="18ecd-176">Access Ports</span></span>](#access-ports)
* [<span data-ttu-id="18ecd-177">Zásady paměti</span><span class="sxs-lookup"><span data-stu-id="18ecd-177">Memory policies</span></span>](#memory-policies)
* [<span data-ttu-id="18ecd-178">Oznámení keyspace (rozšířené nastavení)</span><span class="sxs-lookup"><span data-stu-id="18ecd-178">Keyspace notifications (advanced settings)</span></span>](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a><span data-ttu-id="18ecd-179">Přístupové porty</span><span class="sxs-lookup"><span data-stu-id="18ecd-179">Access Ports</span></span>
<span data-ttu-id="18ecd-180">Přístup bez SSL je ve výchozím nastavení pro nové mezipaměti zakázaný.</span><span class="sxs-lookup"><span data-stu-id="18ecd-180">By default, non-SSL access is disabled for new caches.</span></span> <span data-ttu-id="18ecd-181">tooenable hello bez SSL port, klikněte na tlačítko **ne** pro **povolit přístup jen přes SSL** na hello **upřesňující nastavení** okna a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="18ecd-181">tooenable hello non-SSL port, click **No** for **Allow access only via SSL** on hello **Advanced settings** blade and then click **Save**.</span></span>

![Přístupové porty mezipaměti redis](./media/cache-configure/redis-cache-access-ports.png)

<a name="maxmemory-policy-and-maxmemory-reserved"></a>
#### <a name="memory-policies"></a><span data-ttu-id="18ecd-183">Zásady paměti</span><span class="sxs-lookup"><span data-stu-id="18ecd-183">Memory policies</span></span>
<span data-ttu-id="18ecd-184">Hello **Maxmemory zásad**, **vyhrazené maxmemory**, a **vyhrazené maxfragmentationmemory** nastavení na hello **upřesňující nastavení**okno nakonfigurovat zásady hello paměť pro mezipaměť hello.</span><span class="sxs-lookup"><span data-stu-id="18ecd-184">hello **Maxmemory policy**, **maxmemory-reserved**, and **maxfragmentationmemory-reserved** settings on hello **Advanced settings** blade configure hello memory policies for hello cache.</span></span>

![Zásady Maxmemory mezipaměti redis](./media/cache-configure/redis-cache-maxmemory-policy.png)

<span data-ttu-id="18ecd-186">**Zásady Maxmemory** nakonfiguruje zásady vyřazení hello hello mezipaměti a umožňuje vám toochoose z hello následující zásady vyřazení:</span><span class="sxs-lookup"><span data-stu-id="18ecd-186">**Maxmemory policy** configures hello eviction policy for hello cache and allows you toochoose from hello following eviction policies:</span></span>

* <span data-ttu-id="18ecd-187">`volatile-lru`-Toto je výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="18ecd-187">`volatile-lru` - this is hello default.</span></span>
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

<span data-ttu-id="18ecd-188">Další informace o `maxmemory` zásady, najdete v části [zásady vyřazení](http://redis.io/topics/lru-cache#eviction-policies).</span><span class="sxs-lookup"><span data-stu-id="18ecd-188">For more information about `maxmemory` policies, see [Eviction policies](http://redis.io/topics/lru-cache#eviction-policies).</span></span>

<span data-ttu-id="18ecd-189">Hello **maxmemory vyhrazené** nastavení konfiguruje hello množství paměti v MB, která je vyhrazena pro operace bez mezipaměti, jako je například replikace během převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="18ecd-189">hello **maxmemory-reserved** setting configures hello amount of memory in MB that is reserved for non-cache operations such as replication during failover.</span></span> <span data-ttu-id="18ecd-190">Nastavení této hodnoty můžete toohave více konzistentního prostředí serveru Redis při zatížení se liší.</span><span class="sxs-lookup"><span data-stu-id="18ecd-190">Setting this value allows you toohave a more consistent Redis server experience when your load varies.</span></span> <span data-ttu-id="18ecd-191">Tato hodnota je třeba nastavit vyšší pro úlohy, které jsou zapsat náročné.</span><span class="sxs-lookup"><span data-stu-id="18ecd-191">This value should be set higher for workloads that are write heavy.</span></span> <span data-ttu-id="18ecd-192">Když paměti je vyhrazena pro takové operace, není k dispozici pro úložiště dat uložených v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-192">When memory is reserved for such operations, it is unavailable for storage of cached data.</span></span>

<span data-ttu-id="18ecd-193">Hello **maxfragmentationmemory vyhrazené** nastavení konfiguruje hello množství paměti v MB, který je vyhrazený tooaccommodate pro fragmentace paměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-193">hello **maxfragmentationmemory-reserved** setting configures hello amount of memory in MB that is reserved tooaccommodate for memory fragmentation.</span></span> <span data-ttu-id="18ecd-194">Nastavení této hodnoty můžete toohave více konzistentního prostředí serveru Redis při je hello mezipaměť plná nebo zavřít toofull a hello fragmentace poměr je také vysokou.</span><span class="sxs-lookup"><span data-stu-id="18ecd-194">Setting this value allows you toohave a more consistent Redis server experience when hello cache is full or close toofull and hello fragmentation ratio is also high.</span></span> <span data-ttu-id="18ecd-195">Když paměti je vyhrazena pro takové operace, není k dispozici pro úložiště dat uložených v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-195">When memory is reserved for such operations, it is unavailable for storage of cached data.</span></span>

<span data-ttu-id="18ecd-196">Jednou z věcí tooconsider při výběru novou hodnotu rezervace paměti (**vyhrazené maxmemory** nebo **vyhrazené maxfragmentationmemory**) je, jak tato změna může ovlivnit mezipaměti, která je již spuštěn s velké objemy dat v něm.</span><span class="sxs-lookup"><span data-stu-id="18ecd-196">One thing tooconsider when choosing a new memory reservation value (**maxmemory-reserved** or **maxfragmentationmemory-reserved**) is how this change might affect a cache that is already running with large amounts of data in it.</span></span> <span data-ttu-id="18ecd-197">Například pokud mají mezipaměť 53 GB s 49 GB dat, pak změňte hello rezervace hodnota too8 GB, bude vyřadit hello maximální dostupné paměti pro systém hello dolů too45 GB.</span><span class="sxs-lookup"><span data-stu-id="18ecd-197">For instance, if you have a 53 GB cache with 49 GB of data, then change hello reservation value too8 GB, this will drop hello max available memory for hello system down too45 GB.</span></span> <span data-ttu-id="18ecd-198">Pokud buď vaše aktuální `used_memory` , vaše `used_memory_rss` hodnoty jsou vyšší, než nový limit hello 45 GB a pak hello systému budou mít tooevict data dokud `used_memory` a `used_memory_rss` jsou pod 45 GB.</span><span class="sxs-lookup"><span data-stu-id="18ecd-198">If either your current `used_memory` or your `used_memory_rss` values are higher than hello new limit of 45 GB, then hello system will have tooevict data until both `used_memory` and `used_memory_rss` are below 45 GB.</span></span> <span data-ttu-id="18ecd-199">Vyřazení může zvýšit fragmentace zatížení a paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="18ecd-199">Eviction can increase server load and memory fragmentation.</span></span> <span data-ttu-id="18ecd-200">Další informace o metrikách mezipaměti jako `used_memory` a `used_memory_rss`, najdete v části [k dostupným metrikám a vytváření sestav intervaly](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).</span><span class="sxs-lookup"><span data-stu-id="18ecd-200">For more information on cache metrics such as `used_memory` and `used_memory_rss`, see [Available metrics and reporting intervals](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18ecd-201">Hello **vyhrazené maxmemory** a **maxfragmentationmemory vyhrazené** nastavení jsou dostupná jenom pro Standard a Premium ukládá do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-201">hello **maxmemory-reserved** and **maxfragmentationmemory-reserved** settings are only available for Standard and Premium caches.</span></span>
> 
> 

#### <a name="keyspace-notifications-advanced-settings"></a><span data-ttu-id="18ecd-202">Oznámení keyspace (rozšířené nastavení)</span><span class="sxs-lookup"><span data-stu-id="18ecd-202">Keyspace notifications (advanced settings)</span></span>
<span data-ttu-id="18ecd-203">Redis keyspace oznámení jsou nakonfigurované na hello **upřesňující nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="18ecd-203">Redis keyspace notifications are configured on hello **Advanced settings** blade.</span></span> <span data-ttu-id="18ecd-204">Oznámení keyspace umožňují klienti tooreceive oznámení při určité události.</span><span class="sxs-lookup"><span data-stu-id="18ecd-204">Keyspace notifications allow clients tooreceive notifications when certain events occur.</span></span>

![Upřesňující nastavení mezipaměti redis](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> <span data-ttu-id="18ecd-206">Keyspace oznámení a hello **oznámení události keyspace** nastavení jsou dostupná jenom pro Cache úrovní Standard a Premium.</span><span class="sxs-lookup"><span data-stu-id="18ecd-206">Keyspace notifications and hello **notify-keyspace-events** setting are only available for Standard and Premium caches.</span></span>
> 
> 

<span data-ttu-id="18ecd-207">Další informace najdete v tématu [Redis oznámení Keyspace](http://redis.io/topics/notifications).</span><span class="sxs-lookup"><span data-stu-id="18ecd-207">For more information, see [Redis Keyspace Notifications](http://redis.io/topics/notifications).</span></span> <span data-ttu-id="18ecd-208">Ukázkový kód, najdete v části hello [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) souboru v hello [Hello, world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) ukázka.</span><span class="sxs-lookup"><span data-stu-id="18ecd-208">For sample code, see hello [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) file in hello [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) sample.</span></span>


<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a><span data-ttu-id="18ecd-209">Advisor mezipaměti redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-209">Redis Cache Advisor</span></span>
<span data-ttu-id="18ecd-210">Hello **Redis Cache Advisor** zobrazuje doporučení pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="18ecd-210">hello **Redis Cache Advisor** blade displays recommendations for your cache.</span></span> <span data-ttu-id="18ecd-211">Během normálních operací zobrazí se žádná doporučení.</span><span class="sxs-lookup"><span data-stu-id="18ecd-211">During normal operations, no recommendations are displayed.</span></span> 

![Doporučení](./media/cache-configure/redis-cache-no-recommendations.png)

<span data-ttu-id="18ecd-213">Pokud jakékoliv podmínky, ke kterým došlo během operace hello svojí mezipaměti, jako je například velké množství paměti, šířky pásma sítě nebo zatížení serveru, zobrazí se výstraha na hello **Redis Cache** okno.</span><span class="sxs-lookup"><span data-stu-id="18ecd-213">If any conditions occur during hello operations of your cache such as high memory usage, network bandwidth, or server load, an alert is displayed on hello **Redis Cache** blade.</span></span>

![Doporučení](./media/cache-configure/redis-cache-recommendations-alert.png)

<span data-ttu-id="18ecd-215">Další informace naleznete na hello **doporučení** okno.</span><span class="sxs-lookup"><span data-stu-id="18ecd-215">Further information can be found on hello **Recommendations** blade.</span></span>

![Doporučení](./media/cache-configure/redis-cache-recommendations.png)

<span data-ttu-id="18ecd-217">Můžete sledovat tyto metriky na hello [tabulek sledování](cache-how-to-monitor.md#monitoring-charts) a [využití grafy](cache-how-to-monitor.md#usage-charts) části hello **Redis Cache** okno.</span><span class="sxs-lookup"><span data-stu-id="18ecd-217">You can monitor these metrics on hello [Monitoring charts](cache-how-to-monitor.md#monitoring-charts) and [Usage charts](cache-how-to-monitor.md#usage-charts) sections of hello **Redis Cache** blade.</span></span>

<span data-ttu-id="18ecd-218">Každá cenová úroveň má jiné limity pro připojení klientů, paměti a šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="18ecd-218">Each pricing tier has different limits for client connections, memory, and bandwidth.</span></span> <span data-ttu-id="18ecd-219">Pokud vaše mezipaměť blíží maximální kapacity pro tyto metriky po dobu delší dobu, vytvoří se doporučení.</span><span class="sxs-lookup"><span data-stu-id="18ecd-219">If your cache approaches maximum capacity for these metrics over a sustained period of time, a recommendation is created.</span></span> <span data-ttu-id="18ecd-220">Další informace o hello metriky a omezení zkontrolovány uživatelem hello **doporučení** nástroje najdete v tématu hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="18ecd-220">For more information about hello metrics and limits reviewed by hello **Recommendations** tool, see hello following table:</span></span>

| <span data-ttu-id="18ecd-221">Metrika mezipaměti redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-221">Redis Cache metric</span></span> | <span data-ttu-id="18ecd-222">Další informace</span><span class="sxs-lookup"><span data-stu-id="18ecd-222">More information</span></span> |
| --- | --- |
| <span data-ttu-id="18ecd-223">Využití šířky pásma sítě</span><span class="sxs-lookup"><span data-stu-id="18ecd-223">Network bandwidth usage</span></span> |[<span data-ttu-id="18ecd-224">Výkon mezipaměti - dostupnou šířku pásma</span><span class="sxs-lookup"><span data-stu-id="18ecd-224">Cache performance - available bandwidth</span></span>](cache-faq.md#cache-performance) |
| <span data-ttu-id="18ecd-225">Připojení klienti</span><span class="sxs-lookup"><span data-stu-id="18ecd-225">Connected clients</span></span> |[<span data-ttu-id="18ecd-226">Výchozí konfigurace serveru Redis - maxclients</span><span class="sxs-lookup"><span data-stu-id="18ecd-226">Default Redis server configuration - maxclients</span></span>](#maxclients) |
| <span data-ttu-id="18ecd-227">Zatížení serveru</span><span class="sxs-lookup"><span data-stu-id="18ecd-227">Server load</span></span> |[<span data-ttu-id="18ecd-228">Použití grafů – zatížení serveru Redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-228">Usage charts - Redis Server Load</span></span>](cache-how-to-monitor.md#usage-charts) |
| <span data-ttu-id="18ecd-229">Využití paměti</span><span class="sxs-lookup"><span data-stu-id="18ecd-229">Memory usage</span></span> |[<span data-ttu-id="18ecd-230">Výkon mezipaměti - velikost</span><span class="sxs-lookup"><span data-stu-id="18ecd-230">Cache performance - size</span></span>](cache-faq.md#cache-performance) |

<span data-ttu-id="18ecd-231">tooupgrade mezipaměti, klikněte na tlačítko **upgradovat nyní** toochange hello cenová úroveň a [škálování](#scale) svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-231">tooupgrade your cache, click **Upgrade now** toochange hello pricing tier and [scale](#scale) your cache.</span></span> <span data-ttu-id="18ecd-232">Další informace o výběru cenovou úroveň, najdete v části [jaké mezipaměť Redis nabídky a velikosti použít?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)</span><span class="sxs-lookup"><span data-stu-id="18ecd-232">For more information on choosing a pricing tier, see [What Redis Cache offering and size should I use?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)</span></span>


### <a name="scale"></a><span data-ttu-id="18ecd-233">Měřítko</span><span class="sxs-lookup"><span data-stu-id="18ecd-233">Scale</span></span>
<span data-ttu-id="18ecd-234">Klikněte na tlačítko **škálování** tooview nebo změňte hello cenovou úroveň pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="18ecd-234">Click **Scale** tooview or change hello pricing tier for your cache.</span></span> <span data-ttu-id="18ecd-235">Další informace o škálování najdete v tématu [jak tooScale Azure mezipaměti Redis](cache-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-235">For more information on scaling, see [How tooScale Azure Redis Cache](cache-how-to-scale.md).</span></span>

![Cenová úroveň mezipaměti redis](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a><span data-ttu-id="18ecd-237">Velikost clusteru redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-237">Redis Cluster Size</span></span>
<span data-ttu-id="18ecd-238">Klikněte na tlačítko **velikost clusteru Redis (PREVIEW)** toochange hello velikost clusteru pro spuštěné premium mezipaměti s povoleným clusteringem.</span><span class="sxs-lookup"><span data-stu-id="18ecd-238">Click **(PREVIEW) Redis Cluster Size** toochange hello cluster size for a running premium cache with clustering enabled.</span></span>

> [!NOTE]
> <span data-ttu-id="18ecd-239">Všimněte si, že při hello Azure Redis Cache Premium vrstvy byl vydané tooGeneral dostupnosti, hello Redis velikost clusteru funkce je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="18ecd-239">Note that while hello Azure Redis Cache Premium tier has been released tooGeneral Availability, hello Redis Cluster Size feature is currently in preview.</span></span>
> 
> 

![Velikost clusteru redis](./media/cache-configure/redis-cache-redis-cluster-size.png)

<span data-ttu-id="18ecd-241">velikost clusteru hello toochange, použijte hello posuvníku nebo zadejte číslo mezi 1 a 10 hello **počet horizontálních** textového pole a klikněte na tlačítko **OK** toosave.</span><span class="sxs-lookup"><span data-stu-id="18ecd-241">toochange hello cluster size, use hello slider or type a number between 1 and 10 in hello **Shard count** text box and click **OK** toosave.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18ecd-242">Redis clustering je dostupná jenom pro prémiových mezipamětí.</span><span class="sxs-lookup"><span data-stu-id="18ecd-242">Redis clustering is only available for Premium caches.</span></span> <span data-ttu-id="18ecd-243">Další informace najdete v tématu [jak tooconfigure clusteringu pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-clustering.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-243">For more information, see [How tooconfigure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md).</span></span>
> 
> 


### <a name="redis-data-persistence"></a><span data-ttu-id="18ecd-244">Trvalosti dat redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-244">Redis data persistence</span></span>
<span data-ttu-id="18ecd-245">Klikněte na tlačítko **trvalosti dat Redis** tooenable, zakázat nebo konfigurace trvalosti dat pro mezipaměť premium.</span><span class="sxs-lookup"><span data-stu-id="18ecd-245">Click **Redis data persistence** tooenable, disable, or configure data persistence for your premium cache.</span></span> <span data-ttu-id="18ecd-246">Azure Redis Cache nabízí buď pomocí trvalosti Redis [RDB trvalost](cache-how-to-premium-persistence.md#configure-rdb-persistence) nebo [AOF trvalost](cache-how-to-premium-persistence.md#configure-aof-persistence).</span><span class="sxs-lookup"><span data-stu-id="18ecd-246">Azure Redis Cache offers Redis persistence using either [RDB persistence](cache-how-to-premium-persistence.md#configure-rdb-persistence) or [AOF persistence](cache-how-to-premium-persistence.md#configure-aof-persistence).</span></span>

<span data-ttu-id="18ecd-247">Další informace najdete v tématu [jak tooconfigure trvalosti pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-persistence.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-247">For more information, see [How tooconfigure persistence for a Premium Azure Redis Cache](cache-how-to-premium-persistence.md).</span></span>


> [!IMPORTANT]
> <span data-ttu-id="18ecd-248">Trvalosti dat redis je dostupná jenom pro prémiových mezipamětí.</span><span class="sxs-lookup"><span data-stu-id="18ecd-248">Redis data persistence is only available for Premium caches.</span></span> 
> 
> 

### <a name="schedule-updates"></a><span data-ttu-id="18ecd-249">Aktualizace plánu</span><span class="sxs-lookup"><span data-stu-id="18ecd-249">Schedule updates</span></span>
<span data-ttu-id="18ecd-250">Hello **naplánovat aktualizace** okno vám umožní toodesignate údržby pro aktualizace serveru Redis ke svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-250">hello **Schedule updates** blade allows you toodesignate a maintenance window for Redis server updates for your cache.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="18ecd-251">Hello časové období údržby platí pouze tooRedis serveru aktualizace a ne tooany Azure aktualizace nebo aktualizace operačního systému toohello hello virtuálních počítačů, které jsou hostiteli hello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-251">hello maintenance window applies only tooRedis server updates, and not tooany Azure updates or updates toohello operating system of hello VMs that host hello cache.</span></span>
> 
> 

![Aktualizace plánu](./media/cache-configure/redis-schedule-updates.png)

<span data-ttu-id="18ecd-253">toospecify údržby, zkontrolujte hello potřeby dnů hodina spouštění údržby okno hello za každý den, zadejte a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="18ecd-253">toospecify a maintenance window, check hello desired days and specify hello maintenance window start hour for each day, and click **OK**.</span></span> <span data-ttu-id="18ecd-254">Všimněte si, že hello časového období údržby se ve standardu UTC.</span><span class="sxs-lookup"><span data-stu-id="18ecd-254">Note that hello maintenance window time is in UTC.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="18ecd-255">Hello **naplánovat aktualizace** funkce je dostupná jenom pro prémiových mezipamětí vrstvy.</span><span class="sxs-lookup"><span data-stu-id="18ecd-255">hello **Schedule updates** functionality is only available for Premium tier caches.</span></span> <span data-ttu-id="18ecd-256">Další informace a pokyny najdete v tématu [správy Azure Redis Cache - naplánovat aktualizace](cache-administration.md#schedule-updates).</span><span class="sxs-lookup"><span data-stu-id="18ecd-256">For more information and instructions, see [Azure Redis Cache administration - Schedule updates](cache-administration.md#schedule-updates).</span></span>
> 
> 

### <a name="geo-replication"></a><span data-ttu-id="18ecd-257">Geografická replikace</span><span class="sxs-lookup"><span data-stu-id="18ecd-257">Geo-replication</span></span>

<span data-ttu-id="18ecd-258">Hello **geografická replikace** okno poskytuje mechanismus pro propojení dvě instance vrstvy Azure Redis Cache Premium.</span><span class="sxs-lookup"><span data-stu-id="18ecd-258">hello **Geo-replication** blade provides a mechanism for linking two Premium tier Azure Redis Cache instances.</span></span> <span data-ttu-id="18ecd-259">Jeden mezipaměti je určený jako primární propojené mezipaměti hello a hello jiných jako sekundární propojené mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="18ecd-259">One cache is designated as hello primary linked cache, and hello other as hello secondary linked cache.</span></span> <span data-ttu-id="18ecd-260">Hello sekundární propojené mezipaměti se stane, jen pro čtení a data napsané toohello primární mezipaměť je replikují toohello sekundární propojené mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-260">hello secondary linked cache becomes read-only, and data written toohello primary cache is replicated toohello secondary linked cache.</span></span> <span data-ttu-id="18ecd-261">Tuto funkci lze použít tooreplicate mezipaměti nad oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="18ecd-261">This functionality can be used tooreplicate a cache across Azure regions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18ecd-262">**Geografická replikace** je dostupná jenom u prémiových mezipamětí vrstvy.</span><span class="sxs-lookup"><span data-stu-id="18ecd-262">**Geo-replication** is only available for Premium tier caches.</span></span> <span data-ttu-id="18ecd-263">Další informace a pokyny najdete v tématu [jak tooconfigure geografická replikace pro Azure Redis Cache](cache-how-to-geo-replication.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-263">For more information and instructions, see [How tooconfigure Geo-replication for Azure Redis Cache](cache-how-to-geo-replication.md).</span></span>
> 
> 

### <a name="virtual-network"></a><span data-ttu-id="18ecd-264">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="18ecd-264">Virtual Network</span></span>
<span data-ttu-id="18ecd-265">Hello **virtuální sítě** část vám tooconfigure hello nastavení virtuální sítě pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="18ecd-265">hello **Virtual Network** section allows you tooconfigure hello virtual network settings for your cache.</span></span> <span data-ttu-id="18ecd-266">Informace týkající se vytváření cache ve verzi premium se virtuální síť podporovat a aktualizuje jeho nastavení, najdete v tématu [jak tooconfigure podpora virtuální sítě pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-266">For information on creating a premium cache with VNET support and updating its settings, see [How tooconfigure Virtual Network Support for a Premium Azure Redis Cache](cache-how-to-premium-vnet.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18ecd-267">Nastavení virtuální sítě jsou dostupné pouze pro prémiových mezipamětí, které byly nakonfigurovány s podporou virtuální sítě během vytváření mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-267">Virtual network settings are only available for premium caches that were configured with VNET support during cache creation.</span></span> 
> 
> 

### <a name="firewall"></a><span data-ttu-id="18ecd-268">Brána firewall</span><span class="sxs-lookup"><span data-stu-id="18ecd-268">Firewall</span></span>

<span data-ttu-id="18ecd-269">Klikněte na tlačítko **brány Firewall** tooview a konfigurace pravidel brány firewall pro vaše mezipaměť Azure Redis Cache Premium.</span><span class="sxs-lookup"><span data-stu-id="18ecd-269">Click **Firewall** tooview and configure firewall rules for your Premium Azure Redis Cache.</span></span>

![Brána firewall](./media/cache-configure/redis-firewall-rules.png)

<span data-ttu-id="18ecd-271">Pravidla brány firewall můžete zadat s počáteční a koncové rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="18ecd-271">You can specify firewall rules with a start and end IP address range.</span></span> <span data-ttu-id="18ecd-272">Pokud jsou nakonfigurovaná pravidla brány firewall, připojení klienta pouze z hello zadané rozsahy IP adres můžete připojit toohello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-272">When firewall rules are configured, only client connections from hello specified IP address ranges can connect toohello cache.</span></span> <span data-ttu-id="18ecd-273">Při uložení pravidlo brány firewall není chvíli trvat, než pravidla hello je platná.</span><span class="sxs-lookup"><span data-stu-id="18ecd-273">When a firewall rule is saved there is a short delay before hello rule is effective.</span></span> <span data-ttu-id="18ecd-274">Toto opoždění je obvykle méně než jedna minuta.</span><span class="sxs-lookup"><span data-stu-id="18ecd-274">This delay is typically less than one minute.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18ecd-275">Připojení z Azure Redis Cache monitorování systémů jsou vždy povoleny, i když jsou nakonfigurovaná pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="18ecd-275">Connections from Azure Redis Cache monitoring systems are always permitted, even if firewall rules are configured.</span></span>
> 
> <span data-ttu-id="18ecd-276">Pravidla brány firewall jsou dostupná jenom pro prémiových mezipamětí vrstvy.</span><span class="sxs-lookup"><span data-stu-id="18ecd-276">Firewall rules are only available for Premium tier caches.</span></span>
> 
> 

### <a name="properties"></a><span data-ttu-id="18ecd-277">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="18ecd-277">Properties</span></span>
<span data-ttu-id="18ecd-278">Klikněte na tlačítko **vlastnosti** tooview informace o mezipaměti, včetně hello koncový bod mezipaměti a portů.</span><span class="sxs-lookup"><span data-stu-id="18ecd-278">Click **Properties** tooview information about your cache, including hello cache endpoint and ports.</span></span>

![Vlastnosti mezipaměti redis](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a><span data-ttu-id="18ecd-280">Zámky</span><span class="sxs-lookup"><span data-stu-id="18ecd-280">Locks</span></span>
<span data-ttu-id="18ecd-281">Hello **zamkne** část vám toolock předplatné, skupinu prostředků nebo prostředek tooprevent jiných uživatelů ve vaší organizaci neúmyslnému odstranění nebo úprava důležitých prostředků.</span><span class="sxs-lookup"><span data-stu-id="18ecd-281">hello **Locks** section allows you toolock a subscription, resource group, or resource tooprevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="18ecd-282">Další informace najdete v tématu [Zamknutí prostředků pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-282">For more information, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

### <a name="automation-script"></a><span data-ttu-id="18ecd-283">Skriptu pro automatizaci</span><span class="sxs-lookup"><span data-stu-id="18ecd-283">Automation script</span></span>

<span data-ttu-id="18ecd-284">Klikněte na tlačítko **skriptu pro automatizaci** toobuild a exportovat šablonu vaše nasazené prostředky pro budoucí nasazení.</span><span class="sxs-lookup"><span data-stu-id="18ecd-284">Click **Automation script** toobuild and export a template of your deployed resources for future deployments.</span></span> <span data-ttu-id="18ecd-285">Další informace o práci se šablonami najdete v tématu [nasazení prostředků pomocí šablony Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-285">For more information about working with templates, see [Deploy resources with Azure Resource Manager templates](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

## <a name="administration-settings"></a><span data-ttu-id="18ecd-286">Nastavení správy</span><span class="sxs-lookup"><span data-stu-id="18ecd-286">Administration settings</span></span>
<span data-ttu-id="18ecd-287">Hello nastavení v hello **správy** části umožňují tooperform hello následující úlohy správy pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="18ecd-287">hello settings in hello **Administration** section allow you tooperform hello following administrative tasks for your cache.</span></span> 

![Správa](./media/cache-configure/redis-cache-administration.png)

* [<span data-ttu-id="18ecd-289">Umožňuje importovat data</span><span class="sxs-lookup"><span data-stu-id="18ecd-289">Import data</span></span>](#importexport)
* [<span data-ttu-id="18ecd-290">Export dat</span><span class="sxs-lookup"><span data-stu-id="18ecd-290">Export data</span></span>](#importexport)
* [<span data-ttu-id="18ecd-291">Restartování</span><span class="sxs-lookup"><span data-stu-id="18ecd-291">Reboot</span></span>](#reboot)


### <a name="importexport"></a><span data-ttu-id="18ecd-292">Import/export</span><span class="sxs-lookup"><span data-stu-id="18ecd-292">Import/Export</span></span>
<span data-ttu-id="18ecd-293">Import a Export je operace správy Azure Redis Cache data, která vám umožní tooimport a export dat v mezipaměti hello import a Export snímku databáze Redis Cache (RDB) ze stránky premium mezipaměti tooa blob v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="18ecd-293">Import/Export is an Azure Redis Cache data management operation, which allows you tooimport and export data in hello cache by importing and exporting a Redis Cache Database (RDB) snapshot from a premium cache tooa page blob in an Azure Storage Account.</span></span> <span data-ttu-id="18ecd-294">Import a Export vám umožní toomigrate mezi různými instancemi Azure Redis Cache nebo naplňte hello mezipaměť s daty před použitím.</span><span class="sxs-lookup"><span data-stu-id="18ecd-294">Import/Export enables you toomigrate between different Azure Redis Cache instances or populate hello cache with data before use.</span></span>

<span data-ttu-id="18ecd-295">Import lze použít toobring Redis kompatibilní RDB soubory z jakéhokoli Redis serveru se systémem v libovolném cloudu nebo prostředí, včetně Redis systémem Linux, Windows nebo kteréhokoli poskytovatele cloudových služeb jako Amazon Web Services a dalších.</span><span class="sxs-lookup"><span data-stu-id="18ecd-295">Import can be used toobring Redis compatible RDB files from any Redis server running in any cloud or environment, including Redis running on Linux, Windows, or any cloud provider such as Amazon Web Services and others.</span></span> <span data-ttu-id="18ecd-296">Import dat se snadný způsob toocreate mezipaměti s předem vyplněná daty.</span><span class="sxs-lookup"><span data-stu-id="18ecd-296">Importing data is an easy way toocreate a cache with pre-populated data.</span></span> <span data-ttu-id="18ecd-297">Během procesu importu hello Azure Redis Cache načte hello RDB soubory z úložiště Azure do paměti a pak vloží hello klíčů do mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="18ecd-297">During hello import process, Azure Redis Cache loads hello RDB files from Azure storage into memory, and then inserts hello keys into hello cache.</span></span>

<span data-ttu-id="18ecd-298">Export umožňuje tooexport hello data uložená v Azure Redis Cache tooRedis kompatibilní RDB soubory.</span><span class="sxs-lookup"><span data-stu-id="18ecd-298">Export allows you tooexport hello data stored in Azure Redis Cache tooRedis compatible RDB files.</span></span> <span data-ttu-id="18ecd-299">Můžete použít tuto funkci toomove data tooanother instanci Azure Redis Cache či tooanother serveru Redis.</span><span class="sxs-lookup"><span data-stu-id="18ecd-299">You can use this feature toomove data from one Azure Redis Cache instance tooanother or tooanother Redis server.</span></span> <span data-ttu-id="18ecd-300">Během procesu exportu hello dočasný soubor vytvořen na hello virtuálních počítačů, hostitelů hello instanci serveru Azure Redis Cache a soubor hello je nahrané toohello určený účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="18ecd-300">During hello export process, a temporary file is created on hello VM that hosts hello Azure Redis Cache server instance, and hello file is uploaded toohello designated storage account.</span></span> <span data-ttu-id="18ecd-301">Po dokončení operace exportu hello se stavem úspěch nebo selhání hello dočasný soubor bude odstraněn.</span><span class="sxs-lookup"><span data-stu-id="18ecd-301">When hello export operation completes with either a status of success or failure, hello temporary file is deleted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18ecd-302">Import a Export je dostupná jenom pro prémiových mezipamětí vrstvy.</span><span class="sxs-lookup"><span data-stu-id="18ecd-302">Import/Export is only available for Premium tier caches.</span></span> <span data-ttu-id="18ecd-303">Další informace a pokyny najdete v tématu [importu a exportu dat ve službě Azure Redis Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-303">For more information and instructions, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

### <a name="reboot"></a><span data-ttu-id="18ecd-304">Restartování</span><span class="sxs-lookup"><span data-stu-id="18ecd-304">Reboot</span></span>
<span data-ttu-id="18ecd-305">Hello **restartovat** okno vám umožní tooreboot hello uzly svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-305">hello **Reboot** blade allows you tooreboot hello nodes of your cache.</span></span> <span data-ttu-id="18ecd-306">Tato možnost restartování umožňuje vám tootest Pokud dojde k selhání uzlu mezipaměti aplikace pro odolnost proti chybám.</span><span class="sxs-lookup"><span data-stu-id="18ecd-306">This reboot capability enables you tootest your application for resiliency if there is a failure of a cache node.</span></span>

![Restartování](./media/cache-configure/redis-cache-reboot.png)

<span data-ttu-id="18ecd-308">Pokud máte s povoleným clusteringem cache ve verzi premium, můžete vybrat které horizontálních tooreboot mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="18ecd-308">If you have a premium cache with clustering enabled, you can select which shards of hello cache tooreboot.</span></span>

![Restartování](./media/cache-configure/redis-cache-reboot-cluster.png)

<span data-ttu-id="18ecd-310">tooreboot jeden nebo více uzlů vaší mezipaměti, vyberte uzel hello potřeby a klikněte na tlačítko **restartovat**.</span><span class="sxs-lookup"><span data-stu-id="18ecd-310">tooreboot one or more nodes of your cache, select hello desired nodes and click **Reboot**.</span></span> <span data-ttu-id="18ecd-311">Pokud máte cache ve verzi premium s povoleným clusteringem, vyberte hello shard(s) tooreboot a pak klikněte na **restartovat**.</span><span class="sxs-lookup"><span data-stu-id="18ecd-311">If you have a premium cache with clustering enabled, select hello shard(s) tooreboot and then click **Reboot**.</span></span> <span data-ttu-id="18ecd-312">Po několika minutách hello restartování vybrané uzly a jsou zpět do režimu online několik minut později.</span><span class="sxs-lookup"><span data-stu-id="18ecd-312">After a few minutes, hello selected node(s) reboot, and are back online a few minutes later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18ecd-313">Restart je nyní k dispozici pro všechny cenové úrovně.</span><span class="sxs-lookup"><span data-stu-id="18ecd-313">Reboot is now available for all pricing tiers.</span></span> <span data-ttu-id="18ecd-314">Další informace a pokyny najdete v tématu [správy Azure Redis Cache - restartování](cache-administration.md#reboot).</span><span class="sxs-lookup"><span data-stu-id="18ecd-314">For more information and instructions, see [Azure Redis Cache administration - Reboot](cache-administration.md#reboot).</span></span>
> 
> 


## <a name="monitoring"></a><span data-ttu-id="18ecd-315">Monitorování</span><span class="sxs-lookup"><span data-stu-id="18ecd-315">Monitoring</span></span>

<span data-ttu-id="18ecd-316">Hello **monitorování** část vám tooconfigure diagnostiky a monitorování pro mezipaměť Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="18ecd-316">hello **Monitoring** section allows you tooconfigure diagnostics and monitoring for your Redis Cache.</span></span> <span data-ttu-id="18ecd-317">Další informace o Azure Redis Cache monitorování a Diagnostika, najdete v části [jak toomonitor Azure mezipaměti Redis](cache-how-to-monitor.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-317">For more information on Azure Redis Cache monitoring and diagnostics, see [How toomonitor Azure Redis Cache](cache-how-to-monitor.md).</span></span>

![Diagnostika](./media/cache-configure/redis-cache-diagnostics.png)

* [<span data-ttu-id="18ecd-319">Metriky pro redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-319">Redis metrics</span></span>](#redis-metrics)
* [<span data-ttu-id="18ecd-320">Pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="18ecd-320">Alert rules</span></span>](#alert-rules)
* [<span data-ttu-id="18ecd-321">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="18ecd-321">Diagnostics</span></span>](#diagnostics)

### <a name="redis-metrics"></a><span data-ttu-id="18ecd-322">Metriky pro redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-322">Redis metrics</span></span>
<span data-ttu-id="18ecd-323">Klikněte na tlačítko **Redis metriky** příliš[metriky zobrazit](cache-how-to-monitor.md#view-cache-metrics) ke svojí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-323">Click **Redis metrics** too[view metrics](cache-how-to-monitor.md#view-cache-metrics) for your cache.</span></span>

### <a name="alert-rules"></a><span data-ttu-id="18ecd-324">Pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="18ecd-324">Alert rules</span></span>

<span data-ttu-id="18ecd-325">Klikněte na tlačítko **výstrah pravidla** tooconfigure výstrahy založené na metriky pro Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="18ecd-325">Click **Alert rules** tooconfigure alerts based on Redis Cache metrics.</span></span> <span data-ttu-id="18ecd-326">Další informace najdete v tématu [výstrahy](cache-how-to-monitor.md#alerts).</span><span class="sxs-lookup"><span data-stu-id="18ecd-326">For more information, see [Alerts](cache-how-to-monitor.md#alerts).</span></span>

### <a name="diagnostics"></a><span data-ttu-id="18ecd-327">Diagnostika</span><span class="sxs-lookup"><span data-stu-id="18ecd-327">Diagnostics</span></span>

<span data-ttu-id="18ecd-328">Ve výchozím nastavení, mezipaměti metriky v Azure monitorování jsou [uchovávají po dobu 30 dnů](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) a odstraní.</span><span class="sxs-lookup"><span data-stu-id="18ecd-328">By default, cache metrics in Azure Monitor are [stored for 30 days](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) and then deleted.</span></span> <span data-ttu-id="18ecd-329">Klikněte na tlačítko toopersist metriky vaší mezipaměti po dobu delší než 30 dní, **diagnostiky** příliš[konfigurace účtu úložiště hello](cache-how-to-monitor.md#export-cache-metrics) používá toostore diagnostiku mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-329">toopersist your cache metrics for longer than 30 days, click **Diagnostics** too[configure hello storage account](cache-how-to-monitor.md#export-cache-metrics) used toostore cache diagnostics.</span></span>

>[!NOTE]
><span data-ttu-id="18ecd-330">V přidání tooarchiving toostorage vaší mezipaměti metriky, můžete také [stream je centra událostí tooan nebo poslat tooLog Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).</span><span class="sxs-lookup"><span data-stu-id="18ecd-330">In addition tooarchiving your cache metrics toostorage, you can also [stream them tooan Event hub or send them tooLog Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).</span></span>
>
>

## <a name="support--troubleshooting-settings"></a><span data-ttu-id="18ecd-331">Podporovat & řešení potíží s nastavení</span><span class="sxs-lookup"><span data-stu-id="18ecd-331">Support & troubleshooting settings</span></span>
<span data-ttu-id="18ecd-332">Hello nastavení v hello **podpory a řešení potíží s** části poskytují možnosti pro řešení problémů s mezipamětí.</span><span class="sxs-lookup"><span data-stu-id="18ecd-332">hello settings in hello **Support + troubleshooting** section provide you with options for resolving issues with your cache.</span></span>

![Podpora + řešení potíží](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [<span data-ttu-id="18ecd-334">Stav prostředků</span><span class="sxs-lookup"><span data-stu-id="18ecd-334">Resource health</span></span>](#resource-health)
* [<span data-ttu-id="18ecd-335">Nová žádost o podporu</span><span class="sxs-lookup"><span data-stu-id="18ecd-335">New support request</span></span>](#new-support-request)

### <a name="resource-health"></a><span data-ttu-id="18ecd-336">Stav prostředků</span><span class="sxs-lookup"><span data-stu-id="18ecd-336">Resource health</span></span>
<span data-ttu-id="18ecd-337">**Stav prostředku** sleduje prostředku a zjistíte, zda je spuštěn podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="18ecd-337">**Resource health** watches your resource and tells you if it's running as expected.</span></span> <span data-ttu-id="18ecd-338">Další informace o hello služby stavu prostředků Azure najdete v tématu [přehled stavu prostředků Azure](../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-338">For more information about hello Azure Resource health service, see [Azure Resource health overview](../resource-health/resource-health-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="18ecd-339">Stav prostředku je momentálně nemůže tooreport na dobrý stav instance služby Azure Redis Cache hostované ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-339">Resource health is currently unable tooreport on hello health of Azure Redis Cache instances hosted in a virtual network.</span></span> <span data-ttu-id="18ecd-340">Další informace najdete v tématu [všechny funkce mezipaměti fungují při hostování mezipaměti ve virtuální síti?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)</span><span class="sxs-lookup"><span data-stu-id="18ecd-340">For more information, see [Do all cache features work when hosting a cache in a VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)</span></span>
> 
> 

### <a name="new-support-request"></a><span data-ttu-id="18ecd-341">Nová žádost o podporu</span><span class="sxs-lookup"><span data-stu-id="18ecd-341">New support request</span></span>
<span data-ttu-id="18ecd-342">Klikněte na tlačítko **nová žádost o podporu** tooopen žádost o podporu pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="18ecd-342">Click **New support request** tooopen a support request for your cache.</span></span>





## <a name="default-redis-server-configuration"></a><span data-ttu-id="18ecd-343">Výchozí konfigurace serveru Redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-343">Default Redis server configuration</span></span>
<span data-ttu-id="18ecd-344">Nové instance služby Azure Redis Cache jsou nakonfigurovány s následující výchozí hodnoty konfigurace Redis hello.</span><span class="sxs-lookup"><span data-stu-id="18ecd-344">New Azure Redis Cache instances are configured with hello following default Redis configuration values.</span></span>

> [!NOTE]
> <span data-ttu-id="18ecd-345">Hello nastavení v této části nelze změnit pomocí hello `StackExchange.Redis.IServer.ConfigSet` metoda.</span><span class="sxs-lookup"><span data-stu-id="18ecd-345">hello settings in this section cannot be changed using hello `StackExchange.Redis.IServer.ConfigSet` method.</span></span> <span data-ttu-id="18ecd-346">Pokud tato metoda je volána s jedním z hello příkazy v této části, je vyvolána následující výjimce podobné toohello:</span><span class="sxs-lookup"><span data-stu-id="18ecd-346">If this method is called with one of hello commands in this section, an exception similar toohello following is thrown:</span></span>  
> 
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
> 
> <span data-ttu-id="18ecd-347">Všechny hodnoty, které se dají konfigurovat jako **max paměti policy**, se dají konfigurovat prostřednictvím hello portál Azure nebo nástroje příkazového řádku pro správu, například Azure CLI nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="18ecd-347">Any values that are configurable, such as **max-memory-policy**, are configurable through hello Azure portal or command-line management tools such as Azure CLI or PowerShell.</span></span>
> 
> 

| <span data-ttu-id="18ecd-348">Nastavení</span><span class="sxs-lookup"><span data-stu-id="18ecd-348">Setting</span></span> | <span data-ttu-id="18ecd-349">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="18ecd-349">Default value</span></span> | <span data-ttu-id="18ecd-350">Popis</span><span class="sxs-lookup"><span data-stu-id="18ecd-350">Description</span></span> |
| --- | --- | --- |
| `databases` |<span data-ttu-id="18ecd-351">16</span><span class="sxs-lookup"><span data-stu-id="18ecd-351">16</span></span> |<span data-ttu-id="18ecd-352">Hello výchozí počet databází je 16, ale můžete nakonfigurovat různých čísel na základě hello cenová úroveň. <sup>1</sup> hello výchozí databáze je databáze 0, můžete vybrat jinou na základě jednotlivých připojení pomocí `connection.GetDatabase(dbid)` kde `dbid` je číslo v rozsahu od `0` a `databases - 1`.</span><span class="sxs-lookup"><span data-stu-id="18ecd-352">hello default number of databases is 16 but you can configure a different number based on hello pricing tier.<sup>1</sup> hello default database is DB 0, you can select a different one on a per-connection basis using `connection.GetDatabase(dbid)` where `dbid` is a number between `0` and `databases - 1`.</span></span> |
| `maxclients` |<span data-ttu-id="18ecd-353">Závisí na hello cenová úroveň<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="18ecd-353">Depends on hello pricing tier<sup>2</sup></span></span> |<span data-ttu-id="18ecd-354">Toto je hello maximální počet připojených klientů, které jsou povolené v hello stejný čas.</span><span class="sxs-lookup"><span data-stu-id="18ecd-354">This is hello maximum number of connected clients allowed at hello same time.</span></span> <span data-ttu-id="18ecd-355">Po dosažení limitu hello Redis zavře všechny hello nová připojení, vrátíte k chybě 'maximální počet klientů dosaženo'.</span><span class="sxs-lookup"><span data-stu-id="18ecd-355">Once hello limit is reached Redis closes all hello new connections, returning a 'max number of clients reached' error.</span></span> |
| `maxmemory-policy` |`volatile-lru` |<span data-ttu-id="18ecd-356">Zásady Maxmemory je hello nastavení pro jak Redis vybere co tooremove při `maxmemory` je dosaženo (hello velikost mezipaměti hello nabídky, jste vybrali při vytvoření mezipaměti hello).</span><span class="sxs-lookup"><span data-stu-id="18ecd-356">Maxmemory policy is hello setting for how Redis selects what tooremove when `maxmemory` (hello size of hello cache offering you selected when you created hello cache) is reached.</span></span> <span data-ttu-id="18ecd-357">S Azure Redis Cache hello výchozí nastavení je `volatile-lru`, které odebere hello klíče s vypršení platnosti nastavit pomocí algoritmu hodnoty nejdelšího Nepoužití.</span><span class="sxs-lookup"><span data-stu-id="18ecd-357">With Azure Redis Cache hello default setting is `volatile-lru`, which removes hello keys with an expiration set using an LRU algorithm.</span></span> <span data-ttu-id="18ecd-358">Toto nastavení můžete nakonfigurovat v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="18ecd-358">This setting can be configured in hello Azure portal.</span></span> <span data-ttu-id="18ecd-359">Další informace najdete v tématu [paměti zásady](#memory-policies).</span><span class="sxs-lookup"><span data-stu-id="18ecd-359">For more information, see [Memory policies](#memory-policies).</span></span> |
| `maxmemory-samples` |<span data-ttu-id="18ecd-360">3</span><span class="sxs-lookup"><span data-stu-id="18ecd-360">3</span></span> |<span data-ttu-id="18ecd-361">paměť toosave, LRU a minimální hodnota TTL algoritmy jsou přibližný algoritmy místo přesné algoritmy.</span><span class="sxs-lookup"><span data-stu-id="18ecd-361">toosave memory, LRU and minimal TTL algorithms are approximated algorithms instead of precise algorithms.</span></span> <span data-ttu-id="18ecd-362">Ve výchozím nastavení Redis kontroly tři klíče a vyskladnění hello jeden použitou méně nedávno.</span><span class="sxs-lookup"><span data-stu-id="18ecd-362">By default Redis checks three keys and picks hello one that was used less recently.</span></span> |
| `lua-time-limit` |<span data-ttu-id="18ecd-363">5,000</span><span class="sxs-lookup"><span data-stu-id="18ecd-363">5,000</span></span> |<span data-ttu-id="18ecd-364">Maximální doba provádění skriptu Lua v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="18ecd-364">Max execution time of a Lua script in milliseconds.</span></span> <span data-ttu-id="18ecd-365">Když je dosaženo maximální dobu spuštění hello, zaznamená Redis, je stále v provádění po hello maximální povolená doba skript a tooreply tooqueries začíná k chybě.</span><span class="sxs-lookup"><span data-stu-id="18ecd-365">If hello maximum execution time is reached, Redis logs that a script is still in execution after hello maximum allowed time, and starts tooreply tooqueries with an error.</span></span> |
| `lua-event-limit` |<span data-ttu-id="18ecd-366">500</span><span class="sxs-lookup"><span data-stu-id="18ecd-366">500</span></span> |<span data-ttu-id="18ecd-367">Maximální velikost fronty událostí skriptu.</span><span class="sxs-lookup"><span data-stu-id="18ecd-367">Max size of script event queue.</span></span> |
| <span data-ttu-id="18ecd-368">`client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub`</span><span class="sxs-lookup"><span data-stu-id="18ecd-368">`client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub`</span></span> |<span data-ttu-id="18ecd-369">0 0 032mb 8mb 60</span><span class="sxs-lookup"><span data-stu-id="18ecd-369">0 0 032mb 8mb 60</span></span> |<span data-ttu-id="18ecd-370">Hello klienta výstupní vyrovnávací paměť omezení může být použit tooforce odpojení klientů, které nejsou čtení dat ze serveru hello dostatečně rychle z nějakého důvodu (obvyklým důvodem je, že klienta Pub nebo Sub tak rychle, jak je může vytvořit vydavatele hello nelze využívat zprávy).</span><span class="sxs-lookup"><span data-stu-id="18ecd-370">hello client output buffer limits can be used tooforce disconnection of clients that are not reading data from hello server fast enough for some reason (a common reason is that a Pub/Sub client can't consume messages as fast as hello publisher can produce them).</span></span> <span data-ttu-id="18ecd-371">Další informace najdete v tématu [http://redis.io/topics/clients](http://redis.io/topics/clients).</span><span class="sxs-lookup"><span data-stu-id="18ecd-371">For more information, see [http://redis.io/topics/clients](http://redis.io/topics/clients).</span></span> |

<span data-ttu-id="18ecd-372"><a name="databases"></a>
<sup>1</sup>hello limit pro `databases` se liší pro každý Azure Redis Cache cenová úroveň a můžete nastavit při vytváření mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-372"><a name="databases"></a>
<sup>1</sup>hello limit for `databases` is different for each Azure Redis Cache pricing tier and can be set at cache creation.</span></span> <span data-ttu-id="18ecd-373">Pokud žádné `databases` nastavení zadat během vytváření mezipaměti hello výchozí hodnota je 16.</span><span class="sxs-lookup"><span data-stu-id="18ecd-373">If no `databases` setting is specified during cache creation, hello default is 16.</span></span>

* <span data-ttu-id="18ecd-374">Základní a standardní mezipaměti</span><span class="sxs-lookup"><span data-stu-id="18ecd-374">Basic and Standard caches</span></span>
  * <span data-ttu-id="18ecd-375">C0 (250 MB) mezipaměti - až too16 databáze</span><span class="sxs-lookup"><span data-stu-id="18ecd-375">C0 (250 MB) cache - up too16 databases</span></span>
  * <span data-ttu-id="18ecd-376">C1 mezipaměti (1 GB) – až too16 databáze</span><span class="sxs-lookup"><span data-stu-id="18ecd-376">C1 (1 GB) cache - up too16 databases</span></span>
  * <span data-ttu-id="18ecd-377">C2 mezipaměti (2,5 GB) – až too16 databáze</span><span class="sxs-lookup"><span data-stu-id="18ecd-377">C2 (2.5 GB) cache - up too16 databases</span></span>
  * <span data-ttu-id="18ecd-378">C3 (6 GB) mezipaměti - až too16 databáze</span><span class="sxs-lookup"><span data-stu-id="18ecd-378">C3 (6 GB) cache - up too16 databases</span></span>
  * <span data-ttu-id="18ecd-379">C4 (13 GB) mezipaměti - až too32 databáze</span><span class="sxs-lookup"><span data-stu-id="18ecd-379">C4 (13 GB) cache - up too32 databases</span></span>
  * <span data-ttu-id="18ecd-380">C5 (26 GB) mezipaměti - až too48 databáze</span><span class="sxs-lookup"><span data-stu-id="18ecd-380">C5 (26 GB) cache - up too48 databases</span></span>
  * <span data-ttu-id="18ecd-381">C6 (53 GB) mezipaměti - až too64 databáze</span><span class="sxs-lookup"><span data-stu-id="18ecd-381">C6 (53 GB) cache - up too64 databases</span></span>
* <span data-ttu-id="18ecd-382">Ukládá do mezipaměti Premium</span><span class="sxs-lookup"><span data-stu-id="18ecd-382">Premium caches</span></span>
  * <span data-ttu-id="18ecd-383">P1 (6 GB - 60 GB) – až too16 databáze</span><span class="sxs-lookup"><span data-stu-id="18ecd-383">P1 (6 GB - 60 GB) - up too16 databases</span></span>
  * <span data-ttu-id="18ecd-384">P2 (13 GB - 130 GB) – až too32 databáze</span><span class="sxs-lookup"><span data-stu-id="18ecd-384">P2 (13 GB - 130 GB) - up too32 databases</span></span>
  * <span data-ttu-id="18ecd-385">P3 (26 GB - 260 GB) – až too48 databáze</span><span class="sxs-lookup"><span data-stu-id="18ecd-385">P3 (26 GB - 260 GB) - up too48 databases</span></span>
  * <span data-ttu-id="18ecd-386">P4 (53 GB - 530 GB) – až too64 databáze</span><span class="sxs-lookup"><span data-stu-id="18ecd-386">P4 (53 GB - 530 GB) - up too64 databases</span></span>
  * <span data-ttu-id="18ecd-387">Všechny prémiových mezipamětí s clusterem Redis povoleny – Redis clusteru podporuje použití databáze 0 tak hello `databases` omezení pro všechny premium mezipaměť Redis clusteru povolena je efektivně 1 a hello [vyberte](http://redis.io/commands/select) není povolen příkaz.</span><span class="sxs-lookup"><span data-stu-id="18ecd-387">All premium caches with Redis cluster enabled - Redis cluster only supports use of database 0 so hello `databases` limit for any premium cache with Redis cluster enabled is effectively 1 and hello [Select](http://redis.io/commands/select) command is not allowed.</span></span> <span data-ttu-id="18ecd-388">Další informace najdete v tématu [potřebuji toomake žádné změny toomy klienta aplikace toouse clustering?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)</span><span class="sxs-lookup"><span data-stu-id="18ecd-388">For more information, see [Do I need toomake any changes toomy client application toouse clustering?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)</span></span>

<span data-ttu-id="18ecd-389">Další informace o databáze najdete v tématu [co jsou databáze Redis?](cache-faq.md#what-are-redis-databases)</span><span class="sxs-lookup"><span data-stu-id="18ecd-389">For more information about databases, see [What are Redis databases?](cache-faq.md#what-are-redis-databases)</span></span>

> [!NOTE]
> <span data-ttu-id="18ecd-390">Hello `databases` nastavení může být nakonfigurovaný jenom během vytváření mezipaměti a pouze pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiných klientů pro správu.</span><span class="sxs-lookup"><span data-stu-id="18ecd-390">hello `databases` setting can be configured only during cache creation and only using PowerShell, CLI, or other management clients.</span></span> <span data-ttu-id="18ecd-391">Příklad konfigurace `databases` během vytváření mezipaměti pomocí prostředí PowerShell, najdete v části [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).</span><span class="sxs-lookup"><span data-stu-id="18ecd-391">For an example of configuring `databases` during cache creation using PowerShell, see [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).</span></span>
> 
> 

<span data-ttu-id="18ecd-392"><a name="maxclients"></a>
<sup>2</sup> `maxclients` se liší pro každý Azure Redis Cache cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="18ecd-392"><a name="maxclients"></a>
<sup>2</sup>`maxclients` is different for each Azure Redis Cache pricing tier.</span></span>

* <span data-ttu-id="18ecd-393">Základní a standardní mezipaměti</span><span class="sxs-lookup"><span data-stu-id="18ecd-393">Basic and Standard caches</span></span>
  * <span data-ttu-id="18ecd-394">C0 (250 MB) mezipaměti - až too256 připojení</span><span class="sxs-lookup"><span data-stu-id="18ecd-394">C0 (250 MB) cache - up too256 connections</span></span>
  * <span data-ttu-id="18ecd-395">C1 mezipaměti (1 GB) – až too1, 000 připojení</span><span class="sxs-lookup"><span data-stu-id="18ecd-395">C1 (1 GB) cache - up too1,000 connections</span></span>
  * <span data-ttu-id="18ecd-396">C2 mezipaměti (2,5 GB) – až too2, 000 připojení</span><span class="sxs-lookup"><span data-stu-id="18ecd-396">C2 (2.5 GB) cache - up too2,000 connections</span></span>
  * <span data-ttu-id="18ecd-397">C3 (6 GB) mezipaměti - až too5, 000 připojení</span><span class="sxs-lookup"><span data-stu-id="18ecd-397">C3 (6 GB) cache - up too5,000 connections</span></span>
  * <span data-ttu-id="18ecd-398">C4 (13 GB) mezipaměti - až too10, 000 připojení</span><span class="sxs-lookup"><span data-stu-id="18ecd-398">C4 (13 GB) cache - up too10,000 connections</span></span>
  * <span data-ttu-id="18ecd-399">C5 (26 GB) mezipaměti - až too15, 000 připojení</span><span class="sxs-lookup"><span data-stu-id="18ecd-399">C5 (26 GB) cache - up too15,000 connections</span></span>
  * <span data-ttu-id="18ecd-400">C6 (53 GB) mezipaměti - až too20, 000 připojení</span><span class="sxs-lookup"><span data-stu-id="18ecd-400">C6 (53 GB) cache - up too20,000 connections</span></span>
* <span data-ttu-id="18ecd-401">Ukládá do mezipaměti Premium</span><span class="sxs-lookup"><span data-stu-id="18ecd-401">Premium caches</span></span>
  * <span data-ttu-id="18ecd-402">P1 (6 GB - 60 GB) – až too7, 500 připojení</span><span class="sxs-lookup"><span data-stu-id="18ecd-402">P1 (6 GB - 60 GB) - up too7,500 connections</span></span>
  * <span data-ttu-id="18ecd-403">P2 (13 GB - 130 GB) – až too15 000 připojení</span><span class="sxs-lookup"><span data-stu-id="18ecd-403">P2 (13 GB - 130 GB) - up too15,000 connections</span></span>
  * <span data-ttu-id="18ecd-404">P3 (26 GB - 260 GB) – až too30 000 připojení</span><span class="sxs-lookup"><span data-stu-id="18ecd-404">P3 (26 GB - 260 GB) - up too30,000 connections</span></span>
  * <span data-ttu-id="18ecd-405">P4 (53 GB - 530 GB) – až too40 000 připojení</span><span class="sxs-lookup"><span data-stu-id="18ecd-405">P4 (53 GB - 530 GB) - up too40,000 connections</span></span>

> [!NOTE]
> <span data-ttu-id="18ecd-406">Při každém velikost mezipaměti umožňuje *až* určitý počet připojení, každý tooRedis připojení má režie spojená s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="18ecd-406">While each size of cache allows *up to* a certain number of connections, each connection tooRedis has overhead associated with it.</span></span> <span data-ttu-id="18ecd-407">Příkladem takových režie může být využití procesoru a paměti v důsledku šifrování pomocí protokolu TLS/SSL.</span><span class="sxs-lookup"><span data-stu-id="18ecd-407">An example of such overhead would be CPU and memory usage as a result of TLS/SSL encryption.</span></span> <span data-ttu-id="18ecd-408">Hello připojení maximální limit pro velikost daného mezipaměti předpokládá lehkým načíst mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-408">hello maximum connection limit for a given cache size assumes a lightly loaded cache.</span></span> <span data-ttu-id="18ecd-409">Pokud načíst z režijní náklady na připojení *plus* zatížení z klientské operace překročí kapacitu pro systém hello, hello mezipaměti můžete mít problémy kapacity, i pokud nebyl překročen limit pro aktuální velikost mezipaměti hello hello připojení.</span><span class="sxs-lookup"><span data-stu-id="18ecd-409">If load from connection overhead *plus* load from client operations exceeds capacity for hello system, hello cache can experience capacity issues even if you have not exceeded hello connection limit for hello current cache size.</span></span>
> 
> 



## <a name="redis-commands-not-supported-in-azure-redis-cache"></a><span data-ttu-id="18ecd-410">Redis příkazy nejsou podporované ve službě Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="18ecd-410">Redis commands not supported in Azure Redis Cache</span></span>
> [!IMPORTANT]
> <span data-ttu-id="18ecd-411">Vzhledem k tomu, že konfigurace a Správa instance služby Azure Redis Cache spravuje Microsoft, hello následující příkazy jsou zakázány.</span><span class="sxs-lookup"><span data-stu-id="18ecd-411">Because configuration and management of Azure Redis Cache instances is managed by Microsoft, hello following commands are disabled.</span></span> <span data-ttu-id="18ecd-412">Pokud se pokusíte tooinvoke je příliš zobrazí chybová zpráva`"(error) ERR unknown command"`.</span><span class="sxs-lookup"><span data-stu-id="18ecd-412">If you try tooinvoke them, you receive an error message similar too`"(error) ERR unknown command"`.</span></span>
> 
> * <span data-ttu-id="18ecd-413">BGREWRITEAOF</span><span class="sxs-lookup"><span data-stu-id="18ecd-413">BGREWRITEAOF</span></span>
> * <span data-ttu-id="18ecd-414">BGSAVE</span><span class="sxs-lookup"><span data-stu-id="18ecd-414">BGSAVE</span></span>
> * <span data-ttu-id="18ecd-415">KONFIGURACE</span><span class="sxs-lookup"><span data-stu-id="18ecd-415">CONFIG</span></span>
> * <span data-ttu-id="18ecd-416">LADĚNÍ</span><span class="sxs-lookup"><span data-stu-id="18ecd-416">DEBUG</span></span>
> * <span data-ttu-id="18ecd-417">MIGRACE</span><span class="sxs-lookup"><span data-stu-id="18ecd-417">MIGRATE</span></span>
> * <span data-ttu-id="18ecd-418">ULOŽIT</span><span class="sxs-lookup"><span data-stu-id="18ecd-418">SAVE</span></span>
> * <span data-ttu-id="18ecd-419">VYPNUTÍ</span><span class="sxs-lookup"><span data-stu-id="18ecd-419">SHUTDOWN</span></span>
> * <span data-ttu-id="18ecd-420">SLAVEOF</span><span class="sxs-lookup"><span data-stu-id="18ecd-420">SLAVEOF</span></span>
> * <span data-ttu-id="18ecd-421">CLUSTER - clusteru zápisu, které jsou příkazy, ale jen pro čtení clusteru příkazy jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="18ecd-421">CLUSTER - Cluster write commands are disabled, but read-only Cluster commands are permitted.</span></span>
> 
> 

<span data-ttu-id="18ecd-422">Další informace o příkazech Redis najdete v tématu [http://redis.io/commands](http://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="18ecd-422">For more information about Redis commands, see [http://redis.io/commands](http://redis.io/commands).</span></span>

## <a name="redis-console"></a><span data-ttu-id="18ecd-423">Konzola redis</span><span class="sxs-lookup"><span data-stu-id="18ecd-423">Redis console</span></span>
<span data-ttu-id="18ecd-424">Můžete bezpečně vydávat příkazy tooyour instancí Azure Redis Cache pomocí hello **konzola Redis**, která je dostupná v hello portál Azure pro všechny úrovně mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-424">You can securely issue commands tooyour Azure Redis Cache instances using hello **Redis Console**, which is available in hello Azure portal for all cache tiers.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="18ecd-425">Hello Redis konzola nefunguje s [VNET](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-425">hello Redis Console does not work with [VNET](cache-how-to-premium-vnet.md).</span></span> <span data-ttu-id="18ecd-426">Pokud vaše mezipaměť je součástí virtuální sítě, můžete přístup jenom pro klienty v hello virtuální síť hello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-426">When your cache is part of a VNET, only clients in hello VNET can access hello cache.</span></span> <span data-ttu-id="18ecd-427">Vzhledem k tomu konzola Redis běží v prohlížeči místní, což je mimo hello virtuální sítě, se nemůže připojit tooyour mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-427">Because Redis Console runs in your local browser, which is outside hello VNET, it can't connect tooyour cache.</span></span>
> - <span data-ttu-id="18ecd-428">Ne všechny příkazy Redis jsou podporovány ve službě Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="18ecd-428">Not all Redis commands are supported in Azure Redis Cache.</span></span> <span data-ttu-id="18ecd-429">Seznam příkazů Redis, kteří jsou zakázáni pro Azure Redis Cache najdete v tématu hello předchozí [Redis příkazy nejsou podporované ve službě Azure Redis Cache](#redis-commands-not-supported-in-azure-redis-cache) části.</span><span class="sxs-lookup"><span data-stu-id="18ecd-429">For a list of Redis commands that are disabled for Azure Redis Cache, see hello previous [Redis commands not supported in Azure Redis Cache](#redis-commands-not-supported-in-azure-redis-cache) section.</span></span> <span data-ttu-id="18ecd-430">Další informace o příkazech Redis najdete v tématu [http://redis.io/commands](http://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="18ecd-430">For more information about Redis commands, see [http://redis.io/commands](http://redis.io/commands).</span></span>
> 
> 

<span data-ttu-id="18ecd-431">Klikněte na tlačítko tooaccess hello konzola Redis **konzoly** z hello **Redis Cache** okno.</span><span class="sxs-lookup"><span data-stu-id="18ecd-431">tooaccess hello Redis Console, click **Console** from hello **Redis Cache** blade.</span></span>

![Konzola redis](./media/cache-configure/redis-console-menu.png)

<span data-ttu-id="18ecd-433">příkazy tooissue proti instance mezipaměti, jednoduše zadejte hello požadovaný příkaz do konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="18ecd-433">tooissue commands against your cache instance, simply type hello desired command into hello console.</span></span>

![Konzola redis](./media/cache-configure/redis-console.png)


### <a name="using-hello-redis-console-with-a-premium-clustered-cache"></a><span data-ttu-id="18ecd-435">Pomocí hello konzola Redis se prémiový mezipaměť clusteru</span><span class="sxs-lookup"><span data-stu-id="18ecd-435">Using hello Redis Console with a premium clustered cache</span></span>

<span data-ttu-id="18ecd-436">Když mezipaměť hello konzola Redis pomocí prémiový clusteru, můžete použít příkazy tooa jeden horizontálních hello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="18ecd-436">When using hello Redis Console with a premium clustered cache, you can issue commands tooa single shard of hello cache.</span></span> <span data-ttu-id="18ecd-437">tooissue příkaz tooa konkrétní horizontálního oddílu, nejdřív připojit požadované horizontálního oddílu toohello kliknutím na výběr hello horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="18ecd-437">tooissue a command tooa specific shard, first connect toohello desired shard by clicking it on hello shard picker.</span></span>

![Konzola redis](./media/cache-configure/redis-console-premium-cluster.png)

<span data-ttu-id="18ecd-439">Když zkusíte tooaccess klíč, který je uložen v různých horizontálního oddílu než hello připojené horizontálního oddílu, obdržíte chybové zprávy podobné toohello následující zprávou.</span><span class="sxs-lookup"><span data-stu-id="18ecd-439">If you attempt tooaccess a key that is stored in a different shard than hello connected shard, you receive an error message similar toohello following message.</span></span>

```
shard1>get myKey
(error) MOVED 866 13.90.202.154:13000 (shard 0)
```

<span data-ttu-id="18ecd-440">V předchozím příkladu hello horizontálního oddílu 1 je hello vybrané horizontálního oddílu, ale `myKey` se nachází v horizontálního oddílu 0, jak je indikován hello `(shard 0)` část hello chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="18ecd-440">In hello previous example, shard 1 is hello selected shard, but `myKey` is located in shard 0, as indicated by hello `(shard 0)` portion of hello error message.</span></span> <span data-ttu-id="18ecd-441">V tomto příkladu tooaccess `myKey`, vyberte pomocí horizontálního oddílu 0 hello horizontálního oddílu výběr a potom příkaz hello potřeby problém.</span><span class="sxs-lookup"><span data-stu-id="18ecd-441">In this example, tooaccess `myKey`, select shard 0 using hello shard picker, and then issue hello desired command.</span></span>


## <a name="move-your-cache-tooa-new-subscription"></a><span data-ttu-id="18ecd-442">Přesunout nového předplatného tooa mezipaměti</span><span class="sxs-lookup"><span data-stu-id="18ecd-442">Move your cache tooa new subscription</span></span>
<span data-ttu-id="18ecd-443">Kliknutím můžete přesunout nového předplatného tooa mezipaměti **přesunout**.</span><span class="sxs-lookup"><span data-stu-id="18ecd-443">You can move your cache tooa new subscription by clicking **Move**.</span></span>

![Přesunout mezipaměti Redis](./media/cache-configure/redis-cache-move.png)

<span data-ttu-id="18ecd-445">Informace o přesun prostředků z jednoho prostředku skupiny tooanother a z tooanother jedno předplatné, najdete v části [přesunout skupiny prostředků toonew prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="18ecd-445">For information on moving resources from one resource group tooanother, and from one subscription tooanother, see [Move resources toonew resource group or subscription](../azure-resource-manager/resource-group-move-resources.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="18ecd-446">Další kroky</span><span class="sxs-lookup"><span data-stu-id="18ecd-446">Next steps</span></span>
* <span data-ttu-id="18ecd-447">Další informace o práci s příkazy Redis najdete v tématu [jak můžete spouštět příkazy Redis?](cache-faq.md#how-can-i-run-redis-commands)</span><span class="sxs-lookup"><span data-stu-id="18ecd-447">For more information on working with Redis commands, see [How can I run Redis commands?](cache-faq.md#how-can-i-run-redis-commands)</span></span>

