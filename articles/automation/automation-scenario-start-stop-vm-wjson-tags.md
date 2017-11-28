---
title: "aaaUse stav virtuálního počítače Azure tooschedule formátu JSON značky | Microsoft Docs"
description: "Tento článek ukazuje, jak toouse JSON řetězce na značky tooautomate hello plánování virtuálních počítačů při spuštění a vypnutí."
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
ms.openlocfilehash: f6bbf1dea1c193e5d1010f12f3b1ed63562f9daf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario-using-json-formatted-tags-toocreate-a-schedule-for-azure-vm-startup-and-shutdown"></a><span data-ttu-id="cbf72-103">Azure scénář automatizace: použití formátu JSON značky toocreate plán pro virtuální počítač Azure spuštění a vypnutí</span><span class="sxs-lookup"><span data-stu-id="cbf72-103">Azure Automation scenario: Using JSON-formatted tags toocreate a schedule for Azure VM startup and shutdown</span></span>
<span data-ttu-id="cbf72-104">Zákazníci často mají tooschedule hello spuštění a vypnutí virtuálního počítače toohelp snížit náklady na předplatné nebo podporu obchodních a technických požadavků.</span><span class="sxs-lookup"><span data-stu-id="cbf72-104">Customers often want tooschedule hello startup and shutdown of virtual machines toohelp reduce subscription costs or support business and technical requirements.</span></span>

<span data-ttu-id="cbf72-105">Hello následující scénáře vám umožní tooset automatické spuštění a vypnutí virtuální počítače pomocí značku plán na úrovni skupiny prostředků nebo úrovni virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="cbf72-105">hello following scenario enables you tooset up automated startup and shutdown of your VMs by using a tag called Schedule at a resource group level or virtual machine level in Azure.</span></span> <span data-ttu-id="cbf72-106">Tento plán lze konfigurovat v neděli tooSaturday času spuštění a čas ukončení.</span><span class="sxs-lookup"><span data-stu-id="cbf72-106">This schedule can be configured from Sunday tooSaturday with a startup time and shutdown time.</span></span>

<span data-ttu-id="cbf72-107">Máme některé možnosti se na pole.</span><span class="sxs-lookup"><span data-stu-id="cbf72-107">We do have some out-of-the-box options.</span></span> <span data-ttu-id="cbf72-108">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="cbf72-108">These include:</span></span>

* <span data-ttu-id="cbf72-109">[Sady škálování virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) s nastavení automatického škálování, které umožňují tooscale příchozí nebo odchozí.</span><span class="sxs-lookup"><span data-stu-id="cbf72-109">[Virtual machine scale sets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) with autoscale settings that enable you tooscale in or out.</span></span>
* <span data-ttu-id="cbf72-110">[DevTest Labs](../devtest-lab/devtest-lab-overview.md) služby, která obsahuje integrovanou schopnost hello plánování operace spuštění a vypnutí.</span><span class="sxs-lookup"><span data-stu-id="cbf72-110">[DevTest Labs](../devtest-lab/devtest-lab-overview.md) service, which has hello built-in capability of scheduling startup and shutdown operations.</span></span>

<span data-ttu-id="cbf72-111">Ale tyto možnosti podporují pouze konkrétní scénáře a nemůže být virtuální počítače použité tooinfrastructure jako služba (IaaS).</span><span class="sxs-lookup"><span data-stu-id="cbf72-111">However, these options only support specific scenarios and cannot be applied tooinfrastructure-as-a-service (IaaS) VMs.</span></span>

<span data-ttu-id="cbf72-112">Při hello plán značky je použité tooa skupinu prostředků, je také použité tooall virtuální počítače v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="cbf72-112">When hello Schedule tag is applied tooa resource group, it's also applied tooall virtual machines inside that resource group.</span></span> <span data-ttu-id="cbf72-113">Plán se také přímo použité tooa virtuálních počítačů, poslední plán hello přednost v hello následující pořadí:</span><span class="sxs-lookup"><span data-stu-id="cbf72-113">If a schedule is also directly applied tooa VM, hello last schedule takes precedence in hello following order:</span></span>

1. <span data-ttu-id="cbf72-114">Skupina prostředků použité tooa plán</span><span class="sxs-lookup"><span data-stu-id="cbf72-114">Schedule applied tooa resource group</span></span>
2. <span data-ttu-id="cbf72-115">Naplánovat použité tooa skupinu prostředků a virtuální počítač ve skupině prostředků hello</span><span class="sxs-lookup"><span data-stu-id="cbf72-115">Schedule applied tooa resource group and virtual machine in hello resource group</span></span>
3. <span data-ttu-id="cbf72-116">Použít plán tooa virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cbf72-116">Schedule applied tooa virtual machine</span></span>

<span data-ttu-id="cbf72-117">Tento scénář v podstatě přebírá řetězec formátu JSON s zadaný formát a přidá ji jako hello hodnotu pro značku plán.</span><span class="sxs-lookup"><span data-stu-id="cbf72-117">This scenario essentially takes a JSON string with a specified format and adds it as hello value for a tag called Schedule.</span></span> <span data-ttu-id="cbf72-118">Potom runbook jsou uvedeny všechny skupiny prostředků a virtuální počítače a identifikuje hello plány pro každý virtuální počítač, na základě výše uvedených scénářů hello.</span><span class="sxs-lookup"><span data-stu-id="cbf72-118">Then a runbook lists all resource groups and virtual machines and identifies hello schedules for each VM based on hello scenarios listed earlier.</span></span> <span data-ttu-id="cbf72-119">V dalším projde hello virtuálních počítačů, které mají plány připojen a vyhodnotí, jakou akci by měla být provedena.</span><span class="sxs-lookup"><span data-stu-id="cbf72-119">Next it loops through hello VMs that have schedules attached and evaluates what action should be taken.</span></span> <span data-ttu-id="cbf72-120">Například určuje který virtuálních počítačů potřebovat toobe zastavit, vypnout nebo ignorovat.</span><span class="sxs-lookup"><span data-stu-id="cbf72-120">For example, it determines which VMs need toobe stopped, shut down, or ignored.</span></span>

<span data-ttu-id="cbf72-121">Tyto sady runbook ověřit pomocí hello [účet spustit v Azure jako](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="cbf72-121">These runbooks authenticate by using hello [Azure Run As account](automation-sec-configure-azure-runas-account.md).</span></span>

## <a name="download-hello-runbooks-for-hello-scenario"></a><span data-ttu-id="cbf72-122">Stáhnout runbooky hello hello scénář</span><span class="sxs-lookup"><span data-stu-id="cbf72-122">Download hello runbooks for hello scenario</span></span>
<span data-ttu-id="cbf72-123">Tento scénář se skládá ze čtyř runbooky pracovních postupů Powershellu, které si můžete stáhnout z hello [Galerii TechNet](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) nebo hello [Githubu](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) úložiště pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="cbf72-123">This scenario consists of four PowerShell Workflow runbooks that you can download from hello [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) or hello [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) repository for this project.</span></span>

| <span data-ttu-id="cbf72-124">Runbook</span><span class="sxs-lookup"><span data-stu-id="cbf72-124">Runbook</span></span> | <span data-ttu-id="cbf72-125">Popis</span><span class="sxs-lookup"><span data-stu-id="cbf72-125">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cbf72-126">Test ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="cbf72-126">Test-ResourceSchedule</span></span> |<span data-ttu-id="cbf72-127">Zkontroluje každý plán virtuální počítač a provede vypínání nebo spouštění podle plánu hello.</span><span class="sxs-lookup"><span data-stu-id="cbf72-127">Checks each virtual machine schedule and performs shutdown or startup depending on hello schedule.</span></span> |
| <span data-ttu-id="cbf72-128">Přidat ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="cbf72-128">Add-ResourceSchedule</span></span> |<span data-ttu-id="cbf72-129">Přidá hello plán značky tooa virtuálního počítače nebo skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="cbf72-129">Adds hello Schedule tag tooa VM or resource group.</span></span> |
| <span data-ttu-id="cbf72-130">Aktualizace ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="cbf72-130">Update-ResourceSchedule</span></span> |<span data-ttu-id="cbf72-131">Upravuje existující plán značky hello tak, že nahradíte novým připojením.</span><span class="sxs-lookup"><span data-stu-id="cbf72-131">Modifies hello existing Schedule tag by replacing it with a new one.</span></span> |
| <span data-ttu-id="cbf72-132">Odebrat ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="cbf72-132">Remove-ResourceSchedule</span></span> |<span data-ttu-id="cbf72-133">Odebere z virtuálního počítače nebo prostředku skupiny hello plán značky.</span><span class="sxs-lookup"><span data-stu-id="cbf72-133">Removes hello Schedule tag from a VM or resource group.</span></span> |

## <a name="install-and-configure-this-scenario"></a><span data-ttu-id="cbf72-134">Instalace a konfigurace tohoto scénáře</span><span class="sxs-lookup"><span data-stu-id="cbf72-134">Install and configure this scenario</span></span>
### <a name="install-and-publish-hello-runbooks"></a><span data-ttu-id="cbf72-135">Instalace a publikování sad runbook hello</span><span class="sxs-lookup"><span data-stu-id="cbf72-135">Install and publish hello runbooks</span></span>
<span data-ttu-id="cbf72-136">Po stažení hello sady runbook, můžete je importovat pomocí postupu hello v [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).</span><span class="sxs-lookup"><span data-stu-id="cbf72-136">After downloading hello runbooks, you can import them by using hello procedure in [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).</span></span>  <span data-ttu-id="cbf72-137">Každá sada runbook publikujte po byl úspěšně importován do vašeho účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="cbf72-137">Publish each runbook after it has been successfully imported into your Automation account.</span></span>

### <a name="add-a-schedule-toohello-test-resourceschedule-runbook"></a><span data-ttu-id="cbf72-138">Přidat runbook toohello ResourceSchedule testovací plán</span><span class="sxs-lookup"><span data-stu-id="cbf72-138">Add a schedule toohello Test-ResourceSchedule runbook</span></span>
<span data-ttu-id="cbf72-139">Postupujte podle těchto kroků tooenable hello plán pro testovací ResourceSchedule runbook hello.</span><span class="sxs-lookup"><span data-stu-id="cbf72-139">Follow these steps tooenable hello schedule for hello Test-ResourceSchedule runbook.</span></span> <span data-ttu-id="cbf72-140">Je to hello runbooku, který ověřuje, které virtuální počítače by měla být spuštěna, vypnout, nebo vlevo, jako je.</span><span class="sxs-lookup"><span data-stu-id="cbf72-140">This is hello runbook that verifies which virtual machines should be started, shut down, or left as is.</span></span>

1. <span data-ttu-id="cbf72-141">Z hello portálu Azure, otevřete svůj účet Automation a pak klikněte na tlačítko hello **Runbooky** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="cbf72-141">From hello Azure portal, open your Automation account, and then click hello **Runbooks** tile.</span></span>
2. <span data-ttu-id="cbf72-142">Na hello **Test ResourceSchedule** okně klikněte na tlačítko hello **plány** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="cbf72-142">On hello **Test-ResourceSchedule** blade, click hello **Schedules** tile.</span></span>
3. <span data-ttu-id="cbf72-143">Na hello **plány** okně klikněte na tlačítko **přidat plán**.</span><span class="sxs-lookup"><span data-stu-id="cbf72-143">On hello **Schedules** blade, click **Add a schedule**.</span></span>
4. <span data-ttu-id="cbf72-144">Na hello **plány** vyberte **propojit plán tooyour runbook**.</span><span class="sxs-lookup"><span data-stu-id="cbf72-144">On hello **Schedules** blade, select **Link a schedule tooyour runbook**.</span></span> <span data-ttu-id="cbf72-145">Potom vyberte **vytvořte nový plán**.</span><span class="sxs-lookup"><span data-stu-id="cbf72-145">Then select **Create a new schedule**.</span></span>
5. <span data-ttu-id="cbf72-146">Na hello **nový plán** okno, zadejte název hello tento plán, například: *HourlyExecution*.</span><span class="sxs-lookup"><span data-stu-id="cbf72-146">On hello **New schedule** blade, type in hello name of this schedule, for example: *HourlyExecution*.</span></span>
6. <span data-ttu-id="cbf72-147">Pro plán hello **spustit**, nastavte hello počáteční čas tooan hodinu přírůstku.</span><span class="sxs-lookup"><span data-stu-id="cbf72-147">For hello schedule **Start**, set hello start time tooan hour increment.</span></span>
7. <span data-ttu-id="cbf72-148">Vyberte **opakování**a pak pro **opakovat každých interval**, vyberte **1 hodina**.</span><span class="sxs-lookup"><span data-stu-id="cbf72-148">Select **Recurrence**, and then for **Recur every interval**, select **1 hour**.</span></span>
8. <span data-ttu-id="cbf72-149">Ověřte, že **nastavit dobu platnosti** je nastaven příliš**ne**a potom klikněte na **vytvořit** toosave nový plán.</span><span class="sxs-lookup"><span data-stu-id="cbf72-149">Verify that **Set expiration** is set too**No**, and then click **Create** toosave your new schedule.</span></span>
9. <span data-ttu-id="cbf72-150">Na hello **plán Runbook** okno Možnosti, vyberte **nastavení parametrů a běhu**.</span><span class="sxs-lookup"><span data-stu-id="cbf72-150">On hello **Schedule Runbook** options blade, select **Parameters and run settings**.</span></span> <span data-ttu-id="cbf72-151">V hello Test ResourceSchedule **parametry** okno, zadejte název hello předplatného v hello **Název_předplatného** pole.</span><span class="sxs-lookup"><span data-stu-id="cbf72-151">In hello Test-ResourceSchedule **Parameters** blade, enter hello name of your subscription in hello **SubscriptionName** field.</span></span>  <span data-ttu-id="cbf72-152">Toto je pouze parametr hello, které je nutné pro sadu runbook hello.</span><span class="sxs-lookup"><span data-stu-id="cbf72-152">This is hello only parameter that's required for hello runbook.</span></span>  <span data-ttu-id="cbf72-153">Až budete hotovi, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="cbf72-153">When you're finished, click **OK**.</span></span>

<span data-ttu-id="cbf72-154">plán sad runbook Hello by měl vypadat jako následující hello, až se dokončí:</span><span class="sxs-lookup"><span data-stu-id="cbf72-154">hello runbook schedule should look like hello following when it's completed:</span></span>

![Nakonfigurované ResourceSchedule testu runbooku](./media/automation-scenario-start-stop-vm-wjson-tags/automation-schedule-config.png)<br>

## <a name="format-hello-json-string"></a><span data-ttu-id="cbf72-156">Formát hello řetězce formátu JSON</span><span class="sxs-lookup"><span data-stu-id="cbf72-156">Format hello JSON string</span></span>
<span data-ttu-id="cbf72-157">Toto řešení v podstatě trvá řetězce s zadaný formát JSON a přidá ji jako hello hodnotu pro značku volá plán.</span><span class="sxs-lookup"><span data-stu-id="cbf72-157">This solution basically takes a JSON string with a specified format and adds it as hello value for a tag called Schedule.</span></span> <span data-ttu-id="cbf72-158">Potom runbook jsou uvedeny všechny skupiny prostředků a virtuální počítače a identifikuje hello plány pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="cbf72-158">Then a runbook lists all resource groups and virtual machines and identifies hello schedules for each virtual machine.</span></span>

<span data-ttu-id="cbf72-159">Hello runbook cyklicky prochází přes hello virtuálních počítačů, které mají plány připojen a zkontroluje, jaké akce by měla být provedena.</span><span class="sxs-lookup"><span data-stu-id="cbf72-159">hello runbook loops over hello virtual machines that have schedules attached and checks what actions should be taken.</span></span> <span data-ttu-id="cbf72-160">Hello následuje příklad formátování hello řešení:</span><span class="sxs-lookup"><span data-stu-id="cbf72-160">hello following is an example of how hello solutions should be formatted:</span></span>

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

<span data-ttu-id="cbf72-161">Zde jsou některé podrobné informace o tuto strukturu:</span><span class="sxs-lookup"><span data-stu-id="cbf72-161">Here is some detailed information about this structure:</span></span>

1. <span data-ttu-id="cbf72-162">Formát Hello této struktury JSON je optimalizované toowork obejít omezení 256 znaků hello jedinou značku hodnoty v Azure.</span><span class="sxs-lookup"><span data-stu-id="cbf72-162">hello format of this JSON structure is optimized toowork around hello 256-character limitation of a single tag value in Azure.</span></span>
2. <span data-ttu-id="cbf72-163">*TzId* představuje hello časové pásmo hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cbf72-163">*TzId* represents hello time zone of hello virtual machine.</span></span> <span data-ttu-id="cbf72-164">Toto ID můžete získat pomocí třída TimeZoneInfo .NET hello v relaci prostředí PowerShell –**[System.TimeZoneInfo]:: GetSystemTimeZones()**.</span><span class="sxs-lookup"><span data-stu-id="cbf72-164">This ID can be obtained by using hello TimeZoneInfo .NET class in a PowerShell session--**[System.TimeZoneInfo]::GetSystemTimeZones()**.</span></span>

   ![GetSystemTimeZones v prostředí PowerShell](./media/automation-scenario-start-stop-vm-wjson-tags/automation-get-timzone-powershell.png)

   * <span data-ttu-id="cbf72-166">Dny v týdnu jsou reprezentované pomocí číselná hodnota nula toosix.</span><span class="sxs-lookup"><span data-stu-id="cbf72-166">Weekdays are represented with a numeric value of zero toosix.</span></span> <span data-ttu-id="cbf72-167">Hello hodnota nula rovná neděli.</span><span class="sxs-lookup"><span data-stu-id="cbf72-167">hello value zero equals Sunday.</span></span>
   * <span data-ttu-id="cbf72-168">Hello čas spuštění je reprezentována pomocí hello **S** atribut a jeho hodnota je ve 24hodinovém formátu.</span><span class="sxs-lookup"><span data-stu-id="cbf72-168">hello start time is represented with hello **S** attribute, and its value is in a 24-hour format.</span></span>
   * <span data-ttu-id="cbf72-169">Hello čas end nebo ukončení je reprezentována pomocí hello **E** atribut a jeho hodnota je ve 24hodinovém formátu.</span><span class="sxs-lookup"><span data-stu-id="cbf72-169">hello end or shutdown time is represented with hello **E** attribute, and its value is in a 24-hour format.</span></span>

     <span data-ttu-id="cbf72-170">Pokud hello **S** a **E** atributy každý mít hodnotu nula (0), hello virtuální počítač zůstane za současného stavu v době vyhodnocení hello.</span><span class="sxs-lookup"><span data-stu-id="cbf72-170">If hello **S** and **E** attributes each have a value of zero (0), hello virtual machine will be left in its present state at hello time of evaluation.</span></span>
3. <span data-ttu-id="cbf72-171">Pokud chcete zkušební tooskip pro určitý den v týdnu hello, není pro daný den v týdnu hello přidat oddíl.</span><span class="sxs-lookup"><span data-stu-id="cbf72-171">If you want tooskip evaluation for a specific day of hello week, don’t add a section for that day of hello week.</span></span> <span data-ttu-id="cbf72-172">Hello následující ukázka, pouze pondělí je vyhodnocena a hello jiných dny v týdnu hello se ignorují:</span><span class="sxs-lookup"><span data-stu-id="cbf72-172">In hello following example, only Monday is evaluated, and hello other days of hello week are ignored:</span></span>

    ```json
    {
        "TzId": "Eastern Standard Time",
        "1": {
            "S": "11",
            "E": "17"
        }
    }
    ```

## <a name="tag-resource-groups-or-vms"></a><span data-ttu-id="cbf72-173">Skupiny prostředků značka nebo virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="cbf72-173">Tag resource groups or VMs</span></span>
<span data-ttu-id="cbf72-174">tooshut dolů virtuálních počítačů, je nutné tootag hello virtuální počítače nebo skupiny prostředků hello, ve kterých se nachází.</span><span class="sxs-lookup"><span data-stu-id="cbf72-174">tooshut down VMs, you need tootag either hello VMs or hello resource groups in which they're located.</span></span> <span data-ttu-id="cbf72-175">Virtuální počítače, které nemají značku plán nevyhodnocují.</span><span class="sxs-lookup"><span data-stu-id="cbf72-175">Virtual machines that don't have a Schedule tag are not evaluated.</span></span> <span data-ttu-id="cbf72-176">Proto není spuštěna nebo vypnout.</span><span class="sxs-lookup"><span data-stu-id="cbf72-176">Therefore, they aren't started or shut down.</span></span>

<span data-ttu-id="cbf72-177">Existují dva způsoby skupiny prostředků tootag nebo virtuální počítače s tímto řešením.</span><span class="sxs-lookup"><span data-stu-id="cbf72-177">There are two ways tootag resource groups or VMs with this solution.</span></span> <span data-ttu-id="cbf72-178">Můžete provést přímo z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="cbf72-178">You can do it directly from hello portal.</span></span> <span data-ttu-id="cbf72-179">Nebo můžete použít hello přidat ResourceSchedule, aktualizace ResourceSchedule a odebrat ResourceSchedule sady runbook.</span><span class="sxs-lookup"><span data-stu-id="cbf72-179">Or you can use hello Add-ResourceSchedule, Update-ResourceSchedule, and Remove-ResourceSchedule runbooks.</span></span>

### <a name="tag-through-hello-portal"></a><span data-ttu-id="cbf72-180">Značka prostřednictvím portálu hello</span><span class="sxs-lookup"><span data-stu-id="cbf72-180">Tag through hello portal</span></span>
<span data-ttu-id="cbf72-181">Postupujte podle těchto kroků tootag virtuální počítač nebo skupinu prostředků hello portálu:</span><span class="sxs-lookup"><span data-stu-id="cbf72-181">Follow these steps tootag a virtual machine or resource group in hello portal:</span></span>

1. <span data-ttu-id="cbf72-182">Vyrovnání hello řetězce formátu JSON a ověřte, že nejsou žádné mezery.</span><span class="sxs-lookup"><span data-stu-id="cbf72-182">Flatten hello JSON string and verify that there aren't any spaces.</span></span>  <span data-ttu-id="cbf72-183">Řetězec vašeho JSON by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="cbf72-183">Your JSON string should look like this:</span></span>

    ```json
    {"TzId":"Eastern Standard Time","0":{"S":"11","E":"17"},"1":{"S":"9","E":"19"},"2": {"S":"9","E":"19"},"3":{"S":"9","E":"19"},"4":{"S":"9","E":"19"},"5":{"S":"9","E":"19"},"6":{"S":"11","E":"17"}}
    ```

2. <span data-ttu-id="cbf72-184">Vyberte hello **značky** ikonu pro virtuální počítač nebo prostředek skupiny tooapply tento plán.</span><span class="sxs-lookup"><span data-stu-id="cbf72-184">Select hello **Tag** icon for a VM or resource group tooapply this schedule.</span></span>

   ![Možnosti značky virtuálních počítačů](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-tag-option.png)

3. <span data-ttu-id="cbf72-186">Značky jsou definovány následující dvojici klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="cbf72-186">Tags are defined following a key/value pair.</span></span> <span data-ttu-id="cbf72-187">Typ **plán** v hello **klíč** pole a vložte hello řetězce formátu JSON do hello **hodnotu** pole.</span><span class="sxs-lookup"><span data-stu-id="cbf72-187">Type **Schedule** in hello **Key** field, and then paste hello JSON string into hello **Value** field.</span></span> <span data-ttu-id="cbf72-188">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cbf72-188">Click **Save**.</span></span> <span data-ttu-id="cbf72-189">Novou značku by se měla zobrazit v seznamu hello značky prostředku.</span><span class="sxs-lookup"><span data-stu-id="cbf72-189">Your new tag should now appear in hello list of tags for your resource.</span></span>

   ![Virtuální počítač plán značky](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-schedule-tag.png)

### <a name="tag-from-powershell"></a><span data-ttu-id="cbf72-191">Značka z prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="cbf72-191">Tag from PowerShell</span></span>
<span data-ttu-id="cbf72-192">Všechny importované sady runbook obsahují informace nápovědy od začátku hello hello skriptu, který popisuje, jak tooexecute hello sady runbook přímo z prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cbf72-192">All imported runbooks contain help information at hello beginning of hello script that describes how tooexecute hello runbooks directly from PowerShell.</span></span> <span data-ttu-id="cbf72-193">Hello přidat ScheduleResource a ScheduleResource aktualizace sad runbook můžete volat z prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cbf72-193">You can call hello Add-ScheduleResource and Update-ScheduleResource runbooks from PowerShell.</span></span> <span data-ttu-id="cbf72-194">To provedete pomocí předání požadované parametry, které umožňují toocreate nebo aktualizace hello plán značky pro skupinu virtuálních počítačů nebo prostředek mimo portál hello.</span><span class="sxs-lookup"><span data-stu-id="cbf72-194">You do this by passing required parameters that enable you toocreate or update hello Schedule tag on a VM or resource group outside of hello portal.</span></span>

<span data-ttu-id="cbf72-195">toocreate, přidání a odstranění značky pomocí prostředí PowerShell, musíte nejprve příliš[nastavení prostředí PowerShell pro Azure](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cbf72-195">toocreate, add, and delete tags through PowerShell, you first need too[set up your PowerShell environment for Azure](/powershell/azure/overview).</span></span> <span data-ttu-id="cbf72-196">Po dokončení nastavení hello, můžete pokračovat hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="cbf72-196">After you complete hello setup, you can proceed with hello following steps.</span></span>

### <a name="create-a-schedule-tag-with-powershell"></a><span data-ttu-id="cbf72-197">Vytvoření plánu značku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="cbf72-197">Create a schedule tag with PowerShell</span></span>
1. <span data-ttu-id="cbf72-198">Otevřete relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cbf72-198">Open a PowerShell session.</span></span> <span data-ttu-id="cbf72-199">Pak použijte následující příklad tooauthenticate se účet Spustit jako a toospecify předplatné hello:</span><span class="sxs-lookup"><span data-stu-id="cbf72-199">Then use hello following example tooauthenticate with your Run As account and toospecify a subscription:</span></span>

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. <span data-ttu-id="cbf72-200">Definujte plán zatřiďovací tabulku.</span><span class="sxs-lookup"><span data-stu-id="cbf72-200">Define a schedule hash table.</span></span> <span data-ttu-id="cbf72-201">Tady je příklad, jak by měla zkonstruovat:</span><span class="sxs-lookup"><span data-stu-id="cbf72-201">Here is an example of how it should be constructed:</span></span>

    ```powershell
    $schedule= @{ "TzId"="Eastern Standard Time"; "0"= @{"S"="11";"E"="17"};"1"= @{"S"="9";"E"="19"};"2"= @{"S"="9";"E"="19"};"3"= @{"S"="9";"E"="19"};"4"= @{"S"="9";"E"="19"};"5"= @{"S"="9";"E"="19"};"6"= @{"S"="11";"E"="17"}}
    ```

3. <span data-ttu-id="cbf72-202">Definujte hello parametry, které jsou vyžadované hello runbook.</span><span class="sxs-lookup"><span data-stu-id="cbf72-202">Define hello parameters that are required by hello runbook.</span></span> <span data-ttu-id="cbf72-203">V následujícím příkladu hello jsme cílení virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="cbf72-203">In hello following example, we are targeting a VM:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "VmName"="VM01";"Schedule"=$schedule}
    ```

    <span data-ttu-id="cbf72-204">Pokud jste označování skupinu prostředků, odeberte hello *VMName* parametr z hello $params hash tabulky následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cbf72-204">If you’re tagging a resource group, remove hello *VMName* parameter from hello $params hash table as follows:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "Schedule"=$schedule}
    ```

4. <span data-ttu-id="cbf72-205">Spusťte runbook hello přidat ResourceSchedule s hello následující parametry toocreate hello plán značky:</span><span class="sxs-lookup"><span data-stu-id="cbf72-205">Run hello Add-ResourceSchedule runbook with hello following parameters toocreate hello Schedule tag:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Add-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

5. <span data-ttu-id="cbf72-206">tooupdate značka virtuální počítač nebo skupinu prostředků provést hello **aktualizace ResourceSchedule** runbook s hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="cbf72-206">tooupdate a resource group or virtual machine tag, execute hello **Update-ResourceSchedule** runbook with hello following parameters:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Update-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

### <a name="remove-a-schedule-tag-with-powershell"></a><span data-ttu-id="cbf72-207">Odebrání značky plánu pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="cbf72-207">Remove a schedule tag with PowerShell</span></span>
1. <span data-ttu-id="cbf72-208">Otevřete relaci prostředí PowerShell a spusťte následující tooauthenticate se účet Spustit jako a tooselect hello a zadejte předplatné:</span><span class="sxs-lookup"><span data-stu-id="cbf72-208">Open a PowerShell session and run hello following tooauthenticate with your Run As account and tooselect and specify a subscription:</span></span>

    ```powershell
    Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. <span data-ttu-id="cbf72-209">Definujte hello parametry, které jsou vyžadované hello runbook.</span><span class="sxs-lookup"><span data-stu-id="cbf72-209">Define hello parameters that are required by hello runbook.</span></span> <span data-ttu-id="cbf72-210">V následujícím příkladu hello jsme cílení virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="cbf72-210">In hello following example, we are targeting a VM:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01";"VmName"="VM01"}
    ```

    <span data-ttu-id="cbf72-211">Pokud odstraňujete značku ze skupiny prostředků, odeberte hello *VMName* parametr z hello $params hash tabulky následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cbf72-211">If you’re removing a tag from a resource group, remove hello *VMName* parameter from hello $params hash table as follows:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"}
    ```

3. <span data-ttu-id="cbf72-212">Spusťte hello odebrat ResourceSchedule runbook tooremove hello plán značky:</span><span class="sxs-lookup"><span data-stu-id="cbf72-212">Execute hello Remove-ResourceSchedule runbook tooremove hello Schedule tag:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

4. <span data-ttu-id="cbf72-213">tooupdate značka virtuální počítač nebo skupinu prostředků vykonat hello odebrat ResourceSchedule runbook s hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="cbf72-213">tooupdate a resource group or virtual machine tag, execute hello Remove-ResourceSchedule runbook with hello following parameters:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

> [!NOTE]
> <span data-ttu-id="cbf72-214">Doporučujeme proaktivnímu monitorování tyto sady runbook (a stavech virtuálních počítačů hello) vypnout tooverify, který bude vaše virtuální počítače a spustit odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="cbf72-214">We recommend that you proactively monitor these runbooks (and hello virtual machine states) tooverify that your virtual machines are being shut down and started accordingly.</span></span>
>

<span data-ttu-id="cbf72-215">portál Azure, vyberte hello tooview hello podrobnosti sady runbook hello ResourceSchedule testovací úlohy hello **úlohy** dlaždice hello sady runbook.</span><span class="sxs-lookup"><span data-stu-id="cbf72-215">tooview hello details of hello Test-ResourceSchedule runbook job in hello Azure portal, select hello **Jobs** tile of hello runbook.</span></span> <span data-ttu-id="cbf72-216">Hello úlohy Souhrn zobrazí hello vstupní parametry a výstup hello stream, kromě toogeneral informace o úloze hello a všechny výjimky, pokud k nim došlo.</span><span class="sxs-lookup"><span data-stu-id="cbf72-216">hello job summary displays hello input parameters and hello output stream, in addition toogeneral information about hello job and any exceptions if they occurred.</span></span>

<span data-ttu-id="cbf72-217">Hello **Souhrn úlohy** zahrnuje zprávy z datových proudů hello výstup, upozornění a chyby.</span><span class="sxs-lookup"><span data-stu-id="cbf72-217">hello **Job Summary** includes messages from hello output, warning, and error streams.</span></span> <span data-ttu-id="cbf72-218">Vyberte hello **výstup** dlaždici tooview podrobné výsledky z hello spuštění sady runbook.</span><span class="sxs-lookup"><span data-stu-id="cbf72-218">Select hello **Output** tile tooview detailed results from hello runbook execution.</span></span>

![Výstup testu ResourceSchedule](./media/automation-scenario-start-stop-vm-wjson-tags/automation-job-output.png)

## <a name="next-steps"></a><span data-ttu-id="cbf72-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cbf72-220">Next steps</span></span>
* <span data-ttu-id="cbf72-221">tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md).</span><span class="sxs-lookup"><span data-stu-id="cbf72-221">tooget started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md).</span></span>
* <span data-ttu-id="cbf72-222">toolearn Další informace o typech runbooků a jejich výhody a omezení, najdete v části [typy runbooků ve službě Azure Automation](automation-runbook-types.md).</span><span class="sxs-lookup"><span data-stu-id="cbf72-222">toolearn more about runbook types, and their advantages and limitations, see [Azure Automation runbook types](automation-runbook-types.md).</span></span>
* <span data-ttu-id="cbf72-223">Další informace o funkcích podpory skript prostředí PowerShell najdete v tématu [nativní podpora Powershellových skriptů ve službě Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).</span><span class="sxs-lookup"><span data-stu-id="cbf72-223">For more information about PowerShell script support features, see [Native PowerShell script support in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).</span></span>
* <span data-ttu-id="cbf72-224">toolearn Další informace o protokolování sad runbook a výstup, najdete v části [Runbook výstupu a zpráv ve službě Azure Automation](automation-runbook-output-and-messages.md).</span><span class="sxs-lookup"><span data-stu-id="cbf72-224">toolearn more about runbook logging and output, see [Runbook output and messages in Azure Automation](automation-runbook-output-and-messages.md).</span></span>
* <span data-ttu-id="cbf72-225">Další informace o účtu spustit v Azure jako a jak tooauthenticate runbooků pomocí, najdete v části toolearn [ověření runbooků pomocí účtu spustit v Azure jako](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="cbf72-225">toolearn more about an Azure Run As account and how tooauthenticate your runbooks by using it, see [Authenticate runbooks with Azure Run As account](automation-sec-configure-azure-runas-account.md).</span></span>
