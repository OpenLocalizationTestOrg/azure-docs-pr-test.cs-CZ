---
title: "Automatické škálování cloudové služby na portálu classic | Microsoft Docs"
description: "(klasické) Další informace o použití portálu classic konfigurace pravidel automatického škálování pro cloudové služby webovou roli nebo role pracovního procesu v Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: eb167d70-4eba-42a4-b157-d8d0688abf4b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 90d55bbac6e113d6add848ace67cf0749e26342b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-classic-portal"></a><span data-ttu-id="d9eff-103">Postup konfigurace automatického škálování pro cloudové služby na portálu classic</span><span class="sxs-lookup"><span data-stu-id="d9eff-103">How to configure auto scaling for a Cloud Service in the classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d9eff-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d9eff-104">Azure portal</span></span>](cloud-services-how-to-scale-portal.md)
> * [<span data-ttu-id="d9eff-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="d9eff-105">Azure classic portal</span></span>](cloud-services-how-to-scale.md)

<span data-ttu-id="d9eff-106">Na stránce škálování portálu Azure classic můžete nakonfigurovat nastavení automatického škálování pro vaši webovou roli nebo role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="d9eff-106">On the Scale page of the Azure classic portal, you can configure automatic scale settings for your web role or worker role.</span></span> <span data-ttu-id="d9eff-107">Alternativně můžete nakonfigurovat, ruční škálování místo založeného na pravidlech automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="d9eff-107">Alternatively, you can configure manual scaling instead of rules-based automatic scaling.</span></span>

> [!NOTE]
> <span data-ttu-id="d9eff-108">Tento článek se zaměřuje na webových a pracovních rolí cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="d9eff-108">This article focuses on Cloud Service web and worker roles.</span></span> <span data-ttu-id="d9eff-109">Když vytvoříte virtuální počítač (klasický) přímo, je hostovaná v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="d9eff-109">When you create a virtual machine (classic) directly, it is hosted in a cloud service.</span></span> <span data-ttu-id="d9eff-110">Některé z těchto informací se vztahuje na tyto typy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d9eff-110">Some of this information applies to these types of virtual machines.</span></span> <span data-ttu-id="d9eff-111">Škálování skupinu dostupnosti virtuálních počítačů se právě vypíná je zapnout a vypnout na základě pravidel škálování, které nakonfigurujete.</span><span class="sxs-lookup"><span data-stu-id="d9eff-111">Scaling an availability set of virtual machines is just shutting them on and off based on the scale rules you configure.</span></span> <span data-ttu-id="d9eff-112">Další informace o virtuální počítače a skupiny dostupnosti najdete v tématu [Správa dostupnosti virtuálních počítačů](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="d9eff-112">For more information about Virtual Machines and availability sets, see [Manage the Availability of Virtual Machines](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="d9eff-113">Před konfigurací škálování pro vaši aplikaci je třeba zvážit následující informace:</span><span class="sxs-lookup"><span data-stu-id="d9eff-113">You should consider the following information before you configure scaling for your application:</span></span>

* <span data-ttu-id="d9eff-114">Škálování má vliv základní použití.</span><span class="sxs-lookup"><span data-stu-id="d9eff-114">Scaling is affected by core usage.</span></span>

    <span data-ttu-id="d9eff-115">Větší instance rolí pomocí více jader.</span><span class="sxs-lookup"><span data-stu-id="d9eff-115">Larger role instances use more cores.</span></span> <span data-ttu-id="d9eff-116">Je možné škálovat aplikaci pouze v rámci maximální počet jader pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="d9eff-116">You can scale an application only within the limit of cores for your subscription.</span></span> <span data-ttu-id="d9eff-117">Řekněme například, že vaše předplatné může mít 20 jader.</span><span class="sxs-lookup"><span data-stu-id="d9eff-117">For example, say your subscription has a limit of 20 cores.</span></span> <span data-ttu-id="d9eff-118">Pokud spouštíte aplikaci pomocí dvou středních cloudové služby (celkem 4 jádra), můžete pouze postupně škálovat ostatních nasazení cloudové služby ve vašem předplatném ve zbývajících 16 jader.</span><span class="sxs-lookup"><span data-stu-id="d9eff-118">If you run an application with two medium-sized cloud services (a total of 4 cores), you can only scale up other cloud service deployments in your subscription by the remaining 16 cores.</span></span> <span data-ttu-id="d9eff-119">Další informace o velikosti najdete v tématu [cloudové služby velikosti](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="d9eff-119">For more information about sizes, see [Cloud Service Sizes](cloud-services-sizes-specs.md).</span></span>

* <span data-ttu-id="d9eff-120">Musíte vytvořit frontu a přidružte ji k roli předtím, než je možné škálovat aplikaci pro zprávu prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d9eff-120">You must create a queue and associate it with a role before you can scale an application based on a message threshold.</span></span> <span data-ttu-id="d9eff-121">Další informace najdete v tématu [jak používat fronty služby úložiště](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="d9eff-121">For more information, see [How to use the Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>

* <span data-ttu-id="d9eff-122">Je možné škálovat prostředky, které jsou propojeny s cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="d9eff-122">You can scale resources that are linked to your cloud service.</span></span> <span data-ttu-id="d9eff-123">Další informace o propojení prostředků, najdete v tématu [postupy: odkaz prostředek cloudové služby](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="d9eff-123">For more information about linking resources, see [How to: Link a resource to a cloud service](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).</span></span>

* <span data-ttu-id="d9eff-124">Povolit vysokou dostupnost vaší aplikace, se ujistěte, že je nasazený s dvěma nebo více instancí role.</span><span class="sxs-lookup"><span data-stu-id="d9eff-124">To enable high availability of your application, you should ensure that it is deployed with two or more role instances.</span></span> <span data-ttu-id="d9eff-125">Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="d9eff-125">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="schedule-scaling"></a><span data-ttu-id="d9eff-126">Plánování škálování</span><span class="sxs-lookup"><span data-stu-id="d9eff-126">Schedule scaling</span></span>
<span data-ttu-id="d9eff-127">Ve výchozím nastavení všechny role neprovádějte určitého plánu.</span><span class="sxs-lookup"><span data-stu-id="d9eff-127">By default, all roles do not follow a specific schedule.</span></span> <span data-ttu-id="d9eff-128">Proto žádné nastavení změnit platit pro všechny časy a všechny dny po celý rok.</span><span class="sxs-lookup"><span data-stu-id="d9eff-128">Therefore, any settings changed apply to all times and all days throughout the year.</span></span> <span data-ttu-id="d9eff-129">Pokud chcete, můžete nastavit ruční nebo automatické škálování pro jednu z následujících režimů:</span><span class="sxs-lookup"><span data-stu-id="d9eff-129">If you want, you can setup manual or automatic scaling for one of the following modes:</span></span>

* <span data-ttu-id="d9eff-130">Dny v týdnu</span><span class="sxs-lookup"><span data-stu-id="d9eff-130">Weekdays</span></span>
* <span data-ttu-id="d9eff-131">Víkendů</span><span class="sxs-lookup"><span data-stu-id="d9eff-131">Weekends</span></span>
* <span data-ttu-id="d9eff-132">Týden noci</span><span class="sxs-lookup"><span data-stu-id="d9eff-132">Week nights</span></span>
* <span data-ttu-id="d9eff-133">Ráno týden</span><span class="sxs-lookup"><span data-stu-id="d9eff-133">Week mornings</span></span>
* <span data-ttu-id="d9eff-134">Konkrétní kalendářní data</span><span class="sxs-lookup"><span data-stu-id="d9eff-134">Specific dates</span></span>
* <span data-ttu-id="d9eff-135">Konkrétní rozsahy dat</span><span class="sxs-lookup"><span data-stu-id="d9eff-135">Specific date ranges</span></span>

<span data-ttu-id="d9eff-136">Nastavení plánu je nakonfigurované [portál Azure classic](https://manage.windowsazure.com/) na</span><span class="sxs-lookup"><span data-stu-id="d9eff-136">The schedule setting is configured in the [Azure classic portal](https://manage.windowsazure.com/) on the</span></span>  
<span data-ttu-id="d9eff-137">**Cloudové služby** > **\[cloudové služby\]** > **škálování**  >   **\[Produkční nebo pracovní\]**  stránky.</span><span class="sxs-lookup"><span data-stu-id="d9eff-137">**Cloud Services** > **\[Your cloud service\]** > **Scale** > **\[Production or Staging\]** page.</span></span>

<span data-ttu-id="d9eff-138">Klikněte **časů plánu** tlačítko pro každou roli, kterou chcete změnit.</span><span class="sxs-lookup"><span data-stu-id="d9eff-138">Click the **set up schedule times** button for each role you want to change.</span></span>

![Cloudové služby Automatické škálování podle plánu][scale_schedules]

## <a name="manual-scale"></a><span data-ttu-id="d9eff-140">Ruční škálování</span><span class="sxs-lookup"><span data-stu-id="d9eff-140">Manual scale</span></span>
<span data-ttu-id="d9eff-141">Na **škálování** stránky, můžete ručně zvýšit nebo snížit počet spuštěných instancí v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="d9eff-141">On the **Scale** page, you can manually increase or decrease the number of running instances in a cloud service.</span></span> <span data-ttu-id="d9eff-142">Toto nastavení je nakonfigurovat pro každý plán, který jste vytvořili nebo celou dobu, pokud jste dosud nevytvořili plánu.</span><span class="sxs-lookup"><span data-stu-id="d9eff-142">This setting is configured for each schedule you've created or to all time if you have not created a schedule.</span></span>

1. <span data-ttu-id="d9eff-143">V [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a pak klikněte na název cloudové služby za účelem otevře řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="d9eff-143">In the [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click the name of the cloud service to open the dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="d9eff-144">Pokud nevidíte cloudové služby, budete muset změnit z **produkční** k **pracovní** nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="d9eff-144">If you don't see your cloud service, you may need to change from **Production** to **Staging** or vice versa.</span></span>

2. <span data-ttu-id="d9eff-145">Klikněte na tlačítko **škálování**.</span><span class="sxs-lookup"><span data-stu-id="d9eff-145">Click **Scale**.</span></span>
3. <span data-ttu-id="d9eff-146">Vyberte plán, kterou chcete změnit možnosti škálování.</span><span class="sxs-lookup"><span data-stu-id="d9eff-146">Select the schedule you want to change scaling options for.</span></span> <span data-ttu-id="d9eff-147">*Ne naplánovaném čase* je výchozí, pokud máte žádné plány definované.</span><span class="sxs-lookup"><span data-stu-id="d9eff-147">*No scheduled times* is the default if you have no schedules defined.</span></span>
4. <span data-ttu-id="d9eff-148">Najít **škálování podle metriky** a vyberte **NONE**.</span><span class="sxs-lookup"><span data-stu-id="d9eff-148">Find the **Scale by metric** section and select **NONE**.</span></span> <span data-ttu-id="d9eff-149">Toto nastavení je výchozí nastavení pro všechny role.</span><span class="sxs-lookup"><span data-stu-id="d9eff-149">This setting is the default for all roles.</span></span>
5. <span data-ttu-id="d9eff-150">Každá role v rámci cloudové služby má jezdce pro změnu počtu instancí používat.</span><span class="sxs-lookup"><span data-stu-id="d9eff-150">Each role in the cloud service has a slider for changing the number of instances to use.</span></span>
   
    ![Ručně škálovat, roli cloudové služby][manual_scale]
   
    <span data-ttu-id="d9eff-152">Pokud potřebujete více instancí, budete muset změnit [cloudu velikost virtuálního počítače služby](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="d9eff-152">If you need more instances, you may need to change the [cloud service virtual machine size](cloud-services-sizes-specs.md).</span></span>
6. <span data-ttu-id="d9eff-153">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d9eff-153">Click **Save**.</span></span>  
   <span data-ttu-id="d9eff-154">Instance role jsou přidat nebo odebrat závislosti na vybrané položky.</span><span class="sxs-lookup"><span data-stu-id="d9eff-154">Role instances are added or removed based on your selections.</span></span>

> [!TIP]
> <span data-ttu-id="d9eff-155">Vždy, když se zobrazí ![][tip_icon] umožňující ho a můžete získat nápovědu ke konkrétní nastavení nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="d9eff-155">Whenever you see ![][tip_icon] move your mouse to it and you can get help about what a specific setting does.</span></span>

## <a name="automatic-scale---cpu"></a><span data-ttu-id="d9eff-156">Automatické škálování - procesoru</span><span class="sxs-lookup"><span data-stu-id="d9eff-156">Automatic scale - CPU</span></span>
<span data-ttu-id="d9eff-157">Tento režim škáluje, pokud průměrná procento využití procesoru přejde nad nebo pod zadaných prahových hodnot.</span><span class="sxs-lookup"><span data-stu-id="d9eff-157">This mode scales if the average percentage of CPU usage goes above or below specified thresholds.</span></span> <span data-ttu-id="d9eff-158">Instance role jsou vytvořené nebo odstraněné v takovém případě.</span><span class="sxs-lookup"><span data-stu-id="d9eff-158">Role instances are created or deleted when this happens.</span></span>

1. <span data-ttu-id="d9eff-159">V [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a pak klikněte na název cloudové služby za účelem otevře řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="d9eff-159">In the [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click the name of the cloud service to open the dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="d9eff-160">Pokud nevidíte cloudové služby, budete muset změnit z **produkční** k **pracovní** nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="d9eff-160">If you don't see your cloud service, you may need to change from **Production** to **Staging** or vice versa.</span></span>

2. <span data-ttu-id="d9eff-161">Klikněte na tlačítko **škálování**.</span><span class="sxs-lookup"><span data-stu-id="d9eff-161">Click **Scale**.</span></span>
3. <span data-ttu-id="d9eff-162">Vyberte plán, kterou chcete změnit možnosti škálování.</span><span class="sxs-lookup"><span data-stu-id="d9eff-162">Select the schedule you want to change scaling options for.</span></span> <span data-ttu-id="d9eff-163">*Ne naplánovaném čase* je výchozí, pokud máte žádné plány definované.</span><span class="sxs-lookup"><span data-stu-id="d9eff-163">*No scheduled times* is the default if you have no schedules defined.</span></span>
4. <span data-ttu-id="d9eff-164">Najít **škálování podle metriky** a vyberte **procesoru**.</span><span class="sxs-lookup"><span data-stu-id="d9eff-164">Find the **Scale by metric** section and select **CPU**.</span></span>
5. <span data-ttu-id="d9eff-165">Teď můžete nakonfigurovat minimální a maximální rozsah instancí role, cíl využití procesoru (pro aktivaci škálování nahoru) a jak velký počet instancí škálovat nahoru a dolů pomocí.</span><span class="sxs-lookup"><span data-stu-id="d9eff-165">Now you can configure a minimum and maximum range of roles instances, the target CPU usage (to trigger a scale up), and how many instances to scale up and down by.</span></span>

![Škálování role cloudové služby podle zatížení procesoru][cpu_scale]

> [!TIP]
> <span data-ttu-id="d9eff-167">Vždy, když se zobrazí ![][tip_icon] umožňující ho a můžete získat nápovědu ke konkrétní nastavení nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="d9eff-167">Whenever you see ![][tip_icon] move your mouse to it and you can get help about what a specific setting does.</span></span>

## <a name="automatic-scale---queue"></a><span data-ttu-id="d9eff-168">Automatické škálování - fronty</span><span class="sxs-lookup"><span data-stu-id="d9eff-168">Automatic scale - Queue</span></span>
<span data-ttu-id="d9eff-169">Tento režim automaticky přizpůsobí, pokud počet zpráv ve frontě přejde nad nebo pod zadanou prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d9eff-169">This mode automatically scales if the number of messages in a queue goes above or below a specified threshold.</span></span> <span data-ttu-id="d9eff-170">Instance role jsou vytvořené nebo odstraněné v takovém případě.</span><span class="sxs-lookup"><span data-stu-id="d9eff-170">Role instances are created or deleted when this happens.</span></span>

1. <span data-ttu-id="d9eff-171">V [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a pak klikněte na název cloudové služby za účelem otevře řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="d9eff-171">In the [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click the name of the cloud service to open the dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="d9eff-172">Pokud nevidíte cloudové služby, budete muset změnit z **produkční** k **pracovní** nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="d9eff-172">If you don't see your cloud service, you may need to change from **Production** to **Staging** or vice versa.</span></span>

2. <span data-ttu-id="d9eff-173">Klikněte na tlačítko **škálování**.</span><span class="sxs-lookup"><span data-stu-id="d9eff-173">Click **Scale**.</span></span>
3. <span data-ttu-id="d9eff-174">Najít **škálování podle metriky** a vyberte **fronty**.</span><span class="sxs-lookup"><span data-stu-id="d9eff-174">Find the **Scale by metric** section and select **QUEUE**.</span></span>
4. <span data-ttu-id="d9eff-175">Teď můžete nakonfigurovat minimální a maximální rozsah instancí role, fronty a počet zpráv fronty pro zpracování pro každou instanci a kolik instancí škálovat nahoru a dolů pomocí.</span><span class="sxs-lookup"><span data-stu-id="d9eff-175">Now you can configure a minimum and maximum range of roles instances, the queue and number of queue messages to process for each instance, and how many instances to scale up and down by.</span></span>

![Škálovat roli cloudové služby, protože fronta zpráv][queue_scale]

> [!TIP]
> <span data-ttu-id="d9eff-177">Vždy, když se zobrazí ![][tip_icon] umožňující ho a můžete získat nápovědu ke konkrétní nastavení nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="d9eff-177">Whenever you see ![][tip_icon] move your mouse to it and you can get help about what a specific setting does.</span></span>

## <a name="scale-linked-resources"></a><span data-ttu-id="d9eff-178">Škálování odkazované zdroje.</span><span class="sxs-lookup"><span data-stu-id="d9eff-178">Scale linked resources</span></span>
<span data-ttu-id="d9eff-179">Často při změně měřítka roli, je výhodné se škálovat databázi, který také používá aplikace.</span><span class="sxs-lookup"><span data-stu-id="d9eff-179">Often when you scale a role, it's beneficial to scale the database that the application is using also.</span></span> <span data-ttu-id="d9eff-180">Pokud jste databáze ke cloudové službě, můžete kliknutím na příslušný odkaz přístup nastavení škálování pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="d9eff-180">If you link the database to the cloud service, you can access the scaling settings for that resource by clicking the appropriate link.</span></span>

1. <span data-ttu-id="d9eff-181">V [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a pak klikněte na název cloudové služby za účelem otevře řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="d9eff-181">In the [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click the name of the cloud service to open the dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="d9eff-182">Pokud nevidíte cloudové služby, budete muset změnit z **produkční** k **pracovní** nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="d9eff-182">If you don't see your cloud service, you may need to change from **Production** to **Staging** or vice versa.</span></span>

2. <span data-ttu-id="d9eff-183">Klikněte na tlačítko **škálování**.</span><span class="sxs-lookup"><span data-stu-id="d9eff-183">Click **Scale**.</span></span>
3. <span data-ttu-id="d9eff-184">Najít **propojené prostředky** části a klikněte na tlačítko **spravovat škálování pro tuto databázi**.</span><span class="sxs-lookup"><span data-stu-id="d9eff-184">Find the **linked resources** section and click **Manage scale for this database**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d9eff-185">Pokud se nezobrazí **propojené prostředky** části pravděpodobně nemáte žádné propojené prostředky.</span><span class="sxs-lookup"><span data-stu-id="d9eff-185">If you don't see a **linked resources** section, you probably do not have any linked resources.</span></span>

![][linked_resource]

[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
