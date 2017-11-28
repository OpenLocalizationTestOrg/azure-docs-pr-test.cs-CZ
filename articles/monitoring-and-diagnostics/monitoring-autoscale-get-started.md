---
title: "aaaGet začít s škálování v Azure | Microsoft Docs"
description: "Zjistěte, jak tooscale prostředku v Azure."
author: rajram
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: rajram
ms.openlocfilehash: 6b3c3f4529018dcaf9691c538fec63dfbb3cea06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-autoscale-in-azure"></a><span data-ttu-id="62de7-103">Začínáme s škálování v Azure</span><span class="sxs-lookup"><span data-stu-id="62de7-103">Get started with Autoscale in Azure</span></span>
<span data-ttu-id="62de7-104">Tento článek popisuje, jak tooset nastavení automatického škálování prostředku v portálu Microsoft Azure hello.</span><span class="sxs-lookup"><span data-stu-id="62de7-104">This article describes how tooset up your Autoscale settings for your resource in hello Microsoft Azure portal.</span></span>

<span data-ttu-id="62de7-105">Azure monitorování škálování se vztahují pouze sady škálování počítače toovirtual, cloudové služby, plány služby Azure App Service a služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="62de7-105">Azure Monitor Autoscale applies only toovirtual machine scale sets, cloud services, Azure App Service plans, and App Service environments.</span></span> 

## <a name="discover-hello-autoscale-settings-in-your-subscription"></a><span data-ttu-id="62de7-106">Zjištění nastavení automatického škálování hello v rámci vašeho předplatného</span><span class="sxs-lookup"><span data-stu-id="62de7-106">Discover hello Autoscale settings in your subscription</span></span>
<span data-ttu-id="62de7-107">Můžete zjistit všechny prostředky hello, na které se vztahuje v Azure monitorování škálování.</span><span class="sxs-lookup"><span data-stu-id="62de7-107">You can discover all hello resources for which Autoscale is applicable in Azure Monitor.</span></span> <span data-ttu-id="62de7-108">Pomocí následujících kroků pro podrobný návod hello:</span><span class="sxs-lookup"><span data-stu-id="62de7-108">Use hello following steps for a step-by-step walkthrough:</span></span>

1. <span data-ttu-id="62de7-109">Otevřete hello [portálu Azure.][1]</span><span class="sxs-lookup"><span data-stu-id="62de7-109">Open hello [Azure portal.][1]</span></span>
2. <span data-ttu-id="62de7-110">Klikněte v levém podokně hello hello ikonu monitorování Azure.</span><span class="sxs-lookup"><span data-stu-id="62de7-110">Click hello Azure Monitor icon in hello left pane.</span></span>
  <span data-ttu-id="62de7-111">![Otevřete Azure monitorování][2]</span><span class="sxs-lookup"><span data-stu-id="62de7-111">![Open Azure Monitor][2]</span></span>
3. <span data-ttu-id="62de7-112">Klikněte na tlačítko **škálování** tooview všechny hello prostředky pro škálování, které se vztahuje, včetně jejich aktuálního stavu škálování.</span><span class="sxs-lookup"><span data-stu-id="62de7-112">Click **Autoscale** tooview all hello resources for which Autoscale is applicable, along with their current Autoscale status.</span></span>
  <span data-ttu-id="62de7-113">![Zjistit škálování v Azure monitorování][3]</span><span class="sxs-lookup"><span data-stu-id="62de7-113">![Discover Autoscale in Azure Monitor][3]</span></span>

<span data-ttu-id="62de7-114">V horní tooscope hello dolů hello seznamu tooselect prostředky v určité skupiny zdrojů, konkrétní typy prostředků nebo konkrétní prostředek, můžete použít podokno filtru hello.</span><span class="sxs-lookup"><span data-stu-id="62de7-114">You can use hello filter pane at hello top tooscope down hello list tooselect resources in a specific resource group, specific resource types, or a specific resource.</span></span>

<span data-ttu-id="62de7-115">Pro každý prostředek zjistíte hello aktuální počet instancí a stav automatického škálování hello.</span><span class="sxs-lookup"><span data-stu-id="62de7-115">For each resource, you will find hello current instance count and hello Autoscale status.</span></span> <span data-ttu-id="62de7-116">Hello stav škálování může být:</span><span class="sxs-lookup"><span data-stu-id="62de7-116">hello Autoscale status can be:</span></span>

- <span data-ttu-id="62de7-117">**Není nakonfigurováno**: jste nepovolili automatické škálování ještě pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="62de7-117">**Not configured**: You have not enabled Autoscale yet for this resource.</span></span>
- <span data-ttu-id="62de7-118">**Povolit**: jste povolili automatické škálování pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="62de7-118">**Enabled**: You have enabled Autoscale for this resource.</span></span>
- <span data-ttu-id="62de7-119">**Zakázané**: Zakázání automatického škálování pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="62de7-119">**Disabled**: You have disabled Autoscale for this resource.</span></span>

## <a name="create-your-first-autoscale-setting"></a><span data-ttu-id="62de7-120">Vytvoření vaší první nastavení automatického škálování</span><span class="sxs-lookup"><span data-stu-id="62de7-120">Create your first Autoscale setting</span></span>

<span data-ttu-id="62de7-121">Pojďme nyní projít jednoduché podrobný návod toocreate vaše první nastavení automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="62de7-121">Let's now go through a simple step-by-step walkthrough toocreate your first Autoscale setting.</span></span>

1. <span data-ttu-id="62de7-122">Otevřete hello **škálování** okno v Azure monitorování a vyberte prostředek, který má tooscale.</span><span class="sxs-lookup"><span data-stu-id="62de7-122">Open hello **Autoscale** blade in Azure Monitor and select a resource that you want tooscale.</span></span> <span data-ttu-id="62de7-123">(hello následující kroky použít plán služby App Service přidružených k webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="62de7-123">(hello following steps use an App Service plan associated with a web app.</span></span> <span data-ttu-id="62de7-124">Můžete [vytvořte první webové aplikace ASP.NET v Azure během 5 minut.] [4])</span><span class="sxs-lookup"><span data-stu-id="62de7-124">You can [create your first ASP.NET web app in Azure in 5 minutes.][4])</span></span>
2. <span data-ttu-id="62de7-125">Všimněte si, že je aktuální počet instancí hello 1.</span><span class="sxs-lookup"><span data-stu-id="62de7-125">Note that hello current instance count is 1.</span></span> <span data-ttu-id="62de7-126">Klikněte na tlačítko **povolit škálování**.</span><span class="sxs-lookup"><span data-stu-id="62de7-126">Click **Enable autoscale**.</span></span>
  <span data-ttu-id="62de7-127">![Nastavení škálování pro novou webovou aplikaci][5]</span><span class="sxs-lookup"><span data-stu-id="62de7-127">![Scale setting for new web app][5]</span></span>
3. <span data-ttu-id="62de7-128">Zadejte název pro nastavení možností horizontálního rozšíření hello a pak klikněte na tlačítko **přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="62de7-128">Provide a name for hello scale setting, and then click **Add a rule**.</span></span> <span data-ttu-id="62de7-129">Všimněte si, možnosti hello škálování pravidlo, které se otevřou jako kontext podokně na pravé straně hello.</span><span class="sxs-lookup"><span data-stu-id="62de7-129">Notice hello scale rule options that open as a context pane on hello right side.</span></span> <span data-ttu-id="62de7-130">Ve výchozím nastavení nastaví tato tooscale možnost hello instanci počet o 1, pokud hello procento využití procesoru hello prostředku přesahuje 70 procent.</span><span class="sxs-lookup"><span data-stu-id="62de7-130">By default, this sets hello option tooscale your instance count by 1 if hello CPU percentage of hello resource exceeds 70 percent.</span></span> <span data-ttu-id="62de7-131">Nechte na jeho výchozí hodnoty a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="62de7-131">Leave it at its default values and click **Add**.</span></span>
  <span data-ttu-id="62de7-132">![Vytvoření nastavení škálování pro webovou aplikaci][6]</span><span class="sxs-lookup"><span data-stu-id="62de7-132">![Create scale setting for a web app][6]</span></span>
4. <span data-ttu-id="62de7-133">Nyní jste vytvořili vaší první pravidlo škálování.</span><span class="sxs-lookup"><span data-stu-id="62de7-133">You've now created your first scale rule.</span></span> <span data-ttu-id="62de7-134">Všimněte si, že hello UX doporučuje osvědčených postupů a informací, že "se doporučuje toohave alespoň jeden škálování v pravidle."</span><span class="sxs-lookup"><span data-stu-id="62de7-134">Note that hello UX recommends best practices and states that "It is recommended toohave at least one scale in rule."</span></span> <span data-ttu-id="62de7-135">toodo tak:</span><span class="sxs-lookup"><span data-stu-id="62de7-135">toodo so:</span></span>
  
    <span data-ttu-id="62de7-136">a.</span><span class="sxs-lookup"><span data-stu-id="62de7-136">a.</span></span> <span data-ttu-id="62de7-137">Klikněte na tlačítko **přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="62de7-137">Click **Add a rule**.</span></span> 

    <span data-ttu-id="62de7-138">b.</span><span class="sxs-lookup"><span data-stu-id="62de7-138">b.</span></span> <span data-ttu-id="62de7-139">Nastavit **operátor** příliš**menší než**.</span><span class="sxs-lookup"><span data-stu-id="62de7-139">Set **Operator** too**Less than**.</span></span>

    <span data-ttu-id="62de7-140">c.</span><span class="sxs-lookup"><span data-stu-id="62de7-140">c.</span></span> <span data-ttu-id="62de7-141">Nastavit **prahová hodnota** příliš**20**.</span><span class="sxs-lookup"><span data-stu-id="62de7-141">Set **Threshold** too**20**.</span></span>

    <span data-ttu-id="62de7-142">d.</span><span class="sxs-lookup"><span data-stu-id="62de7-142">d.</span></span> <span data-ttu-id="62de7-143">Nastavit **operace** příliš**snížit počet podle**.</span><span class="sxs-lookup"><span data-stu-id="62de7-143">Set **Operation** too**Decrease count by**.</span></span>

   <span data-ttu-id="62de7-144">Teď byste měli mít nastavení škálování, měřítka na více systémů nebo měřítka v podle využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="62de7-144">You should now have a scale setting that scales out/scales in based on CPU usage.</span></span>
   <span data-ttu-id="62de7-145">![Škálování podle využití procesoru][8]</span><span class="sxs-lookup"><span data-stu-id="62de7-145">![Scale based on CPU][8]</span></span>
5. <span data-ttu-id="62de7-146">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="62de7-146">Click **Save**.</span></span>

<span data-ttu-id="62de7-147">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="62de7-147">Congratulations!</span></span> <span data-ttu-id="62de7-148">Nyní jste úspěšně vytvořili vaší první tooautoscale nastavení škálování webové aplikace podle využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="62de7-148">You've now successfully created your first scale setting tooautoscale your web app based on CPU usage.</span></span>

> [!NOTE] 
> <span data-ttu-id="62de7-149">Hello stejné kroky jsou příslušné tooget začít s škálování virtuálních počítačů sady nebo cloudové služby role.</span><span class="sxs-lookup"><span data-stu-id="62de7-149">hello same steps are applicable tooget started with a virtual machine scale set or cloud service role.</span></span>

## <a name="other-considerations"></a><span data-ttu-id="62de7-150">Další důležité informace</span><span class="sxs-lookup"><span data-stu-id="62de7-150">Other considerations</span></span>
### <a name="scale-based-on-a-schedule"></a><span data-ttu-id="62de7-151">Škálování podle plánu</span><span class="sxs-lookup"><span data-stu-id="62de7-151">Scale based on a schedule</span></span>
<span data-ttu-id="62de7-152">Kromě toho tooscale podle využití procesoru, můžete nastavit vaše škálování odlišně pro konkrétní dny v týdnu hello.</span><span class="sxs-lookup"><span data-stu-id="62de7-152">In addition tooscale based on CPU, you can set your scale differently for specific days of hello week.</span></span>

1. <span data-ttu-id="62de7-153">Klikněte na tlačítko **přidat podmínku škálování**.</span><span class="sxs-lookup"><span data-stu-id="62de7-153">Click **Add a scale condition**.</span></span>
2. <span data-ttu-id="62de7-154">Nastavení škálování hello pravidla režimu a hello je hello stejné jako hello výchozí podmínku.</span><span class="sxs-lookup"><span data-stu-id="62de7-154">Setting hello scale mode and hello rules is hello same as hello default condition.</span></span>
3. <span data-ttu-id="62de7-155">Vyberte **opakujte konkrétní dny** hello plánu.</span><span class="sxs-lookup"><span data-stu-id="62de7-155">Select **Repeat specific days** for hello schedule.</span></span>
4. <span data-ttu-id="62de7-156">Vyberte hello dny a čas zahájení a ukončení hello kdy má být použita podmínka škálování hello.</span><span class="sxs-lookup"><span data-stu-id="62de7-156">Select hello days and hello start/end time for when hello scale condition should be applied.</span></span>

![Podmínka škálování podle plánu][9]
### <a name="scale-differently-on-specific-dates"></a><span data-ttu-id="62de7-158">Jinak škálování na konkrétní kalendářní data</span><span class="sxs-lookup"><span data-stu-id="62de7-158">Scale differently on specific dates</span></span>
<span data-ttu-id="62de7-159">Kromě toho tooscale podle využití procesoru, můžete nastavit vaše škálování odlišně pro konkrétní kalendářní data.</span><span class="sxs-lookup"><span data-stu-id="62de7-159">In addition tooscale based on CPU, you can set your scale differently for specific dates.</span></span>

1. <span data-ttu-id="62de7-160">Klikněte na tlačítko **přidat podmínku škálování**.</span><span class="sxs-lookup"><span data-stu-id="62de7-160">Click **Add a scale condition**.</span></span>
2. <span data-ttu-id="62de7-161">Nastavení škálování hello pravidla režimu a hello je hello stejné jako hello výchozí podmínku.</span><span class="sxs-lookup"><span data-stu-id="62de7-161">Setting hello scale mode and hello rules is hello same as hello default condition.</span></span>
3. <span data-ttu-id="62de7-162">Vyberte **zadejte počáteční a koncové datum** hello plánu.</span><span class="sxs-lookup"><span data-stu-id="62de7-162">Select **Specify start/end dates** for hello schedule.</span></span>
4. <span data-ttu-id="62de7-163">Vyberte hello počáteční a koncové datum a čas zahájení a ukončení hello kdy má být použita podmínka škálování hello.</span><span class="sxs-lookup"><span data-stu-id="62de7-163">Select hello start/end dates and hello start/end time for when hello scale condition should be applied.</span></span>

![Škálování podmínky na základě dat][10]

### <a name="view-hello-scale-history-of-your-resource"></a><span data-ttu-id="62de7-165">Zobrazit historii hello škálování prostředku</span><span class="sxs-lookup"><span data-stu-id="62de7-165">View hello scale history of your resource</span></span>
<span data-ttu-id="62de7-166">Vždy, když prostředek je škálovat nahoru nebo dolů, je v protokolu aktivit hello zaprotokoluje událost.</span><span class="sxs-lookup"><span data-stu-id="62de7-166">Whenever your resource is scaled up or down, an event is logged in hello activity log.</span></span> <span data-ttu-id="62de7-167">Můžete zobrazit historii hello škálování prostředku pro hello posledních 24 hodin přepnutím toohello **historie spouštění** kartě.</span><span class="sxs-lookup"><span data-stu-id="62de7-167">You can view hello scale history of your resource for hello past 24 hours by switching toohello **Run history** tab.</span></span>

![Historie spouštění][11]

<span data-ttu-id="62de7-169">Pokud chcete, aby tooview hello dokončení škálování historie (pro až too90 dnů), vyberte **kliknutím sem toosee podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="62de7-169">If you want tooview hello complete scale history (for up too90 days), select **Click here toosee more details**.</span></span> <span data-ttu-id="62de7-170">Protokol aktivit Hello otevře s škálování předem vybraná pro prostředek a kategorie.</span><span class="sxs-lookup"><span data-stu-id="62de7-170">hello activity log opens, with Autoscale pre-selected for your resource and category.</span></span>

### <a name="view-hello-scale-definition-of-your-resource"></a><span data-ttu-id="62de7-171">Zobrazení hello škálování definice prostředku</span><span class="sxs-lookup"><span data-stu-id="62de7-171">View hello scale definition of your resource</span></span>
<span data-ttu-id="62de7-172">Při automatickém škálování je prostředek Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="62de7-172">Autoscale is an Azure Resource Manager resource.</span></span> <span data-ttu-id="62de7-173">Můžete zobrazit hello škálování definice ve formátu JSON přepínání toohello **JSON** kartě.</span><span class="sxs-lookup"><span data-stu-id="62de7-173">You can view hello scale definition in JSON by switching toohello **JSON** tab.</span></span>

![Definice škálování][12]

<span data-ttu-id="62de7-175">Můžete provádět změny v kódu JSON přímo, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="62de7-175">You can make changes in JSON directly, if required.</span></span> <span data-ttu-id="62de7-176">Tyto změny se projeví po uložení je.</span><span class="sxs-lookup"><span data-stu-id="62de7-176">These changes will be reflected after you save them.</span></span>

### <a name="disable-autoscale-and-manually-scale-your-instances"></a><span data-ttu-id="62de7-177">Zakažte automatické škálování a ručně škálovat vaše instance</span><span class="sxs-lookup"><span data-stu-id="62de7-177">Disable Autoscale and manually scale your instances</span></span>
<span data-ttu-id="62de7-178">Je možné doby kdy chcete toodisable vaše aktuální nastavení škálování a ručně škálovat prostředek.</span><span class="sxs-lookup"><span data-stu-id="62de7-178">There might be times when you want toodisable your current scale setting and manually scale your resource.</span></span>

<span data-ttu-id="62de7-179">Klikněte na tlačítko hello **zakázat automatické škálování** tlačítka v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="62de7-179">Click hello **Disable autoscale** button at hello top.</span></span>
<span data-ttu-id="62de7-180">![Zakázat automatické škálování][13]</span><span class="sxs-lookup"><span data-stu-id="62de7-180">![Disable Autoscale][13]</span></span>

> [!NOTE] 
> <span data-ttu-id="62de7-181">Tato možnost zakáže konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="62de7-181">This option disables your configuration.</span></span> <span data-ttu-id="62de7-182">Však můžete získat zpět tooit po znovu povolte automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="62de7-182">However, you can get back tooit after you enable Autoscale again.</span></span> 

<span data-ttu-id="62de7-183">Teď můžete nastavit hello počet instancí, které chcete tooscale toomanually.</span><span class="sxs-lookup"><span data-stu-id="62de7-183">You can now set hello number of instances that you want tooscale toomanually.</span></span>

![Ruční škálování sady][14]

<span data-ttu-id="62de7-185">Kliknutím můžete kdykoli vrátit tooAutoscale **povolit škálování** a potom **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="62de7-185">You can always return tooAutoscale by clicking **Enable autoscale** and then **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="62de7-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62de7-186">Next steps</span></span>
- [<span data-ttu-id="62de7-187">Vytvoření aktivity protokolu výstrahy toomonitor všechny operace škálování modul vaše předplatné</span><span class="sxs-lookup"><span data-stu-id="62de7-187">Create an Activity Log Alert toomonitor all Autoscale engine operations on your subscription</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [<span data-ttu-id="62de7-188">Vytvoření aktivity protokolu výstrahy toomonitor všechny neúspěšné operace škálování nebo škálovatelnou škálování na vaše předplatné</span><span class="sxs-lookup"><span data-stu-id="62de7-188">Create an Activity Log Alert toomonitor all failed Autoscale scale-in/scale-out operations on your subscription</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

<!--Reference-->
[1]:https://portal.azure.com
[2]: ./media/monitoring-autoscale-get-started/azure-monitor-launch.png
[3]: ./media/monitoring-autoscale-get-started/discover-autoscale-azure-monitor.png
[4]: https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-get-started-dotnet
[5]: ./media/monitoring-autoscale-get-started/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-get-started/create-scale-setting-web-app.png
[7]: ./media/monitoring-autoscale-get-started/scale-in-recommendation.png
[8]: ./media/monitoring-autoscale-get-started/scale-based-on-cpu.png
[9]: ./media/monitoring-autoscale-get-started/scale-condition-schedule.png
[10]: ./media/monitoring-autoscale-get-started/scale-condition-dates.png
[11]: ./media/monitoring-autoscale-get-started/scale-history.png
[12]: ./media/monitoring-autoscale-get-started/scale-definition-json.png
[13]: ./media/monitoring-autoscale-get-started/disable-autoscale.png
[14]: ./media/monitoring-autoscale-get-started/set-manualscale.png

