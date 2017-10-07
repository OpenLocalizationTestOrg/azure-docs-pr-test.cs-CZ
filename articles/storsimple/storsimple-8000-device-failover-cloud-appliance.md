---
title: "aaaStorSimple převzetí služeb při selhání, tooa obnovení po havárii cloudu zařízení StorSimple | Microsoft Docs"
description: "Zjistěte, jak toofail přes vaše tooa fyzického zařízení řady StorSimple 8000 cloudové zařízení."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: e8a0bca057024358e3a557fe85a42ddefea36cff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-tooyour-storsimple-cloud-appliance"></a><span data-ttu-id="86d7a-103">Selhání tooyour cloudu zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="86d7a-103">Fail over tooyour StorSimple Cloud Appliance</span></span>

## <a name="overview"></a><span data-ttu-id="86d7a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="86d7a-104">Overview</span></span>

<span data-ttu-id="86d7a-105">Tento kurz popisuje hello kroky požadované toofail přes tooa fyzického zařízení řady StorSimple 8000 cloudu zařízení StorSimple, pokud dojde k havárii.</span><span class="sxs-lookup"><span data-stu-id="86d7a-105">This tutorial describes hello steps required toofail over a StorSimple 8000 series physical device tooa StorSimple Cloud Appliance if there is a disaster.</span></span> <span data-ttu-id="86d7a-106">StorSimple použije hello zařízení převzetí služeb při selhání funkce toomigrate data z fyzického zařízení zdroj hello datacenter tooa cloudu zařízení běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="86d7a-106">StorSimple uses hello device failover feature toomigrate data from a source physical device in hello datacenter tooa cloud appliance running in Azure.</span></span> <span data-ttu-id="86d7a-107">Hello pokyny v tomto kurzu platí tooStorSimple 8000 řady fyzických zařízení a zařízení cloudu spuštěná verze softwaru Update 3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="86d7a-107">hello guidance in this tutorial applies tooStorSimple 8000 series physical devices and cloud appliances running software versions Update 3 and later.</span></span>

<span data-ttu-id="86d7a-108">toolearn informace o převzetí služeb při selhání zařízení a jak je použité toorecover po havárii, přejděte příliš[převzetí služeb při selhání a zotavení po havárii pro řadu zařízení StorSimple 8000](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="86d7a-108">toolearn more about device failover and how it is used toorecover from a disaster, go too[Failover and disaster recovery for StorSimple 8000 series devices](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

<span data-ttu-id="86d7a-109">toofail přes StorSimple fyzického zařízení tooanother fyzické zařízení, přejděte příliš[selhání fyzického zařízení StorSimple tooa](storsimple-8000-device-failover-physical-device.md).</span><span class="sxs-lookup"><span data-stu-id="86d7a-109">toofail over a StorSimple physical device tooanother physical device, go too[Fail over tooa StorSimple physical device](storsimple-8000-device-failover-physical-device.md).</span></span> <span data-ttu-id="86d7a-110">toofail přes tooitself zařízení, přejděte příliš[převzetí služeb při selhání toohello stejné fyzického zařízení StorSimple](storsimple-8000-device-failover-same-device.md).</span><span class="sxs-lookup"><span data-stu-id="86d7a-110">toofail over a device tooitself, go too[Fail over toohello same StorSimple physical device](storsimple-8000-device-failover-same-device.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86d7a-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="86d7a-111">Prerequisites</span></span>

- <span data-ttu-id="86d7a-112">Ujistěte se, že jste si přečetli hello aspekty pro převzetí služeb při selhání zařízení.</span><span class="sxs-lookup"><span data-stu-id="86d7a-112">Ensure that you have reviewed hello considerations for device failover.</span></span> <span data-ttu-id="86d7a-113">Další informace, přejděte příliš[častá rozhodnutí při převzetí služeb při selhání zařízení](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="86d7a-113">For more information, go too[Common considerations for device failover](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

- <span data-ttu-id="86d7a-114">Musíte mít cloudu zařízení StorSimple, vytvořený a nakonfigurovaný před spuštěním tohoto postupu.</span><span class="sxs-lookup"><span data-stu-id="86d7a-114">You must have a StorSimple Cloud Appliance created and configured before running this procedure.</span></span> <span data-ttu-id="86d7a-115">Pokud spuštěná aktualizace softwaru verze 3 nebo novější, zvažte použití zařízení 8020 cloudu pro hello zotavení po Havárii.</span><span class="sxs-lookup"><span data-stu-id="86d7a-115">If running   Update 3 software version or later, consider using an 8020 cloud appliance for hello DR.</span></span> <span data-ttu-id="86d7a-116">Hello 8020 model má 64 TB a používá úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="86d7a-116">hello 8020 model has 64 TB and uses Premium Storage.</span></span> <span data-ttu-id="86d7a-117">Další informace, přejděte příliš[nasadit a spravovat zařízení s StorSimple cloudu](storsimple-8000-cloud-appliance-u2.md).</span><span class="sxs-lookup"><span data-stu-id="86d7a-117">For more information, go too[Deploy and manage a StorSimple Cloud Appliance](storsimple-8000-cloud-appliance-u2.md).</span></span>

## <a name="steps-toofail-over-tooa-cloud-appliance"></a><span data-ttu-id="86d7a-118">Kroky toofail přes tooa cloudu zařízení</span><span class="sxs-lookup"><span data-stu-id="86d7a-118">Steps toofail over tooa cloud appliance</span></span>

<span data-ttu-id="86d7a-119">Proveďte následující kroky toorestore hello zařízení tooa cílový Cloud zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="86d7a-119">Perform hello following steps toorestore hello device tooa target StorSimple Cloud Appliance.</span></span>

1.  <span data-ttu-id="86d7a-120">Ověřte, že tento kontejner svazků hello, které chcete toofail přes přidruženy cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="86d7a-120">Verify that hello volume container you want toofail over has associated cloud snapshots.</span></span> <span data-ttu-id="86d7a-121">Další informace, přejděte příliš[zálohování toocreate služby pomocí Správce zařízení StorSimple](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="86d7a-121">For more information, go too[Use StorSimple Device Manager service toocreate backups](storsimple-8000-manage-backup-policies-u2.md).</span></span>
2. <span data-ttu-id="86d7a-122">Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="86d7a-122">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="86d7a-123">V hello **zařízení** okno, přejděte toohello seznam zařízení připojená k vaší službě.</span><span class="sxs-lookup"><span data-stu-id="86d7a-123">In hello **Devices** blade, go toohello list of devices connected with your service.</span></span>
    <span data-ttu-id="86d7a-124">![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)</span><span class="sxs-lookup"><span data-stu-id="86d7a-124">![Select device](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)</span></span>
3. <span data-ttu-id="86d7a-125">Vyberte a klikněte na vaše zdrojové zařízení.</span><span class="sxs-lookup"><span data-stu-id="86d7a-125">Select and click your source device.</span></span> <span data-ttu-id="86d7a-126">Hello zdrojové zařízení má hello kontejnery svazků, které chcete toofail přes.</span><span class="sxs-lookup"><span data-stu-id="86d7a-126">hello source device has hello volume containers that you want toofail over.</span></span> <span data-ttu-id="86d7a-127">Přejděte příliš**Nastavení > kontejnery svazků**.</span><span class="sxs-lookup"><span data-stu-id="86d7a-127">Go too**Settings > Volume Containers**.</span></span>

    ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. <span data-ttu-id="86d7a-129">Vyberte kontejner svazků, které chcete toofail přes tooanother zařízení.</span><span class="sxs-lookup"><span data-stu-id="86d7a-129">Select a volume container that you would like toofail over tooanother device.</span></span> <span data-ttu-id="86d7a-130">Klikněte na tlačítko hello svazku kontejneru toodisplay hello seznam svazků v tomto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="86d7a-130">Click hello volume container toodisplay hello list of volumes within this container.</span></span> <span data-ttu-id="86d7a-131">Vyberte svazek, klikněte pravým tlačítkem a klikněte na **přepnout do režimu Offline** tootake hello svazek do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="86d7a-131">Select a volume, right-click, and click **Take Offline** tootake hello volume offline.</span></span>

    ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. <span data-ttu-id="86d7a-133">Tento postup opakujte pro všechny svazky hello v kontejneru svazků hello.</span><span class="sxs-lookup"><span data-stu-id="86d7a-133">Repeat this process for all hello volumes in hello volume container.</span></span>

     ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. <span data-ttu-id="86d7a-135">Předchozí krok zopakujte hello pro všechny kontejnery svazků hello chcete toofail přes tooanother zařízení.</span><span class="sxs-lookup"><span data-stu-id="86d7a-135">Repeat hello previous step for all hello volume containers you would like toofail over tooanother device.</span></span>

7. <span data-ttu-id="86d7a-136">Přejděte zpět toohello **zařízení** okno.</span><span class="sxs-lookup"><span data-stu-id="86d7a-136">Go back toohello **Devices** blade.</span></span> <span data-ttu-id="86d7a-137">Na panelu příkazů hello, klikněte na **převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="86d7a-137">From hello command bar, click **Fail over**.</span></span>

    ![Klikněte na tlačítko selhání přes](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. <span data-ttu-id="86d7a-139">V hello **převzetí služeb při selhání** okně provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="86d7a-139">In hello **Fail over** blade, perform hello following steps:</span></span>
   
    1. <span data-ttu-id="86d7a-140">Klikněte na tlačítko **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="86d7a-140">Click **Source**.</span></span> <span data-ttu-id="86d7a-141">Vyberte hello svazku kontejnery toofail přes.</span><span class="sxs-lookup"><span data-stu-id="86d7a-141">Select hello volume containers toofail over.</span></span> <span data-ttu-id="86d7a-142">**Pouze hello kontejnery svazků s přidružený cloud snímky a offline svazky jsou zobrazeny.**</span><span class="sxs-lookup"><span data-stu-id="86d7a-142">**Only hello volume containers with associated cloud snapshots and offline volumes are displayed.**</span></span>
        <span data-ttu-id="86d7a-143">![Vyberte zdroj](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)</span><span class="sxs-lookup"><span data-stu-id="86d7a-143">![Select source](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)</span></span>
    2. <span data-ttu-id="86d7a-144">Klikněte na tlačítko **cíl**.</span><span class="sxs-lookup"><span data-stu-id="86d7a-144">Click **Target**.</span></span> <span data-ttu-id="86d7a-145">Z rozevíracího seznamu hello k dispozici zařízení vyberte zařízení s cílový cloud.</span><span class="sxs-lookup"><span data-stu-id="86d7a-145">Select a target cloud appliance from hello dropdown list of available devices.</span></span> <span data-ttu-id="86d7a-146">**Pouze hello zařízení, která mají dostatečná kapacita tooaccommodate zdrojového svazku kontejnery se zobrazí v seznamu hello.**</span><span class="sxs-lookup"><span data-stu-id="86d7a-146">**Only hello devices that have sufficient capacity tooaccommodate source volume containers are displayed in hello list.**</span></span>

        ![Vyberte cíl](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. <span data-ttu-id="86d7a-148">Zkontrolujte nastavení hello převzetí služeb při selhání v rámci **Souhrn** a vyberte hello políčko o tom, že hello svazky v kontejnerech vybraný svazek offline.</span><span class="sxs-lookup"><span data-stu-id="86d7a-148">Review hello failover settings under **Summary** and select hello checkbox indicating that hello volumes in selected volume containers are offline.</span></span> 

        ![Zkontrolujte nastavení převzetí služeb při selhání](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. <span data-ttu-id="86d7a-150">Bude vytvořena úloha převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="86d7a-150">A failover job is created.</span></span> <span data-ttu-id="86d7a-151">toomonitor hello převzetí služeb při selhání úlohy, klikněte na úlohu oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="86d7a-151">toomonitor hello failover job, click hello job notification.</span></span>

    ![Úloha monitoru převzetí služeb při selhání](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. <span data-ttu-id="86d7a-153">Po dokončení převzetí služeb při selhání hello vraťte toohello **zařízení** okno.</span><span class="sxs-lookup"><span data-stu-id="86d7a-153">After hello failover is completed, go back toohello **Devices** blade.</span></span>

    1. <span data-ttu-id="86d7a-154">Vyberte hello zařízení, která byla použita jako cíl hello hello převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="86d7a-154">Select hello device that was used as hello target for hello failover.</span></span>

       ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. <span data-ttu-id="86d7a-156">Klikněte na tlačítko **kontejnery svazků**.</span><span class="sxs-lookup"><span data-stu-id="86d7a-156">Click **Volume Containers**.</span></span> <span data-ttu-id="86d7a-157">Všechny kontejnery svazků hello, společně s hello svazky z původního zařízení hello by měl být uvedený.</span><span class="sxs-lookup"><span data-stu-id="86d7a-157">All hello volume containers, along with hello volumes from hello old device, should be listed.</span></span>

       <span data-ttu-id="86d7a-158">Pokud hello kontejneru svazků, které při selhání má místně vázaný svazky, jsou tyto svazky při selhání jako vrstvené svazky.</span><span class="sxs-lookup"><span data-stu-id="86d7a-158">If hello volume container that you failed over has locally pinned volumes, those volumes are failed over as tiered volumes.</span></span> <span data-ttu-id="86d7a-159">O cloudu zařízení StorSimple nepodporuje místně vázaných svazků.</span><span class="sxs-lookup"><span data-stu-id="86d7a-159">Locally pinned volumes are not supported on a StorSimple Cloud Appliance.</span></span>

       ![Kontejnery svazků cílové zobrazení](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a><span data-ttu-id="86d7a-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="86d7a-161">Next steps</span></span>

* <span data-ttu-id="86d7a-162">Po provedení převzetí služeb při selhání, můžete potřebovat příliš[deaktivujte nebo odstraňte zařízení StorSimple](storsimple-8000-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="86d7a-162">After you have performed a failover, you may need too[deactivate or delete your StorSimple device](storsimple-8000-deactivate-and-delete-device.md).</span></span>

* <span data-ttu-id="86d7a-163">Informace o tom, jak toouse hello Správce zařízení StorSimple služby, přejděte příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="86d7a-163">For information about how toouse hello StorSimple Device Manager service, go too[Use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

