---
title: "Azure doporučení služby Advisor vysokou dostupnost | Microsoft Docs"
description: "Pomocí Azure Advisor lze zvýšit vysokou dostupnost vašich Azure nasazení."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 23c159471a6e5a7ad9cb545840e8afd3ac72ecba
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="advisor-high-availability-recommendations"></a><span data-ttu-id="c17de-103">Doporučení pro vysokou dostupnost služby Advisor</span><span class="sxs-lookup"><span data-stu-id="c17de-103">Advisor High Availability recommendations</span></span>

<span data-ttu-id="c17de-104">Azure Advisor pomáhá zajistit, a zlepšování kontinuity důležitými obchodními aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="c17de-104">Azure Advisor helps you ensure and improve the continuity of your business-critical applications.</span></span> <span data-ttu-id="c17de-105">Můžete získat vysokou dostupnost doporučení Advisor z **vysokou dostupnost** Advisor řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="c17de-105">You can get high availability recommendations by Advisor from the **High Availability** tab of the Advisor dashboard.</span></span>

![Tlačítko vysoké dostupnosti na řídicím panelu Advisor](./media/advisor-high-availability-recommendations/advisor-high-availability-tab.png)


## <a name="ensure-virtual-machine-fault-tolerance"></a><span data-ttu-id="c17de-107">Zajištění odolnosti proti chybám virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c17de-107">Ensure virtual machine fault tolerance</span></span>

<span data-ttu-id="c17de-108">Advisor identifikuje virtuálních počítačů, které nejsou součástí skupiny dostupnosti a doporučuje jejich přesunutí do skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c17de-108">Advisor identifies virtual machines that are not part of an availability set and recommends moving them into an availability set.</span></span> <span data-ttu-id="c17de-109">Pokud chcete zajistit redundanci pro vaši aplikaci, doporučujeme seskupit dva nebo více virtuálních počítačů do skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c17de-109">To provide redundancy to your application, we recommend that you group two or more virtual machines in an availability set.</span></span> <span data-ttu-id="c17de-110">Tato konfigurace zajistí, že během buď plánované i neplánované údržby alespoň jeden virtuální počítač je k dispozici a splňuje virtuální počítač Azure SLA.</span><span class="sxs-lookup"><span data-stu-id="c17de-110">This configuration ensures that during either a planned or unplanned maintenance event, at least one virtual machine is available and meets the Azure virtual machine SLA.</span></span> <span data-ttu-id="c17de-111">Můžete si vybrat, chcete-li vytvořit sadu dostupnosti pro virtuální počítač nebo virtuální počítač přidat do stávající sadu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c17de-111">You can choose either to create an availability set for the virtual machine or to add the virtual machine to an existing availability set.</span></span>

> [!NOTE]
> <span data-ttu-id="c17de-112">Pokud se rozhodnete vytvořit skupinu dostupnosti, je nutné přidat alespoň jeden další virtuální počítač do ní.</span><span class="sxs-lookup"><span data-stu-id="c17de-112">If you choose to create an availability set, you must add at least one more virtual machine into it.</span></span> <span data-ttu-id="c17de-113">Doporučujeme, které jsou seskupeny dvě nebo více virtuálních počítačů ve skupině dostupnosti nastavena zajistit, aby aspoň jeden počítač je k dispozici během výpadku.</span><span class="sxs-lookup"><span data-stu-id="c17de-113">We recommend that you group two or more virtual machines in an availability set to ensure that at least one machine is available during an outage.</span></span>

![Advisor doporučení: redundanci virtuálního počítače, použijte nastavení dostupnosti](./media/advisor-high-availability-recommendations/advisor-high-availability-create-availability-set.png)

## <a name="ensure-availability-set-fault-tolerance"></a><span data-ttu-id="c17de-115">Ujistěte se, že odolnost proti chybám sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c17de-115">Ensure availability set fault tolerance</span></span> 

<span data-ttu-id="c17de-116">Advisor určí skupiny dostupnosti, které obsahují jeden virtuální počítač a doporučuje přidání do jednoho nebo více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c17de-116">Advisor identifies availability sets that contain a single virtual machine and recommends adding one or more virtual machines to it.</span></span> <span data-ttu-id="c17de-117">Pokud chcete zajistit redundanci pro vaši aplikaci, doporučujeme seskupit dva nebo více virtuálních počítačů do skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c17de-117">To provide redundancy to your application, we recommend that you group two or more virtual machines in an availability set.</span></span> <span data-ttu-id="c17de-118">Tato konfigurace zajistí, že během buď plánované i neplánované údržby alespoň jeden virtuální počítač je k dispozici a splňuje virtuální počítač Azure SLA.</span><span class="sxs-lookup"><span data-stu-id="c17de-118">This configuration ensures that during either a planned or unplanned maintenance event, at least one virtual machine is available and meets the Azure virtual machine SLA.</span></span> <span data-ttu-id="c17de-119">Můžete buď vytvořit virtuální počítač nebo použít existující virtuální počítač a tím ho přidáte do skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c17de-119">You can choose either to create a virtual machine or to use an existing virtual machine, and to add it to the availability set.</span></span>  

![Advisor doporučení: Přidejte jednu nebo více virtuálních počítačů do této skupiny dostupnosti](./media/advisor-high-availability-recommendations/advisor-high-availability-add-vm-to-availability-set.png)


## <a name="ensure-application-gateway-fault-tolerance"></a><span data-ttu-id="c17de-121">Ujistěte se, aplikace brány odolnost proti chybám</span><span class="sxs-lookup"><span data-stu-id="c17de-121">Ensure application gateway fault tolerance</span></span>
<span data-ttu-id="c17de-122">Zajištění kontinuity obchodních procesů důležitých aplikací, které jsou zapnuté nástrojem application Gateway, Advisor identifikuje instance brány aplikace, kteří nejsou nakonfigurovaní pro odolnost proti chybám, a navrhování nápravné akce, které můžete provést.</span><span class="sxs-lookup"><span data-stu-id="c17de-122">To ensure the business continuity of mission-critical applications that are powered by application gateways, Advisor identifies application gateway instances that are not configured for fault tolerance, and it suggests remediation actions that you can take.</span></span> <span data-ttu-id="c17de-123">Advisor určí střední a velké aplikace s jedinou instancí brány a doporučí přidat nejméně jedna další instance.</span><span class="sxs-lookup"><span data-stu-id="c17de-123">Advisor identifies medium or large single-instance application gateways, and it recommends adding at least one more instance.</span></span> <span data-ttu-id="c17de-124">Také určí jeden nebo více instance malé application Gateway a doporučuje migrace na střední a velké jednotky SKU.</span><span class="sxs-lookup"><span data-stu-id="c17de-124">It also identifies single- or multi-instance small application gateways and recommends migrating to medium or large SKUs.</span></span> <span data-ttu-id="c17de-125">Advisor doporučuje tyto akce se ujistíte, že instance brány vaší aplikace nakonfigurovaná vyhovět požadavkům SLA pro tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="c17de-125">Advisor recommends these actions to ensure that your application gateway instances are configured to satisfy the current SLA requirements for these resources.</span></span>

![Advisor doporučení: nasazení dvou nebo více instancí brány střední a velké velikostí aplikace](./media/advisor-high-availability-recommendations/advisor-high-availability-application-gateway.png)

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks"></a><span data-ttu-id="c17de-127">Zvýšení výkonu a spolehlivosti disky virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c17de-127">Improve the performance and reliability of virtual machine disks</span></span>

<span data-ttu-id="c17de-128">Advisor určí virtuálních počítačů s standardní disky a doporučuje upgrade na prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="c17de-128">Advisor identifies virtual machines with standard disks and recommends upgrading to premium disks.</span></span>
 
<span data-ttu-id="c17de-129">Azure Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disků pro virtuální počítače spustit I náročnými úlohy.</span><span class="sxs-lookup"><span data-stu-id="c17de-129">Azure Premium Storage delivers high-performance, low-latency disk support for virtual machines that run I/O-intensive workloads.</span></span> <span data-ttu-id="c17de-130">Disky virtuálního počítače, které používají účty úložiště premium ukládat data do disků SSD (Solid-State Drive).</span><span class="sxs-lookup"><span data-stu-id="c17de-130">Virtual machine disks that use premium storage accounts store data on solid-state drives (SSDs).</span></span> <span data-ttu-id="c17de-131">Nejlepšího výkonu pro vaši aplikaci doporučujeme migraci všechny disky virtuálního počítače, které vyžadují vysokou IOPS pro storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="c17de-131">For the best performance for your application, we recommend that you migrate any virtual machine disks requiring high IOPS to premium storage.</span></span> 

<span data-ttu-id="c17de-132">Pokud vaše disky nevyžadují vysokou IOPS, můžete omezit náklady na údržbu je v standardní úložiště.</span><span class="sxs-lookup"><span data-stu-id="c17de-132">If your disks do not require high IOPS, you can limit costs by maintaining them in standard storage.</span></span> <span data-ttu-id="c17de-133">Standardní úložiště ukládá data disku virtuálního počítače na jednotkách pevných disků (HDD) namísto SSD.</span><span class="sxs-lookup"><span data-stu-id="c17de-133">Standard storage stores virtual machine disk data on hard disk drives (HDDs) instead of SSDs.</span></span> <span data-ttu-id="c17de-134">Můžete migrovat disky virtuálního počítače do prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="c17de-134">You can choose to migrate your virtual machine disks to premium disks.</span></span> <span data-ttu-id="c17de-135">Pro prémiové disky jsou podporovány na virtuálním počítači většina SKU.</span><span class="sxs-lookup"><span data-stu-id="c17de-135">Premium disks are supported on most virtual machine SKUs.</span></span> <span data-ttu-id="c17de-136">Ale v některých případech, pokud chcete použít prémiové disky, může musíte upgradovat virtuální počítač SKU také.</span><span class="sxs-lookup"><span data-stu-id="c17de-136">However, in some cases, if you want to use premium disks, you might need to upgrade your virtual machine SKUs as well.</span></span>

![Advisor doporučení: upgrade na prémiové disky vylepšit spolehlivost disky virtuálního počítače](./media/advisor-high-availability-recommendations/advisor-high-availability-upgrade-to-premium-disks.png)

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a><span data-ttu-id="c17de-138">Virtuální počítač data chránit před nechtěným odstraněním</span><span class="sxs-lookup"><span data-stu-id="c17de-138">Protect your virtual machine data from accidental deletion</span></span>
<span data-ttu-id="c17de-139">Advisor identifikuje virtuální počítače, kde není povolené zálohování, a doporučí povolení zálohování.</span><span class="sxs-lookup"><span data-stu-id="c17de-139">Advisor identifies virtual machines where backup is not enabled, and it recommends enabling backup.</span></span> <span data-ttu-id="c17de-140">Nastavení zálohování virtuálních počítačů zajistíte dostupnost důležitých podnikových dat a poskytuje ochranu před náhodným odstraněním nebo poškození.</span><span class="sxs-lookup"><span data-stu-id="c17de-140">Setting up virtual machine backup ensures the availability of your business-critical data and offers protection against accidental deletion or corruption.</span></span>

![Advisor doporučení: Konfigurace zálohování virtuálních počítačů k ochraně důležitých dat](./media/advisor-high-availability-recommendations/advisor-high-availability-virtual-machine-backup.png)

## <a name="access-high-availability-recommendations-in-advisor"></a><span data-ttu-id="c17de-142">Přístup k zajištění vysoké dostupnosti doporučení v Advisor</span><span class="sxs-lookup"><span data-stu-id="c17de-142">Access high availability recommendations in Advisor</span></span>

1. <span data-ttu-id="c17de-143">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c17de-143">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="c17de-144">V levém podokně klikněte na **další služby**.</span><span class="sxs-lookup"><span data-stu-id="c17de-144">In the left pane, click **More services**.</span></span>

3. <span data-ttu-id="c17de-145">V podokně nabídky služby v rámci **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="c17de-145">In the service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="c17de-146">Se zobrazí řídicí panel služby Advisor.</span><span class="sxs-lookup"><span data-stu-id="c17de-146">The Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="c17de-147">Na řídicím panelu služby Advisor, klikněte na **vysokou dostupnost** kartě a potom vyberte předplatné, pro který chcete dostávat doporučení.</span><span class="sxs-lookup"><span data-stu-id="c17de-147">On the Advisor dashboard, click the **High Availability** tab, and then select the subscription for which you want to receive recommendations.</span></span>

> [!NOTE]
> <span data-ttu-id="c17de-148">Chcete-li získat přístup k doporučení služby Advisor, je nutné nejprve *zaregistrovat předplatné* službou Advisor.</span><span class="sxs-lookup"><span data-stu-id="c17de-148">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="c17de-149">Předplatné je zaregistrován při *předplatné vlastníka* spustí Advisor řídicího panelu a klikne na tlačítko **získat doporučení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c17de-149">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="c17de-150">Toto je *jednorázovou operaci*.</span><span class="sxs-lookup"><span data-stu-id="c17de-150">This is a *one-time operation*.</span></span> <span data-ttu-id="c17de-151">Po registraci předplatného dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="c17de-151">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c17de-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c17de-152">Next steps</span></span>

<span data-ttu-id="c17de-153">Další informace o doporučení služby Advisor najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="c17de-153">For more information about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="c17de-154">Úvod do Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="c17de-154">Introduction to Azure Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="c17de-155">Začínáme se službou Advisor</span><span class="sxs-lookup"><span data-stu-id="c17de-155">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="c17de-156">Náklady na doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="c17de-156">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="c17de-157">Poradce při hodnocení výkonu doporučení</span><span class="sxs-lookup"><span data-stu-id="c17de-157">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="c17de-158">Doporučení zabezpečení Advisor</span><span class="sxs-lookup"><span data-stu-id="c17de-158">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)

