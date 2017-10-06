---
title: aaaStorSimple Snapshot Manager svazku skupiny | Microsoft Docs
description: "Popisuje, jak toouse hello toocreate modul snap-in konzoly MMC StorSimple Snapshot Manager a Správa skupin svazku."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 7a232414-6a28-4b81-bd7b-cf61e28b33d7
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: f7830b62761db7aa5139456b555b6168fb6a85de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toocreate-and-manage-volume-groups"></a><span data-ttu-id="43b35-103">Použít toocreate StorSimple Snapshot Manager a Správa skupin svazku</span><span class="sxs-lookup"><span data-stu-id="43b35-103">Use StorSimple Snapshot Manager toocreate and manage volume groups</span></span>
## <a name="overview"></a><span data-ttu-id="43b35-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="43b35-104">Overview</span></span>
<span data-ttu-id="43b35-105">Můžete použít hello **svazku skupiny** uzlu na hello **oboru** podokně tooassign svazky toovolume skupiny, zobrazení informací o skupinu svazku naplánovat zálohování a upravit skupiny pro svazek.</span><span class="sxs-lookup"><span data-stu-id="43b35-105">You can use hello **Volume Groups** node on hello **Scope** pane tooassign volumes toovolume groups, view information about a volume group, schedule backups, and edit volume groups.</span></span>

<span data-ttu-id="43b35-106">Svazek skupiny jsou fondy související svazky, které používají tooensure zálohy jsou konzistentní s aplikací.</span><span class="sxs-lookup"><span data-stu-id="43b35-106">Volume groups are pools of related volumes used tooensure that backups are application-consistent.</span></span> <span data-ttu-id="43b35-107">Další informace najdete v tématu [svazky a svazek skupiny](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) a [integrace s Windows služby Stínová kopie svazku](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).</span><span class="sxs-lookup"><span data-stu-id="43b35-107">For more information, see [Volumes and volume groups](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) and [Integration with Windows Volume Shadow Copy Service](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="43b35-108">Všechny svazky ve skupině svazku musí pocházet ze zprostředkovatele služeb jeden cloud.</span><span class="sxs-lookup"><span data-stu-id="43b35-108">All volumes in a volume group must come from a single cloud service provider.</span></span>
> * <span data-ttu-id="43b35-109">Když konfigurujete skupiny svazku, nemíchat sdílených svazcích clusteru (CSV) a jiný CSV hello stejnou skupinu svazku.</span><span class="sxs-lookup"><span data-stu-id="43b35-109">When you configure volume groups, do not mix cluster-shared volumes (CSVs) and non-CSVs in hello same volume group.</span></span> <span data-ttu-id="43b35-110">Snapshot Manager zařízení StorSimple nepodporuje kombinaci sdílených svazků clusteru a bez CSV v hello stejné snímku.</span><span class="sxs-lookup"><span data-stu-id="43b35-110">StorSimple Snapshot Manager does not support a mix of CSVs and non-CSVs in hello same snapshot.</span></span>

![Uzel skupiny svazku](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

<span data-ttu-id="43b35-112">**Obrázek 1: Uzel StorSimple Snapshot Manager svazku skupiny**</span><span class="sxs-lookup"><span data-stu-id="43b35-112">**Figure 1: StorSimple Snapshot Manager Volume Groups node**</span></span> 

<span data-ttu-id="43b35-113">Tento kurz vysvětluje, jak můžete pomocí StorSimple Snapshot Manager do:</span><span class="sxs-lookup"><span data-stu-id="43b35-113">This tutorial explains how you can use StorSimple Snapshot Manager to:</span></span>

* <span data-ttu-id="43b35-114">Zobrazení informací o svazku skupin</span><span class="sxs-lookup"><span data-stu-id="43b35-114">View information about your volume groups</span></span>
* <span data-ttu-id="43b35-115">Vytvořte skupinu svazku</span><span class="sxs-lookup"><span data-stu-id="43b35-115">Create a volume group</span></span>
* <span data-ttu-id="43b35-116">Zálohování svazku skupiny</span><span class="sxs-lookup"><span data-stu-id="43b35-116">Back up a volume group</span></span>
* <span data-ttu-id="43b35-117">Úprava skupiny svazku</span><span class="sxs-lookup"><span data-stu-id="43b35-117">Edit a volume group</span></span>
* <span data-ttu-id="43b35-118">Odstranit skupinu svazku</span><span class="sxs-lookup"><span data-stu-id="43b35-118">Delete a volume group</span></span>

<span data-ttu-id="43b35-119">Všechny tyto akce jsou také k dispozici na hello **akce** podokně.</span><span class="sxs-lookup"><span data-stu-id="43b35-119">All of these actions are also available on hello **Actions** pane.</span></span>

## <a name="view-volume-groups"></a><span data-ttu-id="43b35-120">Zobrazit svazku skupiny</span><span class="sxs-lookup"><span data-stu-id="43b35-120">View volume groups</span></span>
<span data-ttu-id="43b35-121">Pokud kliknete na tlačítko hello **svazku skupiny** uzlu, hello **výsledky** podokně zobrazí hello následující informace o každé skupiny svazku, v závislosti na vybrané sloupce hello provedete.</span><span class="sxs-lookup"><span data-stu-id="43b35-121">If you click hello **Volume Groups** node, hello **Results** pane shows hello following information about each volume group, depending on hello column selections you make.</span></span> <span data-ttu-id="43b35-122">(hello sloupců v hello **výsledky** podokně se dají konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="43b35-122">(hello columns in hello **Results** pane are configurable.</span></span> <span data-ttu-id="43b35-123">Klikněte pravým tlačítkem na hello **svazky** uzlu, vyberte **zobrazení**a potom vyberte **přidat nebo odebrat sloupce**.)</span><span class="sxs-lookup"><span data-stu-id="43b35-123">Right-click hello **Volumes** node, select **View**, and then select **Add/Remove Columns**.)</span></span>

| <span data-ttu-id="43b35-124">Sloupec výsledků</span><span class="sxs-lookup"><span data-stu-id="43b35-124">Results column</span></span> | <span data-ttu-id="43b35-125">Popis</span><span class="sxs-lookup"><span data-stu-id="43b35-125">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="43b35-126">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="43b35-126">Name</span></span> |<span data-ttu-id="43b35-127">Hello **název** sloupec obsahuje název hello hello svazku skupiny.</span><span class="sxs-lookup"><span data-stu-id="43b35-127">hello **Name** column contains hello name of hello volume group.</span></span> |
| <span data-ttu-id="43b35-128">Aplikace</span><span class="sxs-lookup"><span data-stu-id="43b35-128">Application</span></span> |<span data-ttu-id="43b35-129">Hello **aplikace** sloupci se zobrazuje počet hello zapisovače VSS aktuálně nainstalovaná a spuštěná hostitele Windows hello.</span><span class="sxs-lookup"><span data-stu-id="43b35-129">hello **Applications** column shows hello number of VSS writers currently installed and running on hello Windows host.</span></span> |
| <span data-ttu-id="43b35-130">Vybráno</span><span class="sxs-lookup"><span data-stu-id="43b35-130">Selected</span></span> |<span data-ttu-id="43b35-131">Hello **vybrané** sloupci se zobrazuje hello počtu svazků, které jsou obsaženy ve skupině hello svazku.</span><span class="sxs-lookup"><span data-stu-id="43b35-131">hello **Selected** column shows hello number of volumes that are contained in hello volume group.</span></span> <span data-ttu-id="43b35-132">Nula (0) označuje, že žádná aplikace je přidružen hello svazky ve skupině hello svazku.</span><span class="sxs-lookup"><span data-stu-id="43b35-132">A zero (0) indicates that no application is associated with hello volumes in hello volume group.</span></span> |
| <span data-ttu-id="43b35-133">Importovat</span><span class="sxs-lookup"><span data-stu-id="43b35-133">Imported</span></span> |<span data-ttu-id="43b35-134">Hello **importovaný** sloupci se zobrazuje hello počtu importovaných svazků.</span><span class="sxs-lookup"><span data-stu-id="43b35-134">hello **Imported** column shows hello number of imported volumes.</span></span> <span data-ttu-id="43b35-135">Pokud nastavíte příliš**True**, tento sloupec označuje, že svazek skupinu byla naimportována ze hello portál Azure a nebyl vytvořen v zařízení StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="43b35-135">When set too**True**, this column indicates that a volume group was imported from hello Azure portal and was not created in StorSimple Snapshot Manager.</span></span> |

> [!NOTE]
> <span data-ttu-id="43b35-136">Skupiny svazek StorSimple Snapshot Manager se zobrazuje také na hello **zásady zálohování** ve hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="43b35-136">StorSimple Snapshot Manager volume groups are also displayed on hello **Backup Policies** tab in hello Azure portal.</span></span>
> 
> 

## <a name="create-a-volume-group"></a><span data-ttu-id="43b35-137">Vytvořte skupinu svazku</span><span class="sxs-lookup"><span data-stu-id="43b35-137">Create a volume group</span></span>
<span data-ttu-id="43b35-138">Použijte následující postup toocreate skupinu svazku hello.</span><span class="sxs-lookup"><span data-stu-id="43b35-138">Use hello following procedure toocreate a volume group.</span></span>

#### <a name="toocreate-a-volume-group"></a><span data-ttu-id="43b35-139">toocreate skupinu svazku</span><span class="sxs-lookup"><span data-stu-id="43b35-139">toocreate a volume group</span></span>
1. <span data-ttu-id="43b35-140">Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="43b35-140">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="43b35-141">V hello **oboru** podokně klikněte pravým tlačítkem na **svazku skupiny**a potom klikněte na **vytvořit skupinu svazku**.</span><span class="sxs-lookup"><span data-stu-id="43b35-141">In hello **Scope** pane, right-click **Volume Groups**, and then click **Create Volume Group**.</span></span>
   
    ![Vytvoření svazku skupiny](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
   
    <span data-ttu-id="43b35-143">Hello **vytvořte skupinu svazku** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="43b35-143">hello **Create a volume group** dialog box appears.</span></span>
   
    ![Vytvoření skupiny dialogu svazku](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png)
3. <span data-ttu-id="43b35-145">Zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="43b35-145">Enter hello following information:</span></span>
   
   1. <span data-ttu-id="43b35-146">V hello **název** zadejte jedinečný název pro novou skupinu svazku hello.</span><span class="sxs-lookup"><span data-stu-id="43b35-146">In hello **Name** box, type a unique name for hello new volume group.</span></span>
   2. <span data-ttu-id="43b35-147">V hello **aplikace** pole, vyberte aplikací přidružených k hello svazky, že budete přidávat skupiny toohello svazku.</span><span class="sxs-lookup"><span data-stu-id="43b35-147">In hello **Applications** box, select applications associated with hello volumes that you will be adding toohello volume group.</span></span>
      
       <span data-ttu-id="43b35-148">Hello **aplikace** seznamy pro je povoleno pouze aplikace, které používají svazky zařízení StorSimple a mají zapisovače služby Stínová kopie svazku.</span><span class="sxs-lookup"><span data-stu-id="43b35-148">hello **Applications** box lists only those applications that use StorSimple volumes and have VSS writers enabled for them.</span></span> <span data-ttu-id="43b35-149">Zapisovač VSS je povoleno, jen pokud všechny svazky hello je vědět, že zapisovač hello jsou svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="43b35-149">A VSS writer is enabled only if all hello volumes that hello writer is aware of are StorSimple volumes.</span></span> <span data-ttu-id="43b35-150">Pokud pole aplikace hello je prázdná, jsou nainstalovány žádné aplikace, které používají Azure StorSimple svazky a podporovaná zapisovače služby Stínová kopie svazku.</span><span class="sxs-lookup"><span data-stu-id="43b35-150">If hello Applications box is empty, then no applications that use Azure StorSimple volumes and have supported VSS writers are installed.</span></span> <span data-ttu-id="43b35-151">(V současné době Azure StorSimple podporuje Microsoft Exchange a SQL Server.) Další informace o zapisovače VSS najdete v tématu [integrace s Windows služby Stínová kopie svazku](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).</span><span class="sxs-lookup"><span data-stu-id="43b35-151">(Currently, Azure StorSimple supports Microsoft Exchange and SQL Server.) For more information about VSS writers, see [Integration with Windows Volume Shadow Copy Service](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).</span></span>
      
       <span data-ttu-id="43b35-152">Pokud vyberete aplikace, automaticky se vyberou všechny svazky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="43b35-152">If you select an application, all volumes associated with it are automatically selected.</span></span> <span data-ttu-id="43b35-153">Naopak, pokud jste vybrali svazky, které jsou přidruženy k určité aplikaci, aplikace hello je automaticky vybrán v hello **aplikace** pole.</span><span class="sxs-lookup"><span data-stu-id="43b35-153">Conversely, if you select volumes associated with a specific application, hello application is automatically selected in hello **Applications** box.</span></span> 
   3. <span data-ttu-id="43b35-154">V hello **svazky** vyberte skupinu, StorSimple svazky tooadd toohello svazku.</span><span class="sxs-lookup"><span data-stu-id="43b35-154">In hello **Volumes** box, select StorSimple volumes tooadd toohello volume group.</span></span> 
      
      * <span data-ttu-id="43b35-155">Můžete zahrnout svazků s jednoho nebo více oddílů.</span><span class="sxs-lookup"><span data-stu-id="43b35-155">You can include volumes with single or multiple partitions.</span></span> <span data-ttu-id="43b35-156">(Více svazků oddílu může být dynamické disky nebo základní disky s více oddílů.) Svazek, který obsahuje více oddílů je považována za jednu jednotku.</span><span class="sxs-lookup"><span data-stu-id="43b35-156">(Multiple partition volumes can be dynamic disks or basic disks with multiple partitions.) A volume that contains multiple partitions is treated as a single unit.</span></span> <span data-ttu-id="43b35-157">V důsledku toho, pokud přidáte pouze jeden svazek skupiny hello oddíly tooa, všechny hello oddíly jsou automaticky přidané toothat svazku skupina na hello stejný čas.</span><span class="sxs-lookup"><span data-stu-id="43b35-157">Consequently, if you add only one of hello partitions tooa volume group, all hello other partitions are automatically added toothat volume group at hello same time.</span></span> <span data-ttu-id="43b35-158">Po přidání více tooa svazku skupinu svazku oddílu hello více svazku oddílu pokračuje toobe považován za jednu jednotku.</span><span class="sxs-lookup"><span data-stu-id="43b35-158">After you add a multiple partition volume tooa volume group, hello multiple partition volume continues toobe treated as a single unit.</span></span>
      * <span data-ttu-id="43b35-159">Můžete vytvořit prázdný svazku skupiny není přiřazením toothem žádné svazky.</span><span class="sxs-lookup"><span data-stu-id="43b35-159">You can create empty volume groups by not assigning any volumes toothem.</span></span> 
      * <span data-ttu-id="43b35-160">Nemíchat sdílených svazcích clusteru (CSV) a jiný CSV hello stejnou skupinu svazku.</span><span class="sxs-lookup"><span data-stu-id="43b35-160">Do not mix cluster-shared volumes (CSVs) and non-CSVs in hello same volume group.</span></span> <span data-ttu-id="43b35-161">Snapshot Manager zařízení StorSimple nepodporuje směs Sdílené svazky clusteru a bez Sdílené svazky clusteru v hello stejné snímku.</span><span class="sxs-lookup"><span data-stu-id="43b35-161">StorSimple Snapshot Manager does not support a mix of CSV volumes and non-CSV volumes in hello same snapshot.</span></span>
4. <span data-ttu-id="43b35-162">Klikněte na tlačítko **OK** toosave hello svazku skupiny.</span><span class="sxs-lookup"><span data-stu-id="43b35-162">Click **OK** toosave hello volume group.</span></span>

## <a name="back-up-a-volume-group"></a><span data-ttu-id="43b35-163">Zálohování svazku skupiny</span><span class="sxs-lookup"><span data-stu-id="43b35-163">Back up a volume group</span></span>
<span data-ttu-id="43b35-164">Použijte následující postup tooback skupiny svazku hello.</span><span class="sxs-lookup"><span data-stu-id="43b35-164">Use hello following procedure tooback up a volume group.</span></span>

#### <a name="tooback-up-a-volume-group"></a><span data-ttu-id="43b35-165">tooback skupiny svazku</span><span class="sxs-lookup"><span data-stu-id="43b35-165">tooback up a volume group</span></span>
1. <span data-ttu-id="43b35-166">Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="43b35-166">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="43b35-167">V hello **oboru** podokně rozbalte hello **svazku skupiny** uzel, klikněte pravým tlačítkem na název skupiny svazku a pak klikněte na tlačítko **provést zálohování**.</span><span class="sxs-lookup"><span data-stu-id="43b35-167">In hello **Scope** pane, expand hello **Volume Groups** node, right-click a volume group name, and then click **Take Backup**.</span></span>
   
    ![Zálohování svazku skupiny okamžitě](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)
3. <span data-ttu-id="43b35-169">V hello **provést zálohování** dialogové okno, vyberte **místní snímek** nebo **Cloudový snímek**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="43b35-169">In hello **Take Backup** dialog box, select **Local Snapshot** or **Cloud Snapshot**, and then click **Create**.</span></span>
   
    ![Trvat zálohování dialogové okno](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png)
4. <span data-ttu-id="43b35-171">je spuštěna tooconfirm, který hello zálohování, rozbalte položku hello **úlohy** uzel a pak klikněte na tlačítko **systémem**.</span><span class="sxs-lookup"><span data-stu-id="43b35-171">tooconfirm that hello backup is running, expand hello **Jobs** node, and then click **Running**.</span></span> <span data-ttu-id="43b35-172">Hello zálohování by měl být uvedený.</span><span class="sxs-lookup"><span data-stu-id="43b35-172">hello backup should be listed.</span></span>
5. <span data-ttu-id="43b35-173">hello tooview dokončit snímku, rozbalte položku hello **katalog zálohování** uzlu, rozbalte název skupiny svazku hello a pak klikněte na tlačítko **místní snímek** nebo **Cloudový snímek**.</span><span class="sxs-lookup"><span data-stu-id="43b35-173">tooview hello completed snapshot, expand hello **Backup Catalog** node, expand hello volume group name, and then click **Local Snapshot** or **Cloud Snapshot**.</span></span> <span data-ttu-id="43b35-174">zálohování Hello se objeví, pokud byl úspěšně dokončen.</span><span class="sxs-lookup"><span data-stu-id="43b35-174">hello backup will be listed if it finished successfully.</span></span>

## <a name="edit-a-volume-group"></a><span data-ttu-id="43b35-175">Úprava skupiny svazku</span><span class="sxs-lookup"><span data-stu-id="43b35-175">Edit a volume group</span></span>
<span data-ttu-id="43b35-176">Použijte následující postup tooedit skupinu svazku hello.</span><span class="sxs-lookup"><span data-stu-id="43b35-176">Use hello following procedure tooedit a volume group.</span></span>

#### <a name="tooedit-a-volume-group"></a><span data-ttu-id="43b35-177">tooedit skupinu svazku</span><span class="sxs-lookup"><span data-stu-id="43b35-177">tooedit a volume group</span></span>
1. <span data-ttu-id="43b35-178">Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="43b35-178">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="43b35-179">V hello **oboru** podokně rozbalte hello **svazku skupiny** uzel, klikněte pravým tlačítkem na název skupiny svazku a pak klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="43b35-179">In hello **Scope** pane, expand hello **Volume Groups** node, right-click a volume group name, and then click **Edit**.</span></span>
3. <span data-ttu-id="43b35-180">Hello ** vytvořte skupinu svazku ** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="43b35-180">hello **Create a volume group **dialog box appears.</span></span> <span data-ttu-id="43b35-181">Můžete změnit hello **název**, **aplikace**, a **svazky** položky.</span><span class="sxs-lookup"><span data-stu-id="43b35-181">You can change hello **Name**, **Applications**, and **Volumes** entries.</span></span>
4. <span data-ttu-id="43b35-182">Klikněte na tlačítko **OK** toosave změny.</span><span class="sxs-lookup"><span data-stu-id="43b35-182">Click **OK** toosave your changes.</span></span>

## <a name="delete-a-volume-group"></a><span data-ttu-id="43b35-183">Odstranit skupinu svazku</span><span class="sxs-lookup"><span data-stu-id="43b35-183">Delete a volume group</span></span>
<span data-ttu-id="43b35-184">Použijte následující postup toodelete skupinu svazku hello.</span><span class="sxs-lookup"><span data-stu-id="43b35-184">Use hello following procedure toodelete a volume group.</span></span> 

> [!WARNING]
> <span data-ttu-id="43b35-185">To také odstraní všechny zálohy hello přidružené skupině hello svazku.</span><span class="sxs-lookup"><span data-stu-id="43b35-185">This also deletes all hello backups associated with hello volume group.</span></span>
> 
> 

#### <a name="toodelete-a-volume-group"></a><span data-ttu-id="43b35-186">toodelete skupinu svazku</span><span class="sxs-lookup"><span data-stu-id="43b35-186">toodelete a volume group</span></span>
1. <span data-ttu-id="43b35-187">Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="43b35-187">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="43b35-188">V hello **oboru** podokně rozbalte hello **svazku skupiny** uzel, klikněte pravým tlačítkem na název skupiny svazku a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="43b35-188">In hello **Scope** pane, expand hello **Volume Groups** node, right-click a volume group name, and then click **Delete**.</span></span>
3. <span data-ttu-id="43b35-189">Hello **odstranit skupinu svazku** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="43b35-189">hello **Delete Volume Group** dialog box appears.</span></span> <span data-ttu-id="43b35-190">Typ **potvrdit** v hello textového pole a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="43b35-190">Type **Confirm** in hello text box, and then click **OK**.</span></span>
   
    <span data-ttu-id="43b35-191">skupiny svazku Hello odstranit zmizí ze seznamu hello v hello **výsledky** podokně a všechny zálohy, které jsou přidružené k této skupině svazku se odstraní z katalogu zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="43b35-191">hello deleted volume group vanishes from hello list in hello **Results** pane and all backups that are associated with that volume group are deleted from hello backup catalog.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43b35-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="43b35-192">Next steps</span></span>
* <span data-ttu-id="43b35-193">Zjistěte, jak příliš[pomocí vašeho řešení StorSimple Snapshot Manager zařízení StorSimple tooadminister](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="43b35-193">Learn how too[use StorSimple Snapshot Manager tooadminister your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="43b35-194">Zjistěte, jak příliš[použít toocreate Snapshot Manager zařízení StorSimple a spravovat zásady zálohování](storsimple-snapshot-manager-manage-backup-policies.md).</span><span class="sxs-lookup"><span data-stu-id="43b35-194">Learn how too[use StorSimple Snapshot Manager toocreate and manage backup policies](storsimple-snapshot-manager-manage-backup-policies.md).</span></span>

