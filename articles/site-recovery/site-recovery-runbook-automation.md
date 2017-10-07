---
title: "aaaAdd Azure Automation. runbooky toorecovery plány v Azure Site Recovery | Microsoft Docs"
description: "Zjistěte, jak Azure Site Recovery můžete rozšířit plánů obnovení pomocí Azure Automation. Zjistěte, jak toocomplete komplexní úlohy během obnovení tooAzure."
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
ms.openlocfilehash: 90d517200cec5527e98a0d00da466bace587b70b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-azure-automation-runbooks-toorecovery-plans"></a><span data-ttu-id="e2f19-104">Přidat plány toorecovery sady runbook automatizace Azure.</span><span class="sxs-lookup"><span data-stu-id="e2f19-104">Add Azure Automation runbooks toorecovery plans</span></span>
<span data-ttu-id="e2f19-105">V tomto článku jsme popisují, jak Azure Site Recovery integruje se službou Azure Automation toohelp rozšíříte plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-105">In this article, we describe how Azure Site Recovery integrates with Azure Automation toohelp you extend your recovery plans.</span></span> <span data-ttu-id="e2f19-106">Plány obnovení můžete orchestraci obnovení virtuálních počítačů, které jsou chráněné službou Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="e2f19-106">Recovery plans can orchestrate recovery of VMs that are protected with Site Recovery.</span></span> <span data-ttu-id="e2f19-107">Jak pro replikaci tooa sekundární cloud a pro replikace tooAzure pracovat plány obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-107">Recovery plans work both for replication tooa secondary cloud, and for replication tooAzure.</span></span> <span data-ttu-id="e2f19-108">Plány obnovení také pomoci zajistit, aby hello obnovení **přesné**, **repeatable**, a **automatizované**.</span><span class="sxs-lookup"><span data-stu-id="e2f19-108">Recovery plans also help make hello recovery **consistently accurate**, **repeatable**, and **automated**.</span></span> <span data-ttu-id="e2f19-109">Pokud budete selhání tooAzure vaše virtuální počítače, integraci s Azure Automation rozšiřuje plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-109">If you fail over your VMs tooAzure, integration with Azure Automation extends your recovery plans.</span></span> <span data-ttu-id="e2f19-110">Můžete ho tooexecute sady runbook, které nabízí výkonné automatizace úloh.</span><span class="sxs-lookup"><span data-stu-id="e2f19-110">You can use it tooexecute runbooks, which offer powerful automation tasks.</span></span>

<span data-ttu-id="e2f19-111">Pokud jste nový tooAzure automatizace, můžete [zaregistrovat](https://azure.microsoft.com/services/automation/) a [stažení ukázkové skripty](https://azure.microsoft.com/documentation/scripts/).</span><span class="sxs-lookup"><span data-stu-id="e2f19-111">If you are new tooAzure Automation, you can [sign up](https://azure.microsoft.com/services/automation/) and [download sample scripts](https://azure.microsoft.com/documentation/scripts/).</span></span> <span data-ttu-id="e2f19-112">Další informace a toolearn jak tooorchestrate tooAzure obnovení pomocí [plány obnovení](https://azure.microsoft.com/blog/?p=166264), najdete v části [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="e2f19-112">For more information, and toolearn how tooorchestrate recovery tooAzure by using [recovery plans](https://azure.microsoft.com/blog/?p=166264), see [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).</span></span>

<span data-ttu-id="e2f19-113">V tomto článku jsme popisují, jak můžete integrovat Azure Automation runbook do plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-113">In this article, we describe how you can integrate Azure Automation runbooks into your recovery plans.</span></span> <span data-ttu-id="e2f19-114">Používáme tooautomate příklady základní se úlohy, které dříve vyžadovaly ruční zásah.</span><span class="sxs-lookup"><span data-stu-id="e2f19-114">We use examples tooautomate basic tasks that previously required manual intervention.</span></span> <span data-ttu-id="e2f19-115">Můžeme také popisují, jak tooconvert tooa několika kroky obnovení jedním kliknutím akce obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-115">We also describe how tooconvert a multi-step recovery tooa single-click recovery action.</span></span>

## <a name="customize-hello-recovery-plan"></a><span data-ttu-id="e2f19-116">Přizpůsobit plán obnovení hello</span><span class="sxs-lookup"><span data-stu-id="e2f19-116">Customize hello recovery plan</span></span>
1. <span data-ttu-id="e2f19-117">Přejděte toohello **Site Recovery** okna prostředků plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-117">Go toohello **Site Recovery** recovery plan resource blade.</span></span> <span data-ttu-id="e2f19-118">V tomto příkladu hello plán obnovení má dva virtuální počítače přidané tooit, pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-118">For this example, hello recovery plan has two VMs added tooit, for recovery.</span></span> <span data-ttu-id="e2f19-119">toobegin přidání sady runbook, klikněte na tlačítko hello **přizpůsobit** kartě.</span><span class="sxs-lookup"><span data-stu-id="e2f19-119">toobegin adding a runbook, click hello **Customize** tab.</span></span>

    ![Klikněte na tlačítko Přizpůsobit hello](media/site-recovery-runbook-automation-new/essentials-rp.png)


2. <span data-ttu-id="e2f19-121">Klikněte pravým tlačítkem na **1. skupina: spuštění**a potom vyberte **akce po přidání**.</span><span class="sxs-lookup"><span data-stu-id="e2f19-121">Right-click **Group 1: Start**, and then select **Add post action**.</span></span>

    ![Klikněte pravým tlačítkem na skupinu 1: Spuštění a přidejte po akce](media/site-recovery-runbook-automation-new/customize-rp.png)

3. <span data-ttu-id="e2f19-123">Klikněte na tlačítko **vyberte skript**.</span><span class="sxs-lookup"><span data-stu-id="e2f19-123">Click **Choose a script**.</span></span>

4. <span data-ttu-id="e2f19-124">Na hello **akce aktualizace** okno, název hello skriptu **Hello, World**.</span><span class="sxs-lookup"><span data-stu-id="e2f19-124">On hello **Update action** blade, name hello script **Hello World**.</span></span>

    ![okno Akce aktualizace Hello](media/site-recovery-runbook-automation-new/update-rp.png)

5. <span data-ttu-id="e2f19-126">Zadejte název účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="e2f19-126">Enter an Automation account name.</span></span>
    >[!NOTE]
    > <span data-ttu-id="e2f19-127">Hello účet Automation může být v libovolné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="e2f19-127">hello Automation account can be in any Azure region.</span></span> <span data-ttu-id="e2f19-128">Hello účet Automation musí být v hello stejnému předplatnému jako hello trezoru Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="e2f19-128">hello Automation account must be in hello same subscription as hello Azure Site Recovery vault.</span></span>

6. <span data-ttu-id="e2f19-129">V účtu Automation vyberte sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="e2f19-129">In your Automation account, select a runbook.</span></span> <span data-ttu-id="e2f19-130">Tato sada runbook je hello skript, který běží během provádění hello hello plán obnovení po obnovení hello hello první skupiny.</span><span class="sxs-lookup"><span data-stu-id="e2f19-130">This runbook is hello script that runs during hello execution of hello recovery plan, after hello recovery of hello first group.</span></span>

7. <span data-ttu-id="e2f19-131">toosave hello skriptu, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2f19-131">toosave hello script, click **OK**.</span></span> <span data-ttu-id="e2f19-132">Hello skript se přidá příliš**1. skupina: po kroky**.</span><span class="sxs-lookup"><span data-stu-id="e2f19-132">hello script is added too**Group 1: Post-steps**.</span></span>

    ![1:Start skupiny následné akce](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="considerations-for-adding-a-script"></a><span data-ttu-id="e2f19-134">Důležité informace týkající se přidání skriptu</span><span class="sxs-lookup"><span data-stu-id="e2f19-134">Considerations for adding a script</span></span>

* <span data-ttu-id="e2f19-135">Možnosti příliš**odstranit krok** nebo **aktualizovat hello skriptu**, klikněte pravým tlačítkem na skript hello.</span><span class="sxs-lookup"><span data-stu-id="e2f19-135">For options too**delete a step** or **update hello script**, right-click hello script.</span></span>
* <span data-ttu-id="e2f19-136">Skript můžete spustit v Azure při převzetí služeb při selhání z tooAzure místní počítač.</span><span class="sxs-lookup"><span data-stu-id="e2f19-136">A script can run on Azure during failover from an on-premises machine tooAzure.</span></span> <span data-ttu-id="e2f19-137">Ho také můžete spustit v Azure jako skript primární lokalitu před vypnutí, během navrácení služeb po obnovení z Azure tooan na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="e2f19-137">It also can run on Azure as a primary-site script before shutdown, during failback from Azure tooan on-premises machine.</span></span>
* <span data-ttu-id="e2f19-138">Při spuštění skriptu, vloží kontextu plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-138">When a script runs, it injects a recovery plan context.</span></span> <span data-ttu-id="e2f19-139">Hello následující příklad ukazuje kontextové proměnné:</span><span class="sxs-lookup"><span data-stu-id="e2f19-139">hello following example shows a context variable:</span></span>

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

    <span data-ttu-id="e2f19-140">Hello následující tabulka uvádí hello název a popis pro každou proměnnou v kontextu hello.</span><span class="sxs-lookup"><span data-stu-id="e2f19-140">hello following table lists hello name and description of each variable in hello context.</span></span>

    | <span data-ttu-id="e2f19-141">**Název proměnné**</span><span class="sxs-lookup"><span data-stu-id="e2f19-141">**Variable name**</span></span> | <span data-ttu-id="e2f19-142">**Popis**</span><span class="sxs-lookup"><span data-stu-id="e2f19-142">**Description**</span></span> |
    | --- | --- |
    | <span data-ttu-id="e2f19-143">RecoveryPlanName</span><span class="sxs-lookup"><span data-stu-id="e2f19-143">RecoveryPlanName</span></span> |<span data-ttu-id="e2f19-144">Hello název plánu hello spuštěn.</span><span class="sxs-lookup"><span data-stu-id="e2f19-144">hello name of hello plan being run.</span></span> <span data-ttu-id="e2f19-145">Tato proměnná umožňuje provádět různé akce na základě názvu plánu obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="e2f19-145">This variable helps you take different actions based on hello recovery plan name.</span></span> <span data-ttu-id="e2f19-146">Také můžete znovu použít skript hello.</span><span class="sxs-lookup"><span data-stu-id="e2f19-146">You also can reuse hello script.</span></span> |
    | <span data-ttu-id="e2f19-147">FailoverType</span><span class="sxs-lookup"><span data-stu-id="e2f19-147">FailoverType</span></span> |<span data-ttu-id="e2f19-148">Určuje, zda text hello převzetí služeb při selhání testu, plánované, nebo neplánované.</span><span class="sxs-lookup"><span data-stu-id="e2f19-148">Specifies whether hello failover is a test, planned, or unplanned.</span></span> |
    | <span data-ttu-id="e2f19-149">FailoverDirection</span><span class="sxs-lookup"><span data-stu-id="e2f19-149">FailoverDirection</span></span> |<span data-ttu-id="e2f19-150">Určuje, zda obnovení tooa primární nebo sekundární lokality.</span><span class="sxs-lookup"><span data-stu-id="e2f19-150">Specifies whether recovery is tooa primary or secondary site.</span></span> |
    | <span data-ttu-id="e2f19-151">GroupID</span><span class="sxs-lookup"><span data-stu-id="e2f19-151">GroupID</span></span> |<span data-ttu-id="e2f19-152">Určuje číslo skupiny hello v plánu obnovení hello po spuštění plánu hello.</span><span class="sxs-lookup"><span data-stu-id="e2f19-152">Identifies hello group number in hello recovery plan when hello plan is running.</span></span> |
    | <span data-ttu-id="e2f19-153">VmMap</span><span class="sxs-lookup"><span data-stu-id="e2f19-153">VmMap</span></span> |<span data-ttu-id="e2f19-154">Pole všechny virtuální počítače ve skupině hello.</span><span class="sxs-lookup"><span data-stu-id="e2f19-154">An array of all VMs in hello group.</span></span> |
    | <span data-ttu-id="e2f19-155">Klíč VMMap</span><span class="sxs-lookup"><span data-stu-id="e2f19-155">VMMap key</span></span> |<span data-ttu-id="e2f19-156">Jedinečný klíč (GUID) pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e2f19-156">A unique key (GUID) for each VM.</span></span> <span data-ttu-id="e2f19-157">Jeho hello stejný jako hello Azure Virtual Machine Manager (VMM) ID hello virtuální počítač, kde je to možné.</span><span class="sxs-lookup"><span data-stu-id="e2f19-157">It's hello same as hello Azure Virtual Machine Manager (VMM) ID of hello VM, where applicable.</span></span> |
    | <span data-ttu-id="e2f19-158">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="e2f19-158">SubscriptionId</span></span> |<span data-ttu-id="e2f19-159">ID předplatného Azure Hello, ve kterém hello virtuálních počítačů vytvořila.</span><span class="sxs-lookup"><span data-stu-id="e2f19-159">hello Azure subscription ID in which hello VM was created.</span></span> |
    | <span data-ttu-id="e2f19-160">RoleName</span><span class="sxs-lookup"><span data-stu-id="e2f19-160">RoleName</span></span> |<span data-ttu-id="e2f19-161">Název Hello hello virtuálního počítače Azure, který se obnovuje.</span><span class="sxs-lookup"><span data-stu-id="e2f19-161">hello name of hello Azure VM that's being recovered.</span></span> |
    | <span data-ttu-id="e2f19-162">CloudServiceName</span><span class="sxs-lookup"><span data-stu-id="e2f19-162">CloudServiceName</span></span> |<span data-ttu-id="e2f19-163">název služby Azure cloud Hello, pod kterým hello virtuálních počítačů vytvořila.</span><span class="sxs-lookup"><span data-stu-id="e2f19-163">hello Azure cloud service name under which hello VM was created.</span></span> |
    | <span data-ttu-id="e2f19-164">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="e2f19-164">ResourceGroupName</span></span>|<span data-ttu-id="e2f19-165">Název skupiny prostředků Azure Hello, pod kterým hello virtuálních počítačů vytvořila.</span><span class="sxs-lookup"><span data-stu-id="e2f19-165">hello Azure resource group name under which hello VM was created.</span></span> |
    | <span data-ttu-id="e2f19-166">RecoveryPointId</span><span class="sxs-lookup"><span data-stu-id="e2f19-166">RecoveryPointId</span></span>|<span data-ttu-id="e2f19-167">Hello časové razítko pro při obnovení hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e2f19-167">hello timestamp for when hello VM is recovered.</span></span> |

* <span data-ttu-id="e2f19-168">Zajistěte, aby byl tento účet Automation hello hello následující moduly:</span><span class="sxs-lookup"><span data-stu-id="e2f19-168">Ensure that hello Automation account has hello following modules:</span></span>
    * <span data-ttu-id="e2f19-169">AzureRM.profile</span><span class="sxs-lookup"><span data-stu-id="e2f19-169">AzureRM.profile</span></span>
    * <span data-ttu-id="e2f19-170">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="e2f19-170">AzureRM.Resources</span></span>
    * <span data-ttu-id="e2f19-171">AzureRM.Automation</span><span class="sxs-lookup"><span data-stu-id="e2f19-171">AzureRM.Automation</span></span>
    * <span data-ttu-id="e2f19-172">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="e2f19-172">AzureRM.Network</span></span>
    * <span data-ttu-id="e2f19-173">AzureRM.Compute</span><span class="sxs-lookup"><span data-stu-id="e2f19-173">AzureRM.Compute</span></span>

<span data-ttu-id="e2f19-174">Všechny moduly musí být kompatibilní verze.</span><span class="sxs-lookup"><span data-stu-id="e2f19-174">All modules should be of compatible versions.</span></span> <span data-ttu-id="e2f19-175">Tooensure snadný způsob, jestli jsou všechny moduly kompatibilní je toouse hello nejnovější verze všechny moduly hello.</span><span class="sxs-lookup"><span data-stu-id="e2f19-175">An easy way tooensure that all modules are compatible is toouse hello latest versions of all hello modules.</span></span>

### <a name="access-all-vms-of-hello-vmmap-in-a-loop"></a><span data-ttu-id="e2f19-176">Přístup k všechny virtuální počítače hello VMMap ve smyčce</span><span class="sxs-lookup"><span data-stu-id="e2f19-176">Access all VMs of hello VMMap in a loop</span></span>
<span data-ttu-id="e2f19-177">Použijte následující kód tooloop napříč všechny virtuální počítače hello Microsoft VMMap hello:</span><span class="sxs-lookup"><span data-stu-id="e2f19-177">Use hello following code tooloop across all VMs of hello Microsoft VMMap:</span></span>

```
$VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
$vmMap = $RecoveryPlanContext.VmMap
 foreach($VMID in $VMinfo)
 {
     $VM = $vmMap.$VMID                
             if( !(($VM -eq $Null) -Or ($VM.ResourceGroupName -eq $Null) -Or ($VM.RoleName -eq $Null))) {
         #this check is tooensure that we skip when some data is not available else it will fail
 Write-output "Resource group name ", $VM.ResourceGroupName
 Write-output "Rolename " = $VM.RoleName
     }
 }

```

> [!NOTE]
> <span data-ttu-id="e2f19-178">Hello prostředků skupiny název a role název hodnoty jsou prázdné, pokud je skript hello spouštěcí skupině tooa před akcí.</span><span class="sxs-lookup"><span data-stu-id="e2f19-178">hello resource group name and role name values are empty when hello script is a pre-action tooa boot group.</span></span> <span data-ttu-id="e2f19-179">Hello hodnoty jsou naplněny pouze v případě, že v převzetí služeb při selhání se podaří hello virtuálních počítačů této skupiny.</span><span class="sxs-lookup"><span data-stu-id="e2f19-179">hello values are populated only if hello VM of that group succeeds in failover.</span></span> <span data-ttu-id="e2f19-180">skript Hello je po akce hello spouštěcí skupině.</span><span class="sxs-lookup"><span data-stu-id="e2f19-180">hello script is a post-action of hello boot group.</span></span>

## <a name="use-hello-same-automation-runbook-in-multiple-recovery-plans"></a><span data-ttu-id="e2f19-181">Použití hello stejné sady Automation runbook v více plánů obnovení</span><span class="sxs-lookup"><span data-stu-id="e2f19-181">Use hello same Automation runbook in multiple recovery plans</span></span>

<span data-ttu-id="e2f19-182">Můžete pomocí jednoho skriptu v více plánů obnovení pomocí externích proměnných.</span><span class="sxs-lookup"><span data-stu-id="e2f19-182">You can use a single script in multiple recovery plans by using external variables.</span></span> <span data-ttu-id="e2f19-183">Můžete použít [proměnné automatizace Azure](../automation/automation-variables.md) toostore parametry, které můžete předat pro obnovení, naplánujte spuštění.</span><span class="sxs-lookup"><span data-stu-id="e2f19-183">You can use [Azure Automation variables](../automation/automation-variables.md) toostore parameters that you can pass for a recovery plan execution.</span></span> <span data-ttu-id="e2f19-184">Přidáním název plánu obnovení hello jako předpona toohello proměnné, můžete vytvořit jednotlivé proměnných pro každý plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-184">By adding hello recovery plan name as a prefix toohello variable, you can create individual variables for each recovery plan.</span></span> <span data-ttu-id="e2f19-185">Pak použijte proměnné hello jako parametry.</span><span class="sxs-lookup"><span data-stu-id="e2f19-185">Then, use hello variables as parameters.</span></span> <span data-ttu-id="e2f19-186">Můžete změnit parametr beze změny hello skript, ale stále změnu hello způsobem hello skriptu funguje.</span><span class="sxs-lookup"><span data-stu-id="e2f19-186">You can change a parameter without changing hello script, but still change hello way hello script works.</span></span>

### <a name="use-a-simple-string-variable-in-a-runbook-script"></a><span data-ttu-id="e2f19-187">Použití jednoduché řetězec proměnné ve skriptu runbook</span><span class="sxs-lookup"><span data-stu-id="e2f19-187">Use a simple string variable in a runbook script</span></span>

<span data-ttu-id="e2f19-188">V tomto příkladu skript přijímá hello vstup z skupina zabezpečení sítě (NSG) a použije ho toohello virtuální počítače v plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-188">In this example, a script takes hello input of a Network Security Group (NSG) and applies it toohello VMs of a recovery plan.</span></span>

<span data-ttu-id="e2f19-189">Pro skript toodetect hello plánu obnovení je spuštěna použijte kontextu plán obnovení hello:</span><span class="sxs-lookup"><span data-stu-id="e2f19-189">For hello script toodetect which recovery plan is running, use hello recovery plan context:</span></span>

```
workflow AddPublicIPAndNSG {
    param (
          [parameter(Mandatory=$false)]
          [Object]$RecoveryPlanContext
    )

    $RPName = $RecoveryPlanContext.RecoveryPlanName
```

<span data-ttu-id="e2f19-190">tooapply existující skupina NSG, musíte znát název hello NSG a název skupiny prostředků hello NSG.</span><span class="sxs-lookup"><span data-stu-id="e2f19-190">tooapply an existing NSG, you must know hello NSG name and hello NSG resource group name.</span></span> <span data-ttu-id="e2f19-191">Tyto proměnné můžete použijte jako vstupy pro skripty plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-191">Use these variables as inputs for recovery plan scripts.</span></span> <span data-ttu-id="e2f19-192">toodo, vytvořte dvě proměnné v hello prostředky účet Automation.</span><span class="sxs-lookup"><span data-stu-id="e2f19-192">toodo this, create two variables in hello Automation account assets.</span></span> <span data-ttu-id="e2f19-193">Přidejte název hello hello plán obnovení, který vytváříte hello parametry pro jako název proměnné toohello předponu.</span><span class="sxs-lookup"><span data-stu-id="e2f19-193">Add hello name of hello recovery plan that you are creating hello parameters for as a prefix toohello variable name.</span></span>

1. <span data-ttu-id="e2f19-194">Vytvořte název proměnné toostore hello NSG.</span><span class="sxs-lookup"><span data-stu-id="e2f19-194">Create a variable toostore hello NSG name.</span></span> <span data-ttu-id="e2f19-195">Přidejte název proměnné toohello předponu pomocí názvu hello hello plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-195">Add a prefix toohello variable name by using hello name of hello recovery plan.</span></span>

    ![Vytvořte proměnnou Název skupiny NSG](media/site-recovery-runbook-automation-new/var1.png)

2. <span data-ttu-id="e2f19-197">Vytvořte název skupiny prostředků NSG proměnné toostore hello.</span><span class="sxs-lookup"><span data-stu-id="e2f19-197">Create a variable toostore hello NSG's resource group name.</span></span> <span data-ttu-id="e2f19-198">Přidejte název proměnné toohello předponu pomocí názvu hello hello plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-198">Add a prefix toohello variable name by using hello name of hello recovery plan.</span></span>

    ![Vytvoření názvu skupiny prostředků NSG](media/site-recovery-runbook-automation-new/var2.png)


3.  <span data-ttu-id="e2f19-200">Ve skriptu hello použijte následující hodnoty proměnné tooget hello kódu pro odkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e2f19-200">In hello script, use hello following reference code tooget hello variable values:</span></span>

    ```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
    ```

4.  <span data-ttu-id="e2f19-201">Použití proměnné hello v hello runbook tooapply hello NSG toohello síťové rozhraní služeb hello při selhání virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="e2f19-201">Use hello variables in hello runbook tooapply hello NSG toohello network interface of hello failed-over VM:</span></span>

    ```
    InlineScript {
    if (($Using:NSGname -ne $Null) -And ($Using:NSGRGname -ne $Null)) {
            $NSG = Get-AzureRmNetworkSecurityGroup -Name $Using:NSGname -ResourceGroupName $Using:NSGRGname
            Write-output $NSG.Id
            #Apply hello NSG tooa network interface
            #$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
            #Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            #  -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $NSG
        }
    }
    ```

<span data-ttu-id="e2f19-202">Pro každý plán obnovení vytvořte nezávislých proměnných, takže můžete opakovaně použít skript hello.</span><span class="sxs-lookup"><span data-stu-id="e2f19-202">For each recovery plan, create independent variables so that you can reuse hello script.</span></span> <span data-ttu-id="e2f19-203">Přidání předpony pomocí hello název plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-203">Add a prefix by using hello recovery plan name.</span></span> <span data-ttu-id="e2f19-204">Pro dokončení, začátku do konce skript pro tento scénář, najdete v části [během testovacího převzetí služeb při selhání plánu obnovení Site Recovery přidat veřejné IP adresy a NSG tooVMs](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).</span><span class="sxs-lookup"><span data-stu-id="e2f19-204">For a complete, end-to-end script for this scenario, see [Add a public IP and NSG tooVMs during test failover of a Site Recovery recovery plan](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).</span></span>


### <a name="use-a-complex-variable-toostore-more-information"></a><span data-ttu-id="e2f19-205">Použití proměnné komplexní toostore Další informace</span><span class="sxs-lookup"><span data-stu-id="e2f19-205">Use a complex variable toostore more information</span></span>

<span data-ttu-id="e2f19-206">Vezměte v úvahu scénář, ve kterém chcete tooturn jednoho skriptu na veřejnou IP adresu pro konkrétní virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="e2f19-206">Consider a scenario in which you want a single script tooturn on a public IP on specific VMs.</span></span> <span data-ttu-id="e2f19-207">V jiné scénáři můžete chtít tooapply odlišné skupiny Nsg na různé virtuální počítače (ne na všech virtuálních počítačích).</span><span class="sxs-lookup"><span data-stu-id="e2f19-207">In another scenario, you might want tooapply different NSGs on different VMs (not on all VMs).</span></span> <span data-ttu-id="e2f19-208">Můžete použít skript, který je opakovaně použitelné pro každého plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-208">You can make a script that is reusable for any recovery plan.</span></span> <span data-ttu-id="e2f19-209">Proměnný počet virtuálních počítačů může mít každý plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-209">Each recovery plan can have a variable number of VMs.</span></span> <span data-ttu-id="e2f19-210">Například obnovení služby SharePoint má dva elementy front end.</span><span class="sxs-lookup"><span data-stu-id="e2f19-210">For example, a SharePoint recovery has two front ends.</span></span> <span data-ttu-id="e2f19-211">Základní-obchodní (LOB) aplikace má pouze jeden front-endu.</span><span class="sxs-lookup"><span data-stu-id="e2f19-211">A basic line-of-business (LOB) application has only one front end.</span></span> <span data-ttu-id="e2f19-212">Nelze vytvořit samostatné proměnných pro každý plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="e2f19-212">You cannot create separate variables for each recovery plan.</span></span> 

<span data-ttu-id="e2f19-213">V následující ukázka hello, jsme nové způsobem a vytvoříte [komplexní proměnná](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) v prostředky účet Azure Automation hello.</span><span class="sxs-lookup"><span data-stu-id="e2f19-213">In hello following example, we use a new technique and create a [complex variable](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) in hello Azure Automation account assets.</span></span> <span data-ttu-id="e2f19-214">To uděláte tak, že zadáte více hodnot.</span><span class="sxs-lookup"><span data-stu-id="e2f19-214">You do this by specifying multiple values.</span></span> <span data-ttu-id="e2f19-215">Je nutné použít hello toocomplete prostředí Azure PowerShell následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e2f19-215">You must use Azure PowerShell toocomplete hello following steps:</span></span>

1. <span data-ttu-id="e2f19-216">V prostředí PowerShell Přihlaste se tooyour předplatné Azure:</span><span class="sxs-lookup"><span data-stu-id="e2f19-216">In PowerShell, sign in tooyour Azure subscription:</span></span>

    ```
    login-azurermaccount
    $sub = Get-AzureRmSubscription -Name <SubscriptionName>
    $sub | Select-AzureRmSubscription
    ```

2. <span data-ttu-id="e2f19-217">toostore hello parametry, jak vytvořit proměnnou komplexní hello pomocí názvu hello hello plánu obnovení:</span><span class="sxs-lookup"><span data-stu-id="e2f19-217">toostore hello parameters, create hello complex variable by using hello name of hello recovery plan:</span></span>

    ```
    $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

3. <span data-ttu-id="e2f19-218">V této proměnné komplexní **VMDetails** je hello ID virtuálního počítače pro hello chráněných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e2f19-218">In this complex variable, **VMDetails** is hello VM ID for hello protected VM.</span></span> <span data-ttu-id="e2f19-219">tooget hello ID virtuálního počítače v hello portál Azure, zobrazit vlastnosti virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="e2f19-219">tooget hello VM ID, in hello Azure portal, view hello VM properties.</span></span> <span data-ttu-id="e2f19-220">Hello následující snímek obrazovky ukazuje proměnné, která ukládá hello podrobnosti o dva virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="e2f19-220">hello following screenshot shows a variable that stores hello details of two VMs:</span></span>

    ![Použít hello ID virtuálního počítače jako hello GUID](media/site-recovery-runbook-automation-new/vmguid.png)

4. <span data-ttu-id="e2f19-222">Pomocí této proměnné v runbooku.</span><span class="sxs-lookup"><span data-stu-id="e2f19-222">Use this variable in your runbook.</span></span> <span data-ttu-id="e2f19-223">Pokud hello označil že identifikátor GUID virtuálního počítače je nalezeno v kontextu plán obnovení hello, použít hello NSG na hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="e2f19-223">If hello indicated VM GUID is found in hello recovery plan context, apply hello NSG on hello VM:</span></span>

    ```
    $VMDetailsObj = Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName
    ```

4. <span data-ttu-id="e2f19-224">V sadě runbook projděte virtuální počítače hello kontextu plán obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="e2f19-224">In your runbook, loop through hello VMs of hello recovery plan context.</span></span> <span data-ttu-id="e2f19-225">Zkontrolujte, zda text hello virtuálních počítačů existuje v **$VMDetailsObj**.</span><span class="sxs-lookup"><span data-stu-id="e2f19-225">Check whether hello VM exists in **$VMDetailsObj**.</span></span> <span data-ttu-id="e2f19-226">Pokud existuje, přístup k vlastnostem hello hello proměnné tooapply hello NSG:</span><span class="sxs-lookup"><span data-stu-id="e2f19-226">If it exists, access hello properties of hello variable tooapply hello NSG:</span></span>

    ```
        $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
        $vmMap = $RecoveryPlanContext.VmMap

        foreach($VMID in $VMinfo) {
            Write-output $VMDetailsObj.value.$VMID

            if ($VMDetailsObj.value.$VMID -ne $Null) { #If hello VM exists in hello context, this will not b Null
                $VM = $vmMap.$VMID
                # Access hello properties of hello variable
                $NSGname = $VMDetailsObj.value.$VMID.'NSGName'
                $NSGRGname = $VMDetailsObj.value.$VMID.'NSGResourceGroupName'

                # Add code tooapply hello NSG properties toohello VM
            }
        }
    ```

<span data-ttu-id="e2f19-227">Hello stejný skriptu můžete použít pro jinou obnovení plány.</span><span class="sxs-lookup"><span data-stu-id="e2f19-227">You can use hello same script for different recovery plans.</span></span> <span data-ttu-id="e2f19-228">Zadejte jiné parametry uložením hello hodnotu, která odpovídá plánu obnovení tooa v různých proměnných.</span><span class="sxs-lookup"><span data-stu-id="e2f19-228">Enter different parameters by storing hello value that corresponds tooa recovery plan in different variables.</span></span>

## <a name="sample-scripts"></a><span data-ttu-id="e2f19-229">Ukázkové skripty</span><span class="sxs-lookup"><span data-stu-id="e2f19-229">Sample scripts</span></span>

<span data-ttu-id="e2f19-230">toodeploy ukázkové skripty tooyour účet Automation, klikněte na tlačítko hello **nasazení tooAzure** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e2f19-230">toodeploy sample scripts tooyour Automation account, click hello **Deploy tooAzure** button.</span></span>

<span data-ttu-id="e2f19-231">[![Nasazení tooAzure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)</span><span class="sxs-lookup"><span data-stu-id="e2f19-231">[![Deploy tooAzure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)</span></span>

<span data-ttu-id="e2f19-232">Jiný příklad najdete v tématu hello následující videa.</span><span class="sxs-lookup"><span data-stu-id="e2f19-232">For another example, see hello following video.</span></span> <span data-ttu-id="e2f19-233">Ukazuje, jak toorecover tooAzure aplikace WordPress dvouvrstvá:</span><span class="sxs-lookup"><span data-stu-id="e2f19-233">It demonstrates how toorecover a two-tier WordPress application tooAzure:</span></span>


> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="additional-resources"></a><span data-ttu-id="e2f19-234">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e2f19-234">Additional resources</span></span>
* [<span data-ttu-id="e2f19-235">Azure Automation spustit jako účet služby</span><span class="sxs-lookup"><span data-stu-id="e2f19-235">Azure Automation service Run As account</span></span>](../automation/automation-sec-configure-azure-runas-account.md)
* [<span data-ttu-id="e2f19-236">Přehled Azure Automation</span><span class="sxs-lookup"><span data-stu-id="e2f19-236">Azure Automation overview</span></span>](http://msdn.microsoft.com/library/azure/dn643629.aspx "přehled Azure Automation.")
* [<span data-ttu-id="e2f19-237">Azure Automation ukázkové skripty</span><span class="sxs-lookup"><span data-stu-id="e2f19-237">Azure Automation sample scripts</span></span>](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "ukázkové skripty Azure Automation.")
