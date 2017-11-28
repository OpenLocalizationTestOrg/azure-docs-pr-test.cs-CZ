---
title: "aaaBack virtuální počítače Azure | Microsoft Docs"
description: "Zjistit, zaregistrujte a zálohovat trezoru služeb zotavení tooa virtuální počítače Azure."
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
ms.openlocfilehash: a204a42726450a7fd89b5563a786b5070578b113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-virtual-machines-tooa-recovery-services-vault"></a><span data-ttu-id="18973-104">Zálohování virtuálních počítačů Azure trezor služeb zotavení tooa</span><span class="sxs-lookup"><span data-stu-id="18973-104">Back up Azure virtual machines tooa Recovery Services vault</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="18973-105">Zálohování trezor služeb tooRecovery virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="18973-105">Back up VMs tooRecovery Services vault</span></span>](backup-azure-arm-vms.md)
> * [<span data-ttu-id="18973-106">Zálohování virtuálních počítačů tooBackup trezoru</span><span class="sxs-lookup"><span data-stu-id="18973-106">Back up VMs tooBackup vault</span></span>](backup-azure-vms.md)
>
>

<span data-ttu-id="18973-107">Tento článek podrobně popisuje, jak trezoru tooback zálohu virtuálních počítačích Azure (nasazení Resource Manager i nasazení Classic) tooa služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="18973-107">This article details how tooback up Azure VMs (both Resource Manager-deployed and Classic-deployed) tooa Recovery Services vault.</span></span> <span data-ttu-id="18973-108">Většina práce hello pro zálohování virtuálních počítačů je hello přípravy.</span><span class="sxs-lookup"><span data-stu-id="18973-108">Most of hello work for backing up VMs is hello preparation.</span></span> <span data-ttu-id="18973-109">Před zahájením zálohování nebo chránit virtuální počítač, je nutné dokončit hello [požadavky](backup-azure-arm-vms-prepare.md) tooprepare prostředí pro ochranu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="18973-109">Before you can back up or protect a VM, you must complete hello [prerequisites](backup-azure-arm-vms-prepare.md) tooprepare your environment for protecting your VMs.</span></span> <span data-ttu-id="18973-110">Po dokončení hello požadavky, můžete zahájit hello operace zálohování tootake snímky virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="18973-110">Once you have completed hello prerequisites, then you can initiate hello backup operation tootake snapshots of your VM.</span></span>


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

<span data-ttu-id="18973-111">Další informace najdete v tématu články hello na [plánování infrastruktury zálohování virtuálních počítačů v Azure](backup-azure-vms-introduction.md) a [virtuální počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="18973-111">For more information, see hello articles on [planning your VM backup infrastructure in Azure](backup-azure-vms-introduction.md) and [Azure virtual machines](https://azure.microsoft.com/documentation/services/virtual-machines/).</span></span>

## <a name="triggering-hello-backup-job"></a><span data-ttu-id="18973-112">Aktivuje úlohu zálohování hello</span><span class="sxs-lookup"><span data-stu-id="18973-112">Triggering hello backup job</span></span>
<span data-ttu-id="18973-113">zásady zálohování Hello přidružené k trezoru služeb zotavení hello definuje, jak často a kdy běží hello operace zálohování.</span><span class="sxs-lookup"><span data-stu-id="18973-113">hello backup policy associated with hello Recovery Services vault defines how often and when hello backup operation runs.</span></span> <span data-ttu-id="18973-114">Ve výchozím nastavení je prvním plánovaným zálohováním hello hello prvotní zálohování.</span><span class="sxs-lookup"><span data-stu-id="18973-114">By default, hello first scheduled backup is hello initial backup.</span></span> <span data-ttu-id="18973-115">Dokud prvotního zálohování hello hello stav poslední zálohy na hello **úlohy zálohování** ukazovat **upozornění (nedokončené prvotní zálohování)**.</span><span class="sxs-lookup"><span data-stu-id="18973-115">Until hello initial backup occurs, hello Last Backup Status on hello **Backup Jobs** blade shows as **Warning(initial backup pending)**.</span></span>

![Zálohování čeká na zpracování](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

<span data-ttu-id="18973-117">Pokud prvotní zálohování toobegin brzy, doporučujeme spustit **zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="18973-117">Unless your initial backup is due toobegin soon, it is recommended that you run **Back up Now**.</span></span> <span data-ttu-id="18973-118">Hello následující postup spustí z panelu trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="18973-118">hello following procedure starts from hello vault dashboard.</span></span> <span data-ttu-id="18973-119">Tento postup slouží ke spuštění úlohu prvotního zálohování hello po dokončení všech požadavků.</span><span class="sxs-lookup"><span data-stu-id="18973-119">This procedure serves for running hello initial backup job after you have completed all prerequisites.</span></span> <span data-ttu-id="18973-120">Pokud již byl spuštěn hello úlohu prvotního zálohování, tento postup není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="18973-120">If hello initial backup job has already been run, this procedure is not available.</span></span> <span data-ttu-id="18973-121">Hello přidružené zásady zálohování určuje hello další úloha zálohování.</span><span class="sxs-lookup"><span data-stu-id="18973-121">hello associated backup policy determines hello next backup job.</span></span>  

<span data-ttu-id="18973-122">toorun hello úlohu prvotního zálohování:</span><span class="sxs-lookup"><span data-stu-id="18973-122">toorun hello initial backup job:</span></span>

1. <span data-ttu-id="18973-123">Na řídicím panelu trezoru hello, klikněte na číslo hello pod **zálohování položek**, nebo klikněte na tlačítko hello **zálohování položek** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="18973-123">On hello vault dashboard, click hello number under **Backup Items**, or click hello **Backup Items** tile.</span></span> <br/><span data-ttu-id="18973-124">
  ![Ikona nastavení](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)</span><span class="sxs-lookup"><span data-stu-id="18973-124">
![Settings icon](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)</span></span>

  <span data-ttu-id="18973-125">Hello **zálohování položek** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="18973-125">hello **Backup Items** blade opens.</span></span>

  ![Zálohování položek](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. <span data-ttu-id="18973-127">Na hello **zálohování položek** okně, vyberte hello položky.</span><span class="sxs-lookup"><span data-stu-id="18973-127">On hello **Backup Items** blade, select hello item.</span></span>

  ![Ikona nastavení](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  <span data-ttu-id="18973-129">Hello **zálohování položek** seznamu otevře.</span><span class="sxs-lookup"><span data-stu-id="18973-129">hello **Backup Items** list opens.</span></span> <br/>

  ![Aktivovaná úloha zálohování.](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. <span data-ttu-id="18973-131">Na hello **zálohování položek** klikněte na symbol tří teček hello **...**  tooopen hello kontextové nabídky.</span><span class="sxs-lookup"><span data-stu-id="18973-131">On hello **Backup Items** list, click hello ellipses **...** tooopen hello Context menu.</span></span>

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu.png)

  <span data-ttu-id="18973-133">Zobrazí se Hello kontextové nabídky.</span><span class="sxs-lookup"><span data-stu-id="18973-133">hello Context menu appears.</span></span>

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. <span data-ttu-id="18973-135">V místní nabídce hello, klikněte na tlačítko **zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="18973-135">On hello Context menu, click **Backup now**.</span></span>

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  <span data-ttu-id="18973-137">Otevře se okno zálohovat nyní Hello.</span><span class="sxs-lookup"><span data-stu-id="18973-137">hello Backup Now blade opens.</span></span>

  ![Zobrazí okno zálohovat nyní hello](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. <span data-ttu-id="18973-139">Na hello zálohovat nyní okně klikněte na ikonu hello kalendáře, použijte hello kalendáře řízení tooselect hello poslední den tohoto bodu obnovení se zachovává a klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="18973-139">On hello Backup Now blade, click hello calendar icon, use hello calendar control tooselect hello last day this recovery point is retained, and click **Backup**.</span></span>

  ![Sada hello poslední den hello zálohovat nyní bod obnovení je zachován.](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  <span data-ttu-id="18973-141">Nasazení oznámení umožňují vědět byla spuštěna úloha zálohování hello, a že můžete monitorovat průběh hello hello úlohy na stránce úloh zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="18973-141">Deployment notifications let you know hello backup job has been triggered, and that you can monitor hello progress of hello job on hello Backup jobs page.</span></span> <span data-ttu-id="18973-142">V závislosti na velikosti hello vašeho virtuálního počítače vytváření hello prvotní zálohování může chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="18973-142">Depending on hello size of your VM, creating hello initial backup may take a while.</span></span>

6. <span data-ttu-id="18973-143">tooview nebo sledovat stav hello hello prvotní zálohování, na panelu trezoru hello na hello **úlohy zálohování** klikněte na dlaždici **v průběhu**.</span><span class="sxs-lookup"><span data-stu-id="18973-143">tooview or track hello status of hello initial backup, on hello vault dashboard, on hello **Backup Jobs** tile click **In progress**.</span></span>

  ![Dlaždice Úlohy zálohování](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  <span data-ttu-id="18973-145">Otevře se okno úlohy zálohování Hello.</span><span class="sxs-lookup"><span data-stu-id="18973-145">hello Backup Jobs blade opens.</span></span>

  ![Dlaždice Úlohy zálohování](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  <span data-ttu-id="18973-147">V hello **zálohování úloh** okně uvidíte hello stav všech úloh.</span><span class="sxs-lookup"><span data-stu-id="18973-147">In hello **Backup jobs** blade, you can see hello status of all jobs.</span></span> <span data-ttu-id="18973-148">Zkontrolujte, zda hello úloha zálohování pro virtuální počítač stále probíhá, nebo pokud se nedokončí.</span><span class="sxs-lookup"><span data-stu-id="18973-148">Check if hello backup job for your VM is still in progress, or if it has finished.</span></span> <span data-ttu-id="18973-149">Po dokončení úlohy zálohování se stav hello je *dokončeno*.</span><span class="sxs-lookup"><span data-stu-id="18973-149">When a backup job is finished, hello status is *Completed*.</span></span>

  > [!NOTE]
  > <span data-ttu-id="18973-150">Jako součást operace zálohování hello vydá hello služba Azure Backup příkaz toohello rozšířením zálohování v jednotlivých virtuálních počítačů tooflush všech zápisů a pořízení konzistentního snímku.</span><span class="sxs-lookup"><span data-stu-id="18973-150">As a part of hello backup operation, hello Azure Backup service issues a command toohello backup extension in each VM tooflush all writes and take a consistent snapshot.</span></span>
  >
  >

## <a name="troubleshooting-errors"></a><span data-ttu-id="18973-151">Řešení potíží s chybami</span><span class="sxs-lookup"><span data-stu-id="18973-151">Troubleshooting errors</span></span>
<span data-ttu-id="18973-152">Pokud narazíte na problémy při zálohování virtuálního počítače, přečtěte si téma hello [článku Poradce při potížích virtuálních počítačů](backup-azure-vms-troubleshoot.md) nápovědu.</span><span class="sxs-lookup"><span data-stu-id="18973-152">If you run into issues while backing up your virtual machine, see hello [VM troubleshooting article](backup-azure-vms-troubleshoot.md) for help.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18973-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="18973-153">Next steps</span></span>
<span data-ttu-id="18973-154">Teď, když chráníte virtuální počítač, najdete v části hello následující články toolearn o úlohách správy virtuálních počítačů a jak toorestore virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="18973-154">Now that you have protected your VM, see hello following articles toolearn about VM management tasks, and how toorestore VMs.</span></span>

* [<span data-ttu-id="18973-155">Správa a monitorování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="18973-155">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="18973-156">Obnovení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="18973-156">Restore virtual machines</span></span>](backup-azure-arm-restore-vms.md)
