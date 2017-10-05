---
title: "Limity předplatného Azure a kvóty | Microsoft Docs"
description: "Poskytuje seznam běžných předplatného Azure a omezení služby, kvóty a omezení. To zahrnuje informace o tom, jak zvýšit omezení spolu s maximální hodnoty."
services: 
documentationcenter: 
author: rothja
manager: jeffreyg
editor: 
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: byvinyal
ms.openlocfilehash: a76acd67e9ba7822f2837b3c08e2ede389047f11
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a><span data-ttu-id="1a6dc-104">Limity, kvóty a omezení předplatného a služeb Azure</span><span class="sxs-lookup"><span data-stu-id="1a6dc-104">Azure subscription and service limits, quotas, and constraints</span></span>
<span data-ttu-id="1a6dc-105">Tento dokument uvádí některé z nejběžnějších omezení Microsoft Azure, což se taky někdy označují jako kvóty.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-105">This document lists some of the most common Microsoft Azure limits, which are also sometimes called quotas.</span></span> <span data-ttu-id="1a6dc-106">Tento dokument nepokrývá aktuálně všech služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-106">This document doesn't currently cover all Azure services.</span></span> <span data-ttu-id="1a6dc-107">V čase v seznamu rozbalit a aktualizovat tak, aby pokrývalo více platformou.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-107">Over time, the list will be expanded and updated to cover more of the platform.</span></span>

<span data-ttu-id="1a6dc-108">Navštivte [ceny přehled Azure](https://azure.microsoft.com/pricing/) Další informace o cenách služby Azure.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-108">Please visit [Azure Pricing Overview](https://azure.microsoft.com/pricing/) to learn more about Azure pricing.</span></span> <span data-ttu-id="1a6dc-109">Zde můžete odhadnout náklady na používání [cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/) nebo když přejdete na stránce s cenami podrobnosti pro službu (například [virtuálních počítačů Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).</span><span class="sxs-lookup"><span data-stu-id="1a6dc-109">There, you can estimate your costs using the [Pricing Calculator](https://azure.microsoft.com/pricing/calculator/) or by visiting the pricing details page for a service (for example, [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).</span></span> <span data-ttu-id="1a6dc-110">Tipy ke správě náklady na najdete v tématu [zabránit neočekávané náklady s Azure fakturace a náklady na správu](billing/billing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="1a6dc-110">For tips to help manage your costs, see [Prevent unexpected costs with Azure billing and cost management](billing/billing-getting-started.md).</span></span>

> [!NOTE]
> <span data-ttu-id="1a6dc-111">Pokud chcete zvýšit limit nebo kvóty výše **výchozí Limit**, [otevřete žádosti o podporu online zákazníka zdarma](azure-supportability/resource-manager-core-quotas-request.md).</span><span class="sxs-lookup"><span data-stu-id="1a6dc-111">If you want to raise the limit or quota above the **Default Limit**, [open an online customer support request at no charge](azure-supportability/resource-manager-core-quotas-request.md).</span></span> <span data-ttu-id="1a6dc-112">Není možné zvýšit limity výše **maximální Limit** hodnota použitá v následujících tabulkách.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-112">The limits can't be raised above the **Maximum Limit** value shown in the following tables.</span></span> <span data-ttu-id="1a6dc-113">Pokud není žádná **maximální Limit** sloupec a potom prostředek nemá nastavitelná omezení.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-113">If there is no **Maximum Limit** column, then the resource doesn't have adjustable limits.</span></span> 
> 
> <span data-ttu-id="1a6dc-114">Bezplatné předplatné zkušební verze nejsou způsobilé k omezení nebo zvyšuje kvóty.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-114">Free Trial subscriptions are not eligible for limit or quota increases.</span></span> <span data-ttu-id="1a6dc-115">Pokud máte bezplatnou zkušební verzi, můžete upgradovat na [průběžné platby](https://azure.microsoft.com/offers/ms-azr-0003p/) předplatné.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-115">If you have a Free Trial, you can upgrade to a [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) subscription.</span></span> <span data-ttu-id="1a6dc-116">Další informace najdete v tématu [Upgrade bezplatné zkušební verze Azure na průběžné platby](billing/billing-upgrade-azure-subscription.md).</span><span class="sxs-lookup"><span data-stu-id="1a6dc-116">For more information, see [Upgrade Azure Free Trial to Pay-As-You-Go](billing/billing-upgrade-azure-subscription.md).</span></span>
> 

## <a name="limits-and-the-azure-resource-manager"></a><span data-ttu-id="1a6dc-117">Omezení a Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1a6dc-117">Limits and the Azure Resource Manager</span></span>
<span data-ttu-id="1a6dc-118">Nyní je možné kombinovat více prostředků Azure v do jedné skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-118">It is now possible to combine multiple Azure resources in to a single Azure Resource Group.</span></span> <span data-ttu-id="1a6dc-119">Při použití skupin prostředků, omezení, které byly jednou globální spravovanou na místní úrovni s Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-119">When using Resource Groups, limits that once were global become managed at a regional level with the Azure Resource Manager.</span></span> <span data-ttu-id="1a6dc-120">Další informace o skupinách prostředků Azure najdete v tématu [přehled Azure Resource Manageru](azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1a6dc-120">For more information about Azure Resource Groups, see [Azure Resource Manager overview](azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="1a6dc-121">V níže uvedené limity se přidal nové tabulky, aby odrážela případné rozdíly v omezení při použití Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-121">In the limits below, a new table has been added to reflect any differences in limits when using the Azure Resource Manager.</span></span> <span data-ttu-id="1a6dc-122">Například, že je **limity předplatného** tabulky a **limity předplatného – Azure Resource Manager** tabulky.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-122">For example, there is a **Subscription Limits** table and a **Subscription Limits - Azure Resource Manager** table.</span></span> <span data-ttu-id="1a6dc-123">Když omezení platí pro oba scénáře, se zobrazí pouze v první tabulce.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-123">When a limit applies to both scenarios, it is only shown in the first table.</span></span> <span data-ttu-id="1a6dc-124">Pokud není uvedeno jinak, jsou omezení globální přes všechny oblasti.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-124">Unless otherwise indicated, limits are global across all regions.</span></span>

> [!NOTE]
> <span data-ttu-id="1a6dc-125">Je nutné zdůraznit, že jsou na oblast přístupný pro vaše předplatné kvóty pro prostředky ve skupinách prostředků Azure a nejsou za předplatné, jako jsou kvóty správy služby.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-125">It is important to emphasize that quotas for resources in Azure Resource Groups are per-region accessible by your subscription, and are not per-subscription, as the service management quotas are.</span></span> <span data-ttu-id="1a6dc-126">Jako příklad použijeme základní kvóty.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-126">Let's use core quotas as an example.</span></span> <span data-ttu-id="1a6dc-127">Pokud potřebujete požádat o zvýšení kvóty s podporou pro počet jader, musíte se rozhodnout, kolik jader, kterou chcete použít v oblasti, které a pak proveďte konkrétního požadavku pro skupiny prostředků Azure základní kvóty pro částky a oblastí, které chcete.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-127">If you need to request a quota increase with support for cores, you need to decide how many cores you want to use in which regions, and then make a specific request for Azure Resource Group core quotas for the amounts and regions that you want.</span></span> <span data-ttu-id="1a6dc-128">Proto v případě, že budete muset použít 30 jader v oblasti západní Evropa spusťte aplikaci; Konkrétně měli požádat o 30 jader v oblasti západní Evropa.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-128">Therefore, if you need to use 30 cores in West Europe to run your application there; you should specifically request 30 cores in West Europe.</span></span> <span data-ttu-id="1a6dc-129">Ale nebudete mít základní kvóta zvyšují v jiné oblasti – pouze západní Evropa budou mít kvótu 30 jádra.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-129">But you will not have a core quota increase in any other region -- only West Europe will have the 30-core quota.</span></span>
> <!-- -->
> <span data-ttu-id="1a6dc-130">V důsledku toho může být vhodné vzít v úvahu při rozhodování o tom, co kvóty vaší skupiny prostředků Azure musí být pro úlohy v libovolné oblasti jeden a požadovat tato částka v každé oblasti, do kterého uvažujete o nasazení.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-130">As a result, you may find it useful to consider deciding what your Azure Resource Group quotas need to be for your workload in any one region, and request that amount in each region into which you are considering deployment.</span></span> <span data-ttu-id="1a6dc-131">V tématu [potíží s nasazením](resource-manager-common-deployment-errors.md) další pomoc zjišťování vaše aktuální kvóty pro konkrétní oblasti.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-131">See [troubleshooting deployment issues](resource-manager-common-deployment-errors.md) for more help discovering your current quotas for specific regions.</span></span>
> 
> 

## <a name="service-specific-limits"></a><span data-ttu-id="1a6dc-132">Omezení specifickou pro službu</span><span class="sxs-lookup"><span data-stu-id="1a6dc-132">Service-specific limits</span></span>
* [<span data-ttu-id="1a6dc-133">Active Directory</span><span class="sxs-lookup"><span data-stu-id="1a6dc-133">Active Directory</span></span>](#active-directory-limits)
* [<span data-ttu-id="1a6dc-134">API Management</span><span class="sxs-lookup"><span data-stu-id="1a6dc-134">API Management</span></span>](#api-management-limits)
* [<span data-ttu-id="1a6dc-135">App Service</span><span class="sxs-lookup"><span data-stu-id="1a6dc-135">App Service</span></span>](#app-service-limits)
* [<span data-ttu-id="1a6dc-136">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="1a6dc-136">Application Gateway</span></span>](#application-gateway-limits)
* [<span data-ttu-id="1a6dc-137">Application Insights</span><span class="sxs-lookup"><span data-stu-id="1a6dc-137">Application Insights</span></span>](#application-insights-limits)
* [<span data-ttu-id="1a6dc-138">Automation</span><span class="sxs-lookup"><span data-stu-id="1a6dc-138">Automation</span></span>](#automation-limits)
* [<span data-ttu-id="1a6dc-139">Databáze Azure Cosmos</span><span class="sxs-lookup"><span data-stu-id="1a6dc-139">Azure Cosmos DB</span></span>](#azure-cosmos-db-limits)
* [<span data-ttu-id="1a6dc-140">Azure událostí mřížky</span><span class="sxs-lookup"><span data-stu-id="1a6dc-140">Azure Event Grid</span></span>](#azure-event-grid-limits)
* [<span data-ttu-id="1a6dc-141">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="1a6dc-141">Azure Redis Cache</span></span>](#azure-redis-cache-limits)
* [<span data-ttu-id="1a6dc-142">Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="1a6dc-142">Azure RemoteApp</span></span>](#azure-remoteapp-limits)
* [<span data-ttu-id="1a6dc-143">Backup</span><span class="sxs-lookup"><span data-stu-id="1a6dc-143">Backup</span></span>](#backup-limits)
* [<span data-ttu-id="1a6dc-144">Batch</span><span class="sxs-lookup"><span data-stu-id="1a6dc-144">Batch</span></span>](#batch-limits)
* [<span data-ttu-id="1a6dc-145">BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="1a6dc-145">BizTalk Services</span></span>](#biztalk-services-limits)
* [<span data-ttu-id="1a6dc-146">CDN</span><span class="sxs-lookup"><span data-stu-id="1a6dc-146">CDN</span></span>](#cdn-limits)
* [<span data-ttu-id="1a6dc-147">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="1a6dc-147">Cloud Services</span></span>](#cloud-services-limits)
* [<span data-ttu-id="1a6dc-148">Container Instances</span><span class="sxs-lookup"><span data-stu-id="1a6dc-148">Container Instances</span></span>](#container-instances-limits)
* [<span data-ttu-id="1a6dc-149">Data Factory</span><span class="sxs-lookup"><span data-stu-id="1a6dc-149">Data Factory</span></span>](#data-factory-limits)
* [<span data-ttu-id="1a6dc-150">Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="1a6dc-150">Data Lake Analytics</span></span>](#data-lake-analytics-limits)
* [<span data-ttu-id="1a6dc-151">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="1a6dc-151">Data Lake Store</span></span>](#data-lake-store-limits)
* [<span data-ttu-id="1a6dc-152">DNS</span><span class="sxs-lookup"><span data-stu-id="1a6dc-152">DNS</span></span>](#dns-limits)
* [<span data-ttu-id="1a6dc-153">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="1a6dc-153">Event Hubs</span></span>](#event-hubs-limits)
* [<span data-ttu-id="1a6dc-154">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="1a6dc-154">IoT Hub</span></span>](#iot-hub-limits)
* [<span data-ttu-id="1a6dc-155">Key Vault</span><span class="sxs-lookup"><span data-stu-id="1a6dc-155">Key Vault</span></span>](#key-vault-limits)
* [<span data-ttu-id="1a6dc-156">Analýza protokolu / provozní statistiky</span><span class="sxs-lookup"><span data-stu-id="1a6dc-156">Log Analytics / Operational Insights</span></span>](#log-analytics-limits)
* [<span data-ttu-id="1a6dc-157">Media Services</span><span class="sxs-lookup"><span data-stu-id="1a6dc-157">Media Services</span></span>](#media-services-limits)
* [<span data-ttu-id="1a6dc-158">Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="1a6dc-158">Mobile Engagement</span></span>](#mobile-engagement-limits)
* [<span data-ttu-id="1a6dc-159">Mobilní služby</span><span class="sxs-lookup"><span data-stu-id="1a6dc-159">Mobile Services</span></span>](#mobile-services-limits)
* [<span data-ttu-id="1a6dc-160">Monitorování</span><span class="sxs-lookup"><span data-stu-id="1a6dc-160">Monitor</span></span>](#monitor-limits)
* [<span data-ttu-id="1a6dc-161">Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="1a6dc-161">Multi-Factor Authentication</span></span>](#multi-factor-authentication)
* [<span data-ttu-id="1a6dc-162">Sítě</span><span class="sxs-lookup"><span data-stu-id="1a6dc-162">Networking</span></span>](#networking-limits)
* [<span data-ttu-id="1a6dc-163">Sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="1a6dc-163">Network Watcher</span></span>](#network-watcher-limits)
* [<span data-ttu-id="1a6dc-164">Služby centra oznámení</span><span class="sxs-lookup"><span data-stu-id="1a6dc-164">Notification Hub Service</span></span>](#notification-hub-service-limits)
* [<span data-ttu-id="1a6dc-165">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="1a6dc-165">Resource Group</span></span>](#resource-group-limits)
* [<span data-ttu-id="1a6dc-166">Scheduler</span><span class="sxs-lookup"><span data-stu-id="1a6dc-166">Scheduler</span></span>](#scheduler-limits)
* [<span data-ttu-id="1a6dc-167">Search</span><span class="sxs-lookup"><span data-stu-id="1a6dc-167">Search</span></span>](#search-limits)
* [<span data-ttu-id="1a6dc-168">Service Bus</span><span class="sxs-lookup"><span data-stu-id="1a6dc-168">Service Bus</span></span>](#service-bus-limits)
* [<span data-ttu-id="1a6dc-169">Site Recovery</span><span class="sxs-lookup"><span data-stu-id="1a6dc-169">Site Recovery</span></span>](#site-recovery-limits)
* [<span data-ttu-id="1a6dc-170">SQL Database</span><span class="sxs-lookup"><span data-stu-id="1a6dc-170">SQL Database</span></span>](#sql-database-limits)
* [<span data-ttu-id="1a6dc-171">Storage</span><span class="sxs-lookup"><span data-stu-id="1a6dc-171">Storage</span></span>](#storage-limits)
* [<span data-ttu-id="1a6dc-172">Systém StorSimple</span><span class="sxs-lookup"><span data-stu-id="1a6dc-172">StorSimple System</span></span>](#storsimple-system-limits)
* [<span data-ttu-id="1a6dc-173">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="1a6dc-173">Stream Analytics</span></span>](#stream-analytics-limits)
* [<span data-ttu-id="1a6dc-174">Předplatné</span><span class="sxs-lookup"><span data-stu-id="1a6dc-174">Subscription</span></span>](#subscription-limits)
* [<span data-ttu-id="1a6dc-175">Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="1a6dc-175">Traffic Manager</span></span>](#traffic-manager-limits)
* [<span data-ttu-id="1a6dc-176">Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="1a6dc-176">Virtual Machines</span></span>](#virtual-machines-limits)
* [<span data-ttu-id="1a6dc-177">Škálovací sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="1a6dc-177">Virtual Machine Scale Sets</span></span>](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a><span data-ttu-id="1a6dc-178">Limity předplatného</span><span class="sxs-lookup"><span data-stu-id="1a6dc-178">Subscription limits</span></span>
#### <a name="subscription-limits"></a><span data-ttu-id="1a6dc-179">Limity předplatného</span><span class="sxs-lookup"><span data-stu-id="1a6dc-179">Subscription limits</span></span>
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a><span data-ttu-id="1a6dc-180">Limity předplatného – Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1a6dc-180">Subscription limits - Azure Resource Manager</span></span>
<span data-ttu-id="1a6dc-181">Následující omezení platí při použití skupiny prostředků Azure a Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-181">The following limits apply when using the Azure Resource Manager and Azure Resource Groups.</span></span> <span data-ttu-id="1a6dc-182">Limity, které nebyly změněny s Azure Resource Manager nejsou uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-182">Limits that have not changed with the Azure Resource Manager are not listed below.</span></span> <span data-ttu-id="1a6dc-183">Naleznete v předchozí tabulce těchto omezení.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-183">Please refer to the previous table for those limits.</span></span>

<span data-ttu-id="1a6dc-184">Informace o zpracování omezení pro správce prostředků požadavky najdete v tématu [omezení Resource Manager požadavky](resource-manager-request-limits.md).</span><span class="sxs-lookup"><span data-stu-id="1a6dc-184">For information about handling limits on Resource Manager requests, see [Throttling Resource Manager requests](resource-manager-request-limits.md).</span></span>

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a><span data-ttu-id="1a6dc-185">Omezení skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="1a6dc-185">Resource Group limits</span></span>
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a><span data-ttu-id="1a6dc-186">Limity virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="1a6dc-186">Virtual Machines limits</span></span>
#### <a name="virtual-machine-limits"></a><span data-ttu-id="1a6dc-187">Limity virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="1a6dc-187">Virtual Machine limits</span></span>
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a><span data-ttu-id="1a6dc-188">Virtuální počítače omezení - Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1a6dc-188">Virtual Machines limits - Azure Resource Manager</span></span>
<span data-ttu-id="1a6dc-189">Následující omezení platí při použití skupiny prostředků Azure a Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-189">The following limits apply when using the Azure Resource Manager and Azure Resource Groups.</span></span> <span data-ttu-id="1a6dc-190">Limity, které nebyly změněny s Azure Resource Manager nejsou uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-190">Limits that have not changed with the Azure Resource Manager are not listed below.</span></span> <span data-ttu-id="1a6dc-191">Naleznete v předchozí tabulce těchto omezení.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-191">Please refer to the previous table for those limits.</span></span>

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a><span data-ttu-id="1a6dc-192">Omezení sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1a6dc-192">Virtual Machine Scale Sets limits</span></span>
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a><span data-ttu-id="1a6dc-193">Kontejner instancí omezení</span><span class="sxs-lookup"><span data-stu-id="1a6dc-193">Container Instances Limits</span></span>
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="networking-limits"></a><span data-ttu-id="1a6dc-194">Síťová omezení</span><span class="sxs-lookup"><span data-stu-id="1a6dc-194">Networking limits</span></span>
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a><span data-ttu-id="1a6dc-195">Síťová omezení</span><span class="sxs-lookup"><span data-stu-id="1a6dc-195">Networking limits</span></span>
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a><span data-ttu-id="1a6dc-196">Omezuje aplikační brány</span><span class="sxs-lookup"><span data-stu-id="1a6dc-196">Application Gateway limits</span></span>
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a><span data-ttu-id="1a6dc-197">Sledovací proces sítě omezuje</span><span class="sxs-lookup"><span data-stu-id="1a6dc-197">Network Watcher limits</span></span>
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a><span data-ttu-id="1a6dc-198">Omezení Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="1a6dc-198">Traffic Manager limits</span></span>
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a><span data-ttu-id="1a6dc-199">Omezení DNS</span><span class="sxs-lookup"><span data-stu-id="1a6dc-199">DNS limits</span></span>
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a><span data-ttu-id="1a6dc-200">Limity úložiště</span><span class="sxs-lookup"><span data-stu-id="1a6dc-200">Storage limits</span></span>
<span data-ttu-id="1a6dc-201">Další informace o limity účtu úložiště najdete v tématu [a cíle výkonnosti služby Azure Storage Scalability](storage/common/storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="1a6dc-201">For additional details on storage account limits, see [Azure Storage Scalability and Performance Targets](storage/common/storage-scalability-targets.md).</span></span>
<!--like # storage accts --> 
#### <a name="storage-service-limits"></a><span data-ttu-id="1a6dc-202">Omezení služby úložiště</span><span class="sxs-lookup"><span data-stu-id="1a6dc-202">Storage Service limits</span></span>
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a><span data-ttu-id="1a6dc-203">Omezení disku virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1a6dc-203">Virtual machine disk limits</span></span> 
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

<span data-ttu-id="1a6dc-204">V tématu [velikostí virtuálních počítačů](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-204">See [Virtual machine sizes](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for additional details.</span></span>

#### <a name="managed-virtual-machine-disks"></a><span data-ttu-id="1a6dc-205">Disky spravovaných virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="1a6dc-205">Managed virtual machine disks</span></span>

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a><span data-ttu-id="1a6dc-206">Disky nespravované virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1a6dc-206">Unmanaged virtual machine disks</span></span>

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a><span data-ttu-id="1a6dc-207">Omezení poskytovatele prostředků úložiště</span><span class="sxs-lookup"><span data-stu-id="1a6dc-207">Storage Resource Provider limits</span></span>
[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

### <a name="cloud-services-limits"></a><span data-ttu-id="1a6dc-208">Omezení cloudové služby</span><span class="sxs-lookup"><span data-stu-id="1a6dc-208">Cloud Services limits</span></span>
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a><span data-ttu-id="1a6dc-209">Omezení služby App Service</span><span class="sxs-lookup"><span data-stu-id="1a6dc-209">App Service limits</span></span>
<span data-ttu-id="1a6dc-210">Následující omezení služby App Service zahrnují omezení pro webové aplikace, mobilní aplikace, aplikace API a Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-210">The following App Service limits include limits for Web Apps, Mobile Apps, API Apps, and Logic Apps.</span></span>

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a><span data-ttu-id="1a6dc-211">Omezení</span><span class="sxs-lookup"><span data-stu-id="1a6dc-211">Scheduler limits</span></span>
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a><span data-ttu-id="1a6dc-212">Omezení dávky</span><span class="sxs-lookup"><span data-stu-id="1a6dc-212">Batch limits</span></span>
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="biztalk-services-limits"></a><span data-ttu-id="1a6dc-213">Omezení služby BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="1a6dc-213">BizTalk Services limits</span></span>
<span data-ttu-id="1a6dc-214">Následující tabulka uvádí omezení služby Azure Biztalk Services.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-214">The following table shows the limits for Azure Biztalk Services.</span></span>

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a><span data-ttu-id="1a6dc-215">Omezení Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1a6dc-215">Azure Cosmos DB limits</span></span>
<span data-ttu-id="1a6dc-216">Azure Cosmos DB je globálním měřítku databáze, ve které je možné rozšířit propustnost a úložiště pro zpracování, ať vaše aplikace vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-216">Azure Cosmos DB is a global scale database in which throughput and storage can be scaled to handle whatever your application requires.</span></span> <span data-ttu-id="1a6dc-217">Pokud máte nějaké otázky o rozsahu poskytuje Azure Cosmos DB, pošlete e-mail na askcosmosdb@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-217">If you have any questions about the scale Azure Cosmos DB provides, please send email to askcosmosdb@microsoft.com.</span></span>

### <a name="mobile-engagement-limits"></a><span data-ttu-id="1a6dc-218">Omezení Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="1a6dc-218">Mobile Engagement limits</span></span>
[!INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]

### <a name="search-limits"></a><span data-ttu-id="1a6dc-219">Omezení vyhledávání</span><span class="sxs-lookup"><span data-stu-id="1a6dc-219">Search limits</span></span>
<span data-ttu-id="1a6dc-220">Cenové úrovně určete kapacitu a omezení služby search.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-220">Pricing tiers determine the capacity and limits of your search service.</span></span> <span data-ttu-id="1a6dc-221">Úrovně zahrnují:</span><span class="sxs-lookup"><span data-stu-id="1a6dc-221">Tiers include:</span></span>

* <span data-ttu-id="1a6dc-222">*Volné* víceklientské služby, sdílené s další předplatitelé služby Azure, určený pro malé vývoj a testování projektů.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-222">*Free* multi-tenant service, shared with other Azure subscribers, intended for evaluation and small development projects.</span></span>
* <span data-ttu-id="1a6dc-223">*Základní* poskytuje vyhrazený výpočetní prostředky pro úlohy v produkčním prostředí v menším měřítku, s až tři repliky pro vysokou dostupnost dotazu úlohy.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-223">*Basic* provides dedicated computing resources for production workloads at a smaller scale, with up to three replicas for highly available query workloads.</span></span>
* <span data-ttu-id="1a6dc-224">*Standard (S1, S2, S3, S3 s vysokou hustotou)* je pro větší úlohy v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-224">*Standard (S1, S2, S3, S3 High Density)* is for larger production workloads.</span></span> <span data-ttu-id="1a6dc-225">Více úrovní existovat v rámci úroveň standard, aby mohli vybrat konfiguraci prostředků, který nejlépe odpovídá váš profil zatížení.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-225">Multiple levels exist within the standard tier so that you can choose a resource configuration that best matches your workload profile.</span></span>

<span data-ttu-id="1a6dc-226">**Omezení podle předplatného**</span><span class="sxs-lookup"><span data-stu-id="1a6dc-226">**Limits per subscription**</span></span>

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

<span data-ttu-id="1a6dc-227">**Omezení jednu službu vyhledávání**</span><span class="sxs-lookup"><span data-stu-id="1a6dc-227">**Limits per search service**</span></span>

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

<span data-ttu-id="1a6dc-228">Další informace o omezeních na podrobnější úrovni, jako je například velikost dokumentu, dotazů za sekundu, klíčů, požadavků a odpovědí, najdete v části [omezení ve službě Azure Search služby](search/search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="1a6dc-228">To learn more about limits on a more granular level, such as document size, queries per second, keys, requests, and responses, see [Service limits in Azure Search](search/search-limits-quotas-capacity.md).</span></span>

### <a name="media-services-limits"></a><span data-ttu-id="1a6dc-229">Omezení služby Media Services</span><span class="sxs-lookup"><span data-stu-id="1a6dc-229">Media Services limits</span></span>
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a><span data-ttu-id="1a6dc-230">Omezení CDN</span><span class="sxs-lookup"><span data-stu-id="1a6dc-230">CDN limits</span></span>
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a><span data-ttu-id="1a6dc-231">Omezení Mobile Services</span><span class="sxs-lookup"><span data-stu-id="1a6dc-231">Mobile Services limits</span></span>
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a><span data-ttu-id="1a6dc-232">Omezení monitorování</span><span class="sxs-lookup"><span data-stu-id="1a6dc-232">Monitor limits</span></span>
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a><span data-ttu-id="1a6dc-233">Omezení služby centra oznámení</span><span class="sxs-lookup"><span data-stu-id="1a6dc-233">Notification Hub Service limits</span></span>
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a><span data-ttu-id="1a6dc-234">Omezení centra událostí</span><span class="sxs-lookup"><span data-stu-id="1a6dc-234">Event Hubs limits</span></span>
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a><span data-ttu-id="1a6dc-235">Omezení služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="1a6dc-235">Service Bus limits</span></span>
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a><span data-ttu-id="1a6dc-236">Omezení služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="1a6dc-236">IoT Hub limits</span></span>
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a><span data-ttu-id="1a6dc-237">Omezení pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="1a6dc-237">Data Factory limits</span></span>
[!INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a><span data-ttu-id="1a6dc-238">Omezuje data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="1a6dc-238">Data Lake Analytics limits</span></span>
[!INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="data-lake-store-limits"></a><span data-ttu-id="1a6dc-239">Omezení data Lake Store</span><span class="sxs-lookup"><span data-stu-id="1a6dc-239">Data Lake Store limits</span></span>
[!INCLUDE [azure-data-lake-store-limits](../includes/azure-data-lake-store-limits.md)]

### <a name="stream-analytics-limits"></a><span data-ttu-id="1a6dc-240">Omezuje služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="1a6dc-240">Stream Analytics limits</span></span>
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a><span data-ttu-id="1a6dc-241">Omezuje služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="1a6dc-241">Active Directory limits</span></span>
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="azure-event-grid-limits"></a><span data-ttu-id="1a6dc-242">Azure omezení událostí mřížky</span><span class="sxs-lookup"><span data-stu-id="1a6dc-242">Azure Event Grid limits</span></span>
[!INCLUDE [event-grid-limits](../includes/event-grid-limits.md)]

### <a name="azure-remoteapp-limits"></a><span data-ttu-id="1a6dc-243">Omezení Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="1a6dc-243">Azure RemoteApp limits</span></span>
[!INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a><span data-ttu-id="1a6dc-244">Omezení systému StorSimple</span><span class="sxs-lookup"><span data-stu-id="1a6dc-244">StorSimple System limits</span></span>
[!INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]

### <a name="log-analytics-limits"></a><span data-ttu-id="1a6dc-245">Omezuje analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="1a6dc-245">Log Analytics limits</span></span>
[!INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a><span data-ttu-id="1a6dc-246">Zálohování omezení</span><span class="sxs-lookup"><span data-stu-id="1a6dc-246">Backup limits</span></span>
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a><span data-ttu-id="1a6dc-247">Omezení Site Recovery</span><span class="sxs-lookup"><span data-stu-id="1a6dc-247">Site Recovery limits</span></span>
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a><span data-ttu-id="1a6dc-248">Omezení Application Insights</span><span class="sxs-lookup"><span data-stu-id="1a6dc-248">Application Insights limits</span></span>
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a><span data-ttu-id="1a6dc-249">Omezení API Management</span><span class="sxs-lookup"><span data-stu-id="1a6dc-249">API Management limits</span></span>
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a><span data-ttu-id="1a6dc-250">Omezení Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="1a6dc-250">Azure Redis Cache limits</span></span>
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a><span data-ttu-id="1a6dc-251">Omezení Key Vault</span><span class="sxs-lookup"><span data-stu-id="1a6dc-251">Key Vault limits</span></span>
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a><span data-ttu-id="1a6dc-252">Multi-factor Authentication</span><span class="sxs-lookup"><span data-stu-id="1a6dc-252">Multi-Factor Authentication</span></span>
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a><span data-ttu-id="1a6dc-253">Omezení automatizace</span><span class="sxs-lookup"><span data-stu-id="1a6dc-253">Automation limits</span></span>
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a><span data-ttu-id="1a6dc-254">Omezení databáze SQL</span><span class="sxs-lookup"><span data-stu-id="1a6dc-254">SQL Database limits</span></span>
<span data-ttu-id="1a6dc-255">Omezení SQL Database, najdete v části [limitů prostředků databáze SQL](sql-database/sql-database-resource-limits.md).</span><span class="sxs-lookup"><span data-stu-id="1a6dc-255">For SQL Database limits, see [SQL Database Resource Limits](sql-database/sql-database-resource-limits.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="1a6dc-256">Viz také</span><span class="sxs-lookup"><span data-stu-id="1a6dc-256">See also</span></span>
[<span data-ttu-id="1a6dc-257">Seznámení s Azure omezení a zvyšuje</span><span class="sxs-lookup"><span data-stu-id="1a6dc-257">Understanding Azure Limits and Increases</span></span>](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[<span data-ttu-id="1a6dc-258">Virtuální počítač velikost služeb a Cloud pro Azure.</span><span class="sxs-lookup"><span data-stu-id="1a6dc-258">Virtual Machine and Cloud Service Sizes for Azure</span></span>](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="1a6dc-259">Velikosti pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="1a6dc-259">Sizes for Cloud Services</span></span>](cloud-services/cloud-services-sizes-specs.md)

