---
title: "aaaStorSimple převzetí služeb při selhání, zotavení po havárii pro řady 8000 zařízení | Microsoft Docs"
description: "Zjistěte, jak toofail přes vaše toohello zařízení StorSimple ve stejném zařízení."
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
ms.openlocfilehash: b0b4216c7af6745ff68b85ca3d655691b43b4334
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-your-storsimple-physical-device-toosame-device"></a><span data-ttu-id="92e09-103">Selhání zařízení toosame fyzického zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="92e09-103">Fail over your StorSimple physical device toosame device</span></span>

## <a name="overview"></a><span data-ttu-id="92e09-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="92e09-104">Overview</span></span>

<span data-ttu-id="92e09-105">Tento kurz popisuje hello kroky požadované toofail přes tooitself fyzického zařízení StorSimple 8000 řad, pokud dojde k havárii.</span><span class="sxs-lookup"><span data-stu-id="92e09-105">This tutorial describes hello steps required toofail over a StorSimple 8000 series physical device tooitself if there is a disaster.</span></span> <span data-ttu-id="92e09-106">StorSimple použije hello zařízení převzetí služeb při selhání funkce toomigrate data z fyzického zařízení zdroj hello datacenter tooanother fyzického zařízení.</span><span class="sxs-lookup"><span data-stu-id="92e09-106">StorSimple uses hello device failover feature toomigrate data from a source physical device in hello datacenter tooanother physical device.</span></span> <span data-ttu-id="92e09-107">Hello pokyny v tomto kurzu platí tooStorSimple 8000 řady fyzických zařízení spuštěná verze softwaru Update 3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="92e09-107">hello guidance in this tutorial applies tooStorSimple 8000 series physical devices running software versions Update 3 and later.</span></span>

<span data-ttu-id="92e09-108">toolearn informace o převzetí služeb při selhání zařízení a jak je použité toorecover po havárii, přejděte příliš[převzetí služeb při selhání a zotavení po havárii pro řadu zařízení StorSimple 8000](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="92e09-108">toolearn more about device failover and how it is used toorecover from a disaster, go too[Failover and disaster recovery for StorSimple 8000 series devices](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

<span data-ttu-id="92e09-109">toofail přes tooanother fyzického zařízení fyzické zařízení, přejděte příliš[převzetí služeb při selhání toohello stejné fyzického zařízení StorSimple](storsimple-8000-device-failover-physical-device.md).</span><span class="sxs-lookup"><span data-stu-id="92e09-109">toofail over a physical device tooanother physical device, go too[Fail over toohello same StorSimple physical device](storsimple-8000-device-failover-physical-device.md).</span></span> <span data-ttu-id="92e09-110">toofail přes tooa fyzického zařízení StorSimple cloudu zařízení StorSimple, přejděte příliš[převzít tooa zařízení StorSimple cloudu](storsimple-8000-device-failover-cloud-appliance.md).</span><span class="sxs-lookup"><span data-stu-id="92e09-110">toofail over a StorSimple physical device tooa StorSimple Cloud Appliance, go too[Fail over tooa StorSimple Cloud Appliance](storsimple-8000-device-failover-cloud-appliance.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="92e09-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="92e09-111">Prerequisites</span></span>

- <span data-ttu-id="92e09-112">Ujistěte se, že jste si přečetli hello aspekty pro převzetí služeb při selhání zařízení.</span><span class="sxs-lookup"><span data-stu-id="92e09-112">Ensure that you have reviewed hello considerations for device failover.</span></span> <span data-ttu-id="92e09-113">Další informace, přejděte příliš[častá rozhodnutí při převzetí služeb při selhání zařízení](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="92e09-113">For more information, go too[Common considerations for device failover](storsimple-8000-device-failover-disaster-recovery.md).</span></span>


## <a name="steps-toofail-over-toohello-same-device"></a><span data-ttu-id="92e09-114">Kroky toofail přes toohello stejného zařízení</span><span class="sxs-lookup"><span data-stu-id="92e09-114">Steps toofail over toohello same device</span></span>

<span data-ttu-id="92e09-115">Proveďte následující kroky, pokud potřebujete toofail přes toohello hello stejné zařízení.</span><span class="sxs-lookup"><span data-stu-id="92e09-115">Perform hello following steps if you need toofail over toohello same device.</span></span>

1. <span data-ttu-id="92e09-116">Cloudové snímky všech svazků hello proveďte v zařízení.</span><span class="sxs-lookup"><span data-stu-id="92e09-116">Take cloud snapshots of all hello volumes in your device.</span></span> <span data-ttu-id="92e09-117">Další informace, přejděte příliš[zálohování toocreate služby pomocí Správce zařízení StorSimple](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="92e09-117">For more information, go too[Use StorSimple Device Manager service toocreate backups](storsimple-8000-manage-backup-policies-u2.md).</span></span>
2. <span data-ttu-id="92e09-118">Obnovte výchozí hodnoty pro toofactory vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="92e09-118">Reset your device toofactory defaults.</span></span> <span data-ttu-id="92e09-119">Postupujte podle hello podrobné pokyny v [jak tooreset toofactory zařízení StorSimple výchozí nastavení](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="92e09-119">Follow hello detailed instructions in [how tooreset a StorSimple device toofactory default settings](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>
3. <span data-ttu-id="92e09-120">Přejděte služby StorSimple Manager zařízení toohello a pak vyberte **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="92e09-120">Go toohello StorSimple Device Manager service and then select **Devices**.</span></span> <span data-ttu-id="92e09-121">V hello **zařízení** okně hello staré zařízení by měl zobrazit jako **Offline**.</span><span class="sxs-lookup"><span data-stu-id="92e09-121">In hello **Devices** blade, hello old device should show as **Offline**.</span></span>

    ![Zdrojového zařízení do offline režimu](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev2.png)

4. <span data-ttu-id="92e09-123">Konfigurace zařízení a zaregistrujte ho znovu pomocí služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="92e09-123">Configure your device and register it again with your StorSimple Device Manager service.</span></span> <span data-ttu-id="92e09-124">Hello nově registrovaná zařízení by měl zobrazit jako **připraveni tooset až**.</span><span class="sxs-lookup"><span data-stu-id="92e09-124">hello newly registered device should show as **Ready tooset up**.</span></span> <span data-ttu-id="92e09-125">název zařízení Hello hello nového zařízení je hello stejný jako původní zařízení hello ale s příponou číslice tooindicate hello zařízení byla výchozí toofactory resetování a registraci znovu.</span><span class="sxs-lookup"><span data-stu-id="92e09-125">hello device name for hello new device is hello same as hello old device but appended with a numeral tooindicate that hello device was reset toofactory default and registered again.</span></span>

    ![Nově registrovaná zařízení připraveno tooset nahoru](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev3.png)
5. <span data-ttu-id="92e09-127">Pro nové zařízení hello dokončení instalace zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="92e09-127">For hello new device, complete hello device setup.</span></span> <span data-ttu-id="92e09-128">Další informace, přejděte příliš[krok 4: dokončení minimální instalace zařízení](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup).</span><span class="sxs-lookup"><span data-stu-id="92e09-128">For more information, go too[Step 4: Complete minimum device setup](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup).</span></span> <span data-ttu-id="92e09-129">Na hello **zařízení** okně hello stav zařízení hello změní příliš**Online**.</span><span class="sxs-lookup"><span data-stu-id="92e09-129">On hello **Devices** blade, hello status of hello device changes too**Online**.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="92e09-130">**Dokončení minimální konfigurace hello nejprve nebo vaše zotavení po Havárii může selhat.**</span><span class="sxs-lookup"><span data-stu-id="92e09-130">**Complete hello minimum configuration first, or your DR may fail.**</span></span>

    ![Online nově registrovaná zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev7.png)

6. <span data-ttu-id="92e09-132">Vyberte zařízení staré hello (offline stavu) a na panelu příkazů hello, klikněte na **převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="92e09-132">Select hello old device (status offline) and from hello command bar, click **Fail over**.</span></span> <span data-ttu-id="92e09-133">V hello **převzetí služeb při selhání** okně vyberte původní zařízení jako zdroj hello a zadejte hello cílové zařízení hello nově registrovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="92e09-133">In hello **Fail over** blade, select old device as hello source and specify hello target device as hello newly registered device.</span></span>

    ![Souhrn převzetí služeb při selhání](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev11.png)

    <span data-ttu-id="92e09-135">Podrobné pokyny najdete v části příliš[selhání fyzického zařízení tooanother](#fail-over-to-another-physical-device).</span><span class="sxs-lookup"><span data-stu-id="92e09-135">For detailed instructions, refer too[Fail over tooanother physical device](#fail-over-to-another-physical-device).</span></span>

7. <span data-ttu-id="92e09-136">Úloha obnovení zařízení se vytvoří, můžete monitorovat z hello **úlohy** okno.</span><span class="sxs-lookup"><span data-stu-id="92e09-136">A device restore job is created that you can monitor from hello **Jobs** blade.</span></span>

8. <span data-ttu-id="92e09-137">Po úspěšném dokončení úlohy hello hello nové zařízení pro přístup k a přejděte toohello **kontejnery svazků** okno.</span><span class="sxs-lookup"><span data-stu-id="92e09-137">After hello job has successfully completed, access hello new device and navigate toohello **Volume containers** blade.</span></span> <span data-ttu-id="92e09-138">Ověřte, že všechny kontejnery svazků hello z původního zařízení hello byly migrovány toohello nové zařízení.</span><span class="sxs-lookup"><span data-stu-id="92e09-138">Verify that all hello volume containers from hello old device have migrated toohello new device.</span></span>

   ![Kontejnery svazků migrovat](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev13.png)

9. <span data-ttu-id="92e09-140">Po dokončení převzetí služeb při selhání hello můžete deaktivovat a odstranit stará zařízení hello z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="92e09-140">After hello failover is complete, you can deactivate and delete hello old device from hello portal.</span></span> <span data-ttu-id="92e09-141">Vyberte hello staré zařízení (offline), klikněte pravým tlačítkem a pak vyberte **deaktivovat**.</span><span class="sxs-lookup"><span data-stu-id="92e09-141">Select hello old device (offline), right-click, and then select **Deactivate**.</span></span> <span data-ttu-id="92e09-142">Po deaktivaci hello zařízení hello stav zařízení hello se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="92e09-142">After hello device is deactivated, hello status of hello device is updated.</span></span>

     ![Deaktivovat zdrojového zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev14.png)

10. <span data-ttu-id="92e09-144">Vyberte hello deaktivovat zařízení, klikněte pravým tlačítkem a pak vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="92e09-144">Select hello deactivated device, right-click, and then select **Delete**.</span></span> <span data-ttu-id="92e09-145">Toto zařízení hello odstraní z hello seznam zařízení.</span><span class="sxs-lookup"><span data-stu-id="92e09-145">This deletes hello device from hello list of devices.</span></span>

    ![Odstranit zdrojového zařízení](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev15.png)



## <a name="next-steps"></a><span data-ttu-id="92e09-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="92e09-147">Next steps</span></span>

* <span data-ttu-id="92e09-148">Po provedení převzetí služeb při selhání, můžete potřebovat příliš[deaktivujte nebo odstraňte zařízení StorSimple](storsimple-8000-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="92e09-148">After you have performed a failover, you may need too[deactivate or delete your StorSimple device](storsimple-8000-deactivate-and-delete-device.md).</span></span>
* <span data-ttu-id="92e09-149">Informace o tom, jak toouse hello Správce zařízení StorSimple služby, přejděte příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="92e09-149">For information about how toouse hello StorSimple Device Manager service, go too[Use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

