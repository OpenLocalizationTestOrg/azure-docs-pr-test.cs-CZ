---
title: aaaClone svazek StorSimple | Microsoft Docs
description: "Popisuje typy hello různých klonování a kdy toouse a vysvětluje, jak můžete pomocí zálohovacího skladu tooclone svazku."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b5d615f2-02a7-4222-9867-6c0385ce748c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: e98d28db1abeb515ba78ab5860e7c5eba0dfcbb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooclone-a-volume"></a><span data-ttu-id="0a312-103">Použít tooclone služby StorSimple Manager hello svazku</span><span class="sxs-lookup"><span data-stu-id="0a312-103">Use hello StorSimple Manager service tooclone a volume</span></span>
[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a><span data-ttu-id="0a312-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="0a312-104">Overview</span></span>
<span data-ttu-id="0a312-105">Hello služby StorSimple Manager **zálohování katalogu** stránka zobrazuje všechny zálohovací sklady hello, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy.</span><span class="sxs-lookup"><span data-stu-id="0a312-105">hello StorSimple Manager service **Backup Catalog** page displays all hello backup sets that are created when manual or automated backups are taken.</span></span> <span data-ttu-id="0a312-106">Můžete použít tuto stránku toolist všechny hello zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohování toorestore nebo klonování svazku.</span><span class="sxs-lookup"><span data-stu-id="0a312-106">You can use this page toolist all hello backups for a backup policy or a volume, select or delete backups, or use a backup toorestore or clone a volume.</span></span>

![Stránka zálohování katalogu](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

<span data-ttu-id="0a312-108">Tento kurz popisuje, jak můžete pomocí zálohovacího skladu tooclone svazku.</span><span class="sxs-lookup"><span data-stu-id="0a312-108">This tutorial describes how you can use a backup set tooclone an individual volume.</span></span> <span data-ttu-id="0a312-109">Také vysvětluje hello rozdíl mezi *přechodný* a *trvalé* provede klonování.</span><span class="sxs-lookup"><span data-stu-id="0a312-109">It also explains hello difference between *transient* and *permanent* clones.</span></span> 

## <a name="create-a-clone-of-a-volume"></a><span data-ttu-id="0a312-110">Vytvořit klon svazku</span><span class="sxs-lookup"><span data-stu-id="0a312-110">Create a clone of a volume</span></span>
<span data-ttu-id="0a312-111">Můžete vytvořit klon na hello stejného zařízení, jiná zařízení nebo i virtuálního počítače pomocí místní nebo cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="0a312-111">You can create a clone on hello same device, another device, or even a virtual machine by using a local or a cloud snapshot.</span></span>

#### <a name="tooclone-a-volume"></a><span data-ttu-id="0a312-112">tooclone svazku</span><span class="sxs-lookup"><span data-stu-id="0a312-112">tooclone a volume</span></span>
1. <span data-ttu-id="0a312-113">Na stránce služby StorSimple Manager hello, klikněte na hello **katalog zálohování** a vyberte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="0a312-113">On hello StorSimple Manager service page, click hello **Backup catalog** tab and select a backup set.</span></span>
2. <span data-ttu-id="0a312-114">Rozbalte hello zálohovacího skladu tooview hello přidružené svazky.</span><span class="sxs-lookup"><span data-stu-id="0a312-114">Expand hello backup set tooview hello associated volumes.</span></span> <span data-ttu-id="0a312-115">Klikněte a vyberte svazek z hello zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="0a312-115">Click and select a volume from hello backup set.</span></span>
   
     ![Klonování svazku](./media/storsimple-clone-volume/HCS_Clone.png) 
3. <span data-ttu-id="0a312-117">Klikněte na tlačítko **klon** toobegin klonování hello vybraný svazek.</span><span class="sxs-lookup"><span data-stu-id="0a312-117">Click **Clone** toobegin cloning hello selected volume.</span></span>
4. <span data-ttu-id="0a312-118">V Průvodci hello klonování svazku v části **zadejte název a umístění**:</span><span class="sxs-lookup"><span data-stu-id="0a312-118">In hello Clone Volume wizard, under **Specify name and location**:</span></span>
   
   1. <span data-ttu-id="0a312-119">Určete cílové zařízení.</span><span class="sxs-lookup"><span data-stu-id="0a312-119">Identify a target device.</span></span> <span data-ttu-id="0a312-120">Toto je hello umístění, kde bude vytvořen hello klonování.</span><span class="sxs-lookup"><span data-stu-id="0a312-120">This is hello location where hello clone will be created.</span></span> <span data-ttu-id="0a312-121">Můžete vybrat hello stejné zařízení nebo zadejte jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="0a312-121">You can choose hello same device or specify another device.</span></span> <span data-ttu-id="0a312-122">Pokud zvolíte svazku přidruženém k ostatní poskytovatele cloudových služeb (ne Azure), hello rozevíracího seznamu pro hello cílové zařízení se zobrazí pouze fyzických zařízení.</span><span class="sxs-lookup"><span data-stu-id="0a312-122">If you choose a volume associated with other cloud service providers (not Azure), hello drop-down list for hello target device will only show physical devices.</span></span> <span data-ttu-id="0a312-123">Nelze klonovat svazku přidruženém k ostatní poskytovatele cloudových služeb na virtuální zařízení.</span><span class="sxs-lookup"><span data-stu-id="0a312-123">You cannot clone a volume associated with other cloud service providers on a virtual device.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="0a312-124">Ujistěte se, že požadovaná pro klon hello kapacita hello je nižší než hello kapacity, které jsou k dispozici na cílovém zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="0a312-124">Make sure that hello capacity required for hello clone is lower than hello capacity available on hello target device.</span></span>
      > 
      > 
   2. <span data-ttu-id="0a312-125">Zadejte název jedinečné svazku pro vaše klon.</span><span class="sxs-lookup"><span data-stu-id="0a312-125">Specify a unique volume name for your clone.</span></span> <span data-ttu-id="0a312-126">Hello název musí obsahovat 3 až 127 znaků.</span><span class="sxs-lookup"><span data-stu-id="0a312-126">hello name must contain between 3 and 127 characters.</span></span>
   3. <span data-ttu-id="0a312-127">Klikněte na ikonu šipky hello</span><span class="sxs-lookup"><span data-stu-id="0a312-127">Click hello arrow icon</span></span> ![ikona šipky](./media/storsimple-clone-volume/HCS_ArrowIcon.png) <span data-ttu-id="0a312-129">tooproceed toohello další stránku.</span><span class="sxs-lookup"><span data-stu-id="0a312-129">tooproceed toohello next page.</span></span>
5. <span data-ttu-id="0a312-130">V části **zadejte hostitelů, které můžete použít tento svazek**:</span><span class="sxs-lookup"><span data-stu-id="0a312-130">Under **Specify hosts that can use this volume**:</span></span>
   
   1. <span data-ttu-id="0a312-131">Zadejte záznam řízení přístupu (ACR) pro klon hello.</span><span class="sxs-lookup"><span data-stu-id="0a312-131">Specify an access control record (ACR) for hello clone.</span></span> <span data-ttu-id="0a312-132">Můžete přidat nové ACR nebo vyberte možnost ze seznamu existující hello.</span><span class="sxs-lookup"><span data-stu-id="0a312-132">You can add a new ACR or choose from hello existing list.</span></span>
   2. <span data-ttu-id="0a312-133">Klikněte na ikonu zaškrtnutí hello</span><span class="sxs-lookup"><span data-stu-id="0a312-133">Click hello check icon</span></span> ![ikona zaškrtnutí](./media/storsimple-clone-volume/HCS_CheckIcon.png)<span data-ttu-id="0a312-135">operace toocomplete hello.</span><span class="sxs-lookup"><span data-stu-id="0a312-135">toocomplete hello operation.</span></span>
6. <span data-ttu-id="0a312-136">Úloha klonování bude inicializován a budete informováni při klonování hello je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="0a312-136">A clone job will be initiated and you will be notified when hello clone is successfully created.</span></span> <span data-ttu-id="0a312-137">Klikněte na tlačítko **zobrazit úlohy** toomonitor hello klon úlohy na hello **úlohy** stránky.</span><span class="sxs-lookup"><span data-stu-id="0a312-137">Click **View Job** toomonitor hello clone job on hello **Jobs** page.</span></span>
7. <span data-ttu-id="0a312-138">Po hello dokončení úlohy klonu:</span><span class="sxs-lookup"><span data-stu-id="0a312-138">After hello clone job is completed:</span></span>
   
   1. <span data-ttu-id="0a312-139">Přejděte toohello **zařízení** stránku a vyberte hello **kontejnery svazků** kartě.</span><span class="sxs-lookup"><span data-stu-id="0a312-139">Go toohello **Devices** page, and select hello **Volume Containers** tab.</span></span> 
   2. <span data-ttu-id="0a312-140">Vyberte kontejner hello svazek, který je přidružen hello zdrojový svazek, který jste naklonovali.</span><span class="sxs-lookup"><span data-stu-id="0a312-140">Select hello volume container that is associated with hello source volume that you cloned.</span></span> <span data-ttu-id="0a312-141">V seznamu hello svazků měli byste vidět hello klonování, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0a312-141">In hello list of volumes, you should see hello clone that was just created.</span></span>

> [!NOTE]
> <span data-ttu-id="0a312-142">Monitorování a výchozí zálohování jsou automaticky zakázán na klonovaný svazku.</span><span class="sxs-lookup"><span data-stu-id="0a312-142">Monitoring and default backup are automatically disabled on a cloned volume.</span></span>
> 
> 

<span data-ttu-id="0a312-143">Klon, který je vytvořen tímto způsobem je přechodný klonu.</span><span class="sxs-lookup"><span data-stu-id="0a312-143">A clone that is created this way is a transient clone.</span></span> <span data-ttu-id="0a312-144">Další informace o typech klonování najdete v tématu [přechodný oproti trvalé klony](#transient-vs-permanent-clones).</span><span class="sxs-lookup"><span data-stu-id="0a312-144">For more information about clone types, see [Transient vs. permanent clones](#transient-vs-permanent-clones).</span></span>

<span data-ttu-id="0a312-145">Tenhle klon. je teď regulární svazek a všechny operace, které je možné ve svazku, bude k dispozici pro klonování hello.</span><span class="sxs-lookup"><span data-stu-id="0a312-145">This clone is now a regular volume, and any operation that is possible on a volume will be available for hello clone.</span></span> <span data-ttu-id="0a312-146">Budete potřebovat tooconfigure tento svazek pro všechny zálohy.</span><span class="sxs-lookup"><span data-stu-id="0a312-146">You will need tooconfigure this volume for any backups.</span></span>

## <a name="transient-vs-permanent-clones"></a><span data-ttu-id="0a312-147">Pouze dočasné a trvalé klony</span><span class="sxs-lookup"><span data-stu-id="0a312-147">Transient vs. permanent clones</span></span>
<span data-ttu-id="0a312-148">Přechodný a trvalé klony vytvoří jenom v případě, že se klonování na tooa jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="0a312-148">Transient and permanent clones are created only when you are cloning on tooa different device.</span></span> <span data-ttu-id="0a312-149">Může klonovat konkrétní svazku z jiného zařízení tooa zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="0a312-149">You can clone a specific volume from a backup set tooa different device.</span></span> <span data-ttu-id="0a312-150">Klon vytvořené v tomto případě je *přechodný* klonování.</span><span class="sxs-lookup"><span data-stu-id="0a312-150">A clone created in this way is a *transient* clone.</span></span> <span data-ttu-id="0a312-151">Hello přechodný klonování bude mít odkazy toohello původního svazku a bude používat tento svazek tooread při zápisu místně.</span><span class="sxs-lookup"><span data-stu-id="0a312-151">hello transient clone will have references toohello original volume and will use that volume tooread while writing locally.</span></span> 

<span data-ttu-id="0a312-152">Po provedení cloudový snímek přechodný klon hello výsledné klonování bude *trvalé* klonování.</span><span class="sxs-lookup"><span data-stu-id="0a312-152">After you take a cloud snapshot of a transient clone, hello resulting clone will be a *permanent* clone.</span></span> <span data-ttu-id="0a312-153">Hello trvalé klon je nezávislá a nemá žádné odkazy toohello původní svazek, který byl naklonována ze.</span><span class="sxs-lookup"><span data-stu-id="0a312-153">hello permanent clone is independent and doesn’t have any references toohello original volume that it was cloned from.</span></span>  

## <a name="scenarios-for-transient-and-permanent-clones"></a><span data-ttu-id="0a312-154">Scénáře pro přechodný a trvalé klony</span><span class="sxs-lookup"><span data-stu-id="0a312-154">Scenarios for transient and permanent clones</span></span>
<span data-ttu-id="0a312-155">Hello následující oddíly popisují příklad situacích, ve kterých může být použit klony přechodný a trvalé.</span><span class="sxs-lookup"><span data-stu-id="0a312-155">hello following sections describe example situations in which transient and permanent clones can be used.</span></span>

### <a name="item-level-recovery-with-a-transient-clone"></a><span data-ttu-id="0a312-156">Obnovení na úrovni položek s přechodný klon</span><span class="sxs-lookup"><span data-stu-id="0a312-156">Item-level recovery with a transient clone</span></span>
<span data-ttu-id="0a312-157">Je nutné toorecover jeden rok starý soubor s Microsoft PowerPoint.</span><span class="sxs-lookup"><span data-stu-id="0a312-157">You need toorecover a one-year-old Microsoft PowerPoint presentation file.</span></span> <span data-ttu-id="0a312-158">Váš správce IT identifikuje hello konkrétní zálohování z toto časové období a pak filtry hello svazku.</span><span class="sxs-lookup"><span data-stu-id="0a312-158">Your IT administrator identifies hello specific backup from that time frame, and then filters hello volume.</span></span> <span data-ttu-id="0a312-159">Správce Hello pak provede klonování svazku hello, vyhledá hello soubor, který hledáte a poskytuje ji tooyou.</span><span class="sxs-lookup"><span data-stu-id="0a312-159">hello administrator then clones hello volume, locates hello file that you are looking for, and provides it tooyou.</span></span> <span data-ttu-id="0a312-160">V tomto scénáři se používá přechodný klonu.</span><span class="sxs-lookup"><span data-stu-id="0a312-160">In this scenario, a transient clone is used.</span></span> 

<span data-ttu-id="0a312-161">![Dostupné video](./media/storsimple-clone-volume/Video_icon.png)**Dostupné video**</span><span class="sxs-lookup"><span data-stu-id="0a312-161">![Video available](./media/storsimple-clone-volume/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="0a312-162">Klikněte na tlačítko toowatch video, které ukazuje, jak můžete použít klonu hello a obnovení funkcí v zařízení StorSimple toorecover odstranit soubory, [zde](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span><span class="sxs-lookup"><span data-stu-id="0a312-162">toowatch a video that demonstrates how you can use hello clone and restore features in StorSimple toorecover deleted files, click [here](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span></span>

### <a name="testing-in-hello-production-environment-with-a-permanent-clone"></a><span data-ttu-id="0a312-163">Testování hello produkčního prostředí s trvalé klon</span><span class="sxs-lookup"><span data-stu-id="0a312-163">Testing in hello production environment with a permanent clone</span></span>
<span data-ttu-id="0a312-164">Je nutné tooverify chyby testování v produkčním prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="0a312-164">You need tooverify a testing bug in hello production environment.</span></span> <span data-ttu-id="0a312-165">Při vytváření klon hello svazku v provozním prostředí hello pořízení snímku cloudu Tenhle klon.</span><span class="sxs-lookup"><span data-stu-id="0a312-165">You create a clone of hello volume in hello production environment by taking a cloud snapshot of this clone.</span></span> <span data-ttu-id="0a312-166">Hello klonovaný svazek je nyní nezávislé.</span><span class="sxs-lookup"><span data-stu-id="0a312-166">hello cloned volume is now independent.</span></span> <span data-ttu-id="0a312-167">V tomto scénáři se používá trvalá klonu.</span><span class="sxs-lookup"><span data-stu-id="0a312-167">In this scenario, a permanent clone is used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a312-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0a312-168">Next steps</span></span>
* <span data-ttu-id="0a312-169">Zjistěte, jak příliš[obnovit svazek StorSimple ze zálohovacího skladu](storsimple-restore-from-backup-set.md).</span><span class="sxs-lookup"><span data-stu-id="0a312-169">Learn how too[restore a StorSimple volume from a backup set](storsimple-restore-from-backup-set.md).</span></span>
* <span data-ttu-id="0a312-170">Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="0a312-170">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

