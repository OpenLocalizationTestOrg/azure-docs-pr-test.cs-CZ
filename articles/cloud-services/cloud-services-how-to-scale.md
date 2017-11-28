---
title: "aaaAuto škálování cloudové služby na portálu classic hello | Microsoft Docs"
description: "(klasické) Zjistěte, jak toouse hello portálu classic tooconfigure automatické škálování pravidla pro cloudové služby webovou roli nebo role pracovního procesu v Azure."
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
ms.openlocfilehash: ddb5816d4d22192c6d2f51d7508e390779742078
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-auto-scaling-for-a-cloud-service-in-hello-classic-portal"></a><span data-ttu-id="3cf90-103">Jak tooconfigure automatické škálování pro cloudové služby na portálu classic hello</span><span class="sxs-lookup"><span data-stu-id="3cf90-103">How tooconfigure auto scaling for a Cloud Service in hello classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3cf90-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3cf90-104">Azure portal</span></span>](cloud-services-how-to-scale-portal.md)
> * [<span data-ttu-id="3cf90-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="3cf90-105">Azure classic portal</span></span>](cloud-services-how-to-scale.md)

<span data-ttu-id="3cf90-106">Na stránce škálování hello hello portál Azure classic můžete nakonfigurovat nastavení automatického škálování pro vaši webovou roli nebo role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="3cf90-106">On hello Scale page of hello Azure classic portal, you can configure automatic scale settings for your web role or worker role.</span></span> <span data-ttu-id="3cf90-107">Alternativně můžete nakonfigurovat, ruční škálování místo založeného na pravidlech automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="3cf90-107">Alternatively, you can configure manual scaling instead of rules-based automatic scaling.</span></span>

> [!NOTE]
> <span data-ttu-id="3cf90-108">Tento článek se zaměřuje na webových a pracovních rolí cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="3cf90-108">This article focuses on Cloud Service web and worker roles.</span></span> <span data-ttu-id="3cf90-109">Když vytvoříte virtuální počítač (klasický) přímo, je hostovaná v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="3cf90-109">When you create a virtual machine (classic) directly, it is hosted in a cloud service.</span></span> <span data-ttu-id="3cf90-110">Některé z těchto informací se vztahuje toothese typy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3cf90-110">Some of this information applies toothese types of virtual machines.</span></span> <span data-ttu-id="3cf90-111">Škálování skupinu dostupnosti virtuálních počítačů se právě vypíná je zapnout a vypnout na základě pravidel škálování hello, kterou konfigurujete.</span><span class="sxs-lookup"><span data-stu-id="3cf90-111">Scaling an availability set of virtual machines is just shutting them on and off based on hello scale rules you configure.</span></span> <span data-ttu-id="3cf90-112">Další informace o virtuální počítače a skupiny dostupnosti najdete v tématu [hello Správa dostupnosti virtuálních počítačů](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="3cf90-112">For more information about Virtual Machines and availability sets, see [Manage hello Availability of Virtual Machines](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="3cf90-113">Měli byste zvážit hello před konfigurací škálování pro vaši aplikaci následující informace:</span><span class="sxs-lookup"><span data-stu-id="3cf90-113">You should consider hello following information before you configure scaling for your application:</span></span>

* <span data-ttu-id="3cf90-114">Škálování má vliv základní použití.</span><span class="sxs-lookup"><span data-stu-id="3cf90-114">Scaling is affected by core usage.</span></span>

    <span data-ttu-id="3cf90-115">Větší instance rolí pomocí více jader.</span><span class="sxs-lookup"><span data-stu-id="3cf90-115">Larger role instances use more cores.</span></span> <span data-ttu-id="3cf90-116">Je možné škálovat aplikaci pouze v rámci limitu hello jader pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="3cf90-116">You can scale an application only within hello limit of cores for your subscription.</span></span> <span data-ttu-id="3cf90-117">Řekněme například, že vaše předplatné může mít 20 jader.</span><span class="sxs-lookup"><span data-stu-id="3cf90-117">For example, say your subscription has a limit of 20 cores.</span></span> <span data-ttu-id="3cf90-118">Pokud spouštíte aplikaci pomocí dvou středních cloudové služby (celkem 4 jádra), můžete pouze postupně škálovat ostatních nasazení cloudové služby ve vašem předplatném ve zbývajících 16 jader hello.</span><span class="sxs-lookup"><span data-stu-id="3cf90-118">If you run an application with two medium-sized cloud services (a total of 4 cores), you can only scale up other cloud service deployments in your subscription by hello remaining 16 cores.</span></span> <span data-ttu-id="3cf90-119">Další informace o velikosti najdete v tématu [cloudové služby velikosti](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="3cf90-119">For more information about sizes, see [Cloud Service Sizes](cloud-services-sizes-specs.md).</span></span>

* <span data-ttu-id="3cf90-120">Musíte vytvořit frontu a přidružte ji k roli předtím, než je možné škálovat aplikaci pro zprávu prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3cf90-120">You must create a queue and associate it with a role before you can scale an application based on a message threshold.</span></span> <span data-ttu-id="3cf90-121">Další informace najdete v tématu [jak toouse hello fronty úložiště služby](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="3cf90-121">For more information, see [How toouse hello Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>

* <span data-ttu-id="3cf90-122">Je možné škálovat prostředky, které jsou propojené tooyour Cloudová služba.</span><span class="sxs-lookup"><span data-stu-id="3cf90-122">You can scale resources that are linked tooyour cloud service.</span></span> <span data-ttu-id="3cf90-123">Další informace o propojení prostředků, najdete v tématu [postupy: odkaz prostředků tooa Cloudová služba](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="3cf90-123">For more information about linking resources, see [How to: Link a resource tooa cloud service](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).</span></span>

* <span data-ttu-id="3cf90-124">tooenable vysokou dostupnost vaší aplikace, ujistěte se, že je nasazený s dvěma nebo více instancí role.</span><span class="sxs-lookup"><span data-stu-id="3cf90-124">tooenable high availability of your application, you should ensure that it is deployed with two or more role instances.</span></span> <span data-ttu-id="3cf90-125">Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="3cf90-125">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="schedule-scaling"></a><span data-ttu-id="3cf90-126">Plánování škálování</span><span class="sxs-lookup"><span data-stu-id="3cf90-126">Schedule scaling</span></span>
<span data-ttu-id="3cf90-127">Ve výchozím nastavení všechny role neprovádějte určitého plánu.</span><span class="sxs-lookup"><span data-stu-id="3cf90-127">By default, all roles do not follow a specific schedule.</span></span> <span data-ttu-id="3cf90-128">Proto nastavení změnit platit tooall časy a všechny dny po celý rok hello.</span><span class="sxs-lookup"><span data-stu-id="3cf90-128">Therefore, any settings changed apply tooall times and all days throughout hello year.</span></span> <span data-ttu-id="3cf90-129">Pokud chcete, můžete nastavit ruční nebo automatické škálování pro jednu z následujících režimů hello:</span><span class="sxs-lookup"><span data-stu-id="3cf90-129">If you want, you can setup manual or automatic scaling for one of hello following modes:</span></span>

* <span data-ttu-id="3cf90-130">Dny v týdnu</span><span class="sxs-lookup"><span data-stu-id="3cf90-130">Weekdays</span></span>
* <span data-ttu-id="3cf90-131">Víkendů</span><span class="sxs-lookup"><span data-stu-id="3cf90-131">Weekends</span></span>
* <span data-ttu-id="3cf90-132">Týden noci</span><span class="sxs-lookup"><span data-stu-id="3cf90-132">Week nights</span></span>
* <span data-ttu-id="3cf90-133">Ráno týden</span><span class="sxs-lookup"><span data-stu-id="3cf90-133">Week mornings</span></span>
* <span data-ttu-id="3cf90-134">Konkrétní kalendářní data</span><span class="sxs-lookup"><span data-stu-id="3cf90-134">Specific dates</span></span>
* <span data-ttu-id="3cf90-135">Konkrétní rozsahy dat</span><span class="sxs-lookup"><span data-stu-id="3cf90-135">Specific date ranges</span></span>

<span data-ttu-id="3cf90-136">Hello plán je nastavena v hello [portál Azure classic](https://manage.windowsazure.com/) na hello</span><span class="sxs-lookup"><span data-stu-id="3cf90-136">hello schedule setting is configured in hello [Azure classic portal](https://manage.windowsazure.com/) on hello</span></span>  
<span data-ttu-id="3cf90-137">**Cloudové služby** > **\[cloudové služby\]** > **škálování**  >   **\[Produkční nebo pracovní\]**  stránky.</span><span class="sxs-lookup"><span data-stu-id="3cf90-137">**Cloud Services** > **\[Your cloud service\]** > **Scale** > **\[Production or Staging\]** page.</span></span>

<span data-ttu-id="3cf90-138">Klikněte na tlačítko hello **časů plánu** tlačítko pro každou roli chcete toochange.</span><span class="sxs-lookup"><span data-stu-id="3cf90-138">Click hello **set up schedule times** button for each role you want toochange.</span></span>

![Cloudové služby Automatické škálování podle plánu][scale_schedules]

## <a name="manual-scale"></a><span data-ttu-id="3cf90-140">Ruční škálování</span><span class="sxs-lookup"><span data-stu-id="3cf90-140">Manual scale</span></span>
<span data-ttu-id="3cf90-141">Na hello **škálování** stránky, můžete ručně zvýšit nebo snížit počet hello spuštěné instance v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="3cf90-141">On hello **Scale** page, you can manually increase or decrease hello number of running instances in a cloud service.</span></span> <span data-ttu-id="3cf90-142">Toto nastavení je nakonfigurovat pro každý plán, který jste vytvořili nebo tooall čas, pokud jste dosud nevytvořili plánu.</span><span class="sxs-lookup"><span data-stu-id="3cf90-142">This setting is configured for each schedule you've created or tooall time if you have not created a schedule.</span></span>

1. <span data-ttu-id="3cf90-143">V hello [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a potom klikněte na název hello hello řídicí panel hello tooopen cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="3cf90-143">In hello [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click hello name of hello cloud service tooopen hello dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="3cf90-144">Pokud nevidíte cloudové služby, pravděpodobně bude třeba toochange z **produkční** příliš**pracovní** nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="3cf90-144">If you don't see your cloud service, you may need toochange from **Production** too**Staging** or vice versa.</span></span>

2. <span data-ttu-id="3cf90-145">Klikněte na tlačítko **škálování**.</span><span class="sxs-lookup"><span data-stu-id="3cf90-145">Click **Scale**.</span></span>
3. <span data-ttu-id="3cf90-146">Vyberte plán hello chcete toochange možnosti škálování.</span><span class="sxs-lookup"><span data-stu-id="3cf90-146">Select hello schedule you want toochange scaling options for.</span></span> <span data-ttu-id="3cf90-147">*Ne naplánovaném čase* je výchozí hello, pokud máte žádné plány definované.</span><span class="sxs-lookup"><span data-stu-id="3cf90-147">*No scheduled times* is hello default if you have no schedules defined.</span></span>
4. <span data-ttu-id="3cf90-148">Najde hello **škálování podle metriky** a vyberte **NONE**.</span><span class="sxs-lookup"><span data-stu-id="3cf90-148">Find hello **Scale by metric** section and select **NONE**.</span></span> <span data-ttu-id="3cf90-149">Toto nastavení je výchozí hello u všech rolí.</span><span class="sxs-lookup"><span data-stu-id="3cf90-149">This setting is hello default for all roles.</span></span>
5. <span data-ttu-id="3cf90-150">Ke každé roli v hello cloudové služby jsou pro změnu hello počet instancí toouse jezdce.</span><span class="sxs-lookup"><span data-stu-id="3cf90-150">Each role in hello cloud service has a slider for changing hello number of instances toouse.</span></span>
   
    ![Ručně škálovat, roli cloudové služby][manual_scale]
   
    <span data-ttu-id="3cf90-152">Pokud potřebujete další instance, pravděpodobně bude třeba toochange hello [cloudu velikost virtuálního počítače služby](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="3cf90-152">If you need more instances, you may need toochange hello [cloud service virtual machine size](cloud-services-sizes-specs.md).</span></span>
6. <span data-ttu-id="3cf90-153">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3cf90-153">Click **Save**.</span></span>  
   <span data-ttu-id="3cf90-154">Instance role jsou přidat nebo odebrat závislosti na vybrané položky.</span><span class="sxs-lookup"><span data-stu-id="3cf90-154">Role instances are added or removed based on your selections.</span></span>

> [!TIP]
> <span data-ttu-id="3cf90-155">Vždy, když se zobrazí ![][tip_icon] přesunout vaše tooit myši a může získat nápovědu ke konkrétní nastavení nemá.</span><span class="sxs-lookup"><span data-stu-id="3cf90-155">Whenever you see ![][tip_icon] move your mouse tooit and you can get help about what a specific setting does.</span></span>

## <a name="automatic-scale---cpu"></a><span data-ttu-id="3cf90-156">Automatické škálování - procesoru</span><span class="sxs-lookup"><span data-stu-id="3cf90-156">Automatic scale - CPU</span></span>
<span data-ttu-id="3cf90-157">Tento režim škáluje v případě hello průměrnou procentuální hodnotu využití procesoru nad nebo pod zadaných prahových hodnot.</span><span class="sxs-lookup"><span data-stu-id="3cf90-157">This mode scales if hello average percentage of CPU usage goes above or below specified thresholds.</span></span> <span data-ttu-id="3cf90-158">Instance role jsou vytvořené nebo odstraněné v takovém případě.</span><span class="sxs-lookup"><span data-stu-id="3cf90-158">Role instances are created or deleted when this happens.</span></span>

1. <span data-ttu-id="3cf90-159">V hello [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a potom klikněte na název hello hello řídicí panel hello tooopen cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="3cf90-159">In hello [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click hello name of hello cloud service tooopen hello dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="3cf90-160">Pokud nevidíte cloudové služby, pravděpodobně bude třeba toochange z **produkční** příliš**pracovní** nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="3cf90-160">If you don't see your cloud service, you may need toochange from **Production** too**Staging** or vice versa.</span></span>

2. <span data-ttu-id="3cf90-161">Klikněte na tlačítko **škálování**.</span><span class="sxs-lookup"><span data-stu-id="3cf90-161">Click **Scale**.</span></span>
3. <span data-ttu-id="3cf90-162">Vyberte plán hello chcete toochange možnosti škálování.</span><span class="sxs-lookup"><span data-stu-id="3cf90-162">Select hello schedule you want toochange scaling options for.</span></span> <span data-ttu-id="3cf90-163">*Ne naplánovaném čase* je výchozí hello, pokud máte žádné plány definované.</span><span class="sxs-lookup"><span data-stu-id="3cf90-163">*No scheduled times* is hello default if you have no schedules defined.</span></span>
4. <span data-ttu-id="3cf90-164">Najde hello **škálování podle metriky** a vyberte **procesoru**.</span><span class="sxs-lookup"><span data-stu-id="3cf90-164">Find hello **Scale by metric** section and select **CPU**.</span></span>
5. <span data-ttu-id="3cf90-165">Teď můžete nakonfigurovat minimální a maximální rozsah instancí role, využití procesoru cílového hello (tootrigger a rozšiřování škálování využívajících) a kolik instancí tooscale nahoru a dolů podle.</span><span class="sxs-lookup"><span data-stu-id="3cf90-165">Now you can configure a minimum and maximum range of roles instances, hello target CPU usage (tootrigger a scale up), and how many instances tooscale up and down by.</span></span>

![Škálování role cloudové služby podle zatížení procesoru][cpu_scale]

> [!TIP]
> <span data-ttu-id="3cf90-167">Vždy, když se zobrazí ![][tip_icon] přesunout vaše tooit myši a může získat nápovědu ke konkrétní nastavení nemá.</span><span class="sxs-lookup"><span data-stu-id="3cf90-167">Whenever you see ![][tip_icon] move your mouse tooit and you can get help about what a specific setting does.</span></span>

## <a name="automatic-scale---queue"></a><span data-ttu-id="3cf90-168">Automatické škálování - fronty</span><span class="sxs-lookup"><span data-stu-id="3cf90-168">Automatic scale - Queue</span></span>
<span data-ttu-id="3cf90-169">Tento režim automaticky přizpůsobí Pokud hello počet zpráv ve frontě přejde nad nebo pod zadanou prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3cf90-169">This mode automatically scales if hello number of messages in a queue goes above or below a specified threshold.</span></span> <span data-ttu-id="3cf90-170">Instance role jsou vytvořené nebo odstraněné v takovém případě.</span><span class="sxs-lookup"><span data-stu-id="3cf90-170">Role instances are created or deleted when this happens.</span></span>

1. <span data-ttu-id="3cf90-171">V hello [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a potom klikněte na název hello hello řídicí panel hello tooopen cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="3cf90-171">In hello [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click hello name of hello cloud service tooopen hello dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="3cf90-172">Pokud nevidíte cloudové služby, pravděpodobně bude třeba toochange z **produkční** příliš**pracovní** nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="3cf90-172">If you don't see your cloud service, you may need toochange from **Production** too**Staging** or vice versa.</span></span>

2. <span data-ttu-id="3cf90-173">Klikněte na tlačítko **škálování**.</span><span class="sxs-lookup"><span data-stu-id="3cf90-173">Click **Scale**.</span></span>
3. <span data-ttu-id="3cf90-174">Najde hello **škálování podle metriky** a vyberte **fronty**.</span><span class="sxs-lookup"><span data-stu-id="3cf90-174">Find hello **Scale by metric** section and select **QUEUE**.</span></span>
4. <span data-ttu-id="3cf90-175">Teď můžete nakonfigurovat minimální a maximální rozsah instancí role, hello fronty a počet tooprocess zprávy fronty pro každou instanci a kolik instancí tooscale vertikálně a horizontálně podle.</span><span class="sxs-lookup"><span data-stu-id="3cf90-175">Now you can configure a minimum and maximum range of roles instances, hello queue and number of queue messages tooprocess for each instance, and how many instances tooscale up and down by.</span></span>

![Škálovat roli cloudové služby, protože fronta zpráv][queue_scale]

> [!TIP]
> <span data-ttu-id="3cf90-177">Vždy, když se zobrazí ![][tip_icon] přesunout vaše tooit myši a může získat nápovědu ke konkrétní nastavení nemá.</span><span class="sxs-lookup"><span data-stu-id="3cf90-177">Whenever you see ![][tip_icon] move your mouse tooit and you can get help about what a specific setting does.</span></span>

## <a name="scale-linked-resources"></a><span data-ttu-id="3cf90-178">Škálování odkazované zdroje.</span><span class="sxs-lookup"><span data-stu-id="3cf90-178">Scale linked resources</span></span>
<span data-ttu-id="3cf90-179">Často při změně měřítka roli, je výhodné tooscale hello databázi, která také používá aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3cf90-179">Often when you scale a role, it's beneficial tooscale hello database that hello application is using also.</span></span> <span data-ttu-id="3cf90-180">Pokud jste hello databáze toohello cloudovou službu, můžete přistupovat hello škálování nastavení pro daný prostředek kliknutím na příslušný odkaz hello.</span><span class="sxs-lookup"><span data-stu-id="3cf90-180">If you link hello database toohello cloud service, you can access hello scaling settings for that resource by clicking hello appropriate link.</span></span>

1. <span data-ttu-id="3cf90-181">V hello [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**a potom klikněte na název hello hello řídicí panel hello tooopen cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="3cf90-181">In hello [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click hello name of hello cloud service tooopen hello dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="3cf90-182">Pokud nevidíte cloudové služby, pravděpodobně bude třeba toochange z **produkční** příliš**pracovní** nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="3cf90-182">If you don't see your cloud service, you may need toochange from **Production** too**Staging** or vice versa.</span></span>

2. <span data-ttu-id="3cf90-183">Klikněte na tlačítko **škálování**.</span><span class="sxs-lookup"><span data-stu-id="3cf90-183">Click **Scale**.</span></span>
3. <span data-ttu-id="3cf90-184">Najde hello **propojené prostředky** části a klikněte na tlačítko **spravovat škálování pro tuto databázi**.</span><span class="sxs-lookup"><span data-stu-id="3cf90-184">Find hello **linked resources** section and click **Manage scale for this database**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="3cf90-185">Pokud se nezobrazí **propojené prostředky** části pravděpodobně nemáte žádné propojené prostředky.</span><span class="sxs-lookup"><span data-stu-id="3cf90-185">If you don't see a **linked resources** section, you probably do not have any linked resources.</span></span>

![][linked_resource]

[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
