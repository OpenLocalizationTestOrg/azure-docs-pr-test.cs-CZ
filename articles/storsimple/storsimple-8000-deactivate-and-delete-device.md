---
title: "Deaktivace a odstranění zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje postup odstranění zařízení StorSimple ze služby tak, že nejprve deaktivace služby a poté ji odstranit."
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
ms.date: 05/23/2017
ms.author: alkohli
ms.openlocfilehash: 3c00867a29cf8343a57e74e2aabe3971ae6837af
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="deactivate-and-delete-a-storsimple-device"></a><span data-ttu-id="5200e-103">Deaktivace a odstranění zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="5200e-103">Deactivate and delete a StorSimple device</span></span>

## <a name="overview"></a><span data-ttu-id="5200e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="5200e-104">Overview</span></span>

<span data-ttu-id="5200e-105">Tento článek popisuje postup deaktivace a odstranění zařízení StorSimple, která je připojena k službě StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="5200e-105">This article describes how to deactivate and delete a StorSimple device that is connected to a StorSimple Device Manager service.</span></span> <span data-ttu-id="5200e-106">Pokyny v tomto článku se vztahuje pouze na řadu zařízení StorSimple 8000 včetně zařízení cloudu StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5200e-106">The guidance in this article applies only to StorSimple 8000 series devices including the StorSimple Cloud Appliances.</span></span> <span data-ttu-id="5200e-107">Pokud používáte o pole virtuální zařízení StorSimple, potom přejděte na [deaktivace a odstranění o pole virtuální zařízení StorSimple](storsimple-virtual-array-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="5200e-107">If you are using a StorSimple Virtual Array, then go to [Deactivate and delete a StorSimple Virtual Array](storsimple-virtual-array-deactivate-and-delete-device.md).</span></span>

<span data-ttu-id="5200e-108">Deaktivace přeruší připojení mezi zařízením a odpovídající služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="5200e-108">Deactivation severs the connection between the device and the corresponding StorSimple Device Manager service.</span></span> <span data-ttu-id="5200e-109">Můžete chtít provést zařízení StorSimple mimo provoz (např. Pokud jsou výměna nebo upgrade zařízení nebo pokud už používáte StorSimple).</span><span class="sxs-lookup"><span data-stu-id="5200e-109">You may wish to take a StorSimple device out of service (for example, if you are replacing or upgrading your device or if you are no longer using StorSimple).</span></span> <span data-ttu-id="5200e-110">Pokud ano, budete muset před odstraněním jej deaktivovat zařízení.</span><span class="sxs-lookup"><span data-stu-id="5200e-110">If so, you need to deactivate the device before you can delete it.</span></span>

<span data-ttu-id="5200e-111">Po deaktivaci zařízení žádná data, která byla uložená místně na zařízení již není dostupný.</span><span class="sxs-lookup"><span data-stu-id="5200e-111">When you deactivate a device, any data that was stored locally on the device is no longer accessible.</span></span> <span data-ttu-id="5200e-112">Lze obnovit pouze data, které jsou přidružené k zařízení, která byla uložená v cloudu.</span><span class="sxs-lookup"><span data-stu-id="5200e-112">Only the data associated with the device that was stored in the cloud can be recovered.</span></span>

> [!WARNING]
> <span data-ttu-id="5200e-113">Deaktivace je trvalé a nedá se vrátit zpátky.</span><span class="sxs-lookup"><span data-stu-id="5200e-113">Deactivation is a PERMANENT operation and cannot be undone.</span></span> <span data-ttu-id="5200e-114">Deaktivované zařízení nelze zaregistrovat u služby StorSimple Manager zařízení, pokud se resetují na výchozí tovární nastavení.</span><span class="sxs-lookup"><span data-stu-id="5200e-114">A deactivated device cannot be registered with the StorSimple Device Manager service unless it is reset to factory defaults.</span></span>
>
> <span data-ttu-id="5200e-115">Objekt factory resetovat proces odstraní všechna data, která byla uložená místně na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="5200e-115">The factory reset process deletes all the data that was stored locally on your device.</span></span> <span data-ttu-id="5200e-116">Proto je třeba provést cloudový snímek všechna vaše data před deaktivací zařízení.</span><span class="sxs-lookup"><span data-stu-id="5200e-116">Therefore, you must take a cloud snapshot of all your data before you deactivate a device.</span></span> <span data-ttu-id="5200e-117">Tento snímek cloudu umožňuje obnovit všechna data v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="5200e-117">This cloud snapshot allows you to recover all the data at a later stage.</span></span>

<span data-ttu-id="5200e-118">Po přečtení tohoto kurzu, budete moci:</span><span class="sxs-lookup"><span data-stu-id="5200e-118">After reading this tutorial, you will be able to:</span></span>

* <span data-ttu-id="5200e-119">Deaktivace zařízení a odstranit data.</span><span class="sxs-lookup"><span data-stu-id="5200e-119">Deactivate a device and delete the data.</span></span>
* <span data-ttu-id="5200e-120">Deaktivace zařízení a ukládat data.</span><span class="sxs-lookup"><span data-stu-id="5200e-120">Deactivate a device and retain the data.</span></span>

> [!NOTE]
> <span data-ttu-id="5200e-121">Před deaktivací fyzického zařízení StorSimple nebo cloudu zařízení, zastavte nebo odstraňte klienty a hostitele, které jsou závislé na tomto zařízení.</span><span class="sxs-lookup"><span data-stu-id="5200e-121">Before you deactivate a StorSimple physical device or cloud appliance, stop or delete clients and hosts that depend on that device.</span></span>


## <a name="deactivate-and-delete-data"></a><span data-ttu-id="5200e-122">Deaktivace a odstranění dat</span><span class="sxs-lookup"><span data-stu-id="5200e-122">Deactivate and delete data</span></span>

<span data-ttu-id="5200e-123">Pokud mají zájem o odstraňování zařízení zcela a nechcete ukládat data na zařízení, potom proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="5200e-123">If you are interested in deleting the device completely and do not want to retain the data on the device, then complete the following steps.</span></span>

#### <a name="to-deactivate-the-device-and-delete-the-data"></a><span data-ttu-id="5200e-124">Chcete-li deaktivovat zařízení a odstranění dat</span><span class="sxs-lookup"><span data-stu-id="5200e-124">To deactivate the device and delete the data</span></span>

1. <span data-ttu-id="5200e-125">Před deaktivací zařízení, je nutné odstranit všechny svazku kontejnery (a svazcích) přidružené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="5200e-125">Before you deactivate a device, you must delete all the volume containers (and the volumes) associated with the device.</span></span> <span data-ttu-id="5200e-126">Kontejnery svazků můžete odstranit až poté, co jste odstranili přidružených záloh.</span><span class="sxs-lookup"><span data-stu-id="5200e-126">You can delete volume containers only after you have deleted the associated backups.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5200e-127">Před deaktivací fyzického zařízení StorSimple nebo cloudu zařízení, ujistěte se, že data z kontejneru odstraněné svazku je ve skutečnosti odstranit ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="5200e-127">Before you deactivate a StorSimple physical device or cloud appliance, ensure that the data from the deleted volume container is actually deleted from the device.</span></span> <span data-ttu-id="5200e-128">Můžete monitorovat grafy využívání cloud a po zobrazení využití cloudu vyřadit z důvodu zálohování, které jste odstranili, potom můžete přejít k deaktivovat zařízení.</span><span class="sxs-lookup"><span data-stu-id="5200e-128">You can monitor the cloud consumption charts and when you see the cloud usage drop because of the backups you have deleted, then you can proceed to deactivate the device.</span></span> <span data-ttu-id="5200e-129">Pokud deaktivujete zařízení, než dojde k této rozevírací, data se Bezvýchodná situace v účtu úložiště a nabíhají poplatky.</span><span class="sxs-lookup"><span data-stu-id="5200e-129">If you deactivate the device before this drop occurs, the data is stranded in the storage account and accrues charges.</span></span>

2. <span data-ttu-id="5200e-130">Deaktivace zařízení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5200e-130">Deactivate the device as follows:</span></span>
   
   1. <span data-ttu-id="5200e-131">Přejděte do služby Správce zařízení StorSimple a klikněte na **Zařízení**.</span><span class="sxs-lookup"><span data-stu-id="5200e-131">Go to your StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="5200e-132">V **zařízení** okně vyberte zařízení, které chcete deaktivovat, klikněte pravým tlačítkem a pak klikněte na tlačítko **deaktivovat**.</span><span class="sxs-lookup"><span data-stu-id="5200e-132">In the **Devices** blade, select the device that you wish to deactivate, right-click, and then click **Deactivate**.</span></span>

        ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. <span data-ttu-id="5200e-134">V **deaktivovat** okno, zadejte název zařízení pro potvrzení a pak klikněte na tlačítko **deaktivovat**.</span><span class="sxs-lookup"><span data-stu-id="5200e-134">In the **Deactivate** blade, type the device name to confirm and then click **Deactivate**.</span></span> <span data-ttu-id="5200e-135">Proces deaktivovat spustí a trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="5200e-135">The deactivate process starts and takes a few minutes to complete.</span></span>

        ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)

3. <span data-ttu-id="5200e-137">Po deaktivaci můžete odstranit zařízení zcela.</span><span class="sxs-lookup"><span data-stu-id="5200e-137">After deactivation, you can delete the device completely.</span></span> <span data-ttu-id="5200e-138">Odstranění zařízení odebere ze seznamu zařízení připojená ke službě.</span><span class="sxs-lookup"><span data-stu-id="5200e-138">Deleting a device removes it from the list of devices connected to the service.</span></span> <span data-ttu-id="5200e-139">Služba poté již nebude možné spravovat zařízení.</span><span class="sxs-lookup"><span data-stu-id="5200e-139">The service can then no longer manage the deleted device.</span></span> <span data-ttu-id="5200e-140">Pomocí následujících kroků můžete odstranit zařízení:</span><span class="sxs-lookup"><span data-stu-id="5200e-140">Use the following steps to delete the device:</span></span>
   
   1. <span data-ttu-id="5200e-141">Přejděte do služby Správce zařízení StorSimple a klikněte na **Zařízení**.</span><span class="sxs-lookup"><span data-stu-id="5200e-141">Go to your StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="5200e-142">V **zařízení** okně vyberte deaktivované zařízení, který chcete odstranit, klikněte pravým tlačítkem a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="5200e-142">In the **Devices** blade, select the deactivated device that you wish to delete, right-click, and then click **Delete**.</span></span>

        ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. <span data-ttu-id="5200e-144">V **odstranit** okno, zadejte název zařízení pro potvrzení a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="5200e-144">In the **Delete** blade, type the device name to confirm and then click **Delete**.</span></span> <span data-ttu-id="5200e-145">Odstranění trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="5200e-145">The deletion takes a few minutes to complete.</span></span>

        ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. <span data-ttu-id="5200e-147">Po odstranění je úspěšně dokončete, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="5200e-147">After the deletion is successfully complete, you are notified.</span></span> <span data-ttu-id="5200e-148">V seznamu zařízení také aktualizuje tak, aby odrážela odstranění.</span><span class="sxs-lookup"><span data-stu-id="5200e-148">The device list also updates to reflect the deletion.</span></span>

## <a name="deactivate-and-retain-data"></a><span data-ttu-id="5200e-149">Deaktivovat a zachovat data</span><span class="sxs-lookup"><span data-stu-id="5200e-149">Deactivate and retain data</span></span>

<span data-ttu-id="5200e-150">Pokud jsou zájem o odstraňování zařízení, ale chcete zachovat data, potom proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5200e-150">If you are interested in deleting the device but want to retain the data, then complete the following steps:</span></span>

#### <a name="to-deactivate-a-device-and-retain-the-data"></a><span data-ttu-id="5200e-151">Chcete-li deaktivovat zařízení a ukládat data</span><span class="sxs-lookup"><span data-stu-id="5200e-151">To deactivate a device and retain the data</span></span>
1. <span data-ttu-id="5200e-152">Deaktivujte zařízení.</span><span class="sxs-lookup"><span data-stu-id="5200e-152">Deactivate the device.</span></span> <span data-ttu-id="5200e-153">Všechny kontejnery svazků a snímky zařízení zůstanou.</span><span class="sxs-lookup"><span data-stu-id="5200e-153">All the volume containers and the snapshots of the device remain.</span></span>
   
   1. <span data-ttu-id="5200e-154">Přejděte do služby Správce zařízení StorSimple a klikněte na **Zařízení**.</span><span class="sxs-lookup"><span data-stu-id="5200e-154">Go to your StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="5200e-155">V **zařízení** okně vyberte zařízení, které chcete deaktivovat, klikněte pravým tlačítkem a pak klikněte na tlačítko **deaktivovat**.</span><span class="sxs-lookup"><span data-stu-id="5200e-155">In the **Devices** blade, select the device that you wish to deactivate, right-click, and then click **Deactivate**.</span></span>

         ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. <span data-ttu-id="5200e-157">V **deaktivovat** okno, zadejte název zařízení pro potvrzení a pak klikněte na tlačítko **deaktivovat**.</span><span class="sxs-lookup"><span data-stu-id="5200e-157">In the **Deactivate** blade, type the device name to confirm and then click **Deactivate**.</span></span> <span data-ttu-id="5200e-158">Proces deaktivovat spustí a trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="5200e-158">The deactivate process starts and takes a few minutes to complete.</span></span>

         ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)
2. <span data-ttu-id="5200e-160">Nyní můžete převzít kontejnery svazků a přidružené snímky.</span><span class="sxs-lookup"><span data-stu-id="5200e-160">You can now fail over the volume containers and the associated snapshots.</span></span> <span data-ttu-id="5200e-161">Postupy, přejděte na [převzetí služeb při selhání a zotavení po havárii pro zařízení StorSimple](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="5200e-161">For procedures, go to [Failover and disaster recovery for your StorSimple device](storsimple-8000-device-failover-disaster-recovery.md).</span></span>
3. <span data-ttu-id="5200e-162">Po deaktivaci a převzetí služeb při selhání můžete odstranit zařízení zcela.</span><span class="sxs-lookup"><span data-stu-id="5200e-162">After deactivation and failover, you can delete the device completely.</span></span> <span data-ttu-id="5200e-163">Odstranění zařízení odebere ze seznamu zařízení připojená ke službě.</span><span class="sxs-lookup"><span data-stu-id="5200e-163">Deleting a device removes it from the list of devices connected to the service.</span></span> <span data-ttu-id="5200e-164">Služba poté již nebude možné spravovat zařízení.</span><span class="sxs-lookup"><span data-stu-id="5200e-164">The service can then no longer manage the deleted device.</span></span> <span data-ttu-id="5200e-165">Pokud chcete odstranit zařízení, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5200e-165">To delete the device, complete the following steps:</span></span>
   
   1. <span data-ttu-id="5200e-166">Přejděte do služby Správce zařízení StorSimple a klikněte na **Zařízení**.</span><span class="sxs-lookup"><span data-stu-id="5200e-166">Go to your StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="5200e-167">V **zařízení** okně vyberte deaktivované zařízení, který chcete odstranit, klikněte pravým tlačítkem a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="5200e-167">In the **Devices** blade, select the deactivated device that you wish to delete, right-click, and then click **Delete**.</span></span>

       ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. <span data-ttu-id="5200e-169">V **odstranit** okno, zadejte název zařízení pro potvrzení a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="5200e-169">In the **Delete** blade, type the device name to confirm and then click **Delete**.</span></span> <span data-ttu-id="5200e-170">Odstranění trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="5200e-170">The deletion takes a few minutes to complete.</span></span>

       ![Deaktivace zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. <span data-ttu-id="5200e-172">Po odstranění je úspěšně dokončete, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="5200e-172">After the deletion is successfully complete, you are notified.</span></span> <span data-ttu-id="5200e-173">V seznamu zařízení také aktualizuje tak, aby odrážela odstranění.</span><span class="sxs-lookup"><span data-stu-id="5200e-173">The device list also updates to reflect the deletion.</span></span>

     
## <a name="deactivate-and-delete-a-cloud-appliance"></a><span data-ttu-id="5200e-174">Deaktivace a odstranění zařízení cloudu</span><span class="sxs-lookup"><span data-stu-id="5200e-174">Deactivate and delete a cloud appliance</span></span>

<span data-ttu-id="5200e-175">Pro zařízení StorSimple cloudu deaktivace z portálu zruší přidělení a odstraní virtuální počítač a prostředky vytvořené při jeho zřizování.</span><span class="sxs-lookup"><span data-stu-id="5200e-175">For a StorSimple Cloud Appliance, deactivation from the portal deallocates and deletes the virtual machine, and the resources created when it was provisioned.</span></span> <span data-ttu-id="5200e-176">Po deaktivaci cloudového zařízení není možné ho obnovit do předchozího stavu.</span><span class="sxs-lookup"><span data-stu-id="5200e-176">After the cloud appliance is deactivated, it cannot be restored to its previous state.</span></span>

![Deaktivace zařízení StorSimple cloudu](./media/storsimple-8000-deactivate-and-delete-device/deactivate7.png)

<span data-ttu-id="5200e-178">Deaktivaci má za následek následující akce:</span><span class="sxs-lookup"><span data-stu-id="5200e-178">Deactivation results in the following actions:</span></span>

* <span data-ttu-id="5200e-179">Zařízení StorSimple cloudu se odebere ze služby.</span><span class="sxs-lookup"><span data-stu-id="5200e-179">The StorSimple Cloud Appliance is removed from the service.</span></span>
* <span data-ttu-id="5200e-180">Virtuální počítač pro zařízení StorSimple cloudu je odstranit.</span><span class="sxs-lookup"><span data-stu-id="5200e-180">The virtual machine for the StorSimple Cloud Appliance is deleted.</span></span>
* <span data-ttu-id="5200e-181">Disk operačního systému a datové disky, které jsou vytvořené pro zařízení StorSimple cloudu se odeberou.</span><span class="sxs-lookup"><span data-stu-id="5200e-181">The OS disk and data disks created for the StorSimple Cloud Appliance are removed.</span></span>
* <span data-ttu-id="5200e-182">Hostované služby a virtuální sítě, které byly vytvořeny při zřizování zůstanou zachovány.</span><span class="sxs-lookup"><span data-stu-id="5200e-182">The hosted service and Virtual Network that were created during provisioning are retained.</span></span> <span data-ttu-id="5200e-183">Pokud nepoužíváte tyto entity, odstraňte je ručně.</span><span class="sxs-lookup"><span data-stu-id="5200e-183">If you are not using these entities, you should delete them manually.</span></span>
* <span data-ttu-id="5200e-184">Cloudové snímky vytvořené cloudové zařízení StorSimple se zachovají.</span><span class="sxs-lookup"><span data-stu-id="5200e-184">Cloud snapshots created by the StorSimple Cloud Appliance are retained.</span></span>

<span data-ttu-id="5200e-185">Po deaktivaci cloudu zařízení ji můžete odstranit ze seznamu zařízení.</span><span class="sxs-lookup"><span data-stu-id="5200e-185">After the cloud appliance is deactivated, you can delete it from the list of devices.</span></span> <span data-ttu-id="5200e-186">Vyberte deaktivované zařízení, klikněte pravým tlačítkem a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="5200e-186">Select the deactivated device, right-click, and then click **Delete**.</span></span> <span data-ttu-id="5200e-187">StorSimple oznámení po odstranění zařízení a seznam zařízení aktualizací.</span><span class="sxs-lookup"><span data-stu-id="5200e-187">StorSimple notifies you once the device is deleted and the list of devices updates.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5200e-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5200e-188">Next steps</span></span>

* <span data-ttu-id="5200e-189">Chcete-li obnovit deaktivované zařízení výchozí tovární nastavení, přejděte na [zařízení resetovat výchozí tovární nastavení](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="5200e-189">To restore the deactivated device to factory defaults, go to [Reset the device to factory default settings](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>
* <span data-ttu-id="5200e-190">Pro technickou podporu [kontaktovat Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="5200e-190">For technical assistance, [contact Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span></span>
* <span data-ttu-id="5200e-191">Další informace o tom, jak používat službu StorSimple Manager zařízení, přejděte na [použít službu StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="5200e-191">To learn more about how to use the StorSimple Device Manager service, go to [Use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

