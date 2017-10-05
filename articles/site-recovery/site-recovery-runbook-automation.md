---
title: "Přidat Azure Automation runbook do plánů obnovení v Azure Site Recovery | Microsoft Docs"
description: "Zjistěte, jak Azure Site Recovery můžete rozšířit plánů obnovení pomocí Azure Automation. Zjistěte, jak dokončit složité úlohy během obnovení do Azure."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: ecece14d-5f92-4596-bbaf-5204addb95c2
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/23/2017
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: 064a6782970b950543f93c24800998c1f104c8df
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="add-azure-automation-runbooks-to-recovery-plans"></a><span data-ttu-id="e130e-104">Přidání sad Azure Automation runbook do plánů obnovení</span><span class="sxs-lookup"><span data-stu-id="e130e-104">Add Azure Automation runbooks to recovery plans</span></span>
<span data-ttu-id="e130e-105">V tomto článku jsme popisují, jak Azure Site Recovery integruje se službou Azure Automation můžete rozšířit plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-105">In this article, we describe how Azure Site Recovery integrates with Azure Automation to help you extend your recovery plans.</span></span> <span data-ttu-id="e130e-106">Plány obnovení můžete orchestraci obnovení virtuálních počítačů, které jsou chráněné službou Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="e130e-106">Recovery plans can orchestrate recovery of VMs that are protected with Site Recovery.</span></span> <span data-ttu-id="e130e-107">Plány obnovení fungovat pro replikaci do sekundární cloudu i pro replikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="e130e-107">Recovery plans work both for replication to a secondary cloud, and for replication to Azure.</span></span> <span data-ttu-id="e130e-108">Plány obnovení také pomoci zajistit, aby obnovení **přesné**, **repeatable**, a **automatizované**.</span><span class="sxs-lookup"><span data-stu-id="e130e-108">Recovery plans also help make the recovery **consistently accurate**, **repeatable**, and **automated**.</span></span> <span data-ttu-id="e130e-109">Pokud jste převzetí služeb při selhání virtuálních počítačů do Azure, integraci s Azure Automation rozšiřuje plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-109">If you fail over your VMs to Azure, integration with Azure Automation extends your recovery plans.</span></span> <span data-ttu-id="e130e-110">Můžete ho spuštění sady runbook, které nabízí výkonné automatizace úloh.</span><span class="sxs-lookup"><span data-stu-id="e130e-110">You can use it to execute runbooks, which offer powerful automation tasks.</span></span>

<span data-ttu-id="e130e-111">Pokud začínáte Azure Automation, můžete [zaregistrovat](https://azure.microsoft.com/services/automation/) a [stažení ukázkové skripty](https://azure.microsoft.com/documentation/scripts/).</span><span class="sxs-lookup"><span data-stu-id="e130e-111">If you are new to Azure Automation, you can [sign up](https://azure.microsoft.com/services/automation/) and [download sample scripts](https://azure.microsoft.com/documentation/scripts/).</span></span> <span data-ttu-id="e130e-112">Další informace a další informace o orchestraci obnovení do Azure pomocí [plány obnovení](https://azure.microsoft.com/blog/?p=166264), najdete v části [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="e130e-112">For more information, and to learn how to orchestrate recovery to Azure by using [recovery plans](https://azure.microsoft.com/blog/?p=166264), see [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).</span></span>

<span data-ttu-id="e130e-113">V tomto článku jsme popisují, jak můžete integrovat Azure Automation runbook do plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-113">In this article, we describe how you can integrate Azure Automation runbooks into your recovery plans.</span></span> <span data-ttu-id="e130e-114">Příklady použít k automatizaci základní úlohy, které dříve vyžadovaly ruční zásah.</span><span class="sxs-lookup"><span data-stu-id="e130e-114">We use examples to automate basic tasks that previously required manual intervention.</span></span> <span data-ttu-id="e130e-115">Můžeme také popisují, jak převést obnovení několika kroky akce obnovení jedním kliknutím.</span><span class="sxs-lookup"><span data-stu-id="e130e-115">We also describe how to convert a multi-step recovery to a single-click recovery action.</span></span>

## <a name="customize-the-recovery-plan"></a><span data-ttu-id="e130e-116">Přizpůsobit plán obnovení</span><span class="sxs-lookup"><span data-stu-id="e130e-116">Customize the recovery plan</span></span>
1. <span data-ttu-id="e130e-117">Přejděte na **Site Recovery** okna prostředků plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-117">Go to the **Site Recovery** recovery plan resource blade.</span></span> <span data-ttu-id="e130e-118">V tomto příkladu plán obnovení má dva virtuální počítače přidat do něj pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-118">For this example, the recovery plan has two VMs added to it, for recovery.</span></span> <span data-ttu-id="e130e-119">Chcete-li začít přidávat sady runbook, klikněte na tlačítko **přizpůsobit** kartě.</span><span class="sxs-lookup"><span data-stu-id="e130e-119">To begin adding a runbook, click the **Customize** tab.</span></span>

    ![Klikněte na tlačítko Upravit](media/site-recovery-runbook-automation-new/essentials-rp.png)


2. <span data-ttu-id="e130e-121">Klikněte pravým tlačítkem na **1. skupina: spuštění**a potom vyberte **akce po přidání**.</span><span class="sxs-lookup"><span data-stu-id="e130e-121">Right-click **Group 1: Start**, and then select **Add post action**.</span></span>

    ![Klikněte pravým tlačítkem na skupinu 1: Spuštění a přidejte po akce](media/site-recovery-runbook-automation-new/customize-rp.png)

3. <span data-ttu-id="e130e-123">Klikněte na tlačítko **vyberte skript**.</span><span class="sxs-lookup"><span data-stu-id="e130e-123">Click **Choose a script**.</span></span>

4. <span data-ttu-id="e130e-124">Na **akce aktualizace** okně Název skriptu **Hello, World**.</span><span class="sxs-lookup"><span data-stu-id="e130e-124">On the **Update action** blade, name the script **Hello World**.</span></span>

    ![V okně Akce aktualizace](media/site-recovery-runbook-automation-new/update-rp.png)

5. <span data-ttu-id="e130e-126">Zadejte název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="e130e-126">Enter an Automation account name.</span></span>
    >[!NOTE]
    > <span data-ttu-id="e130e-127">Účet Automation může být v libovolné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="e130e-127">The Automation account can be in any Azure region.</span></span> <span data-ttu-id="e130e-128">Účet Automation musí být ve stejném předplatném jako trezor Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="e130e-128">The Automation account must be in the same subscription as the Azure Site Recovery vault.</span></span>

6. <span data-ttu-id="e130e-129">V účtu Automation vyberte sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="e130e-129">In your Automation account, select a runbook.</span></span> <span data-ttu-id="e130e-130">Tato sada runbook je skript, který běží během provádění plán obnovení po obnovení první skupinu.</span><span class="sxs-lookup"><span data-stu-id="e130e-130">This runbook is the script that runs during the execution of the recovery plan, after the recovery of the first group.</span></span>

7. <span data-ttu-id="e130e-131">Chcete-li uložit skript, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e130e-131">To save the script, click **OK**.</span></span> <span data-ttu-id="e130e-132">Skript se přidá do **1. skupina: po kroky**.</span><span class="sxs-lookup"><span data-stu-id="e130e-132">The script is added to **Group 1: Post-steps**.</span></span>

    ![1:Start skupiny následné akce](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="considerations-for-adding-a-script"></a><span data-ttu-id="e130e-134">Důležité informace týkající se přidání skriptu</span><span class="sxs-lookup"><span data-stu-id="e130e-134">Considerations for adding a script</span></span>

* <span data-ttu-id="e130e-135">Pro možnost **odstranit krok** nebo **skript aktualizovat**, klikněte pravým tlačítkem na skript.</span><span class="sxs-lookup"><span data-stu-id="e130e-135">For options to **delete a step** or **update the script**, right-click the script.</span></span>
* <span data-ttu-id="e130e-136">Skript můžete spustit v Azure při převzetí služeb při selhání z místního počítače do Azure.</span><span class="sxs-lookup"><span data-stu-id="e130e-136">A script can run on Azure during failover from an on-premises machine to Azure.</span></span> <span data-ttu-id="e130e-137">Ho také můžete spustit v Azure jako skript primární lokalitu před vypnutí, během navrácení služeb po obnovení z Azure do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="e130e-137">It also can run on Azure as a primary-site script before shutdown, during failback from Azure to an on-premises machine.</span></span>
* <span data-ttu-id="e130e-138">Při spuštění skriptu, vloží kontextu plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-138">When a script runs, it injects a recovery plan context.</span></span> <span data-ttu-id="e130e-139">Následující příklad ukazuje kontextové proměnné:</span><span class="sxs-lookup"><span data-stu-id="e130e-139">The following example shows a context variable:</span></span>

    ```
            {"RecoveryPlanName":"hrweb-recovery",

            "FailoverType":"Test",

            "FailoverDirection":"PrimaryToSecondary",

            "GroupId":"1",

            "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                    { "SubscriptionId":"7a1111111-c1d6-49c5-8c5d-111ce8dd183",

                    "ResourceGroupName":"ContosoRG",

                    "CloudServiceName":"pod02hrweb-Chicago-test",

                    "RoleName":"Fabrikam-Hrweb-frontend-test",

                    "RecoveryPointId":"TimeStamp"}

                    }

            }
    ```

    <span data-ttu-id="e130e-140">Následující tabulka uvádí název a popis pro každou proměnnou v kontextu.</span><span class="sxs-lookup"><span data-stu-id="e130e-140">The following table lists the name and description of each variable in the context.</span></span>

    | <span data-ttu-id="e130e-141">**Název proměnné**</span><span class="sxs-lookup"><span data-stu-id="e130e-141">**Variable name**</span></span> | <span data-ttu-id="e130e-142">**Popis**</span><span class="sxs-lookup"><span data-stu-id="e130e-142">**Description**</span></span> |
    | --- | --- |
    | <span data-ttu-id="e130e-143">RecoveryPlanName</span><span class="sxs-lookup"><span data-stu-id="e130e-143">RecoveryPlanName</span></span> |<span data-ttu-id="e130e-144">Název plánu spuštěn.</span><span class="sxs-lookup"><span data-stu-id="e130e-144">The name of the plan being run.</span></span> <span data-ttu-id="e130e-145">Tato proměnná umožňuje provádět různé akce na základě názvu plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-145">This variable helps you take different actions based on the recovery plan name.</span></span> <span data-ttu-id="e130e-146">Také můžete znovu použít skript.</span><span class="sxs-lookup"><span data-stu-id="e130e-146">You also can reuse the script.</span></span> |
    | <span data-ttu-id="e130e-147">FailoverType</span><span class="sxs-lookup"><span data-stu-id="e130e-147">FailoverType</span></span> |<span data-ttu-id="e130e-148">Určuje, jestli je převzetí služeb při selhání testu, plánované, nebo neplánované.</span><span class="sxs-lookup"><span data-stu-id="e130e-148">Specifies whether the failover is a test, planned, or unplanned.</span></span> |
    | <span data-ttu-id="e130e-149">FailoverDirection</span><span class="sxs-lookup"><span data-stu-id="e130e-149">FailoverDirection</span></span> |<span data-ttu-id="e130e-150">Určuje, zda probíhá obnovení do primární nebo sekundární lokality.</span><span class="sxs-lookup"><span data-stu-id="e130e-150">Specifies whether recovery is to a primary or secondary site.</span></span> |
    | <span data-ttu-id="e130e-151">GroupID</span><span class="sxs-lookup"><span data-stu-id="e130e-151">GroupID</span></span> |<span data-ttu-id="e130e-152">Identifikuje číslo skupiny v plánu obnovení, když běží plánu.</span><span class="sxs-lookup"><span data-stu-id="e130e-152">Identifies the group number in the recovery plan when the plan is running.</span></span> |
    | <span data-ttu-id="e130e-153">VmMap</span><span class="sxs-lookup"><span data-stu-id="e130e-153">VmMap</span></span> |<span data-ttu-id="e130e-154">Pole všechny virtuální počítače ve skupině.</span><span class="sxs-lookup"><span data-stu-id="e130e-154">An array of all VMs in the group.</span></span> |
    | <span data-ttu-id="e130e-155">Klíč VMMap</span><span class="sxs-lookup"><span data-stu-id="e130e-155">VMMap key</span></span> |<span data-ttu-id="e130e-156">Jedinečný klíč (GUID) pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e130e-156">A unique key (GUID) for each VM.</span></span> <span data-ttu-id="e130e-157">Je stejný jako Identifikátor Azure Virtual Machine Manager (VMM) virtuálního počítače, kde je to možné.</span><span class="sxs-lookup"><span data-stu-id="e130e-157">It's the same as the Azure Virtual Machine Manager (VMM) ID of the VM, where applicable.</span></span> |
    | <span data-ttu-id="e130e-158">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="e130e-158">SubscriptionId</span></span> |<span data-ttu-id="e130e-159">ID předplatného Azure, ve kterém byl vytvořen virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e130e-159">The Azure subscription ID in which the VM was created.</span></span> |
    | <span data-ttu-id="e130e-160">RoleName</span><span class="sxs-lookup"><span data-stu-id="e130e-160">RoleName</span></span> |<span data-ttu-id="e130e-161">Název virtuálního počítače Azure, který se obnovuje.</span><span class="sxs-lookup"><span data-stu-id="e130e-161">The name of the Azure VM that's being recovered.</span></span> |
    | <span data-ttu-id="e130e-162">CloudServiceName</span><span class="sxs-lookup"><span data-stu-id="e130e-162">CloudServiceName</span></span> |<span data-ttu-id="e130e-163">Název služby cloudu Azure, ve kterém byl vytvořen virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e130e-163">The Azure cloud service name under which the VM was created.</span></span> |
    | <span data-ttu-id="e130e-164">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="e130e-164">ResourceGroupName</span></span>|<span data-ttu-id="e130e-165">Název skupiny prostředků Azure, ve kterém byl vytvořen virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e130e-165">The Azure resource group name under which the VM was created.</span></span> |
    | <span data-ttu-id="e130e-166">RecoveryPointId</span><span class="sxs-lookup"><span data-stu-id="e130e-166">RecoveryPointId</span></span>|<span data-ttu-id="e130e-167">Časové razítko pro při obnovení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e130e-167">The timestamp for when the VM is recovered.</span></span> |

* <span data-ttu-id="e130e-168">Zkontrolujte, zda má účet služby Automation následující moduly:</span><span class="sxs-lookup"><span data-stu-id="e130e-168">Ensure that the Automation account has the following modules:</span></span>
    * <span data-ttu-id="e130e-169">AzureRM.profile</span><span class="sxs-lookup"><span data-stu-id="e130e-169">AzureRM.profile</span></span>
    * <span data-ttu-id="e130e-170">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="e130e-170">AzureRM.Resources</span></span>
    * <span data-ttu-id="e130e-171">AzureRM.Automation</span><span class="sxs-lookup"><span data-stu-id="e130e-171">AzureRM.Automation</span></span>
    * <span data-ttu-id="e130e-172">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="e130e-172">AzureRM.Network</span></span>
    * <span data-ttu-id="e130e-173">AzureRM.Compute</span><span class="sxs-lookup"><span data-stu-id="e130e-173">AzureRM.Compute</span></span>

<span data-ttu-id="e130e-174">Všechny moduly musí být kompatibilní verze.</span><span class="sxs-lookup"><span data-stu-id="e130e-174">All modules should be of compatible versions.</span></span> <span data-ttu-id="e130e-175">Snadný způsob, jak všechny moduly musí být kompatibilní se má používat nejnovější verze všech modulů.</span><span class="sxs-lookup"><span data-stu-id="e130e-175">An easy way to ensure that all modules are compatible is to use the latest versions of all the modules.</span></span>

### <a name="access-all-vms-of-the-vmmap-in-a-loop"></a><span data-ttu-id="e130e-176">Přístup k všechny virtuální počítače VMMap ve smyčce</span><span class="sxs-lookup"><span data-stu-id="e130e-176">Access all VMs of the VMMap in a loop</span></span>
<span data-ttu-id="e130e-177">Použijte následující kód na opakování mezi všechny virtuální počítače Microsoft VMMap:</span><span class="sxs-lookup"><span data-stu-id="e130e-177">Use the following code to loop across all VMs of the Microsoft VMMap:</span></span>

```
$VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
$vmMap = $RecoveryPlanContext.VmMap
 foreach($VMID in $VMinfo)
 {
     $VM = $vmMap.$VMID                
             if( !(($VM -eq $Null) -Or ($VM.ResourceGroupName -eq $Null) -Or ($VM.RoleName -eq $Null))) {
         #this check is to ensure that we skip when some data is not available else it will fail
 Write-output "Resource group name ", $VM.ResourceGroupName
 Write-output "Rolename " = $VM.RoleName
     }
 }

```

> [!NOTE]
> <span data-ttu-id="e130e-178">Skupina název a role název hodnoty prostředků jsou prázdné po před akcí pro skupinu spouštěcí skript.</span><span class="sxs-lookup"><span data-stu-id="e130e-178">The resource group name and role name values are empty when the script is a pre-action to a boot group.</span></span> <span data-ttu-id="e130e-179">Pouze v případě, že virtuální počítač této skupiny úspěšné v převzetí služeb při selhání, naplní se hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e130e-179">The values are populated only if the VM of that group succeeds in failover.</span></span> <span data-ttu-id="e130e-180">Skript je po akce spouštěcí skupiny.</span><span class="sxs-lookup"><span data-stu-id="e130e-180">The script is a post-action of the boot group.</span></span>

## <a name="use-the-same-automation-runbook-in-multiple-recovery-plans"></a><span data-ttu-id="e130e-181">Použijte stejné sady Automation runbook v více plánů obnovení</span><span class="sxs-lookup"><span data-stu-id="e130e-181">Use the same Automation runbook in multiple recovery plans</span></span>

<span data-ttu-id="e130e-182">Můžete pomocí jednoho skriptu v více plánů obnovení pomocí externích proměnných.</span><span class="sxs-lookup"><span data-stu-id="e130e-182">You can use a single script in multiple recovery plans by using external variables.</span></span> <span data-ttu-id="e130e-183">Můžete použít [proměnné automatizace Azure](../automation/automation-variables.md) k uložení parametry, které můžete předat provedení plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-183">You can use [Azure Automation variables](../automation/automation-variables.md) to store parameters that you can pass for a recovery plan execution.</span></span> <span data-ttu-id="e130e-184">Přidáním předpony název plánu obnovení proměnnou můžete vytvořit jednotlivé proměnných pro každý plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-184">By adding the recovery plan name as a prefix to the variable, you can create individual variables for each recovery plan.</span></span> <span data-ttu-id="e130e-185">Pak použijte proměnné jako parametry.</span><span class="sxs-lookup"><span data-stu-id="e130e-185">Then, use the variables as parameters.</span></span> <span data-ttu-id="e130e-186">Můžete změnit parametr beze změny skript, ale stále Změna způsobu práce skript.</span><span class="sxs-lookup"><span data-stu-id="e130e-186">You can change a parameter without changing the script, but still change the way the script works.</span></span>

### <a name="use-a-simple-string-variable-in-a-runbook-script"></a><span data-ttu-id="e130e-187">Použití jednoduché řetězec proměnné ve skriptu runbook</span><span class="sxs-lookup"><span data-stu-id="e130e-187">Use a simple string variable in a runbook script</span></span>

<span data-ttu-id="e130e-188">V tomto příkladu skript přijímá vstup z skupina zabezpečení sítě (NSG) a platí pro virtuální počítače v plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-188">In this example, a script takes the input of a Network Security Group (NSG) and applies it to the VMs of a recovery plan.</span></span>

<span data-ttu-id="e130e-189">Pro skript k detekci obnovení, který je spuštěn plán, použijte kontext plánu obnovení:</span><span class="sxs-lookup"><span data-stu-id="e130e-189">For the script to detect which recovery plan is running, use the recovery plan context:</span></span>

```
workflow AddPublicIPAndNSG {
    param (
          [parameter(Mandatory=$false)]
          [Object]$RecoveryPlanContext
    )

    $RPName = $RecoveryPlanContext.RecoveryPlanName
```

<span data-ttu-id="e130e-190">Použít existující skupina NSG, musíte znát název skupiny NSG a název skupiny prostředků NSG.</span><span class="sxs-lookup"><span data-stu-id="e130e-190">To apply an existing NSG, you must know the NSG name and the NSG resource group name.</span></span> <span data-ttu-id="e130e-191">Tyto proměnné můžete použijte jako vstupy pro skripty plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-191">Use these variables as inputs for recovery plan scripts.</span></span> <span data-ttu-id="e130e-192">K tomu, vytvořte dvě proměnné v prostředky účet Automation.</span><span class="sxs-lookup"><span data-stu-id="e130e-192">To do this, create two variables in the Automation account assets.</span></span> <span data-ttu-id="e130e-193">Přidejte název plánu obnovení, který vytváříte parametry jako předpony názvu proměnné.</span><span class="sxs-lookup"><span data-stu-id="e130e-193">Add the name of the recovery plan that you are creating the parameters for as a prefix to the variable name.</span></span>

1. <span data-ttu-id="e130e-194">Vytvořte proměnnou pro uložení názvu skupiny NSG.</span><span class="sxs-lookup"><span data-stu-id="e130e-194">Create a variable to store the NSG name.</span></span> <span data-ttu-id="e130e-195">Přidání předpony k názvu proměnné pomocí názvu plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-195">Add a prefix to the variable name by using the name of the recovery plan.</span></span>

    ![Vytvořte proměnnou Název skupiny NSG](media/site-recovery-runbook-automation-new/var1.png)

2. <span data-ttu-id="e130e-197">Vytvořte proměnnou pro uložení této skupině název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="e130e-197">Create a variable to store the NSG's resource group name.</span></span> <span data-ttu-id="e130e-198">Přidání předpony k názvu proměnné pomocí názvu plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-198">Add a prefix to the variable name by using the name of the recovery plan.</span></span>

    ![Vytvoření názvu skupiny prostředků NSG](media/site-recovery-runbook-automation-new/var2.png)


3.  <span data-ttu-id="e130e-200">Ve skriptu použijte následující kód odkaz získat hodnoty proměnné:</span><span class="sxs-lookup"><span data-stu-id="e130e-200">In the script, use the following reference code to get the variable values:</span></span>

    ```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
    ```

4.  <span data-ttu-id="e130e-201">Pomocí proměnné v runbooku použít NSG k síťové rozhraní virtuálního počítače při selhání:</span><span class="sxs-lookup"><span data-stu-id="e130e-201">Use the variables in the runbook to apply the NSG to the network interface of the failed-over VM:</span></span>

    ```
    InlineScript {
    if (($Using:NSGname -ne $Null) -And ($Using:NSGRGname -ne $Null)) {
            $NSG = Get-AzureRmNetworkSecurityGroup -Name $Using:NSGname -ResourceGroupName $Using:NSGRGname
            Write-output $NSG.Id
            #Apply the NSG to a network interface
            #$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
            #Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            #  -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $NSG
        }
    }
    ```

<span data-ttu-id="e130e-202">Pro každý plán obnovení vytvořte nezávislých proměnných, takže můžete opakovaně použít skript.</span><span class="sxs-lookup"><span data-stu-id="e130e-202">For each recovery plan, create independent variables so that you can reuse the script.</span></span> <span data-ttu-id="e130e-203">Přidání předpony pomocí název plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-203">Add a prefix by using the recovery plan name.</span></span> <span data-ttu-id="e130e-204">Pro dokončení, začátku do konce skript pro tento scénář, najdete v části [přidat veřejné IP adresy a NSG na virtuálních počítačích během testovacího převzetí služeb při selhání plánu obnovení Site Recovery](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).</span><span class="sxs-lookup"><span data-stu-id="e130e-204">For a complete, end-to-end script for this scenario, see [Add a public IP and NSG to VMs during test failover of a Site Recovery recovery plan](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).</span></span>


### <a name="use-a-complex-variable-to-store-more-information"></a><span data-ttu-id="e130e-205">Použití proměnné komplexní ukládat další informace</span><span class="sxs-lookup"><span data-stu-id="e130e-205">Use a complex variable to store more information</span></span>

<span data-ttu-id="e130e-206">Vezměte v úvahu scénář, ve kterém chcete jednoho skriptu zapnout veřejnou IP adresu pro konkrétní virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="e130e-206">Consider a scenario in which you want a single script to turn on a public IP on specific VMs.</span></span> <span data-ttu-id="e130e-207">V jiné scénáři můžete chtít použít odlišné skupiny Nsg na různé virtuální počítače (ne na všech virtuálních počítačích).</span><span class="sxs-lookup"><span data-stu-id="e130e-207">In another scenario, you might want to apply different NSGs on different VMs (not on all VMs).</span></span> <span data-ttu-id="e130e-208">Můžete použít skript, který je opakovaně použitelné pro každého plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-208">You can make a script that is reusable for any recovery plan.</span></span> <span data-ttu-id="e130e-209">Proměnný počet virtuálních počítačů může mít každý plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-209">Each recovery plan can have a variable number of VMs.</span></span> <span data-ttu-id="e130e-210">Například obnovení služby SharePoint má dva elementy front end.</span><span class="sxs-lookup"><span data-stu-id="e130e-210">For example, a SharePoint recovery has two front ends.</span></span> <span data-ttu-id="e130e-211">Základní-obchodní (LOB) aplikace má pouze jeden front-endu.</span><span class="sxs-lookup"><span data-stu-id="e130e-211">A basic line-of-business (LOB) application has only one front end.</span></span> <span data-ttu-id="e130e-212">Nelze vytvořit samostatné proměnných pro každý plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-212">You cannot create separate variables for each recovery plan.</span></span> 

<span data-ttu-id="e130e-213">V následujícím příkladu jsme nové způsobem a vytvořte [komplexní proměnná](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) v prostředky účet Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="e130e-213">In the following example, we use a new technique and create a [complex variable](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) in the Azure Automation account assets.</span></span> <span data-ttu-id="e130e-214">To uděláte tak, že zadáte více hodnot.</span><span class="sxs-lookup"><span data-stu-id="e130e-214">You do this by specifying multiple values.</span></span> <span data-ttu-id="e130e-215">Pokud chcete provést následující kroky je nutné použít prostředí Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="e130e-215">You must use Azure PowerShell to complete the following steps:</span></span>

1. <span data-ttu-id="e130e-216">V prostředí PowerShell Přihlaste se k předplatnému Azure:</span><span class="sxs-lookup"><span data-stu-id="e130e-216">In PowerShell, sign in to your Azure subscription:</span></span>

    ```
    login-azurermaccount
    $sub = Get-AzureRmSubscription -Name <SubscriptionName>
    $sub | Select-AzureRmSubscription
    ```

2. <span data-ttu-id="e130e-217">Uložit parametry, vytvořte proměnnou komplexní pomocí název plánu obnovení:</span><span class="sxs-lookup"><span data-stu-id="e130e-217">To store the parameters, create the complex variable by using the name of the recovery plan:</span></span>

    ```
    $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

3. <span data-ttu-id="e130e-218">V této proměnné komplexní **VMDetails** je ID virtuálního počítače pro chráněný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e130e-218">In this complex variable, **VMDetails** is the VM ID for the protected VM.</span></span> <span data-ttu-id="e130e-219">Chcete-li získat ID virtuálního počítače na portálu Azure, zobrazte vlastnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e130e-219">To get the VM ID, in the Azure portal, view the VM properties.</span></span> <span data-ttu-id="e130e-220">Následující snímek obrazovky ukazuje proměnné, která jsou uloženy podrobnosti o dva virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="e130e-220">The following screenshot shows a variable that stores the details of two VMs:</span></span>

    ![Pomocí ID virtuálního počítače jako identifikátor GUID](media/site-recovery-runbook-automation-new/vmguid.png)

4. <span data-ttu-id="e130e-222">Pomocí této proměnné v runbooku.</span><span class="sxs-lookup"><span data-stu-id="e130e-222">Use this variable in your runbook.</span></span> <span data-ttu-id="e130e-223">Pokud se najde uvedené identifikátor GUID virtuálního počítače v rámci plánu obnovení, použít NSG na virtuálním počítači:</span><span class="sxs-lookup"><span data-stu-id="e130e-223">If the indicated VM GUID is found in the recovery plan context, apply the NSG on the VM:</span></span>

    ```
    $VMDetailsObj = Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName
    ```

4. <span data-ttu-id="e130e-224">V sadě runbook projděte virtuálních počítačů kontextu plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e130e-224">In your runbook, loop through the VMs of the recovery plan context.</span></span> <span data-ttu-id="e130e-225">Zkontrolujte, zda existuje virtuální počítač v **$VMDetailsObj**.</span><span class="sxs-lookup"><span data-stu-id="e130e-225">Check whether the VM exists in **$VMDetailsObj**.</span></span> <span data-ttu-id="e130e-226">Pokud existuje, přístup k vlastnostem proměnnou použít NSG:</span><span class="sxs-lookup"><span data-stu-id="e130e-226">If it exists, access the properties of the variable to apply the NSG:</span></span>

    ```
        $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
        $vmMap = $RecoveryPlanContext.VmMap

        foreach($VMID in $VMinfo) {
            Write-output $VMDetailsObj.value.$VMID

            if ($VMDetailsObj.value.$VMID -ne $Null) { #If the VM exists in the context, this will not b Null
                $VM = $vmMap.$VMID
                # Access the properties of the variable
                $NSGname = $VMDetailsObj.value.$VMID.'NSGName'
                $NSGRGname = $VMDetailsObj.value.$VMID.'NSGResourceGroupName'

                # Add code to apply the NSG properties to the VM
            }
        }
    ```

<span data-ttu-id="e130e-227">Můžete použít stejný skriptu pro různé obnovení plány.</span><span class="sxs-lookup"><span data-stu-id="e130e-227">You can use the same script for different recovery plans.</span></span> <span data-ttu-id="e130e-228">Zadejte jiné parametry uložením hodnotu, která odpovídá do plánu obnovení v různých proměnných.</span><span class="sxs-lookup"><span data-stu-id="e130e-228">Enter different parameters by storing the value that corresponds to a recovery plan in different variables.</span></span>

## <a name="sample-scripts"></a><span data-ttu-id="e130e-229">Ukázkové skripty</span><span class="sxs-lookup"><span data-stu-id="e130e-229">Sample scripts</span></span>

<span data-ttu-id="e130e-230">Chcete-li nasadit ukázkové skripty do vašeho účtu Automation, klikněte na tlačítko **nasadit do Azure** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e130e-230">To deploy sample scripts to your Automation account, click the **Deploy to Azure** button.</span></span>

<span data-ttu-id="e130e-231">[![Nasazení do Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)</span><span class="sxs-lookup"><span data-stu-id="e130e-231">[![Deploy to Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)</span></span>

<span data-ttu-id="e130e-232">Další příklad najdete v následujícím videu.</span><span class="sxs-lookup"><span data-stu-id="e130e-232">For another example, see the following video.</span></span> <span data-ttu-id="e130e-233">Ukazuje, jak obnovit dvouvrstvé WordPress aplikace do Azure:</span><span class="sxs-lookup"><span data-stu-id="e130e-233">It demonstrates how to recover a two-tier WordPress application to Azure:</span></span>


> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="additional-resources"></a><span data-ttu-id="e130e-234">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e130e-234">Additional resources</span></span>
* [<span data-ttu-id="e130e-235">Azure Automation spustit jako účet služby</span><span class="sxs-lookup"><span data-stu-id="e130e-235">Azure Automation service Run As account</span></span>](../automation/automation-sec-configure-azure-runas-account.md)
* [<span data-ttu-id="e130e-236">Přehled Azure Automation</span><span class="sxs-lookup"><span data-stu-id="e130e-236">Azure Automation overview</span></span>](http://msdn.microsoft.com/library/azure/dn643629.aspx "přehled Azure Automation.")
* [<span data-ttu-id="e130e-237">Azure Automation ukázkové skripty</span><span class="sxs-lookup"><span data-stu-id="e130e-237">Azure Automation sample scripts</span></span>](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "ukázkové skripty Azure Automation.")
