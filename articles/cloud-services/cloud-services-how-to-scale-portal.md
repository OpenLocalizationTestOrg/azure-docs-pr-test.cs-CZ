---
title: "Automatické škálování cloudové služby na portálu | Microsoft Docs"
description: "Další informace o použití portálu ke konfiguraci pravidel automatického škálování pro cloudové služby webovou roli nebo role pracovního procesu v Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 701d4404-5cc0-454b-999c-feb94c1685c0
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: e9683d4c5779450fd67fa42ab13095c7f201b4cd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-portal"></a><span data-ttu-id="f6932-103">Postup konfigurace automatického škálování pro cloudové služby na portálu</span><span class="sxs-lookup"><span data-stu-id="f6932-103">How to configure auto scaling for a Cloud Service in the portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f6932-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f6932-104">Azure portal</span></span>](cloud-services-how-to-scale-portal.md)
> * [<span data-ttu-id="f6932-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="f6932-105">Azure classic portal</span></span>](cloud-services-how-to-scale.md)

<span data-ttu-id="f6932-106">Podmínky lze nastavit pro roli pracovního procesu cloudové služby, které aktivovat škálování nebo výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="f6932-106">Conditions can be set for a cloud service worker role that trigger a scale in or out operation.</span></span> <span data-ttu-id="f6932-107">Podmínky pro roli může být založená na využití procesoru, disku nebo zatížení sítě role.</span><span class="sxs-lookup"><span data-stu-id="f6932-107">The conditions for the role can be based on the CPU, disk, or network load of the role.</span></span> <span data-ttu-id="f6932-108">Můžete také nastavit podmínku na základě fronty zpráv nebo metriku jiný Azure prostředek spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="f6932-108">You can also set a condition based on a message queue or the metric of some other Azure resource associated with your subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="f6932-109">Tento článek se zaměřuje na webových a pracovních rolí cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="f6932-109">This article focuses on Cloud Service web and worker roles.</span></span> <span data-ttu-id="f6932-110">Když vytvoříte virtuální počítač (klasický) přímo, je hostovaná v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="f6932-110">When you create a virtual machine (classic) directly, it is hosted in a cloud service.</span></span> <span data-ttu-id="f6932-111">Standardní virtuálního počítače je možné škálovat jeho s přidružením [skupinu dostupnosti](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) a ručně je zapnout nebo vypnout.</span><span class="sxs-lookup"><span data-stu-id="f6932-111">You can scale a standard virtual machine by associating it with an [availability set](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) and manually turn them on or off.</span></span>

## <a name="considerations"></a><span data-ttu-id="f6932-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f6932-112">Considerations</span></span>
<span data-ttu-id="f6932-113">Před konfigurací škálování pro vaši aplikaci je třeba zvážit následující informace:</span><span class="sxs-lookup"><span data-stu-id="f6932-113">You should consider the following information before you configure scaling for your application:</span></span>

* <span data-ttu-id="f6932-114">Škálování má vliv základní použití.</span><span class="sxs-lookup"><span data-stu-id="f6932-114">Scaling is affected by core usage.</span></span>

    <span data-ttu-id="f6932-115">Větší instance rolí pomocí více jader.</span><span class="sxs-lookup"><span data-stu-id="f6932-115">Larger role instances use more cores.</span></span> <span data-ttu-id="f6932-116">Je možné škálovat aplikaci pouze v rámci maximální počet jader pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="f6932-116">You can scale an application only within the limit of cores for your subscription.</span></span> <span data-ttu-id="f6932-117">Řekněme například, že vaše předplatné může mít 20 jader.</span><span class="sxs-lookup"><span data-stu-id="f6932-117">For example, say your subscription has a limit of 20 cores.</span></span> <span data-ttu-id="f6932-118">Pokud spouštíte aplikaci pomocí dvou středních cloudové služby (celkem 4 jádra), můžete pouze postupně škálovat ostatních nasazení cloudové služby ve vašem předplatném ve zbývajících 16 jader.</span><span class="sxs-lookup"><span data-stu-id="f6932-118">If you run an application with two medium-sized cloud services (a total of 4 cores), you can only scale up other cloud service deployments in your subscription by the remaining 16 cores.</span></span> <span data-ttu-id="f6932-119">Další informace o velikosti najdete v tématu [cloudové služby velikosti](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="f6932-119">For more information about sizes, see [Cloud Service Sizes](cloud-services-sizes-specs.md).</span></span>

* <span data-ttu-id="f6932-120">Je možné škálovat podle prahová hodnota fronty zpráv.</span><span class="sxs-lookup"><span data-stu-id="f6932-120">You can scale based on a queue message threshold.</span></span> <span data-ttu-id="f6932-121">Další informace o tom, jak používat fronty najdete v tématu [jak používat fronty služby úložiště](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="f6932-121">For more information about how to use queues, see [How to use the Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>

* <span data-ttu-id="f6932-122">Můžete škálovat také další prostředky spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="f6932-122">You can also scale other resources associated with your subscription.</span></span>

* <span data-ttu-id="f6932-123">Povolit vysokou dostupnost vaší aplikace, se ujistěte, že je nasazený s dvěma nebo více instancí role.</span><span class="sxs-lookup"><span data-stu-id="f6932-123">To enable high availability of your application, you should ensure that it is deployed with two or more role instances.</span></span> <span data-ttu-id="f6932-124">Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="f6932-124">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>


## <a name="where-scale-is-located"></a><span data-ttu-id="f6932-125">Kde se nachází škálování</span><span class="sxs-lookup"><span data-stu-id="f6932-125">Where scale is located</span></span>
<span data-ttu-id="f6932-126">Po výběru cloudové služby, měli byste v okně služby cloud viditelné.</span><span class="sxs-lookup"><span data-stu-id="f6932-126">After you select your cloud service, you should have the cloud service blade visible.</span></span>

1. <span data-ttu-id="f6932-127">V okně cloudové služby na **rolí a instancí** dlaždici, vyberte název cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="f6932-127">On the cloud service blade, on the **Roles and Instances** tile, select the name of the cloud service.</span></span>   
   <span data-ttu-id="f6932-128">**Důležité**: Ujistěte se, klikněte na roli služby cloud není instanci role, která je nižší než roli.</span><span class="sxs-lookup"><span data-stu-id="f6932-128">**IMPORTANT**: Make sure to click the cloud service role, not the role instance that is below the role.</span></span>

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)
2. <span data-ttu-id="f6932-129">Vyberte **škálování** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="f6932-129">Select the **scale** tile.</span></span>

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a><span data-ttu-id="f6932-130">Automatické škálování</span><span class="sxs-lookup"><span data-stu-id="f6932-130">Automatic scale</span></span>
<span data-ttu-id="f6932-131">Nastavení škálování v roli lze nakonfigurovat buď dva režimy **ruční** nebo **automatické**.</span><span class="sxs-lookup"><span data-stu-id="f6932-131">You can configure scale settings for a role with either two modes **manual** or **automatic**.</span></span> <span data-ttu-id="f6932-132">Ruční je, jak jste zvyklí, nastavíte absolutní počet instancí.</span><span class="sxs-lookup"><span data-stu-id="f6932-132">Manual is as you would expect, you set the absolute count of instances.</span></span> <span data-ttu-id="f6932-133">Automatické umožňuje ale, že je sada pravidel, které řídí způsob a jak mnohem je měli škálovat.</span><span class="sxs-lookup"><span data-stu-id="f6932-133">Automatic however allows you to set rules that govern how and by how much you should scale.</span></span>

<span data-ttu-id="f6932-134">Nastavte **škálovat podle** možnost k **pravidla plánu a výkonu**.</span><span class="sxs-lookup"><span data-stu-id="f6932-134">Set the **Scale by** option to **schedule and performance rules**.</span></span>

![Nastavení škálování cloudové služby s profil a pravidla](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. <span data-ttu-id="f6932-136">Existující profil.</span><span class="sxs-lookup"><span data-stu-id="f6932-136">An existing profile.</span></span>
2. <span data-ttu-id="f6932-137">Přidáte pravidlo pro nadřazené profil.</span><span class="sxs-lookup"><span data-stu-id="f6932-137">Add a rule for the parent profile.</span></span>
3. <span data-ttu-id="f6932-138">Přidejte jiný profil.</span><span class="sxs-lookup"><span data-stu-id="f6932-138">Add another profile.</span></span>

<span data-ttu-id="f6932-139">Vyberte **přidat profil**.</span><span class="sxs-lookup"><span data-stu-id="f6932-139">Select **Add Profile**.</span></span> <span data-ttu-id="f6932-140">Profil určuje režimu, který chcete použít pro měřítka: **vždy**, **opakování**, **pevným datem**.</span><span class="sxs-lookup"><span data-stu-id="f6932-140">The profile determines which mode you want to use for the scale: **always**, **recurrence**, **fixed date**.</span></span>

<span data-ttu-id="f6932-141">Po dokončení konfigurace profilu a pravidla, vyberte **Uložit** horním.</span><span class="sxs-lookup"><span data-stu-id="f6932-141">After you have configured the profile and rules, select the **Save** icon at the top.</span></span>

#### <a name="profile"></a><span data-ttu-id="f6932-142">Profil</span><span class="sxs-lookup"><span data-stu-id="f6932-142">Profile</span></span>
<span data-ttu-id="f6932-143">Profil Nastaví minimální a maximální instancí měřítka, a také tento rozsah škálování je aktivní.</span><span class="sxs-lookup"><span data-stu-id="f6932-143">The profile sets minimum and maximum instances for the scale, and also when this scale range is active.</span></span>

* <span data-ttu-id="f6932-144">**Vždy**</span><span class="sxs-lookup"><span data-stu-id="f6932-144">**Always**</span></span>

    <span data-ttu-id="f6932-145">Vždy mít tento rozsah instancí, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f6932-145">Always keep this range of instances available.</span></span>  

    ![Cloudová služba, která vždy škálování](./media/cloud-services-how-to-scale-portal/select-always.png)
* <span data-ttu-id="f6932-147">**Opakování**</span><span class="sxs-lookup"><span data-stu-id="f6932-147">**Recurrence**</span></span>

    <span data-ttu-id="f6932-148">Vyberte sadu dny v týdnu škálování.</span><span class="sxs-lookup"><span data-stu-id="f6932-148">Choose a set of days of the week to scale.</span></span>

    ![Cloudové služby škálování s plán opakování](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
* <span data-ttu-id="f6932-150">**Pevné datum**</span><span class="sxs-lookup"><span data-stu-id="f6932-150">**Fixed Date**</span></span>

    <span data-ttu-id="f6932-151">Pevné časové období se škálovat roli.</span><span class="sxs-lookup"><span data-stu-id="f6932-151">A fixed date range to scale the role.</span></span>

    ![Cloudové služby škálování s pevnou datum](./media/cloud-services-how-to-scale-portal/select-fixed.png)

<span data-ttu-id="f6932-153">Po nakonfigurování profil, vyberte **OK** tlačítko v dolní části okna profilu.</span><span class="sxs-lookup"><span data-stu-id="f6932-153">After you have configured the profile, select the **OK** button at the bottom of the profile blade.</span></span>

#### <a name="rule"></a><span data-ttu-id="f6932-154">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="f6932-154">Rule</span></span>
<span data-ttu-id="f6932-155">Pravidla se přidají do profilu a představují podmínku, která aktivuje měřítka.</span><span class="sxs-lookup"><span data-stu-id="f6932-155">Rules are added to a profile and represent a condition that triggers the scale.</span></span>

<span data-ttu-id="f6932-156">Aktivační událost pravidlo je založena na metrika cloudové služby (využití procesoru, disková aktivita nebo síťové aktivity) do kterého můžete přidat podmíněného hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f6932-156">The rule trigger is based on a metric of the cloud service (CPU usage, disk activity, or network activity) to which you can add a conditional value.</span></span> <span data-ttu-id="f6932-157">Kromě toho může mít aktivační události na základě fronty zpráv nebo metriku jiný Azure prostředek spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="f6932-157">Additionally you can have the trigger based on a message queue or the metric of some other Azure resource associated with your subscription.</span></span>

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

<span data-ttu-id="f6932-158">Po nakonfigurování pravidla, vyberte **OK** tlačítko v dolní části okna pravidlo.</span><span class="sxs-lookup"><span data-stu-id="f6932-158">After you have configured the rule, select the **OK** button at the bottom of the rule blade.</span></span>

## <a name="back-to-manual-scale"></a><span data-ttu-id="f6932-159">Zpět na ruční škálování</span><span class="sxs-lookup"><span data-stu-id="f6932-159">Back to manual scale</span></span>
<span data-ttu-id="f6932-160">Přejděte na [nastavení škálování](#where-scale-is-located) a nastavte **škálovat podle** možnost k **počet instancí, který nastavím ručně**.</span><span class="sxs-lookup"><span data-stu-id="f6932-160">Navigate to the [scale settings](#where-scale-is-located) and set the **Scale by** option to **an instance count that I enter manually**.</span></span>

![Nastavení škálování cloudové služby s profil a pravidla](./media/cloud-services-how-to-scale-portal/manual-basics.png)

<span data-ttu-id="f6932-162">Toto nastavení se odebere, automatické škálování z role a potom můžete nastavit počet instancí přímo.</span><span class="sxs-lookup"><span data-stu-id="f6932-162">This setting removes automated scaling from the role and then you can set the instance count directly.</span></span>

1. <span data-ttu-id="f6932-163">(Ruční nebo automatické) možnost škálování.</span><span class="sxs-lookup"><span data-stu-id="f6932-163">The scale (manual or automated) option.</span></span>
2. <span data-ttu-id="f6932-164">Posuvník instance role pro nastavení instance, které chcete škálovat.</span><span class="sxs-lookup"><span data-stu-id="f6932-164">A role instance slider to set the instances to scale to.</span></span>
3. <span data-ttu-id="f6932-165">Instance role, kterou chcete škálovat.</span><span class="sxs-lookup"><span data-stu-id="f6932-165">Instances of the role to scale to.</span></span>

<span data-ttu-id="f6932-166">Po nakonfigurování nastavení škálování, vyberte **Uložit** horním.</span><span class="sxs-lookup"><span data-stu-id="f6932-166">After you have configured the scale settings, select the **Save** icon at the top.</span></span>
