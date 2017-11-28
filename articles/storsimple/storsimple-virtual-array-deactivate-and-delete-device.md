---
title: "aaaDeactivate a odstranit virtuální pole Microsoft Azure StorSimple | Microsoft Docs"
description: "Popisuje, jak zařízení StorSimple tooremove ze služby tak, že nejprve deaktivace služby a poté ji odstranit."
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
ms.openlocfilehash: b1f3ddb5822d19965739777e238af19b507df984
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a><span data-ttu-id="3a549-103">Deaktivovat a odstranit pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="3a549-103">Deactivate and delete a StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="3a549-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3a549-104">Overview</span></span>

<span data-ttu-id="3a549-105">Pokud deaktivujete o pole virtuální zařízení StorSimple, můžete zrušit hello připojení mezi hello zařízení a služby StorSimple Manager zařízení odpovídající hello.</span><span class="sxs-lookup"><span data-stu-id="3a549-105">When you deactivate a StorSimple Virtual Array, you break hello connection between hello device and hello corresponding StorSimple Device Manager service.</span></span> <span data-ttu-id="3a549-106">Tento kurz vysvětluje postup:</span><span class="sxs-lookup"><span data-stu-id="3a549-106">This tutorial explains how to:</span></span>

* <span data-ttu-id="3a549-107">Deaktivace zařízení</span><span class="sxs-lookup"><span data-stu-id="3a549-107">Deactivate a device</span></span> 
* <span data-ttu-id="3a549-108">Odstranit deaktivované zařízení</span><span class="sxs-lookup"><span data-stu-id="3a549-108">Delete a deactivated device</span></span>

<span data-ttu-id="3a549-109">Hello informace v tomto článku se vztahují pouze tooStorSimple virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="3a549-109">hello information in this article applies tooStorSimple Virtual Arrays only.</span></span> <span data-ttu-id="3a549-110">Informace o řady 8000 přejděte toohow příliš[deaktivujte nebo odstraňte zařízení](storsimple-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="3a549-110">For information on 8000 series, go toohow too[deactivate or delete a device](storsimple-deactivate-and-delete-device.md).</span></span>

## <a name="when-toodeactivate"></a><span data-ttu-id="3a549-111">Když toodeactivate?</span><span class="sxs-lookup"><span data-stu-id="3a549-111">When toodeactivate?</span></span>

<span data-ttu-id="3a549-112">Deaktivace je trvalé a nedá se vrátit zpátky.</span><span class="sxs-lookup"><span data-stu-id="3a549-112">Deactivation is a PERMANENT operation and cannot be undone.</span></span> <span data-ttu-id="3a549-113">Deaktivované zařízení s hello služby StorSimple Manager zařízení nelze zaregistrovat znovu.</span><span class="sxs-lookup"><span data-stu-id="3a549-113">You cannot register a deactivated device with hello StorSimple Device Manager service again.</span></span> <span data-ttu-id="3a549-114">Můžete třeba toodeactivate a odstranit o virtuální zařízení StorSimple pole v hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="3a549-114">You may need toodeactivate and delete a StorSimple Virtual Array in hello following scenarios:</span></span>

* <span data-ttu-id="3a549-115">**Plánované převzetí služeb při selhání** : zařízení je online a plánování toofail přes zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a549-115">**Planned failover** : Your device is online and you plan toofail over your device.</span></span> <span data-ttu-id="3a549-116">Pokud plánujete tooupgrade tooa větší zařízení, musíte toofail přes zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a549-116">If you are planning tooupgrade tooa larger device, you may need toofail over your device.</span></span> <span data-ttu-id="3a549-117">Po převodu vlastnictví hello dat a dokončení hello převzetí služeb při selhání, hello zdrojového zařízení se automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="3a549-117">After hello data ownership is transferred and hello failover is complete, hello source device is automatically deleted.</span></span>
* <span data-ttu-id="3a549-118">**Neplánované převzetí služeb při selhání** : zařízení je offline a potřebují toofail přes hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a549-118">**Unplanned failover** : Your device is offline and you need toofail over hello device.</span></span> <span data-ttu-id="3a549-119">Tento scénář může dojít při havárii, když je výpadek v datovém centru hello a primární zařízení je mimo provoz.</span><span class="sxs-lookup"><span data-stu-id="3a549-119">This scenario may occur during a disaster when there is an outage in hello datacenter and your primary device is down.</span></span> <span data-ttu-id="3a549-120">Máte v plánu toofail přes hello zařízení tooa sekundární zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a549-120">You plan toofail over hello device tooa secondary device.</span></span> <span data-ttu-id="3a549-121">Po převodu vlastnictví hello dat a dokončení hello převzetí služeb při selhání, hello zdrojového zařízení se automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="3a549-121">After hello data ownership is transferred and hello failover is complete, hello source device is automatically deleted.</span></span>
* <span data-ttu-id="3a549-122">**Vyřadit z provozu** : Chcete toodecommission hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a549-122">**Decommission** : You want toodecommission hello device.</span></span> <span data-ttu-id="3a549-123">To vyžaduje, abyste toofirst deaktivovat hello zařízení a potom jej odstraňte.</span><span class="sxs-lookup"><span data-stu-id="3a549-123">This requires you toofirst deactivate hello device and then delete it.</span></span> <span data-ttu-id="3a549-124">Po deaktivaci zařízení jste už mít přístup k všechna data, která je uložený místně.</span><span class="sxs-lookup"><span data-stu-id="3a549-124">When you deactivate a device, you can no longer access any data that is stored locally.</span></span> <span data-ttu-id="3a549-125">Můžete pouze přístup a obnovit data hello, které jsou uložená v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="3a549-125">You can only access and recover hello data stored in hello cloud.</span></span> <span data-ttu-id="3a549-126">Pokud máte v plánu tookeep hello zařízení dat po deaktivaci, byste měli před deaktivací zařízení vzít cloudový snímek všechna vaše data.</span><span class="sxs-lookup"><span data-stu-id="3a549-126">If you plan tookeep hello device data after deactivation, then you should take a cloud snapshot of all your data before you deactivate a device.</span></span> <span data-ttu-id="3a549-127">Tento snímek cloudu vám umožní toorecover všechny hello data v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="3a549-127">This cloud snapshot allows you toorecover all hello data at a later stage.</span></span>

## <a name="deactivate-a-device"></a><span data-ttu-id="3a549-128">Deaktivace zařízení</span><span class="sxs-lookup"><span data-stu-id="3a549-128">Deactivate a device</span></span>

<span data-ttu-id="3a549-129">toodeactivate zařízení, proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="3a549-129">toodeactivate your device, perform hello following steps.</span></span>

#### <a name="toodeactivate-hello-device"></a><span data-ttu-id="3a549-130">toodeactivate hello zařízení</span><span class="sxs-lookup"><span data-stu-id="3a549-130">toodeactivate hello device</span></span>

1. <span data-ttu-id="3a549-131">Ve službě, přejděte příliš**správy > zařízení**.</span><span class="sxs-lookup"><span data-stu-id="3a549-131">In your service, go too**Management > Devices**.</span></span> <span data-ttu-id="3a549-132">V hello **zařízení** okno, klikněte na tlačítko a vyberte hello zařízení chcete toodeactivate.</span><span class="sxs-lookup"><span data-stu-id="3a549-132">In hello **Devices** blade, click and select hello device that you wish toodeactivate.</span></span>
   
    ![Vyberte zařízení toodeactivate](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete7.png)
2. <span data-ttu-id="3a549-134">Ve vaší **řídicí panel zařízení** okně klikněte na tlačítko **... Další** a hello seznamu, vyberte **deaktivovat**.</span><span class="sxs-lookup"><span data-stu-id="3a549-134">In your **Device dashboard** blade, click **… More** and from hello list, select **Deactivate**.</span></span>
   
    ![Kliknutím na možnost deaktivovat](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete8.png)
3. <span data-ttu-id="3a549-136">V hello **deaktivovat** okno, název typu hello zařízení a pak klikněte na tlačítko **deaktivovat**.</span><span class="sxs-lookup"><span data-stu-id="3a549-136">In hello **Deactivate** blade, type hello device name and then click **Deactivate**.</span></span> 
   
    ![Potvrďte deaktivovat](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete1.png)
   
    <span data-ttu-id="3a549-138">Hello deaktivovat spuštění procesu a trvá několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="3a549-138">hello deactivate process starts and takes a few minutes toocomplete.</span></span>
   
    ![Deaktivace v průběhu](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete2.png)
4. <span data-ttu-id="3a549-140">Po deaktivaci aktualizuje hello seznam zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a549-140">After deactivation, hello list of devices refreshes.</span></span>
   
    ![Deaktivovat dokončení](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete3.png)
   
    <span data-ttu-id="3a549-142">Nyní můžete odstranit toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a549-142">You can now delete this device.</span></span>

## <a name="delete-hello-device"></a><span data-ttu-id="3a549-143">Odstranit hello zařízení</span><span class="sxs-lookup"><span data-stu-id="3a549-143">Delete hello device</span></span>

<span data-ttu-id="3a549-144">Zařízení má první deaktivované toodelete toobe ho.</span><span class="sxs-lookup"><span data-stu-id="3a549-144">A device has toobe first deactivated toodelete it.</span></span> <span data-ttu-id="3a549-145">Odstranění zařízení odebere ze seznamu hello zařízení připojených toohello služby.</span><span class="sxs-lookup"><span data-stu-id="3a549-145">Deleting a device removes it from hello list of devices connected toohello service.</span></span> <span data-ttu-id="3a549-146">Služba Hello pak již nebude možné spravovat zařízení hello odstranit.</span><span class="sxs-lookup"><span data-stu-id="3a549-146">hello service can then no longer manage hello deleted device.</span></span> <span data-ttu-id="3a549-147">Hello data přidružená k hello zařízení ale zůstává v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="3a549-147">hello data associated with hello device however remains in hello cloud.</span></span> <span data-ttu-id="3a549-148">Tato data pak nabíhají poplatky.</span><span class="sxs-lookup"><span data-stu-id="3a549-148">This data then accrues charges.</span></span>

<span data-ttu-id="3a549-149">toodelete hello zařízení, proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="3a549-149">toodelete hello device, perform hello following steps.</span></span>

#### <a name="toodelete-hello-device"></a><span data-ttu-id="3a549-150">toodelete hello zařízení</span><span class="sxs-lookup"><span data-stu-id="3a549-150">toodelete hello device</span></span>

1. <span data-ttu-id="3a549-151">Přejděte ve vašem Správci zařízení StorSimple příliš**správy > zařízení**.</span><span class="sxs-lookup"><span data-stu-id="3a549-151">In your StorSimple Device Manager, go too**Management > Devices**.</span></span> <span data-ttu-id="3a549-152">V hello **zařízení** okně vyberte deaktivované zařízení chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="3a549-152">In hello **Devices** blade, select a deactivated device that you wish toodelete.</span></span>
2. <span data-ttu-id="3a549-153">V hello **řídicí panel zařízení** okně klikněte na tlačítko **... Další** a pak klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a549-153">In hello **Device dashboard** blade, click **… More** and then click **Delete**.</span></span>
   
   ![Vyberte zařízení toodelete](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete4.png)
3. <span data-ttu-id="3a549-155">V hello **odstranit** okno, název typu hello odstranění hello tooconfirm zařízení a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="3a549-155">In hello **Delete** blade, type hello name of your device tooconfirm hello deletion and then click **Delete**.</span></span> <span data-ttu-id="3a549-156">Odstraňování hello zařízení neodstraní data v cloudu hello přidružené hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a549-156">Deleting hello device does not delete hello cloud data associated with hello device.</span></span> 
   
   ![Potvrzení odstranění](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete5.png) 
4. <span data-ttu-id="3a549-158">odstranění Hello spustí a trvá několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="3a549-158">hello deletion starts and takes a few minutes toocomplete.</span></span>
   
   ![Probíhá odstraňování](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete6.png)
   
    <span data-ttu-id="3a549-160">Po odstranění hello zařízení můžete zobrazit hello aktualizovat seznam zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a549-160">After hello device is deleted, you can view hello updated list of devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a549-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3a549-161">Next steps</span></span>

* <span data-ttu-id="3a549-162">Informace o tom toofail přes, přejděte příliš[převzetí služeb při selhání a zotavení po havárii vaše pole virtuální zařízení StorSimple](storsimple-virtual-array-failover-dr.md).</span><span class="sxs-lookup"><span data-stu-id="3a549-162">For information on how toofail over, go too[Failover and disaster recovery of your StorSimple Virtual Array](storsimple-virtual-array-failover-dr.md).</span></span>

* <span data-ttu-id="3a549-163">toolearn Další informace o tom, jak toouse hello služby StorSimple Manager zařízení, přejděte příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple virtuální pole](storsimple-virtual-array-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="3a549-163">toolearn more about how toouse hello StorSimple Device Manager service, go too[Use hello StorSimple Device Manager service tooadminister your StorSimple Virtual Array](storsimple-virtual-array-manager-service-administration.md).</span></span> 

