---
title: "Začínáme s škálování v Azure | Microsoft Docs"
description: "Informace o škálování prostředku v Azure."
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
ms.openlocfilehash: 68cb624b3ef4a77e7cfc949979e0b1949c2e5535
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-autoscale-in-azure"></a><span data-ttu-id="d0cd0-103">Začínáme s škálování v Azure</span><span class="sxs-lookup"><span data-stu-id="d0cd0-103">Get started with Autoscale in Azure</span></span>
<span data-ttu-id="d0cd0-104">Tento článek popisuje, jak nastavit nastavení automatického škálování prostředku na portálu Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-104">This article describes how to set up your Autoscale settings for your resource in the Microsoft Azure portal.</span></span>

<span data-ttu-id="d0cd0-105">Azure monitorování škálování se vztahuje pouze na sady škálování virtuálního počítače, cloudové služby, plány služby Azure App Service a služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-105">Azure Monitor Autoscale applies only to virtual machine scale sets, cloud services, Azure App Service plans, and App Service environments.</span></span> 

## <a name="discover-the-autoscale-settings-in-your-subscription"></a><span data-ttu-id="d0cd0-106">Zjištění nastavení škálování v rámci vašeho předplatného</span><span class="sxs-lookup"><span data-stu-id="d0cd0-106">Discover the Autoscale settings in your subscription</span></span>
<span data-ttu-id="d0cd0-107">Můžete zjistit všechny prostředky, na které se vztahuje v Azure monitorování škálování.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-107">You can discover all the resources for which Autoscale is applicable in Azure Monitor.</span></span> <span data-ttu-id="d0cd0-108">Podrobný návod, pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="d0cd0-108">Use the following steps for a step-by-step walkthrough:</span></span>

1. <span data-ttu-id="d0cd0-109">Otevřete [portálu Azure.][1]</span><span class="sxs-lookup"><span data-stu-id="d0cd0-109">Open the [Azure portal.][1]</span></span>
2. <span data-ttu-id="d0cd0-110">Klikněte na ikonu monitorování Azure v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-110">Click the Azure Monitor icon in the left pane.</span></span>
  <span data-ttu-id="d0cd0-111">![Otevřete Azure monitorování][2]</span><span class="sxs-lookup"><span data-stu-id="d0cd0-111">![Open Azure Monitor][2]</span></span>
3. <span data-ttu-id="d0cd0-112">Klikněte na tlačítko **škálování** zobrazíte všechny prostředky, na které se vztahuje, včetně jejich aktuálního stavu škálování škálování.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-112">Click **Autoscale** to view all the resources for which Autoscale is applicable, along with their current Autoscale status.</span></span>
  <span data-ttu-id="d0cd0-113">![Zjistit škálování v Azure monitorování][3]</span><span class="sxs-lookup"><span data-stu-id="d0cd0-113">![Discover Autoscale in Azure Monitor][3]</span></span>

<span data-ttu-id="d0cd0-114">Můžete podokno filtru v horní části do oboru dolů v seznamu vyberte prostředky v určité skupiny zdrojů, konkrétní typy prostředků nebo konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-114">You can use the filter pane at the top to scope down the list to select resources in a specific resource group, specific resource types, or a specific resource.</span></span>

<span data-ttu-id="d0cd0-115">Pro každý prostředek zjistíte, aktuální počet instancí a stav automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-115">For each resource, you will find the current instance count and the Autoscale status.</span></span> <span data-ttu-id="d0cd0-116">Stav škálování může být:</span><span class="sxs-lookup"><span data-stu-id="d0cd0-116">The Autoscale status can be:</span></span>

- <span data-ttu-id="d0cd0-117">**Není nakonfigurováno**: jste nepovolili automatické škálování ještě pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-117">**Not configured**: You have not enabled Autoscale yet for this resource.</span></span>
- <span data-ttu-id="d0cd0-118">**Povolit**: jste povolili automatické škálování pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-118">**Enabled**: You have enabled Autoscale for this resource.</span></span>
- <span data-ttu-id="d0cd0-119">**Zakázané**: Zakázání automatického škálování pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-119">**Disabled**: You have disabled Autoscale for this resource.</span></span>

## <a name="create-your-first-autoscale-setting"></a><span data-ttu-id="d0cd0-120">Vytvoření vaší první nastavení automatického škálování</span><span class="sxs-lookup"><span data-stu-id="d0cd0-120">Create your first Autoscale setting</span></span>

<span data-ttu-id="d0cd0-121">Pojďme se teď přejděte prostřednictvím jednoduchého podrobný návod k vytvoření vaší první nastavení automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-121">Let's now go through a simple step-by-step walkthrough to create your first Autoscale setting.</span></span>

1. <span data-ttu-id="d0cd0-122">Otevřete **škálování** okno v Azure monitorování a vyberte prostředek, který chcete škálovat.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-122">Open the **Autoscale** blade in Azure Monitor and select a resource that you want to scale.</span></span> <span data-ttu-id="d0cd0-123">(Plán služby App Service přidružených k webové aplikaci použít následující kroky.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-123">(The following steps use an App Service plan associated with a web app.</span></span> <span data-ttu-id="d0cd0-124">Můžete [vytvořte první webové aplikace ASP.NET v Azure během 5 minut.] [4])</span><span class="sxs-lookup"><span data-stu-id="d0cd0-124">You can [create your first ASP.NET web app in Azure in 5 minutes.][4])</span></span>
2. <span data-ttu-id="d0cd0-125">Všimněte si, že je aktuální počet instancí 1.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-125">Note that the current instance count is 1.</span></span> <span data-ttu-id="d0cd0-126">Klikněte na tlačítko **povolit škálování**.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-126">Click **Enable autoscale**.</span></span>
  <span data-ttu-id="d0cd0-127">![Nastavení škálování pro novou webovou aplikaci][5]</span><span class="sxs-lookup"><span data-stu-id="d0cd0-127">![Scale setting for new web app][5]</span></span>
3. <span data-ttu-id="d0cd0-128">Zadejte název pro nastavení škálování a pak klikněte na tlačítko **přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-128">Provide a name for the scale setting, and then click **Add a rule**.</span></span> <span data-ttu-id="d0cd0-129">Všimněte si, pravidlo možnosti škálování, které se otevřou jako kontext podokně na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-129">Notice the scale rule options that open as a context pane on the right side.</span></span> <span data-ttu-id="d0cd0-130">Ve výchozím nastavení nastaví tato možnost škálování vašeho počet instancí 1, pokud procento využití procesoru prostředku přesahuje 70 procent.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-130">By default, this sets the option to scale your instance count by 1 if the CPU percentage of the resource exceeds 70 percent.</span></span> <span data-ttu-id="d0cd0-131">Nechte na jeho výchozí hodnoty a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-131">Leave it at its default values and click **Add**.</span></span>
  <span data-ttu-id="d0cd0-132">![Vytvoření nastavení škálování pro webovou aplikaci][6]</span><span class="sxs-lookup"><span data-stu-id="d0cd0-132">![Create scale setting for a web app][6]</span></span>
4. <span data-ttu-id="d0cd0-133">Nyní jste vytvořili vaší první pravidlo škálování.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-133">You've now created your first scale rule.</span></span> <span data-ttu-id="d0cd0-134">Upozorňujeme, že UX doporučuje osvědčené postupy a uvádí, že "se doporučuje mít alespoň jeden škálování v pravidle."</span><span class="sxs-lookup"><span data-stu-id="d0cd0-134">Note that the UX recommends best practices and states that "It is recommended to have at least one scale in rule."</span></span> <span data-ttu-id="d0cd0-135">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="d0cd0-135">To do so:</span></span>
  
    <span data-ttu-id="d0cd0-136">a.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-136">a.</span></span> <span data-ttu-id="d0cd0-137">Klikněte na tlačítko **přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-137">Click **Add a rule**.</span></span> 

    <span data-ttu-id="d0cd0-138">b.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-138">b.</span></span> <span data-ttu-id="d0cd0-139">Nastavit **operátor** k **menší než**.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-139">Set **Operator** to **Less than**.</span></span>

    <span data-ttu-id="d0cd0-140">c.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-140">c.</span></span> <span data-ttu-id="d0cd0-141">Nastavit **prahová hodnota** k **20**.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-141">Set **Threshold** to **20**.</span></span>

    <span data-ttu-id="d0cd0-142">d.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-142">d.</span></span> <span data-ttu-id="d0cd0-143">Nastavit **operace** k **snížit počet podle**.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-143">Set **Operation** to **Decrease count by**.</span></span>

   <span data-ttu-id="d0cd0-144">Teď byste měli mít nastavení škálování, měřítka na více systémů nebo měřítka v podle využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-144">You should now have a scale setting that scales out/scales in based on CPU usage.</span></span>
   <span data-ttu-id="d0cd0-145">![Škálování podle využití procesoru][8]</span><span class="sxs-lookup"><span data-stu-id="d0cd0-145">![Scale based on CPU][8]</span></span>
5. <span data-ttu-id="d0cd0-146">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-146">Click **Save**.</span></span>

<span data-ttu-id="d0cd0-147">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="d0cd0-147">Congratulations!</span></span> <span data-ttu-id="d0cd0-148">Nyní jste úspěšně vytvořili vaší první nastavení škálování chcete používat automatické škálování podle využití procesoru na základě vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-148">You've now successfully created your first scale setting to autoscale your web app based on CPU usage.</span></span>

> [!NOTE] 
> <span data-ttu-id="d0cd0-149">Stejný postup platí pro začít pracovat s škálovací sadu virtuálních počítačů nebo cloudu, služby rolí.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-149">The same steps are applicable to get started with a virtual machine scale set or cloud service role.</span></span>

## <a name="other-considerations"></a><span data-ttu-id="d0cd0-150">Další důležité informace</span><span class="sxs-lookup"><span data-stu-id="d0cd0-150">Other considerations</span></span>
### <a name="scale-based-on-a-schedule"></a><span data-ttu-id="d0cd0-151">Škálování podle plánu</span><span class="sxs-lookup"><span data-stu-id="d0cd0-151">Scale based on a schedule</span></span>
<span data-ttu-id="d0cd0-152">Kromě škálování podle využití procesoru můžete nastavit vaše škálování odlišně pro konkrétní dny v týdnu.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-152">In addition to scale based on CPU, you can set your scale differently for specific days of the week.</span></span>

1. <span data-ttu-id="d0cd0-153">Klikněte na tlačítko **přidat podmínku škálování**.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-153">Click **Add a scale condition**.</span></span>
2. <span data-ttu-id="d0cd0-154">Nastavení režimu škálování a pravidel je stejná jako výchozí podmínku.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-154">Setting the scale mode and the rules is the same as the default condition.</span></span>
3. <span data-ttu-id="d0cd0-155">Vyberte **opakujte konkrétní dny** pro plán.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-155">Select **Repeat specific days** for the schedule.</span></span>
4. <span data-ttu-id="d0cd0-156">Vyberte dny a čas zahájení a ukončení kdy má být použita podmínka škálování.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-156">Select the days and the start/end time for when the scale condition should be applied.</span></span>

![Podmínka škálování podle plánu][9]
### <a name="scale-differently-on-specific-dates"></a><span data-ttu-id="d0cd0-158">Jinak škálování na konkrétní kalendářní data</span><span class="sxs-lookup"><span data-stu-id="d0cd0-158">Scale differently on specific dates</span></span>
<span data-ttu-id="d0cd0-159">Kromě škálování podle využití procesoru můžete nastavit vaše škálování odlišně pro konkrétní kalendářní data.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-159">In addition to scale based on CPU, you can set your scale differently for specific dates.</span></span>

1. <span data-ttu-id="d0cd0-160">Klikněte na tlačítko **přidat podmínku škálování**.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-160">Click **Add a scale condition**.</span></span>
2. <span data-ttu-id="d0cd0-161">Nastavení režimu škálování a pravidel je stejná jako výchozí podmínku.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-161">Setting the scale mode and the rules is the same as the default condition.</span></span>
3. <span data-ttu-id="d0cd0-162">Vyberte **zadejte počáteční a koncové datum** pro plán.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-162">Select **Specify start/end dates** for the schedule.</span></span>
4. <span data-ttu-id="d0cd0-163">Vyberte počáteční a koncové datum a čas zahájení a ukončení kdy má být použita podmínka škálování.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-163">Select the start/end dates and the start/end time for when the scale condition should be applied.</span></span>

![Škálování podmínky na základě dat][10]

### <a name="view-the-scale-history-of-your-resource"></a><span data-ttu-id="d0cd0-165">Zobrazení historie škálování prostředku</span><span class="sxs-lookup"><span data-stu-id="d0cd0-165">View the scale history of your resource</span></span>
<span data-ttu-id="d0cd0-166">Vždy, když prostředek je škálovat nahoru nebo dolů, se zaprotokoluje událost v protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-166">Whenever your resource is scaled up or down, an event is logged in the activity log.</span></span> <span data-ttu-id="d0cd0-167">Můžete zobrazit historii škálování prostředku za posledních 24 hodin při přechodu **historie spouštění** kartě.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-167">You can view the scale history of your resource for the past 24 hours by switching to the **Run history** tab.</span></span>

![Historie spouštění][11]

<span data-ttu-id="d0cd0-169">Pokud chcete zobrazit historii dokončení škálování (po dobu 90 dnů), vyberte **kliknutím sem zobrazíte další podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-169">If you want to view the complete scale history (for up to 90 days), select **Click here to see more details**.</span></span> <span data-ttu-id="d0cd0-170">Protokol aktivit otevře s škálování předem vybraná pro prostředek a kategorie.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-170">The activity log opens, with Autoscale pre-selected for your resource and category.</span></span>

### <a name="view-the-scale-definition-of-your-resource"></a><span data-ttu-id="d0cd0-171">Zobrazit definici škálování prostředku</span><span class="sxs-lookup"><span data-stu-id="d0cd0-171">View the scale definition of your resource</span></span>
<span data-ttu-id="d0cd0-172">Při automatickém škálování je prostředek Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-172">Autoscale is an Azure Resource Manager resource.</span></span> <span data-ttu-id="d0cd0-173">Definici škálování si můžete prohlédnout v JSON při přechodu **JSON** kartě.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-173">You can view the scale definition in JSON by switching to the **JSON** tab.</span></span>

![Definice škálování][12]

<span data-ttu-id="d0cd0-175">Můžete provádět změny v kódu JSON přímo, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-175">You can make changes in JSON directly, if required.</span></span> <span data-ttu-id="d0cd0-176">Tyto změny se projeví po uložení je.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-176">These changes will be reflected after you save them.</span></span>

### <a name="disable-autoscale-and-manually-scale-your-instances"></a><span data-ttu-id="d0cd0-177">Zakažte automatické škálování a ručně škálovat vaše instance</span><span class="sxs-lookup"><span data-stu-id="d0cd0-177">Disable Autoscale and manually scale your instances</span></span>
<span data-ttu-id="d0cd0-178">Může být časy, kdy chcete zakázat aktuální nastavení škálování a ručně škálovat prostředek.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-178">There might be times when you want to disable your current scale setting and manually scale your resource.</span></span>

<span data-ttu-id="d0cd0-179">Klikněte **zakázat automatické škálování** tlačítka v horní části.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-179">Click the **Disable autoscale** button at the top.</span></span>
<span data-ttu-id="d0cd0-180">![Zakázat automatické škálování][13]</span><span class="sxs-lookup"><span data-stu-id="d0cd0-180">![Disable Autoscale][13]</span></span>

> [!NOTE] 
> <span data-ttu-id="d0cd0-181">Tato možnost zakáže konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-181">This option disables your configuration.</span></span> <span data-ttu-id="d0cd0-182">Však můžete vrátit zpět k němu po znovu povolte automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-182">However, you can get back to it after you enable Autoscale again.</span></span> 

<span data-ttu-id="d0cd0-183">Teď můžete nastavit počet instancí, které chcete škálovat ručně.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-183">You can now set the number of instances that you want to scale to manually.</span></span>

![Ruční škálování sady][14]

<span data-ttu-id="d0cd0-185">Můžete vždy se vrátit škálování kliknutím **povolit škálování** a potom **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d0cd0-185">You can always return to Autoscale by clicking **Enable autoscale** and then **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d0cd0-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0cd0-186">Next steps</span></span>
- [<span data-ttu-id="d0cd0-187">Vytvoření aktivity protokolu výstrahy monitorovat všechny operace škálování modul vaše předplatné</span><span class="sxs-lookup"><span data-stu-id="d0cd0-187">Create an Activity Log Alert to monitor all Autoscale engine operations on your subscription</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [<span data-ttu-id="d0cd0-188">Vytvoření aktivity protokolu výstrahy monitorovat všechny neúspěšné operace škálování nebo škálovatelnou škálování na vaše předplatné</span><span class="sxs-lookup"><span data-stu-id="d0cd0-188">Create an Activity Log Alert to monitor all failed Autoscale scale-in/scale-out operations on your subscription</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

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

