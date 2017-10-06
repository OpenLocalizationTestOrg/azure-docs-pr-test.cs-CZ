---
title: "zálohování virtuálního počítače Azure aaaManage a monitorování | Microsoft Docs"
description: "Zjistěte, jak toomanage a monitorování Azure virtuální počítač zálohy"
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
ms.openlocfilehash: cb95e0b3760c96f7fd563c42cb4c635553f48957
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-common-azure-backup-jobs-and-trigger-alerts-in-hello-classic-portal"></a><span data-ttu-id="4bd98-103">Spravovat běžné úlohy zálohování Azure a aktivační události upozornění v portálu classic hello</span><span class="sxs-lookup"><span data-stu-id="4bd98-103">Manage common Azure Backup jobs and trigger alerts in hello classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4bd98-104">Spravovat zálohy virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="4bd98-104">Manage Azure VM backups</span></span>](backup-azure-manage-vms.md)
> * [<span data-ttu-id="4bd98-105">Spravovat zálohy Classic virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4bd98-105">Manage Classic VM backups</span></span>](backup-azure-manage-vms-classic.md)
>
>

<span data-ttu-id="4bd98-106">Tento článek obsahuje informace o běžné úlohy správy a sledování pro Classic model virtuální počítače chráněné v Azure.</span><span class="sxs-lookup"><span data-stu-id="4bd98-106">This article provides information about common management and monitoring tasks for Classic-model virtual machines protected in Azure.</span></span>  

> [!NOTE]
> <span data-ttu-id="4bd98-107">Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="4bd98-107">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="4bd98-108">V tématu [připravit vaše prostředí tooback zálohu virtuálních počítačích Azure](backup-azure-vms-prepare.md) podrobnosti o práci s klasického nasazení modelu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bd98-108">See [Prepare your environment tooback up Azure virtual machines](backup-azure-vms-prepare.md) for details on working with Classic deployment model VMs.</span></span>
>
> [!IMPORTANT]
><span data-ttu-id="4bd98-109">Od března 2017 se už můžete trezory Backup portálu classic toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-109">Starting March 2017, you can no longer use hello classic portal toocreate Backup vaults.</span></span>
>
> <span data-ttu-id="4bd98-110">Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup.</span><span class="sxs-lookup"><span data-stu-id="4bd98-110">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="4bd98-111">Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="4bd98-111">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="4bd98-112">Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.</span><span class="sxs-lookup"><span data-stu-id="4bd98-112">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="4bd98-113">Po 15 říjen 2017 nemůžete použít trezory Backup toocreate prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4bd98-113">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="4bd98-114">**Do 1. listopadu 2017:**</span><span class="sxs-lookup"><span data-stu-id="4bd98-114">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="4bd98-115">Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.</span><span class="sxs-lookup"><span data-stu-id="4bd98-115">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="4bd98-116">Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-116">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="4bd98-117">Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="4bd98-117">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>

## <a name="manage-protected-virtual-machines"></a><span data-ttu-id="4bd98-118">Správa chráněných virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4bd98-118">Manage protected virtual machines</span></span>
<span data-ttu-id="4bd98-119">toomanage chráněné virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="4bd98-119">toomanage protected virtual machines:</span></span>

1. <span data-ttu-id="4bd98-120">tooview a spravovat nastavení zálohování pro virtuální počítač, klikněte na tlačítko hello **chráněné položky** kartě.</span><span class="sxs-lookup"><span data-stu-id="4bd98-120">tooview and manage backup settings for a virtual machine click hello **Protected Items** tab.</span></span>
2. <span data-ttu-id="4bd98-121">Klikněte na název hello chráněné položky toosee hello **zálohování podrobnosti** kartu, která vám zobrazí informace o hello poslední zálohy.</span><span class="sxs-lookup"><span data-stu-id="4bd98-121">Click on hello name of a protected item toosee hello **Backup Details** tab, which shows you information about hello last backup.</span></span>

    ![Záloha virtuálního počítače](./media/backup-azure-manage-vms/backup-vmdetails.png)
3. <span data-ttu-id="4bd98-123">tooview a spravovat nastavení zásad zálohování pro virtuální počítač, klikněte na tlačítko hello **zásady** kartě.</span><span class="sxs-lookup"><span data-stu-id="4bd98-123">tooview and manage backup policy settings for a virtual machine click hello **Policies** tab.</span></span>

    ![Virtuální počítač zásad](./media/backup-azure-manage-vms/manage-policy-settings.png)

    <span data-ttu-id="4bd98-125">Hello **zásady zálohování** kartě ukazuje hello existující zásady.</span><span class="sxs-lookup"><span data-stu-id="4bd98-125">hello **Backup Policies** tab shows you hello existing policy.</span></span> <span data-ttu-id="4bd98-126">Můžete upravit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="4bd98-126">You can modify as needed.</span></span> <span data-ttu-id="4bd98-127">Pokud potřebujete toocreate nových zásad klikněte na tlačítko **vytvořit** na hello **zásady** stránky.</span><span class="sxs-lookup"><span data-stu-id="4bd98-127">If you need toocreate a new policy click **Create** on hello **Policies** page.</span></span> <span data-ttu-id="4bd98-128">Všimněte si, že pokud chcete, aby tooremove zásady by neměl mít všechny virtuální počítače s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="4bd98-128">Note that if you want tooremove a policy it shouldn't have any virtual machines associated with it.</span></span>

    ![Virtuální počítač zásad](./media/backup-azure-manage-vms/backup-vmpolicy.png)
4. <span data-ttu-id="4bd98-130">Můžete získat další informace o stavu nebo akce pro virtuální počítač na hello **úlohy** stránky.</span><span class="sxs-lookup"><span data-stu-id="4bd98-130">You can get more information about actions or status for a virtual machine on hello **Jobs** page.</span></span> <span data-ttu-id="4bd98-131">Další podrobnosti nebo filtr úlohy pro konkrétní virtuální počítač, klikněte na úlohu v seznamu tooget hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-131">Click a job in hello list tooget more details, or filter jobs for a specific virtual machine.</span></span>

    ![Úlohy](./media/backup-azure-manage-vms/backup-job.png)

## <a name="on-demand-backup-of-a-virtual-machine"></a><span data-ttu-id="4bd98-133">Zálohování na vyžádání virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4bd98-133">On-demand backup of a virtual machine</span></span>
<span data-ttu-id="4bd98-134">Na vyžádání můžete provést zálohování virtuálního počítače, jakmile je nakonfigurován pro ochranu.</span><span class="sxs-lookup"><span data-stu-id="4bd98-134">You can take an on-demand backup of a virtual machine once it is configured for protection.</span></span> <span data-ttu-id="4bd98-135">Pokud hello prvotní zálohování čeká na vyřízení pro hello virtuálního počítače, bude zálohování na vyžádání vytvořit úplnou kopii hello virtuálního počítače v záložním trezoru Azure.</span><span class="sxs-lookup"><span data-stu-id="4bd98-135">If hello initial backup is pending for hello virtual machine, on-demand backup will create a full copy of hello virtual machine in Azure backup vault.</span></span> <span data-ttu-id="4bd98-136">Pokud se první zálohování je dokončeno, bude zálohování na vyžádání jen odeslání změn z předchozí zálohy zálohování tooAzure trezoru tj ho je vždy přírůstkové.</span><span class="sxs-lookup"><span data-stu-id="4bd98-136">If first backup is completed, on-demand backup will only send changes from previous backup tooAzure backup vault i.e. it is always incremental.</span></span>

> [!NOTE]
> <span data-ttu-id="4bd98-137">Rozsah uchování zálohu na vyžádání je nastavena hodnota tooretention zadaná pro denní doba uchování ve toohello odpovídající zásady zálohování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bd98-137">Retention range of an on-demand backup is set tooretention value specified for Daily retention in backup policy corresponding toohello VM.</span></span>  
>
>

<span data-ttu-id="4bd98-138">tootake na vyžádání zálohování virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="4bd98-138">tootake an on-demand backup of a virtual machine:</span></span>

1. <span data-ttu-id="4bd98-139">Přejděte toohello **chráněné položky** a vyberte **virtuální počítač Azure** jako **typ** (Pokud již není vybrána) a klikněte na **vyberte**tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4bd98-139">Navigate toohello **Protected Items** page and select **Azure Virtual Machine** as **Type** (if not already selected) and click on **Select** button.</span></span>

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="4bd98-141">Vyberte hello virtuální počítač, na kterém chcete zálohování tootake na vyžádání a klikněte na **zálohování** tlačítko v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-141">Select hello virtual machine on which you want tootake an on-demand backup and click on **Backup Now** button at hello bottom of hello page.</span></span>

    ![Zálohovat nyní](./media/backup-azure-manage-vms/backup-now.png)

    <span data-ttu-id="4bd98-143">Tím se vytvoří úlohu zálohování na hello vybraný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4bd98-143">This will create a backup job on hello selected virtual machine.</span></span> <span data-ttu-id="4bd98-144">Rozsah uchování bodu obnovení, které jsou vytvořené pomocí této úlohy bude stejné jako příkaz, který je uveden v zásadách hello přidruženého hello virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="4bd98-144">Retention range of recovery point created through this job will be same as that specified in hello policy associated with hello virtual machine.</span></span>

    ![Vytvoření úlohy zálohování](./media/backup-azure-manage-vms/creating-job.png)

   > [!NOTE]
   > <span data-ttu-id="4bd98-146">zásady hello tooview spojené s virtuálním počítačem, procházení dolů do virtuálního počítače v hello **chráněné položky** stránku a přejděte toobackup kartu zásad.</span><span class="sxs-lookup"><span data-stu-id="4bd98-146">tooview hello policy associated with a virtual machine, drill down into virtual machine in hello **Protected Items** page and go toobackup policy tab.</span></span>
   >
   >
3. <span data-ttu-id="4bd98-147">Jakmile bude vytvořena úloha hello, můžete kliknutím na **zobrazit úlohy** tlačítka na hello informační panel toosee hello odpovídající úlohy na stránce úloh hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-147">Once hello job is created, you can click on **View job** button in hello toast bar toosee hello corresponding job in hello jobs page.</span></span>

    ![Vytvořit úlohu zálohování](./media/backup-azure-manage-vms/created-job.png)
4. <span data-ttu-id="4bd98-149">Po úspěšném dokončení úlohy hello bodu obnovení se vytvoří který můžete použít toorestore hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4bd98-149">After successful completion of hello job, a recovery point will be created which you can use toorestore hello virtual machine.</span></span> <span data-ttu-id="4bd98-150">Hodnota sloupce bodu obnovení hello to také se zvýší o 1 v **chráněné položky** stránky.</span><span class="sxs-lookup"><span data-stu-id="4bd98-150">This will also increment hello recovery point column value by 1 in **Protected Items** page.</span></span>

## <a name="stop-protecting-virtual-machines"></a><span data-ttu-id="4bd98-151">Zastavte ochranu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4bd98-151">Stop protecting virtual machines</span></span>
<span data-ttu-id="4bd98-152">Můžete toostop hello budoucí zálohy virtuálního počítače s hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="4bd98-152">You can choose toostop hello future backups of a virtual machine with hello following options:</span></span>

* <span data-ttu-id="4bd98-153">Zachovat zálohovaná data související s virtuálním počítačem v trezoru zálohování Azure</span><span class="sxs-lookup"><span data-stu-id="4bd98-153">Retain backup data associated with virtual machine in Azure Backup vault</span></span>
* <span data-ttu-id="4bd98-154">Odstranit záložní data související s virtuálním počítačem</span><span class="sxs-lookup"><span data-stu-id="4bd98-154">Delete backup data associated with virtual machine</span></span>

<span data-ttu-id="4bd98-155">Pokud jste vybrali tooretain zálohovaná data související s virtuálním počítačem, můžete použít hello zálohovaná data toorestore hello virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4bd98-155">If you have selected tooretain backup data associated with virtual machine, you can use hello backup data toorestore hello virtual machine.</span></span> <span data-ttu-id="4bd98-156">O cenách podrobnosti takových virtuálních počítačů, klikněte na tlačítko [zde](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="4bd98-156">For pricing details for such virtual machines, click [here](https://azure.microsoft.com/pricing/details/backup/).</span></span>

<span data-ttu-id="4bd98-157">tooStop ochranu pro virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="4bd98-157">tooStop protection for a virtual machine:</span></span>

1. <span data-ttu-id="4bd98-158">Přejděte příliš**chráněné položky** a vyberte **virtuální počítač Azure** jako typ filtru hello (Pokud již není vybrána) a klikněte na **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4bd98-158">Navigate too**Protected Items** page and select **Azure virtual machine** as hello filter type (if not already selected) and click on **Select** button.</span></span>

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="4bd98-160">Vyberte hello virtuální počítač a klikněte na **zastavit ochranu** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-160">Select hello virtual machine and click on **Stop Protection** at hello bottom of hello page.</span></span>

    ![Zastavení ochrany](./media/backup-azure-manage-vms/stop-protection.png)
3. <span data-ttu-id="4bd98-162">Ve výchozím nastavení zálohování Azure nedojde k odstranění hello zálohovaná data související s virtuálním počítačem hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-162">By default, Azure Backup doesn’t delete hello backup data associated with hello virtual machine.</span></span>

    ![Potvrďte zastavení ochrany](./media/backup-azure-manage-vms/confirm-stop-protection.png)

    <span data-ttu-id="4bd98-164">Pokud chcete toodelete zálohování dat, zaškrtněte políčko hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-164">If you want toodelete backup data, select hello check box.</span></span>

    ![Zaškrtávací políčko](./media/backup-azure-manage-vms/checkbox.png)

    <span data-ttu-id="4bd98-166">Vyberte důvod zastavení zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-166">Please select a reason for stopping hello backup.</span></span> <span data-ttu-id="4bd98-167">Zatímco tato položka je nepovinná, poskytuje příslušný důvod. bude pomoci toowork Azure Backup na zpětnou vazbu hello a určit jejich prioritu hello zákazníka scénáře.</span><span class="sxs-lookup"><span data-stu-id="4bd98-167">While this is optional, providing a reason will help Azure Backup toowork on hello feedback and prioritize hello customer scenarios.</span></span>
4. <span data-ttu-id="4bd98-168">Klikněte na **odeslání** tlačítko toosubmit hello **zastavit ochranu** úlohy.</span><span class="sxs-lookup"><span data-stu-id="4bd98-168">Click on **Submit** button toosubmit hello **Stop protection** job.</span></span> <span data-ttu-id="4bd98-169">Klikněte na **zobrazit úlohy** toosee hello odpovídající úloha hello v **úlohy** stránky.</span><span class="sxs-lookup"><span data-stu-id="4bd98-169">Click on **View Job** toosee hello corresponding hello job in **Jobs** page.</span></span>

    ![Zastavení ochrany](./media/backup-azure-manage-vms/stop-protect-success.png)

    <span data-ttu-id="4bd98-171">Pokud nevyberete **odstranit související zálohovaná data** možnost během **zastavit ochranu** ochrany průvodce a po dokončení úlohy post, stav se změní příliš**ochrana zastavena**.</span><span class="sxs-lookup"><span data-stu-id="4bd98-171">If you have not selected **Delete associated backup data** option during **Stop Protection** wizard, then post job completion, protection status changes too**Protection Stopped**.</span></span> <span data-ttu-id="4bd98-172">Hello data zůstávají s Azure Backup, dokud je explicitně odstranit.</span><span class="sxs-lookup"><span data-stu-id="4bd98-172">hello data remains with Azure Backup until it is explicitly deleted.</span></span> <span data-ttu-id="4bd98-173">Hello data můžete odstranit vždy výběrem hello virtuálního počítače v hello **chráněné položky** stránky a kliknutím na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="4bd98-173">You can always delete hello data by selecting hello virtual machine in hello **Protected Items** page and clicking **Delete**.</span></span>

    ![Ukončeno ochrany](./media/backup-azure-manage-vms/protection-stopped-status.png)

    <span data-ttu-id="4bd98-175">Pokud jste vybrali hello **odstranit související zálohovaná data** možnost, hello virtuální počítač nebude součástí hello **chráněné položky** stránky.</span><span class="sxs-lookup"><span data-stu-id="4bd98-175">If you have selected hello **Delete associated backup data** option, hello virtual machine won’t be part of hello **Protected Items** page.</span></span>

## <a name="re-protect-virtual-machine"></a><span data-ttu-id="4bd98-176">Znovu nastavit ochranu virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4bd98-176">Re-protect Virtual machine</span></span>
<span data-ttu-id="4bd98-177">Pokud nevyberete hello **odstranění přidružení zálohovaná data** možnost **zastavit ochranu**, můžete znovu nastavit ochranu hello virtuálního počítače pomocí následujících hello kroky podobné toobacking si zaregistrovat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="4bd98-177">If you have not selected hello **Delete associate backup data** option in **Stop Protection**, you can re-protect hello virtual machine by following hello steps similar toobacking up registered virtual machines.</span></span> <span data-ttu-id="4bd98-178">Jakmile chráněný, tento virtuální počítač bude mít zálohovaná data zachována předchozí toostop ochrany a vytvoří body obnovení po se znovu nastavit ochranu.</span><span class="sxs-lookup"><span data-stu-id="4bd98-178">Once protected, this virtual machine will have backup data retained prior toostop protection and recovery points created after re-protect.</span></span>

<span data-ttu-id="4bd98-179">Po znovu nastavit ochranu, stav ochrany hello virtuálního počítače se změní příliš**chráněné** Pokud jsou body obnovení předchozí příliš**zastavit ochranu**.</span><span class="sxs-lookup"><span data-stu-id="4bd98-179">After re-protect, hello virtual machine’s protection status will be changed too**Protected** if there are recovery points prior too**Stop Protection**.</span></span>

  ![Vytvořit ochranu virtuálního počítače](./media/backup-azure-manage-vms/reprotected-status.png)

> [!NOTE]
> <span data-ttu-id="4bd98-181">Při ochranu znovu hello virtuálního počítače, můžete zvolit jinou zásadu než hello zásada, ke které byl virtuální počítač původně chráněný.</span><span class="sxs-lookup"><span data-stu-id="4bd98-181">When re-protecting hello virtual machine, you can choose a different policy than hello policy with which virtual machine was protected initially.</span></span>
>
>

## <a name="unregister-virtual-machines"></a><span data-ttu-id="4bd98-182">Zrušit registraci virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4bd98-182">Unregister virtual machines</span></span>
<span data-ttu-id="4bd98-183">Pokud chcete, aby tooremove hello virtuální počítač z trezoru záloh hello:</span><span class="sxs-lookup"><span data-stu-id="4bd98-183">If you want tooremove hello virtual machine from hello backup vault:</span></span>

1. <span data-ttu-id="4bd98-184">Klikněte na hello **UNREGISTER** tlačítko v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-184">Click on hello **UNREGISTER** button at hello bottom of hello page.</span></span>

    ![Zakažte ochranu](./media/backup-azure-manage-vms/unregister-button.png)

    <span data-ttu-id="4bd98-186">Informační zpráva se zobrazí v hello dolní části obrazovky hello žádají o potvrzení.</span><span class="sxs-lookup"><span data-stu-id="4bd98-186">A toast notification will appear at hello bottom of hello screen requesting confirmation.</span></span> <span data-ttu-id="4bd98-187">Klikněte na tlačítko **Ano** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="4bd98-187">Click **YES** toocontinue.</span></span>

    ![Zakažte ochranu](./media/backup-azure-manage-vms/confirm-unregister.png)

## <a name="delete-backup-data"></a><span data-ttu-id="4bd98-189">Odstranění dat zálohy</span><span class="sxs-lookup"><span data-stu-id="4bd98-189">Delete Backup data</span></span>
<span data-ttu-id="4bd98-190">Odstraněním hello zálohovaná data související s virtuálním počítačem, buď:</span><span class="sxs-lookup"><span data-stu-id="4bd98-190">You can delete hello backup data associated with a virtual machine, either:</span></span>

* <span data-ttu-id="4bd98-191">Během úlohu zastavení ochrany</span><span class="sxs-lookup"><span data-stu-id="4bd98-191">During Stop Protection Job</span></span>
* <span data-ttu-id="4bd98-192">Po zastavení ochrany dokončení úlohy na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="4bd98-192">After a stop protection job is completed on a virtual machine</span></span>

<span data-ttu-id="4bd98-193">toodelete zálohovaná data na virtuálním počítači, který je v hello *ukončena ochrana* stavu post úspěšné dokončení **zastavení zálohování** úlohy:</span><span class="sxs-lookup"><span data-stu-id="4bd98-193">toodelete backup data on a virtual machine, which is in hello *Protection Stopped* state post successful completion of a **Stop Backup** job:</span></span>

1. <span data-ttu-id="4bd98-194">Přejděte toohello **chráněné položky** a vyberte **virtuální počítač Azure** jako *typ* a klikněte na tlačítko hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4bd98-194">Navigate toohello **Protected Items** page and select **Azure Virtual Machine** as *type* and click hello **Select** button.</span></span>

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="4bd98-196">Vyberte virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-196">Select hello virtual machine.</span></span> <span data-ttu-id="4bd98-197">Hello virtuální počítač bude v **ukončena ochrana** stavu.</span><span class="sxs-lookup"><span data-stu-id="4bd98-197">hello virtual machine will be in **Protection Stopped** state.</span></span>

    ![Zastavení ochrany](./media/backup-azure-manage-vms/protection-stopped-b.png)
3. <span data-ttu-id="4bd98-199">Klikněte na tlačítko hello **odstranit** tlačítko v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-199">Click hello **DELETE** button at hello bottom of hello page.</span></span>

    ![Odstranit zálohování](./media/backup-azure-manage-vms/delete-backup.png)
4. <span data-ttu-id="4bd98-201">V hello **odstranit záložní data** průvodce, vyberte důvod pro odstraňují se záložní data (důrazně doporučujeme) a klikněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="4bd98-201">In hello **Delete backup data** wizard, select a reason for deleting backup data (highly recommended) and click **Submit**.</span></span>

    ![Odstranit záložní data](./media/backup-azure-manage-vms/delete-backup-data.png)
5. <span data-ttu-id="4bd98-203">Tím se vytvoří záložní data úlohy toodelete vybraného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4bd98-203">This will create a job toodelete backup data of selected virtual machine.</span></span> <span data-ttu-id="4bd98-204">Klikněte na tlačítko **zobrazit úlohy** toosee odpovídající úlohy na stránce úloh.</span><span class="sxs-lookup"><span data-stu-id="4bd98-204">Click **View job** toosee corresponding job in Jobs page.</span></span>

    ![Úspěšné odstranění dat](./media/backup-azure-manage-vms/delete-data-success.png)

    <span data-ttu-id="4bd98-206">Po dokončení úlohy hello hello záznam odpovídající toohello virtuální počítač se odebere z **chráněné položky** stránky.</span><span class="sxs-lookup"><span data-stu-id="4bd98-206">Once hello job is completed, hello entry corresponding toohello virtual machine will be removed from **Protected items** page.</span></span>

## <a name="dashboard"></a><span data-ttu-id="4bd98-207">Řídicí panel</span><span class="sxs-lookup"><span data-stu-id="4bd98-207">Dashboard</span></span>
<span data-ttu-id="4bd98-208">Na hello **řídicí panel** stránce můžete zkontrolovat informace o virtuálních počítačích Azure, jejich úložiště a úlohy související s nimi v hello posledních 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="4bd98-208">On hello **Dashboard** page you can review information about Azure virtual machines, their storage, and jobs associated with them in hello last 24 hours.</span></span> <span data-ttu-id="4bd98-209">Můžete zobrazit stav zálohování a všechny související chyby zálohování.</span><span class="sxs-lookup"><span data-stu-id="4bd98-209">You can view backup status and any associated backup errors.</span></span>

![Řídicí panel](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

> [!NOTE]
> <span data-ttu-id="4bd98-211">Hodnoty v řídicím panelu hello se aktualizuje každých 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="4bd98-211">Values in hello dashboard are refreshed once every 24 hours.</span></span>
>
>

## <a name="auditing-operations"></a><span data-ttu-id="4bd98-212">Auditování operací</span><span class="sxs-lookup"><span data-stu-id="4bd98-212">Auditing Operations</span></span>
<span data-ttu-id="4bd98-213">Azure backup poskytuje kontrolu hello "protokoly operací" operace zálohování aktivovány hello zákazníků, takže je easy toosee přesně jaké operace správy, které byly provedeny v trezoru záloh hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-213">Azure backup provides review of hello "operation logs" of backup operations triggered by hello customer making it easy toosee exactly what management operations were performed on hello backup vault.</span></span> <span data-ttu-id="4bd98-214">Protokoly operací povolit skvělé postmortální a auditování podpory u operací zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-214">Operations logs enable great post-mortem and audit support for hello backup operations.</span></span>

<span data-ttu-id="4bd98-215">protokoly operací přihlášeni Hello následující operace:</span><span class="sxs-lookup"><span data-stu-id="4bd98-215">hello following operations are logged in Operation logs:</span></span>

* <span data-ttu-id="4bd98-216">Registrace</span><span class="sxs-lookup"><span data-stu-id="4bd98-216">Register</span></span>
* <span data-ttu-id="4bd98-217">Zrušit registraci</span><span class="sxs-lookup"><span data-stu-id="4bd98-217">Unregister</span></span>
* <span data-ttu-id="4bd98-218">Konfigurace ochrany</span><span class="sxs-lookup"><span data-stu-id="4bd98-218">Configure protection</span></span>
* <span data-ttu-id="4bd98-219">Zálohování (obě plánované i zálohování na vyžádání prostřednictvím BackupNow)</span><span class="sxs-lookup"><span data-stu-id="4bd98-219">Backup ( Both scheduled as well as on-demand backup through BackupNow)</span></span>
* <span data-ttu-id="4bd98-220">Obnovení</span><span class="sxs-lookup"><span data-stu-id="4bd98-220">Restore</span></span>
* <span data-ttu-id="4bd98-221">Zastavení ochrany</span><span class="sxs-lookup"><span data-stu-id="4bd98-221">Stop protection</span></span>
* <span data-ttu-id="4bd98-222">Odstranit záložní data</span><span class="sxs-lookup"><span data-stu-id="4bd98-222">Delete backup data</span></span>
* <span data-ttu-id="4bd98-223">Přidání zásad</span><span class="sxs-lookup"><span data-stu-id="4bd98-223">Add policy</span></span>
* <span data-ttu-id="4bd98-224">Odstranit zásadu</span><span class="sxs-lookup"><span data-stu-id="4bd98-224">Delete policy</span></span>
* <span data-ttu-id="4bd98-225">Aktualizovat zásady</span><span class="sxs-lookup"><span data-stu-id="4bd98-225">Update policy</span></span>
* <span data-ttu-id="4bd98-226">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="4bd98-226">Cancel job</span></span>

<span data-ttu-id="4bd98-227">operace tooview protokolů odpovídajícího úložiště záloh tooa:</span><span class="sxs-lookup"><span data-stu-id="4bd98-227">tooview operation logs corresponding tooa backup vault:</span></span>

1. <span data-ttu-id="4bd98-228">Přejděte příliš**služby pro** na portálu Azure a potom klikněte na hello **protokoly operací** kartě.</span><span class="sxs-lookup"><span data-stu-id="4bd98-228">Navigate too**Management services** in Azure portal, and then click hello **Operation Logs** tab.</span></span>

    ![Protokoly operací](./media/backup-azure-manage-vms/ops-logs.png)
2. <span data-ttu-id="4bd98-230">V hello filtry, vyberte **zálohování** jako *typ* a zadejte název úložiště záloh hello v *název služby* a klikněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="4bd98-230">In hello filters, select **Backup** as *Type* and specify hello backup vault name in *service name* and click on **Submit**.</span></span>

    ![Operace protokoly filtru](./media/backup-azure-manage-vms/ops-logs-filter.png)
3. <span data-ttu-id="4bd98-232">V protokolech operací hello, vyberte všechny operace a klikněte na **podrobnosti** toosee podrobnosti odpovídající tooan operaci.</span><span class="sxs-lookup"><span data-stu-id="4bd98-232">In hello operations logs, select any operation and click  **Details** toosee details corresponding tooan operation.</span></span>

    ![Podrobnosti operace načítání protokolů](./media/backup-azure-manage-vms/ops-logs-details.png)

    <span data-ttu-id="4bd98-234">Hello **podrobnosti průvodce** obsahuje informace o aktivaci, operace hello úloha s Id prostředku, na které se aktivuje tuto operaci a čas zahájení operace hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-234">hello **Details wizard** contains information about hello operation triggered, job Id, resource on which this operation is triggered, and start time of hello operation.</span></span>

    ![Podrobnosti o operaci](./media/backup-azure-manage-vms/ops-logs-details-window.png)

## <a name="alert-notifications"></a><span data-ttu-id="4bd98-236">Oznámení o výstrahách</span><span class="sxs-lookup"><span data-stu-id="4bd98-236">Alert notifications</span></span>
<span data-ttu-id="4bd98-237">Vlastní oznámení výstrah pro úlohy hello můžete získat na portálu.</span><span class="sxs-lookup"><span data-stu-id="4bd98-237">You can get custom alert notifications for hello jobs in portal.</span></span> <span data-ttu-id="4bd98-238">Toho dosáhnete tak, že definujete pravidla výstrah pomocí prostředí PowerShell operační protokoly událostí.</span><span class="sxs-lookup"><span data-stu-id="4bd98-238">This is achieved by defining PowerShell-based alert rules on operational logs events.</span></span> <span data-ttu-id="4bd98-239">Doporučujeme používat *prostředí PowerShell, verze 1.3.0 nebo vyšší*.</span><span class="sxs-lookup"><span data-stu-id="4bd98-239">We recommend using *PowerShell version 1.3.0 or above*.</span></span>

<span data-ttu-id="4bd98-240">toodefine tooalert vlastní oznámení selhání zálohování, ukázka příkazu bude vypadat jako:</span><span class="sxs-lookup"><span data-stu-id="4bd98-240">toodefine a custom notification tooalert for backup failures, a sample command will look like:</span></span>

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/backupVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/BackupVault/trinadhVault -Actions $actionEmail
```

<span data-ttu-id="4bd98-241">**ResourceId**: vám to z protokolů Operations místní podle popisu ve výše části.</span><span class="sxs-lookup"><span data-stu-id="4bd98-241">**ResourceId**: You can get this from Operations Logs popup as described in above section.</span></span> <span data-ttu-id="4bd98-242">ResourceUri v automaticky otevíraném okně Podrobnosti operace je toobe ResourceId hello zadaný pro tuto rutinu.</span><span class="sxs-lookup"><span data-stu-id="4bd98-242">ResourceUri in details popup window of an operation is hello ResourceId toobe supplied for this cmdlet.</span></span>

<span data-ttu-id="4bd98-243">**OperationName**: to bude mít formát hello "Microsoft.Backup/backupvault/<EventName>" kde EventName je jedním z registru, Unregister, ConfigureProtection, zálohování, obnovení, StopProtection, DeleteBackupData, UpdateProtectionPolicy CreateProtectionPolicy, DeleteProtectionPolicy,</span><span class="sxs-lookup"><span data-stu-id="4bd98-243">**OperationName**: This will be of hello format "Microsoft.Backup/backupvault/<EventName>" where EventName is one of Register,Unregister,ConfigureProtection,Backup,Restore,StopProtection,DeleteBackupData,CreateProtectionPolicy,DeleteProtectionPolicy,UpdateProtectionPolicy</span></span>

<span data-ttu-id="4bd98-244">**Stav**: podporované hodnoty jsou – Začínáme, úspěšná a se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="4bd98-244">**Status**: Supported values are- Started, Succeeded and Failed.</span></span>

<span data-ttu-id="4bd98-245">**ResourceGroup**: ResourceGroup hello prostředku, na kterém se spustí operaci.</span><span class="sxs-lookup"><span data-stu-id="4bd98-245">**ResourceGroup**:ResourceGroup of hello resource on which operation is triggered.</span></span> <span data-ttu-id="4bd98-246">To můžete získat od ResourceId hodnota.</span><span class="sxs-lookup"><span data-stu-id="4bd98-246">You can obtain this from ResourceId value.</span></span> <span data-ttu-id="4bd98-247">Hodnota mezi poli */resourceGroups/* a */providers/* v ResourceId hodnota je hodnota hello ResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="4bd98-247">Value between fields */resourceGroups/* and */providers/* in ResourceId value is hello value for ResourceGroup.</span></span>

<span data-ttu-id="4bd98-248">**Název**: název hello pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4bd98-248">**Name**: Name of hello Alert Rule.</span></span>

<span data-ttu-id="4bd98-249">**CustomEmail**: Zadejte hello vlastní e-mailové adresy toowhich chcete toosend oznámení výstrah</span><span class="sxs-lookup"><span data-stu-id="4bd98-249">**CustomEmail**: Specify hello custom email address toowhich you want toosend alert notification</span></span>

<span data-ttu-id="4bd98-250">**SendToServiceOwners**: tuto možnost odesílá oznámení výstrah tooall správci a spolusprávci předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-250">**SendToServiceOwners**: This option sends alert notification tooall administrators and co-administrators of hello subscription.</span></span> <span data-ttu-id="4bd98-251">Je možné v **New-AzureRmAlertRuleEmail** rutiny</span><span class="sxs-lookup"><span data-stu-id="4bd98-251">It can be used in **New-AzureRmAlertRuleEmail** cmdlet</span></span>

### <a name="limitations-on-alerts"></a><span data-ttu-id="4bd98-252">Omezení výstrahy</span><span class="sxs-lookup"><span data-stu-id="4bd98-252">Limitations on Alerts</span></span>
<span data-ttu-id="4bd98-253">Na základě událostí výstrahy jsou vystaveno toohello následující omezení:</span><span class="sxs-lookup"><span data-stu-id="4bd98-253">Event-based alerts are subjected toohello following limitations:</span></span>

1. <span data-ttu-id="4bd98-254">Výstrahy se spouštějí na všechny virtuální počítače v trezoru záloh hello.</span><span class="sxs-lookup"><span data-stu-id="4bd98-254">Alerts are triggered on all virtual machines in hello backup vault.</span></span> <span data-ttu-id="4bd98-255">Nelze přizpůsobit ho výstrah tooget pro konkrétní sadu virtuálních počítačů v trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="4bd98-255">You cannot customize it tooget alerts for specific set of virtual machines in a backup vault.</span></span>
2. <span data-ttu-id="4bd98-256">Tato funkce je ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="4bd98-256">This feature is in Preview.</span></span> [<span data-ttu-id="4bd98-257">Další informace</span><span class="sxs-lookup"><span data-stu-id="4bd98-257">Learn more</span></span>](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. <span data-ttu-id="4bd98-258">Zobrazí se výstrahy z "alerts-noreply@mail.windowsazure.com".</span><span class="sxs-lookup"><span data-stu-id="4bd98-258">You will receive alerts from "alerts-noreply@mail.windowsazure.com".</span></span> <span data-ttu-id="4bd98-259">Momentálně nelze upravit hello odesílatelem e-mailu.</span><span class="sxs-lookup"><span data-stu-id="4bd98-259">Currently you can't modify hello email sender.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4bd98-260">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4bd98-260">Next steps</span></span>
* [<span data-ttu-id="4bd98-261">Obnovení virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="4bd98-261">Restore Azure VMs</span></span>](backup-azure-restore-vms.md)
