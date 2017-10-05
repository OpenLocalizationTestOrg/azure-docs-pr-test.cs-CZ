---
title: "Deaktivovat a odstranit virtuální pole Microsoft Azure StorSimple | Microsoft Docs"
description: "Popisuje postup odstranění zařízení StorSimple ze služby tak, že nejprve deaktivace služby a poté ji odstranit."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: a929f5bc-03e2-4b01-b925-973db236f19f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: 8dea36f92b034f8c6cdb6875634848d37f4c6606
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a><span data-ttu-id="6ed9c-103">Deaktivovat a odstranit pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="6ed9c-103">Deactivate and delete a StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="6ed9c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="6ed9c-104">Overview</span></span>

<span data-ttu-id="6ed9c-105">Pokud deaktivujete o pole virtuální zařízení StorSimple, zrušení propojení mezi zařízením a odpovídající služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-105">When you deactivate a StorSimple Virtual Array, you break the connection between the device and the corresponding StorSimple Device Manager service.</span></span> <span data-ttu-id="6ed9c-106">Tento kurz vysvětluje postup:</span><span class="sxs-lookup"><span data-stu-id="6ed9c-106">This tutorial explains how to:</span></span>

* <span data-ttu-id="6ed9c-107">Deaktivace zařízení</span><span class="sxs-lookup"><span data-stu-id="6ed9c-107">Deactivate a device</span></span> 
* <span data-ttu-id="6ed9c-108">Odstranit deaktivované zařízení</span><span class="sxs-lookup"><span data-stu-id="6ed9c-108">Delete a deactivated device</span></span>

<span data-ttu-id="6ed9c-109">Informace v tomto článku se vztahují pouze na pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-109">The information in this article applies to StorSimple Virtual Arrays only.</span></span> <span data-ttu-id="6ed9c-110">Informace o řady 8000, přejděte na postup [deaktivujte nebo odstraňte zařízení](storsimple-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="6ed9c-110">For information on 8000 series, go to how to [deactivate or delete a device](storsimple-deactivate-and-delete-device.md).</span></span>

## <a name="when-to-deactivate"></a><span data-ttu-id="6ed9c-111">Kdy deaktivovat?</span><span class="sxs-lookup"><span data-stu-id="6ed9c-111">When to deactivate?</span></span>

<span data-ttu-id="6ed9c-112">Deaktivace je trvalé a nedá se vrátit zpátky.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-112">Deactivation is a PERMANENT operation and cannot be undone.</span></span> <span data-ttu-id="6ed9c-113">Deaktivované zařízení pomocí služby StorSimple Manager zařízení nelze zaregistrovat znovu.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-113">You cannot register a deactivated device with the StorSimple Device Manager service again.</span></span> <span data-ttu-id="6ed9c-114">Může třeba deaktivovat a odstranit o virtuální zařízení StorSimple pole v následujících scénářích:</span><span class="sxs-lookup"><span data-stu-id="6ed9c-114">You may need to deactivate and delete a StorSimple Virtual Array in the following scenarios:</span></span>

* <span data-ttu-id="6ed9c-115">**Plánované převzetí služeb při selhání** : zařízení je online a chcete převzít zařízení.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-115">**Planned failover** : Your device is online and you plan to fail over your device.</span></span> <span data-ttu-id="6ed9c-116">Pokud máte v úmyslu upgradovat na větší zařízení, musíte při selhání zařízení.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-116">If you are planning to upgrade to a larger device, you may need to fail over your device.</span></span> <span data-ttu-id="6ed9c-117">Po převodu vlastnictví dat a dokončení převzetí služeb při selhání, zdrojového zařízení se automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-117">After the data ownership is transferred and the failover is complete, the source device is automatically deleted.</span></span>
* <span data-ttu-id="6ed9c-118">**Neplánované převzetí služeb při selhání** : zařízení je offline a potřebujete převzít zařízení.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-118">**Unplanned failover** : Your device is offline and you need to fail over the device.</span></span> <span data-ttu-id="6ed9c-119">Tento scénář může dojít při havárii, když je výpadek v datovém centru a primární zařízení je mimo provoz.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-119">This scenario may occur during a disaster when there is an outage in the datacenter and your primary device is down.</span></span> <span data-ttu-id="6ed9c-120">Chcete převzít zařízení sekundární zařízení.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-120">You plan to fail over the device to a secondary device.</span></span> <span data-ttu-id="6ed9c-121">Po převodu vlastnictví dat a dokončení převzetí služeb při selhání, zdrojového zařízení se automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-121">After the data ownership is transferred and the failover is complete, the source device is automatically deleted.</span></span>
* <span data-ttu-id="6ed9c-122">**Vyřadit z provozu** : chcete vyřadit z provozu zařízení.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-122">**Decommission** : You want to decommission the device.</span></span> <span data-ttu-id="6ed9c-123">To vyžaduje, abyste nejdřív deaktivovat zařízení a potom jej odstraňte.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-123">This requires you to first deactivate the device and then delete it.</span></span> <span data-ttu-id="6ed9c-124">Po deaktivaci zařízení jste už mít přístup k všechna data, která je uložený místně.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-124">When you deactivate a device, you can no longer access any data that is stored locally.</span></span> <span data-ttu-id="6ed9c-125">Je možné provádět pouze přístup a obnovit data uložená v cloudu.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-125">You can only access and recover the data stored in the cloud.</span></span> <span data-ttu-id="6ed9c-126">Pokud budete chtít zachovat data zařízení po deaktivaci, pak byste měli vzít cloudový snímek všechna vaše data před deaktivací zařízení.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-126">If you plan to keep the device data after deactivation, then you should take a cloud snapshot of all your data before you deactivate a device.</span></span> <span data-ttu-id="6ed9c-127">Tento snímek cloudu umožňuje obnovit všechna data v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-127">This cloud snapshot allows you to recover all the data at a later stage.</span></span>

## <a name="deactivate-a-device"></a><span data-ttu-id="6ed9c-128">Deaktivace zařízení</span><span class="sxs-lookup"><span data-stu-id="6ed9c-128">Deactivate a device</span></span>

<span data-ttu-id="6ed9c-129">Chcete-li deaktivovat zařízení, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-129">To deactivate your device, perform the following steps.</span></span>

#### <a name="to-deactivate-the-device"></a><span data-ttu-id="6ed9c-130">Chcete-li deaktivovat zařízení</span><span class="sxs-lookup"><span data-stu-id="6ed9c-130">To deactivate the device</span></span>

1. <span data-ttu-id="6ed9c-131">Ve službě, přejděte na **správy > zařízení**.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-131">In your service, go to **Management > Devices**.</span></span> <span data-ttu-id="6ed9c-132">V **zařízení** okně klikněte a vyberte zařízení, které chcete deaktivovat.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-132">In the **Devices** blade, click and select the device that you wish to deactivate.</span></span>
   
    ![Vyberte zařízení deaktivace](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete7.png)
2. <span data-ttu-id="6ed9c-134">Ve vaší **řídicí panel zařízení** okně klikněte na tlačítko **... Další** a ze seznamu vyberte **deaktivovat**.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-134">In your **Device dashboard** blade, click **… More** and from the list, select **Deactivate**.</span></span>
   
    ![Kliknutím na možnost deaktivovat](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete8.png)
3. <span data-ttu-id="6ed9c-136">V **deaktivovat** okno, zadejte název zařízení a pak klikněte na tlačítko **deaktivovat**.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-136">In the **Deactivate** blade, type the device name and then click **Deactivate**.</span></span> 
   
    ![Potvrďte deaktivovat](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete1.png)
   
    <span data-ttu-id="6ed9c-138">Proces deaktivovat spustí a trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-138">The deactivate process starts and takes a few minutes to complete.</span></span>
   
    ![Deaktivace v průběhu](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete2.png)
4. <span data-ttu-id="6ed9c-140">Po deaktivaci aktualizuje seznam zařízení.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-140">After deactivation, the list of devices refreshes.</span></span>
   
    ![Deaktivovat dokončení](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete3.png)
   
    <span data-ttu-id="6ed9c-142">Nyní můžete odstranit toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-142">You can now delete this device.</span></span>

## <a name="delete-the-device"></a><span data-ttu-id="6ed9c-143">Odstranění zařízení</span><span class="sxs-lookup"><span data-stu-id="6ed9c-143">Delete the device</span></span>

<span data-ttu-id="6ed9c-144">Zařízení musí nejprve deaktivovat ho odstranit.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-144">A device has to be first deactivated to delete it.</span></span> <span data-ttu-id="6ed9c-145">Odstranění zařízení odebere ze seznamu zařízení připojená ke službě.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-145">Deleting a device removes it from the list of devices connected to the service.</span></span> <span data-ttu-id="6ed9c-146">Služba poté již nebude možné spravovat zařízení.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-146">The service can then no longer manage the deleted device.</span></span> <span data-ttu-id="6ed9c-147">Data přidružená k zařízení, ale zůstává v cloudu.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-147">The data associated with the device however remains in the cloud.</span></span> <span data-ttu-id="6ed9c-148">Tato data pak nabíhají poplatky.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-148">This data then accrues charges.</span></span>

<span data-ttu-id="6ed9c-149">Pokud chcete odstranit zařízení, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-149">To delete the device, perform the following steps.</span></span>

#### <a name="to-delete-the-device"></a><span data-ttu-id="6ed9c-150">Odstranění zařízení</span><span class="sxs-lookup"><span data-stu-id="6ed9c-150">To delete the device</span></span>

1. <span data-ttu-id="6ed9c-151">Přejděte ve vašem Správci zařízení StorSimple na **správy > zařízení**.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-151">In your StorSimple Device Manager, go to **Management > Devices**.</span></span> <span data-ttu-id="6ed9c-152">V **zařízení** okně vyberte deaktivované zařízení, které chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-152">In the **Devices** blade, select a deactivated device that you wish to delete.</span></span>
2. <span data-ttu-id="6ed9c-153">V **řídicí panel zařízení** okně klikněte na tlačítko **... Další** a pak klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-153">In the **Device dashboard** blade, click **… More** and then click **Delete**.</span></span>
   
   ![Vyberte zařízení k odstranění](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete4.png)
3. <span data-ttu-id="6ed9c-155">V **odstranit** okno, zadejte název zařízení pro potvrzení odstranění a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-155">In the **Delete** blade, type the name of your device to confirm the deletion and then click **Delete**.</span></span> <span data-ttu-id="6ed9c-156">Odstraněním zařízení nedojde k odstranění dat cloud přidružené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-156">Deleting the device does not delete the cloud data associated with the device.</span></span> 
   
   ![Potvrzení odstranění](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete5.png) 
4. <span data-ttu-id="6ed9c-158">Odstranění se spustí a trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-158">The deletion starts and takes a few minutes to complete.</span></span>
   
   ![Probíhá odstraňování](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete6.png)
   
    <span data-ttu-id="6ed9c-160">Po odstranění zařízení můžete zobrazit aktualizovaný seznam zařízení.</span><span class="sxs-lookup"><span data-stu-id="6ed9c-160">After the device is deleted, you can view the updated list of devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ed9c-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6ed9c-161">Next steps</span></span>

* <span data-ttu-id="6ed9c-162">Informace o tom, jak převzetí služeb při selhání, přejděte na [převzetí služeb při selhání a zotavení po havárii vaše pole virtuální zařízení StorSimple](storsimple-virtual-array-failover-dr.md).</span><span class="sxs-lookup"><span data-stu-id="6ed9c-162">For information on how to fail over, go to [Failover and disaster recovery of your StorSimple Virtual Array](storsimple-virtual-array-failover-dr.md).</span></span>

* <span data-ttu-id="6ed9c-163">Další informace o tom, jak používat službu StorSimple Manager zařízení, přejděte na [použít službu StorSimple Manager zařízení ke správě pole virtuální zařízení StorSimple](storsimple-virtual-array-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="6ed9c-163">To learn more about how to use the StorSimple Device Manager service, go to [Use the StorSimple Device Manager service to administer your StorSimple Virtual Array](storsimple-virtual-array-manager-service-administration.md).</span></span> 

