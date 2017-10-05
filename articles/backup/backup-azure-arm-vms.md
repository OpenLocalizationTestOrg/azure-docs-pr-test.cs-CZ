---
title: "Zálohování virtuálních počítačů Azure | Microsoft Docs"
description: "Zjistit, zaregistrujte a zálohování virtuálních počítačů Azure do trezoru služeb zotavení."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "zálohování virtuálních počítačů; zálohování virtuálního počítače; zálohování a zotavení po havárii; zálohování virtuálních počítačů arm"
ms.assetid: 5c68481d-7be3-4e68-b87c-0961c267053e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40983a3de104238d09b976b5fcf2419da42c1bba
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="back-up-azure-virtual-machines-to-a-recovery-services-vault"></a><span data-ttu-id="28318-104">Zálohování virtuálních počítačů Azure do trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="28318-104">Back up Azure virtual machines to a Recovery Services vault</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="28318-105">Zálohování virtuálních počítačů do trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="28318-105">Back up VMs to Recovery Services vault</span></span>](backup-azure-arm-vms.md)
> * [<span data-ttu-id="28318-106">Zálohování virtuálních počítačů do trezoru zálohování</span><span class="sxs-lookup"><span data-stu-id="28318-106">Back up VMs to Backup vault</span></span>](backup-azure-vms.md)
>
>

<span data-ttu-id="28318-107">Tento článek podrobné informace o tom, jak zálohovat virtuální počítače Azure (nasazení Resource Manager i nasazení Classic) do trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="28318-107">This article details how to back up Azure VMs (both Resource Manager-deployed and Classic-deployed) to a Recovery Services vault.</span></span> <span data-ttu-id="28318-108">Většina práce pro zálohování virtuálních počítačů je přípravy.</span><span class="sxs-lookup"><span data-stu-id="28318-108">Most of the work for backing up VMs is the preparation.</span></span> <span data-ttu-id="28318-109">Před zahájením zálohování nebo chránit virtuální počítač, musíte provést [požadavky](backup-azure-arm-vms-prepare.md) při přípravě svého prostředí pro ochranu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="28318-109">Before you can back up or protect a VM, you must complete the [prerequisites](backup-azure-arm-vms-prepare.md) to prepare your environment for protecting your VMs.</span></span> <span data-ttu-id="28318-110">Po dokončení požadavky, můžete spustit zálohování udělat snímky virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="28318-110">Once you have completed the prerequisites, then you can initiate the backup operation to take snapshots of your VM.</span></span>


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

<span data-ttu-id="28318-111">Další informace najdete v článcích na [plánování infrastruktury zálohování virtuálních počítačů v Azure](backup-azure-vms-introduction.md) a [virtuální počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="28318-111">For more information, see the articles on [planning your VM backup infrastructure in Azure](backup-azure-vms-introduction.md) and [Azure virtual machines](https://azure.microsoft.com/documentation/services/virtual-machines/).</span></span>

## <a name="triggering-the-backup-job"></a><span data-ttu-id="28318-112">Spuštění úlohy zálohování</span><span class="sxs-lookup"><span data-stu-id="28318-112">Triggering the backup job</span></span>
<span data-ttu-id="28318-113">Zásady zálohování, které jsou přidružené k trezoru služeb zotavení definuje, jak často a kdy se spustí operace zálohování.</span><span class="sxs-lookup"><span data-stu-id="28318-113">The backup policy associated with the Recovery Services vault defines how often and when the backup operation runs.</span></span> <span data-ttu-id="28318-114">Ve výchozím nastavení je prvním plánovaným zálohováním prvotní zálohování.</span><span class="sxs-lookup"><span data-stu-id="28318-114">By default, the first scheduled backup is the initial backup.</span></span> <span data-ttu-id="28318-115">Než proběhne prvotní zálohování, bude Stav poslední zálohy v okně **Úlohy zálohování** ukazovat **Upozornění (nedokončené prvotní zálohování)**.</span><span class="sxs-lookup"><span data-stu-id="28318-115">Until the initial backup occurs, the Last Backup Status on the **Backup Jobs** blade shows as **Warning(initial backup pending)**.</span></span>

![Zálohování čeká na zpracování](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

<span data-ttu-id="28318-117">Pokud nemá prvotní zálohování brzy začít, doporučujeme spustit **Zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="28318-117">Unless your initial backup is due to begin soon, it is recommended that you run **Back up Now**.</span></span> <span data-ttu-id="28318-118">Následující postup se spustí na řídicím panelu trezoru.</span><span class="sxs-lookup"><span data-stu-id="28318-118">The following procedure starts from the vault dashboard.</span></span> <span data-ttu-id="28318-119">Tento postup slouží ke spuštění úlohu prvotního zálohování po dokončení všech požadavků.</span><span class="sxs-lookup"><span data-stu-id="28318-119">This procedure serves for running the initial backup job after you have completed all prerequisites.</span></span> <span data-ttu-id="28318-120">Pokud již byl spuštěn úlohu prvotního zálohování, tento postup není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="28318-120">If the initial backup job has already been run, this procedure is not available.</span></span> <span data-ttu-id="28318-121">Přidružené zásady zálohování Určuje další úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="28318-121">The associated backup policy determines the next backup job.</span></span>  

<span data-ttu-id="28318-122">Spuštění úlohy prvotního zálohování:</span><span class="sxs-lookup"><span data-stu-id="28318-122">To run the initial backup job:</span></span>

1. <span data-ttu-id="28318-123">Na řídicím panelu trezoru klikněte na číslo pod záznamem **Zálohování položek** nebo klikněte na dlaždici **Zálohování položek**.</span><span class="sxs-lookup"><span data-stu-id="28318-123">On the vault dashboard, click the number under **Backup Items**, or click the **Backup Items** tile.</span></span> <br/><span data-ttu-id="28318-124">
  ![Ikona nastavení](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)</span><span class="sxs-lookup"><span data-stu-id="28318-124">
![Settings icon](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)</span></span>

  <span data-ttu-id="28318-125">Otevře se okno **Zálohování položek**</span><span class="sxs-lookup"><span data-stu-id="28318-125">The **Backup Items** blade opens.</span></span>

  ![Zálohování položek](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. <span data-ttu-id="28318-127">V okně **Zálohování položek** vyberte položku.</span><span class="sxs-lookup"><span data-stu-id="28318-127">On the **Backup Items** blade, select the item.</span></span>

  ![Ikona nastavení](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  <span data-ttu-id="28318-129">Otevře se seznam **Zálohování položek**.</span><span class="sxs-lookup"><span data-stu-id="28318-129">The **Backup Items** list opens.</span></span> <br/>

  ![Aktivovaná úloha zálohování.](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. <span data-ttu-id="28318-131">V seznamu **Zálohování položek** klikněte na **...** (tři tečky) a otevřete místní nabídku.</span><span class="sxs-lookup"><span data-stu-id="28318-131">On the **Backup Items** list, click the ellipses **...** to open the Context menu.</span></span>

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu.png)

  <span data-ttu-id="28318-133">Zobrazí se místní nabídka.</span><span class="sxs-lookup"><span data-stu-id="28318-133">The Context menu appears.</span></span>

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. <span data-ttu-id="28318-135">V místní nabídce klikněte na **Zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="28318-135">On the Context menu, click **Backup now**.</span></span>

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  <span data-ttu-id="28318-137">Otevře se okno Zálohovat nyní.</span><span class="sxs-lookup"><span data-stu-id="28318-137">The Backup Now blade opens.</span></span>

  ![ukazuje okno Zálohovat nyní](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. <span data-ttu-id="28318-139">V okně Zálohovat nyní klikněte na ikonu kalendáře, pomocí ovládacího prvku kalendáře vyberte poslední den uchování tohoto bodu obnovení a klikněte na **Zálohovat**.</span><span class="sxs-lookup"><span data-stu-id="28318-139">On the Backup Now blade, click the calendar icon, use the calendar control to select the last day this recovery point is retained, and click **Backup**.</span></span>

  ![nastavte poslední den uchování bodu obnovení vytvořeného pomocí možnosti Zálohovat nyní](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  <span data-ttu-id="28318-141">Oznámení nasazení vás budou informovat o aktivaci úlohy zálohování a možnosti sledovat průběh úlohy na stránce Úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="28318-141">Deployment notifications let you know the backup job has been triggered, and that you can monitor the progress of the job on the Backup jobs page.</span></span> <span data-ttu-id="28318-142">V závislosti na velikosti virtuálního počítače může vytváření prvotní zálohy chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="28318-142">Depending on the size of your VM, creating the initial backup may take a while.</span></span>

6. <span data-ttu-id="28318-143">Pokud chcete zobrazit nebo sledovat stav prvotní zálohy, na řídicím panelu trezoru na dlaždici **Úlohy zálohování** klikněte na **Probíhající**.</span><span class="sxs-lookup"><span data-stu-id="28318-143">To view or track the status of the initial backup, on the vault dashboard, on the **Backup Jobs** tile click **In progress**.</span></span>

  ![Dlaždice Úlohy zálohování](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  <span data-ttu-id="28318-145">Otevře se okno Úlohy zálohování</span><span class="sxs-lookup"><span data-stu-id="28318-145">The Backup Jobs blade opens.</span></span>

  ![Dlaždice Úlohy zálohování](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  <span data-ttu-id="28318-147">V okně **Úlohy zálohování** vidíte stav všech úloh.</span><span class="sxs-lookup"><span data-stu-id="28318-147">In the **Backup jobs** blade, you can see the status of all jobs.</span></span> <span data-ttu-id="28318-148">Zkontrolujte, jestli úloha zálohování vašeho virtuálního počítače stále probíhá, nebo se dokončila.</span><span class="sxs-lookup"><span data-stu-id="28318-148">Check if the backup job for your VM is still in progress, or if it has finished.</span></span> <span data-ttu-id="28318-149">Po dokončení úlohy zálohování se stav změní na *Dokončeno*.</span><span class="sxs-lookup"><span data-stu-id="28318-149">When a backup job is finished, the status is *Completed*.</span></span>

  > [!NOTE]
  > <span data-ttu-id="28318-150">Jako součást operace zálohování vydá služba Azure Backup rozšířením zálohování v každém virtuálním počítači příkaz k vyprázdnění všech zápisů a pořízení konzistentního snímku.</span><span class="sxs-lookup"><span data-stu-id="28318-150">As a part of the backup operation, the Azure Backup service issues a command to the backup extension in each VM to flush all writes and take a consistent snapshot.</span></span>
  >
  >

## <a name="troubleshooting-errors"></a><span data-ttu-id="28318-151">Řešení potíží s chybami</span><span class="sxs-lookup"><span data-stu-id="28318-151">Troubleshooting errors</span></span>
<span data-ttu-id="28318-152">Pokud narazíte na problémy při zálohování virtuálního počítače, přečtěte si téma [článku Poradce při potížích virtuálních počítačů](backup-azure-vms-troubleshoot.md) nápovědu.</span><span class="sxs-lookup"><span data-stu-id="28318-152">If you run into issues while backing up your virtual machine, see the [VM troubleshooting article](backup-azure-vms-troubleshoot.md) for help.</span></span>

## <a name="next-steps"></a><span data-ttu-id="28318-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="28318-153">Next steps</span></span>
<span data-ttu-id="28318-154">Teď, když chráníte virtuální počítač, najdete v následujících článcích Další informace o úlohách správy virtuálních počítačů a obnovení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="28318-154">Now that you have protected your VM, see the following articles to learn about VM management tasks, and how to restore VMs.</span></span>

* [<span data-ttu-id="28318-155">Správa a monitorování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="28318-155">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="28318-156">Obnovení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="28318-156">Restore virtual machines</span></span>](backup-azure-arm-restore-vms.md)
