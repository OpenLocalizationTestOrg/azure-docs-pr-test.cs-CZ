---
title: "Spravovat a monitorovat zálohování virtuálního počítače Azure | Microsoft Docs"
description: "Zjistěte, jak spravovat a monitorovat zálohování virtuálního počítače Azure"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 4372944e-d33a-4f6a-bd5f-d04af2842713
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: trinadhk;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d876bb1759600fa29a26730bfa8b4ec19db1e442
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="manage-common-azure-backup-jobs-and-trigger-alerts-in-the-classic-portal"></a><span data-ttu-id="027b9-103">Spravovat běžné úlohy zálohování Azure a aktivační události upozornění v portálu classic</span><span class="sxs-lookup"><span data-stu-id="027b9-103">Manage common Azure Backup jobs and trigger alerts in the classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="027b9-104">Spravovat zálohy virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="027b9-104">Manage Azure VM backups</span></span>](backup-azure-manage-vms.md)
> * [<span data-ttu-id="027b9-105">Spravovat zálohy Classic virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="027b9-105">Manage Classic VM backups</span></span>](backup-azure-manage-vms-classic.md)
>
>

<span data-ttu-id="027b9-106">Tento článek obsahuje informace o běžné úlohy správy a sledování pro Classic model virtuální počítače chráněné v Azure.</span><span class="sxs-lookup"><span data-stu-id="027b9-106">This article provides information about common management and monitoring tasks for Classic-model virtual machines protected in Azure.</span></span>  

> [!NOTE]
> <span data-ttu-id="027b9-107">Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="027b9-107">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="027b9-108">V tématu [Příprava prostředí pro zálohování virtuálních počítačů Azure](backup-azure-vms-prepare.md) podrobnosti o práci s klasického nasazení modelu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="027b9-108">See [Prepare your environment to back up Azure virtual machines](backup-azure-vms-prepare.md) for details on working with Classic deployment model VMs.</span></span>
>
> [!IMPORTANT]
><span data-ttu-id="027b9-109">Od března 2017 již nelze k vytvoření trezorů služby Backup použít portál Classic.</span><span class="sxs-lookup"><span data-stu-id="027b9-109">Starting March 2017, you can no longer use the classic portal to create Backup vaults.</span></span>
>
> <span data-ttu-id="027b9-110">Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="027b9-110">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="027b9-111">Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="027b9-111">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="027b9-112">Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="027b9-112">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="027b9-113">Od 15. října 2017 nebude možné pomocí PowerShellu vytvářet trezory služby Backup.</span><span class="sxs-lookup"><span data-stu-id="027b9-113">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="027b9-114">**Do 1. listopadu 2017:**</span><span class="sxs-lookup"><span data-stu-id="027b9-114">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="027b9-115">Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="027b9-115">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="027b9-116">Nebudete mít přístup k datům záloh na portálu Classic.</span><span class="sxs-lookup"><span data-stu-id="027b9-116">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="027b9-117">Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="027b9-117">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>

## <a name="manage-protected-virtual-machines"></a><span data-ttu-id="027b9-118">Správa chráněných virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="027b9-118">Manage protected virtual machines</span></span>
<span data-ttu-id="027b9-119">Správa chráněných virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="027b9-119">To manage protected virtual machines:</span></span>

1. <span data-ttu-id="027b9-120">Zobrazení a správa klikněte na tlačítko Nastavení zálohování pro virtuální počítač **chráněné položky** kartě.</span><span class="sxs-lookup"><span data-stu-id="027b9-120">To view and manage backup settings for a virtual machine click the **Protected Items** tab.</span></span>
2. <span data-ttu-id="027b9-121">Klikněte na název chráněné položky zobrazíte **zálohování podrobnosti** kartu, která vám zobrazí informace o poslední zálohy.</span><span class="sxs-lookup"><span data-stu-id="027b9-121">Click on the name of a protected item to see the **Backup Details** tab, which shows you information about the last backup.</span></span>

    ![Záloha virtuálního počítače](./media/backup-azure-manage-vms/backup-vmdetails.png)
3. <span data-ttu-id="027b9-123">Zobrazení a správa klikněte na nastavení zásad zálohování pro virtuální počítač **zásady** kartě.</span><span class="sxs-lookup"><span data-stu-id="027b9-123">To view and manage backup policy settings for a virtual machine click the **Policies** tab.</span></span>

    ![Virtuální počítač zásad](./media/backup-azure-manage-vms/manage-policy-settings.png)

    <span data-ttu-id="027b9-125">**Zásady zálohování** kartě se zobrazují existující zásady.</span><span class="sxs-lookup"><span data-stu-id="027b9-125">The **Backup Policies** tab shows you the existing policy.</span></span> <span data-ttu-id="027b9-126">Můžete upravit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="027b9-126">You can modify as needed.</span></span> <span data-ttu-id="027b9-127">Pokud budete muset vytvořit nové zásady, klepněte na tlačítko **vytvořit** na **zásady** stránky.</span><span class="sxs-lookup"><span data-stu-id="027b9-127">If you need to create a new policy click **Create** on the **Policies** page.</span></span> <span data-ttu-id="027b9-128">Všimněte si, že pokud chcete odebrat zásadu by neměl mít všechny virtuální počítače s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="027b9-128">Note that if you want to remove a policy it shouldn't have any virtual machines associated with it.</span></span>

    ![Virtuální počítač zásad](./media/backup-azure-manage-vms/backup-vmpolicy.png)
4. <span data-ttu-id="027b9-130">Můžete získat další informace o stavu nebo akce pro virtuální počítač **úlohy** stránky.</span><span class="sxs-lookup"><span data-stu-id="027b9-130">You can get more information about actions or status for a virtual machine on the **Jobs** page.</span></span> <span data-ttu-id="027b9-131">Klikněte na úlohu v seznamu zobrazíte další podrobnosti, nebo filtrovat úlohy pro konkrétní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="027b9-131">Click a job in the list to get more details, or filter jobs for a specific virtual machine.</span></span>

    ![Úlohy](./media/backup-azure-manage-vms/backup-job.png)

## <a name="on-demand-backup-of-a-virtual-machine"></a><span data-ttu-id="027b9-133">Zálohování na vyžádání virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="027b9-133">On-demand backup of a virtual machine</span></span>
<span data-ttu-id="027b9-134">Na vyžádání můžete provést zálohování virtuálního počítače, jakmile je nakonfigurován pro ochranu.</span><span class="sxs-lookup"><span data-stu-id="027b9-134">You can take an on-demand backup of a virtual machine once it is configured for protection.</span></span> <span data-ttu-id="027b9-135">Pokud prvotní zálohování čeká na vyřízení pro virtuální počítač, bude zálohování na vyžádání vytvoření úplné kopie virtuálního počítače v záložním trezoru Azure.</span><span class="sxs-lookup"><span data-stu-id="027b9-135">If the initial backup is pending for the virtual machine, on-demand backup will create a full copy of the virtual machine in Azure backup vault.</span></span> <span data-ttu-id="027b9-136">Pokud se první zálohování je dokončeno, bude zálohování na vyžádání jen odeslání změn z předchozí zálohy pro zálohování Azure trezoru tj ho je vždy přírůstkové.</span><span class="sxs-lookup"><span data-stu-id="027b9-136">If first backup is completed, on-demand backup will only send changes from previous backup to Azure backup vault i.e. it is always incremental.</span></span>

> [!NOTE]
> <span data-ttu-id="027b9-137">Rozsah uchování zálohu na vyžádání je nastavena na hodnotu uchování zadaná pro denní doba uchování ve zásady zálohování odpovídající virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="027b9-137">Retention range of an on-demand backup is set to retention value specified for Daily retention in backup policy corresponding to the VM.</span></span>  
>
>

<span data-ttu-id="027b9-138">Chcete-li provést zálohování virtuálního počítače na vyžádání:</span><span class="sxs-lookup"><span data-stu-id="027b9-138">To take an on-demand backup of a virtual machine:</span></span>

1. <span data-ttu-id="027b9-139">Přejděte na **chráněné položky** a vyberte **virtuální počítač Azure** jako **typ** (Pokud již není vybrána) a klikněte na **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="027b9-139">Navigate to the **Protected Items** page and select **Azure Virtual Machine** as **Type** (if not already selected) and click on **Select** button.</span></span>

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="027b9-141">Vyberte virtuální počítač, na kterém chcete provést zálohování na vyžádání a klikněte na **zálohování** tlačítko v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="027b9-141">Select the virtual machine on which you want to take an on-demand backup and click on **Backup Now** button at the bottom of the page.</span></span>

    ![Zálohovat nyní](./media/backup-azure-manage-vms/backup-now.png)

    <span data-ttu-id="027b9-143">Tím se vytvoří úlohu zálohování na vybraný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="027b9-143">This will create a backup job on the selected virtual machine.</span></span> <span data-ttu-id="027b9-144">Rozsah uchování bodu obnovení, které jsou vytvořené pomocí této úlohy bude stejné jako příkaz, který je uveden v zásadách, které jsou spojené s virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="027b9-144">Retention range of recovery point created through this job will be same as that specified in the policy associated with the virtual machine.</span></span>

    ![Vytvoření úlohy zálohování](./media/backup-azure-manage-vms/creating-job.png)

   > [!NOTE]
   > <span data-ttu-id="027b9-146">Chcete-li zobrazit zásady přidružené virtuální počítač, přejděte do virtuálního počítače v **chráněné položky** stránky a přejděte na kartu zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="027b9-146">To view the policy associated with a virtual machine, drill down into virtual machine in the **Protected Items** page and go to backup policy tab.</span></span>
   >
   >
3. <span data-ttu-id="027b9-147">Po vytvoření úlohy, můžete kliknutím na **zobrazit úlohy** tlačítka na panelu informační zobrazíte odpovídající úlohy na stránce úloh.</span><span class="sxs-lookup"><span data-stu-id="027b9-147">Once the job is created, you can click on **View job** button in the toast bar to see the corresponding job in the jobs page.</span></span>

    ![Vytvořit úlohu zálohování](./media/backup-azure-manage-vms/created-job.png)
4. <span data-ttu-id="027b9-149">Po úspěšném dokončení úlohy bude možné vytvořit bod obnovení, který můžete použít k obnovení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="027b9-149">After successful completion of the job, a recovery point will be created which you can use to restore the virtual machine.</span></span> <span data-ttu-id="027b9-150">Hodnota sloupce bodu obnovení to také se zvýší o 1 v **chráněné položky** stránky.</span><span class="sxs-lookup"><span data-stu-id="027b9-150">This will also increment the recovery point column value by 1 in **Protected Items** page.</span></span>

## <a name="stop-protecting-virtual-machines"></a><span data-ttu-id="027b9-151">Zastavte ochranu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="027b9-151">Stop protecting virtual machines</span></span>
<span data-ttu-id="027b9-152">Můžete zastavit budoucí zálohy virtuálního počítače s následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="027b9-152">You can choose to stop the future backups of a virtual machine with the following options:</span></span>

* <span data-ttu-id="027b9-153">Zachovat zálohovaná data související s virtuálním počítačem v trezoru zálohování Azure</span><span class="sxs-lookup"><span data-stu-id="027b9-153">Retain backup data associated with virtual machine in Azure Backup vault</span></span>
* <span data-ttu-id="027b9-154">Odstranit záložní data související s virtuálním počítačem</span><span class="sxs-lookup"><span data-stu-id="027b9-154">Delete backup data associated with virtual machine</span></span>

<span data-ttu-id="027b9-155">Pokud jste vybrali možnost zachování zálohovaná data související s virtuálním počítačem, můžete tento virtuální počítač obnovit zálohovaná data.</span><span class="sxs-lookup"><span data-stu-id="027b9-155">If you have selected to retain backup data associated with virtual machine, you can use the backup data to restore the virtual machine.</span></span> <span data-ttu-id="027b9-156">O cenách podrobnosti takových virtuálních počítačů, klikněte na tlačítko [zde](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="027b9-156">For pricing details for such virtual machines, click [here](https://azure.microsoft.com/pricing/details/backup/).</span></span>

<span data-ttu-id="027b9-157">Ukončete ochranu pro virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="027b9-157">To Stop protection for a virtual machine:</span></span>

1. <span data-ttu-id="027b9-158">Přejděte na **chráněné položky** a vyberte **virtuální počítač Azure** jako typ filtru (Pokud již není vybrána) a klikněte na **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="027b9-158">Navigate to **Protected Items** page and select **Azure virtual machine** as the filter type (if not already selected) and click on **Select** button.</span></span>

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="027b9-160">Vyberte virtuální počítač a klikněte na **zastavit ochranu** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="027b9-160">Select the virtual machine and click on **Stop Protection** at the bottom of the page.</span></span>

    ![Zastavení ochrany](./media/backup-azure-manage-vms/stop-protection.png)
3. <span data-ttu-id="027b9-162">Ve výchozím nastavení zálohování Azure nedojde k odstranění zálohovaná data související s virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="027b9-162">By default, Azure Backup doesn’t delete the backup data associated with the virtual machine.</span></span>

    ![Potvrďte zastavení ochrany](./media/backup-azure-manage-vms/confirm-stop-protection.png)

    <span data-ttu-id="027b9-164">Pokud chcete odstranit záložní data, zaškrtněte políčko.</span><span class="sxs-lookup"><span data-stu-id="027b9-164">If you want to delete backup data, select the check box.</span></span>

    ![Zaškrtávací políčko](./media/backup-azure-manage-vms/checkbox.png)

    <span data-ttu-id="027b9-166">Vyberte důvod zastavení zálohování.</span><span class="sxs-lookup"><span data-stu-id="027b9-166">Please select a reason for stopping the backup.</span></span> <span data-ttu-id="027b9-167">Zatímco tato položka je nepovinná, poskytování důvod pomůže Azure Backup za účelem pracovat na zpětnou vazbu a určit jejich prioritu scénáře zákazníka.</span><span class="sxs-lookup"><span data-stu-id="027b9-167">While this is optional, providing a reason will help Azure Backup to work on the feedback and prioritize the customer scenarios.</span></span>
4. <span data-ttu-id="027b9-168">Klikněte na **odeslání** tlačítko Odeslat **zastavit ochranu** úlohy.</span><span class="sxs-lookup"><span data-stu-id="027b9-168">Click on **Submit** button to submit the **Stop protection** job.</span></span> <span data-ttu-id="027b9-169">Klikněte na **zobrazit úlohy** zobrazíte úlohu v odpovídající **úlohy** stránky.</span><span class="sxs-lookup"><span data-stu-id="027b9-169">Click on **View Job** to see the corresponding the job in **Jobs** page.</span></span>

    ![Zastavení ochrany](./media/backup-azure-manage-vms/stop-protect-success.png)

    <span data-ttu-id="027b9-171">Pokud nevyberete **odstranit související zálohovaná data** možnost během **zastavit ochranu** průvodce a po dokončení úlohy post, stav ochrany se změní na **ukončena ochrana**.</span><span class="sxs-lookup"><span data-stu-id="027b9-171">If you have not selected **Delete associated backup data** option during **Stop Protection** wizard, then post job completion, protection status changes to **Protection Stopped**.</span></span> <span data-ttu-id="027b9-172">Data zůstávají s Azure Backup, dokud je explicitně odstranit.</span><span class="sxs-lookup"><span data-stu-id="027b9-172">The data remains with Azure Backup until it is explicitly deleted.</span></span> <span data-ttu-id="027b9-173">Data můžete odstranit vždy tak, že vyberete virtuální počítač **chráněné položky** stránky a kliknutím na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="027b9-173">You can always delete the data by selecting the virtual machine in the **Protected Items** page and clicking **Delete**.</span></span>

    ![Ukončeno ochrany](./media/backup-azure-manage-vms/protection-stopped-status.png)

    <span data-ttu-id="027b9-175">Pokud jste vybrali **odstranit související zálohovaná data** možnost, virtuální počítač nebude součástí **chráněné položky** stránky.</span><span class="sxs-lookup"><span data-stu-id="027b9-175">If you have selected the **Delete associated backup data** option, the virtual machine won’t be part of the **Protected Items** page.</span></span>

## <a name="re-protect-virtual-machine"></a><span data-ttu-id="027b9-176">Znovu nastavit ochranu virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="027b9-176">Re-protect Virtual machine</span></span>
<span data-ttu-id="027b9-177">Pokud nevyberete **odstranění přidružení zálohovaná data** možnost **zastavit ochranu**, můžete znovu nastavit ochranu virtuálního počítače pomocí kroků podobná zálohování registrované virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="027b9-177">If you have not selected the **Delete associate backup data** option in **Stop Protection**, you can re-protect the virtual machine by following the steps similar to backing up registered virtual machines.</span></span> <span data-ttu-id="027b9-178">Jakmile chráněný, tento virtuální počítač bude mít zálohovaná data zachována před zastavení ochrany a vytvoří body obnovení po se znovu nastavit ochranu.</span><span class="sxs-lookup"><span data-stu-id="027b9-178">Once protected, this virtual machine will have backup data retained prior to stop protection and recovery points created after re-protect.</span></span>

<span data-ttu-id="027b9-179">Po znovu nastavit ochranu, se změní stav ochrany virtuálního počítače na **chráněné** Pokud existují bodů obnovení před **zastavit ochranu**.</span><span class="sxs-lookup"><span data-stu-id="027b9-179">After re-protect, the virtual machine’s protection status will be changed to **Protected** if there are recovery points prior to **Stop Protection**.</span></span>

  ![Vytvořit ochranu virtuálního počítače](./media/backup-azure-manage-vms/reprotected-status.png)

> [!NOTE]
> <span data-ttu-id="027b9-181">Při znovu ochranu virtuálního počítače, můžete zvolit jinou zásadu než zásada, ke které byl virtuální počítač původně chráněný.</span><span class="sxs-lookup"><span data-stu-id="027b9-181">When re-protecting the virtual machine, you can choose a different policy than the policy with which virtual machine was protected initially.</span></span>
>
>

## <a name="unregister-virtual-machines"></a><span data-ttu-id="027b9-182">Zrušit registraci virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="027b9-182">Unregister virtual machines</span></span>
<span data-ttu-id="027b9-183">Pokud chcete odebrat virtuální počítač z trezoru záloh:</span><span class="sxs-lookup"><span data-stu-id="027b9-183">If you want to remove the virtual machine from the backup vault:</span></span>

1. <span data-ttu-id="027b9-184">Klikněte na **UNREGISTER** tlačítko v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="027b9-184">Click on the **UNREGISTER** button at the bottom of the page.</span></span>

    ![Zakažte ochranu](./media/backup-azure-manage-vms/unregister-button.png)

    <span data-ttu-id="027b9-186">Informační zpráva se zobrazí v dolní části obrazovky žádají o potvrzení.</span><span class="sxs-lookup"><span data-stu-id="027b9-186">A toast notification will appear at the bottom of the screen requesting confirmation.</span></span> <span data-ttu-id="027b9-187">Klikněte na tlačítko **Ano** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="027b9-187">Click **YES** to continue.</span></span>

    ![Zakažte ochranu](./media/backup-azure-manage-vms/confirm-unregister.png)

## <a name="delete-backup-data"></a><span data-ttu-id="027b9-189">Odstranění dat zálohy</span><span class="sxs-lookup"><span data-stu-id="027b9-189">Delete Backup data</span></span>
<span data-ttu-id="027b9-190">Můžete odstranit záložní data související s virtuálním počítačem, buď:</span><span class="sxs-lookup"><span data-stu-id="027b9-190">You can delete the backup data associated with a virtual machine, either:</span></span>

* <span data-ttu-id="027b9-191">Během úlohu zastavení ochrany</span><span class="sxs-lookup"><span data-stu-id="027b9-191">During Stop Protection Job</span></span>
* <span data-ttu-id="027b9-192">Po zastavení ochrany dokončení úlohy na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="027b9-192">After a stop protection job is completed on a virtual machine</span></span>

<span data-ttu-id="027b9-193">Odstranit záložní data na virtuálním počítači, který je v *ukončena ochrana* stavu post úspěšné dokončení **zastavení zálohování** úlohy:</span><span class="sxs-lookup"><span data-stu-id="027b9-193">To delete backup data on a virtual machine, which is in the *Protection Stopped* state post successful completion of a **Stop Backup** job:</span></span>

1. <span data-ttu-id="027b9-194">Přejděte na **chráněné položky** a vyberte **virtuální počítač Azure** jako *typ* a klikněte na tlačítko **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="027b9-194">Navigate to the **Protected Items** page and select **Azure Virtual Machine** as *type* and click the **Select** button.</span></span>

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="027b9-196">Vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="027b9-196">Select the virtual machine.</span></span> <span data-ttu-id="027b9-197">Virtuální počítač bude v **ukončena ochrana** stavu.</span><span class="sxs-lookup"><span data-stu-id="027b9-197">The virtual machine will be in **Protection Stopped** state.</span></span>

    ![Zastavení ochrany](./media/backup-azure-manage-vms/protection-stopped-b.png)
3. <span data-ttu-id="027b9-199">Klikněte **odstranit** tlačítko v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="027b9-199">Click the **DELETE** button at the bottom of the page.</span></span>

    ![Odstranit zálohování](./media/backup-azure-manage-vms/delete-backup.png)
4. <span data-ttu-id="027b9-201">V **odstranit záložní data** průvodce, vyberte důvod pro odstraňují se záložní data (důrazně doporučujeme) a klikněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="027b9-201">In the **Delete backup data** wizard, select a reason for deleting backup data (highly recommended) and click **Submit**.</span></span>

    ![Odstranit záložní data](./media/backup-azure-manage-vms/delete-backup-data.png)
5. <span data-ttu-id="027b9-203">Tím se vytvoří úlohu odstranit záložní data vybraný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="027b9-203">This will create a job to delete backup data of selected virtual machine.</span></span> <span data-ttu-id="027b9-204">Klikněte na tlačítko **zobrazit úlohy** zobrazíte odpovídající úlohy na stránce úloh.</span><span class="sxs-lookup"><span data-stu-id="027b9-204">Click **View job** to see corresponding job in Jobs page.</span></span>

    ![Úspěšné odstranění dat](./media/backup-azure-manage-vms/delete-data-success.png)

    <span data-ttu-id="027b9-206">Po dokončení úlohy na záznam odpovídající virtuální počítač se odebere z **chráněné položky** stránky.</span><span class="sxs-lookup"><span data-stu-id="027b9-206">Once the job is completed, the entry corresponding to the virtual machine will be removed from **Protected items** page.</span></span>

## <a name="dashboard"></a><span data-ttu-id="027b9-207">Řídicí panel</span><span class="sxs-lookup"><span data-stu-id="027b9-207">Dashboard</span></span>
<span data-ttu-id="027b9-208">Na **řídicí panel** stránce můžete zkontrolovat informace o virtuálních počítačích Azure, jejich úložiště a úlohy spojené s nimi za posledních 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="027b9-208">On the **Dashboard** page you can review information about Azure virtual machines, their storage, and jobs associated with them in the last 24 hours.</span></span> <span data-ttu-id="027b9-209">Můžete zobrazit stav zálohování a všechny související chyby zálohování.</span><span class="sxs-lookup"><span data-stu-id="027b9-209">You can view backup status and any associated backup errors.</span></span>

![Řídicí panel](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

> [!NOTE]
> <span data-ttu-id="027b9-211">Hodnoty v řídicím panelu se aktualizuje každých 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="027b9-211">Values in the dashboard are refreshed once every 24 hours.</span></span>
>
>

## <a name="auditing-operations"></a><span data-ttu-id="027b9-212">Auditování operací</span><span class="sxs-lookup"><span data-stu-id="027b9-212">Auditing Operations</span></span>
<span data-ttu-id="027b9-213">Azure backup poskytuje kontrolu "operace protokoly" operací zálohování aktivovány zákazníka, což usnadňuje najdete v části přesně byly provedeny jaké operace správy v trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="027b9-213">Azure backup provides review of the "operation logs" of backup operations triggered by the customer making it easy to see exactly what management operations were performed on the backup vault.</span></span> <span data-ttu-id="027b9-214">Protokoly operací povolit skvělé postmortální a auditování podpory u operací zálohování.</span><span class="sxs-lookup"><span data-stu-id="027b9-214">Operations logs enable great post-mortem and audit support for the backup operations.</span></span>

<span data-ttu-id="027b9-215">Protokoly operací přihlášeni následující operace:</span><span class="sxs-lookup"><span data-stu-id="027b9-215">The following operations are logged in Operation logs:</span></span>

* <span data-ttu-id="027b9-216">Registrace</span><span class="sxs-lookup"><span data-stu-id="027b9-216">Register</span></span>
* <span data-ttu-id="027b9-217">Zrušit registraci</span><span class="sxs-lookup"><span data-stu-id="027b9-217">Unregister</span></span>
* <span data-ttu-id="027b9-218">Konfigurace ochrany</span><span class="sxs-lookup"><span data-stu-id="027b9-218">Configure protection</span></span>
* <span data-ttu-id="027b9-219">Zálohování (obě plánované i zálohování na vyžádání prostřednictvím BackupNow)</span><span class="sxs-lookup"><span data-stu-id="027b9-219">Backup ( Both scheduled as well as on-demand backup through BackupNow)</span></span>
* <span data-ttu-id="027b9-220">Obnovení</span><span class="sxs-lookup"><span data-stu-id="027b9-220">Restore</span></span>
* <span data-ttu-id="027b9-221">Zastavení ochrany</span><span class="sxs-lookup"><span data-stu-id="027b9-221">Stop protection</span></span>
* <span data-ttu-id="027b9-222">Odstranit záložní data</span><span class="sxs-lookup"><span data-stu-id="027b9-222">Delete backup data</span></span>
* <span data-ttu-id="027b9-223">Přidání zásad</span><span class="sxs-lookup"><span data-stu-id="027b9-223">Add policy</span></span>
* <span data-ttu-id="027b9-224">Odstranit zásadu</span><span class="sxs-lookup"><span data-stu-id="027b9-224">Delete policy</span></span>
* <span data-ttu-id="027b9-225">Aktualizovat zásady</span><span class="sxs-lookup"><span data-stu-id="027b9-225">Update policy</span></span>
* <span data-ttu-id="027b9-226">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="027b9-226">Cancel job</span></span>

<span data-ttu-id="027b9-227">Chcete-li zobrazit protokoly operací odpovídající do trezoru záloh:</span><span class="sxs-lookup"><span data-stu-id="027b9-227">To view operation logs corresponding to a backup vault:</span></span>

1. <span data-ttu-id="027b9-228">Přejděte na **služby pro** v portálu Azure a pak klikněte na tlačítko **protokoly operací** kartě.</span><span class="sxs-lookup"><span data-stu-id="027b9-228">Navigate to **Management services** in Azure portal, and then click the **Operation Logs** tab.</span></span>

    ![Protokoly operací](./media/backup-azure-manage-vms/ops-logs.png)
2. <span data-ttu-id="027b9-230">Filtry, vyberte **zálohování** jako *typ* a zadejte název úložiště záloh v *název služby* a klikněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="027b9-230">In the filters, select **Backup** as *Type* and specify the backup vault name in *service name* and click on **Submit**.</span></span>

    ![Operace protokoly filtru](./media/backup-azure-manage-vms/ops-logs-filter.png)
3. <span data-ttu-id="027b9-232">V protokolech operací, vyberte všechny operace a klikněte na **podrobnosti** zobrazíte podrobnosti odpovídající operace.</span><span class="sxs-lookup"><span data-stu-id="027b9-232">In the operations logs, select any operation and click  **Details** to see details corresponding to an operation.</span></span>

    ![Podrobnosti operace načítání protokolů](./media/backup-azure-manage-vms/ops-logs-details.png)

    <span data-ttu-id="027b9-234">**Podrobnosti průvodce** obsahuje informace o operaci aktivuje úloha s Id prostředku, na které tato operace se aktivuje a počáteční čas operace.</span><span class="sxs-lookup"><span data-stu-id="027b9-234">The **Details wizard** contains information about the operation triggered, job Id, resource on which this operation is triggered, and start time of the operation.</span></span>

    ![Podrobnosti o operaci](./media/backup-azure-manage-vms/ops-logs-details-window.png)

## <a name="alert-notifications"></a><span data-ttu-id="027b9-236">Oznámení o výstrahách</span><span class="sxs-lookup"><span data-stu-id="027b9-236">Alert notifications</span></span>
<span data-ttu-id="027b9-237">Vlastní oznámení výstrah pro úlohy můžete získat na portálu.</span><span class="sxs-lookup"><span data-stu-id="027b9-237">You can get custom alert notifications for the jobs in portal.</span></span> <span data-ttu-id="027b9-238">Toho dosáhnete tak, že definujete pravidla výstrah pomocí prostředí PowerShell operační protokoly událostí.</span><span class="sxs-lookup"><span data-stu-id="027b9-238">This is achieved by defining PowerShell-based alert rules on operational logs events.</span></span> <span data-ttu-id="027b9-239">Doporučujeme používat *prostředí PowerShell, verze 1.3.0 nebo vyšší*.</span><span class="sxs-lookup"><span data-stu-id="027b9-239">We recommend using *PowerShell version 1.3.0 or above*.</span></span>

<span data-ttu-id="027b9-240">Pokud chcete definovat vlastní oznámení a výstrahy pro selhání zálohování, bude vypadat Ukázkový příkaz:</span><span class="sxs-lookup"><span data-stu-id="027b9-240">To define a custom notification to alert for backup failures, a sample command will look like:</span></span>

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/backupVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/BackupVault/trinadhVault -Actions $actionEmail
```

<span data-ttu-id="027b9-241">**ResourceId**: vám to z protokolů Operations místní podle popisu ve výše části.</span><span class="sxs-lookup"><span data-stu-id="027b9-241">**ResourceId**: You can get this from Operations Logs popup as described in above section.</span></span> <span data-ttu-id="027b9-242">ResourceUri v automaticky otevíraném okně Podrobnosti operace je ResourceId poskytované pro tuto rutinu.</span><span class="sxs-lookup"><span data-stu-id="027b9-242">ResourceUri in details popup window of an operation is the ResourceId to be supplied for this cmdlet.</span></span>

<span data-ttu-id="027b9-243">**OperationName**: to bude mít formát "Microsoft.Backup/backupvault/<EventName>" EventName jsou jedním z registru, Unregister, ConfigureProtection, zálohování, obnovení, StopProtection, DeleteBackupData, CreateProtectionPolicy, DeleteProtectionPolicy, UpdateProtectionPolicy</span><span class="sxs-lookup"><span data-stu-id="027b9-243">**OperationName**: This will be of the format "Microsoft.Backup/backupvault/<EventName>" where EventName is one of Register,Unregister,ConfigureProtection,Backup,Restore,StopProtection,DeleteBackupData,CreateProtectionPolicy,DeleteProtectionPolicy,UpdateProtectionPolicy</span></span>

<span data-ttu-id="027b9-244">**Stav**: podporované hodnoty jsou – Začínáme, úspěšná a se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="027b9-244">**Status**: Supported values are- Started, Succeeded and Failed.</span></span>

<span data-ttu-id="027b9-245">**ResourceGroup**: ResourceGroup prostředku, na kterém se spustí operaci.</span><span class="sxs-lookup"><span data-stu-id="027b9-245">**ResourceGroup**:ResourceGroup of the resource on which operation is triggered.</span></span> <span data-ttu-id="027b9-246">To můžete získat od ResourceId hodnota.</span><span class="sxs-lookup"><span data-stu-id="027b9-246">You can obtain this from ResourceId value.</span></span> <span data-ttu-id="027b9-247">Hodnota mezi poli */resourceGroups/* a */providers/* v ResourceId hodnota je hodnota ResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="027b9-247">Value between fields */resourceGroups/* and */providers/* in ResourceId value is the value for ResourceGroup.</span></span>

<span data-ttu-id="027b9-248">**Název**: název pravidla výstrahy.</span><span class="sxs-lookup"><span data-stu-id="027b9-248">**Name**: Name of the Alert Rule.</span></span>

<span data-ttu-id="027b9-249">**CustomEmail**: Zadejte vlastní e-mailovou adresu, na který chcete odeslat oznámení výstrah</span><span class="sxs-lookup"><span data-stu-id="027b9-249">**CustomEmail**: Specify the custom email address to which you want to send alert notification</span></span>

<span data-ttu-id="027b9-250">**SendToServiceOwners**: tuto možnost odesílá oznámení výstrah pro všechny správce a spolusprávci předplatného.</span><span class="sxs-lookup"><span data-stu-id="027b9-250">**SendToServiceOwners**: This option sends alert notification to all administrators and co-administrators of the subscription.</span></span> <span data-ttu-id="027b9-251">Je možné v **New-AzureRmAlertRuleEmail** rutiny</span><span class="sxs-lookup"><span data-stu-id="027b9-251">It can be used in **New-AzureRmAlertRuleEmail** cmdlet</span></span>

### <a name="limitations-on-alerts"></a><span data-ttu-id="027b9-252">Omezení výstrahy</span><span class="sxs-lookup"><span data-stu-id="027b9-252">Limitations on Alerts</span></span>
<span data-ttu-id="027b9-253">Výstrahy na základě událostí se podrobí následující omezení:</span><span class="sxs-lookup"><span data-stu-id="027b9-253">Event-based alerts are subjected to the following limitations:</span></span>

1. <span data-ttu-id="027b9-254">Výstrahy se spouštějí na všechny virtuální počítače v trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="027b9-254">Alerts are triggered on all virtual machines in the backup vault.</span></span> <span data-ttu-id="027b9-255">Nelze přizpůsobit ho k získání výstrah pro konkrétní sadu virtuálních počítačů v trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="027b9-255">You cannot customize it to get alerts for specific set of virtual machines in a backup vault.</span></span>
2. <span data-ttu-id="027b9-256">Tato funkce je ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="027b9-256">This feature is in Preview.</span></span> [<span data-ttu-id="027b9-257">Další informace</span><span class="sxs-lookup"><span data-stu-id="027b9-257">Learn more</span></span>](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. <span data-ttu-id="027b9-258">Zobrazí se výstrahy z "alerts-noreply@mail.windowsazure.com".</span><span class="sxs-lookup"><span data-stu-id="027b9-258">You will receive alerts from "alerts-noreply@mail.windowsazure.com".</span></span> <span data-ttu-id="027b9-259">Momentálně nelze upravit odesílatelem e-mailu.</span><span class="sxs-lookup"><span data-stu-id="027b9-259">Currently you can't modify the email sender.</span></span>

## <a name="next-steps"></a><span data-ttu-id="027b9-260">Další kroky</span><span class="sxs-lookup"><span data-stu-id="027b9-260">Next steps</span></span>
* [<span data-ttu-id="027b9-261">Obnovení virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="027b9-261">Restore Azure VMs</span></span>](backup-azure-restore-vms.md)
