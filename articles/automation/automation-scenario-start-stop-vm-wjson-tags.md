---
title: "Naplánování stav virtuálního počítače Azure pomocí formátu JSON značky | Microsoft Docs"
description: "Tento článek ukazuje, jak použít řetězce formátu JSON na značky k automatizaci plánování virtuálních počítačů při spuštění a vypnutí."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 6afed5d2-e939-4749-8b2c-9312b4c16fb2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: magoedte;paulomarquesc
ms.openlocfilehash: f8d9563318c3afe299cebb7c889874392f114f84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-scenario-using-json-formatted-tags-to-create-a-schedule-for-azure-vm-startup-and-shutdown"></a><span data-ttu-id="d38cc-103">Azure scénář automatizace: použití formátu JSON značek k vytvoření plánu pro virtuální počítač Azure spuštění a vypnutí</span><span class="sxs-lookup"><span data-stu-id="d38cc-103">Azure Automation scenario: Using JSON-formatted tags to create a schedule for Azure VM startup and shutdown</span></span>
<span data-ttu-id="d38cc-104">Zákazníci chtějí často naplánovat spuštění a vypnutí virtuálních počítačů můžete snížit náklady na předplatné nebo podporu obchodních a technických požadavků.</span><span class="sxs-lookup"><span data-stu-id="d38cc-104">Customers often want to schedule the startup and shutdown of virtual machines to help reduce subscription costs or support business and technical requirements.</span></span>

<span data-ttu-id="d38cc-105">V následujícím scénáři můžete nastavit automatické spuštění a vypnutí virtuální počítače pomocí značku plán na úrovni skupiny prostředků nebo úrovni virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="d38cc-105">The following scenario enables you to set up automated startup and shutdown of your VMs by using a tag called Schedule at a resource group level or virtual machine level in Azure.</span></span> <span data-ttu-id="d38cc-106">Tento plán lze nakonfigurovat od neděle do soboty času spuštění a čas ukončení.</span><span class="sxs-lookup"><span data-stu-id="d38cc-106">This schedule can be configured from Sunday to Saturday with a startup time and shutdown time.</span></span>

<span data-ttu-id="d38cc-107">Máme některé možnosti se na pole.</span><span class="sxs-lookup"><span data-stu-id="d38cc-107">We do have some out-of-the-box options.</span></span> <span data-ttu-id="d38cc-108">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="d38cc-108">These include:</span></span>

* <span data-ttu-id="d38cc-109">[Sady škálování virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) s nastavení automatického škálování, které vám umožní škálování příchozí nebo odchozí.</span><span class="sxs-lookup"><span data-stu-id="d38cc-109">[Virtual machine scale sets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) with autoscale settings that enable you to scale in or out.</span></span>
* <span data-ttu-id="d38cc-110">[DevTest Labs](../devtest-lab/devtest-lab-overview.md) služby, která obsahuje integrovanou schopnost plánování operace spuštění a vypnutí.</span><span class="sxs-lookup"><span data-stu-id="d38cc-110">[DevTest Labs](../devtest-lab/devtest-lab-overview.md) service, which has the built-in capability of scheduling startup and shutdown operations.</span></span>

<span data-ttu-id="d38cc-111">Ale tyto možnosti podporují pouze konkrétní scénáře a nelze ji použít k virtuálním počítačům infrastruktury jako služba (IaaS).</span><span class="sxs-lookup"><span data-stu-id="d38cc-111">However, these options only support specific scenarios and cannot be applied to infrastructure-as-a-service (IaaS) VMs.</span></span>

<span data-ttu-id="d38cc-112">Plán značky se použijí pro skupinu prostředků, se taky použije pro všechny virtuální počítače v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="d38cc-112">When the Schedule tag is applied to a resource group, it's also applied to all virtual machines inside that resource group.</span></span> <span data-ttu-id="d38cc-113">Pokud plán platí také přímo k virtuálnímu počítači, poslední plán má přednost před v následujícím pořadí:</span><span class="sxs-lookup"><span data-stu-id="d38cc-113">If a schedule is also directly applied to a VM, the last schedule takes precedence in the following order:</span></span>

1. <span data-ttu-id="d38cc-114">Plán použijí pro skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="d38cc-114">Schedule applied to a resource group</span></span>
2. <span data-ttu-id="d38cc-115">Plán u virtuálního počítače ve skupině prostředků a skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="d38cc-115">Schedule applied to a resource group and virtual machine in the resource group</span></span>
3. <span data-ttu-id="d38cc-116">Plán pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="d38cc-116">Schedule applied to a virtual machine</span></span>

<span data-ttu-id="d38cc-117">Tento scénář v podstatě přebírá řetězec formátu JSON s zadaný formát a přidá ji jako hodnotu pro značku plán.</span><span class="sxs-lookup"><span data-stu-id="d38cc-117">This scenario essentially takes a JSON string with a specified format and adds it as the value for a tag called Schedule.</span></span> <span data-ttu-id="d38cc-118">Potom runbook jsou uvedeny všechny skupiny prostředků a virtuální počítače a identifikuje plány pro každý virtuální počítač, na základě scénářů uvedena i výše.</span><span class="sxs-lookup"><span data-stu-id="d38cc-118">Then a runbook lists all resource groups and virtual machines and identifies the schedules for each VM based on the scenarios listed earlier.</span></span> <span data-ttu-id="d38cc-119">V dalším prochází virtuální počítače, které mají plány připojen a vyhodnotí, jakou akci by měla být provedena.</span><span class="sxs-lookup"><span data-stu-id="d38cc-119">Next it loops through the VMs that have schedules attached and evaluates what action should be taken.</span></span> <span data-ttu-id="d38cc-120">Například se určuje, které virtuální počítače je potřeba zastavit, vypnout nebo ignorovat.</span><span class="sxs-lookup"><span data-stu-id="d38cc-120">For example, it determines which VMs need to be stopped, shut down, or ignored.</span></span>

<span data-ttu-id="d38cc-121">Tyto sady runbook ověřit pomocí [účet spustit v Azure jako](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="d38cc-121">These runbooks authenticate by using the [Azure Run As account](automation-sec-configure-azure-runas-account.md).</span></span>

## <a name="download-the-runbooks-for-the-scenario"></a><span data-ttu-id="d38cc-122">Stáhnout runbooky pro scénář</span><span class="sxs-lookup"><span data-stu-id="d38cc-122">Download the runbooks for the scenario</span></span>
<span data-ttu-id="d38cc-123">Tento scénář se skládá ze čtyř runbooky pracovních postupů Powershellu, které si můžete stáhnout z [Galerii TechNet](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) nebo [Githubu](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) úložiště pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="d38cc-123">This scenario consists of four PowerShell Workflow runbooks that you can download from the [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) or the [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) repository for this project.</span></span>

| <span data-ttu-id="d38cc-124">Runbook</span><span class="sxs-lookup"><span data-stu-id="d38cc-124">Runbook</span></span> | <span data-ttu-id="d38cc-125">Popis</span><span class="sxs-lookup"><span data-stu-id="d38cc-125">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d38cc-126">Test ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="d38cc-126">Test-ResourceSchedule</span></span> |<span data-ttu-id="d38cc-127">Zkontroluje každý plán virtuální počítač a provede vypínání nebo spouštění podle plánu.</span><span class="sxs-lookup"><span data-stu-id="d38cc-127">Checks each virtual machine schedule and performs shutdown or startup depending on the schedule.</span></span> |
| <span data-ttu-id="d38cc-128">Přidat ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="d38cc-128">Add-ResourceSchedule</span></span> |<span data-ttu-id="d38cc-129">Přidá značku plán pro skupinu virtuálních počítačů nebo prostředku.</span><span class="sxs-lookup"><span data-stu-id="d38cc-129">Adds the Schedule tag to a VM or resource group.</span></span> |
| <span data-ttu-id="d38cc-130">Aktualizace ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="d38cc-130">Update-ResourceSchedule</span></span> |<span data-ttu-id="d38cc-131">Upravuje existující plán značky tak, že nahradíte novým připojením.</span><span class="sxs-lookup"><span data-stu-id="d38cc-131">Modifies the existing Schedule tag by replacing it with a new one.</span></span> |
| <span data-ttu-id="d38cc-132">Odebrat ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="d38cc-132">Remove-ResourceSchedule</span></span> |<span data-ttu-id="d38cc-133">Značky plán odebere ze skupiny virtuálních počítačů nebo prostředku.</span><span class="sxs-lookup"><span data-stu-id="d38cc-133">Removes the Schedule tag from a VM or resource group.</span></span> |

## <a name="install-and-configure-this-scenario"></a><span data-ttu-id="d38cc-134">Instalace a konfigurace tohoto scénáře</span><span class="sxs-lookup"><span data-stu-id="d38cc-134">Install and configure this scenario</span></span>
### <a name="install-and-publish-the-runbooks"></a><span data-ttu-id="d38cc-135">Instalace a zveřejnění runbooků</span><span class="sxs-lookup"><span data-stu-id="d38cc-135">Install and publish the runbooks</span></span>
<span data-ttu-id="d38cc-136">Po stažení sady runbook, můžete je importovat pomocí postupu v [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).</span><span class="sxs-lookup"><span data-stu-id="d38cc-136">After downloading the runbooks, you can import them by using the procedure in [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).</span></span>  <span data-ttu-id="d38cc-137">Každá sada runbook publikujte po byl úspěšně importován do vašeho účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="d38cc-137">Publish each runbook after it has been successfully imported into your Automation account.</span></span>

### <a name="add-a-schedule-to-the-test-resourceschedule-runbook"></a><span data-ttu-id="d38cc-138">Přidání do runbooku ResourceSchedule testovací plán</span><span class="sxs-lookup"><span data-stu-id="d38cc-138">Add a schedule to the Test-ResourceSchedule runbook</span></span>
<span data-ttu-id="d38cc-139">Použijte následující postup povolení plán pro testovací ResourceSchedule runbook.</span><span class="sxs-lookup"><span data-stu-id="d38cc-139">Follow these steps to enable the schedule for the Test-ResourceSchedule runbook.</span></span> <span data-ttu-id="d38cc-140">Toto je sada runbook, která ověřuje, které virtuální počítače by měla být spuštěna, vypnutí nebo ponechán beze.</span><span class="sxs-lookup"><span data-stu-id="d38cc-140">This is the runbook that verifies which virtual machines should be started, shut down, or left as is.</span></span>

1. <span data-ttu-id="d38cc-141">Z portálu Azure otevřete účet Automation a klikněte **Runbooky** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="d38cc-141">From the Azure portal, open your Automation account, and then click the **Runbooks** tile.</span></span>
2. <span data-ttu-id="d38cc-142">Na **Test ResourceSchedule** okně klikněte na tlačítko **plány** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="d38cc-142">On the **Test-ResourceSchedule** blade, click the **Schedules** tile.</span></span>
3. <span data-ttu-id="d38cc-143">Na **plány** okně klikněte na tlačítko **přidat plán**.</span><span class="sxs-lookup"><span data-stu-id="d38cc-143">On the **Schedules** blade, click **Add a schedule**.</span></span>
4. <span data-ttu-id="d38cc-144">Na **plány** vyberte **propojit plán s runbookem**.</span><span class="sxs-lookup"><span data-stu-id="d38cc-144">On the **Schedules** blade, select **Link a schedule to your runbook**.</span></span> <span data-ttu-id="d38cc-145">Potom vyberte **vytvořte nový plán**.</span><span class="sxs-lookup"><span data-stu-id="d38cc-145">Then select **Create a new schedule**.</span></span>
5. <span data-ttu-id="d38cc-146">Na **nový plán** okno, zadejte název tohoto plánu, například: *HourlyExecution*.</span><span class="sxs-lookup"><span data-stu-id="d38cc-146">On the **New schedule** blade, type in the name of this schedule, for example: *HourlyExecution*.</span></span>
6. <span data-ttu-id="d38cc-147">Pro plán **spustit**, nastavte hodnotu doby spuštění na vyšší než hodinu.</span><span class="sxs-lookup"><span data-stu-id="d38cc-147">For the schedule **Start**, set the start time to an hour increment.</span></span>
7. <span data-ttu-id="d38cc-148">Vyberte **opakování**a pak pro **opakovat každých interval**, vyberte **1 hodina**.</span><span class="sxs-lookup"><span data-stu-id="d38cc-148">Select **Recurrence**, and then for **Recur every interval**, select **1 hour**.</span></span>
8. <span data-ttu-id="d38cc-149">Ověřte, že **nastavit dobu platnosti** je nastaven na **ne**a potom klikněte na **vytvořit** uložit nový plán.</span><span class="sxs-lookup"><span data-stu-id="d38cc-149">Verify that **Set expiration** is set to **No**, and then click **Create** to save your new schedule.</span></span>
9. <span data-ttu-id="d38cc-150">Na **plán Runbook** okno Možnosti, vyberte **nastavení parametrů a běhu**.</span><span class="sxs-lookup"><span data-stu-id="d38cc-150">On the **Schedule Runbook** options blade, select **Parameters and run settings**.</span></span> <span data-ttu-id="d38cc-151">V testu-ResourceSchedule **parametry** okno, zadejte název svého předplatného v **Název_předplatného** pole.</span><span class="sxs-lookup"><span data-stu-id="d38cc-151">In the Test-ResourceSchedule **Parameters** blade, enter the name of your subscription in the **SubscriptionName** field.</span></span>  <span data-ttu-id="d38cc-152">Toto je jediný parametr, který vyžaduje pro sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="d38cc-152">This is the only parameter that's required for the runbook.</span></span>  <span data-ttu-id="d38cc-153">Až budete hotovi, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d38cc-153">When you're finished, click **OK**.</span></span>

<span data-ttu-id="d38cc-154">Až se dokončí, plán sad runbook by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="d38cc-154">The runbook schedule should look like the following when it's completed:</span></span>

![Nakonfigurované ResourceSchedule testu runbooku](./media/automation-scenario-start-stop-vm-wjson-tags/automation-schedule-config.png)<br>

## <a name="format-the-json-string"></a><span data-ttu-id="d38cc-156">Formátování řetězce formátu JSON</span><span class="sxs-lookup"><span data-stu-id="d38cc-156">Format the JSON string</span></span>
<span data-ttu-id="d38cc-157">Toto řešení v podstatě trvá řetězce s zadaný formát JSON a přidá ji jako hodnotu pro značku volá plán.</span><span class="sxs-lookup"><span data-stu-id="d38cc-157">This solution basically takes a JSON string with a specified format and adds it as the value for a tag called Schedule.</span></span> <span data-ttu-id="d38cc-158">Potom runbook jsou uvedeny všechny skupiny prostředků a virtuální počítače a identifikuje plány pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d38cc-158">Then a runbook lists all resource groups and virtual machines and identifies the schedules for each virtual machine.</span></span>

<span data-ttu-id="d38cc-159">Sada runbook cyklicky prochází přes virtuální počítače, které mají plány připojen a zkontroluje, jaké akce by měla být provedena.</span><span class="sxs-lookup"><span data-stu-id="d38cc-159">The runbook loops over the virtual machines that have schedules attached and checks what actions should be taken.</span></span> <span data-ttu-id="d38cc-160">Následuje příklad formátování řešení:</span><span class="sxs-lookup"><span data-stu-id="d38cc-160">The following is an example of how the solutions should be formatted:</span></span>

```json
{
    "TzId": "Eastern Standard Time",
    "0": {
        "S": "11",
        "E": "17"
    },
    "1": {
        "S": "9",
        "E": "19"
    },
    "2": {
        "S": "9",
        "E": "19"
    },
}
```

<span data-ttu-id="d38cc-161">Zde jsou některé podrobné informace o tuto strukturu:</span><span class="sxs-lookup"><span data-stu-id="d38cc-161">Here is some detailed information about this structure:</span></span>

1. <span data-ttu-id="d38cc-162">Formát tato struktura JSON je optimalizovaná tak, aby obejít omezení na 256 znaků jedinou značku hodnoty v Azure.</span><span class="sxs-lookup"><span data-stu-id="d38cc-162">The format of this JSON structure is optimized to work around the 256-character limitation of a single tag value in Azure.</span></span>
2. <span data-ttu-id="d38cc-163">*TzId* představuje časové pásmo pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d38cc-163">*TzId* represents the time zone of the virtual machine.</span></span> <span data-ttu-id="d38cc-164">Toto ID můžete získat pomocí třídě TimeZoneInfo .NET v relaci prostředí PowerShell –**[System.TimeZoneInfo]:: GetSystemTimeZones()**.</span><span class="sxs-lookup"><span data-stu-id="d38cc-164">This ID can be obtained by using the TimeZoneInfo .NET class in a PowerShell session--**[System.TimeZoneInfo]::GetSystemTimeZones()**.</span></span>

   ![GetSystemTimeZones v prostředí PowerShell](./media/automation-scenario-start-stop-vm-wjson-tags/automation-get-timzone-powershell.png)

   * <span data-ttu-id="d38cc-166">Dny v týdnu jsou reprezentované pomocí číselnou hodnotu nula na šest.</span><span class="sxs-lookup"><span data-stu-id="d38cc-166">Weekdays are represented with a numeric value of zero to six.</span></span> <span data-ttu-id="d38cc-167">Neděle se rovná hodnotě nula.</span><span class="sxs-lookup"><span data-stu-id="d38cc-167">The value zero equals Sunday.</span></span>
   * <span data-ttu-id="d38cc-168">Čas spuštění je reprezentována pomocí **S** atribut a jeho hodnota je ve 24hodinovém formátu.</span><span class="sxs-lookup"><span data-stu-id="d38cc-168">The start time is represented with the **S** attribute, and its value is in a 24-hour format.</span></span>
   * <span data-ttu-id="d38cc-169">Čas end nebo ukončení je reprezentována pomocí **E** atribut a jeho hodnota je ve 24hodinovém formátu.</span><span class="sxs-lookup"><span data-stu-id="d38cc-169">The end or shutdown time is represented with the **E** attribute, and its value is in a 24-hour format.</span></span>

     <span data-ttu-id="d38cc-170">Pokud **S** a **E** atributy každý mít hodnotu nula (0), virtuální počítač zůstane za současného stavu v době vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="d38cc-170">If the **S** and **E** attributes each have a value of zero (0), the virtual machine will be left in its present state at the time of evaluation.</span></span>
3. <span data-ttu-id="d38cc-171">Pokud chcete nechat přeskočit vyhodnocení pro určitý den v týdnu, nepřidáte oddíl pro daný den v týdnu.</span><span class="sxs-lookup"><span data-stu-id="d38cc-171">If you want to skip evaluation for a specific day of the week, don’t add a section for that day of the week.</span></span> <span data-ttu-id="d38cc-172">V následujícím příkladu je vyhodnotit pouze pondělí a ignorují jiných dny v týdnu:</span><span class="sxs-lookup"><span data-stu-id="d38cc-172">In the following example, only Monday is evaluated, and the other days of the week are ignored:</span></span>

    ```json
    {
        "TzId": "Eastern Standard Time",
        "1": {
            "S": "11",
            "E": "17"
        }
    }
    ```

## <a name="tag-resource-groups-or-vms"></a><span data-ttu-id="d38cc-173">Skupiny prostředků značka nebo virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="d38cc-173">Tag resource groups or VMs</span></span>
<span data-ttu-id="d38cc-174">Vypnout virtuální počítače, musíte označit virtuální počítače nebo skupiny prostředků, ve kterých se nachází.</span><span class="sxs-lookup"><span data-stu-id="d38cc-174">To shut down VMs, you need to tag either the VMs or the resource groups in which they're located.</span></span> <span data-ttu-id="d38cc-175">Virtuální počítače, které nemají značku plán nevyhodnocují.</span><span class="sxs-lookup"><span data-stu-id="d38cc-175">Virtual machines that don't have a Schedule tag are not evaluated.</span></span> <span data-ttu-id="d38cc-176">Proto není spuštěna nebo vypnout.</span><span class="sxs-lookup"><span data-stu-id="d38cc-176">Therefore, they aren't started or shut down.</span></span>

<span data-ttu-id="d38cc-177">Existují dva způsoby skupiny prostředků značka nebo virtuální počítače s tímto řešením.</span><span class="sxs-lookup"><span data-stu-id="d38cc-177">There are two ways to tag resource groups or VMs with this solution.</span></span> <span data-ttu-id="d38cc-178">Můžete provést přímo z portálu.</span><span class="sxs-lookup"><span data-stu-id="d38cc-178">You can do it directly from the portal.</span></span> <span data-ttu-id="d38cc-179">Nebo můžete přidat ResourceSchedule, aktualizace ResourceSchedule a odebrat ResourceSchedule sady runbook.</span><span class="sxs-lookup"><span data-stu-id="d38cc-179">Or you can use the Add-ResourceSchedule, Update-ResourceSchedule, and Remove-ResourceSchedule runbooks.</span></span>

### <a name="tag-through-the-portal"></a><span data-ttu-id="d38cc-180">Značka prostřednictvím portálu</span><span class="sxs-lookup"><span data-stu-id="d38cc-180">Tag through the portal</span></span>
<span data-ttu-id="d38cc-181">Postupujte podle těchto kroků k označení virtuální počítač nebo skupinu prostředků na portálu:</span><span class="sxs-lookup"><span data-stu-id="d38cc-181">Follow these steps to tag a virtual machine or resource group in the portal:</span></span>

1. <span data-ttu-id="d38cc-182">Vyrovnání řetězce formátu JSON a ověřte, že nejsou žádné mezery.</span><span class="sxs-lookup"><span data-stu-id="d38cc-182">Flatten the JSON string and verify that there aren't any spaces.</span></span>  <span data-ttu-id="d38cc-183">Řetězec vašeho JSON by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="d38cc-183">Your JSON string should look like this:</span></span>

    ```json
    {"TzId":"Eastern Standard Time","0":{"S":"11","E":"17"},"1":{"S":"9","E":"19"},"2": {"S":"9","E":"19"},"3":{"S":"9","E":"19"},"4":{"S":"9","E":"19"},"5":{"S":"9","E":"19"},"6":{"S":"11","E":"17"}}
    ```

2. <span data-ttu-id="d38cc-184">Vyberte **značky** ikonu pro skupinu virtuálních počítačů nebo prostředek použít tento plán.</span><span class="sxs-lookup"><span data-stu-id="d38cc-184">Select the **Tag** icon for a VM or resource group to apply this schedule.</span></span>

   ![Možnosti značky virtuálních počítačů](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-tag-option.png)

3. <span data-ttu-id="d38cc-186">Značky jsou definovány následující dvojici klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="d38cc-186">Tags are defined following a key/value pair.</span></span> <span data-ttu-id="d38cc-187">Typ **plán** v **klíč** pole a vložte do řetězce formátu JSON **hodnotu** pole.</span><span class="sxs-lookup"><span data-stu-id="d38cc-187">Type **Schedule** in the **Key** field, and then paste the JSON string into the **Value** field.</span></span> <span data-ttu-id="d38cc-188">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d38cc-188">Click **Save**.</span></span> <span data-ttu-id="d38cc-189">Novou značku by se měla zobrazit v seznamu značky prostředku.</span><span class="sxs-lookup"><span data-stu-id="d38cc-189">Your new tag should now appear in the list of tags for your resource.</span></span>

   ![Virtuální počítač plán značky](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-schedule-tag.png)

### <a name="tag-from-powershell"></a><span data-ttu-id="d38cc-191">Značka z prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d38cc-191">Tag from PowerShell</span></span>
<span data-ttu-id="d38cc-192">Všechny importované sady runbook obsahují informace nápovědy na začátku skriptu, který popisuje, jak při spuštění sady runbook přímo z prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d38cc-192">All imported runbooks contain help information at the beginning of the script that describes how to execute the runbooks directly from PowerShell.</span></span> <span data-ttu-id="d38cc-193">Přidat ScheduleResource a ScheduleResource aktualizace sad runbook můžete volat z prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d38cc-193">You can call the Add-ScheduleResource and Update-ScheduleResource runbooks from PowerShell.</span></span> <span data-ttu-id="d38cc-194">Uděláte to pomocí předání požadované parametry, které vám umožní vytvořit nebo aktualizovat plán značky pro skupinu virtuálních počítačů nebo prostředek mimo portál.</span><span class="sxs-lookup"><span data-stu-id="d38cc-194">You do this by passing required parameters that enable you to create or update the Schedule tag on a VM or resource group outside of the portal.</span></span>

<span data-ttu-id="d38cc-195">Pokud chcete vytvořit, přidat a odstranit značky pomocí prostředí PowerShell, nejprve musíte [nastavení prostředí PowerShell pro Azure](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d38cc-195">To create, add, and delete tags through PowerShell, you first need to [set up your PowerShell environment for Azure](/powershell/azure/overview).</span></span> <span data-ttu-id="d38cc-196">Po dokončení instalace, můžete pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="d38cc-196">After you complete the setup, you can proceed with the following steps.</span></span>

### <a name="create-a-schedule-tag-with-powershell"></a><span data-ttu-id="d38cc-197">Vytvoření plánu značku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d38cc-197">Create a schedule tag with PowerShell</span></span>
1. <span data-ttu-id="d38cc-198">Otevřete relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d38cc-198">Open a PowerShell session.</span></span> <span data-ttu-id="d38cc-199">Pak použijte následující příklad k ověření pomocí účtu spustit jako a zadejte předplatné:</span><span class="sxs-lookup"><span data-stu-id="d38cc-199">Then use the following example to authenticate with your Run As account and to specify a subscription:</span></span>

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. <span data-ttu-id="d38cc-200">Definujte plán zatřiďovací tabulku.</span><span class="sxs-lookup"><span data-stu-id="d38cc-200">Define a schedule hash table.</span></span> <span data-ttu-id="d38cc-201">Tady je příklad, jak by měla zkonstruovat:</span><span class="sxs-lookup"><span data-stu-id="d38cc-201">Here is an example of how it should be constructed:</span></span>

    ```powershell
    $schedule= @{ "TzId"="Eastern Standard Time"; "0"= @{"S"="11";"E"="17"};"1"= @{"S"="9";"E"="19"};"2"= @{"S"="9";"E"="19"};"3"= @{"S"="9";"E"="19"};"4"= @{"S"="9";"E"="19"};"5"= @{"S"="9";"E"="19"};"6"= @{"S"="11";"E"="17"}}
    ```

3. <span data-ttu-id="d38cc-202">Zadejte parametry, které jsou vyžadovány sady runbook.</span><span class="sxs-lookup"><span data-stu-id="d38cc-202">Define the parameters that are required by the runbook.</span></span> <span data-ttu-id="d38cc-203">V následujícím příkladu jsme cílení virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="d38cc-203">In the following example, we are targeting a VM:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "VmName"="VM01";"Schedule"=$schedule}
    ```

    <span data-ttu-id="d38cc-204">Pokud jste označování skupinu prostředků, odeberte *VMName* parametr z hodnoty hash $params tabulky následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d38cc-204">If you’re tagging a resource group, remove the *VMName* parameter from the $params hash table as follows:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "Schedule"=$schedule}
    ```

4. <span data-ttu-id="d38cc-205">Spuštění sady runbook přidat ResourceSchedule s následujícími parametry pro vytvoření plán značky:</span><span class="sxs-lookup"><span data-stu-id="d38cc-205">Run the Add-ResourceSchedule runbook with the following parameters to create the Schedule tag:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Add-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

5. <span data-ttu-id="d38cc-206">Chcete-li aktualizovat skupinu prostředků nebo značka virtuálního počítače, spusťte **aktualizace ResourceSchedule** runbook s následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="d38cc-206">To update a resource group or virtual machine tag, execute the **Update-ResourceSchedule** runbook with the following parameters:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Update-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

### <a name="remove-a-schedule-tag-with-powershell"></a><span data-ttu-id="d38cc-207">Odebrání značky plánu pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d38cc-207">Remove a schedule tag with PowerShell</span></span>
1. <span data-ttu-id="d38cc-208">Otevřete relaci prostředí PowerShell a spusťte následující příkaz k ověření pomocí účtu spustit jako a a vyberte a zadejte předplatné:</span><span class="sxs-lookup"><span data-stu-id="d38cc-208">Open a PowerShell session and run the following to authenticate with your Run As account and to select and specify a subscription:</span></span>

    ```powershell
    Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. <span data-ttu-id="d38cc-209">Zadejte parametry, které jsou vyžadovány sady runbook.</span><span class="sxs-lookup"><span data-stu-id="d38cc-209">Define the parameters that are required by the runbook.</span></span> <span data-ttu-id="d38cc-210">V následujícím příkladu jsme cílení virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="d38cc-210">In the following example, we are targeting a VM:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01";"VmName"="VM01"}
    ```

    <span data-ttu-id="d38cc-211">Pokud odstraňujete značku ze skupiny prostředků, odeberte *VMName* parametr z hodnoty hash $params tabulky následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d38cc-211">If you’re removing a tag from a resource group, remove the *VMName* parameter from the $params hash table as follows:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"}
    ```

3. <span data-ttu-id="d38cc-212">Spuštění sady runbook odebrat ResourceSchedule odebrat plán značky:</span><span class="sxs-lookup"><span data-stu-id="d38cc-212">Execute the Remove-ResourceSchedule runbook to remove the Schedule tag:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

4. <span data-ttu-id="d38cc-213">Chcete-li aktualizovat skupinu prostředků nebo značky virtuální počítač, spusťte odebrat ResourceSchedule runbook s následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="d38cc-213">To update a resource group or virtual machine tag, execute the Remove-ResourceSchedule runbook with the following parameters:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

> [!NOTE]
> <span data-ttu-id="d38cc-214">Doporučujeme aktivně sledovat tyto sady runbook (a stavech virtuálních počítačů), chcete-li ověřit, že virtuální počítače jsou vypínán dolů a spuštění odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="d38cc-214">We recommend that you proactively monitor these runbooks (and the virtual machine states) to verify that your virtual machines are being shut down and started accordingly.</span></span>
>

<span data-ttu-id="d38cc-215">Chcete-li zobrazit podrobnosti o ResourceSchedule testovací úlohy runbooku na portálu Azure, vyberte **úlohy** dlaždice sady runbook.</span><span class="sxs-lookup"><span data-stu-id="d38cc-215">To view the details of the Test-ResourceSchedule runbook job in the Azure portal, select the **Jobs** tile of the runbook.</span></span> <span data-ttu-id="d38cc-216">V souhrnu úlohy se zobrazí vstupní parametry a výstupní datový proud a také obecné informace o příslušné úloze a případné výjimky, které nastaly.</span><span class="sxs-lookup"><span data-stu-id="d38cc-216">The job summary displays the input parameters and the output stream, in addition to general information about the job and any exceptions if they occurred.</span></span>

<span data-ttu-id="d38cc-217">**Souhrn úlohy** zahrnuje zprávy z datových proudů výstupu, upozornění a chyb.</span><span class="sxs-lookup"><span data-stu-id="d38cc-217">The **Job Summary** includes messages from the output, warning, and error streams.</span></span> <span data-ttu-id="d38cc-218">Pokud chcete zobrazit podrobné výsledky spuštění runbooku, vyberte dlaždici **Výstup**.</span><span class="sxs-lookup"><span data-stu-id="d38cc-218">Select the **Output** tile to view detailed results from the runbook execution.</span></span>

![Výstup testu ResourceSchedule](./media/automation-scenario-start-stop-vm-wjson-tags/automation-job-output.png)

## <a name="next-steps"></a><span data-ttu-id="d38cc-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d38cc-220">Next steps</span></span>
* <span data-ttu-id="d38cc-221">První kroky s runbooky pracovních postupů PowerShellu najdete v článku [Můj první runbook pracovního postupu PowerShellu](automation-first-runbook-textual.md).</span><span class="sxs-lookup"><span data-stu-id="d38cc-221">To get started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md).</span></span>
* <span data-ttu-id="d38cc-222">Další informace o typech runbooků a jejich výhody a omezení, najdete v části [typy runbooků ve službě Azure Automation](automation-runbook-types.md).</span><span class="sxs-lookup"><span data-stu-id="d38cc-222">To learn more about runbook types, and their advantages and limitations, see [Azure Automation runbook types](automation-runbook-types.md).</span></span>
* <span data-ttu-id="d38cc-223">Další informace o funkcích podpory skript prostředí PowerShell najdete v tématu [nativní podpora Powershellových skriptů ve službě Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).</span><span class="sxs-lookup"><span data-stu-id="d38cc-223">For more information about PowerShell script support features, see [Native PowerShell script support in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).</span></span>
* <span data-ttu-id="d38cc-224">Další informace o protokolování sad runbook a výstup, najdete v části [Runbook výstupu a zpráv ve službě Azure Automation](automation-runbook-output-and-messages.md).</span><span class="sxs-lookup"><span data-stu-id="d38cc-224">To learn more about runbook logging and output, see [Runbook output and messages in Azure Automation](automation-runbook-output-and-messages.md).</span></span>
* <span data-ttu-id="d38cc-225">Další informace o účtu spustit v Azure jako a ověřit svoje runbooky použití naleznete v tématu [ověření runbooků pomocí účtu spustit v Azure jako](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="d38cc-225">To learn more about an Azure Run As account and how to authenticate your runbooks by using it, see [Authenticate runbooks with Azure Run As account](automation-sec-configure-azure-runas-account.md).</span></span>
