---
title: "aaaAuto škálování cloudové služby na portálu hello | Microsoft Docs"
description: "Zjistěte, jak toouse hello portálu tooconfigure automatické škálování pravidla pro cloudové služby webovou roli nebo role pracovního procesu v Azure."
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
ms.openlocfilehash: 265f4c8ec5e1ec2f85585df25f18cd0d0c9946a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-auto-scaling-for-a-cloud-service-in-hello-portal"></a><span data-ttu-id="d2e56-103">Jak tooconfigure automatické škálování pro cloudové služby v rámci portálu hello</span><span class="sxs-lookup"><span data-stu-id="d2e56-103">How tooconfigure auto scaling for a Cloud Service in hello portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d2e56-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d2e56-104">Azure portal</span></span>](cloud-services-how-to-scale-portal.md)
> * [<span data-ttu-id="d2e56-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="d2e56-105">Azure classic portal</span></span>](cloud-services-how-to-scale.md)

<span data-ttu-id="d2e56-106">Podmínky lze nastavit pro roli pracovního procesu cloudové služby, které aktivovat škálování nebo výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="d2e56-106">Conditions can be set for a cloud service worker role that trigger a scale in or out operation.</span></span> <span data-ttu-id="d2e56-107">Hello podmínky pro hello role může být založená na hello procesoru, disku nebo zatížení sítě hello role.</span><span class="sxs-lookup"><span data-stu-id="d2e56-107">hello conditions for hello role can be based on hello CPU, disk, or network load of hello role.</span></span> <span data-ttu-id="d2e56-108">Můžete také nastavit podmínku podle zprávy fronty nebo hello metrikou jiný Azure prostředek spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="d2e56-108">You can also set a condition based on a message queue or hello metric of some other Azure resource associated with your subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="d2e56-109">Tento článek se zaměřuje na webových a pracovních rolí cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="d2e56-109">This article focuses on Cloud Service web and worker roles.</span></span> <span data-ttu-id="d2e56-110">Když vytvoříte virtuální počítač (klasický) přímo, je hostovaná v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="d2e56-110">When you create a virtual machine (classic) directly, it is hosted in a cloud service.</span></span> <span data-ttu-id="d2e56-111">Standardní virtuálního počítače je možné škálovat jeho s přidružením [skupinu dostupnosti](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) a ručně je zapnout nebo vypnout.</span><span class="sxs-lookup"><span data-stu-id="d2e56-111">You can scale a standard virtual machine by associating it with an [availability set](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) and manually turn them on or off.</span></span>

## <a name="considerations"></a><span data-ttu-id="d2e56-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d2e56-112">Considerations</span></span>
<span data-ttu-id="d2e56-113">Měli byste zvážit hello před konfigurací škálování pro vaši aplikaci následující informace:</span><span class="sxs-lookup"><span data-stu-id="d2e56-113">You should consider hello following information before you configure scaling for your application:</span></span>

* <span data-ttu-id="d2e56-114">Škálování má vliv základní použití.</span><span class="sxs-lookup"><span data-stu-id="d2e56-114">Scaling is affected by core usage.</span></span>

    <span data-ttu-id="d2e56-115">Větší instance rolí pomocí více jader.</span><span class="sxs-lookup"><span data-stu-id="d2e56-115">Larger role instances use more cores.</span></span> <span data-ttu-id="d2e56-116">Je možné škálovat aplikaci pouze v rámci limitu hello jader pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="d2e56-116">You can scale an application only within hello limit of cores for your subscription.</span></span> <span data-ttu-id="d2e56-117">Řekněme například, že vaše předplatné může mít 20 jader.</span><span class="sxs-lookup"><span data-stu-id="d2e56-117">For example, say your subscription has a limit of 20 cores.</span></span> <span data-ttu-id="d2e56-118">Pokud spouštíte aplikaci pomocí dvou středních cloudové služby (celkem 4 jádra), můžete pouze postupně škálovat ostatních nasazení cloudové služby ve vašem předplatném ve zbývajících 16 jader hello.</span><span class="sxs-lookup"><span data-stu-id="d2e56-118">If you run an application with two medium-sized cloud services (a total of 4 cores), you can only scale up other cloud service deployments in your subscription by hello remaining 16 cores.</span></span> <span data-ttu-id="d2e56-119">Další informace o velikosti najdete v tématu [cloudové služby velikosti](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="d2e56-119">For more information about sizes, see [Cloud Service Sizes](cloud-services-sizes-specs.md).</span></span>

* <span data-ttu-id="d2e56-120">Je možné škálovat podle prahová hodnota fronty zpráv.</span><span class="sxs-lookup"><span data-stu-id="d2e56-120">You can scale based on a queue message threshold.</span></span> <span data-ttu-id="d2e56-121">Další informace o tom, najdete v části front toouse [jak toouse hello fronty úložiště služby](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="d2e56-121">For more information about how toouse queues, see [How toouse hello Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>

* <span data-ttu-id="d2e56-122">Můžete škálovat také další prostředky spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="d2e56-122">You can also scale other resources associated with your subscription.</span></span>

* <span data-ttu-id="d2e56-123">tooenable vysokou dostupnost vaší aplikace, ujistěte se, že je nasazený s dvěma nebo více instancí role.</span><span class="sxs-lookup"><span data-stu-id="d2e56-123">tooenable high availability of your application, you should ensure that it is deployed with two or more role instances.</span></span> <span data-ttu-id="d2e56-124">Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="d2e56-124">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>


## <a name="where-scale-is-located"></a><span data-ttu-id="d2e56-125">Kde se nachází škálování</span><span class="sxs-lookup"><span data-stu-id="d2e56-125">Where scale is located</span></span>
<span data-ttu-id="d2e56-126">Po výběru cloudové služby, měli byste mít hello cloudové služby okno viditelné.</span><span class="sxs-lookup"><span data-stu-id="d2e56-126">After you select your cloud service, you should have hello cloud service blade visible.</span></span>

1. <span data-ttu-id="d2e56-127">V okně služby cloud hello, na hello **rolí a instancí** dlaždice, vyberte hello název hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="d2e56-127">On hello cloud service blade, on hello **Roles and Instances** tile, select hello name of hello cloud service.</span></span>   
   <span data-ttu-id="d2e56-128">**Důležité**: Ujistěte se, že tooclick hello cloudové služby role, není hello instanci role, která je nižší než hello role.</span><span class="sxs-lookup"><span data-stu-id="d2e56-128">**IMPORTANT**: Make sure tooclick hello cloud service role, not hello role instance that is below hello role.</span></span>

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)
2. <span data-ttu-id="d2e56-129">Vyberte hello **škálování** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="d2e56-129">Select hello **scale** tile.</span></span>

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a><span data-ttu-id="d2e56-130">Automatické škálování</span><span class="sxs-lookup"><span data-stu-id="d2e56-130">Automatic scale</span></span>
<span data-ttu-id="d2e56-131">Nastavení škálování v roli lze nakonfigurovat buď dva režimy **ruční** nebo **automatické**.</span><span class="sxs-lookup"><span data-stu-id="d2e56-131">You can configure scale settings for a role with either two modes **manual** or **automatic**.</span></span> <span data-ttu-id="d2e56-132">Ruční je, jak jste zvyklí, nastavíte hello absolutní počet instancí.</span><span class="sxs-lookup"><span data-stu-id="d2e56-132">Manual is as you would expect, you set hello absolute count of instances.</span></span> <span data-ttu-id="d2e56-133">Automatické ale umožňuje tooset, které by měl škálování pravidla, které řídí způsob a jak mnohem je.</span><span class="sxs-lookup"><span data-stu-id="d2e56-133">Automatic however allows you tooset rules that govern how and by how much you should scale.</span></span>

<span data-ttu-id="d2e56-134">Sada hello **škálovat podle** možnost příliš**pravidla plánu a výkonu**.</span><span class="sxs-lookup"><span data-stu-id="d2e56-134">Set hello **Scale by** option too**schedule and performance rules**.</span></span>

![Nastavení škálování cloudové služby s profil a pravidla](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. <span data-ttu-id="d2e56-136">Existující profil.</span><span class="sxs-lookup"><span data-stu-id="d2e56-136">An existing profile.</span></span>
2. <span data-ttu-id="d2e56-137">Přidáte pravidlo pro profil nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="d2e56-137">Add a rule for hello parent profile.</span></span>
3. <span data-ttu-id="d2e56-138">Přidejte jiný profil.</span><span class="sxs-lookup"><span data-stu-id="d2e56-138">Add another profile.</span></span>

<span data-ttu-id="d2e56-139">Vyberte **přidat profil**.</span><span class="sxs-lookup"><span data-stu-id="d2e56-139">Select **Add Profile**.</span></span> <span data-ttu-id="d2e56-140">Hello profil určuje režimu, který chcete použít pro škálování hello toouse: **vždy**, **opakování**, **pevným datem**.</span><span class="sxs-lookup"><span data-stu-id="d2e56-140">hello profile determines which mode you want toouse for hello scale: **always**, **recurrence**, **fixed date**.</span></span>

<span data-ttu-id="d2e56-141">Po dokončení konfigurace profilu hello a pravidla, vyberte hello **Uložit** ikona v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="d2e56-141">After you have configured hello profile and rules, select hello **Save** icon at hello top.</span></span>

#### <a name="profile"></a><span data-ttu-id="d2e56-142">Profil</span><span class="sxs-lookup"><span data-stu-id="d2e56-142">Profile</span></span>
<span data-ttu-id="d2e56-143">profil Hello Nastaví minimální a maximální počet instancí pro hello škálování a když tento rozsah škálování je také aktivní.</span><span class="sxs-lookup"><span data-stu-id="d2e56-143">hello profile sets minimum and maximum instances for hello scale, and also when this scale range is active.</span></span>

* <span data-ttu-id="d2e56-144">**Vždy**</span><span class="sxs-lookup"><span data-stu-id="d2e56-144">**Always**</span></span>

    <span data-ttu-id="d2e56-145">Vždy mít tento rozsah instancí, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d2e56-145">Always keep this range of instances available.</span></span>  

    ![Cloudová služba, která vždy škálování](./media/cloud-services-how-to-scale-portal/select-always.png)
* <span data-ttu-id="d2e56-147">**Opakování**</span><span class="sxs-lookup"><span data-stu-id="d2e56-147">**Recurrence**</span></span>

    <span data-ttu-id="d2e56-148">Vyberte sadu dny v týdnu tooscale hello.</span><span class="sxs-lookup"><span data-stu-id="d2e56-148">Choose a set of days of hello week tooscale.</span></span>

    ![Cloudové služby škálování s plán opakování](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
* <span data-ttu-id="d2e56-150">**Pevné datum**</span><span class="sxs-lookup"><span data-stu-id="d2e56-150">**Fixed Date**</span></span>

    <span data-ttu-id="d2e56-151">Pevné datum rozsahu tooscale hello roli.</span><span class="sxs-lookup"><span data-stu-id="d2e56-151">A fixed date range tooscale hello role.</span></span>

    ![Cloudové služby škálování s pevnou datum](./media/cloud-services-how-to-scale-portal/select-fixed.png)

<span data-ttu-id="d2e56-153">Po nakonfigurování hello profil, vyberte hello **OK** tlačítko v hello dolní části okna profil hello.</span><span class="sxs-lookup"><span data-stu-id="d2e56-153">After you have configured hello profile, select hello **OK** button at hello bottom of hello profile blade.</span></span>

#### <a name="rule"></a><span data-ttu-id="d2e56-154">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="d2e56-154">Rule</span></span>
<span data-ttu-id="d2e56-155">Pravidla se přidají tooa profil a představují podmínku, která aktivuje hello škálování.</span><span class="sxs-lookup"><span data-stu-id="d2e56-155">Rules are added tooa profile and represent a condition that triggers hello scale.</span></span>

<span data-ttu-id="d2e56-156">aktivační událost pravidlo Hello je založena na metrika hello cloudové služby (využití procesoru, disková aktivita nebo síťové aktivity) toowhich podmíněného hodnotu můžete přidat.</span><span class="sxs-lookup"><span data-stu-id="d2e56-156">hello rule trigger is based on a metric of hello cloud service (CPU usage, disk activity, or network activity) toowhich you can add a conditional value.</span></span> <span data-ttu-id="d2e56-157">Kromě toho může mít hello aktivační události podle zprávy fronty nebo hello metrikou jiný Azure prostředek spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="d2e56-157">Additionally you can have hello trigger based on a message queue or hello metric of some other Azure resource associated with your subscription.</span></span>

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

<span data-ttu-id="d2e56-158">Po nakonfigurování hello pravidla, vyberte hello **OK** tlačítko v hello dolní části okna pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="d2e56-158">After you have configured hello rule, select hello **OK** button at hello bottom of hello rule blade.</span></span>

## <a name="back-toomanual-scale"></a><span data-ttu-id="d2e56-159">Zpět toomanual škálování</span><span class="sxs-lookup"><span data-stu-id="d2e56-159">Back toomanual scale</span></span>
<span data-ttu-id="d2e56-160">Přejděte toohello [nastavení škálování](#where-scale-is-located) a sadu hello **škálovat podle** možnost příliš**počet instancí, který nastavím ručně**.</span><span class="sxs-lookup"><span data-stu-id="d2e56-160">Navigate toohello [scale settings](#where-scale-is-located) and set hello **Scale by** option too**an instance count that I enter manually**.</span></span>

![Nastavení škálování cloudové služby s profil a pravidla](./media/cloud-services-how-to-scale-portal/manual-basics.png)

<span data-ttu-id="d2e56-162">Toto nastavení se odebere z hello role automatické škálování a potom můžete nastavit počet instancí hello přímo.</span><span class="sxs-lookup"><span data-stu-id="d2e56-162">This setting removes automated scaling from hello role and then you can set hello instance count directly.</span></span>

1. <span data-ttu-id="d2e56-163">možnost Hello škálování (ruční nebo automatické).</span><span class="sxs-lookup"><span data-stu-id="d2e56-163">hello scale (manual or automated) option.</span></span>
2. <span data-ttu-id="d2e56-164">Role instance posuvníku tooset hello instancí tooscale k.</span><span class="sxs-lookup"><span data-stu-id="d2e56-164">A role instance slider tooset hello instances tooscale to.</span></span>
3. <span data-ttu-id="d2e56-165">Instance role tooscale hello k.</span><span class="sxs-lookup"><span data-stu-id="d2e56-165">Instances of hello role tooscale to.</span></span>

<span data-ttu-id="d2e56-166">Po nakonfigurování nastavení škálování hello vyberte hello **Uložit** ikona v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="d2e56-166">After you have configured hello scale settings, select hello **Save** icon at hello top.</span></span>
