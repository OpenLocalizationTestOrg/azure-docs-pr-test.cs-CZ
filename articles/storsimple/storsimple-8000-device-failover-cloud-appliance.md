---
title: "StorSimple převzetí služeb při selhání, zotavení po havárii pro o cloudu zařízení StorSimple | Microsoft Docs"
description: "Zjistěte, jak při selhání fyzického zařízení řady StorSimple 8000 do cloudu zařízení."
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
ms.openlocfilehash: ec8bebf2854e84a37e84b45564e80fc20b63d8d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="fail-over-to-your-storsimple-cloud-appliance"></a><span data-ttu-id="61f47-103">Převzetí služeb při selhání pro vaše zařízení StorSimple cloudu</span><span class="sxs-lookup"><span data-stu-id="61f47-103">Fail over to your StorSimple Cloud Appliance</span></span>

## <a name="overview"></a><span data-ttu-id="61f47-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="61f47-104">Overview</span></span>

<span data-ttu-id="61f47-105">Tento kurz popisuje kroky potřebné k převzetí služeb při selhání fyzického zařízení řady StorSimple 8000 a o cloudu zařízení StorSimple Pokud dojde k havárii.</span><span class="sxs-lookup"><span data-stu-id="61f47-105">This tutorial describes the steps required to fail over a StorSimple 8000 series physical device to a StorSimple Cloud Appliance if there is a disaster.</span></span> <span data-ttu-id="61f47-106">StorSimple používá funkci zařízení převzetí služeb při selhání k migraci dat ze zdrojového fyzického zařízení v datovém centru cloudu zařízení běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="61f47-106">StorSimple uses the device failover feature to migrate data from a source physical device in the datacenter to a cloud appliance running in Azure.</span></span> <span data-ttu-id="61f47-107">Pokyny v tomto kurzu platí pro řadu fyzického zařízení StorSimple 8000 a cloudu zařízení se systémem verze softwaru Update 3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="61f47-107">The guidance in this tutorial applies to StorSimple 8000 series physical devices and cloud appliances running software versions Update 3 and later.</span></span>

<span data-ttu-id="61f47-108">Další informace o převzetí služeb při selhání zařízení a jak se používají k zotavení po havárii, přejděte na [převzetí služeb při selhání a zotavení po havárii pro řadu zařízení StorSimple 8000](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="61f47-108">To learn more about device failover and how it is used to recover from a disaster, go to [Failover and disaster recovery for StorSimple 8000 series devices](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

<span data-ttu-id="61f47-109">Selhání fyzického zařízení StorSimple na jiné fyzické zařízení, přejděte na [převzetí služeb při selhání fyzického zařízení StorSimple](storsimple-8000-device-failover-physical-device.md).</span><span class="sxs-lookup"><span data-stu-id="61f47-109">To fail over a StorSimple physical device to another physical device, go to [Fail over to a StorSimple physical device](storsimple-8000-device-failover-physical-device.md).</span></span> <span data-ttu-id="61f47-110">Pokud chcete převzít zařízení sám na sebe, přejděte na [převzetí služeb při selhání do stejného fyzického zařízení StorSimple](storsimple-8000-device-failover-same-device.md).</span><span class="sxs-lookup"><span data-stu-id="61f47-110">To fail over a device to itself, go to [Fail over to the same StorSimple physical device](storsimple-8000-device-failover-same-device.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="61f47-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="61f47-111">Prerequisites</span></span>

- <span data-ttu-id="61f47-112">Ujistěte se, že jste si přečetli důležité informace pro převzetí služeb při selhání zařízení.</span><span class="sxs-lookup"><span data-stu-id="61f47-112">Ensure that you have reviewed the considerations for device failover.</span></span> <span data-ttu-id="61f47-113">Další informace, přejděte na [častá rozhodnutí při převzetí služeb při selhání zařízení](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="61f47-113">For more information, go to [Common considerations for device failover](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

- <span data-ttu-id="61f47-114">Musíte mít cloudu zařízení StorSimple, vytvořený a nakonfigurovaný před spuštěním tohoto postupu.</span><span class="sxs-lookup"><span data-stu-id="61f47-114">You must have a StorSimple Cloud Appliance created and configured before running this procedure.</span></span> <span data-ttu-id="61f47-115">Pokud spuštění aktualizace 3 verze softwaru nebo novější, zvažte použití zařízení 8020 cloudu zotavení po Havárii.</span><span class="sxs-lookup"><span data-stu-id="61f47-115">If running   Update 3 software version or later, consider using an 8020 cloud appliance for the DR.</span></span> <span data-ttu-id="61f47-116">8020 model má 64 TB a používá úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="61f47-116">The 8020 model has 64 TB and uses Premium Storage.</span></span> <span data-ttu-id="61f47-117">Další informace, přejděte na [nasadit a spravovat zařízení s StorSimple cloudu](storsimple-8000-cloud-appliance-u2.md).</span><span class="sxs-lookup"><span data-stu-id="61f47-117">For more information, go to [Deploy and manage a StorSimple Cloud Appliance](storsimple-8000-cloud-appliance-u2.md).</span></span>

## <a name="steps-to-fail-over-to-a-cloud-appliance"></a><span data-ttu-id="61f47-118">Postup převzetí služeb při selhání do cloudu zařízení</span><span class="sxs-lookup"><span data-stu-id="61f47-118">Steps to fail over to a cloud appliance</span></span>

<span data-ttu-id="61f47-119">Proveďte následující kroky a v zařízení obnovit do cílového zařízení StorSimple cloudu.</span><span class="sxs-lookup"><span data-stu-id="61f47-119">Perform the following steps to restore the device to a target StorSimple Cloud Appliance.</span></span>

1.  <span data-ttu-id="61f47-120">Ověřte, že kontejner svazků, které chcete převzetí služeb při selhání je spojen cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="61f47-120">Verify that the volume container you want to fail over has associated cloud snapshots.</span></span> <span data-ttu-id="61f47-121">Další informace, přejděte na [služby pomocí Správce zařízení StorSimple vytvářet zálohy](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="61f47-121">For more information, go to [Use StorSimple Device Manager service to create backups](storsimple-8000-manage-backup-policies-u2.md).</span></span>
2. <span data-ttu-id="61f47-122">Přejděte do služby Správce zařízení StorSimple a klikněte na **Zařízení**.</span><span class="sxs-lookup"><span data-stu-id="61f47-122">Go to your StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="61f47-123">V **zařízení** okno, přejděte na seznam zařízení připojená k vaší službě.</span><span class="sxs-lookup"><span data-stu-id="61f47-123">In the **Devices** blade, go to the list of devices connected with your service.</span></span>
    <span data-ttu-id="61f47-124">![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)</span><span class="sxs-lookup"><span data-stu-id="61f47-124">![Select device](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)</span></span>
3. <span data-ttu-id="61f47-125">Vyberte a klikněte na vaše zdrojové zařízení.</span><span class="sxs-lookup"><span data-stu-id="61f47-125">Select and click your source device.</span></span> <span data-ttu-id="61f47-126">Zdrojové zařízení má kontejnery svazků, které chcete převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="61f47-126">The source device has the volume containers that you want to fail over.</span></span> <span data-ttu-id="61f47-127">Přejděte na **Nastavení > kontejnery svazků**.</span><span class="sxs-lookup"><span data-stu-id="61f47-127">Go to **Settings > Volume Containers**.</span></span>

    ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. <span data-ttu-id="61f47-129">Vyberte kontejner svazků, které byste chtěli převzetí služeb při selhání na jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="61f47-129">Select a volume container that you would like to fail over to another device.</span></span> <span data-ttu-id="61f47-130">Klikněte na tlačítko kontejneru svazků, které chcete zobrazit seznam svazků v tomto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="61f47-130">Click the volume container to display the list of volumes within this container.</span></span> <span data-ttu-id="61f47-131">Vyberte svazek, klikněte pravým tlačítkem a klikněte na **přepnout do režimu Offline** uvedení svazku do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="61f47-131">Select a volume, right-click, and click **Take Offline** to take the volume offline.</span></span>

    ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. <span data-ttu-id="61f47-133">Tento postup opakujte pro všechny svazky v kontejneru svazků.</span><span class="sxs-lookup"><span data-stu-id="61f47-133">Repeat this process for all the volumes in the volume container.</span></span>

     ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. <span data-ttu-id="61f47-135">Předchozí krok opakujte pro všechny kontejnery svazků, které byste chtěli převzetí služeb při selhání na jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="61f47-135">Repeat the previous step for all the volume containers you would like to fail over to another device.</span></span>

7. <span data-ttu-id="61f47-136">Přejděte zpět **zařízení** okno.</span><span class="sxs-lookup"><span data-stu-id="61f47-136">Go back to the **Devices** blade.</span></span> <span data-ttu-id="61f47-137">Na panelu příkazů klikněte na **převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="61f47-137">From the command bar, click **Fail over**.</span></span>

    ![Klikněte na tlačítko selhání přes](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. <span data-ttu-id="61f47-139">V **převzetí služeb při selhání** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="61f47-139">In the **Fail over** blade, perform the following steps:</span></span>
   
    1. <span data-ttu-id="61f47-140">Klikněte na tlačítko **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="61f47-140">Click **Source**.</span></span> <span data-ttu-id="61f47-141">Vyberte kontejnery svazků pro převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="61f47-141">Select the volume containers to fail over.</span></span> <span data-ttu-id="61f47-142">**Zobrazí se pouze kontejnery svazků přidruženého cloudu snímky a offline svazky.**</span><span class="sxs-lookup"><span data-stu-id="61f47-142">**Only the volume containers with associated cloud snapshots and offline volumes are displayed.**</span></span>
        <span data-ttu-id="61f47-143">![Vyberte zdroj](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)</span><span class="sxs-lookup"><span data-stu-id="61f47-143">![Select source](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)</span></span>
    2. <span data-ttu-id="61f47-144">Klikněte na tlačítko **cíl**.</span><span class="sxs-lookup"><span data-stu-id="61f47-144">Click **Target**.</span></span> <span data-ttu-id="61f47-145">Z rozevíracího seznamu k dispozici zařízení vyberte zařízení s cílový cloud.</span><span class="sxs-lookup"><span data-stu-id="61f47-145">Select a target cloud appliance from the dropdown list of available devices.</span></span> <span data-ttu-id="61f47-146">**V seznamu se zobrazí jenom zařízení, která mají dostatečnou kapacitu, aby dokázala pojmout kontejnery svazků zdroje.**</span><span class="sxs-lookup"><span data-stu-id="61f47-146">**Only the devices that have sufficient capacity to accommodate source volume containers are displayed in the list.**</span></span>

        ![Vyberte cíl](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. <span data-ttu-id="61f47-148">Zkontrolujte nastavení převzetí služeb při selhání v rámci **Souhrn** a zaškrtněte políčko označující, zda jsou svazky v kontejnerech vybraný svazek offline.</span><span class="sxs-lookup"><span data-stu-id="61f47-148">Review the failover settings under **Summary** and select the checkbox indicating that the volumes in selected volume containers are offline.</span></span> 

        ![Zkontrolujte nastavení převzetí služeb při selhání](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. <span data-ttu-id="61f47-150">Bude vytvořena úloha převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="61f47-150">A failover job is created.</span></span> <span data-ttu-id="61f47-151">Monitorování úlohy převzetí služeb při selhání, klikněte na úlohu oznámení.</span><span class="sxs-lookup"><span data-stu-id="61f47-151">To monitor the failover job, click the job notification.</span></span>

    ![Úloha monitoru převzetí služeb při selhání](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. <span data-ttu-id="61f47-153">Po dokončení převzetí služeb při selhání, přejděte zpět na **zařízení** okno.</span><span class="sxs-lookup"><span data-stu-id="61f47-153">After the failover is completed, go back to the **Devices** blade.</span></span>

    1. <span data-ttu-id="61f47-154">Vyberte zařízení, která byla použita jako cíl pro převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="61f47-154">Select the device that was used as the target for the failover.</span></span>

       ![Vyberte zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. <span data-ttu-id="61f47-156">Klikněte na tlačítko **kontejnery svazků**.</span><span class="sxs-lookup"><span data-stu-id="61f47-156">Click **Volume Containers**.</span></span> <span data-ttu-id="61f47-157">Všechny kontejnery svazků, společně s svazky z původního zařízení by měl být uvedený.</span><span class="sxs-lookup"><span data-stu-id="61f47-157">All the volume containers, along with the volumes from the old device, should be listed.</span></span>

       <span data-ttu-id="61f47-158">Pokud kontejneru svazků, které při selhání má místně vázaný svazky, jsou tyto svazky při selhání jako vrstvené svazky.</span><span class="sxs-lookup"><span data-stu-id="61f47-158">If the volume container that you failed over has locally pinned volumes, those volumes are failed over as tiered volumes.</span></span> <span data-ttu-id="61f47-159">O cloudu zařízení StorSimple nepodporuje místně vázaných svazků.</span><span class="sxs-lookup"><span data-stu-id="61f47-159">Locally pinned volumes are not supported on a StorSimple Cloud Appliance.</span></span>

       ![Kontejnery svazků cílové zobrazení](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a><span data-ttu-id="61f47-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61f47-161">Next steps</span></span>

* <span data-ttu-id="61f47-162">Po provedení převzetí služeb při selhání, budete muset [deaktivujte nebo odstraňte zařízení StorSimple](storsimple-8000-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="61f47-162">After you have performed a failover, you may need to [deactivate or delete your StorSimple device](storsimple-8000-deactivate-and-delete-device.md).</span></span>

* <span data-ttu-id="61f47-163">Informace o tom, jak používat službu StorSimple Manager zařízení, přejděte na [použít službu StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="61f47-163">For information about how to use the StorSimple Device Manager service, go to [Use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

