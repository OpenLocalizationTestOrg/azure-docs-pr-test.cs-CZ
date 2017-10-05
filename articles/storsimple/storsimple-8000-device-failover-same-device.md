---
title: "StorSimple převzetí služeb při selhání, zotavení po havárii pro řady 8000 zařízení | Microsoft Docs"
description: "Naučte se převzít zařízení StorSimple na stejné zařízení."
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
ms.date: 06/23/2017
ms.author: alkohli
ms.openlocfilehash: acc8929dc3476e9590e8e4d9526b38b7c0719570
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="fail-over-your-storsimple-physical-device-to-same-device"></a><span data-ttu-id="cc579-103">Selhání fyzického zařízení StorSimple ve stejném zařízení</span><span class="sxs-lookup"><span data-stu-id="cc579-103">Fail over your StorSimple physical device to same device</span></span>

## <a name="overview"></a><span data-ttu-id="cc579-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="cc579-104">Overview</span></span>

<span data-ttu-id="cc579-105">Tento kurz popisuje kroky potřebné k převzetí služeb při selhání fyzického zařízení StorSimple 8000 řady na sebe sama Pokud dojde k havárii.</span><span class="sxs-lookup"><span data-stu-id="cc579-105">This tutorial describes the steps required to fail over a StorSimple 8000 series physical device to itself if there is a disaster.</span></span> <span data-ttu-id="cc579-106">StorSimple používá funkci zařízení převzetí služeb při selhání k migraci dat ze zdrojového fyzického zařízení v datovém centru do jiného fyzického zařízení.</span><span class="sxs-lookup"><span data-stu-id="cc579-106">StorSimple uses the device failover feature to migrate data from a source physical device in the datacenter to another physical device.</span></span> <span data-ttu-id="cc579-107">Pokyny v tomto kurzu platí pro řady StorSimple 8000 fyzických zařízení spuštěná verze softwaru Update 3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="cc579-107">The guidance in this tutorial applies to StorSimple 8000 series physical devices running software versions Update 3 and later.</span></span>

<span data-ttu-id="cc579-108">Další informace o převzetí služeb při selhání zařízení a jak se používají k zotavení po havárii, přejděte na [převzetí služeb při selhání a zotavení po havárii pro řadu zařízení StorSimple 8000](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="cc579-108">To learn more about device failover and how it is used to recover from a disaster, go to [Failover and disaster recovery for StorSimple 8000 series devices](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

<span data-ttu-id="cc579-109">Selhání fyzického zařízení k jiné fyzické zařízení, přejděte na [převzetí služeb při selhání do stejného fyzického zařízení StorSimple](storsimple-8000-device-failover-physical-device.md).</span><span class="sxs-lookup"><span data-stu-id="cc579-109">To fail over a physical device to another physical device, go to [Fail over to the same StorSimple physical device](storsimple-8000-device-failover-physical-device.md).</span></span> <span data-ttu-id="cc579-110">Selhání fyzického zařízení StorSimple a o cloudu zařízení StorSimple, přejděte na [převzetí služeb při selhání o cloudu zařízení StorSimple](storsimple-8000-device-failover-cloud-appliance.md).</span><span class="sxs-lookup"><span data-stu-id="cc579-110">To fail over a StorSimple physical device to a StorSimple Cloud Appliance, go to [Fail over to a StorSimple Cloud Appliance](storsimple-8000-device-failover-cloud-appliance.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="cc579-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cc579-111">Prerequisites</span></span>

- <span data-ttu-id="cc579-112">Ujistěte se, že jste si přečetli důležité informace pro převzetí služeb při selhání zařízení.</span><span class="sxs-lookup"><span data-stu-id="cc579-112">Ensure that you have reviewed the considerations for device failover.</span></span> <span data-ttu-id="cc579-113">Další informace, přejděte na [častá rozhodnutí při převzetí služeb při selhání zařízení](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="cc579-113">For more information, go to [Common considerations for device failover](storsimple-8000-device-failover-disaster-recovery.md).</span></span>


## <a name="steps-to-fail-over-to-the-same-device"></a><span data-ttu-id="cc579-114">Postup převzetí služeb při selhání do stejného zařízení</span><span class="sxs-lookup"><span data-stu-id="cc579-114">Steps to fail over to the same device</span></span>

<span data-ttu-id="cc579-115">Pokud potřebujete převzetí služeb při selhání do stejného zařízení, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="cc579-115">Perform the following steps if you need to fail over to the same device.</span></span>

1. <span data-ttu-id="cc579-116">Udělat snímky cloudu všechny svazky v zařízení.</span><span class="sxs-lookup"><span data-stu-id="cc579-116">Take cloud snapshots of all the volumes in your device.</span></span> <span data-ttu-id="cc579-117">Další informace, přejděte na [služby pomocí Správce zařízení StorSimple vytvářet zálohy](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="cc579-117">For more information, go to [Use StorSimple Device Manager service to create backups](storsimple-8000-manage-backup-policies-u2.md).</span></span>
2. <span data-ttu-id="cc579-118">Obnovte v zařízení výchozí tovární nastavení.</span><span class="sxs-lookup"><span data-stu-id="cc579-118">Reset your device to factory defaults.</span></span> <span data-ttu-id="cc579-119">Podrobné pokyny v [jak resetovat výchozí tovární nastavení zařízení StorSimple](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="cc579-119">Follow the detailed instructions in [how to reset a StorSimple device to factory default settings](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>
3. <span data-ttu-id="cc579-120">Přejděte do služby StorSimple Manager zařízení a potom vyberte **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="cc579-120">Go to the StorSimple Device Manager service and then select **Devices**.</span></span> <span data-ttu-id="cc579-121">V **zařízení** okně staré zařízení by měl zobrazit jako **Offline**.</span><span class="sxs-lookup"><span data-stu-id="cc579-121">In the **Devices** blade, the old device should show as **Offline**.</span></span>

    ![Zdrojového zařízení do offline režimu](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev2.png)

4. <span data-ttu-id="cc579-123">Konfigurace zařízení a zaregistrujte ho znovu pomocí služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="cc579-123">Configure your device and register it again with your StorSimple Device Manager service.</span></span> <span data-ttu-id="cc579-124">Nově registrovaná zařízení by měl zobrazit jako **připravení nastavit**.</span><span class="sxs-lookup"><span data-stu-id="cc579-124">The newly registered device should show as **Ready to set up**.</span></span> <span data-ttu-id="cc579-125">Název zařízení pro nové zařízení je stejný jako původní zařízení ale připojí číslem, chcete-li znamenat, že se zařízení resetovat na výchozí objekt pro vytváření a znovu zaregistrovat.</span><span class="sxs-lookup"><span data-stu-id="cc579-125">The device name for the new device is the same as the old device but appended with a numeral to indicate that the device was reset to factory default and registered again.</span></span>

    ![Nově registrovaná zařízení jste připraveni k nastavení](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev3.png)
5. <span data-ttu-id="cc579-127">Pro nové zařízení dokončete instalaci zařízení.</span><span class="sxs-lookup"><span data-stu-id="cc579-127">For the new device, complete the device setup.</span></span> <span data-ttu-id="cc579-128">Další informace, přejděte na [krok 4: dokončení minimální instalace zařízení](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup).</span><span class="sxs-lookup"><span data-stu-id="cc579-128">For more information, go to [Step 4: Complete minimum device setup](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup).</span></span> <span data-ttu-id="cc579-129">Na **zařízení** okně Stav zařízení změní na **Online**.</span><span class="sxs-lookup"><span data-stu-id="cc579-129">On the **Devices** blade, the status of the device changes to **Online**.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="cc579-130">**Dokončení minimální požadavky na konfiguraci nejprve nebo vaše zotavení po Havárii může selhat.**</span><span class="sxs-lookup"><span data-stu-id="cc579-130">**Complete the minimum configuration first, or your DR may fail.**</span></span>

    ![Online nově registrovaná zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev7.png)

6. <span data-ttu-id="cc579-132">Vyberte původní zařízení (offline stavu) a na panelu příkazů klikněte na **převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="cc579-132">Select the old device (status offline) and from the command bar, click **Fail over**.</span></span> <span data-ttu-id="cc579-133">V **převzetí služeb při selhání** okně vyberte původní zařízení jako zdroj a zadat cílové zařízení jako nově registrovaná zařízení.</span><span class="sxs-lookup"><span data-stu-id="cc579-133">In the **Fail over** blade, select old device as the source and specify the target device as the newly registered device.</span></span>

    ![Souhrn převzetí služeb při selhání](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev11.png)

    <span data-ttu-id="cc579-135">Podrobné pokyny najdete v části [převzetí služeb při selhání do jiného fyzického zařízení](#fail-over-to-another-physical-device).</span><span class="sxs-lookup"><span data-stu-id="cc579-135">For detailed instructions, refer to [Fail over to another physical device](#fail-over-to-another-physical-device).</span></span>

7. <span data-ttu-id="cc579-136">Úloha obnovení zařízení se vytvoří, můžete monitorovat z **úlohy** okno.</span><span class="sxs-lookup"><span data-stu-id="cc579-136">A device restore job is created that you can monitor from the **Jobs** blade.</span></span>

8. <span data-ttu-id="cc579-137">Po úspěšném dokončení úlohy, přístup k nové zařízení a přejděte do **kontejnery svazků** okno.</span><span class="sxs-lookup"><span data-stu-id="cc579-137">After the job has successfully completed, access the new device and navigate to the **Volume containers** blade.</span></span> <span data-ttu-id="cc579-138">Ověřte, že všechny kontejnery svazků ze staré zařízení byly migrovány do nového zařízení.</span><span class="sxs-lookup"><span data-stu-id="cc579-138">Verify that all the volume containers from the old device have migrated to the new device.</span></span>

   ![Kontejnery svazků migrovat](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev13.png)

9. <span data-ttu-id="cc579-140">Po dokončení převzetí služeb při selhání můžete deaktivovat a odstranit stará zařízení z portálu.</span><span class="sxs-lookup"><span data-stu-id="cc579-140">After the failover is complete, you can deactivate and delete the old device from the portal.</span></span> <span data-ttu-id="cc579-141">Vyberte v původním zařízení (offline), klikněte pravým tlačítkem a pak vyberte **deaktivovat**.</span><span class="sxs-lookup"><span data-stu-id="cc579-141">Select the old device (offline), right-click, and then select **Deactivate**.</span></span> <span data-ttu-id="cc579-142">Po deaktivaci zařízení se aktualizuje stav zařízení.</span><span class="sxs-lookup"><span data-stu-id="cc579-142">After the device is deactivated, the status of the device is updated.</span></span>

     ![Deaktivovat zdrojového zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev14.png)

10. <span data-ttu-id="cc579-144">Vyberte deaktivované zařízení, klikněte pravým tlačítkem a pak vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="cc579-144">Select the deactivated device, right-click, and then select **Delete**.</span></span> <span data-ttu-id="cc579-145">Toto zařízení odstraní ze seznamu zařízení.</span><span class="sxs-lookup"><span data-stu-id="cc579-145">This deletes the device from the list of devices.</span></span>

    ![Odstranit zdrojového zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev15.png)



## <a name="next-steps"></a><span data-ttu-id="cc579-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cc579-147">Next steps</span></span>

* <span data-ttu-id="cc579-148">Po provedení převzetí služeb při selhání, budete muset [deaktivujte nebo odstraňte zařízení StorSimple](storsimple-8000-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="cc579-148">After you have performed a failover, you may need to [deactivate or delete your StorSimple device](storsimple-8000-deactivate-and-delete-device.md).</span></span>
* <span data-ttu-id="cc579-149">Informace o tom, jak používat službu StorSimple Manager zařízení, přejděte na [použít službu StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="cc579-149">For information about how to use the StorSimple Device Manager service, go to [Use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

