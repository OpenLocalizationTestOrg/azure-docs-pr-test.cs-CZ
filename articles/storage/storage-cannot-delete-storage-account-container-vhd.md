---
title: "Řešení potíží s odstraňování účtů úložiště Azure, kontejnery nebo virtuální pevné disky v nasazení classic | Microsoft Docs"
description: "Řešení potíží s odstraňování účtů úložiště Azure, kontejnery nebo virtuální pevné disky v nasazení classic"
services: storage
documentationcenter: 
author: genlin
manager: felixwu
editor: tysonn
tags: storage
ms.assetid: 0f7a8243-d8dc-432a-9d37-1272a0cb3a5c
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 9f3e824414ad6c1a0aba98a3d549ee63ddc7272f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a><span data-ttu-id="6cfe5-103">Řešení potíží s odstraňování účtů úložiště Azure, kontejnery nebo virtuální pevné disky v nasazení classic</span><span class="sxs-lookup"><span data-stu-id="6cfe5-103">Troubleshoot deleting Azure storage accounts, containers, or VHDs in a classic deployment</span></span>
[!INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

<span data-ttu-id="6cfe5-104">Taky může docházet k chybám při pokusu o odstranění účtu úložiště Azure, kontejneru nebo virtuální pevný disk v [portál Azure](https://portal.azure.com/) nebo [portál Azure classic](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="6cfe5-104">You might receive errors when you try to delete the Azure storage account, container, or VHD in the [Azure portal](https://portal.azure.com/) or the [Azure classic portal](https://manage.windowsazure.com/).</span></span> <span data-ttu-id="6cfe5-105">Tyto problémy můžou být způsobené těmito okolnostmi:</span><span class="sxs-lookup"><span data-stu-id="6cfe5-105">The issues can be caused by the following circumstances:</span></span>

* <span data-ttu-id="6cfe5-106">Když odstraníte virtuální počítač, disk a virtuální pevný disk se neodstraní automaticky.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-106">When you delete a VM, the disk and VHD are not automatically deleted.</span></span> <span data-ttu-id="6cfe5-107">To může být důvod chyby při odstraňování účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-107">That might be the reason for failure on storage account deletion.</span></span> <span data-ttu-id="6cfe5-108">Neodstraňujte jsme disku tak, aby disk můžete připojit jiným virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-108">We don't delete the disk so that you can use the disk to mount another VM.</span></span>
* <span data-ttu-id="6cfe5-109">Disk nebo objekt blob, který je k tomuto disku přidružený, je stále ještě zapůjčený.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-109">There is still a lease on a disk or the blob that's associated with the disk.</span></span>
* <span data-ttu-id="6cfe5-110">Je stále image virtuálního počítače, který používá účet úložiště, kontejner nebo objekt blob.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-110">There is still a VM image that is using a blob, container, or storage account.</span></span>

<span data-ttu-id="6cfe5-111">Pokud není Azure problém řešený v tomto článku, navštivte fórech Azure na [MSDN a Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="6cfe5-111">If your Azure issue is not addressed in this article, visit the Azure forums on [MSDN and the Stack Overflow](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="6cfe5-112">Problém můžete účtovat na tyto fóra nebo na @AzureSupport na Twitteru.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-112">You can post your issue on these forums or to @AzureSupport on Twitter.</span></span> <span data-ttu-id="6cfe5-113">Navíc můžete soubor žádost o podporu Azure tak, že vyberete **získat podporu** na [podporu Azure](https://azure.microsoft.com/support/options/) lokality.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-113">Also, you can file an Azure support request by selecting **Get support** on the [Azure support](https://azure.microsoft.com/support/options/) site.</span></span>

## <a name="symptoms"></a><span data-ttu-id="6cfe5-114">Příznaky</span><span class="sxs-lookup"><span data-stu-id="6cfe5-114">Symptoms</span></span>
<span data-ttu-id="6cfe5-115">V následující části jsou uvedeny běžné chyby, které může dojít při pokusu o odstranění účtů úložiště Azure, kontejnery nebo virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-115">The following section lists common errors that you might receive when you try to delete the Azure storage accounts, containers, or VHDs.</span></span>

### <a name="scenario-1-unable-to-delete-a-storage-account"></a><span data-ttu-id="6cfe5-116">Scénář 1: Nelze odstranit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="6cfe5-116">Scenario 1: Unable to delete a storage account</span></span>
<span data-ttu-id="6cfe5-117">Když přejdete k účtu úložiště classic v [portál Azure](https://portal.azure.com/) a vyberte **odstranit**, může být zobrazí se seznam objektů, které brání odstranění účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="6cfe5-117">When you navigate to the classic storage account in the [Azure portal](https://portal.azure.com/) and select **Delete**, you may be presented with a list of objects that are preventing deletion of the storage account:</span></span>

  ![Obrázek chyby při odstranění účtu úložiště](./media/storage-cannot-delete-storage-account-container-vhd/newerror.png)

<span data-ttu-id="6cfe5-119">Když přejdete k účtu úložiště v [portál Azure classic](https://manage.windowsazure.com/) a vyberte **odstranit**, můžete sledovat některé z těchto chyb:</span><span class="sxs-lookup"><span data-stu-id="6cfe5-119">When you navigate to the storage account in the [Azure classic portal](https://manage.windowsazure.com/) and select **Delete**, you might see one of the following errors:</span></span>

- <span data-ttu-id="6cfe5-120">*Účet úložiště StorageAccountName obsahuje Image virtuálních počítačů. Zkontrolujte, zda že jsou tyto Image virtuálních počítačů odebrány před odstraněním tohoto účtu úložiště.*</span><span class="sxs-lookup"><span data-stu-id="6cfe5-120">*Storage account StorageAccountName contains VM Images. Ensure these VM Images are removed before deleting this storage account.*</span></span>

- <span data-ttu-id="6cfe5-121">*Nepodařilo se odstranit účet úložiště < vm-storage účet name >. Nelze odstranit účet úložiště < vm-storage účet name >: "účet úložiště < vm-storage účet name > má některé aktivní Image a/nebo disky. Tyto bitové kopie a/nebo disky odebrání před odstraněním tohoto účtu úložiště.'.*</span><span class="sxs-lookup"><span data-stu-id="6cfe5-121">*Failed to delete storage account <vm-storage-account-name>. Unable to delete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.*</span></span>

- <span data-ttu-id="6cfe5-122">*Účet úložiště < vm-storage účet name > obsahuje některé aktivní Image a/nebo disky, například xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Ujistěte se, tyto Image a/nebo disky jsou odebrány před odstraněním tohoto účtu úložiště.*</span><span class="sxs-lookup"><span data-stu-id="6cfe5-122">*Storage account <vm-storage-account-name> has some active image(s) and/or disk(s), e.g. xxxxxxxxx- xxxxxxxxx-O-209490240936090599. Ensure these image(s) and/or disk(s) are removed before deleting this storage account.*</span></span>

- <span data-ttu-id="6cfe5-123">*Účet úložiště < vm-storage účet name > má 1 kontejnery, které mít aktivní image a/nebo disk artefakty. Ujistěte se, odeberte tyto artefakty z úložiště imagí před odstraněním tohoto účtu úložiště*.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-123">*Storage account <vm-storage-account-name> has 1 container(s) which have an active image and/or disk artifacts. Ensure those artifacts are removed from the image repository before deleting this storage account*.</span></span>

- <span data-ttu-id="6cfe5-124">*Odešlete, že má účet úložiště se nezdařilo < vm-storage účet name > 1 kontejnery, které mít aktivní image a/nebo disk artefakty. Zajistěte, aby že tyto artefakty z úložiště imagí před odstraněním tohoto účtu úložiště. Při pokusu o odstranění účtu úložiště a je stále aktivní disky s ním spojená, zobrazí se zpráva s upozorněním, nejsou aktivní disky, které je potřeba odstranit*.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-124">*Submit Failed Storage account <vm-storage-account-name> has 1 container(s) which have an active image and/or disk artifacts. Ensure those artifacts are removed from the image repository before deleting this storage account. When you attempt to delete a storage account and there are still active disks associated with it, you will see a message telling you there are active disks that need to be deleted*.</span></span>

### <a name="scenario-2-unable-to-delete-a-container"></a><span data-ttu-id="6cfe5-125">Scénář 2: Nelze odstranit kontejner</span><span class="sxs-lookup"><span data-stu-id="6cfe5-125">Scenario 2: Unable to delete a container</span></span>
<span data-ttu-id="6cfe5-126">Když se pokusíte odstranit kontejner úložiště, může se zobrazit chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="6cfe5-126">When you try to delete the storage container, you might see the following error:</span></span>

<span data-ttu-id="6cfe5-127">*Nepodařilo se odstranit kontejner úložiště <container name>. Chyba: ' není aktuálně k zapůjčení adresy v kontejneru a žádné ID zapůjčení zadaná v žádosti o*.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-127">*Failed to delete storage container <container name>. Error: 'There is currently a lease on the container and no lease ID was specified in the request*.</span></span>

<span data-ttu-id="6cfe5-128">Nebo</span><span class="sxs-lookup"><span data-stu-id="6cfe5-128">Or</span></span>

<span data-ttu-id="6cfe5-129">*Následující disky virtuálního počítače používat objekty BLOB v tomto kontejneru, nelze ho odstranit kontejner: VirtualMachineDiskName1, VirtualMachineDiskName2,...*</span><span class="sxs-lookup"><span data-stu-id="6cfe5-129">*The following virtual machine disks use blobs in this container, so the container cannot be deleted: VirtualMachineDiskName1, VirtualMachineDiskName2, ...*</span></span>

### <a name="scenario-3-unable-to-delete-a-vhd"></a><span data-ttu-id="6cfe5-130">Scénář 3: Nelze odstranit virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="6cfe5-130">Scenario 3: Unable to delete a VHD</span></span>
<span data-ttu-id="6cfe5-131">Po odstranění virtuálního počítače a potom se pokuste odstranit objekty BLOB pro přidružené virtuální pevné disky, zobrazí se následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="6cfe5-131">After you delete a VM and then try to delete the blobs for the associated VHDs, you might receive the following message:</span></span>

<span data-ttu-id="6cfe5-132">*Nepodařilo se odstranit objekt blob "cesta/XXXXXX-XXXXXX-os-1447379084699.vhd'. Chyba: ' u objektu blob je momentálně zapůjčení a žádné ID zapůjčení zadaná v žádosti.*</span><span class="sxs-lookup"><span data-stu-id="6cfe5-132">*Failed to delete blob 'path/XXXXXX-XXXXXX-os-1447379084699.vhd'. Error: 'There is currently a lease on the blob and no lease ID was specified in the request.*</span></span>

<span data-ttu-id="6cfe5-133">Nebo</span><span class="sxs-lookup"><span data-stu-id="6cfe5-133">Or</span></span>

<span data-ttu-id="6cfe5-134">*Objekt BLOB BlobName.vhd je používán jako disk virtuálního počítače "VirtualMachineDiskName", nelze ho odstranit objekt blob.*</span><span class="sxs-lookup"><span data-stu-id="6cfe5-134">*Blob 'BlobName.vhd' is in use as virtual machine disk 'VirtualMachineDiskName', so the blob cannot be deleted.*</span></span>

## <a name="solution"></a><span data-ttu-id="6cfe5-135">Řešení</span><span class="sxs-lookup"><span data-stu-id="6cfe5-135">Solution</span></span>
<span data-ttu-id="6cfe5-136">Chcete-li vyřešit nejběžnější problémy, zkuste následující metodu:</span><span class="sxs-lookup"><span data-stu-id="6cfe5-136">To resolve the most common issues, try the following method:</span></span>

### <a name="step-1-delete-any-disks-that-are-preventing-deletion-of-the-storage-account-container-or-vhd"></a><span data-ttu-id="6cfe5-137">Krok 1: Odstranění disky, které brání odstranění účtu úložiště, kontejneru nebo virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="6cfe5-137">Step 1: Delete any disks that are preventing deletion of the storage account, container, or VHD</span></span>
1. <span data-ttu-id="6cfe5-138">Přepnout [portál Azure classic](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="6cfe5-138">Switch to the [Azure classic portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="6cfe5-139">Vyberte **VIRTUÁLNÍHO počítače** > **disky**.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-139">Select **VIRTUAL MACHINE** > **DISKS**.</span></span>

    ![Obrázek disky na virtuální počítače na portálu Azure classic.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)
3. <span data-ttu-id="6cfe5-141">Vyhledejte disky přidružené k virtuálnímu pevnému disku, kontejneru nebo účtu úložiště, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-141">Locate the disks that are associated with the storage account, container, or VHD that you want to delete.</span></span> <span data-ttu-id="6cfe5-142">Přidružený virtuální pevný disk, kontejner nebo účet úložiště najdete, když zkontrolujete umístění tohoto disku.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-142">When you check the location of the disk, you will find the associated storage account, container, or VHD.</span></span>

    ![Obrázek, který obsahuje informace o umístění pro disky na portálu Azure classic](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)
4. <span data-ttu-id="6cfe5-144">Odstraňte disky pomocí jedné z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="6cfe5-144">Delete the disks by using one of the following methods:</span></span>

  - <span data-ttu-id="6cfe5-145">Pokud neexistuje žádné virtuální uvedený na **připojené k** pole disku, můžete odstranit disk přímo.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-145">If  there is no VM listed on the **Attached To** field of the disk, you can delete the disk directly.</span></span>

  - <span data-ttu-id="6cfe5-146">Pokud je disk datový disk, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="6cfe5-146">If the disk is a data disk, follow these steps:</span></span>

    1. <span data-ttu-id="6cfe5-147">Zkontrolujte název virtuálního počítače, který je disk připojen k.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-147">Check the name of the VM that the disk is attached to.</span></span>
    2. <span data-ttu-id="6cfe5-148">Přejděte na **virtuální počítače** > **instance**a vyhledejte virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-148">Go to **Virtual Machines** > **Instances**, and then locate the VM.</span></span>
    3. <span data-ttu-id="6cfe5-149">Ujistěte se, že nic aktivně používá disk.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-149">Make sure that nothing is actively using the disk.</span></span>
    4. <span data-ttu-id="6cfe5-150">Vyberte **odpojit Disk** v dolní části portálu se odpojit disk.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-150">Select **Detach Disk** at the bottom of the portal to detach the disk.</span></span>
    5. <span data-ttu-id="6cfe5-151">Přejděte na **virtuální počítače** > **disky**a počkejte **připojené k** pole, které chcete zapnout prázdné.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-151">Go to **Virtual Machines** > **Disks**, and wait for the **Attached To** field to turn blank.</span></span> <span data-ttu-id="6cfe5-152">To znamená, že je disk se úspěšně odpojil od virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-152">This indicates the disk has successfully detached from the VM.</span></span>
    6. <span data-ttu-id="6cfe5-153">Vyberte **odstranit** v dolní části **virtuální počítače** > **disky** odstranit disk.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-153">Select **Delete** at the bottom of **Virtual Machines** > **Disks** to delete the disk.</span></span>

  - <span data-ttu-id="6cfe5-154">Pokud je disk disk s operačním systémem ( **obsahuje OS** pole má hodnotu, jako je Windows) a připojené k virtuálnímu počítači, postupujte podle těchto kroků se odstranit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-154">If the disk is an OS disk (the **Contains OS** field has a value like Windows) and attached to a VM, follow these steps to delete the VM.</span></span> <span data-ttu-id="6cfe5-155">Disk operačního systému nelze odpojit, takže jsme muset odstranit virtuální počítač k uvolnění zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-155">The OS disk cannot be detached, so we have to delete the VM to release the lease.</span></span>

    1. <span data-ttu-id="6cfe5-156">Zkontrolujte název virtuálního počítače je datový Disk připojen k.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-156">Check the name of the Virtual Machine the Data Disk is attached to.</span></span>  
    2. <span data-ttu-id="6cfe5-157">Přejděte na **virtuální počítače** > **instance**a potom vyberte virtuální počítač, který je disk připojen k.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-157">Go to **Virtual Machines** > **Instances**, and then select the VM that the disk is attached to.</span></span>
    3. <span data-ttu-id="6cfe5-158">Ujistěte se, že nic aktivně používá virtuální počítač a virtuální počítač už nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-158">Make sure that nothing is actively using the virtual machine, and that you no longer need the virtual machine.</span></span>
    4. <span data-ttu-id="6cfe5-159">Vyberte virtuální disk je připojen k, pak vyberte **odstranit** > **odstranění připojených disků**.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-159">Select the VM the disk is attached to, then select **Delete** > **Delete the attached disks**.</span></span>
    5. <span data-ttu-id="6cfe5-160">Přejděte na **virtuální počítače** > **disky**a počkejte, než se disk zmizí.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-160">Go to **Virtual Machines** > **Disks**, and wait for the disk to disappear.</span></span>  <span data-ttu-id="6cfe5-161">Může trvat několik minut, než tuto funkci používat, a budete muset aktualizujte stránku.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-161">It may take a few minutes for this to occur, and you may need to refresh the page.</span></span>
    6. <span data-ttu-id="6cfe5-162">Pokud disk nezmizí, počkejte **připojené k** pole, které chcete zapnout prázdné.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-162">If the disk does not disappear, wait for the **Attached To** field to turn blank.</span></span> <span data-ttu-id="6cfe5-163">To znamená, že na disku má plně odpojený od virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-163">This indicates the disk has fully detached from the VM.</span></span>  <span data-ttu-id="6cfe5-164">Pak vyberte disk a vyberte **odstranit** v dolní části stránky se odstranit disk.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-164">Then, select the disk, and select **Delete** at the bottom of the page to delete the disk.</span></span>


   > [!NOTE]
   > <span data-ttu-id="6cfe5-165">Pokud je disk připojen k virtuálnímu počítači, nebude možné jej odstranit.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-165">If a disk is attached to a VM, you will not be able to delete it.</span></span> <span data-ttu-id="6cfe5-166">Disky jsou asynchronně odpojit z odstraněné virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-166">Disks are detached from a deleted VM asynchronously.</span></span> <span data-ttu-id="6cfe5-167">Může trvat několik minut, po odstranění virtuálního počítače pro toto pole, aby vymazány.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-167">It might take a few minutes after the VM is deleted for this field to clear up.</span></span>
   >
   >


### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-the-storage-account-or-container"></a><span data-ttu-id="6cfe5-168">Krok 2: Odstraňte všechny Image virtuálních počítačů, které brání odstranění účtu úložiště nebo kontejneru</span><span class="sxs-lookup"><span data-stu-id="6cfe5-168">Step 2: Delete any VM Images that are preventing deletion of the storage account or container</span></span>
1. <span data-ttu-id="6cfe5-169">Přepnout [portál Azure classic](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="6cfe5-169">Switch to the [Azure classic portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="6cfe5-170">Vyberte **VIRTUÁLNÍHO počítače** > **bitové kopie**a potom odstraňte obrázky, které jsou přidruženy k účtu úložiště, kontejneru nebo virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-170">Select **VIRTUAL MACHINE** > **IMAGES**, and then delete the images that are associated with the storage account, container, or VHD.</span></span>

    <span data-ttu-id="6cfe5-171">Potom se pokuste odstranit účet úložiště, kontejneru nebo virtuální pevný disk znovu.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-171">After that, try to delete the storage account, container, or VHD again.</span></span>

> [!WARNING]
> <span data-ttu-id="6cfe5-172">Nezapomeňte si před odstraněním účtu zazálohovat všechno, co chcete uložit.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-172">Be sure to back up anything you want to save before you delete the account.</span></span> <span data-ttu-id="6cfe5-173">Jakmile odstraníte virtuální pevný disk, objektů blob, tabulka, fronta nebo soubor, je trvale odstraněn.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-173">Once you delete a VHD, blob, table, queue, or file, it is permanently deleted.</span></span> <span data-ttu-id="6cfe5-174">Ujistěte se, že prostředek není používán.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-174">Ensure that the resource is not in use.</span></span>
>
>

## <a name="about-the-stopped-deallocated-status"></a><span data-ttu-id="6cfe5-175">O zastaveném (nepřiřazeném) stavu</span><span class="sxs-lookup"><span data-stu-id="6cfe5-175">About the Stopped (deallocated) status</span></span>
<span data-ttu-id="6cfe5-176">Bude mít virtuální počítače, které byly vytvořené v modelu nasazení classic a které byly ponechány **zastaveném (nepřiřazeném)** stavu na buď [portál Azure](https://portal.azure.com/) nebo [portál Azure classic](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="6cfe5-176">VMs that were created in the classic deployment model and that have been retained will have the **Stopped (deallocated)** status on either the [Azure portal](https://portal.azure.com/) or [Azure classic portal](https://manage.windowsazure.com/).</span></span>

<span data-ttu-id="6cfe5-177">**Portál Azure classic**:</span><span class="sxs-lookup"><span data-stu-id="6cfe5-177">**Azure classic portal**:</span></span>

![Zastavit stav (Deallocated) pro virtuální počítače na portálu Azure.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)

<span data-ttu-id="6cfe5-179">**Portál Azure**:</span><span class="sxs-lookup"><span data-stu-id="6cfe5-179">**Azure portal**:</span></span>

![Zastavit (deallocated) stav pro virtuální počítače na portálu Azure classic.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

<span data-ttu-id="6cfe5-181">Stav "Zastaveném (nepřiřazeném)" uvolní prostředky počítače, jako je například procesoru, paměti a sítě.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-181">A "Stopped (deallocated)" status releases the computer resources, such as the CPU, memory, and network.</span></span> <span data-ttu-id="6cfe5-182">Disky, ale stále zachovány, takže můžete rychle znovu vytvořit virtuální počítač v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-182">The disks, however, are still retained so that you can quickly re-create the VM if necessary.</span></span> <span data-ttu-id="6cfe5-183">Tyto disky se vytvářejí na virtuálních pevných disků, které by podporovala úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-183">These disks are created on top of VHDs, which are backed by Azure storage.</span></span> <span data-ttu-id="6cfe5-184">Účet úložiště obsahuje tyto virtuální pevné disky a disky jsou zapůjčení na těchto virtuálních pevných discích.</span><span class="sxs-lookup"><span data-stu-id="6cfe5-184">The storage account has these VHDs, and the disks have leases on those VHDs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6cfe5-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6cfe5-185">Next steps</span></span>
* [<span data-ttu-id="6cfe5-186">Odstranit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="6cfe5-186">Delete a storage account</span></span>](storage-create-storage-account.md#delete-a-storage-account)
