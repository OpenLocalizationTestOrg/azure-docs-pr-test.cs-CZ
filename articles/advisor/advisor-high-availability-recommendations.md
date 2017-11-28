---
title: "doporučení pro vysokou dostupnost Advisor aaaAzure | Microsoft Docs"
description: "Pomocí Azure Advisor tooimprove vysokou dostupnost vašich Azure nasazení."
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
ms.openlocfilehash: 3ac75ce401271f0212d198d7a7dc75ab702b6eda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-high-availability-recommendations"></a><span data-ttu-id="264ca-103">Doporučení pro vysokou dostupnost služby Advisor</span><span class="sxs-lookup"><span data-stu-id="264ca-103">Advisor High Availability recommendations</span></span>

<span data-ttu-id="264ca-104">Azure Advisor pomáhá zajistit, a zlepšování kontinuity hello důležitými obchodními aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="264ca-104">Azure Advisor helps you ensure and improve hello continuity of your business-critical applications.</span></span> <span data-ttu-id="264ca-105">Vysoká dostupnost doporučení Advisor můžete získat z hello **vysokou dostupnost** kartě hello Advisor řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="264ca-105">You can get high availability recommendations by Advisor from hello **High Availability** tab of hello Advisor dashboard.</span></span>

![Tlačítko vysoké dostupnosti na řídicím panelu Advisor hello](./media/advisor-high-availability-recommendations/advisor-high-availability-tab.png)


## <a name="ensure-virtual-machine-fault-tolerance"></a><span data-ttu-id="264ca-107">Zajištění odolnosti proti chybám virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="264ca-107">Ensure virtual machine fault tolerance</span></span>

<span data-ttu-id="264ca-108">Advisor identifikuje virtuálních počítačů, které nejsou součástí skupiny dostupnosti a doporučuje jejich přesunutí do skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="264ca-108">Advisor identifies virtual machines that are not part of an availability set and recommends moving them into an availability set.</span></span> <span data-ttu-id="264ca-109">Zajistit aplikace tooyour redundanci, doporučujeme seskupit dva nebo více virtuálních počítačů v nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="264ca-109">To provide redundancy tooyour application, we recommend that you group two or more virtual machines in an availability set.</span></span> <span data-ttu-id="264ca-110">Tato konfigurace zajistí, že během buď plánované i neplánované údržby, je k dispozici alespoň jeden virtuální počítač a splňuje hello virtuální počítač Azure SLA.</span><span class="sxs-lookup"><span data-stu-id="264ca-110">This configuration ensures that during either a planned or unplanned maintenance event, at least one virtual machine is available and meets hello Azure virtual machine SLA.</span></span> <span data-ttu-id="264ca-111">Můžete buď toocreate sadu dostupnosti pro hello virtuálního počítače nebo tooadd hello virtuálního počítače tooan stávající sadu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="264ca-111">You can choose either toocreate an availability set for hello virtual machine or tooadd hello virtual machine tooan existing availability set.</span></span>

> [!NOTE]
> <span data-ttu-id="264ca-112">Pokud se rozhodnete toocreate dostupnosti nastavena, je nutné přidat alespoň jeden další virtuální počítač do ní.</span><span class="sxs-lookup"><span data-stu-id="264ca-112">If you choose toocreate an availability set, you must add at least one more virtual machine into it.</span></span> <span data-ttu-id="264ca-113">Doporučujeme seskupit dva nebo víc virtuálních počítačů ve skupině dostupnosti nastavena tooensure, který je k dispozici alespoň jeden počítač během výpadku.</span><span class="sxs-lookup"><span data-stu-id="264ca-113">We recommend that you group two or more virtual machines in an availability set tooensure that at least one machine is available during an outage.</span></span>

![Advisor doporučení: redundanci virtuálního počítače, použijte nastavení dostupnosti](./media/advisor-high-availability-recommendations/advisor-high-availability-create-availability-set.png)

## <a name="ensure-availability-set-fault-tolerance"></a><span data-ttu-id="264ca-115">Ujistěte se, že odolnost proti chybám sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="264ca-115">Ensure availability set fault tolerance</span></span> 

<span data-ttu-id="264ca-116">Advisor určí skupiny dostupnosti, které obsahují jeden virtuální počítač a doporučuje přidání tooit jeden nebo více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="264ca-116">Advisor identifies availability sets that contain a single virtual machine and recommends adding one or more virtual machines tooit.</span></span> <span data-ttu-id="264ca-117">Zajistit aplikace tooyour redundanci, doporučujeme seskupit dva nebo více virtuálních počítačů v nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="264ca-117">To provide redundancy tooyour application, we recommend that you group two or more virtual machines in an availability set.</span></span> <span data-ttu-id="264ca-118">Tato konfigurace zajistí, že během buď plánované i neplánované údržby, je k dispozici alespoň jeden virtuální počítač a splňuje hello virtuální počítač Azure SLA.</span><span class="sxs-lookup"><span data-stu-id="264ca-118">This configuration ensures that during either a planned or unplanned maintenance event, at least one virtual machine is available and meets hello Azure virtual machine SLA.</span></span> <span data-ttu-id="264ca-119">Můžete buď toocreate na virtuálním počítači nebo toouse existující virtuální počítač a tooadd ho toohello sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="264ca-119">You can choose either toocreate a virtual machine or toouse an existing virtual machine, and tooadd it toohello availability set.</span></span>  

![Advisor doporučení: přidejte jeden nebo více nastavení dostupnosti toothis virtuální počítače](./media/advisor-high-availability-recommendations/advisor-high-availability-add-vm-to-availability-set.png)


## <a name="ensure-application-gateway-fault-tolerance"></a><span data-ttu-id="264ca-121">Ujistěte se, aplikace brány odolnost proti chybám</span><span class="sxs-lookup"><span data-stu-id="264ca-121">Ensure application gateway fault tolerance</span></span>
<span data-ttu-id="264ca-122">tooensure hello kontinuity podnikových procesů důležitých aplikací, které jsou zapnuté nástrojem application Gateway, Advisor určí instance brány aplikace, kteří nejsou nakonfigurovaní pro odolnost proti chybám, a navrhování nápravné akce, které můžete provést .</span><span class="sxs-lookup"><span data-stu-id="264ca-122">tooensure hello business continuity of mission-critical applications that are powered by application gateways, Advisor identifies application gateway instances that are not configured for fault tolerance, and it suggests remediation actions that you can take.</span></span> <span data-ttu-id="264ca-123">Advisor určí střední a velké aplikace s jedinou instancí brány a doporučí přidat nejméně jedna další instance.</span><span class="sxs-lookup"><span data-stu-id="264ca-123">Advisor identifies medium or large single-instance application gateways, and it recommends adding at least one more instance.</span></span> <span data-ttu-id="264ca-124">Také identifikuje instance jednoho nebo více malé aplikačních bran a doporučuje migrace toomedium nebo velké SKU.</span><span class="sxs-lookup"><span data-stu-id="264ca-124">It also identifies single- or multi-instance small application gateways and recommends migrating toomedium or large SKUs.</span></span> <span data-ttu-id="264ca-125">Advisor doporučuje tyto akce tooensure, že vaše aplikace instance brány jsou nakonfigurované toosatisfy hello požadavků na aktuální SLA pro tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="264ca-125">Advisor recommends these actions tooensure that your application gateway instances are configured toosatisfy hello current SLA requirements for these resources.</span></span>

![Advisor doporučení: nasazení dvou nebo více instancí brány střední a velké velikostí aplikace](./media/advisor-high-availability-recommendations/advisor-high-availability-application-gateway.png)

## <a name="improve-hello-performance-and-reliability-of-virtual-machine-disks"></a><span data-ttu-id="264ca-127">Zvýšení výkonu hello a spolehlivost disky virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="264ca-127">Improve hello performance and reliability of virtual machine disks</span></span>

<span data-ttu-id="264ca-128">Advisor určí virtuálních počítačů s standardní disky a doporučuje upgrade toopremium disky.</span><span class="sxs-lookup"><span data-stu-id="264ca-128">Advisor identifies virtual machines with standard disks and recommends upgrading toopremium disks.</span></span>
 
<span data-ttu-id="264ca-129">Azure Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disků pro virtuální počítače spustit I náročnými úlohy.</span><span class="sxs-lookup"><span data-stu-id="264ca-129">Azure Premium Storage delivers high-performance, low-latency disk support for virtual machines that run I/O-intensive workloads.</span></span> <span data-ttu-id="264ca-130">Disky virtuálního počítače, které používají účty úložiště premium ukládat data do disků SSD (Solid-State Drive).</span><span class="sxs-lookup"><span data-stu-id="264ca-130">Virtual machine disks that use premium storage accounts store data on solid-state drives (SSDs).</span></span> <span data-ttu-id="264ca-131">Hello nejlepšího výkonu pro vaši aplikaci doporučujeme migraci všechny disky virtuálního počítače vyžadující vysokou úložiště toopremium IOPS.</span><span class="sxs-lookup"><span data-stu-id="264ca-131">For hello best performance for your application, we recommend that you migrate any virtual machine disks requiring high IOPS toopremium storage.</span></span> 

<span data-ttu-id="264ca-132">Pokud vaše disky nevyžadují vysokou IOPS, můžete omezit náklady na údržbu je v standardní úložiště.</span><span class="sxs-lookup"><span data-stu-id="264ca-132">If your disks do not require high IOPS, you can limit costs by maintaining them in standard storage.</span></span> <span data-ttu-id="264ca-133">Standardní úložiště ukládá data disku virtuálního počítače na jednotkách pevných disků (HDD) namísto SSD.</span><span class="sxs-lookup"><span data-stu-id="264ca-133">Standard storage stores virtual machine disk data on hard disk drives (HDDs) instead of SSDs.</span></span> <span data-ttu-id="264ca-134">Můžete toomigrate disky toopremium disky virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="264ca-134">You can choose toomigrate your virtual machine disks toopremium disks.</span></span> <span data-ttu-id="264ca-135">Pro prémiové disky jsou podporovány na virtuálním počítači většina SKU.</span><span class="sxs-lookup"><span data-stu-id="264ca-135">Premium disks are supported on most virtual machine SKUs.</span></span> <span data-ttu-id="264ca-136">Ale v některých případech, pokud chcete, aby toouse prémiové disky, můžete potřebovat tooupgrade virtuálního počítače SKU také.</span><span class="sxs-lookup"><span data-stu-id="264ca-136">However, in some cases, if you want toouse premium disks, you might need tooupgrade your virtual machine SKUs as well.</span></span>

![Advisor doporučení: upgrade toopremium disky vylepšit spolehlivost hello disky virtuálního počítače](./media/advisor-high-availability-recommendations/advisor-high-availability-upgrade-to-premium-disks.png)

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a><span data-ttu-id="264ca-138">Virtuální počítač data chránit před nechtěným odstraněním</span><span class="sxs-lookup"><span data-stu-id="264ca-138">Protect your virtual machine data from accidental deletion</span></span>
<span data-ttu-id="264ca-139">Advisor identifikuje virtuální počítače, kde není povolené zálohování, a doporučí povolení zálohování.</span><span class="sxs-lookup"><span data-stu-id="264ca-139">Advisor identifies virtual machines where backup is not enabled, and it recommends enabling backup.</span></span> <span data-ttu-id="264ca-140">Nastavení zálohování virtuálních počítačů zajistí hello dostupnost důležitých podnikových dat a nabízí ochranu proti náhodnému odstranění nebo poškození.</span><span class="sxs-lookup"><span data-stu-id="264ca-140">Setting up virtual machine backup ensures hello availability of your business-critical data and offers protection against accidental deletion or corruption.</span></span>

![Advisor doporučení: Konfigurace virtuálního počítače zálohování tooprotect důležitá data](./media/advisor-high-availability-recommendations/advisor-high-availability-virtual-machine-backup.png)

## <a name="access-high-availability-recommendations-in-advisor"></a><span data-ttu-id="264ca-142">Přístup k zajištění vysoké dostupnosti doporučení v Advisor</span><span class="sxs-lookup"><span data-stu-id="264ca-142">Access high availability recommendations in Advisor</span></span>

1. <span data-ttu-id="264ca-143">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="264ca-143">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="264ca-144">V levém podokně hello, klikněte na **další služby**.</span><span class="sxs-lookup"><span data-stu-id="264ca-144">In hello left pane, click **More services**.</span></span>

3. <span data-ttu-id="264ca-145">V hello služby nabídky podokně v části **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="264ca-145">In hello service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="264ca-146">se zobrazí řídicí panel Advisor Hello.</span><span class="sxs-lookup"><span data-stu-id="264ca-146">hello Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="264ca-147">Na řídicím panelu hello Advisor, klikněte na tlačítko hello **vysokou dostupnost** kartě a potom vyberte hello předplatné, pro které chcete tooreceive doporučení.</span><span class="sxs-lookup"><span data-stu-id="264ca-147">On hello Advisor dashboard, click hello **High Availability** tab, and then select hello subscription for which you want tooreceive recommendations.</span></span>

> [!NOTE]
> <span data-ttu-id="264ca-148">tooaccess doporučení služby Advisor, musíte nejdřív *zaregistrovat předplatné* službou Advisor.</span><span class="sxs-lookup"><span data-stu-id="264ca-148">tooaccess Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="264ca-149">Předplatné je zaregistrován při *předplatné vlastníka* spustí hello Advisor řídicí panel a klikne na tlačítko hello **získat doporučení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="264ca-149">A subscription is registered when a *subscription Owner* launches hello Advisor dashboard and clicks hello **Get recommendations** button.</span></span> <span data-ttu-id="264ca-150">Toto je *jednorázovou operaci*.</span><span class="sxs-lookup"><span data-stu-id="264ca-150">This is a *one-time operation*.</span></span> <span data-ttu-id="264ca-151">Po registraci předplatného hello dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="264ca-151">After hello subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="264ca-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="264ca-152">Next steps</span></span>

<span data-ttu-id="264ca-153">Další informace o doporučení služby Advisor najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="264ca-153">For more information about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="264ca-154">Úvod tooAzure Advisor</span><span class="sxs-lookup"><span data-stu-id="264ca-154">Introduction tooAzure Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="264ca-155">Začínáme se službou Advisor</span><span class="sxs-lookup"><span data-stu-id="264ca-155">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="264ca-156">Náklady na doporučení služby Advisor</span><span class="sxs-lookup"><span data-stu-id="264ca-156">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="264ca-157">Poradce při hodnocení výkonu doporučení</span><span class="sxs-lookup"><span data-stu-id="264ca-157">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="264ca-158">Doporučení zabezpečení Advisor</span><span class="sxs-lookup"><span data-stu-id="264ca-158">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)

