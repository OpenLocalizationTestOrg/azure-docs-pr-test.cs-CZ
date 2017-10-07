---
title: "zálohy virtuálních počítačů nasazených Resource Managerem aaaManage | Microsoft Docs"
description: "Zjistěte, jak toomanage a monitorování záloh virtuálních počítačů nasazených Resource Managerem"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: f3050283-d60f-472d-b464-cb844e70d67e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: trinadhk;markgal
ms.openlocfilehash: 241fc4b2a29c5cd8b8b0ee8efbf8ba4e96c6a7ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-machine-backups"></a><span data-ttu-id="14c54-103">Správa záloh virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="14c54-103">Manage Azure virtual machine backups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="14c54-104">Spravovat zálohy virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="14c54-104">Manage Azure VM backups</span></span>](backup-azure-manage-vms.md)
> * [<span data-ttu-id="14c54-105">Spravovat zálohy Classic virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="14c54-105">Manage Classic VM backups</span></span>](backup-azure-manage-vms-classic.md)
>
>

<span data-ttu-id="14c54-106">Tento článek obsahuje pokyny týkající se správy zálohování virtuálních počítačů a vysvětluje hello výstrahy zálohy informace k dispozici v hello řídicí panel portálu.</span><span class="sxs-lookup"><span data-stu-id="14c54-106">This article provides guidance on managing VM backups, and explains hello backup alerts information available in hello portal dashboard.</span></span> <span data-ttu-id="14c54-107">Hello pokyny v tomto článku se vztahuje toousing virtuálních počítačů s trezory služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="14c54-107">hello guidance in this article applies toousing VMs with Recovery Services vaults.</span></span> <span data-ttu-id="14c54-108">Tento článek nepopisuje hello vytváření virtuálních počítačů, ani se vysvětluje jak tooprotect virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="14c54-108">This article does not cover hello creation of virtual machines, nor does it explain how tooprotect virtual machines.</span></span> <span data-ttu-id="14c54-109">Úvod do o ochraně nasazení Azure Resource Manager virtuálních počítačů v Azure k trezoru služeb zotavení, najdete v části [první pohled: zálohování virtuálních počítačů tooa trezor služeb zotavení](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="14c54-109">For a primer on protecting Azure Resource Manager-deployed VMs in Azure with a Recovery Services vault, see [First look: Back up VMs tooa Recovery Services vault](backup-azure-vms-first-look-arm.md).</span></span>

## <a name="manage-vaults-and-protected-virtual-machines"></a><span data-ttu-id="14c54-110">Správa úložišť a chráněné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="14c54-110">Manage vaults and protected virtual machines</span></span>
<span data-ttu-id="14c54-111">V hello portál Azure poskytuje panelu trezoru služeb zotavení hello přístup tooinformation o hello trezoru včetně:</span><span class="sxs-lookup"><span data-stu-id="14c54-111">In hello Azure portal, hello Recovery Services vault dashboard provides access tooinformation about hello vault including:</span></span>

* <span data-ttu-id="14c54-112">Hello poslední snímek zálohy, který je taky nejnovější bod obnovení hello < br\></span><span class="sxs-lookup"><span data-stu-id="14c54-112">hello most recent backup snapshot, which is also hello latest restore point <br\></span></span>
* <span data-ttu-id="14c54-113">zásady zálohování Hello < br\></span><span class="sxs-lookup"><span data-stu-id="14c54-113">hello backup policy <br\></span></span>
* <span data-ttu-id="14c54-114">Celková velikost všech snímků zálohy < br\></span><span class="sxs-lookup"><span data-stu-id="14c54-114">total size of all backup snapshots <br\></span></span>
* <span data-ttu-id="14c54-115">počet virtuálních počítačů, které jsou chráněné službou hello trezoru < br\></span><span class="sxs-lookup"><span data-stu-id="14c54-115">number of virtual machines that are protected with hello vault <br\></span></span>

<span data-ttu-id="14c54-116">Mnoho úlohy správy počítačů s zálohování virtuálních počítačů začínat otevírání hello trezoru hello řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="14c54-116">Many management tasks with a virtual machine backup begin with opening hello vault in hello dashboard.</span></span> <span data-ttu-id="14c54-117">Ale vzhledem k trezory můžou být použité tooprotect více položek (nebo víc virtuálních počítačů), tooview podrobnosti o konkrétní virtuální počítač, otevřete hello trezoru položek řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="14c54-117">However, because vaults can be used tooprotect multiple items (or multiple VMs), tooview details about a particular VM, open hello vault item dashboard.</span></span> <span data-ttu-id="14c54-118">Hello následující postup ukazuje, jak tooopen hello *panelu trezoru* a pak pokračujte toohello *položky panelu trezoru*.</span><span class="sxs-lookup"><span data-stu-id="14c54-118">hello following procedure shows you how tooopen hello *vault dashboard* and then continue toohello *vault item dashboard*.</span></span> <span data-ttu-id="14c54-119">V obou postupy, které bod jak tooadd hello trezoru a trezoru toohello položky řídicí panel Azure pomocí příkazu toodashboard hello kódu Pin je "tipy".</span><span class="sxs-lookup"><span data-stu-id="14c54-119">There are "tips" in both procedures that point out how tooadd hello vault and vault item toohello Azure dashboard by using hello Pin toodashboard command.</span></span> <span data-ttu-id="14c54-120">Toodashboard kódu PIN je způsob vytvoření trezoru toohello zástupce nebo položky.</span><span class="sxs-lookup"><span data-stu-id="14c54-120">Pin toodashboard is a way of creating a shortcut toohello vault or item.</span></span> <span data-ttu-id="14c54-121">Běžné příkazy můžete spustit také z hello zástupce.</span><span class="sxs-lookup"><span data-stu-id="14c54-121">You can also execute common commands from hello shortcut.</span></span>

> [!TIP]
> <span data-ttu-id="14c54-122">Pokud máte více řídicí panely a otevřete oken, pomocí posuvníku světlý blue hello dole hello hello okno tooslide hello řídicí panel Azure a zpět.</span><span class="sxs-lookup"><span data-stu-id="14c54-122">If you have multiple dashboards and blades open, use hello dark-blue slider at hello bottom of hello window tooslide hello Azure dashboard back and forth.</span></span>
>
>

![Úplné zobrazení s posuvníku](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-hello-dashboard"></a><span data-ttu-id="14c54-124">Otevřete hello řídicím panelu trezoru služeb zotavení:</span><span class="sxs-lookup"><span data-stu-id="14c54-124">Open a Recovery Services vault in hello dashboard:</span></span>
1. <span data-ttu-id="14c54-125">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="14c54-125">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="14c54-126">V nabídce centra hello, klikněte na tlačítko **Procházet** a v hello seznamu prostředků zadejte **služeb zotavení**.</span><span class="sxs-lookup"><span data-stu-id="14c54-126">On hello Hub menu, click **Browse** and in hello list of resources, type **Recovery Services**.</span></span> <span data-ttu-id="14c54-127">Během zadávání hello seznamu filtrů podle vašeho zadání.</span><span class="sxs-lookup"><span data-stu-id="14c54-127">As you begin typing, hello list filters based on your input.</span></span> <span data-ttu-id="14c54-128">Klikněte na **Trezor Recovery Services**.</span><span class="sxs-lookup"><span data-stu-id="14c54-128">Click **Recovery Services vault**.</span></span>

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-manage-vms/browse-to-rs-vaults.png) <br/>

    <span data-ttu-id="14c54-130">Zobrazí se Hello seznamu trezorů služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="14c54-130">hello list of Recovery Services vaults are displayed.</span></span>

    ![<span data-ttu-id="14c54-131">Trezory seznam služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="14c54-131">List of Recovery Services vaults</span></span> ](./media/backup-azure-manage-vms/list-o-vaults.png) <br/>

   > [!TIP]
   > <span data-ttu-id="14c54-132">Pokud připnete trezoru toohello řídicího panelu Azure, je tento trezor okamžitě dostupné při otevření hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="14c54-132">If you pin a vault toohello Azure Dashboard, that vault is immediately accessible when you open hello Azure portal.</span></span> <span data-ttu-id="14c54-133">toopin trezoru toohello řídicího panelu, v seznamu hello trezoru, klikněte pravým tlačítkem na hello trezoru a vyberte **Pin toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="14c54-133">toopin a vault toohello dashboard, in hello vault list, right-click hello vault, and select **Pin toodashboard**.</span></span>
   >
   >
3. <span data-ttu-id="14c54-134">Hello seznamu trezorů vyberte trezor tooopen hello jeho řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="14c54-134">From hello list of vaults, select hello vault tooopen its dashboard.</span></span> <span data-ttu-id="14c54-135">Když vyberete hello trezoru, hello panelu trezoru a hello **nastavení** okno otevřít.</span><span class="sxs-lookup"><span data-stu-id="14c54-135">When you select hello vault, hello vault dashboard and hello **Settings** blade open.</span></span> <span data-ttu-id="14c54-136">V hello následující bitové kopie, hello **Contoso trezoru** zvýrazní řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="14c54-136">In hello following image, hello **Contoso-vault** dashboard is highlighted.</span></span>

    ![Otevření panelu trezoru a okna nastavení](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a><span data-ttu-id="14c54-138">Otevřete položku řídicího panelu trezoru</span><span class="sxs-lookup"><span data-stu-id="14c54-138">Open a vault item dashboard</span></span>
<span data-ttu-id="14c54-139">V předchozím postupu hello otevřít hello panelu trezoru.</span><span class="sxs-lookup"><span data-stu-id="14c54-139">In hello previous procedure you opened hello vault dashboard.</span></span> <span data-ttu-id="14c54-140">tooopen hello trezoru položek řídicího panelu:</span><span class="sxs-lookup"><span data-stu-id="14c54-140">tooopen hello vault item dashboard:</span></span>

1. <span data-ttu-id="14c54-141">V panelu trezoru hello na hello **zálohování položek** dlaždici, klikněte na tlačítko **virtuální počítače Azure**.</span><span class="sxs-lookup"><span data-stu-id="14c54-141">In hello vault dashboard, on hello **Backup Items** tile, click **Azure Virtual Machines**.</span></span>

    ![Otevřete položky zálohování dlaždice](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    <span data-ttu-id="14c54-143">Hello **položky zálohování** okno uvádí hello poslední úloha zálohování pro každou položku.</span><span class="sxs-lookup"><span data-stu-id="14c54-143">hello **Backup Items** blade lists hello last backup job for each item.</span></span> <span data-ttu-id="14c54-144">V tomto příkladu je jeden virtuální počítač, demovm-markgal chráněn tento trezor.</span><span class="sxs-lookup"><span data-stu-id="14c54-144">In this example, there is one virtual machine, demovm-markgal, protected by this vault.</span></span>  

    ![Dlaždice položky zálohování](./media/backup-azure-manage-vms/backup-items-blade.png)

   > [!TIP]
   > <span data-ttu-id="14c54-146">Pro usnadnění přístupu budete moct připnout trezoru položky toohello řídicího panelu Azure.</span><span class="sxs-lookup"><span data-stu-id="14c54-146">For ease of access, you can pin a vault item toohello Azure Dashboard.</span></span> <span data-ttu-id="14c54-147">toopin položku trezoru v seznamu položek hello trezoru, klikněte pravým tlačítkem na položku hello a vyberte **Pin toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="14c54-147">toopin a vault item, in hello vault item list, right-click hello item and select **Pin toodashboard**.</span></span>
   >
   >
2. <span data-ttu-id="14c54-148">V hello **zálohování položek** okně klikněte na tlačítko hello položky tooopen hello trezoru položek řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="14c54-148">In hello **Backup Items** blade, click hello item tooopen hello vault item dashboard.</span></span>

    ![Dlaždice položky zálohování](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    <span data-ttu-id="14c54-150">Hello trezoru položek řídicího panelu a jeho **nastavení** okno otevřít.</span><span class="sxs-lookup"><span data-stu-id="14c54-150">hello vault item dashboard and its **Settings** blade open.</span></span>

    ![Řídicí panel položky zálohování se okno nastavení](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    <span data-ttu-id="14c54-152">Z řídicího panelu trezoru položky hello můžete provést celou řadu úloh správy klíčů, jako například:</span><span class="sxs-lookup"><span data-stu-id="14c54-152">From hello vault item dashboard, you can accomplish many key management tasks, such as:</span></span>

   * <span data-ttu-id="14c54-153">změnit zásady nebo vytvořte nové zásady zálohování < br\></span><span class="sxs-lookup"><span data-stu-id="14c54-153">change policies or create a new backup policy<br\></span></span>
   * <span data-ttu-id="14c54-154">Zobrazit body obnovení a zobrazí jejich stavu konzistence < br\></span><span class="sxs-lookup"><span data-stu-id="14c54-154">view restore points, and see their consistency state <br\></span></span>
   * <span data-ttu-id="14c54-155">zálohování na vyžádání virtuálního počítače < br\></span><span class="sxs-lookup"><span data-stu-id="14c54-155">on-demand backup of a virtual machine <br\></span></span>
   * <span data-ttu-id="14c54-156">Zastavte ochranu virtuálních počítačů < br\></span><span class="sxs-lookup"><span data-stu-id="14c54-156">stop protecting virtual machines <br\></span></span>
   * <span data-ttu-id="14c54-157">Obnovte ochranu virtuálního počítače < br\></span><span class="sxs-lookup"><span data-stu-id="14c54-157">resume protection of a virtual machine <br\></span></span>
   * <span data-ttu-id="14c54-158">odstranit záložní data (nebo bodu obnovení) < br\></span><span class="sxs-lookup"><span data-stu-id="14c54-158">delete a backup data (or recovery point) <br\></span></span>
   * <span data-ttu-id="14c54-159">[obnovit záložní disky](backup-azure-arm-restore-vms.md#restore-backed-up-disks) < br\></span><span class="sxs-lookup"><span data-stu-id="14c54-159">[restore backup disks](backup-azure-arm-restore-vms.md#restore-backed-up-disks)  <br\></span></span>

<span data-ttu-id="14c54-160">Pro hello následující postupy je výchozí bod hello hello trezoru položek řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="14c54-160">For hello following procedures, hello starting point is hello vault item dashboard.</span></span>

## <a name="manage-backup-policies"></a><span data-ttu-id="14c54-161">Správa zásad zálohování</span><span class="sxs-lookup"><span data-stu-id="14c54-161">Manage backup policies</span></span>
1. <span data-ttu-id="14c54-162">Na hello [položky panelu trezoru](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikněte na tlačítko **všechna nastavení** tooopen hello **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="14c54-162">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **All Settings** tooopen hello **Settings** blade.</span></span>

    ![Okno zásady zálohování](./media/backup-azure-manage-vms/all-settings-button.png)
2. <span data-ttu-id="14c54-164">Na hello **nastavení** okně klikněte na tlačítko **zálohování zásad** tooopen tohoto okna.</span><span class="sxs-lookup"><span data-stu-id="14c54-164">On hello **Settings** blade, click **Backup policy** tooopen that blade.</span></span>

    <span data-ttu-id="14c54-165">V okně hello jsou uvedeny hello zálohování četnost a uchování podrobnosti rozsahu.</span><span class="sxs-lookup"><span data-stu-id="14c54-165">On hello blade, hello backup frequency and retention range details are shown.</span></span>

    ![Okno zásady zálohování](./media/backup-azure-manage-vms/backup-policy-blade.png)
3. <span data-ttu-id="14c54-167">Z hello **vyberte zásady zálohování** nabídky:</span><span class="sxs-lookup"><span data-stu-id="14c54-167">From hello **Choose backup policy** menu:</span></span>

   * <span data-ttu-id="14c54-168">Vyberte jinou zásadu toochange zásady a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="14c54-168">toochange policies, select a different policy and click **Save**.</span></span> <span data-ttu-id="14c54-169">nové zásady Hello je okamžitě použité toohello trezoru.</span><span class="sxs-lookup"><span data-stu-id="14c54-169">hello new policy is immediately applied toohello vault.</span></span> <span data-ttu-id="14c54-170">< br\></span><span class="sxs-lookup"><span data-stu-id="14c54-170"><br\></span></span>
   * <span data-ttu-id="14c54-171">Vyberte zásadu, toocreate **vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="14c54-171">toocreate a policy, select **Create New**.</span></span>

     ![Záloha virtuálního počítače](./media/backup-azure-manage-vms/backup-policy-create-new.png)

     <span data-ttu-id="14c54-173">Pokyny pro vytvoření zásady zálohování, naleznete v části [definování zásad zálohování](backup-azure-manage-vms.md#defining-a-backup-policy).</span><span class="sxs-lookup"><span data-stu-id="14c54-173">For instructions on creating a backup policy, see [Defining a backup policy](backup-azure-manage-vms.md#defining-a-backup-policy).</span></span>

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

> [!NOTE]
> <span data-ttu-id="14c54-174">Při správě zásady zálohování, ujistěte se, zda text hello toofollow [osvědčené postupy](backup-azure-vms-introduction.md#best-practices) pro optimální výkon zálohování</span><span class="sxs-lookup"><span data-stu-id="14c54-174">While managing backup policies, make sure toofollow hello [best practices](backup-azure-vms-introduction.md#best-practices) for optimal backup performance</span></span>
>
>

## <a name="on-demand-backup-of-a-virtual-machine"></a><span data-ttu-id="14c54-175">Zálohování na vyžádání virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="14c54-175">On-demand backup of a virtual machine</span></span>
<span data-ttu-id="14c54-176">Na vyžádání můžete provést zálohování virtuálního počítače, jakmile je nakonfigurován pro ochranu.</span><span class="sxs-lookup"><span data-stu-id="14c54-176">You can take an on-demand backup of a virtual machine once it is configured for protection.</span></span> <span data-ttu-id="14c54-177">Pokud probíhá hello prvotní zálohování, zálohování na vyžádání vytvoří úplnou kopii hello virtuálního počítače v hello trezor služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="14c54-177">If hello initial backup is pending, on-demand backup creates a full copy of hello virtual machine in hello Recovery Services vault.</span></span> <span data-ttu-id="14c54-178">Pokud dokončení prvotního zálohování hello odeslat zálohu na vyžádání pouze změny z předchozího snímku hello toohello trezor služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="14c54-178">If hello initial backup is completed, an on-demand backup will only send changes from hello previous snapshot, toohello Recovery Services vault.</span></span> <span data-ttu-id="14c54-179">To znamená jsou následné zálohy vždy přírůstkové.</span><span class="sxs-lookup"><span data-stu-id="14c54-179">That is, subsequent backups are always incremental.</span></span>

> [!NOTE]
> <span data-ttu-id="14c54-180">Hello rozsah uchování pro zálohu na vyžádání je hello uchování hodnota zadaná pro hello denního bodu zálohy v zásadách hello.</span><span class="sxs-lookup"><span data-stu-id="14c54-180">hello retention range for an on-demand backup is hello retention value specified for hello Daily backup point in hello policy.</span></span> <span data-ttu-id="14c54-181">Pokud je vybrána žádná denního bodu zálohy, se používá hello týdenního bodu zálohy.</span><span class="sxs-lookup"><span data-stu-id="14c54-181">If no Daily backup point is selected, then hello weekly backup point is used.</span></span>
>
>

<span data-ttu-id="14c54-182">tootrigger na vyžádání zálohování virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="14c54-182">tootrigger an on-demand backup of a virtual machine:</span></span>

* <span data-ttu-id="14c54-183">Na hello [položky panelu trezoru](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikněte na tlačítko **zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="14c54-183">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Backup now**.</span></span>

    ![Zálohování teď tlačítko.](./media/backup-azure-manage-vms/backup-now-button.png)

    <span data-ttu-id="14c54-185">portál Hello zajišťuje, že chcete toostart úlohu zálohování na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="14c54-185">hello portal makes sure that you want toostart an on-demand backup job.</span></span> <span data-ttu-id="14c54-186">Klikněte na tlačítko **Ano** úlohu zálohování toostart hello.</span><span class="sxs-lookup"><span data-stu-id="14c54-186">Click **Yes** toostart hello backup job.</span></span>

    ![Zálohování teď tlačítko.](./media/backup-azure-manage-vms/backup-now-check.png)

    <span data-ttu-id="14c54-188">Úloha zálohování Hello vytvoří bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="14c54-188">hello backup job creates a recovery point.</span></span> <span data-ttu-id="14c54-189">Hello uchování bodu obnovení hello je hello stejné jako rozsah uchování určená v zásadách hello spojené s virtuálním počítačem hello.</span><span class="sxs-lookup"><span data-stu-id="14c54-189">hello retention range of hello recovery point is hello same as retention range specified in hello policy associated with hello virtual machine.</span></span> <span data-ttu-id="14c54-190">průběh hello tootrack pro úlohu hello v hello panelu trezoru, klikněte na tlačítko hello **úlohy zálohování** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="14c54-190">tootrack hello progress for hello job, in hello vault dashboard, click hello **Backup Jobs** tile.</span></span>  

## <a name="stop-protecting-virtual-machines"></a><span data-ttu-id="14c54-191">Zastavte ochranu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="14c54-191">Stop protecting virtual machines</span></span>
<span data-ttu-id="14c54-192">Pokud si zvolíte toostop ochranu virtuálního počítače, zobrazí se výzva, pokud chcete, aby se body obnovení tooretain hello.</span><span class="sxs-lookup"><span data-stu-id="14c54-192">If you choose toostop protecting a virtual machine, you are asked if you want tooretain hello recovery points.</span></span> <span data-ttu-id="14c54-193">Existují dva způsoby toostop ochranu virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="14c54-193">There are two ways toostop protecting virtual machines:</span></span>

* <span data-ttu-id="14c54-194">Zastavit všechny budoucí úlohy zálohování a odstranění všech bodů obnovení, nebo</span><span class="sxs-lookup"><span data-stu-id="14c54-194">stop all future backup jobs and delete all recovery points, or</span></span>
* <span data-ttu-id="14c54-195">Zastavit všechny budoucí úlohy zálohování, ale ponechte hello bodů obnovení</span><span class="sxs-lookup"><span data-stu-id="14c54-195">stop all future backup jobs but leave hello recovery points</span></span> <br/>

<span data-ttu-id="14c54-196">Není k dispozici s náklady spojené s ponechat hello bodů obnovení v úložišti.</span><span class="sxs-lookup"><span data-stu-id="14c54-196">There is a cost associated with leaving hello recovery points in storage.</span></span> <span data-ttu-id="14c54-197">Výhodou hello ponechat hello bodů obnovení je však, že později, můžete obnovit hello virtuálního počítače v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="14c54-197">However, hello benefit of leaving hello recovery points is you can restore hello virtual machine later, if desired.</span></span> <span data-ttu-id="14c54-198">Informace o hello náklady a hello body obnovení, najdete v části hello [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="14c54-198">For information about hello cost of leaving hello recovery points, see hello  [pricing details](https://azure.microsoft.com/pricing/details/backup/).</span></span> <span data-ttu-id="14c54-199">Pokud si zvolíte toodelete všech bodů obnovení, nelze obnovit hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="14c54-199">If you choose toodelete all recovery points, you cannot restore hello virtual machine.</span></span>

<span data-ttu-id="14c54-200">toostop ochranu pro virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="14c54-200">toostop protection for a virtual machine:</span></span>

1. <span data-ttu-id="14c54-201">Na hello [položky panelu trezoru](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikněte na tlačítko **zastavení zálohování**.</span><span class="sxs-lookup"><span data-stu-id="14c54-201">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Stop backup**.</span></span>

    ![Zastavení zálohování tlačítko](./media/backup-azure-manage-vms/stop-backup-button.png)

    <span data-ttu-id="14c54-203">Otevře se okno zastavení zálohování Hello.</span><span class="sxs-lookup"><span data-stu-id="14c54-203">hello Stop Backup blade opens.</span></span>

    ![Zastavení zálohování okno](./media/backup-azure-manage-vms/stop-backup-blade.png)
2. <span data-ttu-id="14c54-205">Na hello **zastavení zálohování** okně zvolte, zda tooretain nebo odstranění hello zálohovaná data.</span><span class="sxs-lookup"><span data-stu-id="14c54-205">On hello **Stop Backup** blade, choose whether tooretain or delete hello backup data.</span></span> <span data-ttu-id="14c54-206">pole informace Hello poskytuje podrobnosti o svého výběru.</span><span class="sxs-lookup"><span data-stu-id="14c54-206">hello information box provides details about your choice.</span></span>

    ![Zastavení ochrany](./media/backup-azure-manage-vms/retain-or-delete-option.png)
3. <span data-ttu-id="14c54-208">Pokud jste zvolili tooretain hello zálohovaná data, přeskočte toostep 4.</span><span class="sxs-lookup"><span data-stu-id="14c54-208">If you chose tooretain hello backup data, skip toostep 4.</span></span> <span data-ttu-id="14c54-209">Pokud jste zvolili toodelete zálohovaná data, potvrďte mají úlohy zálohování hello toostop a odstranit hello body obnovení – název typu hello hello položky.</span><span class="sxs-lookup"><span data-stu-id="14c54-209">If you chose toodelete backup data, confirm that you want toostop hello backup jobs and delete hello recovery points - type hello name of hello item.</span></span>

    ![Zastavit ověření](./media/backup-azure-manage-vms/item-verification-box.png)

    <span data-ttu-id="14c54-211">Pokud si nejste jisti hello položka názvu, pozastavte ukazatel myši nad hello vykřičník tooview hello název.</span><span class="sxs-lookup"><span data-stu-id="14c54-211">If you aren't sure of hello item name, hover over hello exclamation mark tooview hello name.</span></span> <span data-ttu-id="14c54-212">Navíc je hello název položky hello pod **zastavení zálohování** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="14c54-212">Also, hello name of hello item is under **Stop Backup** at hello top of hello blade.</span></span>
4. <span data-ttu-id="14c54-213">Volitelně můžete zadat **důvod** nebo **komentář**.</span><span class="sxs-lookup"><span data-stu-id="14c54-213">Optionally provide a **Reason** or **Comment**.</span></span>
5. <span data-ttu-id="14c54-214">Úloha zálohování toostop hello hello aktuální položky, klikněte na tlačítko ![tlačítko zastavení zálohování](./media/backup-azure-manage-vms/stop-backup-button-blue.png)</span><span class="sxs-lookup"><span data-stu-id="14c54-214">toostop hello backup job for hello current item, click  ![Stop backup button](./media/backup-azure-manage-vms/stop-backup-button-blue.png)</span></span>

    <span data-ttu-id="14c54-215">Oznámení umožňuje vědět, že byly zastaveny úlohy zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="14c54-215">A notification message lets you know hello backup jobs have been stopped.</span></span>

    ![Potvrďte zastavení ochrany](./media/backup-azure-manage-vms/stop-message.png)

## <a name="resume-protection-of-a-virtual-machine"></a><span data-ttu-id="14c54-217">Pokračovat v ochraně virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="14c54-217">Resume protection of a virtual machine</span></span>
<span data-ttu-id="14c54-218">Pokud hello **zachovat Data zálohování** možnost jste vybrali při ochranu pro virtuální počítač hello byla zastavena, pak je možné tooresume ochrany.</span><span class="sxs-lookup"><span data-stu-id="14c54-218">If hello **Retain Backup Data** option was chosen when protection for hello virtual machine was stopped, then it is possible tooresume protection.</span></span> <span data-ttu-id="14c54-219">Pokud hello **odstranit Data zálohování** jste vybrali možnost a pak ochranu pro virtuální počítač hello nelze obnovit.</span><span class="sxs-lookup"><span data-stu-id="14c54-219">If hello **Delete Backup Data** option was chosen, then protection for hello virtual machine cannot resume.</span></span>

<span data-ttu-id="14c54-220">tooresume ochranu pro virtuální počítač hello</span><span class="sxs-lookup"><span data-stu-id="14c54-220">tooresume protection for hello virtual machine</span></span>

1. <span data-ttu-id="14c54-221">Na hello [položky panelu trezoru](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikněte na tlačítko **obnovit zálohu**.</span><span class="sxs-lookup"><span data-stu-id="14c54-221">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Resume backup**.</span></span>

    ![Pokračovat v ochraně](./media/backup-azure-manage-vms/resume-backup-button.png)

    <span data-ttu-id="14c54-223">Otevře se okno zásady zálohování Hello.</span><span class="sxs-lookup"><span data-stu-id="14c54-223">hello Backup Policy blade opens.</span></span>

   > [!NOTE]
   > <span data-ttu-id="14c54-224">Při ochranu znovu hello virtuálního počítače, můžete zvolit jinou zásadu než hello zásada, ke které byl virtuální počítač původně chráněný.</span><span class="sxs-lookup"><span data-stu-id="14c54-224">When re-protecting hello virtual machine, you can choose a different policy than hello policy with which virtual machine was protected initially.</span></span>
   >
   >
2. <span data-ttu-id="14c54-225">Postupujte podle kroků hello v [spravovat zásady zálohování](backup-azure-manage-vms.md#manage-backup-policies) tooassign hello zásady pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="14c54-225">Follow hello steps in [Manage backup policies](backup-azure-manage-vms.md#manage-backup-policies) tooassign hello policy for hello virtual machine.</span></span>

    <span data-ttu-id="14c54-226">Jakmile zásady zálohování hello je použité toohello virtuální počítač, zobrazí hello následující zprávou.</span><span class="sxs-lookup"><span data-stu-id="14c54-226">Once hello backup policy is applied toohello virtual machine, you see hello following message.</span></span>

    ![Úspěšně chráněných virtuálních počítačů](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a><span data-ttu-id="14c54-228">Odstranění dat zálohy</span><span class="sxs-lookup"><span data-stu-id="14c54-228">Delete Backup data</span></span>
<span data-ttu-id="14c54-229">Odstraněním hello zálohovaná data související s virtuálním počítačem během hello **zastavení zálohování** úlohy, nebo můžete kdykoli po hello dokončení úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="14c54-229">You can delete hello backup data associated with a virtual machine during hello **Stop backup** job, or anytime after hello backup job has completed.</span></span> <span data-ttu-id="14c54-230">I to může být výhodné toowait dny nebo týdny před odstraněním hello body obnovení.</span><span class="sxs-lookup"><span data-stu-id="14c54-230">It may even be beneficial toowait days or weeks before deleting hello recovery points.</span></span> <span data-ttu-id="14c54-231">Na rozdíl od bodů obnovení, obnovení při odstraňování zálohovaná data, nemůžete vybrat konkrétní obnovení toodelete body.</span><span class="sxs-lookup"><span data-stu-id="14c54-231">Unlike restoring recovery points, when deleting backup data, you cannot choose specific recovery points toodelete.</span></span> <span data-ttu-id="14c54-232">Pokud si zvolíte toodelete zálohovaných dat, odstraníte všechny body obnovení, které jsou přidružené k položce hello.</span><span class="sxs-lookup"><span data-stu-id="14c54-232">If you choose toodelete your backup data, you delete all recovery points associated with hello item.</span></span>

<span data-ttu-id="14c54-233">Hello následujícím postupu se předpokládá hello úlohy zálohování pro virtuální počítač hello byla zastavena nebo zakázána.</span><span class="sxs-lookup"><span data-stu-id="14c54-233">hello following procedure assumes hello Backup job for hello virtual machine has been stopped or disabled.</span></span> <span data-ttu-id="14c54-234">Jakmile úloha zálohování hello je zakázána, hello **obnovit zálohu** a **odstranění zálohování** možnosti jsou dostupné na panelu položky hello trezoru.</span><span class="sxs-lookup"><span data-stu-id="14c54-234">Once hello Backup job is disabled, hello **Resume backup** and **Delete backup** options are available in hello vault item dashboard.</span></span>

![Opětovné spuštění a odstranění tlačítka](./media/backup-azure-manage-vms/resume-delete-buttons.png)

<span data-ttu-id="14c54-236">toodelete zálohování dat na virtuální počítač s hello *zálohy zakázané*:</span><span class="sxs-lookup"><span data-stu-id="14c54-236">toodelete backup data on a virtual machine with hello *Backup disabled*:</span></span>

1. <span data-ttu-id="14c54-237">Na hello [položky panelu trezoru](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikněte na tlačítko **zálohování odstranit**.</span><span class="sxs-lookup"><span data-stu-id="14c54-237">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Delete backup**.</span></span>

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    <span data-ttu-id="14c54-239">Hello **odstranit Data zálohování** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="14c54-239">hello **Delete Backup Data** blade opens.</span></span>

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/delete-backup-blade.png)
2. <span data-ttu-id="14c54-241">Název typu hello tooconfirm hello položku, kterou chcete body obnovení toodelete hello.</span><span class="sxs-lookup"><span data-stu-id="14c54-241">Type hello name of hello item tooconfirm you want toodelete hello recovery points.</span></span>

    ![Zastavit ověření](./media/backup-azure-manage-vms/item-verification-box.png)

    <span data-ttu-id="14c54-243">Pokud si nejste jisti hello položka názvu, pozastavte ukazatel myši nad hello vykřičník tooview hello název.</span><span class="sxs-lookup"><span data-stu-id="14c54-243">If you aren't sure of hello item name, hover over hello exclamation mark tooview hello name.</span></span> <span data-ttu-id="14c54-244">Navíc je hello název položky hello pod **odstranit Data zálohování** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="14c54-244">Also, hello name of hello item is under **Delete Backup Data** at hello top of hello blade.</span></span>
3. <span data-ttu-id="14c54-245">Volitelně můžete zadat **důvod** nebo **komentář**.</span><span class="sxs-lookup"><span data-stu-id="14c54-245">Optionally provide a **Reason** or **Comment**.</span></span>
4. <span data-ttu-id="14c54-246">toodelete hello zálohování dat pro aktuální položku hello, klikněte na tlačítko ![tlačítko zastavení zálohování](./media/backup-azure-manage-vms/delete-button.png)</span><span class="sxs-lookup"><span data-stu-id="14c54-246">toodelete hello backup data for hello current item, click  ![Stop backup button](./media/backup-azure-manage-vms/delete-button.png)</span></span>

    <span data-ttu-id="14c54-247">Oznámení umožňuje vědět, že byl odstraněn hello zálohovaná data.</span><span class="sxs-lookup"><span data-stu-id="14c54-247">A notification message lets you know hello backup data has been deleted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14c54-248">Další kroky</span><span class="sxs-lookup"><span data-stu-id="14c54-248">Next steps</span></span>
<span data-ttu-id="14c54-249">Informace o opětovné vytvoření virtuálního počítače z bodu obnovení, podívejte se na [obnovení virtuálních počítačů Azure](backup-azure-restore-vms.md).</span><span class="sxs-lookup"><span data-stu-id="14c54-249">For information on re-creating a virtual machine from a recovery point, check out [Restore Azure VMs](backup-azure-restore-vms.md).</span></span> <span data-ttu-id="14c54-250">Pokud potřebujete informace o ochraně virtuálních počítačů, přečtěte si [první pohled: zálohování virtuálních počítačů tooa trezor služeb zotavení](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="14c54-250">If you need information on protecting your virtual machines, see [First look: Back up VMs tooa Recovery Services vault](backup-azure-vms-first-look-arm.md).</span></span> <span data-ttu-id="14c54-251">Informace o sledování událostí najdete v tématu [monitorování výstrahy pro virtuální počítač Azure zálohy](backup-azure-monitor-vms.md).</span><span class="sxs-lookup"><span data-stu-id="14c54-251">For information on monitoring events, see [Monitor alerts for Azure virtual machine backups](backup-azure-monitor-vms.md).</span></span>
