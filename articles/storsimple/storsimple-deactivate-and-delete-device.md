---
title: "Deaktivace a odstranění zařízení StorSimple | Microsoft Docs"
description: "Popisuje postup odstranění zařízení StorSimple ze služby tak, že nejprve deaktivace služby a poté ji odstranit."
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 155cda38-c5ae-45dc-b7e8-6444494afc9e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c000a642aa088ac80cc7077453b87e9a47f96900
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deactivate-and-delete-a-storsimple-8000-series-device-via-storsimple-manager-service"></a><span data-ttu-id="d45aa-103">Deaktivace a odstranění zařízení řady StorSimple 8000 prostřednictvím služby StorSimple Manager</span><span class="sxs-lookup"><span data-stu-id="d45aa-103">Deactivate and delete a StorSimple 8000 series device via StorSimple Manager service</span></span>
## <a name="overview"></a><span data-ttu-id="d45aa-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="d45aa-104">Overview</span></span>
<span data-ttu-id="d45aa-105">Můžete chtít provést zařízení StorSimple mimo provoz (např. Pokud jsou výměna nebo upgrade zařízení nebo pokud už používáte StorSimple).</span><span class="sxs-lookup"><span data-stu-id="d45aa-105">You may wish to take a StorSimple device out of service (for example, if you are replacing or upgrading your device or if you are no longer using StorSimple).</span></span> <span data-ttu-id="d45aa-106">Pokud je to tento případ, musíte před odstraněním jej deaktivovat zařízení.</span><span class="sxs-lookup"><span data-stu-id="d45aa-106">If this is the case, you will need to deactivate the device before you can delete it.</span></span> <span data-ttu-id="d45aa-107">Deaktivace přeruší připojení mezi zařízením a odpovídající služby StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="d45aa-107">Deactivating severs the connection between the device and the corresponding StorSimple Manager service.</span></span> <span data-ttu-id="d45aa-108">Tento kurz vysvětluje, jak odebrat zařízení StorSimple ze služby tak, že první deaktivace ji a poté ji odstranit.</span><span class="sxs-lookup"><span data-stu-id="d45aa-108">This tutorial explains how to remove a StorSimple device from service by first deactivating it and then deleting it.</span></span> 

<span data-ttu-id="d45aa-109">Po deaktivaci zařízení žádná data, která byla uložená místně na zařízení už nebude přístupný.</span><span class="sxs-lookup"><span data-stu-id="d45aa-109">When you deactivate a device, any data that was stored locally on the device will no longer be accessible.</span></span> <span data-ttu-id="d45aa-110">Lze obnovit pouze data, které jsou přidružené k zařízení, která byla uložená v cloudu.</span><span class="sxs-lookup"><span data-stu-id="d45aa-110">Only the data associated with the device that was stored in the cloud can be recovered.</span></span>  

> [!WARNING]
> <span data-ttu-id="d45aa-111">Deaktivace je trvalé a nedá se vrátit zpátky.</span><span class="sxs-lookup"><span data-stu-id="d45aa-111">Deactivation is a PERMANENT operation and cannot be undone.</span></span> <span data-ttu-id="d45aa-112">Deaktivované zařízení nemůže být zaregistrován u služby StorSimple Manager Pokud nejprve se resetují na výchozí tovární nastavení.</span><span class="sxs-lookup"><span data-stu-id="d45aa-112">A deactivated device cannot be registered with the StorSimple Manager service unless it is first reset to the default factory settings.</span></span> 
> 
> <span data-ttu-id="d45aa-113">Objekt factory resetovat proces odstraní všechna data, která byla uložená místně na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="d45aa-113">The factory reset process deletes all the data that was stored locally on your device.</span></span> <span data-ttu-id="d45aa-114">Proto je důležité provést cloudový snímek všechna vaše data, před deaktivací zařízení.</span><span class="sxs-lookup"><span data-stu-id="d45aa-114">Therefore, it is essential that you take a cloud snapshot of all your data before you deactivate a device.</span></span> <span data-ttu-id="d45aa-115">To vám umožní obnovit všechna data v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="d45aa-115">This will allow you to recover all the data at a later stage.</span></span>
> 
> 

<span data-ttu-id="d45aa-116">Tento kurz vysvětluje postup:</span><span class="sxs-lookup"><span data-stu-id="d45aa-116">This tutorial explains how to:</span></span>

* <span data-ttu-id="d45aa-117">Deaktivace zařízení a odstranění dat</span><span class="sxs-lookup"><span data-stu-id="d45aa-117">Deactivate a device and delete the data</span></span>
* <span data-ttu-id="d45aa-118">Deaktivace zařízení a ukládat data</span><span class="sxs-lookup"><span data-stu-id="d45aa-118">Deactivate a device and retain the data</span></span>

<span data-ttu-id="d45aa-119">Taky se dozvíte, jak deaktivaci a odstranění funguje na virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="d45aa-119">It also explains how deactivation and deletion works on a StorSimple virtual device.</span></span>

> [!NOTE]
> <span data-ttu-id="d45aa-120">Před deaktivací fyzický nebo virtuální zařízení StorSimple, zajistěte, aby pro zastavení nebo odstranit klienty a hostitele, které jsou závislé na tomto zařízení.</span><span class="sxs-lookup"><span data-stu-id="d45aa-120">Before you deactivate a StorSimple physical or virtual device, make sure to stop or delete clients and hosts that depend on that device.</span></span>
> 
> 

## <a name="deactivate-and-delete-data"></a><span data-ttu-id="d45aa-121">Deaktivace a odstranění dat</span><span class="sxs-lookup"><span data-stu-id="d45aa-121">Deactivate and delete data</span></span>
<span data-ttu-id="d45aa-122">Pokud mají zájem o odstraňování zařízení zcela a nechcete ukládat data na zařízení, potom proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="d45aa-122">If you are interested in deleting the device completely and do not want to retain the data on the device, then complete the following steps.</span></span>

#### <a name="to-deactivate-the-device-and-delete-the-data"></a><span data-ttu-id="d45aa-123">Chcete-li deaktivovat zařízení a odstranění dat</span><span class="sxs-lookup"><span data-stu-id="d45aa-123">To deactivate the device and delete the data</span></span>
1. <span data-ttu-id="d45aa-124">Před deaktivace zařízení, je nutné odstranit všechny svazku kontejnery (a svazcích) přidružené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="d45aa-124">Prior to deactivating a device, you must delete all the volume containers (and the volumes) associated with the device.</span></span> <span data-ttu-id="d45aa-125">Kontejnery svazků můžete odstranit až poté, co jste odstranili přidružených záloh.</span><span class="sxs-lookup"><span data-stu-id="d45aa-125">You can delete volume containers only after you have deleted the associated backups.</span></span>
2. <span data-ttu-id="d45aa-126">Deaktivace zařízení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d45aa-126">Deactivate the device as follows:</span></span>
   
   1. <span data-ttu-id="d45aa-127">Ve službě StorSimple Manager **zařízení** vyberte zařízení, které chcete deaktivovat a v dolní části stránky klikněte na tlačítko **deaktivovat**.</span><span class="sxs-lookup"><span data-stu-id="d45aa-127">On the StorSimple Manager service **Devices** page, select the device that you wish to deactivate and, at the bottom of the page, click **Deactivate**.</span></span>
   2. <span data-ttu-id="d45aa-128">Zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="d45aa-128">A confirmation message will appear.</span></span> <span data-ttu-id="d45aa-129">Klikněte na tlačítko **Ano** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="d45aa-129">Click **Yes** to continue.</span></span> <span data-ttu-id="d45aa-130">Proces deaktivovat spustí a trvat několik minut na dokončení.</span><span class="sxs-lookup"><span data-stu-id="d45aa-130">The deactivate process will start and take a few minutes to complete.</span></span>
3. <span data-ttu-id="d45aa-131">Po deaktivaci můžete odstranit zařízení zcela.</span><span class="sxs-lookup"><span data-stu-id="d45aa-131">After deactivation, you can delete the device completely.</span></span> <span data-ttu-id="d45aa-132">Odstranění zařízení odebere ze seznamu zařízení připojená ke službě.</span><span class="sxs-lookup"><span data-stu-id="d45aa-132">Deleting a device removes it from the list of devices connected to the service.</span></span> <span data-ttu-id="d45aa-133">Služba poté již nebude možné spravovat zařízení.</span><span class="sxs-lookup"><span data-stu-id="d45aa-133">The service can then no longer manage the deleted device.</span></span> <span data-ttu-id="d45aa-134">Pomocí následujících kroků můžete odstranit zařízení:</span><span class="sxs-lookup"><span data-stu-id="d45aa-134">Use the following steps to delete the device:</span></span>
   
   1. <span data-ttu-id="d45aa-135">Ve službě StorSimple Manager **zařízení** vyberte deaktivované zařízení, které chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="d45aa-135">On the StorSimple Manager service **Devices** page, select a deactivated device that you wish to delete.</span></span>
   2. <span data-ttu-id="d45aa-136">V dolní části na stránce, klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="d45aa-136">On the bottom on the page, click **Delete**.</span></span>
   3. <span data-ttu-id="d45aa-137">Zobrazí se výzva k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="d45aa-137">You will be prompted for confirmation.</span></span> <span data-ttu-id="d45aa-138">Klikněte na tlačítko **Ano** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="d45aa-138">Click **Yes** to continue.</span></span>
      
      <span data-ttu-id="d45aa-139">To může trvat několik minut, než se zařízení odstranit.</span><span class="sxs-lookup"><span data-stu-id="d45aa-139">It may take a few minutes for the device to be deleted.</span></span>

## <a name="deactivate-and-retain-data"></a><span data-ttu-id="d45aa-140">Deaktivovat a zachovat data</span><span class="sxs-lookup"><span data-stu-id="d45aa-140">Deactivate and retain data</span></span>
<span data-ttu-id="d45aa-141">Pokud jsou zájem o odstraňování zařízení, ale chcete zachovat data, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="d45aa-141">If you are interested in deleting the device but want to retain the data, then complete the following steps.</span></span>

#### <a name="to-deactivate-a-device-and-retain-the-data"></a><span data-ttu-id="d45aa-142">Chcete-li deaktivovat zařízení a ukládat data</span><span class="sxs-lookup"><span data-stu-id="d45aa-142">To deactivate a device and retain the data</span></span>
1. <span data-ttu-id="d45aa-143">Deaktivujte zařízení.</span><span class="sxs-lookup"><span data-stu-id="d45aa-143">Deactivate the device.</span></span> <span data-ttu-id="d45aa-144">Všechny kontejnery svazků a snímky zařízení zůstanou.</span><span class="sxs-lookup"><span data-stu-id="d45aa-144">All the volume containers and the snapshots of the device will remain.</span></span>
   
   1. <span data-ttu-id="d45aa-145">Ve službě StorSimple Manager **zařízení** vyberte zařízení, které chcete deaktivovat a v dolní části stránky klikněte na tlačítko **deaktivovat**.</span><span class="sxs-lookup"><span data-stu-id="d45aa-145">On the StorSimple Manager service **Devices** page, select the device that you wish to deactivate and, at the bottom of the page, click **Deactivate**.</span></span>
   2. <span data-ttu-id="d45aa-146">Zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="d45aa-146">A confirmation message will appear.</span></span> <span data-ttu-id="d45aa-147">Klikněte na tlačítko **Ano** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="d45aa-147">Click **Yes** to continue.</span></span> <span data-ttu-id="d45aa-148">Proces deaktivovat spustí a trvat několik minut na dokončení.</span><span class="sxs-lookup"><span data-stu-id="d45aa-148">The deactivate process will start and take a few minutes to complete.</span></span>
2. <span data-ttu-id="d45aa-149">Nyní můžete převzít kontejnery svazků a přidružené snímky.</span><span class="sxs-lookup"><span data-stu-id="d45aa-149">You can now fail over the volume containers and the associated snapshots.</span></span> <span data-ttu-id="d45aa-150">Postupy, přejděte na [převzetí služeb při selhání a zotavení po havárii pro zařízení StorSimple](storsimple-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="d45aa-150">For procedures, go to [Failover and disaster recovery for your StorSimple device](storsimple-device-failover-disaster-recovery.md).</span></span>
3. <span data-ttu-id="d45aa-151">Po deaktivaci a převzetí služeb při selhání můžete odstranit zařízení zcela.</span><span class="sxs-lookup"><span data-stu-id="d45aa-151">After deactivation and failover, you can delete the device completely.</span></span> <span data-ttu-id="d45aa-152">Odstranění zařízení odebere ze seznamu zařízení připojená ke službě.</span><span class="sxs-lookup"><span data-stu-id="d45aa-152">Deleting a device removes it from the list of devices connected to the service.</span></span> <span data-ttu-id="d45aa-153">Služba poté již nebude možné spravovat zařízení.</span><span class="sxs-lookup"><span data-stu-id="d45aa-153">The service can then no longer manage the deleted device.</span></span> <span data-ttu-id="d45aa-154">Proveďte následující kroky pro odstranění zařízení:</span><span class="sxs-lookup"><span data-stu-id="d45aa-154">Complete the following steps to delete the device:</span></span>
   
   1. <span data-ttu-id="d45aa-155">Ve službě StorSimple Manager **zařízení** vyberte deaktivované zařízení, které chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="d45aa-155">On the StorSimple Manager service **Devices** page, select a deactivated device that you wish to delete.</span></span>
   2. <span data-ttu-id="d45aa-156">V dolní části na stránce, klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="d45aa-156">On the bottom on the page, click **Delete**.</span></span>
   3. <span data-ttu-id="d45aa-157">Zobrazí se výzva k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="d45aa-157">You will be prompted for confirmation.</span></span> <span data-ttu-id="d45aa-158">Klikněte na tlačítko **Ano** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="d45aa-158">Click **Yes** to continue.</span></span>
      
      <span data-ttu-id="d45aa-159">To může trvat několik minut, než se zařízení odstranit.</span><span class="sxs-lookup"><span data-stu-id="d45aa-159">It may take a few minutes for the device to be deleted.</span></span>

## <a name="deactivate-and-delete-a-virtual-device"></a><span data-ttu-id="d45aa-160">Deaktivace a odstranění virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="d45aa-160">Deactivate and delete a virtual device</span></span>
<span data-ttu-id="d45aa-161">Pro virtuální zařízení StorSimple deaktivace zruší přidělení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d45aa-161">For a StorSimple virtual device, deactivation deallocates the virtual machine.</span></span> <span data-ttu-id="d45aa-162">Potom můžete odstranit virtuální počítač a prostředky vytvořené při jeho zřizování.</span><span class="sxs-lookup"><span data-stu-id="d45aa-162">You can then delete the virtual machine and the resources created when it was provisioned.</span></span> <span data-ttu-id="d45aa-163">Po deaktivaci virtuálního zařízení je nelze obnovit do předchozího stavu.</span><span class="sxs-lookup"><span data-stu-id="d45aa-163">After the virtual device is deactivated, it cannot be restored to its previous state.</span></span> 

<span data-ttu-id="d45aa-164">Deaktivaci má za následek následující akce:</span><span class="sxs-lookup"><span data-stu-id="d45aa-164">Deactivation results in the following actions:</span></span>

* <span data-ttu-id="d45aa-165">Virtuální zařízení StorSimple je odebráno.</span><span class="sxs-lookup"><span data-stu-id="d45aa-165">The StorSimple virtual device is removed.</span></span>
* <span data-ttu-id="d45aa-166">OSDisk a datové disky, které jsou vytvořené pro virtuální zařízení StorSimple se odeberou.</span><span class="sxs-lookup"><span data-stu-id="d45aa-166">The OSDisk and Data Disks created for the StorSimple virtual device are removed.</span></span>
* <span data-ttu-id="d45aa-167">Hostované služby a virtuální sítě, které byly vytvořeny při zřizování zůstanou zachovány.</span><span class="sxs-lookup"><span data-stu-id="d45aa-167">The Hosted Service and Virtual Network that were created during provisioning are retained.</span></span> <span data-ttu-id="d45aa-168">Pokud nepoužíváte tyto entity, odstraňte je ručně.</span><span class="sxs-lookup"><span data-stu-id="d45aa-168">If you are not using these entities, you should delete them manually.</span></span>
* <span data-ttu-id="d45aa-169">Cloudové snímky vytvořené virtuální zařízení StorSimple se zachovají.</span><span class="sxs-lookup"><span data-stu-id="d45aa-169">Cloud snapshots created by the StorSimple virtual device are retained.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d45aa-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d45aa-170">Next steps</span></span>
* <span data-ttu-id="d45aa-171">Chcete-li obnovit deaktivované zařízení výchozí tovární nastavení, přejděte na [zařízení resetovat výchozí tovární nastavení](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="d45aa-171">To restore the deactivated device to factory defaults, go to [Reset the device to factory default settings](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>
* <span data-ttu-id="d45aa-172">Pro technickou podporu [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="d45aa-172">For technical assistance, [contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>
* <span data-ttu-id="d45aa-173">Další informace o tom, jak používat službu StorSimple Manager, přejděte na [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="d45aa-173">To learn more about how to use the StorSimple Manager service, go to [Use the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span> 

